---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: Dynamische V. Stark typisierte Ansichten | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2011
ms.topic: article
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 8a96d43e04a0a50d5176c10c26aa49918a0e56ef
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "26504089"
---
<a name="dynamic-v-strongly-typed-views"></a><span data-ttu-id="f61ae-103">Dynamische V.</span><span class="sxs-lookup"><span data-stu-id="f61ae-103">Dynamic v.</span></span> <span data-ttu-id="f61ae-104">Stark typisierte Ansichten</span><span class="sxs-lookup"><span data-stu-id="f61ae-104">Strongly Typed Views</span></span>
====================
<span data-ttu-id="f61ae-105">durch [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="f61ae-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

<span data-ttu-id="f61ae-106">Es gibt drei Möglichkeiten, um Informationen von einem Controller in ASP.NET MVC 3 übergeben:</span><span class="sxs-lookup"><span data-stu-id="f61ae-106">There are three ways to pass information from a controller to a view in ASP.NET MVC 3:</span></span>

1. <span data-ttu-id="f61ae-107">Als stark typisierte Model-Objekts.</span><span class="sxs-lookup"><span data-stu-id="f61ae-107">As a strongly typed model object.</span></span>
2. <span data-ttu-id="f61ae-108">Als ein dynamischer Typ (mit @model dynamische)</span><span class="sxs-lookup"><span data-stu-id="f61ae-108">As a dynamic type (using @model dynamic)</span></span>
3. <span data-ttu-id="f61ae-109">Verwenden das ViewBag-Objekt</span><span class="sxs-lookup"><span data-stu-id="f61ae-109">Using the ViewBag</span></span>

<span data-ttu-id="f61ae-110">Ich habe eine einfache MVC 3 oben Blog-Anwendung zu vergleichen und vergleichen Sie dynamische und stark typisierte Ansichten geschrieben.</span><span class="sxs-lookup"><span data-stu-id="f61ae-110">I've written a simple MVC 3 Top Blog application to compare and contrast dynamic and strongly typed views.</span></span> <span data-ttu-id="f61ae-111">Der Controller beginnt mit einer einfachen Liste von Blogs:</span><span class="sxs-lookup"><span data-stu-id="f61ae-111">The controller starts out with a simple list of blogs:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

<span data-ttu-id="f61ae-112">Klicken Sie mit der mit der rechten Maustaste in der IndexNotStonglyTyped()-Methode, und fügen Sie einer Razor-Ansicht.</span><span class="sxs-lookup"><span data-stu-id="f61ae-112">Right click in the IndexNotStonglyTyped() method and add a Razor view.</span></span>

<span data-ttu-id="f61ae-113">[![8475.NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f61ae-113">[![8475.NotStronglyTypedView[1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span></span>

<span data-ttu-id="f61ae-114">Stellen Sie sicher, dass die **eine stark typisierte Ansicht erstellen** Kontrollkästchen nicht aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="f61ae-114">Make sure the **Create a strongly-typed view** box is not checked.</span></span> <span data-ttu-id="f61ae-115">Die Ansicht enthalten nicht viel:</span><span class="sxs-lookup"><span data-stu-id="f61ae-115">The resulting view doesn't contain much:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

<span data-ttu-id="f61ae-116">Da wir einen dynamischen und nicht auf einer stark typisierten Ansicht verwenden, nicht in Intellisense uns helfen.</span><span class="sxs-lookup"><span data-stu-id="f61ae-116">Because we're using a dynamic and not a strongly typed view, intellisense doesn't help us.</span></span> <span data-ttu-id="f61ae-117">Der vollständige Code wird unten gezeigt:</span><span class="sxs-lookup"><span data-stu-id="f61ae-117">The completed code is shown below:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

<span data-ttu-id="f61ae-118">[![6646.NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="f61ae-118">[![6646.NotStronglyTypedView_5F00_IE[1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span></span>

<span data-ttu-id="f61ae-119">Nun fügen wir eine stark typisierte Ansicht.</span><span class="sxs-lookup"><span data-stu-id="f61ae-119">Now we'll add a strongly typed view.</span></span> <span data-ttu-id="f61ae-120">Fügen Sie den folgenden Code an den Controller aus:</span><span class="sxs-lookup"><span data-stu-id="f61ae-120">Add the following code to the controller:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]


<span data-ttu-id="f61ae-121">Beachten Sie, dass sie genau die gleichen return View(topBlogs) ist. Rufen Sie als nicht stark typisierten Ansicht.</span><span class="sxs-lookup"><span data-stu-id="f61ae-121">Notice it's exactly the same return View(topBlogs); call as the non-strongly typed view.</span></span> <span data-ttu-id="f61ae-122">Klicken Sie mit der rechten Maustaste innerhalb des *StonglyTypedIndex()* , und wählen Sie **Ansicht hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="f61ae-122">Right click inside of *StonglyTypedIndex()* and select **Add View**.</span></span> <span data-ttu-id="f61ae-123">Wählen Sie dieses Mal die **Blog** Modellschemas, und wählen Sie **Liste** als Gerüst Vorlage.</span><span class="sxs-lookup"><span data-stu-id="f61ae-123">This time select the **Blog** Model class and select **List** as the Scaffold template.</span></span>

<span data-ttu-id="f61ae-124">[![5658.StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="f61ae-124">[![5658.StrongView[1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span></span>

<span data-ttu-id="f61ae-125">In der neuen Sicht Vorlage erhalten wir die Intellisense-Unterstützung.</span><span class="sxs-lookup"><span data-stu-id="f61ae-125">Inside the new view template we get intellisense support.</span></span>

<span data-ttu-id="f61ae-126">[![7002.intellesince [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="f61ae-126">[![7002.intellesince[1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span></span>

<span data-ttu-id="f61ae-127">C#-Projekt kann heruntergeladen werden [hier](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span><span class="sxs-lookup"><span data-stu-id="f61ae-127">The c# project can be downloaded [here](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span></span>
