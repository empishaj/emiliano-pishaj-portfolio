# Kommunikation mit Management und Stakeholdern

## Einordnung

Technische Qualität allein reicht in komplexen Organisationen nicht aus. Entscheidungen entstehen nur dann, wenn relevante Stakeholder das Problem, die Optionen, Risiken und Konsequenzen verstehen.

Deshalb ist Kommunikation für mich ein Teil von Architecture Leadership und nicht nur eine Präsentationsfähigkeit.

## Mein Grundsatz

**Ich übersetze technische Komplexität in entscheidbare Zusammenhänge, ohne sie fachlich zu verfälschen.**

---

## 1. Mit der Entscheidungsfrage beginnen

Management benötigt in der Regel nicht zuerst technische Details, sondern Klarheit darüber:

- Was ist das Problem?
- Warum muss jetzt entschieden werden?
- Welche Auswirkungen hat Nichtstun?
- Welche realistischen Optionen gibt es?
- Welche Risiken und Abhängigkeiten bestehen?
- Welche Entscheidung wird benötigt?

Erst danach folgen die technischen Details, die für die Entscheidung relevant sind.

---

## 2. Unterschiedliche Stakeholder brauchen unterschiedliche Perspektiven

Dasselbe Architekturthema kann für verschiedene Gruppen etwas anderes bedeuten.

### Fachseite
Was verändert sich an Fähigkeit, Prozess, Daten oder Verantwortung?

### Management
Welche Wirkung, Risiken, Kosten, Abhängigkeiten und Entscheidungsbedarfe bestehen?

### Entwicklung
Welche technischen Konsequenzen und Umsetzungsspielräume gibt es?

### Security und Datenschutz
Welche Schutz-, Kontroll- und Compliance-Anforderungen sind betroffen?

### Betrieb
Ist die Lösung betreibbar, beobachtbar und wiederherstellbar?

### Dienstleister
Welche konkreten Liefergegenstände, Standards und Abnahmekriterien gelten?

Gute Architekturkommunikation hält die Aussagen konsistent, verändert aber die Perspektive und Detailtiefe.

---

## 3. Fakten, Annahmen und Bewertungen trennen

Ich versuche klar zu unterscheiden zwischen:

- **Fakt:** Was ist belegt?
- **Annahme:** Was nehmen wir derzeit an?
- **Risiko:** Was könnte eintreten?
- **Bewertung:** Wie beurteilen wir eine Option anhand definierter Kriterien?
- **Entscheidung:** Was wurde verbindlich festgelegt?

Diese Trennung ist besonders wichtig, wenn Informationen unvollständig sind.

Unsicherheit darf nicht durch scheinbare Präzision verdeckt werden.

---

## 4. Optionen statt vorgefertigter Zustimmung

Wenn mehrere realistische Optionen existieren, möchte ich sie sichtbar machen.

Eine gute Entscheidungsvorlage enthält beispielsweise:

| Aspekt | Option A | Option B | Option C |
|---|---|---|---|
| Nutzen | | | |
| Risiken | | | |
| Kosten / Aufwand | | | |
| Abhängigkeiten | | | |
| Security / Compliance | | | |
| Betrieb | | | |
| Reversibilität | | | |
| Strategische Passung | | | |

Die Aufgabe des Architekten ist nicht, Entscheidern nur eine scheinbare Wahl zu präsentieren. Empfehlungen dürfen klar sein, müssen aber begründet und alternative Optionen fair dargestellt werden.

---

## 5. Risiken managementfähig formulieren

Ein technisches Risiko sollte nicht nur technisch beschrieben werden.

Statt:

> „Die Anwendung hat hohe Kopplung.“

ist entscheidungsrelevanter:

> „Änderungen an diesem Fachverfahren erfordern Anpassungen in mehreren abhängigen Systemen. Dadurch steigen Änderungsdauer, Testaufwand und das Risiko koordinierter Releases.“

Damit wird sichtbar, warum die technische Eigenschaft organisatorisch relevant ist.

---

## 6. Technische Schulden übersetzen

„Technical Debt“ ist kein Argument für sich.

Ich versuche zu erklären:

- Welche Veränderung wird dadurch langsamer oder riskanter?
- Welche Betriebsrisiken entstehen?
- Welche Kosten wiederholen sich?
- Welche strategischen Optionen werden eingeschränkt?
- Was passiert, wenn wir die Schuld bewusst akzeptieren?

So wird aus einer technischen Beobachtung eine steuerbare Entscheidung.

---

## 7. Schlechte Nachrichten früh kommunizieren

Risiken werden selten besser, wenn sie spät kommuniziert werden.

Ich halte es deshalb für wichtig, Probleme früh sichtbar zu machen – auch wenn noch nicht alle Antworten vorliegen.

Eine gute frühe Risikomeldung enthält:

1. Was wissen wir?
2. Was wissen wir noch nicht?
3. Welche Auswirkung könnte entstehen?
4. Was tun wir zur Klärung?
5. Wann brauchen wir eine Entscheidung?

Das schafft Transparenz, ohne unnötig zu alarmieren.

---

## 8. Executive Communication: kurz, aber nicht oberflächlich

Für Leitungsebenen versuche ich eine klare Struktur einzuhalten:

### Ausgangslage
Was ist heute relevant?

### Problem
Warum reicht der aktuelle Zustand nicht aus?

### Auswirkung
Welche fachliche, technische oder betriebliche Konsequenz hat das?

### Optionen
Welche realistischen Wege gibt es?

### Empfehlung
Welche Option halte ich anhand der Kriterien für sinnvoll und warum?

### Entscheidung
Was wird jetzt konkret benötigt?

Diese Struktur verhindert lange technische Vorträge ohne Handlungsbezug.

---

## 9. Workshops und Gremien moderieren

In Workshops ist mein Ziel nicht, selbst am meisten zu sprechen.

Ich versuche sicherzustellen, dass:

- die Entscheidungsfrage klar ist
- relevante Perspektiven vertreten sind
- Annahmen sichtbar werden
- Konflikte sachlich formuliert sind
- offene Punkte einen Owner bekommen
- Ergebnisse dokumentiert werden

Ein gutes Meeting endet mit mehr Klarheit als es begonnen hat.

---

## 10. Kommunikation im Public Sector

In Ministerien und Behörden ist Kommunikation häufig durch zusätzliche Anforderungen geprägt:

- formale Zuständigkeiten
- Gremien und Entscheidungswege
- unterschiedliche fachliche und technische Sprachen
- Dokumentations- und Nachvollziehbarkeitsanforderungen
- Security und Datenschutz
- Vergabe- und Dienstleisterbeziehungen

Deshalb ist es besonders wichtig, fachliche, technische und organisatorische Aussagen sauber voneinander zu trennen und trotzdem in ein gemeinsames Entscheidungsbild zu bringen.

Eine gute Architekturvorlage muss sowohl für technische Experten belastbar als auch für Entscheider verständlich sein.

---

## 11. Formate, die ich für sinnvoll halte

- One-Page Architecture Brief
- Architecture Vision
- Optionspapier
- Decision Paper
- ADR / Decision Log
- Risiko- und Abhängigkeitsübersicht
- Stakeholder Map
- Zielbild
- Roadmap mit Übergangsarchitekturen
- Architecture Review Summary

Nicht jedes Thema braucht alle Formate. Das Artefakt muss zum Entscheidungsbedarf passen.

---

## 12. Was ich bewusst vermeide

- technische Tiefe ohne Entscheidungsbezug
- Management-Folien ohne belastbare Fakten
- Risiken weichzuzeichnen
- Unsicherheit als Gewissheit darzustellen
- Stakeholder mit Framework-Jargon zu überladen
- Entscheidungen in langen Protokollen zu verstecken
- Alternativen unfair darzustellen, um Zustimmung zu einer bevorzugten Lösung zu erzwingen

---

## Kurzprinzip

**Gute Architekturkommunikation reduziert Komplexität nicht durch Vereinfachung der Wahrheit, sondern durch klare Struktur: Problem, Wirkung, Optionen, Risiken und Entscheidung.**
