---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Aktionen und Funktionen in OData v4 mithilfe von ASP.NET Web API 2.2 | Microsoft-Dokumentation
author: MikeWasson
description: 'In OData können Aktionen und Funktionen: serverseitige Verhaltensweisen hinzufügen, die nicht einfach als CRUD-Vorgänge für Entitäten definiert sind. Dieses Tutorial zeigt, wie Sie...'
ms.author: aspnetcontent
ms.date: 06/27/2014
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: a34361a2b298a6cb1efaa386d0a60011f4a578fa
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37834973"
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a><span data-ttu-id="f8e3d-104">Aktionen und Funktionen in OData v4 ASP.NET Web API 2.2 verwendet</span><span class="sxs-lookup"><span data-stu-id="f8e3d-104">Actions and Functions in OData v4 Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="f8e3d-105">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f8e3d-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="f8e3d-106">In OData können Aktionen und Funktionen: serverseitige Verhaltensweisen hinzufügen, die nicht einfach als CRUD-Vorgänge für Entitäten definiert sind.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-106">In OData, actions and functions are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="f8e3d-107">Dieses Tutorial veranschaulicht das Hinzufügen von Aktionen und Funktionen OData v4-Endpunkt mit Web API 2.2.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-107">This tutorial shows how to add actions and functions to an OData v4 endpoint, using Web API 2.2.</span></span> <span data-ttu-id="f8e3d-108">Das Tutorial baut auf dem Tutorial [erstellen Sie eine OData v4-Endpunkts mit ASP.NET-Web API 2](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="f8e3d-108">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f8e3d-109">Softwareversionen, die in diesem Tutorial verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="f8e3d-110">Web-API 2.2</span><span class="sxs-lookup"><span data-stu-id="f8e3d-110">Web API 2.2</span></span>
> - <span data-ttu-id="f8e3d-111">OData v4</span><span class="sxs-lookup"><span data-stu-id="f8e3d-111">OData v4</span></span>
> - [<span data-ttu-id="f8e3d-112">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="f8e3d-112">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="f8e3d-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="f8e3d-113">.NET 4.5</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="f8e3d-114">Lernprogramm-Versionen</span><span class="sxs-lookup"><span data-stu-id="f8e3d-114">Tutorial versions</span></span>
> 
> <span data-ttu-id="f8e3d-115">OData-Version 3, finden Sie unter [OData-Aktionen in ASP.NET Web API 2](../odata-v3/odata-actions.md).</span><span class="sxs-lookup"><span data-stu-id="f8e3d-115">For OData Version 3, see [OData Actions in ASP.NET Web API 2](../odata-v3/odata-actions.md).</span></span>


<span data-ttu-id="f8e3d-116">Der Unterschied zwischen *Aktionen* und *Funktionen* besteht darin, dass Aktionen Nebeneffekte haben können und Funktionen nicht.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-116">The difference between *actions* and *functions* is that actions can have side effects, and functions do not.</span></span> <span data-ttu-id="f8e3d-117">Aktionen und Funktionen können Daten zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-117">Both actions and functions can return data.</span></span> <span data-ttu-id="f8e3d-118">Einige Verwendungsmöglichkeiten für Aktionen umfassen:</span><span class="sxs-lookup"><span data-stu-id="f8e3d-118">Some uses for actions include:</span></span>

- <span data-ttu-id="f8e3d-119">Komplexe Transaktionen.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-119">Complex transactions.</span></span>
- <span data-ttu-id="f8e3d-120">Bearbeiten gleichzeitig mehrere Entitäten.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-120">Manipulating several entities at once.</span></span>
- <span data-ttu-id="f8e3d-121">Erlauben nur bestimmte Eigenschaften einer Entität aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-121">Allowing updates only to certain properties of an entity.</span></span>
- <span data-ttu-id="f8e3d-122">Senden von Daten, die nicht auf eine Entität ist.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-122">Sending data that is not an entity.</span></span>

<span data-ttu-id="f8e3d-123">Funktionen sind hilfreich für die Rückgabe von Informationen, der nicht direkt an eine Entität oder eine Auflistung.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-123">Functions are useful for returning information that does not correspond directly to an entity or collection.</span></span>

<span data-ttu-id="f8e3d-124">Eine Aktion (oder Funktion) kann eine einzelne Entität oder eine Auflistung abzielen.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-124">An action (or function) can target a single entity or a collection.</span></span> <span data-ttu-id="f8e3d-125">In OData-Terminologie ist dies die *Bindung*.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-125">In OData terminology, this is the *binding*.</span></span> <span data-ttu-id="f8e3d-126">Sie können auch veranlassen &quot;ungebundenen&quot; Aktionen/Funktionen, die als statische Vorgänge im Dienst aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-126">You can also have &quot;unbound&quot; actions/functions, which are called as static operations on the service.</span></span>

## <a name="example-adding-an-action"></a><span data-ttu-id="f8e3d-127">Beispiel: Hinzufügen einer Aktion</span><span class="sxs-lookup"><span data-stu-id="f8e3d-127">Example: Adding an Action</span></span>

<span data-ttu-id="f8e3d-128">Definieren Sie eine Aktion, um ein Produkt zu bewerten.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-128">Let's define an action to rate a product.</span></span>

> [!NOTE]
> <span data-ttu-id="f8e3d-129">Dieses Tutorial baut auf dem Lernprogramm [erstellen Sie eine OData v4-Endpunkts mit ASP.NET-Web API 2](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="f8e3d-129">This tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>


<span data-ttu-id="f8e3d-130">Fügen Sie zunächst eine `ProductRating` Modell für die Bewertungen darstellen.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-130">First, add a `ProductRating` model to represent the ratings.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

<span data-ttu-id="f8e3d-131">Hinzufügen einer **"DbSet"** zu der `ProductsContext` Klasse, sodass EF eine Bewertungen-Tabelle in der Datenbank erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-131">Also add a **DbSet** to the `ProductsContext` class, so that EF will create a Ratings table in the database.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a><span data-ttu-id="f8e3d-132">Hinzufügen der Aktion zum EDM</span><span class="sxs-lookup"><span data-stu-id="f8e3d-132">Add the Action to the EDM</span></span>

<span data-ttu-id="f8e3d-133">Fügen Sie in WebApiConfig.cs den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="f8e3d-133">In WebApiConfig.cs, add the following code:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

<span data-ttu-id="f8e3d-134">Die **EntityTypeConfiguration.Action** Methode fügt eine Aktion mit den Entity Data Model (EDM).</span><span class="sxs-lookup"><span data-stu-id="f8e3d-134">The **EntityTypeConfiguration.Action** method adds an action to the entity data model (EDM).</span></span> <span data-ttu-id="f8e3d-135">Die **Parameter** Methode gibt einen typisierten Parameter für die Aktion an.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-135">The **Parameter** method specifies a typed parameter for the action.</span></span>

<span data-ttu-id="f8e3d-136">Dieser Code legt auch den Namespace für das EDM fest.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-136">This code also sets the namespace for the EDM.</span></span> <span data-ttu-id="f8e3d-137">Der Namespace ist wichtig, weil der URI für die Aktion den vollständig qualifizierten Aktionsnamen enthält:</span><span class="sxs-lookup"><span data-stu-id="f8e3d-137">The namespace matters because the URI for the action includes the fully-qualified action name:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> <span data-ttu-id="f8e3d-138">In einer typischen IIS-Konfiguration wird der Punkt in dieser URL dazu führen, dass IIS den Fehler 404 zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-138">In a typical IIS configuration, the dot in this URL will cause IIS to return error 404.</span></span> <span data-ttu-id="f8e3d-139">Sie können dies beheben, indem Sie den folgenden Abschnitt zu Ihrer Datei "Web.config" hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="f8e3d-139">You can resolve this by adding the following section to your Web.Config file:</span></span>

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a><span data-ttu-id="f8e3d-140">Fügen Sie eine Controllermethode für die Aktion hinzu.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-140">Add a Controller Method for the Action</span></span>

<span data-ttu-id="f8e3d-141">So aktivieren Sie die &quot;Rate&quot; Aktion fügen die folgende Methode zur `ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="f8e3d-141">To enable the &quot;Rate&quot; action, add the following method to `ProductsController`:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

<span data-ttu-id="f8e3d-142">Beachten Sie, dass der Methodenname der Name der Aktion entspricht.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-142">Notice that the method name matches the action name.</span></span> <span data-ttu-id="f8e3d-143">Die **[HttpPost]** Attribut gibt an, die Methode ist eine HTTP POST-Methode.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-143">The **[HttpPost]** attribute specifies the method is an HTTP POST method.</span></span>

<span data-ttu-id="f8e3d-144">Um die Aktion aufzurufen, sendet der Client eine HTTP POST-Anforderung wie folgt:</span><span class="sxs-lookup"><span data-stu-id="f8e3d-144">To invoke the action, the client sends an HTTP POST request like the following:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

<span data-ttu-id="f8e3d-145">Die &quot;Rate&quot; Aktion gebunden ist, die Product-Instanzen, der URI für die Aktion der vollqualifizierten Aktionsname, der an die Entität URI angefügt ist.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-145">The &quot;Rate&quot; action is bound to Product instances, so the URI for the action is the fully-qualified action name appended to the entity URI.</span></span> <span data-ttu-id="f8e3d-146">(Denken Sie daran, dass wir die EDM-Namespace, um festgelegt &quot;ProductService&quot;, deshalb ist der Name der Aktion den vollqualifizierten &quot;ProductService.Rate&quot;.)</span><span class="sxs-lookup"><span data-stu-id="f8e3d-146">(Recall that we set the EDM namespace to &quot;ProductService&quot;, so the fully-qualified action name is &quot;ProductService.Rate&quot;.)</span></span>

<span data-ttu-id="f8e3d-147">Der Text der Anforderung enthält der Action-Parameter als JSON-Nutzlast.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-147">The body of the request contains the action parameters as a JSON payload.</span></span> <span data-ttu-id="f8e3d-148">Web-API automatisch konvertiert die JSON-Nutzlast, die eine **ODataActionParameters** Objekt, das ist einfach ein Wörterbuch der Parameterwerte.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-148">Web API automatically converts the JSON payload to an **ODataActionParameters** object, which is just a dictionary of parameter values.</span></span> <span data-ttu-id="f8e3d-149">Verwenden Sie dieses Wörterbuch, um die Parameter in Ihre Controllermethode zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-149">Use this dictionary to access the parameters in your controller method.</span></span>

<span data-ttu-id="f8e3d-150">Wenn der Client die Aktionsparameter befinden sich am falschen sendet zu formatieren, den Wert der **ModelState.IsValid** ist "false".</span><span class="sxs-lookup"><span data-stu-id="f8e3d-150">If the client sends the action parameters in the wrong format, the value of **ModelState.IsValid** is false.</span></span> <span data-ttu-id="f8e3d-151">Aktivieren Sie dieses Flag in der Controllermethode, und geben Sie einen Fehler zurück, wenn **"IsValid"** ist "false".</span><span class="sxs-lookup"><span data-stu-id="f8e3d-151">Check this flag in your controller method and return an error if **IsValid** is false.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a><span data-ttu-id="f8e3d-152">Beispiel: Hinzufügen einer Funktion</span><span class="sxs-lookup"><span data-stu-id="f8e3d-152">Example: Adding a Function</span></span>

<span data-ttu-id="f8e3d-153">Nun fügen Sie eine OData-Funktion, die die teuerste Produkt zurück.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-153">Now let's add an OData function that returns the most expensive product.</span></span> <span data-ttu-id="f8e3d-154">Wie zuvor im ersten Schritt die Funktion EDM hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-154">As before, the first step is adding the function to the EDM.</span></span> <span data-ttu-id="f8e3d-155">Fügen Sie in WebApiConfig.cs den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-155">In WebApiConfig.cs, add the following code.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

<span data-ttu-id="f8e3d-156">In diesem Fall wird die Funktion an die Produkte, anstatt einzelne Produktinstanzen gebunden.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-156">In this case, the function is bound to the Products collection, rather than individual Product instances.</span></span> <span data-ttu-id="f8e3d-157">Clients rufen Sie die Funktion durch Senden einer GET-Anforderung:</span><span class="sxs-lookup"><span data-stu-id="f8e3d-157">Clients invoke the function by sending a GET request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

<span data-ttu-id="f8e3d-158">Hier ist der Controllermethode für diese Funktion:</span><span class="sxs-lookup"><span data-stu-id="f8e3d-158">Here is the controller method for this function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

<span data-ttu-id="f8e3d-159">Beachten Sie, dass der Name der Methode den Namen der Funktion entspricht.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-159">Notice that the method name matches the function name.</span></span> <span data-ttu-id="f8e3d-160">Die **[HttpGet]** Attribut gibt an, die Methode ist eine HTTP GET-Methode.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-160">The **[HttpGet]** attribute specifies the method is an HTTP GET method.</span></span>

<span data-ttu-id="f8e3d-161">So sieht die HTTP-Antwort aus:</span><span class="sxs-lookup"><span data-stu-id="f8e3d-161">Here is the HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a><span data-ttu-id="f8e3d-162">Beispiel: Hinzufügen einer ungebundenen-Funktion</span><span class="sxs-lookup"><span data-stu-id="f8e3d-162">Example: Adding an Unbound Function</span></span>

<span data-ttu-id="f8e3d-163">Im vorherige Beispiel wurde eine Funktion, die an eine Auflistung gebunden.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-163">The previous example was a function bound to a collection.</span></span> <span data-ttu-id="f8e3d-164">In diesem nächsten Beispiel erstellen wir eine *ungebundenen* Funktion.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-164">In this next example, we'll create an *unbound* function.</span></span> <span data-ttu-id="f8e3d-165">Nicht gebundene Funktionen werden als statische Vorgänge im Dienst aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-165">Unbound functions are called as static operations on the service.</span></span> <span data-ttu-id="f8e3d-166">Die Funktion in diesem Beispiel wird die Mehrwertsteuer für eine bestimmte Postleitzahl zurück.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-166">The function in this example will return the sales tax for a given postal code.</span></span>

<span data-ttu-id="f8e3d-167">Fügen Sie die Funktion zum EDM, in der WebApiConfig-Datei:</span><span class="sxs-lookup"><span data-stu-id="f8e3d-167">In the WebApiConfig file, add the function to the EDM:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

<span data-ttu-id="f8e3d-168">Beachten Sie, die wir Aufrufen **Funktion** direkt auf die **ODataModelBuilder**, statt den Entitätstyp oder einer Auflistung.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-168">Notice that we are calling **Function** directly on the **ODataModelBuilder**, instead of the entity type or collection.</span></span> <span data-ttu-id="f8e3d-169">Dies weist dem Modell-Generator, dass die Funktion aufgehoben wird.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-169">This tells the model builder that the function is unbound.</span></span>

<span data-ttu-id="f8e3d-170">Hier ist der Controllermethode, die die Funktion implementiert:</span><span class="sxs-lookup"><span data-stu-id="f8e3d-170">Here is the controller method that implements the function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

<span data-ttu-id="f8e3d-171">Es spielt keine Rolle, welche Web-API-Controller Sie diese Methode in platzieren.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-171">It does not matter which Web API controller you place this method in.</span></span> <span data-ttu-id="f8e3d-172">Konnte er platziert wurde `ProductsController`, oder definieren Sie einen separaten Controller.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-172">You could put it in `ProductsController`, or define a separate controller.</span></span> <span data-ttu-id="f8e3d-173">Die **[ODataRoute]** Attribut definiert die URI-Vorlage für die Funktion.</span><span class="sxs-lookup"><span data-stu-id="f8e3d-173">The **[ODataRoute]** attribute defines the URI template for the function.</span></span>

<span data-ttu-id="f8e3d-174">Hier ist eine beispielanforderung für den Client:</span><span class="sxs-lookup"><span data-stu-id="f8e3d-174">Here is an example client request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

<span data-ttu-id="f8e3d-175">Die HTTP-Antwort:</span><span class="sxs-lookup"><span data-stu-id="f8e3d-175">The HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
