# EG-JAVA-004 — Virtual Threads für I/O-lastige Nebenläufigkeit

## Kurzfassung

Virtual Threads sind seit Java 21 final. Sie eignen sich vor allem für Anwendungen mit sehr vielen gleichzeitig wartenden, überwiegend blockierenden I/O-Aufgaben. Ihr Hauptnutzen ist ein einfacheres Thread-per-Task-Modell bei hoher Concurrency.

Sie machen CPU-bound Arbeit nicht schneller, beseitigen keine Race Conditions und sind kein Ersatz für Backpressure, Lastbegrenzung, Timeouts oder sauberes Ressourcenmanagement.

## Validierungsstatus

**VERIFIED_WITH_CAVEATS** gegen JEP 444 und aktuellen OpenJDK-Stand.

Primärquellen:

- JEP 444 — Virtual Threads: https://openjdk.org/jeps/444
- Java SE 21 Thread API: https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/Thread.html

Hinweis: Aussagen zu Pinning müssen versionsbezogen formuliert werden, weil spätere JDKs das Verhalten weiterentwickelt haben. Diese Guideline behandelt Java 21 als Baseline und vermeidet deshalb pauschale Aussagen, die über alle zukünftigen JDK-Versionen gelten sollen.

## 1. Problem verstehen

Klassische Platform Threads sind Betriebssystem-Threads. Bei sehr vielen gleichzeitig blockierenden Requests kann deren Anzahl zu einem Skalierungsproblem werden.

Virtual Threads entkoppeln die Anzahl Java-seitiger Tasks weitgehend von der Zahl verfügbarer OS-Threads. Dadurch kann ein lesbares, synchrones Programmiermodell bei hoher I/O-Concurrency erhalten bleiben.

Beispiel:

```java
try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
    var future = executor.submit(() -> remoteClient.loadCustomer(id));
    return future.get();
}
```

## 2. Was JEP 444 tatsächlich sagt

Virtual Threads sind leichtgewichtige `java.lang.Thread`-Instanzen. Viele Virtual Threads können ihre Ausführung auf einer kleineren Zahl von Carrier-/Platform-Threads teilen.

OpenJDK nennt als Ziel insbesondere serverseitige Anwendungen im Thread-per-Request-Stil mit hoher Concurrency.

Wichtig: Virtual Threads sollen **nicht wie teure Platform Threads gepoolt werden**. Das Modell lautet grundsätzlich: ein Virtual Thread pro Task.

## 3. Wann Virtual Threads sinnvoll sind

- viele parallele HTTP-/REST-Aufrufe,
- JDBC-Zugriffe,
- Dateisystem-/Netzwerk-I/O,
- klassische request-orientierte Services,
- Legacy-/Blocking-Bibliotheken, die gut mit Thread-per-Request harmonieren.

## 4. Wann sie wenig helfen

### CPU-bound Workloads

Wenn 1.000 Tasks gleichzeitig CPU benötigen, existieren trotzdem nur endlich viele CPU-Kerne.

```text
mehr Threads ≠ mehr CPU
```

Für CPU-Parallelität bleiben begrenzte Executor-/ForkJoin-/Stream-Modelle relevant.

### knappe externe Ressourcen

Virtual Threads machen Datenbankverbindungen nicht unbegrenzt.

```text
100.000 Virtual Threads
≠
100.000 sinnvolle DB-Verbindungen
```

Connection Pools, Rate Limits, Semaphores und Backpressure bleiben notwendig.

### Concurrency-Korrektheit

Virtual Threads lösen keine:

- Race Conditions,
- Lost Updates,
- Deadlocks,
- inkorrekte Shared-State-Zugriffe.

Diese Themen gehören zu eigener Concurrency-Governance.

## 5. Coach-Perspektive: Trenne Concurrency von Capacity

Die wichtigste Frage lautet nicht:

> „Wie viele Threads können wir starten?“

Sondern:

> **„Welche Ressource limitiert den tatsächlichen Durchsatz?“**

Das kann sein:

- CPU,
- DB Connection Pool,
- Remote API,
- Socket-Limit,
- Rate Limit,
- Memory,
- Downstream-SLO.

Virtual Threads reduzieren den Thread als künstlichen Engpass. Andere Engpässe bleiben bestehen.

## 6. Normative Guideline

### MUSS

- Virtual Threads werden primär für I/O-lastige Tasks eingesetzt.
- Downstream-Ressourcen erhalten weiterhin Timeouts, Rate Limits oder Concurrency Limits.
- Shared Mutable State wird unabhängig vom Threadmodell korrekt synchronisiert oder vermieden.
- Performanceentscheidungen werden gemessen, nicht aus Threadzahlen abgeleitet.

### SOLLTE

- Ein Virtual Thread wird pro Task erzeugt statt über einen klassischen Thread-Pool künstlich begrenzt zu werden.
- Bestehender synchroner Code wird bevorzugt, wenn er fachlich klar und mit Virtual Threads ausreichend skalierbar ist.
- JFR/Profiling und Metriken werden für reale Lastanalysen genutzt.

### DARF NICHT

- Virtual Threads dürfen nicht als Begründung dienen, unbeschränkte Last auf Datenbanken oder externe Systeme zu erzeugen.
- „Virtual Threads lösen Nebenläufigkeit“ ist als Aussage unzulässig.

## 7. Beispiel: Concurrency begrenzen, ohne Threads zu poolen

Wenn ein Downstream nur 50 parallele Aufrufe verträgt, kann ein Semaphore die Ressource schützen:

```java
private final Semaphore permits = new Semaphore(50);

public Customer load(CustomerId id) throws InterruptedException {
    permits.acquire();
    try {
        return remoteClient.load(id);
    } finally {
        permits.release();
    }
}
```

Die Begrenzung gilt der **Downstream-Kapazität**, nicht der Existenz virtueller Threads.

## 8. Failure Modes

- Virtual Threads werden eingeführt, ohne Lastprofil zu messen.
- Ein DB-Pool mit 30 Connections wird mit Tausenden gleichzeitigen DB-Aufrufen überrannt.
- CPU-intensive Transformationen werden massiv parallelisiert und erhöhen nur Scheduling-Overhead.
- ThreadLocals werden gedankenlos für große Datenmengen pro Request verwendet.
- Framework-/Library-Verhalten wird aus veralteten JDK-Versionen übernommen.

## 9. Reviewfragen

1. Ist der Workload überwiegend I/O- oder CPU-bound?
2. Welche echte Ressource begrenzt die Kapazität?
3. Welche Downstreams brauchen Concurrency Limits?
4. Welche SLOs verbessern wir mit Virtual Threads konkret?
5. Welche Messung zeigt den Effekt vor und nach der Änderung?
6. Welche JDK-Version ist produktiv und welche Virtual-Thread-Einschränkungen gelten dort tatsächlich?

## 10. Architektenperspektive

Virtual Threads illustrieren einen zentralen Architekturgrundsatz:

> **Vereinfache das Programmiermodell, ohne physische Ressourcen zu ignorieren.**

Ein guter Architekt trennt logische Concurrency von realer Capacity.

## 11. Review-Trigger

- Wechsel der Java-LTS-Baseline,
- neue OpenJDK-Änderungen an Virtual Threads/Pinning,
- Änderung des Lastprofils,
- Engpässe in Downstream-Systemen,
- Performance-Incident mit Thread-/Pool-Bezug.
