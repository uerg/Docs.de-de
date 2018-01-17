---
title: "Erstellen einer Web-API mit ASP.NET Core und Visual Studio für Mac"
description: "Erstellen einer Web-API mit ASP.NET Core MVC und Visual Studio für Mac"
author: rick-anderson
ms.author: riande
ms.date: 09/15/2017
ms.topic: get-started-article
ms.prod: asp.net-core
uid: tutorials/first-web-api-mac
helpviewer_heywords: ASP.NET Core, WebAPI, Web API, REST, mac, macOS, HTTP, Service, HTTP Service
ms.technology: aspnet
keywords: ASP.NET Core, WebAPI, Web-API, REST, Mac, macOS, HTTP, Service, HTTP-Dienst
manager: wpickett
ms.openlocfilehash: 3f84fa55baf6d808185a28db290d9e6d3c46bdac
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2018
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="5b966-104">Erstellen einer Web-API mit ASP.NET Core MVC und Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="5b966-104">Create a Web API with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="5b966-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT) und [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="5b966-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="5b966-106">In diesem Tutorial erstellen Sie eine Web-API zum Verwalten einer Liste von „To-Do“-Elementen.</span><span class="sxs-lookup"><span data-stu-id="5b966-106">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="5b966-107">Sie werden keine Benutzeroberfläche erstellen.</span><span class="sxs-lookup"><span data-stu-id="5b966-107">You won’t build a UI.</span></span>

<span data-ttu-id="5b966-108">Es gibt drei Versionen dieses Tutorials:</span><span class="sxs-lookup"><span data-stu-id="5b966-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="5b966-109">macOS: Web-API mit Visual Studio für Mac (dieses Tutorial)</span><span class="sxs-lookup"><span data-stu-id="5b966-109">macOS: Web API with Visual Studio for Mac (This tutorial)</span></span>
* <span data-ttu-id="5b966-110">Windows: [Web-API mit Visual Studio für Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="5b966-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>
* <span data-ttu-id="5b966-111">macOS, Linux, Windows: [Web-API mit Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="5b966-111">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

* <span data-ttu-id="5b966-112">Ein Beispiel für die Verwendung einer beständigen Datenbank finden Sie unter [Einführung in ASP.NET Core MVC unter Mac oder Linux](xref:tutorials/first-mvc-app-xplat/index).</span><span class="sxs-lookup"><span data-stu-id="5b966-112">See [Introduction to ASP.NET Core MVC on Mac or Linux](xref:tutorials/first-mvc-app-xplat/index) for an example that uses a persistent database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5b966-113">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="5b966-113">Prerequisites</span></span>

<span data-ttu-id="5b966-114">Installieren Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="5b966-114">Install the following:</span></span>

- <span data-ttu-id="5b966-115">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) oder höher</span><span class="sxs-lookup"><span data-stu-id="5b966-115">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
- [<span data-ttu-id="5b966-116">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="5b966-116">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-the-project"></a><span data-ttu-id="5b966-117">Erstellen eines Projekts</span><span class="sxs-lookup"><span data-stu-id="5b966-117">Create the project</span></span>

<span data-ttu-id="5b966-118">Klicken Sie in Visual Studio auf **Datei > Neue Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="5b966-118">From Visual Studio, select **File > New Solution**.</span></span>

![Neue Projektmappe in macOS](first-web-api-mac/_static/sln.png)

<span data-ttu-id="5b966-120">Klicken Sie auf **.NET Core-App > ASP.NET Core-Web-API > Weiter**.</span><span class="sxs-lookup"><span data-stu-id="5b966-120">Select **.NET Core App >  ASP.NET Core Web API > Next**.</span></span>

![Dialogfeld „Neue Projektmappe“ in macOS](first-web-api-mac/_static/1.png)

<span data-ttu-id="5b966-122">Geben Sie **TodoApi** als **Projektnamen** ein, und klicken Sie dann auf „Erstellen“.</span><span class="sxs-lookup"><span data-stu-id="5b966-122">Enter **TodoApi** for the **Project Name**, and then select Create.</span></span>

![Dialogfeld „Konfiguration“](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="5b966-124">Starten der App</span><span class="sxs-lookup"><span data-stu-id="5b966-124">Launch the app</span></span>

<span data-ttu-id="5b966-125">Klicken Sie in Visual Studio auf **Ausführen > Mit Debugging starten**, um die App zu starten.</span><span class="sxs-lookup"><span data-stu-id="5b966-125">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="5b966-126">Visual Studio startet dann einen Browser und navigiert zu `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="5b966-126">Visual Studio launches a browser and navigates to `http://localhost:5000`.</span></span> <span data-ttu-id="5b966-127">Ihnen wird der HTTP-Fehler „404 (Not Found)“ angezeigt.</span><span class="sxs-lookup"><span data-stu-id="5b966-127">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="5b966-128">Ändern Sie die URL zu `http://localhost:port/api/values`.</span><span class="sxs-lookup"><span data-stu-id="5b966-128">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="5b966-129">Die `ValuesController`-Daten werden angezeigt:</span><span class="sxs-lookup"><span data-stu-id="5b966-129">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="5b966-130">Hinzufügen der Unterstützung für Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="5b966-130">Add support for Entity Framework Core</span></span>

<span data-ttu-id="5b966-131">Installieren Sie den Datenbankanbieter [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/).</span><span class="sxs-lookup"><span data-stu-id="5b966-131">Install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="5b966-132">Dieser Datenbankanbieter ermöglicht, dass Entity Framework Core mit einer In-Memory Database verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="5b966-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

* <span data-ttu-id="5b966-133">Klicken Sie im Menü **Projekt** auf **NuGet-Pakete hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="5b966-133">From the **Project** menu, select **Add NuGet Packages**.</span></span> 

  *  <span data-ttu-id="5b966-134">Sie können alternativ mit der rechten Maustaste auf **Abhängigkeiten** und dann auf **Pakete hinzufügen** klicken.</span><span class="sxs-lookup"><span data-stu-id="5b966-134">Alternately, you can right-click **Dependencies**, and then select **Add Packages**.</span></span>

* <span data-ttu-id="5b966-135">Geben Sie `EntityFrameworkCore.InMemory` in das Suchfeld ein.</span><span class="sxs-lookup"><span data-stu-id="5b966-135">Enter `EntityFrameworkCore.InMemory` in the search box.</span></span>
* <span data-ttu-id="5b966-136">Wählen Sie `Microsoft.EntityFrameworkCore.InMemory` aus, und klicken Sie dann auf **Pakete hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="5b966-136">Select `Microsoft.EntityFrameworkCore.InMemory`, and then select **Add Package**.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="5b966-137">Hinzufügen einer Modellklasse</span><span class="sxs-lookup"><span data-stu-id="5b966-137">Add a model class</span></span>

<span data-ttu-id="5b966-138">Ein Modell ist ein Objekt, das die Daten in Ihrer Anwendung darstellt.</span><span class="sxs-lookup"><span data-stu-id="5b966-138">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="5b966-139">In diesem Fall ist das einzige Modell ein To-do-Element.</span><span class="sxs-lookup"><span data-stu-id="5b966-139">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="5b966-140">Fügen Sie einen Ordner namens *Models* hinzu.</span><span class="sxs-lookup"><span data-stu-id="5b966-140">Add a folder named *Models*.</span></span> <span data-ttu-id="5b966-141">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt.</span><span class="sxs-lookup"><span data-stu-id="5b966-141">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="5b966-142">Klicken Sie auf **Hinzufügen** > **Neuer Ordner**.</span><span class="sxs-lookup"><span data-stu-id="5b966-142">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="5b966-143">Geben Sie dem Ordner den Namen *Modelle*.</span><span class="sxs-lookup"><span data-stu-id="5b966-143">Name the folder *Models*.</span></span>

![Neuer Ordner](first-web-api-mac/_static/folder.png)

<span data-ttu-id="5b966-145">Hinweis: Sie können Modellklassen überall in Ihrem Projekt unterbringen, aber der Ordner *Modelle* wird gemäß der Konvention verwendet.</span><span class="sxs-lookup"><span data-stu-id="5b966-145">Note: You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="5b966-146">Fügen Sie eine `TodoItem`-Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="5b966-146">Add a `TodoItem` class.</span></span> <span data-ttu-id="5b966-147">Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle*, und klicken Sie auf **Hinzufügen > Neue Datei > Allgemein > Leere Klasse**.</span><span class="sxs-lookup"><span data-stu-id="5b966-147">Right-click the *Models* folder and select **Add > New File > General > Empty Class**.</span></span> <span data-ttu-id="5b966-148">Benennen Sie die Klasse `TodoItem`, und klicken Sie dann auf **Neu**.</span><span class="sxs-lookup"><span data-stu-id="5b966-148">Name the class `TodoItem`, and then select **New**.</span></span>

<span data-ttu-id="5b966-149">Ersetzen Sie den generierten Code mit Folgendem:</span><span class="sxs-lookup"><span data-stu-id="5b966-149">Replace the generated code with:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="5b966-150">Die Datenbank generiert die `Id`, wenn ein `TodoItem` erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="5b966-150">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="5b966-151">Erstellen des Datenbankkontexts</span><span class="sxs-lookup"><span data-stu-id="5b966-151">Create the database context</span></span>

<span data-ttu-id="5b966-152">Der *Datenbankkontext* ist die Hauptklasse, die die Entity Framework-Funktionen für ein angegebenes Datenmodell koordiniert.</span><span class="sxs-lookup"><span data-stu-id="5b966-152">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="5b966-153">Sie können diese Klasse durch Ableiten von der `Microsoft.EntityFrameworkCore.DbContext`-Klasse erstellen.</span><span class="sxs-lookup"><span data-stu-id="5b966-153">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="5b966-154">Fügen Sie eine `TodoContext`-Klasse zum Ordner *Modelle* hinzu.</span><span class="sxs-lookup"><span data-stu-id="5b966-154">Add a `TodoContext` class to the *Models* folder.</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="5b966-155">Hinzufügen eines Controllers</span><span class="sxs-lookup"><span data-stu-id="5b966-155">Add a controller</span></span>

<span data-ttu-id="5b966-156">Fügen Sie im Ordner *Controller* im Projektmappen-Explorer die Klasse `TodoController` hinzu.</span><span class="sxs-lookup"><span data-stu-id="5b966-156">In Solution Explorer, in the *Controllers* folder, add the class `TodoController`.</span></span>

<span data-ttu-id="5b966-157">Ersetzen Sie den generierten Code mit Folgendem (und fügen Sie schließende Klammern hinzu):</span><span class="sxs-lookup"><span data-stu-id="5b966-157">Replace the generated code with the following (and add closing braces):</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="5b966-158">Starten der App</span><span class="sxs-lookup"><span data-stu-id="5b966-158">Launch the app</span></span>

<span data-ttu-id="5b966-159">Klicken Sie in Visual Studio auf **Ausführen > Mit Debugging starten**, um die App zu starten.</span><span class="sxs-lookup"><span data-stu-id="5b966-159">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="5b966-160">Visual Studio startet einen Browser und navigiert zu `http://localhost:port`, wobei *port* eine zufällig ausgewählte Portnummer ist.</span><span class="sxs-lookup"><span data-stu-id="5b966-160">Visual Studio launches a browser and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="5b966-161">Ihnen wird der HTTP-Fehler „404 (Not Found)“ angezeigt.</span><span class="sxs-lookup"><span data-stu-id="5b966-161">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="5b966-162">Ändern Sie die URL zu `http://localhost:port/api/values`.</span><span class="sxs-lookup"><span data-stu-id="5b966-162">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="5b966-163">Die `ValuesController`-Daten werden angezeigt:</span><span class="sxs-lookup"><span data-stu-id="5b966-163">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

<span data-ttu-id="5b966-164">Navigieren Sie zum `Todo`-Controller auf `http://localhost:port/api/todo`:</span><span class="sxs-lookup"><span data-stu-id="5b966-164">Navigate to the `Todo` controller at`http://localhost:port/api/todo`:</span></span>

```
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="5b966-165">Implementieren der anderen CRUD-Vorgänge</span><span class="sxs-lookup"><span data-stu-id="5b966-165">Implement the other CRUD operations</span></span>

<span data-ttu-id="5b966-166">Fügen Sie Methoden `Create`, `Update` und `Delete` zum Controller hinzu.</span><span class="sxs-lookup"><span data-stu-id="5b966-166">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="5b966-167">Dabei handelt sich um Variationen eines Designs, darum wird der Code mit den hervorgehobenen Hauptunterschieden dargestellt.</span><span class="sxs-lookup"><span data-stu-id="5b966-167">These are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="5b966-168">Erstellen Sie das Projekt, nachdem Sie Code hinzugefügt oder geändert haben.</span><span class="sxs-lookup"><span data-stu-id="5b966-168">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="5b966-169">Erstellen</span><span class="sxs-lookup"><span data-stu-id="5b966-169">Create</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="5b966-170">Dies ist eine HTTP-POST-Methode, die vom [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute)-Attribut angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="5b966-170">This is an HTTP POST method, indicated by the [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="5b966-171">Das [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute)-Attribut weist MVC an, den Wert des To-Do-Elements aus dem Text der HTTP-Anforderung abzurufen.</span><span class="sxs-lookup"><span data-stu-id="5b966-171">The [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="5b966-172">Die `CreatedAtRoute`-Methode gibt die Antwort „201“ zurück, bei der es sich um die Standardantwort für eine HTTP-POST-Methode handelt, die eine neue Ressource auf dem Server erstellt.</span><span class="sxs-lookup"><span data-stu-id="5b966-172">The `CreatedAtRoute` method returns a 201 response, which is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="5b966-173">`CreatedAtRoute` fügt der Antwort außerdem einen Adressheader hinzu.</span><span class="sxs-lookup"><span data-stu-id="5b966-173">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="5b966-174">Der Adressheader gibt den URI des neu erstellten To-Do-Elements zurück.</span><span class="sxs-lookup"><span data-stu-id="5b966-174">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="5b966-175">Siehe [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="5b966-175">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="5b966-176">Verwenden von Postman zum Senden einer Erstellungsanforderung</span><span class="sxs-lookup"><span data-stu-id="5b966-176">Use Postman to send a Create request</span></span>

* <span data-ttu-id="5b966-177">Starten Sie die App (**Ausführen > Mit Debugging starten**).</span><span class="sxs-lookup"><span data-stu-id="5b966-177">Start the app (**Run > Start With Debugging**).</span></span>
* <span data-ttu-id="5b966-178">Starten Sie Postman.</span><span class="sxs-lookup"><span data-stu-id="5b966-178">Start Postman.</span></span>

![Postman-Konsole](first-web-api/_static/pmc.png)

* <span data-ttu-id="5b966-180">Legen Sie die HTTP-Methode auf `POST` fest.</span><span class="sxs-lookup"><span data-stu-id="5b966-180">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="5b966-181">Wählen Sie das Optionsfeld **Body** aus.</span><span class="sxs-lookup"><span data-stu-id="5b966-181">Select the **Body** radio button</span></span>
* <span data-ttu-id="5b966-182">Wählen Sie das Optionsfeld **raw** aus.</span><span class="sxs-lookup"><span data-stu-id="5b966-182">Select the **raw** radio button</span></span>
* <span data-ttu-id="5b966-183">Legen Sie den Typ auf „JSON“ fest.</span><span class="sxs-lookup"><span data-stu-id="5b966-183">Set the type to JSON</span></span>
* <span data-ttu-id="5b966-184">Geben Sie im Schlüsselwert-Editor zum Beispiel folgendes To-Do-Element ein</span><span class="sxs-lookup"><span data-stu-id="5b966-184">In the key-value editor, enter a Todo item such as</span></span>

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="5b966-185">Klicken Sie auf **Send**.</span><span class="sxs-lookup"><span data-stu-id="5b966-185">Select **Send**</span></span>

* <span data-ttu-id="5b966-186">Klicken Sie auf die Registerkarte „Header“ im unteren Bereich, und kopieren Sie den Header **Location**:</span><span class="sxs-lookup"><span data-stu-id="5b966-186">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Registerkarte „Header“ in der Postman-Konsole](first-web-api/_static/pmget.png)

<span data-ttu-id="5b966-188">Sie können den Adressheader-URI verwenden, um auf die Ressource zuzugreifen, die Sie gerade erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="5b966-188">You can use the Location header URI to access the resource you just created.</span></span> <span data-ttu-id="5b966-189">Denken Sie daran, dass die `GetById`-Methode die benannte Route `"GetTodo"` erstellt hat:</span><span class="sxs-lookup"><span data-stu-id="5b966-189">Recall the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(string id)
```

### <a name="update"></a><span data-ttu-id="5b966-190">Update</span><span class="sxs-lookup"><span data-stu-id="5b966-190">Update</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="5b966-191">`Update` ähnelt `Create`, verwendet aber HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="5b966-191">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="5b966-192">Die Antwort ist [204 (Kein Inhalt)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="5b966-192">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="5b966-193">Gemäß der HTTP-Spezifikation erfordert eine PUT-Anforderung, dass der Client die gesamte aktualisierte Entität und nicht nur die Deltas sendet.</span><span class="sxs-lookup"><span data-stu-id="5b966-193">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="5b966-194">Verwenden Sie HTTP PATCH, um Teilupdates zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="5b966-194">To support partial updates, use HTTP PATCH.</span></span>

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Postman-Konsole mit der Antwort „204 (Kein Inhalt)“](first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="5b966-196">Löschen</span><span class="sxs-lookup"><span data-stu-id="5b966-196">Delete</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="5b966-197">Die Antwort ist [204 (Kein Inhalt)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="5b966-197">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![Postman-Konsole mit der Antwort „204 (Kein Inhalt)“](first-web-api/_static/pmd.png)

## <a name="next-steps"></a><span data-ttu-id="5b966-199">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="5b966-199">Next steps</span></span>

* [<span data-ttu-id="5b966-200">Routing to Controller Actions (Routing zu Controlleraktionen)</span><span class="sxs-lookup"><span data-stu-id="5b966-200">Routing to Controller Actions</span></span>](xref:mvc/controllers/routing)
* <span data-ttu-id="5b966-201">Weitere Informationen zum Bereitstellen Ihrer API finden Sie unter [Host and deploy (Hosten und Bereitstellen)](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="5b966-201">For information about deploying your API, see [Host and deploy](xref:host-and-deploy/index).</span></span>
* <span data-ttu-id="5b966-202">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5b966-202">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
* [<span data-ttu-id="5b966-203">Postman</span><span class="sxs-lookup"><span data-stu-id="5b966-203">Postman</span></span>](https://www.getpostman.com/)
* [<span data-ttu-id="5b966-204">Fiddler</span><span class="sxs-lookup"><span data-stu-id="5b966-204">Fiddler</span></span>](https://www.telerik.com/download/fiddler)
