---
id: AK-102
legacy_ids:
  - ADR-102
title: OpenTelemetry Reference Architecture
artifact_type: reference-architecture
domain: observability
status: active
maturity: reviewed
normative_level: recommended
owner_role: Platform / Operations Architecture
last_validated: 2026-09-28
review_trigger:
  - wesentliche OpenTelemetry-Spec-/Collector-Änderung
  - Wechsel des Telemetrie-Backends
---

# AK-102 — OpenTelemetry Reference Architecture

## 1. Ziel

OpenTelemetry standardisiert Erzeugung, Kontext und Transport von Telemetriesignalen. Es ist **nicht das Observability-Backend** und nicht automatisch ein fertiges Monitoring-System.

Referenzmodell:

```text
Applications / Infrastructure
        ↓
OpenTelemetry Instrumentation / SDK / Agent
        ↓ OTLP
OpenTelemetry Collector
   ├─ receive
   ├─ process / redact / sample
   └─ export
        ↓
Trace / Metric / Log Backends
```

## 2. Warum eine gemeinsame Telemetrieschicht?

Vorteile:

- vendorneutrale Instrumentierung,
- gemeinsame Context Propagation,
- zentrale Processing-/Routing-Schicht,
- Backendwechsel ohne vollständige Neu-Instrumentierung,
- konsistente Semantic Conventions.

Das bedeutet nicht, dass jeder Signaltyp zwingend über denselben Collectorpfad laufen muss.

## 3. Resource Identity

Telemetrie MUSS eindeutig zuordenbar sein.

Typische Resource-Attribute:

- `service.name`,
- Service-Version,
- Environment/Deployment,
- Instanz-/Clusterkontext soweit sinnvoll.

Namen sollen organisationsweit stabil und konsistent sein.

## 4. Context Propagation

Bei verteilten Abläufen SOLL Trace Context über unterstützte Protokollgrenzen propagiert werden.

Zu beachten:

- HTTP/RPC,
- Messaging,
- Async Worker,
- Batch.

Baggage ist mit Vorsicht zu verwenden: Es wird propagiert und darf keine unnötigen sensiblen Informationen transportieren.

## 5. Semantic Conventions

OpenTelemetry Semantic Conventions standardisieren Namen und Bedeutungen für viele Signale.

Zum Validierungszeitpunkt wird eine eigenständige SemConv-Version gepflegt; einzelne Bereiche besitzen unterschiedliche Stability Levels.

Daraus folgt:

> Nicht jede aktuelle SemConv ist gleich stabil. Technology Baselines müssen bei Implementierung erneut geprüft werden.

## 6. Collector-Rollen

Der Collector kann:

- Protokolle empfangen,
- Attribute ergänzen/entfernen,
- PII redigieren,
- Sampling durchführen,
- Daten an mehrere Backends routen,
- Telemetrie puffern/verarbeiten.

Er ist damit ein wichtiger Governance-Punkt.

Aber:

> Sensible Daten erst im Collector zu löschen ist schlechter, als sie bereits in der Anwendung nicht zu erzeugen.

## 7. Sampling

### Head Sampling

Entscheidung früh; geringere Kosten, aber spätere Fehlermerkmale unbekannt.

### Tail Sampling

Entscheidung nach Beobachtung des Traces; kann interessante Fehler-/Latenz-Traces gezielter behalten, benötigt aber Collector-State und Ressourcen.

Samplingstrategie folgt:

- Trafficvolumen,
- Kosten,
- Incident-Anforderungen,
- regulatorischer Retention.

## 8. Logs

OTel Log Data Model kann Logs vereinheitlichen und mit Trace Context korrelieren.

Das bedeutet nicht, dass jede Anwendung ihre bestehende Logging-Library sofort ersetzen muss. Häufig werden bestehende Logs integriert und angereichert.

## 9. Metrics

Metrics sollten SLO- und Betriebsfragen beantworten.

Cardinality muss kontrolliert werden.

High-cardinality fachliche IDs gehören nicht als unkontrollierte Labels in Metrics.

## 10. Backendneutralität realistisch betrachten

OTel reduziert Instrumentierungsbindung, aber Backends unterscheiden sich weiterhin bei:

- Query Language,
- Dashboards,
- Alerting,
- Storage,
- Kosten,
- Trace-/Metric-Korrelation.

„Vendor neutral“ bedeutet nicht „Vendor-Wechsel ohne Aufwand“.

## 11. Security und Privacy

MUSS/SOLLTE:

- Transport absichern,
- Collector-Zugriff kontrollieren,
- sensible Attribute minimieren,
- Retention definieren,
- Backends nach Schutzbedarf absichern.

## 12. Evidence

Ein Referenzservice sollte nachweisen:

- Trace über mindestens eine relevante Systemgrenze,
- korrelierte Logs,
- SLI-taugliche Metrics,
- korrekte Service Identity,
- PII-/Secret-Filter,
- Collector Failure Behavior.

## 13. Quellen

- OpenTelemetry Documentation  
  https://opentelemetry.io/docs/
- Semantic Conventions  
  https://opentelemetry.io/docs/specs/semconv/
- Logs Data Model  
  https://opentelemetry.io/docs/specs/otel/logs/data-model/
- AK-017 — Observability Standard
- AK-106 — Privacy Controls

## 14. Coach-Merksatz

> OpenTelemetry ist die **gemeinsame Sprache und Transportebene der Telemetrie**, nicht die Garantie für gute Observability. Gute Observability beginnt bei den Fragen, die Betrieb und Fachseite beantworten müssen.
