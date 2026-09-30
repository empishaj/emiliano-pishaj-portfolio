# Strategie- und Umsetzungsplan

Stand: 2026-09-30

## Ziel

Das Verzeichnis wird von einer gewachsenen Sammlung in ein einheitliches Architecture-Knowledge-System überführt. Am Ende gibt es nur noch eine kanonische Navigation, eindeutige Artefakttypen, validierte Inhalte und nachvollziehbare Cross-References.

Die Migration erfolgt schrittweise. Legacy-Inhalte werden erst gelöscht, wenn ihr fachlicher Inhalt in der neuen Struktur vollständig übernommen oder bewusst verworfen wurde.

## Arbeitspaket 0 — Bestand sichern und erfassen

**Ziel:** Änderungen reversibel machen und den tatsächlichen Bestand verstehen.

Aufgaben:

- Arbeitsbranch von `main` anlegen.
- Root-Dateien, `AK-*`-Struktur und `v2/` getrennt erfassen.
- fehlende Quellen und Nummernlücken markieren.
- Doppelungen und fachliche Konflikte notieren.
- zeitabhängige Baselines identifizieren.

**Ergebnis:** belastbare Bestandsnotizen.

**Status:** abgeschlossen.

---

## Arbeitspaket 1 — Governance und Informationsarchitektur konsolidieren

**Ziel:** Eine einzige Struktur und ein einheitliches Dokumentmodell festlegen.

Aufgaben:

- `ARTIFACT-MODEL.md` als führendes Modell verwenden.
- ADR-Lifecycle vereinfachen: leichtgewichtiges Standardtemplate plus Erweiterungen für risikoreiche Entscheidungen.
- Validierungs- und Quellenpolicy beibehalten und sprachlich glätten.
- Migrationskatalog als Statusregister weiterführen.
- alle `Coach-*`-Formulierungen in Governance-Dateien entfernen.
- `v2/` nach Übernahme seiner relevanten Inhalte löschen.
- Root-README und Navigation erstellen.

**Definition of Done:** Es existiert genau ein beschriebenes Artefaktmodell und genau ein Migrationsstatus.

---

## Arbeitspaket 2 — Verzeichnisstruktur bereinigen

**Ziel:** Ein eindeutiger Navigationsweg.

Zielstruktur:

```text
00_governance
01_principles
02_decision-guides
03_standards
04_reference-architectures
05_engineering-guidelines
06_operating-models
07_operating-guides
08_learning-guides
99_legacy-source
```

Aufgaben:

- Engineering Guidelines aus `v2` übernehmen.
- vorhandene Operating Guides und Learning Guides in die Zielnummerierung verschieben.
- Legacy-Rootdateien in `99_legacy-source` verschieben, solange sie noch benötigt werden.
- keine doppelten aktiven Quellen im Root belassen.

**Definition of Done:** Der Root enthält nur README und Zielordner.

---

## Arbeitspaket 3 — Sprach- und Stilbereinigung

**Ziel:** Professionelle, normale Sprache ohne künstliche Coaching-Rhetorik.

Regeln:

- `Coach-Merksatz` → `Merksatz` oder Abschnitt entfernen.
- `Coach-Frage` → `Prüffrage`.
- `Coach-Ziel` → `Ziel`.
- `Coach-Perspektive` → `Einordnung` oder fachliche Überschrift.
- keine Selbstinszenierung als Senior-/Guru-/Master-Stimme.
- keine unnötige Meta-Sprache wie „dieses Dokument zeigt dir ...“, wenn der Inhalt direkt formuliert werden kann.
- kurze, klare Sätze; Fachbegriffe nur, wenn sie Präzision schaffen.

**Definition of Done:** Suchlauf nach `Coach`, `Guru-Level` und vergleichbaren Formulierungen liefert im aktiven Compendium keine unerwünschten Treffer.

---

## Arbeitspaket 4 — Java- und Engineering-Grundlagen migrieren

**Scope:** Legacy 001–014, 018, 020, 027–030, 033, 045, 046, 048, 052, 073, 099, 109, 119.

Aufgaben:

- Records, Sealed Types, Pattern Matching und Text Blocks aus `v2` in `05_engineering-guidelines` übernehmen.
- Virtual Threads nicht doppelt führen: `AK-004` bleibt Decision Guide; konkrete Java-Regeln werden dort integriert oder als klar abgegrenzte Guideline geführt.
- JUnit, Mockito, AssertJ, Testklassendesign und Testdaten auf aktuelle APIs prüfen.
- OOP/Patterns als Learning- bzw. Engineering-Inhalte klassifizieren.
- absolute Style-Regeln vermeiden.

**Definition of Done:** Jede alte Engineering-Datei ist migriert, zusammengeführt oder begründet verworfen.

---

## Arbeitspaket 5 — Application-, Domain- und Integrationsarchitektur

**Scope:** DDD, Hexagonal, CQRS, Modulith/Microservices, REST, OpenAPI, AsyncAPI, Kafka/EDA, Outbox, Saga, GraphQL, gRPC, Event Sourcing, WebSocket/SSE.

Aufgaben:

- Decision Guides von Standards klar trennen.
- `AK-056`, `AK-077`, `AK-079` ohne inhaltliche Doppelung verzahnen.
- API Governance über `AK-021`, `AK-066`, `AK-095`, `AK-110` konsolidieren.
- EDA-Themen über Ownership, Contracts, Delivery Semantics, Idempotenz und Betrieb verbinden.
- technologische Produktdetails aus zeitlosen Regeln heraushalten.

**Definition of Done:** Ein Leser kann von Integrationsproblem → Decision Guide → Standard → Evidence navigieren.

---

## Arbeitspaket 6 — Security, IAM und Privacy

**Scope:** Application Security, OAuth/OIDC, Resource Server, Input Validation, Browser Security, Secrets, Supply Chain, Privacy, Data Masking, Zero Trust.

Aufgaben:

- ASVS/OWASP/RFC/BSI bzw. offizielle Quellen als Primärbasis verwenden.
- Authentisierung, Autorisierung, CORS, CSRF und Tokenformat sauber trennen.
- Privacy-by-Design-Prinzip und technische Privacy Controls trennen.
- AI-/LLM-Datenflüsse in Privacy- und Security-Reviews einbeziehen.
- Standards nach Scope und Risikoklasse formulieren.

**Definition of Done:** Keine Security-Regel behauptet Sicherheit durch einen einzelnen Mechanismus.

---

## Arbeitspaket 7 — Data Architecture und Datenbankbetrieb

**Scope:** JPA, Flyway, Indexing, Partitioning, Sharding, Read Replicas, Caching, Elasticsearch, Multi-Tenancy, Backup/Recovery.

Aufgaben:

- starre Row-/RPS-/Lag-Schwellen entfernen.
- System of Record, Read Model, Cache, Replica und Suchindex klar unterscheiden.
- Migrationen als Transition Architecture behandeln.
- RPO/RTO nur aus Anforderungen übernehmen, nicht erfinden.
- Restore-Evidence und Daten-Lifecycle verbinden.

**Definition of Done:** Datenarchitektur wird über Ownership, Konsistenz, Lifecycle und messbare Workloads beschrieben.

---

## Arbeitspaket 8 — Platform, Delivery und Operations

**Scope:** CI/CD, Container, Kubernetes, IaC, GitOps, Helm/Kustomize, Observability, OpenTelemetry, SLO/Alerting, Profiling, Resilience, Chaos Engineering, Incident Management.

Aufgaben:

- Golden Path / Plattformstandard / Referenzarchitektur unterscheiden.
- GitOps-Prinzipien von Argo-CD-Produktfunktionen trennen.
- Observability Standard und OTel Reference Architecture verzahnen.
- SLO, Alerting, Incident Response, Profiling und Post-Mortem zu einem Feedback-Loop verbinden.
- Docker-Hostbetrieb als Operating Guide klar von Container-Image-Standard trennen.

**Definition of Done:** Delivery und Operations bilden eine nachvollziehbare Evidence- und Lernkette.

---

## Arbeitspaket 9 — Enterprise-/Governance-Themen

**Scope:** Technology Radar, Data Strategy, FinOps, Developer Experience, sozio-technische Architektur, Dokumentation, Living Documentation, Architecture Evaluation, Evolutionary Architecture, AI Architecture.

Aufgaben:

- Portfolio-/Strategiethemen aus Java-Nähe lösen.
- Decision Rights, Ownership, Kosten, Capability-/Data-/Application-Sicht sichtbar machen.
- AI-Thema um Use-Case-Zulässigkeit, Datenklassifikation, Evaluation, Human Oversight, Provider-/Modellrisiko, Kosten und Exit-Strategie ergänzen.
- keine Zertifizierungs- oder Erfahrungsbehauptungen aus Lerninhalten ableiten.

**Definition of Done:** Die Sammlung zeigt den Übergang von Engineering zu Enterprise Architecture, ohne Erfahrung zu behaupten, die nicht belegt ist.

---

## Arbeitspaket 10 — Legacy-Abbau und finale Qualitätsprüfung

Aufgaben:

- für jede Legacy-Datei prüfen: migriert, zusammengeführt oder verworfen?
- vollständig ersetzte Dateien löschen.
- `99_legacy-source` nach Abschluss entfernen oder bewusst als Archiv kennzeichnen.
- Cross-References und Links prüfen.
- IDs und Metadaten auf Konsistenz prüfen.
- aktuelle Technologie-Baselines verifizieren.
- aktive Dateien nach `Coach`, `Guru`, veralteten Reviewdaten und erfundenen Universalwerten durchsuchen.
- Root-README und Navigation finalisieren.

**Definition of Done:** Eine aktive Quelle pro Thema, keine Migrationszwischenstände, keine verwaisten Links, keine unbelegten Behauptungen.

---

## Commit-Strategie

Änderungen werden pro Arbeitspaket oder sinnvoller Teilwelle committed. Große Löschaktionen erfolgen erst nach erfolgreicher Migration der Inhalte. Dadurch bleibt die Historie nachvollziehbar und Fehler lassen sich gezielt zurücknehmen.
