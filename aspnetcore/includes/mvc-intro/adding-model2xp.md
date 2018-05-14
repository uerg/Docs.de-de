<a name="cli"></a>
## <a name="perform-initial-migration"></a>Ausführen der anfänglichen Migration

Führen Sie in der Befehlszeile die folgenden .NET Core-CLI-Befehle aus:

```console
dotnet ef migrations add InitialCreate
dotnet ef database update
```

Mit dem Befehl `dotnet ef migrations add InitialCreate` wird Code generiert, um das anfängliche Datenbankschema zu erstellen. Das Schema basiert auf dem Modell, das in der Datei *Models/MvcMovieContext.cs* für `DbContext` angegeben ist. Das Argument `Initial` wird verwendet, um die Migrationen zu benennen. Sie können einen beliebigen Namen auswählen, sollten aber der Konvention zufolge einen Namen verwenden, der die Migration beschreibt. Weitere Informationen finden Sie unter [Introduction to migrations (Einführung in Migrationen)](xref:data/ef-mvc/migrations#introduction-to-migrations).

Mit dem Befehl `dotnet ef database update` führen Sie in der Datei  *Migrations/\<time-stamp>_InitialCreate.cs* die Methode `Up` aus, mit der die Datenbank erstellt wird.
