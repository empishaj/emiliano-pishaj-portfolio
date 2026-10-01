---
id: AK-089
legacy_ids:
  - ADR-089
title: Strategic DDD, Bounded Contexts und Context Mapping
artifact_type: decision-guide
domain: domain-integration
status: active
maturity: reviewed
normative_level: informative
last_validated: 2026-10-01
review_trigger:
  - wesentliche Änderung fachlicher Verantwortungsgrenzen
  - neue organisationsübergreifende Integration
---

# AK-089 — Strategic DDD und Context Mapping

## 1. Kernfrage

Strategic DDD beantwortet nicht primär, wie Klassen aussehen.

Es fragt:

> **Wo gelten welche fachlichen Modelle, wer besitzt sie und wie interagieren unterschiedliche Modelle miteinander?**

Das ist unmittelbar relevant für System-, Team- und Organisationsgrenzen.

## 2. Bounded Context

Ein Bounded Context definiert den Geltungsbereich eines Modells und seiner Sprache.

Derselbe Begriff kann in verschiedenen Contexts bewusst andere Bedeutung besitzen.

Beispiel:

```text
Person im Register
≠
Antragsteller im Verfahren
≠
Benutzer im IAM
```

Der Fehler wäre, diese Modelle künstlich zu einem einzigen Enterprise-Objekt zusammenzwingen zu wollen.

## 3. Context ≠ Microservice

Ein Bounded Context ist eine fachliche Modellgrenze.

Er kann umgesetzt sein als:

- Modul,
- mehrere Module,
- Service,
- mehrere Services,
- Legacy-Anwendung.

Die Deploymentgrenze ist eine separate Architekturentscheidung.

## 4. Context Map

Eine Context Map macht Beziehungen sichtbar.

Fragen:

- Wer ist Upstream, wer Downstream?
- Wer besitzt den Vertrag?
- Wer muss sich an wen anpassen?
- Welche Daten/Semantik wird veröffentlicht?
- Wo schützen wir unser eigenes Modell?

## 5. Beziehungsmuster

### Customer / Supplier

Upstream und Downstream stimmen Entwicklung des Vertrags aktiv ab.

### Conformist

Downstream übernimmt Modell des Upstreams bewusst, weil eigene Übersetzung keinen ausreichenden Nutzen bringt.

### Anti-Corruption Layer

Downstream schützt sein Modell durch Übersetzung.

### Published Language

Ein explizit veröffentlichtes Austauschmodell dient als stabilerer Vertrag.

### Open Host Service

Upstream stellt einen bewusst gestalteten Servicevertrag für mehrere Consumer bereit.

Patternnamen sind Hilfen. Wichtig ist die tatsächliche Macht-/Ownership-Beziehung.

## 6. Anti-Corruption Layer in Behördenlandschaften

Ein ACL kann besonders sinnvoll sein, wenn:

- Legacy-System ein unpassendes Datenmodell vorgibt,
- externe Register andere Begrifflichkeiten verwenden,
- Dienstleisterprodukt proprietäre Modelle liefert,
- andere Organisation/Behörde eigene Semantik besitzt.

Beispiel:

```text
externes Registermodell
      ↓
Adapter / ACL
      ↓
fachliches Modell des Verfahrens
```

Damit bleibt die externe Semantik an einer kontrollierten Grenze.

## 7. Published Language

Ein Published Language muss mehr leisten als ein Schema.

Es benötigt:

- Begriffsdefinitionen,
- fachliche Semantik,
- Versionierung,
- Owner,
- Lifecycle,
- Fehler-/Qualitätsannahmen.

OpenAPI/AsyncAPI können technische Träger sein, ersetzen aber nicht die fachliche Sprache.

## 8. Datenownership

Context Mapping hilft, die Frage zu stellen:

> Ist diese Datenkopie führend oder nur abgeleitet?

Für jedes wichtige Informationsobjekt sollte klar sein:

- welcher Context fachlich führend ist,
- wer Änderungen initiiert,
- wie andere Contexts synchronisieren,
- wie aktuell Kopien sein müssen.

## 9. Organisation und Macht

Upstream/Downstream ist nicht nur technisch.

Ein gesetzlich oder organisatorisch zentrales Register kann einen Vertrag vorgeben, den ein Fachverfahren nicht beeinflussen kann.

Ein interner Plattformdienst kann dagegen eine echte Customer/Supplier-Beziehung mit Consumer-Feedback besitzen.

Architecture Governance muss diese reale Einflussstruktur berücksichtigen.

## 10. Context Mapping Workshop

Schritt für Schritt:

1. fachliche Teilgebiete identifizieren,
2. zentrale Begriffe und Regeln pro Gebiet sammeln,
3. Owner/Verantwortung benennen,
4. bestehende Systeme zuordnen,
5. Datenflüsse/Verträge einzeichnen,
6. Upstream/Downstream bestimmen,
7. Konflikte in Semantik markieren,
8. geeignete Beziehungsmuster diskutieren,
9. Risiken und Übergangsschritte dokumentieren.

## 11. Anti-Patterns

- ein Bounded Context pro Microservice.
- Contexts ausschließlich aus Organigramm ableiten.
- ein globales Canonical Data Model für alle fachlichen Bedeutungen erzwingen.
- ACL als bloßen DTO-Mapper ohne semantische Übersetzung betrachten.
- Context Map ohne Owner/Beziehungsrichtung.

## 12. Enterprise-Architecture-Transfer

Context Mapping ergänzt Capability- und Applikationssicht:

```text
Capability
→ fachlicher Context
→ Owner
→ Anwendungen
→ Daten
→ Integrationsbeziehung
```

Damit wird sichtbar, ob technische Integrationsgrenzen zur fachlichen Verantwortung passen.

## 13. Quellen

- Eric Evans — *Domain-Driven Design*
- Vaughn Vernon — *Implementing Domain-Driven Design*
- Context Mapping Community  
  https://contextmapper.org/
- AK-085 — Sozio-technische Architektur
- AK-023 — DDD Grundlagen

## 14. Merksatz

> Strategic DDD hilft nicht primär, Services zu schneiden. Es hilft, **fachliche Bedeutungs-, Ownership- und Übersetzungsgrenzen sichtbar zu machen, bevor technische Grenzen festgelegt werden**.
