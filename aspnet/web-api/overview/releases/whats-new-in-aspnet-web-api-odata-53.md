---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
title: Neues in ASP.NET Web API OData 5.3 | Microsoft Docs
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/16/2014
ms.topic: article
ms.assetid: e39eaa25-83ff-41dc-869d-3818d59a88ae
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
msc.type: authoredcontent
ms.openlocfilehash: e918f86ebd813f31ad0c21566e617482fc7dd963
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-web-api-odata-53"></a><span data-ttu-id="2a074-102">Was ist neu in ASP.NET Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="2a074-102">What's New in ASP.NET Web API OData 5.3</span></span>
====================
<span data-ttu-id="2a074-103">durch [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="2a074-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="2a074-104">Dieses Thema beschreibt ASP.NET Web API OData 5.3 Neuigkeiten.</span><span class="sxs-lookup"><span data-stu-id="2a074-104">This topic describes what's new for ASP.NET Web API OData 5.3.</span></span>

- [<span data-ttu-id="2a074-105">Herunterladen</span><span class="sxs-lookup"><span data-stu-id="2a074-105">Download</span></span>](#download)
- [<span data-ttu-id="2a074-106">Dokumentation</span><span class="sxs-lookup"><span data-stu-id="2a074-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="2a074-107">OData-Core-Bibliotheken</span><span class="sxs-lookup"><span data-stu-id="2a074-107">OData Core Libraries</span></span>](#corelib)
- [<span data-ttu-id="2a074-108">Neue Funktionen</span><span class="sxs-lookup"><span data-stu-id="2a074-108">New Features</span></span>](#newf)
- [<span data-ttu-id="2a074-109">Bekannte Probleme und aktueller Änderungen</span><span class="sxs-lookup"><span data-stu-id="2a074-109">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="2a074-110">Fehlerkorrekturen</span><span class="sxs-lookup"><span data-stu-id="2a074-110">Bug Fixes</span></span>](#bug-fixes)
- [<span data-ttu-id="2a074-111">ASP.NET Web API OData 5.3.1</span><span class="sxs-lookup"><span data-stu-id="2a074-111">ASP.NET Web API OData 5.3.1</span></span>](#OD)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="2a074-112">Herunterladen</span><span class="sxs-lookup"><span data-stu-id="2a074-112">Download</span></span>

<span data-ttu-id="2a074-113">Die Common Language Runtime-Funktionen werden als NuGet-Pakete auf der NuGet Gallery zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="2a074-113">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="2a074-114">Installieren und für die veröffentlichten NuGet-Pakete über NuGet-Paket-Manager-Konsole aktualisieren zu können:</span><span class="sxs-lookup"><span data-stu-id="2a074-114">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="2a074-115">Dokumentation</span><span class="sxs-lookup"><span data-stu-id="2a074-115">Documentation</span></span>

<span data-ttu-id="2a074-116">Finden Sie Lernprogramme und Weitere Dokumentationen zu ASP.NET Web API OData auf der [ASP.NET-Website](../odata-support-in-aspnet-web-api/index.md).</span><span class="sxs-lookup"><span data-stu-id="2a074-116">You can find tutorials and other documentation about ASP.NET Web API OData at the [ASP.NET web site](../odata-support-in-aspnet-web-api/index.md).</span></span>

<a id="corelib"></a>
## <a name="odata-core-libraries"></a><span data-ttu-id="2a074-117">OData-Core-Bibliotheken</span><span class="sxs-lookup"><span data-stu-id="2a074-117">OData Core Libraries</span></span>

<span data-ttu-id="2a074-118">Für OData v4 verwendet Web-API jetzt ODataLib Version 6.5.0 ist</span><span class="sxs-lookup"><span data-stu-id="2a074-118">For OData v4, Web API now uses ODataLib version 6.5.0</span></span>

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-odata-53"></a><span data-ttu-id="2a074-119">Neue Funktionen in ASP.NET Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="2a074-119">New Features in ASP.NET Web API OData 5.3</span></span>

### <a name="support-for-levels-in-expand"></a><span data-ttu-id="2a074-120">Unterstützung für $levels in $erweitern</span><span class="sxs-lookup"><span data-stu-id="2a074-120">Support for $levels in $expand</span></span>

<span data-ttu-id="2a074-121">Sie können die $levels Abfrage, die Option in der $ Abfragen zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="2a074-121">You can use the $levels query option in $expand queries.</span></span> <span data-ttu-id="2a074-122">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="2a074-122">For example:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample2.cmd)]

<span data-ttu-id="2a074-123">Diese Abfrage ist äquivalent zu:</span><span class="sxs-lookup"><span data-stu-id="2a074-123">This query is equivalent to:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample3.cmd)]

<a id="open-entity-types"></a>
### <a name="support-for-open-entity-types"></a><span data-ttu-id="2a074-124">Unterstützung für Entitätstypen öffnen</span><span class="sxs-lookup"><span data-stu-id="2a074-124">Support for Open Entity Types</span></span>

<span data-ttu-id="2a074-125">Ein *öffnen geben* ist ein Stuctured-Typ, die dynamische Eigenschaften, zusätzlich zu den Eigenschaften enthält, die in der Typdefinition deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="2a074-125">An *open type* is a stuctured type that contains dynamic properties, in addition to any properties that are declared in the type definition.</span></span> <span data-ttu-id="2a074-126">Offene Typen können Sie Ihre Datenmodelle Flexibilität hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="2a074-126">Open types let you add flexibility to your data models.</span></span> <span data-ttu-id="2a074-127">Weitere Informationen finden Sie unter "Xxxx".</span><span class="sxs-lookup"><span data-stu-id="2a074-127">For more information, see xxxx.</span></span>

### <a name="support-for-dynamic-collection-properties-in-open-types"></a><span data-ttu-id="2a074-128">Unterstützung für dynamische Sammlungseigenschaften in offenen Typen</span><span class="sxs-lookup"><span data-stu-id="2a074-128">Support for dynamic collection properties in open types</span></span>

<span data-ttu-id="2a074-129">Bisher mussten eine dynamische Eigenschaft, einen einzelnen Wert sein.</span><span class="sxs-lookup"><span data-stu-id="2a074-129">Previously, a dynamic property had to be a single value.</span></span> <span data-ttu-id="2a074-130">In 5.3 kann über die dynamischen Eigenschaften ausflistungswerten verfügen.</span><span class="sxs-lookup"><span data-stu-id="2a074-130">In 5.3, dynamic properties can have collection values.</span></span> <span data-ttu-id="2a074-131">In der folgenden JSON-Nutzlast, z. B. die `Emails` -Eigenschaft ist eine dynamische Eigenschaft und der Auflistung der Zeichenfolgentyp ist:</span><span class="sxs-lookup"><span data-stu-id="2a074-131">For example, in the following JSON payload, the `Emails` property is a dynamic property and is of collection of string type:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample4.cmd)]

### <a name="support-for-inheritance-for-complex-types"></a><span data-ttu-id="2a074-132">Unterstützung von Vererbung für komplexe Typen</span><span class="sxs-lookup"><span data-stu-id="2a074-132">Support for inheritance for complex types</span></span>

<span data-ttu-id="2a074-133">Komplexe Typen können jetzt von einem Basistyp erben.</span><span class="sxs-lookup"><span data-stu-id="2a074-133">Now complex types can inherit from a base type.</span></span> <span data-ttu-id="2a074-134">Ein OData-Dienst kann z. B. folgenden komplexen Typen definieren:</span><span class="sxs-lookup"><span data-stu-id="2a074-134">For example, an OData service could define the following complex types:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample5.cs)]

<span data-ttu-id="2a074-135">Dies ist das EDM für dieses Beispiel aus:</span><span class="sxs-lookup"><span data-stu-id="2a074-135">Here is the EDM for this example:</span></span>

[!code-xml[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample6.xml?highlight=8,15)]

<span data-ttu-id="2a074-136">Weitere Informationen finden Sie unter [OData-Beispiel für komplexe Vererbung](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="2a074-136">For more information, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="2a074-137">Bekannte Probleme und aktueller Änderungen</span><span class="sxs-lookup"><span data-stu-id="2a074-137">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="2a074-138">Dieser Abschnitt beschreibt bekannte Probleme und neueste Änderungen in der ASP.NET Web API OData 5.3.</span><span class="sxs-lookup"><span data-stu-id="2a074-138">This section describes known issues and breaking changes in the ASP.NET Web API OData 5.3.</span></span>

### <a name="odata-v4"></a><span data-ttu-id="2a074-139">OData v4</span><span class="sxs-lookup"><span data-stu-id="2a074-139">OData v4</span></span>

#### <a name="query-options"></a><span data-ttu-id="2a074-140">Abfrageoptionen</span><span class="sxs-lookup"><span data-stu-id="2a074-140">Query Options</span></span>

<span data-ttu-id="2a074-141">Problem: Verwenden von geschachtelten $erweitern mit $levels = max führt zu einer falschen erweiterungstiefe.</span><span class="sxs-lookup"><span data-stu-id="2a074-141">Issue: Using nested $expand with $levels=max results in an incorrect expansion depth.</span></span>

<span data-ttu-id="2a074-142">Betrachten Sie z. B. die folgende Anforderung:</span><span class="sxs-lookup"><span data-stu-id="2a074-142">For example, given the following request:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample7.cmd)]

<span data-ttu-id="2a074-143">Wenn `MaxExpansionDepth` 5 ist, diese Abfrage würde eine erweiterungstiefe 6.</span><span class="sxs-lookup"><span data-stu-id="2a074-143">If `MaxExpansionDepth` is 5, this query would result in an expansion depth of 6.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a><span data-ttu-id="2a074-144">Fehlerbehebungen und kleinere Feature-Updates</span><span class="sxs-lookup"><span data-stu-id="2a074-144">Bug Fixes and Minor Feature Updates</span></span>

<span data-ttu-id="2a074-145">Diese Version enthält auch mehrere Programmfehlerbehebungen und kleinere Funktion Updates.</span><span class="sxs-lookup"><span data-stu-id="2a074-145">This release also includes several bug fixes and minor feature updates.</span></span> <span data-ttu-id="2a074-146">Sie können die vollständige Liste hier finden:</span><span class="sxs-lookup"><span data-stu-id="2a074-146">You can find the complete list here:</span></span>

- [<span data-ttu-id="2a074-147">Fehlerkorrekturen</span><span class="sxs-lookup"><span data-stu-id="2a074-147">Bug fixes</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.3%20Beta&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="OD"></a>
## <a name="aspnet-web-api-odata-531"></a><span data-ttu-id="2a074-148">ASP.NET Web API OData 5.3.1</span><span class="sxs-lookup"><span data-stu-id="2a074-148">ASP.NET Web API OData 5.3.1</span></span>

<span data-ttu-id="2a074-149">In dieser Version erzielt wir eine [Fehlerkorrektur](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All) einiger AllowedFunctions-Enumerationen.</span><span class="sxs-lookup"><span data-stu-id="2a074-149">In this release we made a [bug fix](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All) to some of the AllowedFunctions enums.</span></span> <span data-ttu-id="2a074-150">Diese Version keine keine weiteren Fehlerkorrekturen oder neue Features.</span><span class="sxs-lookup"><span data-stu-id="2a074-150">This release doesn't have any other bug fixes or new features.</span></span>
