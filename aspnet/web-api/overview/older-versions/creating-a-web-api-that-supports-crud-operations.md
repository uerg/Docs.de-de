---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: "Aktivieren von CRUD-Vorgänge in ASP.NET Web-API 1 | Microsoft Docs"
author: MikeWasson
description: "Dieses Lernprogramm zeigt, wie zur Unterstützung von CRUD-Vorgänge in einen HTTP-Dienst mithilfe der ASP.NET Web API. Softwareversionen, in dem Lernprogramm Visual Studio 2012 Web Zugriffspunkt verwendet..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/28/2012
ms.topic: article
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: a91bf065c9ce0fc5bd9b7115340edabea975a7e0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="enabling-crud-operations-in-aspnet-web-api-1"></a><span data-ttu-id="f6d0a-104">Aktivieren von CRUD-Vorgänge in ASP.NET Web-API 1</span><span class="sxs-lookup"><span data-stu-id="f6d0a-104">Enabling CRUD Operations in ASP.NET Web API 1</span></span>
====================
<span data-ttu-id="f6d0a-105">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f6d0a-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="f6d0a-106">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="f6d0a-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> <span data-ttu-id="f6d0a-107">Dieses Lernprogramm zeigt, wie zur Unterstützung von CRUD-Vorgänge in einen HTTP-Dienst mithilfe der ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-107">This tutorial shows how to support CRUD operations in an HTTP service using ASP.NET Web API.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f6d0a-108">In diesem Lernprogramm verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="f6d0a-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="f6d0a-109">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="f6d0a-109">Visual Studio 2012</span></span>
> - <span data-ttu-id="f6d0a-110">Web-API 1 (auch funktioniert mit Web-API 2)</span><span class="sxs-lookup"><span data-stu-id="f6d0a-110">Web API 1 (also works with Web API 2)</span></span>


<span data-ttu-id="f6d0a-111">CRUD steht für &quot;Create, Read, Update und "Delete"&quot; sind die vier grundlegenden Datenbankvorgängen.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-111">CRUD stands for &quot;Create, Read, Update, and Delete,&quot; which are the four basic database operations.</span></span> <span data-ttu-id="f6d0a-112">Viele HTTP-Diensten Modell auch CRUD-Vorgänge über REST oder REST-ähnliche-APIs.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-112">Many HTTP services also model CRUD operations through REST or REST-like APIs.</span></span>

<span data-ttu-id="f6d0a-113">In diesem Lernprogramm erstellen Sie eine sehr einfache Web-API zum Verwalten einer Liste von Produkten.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-113">In this tutorial, you will build a very simple web API to manage a list of products.</span></span> <span data-ttu-id="f6d0a-114">Jedes Produkt enthält ein Name, Preis und die Kategorie (z. B. &quot;"Toy"&quot; oder &quot;Hardware&quot;), sowie eine Produkt-ID.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-114">Each product will contain a name, price, and category (such as &quot;toys&quot; or &quot;hardware&quot;), plus a product ID.</span></span>

<span data-ttu-id="f6d0a-115">Die API-Produkte werden folgende Methoden verfügbar machen.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-115">The products API will expose following methods.</span></span>

| <span data-ttu-id="f6d0a-116">Aktion</span><span class="sxs-lookup"><span data-stu-id="f6d0a-116">Action</span></span> | <span data-ttu-id="f6d0a-117">HTTP-Methode</span><span class="sxs-lookup"><span data-stu-id="f6d0a-117">HTTP method</span></span> | <span data-ttu-id="f6d0a-118">Relativer URI</span><span class="sxs-lookup"><span data-stu-id="f6d0a-118">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f6d0a-119">Abrufen einer Liste aller Produkte</span><span class="sxs-lookup"><span data-stu-id="f6d0a-119">Get a list of all products</span></span> | <span data-ttu-id="f6d0a-120">GET</span><span class="sxs-lookup"><span data-stu-id="f6d0a-120">GET</span></span> | <span data-ttu-id="f6d0a-121">/ api /-Produkte</span><span class="sxs-lookup"><span data-stu-id="f6d0a-121">/api/products</span></span> |
| <span data-ttu-id="f6d0a-122">Abrufen eines Produkts nach ID</span><span class="sxs-lookup"><span data-stu-id="f6d0a-122">Get a product by ID</span></span> | <span data-ttu-id="f6d0a-123">GET</span><span class="sxs-lookup"><span data-stu-id="f6d0a-123">GET</span></span> | <span data-ttu-id="f6d0a-124">/API/Produkte/*Id*</span><span class="sxs-lookup"><span data-stu-id="f6d0a-124">/api/products/*id*</span></span> |
| <span data-ttu-id="f6d0a-125">Abrufen eines Produkts nach Kategorie</span><span class="sxs-lookup"><span data-stu-id="f6d0a-125">Get a product by category</span></span> | <span data-ttu-id="f6d0a-126">GET</span><span class="sxs-lookup"><span data-stu-id="f6d0a-126">GET</span></span> | <span data-ttu-id="f6d0a-127">Produkte/api /? Kategorie =*Kategorie*</span><span class="sxs-lookup"><span data-stu-id="f6d0a-127">/api/products?category=*category*</span></span> |
| <span data-ttu-id="f6d0a-128">Erstellen eines neuen Produkts</span><span class="sxs-lookup"><span data-stu-id="f6d0a-128">Create a new product</span></span> | <span data-ttu-id="f6d0a-129">BEREITSTELLEN</span><span class="sxs-lookup"><span data-stu-id="f6d0a-129">POST</span></span> | <span data-ttu-id="f6d0a-130">/ api /-Produkte</span><span class="sxs-lookup"><span data-stu-id="f6d0a-130">/api/products</span></span> |
| <span data-ttu-id="f6d0a-131">Aktualisieren eines Produkts</span><span class="sxs-lookup"><span data-stu-id="f6d0a-131">Update a product</span></span> | <span data-ttu-id="f6d0a-132">PUT</span><span class="sxs-lookup"><span data-stu-id="f6d0a-132">PUT</span></span> | <span data-ttu-id="f6d0a-133">/API/Produkte/*Id*</span><span class="sxs-lookup"><span data-stu-id="f6d0a-133">/api/products/*id*</span></span> |
| <span data-ttu-id="f6d0a-134">Löschen eines Produkts</span><span class="sxs-lookup"><span data-stu-id="f6d0a-134">Delete a product</span></span> | <span data-ttu-id="f6d0a-135">DELETE</span><span class="sxs-lookup"><span data-stu-id="f6d0a-135">DELETE</span></span> | <span data-ttu-id="f6d0a-136">/API/Produkte/*Id*</span><span class="sxs-lookup"><span data-stu-id="f6d0a-136">/api/products/*id*</span></span> |

<span data-ttu-id="f6d0a-137">Beachten Sie, dass einige der URIs die Produkt-ID im Pfad enthalten.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-137">Notice that some of the URIs include the product ID in path.</span></span> <span data-ttu-id="f6d0a-138">Um das Produkt zu erhalten, deren ID 28 ist, beispielsweise sendet der Client eine GET-Anforderung für `http://hostname/api/products/28`.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-138">For example, to get the product whose ID is 28, the client sends a GET request for `http://hostname/api/products/28`.</span></span>

### <a name="resources"></a><span data-ttu-id="f6d0a-139">Ressourcen</span><span class="sxs-lookup"><span data-stu-id="f6d0a-139">Resources</span></span>

<span data-ttu-id="f6d0a-140">Die Produkte API definiert URIs für zwei Ressourcentypen zur Verfügung:</span><span class="sxs-lookup"><span data-stu-id="f6d0a-140">The products API defines URIs for two resource types:</span></span>

| <span data-ttu-id="f6d0a-141">Ressource</span><span class="sxs-lookup"><span data-stu-id="f6d0a-141">Resource</span></span> | <span data-ttu-id="f6d0a-142">URI</span><span class="sxs-lookup"><span data-stu-id="f6d0a-142">URI</span></span> |
| --- | --- |
| <span data-ttu-id="f6d0a-143">Die Liste aller Produkte.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-143">The list of all the products.</span></span> | <span data-ttu-id="f6d0a-144">/ api /-Produkte</span><span class="sxs-lookup"><span data-stu-id="f6d0a-144">/api/products</span></span> |
| <span data-ttu-id="f6d0a-145">Ein einzelnes Produkt.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-145">An individual product.</span></span> | <span data-ttu-id="f6d0a-146">/API/Produkte/*Id*</span><span class="sxs-lookup"><span data-stu-id="f6d0a-146">/api/products/*id*</span></span> |

### <a name="methods"></a><span data-ttu-id="f6d0a-147">Methoden</span><span class="sxs-lookup"><span data-stu-id="f6d0a-147">Methods</span></span>

<span data-ttu-id="f6d0a-148">Die vier wichtigsten HTTP-Methoden (GET, PUT, POST und DELETE) können für CRUD-Vorgänge wie folgt zugeordnet:</span><span class="sxs-lookup"><span data-stu-id="f6d0a-148">The four main HTTP methods (GET, PUT, POST, and DELETE) can be mapped to CRUD operations as follows:</span></span>

- <span data-ttu-id="f6d0a-149">GET Ruft die Darstellung der Ressource an einem angegebenen URI ab.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-149">GET retrieves the representation of the resource at a specified URI.</span></span> <span data-ttu-id="f6d0a-150">GET sollte auf dem Server keine Nebeneffekte haben.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-150">GET should have no side effects on the server.</span></span>
- <span data-ttu-id="f6d0a-151">PUT aktualisiert eine Ressource an einem angegebenen URI.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-151">PUT updates a resource at a specified URI.</span></span> <span data-ttu-id="f6d0a-152">PUT kann auch zum Erstellen einer neuen Ressource an einem angegebenen URI verwendet werden, wenn der Server Clients an neue URIs zulässt.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-152">PUT can also be used to create a new resource at a specified URI, if the server allows clients to specify new URIs.</span></span> <span data-ttu-id="f6d0a-153">In diesem Lernprogramm wird die API Erstellung über PUT nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-153">For this tutorial, the API will not support creation through PUT.</span></span>
- <span data-ttu-id="f6d0a-154">POST erstellt eine neue Ressource.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-154">POST creates a new resource.</span></span> <span data-ttu-id="f6d0a-155">Der Server weist den URI für das neue Objekt und gibt diesen URI im Rahmen der Antwortnachricht zurück.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-155">The server assigns the URI for the new object and returns this URI as part of the response message.</span></span>
- <span data-ttu-id="f6d0a-156">DELETE Löscht eine Ressource an einem angegebenen URI.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-156">DELETE deletes a resource at a specified URI.</span></span>

<span data-ttu-id="f6d0a-157">Hinweis: Die PUT-Methode ersetzt die gesamte Product-Entität.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-157">Note: The PUT method replaces the entire product entity.</span></span> <span data-ttu-id="f6d0a-158">Der Client muss, also eine vollständige Darstellung des aktualisierten Produkts zu senden.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-158">That is, the client is expected to send a complete representation of the updated product.</span></span> <span data-ttu-id="f6d0a-159">Wenn Sie teilweise Updates unterstützen möchten, ist die PATCH-Methode bevorzugt.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-159">If you want to support partial updates, the PATCH method is preferred.</span></span> <span data-ttu-id="f6d0a-160">Dieses Lernprogramm implementiert den PATCH nicht.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-160">This tutorial does not implement PATCH.</span></span>

## <a name="create-a-new-web-api-project"></a><span data-ttu-id="f6d0a-161">Erstellen Sie eine neue Web-API-Projekt</span><span class="sxs-lookup"><span data-stu-id="f6d0a-161">Create a New Web API Project</span></span>

<span data-ttu-id="f6d0a-162">Starten Sie durch Ausführen von Visual Studio, und wählen Sie **neues Projekt** aus der **starten** Seite.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-162">Start by running Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="f6d0a-163">Oder von der **Datei** klicken Sie im Menü **neu** und dann **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-163">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="f6d0a-164">In der **Vorlagen** klicken Sie im Bereich **installierte Vorlagen** und erweitern Sie die **Visual C#-** Knoten.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-164">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="f6d0a-165">Klicken Sie unter **Visual C#-**Option **Web**.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-165">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="f6d0a-166">Wählen Sie in der Liste der Projektvorlagen **ASP.NET MVC 4-Webanwendung**.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-166">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="f6d0a-167">Nennen Sie das Projekt &quot;ProductStore&quot; , und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-167">Name the project &quot;ProductStore&quot; and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

<span data-ttu-id="f6d0a-168">In der **neues ASP.NET MVC 4-Projekt** wählen Sie im Dialogfeld **Web-API** , und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-168">In the **New ASP.NET MVC 4 Project** dialog, select **Web API** and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a><span data-ttu-id="f6d0a-169">Hinzufügen eines Modells</span><span class="sxs-lookup"><span data-stu-id="f6d0a-169">Adding a Model</span></span>

<span data-ttu-id="f6d0a-170">Ein *Modell* ist ein Objekt, das die Daten in Ihrer Anwendung darstellt.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-170">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="f6d0a-171">In ASP.NET Web-API können Sie als Modelle stark typisierter CLR-Objekte, und sie werden automatisch in XML oder JSON serialisiert werden, für den Client.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-171">In ASP.NET Web API, you can use strongly typed CLR objects as models, and they will automatically be serialized to XML or JSON for the client.</span></span>

<span data-ttu-id="f6d0a-172">Die ProductStore-API, besteht unsere Daten von Produkten, damit wir eine neue Klasse namens erstellen `Product`.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-172">For the ProductStore API, our data consists of products, so we'll create a new class named `Product`.</span></span>

<span data-ttu-id="f6d0a-173">Wenn der Projektmappen-Explorer nicht sichtbar ist, klicken Sie auf die **Ansicht** Menü **Projektmappen-Explorer**.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-173">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="f6d0a-174">Klicken Sie im Projektmappen-Explorer mit der Maustaste die **Modelle** Ordner.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-174">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="f6d0a-175">Wählen Sie aus dem Kontext Meny **hinzufügen**, und wählen Sie dann **Klasse**.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-175">From the context meny, select **Add**, then select **Class**.</span></span> <span data-ttu-id="f6d0a-176">Benennen Sie die Klasse &quot;Produkt&quot;.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-176">Name the class &quot;Product&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

<span data-ttu-id="f6d0a-177">Fügen Sie die folgenden Eigenschaften auf der `Product` Klasse.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-177">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a><span data-ttu-id="f6d0a-178">Hinzufügen eines Repositorys</span><span class="sxs-lookup"><span data-stu-id="f6d0a-178">Adding a Repository</span></span>

<span data-ttu-id="f6d0a-179">Speichern einer Sammlung von Produkten ist erforderlich.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-179">We need to store a collection of products.</span></span> <span data-ttu-id="f6d0a-180">Es ist eine gute Idee, trennen Sie die Auflistung von unseren dienstimplementierung.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-180">It's a good idea to separate the collection from our service implementation.</span></span> <span data-ttu-id="f6d0a-181">Auf diese Weise können wir den Sicherungsspeicher ändern, ohne die Dienstklasse umzuschreiben.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-181">That way, we can change the backing store without rewriting the service class.</span></span> <span data-ttu-id="f6d0a-182">Diese Art von Design heißt die *Repository* Muster.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-182">This type of design is called the *repository* pattern.</span></span> <span data-ttu-id="f6d0a-183">Starten Sie durch die Definition einer generischen Schnittstelle für das Repository.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-183">Start by defining a generic interface for the repository.</span></span>

<span data-ttu-id="f6d0a-184">Klicken Sie im Projektmappen-Explorer mit der Maustaste die **Modelle** Ordner.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-184">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="f6d0a-185">Wählen Sie **hinzufügen**, und wählen Sie dann **neues Element**.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-185">Select **Add**, then select **New Item**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

<span data-ttu-id="f6d0a-186">In der **Vorlagen** klicken Sie im Bereich **installierte Vorlagen** und erweitern Sie den C#-Knoten.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-186">In the **Templates** pane, select **Installed Templates** and expand the C# node.</span></span> <span data-ttu-id="f6d0a-187">Wählen Sie unter C#-, **Code**.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-187">Under C#, select **Code**.</span></span> <span data-ttu-id="f6d0a-188">Wählen Sie in der Liste der Vorlagen für Code, **Schnittstelle**.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-188">In the list of code templates, select **Interface**.</span></span> <span data-ttu-id="f6d0a-189">Benennen Sie die Schnittstelle &quot;IProductRepository&quot;.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-189">Name the interface &quot;IProductRepository&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

<span data-ttu-id="f6d0a-190">Fügen Sie die folgende Implementierung hinzu:</span><span class="sxs-lookup"><span data-stu-id="f6d0a-190">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

<span data-ttu-id="f6d0a-191">Fügen Sie einer anderen Klasse jetzt Ordner Models, mit dem Namen &quot;ProductRepository.&quot; Diese Klasse implementiert die `IProductRespository`-Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-191">Now add another class to the Models folder, named &quot;ProductRepository.&quot; This class will implement the `IProductRespository` interface.</span></span> <span data-ttu-id="f6d0a-192">Fügen Sie die folgende Implementierung hinzu:</span><span class="sxs-lookup"><span data-stu-id="f6d0a-192">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

<span data-ttu-id="f6d0a-193">Das Repository behält die Liste im lokalen Speicher.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-193">The repository keeps the list in local memory.</span></span> <span data-ttu-id="f6d0a-194">Dies ist in Ordnung ein Lernprogramm, aber in einer echten Anwendung würden Sie extern können die Daten entweder einer Datenbank speichern oder im cloudspeicher.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-194">This is OK for a tutorial, but in a real application, you would store the data externally, either a database or in cloud storage.</span></span> <span data-ttu-id="f6d0a-195">Das Repositorymuster wird so ändern Sie die Implementierung zu einem späteren Zeitpunkt erleichtern.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-195">The repository pattern will make it easier to change the implementation later.</span></span>

## <a name="adding-a-web-api-controller"></a><span data-ttu-id="f6d0a-196">Hinzufügen eines Web-API-Controllers</span><span class="sxs-lookup"><span data-stu-id="f6d0a-196">Adding a Web API Controller</span></span>

<span data-ttu-id="f6d0a-197">Wenn Sie mit ASP.NET MVC gearbeitet haben, sind Sie bereits vertraut mit Domänencontrollern.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-197">If you have worked with ASP.NET MVC, then you are already familiar with controllers.</span></span> <span data-ttu-id="f6d0a-198">In ASP.NET Web-API eine *Controller* ist eine Klasse, die HTTP-Anforderungen vom Client verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-198">In ASP.NET Web API, a *controller* is a class that handles HTTP requests from the client.</span></span> <span data-ttu-id="f6d0a-199">Der neue Assistent für zwei Controller für Sie erstellt, bei der Erstellung des Projekts.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-199">The New Project wizard created two controllers for you when it created the project.</span></span> <span data-ttu-id="f6d0a-200">Um diese anzuzeigen, erweitern Sie den Ordner "Controller" im Projektmappen-Explorer aus.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-200">To see them, expand the Controllers folder in Solution Explorer.</span></span>

- <span data-ttu-id="f6d0a-201">HomeController ist eine herkömmliche ASP.NET MVC-Controllers.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-201">HomeController is a traditional ASP.NET MVC controller.</span></span> <span data-ttu-id="f6d0a-202">Er ist verantwortlich für das Bereitstellen von HTML-Seiten für die Website, und nicht direkt mit unserer Web-API verknüpft ist.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-202">It is responsible for serving HTML pages for the site, and is not directly related to our web API.</span></span>
- <span data-ttu-id="f6d0a-203">ValuesController ist eine Beispiel-WebAPI-Controller.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-203">ValuesController is an example WebAPI controller.</span></span>

<span data-ttu-id="f6d0a-204">Fahren Sie fort, und Löschen von ValuesController, indem Sie die Datei im Projektmappen-Explorer mit der rechten Maustaste und auswählen **löschen.**</span><span class="sxs-lookup"><span data-stu-id="f6d0a-204">Go ahead and delete ValuesController, by right-clicking the file in Solution Explorer and selecting **Delete.**</span></span> <span data-ttu-id="f6d0a-205">Nun fügen Sie einen neuen Domänencontroller wie folgt:</span><span class="sxs-lookup"><span data-stu-id="f6d0a-205">Now add a new controller, as follows:</span></span>

<span data-ttu-id="f6d0a-206">In **Projektmappen-Explorer**, mit der rechten Maustaste den Ordner.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-206">In **Solution Explorer**, right-click the the Controllers folder.</span></span> <span data-ttu-id="f6d0a-207">Wählen Sie **hinzufügen** und wählen Sie dann **Controller**.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-207">Select **Add** and then select **Controller**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

<span data-ttu-id="f6d0a-208">In der **Controller hinzufügen** Assistenten, der Name des Controllers &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-208">In the **Add Controller** wizard, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="f6d0a-209">In der **Vorlage** Dropdown-Liste **leerer API-Controller**.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-209">In the **Template** drop-down list, select **Empty API Controller**.</span></span> <span data-ttu-id="f6d0a-210">Klicken Sie anschließend auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-210">Then click **Add**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="f6d0a-211">Es ist nicht notwendig, einen Ordner namens Controller Ihrer Controllern abgelegt.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-211">It is not necessary to put your contollers into a folder named Controllers.</span></span> <span data-ttu-id="f6d0a-212">Der Name des Ordners spielt keine Rolle; Es ist einfach eine einfache Möglichkeit, Ihre Quelldateien zu organisieren.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-212">The folder name is not important; it is simply a convenient way to organize your source files.</span></span>


<span data-ttu-id="f6d0a-213">Die **Controller hinzufügen** Assistenten wird eine Datei namens ProductsController.cs im Ordner "Controller" erstellt.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-213">The **Add Controller** wizard will create a file named ProductsController.cs in the Controllers folder.</span></span> <span data-ttu-id="f6d0a-214">Wenn diese Datei noch nicht geöffnet ist, doppelklicken Sie auf die Datei, um ihn zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-214">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="f6d0a-215">Fügen Sie die folgenden **mit** Anweisung:</span><span class="sxs-lookup"><span data-stu-id="f6d0a-215">Add the following **using** statement:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

<span data-ttu-id="f6d0a-216">Hinzufügen eines Felds, der enthält einem **IProductRepository** Instanz.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-216">Add a field that holds an **IProductRepository** instance.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> <span data-ttu-id="f6d0a-217">Aufrufen von `new ProductRepository()` im Controller ist nicht zum optimale Entwurf, da es den Controller aus, um eine bestimmte Implementierung des bindet `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-217">Calling `new ProductRepository()` in the controller is not the best design, because it ties the controller to a particular implementation of `IProductRepository`.</span></span> <span data-ttu-id="f6d0a-218">Ein besserer Ansatz finden Sie unter [mithilfe der Web-API-Abhängigkeitskonfliktlöser](../advanced/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="f6d0a-218">For a better approach, see [Using the Web API Dependency Resolver](../advanced/dependency-injection.md).</span></span>


## <a name="getting-a-resource"></a><span data-ttu-id="f6d0a-219">Abrufen einer Ressourcenpools</span><span class="sxs-lookup"><span data-stu-id="f6d0a-219">Getting a Resource</span></span>

<span data-ttu-id="f6d0a-220">Die ProductStore-API macht mehrere &quot;lesen&quot; Aktionen als HTTP GET-Methoden.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-220">The ProductStore API will expose several &quot;read&quot; actions as HTTP GET methods.</span></span> <span data-ttu-id="f6d0a-221">Jede Aktion wird eine Methode in entsprechen den `ProductsController` Klasse.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-221">Each action will correspond to a method in the `ProductsController` class.</span></span>

| <span data-ttu-id="f6d0a-222">Aktion</span><span class="sxs-lookup"><span data-stu-id="f6d0a-222">Action</span></span> | <span data-ttu-id="f6d0a-223">HTTP-Methode</span><span class="sxs-lookup"><span data-stu-id="f6d0a-223">HTTP method</span></span> | <span data-ttu-id="f6d0a-224">Relativer URI</span><span class="sxs-lookup"><span data-stu-id="f6d0a-224">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f6d0a-225">Abrufen einer Liste aller Produkte</span><span class="sxs-lookup"><span data-stu-id="f6d0a-225">Get a list of all products</span></span> | <span data-ttu-id="f6d0a-226">GET</span><span class="sxs-lookup"><span data-stu-id="f6d0a-226">GET</span></span> | <span data-ttu-id="f6d0a-227">/ api /-Produkte</span><span class="sxs-lookup"><span data-stu-id="f6d0a-227">/api/products</span></span> |
| <span data-ttu-id="f6d0a-228">Abrufen eines Produkts nach ID</span><span class="sxs-lookup"><span data-stu-id="f6d0a-228">Get a product by ID</span></span> | <span data-ttu-id="f6d0a-229">GET</span><span class="sxs-lookup"><span data-stu-id="f6d0a-229">GET</span></span> | <span data-ttu-id="f6d0a-230">/API/Produkte/*Id*</span><span class="sxs-lookup"><span data-stu-id="f6d0a-230">/api/products/*id*</span></span> |
| <span data-ttu-id="f6d0a-231">Abrufen eines Produkts nach Kategorie</span><span class="sxs-lookup"><span data-stu-id="f6d0a-231">Get a product by category</span></span> | <span data-ttu-id="f6d0a-232">GET</span><span class="sxs-lookup"><span data-stu-id="f6d0a-232">GET</span></span> | <span data-ttu-id="f6d0a-233">Produkte/api /? Kategorie =*Kategorie*</span><span class="sxs-lookup"><span data-stu-id="f6d0a-233">/api/products?category=*category*</span></span> |

<span data-ttu-id="f6d0a-234">Um die Liste aller Produkte abzurufen, fügen Sie diese Methode, um die `ProductsController` Klasse:</span><span class="sxs-lookup"><span data-stu-id="f6d0a-234">To get the list of all products, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

<span data-ttu-id="f6d0a-235">Der Name der Methode beginnt mit &quot;abrufen&quot;, so, dass gemäß der Konvention sie GET-Anforderungen zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-235">The method name starts with &quot;Get&quot;, so by convention it maps to GET requests.</span></span> <span data-ttu-id="f6d0a-236">Ordnet auch dann, da die Methode keine Parameter hat, es ein URI, der keine enthält ein  *&quot;Id&quot;*  Segment im Pfad.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-236">Also, because the method has no parameters, it maps to a URI that does not contain an *&quot;id&quot;* segment in the path.</span></span>

<span data-ttu-id="f6d0a-237">Um ein Produkt nach ID zu erhalten, fügen Sie diese Methode, um die `ProductsController` Klasse:</span><span class="sxs-lookup"><span data-stu-id="f6d0a-237">To get a product by ID, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

<span data-ttu-id="f6d0a-238">Dieser Methodenname auch beginnt mit &quot;abrufen&quot;, aber die Methode verfügt über einen Parameter namens *Id*. Dieser Parameter zugeordnet ist die &quot;Id&quot; Segment des URI-Pfads.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-238">This method name also starts with &quot;Get&quot;, but the method has a parameter named *id*. This parameter is mapped to the &quot;id&quot; segment of the URI path.</span></span> <span data-ttu-id="f6d0a-239">Der ASP.NET Web API-Framework konvertiert automatisch die ID in den richtigen Datentyp (**Int**) für den Parameter.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-239">The ASP.NET Web API framework automatically converts the ID to the correct data type (**int**) for the parameter.</span></span>

<span data-ttu-id="f6d0a-240">Die GetProduct Methode löst eine Ausnahme vom Typ **HttpResponseException** Wenn *Id* ist ungültig.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-240">The GetProduct method throws an exception of type **HttpResponseException** if *id* is not valid.</span></span> <span data-ttu-id="f6d0a-241">Diese Ausnahme wird vom Framework in Fehler 404 (Nichtgefunden) übersetzt werden.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-241">This exception will be translated by the framework into a 404 (Not Found) error.</span></span>

<span data-ttu-id="f6d0a-242">Abschließend fügen Sie eine Methode zum Suchen nach Produkten nach Kategorie hinzu:</span><span class="sxs-lookup"><span data-stu-id="f6d0a-242">Finally, add a method to find products by category:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

<span data-ttu-id="f6d0a-243">Wenn im Anforderungs-URI eine Abfragezeichenfolge enthält, versucht Web-API ab, die Abfrageparameter an einen Parameter in der Controllermethode übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-243">If the request URI has a query string, Web API tries to match the query parameters to parameters on the controller method.</span></span> <span data-ttu-id="f6d0a-244">Deshalb einen URI im Format "api-Produkte? Kategorie =*Kategorie*" werden an diese Methode zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-244">Therefore, a URI of the form "api/products?category=*category*" will map to this method.</span></span>

## <a name="creating-a-resource"></a><span data-ttu-id="f6d0a-245">Erstellen einer Ressource</span><span class="sxs-lookup"><span data-stu-id="f6d0a-245">Creating a Resource</span></span>

<span data-ttu-id="f6d0a-246">Als Nächstes fügen wir eine Methode, um die `ProductsController` Klasse, um ein neues Produkt zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-246">Next, we'll add a method to the `ProductsController` class to create a new product.</span></span> <span data-ttu-id="f6d0a-247">Hier ist eine einfache Implementierung der Methode:</span><span class="sxs-lookup"><span data-stu-id="f6d0a-247">Here is a simple implementation of the method:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

<span data-ttu-id="f6d0a-248">Beachten Sie zwei Dinge zu dieser Methode ein:</span><span class="sxs-lookup"><span data-stu-id="f6d0a-248">Note two things about this method:</span></span>

- <span data-ttu-id="f6d0a-249">Der Name der Methode beginnt mit &quot;Post... &quot;.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-249">The method name starts with &quot;Post...&quot;.</span></span> <span data-ttu-id="f6d0a-250">So erstellen ein neues Produkt, sendet der Client eine HTTP POST-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-250">To create a new product, the client sends an HTTP POST request.</span></span>
- <span data-ttu-id="f6d0a-251">Die Methode nimmt einen Parameter vom Typ Produkt.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-251">The method takes a parameter of type Product.</span></span> <span data-ttu-id="f6d0a-252">In der Web-API werden die Parameter mit komplexen Typen aus dem Anforderungstext deserialisiert.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-252">In Web API, parameters with complex types are deserialized from the request body.</span></span> <span data-ttu-id="f6d0a-253">Daher erwarten wir den Client eine serialisierte Darstellung eines Objekts Produkt in XML oder JSON-Format senden.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-253">Therefore, we expect the client to send a serialized representation of a product object, in either XML or JSON format.</span></span>

<span data-ttu-id="f6d0a-254">Diese Implementierung funktioniert, aber es ist nicht ganz abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-254">This implementation will work, but it is not quite complete.</span></span> <span data-ttu-id="f6d0a-255">Im Idealfall möchten wir die HTTP-Antwort, die Folgendes umfassen:</span><span class="sxs-lookup"><span data-stu-id="f6d0a-255">Ideally, we would like the HTTP response to include the following:</span></span>

- <span data-ttu-id="f6d0a-256">**Antwortcode:** Standardmäßig legt der Web-API-Framework Statuscode der Antwort 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="f6d0a-256">**Response code:** By default, the Web API framework sets the response status code to 200 (OK).</span></span> <span data-ttu-id="f6d0a-257">Jedoch gemäß der HTTP/1.1-Protokoll, wenn bei der Erstellung einer Ressource, eine POST-Anforderung führt der Server sollten Antworten mit dem Status 201 (erstellt).</span><span class="sxs-lookup"><span data-stu-id="f6d0a-257">But according to the HTTP/1.1 protocol, when a POST request results in the creation of a resource, the server should reply with status 201 (Created).</span></span>
- <span data-ttu-id="f6d0a-258">**Ort:** , wenn der Server eine Ressource erstellt, sollte er den URI der neuen Ressource im Location-Header der Antwort enthalten.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-258">**Location:** When the server creates a resource, it should include the URI of the new resource in the Location header of the response.</span></span>

<span data-ttu-id="f6d0a-259">ASP.NET Web API erleichtert die Bearbeitung der HTTP-Antwort-Nachricht.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-259">ASP.NET Web API makes it easy to manipulate the HTTP response message.</span></span> <span data-ttu-id="f6d0a-260">So sieht die verbesserte Implementierung aus:</span><span class="sxs-lookup"><span data-stu-id="f6d0a-260">Here is the improved implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

<span data-ttu-id="f6d0a-261">Beachten Sie, dass der Rückgabetyp der Methode jetzt **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-261">Notice that the method return type is now **HttpResponseMessage**.</span></span> <span data-ttu-id="f6d0a-262">Durch das Zurückgeben einer **HttpResponseMessage** anstelle eines Produkts können zu steuern, die Details der HTTP-Antwortnachricht, einschließlich der Statuscode und den Location-Header.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-262">By returning an **HttpResponseMessage** instead of a Product, we can control the details of the HTTP response message, including the status code and the Location header.</span></span>

<span data-ttu-id="f6d0a-263">Die **CreateResponse** Methode erstellt ein **HttpResponseMessage** und schreibt Sie eine serialisierte Darstellung des Objekts Produkt automatisch in den Textkörper fo der Antwortnachricht.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-263">The **CreateResponse** method creates an **HttpResponseMessage** and automatically writes a serialized representation of the Product object into the body fo the response message.</span></span>

> [!NOTE]
> <span data-ttu-id="f6d0a-264">In diesem Beispiel führt keine Überprüfung der `Product`.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-264">This example does not validate the `Product`.</span></span> <span data-ttu-id="f6d0a-265">Informationen zur modellvalidierung finden Sie unter [Modellvalidierung in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="f6d0a-265">For information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>


## <a name="updating-a-resource"></a><span data-ttu-id="f6d0a-266">Aktualisieren einer Ressource</span><span class="sxs-lookup"><span data-stu-id="f6d0a-266">Updating a Resource</span></span>

<span data-ttu-id="f6d0a-267">Aktualisieren eines Produkts bei PUT ist einfach:</span><span class="sxs-lookup"><span data-stu-id="f6d0a-267">Updating a product with PUT is straightforward:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

<span data-ttu-id="f6d0a-268">Der Name der Methode beginnt mit &quot;einfügen... &quot;, sodass es Web-API für PUT-Anforderungen entspricht.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-268">The method name starts with &quot;Put...&quot;, so Web API matches it to PUT requests.</span></span> <span data-ttu-id="f6d0a-269">Die Methode akzeptiert zwei Parameter, die Produkt-ID und des aktualisierten Produkts.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-269">The method takes two parameters, the product ID and the updated product.</span></span> <span data-ttu-id="f6d0a-270">Die *Id* Parameter stammt aus dem URI-Pfad und der *Produkt* Parameter aus dem Anforderungstext deserialisiert wird.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-270">The *id* parameter is taken from the URI path, and the *product* parameter is deserialized from the request body.</span></span> <span data-ttu-id="f6d0a-271">Standardmäßig führt der ASP.NET Web API-Framework einfache Parametertypen aus der Route und komplexe Typen aus dem Anforderungstext.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-271">By default, the ASP.NET Web API framework takes simple parameter types from the route and complex types from the request body.</span></span>

## <a name="deleting-a-resource"></a><span data-ttu-id="f6d0a-272">Beim Löschen einer Ressource</span><span class="sxs-lookup"><span data-stu-id="f6d0a-272">Deleting a Resource</span></span>

<span data-ttu-id="f6d0a-273">Definieren Sie eine "Löschen..."-Methode, um eine Ressourcen zu löschen.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-273">To delete a resourse, define a "Delete..." method.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

<span data-ttu-id="f6d0a-274">Wenn eine DELETE-Anforderung erfolgreich ist, können sie Statusinformationen 200 (OK) mit einem Entitätstext zurückgeben, die den Status beschreibt; Status 202 (akzeptiert), wenn der Löschvorgang noch ausstehende; oder der Status 204 (kein Inhalt) ohne Text Entität.</span><span class="sxs-lookup"><span data-stu-id="f6d0a-274">If a DELETE request succeeds, it can return status 200 (OK) with an entity-body that describes the status; status 202 (Accepted) if the deletion is still pending; or status 204 (No Content) with no entity body.</span></span> <span data-ttu-id="f6d0a-275">In diesem Fall die `DeleteProduct` Methode verfügt über eine `void` Typ zurückgeben, damit ASP.NET Web API dies automatisch in den Status der übersetzt code 204 (kein Inhalt).</span><span class="sxs-lookup"><span data-stu-id="f6d0a-275">In this case, the `DeleteProduct` method has a `void` return type, so ASP.NET Web API automatically translates this into status code 204 (No Content).</span></span>
