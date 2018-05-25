---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: Unterstützen von OData-Aktionen in ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: 'In OData sind Aktionen eine Möglichkeit, serverseitige Verhalten hinzufügen, die nicht einfach als CRUD-Vorgänge für Entitäten definiert sind. Einige Verwendungsmöglichkeiten für Aktionen aufgeführt: implementieren...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: acb369ca8f1bab8d7cad14c15f46cfd44beb9fdd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="supporting-odata-actions-in-aspnet-web-api-2"></a><span data-ttu-id="9bf2a-104">Unterstützen von OData-Aktionen in ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="9bf2a-104">Supporting OData Actions in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="9bf2a-105">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="9bf2a-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="9bf2a-106">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="9bf2a-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="9bf2a-107">In OData *Aktionen* bieten eine Möglichkeit, serverseitige Verhalten hinzufügen, die nicht einfach als CRUD-Vorgänge für Entitäten definiert sind.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-107">In OData, *actions* are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="9bf2a-108">Einige Verwendungsmöglichkeiten für Aktionen aufgeführt:</span><span class="sxs-lookup"><span data-stu-id="9bf2a-108">Some uses for actions include:</span></span>
> 
> - <span data-ttu-id="9bf2a-109">Implementieren von komplexen Transaktionen.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-109">Implementing complex transactions.</span></span>
> - <span data-ttu-id="9bf2a-110">Bearbeiten gleichzeitig mehrere Entitäten.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-110">Manipulating several entities at once.</span></span>
> - <span data-ttu-id="9bf2a-111">Zulassen, dass Updates nur für bestimmte Eigenschaften einer Entität.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-111">Allowing updates only to certain properties of an entity.</span></span>
> - <span data-ttu-id="9bf2a-112">Senden von Informationen an den Server, die nicht in einer Entität definiert ist.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-112">Sending information to the server that is not defined in an entity.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="9bf2a-113">In diesem Lernprogramm verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="9bf2a-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="9bf2a-114">Web-API 2</span><span class="sxs-lookup"><span data-stu-id="9bf2a-114">Web API 2</span></span>
> - <span data-ttu-id="9bf2a-115">OData-Version 3</span><span class="sxs-lookup"><span data-stu-id="9bf2a-115">OData Version 3</span></span>
> - <span data-ttu-id="9bf2a-116">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="9bf2a-116">Entity Framework 6</span></span>


## <a name="example-rating-a-product"></a><span data-ttu-id="9bf2a-117">Beispiel: Bewertung eines Produkts</span><span class="sxs-lookup"><span data-stu-id="9bf2a-117">Example: Rating a Product</span></span>

<span data-ttu-id="9bf2a-118">In diesem Beispiel möchten wir, damit Benutzer Produkte zu bewerten, und machen Sie dann die durchschnittsbewertungen für jedes Produkt.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-118">In this example, we want to let users rate products, and then expose the average ratings for each product.</span></span> <span data-ttu-id="9bf2a-119">In der Datenbank speichern wir eine Liste von Bewertungen, die als Schlüssel für Produkte.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-119">On the database, we will store a list of ratings, keyed to products.</span></span>

<span data-ttu-id="9bf2a-120">Hier wird das Modell, die zur Darstellung der Bewertungen in Entity Framework verwendet werden kann:</span><span class="sxs-lookup"><span data-stu-id="9bf2a-120">Here is the model we might use to represent the ratings in Entity Framework:</span></span>

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

<span data-ttu-id="9bf2a-121">Aber wir nicht möchten, dass Clients auf POST ein `ProductRating` Objekt, das eine Auflistung von "Bewertung".</span><span class="sxs-lookup"><span data-stu-id="9bf2a-121">But we don't want clients to POST a `ProductRating` object to a "Ratings" collection.</span></span> <span data-ttu-id="9bf2a-122">Intuitiv die Bewertung der Sammlung von Produkten zugeordnet ist, und der Client müssen nur den Bewertungswert zu senden.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-122">Intuitively, the rating is associated with the Products collection, and the client should only need to post the rating value.</span></span>

<span data-ttu-id="9bf2a-123">Aus diesem Grund definieren anstelle der normalen CRUD-Vorgänge wir eine Aktion, die ein Client aufrufen kann an einem Produkt.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-123">Therefore, instead of using the normal CRUD operations, we define an action that a client can invoke on a Product.</span></span> <span data-ttu-id="9bf2a-124">In der Terminologie von OData-Aktion ist *gebunden* Product-Entitäten.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-124">In OData terminology, the action is *bound* to Product entities.</span></span>

><span data-ttu-id="9bf2a-125">Aktionen verursachen Nebeneffekte auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-125">Actions have side-effects on the server.</span></span> <span data-ttu-id="9bf2a-126">Aus diesem Grund werden sie aufgerufen, indem HTTP POST-Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-126">For this reason, they are invoked using HTTP POST requests.</span></span> <span data-ttu-id="9bf2a-127">Aktionen möglich, Parameter und Rückgabetypen, die in den Dienstmetadaten beschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-127">Actions can have parameters and return types, which are described in the service metadata.</span></span> <span data-ttu-id="9bf2a-128">Der Client sendet die Parameter im Hauptteil Anforderung, und der Server sendet den Rückgabewert im Antworttext.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-128">The client sends the parameters in the request body, and the server sends the return value in the response body.</span></span> <span data-ttu-id="9bf2a-129">Um die Aktion "Rate Produkt" aufzurufen, sendet der Client einer POST-Anforderung an einen URI wie folgt:</span><span class="sxs-lookup"><span data-stu-id="9bf2a-129">To invoke the "Rate Product" action, the client sends a POST to a URI like the following:</span></span>

[!code-console[Main](odata-actions/samples/sample2.cmd)]

<span data-ttu-id="9bf2a-130">Die Daten in die POST-Anforderung ist einfach die Bewertung des Produkts:</span><span class="sxs-lookup"><span data-stu-id="9bf2a-130">The data in the POST request is simply the product rating:</span></span>

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a><span data-ttu-id="9bf2a-131">Deklarieren Sie die Aktion in das Entity Data Model</span><span class="sxs-lookup"><span data-stu-id="9bf2a-131">Declare the Action in the Entity Data Model</span></span>

<span data-ttu-id="9bf2a-132">Fügen Sie die Aktion in Ihrer Web-API-Konfiguration mit den Entity Data Model (EDM):</span><span class="sxs-lookup"><span data-stu-id="9bf2a-132">In your Web API configuration, add the action to the entity data model (EDM):</span></span>

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

<span data-ttu-id="9bf2a-133">Dieser Code definiert "RateProduct" als eine Aktion, die für die Product-Entitäten durchgeführt werden kann.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-133">This code defines "RateProduct" as an action that can be performed on Product entities.</span></span> <span data-ttu-id="9bf2a-134">Deklariert außerdem, dass die Aktion wird ein **Int** Parameter mit dem Namen "Bewertung", und gibt eine **Int** Wert.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-134">It also declares that the action takes an **int** parameter named "Rating", and returns an **int** value.</span></span>

## <a name="add-the-action-to-the-controller"></a><span data-ttu-id="9bf2a-135">Die Aktion an den Controller hinzufügen</span><span class="sxs-lookup"><span data-stu-id="9bf2a-135">Add the Action to the Controller</span></span>

<span data-ttu-id="9bf2a-136">Die Aktion "RateProduct" ist an der Product-Entitäten gebunden.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-136">The "RateProduct" action is bound to Product entities.</span></span> <span data-ttu-id="9bf2a-137">Um die Aktion zu implementieren, fügen Sie eine Methode namens `RateProduct` an den Controller Produkte:</span><span class="sxs-lookup"><span data-stu-id="9bf2a-137">To implement the action, add a method named `RateProduct` to the Products controller:</span></span>

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

<span data-ttu-id="9bf2a-138">Beachten Sie, dass der Name der Methode den Namen der Aktion im EDM übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-138">Notice that the method name matches the name of the action in the EDM.</span></span> <span data-ttu-id="9bf2a-139">Die Methode verfügt über zwei Parameter:</span><span class="sxs-lookup"><span data-stu-id="9bf2a-139">The method has two parameters:</span></span>

- <span data-ttu-id="9bf2a-140">*Schlüssel*: der Schlüssel für das Produkt zu Rate.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-140">*key*: The key for the product to rate.</span></span>
- <span data-ttu-id="9bf2a-141">*Parameter*: ein Wörterbuch von Parameterwerten für die Aktion.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-141">*parameters*: A dictionary of action parameter values.</span></span>

<span data-ttu-id="9bf2a-142">Wenn Sie standardroutingkonventionen verwenden, muss der Key-Parameter "Key" benannt werden.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-142">If you are using the default routing conventions, the key parameter must be named "key".</span></span> <span data-ttu-id="9bf2a-143">Es ist auch wichtig, enthalten die **[FromOdataUri]** Attribut, wie dargestellt.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-143">It is also important to include the **[FromOdataUri]** attribute, as shown.</span></span> <span data-ttu-id="9bf2a-144">Dieses Attribut weist Web API OData-Syntaxregeln verwenden, wenn es sich um den Schlüssel vom Anforderungs-URI analysiert werden.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-144">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

<span data-ttu-id="9bf2a-145">Verwenden der *Parameter* Wörterbuch zum Abrufen der Action-Parameter:</span><span class="sxs-lookup"><span data-stu-id="9bf2a-145">Use the *parameters* dictionary to get the action parameters:</span></span>

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

<span data-ttu-id="9bf2a-146">Wenn der Client sendet, der Action-Parameter in der richtigen zu formatieren, den Wert der **ModelState.IsValid** ist "true".</span><span class="sxs-lookup"><span data-stu-id="9bf2a-146">If the client sends the action parameters in the correct format, the value of **ModelState.IsValid** is true.</span></span> <span data-ttu-id="9bf2a-147">In diesem Fall können Sie die **ODataActionParameters** Wörterbuch, das die Werte der Parameter erhalten.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-147">In that case, you can use the **ODataActionParameters** dictionary to get the parameter values.</span></span> <span data-ttu-id="9bf2a-148">In diesem Beispiel wird die `RateProduct` Aktion nimmt einen Parameter mit dem Namen "Bewertung".</span><span class="sxs-lookup"><span data-stu-id="9bf2a-148">In this example, the `RateProduct` action takes a single parameter named "Rating".</span></span>

## <a name="action-metadata"></a><span data-ttu-id="9bf2a-149">Aktion-Metadaten</span><span class="sxs-lookup"><span data-stu-id="9bf2a-149">Action Metadata</span></span>

<span data-ttu-id="9bf2a-150">Um die Metadaten des Diensts anzuzeigen, senden Sie eine GET-Anforderung an /odata/$ Metadaten ein.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-150">To view the service metadata, send a GET request to /odata/$metadata.</span></span> <span data-ttu-id="9bf2a-151">Hier ist der Teil der Metadaten, die deklariert die `RateProduct` Aktion:</span><span class="sxs-lookup"><span data-stu-id="9bf2a-151">Here is the portion of the metadata that declares the `RateProduct` action:</span></span>

[!code-xml[Main](odata-actions/samples/sample7.xml)]

<span data-ttu-id="9bf2a-152">Die **FunctionImport** Element deklariert, die Aktion.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-152">The **FunctionImport** element declares the action.</span></span> <span data-ttu-id="9bf2a-153">Die meisten Felder sind selbsterklärend, doch es sind zwei Folgendes zu beachten:</span><span class="sxs-lookup"><span data-stu-id="9bf2a-153">Most of the fields are self-explanatory, but two are worth noting:</span></span>

- <span data-ttu-id="9bf2a-154">**IsBindable** bedeutet, dass die Aktion kann aufgerufen werden, in der Zielentität mindestens ein Teil der Zeit.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-154">**IsBindable** means the action can be invoked on the target entity, at least some of the time.</span></span>
- <span data-ttu-id="9bf2a-155">**IsAlwaysBindable** bedeutet, dass die Aktion kann für die Zielentität immer aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-155">**IsAlwaysBindable** means the action can always be invoked on the target entity.</span></span>

<span data-ttu-id="9bf2a-156">Der Unterschied ist, dass einige Aktionen immer für Clients verfügbar sind, aber andere Aktionen können von den Zustand der Entität abhängen.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-156">The difference is that some actions are always available to clients, but other actions might depend on the state of the entity.</span></span> <span data-ttu-id="9bf2a-157">Nehmen wir beispielsweise an, dass Sie eine Aktion "Einkauf" definieren.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-157">For example, suppose you define a "Purchase" action.</span></span> <span data-ttu-id="9bf2a-158">Sie können nur ein Element erwerben, die vorrätig ist.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-158">You can only purchase an item that is in stock.</span></span> <span data-ttu-id="9bf2a-159">Wenn das Element keine Lagerbestände mehr verfügbar ist, kann kein Client mit dieser Aktion aufrufen.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-159">If the item is out of stock, a client cannot invoke that action.</span></span>

<span data-ttu-id="9bf2a-160">Wenn Sie das EDM definieren die **Aktion** Methode erstellt eine Aktion immer gebunden werden kann:</span><span class="sxs-lookup"><span data-stu-id="9bf2a-160">When you define the EDM, the **Action** method creates an always-bindable action:</span></span>

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

<span data-ttu-id="9bf2a-161">Ich werde zu Aktionen Not stets-gebunden werden kann (so genannte *vorübergehenden* Aktionen) weiter unten in diesem Thema.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-161">I'll talk about not-always-bindable actions (also called *transient* actions) later in this topic.</span></span>

## <a name="invoking-the-action"></a><span data-ttu-id="9bf2a-162">Aufrufen der Aktion</span><span class="sxs-lookup"><span data-stu-id="9bf2a-162">Invoking the Action</span></span>

<span data-ttu-id="9bf2a-163">Jetzt sehen wir, wie ein Client dieser Aktion aufgerufen werden würde.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-163">Now let's see how a client would invoke this action.</span></span> <span data-ttu-id="9bf2a-164">Angenommen, das der Client möchte Geben Sie eine Bewertung von 2 für das Produkt mit der ID = 4.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-164">Suppose the client wants to give a rating of 2 to the product with ID = 4.</span></span> <span data-ttu-id="9bf2a-165">Hier ist eine Beispiel-Anforderungsnachricht, für Anforderungstext JSON-Format verwenden:</span><span class="sxs-lookup"><span data-stu-id="9bf2a-165">Here is an example request message, using JSON format for the request body:</span></span>

[!code-console[Main](odata-actions/samples/sample9.cmd)]

<span data-ttu-id="9bf2a-166">So sieht die Antwortnachricht aus:</span><span class="sxs-lookup"><span data-stu-id="9bf2a-166">Here is the response message:</span></span>

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a><span data-ttu-id="9bf2a-167">Binden eine Aktion an einer Entitätenmenge</span><span class="sxs-lookup"><span data-stu-id="9bf2a-167">Binding an Action to an Entity Set</span></span>

<span data-ttu-id="9bf2a-168">Im vorherigen Beispiel ist die Aktion für eine einzelne Entität gebunden: der Client bewertet ein einzelnes Produkts.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-168">In the previous example, the action is bound to a single entity: The client rates a single product.</span></span> <span data-ttu-id="9bf2a-169">Sie können auch eine Aktion an eine Auflistung von Entitäten binden.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-169">You can also bind an action to a collection of entities.</span></span> <span data-ttu-id="9bf2a-170">Nur die folgenden Änderungen vornehmen:</span><span class="sxs-lookup"><span data-stu-id="9bf2a-170">Just make the following changes:</span></span>

<span data-ttu-id="9bf2a-171">Fügen Sie die Aktion im EDM, auf der Entität **Auflistung** Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-171">In the EDM, add the action to the entity's **Collection** property.</span></span>

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

<span data-ttu-id="9bf2a-172">Lassen Sie die Controllermethode, die *Schlüssel* Parameter.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-172">In the controller method, omit the *key* parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

<span data-ttu-id="9bf2a-173">Nachdem der Client die Aktion auf die Entitätenmenge Produkte aufruft:</span><span class="sxs-lookup"><span data-stu-id="9bf2a-173">Now the client invokes the action on the Products entity set:</span></span>

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a><span data-ttu-id="9bf2a-174">Aktionen mit Parametern für die Datensammlung</span><span class="sxs-lookup"><span data-stu-id="9bf2a-174">Actions with Collection Parameters</span></span>

<span data-ttu-id="9bf2a-175">Aktionen können Parameter verfügen, die eine Auflistung von Werten annehmen.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-175">Actions can have parameters that take a collection of values.</span></span> <span data-ttu-id="9bf2a-176">Verwenden Sie im EDM **CollectionParameter&lt;T&gt;**  um den Parameter zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-176">In the EDM, use **CollectionParameter&lt;T&gt;** to declare the parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

<span data-ttu-id="9bf2a-177">Damit wird deklariert einen Parameter namens "Bewertung", die eine Auflistung von akzeptiert **Int** Werte.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-177">This declares a parameter named "Ratings" that takes a collection of **int** values.</span></span> <span data-ttu-id="9bf2a-178">In der Controllermethode erhalten Sie weiterhin den Wert des Parameters aus der **ODataActionParameters** Objekt aber jetzt der Wert ist ein **ICollection&lt;Int&gt;**  Wert:</span><span class="sxs-lookup"><span data-stu-id="9bf2a-178">In the controller method, you still get the parameter value from the **ODataActionParameters** object, but now the value is an **ICollection&lt;int&gt;** value:</span></span>

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a><span data-ttu-id="9bf2a-179">Vorübergehender Aktionen</span><span class="sxs-lookup"><span data-stu-id="9bf2a-179">Transient Actions</span></span>

<span data-ttu-id="9bf2a-180">Im Beispiel "RateProduct" können Benutzer immer ein Produkt bewerten, sodass die Aktion immer verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-180">In the "RateProduct" example, users can always rate a product, so the action is always available.</span></span> <span data-ttu-id="9bf2a-181">Aber einige Aktionen hängen von den Zustand der Entität.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-181">But some actions depend on the state of the entity.</span></span> <span data-ttu-id="9bf2a-182">Beispielsweise ist in einem Dienst videoverleih die Aktion "Kasse" nicht immer verfügbar.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-182">For example, in a video rental service, the "CheckOut" action is not always available.</span></span> <span data-ttu-id="9bf2a-183">(Es abhängt, ob eine Kopie dieses Video verfügbar ist.) Dieser Typ der Aktion wird aufgerufen, eine *vorübergehenden* Aktion.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-183">(It depends whether a copy of that video is available.) This type of action is called a *transient* action.</span></span>

<span data-ttu-id="9bf2a-184">In den Dienstmetadaten wurde eine vorübergehende Aktion **IsAlwaysBindable** gleich "false".</span><span class="sxs-lookup"><span data-stu-id="9bf2a-184">In the service metadata, a transient action has **IsAlwaysBindable** equal to false.</span></span> <span data-ttu-id="9bf2a-185">Die tatsächlich Wert ist der Standardwert, damit die Metadaten wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="9bf2a-185">That's actually the default value, so the metadata will look like this:</span></span>

[!code-xml[Main](odata-actions/samples/sample16.xml)]

<span data-ttu-id="9bf2a-186">Hier warum dies wichtig ist: Wenn eine Aktion vorübergehend ist, der Server muss dem Client feststellen, wann die Aktion verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-186">Here's why this matters: If an action is transient, the server needs to tell the client when the action is available.</span></span> <span data-ttu-id="9bf2a-187">Es wird ein Link zu der Aktion in der Entität eingeschlossen.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-187">It does this by including a link to the action in the entity.</span></span> <span data-ttu-id="9bf2a-188">Hier ist ein Beispiel für eine Entität Film:</span><span class="sxs-lookup"><span data-stu-id="9bf2a-188">Here is an example for a Movie entity:</span></span>

[!code-console[Main](odata-actions/samples/sample17.cmd)]

<span data-ttu-id="9bf2a-189">Die Eigenschaft "#CheckOut" enthält einen Link zu der Auscheckaktion.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-189">The "#CheckOut" property contains a link to the CheckOut action.</span></span> <span data-ttu-id="9bf2a-190">Wenn die Aktion nicht verfügbar ist, lässt der Server den Link aus.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-190">If the action is not available, the server omits the link.</span></span>

<span data-ttu-id="9bf2a-191">Um einen vorübergehenden Aktion im EDM zu deklarieren, rufen Sie die **TransientAction** Methode:</span><span class="sxs-lookup"><span data-stu-id="9bf2a-191">To declare a transient action in the EDM, call the **TransientAction** method:</span></span>

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

<span data-ttu-id="9bf2a-192">Darüber hinaus müssen Sie eine Funktion bereitstellen, die einen Aktionslink für eine bestimmte Entität zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-192">Also, you must provide a function that returns an action link for a given entity.</span></span> <span data-ttu-id="9bf2a-193">Legen Sie diese Funktion durch Aufrufen von **HasActionLink**.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-193">Set this function by calling **HasActionLink**.</span></span> <span data-ttu-id="9bf2a-194">Sie können die Funktion als Lambda-Ausdruck schreiben:</span><span class="sxs-lookup"><span data-stu-id="9bf2a-194">You can write the function as a lambda expression:</span></span>

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

<span data-ttu-id="9bf2a-195">Wenn die Aktion verfügbar ist, gibt der Lambda-Ausdruck einen Link mit der Aktion zurück.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-195">If the action is available, the lambda expression returns a link to the action.</span></span> <span data-ttu-id="9bf2a-196">Die OData-Serialisierer enthält diesen Link, wenn sie die Entität serialisiert.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-196">The OData serializer includes this link when it serializes the entity.</span></span> <span data-ttu-id="9bf2a-197">Wenn die Aktion nicht verfügbar ist, gibt die Funktion `null`.</span><span class="sxs-lookup"><span data-stu-id="9bf2a-197">When the action is not available, the function returns `null`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9bf2a-198">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="9bf2a-198">Additional Resources</span></span>

[<span data-ttu-id="9bf2a-199">Beispiel für OData-Aktionen</span><span class="sxs-lookup"><span data-stu-id="9bf2a-199">OData Actions Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
