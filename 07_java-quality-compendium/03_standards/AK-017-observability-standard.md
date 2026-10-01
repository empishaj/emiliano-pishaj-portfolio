---
id: AK-017
legacy_ids:
  - QG-JAVA-017
title: Observability Mindeststandard
artifact_type: architecture-standard
domain: operations
status: active
maturity: reviewed
normative_level: normative
owner_role: Operations / Platform Architecture
last_validated: 2026-10-01
review_trigger:
  - Änderung der Telemetrieplattform
  - wiederkehrende Diagnoseprobleme
  - Datenschutzbefund in Telemetrie
---

# AK-017 — Observability Mindeststandard

## 1. Zweck

Monitoring beantwortet bekannte Fragen. Observability unterstützt zusätzlich die Analyse unbekannter Fehlerzustände durch geeignete Telemetrie und Korrelation.

Der Standard fordert nicht „mehr Logs“, sondern **diagnostisch nutzbare und datenschutzbewusste Telemetrie**.

## 2. Signale

### Logs

Diskrete Ereignisse mit Kontext.

### Metrics

Aggregierte Zeitreihen für Verhalten und SLOs.

### Traces

Zusammenhängende Ausführung über Prozess-/Netzwerkgrenzen.

### Events

Fachlich oder betrieblich bedeutsame punktuelle Ereignisse.

Die Signale ergänzen sich.

## 3. Mindestanforderungen pro Service

SOLLTE mindestens sichtbar machen:

- Service-/Runtime-Identität,
- Umgebung,
- Request-/Workload-Rate,
- Fehler,
- Latenz,
- relevante Ressourcensättigung,
- kritische Downstream-Abhängigkeiten,
- Version/Build,
- Health/Readiness,
- Trace-/Correlation Context für verteilte Abläufe.

Die konkrete Metrikliste folgt Systemtyp und SLOs.

## 4. Strukturierte Logs

Logs SOLLEN strukturiert und maschinenlesbar sein.

Typische Felder:

- Zeitstempel,
- Severity,
- Service,
- Umgebung,
- technische Operation,
- Trace-/Correlation-ID,
- stabiler Fehlercode,
- Ergebnisstatus.

Personenbezogene Inhalte werden minimiert; Tokens und Secrets dürfen nicht geloggt werden.

## 5. Korrelation

Ein Request oder fachlicher Vorgang sollte dort korrelierbar sein, wo dies für Betrieb und Support erforderlich und datenschutzrechtlich zulässig ist.

Technische Trace-ID und fachliche Vorgangsreferenz sind unterschiedliche Konzepte und dürfen nicht unreflektiert gleichgesetzt werden.

## 6. Metrics

Metriken SOLLEN Entscheidungen unterstützen.

Beispiele:

- SLI für Verfügbarkeit/Erfolg,
- Latenzverteilung,
- Queue Lag,
- DB Pool Saturation,
- Retry-/Circuit-Breaker-Zustände.

High-cardinality Labels müssen kontrolliert werden. Personen- oder Request-IDs gehören typischerweise nicht als Metric Label.

## 7. Tracing

Tracing SOLL bei verteilten kritischen Pfaden Kontextpropagation ermöglichen.

Prüfen:

- Eingang und Ausgang,
- HTTP/RPC,
- Messaging,
- Datenbank-/externe Aufrufe,
- Samplingstrategie.

Nicht jede interne Methode braucht einen Span.

## 8. Dashboards nach Rolle

### Betrieb

Fehler, SLOs, Saturation, Abhängigkeiten.

### Entwicklung

Traces, technische Detailmetriken.

### Service Owner

fachliche Durchsätze und Servicequalität.

### Management

Verfügbarkeit/Kritikalität/Trend auf verständlicher Ebene.

Ein Executive Dashboard zeigt nicht primär JVM Heap.

## 9. Alerting

Ein Alert muss eine Handlung auslösen können.

Schlecht:

> CPU > 70 % einmalig.

Stärker:

> SLO-relevante Fehler-/Latenzbedingung über sinnvolles Zeitfenster, mit Runbook und Owner.

## 10. Datenschutz

Telemetrie ist ein eigener Datenbestand.

MUSS/SOLLTE:

- Zweck und Retention klären,
- PII minimieren/redacten,
- Zugriffe kontrollieren,
- Export/Weiterleitung kennen.

## 11. Evidence

Observability selbst muss abnahmefähig sein.

Beispiele:

- Trace folgt kritischem End-to-End-Pfad,
- relevante SLI-Metrik vorhanden,
- Alert löst in Test aus,
- Runbook verlinkt,
- PII-Test für Logging.

## 12. Quellen

- OpenTelemetry  
  https://opentelemetry.io/docs/what-is-opentelemetry/
- OpenTelemetry Semantic Conventions  
  https://opentelemetry.io/docs/specs/semconv/
- Google SRE — SLI/SLO
- AK-102 — OpenTelemetry Reference Architecture

## 13. Merksatz

> Observability ist Betriebsarchitektur: Wir definieren vor dem Incident, **welche Signale uns erlauben, Zustand, Ursache und Auswirkung eines Systems nachvollziehbar zu erkennen**.
