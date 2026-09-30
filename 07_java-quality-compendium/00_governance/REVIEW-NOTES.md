# Bestandsnotizen zur Überarbeitung

Stand: 2026-09-30

## 1. Ausgangslage

Im Verzeichnis `07_java-quality-compendium` liegen derzeit drei Entwicklungsstände parallel:

1. ältere `QG-JAVA-*`-Dateien im Wurzelverzeichnis,
2. die neuere thematische `AK-*`-Struktur,
3. ein zusätzlicher Migrationsbereich `v2/`.

Alle drei enthalten wertvolle Inhalte, erzeugen zusammen aber unnötige Doppelungen, widersprüchliche Navigationswege und unterschiedliche Dokumentmodelle.

## 2. Was bereits gut funktioniert

Die neuere `AK-*`-Struktur ist die fachlich stärkste Basis. Besonders gelungen sind:

- die Trennung von Prinzip, Decision Guide, Standard, Referenzarchitektur, Operating Model, Operating Guide und Learning Guide,
- die stabile Knowledge-ID `AK-xxx`, unabhängig vom Artefakttyp,
- die Trennung zwischen allgemeinem Architekturwissen und echten projektspezifischen ADRs,
- ereignisbasierte Review-Trigger statt rein kosmetischer Jahresdaten,
- Trade-offs statt pauschaler Best Practices,
- Evidence und überprüfbare Architekturregeln,
- explizite Ownership- und Governance-Fragen.

Diese Struktur wird zur kanonischen Basis.

## 3. Hauptprobleme

### 3.1 Drei konkurrierende Strukturen

`QG-JAVA-*`, `AK-*` und `v2/` dürfen nicht dauerhaft parallel bestehen. Das erschwert Orientierung und Cross-References.

### 3.2 Artefakttypen waren historisch vermischt

Coding Guideline, Architekturentscheidung, Strategie, Referenzarchitektur und Betriebsmodell wurden teilweise mit derselben ADR-/QG-Logik behandelt. Das ist fachlich nicht sauber.

### 3.3 Stil ist teilweise künstlich

Begriffe wie:

- `Coach-Merksatz`,
- `Coach-Frage`,
- `Coach-Ziel`,
- `Coach-Perspektive`,
- `Coach-Master-Ton`

wirken im Repository unnötig inszeniert. Sie werden durch normale Formulierungen ersetzt:

- `Merksatz`,
- `Prüffrage`,
- `Ziel`,
- `Einordnung`,
- oder ganz entfernt, wenn die Überschrift keinen Mehrwert hat.

Der Text soll sachlich, verständlich und direkt wirken.

### 3.4 Zu viel Meta-Sprache

Einige Texte erklären häufig, welche Rolle das Dokument im Wissenssystem spielt. Diese Information gehört primär in Metadaten, README und Navigation. Fachtexte sollen schneller zum eigentlichen Problem kommen.

### 3.5 Legacy-Dateien sind teilweise noch die einzige Quelle

Viele alte `QG-JAVA-*`-Dateien wurden noch nicht in die `AK-*`-Struktur überführt. Sie dürfen deshalb nicht pauschal gelöscht werden. Solange ein Thema nicht vollständig migriert und geprüft ist, bleibt die Legacy-Datei als Quellmaterial erhalten.

### 3.6 `QG-JAVA-006` fehlt

Mehrere alte Dokumente verweisen auf `QG-JAVA-006` als Spring-Boot-Serviceschicht. Die Quelldatei ist nicht im Repository vorhanden. Der Inhalt wird nicht aus Querverweisen erfunden. `AK-006` bleibt als Source Gap markiert, bis eine belastbare Quelle vorliegt.

### 3.7 `121–124` sind unbelegt

Für diese Nummern liegt keine Quelle vor. Sie bleiben reserviert; es werden keine Themen konstruiert, nur um Nummernlücken zu schließen.

## 4. Fachliche Korrekturen, die bereits erkannt wurden

Die Überarbeitung übernimmt nicht blind Aussagen aus dem Altbestand. Unter anderem werden folgende Punkte korrigiert oder präzisiert:

- Records sind nur shallowly immutable; mutable Referenzen bleiben mutable.
- Virtual Threads lösen keine Race Conditions und keine Downstream-Kapazitätsprobleme.
- CORS ist kein allgemeiner CSRF-Schutz.
- OAuth2, OIDC und JWT sind unterschiedliche Konzepte; OAuth2 schreibt JWT als Access-Token-Format nicht allgemein vor.
- Read Replicas sind eine Replikations-/Skalierungstechnik und nicht automatisch CQRS.
- Partitionierung und Sharding werden nicht anhand universeller Row-Schwellen entschieden.
- Mutation Score, Coverage, SLO, Tech-Debt-Quoten und ähnliche Zahlen werden nicht ohne Kontext als globaler Standard verwendet.
- GraphQL Federation wird nicht an eine starre Teamzahl gekoppelt.
- Post-Mortems reduzieren Wiederholungsrisiken, garantieren aber nicht, dass ein Fehler "nie wieder" auftritt.
- Privacy, Security und AI Governance werden nicht auf einzelne technische Mechanismen reduziert.

## 5. Aktuell extern validierte Baselines

Zeitabhängige Aussagen wurden gegen offizielle Primärquellen geprüft:

- OpenAPI Specification: 3.2.1, veröffentlicht am 2026-09-10.
- AsyncAPI Specification: 3.1.0.
- OWASP ASVS: stabile Version 5.0.0.
- Spring AI: stabile Linie 2.0.1; 2.1.0-M1 ist Preview und wird nicht als stabile Baseline verwendet.
- Virtual Threads: final seit Java 21; Aussagen zu Pinning werden JDK-spezifisch behandelt und nicht aus alten Loom-Heuristiken verallgemeinert.

Technologieversionen gehören in eine datierte Baseline, nicht in eine vermeintlich zeitlose Architekturregel.

## 6. Zielstruktur

Die kanonische Struktur soll sein:

```text
07_java-quality-compendium/
├── README.md
├── 00_governance/
├── 01_principles/
├── 02_decision-guides/
├── 03_standards/
├── 04_reference-architectures/
├── 05_engineering-guidelines/
├── 06_operating-models/
├── 07_operating-guides/
├── 08_learning-guides/
└── 99_legacy-source/        # nur solange Migration noch nicht abgeschlossen ist
```

`v2/` wird nach Übernahme seiner noch relevanten Inhalte entfernt.

## 7. Inhaltliche Leitlinie

Jedes Dokument soll klar erkennbar beantworten, was es ist:

- **Principle:** Welche langlebige Leitidee hilft bei Entscheidungen?
- **Decision Guide:** Welche Optionen und Trade-offs muss ich vor einer konkreten Entscheidung verstehen?
- **Standard/Policy:** Welche Regel gilt in einem definierten Scope?
- **Reference Architecture:** Wie sieht ein wiederverwendbares Lösungsbild aus?
- **Engineering Guideline:** Wie wird eine technische Praxis sinnvoll umgesetzt und geprüft?
- **Operating Model:** Wer entscheidet, betreibt, prüft und eskaliert wie?
- **Operating Guide/Runbook:** Was ist bei einem konkreten Betriebszustand zu tun?
- **Learning Guide:** Welches Konzept muss verstanden werden, ohne daraus eine verbindliche Regel zu machen?
- **ADR:** Welche konkrete Entscheidung wurde in einem konkreten Kontext tatsächlich getroffen?

## 8. Löschregeln

Eine Datei wird gelöscht, wenn mindestens einer dieser Fälle erfüllt ist:

1. Der Inhalt ist vollständig und besser in einem kanonischen Dokument enthalten.
2. Die Datei ist nur ein veralteter Migrationszwischenstand.
3. Sie erzeugt eine fachlich schädliche Doppelquelle.
4. Sie enthält keinen eigenständigen Informationswert mehr.

Eine Datei wird **nicht** gelöscht, wenn sie noch die einzige Quelle für ein nicht migriertes Thema ist.

## 9. Qualitätskriterien für die Überarbeitung

Ein Thema gilt erst als fertig, wenn:

- Artefakttyp und Scope stimmen,
- zentrale Aussagen fachlich validiert wurden,
- Primärquellen bevorzugt werden,
- veraltete oder absolute Heuristiken entfernt sind,
- positive und negative Trade-offs sichtbar sind,
- normative Sprache nur bei tatsächlichen Standards verwendet wird,
- Cross-References auf existierende Ziele zeigen,
- Beispiele klar als Beispiele erkennbar sind,
- Review-Trigger sinnvoll sind,
- keine künstliche Rollen- oder Gremienrealität erfunden wird,
- die Sprache natürlich und professionell ist.
