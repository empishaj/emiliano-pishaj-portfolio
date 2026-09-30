---
id: AK-083
legacy_ids:
  - ADR-083
title: Architekturmuster problemorientiert auswählen
artifact_type: learning-guide
domain: architecture-fundamentals
status: active
maturity: reviewed
normative_level: informative
last_validated: 2026-09-28
review_trigger:
  - wesentliche Erweiterung des Pattern-Katalogs
---

# AK-083 — Architekturmuster sind Optionen, keine Zielbilder

## 1. Kernregel

> **Kein Muster ohne Problem, Qualitätsziel und Kontext.**

Pattern-Namen sind Abkürzungen für wiederkehrende Lösungsstrukturen und deren Trade-offs.

Sie sind keine automatische Empfehlung.

## 2. Die Pattern-first-Falle

Schlecht:

```text
"Wir wollen Event Sourcing lernen. Wo können wir es einsetzen?"
```

Besser:

```text
"Wir brauchen eine vollständig rekonstruierbare fachliche Historie.
Welche Architekturoptionen erfüllen das und zu welchem Preis?"
```

Erst danach kann Event Sourcing eine Option sein.

## 3. Pattern-Bewertungsformel

Für jedes Muster:

```text
Problem
+ Forces / Constraints
+ Quality Goals
→ Pattern Candidate
→ Benefits
→ Liabilities
→ Evidence Needed
```

## 4. Patternfamilien

### Struktur

- Layered Architecture,
- Hexagonal,
- Modular Monolith,
- Microservices.

### Integration

- Request/Response,
- Event-Driven,
- Saga,
- Outbox.

### Daten

- CQRS,
- Event Sourcing,
- Cache,
- Read Model.

### Transformation

- Strangler,
- Branch by Abstraction,
- Expand/Contract.

### Resilience

- Retry,
- Timeout,
- Circuit Breaker,
- Bulkhead.

Die Katalogisierung hilft beim Denken, ersetzt aber keine Entscheidung.

## 5. Pattern-Komposition

Patterns treten häufig gemeinsam auf.

Beispiel:

```text
Microservice
+ REST
+ Outbox
+ Kafka
+ Saga
+ Observability
```

Jedes zusätzliche Pattern bringt eigene Betriebs- und Governancekosten.

Darum muss die **Gesamtkomplexität der Kombination** bewertet werden, nicht jedes Pattern isoliert.

## 6. Quality Trade-offs

Beispiel Event-Driven Architecture:

```text
+ zeitliche Entkopplung
+ mehrere Consumer
+ Resilience gegen temporäre Consumer-Ausfälle

- Eventual Consistency
- komplexere Diagnose
- Schema-/Lifecycle-Governance
- Idempotenz / Retry
```

Ein Pattern ist stark, wenn seine Nachteile im aktuellen Kontext tragfähig sind.

## 7. Teamfähigkeit

Ein Pattern ist nur so tragfähig wie die Organisation, die es betreiben kann.

Prüfen:

- versteht das Team Failure Modes?
- existiert Observability?
- kann der Betrieb das Muster unterstützen?
- existieren Standards und Golden Paths?
- wer besitzt Entscheidungen und Ausnahmen?

## 8. Evidence

Vor einer weitreichenden Einführung können sinnvoll sein:

- Spike,
- Lasttest,
- Failure-Test,
- Architekturreview,
- Kostenmodell,
- Pilot.

## 9. Anti-Patterns

- Microservices weil „skalierbar“.
- Event Sourcing weil Audit wichtig klingt.
- CQRS weil Reads und Writes im Code getrennt sind.
- Hexagonal Architecture mit fünf Schichten für triviales CRUD.
- Patternkatalog als Reifegradmodell: mehr Patterns ≠ bessere Architektur.

## 10. Coach-Merksatz

> Ein Pattern ist nicht die Antwort. Es ist **eine bekannte Antwort auf eine bestimmte Kräfteverteilung**. Seniorität zeigt sich darin, zu erkennen, wann diese Kräfte wirklich vorhanden sind.
