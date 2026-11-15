# Yolks — NeuMünsterHost Eggs

Egg-Sammlung für die Calagopus-Panel-Instanz von **NeuMünsterHost**.

Dieses Repository ist als Calagopus *Egg Repository* aufgebaut: ein Git-Repo mit
Unterordnern, in denen jeweils ein Egg pro Datei liegt (JSON oder YAML).
Calagopus kann das Repo direkt unter **Admin → Egg Repositories** einbinden und
synchronisiert es automatisch.

## Struktur

```
applications/               # Anwendungs-Eggs
└── bots/
    └── ree6/
        ├── egg-ree6.json           # Ree6 Discord-Bot
        ├── config.yml              # Referenz-Template (nicht vom Egg genutzt)
        └── README.md

database/                   # Datenbank-Eggs
├── nosql/
│   ├── mongodb/
│   │   ├── egg-mongo-d-b7.json     # MongoDB 7
│   │   ├── egg-mongo-d-b8.json     # MongoDB 8
│   │   └── README.md
│   └── rethinkdb/
│       ├── egg-rethinkdb.json
│       └── README.md
└── sql/
    ├── mariadb/
    │   ├── egg-maria-d-b.json      # MariaDB 10.6 – 11.8
    │   └── README.md
    └── postgres/
        ├── egg-postgres13.json     # je ein Egg pro Version
        ├── egg-postgres14.json
        ├── egg-postgres15.json
        ├── egg-postgres16.json
        ├── egg-postgres17.json
        └── README.md

games/                      # Gameserver-Eggs — noch leer

generic/                    # Sprach-/Runtime-Eggs
├── java/
│   ├── egg-java.json               # Generic Java (Java 8 – 21)
│   └── README.md
├── lua/
│   ├── egg-luvit-generic.json      # Lua via Luvit
│   └── README.md
├── nodejs/
│   ├── egg-nodejs.json             # Node.js 22 / 24 / 26
│   └── README.md
├── nodejs-playwright/
│   ├── egg-nodejs-playwright.json  # Node.js + Browser-Libs (Chromium, Firefox)
│   └── README.md
├── python/
│   ├── egg-python.json             # Python 3.11 – 3.15
│   └── README.md
└── rust/
    ├── egg-rust-generic.json       # Rust via Cargo
    └── README.md
```

Eggs kommen aus drei Quellen: [daki-yolks](https://github.com/niclasspecht/daki-yolks)
(Node.js, Python, Playwright), [pelican-eggs](https://github.com/pelican-eggs/eggs)
(Java, Lua, Rust, Ree6) und
[pterodactyl/application-eggs](https://github.com/pterodactyl/application-eggs)
(MongoDB, RethinkDB, MariaDB, PostgreSQL). Details und Lizenzhinweise:
[CREDITS.md](CREDITS.md).

Jedes Egg ist eine eigene Datei, z. B. `games/minecraft-java/egg-paper.json`.
Der Ordnername innerhalb der Kategorie ist frei wählbar — Calagopus liest alle
Egg-Dateien rekursiv ein.

## Egg hinzufügen

1. Egg im Panel erstellen oder importieren.
2. Im Egg auf **Export → Export as JSON** (oder **YAML**) gehen.
3. Datei passend einsortieren (z. B. `games/<name>/egg-<name>.json`).
4. Committen und pushen.
5. Im Panel unter **Admin → Egg Repositories → Sync** klicken.

> Hinweis: Eggs aus einer URL oder Datei werden nicht automatisch aktualisiert.
> Nur Eggs, die aus einem Repository installiert wurden, bekommen
> "Update Available" angezeigt.

## Konventionen

- **Eine Datei pro Egg**, Dateiname `egg-<slug>.json` (bzw. `.yml`).
- Optional pro Egg ein `README.md` im selben Ordner — Calagopus zeigt das als
  Egg-Dokumentation in der Egg Library an.
- Keine Secrets, Tokens oder kundenspezifischen Werte in Eggs oder READMEs.
- Maximale Egg-Größe bei Import per URL: 1 MiB.
- YAML mit Anchors/Aliases (`*name`, `<<:`) vermeiden — Calagopus kann das beim
  Import per URL/Sync nicht auflösen und überspringt das Egg stillschweigend.

## Pterodactyl-Kompatibilität

Calagopus nutzt das Pterodactyl-Egg-Format. Beim Import werden u. a. übersetzt:

| Pterodactyl | Calagopus |
| --- | --- |
| `^C` | `SIGINT` |
| `^SIGTERM` / `^SIGQUIT` / `^SIGABRT` / `^SIGKILL` | entsprechende Signale |
| sonstige `^`-Werte | `SIGKILL` |
| bedingte `config.files`-Replacements | Replacements mit `If Value` |

Beim Export nach Pterodactyl werden `Docker Stop` und `SIGQUIT` als `^SIGTERM`
geschrieben, und der Author muss eine E-Mail-Adresse sein.

## Lizenz

Siehe [LICENSE](LICENSE) — sofern nicht anders angegeben, gilt die Lizenz pro Egg.
