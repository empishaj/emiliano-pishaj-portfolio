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
  - Wechsel der Java-LTS-Baseline
  - neue relevante Pattern-Matching-Sprachfeatures
  - wiederholte Typ-Switches in mehreren Modulen
---

# AK-003 — Pattern Matching und switch

## Kurzfassung

Pattern Matching reduziert wiederholte Typprüfungen und Casts und kann geschlossene Fallunterscheidungen deutlich lesbarer machen. Besonders gut funktioniert es bei Transformationen und in Kombination mit geschlossenen Hierarchien.

Es ersetzt Polymorphie nicht. Wenn Verhalten natürlich zu einem Typ gehört und dieselbe Typunterscheidung an vielen Stellen wiederholt wird, liegt die Verantwortung möglicherweise an der falschen Stelle.

## 1. Grundidee

Statt Typprüfung und Cast getrennt zu schreiben:

```java
if (result instanceof Success) {
    Success success = (Success) result;
    return success.value();
}
```

kann Java die Bindung direkt ausdrücken:

```java
if (result instanceof Success success) {
    return success.value();
}
```

Bei einer geschlossenen Variantenmenge kann ein `switch` die Fälle kompakt und überprüfbar darstellen.

## 2. Gute Einsatzfälle

- Mapping an Systemgrenzen,
- Parser und ASTs,
- Ergebnis- und Fehlertypen,
- Transformationen,
- Projektionen,
- kleine geschlossene Variantentypen.

Pattern Matching ist besonders passend, wenn die Verantwortung der Operation **außerhalb** der Varianten liegt, etwa in einem Mapper.

## 3. Pattern Matching oder Polymorphie?

Pattern Matching ist oft klarer, wenn:

- eine Transformation mehrere Varianten abbildet,
- die Variantenmenge bewusst geschlossen ist,
- verschiedene unabhängige Auswertungen benötigt werden.

Polymorphie ist oft klarer, wenn:

- das Verhalten natürlicherweise zum Objekt gehört,
- dieselbe Fallunterscheidung an vielen Stellen wiederholt wird,
- neue Subtypen bewusst offen ergänzt werden sollen.

Ein großer `switch` ist deshalb nicht automatisch schlecht. Er ist ein Signal, die Verantwortungsgrenze zu prüfen.

## 4. Beispiel mit geschlossener Hierarchie

```java
static ProblemView toView(PaymentResult result) {
    return switch (result) {
        case Accepted accepted -> new ProblemView("accepted", accepted.transactionId());
        case Rejected rejected -> new ProblemView("rejected", rejected.reason());
        case Pending pending -> new ProblemView("pending", pending.reference());
    };
}
```

Bei einer vollständig geschlossenen Hierarchie kann das Weglassen eines pauschalen `default` sinnvoll sein, damit neue Varianten bewusst behandelt werden müssen.

## 5. Normative Regeln

### MUSS

- Die Fallunterscheidung muss semantisch klarer werden; Zeilenersparnis allein ist kein Grund.
- Wiederholte große Typ-Switches werden auf falsch platzierte Verantwortung geprüft.
- Bei offenen Verträgen muss mit zukünftigen Varianten gerechnet werden.

### SOLLTE

- Mapping- und Adaptercode darf Pattern Matching bevorzugen, wenn Transformation seine natürliche Aufgabe ist.
- Bei bewusst geschlossenen Hierarchien wird geprüft, ob exhaustive Behandlung Fehler bei Evolution sichtbar machen kann.

### DARF NICHT

- Pattern Matching ersetzt keine fehlende Domänenmodellierung.
- Ein pauschaler `default` darf fachlich relevante neue Varianten nicht unbemerkt verschlucken.

## 6. Typische Fehler

### Mega-Switch

Wenn ein `switch` immer weiter wächst, kann das auf eine falsche Modell- oder Modulgrenze hindeuten.

### Wiederholte Fallunterscheidung

Wenn Fläche, Umfang, Validierung und Persistenz jeweils erneut nach demselben Typ verzweigen, sollte geprüft werden, ob Verhalten besser gekapselt werden kann.

### Fachlogik in technischen Adaptern

Controller oder Persistence-Code sollten nicht nur wegen bequemer Pattern Syntax zum Ort zentraler fachlicher Entscheidungen werden.

## 7. Prüffragen

1. Ist die Fallmenge offen oder geschlossen?
2. Beschreibt der `switch` eine Transformation oder eigentlich Domänenverhalten?
3. Wiederholt sich dieselbe Unterscheidung an mehreren Stellen?
4. Wird Evolution sichtbar oder durch einen pauschalen `default` verdeckt?
5. Ist die resultierende Struktur leichter zu verstehen und zu testen?

## 8. Architekturperspektive

Pattern Matching ist dann wertvoll, wenn es eine bereits sinnvolle Modellgrenze klarer ausdrückt. Sprachkomfort sollte niemals eine unklare Verantwortungsverteilung verdecken.

## 9. Quellen

- Oracle Java SE 21 — Pattern Matching  
  https://docs.oracle.com/en/java/javase/21/language/pattern-matching.html
- Oracle Java SE 21 — Pattern Matching for switch  
  https://docs.oracle.com/en/java/javase/21/language/pattern-matching-switch.html
- JEP 441 — Pattern Matching for switch  
  https://openjdk.org/jeps/441

## 10. Merksatz

> Explizite Varianten sind hilfreich. Wenn dieselbe Typunterscheidung überall auftaucht, sollte jedoch zuerst die Verantwortungsgrenze geprüft werden.
