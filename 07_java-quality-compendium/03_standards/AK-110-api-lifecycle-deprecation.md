---
id: AK-110
legacy_ids:
  - ADR-110
title: API Lifecycle, Deprecation und Sunset
artifact_type: lifecycle-policy
domain: integration-governance
status: active
maturity: reviewed
normative_level: normative
owner_role: Integration Architecture / API Governance
last_validated: 2026-09-28
review_trigger:
  - neue HTTP-Lifecycle-Spezifikation
  - Änderung der organisationsweiten API-Governance
---

# AK-110 — API Lifecycle Policy

## 1. Zweck

Eine API ist nicht fertig, wenn sie veröffentlicht wurde.

Sie besitzt einen Lebenszyklus:

```text
Draft
→ Active
→ Deprecated
→ Migration
→ Sunset
→ Retired
```

Die zentrale Governance-Frage lautet:

> Wie verändern oder entfernen wir einen Vertrag, ohne Consumer überraschend zu brechen?

## 2. Keine universelle Mindestfrist

Die historische Guideline setzte pauschal mindestens sechs Monate parallelen Betrieb voraus.

Das kann organisationsspezifisch sinnvoll sein, ist aber kein allgemeingültiger technischer Standard.

Die Deprecation-/Sunset-Frist wird anhand von Faktoren festgelegt wie:

- Anzahl und Art der Consumer,
- organisationsübergreifende Abhängigkeiten,
- Vertrags-/Vergabezyklen,
- Kritikalität,
- Migrationsaufwand,
- Security-/EOL-Druck,
- gesetzliche Fristen.

Eine organisationsweite Policy kann daraus Mindestfristen definieren.

## 3. Deprecation bedeutet nicht Abschaltung

RFC 9745 standardisiert seit 2025 das `Deprecation` HTTP Response Header Field.

Eine Deprecation signalisiert:

> Diese Ressource ist oder wird deprecated; neue Abhängigkeiten sollen vermieden und Migration geplant werden.

Das Verhalten der Ressource ändert sich durch die Kennzeichnung allein nicht automatisch.

## 4. Sunset

RFC 8594 definiert den `Sunset` Header für den Zeitpunkt, ab dem eine Ressource voraussichtlich nicht mehr verfügbar sein wird.

Deprecation und Sunset sind damit unterschiedliche Informationen:

```text
Deprecation
→ nicht mehr für neue Nutzung vorgesehen

Sunset
→ geplanter Zeitpunkt der Außerbetriebnahme
```

## 5. Consumer-Inventar

Eine API darf nicht ausschließlich anhand theoretischer Dokumentation stillgelegt werden.

Vor Sunset SOLLTE bekannt sein:

- welche Consumer existieren,
- wer sie verantwortet,
- welche Versionen sie nutzen,
- wie kritisch sie sind,
- ob Migrationsfortschritt sichtbar ist.

Quellen können sein:

- API Gateway Metrics,
- Access Logs,
- Consumer Registry,
- Contract Broker,
- organisatorische Vereinbarungen.

Runtime-Telemetrie allein erkennt nicht zwangsläufig selten genutzte oder saisonale Consumer.

## 6. Lifecycle-Phasen

### Phase 1 — Active

- vollständig unterstützt,
- neue Consumer zulässig.

### Phase 2 — Deprecation announced

- Migration dokumentieren,
- Consumer informieren,
- neue Integrationen auf Nachfolger lenken,
- Runtime-Signal optional/standardisiert bereitstellen.

### Phase 3 — Migration

- Consumer-Fortschritt verfolgen,
- Support-/Testmöglichkeiten bereitstellen,
- verbleibende Risiken eskalieren.

### Phase 4 — Sunset scheduled

- verbindlichen Termin bekanntgeben,
- technische und organisatorische Readiness prüfen,
- Ausnahmen explizit entscheiden.

### Phase 5 — Retired

- Traffic ist erwartungsgemäß beendet,
- Altvertrag deaktiviert,
- unnötige Infrastruktur/Policies entfernt,
- Dokumentation archiviert.

## 7. Breaking Changes

Bevor eine neue Version erzeugt wird, sollte geprüft werden, ob additive Evolution genügt.

Breaking Changes können sein:

- Ressource/Operation entfernen,
- Pflichtfeld hinzufügen,
- Semantik ändern,
- Security-Modell inkompatibel verändern,
- Fehler-/Statussemantik brechen.

Eine neue URL allein löst den Consumer-Migrationsprozess nicht.

## 8. Organisatorische Kommunikation

Bei organisationsübergreifenden APIs braucht es neben technischen Headern:

- Owner-to-Owner-Kommunikation,
- Migrationsanleitung,
- Supportkanal,
- Eskalationsweg,
- dokumentierte Frist und Ausnahmeprozess.

Gerade in Behörden kann ein Consumer wegen eigener Vergabe-, Release- oder Freigabezyklen nicht kurzfristig migrieren.

## 9. Ausnahmen

Eine Sunset-Ausnahme enthält:

- Consumer,
- Grund,
- neues Exit-Datum,
- Risiko,
- Kompensationsmaßnahmen,
- Owner.

Eine Ausnahme verlängert nicht still die gesamte Legacy-API für alle Consumer.

## 10. Evidence

Vor endgültigem Retirement:

- Consumer-Inventar geprüft,
- produktive Nutzung gemessen,
- Ausnahmen geschlossen oder aktiv dokumentiert,
- Nachfolger betriebsfähig,
- Support-/Betriebsdokumentation angepasst.

## 11. Quellen

- RFC 9745 — Deprecation HTTP Response Header Field  
  https://www.rfc-editor.org/rfc/rfc9745.html
- RFC 8594 — Sunset HTTP Header Field  
  https://www.rfc-editor.org/rfc/rfc8594.html
- AK-064 — Versionierung
- AK-066 — API First

## 12. Coach-Merksatz

> Eine API abzuschalten ist kein technischer Toggle. Es ist **Consumer- und Veränderungsmanagement mit messbarer Nutzung, klarer Verantwortung und kontrolliertem Exit**.
