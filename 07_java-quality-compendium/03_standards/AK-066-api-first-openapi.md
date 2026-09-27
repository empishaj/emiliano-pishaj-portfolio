---
id: AK-066
legacy_ids:
  - ADR-066
title: API First und OpenAPI Governance
artifact_type: architecture-standard
domain: integration-governance
status: active
maturity: reviewed
normative_level: normative
owner_role: Integration Architecture
last_validated: 2026-09-28
technology_baseline:
  openapi: "3.2.x current specification line at validation date"
review_trigger:
  - neue OpenAPI-Minor-/Major-Linie
  - Änderung der organisationsweiten API-Governance
---

# AK-066 — API First und OpenAPI Governance

## 1. Zweck

API First bedeutet nicht, dass YAML wichtiger als Software ist.

Es bedeutet:

> **Der Vertrag wird vor oder gemeinsam mit der Implementierung bewusst gestaltet und reviewbar gemacht.**

Dadurch können Fachlichkeit, Security, Consumer-Bedarf und Kompatibilität diskutiert werden, bevor eine interne Implementierung den Vertrag faktisch festschreibt.

## 2. Contract Source of Truth

Für HTTP-APIs SOLL ein OpenAPI-Dokument als maschinenlesbarer Vertrag verwendet werden.

Die Organisation MUSS klären, ob:

- Contract-first der primäre Workflow ist,
- Code-first zulässig ist, solange CI die Gleichheit sicherstellt,
- oder beide Modelle je Produktklasse erlaubt sind.

Das zentrale Ziel ist **kein Drift zwischen Vertrag und produktivem Verhalten**.

## 3. OpenAPI-Baseline

Zum Validierungszeitpunkt ist OpenAPI 3.2.1 die aktuell veröffentlichte Spezifikation. Ein organisationsweiter Standard sollte jedoch nicht automatisch jede neueste Version erzwingen.

Baselines werden bewusst freigegeben anhand von:

- Tooling-Support,
- Generatoren,
- Gateway/Portal,
- Linter,
- Consumer-Ökosystem.

Darum steht die konkrete erlaubte Version in einer Technology Baseline und kann unabhängig von den Architekturregeln aktualisiert werden.

## 4. Mindestinhalt eines API-Vertrags

SOLLTE mindestens beschreiben:

- API-Identität und Version,
- Operationen,
- Request-/Response-Schemas,
- relevante Statuscodes,
- Problem-/Fehlerformat,
- Security Schemes,
- Pflichtheader,
- Pagination/Filter soweit vorhanden,
- Beispiele für kritische Operationen,
- Deprecation/Lifecycle-Hinweise.

## 5. Design Review vor Implementierung

Ein relevanter API-Vertrag SOLLTE vor breiter Implementierung folgende Perspektiven prüfen:

### Fachlichkeit

- Sind Ressourcen und Begriffe korrekt?
- Ist Ownership klar?

### Consumer

- Ist der Vertrag für den Use Case geeignet?
- Werden unnötige Round Trips erzeugt?

### Security

- Welche Daten werden exponiert?
- Welche Scopes/Policies gelten?

### Betrieb

- Sind Fehler und Korrelation behandelbar?
- Ist die API beobachtbar?

### Lifecycle

- Wie kann der Vertrag additiv evolvieren?
- Was wäre ein Breaking Change?

## 6. Linting

Ein Linter kann organisationsspezifische Regeln automatisieren, zum Beispiel:

- `operationId` vorhanden,
- Security definiert,
- Fehlerformat einheitlich,
- Namenskonventionen,
- Beschreibungen an kritischen Elementen,
- keine verbotenen Datentypen oder Patterns.

Nicht jede Stilregel gehört in ein Blocking Gate. Gates sollten echte Interoperabilitäts-, Security- oder Governance-Relevanz besitzen.

## 7. Breaking Change Detection

CI SOLLTE Schema-/Contract-Diffs verwenden, um potenziell inkompatible Änderungen sichtbar zu machen.

Typische Breaking Changes können sein:

- Pflichtfeld hinzufügen,
- Feld entfernen,
- Typ verengen,
- Operation/Pfad entfernen,
- Security-Anforderung inkompatibel ändern.

Aber Kompatibilität ist nicht ausschließlich syntaktisch. Eine semantische Bedeutungsänderung kann ebenfalls breaking sein, obwohl das Schema identisch bleibt.

## 8. Code Generation

Codegen KANN sinnvoll sein für:

- Client Stubs,
- Server Interfaces,
- Datentypen,
- Dokumentation.

Es ist kein Muss.

Risiken:

- generierter Code dominiert Domain Design,
- Generatorwechsel erzeugt große Diffs,
- Frameworkdetails gelangen in fachliche Schichten.

Empfehlung:

> Generierung an der Integrationsgrenze halten.

## 9. API Catalog

In größeren Organisationen reicht eine Datei pro Repository nicht.

Ein API-Katalog SOLLTE auffindbar machen:

- API Owner,
- Lifecycle-Status,
- Consumer,
- Vertrag,
- Security-/Datenschutzklasse,
- produktive Endpunkte,
- Deprecation.

Das ist besonders bei behördenübergreifenden Integrationen relevant.

## 10. Governance-Flow

```text
API Need
→ Contract Draft
→ Fach-/Security-/Consumer Review
→ Lint / Validation
→ Implementierung
→ Contract Test
→ Veröffentlichung im Catalog
→ Runtime Monitoring
→ Lifecycle Management
```

## 11. Anti-Patterns

- OpenAPI nach Implementierung nur für Swagger UI generieren und nie reviewen.
- Schemas als direkte Kopie interner JPA-Entities.
- Linter mit hunderten kosmetischen Regeln als Governance-Ersatz.
- Codegen bestimmt Domain-Modell.
- Breaking-Change-Check ohne Consumer-/Lifecycle-Prozess.

## 12. Quellen

- OpenAPI Specification  
  https://spec.openapis.org/oas/
- OpenAPI 3.2.1, veröffentlicht 2026-09-10  
  https://spec.openapis.org/oas/v3.2.1.html
- RFC 9457 — Problem Details for HTTP APIs
- AK-019 — Contract Testing
- AK-021 — REST API Standard
- AK-110 — API Lifecycle

## 13. Coach-Merksatz

> API First bedeutet nicht „YAML first“. Es bedeutet, den **organisationsübergreifenden Vertrag bewusst zu entscheiden, zu reviewen und automatisiert gegen Drift und inkompatible Änderungen zu schützen**.
