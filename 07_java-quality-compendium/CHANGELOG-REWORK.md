# Rework Summary — 2026-10-01

Diese Datei dokumentiert die wesentlichen Strukturentscheidungen der einmaligen Konsolidierung des Compendiums. Sie ist kein Migrationskatalog und enthält keine zweite Inhaltsstruktur.

## Ziel

Aus mehreren parallel gewachsenen Java-/ADR-/AK-Strukturen wurde ein einziges Architecture Knowledge System mit klar getrennten Artefakttypen.

## Wesentliche Änderungen

- Historische `QG-JAVA-*`-Flat-Files entfernt, nachdem relevante Inhalte geprüft und übernommen wurden.
- Parallelen `v2/`-Ansatz entfernt.
- Allgemeines Wissen konsequent von echten Architecture Decision Records getrennt.
- Kanonische Ordner nach Artefakttyp eingeführt bzw. bereinigt.
- `08_engineering-guidelines/` für implementierungsnahe Java-/Testing-/Persistence-Themen etabliert.
- `INDEX.md` als vollständige Navigation und Lernpfadübersicht ergänzt.
- Künstliche `Coach-*`-Bezeichnungen aus dem aktiven Wissensbestand entfernt.
- Universelle Beispielgrenzwerte durch kontext-, mess- oder policybasierte Regeln ersetzt.
- Technische Aussagen, insbesondere zu Java, OpenAPI, AsyncAPI, Security, Persistence, GitOps und Runtime-Themen, gegen geeignete Primärquellen geprüft.
- Tote Querverweise und redundante Artefakte entfernt.
- `AK-082` entsprechend seinem Artefakttyp zu den Learning Guides verschoben.
- GitOps (`AK-114`) auf eine einzige kanonische Reference Architecture konsolidiert.

## Kanonische Struktur

```text
00_governance/
01_principles/
02_decision-guides/
03_standards/
04_reference-architectures/
05_operating-guides/
06_operating-models/
07_learning-guides/
08_engineering-guidelines/
INDEX.md
README.md
```

## Entscheidungsregel für die Zukunft

Ein Thema wird im aktiven Bestand genau einmal kanonisch geführt. Neue Inhalte werden nach ihrem tatsächlichen Zweck klassifiziert. Ein echtes ADR entsteht nur bei einer konkreten architekturrelevanten Entscheidung in einem konkreten Kontext.

Diese Zusammenfassung kann nach einigen Versionen entfernt werden, sobald die neue Struktur selbstverständlich geworden ist.
