Die folgende Tabelle zeigt die Details der Parameter des Codegenerators von ASP.NET:

| Parameter               | description|
| ----------------- | ------------ |
| -m  | Der Name des Modells. |
| -dc  | Der Datenkontext. |
| -udl | Verwendung des Standardlayouts. |
| -outDir | Der relative Ausgabeordnerpfad zur Erstellung der Ansichten. |
| --referenceScriptLibraries | Fügt `_ValidationScriptsPartial` zu den Seiten „Create“ und „Edit“ hinzu. |

Verwenden Sie den `h`-Switch um Hilfe für den `aspnet-codegenerator razorpage`-Befehl zu erhalten:

```console
dotnet aspnet-codegenerator razorpage -h
```
<a name="test"></a>
### <a name="test-the-app"></a>Testen der App

* Führen Sie die App aus, und fügen Sie `/Movies` an die URL im Browser an (`http://localhost:port/movies`).
* Testen Sie den Link **Create** (Erstellen).

  ![Seite „Create“](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* Testen Sie die Links **Edit** (Bearbeiten), **Details** und **Delete** (Löschen).

Wenn der folgende (oder ein ähnlicher) Fehler angezeigt wird, stellen Sie sicher, dass Migrationen ausgeführt wurden und die Datenbank aktualisiert wurde:

```
An unhandled exception occurred while processing the request.
'no such table: Movie'.
```
