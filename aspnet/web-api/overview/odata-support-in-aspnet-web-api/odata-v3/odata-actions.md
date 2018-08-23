---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: Unterstützen von OData-Aktionen in der ASP.NET-Web-API 2 | Microsoft-Dokumentation
author: MikeWasson
description: 'In OData sind Aktionen eine Möglichkeit, serverseitige Verhaltensweisen hinzufügen, die nicht einfach als CRUD-Vorgänge für Entitäten definiert sind. Einige Verwendungsmöglichkeiten für Aktionen umfassen: implementieren...'
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: 71c0f91f709aca0deb5548bdbcad60d79a2702f6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832102"
---
<a name="supporting-odata-actions-in-aspnet-web-api-2"></a><span data-ttu-id="e6fb1-104">Unterstützung von OData-Aktionen in der ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="e6fb1-104">Supporting OData Actions in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="e6fb1-105">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e6fb1-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="e6fb1-106">Abgeschlossenes Projekt herunterladen</span><span class="sxs-lookup"><span data-stu-id="e6fb1-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="e6fb1-107">In OData *Aktionen* sind eine Möglichkeit, serverseitige Verhaltensweisen hinzufügen, die nicht einfach als CRUD-Vorgänge für Entitäten definiert sind.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-107">In OData, *actions* are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="e6fb1-108">Einige Verwendungsmöglichkeiten für Aktionen umfassen:</span><span class="sxs-lookup"><span data-stu-id="e6fb1-108">Some uses for actions include:</span></span>
> 
> - <span data-ttu-id="e6fb1-109">Implementieren von komplexen Transaktionen.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-109">Implementing complex transactions.</span></span>
> - <span data-ttu-id="e6fb1-110">Bearbeiten gleichzeitig mehrere Entitäten.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-110">Manipulating several entities at once.</span></span>
> - <span data-ttu-id="e6fb1-111">Erlauben nur bestimmte Eigenschaften einer Entität aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-111">Allowing updates only to certain properties of an entity.</span></span>
> - <span data-ttu-id="e6fb1-112">Senden von Informationen an den Server, die nicht in einer Entität definiert wird.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-112">Sending information to the server that is not defined in an entity.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="e6fb1-113">Softwareversionen, die in diesem Tutorial verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="e6fb1-114">Web-API 2</span><span class="sxs-lookup"><span data-stu-id="e6fb1-114">Web API 2</span></span>
> - <span data-ttu-id="e6fb1-115">OData v3</span><span class="sxs-lookup"><span data-stu-id="e6fb1-115">OData Version 3</span></span>
> - <span data-ttu-id="e6fb1-116">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="e6fb1-116">Entity Framework 6</span></span>


## <a name="example-rating-a-product"></a><span data-ttu-id="e6fb1-117">Beispiel: Bewertung eines Produkts</span><span class="sxs-lookup"><span data-stu-id="e6fb1-117">Example: Rating a Product</span></span>

<span data-ttu-id="e6fb1-118">In diesem Beispiel möchten wir, damit Benutzer, die Produkte zu bewerten, und machen Sie dann die durchschnittliche Bewertung für jedes Produkt.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-118">In this example, we want to let users rate products, and then expose the average ratings for each product.</span></span> <span data-ttu-id="e6fb1-119">In der Datenbank werden wir eine Liste von Bewertungen auf Produkte verschlüsselt gespeichert.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-119">On the database, we will store a list of ratings, keyed to products.</span></span>

<span data-ttu-id="e6fb1-120">Hier ist das Modell, das wir verwenden können, um die Bewertungen im Entity Framework darzustellen:</span><span class="sxs-lookup"><span data-stu-id="e6fb1-120">Here is the model we might use to represent the ratings in Entity Framework:</span></span>

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

<span data-ttu-id="e6fb1-121">Wir möchten jedoch nicht POST-Clients eine `ProductRating` Objekt, das eine Auflistung von "Ratings".</span><span class="sxs-lookup"><span data-stu-id="e6fb1-121">But we don't want clients to POST a `ProductRating` object to a "Ratings" collection.</span></span> <span data-ttu-id="e6fb1-122">Intuitiv die Bewertung der Sammlung von Produkten zugeordnet ist, und der Client müssen nur den Bewertungswert bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-122">Intuitively, the rating is associated with the Products collection, and the client should only need to post the rating value.</span></span>

<span data-ttu-id="e6fb1-123">Aus diesem Grund definieren anstatt die normalen CRUD-Vorgänge zu verwenden, wir eine Aktion, die ein Client aufrufen kann, auf ein Produkt.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-123">Therefore, instead of using the normal CRUD operations, we define an action that a client can invoke on a Product.</span></span> <span data-ttu-id="e6fb1-124">In der OData-Terminologie, die Aktion ist *gebunden* zu Product-Entitäten.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-124">In OData terminology, the action is *bound* to Product entities.</span></span>

><span data-ttu-id="e6fb1-125">Aktionen verursachen Nebeneffekte auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-125">Actions have side-effects on the server.</span></span> <span data-ttu-id="e6fb1-126">Aus diesem Grund werden sie die mit HTTP POST-Anforderungen aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-126">For this reason, they are invoked using HTTP POST requests.</span></span> <span data-ttu-id="e6fb1-127">Aktionen möglich, Parameter und Rückgabetypen auf, die in den Metadaten beschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-127">Actions can have parameters and return types, which are described in the service metadata.</span></span> <span data-ttu-id="e6fb1-128">Der Client sendet die Parameter im Hauptteil Anforderung, und der Server sendet den Rückgabewert in den Antworttext.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-128">The client sends the parameters in the request body, and the server sends the return value in the response body.</span></span> <span data-ttu-id="e6fb1-129">Zum Aufrufen der Aktion "Rate Produkt" sendet der Client einen Beitrag zu einem URI wie folgt:</span><span class="sxs-lookup"><span data-stu-id="e6fb1-129">To invoke the "Rate Product" action, the client sends a POST to a URI like the following:</span></span>

[!code-console[Main](odata-actions/samples/sample2.cmd)]

<span data-ttu-id="e6fb1-130">Die Daten in der POST-Anforderung werden einfach die Produkt-Bewertung:</span><span class="sxs-lookup"><span data-stu-id="e6fb1-130">The data in the POST request is simply the product rating:</span></span>

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a><span data-ttu-id="e6fb1-131">Deklarieren Sie die Aktion in das Entity Data Model</span><span class="sxs-lookup"><span data-stu-id="e6fb1-131">Declare the Action in the Entity Data Model</span></span>

<span data-ttu-id="e6fb1-132">Fügen Sie in Ihrer Web-API-Konfiguration die Aktion mit den Entity Data Model (EDM) hinzu:</span><span class="sxs-lookup"><span data-stu-id="e6fb1-132">In your Web API configuration, add the action to the entity data model (EDM):</span></span>

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

<span data-ttu-id="e6fb1-133">Dieser Code definiert die "RateProduct" als eine Aktion, die für die Product-Entitäten ausgeführt werden kann.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-133">This code defines "RateProduct" as an action that can be performed on Product entities.</span></span> <span data-ttu-id="e6fb1-134">Deklariert außerdem, dass die Aktion eine **Int** Parameter mit dem Namen "Rating", und gibt eine **Int** Wert.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-134">It also declares that the action takes an **int** parameter named "Rating", and returns an **int** value.</span></span>

## <a name="add-the-action-to-the-controller"></a><span data-ttu-id="e6fb1-135">Hinzufügen der Aktions zum Controller</span><span class="sxs-lookup"><span data-stu-id="e6fb1-135">Add the Action to the Controller</span></span>

<span data-ttu-id="e6fb1-136">Die Aktion "RateProduct" ist an Product-Entitäten gebunden.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-136">The "RateProduct" action is bound to Product entities.</span></span> <span data-ttu-id="e6fb1-137">Um die Aktion zu implementieren, fügen Sie eine Methode, die mit dem Namen `RateProduct` an den Controller Produkte:</span><span class="sxs-lookup"><span data-stu-id="e6fb1-137">To implement the action, add a method named `RateProduct` to the Products controller:</span></span>

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

<span data-ttu-id="e6fb1-138">Beachten Sie, dass der Name der Methode mit dem Namen der Aktion im EDM übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-138">Notice that the method name matches the name of the action in the EDM.</span></span> <span data-ttu-id="e6fb1-139">Die Methode verfügt über zwei Parameter:</span><span class="sxs-lookup"><span data-stu-id="e6fb1-139">The method has two parameters:</span></span>

- <span data-ttu-id="e6fb1-140">*Schlüssel*: der Schlüssel für das Produkt zu Rate.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-140">*key*: The key for the product to rate.</span></span>
- <span data-ttu-id="e6fb1-141">*Parameter*: ein Wörterbuch mit Werten für Aktion-Parameter.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-141">*parameters*: A dictionary of action parameter values.</span></span>

<span data-ttu-id="e6fb1-142">Wenn Sie die Standard-routingkonventionen verwenden, muss der Key-Parameter "Key" benannt werden.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-142">If you are using the default routing conventions, the key parameter must be named "key".</span></span> <span data-ttu-id="e6fb1-143">Es ist auch wichtig, Sie enthalten die **[FromOdataUri]** Attribut, wie gezeigt.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-143">It is also important to include the **[FromOdataUri]** attribute, as shown.</span></span> <span data-ttu-id="e6fb1-144">Dieses Attribut teilt die Web-API-OData-Syntaxregeln beim Analysieren des Schlüssels aus der Anforderungs-URI zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-144">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

<span data-ttu-id="e6fb1-145">Verwenden der *Parameter* Wörterbuch, das die Aktionsparameter zu erhalten:</span><span class="sxs-lookup"><span data-stu-id="e6fb1-145">Use the *parameters* dictionary to get the action parameters:</span></span>

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

<span data-ttu-id="e6fb1-146">Wenn der Client der Action-Parameter in der richtigen sendet zu formatieren, den Wert der **ModelState.IsValid** ist "true".</span><span class="sxs-lookup"><span data-stu-id="e6fb1-146">If the client sends the action parameters in the correct format, the value of **ModelState.IsValid** is true.</span></span> <span data-ttu-id="e6fb1-147">In diesem Fall können Sie die **ODataActionParameters** Wörterbuch, das die Parameterwerte abzurufen.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-147">In that case, you can use the **ODataActionParameters** dictionary to get the parameter values.</span></span> <span data-ttu-id="e6fb1-148">In diesem Beispiel die `RateProduct` Aktion nimmt einen einzelnen Parameter, die mit dem Namen "Rating".</span><span class="sxs-lookup"><span data-stu-id="e6fb1-148">In this example, the `RateProduct` action takes a single parameter named "Rating".</span></span>

## <a name="action-metadata"></a><span data-ttu-id="e6fb1-149">Aktion-Metadaten</span><span class="sxs-lookup"><span data-stu-id="e6fb1-149">Action Metadata</span></span>

<span data-ttu-id="e6fb1-150">Um die Metadaten des Diensts anzuzeigen, senden Sie eine GET-Anforderung an /odata/$ Metadaten ein.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-150">To view the service metadata, send a GET request to /odata/$metadata.</span></span> <span data-ttu-id="e6fb1-151">Dies ist der Teil der Metadaten, die deklariert die `RateProduct` Aktion:</span><span class="sxs-lookup"><span data-stu-id="e6fb1-151">Here is the portion of the metadata that declares the `RateProduct` action:</span></span>

[!code-xml[Main](odata-actions/samples/sample7.xml)]

<span data-ttu-id="e6fb1-152">Die **FunctionImport** Element deklariert, die Aktion.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-152">The **FunctionImport** element declares the action.</span></span> <span data-ttu-id="e6fb1-153">Die meisten Felder sind selbsterklärend, doch die beiden sind zu beachten:</span><span class="sxs-lookup"><span data-stu-id="e6fb1-153">Most of the fields are self-explanatory, but two are worth noting:</span></span>

- <span data-ttu-id="e6fb1-154">**IsBindable** bedeutet, dass die Aktion kann aufgerufen werden, für die Zielentität, mindestens ein Teil der Zeit.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-154">**IsBindable** means the action can be invoked on the target entity, at least some of the time.</span></span>
- <span data-ttu-id="e6fb1-155">**IsAlwaysBindable** bedeutet, dass die Aktion kann für die Zielentität immer aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-155">**IsAlwaysBindable** means the action can always be invoked on the target entity.</span></span>

<span data-ttu-id="e6fb1-156">Der Unterschied besteht darin, dass einige Aktionen immer für Clients verfügbar sind, aber andere Aktionen vom Zustand der Entität abhängig können.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-156">The difference is that some actions are always available to clients, but other actions might depend on the state of the entity.</span></span> <span data-ttu-id="e6fb1-157">Nehmen wir beispielsweise an, dass Sie eine Aktion "Kaufen" definieren.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-157">For example, suppose you define a "Purchase" action.</span></span> <span data-ttu-id="e6fb1-158">Sie können nur ein Element erwerben, die auf Lager ist.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-158">You can only purchase an item that is in stock.</span></span> <span data-ttu-id="e6fb1-159">Wenn der Artikel nicht vorrätig ist, kann kein Client mit dieser Aktion aufrufen.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-159">If the item is out of stock, a client cannot invoke that action.</span></span>

<span data-ttu-id="e6fb1-160">Wenn Sie das EDM definieren die **Aktion** Methode erstellt eine Aktion immer gebunden werden kann:</span><span class="sxs-lookup"><span data-stu-id="e6fb1-160">When you define the EDM, the **Action** method creates an always-bindable action:</span></span>

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

<span data-ttu-id="e6fb1-161">Ich werde nicht immer – bindbare Aktionen (so genannte *vorübergehende* Aktionen) weiter unten in diesem Thema.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-161">I'll talk about not-always-bindable actions (also called *transient* actions) later in this topic.</span></span>

## <a name="invoking-the-action"></a><span data-ttu-id="e6fb1-162">Aufrufen der Aktion</span><span class="sxs-lookup"><span data-stu-id="e6fb1-162">Invoking the Action</span></span>

<span data-ttu-id="e6fb1-163">Jetzt sehen wir uns an, wie ein Client dadurch aufgerufen würde.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-163">Now let's see how a client would invoke this action.</span></span> <span data-ttu-id="e6fb1-164">Angenommen, das der Client möchte gerne eine Bewertung von 2 für das Produkt mit der ID = 4.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-164">Suppose the client wants to give a rating of 2 to the product with ID = 4.</span></span> <span data-ttu-id="e6fb1-165">Hier ist eine Beispiel-Anforderungsnachricht, die mithilfe von JSON-Format für den Anforderungstext ein:</span><span class="sxs-lookup"><span data-stu-id="e6fb1-165">Here is an example request message, using JSON format for the request body:</span></span>

[!code-console[Main](odata-actions/samples/sample9.cmd)]

<span data-ttu-id="e6fb1-166">So sieht die Response-Nachricht aus:</span><span class="sxs-lookup"><span data-stu-id="e6fb1-166">Here is the response message:</span></span>

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a><span data-ttu-id="e6fb1-167">Binden eine Aktion an einer Entitätenmenge</span><span class="sxs-lookup"><span data-stu-id="e6fb1-167">Binding an Action to an Entity Set</span></span>

<span data-ttu-id="e6fb1-168">Im vorherigen Beispiel ist die Aktion an eine einzelne Entität gebunden ist: der Client bewertet, ein einzelnes Produkt.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-168">In the previous example, the action is bound to a single entity: The client rates a single product.</span></span> <span data-ttu-id="e6fb1-169">Sie können auch eine Aktion an eine Auflistung von Entitäten binden.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-169">You can also bind an action to a collection of entities.</span></span> <span data-ttu-id="e6fb1-170">Stellen Sie einfach die folgenden Änderungen:</span><span class="sxs-lookup"><span data-stu-id="e6fb1-170">Just make the following changes:</span></span>

<span data-ttu-id="e6fb1-171">Im EDM hinzufügen der Aktion auf der Entität **Auflistung** Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-171">In the EDM, add the action to the entity's **Collection** property.</span></span>

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

<span data-ttu-id="e6fb1-172">Lassen Sie in der Controllermethode, die *Schlüssel* Parameter.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-172">In the controller method, omit the *key* parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

<span data-ttu-id="e6fb1-173">Nun ruft der Client die Aktion auf die Entitätenmenge Produkte:</span><span class="sxs-lookup"><span data-stu-id="e6fb1-173">Now the client invokes the action on the Products entity set:</span></span>

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a><span data-ttu-id="e6fb1-174">Aktionen mit Parametern für die Datensammlung</span><span class="sxs-lookup"><span data-stu-id="e6fb1-174">Actions with Collection Parameters</span></span>

<span data-ttu-id="e6fb1-175">Aktionen können Parameter aufweisen, die eine Auflistung von Werten zu akzeptieren.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-175">Actions can have parameters that take a collection of values.</span></span> <span data-ttu-id="e6fb1-176">Verwenden Sie im EDM **CollectionParameter&lt;T&gt;**  um den Parameter zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-176">In the EDM, use **CollectionParameter&lt;T&gt;** to declare the parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

<span data-ttu-id="e6fb1-177">Damit wird deklariert einen Parameter namens "Ratings", die eine Auflistung von akzeptiert **Int** Werte.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-177">This declares a parameter named "Ratings" that takes a collection of **int** values.</span></span> <span data-ttu-id="e6fb1-178">In der Controllermethode, erhalten Sie immer noch den Wert des Parameters aus der **ODataActionParameters** -Objekt, aber jetzt der Wert ist eine **ICollection&lt;Int&gt;**  Wert:</span><span class="sxs-lookup"><span data-stu-id="e6fb1-178">In the controller method, you still get the parameter value from the **ODataActionParameters** object, but now the value is an **ICollection&lt;int&gt;** value:</span></span>

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a><span data-ttu-id="e6fb1-179">Vorübergehende Aktionen</span><span class="sxs-lookup"><span data-stu-id="e6fb1-179">Transient Actions</span></span>

<span data-ttu-id="e6fb1-180">Im Beispiel "RateProduct" können Benutzer ein Produkt, immer bewerten, damit die Aktion immer verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-180">In the "RateProduct" example, users can always rate a product, so the action is always available.</span></span> <span data-ttu-id="e6fb1-181">Aber einige Aktionen abhängig vom Zustand der Entität.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-181">But some actions depend on the state of the entity.</span></span> <span data-ttu-id="e6fb1-182">Beispielsweise ist in einem Dienst videoverleih die Aktion "Kasse" nicht immer verfügbar.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-182">For example, in a video rental service, the "CheckOut" action is not always available.</span></span> <span data-ttu-id="e6fb1-183">(Diese hängt davon ab, ob eine Kopie des betreffenden Videos verfügbar ist.) Diese Art von Aktion wird aufgerufen, eine *vorübergehende* Aktion.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-183">(It depends whether a copy of that video is available.) This type of action is called a *transient* action.</span></span>

<span data-ttu-id="e6fb1-184">In den Dienstmetadaten eine vorübergehende Aktion verfügt **IsAlwaysBindable** gleich "false".</span><span class="sxs-lookup"><span data-stu-id="e6fb1-184">In the service metadata, a transient action has **IsAlwaysBindable** equal to false.</span></span> <span data-ttu-id="e6fb1-185">Das ist eigentlich den Standardwert, damit die Metadaten wie folgt aussehen wird:</span><span class="sxs-lookup"><span data-stu-id="e6fb1-185">That's actually the default value, so the metadata will look like this:</span></span>

[!code-xml[Main](odata-actions/samples/sample16.xml)]

<span data-ttu-id="e6fb1-186">Der folgende warum dies wichtig ist: Wenn eine Aktion vorübergehend auftritt, ist der Server muss dem Client weiß, dass die Aktion verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-186">Here's why this matters: If an action is transient, the server needs to tell the client when the action is available.</span></span> <span data-ttu-id="e6fb1-187">Hierzu werden ein Link zu der Aktion in der Entität eingeschlossen.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-187">It does this by including a link to the action in the entity.</span></span> <span data-ttu-id="e6fb1-188">Hier ist ein Beispiel für eine Movie-Entität:</span><span class="sxs-lookup"><span data-stu-id="e6fb1-188">Here is an example for a Movie entity:</span></span>

[!code-console[Main](odata-actions/samples/sample17.cmd)]

<span data-ttu-id="e6fb1-189">Die Eigenschaft "#CheckOut" enthält einen Link zu der CheckOut-Aktion.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-189">The "#CheckOut" property contains a link to the CheckOut action.</span></span> <span data-ttu-id="e6fb1-190">Wenn die Aktion nicht verfügbar ist, lässt der Server den Link aus.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-190">If the action is not available, the server omits the link.</span></span>

<span data-ttu-id="e6fb1-191">Um eine vorübergehende Aktion im EDM zu deklarieren, rufen Sie die **TransientAction** Methode:</span><span class="sxs-lookup"><span data-stu-id="e6fb1-191">To declare a transient action in the EDM, call the **TransientAction** method:</span></span>

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

<span data-ttu-id="e6fb1-192">Darüber hinaus müssen Sie eine Funktion bereitstellen, die einen Aktionslink für eine bestimmte Entität zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-192">Also, you must provide a function that returns an action link for a given entity.</span></span> <span data-ttu-id="e6fb1-193">Legen Sie diese Funktion durch den Aufruf **HasActionLink**.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-193">Set this function by calling **HasActionLink**.</span></span> <span data-ttu-id="e6fb1-194">Sie können die Funktion als Lambda-Ausdruck schreiben:</span><span class="sxs-lookup"><span data-stu-id="e6fb1-194">You can write the function as a lambda expression:</span></span>

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

<span data-ttu-id="e6fb1-195">Wenn die Aktion verfügbar ist, gibt der Lambda-Ausdruck einen Link auf die Aktion zurück.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-195">If the action is available, the lambda expression returns a link to the action.</span></span> <span data-ttu-id="e6fb1-196">Die OData-Serialisierer enthält diesen Link an, beim Serialisieren der Entitäts.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-196">The OData serializer includes this link when it serializes the entity.</span></span> <span data-ttu-id="e6fb1-197">Wenn die Aktion nicht verfügbar ist, wird die Funktion gibt `null`.</span><span class="sxs-lookup"><span data-stu-id="e6fb1-197">When the action is not available, the function returns `null`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e6fb1-198">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="e6fb1-198">Additional Resources</span></span>

[<span data-ttu-id="e6fb1-199">Beispiel für OData-Aktionen</span><span class="sxs-lookup"><span data-stu-id="e6fb1-199">OData Actions Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
