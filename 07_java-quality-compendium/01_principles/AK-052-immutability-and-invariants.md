---
id: AK-052
legacy_ids:
  - ADR-052
title: Immutability, Invarianten und defensive Grenzen
artifact_type: engineering-principle
domain: software-design
status: active
maturity: reviewed
normative_level: recommended
last_validated: 2026-09-28
technology_baseline:
  java: "21+"
review_trigger:
  - grundlegende Änderung der Java-Baseline
---

# AK-052 — Immutability und Invarianten

## 1. Ziel

Mutable State ist nicht grundsätzlich falsch. Unkontrollierter, geteilter und schwer nachvollziehbarer Mutable State ist jedoch eine häufige Quelle für Fehler.

Das Ziel lautet deshalb nicht:

> „Alles muss immutable sein.“

Sondern:

> **Zustandsänderungen sollen explizit, lokal, regelkonform und möglichst schwer missbrauchbar sein.**

Immutability ist dafür ein starkes Werkzeug, besonders bei Value Objects, DTOs, Events, Konfiguration und nebenläufig genutzten Daten.

## 2. Invarianten zuerst

Eine Invariante ist eine Bedingung, die für einen gültigen Zustand gelten muss.

Beispiele:

- eine Menge darf nicht negativ sein,
- ein Zeitraum endet nicht vor seinem Beginn,
- eine Währung gehört immer zu einem Geldbetrag,
- eine fachliche ID besitzt ein gültiges Format,
- ein Statusübergang ist nur aus bestimmten Vorgängerzuständen erlaubt.

Der wichtigste Designschritt lautet:

> Baue Typen so, dass ungültige Zustände möglichst **nicht repräsentierbar** oder zumindest früh ablehnbar sind.

## 3. Records korrekt einordnen

Java Records sind laut Java-API **shallowly immutable**.

Das bedeutet:

- die Komponentenreferenzen sind final,
- sie können nach Konstruktion nicht neu zugewiesen werden,
- referenzierte mutable Objekte können aber weiterhin ihren internen Zustand ändern.

Deshalb ist dies nicht automatisch tief immutable:

```java
public record OrderSnapshot(List<OrderItem> items) { }
```

Wenn die übergebene Liste mutable bleibt, kann ihr Inhalt außerhalb des Records geändert werden.

Besser:

```java
public record OrderSnapshot(List<OrderItem> items) {
    public OrderSnapshot {
        items = List.copyOf(items);
    }
}
```

Auch dann müssen `OrderItem` selbst passend modelliert sein, wenn tiefe Unveränderlichkeit benötigt wird.

## 4. Defensive Copies

Defensive Copies sind sinnvoll an Vertrauens- oder Ownership-Grenzen.

```java
public final class Order {
    private final List<OrderItem> items;

    public Order(List<OrderItem> items) {
        this.items = List.copyOf(items);
    }

    public List<OrderItem> items() {
        return items;
    }
}
```

`List.copyOf` erzeugt eine unmodifizierbare Listendarstellung und schützt gegen spätere strukturelle Änderungen der ursprünglichen Liste.

Wichtig:

> Eine defensive Collection Copy macht referenzierte mutable Elemente nicht automatisch immutable.

## 5. Mutation als fachliche Operation

Wenn ein Objekt veränderlich sein muss, sollte die Änderung fachliche Semantik besitzen.

Schwach:

```java
order.setStatus("CANCELLED");
```

Stärker:

```java
order.cancel(reason, actor, clock);
```

Warum?

Die zweite Form kann:

- erlaubte Zustandsübergänge prüfen,
- Auditinformationen erzeugen,
- Domänenereignisse auslösen,
- verbotene Übergänge verhindern.

Mutable Domain Objects können damit sehr wohl robust sein, wenn Mutation **gekapselt** ist.

## 6. Fail Fast an sinnvollen Grenzen

Ungültige Werte sollten möglichst dort erkannt werden, wo ein Vertrag verletzt wird.

Aber nicht jeder `null`-Check gehört überall wiederholt.

Frage:

- Ist dies eine öffentliche oder externe Grenze?
- garantiert der aufrufende Typ bereits die Invariante?
- wäre ein eigener Value Object sinnvoller?

Beispiel:

```java
public record Quantity(int value) {
    public Quantity {
        if (value <= 0) {
            throw new IllegalArgumentException("quantity must be > 0");
        }
    }
}
```

Danach muss nicht jede Methode erneut `quantity > 0` prüfen.

## 7. Immutability und Concurrency

Immutable Werte reduzieren eine wichtige Klasse von Nebenläufigkeitsproblemen, weil ihr Zustand nach Konstruktion nicht verändert wird.

Aber:

> **Immutability allein macht kein nebenläufiges System korrekt.**

Weiterhin relevant sind:

- atomare Geschäftsoperationen,
- Datenbankkonkurrenz,
- Lost Updates,
- Message Ordering,
- Idempotenz,
- Locks oder Compare-and-Set,
- Ressourcenlimits.

Die Concurrency-Regeln werden separat behandelt.

## 8. Wann Immutability besonders sinnvoll ist

- Value Objects,
- API Request/Response Models,
- Domain Events,
- Configuration Objects,
- Cache Values,
- Nachrichten zwischen Threads,
- Snapshots,
- Security-/Identity Claims nach Validierung.

## 9. Wann Mutable State sinnvoll sein kann

- JPA-Entities unter kontrollierter Kapselung,
- Aggregates mit fachlichen Zustandsübergängen,
- performanzkritische interne Algorithmen,
- Builder während der Konstruktion,
- Frameworks mit explizitem Lifecycle.

Die Frage lautet nicht „mutable oder immutable?“ abstrakt, sondern:

> **Wer besitzt den Zustand, wer darf ihn ändern und welche Invarianten müssen dabei gelten?**

## 10. `final` richtig einordnen

`final` verhindert Reassignment einer Variable beziehungsweise Referenz. Es macht das referenzierte Objekt nicht automatisch immutable.

Darum ist:

```java
final List<String> values = new ArrayList<>();
values.add("x");
```

vollkommen zulässig.

`final` ist nützlich für Felder und kann Intent ausdrücken. Ein pauschaler Standard „alle lokalen Variablen und Parameter müssen final sein“ erzeugt dagegen häufig mehr visuelles Rauschen als Schutz und sollte organisationsspezifisch entschieden werden.

## 11. Security-Bezug

Gekapselte Invarianten können auch Sicherheitsfehler reduzieren.

Beispiele:

- Rollen-/Tenant-IDs nicht nachträglich beliebig überschreiben,
- validierte Pfade als eigene Typen,
- normalisierte Identifiers,
- immutable Security Contexts innerhalb eines Request-Lifecycles.

Das ersetzt keine Authorization. Es reduziert aber versehentliche Zustandsmanipulation.

## 12. Review-Schritte

1. Wer besitzt diesen Zustand?
2. Muss er nach Konstruktion verändert werden?
3. Falls ja: Welche fachliche Operation darf ihn ändern?
4. Welche Invarianten gelten?
5. Können sie beim Erzeugen oder Ändern zentral erzwungen werden?
6. Werden mutable Collections oder Arrays über Grenzen exponiert?
7. Ist die Unveränderlichkeit nur shallow?
8. Wird Immutability aus Performancegründen vermieden – und wurde das gemessen?

## 13. Anti-Patterns

### Record = automatisch tief immutable

Falsch. Records sind shallowly immutable.

### Jede Domänenänderung erzeugt ein neues riesiges Objektgraph

Kann unnötig teuer oder kompliziert sein.

### Defensive Copy überall

Copies haben Kosten. Sie sind dort sinnvoll, wo Ownership- oder Mutationrisiko besteht.

### Setter als universelle Änderungs-API

Versteckt fachliche Regeln und erlaubt ungültige Zustandsübergänge.

## 14. Quellen

- Java SE 21 `java.lang.Record`: Records sind shallowly immutable  
  https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/Record.html
- Java Language Specification — `final` variables
- AK-033 — Concurrency und Thread Safety
- AK-001 — Records als Datenträger

## 15. Coach-Merksatz

> Das Ziel ist nicht „immutable um jeden Preis“. Das Ziel ist, dass **Ownership, erlaubte Zustandsänderungen und Invarianten im Modell so klar sind, dass falsche Zustände schwer entstehen können**.
