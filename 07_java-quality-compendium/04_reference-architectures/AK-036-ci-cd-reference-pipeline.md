---
id: AK-036
legacy_ids:
  - QG-JAVA-036
title: CI/CD Reference Pipeline
artifact_type: reference-architecture
domain: delivery
status: active
maturity: reviewed
normative_level: recommended
owner_role: Platform Engineering
last_validated: 2026-10-01
review_trigger:
  - Wechsel der zentralen CI/CD-Plattform
  - Delivery-/Supply-Chain-Incident
---

# AK-036 — CI/CD als Delivery- und Evidence-Pipeline

## 1. Ziel

Eine CI/CD-Pipeline soll nicht nur Software schneller ausrollen. Sie soll reproduzierbar belegen, **welcher Source-Stand welche Prüfungen bestanden hat und welches unveränderliche Artefakt in welche Umgebung gelangt ist**.

Referenzfluss:

```text
Change
→ Build
→ Unit / Static Checks
→ Integration / Contract
→ Security / Supply Chain
→ Artifact + SBOM
→ Promotion
→ Deployment
→ Smoke / Health
→ Runtime Evidence
```

Die konkrete Pipeline variiert nach Systemrisiko.

## 2. Build once, promote

Ein Release-Artefakt SOLLTE einmal gebaut und anschließend zwischen Umgebungen promotet werden.

Nicht:

```text
staging build ≠ production build
```

sondern:

```text
identisches Artifact Digest
+ environment-spezifische externe Configuration
```

Das verbessert Reproduzierbarkeit und Supply-Chain-Nachweis.

## 3. Stufen nach Risiko

### Commit/PR

Typisch:

- Compile,
- Unit Tests,
- Linting,
- Architecture Checks,
- Secret Detection,
- SAST/SCA soweit schnell genug.

### Integrationsstufe

- DB-/Broker-/Service Integration,
- Contract Tests,
- Migration Tests,
- Container Build/Scan.

### Release

- immutable Artifact,
- SBOM,
- Provenance/Metadata,
- Signierung je Policy,
- Release Notes.

### Deployment

- kontrollierte Promotion,
- Policy/Approval je Risikoklasse,
- Health/Smoke Tests,
- Rollback-/Roll-forward-Fähigkeit.

## 4. Gate-Prinzip

Ein Gate MUSS beantworten:

- welches Risiko wird kontrolliert?
- welches Signal entscheidet?
- wer kann eine Ausnahme genehmigen?
- wie lange gilt die Ausnahme?

Ein Tool-Scan ohne Entscheidungsregel ist nur ein Report.

## 5. Umgebungen

Umgebungen SOLLEN sich möglichst in Daten, Größe und externen Endpunkten unterscheiden – nicht in fundamental anderer Softwarelogik.

Environment-spezifische Unterschiede werden explizit konfiguriert und getestet.

## 6. Secrets und Berechtigungen

Pipeline-Identitäten besitzen hohes Risiko.

MUSS:

- Least Privilege,
- getrennte Rechte je Umgebung,
- geschützte produktive Credentials,
- nachvollziehbare Approvals,
- keine Secrets in Logs/Artifacts.

## 7. Deploymentstrategie

CI/CD ist nicht gleich Deploymentstrategie.

Je Kontext:

- Rolling,
- Blue/Green,
- Canary,
- Feature Flags.

Die Strategie richtet sich nach Risiko, Datenmigration und Observability.

## 8. Rollback vs Roll-forward

Vor Release wird geklärt:

- kann altes Binary mit neuem Schema laufen?
- sind Messages rückwärtskompatibel?
- kann ein Deployment technisch zurückgerollt werden?
- ist Roll-forward sicherer?

Ein Pipeline-Button „rollback“ garantiert keine fachliche Reversibilität.

## 9. Evidence

Ein Release SOLLTE nachvollziehbar verbinden:

```text
Commit
→ Pipeline
→ Test-/Scan-Ergebnisse
→ Artifact Digest
→ SBOM
→ Deployment
→ Runtime Version
```

## 10. Anti-Patterns

- Pipeline = lange Shell-Datei ohne Ownership.
- jedes Toolfinding blockiert ungeachtet des Risikos.
- Production wird neu gebaut statt promotet.
- manuelle Hotfixes ohne Rückführung in Source of Truth.
- grüne Pipeline wird mit produktiver Betriebsfähigkeit verwechselt.

## 11. Quellen

- AK-057 — Software Supply Chain
- AK-080 — DevSecOps Reference Architecture
- AK-114 — GitOps
- DORA / Accelerate — Delivery Performance als Lernkontext

## 12. Merksatz

> Eine reife Pipeline ist keine Automatisierungsstrecke. Sie ist eine **kontrollierte Nachweiskette von Änderung zu geprüftem Artefakt zu beobachtbarem Deployment**.
