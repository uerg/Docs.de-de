## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="b9b9e-101">Implementieren der anderen CRUD-Vorgänge</span><span class="sxs-lookup"><span data-stu-id="b9b9e-101">Implement the other CRUD operations</span></span>

<span data-ttu-id="b9b9e-102">In den folgenden Abschnitten werden dem Controller `Create`-, `Update`- und `Delete`-Methoden hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="b9b9e-102">In the following sections, `Create`, `Update`, and `Delete` methods are added to the controller.</span></span>

### <a name="create"></a><span data-ttu-id="b9b9e-103">Erstellen</span><span class="sxs-lookup"><span data-stu-id="b9b9e-103">Create</span></span>

<span data-ttu-id="b9b9e-104">Fügen Sie die folgende `Create`-Methode hinzu:</span><span class="sxs-lookup"><span data-stu-id="b9b9e-104">Add the following `Create` method:</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="b9b9e-105">Der vorherige Code ist eine HTTP POST-Methode, die durch das Attribut [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="b9b9e-105">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="b9b9e-106">Das [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)-Attribut weist MVC an, den Wert des To-Do-Elements aus dem Text der HTTP-Anforderung abzurufen.</span><span class="sxs-lookup"><span data-stu-id="b9b9e-106">The [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="b9b9e-107">Der vorherige Code ist eine HTTP POST-Methode, die durch das Attribut [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="b9b9e-107">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="b9b9e-108">MVC ruft den Wert des To-Do-Elements aus dem Text der HTTP-Anforderung ab.</span><span class="sxs-lookup"><span data-stu-id="b9b9e-108">MVC gets the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end

<span data-ttu-id="b9b9e-109">Die `CreatedAtRoute`-Methode:</span><span class="sxs-lookup"><span data-stu-id="b9b9e-109">The `CreatedAtRoute` method:</span></span>

* <span data-ttu-id="b9b9e-110">Gibt eine 201-Antwort zurück.</span><span class="sxs-lookup"><span data-stu-id="b9b9e-110">Returns a 201 response.</span></span> <span data-ttu-id="b9b9e-111">HTTP 201 ist die Standardantwort für eine HTTP POST-Methode, die eine neue Ressource auf dem Server erstellt.</span><span class="sxs-lookup"><span data-stu-id="b9b9e-111">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="b9b9e-112">Fügt der Antwort einen Adressheader hinzu.</span><span class="sxs-lookup"><span data-stu-id="b9b9e-112">Adds a Location header to the response.</span></span> <span data-ttu-id="b9b9e-113">Der Adressheader gibt den URI des neu erstellten To-Do-Elements zurück.</span><span class="sxs-lookup"><span data-stu-id="b9b9e-113">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="b9b9e-114">Siehe [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="b9b9e-114">See [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="b9b9e-115">Verwendet die „GetTodo“ genannte Route, um die URL zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="b9b9e-115">Uses the "GetTodo" named route to create the URL.</span></span> <span data-ttu-id="b9b9e-116">Die Route mit dem Namen „GetTodo“ wird in `GetById` definiert:</span><span class="sxs-lookup"><span data-stu-id="b9b9e-116">The "GetTodo" named route is defined in `GetById`:</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="b9b9e-117">Verwenden von Postman zum Senden einer Erstellungsanforderung</span><span class="sxs-lookup"><span data-stu-id="b9b9e-117">Use Postman to send a Create request</span></span>

* <span data-ttu-id="b9b9e-118">Starten Sie die Anwendung.</span><span class="sxs-lookup"><span data-stu-id="b9b9e-118">Start the app.</span></span>
* <span data-ttu-id="b9b9e-119">Öffnen Sie Postman.</span><span class="sxs-lookup"><span data-stu-id="b9b9e-119">Open Postman.</span></span>

![Postman-Konsole](../../tutorials/first-web-api/_static/pmc.png)

* <span data-ttu-id="b9b9e-121">Aktualisieren Sie die Portnummer in der localhost-URL.</span><span class="sxs-lookup"><span data-stu-id="b9b9e-121">Update the port number in the localhost URL.</span></span>
* <span data-ttu-id="b9b9e-122">Legen Sie die HTTP-Methode auf *POST* fest.</span><span class="sxs-lookup"><span data-stu-id="b9b9e-122">Set the HTTP method to *POST*.</span></span>
* <span data-ttu-id="b9b9e-123">Klicken Sie auf die Registerkarte **Body** (Text).</span><span class="sxs-lookup"><span data-stu-id="b9b9e-123">Click the **Body** tab.</span></span>
* <span data-ttu-id="b9b9e-124">Wählen Sie das Optionsfeld **raw** (Unformatiert) aus.</span><span class="sxs-lookup"><span data-stu-id="b9b9e-124">Select the **raw** radio button.</span></span>
* <span data-ttu-id="b9b9e-125">Legen Sie den Typ auf *JSON (application/json)* fest.</span><span class="sxs-lookup"><span data-stu-id="b9b9e-125">Set the type to *JSON (application/json)*.</span></span>
* <span data-ttu-id="b9b9e-126">Geben Sie einen Anforderungstext mit einem To-Do-Element ein, der folgendem JSON-Code ähnelt:</span><span class="sxs-lookup"><span data-stu-id="b9b9e-126">Enter a request body with a to-do item resembling the following JSON:</span></span>

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* <span data-ttu-id="b9b9e-127">Klicken Sie auf die Schaltfläche **Senden**.</span><span class="sxs-lookup"><span data-stu-id="b9b9e-127">Click the **Send** button.</span></span>

::: moniker range=">= aspnetcore-2.1"
> [!TIP]
> <span data-ttu-id="b9b9e-128">Wenn nach dem Klicken auf **Senden** keine Antwort angezeigt wird, deaktivieren Sie die Option **SSL certification verification** (Überprüfung der SSL-Zertifizierung).</span><span class="sxs-lookup"><span data-stu-id="b9b9e-128">If no response displays after clicking **Send**, disable the **SSL certification verification** option.</span></span> <span data-ttu-id="b9b9e-129">Diese finden Sie unter **Datei** > **Einstellungen**.</span><span class="sxs-lookup"><span data-stu-id="b9b9e-129">This is found under **File** > **Settings**.</span></span> <span data-ttu-id="b9b9e-130">Klicken Sie erneut auf die Schaltfläche **Senden**, nachdem Sie diese Einstellung deaktiviert haben.</span><span class="sxs-lookup"><span data-stu-id="b9b9e-130">Click the **Send** button again after disabling the setting.</span></span>
::: moniker-end

<span data-ttu-id="b9b9e-131">Klicken Sie auf die Registerkarte **Header** im Bereich **Antwort**, und kopieren Sie den Headerwert von **Location** (Speicherort):</span><span class="sxs-lookup"><span data-stu-id="b9b9e-131">Click the **Headers** tab in the **Response** pane and copy the **Location** header value:</span></span>

![Registerkarte „Header“ in der Postman-Konsole](../../tutorials/first-web-api/_static/pmc2.png)

<span data-ttu-id="b9b9e-133">Der Adressheader-URI kann verwendet werden, um auf das neue Element zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="b9b9e-133">The Location header URI can be used to access the new item.</span></span>

### <a name="update"></a><span data-ttu-id="b9b9e-134">Update</span><span class="sxs-lookup"><span data-stu-id="b9b9e-134">Update</span></span>

<span data-ttu-id="b9b9e-135">Fügen Sie die folgende `Update`-Methode hinzu:</span><span class="sxs-lookup"><span data-stu-id="b9b9e-135">Add the following `Update` method:</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end

<span data-ttu-id="b9b9e-136">`Update` ähnelt `Create`, verwendet allerdings HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="b9b9e-136">`Update` is similar to `Create`, except it uses HTTP PUT.</span></span> <span data-ttu-id="b9b9e-137">Die Antwort ist [204 (Kein Inhalt)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="b9b9e-137">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="b9b9e-138">Gemäß der HTTP-Spezifikation erfordert eine PUT-Anforderung, dass der Client die gesamte aktualisierte Entität (nicht nur die Deltas) sendet.</span><span class="sxs-lookup"><span data-stu-id="b9b9e-138">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="b9b9e-139">Verwenden Sie HTTP PATCH, um Teilupdates zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="b9b9e-139">To support partial updates, use HTTP PATCH.</span></span>

<span data-ttu-id="b9b9e-140">Verwenden Sie Postman, um den Namen des To-do-Elements in „walk cat“ zu ändern:</span><span class="sxs-lookup"><span data-stu-id="b9b9e-140">Use Postman to update the to-do item's name to "walk cat":</span></span>

![Postman-Konsole mit der Antwort „204 (Kein Inhalt)“](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="b9b9e-142">Löschen</span><span class="sxs-lookup"><span data-stu-id="b9b9e-142">Delete</span></span>

<span data-ttu-id="b9b9e-143">Fügen Sie die folgende `Delete`-Methode hinzu:</span><span class="sxs-lookup"><span data-stu-id="b9b9e-143">Add the following `Delete` method:</span></span>

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="b9b9e-144">Die `Delete`-Antwort lautet [204 (Kein Inhalt)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="b9b9e-144">The `Delete` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

<span data-ttu-id="b9b9e-145">Verwenden Sie Postman, um das To-do-Element zu löschen:</span><span class="sxs-lookup"><span data-stu-id="b9b9e-145">Use Postman to delete the to-do item:</span></span>

![Postman-Konsole mit der Antwort „204 (Kein Inhalt)“](../../tutorials/first-web-api/_static/pmd.png)
