# <a name="adding-search-to-an-aspnet-core-mvc-app"></a>Hinzufügen der Suche zu einer ASP.NET Core MVC-App

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

In diesem Abschnitt fügen Sie die Suchfunktion der Aktionsmethode `Index` hinzu, mit der Sie Filme nach *Genre* oder *Namen* suchen können.

Aktualisieren Sie die `Index`-Methode mit folgendem Code:
<!--
[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]
-->

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

Die erste Zeile der Aktionsmethode `Index` erstellt eine [LINQ](http://msdn.microsoft.com/library/bb397926.aspx)-Abfrage zum Auswählen der Filme:

```csharp
var movies = from m in _context.Movie
             select m;
```

Die Abfrage wird an dieser Stelle *nur* definiert und **nicht** auf die Datenbank angewendet.

Wenn der `searchString`-Parameter eine Zeichenfolge enthält, wird die Filmabfrage so geändert, dass nach dem Wert der Suchzeichenfolge gefiltert wird:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

Der Code `s => s.Title.Contains()` oben ist ein [Lambdaausdruck](http://msdn.microsoft.com/library/bb397687.aspx). Lambdas werden in methodenbasierten [LINQ](http://msdn.microsoft.com/library/bb397926.aspx)-Abfragen als Argumente für standardmäßige Abfrageoperatormethoden wie die [Where](http://msdn.microsoft.com/library/system.linq.enumerable.where.aspx)-Methode oder `Contains` verwendet (siehe den vorangehenden Code). LINQ-Abfragen werden nicht ausgeführt, wenn sie definiert oder durch Aufrufen einer Methode geändert werden (z.B. `Where`, `Contains` oder `OrderBy`). Stattdessen wird die Ausführung der Abfrage verzögert.  Dies bedeutet, dass die Auswertung eines Ausdrucks so lange hinausgezögert wird, bis dessen realisierter Wert tatsächlich durchlaufen oder die `ToListAsync`-Methode aufgerufen wird. Weitere Informationen zur verzögerten Abfrageausführung finden Sie unter [Abfrageausführung](http://msdn.microsoft.com/library/bb738633.aspx).

Hinweis: Die [Contains](http://msdn.microsoft.com/library/bb155125.aspx)-Methode wird in der Datenbank und nicht im oben gezeigten C#-Code ausgeführt. Die Groß-/Kleinschreibung in der Abfrage hängt von der Datenbank und Sortierung ab. In SQL Server wird [Contains](http://msdn.microsoft.com/library/bb155125.aspx) zu [SQL LIKE](http://msdn.microsoft.com/library/ms179859.aspx) zugeordnet, das Groß-/Kleinschreibung nicht beachtet. In SQLite wird bei der Standardsortierung Groß-/Kleinschreibung beachtet.

Navigieren Sie zu `/Movies/Index`. Fügen Sie eine Abfragezeichenfolge wie `?searchString=Ghost` an die URL an. Die gefilterten Filme werden angezeigt.

![Indexansicht](../../tutorials/first-mvc-app/search/_static/ghost.png)

Wenn Sie die Signatur der `Index`-Methode so ändern, dass sie einen Parameter mit dem Namen `id` hat, entspricht der Parameter `id` dem optionalen Platzhalter `{id}` für die Standardrouten, die in *Startup.cs* festgelegt sind.

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]