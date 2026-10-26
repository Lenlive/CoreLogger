# Paper 26.3 Plugins

Requires Paper 26.3 and Java 25. Built against Paper API 26.3.build.224-stable.

- CoreLogger: player block placement/removal and container-change audit log in SQLite. `/cl inspect`, `/cl lookup [radius] [hours] [player]`. Inspection permission `corelogger.inspect` defaults to operators. No rollback; not a full CoreProtect replacement.
- QuartzReturn: shapeless crafting recipe, one normal quartz block to four nether quartz.
- Paper-26.3-Plugins.zip contains both plugins under the names CoreLogger.jar and QuartzReturn.jar.

Put both JAR files in the Paper server's plugins directory and restart.
Compilation and the packaged SQLite driver's persistent write/read check passed.
Live Minecraft server behavior has not been tested.

SHA-256:

```
412f0bf80130a20cc2735765ef142b8969509d8fe753540f9fc084bd2b00de28  CoreLogger-1.0.0-paper-26.3.jar
8011d3fce9c09916b65719615c5e9093655d658d16784d05f9096eb821f1eb9e  QuartzReturn-1.0.0-paper-26.3.jar
```
