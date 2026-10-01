---
id: AK-056
legacy_ids:
  - ADR-056
title: Modularer Monolith – Reference Architecture
artifact_type: reference-architecture
domain: application-architecture
status: active
maturity: reviewed
normative_level: recommended
last_validated: 2026-10-01
review_trigger:
  - wesentliche Änderung der Modul-/Teamstruktur
  - geplante Service-Extraktion
---

# AK-056 — Modularer Monolith als bewusstes Architekturmodell

## 1. Ziel

Ein modularer Monolith ist nicht „Microservices später vielleicht“ und nicht bloß ein Monolith mit Packages.

Er kombiniert:

- **eine deploybare Einheit**,
- mit **expliziten fachlichen Modulen**,
- kontrollierten Abhängigkeiten,
- klarer Ownership und
- technisch überprüfbaren Grenzen.

Er ist besonders wertvoll, wenn fachliche Modularität benötigt wird, aber verteilte Betriebs- und Datenkomplexität noch nicht gerechtfertigt ist.

## 2. Referenzstruktur

```text
Application
├─ case-management
│  ├─ api
│  ├─ application
│  ├─ domain
│  └─ internal
├─ document
├─ notification
└─ shared-foundation   ← klein und bewusst begrenzt
```

Die konkrete Packageform ist nicht normativ.

Wichtig sind:

- erkennbare Modulverantwortung,
- definierte öffentliche Modul-API,
- interne Elemente außerhalb des Vertrags,
- überprüfte Dependency-Richtung.

## 3. Modulgrenzen

Ein Modul SOLLTE nach fachlicher oder stabiler technischer Verantwortung geschnitten werden.

Schlecht:

```text
controller/
service/
repository/
```

als einzige Top-Level-Struktur für das gesamte System.

Stärker:

```text
case/
document/
notification/
```

mit internen Schichten je Modul, wenn nötig.

## 4. Modulkommunikation

Mögliche Formen:

### direkter Modulvertrag

Für synchrone, klare Abhängigkeit innerhalb desselben Prozesses.

### internes Event

Für lose Kopplung oder mehrere Reaktionen.

Wichtig:

> Ein internes Modul-Event muss nicht automatisch in einen externen Broker publiziert werden.

## 5. Datenownership

Ein modularer Monolith kann eine gemeinsame physische Datenbank besitzen und trotzdem logische Ownership definieren.

Regeln können sein:

- Tabellen einem Modul zuordnen,
- kein direkter Fremdtabellenzugriff,
- Zugriff über Modul-API,
- getrennte Schemas wo sinnvoll.

Schema-per-module ist eine Option, kein universelles Muss.

## 6. Transaktionen

Ein Vorteil des Modulithen ist, dass fachlich notwendige lokale ACID-Transaktionen ohne verteilte Protokolle möglich bleiben.

Das darf aber nicht dazu führen, dass beliebige Module dauerhaft in riesigen Cross-Module-Transaktionen gekoppelt werden.

Review-Frage:

> Gehört diese Konsistenzanforderung wirklich über beide Modulgrenzen hinweg in eine atomare Transaktion?

## 7. Fitness Functions

Modulgrenzen SOLLTEN automatisiert prüfbar sein, zum Beispiel:

- verbotene Imports,
- keine Zyklen,
- nur definierte Modul-APIs sichtbar,
- keine Cross-Module-Repository-Zugriffe.

Werkzeuge können Spring Modulith, ArchUnit oder Build-Module sein. Das Prinzip ist toolunabhängig.

## 8. Ownership

Für jedes Modul sollte geklärt sein:

- fachlicher Owner,
- technischer Owner/Team,
- Datenverantwortung,
- öffentliche Schnittstellen,
- kritische Qualitätsziele.

## 9. Deployment

Ein Deployment bedeutet:

- gemeinsame Releaseeinheit,
- gemeinsamer Runtime-Prozess beziehungsweise Satz eng gebundener Prozesse,
- gemeinsame Skalierung der deploybaren Einheit.

Das ist ein bewusster Trade-off gegenüber Microservices.

## 10. Extraktionsfähigkeit

Ein guter Modulith hält eine spätere Service-Extraktion möglich, ohne sie zu versprechen.

Vor Extraktion prüfen:

- eigener fachlicher Kontext?
- klares Datenownership?
- wenige Cross-Module-Transaktionen?
- eigener Skalierungs-/Release-Treiber?
- eigenes Betriebsownership?

Erst dann ist Extraktion ein ADR-Kandidat.

## 11. Anti-Patterns

- „Modulith“ als Name für ungeordneten Monolithen.
- Shared Package wird zum globalen Müllplatz.
- jedes Modul darf jede Tabelle lesen.
- interne Events werden automatisch externe Kafka-Events.
- Module werden nur nach technischen Layern geschnitten.
- Microservice-Extraktion als Erfolgskriterium des Modulithen.

## 12. Quellen

- Spring Modulith Documentation  
  https://spring.io/projects/spring-modulith
- David Parnas — Information Hiding
- AK-077 — Modulith vs. Microservices
- AK-079 — Modulith Strategy

## 13. Merksatz

> Der Wert eines Modulithen liegt nicht darin, dass er „noch keine Microservices“ hat. Er liegt darin, **fachliche Grenzen und Ownership zu beweisen, bevor verteilte Komplexität eingeführt wird**.
