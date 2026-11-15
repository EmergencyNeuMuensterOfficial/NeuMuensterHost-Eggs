# Node.js + Playwright

Node.js-Egg mit allen Systembibliotheken, die Playwright/Puppeteer für Chromium und
Firefox brauchen — gedacht für Panel-Server, die **ohne root** laufen.

| Egg-Datei | `egg-nodejs-playwright.json` |
| --- | --- |
| Name im Panel | NodeJS Playwright |
| Quelle | https://github.com/niclasspecht/daki-yolks (`custom/nodejs-playwright`) |

## Images

| Label | Image | Basiert auf |
| --- | --- | --- |
| Nodejs 26 Playwright | `ghcr.io/niclasspecht/daki-yolks:nodejs-playwright_26` | `nodejs_26` |
| Nodejs 24 Playwright | `ghcr.io/niclasspecht/daki-yolks:nodejs-playwright_24` | `nodejs_24` |
| Nodejs 22 Playwright | `ghcr.io/niclasspecht/daki-yolks:nodejs-playwright_22` | `nodejs_22` |

## Warum ein eigenes Egg

`npx playwright install --with-deps` installiert Systempakete per `apt` und scheitert
ohne root (`su: Authentication failure`). Ohne diese Pakete bricht Chromium mit
`libatk-1.0.so.0: cannot open shared object file` ab. Das Image bringt die Pakete mit,
im Server wird nur noch der Browser selbst heruntergeladen — das geht als normaler User.

## Enthalten

- Alles aus dem Node.js-Basis-Image (Versionen siehe [../nodejs/](../nodejs/))
- Chromium- und Firefox-Systemlibs, bei jedem Rebuild über `playwright install-deps` für Debian 13 aufgelöst
- `Xvfb` + `xauth` für Headed-Betrieb, `procps` zum Aufräumen hängender Browser-Prozesse
- Fonts für Latin, CJK, Thai und Emoji

Die **Browser sind nicht im Image**. Jede Playwright-Version braucht einen eigenen
Browser-Build, deshalb werden sie pro Server nach `/home/container/.cache/ms-playwright`
installiert.

## Variablen

| Variable | Default | Zweck |
| --- | --- | --- |
| `STARTUP_CMD` | `npm install --save --production` | Erstes Kommando beim Start |
| `PLAYWRIGHT_INSTALL` | `chromium` | Argumente für `playwright install`, läuft nach `npm install` mit der Version aus `node_modules`. Leer = deaktiviert. |
| `SECOND_CMD` | `node index.js` | Zweites Kommando; leer = Server beendet sich |

Sinnvolle Werte für `PLAYWRIGHT_INSTALL`: `chromium`, `chromium firefox` oder
`chromium --only-shell` (nur die Headless-Shell, spart ca. 190 MB).

## Betriebshinweise

- Browser **headless** starten (Playwright-Default). Nur das ist getestet.
- Headed-Betrieb: `xvfb-run -a -s "-screen 0 1920x1080x24" node index.js`.
  Headed Chromium auf Xvfb hat keine GPU und kann keine Screenshots machen.
- Chromium braucht viel RAM: 512 MB reichen für eine Seite gleichzeitig, für parallele
  Seiten mehr einplanen.
- Puppeteer funktioniert ebenfalls — es lädt sein eigenes Chrome nach `~/.cache/puppeteer`
  und nutzt dieselben Libraries.
- Die Browser liegen im Server-Volume. Bei einem Reinstall oder gelöschten
  `.cache`-Ordner werden sie beim nächsten Start neu geladen.
