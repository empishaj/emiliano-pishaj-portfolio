---
id: AK-106
legacy_ids:
  - QG-JAVA-106
  - ADR-106
title: Privacy Technical Controls – Minimierung, Redaction, Pseudonymisierung und Löschung
artifact_type: privacy-standard
domain: privacy-data-protection
status: active
maturity: reviewed
normative_level: normative
owner_role: Privacy Architecture / Security Architecture
last_validated: 2026-09-28
review_trigger:
  - Änderung rechtlicher oder behördeninterner Datenschutzvorgaben
  - neue Datenklasse oder neue Telemetrie-/AI-Verarbeitung
  - Privacy Incident
---

# AK-106 — Technische Privacy Controls

## 1. Zweck

Dieses Dokument übersetzt Privacy-by-Design-Prinzipien in technische Kontrollen.

Es ersetzt **keine juristische Bewertung** und entscheidet nicht über Rechtsgrundlagen oder Zweckbindung.

Es beantwortet die technische Frage:

> Wie verhindern wir, dass personenbezogene oder schutzwürdige Informationen unnötig kopiert, offengelegt, geloggt, in Testsysteme übertragen oder länger als erforderlich gespeichert werden?

## 2. Begriffe unterscheiden

### Datenminimierung

Nur Daten verarbeiten, die für den definierten Zweck erforderlich sind.

### Redaction / Masking

Werte werden vollständig oder teilweise unkenntlich gemacht, typischerweise für Anzeige oder Telemetrie.

### Pseudonymisierung

Personenbezug wird durch einen Ersatzbezug reduziert; mit Zusatzinformation kann Zuordnung wieder möglich sein.

Pseudonymisierte Daten sind damit nicht automatisch anonym.

### Verschlüsselung

Schützt Daten gegen unberechtigte Einsicht, ändert aber nicht ihren Personenbezug.

### Anonymisierung

Daten sind so verändert, dass die betroffene Person nicht mehr identifizierbar ist. Dies ist eine hohe Anforderung und darf nicht leichtfertig behauptet werden.

## 3. Normative Regeln

### Datenklassifikation

Für sensible Daten SOLL bekannt sein:

- fachliche Bedeutung,
- Owner,
- Schutzbedarf / Sensitivität,
- zulässige Verarbeitungszwecke,
- Aufbewahrung,
- Lösch-/Archivierungslogik.

### Logging und Tracing

Personenbezogene Inhalte DÜRFEN NICHT standardmäßig in Logs/Traces geschrieben werden.

Stattdessen bevorzugen:

- technische IDs,
- pseudonymisierte Referenzen,
- Ereigniskategorien,
- minimierte strukturierte Attribute.

### Testdaten

Produktionsdaten DÜRFEN NICHT unkontrolliert in Entwicklungs-/Testumgebungen kopiert werden.

Wenn produktionsnahe Daten notwendig sind, braucht es:

- definierten Zweck,
- technische Schutzmaßnahme,
- Zugriffskontrolle,
- Löschfrist,
- dokumentierte Freigabe nach Organisationsvorgaben.

### Events und Messaging

Events SOLLEN nur die Informationen enthalten, die Consumer tatsächlich benötigen.

Ein „Fat Event mit allen Personendaten“ vergrößert Datenkopien, Consumerkreis und Löschkomplexität.

### AI-/LLM-Aufrufe

Personenbezogene oder vertrauliche Inhalte DÜRFEN nur dann an externe Modelle/Provider übermittelt werden, wenn die dafür notwendige fachliche, rechtliche, vertragliche und technische Freigabe existiert.

Masking allein ist nicht automatisch ausreichend.

## 4. Tokenisierung und Pseudonyme

Ein Ersatzidentifier ist sinnvoll, wenn Systeme korrelieren müssen, ohne den Klarwert überall zu verteilen.

Fragen:

- ist der Ersatzwert reversibel?
- wer besitzt die Zuordnung?
- braucht jeder Consumer dieselbe stabile Pseudonym-ID?
- kann dadurch Profilbildung entstehen?
- wie wird Löschung propagiert?

Ein stabiler Hash einer E-Mail-Adresse ist nicht automatisch sichere Pseudonymisierung, insbesondere wenn der Eingaberaum leicht erraten werden kann.

## 5. Löschung ist eine Architekturfrage

Ein Löschprozess betrifft potenziell:

```text
System of Record
→ operative Kopien
→ Suchindex
→ Cache
→ Event-/Stream-Retention
→ Analytics
→ Dokumente
→ Backups
→ Logs
```

Nicht alle Speicherformen werden auf dieselbe Weise gelöscht. Backups können beispielsweise über Retention und kontrollierte Wiederherstellungsprozesse behandelt werden.

Die Architektur MUSS deshalb unterscheiden:

- aktive Daten,
- abgeleitete Daten,
- immutable Audit-/Logdaten,
- Backups,
- rechtlich aufzubewahrende Daten.

## 6. Observability

Privacy und Observability stehen nicht grundsätzlich im Konflikt.

Gute Telemetrie beschreibt:

- was passiert ist,
- wo,
- wann,
- mit welchem technischen Kontext,

ohne unnötig fachliche Inhalte offenzulegen.

Ein Telemetry Collector kann Redaction oder Attribute Filtering zentral unterstützen, ersetzt aber nicht die Minimierung in der Anwendung.

## 7. Datenflüsse als Reviewinstrument

Privacy Review beginnt mit einer Datenflussfrage:

```text
Wo entsteht die Information?
→ wo wird sie übertragen?
→ wo gespeichert?
→ wer kann zugreifen?
→ welche Kopien entstehen?
→ wann wird sie gelöscht?
```

Daraus lassen sich Controls und Verantwortlichkeiten ableiten.

## 8. Verifikation

Mögliche Evidence:

- automatisierte Log-/Telemetry-Tests auf verbotene Felder,
- Datenfluss-/Privacy Review,
- Testdatenprüfung,
- Zugriffskontrollen,
- Lösch-/Retention-Test,
- Scan auf Secrets und sensible Muster,
- dokumentierte Datenowner.

## 9. Anti-Patterns

- „verschlüsselt = anonym“.
- „gehasht = anonym“.
- vollständige Request-/Response-Bodies standardmäßig loggen.
- Produktionsdaten als bequemste Testfixture.
- Personendaten in Events replizieren, weil sie „später vielleicht gebraucht werden“.
- PII-Erkennung als alleinige Privacy-Governance für LLMs.

## 10. Quellen

- Verordnung (EU) 2016/679 (DSGVO), insbesondere Art. 4 und Art. 25  
  https://eur-lex.europa.eu/eli/reg/2016/679/oj
- OWASP Logging Cheat Sheet  
  https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html
- AK-035 — Privacy by Design Principle
- AK-102 — Observability Reference Architecture

## 11. Coach-Merksatz

> Datenschutz wird architektonisch greifbar, wenn du für jede relevante Information beantworten kannst: **Warum haben wir sie, wer besitzt sie, wohin fließt sie, welche Kopien entstehen und wie endet ihr Lebenszyklus?**
