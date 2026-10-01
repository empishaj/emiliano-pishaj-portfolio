---
id: AK-082
legacy_ids:
  - ADR-082
title: Qualitätsziele und Qualitätsszenarien
artifact_type: learning-guide
domain: architecture-fundamentals
status: active
maturity: reviewed
normative_level: informative
last_validated: 2026-10-01
review_trigger:
  - neue Ausgabe ISO/IEC 25010
  - wesentliche Änderung der Qualitätsziele des betrachteten Systems
---

# AK-082 — Von Qualitätswünschen zu entscheidbaren Szenarien

## 1. Kernidee

Technologie kommt **nach** dem Qualitätsziel.

Falsche Reihenfolge:

```text
Microservices wählen
→ System bauen
→ hoffen, dass Skalierung, Wartbarkeit und Betrieb passen
```

Bessere Reihenfolge:

```text
Stakeholder-Concern
→ Qualitätsziel
→ messbares Szenario
→ Architektur-Optionen
→ Trade-off
→ Entscheidung
→ Evidence
```

## 2. Qualitätsmodelle sind Vokabular, keine Prioritätenliste

ISO/IEC 25010 ist ein Referenzmodell für Produktqualität.

Wichtig für die Pflege dieses Compendiums: Die Ausgabe **ISO/IEC 25010:2011 ist zurückgezogen**. Die aktuelle Ausgabe ist ISO/IEC 25010:2023 und verwendet neun Produktqualitätsmerkmale.

Die Norm sagt dir aber nicht automatisch:

> „Performance ist wichtiger als Modifizierbarkeit.“

Diese Priorität entsteht aus dem konkreten Auftrag und den Stakeholder-Concerns.

Deshalb verwenden wir Qualitätsmodelle als:

- Vollständigkeitscheck,
- gemeinsames Vokabular,
- Ausgangspunkt für Szenarien.

Nicht als automatische Architekturentscheidung.

## 3. Von „schnell“ zu messbar

Schwach:

> Das System muss schnell sein.

Stärker:

> Unter der für den Normalbetrieb definierten Last beantwortet das System 95 % der Suchanfragen innerhalb des vereinbarten Latenzziels.

Noch besser ist ein vollständiges Szenario.

## 4. Szenario-Struktur

Ein Qualitätsszenario enthält:

| Element | Frage |
|---|---|
| Quelle | Wer oder was löst den Stimulus aus? |
| Stimulus | Was passiert? |
| Umgebung | Unter welchen Bedingungen? |
| betroffenes Artefakt | Was muss reagieren? |
| Response | Wie soll reagiert werden? |
| Messgröße | Woran erkennen wir Erfolg? |

Beispiel:

```text
Quelle:
Nutzer des Fachportals

Stimulus:
sendet eine Statusabfrage

Umgebung:
Normalbetrieb mit definierter Spitzenlast

Artefakt:
Portal → Gateway → Fachverfahren

Response:
Status wird geliefert oder ein fachlich definierter Ersatzpfad greift

Messgröße:
P95-Latenz, Erfolgsrate und Aktualität innerhalb der vereinbarten Grenzen
```

Die konkreten Werte werden aus Anforderungen und Messungen abgeleitet – nicht aus Beispielzahlen eines Compendiums.

## 5. Qualitätsziele priorisieren

Nicht jede Eigenschaft kann gleichzeitig maximiert werden.

Typische Spannungen:

```text
Konsistenz ↔ Verfügbarkeit
Security ↔ Bedienkomfort
Autonomie ↔ Standardisierung
Performance ↔ Kosten
Flexibilität ↔ Einfachheit
Time-to-market ↔ technische Absicherung
```

Priorisierung bedeutet nicht, eine Qualität „unwichtig“ zu machen.

Sie bedeutet:

> Bei Konflikten wissen wir, welche Konsequenz wir bewusst akzeptieren.

## 6. Business Driver → Quality Scenario

Architekturarbeit übersetzt Managementsprache in technische Prüfbarkeit.

```text
"Der Service darf den Fachprozess nicht aufhalten."
        ↓
Verfügbarkeit / Resilience
        ↓
Ausfallszenario
        ↓
Architekturmechanismus
        ↓
Failure-Test / SLO
```

Oder:

```text
"Wir müssen Anbieter wechseln können."
        ↓
Portability / Modifiability / Vendor Risk
        ↓
Wechselszenario
        ↓
Vertrags-/Adaptergrenzen
        ↓
Exit-Test / Migrationsplan
```

## 7. Qualitätsziele im Behördenkontext

Typische Concerns können zusätzlich sein:

- Nachvollziehbarkeit,
- Schutzbedarf,
- Datenminimierung,
- Barrierefreiheit,
- Interoperabilität,
- Wiederanlauf,
- Auditierbarkeit,
- Dienstleisterwechsel,
- Langzeitbetrieb,
- Migrationsfähigkeit,
- rechtliche Änderbarkeit.

Diese werden nicht einfach als technische NFR-Liste behandelt. Sie werden in konkrete Szenarien und Abnahmekriterien übersetzt.

## 8. Von Szenario zu Architekturentscheidung

Beispiel:

```text
Qualitätsziel:
Änderbarkeit einer Registerintegration

Szenario:
Registervertrag ändert sich, ohne dass Domänenlogik des Fachverfahrens
direkt vom externen Datenmodell abhängig werden soll.

Optionen:
A Direkte Nutzung des externen Schemas
B Mapping im Application Service
C Anti-Corruption Layer / Adapter

Bewertung:
Änderbarkeit, Komplexität, Testbarkeit, Mapping-Aufwand

Entscheidung:
wird im konkreten ADR dokumentiert.
```

AK-082 trifft die Entscheidung **nicht**. Es liefert die Methode.

## 9. Evidence-Kette

```text
Quality Scenario
→ Acceptance Criterion
→ Architecture Mechanism
→ Test / Metric / Review
→ Evidence
```

Beispiele:

- Performance → Lasttest + Runtime-SLI.
- Recovery → Restore-Test.
- Modularity → Dependency-/Architecture-Test.
- API-Kompatibilität → Contract-/Schema-Diff.
- Security → Control-Test/Review.
- Operability → Runbook- und Incident-Übung.

## 10. Anti-Patterns

### Beispielwert wird zum Standard

Ein Beispiel `P95 < 500 ms` darf nicht unbemerkt zur Unternehmensanforderung werden.

### Coverage = Wartbarkeit

Coverage ist höchstens ein Signal. Wartbarkeit entsteht aus Struktur, Verständlichkeit, Testbarkeit, Kopplung und Änderungsrisiko.

### Technologie als Qualitätsziel

„Wir wollen Kubernetes“ ist kein Qualitätsziel.

Frage stattdessen:

> Welches Problem soll eine Containerplattform lösen?

### Alles ist Priorität 1

Dann existiert keine Priorisierung.

## 11. Quellen

- ISO/IEC 25010:2023  
  https://www.iso.org/standard/78176.html
- ISO/IEC/IEEE 42010:2022  
  https://www.iso.org/standard/74393.html
- CMU/SEI – ATAM  
  https://www.sei.cmu.edu/library/architecture-tradeoff-analysis-method-collection/

## 12. Merksatz

> Ein Qualitätsziel ist erst architektonisch brauchbar, wenn erklärt werden kann, **welcher konkrete Stimulus unter welchen Bedingungen welche messbare Reaktion verlangt**.
