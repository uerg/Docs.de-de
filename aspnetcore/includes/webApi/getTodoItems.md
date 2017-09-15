<span data-ttu-id="4f53b-101">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]</span><span class="sxs-lookup"><span data-stu-id="4f53b-101">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]</span></span>

<span data-ttu-id="4f53b-102">Der vorangehende Code:</span><span class="sxs-lookup"><span data-stu-id="4f53b-102">The preceding code:</span></span>

* <span data-ttu-id="4f53b-103">Definiert eine leere Controller-Klasse.</span><span class="sxs-lookup"><span data-stu-id="4f53b-103">Defines an empty controller class.</span></span> <span data-ttu-id="4f53b-104">In den nächsten Abschnitten fügen wir Methoden zum Implementieren der API hinzu.</span><span class="sxs-lookup"><span data-stu-id="4f53b-104">In the next sections, we'll add methods to implement the API.</span></span>
* <span data-ttu-id="4f53b-105">Der Konstruktor verwendet die [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) zum Einfügen des Datenbankkontexts (`TodoContext `) in den Controller.</span><span class="sxs-lookup"><span data-stu-id="4f53b-105">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext `) into the controller.</span></span> <span data-ttu-id="4f53b-106">Der Datenbankkontext wird in den einzelnen [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete)-Methoden im Controller verwendet.</span><span class="sxs-lookup"><span data-stu-id="4f53b-106">The database context is used in each of the [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="4f53b-107">Der Konstruktor fügt ein Element der In-Memory Database hinzu, falls es nicht vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="4f53b-107">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="getting-to-do-items"></a><span data-ttu-id="4f53b-108">Abrufen von „To-do“-Elementen</span><span class="sxs-lookup"><span data-stu-id="4f53b-108">Getting to-do items</span></span>

<span data-ttu-id="4f53b-109">Fügen Sie der `TodoController`-Klasse die folgenden Methoden hinzu, „To-do“-Elemente abzurufen:</span><span class="sxs-lookup"><span data-stu-id="4f53b-109">To get to-do items, add the following methods to the `TodoController` class.</span></span>

<span data-ttu-id="4f53b-110">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]</span><span class="sxs-lookup"><span data-stu-id="4f53b-110">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]</span></span>

<span data-ttu-id="4f53b-111">Diese Methoden implementieren die beiden GET-Methoden:</span><span class="sxs-lookup"><span data-stu-id="4f53b-111">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="4f53b-112">Hier ist eine HTTP-Beispielantwort für die `GetAll`-Methode:</span><span class="sxs-lookup"><span data-stu-id="4f53b-112">Here is an example HTTP response for the `GetAll` method:</span></span>

```
HTTP/1.1 200 OK
   Content-Type: application/json; charset=utf-8
   Server: Microsoft-IIS/10.0
   Date: Thu, 18 Jun 2015 20:51:10 GMT
   Content-Length: 82

   [{"Key":"1", "Name":"Item1","IsComplete":false}]
   ```

<span data-ttu-id="4f53b-113">Später in diesem Tutorial zeige ich Ihnen, wie Sie die HTTP-Antwort mit [Postman](https://www.getpostman.com/) oder [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html) anzeigen können.</span><span class="sxs-lookup"><span data-stu-id="4f53b-113">Later in the tutorial I'll show how you can view the HTTP response using [Postman](https://www.getpostman.com/) or [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="4f53b-114">Routing und URL-Pfade</span><span class="sxs-lookup"><span data-stu-id="4f53b-114">Routing and URL paths</span></span>

<span data-ttu-id="4f53b-115">Das `[HttpGet]`-Attribut gibt eine HTTP GET-Methode an.</span><span class="sxs-lookup"><span data-stu-id="4f53b-115">The `[HttpGet]` attribute specifies an HTTP GET method.</span></span> <span data-ttu-id="4f53b-116">Der URL-Pfad für jede Methode wird wie folgt erstellt:</span><span class="sxs-lookup"><span data-stu-id="4f53b-116">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="4f53b-117">Verwenden Sie die Vorlagenzeichenfolge im „route“-Attribut des Controllers:</span><span class="sxs-lookup"><span data-stu-id="4f53b-117">Take the template string in the controller’s route attribute:</span></span>

<span data-ttu-id="4f53b-118">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="4f53b-118">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]</span></span>

* <span data-ttu-id="4f53b-119">Ersetzen Sie „[Controller]“ durch den Namen des Controllers, bei dem es sich um den Namen der Controller-Klasse ohne das Suffix „Controller“ handelt.</span><span class="sxs-lookup"><span data-stu-id="4f53b-119">Replace "[Controller]" with the name of the controller, which is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="4f53b-120">Bei diesem Beispiel ist der Klassenname des Controllers „**Todo**Controller“ und der Stammname ist „todo“.</span><span class="sxs-lookup"><span data-stu-id="4f53b-120">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="4f53b-121">Beim ASP.NET Core-[Routing](xref:mvc/controllers/routing) wird Groß-/Kleinschreibung nicht beachtet.</span><span class="sxs-lookup"><span data-stu-id="4f53b-121">ASP.NET Core [routing](xref:mvc/controllers/routing) is not case sensitive.</span></span>
* <span data-ttu-id="4f53b-122">Wenn das `[HttpGet]`-Attribut eine Routenvorlage (z.B. `[HttpGet("/products")]`) hat, fügen Sie diese an den Pfad an.</span><span class="sxs-lookup"><span data-stu-id="4f53b-122">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="4f53b-123">In diesem Beispiel wird keine Vorlage verwendet.</span><span class="sxs-lookup"><span data-stu-id="4f53b-123">This sample doesn't use a template.</span></span> <span data-ttu-id="4f53b-124">Weitere Informationen finden Sie unter [Attributrouting mit Http[Verb]-Attributen](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="4f53b-124">See [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes) for more information.</span></span>

<span data-ttu-id="4f53b-125">Für die `GetById`-Methode gilt Folgendes:</span><span class="sxs-lookup"><span data-stu-id="4f53b-125">In the `GetById` method:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(long id)
```

<span data-ttu-id="4f53b-126">`"{id}"` ist eine Platzhaltervariable für die ID des `todo`-Elements.</span><span class="sxs-lookup"><span data-stu-id="4f53b-126">`"{id}"` is a placeholder variable for the ID of the `todo` item.</span></span> <span data-ttu-id="4f53b-127">Wenn `GetById` aufgerufen wird, wird der Wert von „{id}“ in der URL dem Parameter `id` der Methode zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="4f53b-127">When `GetById` is invoked, it assigns the value of "{id}" in the URL to the method's `id` parameter.</span></span>

<span data-ttu-id="4f53b-128">`Name = "GetTodo"` erstellt eine benannte Route und erlaubt Ihnen das Herstellen einer Verknüpfung mit dieser Route in einer HTTP-Antwort.</span><span class="sxs-lookup"><span data-stu-id="4f53b-128">`Name = "GetTodo"` creates a named route and allows you to link to this route in an HTTP Response.</span></span> <span data-ttu-id="4f53b-129">Dies erläutere ich in einem Beispiel weiter unten.</span><span class="sxs-lookup"><span data-stu-id="4f53b-129">I'll explain it with an example later.</span></span> <span data-ttu-id="4f53b-130">Ausführliche Informationen finden Sie unter [Routing zu Controlleraktionen](xref:mvc/controllers/routing).</span><span class="sxs-lookup"><span data-stu-id="4f53b-130">See [Routing to Controller Actions](xref:mvc/controllers/routing) for detailed information.</span></span>

### <a name="return-values"></a><span data-ttu-id="4f53b-131">Rückgabewert</span><span class="sxs-lookup"><span data-stu-id="4f53b-131">Return values</span></span>

<span data-ttu-id="4f53b-132">Die `GetAll`-Methode gibt `IEnumerable` zurück.</span><span class="sxs-lookup"><span data-stu-id="4f53b-132">The `GetAll` method returns an `IEnumerable`.</span></span> <span data-ttu-id="4f53b-133">MVC serialisiert automatisch das Objekt in [JSON](http://www.json.org/) und schreibt den JSON-Code in den Text der Antwortnachricht.</span><span class="sxs-lookup"><span data-stu-id="4f53b-133">MVC automatically serializes the object to [JSON](http://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="4f53b-134">Der Antwortcode für diese Methode ist 200, vorausgesetzt, es gibt keine nicht behandelten Ausnahmen.</span><span class="sxs-lookup"><span data-stu-id="4f53b-134">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="4f53b-135">(Nicht behandelte Ausnahmen werden in 5xx-Fehler übersetzt.)</span><span class="sxs-lookup"><span data-stu-id="4f53b-135">(Unhandled exceptions are translated into 5xx errors.)</span></span>

<span data-ttu-id="4f53b-136">Im Gegensatz dazu gibt die `GetById`-Methode den allgemeineren Typ `IActionResult` zurück, der eine Vielzahl von Rückgabetypen darstellt.</span><span class="sxs-lookup"><span data-stu-id="4f53b-136">In contrast, the `GetById` method returns the more general `IActionResult` type, which represents a wide range of return types.</span></span> <span data-ttu-id="4f53b-137">`GetById` hat zwei unterschiedliche Rückgabetypen:</span><span class="sxs-lookup"><span data-stu-id="4f53b-137">`GetById` has two different return types:</span></span>

* <span data-ttu-id="4f53b-138">Wenn kein Element mit der angeforderten ID übereinstimmt, gibt die Methode einen 404-Fehler zurück.</span><span class="sxs-lookup"><span data-stu-id="4f53b-138">If no item matches the requested ID, the method returns a 404 error.</span></span>  <span data-ttu-id="4f53b-139">Hierzu wird `NotFound` zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="4f53b-139">This is done by returning `NotFound`.</span></span>

* <span data-ttu-id="4f53b-140">Andernfalls gibt die Methode 200 mit einem JSON-Antworttext zurück.</span><span class="sxs-lookup"><span data-stu-id="4f53b-140">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="4f53b-141">Hierzu wird `ObjectResult` zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="4f53b-141">This is done by returning an `ObjectResult`</span></span>
