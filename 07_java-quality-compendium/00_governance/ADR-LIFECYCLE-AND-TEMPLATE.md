# ADR Lifecycle und Master-Template

## 1. Wann entsteht ein ADR?

Ein ADR entsteht nur, wenn eine **konkrete Entscheidung** im konkreten Kontext getroffen werden muss.

Prüffragen:

1. Was muss entschieden werden?
2. Warum jetzt?
3. Welche Stakeholder haben ein legitimes Concern?
4. Welche Qualitätsziele oder Constraints werden beeinflusst?
5. Welche realistischen Optionen existieren?
6. Welche Option wird gewählt?
7. Was kostet uns diese Wahl?
8. Welche Evidenz zeigt später, ob die Entscheidung trägt?
9. Wann muss sie überprüft werden?

Kann die erste Frage nicht als einzelne, klare Frage formuliert werden, ist der Scope noch nicht sauber.

## 2. Lifecycle

```text
Draft
  ↓
Proposed
  ↓
Consulted / Reviewed
  ↓
Accepted ────────────────┐
  │                       │
  ├─ remains valid        │
  │                       │
  └─ new decision needed  │
                          ↓
                    new ADR
                          ↓
old ADR ← Superseded by ──┘

Alternative:
Proposed → Rejected
Accepted → Deprecated
```

Akzeptierte ADRs werden nicht rückwirkend so umgeschrieben, als hätte die Organisation früher schon anders entschieden.

## 3. Rollen

- **Owner:** formuliert und pflegt den Entscheidungsprozess.
- **Decision Makers:** besitzen das Mandat zur Entscheidung.
- **Consulted:** liefern Expertise und Auswirkungen.
- **Informed:** müssen die Entscheidung kennen und umsetzen können.
- **Evidence Owner:** stellt Nachweise oder Messwerte bereit.
- **Exception Owner:** verantwortet gegebenenfalls eine befristete Abweichung.

In einem persönlichen Portfolio werden keine realen Gremien oder Entscheider erfunden. Fallstudien werden klar als **synthetische Fallstudie** markiert.

## 4. Master-Template

```markdown
---
id: ADR-YYYY-NNN
title: ""
status: draft
date_proposed: YYYY-MM-DD
date_decided:
owner_role:
decision_makers: []
consulted: []
informed: []
scope:
domains: []
related_knowledge: []
supersedes: []
superseded_by:
review_trigger: []
evidence: []
---

# ADR-YYYY-NNN — [Entscheidungstitel]

## Executive Summary
[Problem, gewählte Option, wichtigster Nutzen, wichtigster Trade-off in 4–7 Sätzen.]

## 1. Kontext und Problem
[Wertneutrale Ausgangslage. Welche Kräfte, Zwänge und Risiken wirken?]

## 2. Entscheidungsfrage
[Genau eine Frage.]

## 3. Scope
### In Scope
- ...

### Out of Scope
- ...

## 4. Stakeholder und Concerns
| Stakeholder | Concern | Warum relevant |
|---|---|---|

## 5. Decision Drivers
- [Treiber / Constraint / Qualitätsziel]
- ...

## 6. Messbare Qualitätsszenarien
| Qualität | Stimulus | Umgebung | Response | Messgröße |
|---|---|---|---|---|

## 7. Betrachtete Optionen
### Option A — ...
**Beschreibung**

**Vorteile**

**Nachteile / Risiken**

**Offene Annahmen**

### Option B — ...
...

## 8. Bewertung
[Keine Scheingenauigkeit. Gewichtungen nur, wenn sie fachlich legitimiert sind.]

| Kriterium | A | B | C | Evidenz / Begründung |
|---|---:|---:|---:|---|

## 9. Entscheidung
**Wir werden ...**

[Geltungsbereich, Stichtag, Bedingungen.]

## 10. Rationale
[Warum ist diese Option im aktuellen Kontext tragfähiger als die Alternativen?]

## 11. Konsequenzen
### Positiv
- ...

### Negativ / Trade-offs
- ...

### Neue Risiken
- ...

## 12. Kompensationsmaßnahmen
- ...

## 13. Umsetzung
| Maßnahme | Owner-Rolle | Abhängigkeit | Nachweis |
|---|---|---|---|

## 14. Compliance und Evidence
[Tests, Architekturtests, Policy Checks, SLOs, Scans, Abnahmeunterlagen ...]

## 15. Auswirkungen auf Architektursichten
| Sicht | Auswirkung |
|---|---|
| Business / Capability | |
| Organisation / Verantwortung | |
| Daten | |
| Anwendungen | |
| Integration | |
| Security / IAM | |
| Plattform | |
| Betrieb | |
| Transformation / Migration | |

## 16. Ausnahmen
[Wie kann abgewichen werden? Wer darf entscheiden? Ablaufdatum? Kompensationen?]

## 17. Review-Trigger
- ...
- ...

## 18. Beziehungen
- Principles:
- Standards:
- Reference Architectures:
- andere ADRs:

## 19. Validierungsquellen
### Primärquellen
- ...

### Sekundärquellen
- ...

## 20. Entscheidungsverlauf
| Datum | Ereignis | Ergebnis |
|---|---|---|
```

## 5. Bewertung ohne Scheingenauigkeit

Nicht jede Entscheidung braucht Scores.

Scores sind sinnvoll, wenn:

- Kriterien vergleichbar sind,
- Gewichte legitimiert wurden,
- Stakeholder die Gewichtung mittragen,
- Zahlen nicht nur eine bereits getroffene Präferenz dekorieren.

Für viele Entscheidungen ist eine qualitative Trade-off-Matrix ehrlicher:

```text
++ deutlich förderlich
+  förderlich
0  neutral / kein wesentlicher Effekt
-  nachteilig
-- stark nachteilig
?  Evidenz fehlt
```

Die Kategorie `?` ist wichtig. Unsicherheit darf sichtbar bleiben, solange sie bewusst behandelt wird.

## 6. Ausnahme-ADR

Ein Standard ohne Ausnahmeweg wird entweder ignoriert oder dogmatisch.

Eine Ausnahme muss mindestens enthalten:

- Standard, von dem abgewichen wird,
- konkreter Grund,
- Geltungsbereich,
- Risiko,
- Kompensationsmaßnahmen,
- Owner,
- Ablaufdatum,
- Exit-/Migrationsplan.

## 7. Regel

Ein professioneller ADR beweist nicht, dass die Antwort schon vor der Analyse feststand.

Er zeigt, dass eine relevante Frage transparent, vergleichbar und nachvollziehbar entschieden wurde.
