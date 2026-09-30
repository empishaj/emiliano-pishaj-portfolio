---
id: AK-001
legacy_ids:
  - QG-JAVA-001
title: Records für transparente Datenträger
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
  - wesentliche Änderungen verwendeter Persistence- oder Serialization-Frameworks
  - Security-Befund zu Logging sensibler Daten
---

# AK-001 — Records für transparente Datenträger

## Kurzfassung

Java Records eignen sich für Typen, deren wesentliche Aufgabe darin besteht, eine feste Menge von Werten transparent zu repräsentieren. Typische Beispiele sind DTOs, Commands, Queries, Events, Projektionen und kleine Value Objects.

Sie sind kein allgemeiner Ersatz für Domänenobjekte oder Persistenzmodelle.

## Technisches Modell

Für jede Record-Komponente erzeugt Java ein `private final` Feld und einen Accessor. Records sind implizit `final`; der Compiler erzeugt außerdem den kanonischen Konstruktor sowie `equals`, `hashCode` und `toString`.

Wichtig: Records sind nur **shallowly immutable**. Eine finale Referenz kann weiterhin auf ein veränderbares Objekt zeigen.

```java
public record SearchResult(List<String> hits) {
    public SearchResult {
        hits = List.copyOf(hits);
    }
}
```

Wenn der Typ als unveränderlicher Wert verstanden wird, müssen mutable Collections oder Arrays daher defensiv behandelt werden.

## Geeignete Einsatzfälle

Records passen gut für:

- REST Request-/Response-Modelle,
- Commands und Queries,
- Domain Events,
- Read Models und Projektionen,
- Konfigurationstypen,
- kleine Value Objects mit überschaubaren Invarianten.

Ein Record darf Verhalten enthalten. Entscheidend ist nicht, ob Methoden vorhanden sind, sondern ob die feste Wertestruktur weiterhin die natürliche Semantik des Typs beschreibt.

```java
public record Percentage(int value) {
    public Percentage {
        if (value < 0 || value > 100) {
            throw new IllegalArgumentException("percentage must be between 0 and 100");
        }
    }

    public double asFactor() {
        return value / 100.0;
    }
}
```

## Wann eine normale Klasse meist besser passt

### Komplexe Aggregate

Wenn ein Typ vor allem Lifecycle, Zustandsübergänge und komplexe Invarianten schützt, ist transparente Datenrepräsentation nicht sein Hauptzweck.

### Persistenzmodelle

JPA-Entities besitzen Anforderungen an Identität, Lifecycle und je nach Modell Frameworkmechanismen. Ein Record ist dafür nicht automatisch die richtige Wahl.

Eine saubere Trennung kann so aussehen:

```text
Persistence Entity
      ↓ Mapping
Domain / Application Model
      ↓ Mapping
API Record
```

### Sensible Daten

Der generierte `toString()` enthält Komponentenwerte. Records mit Passwörtern, Tokens oder anderen Geheimnissen brauchen deshalb besondere Vorsicht und sind häufig die falsche Modellwahl.

## Regeln

### Muss

- Mutable Collections und Arrays werden defensiv kopiert, wenn der Typ als unveränderlicher Wert gelten soll.
- Sensible Komponenten werden hinsichtlich `toString()` und Logging bewertet.

### Sollte

- Records werden bevorzugt, wenn eine feste, transparente Wertemenge die natürliche Semantik des Typs ist.
- API-, Event- und Persistenzmodelle bleiben getrennt, wenn sie unabhängig voneinander evolvieren müssen.
- Fachlich aussagekräftige Typen werden primitiven String-Sammlungen vorgezogen, wenn dadurch Invarianten oder Bedeutung klarer werden.

### Nicht als Begründung verwenden

- „Ein Record hat weniger Boilerplate“ reicht allein nicht als Modellierungsgrund.
- „Record bedeutet immutable“ ist falsch, sobald mutable referenzierte Objekte enthalten sind.

## Reviewfragen

1. Ist der Typ tatsächlich ein transparenter Datenträger?
2. Enthält er mutable Komponenten?
3. Können sensible Werte über `toString()` sichtbar werden?
4. Wird ein Persistenzmodell unnötig direkt als API-Vertrag verwendet?
5. Sind die Invarianten klein genug, um in diesem Typ verständlich geschützt zu werden?

## Merksatz

> Ein Record ist dann stark, wenn die feste Wertestruktur selbst die Semantik des Typs ausdrückt. Weniger Code ist ein Nebeneffekt, nicht der Architekturgrund.

## Quellen

- Oracle Java SE 21 — Record Classes: https://docs.oracle.com/en/java/javase/21/language/records.html
- Java SE 21 API — `java.lang.Record`: https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/Record.html
- JEP 395 — Records: https://openjdk.org/jeps/395
