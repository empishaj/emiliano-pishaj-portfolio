---
id: AK-011
legacy_ids:
  - QG-JAVA-011
title: Test Doubles und Mockito gezielt einsetzen
artifact_type: engineering-guideline
domain: testing
status: active
maturity: reviewed
normative_level: recommended
last_validated: 2026-09-30
review_trigger:
  - Wechsel der Mockito- oder Spring-Testinfrastruktur
  - wiederkehrende fragile Mock-basierte Tests
---

# AK-011 — Test Doubles und Mockito gezielt einsetzen

## Kurzfassung

Mocks sind ein Werkzeug zur Isolation von Kollaboratoren, kein Qualitätsziel. Sie sind besonders nützlich an klaren Ports oder externen Abhängigkeiten. Zu viele Mocks, tiefe Stubbing-Ketten oder Verifikation interner Aufrufreihenfolgen machen Tests fragil und können auf zu starke Kopplung im Produktionsdesign hinweisen.

## 1. Die Arten von Test Doubles

- **Stub:** liefert vorbereitete Antworten.
- **Fake:** einfache funktionierende Ersatzimplementierung, etwa In-Memory-Repository.
- **Mock:** prüft Interaktionen und kann Verhalten simulieren.
- **Spy:** umschließt ein reales Objekt und beobachtet oder überschreibt Teile seines Verhaltens.

Die Wahl folgt der Testabsicht.

## 2. Mockito für Unit-Tests

```java
@ExtendWith(MockitoExtension.class)
class PaymentServiceTest {

    @Mock
    PaymentGateway gateway;

    @InjectMocks
    PaymentService service;

    @Test
    void rejectedPaymentDoesNotConfirmOrder() {
        given(gateway.charge(any())).willReturn(REJECTED);

        var result = service.pay(command());

        assertThat(result.status()).isEqualTo(PAYMENT_REJECTED);
    }
}
```

Das fachlich relevante Ergebnis sollte im Zentrum stehen. `verify()` ist sinnvoll, wenn eine Interaktion selbst Teil des Vertrags ist, beispielsweise dass ein Event genau einmal publiziert wird.

## 3. Interaktion nicht übertesten

Fragil:

```java
verify(repository).findById(id);
verify(mapper).toEntity(order);
verify(repository).save(any());
verify(publisher).publish(any());
verifyNoMoreInteractions(repository, mapper, publisher);
```

Wenn der Test jeden internen Schritt festschreibt, verhindert er Refactoring, ohne zwingend mehr fachliche Sicherheit zu liefern.

Besser ist, nur Interaktionen zu verifizieren, deren Ausbleiben oder Mehrfachausführung beobachtbares Fehlverhalten wäre.

## 4. Keine Deep Stubs als Standard

```java
when(client.getConfiguration().getRetry().getLimit()).thenReturn(3);
```

Solche Ketten spiegeln häufig eine ungünstige Produktionsschnittstelle. Ein Test sollte nicht die gesamte Objektstruktur einer Abhängigkeit imitieren müssen.

## 5. Fakes können besser sein

Für stabile Ports kann eine kleine Fake-Implementierung ausdrucksstärker sein als viele Mock-Setups:

```java
final class InMemoryOrderRepository implements OrderRepository {
    private final Map<OrderId, Order> data = new HashMap<>();
    // ...
}
```

Fakes sind besonders nützlich, wenn viele Tests dieselbe einfache Kollaboratorsemantik brauchen. Sie dürfen aber nicht vortäuschen, dass reale Infrastruktur bereits getestet sei.

## 6. Mockito in Spring-Kontexttests

Aktuelle Spring-Framework-Versionen stellen `@MockitoBean` und `@MockitoSpyBean` bereit, um Beans in einem Test-`ApplicationContext` zu überschreiben. Diese Annotationen sind vom normalen Mockito-`@Mock` zu unterscheiden.

```java
@SpringJUnitConfig(AppConfig.class)
class NotificationIntegrationTest {

    @MockitoBean
    MailGateway mailGateway;
}
```

Ein Spring-Kontext sollte nur gestartet werden, wenn der Test tatsächlich Frameworkintegration benötigt. Für reine Businesslogik bleibt ein normaler Mockito-/JUnit-Test günstiger und klarer.

Spies werden zurückhaltend verwendet. Bei Spring-AOP-Proxies und Seiteneffekten ist besonders sorgfältig zu prüfen, was tatsächlich gestubbt und verifiziert wird.

## 7. Normative Regeln

### MUSS

- Ein Test Double darf keine reale Infrastruktur vortäuschen, die für die Aussage des Tests relevant ist.
- Interaktionsverifikation muss einen fachlichen oder technischen Vertrag schützen.
- Spring-Kontext-Mocks werden nur verwendet, wenn ein Kontexttest erforderlich ist.

### SOLLTE

- Ports und externe Kollaboratoren sind natürliche Mock-/Fake-Grenzen.
- Zustands- oder Ergebnisassertions werden gegenüber unnötiger interner Interaktionsverifikation bevorzugt.
- Wiederkehrend komplexe Mock-Konfiguration wird als Designsignal betrachtet.

### DARF NICHT

- `verifyNoMoreInteractions()` wird nicht pauschal als Qualitätsregel verwendet.
- Deep Stubs werden nicht zum Standarddesign.
- reale Zeit, Netzwerk oder Datenbank werden nicht durch Mocks ersetzt, wenn genau deren Verhalten Gegenstand des Tests ist.

## 8. Prüffragen

1. Welche Grenze isoliert der Test Double?
2. Ist diese Grenze im Produktionsdesign ebenfalls klar?
3. Prüft der Test Verhalten oder hauptsächlich Implementierungsdetails?
4. Wäre ein Fake lesbarer als wiederholtes Stubbing?
5. Braucht der Test wirklich einen Spring-Kontext?
6. Wird bei einem Spy unbeabsichtigt reales Verhalten ausgeführt?

## 9. Quellen

- Mockito Documentation: https://site.mockito.org/
- Spring Framework — `@MockitoBean` und `@MockitoSpyBean`: https://docs.spring.io/spring-framework/reference/testing/annotations/integration-spring/annotation-mockitobean.html
- Spring Framework Testing: https://docs.spring.io/spring-framework/reference/testing.html

## 10. Merksatz

> Mocks helfen beim Isolieren klarer Grenzen. Wenn ein Test das gesamte Innenleben eines Objekts nachbauen muss, sollte zuerst das Design geprüft werden.
