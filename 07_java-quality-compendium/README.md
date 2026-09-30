# Architecture & Engineering Compendium

Dieses Verzeichnis sammelt Architekturwissen, technische Standards und Engineering-Guidelines. Der Schwerpunkt liegt auf Java-basierten Systemen, der Inhalt reicht aber bewusst darüber hinaus: Integration, Security, Daten, Plattform, Betrieb, Governance und Transformation gehören zusammen.

Das Compendium ist **kein Projektarchiv echter Architekturentscheidungen**. Allgemeines Wissen und wiederverwendbare Regeln tragen stabile `AK-*`-IDs. Ein echtes ADR wird nur für eine konkrete Entscheidung in einem konkreten Kontext geschrieben.

## Navigation

| Bereich | Zweck |
|---|---|
| [`00_governance/`](00_governance/) | Artefaktmodell, Entscheidungsprozess, Qualitäts- und Validierungsregeln, Migrationsstatus |
| [`01_principles/`](01_principles/) | langlebige Architektur- und Gestaltungsprinzipien |
| [`02_decision-guides/`](02_decision-guides/) | Entscheidungslogik, Optionen und Trade-offs vor einer konkreten Entscheidung |
| [`03_standards/`](03_standards/) | wiederverwendbare normative Architektur-, Security- und Qualitätsstandards |
| [`04_reference-architectures/`](04_reference-architectures/) | wiederverwendbare Lösungsbilder und technische Zielstrukturen |
| `05_engineering-guidelines/` | konkrete Java-, Test- und Implementierungsleitlinien; wird schrittweise aufgebaut |
| [`06_operating-models/`](06_operating-models/) | Rollen, Decision Rights, Reviews und Governance-Abläufe |
| `07_operating-guides/` | konkrete Betriebs- und Diagnoseanleitungen; wird aus dem bisherigen Bestand konsolidiert |
| `08_learning-guides/` | Grundlagen und Synthesen ohne normative Verbindlichkeit |
| `99_legacy-source/` | temporäres Quellarchiv für noch nicht vollständig migrierte Altinhalte |

## Dokumenttypen

Das führende Modell steht in [`00_governance/ARTIFACT-MODEL.md`](00_governance/ARTIFACT-MODEL.md). Kurz zusammengefasst:

- **Architecture Principle:** langlebige Leitidee.
- **Decision Guide:** hilft, eine konkrete Entscheidung vorzubereiten.
- **Architecture Standard / Policy:** verbindliche Regel in definiertem Geltungsbereich.
- **Reference Architecture:** wiederverwendbares Lösungsbild.
- **Engineering Guideline:** konkrete technische Umsetzungshilfe.
- **Operating Model:** Rollen und Arbeitsweise.
- **Operating Guide / Runbook:** konkrete Betriebsanleitung.
- **Learning Guide:** erklärt Zusammenhänge ohne Vorgabecharakter.
- **ADR:** dokumentiert genau eine konkrete architekturrelevante Entscheidung.

## Arbeitsprinzip

Die wichtigste Kette im Compendium ist:

```text
Auftrag / Problem
→ Stakeholder und Concerns
→ Qualitätsziele und Constraints
→ Optionen und Trade-offs
→ konkrete Entscheidung
→ Standard / Referenzarchitektur
→ Engineering und Delivery
→ Evidence im Betrieb
→ Review und Weiterentwicklung
```

Technologien werden nicht als Ziel behandelt. Erst das Problem und die Qualitätsanforderungen bestimmen, welche technische Option sinnvoll ist.

## Qualität und Quellen

Technische Aussagen werden bevorzugt gegen Primärquellen geprüft:

1. Norm, RFC, JEP oder offizielle Spezifikation,
2. offizielle Projekt- oder Herstellerdokumentation,
3. etablierte Security-/Operations-Quellen wie OWASP, BSI, NIST oder Google SRE,
4. Fachliteratur,
5. Communityquellen nur ergänzend.

Heuristiken bleiben als Heuristiken gekennzeichnet. Es gibt keine organisationsweiten Magic Numbers für Coverage, Mutation Score, P95, Threadzahlen, Datenmengen oder ähnliche Werte ohne konkreten Anforderungskontext.

## Sprache

Die Texte sind bewusst direkt gehalten. Fachbegriffe werden verwendet, wenn sie Präzision schaffen. Merksätze fassen einen Gedanken zusammen, ersetzen aber keine Begründung. Künstliche Rollen- oder Kompetenzinszenierung gehört nicht in das Compendium.

## Migration

Der Bestand wird aktuell konsolidiert. Der Fortschritt ist nachvollziehbar in:

- [`00_governance/REVIEW-NOTES.md`](00_governance/REVIEW-NOTES.md)
- [`00_governance/REWORK-PLAN.md`](00_governance/REWORK-PLAN.md)
- [`00_governance/REWORK-CHECKLIST.md`](00_governance/REWORK-CHECKLIST.md)
- [`00_governance/MIGRATION-CATALOG.md`](00_governance/MIGRATION-CATALOG.md)

Legacy-Dateien werden erst entfernt, wenn ihr Inhalt übernommen, fachlich verworfen oder bewusst durch ein besseres Artefakt ersetzt wurde.

## Methodische Basis

Das Modell orientiert sich unter anderem an:

- Michael Nygard: *Documenting Architecture Decisions*
- MADR
- arc42
- ISO/IEC/IEEE 42010:2022
- Architecture Governance und Compliance Reviews nach TOGAF/Open Group

Die konkreten Quellen stehen jeweils am fachlichen Dokument, damit Aussagen dort geprüft werden können, wo sie verwendet werden.
