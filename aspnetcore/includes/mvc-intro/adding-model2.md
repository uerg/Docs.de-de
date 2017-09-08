## <a name="add-initial-migration-and-update-the-database"></a>Hinzufügen der anfänglichen Migration und Aktualisierung der Datenbank

* Öffnen Sie ein Eingabeaufforderungsfenster, und navigieren Sie zum Projektverzeichnis. (Das Verzeichnis mit den *Startup.cs* Datei).

* Führen Sie an der Eingabeaufforderung die folgenden Befehle aus:

  ```console
  dotnet restore
  dotnet ef migrations add Initial
  dotnet ef database update
  ```
  
  [.NET Core](http://go.microsoft.com/fwlink/?LinkID=517853) ist eine plattformübergreifende Implementierung von .NET. Hier ist diesen Befehlen:

  * `dotnet restore`: Lädt die NuGet-Pakete, die im angegebenen der *csproj* Datei.
  * `dotnet ef migrations add Initial`Führt das Entity Framework .NET Core CLI Migrationen--Befehl aus, und die anfängliche Migration erstellt. Der Parameter nach "hinzufügen" ist ein Name, der Sie die Migration zuweisen. Hier sind Sie die Migration "Initial" benannt, da es sich um die ursprüngliche Datenbankmigration ist. Dieser Vorgang erstellt die *Datenmigration / /\<Datum / Uhrzeit > _Initial.cs* Datei mit der Migration-Befehlen zum Hinzufügen der *Film* Tabelle in der Datenbank.
  * `dotnet ef database update`Aktualisiert die Datenbank mit der Migration, die wir gerade erstellt haben.

Sie erfahren über die Datenbank und der Verbindung-Zeichenfolge in den nächsten Lernprogrammen. Erfahren Sie datenmodelländerungen in der [Hinzufügen eines Felds](xref:tutorials/first-mvc-app/new-field) Lernprogramm.