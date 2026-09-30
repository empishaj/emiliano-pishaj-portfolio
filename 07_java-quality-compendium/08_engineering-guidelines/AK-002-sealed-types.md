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
  - Wechsel der Java-LTS-Baseline
  - Öffnung eines Moduls für externe Erweiterungen
  - Änderung der fachlichen Variantenmenge
---

# AK-002 — Sealed Types für bewusst geschlossene Hierarchien

## Kurzfassung

Sealed Classes und Interfaces eignen sich, wenn die zulässigen direkten Subtypen bewusst begrenzt sein sollen. Damit wird Erweiterbarkeit Teil des expliziten Vertrags und kann vom Compiler unterstützt werden.

Sie passen nicht zu APIs oder Plugin-Modellen, bei denen unbekannte externe Implementierungen ausdrücklich erwünscht sind.

## 1. Was Java garantiert

Ein `sealed` Typ beschränkt seine direkten Subtypen auf eine definierte Menge. Erlaubte Subtypen müssen ihrerseits `final`, `sealed` oder `non-sealed` sein.

```java
public sealed interface PaymentResult
        permits PaymentAccepted, PaymentRejected, PaymentPending {
}
```

Mit `non-sealed` kann ein einzelner Ast der Hierarchie bewusst wieder geöffnet werden.

## 2. Gute Einsatzfälle

- geschlossene fachliche Ergebnis- oder Fehlertypen,
- kleine Zustands- oder Variantenmodelle,
- AST-/Parser-Strukturen,
- interne Protokollmodelle,
- Kombination mit exhaustivem Pattern Matching.

## 3. Wann nicht

Sealing ist ungeeignet, wenn:

- externe Module neue Implementierungen ergänzen sollen,
- eine Plugin-/Extension-Schnittstelle bewusst offen ist,
- die Variantenmenge fachlich noch nicht verstanden ist,
- die Beschränkung nur gewählt wird, weil heute zufällig wenige Implementierungen existieren.

Die entscheidende Frage lautet:

> Ist die Menge zulässiger Varianten tatsächlich Teil unseres Vertrags?

## 4. Zusammenspiel mit Pattern Matching

Geschlossene Hierarchien können Fallunterscheidungen klarer und überprüfbarer machen:

```java
static String message(PaymentResult result) {
    return switch (result) {
        case PaymentAccepted accepted -> "Accepted: " + accepted.transactionId();
        case PaymentRejected rejected -> "Rejected: " + rejected.reason();
        case PaymentPending pending -> "Pending: " + pending.reference();
    };
}
```

Wird eine neue erlaubte Variante ergänzt, kann der Compiler fehlende Behandlungen sichtbar machen.

## 5. Normative Regeln

### MUSS

- Für einen `sealed` Typ muss nachvollziehbar sein, warum die Hierarchie geschlossen ist.
- Änderungen der erlaubten Varianten werden wie Vertragsänderungen reviewed.
- `non-sealed` wird bewusst eingesetzt und nicht nur zur Umgehung einer Compile-Fehlermeldung.

### SOLLTE

- Fachliche Variantennamen drücken Domänensprache aus.
- Exhaustive Fallbehandlung wird genutzt, wenn sie die Modellklarheit verbessert.

### DARF NICHT

- Eine öffentliche Erweiterungsschnittstelle wird nicht versehentlich versiegelt.
- Sealed Types ersetzen kein sinnvolles Modul- oder Ownership-Modell.

## 6. Typische Fehler

### Technische Klassen statt fachlicher Varianten

Eine Hierarchie wird nur deshalb versiegelt, weil Klassen dasselbe Framework-Interface implementieren. Das sagt noch nichts über einen fachlich geschlossenen Vertrag aus.

### `non-sealed` ohne bewusste Entscheidung

Damit wird ein Teilbaum wieder offen. Diese Öffnung gehört genauso zum Modell wie die ursprüngliche Beschränkung.

### Öffentlicher Vertrag wird unbeabsichtigt starr

Wenn Consumer eigene Implementierungen benötigen, ist eine geschlossene Hierarchie ein Breaking Design Constraint.

## 7. Prüffragen

1. Ist die Variantenmenge wirklich geschlossen?
2. Wer darf neue Varianten hinzufügen?
3. Ist die Hierarchie Teil eines öffentlichen oder organisationsübergreifenden Vertrags?
4. Müssen Consumer bei einer neuen Variante angepasst werden?
5. Hilft exhaustive Behandlung oder liegt das Verhalten besser in den Typen selbst?

## 8. Architekturperspektive

Sealed Types zeigen, wie eine Designentscheidung direkt in die Sprache übersetzt werden kann: Erlaubte Erweiterbarkeit wird nicht nur beschrieben, sondern durch den Compiler kontrolliert.

## 9. Quellen

- Oracle Java — Sealed Classes and Interfaces  
  https://docs.oracle.com/en/java/javase/17/language/sealed-classes-and-interfaces.html
- JEP 409 — Sealed Classes  
  https://openjdk.org/jeps/409

## 10. Merksatz

> `sealed` ist sinnvoll, wenn Geschlossenheit Teil des fachlichen oder technischen Vertrags ist. Es ist kein allgemeines Qualitätsmerkmal einer Klassenhierarchie.
