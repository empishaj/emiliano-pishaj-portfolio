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
  - Wechsel der Java-LTS-Baseline
  - Security-Befund durch String-Interpolation
  - starkes Wachstum eingebetteter strukturierter Artefakte
---

# AK-005 — Text Blocks für lesbare mehrzeilige Strings

## Kurzfassung

Text Blocks verbessern die Lesbarkeit mehrzeiliger String-Literale, etwa für SQL, JSON, HTML, XML, Markdown oder Testdaten.

Sie ändern jedoch nichts an den Regeln des eingebetteten Formats. Ein SQL-Textblock schützt nicht vor SQL Injection, ein JSON-Textblock validiert kein JSON und ein HTML-Textblock macht keine Ausgabe XSS-sicher.

## 1. Der eigentliche Nutzen

Statt mehrzeiligen Inhalt über Escape-Sequenzen und Konkatenation zu schreiben:

```java
String sql = "SELECT id, name\n" +
             "FROM customer\n" +
             "WHERE status = ?\n";
```

kann die natürliche Struktur erhalten bleiben:

```java
String sql = """
    SELECT id, name
    FROM customer
    WHERE status = ?
    """;
```

Der Gewinn ist vor allem Lesbarkeit und Wartbarkeit.

## 2. Gute Einsatzfälle

- überschaubare SQL-Statements,
- JSON-/XML-Testdaten,
- erwartete mehrzeilige Testergebnisse,
- kleine HTML-/Markdown-Fragmente,
- Prompt- oder Template-Texte,
- Konfigurationsbeispiele in Tests.

## 3. Sicherheit bleibt Sache des Formats

### SQL

Unsicher bleibt unsicher:

```java
String sql = """
    SELECT * FROM customer
    WHERE email = '%s'
    """.formatted(userInput);
```

Parameterisierung ist weiterhin erforderlich:

```java
String sql = """
    SELECT * FROM customer
    WHERE email = ?
    """;

jdbcTemplate.query(sql, rowMapper, userInput);
```

### JSON

Ein Text Block ist nur ein String. Wenn Struktur relevant ist, wird das JSON geparst, validiert oder über Typen erzeugt.

### HTML und Templates

Output-Encoding, Template-Sicherheit und CSP werden nicht durch die Literalform ersetzt.

## 4. Whitespace bewusst behandeln

Text Blocks besitzen Regeln für Einrückung, Zeilenenden und incidental whitespace. Bei Formaten, bei denen exakte Zeichenfolgen relevant sind, wird der resultierende String getestet und nicht nur die visuelle Darstellung im Quellcode betrachtet.

## 5. Wann eine eigene Datei besser ist

Die entscheidende Frage lautet:

> Ist das wirklich ein String-Literal oder verstecken wir ein eigenständiges Artefakt im Java-Code?

Eine kleine Query kann als Text Block sehr gut lesbar sein. Eine große OpenAPI-Spezifikation, ein umfangreiches HTML-Template oder hunderte Zeilen Konfiguration gehören meist in ein eigenes Artefakt, weil dort spezialisierte Validatoren, Ownership und Diffing besser funktionieren.

## 6. Normative Regeln

### MUSS

- externe Werte in SQL werden weiterhin über sichere Parameterbindung verarbeitet.
- für sicherheitskritische Formate gelten die Sicherheitsregeln des jeweiligen Formats unabhängig vom Text Block.
- große strukturierte Artefakte werden ausgelagert, wenn dadurch Validierung, Ownership oder Wartbarkeit verbessert werden.

### SOLLTE

- Text Blocks werden gegenüber schwer lesbaren verketteten mehrzeiligen Literalen bevorzugt.
- die Formatierung des enthaltenen Formats bleibt erkennbar.
- exakte Whitespace-Semantik wird durch Tests abgesichert, wenn sie relevant ist.

### DARF NICHT

- Text Blocks werden nicht als Sicherheitsmechanismus beschrieben.
- String-Interpolation ersetzt keine Query-Parameterisierung.

## 7. Prüffragen

1. Ist der Inhalt tatsächlich ein Literal oder ein eigenständiges Artefakt?
2. Werden externe Werte sicher eingebunden?
3. Ist Whitespace semantisch relevant?
4. Gibt es einen Parser oder Linter für das eingebettete Format?
5. Verbessert der Text Block die Verständlichkeit oder macht er nur eine zu große eingebettete Struktur optisch erträglicher?

## 8. Architekturperspektive

Text Blocks sind eine kleine Sprachverbesserung. Sie erinnern an einen allgemeinen Grundsatz: bessere Syntax kann Lesbarkeit erhöhen, ersetzt aber weder Datenmodell, Validierung noch Security.

## 9. Quellen

- Oracle — Programmer's Guide to Text Blocks  
  https://docs.oracle.com/en/java/javase/18/text-blocks/index.html
- Java Language Specification — Text Blocks  
  https://docs.oracle.com/javase/specs/jls/se21/html/jls-3.html#jls-3.10.6
- JEP 378 — Text Blocks  
  https://openjdk.org/jeps/378

## 10. Merksatz

> Text Blocks machen mehrzeilige Strings lesbarer. Sie ändern weder die Semantik noch die Sicherheitsanforderungen des enthaltenen Formats.
