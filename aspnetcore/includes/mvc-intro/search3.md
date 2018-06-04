<!--
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](~/tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

<span data-ttu-id="59e19-101">Wenn Sie nun eine Suche übermitteln, enthält die URL die Suchabfragezeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="59e19-101">Now when you submit a search, the URL contains the search query string.</span></span> <span data-ttu-id="59e19-102">Das Suchen wird auch an Aktionsmethode `HttpGet Index` übertragen, auch wenn Sie eine `HttpPost Index`-Methode haben.</span><span class="sxs-lookup"><span data-stu-id="59e19-102">Searching will also go to the `HttpGet Index` action method, even if you have a `HttpPost Index` method.</span></span>

![Browserfenster mit „searchString=ghost“ in der URL und den zurückgegebenen Filmen Ghostbusters und Ghostbusters 2, die das Wort „Ghost“ enthalten](~/tutorials/first-mvc-app/search/_static/search_get.png)

<span data-ttu-id="59e19-104">Das folgende Markup zeigt die Änderung am Tag `form`:</span><span class="sxs-lookup"><span data-stu-id="59e19-104">The following markup shows the change to the `form` tag:</span></span>

```html
<form asp-controller="Movies" asp-action="Index" method="get">
   ```

## <a name="adding-search-by-genre"></a><span data-ttu-id="59e19-105">Hinzufügen einer Suche nach Genre</span><span class="sxs-lookup"><span data-stu-id="59e19-105">Adding Search by genre</span></span>

<span data-ttu-id="59e19-106">Fügen Sie dem Ordner *Models* die folgende `MovieGenreViewModel`-Klasse hinzu:</span><span class="sxs-lookup"><span data-stu-id="59e19-106">Add the following `MovieGenreViewModel` class to the *Models* folder:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieGenreViewModel.cs)]

<span data-ttu-id="59e19-107">Das Ansichtsmodell „movie-genre“ enthält Folgendes:</span><span class="sxs-lookup"><span data-stu-id="59e19-107">The movie-genre view model will contain:</span></span>

   * <span data-ttu-id="59e19-108">Eine Liste von Filmen.</span><span class="sxs-lookup"><span data-stu-id="59e19-108">A list of movies.</span></span>
   * <span data-ttu-id="59e19-109">Ein `SelectList`-Element mit der Liste der Genres.</span><span class="sxs-lookup"><span data-stu-id="59e19-109">A `SelectList` containing the list of genres.</span></span> <span data-ttu-id="59e19-110">Es ermöglicht dem Benutzer, ein Genre in der Liste auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="59e19-110">This will allow the user to select a genre from the list.</span></span>
   * <span data-ttu-id="59e19-111">Ein `movieGenre`-Element, das das ausgewählte Genre enthält.</span><span class="sxs-lookup"><span data-stu-id="59e19-111">`movieGenre`, which contains the selected genre.</span></span>

<span data-ttu-id="59e19-112">Ersetzen Sie die `Index`-Methode in `MoviesController.cs` durch folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="59e19-112">Replace the `Index` method in `MoviesController.cs` with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchGenre)]

<span data-ttu-id="59e19-113">Der folgende Code ist eine `LINQ`-Abfrage, die alle Genres aus der Datenbank abruft.</span><span class="sxs-lookup"><span data-stu-id="59e19-113">The following code is a `LINQ` query that retrieves all the genres from the database.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_LINQ)]

<span data-ttu-id="59e19-114">Das `SelectList`-Element von Genres wird durch Projizieren der unterschiedlichen Genres erstellt (wir möchten nicht, dass unsere Auswahlliste doppelte Genres enthält).</span><span class="sxs-lookup"><span data-stu-id="59e19-114">The `SelectList` of genres is created by projecting the distinct genres (we don't want our select list to have duplicate genres).</span></span>

```csharp
movieGenreVM.genres = new SelectList(await genreQuery.Distinct().ToListAsync())
   ```

## <a name="adding-search-by-genre-to-the-index-view"></a><span data-ttu-id="59e19-115">Hinzufügen einer Suche nach Genre zur Indexansicht</span><span class="sxs-lookup"><span data-stu-id="59e19-115">Adding search by genre to the Index view</span></span>

<span data-ttu-id="59e19-116">Aktualisieren Sie `Index.cshtml` wie folgt:</span><span class="sxs-lookup"><span data-stu-id="59e19-116">Update `Index.cshtml` as follows:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexFormGenreNoRating.cshtml?highlight=1,15,16,17,28,31,34,37,43)]

<span data-ttu-id="59e19-117">Überprüfen Sie den Lambdaausdruck, der im folgenden HTML-Hilfsprogramm verwendet wird:</span><span class="sxs-lookup"><span data-stu-id="59e19-117">Examine the lambda expression used in the following HTML Helper:</span></span>

`@Html.DisplayNameFor(model => model.movies[0].Title)`
 
<span data-ttu-id="59e19-118">Das HTML-Hilfsprogramm `DisplayNameFor` im vorangehenden Code überprüft die Eigenschaft `Title`, auf die im Lambdaausdruck verwiesen wird, um den Anzeigenamen zu bestimmen.</span><span class="sxs-lookup"><span data-stu-id="59e19-118">In the preceding code, the `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="59e19-119">Da der Lambda-Ausdruck überprüft statt ausgewertet wird, erhalten Sie keine Zugriffsverletzung, wenn `model`, `model.movies`, `model.movies[0]` oder `null` leer sind.</span><span class="sxs-lookup"><span data-stu-id="59e19-119">Since the lambda expression is inspected rather than evaluated, you don't receive an access violation when `model`, `model.movies`, or `model.movies[0]` are `null` or empty.</span></span> <span data-ttu-id="59e19-120">Wenn der Lambdaausdruck ausgewertet wird, (z.B. mit `@Html.DisplayFor(modelItem => item.Title)`), werden die Eigenschaftswerte ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="59e19-120">When the lambda expression is evaluated (for example, `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<span data-ttu-id="59e19-121">Testen Sie die App mit einer Suche nach Genre, Filmtitel und beidem.</span><span class="sxs-lookup"><span data-stu-id="59e19-121">Test the app by searching by genre, by movie title, and by both.</span></span>
