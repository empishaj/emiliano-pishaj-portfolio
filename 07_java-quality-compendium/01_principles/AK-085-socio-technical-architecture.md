---
id: AK-085
legacy_ids:
  - ADR-085
title: Sozio-technische Architektur, Ownership, Teamgrenzen und Decision Rights
artifact_type: architecture-principle
domain: organization-and-architecture
status: active
maturity: reviewed
normative_level: recommended
last_validated: 2026-10-05
review_trigger:
  - wesentliche Organisationsänderung
  - dauerhafte teamübergreifende Delivery-Blockaden
  - wiederkehrende Ownership-Konflikte
  - strukturelle Änderung des Lieferanten- oder Betriebsmodells
  - relevante Weiterentwicklung von Team-Topologies-Praktiken oder Governance-Standards
---

# AK-085 — Sozio-technische Architektur, Ownership, Teamgrenzen und Decision Rights

## 1. Zweck dieses Dokuments

Softwarearchitektur ist nicht ausschließlich eine technische Struktur.

Software wird von Menschen entworfen, verändert, betrieben, geprüft, finanziert und verantwortet.

Deshalb wirken auf Architektur nicht nur:

- Domänenmodelle,
- APIs,
- Datenbanken,
- Plattformen,
- Security Controls,
- Deployment-Technologien,

sondern ebenso:

- Kommunikationswege,
- Teamgrenzen,
- Entscheidungsrechte,
- Budgetverantwortung,
- Betriebsmodelle,
- Lieferantenbeziehungen,
- Governance,
- Zuständigkeiten.

Ein technisch sauber geschnittenes System kann in der Praxis trotzdem schwer änderbar sein, wenn jede Änderung fünf Organisationseinheiten, drei Freigaben und zwei Lieferanten benötigt.

Umgekehrt kann eine organisatorisch scheinbar einfache Struktur technische Kopplung erzeugen, wenn Verantwortungen quer zu den fachlichen und technischen Grenzen liegen.

Die zentrale Frage dieses Dokuments lautet:

> **Wie müssen technische Grenzen, Verantwortungen, Kommunikationswege und Entscheidungsrechte zusammenspielen, damit eine Organisation Software nachhaltig verändern und betreiben kann?**

Das Ziel ist nicht:

> Für jedes System genau ein Team.

Und auch nicht:

> Das Organigramm muss die Architektur spiegeln.

Sondern:

> **Die notwendigen technischen und organisatorischen Abhängigkeiten müssen sichtbar, bewusst gestaltet und entscheidbar sein.**

---

# Teil I — Sozio-technische Systeme verstehen

## 2. Was bedeutet sozio-technisch?

Ein sozio-technisches System besteht aus miteinander verbundenen sozialen und technischen Elementen.

Im Softwarekontext gehören dazu beispielsweise:

### Technische Elemente

- Anwendungen,
- Daten,
- APIs,
- Plattformen,
- Infrastruktur,
- Build- und Deployment-Systeme,
- Security Controls,
- Betriebswerkzeuge.

### Soziale und organisatorische Elemente

- Teams,
- Fachverantwortung,
- Betriebsorganisation,
- Architektur,
- Informationssicherheit,
- Datenschutz,
- Management,
- Lieferanten,
- Entscheidungsgremien,
- Kommunikationswege,
- Budget- und Beschaffungsmechanismen.

Architekturprobleme können daher aus beiden Richtungen entstehen.

Beispiel:

~~~text
technische Grenze gut
+
Ownership unklar
=
Änderung blockiert
~~~

oder:

~~~text
ein Team besitzt End-to-End-Verantwortung
+
Systemgrenzen stark gekoppelt
=
Autonomie nur auf dem Papier
~~~

---

## 3. Technische und organisatorische Architektur beeinflussen sich gegenseitig

Ein Team kann nur innerhalb bestimmter Rahmenbedingungen arbeiten.

Wenn es für jede Änderung externe Entscheidungen benötigt, entstehen Abhängigkeiten.

Wenn eine technische Komponente von vielen Teams verändert wird, entsteht Koordinationsbedarf.

Damit existiert eine Rückkopplung:

~~~text
Organisationsstruktur
      ↓
Kommunikationswege
      ↓
Design- und Änderungsentscheidungen
      ↓
Systemstruktur
      ↓
neuer Koordinationsbedarf
      ↓
Organisationsstruktur
~~~

Diese Beziehung ist dynamisch.

Sie ist kein einmaliger Organisationsentwurf.

---

# Teil II — Conway's Law korrekt einordnen

## 4. Ursprung

Melvin E. Conway veröffentlichte 1968 den Beitrag:

*How Do Committees Invent?*

Die zentrale Beobachtung lautet sinngemäß:

> Organisationen, die Systeme entwerfen, sind darin eingeschränkt, Designs hervorzubringen, deren Strukturen ihre Kommunikationsstrukturen abbilden.

Wichtig ist das Wort:

> **Kommunikationsstrukturen**

Nicht:

> Organigramme.

---

## 5. Warum Kommunikation Design beeinflusst

Wenn zwei Gruppen kaum miteinander kommunizieren können, wird es schwieriger, Designentscheidungen zu treffen, die enge technische Zusammenarbeit zwischen ihnen voraussetzen.

Umgekehrt führt intensive Kommunikation häufig dazu, dass:

- gemeinsames Wissen entsteht,
- gemeinsame Entscheidungen getroffen werden,
- gemeinsame Komponenten wachsen,
- Schnittstellen anders geschnitten werden.

Conways ursprüngliche Argumentation betont, dass die Wahl einer Organisationsstruktur bestimmte Designalternativen erleichtert und andere erschwert.

---

## 6. Conway's Law ist kein deterministisches Naturgesetz

Zu stark wäre die Aussage:

~~~text
Org Chart
=
Software Architecture
~~~

Realität ist komplexer.

Architektur wird zusätzlich beeinflusst durch:

- bestehende Systeme,
- technische Standards,
- externe Produkte,
- regulatorische Vorgaben,
- Beschaffung,
- historische Entscheidungen,
- Personalfluktuation,
- Fusionen,
- Reorganisationen,
- Plattformen,
- externe Dienstleister.

Conway's Law ist daher ein starkes Analyseprinzip, aber kein vollständiges Organisationsmodell.

---

## 7. Mirroring Hypothesis

Spätere Forschung untersucht die sogenannte Mirroring Hypothesis:

> Produkt- und Organisationsarchitekturen zeigen häufig strukturelle Ähnlichkeiten.

MacCormack, Rusnak und Baldwin verglichen unter anderem unterschiedlich organisierte Softwareentwicklungsmodelle und fanden Hinweise darauf, dass stärker entkoppelte organisatorische Strukturen mit modulareren Produktarchitekturen verbunden sein können.

Diese Forschung stärkt die Annahme eines Zusammenhangs.

Sie beweist nicht:

> Eine bestimmte Teamstruktur erzeugt garantiert eine bestimmte Softwarearchitektur.

---

## 8. Architekturlektion aus Conway

Für den Architekten lautet die relevante Frage:

> Welche Kommunikationsbeziehungen verlangt meine Zielarchitektur dauerhaft?

Wenn zwei Teams täglich eng koordinieren müssen, obwohl die Architektur vollständige Unabhängigkeit behauptet, stimmt mindestens eine dieser Annahmen nicht.

Mögliche Ursachen:

- Grenze falsch geschnitten,
- Vertrag instabil,
- Ownership unklar,
- gemeinsames Datenmodell,
- fehlende Plattformfähigkeit,
- Prozess organisatorisch falsch geschnitten.

---

# Teil III — Coordination Requirements und socio-technical congruence

## 9. Technische Abhängigkeit erzeugt Koordinationsbedarf

Wenn zwei Entwickler oder Teams technisch voneinander abhängige Artefakte verändern, entsteht häufig Koordinationsbedarf.

Beispiel:

~~~text
Team A ändert API
        ↓
Team B muss Consumer anpassen
~~~

oder:

~~~text
Team A ändert gemeinsame Datenstruktur
        ↓
Team B und C müssen Verhalten prüfen
~~~

Die technische Abhängigkeit wird zur sozialen Koordinationsanforderung.

---

## 10. Coordination Requirements

Cataldo, Wagstrom, Herbsleb und Carley untersuchten, wie technische Arbeitsabhängigkeiten verwendet werden können, um notwendige Koordinationsbeziehungen zwischen Menschen zu bestimmen.

Die Grundidee:

~~~text
Task Dependency
      ↓
Coordination Requirement
~~~

Das ist für Architektur relevant, weil eine technische Grenze nicht nur Codeabhängigkeit erzeugt.

Sie erzeugt möglicherweise:

- Meetings,
- Abstimmungen,
- Freigaben,
- Wartezeit,
- gemeinsame Releases.

---

## 11. Socio-Technical Congruence

Cataldo, Herbsleb und Carley untersuchten später, wie gut tatsächliche Koordinationsmuster zu technischen beziehungsweise Arbeitsabhängigkeiten passen.

Die relevante Erkenntnis:

> Modularisierung allein reicht nicht aus; entscheidend ist auch, ob die Organisation die verbleibenden realen Abhängigkeiten ausreichend koordinieren kann.

Für Architektur bedeutet das:

~~~text
technische Entkopplung
+
fehlende Koordination an verbleibenden Grenzen
=
weiterhin schlechte Delivery
~~~

---

## 12. Mehr Kommunikation ist nicht automatisch besser

Ein häufiges Missverständnis lautet:

> Dann müssen die Teams einfach mehr miteinander reden.

Zu viel Kommunikation kann ebenfalls teuer sein.

Wenn jedes Team ständig mit jedem anderen Team koordinieren muss:

~~~text
A ↔ B
A ↔ C
A ↔ D
B ↔ C
B ↔ D
C ↔ D
~~~

wächst die Koordinationslast.

Architektur soll deshalb nicht maximale Kommunikation erzeugen.

Sie soll:

> **notwendige Kommunikation ermöglichen und unnötige Kommunikation strukturell reduzieren.**

---

# Teil IV — Ownership als Architekturkonzept

## 13. Was bedeutet Ownership?

Ownership bedeutet nicht nur:

> Dieses Team arbeitet meistens an dieser Anwendung.

Belastbare Ownership umfasst mehrere Dimensionen.

---

## 14. Fachliche Ownership

Fragen:

- Wer verantwortet die fachliche Fähigkeit?
- Wer entscheidet über fachliche Regeln?
- Wer priorisiert fachliche Änderungen?
- Wer akzeptiert fachliche Risiken?

---

## 15. Datenownership

Fragen:

- Wer definiert Semantik?
- Wer darf Daten verändern?
- Wer verantwortet Datenqualität?
- Wer entscheidet über Retention?
- Wer genehmigt neue Consumer?

---

## 16. Anwendungsownership

Fragen:

- Wer verantwortet technische Weiterentwicklung?
- Wer genehmigt technische Änderungen?
- Wer kennt technische Schulden?
- Wer priorisiert Modernisierung?

---

## 17. Betriebsownership

Fragen:

- Wer reagiert auf Incidents?
- Wer besitzt Runbooks?
- Wer verantwortet Capacity?
- Wer pflegt Monitoring und Alerts?
- Wer entscheidet im Störungsfall?

---

## 18. Security Ownership

Fragen:

- Wer verantwortet Schutzmaßnahmen?
- Wer akzeptiert Risiken?
- Wer bearbeitet Findings?
- Wer entscheidet über Ausnahmen?

---

## 19. Contract Ownership

Für API oder Event:

- Wer darf den Vertrag verändern?
- Wer kommuniziert Breaking Changes?
- Wer entscheidet über Deprecation?
- Wer kennt die Consumer?

---

## 20. Financial und Procurement Ownership

Gerade im öffentlichen Sektor relevant:

- Wer finanziert?
- Wer beauftragt?
- Wer steuert Vertrag und Lieferant?
- Wer darf Scope ändern?

Diese Verantwortung kann technischen Handlungsspielraum direkt beeinflussen.

---

# Teil V — Decision Rights

## 21. Ownership ohne Entscheidungsrecht ist schwach

Ein Team kann formal "Owner" sein und trotzdem keine Entscheidung selbst treffen dürfen.

Beispiel:

~~~text
Team besitzt Service
aber:
Datenbankänderung → DBA-Gremium
Security → zentrale Stelle
Deployment → Betriebsdienstleister
API → Architekturboard
Budget → Fachreferat
~~~

Dann besitzt das Team nur einen Teil des Entscheidungsraums.

---

## 22. Decision Rights explizit machen

Für relevante Architekturentscheidungen sollte klar sein:

~~~text
Wer schlägt vor?
Wer analysiert?
Wer muss konsultiert werden?
Wer entscheidet?
Wer setzt um?
Wer prüft Evidence?
Wer genehmigt Ausnahme?
~~~

Das ist oft wichtiger als eine pauschale Rollenbezeichnung.

---

## 23. Enterprise Architect besitzt nicht automatisch alle Architekturentscheidungen

Ein Enterprise Architect sollte:

- Auswirkungen sichtbar machen,
- Optionen strukturieren,
- Standards klären,
- Trade-offs analysieren,
- Entscheidungsvorlagen schaffen,
- Governance anwenden.

Er entscheidet nur dort selbst, wo das Governance-Modell ihm dieses Entscheidungsrecht ausdrücklich gibt.

---

## 24. Verantwortungsarchitektur

Eine nützliche Darstellung:

~~~text
Capability
→ fachlicher Owner
→ Prozessowner
→ Datenowner
→ Anwendungsowner
→ Schnittstellenowner
→ Betriebsowner
→ Security-Verantwortung
→ Lieferant
→ Entscheidungsgremium
~~~

Damit wird Architektur mit Verantwortung verbunden.

---

# Teil VI — Team Topologies als Denkmodell

## 25. Einordnung

Team Topologies von Matthew Skelton und Manuel Pais ist ein Organisations- und Interaktionsmodell für einen schnelleren Flow von Veränderung.

Es ist kein formaler wissenschaftlicher Standard.

Es ist auch kein vorgeschriebenes Organigramm.

Sein Nutzen liegt vor allem in einer gemeinsamen Sprache für:

- Teamzweck,
- Interaktion,
- Plattformen,
- Cognitive Load,
- Evolution von Teamgrenzen.

---

## 26. Vier grundlegende Teamtypen

Das Modell beschreibt vier grundlegende Teamtypen.

---

## 27. Stream-aligned Team

Ein Stream-aligned Team ist an einem Flow von Arbeit beziehungsweise Wert ausgerichtet.

Typischer Anspruch:

- End-to-End-Verantwortung,
- möglichst wenige Übergaben,
- Nähe zu Nutzern oder Fachdomäne,
- eigener Delivery-Flow.

Das bedeutet nicht:

> genau ein Team pro Microservice.

---

## 28. Platform Team beziehungsweise Platform Grouping

Eine Plattform stellt Fähigkeiten bereit, die andere Teams schneller und mit weniger kognitiver Belastung nutzen können.

Beispiele:

- Deployment Self-Service,
- Observability-Bausteine,
- standardisierte Runtime,
- Identity Integration,
- Golden Paths.

In der aktuellen Team-Topologies-Darstellung wird zusätzlich betont, dass eine Plattform in größeren Organisationen häufig eine **Gruppierung mehrerer Teams** ist und nicht zwingend ein einzelnes Plattformteam.

---

## 29. Enabling Team

Ein Enabling Team hilft anderen Teams zeitlich begrenzt, Fähigkeiten aufzubauen.

Beispiele:

- Testautomatisierung,
- Security Engineering,
- Observability,
- Domain Modeling,
- Cloud-Nutzung.

Das Ziel ist nicht dauerhafte Abhängigkeit.

Das Ziel ist:

> Fähigkeit beim konsumierenden Team erhöhen.

---

## 30. Complicated Subsystem Team

Ein Complicated Subsystem Team kapselt Spezialkomplexität, die nicht sinnvoll von jedem Stream-Team beherrscht werden kann.

Beispiele können sein:

- mathematisch komplexe Optimierung,
- spezielle Algorithmik,
- hochspezialisierte technische Domäne.

Nicht jeder Shared Service ist deshalb ein Complicated Subsystem.

---

# Teil VII — Team Interaction Modes

## 31. Warum Interaktion explizit sein sollte

"Die Teams arbeiten zusammen" ist zu unpräzise.

Team Topologies beschreibt drei grundlegende Interaktionsmodi:

- Collaboration,
- X-as-a-Service,
- Facilitation.

---

## 32. Collaboration

Zwei Teams arbeiten zeitlich begrenzt eng zusammen.

Sinnvoll bei:

- Discovery,
- neuer Domänengrenze,
- neuer Technologie,
- neuem Vertrag,
- hoher Unsicherheit.

Collaboration ist hochbandbreitig und teuer.

Sie sollte nicht automatisch zum Dauerzustand werden.

---

## 33. X-as-a-Service

Ein Team bietet eine klar definierte Fähigkeit an, die andere Teams mit möglichst wenig direkter Koordination konsumieren.

Beispiele:

- Deployment Platform,
- Identity Service,
- Document Service,
- standardisierte API.

Voraussetzungen:

- klarer Vertrag,
- gute Consumer Experience,
- dokumentierte Verantwortungen,
- stabiler Supportweg.

---

## 34. Facilitation

Ein Team hilft einem anderen Team, eine Fähigkeit zu erlernen oder ein Hindernis zu überwinden.

Typischerweise:

~~~text
Enabling Team
→ Facilitation
→ Stream Team
~~~

Das Ziel ist Kompetenztransfer.

---

## 35. Interaktionsmodi können sich verändern

Eine gesunde Entwicklung kann sein:

~~~text
Collaboration
      ↓
Grenze verstanden
      ↓
X-as-a-Service
~~~

oder:

~~~text
Facilitation
      ↓
Fähigkeit aufgebaut
      ↓
keine dauerhafte Abhängigkeit
~~~

Das Organisationsmodell ist dynamisch.

---

# Teil VIII — Cognitive Load korrekt einordnen

## 36. Herkunft des Begriffs

Cognitive Load Theory stammt aus der Forschung zu individuellem Lernen und Problemlösen, insbesondere aus Arbeiten von John Sweller.

Sie untersucht Begrenzungen menschlicher kognitiver Verarbeitung beim Lernen komplexer Inhalte.

---

## 37. Keine direkte Gleichsetzung mit Teamkapazität

Wissenschaftlich unsauber wäre:

> Eine Theorie über individuelles Arbeitsgedächtnis beweist, wie viele Systeme ein Team besitzen darf.

Diese direkte Übertragung ist nicht gerechtfertigt.

Team Topologies verwendet Cognitive Load als praktische Organisationsmetapher und Analyseperspektive.

---

## 38. Praktische Teamfrage

Trotzdem ist die Beobachtung plausibel und praktisch relevant:

> Ein Team kann nicht unbegrenzt viele Domänen, Technologien, Plattformen, Prozesse und Abhängigkeiten gleichzeitig tief beherrschen.

Deshalb sollten Architekten nach Belastungsquellen fragen.

---

## 39. Arten praktischer Team-Komplexität

### Fachliche Komplexität

- viele Fachdomänen,
- viele unterschiedliche Regelwerke,
- hohe Prozessvarianz.

### Technische Komplexität

- Kubernetes,
- Datenbank,
- Messaging,
- IAM,
- Observability,
- CI/CD,
- Netzwerk,
- Security.

### Betriebs-Komplexität

- viele Services,
- unterschiedliche SLOs,
- Incidents,
- Bereitschaft,
- mehrere Betriebsmodelle.

### Koordinations-Komplexität

- viele Teams,
- viele Gremien,
- viele Lieferanten,
- viele Freigaben.

---

## 40. Kein universeller Team-Cognitive-Load-Score

Ein Wert wie:

~~~text
Cognitive Load = 73
~~~

ist ohne definiertes Messmodell wenig belastbar.

Besser sind konkrete Fragen:

- Welche Domänen muss das Team verstehen?
- Welche Plattformdetails muss es selbst beherrschen?
- Wie viele Systeme betreibt es?
- Welche externen Abhängigkeiten blockieren Delivery?
- Wie viel Zeit fließt in Koordination?
- Welche Tätigkeiten könnten Self-Service werden?

---

# Teil IX — Team API und organisatorische Verträge

## 41. Idee der Team API

Teams besitzen ebenfalls Schnittstellen.

Eine Team API kann beschreiben:

- Verantwortungsbereich,
- angebotene Fähigkeiten,
- Eingangswege,
- Self-Service-Angebote,
- Support,
- erwartete Reaktionszeiten,
- Kommunikationskanäle,
- Änderungspolitik.

---

## 42. Beispiel

~~~text
Identity Platform

Provides:
- OIDC Client Registration
- Token Validation Library
- Service Account Lifecycle

Self-Service:
- Portal
- API

Support:
- incident channel
- office hours

Changes:
- documented deprecation policy
~~~

Das reduziert die Notwendigkeit individueller Abstimmung.

---

## 43. Team API ersetzt keine Beziehung

Eine Team API ist kein Argument gegen direkte Kommunikation.

Bei:

- neuen Anforderungen,
- Unsicherheit,
- Incidents,
- Konflikten

ist direkte Zusammenarbeit notwendig.

Die Team API soll wiederkehrende Interaktion klarer machen.

---

# Teil X — Architekturgrenze und Teamgrenze

## 44. Keine 1:1-Regel

Folgende Gleichungen sind falsch:

~~~text
1 Bounded Context = 1 Team
1 Microservice = 1 Team
1 Team = 1 Service
~~~

Mögliche Strukturen:

~~~text
1 Team
→ mehrere kohärente Services

1 Team
→ Modulith

mehrere Teams
→ Plattformgruppierung

Spezialteam
→ kompliziertes Subsystem
~~~

---

## 45. Was ein Team sinnvoll besitzen können sollte

Eine gute Ownership-Grenze erlaubt idealerweise:

- Änderung,
- Test,
- Deployment,
- Betrieb,
- Incident-Analyse

mit begrenzter Fremdkoordination.

Das ist ein Zielbild.

Im Behördenkontext kann es aufgrund formaler Zuständigkeiten nicht vollständig erreichbar sein.

---

## 46. Ownership von Modulen statt nur Services

Ein Team kann auch in einem Modulith klaren Ownership-Bereich besitzen.

~~~text
Application
├── Case Module        → Team A
├── Document Module    → Team B
└── Reporting Module   → Team C
~~~

Technische Deployment-Einheit und Ownership-Einheit müssen nicht identisch sein.

---

## 47. Shared Component braucht echte Governance

Wenn mehrere Teams eine Komponente besitzen:

- Wer entscheidet?
- Wer reviewed?
- Wer released?
- Wer trägt Incident-Verantwortung?
- Wer priorisiert technische Schulden?

Ohne Antwort entsteht "everyone owns it" häufig als:

> Niemand besitzt es wirklich.

---

# Teil XI — Inverse Conway Maneuver

## 48. Idee

Das sogenannte Inverse Conway Maneuver versucht, Kommunikations- und Teamstrukturen bewusst so zu gestalten, dass eine gewünschte Architektur leichter entstehen kann.

---

## 49. Nicht zuerst reorganisieren

Gefährlich:

~~~text
Ziel:
Microservices

Aktion:
20 Teams nach 20 geplanten Services schneiden
~~~

bevor fachliche Grenzen verstanden sind.

Eine Organisation kann dadurch künstliche Schnittstellen erzeugen.

---

## 50. Bessere Reihenfolge

~~~text
Problem verstehen
      ↓
fachliche Grenzen untersuchen
      ↓
technische Abhängigkeiten analysieren
      ↓
Ownership verstehen
      ↓
gewünschten Flow definieren
      ↓
Team-/Interaktionsstruktur anpassen
~~~

---

## 51. Reorganisation ist teuer

Organisationsänderungen erzeugen:

- Lernkosten,
- Beziehungsverlust,
- neue Zuständigkeiten,
- neue Schnittstellen,
- Unsicherheit,
- Übergangsphasen.

Deshalb sollte eine Teamänderung mit derselben Ernsthaftigkeit behandelt werden wie eine große Architekturänderung.

---

## 52. Inverse Conway als Option

Vor einer Reorganisation prüfen:

1. Kann ein besserer Vertrag helfen?
2. Kann Ownership expliziter werden?
3. Kann Self-Service Wartezeit reduzieren?
4. Kann eine Plattform repetitive Arbeit übernehmen?
5. Ist die fachliche Grenze ausreichend stabil?
6. Ist das Problem dauerhaft oder temporär?

---

# Teil XII — Plattformen als sozio-technische Architektur

## 53. Plattformziel

Eine interne Plattform soll anderen Teams wiederkehrende Komplexität abnehmen.

Beispiele:

~~~text
Deployment
Observability
Runtime
Secrets
Identity
Templates
Golden Paths
~~~

---

## 54. Plattform als Produkt

Eine Plattform sollte Consumer besitzen.

Deshalb braucht sie:

- Nutzerverständnis,
- Produktverantwortung,
- Roadmap,
- Support,
- Feedback,
- messbaren Nutzen.

---

## 55. Thinnest Viable Platform

Team Topologies verwendet die Idee der Thinnest Viable Platform:

> Nur so viel Plattform bereitstellen, wie den Flow der konsumierenden Teams tatsächlich verbessert.

Das schützt vor Plattformen, die größer werden als das Problem.

---

## 56. Plattform als Ticketqueue ist ein Anti-Pattern

Schlecht:

~~~text
Team
→ Ticket
→ Platform Team
→ manuelle Änderung
→ Wartezeit
~~~

Besser, wenn sinnvoll:

~~~text
Team
→ Self-Service API / Portal
→ automatisierter Guardrail
~~~

---

## 57. Plattform erzeugt ebenfalls Kopplung

30 Teams auf einer Plattform bedeuten:

> Die Plattform ist Enterprise-kritisch.

Damit werden relevant:

- Verfügbarkeit,
- Versionierung,
- Security,
- Migration,
- Support,
- Exit-Szenarien.

Plattformkopplung ist nicht schlecht.

Sie muss bewusst regiert werden.

---

# Teil XIII — Behördenkontext

## 58. Warum Behörden besonders sozio-technisch betrachtet werden müssen

Behörden besitzen häufig zusätzliche strukturelle Grenzen:

- gesetzliche Zuständigkeiten,
- Referatsgrenzen,
- Haushaltsverantwortung,
- Vergabe,
- Datenschutz,
- Informationssicherheit,
- Mitbestimmung,
- externe IT-Dienstleister,
- ressortübergreifende Zusammenarbeit,
- Bund-/Länder-/Kommunalbeziehungen.

Diese Grenzen können technisch nicht einfach ignoriert werden.

---

## 59. Fachreferat und IT sind unterschiedliche Verantwortungsräume

Ein mögliches Modell:

~~~text
Fachseite
→ fachliches Ziel
→ fachliche Regeln
→ Prioritäten

IT / Architektur
→ technische Konsequenzen
→ Zielarchitektur
→ Standards
→ Risiken
→ Übergangsarchitektur
~~~

Keine Seite ersetzt die andere.

---

## 60. Enterprise Architect im Behördenkontext

Der EA besitzt typischerweise nicht automatisch:

- Budget,
- fachliche Priorität,
- Personal,
- Betriebsverantwortung.

Sein Hebel liegt häufig in:

- Transparenz,
- Standards,
- Entscheidungsreife,
- Risikoanalyse,
- Zielbildern,
- Governance,
- Eskalation.

Das ist Influence without Authority.

---

## 61. Gremien sind nicht automatisch schlecht

Ein Architekturboard kann sinnvoll sein, wenn es Entscheidungen bündelt, die tatsächlich mehrere Bereiche betreffen.

Problematisch wird es, wenn jede lokale Entscheidung zentral freigegeben werden muss.

Die Governance-Frage lautet:

> Welche Entscheidungsebene passt zur Tragweite und Reversibilität der Entscheidung?

---

## 62. Zentrale Stelle als Engpass

Warnsignal:

~~~text
jedes Team
→ eine zentrale Person
→ Architekturentscheidung
~~~

Dann entsteht Bus-Factor- und Flow-Risiko.

Bessere Mechanismen können sein:

- Standards,
- Referenzarchitekturen,
- Delegation,
- Decision Guides,
- Self-Service,
- klare Eskalationsregeln.

---

## 63. Formale Zuständigkeit versus tatsächliche Entscheidung

In Behörden kann formal eine Stelle verantwortlich sein, während technisch ein Dienstleister oder Spezialist die Entscheidung faktisch prägt.

Diese Differenz muss sichtbar werden.

~~~text
formal accountable
≠
tatsächlich entscheidungsfähig
~~~

---

# Teil XIV — Externe Dienstleister

## 64. Outsourcing entfernt Ownership nicht

Ein Lieferant kann:

- entwickeln,
- betreiben,
- analysieren,
- Optionen entwerfen.

Die Auftraggeberorganisation bleibt dennoch verantwortlich für:

- Zielbild,
- fachliche Verantwortung,
- akzeptierte Risiken,
- strategische Abhängigkeiten,
- Architekturentscheidungsfähigkeit.

---

## 65. Gefährliches Muster

~~~text
Behörde
→ formuliert Bedarf

Dienstleister
→ kennt Architektur
→ entscheidet Technologie
→ dokumentiert teilweise
→ betreibt

Behörde
→ nimmt Ergebnis ab
~~~

Langfristig kann dadurch Architekturwissen außerhalb der Behörde akkumulieren.

---

## 66. Besseres Muster

~~~text
Behörde:
Zielbild
Standards
Decision Rights
Qualitätskriterien
Risikoakzeptanz

        ↓

Dienstleister:
Analyse
Optionen
Implementierung
Evidence

        ↓

gemeinsames Review

        ↓

nachvollziehbare Entscheidung
~~~

---

## 67. Lieferantenownership präzisieren

Nicht:

> Lieferant besitzt Anwendung.

Sondern getrennt:

- Wer entwickelt?
- Wer wartet?
- Wer betreibt?
- Wer entscheidet?
- Wer besitzt Source Code?
- Wer besitzt Daten?
- Wer besitzt Architekturwissen?
- Wer kann Lieferantenwechsel durchführen?

---

## 68. Exit-Fähigkeit

Sozio-technische Architektur muss auch fragen:

> Kann die Organisation einen Lieferanten wechseln, ohne Entscheidungsfähigkeit zu verlieren?

Evidence kann sein:

- dokumentierte Schnittstellen,
- Architekturentscheidungen,
- Runbooks,
- IaC,
- Datenexport,
- Wissenstransfer,
- internes Review-Know-how.

---

# Teil XV — Governance und Decision Rights

## 69. Governance soll Entscheidungen ermöglichen

Schlechte Governance:

~~~text
jede Entscheidung
→ Board
→ Wartezeit
~~~

Andere schlechte Governance:

~~~text
jeder entscheidet alles lokal
→ inkonsistente Landschaft
~~~

Ziel:

> Entscheidung dort treffen, wo ausreichend Kontext und legitimes Entscheidungsrecht zusammenkommen.

---

## 70. Entscheidungslevel

Beispiel:

### Lokal

- interne Klassenstruktur,
- kleine reversible Implementierungswahl.

### Produkt-/Anwendungsebene

- Datenmodell,
- API,
- Frameworkentscheidungen.

### Enterprise-Ebene

- strategische Plattform,
- behördenweite Standards,
- gemeinsame Datenverantwortung,
- langfristige Vendor-Abhängigkeit.

---

## 71. Architekturboard

Ein Board sollte nicht selbst jede Architektur entwerfen.

Seine Aufgabe kann sein:

- hochwirksame Entscheidungen prüfen,
- Konflikte zwischen Verantwortungsbereichen lösen,
- Ausnahmen genehmigen,
- Risiken akzeptieren oder eskalieren.

---

## 72. Standard plus Exception Process

Ein Standard ohne Ausnahmeprozess erzeugt häufig:

- Schattenlösungen,
- inoffizielle Abweichungen,
- politische Konflikte.

Ein professionelles Modell:

~~~text
Standard
      ↓
lokale Anwendung
      ↓
falls nicht passend:
begründete Ausnahme
      ↓
Review
      ↓
befristete oder dauerhafte Entscheidung
~~~

---

# Teil XVI — Data Ownership als sozio-technische Grenze

## 73. Datenownership ist keine Datenbankfrage

Ein Datenowner verantwortet nicht nur Tabellen.

Relevante Fragen:

- Was bedeutet die Information?
- Wer darf sie ändern?
- Welche Qualität wird erwartet?
- Welche rechtliche Grundlage gilt?
- Welche Consumer dürfen sie nutzen?

---

## 74. Technischer System Owner ist nicht automatisch Data Owner

Beispiel:

~~~text
Anwendung X speichert Personendaten
~~~

Daraus folgt nicht automatisch:

> Team X entscheidet fachlich über die Bedeutung dieser Personendaten.

---

## 75. Shared Data erzeugt organisatorische Abhängigkeit

Wenn fünf Fachverfahren dieselbe Tabelle verändern:

~~~text
5 Systeme
→ 1 Schema
~~~

entsteht nicht nur technische Kopplung.

Es entsteht:

- gemeinsame Entscheidungsnotwendigkeit,
- gemeinsames Migrationsrisiko,
- gemeinsame Verantwortung.

---

# Teil XVII — Incident Ownership

## 76. Architektur zeigt sich im Incident

Im Incident wird sichtbar, ob Ownership real ist.

Warnsignale:

- niemand weiß, wer führt,
- fünf Teams analysieren isoliert,
- Provider und Consumer schieben Verantwortung,
- Logs fehlen,
- Dienstleister wartet auf Freigabe,
- Fachseite kennt Auswirkung nicht.

---

## 77. End-to-End-Verantwortung

Ein sinnvoller Incident-Prozess braucht:

- technische Diagnose,
- Business Impact,
- Kommunikationsverantwortung,
- Entscheidungsrecht,
- Wiederherstellungsentscheidung.

Nicht alles muss bei einem Team liegen.

Aber die Rollen müssen vorher klar sein.

---

# Teil XVIII — Symptome schlechter sozio-technischer Architektur

## 78. Meeting Load

Kleine Änderungen benötigen viele Abstimmungen.

Mögliche Ursache:

- zu viele Ownership-Grenzen,
- instabile Verträge,
- fehlende Entscheidungsrechte.

---

## 79. Ticket Ping-Pong

~~~text
Team A → Team B
Team B → Plattform
Plattform → Betrieb
Betrieb → Security
Security → Team A
~~~

Das ist häufig ein Flow-Problem, nicht nur ein Prozessproblem.

---

## 80. Zentraler Spezialist

Eine Person ist nötig für:

- jede Architekturentscheidung,
- jede Datenbankmigration,
- jeden Incident.

Das ist ein strukturelles Risiko.

---

## 81. Coordinated Release Train

Viele technisch getrennte Services können nur gemeinsam released werden.

Dann ist ihre tatsächliche Autonomie geringer als das Architekturdiagramm behauptet.

---

## 82. Ownership Switching

Bei Feature:

> Team A.

Bei Incident:

> Betrieb.

Bei Security:

> Plattform.

Bei Datenfehler:

> niemand.

Solche wechselnden Zuständigkeiten erzeugen Reibung.

---

## 83. Dienstleister besitzt Kontext

Wenn der Auftraggeber für jede Architekturfrage externe Unterstützung braucht, ist interne Entscheidungsfähigkeit zu schwach.

---

# Teil XIX — Evidence und Messbarkeit

## 84. Organisationsqualität ist nicht eine einzelne Kennzahl

Metriken wie:

~~~text
Meetings pro Woche
Tickets pro Team
Teamgröße
~~~

sind keine direkten Qualitätsbeweise.

Sie können Hinweise liefern.

---

## 85. Flow Metrics

Mögliche Signale:

- Lead Time,
- Wartezeit,
- Blocked Time,
- Handoffs,
- Deployment Frequency,
- Change Failure Rate.

Sie müssen kontextbezogen interpretiert werden.

---

## 86. Coordination Load

Erfassbar sind beispielsweise:

- Anzahl beteiligter Teams pro Change,
- Anzahl notwendiger Freigaben,
- Wartezeit auf andere Teams,
- Anzahl koordinierter Releases.

Keine universellen Grenzwerte.

---

## 87. Ownership Coverage

Frage:

> Gibt es für jede kritische Capability einen klaren Verantwortungsweg?

Nicht nur:

~~~text
Owner = Team X
~~~

sondern:

- fachlich,
- Daten,
- Anwendung,
- Betrieb,
- Security,
- Contract.

---

## 88. Architecture-to-Organization Mapping

Ein hilfreiches Artefakt:

| Architekturgrenze | Owner | Consumer | Interaktionsmodus | Decision Right |
|---|---|---|---|---|
| Case API | Team A | Portal | X-as-a-Service | Team A |
| Identity Platform | Platform Group | alle Teams | X-as-a-Service | Platform Governance |
| neue Security Fähigkeit | Enabling Team + Team A | Team A | Facilitation | Team A / Security Policy |

Das macht sozio-technische Architektur sichtbar.

---

## 89. Change Dependency Map

Für reale Änderung:

~~~text
Change
→ Systeme
→ Teams
→ Gremien
→ Lieferanten
→ Freigaben
~~~

Wenn dieser Graph unerwartet groß wird, ist das Architecture Evidence.

---

## 90. Incident Dependency Map

Für Incident:

~~~text
Symptom
→ Service
→ Plattform
→ IAM
→ Betrieb
→ externer Provider
~~~

Damit werden reale Betriebsabhängigkeiten sichtbar.

---

# Teil XX — Durchgängiger Praxisfall

## 91. Ausgangslage

Eine Behörde digitalisiert eine Verwaltungsleistung.

Beteiligt:

- Fachreferat,
- Entwicklungsteam,
- zentrale Plattform,
- Betrieb,
- Informationssicherheit,
- externer Lieferant.

Technisch:

~~~text
Portal
→ Case Service
→ Register Adapter
→ Document Service
→ IAM
~~~

---

## 92. Problem

Eine kleine Änderung an der Fallentscheidung benötigt:

1. Fachfreigabe,
2. Änderung im Case Service,
3. Registermapping durch Lieferant,
4. Plattformticket,
5. Security Review,
6. Betriebsdeployment.

Lead Time:

> Wochen.

Coding Time:

> Stunden.

Das Problem ist offensichtlich nicht primär Programmierung.

---

## 93. Analyse fachlicher Ownership

Fragen:

- Wer entscheidet fachliche Regel?
- Wer akzeptiert fachliches Risiko?
- Welche Stelle darf Regel ändern?

Ergebnis:

~~~text
Fachreferat = fachlicher Owner
~~~

---

## 94. Analyse technischer Ownership

Case Service wird vom internen Team verantwortet.

Registeradapter wird faktisch vollständig vom Lieferanten kontrolliert.

Das ist ein sozio-technisches Risiko.

---

## 95. Analyse der Plattforminteraktion

Jedes Deployment erfordert ein manuelles Plattformticket.

Das Plattformteam ist damit keine Self-Service-Plattform, sondern ein operativer Übergabepunkt.

---

## 96. Analyse Security

Jede kleine Änderung geht erneut durch vollständiges Security Review.

Frage:

> Kann ein genehmigter Guardrail wiederkehrende Standardänderungen abdecken?

---

## 97. Zielbild

~~~text
Fachreferat
→ entscheidet Fachregel

Case Team
→ besitzt Anwendung und Delivery

Register Contract
→ behördenintern dokumentiert
→ Lieferant implementiert Adapter

Platform Grouping
→ bietet Deployment Self-Service

Security
→ definiert Baseline und Review-Trigger
~~~

---

## 98. Neue Interaktionsmodi

~~~text
Case Team
→ Platform: X-as-a-Service

Security Enabling
→ Case Team: Facilitation bei neuer Fähigkeit

Case Team
↔ Lieferant: Collaboration bei Registeränderung

danach
Register Contract
→ stabiler Service-Vertrag
~~~

---

## 99. Ergebnis

Nicht jedes Gremium wurde entfernt.

Nicht jede Abhängigkeit wurde beseitigt.

Aber:

- Decision Rights sind klarer,
- manuelle Übergaben werden reduziert,
- Lieferantenwissen wird intern nachvollziehbar,
- Plattforminteraktion wird produktisiert,
- Security wird risikobasiert eingebunden.

Das ist sozio-technische Architekturarbeit.

---

# Teil XXI — Praktisches Reviewverfahren

## 100. Schritt 1 — Capability bestimmen

Welche fachliche Fähigkeit betrachten wir?

---

## 101. Schritt 2 — technischen Flow abbilden

~~~text
User
→ Prozess
→ Anwendung
→ Daten
→ Integration
→ Plattform
→ Betrieb
~~~

---

## 102. Schritt 3 — Ownership abbilden

Für jedes Element:

- fachlich,
- Daten,
- Anwendung,
- Betrieb,
- Security,
- Lieferant.

---

## 103. Schritt 4 — Decision Rights bestimmen

Wer entscheidet welche Änderung?

---

## 104. Schritt 5 — Kommunikationswege beobachten

Wer muss in der Realität mit wem sprechen?

Nicht nur Organigramm analysieren.

---

## 105. Schritt 6 — Handoffs identifizieren

Wo wartet Arbeit?

---

## 106. Schritt 7 — technische Abhängigkeiten prüfen

Welche Handoffs sind durch Architektur erzwungen?

---

## 107. Schritt 8 — unnötige Organisationkopplung prüfen

Welche Abstimmungen existieren nur wegen historischer Verantwortungsverteilung?

---

## 108. Schritt 9 — Interaktionsmodus benennen

- Collaboration?
- X-as-a-Service?
- Facilitation?

Oder ist der Modus unklar?

---

## 109. Schritt 10 — Cognitive Load qualitativ prüfen

Welche Fachlichkeit und Technik muss das Team gleichzeitig beherrschen?

---

## 110. Schritt 11 — Plattformchancen prüfen

Welche wiederkehrende Last kann als Self-Service bereitgestellt werden?

---

## 111. Schritt 12 — Lieferantenabhängigkeit prüfen

Welche Entscheidungsfähigkeit liegt außerhalb der eigenen Organisation?

---

## 112. Schritt 13 — Reorganisation nur bei strukturellem Bedarf prüfen

Kann das Problem ohne Teamumbau gelöst werden?

---

## 113. Schritt 14 — Evidence definieren

Wie erkennen wir Verbesserung?

- weniger Handoffs,
- weniger Blocked Time,
- klarere Ownership,
- unabhängigere Changes,
- bessere Incident-Zuordnung.

---

# Teil XXII — Typische Fehlanwendungen

## 114. Conway = Organigramm

Falsch.

Relevant sind reale Kommunikations- und Koordinationsstrukturen.

---

## 115. Team Topologies = neues Organigramm

Zu oberflächlich.

Es ist vor allem ein Denkmodell für Flow, Teamzweck und Interaktion.

---

## 116. Ein Bounded Context = ein Team

Falsch.

Das kann passen.

Es ist keine allgemeine Regel.

---

## 117. Ein Microservice = ein Team

Falsch.

Servicegranularität und Teamgranularität sind unterschiedliche Entscheidungen.

---

## 118. Platform Team = Ticketqueue

Anti-Pattern.

Eine Plattform sollte wiederkehrende Komplexität produktisieren und möglichst Self-Service ermöglichen.

---

## 119. Enabling Team = dauerhafte Beratung

Anti-Pattern.

Facilitation soll Fähigkeiten aufbauen, nicht Abhängigkeit konservieren.

---

## 120. Cognitive Load = feste Teamzahl

Falsch.

Es gibt keinen seriösen universellen Wert wie:

~~~text
maximal 7 Systeme pro Team
~~~

---

## 121. Mehr Kommunikation löst alles

Falsch.

Unnötige Kommunikation kann selbst Symptom schlechter Grenzen sein.

---

## 122. Autonomes Team ohne Decision Rights

Widersprüchlich.

Autonomie setzt echten Entscheidungsspielraum voraus.

---

## 123. Outsourcing = Ownership übertragen

Falsch.

Lieferfähigkeit kann ausgelagert werden.

Strategische Entscheidungsverantwortung verschwindet nicht.

---

## 124. Architecture Board entscheidet alles

Anti-Pattern.

Zentrale Governance sollte nur Entscheidungen zentralisieren, deren Wirkung dies rechtfertigt.

---

## 125. Reorganisation zuerst

Gefährlich.

Ohne verstandene Fach- und Architekturgrenzen entstehen neue künstliche Silos.

---

# Teil XXIII — Review-Checklisten

## 126. Capability

- Welche Capability wird betrachtet?
- Wer ist fachlich accountable?
- Welche Outcomes werden erwartet?

## 127. Ownership

- Wer besitzt Daten?
- Wer besitzt Anwendung?
- Wer betreibt?
- Wer trägt Security-Verantwortung?
- Wer besitzt Contracts?

## 128. Decision Rights

- Wer darf lokal entscheiden?
- Was braucht Review?
- Wer genehmigt Ausnahmen?
- Wer akzeptiert Risiko?

## 129. Teamgrenzen

- Welche Komponenten besitzt das Team?
- Kann es End-to-End liefern?
- Welche Fremdkoordination ist dauerhaft erforderlich?

## 130. Interaktion

- Collaboration?
- X-as-a-Service?
- Facilitation?
- Ist die Interaktion bewusst oder historisch entstanden?

## 131. Cognitive Load

- Wie viele Domänen?
- Wie viele Technologien?
- Wie viel Betriebsverantwortung?
- Wie viele externe Schnittstellen?
- Welche Last könnte Plattform übernehmen?

## 132. Plattform

- Gibt es echte Consumer?
- Existiert Self-Service?
- Wird Plattform als Produkt geführt?
- Reduziert sie messbar Wartezeit oder Komplexität?

## 133. Lieferant

- Wo liegen Architekturentscheidungen?
- Ist Wissen intern verfügbar?
- Gibt es Exit-Fähigkeit?
- Ist Contract Ownership intern klar?

## 134. Behörden-Governance

- Sind formale Zuständigkeiten klar?
- Entsprechen sie tatsächlicher Entscheidungsfähigkeit?
- Welche Gremien sind notwendig?
- Welche können durch Standards ersetzt werden?

## 135. Betrieb

- Wer führt Incident?
- Wer kann End-to-End diagnostizieren?
- Sind Übergaben vorher definiert?

---

# Teil XXIV — Woran erkennt man tragfähige sozio-technische Architektur?

## 136. Eigenschaften

Eine tragfähige sozio-technische Architektur zeigt typischerweise:

- klare fachliche Verantwortung,
- klare Datenownership,
- klare Anwendungs- und Betriebsownership,
- explizite Decision Rights,
- technische Grenzen mit verständlichen Ownern,
- bewusst definierte Teaminteraktionen,
- begrenzte dauerhafte Koordinationslast,
- Plattformen als Self-Service-Produkte,
- risikobasierte Governance,
- interne Entscheidungsfähigkeit trotz externer Lieferanten,
- messbare Verbesserung des Delivery-Flows.

Die Balance lautet:

~~~text
notwendige Kommunikation
+
klare Verantwortung
+
passende technische Grenzen
+
delegierte Decision Rights
+
so wenig dauerhafte Koordinationslast wie sinnvoll
~~~

---

# Teil XXV — Glossar

## Accountability

Verantwortung dafür, dass ein Ergebnis erreicht beziehungsweise eine Entscheidung getragen wird.

## Collaboration

Zeitlich begrenzte intensive Zusammenarbeit zwischen Teams.

## Conway's Law

Beobachtung, dass Systemdesign durch Kommunikationsstrukturen der entwerfenden Organisation geprägt wird.

## Coordination Requirement

Aus Arbeits- oder technischen Abhängigkeiten entstehender Bedarf zur Koordination zwischen Beteiligten.

## Cognitive Load

Ursprünglich psychologisches Konzept zur begrenzten kognitiven Verarbeitung; in Team Topologies als praktische Perspektive auf die Menge von Komplexität verwendet, die ein Team beherrschen muss.

## Decision Right

Explizites Recht, eine bestimmte Klasse von Entscheidungen zu treffen.

## Enabling Team

Team, das andere Teams zeitlich begrenzt beim Aufbau einer Fähigkeit unterstützt.

## Facilitation

Interaktionsmodus, bei dem ein Team einem anderen hilft, Fähigkeiten oder Verständnis aufzubauen.

## Inverse Conway Maneuver

Bewusste Veränderung von Kommunikations- oder Teamstrukturen, um eine gewünschte Architektur zu begünstigen.

## Ownership

Verantwortung und tatsächliche Fähigkeit, einen fachlichen oder technischen Bereich zu verändern, zu betreiben und Entscheidungen darüber zu tragen.

## Platform Grouping

Gruppierung von Teams, die gemeinsam interne Plattformfähigkeiten bereitstellen.

## Socio-Technical Congruence

Grad der Passung zwischen erforderlichen Koordinationsbeziehungen und tatsächlichen Koordinationsmustern.

## Stream-aligned Team

Team, das an einem Flow von Arbeit beziehungsweise Wert ausgerichtet ist.

## Team API

Explizite Beschreibung der Leistungen, Schnittstellen und Interaktionswege eines Teams.

## X-as-a-Service

Interaktionsmodus, in dem ein Team eine klar definierte Fähigkeit mit geringer notwendiger direkter Koordination bereitstellt.

---

# Teil XXVI — Quellen und weiterführende Literatur

## 137. Melvin E. Conway — How Do Committees Invent?

Melvin E. Conway, *How Do Committees Invent?*, Datamation, 1968.

Original beim Autor:

https://melconway.com/Home/pdf/committees.pdf

Relevant für:

- Zusammenhang zwischen Kommunikationsstruktur und Systemdesign,
- Einschränkung möglicher Designs durch Organisationsstrukturen,
- Koordinationsproblem bei Arbeitsteilung.

---

## 138. MacCormack, Rusnak, Baldwin — Mirroring Hypothesis

Alan MacCormack, John Rusnak, Carliss Baldwin, *Exploring the Duality between Product and Organizational Architectures: A Test of the Mirroring Hypothesis*, Research Policy 41, 2012, S. 1309–1324.

https://www.hbs.edu/faculty/Pages/item.aspx?num=32217

Relevant für:

- empirische Untersuchung der Beziehung zwischen Organisations- und Produktarchitektur,
- Modularität,
- unterschiedliche Organisationsformen.

Die Arbeit wird hier als empirische Evidenz für strukturelle Zusammenhänge verwendet, nicht als deterministisches Organisationsgesetz.

---

## 139. Cataldo, Wagstrom, Herbsleb, Carley — Coordination Requirements

Marcelo Cataldo, Patrick A. Wagstrom, James D. Herbsleb, Kathleen M. Carley, *Identification of Coordination Requirements: Implications for the Design of Collaboration and Awareness Tools*, CSCW 2006.

DOI:

https://doi.org/10.1145/1180875.1180929

Relevant für:

- Task Dependencies,
- Coordination Requirements,
- technische Abhängigkeit als Treiber organisatorischer Koordination.

---

## 140. Cataldo, Herbsleb, Carley — Socio-Technical Congruence

Marcelo Cataldo, James D. Herbsleb, Kathleen M. Carley, *Socio-Technical Congruence: A Framework for Assessing the Impact of Technical and Work Dependencies on Software Development Productivity*, ESEM 2008.

DOI:

https://doi.org/10.1145/1414004.1414008

Relevant für:

- Passung zwischen technischen Arbeitsabhängigkeiten und tatsächlicher Koordination,
- Grenzen rein technischer Modularisierung,
- Zusammenhang mit Bearbeitungszeiten von Änderungen.

---

## 141. Team Topologies

Matthew Skelton, Manuel Pais, *Team Topologies*.

Aktuelle offizielle Key Concepts:

https://teamtopologies.com/key-concepts

Relevant für:

- Stream-aligned Teams,
- Enabling Teams,
- Complicated Subsystem Teams,
- Platform Teams beziehungsweise Platform Groupings,
- Collaboration,
- X-as-a-Service,
- Facilitation,
- Flow und Cognitive Load.

Team Topologies wird in diesem Dokument als Organisations- und Analysemodell verwendet, nicht als wissenschaftlich zwingende Organisationsform.

---

## 142. Team Topologies — Core Team Types

Offizielle Beschreibung:

https://teamtopologies.com/key-concepts-content/what-are-the-core-team-types-in-team-topologies

Relevant für:

- Zweck der vier grundlegenden Teamtypen,
- Unterstützung von Stream-aligned Teams,
- Reduktion unnötiger Belastung.

---

## 143. John Sweller — Cognitive Load

John Sweller, *Cognitive Load During Problem Solving: Effects on Learning*, Cognitive Science 12(2), 1988, S. 257–285.

DOI:

https://doi.org/10.1016/0364-0213(88)90023-7

Relevant für:

- begrenzte kognitive Verarbeitung beim individuellen Lernen und Problemlösen.

Wichtige Abgrenzung:

> Diese Forschung liefert keine direkte wissenschaftliche Formel für Teamgröße oder Anzahl besitzbarer Systeme.

Die Übertragung auf Teamdesign ist eine praktische Organisationsheuristik.

---

## 144. Thoughtworks — Inverse Conway Maneuver

Thoughtworks Technology Radar, *Inverse Conway Maneuver*.

https://www.thoughtworks.com/en-de/radar/techniques/inverse-conway-maneuver

Relevant für:

- Begriff und praktische Idee des gezielten Ausrichtens von Team- und Kommunikationsstrukturen an gewünschter Architektur.

Der Eintrag ist historisch und nicht Teil der aktuellen Radar-Ausgabe; das Dokument behandelt ihn deshalb als etablierte Heuristik, nicht als aktuelle universelle Empfehlung.

---

## 145. ISO/IEC 38500:2024

ISO/IEC 38500:2024, *Information technology — Governance of IT for the organization*.

https://www.iso.org/standard/81684.html

Relevant für:

- Governance von IT als Bestandteil organisationaler Governance,
- Rolle von governing bodies,
- effektive, effiziente und akzeptable Nutzung von IT,
- Anwendbarkeit auf öffentliche Organisationen und externe Service Provider.

---

## 146. ISO/IEC 38505-1:2026

ISO/IEC 38505-1:2026, *Information technology — Governance of data — Part 1: Application of ISO/IEC 38500 to the governance of data*.

https://www.iso.org/standard/87195.html

Relevant für:

- Governance von Daten,
- Verantwortlichkeit für Nutzung und Schutz von Daten,
- Einordnung von Data Governance in IT- und Organisationsgovernance.

---

## 147. Verhältnis zu anderen Knowledge Items

- AK-023 — Domain-Driven Design
- AK-025 — SOLID
- AK-026 — Einfachheit, DRY, YAGNI und geringe Wissenskopplung
- AK-031 — Hexagonal Architecture / Ports & Adapters
- AK-056 — Modularer Monolith
- AK-075 — Architecture Decision Process
- AK-079 — Modulith Strategy
- AK-084 — Kopplung, Kohäsion und Information Hiding
- AK-086 — Governance von Querschnittskonzepten
- AK-087 — Architekturbewertung
- AK-089 — Strategic DDD und Context Mapping
- AK-090 — Evolutionary Architecture

---

# 148. Merksatz

> Eine Architekturgrenze ist erst dann belastbar, wenn nicht nur klar ist, **welche Softwareteile getrennt sind**, sondern auch **wer die Grenze besitzt, wer Entscheidungen treffen darf, wie andere Teams mit ihr interagieren, wer sie betreibt und wie die Organisation verbleibende Abhängigkeiten koordiniert**.
