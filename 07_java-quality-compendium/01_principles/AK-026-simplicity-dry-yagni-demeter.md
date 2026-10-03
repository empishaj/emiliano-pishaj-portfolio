---
id: AK-026
legacy_ids:
  - QG-JAVA-026
title: Einfachheit, Wissensduplikation, YAGNI und geringe Wissenskopplung
artifact_type: architecture-principle
domain: software-design
status: active
maturity: reviewed
normative_level: recommended
last_validated: 2026-10-03
review_trigger:
  - grundlegende Änderung der Design- oder Modularity-Standards
  - neue belastbare empirische Forschung zu Codeverständlichkeit, Duplikation oder Kopplung
---

# AK-026 — Einfachheit, Wissensduplikation, YAGNI und geringe Wissenskopplung

## 1. Zweck dieses Dokuments

KISS, DRY, YAGNI und die Law of Demeter werden häufig als kurze Merksätze vermittelt:

- Keep It Simple.
- Don't Repeat Yourself.
- You Aren't Gonna Need It.
- Talk only to your immediate friends.

Als Erinnerungsstützen sind solche Formeln nützlich. Für reale Software- und Architekturentscheidungen reichen sie jedoch nicht aus.

Jede dieser Heuristiken adressiert einen anderen Teil desselben Grundproblems:

> **Wie vermeiden wir selbst erzeugte Komplexität, ohne notwendige fachliche, technische oder regulatorische Komplexität wegzudiskutieren?**

Dieses Dokument behandelt die vier Prinzipien deshalb nicht als starre Regeln, sondern als zusammenhängende Werkzeuge für:

- Verständlichkeit,
- Änderbarkeit,
- Modularität,
- Wissensverteilung,
- Abhängigkeitskontrolle,
- Vermeidung spekulativer Architektur,
- und die Begrenzung unnötiger kognitiver Last.

Die Leitfrage lautet nicht:

> Ist dieses Design KISS, DRY, YAGNI und Demeter-konform?

Sondern:

> Welche Komplexität ist durch das Problem unvermeidbar, welche haben wir selbst hinzugefügt und welche Abstraktionen reduzieren die Gesamtkosten einer Änderung tatsächlich?

---

# Teil I — Komplexität verstehen

## 2. Einfachheit ist nicht Simplifizierung

Einfachheit und Simplifizierung sind nicht dasselbe.

Eine einfache Lösung:

- bildet die relevanten Anforderungen vollständig ab,
- macht wichtige Regeln sichtbar,
- hält Verantwortungen nachvollziehbar,
- begrenzt unnötige Abhängigkeiten,
- und verwendet nicht mehr Konzepte als nötig.

Eine simplifizierte Lösung dagegen entfernt notwendige Aspekte des Problems.

Beispiel:

~~~text
"Wir brauchen keine Audit-Historie.
Das macht die Lösung nur kompliziert."
~~~

Wenn Nachvollziehbarkeit aus einer fachlichen, regulatorischen oder betrieblichen Anforderung folgt, ist das keine Vereinfachung. Es ist eine unvollständige Architektur.

Dasselbe gilt für:

- Authentisierung,
- Autorisierung,
- Datenschutz,
- Wiederanlauf,
- Backup,
- Observability,
- Barrierefreiheit,
- Nachweisführung,
- Konsistenzanforderungen,
- oder definierte Betriebsgrenzen.

Einfachheit bedeutet daher nicht:

> Möglichst wenig Architektur.

Sondern:

> **Nur die Architektur, die für das tatsächliche Problem und seine Qualitätsanforderungen begründbar ist.**

---

## 3. Notwendige und selbst erzeugte Komplexität

Frederick P. Brooks unterscheidet in *No Silver Bullet* zwischen Schwierigkeiten, die in der Sache selbst liegen, und solchen, die aus der Art ihrer technischen Umsetzung entstehen.

Diese Trennung ist nicht immer scharf. Sie ist dennoch als Denkmodell sehr hilfreich.

### 3.1 Notwendige Komplexität

Komplexität kann aus dem Problem selbst entstehen:

- komplexe Rechts- oder Fachregeln,
- zahlreiche fachlich notwendige Zustände,
- unterschiedliche Zuständigkeiten,
- zeitabhängige Regeln,
- konkurrierende Qualitätsziele,
- verteilte Organisationen,
- externe Register und Partner,
- regulatorische Nachweispflichten,
- hohe Verfügbarkeitsanforderungen,
- Datenschutz- und Sicherheitsauflagen.

Diese Komplexität verschwindet nicht, nur weil der Code kurz ist.

### 3.2 Selbst erzeugte Komplexität

Andere Komplexität entsteht durch Designentscheidungen:

- unnötige Schichten,
- generische Frameworks ohne realen Variationspunkt,
- doppelte Datenmodelle ohne Zweck,
- indirekte Kontrollflüsse,
- Event-Ketten für eigentlich lokale Vorgänge,
- eigene DSLs für triviale Regeln,
- zu frühe Plattformisierung,
- mehrfach repräsentiertes Wissen,
- unnötige technische Kopplung,
- unklare Ownership.

Das Ziel professionellen Designs ist daher nicht:

~~~text
Komplexität → 0
~~~

sondern:

~~~text
notwendige Komplexität
+
so wenig zusätzliche Komplexität wie sinnvoll
~~~

---

## 4. Komplexität ist mehrdimensional

"Das ist zu komplex" ist ohne Präzisierung eine schwache Diagnose.

Mindestens folgende Dimensionen sollten unterschieden werden:

### 4.1 Strukturelle Komplexität

Wie viele Elemente und Beziehungen müssen verstanden werden?

Beispiele:

- Klassen,
- Module,
- Services,
- Abhängigkeiten,
- Interfaces,
- Datenmodelle.

### 4.2 Kontrollflusskomplexität

Wie schwer ist nachvollziehbar, was wann geschieht?

Beispiele:

- tiefe Verschachtelung,
- Callback-Ketten,
- asynchrone Event-Flows,
- versteckte Interceptors,
- Reflection,
- dynamische Dispatch-Logik.

### 4.3 Datenkomplexität

Wie viele Repräsentationen, Transformationen und Wahrheiten existieren?

~~~text
REST DTO
→ Domain Model
→ JPA Entity
→ Kafka Event
→ Search Document
→ Reporting Model
~~~

Mehrere Modelle können sinnvoll sein. Sie sind aber nicht kostenlos.

### 4.4 Fachliche Komplexität

Wie kompliziert ist die Domäne selbst?

Beispiele:

- gesetzliche Anspruchsregeln,
- Fristen,
- Sonderfälle,
- Zuständigkeiten,
- mehrstufige Entscheidungen.

### 4.5 Verteilte Komplexität

Welche zusätzlichen Probleme entstehen durch Netzwerk- und Prozessgrenzen?

- Latenz,
- partielle Ausfälle,
- Retries,
- Timeouts,
- Konsistenz,
- Versionierung,
- Observability,
- Ownership.

### 4.6 Organisatorische Komplexität

Wie viele Teams, Verantwortungen und Entscheidungswege müssen koordiniert werden?

Diese Dimension ist besonders relevant für Architektur:

> Eine technisch elegante Lösung kann organisatorisch sehr teuer sein.

---

## 5. Programmverständnis ist ein realer Kostenfaktor

Software wird nicht nur geschrieben. Sie wird gelesen, untersucht, verändert, überprüft und betrieben.

Empirische Forschung untersucht deshalb seit langem Program Comprehension und Code Understandability.

Studien zeigen unter anderem:

- Programmverständnis nimmt einen erheblichen Anteil an Wartungsarbeit ein.
- Struktur- und Komplexitätsmaße können Teilaspekte von Verständlichkeit abbilden.
- Einzelne Metriken sind jedoch keine vollständigen Modelle menschlicher Verständlichkeit.
- Auch bei Metriken wie Cognitive Complexity bestehen Grenzen hinsichtlich Vorhersagekraft und Interpretation.

Daraus folgt:

> "Einfachheit" ist nicht nur ästhetische Präferenz. Verständlichkeit beeinflusst reale Änderungs- und Wartungsarbeit.

Aber ebenso:

> Keine einzelne Zahl beweist, dass ein Design einfach oder kompliziert ist.

---

## 6. Information Hiding als theoretische Grundlage

David Parnas zeigte 1972, dass Modularität nicht nur davon abhängt, *dass* ein System in Module zerlegt wird, sondern *nach welchen Kriterien*.

Ein zentraler Gedanke:

> Module sollten Designentscheidungen verbergen, die sich unabhängig ändern können.

Das ist für alle vier Themen dieses Dokuments relevant:

- **KISS:** unnötige Details nicht über das System verteilen.
- **DRY:** Wissen nicht in mehreren unabhängigen Repräsentationen pflegen.
- **YAGNI:** keine abstrakten Variationspunkte ohne konkreten Treiber bauen.
- **Law of Demeter:** Wissen über fremde interne Strukturen begrenzen.

Damit ist Einfachheit nicht bloß Kürze, sondern auch eine Frage der **Informationsverteilung**.

---

# Teil II — KISS: die einfachste tragfähige Lösung

## 7. Was KISS in diesem Dokument bedeutet

Die genaue historische Zuschreibung des Akronyms KISS ist für seine fachliche Anwendung nicht entscheidend.

Hier wird KISS als Designheuristik verwendet:

> **Wähle die einfachste Lösung, die die bekannten funktionalen und qualitativen Anforderungen vollständig und nachvollziehbar erfüllt.**

Drei Wörter sind dabei wichtig:

### Einfachste

Keine unnötigen Konzepte.

### Bekannte

Keine Architektur für rein hypothetische Anforderungen.

### Vollständig

Keine notwendige Qualität wegoptimieren.

---

## 8. Einfachheit ist kontextabhängig

### Situation A

Eine Anwendung benötigt drei statische Konfigurationswerte.

Eine Lösung mit:

- Plugin Registry,
- dynamischer Datenbank,
- Event Bus,
- Admin UI,
- Versionierungsworkflow

wäre wahrscheinlich unverhältnismäßig.

### Situation B

Konfiguration muss:

- pro Mandant unterschiedlich sein,
- historisiert werden,
- zeitgesteuert aktiviert werden,
- auditiert werden,
- von Fachadministratoren gepflegt werden,
- ohne Deployment änderbar sein.

Dann kann ein dediziertes Konfigurationssystem die **einfachere Gesamtlösung** sein.

Es besitzt mehr Komponenten, reduziert aber andere Komplexität.

~~~text
weniger Komponenten
≠ automatisch
einfacheres System
~~~

---

## 9. Lokale Einfachheit versus Gesamtsystem

Eine häufige Architekturfehlentscheidung entsteht, wenn nur lokale Einfachheit betrachtet wird.

Beispiel:

~~~text
Team A:
"Wir schreiben direkt in die Tabelle.
Das ist am einfachsten."
~~~

Lokal stimmt das vielleicht.

Systemweit entstehen möglicherweise:

- verletzte Datenhoheit,
- unklare Ownership,
- Schema-Kopplung,
- fehlendes Audit,
- konkurrierende Writes,
- schwerere Migration.

Ein Architekt muss deshalb mindestens zwei Ebenen unterscheiden:

~~~text
lokale Implementierungskosten
vs.
Gesamtkosten des Systems
~~~

Eine Lösung darf lokal etwas aufwendiger sein, wenn sie systemweit relevante Komplexität reduziert.

---

## 10. Explizitheit ist häufig einfacher als Cleverness

Kurzer Code ist nicht automatisch einfacher Code.

~~~java
return Optional.ofNullable(application)
        .map(Application::applicant)
        .map(Applicant::age)
        .filter(this::eligible)
        .map(this::approve)
        .orElseGet(this::reject);
~~~

Dieser Code kann sinnvoll sein.

Er kann aber auch eine fachlich wichtige Entscheidung verstecken.

Eine explizitere Variante kann verständlicher sein:

~~~java
if (application == null) {
    return Decision.notProcessable();
}

Applicant applicant = application.applicant();

if (!eligibilityRule.isSatisfiedBy(applicant)) {
    return Decision.rejected();
}

return Decision.approved();
~~~

Entscheidend ist nicht die Syntax.

Die Frage lautet:

> Welche Darstellung macht die fachlich relevante Struktur am leichtesten erkennbar?

---

## 11. Einfach darf Betriebsrealität nicht ignorieren

Ein lokal minimaler Ansatz kann Betriebsprobleme erzeugen.

Ein HTTP-Aufruf ohne Timeout ist weniger Code.

Eine produktionsreife Lösung benötigt möglicherweise:

- Timeout,
- Fehlerklassifikation,
- Metrik,
- Trace,
- Retry nur bei geeigneten Fehlern,
- Fallback oder definierte Degradation.

Das ist mehr Technik.

Aber nicht notwendigerweise unnötige Komplexität.

KISS bedeutet deshalb nicht:

> Entferne Resilience.

Sondern:

> Implementiere genau die Resilience, die aus realen Ausfall- und Qualitätsanforderungen folgt.

---

## 12. KISS-Prüffragen

- Welches konkrete Problem löst dieses Element?
- Welche Anforderung würde ohne dieses Element verletzt?
- Kann der Kontrollfluss ohne Tooling erklärt werden?
- Gibt es einen einfacheren Mechanismus mit denselben Eigenschaften?
- Ist die Komplexität lokal oder systemweit reduziert?
- Wird notwendige Komplexität nur versteckt?
- Ist eine Plattform oder ein Framework größer als das Problem?
- Ist die Lösung explizit genug, um ihr Verhalten im Fehlerfall zu verstehen?

---

# Teil III — YAGNI: Zukunft nicht mit Spekulation verwechseln

## 13. Herkunft und Idee

YAGNI steht für:

> You Aren't Gonna Need It.

Der Begriff stammt aus dem Umfeld von Extreme Programming und Simple beziehungsweise Incremental Design.

Martin Fowler beschreibt YAGNI als Gegenposition zu "presumptive features": Funktionen oder Erweiterungen, die heute gebaut werden, weil man annimmt, sie später zu brauchen.

Die Kernaussage:

> Baue eine Fähigkeit nicht nur deshalb, weil sie in einer hypothetischen Zukunft nützlich sein könnte.

---

## 14. Warum spekulative Architektur teuer ist

Eine Funktion kostet nicht nur Implementierungszeit.

Auch ungenutzte Architektur verursacht:

- Designaufwand,
- Tests,
- Dokumentation,
- Security-Betrachtung,
- Dependency-Updates,
- Monitoring,
- Fehlersuche,
- Migration,
- Einarbeitung,
- Entscheidungsbedarf.

Beispiel:

~~~text
"Vielleicht brauchen wir künftig mehrere Storage Provider."
~~~

Daraus entsteht:

~~~text
StorageProvider
StorageProviderFactory
StorageProviderRegistry
StorageCapability
StorageConfiguration
StoragePlugin
StorageHealthIndicator
~~~

Aktuell existiert aber genau ein Storage-System.

Die vermeintliche Zukunftssicherheit erzeugt sofort reale Gegenwartskosten.

---

## 15. YAGNI bedeutet nicht keine Architektur

YAGNI wird häufig missverstanden.

Folgendes sind keine automatisch spekulativen Anforderungen:

- Schutzbedarf,
- Datenschutz,
- Mandantentrennung,
- Verfügbarkeit,
- Barrierefreiheit,
- definierte Wiederanlaufziele,
- Protokollierung,
- Nachvollziehbarkeit,
- gesetzlich bekannte Fristen,
- verbindliche Integrationsvorgaben.

Wenn eine Anforderung bereits bekannt und gültig ist, ist sie kein "vielleicht später".

~~~text
bekannter zukünftiger Bedarf
≠
hypothetischer zukünftiger Bedarf
~~~

---

## 16. Architekturvorbereitung versus Vorabimplementierung

Auf eine mögliche Veränderung vorbereitet zu sein ist nicht dasselbe, sie vollständig vorwegzunehmen.

Beispiel:

Heute existiert ein externer Registerdienst.

Sinnvoll:

~~~text
Domain
→ RegistryPort
→ aktueller Adapter
~~~

wenn der externe Dienst eine klare technische Grenze darstellt.

Nicht automatisch sinnvoll:

~~~text
Provider Marketplace
+ Plugin SPI
+ dynamische Provider Discovery
+ Hot Swapping
~~~

nur weil irgendwann ein zweiter Dienst kommen könnte.

Eine gute Grenze kann zukünftige Änderung ermöglichen, ohne die zukünftige Architektur vollständig zu bauen.

---

## 17. YAGNI und Option Value

YAGNI heißt nicht, spätere Optionen wertlos zu machen.

Das bessere Ziel lautet:

> Heute keine unnötige Fähigkeit bauen, aber irreversible Entscheidungen bewusst erkennen.

Wenn eine Entscheidung später nur mit extrem hohen Migrationskosten korrigiert werden könnte, kann heute zusätzlicher Spielraum sinnvoll sein.

Das ist keine Widerlegung von YAGNI.

Es ist eine Trade-off-Frage zwischen:

~~~text
heutigen Kosten
und
Kosten einer späteren Irreversibilität
~~~

---

## 18. Beispiel: generisches Regel-Framework

Anforderung:

> Zwei fachliche Prüfregeln.

Überdimensionierte Lösung:

~~~text
Rule DSL
Rule Engine
Plugin Loader
Dynamic Rule Registry
Rule Marketplace
Rule Version Graph
~~~

Möglicherweise ausreichende Lösung:

~~~java
public interface EligibilityRule {
    RuleResult evaluate(Application application);
}
~~~

mit zwei Implementierungen.

Wenn später tatsächlich Fachadministration, dynamische Aktivierung, zeitliche Versionierung, Regeln ohne Deployment oder Simulation gefordert werden, kann eine Rule Engine neu bewertet werden.

---

## 19. YAGNI-Prüffragen

- Welche heutige Anforderung rechtfertigt diese Fähigkeit?
- Welche konkrete Änderung erwarten wir?
- Ist sie beschlossen, wahrscheinlich oder nur denkbar?
- Was kostet die spätere Ergänzung tatsächlich?
- Ist die aktuelle Entscheidung reversibel?
- Können wir eine stabile Grenze schaffen, ohne die Zukunft zu implementieren?
- Welche laufenden Kosten erzeugt die Vorabimplementierung?

---

# Teil IV — DRY: Wissen statt Text deduplizieren

## 20. Präzise Bedeutung von DRY

DRY wird häufig als:

> Kein Code darf doppelt vorkommen.

verstanden.

Das ist zu oberflächlich.

Hunt und Thomas formulieren DRY als Forderung, dass ein Stück Wissen eine eindeutige, autoritative Repräsentation im System besitzen soll.

Daraus folgt:

> **DRY adressiert primär Wissensduplikation, nicht visuelle Textähnlichkeit.**

---

## 21. Arten von Duplikation

### 21.1 Textduplikation

Zwei Codeblöcke sehen gleich aus.

### 21.2 Wissensduplikation

Dieselbe fachliche Regel wird mehrfach unabhängig ausgedrückt.

Beispiel:

~~~text
Frontend: Alter >= 18
Backend: Alter >= 18
Batch: Alter >= 18
Dokumentation: Mindestalter 18
~~~

Wenn alle vier dieselbe fachliche Regel repräsentieren, besteht ein Synchronisationsproblem.

### 21.3 Datenduplikation

Dieselbe Information wird in mehreren Speichern gehalten.

Das kann bewusst sinnvoll sein, etwa für:

- Cache,
- Suchindex,
- Reporting,
- Read Model.

Die wichtige Frage lautet dann:

> Welche Repräsentation ist führend und wie werden Kopien synchronisiert?

### 21.4 Schemaduplikation

Ein Vertrag wird mehrfach manuell gepflegt:

~~~text
Java DTO
OpenAPI YAML
TypeScript Type
Wiki Tabelle
~~~

Wenn diese Artefakte unabhängig dieselbe Wahrheit beschreiben, droht Drift.

### 21.5 Policy-Duplikation

Dieselbe Security-, Retention- oder Routingregel wird in mehreren Tools separat konfiguriert.

### 21.6 Dokumentationsduplikation

Dokumentation wiederholt Fakten, die bereits maschinenlesbar vorliegen und regelmäßig auseinanderlaufen.

---

## 22. Gleiche Form ist nicht automatisch gleiches Wissen

Betrachten wir zwei Regeln:

~~~java
boolean eligibleForServiceA(Person p) {
    return p.age() >= 18;
}

boolean eligibleForServiceB(Person p) {
    return p.age() >= 18;
}
~~~

Formal identisch.

Aber möglicherweise:

- Service A basiert auf Rechtsgrundlage X.
- Service B basiert auf Rechtsgrundlage Y.
- unterschiedliche Fachbereiche verantworten die Regeln.
- beide Grenzwerte können unabhängig geändert werden.

Dann wäre eine zentrale gemeinsame Altersregel möglicherweise eine falsche Abstraktion.

Heute ist die Form gleich.

Das Wissen ist nicht zwingend dasselbe.

---

## 23. Gemeinsam ändern als wichtiges Kriterium

Eine nützliche DRY-Frage lautet:

> Müssen diese Stellen sich aus demselben Grund gemeinsam ändern?

Wenn ja, deutet vieles auf dasselbe Wissen hin.

Wenn nein, ist die Ähnlichkeit möglicherweise zufällig.

~~~text
gleiche Rechtsgrundlage
+ gleicher Owner
+ gleiche fachliche Bedeutung
→ eher eine gemeinsame Wissensquelle

gleicher Zahlenwert
+ unterschiedliche Rechtsgrundlagen
+ unterschiedliche Owner
→ eher unabhängige Regeln
~~~

---

## 24. Falsche Abstraktion kann teurer sein als Duplikation

Ein häufiges Muster:

Zwei ähnliche Implementierungen werden sofort zusammengezogen.

Später entwickeln sie sich unterschiedlich.

Dann entstehen Parameter wie:

~~~java
process(
    boolean skipValidation,
    boolean useLegacyCalculation,
    boolean allowSpecialCase,
    Mode mode,
    Strategy strategy)
~~~

Die vermeintliche DRY-Abstraktion wird zur Kopplungsmaschine.

Daraus folgt:

> Temporäre Duplikation kann gesünder sein als eine zu frühe gemeinsame Abstraktion.

---

## 25. Empirische Perspektive auf Code Clones

Code-Duplikation ist kein rein ästhetisches Thema.

Jürgens, Deissenboeck, Hummel und Wagner untersuchten in einer groß angelegten Fallstudie Code Clones und inkonsistente Änderungen.

Sie fanden unter anderem:

- inkonsistente Änderungen an Klonen traten häufig auf,
- ein Teil dieser inkonsistenten Änderungen war mit Fehlern verbunden.

Wichtig:

Diese Forschung bedeutet nicht:

> Jede Duplikation ist ein Defekt.

Sie zeigt vielmehr:

> Wenn dieselbe Implementierungslogik mehrfach existiert und gemeinsam gepflegt werden müsste, können inkonsistente Änderungen ein reales Fehlerrisiko darstellen.

---

## 26. DRY und generierter Code

Generierter Code kann bewusst Wiederholung enthalten.

Beispiel:

~~~text
OpenAPI Contract
      ↓
generated client
generated server stubs
generated types
~~~

Die Textduplikation ist nicht automatisch problematisch, wenn die Wissensquelle eindeutig ist.

~~~text
mehrfacher Text
aber
eine Wissensquelle
~~~

Das kann DRY-konformer sein als mehrere manuell gepflegte Artefakte.

---

## 27. DRY in verteilten Systemen

"Single Source of Truth" wird häufig zu weit interpretiert.

Ein verteiltes System kann bewusst Daten replizieren.

Beispiel:

~~~text
Citizen Service
→ CitizenChanged Event
→ Case Service Read Model
~~~

Die Daten liegen mehrfach vor.

Aber die Verantwortungen sind unterschiedlich:

- Citizen Service besitzt die führende Bürgerinformation.
- Case Service besitzt eine lokale, zweckgebundene Projektion.

Die entscheidende Frage ist:

> Ist die Ownership eindeutig und ist die Synchronisationssemantik verstanden?

Nicht:

> Existiert ein Wert nur einmal im gesamten Unternehmen?

---

## 28. DRY und gemeinsame Libraries

Eine Shared Library kann Duplikation reduzieren.

Sie kann aber auch fachlich unabhängige Systeme koppeln.

Gefährlich:

~~~text
enterprise-common-business-rules.jar
~~~

enthält Regeln aus vielen Domänen.

Jede Änderung erzwingt möglicherweise:

- gemeinsame Versionierung,
- Release-Koordination,
- Abhängigkeit auf fremde Ownership.

Manchmal ist lokale Duplikation mit klarer Verantwortung besser als zentrale Wiederverwendung.

---

## 29. DRY-Prüffragen

- Repräsentieren die Stellen wirklich dasselbe Wissen?
- Haben sie denselben fachlichen Owner?
- Ändern sie sich aus demselben Grund?
- Gibt es eine eindeutige führende Repräsentation?
- Ist die Duplikation bewusst oder versehentlich?
- Was kostet eine falsche gemeinsame Abstraktion?
- Ist Codegenerierung geeigneter als manuelle Synchronisierung?
- Erzeugt Wiederverwendung neue organisatorische Kopplung?

---

# Teil V — Law of Demeter: Wissen über Fremdstrukturen begrenzen

## 30. Historische Einordnung

Die Law of Demeter wurde Ende der 1980er Jahre von Karl Lieberherr, Ian Holland und Arthur Riel im Kontext objektorientierten Designs beschrieben.

Sie sollte:

- Encapsulation fördern,
- Information Hiding stärken,
- Kopplung reduzieren,
- und schmale Kommunikationsbeziehungen begünstigen.

Der populäre Merksatz lautet:

> Talk only to your immediate friends.

Das ist nützlich, aber leicht missverständlich.

---

## 31. Das eigentliche Problem

~~~java
String municipalityCode =
        caseFile
            .getApplicant()
            .getAddress()
            .getMunicipality()
            .getCode();
~~~

Das Problem ist nicht die Anzahl der Punkte.

Das Problem lautet:

> Der aufrufende Code kennt die interne Navigationsstruktur mehrerer fremder Objekte.

Er weiß:

~~~text
CaseFile
hat Applicant
hat Address
hat Municipality
hat Code
~~~

Ändert sich diese Struktur, können viele Consumer betroffen sein.

---

## 32. Klassische Form der Law of Demeter

Vereinfacht formuliert sollte eine Methode hauptsächlich mit:

- dem eigenen Objekt,
- ihren Parametern,
- selbst erzeugten Objekten,
- direkt gehaltenen Collaborators

kommunizieren.

Das Ziel ist nicht mathematische Reinheit.

Es geht um eine begrenzte Wissensreichweite.

---

## 33. Tell, don't ask — verwandt, aber nicht identisch

Eine mögliche Verbesserung lautet:

Statt Datenstruktur herauszulesen:

~~~java
if (order.getCustomer().getStatus().isBlocked()) {
    ...
}
~~~

kann Verhalten näher an die zuständige Einheit gebracht werden:

~~~java
if (!order.mayBeProcessed()) {
    ...
}
~~~

Das kann:

- Fachsprache stärken,
- Invarianten schützen,
- Strukturwissen verbergen.

Aber:

> Nicht jede Datenabfrage muss in Verhalten umgewandelt werden.

Read Models, Reporting und Query-Anwendungen haben andere Bedürfnisse.

---

## 34. Query-Modelle sind kein Demeter-Verstoß per Definition

Eine reine Projektion wie:

~~~java
record CaseSummary(
        CaseId id,
        String applicantName,
        String municipalityCode,
        CaseStatus status) {}
~~~

ist bewusst für Lesen optimiert.

Hier kann direkte Feldnutzung sinnvoll sein.

Die relevante Grenze liegt zwischen:

~~~text
Domain Object mit geschützter interner Struktur
und
explizitem Read Model als Vertrag
~~~

Law of Demeter darf nicht dazu führen, dass Query-Code künstlich durch Delegationsketten geschickt wird.

---

## 35. Delegationsmethoden können selbst Komplexität erzeugen

Übertriebene Anwendung:

~~~text
caseFile.applicantStreet()
caseFile.applicantPostalCode()
caseFile.applicantCountry()
caseFile.applicantMunicipalityName()
~~~

kann das Root-Objekt zu einer riesigen Durchreiche-Schnittstelle machen.

Dann wurde Kopplung nicht beseitigt, sondern verschoben.

---

## 36. Law of Demeter und fachliche Ownership

Eine bessere Frage als "Wie viele Punkte sind im Ausdruck?" lautet:

> Wer besitzt diese Information und wer darf ihre Struktur kennen?

Beispiel:

- Address besitzt Adresslogik.
- Applicant kennt seine Adresse.
- CaseFile muss nicht zwingend jedes Detail der Adresse exponieren.

Diese Betrachtung verbindet Law of Demeter mit Information Hiding.

---

## 37. Empirische Grenzen und False Positives

Die Law of Demeter ist eine Heuristik, kein syntaktisch perfektes Qualitätsgesetz.

Eine explorative Untersuchung am Framework JHotDraw analysierte viele potenzielle Law-of-Demeter-Verletzungen und fand einen sehr hohen Anteil kontextabhängiger False Positives.

Die wichtige Lehre daraus ist nicht:

> Law of Demeter ist nutzlos.

Sondern:

> Syntaktische Smell Detection kann Designabsicht nicht vollständig verstehen.

Deshalb sollte eine Warnung eine Reviewfrage auslösen, nicht automatisch ein Refactoring.

---

## 38. Übertragung auf Service- und Systemgrenzen

Die Grundidee lässt sich analog auf Systeme übertragen.

Problematisch:

~~~text
System A
kennt interne Tabellenstruktur von System B
~~~

oder:

~~~text
System A
kennt interne Workflow-Zustände von System B,
obwohl sie nicht Teil eines stabilen Vertrags sind
~~~

Besser ist häufig:

~~~text
System A
→ expliziter Vertrag von System B
~~~

Damit wird Wissen über fremde Implementierungsdetails reduziert.

Aber auch hier gilt:

> Dies ist eine Architektur-Analogie, keine direkte Anwendung der objektorientierten Law of Demeter.

---

## 39. Demeter-Prüffragen

- Welche fremden internen Strukturen kennt dieser Consumer?
- Sind diese Details Teil eines stabilen Vertrags?
- Wer besitzt die Information?
- Wird Verhalten aus einem Owner herausgezogen?
- Würde eine explizite Query-Schnittstelle die Kopplung klarer machen?
- Erzeugen Delegationsmethoden mehr Nutzen als neue Indirektion?
- Ist der vermeintliche Verstoß nur ein syntaktisches Signal?

---

# Teil VI — Die Prinzipien stehen in Spannung

## 40. KISS versus DRY

DRY kann zu einer gemeinsamen Abstraktion führen.

KISS kann dagegen nahelegen, zwei kleine Implementierungen getrennt zu lassen.

~~~text
2 ähnliche Codeblöcke

Option A:
dupliziert lassen
→ lokal verständlich
→ unabhängig änderbar

Option B:
gemeinsame Abstraktion
→ weniger Wiederholung
→ zusätzliche Indirektion
→ gekoppelte Evolution
~~~

Die richtige Entscheidung hängt davon ab, ob dieselbe Wissensquelle vorliegt.

---

## 41. YAGNI versus Erweiterbarkeit

Eine Architektur kann auf Erweiterbarkeit optimiert werden.

Aber jede Erweiterungsachse kostet.

> Erweiterbarkeit ohne realen Variationspunkt kann YAGNI verletzen.

Gleichzeitig:

> YAGNI darf nicht dazu führen, bekannte Änderungsanforderungen bewusst zu ignorieren.

---

## 42. KISS versus Security und Compliance

Ein direkter technischer Weg kann einfacher aussehen.

Wenn zusätzliche Mechanismen notwendig sind, um Rechte, Audit, Ownership oder Schutzbedarf zu kontrollieren, sind sie nicht automatisch unnötige Komplexität.

---

## 43. DRY versus autonome Domänen

Zentrale Wiederverwendung kann lokale Redundanz reduzieren.

Sie kann aber organisatorische Kopplung erhöhen.

Technisch DRY kann organisatorisch problematisch sein.

---

## 44. Demeter versus API-Explosion

Sehr strikte Abschirmung kann hunderte Mikroschnittstellen und Delegationsmethoden erzeugen.

Das reduziert möglicherweise strukturelle Kenntnis, erhöht aber:

- Interface-Anzahl,
- Navigation,
- Testaufwand,
- Mapping.

Deshalb muss immer die Gesamtkostenbetrachtung gelten.

---

# Teil VII — Durchgängiger Praxisfall

## 45. Ausgangslage: digitale Antragsbearbeitung

Angenommen, eine Anwendung verarbeitet einen Verwaltungsantrag.

~~~java
public final class ApplicationProcessor {

    public Decision process(Application application) {

        int age = application
                .getApplicant()
                .getPersonalData()
                .getAge();

        if (age < 18) {
            return Decision.rejected("age");
        }

        if (application.getAttachments().isEmpty()) {
            return Decision.rejected("attachments");
        }

        ExternalRegistryResponse registry =
                registrySdk.query(application.getApplicant().getId());

        repository.save(application);
        mailClient.send(application.getApplicant().getEmail());

        return Decision.approved();
    }
}
~~~

Der Code funktioniert möglicherweise.

Trotzdem enthält er mehrere Fragen:

- Welche Komplexität ist fachlich notwendig?
- Welche wurde durch Strukturentscheidungen erzeugt?
- Welche Regeln werden mehrfach repräsentiert?
- Welche Details kennt der Processor unnötig?
- Welche Zukunftsfähigkeit brauchen wir wirklich?

---

## 46. Schritt 1 — notwendige Komplexität identifizieren

Fachlich notwendig könnten sein:

- Altersregel,
- Pflichtunterlagen,
- Registerprüfung,
- Persistenz,
- Benachrichtigung.

Nicht automatisch notwendig:

- generische Rule Engine,
- Event Bus,
- Plugin Framework,
- mehrere Storage Provider.

Damit wird zuerst verhindert, dass technische Lösungsideen das Problem vergrößern.

---

## 47. Schritt 2 — KISS: expliziten Use Case erhalten

~~~java
public final class ProcessApplication {

    private final EligibilityPolicy eligibility;
    private final RegistryCheck registry;
    private final ApplicationRepository repository;
    private final ApplicantNotification notification;

    public Decision execute(Application application) {

        Decision eligibilityDecision = eligibility.evaluate(application);

        if (eligibilityDecision.isRejected()) {
            return eligibilityDecision;
        }

        RegistryResult registryResult = registry.check(application.applicantId());

        if (!registryResult.isAcceptable()) {
            return Decision.rejected("registry");
        }

        repository.save(application);
        notification.sendApproved(application);

        return Decision.approved();
    }
}
~~~

Der Ablauf bleibt sichtbar.

Es wurde nicht versucht, die gesamte Orchestrierung in eine generische Pipeline zu verwandeln.

---

## 48. Schritt 3 — YAGNI: kein universelles Workflow-Framework

Man könnte jetzt sagen:

> Wir brauchen sicher bald 50 Prozessschritte. Bauen wir einen generischen Workflow Kernel.

Ohne realen Bedarf wäre das spekulativ.

Stattdessen bleibt der Prozess explizit, bis tatsächliche Anforderungen wie dynamische Prozesskonfiguration, lange laufende Prozesse, menschliche Tasks, Wiederaufnahme oder versionierte Prozessmodelle eine Workflow Engine rechtfertigen.

---

## 49. Schritt 4 — DRY: Altersregel fachlich prüfen

Angenommen, dieselbe Altersgrenze existiert zusätzlich in einem anderen Verfahren.

Nicht sofort abstrahieren.

Zuerst fragen:

- gleiche Rechtsgrundlage?
- gleicher fachlicher Owner?
- gleiche Semantik?
- gemeinsame Änderung?

Wenn ja, kann eine gemeinsame Policy sinnvoll sein.

Wenn nein, bleiben zwei Regeln trotz identischem Zahlenwert getrennt.

---

## 50. Schritt 5 — Demeter: Strukturwissen reduzieren

Vorher:

~~~java
application
    .getApplicant()
    .getPersonalData()
    .getAge();
~~~

Mögliche Alternative:

~~~java
application.applicant().age();
~~~

oder fachlicher:

~~~java
eligibility.evaluate(application);
~~~

Welche Variante besser ist, hängt davon ab, wer die Regel besitzt.

Das Ziel ist nicht weniger Punkte.

Das Ziel ist weniger unnötiges Wissen über fremde interne Strukturen.

---

## 51. Schritt 6 — technische Grenze bewusst setzen

Der externe Register-SDK-Typ sollte nicht die Fachlogik dominieren.

~~~java
public interface RegistryCheck {
    RegistryResult check(PersonId personId);
}
~~~

Das ist kein YAGNI-Verstoß, wenn ein realer externer Integrationspunkt existiert.

Die Abstraktion schützt eine vorhandene technische Grenze.

---

## 52. Ergebnis des Praxisfalls

~~~text
ProcessApplication
      |
      +--> EligibilityPolicy
      |
      +--> RegistryCheck
      |
      +--> ApplicationRepository
      |
      +--> ApplicantNotification
~~~

Keine generische Rule Engine.

Kein Workflow Framework.

Kein Event Bus nur zur Entkopplung.

Keine zentrale Altersabstraktion ohne fachliche Evidenz.

Die Struktur enthält trotzdem klare Grenzen für reale Verantwortungen.

> **Einfachheit entsteht nicht durch Weglassen von Struktur, sondern durch begründete Struktur.**

---

# Teil VIII — Übertragung auf Architektur

## 53. Keine verteilte Architektur ohne verteilten Treiber

Ein oft hilfreicher Architekturmerksatz lautet:

> Keine verteilte Lösung ohne ein Problem, das die Verteilung rechtfertigt.

Microservices können sinnvoll sein bei:

- unabhängigen Releasezyklen,
- klarer Ownership,
- unterschiedlichen Skalierungsprofilen,
- notwendigen Sicherheits- oder Isolationsgrenzen,
- organisatorisch unabhängigen Domänen.

Verteilung erzeugt zusätzliche Komplexität:

- Netzwerk,
- Konsistenz,
- Observability,
- Deployment,
- Incident Response,
- Contract Evolution.

---

## 54. YAGNI auf Plattformebene

Plattformteams stehen häufig vor Zukunftsannahmen:

~~~text
"Später brauchen alle Teams..."
~~~

Deshalb sollte jede neue Plattformfähigkeit mindestens beantworten:

- Welche Teams brauchen sie heute?
- Welches wiederkehrende Problem wird gelöst?
- Welche Variationen werden vereinheitlicht?
- Welche laufenden Betriebskosten entstehen?

Eine Plattform kann Komplexität reduzieren.

Eine ungenutzte Plattform kann selbst zu einer neuen Komplexitätsquelle werden.

---

## 55. DRY auf Unternehmensebene

Unternehmensweit gilt DRY nicht als:

> Jede Fähigkeit darf nur einmal implementiert werden.

Ein sinnvollerer Blick:

> Welche Informationen, Policies oder Standards brauchen eine führende Quelle?

Sinnvoll zentral können beispielsweise technische Security Baselines, API-Konventionen oder verbindliche Klassifikationen sein.

Nicht automatisch zentral gehören fachliche Regeln unterschiedlicher Domänen, lokale Prozesslogik oder unabhängige Datenmodelle.

---

## 56. Law of Demeter und Ownership

Auf Enterprise-Ebene ist eine verwandte Frage:

> Wie viel muss Organisation A über die internen Prozesse und Systeme von Organisation B wissen?

Stabile Verträge reduzieren diese Wissenskopplung.

Das kann APIs, Events, Datenprodukte, Servicekataloge und Verantwortungsmodelle betreffen.

---

## 57. Behördenkontext: Einfachheit unter Rahmenbedingungen

In Behörden darf einfach nicht mit technisch minimal verwechselt werden.

Reale Architekturtreiber können sein:

- gesetzliche Nachvollziehbarkeit,
- Akten- und Aufbewahrungspflichten,
- Datenschutz,
- Informationssicherheit,
- föderale Zuständigkeiten,
- externe Register,
- Vergabe- und Lieferantenabhängigkeiten,
- Barrierefreiheit,
- Notbetrieb,
- lange Lebenszyklen.

Diese Faktoren sind Teil des Problems.

Sie dürfen nicht aus dem Architekturmodell entfernt werden, nur weil sie technische Lösungen größer machen.

Die Aufgabe lautet vielmehr:

> Diese notwendige Komplexität so explizit und lokal wie möglich zu organisieren.

---

# Teil IX — Messbarkeit und Evidence

## 58. Kann Einfachheit gemessen werden?

Nicht direkt mit einer einzelnen Zahl.

Mögliche Signale sind:

- Cyclomatic Complexity,
- Cognitive Complexity,
- Anzahl Abhängigkeiten,
- Dependency Cycles,
- Code Clone Reports,
- Change Coupling,
- Anzahl betroffener Module pro Änderung,
- Build- und Deployment-Abhängigkeiten.

Aber:

~~~text
Metrik
≠
Designurteil
~~~

---

## 59. Cognitive Complexity

Empirische Forschung zu Cognitive Complexity zeigt Zusammenhänge mit Teilen von Codeverständlichkeit, insbesondere mit Verständlichkeitsbewertungen und Bearbeitungszeit.

Andere Aspekte sind weniger eindeutig.

Daraus folgt:

> Cognitive Complexity kann ein Signal sein, aber kein automatischer Beweis für Verständlichkeit.

---

## 60. Change Coupling

Repository-Historie kann zeigen, welche Dateien oder Module häufig gemeinsam geändert werden.

Wenn zwei vermeintlich unabhängige Komponenten fast immer gemeinsam angepasst werden müssen, ist das ein Hinweis auf versteckte Kopplung.

Umgekehrt kann langfristig unabhängige Änderung ähnlicher Codebereiche ein Argument gegen eine gemeinsame Abstraktion sein.

---

## 61. Clone Detection

Clone Detection kann helfen, potenzielle Duplikation zu finden.

Danach beginnt die fachliche Prüfung:

~~~text
Clone gefunden
      ↓
gleiches Wissen?
      ↓
gleicher Änderungsgrund?
      ↓
gemeinsame Abstraktion sinnvoll?
~~~

Nicht:

~~~text
Clone gefunden
→ automatisch extrahieren
~~~

---

## 62. Dependency Analysis

Werkzeuge wie ArchUnit, Modulgraphen, Build Dependency Reports oder Static Analysis können strukturelle Kopplung sichtbar machen.

Sie können aber nicht selbst entscheiden, welche Abhängigkeit fachlich richtig ist.

---

## 63. Evidence für Architekturvereinfachung

Eine behauptete Vereinfachung sollte möglichst konkret werden.

Beispiele:

- Änderung betrifft statt sieben nur zwei Module.
- gleiche Policy wird nur noch an einer autoritativen Stelle gepflegt.
- ein Consumer kennt keine interne Datenstruktur des Providers mehr.
- eine ungenutzte Abstraktionsschicht wurde entfernt.
- ein Providerwechsel verändert die Domänenlogik nicht.
- eine Deployment-Abhängigkeit entfällt.

Das ist belastbarer als:

> Der Code ist jetzt cleaner.

---

# Teil X — Praktisches Reviewverfahren

## 64. Schritt 1 — Änderungsszenario formulieren

Nicht allgemein über Komplexität diskutieren.

Beispiel:

> Die Altersgrenze ändert sich für Leistung A.

oder:

> Ein zweiter Registerprovider soll integriert werden.

---

## 65. Schritt 2 — notwendige Komplexität markieren

Welche Teile folgen aus Fachlichkeit, Gesetz, Security, Betrieb oder Integration?

Diese dürfen nicht einfach entfernt werden.

---

## 66. Schritt 3 — selbst erzeugte Komplexität suchen

Für jedes Element fragen:

> Welche reale Anforderung rechtfertigt es?

---

## 67. Schritt 4 — Wissensduplikation identifizieren

- Welche Regel existiert mehrfach?
- Welche Darstellung ist führend?
- Können Repräsentationen auseinanderlaufen?

---

## 68. Schritt 5 — zukünftige Fähigkeiten klassifizieren

~~~text
verbindlich beschlossen
wahrscheinlich
plausibel
rein hypothetisch
~~~

Je unsicherer der Bedarf, desto höher sollte die Schwelle für Vorabimplementierung sein.

---

## 69. Schritt 6 — Wissenskopplung prüfen

- Welche internen Details kennt der Consumer?
- Sind sie Vertrag oder Implementierungsdetail?
- Welche Änderungen propagieren dadurch?

---

## 70. Schritt 7 — Abstraktion bewerten

Eine Abstraktion sollte mindestens einen klaren Nutzen liefern:

- gleiche Wissensquelle,
- reale Variante,
- volatile Grenze,
- stabile Policy,
- notwendige Isolation.

---

## 71. Schritt 8 — Gesamtkosten betrachten

Nicht nur Codezeilen.

Berücksichtigen:

- Entwicklung,
- Test,
- Betrieb,
- Incident Response,
- Migration,
- Schulung,
- Ownership,
- Governance.

---

## 72. Schritt 9 — Alternative nichts abstrahieren prüfen

Eine ernsthafte Architekturentscheidung braucht auch diese Option.

Manchmal sind zwei einfache Implementierungen besser als ein generisches Framework.

---

## 73. Schritt 10 — Evidence definieren

Wie erkennen wir später, ob die Entscheidung funktioniert?

Beispiele:

- weniger Change Coupling,
- weniger inkonsistente Duplikation,
- kürzere Reviewwege,
- weniger Providerwissen in der Domäne,
- geringere Anzahl notwendiger Anpassungspunkte.

---

# Teil XI — Typische Fehlanwendungen

## 74. KISS = wenig Code

Falsch.

Kurzer, impliziter Code kann schwer verständlich sein.

## 75. KISS = keine Architektur

Falsch.

Explizite Grenzen können Komplexität reduzieren.

## 76. DRY = keine zwei gleichen Zeilen

Falsch.

DRY betrifft primär Wissensduplikation.

## 77. Shared Library = automatisch DRY

Falsch.

Eine Shared Library kann neue technische und organisatorische Kopplung erzeugen.

## 78. YAGNI = Qualität später

Falsch.

Bekannte Qualitätsanforderungen sind heutige Anforderungen.

## 79. YAGNI = keine Vorbereitung

Falsch.

Reversible Grenzen können sinnvoll sein, ohne eine hypothetische Fähigkeit vollständig zu bauen.

## 80. Law of Demeter = keine Getter-Ketten

Zu oberflächlich.

Getter-Ketten können ein Signal für Strukturwissen sein, aber syntaktische Form allein reicht für die Beurteilung nicht.

## 81. Jede Demeter-Warnung muss behoben werden

Falsch.

Kontext und Designabsicht entscheiden.

## 82. Zentralisierung reduziert immer Komplexität

Falsch.

Zentralisierung kann Koordination, Releaseabhängigkeit und Ownership-Konflikte erhöhen.

## 83. Generisch = wiederverwendbar = besser

Falsch.

Generizität erzeugt eigene Konzepte und Erweiterungspunkte.

Wiederverwendung ist nur wertvoll, wenn die gemeinsame Abstraktion stabiler ist als ihre Consumer.

---

# Teil XII — Review-Checklisten

## 84. KISS

- Welche Komplexität ist fachlich unvermeidbar?
- Welche haben wir selbst erzeugt?
- Kann jeder Baustein mit einer realen Anforderung begründet werden?
- Ist der Kontrollfluss nachvollziehbar?
- Wird Komplexität nur in Frameworks oder Konfiguration verschoben?
- Ist die Lösung lokal oder systemweit einfacher?

## 85. YAGNI

- Welche Fähigkeit wird heute benötigt?
- Welche nur hypothetisch?
- Welche spätere Änderung wäre wirklich teuer?
- Können wir eine Grenze schaffen, ohne die Zukunft vorwegzunehmen?
- Welche laufenden Kosten erzeugt eine ungenutzte Fähigkeit?

## 86. DRY

- Ist dies dieselbe Form oder dasselbe Wissen?
- Haben die Stellen denselben Owner?
- Ändern sie sich aus demselben Grund?
- Gibt es eine führende Quelle?
- Ist bewusste Replikation dokumentiert?
- Erzeugt Zentralisierung neue Kopplung?

## 87. Law of Demeter

- Welche fremden Strukturen kennt der Consumer?
- Sind diese Details Teil des Vertrags?
- Wer besitzt die Information?
- Ist eine Query-Projektion sinnvoller?
- Würde eine Delegation Klarheit schaffen oder nur Indirektion?
- Handelt es sich um einen echten Design-Smell oder einen syntaktischen False Positive?

---

# Teil XIII — Woran erkennt man ein tragfähiges Design?

## 88. Eigenschaften

Ein tragfähiges Design ist nicht maximal abstrahiert und nicht maximal minimalistisch.

Es zeigt typischerweise:

- verständliche Verantwortungen,
- explizite zentrale Regeln,
- wenige unnötige Konzepte,
- begründete Abstraktionen,
- klare Ownership,
- kontrollierte Wissensverteilung,
- nachvollziehbaren Kontrollfluss,
- bewusst behandelte Duplikation,
- sinnvolle Reversibilität,
- messbare oder beobachtbare Änderungsgrenzen.

Die Balance lautet:

~~~text
notwendige Fachlichkeit
+
notwendige Qualitätsmerkmale
+
notwendige technische Mechanismen
+
so wenig zusätzliche Struktur wie sinnvoll
~~~

---

# Teil XIV — Glossar

## Accidental Complexity

Komplexität, die aus Repräsentation, Werkzeugen oder Designentscheidungen entsteht und nicht unmittelbar zur eigentlichen Problemstruktur gehört.

## Essential Complexity

Komplexität, die in der Natur beziehungsweise Struktur des zu lösenden Problems liegt. Die Trennung zu accidental complexity ist ein nützliches Denkmodell, aber nicht in jedem System eindeutig.

## Abstraktion

Bewusste Repräsentation relevanter Eigenschaften unter Ausblendung anderer Details.

## Change Coupling

Beobachtung, dass Artefakte in der Entwicklung häufig gemeinsam geändert werden.

## Code Clone

Codeabschnitt mit hoher struktureller oder textueller Ähnlichkeit zu einem anderen Abschnitt.

## Cognitive Complexity

Metrik, die versucht, Aspekte der kognitiven Schwierigkeit von Kontrollflussstrukturen abzubilden.

## DRY

Don't Repeat Yourself. Prinzip zur Vermeidung mehrfach unabhängiger Repräsentationen desselben Wissens.

## Information Hiding

Entwurfsprinzip, nach dem Module Designentscheidungen verbergen, die sich unabhängig ändern können.

## KISS

Heuristik, unnötige Komplexität zu vermeiden und die einfachste tragfähige Lösung zu wählen.

## Law of Demeter

Heuristik zur Begrenzung von Wissen über interne Strukturen fremder Objekte beziehungsweise Collaborators.

## Source of Truth

Autoritative Repräsentation einer Information oder Regel.

## YAGNI

You Aren't Gonna Need It. Heuristik gegen Vorabimplementierung hypothetischer Fähigkeiten.

---

# Teil XV — Quellen und weiterführende Literatur

## 89. David L. Parnas — Modularisierung und Information Hiding

David L. Parnas, *On the Criteria to Be Used in Decomposing Systems into Modules*, Communications of the ACM, 15(12), 1972, S. 1053–1058.

https://doi.org/10.1145/361598.361623

Bedeutung für dieses Dokument:

- Modularisierung nach Änderungsgründen,
- Information Hiding,
- Flexibilität und Verständlichkeit.

---

## 90. Frederick P. Brooks Jr. — Komplexität

Frederick P. Brooks Jr., *No Silver Bullet: Essence and Accidents of Software Engineering*, Computer, 20(4), 1987, S. 10–19.

https://doi.org/10.1109/MC.1987.1663532

Bedeutung:

- Unterscheidung notwendiger und zusätzlicher Schwierigkeiten,
- Softwarekomplexität als grundlegende Designherausforderung.

---

## 91. Andrew Hunt / David Thomas — DRY

Andrew Hunt, David Thomas, *The Pragmatic Programmer*.

Offizielle Pragmatic Programmer Tips:

https://pragprog.com/tips/

Relevant insbesondere für DRY als Prinzip einer eindeutigen, autoritativen Wissensrepräsentation.

---

## 92. Martin Fowler — YAGNI

Martin Fowler, *Yagni*, 2015.

https://martinfowler.com/bliki/Yagni.html

Bedeutung:

- Herkunft im Extreme-Programming-Umfeld,
- Abgrenzung von presumptive features,
- Verbindung zu Simple beziehungsweise Incremental Design.

---

## 93. Lieberherr, Holland, Riel — Law of Demeter

Karl J. Lieberherr, Ian M. Holland, Arthur J. Riel, *Object-Oriented Programming: An Objective Sense of Style*, OOPSLA 1988.

https://doi.org/10.1145/62084.62113

Bedeutung:

- ursprüngliche Formulierung der Law of Demeter,
- Encapsulation,
- begrenzte Kopplung,
- schmalere Interfaces.

---

## 94. Jürgens et al. — Code Clones

Elmar Jürgens, Florian Deissenboeck, Benjamin Hummel, Stefan Wagner, *Do Code Clones Matter?*, ICSE 2009, S. 485–495.

https://doi.org/10.1109/ICSE.2009.5070547

Bedeutung:

- empirische Untersuchung inkonsistenter Änderungen an Code Clones,
- reale Fehlerwirkung bestimmter Duplikationsformen.

Diese Arbeit beweist nicht, dass jede Duplikation schädlich ist.

---

## 95. Muñoz Barón, Wyrich, Wagner — Cognitive Complexity

Marvin Muñoz Barón, Marvin Wyrich, Stefan Wagner, *An Empirical Validation of Cognitive Complexity as a Measure of Source Code Understandability*, ESEM 2020.

https://doi.org/10.1145/3382494.3410636

Bedeutung:

- empirische Untersuchung der Beziehung zwischen Cognitive Complexity und Aspekten von Codeverständlichkeit,
- zugleich Hinweis auf Grenzen einzelner Metriken.

---

## 96. Speicher — Kontextabhängigkeit von Smell Detection

Daniel Speicher, *Did JHotDraw Respect the Law of Good Style? A deep dive into the nature of false positives of bad code smells*, The Art, Science, and Engineering of Programming, Vol. 4.

https://arxiv.org/abs/2002.06191

Bedeutung:

- Law-of-Demeter-Warnungen können stark kontextabhängig sein,
- Designabsichten sind für die Bewertung von Smells relevant,
- automatische Regeln dürfen Review nicht ersetzen.

---

## 97. Verhältnis zu anderen Knowledge Items

- AK-025 — SOLID: Änderbarkeit, Verantwortung und Abhängigkeitsdesign
- AK-084 — Kopplung, Kohäsion und Information Hiding
- AK-027 — Code Smells und Refactoring
- AK-028 — Java Design Patterns problemorientiert einsetzen
- AK-031 — Hexagonal Architecture / Ports & Adapters
- AK-090 — Evolutionary Architecture

---

# 98. Merksatz

> Einfachheit bedeutet nicht, notwendige Architektur wegzulassen. Ein tragfähiges Design macht **notwendige Komplexität sichtbar**, entfernt **unbegründete Komplexität**, hält **Wissen an klaren Quellen**, baut **keine hypothetische Zukunft vorab** und begrenzt, wie viel ein Teil des Systems über die internen Strukturen anderer Teile wissen muss.