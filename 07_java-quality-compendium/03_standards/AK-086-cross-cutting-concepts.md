---
id: AK-086
legacy_ids:
  - ADR-086
title: Querschnittskonzepte als Architecture Standards
artifact_type: architecture-standard
domain: cross-cutting-architecture
status: active
maturity: reviewed
normative_level: normative
owner_role: Architecture Governance
last_validated: 2026-10-01
review_trigger:
  - neue organisationsweite Querschnittsanforderung
  - wiederkehrende inkonsistente Implementierung über Systeme hinweg
---

# AK-086 — Querschnittskonzepte systematisch steuern

## 1. Zweck

Viele Qualitätsprobleme entstehen nicht in einem einzelnen Modul, sondern weil dieselbe Querschnittsfrage in jedem Team anders beantwortet wird.

Typische Querschnittskonzepte:

- Authentisierung und Autorisierung,
- Logging und Korrelation,
- Fehlerbehandlung,
- Konfiguration und Secrets,
- Transaktionsgrenzen,
- Idempotenz,
- Caching,
- Observability,
- Zeit/Zeitzonen,
- Datenschutz,
- Resilience,
- Internationalisierung,
- Auditierung.

AK-086 ist kein Sammeldokument, das alle Details dupliziert. Es definiert, **wie solche Themen als Standards geführt werden**.

## 2. Wann ein Querschnittsthema einen Standard braucht

Ein organisationsweiter Standard ist besonders sinnvoll, wenn:

- mehrere Systeme dieselbe Frage beantworten müssen,
- Inkonsistenz Security-/Betriebsrisiko erzeugt,
- Consumer einheitliches Verhalten erwarten,
- eine Plattform zentralen Support bietet,
- Dienstleister gegen einheitliche Abnahmekriterien liefern sollen.

Nicht jedes Utility braucht Enterprise-Governance.

## 3. Struktur eines Querschnittsstandards

Jeder Standard SOLLTE beantworten:

1. Welches Problem wird standardisiert?
2. Für welchen Scope gilt die Regel?
3. Was ist MUSS/SOLLTE/KANN?
4. Welche zentrale Plattformfähigkeit existiert?
5. Welche lokale Verantwortung bleibt beim Team?
6. Wie wird Einhaltung geprüft?
7. Wie funktionieren Ausnahmen?
8. Wer ist Owner?
9. Welcher Review-Trigger gilt?

## 4. Zentrale Plattform vs. lokaler Code

Ein guter Standard trennt:

### zentral sinnvoll

- gemeinsame Telemetriepipeline,
- IAM/Identity Provider,
- Secret Store,
- API Gateway/Policy,
- Standard-Pipeline,
- zentrale Security Controls.

### lokal notwendig

- fachliche Authorization,
- fachliche Fehlercodes,
- Domain Invariants,
- service-spezifische SLOs,
- fachliche Idempotenz.

Zentralisierung darf fachliche Verantwortung nicht verschleiern.

## 5. Reference Implementation / Golden Path

Ein Standard wird wirksamer, wenn Teams nicht alles selbst implementieren müssen.

```text
Standard
→ Referenzimplementierung / Library / Template
→ Golden Path
→ automatischer Check
→ Ausnahmeprozess
```

Aber eine zentrale Library darf nicht zum unersetzbaren Monolithen für alle Querschnittsfragen werden.

## 6. Cross-Cutting Concept Map

| Concern | führendes Knowledge-Item |
|---|---|
| Application Security | AK-015 |
| IAM/OAuth2 | AK-040 / AK-101 |
| Logging/Tracing | AK-017 / AK-102 |
| Konfiguration/Secrets | AK-069 |
| Input Validation | AK-125 |
| Privacy Controls | AK-106 |
| API Fehlerverträge | AK-021 |
| Resilience | AK-022 |
| Supply Chain | AK-057 |
| Rate Limiting | AK-062 |

AK-086 verweist auf diese Standards, statt ihre Inhalte zu kopieren.

## 7. Evolution

Querschnittsstandards entwickeln sich weiter.

Änderungen brauchen besondere Sorgfalt, weil der Blast Radius groß sein kann.

Prüfen:

- Rückwärtskompatibilität,
- Migrationswellen,
- Plattform-/Library-Versionierung,
- bestehende Ausnahmen,
- Consumer-/Serviceinventar.

## 8. Anti-Patterns

- ein riesiges „Architecture Handbook“, das Details aller Standards dupliziert.
- zentrale Library für jede fachliche Entscheidung.
- Standard ohne Implementierungshilfe.
- Golden Path ohne Ausnahmeweg.
- jedes Team macht Security/Logging/Errors vollständig anders.

## 9. Merksatz

> Querschnittskonzepte werden reif, wenn die Organisation klar trennt: **Was muss überall gleich sein, was bleibt bewusst lokal, und wie wird beides technisch und organisatorisch überprüft?**
