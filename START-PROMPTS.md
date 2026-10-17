# Start-Prompts für Claude Code

Vor jeder Phase: vorige Phase in PLAN.md abgehakt, committet, gepusht, `git status` sauber, dann `/clear` und den passenden Prompt einfügen.

## Phase 0 – Machbarkeitstest und Fundament

```
Lies CLAUDE.md, PLAN.md und alles in design/. Nur Phase 0.

Teil A – Machbarkeitstest (vor allem anderen):
1. Erkläre mir Schritt für Schritt, was ich im Spotify-Developer-Dashboard anlegen und eintragen muss (Redirect-Adressen für lokal und für https://josepuerco.github.io/hitlist/).
2. Schlag mir eine schlichte Test-Seite vor (ohne Design), die: per PKCE anmeldet, die Spotify-Geräte auflistet, einen festen Track auf einem gewählten Gerät abspielt, und für eine Liste von Track-IDs das Jahr aus Spotify und aus MusicBrainz (ISRC, Fallback Suche) nebeneinander anzeigt.
   Zeig mir zuerst den Plan, warte auf mein OK.
3. Nach dem Bau: Erkläre mir, wie ich die Test-Seite auf GitHub Pages bringe, damit ich am iPhone teste. Gib mir eine Checkliste für die Tests (Spotify im Hintergrund, Bildschirm gesperrt, Kamera-Erlaubnis in der installierten Web-App, MusicBrainz-Trefferquote mit 10–20 Karten).
4. Wir tragen die Ergebnisse gemeinsam unter „Ergebnisse Machbarkeitstest“ in PLAN.md ein und entscheiden: weiter oder Plan B.

Teil B – Fundament (erst nach meinem OK aus Teil A):
5. Schlag Ordnerstruktur und die Übernahme von design/tokens.json in Tailwind vor, warte auf mein OK.
6. Projekt aufsetzen: React + TypeScript + Vite, PWA unter /hitlist/ installierbar und offline, Figtree lokal gebündelt, App-Hülle ohne Gummiband-Scrollen und ohne Zoom.
7. Leere Screens mit Verlinkung nach DESIGN.md, Bausteine (Knöpfe, Karte, Bottom-Sheet, Schalter, Stepper) auf einer Vorschauseite.
8. GitHub Pages per Actions einrichten und erklären, was ich in den Repo-Einstellungen aktivieren muss.
```

## Phase 1 – Spotify-Anbindung

```
Phase 0 ist fertig. Starte Phase 1 aus PLAN.md. Halte dich an DESIGN.md (Screens 1, 2a, 2b, 8) und die Prototypen Start.dc.html, Bereit.dc.html, KeinGeraet.dc.html, Fehler.dc.html.
Zeig mir zuerst, wie du Login, Token-Erneuerung und Fehlerbehandlung aufteilst (Module, Funktionen, Tests), dann warte auf mein OK.
```

## Phase 2 – Kernablauf (MVP)

```
Phase 1 ist fertig. Starte Phase 2 aus PLAN.md. Halte dich an DESIGN.md (Screens 3a, 3b, 4) und die Prototypen Scanner.dc.html, ScanUngueltig.dc.html, Main.dc.html.
Zeig mir zuerst die Funktion zur Link-Erkennung mit allen Testfällen (inkl. Grenzfällen wie ?si=-Parameter, spotify:track:-URIs, Playlist-, Album- und fremde Links), dann warte auf mein OK. Erst danach die Oberfläche.
```

## Phase 3 – Auflösen und Jahre

```
Phase 2 ist fertig. Starte Phase 3 aus PLAN.md. Halte dich an DESIGN.md (Screens 4, 5a, 5b) und die Prototypen Main.dc.html, Korrektur.dc.html, Vorschlag.dc.html.
Zeig mir zuerst die Jahres-Logik als reine Funktionen mit Tests (Quellen-Reihenfolge, MusicBrainz-Auswertung, Ausschluss von Live/Cover/Remix, unsicher-Markierung) und das Format von public/corrections.json mit Zod-Schema. Warte auf mein OK, dann Oberfläche und Admin-Modus.
Erkläre mir am Ende, wie ich den GitHub-Fine-grained-Token mit minimalen Rechten anlege.
```

## Phase 4 – Freunde

```
Phase 3 ist fertig. Starte Phase 4 aus PLAN.md. Halte dich an DESIGN.md (Screens 6, 7) und die Prototypen Einstellungen.dc.html, ClientId.dc.html.
Zeig mir zuerst, wie die eigene Client ID gespeichert und beim Login verwendet wird und was beim Wechsel passiert, dann warte auf mein OK.
```

## Phase 5 – Feinschliff

```
Phase 4 ist fertig. Starte Phase 5 aus PLAN.md. Vergleiche alle Screens mit den Prototypen und der Motion-Tabelle in DESIGN.md und liste mir Abweichungen auf, bevor du etwas änderst. Schlag mir 2–3 App-Icon-Ideen vor (kein grüner Kreis, keine Wellen).
```
