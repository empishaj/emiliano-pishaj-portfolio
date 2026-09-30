---
id: AK-004
legacy_ids:
  - QG-JAVA-004
title: Virtual Threads für I/O-lastige Nebenläufigkeit
artifact_type: engineering-guideline
domain: java-runtime
status: active
maturity: reviewed
normative_level: recommended
last_validated: 2026-09-30
technology_baseline:
  java: "21+; Virtual Threads final since JEP 444"
review_trigger:
  - Wechsel der Java-LTS-Baseline
  - relevante OpenJDK-Änderungen an Virtual Threads oder Pinning
  - Änderung des Lastprofils
  - Performance-Incident mit Thread- oder Poolbezug
---

# AK-004 — Virtual Threads für I/O-lastige Nebenläufigkeit

## Kurzfassung

Virtual Threads sind seit Java 21 final. Sie sind besonders für Anwendungen mit vielen gleichzeitig wartenden, überwiegend blockierenden I/O-Aufgaben geeignet. Ihr Nutzen liegt vor allem darin, ein einfaches Thread-per-Task-Modell bei hoher Concurrency wirtschaftlicher zu machen.

Sie machen CPU-bound Arbeit nicht schneller, lösen keine Race Conditions und ersetzen weder Backpressure noch Timeouts, Connection-Pool-Limits oder andere Kapazitätsgrenzen.

## 1. Welches Problem sie lösen

Klassische Platform Threads sind Betriebssystem-Threads. Eine sehr große Zahl gleichzeitig blockierender Requests kann dadurch selbst zum Skalierungsengpass werden.

Virtual Threads ermöglichen viele Java-seitige Tasks auf einer kleineren Anzahl von Carrier-Threads. Das ist vor allem für blockierende I/O-Workloads interessant.

```java
try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
    var future = executor.submit(() -> remoteClient.loadCustomer(id));
    return future.get();
}
```

Das Modell lautet grundsätzlich: ein Virtual Thread pro Task. Virtual Threads werden nicht wie teure Platform Threads künstlich gepoolt.

## 2. Gute Kandidaten

- viele parallele HTTP-Aufrufe,
- JDBC-Zugriffe,
- Netzwerk- oder Dateisystem-I/O,
- request-orientierte Services,
- bestehender gut verständlicher Blocking-Code.

## 3. Grenzen

### CPU bleibt knapp

Mehr Threads erzeugen keine zusätzlichen CPU-Kerne. Bei CPU-intensiven Aufgaben bleibt die verfügbare Rechenkapazität der Engpass.

### Downstream-Kapazität bleibt knapp

```text
100.000 Virtual Threads
≠
100.000 sinnvolle Datenbankverbindungen
```

Datenbanken, externe APIs und andere Downstreams brauchen weiterhin angemessene Limits.

### Korrektheit bleibt ein eigenes Thema

Virtual Threads verhindern keine:

- Race Conditions,
- Lost Updates,
- Deadlocks,
- fehlerhaften Zugriffe auf Shared Mutable State.

## 4. Concurrency und Capacity trennen

Die wichtigere Frage lautet nicht:

> Wie viele Threads können wir starten?

Sondern:

> Welche Ressource begrenzt den tatsächlichen Durchsatz?

Das kann CPU, eine Datenbank, eine externe API, ein Socket-Limit, Memory oder ein fachliches Rate Limit sein.

Wenn beispielsweise ein Downstream nur 50 parallele Aufrufe verkraftet, muss genau diese Ressource geschützt werden. Ein Semaphore oder Bulkhead kann dafür sinnvoll sein; ein Threadpool ist bei Virtual Threads nicht automatisch die passende Begrenzung.

## 5. Pinning versionsbezogen behandeln

Warnungen zu Pinning und blockierenden Operationen müssen gegen die tatsächlich verwendete JDK-Version geprüft werden. Das Laufzeitverhalten von Project Loom entwickelt sich weiter.

Dauerhafte Guidelines sollten deshalb keine historischen Einschränkungen so formulieren, als wären sie für alle zukünftigen JDK-Versionen unverändert gültig.

## 6. Virtual Threads und Reactive Programming

Virtual Threads vereinfachen viele I/O-lastige Anwendungen mit blocking APIs.

Reactive Programming bleibt eine sinnvolle Option, wenn Anforderungen wie diese im Vordergrund stehen:

- durchgängige Stream-Verarbeitung,
- explizite Backpressure,
- reaktive Bibliotheks- und Protokollketten,
- sehr spezifische Streamingmodelle.

Die Entscheidung folgt dem Workload und nicht einer allgemeinen Technologiepräferenz.

## 7. Normative Regeln

### MUSS

- Vor einer Standardisierung wird das reale Lastprofil betrachtet.
- Downstream-Ressourcen behalten Timeouts und angemessene Concurrency- oder Rate-Limits.
- Shared Mutable State wird unabhängig vom Threadmodell korrekt behandelt.
- Performanceentscheidungen werden gemessen.

### SOLLTE

- Virtual Threads werden primär für I/O-lastige Tasks eingesetzt.
- Bestehender synchroner Code wird bevorzugt, wenn er verständlich ist und die Qualitätsziele erfüllt.
- JFR, Profiling und Runtime-Metriken werden bei relevanten Lastanalysen verwendet.

### DARF NICHT

- Virtual Threads werden nicht als Begründung für unbeschränkte Last auf Downstream-Systeme verwendet.
- Die Aussage „Virtual Threads lösen Nebenläufigkeit“ wird nicht verwendet.

## 8. Prüffragen

1. Ist der Workload überwiegend I/O- oder CPU-bound?
2. Welche Ressource begrenzt die Kapazität wirklich?
3. Welche Downstreams benötigen eigene Concurrency-Limits?
4. Welches Qualitätsziel soll sich verbessern?
5. Welche Messung zeigt den Effekt vor und nach der Änderung?
6. Welche JDK-Version läuft produktiv und welche Einschränkungen gelten dort tatsächlich?

## 9. Quellen

- JEP 444 — Virtual Threads  
  https://openjdk.org/jeps/444
- Java SE API — `Thread`  
  https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/Thread.html

## 10. Merksatz

> Virtual Threads machen Warten billiger. Sie machen CPU, Datenbanken und externe Systeme nicht unbegrenzt schnell und parallelen Code nicht automatisch korrekt.
