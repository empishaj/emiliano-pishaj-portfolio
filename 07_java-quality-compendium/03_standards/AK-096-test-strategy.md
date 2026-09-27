---
id: AK-096
legacy_ids:
  - ADR-096
title: Teststrategie als Quality Evidence
artifact_type: quality-standard
domain: testing
status: active
maturity: reviewed
normative_level: recommended
owner_role: Engineering / QA Architecture
last_validated: 2026-09-28
review_trigger:
  - wiederkehrende Produktionsfehler trotz grüner Tests
  - wesentliche Architektur-/Delivery-Änderung
---

# AK-096 — Teststrategie als risikobasierte Evidenz

## 1. Ziel

Eine Teststrategie beantwortet nicht:

> „Wie viel Coverage brauchen wir?“

Sondern:

> **Welche Risiken müssen auf welcher Ebene mit welcher Art Evidenz kontrolliert werden?**

Daraus folgt:

```text
Risiko
→ geeignete Testebene
→ schnelle Feedbackschleife
→ zusätzliche Integrationsevidence wo nötig
```

## 2. Testebenen

### Static Analysis

Findet bestimmte Code-/Dependency-/Policy-Verstöße ohne Ausführung.

### Unit Tests

Schnelles Feedback für fachliche Regeln und isolierte Logik.

### Component / Slice Tests

Prüfen gezielt Frameworkintegration oder Komponentengrenzen.

### Integration Tests

Prüfen reale Technologien beziehungsweise Integrationen, zum Beispiel Datenbank oder Broker.

### Contract Tests

Prüfen Verträge zwischen Consumer und Provider.

### End-to-End Tests

Prüfen ausgewählte kritische Nutzer-/Prozesspfade über mehrere Komponenten.

### Non-functional Tests

Performance, Security, Recovery, Chaos oder andere Qualitätsattribute.

## 3. Testpyramide nicht dogmatisch verstehen

Das bekannte Pyramidenmodell ist eine Heuristik:

- viele schnelle, lokale Tests,
- weniger teure systemübergreifende Tests.

Moderne Systeme können zusätzliche Ebenen benötigen, etwa Contract Tests oder Component Tests.

Entscheidend ist:

> schnelle und zuverlässige Evidence dort erzeugen, wo der Fehler am günstigsten entdeckt werden kann.

## 4. Coverage

Coverage zeigt, welcher Code während Tests ausgeführt wurde.

Coverage beweist nicht:

- dass Assertions sinnvoll sind,
- dass Grenzfälle getestet wurden,
- dass fachliche Invarianten geschützt sind,
- dass Architektur korrekt ist.

Darum werden Coverage-Schwellen nicht organisationsweit ohne Kontext als Qualitätsbeweis verwendet.

Coverage kann als Signal und Mindest-Hygiene dienen.

## 5. Mutation Testing

Mutation Testing kann prüfen, ob Tests bestimmte künstliche Verhaltensänderungen erkennen.

Es kann wertvoll sein für:

- kritische Domänenlogik,
- Libraries,
- hochwertige Unit-Test-Suites.

Ein globaler Mutation-Score ist kein universeller Standard.

## 6. Testdesign nach Failure Mode

Beispiel:

```text
Risiko:
Event wird doppelt verarbeitet

Evidence:
Idempotenz-Integrationstest

Risiko:
Provider entfernt API-Feld

Evidence:
Contract-/Schema-Compatibility-Test

Risiko:
Restore dauert zu lange

Evidence:
Recovery Drill, nicht JUnit
```

Der letzte Punkt ist entscheidend: Nicht jede Qualität wird durch Code-Tests bewiesen.

## 7. Testdaten

Tests SOLLEN:

- deterministisch sein, wenn Reproduzierbarkeit erforderlich ist,
- relevante fachliche Szenarien ausdrücken,
- personenbezogene Produktionsdaten vermeiden,
- Aufbaukosten reduzieren.

Siehe AK-119.

## 8. Flaky Tests

Flaky Tests sind Governance-Schulden.

Sie führen dazu, dass Teams rote Pipelines ignorieren.

SOLLTE:

- Flakiness messen,
- Ursache beheben,
- Quarantäne nur zeitlich begrenzt mit Owner verwenden.

## 9. CI-Strategie

Schnelle Tests früh, teure Tests gezielt später.

Beispiel:

```text
PR
→ Unit + Static + Architecture
→ Component/Contract
→ Integration
→ Artifact
→ ausgewählte E2E/Performance/Security
```

Das konkrete Modell folgt Risikoklasse und Delivery-Frequenz.

## 10. Quality Evidence Matrix

| Risiko | primäre Evidence |
|---|---|
| Domain-Invariante | Unit / Property Test |
| Persistence Mapping | Integration Test |
| API Contract | Contract Test |
| Event Schema | Compatibility Test |
| Modulgrenze | Architecture Test |
| Performance | Load Test |
| Recovery | Restore/DR Test |
| Security Architecture | Review + Security Tests/Scans |
| Browser Workflow | ausgewählte E2E Tests |

## 11. Anti-Patterns

- 100 % Coverage als Qualitätsziel.
- große E2E-Suite als Ersatz für lokale Tests.
- Mocking der eigenen Datenstrukturen bis der Test keine reale Semantik mehr hat.
- Integrationstest gegen dauerhaft instabile Shared Environment.
- Tests, die Implementierungsdetails statt Verhalten fixieren.

## 12. Coach-Merksatz

> Die richtige Testfrage lautet nicht „Welche Testart fehlt uns?“, sondern: **Welches Risiko wollen wir mit welcher kosteneffizienten Evidence früh genug sichtbar machen?**
