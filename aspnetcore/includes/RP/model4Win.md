<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a>Erstellen des Filmmodells

* Öffnen Sie ein Befehlsfenster im Projektverzeichnis (das Verzeichnis mit den Dateien *Program.cs*, *Startup.cs*, und *CSPROJ*).
* Führen Sie den folgenden Befehl aus:

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

Wenn Sie eine Fehlermeldung erhalten, müssen Sie Folgendes tun:
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

Beenden Sie Visual Studio, und führen Sie den Befehl erneut aus.