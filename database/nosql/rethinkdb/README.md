# RethinkDB

Realtime-Datenbank aus den
[Pterodactyl Application Eggs](https://github.com/pterodactyl/application-eggs/tree/main/database/nosql/rethinkdb).

| Egg | `egg-rethinkdb.json` |
| --- | --- |
| Kategorie | `database/nosql` |
| Image | `ghcr.io/ptero-eggs/yolks:debian` (RethinkDB wird beim Install nachgeladen) |
| Startup-Erkennung | `Server ready` |
| Stop | `^C` → `SIGINT` |

## Ports

Es werden **drei Allocations** gebraucht:

| Zweck | Variable | Default |
| --- | --- | --- |
| Cluster | `SERVER_PORT` | 25567 |
| Driver (Client-Zugriff) | `DRIVER_PORT` | 25568 |
| HTTP-Admin | `HTTP_PORT` | 25569 |

## Variablen

| Variable | Default | Zweck |
| --- | --- | --- |
| `VERSION` | `2.4.4` | RethinkDB-Version, die beim Install als `.deb` geladen wird |
| `DRIVER_PORT` | `25568` | Port für Treiber-Verbindungen |
| `HTTP_PORT` | `25569` | Port des Web-Dashboards |

## Installation

Das Install-Skript lädt das passende Debian-Paket von `download.rethinkdb.com`
(Bookworm-Build, `amd64`/`arm64` automatisch erkannt), entpackt es und legt nur das
Binary `rethinkdb` ins Server-Verzeichnis. Kein root, keine Systempakete.

## ⚠️ Vor dem ersten Einsatz

1. **Bindet auf `0.0.0.0`** — die Datenbank ist von außen über die Allocation erreichbar,
   sofern der Port offen ist. Wer das nicht will, muss die `--bind`-Angabe im
   Startup-Kommando auf die interne IP ändern.
2. **Admin-Passwort wird beim ersten Start zufällig erzeugt** (`--initial-password auto`)
   und steht im Log. Nach dem ersten Start ablesen und sicher notieren.
3. **Web-Dashboard ist standardmäßig aus** (`--no-http-admin`). Zum Aktivieren den Flag
   aus dem Startup-Kommando entfernen — dann ist das Dashboard aber auch ohne TLS
   erreichbar.
