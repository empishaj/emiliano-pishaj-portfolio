---
id: AK-084
legacy_ids:
  - ADR-084
title: Kopplung, Kohäsion, Information Hiding und Änderungsgrenzen
artifact_type: architecture-principle
domain: software-architecture
status: active
maturity: reviewed
normative_level: recommended
last_validated: 2026-10-03
review_trigger:
  - grundlegende Änderung der Modularitäts- oder Architekturstandards
  - relevante Änderung des Qualitätsmodells ISO/IEC 25010
  - neue belastbare Forschung zu Change Coupling oder sozio-technischen Abhängigkeiten
---

# AK-084 — Kopplung, Kohäsion, Information Hiding und Änderungsgrenzen

## 1. Zweck dieses Dokuments

Softwarearchitektur entsteht wesentlich dadurch, **wo Grenzen gezogen werden und welches Wissen diese Grenzen überschreiten darf**.

Viele Designprobleme lassen sich auf zwei Grundfragen zurückführen:

1. **Was gehört zusammen?**
2. **Was muss voneinander wissen, abhängen oder gemeinsam verändert werden?**

Die erste Frage führt zur **Kohäsion**.

Die zweite Frage führt zur **Kopplung**.

Information Hiding ergänzt beide Perspektiven um eine dritte Frage:

> **Welche Designentscheidung soll außerhalb einer Grenze gerade nicht bekannt sein?**

Das Ziel lautet nicht, abstrakt maximale Kohäsion und minimale Kopplung zu erzeugen.

Ein reales System braucht Zusammenarbeit und damit Abhängigkeiten.

Die wichtigere Architekturfrage lautet:

> **Welche Abhängigkeiten sind durch Fachlichkeit oder Qualitätsziele notwendig, welche sind selbst erzeugt und welche Designentscheidungen müssen hinter stabilen Grenzen verborgen werden, damit Änderungen lokal bleiben?**

Dieses Dokument betrachtet Kopplung und Kohäsion über mehrere Ebenen:

- Code,
- Modul,
- Daten,
- API,
- Runtime,
- Deployment,
- Team,
- Lieferant,
- Organisation,
- Enterprise.

---

# Teil I — Historische und theoretische Grundlagen

## 2. Herkunft von Kopplung und Kohäsion

Die systematische Verwendung von Coupling und Cohesion als Kriterien für Softwaremodularisierung geht auf die Structured-Design-Arbeiten von Wayne P. Stevens, Glenford J. Myers und Larry L. Constantine zurück.

Der 1974 veröffentlichte Beitrag *Structured Design* beschäftigt sich mit der Zerlegung von Programmen in Module und bewertet dabei unter anderem:

- die Stärke der Verbindungen zwischen Modulen,
- die Art der Kommunikation,
- die innere Zusammengehörigkeit eines Moduls.

Eine bis heute relevante Grundidee lautet:

> Weniger und einfachere Verbindungen zwischen Modulen erleichtern es, ein Modul zu verstehen und Änderungen auf einen kleineren Bereich zu begrenzen.

Die konkreten historischen Kategorien entstanden in einer anderen technischen Epoche.

Sie sollten deshalb nicht mechanisch auf moderne Java-, Cloud- oder Microservice-Systeme übertragen werden.

Ihre Grundfrage bleibt jedoch aktuell:

> **Wie stark sind Teile miteinander verbunden und warum gehören Elemente innerhalb einer Grenze zusammen?**

---

## 3. Parnas und Information Hiding

David Parnas formulierte 1972 einen komplementären Blick auf Modularisierung.

Die entscheidende Frage war nicht nur:

> Welche Verarbeitungsschritte folgen aufeinander?

Sondern:

> Welche Designentscheidungen sollten jeweils in einem Modul verborgen werden?

Parnas zeigte, dass zwei Systeme dieselbe Funktion erfüllen und trotzdem sehr unterschiedliche Änderbarkeit besitzen können, abhängig davon, **nach welchem Kriterium sie zerlegt wurden**.

Ein zentrales Prinzip daraus:

> Module sollten Wissen über Designentscheidungen kapseln, die sich unabhängig ändern können.

Beispiele moderner Designentscheidungen:

- Datenbanktechnologie,
- Dateiformat,
- Provider-spezifischer API-Vertrag,
- IAM-Claim-Struktur,
- Mapping auf ein externes Register,
- fachliche Berechnungsregel,
- Serialisierungsformat,
- Cache-Mechanismus.

Information Hiding ist damit stärker als Sichtbarkeit durch **private**.

Es geht nicht nur darum, ob ein Feld von außen erreichbar ist.

Es geht darum:

> **Welches Wissen darf außerhalb des Moduls überhaupt entstehen?**

---

## 4. Modularität ist kein Selbstzweck

Ein System kann in sehr viele Module zerlegt sein und trotzdem schwer änderbar bleiben.

Beispiel:

~~~text
Module A
→ Module B
→ Module C
→ Module D
→ Module E
~~~

wenn jede kleine Änderung alle fünf betrifft.

Umgekehrt kann ein größerer Modulith gut veränderbar sein, wenn:

- interne Grenzen klar sind,
- Datenownership eindeutig ist,
- Verträge stabil sind,
- Abhängigkeiten gerichtet sind,
- Änderungen lokal bleiben.

Deshalb gilt:

~~~text
viele Module
≠
hohe Modularität
~~~

Modularität zeigt sich in der Qualität der Grenzen und im Änderungsverhalten.

---

## 5. Qualitätskontext ISO/IEC 25010

ISO/IEC 25010:2023 definiert ein Produktqualitätsmodell für ICT- und Softwareprodukte.

Im Qualitätskontext Maintainability ist Modularity eine wichtige Perspektive auf die Frage, wie stark eine Änderung an einer Komponente andere Komponenten beeinflusst.

Für Architekturarbeit bedeutet das:

> Kopplung, Kohäsion und Information Hiding sind keine isolierten Stilfragen. Sie stehen in engem Zusammenhang mit Änderbarkeit, Analysierbarkeit, Testbarkeit und Wartbarkeit.

Die Norm liefert jedoch kein universelles Kopplungsmaß und keine konkrete Modulzerlegung.

Die eigentliche Architekturentscheidung bleibt kontextabhängig.

---

# Teil II — Grundbegriffe

## 6. Kohäsion

Kohäsion beschreibt, wie stark die Bestandteile einer Einheit inhaltlich zusammengehören.

Eine Einheit kann sein:

- Methode,
- Klasse,
- Package,
- Modul,
- Service,
- Datenprodukt,
- Plattformfähigkeit.

Hohe Kohäsion bedeutet nicht:

> Die Einheit ist klein.

Sondern:

> Ihre Bestandteile besitzen einen nachvollziehbaren gemeinsamen Zweck, gemeinsame Regeln oder einen gemeinsamen Änderungsgrund.

---

## 7. Kopplung

Kopplung beschreibt Abhängigkeit oder Bindung zwischen Teilen eines Systems.

Abhängigkeit kann bedeuten:

- A importiert B,
- A ruft B auf,
- A kennt das Datenschema von B,
- A benötigt B gleichzeitig zur Laufzeit,
- A muss gemeinsam mit B ausgerollt werden,
- Änderungen an A erzwingen Änderungen an B,
- Team A kann ohne Team B nicht liefern.

Damit ist Kopplung wesentlich breiter als ein Importgraph.

---

## 8. Information Hiding

Information Hiding fragt nicht nur:

> Welche API ist öffentlich?

Sondern:

> Welche Designentscheidung wird durch diese Grenze verborgen?

Eine gute Grenze kann beispielsweise verbergen:

~~~text
externer Registervertrag
      |
      v
RegisterAdapter
      |
      v
fachlicher RegisterPort
~~~

Der fachliche Teil kennt dann nicht:

- externe Feldnamen,
- Transportdetails,
- Providerfehlercodes,
- technische Authentisierung des Providers.

Diese Details dürfen sich ändern, ohne die Fachlogik zu dominieren.

---

## 9. Encapsulation und Information Hiding unterscheiden

Für dieses Dokument ist folgende Unterscheidung hilfreich:

### Encapsulation

Mechanismus, der Zugriff auf interne Repräsentation kontrolliert.

### Information Hiding

Designentscheidung darüber, welche Kenntnisse außerhalb einer Grenze nicht benötigt werden sollen.

Beispiel:

~~~text
Consumer kennt CreditAssessment
aber nicht das proprietäre Response-Schema des Providers
~~~

Encapsulation ist ein Werkzeug.

Information Hiding ist das Designziel.

---

# Teil III — Kohäsion verstehen

## 10. Hohe Kohäsion

Beispiel eines kohärenten Moduls:

~~~text
case-decision/
├── DecisionPolicy
├── DecisionRule
├── DecisionResult
├── DecisionReason
└── DecisionEvidence
~~~

Die Bestandteile teilen:

- dieselbe fachliche Sprache,
- ähnliche Änderungsgründe,
- dieselbe fachliche Verantwortung.

---

## 11. Niedrige Kohäsion

~~~text
common/
├── TaxCalculator
├── MailSender
├── PdfGenerator
├── DateParser
├── EncryptionHelper
└── CaseStatusMapper
~~~

Der gemeinsame Grund lautet hier nur:

> Diese Dinge passen nirgendwo anders hin.

Das ist organisatorische Ablage, keine fachliche Kohäsion.

---

## 12. Klassische Cohesion-Kategorien

Die Structured-Design-Literatur unterscheidet historisch mehrere Kohäsionsformen.

Die Terminologie ist weiterhin didaktisch nützlich, sollte aber nicht als exakte moderne Messskala verstanden werden.

### Coincidental Cohesion

Elemente befinden sich zufällig zusammen.

### Logical Cohesion

Elemente gehören grob derselben technischen Kategorie an, obwohl sie fachlich unabhängig sind.

### Temporal Cohesion

Elemente werden zusammengefasst, weil sie zur selben Zeit ausgeführt werden.

### Procedural Cohesion

Elemente hängen vor allem durch eine Ablaufreihenfolge zusammen.

### Communicational Cohesion

Operationen arbeiten auf denselben Daten.

### Sequential Cohesion

Output einer Aktivität ist Input der nächsten.

### Functional Cohesion

Eine Einheit dient einem klar umrissenen Zweck.

In modernen Systemen sind diese Kategorien eher Analysebegriffe als ein Ranking, das mechanisch angewendet werden sollte.

---

## 13. Fachliche Kohäsion

Für Domain- und Enterprise-Architektur ist eine weitere Perspektive wichtig:

> Gehören Regeln zur selben fachlichen Verantwortung?

Beispiel:

~~~text
Case Eligibility
~~~

kann kohärent sein, wenn enthaltene Regeln:

- dieselbe fachliche Entscheidung unterstützen,
- denselben Owner besitzen,
- gemeinsam fachlich verändert werden.

---

## 14. Change Cohesion

Eine praktische Frage lautet:

> Ändern sich diese Elemente typischerweise gemeinsam aus demselben Grund?

Wenn ja, spricht das für eine sinnvolle Zusammengehörigkeit.

Wenn zwei Elemente im selben Modul liegen, aber über Jahre unabhängig geändert werden, kann das ein Hinweis auf geringe Kohäsion sein.

Umgekehrt können Dateien in unterschiedlichen Packages historisch fast immer gemeinsam geändert werden.

Dann existiert möglicherweise eine versteckte fachliche Einheit.

---

## 15. Kohäsion und Größe sind nicht dasselbe

Eine große Klasse kann hohe Kohäsion besitzen.

Eine kleine Klasse kann niedrige Kohäsion besitzen.

Beispiel:

~~~java
class Utility {
    String formatDate(...) { ... }
    String hashPassword(...) { ... }
}
~~~

Nur zwei Methoden.

Trotzdem kaum fachliche Kohäsion.

---

## 16. Kohäsion und Single Responsibility

SRP und Kohäsion sind eng verwandt.

SRP fragt:

> Welche unterschiedlichen Änderungsgründe sind in einer Einheit gekoppelt?

Kohäsion fragt:

> Warum gehören diese Elemente überhaupt zusammen?

Beide Perspektiven helfen, Grenzen zu beurteilen.

Sie sind aber keine identischen Begriffe.

---

# Teil IV — Kopplung ist mehrdimensional

## 17. Kopplung darf nicht auf Imports reduziert werden

Ein Java-Modul kann keinerlei Import auf ein anderes Modul besitzen und trotzdem stark gekoppelt sein.

Beispiele:

- gemeinsames Datenbankschema,
- gemeinsamer Eventvertrag,
- exakte Reihenfolgeannahmen,
- koordinierte Releases,
- gemeinsame Betriebsparameter.

Deshalb sollte Kopplung immer mehrdimensional analysiert werden.

---

## 18. Source-Code-Kopplung

~~~text
A → B
~~~

A benötigt Typen oder Implementierungen aus B.

Beispiel:

~~~java
import external.provider.sdk.ProviderResponse;
~~~

in fachlicher Domänenlogik.

Die Kopplung ist unmittelbar im Source sichtbar.

---

## 19. Structural Coupling

A kennt die interne Struktur von B.

Beispiel:

~~~java
caseFile
    .applicant()
    .address()
    .municipality()
    .code();
~~~

oder systemisch:

~~~text
Service A liest Tabellen von Service B direkt.
~~~

Änderungen an der Struktur von B propagieren nach A.

---

## 20. Data Coupling

Mehrere Einheiten hängen von derselben Datenrepräsentation ab.

Beispiel:

~~~text
Service A
Service B
Service C
   |
   +--> shared_case_table
~~~

Das kann Deployment- und Änderungsabhängigkeiten erzeugen, selbst wenn jeder Service eine eigene API besitzt.

---

## 21. Schema Coupling

Consumer kennen konkrete Felder und Bedeutungen eines Schemas.

Beispiel:

~~~json
{
  "state": "A17",
  "flag": "J",
  "type": "03"
}
~~~

Wenn Consumer die internen Codes direkt interpretieren, verbreitet sich Providerwissen.

Eine semantische Grenze kann stattdessen ein internes Modell bereitstellen.

---

## 22. Semantic Coupling

Semantic Coupling bedeutet:

> Ein Consumer hängt von der Bedeutung eines Verhaltens oder Datenpunkts ab.

Diese Kopplung verschwindet nicht durch JSON, REST oder Events.

Beispiel:

~~~text
Event:
ApplicationStatusChanged
~~~

Consumer B interpretiert:

~~~text
status = 7
→ fachlich abgeschlossen
~~~

Wenn diese Bedeutung nicht stabiler Vertragsbestandteil ist, entsteht fragile semantische Kopplung.

---

## 23. Control Coupling

Ein Consumer steuert interne Ablauflogik eines anderen Moduls.

Beispiel:

~~~java
service.execute(
        command,
        Mode.SKIP_VALIDATION,
        true,
        false);
~~~

Der Aufrufer kennt interne Kontrollentscheidungen.

Stabiler kann ein fachlicher Vertrag sein:

~~~java
service.reprocess(command);
~~~

falls Reprocessing tatsächlich eine eigenständige Fähigkeit ist.

---

## 24. Temporal Coupling

Zwei Komponenten sind zeitlich gekoppelt, wenn sie:

- gleichzeitig verfügbar sein müssen,
- in enger Reihenfolge arbeiten müssen,
- innerhalb eines gemeinsamen Zeitfensters reagieren müssen.

Beispiel:

~~~text
Portal
→ Case API
→ Register API
→ external Provider
~~~

Die Verfügbarkeit des Portals hängt nun möglicherweise von der gesamten synchronen Kette ab.

---

## 25. Order Coupling

Auch asynchrone Systeme können Reihenfolgekopplung besitzen.

Beispiel:

~~~text
CaseCreated
muss vor
CaseApproved
verarbeitet werden.
~~~

Wenn Consumer diese Reihenfolge benötigen, existiert eine reale Kopplung.

Ein Message Broker entfernt sie nicht.

---

## 26. Availability Coupling

Consumer A kann seine Aufgabe nur erfüllen, wenn B verfügbar ist.

Synchronität ist ein häufiger, aber nicht der einzige Auslöser.

Auch ein asynchroner Prozess kann fachlich von rechtzeitiger Verfügbarkeit eines Downstream-Systems abhängen.

---

## 27. Deployment Coupling

A und B müssen gemeinsam deployed werden.

Ursachen können sein:

- gemeinsame Binärdatei,
- inkompatible API-Änderung,
- gemeinsame Datenbankmigration,
- unversionierter Vertrag,
- Build-Abhängigkeit.

---

## 28. Release Coupling

Technisch getrennte Deployments können trotzdem koordinierte Releases verlangen.

Beispiel:

~~~text
Producer v4
nur kompatibel mit
Consumer v7
~~~

Dann existiert Release Coupling.

---

## 29. Runtime Coupling

Zur Laufzeit hängen Komponenten voneinander ab.

Das ist nicht automatisch schlecht.

Jeder verteilte Geschäftsprozess benötigt Runtime-Zusammenarbeit.

Die Frage lautet:

> Ist die Abhängigkeit explizit, notwendig und betrieblich beherrscht?

---

## 30. Operational Coupling

Komponenten können betrieblich gekoppelt sein durch:

- gemeinsames Scaling,
- gemeinsame Incident-Diagnose,
- gemeinsame Queue,
- gemeinsame Datenbank,
- gemeinsamen Cluster,
- gemeinsame Zertifikate,
- gemeinsame Rate Limits.

Diese Abhängigkeiten erscheinen nicht im Klassendiagramm.

---

## 31. Security Coupling

Beispiele:

- Consumer interpretiert Provider-spezifische Rollenstrings.
- mehrere Systeme teilen denselben technischen Account.
- Autorisierungslogik ist an einen konkreten IAM-Provider gebunden.
- Token Claims sind im ganzen Unternehmen hart codiert.

Eine interne Identity-/Authorization-Abstraktion kann bestimmte Providerdetails kapseln.

---

## 32. Organizational Coupling

Eine Änderung benötigt regelmäßig mehrere Teams.

Beispiel:

~~~text
kleine Fachregel
→ Team A
→ Datenbankteam
→ Plattformteam
→ Integrationsteam
→ Lieferant
~~~

Nicht jede Abstimmung ist vermeidbar.

Aber häufige organisatorische Koordination kann ein Hinweis auf technische oder Ownership-Grenzen sein.

---

## 33. Vendor Coupling

Ein System kann an einen Lieferanten gekoppelt sein durch:

- proprietäre APIs,
- proprietäre Datenformate,
- Spezialbibliotheken,
- Betriebswissen,
- Vertrags- und Lizenzmodelle,
- fehlende Exportmöglichkeiten.

Das ist eine Enterprise-relevante Kopplungsdimension.

Sie ist nicht automatisch schlecht.

Sie sollte aber bewusst sein.

---

## 34. Procurement Coupling

Im Behördenkontext kann eine Architekturentscheidung Beschaffungsabhängigkeiten erzeugen.

Beispiele:

- nur ein Hersteller erfüllt eine proprietäre Schnittstelle,
- mehrere Lose müssen für eine Änderung gleichzeitig beauftragt werden,
- ein Betriebsvertrag blockiert technische Änderungsgeschwindigkeit.

Diese Kopplung ist nicht im Code sichtbar.

Sie kann trotzdem die tatsächliche Änderbarkeit dominieren.

---

# Teil V — Klassische Kopplungskategorien als Denkmodell

## 35. Historische Perspektive

Structured Design unterschied Kopplung nach Art und Stärke der Verbindung zwischen Modulen.

Diese Taxonomie entstand vor modernen OO-, API- und Cloud-Systemen.

Sie bleibt nützlich, um zu erkennen, warum manche Verbindungen fragiler sind als andere.

---

## 36. Content Coupling

Ein Modul greift in Interna eines anderen ein.

Modernes Beispiel:

~~~text
Service A
ändert direkt Tabellen von Service B.
~~~

oder Reflection verändert fremde Interna.

Das verletzt klare Ownership.

---

## 37. Common Coupling

Mehrere Module teilen globalen mutable Zustand.

Beispiel:

~~~text
global shared config map
shared mutable database table
shared singleton state
~~~

Änderungen und Seiteneffekte können schwer lokalisierbar werden.

---

## 38. Control Coupling

Ein Aufrufer übergibt Steuerinformationen, die bestimmen, welchen internen Pfad der Empfänger nimmt.

Nicht jeder Mode-Parameter ist falsch.

Problematisch wird es, wenn der Consumer interne Mechanismen orchestriert.

---

## 39. Stamp Coupling

Ein großes Datenobjekt wird übergeben, obwohl der Consumer nur einen kleinen Teil benötigt.

Modernes Beispiel:

~~~text
EntireCaseAggregate
→ NotificationService
~~~

obwohl NotificationService nur:

~~~text
recipient
templateId
caseReference
~~~

benötigt.

Dadurch entsteht unnötiges Wissen über die Gesamtstruktur.

---

## 40. Data Coupling

Module tauschen explizit genau die benötigten Daten aus.

In der klassischen Betrachtung ist das häufig günstiger als größere implizite Abhängigkeiten.

Modern muss zusätzlich geprüft werden:

- Semantik,
- Versionierung,
- Datenschutz,
- Ownership.

---

# Teil VI — Information Hiding praktisch anwenden

## 41. Volatilität identifizieren

Eine gute Hidden Design Decision ist häufig etwas, das sich unabhängig ändern kann.

Fragen:

- Was ist providerabhängig?
- Was ist frameworkabhängig?
- Was ist ein externes Schema?
- Was ist eine volatile fachliche Policy?
- Was ist eine technische Optimierung?

---

## 42. Beispiel externer Registervertrag

Schwach:

~~~java
public Decision evaluate(
        ExternalRegisterResponse response) {

    if ("A17".equals(response.statusCode())) {
        ...
    }
}
~~~

Die Fachlogik kennt:

- Providerdatentyp,
- Providercode,
- externe Semantik.

---

## 43. Interne semantische Grenze

~~~java
public record RegistryAssessment(
        RegistryStatus status,
        Instant assessedAt) {
}
~~~

~~~java
public interface RegistryAssessmentPort {
    RegistryAssessment assess(PersonId personId);
}
~~~

Adapter:

~~~text
Provider Response
      |
      v
Provider Adapter
      |
      v
RegistryAssessment
~~~

Jetzt kann sich der Providervertrag ändern, ohne dass jede Fachregel seine Codes kennen muss.

---

## 44. Information Hiding bedeutet nicht generisches Wrappern

Schwach:

~~~java
interface GenericExternalService {
    Map<String, Object> call(
            String operation,
            Map<String, Object> payload);
}
~~~

Technische Details sind scheinbar versteckt.

Semantisch wurde aber keine stabile Grenze geschaffen.

Eine gute Abstraktion verbirgt nicht nur Syntax.

Sie bietet eine **bedeutungsvolle interne Sprache**.

---

## 45. Stable Interface ist nicht immutable Interface

Eine stabile Grenze darf sich weiterentwickeln.

Stabil bedeutet:

> Änderungen werden bewusst, kompatibel und kontrolliert vorgenommen.

Nicht:

> Die API darf nie geändert werden.

---

# Teil VII — Datenkopplung und Ownership

## 46. Shared Database ist eine starke Grenze

Zwei Anwendungen können getrennt deployed werden und trotzdem dieselben Tabellen besitzen.

~~~text
Application A ─┐
               ├── shared schema
Application B ─┘
~~~

Dann stellen sich Fragen:

- Wer besitzt Schemaänderungen?
- Wer darf schreiben?
- Wer garantiert Invarianten?
- Wer plant Migrationen?
- Wer entscheidet über Indizes?

Ohne klare Antwort ist die Systemgrenze möglicherweise nur visuell getrennt.

---

## 47. Shared Read ist anders als Shared Write

Ein Read-only Reporting-Zugriff kann andere Risiken besitzen als konkurrierende Writes.

Trotzdem existiert Schema Coupling.

Die Architektur sollte unterscheiden:

~~~text
shared write ownership
shared read dependency
published data product
replicated read model
~~~

---

## 48. Datenreplikation kann Kopplung verschieben

Lokale Read Models reduzieren direkte Datenbankabhängigkeit.

Dafür entstehen:

- Event- oder ETL-Abhängigkeit,
- Aktualitätssemantik,
- Reconciliation,
- Datenqualitätsfragen.

Entkopplung ist deshalb meist:

> Transformation von Kopplung, nicht vollständige Entfernung.

---

## 49. Data Ownership

Eine robuste Grenze beantwortet:

- Wer ist fachlicher Owner?
- Wer ist technischer System of Record?
- Wer darf mutieren?
- Welche Consumer erhalten Kopien?
- Wie werden Änderungen publiziert?
- Welche Semantik besitzt Staleness?

---

# Teil VIII — Synchron und asynchron

## 50. Synchroner Aufruf

~~~text
A → B
~~~

Vorteile:

- unmittelbare Antwort,
- einfacher Kontrollfluss,
- klare Fehlerpropagation,
- häufig einfachere Konsistenzsemantik.

Kosten:

- zeitliche und Verfügbarkeitskopplung,
- Latenzakkumulation,
- Failure Propagation.

---

## 51. Asynchrones Event

~~~text
A → Event Broker → B
~~~

kann reduzieren:

- gleichzeitige Verfügbarkeitsanforderung,
- direkte Adressierung des Consumers.

Dafür entstehen:

- Eventual Consistency,
- Retry- und Idempotenzbedarf,
- Schemaevolution,
- Observability,
- Reihenfolgefragen,
- Ownership des Events.

---

## 52. Events sind nicht automatisch loose coupling

Ein Event:

~~~text
CaseChanged
~~~

mit 70 Feldern und internen Statuscodes kann Consumer stark an das Datenmodell des Producers koppeln.

Ein Event kann mehr Consumer gleichzeitig binden als eine gezielte API.

---

## 53. Event Notification versus Event-Carried State

Eine kleine Notification:

~~~text
CaseDecisionCompleted
caseId
~~~

erzeugt andere Kopplung als ein vollständiges State-Transfer-Event.

Beide Ansätze besitzen Trade-offs.

Entscheidend ist:

> Welches Wissen muss der Consumer besitzen, und wer ist Owner der Wahrheit?

---

# Teil IX — Code-, Modul- und Build-Grenzen

## 54. Package-Grenzen

Java-Packages können semantische Grenzen unterstützen.

Aber ein Package-Name allein erzwingt nichts.

~~~text
domain/
adapter/
application/
~~~

ist nur dann relevant, wenn Abhängigkeitsregeln tatsächlich gelten.

---

## 55. Java Module oder Build Module

Separate Module können:

- Dependency Direction erzwingen,
- interne APIs begrenzen,
- Build Boundaries schaffen.

Sie können aber auch:

- Build-Komplexität,
- Versionierungsaufwand,
- Navigation

erhöhen.

Grenzen brauchen einen Grund.

---

## 56. Cyclic Dependencies

~~~text
A → B
↑   ↓
D ← C
~~~

Zyklen erschweren häufig:

- isolierte Änderung,
- Test,
- Build,
- Verständnis,
- Wiederverwendung.

Nicht jeder graphische Zyklus hat gleiche Schwere.

Aber zyklische Abhängigkeiten sind ein starkes Review-Signal.

---

## 57. Dependency Direction

Ein häufig sinnvolles Ziel ist, dass fachlich stabile Policies nicht unnötig technische Details importieren.

Beispiel:

~~~text
adapter
   |
   v
port
   ^
   |
domain policy
~~~

Die konkrete Struktur hängt vom Architekturmodell ab.

---

# Teil X — Change Coupling und Evolution

## 58. Statische Struktur zeigt nicht alle Abhängigkeiten

Zwei Module können im Code unabhängig erscheinen.

Trotzdem werden sie in jeder Änderung gemeinsam angepasst.

Das ist Logical oder Change Coupling.

---

## 59. Release-Historie als Evidence

Gall, Hajek und Jazayeri untersuchten 1998 Release-Historien eines großen Telekommunikationssystems, um logisch gekoppelte Module anhand gemeinsamer Änderungshistorien zu erkennen.

Die Grundidee ist bis heute praktisch:

> Gemeinsame Änderungsmuster können versteckte Abhängigkeiten sichtbar machen, die ein statischer Dependency Graph nicht zeigt.

---

## 60. Beispiel

~~~text
Änderung 101:
A, B

Änderung 108:
A, B

Änderung 121:
A, B

Änderung 130:
A, B
~~~

obwohl kein direkter Source-Import zwischen A und B existiert.

Mögliche Ursachen:

- gemeinsames Protokoll,
- duplizierte Regel,
- koordinierte Datenmigration,
- versteckte semantische Kopplung.

---

## 61. Change Coupling ist kein Beweis

Gemeinsame Commits können auch entstehen durch:

- monolithische Tickets,
- Formatierung,
- Release-Vorbereitung,
- organisatorische Arbeitsweise.

Deshalb:

> Change Coupling ist ein Analysehinweis, kein automatisches Refactoring-Signal.

---

## 62. Change Amplification

Eine weitere praktische Größe:

> Wie viele Artefakte müssen sich ändern, wenn eine fachliche Entscheidung geändert wird?

Beispiel:

~~~text
eine neue fachliche Fallart
→ 14 Services
→ 7 Datenbanken
→ 4 Teams
~~~

Das kann auf eine ungünstige Verteilung des fachlichen Wissens hindeuten.

---

# Teil XI — Messbarkeit

## 63. Kopplung ist teilweise messbar

Mögliche technische Signale:

- Anzahl Dependencies,
- afferent/efferent Dependencies,
- Zyklen,
- API-Abhängigkeiten,
- Shared Schemas,
- Contract References,
- Change Coupling,
- Deployment Coupling.

Aber:

~~~text
niedrige Zahl
≠ automatisch gute Architektur
~~~

---

## 64. Klassische Metriken

Metriken wie:

- Coupling Between Objects,
- afferent coupling,
- efferent coupling,
- Instability,
- LCOM-Varianten

können Signale liefern.

Sie messen jeweils nur bestimmte strukturelle Aspekte.

Sie messen nicht direkt:

- fachliche Semantik,
- Ownership,
- Vendor Lock-in,
- organisatorischen Koordinationsaufwand.

---

## 65. Kohäsionsmetriken haben Grenzen

Kohäsion ist besonders schwer auf eine einzelne Zahl zu reduzieren.

Eine Klasse kann metrisch auffällig, fachlich aber sehr kohärent sein.

Oder umgekehrt.

Deshalb sollten Metriken immer mit:

- Sprache der Domäne,
- Change History,
- Ownership,
- konkreten Änderungsszenarien

kombiniert werden.

---

## 66. Dependency Graph

Ein Dependency Graph kann sichtbar machen:

- Hubs,
- Zyklen,
- unerwartete Layer-Verletzungen,
- zentrale Shared Libraries.

Er beantwortet nicht automatisch:

> Ist diese Abhängigkeit falsch?

---

## 67. Deployment Graph

Zusätzlich sinnvoll:

~~~text
Service
→ Database
→ Broker
→ IAM
→ external API
~~~

Damit werden Runtime- und Betriebsabhängigkeiten sichtbar, die der Source Graph nicht zeigt.

---

## 68. Team-/Ownership-Map

~~~text
Capability
→ Fachowner
→ Anwendungsowner
→ Datenowner
→ Betrieb
→ Security
→ Lieferant
~~~

Wenn jede technische Änderung viele Ownership-Grenzen quert, sollte geprüft werden, ob Architektur und Verantwortung sinnvoll geschnitten sind.

---

# Teil XII — Sozio-technische Kopplung

## 69. Technische Abhängigkeiten erzeugen Koordinationsbedarf

Wenn zwei Teams Komponenten verändern, die stark voneinander abhängen, entsteht Koordinationsbedarf.

Forschung zu socio-technical congruence untersucht den Zusammenhang zwischen technischen Task Dependencies und organisatorischer Kommunikation beziehungsweise Koordination.

Die wichtige Architekturlektion:

> Technische Grenzen und Teamgrenzen sind nicht unabhängig voneinander.

---

## 70. Keine automatische Conway-Formel

Daraus folgt nicht:

~~~text
jede technische Abhängigkeit
→ gleiches Team
~~~

Große Organisationen benötigen bewusst teamübergreifende Verträge.

Die Frage lautet:

> Ist der notwendige Koordinationsbedarf bekannt und beherrschbar?

---

## 71. Shared Ownership

Ein Modul mit fünf gleichberechtigten Owning Teams kann theoretisch flexibel wirken.

Praktisch kann jede Änderung Abstimmung verlangen.

Shared Ownership braucht deshalb klare Mechanismen:

- Verantwortungsmodell,
- Maintainer,
- Reviewregeln,
- Versionierung,
- Entscheidungsweg.

---

## 72. Plattformkopplung

Eine Plattform reduziert Wiederholung.

Sie erzeugt gleichzeitig Abhängigkeit.

Beispiel:

~~~text
30 Teams
→ zentrale CI/CD Plattform
~~~

Das kann sehr sinnvoll sein.

Aber Plattformverfügbarkeit, Roadmap, API-Stabilität und Supportmodell werden Enterprise-relevant.

Das Ziel ist nicht keine Plattformkopplung.

Das Ziel ist **bewusst gestaltete Plattformkopplung**.

---

# Teil XIII — Durchgängiger Praxisfall

## 73. Ausgangslage: digitale Antragsbearbeitung

Angenommen, ein Fachverfahren bearbeitet Anträge und benötigt:

- fachliche Entscheidungslogik,
- Registerabfrage,
- Dokumentenerzeugung,
- Benachrichtigung,
- Persistenz.

Eine erste Struktur:

~~~text
ApplicationService
├── entscheidet fachlich
├── kennt Register-DTO
├── schreibt SQL
├── erzeugt PDF
├── versendet Mail
└── interpretiert IAM-Claims
~~~

---

## 74. Analyse der Kohäsion

Frage:

> Warum gehören diese Verantwortungen zusammen?

Die Antwort lautet häufig nur:

> Sie werden während desselben Use Cases benötigt.

Das ist keine ausreichende fachliche Kohäsion.

---

## 75. Volatile Entscheidungen identifizieren

Mögliche unabhängige Änderungsgründe:

~~~text
Entscheidungsregel
Registerprovider
PDF-Technologie
Mailprovider
Datenbankschema
IAM-Provider
~~~

Diese Entscheidungen können sich unabhängig ändern.

---

## 76. Information-Hiding-Grenzen

Mögliche Struktur:

~~~text
ProcessApplication
      |
      +--> DecisionPolicy
      |
      +--> RegistryPort
      |
      +--> DocumentPort
      |
      +--> NotificationPort
      |
      +--> ApplicationRepository
~~~

Adapter kapseln technische Details.

---

## 77. Nicht überabstrahieren

Es ist nicht automatisch nötig:

~~~text
AbstractPortFactory
GenericProviderAdapter
UniversalIntegrationFacade
~~~

zu bauen.

Jede Grenze sollte ein konkretes Wissen oder einen realen Variationspunkt verbergen.

---

## 78. Datenownership prüfen

Wenn Notification direkt Tabellen des Case-Moduls liest, bleibt starke Datenkopplung bestehen.

Besser kann ein expliziter Contract oder ein geeigneter Snapshot/Event sein.

---

## 79. Temporale Kopplung prüfen

Wenn jeder Antrag synchron:

~~~text
Case
→ Register
→ Document
→ Notification
~~~

aufruft, hängt die End-to-End-Verfügbarkeit an der gesamten Kette.

Danach muss fachlich entschieden werden:

- Was muss synchron sein?
- Was kann später erfolgen?
- Was braucht sofortige Bestätigung?

Nicht:

> Alles auf Events umstellen.

---

## 80. Release Coupling prüfen

Wenn eine Änderung des Registervertrags gleichzeitig Fachmodul, Mapping, Tests und API-Consumer ändert, ist zu prüfen, welches Providerwissen die Grenze überschritten hat.

---

## 81. Organisatorische Kopplung prüfen

Wenn eine kleine Regeländerung vier Teams und zwei Lieferanten benötigt, kann das auf:

- verteilte Regelimplementierung,
- Shared Schema,
- unklare Ownership

hinweisen.

---

## 82. Ergebnis des Praxisfalls

~~~text
fachliche Kohäsion
      +
explizite Ports
      +
klare Datenownership
      +
bewusste Sync/Async-Entscheidung
      +
stabile Verträge
      =
kleinerer Änderungsradius
~~~

Nicht jede Abhängigkeit wurde entfernt.

Die notwendigen Abhängigkeiten wurden **sichtbar, gerichtet und beherrschbar** gemacht.

---

# Teil XIV — Enterprise- und Behördenkontext

## 83. Kopplung zwischen Fachverfahren

In Behörden entstehen Abhängigkeiten häufig über:

- Register,
- zentrale Identitätsdienste,
- Dokumentendienste,
- Portale,
- Querschnittsplattformen,
- gemeinsame Datenbestände.

Die zentrale Frage lautet:

> Welche Information oder Fähigkeit ist wirklich gemeinsam, und welche interne Struktur wird unnötig geteilt?

---

## 84. Föderale Kopplung

Bei Bund-, Länder- oder Kommunalgrenzen können technische Verträge zusätzlich abhängen von:

- Zuständigkeit,
- Datenhoheit,
- rechtlicher Grundlage,
- Betriebsverantwortung,
- Releasekoordination.

Diese Dimensionen gehören in die Architekturbetrachtung.

---

## 85. Standardisierung als kontrollierte Kopplung

Ein Standard koppelt Systeme bewusst an gemeinsame Regeln.

Beispiel:

~~~text
OpenAPI-Konvention
Security Baseline
Identity Standard
Logging Standard
~~~

Das ist nicht automatisch negativ.

Die Kopplung wird akzeptiert, weil sie Interoperabilität, Sicherheit, Betrieb oder Wiederverwendung verbessert.

Architektur bedeutet daher nicht:

> Kopplung vermeiden.

Sondern:

> **Kopplung dort vereinheitlichen, wo sie gemeinsamen Nutzen schafft, und lokale Freiheit dort erhalten, wo Unterschiede fachlich notwendig sind.**

---

## 86. Lieferantenkopplung

In lang laufenden öffentlichen IT-Vorhaben ist relevant:

- Kann die Behörde Architekturentscheidungen selbst nachvollziehen?
- Sind Schnittstellen und Datenformate dokumentiert?
- Ist Betriebswissen ausschließlich beim Lieferanten?
- Gibt es proprietäre Erweiterungen ohne Exit-Pfad?

Technische Information-Hiding-Grenzen können helfen, Lieferantenabhängigkeit zu begrenzen.

Sie ersetzen aber keine Beschaffungs- und Vertragsstrategie.

---

## 87. Architektur und Verantwortung

Eine technische Grenze ohne Owner bleibt fragil.

Für wesentliche Grenzen sollte klar sein:

~~~text
Capability
→ fachlicher Owner
→ Datenowner
→ Anwendungsowner
→ Betriebsverantwortung
→ Security-Verantwortung
→ Lieferant
→ Entscheidungsgremium
~~~

Damit wird Kopplung nicht nur technisch, sondern governancefähig.

---

# Teil XV — Architecture Guardrails und Evidence

## 88. Dependency Direction automatisieren

Beispiel mit ArchUnit:

~~~java
noClasses()
    .that().resideInAPackage("..domain..")
    .should().dependOnClassesThat()
    .resideInAnyPackage("..adapter..");
~~~

Eine solche Regel ist nur sinnvoll, wenn die Architekturentscheidung fachlich begründet ist.

---

## 89. Zyklen prüfen

Automatisierte Checks können Package- oder Modulzyklen erkennen.

Ein Fund bedeutet:

> Review erforderlich.

Nicht:

> Architektur automatisch abgelehnt.

---

## 90. Contract Tests

API- oder Eventverträge können durch Contract Tests überprüft werden.

Das reduziert nicht jede Kopplung.

Es macht bestimmte Kopplung **explizit und überprüfbar**.

---

## 91. Schema Ownership

Für relevante Datenbestände sollte dokumentiert sein:

- Owner,
- Schreibrechte,
- Consumer,
- Migrationsprozess.

Direkte Shared-Database-Zugriffe über Ownership-Grenzen sollten mindestens sichtbar und begründet sein.

---

## 92. Change-Coupling-Analyse

Git-Historie kann regelmäßig auf:

- häufig gemeinsam geänderte Dateien,
- Cross-Module Commits,
- Hotspots

untersucht werden.

Interessant ist besonders:

> Ändern sich Artefakte gemeinsam, obwohl die Architektur behauptet, sie seien unabhängig?

---

## 93. Deployment Evidence

Mögliche Nachweise:

- unabhängige Deployments,
- kompatible API-Versionen,
- entkoppelte Datenmigrationen,
- keine zwingende koordinierte Releasekette.

---

## 94. Runtime Evidence

Observability kann zeigen:

- Latenzketten,
- Downstream-Abhängigkeiten,
- Failure Propagation,
- Retry Storms,
- zentrale Bottlenecks.

Damit wird Runtime Coupling sichtbar.

---

## 95. Organisatorische Evidence

Mögliche Indikatoren:

- Anzahl Teams pro Änderung,
- Anzahl notwendiger Freigaben,
- Anzahl Lieferantenübergaben,
- Wartezeit durch Dependencies.

Diese Zahlen sind keine universellen Qualitätsgrenzen.

Sie helfen, strukturelle Engpässe zu erkennen.

---

# Teil XVI — Praktisches Reviewverfahren

## 96. Schritt 1 — Änderungsszenario formulieren

Beispiel:

> Der externe Registerprovider ändert sein Datenmodell.

oder:

> Eine fachliche Entscheidungsregel wird angepasst.

---

## 97. Schritt 2 — Change Surface bestimmen

Welche Artefakte müssten heute geändert werden?

- Klassen,
- Module,
- APIs,
- Datenbanken,
- Events,
- Deployments,
- Teams.

---

## 98. Schritt 3 — gemeinsamen Änderungsgrund prüfen

Was gehört tatsächlich zusammen?

Welche Elemente ändern sich aus demselben Grund?

---

## 99. Schritt 4 — unnötiges Wissen suchen

Welche internen Details sind außerhalb ihrer Ownership-Grenze bekannt?

---

## 100. Schritt 5 — Kopplungsart benennen

Nicht nur:

> stark gekoppelt.

Sondern beispielsweise:

- Schema Coupling,
- Temporal Coupling,
- Release Coupling,
- Semantic Coupling,
- Organizational Coupling.

Die Benennung macht Trade-offs diskutierbar.

---

## 101. Schritt 6 — Notwendigkeit prüfen

Ist diese Kopplung fachlich notwendig?

Oder durch aktuelle Technik entstanden?

---

## 102. Schritt 7 — Hidden Decision identifizieren

Welche volatile Entscheidung könnte hinter einer stabileren Grenze liegen?

---

## 103. Schritt 8 — Alternative Grenzen entwerfen

Mindestens vergleichen:

~~~text
bestehende Grenze
neue interne API
Port/Adapter
lokale Replikation
gemeinsame Plattform
keine neue Grenze
~~~

---

## 104. Schritt 9 — Kosten der Entkopplung bewerten

Eine neue Grenze kostet möglicherweise:

- Mapping,
- zusätzliche API,
- Latenz,
- Eventual Consistency,
- Betrieb,
- Governance.

---

## 105. Schritt 10 — Datenownership prüfen

Wer besitzt Daten und fachliche Bedeutung?

---

## 106. Schritt 11 — Team-/Vendor-Grenzen prüfen

Welche Koordination erzeugt die Architektur?

---

## 107. Schritt 12 — Evidence definieren

Wie wird die gewünschte Grenze überprüft?

- ArchUnit,
- Contract Test,
- Change Coupling,
- Deployment-Unabhängigkeit,
- Ownership Map,
- Runtime Tracing.

---

# Teil XVII — Typische Fehlanwendungen

## 108. Interfaces = geringe Kopplung

Falsch.

Ein großes Interface kann extrem starke semantische Kopplung besitzen.

---

## 109. Events = entkoppelt

Falsch.

Events erzeugen weiterhin:

- Schema,
- Semantik,
- Reihenfolge,
- Lifecycle

als mögliche Abhängigkeiten.

---

## 110. Microservices = unabhängig

Falsch.

Shared Database, koordinierte Releases und gemeinsame Teams können starke Kopplung erzeugen.

---

## 111. Keine Imports = keine Kopplung

Falsch.

Daten-, Runtime-, semantische und organisatorische Kopplung bleiben möglich.

---

## 112. Shared Library = Wiederverwendung ohne Kosten

Falsch.

Eine Shared Library erzeugt:

- Versionsabhängigkeit,
- Releaseabhängigkeit,
- Ownership-Fragen.

---

## 113. Hohe Kohäsion = kleine Klasse

Falsch.

Größe und Kohäsion sind unterschiedliche Eigenschaften.

---

## 114. Ein Service pro Capability = automatisch richtig

Zu grob.

Eine Capability kann mehrere technische Komponenten benötigen.

Umgekehrt können kleine Capabilities gemeinsam in einem kohärenten Modul leben.

---

## 115. Niedrige Kopplung um jeden Preis

Falsch.

Extrem viele Grenzen erzeugen:

- Mapping,
- Netzwerk,
- Versionierung,
- Betriebsaufwand.

---

## 116. Information Hiding = Wrapper um alles

Falsch.

Ein Wrapper ohne eigene stabile Semantik versteckt häufig nur Syntax.

---

## 117. Zyklus = immer Architekturfehler

Zu absolut.

Ein Zyklus ist ein starkes Analyse-Signal.

Seine Bedeutung hängt von Ebene und Kontext ab.

---

## 118. Zahl = Architektururteil

Eine Kopplungsmetrik ist kein ausreichender Beweis für schlechtes Design.

Metriken brauchen Kontext.

---

# Teil XVIII — Review-Checklisten

## 119. Kohäsion

- Welcher gemeinsame Zweck verbindet die Elemente?
- Haben sie gemeinsame Änderungsgründe?
- Teilen sie dieselbe fachliche Sprache?
- Gibt es Elemente, die nur aus Bequemlichkeit dort liegen?
- Könnte die Einheit in unabhängig evolvierende Verantwortungen zerfallen?

## 120. Source- und Structural Coupling

- Welche internen Typen werden über Grenzen importiert?
- Kennt ein Consumer interne Datenstrukturen?
- Gibt es direkte Zugriffe auf fremde Implementierung?

## 121. Data Coupling

- Wer besitzt das Schema?
- Wer darf schreiben?
- Wer ist Source of Truth?
- Können Consumer unabhängig migrieren?

## 122. Semantic Coupling

- Welche Bedeutungen muss ein Consumer kennen?
- Sind Codes und Statuswerte stabiler Vertragsbestandteil?
- Ist Semantik dokumentiert?

## 123. Temporal Coupling

- Müssen Komponenten gleichzeitig verfügbar sein?
- Welche Latenzkette entsteht?
- Ist synchrone Antwort fachlich notwendig?

## 124. Async Coupling

- Welche Reihenfolge wird erwartet?
- Welche Eventual Consistency ist zulässig?
- Wer besitzt das Event?
- Wie wird Schemaevolution behandelt?

## 125. Deployment und Release

- Können Komponenten unabhängig deployed werden?
- Müssen Releases koordiniert werden?
- Welche Datenmigration bindet Versionen?

## 126. Organisation

- Wie viele Teams benötigt eine typische Änderung?
- Wer besitzt die Grenze?
- Welche Abstimmung ist fachlich notwendig?
- Welche ist historisch gewachsen?

## 127. Vendor

- Welche proprietären Verträge sind außerhalb eines Adapters sichtbar?
- Gibt es Export- und Migrationsfähigkeit?
- Wo liegt kritisches Betriebswissen?

## 128. Information Hiding

- Welche Designentscheidung verbirgt das Modul?
- Ist diese Entscheidung tatsächlich volatil oder komplex?
- Ist die öffentliche Sprache stabiler als das verborgene Detail?

---

# Teil XIX — Woran erkennt man eine tragfähige Modulgrenze?

## 129. Eigenschaften

Eine tragfähige Grenze zeigt typischerweise:

- hohe fachliche oder technische Kohäsion,
- klaren Owner,
- explizite Verträge,
- kontrollierte Dependency Direction,
- versteckte volatile Details,
- klar geregelte Datenownership,
- möglichst lokalen Änderungsradius,
- verständliche Runtime-Abhängigkeiten,
- beherrschbare Release- und Teamkoordination.

Die Balance lautet:

~~~text
notwendige Zusammenarbeit
+
klare Ownership
+
stabile Verträge
+
verborgene volatile Details
+
so wenig unnötige Abhängigkeit wie sinnvoll
~~~

---

# Teil XX — Glossar

## Afferent Coupling

Abhängigkeiten anderer Elemente auf ein betrachtetes Element.

## Change Coupling

Historisch beobachtetes gemeinsames Änderungsverhalten von Artefakten.

## Cohesion

Grad der inhaltlichen Zusammengehörigkeit innerhalb einer Einheit.

## Coupling

Abhängigkeit oder Bindung zwischen Einheiten.

## Data Ownership

Verantwortung für Bedeutung, Integrität und Änderung eines Datenbestands.

## Deployment Coupling

Abhängigkeit, die gemeinsame oder koordinierte Deployments erfordert.

## Efferent Coupling

Abhängigkeiten eines betrachteten Elements auf andere Elemente.

## Information Hiding

Verbergen einer Designentscheidung hinter einer Grenze, damit fremde Teile diese Entscheidung nicht kennen müssen.

## Logical Coupling

Nicht zwingend statisch sichtbare Abhängigkeit, die sich etwa durch gemeinsames Änderungsverhalten zeigt.

## Modularity

Strukturelle Eigenschaft eines Systems, bei der Komponenten mit definierten Verantwortungen und Grenzen organisiert werden.

## Release Coupling

Abhängigkeit, die koordinierte Versionen oder Releases verlangt.

## Semantic Coupling

Abhängigkeit von der Bedeutung eines Vertrags, Werts oder Verhaltens.

## Temporal Coupling

Abhängigkeit von gleichzeitiger Verfügbarkeit oder zeitlicher Reihenfolge.

---

# Teil XXI — Quellen und weiterführende Literatur

## 130. David L. Parnas — Information Hiding und Modularisierung

David L. Parnas, *On the Criteria to Be Used in Decomposing Systems into Modules*, Communications of the ACM, Vol. 15, No. 12, 1972, S. 1053–1058.

DOI:

https://doi.org/10.1145/361598.361623

Relevant für:

- Modularisierung nach Änderungsentscheidungen,
- Information Hiding,
- Änderbarkeit und Verständlichkeit.

---

## 131. Stevens, Myers, Constantine — Structured Design

Wayne P. Stevens, Glenford J. Myers, Larry L. Constantine, *Structured Design*, IBM Systems Journal, Vol. 13, No. 2, 1974, S. 115–139.

DOI:

https://doi.org/10.1147/sj.132.0115

Relevant für:

- frühe systematische Behandlung von Coupling und Cohesion,
- Modulverbindungen,
- Modularitätskriterien.

Die historischen Kategorien werden in diesem Dokument als Analysebegriffe verwendet, nicht als universelle moderne Rangskala.

---

## 132. Yourdon / Constantine — Structured Design

Edward Yourdon, Larry L. Constantine, *Structured Design: Fundamentals of a Discipline of Computer Program and Systems Design*, Prentice Hall, 1979.

Relevant für:

- ausführliche historische Taxonomien von Kopplung und Kohäsion,
- strukturierte Modularisierung.

---

## 133. Gall, Hajek, Jazayeri — Logical Coupling

Harald Gall, Karin Hajek, Mehdi Jazayeri, *Detection of Logical Coupling Based on Product Release History*, International Conference on Software Maintenance, 1998, S. 190–197.

DOI:

https://doi.org/10.1109/ICSM.1998.738508

Relevant für:

- Nutzung von Release-Historien,
- Erkennung gemeinsamer Änderungsmuster,
- versteckte logische Abhängigkeiten zwischen Modulen.

---

## 134. ISO/IEC 25010:2023

ISO/IEC 25010:2023, *Systems and software engineering — Systems and software Quality Requirements and Evaluation (SQuaRE) — Product quality model*.

https://www.iso.org/standard/78176.html

Relevant für:

- Produktqualitätsmodell,
- Maintainability,
- Modularity als Qualitätskontext.

ISO/IEC 25010:2011 wurde durch die Ausgabe 2023 ersetzt.

---

## 135. Cataldo et al. — Coordination Requirements

Marcelo Cataldo, Patrick A. Wagstrom, James D. Herbsleb, Kathleen M. Carley, *Identification of Coordination Requirements: Implications for the Design of Collaboration and Awareness Tools*, CSCW 2006.

DOI:

https://doi.org/10.1145/1180875.1180929

Relevant für:

- technische Task Dependencies,
- daraus entstehenden Koordinationsbedarf,
- sozio-technische Betrachtung von Softwareentwicklung.

Die Forschung wird hier nicht als Beweis für eine bestimmte Teamstruktur verwendet, sondern als Grundlage dafür, technische und organisatorische Abhängigkeiten gemeinsam zu analysieren.

---

## 136. Verhältnis zu anderen Knowledge Items

- AK-025 — SOLID: Änderbarkeit, Verantwortung und Abhängigkeitsdesign
- AK-026 — Einfachheit, Wissensduplikation, YAGNI und geringe Wissenskopplung
- AK-031 — Hexagonal Architecture / Ports & Adapters
- AK-041 — Event-Driven Architecture
- AK-052 — Immutability, Invarianten, Ownership und defensive Grenzen
- AK-085 — Sozio-technische Architektur, Ownership und Teamgrenzen
- AK-089 — Strategic DDD / Context Mapping
- AK-090 — Evolutionary Architecture
- AK-095 — AsyncAPI und Event Contract Governance

---

# 137. Merksatz

> Gute Modularität bedeutet nicht, möglichst viele Grenzen zu erzeugen. Sie bedeutet, **zusammengehöriges Wissen kohärent zu bündeln**, **notwendige Abhängigkeiten explizit zu machen**, **volatile Designentscheidungen zu verbergen** und den Änderungsradius über Code, Daten, Runtime, Deployment und Organisation hinweg beherrschbar zu halten.
