---
id: AK-003
legacy_ids:
  - QG-JAVA-003
title: Pattern Matching und switch als explizite Fallunterscheidung
artifact_type: engineering-guideline
domain: java-language
status: active
maturity: reviewed
normative_level: recommended
last_validated: 2026-09-30
technology_baseline:
  java: "21+"
review_trigger:
  - Wechsel der Java-Baseline
  - Wachstum geschlossener Hierarchien
  - wiederholte Typ-Switches über mehrere Module
---

# AK-003 — Pattern Matching und switch

## Kurzfassung

Pattern Matching reduziert Casts und kann Fallunterscheidungen über Typen oder Record-Strukturen klarer ausdrücken. Besonders gut funktioniert es mit bewusst geschlossenen Hierarchien.

Es ersetzt nicht automatisch Polymorphie. Wiederholt sich derselbe Typ-Switch an vielen Stellen, liegt häufig ein Verantwortungsproblem im Modell vor.

## Geeignete Einsatzfälle

- Mapping an Systemgrenzen,
- Parser und ASTs,
- Ergebnis- und Fehlertypen,
- Auswertung geschlossener Varianten,
- Serialisierungs- und Transformationslogik.

## Pattern Matching oder Polymorphie?

Pattern Matching passt gut, wenn die Operation natürlicherweise **außerhalb** der Variantentypen liegt, etwa bei einer Projektion oder Transformation.

Polymorphie ist häufig besser, wenn:

- das Verhalten zum Objekt selbst gehört,
- dieselbe Fallunterscheidung an vielen Stellen wiederkehrt,
- neue Subtypen bewusst offen hinzukommen sollen.

## Beispiel

```java
public sealed interface PaymentResult
        permits Accepted, Rejected, Pending {}

public record Accepted(String transactionId) implements PaymentResult {}
public record Rejected(String reason) implements PaymentResult {}
public record Pending(String reference) implements PaymentResult {}

static String toMessage(PaymentResult result) {
    return switch (result) {
        case Accepted a -> "accepted: " + a.transactionId();
        case Rejected r -> "rejected: " + r.reason();
        case Pending p -> "pending: " + p.reference();
    };
}
```

Bei einer geschlossenen Hierarchie bleibt sichtbar, welche Varianten behandelt werden.

## Regeln

### Muss

- Pattern Matching wird nur eingesetzt, wenn die Fallunterscheidung dadurch semantisch klarer wird.
- Große oder wiederholte Typ-Switches werden auf falsch platzierte Verantwortung geprüft.

### Sollte

- Geschlossene fachliche Varianten werden mit `sealed` modelliert, wenn der Vertrag tatsächlich geschlossen ist.
- Mapping- und Adaptercode darf Pattern Matching bevorzugen, weil Transformation dort eine natürliche Verantwortung ist.
- Bei geschlossenen Hierarchien wird ein pauschaler `default` vermieden, wenn dadurch neue Varianten unbemerkt bleiben könnten.

### Darf nicht

- Pattern Matching darf fehlende Domänenmodellierung nicht kaschieren.
- Fachlich relevante unbekannte Varianten dürfen nicht still ignoriert werden.

## Reviewfragen

1. Ist die Fallmenge offen oder geschlossen?
2. Ist der `switch` eine Transformation oder eigentlich Domänenverhalten?
3. Wiederholt sich dieselbe Fallunterscheidung an mehreren Stellen?
4. Werden neue Varianten sichtbar oder durch einen generischen `default` verdeckt?
5. Wird der Code tatsächlich verständlicher?

## Merksatz

> Explizite Varianten sind nützlich. Verteilte Verantwortung ist es nicht. Pattern Matching ist dann stark, wenn es eine bereits sinnvolle Modellgrenze klarer ausdrückt.

## Quellen

- Oracle Java SE 21 — Pattern Matching: https://docs.oracle.com/en/java/javase/21/language/pattern-matching.html
- Oracle Java SE 21 — Pattern Matching for `switch`: https://docs.oracle.com/en/java/javase/21/language/pattern-matching-switch.html
- JEP 441: https://openjdk.org/jeps/441
