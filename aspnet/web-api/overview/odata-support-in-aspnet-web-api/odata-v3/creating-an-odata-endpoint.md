---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: Erstellen eine OData v3-Endpunkts mit Web-API 2 | Microsoft-Dokumentation
author: MikeWasson
description: Das Open Data Protocol (OData) ist eine Data Access-Protokoll für das Web. OData bietet eine einheitliche Möglichkeit zum Strukturieren von Daten, die Daten Abfragen und Bearbeiten der Daten...
ms.author: aspnetcontent
ms.date: 02/25/2014
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 67a41f37a09d089336dc96d393f929e56661c4f8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813705"
---
<a name="creating-an-odata-v3-endpoint-with-web-api-2"></a><span data-ttu-id="9d405-104">Erstellen eine OData v3-Endpunkts mit Web-API 2</span><span class="sxs-lookup"><span data-stu-id="9d405-104">Creating an OData v3 Endpoint with Web API 2</span></span>
====================
<span data-ttu-id="9d405-105">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="9d405-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="9d405-106">Abgeschlossenes Projekt herunterladen</span><span class="sxs-lookup"><span data-stu-id="9d405-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="9d405-107">Die [Open Data Protocol](http://www.odata.org/) (OData) ist eine Data Access-Protokoll für das Web.</span><span class="sxs-lookup"><span data-stu-id="9d405-107">The [Open Data Protocol](http://www.odata.org/) (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="9d405-108">OData bietet eine einheitliche Möglichkeit zum Strukturieren von Daten, die Daten Abfragen und bearbeiten das DataSet über CRUD-Vorgänge (erstellen, lesen, aktualisieren und löschen).</span><span class="sxs-lookup"><span data-stu-id="9d405-108">OData provides a uniform way to structure data, query the data, and manipulate the data set through CRUD operations (create, read, update, and delete).</span></span> <span data-ttu-id="9d405-109">OData unterstützt AtomPub (XML) und JSON-Format.</span><span class="sxs-lookup"><span data-stu-id="9d405-109">OData supports both AtomPub (XML) and JSON formats.</span></span> <span data-ttu-id="9d405-110">OData definiert auch eine Möglichkeit, Metadaten zu den Daten verfügbar zu machen.</span><span class="sxs-lookup"><span data-stu-id="9d405-110">OData also defines a way to expose metadata about the data.</span></span> <span data-ttu-id="9d405-111">Clients können die Metadaten verwenden, um die Typinformationen und die Beziehungen für das Dataset zu ermitteln.</span><span class="sxs-lookup"><span data-stu-id="9d405-111">Clients can use the metadata to discover the type information and relationships for the data set.</span></span>
> 
> <span data-ttu-id="9d405-112">ASP.NET Web-API erleichtert es, um einen OData-Endpunkt für ein Dataset zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="9d405-112">ASP.NET Web API makes it easy to create an OData endpoint for a data set.</span></span> <span data-ttu-id="9d405-113">Sie können steuern, auf genau die OData-Vorgänge der Endpunkt unterstützt.</span><span class="sxs-lookup"><span data-stu-id="9d405-113">You can control exactly which OData operations the endpoint supports.</span></span> <span data-ttu-id="9d405-114">Sie können mehrere OData-Endpunkte zusammen mit nicht-OData-Endpunkte hosten.</span><span class="sxs-lookup"><span data-stu-id="9d405-114">You can host multiple OData endpoints, alongside non-OData endpoints.</span></span> <span data-ttu-id="9d405-115">Sie haben vollständige Kontrolle über Ihr Datenmodell, die Back-End-Geschäftslogik und die Datenschicht.</span><span class="sxs-lookup"><span data-stu-id="9d405-115">You have full control over your data model, back-end business logic, and data layer.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="9d405-116">Softwareversionen, die in diesem Tutorial verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="9d405-116">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="9d405-117">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="9d405-117">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="9d405-118">Web-API 2</span><span class="sxs-lookup"><span data-stu-id="9d405-118">Web API 2</span></span>
> - <span data-ttu-id="9d405-119">OData v3</span><span class="sxs-lookup"><span data-stu-id="9d405-119">OData Version 3</span></span>
> - <span data-ttu-id="9d405-120">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="9d405-120">Entity Framework 6</span></span>
> - [<span data-ttu-id="9d405-121">Fiddler Web Debugging Proxy (Optional)</span><span class="sxs-lookup"><span data-stu-id="9d405-121">Fiddler Web Debugging Proxy (Optional)</span></span>](http://www.fiddler2.com)
> 
> <span data-ttu-id="9d405-122">Web-API-OData-Unterstützung wurde hinzugefügt, [ASP.NET und Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span><span class="sxs-lookup"><span data-stu-id="9d405-122">Web API OData support was added in [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="9d405-123">In diesem Lernprogramm verwendet jedoch Gerüst, das in Visual Studio 2013 hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="9d405-123">However, this tutorial uses scaffolding that was added in Visual Studio 2013.</span></span>


<span data-ttu-id="9d405-124">In diesem Tutorial erstellen Sie einen einfachen OData-Endpunkt, den von Clients abgefragt werden können.</span><span class="sxs-lookup"><span data-stu-id="9d405-124">In this tutorial, you will create a simple OData endpoint that clients can query.</span></span> <span data-ttu-id="9d405-125">Außerdem erstellen Sie einen C#-Client für den Endpunkt.</span><span class="sxs-lookup"><span data-stu-id="9d405-125">You will also create a C# client for the endpoint.</span></span> <span data-ttu-id="9d405-126">Nachdem Sie dieses Tutorial abgeschlossen haben, der nächste Satz von Tutorials zeigen, wie Sie weitere Funktionen, einschließlich von entitätsbeziehungen, Aktionen hinzufügen, und wählen Sie die $erweitern / $.</span><span class="sxs-lookup"><span data-stu-id="9d405-126">After you complete this tutorial, the next set of tutorials show how to add more functionality, including entity relations, actions, and $expand/$select.</span></span>

- [<span data-ttu-id="9d405-127">Visual Studio-Projekt erstellen</span><span class="sxs-lookup"><span data-stu-id="9d405-127">Create the Visual Studio Project</span></span>](#create-project)
- [<span data-ttu-id="9d405-128">Fügen Sie ein Entitätsmodell hinzu.</span><span class="sxs-lookup"><span data-stu-id="9d405-128">Add an Entity Model</span></span>](#add-model)
- [<span data-ttu-id="9d405-129">Fügen Sie einen OData-Controller hinzu.</span><span class="sxs-lookup"><span data-stu-id="9d405-129">Add an OData Controller</span></span>](#add-controller)
- [<span data-ttu-id="9d405-130">Hinzufügen des EDM und Weiterleiten</span><span class="sxs-lookup"><span data-stu-id="9d405-130">Add the EDM and Route</span></span>](#edm)
- [<span data-ttu-id="9d405-131">Seeding für die Datenbank (Optional)</span><span class="sxs-lookup"><span data-stu-id="9d405-131">Seed the Database (Optional)</span></span>](#seed-db)
- [<span data-ttu-id="9d405-132">Untersuchen den OData-Endpunkt</span><span class="sxs-lookup"><span data-stu-id="9d405-132">Exploring the OData Endpoint</span></span>](#explore)
- [<span data-ttu-id="9d405-133">OData-Serialisierungsformate</span><span class="sxs-lookup"><span data-stu-id="9d405-133">OData Serialization Formats</span></span>](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a><span data-ttu-id="9d405-134">Visual Studio-Projekt erstellen</span><span class="sxs-lookup"><span data-stu-id="9d405-134">Create the Visual Studio Project</span></span>

<span data-ttu-id="9d405-135">In diesem Tutorial erstellen Sie einen OData-Endpunkt, der grundlegende CRUD-Vorgänge unterstützt.</span><span class="sxs-lookup"><span data-stu-id="9d405-135">In this tutorial, you will create an OData endpoint that supports basic CRUD operations.</span></span> <span data-ttu-id="9d405-136">Der Endpunkt wird eine einzelne Ressource, eine Liste der Produkte verfügbar machen.</span><span class="sxs-lookup"><span data-stu-id="9d405-136">The endpoint will expose a single resource, a list of products.</span></span> <span data-ttu-id="9d405-137">Späteren Tutorials werden der Funktionsumfang erweitert.</span><span class="sxs-lookup"><span data-stu-id="9d405-137">Later tutorials will add more features.</span></span>

<span data-ttu-id="9d405-138">Starten Sie Visual Studio, und wählen Sie **neues Projekt** von der Startseite.</span><span class="sxs-lookup"><span data-stu-id="9d405-138">Start Visual Studio and select **New Project** from the Start page.</span></span> <span data-ttu-id="9d405-139">Alternativ wählen Sie in der **Datei** , wählen Sie im Menü **neu** und dann **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="9d405-139">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="9d405-140">In der **Vorlagen** wählen Sie im Bereich **installierte Vorlagen** und erweitern Sie den Visual C#-Knoten.</span><span class="sxs-lookup"><span data-stu-id="9d405-140">In the **Templates** pane, select **Installed Templates** and expand the Visual C# node.</span></span> <span data-ttu-id="9d405-141">Klicken Sie unter **Visual C#-** Option **Web**.</span><span class="sxs-lookup"><span data-stu-id="9d405-141">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="9d405-142">Wählen Sie **der ASP.NET-Webanwendung** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="9d405-142">Select **the ASP.NET Web Application** template.</span></span>

![](creating-an-odata-endpoint/_static/image1.png)

<span data-ttu-id="9d405-143">In der **neues ASP.NET-Projekt** wählen Sie im Dialogfeld die **leere** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="9d405-143">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="9d405-144">Klicken Sie unter &quot;fügen Sie Ordner und kernreferenzen für... &quot;, überprüfen Sie **Web-API-**.</span><span class="sxs-lookup"><span data-stu-id="9d405-144">Under &quot;Add folders and core references for...&quot;, check **Web API**.</span></span> <span data-ttu-id="9d405-145">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="9d405-145">Click **OK**.</span></span>

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a><span data-ttu-id="9d405-146">Fügen Sie ein Entitätsmodell hinzu.</span><span class="sxs-lookup"><span data-stu-id="9d405-146">Add an Entity Model</span></span>

<span data-ttu-id="9d405-147">Ein *Modell* ist ein Objekt, das die Daten in Ihrer Anwendung darstellt.</span><span class="sxs-lookup"><span data-stu-id="9d405-147">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="9d405-148">Für dieses Tutorial benötigen wir ein Modell, das ein Produkt darstellt.</span><span class="sxs-lookup"><span data-stu-id="9d405-148">For this tutorial, we need a model that represents a product.</span></span> <span data-ttu-id="9d405-149">Das Modell entspricht unserer OData-Entitätstyp.</span><span class="sxs-lookup"><span data-stu-id="9d405-149">The model corresponds to our OData entity type.</span></span>

<span data-ttu-id="9d405-150">Klicken Sie im Projektmappen-Explorer den Ordner "Models".</span><span class="sxs-lookup"><span data-stu-id="9d405-150">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="9d405-151">Wählen Sie im Kontextmenü des **hinzufügen** wählen Sie dann **Klasse**.</span><span class="sxs-lookup"><span data-stu-id="9d405-151">From the context menu, select **Add** then select **Class**.</span></span>

![](creating-an-odata-endpoint/_static/image3.png)

<span data-ttu-id="9d405-152">In der **Add New** Element Dialogfeld, nennen Sie die Klasse &quot;Produkt&quot;.</span><span class="sxs-lookup"><span data-stu-id="9d405-152">In the **Add New** Item dialog, name the class &quot;Product&quot;.</span></span>

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> <span data-ttu-id="9d405-153">Gemäß der Konvention werden ViewModel-Klassen im Ordner "Models" abgelegt.</span><span class="sxs-lookup"><span data-stu-id="9d405-153">By convention, model classes are placed in the Models folder.</span></span> <span data-ttu-id="9d405-154">Sie müssen keine dieser Konvention in Ihren eigenen Projekten folgen, aber wir verwenden sie für dieses Tutorial.</span><span class="sxs-lookup"><span data-stu-id="9d405-154">You don't have to follow this convention in your own projects, but we'll use it for this tutorial.</span></span>


<span data-ttu-id="9d405-155">Fügen Sie in der Datei "Product.cs" die folgende Klassendefinition hinzu:</span><span class="sxs-lookup"><span data-stu-id="9d405-155">In the Product.cs file, add the following class definition:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

<span data-ttu-id="9d405-156">Die ID-Eigenschaft werden die Entitätsschlüssel.</span><span class="sxs-lookup"><span data-stu-id="9d405-156">The ID property will be the entity key.</span></span> <span data-ttu-id="9d405-157">Clients können die Produkte nach ID Abfragen.</span><span class="sxs-lookup"><span data-stu-id="9d405-157">Clients can query products by ID.</span></span> <span data-ttu-id="9d405-158">Dieses Feld wird auch der Primärschlüssel in der Back-End-Datenbank sein.</span><span class="sxs-lookup"><span data-stu-id="9d405-158">This field would also be the primary key in the back-end database.</span></span>

<span data-ttu-id="9d405-159">Erstellen Sie jetzt das Projekt.</span><span class="sxs-lookup"><span data-stu-id="9d405-159">Build the project now.</span></span> <span data-ttu-id="9d405-160">Im nächsten Schritt verwenden wir einige Visual Studio-Gerüst, die Reflektion verwendet, um den Typ des Produkts zu suchen.</span><span class="sxs-lookup"><span data-stu-id="9d405-160">In the next step, we'll use some Visual Studio scaffolding that uses reflection to find the Product type.</span></span>

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a><span data-ttu-id="9d405-161">Fügen Sie einen OData-Controller hinzu.</span><span class="sxs-lookup"><span data-stu-id="9d405-161">Add an OData Controller</span></span>

<span data-ttu-id="9d405-162">Ein *Controller* ist eine Klasse, die HTTP-Anforderungen verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="9d405-162">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="9d405-163">Sie definieren einen separaten Controller für jede Entität, die Sie OData-Dienst festgelegt.</span><span class="sxs-lookup"><span data-stu-id="9d405-163">You define a separate controller for each entity set in you OData service.</span></span> <span data-ttu-id="9d405-164">In diesem Tutorial erstellen wir einen einzelnen Controller.</span><span class="sxs-lookup"><span data-stu-id="9d405-164">In this tutorial, we'll create a single controller.</span></span>

<span data-ttu-id="9d405-165">Klicken Sie im Projektmappen-Explorer den Ordner "Controllers".</span><span class="sxs-lookup"><span data-stu-id="9d405-165">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="9d405-166">Wählen Sie **hinzufügen** und wählen Sie dann **Controller**.</span><span class="sxs-lookup"><span data-stu-id="9d405-166">Select **Add** and then select **Controller**.</span></span>

![](creating-an-odata-endpoint/_static/image5.png)

<span data-ttu-id="9d405-167">In der **Gerüst hinzufügen** wählen Sie im Dialogfeld &quot;Web API 2 OData-Controller mit Aktionen unter Verwendung von Entity Framework&quot;.</span><span class="sxs-lookup"><span data-stu-id="9d405-167">In the **Add Scaffold** dialog, select &quot;Web API 2 OData Controller with actions, using Entity Framework&quot;.</span></span>

![](creating-an-odata-endpoint/_static/image6.png)

<span data-ttu-id="9d405-168">In der **Controller hinzufügen** Dialogfeld benennen Sie den Controller "ProductsController".</span><span class="sxs-lookup"><span data-stu-id="9d405-168">In the **Add Controller** dialog, name the controller "ProductsController".</span></span> <span data-ttu-id="9d405-169">Wählen Sie die &quot;Async-Controlleraktionen verwenden&quot; Kontrollkästchen.</span><span class="sxs-lookup"><span data-stu-id="9d405-169">Select the &quot;Use async controller actions&quot; checkbox.</span></span> <span data-ttu-id="9d405-170">In der **Modell** Dropdown-Liste, wählen Sie die Product-Klasse.</span><span class="sxs-lookup"><span data-stu-id="9d405-170">In the **Model** drop-down list, select the Product class.</span></span>

![](creating-an-odata-endpoint/_static/image7.png)

<span data-ttu-id="9d405-171">Klicken Sie auf die **neuer Datenkontext...**  Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="9d405-171">Click the **New data context...** button.</span></span> <span data-ttu-id="9d405-172">Übernehmen Sie den Standardnamen für den Datentyp für den Kontext, und klicken Sie auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="9d405-172">Leave the default name for the data context type, and click **Add**.</span></span>

![](creating-an-odata-endpoint/_static/image8.png)

<span data-ttu-id="9d405-173">Klicken Sie auf "hinzufügen", in das Dialogfeld "Controller hinzufügen", um den Controller hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="9d405-173">Click Add in the Add Controller dialog to add the controller.</span></span>

![](creating-an-odata-endpoint/_static/image9.png)

<span data-ttu-id="9d405-174">Hinweis: Wenn Sie eine Fehlermeldung erhalten, auf welchem &quot;gab es Fehler beim Abrufen des Typs... &quot;, stellen Sie sicher, dass Sie Visual Studio-Projekt erstellt, nachdem Sie die Product-Klasse hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="9d405-174">Note: If you get an error message that says &quot;There was an error getting the type...&quot;, make sure that you built the Visual Studio project after you added the Product class.</span></span> <span data-ttu-id="9d405-175">Das Gerüst verwendet Reflektion, um die Klasse zu finden.</span><span class="sxs-lookup"><span data-stu-id="9d405-175">The scaffolding uses reflection to find the class.</span></span>

![](creating-an-odata-endpoint/_static/image10.png)

<span data-ttu-id="9d405-176">Das Gerüst wird dem Projekt zwei Codedateien hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="9d405-176">The scaffolding adds two code files to the project:</span></span>

- <span data-ttu-id="9d405-177">Products.cs definiert den Web-API-Controller, der den OData-Endpunkt implementiert.</span><span class="sxs-lookup"><span data-stu-id="9d405-177">Products.cs defines the Web API controller that implements the OData endpoint.</span></span>
- <span data-ttu-id="9d405-178">ProductServiceContext.cs bietet Methoden zum Abfragen der zugrunde liegenden Datenbank mithilfe von Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9d405-178">ProductServiceContext.cs provides methods to query the underlying database, using Entity Framework.</span></span>

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a><span data-ttu-id="9d405-179">Hinzufügen des EDM und Weiterleiten</span><span class="sxs-lookup"><span data-stu-id="9d405-179">Add the EDM and Route</span></span>

<span data-ttu-id="9d405-180">Erweitern Sie im Projektmappen-Explorer die App\_starten Sie die Ordner, und öffnen Sie die Datei mit dem Namen WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="9d405-180">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="9d405-181">Diese Klasse enthält den Konfigurationscode für Web-API.</span><span class="sxs-lookup"><span data-stu-id="9d405-181">This class holds configuration code for Web API.</span></span> <span data-ttu-id="9d405-182">Ersetzen Sie diesen Code durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="9d405-182">Replace this code with the following:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

<span data-ttu-id="9d405-183">Dieser Code bewirkt zwei Dinge:</span><span class="sxs-lookup"><span data-stu-id="9d405-183">This code does two things:</span></span>

- <span data-ttu-id="9d405-184">Erstellt ein Entity Data Model (EDM) für den OData-Endpunkt.</span><span class="sxs-lookup"><span data-stu-id="9d405-184">Creates an Entity Data Model (EDM) for the OData endpoint.</span></span>
- <span data-ttu-id="9d405-185">Fügt eine Route für den Endpunkt hinzu.</span><span class="sxs-lookup"><span data-stu-id="9d405-185">Adds a route for the endpoint.</span></span>

<span data-ttu-id="9d405-186">Ein EDM ist ein abstraktes Modell der Daten.</span><span class="sxs-lookup"><span data-stu-id="9d405-186">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="9d405-187">Das EDM wird verwendet, um das Metadatendokument zu erstellen und definieren Sie die URIs für den Dienst.</span><span class="sxs-lookup"><span data-stu-id="9d405-187">The EDM is used to create the metadata document and define the URIs for the service.</span></span> <span data-ttu-id="9d405-188">Die **ODataConventionModelBuilder** ein EDM mithilfe eines Satzes von Standard-Benennungskonventionen EDM erstellt.</span><span class="sxs-lookup"><span data-stu-id="9d405-188">The **ODataConventionModelBuilder** creates an EDM by using a set of default naming conventions EDM.</span></span> <span data-ttu-id="9d405-189">Dieser Ansatz erfordert den geringsten Code.</span><span class="sxs-lookup"><span data-stu-id="9d405-189">This approach requires the least code.</span></span> <span data-ttu-id="9d405-190">Wenn Sie mehr Kontrolle über das EDM möchten, können Sie die **ODataModelBuilder** Klasse, um die EDM zu erstellen, durch das Hinzufügen von Eigenschaften, Schlüssel und Navigationseigenschaften explizit.</span><span class="sxs-lookup"><span data-stu-id="9d405-190">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="9d405-191">Die **EntitySet** Methode fügt eine Entität, auf das EDM festgelegt:</span><span class="sxs-lookup"><span data-stu-id="9d405-191">The **EntitySet** method adds an entity set to the EDM:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

<span data-ttu-id="9d405-192">Die Zeichenfolge "Produkte" definiert den Namen der Entitätenmenge.</span><span class="sxs-lookup"><span data-stu-id="9d405-192">The string "Products" defines the name of the entity set.</span></span> <span data-ttu-id="9d405-193">Der Name des Controllers muss den Namen der Entitätenmenge überein.</span><span class="sxs-lookup"><span data-stu-id="9d405-193">The name of the controller must match the name of the entity set.</span></span> <span data-ttu-id="9d405-194">In diesem Tutorial die Entitätenmenge ist mit dem Namen "Products" und den Namen des Controllers `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="9d405-194">In this tutorial, the entity set is named "Products" and the controller is named `ProductsController`.</span></span> <span data-ttu-id="9d405-195">Wenn Sie den Namen der Entitätenmenge "ProductSet", nennen Sie den Controller `ProductSetController`.</span><span class="sxs-lookup"><span data-stu-id="9d405-195">If you named the entity set "ProductSet", you would name the controller `ProductSetController`.</span></span> <span data-ttu-id="9d405-196">Beachten Sie, dass ein Endpunkt mehrere Entitätenmengen kann.</span><span class="sxs-lookup"><span data-stu-id="9d405-196">Note that an endpoint can have multiple entity sets.</span></span> <span data-ttu-id="9d405-197">Rufen Sie **EntitySet&lt;T&gt;**  für jede Entität festlegen und anschließend definieren Sie einen entsprechenden Controller.</span><span class="sxs-lookup"><span data-stu-id="9d405-197">Call **EntitySet&lt;T&gt;** for each entity set, and then define a corresponding controller.</span></span>

<span data-ttu-id="9d405-198">Die **MapODataRoute** Methode fügt eine Route für den OData-Endpunkt hinzu.</span><span class="sxs-lookup"><span data-stu-id="9d405-198">The **MapODataRoute** method adds a route for the OData endpoint.</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

<span data-ttu-id="9d405-199">Der erste Parameter ist ein Anzeigename für die Route.</span><span class="sxs-lookup"><span data-stu-id="9d405-199">The first parameter is a friendly name for the route.</span></span> <span data-ttu-id="9d405-200">Dieser Name wird von Clients des Dienstes nicht angezeigt.</span><span class="sxs-lookup"><span data-stu-id="9d405-200">Clients of your service do not see this name.</span></span> <span data-ttu-id="9d405-201">Der zweite Parameter ist der URI-Präfix für den Endpunkt.</span><span class="sxs-lookup"><span data-stu-id="9d405-201">The second parameter is the URI prefix for the endpoint.</span></span> <span data-ttu-id="9d405-202">Wenn diesen Code, der URI für die Produkte Entitätenmenge lautet http://<em>Hostname</em>  /Odata/Produkte.</span><span class="sxs-lookup"><span data-stu-id="9d405-202">Given this code, the URI for the Products entity set is http://<em>hostname</em>/odata/Products.</span></span> <span data-ttu-id="9d405-203">Ihre Anwendung kann mehr als ein OData-Endpunkt haben.</span><span class="sxs-lookup"><span data-stu-id="9d405-203">Your application can have more than one OData endpoint.</span></span> <span data-ttu-id="9d405-204">Rufen Sie für jeden Endpunkt <strong>MapODataRoute</strong> und geben Sie einen eindeutigen Routennamen und einem eindeutigen URI-Präfix.</span><span class="sxs-lookup"><span data-stu-id="9d405-204">For each endpoint, call <strong>MapODataRoute</strong> and provide a unique route name and a unique URI prefix.</span></span>

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a><span data-ttu-id="9d405-205">Seeding für die Datenbank (Optional)</span><span class="sxs-lookup"><span data-stu-id="9d405-205">Seed the Database (Optional)</span></span>

<span data-ttu-id="9d405-206">In diesem Schritt verwenden Sie Entity Framework, das Seeding der Datenbank mit einigen Testdaten.</span><span class="sxs-lookup"><span data-stu-id="9d405-206">In this step, you will use Entity Framework to seed the database with some test data.</span></span> <span data-ttu-id="9d405-207">Dieser Schritt ist optional, jedoch können Sie Ihre OData-Endpunkt direkt zu testen.</span><span class="sxs-lookup"><span data-stu-id="9d405-207">This step is optional, but it lets you test out your OData endpoint right away.</span></span>

<span data-ttu-id="9d405-208">Von der **Tools** , wählen Sie im Menü **Bibliothekspaket-Manager**, und wählen Sie dann **-Paket-Manager-Konsole**.</span><span class="sxs-lookup"><span data-stu-id="9d405-208">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="9d405-209">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="9d405-209">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

<span data-ttu-id="9d405-210">Dadurch wird einen Ordner namens "Migrations" und eine Codedatei Configuration.cs Namens hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="9d405-210">This adds a folder named Migrations and a code file named Configuration.cs.</span></span>

![](creating-an-odata-endpoint/_static/image12.png)

<span data-ttu-id="9d405-211">Öffnen Sie diese Datei, und fügen Sie den folgenden Code der `Configuration.Seed` Methode.</span><span class="sxs-lookup"><span data-stu-id="9d405-211">Open this file and add the following code to the `Configuration.Seed` method.</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

<span data-ttu-id="9d405-212">Geben Sie im Fenster Paket-Manager-Konsole die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="9d405-212">In the Package Manager Console Window, enter the following commands:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

<span data-ttu-id="9d405-213">Diese Befehle generieren Code, der die Datenbank wird erstellt und führt dann diesen Code.</span><span class="sxs-lookup"><span data-stu-id="9d405-213">These commands generate code that creates the database, and then executes that code.</span></span>

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a><span data-ttu-id="9d405-214">Untersuchen den OData-Endpunkt</span><span class="sxs-lookup"><span data-stu-id="9d405-214">Exploring the OData Endpoint</span></span>

<span data-ttu-id="9d405-215">In diesem Abschnitt verwenden wir die [Fiddler Web Debugging Proxy](http://www.fiddler2.com) zum Senden von Anforderungen an den Endpunkt, und überprüfen Sie die Antwortnachrichten.</span><span class="sxs-lookup"><span data-stu-id="9d405-215">In this section, we'll use the [Fiddler Web Debugging Proxy](http://www.fiddler2.com) to send requests to the endpoint and examine the response messages.</span></span> <span data-ttu-id="9d405-216">Dies hilft Ihnen, das die Funktionen eines OData-Endpunkts zu verstehen.</span><span class="sxs-lookup"><span data-stu-id="9d405-216">This will help you to understand the capabilities of an OData endpoint.</span></span>

<span data-ttu-id="9d405-217">Drücken Sie in Visual Studio F5, um mit dem Debuggen beginnen.</span><span class="sxs-lookup"><span data-stu-id="9d405-217">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="9d405-218">Standardmäßig wird Visual Studio im Browser geöffnet, mit denen `http://localhost:*port*`, wobei *Port* ist die Portnummer, die in den projekteinstellungen konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="9d405-218">By default, Visual Studio opens your browser to `http://localhost:*port*`, where *port* is the port number configured in the project settings.</span></span>

<span data-ttu-id="9d405-219">Sie können die Portnummer in den projekteinstellungen ändern.</span><span class="sxs-lookup"><span data-stu-id="9d405-219">You can change the port number in the project settings.</span></span> <span data-ttu-id="9d405-220">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in des Projekts, und wählen **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="9d405-220">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="9d405-221">Wählen Sie im Eigenschaftenfenster **Web**.</span><span class="sxs-lookup"><span data-stu-id="9d405-221">In the properties window, select **Web**.</span></span> <span data-ttu-id="9d405-222">Geben Sie die Portnummer unter **Projekt-Url**.</span><span class="sxs-lookup"><span data-stu-id="9d405-222">Enter the port number under **Project Url**.</span></span>

### <a name="service-document"></a><span data-ttu-id="9d405-223">-Dienstdokument</span><span class="sxs-lookup"><span data-stu-id="9d405-223">Service Document</span></span>

<span data-ttu-id="9d405-224">Die *dienstdokument* enthält eine Liste von Entitätenmengen für den OData-Endpunkt.</span><span class="sxs-lookup"><span data-stu-id="9d405-224">The *service document* contains a list of the entity sets for the OData endpoint.</span></span> <span data-ttu-id="9d405-225">Um das dienstdokument zu erhalten, senden Sie eine GET-Anforderung an die Stamm-URI des Diensts ein.</span><span class="sxs-lookup"><span data-stu-id="9d405-225">To get the service document, send a GET request to the root URI of the service.</span></span>

<span data-ttu-id="9d405-226">Mithilfe von Fiddler, geben Sie in den folgenden URI in der **Composer** Registerkarte: `http://localhost:port/odata/`, wobei *Port* gibt die Portnummer an.</span><span class="sxs-lookup"><span data-stu-id="9d405-226">Using Fiddler, enter the following URI in the **Composer** tab: `http://localhost:port/odata/`, where *port* is the port number.</span></span>

![](creating-an-odata-endpoint/_static/image13.png)

<span data-ttu-id="9d405-227">Klicken Sie auf die **Execute** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="9d405-227">Click the **Execute** button.</span></span> <span data-ttu-id="9d405-228">Fiddler sendet eine HTTP GET-Anforderung an Ihre Anwendung.</span><span class="sxs-lookup"><span data-stu-id="9d405-228">Fiddler sends an HTTP GET request to your application.</span></span> <span data-ttu-id="9d405-229">Daraufhin sollte die Antwort in der Liste der Web-Sitzungen.</span><span class="sxs-lookup"><span data-stu-id="9d405-229">You should see the response in the Web Sessions list.</span></span> <span data-ttu-id="9d405-230">Wenn alles funktioniert, wird der Statuscode 200 lauten.</span><span class="sxs-lookup"><span data-stu-id="9d405-230">If everything is working, the status code will be 200.</span></span>

![](creating-an-odata-endpoint/_static/image14.png)

<span data-ttu-id="9d405-231">Doppelklicken Sie auf die Antwort in der Liste Websitzungen, um die Details der Antwort auf die Registerkarte "Inspektoren" anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="9d405-231">Double-click the response in the Web Sessions list to see the details of the response message in the Inspectors tab.</span></span>

![](creating-an-odata-endpoint/_static/image15.png)

<span data-ttu-id="9d405-232">Unformatierte HTTP-Antwortnachricht sollte etwa wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="9d405-232">The raw HTTP response message should look similar to the following:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

<span data-ttu-id="9d405-233">Standardmäßig gibt das dienstdokument von Web-API im AtomPub-Format zurück.</span><span class="sxs-lookup"><span data-stu-id="9d405-233">By default, Web API returns the service document in AtomPub format.</span></span> <span data-ttu-id="9d405-234">Um JSON-Anforderung, fügen Sie die folgenden Header zur HTTP-Anforderung:</span><span class="sxs-lookup"><span data-stu-id="9d405-234">To request JSON, add the following header to the HTTP request:</span></span>

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

<span data-ttu-id="9d405-235">Die HTTP-Antwort enthält jetzt eine JSON-Nutzlast:</span><span class="sxs-lookup"><span data-stu-id="9d405-235">Now the HTTP response contains a JSON payload:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a><span data-ttu-id="9d405-236">Dienstmetadatendokument</span><span class="sxs-lookup"><span data-stu-id="9d405-236">Service Metadata Document</span></span>

<span data-ttu-id="9d405-237">Die *dienstmetadatendokument* beschreibt das Datenmodell des Diensts mithilfe einer XML-Sprache, die der konzeptionellen Schemadefinitionssprache (CSDL) bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="9d405-237">The *service metadata document* describes the data model of the service, using an XML language called the Conceptual Schema Definition Language (CSDL).</span></span> <span data-ttu-id="9d405-238">Das Dokument zeigt die Struktur der Daten im Dienst, und Sie können zum Generieren von Clientcode verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="9d405-238">The metadata document shows the structure of the data in the service, and can be used to generate client code.</span></span>

<span data-ttu-id="9d405-239">Um das Metadatendokument zu erhalten, senden Sie eine GET-Anforderung an `http://localhost:port/odata/$metadata`.</span><span class="sxs-lookup"><span data-stu-id="9d405-239">To get the metadata document, send a GET request to `http://localhost:port/odata/$metadata`.</span></span> <span data-ttu-id="9d405-240">Hier ist die Metadaten für den Endpunkt, der in diesem Tutorial gezeigt.</span><span class="sxs-lookup"><span data-stu-id="9d405-240">Here is the metadata for the endpoint shown in this tutorial.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a><span data-ttu-id="9d405-241">Entitätenmenge</span><span class="sxs-lookup"><span data-stu-id="9d405-241">Entity Set</span></span>

<span data-ttu-id="9d405-242">Um die Entitätenmenge für die Produkte zu erhalten, senden Sie eine GET-Anforderung an `http://localhost:port/odata/Products`.</span><span class="sxs-lookup"><span data-stu-id="9d405-242">To get the Products entity set, send a GET request to `http://localhost:port/odata/Products`.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a><span data-ttu-id="9d405-243">Entität</span><span class="sxs-lookup"><span data-stu-id="9d405-243">Entity</span></span>

<span data-ttu-id="9d405-244">Um ein einzelnes Produkt zu erhalten, senden Sie eine GET-Anforderung an `http://localhost:port/odata/Products(1)`, wobei "1" die Produkt-ID</span><span class="sxs-lookup"><span data-stu-id="9d405-244">To get an individual product, send a GET request to `http://localhost:port/odata/Products(1)`, where "1" is the product ID.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a><span data-ttu-id="9d405-245">OData-Serialisierungsformate</span><span class="sxs-lookup"><span data-stu-id="9d405-245">OData Serialization Formats</span></span>

<span data-ttu-id="9d405-246">OData unterstützt mehrere Serialisierungsformate:</span><span class="sxs-lookup"><span data-stu-id="9d405-246">OData supports several serialization formats:</span></span>

- <span data-ttu-id="9d405-247">Atom pub-Protokolls (XML)</span><span class="sxs-lookup"><span data-stu-id="9d405-247">Atom Pub (XML)</span></span>
- <span data-ttu-id="9d405-248">JSON "Light" (eingeführt in OData v3)</span><span class="sxs-lookup"><span data-stu-id="9d405-248">JSON "light" (introduced in OData v3)</span></span>
- <span data-ttu-id="9d405-249">JSON "verbose" (OData v2)</span><span class="sxs-lookup"><span data-stu-id="9d405-249">JSON "verbose" (OData v2)</span></span>

<span data-ttu-id="9d405-250">Standardmäßig verwendet die Web-API-AtomPubJSON "Light" Format.</span><span class="sxs-lookup"><span data-stu-id="9d405-250">By default, Web API uses AtomPubJSON "light" format.</span></span> 

<span data-ttu-id="9d405-251">Um AtomPub-Format zu erhalten, wird den Accept-Header auf "Application/Atom + Xml" fest.</span><span class="sxs-lookup"><span data-stu-id="9d405-251">To get AtomPub format, set the Accept header to "application/atom+xml".</span></span> <span data-ttu-id="9d405-252">Hier ist ein Beispiel-Antworttext:</span><span class="sxs-lookup"><span data-stu-id="9d405-252">Here is an example response body:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

<span data-ttu-id="9d405-253">Sehen Sie ein offensichtlicher Nachteil des Atom-Format: Es ist viel ausführlicher als die JSON-Light.</span><span class="sxs-lookup"><span data-stu-id="9d405-253">You can see one obvious disadvantage of the Atom format: It's a lot more verbose than the JSON light.</span></span> <span data-ttu-id="9d405-254">Wenn Sie einen Client, der AtomPub versteht verfügen, kann der Client dieses Format über JSON bevorzugen es jedoch.</span><span class="sxs-lookup"><span data-stu-id="9d405-254">However, if you have a client that understands AtomPub, the client might prefer that format over JSON.</span></span>

<span data-ttu-id="9d405-255">So sieht die JSON-light-Version der gleichen Entität aus:</span><span class="sxs-lookup"><span data-stu-id="9d405-255">Here is the JSON light version of the same entity:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

<span data-ttu-id="9d405-256">Die JSON-light-Format wurde in Version 3 des OData-Protokolls eingeführt.</span><span class="sxs-lookup"><span data-stu-id="9d405-256">The JSON light format was introduced in version 3 of the OData protocol.</span></span> <span data-ttu-id="9d405-257">Um Abwärtskompatibilität zu gewährleisten kann ein Client das ältere "verbose" JSON-Format anfordern.</span><span class="sxs-lookup"><span data-stu-id="9d405-257">For backward compatibility, a client can request the older "verbose" JSON format.</span></span> <span data-ttu-id="9d405-258">Um ausführlichen JSON-Format anzufordern, legen Sie den Accept-Header auf `application/json;odata=verbose`.</span><span class="sxs-lookup"><span data-stu-id="9d405-258">To request verbose JSON, set the Accept header to `application/json;odata=verbose`.</span></span> <span data-ttu-id="9d405-259">Hier ist die ausführliche Version aus:</span><span class="sxs-lookup"><span data-stu-id="9d405-259">Here is the verbose version:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

<span data-ttu-id="9d405-260">Dieses Format gewährt mehr Metadaten in den Antworttext, der beträchtlichen Aufwand über eine gesamte Sitzung hinzufügen kann.</span><span class="sxs-lookup"><span data-stu-id="9d405-260">This format conveys more metadata in the response body, which can add considerable overhead over an entire session.</span></span> <span data-ttu-id="9d405-261">Darüber hinaus fügt eine Dereferenzierungsebene hinzu, durch Umschließen des Objekts in einer Eigenschaft mit dem Namen "d".</span><span class="sxs-lookup"><span data-stu-id="9d405-261">Also, it adds a level of indirection by wrapping the object in a property named "d".</span></span>

## <a name="next-steps"></a><span data-ttu-id="9d405-262">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="9d405-262">Next Steps</span></span>

- [<span data-ttu-id="9d405-263">Hinzufügen von Entitätsbeziehungen</span><span class="sxs-lookup"><span data-stu-id="9d405-263">Add Entity Relations</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="9d405-264">Hinzufügen von OData-Aktionen</span><span class="sxs-lookup"><span data-stu-id="9d405-264">Add OData Actions</span></span>](odata-actions.md)
- [<span data-ttu-id="9d405-265">Rufen Sie den OData-Dienst aus einem .NET-Client</span><span class="sxs-lookup"><span data-stu-id="9d405-265">Call the OData Service From a .NET Client</span></span>](calling-an-odata-service-from-a-net-client.md)
