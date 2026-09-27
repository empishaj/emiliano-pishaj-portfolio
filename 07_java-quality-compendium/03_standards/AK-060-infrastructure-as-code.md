---
id: AK-060
legacy_ids:
  - ADR-060
title: Infrastructure as Code Governance
artifact_type: architecture-standard
domain: platform-delivery
status: active
maturity: reviewed
normative_level: normative
owner_role: Platform Engineering
last_validated: 2026-09-28
review_trigger:
  - Wechsel des IaC- oder Cloud-Plattformmodells
  - Infrastructure Drift / Security Incident
---

# AK-060 — Infrastructure as Code Governance

## 1. Zweck

Infrastruktur ist Architekturzustand.

Wenn produktive Infrastruktur nur aus manuellen Konsolenaktionen und implizitem Wissen besteht, fehlen:

- Reproduzierbarkeit,
- Reviewbarkeit,
- Drift-Erkennung,
- nachvollziehbare Historie,
- Recovery-Fähigkeit.

IaC macht Änderungen als versionierbare Artefakte behandelbar.

## 2. Normative Regeln

Produktive Infrastruktur SOLL soweit technisch sinnvoll deklarativ beziehungsweise automatisiert beschrieben werden.

MUSS:

- Änderungen reviewbar machen,
- Secrets vom IaC-Code trennen,
- State/Backend schützen,
- Rechte für Plan/Apply kontrollieren,
- manuelle Änderungen entweder verhindern oder in Source of Truth zurückführen.

## 3. Toolrollen trennen

### Provisioning

Beispiel Terraform/OpenTofu: Cloud-/Infrastrukturressourcen und gewünschter Zustand.

### Configuration Management

Beispiel Ansible: Betriebssystem-/Softwarekonfiguration und prozedurale Orchestrierung.

### Kubernetes Packaging

Helm/Kustomize: Workload-/Plattformmanifest-Organisation.

### GitOps

Operating Model für deklarativen Desired State und kontinuierliche Reconciliation.

Diese Werkzeuge lösen unterschiedliche Probleme und sollten nicht begrifflich vermischt werden.

## 4. State ist kritischer Bestandteil

Bei state-basierten IaC-Werkzeugen MUSS geklärt sein:

- Remote Storage,
- Zugriffsschutz,
- Locking/Concurrency,
- Backup/Recovery,
- Verschlüsselung,
- sensible Werte im State.

Der IaC-Code allein reicht nicht, um Infrastruktur zu rekonstruieren, wenn benötigter State verloren oder kompromittiert ist.

## 5. Plan vor Apply

Änderungen SOLLEN vor produktiver Anwendung eine nachvollziehbare Vorschau/Planung erzeugen.

Review fragt:

- Welche Ressourcen werden neu erzeugt?
- Was wird verändert?
- Was wird gelöscht/ersetzt?
- Welche IAM-/Netz-/Security-Auswirkungen entstehen?
- Gibt es irreversible Datenfolgen?

Ein großer, unverständlicher Plan ist ein Governance-Signal: Scope eventuell schneiden.

## 6. Module

Module SOLLEN reale wiederverwendbare Infrastrukturstandards kapseln.

Nicht jede Ressource braucht eine eigene generische Abstraktion.

Gute Module verbergen unter anderem:

- sichere Defaults,
- Logging/Monitoring-Anbindung,
- Tagging/Ownership,
- Netzwerk-/IAM-Leitplanken.

## 7. Drift

Drift ist die Differenz zwischen gewünschtem und tatsächlichem Zustand.

Mögliche Strategie:

- manuelle Produktionseingriffe verbieten,
- Drift regelmäßig erkennen,
- Notfalländerungen dokumentieren und zurückführen,
- bei GitOps kontinuierlich reconciliieren.

Nicht jede Plattform kann vollständig immutable betrieben werden; der Ausnahmeweg muss dann explizit sein.

## 8. Policy as Code

Bestimmte Infrastructure Controls können automatisiert geprüft werden:

- keine öffentliche Datenbank,
- Verschlüsselung aktiviert,
- Pflicht-Tags/Owner,
- erlaubte Regionen,
- keine überbreiten IAM-Rollen,
- Logging aktiviert.

Policies müssen mit realem Risiko und Verantwortlichkeit verbunden sein, sonst werden sie nur ein weiteres Build-Hindernis.

## 9. Evidence

- Git-Historie,
- Review/Approval,
- Plan Output,
- Policy Check,
- Deployment/Apply Log,
- Drift Report,
- Cloud Audit Log.

## 10. Behörden-/Enterprise-Perspektive

IaC ist besonders wertvoll für:

- standardisierte Dienstleisterlieferungen,
- nachvollziehbare Änderungen,
- Wiederherstellung,
- Trennung von Zuständigkeiten,
- Security-Nachweise,
- reproduzierbare Umgebungen.

Aber der Code ersetzt nicht Betriebsmodell, Schutzbedarfsprüfung oder Freigabeprozesse.

## 11. Coach-Merksatz

> Infrastructure as Code ist nicht „Terraform benutzen“. Es bedeutet, Infrastrukturänderungen **wie kontrollierte Architekturänderungen versionierbar, reviewbar, reproduzierbar und evidenzfähig zu machen**.
