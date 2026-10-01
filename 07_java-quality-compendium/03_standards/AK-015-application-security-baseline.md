---
id: AK-015
legacy_ids:
  - QG-JAVA-015
title: Application Security Baseline für Java-/Web-Services
artifact_type: architecture-standard
domain: application-security
status: active
maturity: reviewed
normative_level: normative
owner_role: Security Architecture
last_validated: 2026-10-01
review_trigger:
  - neue OWASP-ASVS-Hauptversion
  - wesentliche Änderung des Authentisierungs- oder Plattformmodells
  - schwerwiegender Security-Incident
---

# AK-015 — Application Security Baseline

## 1. Zweck

Dieser Standard bündelt **keine komplette Security-Architektur**. Er definiert die Mindestlogik, die neue oder wesentlich modernisierte Web-/API-Services bei Design, Implementierung, Review und Delivery berücksichtigen müssen.

Security wird nicht als einzelner Framework-Schalter verstanden, sondern als Kette:

```text
Threat / Schutzbedarf
→ Control
→ Implementierung
→ Test / Scan / Review
→ Runtime Evidence
```

## 2. Geltungsbereich

Der Standard gilt für:

- HTTP-/REST-Services,
- Browseranwendungen mit Backend,
- interne APIs,
- externe APIs,
- Service-to-Service-Kommunikation,
- Batch-/Worker-Komponenten soweit sie Security-relevante Daten verarbeiten.

Spezialisierte Themen werden in eigenen Knowledge-Items vertieft, insbesondere IAM, Input Validation, Privacy, Supply Chain und Browser Header.

## 3. Verbindliche Grundregeln

### 3.1 Trust Boundaries explizit machen

Externe Eingaben, Tokens, Events, Dateien und Daten anderer Systeme werden nicht automatisch vertraut.

MUSS:

- Herkunft und Vertrauensniveau benennen,
- syntaktische und fachliche Validierung trennen,
- Autorisierung serverseitig durchsetzen,
- Datenzugriffe auf tatsächliche Ownership beschränken.

### 3.2 Authentisierung nicht selbst erfinden

MUSS:

- etablierte, gepflegte IAM-/Security-Komponenten verwenden,
- Token-/Credential-Prüfung über standardkonforme Libraries durchführen,
- Signatur-, Issuer-, Zeit- und gegebenenfalls Audience-/Scope-Prüfung passend zum Tokenmodell sicherstellen.

DARF NICHT:

- JWT nur Base64-dekodieren und Claims ungeprüft verwenden,
- Passwörter oder Secrets im Code speichern,
- Authentisierung und Autorisierung vermischen.

### 3.3 Autorisierung an Ressourcen und Aktionen binden

Ein gültiger Login bedeutet nicht automatisch Zugriff auf jede Ressource.

MUSS:

- Operation, Rolle/Scope und gegebenenfalls Objekt-/Tenant-Ownership prüfen,
- 401 und 403 semantisch korrekt unterscheiden,
- sensible Administrative Endpunkte separat absichern.

### 3.4 Eingaben validieren und Ausgaben kontextgerecht behandeln

MUSS:

- Eingaben an der Systemgrenze validieren,
- Datenbankabfragen parameterisieren,
- Output-Encoding beziehungsweise Framework-Escaping passend zum Ausgabekontext verwenden,
- Dateipfade, URLs, Header und Deserialisierung als eigene Angriffsflächen behandeln.

### 3.5 Secrets auslagern

MUSS:

- Secrets aus dafür vorgesehenem Secret Management beziehen,
- Rotation ermöglichen,
- Secrets aus Logs und Fehlermeldungen fernhalten.

DARF NICHT:

- produktive Secrets in Repository, Image oder Klartext-Konfiguration einbetten.

### 3.6 Fehler sicher behandeln

MUSS:

- nach außen stabile, minimale Fehlerverträge verwenden,
- intern ausreichend Diagnosekontext erzeugen,
- Stacktraces, SQL, Tokens und interne Topologie nicht ungefiltert an Clients ausgeben.

### 3.7 Security Events beobachtbar machen

MUSS:

- sicherheitsrelevante Ereigniskategorien definieren,
- Korrelationsinformationen verwenden,
- sensible Inhalte minimieren,
- Alerts und Incident-Prozess für kritische Befunde besitzen.

## 4. Security by Architecture

Security Controls werden bereits bei Architekturentscheidungen betrachtet.

Beispiel:

```text
Entscheidung:
Service verarbeitet hoch schutzbedürftige Fachdaten

Fragen:
- Wer darf zugreifen?
- Wo wird Autorisierung entschieden?
- Wie werden technische Identitäten behandelt?
- Welche Daten dürfen geloggt werden?
- Welche Netz- und Plattformgrenzen existieren?
- Wie erfolgt Recovery ohne Schutzverlust?
```

Security darf nicht erst nach Implementierung als „Penetration-Test-Phase“ beginnen.

## 5. Defense in Depth

Ein einzelner Control ist kein vollständiges Sicherheitsmodell.

Beispiel:

```text
API Gateway
+ Resource Server Authorization
+ Domain Ownership Check
+ DB Least Privilege
+ Audit Event
```

Die Ebenen sollen sich ergänzen, nicht widersprüchliche Policies duplizieren.

## 6. Verifikation

Je nach Risiko SOLLEN kombiniert werden:

- Code Review,
- SAST,
- SCA / Dependency Scan,
- Secret Detection,
- IaC-/Container-Scanning,
- Security Tests,
- Contract-/Authorization Tests,
- DAST bei geeigneten Systemen,
- manuelles Security Review,
- Penetration Test bei entsprechendem Risiko.

Ein grüner Scanner bedeutet nicht automatisch sichere Architektur.

## 7. Ausnahmeprozess

Eine Ausnahme benötigt:

- betroffene Regel,
- konkreten Grund,
- Risikobewertung,
- Kompensationsmaßnahme,
- Owner,
- Ablaufdatum oder Exit-Trigger.

„Legacy“ allein ist keine vollständige Begründung.

## 8. Verwandte Standards

- AK-040 — OAuth2/OIDC Decision Guide
- AK-057 — Software Supply Chain
- AK-069 — Configuration & Secrets
- AK-101 — Spring Security Resource Server
- AK-106 — Privacy Controls
- AK-118 — Browser Security Header
- AK-125 — Input Validation

## 9. Quellen

- OWASP Application Security Verification Standard  
  https://owasp.org/www-project-application-security-verification-standard/
- OWASP Cheat Sheet Series  
  https://cheatsheetseries.owasp.org/
- RFC 9700 — OAuth 2.0 Security Best Current Practice  
  https://www.rfc-editor.org/rfc/rfc9700.html

## 10. Merksatz

> Security ist kein Framework-Häkchen. Ein belastbarer Security-Standard verbindet **Threat, Control, Ownership, technische Durchsetzung und überprüfbare Evidence**.
