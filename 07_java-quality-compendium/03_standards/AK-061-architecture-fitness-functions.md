---
id: AK-061
legacy_ids:
  - ADR-061
title: Architecture Fitness Functions und automatisierte Guardrails
artifact_type: governance-standard
domain: architecture-governance
status: active
maturity: reviewed
normative_level: recommended
owner_role: Architecture Governance
last_validated: 2026-09-28
review_trigger:
  - neue Architekturstandards
  - hohe False-Positive-Rate bestehender Gates
  - Architecture Drift Incident
---

# AK-061 — Architecture Fitness Functions

## 1. Kernidee

Eine Architecture Fitness Function überprüft eine gewünschte Architektureigenschaft wiederholbar.

Damit wird aus:

> „Bitte haltet die Architektur ein.“

möglicherweise:

```text
Architektureigenschaft
→ prüfbare Regel
→ automatisierter Check
→ Feedback im Delivery-Prozess
```

Aber nicht jede Architektureigenschaft ist automatisch testbar.

## 2. Was sich gut automatisieren lässt

### Struktur

- keine verbotenen Package-/Modulabhängigkeiten,
- keine Zyklen,
- Domain kennt bestimmte Frameworkpackages nicht.

### Verträge

- OpenAPI-/AsyncAPI-Kompatibilität,
- Schema-Linting,
- Contract Tests.

### Security / Supply Chain

- keine bekannten Findings über definierter Policy,
- keine Secrets,
- SBOM vorhanden,
- Container-/IaC-Policies.

### Performance

- messbare Szenarien unter definierter Last.

### Documentation Hygiene

- fehlende Owner,
- gebrochene Links,
- Review-Due-Metadaten.

## 3. Was nicht sinnvoll vollständig automatisiert wird

- fachliche Passung,
- Qualität einer Domänengrenze,
- strategische Relevanz einer Capability,
- richtige Organisationsverantwortung,
- Wirtschaftlichkeit einer Zielarchitektur,
- Akzeptanz eines Trade-offs.

Automatisierung ist kein Ersatz für Architektururteil.

## 4. Von Quality Scenario zur Fitness Function

```text
Concern
→ Quality Scenario
→ Architecture Property
→ Observable Signal
→ Check
```

Beispiel:

```text
Quality:
Modifizierbarkeit

Property:
Domain ist frameworkunabhängig

Signal:
keine Imports aus Spring/JPA im Domain-Modul

Check:
ArchUnit / Dependency Rule
```

## 5. Keine universellen Schwellenwerte

Nicht:

```text
Coverage > 80%
Mutation > 70%
P95 < 500ms
```

als globale Fitness Functions ohne Kontext.

Sondern:

- Schwellenwerte aus Quality Scenarios,
- Risikoklassen,
- Teams-/Produktkontext,
- gemessenen Baselines

ableiten.

## 6. Blocking vs. Informational

Nicht jeder Check muss einen Merge verhindern.

### Blocking

Geeignet bei klaren, stabilen Regeln mit hoher Relevanz.

Beispiele:

- Secret gefunden,
- verbotene Modulabhängigkeit,
- inkompatibler Contract Change ohne Version/Lifecycle.

### Warning / Trend

Geeignet bei heuristischen Signalen.

Beispiele:

- steigende Komplexität,
- Coverage-Trend,
- technische Schulden,
- Performance nahe Budget.

Ein zu aggressives Gate erzeugt Umgehungsverhalten.

## 7. Ownership

Jede wichtige Fitness Function braucht:

- Owner,
- Rationale,
- referenzierten Standard/ADR,
- Ausnahmeweg,
- Review-Trigger.

Sonst bleibt nach zwei Jahren ein Build Gate bestehen, dessen ursprünglicher Zweck niemand mehr kennt.

## 8. Evolution

Fitness Functions selbst altern.

Sie werden überprüft, wenn:

- Architekturentscheidung ersetzt wird,
- neue Plattform eingeführt wird,
- zu viele False Positives entstehen,
- Teams regelmäßig Ausnahmen benötigen,
- Qualitätsziel sich ändert.

## 9. Beispielkatalog

| Eigenschaft | mögliche Evidence |
|---|---|
| Modulgrenzen | ArchUnit / Modulprüfung |
| API-Kompatibilität | OpenAPI Diff / Contract Test |
| Event Compatibility | Schema Registry / AsyncAPI Check |
| Supply Chain | SBOM/SCA/Provenance |
| Performance | Lasttest / SLI |
| Recovery | Restore-/DR-Test |
| GitOps Compliance | Drift/Policy Check |
| Privacy Logging | Telemetry-/Log-Test |

## 10. Coach-Merksatz

> Eine Fitness Function ist dann wertvoll, wenn sie **eine wirklich wichtige Architektureigenschaft schnell und zuverlässig sichtbar macht** – nicht weil „mehr Gates“ automatisch bessere Governance bedeuten.
