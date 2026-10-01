---
id: AK-077
legacy_ids:
  - ADR-077
title: Modularer Monolith oder Microservices?
artifact_type: decision-guide
domain: application-architecture
status: active
maturity: reviewed
normative_level: informative
last_validated: 2026-10-01
review_trigger:
  - wesentliche Skalierungs-, Team- oder Deploymentänderung
---

# AK-077 — Modulith vs. Microservices

## 1. Die falsche Frage

> „Sind Microservices moderner als ein Monolith?“

Die richtige Frage:

> **Welcher konkrete Treiber rechtfertigt die zusätzlichen Kosten verteilter Systemgrenzen?**

## 2. Kosten verteilter Services

Microservices bringen mögliche Vorteile, aber auch neue Pflichten:

- Netzwerkfehler,
- Timeouts/Retry,
- verteilte Observability,
- API-/Event-Governance,
- Datenownership,
- Eventual Consistency,
- unabhängige Deployments,
- Plattformautomation,
- On-Call/Betrieb je Servicefamilie.

Diese Kosten existieren unabhängig davon, ob das Framework die Serviceerstellung einfach macht.

## 3. Reale Extraktionstreiber

Eine Servicegrenze ist eher gerechtfertigt, wenn mehrere der folgenden Treiber bestehen:

### unabhängige Änderung

Ein fachlicher Bereich muss deutlich häufiger oder unabhängig deployen.

### unabhängige Skalierung

Lastprofil unterscheidet sich signifikant und gemeinsame Skalierung ist teuer/problematisch.

### starke Isolation

Security, Compliance, Daten- oder Failure-Isolation erfordern eine eigene Betriebsgrenze.

### klare Ownership

Ein Team kann die Fähigkeit Ende-zu-Ende besitzen und betreiben.

### Technologiebedarf

Ein abgegrenzter Bereich besitzt einen echten, nicht bloß modischen Technologiezwang.

### organisatorische Grenze

System- und Teamgrenze können bewusst aufeinander abgestimmt werden.

Keiner dieser Punkte allein ist automatische Freigabe.

## 4. Signale gegen Microservices

- Domänengrenzen sind unklar.
- Teams teilen dieselben Tabellen.
- Releases müssen ohnehin gemeinsam erfolgen.
- CI/CD und Observability sind unreif.
- Team kann Betrieb nicht Ende-zu-Ende übernehmen.
- viele synchrone Cross-Service-Transaktionen wären nötig.
- vermeintlicher Treiber lautet nur „bessere Wartbarkeit“.

In solchen Fällen kann ein Modulith die bessere Lern- und Strukturierungsphase sein.

## 5. Vergleich

| Dimension | Modulith | Microservices |
|---|---|---|
| Deployment | gemeinsam | potenziell unabhängig |
| Netzwerkfehler intern | gering | inhärent |
| lokale Transaktion | einfacher | über Grenzen schwierig |
| Skalierung | gemeinsam | je Service möglich |
| Plattformbedarf | geringer | höher |
| Ownership | Module möglich | Services idealerweise klar |
| Governance | Modulregeln | Verträge + Runtime + Daten |
| Debugging | häufig einfacher | verteilte Diagnose nötig |

Die Tabelle ist keine Wertung. Die passende Option hängt vom Kontext ab.

## 6. Die Extraction Test Question

Vor einer Service-Extraktion sollte das Team mindestens beantworten können:

1. Welches Problem löst die Extraktion?
2. Welche Daten gehören dem Service?
3. Welche Konsistenzgrenzen entstehen?
4. Welche Consumer existieren?
5. Was passiert bei Nichtverfügbarkeit?
6. Wer betreibt den Service?
7. Wie wird er beobachtet?
8. Wie wird unabhängig deployt?
9. Welche neue Komplexität entsteht?
10. Welche Evidence zeigt später, dass der Treiber tatsächlich erfüllt wurde?

## 7. Organisation und Conway

Ein Service ohne passende Ownership kann technisch getrennt und organisatorisch dennoch eng gekoppelt sein.

Wenn zwei Services bei jeder Änderung zwei Teams und ein gemeinsames Releasemeeting benötigen, ist die gewünschte Autonomie nicht erreicht.

Siehe AK-085.

## 8. Migration

Nicht:

```text
Monolith
→ Big Bang
→ 40 Microservices
```

Besser:

```text
Monolith verstehen
→ Modulgrenzen schaffen
→ Abhängigkeiten messen
→ echte Extraktionskandidaten identifizieren
→ einzeln entscheiden
```

Strangler oder Branch-by-Abstraction können helfen, sind aber ebenfalls kontextabhängig.

## 9. Anti-Patterns

- Service pro Tabelle.
- Service pro Entwicklerteam ohne Domänenlogik.
- Shared Database mit „Microservices“ als Fassade.
- Microservices als Ersatz für schlechte Modulstruktur.
- künstliche Servicegrenzen, obwohl alles synchron und transaktional bleiben muss.

## 10. Von diesem Guide zum ADR

Ein echtes ADR formuliert zum Beispiel:

> „Soll das Dokumentenmodul des Fachverfahrens als eigener Service extrahiert werden?“

Dann werden konkrete Treiber, Quality Scenarios und Trade-offs bewertet.

## 11. Quellen

- Sam Newman — *Building Microservices*
- Team Topologies
- AK-056 — Modularer Monolith
- AK-085 — Sozio-technische Architektur

## 12. Merksatz

> Eine Servicegrenze ist keine Belohnung für „saubere Architektur“. Sie ist eine **teure Betriebs- und Ownership-Grenze, die einen nachweisbaren Nutzen besitzen muss**.
