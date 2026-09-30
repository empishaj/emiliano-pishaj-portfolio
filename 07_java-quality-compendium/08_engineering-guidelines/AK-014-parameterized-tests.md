---
id: AK-014
legacy_ids:
  - QG-JAVA-014
title: Parametrisierte Tests für Varianten und Randfälle
artifact_type: engineering-guideline
domain: testing
status: active
maturity: reviewed
normative_level: recommended
last_validated: 2026-09-30
review_trigger:
  - Wechsel der JUnit-Major-Version
  - stark wachsende Testfallmatrizen
---

# AK-014 — Parametrisierte Tests für Varianten und Randfälle

## Kurzfassung

Parametrisierte Tests sind sinnvoll, wenn dieselbe Verhaltensregel mit mehreren Eingaben oder Randfällen geprüft werden soll. Sie reduzieren Duplikation, solange der einzelne Fall im Testreport verständlich bleibt.

## 1. Wann parametrisieren?

Gut geeignet sind:

- Grenzwerte,
- Äquivalenzklassen,
- mehrere gültige oder ungültige Formate,
- Mappingtabellen,
- Kombinationen derselben Regel.

```java
@ParameterizedTest(name = "quantity {0} is invalid")
@ValueSource(ints = {-10, -1, 0})
void nonPositiveQuantityIsRejected(int quantity) {
    assertThatThrownBy(() -> Quantity.of(quantity))
        .isInstanceOf(IllegalArgumentException.class);
}
```

## 2. Quellen passend wählen

JUnit Jupiter bietet unter anderem:

- `@ValueSource` für einfache Werte,
- `@NullSource` / `@EmptySource`,
- `@EnumSource`,
- `@CsvSource` für kleine tabellarische Fälle,
- `@MethodSource` für komplexere typsichere Argumente.

`@MethodSource` ist oft klarer als große CSV-Blöcke, sobald Domänenobjekte oder komplexe Erwartungen beteiligt sind.

## 3. Nicht alles in eine Matrix pressen

Wenn einzelne Fälle unterschiedliche fachliche Gründe oder unterschiedliche Erwartungen haben, sind separate Tests meist verständlicher. Ein riesiger parametrisierter Test kann Duplikation reduzieren und gleichzeitig die Spezifikation verschlechtern.

## 4. Testfallname als Diagnose

Bei vielen Varianten ist ein lesbarer Anzeigename wichtig:

```java
@ParameterizedTest(name = "{0} -> {1}")
@MethodSource("statusTransitions")
void transitionIsAllowed(OrderStatus from, OrderStatus to, boolean expected) { ... }
```

Ein CI-Fehler soll erkennen lassen, welche Variante gescheitert ist.

## 5. Zufallsdaten sind ein anderes Werkzeug

Parametrisierte Beispiele und Property-Based Testing verfolgen unterschiedliche Ziele. Parametrisierte Tests dokumentieren ausgewählte repräsentative Fälle; Property-Based Testing sucht über viele generierte Werte nach Verletzungen einer allgemeinen Eigenschaft.

## 6. Normative Regeln

### MUSS

- Alle Parameterfälle müssen dieselbe zentrale Verhaltensregel prüfen.
- Fehlgeschlagene Varianten müssen im Testreport identifizierbar sein.
- Randwerte werden bewusst ausgewählt und nicht zufällig gesammelt.

### SOLLTE

- kleine einfache Tabellen dürfen `@CsvSource` verwenden.
- komplexe fachliche Fälle werden typsicher über `@MethodSource` bereitgestellt.
- ein separater Test wird bevorzugt, wenn ein Fall eine eigene fachliche Erklärung braucht.

### DARF NICHT

- Parametrisierung wird nicht nur verwendet, um möglichst wenige Testmethoden zu haben.
- ein großer Datensatz ersetzt keine fachlich begründete Fallauswahl.

## 7. Prüffragen

1. Prüfen alle Zeilen wirklich dieselbe Regel?
2. Sind Grenzwerte und Äquivalenzklassen bewusst ausgewählt?
3. Ist im Fehlerfall der konkrete Datensatz sichtbar?
4. Wäre ein einzelner benannter Test für einen Sonderfall verständlicher?
5. Braucht der Test eher Property-Based Testing als eine wachsende Beispieltabelle?

## 8. Quellen

- JUnit User Guide — Parameterized Tests: https://docs.junit.org/current/user-guide/#writing-tests-parameterized-tests

## 9. Merksatz

> Parametrisierung ist gut für Varianten derselben Regel. Unterschiedliche Regeln verdienen unterschiedliche Tests.
