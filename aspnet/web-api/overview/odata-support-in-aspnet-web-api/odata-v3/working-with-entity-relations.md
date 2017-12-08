---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: "Unterstützen von Entitätsbeziehungen in OData v3 mit Web-API 2 | Microsoft Docs"
author: MikeWasson
description: "Die meisten Datasets definieren die Beziehungen zwischen Entitäten: Kunden Bestellungen; haben. Bücher weisen Autoren; Produkte wurden Lieferanten. Verwendung von OData können Clients über navigieren..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: dec7e10e59cc2441c967afe062df227b105106a1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a><span data-ttu-id="88c26-104">Unterstützen von Entitätsbeziehungen in OData v3 mit Web-API 2</span><span class="sxs-lookup"><span data-stu-id="88c26-104">Supporting Entity Relations in OData v3 with Web API 2</span></span>
====================
<span data-ttu-id="88c26-105">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="88c26-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="88c26-106">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="88c26-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="88c26-107">Die meisten Datasets definieren die Beziehungen zwischen Entitäten: Kunden Bestellungen; haben. Bücher weisen Autoren; Produkte wurden Lieferanten.</span><span class="sxs-lookup"><span data-stu-id="88c26-107">Most data sets define relations between entities: Customers have orders; books have authors; products have suppliers.</span></span> <span data-ttu-id="88c26-108">Verwendung von OData können Clients über entitätsbeziehungen navigieren.</span><span class="sxs-lookup"><span data-stu-id="88c26-108">Using OData, clients can navigate over entity relations.</span></span> <span data-ttu-id="88c26-109">Wenn Sie ein Produkt, können Sie den Lieferanten suchen.</span><span class="sxs-lookup"><span data-stu-id="88c26-109">Given a product, you can find the supplier.</span></span> <span data-ttu-id="88c26-110">Sie können auch erstellen oder Beziehungen entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="88c26-110">You can also create or remove relationships.</span></span> <span data-ttu-id="88c26-111">Beispielsweise können Sie den Lieferanten für ein Produkt festlegen.</span><span class="sxs-lookup"><span data-stu-id="88c26-111">For example, you can set the supplier for a product.</span></span>
> 
> <span data-ttu-id="88c26-112">In diesem Lernprogramm wird gezeigt, wie diese Vorgänge in ASP.NET Web-API unterstützt.</span><span class="sxs-lookup"><span data-stu-id="88c26-112">This tutorial shows how to support these operations in ASP.NET Web API.</span></span> <span data-ttu-id="88c26-113">Das Lernprogramm baut auf das Lernprogramm [erstellen einen OData-v3-Endpunkt mit Web-API 2](creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="88c26-113">The tutorial builds on the tutorial [Creating an OData v3 Endpoint with Web API 2](creating-an-odata-endpoint.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="88c26-114">In diesem Lernprogramm verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="88c26-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="88c26-115">Web-API 2</span><span class="sxs-lookup"><span data-stu-id="88c26-115">Web API 2</span></span>
> - <span data-ttu-id="88c26-116">OData-Version 3</span><span class="sxs-lookup"><span data-stu-id="88c26-116">OData Version 3</span></span>
> - <span data-ttu-id="88c26-117">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="88c26-117">Entity Framework 6</span></span>


## <a name="add-a-supplier-entity"></a><span data-ttu-id="88c26-118">Hinzufügen einer Entität "Supplier"</span><span class="sxs-lookup"><span data-stu-id="88c26-118">Add a Supplier Entity</span></span>

<span data-ttu-id="88c26-119">Zunächst müssen wir unsere OData-feed ein neuen Entitätstyps hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="88c26-119">First we need to add a new entity type to our OData feed.</span></span> <span data-ttu-id="88c26-120">Fügen wir eine `Supplier` Klasse.</span><span class="sxs-lookup"><span data-stu-id="88c26-120">We'll add a `Supplier` class.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

<span data-ttu-id="88c26-121">Diese Klasse verwendet eine Zeichenfolge für den Entitätsschlüssel.</span><span class="sxs-lookup"><span data-stu-id="88c26-121">This class uses a string for the entity key.</span></span> <span data-ttu-id="88c26-122">In der Praxis, die möglicherweise weniger gebräuchlich als mit einem Integer-Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="88c26-122">In practice, that might be less common than using an integer key.</span></span> <span data-ttu-id="88c26-123">Aber es ist Folgendes zu sehen, wie OData andere Schlüsseltypen als ganze Zahlen behandelt.</span><span class="sxs-lookup"><span data-stu-id="88c26-123">But it's worth seeing how OData handles other key types besides integers.</span></span>

<span data-ttu-id="88c26-124">Als Nächstes erstellen wir eine Beziehung durch Hinzufügen einer `Supplier` Eigenschaft, um die `Product` Klasse:</span><span class="sxs-lookup"><span data-stu-id="88c26-124">Next, we'll create a relation by adding a `Supplier` property to the `Product` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

<span data-ttu-id="88c26-125">Fügen Sie einen neuen **DbSet** auf die `ProductServiceContext` Klasse, sodass Entity Framework umfasst die `Supplier` Tabelle in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="88c26-125">Add a new **DbSet** to the `ProductServiceContext` class, so that Entity Framework will include the `Supplier` table in the database.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

<span data-ttu-id="88c26-126">Fügen Sie in WebApiConfig.cs eine Entität "Suppliers" zum EDM-Modell hinzu:</span><span class="sxs-lookup"><span data-stu-id="88c26-126">In WebApiConfig.cs, add a "Suppliers" entity to the EDM model:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a><span data-ttu-id="88c26-127">Navigationseigenschaften</span><span class="sxs-lookup"><span data-stu-id="88c26-127">Navigation Properties</span></span>

<span data-ttu-id="88c26-128">Um den Lieferanten für ein Produkt zu erhalten, sendet der Client eine GET-Anforderung:</span><span class="sxs-lookup"><span data-stu-id="88c26-128">To get the supplier for a product, the client sends a GET request:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

<span data-ttu-id="88c26-129">"Supplier" wird hier eine Navigationseigenschaft, auf die `Product` Typ.</span><span class="sxs-lookup"><span data-stu-id="88c26-129">Here "Supplier" is a navigation property on the `Product` type.</span></span> <span data-ttu-id="88c26-130">In diesem Fall `Supplier` bezieht sich auf ein einzelnes Element, aber eine Navigation kann Eigenschaft auch eine Auflistung (1: n- oder m: n-Beziehung) zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="88c26-130">In this case, `Supplier` refers to a single item, but a navigation property can also return a collection (one-to-many or many-to-many relation).</span></span>

<span data-ttu-id="88c26-131">Um diese Anforderung zu unterstützen, fügen Sie die folgende Methode, die die `ProductsController` Klasse:</span><span class="sxs-lookup"><span data-stu-id="88c26-131">To support this request, add the following method to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

<span data-ttu-id="88c26-132">Die *Schlüssel* Parameter ist der Schlüssel des Produkts.</span><span class="sxs-lookup"><span data-stu-id="88c26-132">The *key* parameter is the key of the product.</span></span> <span data-ttu-id="88c26-133">Die Methode gibt die verknüpfte Entität &#8212;in diesem Fall eine `Supplier` Instanz.</span><span class="sxs-lookup"><span data-stu-id="88c26-133">The method returns the related entity&#8212in this case, a `Supplier` instance.</span></span> <span data-ttu-id="88c26-134">Der Methodenname und Parameternamen sind wichtig.</span><span class="sxs-lookup"><span data-stu-id="88c26-134">The method name and parameter name are both important.</span></span> <span data-ttu-id="88c26-135">Wenn die Navigationseigenschaft "X" benannt wird, müssen Sie im Allgemeinen eine Methode mit dem Namen "GetX" hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="88c26-135">In general, if the navigation property is named "X", you need to add a method named "GetX".</span></span> <span data-ttu-id="88c26-136">Die Methode muss einen Parameter namens annehmen "*Schlüssel*", die den Datentyp der Schlüssel des übergeordneten Elements entspricht.</span><span class="sxs-lookup"><span data-stu-id="88c26-136">The method must take a parameter named "*key*" that matches the data type of the parent's key.</span></span>

<span data-ttu-id="88c26-137">Es ist auch wichtig, enthalten die **[FromOdataUri]** Attribut in der *Schlüssel* Parameter.</span><span class="sxs-lookup"><span data-stu-id="88c26-137">It is also important to include the **[FromOdataUri]** attribute in the *key* parameter.</span></span> <span data-ttu-id="88c26-138">Dieses Attribut weist Web API OData-Syntaxregeln verwenden, wenn es sich um den Schlüssel vom Anforderungs-URI analysiert werden.</span><span class="sxs-lookup"><span data-stu-id="88c26-138">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

## <a name="creating-and-deleting-links"></a><span data-ttu-id="88c26-139">Erstellen und Löschen von Verknüpfungen</span><span class="sxs-lookup"><span data-stu-id="88c26-139">Creating and Deleting Links</span></span>

<span data-ttu-id="88c26-140">OData unterstützt beim Erstellen oder Entfernen von Beziehungen zwischen zwei Entitäten.</span><span class="sxs-lookup"><span data-stu-id="88c26-140">OData supports creating or removing relationships between two entities.</span></span> <span data-ttu-id="88c26-141">In der Terminologie von OData-ist die Beziehung "Link".</span><span class="sxs-lookup"><span data-stu-id="88c26-141">In OData terminology, the relationship is a "link."</span></span> <span data-ttu-id="88c26-142">Jeder Link verfügt über einen URI mit dem Formular *Entität*/$links /*Entität*.</span><span class="sxs-lookup"><span data-stu-id="88c26-142">Each link has a URI with the form *entity*/$links/*entity*.</span></span> <span data-ttu-id="88c26-143">Beispielsweise sieht die Verknüpfung Produkt mit Lieferanten wie folgt:</span><span class="sxs-lookup"><span data-stu-id="88c26-143">For example, the link from product to supplier looks like this:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

<span data-ttu-id="88c26-144">Um einen neuen Link erstellen, sendet der Client eine POST-Anforderung an die Link-URI an.</span><span class="sxs-lookup"><span data-stu-id="88c26-144">To create a new link, the client sends a POST request to the link URI.</span></span> <span data-ttu-id="88c26-145">Der Text der Anforderung ist der URI der Zielentität.</span><span class="sxs-lookup"><span data-stu-id="88c26-145">The body of the request is the URI of the target entity.</span></span> <span data-ttu-id="88c26-146">Nehmen Sie z. B. an, dass ein Lieferant mit dem Schlüssel "CTSO" vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="88c26-146">For example, suppose there is a supplier with the key "CTSO".</span></span> <span data-ttu-id="88c26-147">Um eine Verknüpfung zwischen "Product(1)" und "Supplier('CTSO')" zu erstellen, sendet der Client eine Anforderung wie folgt:</span><span class="sxs-lookup"><span data-stu-id="88c26-147">To create a link from "Product(1)" to "Supplier('CTSO')", the client sends a request like the following:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

<span data-ttu-id="88c26-148">Um einen Link zu löschen, sendet der Client eine DELETE-Anforderung an die Link-URI an.</span><span class="sxs-lookup"><span data-stu-id="88c26-148">To delete a link, the client sends a DELETE request to the link URI.</span></span>

<span data-ttu-id="88c26-149">**Zum Erstellen von Verknüpfungen**</span><span class="sxs-lookup"><span data-stu-id="88c26-149">**Creating Links**</span></span>

<span data-ttu-id="88c26-150">Um einen Client zum Erstellen von Produkt-Lieferanten Links zu aktivieren, fügen Sie den folgenden Code, der `ProductsController` Klasse:</span><span class="sxs-lookup"><span data-stu-id="88c26-150">To enable a client to create product-supplier links, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

<span data-ttu-id="88c26-151">Diese Methode nimmt drei Parameter:</span><span class="sxs-lookup"><span data-stu-id="88c26-151">This method takes three parameters:</span></span>

- <span data-ttu-id="88c26-152">*Schlüssel*: der Schlüssel für die übergeordnete Entität (Product)</span><span class="sxs-lookup"><span data-stu-id="88c26-152">*key*: The key to the parent entity (the product)</span></span>
- <span data-ttu-id="88c26-153">*NavigationProperty*: der Name der Navigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="88c26-153">*navigationProperty*: The name of the navigation property.</span></span> <span data-ttu-id="88c26-154">In diesem Beispiel ist die einzige gültige Navigationseigenschaft "Supplier".</span><span class="sxs-lookup"><span data-stu-id="88c26-154">In this example, the only valid navigation property is "Supplier".</span></span>
- <span data-ttu-id="88c26-155">*Link*: der OData-URI der verknüpften Entität.</span><span class="sxs-lookup"><span data-stu-id="88c26-155">*link*: The OData URI of the related entity.</span></span> <span data-ttu-id="88c26-156">Dieser Wert stammt aus dem Anforderungstext.</span><span class="sxs-lookup"><span data-stu-id="88c26-156">This value is taken from the request body.</span></span> <span data-ttu-id="88c26-157">Beispielsweise den Link URI möglicherweise "`http://localhost/odata/Suppliers('CTSO')`, d. h. den Lieferanten mit der ID ="CTSO".</span><span class="sxs-lookup"><span data-stu-id="88c26-157">For example, the link URI might be "`http://localhost/odata/Suppliers('CTSO')`, meaning the supplier with ID = ‘CTSO'.</span></span>

<span data-ttu-id="88c26-158">Die Methode verwendet der Link, um den Lieferanten zu suchen.</span><span class="sxs-lookup"><span data-stu-id="88c26-158">The method uses the link to look up the supplier.</span></span> <span data-ttu-id="88c26-159">Wenn übereinstimmende Lieferanten gefunden wird, legt die Methode die `Product.Supplier` Eigenschaft und speichert das Ergebnis in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="88c26-159">If the matching supplier is found, the method sets the `Product.Supplier` property and saves the result to the database.</span></span>

<span data-ttu-id="88c26-160">Das schwierigste Teil ist der Link-URI analysieren.</span><span class="sxs-lookup"><span data-stu-id="88c26-160">The hardest part is parsing the link URI.</span></span> <span data-ttu-id="88c26-161">Im Wesentlichen müssen Sie das Ergebnis eine GET-Anforderung an diesen URI senden zu simulieren.</span><span class="sxs-lookup"><span data-stu-id="88c26-161">Basically, you need to simulate the result of sending a GET request to that URI.</span></span> <span data-ttu-id="88c26-162">Die folgende Hilfsmethode veranschaulicht dies.</span><span class="sxs-lookup"><span data-stu-id="88c26-162">The following helper method shows how to do this.</span></span> <span data-ttu-id="88c26-163">Die Methode ruft die Web-API-routing-Prozess und ruft wieder ein **OData-Pfad** Instanz, die die analysierten OData-Pfad darstellt.</span><span class="sxs-lookup"><span data-stu-id="88c26-163">The method invokes the Web API routing process and gets back an **ODataPath** instance that represents the parsed OData path.</span></span> <span data-ttu-id="88c26-164">Für eine Link-URI sollte der Segmente der Entitätsschlüssel.</span><span class="sxs-lookup"><span data-stu-id="88c26-164">For a link URI, one of the segments should be the entity key.</span></span> <span data-ttu-id="88c26-165">(Wenn nicht, den Client gesendet hat einen ungültigen URI.)</span><span class="sxs-lookup"><span data-stu-id="88c26-165">(If not, the client sent a bad URI.)</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

<span data-ttu-id="88c26-166">**Löschen von Links**</span><span class="sxs-lookup"><span data-stu-id="88c26-166">**Deleting Links**</span></span>

<span data-ttu-id="88c26-167">Um einen Link zu löschen, fügen Sie den folgenden Code aus, um die `ProductsController` Klasse:</span><span class="sxs-lookup"><span data-stu-id="88c26-167">To delete a link, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

<span data-ttu-id="88c26-168">In diesem Beispiel ist die Navigationseigenschaft eine einzelne `Supplier` Entität.</span><span class="sxs-lookup"><span data-stu-id="88c26-168">In this example, the navigation property is a single `Supplier` entity.</span></span> <span data-ttu-id="88c26-169">Wenn die Navigationseigenschaft eine Auflistung ist, muss der URI, der die Verknüpfung zu löschen einen Schlüssel für die verknüpfte Entität enthalten.</span><span class="sxs-lookup"><span data-stu-id="88c26-169">If the navigation property is a collection, the URI to delete a link must include a key for the related entity.</span></span> <span data-ttu-id="88c26-170">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="88c26-170">For example:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

<span data-ttu-id="88c26-171">Diese Anforderung wird von Kunde 1 Reihenfolge 1 entfernt.</span><span class="sxs-lookup"><span data-stu-id="88c26-171">This request removes order 1 from customer 1.</span></span> <span data-ttu-id="88c26-172">In diesem Fall wird die DeleteLink-Methode, die folgende Signatur aufweisen:</span><span class="sxs-lookup"><span data-stu-id="88c26-172">In this case, the DeleteLink method will have the following signature:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

<span data-ttu-id="88c26-173">Die *RelatedKey* Parameter enthält den Schlüssel für die verknüpfte Entität.</span><span class="sxs-lookup"><span data-stu-id="88c26-173">The *relatedKey* parameter gives the key for the related entity.</span></span> <span data-ttu-id="88c26-174">In diesem Fall Ihre `DeleteLink` -Methode, die primäre Entität durch Suchen der *Schlüssel* Parameter, suchen Sie die verknüpfte Entität von der *RelatedKey* Parameter, und entfernen Sie dann die Zuordnung.</span><span class="sxs-lookup"><span data-stu-id="88c26-174">So in your `DeleteLink` method, look up the primary entity by the *key* parameter, find the related entity by the *relatedKey* parameter, and then remove the association.</span></span> <span data-ttu-id="88c26-175">Je nach Ihr Datenmodell müssen beide Versionen des implementieren `DeleteLink`.</span><span class="sxs-lookup"><span data-stu-id="88c26-175">Depending on your data model, you might need to implement both versions of `DeleteLink`.</span></span> <span data-ttu-id="88c26-176">Web-API wird die richtige Version basierend auf den Anforderungs-URI aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="88c26-176">Web API will call the correct version based on the request URI.</span></span>
