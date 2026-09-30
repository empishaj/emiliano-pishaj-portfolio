---
id: AK-041
legacy_ids:
  - QG-JAVA-041
title: Event-Driven Architecture und Kafka bewusst auswählen
artifact_type: decision-guide
domain: integration
status: active
maturity: reviewed
normative_level: informative
last_validated: 2026-09-30
review_trigger:
  - Änderung der Integrationsstrategie
  - Wechsel der Kafka-Major-Version
  - Incident durch Ordering, Duplicate Processing oder Schema-Evolution
---

# AK-041 — Event-Driven Architecture und Kafka bewusst auswählen

## 1. Einordnung

Event-Driven Architecture (EDA) ist ein Integrationsstil, bei dem Zustandsänderungen als Ereignisse publiziert und von anderen Komponenten asynchron verarbeitet werden können. Kafka kann dafür ein geeigneter Event-Streaming-Baustein sein. Es ist jedoch weder für jede Integration nötig noch ersetzt ein Broker klare fachliche Grenzen.

Die wichtigste Entscheidungsfrage lautet:

> Brauchen wir zeitliche Entkopplung, mehrere unabhängige Consumer, Replay/Streaming oder hohe Ereignisraten so stark, dass die zusätzliche Betriebs- und Konsistenzkomplexität gerechtfertigt ist?

## 2. Gute Treiber für EDA

EDA ist besonders interessant, wenn:

- Producer und Consumer zeitlich entkoppelt sein sollen,
- mehrere Consumer unabhängig auf dasselbe Ereignis reagieren,
- Ereignisse als nachvollziehbarer Integrationsstrom benötigt werden,
- hohe Daten- oder Ereignisraten verarbeitet werden,
- Replay oder stream-orientierte Verarbeitung fachlich/technisch relevant ist,
- ein synchroner Aufrufpfad unnötig viele Systeme koppeln würde.

Nicht jeder Hintergrundjob und nicht jede interne Methodenausführung benötigt einen Event Broker.

## 3. Event oder Command?

Ein Event beschreibt etwas, das bereits geschehen ist:

```text
OrderConfirmed
PaymentReceived
DocumentClassified
```

Ein Command fordert eine Aktion an:

```text
CapturePayment
GenerateDocument
```

Die Unterscheidung ist wichtig für Ownership. Ein Event verkündet einen Fakt; ein Command adressiert eine Verantwortung.

## 4. Event Ownership

Der Producer besitzt die Bedeutung des Ereignisses und den fachlichen Zeitpunkt, zu dem es wahr wird. Consumer dürfen den Eventvertrag interpretieren, aber nicht still eine andere Semantik hineinlesen.

Für relevante Events werden mindestens geklärt:

- fachlicher Owner,
- Producer,
- Consumer-Gruppen,
- Schema/Contract,
- Versionierungsstrategie,
- Schlüssel/Partitionierungssemantik,
- Datenschutz/Schutzbedarf,
- Retention und Replay-Folgen.

## 5. Kafka Ordering richtig verstehen

Kafka garantiert Reihenfolge innerhalb einer Partition. Ereignisse mit demselben Schlüssel werden typischerweise derselben Partition zugeordnet und können dort in Schreibreihenfolge konsumiert werden.

Daraus folgt:

> Wenn Reihenfolge pro fachlicher Entität wichtig ist, muss die Partitionierungsstrategie genau diese Entität stabil zusammenhalten.

Globale Reihenfolge über ein skaliertes Topic hinweg sollte nicht still angenommen werden.

## 6. Delivery Semantics

### At-most-once

Verarbeitung kann verloren gehen, aber nicht wiederholt werden. Für viele Geschäftsprozesse ungeeignet.

### At-least-once

Eine Nachricht kann erneut zugestellt oder erneut verarbeitet werden. Consumer müssen deshalb Wiederholung bewusst behandeln.

### Kafka Idempotent Producer

Kafka kann Producer-Retries deduplizieren. Das verhindert jedoch nicht automatisch anwendungsseitige Doppelverarbeitung oder einen doppelten externen Seiteneffekt.

### Kafka Transactions / Exactly Once

Kafka-Transaktionen können für klar definierte Kafka-`read → process → write`-Pfade Exactly-Once-Semantik ermöglichen, wenn Producer, Broker und Consumer entsprechend konfiguriert sind.

Das ist **keine universelle Ende-zu-Ende-Garantie** für Datenbank, E-Mail, HTTP-API oder andere externe Systeme.

## 7. Idempotente Consumer

At-least-once-Verarbeitung erfordert häufig einen idempotenten Consumer.

Mögliche Strategien:

- fachlich idempotente Operation,
- deduplizierende Event-/Message-ID,
- Unique Constraint,
- Inbox-/Processed-Message-Tabelle,
- versionsbasierte Zustandsprüfung.

Die konkrete Strategie richtet sich nach Seiteneffekt und Datenmodell. Eine zentrale Idempotenz-Tabelle für jeden Consumer ist kein universeller Standard.

## 8. Dual Write und Outbox

Wenn eine Anwendung in derselben fachlichen Operation sowohl ihre Datenbank ändern als auch ein Event veröffentlichen muss, entsteht ein Dual-Write-Problem.

Der Transactional-Outbox-Ansatz wird in `AK-042` behandelt. Seine Rolle ist nicht „Kafka braucht Outbox“, sondern:

> Datenbankzustand und zu publizierendes Ereignis werden zunächst atomar in einer lokalen Transaktion festgehalten; die Publikation folgt zuverlässig danach.

## 9. Schema und Vertrag

Ein Event ist eine organisationsübergreifende Schnittstelle, sobald mehrere Teams oder Systeme davon abhängen.

Wichtiger als die pauschale Wahl „Avro ist Standard“ ist:

- maschinenlesbarer Vertrag,
- dokumentierte Semantik,
- kontrollierte Evolution,
- Compatibility Checks,
- Consumer-Verantwortung.

Geeignete Schemaformate können je nach Plattform Avro, Protobuf oder JSON Schema sein. AsyncAPI kann zusätzlich Channels, Rollen und Nachrichtenverträge dokumentieren; siehe `AK-095`.

## 10. Choreography oder Orchestration

Keine Variante ist grundsätzlich überlegen.

### Choreography

Vorteile:

- lokale Autonomie,
- geringe zentrale Kopplung,
- natürliche Reaktion auf Events.

Risiken:

- Gesamtprozess schwerer sichtbar,
- implizite Abhängigkeiten,
- schwierigeres Fehler-/Timeout-Management bei langen Prozessketten.

### Orchestration

Vorteile:

- Prozesszustand und Ablauf explizit,
- zentrale Sicht auf langlaufende Koordination,
- klare Timeout-/Kompensationssteuerung.

Risiken:

- zusätzliche zentrale Verantwortung,
- Gefahr eines fachlich übermächtigen Orchestrators.

Die Wahl folgt Prozesskomplexität, Ownership und Beobachtbarkeit. `AK-065` behandelt Saga-Entscheidungen vertieft.

## 11. Retry, DLT und Poison Messages

Technische Fehler, ungültige Nachrichten und fachliche Ablehnungen sind zu unterscheiden.

```text
transienter technischer Fehler
→ begrenzter Retry / Backoff

wiederholt nicht verarbeitbare Nachricht
→ DLT/DLQ + Alarm + Diagnose + kontrolliertes Reprocessing

fachlicher Konflikt
→ fachlicher Fehlerpfad / Kompensation
```

Ein DLT ist kein Archiv, in dem Fehler ungesehen liegen dürfen. Siehe `AK-097`.

## 12. Security und Datenschutz

Events können lange gespeichert, repliziert und von mehreren Consumern gelesen werden. Deshalb sind vor Publikation zu klären:

- Datenminimierung,
- zulässige Consumer,
- Verschlüsselung in Transit und gegebenenfalls at Rest,
- ACLs/Topic Authorization,
- Retention,
- Lösch-/Berichtigungskonzept,
- Umgang mit PII in Event Payload und Headern.

## 13. Observability

Relevante Signale sind unter anderem:

- Consumer Lag,
- Durchsatz,
- Fehler-/Retryrate,
- DLT-Zufluss,
- Processing Latency,
- Rebalance-/Consumer-Gruppen-Ereignisse,
- Broker-/Clientfehler.

Technische Metriken werden mit fachlichen Prozessindikatoren verbunden, wenn ein Event Teil eines kritischen Geschäftsprozesses ist.

## 14. Wann Kafka nicht die richtige Antwort ist

Ein synchroner API-Call kann klarer sein, wenn:

- der Aufrufer unmittelbar eine Antwort benötigt,
- genau ein Consumer existiert,
- kein Replay-/Streamingbedarf besteht,
- Fehler sofort an den Benutzer zurückgegeben werden müssen,
- zusätzliche asynchrone Zustände mehr Komplexität als Nutzen erzeugen.

Eine Datenbanktabelle oder Job Queue kann ausreichend sein, wenn nur einfache Hintergrundverarbeitung innerhalb eines Systems benötigt wird.

## 15. Entscheidungsfragen

1. Welches Kopplungsproblem soll Asynchronität lösen?
2. Wer besitzt das Event fachlich?
3. Welche Reihenfolge ist wirklich erforderlich und pro welchem Schlüssel?
4. Welche Delivery-Semantik benötigt der Use Case?
5. Welche externen Seiteneffekte bleiben außerhalb von Kafka-Transaktionen?
6. Wie wird Idempotenz erreicht?
7. Wie entwickelt sich der Eventvertrag kompatibel?
8. Welche Retention und Replay-Folgen sind akzeptabel?
9. Wie werden Fehler erkannt und reprocessed?
10. Ist Choreography oder explizite Orchestration für diesen Prozess verständlicher?

## 16. Quellen

- Apache Kafka Documentation: https://kafka.apache.org/documentation/
- Apache Kafka Producer Configuration: https://kafka.apache.org/documentation/#producerconfigs
- Apache Kafka Design: https://kafka.apache.org/documentation/#design
- Spring for Apache Kafka — Transactions / Exactly Once: https://docs.spring.io/spring-kafka/reference/kafka/exactly-once.html
- AsyncAPI: https://www.asyncapi.com/docs

## 17. Verwandte Knowledge-Items

- `AK-019` — Contract Testing
- `AK-042` — Transactional Outbox
- `AK-065` — Saga
- `AK-095` — AsyncAPI Event Contracts
- `AK-097` — DLQ / Messaging Operations

## 18. Merksatz

> EDA kauft zeitliche Entkopplung mit zusätzlicher Zustands-, Vertrags- und Betriebsverantwortung. Kafka ist dann wertvoll, wenn diese Entkopplung ein reales Problem löst und Delivery, Ordering, Ownership und Fehlerbetrieb explizit gestaltet sind.
