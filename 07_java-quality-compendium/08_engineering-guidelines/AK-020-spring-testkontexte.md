---
id: AK-020
legacy_ids:
  - QG-JAVA-020
title: Den kleinsten sinnvollen Spring-Testkontext wählen
artifact_type: engineering-guideline
domain: testing
status: active
maturity: reviewed
normative_level: recommended
last_validated: 2026-09-30
review_trigger:
  - Wechsel der Spring-Boot-Major-Version
  - stark steigende Testlaufzeiten
  - häufige Context-Cache-Misses
---

# AK-020 — Den kleinsten sinnvollen Spring-Testkontext wählen

## Kurzfassung

Spring bietet Unit-Tests ohne Kontext, fokussierte Test Slices und vollständige Anwendungskontexte. Die richtige Ebene ist die kleinste, die die relevante Integrationsannahme tatsächlich prüfen kann.

`@SpringBootTest` ist kein Qualitätsmerkmal. Ein großer Kontext macht einen Test nicht realistischer, wenn die getestete Aussage nur eine einzelne Komponente betrifft.

## 1. Entscheidungslogik

```text
reine Java-/Domänenlogik
→ kein Spring-Kontext

MVC/Web-Schicht
→ passender Web-Slice

JPA-/Repository-Verhalten
→ Data-Slice + reale DB, wenn DB-Semantik relevant

mehrere reale Spring-Komponenten / vollständige Konfiguration
→ @SpringBootTest
```

Spring Boot bietet verschiedene `@...Test`-Slices, deren genaue Verfügbarkeit von der eingesetzten Version und den Testmodulen abhängt.

## 2. Unit-Test ohne Spring

Wenn Constructor Injection ein Objekt leicht zusammenbaubar macht, ist für reine Businesslogik kein Container nötig:

```java
class PriceCalculatorTest {
    private final DiscountPolicy policy = new FixedDiscountPolicy();
    private final PriceCalculator calculator = new PriceCalculator(policy);
}
```

Das ist schnell, explizit und unabhängig von Framework-Bootstrapping.

## 3. Slice-Tests

Ein Slice startet nur die für eine technische Schicht relevanten Teile.

Beispiele sind je nach Spring-Boot-Baseline unter anderem:

- `@WebMvcTest`,
- `@WebFluxTest`,
- `@DataJpaTest`,
- `@JdbcTest`,
- `@RestClientTest`,
- `@JsonTest` beziehungsweise entsprechende fokussierte Testmodule.

Der konkrete Importumfang eines Slices ist Frameworkverhalten und wird nicht im Compendium dupliziert, sondern in der offiziellen Dokumentation geprüft.

## 4. Vollständiger Kontext

`@SpringBootTest` ist sinnvoll, wenn die Testaussage mehrere Auto-Konfigurationen oder echte Komponenten gemeinsam betrifft, zum Beispiel:

- Security + Controller + Service + Persistence,
- vollständige Anwendungsverdrahtung,
- Konfigurationsprofile,
- Startup-Verhalten,
- End-to-End-nahe Integrationspfade innerhalb einer Anwendung.

Ein voller Kontext sollte nicht benutzt werden, nur weil das Setup dann weniger explizit wirkt.

## 5. Bean Overrides

Aktuelle Spring-Framework-Versionen bieten unter anderem `@MockitoBean`, `@MockitoSpyBean` und `@TestBean` zum gezielten Überschreiben von Beans im TestContext. Das ist nützlich, verändert aber den tatsächlichen Anwendungskontext.

Ein Test mit vielen überschriebenen Beans ist ein Signal: Vielleicht wäre ein kleinerer Slice oder Unit-Test ehrlicher und verständlicher.

## 6. Context Cache und `@DirtiesContext`

Spring kann Testkontexte zwischen Tests cachen. Unterschiedliche Konfigurationen, uneinheitliche Bean-Override-Namen und häufiges `@DirtiesContext` können Wiederverwendung verhindern und Testlaufzeiten erhöhen.

`@DirtiesContext` sollte daher nur verwendet werden, wenn der Test den Kontext tatsächlich so verändert, dass er nicht sicher wiederverwendet werden kann.

## 7. Normative Regeln

### MUSS

- Die Testebene muss zur Testaussage passen.
- Ein Spring-Kontext darf nur gestartet werden, wenn Framework- oder Integrationsverhalten Teil der Aussage ist.
- Tests dürfen nicht nur deshalb grün sein, weil zentrale produktive Beans durch Mocks ersetzt wurden.

### SOLLTE

- Unit-Tests werden für reine Fachlogik bevorzugt.
- Slice-Tests werden für fokussierte Frameworkintegration bevorzugt.
- `@SpringBootTest` wird für echte Querschnitts-/Integrationsaussagen eingesetzt.
- Context-Cache-Verhalten wird bei langsamen Test-Suites analysiert.

### DARF NICHT

- `@SpringBootTest` wird nicht pauschal zum Default für alle Tests.
- `@DirtiesContext` wird nicht reflexartig zur Testisolation eingesetzt.
- ein großer Mock-Anteil im Full-Context-Test wird nicht als realistische Integration ausgegeben.

## 8. Prüffragen

1. Welche Spring-Funktion ist Teil der Testaussage?
2. Könnte derselbe Fehler ohne vollständigen Kontext gefunden werden?
3. Welche Beans werden ersetzt – und verliert der Test dadurch seine Aussage?
4. Warum braucht der Test `@DirtiesContext`?
5. Wird eine reale Infrastrukturkomponente benötigt?
6. Ist die Laufzeit durch unnötig viele unterschiedliche Kontexte erhöht?

## 9. Quellen

- Spring Framework Testing: https://docs.spring.io/spring-framework/reference/testing.html
- Spring Framework Bean Overrides: https://docs.spring.io/spring-framework/reference/testing/testcontext-framework/bean-overriding.html
- Spring Boot Testing: https://docs.spring.io/spring-boot/reference/testing/
- Spring Boot Test Slices: https://docs.spring.io/spring-boot/appendix/test-auto-configuration/slices.html

## 10. Merksatz

> Ein Testkontext ist so groß wie nötig, nicht so groß wie möglich.
