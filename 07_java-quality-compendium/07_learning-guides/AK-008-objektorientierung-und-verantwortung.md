---
id: AK-008
legacy_ids:
  - QG-JAVA-007
  - QG-JAVA-008
title: Objektorientierung, Verantwortung und Fehlanwendungen
artifact_type: learning-guide
domain: software-design
status: active
maturity: reviewed
normative_level: informative
last_validated: 2026-09-30
---

# AK-008 — Objektorientierung, Verantwortung und Fehlanwendungen

## 1. Einordnung

Objektorientierung ist nicht die Kunst, möglichst viele Klassen, Interfaces oder Patterns zu erzeugen. Ihr Nutzen liegt darin, zusammengehörige Daten, Regeln und Verantwortung so zu schneiden, dass Änderungen lokal verständlich bleiben.

Viele Fehlanwendungen entstehen nicht durch fehlende Sprachkenntnis, sondern durch falsche Verantwortungsgrenzen.

## 2. Ein Objekt ist mehr als Daten plus Getter

Ein fachlicher Typ sollte relevante Invarianten möglichst selbst schützen.

```java
public final class Order {
    private OrderStatus status;

    public void cancel(CancellationReason reason) {
        if (!status.canBeCancelled()) {
            throw new OrderCannotBeCancelledException(status);
        }
        status = CANCELLED;
    }
}
```

Wenn jeder Aufrufer Zustände frei setzt und Regeln außerhalb verteilt sind, entsteht ein anämisches Modell. Das kann bei einfachen CRUD-Fällen ausreichend sein; bei komplexen fachlichen Regeln wird es jedoch schnell schwer kontrollierbar.

## 3. Verantwortung statt Schichtenformalismus

Die bessere Frage lautet nicht:

> In welche Schicht gehört diese Methode laut Lehrbuch?

Sondern:

> Welche Einheit besitzt die Information und Verantwortung, diese Entscheidung korrekt zu treffen?

Dadurch entstehen sinnvolle Grenzen zwischen:

- Domänenlogik,
- Orchestrierung,
- Persistenz,
- Integration,
- Präsentation,
- technischen Querschnittsfunktionen.

## 4. Typische Fehlanwendungen

### God Objects

Eine Klasse besitzt zu viele unabhängige Gründe zur Änderung. Das ist häufig ein Kohäsionsproblem.

### Primitive Obsession

Fachlich unterschiedliche Dinge werden als `String`, `long` oder `BigDecimal` herumgereicht, obwohl eigene Typen Regeln und Semantik ausdrücken könnten.

### Setter als fachliche API

`setStatus(CANCELLED)` beschreibt keinen fachlichen Vorgang. `cancel(reason)` kann dagegen Regeln, Audit und Übergänge kapseln.

### Vererbung aus Bequemlichkeit

Vererbung ist sinnvoll, wenn eine echte substituierbare Typbeziehung besteht. Gemeinsamer Code allein ist kein ausreichender Grund.

### Interface für jede Klasse

Ein Interface ist besonders wertvoll als echter Vertrag oder Variationspunkt. Ein Interface nur deshalb anzulegen, weil „man das so macht“, erhöht Struktur ohne zusätzlichen Nutzen.

### Patterns ohne Problem

Factory, Strategy, Observer oder Visitor sind Werkzeuge. Werden sie ohne konkreten Änderungs- oder Kopplungstreiber eingeführt, entsteht abstrakte Komplexität.

## 5. Komposition und Polymorphie

Komposition ist oft flexibler als tiefe Vererbung. Polymorphie lohnt sich, wenn Verhalten entlang einer stabilen fachlichen Abstraktion variiert.

```java
interface PricingPolicy {
    Money calculatePrice(Order order);
}
```

Das ist sinnvoll, wenn tatsächlich mehrere Pricing-Strategien existieren oder erwartet werden. Für eine einzige triviale Implementierung ist zusätzliche Abstraktion nicht automatisch besser.

## 6. Frameworks nicht zum Domänenmodell machen

Spring, JPA und andere Frameworks sind technische Mittel. Wenn fachliche Objekte nur durch Frameworkannotation und Persistenzkonvention definiert werden, wird technischer Lifecycle schnell zur eigentlichen Modellstruktur.

Nicht jede Anwendung braucht eine vollständig frameworkfreie Domain. Entscheidend ist, ob Frameworkkopplung relevante Änderbarkeit, Testbarkeit oder Fachlichkeit beeinträchtigt.

## 7. Verbindung zu anderen Knowledge-Items

- `AK-025` behandelt SOLID als Designprinzipien.
- `AK-026` behandelt KISS, DRY und YAGNI.
- `AK-084` vertieft Kopplung, Kohäsion und Information Hiding.
- `AK-023` behandelt Domain-Driven Design als Entscheidungsansatz.
- `AK-031` behandelt Ports & Adapters.
- `AK-027` behandelt Code Smells und Refactoring.

Dieses Dokument ersetzt diese Themen nicht. Es liefert die gemeinsame Grundidee: **Verantwortung sichtbar machen und unnötige Kopplung vermeiden.**

## 8. Prüffragen

1. Welche Verantwortung besitzt diese Klasse oder dieses Modul?
2. Welche Invarianten müssen dort geschützt werden?
3. Welche Gründe zur Änderung sind zusammengefallen?
4. Ist eine Abstraktion durch echte Variation begründet?
5. Ist Vererbung fachlich korrekt oder nur Code-Wiederverwendung?
6. Macht das Framework die Modellgrenze klarer oder bestimmt es sie unnötig?
7. Wird Komplexität durch das Pattern reduziert oder nur verschoben?

## 9. Quellen

- Robert C. Martin, *Clean Architecture*, 2017.
- Eric Evans, *Domain-Driven Design*, 2003.
- Martin Fowler, *Refactoring*, 2nd ed., 2018.
- Gamma et al., *Design Patterns*, 1994.

## 10. Merksatz

> Gute Objektorientierung verteilt Verantwortung so, dass gültige Zustände geschützt und Änderungen lokal verständlich bleiben. Mehr Klassen oder Patterns sind dafür kein Selbstzweck.
