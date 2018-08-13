::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="a80f5-101">Der vorherige Code beschreibt eine API-Controllerklasse ohne Methoden.</span><span class="sxs-lookup"><span data-stu-id="a80f5-101">The preceding code defines an API controller class without methods.</span></span> <span data-ttu-id="a80f5-102">In den nächsten Abschnitten werden Methoden zum Implementieren der API hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="a80f5-102">In the next sections, methods are added to implement the API.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="a80f5-103">Der vorherige Code beschreibt eine API-Controllerklasse ohne Methoden.</span><span class="sxs-lookup"><span data-stu-id="a80f5-103">The preceding code defines an API controller class without methods.</span></span> <span data-ttu-id="a80f5-104">In den nächsten Abschnitten werden Methoden zum Implementieren der API hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="a80f5-104">In the next sections, methods are added to implement the API.</span></span> <span data-ttu-id="a80f5-105">Die Klasse wird mit einem `[ApiController]`-Attribut versehen, um einige praktische Features zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="a80f5-105">The class is annotated with an `[ApiController]` attribute to enable some convenient features.</span></span> <span data-ttu-id="a80f5-106">Informationen zu den Features, die durch das Attribut aktiviert werden, finden Sie unter [Kommentieren einer Klasse mithilfe von ApiControllerAttribute](xref:web-api/index#annotate-class-with-apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="a80f5-106">For information on features enabled by the attribute, see [Annotate class with ApiControllerAttribute](xref:web-api/index#annotate-class-with-apicontrollerattribute).</span></span>
::: moniker-end

<span data-ttu-id="a80f5-107">Der Konstruktor des Controllers verwendet [Dependency Injection](xref:fundamentals/dependency-injection) zum Einfügen des Datenbankkontexts (`TodoContext`) in den Controller.</span><span class="sxs-lookup"><span data-stu-id="a80f5-107">The controller's constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="a80f5-108">Der Datenbankkontext wird in den einzelnen [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete)-Methoden im Controller verwendet.</span><span class="sxs-lookup"><span data-stu-id="a80f5-108">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span> <span data-ttu-id="a80f5-109">Der Konstruktor fügt ein Element der In-Memory Database hinzu, falls es nicht vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="a80f5-109">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="get-to-do-items"></a><span data-ttu-id="a80f5-110">Abrufen von To-do-Elementen</span><span class="sxs-lookup"><span data-stu-id="a80f5-110">Get to-do items</span></span>

<span data-ttu-id="a80f5-111">Fügen Sie der `TodoController`-Klasse die folgenden Methoden hinzu, um To-do-Elemente abzurufen:</span><span class="sxs-lookup"><span data-stu-id="a80f5-111">To get to-do items, add the following methods to the `TodoController` class:</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]
::: moniker-end

<span data-ttu-id="a80f5-112">Diese Methoden implementieren die beiden GET-Methoden:</span><span class="sxs-lookup"><span data-stu-id="a80f5-112">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="a80f5-113">Hier sehen Sie eine HTTP-Beispielantwort für die Methode `GetAll`:</span><span class="sxs-lookup"><span data-stu-id="a80f5-113">Here's a sample HTTP response for the `GetAll` method:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

<span data-ttu-id="a80f5-114">Später in diesem Tutorial sehen Sie, wie die HTTP-Antwort mit [Postman](https://www.getpostman.com/) oder [cURL](https://curl.haxx.se/docs/manpage.html) angezeigt werden kann.</span><span class="sxs-lookup"><span data-stu-id="a80f5-114">Later in the tutorial, I'll show how the HTTP response can be viewed with [Postman](https://www.getpostman.com/) or [curl](https://curl.haxx.se/docs/manpage.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="a80f5-115">Routing und URL-Pfade</span><span class="sxs-lookup"><span data-stu-id="a80f5-115">Routing and URL paths</span></span>

<span data-ttu-id="a80f5-116">Das Attribut `[HttpGet]` gibt eine Methode an, die auf eine HTTP GET-Anforderung antwortet.</span><span class="sxs-lookup"><span data-stu-id="a80f5-116">The `[HttpGet]` attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="a80f5-117">Der URL-Pfad für jede Methode wird wie folgt erstellt:</span><span class="sxs-lookup"><span data-stu-id="a80f5-117">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="a80f5-118">Verwenden Sie die Vorlagenzeichenfolge im `Route`-Attribut des Controllers:</span><span class="sxs-lookup"><span data-stu-id="a80f5-118">Take the template string in the controller's `Route` attribute:</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]
::: moniker-end

* <span data-ttu-id="a80f5-119">Ersetzen Sie `[controller]` durch den Namen des Controllers, bei dem es sich um den Namen der Controller-Klasse ohne das Suffix „Controller“ handelt.</span><span class="sxs-lookup"><span data-stu-id="a80f5-119">Replace `[controller]` with the name of the controller, which is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="a80f5-120">Bei diesem Beispiel ist der Klassenname des Controllers „**Todo**Controller“ und der Stammname ist „todo“.</span><span class="sxs-lookup"><span data-stu-id="a80f5-120">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="a80f5-121">Beim ASP.NET Core-[Routing](xref:mvc/controllers/routing) wird die Groß-/Kleinschreibung nicht beachtet.</span><span class="sxs-lookup"><span data-stu-id="a80f5-121">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="a80f5-122">Wenn das `[HttpGet]`-Attribut eine Routenvorlage (z.B. `[HttpGet("/products")]`) hat, fügen Sie diese an den Pfad an.</span><span class="sxs-lookup"><span data-stu-id="a80f5-122">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="a80f5-123">In diesem Beispiel wird keine Vorlage verwendet.</span><span class="sxs-lookup"><span data-stu-id="a80f5-123">This sample doesn't use a template.</span></span> <span data-ttu-id="a80f5-124">Weitere Informationen finden Sie unter [Attributrouting mit Http[Verb]-Attributen](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="a80f5-124">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="a80f5-125">In der folgenden `GetById`-Methode ist `"{id}"` eine Platzhaltervariable für den eindeutigen Bezeichners des To-do-Elements.</span><span class="sxs-lookup"><span data-stu-id="a80f5-125">In the following `GetById` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="a80f5-126">Wenn `GetById` aufgerufen wird, wird der Wert von `"{id}"` in der URL dem Parameter `id` der Methode zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="a80f5-126">When `GetById` is invoked, it assigns the value of `"{id}"` in the URL to the method's `id` parameter.</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end

<span data-ttu-id="a80f5-127">`Name = "GetTodo"` erstellt eine benannte Route.</span><span class="sxs-lookup"><span data-stu-id="a80f5-127">`Name = "GetTodo"` creates a named route.</span></span> <span data-ttu-id="a80f5-128">Benannte Routen:</span><span class="sxs-lookup"><span data-stu-id="a80f5-128">Named routes:</span></span>

* <span data-ttu-id="a80f5-129">Ermöglichen der App, einen HTTP-Link mit dem Routennamen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="a80f5-129">Enable the app to create an HTTP link using the route name.</span></span>
* <span data-ttu-id="a80f5-130">Werden später in diesem Tutorial erläutert.</span><span class="sxs-lookup"><span data-stu-id="a80f5-130">Are explained later in the tutorial.</span></span>

### <a name="return-values"></a><span data-ttu-id="a80f5-131">Rückgabewert</span><span class="sxs-lookup"><span data-stu-id="a80f5-131">Return values</span></span>

<span data-ttu-id="a80f5-132">Die Methode `GetAll` gibt eine Sammlung von `TodoItem`-Objekten zurück.</span><span class="sxs-lookup"><span data-stu-id="a80f5-132">The `GetAll` method returns a collection of `TodoItem` objects.</span></span> <span data-ttu-id="a80f5-133">MVC serialisiert automatisch das Objekt in [JSON](https://www.json.org/) und schreibt den JSON-Code in den Text der Antwortnachricht.</span><span class="sxs-lookup"><span data-stu-id="a80f5-133">MVC automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="a80f5-134">Der Antwortcode für diese Methode ist 200, vorausgesetzt, es gibt keine nicht behandelten Ausnahmen.</span><span class="sxs-lookup"><span data-stu-id="a80f5-134">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="a80f5-135">Nicht behandelte Ausnahmen werden in 5xx-Fehler übersetzt.</span><span class="sxs-lookup"><span data-stu-id="a80f5-135">Unhandled exceptions are translated into 5xx errors.</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="a80f5-136">Im Gegensatz dazu gibt die `GetById`-Methode den allgemeineren Typ [IActionResult type](xref:web-api/action-return-types#iactionresult-type) zurück, der eine Vielzahl von Rückgabetypen darstellt.</span><span class="sxs-lookup"><span data-stu-id="a80f5-136">In contrast, the `GetById` method returns the more general [IActionResult type](xref:web-api/action-return-types#iactionresult-type), which represents a wide range of return types.</span></span> <span data-ttu-id="a80f5-137">`GetById` hat zwei unterschiedliche Rückgabetypen:</span><span class="sxs-lookup"><span data-stu-id="a80f5-137">`GetById` has two different return types:</span></span>

* <span data-ttu-id="a80f5-138">Wenn kein Element mit der angeforderten ID übereinstimmt, gibt die Methode einen 404-Fehler zurück.</span><span class="sxs-lookup"><span data-stu-id="a80f5-138">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="a80f5-139">Die Rückgabe von [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) gibt eine HTTP 404-Antwort zurück.</span><span class="sxs-lookup"><span data-stu-id="a80f5-139">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="a80f5-140">Andernfalls gibt die Methode 200 mit einem JSON-Antworttext zurück.</span><span class="sxs-lookup"><span data-stu-id="a80f5-140">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="a80f5-141">Die Rückgabe von [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) löst eine HTTP 200-Antwort aus.</span><span class="sxs-lookup"><span data-stu-id="a80f5-141">Returning [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) results in an HTTP 200 response.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="a80f5-142">Im Gegensatz dazu gibt die `GetById`-Methode den Typ [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type) zurück, der eine Vielzahl von Rückgabetypen darstellt.</span><span class="sxs-lookup"><span data-stu-id="a80f5-142">In contrast, the `GetById` method returns the [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type), which represents a wide range of return types.</span></span> <span data-ttu-id="a80f5-143">`GetById` hat zwei unterschiedliche Rückgabetypen:</span><span class="sxs-lookup"><span data-stu-id="a80f5-143">`GetById` has two different return types:</span></span>

* <span data-ttu-id="a80f5-144">Wenn kein Element mit der angeforderten ID übereinstimmt, gibt die Methode einen 404-Fehler zurück.</span><span class="sxs-lookup"><span data-stu-id="a80f5-144">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="a80f5-145">Die Rückgabe von [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) gibt eine HTTP 404-Antwort zurück.</span><span class="sxs-lookup"><span data-stu-id="a80f5-145">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="a80f5-146">Andernfalls gibt die Methode 200 mit einem JSON-Antworttext zurück.</span><span class="sxs-lookup"><span data-stu-id="a80f5-146">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="a80f5-147">Die Rückgabe von `item` löst eine HTTP 200-Antwort aus.</span><span class="sxs-lookup"><span data-stu-id="a80f5-147">Returning `item` results in an HTTP 200 response.</span></span>
::: moniker-end
