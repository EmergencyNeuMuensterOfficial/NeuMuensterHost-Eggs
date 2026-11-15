# Python

Python-Egg basierend auf den Daki-Yolks-Images (security-gepatchte Drop-in-Replacements
für die ptero-eggs yolks, Debian 13 "trixie").

| Egg-Datei | `egg-python.json` |
| --- | --- |
| Name im Panel | Python |
| Quelle | https://github.com/niclasspecht/daki-yolks (`base/python`) |

## Images

| Label | Image | Support bis |
| --- | --- | --- |
| Python 3.15 | `ghcr.io/niclasspecht/daki-yolks:python_3.15` | Oktober 2031 |
| Python 3.14 | `ghcr.io/niclasspecht/daki-yolks:python_3.14` | Oktober 2030 |
| Python 3.13 | `ghcr.io/niclasspecht/daki-yolks:python_3.13` | Oktober 2029 |
| Python 3.12 | `ghcr.io/niclasspecht/daki-yolks:python_3.12` | Oktober 2028 |
| Python 3.11 | `ghcr.io/niclasspecht/daki-yolks:python_3.11` | Oktober 2027 |

Python 3.9 und 3.10 sind als **EOL – no security updates** enthalten (ptero-eggs-Images).

## Enthalten

- `gcc`, `g++`, `git`, `ffmpeg`, `curl`, `dig`, `openssh-client`, `procps`, `psmisc`,
  `patch`, `less`, `tini`
- Entrypoint setzt `INTERNAL_IP` und führt `STARTUP` aus
- `STOPSIGNAL SIGINT`

## Variablen

| Variable | Default | Zweck |
| --- | --- | --- |
| `STARTUP_CMD` | `pip install -r requirements.txt` | Erstes Kommando beim Start |
| `SECOND_CMD` | `python bot.py` | Zweites Kommando; leer = Server beendet sich |

## Hinweise

- Kein `venv` im Image angelegt — wer eins nutzt, muss es beim Start selbst aktivieren
  (z. B. `STARTUP_CMD = python -m venv .venv && .venv/bin/pip install -r requirements.txt`,
  `SECOND_CMD = .venv/bin/python bot.py`).
- Upgrade von Debian 12 auf 13 (Python 3.11–3.13): bestehende virtuelle Environments
  müssen neu erstellt werden, weil die Interpreter-Pfade nicht mehr passen.
- `pip install` läuft bei jedem Start — bei vielen Dependencies lieber `requirements.txt`
  gepinnt halten, sonst startet der Server langsam.
