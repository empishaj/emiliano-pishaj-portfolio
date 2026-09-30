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
- [x] `00_governance/ARTIFACT-MODEL.md` geprüft
- [~] alle Dateien in `00_governance` prüfen
- [~] alle Dateien in `01_principles` prüfen
- [~] alle Dateien in `02_decision-guides` prüfen
- [~] alle Dateien in `03_standards` prüfen
- [~] alle Dateien in `04_reference-architectures` prüfen
- [~] alle Dateien in `05_operating-guides` prüfen
- [~] alle Dateien in `06_operating-models` prüfen
- [~] alle Dateien in `07_learning-guides` prüfen
- [ ] alle historischen `QG-JAVA-*` vollständig prüfen
- [ ] kompletten `v2/`-Bestand prüfen
- [ ] Dublettenmatrix fertigstellen
- [ ] Tone-of-Voice-Fundstellen vollständig erfassen
- [ ] interne Links / Cross-References prüfen

## AP2 — Zielstruktur

- [x] Artefakttypen als Ausgangsbasis identifiziert
- [ ] endgültige Verzeichnisstruktur festlegen
- [ ] Regeln für Knowledge-IDs festlegen
- [ ] Regeln für echte ADR-IDs festlegen
- [ ] Engineering-Guidelines als eigene Kategorie prüfen
- [ ] Policy vs. Standard verbindlich abgrenzen
- [ ] Regeln für Zusammenführen / Splitten definieren

## AP3 — Governance und Schreibstandard

- [ ] README final überarbeiten
- [ ] Artifact Model final überarbeiten
- [ ] Validation Policy final überarbeiten
- [ ] ADR Lifecycle/Template final überarbeiten
- [ ] Metadatenstandard festlegen
- [ ] Schreibstilregel aufnehmen: keine `Coach-*`-Labels
- [ ] `Coach-Perspektive` ersetzen
- [ ] `Coach-Merksatz` durch `Merksatz` ersetzen
- [ ] `Coach-Prüfung` durch `Prüfung`/`Prüffragen` ersetzen

## AP4 — Konsolidierung der drei Generationen

- [ ] Mapping `QG-JAVA-*` → `AK-*` → `v2/*` vollständig erstellen
- [ ] je Thema kanonische Fassung festlegen
- [ ] Redundanzen entfernen
- [ ] Legacy-IDs erhalten, wo sinnvoll
- [ ] überflüssiges `v2/` nach Migration entfernen
- [ ] historische Flat-Files nach Migration entfernen/archivieren

## AP5 — Fachliche Validierung

- [ ] Java/JVM
- [ ] Testing
- [ ] Application Security
- [ ] REST/OpenAPI
- [ ] AsyncAPI/Eventing/Kafka
- [ ] OAuth2/OIDC/JWT
- [ ] Datenbank/JPA/PostgreSQL/Flyway
- [ ] Docker/Kubernetes/GitOps/IaC
- [ ] Observability/SLO/Incident
- [ ] Supply Chain/DevSecOps
- [ ] Privacy/Data Governance
- [ ] AI/LLM

## AP6 — Neufassung

- [ ] Principles
- [ ] Decision Guides
- [ ] Standards/Policies
- [ ] Reference Architectures
- [ ] Engineering Guidelines
- [ ] Operating Guides
- [ ] Operating Models
- [ ] Learning Guides
- [ ] echte ADR-Beispiele nur falls sinnvoll und klar als Beispiel markiert

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

- [ ] redundante Flat-Files löschen
- [ ] redundante `v2/`-Dateien löschen
- [ ] veraltete Migrationsartefakte entfernen
- [ ] leere / irrelevante Dateien entfernen
- [ ] keine wichtige Aussage unbeabsichtigt verlieren

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
