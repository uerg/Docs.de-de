---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Aktionen und Funktionen in OData v4 mithilfe von ASP.NET Web-API 2.2 | Microsoft Docs
author: MikeWasson
description: "In OData sind die Aktionen und Funktionen eine Möglichkeit, serverseitige Verhalten hinzufügen, die nicht einfach als CRUD-Vorgänge für Entitäten definiert sind. In diesem Lernprogramm wird gezeigt, wie..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: 532362f0c0faaaf0cb0c04726856f0497e5261b5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a><span data-ttu-id="2c7ce-104">Aktionen und Funktionen in OData v4 mithilfe von ASP.NET Web-API 2.2</span><span class="sxs-lookup"><span data-stu-id="2c7ce-104">Actions and Functions in OData v4 Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="2c7ce-105">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2c7ce-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="2c7ce-106">In OData sind die Aktionen und Funktionen eine Möglichkeit, serverseitige Verhalten hinzufügen, die nicht einfach als CRUD-Vorgänge für Entitäten definiert sind.</span><span class="sxs-lookup"><span data-stu-id="2c7ce-106">In OData, actions and functions are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="2c7ce-107">Dieses Lernprogramm zeigt, wie ein OData v4-Endpunkt mithilfe von Web-API 2.2 Aktionen und Funktionen hinzu.</span><span class="sxs-lookup"><span data-stu-id="2c7ce-107">This tutorial shows how to add actions and functions to an OData v4 endpoint, using Web API 2.2.</span></span> <span data-ttu-id="2c7ce-108">Das Lernprogramm baut auf das Lernprogramm [erstellen Sie eine OData v4-Endpunkt mit ASP.NET-Web API 2](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="2c7ce-108">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="2c7ce-109">In diesem Lernprogramm verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="2c7ce-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="2c7ce-110">Web-API 2.2</span><span class="sxs-lookup"><span data-stu-id="2c7ce-110">Web API 2.2</span></span>
> - <span data-ttu-id="2c7ce-111">OData v4</span><span class="sxs-lookup"><span data-stu-id="2c7ce-111">OData v4</span></span>
> - [<span data-ttu-id="2c7ce-112">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="2c7ce-112">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="2c7ce-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="2c7ce-113">.NET 4.5</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="2c7ce-114">Lernprogramm-Versionen</span><span class="sxs-lookup"><span data-stu-id="2c7ce-114">Tutorial versions</span></span>
> 
> <span data-ttu-id="2c7ce-115">OData-Version 3, finden Sie unter [OData-Aktionen in ASP.NET Web API 2](../odata-v3/odata-actions.md).</span><span class="sxs-lookup"><span data-stu-id="2c7ce-115">For OData Version 3, see [OData Actions in ASP.NET Web API 2](../odata-v3/odata-actions.md).</span></span>


<span data-ttu-id="2c7ce-116">Der Unterschied zwischen *Aktionen* und *Funktionen* ist, dass Aktionen können Nebeneffekte haben, und die Funktionen nicht jedoch.</span><span class="sxs-lookup"><span data-stu-id="2c7ce-116">The difference between *actions* and *functions* is that actions can have side effects, and functions do not.</span></span> <span data-ttu-id="2c7ce-117">Aktionen und Funktionen können Daten zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="2c7ce-117">Both actions and functions can return data.</span></span> <span data-ttu-id="2c7ce-118">Einige Verwendungsmöglichkeiten für Aktionen aufgeführt:</span><span class="sxs-lookup"><span data-stu-id="2c7ce-118">Some uses for actions include:</span></span>

- <span data-ttu-id="2c7ce-119">Komplexen Transaktionen.</span><span class="sxs-lookup"><span data-stu-id="2c7ce-119">Complex transactions.</span></span>
- <span data-ttu-id="2c7ce-120">Bearbeiten gleichzeitig mehrere Entitäten.</span><span class="sxs-lookup"><span data-stu-id="2c7ce-120">Manipulating several entities at once.</span></span>
- <span data-ttu-id="2c7ce-121">Zulassen, dass Updates nur für bestimmte Eigenschaften einer Entität.</span><span class="sxs-lookup"><span data-stu-id="2c7ce-121">Allowing updates only to certain properties of an entity.</span></span>
- <span data-ttu-id="2c7ce-122">Senden von Daten, die eine Entität nicht vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="2c7ce-122">Sending data that is not an entity.</span></span>

<span data-ttu-id="2c7ce-123">Funktionen sind hilfreich für die Rückgabe von Informationen, die nicht entsprechen direkt an eine Entität oder eine Auflistung.</span><span class="sxs-lookup"><span data-stu-id="2c7ce-123">Functions are useful for returning information that does not correspond directly to an entity or collection.</span></span>

<span data-ttu-id="2c7ce-124">Eine Aktion (oder Funktion) kann eine einzelne Entität oder eine Auflistung abzielen.</span><span class="sxs-lookup"><span data-stu-id="2c7ce-124">An action (or function) can target a single entity or a collection.</span></span> <span data-ttu-id="2c7ce-125">In der Terminologie von OData-Dies ist die *Bindung*.</span><span class="sxs-lookup"><span data-stu-id="2c7ce-125">In OData terminology, this is the *binding*.</span></span> <span data-ttu-id="2c7ce-126">Sie können auch veranlassen &quot;ungebundenen&quot; Aktionen/Funktionen, die als statische Vorgänge für den Dienst aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="2c7ce-126">You can also have &quot;unbound&quot; actions/functions, which are called as static operations on the service.</span></span>

## <a name="example-adding-an-action"></a><span data-ttu-id="2c7ce-127">Beispiel: Eine Aktion hinzufügen</span><span class="sxs-lookup"><span data-stu-id="2c7ce-127">Example: Adding an Action</span></span>

<span data-ttu-id="2c7ce-128">Definieren Sie eine Aktion aus, um ein Produkt bewerten.</span><span class="sxs-lookup"><span data-stu-id="2c7ce-128">Let's define an action to rate a product.</span></span>

> [!NOTE]
> <span data-ttu-id="2c7ce-129">Dieses Lernprogramm baut auf das Lernprogramm [erstellen Sie eine OData v4-Endpunkt mit ASP.NET-Web API 2](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="2c7ce-129">This tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>


<span data-ttu-id="2c7ce-130">Fügen Sie zunächst eine `ProductRating` Modell, um die Bewertungen darstellen.</span><span class="sxs-lookup"><span data-stu-id="2c7ce-130">First, add a `ProductRating` model to represent the ratings.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

<span data-ttu-id="2c7ce-131">Auch hinzufügen, eine **DbSet** auf die `ProductsContext` Klasse, sodass EF eine Bewertungen-Tabelle in der Datenbank erstellen.</span><span class="sxs-lookup"><span data-stu-id="2c7ce-131">Also add a **DbSet** to the `ProductsContext` class, so that EF will create a Ratings table in the database.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a><span data-ttu-id="2c7ce-132">Fügen Sie die Aktion zum EDM</span><span class="sxs-lookup"><span data-stu-id="2c7ce-132">Add the Action to the EDM</span></span>

<span data-ttu-id="2c7ce-133">Fügen Sie in WebApiConfig.cs den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="2c7ce-133">In WebApiConfig.cs, add the following code:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

<span data-ttu-id="2c7ce-134">Die **EntityTypeConfiguration.Action** Methode fügt eine Aktion mit den Entity Data Model (EDM).</span><span class="sxs-lookup"><span data-stu-id="2c7ce-134">The **EntityTypeConfiguration.Action** method adds an action to the entity data model (EDM).</span></span> <span data-ttu-id="2c7ce-135">Die **Parameter** Methode gibt einen typisierten Parameter für die Aktion an.</span><span class="sxs-lookup"><span data-stu-id="2c7ce-135">The **Parameter** method specifies a typed parameter for the action.</span></span>

<span data-ttu-id="2c7ce-136">Dieser Code legt auch den Namespace für das EDM fest.</span><span class="sxs-lookup"><span data-stu-id="2c7ce-136">This code also sets the namespace for the EDM.</span></span> <span data-ttu-id="2c7ce-137">Der Namespace ist wichtig, weil der URI für die Aktion der eine vollqualifizierte Aktionsname enthält:</span><span class="sxs-lookup"><span data-stu-id="2c7ce-137">The namespace matters because the URI for the action includes the fully-qualified action name:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> <span data-ttu-id="2c7ce-138">In einer IIS-Standardkonfiguration wird der Punkt in dieser URL dazu führen, dass IIS den Fehler 404 zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="2c7ce-138">In a typical IIS configuration, the dot in this URL will cause IIS to return error 404.</span></span> <span data-ttu-id="2c7ce-139">Sie können dieses Problem umgehen, indem Sie im folgenden Abschnitt Ihrer Datei "Web.config" hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="2c7ce-139">You can resolve this by adding the following section to your Web.Config file:</span></span>

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a><span data-ttu-id="2c7ce-140">Fügen Sie einen Controllermethode für die Aktion hinzu</span><span class="sxs-lookup"><span data-stu-id="2c7ce-140">Add a Controller Method for the Action</span></span>

<span data-ttu-id="2c7ce-141">So aktivieren Sie die &quot;Rate&quot; Aktion, fügen Sie die folgende Methode hinzu `ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="2c7ce-141">To enable the &quot;Rate&quot; action, add the following method to `ProductsController`:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

<span data-ttu-id="2c7ce-142">Beachten Sie, dass der Name der Methode der Aktionsname entspricht.</span><span class="sxs-lookup"><span data-stu-id="2c7ce-142">Notice that the method name matches the action name.</span></span> <span data-ttu-id="2c7ce-143">Die **[HttpPost]** Attribut gibt an, die Methode ist eine HTTP POST-Methode.</span><span class="sxs-lookup"><span data-stu-id="2c7ce-143">The **[HttpPost]** attribute specifies the method is an HTTP POST method.</span></span>

<span data-ttu-id="2c7ce-144">Um die Aktion aufzurufen, sendet der Client eine HTTP POST-Anforderung wie folgt:</span><span class="sxs-lookup"><span data-stu-id="2c7ce-144">To invoke the action, the client sends an HTTP POST request like the following:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

<span data-ttu-id="2c7ce-145">Die &quot;Rate&quot; Aktion an Produktinstanzen, gebunden ist, damit der URI für die Aktion der vollqualifizierten Aktionsname, der auf die Entität URI angefügt ist.</span><span class="sxs-lookup"><span data-stu-id="2c7ce-145">The &quot;Rate&quot; action is bound to Product instances, so the URI for the action is the fully-qualified action name appended to the entity URI.</span></span> <span data-ttu-id="2c7ce-146">(Beachten Sie, dass wir den EDM-Namespace eingerichtet &quot;ProductService&quot;, also der voll gekennzeichneten Aktionsnamen &quot;ProductService.Rate&quot;.)</span><span class="sxs-lookup"><span data-stu-id="2c7ce-146">(Recall that we set the EDM namespace to &quot;ProductService&quot;, so the fully-qualified action name is &quot;ProductService.Rate&quot;.)</span></span>

<span data-ttu-id="2c7ce-147">Der Text der Anforderung enthält der Action-Parameter als ein JSON-Nutzlast.</span><span class="sxs-lookup"><span data-stu-id="2c7ce-147">The body of the request contains the action parameters as a JSON payload.</span></span> <span data-ttu-id="2c7ce-148">Web-API automatisch konvertiert, die JSON-Nutzlast ein **ODataActionParameters** Objekt, das nur ein Wörterbuch der Parameterwerte.</span><span class="sxs-lookup"><span data-stu-id="2c7ce-148">Web API automatically converts the JSON payload to an **ODataActionParameters** object, which is just a dictionary of parameter values.</span></span> <span data-ttu-id="2c7ce-149">Verwenden Sie dieses Wörterbuch, um die Parameter in der Controllermethode zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="2c7ce-149">Use this dictionary to access the parameters in your controller method.</span></span>

<span data-ttu-id="2c7ce-150">Wenn der Client sendet, der Action-Parameter in der falschen zu formatieren, den Wert der **ModelState.IsValid** lautet "false".</span><span class="sxs-lookup"><span data-stu-id="2c7ce-150">If the client sends the action parameters in the wrong format, the value of **ModelState.IsValid** is false.</span></span> <span data-ttu-id="2c7ce-151">Überprüfen Sie dieses Flag in der Controllermethode, und geben Sie einen Fehler zurück, wenn **IsValid** lautet "false".</span><span class="sxs-lookup"><span data-stu-id="2c7ce-151">Check this flag in your controller method and return an error if **IsValid** is false.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a><span data-ttu-id="2c7ce-152">Beispiel: Hinzufügen einer Funktion</span><span class="sxs-lookup"><span data-stu-id="2c7ce-152">Example: Adding a Function</span></span>

<span data-ttu-id="2c7ce-153">Nun fügen Sie eine OData-Funktion, die die teuerste Produkt zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="2c7ce-153">Now let's add an OData function that returns the most expensive product.</span></span> <span data-ttu-id="2c7ce-154">Wie zuvor im ersten Schritt die Funktion EDM hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="2c7ce-154">As before, the first step is adding the function to the EDM.</span></span> <span data-ttu-id="2c7ce-155">Fügen Sie in WebApiConfig.cs den folgenden Code ein.</span><span class="sxs-lookup"><span data-stu-id="2c7ce-155">In WebApiConfig.cs, add the following code.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

<span data-ttu-id="2c7ce-156">In diesem Fall wird die Funktion an die Sammlung von Produkten, anstatt einzelne Produktinstanzen gebunden.</span><span class="sxs-lookup"><span data-stu-id="2c7ce-156">In this case, the function is bound to the Products collection, rather than individual Product instances.</span></span> <span data-ttu-id="2c7ce-157">Clients rufen Sie die Funktion durch Senden einer GET-Anforderung:</span><span class="sxs-lookup"><span data-stu-id="2c7ce-157">Clients invoke the function by sending a GET request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

<span data-ttu-id="2c7ce-158">So sieht die Controllermethode für diese Funktion aus:</span><span class="sxs-lookup"><span data-stu-id="2c7ce-158">Here is the controller method for this function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

<span data-ttu-id="2c7ce-159">Beachten Sie, dass der Name der Methode den Namen der Funktion übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="2c7ce-159">Notice that the method name matches the function name.</span></span> <span data-ttu-id="2c7ce-160">Die **[HttpGet]** Attribut gibt an, die Methode ist eine HTTP GET-Methode.</span><span class="sxs-lookup"><span data-stu-id="2c7ce-160">The **[HttpGet]** attribute specifies the method is an HTTP GET method.</span></span>

<span data-ttu-id="2c7ce-161">So sieht die HTTP-Antwort aus:</span><span class="sxs-lookup"><span data-stu-id="2c7ce-161">Here is the HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a><span data-ttu-id="2c7ce-162">Beispiel: Hinzufügen einer ungebundenen-Funktion</span><span class="sxs-lookup"><span data-stu-id="2c7ce-162">Example: Adding an Unbound Function</span></span>

<span data-ttu-id="2c7ce-163">Im vorherige Beispiel wurde eine Funktion, die an eine Auflistung gebunden.</span><span class="sxs-lookup"><span data-stu-id="2c7ce-163">The previous example was a function bound to a collection.</span></span> <span data-ttu-id="2c7ce-164">Im nächsten Beispiel erstellen wir eine *ungebundenen* Funktion.</span><span class="sxs-lookup"><span data-stu-id="2c7ce-164">In this next example, we'll create an *unbound* function.</span></span> <span data-ttu-id="2c7ce-165">Nicht gebundene Funktionen werden als statische Vorgänge für den Dienst aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="2c7ce-165">Unbound functions are called as static operations on the service.</span></span> <span data-ttu-id="2c7ce-166">Die Funktion in diesem Beispiel wird die Steuer für eine bestimmte Postleitzahl zurück.</span><span class="sxs-lookup"><span data-stu-id="2c7ce-166">The function in this example will return the sales tax for a given postal code.</span></span>

<span data-ttu-id="2c7ce-167">Fügen Sie die Funktion zum EDM, in der Datei "webapiconfig":</span><span class="sxs-lookup"><span data-stu-id="2c7ce-167">In the WebApiConfig file, add the function to the EDM:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

<span data-ttu-id="2c7ce-168">Beachten Sie, die wir Aufrufen **Funktion** direkt auf die **ODataModelBuilder**statt der Entitätstyp oder einer Auflistung.</span><span class="sxs-lookup"><span data-stu-id="2c7ce-168">Notice that we are calling **Function** directly on the **ODataModelBuilder**, instead of the entity type or collection.</span></span> <span data-ttu-id="2c7ce-169">Dies weist dem Modell-Generator an, dass die Funktion aufgehoben wird.</span><span class="sxs-lookup"><span data-stu-id="2c7ce-169">This tells the model builder that the function is unbound.</span></span>

<span data-ttu-id="2c7ce-170">Hier ist die Controllermethode, die die Funktion implementiert:</span><span class="sxs-lookup"><span data-stu-id="2c7ce-170">Here is the controller method that implements the function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

<span data-ttu-id="2c7ce-171">Es spielt keine Rolle, welche Web-API-Controller Sie diese Methode in platzieren.</span><span class="sxs-lookup"><span data-stu-id="2c7ce-171">It does not matter which Web API controller you place this method in.</span></span> <span data-ttu-id="2c7ce-172">Sie konnten fügen Sie ihn in `ProductsController`, oder definieren Sie einen separaten Controller.</span><span class="sxs-lookup"><span data-stu-id="2c7ce-172">You could put it in `ProductsController`, or define a separate controller.</span></span> <span data-ttu-id="2c7ce-173">Die **[ODataRoute]** Attribut definiert die URI-Vorlage für die Funktion.</span><span class="sxs-lookup"><span data-stu-id="2c7ce-173">The **[ODataRoute]** attribute defines the URI template for the function.</span></span>

<span data-ttu-id="2c7ce-174">Hier ist eine Beispiel für Client-Anforderung:</span><span class="sxs-lookup"><span data-stu-id="2c7ce-174">Here is an example client request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

<span data-ttu-id="2c7ce-175">Die HTTP-Antwort:</span><span class="sxs-lookup"><span data-stu-id="2c7ce-175">The HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
