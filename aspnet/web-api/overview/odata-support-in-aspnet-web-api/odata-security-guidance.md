---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
title: Sicherheitsempfehlungen für ASP.NET Web-API 2 OData | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 02/06/2013
ms.assetid: b91e6424-1544-4747-bd0b-d1f8418c9653
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
msc.type: authoredcontent
ms.openlocfilehash: 2a5b776a81cb3e3cf809dd3c4229448988086a32
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823744"
---
<a name="security-guidance-for-aspnet-web-api-2-odata"></a><span data-ttu-id="89f3f-102">Sicherheitsempfehlungen für ASP.NET Web-API 2 OData</span><span class="sxs-lookup"><span data-stu-id="89f3f-102">Security Guidance for ASP.NET Web API 2 OData</span></span>
====================
<span data-ttu-id="89f3f-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="89f3f-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="89f3f-104">Dieses Thema beschreibt einige der Sicherheitsprobleme, die Sie berücksichtigen sollten, wenn Sie ein Dataset über OData verfügbar zu machen.</span><span class="sxs-lookup"><span data-stu-id="89f3f-104">This topic describes some of the security issues that you should consider when exposing a dataset through OData.</span></span>

## <a name="edm-security"></a><span data-ttu-id="89f3f-105">EDM-Sicherheit</span><span class="sxs-lookup"><span data-stu-id="89f3f-105">EDM Security</span></span>

<span data-ttu-id="89f3f-106">Die Semantik der Abfrage basiert auf Entity Data Model (EDM) gibt nicht die zugrunde liegenden Modelltypen.</span><span class="sxs-lookup"><span data-stu-id="89f3f-106">The query semantics are based on the entity data model (EDM), not the underlying model types.</span></span> <span data-ttu-id="89f3f-107">Sie können eine Eigenschaft ausschließen, aus dem EDM und wird nicht für die Abfrage sichtbar sein.</span><span class="sxs-lookup"><span data-stu-id="89f3f-107">You can exclude a property from the EDM and it will not be visible to the query.</span></span> <span data-ttu-id="89f3f-108">Nehmen wir beispielsweise an, dass Ihr Modell einen Employee-Typ mit einem Gehalt-Eigenschaft enthält.</span><span class="sxs-lookup"><span data-stu-id="89f3f-108">For example, suppose your model includes an Employee type with a Salary property.</span></span> <span data-ttu-id="89f3f-109">Sie sollten diese Eigenschaft des EDM, um es von Clients ausblenden ausgeschlossen werden.</span><span class="sxs-lookup"><span data-stu-id="89f3f-109">You might want to exclude this property from the EDM to hide it from clients.</span></span>

<span data-ttu-id="89f3f-110">Es gibt zwei Möglichkeiten, schließt eine Eigenschaft aus dem EDM.</span><span class="sxs-lookup"><span data-stu-id="89f3f-110">There are two ways to exlude a property from the EDM.</span></span> <span data-ttu-id="89f3f-111">Sie können festlegen, die **[IgnoreDataMember]** Attribut für die Eigenschaft in der Modellklasse:</span><span class="sxs-lookup"><span data-stu-id="89f3f-111">You can set the **[IgnoreDataMember]** attribute on the property in the model class:</span></span>

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

<span data-ttu-id="89f3f-112">Sie können auch programmgesteuert die-Eigenschaft von EDM entfernen:</span><span class="sxs-lookup"><span data-stu-id="89f3f-112">You can also remove the property from the EDM programmatically:</span></span>

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a><span data-ttu-id="89f3f-113">Abfragesicherheit</span><span class="sxs-lookup"><span data-stu-id="89f3f-113">Query Security</span></span>

<span data-ttu-id="89f3f-114">Ein böswilliger oder naive-Client möglicherweise zum Erstellen einer Abfrage, die zum Ausführen sehr lange dauert.</span><span class="sxs-lookup"><span data-stu-id="89f3f-114">A malicious or naive client may be able to construct a query that takes a very long time to execute.</span></span> <span data-ttu-id="89f3f-115">Im schlimmsten Fall kann dies den Zugriff auf den Dienst unterbrechen.</span><span class="sxs-lookup"><span data-stu-id="89f3f-115">In the worst case this can disrupt access to your service.</span></span>

<span data-ttu-id="89f3f-116">Die **[Queryable]** -Attribut ist ein Aktionsfilter, der analysiert, überprüft und wendet die Abfrage.</span><span class="sxs-lookup"><span data-stu-id="89f3f-116">The **[Queryable]** attribute is an action filter that parses, validates, and applies the query.</span></span> <span data-ttu-id="89f3f-117">Der Filter konvertiert die Abfrageoptionen in einem LINQ-Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="89f3f-117">The filter converts the query options into a LINQ expression.</span></span> <span data-ttu-id="89f3f-118">Wenn der OData-Controller gibt eine **"IQueryable"** Typ, der **"IQueryable"** LINQ-Anbieter konvertiert den LINQ-Ausdruck in einer Abfrage.</span><span class="sxs-lookup"><span data-stu-id="89f3f-118">When the OData controller returns an **IQueryable** type, the **IQueryable** LINQ provider converts the LINQ expression into a query.</span></span> <span data-ttu-id="89f3f-119">Aus diesem Grund hängt die Leistung auf der LINQ-Anbieter, der verwendet wird, sowie auf den bestimmten Merkmalen Ihres Schemas Dataset oder eine Datenbank.</span><span class="sxs-lookup"><span data-stu-id="89f3f-119">Therefore, performance depends on the LINQ provider that is used, and also on the particular characteristics of your dataset or database schema.</span></span>

<span data-ttu-id="89f3f-120">Weitere Informationen zur Verwendung von OData-Abfrageoptionen in ASP.NET Web-API finden Sie unter [unterstützt OData-Abfrageoptionen](supporting-odata-query-options.md).</span><span class="sxs-lookup"><span data-stu-id="89f3f-120">For more information about using OData query options in ASP.NET Web API, see [Supporting OData Query Options](supporting-odata-query-options.md).</span></span>

<span data-ttu-id="89f3f-121">Wenn Sie wissen, dass alle Clients (z. B. in einer unternehmensumgebung) als vertrauenswürdig eingestuft werden, oder wenn Ihr Dataset klein ist, kann die abfrageleistung kein Problem dar.</span><span class="sxs-lookup"><span data-stu-id="89f3f-121">If you know that all clients are trusted (for example, in an enterprise environment), or if your dataset is small, query performance might not be an issue.</span></span> <span data-ttu-id="89f3f-122">Andernfalls sollten Sie die folgenden Empfehlungen.</span><span class="sxs-lookup"><span data-stu-id="89f3f-122">Otherwise, you should consider the following recommendations.</span></span>

- <span data-ttu-id="89f3f-123">Testen Sie des Diensts mit verschiedenen Abfragen aus, und Stufen Sie die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="89f3f-123">Test your service with various queries and profile the DB.</span></span>
- <span data-ttu-id="89f3f-124">Aktivieren Sie servergesteuertes Paging, um zu vermeiden, ein großes Dataset in einer Abfrage zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="89f3f-124">Enable server-driven paging, to avoid returning a large data set in one query.</span></span> <span data-ttu-id="89f3f-125">Weitere Informationen finden Sie unter [Server-Driven Paging](supporting-odata-query-options.md#server-paging).</span><span class="sxs-lookup"><span data-stu-id="89f3f-125">For more information, see [Server-Driven Paging](supporting-odata-query-options.md#server-paging).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- <span data-ttu-id="89f3f-126">Erforderlich $filter und $orderby?</span><span class="sxs-lookup"><span data-stu-id="89f3f-126">Do you need $filter and $orderby?</span></span> <span data-ttu-id="89f3f-127">Bei einigen Anwendungen können paging, mithilfe von $top und $skip zulassen, aber der anderen Abfrageoptionen deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="89f3f-127">Some applications might allow client paging, using $top and $skip, but disable the other query options.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- <span data-ttu-id="89f3f-128">Beachten Sie, $orderby auf Eigenschaften in einem gruppierten Index zu beschränken.</span><span class="sxs-lookup"><span data-stu-id="89f3f-128">Consider restricting $orderby to properties in a clustered index.</span></span> <span data-ttu-id="89f3f-129">Sortieren von großen Daten ohne gruppierten Index ist langsam.</span><span class="sxs-lookup"><span data-stu-id="89f3f-129">Sorting large data without a clustered index is slow.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- <span data-ttu-id="89f3f-130">Maximale Knotenanzahl: die **MaxNodeCount** Eigenschaft **[Queryable]** legt die maximale Anzahl Knoten, die in der Syntaxstruktur $filter erlaubt.</span><span class="sxs-lookup"><span data-stu-id="89f3f-130">Maximum node count: The **MaxNodeCount** property on **[Queryable]** sets the maximum number nodes allowed in the $filter syntax tree.</span></span> <span data-ttu-id="89f3f-131">Der Standardwert ist 100, aber Sie möchten ein niedrigerer Wert, da eine große Anzahl von Knoten zum Kompilieren langsam sein kann.</span><span class="sxs-lookup"><span data-stu-id="89f3f-131">The default value is 100, but you may want to set a lower value, because a large number of nodes can be slow to compile.</span></span> <span data-ttu-id="89f3f-132">Dies gilt insbesondere bei Verwendung von LINQ to Objects (z. B. LINQ-Abfragen in einer Auflistung im Arbeitsspeicher, ohne einen zwischengeschalteten LINQ-Anbieter).</span><span class="sxs-lookup"><span data-stu-id="89f3f-132">This is particularly true if you are using LINQ to Objects (i.e., LINQ queries on a collection in memory, without the use of an intermediate LINQ provider).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- <span data-ttu-id="89f3f-133">Deaktivieren Sie ggf. die Funktionen any() und all(), da diese langsam sein können.</span><span class="sxs-lookup"><span data-stu-id="89f3f-133">Consider disabling the any() and all() functions, as these can be slow.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- <span data-ttu-id="89f3f-134">Wenn alle Zeichenfolgeneigenschaften große Zeichenfolgen #8212for Beispiel produktbeschreibungen oder einen Blogeintrag &, #8212consider Deaktivieren der String-Funktionen enthalten.</span><span class="sxs-lookup"><span data-stu-id="89f3f-134">If any string properties contain large strings&#8212for example, a product description or a blog entry&#8212consider disabling the string functions.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- <span data-ttu-id="89f3f-135">Beachten Sie, untersagen von Filtern auf Navigationseigenschaften.</span><span class="sxs-lookup"><span data-stu-id="89f3f-135">Consider disallowing filtering on navigation properties.</span></span> <span data-ttu-id="89f3f-136">Filterung für Navigationseigenschaften kann in einem Join führen, die je nach Schema Ihrer Datenbank langsam sein kann.</span><span class="sxs-lookup"><span data-stu-id="89f3f-136">Filtering on navigation properties can result in a join, which might be slow, depending on your database schema.</span></span> <span data-ttu-id="89f3f-137">Der folgende Code zeigt ein Abfrage-Validierungssteuerelement, das verhindert, dass für Navigationseigenschaften des filtern.</span><span class="sxs-lookup"><span data-stu-id="89f3f-137">The following code shows a query validator that prevents filtering on navigation properties.</span></span> <span data-ttu-id="89f3f-138">Weitere Informationen zu Abfrage-Validierungssteuerelemente, finden Sie unter [Abfragevalidierung](supporting-odata-query-options.md#query-validation).</span><span class="sxs-lookup"><span data-stu-id="89f3f-138">For more information about query validators, see [Query Validation](supporting-odata-query-options.md#query-validation).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- <span data-ttu-id="89f3f-139">Beachten Sie, Einschränken von $filter Abfragen durch Schreiben ein Validierungssteuerelement, das für Ihre Datenbank angepasst wird.</span><span class="sxs-lookup"><span data-stu-id="89f3f-139">Consider restricting $filter queries by writing a validator that is customized for your database.</span></span> <span data-ttu-id="89f3f-140">Betrachten Sie beispielsweise diese beiden Abfragen aus:</span><span class="sxs-lookup"><span data-stu-id="89f3f-140">For example, consider these two queries:</span></span> 

  - <span data-ttu-id="89f3f-141">Alle Filme mit Actors, deren Nachname mit "A" beginnt.</span><span class="sxs-lookup"><span data-stu-id="89f3f-141">All movies with actors whose last name starts with ‘A'.</span></span>
  - <span data-ttu-id="89f3f-142">Alle Filme im 1994 veröffentlicht wurde.</span><span class="sxs-lookup"><span data-stu-id="89f3f-142">All movies released in 1994.</span></span>

    <span data-ttu-id="89f3f-143">Es sei denn, Filme Akteure indiziert sind, müssen möglicherweise die erste Abfrage der Datenbank-Engine, um die gesamte Liste von Filmen zu scannen.</span><span class="sxs-lookup"><span data-stu-id="89f3f-143">Unless movies are indexed by actors, the first query might require the DB engine to scan the entire list of movies.</span></span> <span data-ttu-id="89f3f-144">Während die zweite Abfrage zulässig ist, werden die Annahme, dass Filme von Veröffentlichungsjahr indiziert.</span><span class="sxs-lookup"><span data-stu-id="89f3f-144">Whereas the second query might be acceptable, assuming movies are indexed by release year.</span></span>

    <span data-ttu-id="89f3f-145">Der folgende Code zeigt ein Validierungssteuerelement, das ermöglicht das Filtern auf die Eigenschaften "ReleaseYear" und "Title", aber keine anderen Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="89f3f-145">The following code shows a validator that allows filtering on the "ReleaseYear" and "Title" properties but no other properties.</span></span>

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- <span data-ttu-id="89f3f-146">Im Allgemeinen sollten Sie die $filter-Funktionen, die Sie benötigen.</span><span class="sxs-lookup"><span data-stu-id="89f3f-146">In general, consider which $filter functions you need.</span></span> <span data-ttu-id="89f3f-147">Wenn Ihre Clients die vollständige ausdrucksfähigkeit der $filter nicht benötigen, können Sie die zulässigen Funktionen begrenzen.</span><span class="sxs-lookup"><span data-stu-id="89f3f-147">If your clients do not need the full expressiveness of $filter, you can limit the allowed functions.</span></span>
