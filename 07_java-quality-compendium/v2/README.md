# Architecture Governance Compendium v2

> Arbeitsbereich für die kontrollierte Migration des bisherigen `07_java-quality-compendium` in ein konsistentes, validiertes Architektur- und Engineering-Governance-System.

## Warum v2?

Der bisherige Bestand enthält wertvolle Inhalte, vermischt aber unterschiedliche Artefakttypen: echte Architekturentscheidungen, Architekturprinzipien, Standards, Referenzarchitekturen, Engineering-Guidelines, Betriebsmodelle, Runbooks und Lernnotizen. Diese Artefakte erfüllen unterschiedliche Zwecke und brauchen deshalb unterschiedliche Strukturen, Lebenszyklen und Qualitätskriterien.

v2 trennt diese Typen bewusst. Der Altbestand bleibt zunächst unangetastet, bis die Migration abgeschlossen und fachlich geprüft ist.

## Leitidee

Das Compendium soll künftig eine durchgängige Nachweislinie ermöglichen:

```text
Business-/Qualitätstreiber
        ↓
Architekturprinzip
        ↓
konkrete Architekturentscheidung
        ↓
Standard / Referenzarchitektur
        ↓
Engineering-Guideline
        ↓
automatisierter Nachweis / Quality Gate
        ↓
Betriebsmetrik / Evidence
        ↓
Review / Governance / Evolution
```

## Artefakttypen

| Typ | Kürzel | Zweck |
|---|---|---|
| Architecture Principle | `AP` | Dauerhafte Leitplanke und Begründungsrahmen für Entscheidungen. |
| Architecture Decision Record | `ADR` | Eine konkrete, architekturrelevante Entscheidung in einem bestimmten Kontext. |
| Architecture Standard | `AS` | Verbindlicher organisations- oder plattformweiter Standard. |
| Reference Architecture | `RA` | Wiederverwendbares Ziel-/Lösungsmuster mit Bausteinen, Schnittstellen und Qualitätsannahmen. |
| Engineering Guideline | `EG` | Konkrete technische Umsetzungs- und Qualitätsregel nahe am Code. |
| Operating Model | `OM` | Rollen, Verantwortlichkeiten, Abläufe, Governance und Betriebsprozesse. |
| Runbook / Operational Practice | `RP` | Konkrete Diagnose-, Recovery- oder Betriebsanleitung. |
| Learning Note | `LN` | Didaktisches Lern- und Syntheseartefakt ohne normative Verbindlichkeit. |

## Grundregel

**Nicht jedes technische Thema ist ein ADR.**

Ein ADR wird nur geschrieben, wenn eine konkrete, später erklärungsbedürftige Entscheidung getroffen wird, die Struktur, Qualitätsmerkmale, wesentliche Abhängigkeiten, Schnittstellen, Betrieb, Sicherheit, Kosten oder Änderbarkeit relevant beeinflusst.

Engineering-Standards wie JavaDoc-Regeln, Testdaten-Builder oder MapStruct-Konventionen sind Guidelines bzw. Standards. Sie werden nicht künstlich als Architekturentscheidung formuliert.

## Validierungsprinzip

Jedes migrierte Dokument erhält einen expliziten Validierungsblock:

- **Quellenbasis:** bevorzugt Primärquellen und offizielle Spezifikationen.
- **Fachliche Prüfung:** zentrale technische Aussagen gegen Spezifikation/Dokumentation geprüft.
- **Versionsbezug:** Versionen und APIs werden datiert; vergängliche Zahlen werden nicht als ewige Wahrheit formuliert.
- **Kontextgrenzen:** Heuristiken werden als Heuristiken gekennzeichnet, nicht als Naturgesetze.
- **Risiko-/Security-Prüfung:** sicherheitsrelevante Aussagen gegen OWASP, RFC, NIST, BSI oder Hersteller-Primärquellen, je nach Thema.
- **Beispielstatus:** Codebeispiele sind entweder `illustrativ`, `referenzvalidiert` oder `kompilierungsvalidiert` gekennzeichnet.
- **Review-Trigger:** möglichst ereignisbasiert statt pauschal jährlich, z. B. bei Major-Version, Plattformwechsel, Sicherheitsstandardänderung oder signifikantem Betriebserlebnis.

## ADR-Lebenszyklus

Für echte ADRs gilt:

`Draft → Proposed → Accepted → Superseded / Deprecated`

Ein akzeptierter ADR wird nicht still umgeschrieben. Ändert sich die Entscheidung, entsteht ein neuer ADR, der den alten explizit ersetzt. Nachträgliche Evidenz kann als datierter Nachtrag ergänzt werden.

## Standard-/Guideline-Lebenszyklus

Standards und Guidelines sind dagegen bewusst versionierte, lebende Dokumente:

`Draft → Active → Review Due → Deprecated → Retired`

Sie dürfen fachlich weiterentwickelt werden, müssen aber Versionshistorie und wesentliche Änderungen sichtbar machen.

## Externe methodische Basis

- Michael Nygard: Architecture Decision Records — Kontext, Entscheidung, Status, Konsequenzen.
- MADR: Decision Drivers, Optionen, Decision Outcome, Confirmation.
- arc42: nur architekturrelevante Entscheidungen dokumentieren; Kriterien und Begründung sichtbar machen.
- ISO/IEC/IEEE 42010:2022: Architektur und Architektur-Beschreibung sowie unterschiedliche Sichten/Modelle bewusst unterscheiden.

## Migrationsstrategie

1. Altbestand inventarisieren.
2. Jedes Dokument einem Ziel-Artefakttyp zuordnen.
3. Kernaussagen und Quellen prüfen.
4. Überlappungen und Doppelungen bereinigen.
5. Dokument nach v2-Struktur neu schreiben.
6. technische Beispiele und Versionsangaben validieren.
7. Cross-References auf die neue Struktur umstellen.
8. erst nach vollständiger Abdeckung den Altbestand archivieren oder ersetzen.

## Aktueller Stand

- Masterkonzept: in Arbeit / v1 vorhanden.
- Migrationskatalog: wird schrittweise gepflegt.
- Erste validierte Migration: Java-Sprach- und Concurrency-Grundlagen `001–005`.
- Altbestand bleibt bis zur vollständigen Migration bestehen.
