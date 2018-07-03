---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: Aktivieren von CRUD-Vorgänge in ASP.NET Web-API 1 | Microsoft-Dokumentation
author: MikeWasson
description: In diesem Tutorial wird gezeigt, wie CRUD-Vorgänge in einen HTTP-Dienst über ASP.NET Web-API unterstützt. Die Softwareversionen, die in den Tutorials Visual Studio 2012 Web Zugriffspunkt verwendet...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/28/2012
ms.topic: article
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: 1658e120225cd3c9425168238981133c96ff398a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402927"
---
<a name="enabling-crud-operations-in-aspnet-web-api-1"></a><span data-ttu-id="e2355-104">Aktivieren von CRUD-Vorgänge in ASP.NET Web-API 1</span><span class="sxs-lookup"><span data-stu-id="e2355-104">Enabling CRUD Operations in ASP.NET Web API 1</span></span>
====================
<span data-ttu-id="e2355-105">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e2355-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="e2355-106">Abgeschlossenes Projekt herunterladen</span><span class="sxs-lookup"><span data-stu-id="e2355-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> <span data-ttu-id="e2355-107">In diesem Tutorial wird gezeigt, wie CRUD-Vorgänge in einen HTTP-Dienst über ASP.NET Web-API unterstützt.</span><span class="sxs-lookup"><span data-stu-id="e2355-107">This tutorial shows how to support CRUD operations in an HTTP service using ASP.NET Web API.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="e2355-108">Softwareversionen, die in diesem Tutorial verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="e2355-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="e2355-109">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="e2355-109">Visual Studio 2012</span></span>
> - <span data-ttu-id="e2355-110">Web-API 1 (Außerdem funktioniert mit Web-API 2)</span><span class="sxs-lookup"><span data-stu-id="e2355-110">Web API 1 (also works with Web API 2)</span></span>


<span data-ttu-id="e2355-111">CRUD steht für &quot;erstellen, lesen, aktualisieren und löschen,&quot; die sind die vier grundlegenden Datenbankvorgänge.</span><span class="sxs-lookup"><span data-stu-id="e2355-111">CRUD stands for &quot;Create, Read, Update, and Delete,&quot; which are the four basic database operations.</span></span> <span data-ttu-id="e2355-112">Viele HTTP-Dienste das Modell auch CRUD-Vorgänge über REST oder die REST-ähnliche APIs.</span><span class="sxs-lookup"><span data-stu-id="e2355-112">Many HTTP services also model CRUD operations through REST or REST-like APIs.</span></span>

<span data-ttu-id="e2355-113">In diesem Tutorial erstellen Sie ein sehr einfaches Web-API, um eine Liste von Produkten zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="e2355-113">In this tutorial, you will build a very simple web API to manage a list of products.</span></span> <span data-ttu-id="e2355-114">Jedes Produkt, Preis, und Auswählen einer Kategorie enthält (z. B. &quot;Toys&quot; oder &quot;Hardware&quot;), sowie eine Produkt-ID.</span><span class="sxs-lookup"><span data-stu-id="e2355-114">Each product will contain a name, price, and category (such as &quot;toys&quot; or &quot;hardware&quot;), plus a product ID.</span></span>

<span data-ttu-id="e2355-115">Die API-Produkte werden folgende Methoden verfügbar machen.</span><span class="sxs-lookup"><span data-stu-id="e2355-115">The products API will expose following methods.</span></span>

| <span data-ttu-id="e2355-116">Aktion</span><span class="sxs-lookup"><span data-stu-id="e2355-116">Action</span></span> | <span data-ttu-id="e2355-117">HTTP-Methode</span><span class="sxs-lookup"><span data-stu-id="e2355-117">HTTP method</span></span> | <span data-ttu-id="e2355-118">Relativer URI</span><span class="sxs-lookup"><span data-stu-id="e2355-118">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e2355-119">Abrufen einer Liste aller Produkte</span><span class="sxs-lookup"><span data-stu-id="e2355-119">Get a list of all products</span></span> | <span data-ttu-id="e2355-120">GET</span><span class="sxs-lookup"><span data-stu-id="e2355-120">GET</span></span> | <span data-ttu-id="e2355-121">/api/products</span><span class="sxs-lookup"><span data-stu-id="e2355-121">/api/products</span></span> |
| <span data-ttu-id="e2355-122">Abrufen eines Produkts nach ID</span><span class="sxs-lookup"><span data-stu-id="e2355-122">Get a product by ID</span></span> | <span data-ttu-id="e2355-123">GET</span><span class="sxs-lookup"><span data-stu-id="e2355-123">GET</span></span> | <span data-ttu-id="e2355-124">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="e2355-124">/api/products/*id*</span></span> |
| <span data-ttu-id="e2355-125">Abrufen eines Produkts nach Kategorie</span><span class="sxs-lookup"><span data-stu-id="e2355-125">Get a product by category</span></span> | <span data-ttu-id="e2355-126">GET</span><span class="sxs-lookup"><span data-stu-id="e2355-126">GET</span></span> | <span data-ttu-id="e2355-127">/api/products?category=*category*</span><span class="sxs-lookup"><span data-stu-id="e2355-127">/api/products?category=*category*</span></span> |
| <span data-ttu-id="e2355-128">Erstellen eines neuen Produkts</span><span class="sxs-lookup"><span data-stu-id="e2355-128">Create a new product</span></span> | <span data-ttu-id="e2355-129">POST</span><span class="sxs-lookup"><span data-stu-id="e2355-129">POST</span></span> | <span data-ttu-id="e2355-130">/api/products</span><span class="sxs-lookup"><span data-stu-id="e2355-130">/api/products</span></span> |
| <span data-ttu-id="e2355-131">Aktualisieren eines Produkts</span><span class="sxs-lookup"><span data-stu-id="e2355-131">Update a product</span></span> | <span data-ttu-id="e2355-132">PUT</span><span class="sxs-lookup"><span data-stu-id="e2355-132">PUT</span></span> | <span data-ttu-id="e2355-133">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="e2355-133">/api/products/*id*</span></span> |
| <span data-ttu-id="e2355-134">Löschen eines Produkts</span><span class="sxs-lookup"><span data-stu-id="e2355-134">Delete a product</span></span> | <span data-ttu-id="e2355-135">DELETE</span><span class="sxs-lookup"><span data-stu-id="e2355-135">DELETE</span></span> | <span data-ttu-id="e2355-136">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="e2355-136">/api/products/*id*</span></span> |

<span data-ttu-id="e2355-137">Beachten Sie, dass einige der URIs die Produkt-ID im Pfad enthalten.</span><span class="sxs-lookup"><span data-stu-id="e2355-137">Notice that some of the URIs include the product ID in path.</span></span> <span data-ttu-id="e2355-138">Um das Produkt zu erhalten, deren ID ist die 28, z. B. sendet der Client eine GET-Anforderung für `http://hostname/api/products/28`.</span><span class="sxs-lookup"><span data-stu-id="e2355-138">For example, to get the product whose ID is 28, the client sends a GET request for `http://hostname/api/products/28`.</span></span>

### <a name="resources"></a><span data-ttu-id="e2355-139">Ressourcen</span><span class="sxs-lookup"><span data-stu-id="e2355-139">Resources</span></span>

<span data-ttu-id="e2355-140">Die API-Produkte werden URIs für zwei Ressourcentypen definiert:</span><span class="sxs-lookup"><span data-stu-id="e2355-140">The products API defines URIs for two resource types:</span></span>

| <span data-ttu-id="e2355-141">Ressource</span><span class="sxs-lookup"><span data-stu-id="e2355-141">Resource</span></span> | <span data-ttu-id="e2355-142">URI</span><span class="sxs-lookup"><span data-stu-id="e2355-142">URI</span></span> |
| --- | --- |
| <span data-ttu-id="e2355-143">Die Liste aller Produkte.</span><span class="sxs-lookup"><span data-stu-id="e2355-143">The list of all the products.</span></span> | <span data-ttu-id="e2355-144">/api/products</span><span class="sxs-lookup"><span data-stu-id="e2355-144">/api/products</span></span> |
| <span data-ttu-id="e2355-145">Ein einzelnes Produkt.</span><span class="sxs-lookup"><span data-stu-id="e2355-145">An individual product.</span></span> | <span data-ttu-id="e2355-146">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="e2355-146">/api/products/*id*</span></span> |

### <a name="methods"></a><span data-ttu-id="e2355-147">Methoden</span><span class="sxs-lookup"><span data-stu-id="e2355-147">Methods</span></span>

<span data-ttu-id="e2355-148">Die vier wichtigsten HTTP-Methoden (GET, PUT, POST und DELETE) können die CRUD-Vorgängen wie folgt zugeordnet:</span><span class="sxs-lookup"><span data-stu-id="e2355-148">The four main HTTP methods (GET, PUT, POST, and DELETE) can be mapped to CRUD operations as follows:</span></span>

- <span data-ttu-id="e2355-149">GET Ruft die Darstellung der Ressource am angegebenen URI ab.</span><span class="sxs-lookup"><span data-stu-id="e2355-149">GET retrieves the representation of the resource at a specified URI.</span></span> <span data-ttu-id="e2355-150">GET, sollte auf dem Server keine Nebeneffekte haben.</span><span class="sxs-lookup"><span data-stu-id="e2355-150">GET should have no side effects on the server.</span></span>
- <span data-ttu-id="e2355-151">PUT aktualisiert eine Ressource auf einem angegebenen URI.</span><span class="sxs-lookup"><span data-stu-id="e2355-151">PUT updates a resource at a specified URI.</span></span> <span data-ttu-id="e2355-152">PUT kann auch verwendet werden, um eine neue Ressource am angegebenen URI, zu erstellen, wenn der Server, Clients an neue URIs zulässt.</span><span class="sxs-lookup"><span data-stu-id="e2355-152">PUT can also be used to create a new resource at a specified URI, if the server allows clients to specify new URIs.</span></span> <span data-ttu-id="e2355-153">In diesem Tutorial wird in die API-Erstellung über PUT nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="e2355-153">For this tutorial, the API will not support creation through PUT.</span></span>
- <span data-ttu-id="e2355-154">POST wird eine neue Ressource erstellt.</span><span class="sxs-lookup"><span data-stu-id="e2355-154">POST creates a new resource.</span></span> <span data-ttu-id="e2355-155">Der Server weist den URI für das neue Objekt und gibt diesen URI als Teil der Antwortnachricht zurück.</span><span class="sxs-lookup"><span data-stu-id="e2355-155">The server assigns the URI for the new object and returns this URI as part of the response message.</span></span>
- <span data-ttu-id="e2355-156">DELETE Löscht eine Ressource auf einem angegebenen URI.</span><span class="sxs-lookup"><span data-stu-id="e2355-156">DELETE deletes a resource at a specified URI.</span></span>

<span data-ttu-id="e2355-157">Hinweis: Die PUT-Methode ersetzt die gesamte Product-Entität.</span><span class="sxs-lookup"><span data-stu-id="e2355-157">Note: The PUT method replaces the entire product entity.</span></span> <span data-ttu-id="e2355-158">Der Client soll, also eine vollständige Darstellung des aktualisierten Produkts zu senden.</span><span class="sxs-lookup"><span data-stu-id="e2355-158">That is, the client is expected to send a complete representation of the updated product.</span></span> <span data-ttu-id="e2355-159">Wenn Sie möchten die teilupdates zu unterstützen, wird die PATCH-Methode bevorzugt.</span><span class="sxs-lookup"><span data-stu-id="e2355-159">If you want to support partial updates, the PATCH method is preferred.</span></span> <span data-ttu-id="e2355-160">In diesem Tutorial implementiert PATCH nicht.</span><span class="sxs-lookup"><span data-stu-id="e2355-160">This tutorial does not implement PATCH.</span></span>

## <a name="create-a-new-web-api-project"></a><span data-ttu-id="e2355-161">Erstellen eines neuen Web-API-Projekts</span><span class="sxs-lookup"><span data-stu-id="e2355-161">Create a New Web API Project</span></span>

<span data-ttu-id="e2355-162">Starten, indem Sie Ausführung von Visual Studio, und wählen Sie **neues Projekt** aus der **starten** Seite.</span><span class="sxs-lookup"><span data-stu-id="e2355-162">Start by running Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="e2355-163">Alternativ wählen Sie in der **Datei** , wählen Sie im Menü **neu** und dann **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="e2355-163">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="e2355-164">In der **Vorlagen** wählen Sie im Bereich **installierte Vorlagen** und erweitern Sie die **Visual C#-** Knoten.</span><span class="sxs-lookup"><span data-stu-id="e2355-164">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="e2355-165">Klicken Sie unter **Visual C#-** Option **Web**.</span><span class="sxs-lookup"><span data-stu-id="e2355-165">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="e2355-166">Wählen Sie in der Liste der Projektvorlagen das Projekt **ASP.NET MVC 4-Webanwendung**.</span><span class="sxs-lookup"><span data-stu-id="e2355-166">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="e2355-167">Nennen Sie das Projekt &quot;ProductStore&quot; , und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="e2355-167">Name the project &quot;ProductStore&quot; and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

<span data-ttu-id="e2355-168">In der **neues ASP.NET MVC 4-Projekt** wählen Sie im Dialogfeld **Web-API-** , und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="e2355-168">In the **New ASP.NET MVC 4 Project** dialog, select **Web API** and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a><span data-ttu-id="e2355-169">Hinzufügen eines Modells</span><span class="sxs-lookup"><span data-stu-id="e2355-169">Adding a Model</span></span>

<span data-ttu-id="e2355-170">Ein *Modell* ist ein Objekt, das die Daten in Ihrer Anwendung darstellt.</span><span class="sxs-lookup"><span data-stu-id="e2355-170">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="e2355-171">In ASP.NET Web-API können Sie stark typisierte CLR-Objekte als Modelle verwenden, und sie werden automatisch in XML oder JSON serialisiert werden, für den Client.</span><span class="sxs-lookup"><span data-stu-id="e2355-171">In ASP.NET Web API, you can use strongly typed CLR objects as models, and they will automatically be serialized to XML or JSON for the client.</span></span>

<span data-ttu-id="e2355-172">Für die API ProductStore, besteht unsere Daten von Produkten, daher wir eine neue Klasse namens erstellen `Product`.</span><span class="sxs-lookup"><span data-stu-id="e2355-172">For the ProductStore API, our data consists of products, so we'll create a new class named `Product`.</span></span>

<span data-ttu-id="e2355-173">Wenn der Projektmappen-Explorer nicht angezeigt wird, klicken Sie auf die **Ansicht** Menü **Projektmappen-Explorer**.</span><span class="sxs-lookup"><span data-stu-id="e2355-173">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="e2355-174">Klicken Sie im Projektmappen-Explorer mit der Maustaste der **Modelle** Ordner.</span><span class="sxs-lookup"><span data-stu-id="e2355-174">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="e2355-175">Wählen Sie die Kontext-Meny **hinzufügen**, und wählen Sie dann **Klasse**.</span><span class="sxs-lookup"><span data-stu-id="e2355-175">From the context meny, select **Add**, then select **Class**.</span></span> <span data-ttu-id="e2355-176">Nennen Sie die Klasse &quot;Produkt&quot;.</span><span class="sxs-lookup"><span data-stu-id="e2355-176">Name the class &quot;Product&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

<span data-ttu-id="e2355-177">Fügen Sie die folgenden Eigenschaften auf der `Product` Klasse.</span><span class="sxs-lookup"><span data-stu-id="e2355-177">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a><span data-ttu-id="e2355-178">Hinzufügen eines Repositorys</span><span class="sxs-lookup"><span data-stu-id="e2355-178">Adding a Repository</span></span>

<span data-ttu-id="e2355-179">Wir müssen eine Sammlung von Produkten zu speichern.</span><span class="sxs-lookup"><span data-stu-id="e2355-179">We need to store a collection of products.</span></span> <span data-ttu-id="e2355-180">Es ist eine gute Idee, trennen Sie die Sammlung von unsere dienstimplementierung.</span><span class="sxs-lookup"><span data-stu-id="e2355-180">It's a good idea to separate the collection from our service implementation.</span></span> <span data-ttu-id="e2355-181">Auf diese Weise können wir Sicherungsspeicher ändern, ohne die Dienstklasse umzuschreiben.</span><span class="sxs-lookup"><span data-stu-id="e2355-181">That way, we can change the backing store without rewriting the service class.</span></span> <span data-ttu-id="e2355-182">Diese Art von Design wird aufgerufen, die *Repository* Muster.</span><span class="sxs-lookup"><span data-stu-id="e2355-182">This type of design is called the *repository* pattern.</span></span> <span data-ttu-id="e2355-183">Starten Sie durch die Definition einer generischen Schnittstelle für das Repository.</span><span class="sxs-lookup"><span data-stu-id="e2355-183">Start by defining a generic interface for the repository.</span></span>

<span data-ttu-id="e2355-184">Klicken Sie im Projektmappen-Explorer mit der Maustaste der **Modelle** Ordner.</span><span class="sxs-lookup"><span data-stu-id="e2355-184">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="e2355-185">Wählen Sie **hinzufügen**, und wählen Sie dann **neues Element**.</span><span class="sxs-lookup"><span data-stu-id="e2355-185">Select **Add**, then select **New Item**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

<span data-ttu-id="e2355-186">In der **Vorlagen** wählen Sie im Bereich **installierte Vorlagen** und erweitern Sie den C#-Knoten.</span><span class="sxs-lookup"><span data-stu-id="e2355-186">In the **Templates** pane, select **Installed Templates** and expand the C# node.</span></span> <span data-ttu-id="e2355-187">Wählen Sie unter c#, **Code**.</span><span class="sxs-lookup"><span data-stu-id="e2355-187">Under C#, select **Code**.</span></span> <span data-ttu-id="e2355-188">Wählen Sie in der Liste der Codevorlagen, **Schnittstelle**.</span><span class="sxs-lookup"><span data-stu-id="e2355-188">In the list of code templates, select **Interface**.</span></span> <span data-ttu-id="e2355-189">Benennen Sie die Schnittstelle &quot;IProductRepository&quot;.</span><span class="sxs-lookup"><span data-stu-id="e2355-189">Name the interface &quot;IProductRepository&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

<span data-ttu-id="e2355-190">Fügen Sie die folgende Implementierung:</span><span class="sxs-lookup"><span data-stu-id="e2355-190">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

<span data-ttu-id="e2355-191">Fügen Sie eine andere Klasse jetzt im Ordner "Models", mit dem Namen &quot;ProductRepository.&quot; Diese Klasse implementiert die `IProductRespository`-Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="e2355-191">Now add another class to the Models folder, named &quot;ProductRepository.&quot; This class will implement the `IProductRespository` interface.</span></span> <span data-ttu-id="e2355-192">Fügen Sie die folgende Implementierung:</span><span class="sxs-lookup"><span data-stu-id="e2355-192">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

<span data-ttu-id="e2355-193">Das Repository speichert die Liste im lokalen Speicher.</span><span class="sxs-lookup"><span data-stu-id="e2355-193">The repository keeps the list in local memory.</span></span> <span data-ttu-id="e2355-194">Dies ist in Ordnung ein Tutorial, aber in einer echten Anwendung würden Sie die Daten extern, entweder eine Datenbank speichern oder in einem cloudspeicher.</span><span class="sxs-lookup"><span data-stu-id="e2355-194">This is OK for a tutorial, but in a real application, you would store the data externally, either a database or in cloud storage.</span></span> <span data-ttu-id="e2355-195">Das Repository-Muster wird so ändern Sie die Implementierung zu einem späteren Zeitpunkt erleichtern.</span><span class="sxs-lookup"><span data-stu-id="e2355-195">The repository pattern will make it easier to change the implementation later.</span></span>

## <a name="adding-a-web-api-controller"></a><span data-ttu-id="e2355-196">Hinzufügen eines Web-API-Controllers</span><span class="sxs-lookup"><span data-stu-id="e2355-196">Adding a Web API Controller</span></span>

<span data-ttu-id="e2355-197">Wenn Sie mit ASP.NET MVC gearbeitet haben, sind Sie bereits vertraut mit Controllern.</span><span class="sxs-lookup"><span data-stu-id="e2355-197">If you have worked with ASP.NET MVC, then you are already familiar with controllers.</span></span> <span data-ttu-id="e2355-198">In ASP.NET Web-API eine *Controller* ist eine Klasse, die HTTP-Anforderungen vom Client verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="e2355-198">In ASP.NET Web API, a *controller* is a class that handles HTTP requests from the client.</span></span> <span data-ttu-id="e2355-199">Der Assistent für neues Projekt für Sie zwei Controller erstellt werden, wenn es sich bei der Erstellung des Projekts.</span><span class="sxs-lookup"><span data-stu-id="e2355-199">The New Project wizard created two controllers for you when it created the project.</span></span> <span data-ttu-id="e2355-200">Um anzuzeigen, erweitern Sie im Projektmappen-Explorer den Ordner "Controllers" aus.</span><span class="sxs-lookup"><span data-stu-id="e2355-200">To see them, expand the Controllers folder in Solution Explorer.</span></span>

- <span data-ttu-id="e2355-201">HomeController ist eine herkömmliche ASP.NET MVC-Controller.</span><span class="sxs-lookup"><span data-stu-id="e2355-201">HomeController is a traditional ASP.NET MVC controller.</span></span> <span data-ttu-id="e2355-202">Es wird dient zum Bereitstellen von HTML-Seiten für den Standort, und nicht direkt an unsere Web-API.</span><span class="sxs-lookup"><span data-stu-id="e2355-202">It is responsible for serving HTML pages for the site, and is not directly related to our web API.</span></span>
- <span data-ttu-id="e2355-203">ValuesController ist eine Beispiel-Web-API-Controller.</span><span class="sxs-lookup"><span data-stu-id="e2355-203">ValuesController is an example WebAPI controller.</span></span>

<span data-ttu-id="e2355-204">Löschen Sie ValuesController, indem Sie mit der rechten Maustaste in der Datei im Projektmappen-Explorer und auswählen **löschen.**</span><span class="sxs-lookup"><span data-stu-id="e2355-204">Go ahead and delete ValuesController, by right-clicking the file in Solution Explorer and selecting **Delete.**</span></span> <span data-ttu-id="e2355-205">Fügen Sie einen neuen Controller jetzt wie folgt:</span><span class="sxs-lookup"><span data-stu-id="e2355-205">Now add a new controller, as follows:</span></span>

<span data-ttu-id="e2355-206">In **Projektmappen-Explorer**, mit der rechten Maustaste in den Ordner "Controllers".</span><span class="sxs-lookup"><span data-stu-id="e2355-206">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="e2355-207">Wählen Sie **hinzufügen** und wählen Sie dann **Controller**.</span><span class="sxs-lookup"><span data-stu-id="e2355-207">Select **Add** and then select **Controller**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

<span data-ttu-id="e2355-208">In der **Controller hinzufügen** Assistenten benennen Sie den Controller &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="e2355-208">In the **Add Controller** wizard, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="e2355-209">In der **Vorlage** Dropdown-Liste **leerer API-Controller**.</span><span class="sxs-lookup"><span data-stu-id="e2355-209">In the **Template** drop-down list, select **Empty API Controller**.</span></span> <span data-ttu-id="e2355-210">Klicken Sie anschließend auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="e2355-210">Then click **Add**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="e2355-211">Es ist nicht notwendig, einen Ordner Controller Namens Ihrer Serviceprojekt abgelegt.</span><span class="sxs-lookup"><span data-stu-id="e2355-211">It is not necessary to put your contollers into a folder named Controllers.</span></span> <span data-ttu-id="e2355-212">Der Name des Ordners ist nicht wichtig; Es ist lediglich eine bequeme Möglichkeit, Ihre Quelldateien zu organisieren.</span><span class="sxs-lookup"><span data-stu-id="e2355-212">The folder name is not important; it is simply a convenient way to organize your source files.</span></span>


<span data-ttu-id="e2355-213">Die **Controller hinzufügen** Assistent erstellt eine Datei namens ProductsController.cs im Ordner "Controllers".</span><span class="sxs-lookup"><span data-stu-id="e2355-213">The **Add Controller** wizard will create a file named ProductsController.cs in the Controllers folder.</span></span> <span data-ttu-id="e2355-214">Wenn diese Datei noch nicht geöffnet ist, doppelklicken Sie auf die Datei, um ihn zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="e2355-214">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="e2355-215">Fügen Sie die folgenden **mit** Anweisung:</span><span class="sxs-lookup"><span data-stu-id="e2355-215">Add the following **using** statement:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

<span data-ttu-id="e2355-216">Hinzufügen eines Felds, das enthält ein **IProductRepository** Instanz.</span><span class="sxs-lookup"><span data-stu-id="e2355-216">Add a field that holds an **IProductRepository** instance.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> <span data-ttu-id="e2355-217">Aufrufen von `new ProductRepository()` im Controller ist nicht der optimale Entwurf, da es den Controller eine bestimmte Implementierung bindet `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="e2355-217">Calling `new ProductRepository()` in the controller is not the best design, because it ties the controller to a particular implementation of `IProductRepository`.</span></span> <span data-ttu-id="e2355-218">Ein besserer Ansatz ist, finden Sie unter [mithilfe der Web-API-Abhängigkeitskonfliktlöser](../advanced/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="e2355-218">For a better approach, see [Using the Web API Dependency Resolver](../advanced/dependency-injection.md).</span></span>


## <a name="getting-a-resource"></a><span data-ttu-id="e2355-219">Abrufen einer Ressourcensatzes</span><span class="sxs-lookup"><span data-stu-id="e2355-219">Getting a Resource</span></span>

<span data-ttu-id="e2355-220">Die ProductStore-API macht mehrere &quot;lesen&quot; Aktionen als HTTP GET-Methoden.</span><span class="sxs-lookup"><span data-stu-id="e2355-220">The ProductStore API will expose several &quot;read&quot; actions as HTTP GET methods.</span></span> <span data-ttu-id="e2355-221">Jede Aktion entspricht einer Methode in der `ProductsController` Klasse.</span><span class="sxs-lookup"><span data-stu-id="e2355-221">Each action will correspond to a method in the `ProductsController` class.</span></span>

| <span data-ttu-id="e2355-222">Aktion</span><span class="sxs-lookup"><span data-stu-id="e2355-222">Action</span></span> | <span data-ttu-id="e2355-223">HTTP-Methode</span><span class="sxs-lookup"><span data-stu-id="e2355-223">HTTP method</span></span> | <span data-ttu-id="e2355-224">Relativer URI</span><span class="sxs-lookup"><span data-stu-id="e2355-224">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e2355-225">Abrufen einer Liste aller Produkte</span><span class="sxs-lookup"><span data-stu-id="e2355-225">Get a list of all products</span></span> | <span data-ttu-id="e2355-226">GET</span><span class="sxs-lookup"><span data-stu-id="e2355-226">GET</span></span> | <span data-ttu-id="e2355-227">/api/products</span><span class="sxs-lookup"><span data-stu-id="e2355-227">/api/products</span></span> |
| <span data-ttu-id="e2355-228">Abrufen eines Produkts nach ID</span><span class="sxs-lookup"><span data-stu-id="e2355-228">Get a product by ID</span></span> | <span data-ttu-id="e2355-229">GET</span><span class="sxs-lookup"><span data-stu-id="e2355-229">GET</span></span> | <span data-ttu-id="e2355-230">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="e2355-230">/api/products/*id*</span></span> |
| <span data-ttu-id="e2355-231">Abrufen eines Produkts nach Kategorie</span><span class="sxs-lookup"><span data-stu-id="e2355-231">Get a product by category</span></span> | <span data-ttu-id="e2355-232">GET</span><span class="sxs-lookup"><span data-stu-id="e2355-232">GET</span></span> | <span data-ttu-id="e2355-233">/api/products?category=*category*</span><span class="sxs-lookup"><span data-stu-id="e2355-233">/api/products?category=*category*</span></span> |

<span data-ttu-id="e2355-234">Um die Liste aller Produkte zu erhalten, fügen Sie diese Methode, um die `ProductsController` Klasse:</span><span class="sxs-lookup"><span data-stu-id="e2355-234">To get the list of all products, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

<span data-ttu-id="e2355-235">Der Name der Methode beginnt mit &quot;erhalten&quot;, so, dass gemäß der Konvention sie GET-Anforderungen zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="e2355-235">The method name starts with &quot;Get&quot;, so by convention it maps to GET requests.</span></span> <span data-ttu-id="e2355-236">Darüber hinaus, da die Methode keine Parameter enthält, entspricht dem ein URI, der keine enthält ein *&quot;Id&quot;* Segment im Pfad.</span><span class="sxs-lookup"><span data-stu-id="e2355-236">Also, because the method has no parameters, it maps to a URI that does not contain an *&quot;id&quot;* segment in the path.</span></span>

<span data-ttu-id="e2355-237">Um ein Produkt nach ID zu erhalten, fügen Sie diese Methode, um die `ProductsController` Klasse:</span><span class="sxs-lookup"><span data-stu-id="e2355-237">To get a product by ID, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

<span data-ttu-id="e2355-238">Dieser Methodenname beginnt auch mit &quot;erhalten&quot;, aber die Methode weist einen Parameter namens *Id*. Dieser Parameter zugeordnet ist die &quot;Id&quot; Segment des URI-Pfads.</span><span class="sxs-lookup"><span data-stu-id="e2355-238">This method name also starts with &quot;Get&quot;, but the method has a parameter named *id*. This parameter is mapped to the &quot;id&quot; segment of the URI path.</span></span> <span data-ttu-id="e2355-239">Das ASP.NET Web-API-Framework konvertiert automatisch die ID in den richtigen Datentyp (**Int**) für den Parameter.</span><span class="sxs-lookup"><span data-stu-id="e2355-239">The ASP.NET Web API framework automatically converts the ID to the correct data type (**int**) for the parameter.</span></span>

<span data-ttu-id="e2355-240">Die GetProduct-Methode löst eine Ausnahme vom Typ **HttpResponseException** Wenn *Id* ist ungültig.</span><span class="sxs-lookup"><span data-stu-id="e2355-240">The GetProduct method throws an exception of type **HttpResponseException** if *id* is not valid.</span></span> <span data-ttu-id="e2355-241">Diese Ausnahme wird vom Framework in den Fehler 404 (nicht gefunden) übersetzt.</span><span class="sxs-lookup"><span data-stu-id="e2355-241">This exception will be translated by the framework into a 404 (Not Found) error.</span></span>

<span data-ttu-id="e2355-242">Abschließend fügen Sie eine Methode, um die Produkte nach Kategorie zu suchen:</span><span class="sxs-lookup"><span data-stu-id="e2355-242">Finally, add a method to find products by category:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

<span data-ttu-id="e2355-243">Wenn der Anforderungs-URI eine Abfragezeichenfolge enthält, versucht Web-API ab, die Abfrageparameter an einen Parameter in der Controllermethode übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="e2355-243">If the request URI has a query string, Web API tries to match the query parameters to parameters on the controller method.</span></span> <span data-ttu-id="e2355-244">Aus diesem Grund einen URI im Format "api/Produkte? Kategorie =*Kategorie*" an diese Methode zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="e2355-244">Therefore, a URI of the form "api/products?category=*category*" will map to this method.</span></span>

## <a name="creating-a-resource"></a><span data-ttu-id="e2355-245">Erstellen einer Ressource</span><span class="sxs-lookup"><span data-stu-id="e2355-245">Creating a Resource</span></span>

<span data-ttu-id="e2355-246">Als Nächstes fügen wir eine Methode, um die `ProductsController` Klasse zum Erstellen eines neuen Produkts.</span><span class="sxs-lookup"><span data-stu-id="e2355-246">Next, we'll add a method to the `ProductsController` class to create a new product.</span></span> <span data-ttu-id="e2355-247">Hier ist eine einfache Implementierung der Methode ein:</span><span class="sxs-lookup"><span data-stu-id="e2355-247">Here is a simple implementation of the method:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

<span data-ttu-id="e2355-248">Beachten Sie diese Methode zwei Dinge:</span><span class="sxs-lookup"><span data-stu-id="e2355-248">Note two things about this method:</span></span>

- <span data-ttu-id="e2355-249">Der Name der Methode beginnt mit &quot;Post... &quot;.</span><span class="sxs-lookup"><span data-stu-id="e2355-249">The method name starts with &quot;Post...&quot;.</span></span> <span data-ttu-id="e2355-250">Um ein neues Produkt zu erstellen, sendet der Client eine HTTP POST-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="e2355-250">To create a new product, the client sends an HTTP POST request.</span></span>
- <span data-ttu-id="e2355-251">Die Methode nimmt einen Parameter vom Typ Produkt.</span><span class="sxs-lookup"><span data-stu-id="e2355-251">The method takes a parameter of type Product.</span></span> <span data-ttu-id="e2355-252">In der Web-API werden die Parameter mit komplexen Typen aus dem Anforderungstext deserialisiert.</span><span class="sxs-lookup"><span data-stu-id="e2355-252">In Web API, parameters with complex types are deserialized from the request body.</span></span> <span data-ttu-id="e2355-253">Daher erwarten wir den Client um eine serialisierte Darstellung des ein Product-Objekt, in XML oder JSON-Format zu senden.</span><span class="sxs-lookup"><span data-stu-id="e2355-253">Therefore, we expect the client to send a serialized representation of a product object, in either XML or JSON format.</span></span>

<span data-ttu-id="e2355-254">Diese Implementierung funktioniert, aber es ist leider unvollständig.</span><span class="sxs-lookup"><span data-stu-id="e2355-254">This implementation will work, but it is not quite complete.</span></span> <span data-ttu-id="e2355-255">Wir möchten im Idealfall HTTP-Antwort auf die Folgendes umfassen:</span><span class="sxs-lookup"><span data-stu-id="e2355-255">Ideally, we would like the HTTP response to include the following:</span></span>

- <span data-ttu-id="e2355-256">**Antwortcode:** Standardmäßig legt der Web-API-Framework Statuscode der Antwort 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="e2355-256">**Response code:** By default, the Web API framework sets the response status code to 200 (OK).</span></span> <span data-ttu-id="e2355-257">Doch gemäß der HTTP/1.1-Protokoll, wenn eine POST-Anforderung bei der Erstellung einer Ressource, führt die sollte Serverantwort mit dem Status 201 (erstellt).</span><span class="sxs-lookup"><span data-stu-id="e2355-257">But according to the HTTP/1.1 protocol, when a POST request results in the creation of a resource, the server should reply with status 201 (Created).</span></span>
- <span data-ttu-id="e2355-258">**Speicherort:** , wenn der Server eine Ressource erstellt, sollte es den URI der neuen Ressource im Location-Header der Antwort enthalten.</span><span class="sxs-lookup"><span data-stu-id="e2355-258">**Location:** When the server creates a resource, it should include the URI of the new resource in the Location header of the response.</span></span>

<span data-ttu-id="e2355-259">ASP.NET Web-API erleichtert die Bearbeitung von HTTP-Antwortnachricht.</span><span class="sxs-lookup"><span data-stu-id="e2355-259">ASP.NET Web API makes it easy to manipulate the HTTP response message.</span></span> <span data-ttu-id="e2355-260">Dies ist die verbesserte Implementierung ein:</span><span class="sxs-lookup"><span data-stu-id="e2355-260">Here is the improved implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

<span data-ttu-id="e2355-261">Beachten Sie, dass der Rückgabetyp der Methode jetzt **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="e2355-261">Notice that the method return type is now **HttpResponseMessage**.</span></span> <span data-ttu-id="e2355-262">Durch Zurückgeben einer **HttpResponseMessage** anstelle eines Produkts, wir können die Details der HTTP-Antwortnachricht, einschließlich der Statuscode und den Location-Header steuern.</span><span class="sxs-lookup"><span data-stu-id="e2355-262">By returning an **HttpResponseMessage** instead of a Product, we can control the details of the HTTP response message, including the status code and the Location header.</span></span>

<span data-ttu-id="e2355-263">Die **CreateResponse** -Methode erstellt eine **HttpResponseMessage** und schreibt Sie automatisch eine serialisierte Darstellung der Product-Objekt in Text fo die Response-Nachricht.</span><span class="sxs-lookup"><span data-stu-id="e2355-263">The **CreateResponse** method creates an **HttpResponseMessage** and automatically writes a serialized representation of the Product object into the body fo the response message.</span></span>

> [!NOTE]
> <span data-ttu-id="e2355-264">In diesem Beispiel überprüft nicht die `Product`.</span><span class="sxs-lookup"><span data-stu-id="e2355-264">This example does not validate the `Product`.</span></span> <span data-ttu-id="e2355-265">Weitere Informationen zur modellvalidierung finden Sie unter [Model Validation in ASP.NET Web-API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="e2355-265">For information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>


## <a name="updating-a-resource"></a><span data-ttu-id="e2355-266">Aktualisieren einer Ressource</span><span class="sxs-lookup"><span data-stu-id="e2355-266">Updating a Resource</span></span>

<span data-ttu-id="e2355-267">Aktualisieren eines Produkts bei Verwendung von PUT ist einfach:</span><span class="sxs-lookup"><span data-stu-id="e2355-267">Updating a product with PUT is straightforward:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

<span data-ttu-id="e2355-268">Der Name der Methode beginnt mit &quot;einfügen... &quot;, sodass es Web-API-PUT-Anforderungen entspricht.</span><span class="sxs-lookup"><span data-stu-id="e2355-268">The method name starts with &quot;Put...&quot;, so Web API matches it to PUT requests.</span></span> <span data-ttu-id="e2355-269">Die Methode nimmt zwei Parameter, die Produkt-ID und des aktualisierten Produkts.</span><span class="sxs-lookup"><span data-stu-id="e2355-269">The method takes two parameters, the product ID and the updated product.</span></span> <span data-ttu-id="e2355-270">Die *Id* Parameter stammt aus dem URI-Pfad, und die *Produkt* Parameter aus dem Anforderungstext deserialisiert wird.</span><span class="sxs-lookup"><span data-stu-id="e2355-270">The *id* parameter is taken from the URI path, and the *product* parameter is deserialized from the request body.</span></span> <span data-ttu-id="e2355-271">Standardmäßig führt das ASP.NET Web-API-Framework einfache Parametertypen aus der Route und komplexe Typen aus dem Anforderungstext.</span><span class="sxs-lookup"><span data-stu-id="e2355-271">By default, the ASP.NET Web API framework takes simple parameter types from the route and complex types from the request body.</span></span>

## <a name="deleting-a-resource"></a><span data-ttu-id="e2355-272">Löschen einer Ressource</span><span class="sxs-lookup"><span data-stu-id="e2355-272">Deleting a Resource</span></span>

<span data-ttu-id="e2355-273">Definieren Sie eine "..." löschen"-Methode, um eine Ressourcen zu löschen.</span><span class="sxs-lookup"><span data-stu-id="e2355-273">To delete a resourse, define a "Delete..." method.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

<span data-ttu-id="e2355-274">Wenn eine DELETE-Anforderung erfolgreich ist, kann es Status 200 (OK) mit einem Entity-Body zurückgegeben wird, die den Status beschreibt. Status 202 (akzeptiert), wenn der Löschvorgang noch ausstehende; oder der Status 204 (kein Inhalt) ohne Text für die Entität.</span><span class="sxs-lookup"><span data-stu-id="e2355-274">If a DELETE request succeeds, it can return status 200 (OK) with an entity-body that describes the status; status 202 (Accepted) if the deletion is still pending; or status 204 (No Content) with no entity body.</span></span> <span data-ttu-id="e2355-275">In diesem Fall die `DeleteProduct` Methode verfügt über eine `void` Typ zurückgeben, sodass ASP.NET Web-API diese automatisch in den Status der übersetzt code 204 (No Content).</span><span class="sxs-lookup"><span data-stu-id="e2355-275">In this case, the `DeleteProduct` method has a `void` return type, so ASP.NET Web API automatically translates this into status code 204 (No Content).</span></span>
