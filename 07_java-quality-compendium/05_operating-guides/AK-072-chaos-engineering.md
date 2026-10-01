---
id: AK-072
legacy_ids:
  - QG-JAVA-072
title: Chaos Engineering risikobasiert durchführen
artifact_type: operating-guide
domain: resilience
status: active
maturity: reviewed
normative_level: recommended
last_validated: 2026-10-01
review_trigger:
  - Änderung kritischer Betriebsarchitektur
  - schwerer Resilience-Incident
  - Einführung neuer Chaos-Tooling-Plattform
---

# AK-072 — Chaos Engineering risikobasiert durchführen

## Kurzfassung

Chaos Engineering ist kein zufälliges Stören von Produktion. Es ist ein kontrolliertes Experiment: Eine konkrete Resilience-Hypothese wird unter begrenztem Risiko geprüft, beobachtet und ausgewertet.

Die Experimentgröße richtet sich nach Kritikalität, Reifegrad, Schutzbedarf und Wiederherstellbarkeit. Feste allgemeine Fehler- oder Latenzwerte sind keine sinnvolle organisationsweite Norm.

## 1. Voraussetzungen

Vor einem Experiment müssen mindestens vorhanden sein:

- klarer Service Owner,
- relevante SLIs/SLOs oder andere beobachtbare Erfolgskriterien,
- funktionierende Telemetrie,
- Runbook und Recovery-Pfad,
- bekannte Abhängigkeiten,
- definierte Stopbedingungen,
- Zustimmung der für den Blast Radius verantwortlichen Rollen.

Chaos Engineering ist kein Ersatz dafür, grundlegende Betriebsfähigkeit erst herzustellen.

## 2. Steady State definieren

Der Normalzustand wird über beobachtbare Größen beschrieben, zum Beispiel:

- erfolgreiche fachliche Transaktionen,
- Verfügbarkeit,
- Latenzverteilung,
- Fehlerrate,
- Queue-/Consumer-Lag,
- Recovery-Zeit.

Der relevante Steady State sollte möglichst aus Nutzer- oder Geschäftsservice-Sicht formuliert sein, nicht nur als „Pod läuft“.

## 3. Hypothese formulieren

Beispiel:

> Wenn eine Instanz des Services ausfällt, bleibt die fachliche Bestellung weiterhin innerhalb des vereinbarten Serviceziels bearbeitbar und es gehen keine bestätigten Vorgänge verloren.

Eine gute Hypothese ist prüfbar und beschreibt erwartetes Systemverhalten, nicht das Toolkommando.

## 4. Blast Radius begrenzen

Der Einstieg erfolgt auf der kleinsten sinnvollen Ebene:

```text
lokale/isolierte Testumgebung
→ Staging / produktionsnahe Umgebung
→ kleiner kontrollierter Produktionsscope
→ breitere Experimente nur bei nachgewiesener Reife
```

Mögliche Begrenzungen:

- einzelne Instanz,
- definierter Namespace,
- kleiner Traffic-Anteil,
- kurzer Zeitraum,
- ausgewählte Region oder Consumer Group,
- explizite Testmandanten, sofern die Architektur dies sicher unterstützt.

## 5. Experimenttypen

Beispiele:

- Prozess-/Pod-Ausfall,
- Netzwerkverzögerung oder Paketverlust,
- Downstream-Unverfügbarkeit,
- Ressourcenknappheit,
- Broker-/Consumer-Störung,
- DNS-/Dependency-Fehler,
- gezielter Verlust eines nichtkritischen Cache-Knotens.

Datenkorruption, Security-Control-Umgehung oder irreversible Fehler sind keine normalen Einstiegsübungen und benötigen deutlich strengere Schutzmaßnahmen.

## 6. Abort Conditions

Ein Experiment wird beendet, wenn definierte Sicherheitsgrenzen verletzt werden. Beispiele können sein:

- fachlicher Datenverlust oder Inkonsistenz,
- kritische Security-/Privacy-Auswirkung,
- überschrittenes lokales Error Budget für den Experiment-Scope,
- Recovery funktioniert nicht wie geplant,
- Auswirkungen verlassen den genehmigten Blast Radius.

Konkrete Schwellenwerte werden je Service festgelegt. Beispielwerte aus Übungen werden nicht als globale Policy missverstanden.

## 7. Beobachtung

Während des Experiments werden nicht nur technische Symptome betrachtet.

```text
Failure Injection
→ Systemreaktion
→ Nutzer-/Prozesswirkung
→ Detection
→ Alert
→ menschliche/automatische Reaktion
→ Recovery
```

Damit kann ein Experiment gleichzeitig technische Resilience und operative Fähigkeiten prüfen.

## 8. Ergebnis behandeln

Ein Experiment ist auch dann wertvoll, wenn die Hypothese widerlegt wird. Entscheidend ist, dass aus dem Ergebnis eine nachvollziehbare Verbesserung entsteht.

```text
Experiment
→ Beobachtung
→ Gap/Risiko
→ Owner
→ Maßnahme
→ erneuter Nachweis
```

Findings fließen gegebenenfalls in ADRs, Standards, Runbooks, SLOs oder Plattformverbesserungen ein.

## 9. Produktion oder nicht?

Chaos Engineering in Produktion kann wertvolle reale Erkenntnisse liefern, ist aber kein Reifegradabzeichen. Bei hohem Schutzbedarf, geringer Recovery-Reife oder fehlender Isolation kann eine produktionsnahe kontrollierte Umgebung angemessener sein.

Die Entscheidung ist eine Risikoentscheidung, keine Ideologie.

## 10. Normative Regeln

### MUSS

- Jedes Experiment besitzt Hypothese, Scope, Owner und Stopbedingungen.
- Observability und Recovery-Pfad müssen vor Beginn funktionsfähig sein.
- Blast Radius wird begrenzt und bewusst genehmigt.
- Findings werden dokumentiert und relevante Maßnahmen verfolgt.
- Experimente dürfen Security-, Datenschutz- oder Datenintegritätsgrenzen nicht unkontrolliert verletzen.

### SOLLTE

- mit kleinen reversiblen Experimenten begonnen werden.
- Steady State aus Nutzersicht formuliert werden.
- wiederkehrende Experimente automatisiert werden, wenn ihre Durchführung sicher und wertvoll ist.
- Recovery-Zeit und Detection mitgemessen werden.

### DARF NICHT

- Chaos wird nicht als zufälliges Fehlerinjizieren ohne Hypothese betrieben.
- Beispielschwellenwerte werden nicht als universelle Limits übernommen.
- Produktionschaos wird nicht durchgeführt, nur um „Reife“ zu demonstrieren.

## 11. Prüffragen

1. Welche konkrete Hypothese prüfen wir?
2. Welcher Steady State zeigt Erfolg oder Verletzung?
3. Wie klein kann der Blast Radius sein?
4. Wie wird das Experiment sofort gestoppt?
5. Welche Daten- oder Security-Risiken entstehen?
6. Wer entscheidet über Produktionsdurchführung?
7. Wie sieht Recovery aus?
8. Was passiert mit Findings nach dem Experiment?

## 12. Quellen

- Principles of Chaos Engineering: https://principlesofchaos.org/
- Google SRE Workbook — Non-Abstract Large System Design / Resilience-Praktiken: https://sre.google/workbook/
- CNCF Chaos Engineering Landscape/Projekte als Toolreferenz, nicht als Policygrundlage: https://landscape.cncf.io/

## 13. Merksatz

> Chaos Engineering ist ein kontrolliertes Resilience-Experiment. Der Wert liegt in überprüfbarer Hypothese, begrenztem Risiko und daraus abgeleiteten Verbesserungen – nicht im spektakulären Ausfall.
