---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: Dynamische VS. Stark typisierte Ansichten | Microsoft-Dokumentation
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/27/2011
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 41a47f0f6e5fd900e8bf35c37ed98566fe416ace
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823881"
---
<a name="dynamic-v-strongly-typed-views"></a><span data-ttu-id="02a7b-103">Dynamische VS.</span><span class="sxs-lookup"><span data-stu-id="02a7b-103">Dynamic v.</span></span> <span data-ttu-id="02a7b-104">Stark typisierte Ansichten</span><span class="sxs-lookup"><span data-stu-id="02a7b-104">Strongly Typed Views</span></span>
====================
<span data-ttu-id="02a7b-105">durch [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="02a7b-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

<span data-ttu-id="02a7b-106">Es gibt drei Möglichkeiten zum Übergeben von Informationen von einem Controller an eine Ansicht in ASP.NET MVC 3:</span><span class="sxs-lookup"><span data-stu-id="02a7b-106">There are three ways to pass information from a controller to a view in ASP.NET MVC 3:</span></span>

1. <span data-ttu-id="02a7b-107">Als ein stark typisiertes Modell-Objekt.</span><span class="sxs-lookup"><span data-stu-id="02a7b-107">As a strongly typed model object.</span></span>
2. <span data-ttu-id="02a7b-108">Als ein dynamischer Typ (mit @model dynamische)</span><span class="sxs-lookup"><span data-stu-id="02a7b-108">As a dynamic type (using @model dynamic)</span></span>
3. <span data-ttu-id="02a7b-109">Verwenden die ViewBag-Klasse</span><span class="sxs-lookup"><span data-stu-id="02a7b-109">Using the ViewBag</span></span>

<span data-ttu-id="02a7b-110">Ich habe eine einfache MVC 3 oben Blog-Anwendung zum Vergleichen und gegenüberstellen von dynamischen und stark typisierte Ansichten geschrieben.</span><span class="sxs-lookup"><span data-stu-id="02a7b-110">I've written a simple MVC 3 Top Blog application to compare and contrast dynamic and strongly typed views.</span></span> <span data-ttu-id="02a7b-111">Der Controller beginnt mit einer einfachen Liste mit Blogs zu:</span><span class="sxs-lookup"><span data-stu-id="02a7b-111">The controller starts out with a simple list of blogs:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

<span data-ttu-id="02a7b-112">Klicken Sie mit der rechten Maustaste in die IndexNotStonglyTyped()-Methode, und fügen Sie eine Razor-Ansicht.</span><span class="sxs-lookup"><span data-stu-id="02a7b-112">Right click in the IndexNotStonglyTyped() method and add a Razor view.</span></span>

<span data-ttu-id="02a7b-113">[![8475.NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="02a7b-113">[![8475.NotStronglyTypedView[1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span></span>

<span data-ttu-id="02a7b-114">Stellen Sie sicher, dass die **eine stark typisierte Ansicht erstellen** Kontrollkästchen nicht aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="02a7b-114">Make sure the **Create a strongly-typed view** box is not checked.</span></span> <span data-ttu-id="02a7b-115">Die resultierende Ansicht enthalten nicht viel:</span><span class="sxs-lookup"><span data-stu-id="02a7b-115">The resulting view doesn't contain much:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

<span data-ttu-id="02a7b-116">Da wir ein dynamisches und nicht für eine stark typisierte Ansicht verwenden, nicht in Intellisense uns helfen.</span><span class="sxs-lookup"><span data-stu-id="02a7b-116">Because we're using a dynamic and not a strongly typed view, intellisense doesn't help us.</span></span> <span data-ttu-id="02a7b-117">Der vollständige Code wird unten gezeigt:</span><span class="sxs-lookup"><span data-stu-id="02a7b-117">The completed code is shown below:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

<span data-ttu-id="02a7b-118">[![6646.NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="02a7b-118">[![6646.NotStronglyTypedView_5F00_IE[1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span></span>

<span data-ttu-id="02a7b-119">Jetzt fügen wir eine stark typisierte Ansicht.</span><span class="sxs-lookup"><span data-stu-id="02a7b-119">Now we'll add a strongly typed view.</span></span> <span data-ttu-id="02a7b-120">Fügen Sie den folgenden Code, mit dem Controller:</span><span class="sxs-lookup"><span data-stu-id="02a7b-120">Add the following code to the controller:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]


<span data-ttu-id="02a7b-121">Beachten Sie, dass sie genau die gleichen return View(topBlogs) ist. Rufen Sie als nicht stark typisierten Ansicht.</span><span class="sxs-lookup"><span data-stu-id="02a7b-121">Notice it's exactly the same return View(topBlogs); call as the non-strongly typed view.</span></span> <span data-ttu-id="02a7b-122">Klicken Sie mit der rechten Maustaste innerhalb des *StonglyTypedIndex()* , und wählen Sie **Ansicht hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="02a7b-122">Right click inside of *StonglyTypedIndex()* and select **Add View**.</span></span> <span data-ttu-id="02a7b-123">Wählen Sie dieses Mal die **Blog** Modellklasse, und wählen Sie **Liste** als Gerüst Vorlage.</span><span class="sxs-lookup"><span data-stu-id="02a7b-123">This time select the **Blog** Model class and select **List** as the Scaffold template.</span></span>

<span data-ttu-id="02a7b-124">[![5658.StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="02a7b-124">[![5658.StrongView[1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span></span>

<span data-ttu-id="02a7b-125">In der neuen ansichtsvorlage erhalten wir Intellisense-Unterstützung.</span><span class="sxs-lookup"><span data-stu-id="02a7b-125">Inside the new view template we get intellisense support.</span></span>

<span data-ttu-id="02a7b-126">[![7002.intellesince [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="02a7b-126">[![7002.intellesince[1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span></span>

<span data-ttu-id="02a7b-127">C#-Projekt kann heruntergeladen werden [hier](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span><span class="sxs-lookup"><span data-stu-id="02a7b-127">The c# project can be downloaded [here](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span></span>
