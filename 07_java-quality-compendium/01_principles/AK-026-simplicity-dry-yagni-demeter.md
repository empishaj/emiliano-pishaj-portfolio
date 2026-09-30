---
id: AK-026
legacy_ids:
  - QG-JAVA-026
title: Einfachheit, DRY, YAGNI und geringe Wissenskopplung
artifact_type: architecture-principle
domain: software-design
status: active
maturity: reviewed
normative_level: recommended
last_validated: 2026-09-30
review_trigger:
  - grundlegende Änderung der Designprinzipien
---

# AK-026 — Einfachheit ohne Simplifizierung

## 1. Kernidee

KISS, DRY, YAGNI und die Law of Demeter sind keine vier unabhängigen Gebote.

Gemeinsam helfen sie, eine Frage zu beantworten:

> **Welche Komplexität ist für das aktuelle Problem wirklich notwendig – und welche haben wir selbst erzeugt?**

Die Gefahr liegt auf beiden Seiten:

```text
zu wenig Struktur
→ Duplikation, starke Kopplung, versteckte Regeln

zu viel Struktur
→ spekulative Abstraktion, Framework-Bau, kognitive Last
```

Professionelles Design sucht nicht die kleinste Anzahl von Codezeilen, sondern die **geringste notwendige kognitive und strukturelle Komplexität**.

## 2. KISS — die einfachste tragfähige Lösung

„Einfach“ bedeutet nicht:

- ohne Security,
- ohne Fehlerbehandlung,
- ohne Tests,
- ohne Betriebsfähigkeit.

Eine Lösung ist einfach, wenn sie das relevante Problem mit möglichst wenigen unnötigen Konzepten löst.

Prüffragen:

- Kann die Lösung in wenigen klaren Begriffen erklärt werden?
- Ist jede Abstraktion notwendig?
- Gibt es versteckte Magie?
- Ist der Daten- und Kontrollfluss nachvollziehbar?
- Wird Komplexität nur verschoben?

### Beispiel

Wenn drei statische Konfigurationswerte benötigt werden, ist ein selbstgebautes Plugin-/Registry-System wahrscheinlich zu groß.

Wenn dieselbe Konfiguration jedoch mandantenabhängig, auditierbar, dynamisch und zeitgesteuert ausgerollt werden muss, ist ein dediziertes System möglicherweise die **einfachere Gesamtlösung**, obwohl es technisch größer aussieht.

## 3. YAGNI — keine Architektur für hypothetische Zukunft

YAGNI bedeutet:

> Baue keinen Variationspunkt, nur weil er theoretisch irgendwann nützlich sein könnte.

Schlecht:

```text
"Vielleicht brauchen wir in drei Jahren fünf Datenbanken,
also bauen wir heute schon ein generisches Storage Framework."
```

Besser:

```text
"Heute existiert genau eine Persistenzanforderung.
Wir schützen die fachliche Logik hinter einer sinnvollen Grenze,
aber implementieren keine hypothetischen Provider."
```

YAGNI bedeutet **nicht**, bekannte nichtfunktionale Anforderungen zu ignorieren.

Security, Datenschutz, Backup oder Observability sind nicht „später vielleicht“. Wenn sie für den Scope erforderlich sind, gehören sie heute in die Architektur.

## 4. DRY — Wissen, nicht Text, deduplizieren

DRY wird häufig zu früh angewendet.

Zwei ähnliche Codeblöcke sind nicht automatisch dieselbe Abstraktion.

Die wichtigere Frage lautet:

> Repräsentieren diese Stellen **dasselbe Wissen** und müssen sie sich aus demselben Grund gemeinsam ändern?

Wenn nein, kann Duplikation vorübergehend gesünder sein als eine falsche gemeinsame Abstraktion.

### Fachliches Beispiel

Eine Altersregel für zwei unterschiedliche Verwaltungsleistungen kann heute zufällig denselben Grenzwert haben.

Wenn beide Regeln unterschiedlichen Rechtsgrundlagen und Verantwortlichen folgen, wäre eine gemeinsame `AgeRule` möglicherweise falsches DRY: Eine spätere Gesetzesänderung müsste nur eine der Regeln ändern.

Damit wird DRY auch zu einer Frage von **fachlicher Ownership**.

## 5. Law of Demeter — Wissen über Fremdstrukturen begrenzen

Eine lange Getter-Kette ist ein Symptom, aber nicht die Definition.

```java
caseFile.getApplicant().getAddress().getMunicipality().getCode()
```

Das Problem ist:

> Der aufrufende Code kennt die interne Struktur mehrerer fremder Objekte.

Wenn diese Struktur geändert wird, breitet sich Änderung aus.

Mögliche Gegenmaßnahmen:

- Verhalten näher an den Owner der Information bringen,
- einen passenden Query-/Read-Vertrag anbieten,
- Value Objects verwenden,
- explizite Mapping-Grenzen schaffen.

Aber auch hier gilt: Nicht jede Kette braucht eine Weiterleitungs-Methode. Entscheidend ist die reale Kopplung.

## 6. Die Prinzipien stehen in Spannung

### DRY vs. YAGNI

Eine frühe Abstraktion kann Duplikation reduzieren, aber einen noch nicht verstandenen Variationspunkt festschreiben.

### KISS vs. Security

Ein simpler Direktzugriff kann lokal einfacher sein, systemisch aber Security- und Auditkomplexität erhöhen.

### KISS vs. Reuse

Eine zentrale Plattform kann für einzelne Teams komplexer wirken, aber organisationsweit Variationen reduzieren.

### Law of Demeter vs. API-Explosion

Zu aggressive Abschirmung kann zu Hunderten trivialer Delegationsmethoden führen.

Das Ziel ist nicht Regelkonformität, sondern eine tragfähige Balance.

## 7. Architekturtransfer

Diese Prinzipien skalieren konzeptionell über Code hinaus.

### KISS

> Keine verteilte Architektur ohne verteiltes Problem.

### YAGNI

> Keine Plattformfähigkeit nur für hypothetische zukünftige Mandanten.

### DRY

> Gemeinsame Standards zentralisieren Wissen; fachlich unabhängige Regeln nicht künstlich vereinheitlichen.

### Demeter

> Organisationen und Systeme sollen nicht mehr über interne Strukturen ihrer Partner wissen als für den Vertrag notwendig.

Das führt zu stabileren Schnittstellen und klarerem Ownership.

## 8. Review-Schritte

Wenn ein Design komplex wirkt:

1. Welches konkrete Problem löst jedes Element?
2. Welche Teile existieren nur „für später“?
3. Welche Duplikation ist echtes doppeltes Wissen?
4. Welche Duplikation ist nur zufällig ähnliche Form?
5. Welche internen Details kennen fremde Komponenten?
6. Welcher Teil der Komplexität ist fachlich inhärent?
7. Welcher Teil ist selbst erzeugt?
8. Könnte ein Standard, eine Plattform oder ein klarerer Vertrag extraneous complexity reduzieren?

## 9. Anti-Patterns

### „DRY um jeden Preis“

Führt zu falschen Basisklassen und generischen Frameworks.

### „YAGNI, also keine Resilience“

Nicht zulässig, wenn Resilience aus dem Qualitätsziel folgt.

### „KISS = möglichst wenig Code“

Kurzer cleverer Code kann schwerer verständlich sein als etwas längerer expliziter Code.

### „Law of Demeter = keine Getter-Ketten“

Zu oberflächlich. Es geht um Wissens- und Strukturkopplung.

## 10. Quellen

- Andy Hunt, Dave Thomas — *The Pragmatic Programmer* / DRY
- Extreme Programming / YAGNI-Literatur
- Karl Lieberherr — Law of Demeter
- David Parnas — Information Hiding
- AK-025 — SOLID als Designheuristik
- AK-084 — Kopplung und Kohäsion

## 11. Merksatz

> Einfachheit ist nicht das Weglassen notwendiger Architektur. Einfachheit ist das konsequente Entfernen von **Komplexität, für die es im aktuellen Kontext keinen belastbaren Treiber gibt**.
