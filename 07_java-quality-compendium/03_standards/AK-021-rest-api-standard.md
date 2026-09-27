---
id: AK-021
legacy_ids:
  - QG-JAVA-021
title: REST/HTTP API Design Standard
artifact_type: architecture-standard
domain: integration
status: active
maturity: reviewed
normative_level: normative
owner_role: Integration Architecture
last_validated: 2026-09-28
review_trigger:
  - neue organisationsweite API-Policy
  - relevante neue HTTP-/OpenAPI-Spezifikation
  - wiederkehrende Integrationsprobleme
---

# AK-021 — REST/HTTP API Design Standard

## 1. Zweck

Dieser Standard definiert wiederverwendbare Leitplanken für HTTP-APIs.

Er entscheidet nicht, **ob** REST/HTTP die richtige Integrationsform ist. Diese Entscheidung wird anhand von Interaktions-, Konsistenz-, Latenz- und Kopplungsanforderungen getroffen.

Wenn HTTP-APIs verwendet werden, sollen sie jedoch konsistent, nachvollziehbar und betreibbar sein.

## 2. Ressourcen und Semantik

APIs SOLLEN fachliche Ressourcen und Aktionen verständlich ausdrücken.

Bevorzugt:

```text
GET    /cases/{id}
POST   /cases
PATCH  /cases/{id}
DELETE /cases/{id}
```

Aber fachlich bedeutsame Commands dürfen explizit modelliert werden, wenn reines CRUD die Semantik verschleiern würde.

Beispiel:

```text
POST /cases/{id}/decisions
```

kann klarer sein als ein generisches Patch eines komplexen Statusobjekts.

## 3. HTTP-Methoden und Idempotenz

Methodensemantik MUSS respektiert werden.

- GET liest und soll keine fachliche Zustandsänderung auslösen.
- PUT ersetzt/aktualisiert eine Ressource gemäß vereinbartem Vertrag und ist idempotent zu gestalten.
- DELETE ist nach HTTP-Semantik idempotent zu behandeln.
- POST kann nicht-idempotente fachliche Operationen repräsentieren.

Für kritische POST-Operationen SOLLTE geprüft werden, ob ein Idempotency-Key oder eine fachliche Deduplizierungsstrategie erforderlich ist.

## 4. Statuscodes

Statuscodes werden nach HTTP-Semantik verwendet, nicht nach lokaler Gewohnheit.

Typische Beispiele:

- `200 OK` erfolgreiche Antwort,
- `201 Created` neu erzeugte Ressource,
- `202 Accepted` Verarbeitung angenommen, aber noch nicht abgeschlossen,
- `204 No Content` erfolgreich ohne Response Body,
- `400 Bad Request` syntaktisch/semantisch ungültige Anfrage je API-Vertrag,
- `401 Unauthorized` Authentisierung fehlt/ungültig,
- `403 Forbidden` authentisiert, aber nicht berechtigt,
- `404 Not Found`,
- `409 Conflict` bei geeigneten Zustands-/Versionskonflikten,
- `412 Precondition Failed` bei fehlgeschlagener HTTP-Precondition,
- `422 Unprocessable Content` kann für fachlich nicht verarbeitbare Inhalte passend sein,
- `429 Too Many Requests` Rate Limit.

Ein Standard sollte nicht jeden fachlichen Fehler auf genau einen HTTP-Code zwingen, ohne die Semantik zu prüfen.

## 5. Fehlerformat

Für APIs, die strukturierte Problem Details benötigen, SOLLTE RFC 9457 `application/problem+json` als Basis geprüft beziehungsweise organisationsweit standardisiert werden.

RFC 9457 hat RFC 7807 ersetzt.

Ein Problemobjekt kann organisationsspezifisch erweitert werden, zum Beispiel um:

- stabile Fehlercodes,
- Korrelations-ID,
- fachliche Detailinformationen,

solange sensible interne Informationen nicht offengelegt werden.

## 6. Pagination

Große Collections DÜRFEN NICHT unkontrolliert vollständig ausgeliefert werden.

Mögliche Strategien:

### Offset/Limit

Einfach, aber bei großen/veränderlichen Datenmengen potenziell ineffizient oder instabil.

### Cursor/Keyset

Geeignet für große, geordnete Datenmengen und stabile Fortsetzung.

Die Strategie folgt Zugriffsmuster und Konsistenzbedarf.

## 7. Filter, Sortierung und Projektion

Parameter müssen:

- dokumentiert,
- validiert,
- in erlaubten Feldern begrenzt,
- gegen Injection sicher umgesetzt werden.

Eine beliebige dynamische SQL-/Property-Sprache im Querystring sollte nicht unkontrolliert angeboten werden.

## 8. Concurrency und Preconditions

Bei konkurrierenden Änderungen SOLLTE geprüft werden, ob HTTP Preconditions wie ETag / `If-Match` oder fachliche Versionsfelder geeignet sind.

Damit kann Lost Update kontrolliert werden, statt die letzte Änderung still gewinnen zu lassen.

## 9. Security

Jede API MUSS festlegen:

- Authentisierungsmodell,
- Autorisierungsmodell,
- Scopes/Rollen/Policies,
- Umgang mit technischen Clients,
- Schutz sensibler Daten,
- Rate-/Abuse-Schutz soweit nötig.

OpenAPI-Dokumentation allein ist kein Security Control.

## 10. Versionierung

Breaking Changes benötigen eine bewusste Lifecycle-Strategie.

Mögliche Modelle:

- URI-Version,
- Header-/Media-Type-Version,
- additive Evolution innerhalb stabiler Version.

Das Compendium schreibt kein universelles Versionierungsformat vor.

Wichtiger sind:

- definierte Compatibility-Regeln,
- Breaking-Change-Erkennung,
- Deprecation-/Sunset-Prozess,
- Consumer-Kommunikation.

Siehe AK-110.

## 11. Observability

Eine produktive API SOLLTE mindestens sichtbar machen:

- Request Rate,
- Error Rate,
- Latenz,
- relevante Downstream-Abhängigkeiten,
- Korrelations-/Trace-Kontext.

Personenbezogene Payloads werden nicht pauschal geloggt.

## 12. API-Vertrag

OpenAPI ist die bevorzugte maschinenlesbare Beschreibung für HTTP-APIs im Compendium.

Der Vertrag SOLLTE:

- Operationen,
- Schemas,
- Security,
- Statuscodes,
- Fehlerformate,
- relevante Header,
- Beispiele

dokumentieren.

## 13. Review-Checkliste

1. Ist HTTP das passende Interaktionsmodell?
2. Ist Ownership klar?
3. Sind Ressourcen/Operationen fachlich verständlich?
4. Stimmen HTTP-Semantik und Statuscodes?
5. Gibt es einen strukturierten Fehlervertrag?
6. Ist Pagination nötig?
7. Ist Concurrency behandelt?
8. Ist Security dokumentiert?
9. Ist Compatibility/Lifecycle geklärt?
10. Ist die API beobachtbar und testbar?

## 14. Quellen

- RFC 9110 — HTTP Semantics  
  https://www.rfc-editor.org/rfc/rfc9110.html
- RFC 9457 — Problem Details for HTTP APIs  
  https://www.rfc-editor.org/rfc/rfc9457.html
- OpenAPI Specification  
  https://spec.openapis.org/oas/
- AK-066 — API First / OpenAPI Governance
- AK-110 — API Lifecycle

## 15. Coach-Merksatz

> Ein gutes REST-API ist nicht „RESTful“ um der Reinheit willen. Es ist ein **stabiler, verständlicher, sicherer und evolvierbarer Vertrag zwischen Verantwortungsbereichen**.
