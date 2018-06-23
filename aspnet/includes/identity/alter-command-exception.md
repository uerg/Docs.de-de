> Einige Befehle werden nicht unterstützt, wenn die app SQLite als Identitätsspeicher Daten verwendet. Aufgrund der Einschränkungen in das Datenbankmodul `Alter` Befehle lösen die folgende Ausnahme:
>
> "System.NotSupportedException: SQLite diese Migrationsvorgang nicht unterstützt." 
>
> Umgehen führen Sie Code First-Migrationen auf die Datenbank so ändern Sie die Tabellen aus.
