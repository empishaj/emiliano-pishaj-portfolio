---
id: AK-049
legacy_ids:
  - QG-JAVA-049
title: Code Review und Pull Requests als Qualitäts- und Wissensfluss
artifact_type: engineering-guideline
domain: delivery
status: active
maturity: reviewed
normative_level: recommended
last_validated: 2026-09-30
review_trigger:
  - wiederkehrende Review-Engpässe
  - Qualitätsprobleme trotz Review
  - Änderung der Repository-Governance
---

# AK-049 — Code Review und Pull Requests

## Kurzfassung

Code Review soll Fehler, Risiken und Wissenslücken früh sichtbar machen und gemeinsame Ownership unterstützen. Ein Pull Request ist dafür ein Arbeitsgegenstand, kein bürokratisches Freigabeformular.

Reviewqualität wird nicht über eine universelle Zahl erlaubter Zeilen gesteuert. Entscheidend sind Kohärenz, Risikoumfang, Verständlichkeit und die Möglichkeit, die Änderung fachlich und technisch sinnvoll zu prüfen.

## 1. Ein guter Pull Request beantwortet

- Was wird geändert?
- Warum ist die Änderung nötig?
- Welche Risiken oder Trade-offs gibt es?
- Wie wurde sie getestet?
- Welche Architektur-/Sicherheits-/Betriebsfolgen sind relevant?
- Gibt es Migration, Rollback oder Feature-Flag-Aspekte?

Bei kleinen Änderungen kann das sehr kurz sein. Bei architekturrelevanten Änderungen braucht der Reviewer mehr Kontext.

## 2. Kleine Änderungen sind ein Mittel, kein Dogma

Kleine, kohärente PRs sind meist leichter zu verstehen. Eine starre Regel wie „maximal 400 Zeilen“ ist jedoch irreführend:

- generierter Code verzerrt Zahlen,
- mechanische Renames können groß und risikoarm sein,
- ein kleiner Security-Fix kann hochkritisch sein,
- künstliches Aufteilen kann einen zusammengehörigen Umbau unprüfbarer machen.

Die bessere Frage lautet:

> Kann ein Reviewer die Änderung in ihrem relevanten Kontext sicher verstehen und bewerten?

## 3. Review-Schwerpunkte

### Korrektheit

- erfüllt die Änderung den beabsichtigten Vertrag?
- sind Randfälle und Fehlerpfade berücksichtigt?

### Architektur

- bleiben Modul- und Verantwortungsgrenzen intakt?
- entsteht neue Kopplung?
- wurde eine architekturrelevante Entscheidung dokumentiert?

### Security und Privacy

- Trust Boundaries,
- AuthN/AuthZ,
- Input-/Output-Behandlung,
- Secrets,
- PII/Logging,
- Dependency-/Supply-Chain-Risiken.

### Betrieb

- Observability,
- Konfiguration,
- Migration,
- Rollback,
- Kapazitäts-/Failure-Verhalten.

### Wartbarkeit

- Namen und Struktur,
- unnötige Abstraktion,
- Tests und Diagnosefähigkeit,
- verständliche Kommentare an nicht offensichtlichen Stellen.

## 4. Automatisierung vor menschlicher Aufmerksamkeit

Formatierung, statische Analyse, Tests, Architekturregeln, Dependency Checks und andere deterministische Prüfungen gehören möglichst in CI.

Menschen sollten Reviewzeit primär für Dinge verwenden, die Kontext und Urteil benötigen:

- Verantwortung,
- Trade-offs,
- Fachlogik,
- Risiken,
- Verständlichkeit,
- Auswirkungen auf andere Systeme.

## 5. Ownership und Branch Protection

GitHub unterstützt geschützte Branches bzw. Rulesets, Required Reviews und CODEOWNERS. Solche Mechanismen können sinnvoll sein, wenn bestimmte Änderungen gezielt von verantwortlichen Rollen geprüft werden müssen.

Das Repository muss aber nicht für jede Datei denselben Freigabeweg erzwingen. Hochkritische Bereiche können strengere Regeln besitzen als normale Dokumentation oder ungefährlicher Code.

## 6. Review-Kommentare

Gute Kommentare unterscheiden:

- **blockierend:** Korrektheit, Security, Architekturvertrag, wesentlicher Betriebsmangel,
- **Verbesserung:** sollte vor Merge adressiert werden, ist aber verhandelbar,
- **Hinweis/Frage:** Verständnis oder mögliche Verbesserung,
- **Nit:** rein stilistische Kleinigkeit.

Automatisierbare Stilfragen sollten nicht immer wieder Menschen beschäftigen.

## 7. Autor und Reviewer teilen Verantwortung

Der Autor macht die Änderung reviewbar:

- verständlicher Scope,
- brauchbare Beschreibung,
- passende Tests,
- keine offensichtlichen CI-Fehler,
- relevante Entscheidungen verlinkt.

Der Reviewer prüft nicht „gegen den Autor“, sondern gegen gemeinsame Qualitätsziele. Zustimmung bedeutet nicht, dass der Reviewer künftig allein für den Code verantwortlich ist.

## 8. Normative Regeln

### MUSS

- Änderungen an geschützten produktiven Branches folgen der festgelegten Repository-Governance.
- sicherheits- oder architekturrelevante Findings werden vor Merge geklärt oder über einen dokumentierten Ausnahmeprozess behandelt.
- automatisierte Pflichtprüfungen müssen erfolgreich sein, sofern kein dokumentierter Ausnahmefall gilt.
- Review-Kommentare bleiben sachbezogen.

### SOLLTE

- PRs sind kohärent und so klein wie sinnvoll.
- relevante Risiko-, Test- und Migrationsinformationen stehen in der Beschreibung.
- CODEOWNERS/Required Reviews werden dort eingesetzt, wo spezielles Ownership relevant ist.
- deterministische Qualitätsregeln werden automatisiert.

### DARF NICHT

- eine feste Zeilenanzahl wird nicht als universelles Merge-Kriterium verwendet.
- Review ersetzt keine automatisierten Tests oder Security Controls.
- persönliche Präferenz wird nicht als Architekturstandard ausgegeben.

## 9. Prüffragen

1. Ist der Zweck der Änderung klar?
2. Ist der Scope kohärent und reviewbar?
3. Welche Fehler würden Tests oder CI nicht entdecken?
4. Ändert sich eine Architektur-, Daten- oder Sicherheitsgrenze?
5. Gibt es Migration oder Rollback?
6. Braucht eine spezialisierte Rolle Review?
7. Ist ein Kommentar blockierend oder nur Präferenz?

## 10. Quellen

- GitHub Docs — Protected Branches: https://docs.github.com/en/repositories/configuring-branches-and-merges/managing-protected-branches/about-protected-branches
- GitHub Docs — CODEOWNERS: https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners
- Google Engineering Practices — Code Review: https://google.github.io/eng-practices/review/

## 11. Merksatz

> Ein gutes Review konzentriert menschliches Urteil auf Korrektheit, Risiken und Verständlichkeit. Was zuverlässig automatisierbar ist, gehört nicht dauerhaft in dieselbe manuelle Diskussion.
