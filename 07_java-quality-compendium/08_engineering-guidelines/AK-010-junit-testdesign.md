---
id: AK-010
legacy_ids:
  - QG-JAVA-010
  - QG-JAVA-013
title: Testdesign mit JUnit Jupiter
artifact_type: engineering-guideline
domain: testing
status: active
maturity: reviewed
normative_level: recommended
last_validated: 2026-09-30
review_trigger:
  - Wechsel der JUnit-Major-Version
  - wiederkehrende Probleme mit Testlesbarkeit oder Testisolation
---

# AK-010 — Testdesign mit JUnit Jupiter

## Kurzfassung

Ein guter Test beschreibt beobachtbares Verhalten, isoliert die relevante Ursache eines Fehlers und bleibt bei Refactorings möglichst stabil. JUnit liefert dafür Lifecycle, Assertions-Anbindung, `@Nested`, Tags, Parameterquellen und Extensions. Die Qualität entsteht aber nicht durch möglichst viele Annotationen, sondern durch klare Testfälle und passende Testgrenzen.

## 1. Was ein Test leisten soll

Ein Test sollte drei Fragen beantworten:

1. Welche Ausgangslage gilt?
2. Welche Aktion wird ausgeführt?
3. Welches beobachtbare Ergebnis wird erwartet?

```java
@Test
void shippedOrderCannotBeCancelled() {
    var order = anOrder().withStatus(SHIPPED).build();

    var action = () -> order.cancel("customer request");

    assertThatThrownBy(action)
        .isInstanceOf(OrderCannotBeCancelledException.class);
}
```

Die Struktur darf Arrange/Act/Assert folgen, ohne dass jeder Test Kommentare dafür braucht.

## 2. Testnamen

Testnamen beschreiben Verhalten oder Regel, nicht die Implementierung.

Gut:

```text
shippedOrderCannotBeCancelled
expiredTokenIsRejected
retryStopsAfterSuccessfulAttempt
```

Schlecht:

```text
testMethod1
cancelTest
shouldCallRepositorySave
```

Der letzte Name koppelt den Test an eine Implementierungsentscheidung, obwohl fachlich vielleicht nur relevant ist, dass eine Bestellung gespeichert wurde.

## 3. Testisolation

Tests sollen unabhängig voneinander ausführbar sein. Gemeinsamer veränderlicher Zustand zwischen Tests ist zu vermeiden.

`@BeforeEach` ist sinnvoll für wirklich gemeinsame, verständliche Vorbereitung. Wenn das Setup den eigentlichen Testfall verdeckt, wird es lokal im Test aufgebaut oder über Testdaten-Builder ausgedrückt.

## 4. `@Nested` sinnvoll einsetzen

JUnit Jupiter unterstützt verschachtelte Testklassen. Sie eignen sich, wenn mehrere Tests denselben fachlichen Zustand teilen und die Verschachtelung die Lesbarkeit erhöht.

```java
@Nested
class WhenOrderIsShipped {

    private Order order;

    @BeforeEach
    void setUp() {
        order = anOrder().withStatus(SHIPPED).build();
    }

    @Test
    void cancellationIsRejected() { ... }

    @Test
    void shippingDateIsVisible() { ... }
}
```

`@Nested` ist kein Ziel an sich. Zu tiefe Hierarchien erschweren Navigation und Setup-Verständnis.

## 5. Eine Klasse, viele Tests?

Es gibt keine allgemeingültige sinnvolle Maximalzahl für Testmethoden oder Zeilen. Aufteilung ist angezeigt, wenn:

- mehrere fachlich unabhängige Verhaltensgruppen entstehen,
- Setup und Fixtures stark auseinanderlaufen,
- Fehlerlokalisierung unübersichtlich wird,
- die Klasse unterschiedliche Testarten vermischt,
- Navigation und Review spürbar leiden.

Die Grenze ist Kohärenz, nicht eine feste Zahl.

## 6. Testarten nicht vermischen

Ein Unit-Test sollte keinen Spring-Kontext starten, wenn er ihn nicht benötigt. Ein Repository-Integrationstest sollte nicht gleichzeitig einen vollständigen HTTP-Workflow prüfen.

Typische Ebenen:

- Unit-Test: eine fachliche Einheit ohne Infrastruktur,
- Slice-Test: fokussierter Frameworkausschnitt,
- Integrationstest: reale technische Abhängigkeit oder mehrere Komponenten,
- Contract-Test: Schnittstellenkompatibilität,
- End-to-End-Test: wenige kritische Gesamtpfade.

Die konkrete Strategie wird in `AK-096` beschrieben.

## 7. Normative Regeln

### MUSS

- Tests müssen unabhängig ausführbar sein oder eine bewusst dokumentierte Abhängigkeit besitzen.
- Ein fehlgeschlagener Test muss erkennen lassen, welche Verhaltensannahme verletzt ist.
- Tests dürfen keine produktionsrelevanten Geheimnisse oder personenbezogenen Echtdaten enthalten.

### SOLLTE

- Testnamen beschreiben Verhalten.
- Testdaten zeigen nur die für den Fall relevanten Unterschiede.
- Tests werden auf der kleinsten sinnvollen Ebene ausgeführt.
- Wiederholtes komplexes Setup wird über verständliche Builder oder Fixtures gekapselt.

### DARF NICHT

- Eine feste Zeilen- oder Testanzahl wird als Qualitätsgesetz verwendet.
- Tests werden nur auf interne Methodenstruktur zugeschnitten, wenn das Verhalten stabiler geprüft werden kann.
- `Thread.sleep()` wird als genereller Synchronisationsmechanismus für asynchrone Tests verwendet.

## 8. Prüffragen

1. Versteht ein Reviewer ohne Produktionscode, welches Verhalten geprüft wird?
2. Scheitert der Test aus genau einem nachvollziehbaren Grund?
3. Ist der Test stabil gegenüber irrelevantem Refactoring?
4. Ist die gewählte Testebene kleiner als nötig oder größer als nötig?
5. Verbirgt gemeinsames Setup wichtige Unterschiede?
6. Gibt es unnötige Abhängigkeiten zwischen Tests?

## 9. Quellen

- JUnit User Guide: https://docs.junit.org/current/user-guide/
- JUnit `@Nested`: https://docs.junit.org/current/api/org.junit.jupiter.api/org/junit/jupiter/api/Nested.html

## 10. Merksatz

> Ein Test ist dann wertvoll, wenn er eine relevante Verhaltensannahme präzise schützt und im Fehlerfall verständliche Evidenz liefert.
