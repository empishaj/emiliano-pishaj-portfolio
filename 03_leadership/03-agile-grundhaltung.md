# Adaptivität und agile Grundhaltung

## Einordnung

Agilität ist für mich keine Sammlung von Ritualen, sondern eine Fähigkeit zum kontrollierten Lernen unter Unsicherheit.

In komplexen IT- und Transformationsvorhaben ist selten alles zu Beginn vollständig bekannt. Anforderungen verändern sich, technische Erkenntnisse entstehen erst im Verlauf, Abhängigkeiten werden sichtbar, Rahmenbedingungen ändern sich und Betriebserfahrungen liefern neue Informationen.

Deshalb ist für mich nicht entscheidend, ob ein Team Scrum, Kanban oder ein anderes Vorgehensmodell nutzt. Entscheidend ist, ob die Organisation in der Lage ist:

- Annahmen sichtbar zu machen
- Risiken früh zu prüfen
- in sinnvollen Schritten zu liefern
- aus Feedback zu lernen
- Entscheidungen an neue Erkenntnisse anzupassen
- technische Qualität zu erhalten
- Verantwortung nah an den relevanten Kontext zu bringen

---

## Mein Grundsatz

**Ein Prozess ist nicht agil, nur weil er Sprints, Dailys oder Boards verwendet.**

Agilität zeigt sich daran, ob eine Organisation neue Informationen wirksam verarbeiten kann.

Ein Plan ist wichtig. Er wird problematisch, wenn er trotz neuer Erkenntnisse unverändert weiterverfolgt wird.

Deshalb verstehe ich Planung als Hypothese über den besten derzeit bekannten Weg – nicht als Garantie, dass dieser Weg unverändert bleibt.

---

## 1. Kleine Schritte reduzieren Risiko

In meiner Arbeit mit geschäftskritischen Systemen habe ich gelernt, wie teuer große, spät validierte Änderungen werden können.

Kleine, gut verstandene Schritte sind leichter:

- zu testen
- zu reviewen
- zurückzunehmen
- betrieblich zu beobachten
- fachlich zu validieren
- sicherheitstechnisch zu bewerten

Für mich bedeutet inkrementelles Arbeiten deshalb nicht, Vorhaben künstlich zu verkleinern. Es bedeutet, Risiko so zu schneiden, dass Lernen früher möglich wird.

---

## 2. Feedback ist ein Steuerungsinstrument

Feedback ist kein Zeichen unvollständiger Planung. Es ist ein Mechanismus zur Risikoreduktion.

Sinnvolle Feedbackpunkte sind beispielsweise:

- frühe fachliche Abstimmungen
- Architektur-Reviews
- technische Spikes
- API- oder Datenmodell-Reviews
- Security-Reviews
- Code Reviews
- Pilotierungen
- DORA-Reviews
- Incident Reviews
- Retrospektiven

Feedback ist dann wertvoll, wenn daraus eine bessere Entscheidung, ein geringeres Risiko oder eine veränderte Priorität entsteht.

---

## 3. Agile Arbeit braucht klare Verantwortung

Agilität bedeutet nicht, dass alles offen bleibt.

Teams müssen wissen:

- welches Ergebnis erreicht werden soll
- welche Randbedingungen gelten
- welche Entscheidungen sie selbst treffen dürfen
- welche Qualitätsanforderungen verbindlich sind
- welche Risiken nicht akzeptabel sind
- wann andere Beteiligte einbezogen werden müssen

Ohne diese Klarheit wird aus Flexibilität Beliebigkeit.

---

## 4. Technische Qualität ist Voraussetzung für Anpassungsfähigkeit

Eine Organisation kann nur dann schnell auf neue Anforderungen reagieren, wenn ihre Systeme veränderbar bleiben.

Deshalb gehören für mich technische Praktiken unmittelbar zur Adaptivität:

- verständlicher Code
- Refactoring
- automatisierte Tests
- CI/CD
- kleine Releases
- nachvollziehbare Architekturentscheidungen
- Security by Design
- Observability
- klare Schnittstellen
- dokumentierte Betriebsabläufe

Technische Schulden sind nicht automatisch falsch. Problematisch werden sie, wenn sie unsichtbar bleiben oder dauerhaft die Veränderbarkeit reduzieren.

---

## 5. DORA-Metriken als Lernhilfe

DORA-Metriken können Hinweise darauf geben, wie gut Delivery und Stabilität zusammenspielen.

Relevant sind insbesondere Fragen wie:

- Wie lange dauert eine Änderung bis zur produktiven Nutzung?
- Wie häufig können Änderungen sicher ausgeliefert werden?
- Wie oft verursachen Änderungen Störungen?
- Wie schnell kann sich ein System nach Fehlern erholen?

Ich nutze solche Kennzahlen nicht zur Bewertung einzelner Personen. Sie sind ein Spiegel für das Delivery-System.

Eine Metrik ist nur dann hilfreich, wenn daraus Analyse, Entscheidung und Verbesserung folgen.

---

## 6. Agilität bedeutet nicht Dokumentationsverzicht

Ich halte die Gleichsetzung von Agilität und Dokumentationsarmut für falsch.

Gerade in komplexen Systemen braucht es verständliche und gepflegte Informationen:

- Architekturentscheidungen
- Schnittstellenverträge
- Betriebswissen
- Risiken
- Verantwortlichkeiten
- technische Schulden
- relevante Annahmen

Die Frage ist nicht, ob dokumentiert wird, sondern **welche Information für spätere Entscheidungen und Zusammenarbeit wirklich gebraucht wird**.

---

## 7. Von agiler Delivery zu inkrementeller Transformation

In Enterprise Architecture erweitert sich die agile Perspektive.

Es geht nicht nur darum, Features inkrementell zu liefern. Ganze Systemlandschaften, Plattformen und Organisationen müssen häufig schrittweise verändert werden.

Dafür nutze ich die Logik von Übergangsarchitekturen:

**Ist-Zustand → kontrollierter Zwischenzustand → nächster Zwischenzustand → Zielarchitektur.**

Ein sinnvoller Zwischenzustand muss:

- fachlich nutzbar sein
- technisch betreibbar sein
- bekannte Risiken beherrschen
- Abhängigkeiten berücksichtigen
- zukünftige Schritte ermöglichen

Damit wird Adaptivität Teil der Transformationsarchitektur.

---

## 8. Adaptivität im Public Sector

Im Behördenkontext ist „maximaler Kundennutzen“ als alleinige Zielgröße zu kurz gegriffen.

Öffentliche IT muss mehrere Anforderungen gleichzeitig berücksichtigen, beispielsweise:

- gesetzlichen und fachlichen Auftrag
- Nachvollziehbarkeit
- Informationssicherheit
- Datenschutz
- Barrierefreiheit
- Betriebsfähigkeit
- Wirtschaftlichkeit
- Interoperabilität
- langfristige Wartbarkeit

Agile und inkrementelle Arbeit bedeutet deshalb nicht, verbindliche Anforderungen flexibel zu behandeln. Sie bedeutet, den Weg zur Erfüllung dieser Anforderungen so zu gestalten, dass Lernen und Risikoreduktion möglich bleiben.

---

## 9. Typische Anti-Patterns

### Ritual-Agilität
Viele Meetings, wenig Lernen.

### Vollplanung unter Unsicherheit
Monate im Voraus wird so detailliert geplant, als könnten neue Erkenntnisse nichts verändern.

### Beliebigkeit
Prioritäten und Ziele ändern sich ständig ohne nachvollziehbare Begründung.

### Technische Qualität wird verschoben
Kurzfristige Geschwindigkeit erzeugt langfristig sinkende Veränderbarkeit.

### Feedback ohne Konsequenz
Retrospektiven und Reviews erzeugen Erkenntnisse, aber keine Maßnahmen.

### Verantwortung ohne Entscheidungsraum
Teams sollen liefern, dürfen aber relevante Entscheidungen nicht selbst treffen.

---

## 10. Fragen, die ich bei komplexen Vorhaben stelle

- Was wissen wir sicher?
- Was nehmen wir nur an?
- Welche Annahme birgt das größte Risiko?
- Welcher kleinste sinnvolle Schritt liefert neue Erkenntnis?
- Welche Entscheidung ist reversibel?
- Welche Entscheidung ist schwer umkehrbar?
- Welche Qualitätsanforderungen sind nicht verhandelbar?
- Welche Zwischenarchitektur ist tatsächlich betreibbar?
- Wann prüfen wir, ob unsere Annahme noch trägt?

---

## Kurzprinzip

**Adaptivität bedeutet für mich, unter Unsicherheit strukturiert zu lernen, Risiken früh zu reduzieren und Transformation in kontrollierbaren, qualitativ tragfähigen Schritten umzusetzen.**
