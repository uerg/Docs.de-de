---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
title: Was ist neu in ASP.NET Web API OData 5.3 | Microsoft-Dokumentation
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 09/16/2014
ms.assetid: e39eaa25-83ff-41dc-869d-3818d59a88ae
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
msc.type: authoredcontent
ms.openlocfilehash: c185e7ef9bfe6e21601ab61c418c63c8a81b680a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827028"
---
<a name="whats-new-in-aspnet-web-api-odata-53"></a><span data-ttu-id="d1936-102">Neuerungen in ASP.NET Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="d1936-102">What's New in ASP.NET Web API OData 5.3</span></span>
====================
<span data-ttu-id="d1936-103">durch [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="d1936-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="d1936-104">Dieses Thema beschreibt, was neu in ASP.NET Web API OData 5.3 ist.</span><span class="sxs-lookup"><span data-stu-id="d1936-104">This topic describes what's new for ASP.NET Web API OData 5.3.</span></span>

- [<span data-ttu-id="d1936-105">Herunterladen</span><span class="sxs-lookup"><span data-stu-id="d1936-105">Download</span></span>](#download)
- [<span data-ttu-id="d1936-106">Dokumentation</span><span class="sxs-lookup"><span data-stu-id="d1936-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="d1936-107">OData-Kernbibliotheken</span><span class="sxs-lookup"><span data-stu-id="d1936-107">OData Core Libraries</span></span>](#corelib)
- [<span data-ttu-id="d1936-108">Neue Features</span><span class="sxs-lookup"><span data-stu-id="d1936-108">New Features</span></span>](#newf)
- [<span data-ttu-id="d1936-109">Bekannte Probleme und aktueller Änderungen</span><span class="sxs-lookup"><span data-stu-id="d1936-109">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="d1936-110">Fehlerbehebungen</span><span class="sxs-lookup"><span data-stu-id="d1936-110">Bug Fixes</span></span>](#bug-fixes)
- [<span data-ttu-id="d1936-111">ASP.NET Web API OData 5.3.1</span><span class="sxs-lookup"><span data-stu-id="d1936-111">ASP.NET Web API OData 5.3.1</span></span>](#OD)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="d1936-112">Herunterladen</span><span class="sxs-lookup"><span data-stu-id="d1936-112">Download</span></span>

<span data-ttu-id="d1936-113">Die CLR-Funktionen werden als NuGet-Pakete im NuGet-Katalog veröffentlicht.</span><span class="sxs-lookup"><span data-stu-id="d1936-113">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="d1936-114">Sie können die installieren oder aktualisieren Sie auf die veröffentlichte NuGet-Pakete mithilfe der NuGet-Paket-Manager-Konsole:</span><span class="sxs-lookup"><span data-stu-id="d1936-114">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="d1936-115">Dokumentation</span><span class="sxs-lookup"><span data-stu-id="d1936-115">Documentation</span></span>

<span data-ttu-id="d1936-116">Finden Sie Tutorials und anderem Dokumentationsmaterial zu ASP.NET Web API OData auf der [ASP.NET-Website](../odata-support-in-aspnet-web-api/index.md).</span><span class="sxs-lookup"><span data-stu-id="d1936-116">You can find tutorials and other documentation about ASP.NET Web API OData at the [ASP.NET web site](../odata-support-in-aspnet-web-api/index.md).</span></span>

<a id="corelib"></a>
## <a name="odata-core-libraries"></a><span data-ttu-id="d1936-117">OData-Kernbibliotheken</span><span class="sxs-lookup"><span data-stu-id="d1936-117">OData Core Libraries</span></span>

<span data-ttu-id="d1936-118">Für OData v4 und verwendet Web-API jetzt ODataLib Version 6.5.0</span><span class="sxs-lookup"><span data-stu-id="d1936-118">For OData v4, Web API now uses ODataLib version 6.5.0</span></span>

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-odata-53"></a><span data-ttu-id="d1936-119">Neue Funktionen in ASP.NET Web-API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="d1936-119">New Features in ASP.NET Web API OData 5.3</span></span>

### <a name="support-for-levels-in-expand"></a><span data-ttu-id="d1936-120">Unterstützung für $levels in $erweitern</span><span class="sxs-lookup"><span data-stu-id="d1936-120">Support for $levels in $expand</span></span>

<span data-ttu-id="d1936-121">Sie können die $levels Abfrage, die Option in der $ Abfragen zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="d1936-121">You can use the $levels query option in $expand queries.</span></span> <span data-ttu-id="d1936-122">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="d1936-122">For example:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample2.cmd)]

<span data-ttu-id="d1936-123">Diese Abfrage entspricht:</span><span class="sxs-lookup"><span data-stu-id="d1936-123">This query is equivalent to:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample3.cmd)]

<a id="open-entity-types"></a>
### <a name="support-for-open-entity-types"></a><span data-ttu-id="d1936-124">Unterstützung für öffnen Entitätstypen</span><span class="sxs-lookup"><span data-stu-id="d1936-124">Support for Open Entity Types</span></span>

<span data-ttu-id="d1936-125">Ein *offener Typ* ist ein Stuctured, die dynamische Eigenschaften, zusätzlich zu den Eigenschaften enthält, die in der Definition eines Typs deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="d1936-125">An *open type* is a stuctured type that contains dynamic properties, in addition to any properties that are declared in the type definition.</span></span> <span data-ttu-id="d1936-126">Offene Typen können Sie die Flexibilität mit Ihren Datenmodellen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="d1936-126">Open types let you add flexibility to your data models.</span></span> <span data-ttu-id="d1936-127">Weitere Informationen finden Sie unter Xxxx.</span><span class="sxs-lookup"><span data-stu-id="d1936-127">For more information, see xxxx.</span></span>

### <a name="support-for-dynamic-collection-properties-in-open-types"></a><span data-ttu-id="d1936-128">Unterstützung für dynamische Sammlung von Eigenschaften in offenen Typen</span><span class="sxs-lookup"><span data-stu-id="d1936-128">Support for dynamic collection properties in open types</span></span>

<span data-ttu-id="d1936-129">Zuvor mussten eine dynamische Eigenschaft ein einzelner Wert sein.</span><span class="sxs-lookup"><span data-stu-id="d1936-129">Previously, a dynamic property had to be a single value.</span></span> <span data-ttu-id="d1936-130">In 5.3 haben dynamische Eigenschaften der Auflistungswerte.</span><span class="sxs-lookup"><span data-stu-id="d1936-130">In 5.3, dynamic properties can have collection values.</span></span> <span data-ttu-id="d1936-131">Z. B. in der folgenden JSON-Nutzlast die `Emails` -Eigenschaft ist eine dynamische Eigenschaft und die Auflistung von String-Typ ist:</span><span class="sxs-lookup"><span data-stu-id="d1936-131">For example, in the following JSON payload, the `Emails` property is a dynamic property and is of collection of string type:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample4.cmd)]

### <a name="support-for-inheritance-for-complex-types"></a><span data-ttu-id="d1936-132">Unterstützung für die Vererbung für komplexe Typen</span><span class="sxs-lookup"><span data-stu-id="d1936-132">Support for inheritance for complex types</span></span>

<span data-ttu-id="d1936-133">Komplexe Typen können nun von einem Basistyp erben.</span><span class="sxs-lookup"><span data-stu-id="d1936-133">Now complex types can inherit from a base type.</span></span> <span data-ttu-id="d1936-134">Ein OData-Dienst kann z. B. folgenden komplexen Typen definieren:</span><span class="sxs-lookup"><span data-stu-id="d1936-134">For example, an OData service could define the following complex types:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample5.cs)]

<span data-ttu-id="d1936-135">So sieht das EDM in diesem Beispiel aus:</span><span class="sxs-lookup"><span data-stu-id="d1936-135">Here is the EDM for this example:</span></span>

[!code-xml[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample6.xml?highlight=8,15)]

<span data-ttu-id="d1936-136">Weitere Informationen finden Sie unter [OData Beispiel für komplexe Vererbung](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="d1936-136">For more information, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="d1936-137">Bekannte Probleme und aktueller Änderungen</span><span class="sxs-lookup"><span data-stu-id="d1936-137">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="d1936-138">Dieser Abschnitt beschreibt bekannte Probleme und wichtige Änderungen in der ASP.NET Web API OData 5.3.</span><span class="sxs-lookup"><span data-stu-id="d1936-138">This section describes known issues and breaking changes in the ASP.NET Web API OData 5.3.</span></span>

### <a name="odata-v4"></a><span data-ttu-id="d1936-139">OData v4</span><span class="sxs-lookup"><span data-stu-id="d1936-139">OData v4</span></span>

#### <a name="query-options"></a><span data-ttu-id="d1936-140">Abfrageoptionen</span><span class="sxs-lookup"><span data-stu-id="d1936-140">Query Options</span></span>

<span data-ttu-id="d1936-141">Problem: Verwenden von geschachtelten $erweitern mit $levels = maximale Anzahl von Ergebnissen in einer falschen erweiterungstiefe.</span><span class="sxs-lookup"><span data-stu-id="d1936-141">Issue: Using nested $expand with $levels=max results in an incorrect expansion depth.</span></span>

<span data-ttu-id="d1936-142">Betrachten Sie z. B. die folgende Anforderung:</span><span class="sxs-lookup"><span data-stu-id="d1936-142">For example, given the following request:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample7.cmd)]

<span data-ttu-id="d1936-143">Wenn `MaxExpansionDepth` 5 ist, diese Abfrage würde eine erweiterungstiefe 6.</span><span class="sxs-lookup"><span data-stu-id="d1936-143">If `MaxExpansionDepth` is 5, this query would result in an expansion depth of 6.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a><span data-ttu-id="d1936-144">Fehlerbehebungen und kleinere Feature-Updates</span><span class="sxs-lookup"><span data-stu-id="d1936-144">Bug Fixes and Minor Feature Updates</span></span>

<span data-ttu-id="d1936-145">Diese Version umfasst auch mehrere Fehlerbehebungen und kleinere Feature Updates.</span><span class="sxs-lookup"><span data-stu-id="d1936-145">This release also includes several bug fixes and minor feature updates.</span></span> <span data-ttu-id="d1936-146">Sie können hier die vollständige Liste finden:</span><span class="sxs-lookup"><span data-stu-id="d1936-146">You can find the complete list here:</span></span>

- [<span data-ttu-id="d1936-147">Fehlerkorrekturen</span><span class="sxs-lookup"><span data-stu-id="d1936-147">Bug fixes</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.3%20Beta&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="OD"></a>
## <a name="aspnet-web-api-odata-531"></a><span data-ttu-id="d1936-148">ASP.NET Web API OData 5.3.1</span><span class="sxs-lookup"><span data-stu-id="d1936-148">ASP.NET Web API OData 5.3.1</span></span>

<span data-ttu-id="d1936-149">In dieser Version, die wir vorgenommen eine [Programmfehlerbehebung](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All) einiger der AllowedFunctions Enumerationen.</span><span class="sxs-lookup"><span data-stu-id="d1936-149">In this release we made a [bug fix](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All) to some of the AllowedFunctions enums.</span></span> <span data-ttu-id="d1936-150">Diese Version ist keine anderen Fehlerbehebungen oder neuen Features.</span><span class="sxs-lookup"><span data-stu-id="d1936-150">This release doesn't have any other bug fixes or new features.</span></span>
