---
id: AK-025
legacy_ids:
  - QG-JAVA-025
title: SOLID als Diagnose- und Gestaltungsheuristik
artifact_type: engineering-principle
domain: software-design
status: active
maturity: reviewed
normative_level: recommended
last_validated: 2026-09-28
review_trigger:
  - grundlegende Änderung der Design- oder Modularity-Standards
---

# AK-025 — SOLID ohne Dogma

## 1. Ziel

SOLID ist kein Zertifikat für „guten Code“ und kein Auftrag, möglichst viele Interfaces, Schichten und Abstraktionen zu erzeugen.

Die fünf Prinzipien sind nützlich, wenn sie als **Diagnosefragen für Änderbarkeit, Substituierbarkeit und Abhängigkeitsstruktur** verwendet werden.

Der zentrale Maßstab ist nicht:

> „Ist dieses Design SOLID?“

sondern:

> „Hilft diese Struktur dabei, die erwartbaren Änderungen lokal, verständlich und sicher durchzuführen?“

## 2. Die fünf Prinzipien als Fragen

### Single Responsibility Principle

Nicht:

> Eine Klasse darf nur eine Methode haben.

Sondern:

> Welche unterschiedlichen Änderungsgründe sind in dieser Einheit gekoppelt?

Warnsignale:

- fachliche Regel und Infrastrukturdetail ändern dieselbe Klasse,
- Reporting, Persistenz, E-Mail und Geschäftslogik leben in einem `Manager`,
- unterschiedliche Stakeholder verlangen unabhängig Änderungen am selben Modul.

### Open/Closed Principle

Nicht:

> Alles muss erweiterbar sein.

Sondern:

> Gibt es eine reale Änderungsachse, für die eine stabile Grenze sinnvoll ist?

Eine Erweiterungsarchitektur ohne realen Änderungsdruck ist spekulative Komplexität.

### Liskov Substitution Principle

Ein Subtyp ist nur dann nützlich, wenn er die Erwartungen des Basistyps einhält.

Prüffragen:

- Verstärkt der Subtyp Vorbedingungen?
- Schwächt er Nachbedingungen?
- wirft er neue, unerwartete Fehler für eigentlich erlaubte Operationen?
- ändert er Semantik, obwohl der Client denselben Vertrag erwartet?

Wenn ja, ist die Abstraktion wahrscheinlich falsch.

### Interface Segregation Principle

Ein Client sollte nicht von Operationen abhängen, die er nicht benötigt.

Das führt nicht automatisch zu sehr vielen Einmethoden-Interfaces.

Die bessere Frage lautet:

> Welche Rolle konsumiert welchen Vertrag?

Interfaces werden entlang echter Consumer-Bedürfnisse geschnitten.

### Dependency Inversion Principle

Fachlich zentrale Logik sollte nicht unnötig von volatilen technischen Details abhängen.

Beispiel:

```text
Domain Policy
    ↓
PaymentPort
    ↑
Stripe Adapter
```

Der Nutzen ist nicht „wir haben ein Interface“.

Der Nutzen ist:

- technische Austauschbarkeit,
- klarere Testgrenze,
- Schutz der Domänensprache,
- lokalisierte Abhängigkeit.

## 3. SOLID und Architektur

SOLID ist primär ein Softwaredesign-Werkzeug. Seine Prinzipien skalieren aber konzeptionell nach oben.

```text
SRP
→ klare Modul-/Service-Verantwortung

ISP
→ kleine, consumer-orientierte Verträge

DIP
→ Domäne nicht von Infrastruktur beherrschen lassen

LSP
→ stabile Verträge und substituierbare Implementierungen

OCP
→ gezielte Änderungsachsen kapseln
```

Das bedeutet nicht, SOLID ungeprüft auf Organisationen oder Enterprise Architecture zu übertragen. Es bedeutet, dass die zugrunde liegenden Fragen nach Verantwortung und Abhängigkeit auch auf höheren Ebenen relevant bleiben.

## 4. Abstraktion braucht Evidenz

Eine neue Abstraktion ist gerechtfertigt, wenn mindestens ein konkreter Treiber erkennbar ist, zum Beispiel:

- mehrere echte Implementierungen,
- externe Technologiegrenze,
- volatile Integration,
- klarer Test-Seam,
- fachlich stabile Port-Semantik,
- messbares Änderungsproblem.

Schwach:

```java
interface UserService {}
class UserServiceImpl implements UserService {}
```

wenn kein eigener Vertrag oder Variationspunkt existiert.

Stärker:

```java
interface CreditAgency {
    CreditAssessment assess(Applicant applicant);
}
```

wenn die Domäne bewusst einen externen Auskunftsdienst abstrahiert.

## 5. Trade-off: Entkopplung kostet

Jede zusätzliche Abstraktion bringt Kosten:

- mehr Konzepte,
- mehr Dateien,
- indirekteren Kontrollfluss,
- mehr Navigation,
- potenziell erschwerte Fehlersuche.

Darum gilt:

```text
Abstraktion
nur wenn
Nutzen der entkoppelten Änderung
>
Kosten der zusätzlichen Indirektion
```

Diese Ungleichung wird nicht mathematisch berechnet. Sie ist eine Reviewfrage.

## 6. Verbindung zu anderen Prinzipien

SOLID darf nicht isoliert angewendet werden.

- **KISS:** verhindert übertriebene Abstraktion.
- **YAGNI:** verhindert spekulative Erweiterungspunkte.
- **Information Hiding:** schützt volatile Entscheidungen.
- **High Cohesion / Low Coupling:** beschreibt das eigentliche Strukturziel.
- **Hexagonal Architecture:** wendet DIP auf Systemgrenzen an.

## 7. Review-Canvas

Bei einer problematischen Klasse oder einem Modul frage Schritt für Schritt:

1. Welche Verantwortung hat die Einheit?
2. Welche unabhängigen Änderungsgründe existieren?
3. Welche Abhängigkeiten sind stabil, welche volatil?
4. Welche Consumer benötigen welchen Vertrag?
5. Existiert echte Substitution oder nur Vererbung aus Bequemlichkeit?
6. Welche Abstraktion ist bereits durch reale Anforderungen begründet?
7. Würde die vorgeschlagene Verbesserung das Design vereinfachen oder nur formaler machen?
8. Welche Tests beweisen, dass die Struktur das gewünschte Verhalten schützt?

## 8. Anti-Patterns

### Interface für jede Klasse

Das ist kein Dependency Inversion, sondern Ritual.

### Ein Pattern pro Prinzip

SOLID schreibt keine Design Patterns vor.

### Jede Änderung ohne Modifikation

Open/Closed bedeutet nicht, dass bestehender Code niemals verändert werden darf.

### SRP = eine Methode

Verantwortung ist semantisch, nicht anhand der Methodenzahl definiert.

### Architekturverschlechterung für „Clean Code“

Lokale Eleganz darf keine unnötigen systemweiten Abhängigkeiten erzeugen.

## 9. Definition of Good Enough

Ein Design ist ausreichend SOLID, wenn:

- Verantwortlichkeiten verständlich sind,
- relevante Änderungen lokal bleiben,
- Verträge sinnvoll geschnitten sind,
- Substitution semantisch funktioniert,
- fachliche Logik nicht unnötig an technische Details gebunden ist,
- die Anzahl der Abstraktionen zum tatsächlichen Problem passt.

## 10. Quellen

- Robert C. Martin — SOLID-Prinzipien und Dependency-Inversion-Literatur
- Barbara Liskov / Jeannette Wing — Behavioral Subtyping
- David Parnas — Information Hiding und Modularisierung
- AK-026 — Einfachheit, DRY und YAGNI
- AK-084 — Kopplung, Kohäsion und Information Hiding

## 11. Coach-Merksatz

> SOLID ist kein Zielbild aus Interfaces. Es ist ein Satz von Fragen, mit denen du erkennst, **wo Verantwortungen, Verträge und Abhängigkeiten künftige Änderungen unnötig teuer machen**.
