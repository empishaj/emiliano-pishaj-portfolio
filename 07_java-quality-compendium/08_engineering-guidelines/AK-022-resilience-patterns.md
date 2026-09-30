---
id: AK-022
legacy_ids:
  - QG-JAVA-022
title: Resilience Patterns für synchrone Abhängigkeiten
artifact_type: engineering-guideline
domain: resilience
status: active
maturity: reviewed
normative_level: recommended
last_validated: 2026-09-30
review_trigger:
  - Änderung kritischer Downstream-Abhängigkeiten
  - Incident durch Timeout-, Retry- oder Überlastverhalten
  - Wechsel der Resilience-Bibliothek
---

# AK-022 — Resilience Patterns für synchrone Abhängigkeiten

## Kurzfassung

Timeout, Retry, Circuit Breaker und Bulkhead lösen unterschiedliche Probleme. Sie dürfen nicht als Paket reflexartig auf jeden Remote Call gelegt werden.

Die zentrale Frage ist: **Welche Fehlerklasse wollen wir begrenzen, und welche zusätzliche Last erzeugt unsere Gegenmaßnahme?**

## 1. Timeout

Jeder Remote Call benötigt eine begrenzte Wartezeit. Ohne Timeout kann ein langsamer Downstream Threads, Connections und Requestkapazität über lange Zeit binden.

Timeouts werden aus SLO, normalem Latenzprofil und verbleibendem End-to-End-Budget abgeleitet. Ein pauschaler organisationsweiter Millisekundenwert ist selten sinnvoll.

## 2. Retry

Retry ist sinnvoll bei plausibel transienten Fehlern, zum Beispiel kurzfristiger Nichterreichbarkeit oder ausgewählten 5xx-/Transportfehlern.

Retry ist gefährlich bei:

- nicht-idempotenten Operationen,
- dauerhaften fachlichen Fehlern,
- Überlast des Downstreams,
- fehlendem Backoff,
- mehreren Retry-Schichten übereinander.

Ein Retry vervielfacht Last. Drei Ebenen mit jeweils drei Versuchen können aus einem Request weit mehr Downstream-Aufrufe machen als erwartet.

## 3. Circuit Breaker

Ein Circuit Breaker kann Aufrufe an eine erkennbar gestörte Abhängigkeit vorübergehend stoppen. Er schützt vor wiederholten aussichtslosen Calls und verkürzt Fehlerpfade.

Er ersetzt keinen Timeout und repariert den Downstream nicht. Schwellenwerte werden gegen reale Metriken und Fehlerbudgets eingestellt.

## 4. Bulkhead

Bulkheads begrenzen, wie viel lokale Kapazität eine Abhängigkeit verbrauchen darf. Resilience4j bietet unter anderem semaphore- und thread-pool-basierte Varianten.

Bei Virtual Threads kann eine Semaphore häufig die eigentliche Ressourcengrenze ausdrücken, ohne künstlich wieder einen Threadpool als Kapazitätsmodell einzuführen.

## 5. Fallback

Ein Fallback ist nur sinnvoll, wenn sein Ergebnis fachlich zulässig ist.

Beispiele:

- gecachter, als möglicherweise veraltet gekennzeichneter Read,
- eingeschränkte Funktion statt Totalausfall,
- asynchrone Nachverarbeitung.

Gefährlich ist ein Fallback, der fachliche Fehler versteckt oder falsche Daten als aktuell erscheinen lässt.

## 6. Kombination bewusst entwerfen

Eine typische Kette kann sein:

```text
lokales Capacity Limit
→ Timeout
→ gezielter Retry
→ Circuit Breaker
→ kontrollierter Fallback
```

Die konkrete Reihenfolge hängt von Bibliothek und Problem ab. Entscheidend ist, die Gesamtwirkung zu verstehen und zu testen.

## 7. Idempotenz

Vor Retry einer schreibenden Operation ist zu klären, ob Wiederholung sicher ist. Mögliche Mechanismen:

- natürliche Idempotenz,
- Idempotency Key,
- deduplizierende Speicherung,
- transaktionale Semantik des Zielsystems.

„POST darf nie retried werden“ ist ebenso zu pauschal wie „Retry ist immer sicher“.

## 8. Observability

Für geschützte Abhängigkeiten werden mindestens beobachtet:

- Call-Latenz,
- Erfolgs-/Fehlerrate,
- Timeouts,
- Retry-Anzahl,
- Circuit-Breaker-Zustand,
- abgelehnte Bulkhead-Aufrufe,
- Fallback-Nutzung.

Resilience ohne Metriken ist schwer steuerbar.

## 9. Normative Regeln

### MUSS

- Remote Calls besitzen angemessene Timeouts.
- Retry wird nur für definierte transiente Fehler und mit begrenzter Versuchszahl eingesetzt.
- schreibende Retries benötigen geklärte Idempotenz.
- Resilience-Konfiguration ist beobachtbar.

### SOLLTE

- Backoff und gegebenenfalls Jitter werden bei Retry berücksichtigt.
- Capacity Limits schützen knappe Downstream-Ressourcen.
- Circuit-Breaker-Schwellenwerte werden aus realem Verhalten abgeleitet.
- Failure-Szenarien werden in Integration-/Resilience-Tests geprüft.

### DARF NICHT

- Retry wird nicht auf fachliche Validierungsfehler angewandt.
- Fallbacks dürfen keine falsche fachliche Sicherheit erzeugen.
- mehrere Retry-Schichten werden nicht unkoordiniert gestapelt.
- Defaultwerte einer Bibliothek gelten nicht automatisch als Produktionsstandard.

## 10. Prüffragen

1. Welche Fehlerklasse adressiert das Pattern?
2. Ist die Operation idempotent oder deduplizierbar?
3. Wie viel zusätzliche Last erzeugt ein Retry?
4. Welche Ressource begrenzt der Bulkhead?
5. Was sieht der Nutzer bei Fallback oder Circuit Open?
6. Welche Metrik zeigt, ob die Konfiguration funktioniert?

## 11. Quellen

- Resilience4j Documentation: https://resilience4j.readme.io/docs
- Resilience4j Retry: https://resilience4j.readme.io/docs/retry
- Resilience4j CircuitBreaker: https://resilience4j.readme.io/docs/circuitbreaker
- Resilience4j Bulkhead: https://resilience4j.readme.io/docs/bulkhead

## 12. Merksatz

> Resilience Patterns sollen Fehler begrenzen. Falsch kombiniert können sie Fehler und Last vervielfachen.
