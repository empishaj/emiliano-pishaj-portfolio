---
id: AK-025
legacy_ids:
  - QG-JAVA-025
title: SOLID — Änderbarkeit, Verantwortung und Abhängigkeitsdesign
artifact_type: architecture-principle
domain: software-design
status: active
maturity: reviewed
normative_level: recommended
last_validated: 2026-10-01
review_trigger:
  - grundlegende Änderung der Design- oder Modularity-Standards
  - neue belastbare empirische Forschung zu Kopplung, Kohäsion oder Änderbarkeit
---

# AK-025 — SOLID: Änderbarkeit, Verantwortung und Abhängigkeitsdesign

## 1. Zweck dieses Dokuments

SOLID wird häufig als Liste von fünf Regeln gelehrt. Das ist für die Praxis zu wenig.

Die fünf Prinzipien adressieren unterschiedliche Fragen rund um **Verantwortung, Erweiterbarkeit, Verträge, Abhängigkeiten und Änderbarkeit**. Sie sind weder Naturgesetze noch ein automatischer Qualitätsnachweis. Ein System kann alle bekannten SOLID-Formeln äußerlich erfüllen und trotzdem schwer verständlich, teuer in der Änderung oder architektonisch unpassend sein.

Dieses Dokument behandelt SOLID deshalb als **Satz von Designheuristiken**, die bei konkreten Strukturproblemen helfen können.

Die zentrale Frage lautet nicht:

> Ist dieser Code SOLID?

Sondern:

> Welche Struktur erlaubt es, wahrscheinliche Änderungen lokal, verständlich und mit kontrollierten Auswirkungen durchzuführen?

SOLID ist damit kein Zielbild aus Interfaces, Patterns oder Schichten. Es ist ein Denkrahmen für Situationen, in denen Verantwortungen, Verträge oder Abhängigkeiten beginnen, Änderungskosten unnötig zu erhöhen.

---

## 2. Ausgangsproblem: Warum Software trotz korrekter Funktion schwer änderbar werden kann

Software kann funktional korrekt sein und trotzdem ein schlechtes Änderungsverhalten besitzen.

Typische Symptome sind:

- eine kleine fachliche Änderung betrifft viele technisch unabhängige Klassen,
- eine Framework- oder Datenbankänderung zwingt Änderungen in fachlicher Logik,
- neue Varianten führen zu immer längeren `if`-/`switch`-Ketten,
- eine Implementierung kann nur ersetzt werden, wenn viele Aufrufer angepasst werden,
- Interfaces enthalten Methoden, die einzelne Consumer gar nicht benötigen,
- Subtypen erfüllen denselben Java-Typ, aber nicht dieselben Verhaltenserwartungen,
- Unit Tests benötigen große Mengen Infrastruktur, weil fachliche Logik technische Details direkt kennt,
- ein vermeintlich lokales Refactoring führt zu unerwarteten Seiteneffekten in anderen Modulen.

Diese Probleme haben unterschiedliche Ursachen. Häufig geht es um eine oder mehrere der folgenden Eigenschaften:

```text
unklare Verantwortung
+ hohe Kopplung
+ geringe Kohäsion
+ instabile Verträge
+ ungünstige Abhängigkeitsrichtung
+ spekulative oder fehlende Abstraktion
= hohe Änderungskosten
```

SOLID liefert dafür fünf unterschiedliche Perspektiven.

---

# Teil I — Fachliche und wissenschaftliche Einordnung

## 3. Historische Einordnung

SOLID ist keine einheitliche wissenschaftliche Theorie, die zu einem bestimmten Zeitpunkt vollständig formuliert wurde. Die fünf Prinzipien stammen aus unterschiedlichen Entwicklungslinien des objektorientierten Softwaredesigns.

Wichtig ist deshalb eine saubere Trennung zwischen:

- formalen Konzepten,
- etablierten Entwurfsprinzipien,
- praxisorientierten Heuristiken,
- empirisch untersuchten Softwareeigenschaften.

### 3.1 Robert C. Martin und die Bündelung der Prinzipien

Robert C. Martin beschrieb und popularisierte mehrere objektorientierte Designprinzipien in Artikeln und späterer Fachliteratur. In *Design Principles and Design Patterns* werden unter anderem Open/Closed, Liskov Substitution, Dependency Inversion und Interface Segregation als Designprinzipien im Zusammenhang mit Softwaredesign und Abhängigkeiten diskutiert.

Quelle:

- Robert C. Martin, *Design Principles and Design Patterns*, 2000  
  https://objectmentor.com/resources/articles/Principles_and_Patterns.pdf

Die heutige SOLID-Zusammenfassung ist in der Praxis nützlich, sollte aber nicht mit einer formalen Theorie verwechselt werden.

### 3.2 Bertrand Meyer und das Open/Closed Principle

Bertrand Meyer formulierte das Open/Closed Principle im Kontext objektorientierter Softwarekonstruktion. Ein Modul soll für die Nutzung stabil beziehungsweise „geschlossen“ sein und gleichzeitig auf geeignete Weise erweiterbar bleiben.

Quelle:

- Bertrand Meyer, *Object-Oriented Software Construction*, 2nd Edition  
  https://bertrandmeyer.com/OOSC2/

Die historische Formulierung steht stark im Kontext von Vererbung. In moderner Software muss Erweiterbarkeit jedoch nicht über Klassenvererbung realisiert werden. Komposition, Polymorphie, Konfiguration, Datenmodelle, Plugins oder klar definierte Verträge können geeignetere Mechanismen sein.

### 3.3 Liskov und Wing: Behavioral Subtyping

Das Liskov Substitution Principle hat eine deutlich formalere Grundlage als viele vereinfachte SOLID-Darstellungen.

Barbara Liskov und Jeannette Wing beschreiben Subtyping als **semantische Beziehung**: Ein Subtyp muss sich aus Sicht eines Programms, das den Supertyp erwartet, so verhalten, dass relevante Eigenschaften des Supertyps erhalten bleiben.

Quelle:

- Barbara H. Liskov, Jeannette M. Wing, *A Behavioral Notion of Subtyping*, ACM TOPLAS, 1994  
  https://www.cs.cmu.edu/~wing/publications/LiskovWing94.pdf

Das ist mehr als die Aussage „Unterklassen müssen austauschbar sein“. Entscheidend ist der **Verhaltensvertrag**.

### 3.4 David Parnas: Modularisierung und Information Hiding

SOLID wird verständlicher, wenn man es zusammen mit älteren Arbeiten zu Modularisierung betrachtet.

David Parnas zeigte bereits 1972, dass die Qualität einer Modularisierung wesentlich davon abhängt, **nach welchem Kriterium** ein System in Module zerlegt wird. Besonders relevant ist die Idee, volatile Designentscheidungen hinter stabileren Modulgrenzen zu verbergen.

Quelle:

- David L. Parnas, *On the Criteria to Be Used in Decomposing Systems into Modules*, Communications of the ACM, 1972  
  https://doi.org/10.1145/361598.361623

Diese Denkweise ist eng mit Themen verbunden, die später auch in SOLID auftauchen: Verantwortungsgrenzen, Informationsverbergung und Abhängigkeitskontrolle.

---

## 4. Was Wissenschaft und Empirie über SOLID sagen — und was nicht

Es wäre fachlich falsch zu behaupten:

> SOLID ist wissenschaftlich bewiesen.

Dafür ist der Begriff zu breit und die einzelnen Prinzipien sind zu unterschiedlich.

Empirische Softwareengineering-Forschung untersucht jedoch Eigenschaften wie:

- Kopplung,
- Kohäsion,
- Vererbungstiefe,
- Klassengröße,
- Methodenaufrufe,
- Fehleranfälligkeit,
- Wartbarkeit.

Briand, Wüst, Daly und Porter untersuchten beispielsweise Beziehungen zwischen objektorientierten Designmaßen und Fehleranfälligkeit. Die Ergebnisse zeigen, dass strukturelle Maße durchaus mit Qualitätsmerkmalen zusammenhängen können; gleichzeitig sind solche Beziehungen kontextabhängig und nicht auf einen einzelnen universellen SOLID-Score reduzierbar.

Quelle:

- Lionel C. Briand, Jürgen Wüst, John W. Daly, D. Victor Porter, *Exploring the relationships between design measures and software quality in object-oriented systems*, Journal of Systems and Software 51(3), 2000  
  DOI: 10.1016/S0164-1212(99)00102-8

Für dieses Dokument gilt daher:

```text
Formale Theorie
≠
Designheuristik
≠
empirisches Qualitätsmodell
```

SOLID wird hier als Designheuristik verwendet. Einzelne zugrunde liegende Konzepte besitzen formale oder empirische Grundlagen, aber das Akronym selbst ist kein wissenschaftliches Gütesiegel.

---

# Teil II — Begriffe, die vor SOLID verstanden werden sollten

## 5. Verantwortung

„Responsibility“ ist einer der missverständlichsten Begriffe in SOLID.

Eine Verantwortung ist nicht automatisch:

- eine Methode,
- eine Klasse,
- ein Use Case,
- ein technischer Schritt.

Für Designentscheidungen sind mindestens vier Sichtweisen relevant.

### 5.1 Fachliche Verantwortung

Beispiel:

- Anspruch prüfen,
- Entscheidung treffen,
- Gebühr berechnen.

### 5.2 Technische Verantwortung

Beispiel:

- Daten persistieren,
- PDF rendern,
- Nachricht versenden.

### 5.3 Änderungsverantwortung

Welche Gründe können dazu führen, dass dieselbe Einheit geändert werden muss?

### 5.4 Ownership-Verantwortung

Wer versteht, pflegt und entscheidet über diesen Teil des Systems?

Eine Klasse kann technisch mehrere Methoden besitzen und trotzdem eine kohärente Verantwortung haben. Umgekehrt kann eine kleine Klasse mehrere völlig unabhängige Änderungsgründe enthalten.

---

## 6. Kopplung

Kopplung beschreibt, wie stark Elemente voneinander abhängen.

Sie ist nicht binär. Unterschiedliche Arten von Kopplung haben unterschiedliche Folgen.

### 6.1 Strukturelle Kopplung

Eine Klasse importiert oder referenziert eine andere Klasse.

### 6.2 Datenkopplung

Zwei Komponenten müssen dasselbe Datenmodell oder dieselben Felder kennen.

### 6.3 Semantische Kopplung

Ein Consumer hängt von impliziten Bedeutungen oder Reihenfolgen ab, die nicht sauber im Vertrag ausgedrückt sind.

### 6.4 Zeitliche Kopplung

Operation B muss unmittelbar nach Operation A erfolgen.

### 6.5 Technologische Kopplung

Fachlogik hängt direkt von Frameworks, Datenbanktypen oder Provider-SDKs ab.

### 6.6 Änderungskopplung

Mehrere Elemente werden in der Praxis häufig gemeinsam geändert.

Für SOLID ist besonders interessant:

> Welche Kopplung zwingt uns, bei einer fachlich lokalen Änderung unnötig viele technische Elemente mitzuziehen?

---

## 7. Kohäsion

Kohäsion beschreibt, wie sinnvoll die Elemente innerhalb einer Einheit zusammengehören.

Hohe Kohäsion bedeutet nicht „wenige Methoden“.

Eine kohärente Einheit bündelt Verhalten und Daten, die aus demselben fachlichen oder technischen Grund gemeinsam verstanden und geändert werden.

Beispiel:

```text
EligibilityPolicy
├── prüft Voraussetzungen
├── berechnet fachliches Ergebnis
└── erklärt Ablehnungsgründe
```

Das kann kohärent sein.

Weniger kohärent wäre:

```text
ApplicationManager
├── prüft Voraussetzungen
├── speichert SQL
├── rendert PDF
├── versendet E-Mail
└── erzeugt Statistikexport
```

---

## 8. Vertrag

Ein Vertrag ist mehr als eine Methodensignatur.

Ein Softwarevertrag kann umfassen:

- Eingabetypen,
- erlaubte Werte,
- Vorbedingungen,
- Rückgabewerte,
- Nachbedingungen,
- Invarianten,
- Fehlersemantik,
- Seiteneffekte,
- Reihenfolgegarantien,
- Performance- oder Konsistenzerwartungen.

Das ist besonders wichtig für LSP und ISP.

---

## 9. Stabilität und Volatilität

Nicht alle Teile eines Systems ändern sich mit derselben Wahrscheinlichkeit.

Typisch eher stabil:

- zentrale fachliche Begriffe,
- gesetzlich oder fachlich etablierte Regeln,
- wohldefinierte Geschäftsobjekte.

Typisch eher volatil:

- konkrete Provider,
- Framework-Konfiguration,
- technische Protokolle,
- Storage-Implementierungen,
- externe APIs.

DIP und Information Hiding sind besonders wertvoll, wenn sie stabile fachliche Konzepte vor unnötiger Kopplung an volatile technische Entscheidungen schützen.

---

# Teil III — Die fünf Prinzipien im Detail

# 10. Single Responsibility Principle — SRP

## 10.1 Kurzdefinition

Eine Einheit sollte eine **kohärente Verantwortung** besitzen und nicht unnötig mehrere unabhängige Änderungsgründe miteinander koppeln.

Eine häufig verwendete Kurzform lautet „one reason to change“. Diese Formulierung ist nützlich, wenn „reason“ nicht mechanisch verstanden wird.

SRP bedeutet nicht:

- eine Klasse = eine Methode,
- eine Klasse = ein Use Case,
- jede technische Tätigkeit braucht eine eigene Klasse.

Die bessere Frage lautet:

> Welche unabhängigen Interessen oder Änderungsgründe werden in dieser Einheit miteinander gekoppelt?

---

## 10.2 Problembeispiel

```java
public class ApplicationProcessingService {

    private final DataSource dataSource;
    private final MailClient mailClient;
    private final PdfRenderer pdfRenderer;

    public Decision process(Application application) {
        validate(application);

        Decision decision = evaluateEligibility(application);

        save(application, decision);

        byte[] pdf = pdfRenderer.render(decision);
        mailClient.send(application.email(), pdf);

        writeAuditLog(application, decision);
        return decision;
    }
}
```

Diese Klasse kann funktionieren. Das Problem ist nicht ihre Länge allein.

Sie wird jedoch aus sehr unterschiedlichen Gründen geändert:

```text
fachliche Prüflogik  ─────┐
Persistenzmodell     ─────┤
PDF-Layout           ─────┤
E-Mail-Provider      ─────┼→ gleiche Klasse
Audit-Anforderung    ─────┤
Validierungsregeln   ─────┘
```

Die Änderungskopplung ist das eigentliche Problem.

---

## 10.3 Schrittweise Verbesserung

Nicht sofort jede Codezeile in einen eigenen Service verschieben.

### Schritt 1 — Änderungsgründe identifizieren

- Fachliche Entscheidung
- Persistenz
- Dokumenterstellung
- Benachrichtigung
- Audit

### Schritt 2 — fachlichen Kern sichtbar machen

```java
public final class EligibilityPolicy {

    public Decision evaluate(Application application) {
        // fachliche Regeln
        return Decision.approved();
    }
}
```

### Schritt 3 — technische Seiteneffekte separieren

```java
public final class ApplicationUseCase {

    private final EligibilityPolicy policy;
    private final ApplicationRepository repository;
    private final NotificationPort notifications;

    public Decision process(Application application) {
        Decision decision = policy.evaluate(application);
        repository.save(application, decision);
        notifications.notify(application, decision);
        return decision;
    }
}
```

Die zentrale fachliche Regel kann sich jetzt unabhängig von PDF- oder E-Mail-Technik entwickeln.

---

## 10.4 Wann SRP übertrieben wird

Eine Überreaktion sieht beispielsweise so aus:

```text
ApplicationValidator
ApplicationEligibilityChecker
ApplicationSaver
ApplicationMapper
ApplicationNotifier
ApplicationAuditWriter
ApplicationProcessor
ApplicationProcessorImpl
```

Viele kleine Klassen erhöhen nicht automatisch Kohäsion.

Sie können neue Kosten erzeugen:

- mehr Navigation,
- schwerer nachvollziehbarer Kontrollfluss,
- verteilte Invarianten,
- unnötige Abstraktionsschichten.

SRP optimiert nicht die Anzahl der Klassen, sondern die **Zusammengehörigkeit von Verantwortung**.

---

## 10.5 Architekturübertragung

Die zugrunde liegende Frage skaliert auf höhere Ebenen:

```text
Klasse
→ Modul
→ Komponente
→ Service
```

Beispiel:

Ein Service, der gleichzeitig Identitätsmanagement, Fachfallbearbeitung und Reporting verantwortet, kann ebenso mehrere unabhängige Änderungsachsen koppeln wie eine schlecht geschnittene Klasse.

Aber:

> SRP allein reicht nicht aus, um Servicegrenzen oder Organisationsgrenzen festzulegen.

Dafür sind zusätzlich Domänenmodell, Datenverantwortung, Betriebsanforderungen, Teamstruktur und Transaktionsgrenzen relevant.

---

## 10.6 Reviewfragen zu SRP

- Welche unabhängigen Gründe führen zu Änderungen dieser Einheit?
- Welche Stakeholder oder fachlichen Rollen treiben diese Änderungen?
- Welche Teile ändern sich typischerweise gemeinsam?
- Welche Invarianten müssen gemeinsam geschützt werden?
- Würde eine Trennung Änderungskosten reduzieren oder nur Navigation erhöhen?
- Ist die Einheit fachlich kohärent oder nur technisch praktisch zusammengelegt?

---

# 11. Open/Closed Principle — OCP

## 11.1 Kurzdefinition

Ein Modul soll an sinnvollen Variationspunkten erweiterbar sein, ohne für jede neue Variante seine stabile Kernlogik verändern zu müssen.

Das bedeutet nicht:

> Bestehender Code darf niemals geändert werden.

Es bedeutet:

> Wenn eine reale, wiederkehrende Änderungsachse bekannt ist, kann eine stabile Erweiterungsgrenze sinnvoll sein.

---

## 11.2 Historischer Kontext

Bei Meyer steht Open/Closed im Kontext wiederverwendbarer Module und Vererbung.

Moderne Systeme können dasselbe Ziel über andere Mechanismen erreichen:

- Komposition,
- Strategy,
- Plugin-Architektur,
- Konfiguration,
- Datengetriebene Regeln,
- Dependency Injection,
- Polymorphie,
- Event Handler.

Entscheidend ist nicht der Mechanismus, sondern die Frage:

> Wo existiert eine echte Variationsachse, die wir stabil kapseln möchten?

---

## 11.3 Problembeispiel

```java
public BigDecimal calculateFee(Application application) {
    if (application.type() == Type.STANDARD) {
        return new BigDecimal("100.00");
    }
    if (application.type() == Type.REDUCED) {
        return new BigDecimal("50.00");
    }
    if (application.type() == Type.EXEMPT) {
        return BigDecimal.ZERO;
    }
    throw new IllegalArgumentException("Unknown type");
}
```

Wenn neue Fallarten regelmäßig hinzukommen und dieselbe Verzweigung in vielen Stellen auftaucht, entsteht ein realer Variationspunkt.

---

## 11.4 Mögliche Verbesserung

```java
public interface FeeRule {
    boolean supports(Application application);
    BigDecimal calculate(Application application);
}
```

```java
public final class ReducedFeeRule implements FeeRule {

    @Override
    public boolean supports(Application application) {
        return application.type() == Type.REDUCED;
    }

    @Override
    public BigDecimal calculate(Application application) {
        return new BigDecimal("50.00");
    }
}
```

Der Gewinn entsteht nur dann, wenn Varianten tatsächlich unabhängig wachsen oder gepflegt werden.

---

## 11.5 Gegenbeispiel: spekulative Erweiterbarkeit

Ein System kennt genau einen Algorithmus und es gibt keine realistische zweite Variante.

Trotzdem entstehen:

```text
Calculator
CalculatorImpl
CalculatorFactory
CalculatorProvider
CalculatorRegistry
CalculatorStrategy
```

Das ist kein OCP-Erfolg.

Es ist möglicherweise spekulative Komplexität.

---

## 11.6 Entscheidungskriterien

Eine Erweiterungsgrenze ist eher sinnvoll, wenn mehrere dieser Punkte zutreffen:

- mehrere reale Implementierungen existieren,
- neue Varianten treten wiederholt auf,
- Varianten werden von unterschiedlichen Teams geliefert,
- ein Provider kann ausgetauscht werden,
- Plugins oder Regeln werden unabhängig deployt oder aktiviert,
- Änderungen an Varianten sollen den stabilen Kern möglichst wenig berühren.

Eine Erweiterungsgrenze ist weniger überzeugend, wenn:

- nur eine hypothetische Zukunftsvariante existiert,
- die Domäne selbst noch nicht verstanden ist,
- die Abstraktion mehr Sonderfälle als Klarheit erzeugt,
- jede „Erweiterung“ trotzdem Änderungen am gemeinsamen Kern benötigt.

---

## 11.7 Reviewfragen zu OCP

- Welche konkrete Änderungsachse soll stabilisiert werden?
- Wie häufig tritt diese Änderung real auf?
- Ist der stabile Kern tatsächlich stabil genug, um abstrahiert zu werden?
- Gibt es heute mindestens zwei relevante Varianten oder starke Evidenz für weitere?
- Entsteht eine echte Erweiterungsgrenze oder nur zusätzliche Indirektion?

---

# 12. Liskov Substitution Principle — LSP

## 12.1 Kurzdefinition

Eine Implementierung oder ein Subtyp darf den erwarteten Vertrag des abstrahierten Typs nicht semantisch brechen.

Die zentrale Frage lautet:

> Kann ein Client, der korrekt gegen den Basistyp programmiert, diese Implementierung verwenden, ohne seine Annahmen über erlaubtes Verhalten ändern zu müssen?

---

## 12.2 Behavioral Subtyping

Liskov und Wing betrachten Subtyping als Verhaltensbeziehung, nicht nur als syntaktische Typbeziehung.

Ein Java-Compiler kann bestätigen:

```text
ReadOnlyArchive implements DocumentStore
```

Der Compiler kann aber nicht automatisch bestätigen, dass `ReadOnlyArchive` denselben semantischen Vertrag erfüllt.

---

## 12.3 Problembeispiel

```java
public interface DocumentStore {
    void save(Document document);
    void delete(DocumentId id);
}
```

```java
public final class ReadOnlyArchive implements DocumentStore {

    @Override
    public void save(Document document) {
        throw new UnsupportedOperationException();
    }

    @Override
    public void delete(DocumentId id) {
        throw new UnsupportedOperationException();
    }
}
```

Syntaktisch ist das korrekt.

Semantisch ist es problematisch, wenn der Vertrag von `DocumentStore` einem Client verspricht, dass Speichern und Löschen grundsätzlich unterstützte Operationen sind.

---

## 12.4 Bessere Modellierung

```java
public interface DocumentReader {
    Optional<Document> find(DocumentId id);
}
```

```java
public interface MutableDocumentStore extends DocumentReader {
    void save(Document document);
    void delete(DocumentId id);
}
```

```java
public final class ReadOnlyArchive implements DocumentReader {
    // nur lesender Vertrag
}
```

Hier wird die tatsächliche Fähigkeit explizit im Typmodell ausgedrückt.

---

## 12.5 Vorbedingungen, Nachbedingungen und Invarianten

In vereinfachten Darstellungen wird LSP oft mit folgenden Regeln erklärt:

- ein Subtyp sollte Vorbedingungen nicht verschärfen,
- Nachbedingungen nicht abschwächen,
- relevante Invarianten erhalten.

Diese Regeln sind nützliche Denkstützen, aber Liskov/Wing behandeln Behavioral Subtyping umfassender als diese Kurzform.

Beispiel einer verschärften Vorbedingung:

Basistyp:

```java
void transfer(Money amount);
```

Vertrag:

```text
amount > 0
```

Subtyp:

```text
amount >= 1000
```

Ein Client, der laut Basistyp legal `100` übergeben darf, würde beim Subtyp scheitern.

---

## 12.6 LSP ist nicht nur Vererbung

Das Prinzip ist auch bei folgenden Strukturen relevant:

- Interface-Implementierungen,
- Adapter,
- Provider-Schnittstellen,
- Repository-Implementierungen,
- API-Versionen,
- alternative Messaging-Clients,
- Test Doubles.

Ein Fake Repository, das sich fundamental anders verhält als die reale Implementierung, kann Tests ebenso irreführen wie ein schlecht entworfener Subtyp.

---

## 12.7 Typische LSP-Verletzungen

- `UnsupportedOperationException` für eigentlich erlaubte Operationen,
- überraschend andere Null-Semantik,
- zusätzliche unerwartete Seiteneffekte,
- abweichende Transaktionssemantik,
- andere Reihenfolgegarantien,
- stilles Ignorieren von Eingaben, die der Basistyp akzeptiert,
- schwächere Sicherheits- oder Konsistenzgarantien.

---

## 12.8 Reviewfragen zu LSP

- Was verspricht der Vertrag semantisch?
- Welche Vorbedingungen erwartet der Client?
- Welche Nachbedingungen gelten?
- Welche Fehlerarten sind Teil des Vertrags?
- Welche Seiteneffekte sind erlaubt?
- Kann jede Implementierung diese Erwartungen erfüllen?
- Müssen einzelne Implementierungen Operationen „wegwerfen“ oder mit `UnsupportedOperationException` ablehnen?

---

# 13. Interface Segregation Principle — ISP

## 13.1 Kurzdefinition

Ein Consumer sollte nicht von Operationen abhängig gemacht werden, die er nicht benötigt.

ISP bedeutet nicht:

> Möglichst viele Einmethoden-Interfaces erzeugen.

Die zentrale Frage lautet:

> Welcher Consumer benötigt welchen fachlich oder technisch kohärenten Vertrag?

---

## 13.2 Problembeispiel

```java
public interface CaseManagement {
    Case create(CreateCaseCommand command);
    Case approve(CaseId id);
    Case reject(CaseId id);
    void archive(CaseId id);
    byte[] exportStatistics();
    void delete(CaseId id);
    Case reopen(CaseId id);
    Case find(CaseId id);
}
```

Ein Reporting-Modul braucht vielleicht ausschließlich:

```java
Case find(CaseId id);
byte[] exportStatistics();
```

Wenn es trotzdem gegen das gesamte Interface gekoppelt ist, hängt es konzeptionell an Fähigkeiten, die für seinen Zweck irrelevant sind.

---

## 13.3 Consumer-orientierter Schnitt

```java
public interface CaseQuery {
    CaseView find(CaseId id);
}
```

```java
public interface CaseStatistics {
    Statistics calculate(Period period);
}
```

```java
public interface CaseCommandService {
    CaseId create(CreateCaseCommand command);
    void approve(CaseId id);
    void reject(CaseId id);
}
```

Die Verträge sind entlang realer Consumer-Bedürfnisse getrennt.

---

## 13.4 Verbindung zu APIs und Modulgrenzen

ISP ist nicht auf Java-Interfaces beschränkt.

Die Grundidee gilt analog für:

- REST-Ressourcen,
- GraphQL-Schemata,
- Modul-APIs,
- Event-Verträge,
- SDKs,
- interne Plattform-Schnittstellen.

Beispiel:

Eine zentrale „Enterprise API“ mit hunderten Operationen kann technisch einen einzigen Endpoint-Katalog bieten, aber fachlich sehr unterschiedliche Consumer koppeln.

---

## 13.5 Gefahr der Übersegregation

Zu viele Mikro-Interfaces können ebenfalls schaden:

```text
CaseLoader
CaseFinder
CaseReader
CaseLookup
CaseByIdProvider
```

Wenn diese Schnittstellen dieselbe fachliche Rolle beschreiben, erzeugt die Trennung keine echte Entkopplung.

---

## 13.6 Reviewfragen zu ISP

- Wer ist der konkrete Consumer?
- Welche Operationen benötigt er wirklich?
- Welche Teile des Vertrags ändern sich aus unterschiedlichen Gründen?
- Erzwingt das Interface Abhängigkeiten auf irrelevante Fähigkeiten?
- Sind getrennte Verträge fachlich verständlicher oder nur kleiner?

---

# 14. Dependency Inversion Principle — DIP

## 14.1 Kurzdefinition

Fachlich zentrale oder langfristig stabile Logik sollte nicht unnötig von volatilen technischen Details abhängen.

Die häufige Verkürzung lautet:

> Depend on abstractions, not concretions.

Das ist nützlich, aber unvollständig.

Denn eine schlechte Abstraktion ist nicht besser als eine konkrete Klasse.

DIP fragt vielmehr:

> Welche Richtung sollen unsere Designabhängigkeiten haben, damit zentrale Policies nicht von austauschbaren Mechanismen beherrscht werden?

---

## 14.2 Unterschiedliche Arten von Dependencies

DIP bezieht sich primär auf Design- beziehungsweise Source-Code-Abhängigkeiten.

Zu unterscheiden sind:

```text
Source Dependency
Runtime Dependency
Deployment Dependency
Data Dependency
Control Flow Dependency
```

Ein System kann zur Laufzeit einen Datenbankserver benötigen, ohne dass seine fachliche Kernlogik direkt von JDBC- oder JPA-Typen abhängen muss.

---

## 14.3 Problembeispiel

```java
public final class EligibilityService {

    private final ExternalCreditSdk sdk;

    public Decision evaluate(Application application) {
        ExternalCreditResponse response = sdk.check(
                application.personId().value());

        if (response.score() < 500) {
            return Decision.rejected("credit-risk");
        }

        return Decision.approved();
    }
}
```

Die fachliche Entscheidung kennt direkt:

- Provider-SDK,
- Provider-Datentyp,
- Provider-Score-Semantik.

Ein Providerwechsel wird dadurch zu einer Änderung im fachlichen Kern.

---

## 14.4 Abhängigkeitsinversion

```java
public interface CreditAgency {
    CreditAssessment assess(PersonId personId);
}
```

```java
public final class EligibilityService {

    private final CreditAgency creditAgency;

    public Decision evaluate(Application application) {
        CreditAssessment assessment =
                creditAgency.assess(application.personId());

        if (assessment.isHighRisk()) {
            return Decision.rejected("credit-risk");
        }

        return Decision.approved();
    }
}
```

Adapter:

```java
public final class ExternalProviderCreditAdapter
        implements CreditAgency {

    private final ExternalCreditSdk sdk;

    @Override
    public CreditAssessment assess(PersonId personId) {
        ExternalCreditResponse response = sdk.check(personId.value());
        return map(response);
    }
}
```

Abhängigkeitsbild:

```text
fachliche Policy
       ↓
CreditAgency
       ↑
Provider Adapter
       ↓
Provider SDK
```

Die fachliche Sprache definiert den Vertrag.

---

## 14.5 Wann DIP keinen Mehrwert erzeugt

Problematisch:

```java
public interface UserService {
    User find(long id);
}
```

```java
public class UserServiceImpl implements UserService {
    // einzige Implementierung
}
```

Wenn das Interface weder fachlichen Vertrag noch volatile Grenze noch Test-Seam noch reale Variante ausdrückt, ist die zusätzliche Ebene möglicherweise nur Ritual.

---

## 14.6 DIP und Hexagonal Architecture

Ports & Adapters nutzt denselben Grundgedanken systematisch:

```text
Domain / Application Core
          ↓
        Ports
          ↑
       Adapters
```

DIP erzwingt jedoch keine vollständige Hexagonal Architecture.

Eine einzelne Abhängigkeitsgrenze kann bereits sinnvoll sein, ohne ein gesamtes System in ein bestimmtes Architekturpattern zu pressen.

---

## 14.7 Reviewfragen zu DIP

- Welche Logik ist fachlich zentral und langfristig stabil?
- Welche technischen Details sind volatil?
- Kennt die Domäne Framework-, Provider- oder Transporttypen?
- Ist die Abstraktion in fachlicher Sprache formuliert?
- Existiert ein realistischer Grund für die Entkopplung?
- Ist die Abhängigkeitsrichtung im Code sichtbar?

---

# Teil IV — Zusammenspiel und Trade-offs

## 15. SOLID wirkt als System, nicht als fünf isolierte Regeln

Die Prinzipien adressieren verschiedene Aspekte desselben Problems.

```text
SRP
→ Verantwortungen und Änderungsgründe erkennen

ISP
→ Consumer-Verträge sinnvoll schneiden

DIP
→ Abhängigkeitsrichtungen kontrollieren

LSP
→ semantische Austauschbarkeit absichern

OCP
→ reale Variationsachsen gezielt stabilisieren
```

Ein typischer Ablauf kann so aussehen:

```text
Änderungsproblem
   ↓
SRP: Welche Verantwortung ist betroffen?
   ↓
ISP: Welcher Consumer braucht welchen Vertrag?
   ↓
DIP: Welche Abhängigkeit sollte umgedreht werden?
   ↓
LSP: Erfüllen alle Implementierungen denselben Vertrag?
   ↓
OCP: Ist hier eine wiederkehrende Variationsachse entstanden?
```

---

## 16. Prinzipien können miteinander in Spannung stehen

Mehr Entkopplung ist nicht kostenlos.

Beispiel:

```text
mehr Interfaces
→ potenziell bessere Entkopplung
→ aber mehr Navigation und Konzepte
```

Mehr OCP kann bedeuten:

```text
mehr Erweiterbarkeit
→ aber abstrakteres Design
```

Mehr SRP-Trennung kann bedeuten:

```text
lokalisiertere Änderungen
→ aber verteilteren Kontrollfluss
```

Die richtige Frage ist daher nicht:

> Wie maximieren wir SOLID?

Sondern:

> Welche Designkosten akzeptieren wir, um welche konkreten Änderungskosten zu reduzieren?

---

# Teil V — Beziehung zu anderen Designprinzipien

## 17. SOLID und KISS

KISS schützt vor übertriebener Abstraktion.

Wenn eine Lösung mit drei klaren Klassen verständlicher ist als eine Lösung aus zwölf Interfaces, Factories und Strategies, ist die kleinere Struktur möglicherweise besser – auch wenn die größere Lösung formaler „SOLID“ aussieht.

---

## 18. SOLID und YAGNI

OCP und DIP sind besonders anfällig für spekulative Architektur.

YAGNI stellt die Gegenfrage:

> Haben wir heute Evidenz für diesen Variationspunkt?

Eine Abstraktion für einen hypothetischen zweiten Provider ist nicht automatisch falsch, aber ihre Kosten müssen gerechtfertigt sein.

---

## 19. SOLID und DRY

Duplikation und Abstraktion sind nicht dasselbe Problem.

Zwei ähnliche Codeblöcke können unterschiedliche fachliche Bedeutungen besitzen.

Eine verfrühte gemeinsame Abstraktion kann später stärkere Kopplung erzeugen als bewusst tolerierte Duplikation.

---

## 20. SOLID und Information Hiding

Parnas' Information-Hiding-Gedanke ergänzt SOLID hervorragend.

Die zentrale Frage lautet:

> Welche Designentscheidung ist wahrscheinlich volatil und sollte deshalb hinter einer stabileren Grenze verborgen werden?

DIP und OCP können als praktische Mechanismen dienen, um genau solche Grenzen zu schaffen.

---

## 21. SOLID und High Cohesion / Low Coupling

Ein sinnvolles übergeordnetes Zielmodell ist:

```text
klare Verantwortung
        ↓
hohe Kohäsion
        +
kontrollierte Kopplung
        ↓
lokalisiertere Änderungen
        ↓
überschaubare Änderungskosten
```

SOLID ist eine Möglichkeit, solche Probleme zu analysieren. Es ist nicht die einzige.

---

# Teil VI — Messbarkeit und empirische Grenzen

## 22. Kann man SOLID messen?

Nicht direkt und nicht verlässlich mit einer einzigen Kennzahl.

Mögliche Strukturmetriken sind beispielsweise:

- Coupling Between Objects,
- afferent/efferent coupling,
- Zyklusanzahl,
- Klassengröße,
- Vererbungstiefe,
- Methodenanzahl,
- Cohesion-Metriken,
- Fan-in/Fan-out.

Zusätzlich können Repository-Daten genutzt werden:

- welche Dateien ändern sich häufig gemeinsam?
- welche Module sind bei kleinen Features regelmäßig gemeinsam betroffen?
- welche Klassen haben hohe Churn-Raten?
- wo konzentrieren sich Defekte?

Aber:

```text
Metrik
≠
Designprinzip
```

Eine große Klasse verletzt nicht automatisch SRP.

Eine kleine Klasse erfüllt SRP nicht automatisch.

Viele Interfaces beweisen kein DIP.

Eine geringe Kopplungsmetrik beweist keine gute fachliche Modularisierung.

---

## 23. Geeignete Evidence für Designqualität

Praktisch wertvoller als ein SOLID-Score sind häufig konkrete Änderungsszenarien.

Beispiel:

> Ein zweiter Auskunftsprovider soll integriert werden.

Prüfung:

- Welche Module müssen geändert werden?
- Berührt die Änderung fachliche Kernlogik?
- Muss der Client Provider-Datentypen kennen?
- Können beide Implementierungen denselben Vertrag erfüllen?
- Welche Tests müssen angepasst werden?

Ein weiteres Szenario:

> Eine neue fachliche Gebührenart kommt hinzu.

Prüfung:

- wächst eine bestehende Verzweigung?
- existiert bereits ein sinnvoller Variationspunkt?
- ist die Änderung lokal?
- entstehen neue Sonderfälle in mehreren Klassen?

---

# Teil VII — Durchgängiger Praxisfall

## 24. Ausgangslage: digitale Antragsbearbeitung

Angenommen, ein System verarbeitet einen Antrag.

Eine erste Implementierung sieht so aus:

```java
public class ApplicationManager {

    public Decision process(Application application) {
        validate(application);

        ExternalResponse response = externalSdk.check(application.personId());

        Decision decision;
        if (application.type() == Type.STANDARD) {
            decision = evaluateStandard(application, response);
        } else if (application.type() == Type.SPECIAL) {
            decision = evaluateSpecial(application, response);
        } else {
            throw new IllegalArgumentException("Unknown type");
        }

        database.save(application, decision);
        pdfRenderer.render(decision);
        mailClient.send(application.email(), decision);

        return decision;
    }
}
```

Die Klasse ist funktional denkbar, aber strukturell problematisch.

---

## 25. Schritt 1 — SRP: Änderungsgründe sichtbar machen

Änderungsachsen:

```text
Validierung
fachliche Entscheidung
externer Auskunftsdienst
Persistenz
Dokumenterstellung
Benachrichtigung
```

Zuerst wird der fachliche Kern extrahiert.

```java
public final class ApplicationDecisionPolicy {

    public Decision evaluate(
            Application application,
            ExternalAssessment assessment) {
        // fachliche Entscheidung
    }
}
```

---

## 26. Schritt 2 — DIP: externen Provider aus der Fachlogik lösen

```java
public interface AssessmentService {
    Assessment assess(PersonId personId);
}
```

Der Provider-Adapter übersetzt technische Providerdaten in ein fachlich verständliches Modell.

```text
Provider SDK
    ↓
Adapter
    ↓
Assessment
    ↓
ApplicationDecisionPolicy
```

---

## 27. Schritt 3 — ISP: Consumer-Verträge schneiden

Die Fachlogik benötigt nur:

```java
Assessment assess(PersonId personId);
```

Sie braucht keine Provider-Administration, Health-Checks oder Cache-Steuerung.

Diese technischen Fähigkeiten gehören nicht in denselben Port.

---

## 28. Schritt 4 — LSP: Austauschbarkeit prüfen

Wenn ein zweiter Provider implementiert wird:

```java
class ProviderAAssessmentAdapter implements AssessmentService
class ProviderBAssessmentAdapter implements AssessmentService
```

müssen beide denselben semantischen Vertrag erfüllen.

Fragen:

- Was bedeutet `Assessment.unavailable()`?
- Darf eine Implementierung `null` liefern?
- Was geschieht bei Timeout?
- Sind Score-Bereiche vergleichbar?
- Welche Datenqualität wird versprochen?

Erst wenn diese Semantik klar ist, existiert echte Substituierbarkeit.

---

## 29. Schritt 5 — OCP: echte Variationsachse erkennen

Nach mehreren fachlichen Fallarten kann sich zeigen, dass Entscheidungsregeln unabhängig wachsen.

Dann kann eine Erweiterungsgrenze sinnvoll werden:

```java
public interface DecisionRule {
    boolean appliesTo(Application application);
    RuleResult evaluate(Application application, Assessment assessment);
}
```

Aber erst jetzt existiert Evidenz für die Abstraktion.

---

## 30. Ergebnis des Praxisfalls

```text
ApplicationUseCase
      ↓
DecisionPolicy
      ↓
fachliche Regeln

ApplicationUseCase
      ↓
AssessmentService ← Provider Adapter

ApplicationUseCase
      ↓
Repository ← Database Adapter

ApplicationUseCase
      ↓
NotificationPort ← Mail Adapter
```

Wichtig:

Das Ergebnis ist nicht automatisch „die richtige Architektur“.

Es ist eine Struktur, deren Abstraktionen jeweils mit einem konkreten Änderungs- oder Abhängigkeitsproblem begründet werden können.

---

# Teil VIII — Übertragung auf Architektur

## 31. Von Klassen zu Modulen und Komponenten

Die zugrunde liegenden Fragen von SOLID sind auch auf höheren Softwareebenen relevant.

| Prinzip | Code-Ebene | mögliche Architektur-Analogie |
|---|---|---|
| SRP | Klasse mit kohärenter Verantwortung | Modul oder Service mit klarer fachlicher Verantwortung |
| OCP | stabiler Extension Point | gezielt erweiterbare Modul-/Provider-Grenze |
| LSP | austauschbarer Subtyp | kompatible Provider oder Adapter |
| ISP | consumer-orientiertes Interface | consumer-orientierte API oder Modul-Schnittstelle |
| DIP | Policy hängt von Abstraktion | fachlicher Kern wird von Technik entkoppelt |

Diese Tabelle beschreibt Analogien, keine mathematische Übertragung der Prinzipien.

---

## 32. SRP auf Modulebene

Ein Modul sollte einen nachvollziehbaren fachlichen oder technischen Zweck besitzen.

Schlechtes Signal:

```text
common/
utils/
manager/
shared/
```

wenn dort nicht zusammengehörige Verantwortungen gesammelt werden.

Besser sind Grenzen, deren Inhalt durch Sprache und Änderungsgründe erklärbar ist.

---

## 33. ISP auf API-Ebene

Eine API, die alle Fähigkeiten einer Plattform in einem einzigen Vertrag bündelt, kann Consumer unnötig koppeln.

Consumer-orientierte Schnittstellen können:

- Versionsrisiken reduzieren,
- Berechtigungen präzisieren,
- Ownership klarer machen,
- Änderungsfolgen begrenzen.

---

## 34. DIP auf Systemebene

DIP wird architektonisch relevant, wenn Fachlogik durch technische Produkte dominiert wird.

Beispiel:

```text
Fachmodell
→ konkrete Kafka-Klasse
→ konkreter Cloud-SDK-Typ
→ konkreter Datenbanktyp
```

Dann wird ein Technologieentscheid schnell zu einer Fachmodelländerung.

Eine saubere Port-/Adapter-Grenze kann diesen Effekt reduzieren.

---

# Teil IX — Wo SOLID nicht ausreicht

## 35. SOLID ist kein Enterprise-Architecture-Framework

SOLID beantwortet nicht:

- Welche Business Capability wird benötigt?
- Welche Anwendung soll konsolidiert oder abgeschaltet werden?
- Wem gehören Daten?
- Welche Informationen sind führend?
- Build, Buy oder Reuse?
- Welche regulatorischen Constraints gelten?
- Welche Sicherheitsklassifikation liegt vor?
- Wie sieht die Transition Architecture aus?
- Welche Betriebsverantwortung besteht?
- Welche Organisations- oder Lieferantenabhängigkeiten existieren?

Deshalb gilt:

```text
SOLID
= Designheuristik für Softwarestruktur

nicht

SOLID
= vollständige Architekturmethodik
```

---

## 36. SOLID löst keine falschen Systemgrenzen

Ein perfekt entkoppelter Service kann trotzdem fachlich falsch geschnitten sein.

Ein hervorragend abstrahiertes Modul kann trotzdem die falschen Daten besitzen.

Eine austauschbare Repository-Schnittstelle löst keine unklare Datenverantwortung.

Eine gute Interface-Segregation löst keine falsche Prozessarchitektur.

Diese Grenzen müssen bewusst bleiben.

---

# Teil X — Typische Fehlanwendungen

## 37. Interface für jede Klasse

```text
FooService
FooServiceImpl
```

ohne realen Vertrag oder Variationspunkt ist kein automatischer Qualitätsgewinn.

---

## 38. Eine Klasse pro Methode

SRP wird nicht durch möglichst kleine Klassen erfüllt.

Zu starke Fragmentierung kann Kohäsion zerstören und kognitive Last erhöhen.

---

## 39. Pattern pro Prinzip

SOLID schreibt keine konkreten Design Patterns vor.

Strategy kann OCP unterstützen, ist aber nicht OCP selbst.

Ports & Adapters kann DIP unterstützen, ist aber nicht zwingend erforderlich.

---

## 40. OCP als Änderungsverbot

Bestehender Code darf selbstverständlich geändert werden.

OCP adressiert sinnvolle Stabilität an wiederkehrenden Variationsgrenzen, nicht Unveränderbarkeit.

---

## 41. LSP nur mit Vererbung verbinden

Auch Interface-Implementierungen und Adapter können semantisch nicht substituierbar sein.

---

## 42. DIP mit Dependency Injection verwechseln

Ein DI-Container kann Abhängigkeiten verdrahten.

Er garantiert aber keine sinnvolle Abhängigkeitsrichtung.

```java
@Service
class EligibilityService {
    @Autowired
    JpaRepository repository;
}
```

ist Dependency Injection, aber nicht automatisch Dependency Inversion.

---

## 43. „Clean Code“ gegen Systemarchitektur optimieren

Lokale Eleganz darf nicht dazu führen, dass:

- zusätzliche Netzwerkgrenzen entstehen,
- Transaktionen unnötig verteilt werden,
- Datenhoheit verwässert wird,
- Betriebsaufwand unverhältnismäßig wächst.

Lokales Design muss in das Gesamtsystem passen.

---

## 44. SOLID-Score als Qualitätsbeweis

Eine Checkliste wie:

```text
SRP 8/10
OCP 9/10
LSP 7/10
```

hat ohne klar definiertes Messmodell wenig Aussagekraft.

Besser sind konkrete Änderungsszenarien und belegbare strukturelle Risiken.

---

# Teil XI — Praktisches Reviewverfahren

## 45. Schritt 1: Änderungsszenario definieren

Keine abstrakte SOLID-Diskussion beginnen.

Zuerst eine reale oder plausible Änderung formulieren:

> Ein zweiter Provider soll integriert werden.

oder:

> Eine neue Fallart wird eingeführt.

oder:

> Das Persistenzmodell ändert sich.

---

## 46. Schritt 2: Betroffene Verantwortungen bestimmen

Fragen:

- Welche fachliche Verantwortung ändert sich?
- Welche technische Verantwortung ändert sich?
- Welche Einheiten sind heute betroffen?

---

## 47. Schritt 3: Abhängigkeiten kartieren

```text
A → B → C → D
```

Nicht nur Klassenimports betrachten.

Auch prüfen:

- Datenmodelle,
- API-Verträge,
- Runtime-Abhängigkeiten,
- Seiteneffekte.

---

## 48. Schritt 4: Verträge prüfen

- Was verspricht das Interface?
- Welche Semantik erwarten Consumer?
- Sind Implementierungen wirklich substituierbar?
- Sind Fehlerfälle Teil des Vertrags?

---

## 49. Schritt 5: volatile Entscheidungen finden

Welche Teile ändern sich häufiger?

- Provider?
- Format?
- Regel?
- Persistenz?
- Transport?

Volatilität liefert Hinweise auf sinnvolle Abstraktionsgrenzen.

---

## 50. Schritt 6: vorhandene Abstraktionen hinterfragen

Für jedes Interface fragen:

> Welche konkrete Designentscheidung rechtfertigt diese Indirektion?

Wenn keine Antwort existiert, ist das Interface möglicherweise nur historisches Ritual.

---

## 51. Schritt 7: neue Abstraktion begründen

Eine neue Abstraktion braucht mindestens einen klaren Treiber:

- reale Variante,
- externe Grenze,
- volatile Integration,
- fachlich stabiler Port,
- notwendiger Test-Seam,
- wiederkehrendes Änderungsproblem.

---

## 52. Schritt 8: Trade-off explizit machen

Jede Verbesserung kostet etwas.

Beispiel:

```text
+ Provider austauschbar
+ Fachmodell bleibt stabil
- zusätzlicher Port
- zusätzlicher Adapter
- Mapping notwendig
```

---

## 53. Schritt 9: Evidence definieren

Mögliche Nachweise:

- Unit Tests gegen den fachlichen Port,
- Contract Tests für Implementierungen,
- ArchUnit-Regel für Dependency Direction,
- Change-Szenario im Review,
- Repository-Historie für Änderungskopplung.

---

# Teil XII — Review-Checkliste

## 54. SRP

- Welche Verantwortung besitzt die Einheit?
- Welche unabhängigen Änderungsgründe existieren?
- Welche Dinge ändern sich tatsächlich gemeinsam?
- Liegt technische und fachliche Verantwortung unnötig zusammen?
- Würde eine Trennung Klarheit erhöhen?

## 55. OCP

- Welche reale Variationsachse existiert?
- Wie häufig tritt sie auf?
- Ist der Kern stabil genug für eine Abstraktion?
- Wird Erweiterbarkeit benötigt oder nur vermutet?

## 56. LSP

- Was ist der semantische Vertrag?
- Erfüllen alle Implementierungen dieselben Vor- und Nachbedingungen?
- Gibt es unerwartete Exceptions oder Seiteneffekte?
- Sind Implementierungen aus Sicht des Consumers wirklich austauschbar?

## 57. ISP

- Wer konsumiert diesen Vertrag?
- Welche Operationen benötigt der Consumer wirklich?
- Sind fachlich unabhängige Rollen in einem Interface gekoppelt?
- Wird das Interface verständlicher oder nur kleiner?

## 58. DIP

- Welche Logik ist fachlich stabil?
- Welche Details sind technologisch volatil?
- Zeigt die Source-Code-Abhängigkeit in die gewünschte Richtung?
- Beschreibt die Abstraktion die Sprache der Fachlichkeit oder der Technik?
- Gibt es einen realen Grund für die Indirektion?

---

# Teil XIII — Definition eines tragfähigen Designs

## 59. Woran erkennt man ein ausreichend gutes Design?

Ein Design ist nicht deshalb gut, weil jedes SOLID-Prinzip sichtbar angewendet wurde.

Ein tragfähiges Design zeigt eher folgende Eigenschaften:

- Verantwortlichkeiten sind verständlich,
- relevante Änderungen bleiben überwiegend lokal,
- Verträge drücken echte Fähigkeiten aus,
- Implementierungen halten dieselbe Semantik ein,
- Fachlogik kennt nicht unnötig volatile technische Details,
- Abstraktionen haben einen erklärbaren Zweck,
- der Kontrollfluss bleibt nachvollziehbar,
- Tests prüfen relevante Verhaltensverträge,
- Designentscheidungen lassen sich mit konkreten Änderungsszenarien begründen.

Die beste Struktur ist nicht die mit den meisten Abstraktionen, sondern die mit einem angemessenen Verhältnis aus:

```text
Verständlichkeit
+
Änderbarkeit
+
Stabilität
+
Testbarkeit
+
Betriebskosten
```

---

# Teil XIV — Glossar

## Abstraktion

Eine bewusst vereinfachte Sicht, die relevante Eigenschaften sichtbar macht und andere Details verbirgt.

## Cohesion / Kohäsion

Maß beziehungsweise qualitative Beschreibung dafür, wie eng die Verantwortungen innerhalb einer Einheit zusammengehören.

## Coupling / Kopplung

Abhängigkeit zwischen Softwareelementen, Daten, Verträgen oder Abläufen.

## Contract / Vertrag

Explizite und implizite Verhaltenserwartungen zwischen Anbieter und Consumer.

## Invariante

Eigenschaft, die während definierter Zustandsübergänge erhalten bleiben muss.

## Nachbedingung

Eigenschaft, die nach erfolgreicher Ausführung einer Operation gelten muss.

## Policy

Fachlich oder architektonisch zentrale Regel beziehungsweise Entscheidung.

## Mechanism

Technischer Mechanismus zur Umsetzung einer Policy.

## Subtyping

Beziehung zwischen Typen, bei der ein Subtyp den relevanten Vertrag des Supertyps erfüllt.

## Volatilität

Wahrscheinlichkeit beziehungsweise Häufigkeit, mit der sich eine Designentscheidung ändert.

## Vorbedingung

Bedingung, die ein Client vor Aufruf einer Operation erfüllen muss.

---

# Teil XV — Quellen und weiterführende Literatur

## 60. Primär- und Grundlagenliteratur

### Robert C. Martin

Robert C. Martin, *Design Principles and Design Patterns*, 2000.

https://objectmentor.com/resources/articles/Principles_and_Patterns.pdf

Relevant für die praxisorientierte Zusammenführung objektorientierter Designprinzipien und deren Beziehung zu Abhängigkeitsstrukturen.

### Bertrand Meyer

Bertrand Meyer, *Object-Oriented Software Construction*, 2nd Edition, Prentice Hall, 1997.

https://bertrandmeyer.com/OOSC2/

Relevant insbesondere für das Open/Closed Principle und Design by Contract.

### Barbara Liskov / Jeannette Wing

Barbara H. Liskov, Jeannette M. Wing, *A Behavioral Notion of Subtyping*, ACM Transactions on Programming Languages and Systems, Vol. 16, No. 6, 1994, S. 1811–1841.

https://www.cs.cmu.edu/~wing/publications/LiskovWing94.pdf

Relevant für Behavioral Subtyping, Spezifikationen, Invarianten sowie Vor- und Nachbedingungen.

### David Parnas

David L. Parnas, *On the Criteria to Be Used in Decomposing Systems into Modules*, Communications of the ACM, Vol. 15, No. 12, 1972.

https://doi.org/10.1145/361598.361623

Relevant für Modularisierung, Information Hiding und die Frage, nach welchen Kriterien Systemgrenzen geschnitten werden sollten.

---

## 61. Empirische Forschung zu Strukturmaßen

Lionel C. Briand, Jürgen Wüst, John W. Daly, D. Victor Porter, *Exploring the relationships between design measures and software quality in object-oriented systems*, Journal of Systems and Software 51(3), 2000, S. 245–273.

DOI: 10.1016/S0164-1212(99)00102-8

Diese Arbeit untersucht Zusammenhänge zwischen objektorientierten Designmaßen und Fehleranfälligkeit. Sie wird hier nicht als Beweis für SOLID verwendet, sondern als Beispiel dafür, dass strukturelle Eigenschaften empirisch untersucht werden können.

Lionel C. Briand, Jürgen Wüst, Hakim Lounis, *Replicated Case Studies for Investigating Quality Factors in Object-Oriented Designs*, Empirical Software Engineering 6, 2001, S. 11–58.

DOI: 10.1023/A:1009815306478

---

## 62. Verhältnis zu anderen Knowledge Items

- AK-026 — Einfachheit, DRY, YAGNI und geringe Wissenskopplung
- AK-084 — Kopplung, Kohäsion und Information Hiding
- AK-031 — Hexagonal Architecture / Ports & Adapters
- AK-008 — Objektorientierung, Verantwortung und Fehlanwendungen
- AK-027 — Code Smells und Refactoring
- AK-028 — Java Design Patterns problemorientiert einsetzen

---

# 63. Merksatz

> SOLID ist kein Zielbild aus Interfaces, Patterns oder Schichten. Die fünf Prinzipien sind unterschiedliche Fragen an Verantwortung, Verträge, Variationspunkte und Abhängigkeitsrichtungen. Ihr Wert zeigt sich erst dann, wenn sie **konkrete Änderungskosten reduzieren, ohne durch unnötige Abstraktion neue Komplexität zu erzeugen**.
