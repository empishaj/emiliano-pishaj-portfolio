# EG-JAVA-002 — Sealed Types für bewusst geschlossene Hierarchien

## Kurzfassung

Sealed Classes und Interfaces eignen sich, wenn eine Hierarchie **fachlich oder technisch bewusst geschlossen** sein soll. Sie machen erlaubte Erweiterungen explizit und verbessern dadurch Modellklarheit, Exhaustiveness und Reviewbarkeit.

Sie sind nicht geeignet, wenn Erweiterbarkeit durch externe Module, Plugins oder unbekannte Implementierungen ein bewusstes Ziel ist.

## Validierungsstatus

**VERIFIED** gegen Java SE 17+/21 und JEP 409.

Primärquellen:

- Oracle Java SE 17 — Sealed Classes: https://docs.oracle.com/en/java/javase/17/language/sealed-classes-and-interfaces.html
- JEP 409 — Sealed Classes: https://openjdk.org/jeps/409

## 1. Problem verstehen

Ein normales Java-Interface ist grundsätzlich offen für neue Implementierungen. Das ist oft sinnvoll. In manchen Domänen ist die Menge zulässiger Fälle aber absichtlich begrenzt.

Beispiel:

```java
public sealed interface PaymentResult
    permits PaymentAccepted, PaymentRejected, PaymentPending {
}

public record PaymentAccepted(String transactionId) implements PaymentResult {}
public record PaymentRejected(String reason) implements PaymentResult {}
public record PaymentPending(String reference) implements PaymentResult {}
```

Damit wird die Typmenge Teil des Modells.

## 2. Was Java garantiert

Eine sealed Class bzw. ein sealed Interface beschränkt direkte Subtypen auf eine explizit erlaubte Menge. Ein erlaubter Subtyp muss selbst `final`, `sealed` oder `non-sealed` sein.

Die Entscheidung `non-sealed` ist wichtig: Sie öffnet einen Ast der Hierarchie wieder bewusst.

## 3. Wann Sealed Types stark sind

- geschlossene fachliche Zustände,
- Ergebnis-/Fehlertypen,
- Commands oder Events mit begrenzter Variantenmenge,
- AST-/Parser-Strukturen,
- interne Protokollmodelle,
- Kombination mit Pattern Matching.

## 4. Wann nicht

### Plugin-/Extension-Architektur

Wenn Dritte neue Implementierungen hinzufügen sollen, widerspricht sealing dem Erweiterungsziel.

### künstliche Zukunftssicherheit

Eine Hierarchie nur deshalb zu versiegeln, weil heute drei Fälle bekannt sind, ist keine ausreichende Begründung. Die Domäne muss tatsächlich geschlossen sein oder das Team muss die Erweiterung bewusst kontrollieren wollen.

## 5. Coach-Perspektive

Die Architektenfrage lautet nicht:

> „Kann ich hier `sealed` verwenden?“

Sondern:

> **„Ist die Menge zulässiger Varianten Teil unseres fachlichen Vertrags?“**

Wenn ja, kann der Compiler helfen, diesen Vertrag sichtbar zu machen.

## 6. Normative Guideline

### MUSS

- Für jeden `sealed` Typ muss begründet sein, warum die Hierarchie geschlossen ist.
- `non-sealed` wird nur bewusst eingesetzt, wenn ein bestimmter Teilbaum erweiterbar bleiben soll.
- Änderungen der `permits`-Menge werden wie Vertragsänderungen reviewed.

### SOLLTE

- Geschlossene Hierarchien werden mit exhaustivem Pattern Matching kombiniert, wenn dies Lesbarkeit erhöht.
- Domänensprache soll in den Variantennamen sichtbar sein.

### DARF NICHT

- Sealing darf nicht als Ersatz für gutes Modul-/Package-Design verwendet werden.
- Eine öffentliche Erweiterungsschnittstelle darf nicht versehentlich geschlossen werden.

## 7. Beispiel: Exhaustiveness

```java
static String message(PaymentResult result) {
    return switch (result) {
        case PaymentAccepted accepted -> "Accepted: " + accepted.transactionId();
        case PaymentRejected rejected -> "Rejected: " + rejected.reason();
        case PaymentPending pending -> "Pending: " + pending.reference();
    };
}
```

Wird später ein neuer erlaubter Typ ergänzt, kann der Compiler fehlende Behandlungen sichtbar machen.

## 8. Failure Modes

- `non-sealed` wird aus Bequemlichkeit benutzt und zerstört die geschlossene Semantik.
- Die Hierarchie bildet technische Klassen statt fachlicher Varianten ab.
- Ein sealed Modell wird über Modul-/API-Grenzen exportiert, obwohl Consumer Erweiterbarkeit erwarten.
- Zu viele Variantentypen signalisieren eventuell ein anderes Modellierungsproblem.

## 9. Reviewfragen

1. Ist die Variantenmenge wirklich geschlossen?
2. Wer darf neue Varianten hinzufügen?
3. Ist die Hierarchie Teil eines öffentlichen Vertrags?
4. Welche Consumer müssen bei einer neuen Variante angepasst werden?
5. Ist Pattern Matching hier klarer als polymorphes Verhalten?

## 10. Architektenperspektive

Sealed Types sind ein Beispiel dafür, wie eine Sprachfunktion **Architekturwissen ausführbar macht**. Die erlaubte Erweiterbarkeit wird nicht nur dokumentiert, sondern vom Compiler kontrolliert.

## 11. Review-Trigger

- Änderung der Java-Baseline,
- Öffnung eines Moduls für externe Erweiterungen,
- Änderung der fachlichen Variantenmenge,
- Migration eines internen Modells in eine öffentliche API.
