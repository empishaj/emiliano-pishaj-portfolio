# Masterkonzept — Architecture Governance Compendium v2

## 1. Ziel

Dieses Compendium ist kein Lexikon für Technologien und keine Sammlung von Meinungen. Es ist ein **Architecture-Governance-System**, das Entscheidungen, Standards, technische Leitplanken, Nachweise und Lernartefakte so miteinander verbindet, dass Architektur nicht nur beschrieben, sondern steuerbar wird.

Der Anspruch lautet:

> Eine gute Architekturinformation sagt nicht nur *was* wir tun, sondern *warum*, *unter welchen Randbedingungen*, *mit welchen Konsequenzen*, *wie die Umsetzung geprüft wird* und *wann neu entschieden werden muss*.

## 2. Das Problem des bisherigen Bestands

Der Altbestand enthält viele starke Inhalte, aber unterschiedliche Dokumentarten tragen denselben ADR-/QG-Charakter. Dadurch entstehen vier typische Probleme:

1. **Flughöhen werden vermischt.** Ein MapStruct-Mappingstandard steht neben Datenstrategie oder FinOps, obwohl diese Inhalte andere Stakeholder und Entscheidungsräume haben.
2. **Normative Aussage und Lerninhalt werden vermischt.** Ein didaktischer Überblick kann nicht dieselbe Verbindlichkeit haben wie ein Security-Standard.
3. **Lebenszyklen passen nicht.** Eine konkrete Entscheidung sollte historisch stabil bleiben; eine Engineering-Guideline muss dagegen mit Java/Spring-Versionen weiterentwickelt werden.
4. **Review-Daten verlieren Aussagekraft.** Viele ältere Dokumente enthalten Reviewdaten aus 2025, obwohl ihre technische Umgebung weitergelaufen ist. Ein Reviewmechanismus muss daher an reale Trigger gebunden sein.

## 3. Das Zielmodell

### 3.1 Architecture Principles (`AP`)

Prinzipien beantworten: **Welche dauerhafte Leitplanke verwenden wir, um Entscheidungen konsistent zu treffen?**

Beispiele:
- Security by Design.
- Einfachheit vor verteilter Komplexität.
- Führende Datenverantwortung explizit machen.
- Betriebsfähigkeit ist Teil der Architektur.
- Automatisierbare Nachweise bevorzugen.

Prinzipien sind bewusst technologiearm. Sie ändern sich selten und dienen als Begründungsrahmen.

### 3.2 Architecture Decision Records (`ADR`)

ADRs beantworten: **Welche konkrete architekturrelevante Entscheidung treffen wir in diesem Kontext?**

Ein ADR ist ein *Point-in-Time-Record*. Er darf nicht zu einer allgemeinen Lehrbuchseite anwachsen. Er dokumentiert genau eine Entscheidung.

Pflichtstruktur:

1. Titel und ID
2. Status
3. Entscheidungsdatum
4. Owner / Entscheider / Consulted / Informed
5. Kontext und Problem
6. Entscheidungsfrage
7. Decision Drivers / Kriterien
8. betrachtete Optionen
9. Entscheidung
10. Begründung
11. Konsequenzen und Trade-offs
12. Confirmation / Nachweis
13. Risiken und Kompensationsmaßnahmen
14. Beziehungen / supersedes / superseded by
15. Review-Trigger

### 3.3 Architecture Standards (`AS`)

Standards beantworten: **Welche verbindliche Regel gilt über mehrere Systeme/Teams hinweg?**

Beispiele:
- OpenAPI-Mindeststandard.
- OAuth2/OIDC Resource-Server-Standard.
- Logging-/Correlation-Standard.
- AsyncAPI-Standard.
- GitOps-Standard.

Ein Standard enthält:
- Geltungsbereich
- normative Regeln (`MUSS`, `DARF NICHT`, `SOLLTE`)
- Ausnahmeprozess
- technische Nachweise
- Referenzimplementierung
- Version und Review-Trigger.

### 3.4 Reference Architectures (`RA`)

Referenzarchitekturen beantworten: **Wie sieht ein wiederverwendbares, zulässiges Lösungsbild für eine Problemklasse aus?**

Beispiele:
- Java-Service-Golden-Path.
- Event-Driven Reference Architecture.
- Observability Reference Architecture.
- Zero-Trust Service-to-Service Architecture.

Sie enthalten Bausteine, Schnittstellen, Deployment, Verantwortlichkeiten, Qualitätsziele und Variationspunkte.

### 3.5 Engineering Guidelines (`EG`)

Guidelines beantworten: **Wie setzen Entwickler und Reviewer eine technische Regel konkret und wartbar um?**

Beispiele:
- Records für transparente Datenträger.
- Sealed Types für geschlossene Hierarchien.
- JavaDoc-Regeln.
- Test Data Builder.
- MapStruct an Modellgrenzen.

Sie sind präzise, technologie- und versionsnah und dürfen sich weiterentwickeln.

### 3.6 Operating Models (`OM`)

Operating Models beantworten: **Wer entscheidet, wer betreibt, wer prüft und wie läuft der Prozess?**

Beispiele:
- Architecture Decision Process.
- Incident Management.
- Technology Radar Governance.
- API Lifecycle Management.
- Platform-Team Operating Model.

### 3.7 Runbooks / Operational Practices (`RP`)

Runbooks beantworten: **Was tun wir konkret bei einem operativen Zustand oder Fehlerfall?**

Beispiele:
- DLQ-Reprocessing.
- Restore-Test.
- JFR/Profiling bei Performance-Incident.
- Replica-Fallback.

### 3.8 Learning Notes (`LN`)

Learning Notes beantworten: **Wie erkläre ich ein Konzept so, dass es verstanden und auf neue Situationen übertragen werden kann?**

Beispiele:
- Was ist Architektur?
- Qualitätsziele und Szenarien.
- arc42/C4/UML.
- Pattern-Synthesen.

Sie sind bewusst nicht normativ.

## 4. Qualitätsmodell für jedes Dokument

Jedes Dokument wird entlang von sieben Dimensionen geprüft:

### Q1 — Entscheidungs-/Aussageklarheit
Ist klar, ob das Dokument erklärt, entscheidet oder vorschreibt?

### Q2 — Fachliche Korrektheit
Sind technische Aussagen mit Primärquellen vereinbar?

### Q3 — Kontexttreue
Werden Heuristiken als Heuristiken gekennzeichnet und nicht als universelle Grenzwerte verkauft?

Beispiel: `> 100 Mio Rows` kann eine Erfahrungsheuristik sein, aber kein allgemeines Gesetz für Partitionierung.

### Q4 — Versions- und Zeitbezug
Sind Versionsangaben aktuell und zeitlich eingeordnet? Aussagen zu Preisen, Performance, Tool-Versionen und Produktfeatures werden datiert.

### Q5 — Trade-off-Qualität
Zeigt das Dokument auch Nachteile, Voraussetzungen, Alternativen und Failure Modes?

### Q6 — Confirmation / Evidence
Kann die Aussage oder Entscheidung geprüft werden?

Beispiele:
- ArchUnit-Test
- Contract Test
- OpenAPI Lint
- Security Scan
- SLO/Metric
- Restore-Test
- Architecture Review

### Q7 — Governance-Fähigkeit
Sind Scope, Owner, Ausnahme, Review-Trigger und Beziehungen zu anderen Artefakten klar?

## 5. Validierungsstufen

Jedes migrierte Dokument erhält einen Status:

- `VERIFIED` — Kernaussagen mit belastbaren Primärquellen geprüft.
- `VERIFIED_WITH_CAVEATS` — fachlich tragfähig, aber mit expliziten Kontextgrenzen/Heuristiken.
- `NEEDS_UPDATE` — wesentliche Teile veraltet oder technisch fragwürdig.
- `SOURCE_GAP` — Thema/ID ist referenziert, aber die zugrunde liegende Datei fehlt.
- `NOT_YET_VALIDATED` — noch nicht in der v2-Migration bearbeitet.

## 6. Quellenhierarchie

Bei technischer Validierung gilt folgende Priorität:

1. Norm / RFC / JEP / offizielle Spezifikation.
2. offizielle Hersteller-/Projekt-Dokumentation.
3. etablierte Security-/Operations-Quellen (z. B. OWASP, NIST, BSI, Google SRE je nach Thema).
4. Fachliteratur.
5. Blog/Community nur ergänzend.

Eine Sekundärquelle darf keine Primärquelle ersetzen, wenn die Aussage direkt in Spezifikation oder Herstellerdokumentation prüfbar ist.

## 7. Schreibstil — Coach-Master-Ton

Jedes Dokument soll lehren, wie ein erfahrener Architekt denkt. Die Struktur folgt deshalb einer didaktischen Dramaturgie:

1. **Problem verstehen** — Was geht real schief?
2. **Architektenfrage formulieren** — Was muss tatsächlich entschieden werden?
3. **Konzepte erklären** — Welche Mechanismen muss man verstehen?
4. **Optionen abwägen** — Was gewinnt und verliert jede Option?
5. **Regel/Entscheidung ableiten** — Was gilt in diesem Kontext?
6. **Umsetzung zeigen** — Wie sieht die technische Realisierung aus?
7. **Failure Modes erklären** — Woran scheitert die Lösung typischerweise?
8. **Nachweis definieren** — Wie wissen wir, dass sie funktioniert?
9. **EA-Abstraktion herstellen** — Welche übertragbare Architekturlektion steckt darin?
10. **Review-Fragen geben** — Welche Fragen stellt ein Senior/Enterprise Architect?

Das Dokument soll weder akademisch trocken noch tutorialhaft beliebig wirken. Es soll die Denkfähigkeit des Lesers erhöhen.

## 8. ADR-Template v2

```markdown
# ADR-XXX — Entscheidungstitel

## Metadaten
- Status: Draft | Proposed | Accepted | Superseded | Deprecated
- Entscheidungsdatum:
- Owner:
- Entscheider:
- Consulted:
- Informed:
- Architekturdomäne:
- Scope:
- Supersedes:
- Superseded by:
- Review-Trigger:

## Executive Summary
2–5 Sätze: Problem, Entscheidung, Hauptnutzen, wichtigster Trade-off.

## Kontext und Problem
Fakten und Randbedingungen, lösungsneutral.

## Entscheidungsfrage
Eine klare Frage.

## Decision Drivers
Messbare oder zumindest explizite Kriterien.

## Betrachtete Optionen
Mindestens Ist-/konservative Option und realistische Alternativen.

## Entscheidung
Aktiv, eindeutig, mit Scope.

## Begründung
Warum diese Option unter diesen Treibern?

## Konsequenzen
### Positiv
### Negativ / Trade-offs
### Neue Pflichten

## Risiken und Kompensationsmaßnahmen

## Confirmation / Evidence
Wie wird die Umsetzung geprüft?

## Umsetzung und Übergang
Nur soweit notwendig; Details ggf. in Standard/RA/Guideline auslagern.

## Beziehungen
Links auf Standards, RAs, Guidelines, andere ADRs.

## Nachträge
Nur datierte neue Evidenz; Entscheidungshistorie nicht still verändern.
```

## 9. Standard-Template v2

```markdown
# AS-XXX — Standardtitel

## Metadaten
- Status: Active | Review Due | Deprecated | Retired
- Version:
- Owner:
- Scope:
- Letzte fachliche Prüfung:
- Review-Trigger:

## Zweck
## Geltungsbereich
## Begriffe
## Normative Regeln
### MUSS
### DARF NICHT
### SOLLTE
## Referenzimplementierung
## Confirmation / automatisierte Nachweise
## Ausnahmeprozess
## Migration für Bestand
## Risiken / Grenzen
## Quellen
## Änderungsverlauf
```

## 10. Guideline-Template v2

```markdown
# EG-XXX — Guideline-Titel

## Kurzfassung
## Wann einsetzen?
## Wann nicht einsetzen?
## Technisches Konzept
## Empfohlene Umsetzung
## Gegenbeispiele / Anti-Patterns
## Testing und Review
## Failure Modes
## Architektenperspektive
## Validierungsstatus und Quellen
```

## 11. Governance-Prozess

1. Frage entsteht in Review, Entwicklung, Betrieb oder Transformation.
2. Zuerst Artefakttyp wählen — nicht automatisch ADR.
3. Bei Entscheidung: Kontext, Drivers und Optionen dokumentieren.
4. Bei Standard: Scope, normative Regeln und Ausnahmeweg definieren.
5. Primärquellen prüfen.
6. Evidence/Confirmation festlegen.
7. Review durch relevante Rollen.
8. Veröffentlichen und im Katalog verlinken.
9. Umsetzung über CI/CD, Reviews, Plattform oder Abnahme prüfbar machen.
10. Review-Trigger beobachten; bei geänderter Entscheidung neuen ADR erzeugen.

## 12. Was bewusst nicht mehr akzeptiert wird

- erfundene Universalgrenzwerte ohne Kontext.
- „Best Practice“ ohne Problembezug.
- ein ADR, der gleichzeitig fünf Entscheidungen trifft.
- Versionsnummern ohne zeitliche Einordnung.
- Security-Regeln nur aus Blogposts.
- Toolpräferenz ohne Qualitätsziel.
- ein positiver Werbetext ohne Trade-offs.
- Akzeptanzkriterien, die keine Architekturqualität prüfen.
- Reviewdaten, die nur kosmetisch verlängert werden.
- Codebeispiele, deren Status unklar ist.

## 13. Ziel für den Gesamtbestand

Nach der Migration soll ein Leser an jeder Stelle erkennen:

- Was ist Wissen?
- Was ist Prinzip?
- Was ist verbindlicher Standard?
- Was wurde konkret entschieden?
- Wie wird es umgesetzt?
- Wie wird es geprüft?
- Wer ist verantwortlich?
- Wann wird es neu bewertet?

Erst dann ist aus einer umfangreichen Sammlung ein belastbares Architecture-Governance-System geworden.
