---
id: AK-079
legacy_ids:
  - ADR-079
title: Modulith Strategy – Governance, Metriken und Extraktionspfade
artifact_type: reference-architecture
domain: application-architecture
status: active
maturity: reviewed
normative_level: recommended
last_validated: 2026-09-28
review_trigger:
  - Modulgrenzen verändern sich wesentlich
  - wiederkehrende Cross-Module-Verletzungen
  - Service-Extraktion wird vorgeschlagen
---

# AK-079 — Modulith Strategy

## 1. Rolle im Wissenssystem

AK-056 beschreibt den modularen Monolithen als technische Referenzarchitektur.
AK-077 hilft bei der Entscheidung Modulith vs. Microservices.

AK-079 beantwortet die dritte Frage:

> **Wie halten wir einen Modulith über Jahre modular und wie erkennen wir, ob eine Grenze später zu einem eigenständigen Service werden sollte?**

## 2. Modulvertrag

Jedes relevante Modul SOLLTE explizit machen:

- fachliche Verantwortung,
- öffentliche API,
- interne Elemente,
- Datenownership,
- erlaubte Abhängigkeiten,
- publizierte Events,
- Owner.

Beispiel:

```text
Module: document
Responsibility: Dokument erzeugen und verwalten
Public API: DocumentService
Owned data: document_*
Published events: DocumentCreated
Forbidden: direkter Zugriff auf case-Repository
```

## 3. Dependency Direction

Abhängigkeiten dürfen nicht zufällig entstehen.

Mögliche Policy:

```text
Feature Module
→ gemeinsame technische Foundation

Feature A
-X→ internals von Feature B
```

Zyklen zwischen Fachmodulen SOLLTEN vermieden beziehungsweise explizit analysiert werden.

## 4. Datenregeln

Gemeinsame physische DB bedeutet nicht gemeinsame Datenhoheit.

Mögliche Reifegrade:

1. Tabellenkonvention je Modul.
2. Repository Ownership.
3. getrennte Schemas.
4. getrennte Datenbanken bei tatsächlicher Service-Extraktion.

Nicht jedes Modul braucht ab Tag eins eigenes Schema.

## 5. Cross-Module Query

Drei Optionen:

### API Call im Prozess

Einfach, wenn Laufzeit-/Kopplungskosten akzeptabel sind.

### Read Projection

Für häufige lesende Aggregationen, wenn Datenkopie und Aktualität bewusst geregelt sind.

### Shared Read View

Kann für Reporting sinnvoll sein, darf aber nicht unbemerkt Ownership unterlaufen.

## 6. interne Events

Interne Events können Reaktionen entkoppeln, ohne sofort Brokerkomplexität einzuführen.

Prüfen:

- synchron oder after-commit?
- Fehlerbehandlung?
- Transaktionssemantik?
- wird das Event später extern relevant?

Ein Modulith sollte nicht einen Kafka-Cluster simulieren, wenn ein lokaler Methodenvertrag besser passt.

## 7. Fitness Functions

Mindestens sinnvolle Kandidaten:

- Modulabhängigkeiten,
- keine verbotenen internen Package-Zugriffe,
- keine Zyklen,
- Datenzugriff nur über Owner-Regeln,
- Modul-API sichtbar dokumentiert.

## 8. Modulgesundheit messen

Nicht eine „Modularity Score“-Zahl erfinden.

Stattdessen Trends beobachten:

- Anzahl Cross-Module-Abhängigkeiten,
- Change Coupling,
- gemeinsame Commits über Module,
- Cross-Module-Transaktionen,
- Incident-Ursachen,
- Wartezeit zwischen Modulowner-Teams.

Diese Daten helfen bei Analyse, entscheiden aber nicht automatisch über Extraktion.

## 9. Extraction Triggers

Ein Modul ist Service-Kandidat, wenn beispielsweise:

- wirklich unabhängige Releases benötigt werden,
- klarer fachlicher Owner existiert,
- Datenownership stabil ist,
- eigene Skalierungs-/Security-Anforderung besteht,
- Cross-Module-Transaktionen gering/beherrschbar sind,
- Team den Service betreiben kann.

Extraktion wird als **neues ADR** entschieden.

## 10. Transition

```text
Modul im Monolith
→ Vertrag stabilisieren
→ Datenzugriffe bereinigen
→ externe Adapter isolieren
→ optional Async Boundary vorbereiten
→ Service extrahieren
→ alte interne Pfade entfernen
```

Nicht jeder Schritt ist immer nötig.

## 11. Anti-Patterns

- Modulith ohne automatisierte Grenzprüfung.
- „Shared“ wächst schneller als Fachmodule.
- Cross-Module SQL überall.
- Service-Extraktion wegen Dateigröße statt Architekturtreiber.
- Module entsprechen technischen Layern statt Verantwortungen.

## 12. Coach-Merksatz

> Ein Modulith bleibt nur dann Architektur und wird nicht wieder zum Big Ball of Mud, wenn **Grenzen einen Owner, einen Vertrag und überprüfbare Regeln besitzen**.
