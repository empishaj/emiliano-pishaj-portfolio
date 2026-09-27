---
id: AK-088
legacy_ids:
  - ADR-088
title: Architektur dokumentieren – Viewpoints, arc42, C4, UML und ArchiMate
artifact_type: documentation-standard
domain: architecture-documentation
status: active
maturity: reviewed
normative_level: recommended
owner_role: Enterprise Architecture
last_validated: 2026-09-28
review_trigger:
  - Änderung der verwendeten Architekturmethoden
  - wiederkehrende Dokumentationslücken in Reviews oder Übergaben
  - wesentliche neue Stakeholder- oder Governance-Anforderungen
---

# AK-088 — Architektur dokumentieren, damit Entscheidungen handlungsfähig werden

## 1. Ziel

Architekturdokumentation ist kein Selbstzweck und kein Wettbewerb um das schönste Diagramm.

Sie soll Stakeholdern ermöglichen,

- Systemgrenzen zu verstehen,
- Verantwortlichkeiten zu erkennen,
- Entscheidungen nachzuvollziehen,
- Risiken und Abhängigkeiten sichtbar zu machen,
- Umsetzung und Betrieb zu steuern,
- Veränderungen kontrolliert vorzubereiten.

Der zentrale Grundsatz lautet:

> **Eine Sicht beantwortet eine konkrete Stakeholderfrage.**

Deshalb beginnt Dokumentation nicht mit der Wahl des Diagrammwerkzeugs, sondern mit der Frage:

> Wer muss was verstehen, um welche Entscheidung oder Handlung durchführen zu können?

## 2. Architektur ist nicht ihre Dokumentation

ISO/IEC/IEEE 42010:2022 unterscheidet zwischen der Architektur eines betrachteten Gegenstands und der Architecture Description, mit der Architektur ausgedrückt wird.

Das hat eine wichtige Konsequenz:

```text
Architektur
≠
Diagramm
≠
Dokument
≠
Tool
```

Ein Diagramm ist eine Sicht auf ausgewählte Aspekte. Jede Sicht lässt bewusst andere Aspekte weg.

## 3. Stakeholder → Concern → Viewpoint → View

Das Dokumentationsmodell folgt einer einfachen Kette:

```text
Stakeholder
    ↓
Concern
    ↓
Viewpoint
    ↓
View
```

Beispiel:

```text
Stakeholder:
Betrieb

Concern:
Wie kann der Service diagnostiziert und wiederhergestellt werden?

Viewpoint:
Deployment / Operations

View:
Runtime-Komponenten, Infrastruktur, externe Abhängigkeiten,
Healthchecks, Telemetrie, Recovery-Pfade
```

Ein anderes Beispiel:

```text
Stakeholder:
Fachverantwortung

Concern:
Wer verantwortet welche fachliche Fähigkeit und Information?

Viewpoint:
Business / Capability / Information

View:
Capability Map, Prozess-/Verantwortungssicht, Informationsobjekte
```

## 4. Welches Werkzeug beantwortet welche Frage?

### arc42

arc42 ist ein Strukturierungsrahmen für Softwarearchitekturdokumentation.

Er hilft, Themen wie Ziele, Randbedingungen, Kontext, Lösungsstrategie, Bausteine, Laufzeit, Deployment, Querschnittskonzepte, Entscheidungen, Qualitätsanforderungen und Risiken zusammenhängend zu dokumentieren.

arc42 ist **keine Modellierungssprache**.

### C4

C4 eignet sich besonders für die statische Struktur von Softwaresystemen.

Die vier Kernabstraktionsebenen sind:

1. System Context,
2. Container,
3. Component,
4. Code.

Wichtig: **Deployment ist nicht „C4 Level 4“.** Das Code Diagram ist Level 4. Deployment Diagrams sind ein zusätzlicher unterstützender Diagrammtyp des C4-Modells.

### UML

UML ist nützlich, wenn präzisere technische Strukturen oder Abläufe beschrieben werden müssen, zum Beispiel:

- Sequenzdiagramme,
- Zustandsautomaten,
- Aktivitätsdiagramme,
- Klassendiagramme.

UML sollte verwendet werden, wenn seine Präzision einen konkreten Mehrwert liefert – nicht weil „Architektur UML braucht“.

### ArchiMate

ArchiMate eignet sich besonders für Enterprise-Architecture-Zusammenhänge zwischen beispielsweise:

- Strategie,
- Fähigkeiten,
- Organisation,
- Prozessen,
- Anwendungen,
- Daten,
- Technologie,
- Motivation,
- Transformation.

ArchiMate und C4 konkurrieren nicht unmittelbar miteinander. Sie beantworten unterschiedliche Fragen und liegen typischerweise auf unterschiedlichen Flughöhen.

## 5. Auswahl nach Stakeholderfrage

| Frage | Geeignete Sicht / Methode |
|---|---|
| Welche externen Akteure und Systeme interagieren mit uns? | C4 System Context / ArchiMate Context |
| Welche deploybaren Softwareeinheiten existieren? | C4 Container |
| Wie ist ein Service intern strukturiert? | C4 Component / UML Component |
| Wie läuft ein kritischer Use Case zur Laufzeit? | UML Sequence / arc42 Laufzeitsicht |
| Wo läuft was? | Deployment Diagram / arc42 Verteilungssicht |
| Welche Capability wird durch welche Anwendung unterstützt? | ArchiMate / Capability Map |
| Welche Daten sind führend und wo fließen sie? | Datenlandkarte / ArchiMate Information View |
| Warum wurde eine Lösung gewählt? | ADR |
| Welche Regeln gelten wiederverwendbar? | Standard / Policy |
| Wie wird eine Transformation schrittweise erreicht? | Transition Architecture / Roadmap |

## 6. Minimal sufficient documentation

Mehr Dokumentation ist nicht automatisch besser.

Die Dokumentationstiefe richtet sich nach:

- Lebensdauer des Systems,
- Kritikalität,
- Anzahl und Vielfalt der Stakeholder,
- organisatorischen Grenzen,
- regulatorischen Anforderungen,
- Dienstleister-/Übergabesituation,
- Änderungshäufigkeit,
- Betriebs- und Migrationsrisiko.

### Kleine, interne Komponente

Möglicherweise ausreichend:

- kurzer Kontext,
- relevante ADRs,
- Schnittstellenvertrag,
- wenige Betriebsinformationen.

### Behördenweites Fachverfahren mit Dienstleistern

Typischerweise erforderlich:

- fachlicher und technischer Kontext,
- Verantwortlichkeiten,
- Daten- und Integrationssichten,
- IAM/Security,
- Betrieb,
- Qualitätsziele,
- Entscheidungen,
- Risiken,
- Migration und Übergangsarchitekturen,
- Nachweise und Abnahmekriterien.

## 7. Fakten, Ziele, Entscheidungen und Standards nicht vermischen

Eine professionelle Dokumentation unterscheidet mindestens:

### Ist-Fakt

> System A schreibt Daten in Datenbank B.

### Zielbild

> Personenstammdaten sollen künftig aus Register R bezogen werden.

### Entscheidung

> Für System A wird Register R als führende Quelle verwendet.

### Standard

> Neue REST-Schnittstellen müssen dem behördenweiten API-Standard entsprechen.

Wer diese Ebenen vermischt, erzeugt Dokumentation, die später nicht mehr erkennen lässt, was beobachtet, geplant, entschieden oder vorgeschrieben war.

## 8. Enterprise-Architecture-Dokumentationskette

Für Enterprise Architecture reicht eine Softwaresicht nicht aus.

Ein nützlicher Navigationspfad ist:

```text
Auftrag / Verwaltungsleistung
        ↓
Capability
        ↓
Prozess / Entscheidung / Verantwortung
        ↓
Information / Datenobjekt
        ↓
Anwendung
        ↓
Integration
        ↓
Technologie / Plattform
        ↓
Security / IAM / Betrieb
        ↓
Transformation / Transition Architecture
```

Der Architekt muss nicht alles in **einem** Diagramm darstellen.

Im Gegenteil: Ein Mega-Diagramm ist meist ein Zeichen fehlender Viewpoint-Disziplin.

## 9. Diagrammregeln

Ein Diagramm SOLLTE:

- einen klaren Titel besitzen,
- Scope und Abstraktionsebene erkennen lassen,
- relevante Elemente benennen,
- Beziehungen beschriften,
- Richtung oder Bedeutung von Pfeilen eindeutig machen,
- externe Elemente kennzeichnen,
- eine Legende verwenden, wenn Notation nicht selbsterklärend ist,
- nur Informationen enthalten, die zur beantworteten Frage beitragen.

Ein Diagramm DARF NICHT so tun, als zeige es mehr Sicherheit oder Vollständigkeit, als tatsächlich vorhanden ist.

## 10. Docs-as-Code richtig einordnen

Docs-as-Code bietet Vorteile:

- Versionierung,
- Review,
- Diffbarkeit,
- Nähe zu technischen Artefakten,
- automatisierbare Builds.

Aber:

> **Markdown in Git ist noch keine Living Documentation.**

Wenn niemand Owner ist, Quellen unklar sind oder Informationen doppelt gepflegt werden, veraltet auch Docs-as-Code.

Die Living-Documentation-Regeln stehen in AK-093.

## 11. Typische Anti-Patterns

### Ein Diagramm für alle

Management, Entwicklung und Betrieb benötigen unterschiedliche Sichten.

### Tool-first

> „Wir dokumentieren jetzt alles in ArchiMate.“

Besser:

> „Welche Frage müssen wir beantworten – und welche Sicht eignet sich dafür?“

### C4 Level 4 = Deployment

Falsch. Level 4 ist Code. Deployment ist ein zusätzlicher Diagrammtyp.

### Modell = Realität

Ein Modell ist eine bewusste Abstraktion und kann veralten.

### Alle Informationen aus Code generieren

Capabilities, Organisationsverantwortung, gesetzliche Randbedingungen, Risiken, Zielbilder und Entscheidungen entstehen nicht vollständig aus Sourcecode.

### Dokumentation ohne Entscheidungsnutzen

Wenn niemand mit einer Information entscheiden, umsetzen, prüfen oder betreiben kann, muss ihr Nutzen hinterfragt werden.

## 12. Review-Checkliste

Vor Veröffentlichung einer Architektursicht:

1. Wer ist der Leser?
2. Welches Concern beantwortet die Sicht?
3. Ist Scope eindeutig?
4. Ist Abstraktionsebene konsistent?
5. Sind Ist, Ziel, Entscheidung und Standard getrennt?
6. Sind Verantwortlichkeiten sichtbar, wenn sie relevant sind?
7. Kann ein neuer Stakeholder die Darstellung ohne mündliche Erklärung interpretieren?
8. Gibt es eine verantwortete Quelle?
9. Wie wird Aktualität geprüft?
10. Welche Entscheidung oder Handlung wird dadurch besser?

## 13. Quellen

- ISO/IEC/IEEE 42010:2022  
  https://www.iso.org/standard/74393.html
- arc42  
  https://docs.arc42.org/
- C4 Model — Diagrams  
  https://c4model.com/diagrams
- C4 Model — Deployment Diagram  
  https://c4model.com/diagrams/deployment
- The Open Group — ArchiMate  
  https://www.opengroup.org/archimate-forum/archimate-overview

## 14. Coach-Merksatz

> Beginne nie mit dem Diagramm. Beginne mit dem **Stakeholder, seinem Concern und der Entscheidung, die durch die Sicht möglich werden soll**.
