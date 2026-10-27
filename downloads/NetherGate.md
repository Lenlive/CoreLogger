# NetherGate 1.1.1 — Paper 26.3 / Java 25

Separates Plugin: `NetherGate-1.1.1-paper-26.3.jar`.
Server stoppen, alte NetherGate-JAR entfernen, neue JAR in `plugins/` kopieren
und Server vollständig neu starten. ServerBasics muss nicht ersetzt werden.

## Admin-Countdown

Bei der ersten Installation ist der Nether für alle gesperrt.
`/nether 123` startet einen 30-Minuten-Countdown; dafür ist OP oder die
Berechtigung `nethergate.unlock` erforderlich (Standard: op).
Die Konsole kann den Befehl ebenfalls verwenden. Den Code allein zu kennen
reicht ohne Berechtigung nicht aus. Erneutes Eingeben setzt einen laufenden
Countdown nicht zurück. Ein falscher Code verändert nichts.

Alle aktuell verbundenen Spieler sehen den Titel `Nether in 30min.` für
3 Sekunden ohne Ein-/Ausblendzeit. Farben: Lavaorange (#FF6A00),
Netherrackrot (#A8463B), Glutgelb (#FFD54A), fette Titelschrift.
Nach dem Titel erscheint oben eine rote Bossleiste mit 20 Abschnitten,
orange-gelber Restzeitanzeige und abnehmender Füllung. Der Countdown läuft
ab dem Adminbefehl, die 3 Titelsekunden sind darin enthalten.
Später beitretende Spieler erhalten die noch laufende Bossleiste.
Nach 30 Minuten wird der Nether automatisch für alle freigegeben; die
Bossleiste verschwindet und der Titel `NETHER IST OFFEN!` erscheint 3 Sekunden.
Es gibt keine Leiste unten links, keinen Client-Mod und kein Ressourcenpaket.

Ein einzelner Scheduler-Task aktualisiert den Timer einmal pro Sekunde,
ausschließlich solange der Countdown läuft. Es werden keine Chatzeilen
pro Sekunde gesendet und keine Zustandsdateien pro Sekunde geschrieben.

## Neustarts und vorhandene Installation

`plugins/NetherGate/config.yml` enthält:

```yaml
nether-unlocked: false
nether-opens-at: 0
```

Der Start speichert den absoluten Öffnungszeitpunkt in Millisekunden.
Neustarts setzen den Countdown nicht zurück, Offlinezeit zählt mit.
Nach Ablauf während eines Serverstopps erfolgt die Freigabe beim nächsten
Start. Der Zustand wird vor Bestätigung gespeichert; bei einem Speicherfehler
bleibt der Nether gesperrt. Nach der Freigabe steht `nether-unlocked: true`.

**Eine bereits gespeicherte Freigabe aus Version 1.0.0 bleibt bestehen.**
Um einen neuen Countdown vorzubereiten: Server stoppen und in dieser Datei
`nether-unlocked: false` sowie `nether-opens-at: 0` setzen, dann starten.
Ein bereits offener Nether wird nicht durch einen erneuten Befehl gesperrt.

## Sperre

Netherwelten werden anhand ihres Dimensionstyps erkannt, unabhängig vom Namen.
Netherportale und Spielerteleports werden bis zur Freigabe abgebrochen.
Overworld und End bleiben zugänglich. Bereits im Nether befindliche Spieler
werden bei Pluginstart sowie beim Einloggen zum Spawn der ersten geladenen
Overworld zurückgebracht. Loginpositionen und Respawnziele im gesperrten
Nether werden umgeleitet. Ist keine Overworld verfügbar oder eine nötige
Rückkehr blockiert, wird der betroffene Spieler getrennt. Keine Adminumgehung.
Portale und Weltdaten werden nicht zerstört.

## Validierung

Maven-Build und JAR-Prüfung erfolgreich. Separater Test mit echten Paper-
Ereignisobjekten, simulierter Uhr, Scheduler, Spielern und Welten prüfte:
Adminrechte und falschen Code, exakt 3 Sekunden Titeldauer, Bossleiste erst
danach, roten Balken und halbe Füllung nach 15 Minuten, mehrfachen Befehl,
gespeicherten Öffnungszeitpunkt und Restzeit nach Wiederherstellung,
Portalblockierung bis unmittelbar vor 30 Minuten, automatische Freigabe,
Entfernung der Bossleiste und Beendigung des Scheduler-Tasks.
Kein vollständiger Live-Test auf einem Minecraft-Server und kein visueller
Test im Client. Andere Plugins, die Ereignisse nachträglich überschreiben
oder Weltwechsel ohne Bukkit-Ereignisse ausführen, separat prüfen.

## Countdown abbrechen (1.1.1)

`/nether abbruch` beendet einen laufenden Countdown. Erfordert ebenfalls OP
oder `nethergate.unlock`. Die Bossleiste wird für alle entfernt, ein noch
sichtbarer Starttitel gelöscht und der Timer-Task beendet. Der Nether bleibt
gesperrt. Das gilt auch nach Neustarts, da die gespeicherte Öffnungszeit auf
0 gesetzt wird. Bei einem Speicherfehler bleibt der Countdown aktiv und der
Admin erhält eine Fehlermeldung. Mit `/nether 123` kann danach ein neuer
30-Minuten-Countdown gestartet werden. Abbruch ohne laufenden Countdown
ändert nichts; ein bereits offener Nether wird dadurch nicht erneut gesperrt.

Zusätzlich geprüft mit simuliertem Scheduler/Spielern und echter Paper-API:
Abbruch ohne Adminrechte abgelehnt, Bossleiste und Timer entfernt, Sperre
bleibt nach ursprünglicher Ablaufzeit bestehen, Abbruchzustand gespeichert,
wiederholter Abbruch ohne Effekt, neuer Countdown nach Abbruch, Abbruch
während der Titeldauer. Bestehende Countdown-Prüfungen ebenfalls bestanden.
Kein vollständiger Live-Test im Spiel.
