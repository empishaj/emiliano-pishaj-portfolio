# Migrationskatalog: Legacy-Themen → Architecture Knowledge System

## Zweck

Dieser Katalog ordnet jeden bisher belegten nummerierten Inhalt einem **fachlich passenden Ziel-Artefakt** zu. Er ist kein bloßer Rename-Plan. Er beantwortet für jedes Thema:

1. Was ist das Thema wirklich?
2. Welcher Artefakttyp passt?
3. Auf welcher Architekturflughöhe liegt es?
4. Wie stark muss es vor einer Neuveröffentlichung validiert werden?
5. Was ist beim Rewrite der zentrale Coaching- und Architekturgedanke?

Die historischen Nummern bleiben als `legacy_id` erhalten. Die Nummer sagt künftig **nicht mehr**, dass alle Dokumente dieselbe Artefaktart sind.

## Validierungsstatus

- **A** – fachlich stabil; primär konzeptionell validieren.
- **B** – fachlich tragfähig; Technologie-/API-/Versionsbaseline aktualisieren.
- **C** – substanzielle Überarbeitung nötig; mindestens eine Aussage ist zu absolut, veraltet oder strukturell falsch.
- **D** – Quelle fehlt; nicht rekonstruieren oder erfinden.

## Rewrite-Reihenfolge

Die Migration erfolgt bewusst **nicht numerisch**, sondern nach Abhängigkeiten.

### Welle 0 – Governance-Fundament

Zuerst werden die Regeln gebaut, nach denen alle anderen Dokumente bewertet werden:

`075 → 081 → 082 → 087 → 088 → 093 → 094`

Ergebnis: Decision Lifecycle, Qualitätsziele, Bewertungsmethodik, Dokumentationsmodell und Learning-/Evidence-Regeln.

### Welle 1 – langlebige Prinzipien

`025 → 026 → 052 → 084 → 085 → 090`

Ergebnis: Design-, Organisations- und Evolutionsprinzipien ohne Toolabhängigkeit.

### Welle 2 – normative Standards

Security, Integration, Delivery, Data und Observability werden harmonisiert:

`015 → 017 → 021 → 034 → 036 → 037 → 040 → 057 → 060 → 061 → 062 → 064 → 066 → 069 → 086 → 095 → 096 → 101 → 102 → 106 → 110 → 114 → 118 → 125 → 126`

### Welle 3 – Decision Guides und Reference Architectures

Kontextabhängige Technologie- und Strukturentscheidungen:

`004 → 016 → 022 → 023 → 024 → 031 → 032 → 038 → 041 → 042 → 053 → 055 → 056 → 058 → 059 → 065 → 067 → 068 → 070 → 071 → 077 → 079 → 080 → 083 → 089 → 091 → 092 → 098 → 100 → 103 → 104 → 105 → 108 → 111 → 112 → 113 → 115 → 116`

### Welle 4 – Engineering Guidelines

Code-, Test-, Build- und Implementierungspraktiken werden an die Standards angehängt, statt selbst Architekturpolitik zu spielen.

### Welle 5 – Operating Models

`049 → 051 → 054 → 063 → 072 → 076 → 107 → 117 → 120`

Ergebnis: Reviews, SLO/On-Call, Continuity, Technology Lifecycle, Kosten, Performance-Diagnose und Incident Learning.

## Katalog

| Legacy-ID | Thema | Ziel-Artefakt | Domäne | V | Neubau-Konzept |
|---|---|---|---|:---:|---|
| 001 | Java Records for DTOs | Engineering Guideline | Java & Code | B | Records als semantisches Werkzeug für unveränderliche Datenträger; Einsatzgrenzen für JPA, Domäne, mutable Daten und sensible Werte. |
| 002 | Sealed Classes für geschlossene Domänentypen | Engineering Guideline | Java & Code | B | Geschlossene Typhierarchien für fachlich endliche Varianten; Regeln für Evolution, Serialisierung und Pattern Matching. |
| 003 | Pattern Matching und Switch Expressions | Engineering Guideline | Java & Code | B | Lesbare Typ- und Zustandsverzweigungen; Grenzen gegenüber Polymorphie und komplexer Geschäftslogik. |
| 004 | Virtual Threads | Decision Guide | Runtime & Concurrency | B | Entscheidungshilfe für I/O-lastige Nebenläufigkeit; Pinning, Backpressure, Ressourcenlimits und Messung statt pauschaler Einführung. |
| 005 | Text Blocks | Engineering Guideline | Java & Code | A | Lesbare mehrzeilige Literale; Grenzen für SQL, JSON, Templates und dynamische Inhalte. |
| 006 | Spring-Boot-Serviceschicht | Reference Architecture | Application Architecture | C | Als fehlende Quelle rekonstruieren, nicht erfinden; Ziel: Servicegrenzen, Transaktionen, Tenant-Kontext, Fehler- und DTO-Grenzen. |
| 007 | Code-Missbrauch moderner Java-Features | Engineering Guideline | Java & Code | A | Anti-Patterns: Feature-Nutzung nur bei semantischem Nutzen; Beispiele gegen Lesbarkeit, Wartbarkeit und Debuggability prüfen. |
| 008 | Objektorientierung richtig anwenden | Learning Guide | Software Design | B | OOP als Kapselung von Invarianten und Verhalten statt Getter/Setter-Datencontainer; Beispiele modernisieren. |
| 009 | Architekturentscheidungen im Code sichtbar machen | Architecture Standard | Governance & Traceability | B | Traceability zwischen Entscheidung, Code, Tests und Fitness Functions; keine künstlichen Annotationen ohne Nutzen erzwingen. |
| 010 | JUnit 5 Grundlagen | Engineering Guideline | Testing | B | Teststruktur, Benennung, Isolation; APIs gegen aktuelle JUnit-Baseline prüfen. |
| 011 | Mockito richtig einsetzen | Engineering Guideline | Testing | B | Mocks nur an geeigneten Kollaborationsgrenzen; Interaktions- vs. Zustandstests sauber trennen. |
| 012 | AssertJ Assertions | Engineering Guideline | Testing | B | Aussagekräftige Assertions und Fehlerdiagnose; aktuelle API prüfen. |
| 013 | Testklassendesign | Engineering Guideline | Testing | A | Isolation, Nested-Struktur, Lesbarkeit und deterministische Tests. |
| 014 | Parametrisierte Tests | Engineering Guideline | Testing | B | Varianten ohne Duplikation; Entscheidungskriterien gegenüber Property-Based Testing ergänzen. |
| 015 | OWASP, Input Validation & Spring Security | Security Standard | Security | C | In mehrere Controls schneiden: Input, AuthN/AuthZ, Fehler, Secrets; OWASP/Framework-Aussagen aktualisieren. |
| 016 | JPA: N+1, Fetch, Transaktionen | Decision Guide | Data Access | B | Persistence-Trade-offs, Transaktionsgrenzen und Query-Strategien; Provider-spezifische Aussagen kennzeichnen. |
| 017 | Observability: Logging, Metrics, Tracing | Architecture Standard | Operations | B | Mindeststandard für Telemetrie und Korrelation; mit OTel/SLO/PII-Regeln harmonisieren. |
| 018 | Integrationstests mit Testcontainers | Engineering Guideline | Testing | B | Wann reale Abhängigkeiten im Test sinnvoll sind; Version/API aktualisieren. |
| 019 | Contract Testing | Architecture Standard | Integration & Testing | B | Consumer-/Provider-Verträge, Sync und Async, CI-Gates, Ownership; Tools austauschbar halten. |
| 020 | SpringBootTest und Slice Tests | Engineering Guideline | Testing | B | Testkontext nach Risiko und Geschwindigkeit wählen; aktuelle Spring-Boot-Test-APIs prüfen. |
| 021 | REST API Design & Versionierung | Architecture Standard | Integration | B | Ressourcenmodell, HTTP-Semantik, Fehlerformat, Versionierung, Pagination, Security und Lifecycle. |
| 022 | Resilience: Circuit Breaker, Retry, Timeout, Bulkhead | Decision Guide | Resilience | B | Fehlerarten und Schutzmechanismen unterscheiden; Defaults nicht universell festschreiben. |
| 023 | DDD: Aggregate, Bounded Context, Ubiquitous Language | Decision Guide | Domain Architecture | A | DDD nur bei relevanter Domänenkomplexität; taktisch vs. strategisch sauber trennen. |
| 024 | Caching Strategie | Decision Guide | Performance | B | Cache nur mit Datenalter, Invalidierung, Konsistenz und Failure Mode entscheiden. |
| 025 | SOLID | Engineering Principle | Software Design | A | Prinzipien problemorientiert, nicht dogmatisch; moderne Java-Beispiele. |
| 026 | KISS, DRY, YAGNI, Law of Demeter | Engineering Principle | Software Design | A | Prinzipien als Spannungsfeld erklären; DRY nicht gegen fachliche Entkopplung missbrauchen. |
| 027 | Code Smells & Refactoring | Engineering Guideline | Software Design | A | Diagnose statt Regelkatalog; Refactoring nur mit beobachtbarem Qualitätsziel. |
| 028 | Creational Patterns | Learning Guide | Software Design | A | Pattern als Option, nicht Ziel; Problem, Kräfte, Trade-offs und Java-Alternativen. |
| 029 | Structural Patterns | Learning Guide | Software Design | A | Pattern problemorientiert; Framework- und Sprachfeatures als Alternativen berücksichtigen. |
| 030 | Behavioral Patterns | Learning Guide | Software Design | A | Pattern problemorientiert; Komplexitätskosten sichtbar machen. |
| 031 | Hexagonal Architecture | Decision Guide | Application Architecture | A | Ports/Adapter als Abhängigkeitsprinzip; nicht jede Anwendung maximal schichten. |
| 032 | CQRS | Decision Guide | Application Architecture | B | Logische Read/Write-Trennung von physischer Replikation abgrenzen; Eventual Consistency explizit. |
| 033 | Thread Safety, Deadlocks, Race Conditions | Engineering Guideline | Runtime & Concurrency | B | Immutability, Ownership, atomare Operationen und Diagnose; Virtual Threads lösen keine Datenrennen. |
| 034 | Flyway Datenbankmigrationen | Architecture Standard | Data & Delivery | B | Expand/Contract, Locking, Rollback-Strategie, Ownership; Flyway-Versionen aus Kernregel entfernen. |
| 035 | GDPR / Privacy by Design | Architecture Principle | Privacy | C | Rechts- und Architekturprinzip von technischen Masking-Regeln trennen; Behörden-/EU-Kontext aktuell validieren. |
| 036 | CI/CD Pipeline | Reference Architecture | Delivery | B | Pipeline-Stufen als Referenz; konkrete Tools austauschbar, Evidence und Promotion verbindlich. |
| 037 | Docker sicher, klein, reproduzierbar | Architecture Standard | Platform | B | Image-Härtung, Non-root, Secrets, SBOM, Patching, Reproducibility; aktuelle Docker-Baseline prüfen. |
| 038 | Kubernetes Architektur | Reference Architecture | Platform | B | Workload-, Security-, Resource-, Network-, Storage- und Ops-Leitplanken; keine Cluster-Admin-Anleitung. |
| 039 | Feature Flags | Decision Guide | Delivery | B | Deployment/Release trennen; Lifecycle, Ownership, Cleanup, Security und Audit. |
| 39-01 | Statische Feature Flags über application.yml | Engineering Guideline | Delivery | B | Begrenzter Spezialfall; klar gegen dynamische Flags und Konfigurationsmanagement abgrenzen. |
| 040 | OAuth2, OIDC & JWT | Decision Guide | IAM & Security | C | OAuth2 nicht mit Authentifizierung gleichsetzen; JWT ist nicht zwingendes Access-Token-Format; aktuelle BCPs nutzen. |
| 041 | Event-Driven Architecture & Kafka | Decision Guide | Integration | B | EDA-Treiber, Event-Verträge, Idempotenz, Ordering, Delivery-Semantik und Betrieb; Kafka als Option, nicht Ziel. |
| 042 | Transactional Outbox | Decision Guide | Integration | A | Dual-Write-Problem, Outbox-Varianten, Idempotenz und CDC; keine Exactly-once-Versprechen. |
| 043 | JVM Tuning & HikariCP | Operating Guide | Runtime & Performance | B | Messbasierte Tuning-Methodik; keine universellen Pool- oder Heap-Werte. |
| 044 | Load & Performance Testing | Quality Standard | Testing & Performance | B | Lastmodell aus SLO/Workload ableiten; P50/P95/P99, Saturation und reproduzierbare Testumgebung. |
| 045 | Mutation Testing mit PIT | Engineering Guideline | Testing | B | Mutation als Testwirksamkeits-Signal; Schwellenwerte risikobasiert statt universell. |
| 046 | Property-Based Testing | Engineering Guideline | Testing | B | Invarianten und Generatoren; sinnvolle Domänen statt wahlloser Zufallsdaten. |
| 047 | C4 Architekturdokumentation | Documentation Standard | Architecture Documentation | B | C4 für Software-Sichten; Beziehung zu arc42/ArchiMate/ADRs klar definieren. |
| 048 | JavaDoc Standards | Engineering Guideline | Documentation | B | Verträge, Semantik, Preconditions; offensichtliche Implementierung nicht dokumentieren. |
| 049 | Code Review & Pull Request | Operating Model | Engineering Governance | B | Reviewziele, Rollen, Risikostufen, Automatisierung und Eskalation; keine starre One-size-fits-all-Regel. |
| 050 | Multi-Module Gradle | Reference Architecture | Build & Modularity | B | Buildmodule als Architekturgrenzen; ArchUnit/Fitness Functions; Gradle-APIs aktuell prüfen. |
| 051 | Definition of Done & Technical Debt | Operating Model | Engineering Governance | C | DoD als organisationsspezifischer Standard; fixe Kapazitätsquoten entfernen, Debt risikobasiert steuern. |
| 052 | Immutability & Defensive Programming | Engineering Principle | Software Design | A | Invarianten, defensive Kopien, Seiteneffekte und Concurrency-Vorteile. |
| 053 | GraphQL | Decision Guide | Integration | B | REST/GraphQL anhand Consumer-Bedarf, Cache, Security, Complexity und Ownership vergleichen. |
| 054 | SLO/SLA, Alerting & On-Call | Operating Model | Operations | B | SLI/SLO/SLA, Error Budgets, Alertqualität, Ownership und Runbooks; Ziele kontextabhängig. |
| 055 | Event Sourcing | Decision Guide | Data & Domain | A | Audit/Temporalität vs. Komplexität; Event-Evolution, Projektionen und Recovery explizit. |
| 056 | Modulith | Reference Architecture | Application Architecture | B | Modularer Monolith als Ziel-/Übergangsmuster; Grenzen technisch prüfbar machen. |
| 057 | SBOM & Dependency Auditing | Security Standard | Supply Chain Security | B | SBOM, SCA, Provenance, Signierung, Lizenz- und Vulnerability-Prozess; Formate/Tools aktualisieren. |
| 058 | Multi-Tenancy | Decision Guide | Data & Security | B | Isolationsebenen, Schutzbedarf, Kosten, Lifecycle und Testbarkeit bewerten. |
| 059 | Blue/Green & Canary | Decision Guide | Delivery | B | Release-Strategien an Risiko, Datenmigration, Observability und Rollback koppeln. |
| 060 | Infrastructure as Code | Architecture Standard | Platform & Delivery | B | Deklarativ, versioniert, reviewbar, reproduzierbar; State, Secrets, Drift und Rechte berücksichtigen. |
| 061 | Architecture Fitness Functions | Governance Standard | Architecture Governance | A | Architektureigenschaften in ausführbare Checks überführen; nur messbare/automatisierbare Regeln automatisieren. |
| 062 | Rate Limiting | Architecture Standard | Security & Resilience | B | Schutzmodell, Fairness, Limits nach Consumer/Operation, 429-Verhalten und Distributed State. |
| 063 | Backup & Disaster Recovery | Operating Model | Resilience & Continuity | B | RPO/RTO, Backup, Restore-Test, Verantwortlichkeiten, Notfallübungen und Evidenz. |
| 064 | Changelog & Semantic Versioning | Architecture Standard | Lifecycle | B | Änderungsnachvollziehbarkeit; SemVer nur wo Versionierungsmodell passt, laufende Version sichtbar. |
| 065 | Saga | Decision Guide | Integration | A | Orchestration vs. Choreography, Kompensation, Persistenz, Idempotenz, Observability. |
| 066 | API First & OpenAPI | Architecture Standard | Integration | B | Contract-first, Review, Linting, Compatibility und Codegen; OpenAPI-Baseline aktuell halten. |
| 067 | gRPC | Decision Guide | Integration | B | gRPC vs REST/Event anhand Latenz, Streaming, Interop, Debuggability und Consumer bewerten. |
| 068 | Spring Batch | Decision Guide | Processing | B | Batch als Processing Model; Restartability, Chunking, Idempotenz, Scheduling und Observability. |
| 069 | Configuration Management | Architecture Standard | Platform & Security | B | Code, Config, Secret trennen; Hierarchie, Rotation, Audit und sichere Defaults. |
| 070 | Zero Trust & Service Mesh | Decision Guide | Security & Platform | B | Zero Trust als Prinzip, Mesh als mögliche Umsetzung; Identity, mTLS, Policy, Observability und Komplexität. |
| 071 | GraalVM / Native Image | Decision Guide | Runtime | B | Startup/RAM vs Build-/AOT-Komplexität; nur anhand gemessener Ziele wählen. |
| 072 | Chaos Engineering | Operating Model | Resilience | B | Hypothese, Blast Radius, Guardrails, Abbruchkriterien und Lernen; nicht als Produktionsexperiment ohne Reife. |
| 073 | Database Indexing | Engineering Guideline | Data & Performance | B | Query-Plan, Selektivität, Write-Kosten und EXPLAIN; keine Index-Religion. |
| 074 | Scheduled Jobs | Architecture Standard | Processing & Operations | B | Single execution, Idempotenz, Retry, Zeit, Locks, Monitoring und Orchestrierung. |
| 075 | Architecture Decision Process | Governance Operating Model | Architecture Governance | A | Wird Kern des neuen Systems: Intake, Kriterien, Entscheidung, Review, Log, Exceptions, Supersession. |
| 076 | Technology Radar | Governance Operating Model | Technology Strategy | B | Lifecycle-Kategorien, Evidenz, Owner, Review und Ausnahmeprozess; kein Meinungsranking ohne Kriterien. |
| 077 | Microservices vs Modulith | Decision Guide | Application Architecture | A | Verteilte Komplexität nur bei echten Treibern: Autonomie, Skalierung, Isolation, Org-Schnitt. |
| 078 | Data Strategy | Architecture Policy | Data Governance | B | Ownership, System of Record, Data Product, Qualität, Lifecycle und föderierte Governance; Enterprise-Flughöhe. |
| 079 | Modulith Strategy | Reference Architecture | Application Architecture | B | Ziel-/Übergangsarchitektur mit Modul-APIs, Datenregeln, Fitness Functions und Extraktionstriggern. |
| 080 | DevSecOps | Reference Architecture | Security & Delivery | B | Controls in Pipeline, Evidence, Policy Gates, SBOM/SAST/SCA/DAST; Tooling austauschbar. |
| 081 | Was ist Architektur? | Learning Guide | Architecture Fundamentals | A | Architekturarbeit als Klären, Entwerfen, Bewerten, Kommunizieren; Entscheidung vs Detail. |
| 082 | Qualitätsziele | Architecture Method | Architecture Fundamentals | A | Qualitätsszenarien vor Technologie; messbare Stimulus/Response-Szenarien und Trade-offs. |
| 083 | Architekturmuster | Learning Guide | Architecture Fundamentals | A | Kein Muster ohne Problem und Qualitätsziel; Kräfte und Konsequenzen vor Pattern-Namen. |
| 084 | Entwurfsprinzipien | Architecture Principle | Software Architecture | A | Kopplung, Kohäsion, Information Hiding, DIP, zeitliche/organisatorische Kopplung. |
| 085 | Sozio-technische Systeme | Architecture Principle | Organization & Architecture | A | Conway, Team Topologies, Ownership, cognitive load; Architektur- und Verantwortungsgrenzen koppeln. |
| 086 | Querschnittskonzepte | Architecture Standard Framework | Cross-cutting Architecture | A | Katalog für Security, Logging, Fehler, Config, Idempotenz etc.; konkrete Standards verlinken statt duplizieren. |
| 087 | Architekturbewertung / ATAM | Governance Operating Model | Architecture Governance | B | Qualitätsszenarien, Risiken, Trade-offs, Fitness Functions und Reviews; Methode an Größe/Risiko anpassen. |
| 088 | arc42, C4, UML, Docs-as-Code | Documentation Standard | Architecture Documentation | B | Viewpoint-getriebene Dokumentation; Werkzeuge nach Stakeholderfrage wählen. |
| 089 | Strategic DDD | Decision Guide | Domain Architecture | A | Bounded Context, Context Map, Up-/Downstream, ACL, Published Language, Ownership. |
| 090 | Evolutionary Architecture | Architecture Principle | Transformation | A | Transition states, Strangler, Branch by Abstraction, Fitness Functions, laufende Neubewertung. |
| 091 | Reactive Architecture | Decision Guide | Runtime & Integration | B | Reactive Eigenschaften von Reactive Programming trennen; Virtual Threads/Streams nach Problem wählen. |
| 092 | Cloud-Native Architecture | Reference Architecture | Cloud & Platform | B | Cloud-native Fähigkeiten statt Hostingetikett; Automatisierung, Resilience, Observability, Portability. |
| 093 | Living Documentation | Documentation Standard | Architecture Governance | B | Generierbare technische Fakten automatisieren; Enterprise-Fakten aus verantworteten Quellen ergänzen. |
| 094 | Gesamtsynthese / iSAQB-Lernmodell | Learning Guide | Architecture Learning | C | Als Lernlandkarte kennzeichnen; keine 'Guru-Level'- oder Zertifizierungsbehauptungen. |
| 095 | AsyncAPI | Architecture Standard | Integration | B | Event-Verträge mit Channel, Message, Producer/Consumer, Security, Semantik und Lifecycle. |
| 096 | Teststrategie | Quality Standard | Testing | A | Testarten nach Risiko/Evidenz ordnen; Coverage nicht als alleinige Qualitätsmetrik. |
| 097 | Dead Letter Queue | Operating Standard | Messaging Operations | B | Fehlerklassen, Retry, DLQ, Reprocessing, Idempotenz, Alerting und Runbook. |
| 098 | Zero-Downtime DB Migration | Reference Pattern | Data & Delivery | A | Expand/Migrate/Contract als Transition Architecture; Rückwärtskompatibilität und Rollback. |
| 099 | MapStruct | Engineering Guideline | Java & Mapping | B | Modelle an Grenzen trennen; konkrete Mapper-Technologie austauschbar halten. |
| 100 | Helm & Kustomize | Decision Guide | Platform | C | Keine universelle Entscheidung; Auswahl nach Packaging/Overlay/Ownership. Überschneidung Company Chart vs Kustomize bereinigen. |
| 101 | Spring Security OAuth2 Resource Server | Security Standard | IAM & Security | C | Resource-Server-Baseline aktualisieren; CSRF-Entscheidung kontextabhängig, JWT nicht manuell parsen. |
| 102 | OpenTelemetry | Reference Architecture | Observability | B | OTel als Instrumentation/Telemetry Pipeline; Backendneutral, PII-Redaction und Sampling. |
| 103 | Kafka Streams | Decision Guide | Event Processing | B | Stateful Stream Processing vs Listener/Batch; State Stores, Rebalancing, EOS-Claims und Betrieb. |
| 104 | Elasticsearch | Decision Guide | Data & Search | B | Suchindex als abgeleitetes Read Model, nicht Source of Truth; Sync, Rebuild und Mapping Lifecycle. |
| 105 | WebSocket & SSE | Decision Guide | Integration | B | Kommunikationsrichtung, Proxy/Timeout, Skalierung, Backpressure und Security entscheiden. |
| 106 | Data Masking & Pseudonymisierung | Privacy Standard | Privacy | B | Technische Schutzmaßnahmen nach Datenklasse/Zweck; Masking, Pseudonymisierung, Verschlüsselung sauber unterscheiden. |
| 107 | FinOps | Operating Model | Cost & Governance | B | Kostenallokation, Unit Economics, Budgets, Guardrails; Zahlen/Preise als variable Baseline, nicht Norm. |
| 108 | Developer Experience & Golden Path | Reference Architecture | Platform & Organization | A | Platform as Product, Self-Service, paved road, Feedback und Adoption-Metriken; Ausnahmen ermöglichen. |
| 109 | Wicket 10 – drei Umgebungen | Engineering Guideline | Legacy / Runtime | B | Framework-spezifischer Guide; sichere Defaults, Environment Guard und Konfigurationsseparation validieren. |
| 110 | API Deprecation & Sunset | Lifecycle Policy | Integration Governance | B | Lifecycle, Consumer Discovery, Deprecation/Sunset-Kommunikation; Fristen organisatorisch festlegen, nicht universell. |
| 111 | GraphQL Federation | Decision Guide | Integration & Organization | C | Federation nach Ownership/Graph-Komposition, nicht nach fixer Teamzahl; Apollo/Hive-Baseline aktualisieren. |
| 112 | Database Partitioning & Sharding | Decision Guide | Data & Scaling | C | Starre Row-/Write-Schwellen entfernen; Workload, Memory, Retention, Query-Muster und Messung entscheiden lassen. |
| 113 | Read Replicas & Routing | Decision Guide | Data & Scaling | C | Replica-Lag und Konsistenz explizit; nicht als 'einfachste CQRS-Form' bezeichnen; Routingmuster messen. |
| 114 | ArgoCD & GitOps | Reference Architecture | Platform & Delivery | B | OpenGitOps-Prinzipien als Basis; Argo-spezifische Sync-/SelfHeal-/Rollback-Semantik aktuell halten. |
| 115 | Spring AI / LLM Integration | Decision Guide | AI Architecture | C | Spring-AI-Versionen/Starter aktualisieren; LLM-Governance, Evaluation, Datenschutz, Provider-Risiko und Human Oversight ergänzen. |
| 116 | Distributed Locking | Decision Guide | Distributed Systems | B | Lock nur wenn Koordination wirklich nötig; Fencing, Lease, Idempotenz und Failure Modes ergänzen. |
| 117 | JFR & Async-Profiler | Operating Guide | Runtime & Performance | B | Metrik→Trace→Profiling→Verifikation; JFR-Flags und Overhead-Aussagen auf aktuelle JDK-Baseline prüfen. |
| 118 | CORS & CSP Security Header | Security Standard | Web Security | C | CORS nicht als CSRF-Schutz darstellen; CSP/HSTS/Header nach OWASP/MDN, Browser/API-Kontext differenzieren. |
| 119 | Test Data Management | Engineering Guideline | Testing | B | Builder/Object Mother/Fixtures als Optionen; deterministische vs generative Daten und Datenschutz ergänzen. |
| 120 | Post-Mortem & Incident Management | Operating Model | Operations & Learning | C | Blameless/systemisch beibehalten; unbelegte Prime-Day-/99.999%-Behauptung und 'nie wieder' entfernen. |
| 121 | Quelle nicht vorhanden | Reserved | Unknown | D | Nicht erfinden; Nummer bleibt reserviert bis Originalquelle vorliegt. |
| 122 | Quelle nicht vorhanden | Reserved | Unknown | D | Nicht erfinden; Nummer bleibt reserviert bis Originalquelle vorliegt. |
| 123 | Quelle nicht vorhanden | Reserved | Unknown | D | Nicht erfinden; Nummer bleibt reserviert bis Originalquelle vorliegt. |
| 124 | Quelle nicht vorhanden | Reserved | Unknown | D | Nicht erfinden; Nummer bleibt reserviert bis Originalquelle vorliegt. |
| 125 | Input Validation | Security Standard | Web Security | B | Trust Boundaries, syntaktische/semantische Validierung, Fehlerantworten, Encoding; mit ADR-015 entflechten. |
| 126 | Docker auf Debian installieren, betreiben, absichern | Operating Guide | Platform Operations | B | Host-/Daemon-/Rootless-/Image-/Compose-Betrieb trennen; Debian/Docker-Versionen als validierte Baseline führen. |

## Besondere Konflikte, die im Rewrite aufgelöst werden müssen

### 1. QG-006 fehlt als Originalquelle

Mehrere Dateien referenzieren `QG-JAVA-006` als Spring-Boot-Serviceschicht bzw. als Quelle für Tenant Context und Optimistic Locking. Da die Quelldatei im Repository nicht vorhanden ist, wird sie **nicht aus Querverweisen erfunden**. `AK-006` bleibt bis zur Originalquelle `source-missing`.

### 2. 039 und 039-01

`039-01` wird als spezialisierter Unterartikel zu `AK-039 Feature Flags` geführt. Die Parent-/Child-Beziehung wird explizit statt über eine ungewöhnliche Nummernkonvention implizit gemacht.

### 3. 041 und 042

`AK-041` beantwortet die breite EDA-/Kafka-Entscheidungsfrage. `AK-042` ist ein spezialisierter Decision Guide für das Dual-Write-Problem und Transactional Outbox.

### 4. 056, 077 und 079

Diese drei Themen werden bewusst getrennt:

- `056` – Reference Architecture: Wie sieht ein Modulith technisch aus?
- `077` – Decision Guide: Wann Modulith, wann Microservices?
- `079` – Strategy/Reference Architecture: Wie entwickelt und überwacht man den Modulith als Übergangs- und Zielarchitektur?

### 5. 066, 095 und 110

Sie bilden zusammen API Governance:

- `066` – API First / OpenAPI.
- `095` – AsyncAPI / Event Contracts.
- `110` – API Lifecycle, Deprecation und Sunset.

### 6. 080, 108 und 114

Sie bilden zusammen Platform & Delivery Governance:

- `080` – DevSecOps Controls und Evidence.
- `108` – Golden Path / Platform as Product.
- `114` – GitOps als deklaratives Betriebs- und Deploymentmodell.

### 7. 017, 054, 102, 117 und 120

Sie bilden den Betriebsregelkreis:

```text
Telemetry Standard (017/102)
       ↓
SLI/SLO/Alerting (054)
       ↓
Incident Detection
       ↓
Profiling/Diagnosis (117)
       ↓
Post-Mortem/Learning (120)
       ↓
Standard / Architecture Improvement
```

### 8. 035 und 106

`035` bleibt auf Principle-/Policy-Ebene: Privacy by Design, Datenminimierung, Zweckbindung, Lifecycle.

`106` wird technischer Standard: Masking, Pseudonymisierung, Redaction, Testdaten und Observability.

### 9. 112 und 113

Diese Dokumente werden von festen Schwellenwerten befreit. Datenbankskalierung wird künftig workload- und evidenzbasiert entschieden.

### 10. 115 AI

`115` wird nicht bloß auf neue Spring-AI-Artefaktnamen aktualisiert. Es braucht eine zusätzliche Enterprise-Ebene: Use-Case-Zulässigkeit, Datenklassifikation, Provider-/Modellrisiko, Evaluation, Human Oversight, Kosten, Logging, Prompt-/Model-Lifecycle und Exit-Strategie.

## Definition of Done für jeden Rewrite

Ein migriertes Knowledge-Item ist erst fertig, wenn:

- der Artefakttyp stimmt,
- der Scope klar ist,
- keine erfundene Entscheidung oder Rolle enthalten ist,
- Primärquellen geprüft wurden,
- Technologie-Baselines datiert sind,
- absolute Heuristiken entfernt oder begründet wurden,
- positive **und negative** Trade-offs sichtbar sind,
- Beziehungen zu anderen Knowledge-Items korrekt sind,
- normative Regeln prüfbar sind,
- Review-Trigger existieren,
- Beispiele ausdrücklich als Beispiele erkennbar sind.

Das Ziel ist nicht, 126 lange Texte zu besitzen.

Das Ziel ist ein System, in dem ein Architekt schnell erkennen kann:

> **Welche Frage habe ich? Welches Wissen hilft mir? Was ist Standard? Was muss ich entscheiden? Wie beweise ich die Einhaltung? Und wann muss ich neu bewerten?**
