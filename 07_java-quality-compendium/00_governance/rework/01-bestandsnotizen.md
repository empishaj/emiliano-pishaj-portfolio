# Bestandsnotizen zur Überarbeitung des Architecture Quality & Governance Compendium

Stand: 2026-09-30
Branch: `rework/java-quality-compendium-2026`

## Ziel

Der Bereich `07_java-quality-compendium` wird vollständig geprüft, fachlich bereinigt, sprachlich vereinheitlicht und auf eine einzige nachvollziehbare Informationsarchitektur reduziert.

## Gesamtbefund

Die Bestandsaufnahme ist abgeschlossen. Der Ordner enthielt drei parallel gewachsene Generationen:

1. historische Flat-Files `QG-JAVA-*`,
2. die thematisch sortierte `AK-*`-Struktur unter `00_governance` bis `07_learning-guides`,
3. einen zusätzlichen Arbeitsstand unter `v2/`.

Die thematische `AK-*`-Struktur ist die fachlich stärkste Basis und wird kanonisch weitergeführt. `v2/` wurde nach vollständiger Prüfung entfernt; seine relevanten Inhalte wurden in `08_engineering-guidelines` übernommen.

## Zielstruktur

Die kanonische Struktur lautet:

```text
07_java-quality-compendium/
├─ README.md
├─ 00_governance/
├─ 01_principles/
├─ 02_decision-guides/
├─ 03_standards/
├─ 04_reference-architectures/
├─ 05_operating-guides/
├─ 06_operating-models/
├─ 07_learning-guides/
└─ 08_engineering-guidelines/
```

Echte ADRs werden nicht künstlich als Portfolioartefakte erzeugt. Ein ADR benötigt realen Kontext, reale Constraints und eine echte Entscheidung. Das Compendium stellt dafür Methode und Template bereit.

## Artefaktmodell

Die Trennung zwischen folgenden Typen bleibt bestehen:

- Architecture Principle,
- Decision Guide,
- Architecture Standard,
- Policy,
- Reference Architecture,
- Engineering Guideline,
- Operating Model,
- Operating Guide / Runbook,
- Learning Guide,
- echte Architecture Decision Records.

Ein allgemeines Wissensdokument ist kein ADR. Ein ADR dokumentiert eine konkrete Entscheidung in einem konkreten Kontext.

## Schreibstil

Didaktische Selbstbezeichnungen werden entfernt. Nicht verwendet werden künftig:

- `Coach-Perspektive`,
- `Coach-Merksatz`,
- `Coach-Prüfung`,
- `Coach-Ziel`,
- ähnliche Meta-Bezeichnungen.

Verwendet werden normale Fachüberschriften wie:

- `Merksatz`,
- `Einordnung`,
- `Prüffragen`,
- `Praxis`,
- `Architekturperspektive`,
- `Entscheidungshilfe`.

## Validierungsprinzip

Besonders prüfpflichtig sind:

- Framework- und Produktversionen,
- Security-Empfehlungen,
- OAuth2/OIDC/JWT-Aussagen,
- Kubernetes- und Container-Empfehlungen,
- Eventing/Kafka,
- Performance- und Kapazitätswerte,
- Datenbankheuristiken,
- SLO-/SLA-Schwellenwerte,
- AI-/LLM-Integrationen,
- regulatorische Aussagen.

Quellenpriorität:

1. Spezifikation, RFC, Gesetz oder Norm,
2. offizielle Projekt-/Produktdokumentation,
3. etablierte Fachliteratur,
4. seriöse Sekundärquellen ergänzend.

Pauschale Zahlenwerte werden nur als Beispiel oder lokale Policy formuliert, nicht als universelle Wahrheit.

## Bereits kanonisch gut abgedeckte historische Dateien

Folgende historische Flat-Files besitzen bereits eine bessere kanonische AK-Fassung und können nach Abschluss der Linkbereinigung entfallen:

- `QG-JAVA-001` bis `005`,
- `QG-JAVA-015`,
- `QG-JAVA-017`,
- `QG-JAVA-019`,
- `QG-JAVA-021`,
- `QG-JAVA-023`,
- `QG-JAVA-025`,
- `QG-JAVA-026`,
- `QG-JAVA-031`,
- `QG-JAVA-032`,
- `QG-JAVA-034`,
- `QG-JAVA-036`,
- `QG-JAVA-037`,
- `QG-JAVA-075`,
- `QG-JAVA-106`,
- `QG-JAVA-125`,
- `QG-JAVA-126`.

## Historische Dateien mit eigener Substanz

Diese Themen werden nicht blind gelöscht, sondern kompakt neu aufgebaut:

| Legacy | Ziel | Entscheidung |
|---|---|---|
| 007 | Principles/Engineering | breite Misuse-Sammlung auf relevante Regeln verteilen |
| 008 | Learning/Principles | OOP-Kern sichern; DDD/JPA/Security-Dubletten nicht übernehmen |
| 009 | Engineering Guideline | Architekturentscheidungen im Code sichtbar/prüfbar machen |
| 010 | Engineering Guideline | JUnit-Testdesign |
| 011 | Engineering Guideline | Mockito/Test Doubles |
| 012 | Engineering Guideline | Assertions |
| 013 | Engineering Guideline | Testklassen-Struktur |
| 014 | Engineering Guideline | parametrisierte Tests |
| 016 | Engineering Guideline | JPA/Persistence Access |
| 018 | Engineering Guideline | Testcontainers |
| 020 | Engineering Guideline | Spring-Testkontext auswählen |
| 022 | Engineering Guideline | Resilience Patterns |
| 024 | Engineering Guideline | Caching |
| 027 | Learning Guide | Code Smells und Refactoring ohne starre Zahlenheuristiken |
| 028–030 | Learning Guide | drei GoF-Kataloge zu einem problemorientierten Java-Pattern-Guide zusammenführen |
| 039 + 039-01 | Engineering Guideline | eine kanonische Feature-Flag-/Runtime-Configuration-Guideline |
| 041 | Standard/Reference | Event-Driven Architecture und Kafka neu validieren und aufteilen, falls nötig |
| 049 | Engineering/Operating Guideline | Code Review und Pull Requests ohne starre PR-Zeilenzahlen |
| 072 | Operating Guide | Chaos Engineering risikobasiert und hypothesengeleitet |

## Wichtige fachliche Korrekturen aus der Altprüfung

### Virtual Threads

Historische Pinning-Aussagen sind JDK-versionsabhängig. Der kanonische Text verweist deshalb auf die konkrete JDK-Baseline und behandelt Downstream-Kapazität getrennt von Thread-Concurrency.

### JPA

Regeln wie „alle Beziehungen LAZY“, feste Batch-Größen oder eine einzige universelle Transaktionsschicht werden nicht als Dogma übernommen. Fokus bleibt auf Query-Verhalten, Transaktionsgrenzen, N+1, Locking und Messbarkeit.

### Testing

Starre Schwellenwerte zu Testklassengröße, Anzahl Tests oder CSV-Zeilen werden entfernt. Lesbarkeit, Risiko und Wartbarkeit sind die Kriterien.

### Flyway / PostgreSQL

Starre Zeilenanzahlen für „große Tabellen“, pauschales `CONCURRENTLY`, pauschale Backfill-Grenzen und ähnliche Heuristiken werden nicht als Standard formuliert. Erhalten bleiben Expand/Contract, Lock-Bewusstsein, Forward-Fix, getrennte DB-Rechte und produktionsnahe Tests.

### Feature Flags

Die beiden historischen `039`-Dateien überschneiden sich stark. Sie werden zu einem Dokument zusammengeführt. Statische Startkonfiguration, dynamische Flags, Experimente und Berechtigungslogik werden klar getrennt.

### Kafka / EDA

Der historische `041` ist fachlich reich, aber zu monolithisch und an mehreren Stellen zu absolut. Zu prüfen beziehungsweise zu korrigieren sind insbesondere:

- konkrete Spring-/Kafka-/Schema-Registry-Versionen,
- pauschale Avro-Vorgaben,
- starre Topic-Naming-Konventionen,
- starre Idempotenz-/Retention-Werte,
- die Behauptung, Choreography sei grundsätzlich besser als Orchestration.

### Code Review

Feste PR-Zeilengrenzen sind Heuristiken, keine Qualitätsgesetze. Reviewbarkeit, Kohärenz und Risikoumfang sind entscheidend.

### Chaos Engineering

Steady State, Hypothese, Blast Radius, Stopbedingungen und Observability bleiben Kern. Beispielwerte wie Error Rate oder Latenz werden nicht als universelle Standardwerte geführt.

## Fehlende Quelle

`QG-JAVA-006` wird in mehreren historischen Dateien referenziert, liegt aber nicht im Repository. Das Thema wird nicht aus Querverweisen rekonstruiert oder erfunden. Verweise werden im Zuge der Konsolidierung entfernt oder auf vorhandene kanonische Artefakte umgestellt.

## Löschregel

Eine Datei wird gelöscht, wenn:

- ihr relevanter Inhalt in einer besseren kanonischen Fassung enthalten ist,
- kein eigenständiger Zweck verbleibt,
- Querverweise angepasst wurden,
- kein fachlich relevanter Inhalt verloren geht.

Git bleibt die Historie. Redundante aktive Dateien müssen nicht zusätzlich dauerhaft im Repository liegen.
