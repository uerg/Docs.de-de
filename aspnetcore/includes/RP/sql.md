# <a name="work-with-sqlite-in-an-aspnet-core-razor-pages-app"></a>Arbeiten mit SQLite in einer ASP.NET Core-App mit Razor Pages

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

Das `MovieContext`-Objekt übernimmt die Aufgabe der Herstellung der Verbindung mit der Datenbank und Zuordnung von `Movie`-Objekten zu Datensätzen in der Datenbank. Der Datenbankkontext wird mit dem [Dependency Injection](xref:fundamentals/dependency-injection)-Container in der Methode `ConfigureServices` in der Datei *Startup.cs* registriert:

[!code-csharp[](code/Startup.cs?name=snippet2&highlight=6-8)]

Weitere Informationen zur Verwendung von `DbContext` mit Dependency Injection (DI) finden Sie unter [Using DbContext with DI (Verwenden von DbContext mit DI)](/ef/core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection).

## <a name="sqlite"></a>SQLite

Auf der [SQLite](https://www.sqlite.org/)-Website ist zu lesen:

> SQLite ist eine eigenständige, sehr zuverlässige, eingebettete, genehmigungsfreie SQL-Datenbank-Engine mit vollem Funktionsumfang. SQLite ist die weltweit am häufigsten verwendete Datenbank-Engine.

Es gibt viele Tools von Drittanbietern, die Sie herunterladen können, um eine SQLite-Datenbank zu verwalten und anzuzeigen. Die folgende Abbildung stammt aus [DB Browser for SQLite](http://sqlitebrowser.org/). Wenn Sie ein bestimmtes SQLite-Tool bevorzugen, geben Sie bitte in einem Kommentar dessen Vorteile an.

![DB Browser for SQLite mit der Filmdatenbank](../../tutorials/first-mvc-app-xplat/working-with-sql/_static/dbb.png)

## <a name="seed-the-database"></a>Ausführen eines Seedings für die Datenbank

Erstellen Sie im Ordner *Models* die neue Klasse `SeedData`. Ersetzen Sie den generierten Code durch den folgenden:

[!code-csharp[](code/Models/SeedData.cs)]

Wenn in der Datenbank Filme vorhanden sind, wird der Initialisierer des Seedings zurückgegeben.

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a>Hinzufügen des Initialisierers des Seedings

Fügen Sie den Initialisierer des Seedings in der Datei *Program.cs* zur `Main`-Methode hinzu:

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Program.cs)]

### <a name="test-the-app"></a>Testen der App

Löschen Sie alle Datensätze in der Datenbank (damit die Seed-Methode ausgeführt wird). Beenden und starten Sie die App, um das Seeding der Datenbank auszuführen.

Die App zeigt die per Seeding hinzugefügten Daten.
