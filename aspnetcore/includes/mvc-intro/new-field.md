<!-- This include not used by windows version -->
# <a name="add-a-new-field-to-an-aspnet-core-mvc-app"></a>Hinzufügen eines neuen Felds zu einer ASP.NET Core MVC-App

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

In diesem Tutorial fügen Sie der Tabelle `Movies` ein neues Feld hinzu. Wenn wir das Schema ändern (durch Hinzufügen eines neuen Felds), löschen wir die Datenbank und erstellen eine neue. Dieser Workflow funktioniert in der Frühphase der Entwicklung gut, wenn es noch keine aufzubewahrenden Produktionsdaten gibt.

Nachdem Ihre App bereitgestellt wurde und Sie über aufzubewahrende Daten verfügen, können Sie die Datenbank nicht löschen, wenn Sie das Schema ändern müssen. Mithilfe von Entity Framework [Code First-Migrationen](/ef/core/get-started/aspnetcore/new-db) können Sie Ihr Schema aktualisieren und die Datenbank ohne Datenverlust migrieren. Migrationen sind bei Verwendung von SQL Server ein beliebtes Feature, doch von SQLlite werden nicht viele Vorgänge für die Schemamigration unterstützt. Daher sind nur sehr einfache Migrationsvorgänge möglich. Weitere Informationen finden Sie unter [SQLite-Einschränkungen](/ef/core/providers/sqlite/limitations).

## <a name="adding-a-rating-property-to-the-movie-model"></a>Hinzufügen einer Rating-Eigenschaft zum Movie-Modell

Öffnen Sie die Datei *Models/Movie.cs*, und fügen Sie eine `Rating`-Eigenschaft hinzu:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Models/MovieDateRating.cs?highlight=12&name=snippet)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

::: moniker-end

Da Sie der `Movie`-Klasse ein neues Feld hinzugefügt haben, müssen Sie auch die Positivliste für die Bindung aktualisieren, damit diese neue Eigenschaft eingeschlossen wird. Aktualisieren Sie in *MoviesController.cs* das `[Bind]`-Attribut für die Aktionsmethoden `Create` und `Edit` so, dass die `Rating`-Eigenschaft eingeschlossen wird:

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

Sie müssen auch die Ansichtsvorlagen aktualisieren, um die neue `Rating`-Eigenschaft in der Browseransicht anzuzeigen, zu erstellen und zu bearbeiten.

Bearbeiten Sie die Datei */Views/Movies/Index.cshtml*, und fügen Sie das Feld `Rating` hinzu:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

Aktualisieren Sie die Datei */Views/Movies/Create.cshtml* mit dem Feld `Rating`.

Die App funktioniert erst, nachdem wir die Datenbank mit dem neuen Feld aktualisiert haben. Wenn Sie sie jetzt ausführen, erhalten Sie die folgende `SqliteException`:

```
SqliteException: SQLite Error 1: 'no such column: m.Rating'.
```

Dieser Fehler wird gemeldet, da die aktualisierte Modellklasse „Movie“ sich vom Schema der Tabelle „Movie“ der vorhandenen Datenbank unterscheidet. (Die Datenbanktabelle enthält nicht die Spalte `Rating`.)

Es gibt mehrere Vorgehensweisen zum Beheben des Fehlers:

1. Lassen Sie die Datenbank von Entity Framework automatisch löschen und basierend auf dem neuen Modellklassenschema neu erstellen. Bei dieser Vorgehensweise gehen in der Datenbank vorhandene Daten verloren, die deshalb bei einer Produktionsdatenbank nicht möglich ist! Das Verwenden eines Initialisierers zum automatischen Ausführen eines Seedings für eine Datenbank mit Testdaten ist häufig eine produktive Möglichkeit zum Entwickeln einer App.

2. Ändern Sie das Schema der vorhandenen Datenbank manuell so, dass es mit den Modellklassen übereinstimmt. Der Vorteil dieses Ansatzes ist, dass Sie Ihre Daten behalten. Sie können diese Änderung entweder manuell oder durch Erstellen eines Änderungsskripts für die Datenbank vornehmen.

3. Verwenden Sie Code First-Migrationen, um das Datenbankschema zu aktualisieren.

In diesem Tutorial löschen wir die Datenbank und erstellen Sie neu, sobald sich das Schema ändert. Führen Sie über ein Terminal den folgenden Befehl aus, um die Datenbank zu löschen:

`dotnet ef database drop`

Aktualisieren Sie die `SeedData`-Klasse so, dass sie einen Wert für die neue Spalte bereitstellt. Eine Beispieländerung wird nachstehend gezeigt, aber Sie sollten diese Änderung für jedes `new Movie`-Element vornehmen.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

Fügen Sie das Feld `Rating` den Ansichten `Edit`, `Details` und `Delete` hinzu.

Führen Sie die App aus, und überprüfen Sie, ob Sie Filme mit dem Feld `Rating` erstellen/bearbeiten/anzeigen können. Vorlagen
