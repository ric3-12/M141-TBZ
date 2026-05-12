

# Einführung, DB-Engines, XAMPP, Workbench

1. **Welches ist die heute am häufigsten verwendete Datenbank-Art?**
   * Relationale Datenbank – dominiert den Markt seit Jahrzehnten

2. **Welche Komponenten sind in einem DB-Server enthalten?**
   * 1 oder mehrere Datenbanken – die eigentlichen Datenspeicher
   * Datenbank-Management-System (DBMS) – verwaltet und steuert den Zugriff auf die Daten

3. **Bei welchen der folgenden Fabrikate handelt es sich um eine relationale Datenbank?**
   * Oracle – kommerzielles RDBMS
   * MySQL – weit verbreitet, Open Source
   * MariaDB – MySQL-Fork, Open Source
   * MS Access – Desktop-RDBMS von Microsoft
   * PostgreSQL – leistungsstarkes Open Source RDBMS

4. **Welches sind Beispiele für Aufgaben eines DB-Clients?**
   * stellt dem Benutzer ein User-Interface für den Datenzugriff zur Verfügung
   * leitet die Befehle des Benutzers an den DB-Server weiter

5. **Welches sind Client-Komponenten von MySQL?**
   * mysql – Kommandozeilen-Client
   * phpMyAdmin – webbasierter Client

6. **Wie heisst die Server-Komponente von MySQL?**
   * mysqld – der eigentliche Datenbankdaemon, läuft im Hintergrund

7. **Beschreiben Sie den Begriff Client/Server-Modell.**
   * Client stellt Benutzeroberfläche bereit und sendet Anfragen an den Server; der Server verarbeitet diese und liefert die Daten zurück. Mehrere Clients können gleichzeitig auf denselben Server zugreifen.

8. **Welche Vorteile hat die Client/Server-Architektur gegenüber einer Desktop-DB?**
   * Mehrbenutzerbetrieb, zentrale Datenhaltung, bessere Sicherheit durch zentrale Benutzerverwaltung, Skalierbarkeit

9. **Wie werden die Daten in einer relationalen Datenbank abgespeichert?**
   * In Tabellen mit Zeilen (Datensätze) und Spalten (Attribute), verknüpft über Primär- und Fremdschlüssel

10. **Was sind die Vorteile, wenn ein DB-Server die referentielle Datenintegrität unterstützt?**
    * Keine verwaisten Datensätze, Konsistenz wird automatisch vom Server erzwungen, fehlerhafte Daten werden auf DB-Ebene verhindert

11. **Welches sind die 4 Gruppen von NoSQL-Datenbanken?**
    * Key-Value-Stores (z. B. Redis)
    * Document Stores (z. B. MongoDB)
    * Column-Family-Stores (z. B. Cassandra)
    * Graph-Datenbanken (z. B. Neo4j)

12. **Was bedeutet DBaaS?**
    * Database as a Service – Datenbank wird als Cloud-Dienst bezogen, z. B. Amazon RDS. Anbieter übernimmt Betrieb, Updates und Backups.

13. **Was sind die Vorteile eines RDBMS gegenüber anderen DB-Modellen?**
    * Standardisierte Abfragesprache SQL, flexible Abfragen via JOINs, ACID-Konformität, Normalisierung reduziert Redundanzen, breite Tool-Unterstützung

14. **DB-Server starten und stoppen"**

    ### XAMPP
    ![Screenshot](/99_Media/image.png)

    ### CMD
    
    ```bash
    net start mysql
    net stop mysql
    ```

15. **DB-Server prüfen**


   ### Task-Manager
   - `Strg + Shift + Esc` → Reiter **Details** → nach `mysqld.exe` suchen

   ### Dienst-Manager
   - `Win + R` → `services.msc` eingeben → Dienst **MySQL** suchen → Status muss **Wird ausgeführt** zeigen

   ### mysql (CLI)
   - Konsole öffnen → `mysql -u root -p` eingeben → wenn Verbindung klappt, läuft der Server

   ### Workbench
   - MySQL Workbench öffnen → auf deine Verbindung klicken → wenn sie sich öffnet ohne Fehler, läuft der Server

   ### phpMyAdmin
   - Browser öffnen → `http://localhost/phpmyadmin` → wenn die Startseite erscheint, läuft der Server