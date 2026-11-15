# Lua (Luvit)

Generisches Lua-Egg aus den [Pelican Eggs](https://github.com/pelican-eggs/eggs/tree/master/generic/lua/luvit).
Klont ein Git-Repo, installiert [Luvit](https://luvit.io) und die Pakete aus Lit und
startet die angegebene `.lua`-Datei. Gedacht für Discord-Bots auf Basis von
[discordia](https://github.com/SinisterRectus/discordia).

| Egg-Datei | `egg-luvit-generic.json` |
| --- | --- |
| Name im Panel | luvit generic |
| Quelle | https://github.com/pelican-eggs/eggs (`generic/lua/luvit`) |
| Autor (Original) | parker@parkervcp.com |

## Images

| Label | Image |
| --- | --- |
| Debian | `ghcr.io/parkervcp/yolks:debian` |

Luvit selbst ist **nicht** im Image. `lit` und `luvit` werden beim Install in das
Server-Verzeichnis geladen, deshalb liegt beides im Volume und überlebt Neustarts.

## Variablen

| Variable | Default | Zweck |
| --- | --- | --- |
| `GIT_ADDRESS` | *(leer)* | Repo, das beim Install geklont wird |
| `BRANCH` | *(leer)* | Branch; leer = Default-Branch |
| `USERNAME` | *(leer)* | Git-Benutzername für private Repos |
| `ACCESS_TOKEN` | *(leer)* | Token/Passwort für private Repos |
| `USER_UPLOAD` | `0` | `1` = Install überspringen, User lädt Dateien selbst hoch |
| `LUA_FILE` | `app.lua` | Datei, die gestartet wird |
| `LIT_PACKAGES` | *(leer)* | Lit-Pakete beim Install, z. B. `SinisterRectus/discordia lduboeuf/cjson` |

Startup: `./luvit {{LUA_FILE}}`

## Anpassungen gegenüber Upstream

1. **`config.stop`**: Original `^c` (klein). Calagopus übersetzt nur `^C` in `SIGINT` —
   jeder andere `^`-Wert wird als **`SIGKILL`** importiert, der Server würde also bei
   jedem Stop hart abgeschossen. Hier steht deshalb `^C`.
2. **`config.startup.done`**: Original Platzhalter `change me`. Jetzt `""`, d. h. der
   Server gilt als *running*, sobald Luvit die erste Konsolenzeile ausgibt — was bei
   einem stillen Lua-Skript allerdings nie passiert. Dann auf eine Zeile setzen, die
   dein Skript ausgibt, z. B. `"done": ["Logged in"]`.

## ⚠️ Vor dem ersten Einsatz anpassen

1. **Nach einem Reinstall** ist `lit`/`luvit` zwar wieder da, die installierten
   Lit-Pakete aber nur, wenn `LIT_PACKAGES` gefüllt ist. Wer Pakete manuell nachinstalliert
   hat, muss das nach einem Reinstall erneut tun.
