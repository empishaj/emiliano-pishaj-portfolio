---
id: AK-040
legacy_ids:
  - ADR-040
title: OAuth2, OpenID Connect und Token-Architektur
artifact_type: decision-guide
domain: iam-security
status: active
maturity: reviewed
normative_level: informative
last_validated: 2026-10-01
review_trigger:
  - neue OAuth Security BCP
  - Änderung des zentralen IAM
  - neue Client- oder Föderationsklassen
---

# AK-040 — OAuth2, OIDC und Token-Architektur richtig entscheiden

## 1. Warum dieses Dokument ein Decision Guide ist

OAuth2, OpenID Connect und JWT sind keine austauschbaren Synonyme.

Eine konkrete Organisation muss entscheiden:

- welcher Akteur auf welche Ressource zugreift,
- ob eine Benutzeridentität oder eine technische Identität vorliegt,
- welche Claims/Scopes benötigt werden,
- welches Tokenformat eingesetzt wird,
- wo Autorisierung stattfindet,
- welche Vertrauensgrenzen gelten.

AK-040 liefert die Entscheidungslogik. Es trifft keine projektspezifische IAM-Entscheidung und ersetzt keinen verbindlichen Security-Standard.

## 2. Begriffe sauber trennen

### OAuth 2.0

OAuth2 ist ein Autorisierungsframework für delegierten Zugriff auf geschützte Ressourcen.

### OpenID Connect

OIDC erweitert OAuth2 um standardisierte Authentisierungsinformationen über den End-User, insbesondere über das ID Token und definierte Claims/Flows.

### Access Token

Ein Access Token repräsentiert eine Berechtigung gegenüber einem Resource Server.

Wichtig:

> OAuth2 schreibt nicht generell vor, dass Access Tokens JWTs sein müssen.

Je nach Architektur können Access Tokens opaque oder strukturiert sein.

### JWT

JWT ist ein kompaktes Format für Claims. Ein JWT ist nicht automatisch ein OAuth Access Token und nicht automatisch vertrauenswürdig.

Vertrauen entsteht erst durch passende Validierung und den umgebenden Sicherheitsvertrag.

## 3. Entscheidungsfrage 1 — Wer handelt?

### Menschlicher Benutzer

Typische Fragen:

- Browser, Mobile App oder Desktop Client?
- braucht die Anwendung Login-/Sessioninformation?
- welche Föderation existiert?
- welche MFA-/Assurance-Anforderungen gelten?

OIDC ist typischerweise relevant, wenn die Anwendung Authentisierungsinformationen über einen Benutzer benötigt.

### Technischer Client / Service

Typische Fragen:

- besitzt der Service eine eigene Identität?
- handelt er im eigenen Namen oder im Auftrag eines Nutzers?
- welche Scopes/Ressourcen benötigt er?
- kann die Identität kurzlebig ausgestellt werden?

Client Credentials kann für bestimmte Machine-to-Machine-Szenarien passen. Es ist nicht dasselbe wie Benutzerdelegation.

## 4. Entscheidungsfrage 2 — Opaque oder JWT Access Token?

### Opaque Token

Vorteile:

- Claims müssen nicht an jeden Consumer verteilt werden,
- zentrale Introspection kann Policy-/Revocation-Kontrolle unterstützen.

Trade-offs:

- zusätzliche Laufzeitabhängigkeit und Latenz,
- Introspection muss skalieren und hochverfügbar sein.

### JWT Access Token

Vorteile:

- lokale kryptografische Validierung möglich,
- weniger Laufzeitkopplung an den Authorization Server.

Trade-offs:

- Claims sind bis Tokenablauf verteilt,
- Revocation/Policy-Änderungen sind schwieriger sofort durchzusetzen,
- Claim-/Key-Lifecycle wird relevant.

Entscheidungstreiber:

- Revocation-Bedarf,
- Latenz,
- Datenschutz der Claims,
- Offline-/lokale Validierung,
- Anzahl Resource Server,
- Betriebsmodell.

## 5. Was ein JWT-basierter Resource Server berücksichtigen muss

Wenn eine Architektur JWT-basierte Access Tokens vorsieht, muss der zugehörige Security-Standard festlegen, welche Prüfungen verbindlich sind. Typischerweise gehören dazu:

- kryptografische Signatur,
- erlaubter Algorithmus und Schlüssel,
- `iss`,
- Gültigkeitszeit (`exp`, gegebenenfalls `nbf`),
- vorgesehene Audience, wenn im Modell verwendet,
- erforderliche Scopes/Authorities,
- gegebenenfalls Tenant-/Domain-Claims.

Claims dürfen nicht allein deshalb vertraut werden, weil sie syntaktisch in einem JWT stehen.

Die konkrete Spring-Security-Umsetzung wird in `AK-101` behandelt.

## 6. RFC 9700 als aktuelle Sicherheitsbasis

RFC 9700 wurde 2025 als OAuth 2.0 Security Best Current Practice veröffentlicht und aktualisiert beziehungsweise erweitert die Security-Empfehlungen zu OAuth2.

Wichtige Konsequenzen für neue Architekturen:

- veraltete oder unsichere Flows nicht aus Kompatibilitätsbequemlichkeit fortschreiben,
- Redirect URIs streng behandeln,
- Token Leakage und Replay berücksichtigen,
- PKCE für geeignete Authorization-Code-Flows einsetzen,
- Sender-Constrained Tokens in risikoreichen Szenarien prüfen,
- TLS als grundlegende Transportvoraussetzung behandeln.

Die konkrete Anwendung hängt vom Client- und Deployment-Modell ab.

## 7. Autorisierung ist mehr als Rollen aus dem Token

Ein Scope oder eine Rolle beantwortet oft nur einen Teil der Frage.

Beispiel:

```text
Rolle:
Sachbearbeitung

Scope:
case:write

zusätzliche fachliche Policy:
Nutzer darf nur Vorgänge der eigenen Organisationseinheit bearbeiten
```

Die Ressourcen-/Objektentscheidung kann deshalb in der Anwendung, einem Policy Decision Point oder einem dedizierten Autorisierungsdienst liegen.

Die Architektur muss explizit machen, **wo die maßgebliche Policy entschieden wird**.

## 8. Token Propagation

Ein häufiger Fehler ist, Benutzer-Tokens unkontrolliert durch lange Serviceketten weiterzureichen.

Fragen:

- Muss Downstream-Service B wirklich die Benutzeridentität kennen?
- benötigt B nur die technische Identität von A?
- wird „on behalf of“ fachlich/regulatorisch benötigt?
- wie wird Least Privilege erhalten?
- wie verhindert man, dass ein Frontend-Token plötzlich Zugriff auf interne Services bedeutet?

Die Lösung kann je nach Kontext Token Exchange, Service Credentials oder bewusst begrenzte Claim Propagation sein.

## 9. Datenschutz und Claims

Tokens sind keine bequemen Datencontainer.

Sinnvolle Leitfragen:

- Welche Claims werden tatsächlich benötigt?
- Welche personenbezogenen Informationen verlassen dadurch ihre ursprüngliche Vertrauensgrenze?
- Werden Tokens oder Claims in Logs, Traces oder Fehlern sichtbar?
- Sind Lebensdauer und Audience angemessen begrenzt?

## 10. Entscheidungsmatrix

| Frage | mögliche Konsequenz |
|---|---|
| Benutzerlogin nötig? | OIDC prüfen |
| API-Autorisierung nötig? | OAuth2 Resource Server |
| M2M im eigenen Namen? | Client Credentials oder Workload Identity |
| sofortige zentrale Revocation sehr wichtig? | opaque/introspection oder kurze Tokenlebensdauer prüfen |
| viele Resource Server mit lokaler Prüfung? | JWT kann sinnvoll sein |
| besonders hohe Replay-Risiken? | Sender Constraint / DPoP / mTLS prüfen |
| alte SSO-Landschaft? | SAML-Föderation kann weiterhin Integrationsrealität sein |

## 11. Anti-Patterns

- OAuth = Login.
- JWT = sicher.
- jede Rolle direkt in jedem Service selbst interpretieren.
- User Token blind über alle Servicegrenzen propagieren.
- langlebige Tokens ohne Risk Review.
- Token vollständig loggen.
- `client_secret` als langlebiges Passwort in `application.yml`.

## 12. Von diesem Guide zum echten ADR

Ein echtes ADR könnte lauten:

> „Wie authentisiert und autorisiert Fachverfahren X technische Zugriffe auf Register Y?“

Dann werden konkrete Optionen, Constraints, Security Requirements, Betriebsmodell und Entscheidung dokumentiert.

## 13. Quellen

- RFC 9700 — OAuth 2.0 Security Best Current Practice  
  https://www.rfc-editor.org/rfc/rfc9700.html
- OpenID Connect Core 1.0  
  https://openid.net/specs/openid-connect-core-1_0.html
- RFC 6750 — Bearer Token Usage
- Spring Security Resource Server Reference  
  https://docs.spring.io/spring-security/reference/servlet/oauth2/resource-server/index.html
- AK-101 — Spring Security Resource Server

## 14. Merksatz

> Beginne IAM nicht mit „JWT oder nicht?“. Beginne mit **Akteur, Ressource, Vertrauensgrenze, Policy und Widerrufs-/Lebenszyklusbedarf**. Das Token ist erst danach eine Designentscheidung.
