# B1 Deutsch Trainer – PWA

Diese Version ist eine installierbare Progressive Web App (PWA).

## Funktionen der PWA
- installierbar auf Android und iPhone/iPad
- eigener Startbildschirm-Eintrag
- Standalone-Darstellung ohne Browser-Chrome
- Offline-Nutzung nach dem ersten erfolgreichen Laden
- lokaler Lernfortschritt und Fehlerbuch über localStorage
- Online-/Offline-Status in der App
- Installationshilfe in der Startansicht

## Wichtig
Eine PWA muss über **HTTPS** oder `localhost` ausgeliefert werden.
Das direkte Öffnen von `index.html` über `file://` reicht für Service Worker und Installation nicht.

## GitHub Pages – Kurzweg
1. Neues GitHub-Repository anlegen, z. B. `b1-deutsch-trainer`.
2. Den **Inhalt dieses Ordners** in das Repository hochladen.
3. Repository → **Settings** → **Pages**.
4. Unter *Build and deployment*:
   - Source: `Deploy from a branch`
   - Branch: `main`
   - Folder: `/ (root)`
5. Speichern.
6. Nach kurzer Zeit erscheint die HTTPS-Adresse der App.
7. Diesen Link auf dem Smartphone öffnen.

## Android / Chrome
- App-Link öffnen.
- In der App auf **App installieren** tippen, wenn der Button aktiv ist.
- Alternativ Chrome-Menü → **App installieren** / **Zum Startbildschirm hinzufügen**.

## iPhone / iPad
- App-Link in **Safari** öffnen.
- Teilen-Symbol → **Zum Home-Bildschirm** → **Hinzufügen**.

## Lernstand
Der Lernstand wird lokal auf dem jeweiligen Gerät und Browser gespeichert.
Ein späterer Gerätewechsel überträgt den Stand aktuell noch nicht automatisch.
