<!--
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](~/tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

Wenn Sie nun eine Suche übermitteln, enthält die URL die Suchabfragezeichenfolge. Das Suchen wird auch an Aktionsmethode `HttpGet Index` übertragen, auch wenn Sie eine `HttpPost Index`-Methode haben.

![Browserfenster mit „searchString=ghost“ in der URL und den zurückgegebenen Filmen Ghostbusters und Ghostbusters 2, die das Wort „Ghost“ enthalten](~/tutorials/first-mvc-app/search/_static/search_get.png)

Das folgende Markup zeigt die Änderung am Tag `form`:

```html
<form asp-controller="Movies" asp-action="Index" method="get">
   ```

## <a name="adding-search-by-genre"></a>Hinzufügen einer Suche nach Genre

Fügen Sie dem Ordner *Models* die folgende `MovieGenreViewModel`-Klasse hinzu:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieGenreViewModel.cs)]

Das Ansichtsmodell „movie-genre“ enthält Folgendes:

   * Eine Liste von Filmen.
   * Ein `SelectList`-Element mit der Liste der Genres. Es ermöglicht dem Benutzer, ein Genre in der Liste auszuwählen.
   * Ein `movieGenre`-Element, das das ausgewählte Genre enthält.

Ersetzen Sie die `Index`-Methode in `MoviesController.cs` durch folgenden Code:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchGenre)]

Der folgende Code ist eine `LINQ`-Abfrage, die alle Genres aus der Datenbank abruft.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_LINQ)]

Das `SelectList`-Element von Genres wird durch Projizieren der unterschiedlichen Genres erstellt (wir möchten nicht, dass unsere Auswahlliste doppelte Genres enthält).

```csharp
movieGenreVM.genres = new SelectList(await genreQuery.Distinct().ToListAsync())
   ```

## <a name="adding-search-by-genre-to-the-index-view"></a>Hinzufügen einer Suche nach Genre zur Indexansicht

Aktualisieren Sie `Index.cshtml` wie folgt:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexFormGenreNoRating.cshtml?highlight=1,15,16,17,28,31,34,37,43)]

Überprüfen Sie den Lambdaausdruck, der im folgenden HTML-Hilfsprogramm verwendet wird:

`@Html.DisplayNameFor(model => model.movies[0].Title)`
 
Das HTML-Hilfsprogramm `DisplayNameFor` im vorangehenden Code überprüft die Eigenschaft `Title`, auf die im Lambdaausdruck verwiesen wird, um den Anzeigenamen zu bestimmen. Da der Lambda-Ausdruck überprüft statt ausgewertet wird, erhalten Sie keine Zugriffsverletzung, wenn `model`, `model.movies`, `model.movies[0]` oder `null` leer sind. Wenn der Lambdaausdruck ausgewertet wird, (z.B. mit `@Html.DisplayFor(modelItem => item.Title)`), werden die Eigenschaftswerte ausgewertet.

Testen Sie die App mit einer Suche nach Genre, Filmtitel und beidem.
