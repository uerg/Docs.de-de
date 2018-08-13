::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

Der vorherige Code beschreibt eine API-Controllerklasse ohne Methoden. In den nächsten Abschnitten werden Methoden zum Implementieren der API hinzugefügt.
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

Der vorherige Code beschreibt eine API-Controllerklasse ohne Methoden. In den nächsten Abschnitten werden Methoden zum Implementieren der API hinzugefügt. Die Klasse wird mit einem `[ApiController]`-Attribut versehen, um einige praktische Features zu aktivieren. Informationen zu den Features, die durch das Attribut aktiviert werden, finden Sie unter [Kommentieren einer Klasse mithilfe von ApiControllerAttribute](xref:web-api/index#annotate-class-with-apicontrollerattribute).
::: moniker-end

Der Konstruktor des Controllers verwendet [Dependency Injection](xref:fundamentals/dependency-injection) zum Einfügen des Datenbankkontexts (`TodoContext`) in den Controller. Der Datenbankkontext wird in den einzelnen [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete)-Methoden im Controller verwendet. Der Konstruktor fügt ein Element der In-Memory Database hinzu, falls es nicht vorhanden ist.

## <a name="get-to-do-items"></a>Abrufen von To-do-Elementen

Fügen Sie der `TodoController`-Klasse die folgenden Methoden hinzu, um To-do-Elemente abzurufen:

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]
::: moniker-end

Diese Methoden implementieren die beiden GET-Methoden:

* `GET /api/todo`
* `GET /api/todo/{id}`

Hier sehen Sie eine HTTP-Beispielantwort für die Methode `GetAll`:

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

Später in diesem Tutorial sehen Sie, wie die HTTP-Antwort mit [Postman](https://www.getpostman.com/) oder [cURL](https://curl.haxx.se/docs/manpage.html) angezeigt werden kann.

### <a name="routing-and-url-paths"></a>Routing und URL-Pfade

Das Attribut `[HttpGet]` gibt eine Methode an, die auf eine HTTP GET-Anforderung antwortet. Der URL-Pfad für jede Methode wird wie folgt erstellt:

* Verwenden Sie die Vorlagenzeichenfolge im `Route`-Attribut des Controllers:

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]
::: moniker-end

* Ersetzen Sie `[controller]` durch den Namen des Controllers, bei dem es sich um den Namen der Controller-Klasse ohne das Suffix „Controller“ handelt. Bei diesem Beispiel ist der Klassenname des Controllers „**Todo**Controller“ und der Stammname ist „todo“. Beim ASP.NET Core-[Routing](xref:mvc/controllers/routing) wird die Groß-/Kleinschreibung nicht beachtet.
* Wenn das `[HttpGet]`-Attribut eine Routenvorlage (z.B. `[HttpGet("/products")]`) hat, fügen Sie diese an den Pfad an. In diesem Beispiel wird keine Vorlage verwendet. Weitere Informationen finden Sie unter [Attributrouting mit Http[Verb]-Attributen](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).

In der folgenden `GetById`-Methode ist `"{id}"` eine Platzhaltervariable für den eindeutigen Bezeichners des To-do-Elements. Wenn `GetById` aufgerufen wird, wird der Wert von `"{id}"` in der URL dem Parameter `id` der Methode zugewiesen.

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end

`Name = "GetTodo"` erstellt eine benannte Route. Benannte Routen:

* Ermöglichen der App, einen HTTP-Link mit dem Routennamen zu erstellen.
* Werden später in diesem Tutorial erläutert.

### <a name="return-values"></a>Rückgabewert

Die Methode `GetAll` gibt eine Sammlung von `TodoItem`-Objekten zurück. MVC serialisiert automatisch das Objekt in [JSON](https://www.json.org/) und schreibt den JSON-Code in den Text der Antwortnachricht. Der Antwortcode für diese Methode ist 200, vorausgesetzt, es gibt keine nicht behandelten Ausnahmen. Nicht behandelte Ausnahmen werden in 5xx-Fehler übersetzt.

::: moniker range="<= aspnetcore-2.0"
Im Gegensatz dazu gibt die `GetById`-Methode den allgemeineren Typ [IActionResult type](xref:web-api/action-return-types#iactionresult-type) zurück, der eine Vielzahl von Rückgabetypen darstellt. `GetById` hat zwei unterschiedliche Rückgabetypen:

* Wenn kein Element mit der angeforderten ID übereinstimmt, gibt die Methode einen 404-Fehler zurück. Die Rückgabe von [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) gibt eine HTTP 404-Antwort zurück.
* Andernfalls gibt die Methode 200 mit einem JSON-Antworttext zurück. Die Rückgabe von [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) löst eine HTTP 200-Antwort aus.
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
Im Gegensatz dazu gibt die `GetById`-Methode den Typ [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type) zurück, der eine Vielzahl von Rückgabetypen darstellt. `GetById` hat zwei unterschiedliche Rückgabetypen:

* Wenn kein Element mit der angeforderten ID übereinstimmt, gibt die Methode einen 404-Fehler zurück. Die Rückgabe von [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) gibt eine HTTP 404-Antwort zurück.
* Andernfalls gibt die Methode 200 mit einem JSON-Antworttext zurück. Die Rückgabe von `item` löst eine HTTP 200-Antwort aus.
::: moniker-end
