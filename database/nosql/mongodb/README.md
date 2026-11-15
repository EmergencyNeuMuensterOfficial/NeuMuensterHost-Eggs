# MongoDB

Dokumentenbasierte Datenbank aus den
[Pterodactyl Application Eggs](https://github.com/pterodactyl/application-eggs/tree/main/database/nosql/mongodb).

| Eggs | `egg-mongo-d-b7.json` (MongoDB 7), `egg-mongo-d-b8.json` (MongoDB 8) |
| --- | --- |
| Kategorie | `database/nosql` |
| Images | `ghcr.io/ptero-eggs/yolks:mongodb_7`, `ghcr.io/ptero-eggs/yolks:mongodb_8` |
| Port | 27017 |
| Startup-Erkennung | `child process started successfully` |
| Stop | `exit` (Kommando an die mongosh-Session) |

## Variablen

| Variable | Default | Zweck |
| --- | --- | --- |
| `MONGO_USER` | `admin` | Admin-Benutzer, wird beim Install im `admin`-Schema angelegt |
| `MONGO_USER_PASS` | *(leer)* | Passwort für diesen Benutzer |

## Wie das Egg startet

`mongod` läuft per `--fork` im Hintergrund, danach hängt das Startup in einer
`mongosh`-Session vorn im Container. Deshalb funktioniert `stop: exit` — das Kommando
geht an mongosh, mongosh beendet sich, der Container stoppt.

Das `mongod.conf` wird beim Install aus dem Image kopiert und liegt danach im
Server-Verzeichnis.

## Sicherheit

**Zugriffskontrolle ist ab MongoDB 6 standardmäßig aktiv** (`security.authorization`,
aus `/etc/mongod.conf.orig`). Gäste können sich verbinden, aber nichts ausführen.

Wer sie bewusst abschalten will, ändert in `mongod.conf`:

```yaml
security:
  authorization: "disabled"
```

## ⚠️ Vor dem ersten Einsatz

1. **`MONGO_USER_PASS` ist leer.** Ohne Passwort schlägt `createUser` im Install-Skript
   fehl und es gibt keinen Admin-Benutzer — die Installation bricht dann mitten drin ab.
   Vor dem Install setzen.
2. **RAM.** Rechnerisch ca. 1 GB pro 100.000 Dokumente. Bei zu wenig RAM swapppt MongoDB
   und wird sehr langsam.
3. Die Meldung zum kostenlosen Monitoring lässt sich mit `db.disableFreeMonitoring()`
   abschalten.
