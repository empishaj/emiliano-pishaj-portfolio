---
id: AK-118
legacy_ids:
  - ADR-118
title: Browser Security – CORS, CSP und Response Header
artifact_type: architecture-standard
domain: web-security
status: active
maturity: reviewed
normative_level: normative
owner_role: Application Security
last_validated: 2026-10-01
review_trigger:
  - relevante Browser-/OWASP-Empfehlungsänderung
  - Frontend-Hosting- oder Authentisierungsänderung
---

# AK-118 — CORS, CSP und Browser Security korrekt einordnen

## 1. Die wichtigste Korrektur

CORS ist **keine allgemeine CSRF-Schutzmaßnahme**.

CORS steuert, welche Browser-Origin JavaScript unter bestimmten Bedingungen auf Antworten einer anderen Origin zugreifen darf.

OWASP weist ausdrücklich darauf hin, dass übliche CSRF-Schutzmaßnahmen weiterhin erforderlich sind. Authentisierung, Authorization und CSRF müssen deshalb getrennt betrachtet werden.

## 2. CORS

### Ziel

Nur die Web-Origins zulassen, die die API tatsächlich browserseitig konsumieren dürfen.

### MUSS

- explizite Origins für sensitive APIs definieren,
- Server-Side Authorization unabhängig von CORS durchführen,
- Methods und Headers auf den benötigten Umfang begrenzen,
- Credentials nur aktivieren, wenn das Client-/Auth-Modell dies erfordert.

### DARF NICHT

- eingehenden `Origin` ungeprüft spiegeln,
- `*` für sensitive Credential-basierte Zugriffe als bequemen Default verwenden,
- CORS als Zugriffsschutz für nicht-browserbasierte Clients behandeln.

Ein nicht-browserbasierter Angreifer kann CORS ignorieren. Deshalb bleibt serverseitige Autorisierung zwingend.

## 3. CSRF separat bewerten

CSRF hängt insbesondere davon ab, ob der Browser Credentials automatisch mitsendet, zum Beispiel Cookies.

Prüffragen:

- Cookie-/Session-Auth oder expliziter Authorization Header?
- SameSite-Konfiguration?
- state-changing Requests?
- CSRF Token erforderlich?
- Origin/Referer-Prüfung als zusätzliche Schicht?

Die konkrete Policy steht im Authentisierungsmodell, nicht in der CORS-Konfiguration.

## 4. Content Security Policy

CSP begrenzt, aus welchen Quellen Browser Inhalte wie Scripts, Styles, Frames oder Netzwerkverbindungen laden dürfen.

CSP ist insbesondere ein wichtiger Defense-in-Depth-Control gegen XSS-Auswirkungen.

### SOLL

- restriktiv beginnen,
- `default-src` und spezialisierte Direktiven bewusst definieren,
- unnötiges `unsafe-inline` und `unsafe-eval` vermeiden,
- `frame-ancestors` verwenden, wenn Einbettung eingeschränkt werden soll,
- `connect-src` auf reale Backend-/WebSocket-Ziele begrenzen.

## 5. CSP schrittweise einführen

Eine neue Policy kann legitime Ressourcen blockieren.

Daher ist ein kontrollierter Rollout sinnvoll:

```text
Policy entwerfen
→ Report-Only
→ Verstöße analysieren
→ Quellen bereinigen / Policy korrigieren
→ Enforcement
→ Reporting überwachen
```

Aktuelle CSP-Reporting-Mechanismen sollten bei Implementierung gegen Browser-/MDN-Dokumentation geprüft werden; alte Beispiele mit ausschließlich `report-uri` können veraltet sein.

## 6. HSTS

HSTS teilt Browsern mit, einen Host künftig nur über HTTPS anzusprechen.

Wichtig:

- HSTS wirkt für zukünftige Browserzugriffe,
- der Header muss über HTTPS ausgeliefert werden,
- `includeSubDomains` und insbesondere Preload dürfen nur gesetzt werden, wenn die Domainstruktur dafür vorbereitet ist.

Preload ist kein Schalter, den man pauschal kopiert: Er hat Auswirkungen auf alle betroffenen Subdomains und erfordert einen kontrollierten Lifecycle.

## 7. Weitere Header

Je nach Anwendungskontext prüfen:

- `X-Content-Type-Options: nosniff`,
- `Referrer-Policy`,
- `Permissions-Policy`,
- `frame-ancestors` in CSP,
- gegebenenfalls `X-Frame-Options` für Legacy-/Defense-in-Depth-Kontexte.

Header werden nicht blind als Checkliste gesetzt. Eine reine JSON-API hat andere Browser-Concerns als eine HTML-Anwendung.

## 8. Zentral oder lokal?

Security Header können auf verschiedenen Ebenen gesetzt werden:

- Anwendung,
- Reverse Proxy,
- API Gateway,
- CDN / Edge.

Die Architektur MUSS klären, **welche Ebene führend ist**.

Doppelkonfiguration mit widersprüchlichen Policies ist zu vermeiden.

## 9. Verifikation

MUSS/SOLLTE je nach Kontext:

- Integrationstest der Header,
- Browser-/E2E-Test für CSP,
- Report-Only-Auswertung,
- automatischer Security Header Check,
- Test unerlaubter CORS-Origin,
- Test erlaubter Origin,
- separate CSRF-Tests bei cookie-/sessionbasierten Anwendungen.

## 10. Anti-Patterns

- CORS = CSRF-Schutz.
- `Access-Control-Allow-Origin: *` als Default.
- CSP mit `unsafe-inline`/`unsafe-eval` kopieren, ohne Risiko zu verstehen.
- HSTS Preload aktivieren, ohne alle Subdomains zu kontrollieren.
- Security Header nur im Entwicklercode dokumentieren, aber am tatsächlichen Edge anders ausliefern.

## 11. Quellen

- OWASP HTTP Security Response Headers Cheat Sheet  
  https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Headers_Cheat_Sheet.html
- OWASP HTML5 Security Cheat Sheet — CORS  
  https://cheatsheetseries.owasp.org/cheatsheets/HTML5_Security_Cheat_Sheet.html
- OWASP CSRF Prevention Cheat Sheet  
  https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html
- MDN Content-Security-Policy  
  https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Content-Security-Policy
- MDN Strict-Transport-Security  
  https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Strict-Transport-Security

## 12. Merksatz

> Browser Security wird gefährlich, wenn Headernamen mit Sicherheitszielen verwechselt werden. Kläre zuerst **Angriffsmodell und Credential-Verhalten**, dann CORS, CSRF, CSP und Header als passende Controls.
