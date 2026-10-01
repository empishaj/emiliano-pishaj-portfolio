# Architecture Quality & Governance Compendium — Index

Dieser Index ist die kanonische Navigation durch das Compendium. Die historischen Nummern bleiben als stabile Knowledge-IDs erhalten; fehlende Nummern bedeuten nicht, dass Inhalte fehlen. Mehrere frühere Einzeldokumente wurden bewusst zusammengeführt oder durch passendere Artefakte ersetzt.

## 1. Governance

| ID / Dokument | Zweck |
|---|---|
| [Artifact Model](00_governance/ARTIFACT-MODEL.md) | Definiert Artefakttypen, Status, IDs und normative Sprache. |
| [Validation Policy](00_governance/VALIDATION-POLICY.md) | Legt Quellenhierarchie, Validierungsstufen und Regeln für zeitabhängige Aussagen fest. |
| [ADR Lifecycle & Template](00_governance/ADR-LIFECYCLE-AND-TEMPLATE.md) | Beschreibt, wann ein echtes ADR entsteht und wie Entscheidungen dokumentiert werden. |

## 2. Architecture Principles

| ID | Thema |
|---|---|
| [AK-025](01_principles/AK-025-solid-as-design-heuristics.md) | SOLID als Diagnose- und Gestaltungsheuristik |
| [AK-026](01_principles/AK-026-simplicity-dry-yagni-demeter.md) | Einfachheit, DRY, YAGNI und geringe Wissenskopplung |
| [AK-052](01_principles/AK-052-immutability-and-invariants.md) | Immutability, Invarianten und defensive Grenzen |
| [AK-084](01_principles/AK-084-coupling-cohesion-information-hiding.md) | Kopplung, Kohäsion und Information Hiding |
| [AK-085](01_principles/AK-085-socio-technical-architecture.md) | Sozio-technische Architektur, Ownership und Teamgrenzen |
| [AK-090](01_principles/AK-090-evolutionary-architecture.md) | Evolutionary Architecture und kontrollierte Veränderbarkeit |

## 3. Decision Guides

Decision Guides helfen bei wiederkehrenden Architekturfragen. Sie ersetzen kein projektspezifisches ADR.

| ID | Entscheidungsfrage |
|---|---|
| [AK-023](02_decision-guides/AK-023-domain-driven-design.md) | Wann ist DDD für die fachliche Komplexität angemessen? |
| [AK-031](02_decision-guides/AK-031-hexagonal-architecture.md) | Wann lohnt sich Ports & Adapters / Hexagonal Architecture? |
| [AK-032](02_decision-guides/AK-032-cqrs.md) | Wann sollten Read- und Write-Modelle getrennt werden? |
| [AK-040](02_decision-guides/AK-040-oauth2-oidc-token-architecture.md) | Wie werden OAuth2, OIDC und Tokenarchitektur kontextbezogen entschieden? |
| [AK-041](02_decision-guides/AK-041-event-driven-architecture.md) | Wann ist Event-Driven Architecture sinnvoll und welche Kosten entstehen? |
| [AK-077](02_decision-guides/AK-077-modulith-vs-microservices.md) | Wann rechtfertigen reale Treiber Microservices statt Modulith? |
| [AK-089](02_decision-guides/AK-089-strategic-ddd-context-mapping.md) | Wie werden fachliche Modell-, Ownership- und Integrationsgrenzen geschnitten? |

## 4. Standards & Policies

| ID | Thema |
|---|---|
| [AK-015](03_standards/AK-015-application-security-baseline.md) | Application Security Baseline |
| [AK-017](03_standards/AK-017-observability-standard.md) | Observability Mindeststandard |
| [AK-019](03_standards/AK-019-contract-testing.md) | Contract Testing |
| [AK-021](03_standards/AK-021-rest-api-standard.md) | REST/HTTP API Design |
| [AK-034](03_standards/AK-034-database-migration-standard.md) | Datenbankmigrationen und Schema Evolution |
| [AK-037](03_standards/AK-037-container-image-standard.md) | Container Image Security und Reproduzierbarkeit |
| [AK-057](03_standards/AK-057-software-supply-chain.md) | Software Supply Chain Security |
| [AK-060](03_standards/AK-060-infrastructure-as-code.md) | Infrastructure as Code Governance |
| [AK-061](03_standards/AK-061-architecture-fitness-functions.md) | Architecture Fitness Functions |
| [AK-062](03_standards/AK-062-rate-limiting-and-admission-control.md) | Rate Limiting und Admission Control |
| [AK-064](03_standards/AK-064-versioning-and-changelog.md) | Versionierung, Release Notes und Traceability |
| [AK-066](03_standards/AK-066-api-first-openapi.md) | API First und OpenAPI Governance |
| [AK-069](03_standards/AK-069-configuration-and-secrets.md) | Konfiguration und Secrets Management |
| [AK-086](03_standards/AK-086-cross-cutting-concepts.md) | Governance von Querschnittskonzepten |
| [AK-088](03_standards/AK-088-architecture-documentation.md) | Architekturdokumentation und Viewpoints |
| [AK-093](03_standards/AK-093-living-documentation.md) | Living Documentation und Architecture Traceability |
| [AK-095](03_standards/AK-095-asyncapi-event-contracts.md) | AsyncAPI und Event Contract Governance |
| [AK-096](03_standards/AK-096-test-strategy.md) | Teststrategie als risikobasierte Evidence |
| [AK-101](03_standards/AK-101-spring-security-resource-server.md) | Spring Security Resource Server Baseline |
| [AK-106](03_standards/AK-106-privacy-technical-controls.md) | Technische Privacy Controls |
| [AK-110](03_standards/AK-110-api-lifecycle-deprecation.md) | API Lifecycle, Deprecation und Sunset Policy |
| [AK-118](03_standards/AK-118-browser-security-headers.md) | Browser Security: CORS, CSP und Header |
| [AK-125](03_standards/AK-125-input-validation.md) | Input Validation und Trust Boundaries |

## 5. Reference Architectures

| ID | Thema |
|---|---|
| [AK-036](04_reference-architectures/AK-036-ci-cd-reference-pipeline.md) | CI/CD als Delivery- und Evidence-Pipeline |
| [AK-056](04_reference-architectures/AK-056-modular-monolith.md) | Modularer Monolith |
| [AK-079](04_reference-architectures/AK-079-modulith-strategy.md) | Modulith Strategy, Governance und Extraktionspfade |
| [AK-080](04_reference-architectures/AK-080-devsecops-controls.md) | DevSecOps Control- und Evidence-Architektur |
| [AK-102](04_reference-architectures/AK-102-opentelemetry.md) | OpenTelemetry Reference Architecture |
| [AK-114](04_reference-architectures/AK-114-gitops-argocd.md) | GitOps und Argo CD |

## 6. Operating Guides

| ID | Thema |
|---|---|
| [AK-072](05_operating-guides/AK-072-chaos-engineering.md) | Chaos Engineering als kontrolliertes Resilience-Experiment |
| [AK-126](05_operating-guides/AK-126-docker-debian-operations.md) | Docker Engine auf Debian sicher betreiben |

## 7. Operating Models

| ID | Thema |
|---|---|
| [AK-075](06_operating-models/AK-075-architecture-decision-process.md) | Architecture Decision Process |
| [AK-087](06_operating-models/AK-087-architecture-evaluation.md) | Architekturbewertung, Risiken und Trade-offs |

## 8. Learning Guides

| ID | Thema |
|---|---|
| [AK-008](07_learning-guides/AK-008-objektorientierung-und-verantwortung.md) | Objektorientierung, Verantwortung und Fehlanwendungen |
| [AK-027](07_learning-guides/AK-027-code-smells-und-refactoring.md) | Code Smells und Refactoring |
| [AK-028](07_learning-guides/AK-028-java-design-patterns.md) | Java Design Patterns problemorientiert einsetzen |
| [AK-081](07_learning-guides/AK-081-architecture-fundamentals.md) | Architekturgrundlagen |
| [AK-082](07_learning-guides/AK-082-quality-goals-and-scenarios.md) | Qualitätsziele und Qualitätsszenarien |
| [AK-083](07_learning-guides/AK-083-architecture-patterns.md) | Architekturmuster problemorientiert auswählen |
| [AK-094](07_learning-guides/AK-094-architecture-synthesis.md) | Architektur-Synthese und Zusammenhänge |

## 9. Engineering Guidelines

| ID | Thema |
|---|---|
| [AK-001](08_engineering-guidelines/AK-001-records-for-data-carriers.md) | Records für Datenträger |
| [AK-002](08_engineering-guidelines/AK-002-sealed-types.md) | Sealed Types |
| [AK-003](08_engineering-guidelines/AK-003-pattern-matching-switch.md) | Pattern Matching und `switch` |
| [AK-004](08_engineering-guidelines/AK-004-virtual-threads.md) | Virtual Threads |
| [AK-005](08_engineering-guidelines/AK-005-text-blocks.md) | Text Blocks |
| [AK-009](08_engineering-guidelines/AK-009-architekturentscheidungen-im-code.md) | Architekturentscheidungen im Code sichtbar machen |
| [AK-010](08_engineering-guidelines/AK-010-junit-testdesign.md) | JUnit-Testdesign und Teststruktur |
| [AK-011](08_engineering-guidelines/AK-011-test-doubles-mockito.md) | Test Doubles und Mockito |
| [AK-012](08_engineering-guidelines/AK-012-assertions-assertj.md) | Assertions mit AssertJ |
| [AK-014](08_engineering-guidelines/AK-014-parameterized-tests.md) | Parametrisierte Tests |
| [AK-016](08_engineering-guidelines/AK-016-jpa-persistence-access.md) | JPA/Persistence Access |
| [AK-018](08_engineering-guidelines/AK-018-testcontainers.md) | Testcontainers |
| [AK-020](08_engineering-guidelines/AK-020-spring-testkontexte.md) | Spring-Testkontexte und Test Slices |
| [AK-022](08_engineering-guidelines/AK-022-resilience-patterns.md) | Resilience Patterns |
| [AK-024](08_engineering-guidelines/AK-024-caching.md) | Caching |
| [AK-039](08_engineering-guidelines/AK-039-feature-flags.md) | Feature Flags |
| [AK-049](08_engineering-guidelines/AK-049-code-review-und-pull-requests.md) | Code Review und Pull Requests |

# Lesepfade

## Architekturgrundlagen

1. AK-081 — Was Architektur ist
2. AK-082 — Qualitätsziele und Szenarien
3. AK-084 — Kopplung, Kohäsion und Information Hiding
4. AK-083 — Pattern-Auswahl
5. AK-087 — Architekturbewertung
6. AK-075 — Architecture Decision Process
7. AK-090 — Evolutionary Architecture
8. AK-094 — Synthese

## Domain & Application Architecture

1. AK-008 — Objektorientierung und Verantwortung
2. AK-023 — DDD
3. AK-089 — Strategic DDD / Context Mapping
4. AK-031 — Hexagonal Architecture
5. AK-056 — Modularer Monolith
6. AK-077 — Modulith vs. Microservices
7. AK-079 — Modulith Strategy
8. AK-032 — CQRS

## Integration

1. AK-021 — REST/HTTP API Design
2. AK-066 — API First / OpenAPI
3. AK-019 — Contract Testing
4. AK-041 — Event-Driven Architecture
5. AK-095 — AsyncAPI / Event Contracts
6. AK-110 — API Lifecycle

## Security & Privacy

1. AK-015 — Application Security Baseline
2. AK-040 — OAuth2/OIDC/Token Architecture
3. AK-101 — Spring Security Resource Server
4. AK-125 — Input Validation
5. AK-118 — Browser Security
6. AK-106 — Privacy Technical Controls
7. AK-057 — Software Supply Chain

## Platform, Delivery & Operations

1. AK-037 — Container Images
2. AK-060 — Infrastructure as Code
3. AK-036 — CI/CD Reference Pipeline
4. AK-080 — DevSecOps Controls
5. AK-114 — GitOps / Argo CD
6. AK-017 — Observability
7. AK-102 — OpenTelemetry
8. AK-062 — Rate Limiting
9. AK-072 — Chaos Engineering
10. AK-126 — Docker/Debian Operations

## Engineering Quality

1. AK-010 — Testdesign
2. AK-011 — Test Doubles
3. AK-012 — Assertions
4. AK-014 — Parametrisierte Tests
5. AK-018 — Testcontainers
6. AK-020 — Spring Testkontexte
7. AK-096 — Teststrategie
8. AK-049 — Code Review
9. AK-061 — Fitness Functions

# Hinweis zu historischen IDs

Die Knowledge-IDs sind nicht als lückenlose Inhaltsnummerierung zu verstehen. Der Bestand ist aus einer historischen Sammlung gewachsen. Mehrere frühere Dokumente wurden zusammengeführt, um Redundanz zu reduzieren. Ein Thema wird im aktiven Bestand genau einmal kanonisch geführt; frühere IDs bleiben in den Metadaten als `legacy_ids` nachvollziehbar.
