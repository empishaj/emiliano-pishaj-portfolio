---
id: AK-037
legacy_ids:
  - QG-JAVA-037
title: Container Image Security und Reproduzierbarkeit
artifact_type: architecture-standard
domain: platform-security
status: active
maturity: reviewed
normative_level: normative
owner_role: Platform Engineering / Security
last_validated: 2026-09-28
review_trigger:
  - Container-Runtime-/Policy-Änderung
  - Supply-Chain-Incident
---

# AK-037 — Container Images sicher und reproduzierbar bauen

## 1. Zweck

Container lösen Packaging-Probleme, aber sie beseitigen nicht automatisch Security-, Patch- oder Betriebsverantwortung.

Ein produktives Image soll:

- nachvollziehbar gebaut,
- möglichst klein im Funktionsumfang,
- nicht unnötig privilegiert,
- scanbar,
- aktualisierbar,
- unveränderlich identifizierbar

sein.

## 2. Build-Regeln

MUSS/SOLLTE:

- Multi-Stage Builds verwenden, wenn Buildtools nicht zur Runtime gehören,
- feste und freigegebene Base-Image-Familien verwenden,
- Images regelmäßig neu bauen, um Base-Image-Fixes aufzunehmen,
- Artifact/Image Digest erfassen,
- SBOM erzeugen,
- Image Scans in Delivery integrieren.

`latest` SOLLTE nicht als produktive Release-Identität verwendet werden.

## 3. Runtime User

Container SOLLEN ohne Root-Rechte laufen, sofern die Anwendung keine begründete Ausnahme benötigt.

Zusätzlich prüfen:

- Linux Capabilities,
- writable filesystem,
- temporäre Verzeichnisse,
- Ports,
- Volume Permissions.

„non-root“ allein ist kein vollständiges Runtime-Security-Modell.

## 4. Secrets

DARF NICHT:

- Secrets per `ENV` im Dockerfile fest einbauen,
- private Schlüssel in Image Layers kopieren,
- Build-Secrets in finalen Layers hinterlassen.

Secrets werden zur Laufzeit oder über sichere Build-Secret-Mechanismen bereitgestellt.

## 5. Base Images

Auswahl nach:

- Vertrauenswürdigkeit/Quelle,
- Patchprozess,
- benötigten Libraries,
- Debug-/Operations-Anforderungen,
- Architektur/CPU-Plattform.

Ein extrem minimales Image kann Security-Oberfläche reduzieren, aber Diagnose und Kompatibilität erschweren. Die Wahl ist ein Trade-off.

## 6. Health und Graceful Shutdown

Das Image beziehungsweise die Anwendung muss mit der Zielplattform korrekt zusammenspielen:

- Startverhalten,
- Health/Readiness,
- SIGTERM/Shutdown,
- Exit Codes,
- Logs nach stdout/stderr oder vereinbartem Modell.

## 7. Unveränderlichkeit

Produktive Images werden nach Veröffentlichung nicht „gepatcht“.

Bei Änderungen:

```text
Source/Base Update
→ neuer Build
→ neue Tests/Scans
→ neues Digest
→ Promotion
```

## 8. Verifikation

- Dockerfile/Containerfile Lint,
- Image Scan,
- SBOM,
- User/Permission Test,
- Secret Scan,
- Start-/Shutdown-Test,
- Policy-as-Code auf Plattformebene.

## 9. Anti-Patterns

- `latest` als einzige Version.
- Paketmanager/Compiler unnötig in Runtime Image.
- Root aus Bequemlichkeit.
- Secret per Build ARG in Layer-Historie.
- nie neu gebautes „goldenes“ Base Image.
- Container als Ersatz für Patch-/Vulnerability-Prozess.

## 10. Quellen

- Docker Build / Dockerfile Documentation  
  https://docs.docker.com/build/
- Kubernetes Security Context Documentation  
  https://kubernetes.io/docs/tasks/configure-pod-container/security-context/
- AK-057 — Software Supply Chain
- AK-126 — Docker Host/Operations Guide

## 11. Coach-Merksatz

> Ein Container ist ein Lieferartefakt. Seine Qualität zeigt sich daran, ob **Herkunft, Inhalt, Rechte, Patchstand und Runtime-Verhalten nachvollziehbar und kontrollierbar** sind.
