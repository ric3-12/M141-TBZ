![](../x_res/tbz_logo.png)

# M141 - DB-Systeme in Betrieb nehmen


# ![](../x_res/CP.png) Checkpoint 4. / 5.Tag


## Datenbank-Sicherheit

1.  Was bedeutet der Begriff "Authentifizierung" im Zusammenhang mit einem DB-Server?

    - [ ] Prüfung der Privilegien des Benutzers

    - [x] Antwort auf die Frage: Wer?

    - [x] Identitätsprüfung

    - [ ] Antwort auf die Frage: Was?

2.  Wann werden Änderungen im Zugriffssystem von MySQL wirksam?

    - [x] sofort nach Eingabe der Änderung

    - [x] nach dem Befehl FLUSH PRIVILEGES

    - [x] nach dem Neustart des DB-Servers

    - [x] nach dem Befehl GRANT

3.  Was bewirkt der SQL-Befehl GRANT ... ON ... TO ...;

    - [x] Privileg(ien) erteilen

    - [ ] Privileg(ien) wegnehmen

    - [x] User erstellen, falls noch nicht vorhanden

    - [ ] User löschen

4.  Mit welchem Befehl werden Privilegien kontrolliert?

    - [ ] REVOKE ... ON ... FROM ;

    - [ ] SELECT user, host, password FROM user ;

    - [ ] SHOW TABLES;

    - [x] SHOW GRANTS FOR ... ;

5.  Welches sind die beiden wichtigsten DCL-Befehle (data control)?

    - [ ] SELECT

    - [x] REVOKE

    - [ ] DELETE

    - [x] GRANT

6.  Was ist nötig, dass Benutzer "meier" keinen Zugang mehr auf den DB-Server hat.

    - [ ] in Systemtabelle user für diesen Benutzer jedes Privileg auf "N" setzen

    - [x] mit DELETE FROM user WHERE user = 'meier'; und FLUSH PRIVILEGES;

    - [ ] in allen Systemtabellen für diesen Benutzer jedes Privileg auf "N" setzen

    - [ ] dem Benutzer das GRANT-Privileg (Grant_priv) wegnehmen

7.  Erklären Sie den Begriff "Autorisierung" im Zusammenhang mit einem DB-Server.

    Autorisierung beantwortet die Frage **„Was?"** — also was ein bereits authentifizierter Benutzer auf dem DB-Server tun darf. Sie legt fest, welche Privilegien (z.B. SELECT, INSERT, UPDATE, DELETE) ein Benutzer auf welcher Ebene (global, Datenbank, Tabelle, Spalte) besitzt. In MySQL/MariaDB wird dies über das `GRANT`- und `REVOKE`-System gesteuert und in den Systemtabellen der `mysql`-Datenbank gespeichert.

8.  Wann wird das Schlüsselwort IDENTIFIED BY verwendet?

    `IDENTIFIED BY` wird verwendet, wenn beim Erteilen von Privilegien mit `GRANT` gleichzeitig ein Passwort für den Benutzer gesetzt oder geändert werden soll. Beispiel:

    ```sql
    GRANT SELECT ON hotel.* TO hotel_user@localhost IDENTIFIED BY 'Passw0rd';
    ```

    Falls der Benutzer noch nicht existiert, wird er dabei automatisch erstellt.

9.  Ergänzen Sie den Befehl REVOKE ... ON ... FROM ... ; mit eigenen Angaben.

    ```sql
    REVOKE INSERT, UPDATE ON hotel.* FROM hotel_user@localhost;
    ```

    Dieser Befehl entzieht dem Benutzer `hotel_user` (auf localhost) die Rechte `INSERT` und `UPDATE` auf alle Tabellen der Datenbank `hotel`.

10.  Beschreiben Sie den Begriff der MySQL-Testdatenbank.

    Die MySQL-Testdatenbank (`test`) ist eine bei der Installation standardmässig angelegte, leere Datenbank. Auf sie hat jeder Benutzer — auch anonyme Benutzer ohne Passwort — automatisch vollen Zugriff. Sie dient zum schnellen Ausprobieren von SQL-Befehlen, stellt aber ein **Sicherheitsrisiko** dar und sollte in Produktivsystemen gelöscht werden:

    ```sql
    DROP DATABASE test;
    ```

11.  Mit welchem Befehl ändern Sie das Passwort von Benutzer Meier auf "abc123"?

    ```sql
    SET PASSWORD FOR meier@localhost = PASSWORD('abc123');
    ```

    Alternativ (ab MariaDB 10.4):

    ```sql
    ALTER USER meier@localhost IDENTIFIED BY 'abc123';
    ```

12.  Geben Sie eine Erklärung für folgende Fehlermeldung.

    ```
    GRANT USAGE ON *.* TO abc IDENTIFIED BY 'a12';
    ERROR 1045: Access denied for user: '@127.0.0.1'
    ```

    Der Fehler bedeutet, dass der aktuell eingeloggte Benutzer (hier ein **anonymer Benutzer** ohne Namen, erkennbar am leeren String vor `@127.0.0.1`) nicht das `GRANT`-Privileg besitzt. Um `GRANT`-Befehle ausführen zu dürfen, muss man als Benutzer mit dem `GRANT OPTION`-Privileg eingeloggt sein — z.B. als `root`. Die Lösung ist, den Befehl als `root`-User auszuführen:

    ```bash
    mysql -u root -p
    ```

13.  Korrigieren Sie den folgenden Befehl:

    ```
    REVOKE ALL FROM ''@localhost;
    ERROR 1064: You have an error
    ```

    Fehler: Es fehlt `ON` mit Angabe des Geltungsbereichs. Der korrekte Befehl lautet:

    ```sql
    REVOKE ALL PRIVILEGES ON *.* FROM ''@localhost;
    ```

    Die vollständige Syntax von `REVOKE` erfordert zwingend `ON Geltungsbereich` zwischen `ALL PRIVILEGES` und `FROM`.

---

# Lernfragen: Tag 5 – Zugriffsberechtigung (aus README)

## Zugriffsberechtigung & Autorisierung

1.  Was ist der Unterschied zwischen **Authentifizierung** und **Autorisierung**?

    **Authentifizierung** beantwortet die Frage „Wer?" — die Identität des Benutzers wird geprüft (Login mit Benutzername und Passwort).  
    **Autorisierung** beantwortet die Frage „Was?" — es wird geprüft, welche Aktionen (Privilegien) der bereits authentifizierte Benutzer ausführen darf.

---

2.  Auf welchen **Ebenen** können Zugriffsrechte in MySQL/MariaDB vergeben werden? Nennen Sie alle fünf mit je einem Syntax-Beispiel.

    | Ebene | Syntax-Beispiel |
    |---|---|
    | **Global** | `ON *.*` |
    | **Datenbank** | `ON mydb.*` |
    | **Tabelle** | `ON mydb.tabelle1` |
    | **Spalte** | `GRANT SELECT (name, email) ON mydb.tabelle1 TO user@host` |
    | **Stored Routine** | `ON PROCEDURE mydb.meineProzedur` |

---

3.  In welchen **Systemtabellen** speichert MariaDB die Privilegien? Welche Tabelle ist seit Version 10.4 die eigentliche Quelle?

    | Tabelle | Speichert Privilegien für |
    |---|---|
    | `mysql.global_priv` | Global (`*.*`) — Passwörter, globale Rechte als JSON |
    | `mysql.db` | Datenbank-Ebene |
    | `mysql.tables_priv` | Tabellen-Ebene |
    | `mysql.columns_priv` | Spalten-Ebene |
    | `mysql.procs_priv` | Stored Procedures / Functions |
    | `mysql.proxies_priv` | Proxy-User (Impersonation) |

    Seit MariaDB 10.4 ist **`mysql.global_priv`** die eigentliche Quelltabelle. Die bekannte `mysql.user` ist nur noch eine **View** darauf (für Kompatibilität mit älteren Tools).

---

4.  Was bewirkt das Privileg **USAGE** und wozu wird es eingesetzt?

    `USAGE` bedeutet „keine Rechte" — der Benutzer darf sich einloggen (Connect), aber keine Daten lesen oder ändern. Es wird verwendet, um einen Benutzer zu erstellen oder Verbindungsattribute (z.B. SSL) zu setzen, ohne ihm Datenzugriff zu gewähren.

---

5.  Was ist der Unterschied zwischen **GRANT** und **REVOKE**? Schreiben Sie die allgemeine Syntax auf.

    `GRANT` erteilt Privilegien, `REVOKE` entzieht sie.

    ```sql
    -- Privilegien erteilen:
    GRANT privileg1 [, privileg2, ...]
    ON [datenbank.]tabelle
    TO user@host [IDENTIFIED BY 'passwort'] [WITH GRANT OPTION];

    -- Privilegien entziehen:
    REVOKE privileg1 [, privileg2, ...]
    ON [datenbank.]tabelle
    FROM user@host;
    ```

    Eselsbrücke: `GRANT ... TO` und `REVOKE ... FROM`.

---

6.  Was bedeutet **WITH GRANT OPTION** und wann wird es verwendet?

    Mit `WITH GRANT OPTION` darf der Benutzer **seine eigenen Rechte** an andere Benutzer weitergeben. Es wird für Admin-User oder Datenbankverantwortliche verwendet, die selbst Benutzer verwalten sollen, ohne Root-Rechte zu haben. Beispiel:

    ```sql
    GRANT ALL ON hotel.* TO hotel_admin@localhost WITH GRANT OPTION;
    ```

---

7.  Warum sollte `FLUSH PRIVILEGES;` nach manuellen Änderungen in den Systemtabellen ausgeführt werden?

    MySQL/MariaDB lädt die Privilegien beim Start in den Arbeitsspeicher. Direkte Änderungen in den Systemtabellen (z.B. via `UPDATE` oder `DELETE`) werden erst nach `FLUSH PRIVILEGES` aktiv, da dadurch der Cache neu geladen wird. Bei `GRANT`/`REVOKE`-Befehlen ist `FLUSH PRIVILEGES` hingegen nicht nötig, da diese es automatisch auslösen.

---

8.  Was sind **Rollen** (Roles) in MariaDB und wozu dienen sie?

    Rollen (ab MariaDB 10.x) sind benannte Gruppen von Privilegien. Statt jedem Benutzer einzeln Rechte zu vergeben, werden Rechte einer Rolle zugewiesen und die Rolle dann dem Benutzer übertragen. Das vereinfacht die Verwaltung bei vielen Benutzern mit gleichen Rechten. Beispiel:

    ```sql
    CREATE ROLE verkauf;
    GRANT SELECT, INSERT ON kunden.* TO verkauf;
    GRANT verkauf TO user@host;
    SET ROLE verkauf;  -- Benutzer muss Rolle aktivieren!
    ```

---

9.  Was ist eine **Zugriffsmatrix** und wozu wird sie verwendet?

    Eine Zugriffsmatrix ist eine tabellarische Übersicht, die für jeden Benutzer bzw. jede Benutzergruppe (Rolle) festlegt, welche Operationen (SELECT, INSERT, UPDATE, DELETE) auf welchen Tabellen oder Attributen erlaubt sind. Sie dient als **Planungsdokument** vor der eigentlichen GRANT-Vergabe und schafft Übersicht über das Rechtesystem einer Datenbank.

---

10.  Was ist der **pma-User** in phpMyAdmin und warum ist er sicherheitsrelevant?

    Der `pma`-User ist ein Steuer-Benutzer, der bei der phpMyAdmin-Installation automatisch angelegt wird. Er hat eingeschränkte Rechte (nur auf `phpmyadmin.*`) und dient phpMyAdmin intern zur Verwaltung. Er ist sicherheitsrelevant, weil er standardmässig **ohne Passwort** angelegt wird. Ohne gesetztes Passwort in `config.inc.php` kann phpMyAdmin nicht gestartet werden. Das Passwort setzt man mit:

    ```sql
    SET PASSWORD FOR pma@localhost = PASSWORD('irgendwas');
    ```

---

11.  Wie lautet der Befehl, um alle Privilegien für einen Benutzer `hotel_admin` auf die Datenbank `hotel` inklusive Weitergabe-Recht zu vergeben?

    ```sql
    GRANT ALL ON hotel.* TO hotel_admin@localhost WITH GRANT OPTION;
    FLUSH PRIVILEGES;
    SHOW GRANTS FOR hotel_admin@localhost;
    ```

---

12.  Wie überprüfen Sie die Privilegien eines bestimmten Benutzers?

    ```sql
    SHOW GRANTS FOR hotel_admin@localhost;
    ```

    Alternativ kann man in die Systemtabellen schauen:

    ```sql
    SELECT * FROM mysql.user;          -- Gewohnte Ansicht (View)
    SELECT * FROM mysql.global_priv;   -- Echte Rohdaten (JSON-Format)
    ```
