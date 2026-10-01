---
id: AK-023
legacy_ids:
  - QG-JAVA-023
title: Domain-Driven Design – wann Domänenmodellierung die Komplexität trägt
artifact_type: decision-guide
domain: domain-architecture
status: active
maturity: reviewed
normative_level: informative
last_validated: 2026-10-01
review_trigger:
  - wesentliche Änderung der fachlichen Domänenstruktur
---

# AK-023 — Domain-Driven Design als Antwort auf fachliche Komplexität

## 1. Entscheidungsfrage

DDD ist kein Architekturstandard für jede CRUD-Anwendung.

Die Frage lautet:

> Ist die fachliche Komplexität hoch genug, dass ein bewusstes Domänenmodell, gemeinsame Sprache und klare Konsistenzgrenzen die Änderungskosten reduzieren?

## 2. Signale für DDD

DDD ist besonders nützlich, wenn:

- fachliche Regeln komplex und häufig verändert werden,
- dieselben Begriffe in verschiedenen Bereichen unterschiedliche Bedeutung besitzen,
- mehrere Fachrollen widersprüchliche Modelle verwenden,
- Datenkonsistenz und Zustandsübergänge fachlich kritisch sind,
- Systemgrenzen entlang fachlicher Verantwortungen geschnitten werden müssen.

Weniger Nutzen besteht bei:

- einfachem CRUD,
- rein technischer Datenweiterleitung,
- kurzfristigen Utilities,
- nahezu regelarmen Stammdatenoberflächen.

## 3. Ubiquitous Language

Das wichtigste DDD-Artefakt ist nicht die Aggregate-Klasse, sondern eine gemeinsame, kontextgebundene Sprache.

Ein Begriff wie `Status`, `Person`, `Antrag` oder `Kunde` kann je Kontext unterschiedliche Semantik besitzen.

Architekturarbeit fragt:

- Wer verwendet den Begriff?
- Welche Regeln gehören dazu?
- Gilt dieselbe Bedeutung organisationsweit oder nur in einem Context?

## 4. Entity und Value Object

### Entity

Identität bleibt über Zustandsänderungen relevant.

### Value Object

Bedeutung entsteht aus dem Wert; oft sinnvoll immutable und invariantengeschützt.

Diese Kategorien sind Modellierungswerkzeuge, keine Pflichtquote.

## 5. Aggregate als Konsistenzgrenze

Ein Aggregate bündelt Daten und Regeln, die innerhalb einer fachlichen Transaktion konsistent gehalten werden müssen.

Warnsignale:

- Aggregate lädt riesige Objektgraphen,
- jeder Geschäftsfall sperrt viele Tabellen,
- „Aggregate“ wird nur als Synonym für JPA Entity Graph benutzt.

Prüffrage:

> Welche Invarianten müssen tatsächlich atomar geschützt werden?

## 6. Domain Service

Fachliches Verhalten, das nicht sinnvoll zu einer einzelnen Entity/Value Object gehört, kann in einem Domain Service liegen.

Nicht jeder Application Service ist Domain Service.

## 7. Repository

Repository abstrahiert Zugriff auf Aggregate/Domain-Objekte dort, wo diese Abstraktion fachlich sinnvoll ist.

Es ist nicht zwingend eine 1:1-Fassade über jede Datenbanktabelle.

## 8. Domain Events

Ein Domain Event beschreibt etwas fachlich Bedeutendes, das geschehen ist.

Intern kann es Module entkoppeln; extern kann es Bestandteil einer EDA werden.

Aber:

> Nicht jedes Domain Event muss als Kafka Event die Systemgrenze verlassen.

Interne und externe Eventverträge sind getrennte Entscheidungen.

## 9. DDD und Datenmodell

Ein DDD-Modell ist kein automatisch normalisiertes Datenbankschema.

Persistenzmodell und Domänenmodell dürfen unterschiedlich sein, wenn dies Komplexität reduziert.

Gleichzeitig ist übermäßiges Mapping für triviale Systeme ebenfalls unnötig.

## 10. Von taktischem zu strategischem DDD

AK-023 fokussiert die Modellierung innerhalb eines Kontexts.

Für System-/Organisationsgrenzen siehe AK-089:

- Bounded Context,
- Context Map,
- Upstream/Downstream,
- Anti-Corruption Layer.

## 11. Review-Fragen

1. Wo liegt die echte fachliche Komplexität?
2. Welche Begriffe sind mehrdeutig?
3. Welche Invarianten sind kritisch?
4. Was muss transaktional konsistent sein?
5. Wo ist das Modell nur technischer Datencontainer?
6. Welche Teile brauchen keine DDD-Komplexität?
7. Wer ist fachlicher Owner der Sprache?

## 12. Anti-Patterns

- DDD = viele Klassen.
- jedes CRUD-Modell bekommt Aggregate/Factory/Repository.
- JPA Entity = Domain Entity automatisch.
- ein globales „Enterprise Domain Model“ zwingt alle Contexts auf dieselbe Bedeutung.
- Domain Event = automatisch Kafka Topic.

## 13. Quellen

- Eric Evans, *Domain-Driven Design*
- Vaughn Vernon, *Implementing Domain-Driven Design*
- AK-089 — Strategic DDD

## 14. Merksatz

> DDD lohnt sich dort, wo **fachliche Bedeutung und Regeln** die eigentliche Komplexität sind. Wenn die Domäne einfach ist, kann DDD selbst zur unnötigen Komplexität werden.
