---
id: AK-024
legacy_ids:
  - QG-JAVA-024
title: Caching als bewusste Konsistenz- und Kapazitätsentscheidung
artifact_type: engineering-guideline
domain: performance
status: active
maturity: reviewed
normative_level: recommended
last_validated: 2026-09-30
review_trigger:
  - Änderung der Datenaktualität oder Konsistenzanforderungen
  - Performance-Incident
  - Cache-Staleness- oder Invalidierungsfehler
---

# AK-024 — Caching als bewusste Konsistenz- und Kapazitätsentscheidung

## Kurzfassung

Ein Cache tauscht Aktualität und zusätzliche Zustandskomplexität gegen geringere Latenz oder weniger Last. Deshalb beginnt die Entscheidung nicht mit Redis, Caffeine oder `@Cacheable`, sondern mit der Frage: **Welche teure Operation soll vermieden werden und wie alt darf das Ergebnis sein?**

## 1. Vor dem Cache messen

Caching ist sinnvoll, wenn eine wiederholte Operation nachweislich relevant teuer ist und Ergebnisse ausreichend wiederverwendbar sind.

Vor Einführung werden mindestens betrachtet:

- Aufruffrequenz,
- Latenz,
- Kosten des Originals,
- Änderungsfrequenz der Daten,
- tolerierbare Staleness,
- Hit-Rate-Potenzial,
- Speicherbedarf.

Ein Cache vor einer schlechten Query kann das Symptom verdecken. Eine optimierbare Query sollte zuerst verstanden werden.

## 2. Lokaler oder verteilter Cache

### Lokaler Cache

Beispiel: Caffeine.

Vorteile:

- sehr geringe Latenz,
- kein Netzwerkhop,
- einfache Betriebsstruktur.

Nachteile:

- jede Instanz besitzt eigenen Zustand,
- Invalidierung ist instanzübergreifend schwieriger,
- Memory wächst pro Prozess.

### Verteilter Cache

Beispiel: Redis.

Vorteile:

- gemeinsamer Cachezustand,
- bessere Nutzung über mehrere Instanzen,
- zentrale TTL-/Eviction-Mechanismen.

Nachteile:

- zusätzliche Netzwerk- und Betriebsabhängigkeit,
- eigener Failure Mode,
- Security- und Datenklassifikation erforderlich.

## 3. Cache-Aside

Ein häufiges Muster:

```text
Read
→ Cache prüfen
→ Miss: Source of Truth lesen
→ Cache befüllen
→ Ergebnis liefern
```

Bei Änderungen muss die Invalidierungsstrategie klar sein.

## 4. TTL ist eine Fach-/Qualitätsentscheidung

Eine TTL ist nicht nur Performance-Tuning. Sie definiert, wie lange ein möglicherweise veralteter Wert akzeptiert wird.

Deshalb wird sie aus der Datenbedeutung abgeleitet:

- Produktkatalog kann gegebenenfalls Minuten alt sein,
- Berechtigungsstatus möglicherweise deutlich weniger,
- sicherheitskritische Sperren sollten unter Umständen gar nicht positiv gecacht werden.

Feste organisationsweite TTLs sind selten sinnvoll.

## 5. Eviction und Begrenzung

Ein lokaler Cache muss begrenzt sein. Caffeine unterstützt unter anderem größen-, zeit- und referenzbasierte Eviction.

Die konkrete Maximalgröße wird aus:

- Memory-Budget,
- Objektgröße,
- Zugriffsmuster,
- Hit Rate

abgeleitet und beobachtet.

## 6. Cache Stampede

Wenn viele Requests gleichzeitig denselben abgelaufenen Wert neu berechnen, kann der Ursprung überlastet werden.

Mögliche Gegenmaßnahmen:

- deduplizierte/atomare Loads,
- Refresh-before-expiry,
- gestaffelte Expiry/Jitter,
- angemessene Concurrency-Limits,
- Serving stale bei fachlich zulässigen Reads.

## 7. Negative Caching

Auch „nicht gefunden“ kann kurzfristig gecacht werden, wenn dadurch wiederholte teure Misses vermieden werden und der fachliche Zustand nicht zu schnell ändern muss. Negative TTLs sind häufig kürzer als positive und müssen bewusst gewählt werden.

## 8. Cache und Security

Cache Keys und Values können personenbezogene oder mandantenspezifische Daten enthalten.

Deshalb:

- Tenant-/Security-Scope muss Teil der Schlüsselbildung sein, wenn Daten getrennt werden müssen,
- Secrets werden nicht als Cachewert behandelt,
- Authorization darf nicht durch einen global gecachten fachlichen Wert umgangen werden,
- Cache-Dumps und Redis-Zugriff gehören in Schutzbedarfsbetrachtung.

## 9. Normative Regeln

### MUSS

- Für jeden Cache müssen Source of Truth, Aktualitätsanforderung und Invalidierungsstrategie klar sein.
- Cache-Wachstum wird begrenzt.
- mandanten- oder berechtigungssensitive Daten werden korrekt gescoped.
- relevante Hit-/Miss- und Fehlerdaten sind beobachtbar.

### SOLLTE

- zuerst wird geprüft, ob die ursprüngliche Operation optimiert werden kann.
- TTL und Größe werden aus messbaren Anforderungen abgeleitet.
- Cache-Stampede wird bei teuren Loads berücksichtigt.
- lokaler Cache wird bevorzugt, wenn keine gemeinsame Konsistenz zwischen Instanzen benötigt wird.

### DARF NICHT

- Cache wird nicht als Source of Truth behandelt.
- eine TTL wird nicht ohne fachliche Staleness-Betrachtung gewählt.
- ein globaler Key darf keine Tenant-/Authorization-Grenze vermischen.

## 10. Prüffragen

1. Welche messbare Kostenstelle reduziert der Cache?
2. Wie alt darf ein Wert sein?
3. Was ist die Source of Truth?
4. Wie wird Invalidierung ausgelöst?
5. Was passiert beim Cache-Ausfall?
6. Was passiert bei gleichzeitigem Miss vieler Requests?
7. Enthält der Cache sensitive oder mandantenspezifische Daten?
8. Welche Metriken zeigen, ob der Cache tatsächlich Nutzen bringt?

## 11. Quellen

- Caffeine Wiki — Eviction: https://github.com/ben-manes/caffeine/wiki/Eviction
- Caffeine Wiki: https://github.com/ben-manes/caffeine/wiki
- Spring Framework Cache Abstraction: https://docs.spring.io/spring-framework/reference/integration/cache.html

## 12. Merksatz

> Ein Cache ist zusätzliche Datenhaltung mit begrenzter Aktualität. Behandle ihn entsprechend bewusst.
