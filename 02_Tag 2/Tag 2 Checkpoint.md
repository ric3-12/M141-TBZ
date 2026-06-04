
## MySQL-Server

**1. Wie kann der MySQL-Server gestartet werden?**

- [ ] Start von `mysql.exe` im CMD-Fenster
- [x] Start von `mysqld.exe` im CMD-Fenster
- [ ] über MySQL-Workbench
- [ ] Eingabe von localhost als URL im Browser
- [x] `NET START mysql` (im CMD-Fenster)
- [x] mit dem Dienstmanager von Windows

**2. Welche Informationen erhalten Sie, wenn Sie im MySQL-Konsolenfenster den Befehl *Status;* eingeben?**

- [x] Version des Konsolenprogramms
- [x] Betriebszeit des Servers
- [x] Version des Servers
- [ ] Betriebszeit des DB-Klienten `mysql`

**3. Welche Daten befinden sich im Verzeichnis datadir (z. B. C:\\…\\mysql\\data)?**

- [x] Protokoll-Dateien (Log-Files)
- [x] Fehlerprotokolle
- [ ] die ausführbaren MySQL-Programme, z.B. mysql.exe
- [x] Datenbanken

**4. Wie prüfen Sie, ob der MySQL-Server läuft?**

- [x] mit dem Dienst-Manager von Windows
- [x] mit dem GUI-Tool Administrator
- [ ] durch Eingabe des Befehls `status` im CMD-Fenster
- [x] mit dem Task-Manager von Windows (Prozess)

**5. Wie testen Sie die Installation des DB-Servers?**

Man verbindet sich mit `mysql -u root -p` im CMD-Fenster. Wenn die Verbindung erfolgreich ist und der MySQL-Prompt (`mysql>`) erscheint, ist die Installation korrekt. Alternativ kann man `SHOW DATABASES;` ausführen und prüfen, ob Systemdatenbanken wie `mysql` und `information_schema` vorhanden sind.

**6. Wie überprüfen Sie die Laufzeit des DB-Servers?**

Im MySQL-Konsolenfenster mit dem Befehl `status;` – dort wird unter „Uptime" angezeigt, wie lange der Server bereits läuft. Alternativ im Windows-Dienstmanager, wo der Startzeitpunkt des Dienstes sichtbar ist.

**7. Wozu verwenden Sie das Programm mysql.exe? Wie starten Sie es?**

`mysql.exe` ist der MySQL-Kommandozeilen-Client. Man verwendet es, um sich mit dem MySQL-Server zu verbinden und SQL-Befehle interaktiv auszuführen (z. B. Datenbanken erstellen, Tabellen abfragen, Benutzer verwalten).

Start im CMD-Fenster:
```
mysql -u root -p
```
Nach dem Start fragt es nach dem Passwort, danach erscheint der MySQL-Prompt (`mysql>`).

**8. Notieren Sie 3 Informationen des status-Befehls mit ihrer Bedeutung.**

| Information | Bedeutung |
|-------------|-----------|
| **Server version** | Gibt die installierte MySQL-Version an (z. B. 8.0.33) |
| **Uptime** | Zeigt, wie lange der Server seit dem letzten Start läuft |
| **Connection id** | Eindeutige ID der aktuellen Client-Verbindung zum Server |

**9. Nennen Sie 2 wichtige Verzeichnisse der MySQL-Installation mit ihrem Inhalt.**

| Verzeichnis | Inhalt |
|-------------|--------|
| **bin** (z. B. `C:\mysql\bin`) | Enthält die ausführbaren Programme, z. B. `mysql.exe`, `mysqld.exe`, `mysqladmin.exe` |
| **data** (z. B. `C:\mysql\data`) | Enthält die Datenbankdateien, Log-Files und Fehlerprotokolle |

**10. Was ist der Inhalt der `my.ini`-Datei?**

Die `my.ini` ist die Konfigurationsdatei des MySQL-Servers. Sie enthält Einstellungen für den Server (`[mysqld]`) und den Client (`[client]`), z. B. den Port (Standard: 3306), das Datenverzeichnis (`datadir`), das Installationsverzeichnis (`basedir`) sowie den Zeichensatz (`character-set-server`). Die Datei wird beim Serverstart eingelesen.


## Codierung und Kollation

**1. Welche Aussagen treffen zum Thema Codierung zu?**

- [ ] Ein Datenbankserver erkennt die Codierung einer Datei automatisch
- [x] Codierung ist eine Vereinbarung zwischen dem Nutzer und dem System.
- [x] Die Codierung legt fest, welche binäre Bitkombination zu welchem Zeichen gehört.
- [ ] ANSI- und ASCII-Codierung ist dasselbe
- [ ] Der Unicode-Zeichensatz hat 32 Bit Codelänge
- [x] `UTF` bedeutet Unicode Transformation Format
- [ ] `UTF-8` hat nur 8 Bit lange Zeichen aus dem Unicode-Zeichensatz

> *Hinweis: UTF-8 ist variabel lang (1–4 Bytes), nicht nur 8 Bit.*

**3. Welche Aussagen treffen zum Thema Kollation zu?**

- [ ] 'utf8_general_cs' ist die Standard-Einstellung bei MySQL.
- [x] In der DIN-Normierung zur deutschen Kollation werden zwei Varianten zur Umlauthandhabung angeboten.
- [ ] Die Endung "_ci" gibt an, dass die Sortierung die Gross-/Kleinschreibweise unterscheidet.
- [x] Seit MySQL 5.5.3 sollte `utf8mb4` anstelle von utf8 verwendet werden.
- [x] In der Konfig-Datei (my.ini) kann die UTF8-Codierung als Standard angegeben werden.
- [ ] Eine Kollationseinstellung gilt für die ganze Tabelle (Entität).
- [x] "Binärsortierung" ist die Sortierung anhand des binären Codes der verglichenen Zeichen.

**4. Was haben Sie bei der DB Kollation beobachtet? (Latin 1 2, general, ci, cs, ...)**

| Begriff | Bedeutung |
|---------|-----------|
| **latin1** | Nur westeuropäische Zeichen werden korrekt dargestellt |
| **utf8mb4_general_ci** | Gebräuchlichste Einstellung; `ci` = case-insensitive (Gross-/Kleinschreibung wird ignoriert) |
| **cs** | case-sensitive (unterscheidet Gross-/Kleinschreibung) |
| **general** | Vereinfachte, schnelle Sortierung; Umlaute werden nicht immer nach deutschen Regeln sortiert |


## Daten importieren

**1. Mit welchem Befehl kontrollieren Sie die Struktur einer Tabelle?**

- [ ] SHOW DATABASES;
- [x] SHOW CREATE TABLE *tabellenname*;
- [x] DESC *tabellenname*;
- [x] DESCRIBE *tabellenname*;
- [ ] SELECT * FROM *tabellenname*;
- [ ] SHOW TABLE *tabellenname*;