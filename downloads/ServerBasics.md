# ServerBasics für Paper 26.3 / Java 25

Datei: `ServerBasics-1.0.0-paper-26.3.jar`

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
