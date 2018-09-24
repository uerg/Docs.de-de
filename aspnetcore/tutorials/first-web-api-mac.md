---
title: Erstellen einer Web-API mit ASP.NET Core und Visual Studio für Mac
author: rick-anderson
description: Erstellen einer Web-API mit ASP.NET Core MVC und Visual Studio für Mac
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2018
uid: tutorials/first-web-api-mac
ms.openlocfilehash: 40f9bd9c57b97826edfddeb00cb4fb38a026d46e
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011624"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-mac"></a><span data-ttu-id="b47c2-103">Erstellen einer Web-API mit ASP.NET Core und Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="b47c2-103">Create a Web API with ASP.NET Core and Visual Studio for Mac</span></span>

<span data-ttu-id="b47c2-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT) und [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="b47c2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="b47c2-105">In diesem Tutorial wird eine Web-API zum Verwalten einer Liste von Aufgabenelementen erstellt.</span><span class="sxs-lookup"><span data-stu-id="b47c2-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="b47c2-106">Die Benutzeroberfläche wird nicht erstellt.</span><span class="sxs-lookup"><span data-stu-id="b47c2-106">The UI isn't constructed.</span></span>

<span data-ttu-id="b47c2-107">Es gibt drei Versionen dieses Tutorials:</span><span class="sxs-lookup"><span data-stu-id="b47c2-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="b47c2-108">macOS: Web-API mit Visual Studio für Mac (dieses Tutorial)</span><span class="sxs-lookup"><span data-stu-id="b47c2-108">macOS: Web API with Visual Studio for Mac (This tutorial)</span></span>
* <span data-ttu-id="b47c2-109">Windows: [Web-API mit Visual Studio für Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="b47c2-109">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>
* <span data-ttu-id="b47c2-110">macOS, Linux, Windows: [Web-API mit Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="b47c2-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

<span data-ttu-id="b47c2-111">Ein Beispiel für die Verwendung einer beständigen Datenbank finden Sie unter [Einführung in ASP.NET Core MVC unter macOS oder Linux](xref:tutorials/first-mvc-app-xplat/index).</span><span class="sxs-lookup"><span data-stu-id="b47c2-111">See [Introduction to ASP.NET Core MVC on macOS or Linux](xref:tutorials/first-mvc-app-xplat/index) for an example that uses a persistent database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b47c2-112">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="b47c2-112">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-macos.md)]

## <a name="create-the-project"></a><span data-ttu-id="b47c2-113">Erstellen eines Projekts</span><span class="sxs-lookup"><span data-stu-id="b47c2-113">Create the project</span></span>

<span data-ttu-id="b47c2-114">Klicken Sie in Visual Studio auf **Datei** > **Neue Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="b47c2-114">From Visual Studio, select **File** > **New Solution**.</span></span>

![Neue Projektmappe in macOS](first-web-api-mac/_static/sln.png)

<span data-ttu-id="b47c2-116">Klicken Sie auf **.NET Core-App** > **ASP.NET Core-Web-API** > **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="b47c2-116">Select **.NET Core App** > **ASP.NET Core Web API** > **Next**.</span></span>

![Dialogfeld „Neue Projektmappe“ in macOS](first-web-api-mac/_static/1.png)

<span data-ttu-id="b47c2-118">Geben Sie *TodoApi* als **Projektnamen** ein, und klicken Sie dann auf **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="b47c2-118">Enter *TodoApi* for the **Project Name**, and then click **Create**.</span></span>

![Dialogfeld „Konfiguration“](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="b47c2-120">Starten der App</span><span class="sxs-lookup"><span data-stu-id="b47c2-120">Launch the app</span></span>

<span data-ttu-id="b47c2-121">Klicken Sie in Visual Studio auf **Ausführen** > **Mit Debugging starten**, um die App zu starten.</span><span class="sxs-lookup"><span data-stu-id="b47c2-121">In Visual Studio, select **Run** > **Start With Debugging** to launch the app.</span></span> <span data-ttu-id="b47c2-122">Visual Studio startet dann einen Browser und navigiert zu `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="b47c2-122">Visual Studio launches a browser and navigates to `http://localhost:5000`.</span></span> <span data-ttu-id="b47c2-123">Ihnen wird der HTTP-Fehler „404 (Not Found)“ angezeigt.</span><span class="sxs-lookup"><span data-stu-id="b47c2-123">You get an HTTP 404 (Not Found) error.</span></span> <span data-ttu-id="b47c2-124">Ändern Sie die URL zu `http://localhost:<port>/api/values`.</span><span class="sxs-lookup"><span data-stu-id="b47c2-124">Change the URL to `http://localhost:<port>/api/values`.</span></span> <span data-ttu-id="b47c2-125">Die Daten von `ValuesController` werden angezeigt:</span><span class="sxs-lookup"><span data-stu-id="b47c2-125">The `ValuesController` data is displayed:</span></span>

```json
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="b47c2-126">Hinzufügen der Unterstützung für Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="b47c2-126">Add support for Entity Framework Core</span></span>

<span data-ttu-id="b47c2-127">Installieren Sie den Datenbankanbieter [Entity Framework Core InMemory](/ef/core/providers/in-memory/).</span><span class="sxs-lookup"><span data-stu-id="b47c2-127">Install the [Entity Framework Core InMemory](/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="b47c2-128">Dieser Datenbankanbieter ermöglicht, dass Entity Framework Core mit einer In-Memory Database verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="b47c2-128">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

* <span data-ttu-id="b47c2-129">Klicken Sie im Menü **Projekt** auf **NuGet-Pakete hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="b47c2-129">From the **Project** menu, select **Add NuGet Packages**.</span></span>

  * <span data-ttu-id="b47c2-130">Sie können alternativ mit der rechten Maustaste auf **Abhängigkeiten** und dann auf **Pakete hinzufügen** klicken.</span><span class="sxs-lookup"><span data-stu-id="b47c2-130">Alternatively, you can right-click **Dependencies**, and then select **Add Packages**.</span></span>

* <span data-ttu-id="b47c2-131">Geben Sie `EntityFrameworkCore.InMemory` in das Suchfeld ein.</span><span class="sxs-lookup"><span data-stu-id="b47c2-131">Enter `EntityFrameworkCore.InMemory` in the search box.</span></span>
* <span data-ttu-id="b47c2-132">Wählen Sie `Microsoft.EntityFrameworkCore.InMemory` aus, und klicken Sie dann auf **Pakete hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="b47c2-132">Select `Microsoft.EntityFrameworkCore.InMemory`, and then select **Add Package**.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="b47c2-133">Hinzufügen einer Modellklasse</span><span class="sxs-lookup"><span data-stu-id="b47c2-133">Add a model class</span></span>

<span data-ttu-id="b47c2-134">Ein Modell ist ein Objekt, das die Daten in Ihrer App darstellt.</span><span class="sxs-lookup"><span data-stu-id="b47c2-134">A model is an object representing the data in your app.</span></span> <span data-ttu-id="b47c2-135">In diesem Fall ist das einzige Modell ein To-do-Element.</span><span class="sxs-lookup"><span data-stu-id="b47c2-135">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="b47c2-136">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen.</span><span class="sxs-lookup"><span data-stu-id="b47c2-136">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="b47c2-137">Klicken Sie auf **Hinzufügen** > **Neuer Ordner**.</span><span class="sxs-lookup"><span data-stu-id="b47c2-137">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="b47c2-138">Geben Sie dem Ordner den Namen *Modelle*.</span><span class="sxs-lookup"><span data-stu-id="b47c2-138">Name the folder *Models*.</span></span>

![Neuer Ordner](first-web-api-mac/_static/folder.png)

> [!NOTE]
> <span data-ttu-id="b47c2-140">Hinweis: Sie können Modellklassen überall in Ihrem Projekt unterbringen, aber gemäß Konvention wird der Ordner *Models* verwendet.</span><span class="sxs-lookup"><span data-stu-id="b47c2-140">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="b47c2-141">Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle*, und klicken Sie auf **Hinzufügen** > **Neue Datei** > **Allgemein** > **Leere Klasse**.</span><span class="sxs-lookup"><span data-stu-id="b47c2-141">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span> <span data-ttu-id="b47c2-142">Nennen Sie die Klasse *TodoItem*, und klicken Sie dann auf **Neu**.</span><span class="sxs-lookup"><span data-stu-id="b47c2-142">Name the class *TodoItem*, and then click **New**.</span></span>

<span data-ttu-id="b47c2-143">Ersetzen Sie den generierten Code mit Folgendem:</span><span class="sxs-lookup"><span data-stu-id="b47c2-143">Replace the generated code with:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="b47c2-144">Die Datenbank generiert die `Id`, wenn ein `TodoItem` erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="b47c2-144">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="b47c2-145">Erstellen des Datenbankkontexts</span><span class="sxs-lookup"><span data-stu-id="b47c2-145">Create the database context</span></span>

<span data-ttu-id="b47c2-146">Der *Datenbankkontext* ist die Hauptklasse, die die Entity Framework-Funktionen für ein angegebenes Datenmodell koordiniert.</span><span class="sxs-lookup"><span data-stu-id="b47c2-146">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="b47c2-147">Sie können diese Klasse durch Ableiten von der `Microsoft.EntityFrameworkCore.DbContext`-Klasse erstellen.</span><span class="sxs-lookup"><span data-stu-id="b47c2-147">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="b47c2-148">Fügen Sie eine `TodoContext`-Klasse zum Ordner *Modelle* hinzu.</span><span class="sxs-lookup"><span data-stu-id="b47c2-148">Add a `TodoContext` class to the *Models* folder.</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="b47c2-149">Hinzufügen eines Controllers</span><span class="sxs-lookup"><span data-stu-id="b47c2-149">Add a controller</span></span>

<span data-ttu-id="b47c2-150">Fügen Sie im Ordner *Controller* im Projektmappen-Explorer die Klasse `TodoController` hinzu.</span><span class="sxs-lookup"><span data-stu-id="b47c2-150">In Solution Explorer, in the *Controllers* folder, add the class `TodoController`.</span></span>

<span data-ttu-id="b47c2-151">Ersetzen Sie den generierten Code durch den folgenden:</span><span class="sxs-lookup"><span data-stu-id="b47c2-151">Replace the generated code with the following:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="b47c2-152">Starten der App</span><span class="sxs-lookup"><span data-stu-id="b47c2-152">Launch the app</span></span>

<span data-ttu-id="b47c2-153">Klicken Sie in Visual Studio auf **Ausführen** > **Mit Debugging starten**, um die App zu starten.</span><span class="sxs-lookup"><span data-stu-id="b47c2-153">In Visual Studio, select **Run** > **Start With Debugging** to launch the app.</span></span> <span data-ttu-id="b47c2-154">Visual Studio startet einen Browser und navigiert zu `http://localhost:<port>`, wobei es sich bei `<port>` um eine zufällig ausgewählte Portnummer handelt.</span><span class="sxs-lookup"><span data-stu-id="b47c2-154">Visual Studio launches a browser and navigates to `http://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="b47c2-155">Ihnen wird der HTTP-Fehler „404 (Not Found)“ angezeigt.</span><span class="sxs-lookup"><span data-stu-id="b47c2-155">You get an HTTP 404 (Not Found) error.</span></span> <span data-ttu-id="b47c2-156">Ändern Sie die URL zu `http://localhost:<port>/api/values`.</span><span class="sxs-lookup"><span data-stu-id="b47c2-156">Change the URL to `http://localhost:<port>/api/values`.</span></span> <span data-ttu-id="b47c2-157">Die Daten von `ValuesController` werden angezeigt:</span><span class="sxs-lookup"><span data-stu-id="b47c2-157">The `ValuesController` data is displayed:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="b47c2-158">Navigieren Sie zum `Todo`-Controller auf `http://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="b47c2-158">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="b47c2-159">Die folgende JSON-Datei wird zurückgegeben:</span><span class="sxs-lookup"><span data-stu-id="b47c2-159">The following JSON is returned:</span></span>

```json
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="b47c2-160">Implementieren der anderen CRUD-Vorgänge</span><span class="sxs-lookup"><span data-stu-id="b47c2-160">Implement the other CRUD operations</span></span>

<span data-ttu-id="b47c2-161">Fügen Sie Methoden `Create`, `Update` und `Delete` zum Controller hinzu.</span><span class="sxs-lookup"><span data-stu-id="b47c2-161">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="b47c2-162">Bei diesen Methoden handelt sich um Variationen eines Designs, darum wird der Code mit den hervorgehobenen Hauptunterschieden dargestellt.</span><span class="sxs-lookup"><span data-stu-id="b47c2-162">These methods are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="b47c2-163">Erstellen Sie das Projekt, nachdem Sie Code hinzugefügt oder geändert haben.</span><span class="sxs-lookup"><span data-stu-id="b47c2-163">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="b47c2-164">Erstellen</span><span class="sxs-lookup"><span data-stu-id="b47c2-164">Create</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="b47c2-165">Die zuvor aufgeführte Methode reagiert auf eine HTTP POST-Anforderung, die vom Attribut [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="b47c2-165">The preceding method responds to an HTTP POST, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="b47c2-166">Das [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)-Attribut weist MVC an, den Wert des To-Do-Elements aus dem Text der HTTP-Anforderung abzurufen.</span><span class="sxs-lookup"><span data-stu-id="b47c2-166">The [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="b47c2-167">Die zuvor aufgeführte Methode reagiert auf eine HTTP POST-Anforderung, die vom Attribut [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="b47c2-167">The preceding method responds to an HTTP POST, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="b47c2-168">MVC ruft den Wert des To-Do-Elements aus dem Text der HTTP-Anforderung ab.</span><span class="sxs-lookup"><span data-stu-id="b47c2-168">MVC gets the value of the to-do item from the body of the HTTP request.</span></span>

::: moniker-end

<span data-ttu-id="b47c2-169">Die `CreatedAtRoute`-Methode gibt eine 201-Antwort zurück.</span><span class="sxs-lookup"><span data-stu-id="b47c2-169">The `CreatedAtRoute` method returns a 201 response.</span></span> <span data-ttu-id="b47c2-170">Dies ist die Standardantwort für eine HTTP POST-Methode, die eine neue Ressource auf dem Server erstellt.</span><span class="sxs-lookup"><span data-stu-id="b47c2-170">It's the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="b47c2-171">`CreatedAtRoute` fügt der Antwort außerdem einen Adressheader hinzu.</span><span class="sxs-lookup"><span data-stu-id="b47c2-171">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="b47c2-172">Der Adressheader gibt den URI des neu erstellten To-Do-Elements zurück.</span><span class="sxs-lookup"><span data-stu-id="b47c2-172">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="b47c2-173">Siehe [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="b47c2-173">See [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="b47c2-174">Verwenden von Postman zum Senden einer Erstellungsanforderung</span><span class="sxs-lookup"><span data-stu-id="b47c2-174">Use Postman to send a Create request</span></span>

* <span data-ttu-id="b47c2-175">Starten Sie die App (**Ausführen** > **Mit Debugging starten**).</span><span class="sxs-lookup"><span data-stu-id="b47c2-175">Start the app (**Run** > **Start With Debugging**).</span></span>
* <span data-ttu-id="b47c2-176">Öffnen Sie Postman.</span><span class="sxs-lookup"><span data-stu-id="b47c2-176">Open Postman.</span></span>

![Postman-Konsole](first-web-api/_static/pmc.png)

* <span data-ttu-id="b47c2-178">Aktualisieren Sie die Portnummer in der localhost-URL.</span><span class="sxs-lookup"><span data-stu-id="b47c2-178">Update the port number in the localhost URL.</span></span>
* <span data-ttu-id="b47c2-179">Legen Sie die HTTP-Methode auf *POST* fest.</span><span class="sxs-lookup"><span data-stu-id="b47c2-179">Set the HTTP method to *POST*.</span></span>
* <span data-ttu-id="b47c2-180">Klicken Sie auf die Registerkarte **Body** (Text).</span><span class="sxs-lookup"><span data-stu-id="b47c2-180">Click the **Body** tab.</span></span>
* <span data-ttu-id="b47c2-181">Wählen Sie das Optionsfeld **raw** (Unformatiert) aus.</span><span class="sxs-lookup"><span data-stu-id="b47c2-181">Select the **raw** radio button.</span></span>
* <span data-ttu-id="b47c2-182">Legen Sie den Typ auf *JSON (application/json)* fest.</span><span class="sxs-lookup"><span data-stu-id="b47c2-182">Set the type to *JSON (application/json)*.</span></span>
* <span data-ttu-id="b47c2-183">Geben Sie einen Anforderungstext mit einem To-Do-Element ein, der folgendem JSON-Code ähnelt:</span><span class="sxs-lookup"><span data-stu-id="b47c2-183">Enter a request body with a to-do item resembling the following JSON:</span></span>

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* <span data-ttu-id="b47c2-184">Klicken Sie auf die Schaltfläche **Senden**.</span><span class="sxs-lookup"><span data-stu-id="b47c2-184">Click the **Send** button.</span></span>

::: moniker range=">= aspnetcore-2.1"

> [!TIP]
> <span data-ttu-id="b47c2-185">Wenn nach dem Klicken auf **Senden** keine Antwort angezeigt wird, deaktivieren Sie die Option **SSL certification verification** (Überprüfung der SSL-Zertifizierung).</span><span class="sxs-lookup"><span data-stu-id="b47c2-185">If no response displays after clicking **Send**, disable the **SSL certification verification** option.</span></span> <span data-ttu-id="b47c2-186">Diese finden Sie unter **Datei** > **Einstellungen**.</span><span class="sxs-lookup"><span data-stu-id="b47c2-186">This is found under **File** > **Settings**.</span></span> <span data-ttu-id="b47c2-187">Klicken Sie erneut auf die Schaltfläche **Senden**, nachdem Sie diese Einstellung deaktiviert haben.</span><span class="sxs-lookup"><span data-stu-id="b47c2-187">Click the **Send** button again after disabling the setting.</span></span>

::: moniker-end

<span data-ttu-id="b47c2-188">Klicken Sie auf die Registerkarte **Header** im Bereich **Antwort**, und kopieren Sie den Headerwert von **Location** (Speicherort):</span><span class="sxs-lookup"><span data-stu-id="b47c2-188">Click the **Headers** tab in the **Response** pane and copy the **Location** header value:</span></span>

![Registerkarte „Header“ in der Postman-Konsole](first-web-api/_static/pmc2.png)

<span data-ttu-id="b47c2-190">Sie können den Adressheader-URI verwenden, um auf die Ressource zuzugreifen, die Sie erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="b47c2-190">You can use the Location header URI to access the resource you created.</span></span> <span data-ttu-id="b47c2-191">Die `Create`-Methode gibt [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_) zurück.</span><span class="sxs-lookup"><span data-stu-id="b47c2-191">The `Create` method returns [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_).</span></span> <span data-ttu-id="b47c2-192">Der erste Parameter, der an `CreatedAtRoute` übergeben wird, stellt die benannte Route dar, die für das Generieren der URL verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="b47c2-192">The first parameter passed to `CreatedAtRoute` represents the named route to use for generating the URL.</span></span> <span data-ttu-id="b47c2-193">Bedenken Sie, dass die `GetById`-Methode die benannte Route `"GetTodo"` erstellt hat:</span><span class="sxs-lookup"><span data-stu-id="b47c2-193">Recall that the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
```

### <a name="update"></a><span data-ttu-id="b47c2-194">Update</span><span class="sxs-lookup"><span data-stu-id="b47c2-194">Update</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

::: moniker-end

<span data-ttu-id="b47c2-195">`Update` ähnelt `Create`, verwendet aber HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="b47c2-195">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="b47c2-196">Die Antwort ist [204 (Kein Inhalt)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="b47c2-196">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="b47c2-197">Gemäß der HTTP-Spezifikation erfordert eine PUT-Anforderung, dass der Client die gesamte aktualisierte Entität und nicht nur die Deltas sendet.</span><span class="sxs-lookup"><span data-stu-id="b47c2-197">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="b47c2-198">Verwenden Sie HTTP PATCH, um Teilupdates zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="b47c2-198">To support partial updates, use HTTP PATCH.</span></span>

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Postman-Konsole mit der Antwort „204 (Kein Inhalt)“](first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="b47c2-200">Löschen</span><span class="sxs-lookup"><span data-stu-id="b47c2-200">Delete</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="b47c2-201">Die Antwort ist [204 (Kein Inhalt)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="b47c2-201">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![Postman-Konsole mit der Antwort „204 (Kein Inhalt)“](first-web-api/_static/pmd.png)

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
