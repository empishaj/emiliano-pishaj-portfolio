---
id: AK-002
legacy_ids:
  - QG-JAVA-002
title: Sealed Types für bewusst geschlossene Hierarchien
artifact_type: engineering-guideline
domain: java-language
status: active
maturity: reviewed
normative_level: recommended
last_validated: 2026-09-30
technology_baseline:
  java: "17+"
review_trigger:
  - Wechsel der Java-Baseline
  - Öffnung eines Moduls für externe Erweiterungen
  - Änderung einer fachlich geschlossenen Variantenmenge
---

# AK-002 — Sealed Types für bewusst geschlossene Hierarchien

## Kurzfassung

Sealed Classes und Interfaces eignen sich, wenn die Menge zulässiger direkter Subtypen bewusst begrenzt werden soll. Damit wird Erweiterbarkeit Teil des expliziten Typvertrags.

Sie passen nicht zu Plugin- oder Extension-Points, bei denen unbekannte Implementierungen ausdrücklich erwünscht sind.

## Technisches Modell

Ein `sealed` Typ beschränkt seine direkten Subtypen auf eine definierte Menge. Ein zugelassener Subtyp muss seine weitere Erweiterbarkeit wiederum als `final`, `sealed` oder `non-sealed` festlegen.

```java
public sealed interface PaymentResult
        permits PaymentAccepted, PaymentRejected, PaymentPending {
}
```

`non-sealed` öffnet einen Ast der Hierarchie bewusst wieder. Diese Entscheidung sollte im Review sichtbar sein.

## Gute Einsatzfälle

- geschlossene fachliche Zustände,
- Ergebnis- und Fehlertypen,
- begrenzte Commands oder Events,
- Parser-/AST-Strukturen,
- interne Protokollmodelle,
- Kombination mit exhaustivem Pattern Matching.

## Wann nicht

Sealing ist unpassend, wenn:

- externe Module eigene Implementierungen hinzufügen sollen,
- die fachliche Variantenmenge bewusst offen ist,
- die Hierarchie nur zufällig heute wenige Subtypen hat,
- die öffentliche API Erweiterbarkeit verspricht.

## Regeln

### Muss

- Für einen `sealed` Typ muss nachvollziehbar sein, warum die Hierarchie geschlossen ist.
- Änderungen der erlaubten direkten Subtypen werden als Vertragsänderung reviewed.

### Sollte

- `non-sealed` wird nur eingesetzt, wenn die erneute Öffnung des betreffenden Asts gewollt ist.
- Geschlossene Hierarchien werden mit exhaustivem Pattern Matching kombiniert, wenn dadurch Fallbehandlung klarer wird.
- Variantennamen verwenden fachliche Sprache statt technische Platzhalter.

### Darf nicht

- Sealing darf nicht fehlende Modul- oder Package-Grenzen ersetzen.
- Eine echte Erweiterungsschnittstelle darf nicht versehentlich geschlossen werden.

## Beispiel

```java
static String message(PaymentResult result) {
    return switch (result) {
        case PaymentAccepted accepted -> "Accepted: " + accepted.transactionId();
        case PaymentRejected rejected -> "Rejected: " + rejected.reason();
        case PaymentPending pending -> "Pending: " + pending.reference();
    };
}
```

Wird die geschlossene Hierarchie erweitert, kann der Compiler fehlende Fallbehandlungen sichtbar machen.

## Reviewfragen

1. Ist die Variantenmenge fachlich oder technisch wirklich geschlossen?
2. Wer darf neue Varianten hinzufügen?
3. Ist die Hierarchie Teil eines öffentlichen Vertrags?
4. Welche Consumer müssen auf neue Varianten reagieren?
5. Ist Pattern Matching hier klarer als polymorphes Verhalten?

## Merksatz

> Sealed Types sind sinnvoll, wenn begrenzte Erweiterbarkeit Teil des Modells ist. Der Compiler kann diese Architekturannahme dann tatsächlich kontrollieren.

## Quellen

- Oracle Java — Sealed Classes and Interfaces: https://docs.oracle.com/en/java/javase/17/language/sealed-classes-and-interfaces.html
- JEP 409 — Sealed Classes: https://openjdk.org/jeps/409
