---
id: AK-028
legacy_ids:
  - QG-JAVA-028
  - QG-JAVA-029
  - QG-JAVA-030
title: Java Design Patterns problemorientiert einsetzen
artifact_type: learning-guide
domain: software-design
status: active
maturity: reviewed
normative_level: informative
last_validated: 2026-09-30
---

# AK-028 — Java Design Patterns problemorientiert einsetzen

## 1. Einordnung

Design Patterns sind benannte Lösungsformen für wiederkehrende Designprobleme. Ihr Wert liegt in gemeinsamer Sprache und wiederverwendbarer Erfahrung. Sie sind kein Katalog, den eine Anwendung möglichst vollständig verwenden sollte.

Die richtige Reihenfolge lautet:

```text
Problem und Veränderungstreiber verstehen
→ einfachste passende Struktur wählen
→ Pattern nur einsetzen, wenn es das Problem sichtbar besser löst
```

## 2. Creational Patterns

### Factory

Sinnvoll, wenn Objekterzeugung eine eigene Entscheidung enthält:

- mehrere Implementierungen,
- komplexe Konstruktion,
- Entkopplung vom konkreten Typ.

Eine Factory für `new Customer()` ohne zusätzliche Semantik erzeugt dagegen nur Umweg.

### Builder

Sinnvoll bei:

- vielen optionalen Parametern,
- lesbarer Testdatenerzeugung,
- schrittweiser Konstruktion komplexer Werte.

Bei kleinen unveränderlichen Typen können Record-Konstruktor oder statische Fabrik einfacher sein.

### Singleton

Ein global erreichbares Singleton koppelt Consumer an globalen Zustand und erschwert Tests. Container-managed Singleton-Lifecycles sind etwas anderes: Die Abhängigkeit kann weiterhin explizit injiziert werden.

## 3. Structural Patterns

### Adapter

Passt eine fremde oder technische Schnittstelle an den eigenen Vertrag an. Besonders wertvoll an Ports, Legacy-Grenzen und Anti-Corruption Layers.

### Decorator

Ergänzt Verhalten um einen bestehenden Vertrag, zum Beispiel Telemetrie, Caching oder Resilience. Wird problematisch, wenn Reihenfolge vieler Decorators nicht mehr nachvollziehbar ist.

### Facade

Bietet einen vereinfachten Einstieg in ein komplexeres Subsystem. Eine Facade ist kein Grund, die innere Struktur ungeordnet zu lassen.

### Proxy

Steuert Zugriff auf ein anderes Objekt, etwa für Lazy Loading oder Remote Calls. Framework-Proxies haben Konsequenzen für Transaktionen, Security und Tests und sollten nicht unsichtbar vorausgesetzt werden.

## 4. Behavioral Patterns

### Strategy

Sinnvoll, wenn eine Operation mehrere austauschbare Algorithmen besitzt:

```java
interface PricingPolicy {
    Money calculate(Order order);
}
```

Für eine einzige Implementierung ohne realen Variationspunkt ist das Interface nicht automatisch besser.

### State

Kann komplexe zustandsabhängige Logik kapseln. Bei wenigen klaren Zuständen können Enum, sealed types oder einfache explizite Übergangsregeln leichter sein.

### Observer

Entkoppelt Produzent und Reaktion, bringt aber implizite Kontrollflüsse. Bei verteilten Events kommen zusätzlich Delivery, Ordering, Schema und Operations hinzu – das ist mehr als das klassische GoF-Pattern.

### Template Method

Kann einen stabilen Ablauf mit variierenden Schritten ausdrücken, basiert aber auf Vererbung. Oft ist Komposition über Strategies flexibler.

### Command

Macht einen Auftrag zu einem Objekt. Das kann Queueing, Audit oder Undo unterstützen. Ein simples Service-Methodenargument muss deshalb nicht automatisch ein Command Pattern werden.

## 5. Muster kombinieren

Patterns treten häufig gemeinsam auf:

```text
Port
→ Adapter
→ Decorator für Telemetrie/Resilience
→ Strategy für austauschbares Verhalten
```

Je mehr Muster kombiniert werden, desto wichtiger ist die Frage, ob die resultierende Struktur noch leichter zu verstehen ist als die Ausgangslösung.

## 6. Pattern-Signale und Gegenfragen

| Problem | mögliches Pattern | Gegenfrage |
|---|---|---|
| externe API passt nicht | Adapter | reicht eine kleine Mappingfunktion? |
| mehrere Algorithmen | Strategy | existiert echte Variation? |
| komplexe Konstruktion | Builder/Factory | ist der Typ selbst zu komplex? |
| Querschnitt um Vertrag | Decorator | ist Reihenfolge klar? |
| komplexe Zustandslogik | State | reicht ein explizites Zustandsmodell? |
| Subsystem zu kompliziert | Facade | versteckt sie nur strukturelle Probleme? |

## 7. Patterns und moderne Java-Mittel

Moderne Sprachmittel verändern die notwendige Form mancher Patterns:

- Lambdas können kleine Strategies ersetzen.
- Records vereinfachen immutable Datenträger und Commands.
- Sealed Types plus Pattern Matching können geschlossene Varianten klar ausdrücken.
- Dependency Injection reduziert die Notwendigkeit eigener Factory-/Singleton-Infrastruktur.

Das Designproblem bleibt; die Implementierungsform darf einfacher werden.

## 8. Prüffragen

1. Welches konkrete Problem löst das Pattern?
2. Welche Variation oder Änderung erwarten wir tatsächlich?
3. Was kostet die zusätzliche Indirektion?
4. Gibt es eine einfachere Sprach- oder Bibliothekslösung?
5. Wird Ownership klarer oder nur verteilt?
6. Könnte ein zukünftiger Entwickler die Struktur ohne Patternkatalog verstehen?

## 9. Verwandte Knowledge-Items

- `AK-008` — Objektorientierung und Verantwortung
- `AK-026` — KISS, DRY, YAGNI
- `AK-027` — Code Smells und Refactoring
- `AK-083` — Architekturmuster und Qualitätsziele

## 10. Quellen

- Gamma, Helm, Johnson, Vlissides, *Design Patterns*, 1994.
- Joshua Kerievsky, *Refactoring to Patterns*, 2004.
- Martin Fowler, *Refactoring*, 2nd ed., 2018.

## 11. Merksatz

> Ein Pattern ist gut, wenn es ein reales Designproblem einfacher erklärbar und veränderbar macht. Bekanntheit allein ist kein Einsatzgrund.
