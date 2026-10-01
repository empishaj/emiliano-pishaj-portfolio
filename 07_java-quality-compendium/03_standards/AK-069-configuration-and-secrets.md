---
id: AK-069
legacy_ids:
  - ADR-069
title: Konfiguration und Secrets Management
artifact_type: architecture-standard
domain: platform-security
status: active
maturity: reviewed
normative_level: normative
owner_role: Platform Engineering / Security Architecture
last_validated: 2026-10-01
review_trigger:
  - Wechsel des Secret-Management-Systems
  - Credential-/Configuration-Incident
---

# AK-069 — Code, Konfiguration und Secrets trennen

## 1. Kernmodell

```text
Code
≠
Configuration
≠
Secret
```

### Code

Versionierte Programmlogik.

### Configuration

Umgebungs- oder deploymentspezifische Parameter, die kein Geheimnis darstellen.

### Secret

Information, deren Offenlegung Zugriff oder Sicherheitswirkung erzeugt, zum Beispiel Passwörter, Private Keys oder Client Secrets.

## 2. Normative Regeln

### Code

MUSS:

- ohne eingebettete produktive Zugangsdaten auslieferbar sein,
- sichere Defaultwerte verwenden, soweit möglich,
- bei fehlender sicherheitskritischer Konfiguration fail fast reagieren.

### Konfiguration

SOLL:

- extern zum Build-Artefakt verwaltet werden,
- schema-/typvalidiert werden,
- nachvollziehbar versioniert sein, soweit sie nicht geheim ist,
- zwischen Umgebungen nur dort abweichen, wo ein tatsächlicher Grund existiert.

### Secrets

MÜSSEN:

- aus einem geeigneten Secret Store / Workload-Identity-System bezogen werden,
- nach Least Privilege ausgegeben werden,
- rotierbar sein,
- aus Logs, Fehlermeldungen und Artefakten ferngehalten werden.

DÜRFEN NICHT:

- in Git eingecheckt werden,
- in Container Images eingebettet werden,
- als langlebige Klartextwerte in Wiki oder Tickets verteilt werden.

## 3. Sichere Defaults

Ein fehlender Parameter darf nicht still zu einem unsicheren Betriebsmodus führen.

Beispiele:

```text
TLS-Konfiguration fehlt
→ Start verweigern

Authentisierungs-Issuer fehlt
→ Start verweigern

Environment unbekannt
→ keine Debug-/Development-Funktion automatisch aktivieren
```

Sichere Defaults sind besonders wichtig bei Legacy- und Nicht-Spring-Systemen.

## 4. Statische vs. dynamische Konfiguration

### statisch / Deployment Configuration

Ändert sich mit Deployment und wird typischerweise über GitOps/Configuration Management ausgerollt.

### dynamisch

Ändert sich zur Laufzeit, zum Beispiel Feature Flags oder bestimmte Policywerte.

Dynamische Konfiguration braucht zusätzlich:

- Ownership,
- Audit Trail,
- Validierung,
- Rollback,
- Cache-/Propagation-Semantik.

Nicht jede Einstellung sollte dynamisch gemacht werden.

## 5. Secrets und Workload Identity

Wo Plattform und IAM es erlauben, sind kurzlebige, automatisch ausgestellte Credentials langlebigen statischen Secrets vorzuziehen.

Architekturfragen:

- Wer stellt Identität aus?
- wie wird Workload authentisiert?
- wie kurz ist die Credential-Lebensdauer?
- wie erfolgt Rotation?
- wie funktioniert Recovery?
- wie wird Nutzung auditiert?

## 6. Configuration Drift

Drift entsteht, wenn Konfiguration außerhalb des führenden Prozesses geändert wird.

Geeignete Maßnahmen:

- GitOps / Desired State,
- Policy Checks,
- automatische Reconciliation,
- Configuration Inventory,
- Änderungsprotokoll.

Notfalländerungen brauchen einen definierten Rückweg in die führende Quelle.

## 7. Datenschutz

Auch Konfiguration kann personenbezogene oder schutzwürdige Informationen enthalten.

Beispiele:

- Empfängeradressen,
- technische Konten mit Personenbezug,
- Mandantenidentifikatoren.

Secret Management ist nicht automatisch Privacy Management.

## 8. Verifikation

- Secret Scan im Repository und Build,
- Starttests bei fehlender Pflichtkonfiguration,
- Policy Check auf unerlaubte Klartext-Secrets,
- Rotationstest,
- Audit der Secret-Zugriffe,
- Drift Detection.

## 9. Merksatz

> Konfigurierbar bedeutet nicht beliebig. Ein gutes Configuration Model macht **Unterschiede zwischen Umgebungen explizit, Secrets kurzlebig und Änderungen nachvollziehbar**.
