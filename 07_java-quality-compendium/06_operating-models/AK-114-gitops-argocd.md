---
id: AK-114
legacy_ids:
  - ADR-114
title: GitOps und Argo CD als deklaratives Deployment Operating Model
artifact_type: operating-model
domain: platform-delivery
status: active
maturity: reviewed
normative_level: recommended
owner_role: Platform Engineering
last_validated: 2026-10-01
review_trigger:
  - neue OpenGitOps-Prinzipien
  - Argo-CD-Major-Version
  - wiederkehrende Production Drift
---

# AK-114 — GitOps und Argo CD

## 1. Erst das Prinzip, dann das Produkt

GitOps wird im Compendium nicht als Synonym für Argo CD definiert.

OpenGitOps beschreibt vier zentrale Prinzipien:

1. Desired State ist deklarativ.
2. Desired State ist versioniert und unveränderlich gespeichert.
3. Software-Agenten ziehen den gewünschten Zustand automatisch.
4. Der tatsächliche Zustand wird kontinuierlich reconciliiert.

Argo CD ist eine mögliche konkrete Implementierung für Kubernetes.

## 2. Zielbild

```text
Application / Platform Change
        ↓
Pull Request
        ↓
Git = Desired State
        ↓
GitOps Controller
        ↓
Cluster
        ↓
Reconciliation / Drift Detection
```

Damit wird Deployment von einem einmaligen imperativen Befehl zu einem kontinuierlichen Zustandsabgleich.

## 3. Source of Truth

Für vom GitOps-Modell verwaltete Ressourcen MUSS klar sein:

> Der gewünschte Zustand wird in der autorisierten Git-Quelle geändert – nicht dauerhaft direkt im Cluster.

Manuelle Notfalländerungen brauchen einen definierten Break-Glass-Prozess und müssen anschließend in die führende Quelle zurückgeführt werden.

## 4. Repository-Struktur

Ein mögliches Modell:

```text
gitops/
├─ applications/
├─ environments/
│  ├─ staging/
│  └─ production/
└─ platform/
```

Die konkrete Struktur ist nicht normativ.

Sie soll jedoch sichtbar machen:

- Ownership,
- Umgebungen,
- Workload vs. Plattform,
- Promotionpfad.

## 5. Argo CD Auto-Sync bewusst konfigurieren

Argo CD kann Änderungen automatisiert synchronisieren und optional Self-Heal/Prune verwenden.

Diese Funktionen werden nicht pauschal aktiviert.

Prüffragen:

- Welche Ressourcen dürfen automatisch gelöscht werden?
- Welche Runtime-Controller ändern Felder legitimerweise?
- Soll Drift automatisch korrigiert oder zunächst alarmiert werden?
- Welche produktiven Changes benötigen Approval?

Die historische Guideline behandelte `selfHeal: true` als universelle Vorgabe. Im neuen Modell ist dies eine **kontextabhängige Policy-Entscheidung**.

## 6. Drift

Drift kann entstehen durch:

- `kubectl`-Änderungen,
- Operatoren/Controller,
- HPA,
- Cloud-Controller,
- fehlerhafte Ignore-Regeln,
- externe Prozesse.

Nicht jede Abweichung ist unerlaubte Drift. Das GitOps-Modell muss deklarieren, welche Felder von anderen Controllern „owned“ werden.

## 7. Promotion

Bevorzugt wird eine nachvollziehbare Promotion des bereits gebauten Artefakts.

```text
Image Digest
→ Staging Desired State
→ Evidence
→ Production PR
→ Production Desired State
```

Das verhindert, dass Produktion aus einem anderen Build entsteht als die getestete Version.

## 8. Rollback

`git revert` kann Desired State zurücksetzen.

Aber:

> Git-Revert ist nicht automatisch fachlicher Rollback.

Problemfälle:

- irreversible DB-Migration,
- inkompatibles Event Schema,
- externe Seiteneffekte,
- gelöschte Daten.

Darum müssen Deployment- und Data-Lifecycle gemeinsam betrachtet werden.

## 9. Zugriffssteuerung

GitOps verschiebt Macht in:

- Git-Repositories,
- Pull-Request-Rechte,
- Controller Service Accounts,
- Argo Projects / Cluster Credentials.

MUSS:

- Least Privilege,
- geschützte Branches/Approvals nach Risiko,
- getrennte produktive Rechte,
- Auditierbarkeit.

## 10. Multi-Cluster

Bei mehreren Clustern muss geklärt werden:

- zentraler oder verteilter Controller,
- Blast Radius,
- Cluster Credentials,
- Tenant-/Environment-Isolation,
- Plattform- vs. Application Ownership.

„App of Apps“ oder ApplicationSets sind Implementierungsmuster, keine universellen Architekturregeln.

## 11. Evidence

- Git Commit/PR,
- gewünschter Artifact Digest,
- Sync Status,
- Health Status,
- Drift-/OutOfSync-Metrik,
- Deployment History,
- Policy Checks.

## 12. Anti-Patterns

- CI macht weiterhin `kubectl apply`, obwohl GitOps angeblich führend ist.
- Self-Heal ohne Verständnis legitimer Controlleränderungen.
- `ignoreDifferences` wächst zum Drift-Versteck.
- Rollback = Git revert ohne Datenprüfung.
- GitOps-Repository ohne Ownership und Reviewregeln.

## 13. Quellen

- OpenGitOps Principles  
  https://opengitops.dev/
- Argo CD — Automated Sync Policy  
  https://argo-cd.readthedocs.io/en/stable/user-guide/auto_sync/
- AK-060 — Infrastructure as Code
- AK-036 — CI/CD Reference Pipeline

## 14. Merksatz

> GitOps ist kein Deployment-Tool. Es ist ein Betriebsmodell, in dem **Desired State, Änderungshistorie und tatsächlicher Zustand kontrolliert miteinander abgeglichen werden**.
