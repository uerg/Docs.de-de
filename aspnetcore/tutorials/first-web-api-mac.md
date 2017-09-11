---
title: "Erstellen einer Web-API mit ASP.NET Core und Visual Studio für Mac"
author: rick-anderson
description: "Erstellen einer Web-API mit ASP.NET Core MVC und Visual Studio für Mac"
keywords: ASP.NET Core, WebAPI, Web-API, REST, Mac, macOS, HTTP, Service, HTTP-Dienst
ms.author: riande
manager: wpickett
ms.date: 5/24/2017
ms.topic: get-started-article
ms.assetid: 830b4af5-ed14-1638-7734-764a6f13a8f6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-web-api-mac
ms.openlocfilehash: 08619d3b4ab2d6fdb04794dcbafac0b696dd8504
ms.sourcegitcommit: 3273675dad5ac3e1dc1c589938b73db3f7d6660a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2017
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="2c2f5-104">Erstellen einer Web-API mit ASP.NET Core MVC und Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="2c2f5-104">Create a Web API with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="2c2f5-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT) und [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="2c2f5-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="2c2f5-106">In diesem Tutorial erstellen Sie eine Web-API zum Verwalten einer Liste von „To-Do“-Elementen.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-106">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="2c2f5-107">Sie werden keine Benutzeroberfläche erstellen.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-107">You won’t build a UI.</span></span>

<span data-ttu-id="2c2f5-108">Es gibt drei Versionen von diesem Tutorial:</span><span class="sxs-lookup"><span data-stu-id="2c2f5-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="2c2f5-109">macOS: Web-API mit Visual Studio für Mac (dieses Tutorial)</span><span class="sxs-lookup"><span data-stu-id="2c2f5-109">macOS: Web API with Visual Studio for Mac (This tutorial)</span></span>
* <span data-ttu-id="2c2f5-110">Windows: [Web-API mit Visual Studio für Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="2c2f5-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>
* <span data-ttu-id="2c2f5-111">macOS, Linux, Windows: [Web-API mit Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="2c2f5-111">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

* <span data-ttu-id="2c2f5-112">Ein Beispiel für die Verwendung einer beständigen Datenbank finden Sie unter [Einführung in ASP.NET Core MVC unter Mac oder Linux](xref:tutorials/first-mvc-app-xplat/index).</span><span class="sxs-lookup"><span data-stu-id="2c2f5-112">See [Introduction to ASP.NET Core MVC on Mac or Linux](xref:tutorials/first-mvc-app-xplat/index) for an example that uses a persistent database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2c2f5-113">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="2c2f5-113">Prerequisites</span></span>

<span data-ttu-id="2c2f5-114">Installieren Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="2c2f5-114">Install the following:</span></span>

- [<span data-ttu-id="2c2f5-115">.NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="2c2f5-115">.NET Core SDK</span></span>](https://www.microsoft.com/net/core#macos)  
- [<span data-ttu-id="2c2f5-116">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="2c2f5-116">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-the-project"></a><span data-ttu-id="2c2f5-117">Erstellen eines Projekts</span><span class="sxs-lookup"><span data-stu-id="2c2f5-117">Create the project</span></span>

<span data-ttu-id="2c2f5-118">Klicken Sie in Visual Studio auf **Datei > Neue Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-118">From Visual Studio, select **File > New Solution**.</span></span>

![Neue Projektmappe in macOS](first-web-api-mac/_static/sln.png)

<span data-ttu-id="2c2f5-120">Klicken Sie auf **.NET Core-App > ASP.NET Core-Web-API > Weiter**.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-120">Select **.NET Core App >  ASP.NET Core Web API > Next**.</span></span>

![Dialogfeld „Neue Projektmappe“ in macOS](first-web-api-mac/_static/1.png)

<span data-ttu-id="2c2f5-122">Geben Sie **TodoApi** als **Projektnamen** ein, und klicken Sie dann auf „Erstellen“.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-122">Enter **TodoApi** for the **Project Name**, and then select Create.</span></span>

![Dialogfeld „Konfiguration“](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="2c2f5-124">Starten der App</span><span class="sxs-lookup"><span data-stu-id="2c2f5-124">Launch the app</span></span>

<span data-ttu-id="2c2f5-125">Klicken Sie in Visual Studio auf **Ausführen > Mit Debugging starten**, um die App zu starten.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-125">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="2c2f5-126">Visual Studio startet dann einen Browser und navigiert zu `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-126">Visual Studio launches a browser and navigates to `http://localhost:5000`.</span></span> <span data-ttu-id="2c2f5-127">Ihnen wird der HTTP-Fehler „404 (Not Found)“ angezeigt.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-127">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="2c2f5-128">Ändern Sie die URL zu `http://localhost:port/api/values`.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-128">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="2c2f5-129">Die `ValuesController`-Daten werden angezeigt:</span><span class="sxs-lookup"><span data-stu-id="2c2f5-129">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="2c2f5-130">Hinzufügen der Unterstützung für Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="2c2f5-130">Add support for Entity Framework Core</span></span>

<span data-ttu-id="2c2f5-131">Installieren Sie den Datenbankanbieter [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/).</span><span class="sxs-lookup"><span data-stu-id="2c2f5-131">Install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="2c2f5-132">Dieser Datenbankanbieter ermöglicht, dass Entity Framework Core mit einer In-Memory Database verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

* <span data-ttu-id="2c2f5-133">Klicken Sie im Menü **Projekt** auf **NuGet-Pakete hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-133">From the **Project** menu, select **Add NuGet Packages**.</span></span> 

  *  <span data-ttu-id="2c2f5-134">Sie können alternativ mit der rechten Maustaste auf **Abhängigkeiten** und dann auf **Pakete hinzufügen** klicken.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-134">Alternately, you can right-click **Dependencies**, and then select **Add Packages**.</span></span>

* <span data-ttu-id="2c2f5-135">Geben Sie `EntityFrameworkCore.InMemory` in das Suchfeld ein.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-135">Enter `EntityFrameworkCore.InMemory` in the search box.</span></span>
* <span data-ttu-id="2c2f5-136">Wählen Sie `Microsoft.EntityFrameworkCore.InMemory` aus, und klicken Sie dann auf **Pakete hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-136">Select `Microsoft.EntityFrameworkCore.InMemory`, and then select **Add Package**.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="2c2f5-137">Hinzufügen einer Modellklasse</span><span class="sxs-lookup"><span data-stu-id="2c2f5-137">Add a model class</span></span>

<span data-ttu-id="2c2f5-138">Ein Modell ist ein Objekt, das die Daten in Ihrer Anwendung darstellt.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-138">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="2c2f5-139">In diesem Fall ist das einzige Modell ein To-Do-Element.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-139">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="2c2f5-140">Fügen Sie einen Ordner namens *Modelle* hinzu.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-140">Add a folder named *Models*.</span></span> <span data-ttu-id="2c2f5-141">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-141">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="2c2f5-142">Klicken Sie auf **Hinzufügen** > **Neuer Ordner**.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-142">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="2c2f5-143">Geben Sie dem Ordner den Namen *Modelle*.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-143">Name the folder *Models*.</span></span>

![Neuer Ordner](first-web-api-mac/_static/folder.png)

<span data-ttu-id="2c2f5-145">Hinweis: Sie können Modellklassen überall in Ihrem Projekt unterbringen, aber der Ordner *Modelle* wird gemäß der Konvention verwendet.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-145">Note: You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="2c2f5-146">Fügen Sie eine `TodoItem`-Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-146">Add a `TodoItem` class.</span></span> <span data-ttu-id="2c2f5-147">Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle*, und klicken Sie auf **Hinzufügen > Neue Datei > Allgemein > Leere Klasse**.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-147">Right-click the *Models* folder and select **Add > New File > General > Empty Class**.</span></span> <span data-ttu-id="2c2f5-148">Benennen Sie die Klasse `TodoItem`, und klicken Sie dann auf **Neu**.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-148">Name the class `TodoItem`, and then select **New**.</span></span>

<span data-ttu-id="2c2f5-149">Ersetzen Sie den generierten Code mit Folgendem:</span><span class="sxs-lookup"><span data-stu-id="2c2f5-149">Replace the generated code with:</span></span>

<span data-ttu-id="2c2f5-150">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]</span><span class="sxs-lookup"><span data-stu-id="2c2f5-150">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]</span></span>

<span data-ttu-id="2c2f5-151">Die Datenbank generiert die `Id`, wenn ein `TodoItem` erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-151">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="2c2f5-152">Erstellen des Datenbankkontexts</span><span class="sxs-lookup"><span data-stu-id="2c2f5-152">Create the database context</span></span>

<span data-ttu-id="2c2f5-153">Der *Datenbankkontext* ist die Hauptklasse, die die Entity Framework-Funktionen für ein angegebenes Datenmodell koordiniert.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-153">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="2c2f5-154">Sie können diese Klasse durch Ableiten von der `Microsoft.EntityFrameworkCore.DbContext`-Klasse erstellen.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-154">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="2c2f5-155">Fügen Sie eine `TodoContext`-Klasse zum Ordner *Modelle* hinzu.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-155">Add a `TodoContext` class to the *Models* folder.</span></span>

<span data-ttu-id="2c2f5-156">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="2c2f5-156">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]</span></span>

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="2c2f5-157">Hinzufügen eines Controllers</span><span class="sxs-lookup"><span data-stu-id="2c2f5-157">Add a controller</span></span>

<span data-ttu-id="2c2f5-158">Fügen Sie im Ordner *Controller* im Projektmappen-Explorer die Klasse `TodoController` hinzu.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-158">In Solution Explorer, in the *Controllers* folder, add the class `TodoController`.</span></span>

<span data-ttu-id="2c2f5-159">Ersetzen Sie den generierten Code mit Folgendem (und fügen Sie schließende Klammern hinzu):</span><span class="sxs-lookup"><span data-stu-id="2c2f5-159">Replace the generated code with the following (and add closing braces):</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="2c2f5-160">Starten der App</span><span class="sxs-lookup"><span data-stu-id="2c2f5-160">Launch the app</span></span>

<span data-ttu-id="2c2f5-161">Klicken Sie in Visual Studio auf **Ausführen > Mit Debugging starten**, um die App zu starten.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-161">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="2c2f5-162">Visual Studio startet einen Browser und navigiert zu `http://localhost:port`, wobei *port* eine zufällig ausgewählte Portnummer ist.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-162">Visual Studio launches a browser and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="2c2f5-163">Ihnen wird der HTTP-Fehler „404 (Not Found)“ angezeigt.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-163">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="2c2f5-164">Ändern Sie die URL zu `http://localhost:port/api/values`.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-164">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="2c2f5-165">Die `ValuesController`-Daten werden angezeigt:</span><span class="sxs-lookup"><span data-stu-id="2c2f5-165">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

<span data-ttu-id="2c2f5-166">Navigieren Sie zum `Todo`-Controller auf `http://localhost:port/api/todo`:</span><span class="sxs-lookup"><span data-stu-id="2c2f5-166">Navigate to the `Todo` controller at`http://localhost:port/api/todo`:</span></span>

```
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="2c2f5-167">Implementieren der anderen CRUD-Vorgänge</span><span class="sxs-lookup"><span data-stu-id="2c2f5-167">Implement the other CRUD operations</span></span>

<span data-ttu-id="2c2f5-168">Fügen Sie `Create`-, `Update`- und `Delete`-Methoden zum Controller hinzu.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-168">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="2c2f5-169">Dabei handelt sich um Variationen eines Designs, darum wird der Code mit den hervorgehobenen Hauptunterschieden dargestellt.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-169">These are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="2c2f5-170">Erstellen Sie das Projekt, nachdem Sie Code hinzugefügt oder geändert haben.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-170">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="2c2f5-171">Erstellen</span><span class="sxs-lookup"><span data-stu-id="2c2f5-171">Create</span></span>

<span data-ttu-id="2c2f5-172">[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="2c2f5-172">[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span></span>

<span data-ttu-id="2c2f5-173">Dies ist eine HTTP-POST-Methode, angegeben durch das [`[HttpPost]`](https://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Mvc/HttpPostAttribute/index.html)-Attribut.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-173">This is an HTTP POST method, indicated by the [`[HttpPost]`](https://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Mvc/HttpPostAttribute/index.html) attribute.</span></span> <span data-ttu-id="2c2f5-174">Das [`[FromBody]`](https://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Mvc/FromBodyAttribute/index.html)-Attribut weist MVC an, den Wert des To-Do-Elements aus dem Text der HTTP-Anforderung abzurufen.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-174">The [`[FromBody]`](https://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Mvc/FromBodyAttribute/index.html) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="2c2f5-175">Die `CreatedAtRoute`-Methode gibt die Antwort „201“ zurück, bei der es sich um die Standardantwort für eine HTTP-POST-Methode handelt, die eine neue Ressource auf dem Server erstellt.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-175">The `CreatedAtRoute` method returns a 201 response, which is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="2c2f5-176">`CreatedAtRoute` fügt der Antwort außerdem einen Adressheader hinzu.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-176">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="2c2f5-177">Der Adressheader gibt die URI des neu erstellten To-Do-Elements zurück.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-177">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="2c2f5-178">Siehe [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="2c2f5-178">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="2c2f5-179">Verwenden von Postman zum Senden einer Erstellungsanforderung</span><span class="sxs-lookup"><span data-stu-id="2c2f5-179">Use Postman to send a Create request</span></span>

* <span data-ttu-id="2c2f5-180">Starten Sie die App (**Ausführen > Mit Debugging starten**).</span><span class="sxs-lookup"><span data-stu-id="2c2f5-180">Start the app (**Run > Start With Debugging**).</span></span>
* <span data-ttu-id="2c2f5-181">Starten Sie Postman.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-181">Start Postman.</span></span>

![Postman-Konsole](first-web-api/_static/pmc.png)

* <span data-ttu-id="2c2f5-183">Legen Sie die HTTP-Methode auf `POST` fest.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-183">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="2c2f5-184">Wählen Sie das Optionsfeld **Text** aus.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-184">Select the **Body** radio button</span></span>
* <span data-ttu-id="2c2f5-185">Wählen Sie das Optionsfeld **Raw** aus.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-185">Select the **raw** radio button</span></span>
* <span data-ttu-id="2c2f5-186">Legen Sie den Typ auf JSON fest.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-186">Set the type to JSON</span></span>
* <span data-ttu-id="2c2f5-187">Geben Sie im Schlüsselwert-Editor zum Beispiel folgendes To-Do-Element ein</span><span class="sxs-lookup"><span data-stu-id="2c2f5-187">In the key-value editor, enter a Todo item such as</span></span>

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="2c2f5-188">Klicken Sie auf **Senden**.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-188">Select **Send**</span></span>

* <span data-ttu-id="2c2f5-189">Klicken Sie auf die Registerkarte „Header“ im unteren Bereich, und kopieren Sie den **Adressheader**:</span><span class="sxs-lookup"><span data-stu-id="2c2f5-189">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Registerkarte „Header“ in der Postman-Konsole](first-web-api/_static/pmget.png)

<span data-ttu-id="2c2f5-191">Sie können die Adressheader-URI verwenden, um auf die Ressource zuzugreifen, die Sie gerade erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-191">You can use the Location header URI to access the resource you just created.</span></span> <span data-ttu-id="2c2f5-192">Denken Sie daran, dass die `GetById`-Methode die benannte Route `"GetTodo"` erstellt hat:</span><span class="sxs-lookup"><span data-stu-id="2c2f5-192">Recall the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(string id)
```

### <a name="update"></a><span data-ttu-id="2c2f5-193">Aktualisieren</span><span class="sxs-lookup"><span data-stu-id="2c2f5-193">Update</span></span>

<span data-ttu-id="2c2f5-194">[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span><span class="sxs-lookup"><span data-stu-id="2c2f5-194">[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span></span>

<span data-ttu-id="2c2f5-195">`Update` ähnelt `Create`, verwendet aber HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-195">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="2c2f5-196">Die Antwort ist [204 (Kein Inhalt)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="2c2f5-196">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="2c2f5-197">Gemäß der HTTP-Spezifikation erfordert eine PUT-Anforderung, dass der Client die gesamte aktualisierte Entität und nicht nur die Deltas sendet.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-197">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="2c2f5-198">Verwenden Sie HTTP PATCH, um Teilupdates zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="2c2f5-198">To support partial updates, use HTTP PATCH.</span></span>

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Postman-Konsole, die die Antwort „204 (Kein Inhalt)“ anzeigt.](first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="2c2f5-200">Löschen</span><span class="sxs-lookup"><span data-stu-id="2c2f5-200">Delete</span></span>

<span data-ttu-id="2c2f5-201">[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]</span><span class="sxs-lookup"><span data-stu-id="2c2f5-201">[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]</span></span>

<span data-ttu-id="2c2f5-202">Die Antwort ist [204 (Kein Inhalt)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="2c2f5-202">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![Postman-Konsole, die die Antwort „204 (Kein Inhalt)“ anzeigt.](first-web-api/_static/pmd.png)

## <a name="next-steps"></a><span data-ttu-id="2c2f5-204">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="2c2f5-204">Next steps</span></span>

* [<span data-ttu-id="2c2f5-205">Routing to Controller Actions (Routing zu Controlleraktionen)</span><span class="sxs-lookup"><span data-stu-id="2c2f5-205">Routing to Controller Actions</span></span>](xref:mvc/controllers/routing)
* <span data-ttu-id="2c2f5-206">Weitere Informationen zum Bereitstellen Ihrer API finden Sie unter [Publishing and Deployment (Veröffentlichung und Bereitstellung)](../publishing/index.md).</span><span class="sxs-lookup"><span data-stu-id="2c2f5-206">For information about deploying your API, see [Publishing and Deployment](../publishing/index.md).</span></span>
* [<span data-ttu-id="2c2f5-207">Anzeigen oder Herunterladen von Beispielcode</span><span class="sxs-lookup"><span data-stu-id="2c2f5-207">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample)
* [<span data-ttu-id="2c2f5-208">Postman</span><span class="sxs-lookup"><span data-stu-id="2c2f5-208">Postman</span></span>](https://www.getpostman.com/)
* [<span data-ttu-id="2c2f5-209">Fiddler</span><span class="sxs-lookup"><span data-stu-id="2c2f5-209">Fiddler</span></span>](http://www.fiddler2.com/fiddler2/)
