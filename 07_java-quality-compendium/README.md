# Architecture Quality & Governance Compendium

Dieses Verzeichnis bündelt wiederverwendbares Wissen zu Softwarearchitektur, Integration, Security, Plattform, Betrieb, Engineering-Qualität und Architecture Governance.

Es ist bewusst **keine Sammlung fiktiver Architecture Decision Records**. Allgemeine Prinzipien, Standards, Entscheidungshilfen und technische Guidelines erfüllen unterschiedliche Aufgaben und werden deshalb getrennt geführt. Ein echtes ADR entsteht erst dann, wenn in einem konkreten Systemkontext tatsächlich eine architekturrelevante Entscheidung getroffen werden muss.

## Einstieg

Der vollständige Katalog und die empfohlenen Lesepfade stehen in:

- [`INDEX.md`](INDEX.md)

Die Regeln des Wissenssystems stehen unter [`00_governance/`](00_governance/).

## Struktur

| Ordner | Zweck |
|---|---|
| [`00_governance/`](00_governance/) | Regeln des Wissenssystems: Artefakttypen, Validierung, ADR-Lifecycle und Templates. |
| [`01_principles/`](01_principles/) | Langlebige Gestaltungsprinzipien wie Kopplung, Kohäsion, Einfachheit und Evolution. |
| [`02_decision-guides/`](02_decision-guides/) | Entscheidungshilfen für kontextabhängige Architekturfragen. |
| [`03_standards/`](03_standards/) | Normative, wiederverwendbare Architektur- und Engineering-Standards sowie Policies. |
| [`04_reference-architectures/`](04_reference-architectures/) | Wiederverwendbare Lösungsbilder und technische Zielmuster. |
| [`05_operating-guides/`](05_operating-guides/) | Konkrete Betriebs-, Diagnose- und Resilience-Anleitungen. |
| [`06_operating-models/`](06_operating-models/) | Rollen, Entscheidungswege, Reviews und Governance-Prozesse. |
| [`07_learning-guides/`](07_learning-guides/) | Grundlagen, Synthesen und erklärende Architekturtexte. |
| [`08_engineering-guidelines/`](08_engineering-guidelines/) | Implementierungsnahe Regeln für Java, Testing, Persistence, Resilience und Delivery. |

## Wie die Artefakte zusammenwirken

```text
Principle / Policy
        ↓
Decision Guide
        ↓
konkreter Systemkontext
        ↓
Architecture Decision Record
        ↓
Standard / Reference Architecture / Ausnahme
        ↓
Engineering Guideline / Golden Path
        ↓
Automated Control / Test / Fitness Function
        ↓
Runtime Evidence
        ↓
Review / Learning / Evolution
```

Nicht jede Entscheidung erzeugt einen neuen Standard. Nicht jeder Standard braucht in jedem Projekt ein eigenes ADR. Entscheidend ist, dass Verantwortung, Geltungsbereich und Nachweis klar sind.

## Kennungen

Wissensartefakte besitzen eine stabile `AK-*`-Kennung, zum Beispiel:

```text
AK-021  REST API Standard
AK-041  Event-Driven Architecture Decision Guide
AK-075  Architecture Decision Process
```

Historische Kennungen werden bei Bedarf nur noch im Metadatum `legacy_ids` geführt.

Echte Entscheidungen verwenden einen getrennten ADR-Namensraum, zum Beispiel:

```text
ADR-2026-001
ADR-2026-002
```

Damit bleibt klar, ob ein Dokument allgemeines Wissen oder eine tatsächlich getroffene Entscheidung beschreibt.

## Qualitätsgrundsätze

- Architekturentscheidungen folgen Problemen, Qualitätszielen und Constraints – nicht Technologiepräferenzen.
- Ein ADR dokumentiert genau eine wesentliche konkrete Entscheidung.
- Optionen und Trade-offs werden fair beschrieben.
- Standards unterscheiden verbindliche Regeln von Beispielen und Empfehlungen.
- Zeitabhängige technische Aussagen werden gegen geeignete Primärquellen geprüft.
- Zahlenwerte sind nur dann verbindlich, wenn sie aus Anforderungen, Messungen oder einer formalen Policy abgeleitet sind.
- Security-, Privacy- und Betriebsanforderungen werden nicht als nachgelagerte Ergänzung behandelt.
- Automatisierbare Regeln werden soweit sinnvoll durch Tests, Policy Checks, Fitness Functions oder andere Evidence überprüfbar gemacht.
- Akzeptierte ADRs werden nicht rückwirkend umgeschrieben; geänderte Entscheidungen werden nachvollziehbar ersetzt.
- Generische Wissensdokumente erhalten keine erfundenen Gremien, Entscheider oder Projektergebnisse.

## Einstieg nach Fragestellung

### Architekturentscheidungen treffen

1. [`00_governance/ARTIFACT-MODEL.md`](00_governance/ARTIFACT-MODEL.md)
2. [`00_governance/ADR-LIFECYCLE-AND-TEMPLATE.md`](00_governance/ADR-LIFECYCLE-AND-TEMPLATE.md)
3. [`07_learning-guides/AK-082-quality-goals-and-scenarios.md`](07_learning-guides/AK-082-quality-goals-and-scenarios.md)
4. passende Decision Guides unter [`02_decision-guides/`](02_decision-guides/)
5. [`06_operating-models/AK-075-architecture-decision-process.md`](06_operating-models/AK-075-architecture-decision-process.md)

### Softwarearchitektur und Domänenschnitt

- [`01_principles/AK-084-coupling-cohesion-information-hiding.md`](01_principles/AK-084-coupling-cohesion-information-hiding.md)
- [`02_decision-guides/AK-023-domain-driven-design.md`](02_decision-guides/AK-023-domain-driven-design.md)
- [`02_decision-guides/AK-031-hexagonal-architecture.md`](02_decision-guides/AK-031-hexagonal-architecture.md)
- [`02_decision-guides/AK-077-modulith-vs-microservices.md`](02_decision-guides/AK-077-modulith-vs-microservices.md)
- [`04_reference-architectures/AK-056-modular-monolith.md`](04_reference-architectures/AK-056-modular-monolith.md)

### Integration und Schnittstellen

- [`03_standards/AK-021-rest-api-standard.md`](03_standards/AK-021-rest-api-standard.md)
- [`02_decision-guides/AK-041-event-driven-architecture.md`](02_decision-guides/AK-041-event-driven-architecture.md)
- [`03_standards/AK-066-api-first-openapi.md`](03_standards/AK-066-api-first-openapi.md)
- [`03_standards/AK-095-asyncapi-event-contracts.md`](03_standards/AK-095-asyncapi-event-contracts.md)
- [`03_standards/AK-110-api-lifecycle-deprecation.md`](03_standards/AK-110-api-lifecycle-deprecation.md)

### Security und Privacy

- [`03_standards/AK-015-application-security-baseline.md`](03_standards/AK-015-application-security-baseline.md)
- [`02_decision-guides/AK-040-oauth2-oidc-token-architecture.md`](02_decision-guides/AK-040-oauth2-oidc-token-architecture.md)
- [`03_standards/AK-101-spring-security-resource-server.md`](03_standards/AK-101-spring-security-resource-server.md)
- [`03_standards/AK-106-privacy-technical-controls.md`](03_standards/AK-106-privacy-technical-controls.md)
- [`03_standards/AK-118-browser-security-headers.md`](03_standards/AK-118-browser-security-headers.md)

### Platform, Delivery und Betrieb

- [`04_reference-architectures/AK-036-ci-cd-reference-pipeline.md`](04_reference-architectures/AK-036-ci-cd-reference-pipeline.md)
- [`04_reference-architectures/AK-080-devsecops-controls.md`](04_reference-architectures/AK-080-devsecops-controls.md)
- [`04_reference-architectures/AK-102-opentelemetry.md`](04_reference-architectures/AK-102-opentelemetry.md)
- [`04_reference-architectures/AK-114-gitops-argocd.md`](04_reference-architectures/AK-114-gitops-argocd.md)
- [`05_operating-guides/AK-072-chaos-engineering.md`](05_operating-guides/AK-072-chaos-engineering.md)

### Engineering-Qualität

- [`08_engineering-guidelines/`](08_engineering-guidelines/)
- [`03_standards/AK-096-test-strategy.md`](03_standards/AK-096-test-strategy.md)
- [`03_standards/AK-061-architecture-fitness-functions.md`](03_standards/AK-061-architecture-fitness-functions.md)

## Validierung

Die Regeln für Quellen, technische Baselines, Zahlenwerte und Review-Trigger stehen in:

- [`00_governance/VALIDATION-POLICY.md`](00_governance/VALIDATION-POLICY.md)

Die fachliche Priorität lautet grundsätzlich:

1. normative oder offizielle Primärquelle,
2. anerkannte Fachliteratur,
3. Praxisheuristik mit klarer Kennzeichnung.

## Anspruch des Compendiums

Das Compendium soll nicht den Eindruck erzeugen, jede aufgeführte Technologie sei in jedem Kontext die richtige Wahl. Es zeigt vielmehr, wie technische und organisatorische Architekturfragen strukturiert, begründet, operationalisiert und überprüft werden können.
