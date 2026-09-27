# Artefaktmodell des Architecture Knowledge Systems

## 1. Ziel

Das Compendium braucht eine klare semantische Trennung zwischen **Wissen**, **Vorgabe**, **Muster**, **Betriebsprozess** und **Entscheidung**. Ohne diese Trennung entsteht ein falscher Eindruck: Ein allgemeiner Artikel über Kafka, GraphQL oder PostgreSQL wäre dann scheinbar dieselbe Art Artefakt wie eine konkrete Entscheidung „System X nutzt Kafka für Ereignis Y“. Das ist fachlich nicht korrekt.

Das neue Modell verwendet deshalb einen stabilen Knowledge-Identifier `AK-xxx` und einen davon unabhängigen Artefakttyp.

## 2. Artefakttypen

### 2.1 Architecture Principle

Ein Principle ist langlebig und technologiearm. Es beschreibt eine gewünschte Eigenschaft der Architektur oder Entscheidungsarbeit.

Beispiele:

- Entscheidungen folgen Qualitätszielen, nicht Technologiepräferenzen.
- Datenhoheit muss explizit sein.
- Security und Betriebsfähigkeit werden im Zielbild berücksichtigt.
- Bevor verteilte Komplexität eingeführt wird, muss ihr Treiber nachgewiesen werden.

Ein Principle enthält **keine** konkrete Produktentscheidung.

### 2.2 Decision Guide

Ein Decision Guide hilft bei einer wiederkehrenden, kontextabhängigen Entscheidung.

Beispiele:

- Modulith oder Microservices?
- REST, gRPC oder Events?
- Read Replica, Cache, Suchindex oder CQRS-Projektion?
- Virtual Threads oder reaktive Verarbeitung?
- Partitionierung oder Sharding?

Pflichtinhalt:

1. Problemklasse.
2. Entscheidungstreiber.
3. realistische Optionen.
4. Eignung und Nicht-Eignung.
5. Trade-offs.
6. Mess- und Evidenzbedarf.
7. typische Fehlentscheidungen.
8. Fragen, die vor einer echten Entscheidung beantwortet werden müssen.

Ein Decision Guide trifft **keine Entscheidung für ein konkretes System**.

### 2.3 Architecture Standard

Ein Standard ist normativ. Er beschreibt, was innerhalb eines definierten Geltungsbereichs **MUSS**, **SOLLTE** oder **DARF NICHT** gelten.

Beispiele:

- API-Verträge werden als OpenAPI gepflegt.
- Logs enthalten definierte Korrelationsfelder.
- Secrets werden nicht im Repository gespeichert.
- Event-Verträge haben Owner und Versionierungsregeln.

Jeder Standard braucht:

- Geltungsbereich,
- normative Regeln,
- Rationale,
- Verifikationsmechanismus,
- Ausnahmeprozess,
- Verantwortlichkeit,
- Review-Trigger.

### 2.4 Policy

Eine Policy definiert Governance- oder Risikoregeln oberhalb einzelner Implementierungen.

Beispiele:

- API Lifecycle Policy.
- Data Governance Policy.
- AI Usage Policy.
- Technology Lifecycle Policy.

Policies beantworten stärker **wer darf was unter welchen Bedingungen** als **wie wird es technisch implementiert**.

### 2.5 Reference Architecture

Eine Reference Architecture zeigt eine bewährte, anpassbare Struktur.

Sie enthält:

- Kontext und Annahmen,
- Qualitätsziele,
- Bausteine und Verantwortlichkeiten,
- Schnittstellen,
- Datenflüsse,
- Security,
- Betrieb,
- Deployment,
- Failure Modes,
- Variationspunkte,
- Compliance-/Evidence-Punkte.

Eine Reference Architecture ist **kein Copy-and-Paste-Rezept**.

### 2.6 Engineering Guideline

Eine Engineering Guideline ist implementierungsnah.

Beispiele:

- Records für DTOs.
- Mockito.
- JavaDoc.
- Test Data Builder.
- MapStruct.

Sie enthält:

- Problem,
- gute und schlechte Beispiele,
- klare Einsatzgrenzen,
- Reviewfragen,
- automatisierbare Checks,
- Verweise auf übergeordnete Standards.

### 2.7 Operating Model

Ein Operating Model beschreibt Rollen, Entscheidungswege, Routinen und Feedbackschleifen.

Beispiele:

- Incident Management.
- Architecture Decision Process.
- Technology Radar.
- FinOps.
- Code Review.
- SLO/On-Call.

Ein Operating Model beantwortet insbesondere:

- Wer entscheidet?
- Wer betreibt?
- Wer überwacht?
- Wer eskaliert?
- Welches Artefakt entsteht?
- Welche Metrik zeigt Wirksamkeit?
- Wie wird gelernt und nachgesteuert?

### 2.8 Operating Guide / Runbook

Ein Operating Guide beschreibt konkrete Diagnose- oder Betriebsabläufe.

Beispiele:

- JFR/Async-Profiler.
- Docker Host auf Debian.
- DLQ-Reprocessing.
- Backup-/Restore-Übung.

Ein Runbook wird an einem **operativen Ereignis** ausgelöst und ist prozedural.

### 2.9 Learning Guide

Ein Learning Guide erklärt Grundlagen, Modelle oder Synthesen.

Er ist ausdrücklich **nicht normativ** und kein Kompetenznachweis.

Beispiele:

- Was ist Architektur?
- Entwurfsmuster.
- iSAQB-Synthese.
- OOP-Grundverständnis.

### 2.10 Architecture Decision Record

Ein ADR dokumentiert genau **eine konkrete architekturrelevante Entscheidung**.

Ein ADR ist gerechtfertigt, wenn die Entscheidung:

- Qualitätsziele wesentlich beeinflusst,
- schwer oder teuer rückgängig zu machen ist,
- mehrere Stakeholder oder Systeme betrifft,
- Security, Datenschutz, Betrieb, Datenhoheit, Kosten, Migration oder Lieferfähigkeit verändert,
- später erklärungsbedürftig sein wird.

Ein ADR ist **nicht**:

- ein Tutorial,
- ein Produktvergleich ohne konkreten Scope,
- eine Coding Guideline,
- eine allgemeine Best Practice,
- ein Architecture Board ausgedacht für ein Portfolio-Dokument.

## 3. Zwei Identitätsebenen

### Knowledge-ID

```text
AK-001
AK-002
...
```

Die Knowledge-ID ist dauerhaft. Sie identifiziert das Thema unabhängig davon, ob das Dokument später von Guideline zu Standard oder Decision Guide umklassifiziert wird.

Metadatum:

```yaml
id: AK-021
legacy_ids:
  - QG-JAVA-021
artifact_type: architecture-standard
```

### Decision-ID

Echte Entscheidungen verwenden einen separaten Namensraum, zum Beispiel:

```text
ADR-2026-001
ADR-2026-002
```

Damit wird verhindert, dass eine allgemeine Wissenssammlung mit real getroffenen Projektentscheidungen verwechselt wird.

## 4. Entscheidungs- und Wissensfluss

```text
Principle / Policy
       ↓
Decision Guide
       ↓
konkreter Kontext
       ↓
ADR
       ↓
Standard / Ausnahme / Reference Architecture
       ↓
Engineering Guideline / Golden Path
       ↓
Automated Control / Fitness Function
       ↓
Runtime Evidence
       ↓
Review / Learning
```

Nicht jeder ADR erzeugt einen neuen Standard. Nicht jeder Standard braucht für jedes Projekt einen neuen ADR. Die Beziehungen werden explizit verlinkt.

## 5. Ziel-Metadaten für Knowledge-Items

```yaml
id: AK-xxx
legacy_ids: []
title: ""
artifact_type: decision-guide
domain: integration
status: active
maturity: reviewed
normative_level: informative
scope: ""
owner_role: ""
last_validated: YYYY-MM-DD
review_trigger:
  - "neue Major-Version"
  - "neue regulatorische Vorgabe"
technology_baseline:
  java: "21+"
sources:
  primary: []
  secondary: []
supersedes: []
related: []
```

`technology_baseline` ist optional. Ein zeitloses Principle braucht sie nicht.

## 6. Statusmodell

Für Knowledge-Items:

```text
draft
→ reviewed
→ active
→ review-due
→ deprecated
→ archived
```

Für ADRs:

```text
draft
→ proposed
→ accepted | rejected
→ superseded | deprecated
```

Ein ADR wird nach `accepted` nicht still inhaltlich auf eine neue Entscheidung umgeschrieben.

## 7. Normative Sprache

Standards verwenden bewusst:

- **MUSS / DARF NICHT** – verbindlich.
- **SOLLTE / SOLLTE NICHT** – begründete Standarderwartung, Abweichung erklärbar.
- **KANN** – zulässige Option.
- **Beispiel** – nicht normativ.

Decision Guides und Learning Guides vermeiden normative Sprache, solange keine Policy oder kein Standard referenziert wird.

## 8. Warum dieses Modell besser zum Enterprise-Architecture-Profil passt

Es zeigt nicht nur technische Breite. Es zeigt, dass unterschiedliche Architekturartefakte unterschiedliche Aufgaben erfüllen:

```text
Principle       → Orientierung
Decision Guide  → Urteilskraft
ADR             → konkrete Entscheidung
Standard        → Wiederverwendung
Reference Arch  → Beschleunigung
Guideline       → Implementierungsqualität
Control         → Nachweis
Operating Model → Verantwortungsfähigkeit
Review          → Lernen und Evolution
```

Genau diese Differenzierung trennt eine Wissenssammlung von einem belastbaren Architecture-Governance-System.
