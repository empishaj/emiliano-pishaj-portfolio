---
id: AK-004
legacy_ids:
  - QG-JAVA-004
title: Virtual Threads für I/O-lastige Nebenläufigkeit
artifact_type: decision-guide
domain: java-runtime
status: active
maturity: reviewed
normative_level: informative
last_validated: 2026-09-30
technology_baseline:
  java: "21+; Virtual Threads final since JEP 444"
review_trigger:
  - JDK-Major-Wechsel mit relevanten Loom-/Concurrency-Änderungen
  - Änderung des Lastprofils oder der Downstream-Kapazität
---

# AK-004 — Virtual Threads richtig entscheiden

## 1. Was Virtual Threads lösen

Virtual Threads sind seit Java 21 final. Sie reduzieren die Kosten, viele blockierende Tasks mit einem Thread-per-request- beziehungsweise Thread-per-task-Modell auszuführen.

Sie verbessern damit vor allem die Skalierbarkeit threadgebundener I/O-Concurrency.

Sie lösen nicht automatisch:

- Race Conditions,
- Shared Mutable State,
- Datenbankpool-Limits,
- externe API-Kapazität,
- CPU-bound Workloads,
- Backpressure oder Admission Control.

## 2. Gute Kandidaten

Virtual Threads sind besonders interessant bei:

- vielen gleichzeitig wartenden I/O-Operationen,
- klassischem blocking JDBC,
- synchronen HTTP-Aufrufen,
- Thread-per-request-Code,
- hoher Concurrent-Request-Zahl bei moderater CPU-Arbeit.

## 3. Die eigentliche Entscheidungsfrage

Nicht:

> Wie viele Threads können wir starten?

Sondern:

> Welche Ressource limitiert den tatsächlichen Durchsatz und wird der Platform Thread dabei selbst zum unnötigen Engpass?

Beispiel:

```text
50.000 Virtual Threads
        ↓
50 DB Connections
        ↓
Datenbankkapazität bleibt die Grenze
```

Billige Threads bedeuten keine unbegrenzte Downstream-Kapazität.

## 4. CPU-bound Arbeit

Für CPU-intensive Arbeit bestimmen weiterhin CPU-Kerne und Scheduling den Durchsatz. Mehr Virtual Threads erzeugen keine zusätzliche Rechenkapazität.

Bei CPU-lastigen Workloads sind begrenzte Parallelitätsmodelle häufig sinnvoller.

## 5. Ressourcen und Admission Control

Auch mit Virtual Threads bleiben notwendig:

- Timeouts,
- Connection-Pool-Limits,
- Rate Limits,
- Semaphores oder Bulkheads,
- begrenzte Queues,
- Backpressure, wo das Kommunikationsmodell sie benötigt.

Die Begrenzung schützt die knappe Ressource, nicht die Existenz virtueller Threads.

## 6. Pinning versionsbezogen betrachten

Historische Virtual-Thread-Empfehlungen enthalten häufig pauschale Warnungen zu `synchronized` und Pinning. Diese Aussagen dürfen nicht ohne JDK-Bezug übernommen werden.

JEP 444 beschreibt die Java-21-Baseline. Spätere OpenJDK-Arbeiten, insbesondere JEP 491, verändern das Pinning-Verhalten weiter. Bei Performanceanalysen gilt deshalb immer die tatsächlich eingesetzte JDK-Version.

Die dauerhafte Regel lautet:

> Runtime-Verhalten messen und gegen die verwendete JDK-Version prüfen, statt alte Loom-Heuristiken fortzuschreiben.

## 7. Entscheidungsschritte

1. Workloadprofil bestimmen: CPU, I/O und Blocking-Anteil.
2. Gleichzeitige Requests beziehungsweise Tasks messen.
3. Knappe Downstream-Ressourcen und deren Budgets bestimmen.
4. Bestehendes Threadpool- oder Reactive-Modell verstehen.
5. Repräsentativen Lasttest aufbauen.
6. Virtual-Thread-Variante unter derselben Last vergleichen.
7. CPU, Memory, Pool Saturation, Queueing sowie P95/P99 beobachten.
8. Entscheidung und Grenzen dokumentieren.

## 8. Virtual Threads und Reactive Programming

Virtual Threads können viele klassische blocking I/O-Anwendungen deutlich vereinfachen.

Reactive Programming bleibt eine eigene Option, etwa wenn durchgängige Stream-Verarbeitung, explizite Backpressure oder ein nicht-blockierendes Ökosystem Teil der eigentlichen Anforderung sind.

Die Wahl ist kein Reifegradvergleich. Sie hängt vom Verarbeitungsmodell ab.

## 9. Reviewfragen

1. Ist der Workload überwiegend I/O- oder CPU-bound?
2. Welche Ressource begrenzt die Kapazität tatsächlich?
3. Welche Downstreams brauchen Concurrency Limits?
4. Welche messbare Verbesserung erwarten wir?
5. Welche JDK-Version läuft produktiv?
6. Welche Messung bestätigt die Entscheidung vor und nach der Änderung?

## Merksatz

> Virtual Threads machen Warten billiger. Sie machen externe Systeme nicht unendlich schnell und parallelen Code nicht automatisch korrekt.

## Quellen

- JEP 444 — Virtual Threads: https://openjdk.org/jeps/444
- JEP 491 — Synchronize Virtual Threads without Pinning: https://openjdk.org/jeps/491
- Java SE Thread API: https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/Thread.html
- AK-033 — Thread Safety
- AK-091 — Reactive Architecture
