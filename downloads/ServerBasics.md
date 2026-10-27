# ServerBasics für Paper 26.3 / Java 25

Datei: `ServerBasics-1.0.4-paper-26.3.jar`

Die JAR in `plugins/` kopieren und den Server vollständig neu starten.
Alle drei Befehle sind standardmäßig für alle Spieler freigegeben:

- `/sit`: Hinsetzen; mit Schleichen oder erneutem `/sit` aufstehen. Nur auf dem Boden möglich.
- `/spawn`: Nur den ausführenden Spieler zu `0 240 -31` in die Hauptwelt teleportieren.
- `/all <Text>`: Gelbe Chatnachricht mit `[ALL] Spielername: Text` an alle Spieler senden.

Berechtigungen: `serverbasics.sit`, `serverbasics.spawn`, `serverbasics.all`.
Standardmäßig sind alle auf `true`. Ein Rechteplugin kann sie überschreiben.
Bei Befehlskonflikten funktionieren `/serverbasics:sit`, `/serverbasics:spawn`
und `/serverbasics:all`.

Konfiguration unter `plugins/ServerBasics/config.yml` nach dem ersten Start:
`spawn-world` leer = erste geladene Welt (Hauptwelt); optional genauen Weltnamen
angeben. Koordinaten und der Abstand zwischen Ankündigungen (standardmäßig
3 Sekunden pro Spieler) sind einstellbar. Nach Änderungen Server neu starten.
Das Plugin erstellt keine Spawnplattform; bei Y=240 muss ein geeigneter
Standplatz vorhanden sein, sonst fällt der Spieler nach dem Teleport.

Sitz-Entities werden beim Aufstehen, Verlassen, Tod, Teleport und beim
Deaktivieren entfernt und nicht gespeichert. Keine regelmäßige Sitz-Task,
keine Datenbank. Spawnchunks werden asynchron geladen.

Validierung: Maven-Build gegen Paper API 26.3.build.224-stable erfolgreich;
JAR-Struktur, API-Deklaration, Java-25-Bytecode und Standardberechtigungen geprüft.
Noch kein Live-Test auf einem Minecraft-Server: Der Serverdownload ist im
Cloud-Netzwerk gesperrt. Keine Aussage über gemessene TPS oder Lasttests.

## Version 1.0.1

Sitzhöhe korrigiert: Der Marker-Sitz liegt jetzt 0,15 Blöcke über der
ursprünglichen Fußposition statt 1,7 Blöcke darunter. Vor dem Hinsetzen
wird freier Platz für die Spielerhöhe geprüft. Bei Kollision wird der
Befehl abgebrochen. Kein zusätzlicher periodischer Task.
Build und JAR-Prüfung erfolgreich, noch kein Live-Test im Spiel.
Die alte Version 1.0.0 wegen dieses Fehlers nicht mehr verwenden.

## Version 1.0.2

`/spawn` funktioniert ausschließlich, wenn der ausführende Spieler in einer
Overworld (Bukkit Environment.NORMAL) ist. Im Nether, End oder benutzerdefinierten
anderen Dimensionstypen wird der Befehl mit einer Meldung abgelehnt. Auch die
konfigurierte Zielwelt muss eine Overworld sein. Es gibt ausschließlich
`/spawn`, keinen Alias. Alle drei Befehle bleiben standardmäßig
für alle Spieler freigegeben. Die Sitzhöhenkorrektur aus 1.0.1 ist enthalten.
Maven-Build und JAR-Prüfung erfolgreich; noch kein Live-Test im Spiel.

## Version 1.0.3

`/all Admin <Text>` sendet den Text in Helllila und Fett mit Sternsymbolen.
`/all Admin` allein sendet das Wort Admin in diesem Stil. Groß-/Kleinschreibung
beim Schlüsselwort Admin ist egal. Normales `/all <Text>` bleibt gelb.
Nur OPs oder Spieler mit `serverbasics.all.admin` dürfen Admin-Ankündigungen
senden (Standard: op). Normales `/all` bleibt für alle Spieler verfügbar.
Der Absender bleibt sichtbar.
Minecraft unterstützt kein echtes Leuchten/Hintergrundlicht in Chatnachrichten.
Kein Blinkeffekt, keine wiederholten Nachrichten und keine neuen periodischen
Tasks. Bisherige Spawn-Einschränkung und Sitzhöhenkorrektur bleiben enthalten.

Build und JAR-Prüfung erfolgreich. Formatierung mit echter Adventure-API geprüft:
Normaltext gelb, Admintext lila und fett, Schlüsselwort ohne Beachtung der
Großschreibung, Admin ohne Folgetext, leere Eingabe und literale Formatierungstags.
Kein Live-Test auf einem Minecraft-Server.

## Version 1.0.4

`/spawn` hat jetzt für jeden Spieler (auch OPs) 10 Minuten Cooldown ab
einem erfolgreichen Teleport. Fehlgeschlagene oder abgelehnte Versuche
starten keinen Cooldown. Bei Wiederholung erscheint die verbleibende
Wartezeit in Minuten und Sekunden. Kein neuer Alias und keine Umgehungsrechte.
Die Overworld-Beschränkung und alle bisherigen Funktionen bleiben enthalten.

Cooldowns werden unter `plugins/ServerBasics/spawn-cooldowns.properties`
gespeichert und bleiben bei Ausloggen und Neustart erhalten. Ein einzelner
Hintergrund-Writer ersetzt die Datei atomar (falls vom Dateisystem unterstützt);
bei normalem Stoppen wird auf ausstehende Schreibvorgänge gewartet. Der Server-
Hauptthread führt nach Teleports keine Dateischreibvorgänge aus. Cooldowns
laufen auch während Offlinezeit ab.

Build und JAR geprüft. Cooldown-Komponente mit kontrollierter Uhr geprüft:
Spielerunabhängigkeit, 10-Minuten-Grenze, Restzeitrundung, Wiederherstellung
nach erneuter Dateiladung, Ersetzung/Entfernung abgelaufener Einträge und
Ablehnung beschädigter Daten. Noch kein vollständiger Live-Test im Spiel.
