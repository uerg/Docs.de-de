---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: Unterstützung für Entitätsbeziehungen in OData v3 mit Web-API 2 | Microsoft-Dokumentation
author: MikeWasson
description: 'Die meisten Datasets definieren die Beziehungen zwischen Entitäten: Kunden haben Aufträge; Bücher weisen Autoren; Produkte haben Lieferanten. Mithilfe von OData, können Clients über navigieren...'
ms.author: riande
ms.date: 02/26/2014
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: fc1c6b938c4e4be379edf1a495ca47f5f5f2eb4f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837086"
---
<a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a><span data-ttu-id="40f64-104">Unterstützung für Entitätsbeziehungen in OData v3 mit Web-API 2</span><span class="sxs-lookup"><span data-stu-id="40f64-104">Supporting Entity Relations in OData v3 with Web API 2</span></span>
====================
<span data-ttu-id="40f64-105">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="40f64-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="40f64-106">Abgeschlossenes Projekt herunterladen</span><span class="sxs-lookup"><span data-stu-id="40f64-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="40f64-107">Die meisten Datasets definieren die Beziehungen zwischen Entitäten: Kunden haben Aufträge; Bücher weisen Autoren; Produkte haben Lieferanten.</span><span class="sxs-lookup"><span data-stu-id="40f64-107">Most data sets define relations between entities: Customers have orders; books have authors; products have suppliers.</span></span> <span data-ttu-id="40f64-108">Mithilfe von OData, können Clients auf entitätsbeziehungen navigieren.</span><span class="sxs-lookup"><span data-stu-id="40f64-108">Using OData, clients can navigate over entity relations.</span></span> <span data-ttu-id="40f64-109">Wenn ein Produkt, finden Sie den Lieferanten.</span><span class="sxs-lookup"><span data-stu-id="40f64-109">Given a product, you can find the supplier.</span></span> <span data-ttu-id="40f64-110">Sie können auch erstellt oder Beziehungen entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="40f64-110">You can also create or remove relationships.</span></span> <span data-ttu-id="40f64-111">Beispielsweise können Sie den Lieferanten für ein Produkt festlegen.</span><span class="sxs-lookup"><span data-stu-id="40f64-111">For example, you can set the supplier for a product.</span></span>
> 
> <span data-ttu-id="40f64-112">In diesem Tutorial wird gezeigt, wie diese Vorgänge in ASP.NET Web-API unterstützt.</span><span class="sxs-lookup"><span data-stu-id="40f64-112">This tutorial shows how to support these operations in ASP.NET Web API.</span></span> <span data-ttu-id="40f64-113">Das Tutorial baut auf dem Tutorial [erstellen eine OData v3-Endpunkts mit Web-API 2](creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="40f64-113">The tutorial builds on the tutorial [Creating an OData v3 Endpoint with Web API 2](creating-an-odata-endpoint.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="40f64-114">Softwareversionen, die in diesem Tutorial verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="40f64-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="40f64-115">Web-API 2</span><span class="sxs-lookup"><span data-stu-id="40f64-115">Web API 2</span></span>
> - <span data-ttu-id="40f64-116">OData v3</span><span class="sxs-lookup"><span data-stu-id="40f64-116">OData Version 3</span></span>
> - <span data-ttu-id="40f64-117">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="40f64-117">Entity Framework 6</span></span>


## <a name="add-a-supplier-entity"></a><span data-ttu-id="40f64-118">Fügen Sie eine Entität "Supplier" hinzu.</span><span class="sxs-lookup"><span data-stu-id="40f64-118">Add a Supplier Entity</span></span>

<span data-ttu-id="40f64-119">Zuerst müssen wir unsere OData-feed ein neuer Entitätstyp hinzu.</span><span class="sxs-lookup"><span data-stu-id="40f64-119">First we need to add a new entity type to our OData feed.</span></span> <span data-ttu-id="40f64-120">Wir fügen eine `Supplier` Klasse.</span><span class="sxs-lookup"><span data-stu-id="40f64-120">We'll add a `Supplier` class.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

<span data-ttu-id="40f64-121">Diese Klasse verwendet eine Zeichenfolge für den Entitätsschlüssel.</span><span class="sxs-lookup"><span data-stu-id="40f64-121">This class uses a string for the entity key.</span></span> <span data-ttu-id="40f64-122">In der Praxis kann dies seltener auf als die Verwendung eines Schlüssels für die ganze Zahl sein.</span><span class="sxs-lookup"><span data-stu-id="40f64-122">In practice, that might be less common than using an integer key.</span></span> <span data-ttu-id="40f64-123">Aber es lohnt sich sehen, wie OData andere Schlüsseltypen als ganze Zahlen verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="40f64-123">But it's worth seeing how OData handles other key types besides integers.</span></span>

<span data-ttu-id="40f64-124">Als Nächstes erstellen wir eine Beziehung durch Hinzufügen einer `Supplier` Eigenschaft, um die `Product` Klasse:</span><span class="sxs-lookup"><span data-stu-id="40f64-124">Next, we'll create a relation by adding a `Supplier` property to the `Product` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

<span data-ttu-id="40f64-125">Fügen Sie einen neuen **"DbSet"** auf die `ProductServiceContext` Klasse, sodass Entity Framework enthält die `Supplier` Tabelle in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="40f64-125">Add a new **DbSet** to the `ProductServiceContext` class, so that Entity Framework will include the `Supplier` table in the database.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

<span data-ttu-id="40f64-126">Fügen Sie eine Entität "Suppliers" in WebApiConfig.cs auf das EDM-Modell:</span><span class="sxs-lookup"><span data-stu-id="40f64-126">In WebApiConfig.cs, add a "Suppliers" entity to the EDM model:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a><span data-ttu-id="40f64-127">Navigationseigenschaften</span><span class="sxs-lookup"><span data-stu-id="40f64-127">Navigation Properties</span></span>

<span data-ttu-id="40f64-128">Um den Lieferanten für ein Produkt zu erhalten, sendet der Client eine GET-Anforderung:</span><span class="sxs-lookup"><span data-stu-id="40f64-128">To get the supplier for a product, the client sends a GET request:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

<span data-ttu-id="40f64-129">Hier ist "Supplier" eine Navigationseigenschaft auf der `Product` Typ.</span><span class="sxs-lookup"><span data-stu-id="40f64-129">Here "Supplier" is a navigation property on the `Product` type.</span></span> <span data-ttu-id="40f64-130">In diesem Fall `Supplier` bezieht sich auf ein einzelnes Element, aber eine Navigation kann Eigenschaft auch eine Auflistung (1: n- oder m: n Beziehung) zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="40f64-130">In this case, `Supplier` refers to a single item, but a navigation property can also return a collection (one-to-many or many-to-many relation).</span></span>

<span data-ttu-id="40f64-131">Um diese Anforderung zu unterstützen, fügen Sie die folgende Methode der `ProductsController` Klasse:</span><span class="sxs-lookup"><span data-stu-id="40f64-131">To support this request, add the following method to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

<span data-ttu-id="40f64-132">Die *Schlüssel* Parameter ist der Schlüssel des Produkts.</span><span class="sxs-lookup"><span data-stu-id="40f64-132">The *key* parameter is the key of the product.</span></span> <span data-ttu-id="40f64-133">Die Methode gibt zurück, die verknüpfte Entität & #8212 in diesem Fall eine `Supplier` Instanz.</span><span class="sxs-lookup"><span data-stu-id="40f64-133">The method returns the related entity&#8212in this case, a `Supplier` instance.</span></span> <span data-ttu-id="40f64-134">Der Methodenname und Parameternamen sind wichtig.</span><span class="sxs-lookup"><span data-stu-id="40f64-134">The method name and parameter name are both important.</span></span> <span data-ttu-id="40f64-135">Wenn die Navigationseigenschaft "X" benannt wird, müssen Sie in der Regel zum Hinzufügen einer Methode, die mit dem Namen "GetX".</span><span class="sxs-lookup"><span data-stu-id="40f64-135">In general, if the navigation property is named "X", you need to add a method named "GetX".</span></span> <span data-ttu-id="40f64-136">Die Methode muss einen Parameter namens annehmen "*Schlüssel*", die den Datentyp des Schlüssels des übergeordneten Elements entspricht.</span><span class="sxs-lookup"><span data-stu-id="40f64-136">The method must take a parameter named "*key*" that matches the data type of the parent's key.</span></span>

<span data-ttu-id="40f64-137">Es ist auch wichtig, Sie enthalten die **[FromOdataUri]** -Attribut in der *Schlüssel* Parameter.</span><span class="sxs-lookup"><span data-stu-id="40f64-137">It is also important to include the **[FromOdataUri]** attribute in the *key* parameter.</span></span> <span data-ttu-id="40f64-138">Dieses Attribut teilt die Web-API-OData-Syntaxregeln beim Analysieren des Schlüssels aus der Anforderungs-URI zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="40f64-138">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

## <a name="creating-and-deleting-links"></a><span data-ttu-id="40f64-139">Erstellen und Löschen von Links</span><span class="sxs-lookup"><span data-stu-id="40f64-139">Creating and Deleting Links</span></span>

<span data-ttu-id="40f64-140">OData unterstützt erstellen oder Entfernen von Beziehungen zwischen zwei Entitäten.</span><span class="sxs-lookup"><span data-stu-id="40f64-140">OData supports creating or removing relationships between two entities.</span></span> <span data-ttu-id="40f64-141">In der OData-Terminologie ist die Beziehung "Link".</span><span class="sxs-lookup"><span data-stu-id="40f64-141">In OData terminology, the relationship is a "link."</span></span> <span data-ttu-id="40f64-142">Jeder Link verfügt über einen URI mit dem Formular *Entität*/$links /*Entität*.</span><span class="sxs-lookup"><span data-stu-id="40f64-142">Each link has a URI with the form *entity*/$links/*entity*.</span></span> <span data-ttu-id="40f64-143">Beispielsweise sieht der Verbindung vom Produkt zum Lieferanten folgendermaßen aus:</span><span class="sxs-lookup"><span data-stu-id="40f64-143">For example, the link from product to supplier looks like this:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

<span data-ttu-id="40f64-144">Um einen neuen Link erstellen zu können, sendet der Client eine POST-Anforderung auf den Link-URI.</span><span class="sxs-lookup"><span data-stu-id="40f64-144">To create a new link, the client sends a POST request to the link URI.</span></span> <span data-ttu-id="40f64-145">Der Hauptteil der Anforderung ist der URI der Zielentität.</span><span class="sxs-lookup"><span data-stu-id="40f64-145">The body of the request is the URI of the target entity.</span></span> <span data-ttu-id="40f64-146">Nehmen wir beispielsweise an, dass ein Anbieter mit dem Schlüssel "CTSO" vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="40f64-146">For example, suppose there is a supplier with the key "CTSO".</span></span> <span data-ttu-id="40f64-147">Um eine Verknüpfung zwischen "Product(1)" und "Supplier('CTSO')" zu erstellen, sendet der Client eine Anforderung wie folgt:</span><span class="sxs-lookup"><span data-stu-id="40f64-147">To create a link from "Product(1)" to "Supplier('CTSO')", the client sends a request like the following:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

<span data-ttu-id="40f64-148">Um einen Link zu löschen, sendet der Client eine DELETE-Anforderung auf den Link-URI.</span><span class="sxs-lookup"><span data-stu-id="40f64-148">To delete a link, the client sends a DELETE request to the link URI.</span></span>

<span data-ttu-id="40f64-149">**Erstellen von Verknüpfungen**</span><span class="sxs-lookup"><span data-stu-id="40f64-149">**Creating Links**</span></span>

<span data-ttu-id="40f64-150">Um einen Client zum Erstellen von Produktlieferant Links zu aktivieren, fügen Sie den folgenden Code der `ProductsController` Klasse:</span><span class="sxs-lookup"><span data-stu-id="40f64-150">To enable a client to create product-supplier links, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

<span data-ttu-id="40f64-151">Diese Methode akzeptiert drei Parameter:</span><span class="sxs-lookup"><span data-stu-id="40f64-151">This method takes three parameters:</span></span>

- <span data-ttu-id="40f64-152">*Schlüssel*: der Schlüssel für die übergeordnete Entität (das Produkt)</span><span class="sxs-lookup"><span data-stu-id="40f64-152">*key*: The key to the parent entity (the product)</span></span>
- <span data-ttu-id="40f64-153">*NavigationProperty*: der Name der Navigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="40f64-153">*navigationProperty*: The name of the navigation property.</span></span> <span data-ttu-id="40f64-154">In diesem Beispiel ist die einzige gültige Navigationseigenschaft "Supplier".</span><span class="sxs-lookup"><span data-stu-id="40f64-154">In this example, the only valid navigation property is "Supplier".</span></span>
- <span data-ttu-id="40f64-155">*Link*: der OData-URI der verknüpften Entität.</span><span class="sxs-lookup"><span data-stu-id="40f64-155">*link*: The OData URI of the related entity.</span></span> <span data-ttu-id="40f64-156">Dieser Wert stammt aus dem Anforderungstext.</span><span class="sxs-lookup"><span data-stu-id="40f64-156">This value is taken from the request body.</span></span> <span data-ttu-id="40f64-157">Beispielsweise den Link URI möglicherweise "`http://localhost/odata/Suppliers('CTSO')`, d. h. den Lieferanten mit ID ="CTSO".</span><span class="sxs-lookup"><span data-stu-id="40f64-157">For example, the link URI might be "`http://localhost/odata/Suppliers('CTSO')`, meaning the supplier with ID = ‘CTSO'.</span></span>

<span data-ttu-id="40f64-158">Die Methode verwendet den Link, um den Lieferanten zu suchen.</span><span class="sxs-lookup"><span data-stu-id="40f64-158">The method uses the link to look up the supplier.</span></span> <span data-ttu-id="40f64-159">Wenn der entsprechende Lieferant gefunden wird, wird die-Methode legt die `Product.Supplier` Eigenschaft und speichert das Ergebnis in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="40f64-159">If the matching supplier is found, the method sets the `Product.Supplier` property and saves the result to the database.</span></span>

<span data-ttu-id="40f64-160">Der schwierigste Teil ist den Link-URI wird analysiert.</span><span class="sxs-lookup"><span data-stu-id="40f64-160">The hardest part is parsing the link URI.</span></span> <span data-ttu-id="40f64-161">Im Grunde müssen Sie das Ergebnis des Sendens einer GET-Anforderung an diesen URI zu simulieren.</span><span class="sxs-lookup"><span data-stu-id="40f64-161">Basically, you need to simulate the result of sending a GET request to that URI.</span></span> <span data-ttu-id="40f64-162">Die folgende Hilfsmethode veranschaulicht dies.</span><span class="sxs-lookup"><span data-stu-id="40f64-162">The following helper method shows how to do this.</span></span> <span data-ttu-id="40f64-163">Die Methode ruft die Web-API-routing-Prozess und zurückerhält ein **OData-Pfad** -Instanz, die analysierten OData-Pfad darstellt.</span><span class="sxs-lookup"><span data-stu-id="40f64-163">The method invokes the Web API routing process and gets back an **ODataPath** instance that represents the parsed OData path.</span></span> <span data-ttu-id="40f64-164">Nach einem Link-URI muss eines der Segmente des Entitätsschlüssels.</span><span class="sxs-lookup"><span data-stu-id="40f64-164">For a link URI, one of the segments should be the entity key.</span></span> <span data-ttu-id="40f64-165">(Wenn nicht, den Client gesendet wurde einen ungültigen URI.)</span><span class="sxs-lookup"><span data-stu-id="40f64-165">(If not, the client sent a bad URI.)</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

<span data-ttu-id="40f64-166">**Löschen von Links**</span><span class="sxs-lookup"><span data-stu-id="40f64-166">**Deleting Links**</span></span>

<span data-ttu-id="40f64-167">Um einen Link zu löschen, fügen Sie den folgenden Code der `ProductsController` Klasse:</span><span class="sxs-lookup"><span data-stu-id="40f64-167">To delete a link, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

<span data-ttu-id="40f64-168">In diesem Beispiel ist die Navigationseigenschaft eine einzelne `Supplier` Entität.</span><span class="sxs-lookup"><span data-stu-id="40f64-168">In this example, the navigation property is a single `Supplier` entity.</span></span> <span data-ttu-id="40f64-169">Wenn die Navigationseigenschaft eine Auflistung ist, muss der URI, um die Verknüpfung löschen einen Schlüssel für die verknüpfte Entität enthalten.</span><span class="sxs-lookup"><span data-stu-id="40f64-169">If the navigation property is a collection, the URI to delete a link must include a key for the related entity.</span></span> <span data-ttu-id="40f64-170">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="40f64-170">For example:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

<span data-ttu-id="40f64-171">Diese Anforderung wird die Reihenfolge 1 von Kunde 1 entfernt.</span><span class="sxs-lookup"><span data-stu-id="40f64-171">This request removes order 1 from customer 1.</span></span> <span data-ttu-id="40f64-172">In diesem Fall verfügen die DeleteLink-Methode die folgende Signatur:</span><span class="sxs-lookup"><span data-stu-id="40f64-172">In this case, the DeleteLink method will have the following signature:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

<span data-ttu-id="40f64-173">Die *RelatedKey* Parameter enthält den Schlüssel für die verknüpfte Entität.</span><span class="sxs-lookup"><span data-stu-id="40f64-173">The *relatedKey* parameter gives the key for the related entity.</span></span> <span data-ttu-id="40f64-174">Im Ihre `DeleteLink` -Methode, suchen die primäre Entität von der *Schlüssel* Parameter finden Sie die verknüpfte Entität durch die *RelatedKey* -Parameter, und entfernen Sie dann die Zuordnung.</span><span class="sxs-lookup"><span data-stu-id="40f64-174">So in your `DeleteLink` method, look up the primary entity by the *key* parameter, find the related entity by the *relatedKey* parameter, and then remove the association.</span></span> <span data-ttu-id="40f64-175">Je nach Ihrem Datenmodell müssen möglicherweise beide Versionen implementieren `DeleteLink`.</span><span class="sxs-lookup"><span data-stu-id="40f64-175">Depending on your data model, you might need to implement both versions of `DeleteLink`.</span></span> <span data-ttu-id="40f64-176">Web-API wird die richtige Version, die basierend auf der Anforderungs-URI aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="40f64-176">Web API will call the correct version based on the request URI.</span></span>
