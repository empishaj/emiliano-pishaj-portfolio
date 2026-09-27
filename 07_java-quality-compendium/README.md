# Architecture Quality & Governance Compendium

> Ein technisches Wissens- und Governance-System für Architekturentscheidungen, Standards, Referenzarchitekturen, Engineering-Qualität und Betriebsfähigkeit.

## Warum dieser Bereich neu geordnet wird

Der historische Ordner ist als **Java Quality Compendium** gewachsen. Mit der Zeit kamen jedoch Themen hinzu, die deutlich über Java hinausgehen: DDD, Integration, Datenarchitektur, IAM, Security, Kubernetes, GitOps, Observability, SLOs, FinOps, Platform Engineering, Technology Strategy, Architecture Evaluation, Incident Management und AI.

Die bisherige Nummerierung ist als Lernchronik wertvoll, aber die Bezeichnung vieler generischer Wissensdokumente als „ADR“ wäre fachlich irreführend. Ein Architecture Decision Record dokumentiert eine **konkrete, architekturrelevante Entscheidung in einem konkreten Kontext**. Ein allgemeiner Leitfaden zu PostgreSQL-Partitionierung, OAuth2, GraphQL oder Testdaten ist dagegen keine bereits getroffene Architekturentscheidung.

Deshalb wird dieser Bereich schrittweise zu einem **Architecture Knowledge System (AKS)** weiterentwickelt.

## Das zentrale Modell

```text
Stakeholder / Auftrag / Problem
            ↓
      Quality & Constraints
            ↓
      Decision Drivers
            ↓
        Optionen
            ↓
   konkrete Entscheidung
            ↓
           ADR
            ↓
  Standards / Leitplanken
            ↓
Reference Architectures / Golden Paths
            ↓
 Engineering & Delivery Controls
            ↓
     Runtime Evidence
            ↓
Review / Learning / Supersession
```

Das System trennt künftig:

1. **Principles** – langlebige Leitgedanken.
2. **Decision Guides** – strukturierte Hilfe, um kontextabhängig zwischen Optionen zu entscheiden.
3. **Standards & Policies** – normative, wiederverwendbare Vorgaben.
4. **Reference Architectures** – bewährte Ziel- oder Umsetzungsmuster.
5. **Engineering Guidelines** – konkrete technische Praktiken.
6. **Operating Models & Runbooks** – Betriebs-, Governance- und Lernprozesse.
7. **Learning Guides** – didaktische Grundlagen und Synthesen.
8. **ADRs** – ausschließlich konkrete, echte Architekturentscheidungen mit Scope, Entscheidern und Konsequenzen.

## Governance-Grundsätze

- Ein ADR enthält **eine** wesentliche Entscheidung.
- Kontext wird wertneutral beschrieben; die Lösung wird nicht im Kontext vorweggenommen.
- Decision Drivers und Bewertungskriterien werden **vor** der Entscheidung sichtbar gemacht.
- Optionen werden fair dargestellt, einschließlich konservativer oder „nichts ändern“-Optionen, wenn sie realistisch sind.
- Negative Konsequenzen und neue Pflichten gehören ausdrücklich in die Entscheidung.
- Akzeptierte ADRs werden nicht still umgeschrieben. Änderungen erfolgen durch neue Entscheidungen und `supersedes`/`superseded-by`.
- Generische Standards erhalten keine erfundenen „Entscheider“ oder „Architecture Board“-Freigaben.
- Zeitabhängige Fakten wie Toolversionen, Preise, Performancewerte und Produkt-APIs werden als **Baseline** mit Validierungsdatum geführt.
- Zahlenwerte sind nur dann normative Schwellenwerte, wenn sie aus Anforderungen, Messungen oder formalen Policies abgeleitet sind.
- Architekturqualität muss soweit sinnvoll durch Tests, Fitness Functions, Policy Checks, SLOs oder andere Evidence überprüfbar werden.
- Abweichungen von Standards werden transparent als befristete Ausnahmeentscheidungen behandelt.

## Übergang

Die bestehenden `QG-JAVA-*`-Dateien bleiben während der Migration erhalten. Der neue Governance-Bereich unter `00_governance/` definiert das Zielmodell. Der Migrationskatalog ordnet jeden historischen Eintrag einem geeigneten Artefakttyp zu und dokumentiert den notwendigen Validierungsgrad.

Die historischen IDs bleiben als `legacy_id` erhalten, damit Querverweise nachvollziehbar bleiben. Neue Knowledge-Items erhalten langfristig stabile `AK-xxx`-IDs. Echte Projektentscheidungen erhalten eigene ADR-IDs und werden nicht mit Wissensartikeln vermischt.

## Verbindliche Grundlagen

Das Modell orientiert sich unter anderem an:

- Michael Nygard: *Documenting Architecture Decisions*
  https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions
- MADR 4.x
  https://adr.github.io/madr/
- arc42 – Architecture Decisions
  https://docs.arc42.org/section-9/
- ISO/IEC/IEEE 42010:2022 – Architecture Description
  https://www.iso.org/standard/74393.html
- The Open Group – Architecture Governance / Compliance Reviews
  https://www.opengroup.org/architecture/togaf7-doc/arch/p4/comp/comp.htm

## Einstieg

- [`00_governance/ARTIFACT-MODEL.md`](00_governance/ARTIFACT-MODEL.md)
- [`00_governance/VALIDATION-POLICY.md`](00_governance/VALIDATION-POLICY.md)
- [`00_governance/ADR-LIFECYCLE-AND-TEMPLATE.md`](00_governance/ADR-LIFECYCLE-AND-TEMPLATE.md)
- [`00_governance/MIGRATION-CATALOG.md`](00_governance/MIGRATION-CATALOG.md)

---

Dieses Compendium soll nicht beweisen, dass jede Technologie „beherrscht“ wird. Es soll zeigen, **wie Architekturfragen strukturiert, Entscheidungen begründet, Standards operationalisiert und technische Aussagen überprüfbar gemacht werden**.
