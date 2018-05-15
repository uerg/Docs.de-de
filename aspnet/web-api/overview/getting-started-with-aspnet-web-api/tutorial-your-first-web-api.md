---
uid: web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
title: Erste Schritte mit ASP.NET Web API 2 (c#)
author: MikeWasson
description: HTTP ist nicht nur für Webseiten bereitstellen. Es ist auch eine leistungsstarke Plattform zum Erstellen von APIs, die Dienste und Daten verfügbar machen. HTTP ist einfacher, flexibler, und Ubiq...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/28/2017
ms.topic: article
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
msc.type: authoredcontent
ms.openlocfilehash: d881563cdb6449aada444ef0528061581113a925
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2018
---
<a name="get-started-with-aspnet-web-api-2-c"></a><span data-ttu-id="c6ef5-105">Erste Schritte mit ASP.NET Web API 2 (c#)</span><span class="sxs-lookup"><span data-stu-id="c6ef5-105">Get Started with ASP.NET Web API 2 (C#)</span></span>
====================
<span data-ttu-id="c6ef5-106">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c6ef5-106">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="c6ef5-107">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="c6ef5-107">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Sample-code-of-Getting-c56ccb28)

<span data-ttu-id="c6ef5-108">HTTP ist nicht nur für Webseiten bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-108">HTTP is not just for serving up web pages.</span></span> <span data-ttu-id="c6ef5-109">HTTP ist auch eine leistungsstarke Plattform zum Erstellen von APIs, die Dienste und Daten verfügbar machen.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-109">HTTP is also a powerful platform for building APIs that expose services and data.</span></span> <span data-ttu-id="c6ef5-110">HTTP ist einfacher, flexibler und weit verbreitete.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-110">HTTP is simple, flexible, and ubiquitous.</span></span> <span data-ttu-id="c6ef5-111">Nahezu jede Plattform, der Sie vorstellen können hat eine HTTP-Clientbibliothek aus, sodass HTTP-Diensten eine Breite Palette von Clients, einschließlich Browsern und mobilen Geräten herkömmliche desktopanwendungen erreichen können.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-111">Almost any platform that you can think of has an HTTP library, so HTTP services can reach a broad range of clients, including browsers, mobile devices, and traditional desktop applications.</span></span>
 
<span data-ttu-id="c6ef5-112">ASP.NET Web-API ist ein Framework zum Erstellen von Web-APIs auf .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-112">ASP.NET Web API is a framework for building web APIs on top of the .NET Framework.</span></span> <span data-ttu-id="c6ef5-113">In diesem Lernprogramm verwenden Sie ASP.NET Web-API, um eine Web-API zu erstellen, die eine Liste der Produkte zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-113">In this tutorial, you will use ASP.NET Web API to create a web API that returns a list of products.</span></span>

 
 ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="c6ef5-114">In diesem Lernprogramm verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="c6ef5-114">Software versions used in the tutorial</span></span>
  
 - [<span data-ttu-id="c6ef5-115">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="c6ef5-115">Visual Studio 2017</span></span>](https://www.visualstudio.com/downloads/)
 - <span data-ttu-id="c6ef5-116">Web-API 2</span><span class="sxs-lookup"><span data-stu-id="c6ef5-116">Web API 2</span></span>

<span data-ttu-id="c6ef5-117">Finden Sie unter [erstellen Sie eine Web-API mit ASP.NET Core und Visual Studio für Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) für eine neuere Version dieses Lernprogramms.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-117">See [Create a web API with ASP.NET Core and Visual Studio for Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) for a newer version of this tutorial.</span></span>

## <a name="create-a-web-api-project"></a><span data-ttu-id="c6ef5-118">Erstellen Sie eine Web-API-Projekt</span><span class="sxs-lookup"><span data-stu-id="c6ef5-118">Create a Web API Project</span></span>

<span data-ttu-id="c6ef5-119">In diesem Lernprogramm verwenden Sie ASP.NET Web-API, um eine Web-API zu erstellen, die eine Liste der Produkte zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-119">In this tutorial, you will use ASP.NET Web API to create a web API that returns a list of products.</span></span> <span data-ttu-id="c6ef5-120">Die Webseite Front-End-verwendet jQuery, um die Ergebnisse anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-120">The front-end web page uses jQuery to display the results.</span></span>

![](tutorial-your-first-web-api/_static/image1.png)

<span data-ttu-id="c6ef5-121">Starten Sie Visual Studio, und wählen Sie **neues Projekt** aus der **starten** Seite.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-121">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="c6ef5-122">Oder von der **Datei** klicken Sie im Menü **neu** und dann **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-122">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="c6ef5-123">In der **Vorlagen** klicken Sie im Bereich **installierte Vorlagen** und erweitern Sie die **Visual C#-** Knoten.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-123">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="c6ef5-124">Klicken Sie unter **Visual C#-** Option **Web**.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-124">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="c6ef5-125">Wählen Sie in der Liste der Projektvorlagen **ASP.NET-Webanwendung**.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-125">In the list of project templates, select **ASP.NET Web Application**.</span></span> <span data-ttu-id="c6ef5-126">Nennen Sie das Projekt "ProductsApp", und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-126">Name the project "ProductsApp" and click **OK**.</span></span>

![](tutorial-your-first-web-api/_static/image2.png)

<span data-ttu-id="c6ef5-127">In der **neues ASP.NET-Projekt** wählen Sie im Dialogfeld die **leere** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-127">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="c6ef5-128">Klicken Sie unter &quot;Hinzufügen von Ordnern und Verweise für core&quot;, überprüfen Sie **Web-API**.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-128">Under &quot;Add folders and core references for&quot;, check **Web API**.</span></span> <span data-ttu-id="c6ef5-129">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-129">Click **OK**.</span></span>

![](tutorial-your-first-web-api/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="c6ef5-130">Sie können auch erstellen, ein Web-API-Projekt mit der &quot;Web-API&quot; Vorlage.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-130">You can also create a Web API project using the &quot;Web API&quot; template.</span></span> <span data-ttu-id="c6ef5-131">Die Web-API-Vorlage verwendet ASP.NET MVC, um API-Hilfeseiten zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-131">The Web API template uses ASP.NET MVC to provide API help pages.</span></span> <span data-ttu-id="c6ef5-132">Ich bin die leere Vorlage für dieses Lernprogramm verwenden, da Web-API ohne MVC angezeigt werden soll.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-132">I'm using the Empty template for this tutorial because I want to show Web API without MVC.</span></span> <span data-ttu-id="c6ef5-133">Im Allgemeinen müssen Sie nicht wissen, ASP.NET MVC, Web-API verwenden.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-133">In general, you don't need to know ASP.NET MVC to use Web API.</span></span>


## <a name="adding-a-model"></a><span data-ttu-id="c6ef5-134">Hinzufügen eines Modells</span><span class="sxs-lookup"><span data-stu-id="c6ef5-134">Adding a Model</span></span>

<span data-ttu-id="c6ef5-135">Ein *Modell* ist ein Objekt, das die Daten in Ihrer Anwendung darstellt.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-135">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="c6ef5-136">ASP.NET Web-API können automatisch das Modell, um JSON-, XML- oder ein anderes Format zu serialisieren und Schreiben Sie die serialisierten Daten in den Text der HTTP-Antwortnachricht.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-136">ASP.NET Web API can automatically serialize your model to JSON, XML, or some other format, and then write the serialized data into the body of the HTTP response message.</span></span> <span data-ttu-id="c6ef5-137">Solange ein Client das Serialisierungsformat lesen kann, können sie das Objekt deserialisieren.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-137">As long as a client can read the serialization format, it can deserialize the object.</span></span> <span data-ttu-id="c6ef5-138">Die meisten Clients können Sie XML oder JSON analysiert werden.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-138">Most clients can parse either XML or JSON.</span></span> <span data-ttu-id="c6ef5-139">Darüber hinaus kann der Client der Formatelemente anzugeben durch Festlegen des Accept-Headers in der HTTP-Anforderungsnachricht.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-139">Moreover, the client can indicate which format it wants by setting the Accept header in the HTTP request message.</span></span>

<span data-ttu-id="c6ef5-140">Zunächst erstellen ein einfaches Modell, das ein Produkt darstellt.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-140">Let's start by creating a simple model that represents a product.</span></span>

<span data-ttu-id="c6ef5-141">Wenn der Projektmappen-Explorer nicht sichtbar ist, klicken Sie auf die **Ansicht** Menü **Projektmappen-Explorer**.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-141">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="c6ef5-142">Im Projektmappen-Explorer mit der rechten Maustaste Ordner Models.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-142">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="c6ef5-143">Wählen Sie im Kontextmenü der **hinzufügen** wählen Sie dann **Klasse**.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-143">From the context menu, select **Add** then select **Class**.</span></span>

![](tutorial-your-first-web-api/_static/image4.png)

<span data-ttu-id="c6ef5-144">Benennen Sie die Klasse &quot;Produkt&quot;.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-144">Name the class &quot;Product&quot;.</span></span> <span data-ttu-id="c6ef5-145">Fügen Sie die folgenden Eigenschaften auf der `Product` Klasse.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-145">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample1.cs)]

## <a name="adding-a-controller"></a><span data-ttu-id="c6ef5-146">Hinzufügen eines Controllers</span><span class="sxs-lookup"><span data-stu-id="c6ef5-146">Adding a Controller</span></span>

<span data-ttu-id="c6ef5-147">In der Web-API eine *Controller* ist ein Objekt, das HTTP-Anforderungen verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-147">In Web API, a *controller* is an object that handles HTTP requests.</span></span> <span data-ttu-id="c6ef5-148">Wir werden einen Controller hinzufügen, der eine Liste mit Produkten oder ein einzelnes Produkt, das durch ID angegebene zurückgeben</span><span class="sxs-lookup"><span data-stu-id="c6ef5-148">We'll add a controller that can return either a list of products or a single product specified by ID.</span></span>

> [!NOTE]
> <span data-ttu-id="c6ef5-149">Wenn Sie ASP.NET MVC verwendet haben, sind Sie bereits mit Domänencontrollern vertraut.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-149">If you have used ASP.NET MVC, you are already familiar with controllers.</span></span> <span data-ttu-id="c6ef5-150">Web-API-Controller mit MVC-Controller vergleichbar sind, aber erben die **ApiController** -Klasse statt der **Controller** Klasse.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-150">Web API controllers are similar to MVC controllers, but inherit the **ApiController** class instead of the **Controller** class.</span></span>

<span data-ttu-id="c6ef5-151">In **Projektmappen-Explorer**, mit der rechten Maustaste in des Ordners Controllers.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-151">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="c6ef5-152">Wählen Sie **hinzufügen** und wählen Sie dann **Controller**.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-152">Select **Add** and then select **Controller**.</span></span>

![](tutorial-your-first-web-api/_static/image5.png)

<span data-ttu-id="c6ef5-153">In der **Gerüst hinzufügen** wählen Sie im Dialogfeld **Web-API-Controller - leere**.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-153">In the **Add Scaffold** dialog, select **Web API Controller - Empty**.</span></span> <span data-ttu-id="c6ef5-154">Klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-154">Click **Add**.</span></span>

![](tutorial-your-first-web-api/_static/image6.png)

<span data-ttu-id="c6ef5-155">In der **Controller hinzufügen** Dialogfeld Namen des Controllers &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-155">In the **Add Controller** dialog, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="c6ef5-156">Klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-156">Click **Add**.</span></span>

![](tutorial-your-first-web-api/_static/image7.png)

<span data-ttu-id="c6ef5-157">Das Gerüst erstellt eine Datei namens ProductsController.cs im Ordner "Controller".</span><span class="sxs-lookup"><span data-stu-id="c6ef5-157">The scaffolding creates a file named ProductsController.cs in the Controllers folder.</span></span>

![](tutorial-your-first-web-api/_static/image8.png)

> [!NOTE]
> <span data-ttu-id="c6ef5-158">Sie müssen Ihren Domänencontrollern in einen Ordner namens Controller gestellt.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-158">You don't need to put your controllers into a folder named Controllers.</span></span> <span data-ttu-id="c6ef5-159">Der Ordnername ist eine komfortable Methode, um Ihre Quelldateien zu organisieren.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-159">The folder name is just a convenient way to organize your source files.</span></span>


<span data-ttu-id="c6ef5-160">Wenn diese Datei noch nicht geöffnet ist, doppelklicken Sie auf die Datei, um ihn zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-160">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="c6ef5-161">Ersetzen Sie den Code in dieser Datei durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="c6ef5-161">Replace the code in this file with the following:</span></span>

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample2.cs)]

<span data-ttu-id="c6ef5-162">Um das Beispiel einfach zu halten, werden Produkte in einem Array fester Größe innerhalb der Controllerklasse gespeichert.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-162">To keep the example simple, products are stored in a fixed array inside the controller class.</span></span> <span data-ttu-id="c6ef5-163">In einer realen Anwendung würde Sie natürlich Abfragen einer Datenbank oder einer anderen externen Datenquelle verwenden.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-163">Of course, in a real application, you would query a database or use some other external data source.</span></span>

<span data-ttu-id="c6ef5-164">Der Controller definiert zwei Methoden, die Produkte zurückgibt:</span><span class="sxs-lookup"><span data-stu-id="c6ef5-164">The controller defines two methods that return products:</span></span>

- <span data-ttu-id="c6ef5-165">Die `GetAllProducts` Methodenrückgabe die gesamte Liste der Produkte als ein **IEnumerable&lt;Produkt&gt;**  Typ.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-165">The `GetAllProducts` method returns the entire list of products as an **IEnumerable&lt;Product&gt;** type.</span></span>
- <span data-ttu-id="c6ef5-166">Die `GetProduct` Methode sucht ein einzelnes Produkt nach seiner ID.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-166">The `GetProduct` method looks up a single product by its ID.</span></span>

<span data-ttu-id="c6ef5-167">Das ist alles!</span><span class="sxs-lookup"><span data-stu-id="c6ef5-167">That's it!</span></span> <span data-ttu-id="c6ef5-168">Sie verfügen über eine funktionierende-Web-API.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-168">You have a working web API.</span></span> <span data-ttu-id="c6ef5-169">Jede Methode auf dem Controller entspricht eine oder mehrere URIs:</span><span class="sxs-lookup"><span data-stu-id="c6ef5-169">Each method on the controller corresponds to one or more URIs:</span></span>

| <span data-ttu-id="c6ef5-170">Controllermethode</span><span class="sxs-lookup"><span data-stu-id="c6ef5-170">Controller Method</span></span> | <span data-ttu-id="c6ef5-171">URI</span><span class="sxs-lookup"><span data-stu-id="c6ef5-171">URI</span></span> |
| --- | --- |
| <span data-ttu-id="c6ef5-172">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="c6ef5-172">GetAllProducts</span></span> | <span data-ttu-id="c6ef5-173">/api/products</span><span class="sxs-lookup"><span data-stu-id="c6ef5-173">/api/products</span></span> |
| <span data-ttu-id="c6ef5-174">GetProduct</span><span class="sxs-lookup"><span data-stu-id="c6ef5-174">GetProduct</span></span> | <span data-ttu-id="c6ef5-175">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="c6ef5-175">/api/products/*id*</span></span> |

<span data-ttu-id="c6ef5-176">Für die `GetProduct` -Methode, die *Id* im URI ist ein Platzhalter.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-176">For the `GetProduct` method, the *id* in the URI is a placeholder.</span></span> <span data-ttu-id="c6ef5-177">Um das Produkt mit der ID 5 zu erhalten, ist der URI z. B. `api/products/5`.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-177">For example, to get the product with ID of 5, the URI is `api/products/5`.</span></span>

<span data-ttu-id="c6ef5-178">Weitere Informationen dazu, wie die Web-API-HTTP-Anforderungen an Controllermethoden weiterleitet, finden Sie unter [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="c6ef5-178">For more information about how Web API routes HTTP requests to controller methods, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="calling-the-web-api-with-javascript-and-jquery"></a><span data-ttu-id="c6ef5-179">Aufrufen der Web-API mit Javascript und jQuery</span><span class="sxs-lookup"><span data-stu-id="c6ef5-179">Calling the Web API with Javascript and jQuery</span></span>

<span data-ttu-id="c6ef5-180">In diesem Abschnitt fügen wir eine HTML-Seite, die von AJAX verwendet wird, um die Web-API aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-180">In this section, we'll add an HTML page that uses AJAX to call the web API.</span></span> <span data-ttu-id="c6ef5-181">Wir verwenden jQuery die AJAX-Aufrufe ausführen und die Seite mit den Ergebnissen zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-181">We'll use jQuery to make the AJAX calls and also to update the page with the results.</span></span>

<span data-ttu-id="c6ef5-182">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste des Projekts, und wählen **hinzufügen**, und wählen Sie dann **neues Element**.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-182">In Solution Explorer, right-click the project and select **Add**, then select **New Item**.</span></span>

![](tutorial-your-first-web-api/_static/image9.png)

<span data-ttu-id="c6ef5-183">In der **neues Element hinzufügen** wählen Sie im Dialogfeld die **Web** Knoten unter **Visual C#-**, und wählen Sie dann die **HTML-Seite** Element.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-183">In the **Add New Item** dialog, select the **Web** node under **Visual C#**, and then select the **HTML Page** item.</span></span> <span data-ttu-id="c6ef5-184">Nennen Sie die Seite &quot;"Index.HTML"&quot;.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-184">Name the page &quot;index.html&quot;.</span></span>

![](tutorial-your-first-web-api/_static/image10.png)

<span data-ttu-id="c6ef5-185">Ersetzen Sie alles in dieser Datei durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="c6ef5-185">Replace everything in this file with the following:</span></span>

[!code-html[Main](tutorial-your-first-web-api/samples/sample3.html)]

<span data-ttu-id="c6ef5-186">Es gibt mehrere Möglichkeiten, jQuery zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-186">There are several ways to get jQuery.</span></span> <span data-ttu-id="c6ef5-187">In diesem Beispiel verwendet die [Microsoft Ajax-CDN](../../../ajax/cdn/overview.md).</span><span class="sxs-lookup"><span data-stu-id="c6ef5-187">In this example, I used the [Microsoft Ajax CDN](../../../ajax/cdn/overview.md).</span></span> <span data-ttu-id="c6ef5-188">Sie können auch herunterladen von [ http://jquery.com/ ](http://jquery.com/), und das ASP.NET "Web-API"-Projektvorlage enthält auch jQuery.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-188">You can also download it from [http://jquery.com/](http://jquery.com/), and the ASP.NET "Web API" project template includes jQuery as well.</span></span>

### <a name="getting-a-list-of-products"></a><span data-ttu-id="c6ef5-189">Abrufen einer Liste von Produkten</span><span class="sxs-lookup"><span data-stu-id="c6ef5-189">Getting a List of Products</span></span>

<span data-ttu-id="c6ef5-190">Um eine Liste der Produkte zu erhalten, senden Sie eine HTTP GET-Anforderung an &quot;/api/Products&quot;.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-190">To get a list of products, send an HTTP GET request to &quot;/api/products&quot;.</span></span>

<span data-ttu-id="c6ef5-191">Die jQuery [GetJSON](http://api.jquery.com/jQuery.getJSON/) -Funktion sendet eine AJAX-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-191">The jQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) function sends an AJAX request.</span></span> <span data-ttu-id="c6ef5-192">Für die Antwort Array von JSON-Objekten enthält.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-192">For response contains array of JSON objects.</span></span> <span data-ttu-id="c6ef5-193">Die `done` -Funktion gibt an, einen Rückruf, der aufgerufen wird, wenn die Anforderung erfolgreich ist.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-193">The `done` function specifies a callback that is called if the request succeeds.</span></span> <span data-ttu-id="c6ef5-194">Der Rückruf aktualisieren wir das DOM mit den Produktinformationen.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-194">In the callback, we update the DOM with the product information.</span></span>

[!code-html[Main](tutorial-your-first-web-api/samples/sample4.html)]

### <a name="getting-a-product-by-id"></a><span data-ttu-id="c6ef5-195">Abrufen eines Produkts nach ID</span><span class="sxs-lookup"><span data-stu-id="c6ef5-195">Getting a Product By ID</span></span>

<span data-ttu-id="c6ef5-196">Um ein Produkt nach ID zu erhalten, senden Sie eine HTTP GET-Anforderung an &quot;/API/Produkte/*Id*&quot;, wobei *Id* wird die Produkt-ID</span><span class="sxs-lookup"><span data-stu-id="c6ef5-196">To get a product by ID, send an HTTP GET request to &quot;/api/products/*id*&quot;, where *id* is the product ID.</span></span>

[!code-javascript[Main](tutorial-your-first-web-api/samples/sample5.js)]

<span data-ttu-id="c6ef5-197">Wir rufen Sie immer noch `getJSON` , dieses Mal jedoch die AJAX-Anforderung senden wir uns die ID im Anforderungs-URI konzentrieren.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-197">We still call `getJSON` to send the AJAX request, but this time we put the ID in the request URI.</span></span> <span data-ttu-id="c6ef5-198">Die Antwort vom diese Anforderung ist eine JSON-Darstellung eines einzelnen Produkts.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-198">The response from this request is a JSON representation of a single product.</span></span>

## <a name="running-the-application"></a><span data-ttu-id="c6ef5-199">Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="c6ef5-199">Running the Application</span></span>

<span data-ttu-id="c6ef5-200">Drücken Sie F5, um die Anwendung zu debuggen.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-200">Press F5 to start debugging the application.</span></span> <span data-ttu-id="c6ef5-201">Die Webseite sollte wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="c6ef5-201">The web page should look like the following:</span></span>

![](tutorial-your-first-web-api/_static/image11.png)

<span data-ttu-id="c6ef5-202">Um ein Produkt nach ID zu erhalten, geben Sie die ID aus, und klicken Sie auf Suchen:</span><span class="sxs-lookup"><span data-stu-id="c6ef5-202">To get a product by ID, enter the ID and click Search:</span></span>

![](tutorial-your-first-web-api/_static/image12.png)

<span data-ttu-id="c6ef5-203">Wenn Sie eine ungültige ID eingeben, gibt der Server einen HTTP-Fehler zurück:</span><span class="sxs-lookup"><span data-stu-id="c6ef5-203">If you enter an invalid ID, the server returns an HTTP error:</span></span>

![](tutorial-your-first-web-api/_static/image13.png)

## <a name="using-f12-to-view-the-http-request-and-response"></a><span data-ttu-id="c6ef5-204">Verwenden zum Anzeigen des HTTP-Anforderung und Antwort F12</span><span class="sxs-lookup"><span data-stu-id="c6ef5-204">Using F12 to View the HTTP Request and Response</span></span>

<span data-ttu-id="c6ef5-205">Bei der Arbeit mit einem HTTP-Dienst kann das finden in der HTTP-Anforderung und die Anforderungsnachrichten sehr nützlich sein.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-205">When you are working with an HTTP service, it can be very useful to see the HTTP request and request messages.</span></span> <span data-ttu-id="c6ef5-206">Sie erreichen dies, indem Sie die F12-Entwicklungstools in Internet Explorer 9.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-206">You can do this by using the F12 developer tools in Internet Explorer 9.</span></span> <span data-ttu-id="c6ef5-207">Drücken Sie in Internet Explorer 9, **F12** zu den Tools zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-207">From Internet Explorer 9, press **F12** to open the tools.</span></span> <span data-ttu-id="c6ef5-208">Klicken Sie auf die **Netzwerk** Registerkarte, und drücken Sie **starten erfassen**.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-208">Click the **Network** tab and press **Start Capturing**.</span></span> <span data-ttu-id="c6ef5-209">Wechseln Sie nun zurück an die Webseite, und drücken Sie **F5** auf der Webseite erneut geladen.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-209">Now go back to the web page and press **F5** to reload the web page.</span></span> <span data-ttu-id="c6ef5-210">Internet Explorer wird den HTTP-Datenverkehr zwischen dem Browser und dem Webserver erfassen.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-210">Internet Explorer will capture the HTTP traffic between the browser and the web server.</span></span> <span data-ttu-id="c6ef5-211">Die Zusammenfassungsansicht zeigt den Netzwerkdatenverkehr für eine Seite an:</span><span class="sxs-lookup"><span data-stu-id="c6ef5-211">The summary view shows all the network traffic for a page:</span></span>

![](tutorial-your-first-web-api/_static/image14.png)

<span data-ttu-id="c6ef5-212">Suchen Sie den Eintrag für den relativen URI "api-Produkte /".</span><span class="sxs-lookup"><span data-stu-id="c6ef5-212">Locate the entry for the relative URI "api/products/".</span></span> <span data-ttu-id="c6ef5-213">Wählen Sie diesen Eintrag, und klicken Sie auf **wechseln Sie zur detaillierten Ansicht**.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-213">Select this entry and click **Go to detailed view**.</span></span> <span data-ttu-id="c6ef5-214">In der Detailansicht sind Registerkarten, um die Anforderung und Antwort-Header und Texte anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-214">In the detail view, there are tabs to view the request and response headers and bodies.</span></span> <span data-ttu-id="c6ef5-215">Angenommen, Sie klicken Sie auf die **Anforderungsheader** Registerkarte können Sie sehen, dass die vom Client angefordert &quot;Application/Json&quot; im Accept-Header.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-215">For example, if you click the **Request headers** tab, you can see that the client requested &quot;application/json&quot; in the Accept header.</span></span>

![](tutorial-your-first-web-api/_static/image15.png)

<span data-ttu-id="c6ef5-216">Wenn Sie auf der Registerkarte "Text Antwort" klicken, können Sie sehen, wie die Produktliste in JSON serialisiert wurde.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-216">If you click the Response body tab, you can see how the product list was serialized to JSON.</span></span> <span data-ttu-id="c6ef5-217">Andere Browser verwenden eine ähnliche Funktionalität.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-217">Other browsers have similar functionality.</span></span> <span data-ttu-id="c6ef5-218">Ist ein weiteres nützliches Tool [Fiddler](http://www.fiddler2.com/fiddler2/), eine Web debugging-Proxy.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-218">Another useful tool is [Fiddler](http://www.fiddler2.com/fiddler2/), a web debugging proxy.</span></span> <span data-ttu-id="c6ef5-219">Sie können Fiddler an den HTTP-Verkehr sowie zum Verfassen von HTTP-Anforderungen, die vollständige Kontrolle über die HTTP-Header in der Anforderung enthält.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-219">You can use Fiddler to view your HTTP traffic, and also to compose HTTP requests, which gives you full control over the HTTP headers in the request.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="c6ef5-220">Finden Sie unter dieser App auf Azure ausgeführt wird</span><span class="sxs-lookup"><span data-stu-id="c6ef5-220">See this App Running on Azure</span></span>

<span data-ttu-id="c6ef5-221">Möchten Sie nicht mehr benötigen Standort ausgeführt wird, als live-Web-app angezeigt wird?</span><span class="sxs-lookup"><span data-stu-id="c6ef5-221">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="c6ef5-222">Sie können eine vollständige Version der app beim Azure-Konto bereitstellen, indem Sie einfach auf die Schaltfläche mit den folgenden.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-222">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://deploy.azure.com/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebAPI-ProductsApp#/form/setup)

<span data-ttu-id="c6ef5-223">Sie benötigen ein Azure-Konto zum Bereitstellen dieser Lösung in Azure.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-223">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="c6ef5-224">Wenn Sie nicht bereits über ein Konto verfügen, müssen Sie die folgenden Optionen aus:</span><span class="sxs-lookup"><span data-stu-id="c6ef5-224">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="c6ef5-225">[Öffnen Sie ein Azure-Konto kostenlos](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) – Sie erhalten ein Guthaben können Sie kostenpflichtige Azure-Dienste zu testen und sogar nachdem sie verwendet werden bis können Sie das Konto beibehalten und Verwendung frei Azure-Dienste.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-225">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="c6ef5-226">[MSDN-abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -Ihr MSDN-Abonnement erhalten Sie Gutschriften jedes Monats, die Sie für kostenpflichtige Azure-Dienste verwenden können.</span><span class="sxs-lookup"><span data-stu-id="c6ef5-226">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c6ef5-227">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="c6ef5-227">Next Steps</span></span>

- <span data-ttu-id="c6ef5-228">Ein ausführlicheres Beispiel für einen HTTP-Dienst, POST, PUT und DELETE-Aktionen unterstützt und schreibt in einer Datenbank, finden Sie unter [mithilfe von Web-API 2 mit Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).</span><span class="sxs-lookup"><span data-stu-id="c6ef5-228">For a more complete example of an HTTP service that supports POST, PUT, and DELETE actions and writes to a database, see [Using Web API 2 with Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).</span></span>
- <span data-ttu-id="c6ef5-229">Weitere Informationen zum Erstellen von flüssig und schnell reagierend Webanwendungen über einen HTTP-Dienst finden Sie unter [ASP.NET einseitigen Anwendung](../../../single-page-application/index.md).</span><span class="sxs-lookup"><span data-stu-id="c6ef5-229">For more about creating fluid and responsive web applications on top of an HTTP service, see [ASP.NET Single Page Application](../../../single-page-application/index.md).</span></span>
- <span data-ttu-id="c6ef5-230">Informationen zum Bereitstellen einer Visual Studio-Webprojekt nach Azure App Service finden Sie unter [erstellen eine ASP.NET Web-app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span><span class="sxs-lookup"><span data-stu-id="c6ef5-230">For information about how to deploy a Visual Studio web project to Azure App Service, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>
