# EG-JAVA-005 — Text Blocks für lesbare mehrzeilige Strings

## Kurzfassung

Text Blocks sind seit Java 15 Bestandteil der Sprache. Sie verbessern die Lesbarkeit mehrzeiliger String-Literale, insbesondere für SQL, JSON, HTML, XML oder Testdaten.

Sie verändern jedoch **nicht** die Sicherheits- oder Semantikregeln des eingebetteten Formats. Ein SQL-Textblock schützt nicht vor SQL Injection; ein JSON-Textblock validiert kein JSON; ein HTML-Textblock macht kein Template sicher.

## Validierungsstatus

**VERIFIED** gegen Java Language Specification / Oracle Text Blocks Guide und JEP 378.

Primärquellen:

- Oracle Text Blocks Guide: https://docs.oracle.com/en/java/javase/18/text-blocks/index.html
- Java SE 21 Language Specification, Text Blocks: https://docs.oracle.com/javase/specs/jls/se21/html/jls-3.html#jls-3.10.6
- JEP 378: https://openjdk.org/jeps/378

## 1. Problem verstehen

Mehrzeilige Inhalte mussten früher häufig über Escape-Sequenzen und String-Konkatenation geschrieben werden:

```java
String sql = "SELECT id, name\n" +
             "FROM customer\n" +
             "WHERE status = ?\n";
```

Mit Text Block:

```java
String sql = """
    SELECT id, name
    FROM customer
    WHERE status = ?
    """;
```

Der Gewinn ist primär **Lesbarkeit und Wartbarkeit**.

## 2. Gute Einsatzfälle

- SQL in kleinen Repository-/Testkontexten,
- JSON-/XML-Testdaten,
- HTML-/Markdown-Fragmente,
- Prompt-/Template-Texte,
- erwartete mehrzeilige Testergebnisse,
- Konfigurationsbeispiele in Tests.

## 3. Was Text Blocks nicht lösen

### SQL Injection

Schlecht:

```java
String sql = """
    SELECT * FROM customer
    WHERE email = '%s'
    """.formatted(userInput);
```

Text Blocks ändern nichts daran, dass hier untrusted Input in SQL eingebettet wird.

Besser:

```java
String sql = """
    SELECT * FROM customer
    WHERE email = ?
    """;

jdbcTemplate.query(sql, rowMapper, userInput);
```

### JSON-Validierung

Ein Text Block ist weiterhin nur ein `String`. Wenn JSON strukturell wichtig ist, sollte es geparst oder über Typen erzeugt werden.

### Template-/XSS-Sicherheit

HTML in einem Text Block wird dadurch nicht automatisch escaped oder CSP-konform.

## 4. Whitespace bewusst behandeln

Text Blocks besitzen Regeln für incidental whitespace und Zeilenenden. Entwickler sollten nicht davon ausgehen, dass die visuelle Einrückung im Quellcode 1:1 dem resultierenden String entspricht, ohne die Text-Block-Regeln zu kennen.

Bei exakten Protokoll-/Signatur-/Snapshot-Formaten muss deshalb getestet werden, welcher String tatsächlich entsteht.

## 5. Coach-Perspektive

Die richtige Frage lautet:

> **Ist dieser Text fachlich ein String-Literal oder versuchen wir, ein strukturiertes Artefakt im Java-Code zu verstecken?**

Ein kleines SQL-Statement kann als Text Block sehr lesbar sein. Eine 800-Zeilen-OpenAPI-Spezifikation gehört dagegen nicht als String in eine Java-Klasse.

## 6. Normative Guideline

### MUSS

- Eingebettete SQL-Werte werden weiterhin parametrisiert.
- Bei sicherheitskritischen Formaten gelten die Sicherheitsregeln des Formats unabhängig vom Text Block.
- Sehr große strukturierte Artefakte werden als eigene Ressourcen/Dateien geführt, wenn dadurch Ownership, Validation oder Tooling verbessert werden.

### SOLLTE

- Text Blocks werden gegenüber verketteten mehrzeiligen Literalen bevorzugt, wenn sie Lesbarkeit erhöhen.
- Einbettungen behalten die natürliche Formatierung des enthaltenen Formats.
- Erwartete exakte Whitespaces werden durch Tests abgesichert.

### DARF NICHT

- Text Blocks dürfen nicht als Sicherheitsmechanismus dargestellt werden.
- String-Interpolation darf keine Query-Parameterisierung ersetzen.

## 7. Anti-Patterns

### riesiges Konfigurationsartefakt im Code

```java
String openApi = """
    ... hunderte Zeilen ...
    """;
```

Das erschwert spezialisierte Validatoren, Diffing und Ownership. Eine eigene `.yaml`-Datei ist meist geeigneter.

### dynamisches SQL per `formatted`

```java
"""
SELECT * FROM account WHERE owner = '%s'
""".formatted(owner)
```

Lesbar, aber unsicher.

## 8. Reviewfragen

1. Ist der Inhalt wirklich ein Literal oder sollte er eine eigene Datei/ein eigenes Modell sein?
2. Werden externe Werte sicher eingebunden?
3. Ist Whitespace semantisch relevant?
4. Gibt es einen Parser/Linter für das eingebettete Format?
5. Wird durch den Text Block nur Syntax schöner oder auch die eigentliche Struktur verständlicher?

## 9. Architektenperspektive

Text Blocks zeigen eine kleine, aber wichtige Regel:

> **Lesbarkeit ist Qualität, aber Lesbarkeit ersetzt keine Semantik, Validierung oder Security.**

Architekten sollten Syntaxverbesserungen nicht mit strukturellen Qualitätsverbesserungen verwechseln.

## 10. Review-Trigger

- Wechsel der Java-Baseline,
- neue Anforderungen an eingebettete Templates/Prompts,
- Security Findings durch String-Interpolation,
- Wachstum eingebetteter Artefakte über sinnvolle Quellcodegröße hinaus.
