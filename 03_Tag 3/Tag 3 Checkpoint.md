![](../x_res/tbz_logo.png)

# M141 - DB-Systeme in Betrieb nehmen


1.  Wie bezeichnet man die Ausführung mehrerer DB-Operationen in einem einzigen Schritt?

    - [ ] Referentielle Integrität

    - [ ] Replikation

    - [x] Transaktion

    - [ ] Storage Procedure

2.  Warum sollen Locks möglichst schnell freigegeben werden?

    - [ ] damit das DBMS nicht zu stark belastet wird

    - [x] damit andere DB-Anwender nicht lange warten müssen

    - [ ] damit niemand die Daten ändern kann

    - [x] damit möglichst viele Benutzer gleichzeitig auf die DB zugreifen können

3.  Welches ist das Standard-Tabellenformat von MySQL (MariaDB)?

    - [ ] InnoDB

    - [x] MyISAM

    - [ ] ARIA

    - [ ] ISAM

4.  Wann verwenden Sie das InnoDB-Tabellenformat?

    - [ ] wenn möglichst schnell auf die Daten zugegriffen werden muss

    - [x] wenn auf gar keinen Fall ein Datenverlust vorkommen darf

    - [x] wenn viele Benutzer gleichzeitig Daten ändern

    - [ ] wenn bei sehr vielen Daten nicht beliebig viel Speicherplatz vorhanden ist

5.  Was trifft auf den sog. Tablespace zu?

    - [ ] Datei, welche die Daten der entsprechenden Tabelle enthält (\*.MYD)

    - [ ] Datei, welche Beschreibung, Daten und Indexe einer Tabelle enthält

    - [x] Datei, welche alle InnoDB-Tabellen enthält (virtueller Speicher)

    - [x] wird nach Erreichen von x MB automatisch vergrössert (falls autoextend eingeschaltet)
    
6.  Mit welchen Befehlen werden Transaktionen gesteuert?

    - [ ] UNLOCK TABLES;

    - [x] COMMIT; oder ROLLBACK;

    - [ ] ALTER TABLE ... TYPE= ...;

    - [x] BEGIN; oder START TRANSACTION;

7.  Was trifft auf das Locking bei Transaktionen auf InnoDB-Tabellen zu?

    - [ ] in Transaktionen kommt Table locking zur Anwendung

    - [x] es wird Row locking angewendet

    - [ ] es werden alle Datensätze der entsprechenden Tabelle(n) gesperrt

    - [x] es werden nur die gerade bearbeiteten Datensätze gesperrt

8.  Welches sind Vorteile der InnoDB-Tabellen gegenüber MyISAM-Tabellen?

    InnoDB unterstützt **Transaktionen** (BEGIN, COMMIT, ROLLBACK) sowie **referentielle Integrität** (Fremdschlüssel). Dank **Row-Level-Locking** werden nur betroffene Datensätze gesperrt, was gleichzeitige Zugriffe effizienter macht. Zusätzlich bietet InnoDB ein **automatisches Crash-Recovery** sowie eine automatische **Deadlock-Erkennung** mit ROLLBACK.

9.  In welchen Dateien wird die MyISAM-Tabelle KUNDEN gespeichert?

    | Datei | Inhalt |
    |-------|--------|
    | `KUNDEN.frm` | Tabellenbeschreibung (Struktur) |
    | `KUNDEN.MYD` | Datensätze (Data) |
    | `KUNDEN.MYI` | Indexe (Index) |

10.  Notieren Sie den SQL-Befehl, der die InnoDB-Tabelle BESTELLUNGEN erstellt.

    ```sql
    CREATE TABLE BESTELLUNGEN (
        id_b     INT AUTO_INCREMENT,
        datum    DATE,
        betrag   DECIMAL(10,2),
        PRIMARY KEY (id_b)
    ) ENGINE = InnoDB;
    ```

11.  Welche Locking-Art ist a) bei MyISAM-Tabellen b) bei InnoDB-Tabellen möglich?

    | Tabellentyp | Locking-Art |
    |-------------|-------------|
    | **a) MyISAM** | Table-Level-Locking – immer die gesamte Tabelle wird gesperrt |
    | **b) InnoDB** | Row-Level-Locking – nur die gerade bearbeiteten Datensätze werden gesperrt |

12.  Beschreiben Sie den Begriff Datenbank-Transaktion!

    Eine Datenbank-Transaktion ist eine **Gruppe von SQL-Befehlen**, die als eine unteilbare Einheit behandelt wird – nach dem Prinzip **„alles oder nichts"**. Sie beginnt mit `BEGIN` (oder `START TRANSACTION`) und endet entweder mit `COMMIT` (Änderungen werden dauerhaft gespeichert) oder `ROLLBACK` (alle Änderungen werden rückgängig gemacht). Transaktionen garantieren Datenkonsistenz bei gleichzeitigen Zugriffen und bei Systemabstürzen.

13.  Beschreiben Sie die Bedeutung von I in der Abkürzung ACID.

    **I = Isoliertheit (Isolation):** Gleichzeitig laufende Transaktionen dürfen sich **gegenseitig nicht beeinflussen**. Jede Transaktion arbeitet so, als wäre sie die einzige aktive Transaktion im System. Dies wird durch Sperrmechanismen (Locks), Arbeitskopien und Timestamps sichergestellt. Die Sperrungen sollen dabei so kurz und begrenzt wie möglich gehalten werden, damit die Performance der anderen Operationen nicht beeinträchtigt wird.

14.  Wie stellen Transaktionen bei einem DB-Server-Crash die Datenkonsistenz sicher? (Schwierig)

    InnoDB führt ein **Transaktions-Log** (Write-Ahead-Log, Dateien `ib_logfile0`, `ib_logfile1`). Jede Änderung wird **zuerst ins Log geschrieben**, bevor sie in die eigentliche Datenbankdatei übernommen wird. Nach einem Crash werden beim nächsten Serverstart alle **vollständig abgeschlossenen** Transaktionen (mit `COMMIT` im Log) **nachgespielt (Redo)** und alle **unvollständigen** Transaktionen (kein `COMMIT` im Log) automatisch **rückgängig gemacht (Undo/Rollback)**. So befindet sich die Datenbank danach stets in einem konsistenten Zustand.
  
15. Mit welcher Locking-Art wartet ein SELECT-Befehl, bis alle Transaktionen auf die angeforderte Tabelle entsperrt sind? 

    ```sql
    SELECT ... LOCK IN SHARE MODE
    ```
    Dieser Befehl wartet, bis alle offenen Exclusive Locks auf die betroffenen Datensätze aufgelöst sind, bevor er die Daten liest. Gleichzeitig setzt er einen Shared Lock, sodass andere Benutzer die Daten zwar lesen, aber nicht verändern können.
          
16. Wie muss Autocommit gesetzt werden, damit jeder SQL-Befehl zu einer Transaktion gehört und damit explizit mit COMMIT abgeschlossen werden muss, damit er ausgeführt wird? 

    ```sql
    SET AUTOCOMMIT = 0;
    ```
    Damit gehört jeder SQL-Befehl automatisch zu einer Transaktion und wird erst mit einem expliziten `COMMIT;` dauerhaft gespeichert. Ohne `COMMIT` werden alle Änderungen bei Verbindungsabbruch automatisch zurückgerollt (`ROLLBACK`).