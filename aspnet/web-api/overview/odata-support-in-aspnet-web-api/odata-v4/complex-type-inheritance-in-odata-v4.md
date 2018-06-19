---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: Vererbung von komplexen Typ in OData v4 mit ASP.NET Web-API | Microsoft Docs
author: microsoft
description: Gemäß den OData v4-Spezifikation kann ein komplexer Typ von einem anderen komplexen Typ erben. (Ein komplexer Typ ist ein strukturierter Typ ohne einen Schlüssel.) Web-API...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/16/2014
ms.topic: article
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: be2dbfa82b99b6c48928e4e767716852c14a463b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508419"
---
<a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="a6302-104">Vererbung von komplexen Typ in OData v4 mit ASP.NET Web-API</span><span class="sxs-lookup"><span data-stu-id="a6302-104">Complex Type Inheritance in OData v4 with ASP.NET Web API</span></span>
====================
<span data-ttu-id="a6302-105">durch [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="a6302-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="a6302-106">Gemäß den OData v4 [Spezifikation](http://www.odata.org/documentation/odata-version-4-0/), ein komplexer Typ kann von einer anderen komplexen Typ erben.</span><span class="sxs-lookup"><span data-stu-id="a6302-106">According to the OData v4 [specification](http://www.odata.org/documentation/odata-version-4-0/), a complex type can inherit from another complex type.</span></span> <span data-ttu-id="a6302-107">(Ein *komplexe* Typ ist ein strukturierter Typ ohne einen Schlüssel.) Web API OData 5.3 unterstützt Vererbung von komplexen Typ.</span><span class="sxs-lookup"><span data-stu-id="a6302-107">(A *complex* type is a structured type without a key.) Web API OData 5.3 supports complex type inheritance.</span></span>
> 
> <span data-ttu-id="a6302-108">Dieses Thema veranschaulicht das Erstellen eines Entity Data Modells (EDM) mit von komplexen Vererbungstypen.</span><span class="sxs-lookup"><span data-stu-id="a6302-108">This topic shows how to build an entity data model (EDM) with complex inheritance types.</span></span> <span data-ttu-id="a6302-109">Den vollständigen Quellcode, finden Sie unter [OData-Beispiel für komplexe Vererbung](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="a6302-109">For the complete source code, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="a6302-110">In diesem Lernprogramm verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="a6302-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="a6302-111">Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="a6302-111">Web API OData 5.3</span></span>
> - <span data-ttu-id="a6302-112">OData v4</span><span class="sxs-lookup"><span data-stu-id="a6302-112">OData v4</span></span>


## <a name="model-hierarchy"></a><span data-ttu-id="a6302-113">Modellhierarchie</span><span class="sxs-lookup"><span data-stu-id="a6302-113">Model Hierarchy</span></span>

<span data-ttu-id="a6302-114">Um die Vererbung von komplexen Typ zu veranschaulichen, verwenden wir die folgenden Klassenhierarchie.</span><span class="sxs-lookup"><span data-stu-id="a6302-114">To illustrate complex type inheritance, we'll use the following class hierarchy.</span></span>

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

<span data-ttu-id="a6302-115">`Shape`ist ein abstrakte komplexer Typ.</span><span class="sxs-lookup"><span data-stu-id="a6302-115">`Shape` is an abstract complex type.</span></span> <span data-ttu-id="a6302-116">`Rectangle`, `Triangle`, und `Circle` komplexe Typen abgeleitet sind `Shape`, und `RoundRectangle` leitet sich von `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="a6302-116">`Rectangle`, `Triangle`, and `Circle` are complex types derived from `Shape`, and `RoundRectangle` derives from `Rectangle`.</span></span> <span data-ttu-id="a6302-117">`Window`ein Entitätstyp ist, und enthält eine `Shape` Instanz.</span><span class="sxs-lookup"><span data-stu-id="a6302-117">`Window` is an entity type and contains a `Shape` instance.</span></span>

<span data-ttu-id="a6302-118">Hier werden die CLR-Klassen, die diese Typen zu definieren.</span><span class="sxs-lookup"><span data-stu-id="a6302-118">Here are the CLR classes that define these types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a><span data-ttu-id="a6302-119">Das EDM-Modell erstellen</span><span class="sxs-lookup"><span data-stu-id="a6302-119">Build the EDM Model</span></span>

<span data-ttu-id="a6302-120">Um das EDM zu erstellen, können Sie **ODataConventionModelBuilder**, dem leitet die vererbungsbeziehungen von CLR-Typen ab.</span><span class="sxs-lookup"><span data-stu-id="a6302-120">To create the EDM, you can use **ODataConventionModelBuilder**, which infers the inheritance relationships from the CLR types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="a6302-121">Sie können auch Versionsattribut EDM explizit **ODataModelBuilder**.</span><span class="sxs-lookup"><span data-stu-id="a6302-121">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span> <span data-ttu-id="a6302-122">Dies nimmt mehr Code, bietet Ihnen jedoch mehr Kontrolle über das EDM.</span><span class="sxs-lookup"><span data-stu-id="a6302-122">This takes more code, but gives you more control over the EDM.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="a6302-123">Diese beiden Beispiele erstellen, das gleiche EDM-Schema.</span><span class="sxs-lookup"><span data-stu-id="a6302-123">These two examples create the same EDM schema.</span></span>

## <a name="metadata-document"></a><span data-ttu-id="a6302-124">Metadatendokument</span><span class="sxs-lookup"><span data-stu-id="a6302-124">Metadata Document</span></span>

<span data-ttu-id="a6302-125">Hier ist die Vererbung von komplexen Typ mit OData-Metadatendokument ein.</span><span class="sxs-lookup"><span data-stu-id="a6302-125">Here is the OData metadata document, showing complex type inheritance.</span></span>

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

<span data-ttu-id="a6302-126">Aus dem Metadatendokument sehen Sie, die Folgendes:</span><span class="sxs-lookup"><span data-stu-id="a6302-126">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="a6302-127">Die `Shape` komplexer Typ abstrakt ist.</span><span class="sxs-lookup"><span data-stu-id="a6302-127">The `Shape` complex type is abstract.</span></span>
- <span data-ttu-id="a6302-128">Die `Rectangle`, `Triangle`, und `Circle` komplexen Typs haben den Basistyp `Shape`.</span><span class="sxs-lookup"><span data-stu-id="a6302-128">The `Rectangle`, `Triangle`, and `Circle` complex type have the base type `Shape`.</span></span>
- <span data-ttu-id="a6302-129">Die `RoundRectangle` Typ ist den Basistyp `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="a6302-129">The `RoundRectangle` type has the base type `Rectangle`.</span></span>

## <a name="casting-complex-types"></a><span data-ttu-id="a6302-130">Umwandeln von komplexen Typen</span><span class="sxs-lookup"><span data-stu-id="a6302-130">Casting Complex Types</span></span>

<span data-ttu-id="a6302-131">Für complex-Typen umwandeln wird jetzt unterstützt.</span><span class="sxs-lookup"><span data-stu-id="a6302-131">Casting on complex types is now supported.</span></span> <span data-ttu-id="a6302-132">Die folgende Abfrage beispielsweise Umwandlungen eine `Shape` zu einem `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="a6302-132">For example, the following query casts a `Shape` to a `Rectangle`.</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

<span data-ttu-id="a6302-133">So sieht die antwortnutzlast aus:</span><span class="sxs-lookup"><span data-stu-id="a6302-133">Here's the response payload:</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
