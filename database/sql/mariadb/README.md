# MariaDB

MySQL-Kompatibilität, gepflegter Fork aus den
[Pterodactyl Application Eggs](https://github.com/pterodactyl/application-eggs/tree/main/database/sql/mariadb).

| Egg | `egg-maria-d-b.json` |
| --- | --- |
| Kategorie | `database/sql` |
| Port | 3306 |
| Startup-Erkennung | `ready for connections.` |
| Stop | `shutdown; exit;` (Kommando an den MariaDB-Client) |

## Images

| Label | Image |
| --- | --- |
| MariaDB 11.8 LTS | `ghcr.io/ptero-eggs/yolks:mariadb_11.8` |
| MariaDB 11.6 | `ghcr.io/ptero-eggs/yolks:mariadb_11.6` |
| MariaDB 11.4 LTS | `ghcr.io/ptero-eggs/yolks:mariadb_11.4` |
| MariaDB 10.11 LTS | `ghcr.io/ptero-eggs/yolks:mariadb_10.11` |
| MariaDB 10.6 LTS | `ghcr.io/ptero-eggs/yolks:mariadb_10.6` |

Für neue Server 11.8 LTS oder 11.4 LTS wählen. 10.6 läuft im April 2026 aus dem
LTS-Fenster.

## Variablen

**Keine.** Das Egg hat `"variables": []`, obwohl das Startup-Kommando
`${MARIADB_EXECUTABLE}` enthält. Die Variable wird nicht vom Panel gesetzt, sondern vom
`entrypoint.sh` im Mariadb-Image (`mariadbd` bzw. `mysqld`, je nach Version) — im Panel
darf also nichts eingetragen werden.

## Konfiguration

Das Egg schreibt beim Boot `.my.cnf` mit dem INI-Parser und setzt daraus u. a.:

- `mysqld.port` und `client.port` = Default-Port der Allocation
- `mysqld.bind-address` = `0.0.0.0`
- PID, Socket und Logfile unter `/home/container`

Das Install-Skript macht **nichts** — es gibt nur „Installation complete" aus. Datenbank
und Benutzer legt das Image-Entrypoint beim ersten Start an.

## ⚠️ Vor dem ersten Einsatz

1. **`{{server.build.default.port}}` ist Pterodactyl-Syntax.** Calagopus muss diesen
   Platzhalter beim Import übersetzen, damit `.my.cnf` korrekt geschrieben wird. Falls
   die Ports in der Config nach dem Import leer sind, die vier `find`-Einträge im
   Config-File-Bereich des Eggs auf den Calagopus-Platzhalter umstellen.
2. **Allocation zwingend nötig** — ohne zugewiesenen Port schlägt das Schreiben der
   `.my.cnf` fehl bzw. der Port ist 0.
3. **Kein root-Passwort, kein Benutzer.** Das Image legt beim ersten Start einen
   Datenbankbenutzer `container` (passwortlos, nur lokal über Socket) an. Für Zugriff
   von außen muss ein eigener Benutzer mit `CREATE USER` und `GRANT` erstellt werden.
