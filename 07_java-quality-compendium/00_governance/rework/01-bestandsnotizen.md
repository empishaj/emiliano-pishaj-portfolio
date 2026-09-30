# Bestandsnotizen zur Überarbeitung des Architecture Quality & Governance Compendium

Stand: 2026-09-30
Branch: `rework/java-quality-compendium-2026`

## Ziel

Der Bereich `07_java-quality-compendium` wird vollständig geprüft, fachlich bereinigt, sprachlich vereinheitlicht und auf eine einzige nachvollziehbare Informationsarchitektur reduziert.

Diese Datei ist ein Arbeitsdokument. Sie sammelt Befunde während der Bestandsaufnahme. Entscheidungen über Löschen, Zusammenführen und Zielstruktur werden erst nach der inhaltlichen Prüfung getroffen.

## Erste strukturelle Befunde

### 1. Drei Generationen existieren parallel

Aktuell existieren gleichzeitig:

1. der historische flache Bestand `QG-JAVA-*`,
2. die thematisch sortierte `AK-*`-Struktur unter `00_governance` bis `07_learning-guides`,
3. ein weiterer Ansatz unter `v2/`.

Diese parallelen Strukturen erzeugen Dubletten, konkurrierende Bezeichnungen und unklare Navigation.

### 2. Das Artefaktmodell ist grundsätzlich tragfähig

Die Trennung zwischen folgenden Typen ist fachlich sinnvoll:

- Architecture Principle,
- Decision Guide,
- Architecture Standard,
- Policy,
- Reference Architecture,
- Engineering Guideline,
- Operating Model,
- Operating Guide / Runbook,
- Learning Guide,
- echte Architecture Decision Records.

Der zentrale Gedanke bleibt erhalten: Ein allgemeines Wissensdokument ist kein ADR. Ein ADR dokumentiert eine konkrete Entscheidung in einem konkreten Kontext.

### 3. Der Schreibstil ist teilweise zu künstlich

Im Bestand finden sich Formulierungen wie:

- `Coach-Perspektive`,
- `Coach-Merksatz`,
- `Coach-Prüfung`,
- ähnliche didaktische Selbstbezeichnungen.

Diese Sprache wird entfernt. Gute didaktische Struktur bleibt erhalten, aber in normaler Fachsprache, zum Beispiel:

- `Merksatz`,
- `Einordnung`,
- `Prüffragen`,
- `Praxis`,
- `Architekturperspektive`,
- `Entscheidungshilfe`.

### 4. Alte und neue Inhalte überschneiden sich

Mehrere Themen existieren sowohl als historisches `QG-JAVA-*` als auch als neues `AK-*`-Dokument und teilweise zusätzlich unter `v2/`.

Beispiele:

- Virtual Threads,
- DDD,
- CQRS,
- Security,
- Observability,
- Contract Testing,
- REST,
- Container,
- Architekturentscheidungsprozess,
- Java Records / Sealed Types / Pattern Matching / Text Blocks.

Das Ziel ist eine einzige kanonische Fassung pro Wissensgegenstand.

### 5. IDs und Typen sind noch nicht vollständig sauber getrennt

Die neue `AK-*`-ID als stabile Knowledge-ID ist grundsätzlich sinnvoll. Echte Projektentscheidungen sollten einen getrennten ADR-Namensraum besitzen.

Offen ist noch, ob alle bestehenden `AK-*`-Dateinamen dauerhaft die historische Nummer tragen sollen oder ob einzelne Themen zusammengeführt werden. Diese Entscheidung fällt nach der Vollprüfung der Inhalte.

### 6. Zeitabhängige Aussagen brauchen stärkere Validierung

Besonders prüfpflichtig sind:

- Framework- und Produktversionen,
- Security-Empfehlungen,
- OAuth/OIDC/JWT-Aussagen,
- Kubernetes- und Container-Empfehlungen,
- Cloud-/GitOps-Tooling,
- Performance- und Kapazitätswerte,
- Datenbankheuristiken,
- SLO-/SLA-Schwellenwerte,
- AI-/LLM-Integrationen,
- regulatorische Aussagen.

Primärquellen haben Vorrang vor Blogartikeln oder Lehrbuchheuristiken.

### 7. Normative Aussagen und Lehrinhalte werden getrennt

Ein Learning Guide darf erklären und einordnen. Ein Standard muss klaren Geltungsbereich, normative Regeln, Verification und Ausnahmeprozess besitzen. Ein Decision Guide darf keine scheinbar allgemeingültige Entscheidung vortäuschen.

### 8. Löschen erfolgt nur nach Ersetzung oder bewusster Archiventscheidung

Gelöscht werden dürfen insbesondere:

- exakte oder weitgehend redundante Dubletten,
- veraltete Zwischenstrukturen,
- leere oder nicht mehr referenzierte Arbeitsdateien,
- Inhalte, deren Thema vollständig in einer besseren kanonischen Fassung aufgeht.

Nicht gelöscht werden wichtige historische Inhalte, bevor relevante Aussagen in die kanonische Zielversion übernommen oder bewusst verworfen wurden.

## Bereits geprüfte Kernartefakte

### `README.md`

Stärken:

- erklärt den Übergang vom Java-Compendium zum Architecture Knowledge System,
- trennt Wissen, Standards und echte ADRs,
- enthält sinnvolle Governance-Grundsätze.

Verbesserung:

- nach Abschluss der Überarbeitung kürzer und stärker als Navigation statt als Migrationsbeschreibung formulieren.

### `00_governance/ARTIFACT-MODEL.md`

Stärken:

- klare Typentrennung,
- sinnvolle Knowledge-ID / Decision-ID-Trennung,
- brauchbares Status- und Metadatenmodell.

Verbesserung:

- sprachlich etwas straffen,
- die Typen `Policy` und `Architecture Standard` noch klarer gegeneinander abgrenzen,
- verbindliche Regeln für Kombination/Splitting von Wissensgegenständen ergänzen.

## Noch zu prüfen

- vollständige Inhalte aller Dateien unter `00_governance` bis `07_learning-guides`,
- alle historischen `QG-JAVA-*`-Dateien,
- der gesamte `v2/`-Bestand,
- Querverweise und tote Links,
- Begriffe und Schreibstil,
- Aktualität externer Quellen,
- fachliche Widersprüche zwischen Alt- und Neufassungen,
- fehlende Knowledge-Items und unbelegte Nummern.
