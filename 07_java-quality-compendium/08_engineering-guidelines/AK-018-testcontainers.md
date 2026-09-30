---
id: AK-018
legacy_ids:
  - QG-JAVA-018
title: Integrationstests mit realer Infrastruktur und Testcontainers
artifact_type: engineering-guideline
domain: testing
status: active
maturity: reviewed
normative_level: recommended
last_validated: 2026-09-30
review_trigger:
  - Wechsel der Spring-Boot- oder Testcontainers-Major-Version
  - neue produktive Infrastrukturkomponente
  - instabile Integrationstests
---

# AK-018 — Integrationstests mit Testcontainers

## Kurzfassung

Testcontainers ist sinnvoll, wenn ein Test Verhalten realer Infrastruktur benötigt, das durch Mocks oder In-Memory-Ersatzsysteme nicht zuverlässig abgebildet wird. Typische Beispiele sind PostgreSQL, Kafka, Redis oder andere Container-basierte Abhängigkeiten.

Der Container ist kein Selbstzweck. Ein Integrationstest soll eine konkrete Integrationsannahme prüfen und reproduzierbar bleiben.

## 1. Warum reale Infrastruktur testen?

Ein In-Memory-Ersatz kann entscheidende Unterschiede verschleiern:

- SQL-Dialekt und Constraints,
- Transaktions- und Lockverhalten,
- Migrationen,
- Index- oder Datentypverhalten,
- Messaging- und Brokersemantik.

Wenn genau diese Eigenschaften relevant sind, ist eine produktionsnahe Testabhängigkeit wertvoller als ein Fake.

## 2. Beispiel mit PostgreSQL

```java
@Testcontainers
@SpringBootTest
class OrderRepositoryIT {

    @Container
    @ServiceConnection
    static PostgreSQLContainer<?> postgres =
        new PostgreSQLContainer<>("postgres:17");

    @Test
    void uniqueBusinessKeyIsEnforced() {
        // Verhalten gegen reale PostgreSQL-Instanz prüfen
    }
}
```

Spring Boot unterstützt Service Connections für Testcontainers. Dadurch können Connection Details aus einem Container in die Testkonfiguration eingebunden werden, ohne jede URL manuell zu verdrahten. Welche Annotationen und Module verfügbar sind, wird gegen die eingesetzte Spring-Boot-Version geprüft.

## 3. Was gehört in einen Integrationstest?

Gute Kandidaten:

- Repository-Queries,
- DB-Constraints,
- Flyway-/Liquibase-Migrationen,
- Serialisierung gegen Broker/Schema,
- echte Client-Konfiguration,
- Transaktionen und Locking,
- Infrastrukturadapter.

Nicht jeder Service-Test braucht einen Container. Reine Fachlogik bleibt schneller und klarer als Unit-Test.

## 4. Lifecycle und Wiederverwendung

Container-Lifecycle wird so gewählt, dass Tests reproduzierbar und ausreichend schnell bleiben. Statische Container pro Testklasse sind häufig sinnvoll; globale Wiederverwendung ist eine Optimierung und darf keine versteckten Zustandsabhängigkeiten erzeugen.

Der Datenzustand zwischen Tests muss kontrolliert sein. Möglichkeiten sind zum Beispiel:

- Transaktionsrollback, wenn semantisch passend,
- gezieltes Cleanup,
- frische Schemas/Datenbanken,
- definierte Fixtures.

## 5. Migrationen mitprüfen

Wenn Produktion Flyway oder Liquibase nutzt, sollte ein relevanter Integrationstest denselben Migrationspfad starten. Ein Schema, das nur über ORM-Autogenerierung entsteht, beweist nicht, dass die produktive Migration funktioniert.

## 6. Docker-/Runtime-Abhängigkeit sichtbar machen

Testcontainers benötigt eine unterstützte Container-Runtime. CI und lokale Entwicklung müssen deshalb einen klar dokumentierten Ausführungspfad besitzen. Ein fehlender Docker-Daemon ist kein fachlicher Testfehler und sollte als Infrastrukturproblem erkennbar sein.

## 7. Normative Regeln

### MUSS

- Reale Infrastruktur wird eingesetzt, wenn deren spezifische Semantik Teil der Testaussage ist.
- Tests kontrollieren ihren Datenzustand und dürfen nicht von zufälliger Ausführungsreihenfolge abhängen.
- verwendete Container-Images werden nachvollziehbar versioniert; `latest` wird vermieden.
- Migrationen werden dort getestet, wo das produktive Schema von ihnen abhängt.

### SOLLTE

- Container werden auf der kleinsten sinnvollen Testebene eingesetzt.
- Spring-Boot-Service-Connections werden genutzt, wenn sie zur eingesetzten Baseline passen und Konfiguration vereinfachen.
- wiederkehrende Infrastrukturkonfiguration wird zentralisiert, ohne versteckte globale Zustände zu schaffen.

### DARF NICHT

- H2 oder ein anderer In-Memory-Ersatz gilt nicht automatisch als Beweis für PostgreSQL-/MySQL-spezifisches Verhalten.
- Container werden nicht gestartet, wenn der Test nur reine Fachlogik prüft.

## 8. Prüffragen

1. Welche konkrete Infrastruktureigenschaft soll bewiesen werden?
2. Würde ein Fake denselben Fehler zuverlässig entdecken?
3. Ist das Image versioniert?
4. Ist der Datenzustand pro Test kontrolliert?
5. Werden produktive Migrationen tatsächlich ausgeführt?
6. Ist der Test in CI reproduzierbar?

## 9. Quellen

- Testcontainers for Java: https://java.testcontainers.org/
- Spring Boot Testing: https://docs.spring.io/spring-boot/reference/testing/
- Spring Boot Testcontainers: https://docs.spring.io/spring-boot/reference/testing/testcontainers.html

## 10. Merksatz

> Reale Infrastruktur gehört in Tests, wenn ihre reale Semantik Teil der Aussage ist – nicht weil ein Container moderner wirkt als ein Mock.
