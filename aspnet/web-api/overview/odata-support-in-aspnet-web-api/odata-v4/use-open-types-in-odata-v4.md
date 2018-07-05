---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: Öffnen Sie die Typen in OData v4 mit ASP.NET Web-API | Microsoft-Dokumentation
author: microsoft
description: In OData v4 ist ein offener Typ ein Stuctured-Typ, der dynamische Eigenschaften, zusätzlich zu den Eigenschaften enthält, die in der Definition eines Typs deklariert werden. Öffnen...
ms.author: aspnetcontent
ms.date: 09/15/2014
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 560d47e0dc451847311eb9e2327190eed2209546
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832267"
---
<a name="open-types-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="0ebe2-104">Öffnen Sie die Typen in OData v4 mit ASP.NET Web-API</span><span class="sxs-lookup"><span data-stu-id="0ebe2-104">Open Types in OData v4 with ASP.NET Web API</span></span>
====================
<span data-ttu-id="0ebe2-105">durch [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="0ebe2-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="0ebe2-106">In OData v4 ein *offener Typ* ist ein Stuctured, die dynamische Eigenschaften, zusätzlich zu den Eigenschaften enthält, die in der Definition eines Typs deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="0ebe2-106">In OData v4, an *open type* is a stuctured type that contains dynamic properties, in addition to any properties that are declared in the type definition.</span></span> <span data-ttu-id="0ebe2-107">Offene Typen können Sie die Flexibilität mit Ihren Datenmodellen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="0ebe2-107">Open types let you add flexibility to your data models.</span></span> <span data-ttu-id="0ebe2-108">Dieses Tutorial zeigt, wie offene Typen in OData der ASP.NET Web-API verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="0ebe2-108">This tutorial shows how to use open types in ASP.NET Web API OData.</span></span>
> 
> <span data-ttu-id="0ebe2-109">In diesem Tutorial wird davon ausgegangen, dass Sie bereits wissen, wie zum Erstellen eines OData-Endpunkts in ASP.NET Web-API.</span><span class="sxs-lookup"><span data-stu-id="0ebe2-109">This tutorial assumes that you already know how to create an OData endpoint in ASP.NET Web API.</span></span> <span data-ttu-id="0ebe2-110">Falls nicht, lesen Sie zunächst [erstellen ein OData v4-Endpunkts](create-an-odata-v4-endpoint.md) erste.</span><span class="sxs-lookup"><span data-stu-id="0ebe2-110">If not, start by reading [Create an OData v4 Endpoint](create-an-odata-v4-endpoint.md) first.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="0ebe2-111">Softwareversionen, die in diesem Tutorial verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="0ebe2-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="0ebe2-112">Web-API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="0ebe2-112">Web API OData 5.3</span></span>
> - <span data-ttu-id="0ebe2-113">OData v4</span><span class="sxs-lookup"><span data-stu-id="0ebe2-113">OData v4</span></span>


<span data-ttu-id="0ebe2-114">Zunächst einige Begriffe OData:</span><span class="sxs-lookup"><span data-stu-id="0ebe2-114">First, some OData terminology:</span></span>

- <span data-ttu-id="0ebe2-115">Entitätstyp: einen strukturierten Typ mit einem Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="0ebe2-115">Entity type: A structured type with a key.</span></span>
- <span data-ttu-id="0ebe2-116">Komplexer Typ: einen strukturierten Typ ohne einen Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="0ebe2-116">Complex type: A structured type without a key.</span></span>
- <span data-ttu-id="0ebe2-117">Öffnen Sie geben: ein Typ mit dynamischen Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="0ebe2-117">Open type: A type with dynamic properties.</span></span> <span data-ttu-id="0ebe2-118">Sowohl Entitätstypen und komplexe Typen können geöffnet sein.</span><span class="sxs-lookup"><span data-stu-id="0ebe2-118">Both entity types and complex types can be open.</span></span>

<span data-ttu-id="0ebe2-119">Der Wert einer dynamischen Eigenschaft kann es sich um einen primitiven Typ, komplexen Typ oder Enumerationstyp sein; oder eine Auflistung aller dieser Typen.</span><span class="sxs-lookup"><span data-stu-id="0ebe2-119">The value of a dynamic property can be a primitive type, complex type, or enumeration type; or a collection of any of those types.</span></span> <span data-ttu-id="0ebe2-120">Weitere Informationen zu offenen Typen, finden Sie unter den [OData v4-Spezifikation](http://www.odata.org/documentation/odata-version-4-0/).</span><span class="sxs-lookup"><span data-stu-id="0ebe2-120">For more information about open types, see the [OData v4 specification](http://www.odata.org/documentation/odata-version-4-0/).</span></span>

## <a name="install-the-web-odata-libraries"></a><span data-ttu-id="0ebe2-121">Installieren Sie die Web-OData-Bibliotheken</span><span class="sxs-lookup"><span data-stu-id="0ebe2-121">Install the Web OData Libraries</span></span>

<span data-ttu-id="0ebe2-122">Verwenden der NuGet-Paketmanager, um die neuesten Web-API OData-Bibliotheken installieren.</span><span class="sxs-lookup"><span data-stu-id="0ebe2-122">Use NuGet Package Manager to install the latest Web API OData libraries.</span></span> <span data-ttu-id="0ebe2-123">Aus dem Fenster Paket-Manager-Konsole:</span><span class="sxs-lookup"><span data-stu-id="0ebe2-123">From the Package Manager Console window:</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a><span data-ttu-id="0ebe2-124">Definieren Sie die CLR-Typen</span><span class="sxs-lookup"><span data-stu-id="0ebe2-124">Define the CLR Types</span></span>

<span data-ttu-id="0ebe2-125">Zuerst definieren die EDM-Modelle als CLR-Typen.</span><span class="sxs-lookup"><span data-stu-id="0ebe2-125">Start by defining the EDM models as CLR types.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="0ebe2-126">Wenn der Entity Data Model (EDM) erstellt wird,</span><span class="sxs-lookup"><span data-stu-id="0ebe2-126">When the Entity Data Model (EDM) is created,</span></span>

- <span data-ttu-id="0ebe2-127">`Category` ist ein Enumerationstyp.</span><span class="sxs-lookup"><span data-stu-id="0ebe2-127">`Category` is an enumeration type.</span></span>
- <span data-ttu-id="0ebe2-128">`Address` ist ein komplexer Typ.</span><span class="sxs-lookup"><span data-stu-id="0ebe2-128">`Address` is a complex type.</span></span> <span data-ttu-id="0ebe2-129">(sie einen Schlüssel muss nicht so, dass es sich nicht um ein Entitätstyp ist.)</span><span class="sxs-lookup"><span data-stu-id="0ebe2-129">(It does not have a key, so it is not an entity type.)</span></span>
- <span data-ttu-id="0ebe2-130">`Customer` ist ein Entitätstyp.</span><span class="sxs-lookup"><span data-stu-id="0ebe2-130">`Customer` is an entity type.</span></span> <span data-ttu-id="0ebe2-131">(sie hat es sich um einen Schlüssel.)</span><span class="sxs-lookup"><span data-stu-id="0ebe2-131">(It has a key.)</span></span>
- <span data-ttu-id="0ebe2-132">`Press` ist ein offener komplexen Typ.</span><span class="sxs-lookup"><span data-stu-id="0ebe2-132">`Press` is an open complex type.</span></span>
- <span data-ttu-id="0ebe2-133">`Book` ist ein offener Entitätstyp an.</span><span class="sxs-lookup"><span data-stu-id="0ebe2-133">`Book` is an open entity type.</span></span>

<span data-ttu-id="0ebe2-134">Um einen offenen Typ zu erstellen, müssen die CLR-Typ eine Eigenschaft des Typs `IDictionary<string, object>`, das die dynamischen Eigenschaften enthält.</span><span class="sxs-lookup"><span data-stu-id="0ebe2-134">To create an open type, the CLR type must have a property of type `IDictionary<string, object>`, which holds the dynamic properties.</span></span>

## <a name="build-the-edm-model"></a><span data-ttu-id="0ebe2-135">Das EDM-Modell erstellen</span><span class="sxs-lookup"><span data-stu-id="0ebe2-135">Build the EDM Model</span></span>

<span data-ttu-id="0ebe2-136">Bei Verwendung von **ODataConventionModelBuilder** EDM erstellen `Press` und `Book` werden automatisch hinzugefügt, als offene Typen, die basierend auf dem Vorhandensein einer `IDictionary<string, object>` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="0ebe2-136">If you use **ODataConventionModelBuilder** to create the EDM, `Press` and `Book` are automatically added as open types, based on the presence of a `IDictionary<string, object>` property.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="0ebe2-137">Sie können auch erstellen, das EDM explizit mit **ODataModelBuilder**.</span><span class="sxs-lookup"><span data-stu-id="0ebe2-137">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a><span data-ttu-id="0ebe2-138">Fügen Sie einen OData-Controller hinzu.</span><span class="sxs-lookup"><span data-stu-id="0ebe2-138">Add an OData Controller</span></span>

<span data-ttu-id="0ebe2-139">Als Nächstes fügen Sie einen OData-Controller hinzu.</span><span class="sxs-lookup"><span data-stu-id="0ebe2-139">Next, add an OData controller.</span></span> <span data-ttu-id="0ebe2-140">In diesem Tutorial verwenden wir einen vereinfachten Controller nur unterstützt zu erhalten und POST-Anforderungen, und verwendet eine in-Memory-Liste, um Entitäten zu speichern.</span><span class="sxs-lookup"><span data-stu-id="0ebe2-140">For this tutorial, we'll use a simplified controller that just supports GET and POST requests, and uses an in-memory list to store entities.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

<span data-ttu-id="0ebe2-141">Beachten Sie, dass die erste `Book` Instanz hat keine dynamischen Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="0ebe2-141">Notice that the first `Book` instance has no dynamic properties.</span></span> <span data-ttu-id="0ebe2-142">Die zweite `Book` Instanz hat die folgenden dynamischen Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="0ebe2-142">The second `Book` instance has the following dynamic properties:</span></span>

- <span data-ttu-id="0ebe2-143">"Veröffentlicht": primitiver Typ</span><span class="sxs-lookup"><span data-stu-id="0ebe2-143">"Published": Primitive type</span></span>
- <span data-ttu-id="0ebe2-144">"Autoren": Auflistung von primitiven Typen</span><span class="sxs-lookup"><span data-stu-id="0ebe2-144">"Authors": Collection of primitive types</span></span>
- <span data-ttu-id="0ebe2-145">"OtherCategories": Auflistung von Enumerationstypen.</span><span class="sxs-lookup"><span data-stu-id="0ebe2-145">"OtherCategories": Collection of enumeration types.</span></span>

<span data-ttu-id="0ebe2-146">Darüber hinaus die `Press` -Eigenschaft dieses `Book` Instanz hat die folgenden dynamischen Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="0ebe2-146">Also, the `Press` property of that `Book` instance has the following dynamic properties:</span></span>

- <span data-ttu-id="0ebe2-147">"Blog": primitiver Typ</span><span class="sxs-lookup"><span data-stu-id="0ebe2-147">"Blog": Primitive type</span></span>
- <span data-ttu-id="0ebe2-148">"Address": komplexer Typ</span><span class="sxs-lookup"><span data-stu-id="0ebe2-148">"Address": Complex type</span></span>

## <a name="query-the-metadata"></a><span data-ttu-id="0ebe2-149">Abfragen der Metadaten</span><span class="sxs-lookup"><span data-stu-id="0ebe2-149">Query the Metadata</span></span>

<span data-ttu-id="0ebe2-150">Um das Metadatendokument des OData zu erhalten, senden Sie eine GET-Anforderung an `~/$metadata`.</span><span class="sxs-lookup"><span data-stu-id="0ebe2-150">To get the OData metadata document, send a GET request to `~/$metadata`.</span></span> <span data-ttu-id="0ebe2-151">Der Antworttext sollte etwa wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="0ebe2-151">The response body should look similar to this:</span></span>

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

<span data-ttu-id="0ebe2-152">Aus dem Dokument sehen Sie, die Folgendes:</span><span class="sxs-lookup"><span data-stu-id="0ebe2-152">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="0ebe2-153">Für die `Book` und `Press` Typen wird der Wert, der die `OpenType` -Attribut ist "true".</span><span class="sxs-lookup"><span data-stu-id="0ebe2-153">For the `Book` and `Press` types, the value of the `OpenType` attribute is true.</span></span> <span data-ttu-id="0ebe2-154">Die `Customer` und `Address` Typen nicht über dieses Attribut verfügen.</span><span class="sxs-lookup"><span data-stu-id="0ebe2-154">The `Customer` and `Address` types don't have this attribute.</span></span>
- <span data-ttu-id="0ebe2-155">Die `Book` Entitätstyp verfügt über drei deklarierten Eigenschaften: ISBN, Titel, und drücken Sie.</span><span class="sxs-lookup"><span data-stu-id="0ebe2-155">The `Book` entity type has three declared properties: ISBN, Title, and Press.</span></span> <span data-ttu-id="0ebe2-156">Die OData-Metadaten enthält keinen der `Book.Properties` Eigenschaft von der CLR-Klasse.</span><span class="sxs-lookup"><span data-stu-id="0ebe2-156">The OData metadata does not include the `Book.Properties` property from the CLR class.</span></span>
- <span data-ttu-id="0ebe2-157">Auf ähnliche Weise die `Press` komplexerer Typ verfügt über nur zwei deklarierten Eigenschaften: Name und Kategorie.</span><span class="sxs-lookup"><span data-stu-id="0ebe2-157">Similarly, the `Press` complex type has only two declared properties: Name and Category.</span></span> <span data-ttu-id="0ebe2-158">Die Metadaten enthält keinen der `Press.DynamicProperties` Eigenschaft von der CLR-Klasse.</span><span class="sxs-lookup"><span data-stu-id="0ebe2-158">The metadata does not include the `Press.DynamicProperties` property from the CLR class.</span></span>

## <a name="query-an-entity"></a><span data-ttu-id="0ebe2-159">Abfragen einer Entität</span><span class="sxs-lookup"><span data-stu-id="0ebe2-159">Query an Entity</span></span>

<span data-ttu-id="0ebe2-160">Um das Buch mit der ISBN-Nummer gleich "978-0-7356-7942-9" erhalten, senden Sie eine GET-Anforderung an `~/Books('978-0-7356-7942-9')`.</span><span class="sxs-lookup"><span data-stu-id="0ebe2-160">To get the book with ISBN equal to "978-0-7356-7942-9", send a GET request to `~/Books('978-0-7356-7942-9')`.</span></span> <span data-ttu-id="0ebe2-161">Der Antworttext sollte etwa wie folgt aussehen.</span><span class="sxs-lookup"><span data-stu-id="0ebe2-161">The response body should look similar to the following.</span></span> <span data-ttu-id="0ebe2-162">(Eingerückt, um sie besser lesbar zu machen.)</span><span class="sxs-lookup"><span data-stu-id="0ebe2-162">(Indented to make it more readable.)</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

<span data-ttu-id="0ebe2-163">Beachten Sie die dynamischen Eigenschaften sind die deklarierten Eigenschaften Inlineschemainformationen enthält.</span><span class="sxs-lookup"><span data-stu-id="0ebe2-163">Notice that the dynamic properties are included inline with the declared properties.</span></span>

## <a name="post-an-entity"></a><span data-ttu-id="0ebe2-164">Stellen Sie eine Entität</span><span class="sxs-lookup"><span data-stu-id="0ebe2-164">POST an Entity</span></span>

<span data-ttu-id="0ebe2-165">Senden Sie eine POST-Anforderung zum Hinzufügen einer Entität Buch `~/Books`.</span><span class="sxs-lookup"><span data-stu-id="0ebe2-165">To add a Book entity, send a POST request to `~/Books`.</span></span> <span data-ttu-id="0ebe2-166">Der Client kann dynamische Eigenschaften in der Anforderungsnutzlast festlegen.</span><span class="sxs-lookup"><span data-stu-id="0ebe2-166">The client can set dynamic properties in the request payload.</span></span>

<span data-ttu-id="0ebe2-167">Hier ist eine beispielanforderung ein.</span><span class="sxs-lookup"><span data-stu-id="0ebe2-167">Here is an example request.</span></span> <span data-ttu-id="0ebe2-168">Beachten Sie die Eigenschaften "Price" und "Veröffentlicht".</span><span class="sxs-lookup"><span data-stu-id="0ebe2-168">Note the "Price" and "Published" properties.</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

<span data-ttu-id="0ebe2-169">Wenn Sie einen Haltepunkt in der Controllermethode festlegen, Sie sehen, dass Web-API diese Eigenschaften auf der `Properties` Wörterbuch.</span><span class="sxs-lookup"><span data-stu-id="0ebe2-169">If you set a breakpoint in the controller method, you can see that Web API added these properties to the `Properties` dictionary.</span></span>

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a><span data-ttu-id="0ebe2-170">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="0ebe2-170">Additional Resources</span></span>

[<span data-ttu-id="0ebe2-171">Beispiel für OData öffnen</span><span class="sxs-lookup"><span data-stu-id="0ebe2-171">OData Open Type Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
