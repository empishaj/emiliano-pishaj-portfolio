# Artefaktmodell des Architecture Knowledge Systems

## 1. Ziel

Das Compendium trennt **Wissen**, **Vorgaben**, **Muster**, **Betriebsprozesse** und **konkrete Entscheidungen** bewusst voneinander. Ohne diese Trennung würde ein allgemeiner Artikel über Kafka, OAuth2 oder PostgreSQL denselben Status erhalten wie eine konkrete Entscheidung für ein bestimmtes System. Das wäre fachlich falsch.

Alle wiederverwendbaren Wissensartefakte verwenden deshalb eine stabile Knowledge-ID `AK-xxx`. Echte Architecture Decision Records besitzen einen eigenen ADR-Namensraum.

## 2. Artefakttypen

### 2.1 Architecture Principle

Ein Principle ist langlebig und technologiearm. Es beschreibt eine gewünschte Eigenschaft von Architektur oder Entscheidungsarbeit.

Beispiele:

- Entscheidungen folgen Qualitätszielen, nicht Technologiepräferenzen.
- Datenhoheit muss explizit sein.
- Security und Betriebsfähigkeit werden im Zielbild berücksichtigt.
- Verteilte Komplexität braucht einen nachweisbaren Treiber.

Ein Principle enthält keine konkrete Produktentscheidung.

### 2.2 Decision Guide

Ein Decision Guide unterstützt eine wiederkehrende, kontextabhängige Architekturentscheidung.

Beispiele:

- Modulith oder Microservices?
- synchrone API oder Eventing?
- Read Replica, Cache oder Suchindex?
- CQRS oder gemeinsames Read-/Write-Modell?

Pflichtinhalt:

1. Problemklasse,
2. Entscheidungstreiber,
3. realistische Optionen,
4. Eignung und Nicht-Eignung,
5. Trade-offs,
6. Mess- und Evidenzbedarf,
7. typische Fehlentscheidungen,
8. Fragen, die vor einer echten Entscheidung beantwortet werden müssen.

Ein Decision Guide trifft keine Entscheidung für ein konkretes System.

### 2.3 Architecture Standard

Ein Standard ist normativ und technisch oder architektonisch konkret. Er beschreibt, was innerhalb eines definierten Geltungsbereichs gelten muss oder sollte.

Beispiele:

- API-Verträge werden maschinenlesbar gepflegt.
- Logs enthalten definierte Korrelationsfelder.
- Secrets liegen nicht im Repository.
- Event-Verträge besitzen Owner und Evolutionsregeln.

Ein Standard braucht mindestens:

- Zweck und Geltungsbereich,
- normative Regeln,
- Begründung,
- Verifikationsmechanismus,
- Ausnahmeweg,
- Verantwortlichkeit,
- Review-Trigger.

### 2.4 Policy

Eine Policy legt organisatorische oder risikobezogene Leitplanken fest. Sie beantwortet primär:

> Wer darf was unter welchen Bedingungen und mit welcher Verantwortung?

Beispiele:

- API Lifecycle Policy,
- Data Governance Policy,
- AI Usage Policy,
- Technology Lifecycle Policy.

Der Unterschied zum Standard:

- **Policy:** definiert Entscheidungsspielraum, Zuständigkeit und Bedingungen.
- **Standard:** konkretisiert wiederverwendbare technische oder architektonische Anforderungen innerhalb dieses Rahmens.

Beispiel:

```text
Policy:
Externe KI-Dienste dürfen nur für freigegebene Datenklassen verwendet werden.

Standard:
Freigegebene LLM-Integrationen müssen Provider, Datenflüsse, Telemetrie,
PII-Schutz und Kostenmessung nach definiertem Schema dokumentieren.
```

Policy und Standard können im selben Themengebiet existieren, erfüllen aber unterschiedliche Funktionen.

### 2.5 Reference Architecture

Eine Reference Architecture beschreibt eine bewährte, anpassbare Lösungsstruktur.

Sie enthält typischerweise:

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
- Compliance- und Evidence-Punkte.

Eine Reference Architecture ist kein Copy-and-Paste-Rezept. Verbindlichkeit entsteht erst durch Policy, Standard oder konkrete Entscheidung.

### 2.6 Engineering Guideline

Eine Engineering Guideline ist implementierungsnah und hat einen engeren Scope als ein organisationsweiter Architecture Standard.

Beispiele:

- Records für Datenträger,
- Test Doubles mit Mockito,
- JPA-Zugriffe,
- Feature Flags,
- Code Review.

Sie enthält:

- Problem und Einsatzbereich,
- gute und schlechte Beispiele,
- klare Grenzen,
- normative Regeln für den definierten Scope,
- Reviewfragen,
- gegebenenfalls automatisierbare Checks,
- Verweise auf übergeordnete Standards.

### 2.7 Operating Model

Ein Operating Model beschreibt Rollen, Entscheidungswege, Routinen und Feedbackschleifen.

Beispiele:

- Incident Management,
- Architecture Decision Process,
- Technology Radar,
- FinOps,
- SLO-/On-Call-Modell.

Ein Operating Model beantwortet insbesondere:

- Wer entscheidet?
- Wer betreibt?
- Wer überwacht?
- Wer eskaliert?
- Welches Artefakt entsteht?
- Welche Metrik zeigt Wirksamkeit?
- Wie wird gelernt und nachgesteuert?

### 2.8 Operating Guide / Runbook

Ein Operating Guide beschreibt konkrete Diagnose-, Betriebs- oder Wiederherstellungsabläufe.

Beispiele:

- JFR/Async-Profiler,
- Docker Host auf Debian,
- DLQ-Reprocessing,
- Backup-/Restore-Übung,
- kontrollierte Chaos-Experimente.

Ein Runbook wird meist durch ein operatives Ereignis ausgelöst und ist stärker prozedural als ein Operating Model.

### 2.9 Learning Guide

Ein Learning Guide erklärt Grundlagen, Modelle oder Synthesen. Er ist ausdrücklich nicht normativ und kein Kompetenznachweis.

Beispiele:

- Was ist Architektur?
- Objektorientierung und Verantwortung,
- Design Patterns,
- Architektursynthesen.

### 2.10 Architecture Decision Record

Ein ADR dokumentiert genau eine konkrete architekturrelevante Entscheidung.

Ein ADR ist sinnvoll, wenn die Entscheidung:

- Qualitätsziele wesentlich beeinflusst,
- schwer oder teuer rückgängig zu machen ist,
- mehrere Stakeholder oder Systeme betrifft,
- Security, Datenschutz, Betrieb, Datenhoheit, Kosten, Migration oder Lieferfähigkeit verändert,
- später erklärungsbedürftig sein wird.

Ein ADR ist nicht:

- ein Tutorial,
- ein Produktvergleich ohne konkreten Scope,
- eine Coding Guideline,
- eine allgemeine Best Practice,
- ein fiktiver Board-Beschluss für ein Portfolio.

## 3. Zwei Identitätsebenen

### Knowledge-ID

```text
AK-001
AK-002
...
```

Die Knowledge-ID identifiziert das Thema dauerhaft, auch wenn sich seine Klassifikation später ändert.

Beispiel:

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

Damit werden Wissenssammlung und reale Projektentscheidung nicht miteinander verwechselt.

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

Nicht jeder ADR erzeugt einen Standard. Nicht jeder Standard braucht in jedem Projekt einen ADR. Die Beziehungen werden explizit verlinkt.

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

`technology_baseline` ist optional. Ein zeitloses Principle benötigt sie nicht.

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

Ein akzeptiertes ADR wird nicht still auf eine neue Entscheidung umgeschrieben. Eine neue Entscheidung erzeugt ein neues ADR und verlinkt den Vorgänger.

## 7. Normative Sprache

Standards und Engineering Guidelines verwenden bewusst:

- **MUSS / DARF NICHT** – verbindlich innerhalb des definierten Scopes.
- **SOLLTE / SOLLTE NICHT** – begründete Standarderwartung; Abweichung ist erklärbar.
- **KANN** – zulässige Option.
- **Beispiel** – nicht normativ.

Decision Guides und Learning Guides vermeiden normative Sprache, solange sie keine bestehende Policy oder keinen Standard wiedergeben.

## 8. Merge- und Split-Regeln

Mehrere Dokumente werden zusammengeführt, wenn:

- sie dieselbe Problemklasse behandeln,
- ihre Zielgruppen und Entscheidungsebenen identisch sind,
- getrennte Pflege nur Redundanz erzeugt.

Ein Dokument wird geteilt, wenn:

- es mehrere unabhängige Entscheidungen vermischt,
- normative und rein erklärende Inhalte nicht mehr sauber getrennt sind,
- unterschiedliche Owner oder Review-Trigger gelten,
- ein Teil stark versionsabhängig ist und der andere langlebig bleiben soll.

Dateigröße allein ist weder Merge- noch Split-Kriterium.

## 9. Qualitätsfrage pro Artefakttyp

| Artefakttyp | zentrale Frage |
|---|---|
| Principle | Welche langlebige Leitidee soll Entscheidungen orientieren? |
| Policy | Wer darf was unter welchen Bedingungen? |
| Decision Guide | Welche Optionen passen zu welchen Treibern? |
| ADR | Was wurde in diesem konkreten Kontext entschieden und warum? |
| Standard | Welche wiederverwendbare Regel gilt im definierten Scope? |
| Reference Architecture | Wie kann eine bewährte Lösungsstruktur aussehen? |
| Engineering Guideline | Wie setzen Entwickler eine eng umrissene Praxis zuverlässig um? |
| Operating Model | Wer tut was, wann und mit welchem Feedback? |
| Operating Guide | Wie wird ein konkreter Betriebsfall ausgeführt? |
| Learning Guide | Welches Konzept muss verstanden werden? |

Diese Trennung macht aus einer Wissenssammlung ein belastbares Architecture-Governance-System.
