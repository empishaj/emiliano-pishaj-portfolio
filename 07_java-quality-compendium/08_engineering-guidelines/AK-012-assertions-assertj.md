---
id: AK-012
legacy_ids:
  - QG-JAVA-012
title: Aussagekräftige Assertions mit AssertJ
artifact_type: engineering-guideline
domain: testing
status: active
maturity: reviewed
normative_level: recommended
last_validated: 2026-09-30
review_trigger:
  - Wechsel der Assertion-Bibliothek
  - wiederkehrende schwer verständliche Testfehler
---

# AK-012 — Aussagekräftige Assertions mit AssertJ

## Kurzfassung

Assertions sollen bei Erfolg das relevante Verhalten schützen und bei Fehlern verständliche Diagnose liefern. AssertJ ist für Java-Tests besonders hilfreich, weil Collections, Exceptions und Domänenobjekte präzise geprüft werden können. Entscheidend ist nicht die Bibliothek, sondern dass die Assertion die fachliche Aussage des Tests sichtbar macht.

## 1. Präzise statt indirekt prüfen

Schwach:

```java
assertThat(result).isNotNull();
```

wenn eigentlich relevant ist:

```java
assertThat(result.status()).isEqualTo(PAID);
assertThat(result.transactionId()).isNotBlank();
```

Ein Test sollte das Merkmal prüfen, dessen Verletzung tatsächlich ein Fehler wäre.

## 2. Collections semantisch prüfen

```java
assertThat(orders)
    .extracting(Order::status)
    .containsExactly(CONFIRMED, SHIPPED);
```

Dabei ist bewusst zu wählen zwischen:

- `containsExactly` — Reihenfolge und Inhalt zählen,
- `containsExactlyInAnyOrder` — Inhalt zählt, Reihenfolge nicht,
- `contains` — Teilmenge genügt,
- `allSatisfy` — jedes Element muss eine Eigenschaft erfüllen.

Eine strengere Assertion ist nur dann besser, wenn die strengere Eigenschaft Teil des Vertrags ist.

## 3. Exceptions gezielt prüfen

```java
assertThatThrownBy(() -> order.cancel(reason))
    .isInstanceOf(OrderCannotBeCancelledException.class)
    .hasMessageContaining("SHIPPED");
```

Die konkrete Fehlermeldung sollte nur vollständig festgeschrieben werden, wenn sie selbst ein stabiler Vertrag ist. Sonst erzeugt sie unnötige Testkopplung.

## 4. Domänenspezifische Assertions

Wenn dieselben fachlichen Prüfungen ständig wiederholt werden, können eigene Assertions die Sprache verbessern:

```java
OrderAssert.assertThat(order)
    .isConfirmed()
    .hasTotal("49.99");
```

Custom Assertions sind sinnvoll, wenn sie Domänensprache verdichten. Sie sind nicht sinnvoll, wenn sie lediglich vorhandene AssertJ-Methoden ohne Mehrwert umbenennen.

## 5. Recursive Comparison mit Vorsicht

Recursive Comparison kann für DTOs und Mappingtests nützlich sein. Bei Domänenobjekten kann es jedoch interne Struktur festschreiben, obwohl nur ausgewählte fachliche Eigenschaften relevant sind.

Deshalb gilt: Je stabiler und öffentlicher der Vertrag, desto stärker sollte die Assertion auf dessen beobachtbare Semantik zielen statt auf zufällige interne Felder.

## 6. Soft Assertions

Soft Assertions sind sinnvoll, wenn mehrere unabhängige Eigenschaften desselben Ergebnisses gemeinsam diagnostiziert werden sollen. Sie dürfen nicht dazu führen, dass ein Test viele fachlich unabhängige Szenarien vermischt.

## 7. Normative Regeln

### MUSS

- Assertions müssen das relevante Verhalten prüfen, nicht bloß Existenz oder Nebeneffekte.
- Reihenfolge wird nur geprüft, wenn sie Vertragsbestandteil ist.
- sensible Werte dürfen nicht unnötig in Assertion-Fehlermeldungen erscheinen.

### SOLLTE

- aussagekräftige domänenspezifische Werte werden gegenüber pauschalen `isNotNull()`-Prüfungen bevorzugt.
- Collections werden mit der semantisch passenden AssertJ-Methode geprüft.
- wiederkehrende fachliche Prüfungen können als Custom Assertion gekapselt werden.

### DARF NICHT

- vollständige Objektgraphen werden nicht reflexartig rekursiv verglichen.
- exakte Fehlermeldungen werden nicht festgeschrieben, wenn nur Fehlertyp oder Fehlercode Teil des Vertrags ist.

## 8. Prüffragen

1. Welche konkrete Verhaltensaussage schützt diese Assertion?
2. Prüft sie versehentlich mehr als der Vertrag fordert?
3. Wird ein irrelevantes Implementierungsdetail festgeschrieben?
4. Liefert ein Fehlschlag genügend Diagnose?
5. Wäre eine domänenspezifische Assertion verständlicher?

## 9. Quellen

- AssertJ Core Documentation: https://assertj.github.io/doc/
- JUnit User Guide: https://docs.junit.org/current/user-guide/

## 10. Merksatz

> Eine gute Assertion ist so präzise wie der Vertrag – nicht schwächer, aber auch nicht unnötig strenger.
