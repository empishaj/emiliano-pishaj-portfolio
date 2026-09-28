# EG-JAVA-003 — Pattern Matching und switch als explizite Fallunterscheidung

## Kurzfassung

Pattern Matching reduziert Casts und macht Fallunterscheidungen über Typen oder Record-Strukturen kompakter. Es ist besonders stark in Kombination mit geschlossenen Hierarchien.

Es ersetzt aber nicht automatisch Polymorphie. Wenn Verhalten natürlich beim Typ selbst liegt, ist ein großer `switch` häufig ein Hinweis auf falsch platzierte Verantwortung.

## Validierungsstatus

**VERIFIED** gegen Java SE 21 und JEP 441.

Primärquellen:

- Oracle Java SE 21 — Pattern Matching: https://docs.oracle.com/en/java/javase/21/language/pattern-matching.html
- Oracle — Pattern Matching for switch: https://docs.oracle.com/en/java/javase/21/language/pattern-matching-switch.html
- JEP 441: https://openjdk.org/jeps/441

## 1. Problem verstehen

Traditionelle Typprüfungen erzeugen häufig wiederholte `instanceof`- und Cast-Strukturen:

```java
if (result instanceof Success) {
    Success success = (Success) result;
    return success.value();
}
```

Pattern Matching verbindet Typprüfung und Bindung:

```java
if (result instanceof Success success) {
    return success.value();
}
```

Mit `switch` kann eine geschlossene Typmenge als klar lesbare Fallanalyse formuliert werden.

## 2. Gute Einsatzfälle

- Mapping an Systemgrenzen,
- Parser / AST,
- Ergebnis-/Fehlertypen,
- Visitor-artige Auswertung kleiner geschlossener Hierarchien,
- Serialisierungs-/Transformationslogik,
- Entscheidungen, die wirklich vom konkreten Variantentyp abhängen.

## 3. Polymorphie oder Pattern Matching?

### Pattern Matching ist stark, wenn

- das Verhalten **außerhalb** der Typen liegt,
- mehrere unabhängige Auswertungen derselben Varianten existieren,
- die Hierarchie bewusst geschlossen ist,
- der `switch` eine Transformation oder Projektion beschreibt.

### Polymorphie ist häufig stärker, wenn

- das Verhalten zum Objekt selbst gehört,
- neue Subtypen häufig hinzukommen sollen,
- viele Stellen denselben Typ-Switch wiederholen.

Beispiel für problematische Wiederholung:

```java
switch (shape) { ... calculateArea ... }
switch (shape) { ... calculatePerimeter ... }
switch (shape) { ... validate ... }
```

Wenn jede Operation in vielen Stellen nach Typ verzweigt, muss geprüft werden, ob Verhalten besser in den Typen kapselbar ist.

## 4. Beispiel mit sealed Hierarchie

```java
public sealed interface PaymentResult
    permits Accepted, Rejected, Pending {}

public record Accepted(String transactionId) implements PaymentResult {}
public record Rejected(String reason) implements PaymentResult {}
public record Pending(String reference) implements PaymentResult {}

static ProblemView toView(PaymentResult result) {
    return switch (result) {
        case Accepted a -> new ProblemView("accepted", a.transactionId());
        case Rejected r -> new ProblemView("rejected", r.reason());
        case Pending p -> new ProblemView("pending", p.reference());
    };
}
```

Der Vorteil liegt nicht nur in Kürze, sondern in der sichtbaren, geschlossenen Fallmenge.

## 5. Normative Guideline

### MUSS

- Pattern Matching wird eingesetzt, wenn die Fallunterscheidung semantisch klarer wird, nicht nur um Zeilen zu sparen.
- Große Typ-Switches werden auf wiederholte Verantwortungsverschiebung geprüft.
- Bei öffentlichen oder offenen Hierarchien muss mit zukünftigen Varianten gerechnet werden.

### SOLLTE

- Geschlossene fachliche Varianten werden mit sealed Types modelliert, wenn der Vertrag tatsächlich geschlossen ist.
- Mapping- und Adaptercode darf Pattern Matching bevorzugen, weil dort Transformation die natürliche Verantwortung ist.
- `default` wird nicht verwendet, um bei einer bewusst geschlossenen Hierarchie neue Varianten still zu verschlucken.

### DARF NICHT

- Pattern Matching darf kein Ersatz für fehlende Domänenmodellierung sein.
- Eine Fallunterscheidung darf keine unbekannten Varianten unbemerkt ignorieren, wenn diese fachlich relevant wären.

## 6. Failure Modes

### Mega-switch

Ein `switch` über 15–20 technische Klassen ist oft weniger ein Sprachproblem als ein Modellierungsproblem.

### `default` versteckt Evolution

```java
return switch (result) {
    case Accepted a -> ...;
    default -> fallback();
};
```

Bei einer geschlossenen fachlichen Menge kann das dazu führen, dass eine neu hinzugefügte Variante nicht bewusst behandelt wird.

### Pattern Matching an falscher Schicht

Controller oder Persistence-Code sollte nicht die zentrale fachliche Entscheidungslogik über Typ-Switches tragen.

## 7. Reviewfragen

1. Ist die Fallmenge offen oder geschlossen?
2. Ist der `switch` Transformation oder eigentlich Domänenverhalten?
3. Wiederholt sich dieselbe Fallunterscheidung an vielen Stellen?
4. Wird eine neue Variante vom Compiler sichtbar gemacht oder durch `default` verdeckt?
5. Wird Lesbarkeit tatsächlich besser?

## 8. Architektenperspektive

Die übertragbare Lektion lautet:

> **Explizite Varianten sind gut; verteilte Verantwortlichkeit ist schlecht.**

Pattern Matching ist dann architektonisch wertvoll, wenn es eine bereits saubere Modellgrenze klarer ausdrückt.

## 9. Review-Trigger

- Wechsel der Java-Baseline,
- Einführung neuer Pattern-Matching-Sprachfeatures,
- Wachstum einer Hierarchie,
- wiederholte Typ-Switches in mehreren Modulen.
