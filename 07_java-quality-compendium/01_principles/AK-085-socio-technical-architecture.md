---
id: AK-085
legacy_ids:
  - ADR-085
title: Sozio-technische Architektur – Verantwortung, Teamgrenzen und Systemgrenzen
artifact_type: architecture-principle
domain: organization-and-architecture
status: active
maturity: reviewed
normative_level: recommended
last_validated: 2026-10-01
review_trigger:
  - wesentliche Organisationsänderung
  - dauerhafte teamübergreifende Delivery-Blockaden
  - wiederkehrende Ownership-Konflikte
---

# AK-085 — Organisation und Architektur gemeinsam denken

## 1. Kernidee

Technische Systeme werden von Organisationen entworfen, verändert und betrieben. Deshalb sind Kommunikationswege, Entscheidungsrechte und Ownership keine äußeren Randbedingungen, sondern Architekturtreiber.

Conway's Law beschreibt die Beobachtung, dass Systemstrukturen dazu tendieren, Kommunikationsstrukturen der Organisation abzubilden.

Die professionelle Schlussfolgerung lautet **nicht**:

> „Wir müssen immer zuerst die Organisation umbauen.“

Sondern:

> **Wenn gewünschte technische Grenzen und tatsächliche Verantwortungs-/Kommunikationsgrenzen dauerhaft gegeneinander arbeiten, entsteht Reibung. Diese Inkonsistenz muss bewusst gestaltet werden.**

## 2. Verantwortungsarchitektur

Für jede wesentliche Systemgrenze sollten folgende Fragen beantwortbar sein:

- Wer besitzt die fachliche Verantwortung?
- Wer besitzt die Daten?
- Wer priorisiert Änderungen?
- Wer darf einen Vertrag verändern?
- Wer betreibt?
- Wer trägt Incident-Verantwortung?
- Wer genehmigt Ausnahmen?
- Wer finanziert beziehungsweise beauftragt die Fähigkeit?

Ein Diagramm mit klaren Services, aber unklaren Verantwortungen ist keine belastbare Entkopplung.

## 3. Team Topologies als Denkmodell

Team Topologies beschreibt vier fundamentale Teamtypen:

- **Stream-aligned Team** – an einem Wert-/Arbeitsstrom ausgerichtet.
- **Platform Team** – stellt interne Plattformfähigkeiten bereit, die andere Teams beschleunigen.
- **Enabling Team** – hilft zeitlich begrenzt beim Fähigkeitsaufbau.
- **Complicated Subsystem Team** – kapselt Spezialkomplexität, die nicht jedes Stream-Team selbst tragen sollte.

Dazu kommen drei Interaktionsmodi:

- Collaboration,
- X-as-a-Service,
- Facilitation.

Diese Begriffe sind keine Pflicht-Organigramme. Sie sind ein **Modell zur Analyse von Flow, kognitiver Last und Teamabhängigkeiten**.

## 4. Cognitive Load ohne Pseudopräzision

Das historische Dokument verband Team Cognitive Load mit der populären „7 ± 2“-Aussage zum menschlichen Arbeitsgedächtnis. Diese Verkürzung ist für Teamdesign nicht belastbar genug und wird nicht übernommen.

Für Architektur reicht die praktische Beobachtung:

> Ein Team kann nur begrenzt viele Domänen, Werkzeuge, Betriebsmodelle und Abhängigkeiten gleichzeitig tief beherrschen.

Fragen zur Analyse:

- Wie viele unterschiedliche Domänen muss das Team verstehen?
- Wie viele Plattformdetails muss es selbst betreiben?
- Wie viele externe Schnittstellen und Freigabewege existieren?
- Wie viel Zeit fließt in Koordination statt Wertlieferung?
- Welche Komplexität ist fachlich unvermeidbar?
- Welche Komplexität könnte durch Plattform, Standard oder klareres Ownership reduziert werden?

## 5. Architekturgrenze ≠ Teamgrenze – aber die Beziehung muss bewusst sein

Nicht jeder Bounded Context braucht ein eigenes Team.
Nicht jeder Service braucht ein eigenes Team.
Nicht jedes Team braucht genau einen Service.

Mögliche Beziehungen:

```text
1 Team → mehrere kohärente Module/Services
mehrere Teams → gemeinsame Plattform
Spezialteam → kompliziertes Subsystem
Fachteam + Dienstleister → gemeinsames Produkt mit geklärten Decision Rights
```

Entscheidend ist, dass Ownership und Abhängigkeiten verständlich bleiben.

## 6. Team API als organisatorischer Vertrag

Nicht nur Softwareservices brauchen Schnittstellen.

Auch Teams sollten sichtbar machen:

- welche Leistungen sie anbieten,
- wie andere Teams sie ansprechen,
- welche Self-Service-Wege existieren,
- welche SLOs oder Erwartungswerte gelten,
- welche Änderungen angekündigt werden,
- wann Collaboration statt Ticketübergabe nötig ist.

Eine Team API reduziert implizite Organisationsabhängigkeiten.

## 7. Behördenkontext

In Behörden sind Verantwortungsgrenzen häufig komplexer als klassische Produktteamgrenzen.

Beispiele beteiligter Rollen und Einheiten:

- Fachreferat,
- IT-Referat,
- Betrieb,
- Informationssicherheit,
- Datenschutz,
- Architektur,
- Vergabe/Einkauf,
- externer IT-Dienstleister,
- andere Behörde,
- Land oder Kommune.

Ein System kann technisch „einem Team“ gehören und fachlich trotzdem von mehreren Entscheidungsstellen abhängen.

Deshalb benötigt der Enterprise Architect eine Responsibility Map:

```text
Capability
→ fachlicher Owner
→ Datenowner
→ Anwendungsowner
→ Betriebsverantwortung
→ Security-Verantwortung
→ Lieferant
→ Entscheidungsgremium
```

## 8. Inverse Conway als Option, nicht Naturgesetz

Das sogenannte Inverse Conway Maneuver bedeutet, Organisations- und Kommunikationsstrukturen bewusst so zu gestalten, dass gewünschte Systemgrenzen unterstützt werden.

Es ist ein nützliches Mittel – aber kein universeller erster Schritt.

Vor einer Reorganisation sollte geprüft werden:

1. Ist das Problem wirklich organisatorisch?
2. Sind die fachlichen Grenzen ausreichend verstanden?
3. Kann ein klarerer Vertrag das Problem lösen?
4. Kann eine Plattform oder Self-Service Abhängigkeit reduzieren?
5. Ist die Reorganisation langfristig tragfähig?
6. Welche neuen Übergaben entstehen durch den neuen Schnitt?

## 9. Dienstleistersteuerung

Extern vergebene Entwicklung macht Ownership-Fragen besonders sichtbar.

Schlecht:

```text
Dienstleister implementiert
Behörde nimmt ab
niemand besitzt Architekturentscheidungen zwischen den Abnahmen
```

Besser:

```text
Behörde besitzt Zielbild, Standards und Decision Rights
        ↓
Dienstleister liefert Optionen / ADRs / Evidence
        ↓
gemeinsame Reviews
        ↓
klare Abnahme gegen vereinbarte Architektur- und Qualitätskriterien
```

Der externe Anbieter kann technische Verantwortung übernehmen. Die Auftraggeberorganisation darf dadurch aber ihre eigene Entscheidungsfähigkeit nicht verlieren.

## 10. Diagnose organisationaler Kopplung

Warnsignale:

- kleine Änderungen benötigen viele Meetings,
- derselbe zentrale Spezialist blockiert mehrere Teams,
- jeder Release benötigt organisationsweite Koordination,
- kein Team kann einen Fehler Ende-zu-Ende analysieren,
- API-Änderungen werden über Tickets statt Verträge gesteuert,
- Ownership wechselt je nach Problem,
- Plattformteams werden zu manuellem Ticket-Support,
- externe Dienstleister besitzen mehr Architekturkontext als Auftraggeber.

## 11. Schritt-für-Schritt-Analyse

1. Welche Capability oder Leistung wird betrachtet?
2. Welche Systeme und Daten tragen sie?
3. Welche Organisationseinheiten ändern diese Systeme?
4. Wo liegen reale Decision Rights?
5. Welche Übergaben verursachen Wartezeit?
6. Welche Abhängigkeiten sind fachlich notwendig?
7. Welche entstehen nur durch aktuelle Teamstruktur?
8. Kann Self-Service oder ein klarer Vertrag helfen?
9. Muss Ownership neu geschnitten werden?
10. Wie messen wir, ob der neue Schnitt Flow und Qualität verbessert?

## 12. Anti-Patterns

### Conway = „Organigramm wird 1:1 Softwarearchitektur“

Zu mechanisch. Relevant sind insbesondere Kommunikations- und Koordinationsstrukturen.

### Teams zuerst umbauen, Domäne später verstehen

Kann neue künstliche Grenzen erzeugen.

### Platform Team als Operations-Ticketqueue

Eine Plattform sollte kognitive Last durch produktisierte Self-Service-Fähigkeiten reduzieren.

### „Autonomes Team“ ohne Daten- oder Betriebsverantwortung

Autonomie braucht echten Entscheidungsraum.

### Organisation als Architekturdiagramm behandeln

Menschen, Macht, Budget und Zuständigkeiten verändern sich nicht durch Modellnotation allein.

## 13. Quellen

- Melvin Conway, *How Do Committees Invent?* (1968)
- Team Topologies — Key Concepts  
  https://teamtopologies.com/key-concepts
- Team Topologies — Core Team Types  
  https://teamtopologies.com/key-concepts-content/what-are-the-core-team-types-in-team-topologies
- AK-084 — Kopplung und Kohäsion
- AK-089 — Strategic DDD / Context Mapping

## 14. Merksatz

> Eine technische Grenze ist erst dann belastbar, wenn klar ist, **wer sie besitzen, verändern, betreiben und im Konfliktfall entscheiden kann**.
