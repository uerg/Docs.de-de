---
uid: web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
title: Verwenden $select, $expand, und $value in OData der ASP.NET Web API 2 | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 10/11/2013
ms.assetid: 43279a80-a96c-4564-b6ea-ad992a2d6828
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
msc.type: authoredcontent
ms.openlocfilehash: c5c0d374a918706e65254121ba5fa1516afdaf75
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814458"
---
<a name="using-select-expand-and-value-in-aspnet-web-api-2-odata"></a><span data-ttu-id="f154b-102">Verwenden $select, $expand, und $value in OData der ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="f154b-102">Using $select, $expand, and $value in ASP.NET Web API 2 OData</span></span>
====================
<span data-ttu-id="f154b-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f154b-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="f154b-104">Web-API 2 bietet Unterstützung für die $expand, $select und Optionen für $value in OData.</span><span class="sxs-lookup"><span data-stu-id="f154b-104">Web API 2 adds support for the $expand, $select, and $value options in OData.</span></span> <span data-ttu-id="f154b-105">Mit diesen Optionen können einen Client, um die Darstellung zu steuern, die sie wieder vom Server abruft.</span><span class="sxs-lookup"><span data-stu-id="f154b-105">These options allow a client to control the representation that it gets back from the server.</span></span>

- <span data-ttu-id="f154b-106">**der $expand-** bewirkt, dass verknüpfte Entitäten Inlineschemainformationen enthält, in der Antwort zu sein.</span><span class="sxs-lookup"><span data-stu-id="f154b-106">**$expand** causes related entities to be included inline in the response.</span></span>
- <span data-ttu-id="f154b-107">**$select** wählt eine Teilmenge der Eigenschaften in der Antwort eingeschlossen werden sollen.</span><span class="sxs-lookup"><span data-stu-id="f154b-107">**$select** selects a subset of properties to include in the response.</span></span>
- <span data-ttu-id="f154b-108">**$value** Ruft den Rohwert einer Eigenschaft ab.</span><span class="sxs-lookup"><span data-stu-id="f154b-108">**$value** gets the raw value of a property.</span></span>

## <a name="example-schema"></a><span data-ttu-id="f154b-109">Beispielschema</span><span class="sxs-lookup"><span data-stu-id="f154b-109">Example Schema</span></span>

<span data-ttu-id="f154b-110">In diesem Artikel verwende ich einen OData-Dienst, der drei Entitäten definiert: Produkt, Lieferanten und Kategorie.</span><span class="sxs-lookup"><span data-stu-id="f154b-110">For this article, I'll use an OData service that defines three entities: Product, Supplier, and Category.</span></span> <span data-ttu-id="f154b-111">Jedes Produkt verfügt über eine Kategorie und ein Lieferant.</span><span class="sxs-lookup"><span data-stu-id="f154b-111">Each product has one category and one supplier.</span></span>

![](using-select-expand-and-value/_static/image1.png)

<span data-ttu-id="f154b-112">Hier sind die C#-Klassen, die die Entitätsmodelle zu definieren:</span><span class="sxs-lookup"><span data-stu-id="f154b-112">Here are the C# classes that define the entity models:</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample1.cs)]

<span data-ttu-id="f154b-113">Beachten Sie, dass die `Product` -Klasse definiert die Navigationseigenschaften für die `Supplier` und `Category`.</span><span class="sxs-lookup"><span data-stu-id="f154b-113">Notice that the `Product` class defines navigation properties for the `Supplier` and `Category`.</span></span> <span data-ttu-id="f154b-114">Die `Category` Klasse definiert eine Navigationseigenschaft für die Produkte in jeder Kategorie.</span><span class="sxs-lookup"><span data-stu-id="f154b-114">The `Category` class defines a navigation property for the products in each category.</span></span>

<span data-ttu-id="f154b-115">Verwenden Sie das Gerüst für Visual Studio 2013, um einen OData-Endpunkt für dieses Schema zu erstellen, siehe [Erstellen eines OData-Endpunkts in ASP.NET Web-API](odata-v3/creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="f154b-115">To create an OData endpoint for this schema, use the Visual Studio 2013 scaffolding, as described in [Creating an OData Endpoint in ASP.NET Web API](odata-v3/creating-an-odata-endpoint.md).</span></span> <span data-ttu-id="f154b-116">Fügen Sie für Produkt, Kategorie und Lieferanten separate Controller hinzu.</span><span class="sxs-lookup"><span data-stu-id="f154b-116">Add separate controllers for Product, Category, and Supplier.</span></span>

## <a name="enabling-expand-and-select"></a><span data-ttu-id="f154b-117">Erweitern Sie mit der Aktivierung von $ und $select</span><span class="sxs-lookup"><span data-stu-id="f154b-117">Enabling $expand and $select</span></span>

<span data-ttu-id="f154b-118">In Visual Studio 2013 erstellt die Web-API OData-Gerüst ein Controllers, automatisch unterstützt, die $expand- und $select.</span><span class="sxs-lookup"><span data-stu-id="f154b-118">In Visual Studio 2013, the Web API OData scaffolding creates a controller that automatically supports $expand and $select.</span></span> <span data-ttu-id="f154b-119">Zu Referenzzwecken hier sind die Anforderungen zur Unterstützung von $erweitern und $select in einem Controller.</span><span class="sxs-lookup"><span data-stu-id="f154b-119">For reference, here are the requirements to support $expand and $select in a controller.</span></span>

<span data-ttu-id="f154b-120">Für Sammlungen, die Controller `Get` Methodenrückgabewert muss ein **"IQueryable"**.</span><span class="sxs-lookup"><span data-stu-id="f154b-120">For collections, the controller's `Get` method must return an **IQueryable**.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample2.cs)]

<span data-ttu-id="f154b-121">Für einzelne Entitäten Zurückgeben einer **"SingleResult"&lt;T&gt;**, wobei T ist ein **"IQueryable"** , das NULL Entitäten oder eine Entität enthält.</span><span class="sxs-lookup"><span data-stu-id="f154b-121">For single entities, return a **SingleResult&lt;T&gt;**, where T is an **IQueryable** that contains zero or one entities.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample3.cs)]

<span data-ttu-id="f154b-122">Außerdem ergänzen Ihre `Get` Methoden mit dem **[Queryable]** Attribut, wie in den vorherigen Codeausschnitten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="f154b-122">Also, decorate your `Get` methods with the **[Queryable]** attribute, as shown in the previous code snippets.</span></span> <span data-ttu-id="f154b-123">Rufen Sie alternativ **EnableQuerySupport** auf die **HttpConfiguration** Objekt beim Start.</span><span class="sxs-lookup"><span data-stu-id="f154b-123">Alternatively, call **EnableQuerySupport** on the **HttpConfiguration** object at startup.</span></span> <span data-ttu-id="f154b-124">(Weitere Informationen finden Sie unter [Aktivieren der OData-Abfrageoptionen](supporting-odata-query-options.md#enable).)</span><span class="sxs-lookup"><span data-stu-id="f154b-124">(For more information, see [Enabling OData Query Options](supporting-odata-query-options.md#enable).)</span></span>

## <a name="using-expand"></a><span data-ttu-id="f154b-125">Erweitern Sie unter Verwendung von $</span><span class="sxs-lookup"><span data-stu-id="f154b-125">Using $expand</span></span>

<span data-ttu-id="f154b-126">Beim Abfragen von einem OData-Entität oder einer Auflistung schließt die Standardantwort verknüpfte Entitäten nicht.</span><span class="sxs-lookup"><span data-stu-id="f154b-126">When you query an OData entity or collection, the default response does not include related entities.</span></span> <span data-ttu-id="f154b-127">So sieht z. B. die Standardantwort für die Entitätenmenge für die Kategorien aus:</span><span class="sxs-lookup"><span data-stu-id="f154b-127">For example, here is the default response for the Categories entity set:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample4.cmd)]

<span data-ttu-id="f154b-128">Wie Sie sehen, enthält die Antwort alle Produkte, keine, obwohl die Category-Entität einen Navigationslink für die Produkte verfügt.</span><span class="sxs-lookup"><span data-stu-id="f154b-128">As you can see, the response does not include any products, even though the Category entity has a Products navigation link.</span></span> <span data-ttu-id="f154b-129">Der Client kann jedoch $ verwenden erweitert werden, um die Liste der Produkte für die einzelnen Kategorien zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="f154b-129">However, the client can use $expand to get the list of products for each category.</span></span> <span data-ttu-id="f154b-130">Der $expand-Option wird in der Abfragezeichenfolge der Anforderung:</span><span class="sxs-lookup"><span data-stu-id="f154b-130">The $expand option goes in the query string of the request:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample5.cmd)]

<span data-ttu-id="f154b-131">Der Server wird jetzt die Produkte für die einzelnen Kategorien, die Inline in die Kategorien enthalten.</span><span class="sxs-lookup"><span data-stu-id="f154b-131">Now the server will include the products for each category, inline with the categories.</span></span> <span data-ttu-id="f154b-132">Hier ist die Nutzlast der Antwort aus:</span><span class="sxs-lookup"><span data-stu-id="f154b-132">Here is the response payload:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample6.cmd)]

<span data-ttu-id="f154b-133">Beachten Sie, dass jeder Eintrag im Array "Value" eine Liste der Produkte enthält.</span><span class="sxs-lookup"><span data-stu-id="f154b-133">Notice that each entry in the "value" array contains a Products list.</span></span>

<span data-ttu-id="f154b-134">Der $expand-Option akzeptiert eine durch Trennzeichen getrennte Liste der Navigationseigenschaften zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="f154b-134">The $expand option takes a comma-separated list of navigation properties to expand.</span></span> <span data-ttu-id="f154b-135">Die folgende Anforderung wird erweitert, sowohl die Kategorie und den Lieferanten für ein Produkt.</span><span class="sxs-lookup"><span data-stu-id="f154b-135">The following request expands both the category and the supplier for a product.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample7.cmd)]

<span data-ttu-id="f154b-136">Hier ist der Antworttext:</span><span class="sxs-lookup"><span data-stu-id="f154b-136">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample8.cmd)]

<span data-ttu-id="f154b-137">Sie können mehrere Ebenen der Navigationseigenschaft erweitern.</span><span class="sxs-lookup"><span data-stu-id="f154b-137">You can expand more than one level of navigation property.</span></span> <span data-ttu-id="f154b-138">Das folgende Beispiel schließt alle Produkte für eine Kategorie und der Lieferant für jedes Produkt.</span><span class="sxs-lookup"><span data-stu-id="f154b-138">The following example includes all the products for a category and also the supplier for each product.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample9.cmd)]

<span data-ttu-id="f154b-139">Hier ist der Antworttext:</span><span class="sxs-lookup"><span data-stu-id="f154b-139">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample10.cmd)]

<span data-ttu-id="f154b-140">Standardmäßig beschränkt die Web-API die maximale erweiterungstiefe auf 2.</span><span class="sxs-lookup"><span data-stu-id="f154b-140">By default, Web API limits the maximum expansion depth to 2.</span></span> <span data-ttu-id="f154b-141">Die wird verhindert, dass den Client sendet komplexe Anforderungen wie `$expand=Orders/OrderDetails/Product/Supplier/Region`, der möglicherweise ineffizient, Abfragen und große Antworten erstellen.</span><span class="sxs-lookup"><span data-stu-id="f154b-141">That prevents the client from sending complex requests like `$expand=Orders/OrderDetails/Product/Supplier/Region`, which might be inefficient to query and create large responses.</span></span> <span data-ttu-id="f154b-142">Um die Standardeinstellung zu überschreiben, legen die **MaxExpansionDepth** Eigenschaft für die **[Queryable]** Attribut.</span><span class="sxs-lookup"><span data-stu-id="f154b-142">To override the default, set the **MaxExpansionDepth** property on the **[Queryable]** attribute.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample11.cs)]

<span data-ttu-id="f154b-143">Weitere Informationen zur $expand-Option, finden Sie unter [Expand System Query-Option (expand, $)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) in der offiziellen OData-Dokumentation.</span><span class="sxs-lookup"><span data-stu-id="f154b-143">For more information about the $expand option, see [Expand System Query Option ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) in the official OData documentation.</span></span>

## <a name="using-select"></a><span data-ttu-id="f154b-144">$Select verwenden</span><span class="sxs-lookup"><span data-stu-id="f154b-144">Using $select</span></span>

<span data-ttu-id="f154b-145">Die $select-Option gibt eine Teilmenge der Eigenschaften in den Antworttext eingeschlossen werden sollen.</span><span class="sxs-lookup"><span data-stu-id="f154b-145">The $select option specifies a subset of properties to include in the response body.</span></span> <span data-ttu-id="f154b-146">Um nur die Namen und die Preise für jedes Produkt zu erhalten, verwenden Sie z. B. die folgende Abfrage:</span><span class="sxs-lookup"><span data-stu-id="f154b-146">For example, to get only the name and price of each product, use the following query:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample12.cmd)]

<span data-ttu-id="f154b-147">Hier ist der Antworttext:</span><span class="sxs-lookup"><span data-stu-id="f154b-147">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample13.cmd)]

<span data-ttu-id="f154b-148">Können kombiniert werden $select und $expand, in der gleichen Abfrage.</span><span class="sxs-lookup"><span data-stu-id="f154b-148">You can combine $select and $expand in the same query.</span></span> <span data-ttu-id="f154b-149">Stellen Sie sicher, dass die erweiterte Eigenschaft in der Option "$select" enthalten.</span><span class="sxs-lookup"><span data-stu-id="f154b-149">Make sure to include the expanded property in the $select option.</span></span> <span data-ttu-id="f154b-150">Die folgende Anforderung ruft z. B. ab, der Produktname und Lieferanten.</span><span class="sxs-lookup"><span data-stu-id="f154b-150">For example, the following request gets the product name and supplier.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample14.cmd)]

<span data-ttu-id="f154b-151">Hier ist der Antworttext:</span><span class="sxs-lookup"><span data-stu-id="f154b-151">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample15.cmd)]

<span data-ttu-id="f154b-152">Sie können auch die Eigenschaften in einer erweiterten Eigenschaft auswählen.</span><span class="sxs-lookup"><span data-stu-id="f154b-152">You can also select the properties within an expanded property.</span></span> <span data-ttu-id="f154b-153">Die folgende Anforderung wird erweitert, Produkte und wählt Kategorienamen und Produktname.</span><span class="sxs-lookup"><span data-stu-id="f154b-153">The following request expands Products and selects category name plus product name.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample16.cmd)]

<span data-ttu-id="f154b-154">Hier ist der Antworttext:</span><span class="sxs-lookup"><span data-stu-id="f154b-154">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample17.cmd)]

<span data-ttu-id="f154b-155">Weitere Informationen zur Option "$select" finden Sie unter [Select System Query Option ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) in der offiziellen OData-Dokumentation.</span><span class="sxs-lookup"><span data-stu-id="f154b-155">For more information about the $select option, see [Select System Query Option ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) in the official OData documentation.</span></span>

## <a name="getting-individual-properties-of-an-entity-value"></a><span data-ttu-id="f154b-156">Abrufen von einzelnen Eigenschaften einer Entität ($value)</span><span class="sxs-lookup"><span data-stu-id="f154b-156">Getting Individual Properties of an Entity ($value)</span></span>

<span data-ttu-id="f154b-157">Es gibt zwei Möglichkeiten für einen OData-Client, um eine einzelne Eigenschaft aus einer Entität zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="f154b-157">There are two ways for an OData client to get an individual property from an entity.</span></span> <span data-ttu-id="f154b-158">Der Client kann entweder rufe den Wert in OData-Format, oder rufen Sie den raw-Wert der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="f154b-158">The client can either get the value in OData format, or get the raw value of the property.</span></span>

<span data-ttu-id="f154b-159">Die folgende Anforderung Ruft eine Eigenschaft in der OData-Format.</span><span class="sxs-lookup"><span data-stu-id="f154b-159">The following request gets a property in OData format.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample18.cmd)]

<span data-ttu-id="f154b-160">Hier ist Beispiel zeigt eine Antwort im JSON-Format ein:</span><span class="sxs-lookup"><span data-stu-id="f154b-160">Here is an example response in JSON format:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample19.cmd)]

<span data-ttu-id="f154b-161">Um den unformatierten Wert der Eigenschaft zu erhalten, fügen Sie $value an den URI ein:</span><span class="sxs-lookup"><span data-stu-id="f154b-161">To get the raw value of the property, append $value to the URI:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample20.cmd)]

<span data-ttu-id="f154b-162">Hier ist die Antwort.</span><span class="sxs-lookup"><span data-stu-id="f154b-162">Here is the response.</span></span> <span data-ttu-id="f154b-163">Beachten Sie, dass der Inhaltstyp "Text/Plain", nicht JSON ist.</span><span class="sxs-lookup"><span data-stu-id="f154b-163">Notice that the content type is "text/plain", not JSON.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample21.cmd)]

<span data-ttu-id="f154b-164">Um diese Abfragen in Ihren OData-Controller zu unterstützen, fügen Sie eine Methode namens `GetProperty`, wobei `Property` ist der Name der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="f154b-164">To support these queries in your OData controller, add a method named `GetProperty`, where `Property` is the name of the property.</span></span> <span data-ttu-id="f154b-165">Beispielsweise würde die Methode zum Abrufen der Eigenschaft Name den Namen `GetName`.</span><span class="sxs-lookup"><span data-stu-id="f154b-165">For example, the method to get the Name property would be named `GetName`.</span></span> <span data-ttu-id="f154b-166">Die Methode sollte den Wert dieser Eigenschaft zurückgeben:</span><span class="sxs-lookup"><span data-stu-id="f154b-166">The method should return the value of that property:</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample22.cs)]
