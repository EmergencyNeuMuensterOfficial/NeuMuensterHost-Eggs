# Ree6

All-in-one-Discord-Bot (Open Source, kostenlos) aus den
[Pelican Eggs](https://github.com/pelican-eggs/eggs/tree/master/bots/discord/ree6).

| Egg-Datei | `egg-ree6.json` |
| --- | --- |
| Name im Panel | Ree6 |
| Quelle | https://github.com/pelican-eggs/eggs (`bots/discord/ree6`) |
| Autor (Original) | presti@presti.me |
| Bot-Projekt | https://github.com/Ree6-Applications/Ree6 |

Funktionsumfang laut Projekt: Moderation, Musik, Level, Economy, Tickets, Vorschläge,
Audit-Logging, Benachrichtigungen (Twitter, Instagram, YouTube, Reddit, Twitch),
Webinterface.

## Image & Start

| Feld | Wert |
| --- | --- |
| Image | `ghcr.io/pterodactyl/yolks:java_17` |
| Startup | `java -Dnogui=true -jar Ree6.jar` |
| Startup-Erkennung | `Finished Loading` |
| Stop | `^C` → `SIGINT` |

Das Image liegt noch im alten `pterodactyl`-Namespace (`ghcr.io/pterodactyl/yolks`) und
existiert dort weiterhin. Aktuell wäre `ghcr.io/ptero-eggs/yolks:java_17` — gleicher
Inhalt, gepflegter Namespace. Ein Wechsel ist unkritisch, weil nur der Java-Runtime
gebraucht wird.

**Ports:** keine. Der Bot braucht keine Allocation, ein Server ohne Port reicht.

## Installation

Das Install-Skript holt bei jedem Reinstall:

1. das neueste Release-Jar (`*jar-with-dependencies.jar`) über die GitHub-API von
   `Ree6-Applications/Ree6`,
2. die `config.yml` von `raw.githubusercontent.com/parkervcp/eggs/master/bots/discord/ree6/config.yml`.

Ein vorhandenes `Ree6.jar` wird vorher zu `Ree6.jar-old`, eine vorhandene `config.yml`
zu `config.yml-old` umbenannt. **Eine angepasste `config.yml` wird also bei jedem
Reinstall überschrieben** — vorher sichern.

Die `config.yml` in diesem Ordner ist nur eine Referenzkopie des Templates; sie wird
vom Egg nicht verwendet.

## Variablen

Das Egg bringt 39 Variablen mit (`BOT_TOKEN`, `DATABASE_*`, `SPOTIFY_*`, `TWITCH_*`,
`REDDIT_*`, `MISC_*`, …). **Keine davon wird vom Install-Skript verwendet und Ree6
liest sie auch nicht** — der Bot liest ausschließlich `config.yml`. Die Variablen
werden nur als Umgebungsvariablen in den Container exportiert und bleiben wirkungslos.

Praktisch heißt das: Token und API-Keys müssen nach dem Install **manuell über den
Dateimanager in `config.yml`** eingetragen werden:

```yaml
bot:
  tokens:
    release: DEIN_BOT_TOKEN
  misc:
    ownerId: DEINE_DISCORD_ID
```

Ohne Token startet der Bot nicht. Die vielen API-Keys sind optional — fehlende Keys
führen nur zu Fehlermeldungen für die jeweilige Funktion.

## Hinweise

- `config.yml` enthält `config.version: 3.0.0`. Passt die Version nicht zum Jar, kann
  Ree6 den Start verweigern — dann Config-Template aus dem Bot-Release übernehmen.
- Der Download läuft über die GitHub-API ohne Token. Bei häufigen Reinstalls kann das
  ins Rate-Limit laufen; das Egg hat dafür keine Variablen (`GITHUB_USER`/`GITHUB_OAUTH_TOKEN`
  sind im Skript-Kommentar erwähnt, aber nicht implementiert).
- Die URL im Install-Skript zeigt auf `parkervcp/eggs` (heute `pelican-eggs/eggs`).
  GitHub leitet um, funktioniert also — bricht aber, falls das alte Repo verschwindet.
