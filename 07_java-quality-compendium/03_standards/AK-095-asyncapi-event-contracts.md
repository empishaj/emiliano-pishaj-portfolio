---
id: AK-095
legacy_ids:
  - ADR-095
title: AsyncAPI und Event Contract Governance
artifact_type: architecture-standard
domain: event-integration
status: active
maturity: reviewed
normative_level: normative
owner_role: Integration Architecture
last_validated: 2026-10-01
technology_baseline:
  asyncapi: "3.1.x"
review_trigger:
  - neue AsyncAPI-Major-/Minor-Linie
  - Änderung der Event-/Messaging-Governance
---

# AK-095 — Event Contracts mit AsyncAPI

## 1. Zweck

Events sind Integrationsverträge.

Ein Payload-Schema allein beantwortet nicht:

- auf welchem Channel eine Nachricht fließt,
- wer sie sendet,
- wer sie konsumiert,
- welche Semantik sie besitzt,
- welche Security-/Betriebsannahmen gelten,
- wie Fehler und Lifecycle behandelt werden.

AsyncAPI beschreibt message-driven APIs protokollunabhängig und ist deshalb die bevorzugte Vertragsbeschreibung für relevante asynchrone Schnittstellen im Compendium.

Zum Validierungszeitpunkt ist AsyncAPI 3.1.0 aktuell.

## 2. Trennung von Contract und Payload Schema

```text
AsyncAPI
→ Kommunikationsvertrag

JSON Schema / Avro / Protobuf
→ Payload-Struktur
```

AsyncAPI kann ein Schema einbetten oder referenzieren. Entscheidend ist die konzeptionelle Trennung.

## 3. Wann ein Event einen formalen Vertrag braucht

MUSS/SOLLTE insbesondere bei:

- Bounded-Context-übergreifenden Events,
- organisationsübergreifenden Nachrichten,
- mehreren Consumer-Teams,
- langfristig gespeicherten/replaybaren Events,
- regulatorisch oder betrieblich kritischen Nachrichten.

Ein rein internes, kurzlebiges Implementation Event innerhalb eines Moduls braucht nicht zwangsläufig dieselbe Governance-Tiefe.

## 4. Mindestinhalte

Ein relevanter Event Contract SOLLTE mindestens enthalten:

- Event-/Message-Name,
- fachliche Bedeutung,
- Producer Owner,
- Channel/Topic,
- Payload Schema,
- Header/Metadata,
- Version/Lifecycle,
- Security-/Zugriffsannahmen,
- Delivery-/Ordering-relevante Hinweise,
- Fehler-/DLQ-Konzept soweit relevant,
- Beispielnachricht.

## 5. Semantik vor Schema

Schwach:

```text
OrderUpdated
```

wenn niemand weiß, **was** geändert wurde und was Consumer daraus ableiten dürfen.

Stärker:

```text
OrderPaymentConfirmed
```

mit dokumentierter fachlicher Bedeutung und Ownership.

Events sollten nicht zu technischen Datenänderungsfeeds verkommen, wenn Consumer eigentlich fachliche Bedeutung benötigen.

## 6. Ownership

Für jeden extern relevanten Eventtyp MUSS ein Owner erkennbar sein.

Der Owner verantwortet:

- Semantik,
- Schema-Evolution,
- Deprecation,
- Consumer-Kommunikation,
- Betriebs-/Incident-Koordination.

Ein Kafka-Topic ohne fachlichen Owner ist kein belastbarer Integrationsvertrag.

## 7. Compatibility

Schema-Kompatibilität ist wichtig, aber nicht ausreichend.

### Syntaktisch

- Feld hinzufügen/entfernen,
- Required/Optional,
- Typänderung,
- Enum-Erweiterung.

### Semantisch

- Bedeutung eines Feldes ändert sich,
- Event wird zu einem anderen Zeitpunkt emittiert,
- bisher garantierte Ordering-Semantik ändert sich,
- Consumer dürfen nicht mehr dieselbe fachliche Schlussfolgerung ziehen.

Semantic Breaking Changes müssen genauso behandelt werden wie Schema-Brüche.

## 8. Delivery Semantics dokumentieren

Consumer müssen wissen, womit sie rechnen müssen:

- Duplikate möglich?
- Ordering pro Key?
- Retry?
- Replay?
- maximale Verzögerung?
- Dead Letter Handling?

Die Contract-Dokumentation darf keine „Exactly Once“-Garantie versprechen, wenn nur Teilmechanismen diese Semantik unterstützen.

## 9. Security und Datenschutz

Event Contracts SOLLEN sichtbar machen:

- sensible Datenfelder,
- Zugriffs-/ACL-Modell,
- Retention,
- gegebenenfalls Verschlüsselungs-/Pseudonymisierungsanforderungen.

Events erzeugen häufig langlebige Kopien. Deshalb müssen Datenminimierung und Lifecycle früh geklärt werden.

## 10. CI-Governance

```text
AsyncAPI Change
→ Syntax/Spec Validation
→ Style/Lint Rules
→ Schema Compatibility
→ semantisches Review bei relevanten Änderungen
→ Contract Tests
→ Veröffentlichung im Event Catalog
```

Nicht jeder kosmetische Unterschied sollte Deployment blockieren.

## 11. Event Catalog

Bei größerer Event-Landschaft SOLLTE ein Katalog mindestens sichtbar machen:

- Event,
- Producer,
- bekannte Consumer,
- fachliche Domäne,
- Contract,
- Lifecycle,
- Datenklassifikation.

## 12. Quellen

- AsyncAPI Specification 3.1.0  
  https://www.asyncapi.com/docs/reference/specification/v3.1.0
- AsyncAPI Release Notes 3.1.0  
  https://www.asyncapi.com/blog/release-notes-3.1.0
- AK-019 — Contract Testing
- AK-041 — Event-Driven Architecture Decision Guide

## 13. Merksatz

> Ein Event ist nicht nur ein JSON-Objekt auf Kafka. Es ist ein **fachlicher Vertrag mit Owner, Semantik, Lifecycle und Betriebsfolgen**.
