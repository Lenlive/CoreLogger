# NetherGate — Paper 26.3 / Java 25

Separates Plugin: `NetherGate-1.0.0-paper-26.3.jar`.
JAR in `plugins/` kopieren und den Server vollständig neu starten.
ServerBasics muss dafür nicht ersetzt werden.

Der Nether ist bei der ersten Installation für alle Spieler gesperrt, auch
für Admins. `/nether 123` gibt ihn für alle dauerhaft frei. Dafür ist OP
oder die Berechtigung `nethergate.unlock` erforderlich (Standard: op).
Nur die Zahlenfolge zu kennen reicht ohne Berechtigung nicht aus.
Die Konsole kann den Befehl ebenfalls ausführen.

Alle Netherwelten werden über ihren Dimensionstyp erkannt; der Weltname
spielt keine Rolle. Netherportale und Spielerteleports (einschließlich
Plugin-/Befehlsteleports) werden bis zur Freigabe abgebrochen. Overworld
und End bleiben zugänglich. Bereits im Nether befindliche Spieler werden
bei Pluginstart sowie beim Einloggen zum Spawn der ersten geladenen
Overworld zurückgebracht. Loginpositionen und Respawnziele im gesperrten
Nether werden umgeleitet. Ist keine Overworld verfügbar oder eine nötige
Rückkehr blockiert, wird der betroffene Spieler getrennt, damit er nicht
im gesperrten Nether bleibt. Keine regelmäßige Prüfschleife.

Die Freigabe wird vor ihrer Bestätigung in
`plugins/NetherGate/config.yml` unter `nether-unlocked: true` gespeichert
und bleibt bei Neustarts erhalten. Ein falscher Code ändert nichts.
Erneutes manuelles Sperren ist bei gestopptem Server möglich, indem man
dort `nether-unlocked: false` setzt und den Server wieder startet.
Das Plugin zerstört keine Portale und verändert keine Weltdaten.

Build gegen Paper API 26.3.build.224-stable erfolgreich. Separater Testlauf
mit echten Paper-Ereignisobjekten und simulierten Spielern/Welten prüfte:
alle Teleportursachen, Portalblockierung, Overworld-/End-Zugriff,
Berechtigungsprüfung, falschen Code, Login-/Respawn-Umleitung,
Speicherung der Admin-Freigabe und anschließenden Zutritt. JAR-Struktur
geprüft. Kein vollständiger Live-Test auf einem Minecraft-Server.
Andere Plugins, die nachträglich Ereignisse umschreiben/Entsperrungen
überschreiben oder Weltwechsel ohne Bukkit-Ereignisse durchführen, müssen
separat auf Kompatibilität geprüft werden.
