## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="b69cd-101">Implementieren der anderen CRUD-Vorgänge</span><span class="sxs-lookup"><span data-stu-id="b69cd-101">Implement the other CRUD operations</span></span>

<span data-ttu-id="b69cd-102">In den folgenden Abschnitten werden dem Controller `Create`-, `Update`- und `Delete`-Methoden hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="b69cd-102">In the following sections, `Create`, `Update`, and `Delete` methods are added to the controller.</span></span>

### <a name="create"></a><span data-ttu-id="b69cd-103">Erstellen</span><span class="sxs-lookup"><span data-stu-id="b69cd-103">Create</span></span>

<span data-ttu-id="b69cd-104">Fügen Sie die folgende `Create`-Methode hinzu:</span><span class="sxs-lookup"><span data-stu-id="b69cd-104">Add the following `Create` method:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="b69cd-105">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="b69cd-105">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span></span>

<span data-ttu-id="b69cd-106">Der vorherige Code ist eine HTTP POST-Methode, die durch das Attribut [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="b69cd-106">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="b69cd-107">Das [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)-Attribut weist MVC an, den Wert des To-Do-Elements aus dem Text der HTTP-Anforderung abzurufen.</span><span class="sxs-lookup"><span data-stu-id="b69cd-107">The [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="b69cd-108">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="b69cd-108">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span></span>

<span data-ttu-id="b69cd-109">Der vorherige Code ist eine HTTP POST-Methode, die durch das Attribut [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="b69cd-109">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="b69cd-110">MVC ruft den Wert des To-Do-Elements aus dem Text der HTTP-Anforderung ab.</span><span class="sxs-lookup"><span data-stu-id="b69cd-110">MVC gets the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end

<span data-ttu-id="b69cd-111">Die `CreatedAtRoute`-Methode:</span><span class="sxs-lookup"><span data-stu-id="b69cd-111">The `CreatedAtRoute` method:</span></span>

* <span data-ttu-id="b69cd-112">Gibt eine 201-Antwort zurück.</span><span class="sxs-lookup"><span data-stu-id="b69cd-112">Returns a 201 response.</span></span> <span data-ttu-id="b69cd-113">HTTP 201 ist die Standardantwort für eine HTTP POST-Methode, die eine neue Ressource auf dem Server erstellt.</span><span class="sxs-lookup"><span data-stu-id="b69cd-113">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="b69cd-114">Fügt der Antwort einen Adressheader hinzu.</span><span class="sxs-lookup"><span data-stu-id="b69cd-114">Adds a Location header to the response.</span></span> <span data-ttu-id="b69cd-115">Der Adressheader gibt den URI des neu erstellten To-Do-Elements zurück.</span><span class="sxs-lookup"><span data-stu-id="b69cd-115">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="b69cd-116">Siehe [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="b69cd-116">See [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="b69cd-117">Verwendet die „GetTodo“ genannte Route, um die URL zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="b69cd-117">Uses the "GetTodo" named route to create the URL.</span></span> <span data-ttu-id="b69cd-118">Die Route mit dem Namen „GetTodo“ wird in `GetById` definiert:</span><span class="sxs-lookup"><span data-stu-id="b69cd-118">The "GetTodo" named route is defined in `GetById`:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="b69cd-119">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span><span class="sxs-lookup"><span data-stu-id="b69cd-119">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="b69cd-120">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span><span class="sxs-lookup"><span data-stu-id="b69cd-120">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span></span>
::: moniker-end

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="b69cd-121">Verwenden von Postman zum Senden einer Erstellungsanforderung</span><span class="sxs-lookup"><span data-stu-id="b69cd-121">Use Postman to send a Create request</span></span>

* <span data-ttu-id="b69cd-122">Starten Sie die Anwendung.</span><span class="sxs-lookup"><span data-stu-id="b69cd-122">Start the app.</span></span>
* <span data-ttu-id="b69cd-123">Öffnen Sie Postman.</span><span class="sxs-lookup"><span data-stu-id="b69cd-123">Open Postman.</span></span>

![Postman-Konsole](../../tutorials/first-web-api/_static/pmc.png)

* <span data-ttu-id="b69cd-125">Aktualisieren Sie die Portnummer in der localhost-URL.</span><span class="sxs-lookup"><span data-stu-id="b69cd-125">Update the port number in the localhost URL.</span></span>
* <span data-ttu-id="b69cd-126">Legen Sie die HTTP-Methode auf *POST* fest.</span><span class="sxs-lookup"><span data-stu-id="b69cd-126">Set the HTTP method to *POST*.</span></span>
* <span data-ttu-id="b69cd-127">Klicken Sie auf die Registerkarte **Body** (Text).</span><span class="sxs-lookup"><span data-stu-id="b69cd-127">Click the **Body** tab.</span></span>
* <span data-ttu-id="b69cd-128">Wählen Sie das Optionsfeld **raw** (Unformatiert) aus.</span><span class="sxs-lookup"><span data-stu-id="b69cd-128">Select the **raw** radio button.</span></span>
* <span data-ttu-id="b69cd-129">Legen Sie den Typ auf *JSON (application/json)* fest.</span><span class="sxs-lookup"><span data-stu-id="b69cd-129">Set the type to *JSON (application/json)*.</span></span>
* <span data-ttu-id="b69cd-130">Geben Sie einen Anforderungstext mit einem To-Do-Element ein, der folgendem JSON-Code ähnelt:</span><span class="sxs-lookup"><span data-stu-id="b69cd-130">Enter a request body with a to-do item resembling the following JSON:</span></span>

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* <span data-ttu-id="b69cd-131">Klicken Sie auf die Schaltfläche **Senden**.</span><span class="sxs-lookup"><span data-stu-id="b69cd-131">Click the **Send** button.</span></span>

::: moniker range=">= aspnetcore-2.1"
> [!TIP]
> <span data-ttu-id="b69cd-132">Wenn nach dem Klicken auf **Senden** keine Antwort angezeigt wird, deaktivieren Sie die Option **SSL certification verification** (Überprüfung der SSL-Zertifizierung).</span><span class="sxs-lookup"><span data-stu-id="b69cd-132">If no response displays after clicking **Send**, disable the **SSL certification verification** option.</span></span> <span data-ttu-id="b69cd-133">Diese finden Sie unter **Datei** > **Einstellungen**.</span><span class="sxs-lookup"><span data-stu-id="b69cd-133">This is found under **File** > **Settings**.</span></span> <span data-ttu-id="b69cd-134">Klicken Sie erneut auf die Schaltfläche **Senden**, nachdem Sie diese Einstellung deaktiviert haben.</span><span class="sxs-lookup"><span data-stu-id="b69cd-134">Click the **Send** button again after disabling the setting.</span></span>
::: moniker-end

<span data-ttu-id="b69cd-135">Klicken Sie auf die Registerkarte **Header** im Bereich **Antwort**, und kopieren Sie den Headerwert von **Location** (Speicherort):</span><span class="sxs-lookup"><span data-stu-id="b69cd-135">Click the **Headers** tab in the **Response** pane and copy the **Location** header value:</span></span>

![Registerkarte „Header“ in der Postman-Konsole](../../tutorials/first-web-api/_static/pmc2.png)

<span data-ttu-id="b69cd-137">Der Adressheader-URI kann verwendet werden, um auf das neue Element zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="b69cd-137">The Location header URI can be used to access the new item.</span></span>

### <a name="update"></a><span data-ttu-id="b69cd-138">Update</span><span class="sxs-lookup"><span data-stu-id="b69cd-138">Update</span></span>

<span data-ttu-id="b69cd-139">Fügen Sie die folgende `Update`-Methode hinzu:</span><span class="sxs-lookup"><span data-stu-id="b69cd-139">Add the following `Update` method:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="b69cd-140">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span><span class="sxs-lookup"><span data-stu-id="b69cd-140">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="b69cd-141">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span><span class="sxs-lookup"><span data-stu-id="b69cd-141">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span></span>
::: moniker-end

<span data-ttu-id="b69cd-142">`Update` ähnelt `Create`, verwendet allerdings HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="b69cd-142">`Update` is similar to `Create`, except it uses HTTP PUT.</span></span> <span data-ttu-id="b69cd-143">Die Antwort ist [204 (Kein Inhalt)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="b69cd-143">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="b69cd-144">Gemäß der HTTP-Spezifikation erfordert eine PUT-Anforderung, dass der Client die gesamte aktualisierte Entität (nicht nur die Deltas) sendet.</span><span class="sxs-lookup"><span data-stu-id="b69cd-144">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="b69cd-145">Verwenden Sie HTTP PATCH, um Teilupdates zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="b69cd-145">To support partial updates, use HTTP PATCH.</span></span>

<span data-ttu-id="b69cd-146">Verwenden Sie Postman, um den Namen des To-do-Elements in „walk cat“ zu ändern:</span><span class="sxs-lookup"><span data-stu-id="b69cd-146">Use Postman to update the to-do item's name to "walk cat":</span></span>

![Postman-Konsole mit der Antwort „204 (Kein Inhalt)“](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="b69cd-148">Löschen</span><span class="sxs-lookup"><span data-stu-id="b69cd-148">Delete</span></span>

<span data-ttu-id="b69cd-149">Fügen Sie die folgende `Delete`-Methode hinzu:</span><span class="sxs-lookup"><span data-stu-id="b69cd-149">Add the following `Delete` method:</span></span>

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="b69cd-150">Die `Delete`-Antwort lautet [204 (Kein Inhalt)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="b69cd-150">The `Delete` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

<span data-ttu-id="b69cd-151">Verwenden Sie Postman, um das To-do-Element zu löschen:</span><span class="sxs-lookup"><span data-stu-id="b69cd-151">Use Postman to delete the to-do item:</span></span>

![Postman-Konsole mit der Antwort „204 (Kein Inhalt)“](../../tutorials/first-web-api/_static/pmd.png)
