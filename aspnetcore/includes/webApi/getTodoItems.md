[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="cc284-101">Der vorangehende Code:</span><span class="sxs-lookup"><span data-stu-id="cc284-101">The preceding code:</span></span>

* <span data-ttu-id="cc284-102">Definiert eine leere Controller-Klasse.</span><span class="sxs-lookup"><span data-stu-id="cc284-102">Defines an empty controller class.</span></span> <span data-ttu-id="cc284-103">In den nächsten Abschnitten werden Methoden zum Implementieren der API hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="cc284-103">In the next sections, methods are added to implement the API.</span></span>
* <span data-ttu-id="cc284-104">Der Konstruktor verwendet die [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) zum Einfügen des Datenbankkontexts (`TodoContext `) in den Controller.</span><span class="sxs-lookup"><span data-stu-id="cc284-104">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext `) into the controller.</span></span> <span data-ttu-id="cc284-105">Der Datenbankkontext wird in den einzelnen [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete)-Methoden im Controller verwendet.</span><span class="sxs-lookup"><span data-stu-id="cc284-105">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="cc284-106">Der Konstruktor fügt ein Element der In-Memory Database hinzu, falls es nicht vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="cc284-106">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="getting-to-do-items"></a><span data-ttu-id="cc284-107">Abrufen von „To-do“-Elementen</span><span class="sxs-lookup"><span data-stu-id="cc284-107">Getting to-do items</span></span>

<span data-ttu-id="cc284-108">Fügen Sie der `TodoController`-Klasse die folgenden Methoden hinzu, „To-do“-Elemente abzurufen:</span><span class="sxs-lookup"><span data-stu-id="cc284-108">To get to-do items, add the following methods to the `TodoController` class.</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="cc284-109">Diese Methoden implementieren die beiden GET-Methoden:</span><span class="sxs-lookup"><span data-stu-id="cc284-109">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="cc284-110">Hier ist eine HTTP-Beispielantwort für die `GetAll`-Methode:</span><span class="sxs-lookup"><span data-stu-id="cc284-110">Here is an example HTTP response for the `GetAll` method:</span></span>

```
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
   ```

<span data-ttu-id="cc284-111">Später in diesem Tutorial zeige ich Ihnen, wie die HTTP-Antwort mit [Postman](https://www.getpostman.com/) oder [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html) angezeigt werden kann.</span><span class="sxs-lookup"><span data-stu-id="cc284-111">Later in the tutorial I'll show how the HTTP response can be viewed with [Postman](https://www.getpostman.com/) or [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="cc284-112">Routing und URL-Pfade</span><span class="sxs-lookup"><span data-stu-id="cc284-112">Routing and URL paths</span></span>

<span data-ttu-id="cc284-113">Das `[HttpGet]`-Attribut gibt eine HTTP GET-Methode an.</span><span class="sxs-lookup"><span data-stu-id="cc284-113">The `[HttpGet]` attribute specifies an HTTP GET method.</span></span> <span data-ttu-id="cc284-114">Der URL-Pfad für jede Methode wird wie folgt erstellt:</span><span class="sxs-lookup"><span data-stu-id="cc284-114">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="cc284-115">Verwenden Sie die Vorlagenzeichenfolge im `Route`-Attribut des Controllers:</span><span class="sxs-lookup"><span data-stu-id="cc284-115">Take the template string in the controller’s `Route` attribute:</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="cc284-116">Ersetzen Sie „[Controller]“ durch den Namen des Controllers, bei dem es sich um den Namen der Controller-Klasse ohne das Suffix „Controller“ handelt.</span><span class="sxs-lookup"><span data-stu-id="cc284-116">Replace "[Controller]" with the name of the controller, which is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="cc284-117">Bei diesem Beispiel ist der Klassenname des Controllers „**Todo**Controller“ und der Stammname ist „todo“.</span><span class="sxs-lookup"><span data-stu-id="cc284-117">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="cc284-118">Beim ASP.NET Core-[Routing](xref:mvc/controllers/routing) wird Groß-/Kleinschreibung nicht beachtet.</span><span class="sxs-lookup"><span data-stu-id="cc284-118">ASP.NET Core [routing](xref:mvc/controllers/routing) is not case sensitive.</span></span>
* <span data-ttu-id="cc284-119">Wenn das `[HttpGet]`-Attribut eine Routenvorlage (z.B. `[HttpGet("/products")]`) hat, fügen Sie diese an den Pfad an.</span><span class="sxs-lookup"><span data-stu-id="cc284-119">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="cc284-120">In diesem Beispiel wird keine Vorlage verwendet.</span><span class="sxs-lookup"><span data-stu-id="cc284-120">This sample doesn't use a template.</span></span> <span data-ttu-id="cc284-121">Weitere Informationen finden Sie unter [Attributrouting mit Http[Verb]-Attributen](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="cc284-121">See [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes) for more information.</span></span>

<span data-ttu-id="cc284-122">Für die `GetById`-Methode gilt Folgendes:</span><span class="sxs-lookup"><span data-stu-id="cc284-122">In the `GetById` method:</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

<span data-ttu-id="cc284-123">`"{id}"` ist eine Platzhaltervariable für die ID des `todo`-Elements.</span><span class="sxs-lookup"><span data-stu-id="cc284-123">`"{id}"` is a placeholder variable for the ID of the `todo` item.</span></span> <span data-ttu-id="cc284-124">Wenn `GetById` aufgerufen wird, wird der Wert von „{id}“ in der URL dem Parameter `id` der Methode zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="cc284-124">When `GetById` is invoked, it assigns the value of "{id}" in the URL to the method's `id` parameter.</span></span>

<span data-ttu-id="cc284-125">`Name = "GetTodo"` erstellt eine benannte Route.</span><span class="sxs-lookup"><span data-stu-id="cc284-125">`Name = "GetTodo"` creates a named route.</span></span> <span data-ttu-id="cc284-126">Benannte Routen:</span><span class="sxs-lookup"><span data-stu-id="cc284-126">Named routes:</span></span>

* <span data-ttu-id="cc284-127">Ermöglichen der App, einen HTTP-Link mit dem Routennamen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="cc284-127">Enable the app to create an HTTP link using the route name.</span></span>
* <span data-ttu-id="cc284-128">Werden später in diesem Tutorial erläutert.</span><span class="sxs-lookup"><span data-stu-id="cc284-128">Are explained later in the tutorial.</span></span>

### <a name="return-values"></a><span data-ttu-id="cc284-129">Rückgabewert</span><span class="sxs-lookup"><span data-stu-id="cc284-129">Return values</span></span>

<span data-ttu-id="cc284-130">Die `GetAll`-Methode gibt `IEnumerable` zurück.</span><span class="sxs-lookup"><span data-stu-id="cc284-130">The `GetAll` method returns an `IEnumerable`.</span></span> <span data-ttu-id="cc284-131">MVC serialisiert automatisch das Objekt in [JSON](http://www.json.org/) und schreibt den JSON-Code in den Text der Antwortnachricht.</span><span class="sxs-lookup"><span data-stu-id="cc284-131">MVC automatically serializes the object to [JSON](http://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="cc284-132">Der Antwortcode für diese Methode ist 200, vorausgesetzt, es gibt keine nicht behandelten Ausnahmen.</span><span class="sxs-lookup"><span data-stu-id="cc284-132">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="cc284-133">(Nicht behandelte Ausnahmen werden in 5xx-Fehler übersetzt.)</span><span class="sxs-lookup"><span data-stu-id="cc284-133">(Unhandled exceptions are translated into 5xx errors.)</span></span>

<span data-ttu-id="cc284-134">Im Gegensatz dazu gibt die `GetById`-Methode den allgemeineren Typ `IActionResult` zurück, der eine Vielzahl von Rückgabetypen darstellt.</span><span class="sxs-lookup"><span data-stu-id="cc284-134">In contrast, the `GetById` method returns the more general `IActionResult` type, which represents a wide range of return types.</span></span> <span data-ttu-id="cc284-135">`GetById` hat zwei unterschiedliche Rückgabetypen:</span><span class="sxs-lookup"><span data-stu-id="cc284-135">`GetById` has two different return types:</span></span>

* <span data-ttu-id="cc284-136">Wenn kein Element mit der angeforderten ID übereinstimmt, gibt die Methode einen 404-Fehler zurück.</span><span class="sxs-lookup"><span data-stu-id="cc284-136">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="cc284-137">Die Rückgabe von `NotFound` gibt eine HTTP 404-Antwort zurück.</span><span class="sxs-lookup"><span data-stu-id="cc284-137">Returning `NotFound` returns an HTTP 404 response.</span></span>

* <span data-ttu-id="cc284-138">Andernfalls gibt die Methode 200 mit einem JSON-Antworttext zurück.</span><span class="sxs-lookup"><span data-stu-id="cc284-138">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="cc284-139">Die Rückgabe von `ObjectResult` gibt eine HTTP 200-Antwort zurück.</span><span class="sxs-lookup"><span data-stu-id="cc284-139">Returning `ObjectResult` returns an HTTP 200 response.</span></span>
