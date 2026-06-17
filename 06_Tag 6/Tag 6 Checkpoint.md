

## 1. Konfiguration

1.  Welche **drei Arten** gibt es, den DB-Server (mysqld) zu konfigurieren?

    1. **Konfigurationsdateien** (`my.ini` / `my.cnf`) — Parameter werden dauerhaft eingetragen, da der Server meist als Dienst läuft und keine direkten Kommandozeilen-Parameter beim Start entgegennimmt.
    2. **Kommandozeilen-Parameter** beim Start von `mysqld` oder bei Admin-Programmen wie `mysql` (z.B. `mysqld --language=german`).
    3. **Systemvariablen**, die zur Laufzeit über `SHOW VARIABLES` abgefragt (und teilweise mit `SET GLOBAL` verändert) werden können.

---

2.  Was ist beim Eintragen von Pfaden in der `my.ini` unter Windows zu beachten?

    Pfade müssen, wie unter UNIX, mit Schrägstrich `/` angegeben werden — nicht mit dem unter Windows üblichen Backslash `\`.

---

3.  Mit welchem Befehl kann eine Konfigurationsdatei **vor dem Serverstart** auf Fehler geprüft werden, und warum ist das wichtig?

    ```cmd
    mysqld.exe --validate-config
    mysqld.exe --defaults-file=C:\path\to\my.ini --validate-config
    ```

    Das ist wichtig, weil ein fehlerhafter Eintrag (z.B. Tippfehler, `-` statt `_`) dazu führen kann, dass der Server nicht mehr startet. Der Check deckt solche Fehler vorab auf, ohne den Dienst tatsächlich neu zu starten.

---

4.  Wann werden Änderungen in der `my.ini` wirksam?

    Erst nach einem **Neustart** des betroffenen Programms — bei Server-spezifischen Optionen muss der gesamte DB-Server neu gestartet werden.

---

## 2. Überwachung (Logging)

5.  Welche fünf Logging-Typen gibt es, und welcher davon kann **nicht** ausgeschaltet werden?

    | Log-Typ | Zweck | Abhängigkeit |
    |---|---|---|
    | Error Log | Server-Fehler, Start/Stop | Immer aktiv |
    | Binary Log (Update Log) | Alle Datenänderungen (Recovery, Replikation) | Muss aktiviert werden |
    | General Query Log | Alle Befehle | Nur für Debugging |
    | Slow Query Log | Langsame Abfragen | Für Optimierung |
    | Transaction Log | InnoDB-Transaktionen | Crash-Recovery |

    Das **Error Log** kann nicht ausgeschaltet werden.

---

6.  Warum muss bei der Verwendung von **Transaktionen** zwingend binäres Logging (statt Textmodus-Logging) verwendet werden?

    Nur binäres Logging behandelt Transaktionen korrekt. Im Textmodus würden auch Befehle protokolliert, die später durch ein `ROLLBACK` wieder zurückgenommen wurden — das Log wäre also inkonsistent mit dem tatsächlichen Datenbestand.

---

7.  Mit welchem Parameter wird das Binary Log aktiviert, und welche Bedingungen führen zu einer neuen Logdatei (Extension +1)?

    Aktivierung über `log-bin=mysql-bin` in der `my.ini` (standardmässig auskommentiert).

    Eine neue Logdatei wird erstellt:
    - beim Neustart des DB-Servers
    - beim Erreichen der mit `max_binlog_size` definierten Maximalgrösse
    - durch den Befehl `FLUSH LOGS`

---

8.  Wie zeigt man den Inhalt einer binären Logdatei lesbar an?

    ```cmd
    mysqlbinlog -u root -p C:\...\MySQL\data\mysql-bin.000001
    ```

    Da Binärlogs nicht direkt mit einem Texteditor lesbar sind, wird der spezielle Client `mysqlbinlog` benötigt.

---

9.  Was unterscheidet das **General Query Log** vom **Slow Query Log**?

    Das General Query Log zeichnet **jeden** Verbindungsaufbau und **jeden** Befehl auf — es wird sehr schnell sehr groß und ist nur für Debugging gedacht. Das Slow Query Log zeichnet hingegen nur **langsame Abfragen** auf und liefert damit direkte Hinweise zur Performance-Optimierung (z.B. fehlende Indizes).

---

## 3. Sicherung (Backup & Recovery)

10.  Wie erstellt man ein vollständiges Backup einer Datenbank mit `mysqldump`, und was bewirkt die Option `--opt`?

    ```cmd
    mysqldump --user=root --password=pwd --opt firma > backup.sql
    ```

    `--opt` fasst mehrere Einzeloptionen zu einer optimalen Backup-Einstellung zusammen, u.a. `quick` (vermeidet Zwischenspeicherung im RAM), `add-drop-table`, `add-locks`, `extended-insert` und `lock-tables`.

---

11.  Wie wird die **Integrität** eines Backups während der Sicherung gewährleistet?

    Mit der Option `--lock-tables` wird ein READ LOCK auf die betroffenen Tabellen gesetzt, sodass keine Änderungen während des Backups erfolgen können. Alternativ ermöglicht `--single-transaction` einen konsistenten Snapshot, funktioniert aber nur, wenn alle beteiligten Tabellentypen Transaktionen unterstützen.

---

12.  Warum ist die Angabe des Passworts direkt in der Befehlszeile (`--password=x`) ein Sicherheitsrisiko, und welche Alternativen gibt es?

    Wird der Befehl geloggt (z.B. im Shell-Verlauf oder Prozess-Log), erscheint das Passwort im Klartext. Sicherere Alternativen sind `mysql_config_editor` oder das Auslagern der Zugangsdaten in eine separate Konfigurationsdatei, die über `--defaults-extra-file` referenziert wird.

---

13.  Beschreiben Sie die **Recovery-Schritte** nach einem Totalverlust der Datenbank (Zusammenspiel von Backup und Binary Log).

    1. Letztes vollständiges Backup einlesen: `mysql.exe -u root -p firma < backup.sql`
    2. Alle seit diesem Backup erstellten Binary-Log-Dateien der Reihe nach (älteste zuerst) einlesen:
       ```cmd
       mysqlbinlog <host>-bin.000001 | mysql.exe -u root -p
       mysqlbinlog <host>-bin.000002 | mysql.exe -u root -p
       ```
    Das Binary Log schliesst die Lücke zwischen dem letzten Backup und dem Crash-Zeitpunkt.

---

14.  Warum sollten Binary Logs nicht mehrfach auf eine Datenbank angewendet werden, und wie verhindert man das?

    Würden INSERTs, UPDATEs und DELETEs mehrfach ausgeführt, entstünde Datenchaos (z.B. doppelte Datensätze). Mit dem Parameter `--master-data=2` beim Backup wird die Position im Binary Log als Kommentar ins Backup eingetragen; mit `--start-position` bei `mysqlbinlog` kann dann gezielt nur ab dieser Position gelesen werden.

---

15.  Welche drei Komponenten umfasst der geschätzte Speicherbedarf einer MyISAM-Tabelle?

    - **Nutzdaten** (`*.MYD`): Anzahl_Datensätze × [Summe(Bytes pro Attribut) + 5]
    - **Indizes** (`*.MYI`): Anzahl_Schlüssel × [(Schlüssellänge + 4) / 0.67]
    - **Systemdaten** (`*.FRM`): 8500 + (Anzahl_Attribute × 40)

---

## 4. Optimierung

16.  Welche drei Ziele verfolgt die Optimierung einer Datenbank?

    Performance verbessern (schnellere Ausführung von SQL-Befehlen), Speicherplatz einsparen (schnellere Übertragung von/auf Festplatte) und Portabilität ermöglichen (Übertragung auf einen anderen Server).

---

17.  Was bewirkt der Befehl `OPTIMIZE TABLE`, und wann sollte er eingesetzt werden?

    ```sql
    OPTIMIZE TABLE person;
    ```

    Er entfernt nicht genutzten Speicherplatz aus einer Tabellendatei und sorgt dafür, dass zusammengehörende Daten wieder physisch zusammen gespeichert werden. Sinnvoll bei Tabellen mit häufigen DELETE- und UPDATE-Befehlen, da diese die Datei fragmentieren. Gilt nur für MyISAM/Aria.

---

18.  Wie funktioniert `EXPLAIN SELECT`, und welche Spalten der Ausgabe sind besonders aussagekräftig?

    `EXPLAIN` vor einem SELECT zeigt den Ausführungsplan, ohne die Abfrage tatsächlich auszuführen.

    ```sql
    EXPLAIN SELECT COUNT(*)
    FROM buchung, person
    WHERE buchung.PersID = person.PersID;
    ```

    Die Spalte `table` zeigt die Lesereihenfolge der Tabellen, `key` zeigt den verwendeten Index (NULL = kein Index), und `rows` zeigt die geschätzte Anzahl zu untersuchender Zeilen. Multipliziert man alle Werte der `rows`-Spalte, erhält man einen Anhaltspunkt für den Suchaufwand — dieser sollte möglichst klein sein.

---

19.  Welche Abfragearten lassen sich laut Optimierungshinweisen **nicht** durch Indizes optimieren?

    - Abfragen mit `NOT` und `<>`
    - Abfragen mit `LIKE`, wenn das `%` am Musteranfang steht
    - Abfragen, die Funktionen auf das gesuchte Attribut anwenden

---

20.  Nennen Sie vier wichtige Speicherparameter für das Server-Tuning mit ihrer jeweiligen Default-Grösse.

    | Parameter | Default | Bedeutung |
    |---|---|---|
    | `key_buffer_size` | 8M | Speicher für Indizes |
    | `table_cache` | 64 | max. Anzahl geöffneter Tabellen |
    | `sort_buffer` | 2M | Buffer zum Sortieren/Gruppieren |
    | `read_buffer_size` | 128K | Speicher für sequentielles Lesen |

---

21.  Was ist die Aufgabe des **Query Cache**, und mit welchem Befehl wird er vollständig geleert?

    Der Query Cache speichert die Ergebnisse von SQL-Abfragen. Wird exakt dieselbe Abfrage erneut ausgeführt, kann das gespeicherte Ergebnis direkt zurückgegeben werden, ohne die Abfrage neu zu berechnen.

    ```sql
    RESET QUERY CACHE;
    ```

---

22.  Ab welcher Datenmenge lohnt sich eine Abfrage-Optimierung laut README überhaupt erst?

    Erst wenn Tabellen über 10'000 Datensätze enthalten oder die Gesamtgrösse der Datenbank die RAM-Grösse des DB-Servers übersteigt. Bei kleineren Datenmengen befindet sich die DB nach den ersten Abfragen meist vollständig im RAM, sodass Indizes kaum einen spürbaren Geschwindigkeitsvorteil bringen.