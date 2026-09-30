---
id: AK-084
legacy_ids:
  - ADR-084
title: Kopplung, Kohäsion und Information Hiding
artifact_type: architecture-principle
domain: software-architecture
status: active
maturity: reviewed
normative_level: recommended
last_validated: 2026-09-30
review_trigger:
  - grundlegende Änderung der Modularitäts- oder Architekturstandards
---

# AK-084 — Änderbarkeit beginnt bei Abhängigkeiten

## 1. Kernidee

Viele Entwurfsprinzipien lassen sich auf zwei Fragen zurückführen:

1. **Was gehört fachlich oder technisch zusammen?**
2. **Wer muss von wem wie viel wissen?**

Daraus entstehen die Begriffe Kohäsion und Kopplung.

Ein nützliches Zielbild ist:

```text
hohe Kohäsion
+
geringe unnötige Kopplung
=
Änderungen bleiben lokaler und verständlicher
```

Das ist keine mathematische Formel und keine Garantie für Wartbarkeit. Es ist ein Strukturprinzip.

## 2. Kohäsion — was gehört zusammen?

Eine Einheit besitzt hohe Kohäsion, wenn ihre Elemente aus demselben fachlichen oder technischen Grund zusammengehören.

Beispiele hoher Kohäsion:

- Regeln zur Berechnung eines fachlichen Status,
- Funktionen rund um eine klar definierte Integrationsschnittstelle,
- ein Modul für Dokumentenerzeugung,
- ein Value Object samt seiner Invarianten.

Warnsignal niedriger Kohäsion:

```text
CommonUtils
├─ calculateTax()
├─ sendMail()
├─ generatePdf()
├─ maskIban()
└─ parseDate()
```

Der gemeinsame Grund lautet hier nur: „Wir wussten nicht, wohin damit.“

## 3. Kopplung — welches Wissen breitet sich aus?

Kopplung beschreibt Abhängigkeit zwischen Teilen eines Systems.

Nicht jede Kopplung ist schlecht. Ein System ohne Abhängigkeiten würde keine Zusammenarbeit zwischen Teilen besitzen.

Die bessere Frage lautet:

> **Welche Kopplung ist fachlich notwendig – und welche entsteht nur durch unsere Struktur?**

### Strukturelle Kopplung

Komponente A muss interne Struktur von B kennen.

### Datenkopplung

Mehrere Teile hängen von demselben Datenmodell oder Schema ab.

### Verhaltenskopplung

A kennt interne Ablaufannahmen von B.

### Zeitliche Kopplung

A und B müssen gleichzeitig verfügbar sein oder in einer bestimmten Reihenfolge ausgeführt werden.

### Deployment-Kopplung

Änderungen müssen gemeinsam gebaut oder ausgerollt werden.

### organisatorische Kopplung

Eine technische Änderung benötigt regelmäßig Abstimmungen zwischen mehreren Verantwortungsbereichen.

Gerade die letzten beiden werden in reinem Code-Design häufig übersehen.

## 4. Temporale Kopplung differenziert betrachten

Eine synchrone Kette erhöht häufig die zeitliche Abhängigkeit:

```text
Portal
→ Service A
→ Service B
→ externer Provider
```

Aber daraus folgt **nicht automatisch**, dass Events besser sind.

Asynchrone Kommunikation tauscht bestimmte Kopplungen gegen andere:

```text
weniger gleichzeitige Verfügbarkeit
+
bessere zeitliche Entkopplung

aber

mehr Eventual Consistency
mehr Fehler-/Retry-Logik
mehr Observability-Bedarf
mehr Schema-/Event-Governance
```

Architekturarbeit bedeutet, diese Kosten bewusst gegeneinander abzuwägen.

## 5. Information Hiding

David Parnas formulierte Modularisierung entlang von Designentscheidungen, die wahrscheinlich geändert werden.

Das Prinzip lautet:

> Verstecke volatile oder komplexe Entscheidungen hinter einer stabileren Grenze.

Beispiele:

- ein externer Registervertrag hinter einem Adapter,
- eine Berechnungsstrategie hinter einer fachlichen Schnittstelle,
- Datenbankdetails hinter einem Repository-Vertrag,
- Provider-spezifische IAM-Claims hinter einem internen Identity Model.

Information Hiding ist stärker als `private`.

Es fragt:

> Welches Wissen darf außerhalb dieses Moduls überhaupt existieren?

## 6. Kopplung auf verschiedenen Ebenen

### Code

Klassen, Methoden, Bibliotheken.

### Modul

Packages, Module, interne APIs.

### Service

Netzwerkverträge, Daten, Deployment.

### Organisation

Teams, Fachbereiche, Plattformen, Dienstleister.

### Enterprise

Fachverfahren, Register, Behörden, gemeinsame Plattformen, Standards.

Der gleiche Mechanismus wiederholt sich: Je mehr Partner interne Details voneinander kennen müssen, desto größer wird der Änderungsradius.

## 7. Datenkopplung ist oft stärker als API-Kopplung

Zwei Services können getrennte REST-Endpunkte besitzen und trotzdem stark gekoppelt sein, wenn beide:

- dieselben Tabellen verändern,
- dieselben fachlichen Datenmodelle voraussetzen,
- dieselben Transaktionen benötigen.

Deshalb reicht ein Microservice-Diagramm nicht, um Entkopplung zu beweisen.

Prüfe zusätzlich:

- Datenownership,
- Schemaabhängigkeiten,
- gemeinsame Deployments,
- gemeinsame Releasezyklen,
- organisatorische Übergaben.

## 8. Messbarkeit

Kopplung kann teilweise technisch gemessen werden:

- Dependency Graphs,
- zyklische Abhängigkeiten,
- Modulverletzungen,
- API-/Schemaabhängigkeiten,
- Change Coupling in Git-Historie,
- Deployment Coupling.

Aber eine einzelne Kennzahl wie `Ce = 7` beweist nicht automatisch schlechtes Design.

Sie ist ein Hinweis für Analyse.

## 9. Schritt-für-Schritt-Analyse

Wenn Änderungen zu viele Bereiche berühren:

1. Welche fachliche Änderung wurde ausgelöst?
2. Welche Komponenten mussten deshalb geändert werden?
3. Warum mussten sie geändert werden?
4. War die Kopplung fachlich notwendig?
5. Welches interne Wissen hat sich über Grenzen ausgebreitet?
6. Kann eine volatile Entscheidung verborgen werden?
7. Würde eine neue Schnittstelle echte Kopplung reduzieren oder nur zusätzliche Indirektion erzeugen?
8. Wie wird die gewünschte Grenze überprüft?

## 10. Architecture Guardrails

Geeignete Regeln können sein:

- Module dürfen nur über definierte APIs kommunizieren.
- Domain Packages dürfen keine Frameworkadapter importieren.
- Schema-Ownership wird explizit dokumentiert.
- direkte Datenbankzugriffe über Verantwortungsgrenzen sind verboten oder begründungspflichtig.
- Event-/API-Verträge besitzen Owner.
- zyklische Modulabhängigkeiten werden automatisiert erkannt.

Die konkrete Normativität gehört in Standards, nicht in dieses Principle.

## 11. Anti-Patterns

### „Events = entkoppelt“

Falsch. Events können starke semantische, Schema- und Lifecycle-Kopplung erzeugen.

### „Interfaces = geringe Kopplung“

Ein Interface hilft nur, wenn es tatsächlich ein relevantes Detail verbirgt.

### „Microservices = unabhängig“

Shared Database, koordinierte Releases oder dauernde Teamabstimmung können das Gegenteil zeigen.

### „Niedrige Kopplung um jeden Preis“

Zu viele Grenzen können Verständlichkeit und Performance verschlechtern.

## 12. Quellen

- David L. Parnas, *On the Criteria To Be Used in Decomposing Systems into Modules* (1972)
- ISO/IEC 25010:2023 — Maintainability / Modularity als Qualitätskontext
- AK-025 — SOLID
- AK-085 — Sozio-technische Architektur
- AK-089 — Strategic DDD

## 13. Merksatz

> Kopplung zeigt sich nicht nur daran, wer wen aufruft. Entscheidend ist, **welches Wissen, welche Verfügbarkeit, welche Daten und welche Abstimmung eine Änderung über Grenzen hinweg erzwingt**.
