

1.  Welches ist die heute am **häufigsten** verwendete Datenbank-Art?

    - [ ] Hierarchische Datenbank

    - [x] Relationale Datenbank

    - [ ] Objektorientierte Datenbank

    - [ ] Netzwerkförmige Datenbank

2.  Welche **Komponenten** sind in einem DB-Server enthalten?

    - [x] 1 oder mehrere Datenbanken

    - [ ] 1 oder mehrere Datenbank-Anwendungen

    - [x] Datenbank-Management-System (DBMS)

    - [ ] Formulare, Reports und Abfragen

3.  Bei welchen der folgenden **Fabrikate** handelt es sich um eine relationale Datenbank?

    - [x] Oracle

    - [ ] Couch-DB

    - [x] MySQL

    - [x] MariaDB

    - [ ] Mongo-DB

    - [x] MS Access

    - [x] PostgreSQL

4.  Welches sind Beispiele für **Aufgaben** eines DB-Clients?

    - [ ] speichert die eigentlichen Daten

    - [x] stellt dem Benutzer ein User-Interface für den Datenzugriff zur Verfügung

    - [ ] verwaltet Benutzer und Passworte und gewährleistet damit die Sicherheit der Datenbank

    - [x] leitet die Befehle des Benutzers an den DB-Server weiter

5.  Welches sind **Client-Komponenten** von MySQL?

    - [ ] mysqld

    - [ ] my.ini

    - [x] mysql

    - [x] phpMyAdmin

6.  Wie heisst die **Server-Komponente** von MySQL?

    - [ ] phpMyAdmin

    - [ ] Workbench

    - [ ] mysql

    - [x] mysqld

7.  Beschreiben Sie den Begriff Client/Server-Modell.

    Der **Client** stellt die Benutzeroberfläche bereit und sendet Anfragen an den Server. Der **Server** verarbeitet diese Anfragen und liefert die Ergebnisse zurück. Mehrere Clients können gleichzeitig auf denselben Server zugreifen, ohne dass die Daten lokal gespeichert werden müssen.

8.  Welche Vorteile hat die Client/Server-Architektur gegenüber einer Desktop-DB?

    Mehrbenutzerbetrieb (viele Clients gleichzeitig), zentrale Datenhaltung, bessere Sicherheit durch zentrale Benutzerverwaltung, einfachere Backups und Wartung, Skalierbarkeit.

9.  Wie werden die Daten in einer relationalen Datenbank abgespeichert?

    In **Tabellen** mit Zeilen (Datensätze) und Spalten (Attribute). Tabellen werden über **Primär- und Fremdschlüssel** miteinander verknüpft.

10.  Was sind die Vorteile, wenn ein DB-Server die **referentielle Datenintegrität** unterstützt?

    Es entstehen keine verwaisten Datensätze (z. B. Bestellungen ohne zugehörigen Kunden). Die Konsistenz der Daten wird automatisch vom Server erzwungen, fehlerhafte oder inkonsistente Einträge werden auf Datenbankebene verhindert.

11.  Welches sind die 4 Gruppen von **NoSQL**-Datenbanken, die zurzeit relevant sind?

    | Gruppe | Beispiel |
    |--------|---------|
    | **Key-Value-Stores** | Redis |
    | **Document Stores** | MongoDB |
    | **Column-Family-Stores** | Cassandra |
    | **Graph-Datenbanken** | Neo4j |

12.  Was bedeutet **DBaaS**? Erklären Sie anhand eines Beispiels.

    **Database as a Service** – die Datenbank wird als Cloud-Dienst bezogen. Der Anbieter übernimmt Betrieb, Updates und Backups. Beispiel: **Amazon RDS** stellt eine MySQL-Datenbank in der Cloud bereit, ohne dass man selbst einen Server installieren oder warten muss.

13.  Was sind die Vorteile eines RDBMS gegenüber anderen DB-Modellen?

    Standardisierte Abfragesprache **SQL**, flexible Abfragen via JOINs, **ACID-Konformität** (Datenkonsistenz), Normalisierung reduziert Redundanzen, breite Tool- und Treiber-Unterstützung.

14.  DB-Server starten und stoppen

    *Via XAMPP Control Panel:* Neben MySQL auf **Start** bzw. **Stop** klicken.

    *Via Konsole (CMD):*
    ```bash
    net start mysql
    net stop mysql
    ```

    *Via MySQL Workbench:* Unter **Server** → **Startup/Shutdown** den Server starten oder stoppen (nur wenn MySQL als Windows-Dienst läuft).

15.  DB-Server prüfen

    | Methode | Vorgehen |
    |---------|----------|
    | **Task-Manager** | `Strg + Shift + Esc` → Reiter **Details** → nach `mysqld.exe` suchen |
    | **Dienst-Manager** | `Win + R` → `services.msc` → Dienst **MySQL** suchen → Status muss **Wird ausgeführt** zeigen |
    | **mysql (CLI)** | Konsole öffnen → `mysql -u root -p` → wenn Verbindung klappt, läuft der Server |
    | **Workbench** | Verbindung öffnen → wenn sie sich ohne Fehler öffnet, läuft der Server |
    | **phpMyAdmin** | Browser → `http://localhost/phpmyadmin` → wenn Startseite erscheint, läuft der Server |