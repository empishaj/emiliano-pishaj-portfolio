# EG-JAVA-001 — Records für transparente Datenträger

## Kurzfassung

Java Records sind für **feste, transparente Datenaggregate** gedacht. Sie reduzieren Boilerplate und machen die Datenstruktur absichtlich sichtbar. Sie sind deshalb sehr gut für DTOs, Commands, Queries, Events, API-Modelle und kleine Value-Carrier geeignet.

Sie sind **kein allgemeiner Ersatz für Domänenobjekte**. Wenn ein Typ komplexes Verhalten, kontrollierte Zustandsübergänge, Framework-Proxies, zusätzliche mutable Zustände oder Identitäts-/Lifecycle-Semantik benötigt, ist eine normale Klasse häufig geeigneter.

## Validierungsstatus

**VERIFIED** gegen Java SE 21 / Oracle Language Guide und `java.lang.Record`.

Wesentliche Primärquellen:

- Oracle Java SE 21 — Record Classes: https://docs.oracle.com/en/java/javase/21/language/records.html
- Java SE 21 API — `java.lang.Record`: https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/Record.html
- JEP 395 — Records: https://openjdk.org/jeps/395

## 1. Problem verstehen

In klassischen Java-Anwendungen entstehen viele Typen, deren Aufgabe ausschließlich darin besteht, eine feste Menge von Werten zu transportieren. Eine traditionelle Klasse braucht dafür häufig Felder, Konstruktor, Getter, `equals`, `hashCode` und `toString`.

Das Problem ist nicht nur Boilerplate. Es ist **semantisches Rauschen**: Der Leser muss erst prüfen, ob hinter der Klasse Verhalten steckt oder ob sie nur Daten trägt.

Ein Record macht diese Absicht explizit:

```java
public record OrderSummary(
    OrderId id,
    OrderStatus status,
    Money total
) {}
```

Der Typ sagt damit: Diese drei Komponenten sind seine wesentliche Zustandsbeschreibung.

## 2. Was Records laut Java tatsächlich garantieren

Ein Record ist ein spezieller Klassentyp. Für jede Komponente erzeugt der Compiler ein `private final` Feld und einen öffentlichen Accessor. Außerdem werden kanonischer Konstruktor, `equals`, `hashCode` und `toString` bereitgestellt.

Records sind implizit `final` und können nicht erweitert werden.

Wichtig ist der Begriff **shallowly immutable**: Die Referenzen der Komponenten sind final. Das bedeutet nicht, dass referenzierte Objekte automatisch unveränderlich werden.

```java
public record SearchResult(List<String> hits) {}
```

`hits` kann weiterhin eine mutable `ArrayList` sein. Wer echte Unveränderlichkeit benötigt, muss defensiv kopieren:

```java
public record SearchResult(List<String> hits) {
    public SearchResult {
        hits = List.copyOf(hits);
    }
}
```

## 3. Wann ein Record eine gute Wahl ist

### Sehr gut geeignet

- REST Request-/Response-DTOs
- Commands und Queries
- Domain Events
- kleine Value Carrier
- Konfigurationstypen
- Projektionen / Read Models
- strukturierte Ergebnisse

Beispiel:

```java
public record CreateOrderCommand(
    CustomerId customerId,
    List<OrderLineCommand> lines
) {
    public CreateOrderCommand {
        lines = List.copyOf(lines);
    }
}
```

### Möglich, aber bewusst entscheiden

Records können auch Verhalten und Validierung enthalten. Sie sind nicht auf „anämische DTOs“ beschränkt.

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

Für kleine Value Objects kann das hervorragend funktionieren.

## 4. Wann ich keinen Record als Default wählen würde

### JPA Entities

Persistenzmodelle haben häufig Anforderungen an Framework-Mechanismen, Lifecycle, Identität, Lazy Loading oder Proxies. Ein Record ist hier nicht automatisch die richtige Wahl.

Die architektonisch sauberere Trennung lautet oft:

```text
JPA Entity
   ↓ Mapping
Domain / Application Model
   ↓ Mapping
API Record
```

### komplexe Aggregate

Ein Aggregate schützt typischerweise Invarianten über mehrere Zustandsübergänge. Der primäre Zweck ist dann Verhalten und Konsistenz, nicht transparente Datenrepräsentation.

### mutable Lifecycle-Objekte

Wenn sich ein Objekt absichtlich intern verändert und Identität über Zeit besitzt, vermittelt ein Record häufig die falsche Semantik.

### geheime Daten

Der automatisch generierte `toString()` enthält Komponentenwerte. Das ist für Credentials, Tokens oder andere sensible Werte gefährlich.

```java
// Schlechte Idee: versehentliches Logging kann das Secret ausgeben.
public record Credentials(String username, String password) {}
```

## 5. Coach-Perspektive: Stelle nicht die Frage „Record oder Klasse?“

Die bessere Frage lautet:

> **Welche Semantik soll dieser Typ ausdrücken?**

Wenn die Antwort lautet:

> „Eine feste Menge transparenter Werte ist der wesentliche Zustand dieses Typs“

ist ein Record stark.

Wenn die Antwort lautet:

> „Der Typ schützt Verhalten, Lifecycle, Invarianten oder technische Framework-Semantik“

musst du genauer prüfen.

## 6. Normative Guideline

### MUSS

- Mutable Collections/Arrays werden defensiv kopiert, wenn der Record als unveränderlicher Wert verstanden werden soll.
- Sensible Komponenten werden hinsichtlich `toString()` und Logging bewertet.
- Record-Komponenten bekommen fachlich verständliche Typen statt primitiver String-Sammlungen, wenn dies die Domäne klarer macht.

### SOLLTE

- Records werden für transportorientierte, zustandszentrierte Typen bevorzugt.
- Validierung einfacher Invarianten darf im kompakten Konstruktor erfolgen.
- API-/Event-Records werden von Persistenzobjekten getrennt.

### DARF NICHT

- Ein Record darf nicht allein deshalb gewählt werden, weil er weniger Code benötigt.
- „Record = immutable“ darf nicht behauptet werden, wenn mutable Referenzen enthalten sind.

## 7. Anti-Patterns

### Record mit veränderbarer Collection ohne Schutz

```java
public record Team(List<Member> members) {}
```

Problem: Der Aufrufer kann die ursprüngliche Liste später verändern.

Besser:

```java
public record Team(List<Member> members) {
    public Team {
        members = List.copyOf(members);
    }
}
```

### Record als Datenbankmodell aus Bequemlichkeit

Wenn Persistenzanforderungen die Form des Domänen-/API-Modells bestimmen, koppeln wir technische und fachliche Modelle unnötig.

## 8. Testing und Review

Reviewer fragen:

1. Ist der Typ wirklich ein transparenter Datenträger?
2. Gibt es mutable Komponenten?
3. Werden sensible Werte automatisch in `toString()` sichtbar?
4. Wird ein Persistence-Modell fälschlich direkt als API verwendet?
5. Sind Invarianten klein genug, um im Record sinnvoll geschützt zu werden?

## 9. Architektenperspektive

Records sind keine Architekturstrategie. Sie sind eine **Sprachfunktion, die Absicht im Code sichtbar machen kann**.

Der EA-/Architecture-Lernpunkt lautet:

> Gute technische Leitplanken reduzieren Mehrdeutigkeit. Ein Typ sollte seine Verantwortung möglichst deutlich ausdrücken.

## 10. Review-Trigger

Diese Guideline wird geprüft bei:

- Wechsel der Java-LTS-Baseline,
- wesentlichen Änderungen in verwendeten Persistence-/Serialization-Frameworks,
- neuen Sicherheitsanforderungen an Logging/Sensitive Data,
- Erkenntnissen aus produktiven Fehlern, bei denen Record-Semantik eine Rolle spielte.
