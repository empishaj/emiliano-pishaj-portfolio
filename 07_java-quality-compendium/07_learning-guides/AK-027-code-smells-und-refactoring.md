---
id: AK-027
legacy_ids:
  - QG-JAVA-027
title: Code Smells und Refactoring als Diagnosewerkzeug
artifact_type: learning-guide
domain: software-design
status: active
maturity: reviewed
normative_level: informative
last_validated: 2026-09-30
---

# AK-027 — Code Smells und Refactoring als Diagnosewerkzeug

## 1. Einordnung

Ein Code Smell ist kein automatischer Fehler. Er ist ein Hinweis darauf, dass Verantwortung, Kopplung, Verständlichkeit oder Änderbarkeit geprüft werden sollten.

Refactoring verändert die interne Struktur, ohne das beobachtbare Verhalten absichtlich zu verändern. Gute Tests reduzieren dabei das Risiko, sind aber kein Ersatz für fachliches Verständnis.

## 2. Wichtige Smell-Familien

### Zu viel Verantwortung

Beispiele:

- sehr große Klassen,
- Methoden mit mehreren unabhängigen Aufgaben,
- zentrale „Manager“- oder „Service“-Klassen, durch die fast alles läuft.

Die Frage ist nicht, ob eine Klasse eine bestimmte Zeilenzahl überschreitet, sondern ob sie mehrere unabhängige Gründe zur Änderung besitzt.

### Feature Envy

Eine Methode arbeitet überwiegend mit Daten eines anderen Objekts. Das kann darauf hinweisen, dass Verhalten näher zu diesen Daten gehört.

### Shotgun Surgery

Eine kleine fachliche Änderung erfordert Anpassungen an vielen Stellen. Das deutet häufig auf verstreute Verantwortung oder eine ungünstige Modulgrenze hin.

### Primitive Obsession

Fachbegriffe werden als primitive Typen behandelt, obwohl eigene Typen Regeln und Sprache verbessern könnten.

### Duplicate Logic

Duplikation ist dann problematisch, wenn dieselbe Wissensregel mehrfach gepflegt werden muss. Ähnlicher Code allein beweist noch nicht, dass beide Stellen dieselbe Verantwortung besitzen.

### Speculative Generality

Abstraktionen, Erweiterungspunkte oder Frameworks werden für hypothetische zukünftige Anforderungen gebaut, ohne aktuellen Treiber.

## 3. Zahlen sind Hinweise, keine Gesetze

Methodenlänge, Anzahl Parameter, zyklomatische Komplexität oder Klassengröße können nützliche Signale liefern. Sie sind aber keine universellen Qualitätsgrenzen.

Ein 40-zeiliger, linearer Parser kann verständlicher sein als zehn künstlich extrahierte Methoden. Eine fünfzeilige Methode kann gleichzeitig architektonisch problematisch sein, wenn sie eine verbotene Abhängigkeit einführt.

Metriken starten eine Untersuchung; sie beenden sie nicht.

## 4. Refactoring schrittweise durchführen

Ein belastbarer Ablauf:

```text
Problem verstehen
→ gewünschtes Verhalten absichern
→ kleinste Strukturänderung durchführen
→ Tests ausführen
→ Auswirkungen messen/reviewen
→ nächsten Schritt entscheiden
```

Große „Cleanup“-Änderungen ohne fachlichen Anlass sind riskanter als inkrementelle Verbesserungen entlang realer Änderungsbedarfe.

## 5. Typische Refactorings

- Extract Method / Extract Class,
- Move Method / Move Field,
- Introduce Parameter Object,
- Replace Primitive with Value Object,
- Replace Conditional with Polymorphism, wenn echte stabile Variation besteht,
- Inline Class oder Remove Abstraction, wenn eine Abstraktion keinen Nutzen mehr trägt,
- Split Phase,
- Encapsulate Collection.

Das Refactoring folgt dem Problem. Ein Pattern ist nicht automatisch das Ziel.

## 6. Architektur-Smells

Manche Probleme liegen oberhalb einzelner Klassen:

- zyklische Modulabhängigkeiten,
- gemeinsamer Datenbankzugriff mehrerer fachlicher Module,
- zentrale Utility-Pakete ohne Ownership,
- technische Layer, die Fachgrenzen überdecken,
- Service-to-Service-Kommunikation ohne klare Verantwortlichkeit,
- wiederkehrende Ausnahmeentscheidungen gegen denselben Standard.

Hier reicht lokales Refactoring nicht. Es braucht Architekturarbeit und häufig eine Übergangsstrategie.

## 7. Prüffragen

1. Welcher reale Änderungs- oder Qualitätsbedarf löst das Refactoring aus?
2. Welche Verantwortung ist aktuell unklar oder verteilt?
3. Welche Tests schützen relevantes Verhalten?
4. Verbessert die Änderung Kopplung, Kohäsion oder Verständlichkeit tatsächlich?
5. Ist das Problem lokal oder architektonisch?
6. Kann die Änderung kleiner und schrittweiser erfolgen?
7. Erzeugen wir eine Abstraktion für einen realen oder nur hypothetischen Bedarf?

## 8. Verwandte Knowledge-Items

- `AK-008` — Objektorientierung und Verantwortung
- `AK-025` — SOLID
- `AK-026` — KISS, DRY, YAGNI
- `AK-084` — Kopplung und Kohäsion
- `AK-090` — Evolutionary Architecture

## 9. Quellen

- Martin Fowler, *Refactoring*, 2nd ed., 2018.
- Kent Beck, *Tidy First?*, 2023.
- Robert C. Martin, *Clean Code*, 2008 — als einflussreiche Heuristiksammlung, nicht als Norm.

## 10. Merksatz

> Ein Smell ist eine Einladung zum Nachdenken, kein automatischer Verstoß. Refactoring verbessert eine konkrete Struktur unter Erhalt des relevanten Verhaltens.
