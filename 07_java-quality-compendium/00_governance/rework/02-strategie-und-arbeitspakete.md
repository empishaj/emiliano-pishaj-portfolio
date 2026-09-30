# Strategie und Arbeitspakete

Stand: 2026-09-30
Branch: `rework/java-quality-compendium-2026`

## Zielzustand

Am Ende existiert im Ordner `07_java-quality-compendium` genau **eine** kanonische Informationsarchitektur. Jedes Thema hat einen klaren Artefakttyp, einen eindeutigen Zweck, eine nachvollziehbare Quelle und eine definierte Beziehung zu anderen Themen.

Das Repository soll sich wie ein professionelles Architektur-Wissenssystem lesen, nicht wie eine Sammlung historisch gewachsener Einzelartikel.

## Leitprinzipien der Überarbeitung

1. **Eine kanonische Fassung pro Thema.**
2. **Echte ADRs nur für konkrete Entscheidungen.**
3. **Normative Standards werden von Lern- und Entscheidungshilfen getrennt.**
4. **Primärquellen vor Sekundärquellen.**
5. **Technische Aussagen werden auf aktuellen Stand geprüft.**
6. **Keine erfundenen Organisations-, Board- oder Projektrealitäten.**
7. **Keine künstliche Coaching-Sprache.**
8. **Keine pauschalen Schwellenwerte ohne Kontext oder Evidenz.**
9. **Querverweise müssen funktionieren und semantisch sinnvoll sein.**
10. **Löschen erst nach inhaltlicher Sicherung.**

## Arbeitspaket 1 — Vollständige Inventur

### Ziel

Jede Datei verstehen und klassifizieren.

### Schritte

1. Alle Dateien und Verzeichnisse erfassen.
2. Inhalt jeder Datei lesen.
3. Zweck und Flughöhe notieren.
4. Dubletten und Überschneidungen markieren.
5. veraltete oder fachlich problematische Aussagen markieren.
6. Tone-of-Voice-Probleme markieren.
7. externe Validierungsbedarfe notieren.
8. fehlende oder tote Querverweise identifizieren.

### Ergebnis

Ein vollständiger Bestandskatalog mit Entscheidung `KEEP`, `MERGE`, `REWRITE`, `DELETE`, `ARCHIVE` oder `VALIDATE`.

## Arbeitspaket 2 — Ziel-Informationsarchitektur festlegen

### Ziel

Eine einzige Struktur definieren.

### Prüfentscheidung

Die bereits begonnene Struktur wird gegen folgende Zielkategorien geprüft:

- `00_governance`
- `01_principles`
- `02_decision-guides`
- `03_standards`
- `04_reference-architectures`
- `05_operating-guides`
- `06_operating-models`
- `07_learning-guides`
- optional `08_engineering-guidelines`, falls Engineering-Guidelines langfristig nicht sinnvoll in Standards/Decision Guides aufgehen.
- optional `09_decisions`, ausschließlich für echte, konkrete ADRs.

### Ergebnis

Festgelegte Verzeichnisstruktur, Naming-Regeln, Metadaten und Navigationslogik.

## Arbeitspaket 3 — Governance und Schreibstandard bereinigen

### Ziel

Die Regeln des Systems selbst konsistent machen.

### Schritte

1. `README.md` als klare Einstiegs- und Navigationsseite überarbeiten.
2. Artefaktmodell finalisieren.
3. ADR-Lifecycle und Template finalisieren.
4. Validation Policy finalisieren.
5. Metadatenstandard definieren.
6. Begriffe vereinheitlichen.
7. alle Formulierungen mit `Coach-*` entfernen oder neutral umbenennen.
8. Regeln für Review, Supersession und Ausnahmeentscheidungen festlegen.

## Arbeitspaket 4 — Dubletten konsolidieren

### Ziel

Die drei parallelen Generationen auf eine kanonische Fassung reduzieren.

### Vorgehen je Thema

1. historische `QG-JAVA-*`-Fassung lesen,
2. vorhandene `AK-*`-Fassung lesen,
3. vorhandene `v2/*`-Fassung lesen,
4. Unterschiede notieren,
5. Quellen validieren,
6. beste Inhalte zusammenführen,
7. kanonische Datei erstellen/überarbeiten,
8. Links anpassen,
9. redundante Dateien löschen.

## Arbeitspaket 5 — Fachliche Validierung

### Ziel

Zeitabhängige oder sicherheitsrelevante Aussagen fachlich belastbar machen.

### Quellenpriorität

1. Spezifikation / RFC / Standard / Gesetz / offizielle Norm,
2. offizielle Produkt- oder Projektdokumentation,
3. etablierte Fachliteratur,
4. seriöse Sekundärquelle nur ergänzend.

### Validierungsgruppen

- Java / JVM / Concurrency,
- Spring / Testing,
- REST / OpenAPI / AsyncAPI / gRPC / GraphQL,
- OAuth2 / OIDC / JWT / Browser Security,
- Kafka / Events / Saga / Outbox,
- PostgreSQL / JPA / Flyway / Replication / Partitioning,
- Docker / Kubernetes / GitOps / IaC,
- Observability / SLO / Incident Management,
- Supply Chain / DevSecOps,
- AI / LLM,
- Datenschutz / Security / Behördenrelevanz.

## Arbeitspaket 6 — Inhaltliche Neufassung

### Ziel

Jedes kanonische Dokument nach seinem Artefakttyp neu strukturieren.

### Standardstruktur für Decision Guides

1. Einordnung
2. Problemklasse
3. Entscheidungstreiber
4. Optionen
5. Trade-offs
6. Wann geeignet / ungeeignet
7. Mess- und Evidenzbedarf
8. Fehlentscheidungen
9. Entscheidungsfragen
10. Quellen
11. verwandte Knowledge-Items
12. Merksatz

### Standardstruktur für Standards

1. Zweck
2. Geltungsbereich
3. Begriffe
4. normative Regeln
5. Begründung
6. Umsetzungshinweise
7. Verifikation
8. Ausnahmen
9. Review-Trigger
10. Quellen
11. verwandte Knowledge-Items

### Standardstruktur für Learning Guides

1. Einordnung
2. Kernkonzept
3. Zusammenhänge
4. Beispiele
5. Grenzen und Missverständnisse
6. Anwendung in Architekturarbeit
7. Prüffragen
8. Quellen
9. Merksatz

## Arbeitspaket 7 — Navigation und Querverweise

### Ziel

Das System muss ohne Kenntnis der Historie navigierbar sein.

### Schritte

1. zentrale README-Navigation,
2. Inhaltsindex nach Domäne,
3. Inhaltsindex nach Artefakttyp,
4. Lernpfade,
5. Querverweise prüfen,
6. tote Links entfernen,
7. Legacy-IDs nur dort behalten, wo sie zur Nachvollziehbarkeit nötig sind.

## Arbeitspaket 8 — Löschen und Archivieren

### Ziel

Redundanz konsequent entfernen.

### Löschkriterien

Eine Datei kann gelöscht werden, wenn:

- ihr gesamter relevanter Inhalt in einer besseren kanonischen Fassung enthalten ist,
- keine sinnvolle eigenständige Funktion mehr besteht,
- Querverweise aktualisiert wurden,
- keine historische Nachvollziehbarkeit verloren geht, die im aktuellen Portfolio noch benötigt wird.

Falls Historie wichtig bleibt, wird sie nicht im aktiven Navigationspfad geführt, sondern gezielt archiviert.

## Arbeitspaket 9 — Qualitätsprüfung

### Ziel

Vor Merge muss der Bestand als Ganzes konsistent sein.

### Prüfung

- keine konkurrierenden kanonischen Fassungen,
- keine `Coach-*`-Bezeichnungen im Bereich,
- keine erfundenen Entscheider oder Projektergebnisse,
- keine ungeprüften harten Versionsbehauptungen,
- keine toten internen Links,
- keine unbelegten universellen Schwellenwerte,
- konsistente IDs und Dateinamen,
- konsistente Metadaten,
- README vollständig,
- alle wichtigen Themen auffindbar,
- Git-Diff nachvollziehbar.

## Arbeitspaket 10 — Review und Merge

Nach Abschluss wird der Branch gegen `main` verglichen. Der finale Diff wird auf unbeabsichtigte Löschungen und inhaltliche Verluste geprüft. Erst danach wird ein Pull Request erstellt beziehungsweise gemergt.
