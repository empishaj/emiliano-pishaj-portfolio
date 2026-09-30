---
id: AK-001
legacy_ids:
  - QG-JAVA-001
title: Java Records für transparente Datenträger
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

# AK-001 — Java Records für transparente Datenträger

## Kurzfassung

Records eignen sich für Typen, deren wesentliche Aufgabe darin besteht, eine feste Menge von Werten transparent zu tragen. Typische Beispiele sind Request-/Response-Modelle, Commands, Queries, Events, Projektionen und kleine Value Objects.

Ein Record ist kein allgemeiner Ersatz für Klassen. Wenn ein Typ einen ausgeprägten Lifecycle, kontrollierte Zustandsübergänge, Proxy-Anforderungen oder bewusst veränderlichen Zustand besitzt, ist eine normale Klasse häufig die bessere Wahl.

## 1. Was ein Record ausdrückt

Ein Record macht die Zustandsbeschreibung eines Typs direkt im Header sichtbar:

```java
public record OrderSummary(
    OrderId id,
    OrderStatus status,
    Money total
) {}
```

Java erzeugt daraus unter anderem Komponentenfelder, Accessors, den kanonischen Konstruktor sowie `equals`, `hashCode` und `toString`.

Records sind implizit `final`. Ihre Komponentenreferenzen können nach der Konstruktion nicht neu zugewiesen werden.

## 2. Shallow Immutability

Records sind nur **shallowly immutable**. Referenzierte Objekte können weiterhin veränderlich sein.

```java
public record SearchResult(List<String> hits) {}
```

Wird eine veränderliche Liste übergeben, kann ihr Inhalt außerhalb des Records geändert werden. Wenn der Record als unveränderlicher Wert verstanden werden soll, wird an der Grenze defensiv kopiert:

```java
public record SearchResult(List<String> hits) {
    public SearchResult {
        hits = List.copyOf(hits);
    }
}
```

Das schützt die Collection-Struktur. Enthaltene mutable Elemente werden dadurch nicht automatisch unveränderlich.

## 3. Gute Einsatzfälle

Records passen besonders gut zu:

- REST-/API-Request- und Response-Modellen,
- Commands und Queries,
- Domain Events,
- Read Models und Projektionen,
- kleinen Value Objects,
- unveränderlicher Konfiguration,
- strukturierten Rückgabewerten.

Records dürfen Verhalten und lokale Invarianten enthalten. Ein kleines Value Object kann deshalb sinnvoll als Record modelliert werden:

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

## 4. Wann eine normale Klasse besser passt

### JPA-Entities und Proxy-Modelle

Persistenzmodelle besitzen eigene Lifecycle- und Frameworkanforderungen. API- oder Domänenmodelle sollten nicht nur deshalb die Form des Persistenzmodells übernehmen.

### Aggregate mit Lifecycle

Ein fachliches Aggregate schützt typischerweise Invarianten über mehrere Zustandsänderungen. Sein Hauptzweck ist Verhalten und Konsistenz, nicht transparente Datenrepräsentation.

### bewusst veränderliche Objekte

Wenn Mutation Teil des Modells ist, vermittelt ein Record häufig die falsche Semantik.

### sensible Daten

Der automatisch generierte `toString()` enthält Komponentenwerte. Passwörter, Tokens und Secrets gehören deshalb nicht unreflektiert in Records, die geloggt werden könnten.

## 5. Collections und Arrays

Mutable Collections werden an Ownership-Grenzen defensiv behandelt.

```java
public record PageResponse<T>(List<T> content, int total) {
    public PageResponse {
        content = List.copyOf(content);
        if (total < 0 || content.size() > total) {
            throw new IllegalArgumentException("invalid page metadata");
        }
    }
}
```

Arrays sind besonders kritisch:

- sie bleiben mutable,
- generierte Equality verhält sich bei Arrays nicht wie ein inhaltlicher Arrayvergleich,
- Binärdaten können Logging und Serialisierung unnötig belasten.

Wenn ein Array unvermeidbar ist, müssen Kopier-, Equality- und Darstellungssemantik bewusst gelöst werden.

## 6. Validierung und Trust Boundaries

Ein Record macht externe Eingaben nicht vertrauenswürdig.

Syntaktische API-Validierung kann zum Beispiel mit Bean Validation erfolgen. Fachliche Regeln und Autorisierung gehören weiterhin in die dafür verantwortete Schicht.

```java
public record CreateUserRequest(
    @NotBlank @Size(max = 120) String displayName,
    @NotBlank @Email @Size(max = 254) String email
) {}
```

Eine vom Client gelieferte Tenant-ID ist keine Autorisierungsentscheidung. Identitäts- und Tenant-Kontext müssen aus vertrauenswürdigen Quellen stammen.

## 7. Framework-Grenzen

Bei Nutzung eines Records muss das konkrete Framework Konstruktor-/Record-Binding unterstützen. Das betrifft insbesondere:

- Persistence,
- Serialisierung,
- UI-Frameworks,
- Konfigurationsbinding.

Framework-Kompatibilität wird gegen die tatsächlich eingesetzte Version geprüft. Eine Sprachfunktion allein garantiert keine Framework-Eignung.

## 8. Normative Regeln

### MUSS

- Mutable Collections oder Arrays werden geschützt, wenn Unveränderlichkeit Teil des Vertrags ist.
- Sensible Komponenten werden hinsichtlich Logging und `toString()` bewertet.
- externe Eingaben werden an der Systemgrenze validiert.

### SOLLTE

- Records werden für kleine, transport- oder wertorientierte Typen bevorzugt, wenn ihre Semantik passt.
- Persistenz-, API- und Domänenmodelle werden nicht aus Bequemlichkeit gleichgesetzt.
- einfache lokale Invarianten dürfen im kompakten Konstruktor geschützt werden.

### DARF NICHT

- `record` wird nicht allein wegen weniger Boilerplate gewählt.
- ein Record mit mutablem Objektgraph darf nicht pauschal als tief immutable bezeichnet werden.
- ein Client-geliefertes Identitäts- oder Tenant-Feld darf nicht allein durch seine Existenz im Record als vertrauenswürdig gelten.

## 9. Prüffragen

1. Ist der Typ hauptsächlich ein transparenter Datenträger oder besitzt er einen eigenen Lifecycle?
2. Gibt es mutable Komponenten?
3. Können sensible Werte über `toString()` oder Logging sichtbar werden?
4. Wird ein technisches Persistenzmodell unnötig nach außen exponiert?
5. Unterstützt das verwendete Framework Records in der benötigten Rolle?
6. Sind Invarianten lokal genug, um im Typ selbst geschützt zu werden?
7. Ist die Null- und Optionalitätssemantik im Vertrag klar?

## 10. Architekturperspektive

Records sind keine Architekturstrategie. Sie sind eine Sprachfunktion, mit der Verantwortung und Absicht im Code klarer ausgedrückt werden können.

Der wichtigere Grundsatz lautet: Ein Typ sollte möglichst eindeutig zeigen, ob er Daten transportiert, fachliches Verhalten kapselt oder technischen Lifecycle trägt.

## 11. Quellen

- Oracle Java SE 21 — Record Classes  
  https://docs.oracle.com/en/java/javase/21/language/records.html
- Java SE 21 API — `java.lang.Record`  
  https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/Record.html
- JEP 395 — Records  
  https://openjdk.org/jeps/395

## 12. Merksatz

> Records sind stark, wenn die feste Menge ihrer Werte die eigentliche Semantik des Typs beschreibt. Sie sind ungeeignet, wenn Lifecycle, Verhalten oder Frameworkanforderungen wichtiger sind als transparente Datenrepräsentation.
