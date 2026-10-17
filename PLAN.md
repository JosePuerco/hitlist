# Hitlist – Projektplan

Stand: 17.10.2026 · maßgebliche Fassung für Claude Code. Erledigte Punkte hier abhaken.

## Überblick

Hitlist ist eine iPhone-Web-App (PWA), die selbstgemachte Spotify-QR-Karten scannt, den Song ohne Titel, Interpret oder Cover abspielt und ihn auf Knopfdruck mit korrektem Erscheinungsjahr auflöst.

| Thema | Entscheidung | Begründung |
| --- | --- | --- |
| Name | Hitlist | Anlehnung an Hitster, ohne den geschützten Namen zu übernehmen |
| Plattform | PWA, nur iPhone (eigenes iPhone 16e und iPhones von Freunden) | Kein Mac, keine 99 €/Jahr, ein Code |
| Weg | Eigene App statt Fork von „Blind Song Scanner“ | Eigenes Design, saubere Basis; das Vorbild-Repo hat keine Lizenz → keinen Code daraus übernehmen |
| Karten | Vorhandene Karten aus generate.blindsongscanner.com, QR = `https://open.spotify.com/track/<id>…` | Kein Neudruck nötig |
| Abspielen | Spotify-App auf dem iPhone wird per Web API ferngesteuert, Ton über Handy-Lautsprecher | Einziger verbleibender Weg (30-s-Vorschauen und Web-Player fallen weg) |
| Login | Spotify-Login per Authorization Code + PKCE direkt im Browser, kein Server | Client ID darf öffentlich sein, kein Secret im Code |
| Nutzer | Alle mit Spotify Premium; bis zu 5 Konten über Jans Client ID, darüber hinaus eigene Client ID je Person | Grenze des Spotify-Entwicklungsmodus |
| Jahr | Kartenrückseite wird ignoriert; App löst auf mit Jahr aus Korrekturliste → MusicBrainz → Spotify | Rückseiten haben falsche Jahre durch Remaster und Compilations |
| Korrekturen | Feste Liste im Repo, gilt nach Veröffentlichung für alle Geräte; Jan speichert direkt aus der App (Admin-Modus), Freunde schlagen vor | Kein Server, trotzdem für alle gleich |
| Umfang v1 | Nur Basics, keine Spielmodi | Bewusst klein starten |
| Hosting | GitHub Pages, `https://josepuerco.github.io/hitlist/` | Bewährt bei Mixli, feste HTTPS-Adresse für den Login |
| Design | Stil A „Studio“: dunkel, clean, an Spotify angelehnt (nicht kopiert), viel Motion | Abgenommen am 17.10.2026 |

## Funktionsumfang v1

### Spielablauf

1. **Erster Start:** Kurze Erklärung, dann „Mit Spotify anmelden“. Login gilt bis zu 6 Monate, danach erneut.
2. **Bereit-Check:** App prüft, ob ein Spotify-Abspielgerät erreichbar ist (normalerweise das eigene iPhone).
    - Kein Gerät gefunden → Screen „Spotify schläft gerade“ mit Knopf „Spotify kurz öffnen“ (öffnet die Spotify-App, danach zurück zu Hitlist) und „Erneut prüfen“.
    - Nur **vor** dem Scannen anbieten, sonst sieht man in Spotify den neuen Song.
3. **Scannen:** Kamera-Ansicht, Karte in den Rahmen halten; Taschenlampe schaltbar.
    - Nur Spotify-**Track**-Links werden akzeptiert; Playlist, Album, fremde Codes → Bottom-Sheet „Das ist kein Song-Code“.
4. **Hören:** Song startet sofort. Bildschirm zeigt nichts zum Song (keine Länge, kein Fortschritt), nur verstrichene Zeit und:
    - Play/Pause
    - Nochmal von vorn
    - Auflösen
    - Nächste Karte scannen
5. **Auflösen:** Jahr groß, darunter Quelle des Jahres, Titel, Interpret.
    - Unsicheres Jahr (Quelle Spotify oder MusicBrainz-Suche) wird dezent markiert.
    - Link „Jahr korrigieren“.
    - Spotify-Logo als Quellenangabe und Link „In Spotify öffnen“ (Pflicht laut Spotify-Richtlinien, erst nach dem Auflösen).

### Jahreskorrektur

- **Reihenfolge der Quellen:** 1. Korrekturliste der App, 2. MusicBrainz per ISRC, 3. MusicBrainz-Suche nach Interpret + Titel, 4. Spotify-Albumjahr (immer als unsicher markiert).
- Das Jahr wird schon beim Scannen im Hintergrund geholt, damit „Auflösen“ sofort reagiert.
- **Admin-Modus (nur Jans iPhone):** GitHub-Schlüssel, der nur dieses eine Repo ändern darf. Die App schreibt die Korrektur direkt in `public/corrections.json`, GitHub veröffentlicht automatisch, nach 1–2 Minuten gilt sie auf allen Geräten; auf Jans Handy sofort.
- **Freunde:** „Korrektur vorschlagen“ erzeugt eine Zeile (Titel, Interpret, Jahr, Track-ID) zum Kopieren/Teilen an Jan.
- Titel und Interpret werden **nie** korrigiert (Spotify-Richtlinie).

### Einstellungen

- Angemeldetes Spotify-Konto, Ablauf des Logins, Abmelden
- **Eigene Client ID** (optional) mit geführter Anleitung in 4 Schritten inkl. Redirect-Adresse zum Kopieren
- Admin-Modus an/aus, GitHub-Schlüssel hinterlegen/ändern
- Über: Spotify-Logo (Quelle Songs), MusicBrainz (Quelle Jahre), Version
- Hinweis „Handy umgedreht hinlegen“ auf dem Player: Sperrbildschirm und Kontrollzentrum zeigen den Titel weiterhin

### Bewusst nicht in v1

- Spielmodi (Herkunftsland, Band/Solo, Cover/Original usw.)
- Punktestand, Timer, zufälliger Startpunkt, Auto-Stopp
- Eigener Karten-Generator
- Android

## Tech-Stack

| Bereich | Wahl | Wofür |
| --- | --- | --- |
| Grundgerüst | React + TypeScript + Vite | Wie Mixli |
| Styling | Tailwind CSS mit Design-Tokens aus `design/tokens.json` | Feste Farben, Abstände, Schrift |
| Animation | Motion (motion/react) | Übergänge, Auflösen-Effekt |
| PWA/Offline | vite-plugin-pwa | Installierbar, App-Hülle offline |
| QR-Scanner | Kamera-Bibliothek (z. B. `qr-scanner`) | Safari hat keinen eingebauten QR-Leser für Web-Apps |
| Speicher | localStorage (Einstellungen, Login, Jahres-Cache) | Keine verwalteten Daten → keine Datenbank nötig |
| Validierung | Zod | Korrekturliste, Antworten der APIs prüfen |
| Tests | Vitest | Link-Erkennung, Jahres-Logik, Token-Ablauf |
| Schrift | Figtree, lokal gebündelt (@fontsource) | Lädt auch offline |

### Spotify Web API

- **Login:** Authorization Code mit PKCE, Scopes `user-read-playback-state` und `user-modify-playback-state`. Token automatisch erneuern; nach 6 Monaten ist ein neuer Login nötig.
- **Endpunkte:** `GET /me/player/devices`, `PUT /me/player/play` (Track-URI + `device_id`), `PUT /me/player/pause`, `GET /tracks/{id}` (Titel, Interpret, Albumjahr, `external_ids.isrc`).
- **Voraussetzungen:** Entwickler-App im Spotify-Dashboard mit Jans Premium-Konto; Redirect-Adresse exakt `https://josepuerco.github.io/hitlist/` plus lokale Adresse für die Entwicklung; jedes Konto einzeln unter „User Management“ freischalten, max. 5.
- **Achtung:** Spotify hat 2026 mehrfach Regeln geändert (u. a. Feb./März 2026: Endpunkte entfernt, Felder umbenannt; Juli 2026: Quoten pro Entwicklerkonto, 429 mit `reason: QUOTA_EXCEEDED`). Anleitungen von vor Februar 2026 sind oft veraltet; im Zweifel die offizielle Doku und das Changelog prüfen.
- **Design-Richtlinien:** Wo Spotify-Daten angezeigt werden, offizielles Spotify-Logo + Link „In Spotify öffnen“; Metadaten unverändert anzeigen; App-Icon/Logo darf nicht an Spotify erinnern.

### MusicBrainz

- Freie Musikdatenbank, ohne Login, aus dem Browser abfragbar (`https://musicbrainz.org/ws/2/…?fmt=json`).
- Abfrage per ISRC → Erstveröffentlichungsdatum der Aufnahme (`first-release-date`); Fallback: Suche nach Interpret + Titel, ältestes plausibles Datum (Live-Versionen, Cover, Karaoke, Remixe ausschließen).
- **Höchstens 1 Anfrage pro Sekunde**, Ergebnisse pro Track zwischenspeichern.
- Grenzen: Remaster mit neuem ISRC und lückenhafte Daten bei weniger bekannten Songs → deshalb Sicherheits-Markierung und Korrekturliste.

### GitHub (nur Admin-Modus)

- Fine-grained Token, nur für das Repo `hitlist`, nur Recht „Contents: Read and write“.
- Liegt nur auf Jans iPhone; die App ändert damit ausschließlich `public/corrections.json` (GitHub Contents API, mit `sha` gegen gleichzeitige Änderungen).
- Jeder Push löst das automatische Deployment aus.

### Hinweise für Entwicklung und Test

- Unterpfad: Vite `base: '/hitlist/'`, Manifest `scope` und `start_url` passend.
- Kamera und Login brauchen HTTPS; am Laptop reicht `localhost` (bzw. `127.0.0.1`, je nach Spotify-Redirect-Regeln), am iPhone über die GitHub-Pages-Version testen.
- Spotify-Abspielen nur mit echtem Premium-Konto testbar, keine Simulation.

## Datenmodell

Es gibt nur eine gemeinsame Datei (die Korrekturliste im Repo) und drei lokale Speicherbereiche pro Gerät; alles ist über die Spotify-Track-ID verknüpft.

### Korrekturliste `public/corrections.json` (im Repo, für alle)

| Feld | Typ | Hinweis |
| --- | --- | --- |
| `formatVersion` | Zahl | Startet bei 1, für spätere Änderungen am Format |
| `corrections` | Objekt, Schlüssel = Spotify-Track-ID | Ein Eintrag pro korrigiertem Song |
| `…year` | Zahl | Korrektes Erscheinungsjahr, Pflicht |
| `…note` | Text, optional | Woher das Jahr stammt, z. B. „Single 1977“ |
| `…updatedAt` | Datum (ISO) | Wann korrigiert |

Die Liste wird mit Zod geprüft; ein fehlerhafter Eintrag darf die App nicht lahmlegen (wird übersprungen und gemeldet).

### Lokal pro Gerät (localStorage)

| Bereich | Inhalt | Hinweis |
| --- | --- | --- |
| `auth` | Access Token, Refresh Token, Ablaufzeit, verwendete Client ID | Wird bei Abmelden gelöscht |
| `settings` | Eigene Client ID (optional), Admin-Modus an/aus, GitHub-Schlüssel (nur Admin) | GitHub-Schlüssel nie anzeigen (nur letzte 4 Zeichen), nie loggen |
| `yearCache` | Pro Track-ID: Jahr, Quelle (`correction`, `mb-isrc`, `mb-search`, `spotify`), sicher ja/nein, Abrufzeit | Spart MusicBrainz-Anfragen; Korrekturliste hat immer Vorrang |

### Regeln

- **Abgeleitet, nicht gespeichert:** Das angezeigte Jahr wird bei jedem Auflösen aus den Quellen bestimmt (Korrekturliste schlägt Cache).
- **Logik als reine Funktionen** in eigenem Modul mit Tests: Link erkennen, Jahr aus MusicBrainz-Antwort bestimmen, Quelle wählen.
- **Kein Backup nötig:** Verlorene lokale Daten bedeuten nur neu anmelden.

## Design und UI

Verbindlich ist `design/DESIGN.md` mit `design/tokens.json` und den Prototypen in `design/prototypes/`. Kurzfassung: Stil A „Studio“ – dunkel (#121212), Karten #1E1E1E, Akzent Mintgrün #4ADE80, Schrift Figtree, runde Knöpfe, animierter Equalizer, Auflösen als umklappende Karte, keine Bottom-Navigation.

## Phasenplan

Phase 0 beginnt mit einem Machbarkeitstest; erst wenn der klappt, wird das Fundament gebaut. Reihenfolge: Test → Fundament → Spotify → Kernablauf → Auflösen → Freunde → Feinschliff.

### Phase 0 – Machbarkeitstest und Fundament

- [ ] Spotify-Entwickler-App anlegen, Redirect-Adressen eintragen
- [ ] Mini-Test: Login per PKCE, Geräte abfragen, festen Track auf dem iPhone abspielen, **während Spotify im Hintergrund ist** und der Bildschirm kurz gesperrt war
- [ ] Mini-Test: Kamera in der installierten Web-App – wie oft fragt iOS nach der Erlaubnis?
- [ ] Mini-Test: MusicBrainz mit 10–20 echten Karten, Trefferquote des richtigen Jahres notieren
- [ ] Ergebnisse unten unter „Ergebnisse Machbarkeitstest“ festhalten, Entscheidung weiter/Plan B
- [ ] Projekt aufsetzen, PWA unter `/hitlist/` installierbar und offline, Schrift lokal
- [ ] App-Hülle ohne Gummiband-Scrollen, Navigation, Bausteine mit Vorschauseite
- [ ] GitHub Pages per Actions, Installation und Offline-Start am iPhone getestet

### Phase 1 – Spotify-Anbindung

- [ ] Login, Token-Erneuerung, Abmelden, abgelaufene Anmeldung sauber abfangen
- [ ] Bereit-Check mit Geräteliste, „Spotify kurz öffnen“-Hilfe
- [ ] Fehlerzustände: kein Premium, nicht freigeschaltet, kein Internet, Mengenlimit erreicht

### Phase 2 – Kernablauf (MVP)

- [ ] Link-Erkennung als reine Funktion mit Tests (Track ja; Playlist, Album, fremd nein)
- [ ] Scanner-Screen inkl. „Ungültiger Code“
- [ ] Abspielen, Player-Screen mit Play/Pause, Nochmal, Nächste Karte
- [ ] Test im echten Spiel mit 10 Karten

### Phase 3 – Auflösen und Jahre

- [ ] Track-Infos inkl. ISRC laden
- [ ] Jahres-Logik als reine Funktionen mit Tests (Quellen-Reihenfolge, MusicBrainz-Auswertung, Ausschluss Live/Cover)
- [ ] Korrekturliste laden und prüfen, Cache
- [ ] Auflösen-Screen mit Sicherheits-Markierung, Spotify-Logo, „In Spotify öffnen“
- [ ] Jahr korrigieren: Admin-Modus mit GitHub-Schlüssel, „Korrektur vorschlagen“ für alle anderen

### Phase 4 – Freunde

- [ ] Eigene Client ID in den Einstellungen
- [ ] Geführte Anleitung in der App zum Anlegen einer eigenen Spotify-Entwickler-App
- [ ] Test mit einem Freund und dessen iPhone

### Phase 5 – Feinschliff

- [ ] Motion nach DESIGN.md, App-Icon (kein grüner Kreis), Splashscreen, Name im Manifest
- [ ] Hinweis „Handy umdrehen“
- [ ] Vorschauseite im Build ausblenden
- [ ] Alle Punkte abgehakt, Kurzanleitung für Freunde

### Später (eigene Phasen, noch nicht geplant)

- Spielmodi mit MusicBrainz-Daten (Herkunftsland, Band/Solo, Original/Cover, Alter bei Erscheinen, Jahrzehnt)
- Eigener Karten-Generator mit Jahresprüfung vor dem Druck
- Android

## Offene Fragen

- [ ] Klappt Abspielen mit Spotify im Hintergrund am iPhone? (Ergebnis Mini-Test, vor Phase 1)
- [ ] Reicht die MusicBrainz-Trefferquote? (Ergebnis Mini-Test, vor Phase 3)
- [ ] Plan B, falls das iPhone im Hintergrund nicht erreichbar ist (erst nach Mini-Test)

## Ergebnisse Machbarkeitstest

_(wird in Phase 0 ausgefüllt)_

## Zwischenstand

_(hier trägt Claude Code beim Pausieren den aktuellen Stand ein)_
