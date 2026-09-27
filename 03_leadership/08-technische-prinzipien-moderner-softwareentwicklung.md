# Technische Prinzipien als Leitplanken

## Einordnung

Technische Prinzipien sind für mich kein Selbstzweck und keine Sammlung persönlicher Vorlieben. Sie sollen wiederkehrende Entscheidungen vereinfachen, Risiken reduzieren und Teams innerhalb klarer Grenzen handlungsfähig machen.

In modernen Softwareorganisationen entstehen viele Probleme nicht deshalb, weil Teams keine Lösungen finden, sondern weil grundlegende Entscheidungen immer wieder neu getroffen werden: API-Design, Security, Observability, Tests, Deployment, Dokumentation oder Betriebsfähigkeit.

## Mein Grundsatz

**Gute technische Leitplanken reduzieren unnötige Varianz, ohne sinnvolle lokale Entscheidungen zu verhindern.**

---

## 1. Prinzipien brauchen einen Entscheidungszweck

Ein Prinzip ist nur dann hilfreich, wenn klar ist, welches Problem es adressiert.

Beispiele:

### API-first
Ziel: Schnittstellen früh explizit machen und Abhängigkeiten besser steuerbar halten.

### Security by Design
Ziel: Sicherheitsanforderungen nicht erst am Ende prüfen.

### Automatisierte Qualität
Ziel: wiederkehrende Qualitätsprüfungen in Delivery-Prozesse integrieren.

### Observability by Default
Ziel: Systeme im Betrieb verständlich und diagnostizierbar machen.

### Entscheidungen dokumentieren
Ziel: Kontext und Konsequenzen wichtiger Architekturentscheidungen erhalten.

Ein Prinzip ohne Problembezug wird schnell zum Dogma.

---

## 2. Leitplanken statt Einheitsarchitektur

Ich unterscheide zwischen verbindlichen Grenzen und lokalen Entscheidungen.

Verbindlich können beispielsweise sein:

- Mindestanforderungen an Security
- API-Versionierungsregeln
- Logging- und Monitoring-Anforderungen
- Qualitätsgates
- Anforderungen an Backup und Wiederherstellung
- dokumentierte Architekturentscheidungen

Innerhalb dieser Grenzen sollten Teams aber möglichst selbst entscheiden können, wie sie eine konkrete Lösung gestalten.

Das schafft Standardisierung dort, wo sie Nutzen bringt, und Autonomie dort, wo lokaler Kontext wichtig ist.

---

## 3. Wartbarkeit und Veränderbarkeit

Software wird über lange Zeit häufiger verändert als neu geschrieben.

Deshalb sind für mich wichtig:

- verständliche Strukturen
- klare Verantwortlichkeiten
- geringe unnötige Kopplung
- automatisierte Tests
- nachvollziehbare Schnittstellen
- dokumentierte Entscheidungen
- beherrschbare technische Schulden

Technische Qualität zeigt sich nicht nur daran, ob ein System heute funktioniert, sondern auch daran, wie sicher es morgen verändert werden kann.

---

## 4. Schnittstellen als Architekturgrenzen

Schnittstellen sind häufig langfristiger als interne Implementierungsdetails.

Deshalb achte ich auf:

- klare fachliche Verantwortung
- explizite Verträge
- Versionierung
- Fehlerverhalten
- Authentifizierung und Autorisierung
- Beobachtbarkeit
- Deprecation-Strategien
- Kompatibilität

Ein sauberer Schnittstellenvertrag reduziert organisatorische und technische Abhängigkeit.

---

## 5. Security als Querschnittsanforderung

Security ist für mich Teil des gesamten Lebenszyklus.

Dazu gehören:

- Bedrohungen und Schutzbedarf früh betrachten
- sichere Defaults
- Identity und Access Management
- Secrets Management
- automatisierte Prüfungen
- nachvollziehbare Berechtigungen
- Auditierbarkeit
- Patch- und Vulnerability-Prozesse

Security darf nicht erst vor Go-live relevant werden.

---

## 6. Betriebsfähigkeit als Architekturqualität

Ein System ist nicht fertig, wenn es deployt werden kann.

Es muss auch:

- beobachtbar
- supportbar
- wiederherstellbar
- skalierbar genug
- dokumentiert
- im Fehlerfall handhabbar

sein.

Deshalb gehören Logging, Monitoring, Alerting, Runbooks, SLOs und Wiederherstellungsanforderungen für mich in Architekturentscheidungen.

---

## 7. Automatisierung als Qualitätsmechanismus

Wiederkehrende Qualitätsanforderungen sollten möglichst automatisiert werden.

Beispiele:

- Tests
- Security Scans
- Dependency Checks
- Code Quality Checks
- Build-Reproduzierbarkeit
- Deployment-Prüfungen
- Infrastrukturvalidierung

Automatisierung macht Standards verlässlicher, weil ihre Einhaltung weniger von Erinnerung und manueller Disziplin abhängt.

---

## 8. Ausnahmen gehören zum Governance-Modell

Kein technischer Standard passt auf jeden Kontext.

Deshalb braucht gute Governance einen bewussten Ausnahmeprozess:

- Welcher Standard wird nicht erfüllt?
- Warum ist die Ausnahme notwendig?
- Welche Risiken entstehen?
- Welche Kompensationsmaßnahmen gibt es?
- Wer entscheidet?
- Wie lange gilt die Ausnahme?

Eine dokumentierte Ausnahme ist besser als informelle Abweichung.

---

## 9. Transfer in Enterprise Architecture

Auf Enterprise-Ebene werden technische Prinzipien zu einem Steuerungsinstrument.

Sie helfen, mehrere Vorhaben und Lieferanten entlang gemeinsamer Erwartungen auszurichten.

Wichtig ist dabei, Prinzipien in prüfbare Anforderungen zu übersetzen.

Aus „API-first“ kann beispielsweise werden:

- Spezifikation vor Implementierung
- dokumentierte Authentifizierung
- Versionierungsstrategie
- Fehlercodes
- Monitoring-Anforderungen
- Deprecation-Konzept

Erst dann wird aus einem Prinzip eine steuerbare Architekturvorgabe.

---

## 10. Was ich bewusst vermeide

- Standards ohne Zweck
- Architekturprinzipien als Geschmack
- zu detaillierte zentrale Vorgaben
- Security und Betrieb erst am Projektende
- Ausnahmen ohne Dokumentation
- Qualität ausschließlich über manuelle Reviews
- technische Regeln, die Teams nicht verstehen

---

## Kurzprinzip

**Technische Prinzipien sind für mich wirksam, wenn sie wiederkehrende Risiken reduzieren, Entscheidungen vereinfachen und als verständliche, überprüfbare Leitplanken im Alltag ankommen.**
