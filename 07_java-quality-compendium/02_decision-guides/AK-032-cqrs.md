---
id: AK-032
legacy_ids:
  - QG-JAVA-032
title: CQRS – Read- und Write-Modelle bewusst trennen
artifact_type: decision-guide
domain: application-architecture
status: active
maturity: reviewed
normative_level: informative
last_validated: 2026-10-01
review_trigger:
  - wesentliche Änderung von Read-/Write-Anforderungen
---

# AK-032 — CQRS ohne unnötige Verteilung

## 1. Kernidee

CQRS bedeutet zunächst:

> Die Modelle beziehungsweise Verantwortungen für Commands und Queries können getrennt werden, wenn sie unterschiedliche Anforderungen besitzen.

CQRS bedeutet **nicht automatisch**:

- zwei Datenbanken,
- Event Sourcing,
- Kafka,
- Microservices,
- eventual consistency.

Die Trennung kann rein logisch innerhalb einer Anwendung beginnen.

## 2. Wann CQRS helfen kann

- Write-Modell enthält komplexe Invarianten,
- Read-Seite braucht stark andere Projektionen,
- sehr unterschiedliche Skalierungsprofile,
- Reporting/Suche würden Domain-Modell verbiegen,
- unterschiedliche Security-/Datenzugriffssichten.

## 3. Wann nicht

Bei einfachem CRUD mit nahezu identischem Read-/Write-Modell erzeugt CQRS häufig nur zusätzliche Klassen und Synchronisationskomplexität.

## 4. Reifestufen

### Level 1 — logische Trennung

```text
Command Service
Query Service
gleiche Datenbank
```

Oft ausreichend.

### Level 2 — getrennte Read-Projektion

```text
Write Model
→ Change/Event
→ Read Model
```

Jetzt entsteht Synchronisations- und Aktualitätssemantik.

### Level 3 — verteilte Modelle

Separate Services/Datenspeicher und gegebenenfalls Event Streaming.

Nur sinnvoll, wenn Skalierung, Ownership oder Unabhängigkeit dies rechtfertigen.

## 5. Read Replica ist nicht automatisch CQRS

Eine PostgreSQL Read Replica ist primär eine Datenbank-Replikations-/Skalierungstechnik.

Sie kann in einer Architektur mit getrennten Read-/Write-Pfaden genutzt werden, macht das System aber nicht allein zu CQRS.

CQRS beschreibt die Verantwortungs- und Modelltrennung auf Anwendungsebene; physische Replikation ist eine Infrastrukturentscheidung.

## 6. Eventual Consistency

Wenn Read Model asynchron aktualisiert wird, muss fachlich beantwortet werden:

- Wie alt darf die Sicht sein?
- Was sieht ein Nutzer direkt nach einem Write?
- wie werden Fehler/Replays behandelt?
- wie wird Read Model neu aufgebaut?
- wie erkennen wir Lag?

„Eventually consistent“ ohne Zeit-/Prozesssemantik ist unvollständig.

## 7. Read Models

Ein Read Model darf den Consumerbedarf optimieren.

Beispiele:

- denormalisierte SQL-Sicht,
- Elasticsearch Index,
- Materialized View,
- Cache,
- Event-Projektion.

Die führende Datenhoheit bleibt explizit getrennt.

## 8. Commands

Commands beschreiben Änderungsabsicht:

```text
ApproveCase
CancelOrder
RegisterDocument
```

Sie sollten nicht nur generische Setter auf ein Datenmodell sein, wenn fachliche Regeln existieren.

## 9. Evidence

Vor Einführung verteilter CQRS-Strukturen sollten messbar oder belegbar sein:

- Read-/Write-Lastprofil,
- Query-Komplexität,
- Aktualitätsanforderung,
- Rebuild-Fähigkeit,
- Betriebsreife,
- Ownership.

## 10. Anti-Patterns

- CQRS für triviales CRUD.
- jeder Command hat eigenen Microservice.
- Read Model wird unbemerkt zweites System of Record.
- keine Rebuild-Strategie für Projektionen.
- Eventual Consistency wird dem Fachbereich erst nach Go-Live erklärt.

## 11. Beziehungen

- AK-055 — Event Sourcing
- AK-104 — Search Read Model
- AK-113 — Read Replicas

## 12. Merksatz

> CQRS ist keine Infrastrukturmode. Es lohnt sich, wenn **Lesen und Schreiben tatsächlich unterschiedliche Modelle, Lastprofile oder Verantwortungen benötigen**.
