# Java

Generisches Java-Egg aus den [Pelican Eggs](https://github.com/pelican-eggs/eggs/tree/master/generic/java).
Startet eine beliebige `.jar` — auch Kotlin-Apps laufen damit.

| Egg-Datei | `egg-java.json` |
| --- | --- |
| Name im Panel | Generic Java |
| Quelle | https://github.com/pelican-eggs/eggs (`generic/java`) |
| Autor (Original) | sneaky@sneakyhub.com |

## Images

| Label | Image |
| --- | --- |
| Java 8 | `ghcr.io/parkervcp/yolks:java_8` |
| Java 11 | `ghcr.io/parkervcp/yolks:java_11` |
| Java 16 | `ghcr.io/parkervcp/yolks:java_16` |
| Java 17 | `ghcr.io/parkervcp/yolks:java_17` |
| Java 19 | `ghcr.io/parkervcp/yolks:java_19` |
| Java 21 | `ghcr.io/parkervcp/yolks:java_21` |

Java 16 und 19 sind EOL und bekommen keine Updates mehr. Für neue Server
Java 21 (oder das passende Major deiner Anwendung) wählen.

## Variablen

| Variable | Default | Zweck |
| --- | --- | --- |
| `JARFILE` | `sneakyhub.jar` | Dateiname des Jars im Server-Hauptverzeichnis |

Startup: `java -Dterminal.jline=false -Dterminal.ansi=true -jar {{JARFILE}}`

## Startup-Erkennung

`config.startup.done` steht auf `""` (Upstream: Platzhalter `change this text`).
Ein leeres Muster matcht jede Konsolenzeile — der Server gilt also als *running*,
sobald er die erste Zeile ausgibt.

Für eine präzisere Erkennung auf eine Zeile setzen, die deine Anwendung beim Start
ausgibt, z. B. `"done": ["Started", "Listening on"]` (Array = mehrere Muster).

## ⚠️ Vor dem ersten Einsatz anpassen

1. **Install-Skript ist unvollständig.** Es installiert nur `curl` und `jq` und
   wechselt nach `/mnt/server` — danach bricht es ab, es wird nichts heruntergeladen
   oder geklont. Ein Serverstart ohne vorher hochgeladenes Jar scheitert daher.
   Das ist der Stand im Upstream-Egg, kein Importfehler.
2. **`JARFILE`-Default** ist `sneakyhub.jar` und hat `max:20` Zeichen als Regel —
   längere Dateinamen werden von der Validierung abgelehnt.
