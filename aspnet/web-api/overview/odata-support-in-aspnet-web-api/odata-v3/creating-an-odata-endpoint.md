---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: Erstellen einen OData-v3-Endpunkt mit Web-API 2 | Microsoft Docs
author: MikeWasson
description: Das Open Data Protocol (OData) ist eine Data Access-Protokoll für das Web. OData bietet eine einheitliche Methode zur Strukturdaten Datenabfragen durchzuführen, und die Daten bearbeiten...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 227faacd3f42731e08a4cd2b71075776309961b6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="creating-an-odata-v3-endpoint-with-web-api-2"></a><span data-ttu-id="f8a09-104">Erstellen einen OData-v3-Endpunkt mit Web-API 2</span><span class="sxs-lookup"><span data-stu-id="f8a09-104">Creating an OData v3 Endpoint with Web API 2</span></span>
====================
<span data-ttu-id="f8a09-105">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f8a09-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="f8a09-106">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="f8a09-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="f8a09-107">Die [Open Data Protocol](http://www.odata.org/) (OData) ist eine Data Access-Protokoll für das Web.</span><span class="sxs-lookup"><span data-stu-id="f8a09-107">The [Open Data Protocol](http://www.odata.org/) (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="f8a09-108">OData bietet eine einheitliche Möglichkeit zum Strukturieren von Daten, die Daten Abfragen und bearbeiten das DataSet über CRUD-Vorgänge (erstellen, lesen, aktualisieren und löschen).</span><span class="sxs-lookup"><span data-stu-id="f8a09-108">OData provides a uniform way to structure data, query the data, and manipulate the data set through CRUD operations (create, read, update, and delete).</span></span> <span data-ttu-id="f8a09-109">OData unterstützt AtomPub (XML) und JSON-Format.</span><span class="sxs-lookup"><span data-stu-id="f8a09-109">OData supports both AtomPub (XML) and JSON formats.</span></span> <span data-ttu-id="f8a09-110">OData definiert auch eine Möglichkeit, die Metadaten zu den Daten verfügbar zu machen.</span><span class="sxs-lookup"><span data-stu-id="f8a09-110">OData also defines a way to expose metadata about the data.</span></span> <span data-ttu-id="f8a09-111">Clients können die Metadaten verwenden, um die Typinformationen und Beziehungen für das Dataset zu ermitteln.</span><span class="sxs-lookup"><span data-stu-id="f8a09-111">Clients can use the metadata to discover the type information and relationships for the data set.</span></span>
> 
> <span data-ttu-id="f8a09-112">ASP.NET Web-API vereinfacht die einen OData-Endpunkt für ein Dataset zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="f8a09-112">ASP.NET Web API makes it easy to create an OData endpoint for a data set.</span></span> <span data-ttu-id="f8a09-113">Sie können steuern, genau dem OData-Vorgänge der Endpunkt unterstützt.</span><span class="sxs-lookup"><span data-stu-id="f8a09-113">You can control exactly which OData operations the endpoint supports.</span></span> <span data-ttu-id="f8a09-114">Sie können mehrere OData-Endpunkte, zusammen mit nicht-OData-Endpunkte hosten.</span><span class="sxs-lookup"><span data-stu-id="f8a09-114">You can host multiple OData endpoints, alongside non-OData endpoints.</span></span> <span data-ttu-id="f8a09-115">Sie haben die vollständige Kontrolle über Ihr Datenmodell, Back-End-Geschäftslogik und Datenschicht.</span><span class="sxs-lookup"><span data-stu-id="f8a09-115">You have full control over your data model, back-end business logic, and data layer.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f8a09-116">In diesem Lernprogramm verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="f8a09-116">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="f8a09-117">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="f8a09-117">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="f8a09-118">Web-API 2</span><span class="sxs-lookup"><span data-stu-id="f8a09-118">Web API 2</span></span>
> - <span data-ttu-id="f8a09-119">OData Version 3</span><span class="sxs-lookup"><span data-stu-id="f8a09-119">OData Version 3</span></span>
> - <span data-ttu-id="f8a09-120">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="f8a09-120">Entity Framework 6</span></span>
> - [<span data-ttu-id="f8a09-121">Fiddler Web Debugging-Proxy (Optional)</span><span class="sxs-lookup"><span data-stu-id="f8a09-121">Fiddler Web Debugging Proxy (Optional)</span></span>](http://www.fiddler2.com)
> 
> <span data-ttu-id="f8a09-122">Web API OData-Unterstützung wurde hinzugefügt, [ASP.NET und Web-Toolsupdate 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650).</span><span class="sxs-lookup"><span data-stu-id="f8a09-122">Web API OData support was added in [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="f8a09-123">Dieses Lernprogramm verwendet jedoch Gerüstbau, die in Visual Studio 2013 hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="f8a09-123">However, this tutorial uses scaffolding that was added in Visual Studio 2013.</span></span>


<span data-ttu-id="f8a09-124">In diesem Lernprogramm erstellen Sie einen einfache OData-Endpunkt, den Clients abgefragt werden können.</span><span class="sxs-lookup"><span data-stu-id="f8a09-124">In this tutorial, you will create a simple OData endpoint that clients can query.</span></span> <span data-ttu-id="f8a09-125">Außerdem erstellen Sie einen C#-Client für den Endpunkt.</span><span class="sxs-lookup"><span data-stu-id="f8a09-125">You will also create a C# client for the endpoint.</span></span> <span data-ttu-id="f8a09-126">Nach Abschluss dieses Lernprogramms wird der nächste Satz von Lernprogrammen wird gezeigt, wie weitere Funktionen, einschließlich entitätsbeziehungen, Aktionen, hinzufügen, und wählen Sie die $erweitern / $.</span><span class="sxs-lookup"><span data-stu-id="f8a09-126">After you complete this tutorial, the next set of tutorials show how to add more functionality, including entity relations, actions, and $expand/$select.</span></span>

- [<span data-ttu-id="f8a09-127">Visual Studio-Projekt erstellen</span><span class="sxs-lookup"><span data-stu-id="f8a09-127">Create the Visual Studio Project</span></span>](#create-project)
- [<span data-ttu-id="f8a09-128">Ein Entitätsmodell hinzufügen</span><span class="sxs-lookup"><span data-stu-id="f8a09-128">Add an Entity Model</span></span>](#add-model)
- [<span data-ttu-id="f8a09-129">Fügen Sie einem OData-Controller hinzu</span><span class="sxs-lookup"><span data-stu-id="f8a09-129">Add an OData Controller</span></span>](#add-controller)
- [<span data-ttu-id="f8a09-130">Hinzufügen des EDM und eine Route</span><span class="sxs-lookup"><span data-stu-id="f8a09-130">Add the EDM and Route</span></span>](#edm)
- [<span data-ttu-id="f8a09-131">Ausgangswert für die Datenbank (Optional)</span><span class="sxs-lookup"><span data-stu-id="f8a09-131">Seed the Database (Optional)</span></span>](#seed-db)
- [<span data-ttu-id="f8a09-132">Untersuchen den OData-Endpunkt</span><span class="sxs-lookup"><span data-stu-id="f8a09-132">Exploring the OData Endpoint</span></span>](#explore)
- [<span data-ttu-id="f8a09-133">OData-Serialisierungsformate</span><span class="sxs-lookup"><span data-stu-id="f8a09-133">OData Serialization Formats</span></span>](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a><span data-ttu-id="f8a09-134">Visual Studio-Projekt erstellen</span><span class="sxs-lookup"><span data-stu-id="f8a09-134">Create the Visual Studio Project</span></span>

<span data-ttu-id="f8a09-135">In diesem Lernprogramm erstellen Sie einen OData-Endpunkt, der grundlegende CRUD-Vorgänge unterstützt.</span><span class="sxs-lookup"><span data-stu-id="f8a09-135">In this tutorial, you will create an OData endpoint that supports basic CRUD operations.</span></span> <span data-ttu-id="f8a09-136">Der Endpunkt wird eine einzelne Ressource, eine Liste der Produkte verfügbar machen.</span><span class="sxs-lookup"><span data-stu-id="f8a09-136">The endpoint will expose a single resource, a list of products.</span></span> <span data-ttu-id="f8a09-137">Spätere Lernprogrammen werden weitere Funktionen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="f8a09-137">Later tutorials will add more features.</span></span>

<span data-ttu-id="f8a09-138">Starten Sie Visual Studio, und wählen Sie **neues Projekt** von der Startseite.</span><span class="sxs-lookup"><span data-stu-id="f8a09-138">Start Visual Studio and select **New Project** from the Start page.</span></span> <span data-ttu-id="f8a09-139">Oder von der **Datei** klicken Sie im Menü **neu** und dann **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="f8a09-139">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="f8a09-140">In der **Vorlagen** klicken Sie im Bereich **installierte Vorlagen** und erweitern Sie den Visual C#-Knoten.</span><span class="sxs-lookup"><span data-stu-id="f8a09-140">In the **Templates** pane, select **Installed Templates** and expand the Visual C# node.</span></span> <span data-ttu-id="f8a09-141">Klicken Sie unter **Visual C#-**Option **Web**.</span><span class="sxs-lookup"><span data-stu-id="f8a09-141">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="f8a09-142">Wählen Sie **ASP.NET-Webanwendung** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="f8a09-142">Select **the ASP.NET Web Application** template.</span></span>

![](creating-an-odata-endpoint/_static/image1.png)

<span data-ttu-id="f8a09-143">In der **neues ASP.NET-Projekt** wählen Sie im Dialogfeld die **leere** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="f8a09-143">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="f8a09-144">Unter &quot;Hinzufügen von Ordnern und Verweise für core... &quot;, überprüfen Sie **Web-API**.</span><span class="sxs-lookup"><span data-stu-id="f8a09-144">Under &quot;Add folders and core references for...&quot;, check **Web API**.</span></span> <span data-ttu-id="f8a09-145">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="f8a09-145">Click **OK**.</span></span>

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a><span data-ttu-id="f8a09-146">Ein Entitätsmodell hinzufügen</span><span class="sxs-lookup"><span data-stu-id="f8a09-146">Add an Entity Model</span></span>

<span data-ttu-id="f8a09-147">Ein *Modell* ist ein Objekt, das die Daten in Ihrer Anwendung darstellt.</span><span class="sxs-lookup"><span data-stu-id="f8a09-147">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="f8a09-148">Für dieses Lernprogramm benötigen wir ein Modell, das ein Produkt darstellt.</span><span class="sxs-lookup"><span data-stu-id="f8a09-148">For this tutorial, we need a model that represents a product.</span></span> <span data-ttu-id="f8a09-149">Das Modell entspricht unserer OData-Entitätstyp.</span><span class="sxs-lookup"><span data-stu-id="f8a09-149">The model corresponds to our OData entity type.</span></span>

<span data-ttu-id="f8a09-150">Im Projektmappen-Explorer mit der rechten Maustaste Ordner Models.</span><span class="sxs-lookup"><span data-stu-id="f8a09-150">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="f8a09-151">Wählen Sie im Kontextmenü der **hinzufügen** wählen Sie dann **Klasse**.</span><span class="sxs-lookup"><span data-stu-id="f8a09-151">From the context menu, select **Add** then select **Class**.</span></span>

![](creating-an-odata-endpoint/_static/image3.png)

<span data-ttu-id="f8a09-152">In der **hinzufügen** -Element Dialogfeld aus, nennen Sie die Klasse &quot;Produkt&quot;.</span><span class="sxs-lookup"><span data-stu-id="f8a09-152">In the **Add New** Item dialog, name the class &quot;Product&quot;.</span></span>

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> <span data-ttu-id="f8a09-153">Gemäß der Konvention werden Modellklassen im Ordner Models platziert.</span><span class="sxs-lookup"><span data-stu-id="f8a09-153">By convention, model classes are placed in the Models folder.</span></span> <span data-ttu-id="f8a09-154">Sie keine dieser Konvention in Ihren eigenen Projekten, aber wir es für dieses Lernprogramm verwenden.</span><span class="sxs-lookup"><span data-stu-id="f8a09-154">You don't have to follow this convention in your own projects, but we'll use it for this tutorial.</span></span>


<span data-ttu-id="f8a09-155">Fügen Sie die folgende Klassendefinition in der Datei Product.cs hinzu:</span><span class="sxs-lookup"><span data-stu-id="f8a09-155">In the Product.cs file, add the following class definition:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

<span data-ttu-id="f8a09-156">Die ID-Eigenschaft werden die Entitätsschlüssel.</span><span class="sxs-lookup"><span data-stu-id="f8a09-156">The ID property will be the entity key.</span></span> <span data-ttu-id="f8a09-157">Clients können die Produkte anhand der ID. Abfragen.</span><span class="sxs-lookup"><span data-stu-id="f8a09-157">Clients can query products by ID.</span></span> <span data-ttu-id="f8a09-158">Dieses Feld wird auch der Primärschlüssel in der Back-End-Datenbank sein.</span><span class="sxs-lookup"><span data-stu-id="f8a09-158">This field would also be the primary key in the back-end database.</span></span>

<span data-ttu-id="f8a09-159">Erstellen Sie jetzt das Projekt.</span><span class="sxs-lookup"><span data-stu-id="f8a09-159">Build the project now.</span></span> <span data-ttu-id="f8a09-160">Im nächsten Schritt verwenden wir einige Visual Studio-Gerüstbau, die Reflektion verwendet, um die Product-Typs zu suchen.</span><span class="sxs-lookup"><span data-stu-id="f8a09-160">In the next step, we'll use some Visual Studio scaffolding that uses reflection to find the Product type.</span></span>

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a><span data-ttu-id="f8a09-161">Fügen Sie einem OData-Controller hinzu</span><span class="sxs-lookup"><span data-stu-id="f8a09-161">Add an OData Controller</span></span>

<span data-ttu-id="f8a09-162">Ein *Controller* ist eine Klasse, die HTTP-Anforderungen verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="f8a09-162">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="f8a09-163">Sie definieren einen separaten Controller für jede Entität in Sie OData-Dienst festlegen.</span><span class="sxs-lookup"><span data-stu-id="f8a09-163">You define a separate controller for each entity set in you OData service.</span></span> <span data-ttu-id="f8a09-164">In diesem Lernprogramm erstellen wir einen einzelnen Controller.</span><span class="sxs-lookup"><span data-stu-id="f8a09-164">In this tutorial, we'll create a single controller.</span></span>

<span data-ttu-id="f8a09-165">Im Projektmappen-Explorer mit der rechten Maustaste des Ordners Controllers.</span><span class="sxs-lookup"><span data-stu-id="f8a09-165">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="f8a09-166">Wählen Sie **hinzufügen** und wählen Sie dann **Controller**.</span><span class="sxs-lookup"><span data-stu-id="f8a09-166">Select **Add** and then select **Controller**.</span></span>

![](creating-an-odata-endpoint/_static/image5.png)

<span data-ttu-id="f8a09-167">In der **Gerüst hinzufügen** wählen Sie im Dialogfeld &quot;Web API 2 OData-Controller mit Aktionen unter Verwendung von Entity Framework&quot;.</span><span class="sxs-lookup"><span data-stu-id="f8a09-167">In the **Add Scaffold** dialog, select &quot;Web API 2 OData Controller with actions, using Entity Framework&quot;.</span></span>

![](creating-an-odata-endpoint/_static/image6.png)

<span data-ttu-id="f8a09-168">In der **Controller hinzufügen** Dialogfeld Namen des Controllers "ProductsController".</span><span class="sxs-lookup"><span data-stu-id="f8a09-168">In the **Add Controller** dialog, name the controller "ProductsController".</span></span> <span data-ttu-id="f8a09-169">Wählen Sie die &quot;asynchrone Controlleraktionen verwenden&quot; Kontrollkästchen.</span><span class="sxs-lookup"><span data-stu-id="f8a09-169">Select the &quot;Use async controller actions&quot; checkbox.</span></span> <span data-ttu-id="f8a09-170">In der **Modell** Dropdown-Liste, wählen Sie die Product-Klasse.</span><span class="sxs-lookup"><span data-stu-id="f8a09-170">In the **Model** drop-down list, select the Product class.</span></span>

![](creating-an-odata-endpoint/_static/image7.png)

<span data-ttu-id="f8a09-171">Klicken Sie auf die **neuen Datenkontext...**  Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="f8a09-171">Click the **New data context...** button.</span></span> <span data-ttu-id="f8a09-172">Lassen Sie den Standardnamen für den Datentyp für den Kontext, und klicken Sie auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="f8a09-172">Leave the default name for the data context type, and click **Add**.</span></span>

![](creating-an-odata-endpoint/_static/image8.png)

<span data-ttu-id="f8a09-173">Klicken Sie auf "hinzufügen", die im Dialogfeld Controller hinzufügen "auf den Controller hinzufügen".</span><span class="sxs-lookup"><span data-stu-id="f8a09-173">Click Add in the Add Controller dialog to add the controller.</span></span>

![](creating-an-odata-endpoint/_static/image9.png)

<span data-ttu-id="f8a09-174">Hinweis: Wenn Sie eine Fehlermeldung erhalten, der besagt, dass &quot;es wurde ein Fehler beim Abrufen des Typs... &quot;, stellen Sie sicher, dass Sie das Visual Studio-Projekt erstellt, nachdem Sie die Produktklasse hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="f8a09-174">Note: If you get an error message that says &quot;There was an error getting the type...&quot;, make sure that you built the Visual Studio project after you added the Product class.</span></span> <span data-ttu-id="f8a09-175">Das Gerüst verwendet Reflektion, um die Klasse zu suchen.</span><span class="sxs-lookup"><span data-stu-id="f8a09-175">The scaffolding uses reflection to find the class.</span></span>

![](creating-an-odata-endpoint/_static/image10.png)

<span data-ttu-id="f8a09-176">Das Gerüst fügt zwei Codedateien zum Projekt hinzu:</span><span class="sxs-lookup"><span data-stu-id="f8a09-176">The scaffolding adds two code files to the project:</span></span>

- <span data-ttu-id="f8a09-177">Products.cs definiert, die Web-API-Controller, der den OData-Endpunkt implementiert wird.</span><span class="sxs-lookup"><span data-stu-id="f8a09-177">Products.cs defines the Web API controller that implements the OData endpoint.</span></span>
- <span data-ttu-id="f8a09-178">ProductServiceContext.cs stellt Methoden zum Abfragen der zugrunde liegenden Datenbank, die Verwendung von Entity Framework bereit.</span><span class="sxs-lookup"><span data-stu-id="f8a09-178">ProductServiceContext.cs provides methods to query the underlying database, using Entity Framework.</span></span>

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a><span data-ttu-id="f8a09-179">Hinzufügen des EDM und eine Route</span><span class="sxs-lookup"><span data-stu-id="f8a09-179">Add the EDM and Route</span></span>

<span data-ttu-id="f8a09-180">Erweitern Sie im Projektmappen-Explorer die App\_starten Sie die Ordner, und öffnen Sie die Datei mit dem Namen WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="f8a09-180">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="f8a09-181">Diese Klasse enthält den Konfigurationscode für die Web-API.</span><span class="sxs-lookup"><span data-stu-id="f8a09-181">This class holds configuration code for Web API.</span></span> <span data-ttu-id="f8a09-182">Ersetzen Sie diesen Code durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="f8a09-182">Replace this code with the following:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

<span data-ttu-id="f8a09-183">Dieser Code führt zwei Aufgaben aus:</span><span class="sxs-lookup"><span data-stu-id="f8a09-183">This code does two things:</span></span>

- <span data-ttu-id="f8a09-184">Erstellt ein Entity Data Model (EDM) für den OData-Endpunkt an.</span><span class="sxs-lookup"><span data-stu-id="f8a09-184">Creates an Entity Data Model (EDM) for the OData endpoint.</span></span>
- <span data-ttu-id="f8a09-185">Fügt eine Route für den Endpunkt hinzu.</span><span class="sxs-lookup"><span data-stu-id="f8a09-185">Adds a route for the endpoint.</span></span>

<span data-ttu-id="f8a09-186">Ein EDM ist eine abstrakte Modell der Daten.</span><span class="sxs-lookup"><span data-stu-id="f8a09-186">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="f8a09-187">Das EDM ist das Metadatendokument erstellen und definieren die URIs für den Dienst verwendet.</span><span class="sxs-lookup"><span data-stu-id="f8a09-187">The EDM is used to create the metadata document and define the URIs for the service.</span></span> <span data-ttu-id="f8a09-188">Die **ODataConventionModelBuilder** EDM werden anhand eines Satzes von Standard-Benennungskonventionen EDM erstellt.</span><span class="sxs-lookup"><span data-stu-id="f8a09-188">The **ODataConventionModelBuilder** creates an EDM by using a set of default naming conventions EDM.</span></span> <span data-ttu-id="f8a09-189">Dieser Ansatz erfordert den geringsten Code.</span><span class="sxs-lookup"><span data-stu-id="f8a09-189">This approach requires the least code.</span></span> <span data-ttu-id="f8a09-190">Wenn Sie mehr Kontrolle über das EDM soll, können Sie mithilfe der **ODataModelBuilder** Klasse EDM zu erstellen, indem Sie die Eigenschaften, Schlüssel und Navigationseigenschaften explizit hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="f8a09-190">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="f8a09-191">Die **EntitySet** Methode fügt eine Entität, auf das EDM festgelegt:</span><span class="sxs-lookup"><span data-stu-id="f8a09-191">The **EntitySet** method adds an entity set to the EDM:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

<span data-ttu-id="f8a09-192">Die Zeichenfolge "Products" definiert den Namen der Entitätenmenge.</span><span class="sxs-lookup"><span data-stu-id="f8a09-192">The string "Products" defines the name of the entity set.</span></span> <span data-ttu-id="f8a09-193">Der Name des Controllers muss der Name der Entitätenmenge überein.</span><span class="sxs-lookup"><span data-stu-id="f8a09-193">The name of the controller must match the name of the entity set.</span></span> <span data-ttu-id="f8a09-194">In diesem Lernprogramm wird die Entitätenmenge "Products" heißt und der Controller `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="f8a09-194">In this tutorial, the entity set is named "Products" and the controller is named `ProductsController`.</span></span> <span data-ttu-id="f8a09-195">Wenn Sie mit dem Namen die Entität "ProductSet", nennen Sie den Controller `ProductSetController`.</span><span class="sxs-lookup"><span data-stu-id="f8a09-195">If you named the entity set "ProductSet", you would name the controller `ProductSetController`.</span></span> <span data-ttu-id="f8a09-196">Beachten Sie, dass ein Endpunkt mehrere Entitätenmengen verfügen kann.</span><span class="sxs-lookup"><span data-stu-id="f8a09-196">Note that an endpoint can have multiple entity sets.</span></span> <span data-ttu-id="f8a09-197">Rufen Sie **EntitySet&lt;T&gt;**  für jede Entität festlegen, und definieren Sie einen entsprechenden Controller.</span><span class="sxs-lookup"><span data-stu-id="f8a09-197">Call **EntitySet&lt;T&gt;** for each entity set, and then define a corresponding controller.</span></span>

<span data-ttu-id="f8a09-198">Die **MapODataRoute** Methode fügt eine Route für den OData-Endpunkt.</span><span class="sxs-lookup"><span data-stu-id="f8a09-198">The **MapODataRoute** method adds a route for the OData endpoint.</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

<span data-ttu-id="f8a09-199">Der erste Parameter ist ein Anzeigename für die Route.</span><span class="sxs-lookup"><span data-stu-id="f8a09-199">The first parameter is a friendly name for the route.</span></span> <span data-ttu-id="f8a09-200">Dieser Name werden von Clients des Diensts nicht angezeigt.</span><span class="sxs-lookup"><span data-stu-id="f8a09-200">Clients of your service do not see this name.</span></span> <span data-ttu-id="f8a09-201">Der zweite Parameter ist der URI-Präfix für den Endpunkt.</span><span class="sxs-lookup"><span data-stu-id="f8a09-201">The second parameter is the URI prefix for the endpoint.</span></span> <span data-ttu-id="f8a09-202">Wenn Sie diesen Code, der URI für die Produkte Entitätenmenge lautet http://<em>Hostname</em>  /Odata/Produkte.</span><span class="sxs-lookup"><span data-stu-id="f8a09-202">Given this code, the URI for the Products entity set is http://<em>hostname</em>/odata/Products.</span></span> <span data-ttu-id="f8a09-203">Ihre Anwendung kann mehr als ein OData-Endpunkt besitzen.</span><span class="sxs-lookup"><span data-stu-id="f8a09-203">Your application can have more than one OData endpoint.</span></span> <span data-ttu-id="f8a09-204">Rufen Sie für jeden Endpunkt <strong>MapODataRoute</strong> , und geben Sie einen eindeutigen Routennamen und einen eindeutigen URI-Präfix.</span><span class="sxs-lookup"><span data-stu-id="f8a09-204">For each endpoint, call <strong>MapODataRoute</strong> and provide a unique route name and a unique URI prefix.</span></span>

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a><span data-ttu-id="f8a09-205">Ausgangswert für die Datenbank (Optional)</span><span class="sxs-lookup"><span data-stu-id="f8a09-205">Seed the Database (Optional)</span></span>

<span data-ttu-id="f8a09-206">In diesem Schritt verwenden Sie Entity Framework, um die Datenbank mit einigen Testdaten Startwerten.</span><span class="sxs-lookup"><span data-stu-id="f8a09-206">In this step, you will use Entity Framework to seed the database with some test data.</span></span> <span data-ttu-id="f8a09-207">Dieser Schritt ist optional, aber sie können die OData-Endpunkt sofort zu testen.</span><span class="sxs-lookup"><span data-stu-id="f8a09-207">This step is optional, but it lets you test out your OData endpoint right away.</span></span>

<span data-ttu-id="f8a09-208">Aus der **Tools** klicken Sie im Menü **Bibliothekspaket-Manager**, und wählen Sie dann **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="f8a09-208">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="f8a09-209">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="f8a09-209">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

<span data-ttu-id="f8a09-210">Dadurch wird einen Ordner namens Migrationen und eine Codedatei mit dem Namen "Configuration.cs" hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="f8a09-210">This adds a folder named Migrations and a code file named Configuration.cs.</span></span>

![](creating-an-odata-endpoint/_static/image12.png)

<span data-ttu-id="f8a09-211">Öffnen Sie diese Datei, und fügen Sie folgenden Code zu der `Configuration.Seed` Methode.</span><span class="sxs-lookup"><span data-stu-id="f8a09-211">Open this file and add the following code to the `Configuration.Seed` method.</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

<span data-ttu-id="f8a09-212">Geben Sie im Fenster Paket-Manager-Konsole die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="f8a09-212">In the Package Manager Console Window, enter the following commands:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

<span data-ttu-id="f8a09-213">Diese Befehle generiert Code, der die Datenbank erstellt und führt dann diesen Code.</span><span class="sxs-lookup"><span data-stu-id="f8a09-213">These commands generate code that creates the database, and then executes that code.</span></span>

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a><span data-ttu-id="f8a09-214">Untersuchen den OData-Endpunkt</span><span class="sxs-lookup"><span data-stu-id="f8a09-214">Exploring the OData Endpoint</span></span>

<span data-ttu-id="f8a09-215">In diesem Abschnitt verwenden wir die [Fiddler Webproxy Debuggen](http://www.fiddler2.com) Anforderungen an den Endpunkt gesendet werden, und überprüfen Sie die Antwortnachrichten.</span><span class="sxs-lookup"><span data-stu-id="f8a09-215">In this section, we'll use the [Fiddler Web Debugging Proxy](http://www.fiddler2.com) to send requests to the endpoint and examine the response messages.</span></span> <span data-ttu-id="f8a09-216">Dies hilft Ihnen beim Verständnis der Funktionen von einem OData-Endpunkt.</span><span class="sxs-lookup"><span data-stu-id="f8a09-216">This will help you to understand the capabilities of an OData endpoint.</span></span>

<span data-ttu-id="f8a09-217">Drücken Sie F5, um mit dem Debuggen beginnen, in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f8a09-217">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="f8a09-218">Standardmäßig wird Visual Studio in Ihrem Browser geöffnet, mit denen `http://localhost:*port*`, wobei *Port* ist die Portnummer, die in den projekteinstellungen konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="f8a09-218">By default, Visual Studio opens your browser to `http://localhost:*port*`, where *port* is the port number configured in the project settings.</span></span>

<span data-ttu-id="f8a09-219">Sie können die Portnummer in den projekteinstellungen ändern.</span><span class="sxs-lookup"><span data-stu-id="f8a09-219">You can change the port number in the project settings.</span></span> <span data-ttu-id="f8a09-220">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste des Projekts, und wählen **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="f8a09-220">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="f8a09-221">Wählen Sie im Eigenschaftenfenster **Web**.</span><span class="sxs-lookup"><span data-stu-id="f8a09-221">In the properties window, select **Web**.</span></span> <span data-ttu-id="f8a09-222">Geben Sie die Portnummer unter **Projekt-Url**.</span><span class="sxs-lookup"><span data-stu-id="f8a09-222">Enter the port number under **Project Url**.</span></span>

### <a name="service-document"></a><span data-ttu-id="f8a09-223">-Dienstdokument</span><span class="sxs-lookup"><span data-stu-id="f8a09-223">Service Document</span></span>

<span data-ttu-id="f8a09-224">Die *dienstdokument* enthält eine Liste von Entitätenmengen für den OData-Endpunkt.</span><span class="sxs-lookup"><span data-stu-id="f8a09-224">The *service document* contains a list of the entity sets for the OData endpoint.</span></span> <span data-ttu-id="f8a09-225">Um das dienstdokument zu erhalten, senden Sie eine GET-Anforderung an die Stamm-URI des Diensts ein.</span><span class="sxs-lookup"><span data-stu-id="f8a09-225">To get the service document, send a GET request to the root URI of the service.</span></span>

<span data-ttu-id="f8a09-226">Mit Fiddler, geben Sie den folgenden URI in die **Composer** Registerkarte: `http://localhost:port/odata/`, wobei *Port* ist die Portnummer.</span><span class="sxs-lookup"><span data-stu-id="f8a09-226">Using Fiddler, enter the following URI in the **Composer** tab: `http://localhost:port/odata/`, where *port* is the port number.</span></span>

![](creating-an-odata-endpoint/_static/image13.png)

<span data-ttu-id="f8a09-227">Klicken Sie auf die **Execute** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="f8a09-227">Click the **Execute** button.</span></span> <span data-ttu-id="f8a09-228">Fiddler sendet eine HTTP GET-Anforderung an die Anwendung.</span><span class="sxs-lookup"><span data-stu-id="f8a09-228">Fiddler sends an HTTP GET request to your application.</span></span> <span data-ttu-id="f8a09-229">Die Antwort in der Liste der Web-Sitzungen sollte angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="f8a09-229">You should see the response in the Web Sessions list.</span></span> <span data-ttu-id="f8a09-230">Wenn alles funktioniert, wird der Statuscode 200 sein.</span><span class="sxs-lookup"><span data-stu-id="f8a09-230">If everything is working, the status code will be 200.</span></span>

![](creating-an-odata-endpoint/_static/image14.png)

<span data-ttu-id="f8a09-231">Doppelklicken Sie auf die Antwort in der Liste Web Sitzungen, um die Details der Antwort auf die Registerkarte "Inspektoren" anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="f8a09-231">Double-click the response in the Web Sessions list to see the details of the response message in the Inspectors tab.</span></span>

![](creating-an-odata-endpoint/_static/image15.png)

<span data-ttu-id="f8a09-232">Die unformatierte HTTP-Antwortnachricht sollte etwa wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="f8a09-232">The raw HTTP response message should look similar to the following:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

<span data-ttu-id="f8a09-233">Standardmäßig werden mit Web-API das dienstdokument in AtomPub-Format zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="f8a09-233">By default, Web API returns the service document in AtomPub format.</span></span> <span data-ttu-id="f8a09-234">Um JSON-Anforderung, fügen Sie die folgende Kopfzeile auf die HTTP-Anforderung:</span><span class="sxs-lookup"><span data-stu-id="f8a09-234">To request JSON, add the following header to the HTTP request:</span></span>

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

<span data-ttu-id="f8a09-235">Nachdem die HTTP-Antwort eine JSON-Nutzlast enthält:</span><span class="sxs-lookup"><span data-stu-id="f8a09-235">Now the HTTP response contains a JSON payload:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a><span data-ttu-id="f8a09-236">Mit dem Dienstmetadatendokument</span><span class="sxs-lookup"><span data-stu-id="f8a09-236">Service Metadata Document</span></span>

<span data-ttu-id="f8a09-237">Die *dienstmetadatendokument* beschreibt das Datenmodell des Diensts mithilfe einer XML-Sprache, die die Conceptual Schema Definition Language (CSDL) aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="f8a09-237">The *service metadata document* describes the data model of the service, using an XML language called the Conceptual Schema Definition Language (CSDL).</span></span> <span data-ttu-id="f8a09-238">Das Metadatendokument veranschaulicht die Struktur der Daten in den Dienst und zum Generieren von Clientcode verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="f8a09-238">The metadata document shows the structure of the data in the service, and can be used to generate client code.</span></span>

<span data-ttu-id="f8a09-239">Um das Metadatendokument zu erhalten, senden Sie eine GET-Anforderung `http://localhost:port/odata/$metadata`.</span><span class="sxs-lookup"><span data-stu-id="f8a09-239">To get the metadata document, send a GET request to `http://localhost:port/odata/$metadata`.</span></span> <span data-ttu-id="f8a09-240">Hier werden die Metadaten für den Endpunkt in diesem Lernprogramm gezeigt.</span><span class="sxs-lookup"><span data-stu-id="f8a09-240">Here is the metadata for the endpoint shown in this tutorial.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a><span data-ttu-id="f8a09-241">Entitätenmenge</span><span class="sxs-lookup"><span data-stu-id="f8a09-241">Entity Set</span></span>

<span data-ttu-id="f8a09-242">Um die Entitätenmenge für die Produkte zu erhalten, senden Sie eine GET-Anforderung `http://localhost:port/odata/Products`.</span><span class="sxs-lookup"><span data-stu-id="f8a09-242">To get the Products entity set, send a GET request to `http://localhost:port/odata/Products`.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a><span data-ttu-id="f8a09-243">Entität</span><span class="sxs-lookup"><span data-stu-id="f8a09-243">Entity</span></span>

<span data-ttu-id="f8a09-244">Um ein einzelnes Produkt zu erhalten, senden Sie eine GET-Anforderung `http://localhost:port/odata/Products(1)`, wobei "1" die Produkt-ID ist</span><span class="sxs-lookup"><span data-stu-id="f8a09-244">To get an individual product, send a GET request to `http://localhost:port/odata/Products(1)`, where "1" is the product ID.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a><span data-ttu-id="f8a09-245">OData-Serialisierungsformate</span><span class="sxs-lookup"><span data-stu-id="f8a09-245">OData Serialization Formats</span></span>

<span data-ttu-id="f8a09-246">OData unterstützt auch mehrere Serialisierungsformate:</span><span class="sxs-lookup"><span data-stu-id="f8a09-246">OData supports several serialization formats:</span></span>

- <span data-ttu-id="f8a09-247">Atom-Pub (XML)</span><span class="sxs-lookup"><span data-stu-id="f8a09-247">Atom Pub (XML)</span></span>
- <span data-ttu-id="f8a09-248">JSON "Light" (eingeführt in OData v3)</span><span class="sxs-lookup"><span data-stu-id="f8a09-248">JSON "light" (introduced in OData v3)</span></span>
- <span data-ttu-id="f8a09-249">JSON "verbose" (OData v2)</span><span class="sxs-lookup"><span data-stu-id="f8a09-249">JSON "verbose" (OData v2)</span></span>

<span data-ttu-id="f8a09-250">Standardmäßig verwendet die Web-API AtomPubJSON "Light" Format.</span><span class="sxs-lookup"><span data-stu-id="f8a09-250">By default, Web API uses AtomPubJSON "light" format.</span></span> 

<span data-ttu-id="f8a09-251">Um AtomPub-Format zu erhalten, legen Sie den Accept-Header auf "Application/Atom + Xml" aus.</span><span class="sxs-lookup"><span data-stu-id="f8a09-251">To get AtomPub format, set the Accept header to "application/atom+xml".</span></span> <span data-ttu-id="f8a09-252">Hier ist ein Beispiel-Antworttext:</span><span class="sxs-lookup"><span data-stu-id="f8a09-252">Here is an example response body:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

<span data-ttu-id="f8a09-253">Sie können ein offensichtlich Nachteil der Atom-Format finden Sie unter: Es ist wesentlich ausführlicher als die JSON-Light.</span><span class="sxs-lookup"><span data-stu-id="f8a09-253">You can see one obvious disadvantage of the Atom format: It's a lot more verbose than the JSON light.</span></span> <span data-ttu-id="f8a09-254">Wenn ein Client ausgeführt wird, der AtomPub versteht, kann der Client dieses Format über JSON bevorzugen jedoch.</span><span class="sxs-lookup"><span data-stu-id="f8a09-254">However, if you have a client that understands AtomPub, the client might prefer that format over JSON.</span></span>

<span data-ttu-id="f8a09-255">So sieht die JSON-light-Version derselben Entität aus:</span><span class="sxs-lookup"><span data-stu-id="f8a09-255">Here is the JSON light version of the same entity:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

<span data-ttu-id="f8a09-256">Die JSON-light-Format wurde in Version 3 des OData-Protokolls eingeführt.</span><span class="sxs-lookup"><span data-stu-id="f8a09-256">The JSON light format was introduced in version 3 of the OData protocol.</span></span> <span data-ttu-id="f8a09-257">Für die Abwärtskompatibilität kann ein Client das ältere "verbose" JSON-Format anfordern.</span><span class="sxs-lookup"><span data-stu-id="f8a09-257">For backward compatibility, a client can request the older "verbose" JSON format.</span></span> <span data-ttu-id="f8a09-258">Um das ausführliche JSON-Anforderung, legen Sie den Accept-Header auf `application/json;odata=verbose`.</span><span class="sxs-lookup"><span data-stu-id="f8a09-258">To request verbose JSON, set the Accept header to `application/json;odata=verbose`.</span></span> <span data-ttu-id="f8a09-259">Hier ist die ausführliche Version:</span><span class="sxs-lookup"><span data-stu-id="f8a09-259">Here is the verbose version:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

<span data-ttu-id="f8a09-260">Dieses Format vermittelt weitere Metadaten in den Antworttext, der erheblichen Mehraufwand über eine gesamte Sitzung hinzufügen kann.</span><span class="sxs-lookup"><span data-stu-id="f8a09-260">This format conveys more metadata in the response body, which can add considerable overhead over an entire session.</span></span> <span data-ttu-id="f8a09-261">Darüber hinaus fügt eine Dereferenzierungsebene hinzu, durch das Objekt in einer Eigenschaft mit dem Namen "d" wrapping.</span><span class="sxs-lookup"><span data-stu-id="f8a09-261">Also, it adds a level of indirection by wrapping the object in a property named "d".</span></span>

## <a name="next-steps"></a><span data-ttu-id="f8a09-262">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="f8a09-262">Next Steps</span></span>

- [<span data-ttu-id="f8a09-263">Hinzufügen von Entitätsbeziehungen</span><span class="sxs-lookup"><span data-stu-id="f8a09-263">Add Entity Relations</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="f8a09-264">Hinzufügen von OData-Aktionen</span><span class="sxs-lookup"><span data-stu-id="f8a09-264">Add OData Actions</span></span>](odata-actions.md)
- [<span data-ttu-id="f8a09-265">Rufen Sie den OData-Dienst aus einem .NET-Client</span><span class="sxs-lookup"><span data-stu-id="f8a09-265">Call the OData Service From a .NET Client</span></span>](calling-an-odata-service-from-a-net-client.md)
