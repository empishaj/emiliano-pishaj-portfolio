---
id: AK-101
legacy_ids:
  - ADR-101
title: Spring Security Resource Server Baseline
artifact_type: architecture-standard
domain: iam-security
status: active
maturity: reviewed
normative_level: normative
technology_baseline:
  spring_security: "current stable line; validate before implementation"
owner_role: Application Security / Platform Engineering
last_validated: 2026-10-01
review_trigger:
  - Spring-Security-Major-Version
  - Änderung des zentralen IAM-/Tokenmodells
---

# AK-101 — Spring Security Resource Server Baseline

## 1. Zweck

AK-040 erklärt die IAM- und Tokenentscheidungen. Dieses Dokument beschreibt die **technische Baseline**, wenn ein Spring-basierter HTTP-Service als OAuth2 Resource Server betrieben wird.

Der Standard ist bewusst frameworkbezogen. Die darüberliegende IAM-Architektur bleibt frameworkunabhängig.

## 2. Normative Regeln

### Framework verwenden

JWT- oder opaque-token-basierte Bearer Tokens MÜSSEN über die dafür vorgesehenen Spring-Security-Mechanismen validiert werden.

DARF NICHT:

- JWT manuell mit String-/Base64-Logik parsen,
- Claims ungeprüft als Benutzer-/Rolleninformation übernehmen,
- Security für Integrationstests global deaktivieren und dadurch das Produktionsverhalten ungetestet lassen.

### Issuer

Ein JWT-basierter Resource Server MUSS den erwarteten Issuer validieren.

### Gültigkeitszeit

`exp` und gegebenenfalls `nbf` MÜSSEN über die Standardvalidierung berücksichtigt werden.

### Audience

Wenn der Tokenvertrag eine Audience verwendet, MUSS die Resource-Server-Konfiguration sicherstellen, dass das Token für die konkrete API vorgesehen ist.

### Authorities / Scopes

Die Abbildung externer Claims auf interne Authorities MUSS zentral und nachvollziehbar erfolgen.

Business-Code SOLLTE nicht überall provider-spezifische Claimstrukturen wie `realm_access.roles` kennen.

## 3. Authentisierung und Autorisierung trennen

```text
Token validieren
→ Authentication Context
→ technische Authority/Scope
→ fachliche Autorisierungsregel
```

Beispiel:

```text
Token:
scope = case:write

zusätzliche Fachregel:
case.officeId == currentUser.officeId
```

Nur die Scopeprüfung reicht hier nicht.

## 4. HTTP- und Method Security

### HTTP Security

Geeignet für:

- öffentliche vs. geschützte Pfade,
- technische Endpunkte,
- grobe Scope-/Rollenregeln.

### Method-/Domain Authorization

Geeignet für:

- objektbezogene Regeln,
- fachliche Policies,
- Ownership-/Mandantenregeln.

Die gleiche Policy sollte nicht widersprüchlich an mehreren Stellen dupliziert werden.

## 5. 401 vs. 403

### 401 Unauthorized

Authentisierung fehlt oder ist ungültig.

### 403 Forbidden

Authentisierung ist vorhanden, aber die angeforderte Aktion ist nicht erlaubt.

Fehlerantworten SOLLEN dem organisationsweiten API-Fehlerstandard folgen und keine sensiblen Details offenlegen.

## 6. CSRF differenziert entscheiden

Die alte Guideline enthielt sinngemäß:

> „JWT im Authorization-Header ist CSRF-immun, daher CSRF abschalten.“

Das ist zu pauschal.

CSRF-Risiko hängt insbesondere davon ab, **ob Credentials vom Browser automatisch mitgesendet werden**.

Für eine reine Bearer-Token-API, bei der der Token ausschließlich explizit über den Authorization Header gesetzt wird und keine cookie-basierte Authentisierung eingesetzt wird, kann eine deaktivierte serverseitige CSRF-Funktion angemessen sein.

Sobald Cookies, browserseitige Sessions oder andere automatisch angefügte Credentials beteiligt sind, MUSS CSRF separat bewertet werden.

CORS ersetzt CSRF-Schutz nicht.

## 7. Service-to-Service

Für M2M-Kommunikation wird geklärt:

- handelt Service A im eigenen Namen?
- handelt er „on behalf of“ eines Nutzers?
- welche minimale Berechtigung benötigt er?
- wie werden Client Credentials / Workload Credentials bereitgestellt?

Langlebige Secrets SOLLTEN vermieden werden, wenn Plattform/IAM kurzlebige Workload Credentials ermöglichen.

## 8. Claim-Normalisierung

Provider-spezifische Tokenstrukturen sollten an einer Grenze auf ein internes Modell abgebildet werden.

Beispiel:

```java
public record CallerIdentity(
        String subject,
        Set<String> scopes,
        Set<String> roles,
        Optional<String> tenantId) {
}
```

Der Beispieltyp ist nicht normativ. Das Prinzip ist wichtig: Providerdetails sollen sich nicht durch die gesamte Domänenlogik ziehen.

## 9. Tests

MUSS mindestens abdecken:

- kein Token → 401,
- ungültiges/abgelaufenes Token → 401,
- gültiges Token ohne Berechtigung → 403,
- gültiges Token mit Berechtigung → erwartetes Verhalten,
- objektbezogene Ownership-Regel,
- wichtige administrative Pfade,
- Claim-/Scope-Mapping.

Security Tests sind Teil der fachlichen Schnittstellenabsicherung.

## 10. Betriebsanforderungen

SOLLTE:

- Authentisierungs-/Autorisierungsfehler metrisch beobachten,
- keine vollständigen Tokens loggen,
- Key Rotation des Authorization Servers verkraften,
- Fehler bei Discovery/JWK-Verfügbarkeit kontrolliert behandeln.

## 11. Verifikation

- Spring Security Integration Tests,
- API Security Tests,
- Configuration Review,
- Dependency/SCA Check,
- zentrale IAM-Contract-Tests wo vorhanden.

## 12. Quellen

- Spring Security — OAuth2 Resource Server  
  https://docs.spring.io/spring-security/reference/servlet/oauth2/resource-server/index.html
- Spring Security — JWT Resource Server  
  https://docs.spring.io/spring-security/reference/servlet/oauth2/resource-server/jwt.html
- RFC 9700 — OAuth 2.0 Security Best Current Practice  
  https://www.rfc-editor.org/rfc/rfc9700.html
- OWASP CSRF Prevention Cheat Sheet  
  https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html

## 13. Merksatz

> Spring Security implementiert einen Teil der IAM-Architektur. Es ersetzt nicht die Frage, **welche Identität auf welche fachliche Ressource unter welcher Policy zugreifen darf**.
