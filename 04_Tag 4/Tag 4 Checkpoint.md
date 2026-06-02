


1.  Welcher Befehl testet die Verbindung zum Server-Rechner mit Adresse 139.79.124.97?

    - [ ] mysql -h 139.79.124.97

    - [ ] ipconfig

    - [x] ping 139.79.124.97

    - [x] mysqladmin -h 139.79.124.97 -u root -p ping

2.  Wozu wird der Parameter -h bei MySQL verwendet?

    - [ ] bewirkt die Abfrage des Passworts

    - [ ] bewirkt die Verbindung als bestimmter Benutzer

    - [ ] Angabe der Adresse des Client-Rechners

    - [x] Angabe der Adresse des Server-Rechners

3.  Was bewirkt der Befehl 'mysqldump -h 139.79.124.97 hotel \> datei.txt'?

    - [ ] Backup der DB hotel in die Datei datei.txt auf Adresse 139.79.124.97

    - [x] Backup der angegebenen DB auf dem Server mit der IP-Adresse 139.79.124.97

    - [ ] Restore der Datenbank hotel auf dem Server mit der Adresse 139.79.124.97

    - [ ] Ausführen des SQL-Skripts datei.txt auf Adresse 139.79.124.97 auf die DB hotel

4.  Welche Aufgabe hat der ODBC-Driver?

    - [ ] passt die SQL-Befehle dem entsprechenden DB-Server an

    - [ ] ermöglicht das Erstellen und Konfigurieren von ODBC-Datenquellen (DSN)

    - [x] ermöglicht den einheitlichen Zugriff einer Applikation auf verschiedene Datenbanken

    - [ ] ermöglicht den Zugriff einer Applikation auf eine bestimmte DB

5.  Wie greifen Sie vom Konsolenfenster auf einen DB-Server mit Adresse 139.79.124.97 zu?

    - [ ] mysqladmin -h 139.79.124.97

    - [ ] mysql -h 139.79.124.97 hotel \< hotel.bkp

    - [x] mysql -h 139.79.124.97 -u root -p

    - [ ] ping 139.79.124.97

1.  Welche Aufgaben hat der DB-Server im Gegensatz zum DB-Client?

    Der **DB-Server** übernimmt die eigentliche Datenverwaltung: Er speichert die Daten, verarbeitet SQL-Anfragen, kontrolliert Zugriffsrechte und stellt die Datenkonsistenz sicher. Der **DB-Client** ist nur die Schnittstelle des Benutzers – er sendet SQL-Befehle an den Server und zeigt die zurückgelieferten Ergebnisse an, enthält aber selbst keine Daten.

2.  Weshalb benutzt man MS Access z.B. zusammen mit einem MySQL-Server?

    MS Access bietet eine benutzerfreundliche Oberfläche (Formulare, Berichte, Abfragedesigner) für Endbenutzer ohne SQL-Kenntnisse. MySQL übernimmt dabei die eigentliche Datenspeicherung und ist stabiler, mehrbenutzerfähig und netzwerkfähig. Access verbindet sich über **ODBC** mit dem MySQL-Server und dient als reines Frontend/Client.

3.  Wie bestimmen Sie die IP-Adresse des Server-Rechners?

    Direkt auf dem Server: `ip a` (Linux) bzw. `ipconfig` (Windows). Vom Netz aus kann der Hostname über `nslookup <hostname>` oder `ping <hostname>` aufgelöst werden, sofern DNS konfiguriert ist.

4.  Wie prüfen Sie, ob der DB-Server auf Adresse 139.79.124.97 läuft?

    ```bash
    mysqladmin -h 139.79.124.97 -u root -p ping
    ```
    Antwortet der Server mit `mysqld is alive`, ist der MySQL-Dienst aktiv. Vorab kann mit `ping 139.79.124.97` geprüft werden, ob der Host überhaupt im Netz erreichbar ist.

5.  Welcher Befehl führt das SQL-Skript xy.sql auf die DB hotel auf Adresse 139.79.124.97 aus?

    ```bash
    mysql -h 139.79.124.97 -u root -p hotel < xy.sql
    ```