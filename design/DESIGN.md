# DESIGN.md – Hitlist (Stil A „Studio“)

Verbindliche Design-Vorgabe. Werte stehen in `tokens.json`, Details (Abstände, Texte, Animationen) in den Prototypen unter `prototypes/`.

## Charakter

- Dunkel, clean, minimalistisch, an Spotify **angelehnt, nicht kopiert** (kein Spotify-Grün, kein Spotify-Logo als Gestaltung, keine Spotify-Schrift).
- Partytauglich: große Knöpfe, mit einer Hand bedienbar, auf Abstand lesbar.
- Viel, aber zielgerichtete Bewegung: federnde Knöpfe, lebendiger Equalizer, ein echter „Moment“ beim Auflösen.
- Spielregel Nr. 1: **Vor dem Auflösen verrät nichts den Song.**

## Grundlagen

| Thema | Vorgabe |
| --- | --- |
| Format | iPhone, Hochformat, Referenz 390 × 844, Safe Areas beachten |
| Modus | Nur dunkel |
| Hintergrund | `bg` #121212; Scanner: radialer Verlauf #2A2A2A → #0C0C0C |
| Flächen | `surface` #1E1E1E (Karten, Sheets), `surface-2` #242424 (Icon-Knöpfe, sekundäre Knöpfe), `surface-3` #2A2A2A (Chips, Kacheln) |
| Text | `text` #FFFFFF, `text-2` #B3B3B3, `text-3` #A0A0A0 |
| Akzent | `accent` #4ADE80, Text darauf `on-accent` #0B0B0B |
| Status | `warning` #F59E0B (Hintergrund #2A2416), `error` #F87171 (Hintergrund #3A1E1E), Erfolg-Hintergrund #1F3326 |
| Schrift | Figtree 400/600/700/800, Zahlen `tabular-nums` |
| Screen-Rand | 56 oben, 20 seitlich, 32 unten; Abstand zwischen Blöcken 20 |
| Touch-Ziele | mind. 44 × 44 |
| Icons | Linien-Icons, Strichstärke 1,8–2, abgerundete Enden; keine Emojis |

## Typografie

| Rolle | Größe / Gewicht | Beispiel |
| --- | --- | --- |
| Jahr (Auflösen) | 96 / 800, Laufweite −4 | 1977 |
| Hero (Start) | 44 / 800, Laufweite −1,5 | Hören. Raten. Auflösen. |
| H1 | 32–34 / 800, Laufweite −1 | Bereit zum Spielen |
| H2 / Sheet-Titel | 24–26 / 800 | Jahr korrigieren |
| Wortmarke | 24 / 800, Laufweite −0,5 | Hitlist |
| Titel | 22–26 / 700 | Dreams |
| Text | 16–17 / 400–600 | |
| Klein | 13–15 | Läuft · 0:42 |
| Abschnittstitel | 13 / 700, Großbuchstaben, Laufweite 0,5, `text-3` | SPOTIFY |
| Hinweis | 11–12, `text-3` | Handy umgedreht hinlegen … |

## Komponenten

| Komponente | Vorgabe |
| --- | --- |
| Primärknopf | Höhe 56, Pille (Radius 28), weiß mit `on-accent`-Text **oder** `accent` mit `on-accent`-Text, 17/700 |
| Sekundärknopf | Höhe 56, Pille, `surface-2`, weißer Text 16/600 |
| Icon-Knopf rund | 44 (Header) bzw. 56 (Player), `surface-2`, Icon 20–22 |
| Play-Knopf | 80 rund, `accent`, Icon 30 in `on-accent` |
| Karte | `surface`, Radius 24 (Hero) bzw. 18–20 (Listen) |
| Listenzeile | Mindesthöhe 60, Trennlinie 1 px #2A2A2A |
| Chip | `surface-3`, Pille, 13/600, Padding 6 × 10 |
| Bottom-Sheet | `surface`, Radius 28 oben, Griff 40 × 5 #444, Inhalt 20 seitlich, 32 unten; Hintergrund dahinter auf 35 % Deckkraft |
| Schalter | 52 × 32, an = `accent`, aus = #3A3A3A, Knopf 26 weiß |
| Stepper (Jahr) | Knöpfe 56 × 56, Radius 16, `surface-3`; Zahl 56/800 in `accent` |
| Eingabefeld | Höhe 48–52, Radius 14, Rahmen #333, Hintergrund `bg` bzw. `surface` |
| Fortschritt (geführter Ablauf) | 4 Segmente, Höhe 5, erledigt = `accent`, offen = #2E2E2E |
| Spotify-Logo | Offizielle Datei von Spotify (weiß/monochrom auf dunkel), nicht nachzeichnen; im Prototyp nur Platzhalter |

## Motion

| Element | Animation | Dauer / Kurve |
| --- | --- | --- |
| Jeder Knopf | Drücken: auf 92–95 % skalieren | 150 ms, cubic-bezier(.3,1.6,.5,1) |
| Equalizer | 7 Balken (Start: 5), Höhe 18 ↔ 92, versetzt, stehen bei Pause | 1,1 s Endlosschleife |
| Auflösen | Karte klappt ein (rotateY 90° → 0, Skalierung .94 → 1), dann Jahr, Quelle, Titel, Interpret, Spotify-Zeile steigen nacheinander 16 px auf | 550 ms, cubic-bezier(.2,1.4,.4,1); Versatz 0,05 / 0,18 / 0,26 / 0,36 s |
| Start-Screen | Überschrift, Text, Liste steigen nacheinander auf | 700 ms, Versatz 0,1 / 0,25 / 0,4 s |
| Bottom-Sheet | Fährt von unten ein | 500 ms, cubic-bezier(.2,1.2,.3,1) |
| Ungültiger Code | Scanner-Rahmen schüttelt | 500 ms |
| Scanner | Scanlinie mit Leuchten fährt auf und ab; Rahmen „atmet“ (Skalierung 1 ↔ 1,03) | 2 s / 2,4 s |
| Gerät gefunden | Punkt pulsiert (Ring wächst auf 3×, blendet aus) | 1,6 s |
| Kein Gerät | Mond-Kachel schwebt 8 px auf und ab | 2,4 s |
| Jahr-Stepper | Zahl federt bei Änderung (Skalierung .85 → 1) | 250 ms |
| Speichern | Ladekreis, dann Bestätigung ploppt auf | 500 ms |
| Schalter | Knopf gleitet mit Feder | 300 ms |
| Geführter Ablauf | Schritt gleitet von rechts ein (24 px) | 450 ms |
| „Bewegung reduzieren“ | Alle Animationen aus, Wechsel sofort | – |

## Screens

Nummern und Dateinamen entsprechen dem Design-Canvas und `prototypes/`.

### 1 Erster Start – `Start.dc.html`
- Wortmarke oben links.
- Mitte: Equalizer (5 Balken), Hero „Hören.↵Raten.↵Auflösen.“, Text „Scanne deine Karten. Hitlist spielt den Song über Spotify ab, ohne Titel, Interpret oder Cover zu zeigen.“
- Karte „DU BRAUCHST“ mit Häkchen: Spotify Premium · Die Spotify-App auf diesem iPhone · Freischaltung durch Jan.
- Unten: Primärknopf weiß „Mit Spotify anmelden“, Textlink „Eigene Client ID verwenden“ → Screen 7.

### 2a Bereit-Check: Gerät gefunden – `Bereit.dc.html`
- Header: Wortmarke + Zahnrad (→ 6).
- H1 „Bereit zum Spielen“, Text „Hitlist spielt die Songs über die Spotify-App auf diesem Gerät ab.“
- Karte mit zwei Zeilen: Gerät („iPhone von Jan“ / „Abspielgerät gefunden“ + pulsierender Punkt) und Konto („Angemeldet als Jan“ / „Spotify Premium“). Gerätename und Kontoname echt aus der API.
- Primärknopf `accent` „Erste Karte scannen“ (→ 3a).
- Der Link „Zustand ‚kein Gerät‘ ansehen“ existiert **nur im Prototyp**.

### 2b Bereit-Check: kein Gerät – `KeinGeraet.dc.html`
- Warn-Kachel (Mond-Icon, `warning`), H1 „Spotify schläft gerade“.
- Text „Dein iPhone ist bei Spotify gerade nicht als Abspielgerät sichtbar. Das passiert, wenn die Spotify-App länger im Hintergrund war.“
- Nummerierte Karte: 1. Tippe auf „Spotify kurz öffnen“. 2. Wechsle zurück zu Hitlist, ohne in Spotify etwas zu starten. 3. Hitlist prüft automatisch erneut.
- Primärknopf `accent` „Spotify kurz öffnen“ (öffnet Spotify-App), Sekundärknopf „Erneut prüfen“ mit Ladekreis „Suche Gerät …“.
- Beim Zurückkehren in die App (Sichtbarkeitswechsel) automatisch neu prüfen.

### 3a Scanner – `Scanner.dc.html`
- Vollbild-Kamera, oben: Schließen (→ 2a), Chip „Karte 13“ (Zähler der Sitzung), Taschenlampe (an = weißer Knopf).
- Mitte: Sucher 260 × 260 mit vier Ecken in `accent` (Radius 20, Strich 4), Scanlinie, darunter „Karte in den Rahmen halten“ / „Der Song startet sofort, ohne Infos.“
- Erfolgreicher Scan → sofort abspielen und zu 4.
- Tippen auf den Sucher und der Link „Ungültigen Code simulieren“ existieren **nur im Prototyp**.

### 3b Ungültiger Code – `ScanUngueltig.dc.html`
- Sucher abgedunkelt, Ecken `error`, schüttelt.
- Bottom-Sheet: Fehler-Kachel, H2 „Das ist kein Song-Code“, Text „Hitlist erkennt nur Spotify-Song-Links von deinen Karten. Playlists, Alben und andere QR-Codes funktionieren nicht.“, Primärknopf „Nochmal scannen“.

### 4 Player und Auflösen – `Main.dc.html`
- Header: Wortmarke, Chip „Karte 12“, Zahnrad (→ 6).
- **Vor dem Auflösen:** Karte (Höhe ca. 350, Radius 24) mit Equalizer in `accent`, „Was läuft hier?“, darunter „Läuft · 0:42“ bzw. „Pausiert · 0:42“ (verstrichene Zeit, nie Gesamtlänge).
- **Nach dem Auflösen:** Karte (Höhe ca. 400) klappt um: Jahr 96/800 in `accent`; Zeile mit Chip „Jahr laut MusicBrainz“ (bzw. „Jahr laut Korrektur“, „Jahr laut Spotify (unsicher)“) und Link „Jahr korrigieren“ (Admin → 5a, sonst → 5b); Titel 26/700; Interpret 17 `text-2`; Trennlinie; links Spotify-Logo, rechts Pillen-Link „In Spotify öffnen“.
- Steuerung: Nochmal (56, Neustart-Icon) · Play/Pause (80, `accent`) · Auflösen (56, Auge-Icon; erneut tippen verbirgt wieder).
- Unten: Primärknopf weiß „Nächste Karte scannen“ (→ 3a), Hinweis „Handy umgedreht hinlegen: Sperrbildschirm zeigt den Titel“.

### 5a Jahr korrigieren (Admin) – `Korrektur.dc.html`
- Bottom-Sheet über abgedunkeltem Player.
- H2 „Jahr korrigieren“ + „Abbrechen“; „Dreams · Fleetwood Mac“, „Bisher: 2004 (Spotify-Albumjahr, unsicher)“.
- Stepper −/+ mit Jahr; Feld „Quelle (optional)“ (Platzhalter „z. B. Single 1977“).
- Primärknopf `accent` „Für alle speichern“ → „Speichere …“ → Erfolg „Gespeichert für alle“ + Link „Zurück zum Song“; Hinweis „Gilt nach 1–2 Minuten auf allen Geräten.“
- Keine Sicherheitsabfrage. Fehler beim Speichern (Netz, Token ungültig, Konflikt) verständlich melden.

### 5b Korrektur vorschlagen (Freunde) – `Vorschlag.dc.html`
- Bottom-Sheet wie 5a, H2 „Korrektur vorschlagen“, Text „Schick die Zeile an Jan. Sobald er sie übernimmt, gilt das Jahr auf allen Geräten.“
- Stepper, Vorschau „Hitlist-Korrektur: Dreams – Fleetwood Mac → 1977 (Track <id>)“.
- Knöpfe „Kopieren“ (→ „Kopiert“) und „Teilen“ (Web Share API).

### 6 Einstellungen – `Einstellungen.dc.html`
- Header: Zurück + „Einstellungen“.
- SPOTIFY: „Angemeldet als Jan“ / „Premium · Login gültig bis April 2027“ + „Abmelden“ (`error`); „Client ID“ / „Standard (Hitlist von Jan)“ bzw. „Eigene“ → 7.
- KORREKTUREN: Schalter „Admin-Modus“ / „Jahre direkt für alle speichern“; wenn an: „GitHub-Schlüssel“ / „Hinterlegt · endet auf 4f2a“ + „Ändern“.
- ÜBER: „Songs und Titel von“ [Spotify-Logo], „Erscheinungsjahre von MusicBrainz“, „Version“.

### 7 Eigene Client ID – `ClientId.dc.html`
- Geführter Ablauf, 4 Segmente, „Schritt X von 4“, Schließen → 6.
- 1 „Spotify-Entwicklerbereich öffnen“ (Knopf „Spotify Dashboard öffnen“ mit externem Link, Erklärung zur 5er-Grenze).
- 2 „App anlegen“ (Liste, Redirect-Adresse mit „Kopieren“, „Web API“ ankreuzen).
- 3 „Client ID einfügen“ (Eingabefeld, kein Client Secret nötig; Format prüfen: 32 Hex-Zeichen).
- 4 „Konten freischalten“ (User Management, max. 5) + Hinweis, dass danach neu angemeldet wird; Knopf `accent` „Speichern und neu anmelden“ → 1.
- Unten: „Zurück“ (ab Schritt 2) und „Weiter“.

### 8 Fehlerzustände – `Fehler.dc.html`
- Vollbild, je Icon-Kachel, H1, Text, Knopf(e). Die Umschaltleiste oben gibt es **nur im Prototyp**.
- Abgemeldet: „Bitte neu anmelden“ / „Deine Spotify-Anmeldung ist abgelaufen. Das passiert alle 6 Monate und ist normal.“ / „Mit Spotify anmelden“.
- Offline: „Keine Verbindung“ / „Hitlist braucht Internet, um Songs über Spotify abzuspielen. Prüfe WLAN oder mobile Daten.“ / „Erneut versuchen“.
- Nicht freigeschaltet: „Konto nicht freigeschaltet“ / „Dein Spotify-Konto ist für diese Hitlist noch nicht eingetragen. Bitte Jan, dich freizuschalten, oder richte eine eigene Client ID ein.“ / „Eigene Client ID einrichten“ + „Erneut versuchen“.
- Zusätzlich (ohne eigenen Prototyp, gleicher Aufbau): kein Premium, Mengenlimit erreicht (429).

## Verworfen

Stilrichtungen B „Vinyl“ und C „Typo“ (liegen nur auf dem Canvas) – nicht verwenden.
