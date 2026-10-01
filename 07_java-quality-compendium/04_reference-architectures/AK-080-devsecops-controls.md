---
id: AK-080
legacy_ids:
  - ADR-080
title: DevSecOps Control Pipeline und Compliance Evidence
artifact_type: reference-architecture
domain: security-delivery
status: active
maturity: reviewed
normative_level: recommended
owner_role: Security Architecture / Platform Engineering
last_validated: 2026-10-01
review_trigger:
  - neue Security-/Supply-Chain-Anforderungen
  - Wechsel zentraler CI/CD-Plattform
  - wiederkehrende Findings trotz grüner Pipeline
---

# AK-080 — DevSecOps als Control- und Evidence-Architektur

## 1. Ziel

DevSecOps bedeutet nicht, möglichst viele Security Scanner in eine Pipeline zu hängen.

Das Ziel lautet:

> **Security Controls so früh, automatisierbar und nachvollziehbar wie sinnvoll in den Delivery-Lifecycle zu integrieren, ohne menschliche Risikoentscheidung vorzutäuschen.**

Referenzfluss:

```text
Source Change
→ Static Controls
→ Build/Test
→ Dependency/Supply Chain Controls
→ Artifact
→ Runtime/Deployment Controls
→ Dynamic Verification
→ Evidence
→ Risk/Exception Process
```

## 2. Control-Kategorien

### Secret Detection

Ziel: Credentials nicht in Source/History gelangen lassen.

### SAST

Ziel: bestimmte Schwachstellenmuster im Code erkennen.

### SCA

Ziel: bekannte Risiken in Dependencies und Komponenteninventar erkennen.

### SBOM

Ziel: Bestandteile eines Artefakts nachvollziehbar machen.

### Container/IaC Scan

Ziel: bekannte Image-/Konfigurationsrisiken erkennen.

### DAST

Ziel: laufendes System auf bestimmte Angriffs-/Fehlkonfigurationen prüfen.

### Policy as Code

Ziel: stabile Security-/Platform-Regeln maschinenlesbar durchsetzen.

Kein einzelner Control deckt alle Security-Risiken ab.

## 3. Policy Gate statt Scanner Gate

Ein Scanner produziert Findings.

Eine Policy entscheidet, was damit geschieht.

```text
Finding
→ Kontext / Severity / Exposure
→ Policy
→ Block | Warning | Risk Acceptance
→ Owner / SLA / Evidence
```

Beispiel:

> „Critical CVE gefunden“

ist noch keine vollständige Entscheidung.

Zusätzlich können relevant sein:

- ist die Komponente tatsächlich im Artefakt?
- ist der verwundbare Pfad erreichbar?
- ist das System exponiert?
- existiert ein Fix?
- welche Kompensationsmaßnahmen existieren?

## 4. Pipeline-Stufen als Defense in Depth

### Pull Request

Schnelles Feedback:

- Unit-/Security Tests,
- Secret Detection,
- SAST,
- Dependency-/License Policy,
- Architecture Checks.

### Artifact Build

- reproduzierbarer Build,
- SBOM,
- Container Scan,
- Provenance/Metadata,
- Signierung gemäß Policy.

### Pre-Production

- Integration-/Contract Tests,
- Migration Tests,
- DAST oder spezialisierte Security Tests,
- Policy Checks.

### Production Promotion

- definierte Freigaberegel gemäß Risikoklasse,
- identisches Artifact,
- nachvollziehbares Deployment.

## 5. Manuelle Security Reviews bleiben notwendig

Automatisierung erkennt nur modellierte Regeln.

Menschliches Review bleibt relevant für:

- Trust Boundaries,
- Authorization Design,
- Datenflüsse,
- Business Logic Abuse,
- neue Angriffspfade,
- Risikoakzeptanz,
- Architekturentscheidungen.

Darum gilt:

```text
Security Automation
+
Risk-based Human Review
```

statt entweder/oder.

## 6. Evidence

DevSecOps erzeugt verwertbare Nachweise:

- Scanreports,
- SBOM,
- Testreports,
- Policy Results,
- Artifact Digest,
- Approval/Risk Acceptance,
- Deployment-Historie.

Evidence muss zu Release und Entscheidung rückverfolgbar sein.

## 7. Ausnahmen

Eine Security-Ausnahme braucht:

- konkrete Regel/Control,
- Finding/Risiko,
- Exposure,
- Kompensationsmaßnahmen,
- Owner,
- Ablaufdatum,
- Ticket/Decision Reference.

Ausnahmen dürfen nicht dauerhaft in CI-Konfiguration versteckt werden.

## 8. Plattformprodukt statt Copy-Paste

Wiederkehrende Controls sollten möglichst als Plattformfähigkeit bereitgestellt werden:

- Pipeline Templates,
- Standard Scanner,
- Golden Path,
- gemeinsame Policy Bundles,
- zentrale Evidence-Ablage.

Teams müssen trotzdem verstehen, welche Risiken die Controls adressieren.

## 9. Messgrößen

Nicht nur Anzahl Findings messen.

Hilfreicher:

- Mean Time to Remediate nach Risikoklasse,
- wiederkehrende Finding-Arten,
- Anteil automatisierter Controls mit Owner,
- überfällige Risk Acceptances,
- False-Positive-Rate,
- Supply-Chain-Abdeckung.

## 10. Anti-Patterns

- mehr Scanner = mehr Security.
- Findings ohne Owner.
- jedes Finding blockiert unabhängig vom Kontext.
- Security Review erst kurz vor Go-Live.
- Ausnahmen ohne Ablaufdatum.
- DAST als Ersatz für Threat Modeling.

## 11. Quellen

- OWASP ASVS  
  https://owasp.org/www-project-application-security-verification-standard/
- SLSA  
  https://slsa.dev/spec/
- CycloneDX  
  https://cyclonedx.org/
- AK-015 — Application Security Baseline
- AK-036 — CI/CD Reference Pipeline
- AK-057 — Software Supply Chain

## 12. Merksatz

> DevSecOps ist reif, wenn aus Security-Anforderungen **konkrete Controls, klare Policies, verantwortete Ausnahmen und rückverfolgbare Evidence** werden.
