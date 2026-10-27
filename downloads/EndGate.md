# EndGate 1.0.0 — Paper 26.3 / Java 25

Separates Plugin: `EndGate-1.0.0-paper-26.3.jar`.
JAR in `plugins/` kopieren und den Server vollständig neu starten.
NetherGate und ServerBasics werden nicht ersetzt und bleiben unabhängig.

## Befehle

- `/end 123`: startet beide Phasen.
- `/end abbruch`: beendet einen laufenden Zeitplan in jeder der beiden Phasen.

Nur OPs, die Serverkonsole oder Spieler mit `endgate.unlock` dürfen diese
Befehle verwenden (Standard: op). Die Zahlenfolge allein ist keine Freigabe.

## Ablauf

Das End ist zunächst für alle gesperrt, auch für Admins.
Beim Start sehen alle verbundenen Spieler für 3 Sekunden den Titel
`ENDPORTAL SUCHE STARTET IN1H.`. Anschließend zählt oben eine violette
Bossleiste 60 Minuten bis zum Beginn der Endportal-Suche herunter.
Design: Enderviolett (#B16CFF), Endstein (#E2DEA4), Türkis (#75E5D8).

Nach genau 60 Minuten erscheint der Titel `ENDPORTAL SUCHE STARTET JETZT!`.
Der zweite Timer läuft 15 Minuten bis zur End-Öffnung. Nach dem kurzen
Phasentitel erscheint die Bossleiste für diese zweite Phase. Erst nach
insgesamt 75 Minuten wird das End für alle freigegeben; Leiste und Task
werden entfernt und `DAS END IST OFFEN!` wird angezeigt.
Titeldauern sind in den Phasen enthalten, keine zusätzlichen Wartezeiten.
Die Suchphase ist eine Ankündigung: Das Plugin verändert keine Strongholds,
versteckt keine Portalräume und verhindert kein früheres Erkunden.

Erneutes `/end 123` verändert einen laufenden Zeitplan nicht. Abbruch entfernt
Bossleiste und einen noch sichtbaren Phasentitel; das End bleibt gesperrt.
Danach startet `/end 123` einen neuen vollständigen 75-Minuten-Zeitplan.
Abbruch ohne laufenden Zeitplan ist wirkungslos; ein bereits offenes End
wird dadurch nicht erneut gesperrt.

## Speicherung und Zugang

`plugins/EndGate/config.yml` speichert `end-unlocked`, `end-search-at` und
`end-opens-at`. Absolute Zeitpunkte erhalten beide Phasen über Neustarts;
Offlinezeit zählt mit. Nach einer längeren Offlinezeit springt das Plugin
beim Start in die passende Phase beziehungsweise gibt nach Ablauf frei.
Bei Speicherfehlern wird kein Start/Abbruch/Entsperren als erfolgreich bestätigt.

Alle Endwelten werden über `Environment.THE_END` erkannt. Portale und
Spielerteleports bleiben während beider Phasen gesperrt. Login-/Respawnziele
im gesperrten End werden zum Spawn der ersten Overworld umgeleitet.
Bereits im End befindliche Spieler werden zurückgebracht; ist das unmöglich,
werden sie getrennt. Overworld und Nether werden von EndGate nicht gesperrt.
Die separate Sperre durch NetherGate bleibt davon unberührt.

Nur während des Zeitplans läuft ein Task mit einer Aktualisierung pro Sekunde.
Keine Dateischreibvorgänge pro Sekunde, keine Chatspam-Schleife.
Andere Plugins, die Teleportereignisse nachträglich überschreiben oder
Weltwechsel ohne Bukkit-Ereignisse ausführen, separat auf Kompatibilität prüfen.

## Validierung

Maven-Build und JAR-Struktur geprüft. Separater Test mit echter Paper-API,
simulierten Spielern/Welten, Scheduler und Uhr prüfte: Adminrechte/falschen
Code, genaues Starttitel-Motiv und 3-Sekunden-Dauer, alle Teleportursachen,
Portalblockierung, 60-Minuten-Grenze, Wechsel zum 15-Minuten-Timer,
violette Bossleiste und halbe Füllung in beiden Phasen, gespeicherte Termine,
Wiederherstellung der Restzeit, automatische Freigabe erst ab Minute 75,
Entfernung des Timers sowie adminpflichtigen, persistierten Abbruch in beiden
Phasen. Kein vollständiger Live-Test auf einem Minecraft-Server.
