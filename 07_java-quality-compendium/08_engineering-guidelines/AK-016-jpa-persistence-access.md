---
id: AK-016
legacy_ids:
  - QG-JAVA-016
title: JPA- und Hibernate-Zugriffe bewusst gestalten
artifact_type: engineering-guideline
domain: persistence
status: active
maturity: reviewed
normative_level: recommended
last_validated: 2026-09-30
review_trigger:
  - Wechsel der Hibernate- oder Jakarta-Persistence-Baseline
  - Performance-Incident mit Datenbankbezug
  - wiederkehrende N+1-, Locking- oder Transaktionsprobleme
---

# AK-016 — JPA- und Hibernate-Zugriffe bewusst gestalten

## Kurzfassung

JPA abstrahiert Persistenzzugriffe, aber nicht die Kosten und Semantik der Datenbank. Gute Persistence-Schichten machen Query-Form, Transaktionsgrenzen, benötigte Daten und Concurrency-Annahmen sichtbar.

Es gibt keine sinnvolle Universalregel wie „alle Beziehungen immer LAZY“, „immer JOIN FETCH“ oder „Batch Size = X“. Fetching, Locking und Transaktionsumfang richten sich nach dem konkreten Use Case und werden gemessen.

## 1. N+1 als Query-Problem verstehen

N+1 entsteht, wenn eine initiale Abfrage mehrere Entitäten lädt und anschließend pro Entität zusätzliche Queries für benötigte Beziehungen ausgelöst werden.

Mögliche Gegenmaßnahmen sind abhängig vom Zugriffsmuster:

- DTO-/Projection-Query,
- gezieltes `JOIN FETCH`,
- Entity Graph,
- Batch Fetching,
- Subselect Fetching,
- anderes Aggregat- oder Read-Model-Design.

Hibernate dokumentiert ausdrücklich, dass SELECT-Fetching anfällig für N+1 ist und Batch Fetching die Anzahl der Roundtrips reduzieren kann. Häufig ist eine zielgerichtete Projection oder ein Join jedoch klarer, wenn die benötigten Daten bereits bekannt sind.

## 2. Fetching ist use-case-spezifisch

Eine Relation ist nicht deshalb „richtig“ konfiguriert, weil sie überall lazy oder eager ist. Entscheidend ist, welche Daten ein konkreter Anwendungsfall benötigt.

```java
@Query("""
    select new com.example.order.OrderListItem(o.id, o.status, c.displayName)
    from OrderEntity o
    join o.customer c
    where o.status = :status
    """)
List<OrderListItem> findListItems(OrderStatus status);
```

Für Listen-/Read-Modelle kann eine Projection besser sein als das Laden großer Entity-Graphen.

## 3. Transaktionsgrenzen

Eine Transaktion umfasst eine fachlich oder technisch konsistente Arbeitseinheit. Sie sollte weder unnötig groß noch so klein sein, dass relevante Konsistenz verloren geht.

Typische Risiken zu großer Transaktionen:

- lange gehaltene Locks,
- größere Konfliktflächen,
- mehr Ressourcenbindung,
- schwierigeres Failure Handling.

Typische Risiken zu kleiner Transaktionen:

- verlorene atomare Fachoperationen,
- inkonsistente Zwischenzustände,
- implizite Abhängigkeit auf spätere Reparatur.

Spring-Transaktionen sind bei imperativem Code typischerweise proxy- und threadgebunden. Ein neu gestarteter Thread übernimmt eine klassische ThreadLocal-Transaktion nicht automatisch. Async- und Virtual-Thread-Design müssen diese Grenze bewusst berücksichtigen.

## 4. Optimistic und Pessimistic Locking

Optimistic Locking ist mit `@Version` ein verbreiteter Standard für konkurrierende Änderungen:

```java
@Version
private long version;
```

Ein Konflikt wird beim Update erkannt, statt Daten still zu überschreiben.

Pessimistic Locking ist sinnvoll, wenn das Geschäftsproblem tatsächlich exklusiven Datenbankzugriff während einer kurzen Transaktion erfordert. Locks über Benutzerinteraktionen oder lange externe Calls sind besonders problematisch.

## 5. Entity und API nicht gleichsetzen

JPA-Entities sind Persistenzmodelle. Sie sind nicht automatisch geeignete:

- API-DTOs,
- Events,
- Domänenobjekte,
- Cache-Werte.

Lazy Proxies, Bidirektionalität, Persistence Lifecycle und interne Felder können sonst unbeabsichtigt Teil externer Verträge werden.

## 6. Queries messen

Bei kritischen Pfaden werden mindestens betrachtet:

- Anzahl SQL-Statements,
- Latenz,
- geladene Zeilen/Spalten,
- Query Plan bei relevanten Abfragen,
- Lock-Wartezeiten,
- Connection-Pool-Auslastung.

Eine Fetchstrategie wird nicht allein anhand von Annotationen bewertet.

## 7. Normative Regeln

### MUSS

- kritische Query-Pfade werden auf N+1 und unnötige Datenmengen geprüft.
- Transaktionsgrenzen müssen die benötigte Konsistenz klar abbilden.
- konkurrierende Änderungen werden bewusst behandelt, wenn Lost Updates möglich sind.
- API- und Persistenzmodell werden nur bewusst gekoppelt.

### SOLLTE

- Read-Modelle verwenden gezielte Projections, wenn kein vollständiger Entity-Graph benötigt wird.
- Optimistic Locking wird für normale konkurrierende Updates bevorzugt, sofern das Geschäftsproblem keine exklusive Sperre verlangt.
- Integrationstests verwenden die reale Datenbank, wenn Dialekt-, Locking- oder Migrationsverhalten relevant ist.

### DARF NICHT

- `EAGER` oder `LAZY` wird nicht als universelle Organisationsregel behandelt.
- feste Batchgrößen werden nicht ohne Messung standardisiert.
- eine lange Transaktion wird nicht über Benutzerwartezeit oder langsame externe Netzaufrufe gehalten, sofern dies vermeidbar ist.

## 8. Prüffragen

1. Welche Daten benötigt der Use Case tatsächlich?
2. Wie viele SQL-Statements entstehen unter realistischen Datenmengen?
3. Wo beginnt und endet die Transaktion und warum?
4. Was passiert bei zwei konkurrierenden Änderungen?
5. Ist das Entity-Modell versehentlich Teil eines externen Vertrags?
6. Ist die gewählte Fetchstrategie gemessen oder nur vermutet?

## 9. Quellen

- Hibernate ORM Documentation: https://hibernate.org/orm/documentation/
- Hibernate ORM User Guide — Fetching: https://docs.hibernate.org/stable/orm/userguide/html_single/
- Hibernate ORM — Locking: https://docs.hibernate.org/orm/7.2/introduction/html_single/#locking
- Spring Framework — Transaction Management: https://docs.spring.io/spring-framework/reference/data-access/transaction.html

## 10. Merksatz

> ORM nimmt dir Mappingarbeit ab, nicht die Verantwortung für Queries, Locks und Transaktionen.
