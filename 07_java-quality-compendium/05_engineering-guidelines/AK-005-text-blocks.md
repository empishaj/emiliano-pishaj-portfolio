---
id: AK-005
legacy_ids:
  - QG-JAVA-005
title: Text Blocks für lesbare mehrzeilige Strings
artifact_type: engineering-guideline
domain: java-language
status: active
maturity: reviewed
normative_level: recommended
last_validated: 2026-09-30
technology_baseline:
  java: "15+"
review_trigger:
  - Wechsel der Java-Baseline
  - Security-Befund durch String-Interpolation
  - starkes Wachstum eingebetteter Artefakte
---

# AK-005 — Text Blocks für lesbare mehrzeilige Strings

## Kurzfassung

Text Blocks verbessern die Lesbarkeit mehrzeiliger String-Literale, etwa für SQL, JSON, XML, HTML oder Testdaten.

Sie verändern nicht die Sicherheits- oder Semantikregeln des eingebetteten Formats. Ein SQL-Textblock schützt nicht vor SQL Injection, ein JSON-Textblock validiert kein JSON und HTML in einem Textblock wird nicht automatisch sicher.

## Beispiel

Statt:

```java
String sql = "SELECT id, name\n" +
             "FROM customer\n" +
             "WHERE status = ?\n";
```

kann ein Text Block die natürliche Struktur erhalten:

```java
String sql = """
    SELECT id, name
    FROM customer
    WHERE status = ?
    """;
```

Der Gewinn ist Lesbarkeit, nicht zusätzliche Sicherheit.

## Geeignete Einsatzfälle

- kleine SQL-Statements,
- JSON-/XML-Testdaten,
- HTML-/Markdown-Fragmente,
- Prompt- oder Template-Texte,
- erwartete mehrzeilige Testergebnisse.

Wenn ein eingebettetes Artefakt groß wird oder eigenes Tooling, Ownership und Validierung braucht, ist eine eigene Datei häufig besser als ein sehr großer Java-String.

## Sicherheit bleibt formatspezifisch

### SQL

Untrusted Werte werden weiterhin parametrisiert:

```java
String sql = """
    SELECT * FROM customer
    WHERE email = ?
    """;

jdbcTemplate.query(sql, rowMapper, userInput);
```

### JSON

Ein Text Block ist nur ein `String`. Strukturell relevantes JSON sollte geparst, validiert oder aus typisierten Modellen erzeugt werden.

### HTML

Text Blocks ersetzen weder Output Encoding noch Content Security Policy oder sichere Template-Mechanismen.

## Whitespace

Text Blocks besitzen Regeln für incidental whitespace und Zeilenenden. Wenn exakte Zeichenfolgen relevant sind, etwa bei Snapshots, Signaturen oder Protokollformaten, muss der resultierende String getestet werden.

## Regeln

### Muss

- Externe Werte in SQL werden weiterhin parametrisiert.
- Sicherheitsregeln des eingebetteten Formats gelten unverändert.
- Große strukturierte Artefakte werden als eigene Ressourcen geführt, wenn dadurch Validation, Diffing oder Ownership verbessert werden.

### Sollte

- Text Blocks werden gegenüber verketteten mehrzeiligen Literalen bevorzugt, wenn sie die Lesbarkeit verbessern.
- Die Formatierung orientiert sich am eingebetteten Format.
- Semantisch relevanter Whitespace wird durch Tests abgesichert.

### Darf nicht

- Text Blocks dürfen nicht als Sicherheitsmechanismus dargestellt werden.
- String-Interpolation darf Query-Parameterisierung nicht ersetzen.

## Reviewfragen

1. Ist der Inhalt wirklich ein Literal oder sollte er eine eigene Datei beziehungsweise ein eigenes Modell sein?
2. Werden externe Werte sicher eingebunden?
3. Ist Whitespace semantisch relevant?
4. Gibt es einen geeigneten Parser oder Linter für das eingebettete Format?
5. Verbessert der Text Block die Struktur oder nur die Syntax?

## Merksatz

> Lesbarkeit ist Qualität. Sie ersetzt aber weder Semantik noch Validierung oder Security.

## Quellen

- Oracle Text Blocks Guide: https://docs.oracle.com/en/java/javase/18/text-blocks/index.html
- Java SE 21 Language Specification, Text Blocks: https://docs.oracle.com/javase/specs/jls/se21/html/jls-3.html#jls-3.10.6
- JEP 378: https://openjdk.org/jeps/378
