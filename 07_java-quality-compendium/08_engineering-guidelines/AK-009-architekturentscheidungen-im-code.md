---
id: AK-009
legacy_ids:
  - QG-JAVA-009
title: Architekturentscheidungen im Code sichtbar und prüfbar machen
artifact_type: engineering-guideline
domain: architecture-governance
status: active
maturity: reviewed
normative_level: recommended
last_validated: 2026-09-30
review_trigger:
  - Einführung neuer Architekturregeln
  - wiederkehrende Architekturverletzungen in Reviews
---

# AK-009 — Architekturentscheidungen im Code sichtbar und prüfbar machen

## Kurzfassung

Eine Architekturentscheidung ist wirksamer, wenn ihre Konsequenzen im Code, in Modulgrenzen, Verträgen und automatisierten Prüfungen erkennbar sind. Kommentare allein reichen nicht; gleichzeitig sollte nicht jede Entscheidung künstlich mit Annotationen markiert werden.

## 1. Von der Entscheidung zur technischen Konsequenz

Beispiel:

```text
Entscheidung:
Domain darf nicht von Spring abhängen.

Konsequenz:
Package-/Modulgrenze
→ keine Spring-Imports im Domain-Modul
→ ArchUnit-Regel
→ CI prüft die Regel
```

Die Kette lautet:

```text
Rationale
→ Architekturregel
→ Implementierungsstruktur
→ automatisierbare Prüfung
→ Review-Evidence
```

## 2. Gute Sichtbarkeitsmechanismen

### Namen und Modulstruktur

Sprechende Module, Packages, Ports und Adapter machen Architektur unmittelbar sichtbar.

### `package-info.java` und kurze Architekturhinweise

Sie sind sinnvoll, wenn ein Package eine nicht offensichtliche Verantwortung oder Abhängigkeitsregel besitzt.

### ADR-/Knowledge-Referenzen

Eine Referenz ist sinnvoll, wenn die Verbindung zur Entscheidung für Wartung relevant ist:

```java
/**
 * Publishes domain events through the transactional outbox.
 * See ADR-014 in the system decision log.
 */
final class OutboxEventPublisher { ... }
```

Die Referenz ersetzt nicht die Erklärung des Codes.

### Architecture Tests

```java
@ArchTest
static final ArchRule domainMustNotDependOnSpring =
    noClasses()
        .that().resideInAPackage("..domain..")
        .should().dependOnClassesThat()
        .resideInAnyPackage("org.springframework..");
```

Automatisierte Regeln sind besonders wertvoll für Grenzen, die häufig versehentlich verletzt werden können.

## 3. Nicht jede Entscheidung ist automatisierbar

Beispiele für schwer vollständig automatisierbare Aussagen:

- „dieses System bleibt fachlich führend für Personendaten“,
- „neue Consumer erhalten zwölf Monate Migrationszeit“,
- „Cloud-Nutzung erfordert Schutzbedarfsprüfung“.

Solche Entscheidungen brauchen andere Evidence: Architekturreview, Vertrag, Datenlandkarte, Abnahmekriterium oder Governance-Prozess.

## 4. Kommentare erklären Warum, nicht Syntax

Schlecht:

```java
// increment counter
counter++;
```

Sinnvoll:

```java
// Keep sequence monotonic because consumers use it to detect missing events.
sequence++;
```

Wenn das Warum stabiler Bestandteil eines Architekturvertrags ist, sollte es zusätzlich in passender Dokumentation oder einem Test sichtbar sein.

## 5. Keine eigene Annotation als Pflichtübung

Eine Annotation wie `@DecisionRef("ADR-014")` kann in speziellen Toolchains nützlich sein. Sie ist aber kein allgemeiner Standard. Ohne Tooling, Query-Bedarf oder automatisierte Auswertung erzeugt sie nur zusätzliche Metadatenpflege.

## 6. Normative Regeln

### MUSS

- technisch erzwingbare Architekturregeln sollen dort automatisiert werden, wo der Nutzen die Wartungskosten rechtfertigt.
- stabile Systemgrenzen und Verträge müssen im Code nachvollziehbar benannt sein.
- Referenzen auf Entscheidungen müssen auf tatsächlich existente, stabile IDs zeigen.

### SOLLTE

- ArchUnit oder vergleichbare Checks werden für wichtige statische Abhängigkeitsregeln genutzt.
- Kommentare erklären nicht offensichtliche Gründe und Constraints.
- CI macht Architekturverletzungen dort sichtbar, wo sie objektiv prüfbar sind.

### DARF NICHT

- erfundene ADR-IDs werden nicht in Code eingebaut.
- Kommentare und Annotationen ersetzen keine klare Struktur.
- jede Architekturidee wird nicht zwanghaft zu einer automatischen Fitness Function gemacht.

## 7. Prüffragen

1. Welche technische Konsequenz hat die Entscheidung?
2. Kann die Regel objektiv geprüft werden?
3. Ist sie wichtig genug für einen Build-Gate?
4. Ist die Entscheidung im Code bereits durch Namen und Struktur sichtbar?
5. Wird eine Referenz später zuverlässig auflösbar sein?
6. Welche Evidence braucht eine nicht automatisierbare Entscheidung?

## 8. Verwandte Knowledge-Items

- `AK-061` — Architecture Fitness Functions
- `AK-075` — Architecture Decision Process
- `AK-093` — Living Documentation

## 9. Quellen

- ArchUnit User Guide: https://www.archunit.org/userguide/html/000_Index.html
- arc42 — Architecture Decisions: https://docs.arc42.org/section-9/
- Neal Ford et al., *Building Evolutionary Architectures*, 2nd ed., 2022.

## 10. Merksatz

> Architektur wird stabiler, wenn wichtige Entscheidungen nicht nur dokumentiert, sondern in Struktur, Verträgen und passenden Prüfungen wiedererkennbar sind.
