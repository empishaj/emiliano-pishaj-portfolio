---
id: AK-090
legacy_ids:
  - ADR-090
title: Evolutionary Architecture und kontrollierte Veränderbarkeit
artifact_type: architecture-principle
domain: transformation
status: active
maturity: reviewed
normative_level: recommended
last_validated: 2026-10-01
review_trigger:
  - neue Transformationsstrategie
  - wiederkehrende Architekturdrift
  - größere Plattform- oder Domänenänderung
---

# AK-090 — Architektur für Veränderung gestalten

## 1. Kernidee

Zwei Extreme sind gleichermaßen riskant:

```text
alles vollständig vorab festlegen
vs.
Architektur vollständig emergent entstehen lassen
```

Evolutionary Architecture sucht einen kontrollierten Mittelweg:

> **wichtige Eigenschaften bewusst festlegen, Veränderungen inkrementell zulassen und relevante Architekturmerkmale kontinuierlich überprüfen.**

Das Ziel ist nicht permanente Veränderung.

Das Ziel ist die Fähigkeit, auf neue Anforderungen, Erkenntnisse und Randbedingungen reagieren zu können, ohne die Systemintegrität unkontrolliert zu verlieren.

## 2. Drei Voraussetzungen

### 2.1 Änderbare Struktur

Grenzen und Verträge müssen Veränderungen lokal genug halten.

### 2.2 Feedback

Man muss erkennen können, ob sich relevante Eigenschaften verschlechtern.

### 2.3 Entscheidungsfähigkeit

Wenn sich Annahmen ändern, müssen Standards und Entscheidungen überprüft werden können.

Damit ist Evolutionary Architecture nicht nur ein Code-Prinzip, sondern verbindet Design, Delivery, Operations und Governance.

## 3. Fitness Functions richtig verstehen

Eine Fitness Function überprüft eine gewünschte Architektureigenschaft.

Beispiele:

### strukturell

```text
Domain darf Adapter nicht importieren.
```

Prüfung:

- ArchUnit,
- Modulprüfung,
- Dependency Analysis.

### Vertrag

```text
Eine API-Änderung darf bestehende Consumer nicht unkontrolliert brechen.
```

Prüfung:

- OpenAPI-/Schema-Diff,
- Contract Tests.

### Runtime

```text
Der kritische Use Case erfüllt sein Latenzziel unter definierter Last.
```

Prüfung:

- Performance Test,
- SLI/SLO.

### Recovery

```text
Das System kann innerhalb des vereinbarten RTO wiederhergestellt werden.
```

Prüfung:

- Restore-/DR-Test.

Nicht jede Architektureigenschaft ist automatisierbar. Ownership, fachliche Passung oder ein strategisches Zielbild brauchen menschliche Governance.

## 4. Keine universellen Fitness-Schwellen

Das historische Dokument enthielt feste Werte wie:

- Komplexität kleiner 10,
- Coverage 80 %, 
- P95 500 ms,
- 100 RPS.

Solche Werte können in einem konkreten System richtig sein, sind aber **keine universellen Architekturregeln**.

Im neuen Modell gilt:

```text
Anforderung / Qualitätsziel
→ konkrete Messgröße
→ vereinbarte Schwelle
→ Fitness Function
```

Nicht umgekehrt.

## 5. Transition Architectures

Enterprise Transformation ist selten ein Sprung von Ist direkt zu Ziel.

```text
Ist
 ↓
Transition A
 ↓
Transition B
 ↓
Ziel
```

Jede Transition Architecture muss selbst ausreichend tragfähig sein:

- betreibbar,
- sicher,
- beobachtbar,
- migrierbar,
- verantwortet.

Ein Übergang, der nur „temporär“ gedacht ist, kann Jahre bestehen. Deshalb darf er nicht architektonisch fahrlässig sein.

## 6. Strangler Pattern

Das Strangler Pattern eignet sich für bestimmte schrittweise Ablösungen:

1. eine geeignete Grenze identifizieren,
2. neue Funktion neben dem Bestand aufbauen,
3. Traffic/Funktion schrittweise umleiten,
4. Verhalten und Daten validieren,
5. Altteil entfernen, wenn kein Consumer mehr abhängt.

Wichtig:

> Strangler ist ein Migrationsmuster, kein automatischer Standard für jede Modernisierung.

Wenn keine geeignete Routing-/Funktionsgrenze existiert, kann ein anderes Vorgehen besser sein.

## 7. Branch by Abstraction

Bei tief eingebetteten Komponenten kann eine Abstraktionsgrenze eingeführt werden:

```text
bestehende Implementierung
      ↓
stabile Abstraktion
      ↑
neue Implementierung
```

Dann wird schrittweise umgeschaltet.

Auch hier gilt:

- Abstraktion muss semantisch sinnvoll sein,
- Umschaltung braucht Tests/Evidence,
- alte Implementierung und temporäre Flags müssen später entfernt werden.

## 8. Technische Schulden differenziert steuern

Nicht jede unschöne Stelle ist automatisch „Technical Debt“.

Für Governance sind mindestens diese Fragen wichtiger als Etiketten:

- Welche Qualitäts- oder Lieferwirkung besitzt die Abweichung?
- Ist sie bewusst oder entdeckt worden?
- Wie groß ist ihr Änderungs-/Betriebsrisiko?
- Gibt es einen Owner?
- Welcher Trigger macht Rückzahlung notwendig?

Beispiel:

```text
Debt:
Legacy SOAP Adapter bleibt für Übergangsphase bestehen

Risiko:
hohe Betriebs- und Security-Kosten

Exit Trigger:
letzter Consumer migriert

Evidence:
Consumer-Inventar = 0

Action:
Adapter und Infrastruktur entfernen
```

Das ist nützlicher als eine allgemeine „20 % Tech Debt“-Regel.

## 9. Architekturentscheidungen altern

Eine Entscheidung kann im Zeitpunkt ihrer Entstehung richtig und später trotzdem unpassend sein.

Deshalb besitzen wichtige Entscheidungen Review-Trigger:

- Lastprofil ändert sich,
- Gesetz/Policy ändert sich,
- Team-/Ownership-Schnitt ändert sich,
- Technologie wird deprecated,
- ein Incident widerlegt eine Annahme,
- Kosten ändern sich signifikant,
- Transition erreicht den nächsten Zustand.

Die Reaktion ist nicht stilles Umschreiben des ADRs, sondern eine neue Entscheidung mit Supersession.

## 10. Architekturdrift

Drift entsteht, wenn reale Struktur und vereinbarte Architektur auseinanderlaufen.

Beispiele:

- verbotene Modulabhängigkeiten,
- neue direkte Datenbankzugriffe,
- API-Verträge außerhalb des Standards,
- manuelle Produktionsänderungen außerhalb GitOps,
- nicht mehr eingehaltene SLOs,
- „temporäre“ Exceptions ohne Exit.

Nicht jede Drift ist automatisch falsch.

Sie kann auch anzeigen, dass der Standard nicht mehr zum Kontext passt.

Professionelle Governance fragt daher:

> Muss die Umsetzung korrigiert werden – oder hat die Realität eine Annahme der Architektur widerlegt?

## 11. Modernisierung Schritt für Schritt

1. Ist-Zustand und Abhängigkeiten verstehen.
2. Ziel-Qualitäten formulieren.
3. Veränderungsgrenzen identifizieren.
4. Transition States definieren.
5. Migrationsrisiken und Rückfallpfade klären.
6. Evidence je Transition festlegen.
7. kleine Änderungen durchführen.
8. messen und lernen.
9. Altbestand aktiv entfernen.
10. Standards/ADRs bei neuer Erkenntnis aktualisieren beziehungsweise ersetzen.

## 12. Behördenkontext

Evolution ist in Behörden besonders relevant, weil häufig gleichzeitig gelten:

- lang lebende Fachverfahren,
- gesetzlich gebundene Prozesse,
- externe Register und Partner,
- mehrere Dienstleister,
- Vergabe- und Vertragszyklen,
- hohe Daten- und Sicherheitsanforderungen,
- begrenzte Big-Bang-Möglichkeiten.

Daher ist eine realistische Modernisierungsroadmap oft stärker als ein perfektes fernes Zielbild.

## 13. Anti-Patterns

### Big Design Upfront als Karikatur

Vorab-Design ist nicht grundsätzlich schlecht. Kritische Entscheidungen benötigen bewusstes Design. Problematisch wird es, wenn Unsicherheit ignoriert und spätere Lernfähigkeit verhindert wird.

### „Emergent Architecture“ ohne Leitplanken

Kann zu zufälliger Struktur und hoher Drift führen.

### Fitness Function = nur Code Metric

Architekturqualität umfasst auch Runtime-, Security-, Contract-, Recovery- und Governance-Evidence.

### Jede technische Schuld mit Annotation versehen

Debt Management gehört in ein verantwortetes System, nicht zwingend als Custom Java Annotation in den Sourcecode.

### Legacy entfernen vergessen

Eine Migration ist erst abgeschlossen, wenn alte Pfade, Flags, Datenkopien und Betriebsprozesse tatsächlich stillgelegt sind.

## 14. Quellen

- Neal Ford, Rebecca Parsons, Patrick Kua — *Building Evolutionary Architectures*
- Martin Fowler — Strangler Fig Application  
  https://martinfowler.com/bliki/StranglerFigApplication.html
- Martin Fowler — Branch by Abstraction  
  https://martinfowler.com/bliki/BranchByAbstraction.html
- AK-061 — Architecture Fitness Functions
- AK-075 — Architecture Decision Process
- AK-093 — Living Documentation

## 15. Merksatz

> Eine Zielarchitektur ist nur dann wertvoll, wenn du auch erklären kannst, **welche sicheren Übergangszustände dorthin führen und welche Evidence dir zeigt, dass du noch auf dem richtigen Pfad bist**.
