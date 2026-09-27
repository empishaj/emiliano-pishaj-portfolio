---
id: AK-034
legacy_ids:
  - QG-JAVA-034
title: Datenbankmigrationen und Schema Evolution
artifact_type: architecture-standard
domain: data-delivery
status: active
maturity: reviewed
normative_level: normative
owner_role: Data / Application Architecture
last_validated: 2026-09-28
review_trigger:
  - Wechsel des Migrationstools oder DBMS
  - schwerwiegender Migrationsincident
---

# AK-034 — Datenbankmigrationen und Schema Evolution

## 1. Zweck

Datenbankschemata sind produktiver Bestandteil einer Anwendung und müssen denselben Anforderungen an Versionierung, Review, Reproduzierbarkeit und Recovery genügen wie Code.

Dieser Standard verwendet Flyway als verbreitetes Beispiel. Die Architekturregeln sind jedoch toolunabhängig.

## 2. Grundregeln

MUSS:

- Schemaänderungen versionieren,
- Änderungen über denselben kontrollierten Delivery-Prozess wie Anwendungscode reviewen,
- Produktionsänderungen reproduzierbar machen,
- Migrationen vor produktiver Ausführung testen,
- Backup/Recovery und Locking-Auswirkungen berücksichtigen.

DARF NICHT:

- produktive Schemata manuell undokumentiert verändern,
- bereits produktiv angewendete unveränderliche Migrationen nachträglich still umschreiben.

## 3. Forward-only vs. Rollback

Ein SQL-`down`-Script ist nicht automatisch ein sicherer Rollback.

Beispiel:

```text
ALTER TABLE DROP COLUMN
```

kann Daten unwiederbringlich entfernen.

Darum wird vor riskanten Migrationen geklärt:

- kann die Anwendung auf alte Version zurückrollen?
- ist Schema rückwärtskompatibel?
- braucht es Backup/Restore statt Down Migration?
- welche Datenmigration ist reversibel?

## 4. Expand / Migrate / Contract

Für Breaking Changes ist ein schrittweises Muster zu bevorzugen:

### Expand

Neue Struktur hinzufügen, alte weiterhin unterstützen.

### Migrate

Daten und Traffic schrittweise umstellen.

### Contract

Alte Struktur erst entfernen, wenn keine aktive Nutzung mehr existiert.

Damit werden Application Deployment und DB Change zeitlich entkoppelt.

## 5. Migrationsrisiken

Vor Review mindestens prüfen:

- Table-/Row Locks,
- Index Creation,
- Rewrite großer Tabellen,
- Constraint Validation,
- Datenvolumen,
- Laufzeit,
- Replication Lag,
- Backup-/Recovery-Auswirkung,
- gleichzeitige alte/neue App-Versionen.

Ein `ALTER TABLE` ist nicht deshalb harmlos, weil das SQL kurz ist.

## 6. Datenmigration getrennt denken

Schemaänderung und fachliche Datenmigration können unterschiedliche Laufzeit-/Recovery-Eigenschaften haben.

Große Backfills SOLLEN:

- resumable sein,
- progress messbar machen,
- in Batches laufen,
- Produktion nicht unkontrolliert saturieren,
- Fehlerzustand und Wiederanlauf besitzen.

## 7. Migration Ownership

Das Team, das das Schema fachlich besitzt, ist auch für die Migrationslogik verantwortlich.

DBA-/Platform-Spezialisten können Standards, Beratung und Reviews bereitstellen, sollten aber nicht automatisch jeden kleinen Change zu einem zentralen Ticket-Bottleneck machen.

Bei hochkritischen Änderungen kann ein zusätzlicher Review verpflichtend sein.

## 8. Tool-Baseline

Flyway oder andere Tools verwalten technische Ausführung und Historie.

Sie entscheiden nicht:

- ob eine Migration fachlich sinnvoll ist,
- ob sie zero-downtime-fähig ist,
- ob Daten gelöscht werden dürfen,
- ob ein Deployment zurückrollbar ist.

Tool und Architekturentscheidung sind getrennt.

## 9. Tests und Evidence

MUSS/SOLLTE je nach Risiko:

- Migration auf leerer DB,
- Migration aus realistischer Vorversion,
- Integrationstest mit Anwendung,
- Laufzeitmessung bei großen Tabellen,
- Lock-/Query-Plan-Analyse,
- Rollback-/Recovery-Plan,
- Smoke Test nach Migration.

## 10. Anti-Patterns

- `ddl-auto=update` als Produktions-Migrationsstrategie.
- produktive Migration nachträglich ändern.
- App benötigt neues Schema sofort, obwohl alte Pods noch laufen.
- großer Backfill im gleichen blocking Transaction Step.
- „Rollback“ behaupten, obwohl Daten bereits zerstört wurden.

## 11. Quellen

- Flyway Documentation  
  https://documentation.red-gate.com/flyway
- PostgreSQL Documentation — DDL/Locking je verwendeter Version
- AK-098 — Zero-Downtime Migration Pattern
- AK-063 — Backup & Recovery

## 12. Coach-Merksatz

> Eine Datenbankmigration ist kein SQL-File. Sie ist eine **kontrollierte Zustandsänderung eines produktiven Informationsbestands mit Locking-, Compatibility- und Recovery-Folgen**.
