# CLAUDE.md – Hitlist

Hitlist ist eine iPhone-PWA: eigene Spotify-QR-Karten scannen, Song ohne Infos abspielen (Spotify-Fernsteuerung), auf Knopfdruck mit korrektem Jahr auflösen.

## Lies zuerst

1. `PLAN.md` – maßgeblicher Projektplan, Phasen, Datenmodell
2. `design/DESIGN.md` – verbindliche Design-Vorgabe für jeden Screen
3. `design/tokens.json` – Farben, Schrift, Radien, Abstände, Motion-Werte
4. `design/screenshots/` – Gesamteindruck (falls vorhanden)
5. `design/prototypes/` – Quellcode der abgenommenen Screens: präziseste Vorlage für Abstände, Texte, Animationen. **Referenz, nicht kopieren** (anderes Format, kein React).

## Feste Regeln

- **Sprache der Oberfläche:** Deutsch, „du“, locker wie in den Prototypen. Texte aus den Prototypen wörtlich übernehmen.
- **Kein eigener Server, kein Tracking, keine Analytics.** Externe Dienste nur: Spotify Web API, Spotify Accounts (Login), MusicBrainz, GitHub API (nur Admin-Modus).
- **Spotify-Login nur per Authorization Code + PKCE.** Kein Client Secret im Code. Client ID aus Konfiguration bzw. Einstellungen (eigene Client ID).
- **Spotify-Doku 2026 beachten:** Viele Anleitungen und Bibliotheken sind seit Feb./März 2026 veraltet (Endpunkte entfernt, Felder umbenannt, Entwicklungsmodus: Premium-Pflicht, max. 5 Nutzer). Im Zweifel die offizielle Doku prüfen statt aus dem Gedächtnis.
- **Nichts verraten:** Vor dem Auflösen nirgends Titel, Interpret, Cover, Songlänge oder Fortschritt anzeigen – auch nicht in `document.title`, Media Session oder Konsolen-Logs.
- **Spotify-Richtlinien:** Wo Spotify-Daten angezeigt werden, offizielles Spotify-Logo (Datei von Spotify, nicht nachzeichnen) und Link „In Spotify öffnen“. Titel/Interpret unverändert anzeigen. App-Icon darf nicht an Spotify erinnern.
- **Jahres-Logik exakt und getestet:** Quellen-Reihenfolge Korrekturliste → MusicBrainz ISRC → MusicBrainz-Suche → Spotify-Albumjahr; reine Funktionen mit Vitest-Tests inkl. Grenzfällen. MusicBrainz höchstens 1 Anfrage pro Sekunde, Ergebnisse cachen.
- **Korrekturliste** `public/corrections.json` mit `formatVersion`; mit Zod prüfen, fehlerhafte Einträge überspringen statt abstürzen.
- **GitHub-Schlüssel (Admin-Modus):** nur in localStorage, nie anzeigen (außer letzte 4 Zeichen), nie loggen, nur für Schreibzugriff auf `public/corrections.json`.
- **Design:** nur Tokens aus `design/tokens.json`, keine frei erfundenen Farben; nur dunkler Modus; Touch-Ziele mind. 44 px; Bottom-Sheets statt Popups; keine Emojis als Icons; keine Bottom-Navigation.
- **Motion** nach DESIGN.md; `prefers-reduced-motion` → alle Animationen aus, Zustandswechsel sofort.
- **App-Hülle:** kein Gummiband-Scrollen (html/body nicht scrollbar, `overscroll-behavior: none`, `100dvh`, Safe Areas); Zoom per Doppeltipp/Pinch verhindern.
- **GitHub Pages Unterpfad:** Vite `base: '/hitlist/'`, Manifest `scope` und `start_url` passend.
- **Schrift Figtree lokal bündeln** (@fontsource), nicht von Google Fonts laden.

## Arbeitsweise

- Jan lernt mit dem Projekt: kurz erklären, **was** du tust und **warum**, Fachbegriffe beim ersten Mal in einem Satz.
- Phasenweise nach `PLAN.md`, **eine Phase pro Sitzung**.
- **Vor größeren Änderungen** einen Plan zeigen und auf OK warten. Bei Unklarheiten nachfragen statt raten.
- Nach jedem Schritt Tests laufen lassen, dann einen Commit vorschlagen. **Nicht mitten in einer Phase pushen** – jeder Push auf `main` geht sofort live.
- Erledigte Punkte in `PLAN.md` abhaken. Beim Pausieren den Zwischenstand unter „Zwischenstand“ eintragen und lokal committen.
- Jan entwickelt auf **Windows 11** (PowerShell). Befehle passend dazu angeben; Pfade mit Leerzeichen in Anführungszeichen (Projektordner liegt unter `C:\Users\Jan\Documents\Claude Projekte\hitlist`).
- Keine Befehle als Administrator vorschlagen.
