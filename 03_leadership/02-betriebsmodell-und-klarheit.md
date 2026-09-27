# Betriebsmodell und Navigationsfähigkeit

## Einordnung

Dieses Dokument beschreibt, warum ich in Engineering- und IT-Organisationen zuerst auf das Betriebsmodell schaue.

Mit Betriebsmodell meine ich nicht das Organigramm. Ich meine die Art und Weise, wie Arbeit tatsächlich funktioniert: wie Verantwortung verteilt ist, wie Entscheidungen entstehen, wie Informationen fließen, wie Qualität gesichert wird und wie Menschen sich im System orientieren können.

Mein zentraler Begriff dafür ist **Navigationsfähigkeit**.

Eine Organisation ist navigationsfähig, wenn Menschen im Alltag nicht raten müssen:

- wer verantwortlich ist
- wer entscheiden darf
- welche Informationen maßgeblich sind
- welche Standards gelten
- wann andere beteiligt werden müssen
- wie Risiken eskaliert werden
- wo Entscheidungen dokumentiert sind
- woran gute Qualität erkannt wird

---

## Mein Grundsatz

**Ich schaue nicht zuerst auf einzelne Personen. Ich schaue zuerst auf das System, in dem sie arbeiten.**

Viele Probleme wirken wie individuelle Schwächen, entstehen aber durch strukturelle Ursachen:

- unklare Rollen
- widersprüchliche Ziele
- fehlender Kontext
- zu viele Übergaben
- langsame Entscheidungswege
- nicht dokumentierte Abhängigkeiten
- unklare Qualitätsmaßstäbe
- Verantwortung ohne Entscheidungsraum

Das bedeutet nicht, dass individuelles Verhalten unwichtig ist. Es bedeutet, dass ich erst verstehen möchte, ob das System das gewünschte Verhalten überhaupt ermöglicht.

---

## 1. Navigationsfähigkeit als Organisationsqualität

Ein Team oder eine Organisation ist navigationsfähig, wenn Menschen wissen, wie sie sich im System bewegen können.

Dazu gehören Antworten auf Fragen wie:

- Welche Teams und Rollen gibt es?
- Welche Verantwortung liegt wo?
- Welche Entscheidungen können dezentral getroffen werden?
- Welche Entscheidungen benötigen Abstimmung?
- Welche Architekturentscheidungen brauchen ein Review?
- Welche Informationen sind verbindlich?
- Wie werden Risiken sichtbar?
- Wie werden Incidents und Ausnahmen behandelt?
- Wo ist Wissen dokumentiert?
- Wie funktioniert die Zusammenarbeit mit Product, QA, DevOps, Security, Betrieb und Management?

Fehlt diese Orientierung, entstehen Wartezeiten und Absicherungsschleifen. Menschen fragen häufiger nach, eskalieren vorsichtshalber oder bauen auf Annahmen.

Das ist keine Kleinigkeit. Es beeinflusst Geschwindigkeit, Qualität und Verantwortungsübernahme direkt.

---

## 2. Typische Warnsignale eines schwachen Betriebsmodells

Ich nehme insbesondere diese Muster ernst:

- kleine Entscheidungen dauern unverhältnismäßig lange
- Teams warten regelmäßig aufeinander
- dieselben Personen werden zu Flaschenhälsen
- niemand fühlt sich für Querschnittsthemen verantwortlich
- Risiken werden spät oder weich formuliert
- Dokumentation existiert, wird aber nicht genutzt
- Product, Engineering und QA verstehen „fertig“ unterschiedlich
- Architekturentscheidungen werden wiederholt diskutiert
- Incidents führen immer wieder zu denselben Fragen
- neue Mitarbeitende benötigen lange, um Zusammenhänge zu verstehen
- Kennzahlen werden berichtet, ohne daraus Entscheidungen abzuleiten
- Teams besitzen Verantwortung, aber keine Entscheidungskompetenz

Solche Symptome sind für mich Hinweise auf fehlende Navigationsfähigkeit.

---

## 3. Rollen und Verantwortlichkeiten klären

Eine Rollenbeschreibung ist nur dann wertvoll, wenn sie im Alltag Orientierung gibt.

Für jede relevante Rolle oder Verantwortung sollten mindestens diese Fragen beantwortbar sein:

- Wofür bin ich verantwortlich?
- Welche Entscheidungen darf ich treffen?
- Welche Entscheidungen muss ich abstimmen?
- Welche Informationen brauche ich?
- Mit welchen Rollen arbeite ich regelmäßig zusammen?
- Wann muss ich eskalieren?
- Welche Ergebnisse werden von mir erwartet?

Ich vermeide Rollenmodelle, die nur aus Titeln bestehen. Entscheidend sind reale Entscheidungs- und Verantwortungsräume.

---

## 4. Entscheidungsräume definieren

Ownership funktioniert nur mit klaren Entscheidungsräumen.

Ich unterscheide deshalb zwischen:

1. Entscheidungen, die ein Team selbst treffen kann.
2. Entscheidungen, die mit anderen Teams abgestimmt werden müssen.
3. Entscheidungen, die Architektur, Security, Betrieb oder Management einbeziehen müssen.
4. Entscheidungen, die wegen Risiko, Kosten, Compliance oder strategischer Auswirkungen formal entschieden werden müssen.

Diese Unterscheidung verhindert zwei Extreme:

- jede Kleinigkeit wird eskaliert
- weitreichende Entscheidungen werden lokal getroffen, obwohl andere stark betroffen sind

Gute Governance bedeutet für mich nicht, möglichst viele Entscheidungen zu zentralisieren. Sie bedeutet, die richtige Entscheidung auf die richtige Ebene zu bringen.

---

## 5. Schnittstellen sichtbar machen

Reibung entsteht häufig an Schnittstellen – organisatorisch wie technisch.

Ich frage deshalb:

- Wo wechseln Informationen den Verantwortungsbereich?
- Wo gehen Kontext oder Verantwortung verloren?
- Welche Teams warten regelmäßig aufeinander?
- Welche technischen Schnittstellen sind zu eng gekoppelt?
- Welche Entscheidungen benötigen zu viele Beteiligte?
- Wo sind Service-, API- oder Datenverantwortlichkeiten unklar?

Klare Schnittstellen reduzieren Abstimmungskosten.

Diese Sicht ist eng mit *Team Topologies* verbunden: Teamgrenzen, Kommunikationswege und Abhängigkeiten beeinflussen direkt, wie gut Systeme verändert und betrieben werden können.

---

## 6. Standards als Entlastung

Standards sind hilfreich, wenn sie wiederkehrende Entscheidungen vereinfachen.

Gute Standards beantworten beispielsweise:

- Wie dokumentieren wir Architekturentscheidungen?
- Welche Qualitätsanforderungen gelten?
- Wie gestalten und versionieren wir APIs?
- Welche Security-Prüfungen sind verpflichtend?
- Welche Informationen braucht ein produktiver Service?
- Was muss vor einem Deployment erfüllt sein?
- Wie werden Ausnahmen entschieden?

Ein Standard ist nur dann gut, wenn er im Alltag verständlich und anwendbar bleibt.

**Gute Governance reduziert Denk- und Abstimmungsaufwand. Schlechte Governance erzeugt zusätzlichen Aufwand ohne Entscheidungsnutzen.**

---

## 7. Routinen machen das Betriebsmodell real

Ein Betriebsmodell wird nicht durch ein einmaliges Dokument wirksam.

Es wird durch wiederholbare Mechanismen sichtbar:

- 1:1s
- Retrospektiven
- Architektur-Reviews
- ADRs und Decision Logs
- Service Deep Dives
- DORA-Reviews
- Incident Reviews
- Chapter-Formate
- Onboarding-Routinen
- Security- und Quality-Gates
- Governance- und Eskalationswege

Routinen schaffen Verlässlichkeit. Sie verhindern, dass wichtige Themen vom Zufall abhängen.

---

## 8. Von Engineering Operating Model zu Enterprise Architecture

Die gleiche Denkweise lässt sich auf Enterprise Architecture übertragen.

Dort erweitert sich die Frage von Team- und Servicegrenzen auf die gesamte Organisation:

- Welche fachlichen Fähigkeiten werden benötigt?
- Welche Organisationseinheit trägt Verantwortung?
- Welche Prozesse setzen diese Fähigkeiten um?
- Welche Daten werden benötigt und wer verantwortet sie?
- Welche Anwendungen unterstützen den Prozess?
- Welche Integrationen und Plattformen sind kritisch?
- Welche Security-, Datenschutz- und Betriebsanforderungen gelten?
- Welche Gremien oder Rollen entscheiden?
- Welche Lieferanten oder externen Dienstleister sind beteiligt?

Damit entsteht eine durchgängige Kette:

**Auftrag → Capability → Prozess → Verantwortung → Daten → Anwendung → Integration → Technologie → Security → Betrieb → Entscheidung → Transformation.**

Das ist für mich Enterprise Architecture als Navigationssystem.

---

## 9. Navigationsfähigkeit im Behördenkontext

In Ministerien und Behörden ist Navigationsfähigkeit besonders relevant, weil Verantwortung häufig über mehrere organisatorische Grenzen verteilt ist.

Typische Perspektiven können sein:

- Fachseite
- IT
- Informationssicherheit
- Datenschutz
- Betrieb
- Projekt- oder Programmleitung
- Vergabe
- zentrale IT-Dienstleister
- externe Lieferanten
- Leitung und Gremien

Dabei ist nicht jede Perspektive automatisch entscheidungsbefugt. Gute Architekturarbeit muss sichtbar machen:

- wer Anforderungen einbringt
- wer betroffen ist
- wer prüft
- wer entscheidet
- wer umsetzt
- wer später betreibt

Genau hier verbindet sich Betriebsmodell mit Architecture Governance.

---

## 10. Minimaler Satz an Betriebsmodell-Artefakten

Je nach Kontext reichen oft wenige, gepflegte Artefakte:

- Stakeholder- und Rollenübersicht
- Entscheidungs- und Eskalationsmatrix
- Verantwortlichkeitsmodell
- Architekturprinzipien
- ADR-/Decision-Log
- Service- oder Systemübersicht
- Schnittstellen- und Abhängigkeitskarte
- Governance- und Review-Rhythmus
- Risiko- und Maßnahmenübersicht

Der Wert dieser Artefakte liegt nicht in ihrer Existenz, sondern darin, ob Menschen dadurch schneller und sicherer handeln können.

---

## Was ich bewusst vermeide

- Betriebsmodelle, die nur auf Papier funktionieren
- Rollen ohne echte Verantwortungs- oder Entscheidungsräume
- Governance ohne klaren Entscheidungszweck
- Standards ohne Ausnahmemechanismus
- Dokumentation ohne Eigentümer und Pflegeprozess
- Prozesse, bei denen niemand mehr weiß, wer entscheidet
- Begriffe wie Ownership, Empowerment oder Governance ohne konkrete Übersetzung in den Alltag

---

## Kurzprinzip

**Ein gutes Betriebsmodell macht Verantwortung sichtbar, Entscheidungen navigierbar und Zusammenarbeit vorhersehbarer. Enterprise Architecture erweitert dieses Prinzip vom Team auf die Organisation.**
