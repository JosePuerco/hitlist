# Prototypen (Referenz, nicht kopieren)

Quellcode der abgenommenen Screens aus dem Design-Canvas. Format: „Design Component“-HTML (Markup in `<x-dc>`, Logik in `class Component extends DCLogic`). Das ist **kein React** und läuft nicht eigenständig.

Nutze die Dateien als präziseste Vorlage für:
- Texte (wörtlich übernehmen)
- Abstände, Größen, Radien, Farben (alle auch in `../tokens.json`)
- Animationen (`@keyframes` im `<style>`-Block) und Zustände (in `renderVals()`)

Nur im Prototyp vorhanden, **nicht** in die App übernehmen: Link „Zustand ‚kein Gerät‘ ansehen“ (2a), Tippen auf den Sucher und „Ungültigen Code simulieren“ (3a), die Umschaltleiste „VORSCHAU“ (8), der `[Spotify-Logo]`-Platzhalter (durch die offizielle Datei ersetzen), Beispieldaten (Dreams – Fleetwood Mac, Karte 12, „Jan“, „4f2a“).

| Datei | Screen |
| --- | --- |
| `Start.dc.html` | 1 Erster Start |
| `Bereit.dc.html` | 2a Bereit-Check: Gerät gefunden |
| `KeinGeraet.dc.html` | 2b Bereit-Check: kein Gerät |
| `Scanner.dc.html` | 3a Scanner |
| `ScanUngueltig.dc.html` | 3b Ungültiger Code |
| `Main.dc.html` | 4 Player und Auflösen |
| `Korrektur.dc.html` | 5a Jahr korrigieren (Admin) |
| `Vorschlag.dc.html` | 5b Korrektur vorschlagen (Freunde) |
| `Einstellungen.dc.html` | 6 Einstellungen |
| `ClientId.dc.html` | 7 Eigene Client ID |
| `Fehler.dc.html` | 8 Fehlerzustände |
