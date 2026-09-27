---
id: AK-075
legacy_ids:
  - QG-JAVA-075
title: Architecture Decision Process
artifact_type: operating-model
domain: architecture-governance
status: active
maturity: reviewed
normative_level: recommended
owner_role: Enterprise Architecture
last_validated: 2026-09-28
review_trigger:
  - Änderung des Architecture-Governance-Modells
  - wiederkehrende Entscheidungen ohne dokumentierte Rationale
  - Audit- oder Compliance-Befund
---

# AK-075 — Architecture Decision Process

## 1. Coach-Ziel

Architektur ist nicht die Fähigkeit, möglichst viele Technologien zu kennen. Architektur ist die Fähigkeit, **relevante Entscheidungen unter realen Randbedingungen nachvollziehbar herbeizuführen**.

Ein guter Entscheidungsprozess sorgt dafür, dass eine Organisation nicht von Einzelmeinungen, Hierarchie, Technologiepräferenzen oder dem lautesten Teilnehmer abhängig wird.

Der Prozess muss gleichzeitig zwei Fehler vermeiden:

- **Untersteuerung:** wesentliche Entscheidungen entstehen implizit im Code, in Tickets oder in Meetings.
- **Übersteuerung:** jede technische Kleinigkeit braucht ein Board, ein Template und mehrere Freigaben.

Die zentrale Frage lautet deshalb nicht:

> „Brauchen wir dafür ein ADR?“

sondern:

> „Ist diese Entscheidung architekturrelevant und später erklärungsbedürftig?“

## 2. Wann eine Entscheidung architekturrelevant ist

Eine Entscheidung ist ein ADR-Kandidat, wenn sie mindestens einen der folgenden Bereiche wesentlich beeinflusst:

- fachliche Fähigkeiten oder Prozessgrenzen,
- Datenhoheit oder Datenlebenszyklus,
- System- oder Verantwortungsgrenzen,
- Integration und Verträge,
- Security, IAM oder Datenschutz,
- Verfügbarkeit, Wiederanlauf oder Betrieb,
- Plattform- oder Technologiebindung,
- Kosten oder Beschaffung,
- Migration und spätere Änderbarkeit,
- organisationsübergreifende Zusammenarbeit.

Nicht jeder Bibliothekswechsel ist Architektur. Eine Bibliothekswahl **kann** Architektur werden, wenn sie beispielsweise Datenformate, Herstellerbindung, Security oder Betriebsfähigkeit langfristig beeinflusst.

## 3. Der Entscheidungsfluss

```text
1. Entscheidungsbedarf erkennen
        ↓
2. Entscheidungsfrage schneiden
        ↓
3. Stakeholder und Concerns klären
        ↓
4. Decision Drivers / Constraints bestimmen
        ↓
5. Optionen erarbeiten
        ↓
6. Evidenz beschaffen
        ↓
7. Trade-offs bewerten
        ↓
8. Entscheidung treffen
        ↓
9. Konsequenzen und Auflagen ableiten
        ↓
10. Umsetzung und Evidence verknüpfen
        ↓
11. Review-Trigger überwachen
```

## 4. Schritt 1 — Entscheidungsbedarf erkennen

Typische Signale:

- dieselbe Grundsatzfrage wird mehrfach diskutiert,
- Teams treffen widersprüchliche Entscheidungen,
- eine Änderung ist teuer rückgängig zu machen,
- mehrere Organisationseinheiten sind betroffen,
- ein Standard oder Zielbild lässt mehrere plausible Wege offen,
- ein Dienstleister benötigt eine verbindliche Vorgabe,
- Security, Datenschutz oder Betrieb verlangen eine bewusste Risikoentscheidung,
- eine Ausnahme von einem Standard wird beantragt.

### Coach-Frage

> Wird in sechs oder zwölf Monaten jemand fragen: „Warum haben wir das so gemacht?“

Wenn die Antwort plausibel ja ist, sollte die Entscheidung auffindbar dokumentiert werden.

## 5. Schritt 2 — Die Entscheidungsfrage schneiden

Ein ADR enthält **eine** Entscheidung.

Schlecht:

> „Wie bauen wir unsere neue Plattform mit Kubernetes, Kafka, OAuth2, GitOps und Microservices?“

Besser:

- Welche Deployment-Plattform wird verwendet?
- Welches Integrationsmodell gilt für Domänenereignisse?
- Wie werden Workload-Identitäten authentisiert?
- Wie wird der gewünschte Deployment-Zustand verwaltet?
- Welche fachlichen Teile benötigen unabhängige Deployments?

Wenn mehrere Entscheidungen unabhängig ersetzt werden könnten, gehören sie nicht in dasselbe ADR.

## 6. Schritt 3 — Stakeholder und Concerns

Architekturentscheidungen existieren nicht im luftleeren Raum.

| Stakeholder | typische Concerns |
|---|---|
| Fachverantwortung | fachliche Eindeutigkeit, Prozesswirkung, Änderbarkeit |
| Entwicklung | Komplexität, Testbarkeit, Wartbarkeit |
| Betrieb | Deployability, Monitoring, Recovery, Support |
| Informationssicherheit | Angriffsfläche, Controls, Nachweis |
| Datenschutz | Zweckbindung, Minimierung, Zugriffe, Lifecycle |
| Plattform | Standardisierung, Skalierbarkeit, Betriebsmodell |
| Management | Risiko, Kosten, Geschwindigkeit, Abhängigkeiten |
| Dienstleister | klare Liefergegenstände und Abnahmekriterien |

ISO/IEC/IEEE 42010:2022 stellt Stakeholder und deren Concerns in den Mittelpunkt von Architekturbeschreibungen. Das Entscheidungsmodell übernimmt diese Logik.

## 7. Schritt 4 — Decision Drivers vor Optionen

Decision Drivers werden **vor** der Bewertung festgelegt.

Beispiele:

- Schutzbedarf,
- P95-Latenz unter definierter Last,
- RTO/RPO,
- Änderungsfrequenz,
- Datenhoheit,
- Interoperabilität,
- Migrationsfähigkeit,
- verfügbare Teamkompetenz,
- Herstellerbindung,
- Kostenrahmen,
- gesetzliche oder organisatorische Constraints.

Ohne explizite Drivers wird die Bewertungsmatrix schnell zu einer nachträglichen Rechtfertigung der Lieblingslösung.

## 8. Schritt 5 — Optionen

Mindestens realistische Alternativen betrachten.

Typische Kategorien:

1. Ist-Zustand beibehalten.
2. konservative Verbesserung.
3. Zielbildoption.
4. Übergangsoption.

Nicht jede Entscheidung braucht drei Optionen. Wenn nur eine Option zulässig ist, wird genau dieser Constraint dokumentiert.

## 9. Schritt 6 — Evidenz

Nicht jede Unsicherheit darf durch Meinung ersetzt werden.

Mögliche Evidence:

- Lasttest,
- technischer Spike,
- Security Review,
- Kostenmodell,
- Betriebsdaten,
- Incident-Daten,
- Dependency-/Portfolioanalyse,
- Architekturprototyp,
- Hersteller-/Standarddokumentation.

Unbekanntes darf als `?` sichtbar bleiben.

## 10. Schritt 7 — Trade-offs

Architektur bedeutet nicht, die Option mit den meisten Vorteilen zu finden.

Architektur bedeutet:

> die Option zu wählen, deren **Trade-offs im aktuellen Kontext tragfähig** sind.

Eine qualitative Bewertung ist oft ehrlicher als ein künstlicher Score:

| Kriterium | A | B | C | Begründung |
|---|:---:|:---:|:---:|---|
| Betrieb | + | ++ | - | |
| Security | + | ++ | 0 | |
| Migration | ++ | 0 | - | |
| Kosten | + | - | ? | |

`?` bedeutet: Evidenz fehlt.

## 11. Schritt 8 — Entscheidung

Eine Entscheidung ist aktiv formuliert:

> **Wir werden ...**

Sie nennt:

- was gilt,
- für welchen Scope,
- ab wann,
- unter welchen Bedingungen.

Ein ADR ist kein Protokoll mit dem Ergebnis „man einigte sich darauf, weiter zu prüfen“.

## 12. Schritt 9 — Konsequenzen und Auflagen

Jede Entscheidung erzeugt einen neuen Kontext.

Deshalb werden mindestens dokumentiert:

- positive Konsequenzen,
- negative Konsequenzen,
- neue Risiken,
- notwendige Kompensationsmaßnahmen,
- neue Verantwortlichkeiten,
- Migrations-/Umsetzungsauflagen.

## 13. Schritt 10 — Von Entscheidung zu Evidence

Ein ADR ist erst wirksam, wenn seine Folgen in der Umsetzung sichtbar werden.

```text
ADR
 ↓
Standard / Work Package / Backlog
 ↓
Implementierung
 ↓
Test / Fitness Function / Policy Check
 ↓
Runtime Evidence / Abnahme
```

Beispiel:

```text
Entscheidung:
Domain darf kein Framework kennen

→ Architekturegel
→ Modulstruktur
→ ArchUnit-Test
→ CI-Gate
→ Review-Evidence
```

## 14. Schritt 11 — Review und Supersession

Ein akzeptiertes ADR wird nicht still umgeschrieben.

Wenn sich die Entscheidung ändert:

1. neues ADR erstellen,
2. altes ADR auf `superseded` setzen,
3. gegenseitig verlinken,
4. Auswirkungen und Migration dokumentieren.

Review-Trigger sind besser als pauschale Kalenderdaten.

Beispiele:

- neue regulatorische Vorgabe,
- Major-Plattformwechsel,
- SLO wird dauerhaft verfehlt,
- Kostenannahmen ändern sich,
- ein Incident widerlegt die Entscheidung,
- Transition Architecture erreicht ihren Exit-Punkt.

## 15. Risikobasierte Governance

Nicht jede Entscheidung braucht dieselbe Governance.

### Level 0 — lokale reversible Entscheidung

- im PR oder Code dokumentieren,
- kein ADR erforderlich.

### Level 1 — teamrelevante Entscheidung

- kurzer Decision Record,
- Peer Review.

### Level 2 — systemübergreifende Entscheidung

- vollständiges ADR,
- relevante Stakeholder konsultieren.

### Level 3 — enterprise-/risikorelevante Entscheidung

- vollständiges ADR,
- formales Review,
- Security/Datenschutz/Betrieb nach Bedarf,
- Entscheidungsgremium gemäß realem Mandat.

## 16. Ausnahmeprozess

Ein Standard ohne Ausnahmeprozess wird entweder dogmatisch oder wirkungslos.

Eine Ausnahme enthält:

- verletzten Standard,
- Grund,
- Scope,
- Risiko,
- Kompensationsmaßnahmen,
- Owner,
- Ablaufdatum oder Exit-Trigger,
- Migrationspfad.

## 17. Anti-Patterns

### Architecture Board für alles

Folge: Bottleneck und Umgehungsverhalten.

### ADR nach der Implementierung

Folge: Dokumentierte Rechtfertigung statt Entscheidungsunterstützung.

### Score-Theater

Folge: 4,37 suggeriert Objektivität, obwohl die Gewichtung subjektiv ist.

### „Accepted“ in einem Portfolio ohne echten Entscheider

Folge: Das Dokument wirkt künstlich. Lern- und Referenzmaterial wird deshalb nicht als Accepted ADR etikettiert.

### ADR als Standard

Folge: dieselbe Entscheidung wird in jedem Projekt neu diskutiert. Wiederkehrende Regeln gehören in Standards oder Policies.

## 18. Quellen

Primär bzw. maßgeblich:

- Michael Nygard, *Documenting Architecture Decisions*  
  https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions
- MADR 4.x  
  https://adr.github.io/madr/
- arc42, Architecture Decisions  
  https://docs.arc42.org/section-9/
- ISO/IEC/IEEE 42010:2022  
  https://www.iso.org/standard/74393.html
- The Open Group, Architecture Compliance  
  https://www.opengroup.org/architecture/togaf7-doc/arch/p4/comp/comp.htm

## 19. Coach-Merksatz

> Der Architekt besitzt nicht automatisch die Entscheidung.  
> Seine professionelle Leistung besteht darin, die Entscheidung **entscheidbar, nachvollziehbar und überprüfbar** zu machen.
