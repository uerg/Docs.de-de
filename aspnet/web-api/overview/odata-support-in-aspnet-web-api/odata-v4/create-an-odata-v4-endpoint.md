---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: Erstellen einer ASP.NET Web API 2.2 verwendet OData v4-Endpunkts | Microsoft-Dokumentation
author: MikeWasson
description: Das Open Data Protocol (OData) ist eine Data Access-Protokoll für das Web. OData bietet eine einheitliche Möglichkeit zum Abfragen und Bearbeiten von Datensätzen über CRUD-Vorgänge...
ms.author: riande
ms.date: 06/24/2014
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 48c1a78c96cb0ebfa0b053dfef84e76433112650
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795417"
---
<a name="create-an-odata-v4-endpoint-using-aspnet-web-api-22"></a><span data-ttu-id="35ff6-104">Erstellen einer ASP.NET Web API 2.2 verwendet OData v4-Endpunkts</span><span class="sxs-lookup"><span data-stu-id="35ff6-104">Create an OData v4 Endpoint Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="35ff6-105">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="35ff6-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="35ff6-106">Das Open Data Protocol (OData) ist eine Data Access-Protokoll für das Web.</span><span class="sxs-lookup"><span data-stu-id="35ff6-106">The Open Data Protocol (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="35ff6-107">OData bietet eine einheitliche Möglichkeit zum Abfragen und Bearbeiten von Datensätzen über CRUD-Vorgänge (erstellen, lesen, aktualisieren und löschen).</span><span class="sxs-lookup"><span data-stu-id="35ff6-107">OData provides a uniform way to query and manipulate data sets through CRUD operations (create, read, update, and delete).</span></span>
>
> <span data-ttu-id="35ff6-108">ASP.NET Web-API unterstützt v3 und v4 des Protokolls.</span><span class="sxs-lookup"><span data-stu-id="35ff6-108">ASP.NET Web API supports both v3 and v4 of the protocol.</span></span> <span data-ttu-id="35ff6-109">Sie haben sogar einen v4-Endpunkts, die Seite-an-Seite ausgeführt wird mit einem v3-Endpunkt.</span><span class="sxs-lookup"><span data-stu-id="35ff6-109">You can even have a v4 endpoint that runs side-by-side with a v3 endpoint.</span></span>
>
> <span data-ttu-id="35ff6-110">Dieses Tutorial veranschaulicht, wie einen OData v4-Endpunkt zu erstellen, der CRUD-Vorgänge unterstützt.</span><span class="sxs-lookup"><span data-stu-id="35ff6-110">This tutorial shows how to create an OData v4 endpoint that supports CRUD operations.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="35ff6-111">Softwareversionen, die in diesem Tutorial verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="35ff6-111">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="35ff6-112">Web-API 2.2</span><span class="sxs-lookup"><span data-stu-id="35ff6-112">Web API 2.2</span></span>
> - <span data-ttu-id="35ff6-113">OData v4</span><span class="sxs-lookup"><span data-stu-id="35ff6-113">OData v4</span></span>
> - <span data-ttu-id="35ff6-114">Visual Studio 2013 (Visual Studio 2017 herunterladen [hier](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span><span class="sxs-lookup"><span data-stu-id="35ff6-114">Visual Studio 2013 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span></span>
> - <span data-ttu-id="35ff6-115">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="35ff6-115">Entity Framework 6</span></span>
> - <span data-ttu-id="35ff6-116">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="35ff6-116">.NET 4.5</span></span>
>
> ## <a name="tutorial-versions"></a><span data-ttu-id="35ff6-117">Lernprogramm-Versionen</span><span class="sxs-lookup"><span data-stu-id="35ff6-117">Tutorial versions</span></span>
>
> <span data-ttu-id="35ff6-118">Die OData-Version 3, finden Sie unter [Erstellen eines OData-v3-Endpunkts](../odata-v3/creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="35ff6-118">For the OData Version 3, see [Creating an OData v3 Endpoint](../odata-v3/creating-an-odata-endpoint.md).</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="35ff6-119">Visual Studio-Projekt erstellen</span><span class="sxs-lookup"><span data-stu-id="35ff6-119">Create the Visual Studio Project</span></span>

<span data-ttu-id="35ff6-120">In Visual Studio aus der **Datei** , wählen Sie im Menü **neu** &gt; **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="35ff6-120">In Visual Studio, from the **File** menu, select **New** &gt; **Project**.</span></span>

<span data-ttu-id="35ff6-121">Erweitern Sie **installiert** &gt; **Vorlagen** &gt; **Visual C#-** &gt; **Web**, und wählen Sie die  **ASP.NET Web Application** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="35ff6-121">Expand **Installed** &gt; **Templates** &gt; **Visual C#** &gt; **Web**, and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="35ff6-122">Nennen Sie das Projekt &quot;ProductService&quot;.</span><span class="sxs-lookup"><span data-stu-id="35ff6-122">Name the project &quot;ProductService&quot;.</span></span>

[![](create-an-odata-v4-endpoint/_static/image2.png)](create-an-odata-v4-endpoint/_static/image1.png)

<span data-ttu-id="35ff6-123">In der **neues Projekt** wählen Sie im Dialogfeld die **leere** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="35ff6-123">In the **New Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="35ff6-124">Klicken Sie unter &quot;fügen Sie Ordner und kernreferenzen... &quot;, klicken Sie auf **Web-API-**.</span><span class="sxs-lookup"><span data-stu-id="35ff6-124">Under &quot;Add folders and core references...&quot;, click **Web API**.</span></span> <span data-ttu-id="35ff6-125">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="35ff6-125">Click **OK**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image4.png)](create-an-odata-v4-endpoint/_static/image3.png)

## <a name="install-the-odata-packages"></a><span data-ttu-id="35ff6-126">Installieren Sie die OData-Pakete</span><span class="sxs-lookup"><span data-stu-id="35ff6-126">Install the OData Packages</span></span>

<span data-ttu-id="35ff6-127">Wählen Sie im Menü **Tools** die Option **NuGet-Paket-Manager** &gt; **Paket-Manager-Konsole** aus.</span><span class="sxs-lookup"><span data-stu-id="35ff6-127">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="35ff6-128">Geben Sie im Fenster Paket-Manager-Konsole:</span><span class="sxs-lookup"><span data-stu-id="35ff6-128">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

<span data-ttu-id="35ff6-129">Mit diesem Befehl wird der aktuelle OData-NuGet-Pakete installiert.</span><span class="sxs-lookup"><span data-stu-id="35ff6-129">This command installs the latest OData NuGet packages.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="35ff6-130">Hinzufügen einer Modellklasse</span><span class="sxs-lookup"><span data-stu-id="35ff6-130">Add a Model Class</span></span>

<span data-ttu-id="35ff6-131">Ein *Modell* ist ein Objekt, das eine Datenentität in Ihrer Anwendung darstellt.</span><span class="sxs-lookup"><span data-stu-id="35ff6-131">A *model* is an object that represents a data entity in your application.</span></span>

<span data-ttu-id="35ff6-132">Klicken Sie im Projektmappen-Explorer den Ordner "Models".</span><span class="sxs-lookup"><span data-stu-id="35ff6-132">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="35ff6-133">Wählen Sie im Kontextmenü des **hinzufügen** &gt; **Klasse**.</span><span class="sxs-lookup"><span data-stu-id="35ff6-133">From the context menu, select **Add** &gt; **Class**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="35ff6-134">Gemäß der Konvention ViewModel-Klassen befinden sich im Ordner "Models", aber Sie müssen keine dieser Konvention in Ihren eigenen Projekten folgen.</span><span class="sxs-lookup"><span data-stu-id="35ff6-134">By convention, model classes are placed in the Models folder, but you don't have to follow this convention in your own projects.</span></span>


<span data-ttu-id="35ff6-135">Nennen Sie die Klasse `Product`.</span><span class="sxs-lookup"><span data-stu-id="35ff6-135">Name the class `Product`.</span></span> <span data-ttu-id="35ff6-136">Ersetzen Sie in der Datei "Product.cs" den Standardcode durch den folgenden:</span><span class="sxs-lookup"><span data-stu-id="35ff6-136">In the Product.cs file, replace the boilerplate code with the following:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

<span data-ttu-id="35ff6-137">Die `Id` -Eigenschaft ist der Entitätsschlüssel.</span><span class="sxs-lookup"><span data-stu-id="35ff6-137">The `Id` property is the entity key.</span></span> <span data-ttu-id="35ff6-138">Clients können Entitäten nach Schlüssel abrufen.</span><span class="sxs-lookup"><span data-stu-id="35ff6-138">Clients can query entities by key.</span></span> <span data-ttu-id="35ff6-139">Um das Produkt mit der ID 5 zu erhalten, ist der URI beispielsweise `/Products(5)`.</span><span class="sxs-lookup"><span data-stu-id="35ff6-139">For example, to get the product with ID of 5, the URI is `/Products(5)`.</span></span> <span data-ttu-id="35ff6-140">Die `Id` Eigenschaft werden ebenfalls der Primärschlüssel in der Back-End-Datenbank.</span><span class="sxs-lookup"><span data-stu-id="35ff6-140">The `Id` property will also be the primary key in the back-end database.</span></span>

## <a name="enable-entity-framework"></a><span data-ttu-id="35ff6-141">Aktivieren von Entitätsframework</span><span class="sxs-lookup"><span data-stu-id="35ff6-141">Enable Entity Framework</span></span>

<span data-ttu-id="35ff6-142">In diesem Tutorial wird Entity Framework (EF) Code First verwendet, um die Back-End-Datenbank zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="35ff6-142">For this tutorial, we'll use Entity Framework (EF) Code First to create the back-end database.</span></span>

> [!NOTE]
> <span data-ttu-id="35ff6-143">Web-API OData ist EF nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="35ff6-143">Web API OData does not require EF.</span></span> <span data-ttu-id="35ff6-144">Verwenden Sie Datenzugriffsebene, die Datenbankentitäten in Modellen umsetzen kann.</span><span class="sxs-lookup"><span data-stu-id="35ff6-144">Use any data-access layer that can translate database entities into models.</span></span>


<span data-ttu-id="35ff6-145">Installieren Sie zuerst das NuGet-Paket für EF.</span><span class="sxs-lookup"><span data-stu-id="35ff6-145">First, install the NuGet package for EF.</span></span> <span data-ttu-id="35ff6-146">Wählen Sie im Menü **Tools** die Option **NuGet-Paket-Manager** &gt; **Paket-Manager-Konsole** aus.</span><span class="sxs-lookup"><span data-stu-id="35ff6-146">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="35ff6-147">Geben Sie im Fenster Paket-Manager-Konsole:</span><span class="sxs-lookup"><span data-stu-id="35ff6-147">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

<span data-ttu-id="35ff6-148">Öffnen Sie die Datei "Web.config", und fügen Sie folgenden Abschnitt in der **Konfiguration** Element nach dem **ConfigSections** Element.</span><span class="sxs-lookup"><span data-stu-id="35ff6-148">Open the Web.config file, and add the following section inside the **configuration** element, after the **configSections** element.</span></span>

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

<span data-ttu-id="35ff6-149">Diese Einstellung wird eine Verbindungszeichenfolge für eine LocalDB-Datenbank hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="35ff6-149">This setting adds a connection string for a LocalDB database.</span></span> <span data-ttu-id="35ff6-150">Wenn Sie die app lokal ausführen, wird diese Datenbank verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="35ff6-150">This database will be used when you run the app locally.</span></span>

<span data-ttu-id="35ff6-151">Als Nächstes fügen Sie eine Klasse, die mit dem Namen `ProductsContext` zum Ordner "Models":</span><span class="sxs-lookup"><span data-stu-id="35ff6-151">Next, add a class named `ProductsContext` to the Models folder:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

<span data-ttu-id="35ff6-152">Im Konstruktor `"name=ProductsContext"` gibt den Namen der Verbindungszeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="35ff6-152">In the constructor, `"name=ProductsContext"` gives the name of the connection string.</span></span>

## <a name="configure-the-odata-endpoint"></a><span data-ttu-id="35ff6-153">Konfigurieren Sie den OData-Endpunkt</span><span class="sxs-lookup"><span data-stu-id="35ff6-153">Configure the OData Endpoint</span></span>

<span data-ttu-id="35ff6-154">Öffnen Sie die Datei App\_Start/WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="35ff6-154">Open the file App\_Start/WebApiConfig.cs.</span></span> <span data-ttu-id="35ff6-155">Fügen Sie die folgenden **mit** Anweisungen:</span><span class="sxs-lookup"><span data-stu-id="35ff6-155">Add the following **using** statements:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

<span data-ttu-id="35ff6-156">Klicken Sie dann fügen Sie den folgenden Code der **registrieren** Methode:</span><span class="sxs-lookup"><span data-stu-id="35ff6-156">Then add the following code to the **Register** method:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

<span data-ttu-id="35ff6-157">Dieser Code bewirkt zwei Dinge:</span><span class="sxs-lookup"><span data-stu-id="35ff6-157">This code does two things:</span></span>

- <span data-ttu-id="35ff6-158">Erstellt ein Entity Data Model (EDM).</span><span class="sxs-lookup"><span data-stu-id="35ff6-158">Creates an Entity Data Model (EDM).</span></span>
- <span data-ttu-id="35ff6-159">Fügt eine Route hinzu.</span><span class="sxs-lookup"><span data-stu-id="35ff6-159">Adds a route.</span></span>

<span data-ttu-id="35ff6-160">Ein EDM ist ein abstraktes Modell der Daten.</span><span class="sxs-lookup"><span data-stu-id="35ff6-160">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="35ff6-161">Das EDM wird verwendet, um das Metadatendokument für den Dienst zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="35ff6-161">The EDM is used to create the service metadata document.</span></span> <span data-ttu-id="35ff6-162">Die **ODataConventionModelBuilder** Klasse erstellt ein EDM mithilfe von Standard-Benennungskonventionen.</span><span class="sxs-lookup"><span data-stu-id="35ff6-162">The **ODataConventionModelBuilder** class creates an EDM by using default naming conventions.</span></span> <span data-ttu-id="35ff6-163">Dieser Ansatz erfordert den geringsten Code.</span><span class="sxs-lookup"><span data-stu-id="35ff6-163">This approach requires the least code.</span></span> <span data-ttu-id="35ff6-164">Wenn Sie mehr Kontrolle über das EDM möchten, können Sie die **ODataModelBuilder** Klasse, um die EDM zu erstellen, durch das Hinzufügen von Eigenschaften, Schlüssel und Navigationseigenschaften explizit.</span><span class="sxs-lookup"><span data-stu-id="35ff6-164">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="35ff6-165">Ein *Route* weist Web-API wie HTTP-Anforderungen an den Endpunkt weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="35ff6-165">A *route* tells Web API how to route HTTP requests to the endpoint.</span></span> <span data-ttu-id="35ff6-166">Rufen Sie zum Erstellen einer OData v4-Route dem **MapODataServiceRoute** -Erweiterungsmethode.</span><span class="sxs-lookup"><span data-stu-id="35ff6-166">To create an OData v4 route, call the **MapODataServiceRoute** extension method.</span></span>

<span data-ttu-id="35ff6-167">Wenn Ihre Anwendung mehrere OData-Endpunkte verfügt, erstellen Sie eine separate Route für jeden.</span><span class="sxs-lookup"><span data-stu-id="35ff6-167">If your application has multiple OData endpoints, create a separate route for each.</span></span> <span data-ttu-id="35ff6-168">Geben Sie jede Route, einen eindeutigen Routennamen und Präfix zu.</span><span class="sxs-lookup"><span data-stu-id="35ff6-168">Give each route a unique route name and prefix.</span></span>

## <a name="add-the-odata-controller"></a><span data-ttu-id="35ff6-169">Hinzufügen des OData-Controllers</span><span class="sxs-lookup"><span data-stu-id="35ff6-169">Add the OData Controller</span></span>

<span data-ttu-id="35ff6-170">Ein *Controller* ist eine Klasse, die HTTP-Anforderungen verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="35ff6-170">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="35ff6-171">Erstellen Sie einen separaten Controller für jede Entität im OData-Dienst festgelegt.</span><span class="sxs-lookup"><span data-stu-id="35ff6-171">You create a separate controller for each entity set in your OData service.</span></span> <span data-ttu-id="35ff6-172">In diesem Tutorial erstellen Sie einen Controller für die `Product` Entität.</span><span class="sxs-lookup"><span data-stu-id="35ff6-172">In this tutorial, you will create one controller, for the `Product` entity.</span></span>

<span data-ttu-id="35ff6-173">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in den Ordner "Controllers", und wählen Sie **hinzufügen** &gt; **Klasse**.</span><span class="sxs-lookup"><span data-stu-id="35ff6-173">In Solution Explorer, right-click the Controllers folder and select **Add** &gt; **Class**.</span></span> <span data-ttu-id="35ff6-174">Nennen Sie die Klasse `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="35ff6-174">Name the class `ProductsController`.</span></span>

> [!NOTE]
> <span data-ttu-id="35ff6-175">Die Version dieses Tutorials für OData v3 verwendet die **Controller hinzufügen** Gerüstbau.</span><span class="sxs-lookup"><span data-stu-id="35ff6-175">The version of this tutorial for OData v3 uses the **Add Controller** scaffolding.</span></span> <span data-ttu-id="35ff6-176">Es gibt derzeit keine Gerüstbau für OData v4.</span><span class="sxs-lookup"><span data-stu-id="35ff6-176">Currently, there is no scaffolding for OData v4.</span></span>


<span data-ttu-id="35ff6-177">Ersetzen Sie die Codebausteine in ProductsController.cs durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="35ff6-177">Replace the boilerplate code in ProductsController.cs with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

<span data-ttu-id="35ff6-178">Der Controller verwendet die `ProductsContext` Klasse Zugriff auf die Datenbank mithilfe von EF.</span><span class="sxs-lookup"><span data-stu-id="35ff6-178">The controller uses the `ProductsContext` class to access the database using EF.</span></span> <span data-ttu-id="35ff6-179">Beachten Sie, die der Controller überschreibt die **Dispose** Methode zum Verwerfen der **ProductsContext**.</span><span class="sxs-lookup"><span data-stu-id="35ff6-179">Notice that the controller overrides the **Dispose** method to dispose of the **ProductsContext**.</span></span>

<span data-ttu-id="35ff6-180">Dies ist der Ausgangspunkt für den Controller.</span><span class="sxs-lookup"><span data-stu-id="35ff6-180">This is the starting point for the controller.</span></span> <span data-ttu-id="35ff6-181">Als Nächstes fügen wir Methoden für alle CRUD-Vorgänge.</span><span class="sxs-lookup"><span data-stu-id="35ff6-181">Next, we'll add methods for all of the CRUD operations.</span></span>

## <a name="querying-the-entity-set"></a><span data-ttu-id="35ff6-182">Die Entitätenmenge Abfragen</span><span class="sxs-lookup"><span data-stu-id="35ff6-182">Querying the Entity Set</span></span>

<span data-ttu-id="35ff6-183">Fügen Sie die folgenden Methoden auf `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="35ff6-183">Add the following methods to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

<span data-ttu-id="35ff6-184">Der parameterlosen Version von der `Get` die gesamte Auflistung für die Produkte der Methodenrückgabe.</span><span class="sxs-lookup"><span data-stu-id="35ff6-184">The parameterless version of the `Get` method returns the entire Products collection.</span></span> <span data-ttu-id="35ff6-185">Die `Get` -Methode mit einem *Schlüssel* Parameter sucht ein Produkt durch deren Schlüssel (in diesem Fall die `Id` Eigenschaft).</span><span class="sxs-lookup"><span data-stu-id="35ff6-185">The `Get` method with a *key* parameter looks up a product by its key (in this case, the `Id` property).</span></span>

<span data-ttu-id="35ff6-186">Die **[EnableQuery]** Attribut ermöglicht Clients, die Abfrage mithilfe von Abfrageoptionen wie $filter "," $sort, und "$page zu ändern.</span><span class="sxs-lookup"><span data-stu-id="35ff6-186">The **[EnableQuery]** attribute enables clients to modify the query, by using query options such as $filter, $sort, and $page.</span></span> <span data-ttu-id="35ff6-187">Weitere Informationen finden Sie unter [unterstützt OData-Abfrageoptionen](../supporting-odata-query-options.md).</span><span class="sxs-lookup"><span data-stu-id="35ff6-187">For more information, see [Supporting OData Query Options](../supporting-odata-query-options.md).</span></span>

## <a name="adding-an-entity-to-the-entity-set"></a><span data-ttu-id="35ff6-188">Beim Hinzufügen einer Entität auf die Entitätenmenge</span><span class="sxs-lookup"><span data-stu-id="35ff6-188">Adding an Entity to the Entity Set</span></span>

<span data-ttu-id="35ff6-189">Um Clients zum Hinzufügen eines neuen Produkts in die Datenbank zu aktivieren, fügen Sie die folgende Methode `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="35ff6-189">To enable clients to add a new product to the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="updating-an-entity"></a><span data-ttu-id="35ff6-190">Aktualisieren einer Entität</span><span class="sxs-lookup"><span data-stu-id="35ff6-190">Updating an Entity</span></span>

<span data-ttu-id="35ff6-191">OData unterstützt zwei unterschiedliche Semantiken zum Aktualisieren einer Entität, Patch- und PUT.</span><span class="sxs-lookup"><span data-stu-id="35ff6-191">OData supports two different semantics for updating an entity, PATCH and PUT.</span></span>

- <span data-ttu-id="35ff6-192">PATCH führt eine partielle Aktualisierung.</span><span class="sxs-lookup"><span data-stu-id="35ff6-192">PATCH performs a partial update.</span></span> <span data-ttu-id="35ff6-193">Der Client gibt nur die Eigenschaften zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="35ff6-193">The client specifies just the properties to update.</span></span>
- <span data-ttu-id="35ff6-194">PUT wird die gesamte Entität ersetzt.</span><span class="sxs-lookup"><span data-stu-id="35ff6-194">PUT replaces the entire entity.</span></span>

<span data-ttu-id="35ff6-195">Der Nachteil von PUT ist, dass der Client gesendet werden muss Werte für alle Eigenschaften in der Entität, einschließlich der Werte, die nicht geändert werden.</span><span class="sxs-lookup"><span data-stu-id="35ff6-195">The disadvantage of PUT is that the client must send values for all of the properties in the entity, including values that are not changing.</span></span> <span data-ttu-id="35ff6-196">Die [OData-Spezifikation](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) gibt an, dass ein PATCH bevorzugt wird.</span><span class="sxs-lookup"><span data-stu-id="35ff6-196">The [OData spec](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) states that PATCH is preferred.</span></span>

<span data-ttu-id="35ff6-197">In jedem Fall sieht der Code für sowohl Patch- und PUT-Methoden:</span><span class="sxs-lookup"><span data-stu-id="35ff6-197">In any case, here is the code for both PATCH and PUT methods:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

<span data-ttu-id="35ff6-198">Im Fall von PATCH, der Controller verwendet die **Delta&lt;T&gt;**  Typ, um die Änderungen nachzuverfolgen.</span><span class="sxs-lookup"><span data-stu-id="35ff6-198">In the case of PATCH, the controller uses the **Delta&lt;T&gt;** type to track the changes.</span></span>

## <a name="deleting-an-entity"></a><span data-ttu-id="35ff6-199">Löschen einer Entität</span><span class="sxs-lookup"><span data-stu-id="35ff6-199">Deleting an Entity</span></span>

<span data-ttu-id="35ff6-200">Um Clients So löschen Sie ein Produkt aus der Datenbank zu aktivieren, fügen Sie die folgende Methode `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="35ff6-200">To enable clients to delete a product from the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
