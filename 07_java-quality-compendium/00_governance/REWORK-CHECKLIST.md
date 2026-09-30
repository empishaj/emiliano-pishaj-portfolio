# Bearbeitungscheckliste

Stand: 2026-09-30

Legende:

- `[x]` abgeschlossen
- `[-]` in Arbeit
- `[ ]` offen
- `[!]` blockiert / Quelle fehlt

## AP0 — Bestand

- [x] Arbeitsbranch `rework/java-quality-compendium-2026-09` angelegt
- [x] aktuelle Root-Struktur erfasst
- [x] `00_governance` geprüft
- [x] `01_principles` geprüft
- [x] `02_decision-guides` geprüft
- [x] `03_standards` geprüft
- [x] `04_reference-architectures` geprüft
- [x] `05_operating-guides` geprüft
- [x] `06_operating-models` geprüft
- [x] `07_learning-guides` geprüft
- [x] `v2` und die darin enthaltenen Java-Grundlagen geprüft
- [x] Legacy-Inhalte aus der bisherigen Gesamtdurchsicht dem Migrationskatalog zugeordnet
- [!] `QG-JAVA-006` Originalquelle fehlt
- [!] `121–124` ohne Quelle; bleiben reserviert

## AP1 — Governance konsolidieren

- [-] `ARTIFACT-MODEL.md` als führendes Modell festlegen
- [ ] `ADR-LIFECYCLE-AND-TEMPLATE.md` sprachlich und strukturell vereinfachen
- [ ] `VALIDATION-POLICY.md` sprachlich normalisieren
- [ ] `MIGRATION-CATALOG.md` zu nüchternem Statusregister überarbeiten
- [ ] `AK-082` final prüfen
- [ ] `v2/00-master-konzept.md` relevante Inhalte übernehmen
- [ ] `v2/01-katalog-und-migrationsplan.md` mit Hauptkatalog abgleichen
- [ ] redundante `v2`-Governance-Dateien löschen

## AP2 — Struktur und Navigation

- [ ] Root-`README.md` erstellen
- [ ] `05_engineering-guidelines` erstellen
- [ ] `05_operating-guides` nach `07_operating-guides` verschieben
- [ ] `07_learning-guides` nach `08_learning-guides` verschieben
- [ ] Legacy-Rootdateien nach `99_legacy-source` verschieben
- [ ] Root von konkurrierenden aktiven Quellen bereinigen

## AP3 — Sprache

- [ ] `Coach-Merksatz` → `Merksatz`
- [ ] `Coach-Frage` → `Prüffrage`
- [ ] `Coach-Ziel` → `Ziel`
- [ ] `Coach-Perspektive` → fachlich passende Überschrift
- [ ] `Coach-Master-Ton` entfernen
- [ ] unnötige Meta-/Selbstbeschreibung reduzieren
- [ ] aktiven Bestand nach `Coach` prüfen
- [ ] aktiven Bestand nach `Guru-Level` und ähnlichen Selbsteinstufungen prüfen

## AP4 — Engineering Guidelines

### Java Sprache

- [ ] 001 Records übernehmen
- [ ] 002 Sealed Types übernehmen
- [ ] 003 Pattern Matching übernehmen
- [ ] 004 Virtual-Thread-Doppelung auflösen
- [ ] 005 Text Blocks übernehmen
- [!] 006 Serviceschicht – Quelle fehlt
- [ ] 007 Java-Feature-Missbrauch migrieren
- [ ] 008 OOP als Learning/Engineering Guide migrieren
- [ ] 009 Traceability mit Governance-Struktur abgleichen

### Testing

- [ ] 010 JUnit
- [ ] 011 Mockito
- [ ] 012 AssertJ
- [ ] 013 Testklassendesign
- [ ] 014 parametrisierte Tests
- [ ] 018 Testcontainers
- [ ] 020 Spring Test Slices
- [ ] 044 Performance Testing
- [ ] 045 Mutation Testing
- [ ] 046 Property-Based Testing
- [ ] 096 Teststrategie als übergeordneten Standard verlinken
- [ ] 119 Test Data Management

### Design / Code

- [ ] 025 SOLID Stil finalisieren
- [ ] 026 KISS/DRY/YAGNI Stil finalisieren
- [ ] 027 Refactoring/Code Smells
- [ ] 028 Creational Patterns
- [ ] 029 Structural Patterns
- [ ] 030 Behavioral Patterns
- [ ] 033 Concurrency
- [ ] 048 JavaDoc
- [ ] 052 Immutability Stil finalisieren
- [ ] 073 Indexing
- [ ] 099 MapStruct
- [ ] 109 Wicket Environment Guide

## AP5 — Application / Domain / Integration

- [ ] 021 REST Standard finalisieren
- [ ] 023 DDD finalisieren
- [ ] 031 Hexagonal finalisieren
- [ ] 032 CQRS finalisieren
- [ ] 039 Feature Flags
- [ ] 039-01 statische Feature Flags
- [ ] 041 EDA/Kafka
- [ ] 042 Transactional Outbox
- [ ] 053 GraphQL
- [ ] 055 Event Sourcing
- [ ] 056 Modulith Reference Architecture finalisieren
- [ ] 065 Saga
- [ ] 066 API First finalisieren
- [ ] 067 gRPC
- [ ] 077 Modulith vs Microservices finalisieren
- [ ] 079 Modulith Strategy finalisieren
- [ ] 089 Strategic DDD finalisieren
- [ ] 095 AsyncAPI finalisieren
- [ ] 097 DLQ
- [ ] 105 WebSocket/SSE
- [ ] 110 API Lifecycle finalisieren
- [ ] 111 GraphQL Federation

## AP6 — Security / IAM / Privacy

- [ ] 015 Application Security finalisieren
- [ ] 035 Privacy by Design
- [ ] 040 OAuth/OIDC finalisieren
- [ ] 057 Supply Chain finalisieren
- [ ] 069 Configuration/Secrets finalisieren
- [ ] 070 Zero Trust/Service Mesh
- [ ] 101 Spring Security finalisieren
- [ ] 106 Privacy Controls finalisieren
- [ ] 118 Browser Security finalisieren
- [ ] 125 Input Validation finalisieren

## AP7 — Data Architecture

- [ ] 016 JPA / Transactions
- [ ] 024 Caching
- [ ] 034 DB Migration finalisieren
- [ ] 043 JVM/Hikari – DB-Pool-Anteil sauber abgrenzen
- [ ] 058 Multi-Tenancy
- [ ] 063 Backup/DR
- [ ] 073 Indexing
- [ ] 078 Data Strategy
- [ ] 098 Zero-Downtime Migration
- [ ] 104 Elasticsearch
- [ ] 112 Partitioning/Sharding
- [ ] 113 Read Replicas

## AP8 — Platform / Delivery / Operations

- [ ] 017 Observability finalisieren
- [ ] 036 CI/CD finalisieren
- [ ] 037 Container Images finalisieren
- [ ] 038 Kubernetes
- [ ] 043 JVM Tuning/Profiling-Bezug
- [ ] 050 Multi-Module Gradle
- [ ] 054 SLO/Alerting/On-Call
- [ ] 059 Blue/Green/Canary
- [ ] 060 IaC finalisieren
- [ ] 061 Fitness Functions finalisieren
- [ ] 062 Rate Limiting finalisieren
- [ ] 068 Batch
- [ ] 071 GraalVM
- [ ] 072 Chaos Engineering
- [ ] 074 Scheduled Jobs
- [ ] 080 DevSecOps finalisieren
- [ ] 091 Reactive Architecture
- [ ] 092 Cloud Native
- [ ] 100 Helm/Kustomize
- [ ] 102 OpenTelemetry finalisieren
- [ ] 103 Kafka Streams
- [ ] 108 Golden Path / Developer Experience
- [ ] 114 GitOps finalisieren
- [ ] 116 Distributed Locking
- [ ] 117 JFR/Async Profiler
- [ ] 120 Incident/Post-Mortem
- [ ] 126 Docker Debian Guide finalisieren

## AP9 — Enterprise / Governance / Learning

- [ ] 047 C4-Dokumentation mit AK-088 zusammenführen/abgrenzen
- [ ] 049 Code Review Operating Model
- [ ] 051 DoD/Tech Debt
- [ ] 064 Versioning finalisieren
- [ ] 075 Decision Process sprachlich finalisieren
- [ ] 076 Technology Radar
- [ ] 081 Architecture Fundamentals finalisieren
- [ ] 082 Quality Goals finalisieren
- [ ] 083 Architecture Patterns finalisieren
- [ ] 084 Coupling/Cohesion finalisieren
- [ ] 085 Socio-technical Architecture finalisieren
- [ ] 086 Cross-cutting Framework prüfen, ob eigenständiger Wert bleibt
- [ ] 087 Architecture Evaluation finalisieren
- [ ] 088 Documentation Standard finalisieren
- [ ] 090 Evolutionary Architecture finalisieren
- [ ] 093 Living Documentation finalisieren
- [ ] 094 Synthesis finalisieren
- [ ] 107 FinOps
- [ ] 115 AI Architecture / Spring AI

## AP10 — Abschluss

- [ ] alle Legacy-Dateien haben Status `migriert`, `zusammengeführt` oder `verworfen`
- [ ] ersetzte Legacy-Dateien löschen
- [ ] `v2` vollständig entfernen
- [ ] Cross-References prüfen
- [ ] Link- und Quellenprüfung
- [ ] aktuelle Technologie-Baselines prüfen
- [ ] keine unerwünschten `Coach`-Treffer im aktiven Bestand
- [ ] keine unbelegten Universalgrenzwerte
- [ ] keine erfundenen Rollen, Boards oder Projekterfolge
- [ ] README/NAVIGATION final
- [ ] Branch-Diff prüfen
- [ ] Pull Request erstellen
- [ ] nach Review in `main` übernehmen
