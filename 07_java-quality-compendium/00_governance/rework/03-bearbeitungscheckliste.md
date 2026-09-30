# Bearbeitungscheckliste

Stand: 2026-09-30
Branch: `rework/java-quality-compendium-2026`

Legende:

- `[ ]` offen
- `[~]` in Arbeit
- `[x]` abgeschlossen
- `[!]` Klärung / externer Validierungsbedarf

## AP1 — Vollständige Inventur

- [x] Branch für Überarbeitung angelegt
- [x] Hauptstruktur des Ordners erfasst
- [x] `README.md` geprüft
- [x] alle Dateien in `00_governance` geprüft
- [x] alle Dateien in `01_principles` geprüft
- [x] alle Dateien in `02_decision-guides` geprüft
- [x] alle Dateien in `03_standards` geprüft
- [x] alle Dateien in `04_reference-architectures` geprüft
- [x] alle Dateien in `05_operating-guides` geprüft
- [x] alle Dateien in `06_operating-models` geprüft
- [x] alle Dateien in `07_learning-guides` geprüft
- [x] alle historischen `QG-JAVA-*` geprüft
- [x] kompletten `v2/`-Bestand geprüft
- [x] Dubletten- und Rewrite-Matrix erstellt
- [x] Tone-of-Voice-Muster identifiziert
- [~] interne Links / Cross-References bereinigen

## AP2 — Zielstruktur

- [x] Artefakttypen als Ausgangsbasis identifiziert
- [x] endgültige Verzeichnisstruktur festgelegt
- [x] `08_engineering-guidelines` als eigene Kategorie festgelegt
- [x] Knowledge-ID `AK-*` und echte ADR-ID getrennt
- [x] keine künstlichen Portfolio-ADRs
- [x] Grundregel für Zusammenführen / Splitten definiert
- [~] Policy vs. Standard im Artifact Model noch präzisieren

## AP3 — Governance und Schreibstandard

- [ ] README final überarbeiten
- [ ] Artifact Model final überarbeiten
- [ ] Validation Policy final überarbeiten
- [ ] ADR Lifecycle/Template final überarbeiten
- [ ] Metadatenstandard final vereinheitlichen
- [x] Schreibstilregel: keine `Coach-*`-Labels
- [~] `Coach-Perspektive` ersetzen
- [~] `Coach-Merksatz` durch `Merksatz` ersetzen
- [~] `Coach-Prüfung` durch `Prüfung`/`Prüffragen` ersetzen
- [~] `Coach-Ziel` neutral umbenennen

## AP4 — Konsolidierung der drei Generationen

- [x] Mapping `QG-JAVA-*` → `AK-*` → `v2/*` erstellt
- [x] `v2/` vollständig geprüft
- [x] relevante v2-Inhalte `001–005` übernommen
- [x] `v2/` entfernt
- [~] je Legacy-Thema kanonische Fassung umsetzen
- [~] redundante Flat-Files entfernen
- [~] Legacy-Querverweise auf `QG-JAVA-*` bereinigen
- [~] Verweise auf fehlendes `QG-JAVA-006` entfernen/ersetzen

## AP5 — Fachliche Validierung

- [x] Java/JVM für AK-001 bis AK-005
- [~] Testing
- [~] Application Security
- [~] REST/OpenAPI
- [~] AsyncAPI/Eventing/Kafka
- [x] OAuth2/OIDC/JWT im kanonischen Decision Guide bereits bereinigt
- [~] Datenbank/JPA/PostgreSQL/Flyway
- [~] Docker/Kubernetes/GitOps/IaC
- [~] Observability/SLO/Incident
- [~] Supply Chain/DevSecOps
- [~] Privacy/Data Governance
- [~] AI/LLM

## AP6 — Neufassung

- [~] Principles — vorhandene Basis gut, Stilbereinigung offen
- [~] Decision Guides — vorhandene Basis gut, Stilbereinigung offen
- [~] Standards/Policies — vorhandene Basis gut, Präzisierung offen
- [~] Reference Architectures — vorhandene Basis gut
- [~] Engineering Guidelines — AK-001 bis AK-005 erstellt; weitere offen
- [~] Operating Guides — Basis vorhanden; Chaos Engineering offen
- [~] Operating Models — Basis vorhanden; Tonbereinigung offen
- [~] Learning Guides — Basis vorhanden; Code Smells/Java Patterns offen

## AP7 — Navigation

- [ ] Hauptindex nach Artefakttyp
- [ ] Index nach Domäne
- [ ] Lernpfad Software Architecture
- [ ] Lernpfad Integration
- [ ] Lernpfad Security/Privacy
- [ ] Lernpfad Platform/Operations
- [ ] Lernpfad Architecture Governance
- [ ] alle internen Links validieren

## AP8 — Löschen / Archivieren

- [~] redundante Flat-Files löschen
- [x] redundante `v2/`-Dateien gelöscht
- [ ] veraltete Migrationsartefakte am Ende entfernen
- [ ] leere / irrelevante Dateien entfernen
- [~] keine wichtige Aussage unbeabsichtigt verlieren

## AP9 — Gesamtprüfung

- [ ] Suche nach `Coach`
- [ ] Suche nach toten Legacy-Links
- [ ] Suche nach unbelegten Versionsständen
- [ ] Suche nach abgelaufenen Review-Daten
- [ ] Suche nach universellen Schwellenwerten ohne Kontext
- [ ] Suche nach erfundenen Rollen/Gremien/Projektentscheidungen
- [ ] konsistente Begriffe
- [ ] konsistente Dateinamen
- [ ] konsistente Metadaten
- [ ] vollständige Quellen
- [ ] finaler Diff geprüft

## AP10 — Abschluss

- [ ] Pull Request erstellen
- [ ] Änderungen im PR zusammenfassen
- [ ] finalen Review durchführen
- [ ] Merge nach `main`
