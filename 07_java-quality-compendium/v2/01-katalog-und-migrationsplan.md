# Migrationskatalog — Altbestand → Architecture Governance Compendium v2

## Leseregel

Dieser Katalog ist der Bauplan für die schrittweise Neuschreibung. Er entscheidet **noch nicht automatisch über technische Richtigkeit**. Für jede Nummer wird festgelegt, welcher Artefakttyp fachlich passt, welche Kernfrage künftig beantwortet werden soll und welche Validierung nötig ist.

Kürzel: `AP` Principle · `ADR` konkrete Entscheidung · `AS` Standard · `RA` Reference Architecture · `EG` Engineering Guideline · `OM` Operating Model · `RP` Runbook/Operational Practice · `LN` Learning Note.

## 001–020 — Sprache, Design, Testing, Security und Persistenz

| Alt-ID | Thema | Zieltyp | v2-Konzept / Kernfrage | Validierung |
|---|---|---|---|---|
| 001 | Records | EG | Wann ist ein Record ein transparenter Datenträger und wann ist eine normale Klasse/Domänenentität geeigneter? | Oracle/JLS/JEP 395; Framework-Grenzen separat prüfen. |
| 002 | Sealed Classes | EG | Wie modellieren wir bewusst geschlossene Typ-Hierarchien und Exhaustiveness? | JLS/JEP 409; Java-Baseline prüfen. |
| 003 | Pattern Matching / switch | EG | Wann erhöht Pattern Matching Ausdrucksstärke und wann versteckt es polymorphes Verhalten? | Oracle/JEP 441/Record Patterns. |
| 004 | Virtual Threads | EG | Für welche I/O-lastigen Workloads verbessern Virtual Threads Durchsatz und Wartbarkeit; welche Grenzen bleiben? | JEP 444 plus aktueller Pinning-Stand; Benchmarks kontextbezogen. |
| 005 | Text Blocks | EG | Wann verbessern Text Blocks Lesbarkeit ohne SQL/JSON/HTML-Sicherheit vorzutäuschen? | JLS/JEP 378. |
| 006 | Spring-Boot-Serviceschicht | SOURCE_GAP → vermutlich EG/RA | Referenziert, aber Datei fehlt. Erst Quelle wiederherstellen; dann Service-/Application-Layer-Verantwortung sauber definieren. | Keine Rekonstruktion aus Querverweisen allein. |
| 007 | Feature-Missbrauch | EG | Wie erkennt man syntaktisch moderne, aber semantisch falsche Nutzung von Java-Features? | Gegen jeweilige Sprachspezifikation; Beispiele fachlich prüfen. |
| 008 | OOP richtig anwenden | LN + EG | Welche OOP-Prinzipien schützen Invarianten und Verhalten statt Datencontainer mit Settern zu erzeugen? | JLS + etablierte Designliteratur; keine Dogmen. |
| 009 | Architekturentscheidungen im Code sichtbar machen | AS | Wie werden Entscheidungen durch Code-Struktur, Annotationen, ArchUnit und Tests nachvollziehbar? | ArchUnit/offizielle Tooling-Doku; Traceability-Konzept. |
| 010 | JUnit 5 | EG | Wie strukturieren wir Unit Tests als lesbare Spezifikation? | JUnit-Dokumentation. |
| 011 | Mockito | EG | Wann ist Mocking sinnvoll und wann testet man nur Implementierungsdetails? | Mockito-Doku; Testdesign-Literatur. |
| 012 | AssertJ | EG | Wie erhöhen Assertions Diagnosequalität und Lesbarkeit? | AssertJ-Doku. |
| 013 | Testklassendesign | EG | Wie organisieren wir Tests nach Verhalten, Isolation und Lesbarkeit? | JUnit + Testliteratur. |
| 014 | Parametrisierte Tests | EG | Wann reduzieren Datenvarianten Duplikation ohne Testabsicht zu verschleiern? | JUnit-Doku. |
| 015 | OWASP / Input / Spring Security | AS | Welcher Security-Baseline-Standard gilt für HTTP-Services? Inhalte später mit 040/101/118/125 entflechten. | OWASP ASVS/Cheat Sheets, Spring Security, RFCs. |
| 016 | JPA / N+1 / Fetch / Transaktionen | EG | Wie halten wir Persistenzzugriff explizit, performant und transaktionssicher? | Jakarta Persistence/Hibernate/Spring Tx. |
| 017 | Observability | AS | Welche Mindesttelemetrie muss jeder Service liefern? Später mit 102 als RA kombinieren. | OpenTelemetry/Micrometer/SRE. |
| 018 | Testcontainers | EG | Wann sind echte Infrastrukturabhängigkeiten im Integrationstest erforderlich? | Testcontainers-Doku. |
| 019 | Contract Testing | AS + EG | Wie schützen Consumer/Provider ihre HTTP- und Messaging-Verträge? | Pact/Spring Cloud Contract/OpenAPI/AsyncAPI. |
| 020 | SpringBootTest / Slice Tests | EG | Welche Testtiefe benötigt welcher Risikotyp? | Spring Boot Test-Doku. |

## 021–040 — APIs, Resilience, Architekturpatterns, Delivery, Privacy, Platform

| Alt-ID | Thema | Zieltyp | v2-Konzept / Kernfrage | Validierung |
|---|---|---|---|---|
| 021 | REST API Design | AS | Verbindlicher REST-/HTTP-API-Standard: Ressourcen, Fehler, Idempotenz, Versionierung, Pagination, Security. | HTTP RFCs, OpenAPI, Zalando/Google nur ergänzend. |
| 022 | Circuit Breaker / Retry / Timeout / Bulkhead | AS + EG | Welche Failure-Handling-Regeln gelten für synchrone Remote Calls? | Resilience4j + SRE; Retry nur bei geeigneter Semantik. |
| 023 | DDD | LN + RA | Wie modellieren wir Domänen, Aggregate und Sprache; klare Trennung Tactical/Strategic. | Evans/Vernon; später 089 verknüpfen. |
| 024 | Caching | AS + EG | Wann ist Cache eine Performanceoptimierung und wie werden Konsistenz, TTL und Stampede beherrscht? | Redis/Caffeine/HTTP Cache je Kontext. |
| 025 | SOLID | LN | Designheuristiken statt starres Regelwerk; Bezug auf Änderbarkeit/Kopplung. | Fachliteratur, Beispiele validieren. |
| 026 | KISS/DRY/YAGNI/LoD | LN | Wie vermeidet man unnötige Abstraktion und versteckte Kopplung? | Literatur; keine absoluten Regeln. |
| 027 | Code Smells | EG/LN | Smells als Diagnosehinweis, nicht automatischer Refactoring-Befehl. | Fowler/Refactoring-Literatur. |
| 028 | Creational Patterns | LN | Patternwahl problemgetrieben, nicht Kataloglernen. | GoF + moderne Java-Eignung. |
| 029 | Structural Patterns | LN | Strukturelle Patterns über Qualitätsziel und Trade-off erklären. | GoF + moderne Anwendung. |
| 030 | Behavioral Patterns | LN | Verhaltenspatterns mit Failure Modes und Alternativen erklären. | GoF + moderne Anwendung. |
| 031 | Hexagonal Architecture | RA | Referenzarchitektur für Ports/Adapters mit Abhängigkeitsregeln und Confirmation. | Cockburn + ArchUnit. |
| 032 | CQRS | ADR/RA je Kontext | Wann rechtfertigt unterschiedliche Read-/Write-Sicht zusätzliche Komplexität? | Fowler/Microsoft/DDD-Literatur; keine Gleichsetzung mit Event Sourcing. |
| 033 | Thread Safety / Deadlocks / Races | EG | Korrektheit bei Nebenläufigkeit: Immutability, Synchronisation, atomare Operationen, Diagnose. | JMM/JDK-Doku. |
| 034 | Flyway | AS + EG | Standard für versionierte DB-Migrationen inkl. Zero-Downtime-Regeln. | Flyway/PostgreSQL; 098 verknüpfen. |
| 035 | Privacy by Design | AP + AS | Datenschutzprinzipien in Datenminimierung, Zweckbindung, Retention und technische Controls übersetzen. | DSGVO/EDPB/BSI; 106 als technische Ausprägung. |
| 036 | CI/CD Pipeline | RA + AS | Referenzpipeline mit Build, Test, Security, Artefakt, Promotion, Rollback und Evidence. | SLSA/OWASP/Tooling-Doku. |
| 037 | Docker Container | AS + EG | Container-Baseline für Images, User, Secrets, Health, SBOM und Runtime. | Docker/OCI/CIS/OWASP. |
| 038 | Kubernetes | RA + AS | Kubernetes-Workload-Standard: Resources, Probes, Security, Config, Availability, Ownership. | Kubernetes-Doku. |
| 039 | Feature Flags | AS + OM | Lifecycle, Ownership, Typen, Telemetrie und Entfernung von Flags. | OpenFeature/Tooling; 039-01 als Spezialfall. |
| 039-01 | statische Flags | EG | Wann reicht Konfiguration statt dynamischer Flag-Plattform? | Spring Config/12-factor; Scope klar. |
| 040 | OAuth2/OIDC/JWT | LN + AS | Protokolle sauber trennen: OAuth2 Authorization, OIDC Authentication, Tokenformate und sichere Flows. | RFC 9700, OIDC Core, OAuth RFCs. |

## 041–060 — EDA, Performance, Dokumentation, Delivery, Plattform

| Alt-ID | Thema | Zieltyp | v2-Konzept / Kernfrage | Validierung |
|---|---|---|---|---|
| 041 | EDA & Kafka | RA + AS | Referenzarchitektur für Events: Ownership, Schema, Delivery-Semantik, Idempotenz, Observability, Security. | Kafka/Confluent specs; 042/095/097 als Spezialisierung. |
| 042 | Transactional Outbox | RA/EG | Wie vermeiden wir Dual-Write zwischen DB und Broker? | Pattern + Debezium/Kafka Connect; Semantik präzise. |
| 043 | JVM Tuning / HikariCP | EG/RP | Performance nur evidenzbasiert tunen: JVM, GC, Memory, Connection Pools. | JDK/Hikari/PostgreSQL. |
| 044 | Load & Performance Testing | AS + EG | Welche Lastmodelle und SLOs müssen vor Release nachgewiesen werden? | k6/Gatling/JMeter + SRE; keine universellen Grenzwerte. |
| 045 | Mutation Testing | EG | Testwirksamkeit ergänzend zu Coverage messen. | PIT-Doku; Schwellenwerte risikobasiert. |
| 046 | Property-Based Testing | EG | Invarianten und generative Tests für Domänen-/Transformationslogik. | jqwik/QuickTheories. |
| 047 | C4 | LN + AS | Softwarearchitektursichten mit C4; Abgrenzung zu ArchiMate und ADR. | C4 Model/Structurizr. |
| 048 | JavaDoc | EG | Dokumentiere Verträge, Semantik und Risiken statt offensichtlichen Code. | Javadoc Tool/Java API conventions. |
| 049 | Code Review / PR | OM + AS | Reviewprozess mit Verantwortlichkeit, Risikoklassen, Checks und Entscheidungseskalation. | GitHub/GitLab + Forschung/Literatur ergänzend. |
| 050 | Multi-Module Gradle | EG/RA | Buildstruktur als technische Durchsetzung von Modulgrenzen. | Gradle/JPMS/ArchUnit. |
| 051 | Definition of Done / Tech Debt | OM | Wie werden Qualitätsanforderungen und Schulden in Delivery steuerbar? | Agile/DevOps-Literatur; fixe Kapazitätsprozente vermeiden. |
| 052 | Immutability / Defensive Programming | EG | Invarianten schützen, Aliasing reduzieren, mutable Collections defensiv behandeln. | JDK/Effective Java. |
| 053 | GraphQL | ADR/AS | Wann ist GraphQL gegenüber REST gerechtfertigt; Security, Complexity, N+1, Schema-Lifecycle. | GraphQL Spec/Spring GraphQL. |
| 054 | SLI/SLO/SLA | AS + OM | Verbindliche Service-Level-Terminologie, Error Budgets, Alerting und Ownership. | Google SRE. |
| 055 | Event Sourcing | ADR/RA | Wann rechtfertigen Audit-/Temporal-Anforderungen Event Sourcing? | Fowler/EventStore/DDIA; nicht als Default. |
| 056 | Spring Modulith | EG/RA | Technische Umsetzung eines modularen Monolithen mit verifizierbaren Grenzen. | Spring Modulith. |
| 057 | SBOM / Dependency Auditing | AS | Software-Supply-Chain-Standard: SBOM, CVE, Lizenz, Provenance, Updateprozess. | CycloneDX/SPDX/SLSA/OWASP. |
| 058 | Multi-Tenancy | ADR/RA | Isolationsebene anhand Schutzbedarf, Kosten, Betrieb und Datenrisiko entscheiden. | PostgreSQL/cloud DB docs/Security. |
| 059 | Blue/Green / Canary | RA + OM | Progressive Delivery mit messbarer Promotion/Rollback-Logik. | Kubernetes/Argo Rollouts/Flagger. |
| 060 | Infrastructure as Code | AS + OM | Infrastrukturänderungen versioniert, reviewbar, reproduzierbar und drift-erkennbar machen. | Terraform/OpenTofu/Ansible/OpenGitOps. |

## 061–080 — Fitness, Betrieb, Integration, Strategie und Governance

| Alt-ID | Thema | Zieltyp | v2-Konzept / Kernfrage | Validierung |
|---|---|---|---|---|
| 061 | Architecture Fitness Functions | AP + AS | Architektureigenschaften als kontinuierlich prüfbare Controls formulieren. | Building Evolutionary Architectures + Tooling. |
| 062 | Rate Limiting | AS/EG | Schutzgrenzen für APIs/Services mit Algorithmus, Scope, Response-Semantik und Monitoring. | IETF HTTP 429 + Gateway/libs. |
| 063 | Backup & DR | AS + OM + RP | Recovery Capability mit RPO/RTO, Restore-Tests, Runbooks und Ownership. | PostgreSQL/cloud provider/NIST/BSI. |
| 064 | Changelog & SemVer | AS | Release-/Compatibility-Standard; SemVer nur wo Versionsvertrag sinnvoll ist. | semver.org + API-Lifecycle. |
| 065 | Saga | RA/ADR | Distributed Business Transaction: Choreography vs Orchestration, Compensation, Observability. | Patternliteratur/Temporal/Camunda je Beispiel. |
| 066 | API First / OpenAPI | AS | Schnittstellenvertrag vor Implementierung; Lint, Breaking-Change-Check, Generierung. | OpenAPI Spec/Spectral/openapi-diff. |
| 067 | gRPC | ADR/AS | Wann gRPC gegenüber HTTP/JSON sinnvoll ist; Protobuf-Lifecycle und Operations. | gRPC/Protobuf docs. |
| 068 | Spring Batch | RA/EG | Batch als eigenes Processing Model: Restart, Chunking, Idempotenz, Scheduling. | Spring Batch. |
| 069 | Configuration Management | AS | Code, Config und Secrets trennen; Lifecycle und Rotation definieren. | 12-factor + Vault/K8s docs. |
| 070 | Zero Trust / Service Mesh | AP + RA | Workload Identity, mTLS, Authorization und Microsegmentation als Zielbild; Mesh nur als Option. | NIST SP 800-207, SPIFFE, Istio. |
| 071 | GraalVM Native | ADR/EG | Native Image nur bei messbaren Startup/Memory-Treibern. | GraalVM docs; aktuelle Spring AOT-Unterstützung. |
| 072 | Chaos Engineering | OM/RP | Hypothesengetriebene Resilience-Experimente mit Blast Radius, Stop Conditions und Learnings. | Principles of Chaos + tooling. |
| 073 | Database Indexing | EG | Indexwahl über Query-Pläne und reale Workloads statt Faustregeln. | PostgreSQL docs. |
| 074 | Scheduled Jobs | AS/RA | Zuverlässige zeitgesteuerte Verarbeitung: Idempotenz, Locking, Retry, Monitoring. | Quartz/K8s CronJob/Spring; 116 verknüpfen. |
| 075 | Architektur-Entscheidungsprozess | OM | Governance für Entscheidungsbedarf, Draft, Review, Board, Evidence, Ausnahme und Lifecycle. | Nygard/MADR/arc42/TOGAF. |
| 076 | Technology Radar | OM | Technologie-Lifecycle mit Adopt/Trial/Assess/Hold, Ownern, Kriterien und Abbaupfaden. | Thoughtworks-Modell als Inspiration; organisationsspezifisch. |
| 077 | Microservices vs Modulith | ADR-Template/LN | Entscheidungsrahmen für Verteilung: Teamautonomie, Skalierung, Isolation, Betriebsreife, Daten. | Fowler/Team Topologies/DDD. |
| 078 | Data Strategy | AP + OM + AS | Data Ownership, Data Products, Quality, Metadata, Lifecycle und Governance. | DAMA/Data Mesh als Quellen; Behördenkontext separat. |
| 079 | strategische Modulith-Architektur | RA | Vollständige Referenzarchitektur für modularen Monolithen, Extraktionstrigger und Governance. | Spring Modulith/DDD/ArchUnit. |
| 080 | DevSecOps Governance | RA + AS + OM | Security Controls durch Pipeline, Policy Gates, SBOM, SAST/SCA/DAST und Evidence. | OWASP/SLSA/NIST SSDF. |

## 081–100 — Architekturlehre, Evolution, Cloud, Integration und Engineering

| Alt-ID | Thema | Zieltyp | v2-Konzept / Kernfrage | Validierung |
|---|---|---|---|---|
| 081 | Was ist Architektur? | LN | Architektur als Klären, Entwerfen, Bewerten und Kommunizieren; Abgrenzung zu Implementierungsdetail. | ISO 42010/arc42/iSAQB. |
| 082 | Qualitätsziele | LN + AP | Qualitätsziele → Szenarien → Trade-offs → Messung. | ISO 25010/arc42/ATAM. |
| 083 | Architekturmuster | LN | Kein Pattern ohne Problem und Qualitätsziel. | Patternliteratur. |
| 084 | Entwurfsprinzipien | LN/AP | Kopplung, Kohäsion, SoC, Information Hiding, DIP als Änderbarkeitshebel. | Parnas/Designliteratur. |
| 085 | Sozio-technische Systeme | LN + AP + OM | Architekturgrenzen, Teamgrenzen, Cognitive Load und Ownership gemeinsam gestalten. | Conway/Team Topologies. |
| 086 | Querschnittskonzepte | AS | Systemweite Konzepte wie Fehler, Logging, Security, Transactions, Idempotenz standardisieren. | arc42 Crosscutting Concepts + fachliche Einzelquellen. |
| 087 | Architekturbewertung / ATAM | OM/LN | Qualitätsszenarien, Sensitivitäten, Trade-offs und Risiken strukturiert bewerten. | SEI ATAM. |
| 088 | arc42 / C4 / UML / Docs-as-Code | LN + AS | Welches Modell beantwortet welche Stakeholderfrage? | arc42/C4/UML. |
| 089 | Strategic DDD | LN/RA | Bounded Context, Context Map, Upstream/Downstream, ACL, Published Language. | Evans/Vernon. |
| 090 | Evolutionary Architecture | AP + OM | Zielbild über Transition States, Fitness Functions, Strangler und Review weiterentwickeln. | Evolutionary Architecture/Fowler. |
| 091 | Reactive Architecture | LN/ADR | Responsive/Resilient/Elastic/Message-Driven von Reactive Programming trennen. | Reactive Manifesto + JDK/Spring. |
| 092 | Cloud Native | AP + RA | Cloud-Native-Eigenschaften statt Hostingetikett: automation, resilience, observability, elasticity. | CNCF/12-factor/Kubernetes. |
| 093 | Living Documentation | AS + OM | Aus Code, APIs, Infrastruktur und Modellen aktualisierbare Architekturinformation erzeugen. | arc42/Structurizr/Docs-as-Code. |
| 094 | Gesamtsynthese | LN | Lernlandkarte des Compendiums; keine Kompetenzzertifizierung. | Interne Synthese, Quellen je Baustein. |
| 095 | AsyncAPI | AS | Event Contracts mit Channel, Message, Security, Ownership und Lifecycle. | AsyncAPI Specification. |
| 096 | Teststrategie | AS + OM | Testarten risikobasiert kombinieren, Quality Gates und Verantwortlichkeiten definieren. | Testpyramide/-honeycomb als Heuristik, Tool-Doku. |
| 097 | Dead Letter Queue | RP + AS | Fehlerklassen, Retry, DLQ, Reprocessing, Idempotenz und Runbook. | Kafka/RabbitMQ je Stack. |
| 098 | Zero-Downtime DB Migration | AS + RA | Expand → Migrate → Contract als Übergangsarchitektur. | PostgreSQL/Flyway/Deployment-Patterns. |
| 099 | MapStruct | EG | Explizites Mapping an Modellgrenzen; Domain/API/Persistence/Event nicht vermischen. | MapStruct-Doku. |
| 100 | Helm & Kustomize | AS/RA | Rollen klar trennen: Packaging vs Overlays; Wildwuchs vermeiden. | Helm/Kustomize/Kubernetes docs. |

## 101–120 — Security, Operations, Plattform, AI und Lernkultur

| Alt-ID | Thema | Zieltyp | v2-Konzept / Kernfrage | Validierung |
|---|---|---|---|---|
| 101 | Spring Security OAuth2 Resource Server | EG + AS | Sichere Standardimplementierung für Resource Server, JWT-Validation, Authorization und M2M. | Spring Security + OAuth/OIDC RFCs. |
| 102 | OpenTelemetry | RA + AS | Telemetrie-Pipeline mit Context Propagation, Collector, Datenschutz und Backend-Unabhängigkeit. | OpenTelemetry Spec/docs. |
| 103 | Kafka Streams | EG/RA | Wann Stateful Stream Processing statt einfacher Consumer sinnvoll ist. | Kafka Streams docs. |
| 104 | Elasticsearch | ADR/RA | Suchindex als abgeleitetes Read Model, nicht automatisch Source of Truth. | Elasticsearch + CQRS/DDD. |
| 105 | WebSocket & SSE | ADR/AS | Kommunikationsrichtung, Skalierung und Betriebsmodell bestimmen Protokollwahl. | WHATWG/SSE, RFC 6455. |
| 106 | Data Masking / Pseudonymisierung | AS + EG | Technische Schutzmechanismen nach Datenklasse und Nutzungszweck. | DSGVO/EDPB/OWASP; Begrifflichkeiten exakt. |
| 107 | FinOps | OM/AP | Kosten als Architekturqualität sichtbar machen; Unit Economics/Cost Allocation kontextgerecht. | FinOps Foundation. |
| 108 | Developer Experience / Golden Path | RA + OM | Platform as Product, Self-Service und Golden Paths als Governance-Mechanismus. | Team Topologies/CNCF Platform Engineering. |
| 109 | Wicket 10 drei Umgebungen | EG/ADR | Sichere Environment-Konfiguration für konkreten Legacy-/Wicket-Kontext; kein allgemeiner Unternehmensstandard. | Apache Wicket 10/Jakarta Servlet. |
| 110 | API Deprecation | AS + OM | API-Lifecycle mit Consumer Inventory, Deprecation, Migration, Sunset und Evidence. | RFC 9745 Sunset? HTTP Deprecation header RFC prüfen; OpenAPI. |
| 111 | GraphQL Federation | ADR/RA | Föderierter Graph nur bei echter Multi-Team-Ownership; Schema Governance und Router-Betrieb. | Apollo Federation/GraphQL spec; Vendorneutralität beachten. |
| 112 | DB Partitioning & Sharding | ADR/EG | Datenvolumen/Workload evidenzbasiert skalieren; harte Row-/Write-Grenzen als Heuristik markieren. | PostgreSQL Partitioning/DDIA. |
| 113 | Read Replicas | ADR/RA | Read-Skalierung gegen Replication Lag und Konsistenzbedarf abwägen. | PostgreSQL Streaming Replication/DDIA. |
| 114 | ArgoCD & GitOps | AS + RA + OM | Git als Desired-State-Quelle, Drift Detection, RBAC, Promotion und Rollback. | OpenGitOps/Argo CD. |
| 115 | Spring AI / LLM | EG + RA, später AI-Governance-AS | LLM-Abstraktion, RAG, PII, Evaluation, Kosten, Providerwechsel; technische Guideline allein reicht nicht. | Spring AI + NIST AI RMF/OWASP LLM + Providerdocs. |
| 116 | Distributed Locking | EG/ADR | Exklusive Ausführung nur wenn Idempotenz/Optimistic Coordination nicht genügen; Fencing beachten. | ShedLock/Redisson/DDIA. |
| 117 | JFR / Async-Profiler | RP + EG | Performance-Diagnose: Metrics → Traces → Profiling → Messung nach Fix. | JDK JFR/async-profiler. |
| 118 | CORS / CSP / Security Headers | AS + EG | Browser-Security-Header korrekt, kontextabhängig und testbar konfigurieren. | MDN/OWASP/W3C; CORS nicht mit CSRF gleichsetzen. |
| 119 | Test Data Management | EG | Builder, Object Mother, Fixtures und synthetische Daten nach Testtyp einsetzen. | xUnit Test Patterns/Datafaker. |
| 120 | Post-Mortem / Incident Management | OM + RP | Incident Lifecycle, blameless Learning, Action-Item-Tracking und Feedback in Standards/Plattform. | Google SRE/NIST Incident Response. |

## 121–126 — Quellenlücken und bestehende spätere Guidelines

| Alt-ID | Thema | Zieltyp | v2-Konzept / Kernfrage | Validierung |
|---|---|---|---|---|
| 121 | Quelle fehlt | SOURCE_GAP | Keine inhaltliche Rekonstruktion ohne Originalquelle. | warten auf Quelle. |
| 122 | Quelle fehlt | SOURCE_GAP | Keine inhaltliche Rekonstruktion ohne Originalquelle. | warten auf Quelle. |
| 123 | Quelle fehlt | SOURCE_GAP | Keine inhaltliche Rekonstruktion ohne Originalquelle. | warten auf Quelle. |
| 124 | Quelle fehlt | SOURCE_GAP | Keine inhaltliche Rekonstruktion ohne Originalquelle. | warten auf Quelle. |
| 125 | Input Validation | AS + EG | Eingaben als untrusted behandeln: Syntax, Semantik, Limits, Fehler, Deserialisierung, Injection-Schutz. | OWASP/Spring Validation/Jakarta Validation. |
| 126 | Docker auf Debian installieren/betreiben/absichern | EG + RP | Host-/Docker-Betriebsstandard getrennt von Container-Image-Standard 037 halten. | Docker Debian docs/systemd/rootless/security. |

## Migrationsreihenfolge

Die Migration erfolgt nicht blind numerisch, sondern in stabilen fachlichen Wellen:

1. **Sprache & Engineering-Grundlagen:** 001–014, 025–030, 048, 052, 099, 119.
2. **Softwarearchitektur:** 023, 031, 032, 050, 056, 077, 079, 081–090.
3. **Integration:** 019, 021, 041, 042, 053, 055, 065–067, 095, 097, 103–105, 110–111.
4. **Security & Privacy:** 015, 035, 040, 057, 062, 069–070, 080, 101, 106, 118, 125.
5. **Data & Persistence:** 016, 024, 034, 043, 073, 078, 098, 104, 112–113.
6. **Platform & Delivery:** 036–039, 059–060, 063, 092, 100, 102, 108, 114, 126.
7. **Operations & Resilience:** 017, 022, 044, 054, 061, 072, 074, 096, 116–117, 120.
8. **Governance & Economics:** 009, 049, 051, 064, 075–076, 093–094, 107.
9. **Emerging Technology:** 071, 115 plus später eigene AI-Governance-Artefakte.

## Coach-Regel für jede Migration

Beim Neuschreiben wird immer zuerst gefragt:

1. Ist das überhaupt eine Entscheidung?
2. Welches Problem löst das Thema?
3. Welche Qualitätsziele und Stakeholder treiben es?
4. Welche Alternativen existieren real?
5. Welche Aussagen sind Primärquelle, welche Heuristik, welche eigene Empfehlung?
6. Welche Failure Modes muss ein Senior Architect kennen?
7. Wie kann Umsetzung oder Compliance nachgewiesen werden?
8. Welche übertragbare Enterprise-Architecture-Lektion steckt darin?

Damit wird aus dem bisherigen Nummernkatalog ein lernbares, prüfbares und governancefähiges System.
