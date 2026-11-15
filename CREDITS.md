# Quellen & Lizenz

## Importierte Eggs

### Daki Yolks

Die Eggs in `generic/nodejs`, `generic/python` und `generic/nodejs-playwright` stammen aus

**https://github.com/niclasspecht/daki-yolks** (Daki Yolks, von [Daki Hosting](https://daki.cc))

| Unser Pfad | Quelle im Upstream |
| --- | --- |
| `generic/nodejs/egg-nodejs.json` | `base/nodejs/egg-nodejs.json` |
| `generic/python/egg-python.json` | `base/python/egg-python.json` |
| `generic/nodejs-playwright/egg-nodejs-playwright.json` | `custom/nodejs-playwright/egg-nodejs-playwright.json` |

Die Eggs wurden unverändert übernommen. Die `README.md` daneben sind eigene
Zusammenfassungen der Upstream-Dokumentation und der jeweiligen `Dockerfile`.

### Pelican Eggs

Die Eggs in `generic/java`, `generic/lua`, `generic/rust` und `applications/bots/ree6`
stammen aus

**https://github.com/pelican-eggs/eggs** (ehemals parkervcp/eggs)

| Unser Pfad | Quelle im Upstream |
| --- | --- |
| `generic/java/egg-java.json` | `generic/java/egg-java.json` |
| `generic/lua/egg-luvit-generic.json` | `generic/lua/luvit/egg-luvit-generic.json` |
| `generic/rust/egg-rust-generic.json` | `generic/rust/egg-rust-generic.json` |
| `applications/bots/ree6/egg-ree6.json` | `bots/discord/ree6/egg-ree6.json` |
| `applications/bots/ree6/config.yml` | `bots/discord/ree6/config.yml` |

Anpassungen gegenüber Upstream: `author` auf unsere Support-Adresse gesetzt (in allen
Eggs), im Luvit-Egg `config.stop` von `^c` auf `^C` korrigiert (Calagopus übersetzt nur
`^C` als `SIGINT`, sonst wird `SIGKILL` daraus) und bei Java, Lua und Rust
`config.startup.done` vom Platzhalter (`change this text` / `change this part` /
`change me`) auf `""` gesetzt, damit der Server beim ersten Konsolen-Output als
*running* gilt statt dauerhaft auf *starting* zu hängen. Das Ree6-Egg ist bis auf den
Autor unverändert; seine `config.yml` liegt als Referenzkopie daneben.

Stand des Imports: 15.11.2026.

### Pterodactyl Application Eggs

Die Eggs in `database/nosql` und `database/sql` stammen aus

**https://github.com/pterodactyl/application-eggs**

| Unser Pfad | Quelle im Upstream |
| --- | --- |
| `database/nosql/mongodb/egg-mongo-d-b7.json` | `database/nosql/mongodb/egg-mongo-d-b7.json` |
| `database/nosql/mongodb/egg-mongo-d-b8.json` | `database/nosql/mongodb/egg-mongo-d-b8.json` |
| `database/nosql/rethinkdb/egg-rethinkdb.json` | `database/nosql/rethinkdb/egg-rethinkdb.json` |
| `database/sql/mariadb/egg-maria-d-b.json` | `database/sql/mariadb/egg-maria-d-b.json` |
| `database/sql/postgres/egg-postgres13.json` | `database/sql/postgres/egg-postgres13.json` |
| `database/sql/postgres/egg-postgres14.json` | `database/sql/postgres/egg-postgres14.json` |
| `database/sql/postgres/egg-postgres15.json` | `database/sql/postgres/egg-postgres15.json` |
| `database/sql/postgres/egg-postgres16.json` | `database/sql/postgres/egg-postgres16.json` |
| `database/sql/postgres/egg-postgres17.json` | `database/sql/postgres/egg-postgres17.json` |

Nicht übernommen: `database/redis` (redis-5, redis-6, redis-7) — bewusst ausgelassen.

Anpassung: nur `author` auf unsere Support-Adresse. `stop`, `done` und Variablen sind
unverändert.

## Was nicht übernommen wurde

- `base/*/Dockerfile`, `entrypoint.sh`, `test/` — gehören zum Image-Build, nicht zum Egg
- `.github/workflows/` — Build-Pipeline der Upstream-Images
- `scripts/` (prepull, migrate-images, restart-servers) — Admin-Helfer für Pterodactyl-Panels
- die Upstream-`README.md` der Pelican-Eggs — Inhalt sinngemäß in unsere Versionen übernommen

## Docker Images

Drei Registrys im Einsatz:

- `ghcr.io/niclasspecht/daki-yolks:*` (Node.js, Python, Playwright) — öffentlich und
  login-frei, security-gepatchte Drop-in-Replacements für die ptero-eggs yolks.
- `ghcr.io/parkervcp/yolks:*` und `ghcr.io/parkervcp/installers:*` (Java, Lua, Rust) —
  die klassischen Community-Images.
- `ghcr.io/ptero-eggs/yolks:*` und `ghcr.io/ptero-eggs/installers:*` (Datenbanken,
  Ree6) — die aktuellen Pterodactyl-Community-Images.

Das Ree6-Egg nutzt zusätzlich `ghcr.io/pterodactyl/yolks:java_17` (alter Namespace,
existiert weiterhin) und das MongoDB-Install-Skript `mongo:7-jammy` von Docker Hub.

Alle lassen sich direkt ziehen; wir können sie aber nicht selbst neu bauen oder patchen.
Wer eigene Images will, braucht einen Fork des jeweiligen Upstream-Repos mit eigener
GHCR-Pipeline und muss die `docker_images`-Einträge anpassen.

## Lizenz

Alle drei Quellen stehen unter der MIT-Lizenz:

**Daki Yolks** — basierend auf [ptero-eggs/yolks](https://github.com/ptero-eggs/yolks):

```
MIT License

Copyright (c) 2024 Pterodactyl team and contributors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

**Pelican Eggs** — [pelican-eggs/eggs](https://github.com/pelican-eggs/eggs):

```
MIT

Copyright (c) 2018 Michael Parker <parker@parkervcp.com> and Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

Diese Lizenzen gelten für die übernommenen Egg-Dateien und die abgeleiteten READMEs.

**Pterodactyl Application Eggs** — [pterodactyl/application-eggs](https://github.com/pterodactyl/application-eggs):

```
MIT License

Copyright (c) 2024 Pterodactyl team and contributors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
