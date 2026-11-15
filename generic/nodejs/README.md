# Node.js

Node.js-Egg basierend auf den Daki-Yolks-Images (security-gepatchte Drop-in-Replacements
für die ptero-eggs yolks, Debian 13 "trixie").

| Egg-Datei | `egg-nodejs.json` |
| --- | --- |
| Name im Panel | NodeJS |
| Quelle | https://github.com/niclasspecht/daki-yolks (`base/nodejs`) |

## Images

| Label | Image | Support bis |
| --- | --- | --- |
| Nodejs 26 | `ghcr.io/niclasspecht/daki-yolks:nodejs_26` | April 2029 |
| Nodejs 24 | `ghcr.io/niclasspecht/daki-yolks:nodejs_24` | April 2028 |
| Nodejs 22 | `ghcr.io/niclasspecht/daki-yolks:nodejs_22` | April 2027 |

Zusätzlich sind die alten ptero-eggs-Images für Node.js 18–21, 23 und 25 enthalten,
explizit als **EOL – no security updates** markiert. Sie laufen, bekommen aber keine
Sicherheitsfixes mehr.

## Enthalten

- `build-essential`, `git`, `ffmpeg`, `sqlite3`, `python3`, `curl`, `dig`, `ping`, `zip`, `tini`
- Globale npm-Pakete: `npm@11`, `typescript@6`, `ts-node`, `@types/node`
  (TypeScript ist auf Major 6 gepinnt, weil `ts-node` mit TypeScript 7 nicht funktioniert)
- `STOPSIGNAL SIGINT`, Entrypoint setzt `INTERNAL_IP` und führt `STARTUP` aus

## Variablen

| Variable | Default | Zweck |
| --- | --- | --- |
| `STARTUP_CMD` | `npm install --save --production` | Erstes Kommando beim Start |
| `SECOND_CMD` | `node index.js` | Zweites Kommando; leer = Server beendet sich |

## Hinweise

- Zwei getrennte Startkommandos statt eines freien Startup-Strings — Dependencies
  werden bei **jedem** Start neu installiert. Bei großen Projekten kostet das Startzeit.
- Nach einem Node-Major-Upgrade (z. B. 24 → 26) müssen native Module neu gebaut werden
  (`npm rebuild`) und `.node`-Artefakte aus dem alten Image passen nicht mehr.
