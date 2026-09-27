---
id: AK-057
legacy_ids:
  - ADR-057
title: Software Supply Chain – SBOM, Abhängigkeiten, Provenance und Artefaktintegrität
artifact_type: security-standard
domain: supply-chain-security
status: active
maturity: reviewed
normative_level: normative
owner_role: Security Architecture / Platform Engineering
last_validated: 2026-09-28
review_trigger:
  - neue SBOM-/Provenance-Spezifikation
  - schwerwiegender Supply-Chain-Incident
  - Wechsel zentraler Build-/Artifact-Plattform
---

# AK-057 — Software Supply Chain Security

## 1. Zweck

Ein Softwareartefakt besteht nicht nur aus eigenem Sourcecode.

Es enthält oder hängt ab von:

- direkten und transitiven Bibliotheken,
- Build-Plugins,
- Container Base Images,
- externen Build-Actions,
- Paketquellen,
- Toolchains,
- Build-Infrastruktur.

Der Standard verfolgt deshalb die Kette:

```text
Source
→ Build
→ Dependencies
→ Artifact
→ SBOM
→ Provenance / Attestation
→ Scan / Policy
→ Repository
→ Deployment
```

## 2. Verbindliche Mindestregeln

### Abhängigkeiten

MUSS:

- über kontrollierte Paketquellen bezogen werden,
- Versionen nachvollziehbar machen,
- regelmäßig auf bekannte Schwachstellen geprüft werden,
- Lizenz-/Policy-Anforderungen berücksichtigen.

### SBOM

Für produktive Artefakte SOLL eine maschinenlesbare SBOM erzeugt und gemeinsam mit dem Release aufbewahrt werden.

Zulässige verbreitete Formate sind insbesondere CycloneDX und SPDX. Die konkrete Organisationsentscheidung wird separat getroffen.

### Artefaktintegrität

MUSS:

- Build-Artefakte unveränderlich identifizieren,
- Hash/Digest-basierte Referenzen unterstützen,
- Promotion statt Neu-Build zwischen Umgebungen bevorzugen.

### Provenance

Für kritische Lieferketten SOLL nachvollziehbar sein:

- aus welchem Source-Stand,
- mit welchem Buildprozess,
- in welcher Buildumgebung,
- wann und wodurch

ein Artefakt entstanden ist.

SLSA beschreibt Provenance als verifizierbare Information darüber, wo, wann und wie Artefakte erzeugt wurden.

### Secrets

Build- und Deployment-Secrets DÜRFEN NICHT in Source, Logs oder erzeugte Artefakte gelangen.

## 3. SBOM ist Inventar, kein Sicherheitsnachweis

Eine SBOM beantwortet:

> Was ist enthalten?

Sie beantwortet nicht automatisch:

- Ist eine Schwachstelle ausnutzbar?
- Ist das Artefakt manipuliert?
- Ist die Lizenz zulässig?
- Ist die Build-Pipeline vertrauenswürdig?

Darum wird SBOM mit weiteren Controls kombiniert.

## 4. Vulnerability Management

Ein Finding braucht einen Lebenszyklus:

```text
Finding
→ Triage
→ Exploitability / Exposure
→ Priorität
→ Fix / Mitigation / Acceptance
→ Evidence
```

CVSS allein ist nicht genug. Relevant sind auch:

- Erreichbarkeit des Codes,
- Exposure,
- vorhandene Controls,
- Schutzbedarf,
- verfügbare Fixes,
- aktive Exploitation.

## 5. Dependency Updates

Automatisierte Update-Vorschläge sind hilfreich, aber nicht gleichbedeutend mit automatischem Production-Upgrade.

SOLLTE:

- kleinere Updates automatisiert vorschlagen,
- Tests und Security-Checks ausführen,
- Major Updates bewusst reviewen,
- veraltete oder nicht mehr gepflegte Dependencies sichtbar machen.

## 6. Container Supply Chain

Zusätzlich prüfen:

- Base Image Herkunft,
- Digest Pinning wo sinnvoll,
- minimale Packages,
- regelmäßige Rebuilds für Security Fixes,
- Image Scan,
- Signierung/Attestation je Organisationsstandard.

Ein „kleines Image“ ist nicht automatisch sicherer, aber reduziert typischerweise Angriffsfläche und Patchumfang.

## 7. Build-Pipeline als Security Boundary

Die Pipeline kann produktive Artefakte erzeugen und besitzt deshalb privilegierte Fähigkeiten.

MUSS:

- Rechte nach Least Privilege vergeben,
- produktive Credentials schützen,
- externe Actions/Plugins kontrollieren,
- Änderungen an Pipelinecode reviewen,
- nachvollziehbare Buildlogs/Evidence erzeugen.

## 8. Verifikation

Mögliche Evidence:

- SBOM-Datei pro Release,
- SCA-Report,
- Container Scan,
- Signatur-/Attestation-Prüfung,
- Dependency Policy Check,
- Artifact Digest,
- Provenance Statement,
- dokumentierte Risk Acceptances.

## 9. Ausnahmeprozess

Eine nicht kurzfristig behebbaren Schwachstelle benötigt:

- betroffene Komponente,
- Exposure-/Exploitability-Bewertung,
- Kompensationsmaßnahmen,
- Owner,
- Review-/Ablaufdatum.

Das bloße Label „false positive“ reicht nicht.

## 10. Quellen

- CycloneDX Specification  
  https://cyclonedx.org/specification/overview/
- SPDX  
  https://spdx.dev/
- SLSA Specification  
  https://slsa.dev/spec/
- OWASP Software Component Verification / Dependency Guidance

## 11. Coach-Merksatz

> Eine sichere Lieferkette beantwortet nicht nur, **welchen Code wir geschrieben haben**, sondern auch, **welche Bestandteile wir beziehen, wie das Artefakt erzeugt wurde und welche Evidence seine Herkunft und Integrität stützt**.
