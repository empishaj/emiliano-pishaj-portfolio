---
id: AK-087
legacy_ids:
  - ADR-087
title: Architekturbewertung – Szenarien, Risiken und Trade-offs
artifact_type: operating-model
domain: architecture-governance
status: active
maturity: reviewed
normative_level: recommended
last_validated: 2026-10-01
review_trigger:
  - wesentliche Änderung der Qualitätsziele
  - neue kritische Architekturentscheidung
  - wiederkehrender Incident mit Architekturursache
---

# AK-087 — Architekturbewertung ohne Scheinsicherheit

## 1. Ziel

Eine Architektur ist nicht deshalb gut, weil sie modern, elegant oder von erfahrenen Personen entworfen wurde.

Sie muss gegen die relevanten Qualitätsziele und Risiken **bewertet** werden.

Das Ziel einer Architekturbewertung ist nicht, eine Architektur mit einer Gesamtnote zu versehen. Das Ziel ist, sichtbar zu machen:

- welche Qualitätsszenarien kritisch sind,
- welche Entscheidungen besonders sensitiv sind,
- wo Qualitätsziele miteinander konkurrieren,
- welche Risiken und Annahmen noch offen sind,
- welche Evidence vor einer Freigabe benötigt wird.

## 2. Bewertungsintensität nach Risiko

Nicht jedes Thema braucht ATAM.

### Level 0 — lokaler Check

Für kleine reversible Änderungen.

Fragen:

- Welche Qualität kann schlechter werden?
- Gibt es einen einfacheren Weg?
- Wird ein bestehender Standard verletzt?

### Level 1 — Peer Architecture Review

Für team- oder serviceweite Entscheidungen.

Artefakte:

- Entscheidungsfrage,
- relevante Qualitätsszenarien,
- Optionen,
- Trade-offs,
- offene Risiken.

### Level 2 — strukturierter Risk Workshop

Für systemübergreifende oder schwer reversible Änderungen.

Zusätzlich:

- Stakeholder-Concerns,
- Architecture Views,
- Abhängigkeits- und Failure-Analyse,
- technische Spikes,
- Risiko- und Maßnahmenlog.

### Level 3 — formale Architecture Evaluation

Für strategische, hochkritische oder programmweite Entscheidungen.

ATAM ist eine mögliche Methode. Das SEI beschreibt ATAM als strukturierte Bewertung einer Architektur gegen konkurrierende Quality Attributes. Ergebnisse umfassen unter anderem Risiken, Sensitivitätspunkte, Trade-off-Punkte, Szenarien und Risikothemen.

Eine vollständige ATAM-Evaluation ist ein substanzieller Workshop mit Stakeholdern; sie ist kein zweistündiges Reviewetikett.

## 3. Bewertung beginnt mit Qualitätszielen

```text
Business Driver
→ Quality Scenario
→ Architecture Approach
→ Analysis
→ Risk / Sensitivity / Trade-off
→ Decision / Mitigation
```

Ohne priorisierte Szenarien wird Bewertung schnell zu Geschmacksdiskussion.

## 4. Sensitivitätspunkt

Ein Sensitivitätspunkt ist eine Architekturentscheidung oder ein Parameter, dessen Änderung eine Qualität stark beeinflusst.

Beispiele:

- Timeout eines externen Aufrufs,
- Partitionsschlüssel,
- Cache-TTL,
- Granularität eines Service-Schnitts,
- Konsistenzmodell,
- RTO/RPO,
- Anzahl synchroner Abhängigkeiten im kritischen Pfad.

Wichtig:

> Der konkrete Wert wird gemessen oder aus Anforderungen abgeleitet.  
> Das Compendium liefert keine universellen Magic Numbers.

## 5. Trade-off-Punkt

Ein Trade-off liegt vor, wenn eine Entscheidung mehrere Qualitätsziele unterschiedlich beeinflusst.

Beispiele:

```text
stärkere Konsistenz
+ fachliche Aktualität
- Latenz / Verfügbarkeit / Kopplung

zentrale Plattform
+ Standardisierung / Governance
- lokale Autonomie / zentrale Abhängigkeit

feingranulare Services
+ unabhängige Deployments
- Netzwerk-/Betriebs-/Datenkomplexität
```

Ein Trade-off ist kein Fehler.

Er wird zum Problem, wenn er **unbewusst** entsteht.

## 6. Risiko, Non-Risk und Annahme

### Risiko

Eine architektonisch relevante Unsicherheit mit möglicher negativer Wirkung.

### Non-Risk

Ein analysierter Punkt, bei dem ausreichend Evidenz vorliegt, dass er im betrachteten Kontext tragfähig ist.

### Annahme

Etwas, das als wahr behandelt wird, aber noch nicht ausreichend bestätigt ist.

Annahmen gehören explizit ins Review. Viele Architekturprobleme entstehen nicht durch falsche Entscheidungen, sondern durch **unbeobachtete Annahmen**.

## 7. Evidence statt erfundener Präzision

Schlecht:

```text
"Diese Architektur spart 473.600 EUR."
```

wenn keine belastbare Kostenbasis existiert.

Besser:

```text
Kostenrisiko:
hoch

bekannte Treiber:
- zusätzlicher Plattformbetrieb
- Migrationsaufwand
- Lizenzkosten

fehlende Evidence:
- belastbares TCO-Modell
```

Das `?` ist eine professionelle Antwort.

## 8. Architekturreview-Canvas

| Bereich | Leitfrage |
|---|---|
| Auftrag | Welches Problem lösen wir? |
| Scope | Was wird bewertet, was nicht? |
| Stakeholder | Wer trägt Auswirkungen? |
| Quality | Welche Szenarien sind priorisiert? |
| Constraints | Was ist nicht verhandelbar? |
| Optionen | Welche Alternativen sind realistisch? |
| Sensitivitäten | Welche Parameter/Entscheidungen haben großen Hebel? |
| Trade-offs | Welche Qualität gewinnt auf Kosten welcher anderen? |
| Risiken | Was kann schiefgehen? |
| Evidence | Was wissen wir, was nehmen wir nur an? |
| Maßnahmen | Welche Mitigation ist nötig? |
| Review | Was muss später neu bewertet werden? |

## 9. Verbindung zu Fitness Functions

Ein Review ist punktuell.

Fitness Functions können einzelne Architektureigenschaften kontinuierlich überprüfen.

```text
Review:
"Domain darf Framework nicht kennen."

→ Fitness Function:
ArchUnit-Regel

Review:
"API muss kompatibel bleiben."

→ Fitness Function:
Schema-/Contract-Diff

Review:
"RTO muss erreichbar sein."

→ Evidence:
regelmäßiger Restore-/DR-Test
```

Nicht alles lässt sich automatisieren. Organisatorische Ownership oder fachliche Passung benötigen weiterhin menschliche Bewertung.

## 10. Typische Fehler

- formales ATAM-Etikett für ein informelles Meeting,
- erfundene Kosten- und Zeitwerte,
- bereits feststehende Lösung wird nur bestätigt,
- Stakeholder fehlen,
- nur technische Risiken werden betrachtet,
- Risiken werden gesammelt, aber nicht einem Owner zugeordnet,
- Review findet nach der Implementierung statt,
- alle Qualitätsziele gelten als gleich wichtig.

## 11. Quellen

- CMU/SEI, Architecture Tradeoff Analysis Method  
  https://www.sei.cmu.edu/library/architecture-tradeoff-analysis-method-collection/
- ISO/IEC/IEEE 42010:2022  
  https://www.iso.org/standard/74393.html
- AK-082 Qualitätsziele und Qualitätsszenarien

## 12. Merksatz

> Bewerte nicht, ob dir die Architektur gefällt.  
> Bewerte, **wie belastbar ihre Entscheidungen gegen die wichtigsten Szenarien, Risiken und Trade-offs sind**.
