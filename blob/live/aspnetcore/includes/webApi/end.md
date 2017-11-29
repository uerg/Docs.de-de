## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="c3013-101">Implementieren der anderen CRUD-Vorgänge</span><span class="sxs-lookup"><span data-stu-id="c3013-101">Implement the other CRUD operations</span></span>

<span data-ttu-id="c3013-102">In den folgenden Abschnitten werden dem Controller `Create`-, `Update`- und `Delete`-Methoden hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="c3013-102">In the following sections, `Create`, `Update`, and `Delete` methods are added to the controller.</span></span>

### <a name="create"></a><span data-ttu-id="c3013-103">Erstellen</span><span class="sxs-lookup"><span data-stu-id="c3013-103">Create</span></span>

<span data-ttu-id="c3013-104">Fügen Sie die folgende `Create`-Methode hinzu.</span><span class="sxs-lookup"><span data-stu-id="c3013-104">Add the following `Create` method.</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="c3013-105">Der Code oben ist eine HTTP POST-Methode, die durch das [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute)-Attribut angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="c3013-105">The preceding code is an HTTP POST method, indicated by the [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="c3013-106">Das [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute)-Attribut weist MVC an, den Wert des To-Do-Elements aus dem Text der HTTP-Anforderung abzurufen.</span><span class="sxs-lookup"><span data-stu-id="c3013-106">The [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="c3013-107">Die `CreatedAtRoute`-Methode:</span><span class="sxs-lookup"><span data-stu-id="c3013-107">The `CreatedAtRoute` method:</span></span>

* <span data-ttu-id="c3013-108">Gibt eine 201-Antwort zurück.</span><span class="sxs-lookup"><span data-stu-id="c3013-108">Returns a 201 response.</span></span> <span data-ttu-id="c3013-109">HTTP 201 ist die Standardantwort für eine HTTP POST-Methode, die eine neue Ressource auf dem Server erstellt.</span><span class="sxs-lookup"><span data-stu-id="c3013-109">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="c3013-110">Fügt der Antwort einen Adressheader hinzu.</span><span class="sxs-lookup"><span data-stu-id="c3013-110">Adds a Location header to the response.</span></span> <span data-ttu-id="c3013-111">Der Adressheader gibt den URI des neu erstellten To-Do-Elements zurück.</span><span class="sxs-lookup"><span data-stu-id="c3013-111">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="c3013-112">Siehe [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="c3013-112">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="c3013-113">Verwendet die „GetTodo“ genannte Route, um die URL zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="c3013-113">Uses the "GetTodo" named route to create the URL.</span></span> <span data-ttu-id="c3013-114">Die Route mit dem Namen „GetTodo“ wird in `GetById` definiert:</span><span class="sxs-lookup"><span data-stu-id="c3013-114">The "GetTodo" named route is defined in `GetById`:</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="c3013-115">Verwenden von Postman zum Senden einer Erstellungsanforderung</span><span class="sxs-lookup"><span data-stu-id="c3013-115">Use Postman to send a Create request</span></span>

![Postman-Konsole](../../tutorials/first-web-api/_static/pmc.png)

* <span data-ttu-id="c3013-117">Legen Sie die HTTP-Methode auf `POST` fest.</span><span class="sxs-lookup"><span data-stu-id="c3013-117">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="c3013-118">Wählen Sie das Optionsfeld **Body** aus.</span><span class="sxs-lookup"><span data-stu-id="c3013-118">Select the **Body** radio button</span></span>
* <span data-ttu-id="c3013-119">Wählen Sie das Optionsfeld **raw** aus.</span><span class="sxs-lookup"><span data-stu-id="c3013-119">Select the **raw** radio button</span></span>
* <span data-ttu-id="c3013-120">Legen Sie den Typ auf „JSON“ fest.</span><span class="sxs-lookup"><span data-stu-id="c3013-120">Set the type to JSON</span></span>
* <span data-ttu-id="c3013-121">Geben Sie im Schlüsselwert-Editor zum Beispiel folgendes To-Do-Element ein</span><span class="sxs-lookup"><span data-stu-id="c3013-121">In the key-value editor, enter a Todo item such as</span></span>

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="c3013-122">Klicken Sie auf **Send**.</span><span class="sxs-lookup"><span data-stu-id="c3013-122">Select **Send**</span></span>
* <span data-ttu-id="c3013-123">Klicken Sie auf die Registerkarte „Header“ im unteren Bereich, und kopieren Sie den Header **Location**:</span><span class="sxs-lookup"><span data-stu-id="c3013-123">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Registerkarte „Header“ in der Postman-Konsole](../../tutorials/first-web-api/_static/pmget.png)

<span data-ttu-id="c3013-125">Der Adressheader-URI kann verwendet werden, um auf das neue Element zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="c3013-125">The Location header URI can be used to access the new item.</span></span>

### <a name="update"></a><span data-ttu-id="c3013-126">Aktualisieren</span><span class="sxs-lookup"><span data-stu-id="c3013-126">Update</span></span>

<span data-ttu-id="c3013-127">Fügen Sie die folgende `Update`-Methode hinzu:</span><span class="sxs-lookup"><span data-stu-id="c3013-127">Add the following `Update` method:</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="c3013-128">`Update` ähnelt `Create`, verwendet aber HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="c3013-128">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="c3013-129">Die Antwort ist [204 (Kein Inhalt)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="c3013-129">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="c3013-130">Gemäß der HTTP-Spezifikation erfordert eine PUT-Anforderung, dass der Client die gesamte aktualisierte Entität und nicht nur die Deltas sendet.</span><span class="sxs-lookup"><span data-stu-id="c3013-130">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="c3013-131">Verwenden Sie HTTP PATCH, um Teilupdates zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="c3013-131">To support partial updates, use HTTP PATCH.</span></span>

![Postman-Konsole mit der Antwort „204 (Kein Inhalt)“](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="c3013-133">Löschen</span><span class="sxs-lookup"><span data-stu-id="c3013-133">Delete</span></span>

<span data-ttu-id="c3013-134">Fügen Sie die folgende `Delete`-Methode hinzu:</span><span class="sxs-lookup"><span data-stu-id="c3013-134">Add the following `Delete` method:</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="c3013-135">Die `Delete`-Antwort lautet [204 (Kein Inhalt)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="c3013-135">The `Delete` response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

<span data-ttu-id="c3013-136">Test `Delete`:</span><span class="sxs-lookup"><span data-stu-id="c3013-136">Test `Delete`:</span></span> 

![Postman-Konsole mit der Antwort „204 (Kein Inhalt)“](../../tutorials/first-web-api/_static/pmd.png)
