## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="e1cba-101">Implementieren der anderen CRUD-Vorgänge</span><span class="sxs-lookup"><span data-stu-id="e1cba-101">Implement the other CRUD operations</span></span>

<span data-ttu-id="e1cba-102">Fügen Sie Methoden `Create`, `Update` und `Delete` zum Controller hinzu.</span><span class="sxs-lookup"><span data-stu-id="e1cba-102">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="e1cba-103">Dabei handelt sich um Variationen eines Designs, darum wird der Code mit den hervorgehobenen Hauptunterschieden dargestellt.</span><span class="sxs-lookup"><span data-stu-id="e1cba-103">These are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="e1cba-104">Erstellen Sie das Projekt, nachdem Sie Code hinzugefügt oder geändert haben.</span><span class="sxs-lookup"><span data-stu-id="e1cba-104">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="e1cba-105">Erstellen</span><span class="sxs-lookup"><span data-stu-id="e1cba-105">Create</span></span>

<span data-ttu-id="e1cba-106">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="e1cba-106">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span></span>

<span data-ttu-id="e1cba-107">Dies ist eine HTTP-POST-Methode, die vom [`[HttpPost]`](https://docs.microsoft.com/aspnet/core/api)-Attribut angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="e1cba-107">This is an HTTP POST method, indicated by the [`[HttpPost]`](https://docs.microsoft.com/aspnet/core/api) attribute.</span></span> <span data-ttu-id="e1cba-108">Das [`[FromBody]`](https://docs.microsoft.com/aspnet/core/api)-Attribut weist MVC an, den Wert des To-Do-Elements aus dem Text der HTTP-Anforderung abzurufen.</span><span class="sxs-lookup"><span data-stu-id="e1cba-108">The [`[FromBody]`](https://docs.microsoft.com/aspnet/core/api) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="e1cba-109">Die `CreatedAtRoute`-Methode gibt die Antwort „201“ zurück, bei der es sich um die Standardantwort für eine HTTP-POST-Methode handelt, die eine neue Ressource auf dem Server erstellt.</span><span class="sxs-lookup"><span data-stu-id="e1cba-109">The `CreatedAtRoute` method returns a 201 response, which is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="e1cba-110">`CreatedAtRoute` fügt der Antwort außerdem einen Adressheader hinzu.</span><span class="sxs-lookup"><span data-stu-id="e1cba-110">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="e1cba-111">Der Adressheader gibt den URI des neu erstellten To-Do-Elements zurück.</span><span class="sxs-lookup"><span data-stu-id="e1cba-111">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="e1cba-112">Siehe [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="e1cba-112">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="e1cba-113">Verwenden von Postman zum Senden einer Erstellungsanforderung</span><span class="sxs-lookup"><span data-stu-id="e1cba-113">Use Postman to send a Create request</span></span>

![Postman-Konsole](../../tutorials/first-web-api/_static/pmc.png)

* <span data-ttu-id="e1cba-115">Legen Sie die HTTP-Methode auf `POST` fest.</span><span class="sxs-lookup"><span data-stu-id="e1cba-115">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="e1cba-116">Wählen Sie das Optionsfeld **Body** aus.</span><span class="sxs-lookup"><span data-stu-id="e1cba-116">Select the **Body** radio button</span></span>
* <span data-ttu-id="e1cba-117">Wählen Sie das Optionsfeld **raw** aus.</span><span class="sxs-lookup"><span data-stu-id="e1cba-117">Select the **raw** radio button</span></span>
* <span data-ttu-id="e1cba-118">Legen Sie den Typ auf „JSON“ fest.</span><span class="sxs-lookup"><span data-stu-id="e1cba-118">Set the type to JSON</span></span>
* <span data-ttu-id="e1cba-119">Geben Sie im Schlüsselwert-Editor zum Beispiel folgendes To-Do-Element ein</span><span class="sxs-lookup"><span data-stu-id="e1cba-119">In the key-value editor, enter a Todo item such as</span></span> 

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="e1cba-120">Klicken Sie auf **Send**.</span><span class="sxs-lookup"><span data-stu-id="e1cba-120">Select **Send**</span></span>

* <span data-ttu-id="e1cba-121">Klicken Sie auf die Registerkarte „Header“ im unteren Bereich, und kopieren Sie den Header **Location**:</span><span class="sxs-lookup"><span data-stu-id="e1cba-121">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Registerkarte „Header“ in der Postman-Konsole](../../tutorials/first-web-api/_static/pmget.png)

<span data-ttu-id="e1cba-123">Sie können den Adressheader-URI verwenden, um auf die Ressource zuzugreifen, die Sie gerade erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="e1cba-123">You can use the Location header URI to access the resource you just created.</span></span> <span data-ttu-id="e1cba-124">Denken Sie daran, dass die `GetById`-Methode die benannte Route `"GetTodo"` erstellt hat:</span><span class="sxs-lookup"><span data-stu-id="e1cba-124">Recall the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(long id)
```

### <a name="update"></a><span data-ttu-id="e1cba-125">Aktualisieren</span><span class="sxs-lookup"><span data-stu-id="e1cba-125">Update</span></span>

<span data-ttu-id="e1cba-126">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span><span class="sxs-lookup"><span data-stu-id="e1cba-126">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span></span>

<span data-ttu-id="e1cba-127">`Update` ähnelt `Create`, verwendet aber HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="e1cba-127">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="e1cba-128">Die Antwort ist [204 (Kein Inhalt)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="e1cba-128">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="e1cba-129">Gemäß der HTTP-Spezifikation erfordert eine PUT-Anforderung, dass der Client die gesamte aktualisierte Entität und nicht nur die Deltas sendet.</span><span class="sxs-lookup"><span data-stu-id="e1cba-129">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="e1cba-130">Verwenden Sie HTTP PATCH, um Teilupdates zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="e1cba-130">To support partial updates, use HTTP PATCH.</span></span>

![Postman-Konsole mit der Antwort „204 (Kein Inhalt)“](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="e1cba-132">Löschen</span><span class="sxs-lookup"><span data-stu-id="e1cba-132">Delete</span></span>

<span data-ttu-id="e1cba-133">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]</span><span class="sxs-lookup"><span data-stu-id="e1cba-133">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]</span></span>

<span data-ttu-id="e1cba-134">Die Antwort ist [204 (Kein Inhalt)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="e1cba-134">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![Postman-Konsole mit der Antwort „204 (Kein Inhalt)“](../../tutorials/first-web-api/_static/pmd.png)
