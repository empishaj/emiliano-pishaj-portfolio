---
id: AK-081
legacy_ids:
  - ADR-081
title: Architektur verstehen – Struktur, Entscheidungen, Concerns und Kommunikation
artifact_type: learning-guide
domain: architecture-fundamentals
status: active
maturity: reviewed
normative_level: informative
last_validated: 2026-09-28
review_trigger:
  - Änderung der zugrunde gelegten Architekturstandards
---

# AK-081 — Was Architektur wirklich ist

## 1. Ziel

Dieses Kapitel ist kein ADR. Es gibt keine konkrete Entscheidung zu treffen.

Es schafft die gemeinsame Sprache, die du brauchst, bevor du über APIs, Microservices, Cloud, Daten oder Plattformen entscheidest.

Die wichtigste Verschiebung lautet:

> Architektur ist weder Technologieliste noch Diagramm.  
> Architektur ist die Struktur eines Systems oder einer Organisation **und die tragenden Entscheidungen und Prinzipien**, durch die diese Struktur entsteht und sich verändert.

ISO/IEC/IEEE 42010:2022 trennt ausdrücklich zwischen der Architektur eines betrachteten Gegenstands und einer **Architecture Description**, die diese Architektur ausdrückt. Ein Diagramm ist damit eine Beschreibung oder Sicht – nicht die Architektur selbst.

## 2. Vier Fragen eines Architekten

Ein belastbares Arbeitsmodell lautet:

```text
KLÄREN
→ ENTWERFEN
→ BEWERTEN
→ KOMMUNIZIEREN
↺
```

### Klären

- Was ist der Auftrag?
- Welche Stakeholder existieren?
- Welche Concerns sind relevant?
- Welche Qualitätsziele gelten?
- Welche Constraints sind real?
- Was ist Scope und Nicht-Scope?

### Entwerfen

- Welche Verantwortungsgrenzen brauchen wir?
- Welche Daten gehören wohin?
- Welche Abhängigkeiten sind unvermeidbar?
- Welche Integrationsform passt?
- Welche Standards und Muster helfen?

### Bewerten

- Unterstützt die Architektur die Qualitätsziele?
- Wo liegen Sensitivitätspunkte?
- Welche Trade-offs akzeptieren wir?
- Welche Risiken kennen wir nicht?
- Welche Evidence fehlt?

### Kommunizieren

- Welche Sicht braucht welcher Stakeholder?
- Welche Entscheidung muss verstanden werden?
- Welche Information ist für Umsetzung oder Betrieb handlungsrelevant?
- Welche Details würden nur ablenken?

## 3. Architektur beginnt mit Concerns

Ein Stakeholder ist nicht nur „der Kunde“.

In einer komplexen Organisation können relevante Stakeholder sein:

- Fachbereich,
- IT,
- Betrieb,
- Informationssicherheit,
- Datenschutz,
- Plattformteam,
- Management,
- Einkauf/Vergabe,
- externe Dienstleister,
- andere Behörden oder Organisationen.

Ein Concern kann technisch, fachlich, organisatorisch, wirtschaftlich, regulatorisch oder betrieblich sein.

Architekturarbeit beginnt deshalb nicht mit:

> „Welche Technologie nehmen wir?“

sondern:

> „Welche Concerns müssen in diesem Kontext tragfähig beantwortet werden?“

## 4. Was macht eine Entscheidung architektonisch?

Nicht jede technische Wahl ist eine Architekturentscheidung.

Architekturrelevant wird eine Entscheidung typischerweise, wenn mehrere Punkte zutreffen:

- hohe Änderungskosten,
- großer Wirkungsradius,
- Einfluss auf Qualitätsziele,
- Einfluss auf andere Entscheidungen,
- organisations- oder systemübergreifende Wirkung,
- langfristige Abhängigkeit,
- Risiko oder Governance-Relevanz.

Beispiele:

- führendes System für Personendaten,
- synchron vs. asynchron bei einer kritischen Integration,
- zentrales IAM vs. lokale Identitäten,
- Modularer Monolith vs. verteilte Services,
- Cloud-/Plattformmodell,
- RPO/RTO,
- API-Lifecycle.

Eine lokale Hilfsmethode ist dagegen normalerweise kein ADR.

## 5. Architektur existiert auf mehreren Ebenen

```text
Enterprise
  Auftrag, Capability, Organisation, Governance

Business
  Leistung, Prozess, Entscheidung, Verantwortung

Information
  Begriffe, Datenobjekte, Ownership, Lebenszyklus

Application
  Anwendungen, Module, Services, Schnittstellen

Technology
  Plattform, Runtime, Netzwerk, Infrastruktur

Operations
  Deployment, Observability, Recovery, Support

Transformation
  Ist → Transition States → Ziel
```

Ein Enterprise Architect muss nicht in jeder Ebene der tiefste Spezialist sein.

Er muss aber erkennen können:

- welche Entscheidung auf welcher Ebene liegt,
- welche Ebenen sich gegenseitig beeinflussen,
- wann Spezialisten beteiligt werden müssen,
- wie die Ergebnisse zu einem konsistenten Zielbild werden.

## 6. Architektur und Organisation

Technische Grenzen und Verantwortungsgrenzen sind nicht unabhängig.

Wenn drei Teams gemeinsam dieselbe Datenbank und dieselben Module verändern, hilft ein Diagramm mit „sauberen Services“ wenig.

Fragen:

- Wer besitzt die fachliche Verantwortung?
- Wer darf Änderungen freigeben?
- Wer betreibt das Ergebnis?
- Wer trägt Incident-Verantwortung?
- Wo liegt Datenhoheit?
- Wer entscheidet über Ausnahmen?

Damit wird Architektur zur **Verantwortungsarchitektur**.

## 7. Architektur und Qualität

„Gute Architektur“ ist kein universelles Stilurteil.

Eine Architektur ist tragfähig, wenn sie:

- die relevanten Qualitätsziele unterstützt,
- Constraints respektiert,
- bekannte Trade-offs bewusst akzeptiert,
- Risiken sichtbar macht,
- veränderbar genug für ihren Kontext bleibt.

Deshalb kommt vor der Pattern-Diskussion das Qualitätsszenario.

## 8. Architektur und Evidence

Eine Architekturbehauptung ist stärker, wenn sie überprüfbar ist.

```text
Behauptung:
"Die Module sind entkoppelt."

schwach:
Diagramm sagt es.

stärker:
Dependency-Analyse zeigt es.

noch stärker:
Fitness Function blockiert verbotene Abhängigkeiten.
```

Evidence kann sein:

- Architekturtest,
- Lasttest,
- SLO,
- Restore-Test,
- Security Scan,
- Contract Test,
- Deployment-Metrik,
- Abnahmeprotokoll.

## 9. Architekturkommunikation

Eine Sicht beantwortet eine Stakeholderfrage.

### Management

Braucht:

- Entscheidung,
- Nutzen,
- Risiko,
- Kosten,
- Abhängigkeit,
- nächste Entscheidung.

### Entwicklung

Braucht:

- Grenzen,
- Verträge,
- Standards,
- Rationale,
- Beispiele.

### Betrieb

Braucht:

- Deployments,
- Abhängigkeiten,
- SLOs,
- Failure Modes,
- Runbooks,
- Recovery.

### Fachseite

Braucht:

- Fähigkeiten,
- Prozesse,
- Informationen,
- Verantwortungen,
- Auswirkungen auf Leistungen.

Das gleiche System wird also unterschiedlich beschrieben.

## 10. Architektur ist kontinuierlich

Architektur endet nicht nach dem Zielbild.

```text
Entscheidung
→ Umsetzung
→ Messung
→ Betrieb
→ Lernen
→ neue Randbedingung
→ Neubewertung
```

Das ist kein Zeichen schlechter Planung. Es ist die normale Evolution komplexer Systeme.

## 11. Quellen

- ISO/IEC/IEEE 42010:2022  
  https://www.iso.org/standard/74393.html
- arc42  
  https://docs.arc42.org/
- C4 model  
  https://c4model.com/
- Michael Nygard, Architecture Decision Records  
  https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions

## 12. Coach-Merksatz

> Ein Senior-Architekt erkennt nicht daran, dass er viele Lösungen kennt.  
> Man erkennt ihn daran, dass er **die richtige Ebene, die richtige Frage, den relevanten Trade-off und die passende Evidence** findet.
