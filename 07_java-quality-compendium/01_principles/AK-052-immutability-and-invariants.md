---
id: AK-052
legacy_ids:
  - ADR-052
title: Immutability, Invarianten, Ownership und defensive Grenzen
artifact_type: architecture-principle
domain: software-design
status: active
maturity: reviewed
normative_level: recommended
last_validated: 2026-10-03
technology_baseline:
  java: "21+"
review_trigger:
  - grundlegende Änderung der Java-Baseline
  - relevante Änderung der Java-Spezifikation zu Records, Collections oder final-Field-Semantik
  - neue belastbare Forschung zu Aliasing, Ownership oder Object Invariants
---

# AK-052 — Immutability, Invarianten, Ownership und defensive Grenzen

## 1. Zweck dieses Dokuments

Mutable State ist kein Fehler an sich.

Viele fachliche Objekte modellieren gerade dadurch Realität, dass sich ihr Zustand kontrolliert verändert:

- ein Antrag wird eingereicht,
- ein Vorgang wird geprüft,
- eine Zahlung wird verbucht,
- ein Auftrag wird storniert,
- ein Fall wechselt seinen Bearbeitungsstatus.

Problematisch wird Mutation dort, wo nicht mehr klar ist:

- wer einen Zustand besitzt,
- wer ihn ändern darf,
- welche Regeln dabei gelten,
- welche Aliase auf denselben Zustand existieren,
- wann ein Objekt als gültig betrachtet werden darf,
- und welche anderen Teile des Systems von einer Änderung betroffen sind.

Dieses Dokument behandelt Immutability deshalb nicht als Stilregel.

Die zentrale Frage lautet nicht:

> Ist dieses Objekt immutable?

Sondern:

> **Wie wird sichergestellt, dass Zustand nur durch erlaubte Operationen verändert wird, Invarianten erhalten bleiben und fremde Aliase den Zustand nicht unbemerkt manipulieren können?**

Immutability ist dafür ein sehr starkes Werkzeug.

Sie ist aber nur eine von mehreren Strategien.

Weitere Strategien sind:

- Encapsulation,
- defensive Kopien,
- explizite Ownership,
- fachliche Änderungsoperationen,
- transaktionale Grenzen,
- Versionierung,
- Concurrency Control,
- unveränderliche Snapshots,
- und klar definierte Verträge.

---

# Teil I — Zustand, Gültigkeit und Invarianten

## 2. Was ist Zustand?

Der Zustand eines Objekts umfasst die Informationen, die sein zukünftiges Verhalten beeinflussen.

Beispiel:

~~~java
public final class Application {

    private ApplicationStatus status;
    private Instant submittedAt;
    private Decision decision;
}
~~~

Der Zustand besteht hier nicht nur aus einzelnen Feldwerten.

Relevant sind auch Beziehungen zwischen ihnen.

Zum Beispiel:

~~~text
status = SUBMITTED
→ submittedAt muss gesetzt sein

status = DECIDED
→ decision muss vorhanden sein
~~~

Ein Objekt kann daher syntaktisch vollständig initialisiert und trotzdem fachlich ungültig sein.

---

## 3. Was ist eine Invariante?

Eine Invariante ist eine Bedingung, die für einen gültigen beobachtbaren Zustand eines Objekts oder einer Abstraktion gelten muss.

Beispiele:

~~~text
Money.amount >= 0
~~~

oder:

~~~text
period.start <= period.end
~~~

oder:

~~~text
status = APPROVED
→ approvedBy != null
→ approvedAt != null
~~~

oder:

~~~text
tenantId eines fachlichen Objekts
darf nach seiner Erzeugung nicht beliebig wechseln
~~~

Invarianten beschreiben damit nicht einzelne Methoden.

Sie beschreiben Eigenschaften, die die **Gültigkeit des Zustands** definieren.

---

## 4. Design by Contract als theoretischer Bezug

Bertrand Meyer ordnet Invarianten zusammen mit Vor- und Nachbedingungen in Design by Contract ein.

Vereinfacht:

~~~text
Precondition
→ was der Aufrufer gewährleisten muss

Operation

Postcondition
→ was die Operation danach gewährleistet

Invariant
→ was für gültige beobachtbare Zustände der Abstraktion gelten muss
~~~

Beispiel:

~~~java
public void cancel(CancellationReason reason) {
    ...
}
~~~

Precondition:

~~~text
reason != null
status erlaubt Cancellation
~~~

Postcondition:

~~~text
status = CANCELLED
cancellationReason = reason
cancelledAt != null
~~~

Invariant:

~~~text
status = CANCELLED
→ cancellationReason != null
→ cancelledAt != null
~~~

Die praktische Stärke liegt darin, Zustand nicht nur als Sammlung von Feldern zu betrachten, sondern als **Vertrag über gültige Zustände und erlaubte Zustandsübergänge**.

---

## 5. Invarianten gelten an stabilen Grenzen

Bei einer komplexen Mutation kann ein Objekt intern kurzfristig einen Zwischenzustand besitzen.

Beispiel:

~~~java
this.status = CANCELLED;
this.reason = reason;
this.cancelledAt = clock.instant();
~~~

Zwischen den einzelnen Zuweisungen ist die vollständige Invariante technisch noch nicht hergestellt.

Deshalb ist eine wichtige Designidee:

> Invarianten müssen an den beobachtbaren stabilen Grenzen einer Operation gelten.

Das setzt voraus, dass fremder Code während eines Zwischenzustands nicht unkontrolliert zugreifen kann.

Damit entsteht unmittelbar die Verbindung zu:

- Encapsulation,
- Reference Leaks,
- Callbacks,
- Concurrency,
- Transaktionen.

---

## 6. Ungültige Zustände möglichst schwer repräsentierbar machen

Ein schwaches Modell:

~~~java
public final class Period {

    private LocalDate start;
    private LocalDate end;

    public void setStart(LocalDate start) {
        this.start = start;
    }

    public void setEnd(LocalDate end) {
        this.end = end;
    }
}
~~~

Erlaubt:

~~~text
start = 2026-10-31
end   = 2026-01-01
~~~

Ein stärkeres Modell:

~~~java
public record Period(LocalDate start, LocalDate end) {

    public Period {
        Objects.requireNonNull(start);
        Objects.requireNonNull(end);

        if (end.isBefore(start)) {
            throw new IllegalArgumentException(
                    "end must not be before start");
        }
    }
}
~~~

Hier kann die zentrale Invariante bereits bei Konstruktion durchgesetzt werden.

---

## 7. Typen können Invarianten tragen

Statt:

~~~java
String tenantId;
String caseId;
String currency;
int quantity;
~~~

können eigene Typen Bedeutung und Invarianten kapseln:

~~~java
public record Quantity(int value) {

    public Quantity {
        if (value <= 0) {
            throw new IllegalArgumentException(
                    "quantity must be positive");
        }
    }
}
~~~

~~~java
public record CaseId(UUID value) {

    public CaseId {
        Objects.requireNonNull(value);
    }
}
~~~

Danach kann Code, der eine Quantity akzeptiert, davon ausgehen, dass die zentrale Quantity-Invariante bereits geprüft wurde.

Das reduziert wiederholte defensive Checks im Inneren des Systems.

---

# Teil II — Mutation, Aliasing und Ownership

## 8. Mutation ist nicht das einzige Problem

Betrachten wir:

~~~java
List<String> roles = new ArrayList<>();

SecurityContext context = new SecurityContext(roles);
~~~

Wenn SecurityContext dieselbe List-Referenz speichert, existieren mindestens zwei Zugriffswege auf denselben Zustand:

~~~text
roles -----------------+
                        |
                        v
                  mutable List
                        ^
                        |
SecurityContext --------+
~~~

Dann kann fremder Code ausführen:

~~~java
roles.add("ADMIN");
~~~

ohne SecurityContext selbst aufzurufen.

Das Problem ist damit nicht nur Mutation.

Das Problem ist:

> **Mutation plus Aliasing plus unklare Ownership.**

---

## 9. Was ist Aliasing?

Aliasing bedeutet, dass mehrere Referenzen auf dasselbe mutable Objekt zeigen.

Beispiel:

~~~java
List<String> source = new ArrayList<>();
List<String> alias = source;

alias.add("x");
~~~

Danach hat sich auch der über source beobachtete Zustand verändert.

Aliasing ist in objektorientierten Programmen normal und oft notwendig.

Es wird problematisch, wenn ein Objekt glaubt, eine Invariante über seinen internen Zustand kontrollieren zu können, obwohl fremde Referenzen denselben Zustand verändern dürfen.

---

## 10. Warum Aliasing Invarianten erschwert

Angenommen:

~~~java
public final class RoleSet {

    private final Set<Role> roles;

    public RoleSet(Set<Role> roles) {
        this.roles = roles;
    }

    public boolean contains(Role role) {
        return roles.contains(role);
    }
}
~~~

Die Klasse besitzt keine Setter.

Trotzdem ist sie nicht wirklich vor Mutation geschützt.

Der Aufrufer kann weiterhin:

~~~java
Set<Role> roles = new HashSet<>();
RoleSet set = new RoleSet(roles);

roles.add(Role.ADMIN);
~~~

ausführen.

Damit wurde die beobachtbare Semantik von RoleSet verändert, ohne dass RoleSet beteiligt war.

---

## 11. Forschung zu Ownership und Reference Leaks

Aliasing und Ownership sind seit Jahrzehnten Gegenstand der Programmiersprachen- und Verifikationsforschung.

Ownership-Type-Ansätze adressieren genau das Problem, dass ein Aggregat seine Invarianten nur kontrollieren kann, wenn Zugriffswege auf seine internen mutable Bestandteile ausreichend eingeschränkt sind.

Die praktische Lehre für Java lautet nicht:

> Wir müssen ein Ownership-Type-System nachbauen.

Sondern:

> **Bei jedem mutable Objektgraphen muss klar sein, welche Einheit Zustand besitzt und welche Referenzen diese Ownership-Grenze überschreiten dürfen.**

---

## 12. Ownership als Designfrage

Für jeden relevanten mutable Zustand sollte beantwortbar sein:

1. Wer erzeugt ihn?
2. Wer besitzt ihn?
3. Wer darf ihn mutieren?
4. Wer darf nur lesen?
5. Darf er die Ownership-Grenze verlassen?
6. Wenn ja: als Live-Referenz, View, Snapshot oder Kopie?
7. Welche Invarianten müssen bei Mutation erhalten bleiben?

Diese Fragen sind oft wertvoller als die pauschale Forderung:

> Alles immutable.

---

# Teil III — Begriffe präzise unterscheiden

## 13. Immutable Object

Ein Objekt ist im praktischen Sinn immutable, wenn sein beobachtbarer logischer Zustand nach abgeschlossener Konstruktion nicht mehr verändert werden kann.

Wichtig ist:

> Nicht nur die Feldreferenzen, sondern die beobachtbare Semantik zählt.

---

## 14. Shallow Immutability

Shallow Immutability bedeutet:

- die direkten Komponenten oder Referenzen werden nicht ausgetauscht,
- referenzierte Objekte können aber selbst mutable sein.

Beispiel:

~~~java
public record Snapshot(List<String> values) {}
~~~

Die Record-Komponente ist final.

Die referenzierte List kann trotzdem veränderlich sein.

Java beschreibt Record Classes ausdrücklich als **shallowly immutable**.

---

## 15. Deep Immutability

Deep Immutability bedeutet konzeptionell:

> Der gesamte vom Objekt semantisch besessene Objektgraph kann nach Konstruktion nicht mehr verändert werden.

Beispiel:

~~~text
OrderSnapshot
  |
  +--> immutable List
          |
          +--> immutable OrderItem
          +--> immutable OrderItem
~~~

In Java existiert dafür kein allgemeiner eingebauter Deep-Immutability-Typmechanismus.

Deep Immutability muss durch Typwahl, Kopien, Encapsulation und Konventionen erreicht werden.

---

## 16. Unmodifiable ist nicht dasselbe wie immutable

Ein unmodifiable Collection-Interface verhindert Mutation **über diese Referenz**.

Das garantiert nicht automatisch, dass:

- die enthaltenen Elemente immutable sind,
- keine andere Referenz die zugrunde liegende Collection verändert,
- die beobachtbaren Inhalte nie wechseln.

Beispiel:

~~~java
List<String> mutable = new ArrayList<>();
List<String> view =
        Collections.unmodifiableList(mutable);

mutable.add("new");
~~~

view sieht danach den neuen Eintrag.

Die View ist unmodifiable.

Der zugrunde liegende Zustand ist nicht immutable.

---

## 17. Defensive Copy ist etwas anderes als View

Bei einer defensiven Kopie:

~~~java
this.items = List.copyOf(items);
~~~

ist die resultierende List strukturell von späteren Änderungen der ursprünglichen Collection entkoppelt.

Die Java-API spezifiziert für List.copyOf:

- die resultierende Liste ist unmodifiable,
- spätere Änderungen der Input-Collection werden nicht in der resultierenden Liste sichtbar.

Aber:

> Die Elemente selbst werden nicht tief kopiert.

---

## 18. final ist nicht immutable

~~~java
final List<String> values =
        new ArrayList<>();

values.add("x");
~~~

ist vollständig legal.

final bedeutet hier:

> Die Variable values darf nicht auf eine andere List-Instanz zeigen.

Es bedeutet nicht:

> Die List kann nicht verändert werden.

---

## 19. Effectively Immutable

Manche Objekte werden nach ihrer Initialisierung praktisch nicht mehr verändert, obwohl das Typsystem dies nicht erzwingt.

Beispiel:

~~~java
class Configuration {
    Map<String, String> values;
}
~~~

wenn nach Startup niemand values ändert.

Das kann funktionieren.

Es ist aber schwächer als strukturell erzwungene Immutability, weil die Garantie:

- konventionsbasiert,
- schwerer überprüfbar,
- leichter versehentlich verletzbar

ist.

---

## 20. Snapshot

Ein Snapshot ist eine unveränderliche Repräsentation eines Zustands zu einem bestimmten Zeitpunkt.

Beispiel:

~~~text
Case Aggregate
    |
    +--> mutable current state

CaseSnapshot
    |
    +--> immutable representation at T1
~~~

Snapshots sind besonders nützlich für:

- Events,
- Audits,
- Caches,
- Read Models,
- asynchrone Verarbeitung,
- Tests.

---

# Teil IV — Java Records richtig einordnen

## 21. Was ein Record garantiert

Eine Java Record Class ist ein transparenter Datenträger für einen festen Satz von Komponenten.

Für jede Komponente existiert unter anderem ein privates final Field.

Die Java-API beschreibt Records als **shallowly immutable**.

Das ist eine wichtige und absichtlich begrenzte Aussage.

---

## 22. Record mit mutable Collection

~~~java
public record OrderSnapshot(
        List<OrderItem> items) {
}
~~~

Problem:

~~~java
List<OrderItem> items =
        new ArrayList<>();

OrderSnapshot snapshot =
        new OrderSnapshot(items);

items.clear();
~~~

Der Record selbst hat seine List-Referenz nicht verändert.

Sein beobachtbarer Inhalt hat sich trotzdem geändert.

---

## 23. Defensive Kopie im Compact Constructor

~~~java
public record OrderSnapshot(
        List<OrderItem> items) {

    public OrderSnapshot {
        Objects.requireNonNull(items);
        items = List.copyOf(items);
    }
}
~~~

Damit wird die Collection-Struktur von späterer Mutation des Inputs entkoppelt.

---

## 24. Aber die Elemente können mutable bleiben

~~~java
public final class MutableOrderItem {

    private int quantity;

    public void setQuantity(int quantity) {
        this.quantity = quantity;
    }
}
~~~

Dann:

~~~java
List<MutableOrderItem> source =
        new ArrayList<>();

OrderSnapshot snapshot =
        new OrderSnapshot(source);
~~~

List.copyOf schützt die List-Struktur.

Es schützt nicht MutableOrderItem.

---

## 25. Arrays sind besonders leicht zu übersehen

Ein Record:

~~~java
public record Payload(byte[] bytes) {}
~~~

ist shallow immutable.

Der Array-Inhalt bleibt mutable.

Robuster:

~~~java
public record Payload(byte[] bytes) {

    public Payload {
        bytes = bytes.clone();
    }

    @Override
    public byte[] bytes() {
        return bytes.clone();
    }
}
~~~

Hier muss sowohl beim Eingang als auch beim Ausgang über Ownership nachgedacht werden.

---

## 26. Record bedeutet nicht automatisch Value Object

Ein Record eignet sich oft gut für Value Objects.

Aber die Syntax allein erzeugt keine gute fachliche Modellierung.

~~~java
public record Customer(
        String id,
        String name,
        String status,
        String tenant) {}
~~~

kann fachlich schwach sein, wenn:

- Strings unvalidierte Konzepte vermischen,
- Invarianten fehlen,
- Zustandsübergänge eigentlich Verhalten benötigen,
- Identität und Lifecycle relevant sind.

Record ist eine Sprachfunktion.

Value Object ist eine Modellierungsentscheidung.

---

# Teil V — Defensive Grenzen

## 27. Defensive Copy beim Eingang

Wenn ein Objekt Ownership über eine mutable Collection übernimmt, sollte die Grenze bewusst gestaltet werden.

~~~java
public final class Route {

    private final List<Stop> stops;

    public Route(List<Stop> stops) {
        this.stops = List.copyOf(stops);
    }
}
~~~

Damit verhindert Route, dass der ursprüngliche List-Container später extern verändert wird.

---

## 28. Defensive Copy beim Ausgang

Wenn ein interner mutable Zustand geschützt werden muss:

Schwach:

~~~java
public List<Item> items() {
    return items;
}
~~~

Fremder Code kann mutieren.

Mögliche Alternative:

~~~java
public List<Item> items() {
    return List.copyOf(items);
}
~~~

oder bei intern bereits unmodifiable gespeichertem Zustand:

~~~java
public List<Item> items() {
    return items;
}
~~~

Die richtige Wahl hängt von der internen Repräsentation und den Elementtypen ab.

---

## 29. Defensive Copy ist keine Religion

Copies haben Kosten:

- Speicher,
- Allocation,
- CPU,
- Garbage Collection,
- eventuell hohe Kosten bei großen Graphen.

Deshalb:

> Defensive Copy dort, wo sie eine reale Ownership- oder Invariantengrenze schützt.

Nicht:

> Jede Collection bei jedem Methodenaufruf kopieren.

---

## 30. Trust Boundary und Ownership Boundary

Eine defensive Grenze ist besonders relevant bei:

- externen API-Eingaben,
- Plugin-Schnittstellen,
- Drittbibliotheken,
- Caches,
- Security Contexts,
- Thread-Grenzen,
- asynchronen Nachrichten,
- öffentlichen Domain-APIs.

Innerhalb eines eng kontrollierten privaten Scopes kann eine Kopie unnötig sein.

---

# Teil VI — Mutation als fachliche Operation

## 31. Setter sind häufig zu schwach

~~~java
application.setStatus(APPROVED);
~~~

sagt wenig über:

- erlaubte Vorgängerzustände,
- Autorisierung,
- Zeitstempel,
- Begründung,
- Audit,
- Seiteneffekte.

---

## 32. Fachliche Operation

~~~java
application.approve(
        decisionMaker,
        reason,
        clock);
~~~

kann:

- den aktuellen Zustand prüfen,
- Actor prüfen,
- Reason verlangen,
- Timestamp setzen,
- Domain Event erzeugen,
- Auditdaten aktualisieren.

Die Mutation bleibt vorhanden.

Aber sie ist **semantisch gekapselt**.

---

## 33. Beispiel einer Zustandsinvariante

~~~java
public final class Application {

    private ApplicationStatus status;
    private DecisionMetadata decision;

    public void approve(
            UserId actor,
            ApprovalReason reason,
            Clock clock) {

        if (status != SUBMITTED) {
            throw new IllegalStateException(
                    "only submitted applications may be approved");
        }

        this.decision =
                DecisionMetadata.approved(
                        actor,
                        reason,
                        clock.instant());

        this.status = APPROVED;
    }
}
~~~

Die Klasse kann jetzt selbst dafür sorgen, dass:

~~~text
APPROVED
→ DecisionMetadata vorhanden
~~~

gilt.

---

## 34. Zustandsmaschine explizit machen

Bei mehreren erlaubten Übergängen hilft eine Zustandsmatrix:

| Von | Nach | erlaubt? |
|---|---|---|
| DRAFT | SUBMITTED | ja |
| DRAFT | APPROVED | nein |
| SUBMITTED | APPROVED | ja |
| SUBMITTED | REJECTED | ja |
| APPROVED | DRAFT | nein |

Der Code sollte diese fachliche Semantik ausdrücken.

Nicht nur technische Feldmutation.

---

## 35. Mutation innerhalb einer Transaktion

Ein fachlicher Zustandsübergang kann zusätzlich Persistenzkonsistenz benötigen.

Beispiel:

~~~text
approve application
+
persist decision
+
write audit entry
~~~

Immutability allein löst diese atomare Geschäftsoperation nicht.

Hier sind zusätzlich relevant:

- Transaction Boundaries,
- Optimistic Locking,
- Outbox oder andere Konsistenzmechanismen,
- Idempotenz.

---

# Teil VII — Value Objects und Entities

## 36. Value Objects

Value Objects werden über ihre Werte und nicht über dauerhafte Identität beschrieben.

Typische Beispiele:

- Money,
- Period,
- PostalCode,
- CaseId,
- Quantity,
- EmailAddress.

Immutability ist für Value Objects besonders hilfreich, weil:

- Equality stabil bleibt,
- Hashing stabil bleibt,
- Sharing sicherer wird,
- Aliasing weniger riskant ist.

---

## 37. Beispiel Money

~~~java
public record Money(
        BigDecimal amount,
        Currency currency) {

    public Money {
        Objects.requireNonNull(amount);
        Objects.requireNonNull(currency);
    }

    public Money add(Money other) {
        requireSameCurrency(other);

        return new Money(
                amount.add(other.amount),
                currency);
    }
}
~~~

Die Operation verändert Money nicht.

Sie erzeugt einen neuen Wert.

---

## 38. Entities

Entities besitzen Identität und Lifecycle.

Beispiel:

~~~text
Application 4711
~~~

kann im Laufe der Zeit:

~~~text
DRAFT
→ SUBMITTED
→ UNDER_REVIEW
→ APPROVED
~~~

durchlaufen.

Eine Entity muss deshalb nicht vollständig immutable sein.

Die wichtigere Forderung lautet:

> Mutation muss Ownership und Invarianten respektieren.

---

## 39. Aggregate als Mutationsgrenze

Im DDD-Kontext kann ein Aggregate Root kontrollieren, wie sein interner Objektgraph verändert wird.

Beispiel:

~~~text
Application
  |
  +--> ApplicantData
  +--> Documents
  +--> Decision
~~~

Fremder Code sollte nicht beliebig tief einzelne mutable Elemente manipulieren können, wenn dadurch Aggregate-Invarianten verletzt werden.

---

## 40. Immutable Value Object in mutable Entity

Eine häufig robuste Kombination:

~~~text
mutable Entity
    |
    +--> immutable CaseId
    +--> immutable Period
    +--> immutable Money
    +--> immutable DecisionReason
~~~

Die Entity verändert ihren Lifecycle.

Ihre fachlichen Werte bleiben lokal einfach und stabil.

---

# Teil VIII — Java final und das Memory Model

## 41. final Field Semantics

final Fields besitzen im Java Memory Model besondere Semantik.

Die Java Language Specification beschreibt, dass korrekt verwendete final Fields die Implementierung thread-sicherer immutable Objekte ohne zusätzliche Synchronisation unterstützen können.

Das ist stärker als:

> final verhindert nur Zuweisung.

Bei Feldern spielt final auch für Sichtbarkeitsgarantien nach Konstruktion eine Rolle.

---

## 42. Konstruktion muss korrekt abgeschlossen sein

Die Vorteile der final-Field-Semantik setzen eine saubere Konstruktion voraus.

Problematisch ist, wenn this während der Konstruktion entkommt.

Beispiel:

~~~java
public final class Listener {

    private final String configuration;

    public Listener(EventBus bus) {
        bus.register(this);
        this.configuration = load();
    }
}
~~~

Jetzt kann ein anderer Thread das Objekt theoretisch sehen, bevor die Konstruktion fachlich abgeschlossen ist.

Die praktische Regel lautet:

> Keine Referenz auf das noch nicht vollständig konstruierte Objekt aus dem Constructor veröffentlichen.

---

## 43. final garantiert keinen immutable Objektgraphen

~~~java
public final class UserContext {

    private final Map<String, String> attributes;
}
~~~

final schützt die Referenz.

Wenn attributes mutable ist und geteilt wird, bleibt der referenzierte Zustand veränderlich.

---

## 44. Safe Publication nicht mit Business Correctness verwechseln

Selbst ein korrekt publiziertes immutable Objekt löst nicht:

- Lost Updates,
- Double Processing,
- konkurrierende Datenbankupdates,
- Reihenfolgeprobleme,
- Idempotenz,
- Transaktionskonflikte.

Java-Memory-Visibility und fachliche Concurrency Correctness sind unterschiedliche Ebenen.

---

# Teil IX — Immutability und Concurrency

## 45. Warum immutable Werte helfen

Wenn Zustand nicht geändert werden kann, entfallen bestimmte Race Conditions.

Zwei Threads können denselben immutable Wert lesen, ohne um seine Mutation zu konkurrieren.

Beispiel:

~~~java
record PricingSnapshot(
        Money basePrice,
        TaxRate taxRate,
        Instant validAt) {}
~~~

Mehrere Threads können denselben Snapshot verwenden.

---

## 46. Shared Mutable State bleibt schwieriger

~~~text
Thread A ──┐
           ├──> mutable object
Thread B ──┘
~~~

Dann muss geklärt werden:

- Synchronisation,
- Atomizität,
- Visibility,
- Locking,
- Reihenfolge,
- Failure Handling.

Immutability reduziert den Raum möglicher Interleavings.

---

## 47. Immutable Message zwischen Threads

Ein sinnvolles Modell:

~~~text
Thread A
   |
   +--> immutable Command/Event
              |
              v
           Thread B
~~~

Der Empfänger sieht eine stabile Nachricht.

Er muss nicht fürchten, dass der Sender dieselbe Nachricht nach dem Versand verändert.

---

## 48. Aber Collections und Elemente prüfen

~~~java
record BatchCommand(
        List<MutableJob> jobs) {}
~~~

ist trotz Record kein garantiert stabiler Message-Snapshot.

Bei Thread- oder Prozessgrenzen sollte besonders auf tiefe Mutability geachtet werden.

---

## 49. Snapshot statt Live Object

Für asynchrone Verarbeitung ist oft besser:

~~~text
mutable Aggregate
      |
      +--> immutable Event Snapshot
                |
                v
           Message Broker
~~~

als eine Live-Referenz auf veränderlichen Zustand.

---

# Teil X — Collections und APIs

## 50. Collections.unmodifiableList

Collections.unmodifiableList erzeugt eine unmodifiable View.

Wenn die zugrunde liegende Collection über einen anderen Alias verändert wird, sieht die View diese Änderung.

Das kann sinnvoll sein.

Es ist aber kein Ownership Transfer.

---

## 51. List.copyOf

List.copyOf erzeugt eine unmodifiable List mit den Elementen der Input-Collection.

Spätere strukturelle Änderungen der ursprünglichen Collection erscheinen nicht in der resultierenden List.

Wichtig:

- keine tiefen Kopien der Elemente,
- null-Elemente sind nicht erlaubt,
- eine bereits geeignete unmodifiable List kann intern wiederverwendet werden.

Daraus folgt:

> Nicht auf Objektidentität der Kopie verlassen.

---

## 52. Optional Mutability der Elemente dokumentieren

Ein API-Vertrag:

~~~java
List<OrderItem> items()
~~~

sagt nicht, ob OrderItem mutable ist.

Für kritische APIs muss die Semantik über Typmodell und Dokumentation klar sein.

---

## 53. Stream macht Daten nicht immutable

~~~java
items.stream()
~~~

liefert eine Verarbeitungssicht.

Es ändert nicht die Mutability der Elemente.

Funktionaler Stil und Immutability sind verwandt, aber nicht identisch.

---

# Teil XI — Hashing, Equality und Caches

## 54. Mutable Key in HashMap

Gefährlich:

~~~java
Map<CustomerKey, Data> cache =
        new HashMap<>();
~~~

wenn CustomerKey-Felder, die equals/hashCode bestimmen, nach Einfügen verändert werden können.

Dann kann ein Eintrag logisch "verschwinden", weil der Hashbucket nicht mehr zur neuen Hash-Repräsentation passt.

---

## 55. Value Keys sollten stabil sein

IDs, Cache Keys und Map Keys profitieren besonders von Immutability.

Beispiel:

~~~java
public record CacheKey(
        TenantId tenant,
        CaseId caseId) {}
~~~

wenn TenantId und CaseId selbst stabile Value Types sind.

---

## 56. Caches brauchen zeitliche Semantik

Ein immutable Cache Value ist nur stabil bezüglich Mutation.

Er kann trotzdem fachlich veraltet sein.

Deshalb zusätzlich klären:

- TTL,
- Version,
- Invalidierung,
- Staleness,
- Source of Truth.

Immutability und Aktualität sind unterschiedliche Eigenschaften.

---

# Teil XII — Persistenz und JPA

## 57. JPA erzeugt besondere Spannungen

JPA-Entities sind häufig mutable, weil:

- Persistenzframeworks Lifecycle und Dirty Checking unterstützen,
- Beziehungen verwaltet werden,
- Proxies und Konstruktion besondere Anforderungen erzeugen können.

Daraus folgt nicht:

> JPA-Entities müssen öffentliche Setter für alles besitzen.

---

## 58. Kapselung trotz JPA

Statt:

~~~java
public void setStatus(Status status) {
    this.status = status;
}
~~~

kann die Entity fachliche Methoden besitzen:

~~~java
public void approve(
        UserId actor,
        ApprovalReason reason,
        Instant at) {
    ...
}
~~~

Persistenz-Mutability und Domain-API sind zwei unterschiedliche Fragen.

---

## 59. Persistence Model versus Domain Model

Je nach Komplexität kann ein separates Domain Model sinnvoll sein.

~~~text
Persistence Entity
      |
      +--> Mapping
              |
              v
        Domain Model
~~~

Das erhöht Mapping-Kosten.

Es kann aber Ownership und Invarianten sauberer machen.

Keine Seite ist universell richtig.

---

## 60. Optimistic Locking bleibt relevant

Immutable Value Objects verhindern keine konkurrierenden Updates einer Entity in der Datenbank.

Beispiel:

~~~text
User A liest Version 7
User B liest Version 7

A speichert Version 8
B überschreibt ohne Kontrolle
~~~

Hier braucht es gegebenenfalls:

- Version Columns,
- Compare-and-Swap,
- fachliche Konfliktbehandlung.

---

# Teil XIII — Security und Trust Boundaries

## 61. Sicherheitsrelevante Claims

Nach erfolgreicher Authentisierung können validierte Claims als immutable Request Context modelliert werden.

Beispiel:

~~~java
public record SecurityContext(
        SubjectId subject,
        TenantId tenant,
        Set<Role> roles) {

    public SecurityContext {
        roles = Set.copyOf(roles);
    }
}
~~~

Wenn Role selbst immutable ist, reduziert dies versehentliche Manipulation innerhalb des Request-Lifecycles.

---

## 62. Immutability ersetzt keine Autorisierung

Ein immutable RoleSet verhindert nicht, dass:

- die falschen Rollen geladen wurden,
- eine Authorization Rule falsch ist,
- Tenant Isolation fehlt.

Immutability schützt Zustand.

Authorization bewertet Berechtigung.

---

## 63. Path und Identifier als validierte Typen

Statt wiederholt Strings zu validieren:

~~~java
String path
String tenant
String externalId
~~~

können normalisierte Value Types eingesetzt werden.

Damit wird ein Teil der Security- und Input-Invarianten näher an die Entstehungsgrenze gebracht.

---

## 64. TOCTOU-Probleme nicht verwechseln

Ein immutable Snapshot kann zwischen:

~~~text
check
und
use
~~~

stabil sein.

Aber wenn die reale Autorität in einem externen System inzwischen geändert wurde, kann ein alter Snapshot fachlich veraltet sein.

Immutability löst kein Time-of-check-to-time-of-use-Problem über externe Wahrheiten.

---

# Teil XIV — Performance und Kosten

## 65. "Immutable ist zu langsam" ist keine ausreichende Aussage

Immutability kann zusätzliche Objekterzeugung oder Kopien verursachen.

Ob das relevant ist, hängt von:

- Datenmenge,
- Objektgröße,
- Allocation Rate,
- Lebensdauer,
- GC-Verhalten,
- Hot Path

ab.

Die Entscheidung sollte gemessen werden.

---

## 66. Kleine Value Objects sind häufig günstig

Moderne JVMs optimieren kurzlebige Objekte stark.

Aber daraus folgt kein Freibrief.

Bei:

- großen Arrays,
- riesigen Collections,
- Image Buffers,
- Hochfrequenz-Processing

können Copy-Kosten relevant werden.

---

## 67. Structural Sharing

Persistente Datenstrukturen können neue logische Versionen erzeugen, ohne den gesamten Graphen zu kopieren.

Das Konzept:

~~~text
Version 1 ----+
              +--> shared structure
Version 2 ----+
        |
        +--> changed branch
~~~

Java Standard Collections sind überwiegend nicht als persistent immutable Collections ausgelegt.

Externe Libraries können andere Modelle anbieten.

---

## 68. Builder als kontrollierte mutable Konstruktionsphase

Ein sinnvolles Muster:

~~~text
mutable Builder
      |
      +--> validation
              |
              v
        immutable Result
~~~

Die Mutability wird auf die Konstruktionsphase begrenzt.

---

## 69. Performance-Evidence

Wenn defensive Kopien als Problem vermutet werden:

1. Hot Path identifizieren.
2. Allocation messen.
3. Objektgrößen bestimmen.
4. Profiling durchführen.
5. Alternative evaluieren.
6. Invariantenschutz nicht ohne Messung entfernen.

---

# Teil XV — Fachlicher Praxisfall

## 70. Ausgangslage: Antragsentscheidung

Eine Anwendung verwaltet einen Antrag.

Schwaches Modell:

~~~java
public final class Application {

    private String status;
    private String decision;
    private String decidedBy;
    private List<String> documents;

    public void setStatus(String status) {
        this.status = status;
    }

    public void setDecision(String decision) {
        this.decision = decision;
    }

    public void setDecidedBy(String decidedBy) {
        this.decidedBy = decidedBy;
    }

    public List<String> getDocuments() {
        return documents;
    }
}
~~~

Dieses Modell erlaubt viele ungültige Zustände.

---

## 71. Ungültige Kombinationen

Möglich sind:

~~~text
status = APPROVED
decision = null
~~~

oder:

~~~text
status = DRAFT
decision = APPROVED
~~~

oder:

~~~text
status = APPROVED
decidedBy = null
~~~

Zusätzlich kann fremder Code:

~~~java
application.getDocuments().clear();
~~~

ausführen.

---

## 72. Value Types einführen

~~~java
public record DecisionMetadata(
        DecisionType type,
        UserId decidedBy,
        Instant decidedAt,
        DecisionReason reason) {

    public DecisionMetadata {
        Objects.requireNonNull(type);
        Objects.requireNonNull(decidedBy);
        Objects.requireNonNull(decidedAt);
        Objects.requireNonNull(reason);
    }
}
~~~

Die zusammengehörigen Entscheidungsdaten werden zu einem validen Wert gebündelt.

---

## 73. Dokumente defensiv kapseln

~~~java
public final class Application {

    private final List<DocumentRef> documents;

    public Application(
            List<DocumentRef> documents) {

        this.documents =
                List.copyOf(documents);
    }

    public List<DocumentRef> documents() {
        return documents;
    }
}
~~~

Voraussetzung für stabile tiefe Semantik:

DocumentRef selbst sollte passend modelliert sein.

---

## 74. Fachliche Mutation

~~~java
public void approve(
        UserId actor,
        DecisionReason reason,
        Clock clock) {

    if (status != SUBMITTED) {
        throw new IllegalStateException(
                "application must be submitted");
    }

    this.decision =
            new DecisionMetadata(
                    APPROVED,
                    actor,
                    clock.instant(),
                    reason);

    this.status = APPROVED;
}
~~~

Jetzt ist der zentrale Zustandsübergang gekapselt.

---

## 75. Snapshot für Event

Nach Approval:

~~~java
public record ApplicationApproved(
        ApplicationId applicationId,
        UserId decidedBy,
        Instant decidedAt,
        DecisionReason reason,
        List<DocumentRef> documents) {

    public ApplicationApproved {
        documents = List.copyOf(documents);
    }
}
~~~

Das Event beschreibt einen stabilen Zustand zum Zeitpunkt der Entscheidung.

---

## 76. Was dadurch erreicht wurde

~~~text
Mutable Lifecycle
      |
      +--> gekapselte Domain-Operationen
      |
      +--> immutable Value Objects
      |
      +--> defensive Collection-Grenzen
      |
      +--> immutable Event Snapshot
~~~

Nicht alles wurde immutable.

Aber Ownership und Invarianten sind klarer.

---

# Teil XVI — Architekturübertragung

## 77. Immutability auf API-Ebene

Request- und Response-Modelle profitieren häufig von Wertsemantik.

Ein Request sollte nach Validierung nicht an vielen Stellen stillschweigend verändert werden.

Möglicher Flow:

~~~text
external DTO
   |
validation
   |
normalized command
   |
domain operation
~~~

---

## 78. Events sind Fakten

Ein publiziertes Domain Event repräsentiert typischerweise ein bereits geschehenes Faktum.

Deshalb sollte sein semantischer Inhalt nach Publikation nicht verändert werden.

~~~text
ApplicationApproved v1
~~~

ist historisches Faktum.

Eine spätere Änderung erzeugt ein neues Ereignis oder eine neue Version, nicht eine Mutation des alten Events.

---

## 79. Konfiguration

Konfiguration eignet sich häufig als immutable Snapshot.

~~~text
load
→ validate
→ freeze
→ publish snapshot
~~~

Bei dynamischer Konfiguration:

~~~text
ConfigSnapshot V1
→ ConfigSnapshot V2
~~~

statt:

~~~text
eine globale mutable Map,
die von vielen Threads verändert wird
~~~

---

## 80. Datenprodukte und Snapshots

Auch auf Enterprise-Ebene ist die Unterscheidung wichtig:

~~~text
Live operational state
vs.
published immutable snapshot
~~~

Ein Snapshot kann für:

- Reporting,
- Audit,
- Datenaustausch,
- Reproduzierbarkeit

wertvoll sein.

Er ersetzt aber keine Source-of-Truth-Strategie.

---

## 81. Behördenkontext

In Behörden sind besonders relevant:

- nachvollziehbare Entscheidungen,
- historische Aktenstände,
- zeitbezogene Gültigkeit,
- Identitäts- und Rolleninformationen,
- Nachweis, welche Daten einer Entscheidung zugrunde lagen.

Immutable Snapshots können hier unterstützen.

Beispiel:

~~~text
Entscheidung zum Zeitpunkt T
      |
      +--> Regelversion
      +--> relevante Inputdaten
      +--> Actor
      +--> Timestamp
      +--> Result
~~~

Aber:

> Aufbewahrung, Aktenführung und rechtliche Beweiskraft entstehen nicht allein durch immutable Java-Objekte.

Dafür braucht es passende organisatorische und technische Gesamtkonzepte.

---

# Teil XVII — Grenzen von Immutability

## 82. Immutability verhindert keine falsche Fachlogik

Ein perfekt immutable Money kann trotzdem mit einer falschen Gebührenregel verwendet werden.

---

## 83. Immutability verhindert keine falsche Ownership

Ein immutable Snapshot kann im falschen System zur führenden Wahrheit erklärt werden.

---

## 84. Immutability verhindert keine veralteten Daten

Immutable bedeutet:

> unverändert

nicht:

> aktuell

---

## 85. Immutability ersetzt keine Transaktion

Mehrere immutable Werte können gemeinsam inkonsistent persistiert werden, wenn die Transaktionsgrenze falsch ist.

---

## 86. Immutability ersetzt keine Synchronisation für mutable Ressourcen

Wenn mehrere Threads eine mutable Counter-, Queue- oder Entity-Struktur teilen, müssen deren Synchronisationsanforderungen weiterhin gelöst werden.

---

## 87. Deep Immutability ist teuer oder unpraktisch bei großen Graphen

Ein riesiger Objektgraph muss nicht bei jeder kleinen Änderung vollständig neu konstruiert werden.

Geeignete Alternativen können sein:

- kontrollierte interne Mutation,
- Builder,
- Copy-on-Write,
- Structural Sharing,
- Aggregate Boundaries.

---

# Teil XVIII — Typische Fehlanwendungen

## 88. Record = tief immutable

Falsch.

Java Records sind shallowly immutable.

---

## 89. final = immutable

Falsch.

final verhindert Reassignment einer Referenz, nicht Mutation des referenzierten Objekts.

---

## 90. Unmodifiable View = Snapshot

Falsch.

Eine unmodifiable View kann Änderungen des zugrunde liegenden Objekts weiterhin reflektieren.

---

## 91. List.copyOf = Deep Copy

Falsch.

Die Collection-Struktur wird geschützt.

Mutable Elemente bleiben mutable.

---

## 92. Jede Domain Entity muss immutable sein

Falsch.

Entities besitzen häufig einen Lifecycle.

Entscheidend ist kontrollierte Mutation.

---

## 93. Jede Änderung erzeugt einen neuen riesigen Graphen

Kann unnötige Laufzeit- und Modellierungskosten erzeugen.

---

## 94. Defensive Copy überall

Kann unnötige Allocation und Datenbewegung verursachen.

Ownership-Grenzen sollten die Entscheidung treiben.

---

## 95. Öffentliche Setter für Framework-Kompatibilität

Framework-Anforderungen sollten nicht automatisch die fachliche API bestimmen.

---

## 96. final für jede lokale Variable als Qualitätsstandard

Kann Intent ausdrücken.

Ein universelles Mandat erzeugt jedoch nicht automatisch bessere Zustandskapselung und kann visuelles Rauschen erhöhen.

---

## 97. Immutable DTO = sichere Anwendung

Falsch.

DTO-Immutability löst keine:

- Authorization,
- Injection,
- Business Rule,
- Datenqualitäts-
- oder Integritätsprobleme.

---

# Teil XIX — Testing und Evidence

## 98. Invariant Tests

Beispiel:

~~~java
@Test
void approved_application_contains_decision_metadata() {
    ...
}
~~~

Die Tests sollten fachliche Invarianten ausdrücken.

Nicht nur Getter prüfen.

---

## 99. Negative Tests

Wichtig sind unerlaubte Zustandsübergänge:

~~~java
@Test
void draft_application_cannot_be_approved_directly() {
    ...
}
~~~

falls diese Fachregel gilt.

---

## 100. Property-based Tests

Bei Value Objects mit mathematischen oder strukturellen Invarianten können Property-based Tests sinnvoll sein.

Beispiel:

~~~text
für alle validen Perioden gilt:
start <= end
~~~

oder:

~~~text
Money.add
erhält die Currency
~~~

---

## 101. Mutation Boundary Tests

Ein Test für defensive Collection-Grenzen:

~~~java
List<DocumentRef> source =
        new ArrayList<>(documents);

ApplicationSnapshot snapshot =
        new ApplicationSnapshot(source);

source.clear();

assertThat(snapshot.documents())
        .containsExactlyElementsOf(documents);
~~~

---

## 102. Contract Tests für Snapshots und Events

Bei Event-Verträgen prüfen:

- Pflichtfelder,
- Version,
- stabile Semantik,
- Serialisierung,
- Kompatibilität.

Immutability ist nur ein Teil des Vertrags.

---

## 103. Concurrency Tests reichen allein nicht

Race Conditions sind oft nicht deterministisch reproduzierbar.

Deshalb zusätzlich:

- Ownership Design,
- klare Thread Boundaries,
- JMM-konforme Publication,
- geeignete Synchronisationsprimitive.

Tests können das Design unterstützen, nicht ersetzen.

---

# Teil XX — Praktisches Reviewverfahren

## 104. Schritt 1 — Zustand identifizieren

Welche Daten bilden den relevanten Zustand?

---

## 105. Schritt 2 — Invarianten formulieren

Welche Bedingungen definieren einen gültigen Zustand?

---

## 106. Schritt 3 — Ownership festlegen

Wer besitzt den Zustand?

---

## 107. Schritt 4 — Aliase suchen

Welche anderen Referenzen können denselben Zustand verändern?

---

## 108. Schritt 5 — Mutationsbedarf prüfen

Muss der Zustand nach Konstruktion verändert werden?

Wenn nein:

> Immutability bevorzugen.

Wenn ja:

> Mutation explizit modellieren.

---

## 109. Schritt 6 — Mutationsoperationen prüfen

Sind es technische Setter oder fachliche Operationen?

---

## 110. Schritt 7 — Collection- und Array-Grenzen prüfen

Werden mutable Container ein- oder ausgegeben?

---

## 111. Schritt 8 — Tiefe der Immutability bestimmen

Reicht shallow immutability?

Oder müssen auch Elemente stabil sein?

---

## 112. Schritt 9 — Concurrency-Kontext prüfen

Wird der Zustand:

- zwischen Threads geteilt,
- gecacht,
- asynchron versendet,
- global publiziert?

---

## 113. Schritt 10 — Persistenzkontext prüfen

Gibt es:

- konkurrierende Updates,
- Optimistic Locking,
- Transaction Boundaries,
- Dirty Checking?

---

## 114. Schritt 11 — Performance prüfen

Sind Copies tatsächlich messbar relevant?

---

## 115. Schritt 12 — Evidence definieren

Wie wird nachgewiesen, dass:

- Invarianten gelten,
- kein Reference Leak besteht,
- unerlaubte Mutationen verhindert werden,
- fachliche Übergänge korrekt sind?

---

# Teil XXI — Review-Checkliste

## 116. Invarianten

- Welche Zustände sind gültig?
- Welche Kombinationen sind verboten?
- Wo werden diese Regeln erzwungen?
- Sind sie an einer zentralen Stelle verständlich?

## 117. Ownership

- Wer besitzt den mutable Zustand?
- Wer darf ihn verändern?
- Welche Referenzen verlassen die Ownership-Grenze?
- Gibt es unbeabsichtigte Aliase?

## 118. Immutability

- Ist das Objekt wirklich immutable oder nur shallow?
- Sind enthaltene Collections oder Arrays mutable?
- Sind enthaltene Elemente mutable?
- Ist die beobachtbare Semantik stabil?

## 119. Java

- Bedeutet final hier nur Reference Stability?
- Nutzt ein Record mutable Components?
- Wird List.copyOf korrekt eingeordnet?
- Entkommt this während Konstruktion?

## 120. Domain Model

- Sind Setter fachlich zu schwach?
- Sind Zustandsübergänge explizit?
- Tragen Value Types ihre Invarianten?
- Ist Entity-Mutation gekapselt?

## 121. Concurrency

- Wird shared mutable state vermieden?
- Sind Snapshots stabil?
- Wird Memory Visibility mit Business Concurrency verwechselt?

## 122. Performance

- Sind Copy-Kosten gemessen?
- Ist die Datenstruktur groß genug, dass Structural Sharing oder kontrollierte Mutation sinnvoller wäre?

---

# Teil XXII — Woran erkennt man ein tragfähiges Zustandsmodell?

## 123. Eigenschaften

Ein tragfähiges Zustandsmodell zeigt typischerweise:

- klar formulierte Invarianten,
- explizite Ownership,
- wenige unkontrollierte Aliase,
- immutable Value Objects, wo Wertsemantik dominiert,
- kontrollierte Mutation, wo Lifecycle dominiert,
- defensive Grenzen an echten Ownership-Übergängen,
- stabile Events und Snapshots,
- klare Concurrency- und Persistenzmechanismen,
- messbasierte Performanceentscheidungen.

Die Balance lautet:

~~~text
so viel Immutability wie sinnvoll
+
so viel Mutation wie fachlich notwendig
+
klare Ownership
+
explizite Invarianten
~~~

---

# Teil XXIII — Glossar

## Alias

Eine weitere Referenz auf dasselbe Objekt.

## Deep Immutability

Unveränderlichkeit des relevanten gesamten Objektgraphen, nicht nur der direkten Felder.

## Defensive Copy

Kopie eines eingehenden oder ausgehenden mutable Werts, um Ownership und interne Repräsentation zu schützen.

## Entity

Objekt mit fachlicher Identität und Lifecycle.

## final Field

Java-Feld, das nach seiner Initialisierung nicht normal neu zugewiesen wird und besondere Semantik im Java Memory Model besitzt.

## Immutable Object

Objekt, dessen beobachtbarer logischer Zustand nach abgeschlossener Konstruktion nicht mehr geändert werden kann.

## Invariant

Bedingung, die für gültige stabile Zustände einer Abstraktion gelten muss.

## Ownership

Verantwortung darüber, welcher Teil des Systems einen Zustand kontrolliert und mutieren darf.

## Reference Leak

Unbeabsichtigtes Exponieren einer Referenz, über die interner Zustand außerhalb seiner vorgesehenen Grenze verändert werden kann.

## Shallow Immutability

Unveränderlichkeit der direkten Referenzen oder Komponenten bei weiterhin möglicherweise mutablen referenzierten Objekten.

## Snapshot

Stabile Repräsentation eines Zustands zu einem definierten Zeitpunkt.

## Unmodifiable

Über eine bestimmte API nicht mutierbar; nicht automatisch identisch mit Immutability.

## Value Object

Objekt, dessen fachliche Bedeutung primär durch seine Werte und nicht durch dauerhafte Identität bestimmt wird.

---

# Teil XXIV — Quellen und weiterführende Literatur

## 124. Java SE 21 — java.lang.Record

Oracle, Java SE 21 API, java.lang.Record.

https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/Record.html

Relevant:

- Record Classes sind shallowly immutable,
- Record Components werden durch private final Fields repräsentiert,
- Record ist Sprachmechanismus für transparente Datenträger.

---

## 125. Java Language Specification — final Field Semantics

Java Language Specification, Kapitel 17.5.

https://docs.oracle.com/javase/specs/jls/se21/html/jls-17.html#jls-17.5

Relevant:

- besondere Semantik von final Fields im Java Memory Model,
- Grundlage für korrekt konstruierte thread-safe immutable Objekte.

---

## 126. Java Collections — List.copyOf und unmodifiable Lists

Oracle Java API, java.util.List.

https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/List.html

Relevant:

- List.copyOf liefert eine unmodifiable List,
- spätere Änderungen der Input-Collection werden nicht reflektiert,
- mutable Elemente können weiterhin zu beobachtbaren Änderungen führen.

---

## 127. Bertrand Meyer — Design by Contract

Bertrand Meyer, *Applying "Design by Contract"*, Computer, Vol. 25, No. 10, 1992, S. 40–51.

DOI:

https://doi.org/10.1109/2.161279

Relevant:

- Preconditions,
- Postconditions,
- Class Invariants,
- Verträge über gültige Zustände und Operationen.

---

## 128. Meyer, Arkadova, Kogtenkov — Class Invariants

Bertrand Meyer, Alisa Arkadova, Alexander Kogtenkov, *The Concept of Class Invariant in Object-Oriented Programming*, Formal Aspects of Computing, 2024.

Preprint:

https://arxiv.org/abs/2109.06557

Relevant:

- präzise Bedeutung von Object/Class Invariants,
- Reference Leaks,
- Callbacks,
- Herausforderungen modularer Verifikation.

---

## 129. Clarke, Potter, Noble — Ownership Types

David G. Clarke, John Potter, James Noble, *Ownership Types for Flexible Alias Protection*, OOPSLA 1998.

DOI:

https://doi.org/10.1145/286936.286947

Relevant:

- Aliasing als Herausforderung für Encapsulation,
- Ownership als Mittel zur Kontrolle von Zugriffswegen,
- Zusammenhang zwischen Objektgraph und Invariantenschutz.

---

## 130. Clarke, Noble, Wrigstad — Aliasing in Object-Oriented Programming

Dave Clarke, James Noble, Tobias Wrigstad (Hrsg.), *Aliasing in Object-Oriented Programming: Types, Analysis and Verification*, Springer, 2013.

DOI:

https://doi.org/10.1007/978-3-642-36946-9

Relevant:

- Überblick über Aliasing,
- Ownership,
- Concurrency,
- Verifikation mutable Objektgraphen.

---

## 131. Eric Evans — Value Objects und Aggregates

Eric Evans, *Domain-Driven Design: Tackling Complexity in the Heart of Software*, Addison-Wesley, 2003.

Relevant für:

- Entities,
- Value Objects,
- Aggregates,
- fachliche Invarianten und Modellgrenzen.

Die DDD-Konzepte werden hier als Modellierungsheuristiken verwendet, nicht als formale Immutability-Theorie.

---

## 132. Joshua Bloch — Effective Java

Joshua Bloch, *Effective Java*, 3rd Edition, Addison-Wesley, 2018.

Relevant insbesondere für:

- Minimierung von Mutability,
- defensive Kopien,
- sichere API-Gestaltung,
- Immutability als Java-Designtechnik.

---

## 133. Verhältnis zu anderen Knowledge Items

- AK-001 — Records für Datenträger
- AK-016 — JPA/Persistence Access
- AK-022 — Resilience Patterns
- AK-025 — SOLID: Änderbarkeit, Verantwortung und Abhängigkeitsdesign
- AK-026 — Einfachheit, Wissensduplikation, YAGNI und geringe Wissenskopplung
- AK-084 — Kopplung, Kohäsion und Information Hiding
- AK-089 — Strategic DDD / Context Mapping

---

# 134. Merksatz

> Immutability ist kein Selbstzweck. Ein robustes Zustandsmodell macht **Invarianten explizit**, legt **Ownership eindeutig fest**, begrenzt **Aliasing und Reference Leaks** und erlaubt Mutation nur dort, wo ein fachlicher oder technischer Lifecycle sie tatsächlich benötigt. Immutable Werte, defensive Kopien und Records sind Werkzeuge dafür — nicht das Ziel selbst.
