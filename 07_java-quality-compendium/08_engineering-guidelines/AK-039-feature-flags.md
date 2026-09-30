---
id: AK-039
legacy_ids:
  - QG-JAVA-039
  - QG-JAVA-039-01
title: Feature Flags und Runtime-Konfiguration bewusst trennen
artifact_type: engineering-guideline
domain: delivery
status: active
maturity: reviewed
normative_level: recommended
last_validated: 2026-09-30
review_trigger:
  - Wechsel des Feature-Flag-Systems
  - Flag-Incident oder Security-Befund
  - wachsender Bestand dauerhaft aktiver Flags
---

# AK-039 — Feature Flags und Runtime-Konfiguration bewusst trennen

## Kurzfassung

Feature Flags entkoppeln Deployment und Aktivierung von Verhalten. Sie sind nützlich für kontrollierte Rollouts, Kill Switches und zeitlich begrenzte Varianten. Sie erzeugen zugleich zusätzliche Zustände und Testkombinationen.

Nicht jede Konfiguration ist ein Feature Flag. Nicht jedes Feature Flag ist ein Experiment. Und ein Feature Flag ist keine Autorisierungsregel.

## 1. Vier unterschiedliche Dinge

### Startup-/statische Konfiguration

Beispiele:

- Endpoint-URL,
- Poolgröße,
- Timeout,
- Aktivierung eines Infrastrukturadapters pro Umgebung.

Solche Werte werden häufig über `application.yml`, Environment Variables oder zentrale Configuration Services gesetzt und ändern sich nicht zwingend während der Laufzeit.

### Release-/Feature Flag

Steuert, ob eine Funktion für einen definierten Scope aktiv ist.

### Operational Kill Switch

Erlaubt, eine problematische Funktion schnell abzuschalten, ohne ein neues Artefakt zu bauen.

### Experiment / progressive rollout

Varianten werden nach definiertem Targeting oder prozentual verteilt und benötigen zusätzlich Mess- und Auswertungslogik.

Diese Kategorien besitzen unterschiedliche Lifecycle- und Governance-Anforderungen.

## 2. Flag-Evaluation kapseln

Business-Code sollte nicht an einen konkreten Vendor-SDK gebunden sein.

```java
interface FeatureDecisions {
    boolean newCheckoutEnabled(CustomerContext context);
}
```

Eine Implementierung kann einen Anbieter oder einen standardisierten Evaluationslayer wie OpenFeature verwenden.

Damit bleibt fachlicher Code frei von Vendor-spezifischer Flag-Konfiguration.

## 3. Evaluation Context und Datenschutz

Dynamische Flags können Kontext für Targeting benötigen. OpenFeature beschreibt dafür einen `evaluation context` und einen optionalen `targeting key`.

Der Kontext darf nicht zum Sammelplatz personenbezogener Daten werden. Es ist zu klären:

- welche Attribute wirklich benötigt werden,
- ob der Provider sie speichert oder überträgt,
- ob Hashing/Pseudonymisierung genügt,
- welche Telemetrie aus Flag-Evaluation entsteht.

## 4. Flags sind keine Authorization

Falsch:

```java
if (flags.adminFeature(user)) {
    deleteAllData();
}
```

Ein Flag kann steuern, ob eine UI oder Funktion ausgerollt wird. Die tatsächliche Berechtigung muss weiterhin vom Security-/Authorization-Modell geprüft werden.

```text
Feature Flag
→ Ist Funktion ausgerollt?

Authorization
→ Darf dieser Principal die Aktion ausführen?
```

Beides kann gleichzeitig erforderlich sein.

## 5. Defaults und Failure Mode

Ein Flag-Aufruf kann fehlschlagen oder der Provider nicht verfügbar sein. Deshalb besitzt jede Evaluation einen bewusst gewählten Default.

Die sichere Richtung hängt vom Flag ab:

- neues optionales Feature: häufig `false`,
- Kill Switch: Semantik explizit definieren,
- sicherheitskritische Funktion: kein stilles Fail-open.

Ein globales „Flags sind bei Fehler immer an/aus“ ist zu grob.

## 6. Lifecycle und technische Schuld

Jedes temporäre Flag braucht mindestens:

- Owner,
- Zweck,
- Erstellungsdatum,
- erwartetes Removal-Ereignis oder Review,
- Typ/Kategorie,
- Dokumentation der Default-Semantik.

Wenn ein Rollout abgeschlossen und die Variante entschieden ist, wird der Flag samt totem Branch entfernt.

Dauerhafte Operational Flags sind legitim, müssen dann aber als dauerhafter Steuerungsmechanismus behandelt werden und dürfen nicht als „temporär“ durch das System altern.

## 7. Teststrategie

Nicht jede theoretische Flagkombination ist testbar. Relevant sind:

- beide Zustände eines neuen Release Flags,
- kritische Interaktionen zwischen Flags,
- Default-/Provider-Fehlerpfad,
- Authorization unabhängig vom Flag,
- Migration und Entfernung alter Codepfade.

Bei vielen interagierenden Flags entsteht kombinatorische Komplexität; das ist ein Signal für Cleanup oder eine andere Modellierung.

## 8. Observability

Für wichtige Flags sollte nachvollziehbar sein:

- welche Variante aktiv ist,
- wie häufig ausgewertet wird,
- ob Evaluation fehlschlägt,
- ob ein Rollout mit Fehler-/Latenzänderungen korreliert.

Personenbezogene Evaluation Contexts werden nicht unkontrolliert geloggt.

## 9. Normative Regeln

### MUSS

- Feature Flags besitzen Owner und Lifecycle.
- Default-/Failure-Semantik ist bewusst definiert.
- Authorization bleibt unabhängig von Feature Flags wirksam.
- sensible Targeting-Daten werden datenschutzgerecht behandelt.
- abgeschlossene temporäre Flags werden entfernt.

### SOLLTE

- Business-Code hängt von einer eigenen fachlichen Flag-Abstraktion oder einem vendor-neutralen Evaluations-API ab.
- Rollout und technische Metriken werden gemeinsam beobachtet.
- Kill Switches werden in Runbooks und Incident-Prozessen berücksichtigt.

### DARF NICHT

- statische Konfiguration und dynamische Feature-Flags werden nicht begrifflich vermischt.
- Feature Flags ersetzen keine Rollen-/Rechteprüfung.
- Flags bleiben nicht unbegrenzt bestehen, nur weil Cleanup unbequem ist.

## 10. Prüffragen

1. Ist das wirklich ein Feature Flag oder normale Konfiguration?
2. Welcher Flag-Typ liegt vor?
3. Wer ist Owner und wann wird reviewed/entfernt?
4. Was ist der Default bei Provider-Ausfall?
5. Enthält der Evaluation Context PII?
6. Ist die Berechtigungsprüfung unabhängig davon korrekt?
7. Welche Varianten müssen getestet werden?
8. Welche Telemetrie zeigt Rollout-Auswirkungen?

## 11. Quellen

- OpenFeature Specification: https://openfeature.dev/specification/
- OpenFeature — Evaluation Context: https://openfeature.dev/specification/sections/evaluation-context/
- OpenFeature — Providers: https://openfeature.dev/specification/sections/providers/
- Martin Fowler — Feature Toggles: https://martinfowler.com/articles/feature-toggles.html

## 12. Merksatz

> Ein Feature Flag ist temporäre oder operative Entscheidungslogik zur Laufzeit. Ohne Owner, Failure-Semantik und Cleanup wird daraus dauerhafte, schwer testbare Komplexität.
