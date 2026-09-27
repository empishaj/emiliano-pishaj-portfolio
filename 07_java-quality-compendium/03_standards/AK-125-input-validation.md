---
id: AK-125
legacy_ids:
  - QG-JAVA-125
title: Input Validation und Trust Boundaries
artifact_type: security-standard
domain: application-security
status: active
maturity: reviewed
normative_level: normative
owner_role: Application Security
last_validated: 2026-09-28
review_trigger:
  - neue Eingabe-/Datei-/Deserialisierungswege
  - Änderung der API- oder Messaging-Standards
---

# AK-125 — Input Validation als Boundary Control

## 1. Kernidee

Input Validation beantwortet nicht die Frage:

> „Enthält der String gefährliche Zeichen?“

Sondern:

> **Entspricht diese Eingabe dem Vertrag und der fachlich zulässigen Bedeutung an dieser Trust Boundary?**

Validation ist deshalb mehrstufig.

## 2. Vier Ebenen

### 2.1 Parsing / Typ

Kann der Wert überhaupt in den erwarteten Typ überführt werden?

Beispiele:

- Datum,
- Integer,
- Enum,
- UUID,
- strukturierter JSON-Vertrag.

### 2.2 syntaktische Validierung

Entspricht der Wert der erlaubten Form?

Beispiele:

- Länge,
- Wertebereich,
- erlaubte Zeichenkategorie,
- erlaubte Enum-Werte,
- vollständiger Regex-Match bei geeigneten strukturierten Werten.

### 2.3 semantische / fachliche Validierung

Ist der Wert im Geschäftsprozess zulässig?

Beispiele:

- Enddatum nicht vor Startdatum,
- Statuswechsel erlaubt,
- Dokumenttyp für diesen Vorgang zulässig,
- Menge innerhalb fachlicher Grenze.

### 2.4 Authorization

Darf der aktuelle Akteur diese gültige Eingabe auf diese Ressource anwenden?

Validation ersetzt keine Authorization.

## 3. Allowlist statt Blacklist als Grundhaltung

OWASP empfiehlt positive/allowlist-basierte Validierung für strukturierbare Eingaben.

Schwach:

```text
verboten:
<script>
DROP TABLE
' OR 1=1
```

Solche Listen sind leicht zu umgehen und blockieren legitime Daten.

Stärker:

```text
countryCode ∈ ISO-Ländercodes
status ∈ definierter Enum
quantity 1..100
identifier folgt vereinbartem Format
```

Bei freiem Unicode-Text ist eine enge Zeichen-Allowlist nicht immer sinnvoll. Dort sind Normalisierung, Längenlimits und kontextgerechte Verarbeitung wichtiger.

## 4. Injection wird nicht nur durch Validation verhindert

Input Validation ist Defense in Depth.

Für Interpreter-/Query-Kontexte braucht es zusätzlich sichere APIs.

### SQL

MUSS parameterisierte Queries / ORM-Parameterbindung verwenden.

### HTML

MUSS kontextbezogenes Output Encoding beziehungsweise sicheres Template-Verhalten verwenden.

### Shell

SOLL Shellaufrufe mit untrusted Input vermeiden; wenn notwendig, sichere APIs und strikte Argumentbehandlung verwenden.

### LDAP/NoSQL/etc.

Jeweilige sichere Query-/Escape-Mechanismen nutzen.

## 5. Bean Validation sinnvoll verwenden

Frameworkvalidierung eignet sich gut für syntaktische DTO-Regeln:

```java
public record CreateCaseRequest(
        @NotBlank
        @Size(max = 100)
        String title,

        @NotNull
        LocalDate receivedAt) {
}
```

Fachregeln gehören dagegen nicht zwangsläufig in Annotationen.

Beispiel:

```text
"Antrag darf nur nach Verfahrensöffnung ergänzt werden"
```

ist Domänenlogik, keine reine DTO-Validierung.

## 6. Dateien

Uploads sind eine eigene Trust Boundary.

Prüfen:

- maximale Größe,
- erlaubter fachlicher Dateityp,
- tatsächlicher Content Type / Signatur statt nur Dateiendung,
- Dateiname nicht als sicherer Pfad verwenden,
- Malware-/Content-Scan gemäß Risikomodell,
- Storage außerhalb direkt ausführbarer Webpfade,
- Dekompressions-/Zip-Bomb-Risiken.

## 7. URLs und SSRF

Eine syntaktisch gültige URL kann trotzdem gefährlich sein.

Bei serverseitigem Fetching prüfen:

- erlaubte Protokolle,
- erlaubte Ziele / Netzbereiche,
- Redirect-Verhalten,
- DNS-Rebinding-/interne Netzrisiken,
- Cloud Metadata Endpoints.

## 8. Events und Nachrichten

Auch interne Events sind nicht automatisch vertrauenswürdig.

MUSS/SOLLTE:

- Schema validieren,
- Version berücksichtigen,
- semantische Constraints prüfen,
- Producer Identity/Topic Policy berücksichtigen,
- ungültige Nachrichten kontrolliert behandeln.

## 9. Normalisierung und Canonicalization

Bei bestimmten Eingaben muss vor fachlicher Prüfung ein kanonischer Zustand hergestellt werden.

Beispiele:

- Unicode-Normalisierung,
- Pfade,
- Case-Sensitivity bei Identifiers,
- Telefonnummern,
- fachlich definierte Nummernformate.

Achtung: Normalisierung ist domänenabhängig. Namen oder freie Texte dürfen nicht blind „bereinigt“ und dadurch semantisch verändert werden.

## 10. Fehlermeldungen

Extern:

- verständlich,
- feldbezogen,
- ohne interne Details.

Intern:

- ausreichend korrelierbar für Diagnose,
- ohne unnötig sensible Eingabeinhalte zu protokollieren.

## 11. Verifikation

- positive und negative Boundary Tests,
- Property-Based Testing für geeignete Invarianten,
- Fuzzing für Parser/Protokolle mit erhöhtem Risiko,
- Security Tests für Injection-/Path-/URL-Fälle,
- Contract Tests für Schema-Validierung.

## 12. Quellen

- OWASP Input Validation Cheat Sheet  
  https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html
- OWASP Injection Prevention Cheat Sheet  
  https://cheatsheetseries.owasp.org/cheatsheets/Injection_Prevention_Cheat_Sheet.html
- OWASP File Upload Cheat Sheet
- OWASP SSRF Prevention Cheat Sheet

## 13. Coach-Merksatz

> Validierung bedeutet nicht, gefährliche Strings zu erraten. Sie bedeutet, an jeder Trust Boundary **den zulässigen Vertrag, die fachliche Bedeutung und die Berechtigung explizit zu machen**.
