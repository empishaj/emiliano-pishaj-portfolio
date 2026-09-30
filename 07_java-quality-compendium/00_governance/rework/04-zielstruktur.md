# Zielstruktur des Compendiums

Stand: 2026-09-30

## Ziel

Der aktive Bestand wird auf eine einzige Informationsarchitektur reduziert:

```text
07_java-quality-compendium/
├─ README.md
├─ 00_governance/
├─ 01_principles/
├─ 02_decision-guides/
├─ 03_standards/
├─ 04_reference-architectures/
├─ 05_operating-guides/
├─ 06_operating-models/
├─ 07_learning-guides/
└─ 08_engineering-guidelines/
```

## 00_governance

Enthält Regeln für das Wissenssystem selbst:

- Artefaktmodell,
- Validierungsregeln,
- ADR-Lifecycle und Template,
- Metadaten-/Naming-Regeln,
- Governance des Compendiums.

Migrations- und Arbeitsunterlagen bleiben während der Überarbeitung erhalten und werden am Ende neu bewertet.

## 01_principles

Stabile Denk- und Gestaltungsprinzipien.

Beispiele:

- Kopplung/Kohäsion,
- Einfachheit,
- Immutability/Invarianten,
- sozio-technische Architektur,
- Evolutionary Architecture.

Prinzipien erklären eine dauerhafte Leitidee. Sie schreiben keine konkrete Technologie vor.

## 02_decision-guides

Hilfen für wiederkehrende Architekturentscheidungen.

Beispiele:

- Modulith vs. Microservices,
- CQRS,
- Virtual Threads,
- OAuth2/OIDC-Tokenarchitektur,
- DDD,
- Hexagonal Architecture.

Ein Decision Guide trifft keine konkrete Projektentscheidung. Er zeigt Treiber, Optionen, Grenzen und Prüffragen.

## 03_standards

Wiederverwendbare normative Vorgaben.

Ein Standard enthält mindestens:

- Zweck,
- Scope,
- normative Regeln,
- Begründung,
- Verifikation/Evidence,
- Ausnahmeprozess beziehungsweise Verweis auf den zentralen Ausnahmeprozess,
- Review-Trigger,
- Quellen.

Policies werden hier nur dann geführt, wenn sie dieselbe Navigationslogik bedienen. `artifact_type` unterscheidet Policy und Standard.

## 04_reference-architectures

Wiederverwendbare Lösungsbilder mit klaren Variationspunkten.

Beispiele:

- modularer Monolith,
- CI/CD,
- DevSecOps,
- OpenTelemetry,
- GitOps.

Eine Reference Architecture ist kein zwingender Standard. Verbindlichkeit entsteht durch einen Standard, eine Policy oder eine konkrete Entscheidung.

## 05_operating-guides

Konkrete technische Betriebs- und Diagnoseanleitungen.

Beispiele:

- Docker Engine auf Debian,
- später Profiling, Recovery, DLQ-Reprocessing, sofern diese Inhalte aus dem Altbestand übernommen werden.

## 06_operating-models

Rollen, Abläufe, Decision Rights und Governance-Prozesse.

Beispiele:

- Architecture Decision Process,
- Architecture Evaluation,
- Incident Management,
- Technology Radar,
- API Lifecycle, sofern dessen Schwerpunkt organisatorisch ist.

## 07_learning-guides

Erklärende Lern- und Synthesetexte ohne normative Wirkung.

Beispiele:

- Was Architektur ist,
- Architekturmuster,
- Gesamtsynthese.

## 08_engineering-guidelines

Technologie- und implementierungsnahe Regeln mit konkreter Entwickler-/Reviewer-Perspektive.

Beispiele:

- Java Records,
- Sealed Types,
- Pattern Matching,
- Text Blocks,
- JUnit/Mockito/AssertJ,
- Testcontainers,
- JavaDoc,
- MapStruct,
- Testdaten,
- Concurrency.

Engineering Guidelines können normative Formulierungen enthalten, sind aber bewusst enger im Scope als organisationsweite Architecture Standards.

## Keine künstlichen Portfolio-ADRs

Ein echter ADR benötigt einen konkreten Kontext, eine tatsächliche Entscheidungsfrage, reale Constraints und ein nachvollziehbares Entscheidungsmandat.

Das Compendium liefert dafür Methode, Template und Beispiele. Es erzeugt keine fiktiv als `Accepted` dargestellten Entscheidungen, nur um eine ADR-Sammlung vorzuweisen.

## Naming

Stabile Knowledge-ID:

```text
AK-001
AK-002
...
```

Dateiname:

```text
AK-<legacy-id>-<sprechender-slug>.md
```

Die historische Nummer bleibt zunächst als stabile Referenz erhalten. Sie ist keine Rangfolge und keine Aussage über den Artefakttyp.

## Schreibstil

Verwendet werden normale Fachüberschriften wie:

- Einordnung,
- Beispiel,
- Prüffragen,
- Merksatz,
- Architekturperspektive,
- Grenzen,
- Entscheidungshilfe.

Nicht verwendet werden Selbstbezeichnungen wie `Coach-Perspektive`, `Coach-Merksatz`, `Coach-Prüfung` oder `Coach-Ziel`.

## Umgang mit Altbestand

1. Inhalt prüfen.
2. Einzigartige fachliche Substanz markieren.
3. In kanonisches AK-Dokument übernehmen, wenn weiterhin relevant.
4. Querverweise umstellen.
5. historische Datei löschen, wenn keine eigenständige Funktion verbleibt.
6. Git-Historie dient als Historie; redundante Dateien müssen nicht dauerhaft im aktiven Baum liegen.
