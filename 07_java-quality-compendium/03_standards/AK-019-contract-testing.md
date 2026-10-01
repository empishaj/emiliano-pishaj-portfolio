---
id: AK-019
legacy_ids:
  - QG-JAVA-019
title: Contract Testing und Consumer-Provider-Kompatibilität
artifact_type: architecture-standard
domain: integration-testing
status: active
maturity: reviewed
normative_level: normative
owner_role: Integration Architecture / QA
last_validated: 2026-10-01
review_trigger:
  - Änderung der API-/Event-Vertragsstrategie
  - wiederkehrende Consumer-Brüche
---

# AK-019 — Contract Testing

## 1. Zweck

Contract Testing beantwortet eine zentrale Integrationsfrage:

> Kann ein Provider seinen Vertrag ändern, ohne bekannte Consumer unkontrolliert zu brechen?

Es ersetzt keine End-to-End-Tests und keine fachliche Abnahme. Es schafft eine schnellere, fokussierte Evidenz an Systemgrenzen.

## 2. Vertragsarten

### Schema Contract

Prüft Form und Typen eines Vertrags, beispielsweise OpenAPI, JSON Schema, Avro oder Protobuf.

### Behavioral Contract

Prüft zusätzlich erwartete Request-/Response-Interaktionen und Semantik.

### Consumer-Driven Contract

Consumer veröffentlichen konkrete Erwartungen, die der Provider gegen seine Implementierung verifiziert.

Nicht jedes Integrationsmodell braucht dasselbe Werkzeug.

## 3. Normative Regeln

Für organisationsübergreifend relevante APIs und Events SOLLTE gelten:

- Vertrag ist versioniert,
- Owner ist bekannt,
- Breaking Changes werden automatisiert erkannt, soweit technisch möglich,
- Provider- und Consumer-Kompatibilität wird vor Produktion geprüft,
- Testdaten und Beispiele sind deterministisch,
- Vertrag und Implementierung dürfen nicht dauerhaft auseinanderlaufen.

## 4. Synchron vs. asynchron

### HTTP/API

Prüfobjekte:

- Methoden/Pfade,
- Request-/Response-Schemas,
- Statuscodes,
- Header,
- Security-Anforderungen,
- Fehlerverträge.

### Events

Prüfobjekte:

- Message Schema,
- Event Type,
- Channel/Topic,
- Required Fields,
- Compatibility-Regeln,
- semantische Bedeutung,
- Header/Metadata.

Ein Event Contract ist mehr als ein Payload Schema. Ownership und Semantik müssen zusätzlich dokumentiert werden.

## 5. Consumer-Driven Contracts richtig einsetzen

CDC ist besonders nützlich, wenn:

- mehrere Consumer unterschiedliche Felder/Verhaltensweisen nutzen,
- Provider unabhängig deployt,
- Integration häufig geändert wird.

Risiken:

- Provider wird durch veraltete Consumerverträge blockiert,
- Tests werden zu Kopien der Implementierung,
- ein Consumer testet nur seine Sicht, nicht die gesamte fachliche API-Semantik.

Darum braucht jeder Contract Lifecycle und Ownership.

## 6. Contract Test ≠ API Design

Ein Test kann einen schlechten Vertrag stabil machen.

Vor Contract Testing muss die Schnittstelle selbst sinnvoll gestaltet sein:

- fachliche Ressource/Operation,
- Ownership,
- Fehlersemantik,
- Versionierungsstrategie,
- Security,
- Lifecycle.

## 7. CI-Gate

Typischer Ablauf:

```text
Contract Change
→ Lint / Schema Validation
→ Breaking Change Detection
→ Provider Verification
→ relevante Consumer Contracts
→ Merge / Release
```

Ein Gate SOLL nur das blockieren, was tatsächlich als verbindlicher Vertrag definiert ist.

## 8. Testumgebungen

Contract Tests sollten möglichst wenig von großen gemeinsamen Testumgebungen abhängen.

Ziel:

- schnell,
- deterministisch,
- nah am Vertrag,
- unabhängig von zufälligem Fremdsystemzustand.

## 9. Anti-Patterns

- OpenAPI-Datei vorhanden, aber Provider verhält sich anders.
- Consumer kopiert Provider-Code in seinen Test.
- Contract Broker ohne Owner-/Cleanup-Prozess.
- Breaking-Change-Gate blockiert auch bewusst versionierte neue API-Versionen.
- Contract Tests werden als Ersatz für fachliche Integrationstests verstanden.

## 10. Quellen

- OpenAPI Specification  
  https://spec.openapis.org/oas/
- AsyncAPI Specification  
  https://www.asyncapi.com/docs/reference/specification/v3.1.0
- Pact Documentation  
  https://docs.pact.io/
- Spring Cloud Contract  
  https://spring.io/projects/spring-cloud-contract

## 11. Merksatz

> Contract Testing schützt nicht „die Schnittstelle“ abstrakt. Es schützt **konkrete Erwartungen zwischen verantworteten Consumer- und Provider-Grenzen**.
