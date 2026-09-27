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
last_validated: 2026-09-28
technology_baseline:
  java: "21+; Virtual Threads final since JEP 444"
review_trigger:
  - JDK-Major-Wechsel mit relevanten Loom-/Concurrency-Änderungen
---

# AK-004 — Virtual Threads richtig entscheiden

## 1. Was Virtual Threads lösen

Virtual Threads sind seit Java 21 als finales Feature verfügbar.

Sie reduzieren die Kosten, große Mengen blockierender Tasks mit einem thread-per-request-or-task-Programmiermodell auszuführen.

Sie ändern damit vor allem die **Skalierbarkeit von Thread-Ressourcen**.

Sie lösen nicht automatisch:

- Race Conditions,
- Shared Mutable State,
- Datenbankpool-Limits,
- externe API-Kapazität,
- CPU-bound Workloads,
- Backpressure.

## 2. Gute Kandidaten

Virtual Threads sind besonders interessant bei:

- vielen gleichzeitig wartenden I/O-Operationen,
- klassischem blocking JDBC,
- HTTP-Aufrufen,
- Thread-per-request-Code,
- hoher Concurrent-Request-Zahl bei moderater CPU-Arbeit.

## 3. Schlechte Erwartung

> „Mit Virtual Threads brauchen wir keine Ressourcenlimits mehr.“

Falsch.

Wenn 50.000 Virtual Threads gleichzeitig auf eine Datenbank mit 50 Connections zugreifen, bleibt die Datenbank der Engpass.

Daraus folgt:

```text
billige Threads
≠
unbegrenzte Downstream-Kapazität
```

## 4. CPU-bound Arbeit

Für CPU-intensive Arbeit bestimmt weiterhin CPU-Kapazität den Durchsatz.

Mehr Virtual Threads erzeugen keine zusätzlichen Kerne.

Prüfen:

- CPU-Auslastung,
- Queueing,
- Thread/Task-Zahl,
- Latenz unter Last.

## 5. Backpressure / Admission Control

Virtual Threads machen es leicht, sehr viele Tasks zu starten.

Genau deshalb braucht die Architektur weiterhin:

- Rate Limiting,
- Semaphores/Bulkheads,
- Connection-Pool-Limits,
- Queue-Bounds,
- Timeouts.

## 6. Pinning und Runtime-Verhalten

Bei Implementierung müssen JDK-spezifische Hinweise zu Pinning beziehungsweise blocking inside certain synchronized/native regions gegen die verwendete Java-Version geprüft werden.

Nicht historische Warnungen blind fortschreiben; Loom-Verhalten entwickelt sich mit JDK-Versionen weiter.

## 7. Entscheidungsschritte

1. Workloadprofil messen: CPU vs I/O.
2. Concurrent Requests/Tasks verstehen.
3. Downstream-Budgets bestimmen.
4. bestehenden Threadpool-/Reactive-Code analysieren.
5. repräsentativen Lasttest erstellen.
6. Virtual-Thread-Variante vergleichen.
7. CPU, Memory, Pool Saturation, P95/P99 beobachten.
8. erst dann Standardisieren.

## 8. Virtual Threads vs. Reactive

Virtual Threads vereinfachen viele blocking I/O-Anwendungen.

Reactive Programming bleibt sinnvoll bei Anforderungen wie:

- durchgängige Stream-Verarbeitung,
- explizite Backpressure,
- nicht-blockierende APIs/Ökosysteme,
- sehr spezifische Streamingmodelle.

Siehe AK-091.

## 9. Quellen

- JEP 444 — Virtual Threads  
  https://openjdk.org/jeps/444
- Java Concurrency Documentation
- AK-033 — Thread Safety
- AK-091 — Reactive Architecture

## 10. Coach-Merksatz

> Virtual Threads machen **Warten billiger**, nicht externe Systeme unendlich schnell und nicht parallelen Code automatisch korrekt.
