# Quellen & Lizenz

## Importierte Eggs

Die Eggs in `generic/nodejs`, `generic/python` und `generic/nodejs-playwright` stammen aus

**https://github.com/niclasspecht/daki-yolks** (Daki Yolks, von [Daki Hosting](https://daki.cc))

| Unser Pfad | Quelle im Upstream |
| --- | --- |
| `generic/nodejs/egg-nodejs.json` | `base/nodejs/egg-nodejs.json` |
| `generic/python/egg-python.json` | `base/python/egg-python.json` |
| `generic/nodejs-playwright/egg-nodejs-playwright.json` | `custom/nodejs-playwright/egg-nodejs-playwright.json` |

Die Eggs wurden unverändert übernommen. Die `README.md` daneben sind eigene
Zusammenfassungen der Upstream-Dokumentation und der jeweiligen `Dockerfile`.

Stand des Imports: 15.11.2026.

## Was nicht übernommen wurde

- `base/*/Dockerfile`, `entrypoint.sh`, `test/` — gehören zum Image-Build, nicht zum Egg
- `.github/workflows/` — Build-Pipeline der Upstream-Images
- `scripts/` (prepull, migrate-images, restart-servers) — Admin-Helfer für Pterodactyl-Panels
- `custom/nodejs-playwright/README.md` — Inhalt sinngemäß in unsere Version übernommen

## Docker Images

Alle Eggs verweisen auf `ghcr.io/niclasspecht/daki-yolks:*`. Das ist eine öffentliche,
login-freie Registry — die Images lassen sich direkt ziehen, wir können sie aber nicht
selbst neu bauen oder patchen. Wer eigene Images will, braucht einen Fork des Upstream-Repos
mit eigener GHCR-Pipeline und muss die `docker_images`-Einträge anpassen.

## Lizenz

Daki Yolks steht unter der MIT-Lizenz, basierend auf [ptero-eggs/yolks](https://github.com/ptero-eggs/yolks):

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

Diese Lizenz gilt für die übernommenen Egg-Dateien und die abgeleiteten READMEs.
