# Technische Zusammenarbeit und Architecture Leadership

## Einordnung

Technische Führung verändert sich mit wachsendem Verantwortungsbereich.

Als Entwickler liegt der Fokus stark auf eigener Umsetzung. Als Tech Lead geht es zusätzlich um technische Richtung und Qualität. In Engineering Leadership kommen Menschen, Delivery und Organisationssysteme hinzu. In Enterprise Architecture verschiebt sich die Aufgabe erneut: Der Schwerpunkt liegt auf organisationsübergreifender Entscheidungsfähigkeit.

Meine technische Erfahrung bleibt dabei wichtig, aber ihr Zweck verändert sich.

## Mein Grundsatz

**Technische Tiefe soll bessere Fragen und bessere Entscheidungen ermöglichen – nicht dazu führen, dass der Architekt jede Detailentscheidung selbst trifft.**

---

## 1. Technische Glaubwürdigkeit ohne Mikromanagement

Ich halte technische Glaubwürdigkeit für wichtig, besonders wenn Entscheidungen Auswirkungen auf Architektur, Security, Betrieb oder langfristige Wartbarkeit haben.

Gleichzeitig ist technische Führung dann schwach, wenn sie Spezialisten systematisch überschreibt.

Meine Rolle sehe ich deshalb darin:

- Probleme strukturiert zu verstehen
- relevante Fragen zu stellen
- Auswirkungen sichtbar zu machen
- Trade-offs zu moderieren
- Qualitätsmaßstäbe zu klären
- Spezialwissen einzubeziehen
- Entscheidungen nachvollziehbar zu machen

---

## 2. Von der Lösung zur Entscheidungsfrage

Technische Diskussionen springen häufig zu früh auf Lösungen.

Ich versuche zunächst zu klären:

- Welches Problem lösen wir?
- Welche fachliche Fähigkeit ist betroffen?
- Welche Qualitätsanforderungen sind relevant?
- Welche Randbedingungen gelten?
- Welche Risiken müssen beherrscht werden?
- Welche Optionen gibt es?
- Welche Konsequenzen haben diese Optionen?
- Wer muss entscheiden?

Damit wird aus einer Technologiefrage eine Architekturentscheidung.

---

## 3. Spezialisten richtig einbinden

Ein Enterprise Architect kann und soll nicht in jeder Domäne der tiefste Spezialist sein.

Deshalb ist die Fähigkeit entscheidend, Expertenwissen wirksam zusammenzuführen.

Beispiele:

- Security bewertet Bedrohungen und Schutzmaßnahmen.
- Betrieb bewertet Betriebsfähigkeit und Wiederherstellbarkeit.
- Entwickler bewerten Implementierungsrisiken.
- Fachseite bewertet fachliche Auswirkungen.
- Datenschutz bewertet datenschutzrechtliche Anforderungen.
- Plattformteams bewerten technische Standards und Betriebsmodelle.

Architecture Leadership verbindet diese Perspektiven, ohne sie fachlich zu vereinnahmen.

---

## 4. Trade-offs sichtbar machen

Viele technische Entscheidungen haben keine eindeutig beste Lösung.

Typische Spannungsfelder sind:

- Geschwindigkeit vs. langfristige Wartbarkeit
- Standardisierung vs. lokale Flexibilität
- Entkopplung vs. zusätzliche Komplexität
- Sicherheit vs. Bedienbarkeit
- Plattformstandard vs. Spezialanforderung
- kurzfristige Migration vs. nachhaltige Modernisierung

Meine Aufgabe ist nicht, diese Spannungen zu verstecken. Ich möchte sie so formulieren, dass Entscheider verstehen, welche Konsequenzen mit einer Option verbunden sind.

---

## 5. Architekturentscheidungen dokumentieren

Entscheidungen verlieren schnell ihren Kontext.

Deshalb nutze ich leichtgewichtige Formate wie ADRs und Decision Logs.

Eine belastbare Entscheidung sollte mindestens enthalten:

- Kontext
- Entscheidungsfrage
- Optionen
- Bewertungskriterien
- Entscheidung
- Begründung
- Konsequenzen
- offene Risiken
- Verantwortlichkeit

Dokumentation ist dabei kein Selbstzweck. Sie schützt die Organisation vor Wissensverlust und wiederkehrenden Grundsatzdiskussionen.

---

## 6. Technische Leitplanken statt Detailsteuerung

Ich bevorzuge Leitplanken, die Teams Entscheidungsfreiheit innerhalb klarer Grenzen geben.

Beispiele:

- API-Standards
- Security-Baselines
- Observability-Anforderungen
- CI/CD Quality Gates
- ADR-Pflicht für bestimmte Entscheidungsklassen
- standardisierte Plattformservices
- Mindestanforderungen an Betriebsdokumentation

Das Ziel ist nicht maximale Vereinheitlichung. Das Ziel ist, wiederkehrende Risiken und Grundsatzfragen zu reduzieren.

---

## 7. Architecture Reviews als gemeinsames Denken

Ein Architecture Review sollte kein Tribunal sein.

Ein gutes Review klärt:

- Ist das Problem richtig verstanden?
- Sind relevante Stakeholder berücksichtigt?
- Sind wesentliche Risiken sichtbar?
- Sind Security und Betrieb früh genug einbezogen?
- Sind Abhängigkeiten verstanden?
- Ist die Lösung mit bestehenden Leitplanken vereinbar?
- Welche Entscheidungen sind noch offen?

Die Qualität eines Reviews zeigt sich nicht daran, wie viele Findings entstehen, sondern daran, ob die Lösung danach belastbarer und die Entscheidungsgrundlage klarer ist.

---

## 8. Einfluss ohne Weisungsbefugnis

Architecture Leadership funktioniert häufig ohne disziplinarische Macht.

Das verändert den Führungsstil.

Wirksamkeit entsteht durch:

- Vorbereitung
- fachliche Glaubwürdigkeit
- transparente Argumentation
- gute Moderation
- faire Darstellung von Optionen
- Verlässlichkeit
- klare Dokumentation
- saubere Eskalation

Wer nur über Titel oder Governance-Gremien Einfluss erzeugen kann, wird in komplexen Organisationen schnell umgangen.

---

## 9. Public-Sector-Perspektive

In Ministerien und Behörden treffen häufig mehrere Verantwortungswelten aufeinander.

Technische Entscheidungen können gleichzeitig Auswirkungen haben auf:

- fachliche Verwaltungsaufgaben
- Datenschutz
- Informationssicherheit
- Betrieb
- Vergabe
- externe Dienstleister
- Interoperabilität
- Haushalts- und Projektplanung

Architecture Leadership bedeutet hier, diese Perspektiven früh zusammenzuführen und technische Diskussionen in nachvollziehbare Entscheidungsfragen zu übersetzen.

---

## 10. Was ich bewusst vermeide

- Architektur als persönliche Meinung
- Technologieentscheidungen ohne Problemverständnis
- Reviews erst kurz vor Umsetzung
- Spezialisten zu überstimmen, ohne ihre Argumente zu verstehen
- Architekturprinzipien ohne Ausnahmemechanismus
- technische Tiefe mit Detailkontrolle zu verwechseln
- Entscheidungen ohne dokumentierte Konsequenzen

---

## Kurzprinzip

**Architecture Leadership bedeutet für mich, technische Tiefe mit systemischem Denken zu verbinden und Entscheidungen so zu moderieren, dass Spezialwissen, Risiken und organisatorische Auswirkungen gemeinsam berücksichtigt werden.**
