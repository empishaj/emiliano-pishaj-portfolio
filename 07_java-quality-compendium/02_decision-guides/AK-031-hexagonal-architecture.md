---
id: AK-031
legacy_ids:
  - QG-JAVA-031
title: Hexagonal Architecture – Ports, Adapter und Abhängigkeitsrichtung
artifact_type: decision-guide
domain: application-architecture
status: active
maturity: reviewed
normative_level: informative
last_validated: 2026-09-28
review_trigger:
  - wesentliche Änderung der Application Architecture
---

# AK-031 — Hexagonal Architecture

## 1. Entscheidungsfrage

Hexagonal Architecture ist sinnvoll, wenn fachliche Logik vor volatilen technischen Details geschützt werden soll.

Die Frage lautet:

> Welche Teile des Systems müssen unabhängig von Webframework, Datenbank, Messaging oder externen Providern verständlich und testbar bleiben?

## 2. Grundmodell

```text
Driving Adapter
   ↓
Input Port / Use Case
   ↓
Application / Domain
   ↓
Output Port
   ↓
Driven Adapter
```

Beispiele:

- REST Controller → Input Port,
- Use Case → Domain,
- Repository Interface → Output Port,
- JPA Adapter → Datenbank.

## 3. Was ein Port ist

Ein Port ist ein Vertrag aus Sicht des Anwendungskerns.

Input Port:

> Welche Fähigkeit bietet der Kern?

Output Port:

> Welche externe Fähigkeit benötigt der Kern?

Ein Port ist nicht einfach „Interface vor jeder Klasse“.

## 4. Abhängigkeitsrichtung

Der Kern soll fachlich wichtige Begriffe definieren.

Beispiel:

```text
Domain kennt PaymentGateway
StripeAdapter implementiert PaymentGateway
```

Nicht:

```text
Domain importiert Stripe SDK
```

Das schützt fachliche Logik vor Providerdetails.

## 5. Wann die Trennung besonders wertvoll ist

- komplexe Domänenlogik,
- mehrere externe Adapter,
- langfristige Systeme,
- hohe Testbarkeitsanforderungen,
- volatile Provider/Legacy-Schnittstellen,
- Migration von Infrastruktur.

## 6. Wann sie zu viel sein kann

- sehr kleine CRUD-Anwendung,
- kurze Lebensdauer,
- kaum fachliche Logik,
- ein stabiler technischer Stack und geringe Änderungsrisiken.

Ein System mit drei Endpunkten braucht nicht automatisch sechs Layer und dutzende Ports.

## 7. Mapping an Grenzen

Hexagonal Architecture führt häufig zu unterschiedlichen Modellen:

- API DTO,
- Application Command,
- Domain Object,
- Persistence Model.

Diese Trennung ist sinnvoll, wenn sie reale Änderungsgrenzen schützt.

Sie ist unnötig, wenn jedes Feld viermal identisch kopiert wird, ohne unabhängige Evolution.

## 8. Transaktionsgrenze

Typischerweise liegt die Orchestrierung eines Use Cases in der Application-Schicht.

Die genaue Transaktionssteuerung hängt von Technologie und Modell ab.

Wichtig ist:

- Domain kennt keine Framework-Transaktionsannotation als Pflicht,
- externe Netzwerkaufrufe werden nicht unreflektiert in DB-Transaktionen eingebettet,
- Konsistenzbedarf ist explizit.

## 9. Testing

Vorteile:

- Domain-Tests ohne Framework,
- Adapter separat integrierbar,
- Ports gut mock-/fakebar.

Aber zu viele Mocks können Implementation Details fixieren. Tests sollen Verhalten und Verträge schützen.

## 10. Architekturtests

Mögliche Fitness Function:

```text
Domain packages may not depend on
Spring, JPA, HTTP or provider-specific SDK packages.
```

Dies ist nur sinnvoll, wenn die Organisation genau diese Grenze beschlossen hat.

## 11. Anti-Patterns

- Interface für jede interne Klasse.
- Layernamen ohne echte Abhängigkeitskontrolle.
- Domain darf zwar kein Spring importieren, ist aber vollständig vom JPA-Datenmodell geprägt.
- Mapping-Explosion ohne Änderungsnutzen.
- Hexagonal Architecture als „Clean Architecture Zertifizierung“.

## 12. Von diesem Guide zum ADR

Ein echtes ADR beantwortet zum Beispiel:

> „Braucht Fachverfahren X eine frameworkunabhängige Domänenschicht oder ist eine einfachere Layered Architecture ausreichend?“

Dazu werden Qualitätsziele und Änderungsrisiken bewertet.

## 13. Quellen

- Alistair Cockburn — Hexagonal Architecture / Ports and Adapters
- David Parnas — Information Hiding
- AK-084 — Kopplung und Information Hiding

## 14. Coach-Merksatz

> Hexagonal Architecture ist erfolgreich, wenn **fachlich wichtige Entscheidungen stabiler sind als die technischen Adapter um sie herum** – nicht wenn das Projekt möglichst viele Packages namens `port` und `adapter` besitzt.
