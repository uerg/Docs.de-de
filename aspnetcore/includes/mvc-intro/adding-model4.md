<span data-ttu-id="d9f22-101">Der oben hervorgehobene Code zeigt den dem Container [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) (in der Datei *Startup.cs*) hinzugefügten Filmdatenbankkontext.</span><span class="sxs-lookup"><span data-stu-id="d9f22-101">The highlighted code above shows the movie database context being added to the [Dependency Injection](xref:fundamentals/dependency-injection) container (In the *Startup.cs* file).</span></span> <span data-ttu-id="d9f22-102">`services.AddDbContext<MvcMovieContext>(options =>` gibt die zu verwendende Datenbank und die Verbindungszeichenfolge an.</span><span class="sxs-lookup"><span data-stu-id="d9f22-102">`services.AddDbContext<MvcMovieContext>(options =>` specifies the database to use and the connection string.</span></span> <span data-ttu-id="d9f22-103">`=>` ist ein [Lambdaoperator](/dotnet/articles/csharp/language-reference/operators/lambda-operator).</span><span class="sxs-lookup"><span data-stu-id="d9f22-103">`=>` is a [lambda operator](/dotnet/articles/csharp/language-reference/operators/lambda-operator).</span></span>

<span data-ttu-id="d9f22-104">Öffnen Sie die Datei *Controllers/MoviesController.cs*, und überprüfen Sie den Konstruktor:</span><span class="sxs-lookup"><span data-stu-id="d9f22-104">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_1)] 

<span data-ttu-id="d9f22-105">Der Konstruktor verwendet die [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) zum Einfügen des Datenbankkontexts (`MvcMovieContext `) in den Controller.</span><span class="sxs-lookup"><span data-stu-id="d9f22-105">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext `) into the controller.</span></span> <span data-ttu-id="d9f22-106">Der Datenbankkontext wird in den einzelnen [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete)-Methoden im Controller verwendet.</span><span class="sxs-lookup"><span data-stu-id="d9f22-106">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="d9f22-107">Stark typisierte Modelle und das Schlüsselwort @model</span><span class="sxs-lookup"><span data-stu-id="d9f22-107">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="d9f22-108">Weiter oben in diesem Tutorial haben Sie gesehen, wie ein Controller mithilfe des Wörterbuchs `ViewData` Daten oder Objekte an eine Ansicht übergeben kann.</span><span class="sxs-lookup"><span data-stu-id="d9f22-108">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="d9f22-109">Das Wörterbuch `ViewData` ist ein dynamisches Objekt, das eine praktische Möglichkeit zur späten Bindung bietet, um Informationen an eine Ansicht zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="d9f22-109">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="d9f22-110">MVC bietet außerdem die Möglichkeit, stark typisierte Modellobjekte an eine Ansicht zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="d9f22-110">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="d9f22-111">Dieser stark typisierte Ansatz ermöglicht eine bessere Überprüfung Ihres Codes zur Kompilierzeit.</span><span class="sxs-lookup"><span data-stu-id="d9f22-111">This strongly typed approach enables better compile-time checking of your code.</span></span> <span data-ttu-id="d9f22-112">Der Gerüstbaumechnismus hat diesen Ansatz (d.h. das Übergeben eines stark typisierten Modells) mit der `MoviesController`-Klasse und Ansichten genutzt, als die Methoden und Ansichten erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="d9f22-112">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views when it created the methods and views.</span></span>

<span data-ttu-id="d9f22-113">Untersuchen Sie die generierte `Details`-Methode in der Datei *Controllers/MoviesController.cs*:</span><span class="sxs-lookup"><span data-stu-id="d9f22-113">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_details)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]

::: moniker-end


<span data-ttu-id="d9f22-114">Der `id`-Parameter wird im Allgemeinen als Routendaten übergeben.</span><span class="sxs-lookup"><span data-stu-id="d9f22-114">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="d9f22-115">`http://localhost:5000/movies/details/1` legt beispielsweise Folgendes fest:</span><span class="sxs-lookup"><span data-stu-id="d9f22-115">For example `http://localhost:5000/movies/details/1` sets:</span></span>

* <span data-ttu-id="d9f22-116">Den Controller auf den Controller `movies` (das erste URL-Segment).</span><span class="sxs-lookup"><span data-stu-id="d9f22-116">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="d9f22-117">Die Aktion auf `details` (das zweite URL-Segment).</span><span class="sxs-lookup"><span data-stu-id="d9f22-117">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="d9f22-118">Die ID auf 1 (das letzte URL-Segment).</span><span class="sxs-lookup"><span data-stu-id="d9f22-118">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="d9f22-119">Sie können die `id` auch mithilfe einer Abfragezeichenfolge übergeben:</span><span class="sxs-lookup"><span data-stu-id="d9f22-119">You can also pass in the `id` with a query string as follows:</span></span>

`http://localhost:1234/movies/details?id=1`

<span data-ttu-id="d9f22-120">Der `id`-Parameter wird als [Nullable-Typ](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) für den Fall definiert, dass kein ID-Wert angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="d9f22-120">The `id` parameter is defined as a [nullable type](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value isn't provided.</span></span>



::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="d9f22-121">Ein [Lambdaausdruck](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) wird an `FirstOrDefaultAsync` übergeben, um Filmentitäten auszuwählen, die mit den Routendaten oder dem Wert der Abfragezeichenfolge übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="d9f22-121">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `FirstOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.ID == id);
```

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="d9f22-122">Ein [Lambdaausdruck](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) wird an `SingleOrDefaultAsync` übergeben, um Filmentitäten auszuwählen, die mit den Routendaten oder dem Wert der Abfragezeichenfolge übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="d9f22-122">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `SingleOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .SingleOrDefaultAsync(m => m.ID == id);
```

::: moniker-end



<span data-ttu-id="d9f22-123">Wenn ein Film gefunden wird, wird eine Instanz des `Movie`-Modells an die Ansicht `Details` übergeben:</span><span class="sxs-lookup"><span data-stu-id="d9f22-123">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
   ```

<span data-ttu-id="d9f22-124">Untersuchen Sie den Inhalt der Datei *Views/Movies/Details.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d9f22-124">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/DetailsOriginal.cshtml)]

<span data-ttu-id="d9f22-125">Durch Einschließen einer `@model`-Anweisung am Anfang der Ansichtsdatei können Sie den Typ des Objekts angeben, den die Ansicht erwartet.</span><span class="sxs-lookup"><span data-stu-id="d9f22-125">By including a `@model` statement at the top of the view file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="d9f22-126">Beim Erstellen des „Movies“-Controllers hat Visual Studio automatisch die folgende `@model`-Anweisung am Anfang der Datei *Details.cshtml* hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="d9f22-126">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Details.cshtml* file:</span></span>

```HTML
@model MvcMovie.Models.Movie
   ```

<span data-ttu-id="d9f22-127">Diese `@model`-Direktive ermöglicht Ihnen den Zugriff auf den Film, den der Controller an die Ansicht übergeben hat, indem ein stark typisiertes `Model`-Objekt verwendet wir.</span><span class="sxs-lookup"><span data-stu-id="d9f22-127">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="d9f22-128">In der Ansicht *Details.cshtml* übergibt der Code jedes Filmfeld an die HTML-Hilfsprogramme `DisplayNameFor` und `DisplayFor` mit dem stark typisierten `Model`-Objekt.</span><span class="sxs-lookup"><span data-stu-id="d9f22-128">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="d9f22-129">Die Methoden `Create` und `Edit` und Ansichten übergeben außerdem ein `Movie`-Modelobjekt.</span><span class="sxs-lookup"><span data-stu-id="d9f22-129">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="d9f22-130">Überprüfen Sie die Datei *Index.cshtml* und die `Index`-Methode im „Movies“-Controller.</span><span class="sxs-lookup"><span data-stu-id="d9f22-130">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="d9f22-131">Beachten Sie, wie der Code beim Aufrufen der `View`-Methode ein `List`-Objekt erstellt.</span><span class="sxs-lookup"><span data-stu-id="d9f22-131">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="d9f22-132">Der Code übergibt diese Liste `Movies` aus der Aktionsmethode `Index` an die Ansicht:</span><span class="sxs-lookup"><span data-stu-id="d9f22-132">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_index)]

<span data-ttu-id="d9f22-133">Beim Erstellen des „Movies“-Controllers hat der Gerüstbau automatisch die folgende `@model`-Anweisung am Anfang der Datei *Index.cshtml* hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="d9f22-133">When you created the movies controller, scaffolding automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?range=1)]

<span data-ttu-id="d9f22-134">Diese `@model`-Direktive ermöglicht Ihnen den Zugriff auf die Liste der Filme, die der Controller an die Ansicht übergeben hat, indem ein stark typisiertes `Model`-Objekt verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="d9f22-134">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="d9f22-135">In der Ansicht *Index.cshtml* durchläuft der Code z.B. die Filme mithilfe einer `foreach`-Anweisung für das stark typisierte `Model`-Objekt:</span><span class="sxs-lookup"><span data-stu-id="d9f22-135">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

<span data-ttu-id="d9f22-136">Da das `Model`-Objekt stark typisiert ist (als ein `IEnumerable<Movie>`-Objekt), wird jedes Element in der Schleife als `Movie` typisiert.</span><span class="sxs-lookup"><span data-stu-id="d9f22-136">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="d9f22-137">Neben anderen Vorteilen bedeutet dies, dass Sie eine Überprüfung des Codes zur Kompilierzeit erhalten:</span><span class="sxs-lookup"><span data-stu-id="d9f22-137">Among other benefits, this means that you get compile-time checking of the code:</span></span>
