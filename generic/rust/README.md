# Rust

Generisches Rust-Egg aus den [Pelican Eggs](https://github.com/pelican-eggs/eggs/tree/master/generic/rust).
Klont ein Git-Repo und startet es mit `cargo run --release`.

| Egg-Datei | `egg-rust-generic.json` |
| --- | --- |
| Name im Panel | rust generic |
| Quelle | https://github.com/pelican-eggs/eggs (`generic/rust`) |
| Autor (Original) | ethan@ethancoward.dev |

## Images

| Label | Image |
| --- | --- |
| Rust latest | `ghcr.io/parkervcp/yolks:rust_latest` |
| Rust 1.60 | `ghcr.io/parkervcp/yolks:rust_1.60` |
| Rust 1.56 | `ghcr.io/parkervcp/yolks:rust_1.56` |
| Rust 1.31 | `ghcr.io/parkervcp/yolks:rust_1.31` |

Die gepinnten Versionen sind alt (1.60 von 2022). Für neue Projekte `rust_latest`
nehmen — das Image wird von parkervcp gepflegt.

## Variablen

| Variable | Default | Zweck |
| --- | --- | --- |
| `GIT_ADDRESS` | *(leer)* | Repo, das beim Install geklont wird |
| `BRANCH` | *(leer)* | Branch; leer = Default-Branch des Repos |
| `AUTO_UPDATE` | `0` | Bei jedem Start `git pull` ausführen |
| `USERNAME` | *(leer)* | Git-Benutzername für private Repos |
| `ACCESS_TOKEN` | *(leer)* | Token/Passwort für private Repos |

Startup: `if [[ -d .git ]] && [[ {{AUTO_UPDATE}} == "1" ]]; then git pull; fi; cargo run --release`

## Startup-Erkennung

`config.startup.done` steht auf `""` (Upstream: Platzhalter `change this part`).
Ein leeres Muster matcht jede Konsolenzeile — der Server gilt also als *running*,
sobald `cargo` die erste Zeile ausgibt. Das passiert schon während des Builds,
nicht erst wenn das Binary läuft.

Für eine präzisere Erkennung auf eine Zeile setzen, die dein Programm beim Start
ausgibt, z. B. `"done": ["listening on"]` (Array = mehrere Muster).

## ⚠️ Vor dem ersten Einsatz anpassen

1. **`cargo run --release` kompiliert bei jedem Start.** Bei mittelgroßen Projekten
   sind das Minuten, in denen der Server belegt ist. Übliche Lösung: einmal
   `cargo build --release` und danach `./target/release/<binary>` starten —
   dafür muss allerdings das Startup-Kommando im Egg umgebaut werden.
2. **RAM/CPU beim Build.** `cargo build` braucht deutlich mehr Ressourcen als der
   spätere Betrieb; ein 512-MB-Plan reicht oft nicht.
