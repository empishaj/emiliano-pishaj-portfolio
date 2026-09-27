---
id: AK-093
legacy_ids:
  - ADR-093
title: Living Documentation und Architecture Traceability
artifact_type: documentation-standard
domain: architecture-governance
status: active
maturity: reviewed
normative_level: recommended
owner_role: Enterprise Architecture
last_validated: 2026-09-28
review_trigger:
  - wiederkehrende Abweichung zwischen Dokumentation und Realität
  - Einführung neuer autoritativer Informationsquellen
  - wesentliche Änderung von Repository-, Modellierungs- oder Telemetrieplattformen
---

# AK-093 — Living Documentation und Architecture Traceability

## 1. Kernidee

Living Documentation bedeutet nicht:

> „Wir generieren alles aus dem Code, dann kann Dokumentation nicht veralten.“

Das wäre zu stark.

Eine Dokumentation bleibt lebendig, wenn für relevante Informationen geklärt ist:

- **woher** sie stammen,
- **wer** sie verantwortet,
- **wie** sie aktualisiert werden,
- **woran** Drift erkannt wird,
- **wann** eine Neubewertung ausgelöst wird.

Automatisierung hilft besonders bei Fakten, die bereits maschinenlesbar vorliegen. Sie ersetzt aber nicht fachliche, organisatorische und strategische Architekturinformation.

## 2. Vier Klassen von Architekturinformation

### Klasse A — direkt generierbare technische Fakten

Beispiele:

- OpenAPI-Endpunkte,
- AsyncAPI-/Event-Schemas,
- Build-Abhängigkeiten,
- Container Images,
- Terraform-Ressourcen,
- Kubernetes Desired State,
- Modulabhängigkeiten.

Diese Informationen SOLLTEN möglichst aus ihrer autoritativen technischen Quelle erzeugt oder verlinkt werden.

### Klasse B — Runtime-abgeleitete Fakten

Beispiele:

- tatsächlich beobachtete Service-Abhängigkeiten,
- Latenz,
- Error Rate,
- Deployment-Version,
- Event-Lag,
- Ressourcennutzung.

Quelle ist nicht das Architekturdiagramm, sondern Telemetrie beziehungsweise Runtime-Inventar.

### Klasse C — bewusst verantwortete Architekturinformation

Beispiele:

- Architekturentscheidungen,
- Qualitätsziele,
- Standards,
- Ausnahmen,
- Risiken,
- Zielbilder,
- Transition Architectures.

Diese Informationen entstehen durch Entscheidungs- und Governance-Arbeit. Sie können versioniert werden, aber nicht aus Sourcecode „entdeckt“ werden.

### Klasse D — Business- und Organisationsinformation

Beispiele:

- Capabilities,
- Verwaltungsleistungen,
- Prozessverantwortung,
- Data Owner,
- organisatorische Zuständigkeiten,
- gesetzliche Randbedingungen,
- Portfolioentscheidungen.

Diese Informationen benötigen verantwortete fachliche Quellen.

## 3. Source-of-Truth-Matrix

Für jede wichtige Informationsklasse sollte die führende Quelle bekannt sein.

| Information | Autoritative Quelle | abgeleitete Darstellung |
|---|---|---|
| REST-Vertrag | OpenAPI im verantworteten Repository | API-Katalog, Dokumentation |
| Event-Vertrag | AsyncAPI + Schema Registry / Schema-Datei | Event-Katalog |
| gewünschter Kubernetes-Zustand | GitOps-Repository | Plattform-/Deployment-Sicht |
| tatsächlich laufende Version | Runtime / Deployment-Metadaten | Dashboard |
| Service-Abhängigkeiten | Contracts + Runtime-Telemetrie | Abhängigkeitsgraph |
| Architekturentscheidung | ADR-Repository | Decision Log / Zielarchitektur |
| Architekturstandard | Standards-Repository | Review-Checkliste / Quality Gate |
| Capability | EA-Repository / fachlich verantwortete Quelle | Capability Map |
| Data Owner | Data-Governance-Repository | Datenlandkarte |
| SLO | Service-Governance-Quelle | Dashboard / Alerting |
| Incident Learning | Post-Mortem-System | Trend- und Maßnahmenübersicht |

Eine Information darf mehrere Darstellungen haben, aber möglichst nur **eine führende Quelle**.

## 4. Traceability statt Dokumentkopien

Wissen wird robuster, wenn Beziehungen explizit werden.

```text
Stakeholder Concern
        ↓
Architecture Requirement / Quality Scenario
        ↓
Decision
        ↓
Standard / Reference Architecture
        ↓
Implementation
        ↓
Control / Test
        ↓
Runtime Evidence
```

Beispiel:

```text
Concern:
Registerabfragen müssen nachvollziehbar sein

→ Qualitäts-/Security-Anforderung
→ ADR zur Korrelationsstrategie
→ Logging-Standard
→ Correlation-ID in Schnittstellenvertrag
→ Integrationstest
→ Trace-/Log-Evidence
```

Traceability bedeutet nicht, jede Codezeile mit einem ADR zu annotieren.

Die Verlinkung wird dort hergestellt, wo sie Entscheidungs-, Review- oder Auditnutzen besitzt.

## 5. Aktualitätsmetadaten

Ein wichtiges Knowledge-Item SOLLTE mindestens besitzen:

```yaml
owner_role: ""
last_validated: YYYY-MM-DD
review_trigger: []
status: active
sources: []
```

Bei technologieabhängigen Dokumenten zusätzlich:

```yaml
technology_baseline:
  product: version
```

Damit wird sichtbar, ob eine Aussage grundsätzlich stabil oder an einen technischen Stand gekoppelt ist.

## 6. Automatisierungspyramide

Nicht alles sollte automatisch generiert werden.

```text
                bewusst kuratiert
        Ziele / Entscheidungen / Risiken
              /                \
       Modelle                  Policies
          /                        \
   generierte technische Fakten   Runtime-Evidence
```

Je näher eine Information an einem maschinenlesbaren technischen Artefakt liegt, desto eher ist Automatisierung sinnvoll.

Je stärker sie Motivation, Verantwortung, Risiko oder Zielzustand beschreibt, desto stärker braucht sie bewusste Governance.

## 7. Living Documentation für Enterprise Architecture

Enterprise Architecture benötigt zusätzlich zu Softwareinformationen insbesondere:

- Capability Maps,
- Organisations-/Verantwortungssichten,
- Datenlandkarten,
- Applikationsportfolio,
- Integrationslandkarte,
- Technologieportfolio,
- Standards und Ausnahmen,
- Risiken,
- Zielbilder,
- Transition Architectures,
- Roadmaps.

Diese Informationen können teilweise aus CMDBs, API-Katalogen, Cloud Inventories oder Repositories gespeist werden.

Aber ein technisches Inventory kann nicht automatisch beantworten:

> Welche Capability ist strategisch kritisch?

oder:

> Welche Behörde besitzt die fachliche Datenverantwortung?

## 8. Drift erkennen

Typische Driftsignale:

- OpenAPI unterscheidet sich von produktivem Verhalten,
- Architekturdiagramm zeigt Komponenten, die nicht mehr existieren,
- Servicekatalog hat keinen Owner,
- ADR verweist auf einen nicht mehr gültigen Standard,
- Datenlandkarte und tatsächliche Schnittstellen widersprechen sich,
- GitOps-Desired-State und Clusterzustand weichen dauerhaft ab,
- SLO-Dokument und Alerting verwenden unterschiedliche Grenzwerte,
- Reviewdatum ist überschritten und niemand reagiert.

Drift ist nicht primär ein Dokumentationsproblem.

Sie zeigt, dass **Ownership oder Feedbackmechanismen** fehlen.

## 9. CI-/Automation-Beispiele

Geeignete automatisierte Checks können sein:

- OpenAPI-/AsyncAPI-Linting,
- Schema Compatibility,
- Link-Checker,
- verwaiste ADR-Referenzen,
- fehlende Owner-Metadaten,
- Review-Due-Check,
- Architecture Fitness Functions,
- Policy-as-Code,
- GitOps Drift Detection,
- SBOM-/Dependency-Inventar.

Diese Checks zeigen formale oder technische Drift. Sie bewerten nicht automatisch fachliche Richtigkeit.

## 10. Anti-Patterns

### „Code ist die einzige Wahrheit“

Code kennt nicht automatisch Auftrag, Capability, Governance, Risiko oder Organisationsverantwortung.

### Dokumentation doppelt pflegen

Wenn OpenAPI bereits führend ist, sollte eine zweite manuell gepflegte Endpunktliste vermieden werden.

### Automatisch generiert = korrekt

Automatisierung kann falsche Metadaten sehr zuverlässig vervielfältigen.

### Wiki ohne Ownership

Ein Wiki ist nur ein Speicherort. Es ist kein Governance-Modell.

### Alles an den Code annotieren

Traceability muss Nutzen haben. Zu viele Custom-Annotations koppeln Business-/Governance-Information unnötig an Implementierungsdetails.

## 11. Review-Fragen

1. Welche Information soll aktuell gehalten werden?
2. Wo entsteht sie ursprünglich?
3. Wer ist Owner?
4. Gibt es doppelte Quellen?
5. Kann sie automatisch abgeleitet werden?
6. Wie erkennen wir Drift?
7. Wer reagiert auf Drift?
8. Welcher Review-Trigger gilt?
9. Welche Stakeholderentscheidung hängt von der Information ab?

## 12. Quellen

- ISO/IEC/IEEE 42010:2022  
  https://www.iso.org/standard/74393.html
- Cyrille Martraire, *Living Documentation* — konzeptionelle Grundlage
- OpenGitOps Principles  
  https://opengitops.dev/
- OpenTelemetry  
  https://opentelemetry.io/docs/what-is-opentelemetry/
- arc42  
  https://docs.arc42.org/

## 13. Coach-Merksatz

> Living Documentation bedeutet nicht, alles zu generieren.  
> Sie bedeutet, dass für wichtige Architekturinformationen **Quelle, Owner, Aktualisierungsmechanismus und Drift-Erkennung** geklärt sind.
