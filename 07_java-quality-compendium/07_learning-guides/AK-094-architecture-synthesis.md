---
id: AK-094
legacy_ids:
  - ADR-094
title: Architektur-Synthese – vom Problem zur lernenden Governance
artifact_type: learning-guide
domain: architecture-learning
status: active
maturity: reviewed
normative_level: informative
last_validated: 2026-10-01
review_trigger:
  - wesentliche Änderung des Architecture-Knowledge-Modells
  - neue übergreifende Governance-Domäne
---

# AK-094 — Architektur-Synthese: die Verbindungen verstehen

## 1. Worum es hier geht

Dieses Dokument ist **kein ADR** und keine Behauptung eines „Guru-Levels“.

Es ist eine Lernlandkarte.

Eine umfangreiche Sammlung zu Java, DDD, APIs, Security, Kubernetes, SLOs oder Governance wird erst dann architektonisch wertvoll, wenn die Beziehungen zwischen den Themen verstanden werden.

Der zentrale Denkweg lautet:

```text
Auftrag / Business Driver
        ↓
Stakeholder & Concerns
        ↓
Qualitätsziele & Constraints
        ↓
Prinzipien
        ↓
Optionen & Trade-offs
        ↓
konkrete Entscheidung
        ↓
Standards / Reference Architectures
        ↓
Engineering & Delivery
        ↓
Runtime Evidence
        ↓
Learning / Review
        ↓
Evolution / Transition Architecture
        ↺
```

## 2. Fünf Ebenen, die nicht verwechselt werden dürfen

### Ebene 1 — Auftrag und Strategie

Fragen:

- Was soll für Bürger, Fachseite oder Organisation verbessert werden?
- Welche Capabilities sind betroffen?
- Welche Risiken oder Zwänge treiben Veränderung?
- Was ist Ziel und was nur Mittel?

Hier entstehen keine Kubernetes- oder Frameworkentscheidungen.

### Ebene 2 — Architektur

Fragen:

- Welche Verantwortungsgrenzen brauchen wir?
- Welche Daten sind führend?
- Wie interagieren Systeme?
- Welche Qualitätsziele dominieren?
- Welche Struktur ist für den Kontext tragfähig?

### Ebene 3 — Engineering

Fragen:

- Wie werden Architekturentscheidungen implementiert?
- Welche Coding-, Test- und Build-Leitplanken gelten?
- Welche Teile lassen sich automatisiert prüfen?

### Ebene 4 — Betrieb

Fragen:

- Wie erkennen wir, ob das System tatsächlich funktioniert?
- Welche SLOs gelten?
- Wie diagnostizieren wir Fehler?
- Wie werden Recovery und Incident Response organisiert?

### Ebene 5 — Evolution

Fragen:

- Welche Annahme wurde widerlegt?
- Welche Standards wirken nicht?
- Welche technische Schuld verändert das Risiko?
- Welche Transition ist als Nächstes sinnvoll?

## 3. Sieben Denkmuster reifer Architekturarbeit

### 3.1 Problem vor Technologie

Nicht:

> „Wo können wir Kafka einsetzen?“

Sondern:

> „Welche Kopplung, Latenz, Verfügbarkeit und Konsistenz benötigt der fachliche Prozess?“

Erst dann wird Kafka, REST, gRPC oder eine andere Option bewertet.

### 3.2 Qualität vor Pattern

Nicht:

> „Wir brauchen Microservices.“

Sondern:

> „Wir brauchen unabhängige Änderung oder Skalierung in genau diesem Bereich – rechtfertigt das die Kosten verteilter Systeme?“

Pattern sind Antworten auf Kräfte im Kontext.

### 3.3 Verantwortung und Architektur zusammen denken

Technische Grenzen ohne Ownership sind instabil.

Fragen:

- Wer besitzt die Daten?
- Wer entscheidet über Änderungen?
- Wer betreibt?
- Wer trägt Incident-Verantwortung?
- Wer darf Ausnahmen genehmigen?

Damit wird Systemarchitektur zur sozio-technischen Architektur.

### 3.4 Trade-offs statt Absolutismen

Es gibt selten „beste Architektur“ unabhängig vom Kontext.

Beispiele:

```text
Standardisierung
↔ Autonomie

Konsistenz
↔ Verfügbarkeit / Entkopplung

Sicherheit
↔ Nutzbarkeit / Geschwindigkeit

Flexibilität
↔ Einfachheit

Isolation
↔ Kosten / Betriebsaufwand
```

Reife zeigt sich darin, beide Seiten verständlich machen zu können.

### 3.5 Evidence statt Architekturbehauptung

Nicht:

> „Das System ist resilient.“

Sondern:

- Welche Failure Modes wurden betrachtet?
- Welche SLOs gelten?
- Welche Resilience-Mechanismen existieren?
- Wann wurde Recovery zuletzt getestet?

Architektur wird stärker, wenn Behauptungen in Evidence übersetzt werden.

### 3.6 Transition statt Big-Bang-Zielbild

Enterprise Architecture muss nicht nur wissen, **wohin**, sondern auch **wie von hier nach dort**.

```text
Ist
→ Transition 1
→ Transition 2
→ Ziel
```

Jeder Übergang muss selbst betreibbar und risikobeherrschbar sein.

### 3.7 Lernen institutionalisieren

Incident, Review oder Architekturabweichung dürfen nicht nur lokale Erkenntnisse bleiben.

```text
Ereignis
→ Analyse
→ Erkenntnis
→ Entscheidung / Standardänderung
→ Umsetzung
→ Evidence
```

So wird aus Erfahrung organisationale Fähigkeit.

## 4. Wie das Compendium zusammenwirkt

### Architecture Foundations

- AK-081: Was Architektur ist.
- AK-082: Wie Qualitätsziele operationalisiert werden.
- AK-087: Wie Architektur bewertet wird.
- AK-088: Wie relevante Sichten dokumentiert werden.

### Governance

- AK-075: Wie Entscheidungen entstehen und leben.
- AK-061: Wie ausgewählte Architektureigenschaften automatisiert geprüft werden.
- AK-093: Wie Architekturinformation aktuell und nachvollziehbar bleibt.

### Software und Domain

DDD, Modulith, Hexagonal Architecture, CQRS, Patterns und Designprinzipien beantworten Strukturfragen auf unterschiedlichen Ebenen.

### Integration und Daten

REST, OpenAPI, Events, AsyncAPI, Outbox, Saga, Datenstrategie und Datenbankmuster beantworten Fragen zu Verträgen, Ownership, Konsistenz und Kopplung.

### Security und Privacy

IAM, OAuth2/OIDC, Privacy by Design, Input Validation, Supply Chain Security und Platform Controls übersetzen Schutzanforderungen in Architekturmechanismen und Evidence.

### Platform und Delivery

Kubernetes, IaC, GitOps, Golden Paths und DevSecOps machen technische Standards wiederholbar und betreibbar.

### Operations

Observability, SLOs, Backup/Restore, Profiling, Incident Management und Post-Mortems schließen den Feedback Loop.

Die genannten Themen sind Wissensdomänen. Nicht jede historische Nummer besitzt im kanonischen Bestand ein eigenes Dokument.

## 5. Enterprise-Architecture-Transfer

Für Enterprise Architecture wird dasselbe Denkmodell auf eine größere Flughöhe übertragen.

```text
Verwaltungsauftrag
→ Capability
→ Prozess / Entscheidung
→ Information
→ Anwendung
→ Integration
→ Technologie
→ Security / Betrieb
→ Governance
→ Transformation
```

Beispiel:

```text
Auftrag:
medienbruchfreien Status bereitstellen

↓
Capability:
Vorgangsstatus bereitstellen

↓
Information:
Status, Zeitpunkt, Quelle, fachliche Bedeutung

↓
Application:
Fachverfahren / Statusprojektion / Portal

↓
Integration:
API oder Event

↓
Quality:
Aktualität, Nachvollziehbarkeit, Verfügbarkeit

↓
Decision:
führende Quelle und Synchronisationsmodell

↓
Governance:
Owner, Vertrag, Monitoring, Ausnahmeprozess
```

Das ist Enterprise Architecture mit technischer Bodenhaftung.

## 6. Wissen ist nicht Erfahrung

Ein öffentliches Compendium darf nicht den Eindruck erzeugen:

> „Weil dieses Dokument existiert, habe ich das Thema in großen Produktionslandschaften beherrscht.“

Wir unterscheiden deshalb:

```text
Wissen
= Konzept verstanden

Übung
= Konzept selbst angewendet

Erfahrung
= unter realen Randbedingungen und Konsequenzen angewendet

Kompetenz
= wiederholt tragfähige Ergebnisse erzeugt
```

Das Compendium dokumentiert in erster Linie **Wissen, Urteilskriterien und Arbeitsmodelle**. Berufserfahrung wird getrennt im Lebenslauf und durch belegbare Projektergebnisse dargestellt.

## 7. Keine Zertifizierungsbehauptungen

Die historischen Lerntexte orientieren sich teilweise an iSAQB-Themenfeldern.

Das bedeutet nicht automatisch:

- vollständige Lehrplanabdeckung,
- Zertifizierung,
- Expert-Level,
- „Guru-Level“.

Solche Aussagen werden aus dem neuen System entfernt, sofern sie nicht durch tatsächliche Qualifikation belegt sind.

## 8. Architektur als Entscheidungsinfrastruktur

Die reifste Synthese lautet:

> Architektur ist nicht die zentrale Stelle, die alle technischen Entscheidungen selbst trifft.

Eine gute Architecture Function schafft:

- klare Prinzipien,
- entscheidbare Fragen,
- wiederverwendbare Standards,
- verständliche Reference Architectures,
- passende Entscheidungsrechte,
- technische Guardrails,
- Evidenz,
- Feedbackschleifen,
- transparente Ausnahmen.

Dadurch können Teams selbstständiger entscheiden, ohne dass Enterprise-Kohärenz verloren geht.

## 9. Quellen

- ISO/IEC/IEEE 42010:2022  
  https://www.iso.org/standard/74393.html
- Michael Nygard, Architecture Decision Records  
  https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions
- arc42  
  https://docs.arc42.org/
- CMU/SEI ATAM  
  https://www.sei.cmu.edu/library/architecture-tradeoff-analysis-method-collection/
- Neal Ford et al., *Building Evolutionary Architectures* — konzeptionelle Grundlage
- Matthew Skelton, Manuel Pais, *Team Topologies* — sozio-technische Organisationsperspektive

## 10. Merksatz

> Der Sprung vom Spezialisten zur Architekturarbeit geschieht nicht dadurch, noch mehr Technologien zu lernen. Entscheidend ist, **Auftrag, Qualität, Verantwortung, Entscheidung, Umsetzung, Evidence und Evolution als einen zusammenhängenden Regelkreis zu denken**.
