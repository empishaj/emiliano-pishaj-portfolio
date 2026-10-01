---
id: AK-126
legacy_ids:
  - QG-JAVA-126
title: Docker Engine auf Debian sicher installieren und betreiben
artifact_type: operating-guide
domain: platform-operations
status: active
maturity: reviewed
normative_level: recommended
owner_role: Platform Operations
last_validated: 2026-10-01
technology_baseline:
  os: "supported Debian release according to Docker documentation"
  docker_engine: "current supported stable line"
review_trigger:
  - Debian-Major-Upgrade
  - Docker-Engine-Major-Upgrade
  - Host-/Container-Security-Incident
---

# AK-126 — Docker auf Debian als Betriebsverantwortung

## 1. Zweck

Dieses Dokument ist bewusst ein **Operating Guide**, kein Enterprise-Architekturstandard.

Es beschreibt, welche Betriebs- und Security-Fragen bei einem direkt betriebenen Docker-Engine-Host geklärt werden müssen.

Für orchestrierte Kubernetes-Plattformen gelten zusätzliche beziehungsweise andere Betriebsmodelle.

## 2. Installation aus verantworteter Quelle

Für produktive Hosts SOLLTE Docker Engine über die offiziell dokumentierte Paketquelle beziehungsweise einen organisationsweit kontrollierten Mirror installiert werden.

Convenience Scripts sind für schnelle Tests nützlich, aber nicht die bevorzugte reproduzierbare Produktionsinstallation.

Die offizielle Docker-Dokumentation nennt für Debian die Installation über das Docker APT Repository als regulären Weg.

## 3. Paket- und Versionsstrategie

MUSS geklärt sein:

- welche Debian-Releases unterstützt werden,
- welche Docker-Versionen freigegeben sind,
- wie Security Updates eingespielt werden,
- wie Breaking Changes vor Rollout getestet werden,
- wer Patchverantwortung besitzt.

Versionen werden nicht hart im dauerhaften Knowledge-Text festgeschrieben, sondern in der Technology Baseline beziehungsweise im Plattformstandard.

## 4. Docker-Daemon-Rechte

Mitgliedschaft in der klassischen `docker`-Gruppe bedeutet weitreichende Rechte und ist sicherheitlich praktisch als privilegierter Hostzugriff zu behandeln.

Optionen:

### Rootful Engine

- etabliertes Betriebsmodell,
- Zugriff auf Docker Socket streng begrenzen.

### Rootless Mode

- reduziert bestimmte Hostprivilegien,
- besitzt eigene Einschränkungen und Betriebsanforderungen.

Die Wahl hängt von Workload, Netzwerk, Storage und Betriebsmodell ab.

## 5. Firewall und Networking

Die aktuelle Docker-Debian-Dokumentation weist ausdrücklich auf Wechselwirkungen mit Firewall-Regeln hin.

Deshalb MUSS vor Produktivbetrieb geklärt werden:

- wie veröffentlichte Containerports mit Host-Firewall zusammenspielen,
- welcher Firewall-Stack unterstützt wird,
- welche Regeln in `DOCKER-USER` oder vergleichbaren Kontrollpunkten gelten,
- welche Netze extern erreichbar sein dürfen.

„Host-Firewall aktiv“ beweist nicht automatisch, dass veröffentlichte Containerports wie erwartet gefiltert werden.

## 6. Image Governance

Der Host darf nur Images beziehen, die dem Supply-Chain-Standard entsprechen.

Prüfen:

- Registry-Vertrauen,
- Digest/Version,
- Scanstatus,
- SBOM,
- Update-/Rebuild-Zyklus.

## 7. Secrets

Secrets nicht in:

- Dockerfile,
- Image Layer,
- Compose-Datei im Klartext,
- Shell History,
- Logs.

Das konkrete Secret-Management richtet sich nach Plattform und Risiko.

## 8. Compose

Docker Compose kann für kleine oder einzelne Host-Deployments sinnvoll sein.

MUSS geklärt sein:

- Wer ist Source of Truth?
- Wie werden Images versioniert?
- Wie laufen Updates/Rollbacks?
- Wo liegen Secrets?
- Welche Volumes sind persistent?
- Wie werden Health und Logs überwacht?

Compose ist kein Ersatz für Betriebsmodell.

## 9. Storage und Backup

Containerfilesystem wird als ephemeral behandelt, sofern nicht bewusst persistent gemacht.

Für Volumes:

- Owner,
- Backup,
- Restore,
- Verschlüsselung,
- Berechtigungen,
- Lifecycle

festlegen.

Ein Container-Neustart darf nicht versehentlich fachliche Daten verlieren.

## 10. Logging und Monitoring

Mindestens überwachen:

- Hostressourcen,
- Docker Daemon,
- Containerstatus,
- Restarts,
- Disk/Volume-Kapazität,
- relevante Anwendungssignale.

Logrotation muss verhindern, dass Hostdisks unkontrolliert gefüllt werden.

## 11. Hardening-Fragen

- Muss Container als Root laufen?
- Welche Capabilities werden benötigt?
- Ist der Docker Socket in Container gemountet?
- Sind Host-Pfade eingebunden?
- Ist `privileged` wirklich notwendig?
- Welche Netzwerkports sind exponiert?
- Ist das Dateisystem soweit möglich read-only?

`privileged` und Docker-Socket-Mounts besitzen besonders großen Blast Radius und benötigen explizite Begründung.

## 12. Upgrade-Runbook

1. Release Notes prüfen.
2. Kompatibilität in nichtproduktiver Umgebung testen.
3. Backup/Recovery relevanter Daten sicherstellen.
4. Wartungs-/Rollbackpfad definieren.
5. Engine/Plugins aktualisieren.
6. Services starten und Health prüfen.
7. Netzwerk, Volumes und Logs verifizieren.
8. Version und Ergebnis dokumentieren.

## 13. Abgrenzung

Dieses Dokument ersetzt nicht:

- Kubernetes Plattformstandard,
- Betriebssystem-Hardening,
- BSI-Grundschutzprüfung,
- Container Image Standard,
- Supply-Chain-Policy.

## 14. Quellen

- Docker Engine — Install on Debian  
  https://docs.docker.com/engine/install/debian/
- Docker Rootless Mode  
  https://docs.docker.com/engine/security/rootless/
- AK-037 — Container Image Standard
- AK-057 — Software Supply Chain

## 15. Merksatz

> Ein Docker-Host ist kein Entwicklerwerkzeug mehr, sobald er produktive Workloads trägt. Dann besitzt er **Patch-, Rechte-, Netzwerk-, Storage-, Recovery- und Nachweisverantwortung**.
