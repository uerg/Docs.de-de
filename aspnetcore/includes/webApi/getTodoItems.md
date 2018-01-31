[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

Der vorangehende Code:

* Definiert eine leere Controller-Klasse. In den nächsten Abschnitten werden Methoden zum Implementieren der API hinzugefügt.
* Der Konstruktor verwendet die [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) zum Einfügen des Datenbankkontexts (`TodoContext `) in den Controller. Der Datenbankkontext wird in den einzelnen [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete)-Methoden im Controller verwendet.
* Der Konstruktor fügt ein Element der In-Memory Database hinzu, falls es nicht vorhanden ist.

## <a name="getting-to-do-items"></a>Abrufen von „To-do“-Elementen

Fügen Sie der `TodoController`-Klasse die folgenden Methoden hinzu, „To-do“-Elemente abzurufen:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

Diese Methoden implementieren die beiden GET-Methoden:

* `GET /api/todo`
* `GET /api/todo/{id}`

Hier ist eine HTTP-Beispielantwort für die `GetAll`-Methode:

```
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
   ```

Später in diesem Tutorial zeige ich Ihnen, wie die HTTP-Antwort mit [Postman](https://www.getpostman.com/) oder [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html) angezeigt werden kann.

### <a name="routing-and-url-paths"></a>Routing und URL-Pfade

Das `[HttpGet]`-Attribut gibt eine HTTP GET-Methode an. Der URL-Pfad für jede Methode wird wie folgt erstellt:

* Verwenden Sie die Vorlagenzeichenfolge im `Route`-Attribut des Controllers:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* Ersetzen Sie `[controller]` durch den Namen des Controllers, bei dem es sich um den Namen der Controller-Klasse ohne das Suffix „Controller“ handelt. Bei diesem Beispiel ist der Klassenname des Controllers „**Todo**Controller“ und der Stammname ist „todo“. Beim ASP.NET Core-[Routing](xref:mvc/controllers/routing) wird die Groß- und Kleinschreibung nicht beachtet.
* Wenn das `[HttpGet]`-Attribut eine Routenvorlage (z.B. `[HttpGet("/products")]`) hat, fügen Sie diese an den Pfad an. In diesem Beispiel wird keine Vorlage verwendet. Weitere Informationen finden Sie unter [Attributrouting mit Http[Verb]-Attributen](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).

Für die `GetById`-Methode gilt Folgendes:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

`"{id}"` ist eine Platzhaltervariable für die ID des `todo`-Elements. Wenn `GetById` aufgerufen wird, wird der Wert von „{id}“ in der URL dem Parameter `id` der Methode zugewiesen.

`Name = "GetTodo"` erstellt eine benannte Route. Benannte Routen:

* Ermöglichen der App, einen HTTP-Link mit dem Routennamen zu erstellen.
* Werden später in diesem Tutorial erläutert.

### <a name="return-values"></a>Rückgabewert

Die `GetAll`-Methode gibt `IEnumerable` zurück. MVC serialisiert automatisch das Objekt in [JSON](http://www.json.org/) und schreibt den JSON-Code in den Text der Antwortnachricht. Der Antwortcode für diese Methode ist 200, vorausgesetzt, es gibt keine nicht behandelten Ausnahmen. (Nicht behandelte Ausnahmen werden in 5xx-Fehler übersetzt.)

Im Gegensatz dazu gibt die `GetById`-Methode den allgemeineren Typ `IActionResult` zurück, der eine Vielzahl von Rückgabetypen darstellt. `GetById` hat zwei unterschiedliche Rückgabetypen:

* Wenn kein Element mit der angeforderten ID übereinstimmt, gibt die Methode einen 404-Fehler zurück. Die Rückgabe von `NotFound` gibt eine HTTP 404-Antwort zurück.

* Andernfalls gibt die Methode 200 mit einem JSON-Antworttext zurück. Die Rückgabe von `ObjectResult` gibt eine HTTP 200-Antwort zurück.
