# PostgreSQL

Relationale Datenbank aus den
[Pterodactyl Application Eggs](https://github.com/pterodactyl/application-eggs/tree/main/database/sql/postgres).

| Eggs | `egg-postgres13.json` … `egg-postgres17.json` |
| --- | --- |
| Kategorie | `database/sql` |
| Port | 5432 |
| Startup-Erkennung | `database system is ready to accept connections` |
| Stop | `\q` (psql-Metakommando, beendet die Client-Session) |

Fünf separate Eggs, die sich nur in Name und Image-Tag unterscheiden — pro Version ein
Eintrag im Panel, damit die Version direkt beim Server angewählt wird statt im Image-Picker.

## Images

| Egg | Image |
| --- | --- |
| Postgres 13 | `ghcr.io/ptero-eggs/yolks:postgres_13` |
| Postgres 14 | `ghcr.io/ptero-eggs/yolks:postgres_14` |
| Postgres 15 | `ghcr.io/ptero-eggs/yolks:postgres_15` |
| Postgres 16 | `ghcr.io/ptero-eggs/yolks:postgres_16` |
| Postgres 17 | `ghcr.io/ptero-eggs/yolks:postgres_17` |

## Variablen

| Variable | Default | Zweck |
| --- | --- | --- |
| `PGUSER` | `pterodactyl` | Superuser, wird beim `initdb` angelegt |
| `PGPASSWORD` | `Pl3453Ch4n63M3!` | Passwort für diesen Benutzer |

## Installation

Das Install-Skript (Alpine) legt den Benutzer `container` an, gibt ihm `/mnt/server` und
führt `initdb` aus:

- `-D /mnt/server/postgres_db/` — Datenverzeichnis
- `-A md5` — Passwort-Auth
- Superuser `PGUSER` mit Passwort aus `PGPASSWORD`

Zusätzlich hängt es eine Regel an `pg_hba.conf`, die Verbindungen von `0.0.0.0/0`
mit md5-Passwort erlaubt.

## ⚠️ Vor dem ersten Einsatz

1. **Das Default-Passwort ist öffentlich bekannt** (`Pl3453Ch4n63M3!`). Vor dem Install
   unbedingt `PGPASSWORD` ändern — sonst ist die Datenbank mit Standard-Credentials
   erreichbar.
2. **`pg_hba.conf` erlaubt `0.0.0.0/0`.** Zusammen mit einer offenen Allocation heißt das:
   von überall erreichbar, sofern Passwort bekannt. Wer nur intern zugreifen will, muss
   die Regel nach dem Install auf die interne IP einschränken.
3. **2 GB RAM empfohlen.** Darunter wird PostgreSQL bei größeren Queries zäh.
4. **Nach dem Install** ist das Datenverzeichnis bereits initialisiert. Ein erneutes
   Install überschreibt `postgres_db/` nicht, sondern scheitert an der bestehenden
   `PG_VERSION` — beim Neuaufsetzen den Ordner vorher löschen.
