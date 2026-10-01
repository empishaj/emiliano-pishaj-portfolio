---
id: AK-064
legacy_ids:
  - ADR-064
title: Versionierung, Release Notes und Änderungsnachvollziehbarkeit
artifact_type: architecture-standard
domain: lifecycle-governance
status: active
maturity: reviewed
normative_level: normative
owner_role: Engineering Governance
last_validated: 2026-10-01
review_trigger:
  - Änderung der Release-/Artifact-Strategie
---

# AK-064 — Versionierung und Changelog

## 1. Zweck

Ein Release muss beantwortbar machen:

- Was wurde geändert?
- Ist die Änderung kompatibel?
- Welcher Source-Stand erzeugte das Artefakt?
- Welche Version läuft tatsächlich?
- Welche Migration oder besondere Betriebsaktion ist nötig?

Versionierung ist damit Teil von Lifecycle Governance und Incident-Nachvollziehbarkeit.

## 2. SemVer bewusst verwenden

Semantic Versioning 2.0.0 definiert:

```text
MAJOR.MINOR.PATCH
```

für Software mit einer klar definierten öffentlichen API:

- MAJOR für inkompatible API-Änderungen,
- MINOR für rückwärtskompatible Funktionalität,
- PATCH für rückwärtskompatible Fehlerkorrekturen.

SemVer ist nützlich für Libraries, APIs und andere veröffentlichte Verträge.

Es ist **kein Naturgesetz für jedes deployte Artefakt**. Ein kontinuierlich deployter interner Service kann zusätzlich oder alternativ Git-SHA, Build-ID und Release-Zeitpunkt benötigen.

## 3. Public API definieren

SemVer ist nur sinnvoll, wenn klar ist, was als öffentlicher Vertrag gilt.

Mögliche Bestandteile:

- Java Library API,
- REST API,
- Event Schema,
- CLI-Vertrag,
- Konfiguration,
- Datenbank-/Exportformat.

Ohne diese Grenze bleibt „Breaking Change“ subjektiv.

## 4. Release-Identität

Ein produktives Artefakt SOLLTE eindeutig auf folgende Informationen zurückgeführt werden können:

```text
Version / Build ID
→ Git Commit
→ CI Pipeline
→ Artifact Digest
→ SBOM
→ Deployment
```

Damit kann bei Incident oder Audit festgestellt werden, was tatsächlich lief.

## 5. Changelog vs. Commit History

Git History ist nicht automatisch ein nutzerverständlicher Changelog.

Ein Changelog beziehungsweise Release Notes SOLLEN relevante Änderungen für Consumer oder Betrieb sichtbar machen:

- Breaking Changes,
- neue Funktionen,
- relevante Fixes,
- Security Fixes,
- Migrationen,
- Deprecations,
- Betriebsänderungen.

Interne Refactorings ohne externe Wirkung müssen nicht zwangsläufig in Consumer Release Notes erscheinen.

## 6. Maschinenlesbare und menschliche Sicht

Beide können nebeneinander existieren:

- Git Tag / Artifact Metadata für Maschinen,
- Release Notes für Menschen,
- OpenAPI-/Schema-Diff für Vertrag,
- SBOM für Komponenten.

## 7. Laufende Version sichtbar machen

Ein Service SOLLTE seine Build-/Versionsidentität für Betrieb und Diagnose zugänglich machen, beispielsweise über sichere Runtime-Metadaten oder Deployment Labels.

Dabei keine unnötigen internen Security-Details öffentlich exponieren.

## 8. Deprecation

Eine neue Major-Version löst Lifecycle Management nicht automatisch.

Consumer brauchen:

- Ankündigung,
- Migrationspfad,
- Deprecation-/Sunset-Information,
- Nutzungsinventar.

Siehe AK-110.

## 9. Anti-Patterns

- Version `1.0.0` ohne definierte Public API.
- jeder Commit erhöht Patch-Version manuell.
- Breaking Change wird als Minor veröffentlicht, weil „nur wenige Consumer betroffen sind“.
- Produktivversion kann nicht zum Source Commit zurückverfolgt werden.
- Changelog enthält nur Ticketnummern ohne Bedeutung für Consumer.

## 10. Quellen

- Semantic Versioning 2.0.0  
  https://semver.org/spec/v2.0.0.html
- AK-110 — API Lifecycle
- AK-057 — Software Supply Chain

## 11. Merksatz

> Versionierung ist nicht die Zahl auf dem Artefakt. Sie ist der Vertrag darüber, **wie Änderungen erkannt, bewertet und zu einem konkreten ausgelieferten Stand zurückverfolgt werden**.
