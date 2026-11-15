# Yolks — NeuMünsterHost Eggs

Egg-Sammlung für die Calagopus-Panel-Instanz von **NeuMünsterHost**.

Dieses Repository ist als Calagopus *Egg Repository* aufgebaut: ein Git-Repo mit
Unterordnern, in denen jeweils ein Egg pro Datei liegt (JSON oder YAML).
Calagopus kann das Repo direkt unter **Admin → Egg Repositories** einbinden und
synchronisiert es automatisch.

## Struktur

```
games/                      # Gameserver-Eggs
applications/               # Anwendungs-Eggs
generic/                    # Sprach-/Runtime-Eggs
├── nodejs/                 # Node.js
├── nodejs-playwright/      # Node.js inkl. Playwright + Browser-Deps
├── python/                 # Python (pip/venv)
├── lua/                    # Lua / LuaJIT
└── rust/                   # Rust (cargo)
```

Die Gruppen sind bewusst leer — sie werden erst mit den jeweiligen Eggs befüllt.

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
