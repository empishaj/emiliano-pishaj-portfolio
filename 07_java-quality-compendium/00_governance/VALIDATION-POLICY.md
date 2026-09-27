# Validierungs- und Quellenpolicy

## 1. Ziel

Jede Aussage im Compendium wird danach behandelt, **welche Art von Wahrheit sie beansprucht**. Ein zeitloses Architekturprinzip braucht andere Evidenz als eine Framework-API, ein Security-Control oder ein Performance-Schwellenwert.

Das Ziel ist nicht, jede Seite mit Links zu überladen. Das Ziel ist, falsche Sicherheit zu vermeiden.

## 2. Quellenhierarchie

### Tier 1 – normative oder offizielle Primärquelle

Bevorzugt für technische und regulatorische Aussagen:

- ISO / IEC / IEEE
- IETF / RFC
- OWASP
- BSI
- Open Group / TOGAF
- OpenJDK / JEP / Oracle Java Dokumentation
- Spring offizielle Dokumentation
- Kubernetes offizielle Dokumentation
- PostgreSQL offizielle Dokumentation
- OpenTelemetry
- OpenGitOps
- Argo CD
- CNCF-Projekt-Dokumentation

### Tier 2 – anerkannte Fachliteratur

Für Modelle, Trade-offs und langlebige Architekturkonzepte:

- Martin Kleppmann
- Google SRE
- Team Topologies
- Accelerate
- arc42
- iSAQB-Literatur
- Domain-Driven Design
- Building Evolutionary Architectures

### Tier 3 – Erfahrungswerte und Community-Praktiken

Blogs, Talks oder persönliche Heuristiken dürfen verwendet werden, müssen aber als **Praxisheuristik** gekennzeichnet werden. Sie dürfen keine normative Aussage vortäuschen.

## 3. Vier Validierungsstufen

### A – konzeptionell stabil

Das Kernkonzept ist langlebig und nicht stark versionsabhängig.

Beispiele:

- Kohäsion und Kopplung,
- Bounded Context,
- ADR-Lifecycle,
- Blameless Post-Mortem,
- Expand/Contract.

### B – fachlich tragfähig, Baseline aktualisieren

Das Konzept stimmt, konkrete APIs, Toolversionen oder Konfigurationen können veralten.

Beispiele:

- Spring Boot Test Slices,
- Kubernetes YAML,
- ArgoCD,
- OpenTelemetry SDK,
- Gradle,
- Testcontainers.

### C – substanziell korrigieren

Mindestens eine zentrale Aussage ist zu absolut, ungenau, veraltet oder vermischt mehrere Ebenen.

Beispiele aus der aktuellen Sammlung:

- feste Row-/Write-Schwellen für Partitionierung,
- Read Replicas als „einfachste Form von CQRS“,
- CORS als CSRF-Schutz,
- pauschales „JWT im Authorization Header ist CSRF-immun“,
- GraphQL Federation ab einer festen Teamzahl,
- Spring AI 1.0 GA mit falschem Veröffentlichungsdatum und alten Starter-IDs,
- unbelegte konkrete Verfügbarkeitsbehauptungen in Post-Mortems.

### D – Quelle fehlt

Das Thema wird nicht aus Querverweisen rekonstruiert. Es bleibt reserviert, bis die Originalquelle vorhanden ist.

## 4. Regeln für Zahlen

Ein numerischer Wert darf nur normativ werden, wenn mindestens eine Bedingung erfüllt ist:

1. Er stammt aus einer expliziten fachlichen oder regulatorischen Anforderung.
2. Er ist ein organisationsweit beschlossener Standard.
3. Er wurde durch Messung und Kapazitätsplanung abgeleitet.
4. Er stammt aus einer Primärquelle und ist im konkreten Kontext anwendbar.

Nicht zulässig als allgemeiner Standard:

```text
"Ab 100 Mio. Rows partitionieren."
"Replica-Lag muss <100 ms sein."
"Federation ab drei Teams."
"Mutation Score immer >80 %."
"20 % Sprint-Kapazität für Tech Debt."
```

Zulässig:

```text
"Partitionierung wird anhand Tabellen-/Indexgröße, Zugriffsmuster,
Retention, Memory-Verhältnis und gemessenem Query-/Maintenance-Verhalten bewertet."

"Der maximal tolerierbare Replica-Lag wird aus dem fachlichen
Konsistenzbedarf der jeweiligen Read-Funktion abgeleitet."
```

## 5. Regeln für Technologieversionen

Versionen stehen in einer **Technology Baseline**, nicht in der ewigen Architekturregel.

Beispiel:

```yaml
last_validated: 2026-09-28
technology_baseline:
  spring_ai: "2.0.1"
  java: "21/25 geprüft"
```

Wenn die Major-Version wechselt, wird das Dokument `review-due`.

## 6. Sicherheitsvalidierung

Security-Dokumente werden mindestens gegen eine aktuelle Primärquelle geprüft.

Beispiele:

- OAuth2/OIDC: IETF / OpenID Foundation / Spring Security.
- Web Security: OWASP / MDN.
- Container/Kubernetes: Kubernetes / CNCF / BSI soweit relevant.
- IAM im Behördenkontext: BSI plus jeweilige Organisationsvorgaben.
- Datenschutz: rechtliche Grundlage und technische Schutzmaßnahmen werden getrennt dokumentiert.

Security-Beispiele dürfen niemals suggerieren, ein einzelner Mechanismus mache ein System „sicher“.

## 7. Beispielhafte Korrekturen aus der aktuellen Prüfung

### PostgreSQL Partitionierung

Die PostgreSQL-Dokumentation nennt **keine universelle Row-Schwelle**. Der Nutzen hängt vom Workload ab; als grobe Heuristik nennt PostgreSQL unter anderem die Relation zur physischen Speicherkapazität des Datenbankservers. Partitionierungsstrategie und Partitionsanzahl müssen anhand von Query- und Retention-Mustern gewählt werden.

Quelle:
https://www.postgresql.org/docs/17/ddl-partitioning.html

### Read Replicas

Hot Standby kann verzögerte Daten liefern. Der fachliche Konsistenzbedarf muss daher explizit bestimmt werden. Replikation ist eine physische Skalierungs-/HA-Technik und wird nicht mit CQRS gleichgesetzt.

Quelle:
https://www.postgresql.org/docs/17/hot-standby.html

### CORS und CSRF

CORS ist keine allgemeine CSRF-Abwehr. OWASP weist ausdrücklich darauf hin, dass übliche CSRF-Schutzmaßnahmen weiterhin notwendig sind. Der Authentisierungsmechanismus und die Browser-Credential-Semantik bestimmen die konkrete CSRF-Gefahr.

Quelle:
https://cheatsheetseries.owasp.org/cheatsheets/HTML5_Security_Cheat_Sheet.html

### Spring AI

Spring AI 1.0 GA wurde am 20. Mai 2025 veröffentlicht, nicht im März 2024. Die Starter-Namen wurden vor GA geändert. Aktuelle Dokumentation muss beim Rewrite erneut geprüft werden.

Quellen:
https://spring.io/blog/2025/05/20/spring-ai-1-0-GA-released
https://docs.spring.io/spring-ai/reference/upgrade-notes.html

### ArgoCD / GitOps

OpenGitOps definiert GitOps über deklarativen, versionierten/immutable Desired State, automatisches Pulling und kontinuierliche Reconciliation. ArgoCD `selfHeal` ist eine konkrete Produktfunktion und muss produktversionsbezogen dokumentiert werden.

Quellen:
https://opengitops.dev/
https://argo-cd.readthedocs.io/en/stable/user-guide/auto_sync/

### Incident Management

Post-Mortems reduzieren die Wahrscheinlichkeit wiederkehrender Fehler, garantieren aber nicht, dass „dieselbe Ursache nie wieder“ auftritt. Action Items brauchen Owner und Tracking.

Quelle:
https://sre.google/workbook/postmortem-culture/

## 8. Review-Trigger

Ein Knowledge-Item wird neu geprüft, wenn mindestens eines eintritt:

- Major-Version einer zentralen Technologie.
- neue Security-/Privacy-Empfehlung.
- relevante Gesetzes-/BSI-/Policy-Änderung.
- Incident widerlegt eine Annahme.
- Fitness Function oder SLO zeigt systematische Abweichung.
- neuer Architekturkontext macht bisherige Trade-offs fragwürdig.
- ein verlinktes Standarddokument wird ersetzt.

## 9. Definition of Validated

Ein Dokument ist `reviewed` oder `active`, wenn:

- seine zentralen Aussagen gegen geeignete Quellen geprüft wurden,
- normative und illustrative Aussagen getrennt sind,
- zeitabhängige Baselines datiert sind,
- Zahlenwerte nachvollziehbar begründet sind,
- widersprechende oder konkurrierende Optionen fair behandelt werden,
- Review-Trigger definiert sind,
- Cross-References auf existierende Artefakte zeigen.

„Ich habe es gelesen“ ist kein Validierungsstatus.
