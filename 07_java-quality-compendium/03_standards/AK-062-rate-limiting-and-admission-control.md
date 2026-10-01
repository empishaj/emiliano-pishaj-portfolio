---
id: AK-062
legacy_ids:
  - ADR-062
title: Rate Limiting und Admission Control
artifact_type: architecture-standard
domain: resilience-security
status: active
maturity: reviewed
normative_level: normative
owner_role: Platform / Security Architecture
last_validated: 2026-10-01
review_trigger:
  - Änderung des API-Gateway- oder Traffic-Modells
  - Abuse-/Capacity-Incident
---

# AK-062 — Rate Limiting und Admission Control

## 1. Zweck

Rate Limiting schützt Systeme nicht nur vor Angreifern. Es begrenzt auch Schäden durch:

- fehlerhafte Clients,
- Retry-Stürme,
- Endlosschleifen,
- noisy neighbours,
- Lastspitzen,
- begrenzte Downstream-Kapazitäten.

Der Standard verlangt deshalb nicht einen universellen Algorithmus, sondern ein **bewusstes Admission-Control-Modell**.

## 2. Vor jedem Limit die Schutzressource klären

Frage zuerst:

> Was soll geschützt werden?

Mögliche Ressourcen:

- API insgesamt,
- einzelne teure Operation,
- Nutzer,
- Mandant,
- technischer Client,
- externe Downstream-API,
- Datenbank,
- AI-/LLM-Budget,
- Batchkapazität.

Erst danach wird die Limitierungsdimension gewählt.

## 3. Verbindliche Regeln

SOLLTE:

- Limits je relevantem Consumer-/Ressourcenkontext definieren,
- Verhalten bei Überschreitung dokumentieren,
- `429 Too Many Requests` bei HTTP passend verwenden,
- gegebenenfalls Retry-Information bereitstellen,
- Limit-Events messen,
- administrative Bypass-/Emergency-Regeln kontrollieren.

DARF NICHT:

- ein einziges globales Limit unreflektiert auf alle Operationen anwenden,
- Rate Limiting als Ersatz für Authorization oder Kapazitätsplanung behandeln.

## 4. Algorithmen als Optionen

### Token Bucket

Geeignet, wenn kurze Bursts erlaubt sein sollen, aber langfristige Rate begrenzt wird.

### Sliding Window

Bietet genauere Betrachtung eines Zeitfensters, ist aber zustandsintensiver.

### Fixed Window

Einfach, kann aber an Fenstergrenzen Burst-Effekte erzeugen.

### Leaky Bucket / Queueing

Kann Zufluss glätten, verändert jedoch Warteverhalten und braucht klare Queue-/Timeout-Strategie.

Es gibt keinen universell besten Algorithmus.

## 5. Verteilte Systeme

Bei mehreren Instanzen muss geklärt werden, ob das Limit:

- lokal pro Instanz,
- zentral,
- partitioniert nach Consumer

gilt.

Ein lokales Limit `100/s` auf zehn Pods ist systemweit etwas anderes als ein globales Limit `100/s`.

## 6. Verhältnis zu anderen Resilience-Mechanismen

```text
Rate Limiting
→ begrenzt Eingang

Timeout
→ begrenzt Wartezeit

Bulkhead
→ begrenzt Ressourcenausbreitung

Circuit Breaker
→ stoppt erfolglose Downstream-Aufrufe

Retry
→ wiederholt ausgewählte Fehler
```

Diese Mechanismen werden kombiniert, aber nicht blind gestapelt.

Insbesondere Retry kann ohne Budget und Jitter ein Lastproblem verschärfen.

## 7. Limits evidenzbasiert festlegen

Nicht:

> „100 Requests pro Sekunde ist unser Standard.“

Sondern:

1. erwartetes Lastprofil bestimmen,
2. Downstream-/Ressourcenkapazität messen,
3. Burstbedarf bestimmen,
4. Schutz- und Fairnessziel definieren,
5. Limit testen,
6. Laufzeitmetriken beobachten,
7. bei Bedarf anpassen.

## 8. Fairness und Mandanten

Bei Multi-Tenant-Systemen muss entschieden werden, ob ein großer Mandant andere verdrängen kann.

Mögliche Modelle:

- globales Budget + Tenant-Limits,
- priorisierte Klassen,
- getrennte Pools für kritische Operationen,
- Kosten-/Token-basierte Limits statt Requestanzahl.

## 9. Evidence

- `rate_limit_allowed_total`,
- `rate_limit_rejected_total`,
- Consumer-/Tenant-Verteilung,
- Downstream Saturation,
- 429-Rate,
- Queue-/Wait-Time,
- Load-Test mit Überschreitungsszenarien.

## 10. Merksatz

> Rate Limiting beginnt nicht mit Token Bucket. Es beginnt mit der Frage: **Welche knappe Ressource schützen wir vor welchem Consumer- und Fehlverhalten?**
