---
id: AK-090
legacy_ids:
  - ADR-090
title: Evolutionary Architecture, Fitness Functions und kontrollierte Veränderbarkeit
artifact_type: architecture-principle
domain: transformation
status: active
maturity: reviewed
normative_level: recommended
last_validated: 2026-10-05
review_trigger:
  - neue Transformationsstrategie
  - wiederkehrende Architekturdrift
  - größere Plattform- oder Domänenänderung
  - geänderte Qualitätsziele oder regulatorische Rahmenbedingungen
  - wesentliche Veränderung von Betriebs-, Lieferanten- oder Datenarchitektur
---

# AK-090 — Evolutionary Architecture, Fitness Functions und kontrollierte Veränderbarkeit

## 1. Zweck dieses Dokuments

Architektur muss zwei scheinbar widersprüchliche Anforderungen gleichzeitig erfüllen:

1. Sie muss **genügend Stabilität** schaffen, damit Systeme zuverlässig, sicher und verständlich betrieben werden können.
2. Sie muss **genügend Veränderbarkeit** erhalten, damit Systeme auf neue Anforderungen, Erkenntnisse, Technologien, Gesetze und Organisationsstrukturen reagieren können.

Zwei Extreme sind dabei gleichermaßen problematisch:

~~~text
alles möglichst vollständig vorab festlegen
vs.
Architektur ausschließlich emergent entstehen lassen
~~~

Das erste Extrem unterschätzt Lernen und Unsicherheit.

Das zweite Extrem unterschätzt die Kosten unkontrollierter Drift.

Evolutionary Architecture versucht deshalb nicht, Architekturplanung abzuschaffen.

Sie versucht:

> **Veränderung bewusst zu ermöglichen, zu führen und kontinuierlich gegen wichtige Architekturmerkmale zu prüfen.**

Die Leitfrage lautet:

> **Wie kann sich ein System schrittweise verändern, ohne dass bei jeder Änderung die wichtigen fachlichen, technischen und betrieblichen Eigenschaften verloren gehen?**

---

# Teil I — Begriff und wissenschaftlich-praktische Einordnung

## 2. Definition von Evolutionary Architecture

Ford, Parsons, Kua und Sadalage definieren Evolutionary Architecture als Architektur, die:

> **guided, incremental change across multiple dimensions**

unterstützt.

Diese Definition enthält drei zentrale Elemente:

1. **guided**
2. **incremental**
3. **multiple dimensions**

Alle drei sind notwendig.

---

## 3. Guided Change

Evolution darf nicht bedeuten:

> Das System verändert sich irgendwie und wir schauen später, was passiert ist.

Guided Change bedeutet:

- wichtige Architekturmerkmale sind bekannt,
- gewünschte Grenzen sind explizit,
- Veränderungen werden gegen diese Merkmale geprüft,
- unerwünschte Verschlechterung wird sichtbar.

Das zentrale Werkzeug dafür sind Architecture Fitness Functions.

---

## 4. Incremental Change

Evolutionary Architecture bevorzugt Veränderungen, die:

- klein genug für Feedback sind,
- reversibel oder begrenzt riskant sind,
- schrittweise ausgeliefert werden können,
- kontinuierliches Lernen ermöglichen.

Das bedeutet nicht:

> Jede Änderung muss klein sein.

Einige Veränderungen sind zwangsläufig groß.

Die Architekturfrage lautet dann:

> Kann die große Veränderung in überprüfbare Übergänge zerlegt werden?

---

## 5. Multiple Dimensions

Architektur entwickelt sich nicht nur im Code.

Betroffen sein können gleichzeitig:

- fachliche Struktur,
- Daten,
- APIs,
- Events,
- Security,
- Performance,
- Availability,
- Deployment,
- Observability,
- Betrieb,
- Kosten,
- Teamgrenzen,
- Lieferanten,
- Governance.

Eine Architektur ist nicht wirklich evolvierbar, wenn zum Beispiel:

~~~text
Code leicht änderbar
aber
Datenmodell praktisch unmigrierbar
~~~

oder:

~~~text
Services unabhängig
aber
jeder Release braucht koordinierte Vertragsänderungen
~~~

oder:

~~~text
technische Architektur flexibel
aber
Vergabe bindet fünf Jahre an ein proprietäres Produkt
~~~

---

## 6. Evolution ist nicht permanente Veränderung

Ein häufiger Irrtum:

> Evolutionary Architecture bedeutet, Architektur ständig umzubauen.

Nein.

Ein guter evolutionärer Zustand kann lange stabil bleiben.

Der relevante Punkt ist:

> Wenn Veränderung notwendig wird, kann sie kontrolliert stattfinden.

---

## 7. Evolvability als Architekturqualität

Evolvability kann als Fähigkeit verstanden werden, auf Veränderungen zu reagieren, ohne dass die Kosten oder Risiken unverhältnismäßig steigen.

Sie hängt unter anderem ab von:

- Modularität,
- Kopplung,
- Datenmigration,
- Testbarkeit,
- Automation,
- Observability,
- Governance,
- Reversibilität.

Es existiert keine einzelne universelle Metrik für Evolvability.

---

# Teil II — Veränderungsfähigkeit als Systemmerkmal

## 8. Veränderung ist ein normaler Systemzustand

Software verändert sich durch:

- neue Fachanforderungen,
- geänderte Gesetze,
- neue Integrationspartner,
- neue Security-Anforderungen,
- neue Lastprofile,
- Technologie-End-of-Life,
- Cloud- oder Plattformwechsel,
- organisatorische Veränderungen,
- Lieferantenwechsel,
- neue Erkenntnisse aus Betrieb und Incidents.

Architektur muss damit rechnen.

---

## 9. Reversibilität

Eine wichtige Eigenschaft evolutionärer Architektur ist Reversibilität.

Frage:

> Wie teuer ist es, diese Entscheidung später zu korrigieren?

Beispiele:

### leicht reversibel

~~~text
interne Library wechseln
~~~

### schwieriger reversibel

~~~text
öffentliches API brechen
~~~

### sehr schwer reversibel

~~~text
fachliche Daten in proprietäres Format migrieren
~~~

---

## 10. Reversibilität beeinflusst Governance

Je schwerer eine Entscheidung rückgängig zu machen ist, desto mehr:

- Analyse,
- Governance,
- Evidence,
- Stakeholder-Einbindung

ist typischerweise gerechtfertigt.

Eine triviale reversible Entscheidung braucht nicht dieselbe Governance wie eine langfristige Plattformbindung.

---

## 11. Optionalität

Evolutionäre Architektur versucht, relevante Optionen offen zu halten.

Aber:

> Optionalität ist nicht kostenlos.

Jede zusätzliche Option erzeugt möglicherweise:

- Abstraktion,
- Testaufwand,
- Komplexität,
- Governance.

Daher gilt:

> Nur Optionen erhalten, deren möglicher zukünftiger Wert die heutigen Kosten rechtfertigt.

---

# Teil III — Architecture Fitness Functions

## 12. Definition

Ford, Parsons, Kua und Sadalage definieren eine Architecture Fitness Function sinngemäß als:

> einen objektiven Integritätscheck für ein oder mehrere Architekturmerkmale.

Fitness Functions sollen nicht Architekturqualität als Ganzes bewerten.

Sie schützen konkrete Eigenschaften.

---

## 13. Warum Fitness Functions wichtig sind

Viele Architekturregeln existieren sonst nur als:

- Wiki-Seite,
- Guideline,
- Diagramm,
- Architekturboard-Erinnerung.

Beispiel:

~~~text
Domain darf keine Adapter importieren.
~~~

Wenn niemand dies prüft, kann die Architektur langsam abweichen.

Eine Fitness Function macht die Regel überprüfbar.

---

## 14. Von Qualitätsziel zu Fitness Function

Die Reihenfolge ist:

~~~text
Business-/Qualitätsziel
      ↓
relevantes Architekturmerkmal
      ↓
Messgröße oder überprüfbares Kriterium
      ↓
Fitness Function
      ↓
Evidence
~~~

Nicht:

~~~text
Tool vorhanden
→ Metrik auswählen
→ Architekturziel erfinden
~~~

---

## 15. Strukturelle Fitness Function

Ziel:

~~~text
Domain darf Adapter nicht importieren.
~~~

Mögliche Prüfung:

- ArchUnit,
- Modulgraph,
- Buildregel.

---

## 16. API-Fitness Function

Ziel:

~~~text
Breaking Changes an öffentlicher API
müssen erkannt werden.
~~~

Mögliche Prüfung:

- OpenAPI Diff,
- Consumer Contract Test,
- Schema Compatibility Check.

---

## 17. Event-Fitness Function

Ziel:

~~~text
Event Contract bleibt kompatibel.
~~~

Mögliche Prüfung:

- AsyncAPI-Schema,
- Schema Registry,
- Contract Tests.

---

## 18. Performance-Fitness Function

Ziel:

~~~text
kritischer Use Case erfüllt
vereinbartes Latenzziel
unter definierter Last.
~~~

Mögliche Prüfung:

- Load Test,
- Runtime SLI,
- SLO.

Wichtig:

Der Schwellenwert kommt aus dem Qualitätsziel.

Nicht aus einer universellen Best Practice.

---

## 19. Availability-Fitness Function

Beispiel:

~~~text
Service erfüllt vereinbartes Availability-Ziel.
~~~

Evidence:

- SLI,
- Error Budget,
- Incidentdaten.

---

## 20. Recovery-Fitness Function

Ziel:

~~~text
Wiederherstellung innerhalb vereinbartem RTO/RPO.
~~~

Evidence:

- Restore Test,
- Disaster-Recovery Exercise,
- Recovery Log.

---

## 21. Security-Fitness Function

Beispiel:

~~~text
keine kritischen bekannten Schwachstellen
im Release-Artefakt
nach definierter Policy.
~~~

Mögliche Mechanismen:

- SCA,
- Container Scan,
- Policy Check,
- Security Tests.

---

## 22. Data-Fitness Function

Beispiel:

~~~text
Migration ist rückwärtskompatibel
während der Übergangsphase.
~~~

Prüfung:

- Expand/Contract Tests,
- Migration Dry Run,
- Schema Compatibility.

---

## 23. Governance-Fitness Function

Nicht jede Fitness Function ist Code.

Beispiel:

~~~text
jede externe API
besitzt einen benannten Owner.
~~~

Prüfung:

- API Catalog,
- Governance Check.

---

## 24. Fitness Functions können manuell sein

Ein Architecture Review kann ebenfalls eine Fitness Function sein, wenn:

- Kriterium klar ist,
- Ergebnis nachvollziehbar ist,
- Review reproduzierbar ist.

Automatisierung ist wertvoll, aber nicht zwingend.

---

## 25. Atomic Fitness Function

Prüft einen eng begrenzten Aspekt.

Beispiel:

~~~text
kein Zyklus zwischen Modulen A, B, C
~~~

---

## 26. Holistic Fitness Function

Prüft ein Gesamtsystemmerkmal.

Beispiel:

~~~text
End-to-End-Recovery funktioniert.
~~~

Dafür können mehrere Systeme und Teams beteiligt sein.

---

## 27. Triggered Fitness Function

Wird bei Ereignis ausgeführt.

Beispiele:

- Build,
- Pull Request,
- Release,
- Migration.

---

## 28. Continuous Fitness Function

Wird fortlaufend beobachtet.

Beispiele:

- Runtime SLI,
- Drift Detection,
- Cost Monitoring,
- Security Posture.

---

## 29. Fitness Functions altern

Eine Fitness Function kann selbst veraltet sein.

Beispiel:

~~~text
P95 < 500 ms
~~~

war sinnvoll, solange:

- User Journey gleich,
- Lastprofil gleich,
- Nutzererwartung gleich

war.

Ändert sich der Kontext, muss auch die Fitness Function überprüft werden.

---

# Teil IV — Keine universellen Schwellen

## 30. Warum Magic Numbers gefährlich sind

Beispiele:

~~~text
Coverage > 80 %
Cyclomatic Complexity < 10
P95 < 500 ms
100 RPS
~~~

können in einem System sinnvoll sein.

Sie sind keine allgemeinen Architekturgesetze.

---

## 31. Schwellen aus Anforderungen ableiten

Beispiel:

~~~text
Fachlicher Prozess:
Sachbearbeitung benötigt Antwort
innerhalb 2 Sekunden
~~~

Dann kann daraus ein technisches Budget entstehen.

Nicht umgekehrt.

---

## 32. Measurement ohne Kontext ist kein Governance-Modell

Eine Metrik beantwortet:

> Was messen wir?

Sie beantwortet nicht automatisch:

> Warum ist dieser Wert akzeptabel?

---

# Teil V — Transition Architectures

## 33. Zielarchitektur allein reicht nicht

Enterprise Transformation sieht selten so aus:

~~~text
Ist
→ Ziel
~~~

Realistischer:

~~~text
Ist
 ↓
Transition A
 ↓
Transition B
 ↓
Transition C
 ↓
Ziel
~~~

---

## 34. Was ist eine Transition Architecture?

Eine Transition Architecture beschreibt einen bewusst geplanten Zwischenzustand.

Dieser Zwischenzustand muss selbst:

- betreibbar,
- sicher,
- supportbar,
- dokumentiert,
- verantwortet

sein.

---

## 35. Temporär kann Jahre dauern

In öffentlichen IT-Landschaften gilt besonders:

> Übergangslösungen leben oft deutlich länger als geplant.

Deshalb ist:

~~~text
ist nur temporär
~~~

keine Rechtfertigung für:

- fehlende Security,
- fehlendes Monitoring,
- unklare Ownership,
- manuelle Notlösungen.

---

## 36. Transition State braucht Exit-Kriterium

Jeder Übergang sollte beantworten:

- Warum existiert er?
- Wie lange darf er existieren?
- Was muss als Nächstes passieren?
- Welche Evidence erlaubt den nächsten Schritt?
- Wann wird Altbestand entfernt?

---

## 37. Beispiel

~~~text
Transition A:
neue API vor Legacy-System

Transition B:
erste Capability im neuen System

Transition C:
Datenmigration vollständig

Target:
Legacy-System stillgelegt
~~~

---

# Teil VI — Roadmap und Capability Evolution

## 38. Roadmap ist keine Feature-Liste

Eine Architekturroadmap sollte zeigen:

- Capability-Veränderungen,
- Architekturzustände,
- Abhängigkeiten,
- Entscheidungszeitpunkte,
- Risiken,
- Decommissioning.

---

## 39. Roadmap mit Evidence Gates

Beispiel:

~~~text
T1
neuer Adapter produktiv
Evidence:
Contract Tests grün

T2
50 % Consumer migriert
Evidence:
Consumer Inventory

T3
100 % Consumer migriert
Evidence:
Legacy Traffic = 0

T4
Legacy abgeschaltet
Evidence:
Betriebsartefakte entfernt
~~~

---

## 40. Transformation ist mehr als Migration

Eine technische Migration ohne geänderte Verantwortung oder Delivery-Fähigkeit kann alte Probleme nur auf neue Technologie übertragen.

---

# Teil VII — Strangler Fig

## 41. Grundidee

Martin Fowler verwendet die Strangler-Fig-Metapher für eine schrittweise Modernisierung.

Statt:

~~~text
altes System
→ Big Bang
→ neues System
~~~

entsteht:

~~~text
altes System
+
neue Komponenten
↓
schrittweise Verlagerung
↓
Altbestand entfernen
~~~

---

## 42. Ziel

Strangler Fig reduziert Transformationsrisiko durch:

- Koexistenz,
- inkrementelle Lieferung,
- frühes Feedback,
- schrittweise Ablösung.

---

## 43. Kein automatisches Microservice-Muster

Strangler Fig bedeutet nicht automatisch:

> Monolith muss in Microservices zerlegt werden.

Auch folgende Zielarchitekturen sind möglich:

- neuer Modulith,
- neue Plattform,
- SaaS-Ersatz,
- neue Servicegrenze.

---

## 44. Geeignete Grenze notwendig

Eine gute Strangler-Grenze kann sein:

- User Journey,
- Capability,
- API Route,
- Datenbereich,
- fachlicher Prozessschritt.

Ohne geeignete Abgrenzung kann Strangling komplizierter werden als die Ausgangslage.

---

## 45. Coexistence ist echte Architektur

Während Migration leben Alt und Neu parallel.

Dann entstehen Fragen:

- Datenkonsistenz,
- Routing,
- Identity,
- Observability,
- Ownership,
- Rollback,
- Support.

Diese Phase muss bewusst entworfen werden.

---

## 46. Elimination ist Teil des Musters

Modernisierung ist nicht abgeschlossen, wenn neue Funktion produktiv ist.

Sie ist abgeschlossen, wenn alte Funktion:

- nicht mehr genutzt,
- nicht mehr deployed,
- nicht mehr betrieben,
- nicht mehr bezahlt

wird.

---

# Teil VIII — Branch by Abstraction

## 47. Grundidee

Branch by Abstraction ermöglicht große interne Änderungen, während das System regelmäßig lieferbar bleibt.

---

## 48. Ablauf

~~~text
1. bestehende Abhängigkeit identifizieren
2. stabile Abstraktion einführen
3. Consumer auf Abstraktion umstellen
4. neue Implementierung ergänzen
5. kontrolliert umschalten
6. alte Implementierung entfernen
7. temporäre Abstraktion ggf. vereinfachen
~~~

---

## 49. Wann sinnvoll?

Besonders bei tief eingebetteten Komponenten:

- Library,
- Framework,
- Persistence Layer,
- internem Service.

---

## 50. Abstraktion muss semantisch stimmen

Eine schlechte Abstraktion:

~~~text
LegacyWrapper
~~~

die nur jede alte Methode 1:1 weiterreicht, schafft möglicherweise keine stabile Designgrenze.

---

# Teil IX — Feature Flags und progressive Veränderung

## 51. Feature Flags

Flags können helfen:

- neue Logik schrittweise zu aktivieren,
- Nutzergruppen zu trennen,
- Risiko zu begrenzen,
- schnellen Rollback zu ermöglichen.

---

## 52. Flags sind temporäre Architektur

Ein Flag erzeugt mindestens zwei mögliche Ausführungspfade.

Das erhöht:

- Testkombinationen,
- kognitive Last,
- Runtime-Komplexität.

---

## 53. Flag braucht Lifecycle

Für jedes Flag:

- Owner,
- Zweck,
- Erstellungsdatum,
- Exit-Kriterium,
- Removal Task.

---

## 54. Permanenter Flag-Friedhof

Warnsignal:

~~~text
flag_legacy_mode_2019
flag_new_api_beta
flag_temp_fix
~~~

ohne klare Entfernung.

Evolutionäre Architektur verlangt auch Bereinigung.

---

# Teil X — Daten evolutionär verändern

## 55. Daten sind häufig der schwierigste Teil

Code kann neu deployed werden.

Persistente Daten bleiben.

Daher kann ein System technisch modular sein und trotzdem durch sein Datenmodell kaum evolvierbar.

---

## 56. Expand and Contract

Ein typisches Migrationsmuster:

~~~text
Expand:
neues Schema zusätzlich unterstützen

Migrate:
Daten und Consumer umstellen

Contract:
altes Schema entfernen
~~~

---

## 57. Dual Write ist riskant

Während Migration kann die Idee entstehen:

~~~text
Alt schreiben
+
Neu schreiben
~~~

Das erzeugt:

- Konsistenzrisiko,
- Fehlerfälle,
- Reconciliation.

Dual Write braucht bewusstes Design.

---

## 58. Data Backfill

Backfill muss klären:

- Laufzeit,
- Last,
- Fehlerwiederholung,
- Validierung,
- Audit,
- Rollback.

---

## 59. Datenmigration ist Business Migration

Daten tragen fachliche Bedeutung.

Eine Migration kann deshalb:

- Regeln neu interpretieren,
- historische Werte transformieren,
- rechtliche Relevanz verändern.

Das braucht fachliche Ownership.

---

# Teil XI — Architecture Decision Lifecycle

## 60. Architekturentscheidungen altern

Eine Entscheidung kann zum Zeitpunkt T0 richtig sein.

Bei T1 ändern sich Annahmen.

Beispiel:

~~~text
T0:
1 interner Consumer

T1:
30 externe Consumer
~~~

Die gleiche API-Strategie kann dann unpassend werden.

---

## 61. Review Trigger

Sinnvolle Trigger:

- Lastprofil ändert sich,
- neue regulatorische Vorgabe,
- Technologie End-of-Life,
- Incident widerlegt Annahme,
- Teamgrenze ändert sich,
- Kosten steigen,
- neuer Consumer-Typ,
- Transition erreicht nächste Phase.

---

## 62. ADR nicht rückwirkend umschreiben

Eine akzeptierte Entscheidung dokumentiert historischen Kontext.

Wenn sie ersetzt wird:

~~~text
ADR-17
status: superseded by ADR-42
~~~

statt:

> alte Entscheidung so bearbeiten, als wäre sie nie getroffen worden.

---

## 63. Decision Freshness

Nicht jede Entscheidung braucht periodischen Kalenderreview.

Besser:

> Review bei relevanter Kontextänderung.

---

# Teil XII — Architecture Drift

## 64. Was ist Drift?

Drift bedeutet:

> reale Architektur weicht von vereinbarten Architekturannahmen oder Standards ab.

---

## 65. Beispiele

- unerlaubte Modulabhängigkeit,
- Shared-DB-Zugriff entsteht,
- API außerhalb Standard,
- manuelle Produktionsänderung,
- Security-Ausnahme bleibt dauerhaft,
- SLO wird systematisch verfehlt.

---

## 66. Drift ist nicht automatisch Fehler

Drift kann bedeuten:

1. Umsetzung ist falsch.
2. Standard ist falsch.
3. Kontext hat sich verändert.
4. Zielarchitektur ist unrealistisch.

Professionelle Governance prüft alle Möglichkeiten.

---

## 67. Intentional Drift

Manchmal wird bewusst von Standard abgewichen.

Dann braucht es:

- Begründung,
- Owner,
- Risiko,
- Exit oder Review Trigger.

---

## 68. Undocumented Drift

Gefährlicher ist:

~~~text
niemand wusste,
dass die Architektur sich verändert hat.
~~~

---

# Teil XIII — Technical Debt

## 69. Ursprung der Metapher

Technical Debt ist eine Metapher, die Ward Cunningham geprägt hat.

Fowler beschreibt die Idee so:

> interne Qualitätsdefizite können zukünftige Änderungen verteuern; dieser zusätzliche Änderungsaufwand entspricht der "Zinszahlung".

---

## 70. Nicht jede Unschönheit ist gleich wichtig

Ein hässlicher, aber stabiler Codebereich kann geringe praktische Wirkung besitzen.

Ein kleiner Smell in einem täglich geänderten Hotspot kann sehr teuer sein.

---

## 71. Debt Interest

Praktisch relevant:

~~~text
Wie stark verteuert diese Abweichung
zukünftige Änderung?
~~~

---

## 72. Debt Principal

Frage:

> Was kostet die Beseitigung?

---

## 73. Debt Governance

Ein sinnvoller Debt-Eintrag enthält:

- Beschreibung,
- Ursache,
- Wirkung,
- Risiko,
- Owner,
- Trigger,
- Evidence,
- Exit.

---

## 74. Beispiel

~~~text
Debt:
Legacy SOAP Adapter bleibt bestehen

Nutzen:
Migration wird inkrementell möglich

Kosten:
zusätzlicher Betrieb
Security-Patching
Mapping

Exit Trigger:
letzter Consumer migriert

Evidence:
Consumer Inventory = 0
Legacy Traffic = 0
~~~

---

## 75. Bewusste Debt kann rational sein

Evolutionäre Architektur verbietet bewusst eingegangene Schuld nicht.

Sie verlangt:

> Schuld muss sichtbar und steuerbar sein.

---

# Teil XIV — Architecture Fitness Function Governance

## 76. Wer definiert Fitness Functions?

Nicht nur Architekten.

Je nach Eigenschaft:

- Fachseite,
- Security,
- Betrieb,
- Plattform,
- Entwickler,
- Datenschutz,
- Architektur.

---

## 77. Wer besitzt sie?

Jede relevante Fitness Function braucht:

- Owner,
- Quelle,
- Schwelle,
- Ausführungsort,
- Reaktion bei Verletzung.

---

## 78. Fehlerreaktion

Nicht jede Verletzung muss Build blockieren.

Mögliche Reaktionen:

~~~text
Warning
Review Required
Release Block
Incident
Exception Process
~~~

---

## 79. Risikoabhängige Enforcement-Stufe

Beispiel:

### kritische Security Policy

~~~text
Block
~~~

### Architekturtrend

~~~text
Warning + Review
~~~

### strategische Zielabweichung

~~~text
Governance Discussion
~~~

---

# Teil XV — Observability als Evolution Evidence

## 80. Produktion liefert Architekturfeedback

Ein Architekturmodell kann behaupten:

> Service A ist unabhängig.

Runtime-Traces zeigen vielleicht:

~~~text
A → B → C → D → E
~~~

für jeden Request.

Dann existiert tatsächliche Runtime Coupling.

---

## 81. Runtime Evidence

Mögliche Signale:

- Latenz,
- Fehler,
- Retry Rate,
- Dependency Graph,
- Queue Lag,
- Saturation,
- Cost.

---

## 82. Architekturannahme versus Realität

Ein guter evolutionärer Prozess vergleicht:

~~~text
Architecture Intent
vs.
Runtime Evidence
~~~

---

# Teil XVI — Modernisierung ist Decommissioning

## 83. Altbestand aktiv entfernen

Neue Architektur plus Legacy parallel bedeutet:

> mehr statt weniger Komplexität.

Nur Decommissioning realisiert den Nutzen.

---

## 84. Was stillgelegt werden muss

Nicht nur Code.

Auch:

- Infrastruktur,
- Datenkopien,
- Jobs,
- Zertifikate,
- Monitoring,
- Supportverträge,
- Accounts,
- Runbooks,
- Dokumentation.

---

## 85. Decommission Evidence

Beispiel:

~~~text
Traffic = 0
Consumers = 0
Data export complete
Retention fulfilled
Infrastructure removed
Contract terminated
~~~

---

# Teil XVII — Behördenkontext

## 86. Warum Evolution besonders relevant ist

Behörden besitzen häufig:

- sehr langlebige Fachverfahren,
- gesetzlich gebundene Prozesse,
- externe Register,
- föderale Schnittstellen,
- hohe Sicherheitsanforderungen,
- mehrere Dienstleister,
- lang laufende Verträge,
- begrenzte Big-Bang-Möglichkeiten.

---

## 87. Gesetzesänderung als Evolution Trigger

Eine neue Rechtslage kann verändern:

- Prozess,
- Daten,
- Entscheidung,
- Frist,
- Schnittstelle.

Architektur muss auf solche Veränderungen reagieren können.

---

## 88. Vergabezyklen als Architekturtreiber

Ein Lieferantenvertrag kann Veränderung verzögern.

Deshalb gehören in eine Transformationsroadmap:

- Vertragslaufzeiten,
- Ausschreibungsfenster,
- Übergaben,
- Exit-Fähigkeit.

---

## 89. Parallelbetrieb

Behörden brauchen häufig längere Parallelphasen.

Gründe:

- sichere Migration,
- Altverfahren müssen weiterlaufen,
- externe Partner migrieren langsamer,
- rechtliche Übergangsregel.

Parallelbetrieb braucht eigenes Betriebsmodell.

---

## 90. Fachliche Nachweisbarkeit

Bei Migration muss oft nachvollziehbar bleiben:

- welche Regelversion galt,
- welcher Datenstand genutzt wurde,
- welches System entschieden hat,
- welche Übergangsregel aktiv war.

---

## 91. Transition Architecture im Behördenkontext

Ein guter Zwischenzustand beantwortet:

~~~text
wer betreibt?
wer entscheidet?
welche Daten sind führend?
welcher Prozess gilt?
welche Security Controls?
welcher Rollback?
welcher Exit?
~~~

---

# Teil XVIII — Lieferanten- und Plattformwechsel

## 92. Vendor Transition

Bei Providerwechseln sind relevant:

- Datenportabilität,
- Vertragssemantik,
- Mapping,
- Betriebsübergabe,
- Exit-Kosten,
- Wissenstransfer.

---

## 93. Adapter kann Vendor Coupling begrenzen

~~~text
Domain
→ internal Port
→ Vendor Adapter
→ Vendor API
~~~

Das entfernt nicht jede Vendor-Abhängigkeit.

Es lokalisiert einen Teil davon.

---

## 94. Plattformmigration

Beispiel:

~~~text
Platform A
→ Übergangsphase
→ Platform B
~~~

Fragen:

- Workload Compatibility,
- Identity,
- Networking,
- Observability,
- Deployment,
- Daten.

---

# Teil XIX — Evolution und Security

## 95. Security darf nicht nachträglich folgen

Evolution bedeutet nicht:

~~~text
erst Migration
später Security
~~~

Jeder Transition State muss ausreichend sicher sein.

---

## 96. Security Fitness Functions

Beispiele:

- Dependency Policy,
- Container Baseline,
- IAM Policy,
- Secrets Scan,
- TLS Policy.

---

## 97. Security-Ausnahme mit Exit

Beispiel:

~~~text
Exception:
Legacy TLS endpoint bleibt

Owner:
System X

Risk:
...

Exit:
Consumer Y migriert

Deadline:
...
~~~

---

# Teil XX — Evolution und Daten-/API-Verträge

## 98. API Evolution

Vertrag muss unterscheiden:

- additive Änderung,
- Breaking Change,
- Deprecation,
- Sunset.

---

## 99. Consumer Inventory

Ohne Consumerwissen ist sichere Evolution schwierig.

Deshalb:

~~~text
API
→ bekannte Consumer
→ Version
→ Owner
~~~

---

## 100. Event Evolution

Event Contract braucht:

- Owner,
- Versionierungsstrategie,
- Kompatibilität,
- Retention,
- Consumeranalyse.

---

# Teil XXI — Durchgängiger Praxisfall

## 101. Ausgangslage

Eine Behörde betreibt ein 15 Jahre altes Fachverfahren.

Eigenschaften:

- monolithische Anwendung,
- gemeinsame Oracle-Datenbank,
- SOAP-Schnittstellen,
- Batch-Jobs,
- mehrere externe Consumer,
- externer Betriebsdienstleister.

Ziel:

> schrittweise Modernisierung ohne Big-Bang-Risiko.

---

## 102. Falscher Ansatz

~~~text
18 Monate neues System bauen
↓
Big Bang Cutover
↓
Legacy abschalten
~~~

Risiken:

- Anforderungen ändern sich während Entwicklung,
- Datenmigration ungeprüft,
- externe Consumer nicht bereit,
- großes Cutover-Risiko.

---

## 103. Schritt 1 — Qualitätsziele definieren

Beispiel:

- bessere Änderbarkeit,
- API-Evolution,
- schnellere Deployments,
- bessere Observability,
- kontrollierte Security.

---

## 104. Schritt 2 — Fitness Functions definieren

Beispiele:

~~~text
keine neue direkte Legacy-DB-Abhängigkeit

jede neue API besitzt OpenAPI Contract

Recovery-Test für neue Plattform

keine Breaking Change ohne Deprecation
~~~

---

## 105. Schritt 3 — Capability-Grenze auswählen

Zum Beispiel:

~~~text
Dokumentenabruf
~~~

mit:

- klarer fachlicher Grenze,
- überschaubarem Datenbedarf,
- mehreren Consumer.

---

## 106. Schritt 4 — Strangler-Grenze

~~~text
Consumer
→ Facade
   ├── Legacy
   └── New Document Service
~~~

---

## 107. Schritt 5 — Parallelbetrieb

Während Transition:

- Alt und Neu laufen,
- Traffic wird schrittweise verschoben,
- Logs und Metriken vergleichen Verhalten.

---

## 108. Schritt 6 — Datenmigration

Dokumentmetadaten werden schrittweise migriert.

Evidence:

- Datensatzanzahl,
- Checksummen,
- fachliche Stichprobe.

---

## 109. Schritt 7 — Consumer Migration

~~~text
Consumer A → New
Consumer B → New
Consumer C → Legacy
~~~

Consumer Inventory steuert Fortschritt.

---

## 110. Schritt 8 — Legacy Traffic = 0

Erst jetzt ist Abschaltung technisch möglich.

---

## 111. Schritt 9 — Decommission

Entfernt werden:

- SOAP Endpoint,
- Batch,
- DB-Schema,
- Monitoring,
- Betriebsvertraganteil.

---

## 112. Schritt 10 — Lernen

Review:

- welche Annahmen waren falsch?
- welche Fitness Function war nützlich?
- welche Transition war zu komplex?
- was gilt für nächste Capability?

---

# Teil XXII — Praktisches Reviewverfahren

## 113. Schritt 1 — Veränderungstreiber bestimmen

Was löst Veränderung aus?

- Fachlichkeit,
- Risiko,
- Kosten,
- Security,
- Technologie,
- Gesetz.

---

## 114. Schritt 2 — Ist-Abhängigkeiten verstehen

- Systeme,
- Daten,
- Consumer,
- Teams,
- Lieferanten.

---

## 115. Schritt 3 — Zielqualitäten definieren

Nicht nur Zieltechnologie.

---

## 116. Schritt 4 — Reversibilität bewerten

Welche Entscheidungen sind schwer korrigierbar?

---

## 117. Schritt 5 — Transition States definieren

Welche sicheren Zwischenzustände sind nötig?

---

## 118. Schritt 6 — Fitness Functions definieren

Welche Merkmale dürfen nicht degradieren?

---

## 119. Schritt 7 — Migrationsmechanismus wählen

- Strangler Fig,
- Branch by Abstraction,
- Expand/Contract,
- Replatform,
- Replace,
- kein spezielles Pattern.

---

## 120. Schritt 8 — Datenmigration planen

Daten sind oft kritischer als Code.

---

## 121. Schritt 9 — Parallelbetrieb planen

- Routing,
- Monitoring,
- Rollback,
- Support.

---

## 122. Schritt 10 — Decision Trigger festlegen

Wann muss Architektur neu bewertet werden?

---

## 123. Schritt 11 — Debt dokumentieren

Welche temporären Abweichungen werden akzeptiert?

---

## 124. Schritt 12 — Decommission planen

Was wird wirklich entfernt?

---

## 125. Schritt 13 — Evidence je Transition festlegen

Was beweist Fortschritt?

---

## 126. Schritt 14 — Ownership prüfen

Wer entscheidet und betreibt jeden Zwischenzustand?

---

# Teil XXIII — Typische Fehlanwendungen

## 127. Evolutionary Architecture = keine Zielarchitektur

Falsch.

Zielbilder können sehr wertvoll sein.

Evolutionary Architecture ergänzt sie um sichere Übergänge und Feedback.

---

## 128. Emergent Architecture ohne Guardrails

Gefährlich.

Emergenz ohne Führung kann Drift erzeugen.

---

## 129. Fitness Function = Code Metric

Zu eng.

Fitness Functions können Runtime, Security, Recovery, Data oder Governance prüfen.

---

## 130. Jede Metrik = Fitness Function

Falsch.

Eine Metrik wird erst relevant, wenn klar ist:

- welches Architekturmerkmal sie schützt,
- warum der Wert wichtig ist.

---

## 131. Magic Thresholds

Falsch.

Schwellen brauchen Kontext.

---

## 132. Strangler = Microservices

Falsch.

Das Muster beschreibt inkrementelle Ablösung, nicht zwingend Zielarchitektur.

---

## 133. Parallelbetrieb ohne Exit

Anti-Pattern.

Dann wächst Komplexität dauerhaft.

---

## 134. Feature Flags ohne Removal

Anti-Pattern.

Temporäre Kontrollmechanismen werden permanente Architektur.

---

## 135. Technical Debt = alles Unschöne

Zu breit.

Debt ist als Steuerungsmetapher nur hilfreich, wenn Wirkung und Rückzahlung relevant sind.

---

## 136. Transition Architecture = Provisorium ohne Qualität

Falsch.

Übergangszustände sind produktive Architektur.

---

## 137. Legacy bleibt für immer als Backup

Ein dauerhaft parallel betriebenes Legacy-System ist kein kostenloser Fallback.

Es verursacht:

- Kosten,
- Security,
- Betriebsaufwand,
- Datenrisiko.

---

# Teil XXIV — Review-Checklisten

## 138. Evolution

- Welche Dimensionen müssen sich verändern können?
- Welche müssen stabil bleiben?
- Welche Abhängigkeiten begrenzen Veränderung?

## 139. Fitness Functions

- Welches Architekturmerkmal wird geschützt?
- Ist die Prüfung objektiv genug?
- Wer besitzt die Fitness Function?
- Wann läuft sie?
- Was passiert bei Verletzung?

## 140. Transition

- Ist der Zwischenzustand sicher?
- Wer betreibt ihn?
- Wie lange darf er bestehen?
- Was ist das Exit-Kriterium?

## 141. Data

- Wie wird migriert?
- Gibt es Backfill?
- Gibt es Dual Write?
- Wie wird Konsistenz bewiesen?

## 142. Contracts

- Welche Consumer existieren?
- Wie werden Breaking Changes vermieden?
- Wie läuft Deprecation?

## 143. Technical Debt

- Welche Wirkung?
- Welcher Owner?
- Welche Zinsen?
- Welcher Exit?

## 144. Drift

- Ist die Umsetzung falsch?
- Oder der Standard?
- Oder hat sich Kontext geändert?

## 145. Decommissioning

- Traffic = 0?
- Consumer = 0?
- Daten behandelt?
- Infrastruktur entfernt?
- Vertrag beendet?

## 146. Behördenkontext

- Welche Vergabe- oder Vertragsgrenzen?
- Welche gesetzliche Übergangsregel?
- Welche externe Behörde?
- Welche Betriebsverantwortung?

---

# Teil XXV — Woran erkennt man evolvierbare Architektur?

## 147. Eigenschaften

Eine tragfähige evolutionäre Architektur zeigt typischerweise:

- klare Zielqualitäten,
- kontrollierte Kopplung,
- bekannte Consumer,
- sichere Transition States,
- objective Fitness Functions,
- kontinuierliches Feedback,
- bewusste Reversibilität,
- explizite Debt,
- kontrollierte Drift,
- aktives Decommissioning,
- klare Ownership.

Die Balance lautet:

~~~text
Stabilität
+
Veränderbarkeit
+
Feedback
+
Governance
+
Evidence
~~~

---

# Teil XXVI — Glossar

## Architecture Drift

Abweichung realer Architektur von vereinbartem Architekturintent.

## Architecture Fitness Function

Mechanismus zur objektiven Integritätsbewertung eines Architekturmerkmals.

## Branch by Abstraction

Migrationsmuster, bei dem eine Abstraktionsgrenze eingeführt wird, um alte und neue Implementierung schrittweise austauschbar zu machen.

## Decommissioning

Aktive Stilllegung alter technischer und betrieblicher Artefakte.

## Evolutionary Architecture

Architektur, die guided, incremental change across multiple dimensions unterstützt.

## Fitness Function

Überprüfbarer Mechanismus, der feststellt, ob ein relevantes Architekturmerkmal erhalten bleibt.

## Reversibility

Aufwand und Risiko, eine Entscheidung rückgängig zu machen.

## Strangler Fig

Muster zur schrittweisen Ablösung eines bestehenden Systems oder Systemteils.

## Technical Debt

Metapher für interne Qualitätsdefizite oder bewusst akzeptierte Designkompromisse, die zukünftige Änderungskosten erhöhen können.

## Transition Architecture

Bewusst geplanter und betreibbarer Zwischenzustand auf dem Weg von Ist zu Ziel.

---

# Teil XXVII — Quellen und weiterführende Literatur

## 148. Ford, Parsons, Kua, Sadalage — Building Evolutionary Architectures

Neal Ford, Rebecca Parsons, Patrick Kua, Pramod Sadalage, *Building Evolutionary Architectures*, 2nd Edition, O'Reilly, 2022.

Offizielle Buchseite:

https://www.thoughtworks.com/en-de/insights/books/building-evolutionaryarchitectures-second-edition

Free Chapter:

https://www.thoughtworks.com/content/dam/thoughtworks/documents/books/bk_building_evolutionary_architectures_second_edition_free_chapter.pdf

Relevant für:

- Definition Evolutionary Architecture,
- guided incremental change,
- multiple dimensions,
- Architecture Fitness Functions,
- Continuous Architecture.

---

## 149. Neal Ford — Evolutionary Architecture and Fitness Functions

https://nealford.com/books/buildingevolutionaryarchitectures.html

Relevant für:

- Fitness Functions,
- architectural dimensions,
- Evolvability als explizites Architekturmerkmal.

---

## 150. Martin Fowler — Strangler Fig

Martin Fowler, *Strangler Fig*, aktualisierte Fassung 2024.

https://martinfowler.com/bliki/StranglerFigApplication.html

Relevant für:

- inkrementelle Legacy-Modernisierung,
- Risikoreduktion,
- schrittweises Ersetzen.

---

## 151. Martin Fowler — Branch by Abstraction

Martin Fowler, *Branch By Abstraction*, 2014.

https://martinfowler.com/bliki/BranchByAbstraction.html

Relevant für:

- große interne Veränderungen,
- kontinuierliche Lieferfähigkeit,
- schrittweise Umschaltung.

---

## 152. Martin Fowler — Technical Debt

Martin Fowler, *Technical Debt*, 2019.

https://martinfowler.com/bliki/TechnicalDebt.html

Relevant für:

- Ward Cunninghams Debt-Metapher,
- Interest als zusätzliche Änderungskosten,
- Grenzen der Metapher.

---

## 153. Martin Fowler — Technical Debt Quadrant

https://martinfowler.com/bliki/TechnicalDebtQuadrant.html

Relevant für:

- deliberate vs. inadvertent,
- prudent vs. reckless Debt.

Diese Kategorien sind Denkmodelle und keine formale Debt-Klassifikation.

---

## 154. AWS Prescriptive Guidance — Strangler Fig

https://docs.aws.amazon.com/prescriptive-guidance/latest/modernization-decomposing-monoliths/strangler-fig.html

Relevant für:

- Transform,
- Coexist,
- Eliminate,
- praktische Grenzen und Risiken des Strangler-Fig-Ansatzes.

---

## 155. AWS Prescriptive Guidance — Branch by Abstraction

https://docs.aws.amazon.com/prescriptive-guidance/latest/modernization-decomposing-monoliths/branch-by-abstraction.html

Relevant für:

- inkrementelles Ersetzen tief eingebetteter Komponenten,
- Coexistence alter und neuer Implementierungen.

---

## 156. ISO/IEC/IEEE 42010:2022

ISO/IEC/IEEE 42010:2022, *Software, systems and enterprise — Architecture description*.

https://www.iso.org/standard/74393.html

Relevant für:

- Architektur als stakeholder- und concern-orientierter Gegenstand,
- Architekturdescription über Software, Systeme und Enterprises,
- klare Trennung zwischen Architektur und ihrer Beschreibung.

Der Standard definiert keine Evolutionary-Architecture-Methode; er liefert einen normativen Kontext für Architekturbegriffe und Architecture Descriptions.

---

## 157. The Open Group — TOGAF Standard, 10th Edition

https://publications.opengroup.org/standards/togaf

Relevant für:

- Enterprise Architecture,
- Architecture Roadmaps,
- Migration Planning,
- Übergänge zwischen Baseline und Target Architecture.

TOGAF und Evolutionary Architecture sind keine identischen Ansätze. In diesem Dokument werden Transition Architectures als Enterprise-Architecture-Ergänzung zur inkrementellen Veränderungslogik verwendet.

---

## 158. Verhältnis zu anderen Knowledge Items

- AK-025 — SOLID
- AK-026 — Einfachheit, DRY, YAGNI
- AK-036 — CI/CD als Delivery- und Evidence-Pipeline
- AK-039 — Feature Flags
- AK-041 — Event-Driven Architecture
- AK-061 — Architecture Fitness Functions
- AK-075 — Architecture Decision Process
- AK-079 — Modulith Strategy
- AK-084 — Kopplung, Kohäsion und Information Hiding
- AK-085 — Sozio-technische Architektur
- AK-087 — Architekturbewertung
- AK-093 — Living Documentation
- AK-098 — Zero-Downtime Database Migration, sofern als historischer Inhalt referenziert
- Roadmap mit Übergangsarchitekturen

---

# 159. Merksatz

> Evolutionary Architecture bedeutet nicht, Architektur dem Zufall zu überlassen. Sie bedeutet, **Veränderung bewusst zu führen, wichtige Architekturmerkmale kontinuierlich zu überprüfen, große Transformationen in sichere Übergänge zu zerlegen und Altbestand konsequent zu entfernen**, sobald seine Funktion vollständig ersetzt ist.
