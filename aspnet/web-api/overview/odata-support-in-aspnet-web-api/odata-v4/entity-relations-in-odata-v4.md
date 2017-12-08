---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
title: "Entitätsbeziehungen in OData v4 mithilfe von ASP.NET Web-API 2.2 | Microsoft Docs"
author: MikeWasson
description: "Die meisten Datasets definieren die Beziehungen zwischen Entitäten: Kunden Bestellungen; haben. Bücher weisen Autoren; Produkte wurden Lieferanten. Verwendung von OData können Clients über navigieren..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2014
ms.topic: article
ms.assetid: 72657550-ec09-4779-9bfc-2fb15ecd51c7
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 4127fb2fa83f513a4faeb222068ca99f234ece22
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="entity-relations-in-odata-v4-using-aspnet-web-api-22"></a><span data-ttu-id="e3544-104">Entitätsbeziehungen in OData v4 mithilfe von ASP.NET Web-API 2.2</span><span class="sxs-lookup"><span data-stu-id="e3544-104">Entity Relations in OData v4 Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="e3544-105">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e3544-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="e3544-106">Die meisten Datasets definieren die Beziehungen zwischen Entitäten: Kunden Bestellungen; haben. Bücher weisen Autoren; Produkte wurden Lieferanten.</span><span class="sxs-lookup"><span data-stu-id="e3544-106">Most data sets define relations between entities: Customers have orders; books have authors; products have suppliers.</span></span> <span data-ttu-id="e3544-107">Verwendung von OData können Clients über entitätsbeziehungen navigieren.</span><span class="sxs-lookup"><span data-stu-id="e3544-107">Using OData, clients can navigate over entity relations.</span></span> <span data-ttu-id="e3544-108">Wenn Sie ein Produkt, können Sie den Lieferanten suchen.</span><span class="sxs-lookup"><span data-stu-id="e3544-108">Given a product, you can find the supplier.</span></span> <span data-ttu-id="e3544-109">Sie können auch erstellen oder Beziehungen entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="e3544-109">You can also create or remove relationships.</span></span> <span data-ttu-id="e3544-110">Beispielsweise können Sie den Lieferanten für ein Produkt festlegen.</span><span class="sxs-lookup"><span data-stu-id="e3544-110">For example, you can set the supplier for a product.</span></span>
> 
> <span data-ttu-id="e3544-111">Dieses Lernprogramm zeigt, wie zur Unterstützung dieser Vorgänge in OData v4 mithilfe der ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="e3544-111">This tutorial shows how to support these operations in OData v4 using ASP.NET Web API.</span></span> <span data-ttu-id="e3544-112">Das Lernprogramm baut auf das Lernprogramm [erstellen Sie eine OData v4-Endpunkt mit ASP.NET-Web API 2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="e3544-112">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="e3544-113">In diesem Lernprogramm verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="e3544-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="e3544-114">Web-API 2.1</span><span class="sxs-lookup"><span data-stu-id="e3544-114">Web API 2.1</span></span>
> - <span data-ttu-id="e3544-115">OData v4</span><span class="sxs-lookup"><span data-stu-id="e3544-115">OData v4</span></span>
> - [<span data-ttu-id="e3544-116">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="e3544-116">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="e3544-117">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="e3544-117">Entity Framework 6</span></span>
> - <span data-ttu-id="e3544-118">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="e3544-118">.NET 4.5</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="e3544-119">Lernprogramm-Versionen</span><span class="sxs-lookup"><span data-stu-id="e3544-119">Tutorial versions</span></span>
> 
> <span data-ttu-id="e3544-120">Die OData-Version 3, finden Sie unter [Entitätsbeziehungen in OData v3 unterstützen](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).</span><span class="sxs-lookup"><span data-stu-id="e3544-120">For the OData Version 3, see [Supporting Entity Relations in OData v3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).</span></span>


## <a name="add-a-supplier-entity"></a><span data-ttu-id="e3544-121">Hinzufügen einer Entität "Supplier"</span><span class="sxs-lookup"><span data-stu-id="e3544-121">Add a Supplier Entity</span></span>

> [!NOTE]
> <span data-ttu-id="e3544-122">Das Lernprogramm baut auf das Lernprogramm [erstellen Sie eine OData v4-Endpunkt mit ASP.NET-Web API 2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="e3544-122">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span></span>


<span data-ttu-id="e3544-123">Zunächst benötigen wir eine verknüpfte Entität.</span><span class="sxs-lookup"><span data-stu-id="e3544-123">First, we need a related entity.</span></span> <span data-ttu-id="e3544-124">Fügen Sie eine Klasse mit dem Namen `Supplier` im Ordner "Modelle".</span><span class="sxs-lookup"><span data-stu-id="e3544-124">Add a class named `Supplier` in the Models folder.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample1.cs)]

<span data-ttu-id="e3544-125">Hinzufügen eine Navigationseigenschaft an die `Product` Klasse:</span><span class="sxs-lookup"><span data-stu-id="e3544-125">Add a navigation property to the `Product` class:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample2.cs?highlight=13-15)]

<span data-ttu-id="e3544-126">Fügen Sie einen neuen **DbSet** auf die `ProductsContext` Klasse, sodass Entity Framework die Lieferanten-Tabelle in der Datenbank berücksichtigt werden.</span><span class="sxs-lookup"><span data-stu-id="e3544-126">Add a new **DbSet** to the `ProductsContext` class, so that Entity Framework will include the Supplier table in the database.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample3.cs?highlight=10)]

<span data-ttu-id="e3544-127">Fügen Sie in WebApiConfig.cs, eine &quot;Lieferanten&quot; Entitätenmenge mit dem Entity Data Model:</span><span class="sxs-lookup"><span data-stu-id="e3544-127">In WebApiConfig.cs, add a &quot;Suppliers&quot; entity set to the entity data model:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample4.cs?highlight=6)]

## <a name="add-a-suppliers-controller"></a><span data-ttu-id="e3544-128">Fügen Sie einen Controller Lieferanten</span><span class="sxs-lookup"><span data-stu-id="e3544-128">Add a Suppliers Controller</span></span>

<span data-ttu-id="e3544-129">Hinzufügen einer `SuppliersController` Klasse, um den Ordner Controllers.</span><span class="sxs-lookup"><span data-stu-id="e3544-129">Add a `SuppliersController` class to the Controllers folder.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample5.cs)]

<span data-ttu-id="e3544-130">Gewusst wie: Hinzufügen von CRUD-Vorgänge für diesen Controller zeigen nicht an.</span><span class="sxs-lookup"><span data-stu-id="e3544-130">I won't show how to add CRUD operations for this controller.</span></span> <span data-ttu-id="e3544-131">Die Schritte entsprechen den Produkts-Controllers (siehe [erstellen Sie einen OData v4-Endpunkt](create-an-odata-v4-endpoint.md)).</span><span class="sxs-lookup"><span data-stu-id="e3544-131">The steps are the same as for the Products controller (see [Create an OData v4 Endpoint](create-an-odata-v4-endpoint.md)).</span></span>

## <a name="getting-related-entities"></a><span data-ttu-id="e3544-132">Beim Abrufen der verknüpften Entitäten</span><span class="sxs-lookup"><span data-stu-id="e3544-132">Getting Related Entities</span></span>

<span data-ttu-id="e3544-133">Um den Lieferanten für ein Produkt zu erhalten, sendet der Client eine GET-Anforderung:</span><span class="sxs-lookup"><span data-stu-id="e3544-133">To get the supplier for a product, the client sends a GET request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample6.cmd)]

<span data-ttu-id="e3544-134">Um diese Anforderung zu unterstützen, fügen Sie die folgende Methode, die die `ProductsController` Klasse:</span><span class="sxs-lookup"><span data-stu-id="e3544-134">To support this request, add the following method to the `ProductsController` class:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample7.cs)]

<span data-ttu-id="e3544-135">Diese Methode verwendet eine standardmäßige Benennungskonvention</span><span class="sxs-lookup"><span data-stu-id="e3544-135">This method uses a default naming convention</span></span>

- <span data-ttu-id="e3544-136">Methodenname: GetX, wobei X die Navigationseigenschaft ist.</span><span class="sxs-lookup"><span data-stu-id="e3544-136">Method name: GetX, where X is the navigation property.</span></span>
- <span data-ttu-id="e3544-137">Parametername: *Schlüssel*</span><span class="sxs-lookup"><span data-stu-id="e3544-137">Parameter name: *key*</span></span>

<span data-ttu-id="e3544-138">Wenn Sie diese Benennungskonvention befolgen, werden die HTTP-Anforderung an die Controllermethode automatisch von Web-API zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="e3544-138">If you follow this naming convention, Web API automatically maps the HTTP request to the controller method.</span></span>

<span data-ttu-id="e3544-139">Für HTTP-beispielanforderung:</span><span class="sxs-lookup"><span data-stu-id="e3544-139">Example HTTP request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample8.cmd)]

<span data-ttu-id="e3544-140">Für HTTP-Beispielantwort:</span><span class="sxs-lookup"><span data-stu-id="e3544-140">Example HTTP response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample9.cmd)]

### <a name="getting-a-related-collection"></a><span data-ttu-id="e3544-141">Abrufen einer verknüpften Sammlung</span><span class="sxs-lookup"><span data-stu-id="e3544-141">Getting a related collection</span></span>

<span data-ttu-id="e3544-142">Im vorherigen Beispiel wurde ein Produkt eine Lieferanten.</span><span class="sxs-lookup"><span data-stu-id="e3544-142">In the previous example, a product has one supplier.</span></span> <span data-ttu-id="e3544-143">Eine Navigationseigenschaft kann auch eine Auflistung zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="e3544-143">A navigation property can also return a collection.</span></span> <span data-ttu-id="e3544-144">Der folgende Code Ruft die Produkte für ein Lieferant ab:</span><span class="sxs-lookup"><span data-stu-id="e3544-144">The following code gets the products for a supplier:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample10.cs)]

<span data-ttu-id="e3544-145">In diesem Fall die Methode gibt ein **IQueryable** anstelle von einer **SingleResult&lt;T&gt;**</span><span class="sxs-lookup"><span data-stu-id="e3544-145">In this case, the method returns an **IQueryable** instead of a **SingleResult&lt;T&gt;**</span></span>

<span data-ttu-id="e3544-146">Für HTTP-beispielanforderung:</span><span class="sxs-lookup"><span data-stu-id="e3544-146">Example HTTP request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample11.cmd)]

<span data-ttu-id="e3544-147">Für HTTP-Beispielantwort:</span><span class="sxs-lookup"><span data-stu-id="e3544-147">Example HTTP response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample12.cmd)]

## <a name="creating-a-relationship-between-entities"></a><span data-ttu-id="e3544-148">Erstellen eine Beziehung zwischen Entitäten</span><span class="sxs-lookup"><span data-stu-id="e3544-148">Creating a Relationship Between Entities</span></span>

<span data-ttu-id="e3544-149">OData unterstützt das Erstellen oder Entfernen von Beziehungen zwischen zwei vorhandene Entitäten.</span><span class="sxs-lookup"><span data-stu-id="e3544-149">OData supports creating or removing relationships between two existing entities.</span></span> <span data-ttu-id="e3544-150">In OData v4-Terminologie, die Beziehung ist eine &quot;Verweis&quot;.</span><span class="sxs-lookup"><span data-stu-id="e3544-150">In OData v4 terminology, the relationship is a &quot;reference&quot;.</span></span> <span data-ttu-id="e3544-151">(Im OData v3 hieß die Beziehung eine *Link*.</span><span class="sxs-lookup"><span data-stu-id="e3544-151">(In OData v3, the relationship was called a *link*.</span></span> <span data-ttu-id="e3544-152">Die Protokollunterschiede unerheblich nicht für dieses Lernprogramm.)</span><span class="sxs-lookup"><span data-stu-id="e3544-152">The protocol differences don't matter for this tutorial.)</span></span>

<span data-ttu-id="e3544-153">Ein Verweis verfügt über einen eigenen URI, mit dem Formular `/Entity/NavigationProperty/$ref`.</span><span class="sxs-lookup"><span data-stu-id="e3544-153">A reference has its own URI, with the form `/Entity/NavigationProperty/$ref`.</span></span> <span data-ttu-id="e3544-154">Hier ist z. B. der URI, der den Verweis zwischen eines Produkts und seiner Lieferanten zu behandeln:</span><span class="sxs-lookup"><span data-stu-id="e3544-154">For example, here is the URI to address the reference between a product and its supplier:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample13.cmd)]

<span data-ttu-id="e3544-155">Um eine Beziehung hinzufügen möchten, sendet der Client eine POST oder PUT-Anforderung an diese Adresse.</span><span class="sxs-lookup"><span data-stu-id="e3544-155">To add a relationship, the client sends a POST or PUT request to this address.</span></span>

- <span data-ttu-id="e3544-156">PUT ist die Navigationseigenschaft eine einzelne Entität, wie z. B. `Product.Supplier`.</span><span class="sxs-lookup"><span data-stu-id="e3544-156">PUT if the navigation property is a single entity, such as `Product.Supplier`.</span></span>
- <span data-ttu-id="e3544-157">BEREITSTELLEN, wenn die Navigationseigenschaft eine Auflistung, z. B. `Supplier.Products`.</span><span class="sxs-lookup"><span data-stu-id="e3544-157">POST if the navigation property is a collection, such as `Supplier.Products`.</span></span>

<span data-ttu-id="e3544-158">Der Anforderungstext enthält den URI der Entität in der Beziehung.</span><span class="sxs-lookup"><span data-stu-id="e3544-158">The body of the request contains the URI of the other entity in the relation.</span></span> <span data-ttu-id="e3544-159">Hier ist eine Beispiel für eine Anforderung:</span><span class="sxs-lookup"><span data-stu-id="e3544-159">Here is an example request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample14.cmd)]

<span data-ttu-id="e3544-160">In diesem Beispiel wird der Client sendet eine PUT-Anforderung zum `/Products(6)/Supplier/$ref`, dies ist der URI "$ref" für die `Supplier` des Produkts mit der ID = 6.</span><span class="sxs-lookup"><span data-stu-id="e3544-160">In this example, the client sends a PUT request to `/Products(6)/Supplier/$ref`, which is the $ref URI for the `Supplier` of the product with ID = 6.</span></span> <span data-ttu-id="e3544-161">Wenn die Anforderung erfolgreich ist, sendet der Server eine Antwort mit 204 (kein Inhalt):</span><span class="sxs-lookup"><span data-stu-id="e3544-161">If the request succeeds, the server sends a 204 (No Content) response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample15.cmd)]

<span data-ttu-id="e3544-162">Hier wird der Controllermethode, um eine Beziehung zum Hinzufügen einer `Product`:</span><span class="sxs-lookup"><span data-stu-id="e3544-162">Here is the controller method to add a relationship to a `Product`:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample16.cs)]

<span data-ttu-id="e3544-163">Die *NavigationProperty* Parameter gibt an, welche Beziehung festzulegen.</span><span class="sxs-lookup"><span data-stu-id="e3544-163">The *navigationProperty* parameter specifies which relationship to set.</span></span> <span data-ttu-id="e3544-164">(Wenn mehr als einer Navigationseigenschaft auf die Entität vorhanden ist, können Sie weitere hinzufügen `case` Anweisungen.)</span><span class="sxs-lookup"><span data-stu-id="e3544-164">(If there is more than one navigation property on the entity, you can add more `case` statements.)</span></span>

<span data-ttu-id="e3544-165">Die *Link* Parameter enthält den URI des Lieferanten.</span><span class="sxs-lookup"><span data-stu-id="e3544-165">The *link* parameter contains the URI of the supplier.</span></span> <span data-ttu-id="e3544-166">Web-API analysiert automatisch den Hauptteil der Anforderung, um den Wert für diesen Parameter abzurufen.</span><span class="sxs-lookup"><span data-stu-id="e3544-166">Web API automatically parses the request body to get the value for this parameter.</span></span>

<span data-ttu-id="e3544-167">Um den Lieferanten zu suchen, die ID (oder Schlüssel) erforderlich ist Teil der *Link* Parameter.</span><span class="sxs-lookup"><span data-stu-id="e3544-167">To look up the supplier, we need the ID (or key), which is part of the *link* parameter.</span></span> <span data-ttu-id="e3544-168">Verwenden Sie hierzu die folgende Hilfsmethode ein:</span><span class="sxs-lookup"><span data-stu-id="e3544-168">To do this, use the following helper method:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample17.cs)]

<span data-ttu-id="e3544-169">Im Grunde mithilfe dieser Methode die OData-Bibliothek auf den URI-Pfad in Segmente unterteilt und das Segment mit dem Schlüssel des Schlüssels in den richtigen Typ zu konvertieren.</span><span class="sxs-lookup"><span data-stu-id="e3544-169">Basically, this method uses the OData library to split the URI path into segments, find the segment that contains the key, and convert the key into the correct type.</span></span>

## <a name="deleting-a-relationship-between-entities"></a><span data-ttu-id="e3544-170">Löschen einer Beziehung zwischen Entitäten</span><span class="sxs-lookup"><span data-stu-id="e3544-170">Deleting a Relationship Between Entities</span></span>

<span data-ttu-id="e3544-171">Um eine Beziehung löschen, sendet der Client eine HTTP DELETE-Anforderung an den $ref-URI:</span><span class="sxs-lookup"><span data-stu-id="e3544-171">To delete a relationship, the client sends an HTTP DELETE request to the $ref URI:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample18.cmd)]

<span data-ttu-id="e3544-172">So sieht die Controllermethode zum Löschen der Beziehung zwischen einem Produkt und einem anderen Lieferanten aus:</span><span class="sxs-lookup"><span data-stu-id="e3544-172">Here is the controller method to delete the relationship between a Product and a Supplier:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample19.cs)]

<span data-ttu-id="e3544-173">In diesem Fall `Product.Supplier` ist die &quot;1&quot; Ende eine 1: n-Beziehung, daher können Sie die Beziehung nur durch Festlegen entfernen `Product.Supplier` auf `null`.</span><span class="sxs-lookup"><span data-stu-id="e3544-173">In this case, `Product.Supplier` is the &quot;1&quot; end of a 1-to-many relation, so you can remove the relationship just by setting `Product.Supplier` to `null`.</span></span>

<span data-ttu-id="e3544-174">In der &quot;viele&quot; -Ende einer Beziehung, die der Client muss angeben, die miteinander in Beziehung Entität zu entfernen.</span><span class="sxs-lookup"><span data-stu-id="e3544-174">In the &quot;many&quot; end of a relationship, the client must specify which related entity to remove.</span></span> <span data-ttu-id="e3544-175">Hierzu sendet der Client den URI der verknüpften Entität in der Abfragezeichenfolge der Anforderung.</span><span class="sxs-lookup"><span data-stu-id="e3544-175">To do so, the client sends the URI of the related entity in the query string of the request.</span></span> <span data-ttu-id="e3544-176">Beispielsweise so entfernen Sie "Product 1" von "Lieferant 1":</span><span class="sxs-lookup"><span data-stu-id="e3544-176">For example, to remove "Product 1" from "Supplier 1":</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample20.cmd?highlight=1)]

<span data-ttu-id="e3544-177">Um dies in Web-API unterstützen, müssen wir einen zusätzlichen Parameter in umfassen die `DeleteRef` Methode.</span><span class="sxs-lookup"><span data-stu-id="e3544-177">To support this in Web API, we need to include an extra parameter in the `DeleteRef` method.</span></span> <span data-ttu-id="e3544-178">Hier ist die Controllermethode zum Entfernen eines Produkts aus der `Supplier.Products` Beziehung.</span><span class="sxs-lookup"><span data-stu-id="e3544-178">Here is the controller method to remove a product from the `Supplier.Products` relation.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample21.cs)]

<span data-ttu-id="e3544-179">Die *Schlüssel* Parameter ist der Schlüssel für den Lieferanten und die *RelatedKey* Parameter ist der Schlüssel für das Produkt, das Aufheben der `Products` Beziehung.</span><span class="sxs-lookup"><span data-stu-id="e3544-179">The *key* parameter is the key for the supplier, and the *relatedKey* parameter is the key for the product to remove from the `Products` relationship.</span></span> <span data-ttu-id="e3544-180">Beachten Sie, dass Web-API ruft automatisch den Schlüssel aus der Abfragezeichenfolge ab.</span><span class="sxs-lookup"><span data-stu-id="e3544-180">Note that Web API automatically gets the key from the query string.</span></span>
