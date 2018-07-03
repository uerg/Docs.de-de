---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
title: Erstellen einer Routeneinschränkung (VB) | Microsoft-Dokumentation
author: StephenWalther
description: Stephen Walther wird in diesem Tutorial veranschaulicht, wie Sie steuern können, wie Browser Übereinstimmung Routen anfordert, durch das Erstellen von Einschränkungen mit regulären Ausdrücken.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: b7cce113-c82c-45bf-b97b-357e5d9f7f56
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 74afb5653f01a5291b77da1bc672b362fec118a0
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364030"
---
<a name="creating-a-route-constraint-vb"></a><span data-ttu-id="9efc9-103">Erstellen einer Routeneinschränkung (VB)</span><span class="sxs-lookup"><span data-stu-id="9efc9-103">Creating a Route Constraint (VB)</span></span>
====================
<span data-ttu-id="9efc9-104">durch [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="9efc9-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="9efc9-105">Stephen Walther wird in diesem Tutorial veranschaulicht, wie Sie steuern können, wie Browser Übereinstimmung Routen anfordert, durch das Erstellen von Einschränkungen mit regulären Ausdrücken.</span><span class="sxs-lookup"><span data-stu-id="9efc9-105">In this tutorial, Stephen Walther demonstrates how you can control how browser requests match routes by creating route constraints with regular expressions.</span></span>


<span data-ttu-id="9efc9-106">Sie verwenden die routeneinschränkungen, um die Browseranforderungen zu beschränken, die eine bestimmte Route übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="9efc9-106">You use route constraints to restrict the browser requests that match a particular route.</span></span> <span data-ttu-id="9efc9-107">Sie können einen regulären Ausdruck verwenden, einer routeneinschränkung an.</span><span class="sxs-lookup"><span data-stu-id="9efc9-107">You can use a regular expression to specify a route constraint.</span></span>

<span data-ttu-id="9efc9-108">Angenommen Sie, dass Sie die Route in Codebeispiel 1 in der Datei "Global.asax" definiert haben.</span><span class="sxs-lookup"><span data-stu-id="9efc9-108">For example, imagine that you have defined the route in Listing 1 in your Global.asax file.</span></span>

<span data-ttu-id="9efc9-109">**1 – Global.asax.vb auflisten**</span><span class="sxs-lookup"><span data-stu-id="9efc9-109">**Listing 1 - Global.asax.vb**</span></span>

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample1.vb)]

<span data-ttu-id="9efc9-110">Codebeispiel 1 enthält eine Route mit dem Namen Product.</span><span class="sxs-lookup"><span data-stu-id="9efc9-110">Listing 1 contains a route named Product.</span></span> <span data-ttu-id="9efc9-111">Sie können die Produkt-Route verwenden, Browseranforderungen ProductController in Listing 2 enthaltenen zuordnen.</span><span class="sxs-lookup"><span data-stu-id="9efc9-111">You can use the Product route to map browser requests to the ProductController contained in Listing 2.</span></span>

<span data-ttu-id="9efc9-112">**Codebeispiel 2 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="9efc9-112">**Listing 2 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample2.vb)]

<span data-ttu-id="9efc9-113">Beachten Sie, dass die Details()-Aktion, die von der Produkt-Controller verfügbar gemacht werden, einen einzelnen Parameter mit dem Namen "ProductID" akzeptiert.</span><span class="sxs-lookup"><span data-stu-id="9efc9-113">Notice that the Details() action exposed by the Product controller accepts a single parameter named productId.</span></span> <span data-ttu-id="9efc9-114">Dieser Parameter ist ein Integer-Parameter.</span><span class="sxs-lookup"><span data-stu-id="9efc9-114">This parameter is an integer parameter.</span></span>

<span data-ttu-id="9efc9-115">Die Route definiert, die in Codebeispiel 1 wird eine der folgenden URLs übereinstimmen:</span><span class="sxs-lookup"><span data-stu-id="9efc9-115">The route defined in Listing 1 will match any of the following URLs:</span></span>

- <span data-ttu-id="9efc9-116">/ Produkt/23</span><span class="sxs-lookup"><span data-stu-id="9efc9-116">/Product/23</span></span>
- <span data-ttu-id="9efc9-117">/ Produkt/7</span><span class="sxs-lookup"><span data-stu-id="9efc9-117">/Product/7</span></span>

<span data-ttu-id="9efc9-118">Leider wird die Route auch die folgenden URLs übereinstimmen:</span><span class="sxs-lookup"><span data-stu-id="9efc9-118">Unfortunately, the route will also match the following URLs:</span></span>

- <span data-ttu-id="9efc9-119">/ Produkt/Bla</span><span class="sxs-lookup"><span data-stu-id="9efc9-119">/Product/blah</span></span>
- <span data-ttu-id="9efc9-120">/ Produkt/apple</span><span class="sxs-lookup"><span data-stu-id="9efc9-120">/Product/apple</span></span>

<span data-ttu-id="9efc9-121">Da die Details() Aktion einen ganzzahligen Parameter erwartet, verursacht der eine Anforderung, die etwas anderes als ein ganzzahliger Wert enthält einen Fehler aus.</span><span class="sxs-lookup"><span data-stu-id="9efc9-121">Because the Details() action expects an integer parameter, making a request that contains something other than an integer value will cause an error.</span></span> <span data-ttu-id="9efc9-122">Z. B. Wenn Sie die URL-/Product/apple in Ihren Browser eingeben erhalten die Fehlerseite in Abbildung 1 Sie.</span><span class="sxs-lookup"><span data-stu-id="9efc9-122">For example, if you type the URL /Product/apple into your browser then you will get the error page in Figure 1.</span></span>


<span data-ttu-id="9efc9-123">[![Das Dialogfeld "Neues Projekt"](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9efc9-123">[![The New Project dialog box](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)</span></span>

<span data-ttu-id="9efc9-124">**Abbildung 01**: eine Seite explode angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-route-constraint-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="9efc9-124">**Figure 01**: Seeing a page explode ([Click to view full-size image](creating-a-route-constraint-vb/_static/image2.png))</span></span>


<span data-ttu-id="9efc9-125">Was Sie wirklich tun werden nur auf URLs überein, die eine richtige ganze Zahl "ProductID" enthalten.</span><span class="sxs-lookup"><span data-stu-id="9efc9-125">What you really want to do is only match URLs that contain a proper integer productId.</span></span> <span data-ttu-id="9efc9-126">Eine Einschränkung können beim Definieren einer Route zum Einschränken der URLs, die mit die Route übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="9efc9-126">You can use a constraint when defining a route to restrict the URLs that match the route.</span></span> <span data-ttu-id="9efc9-127">Die geänderte Produkt Route in Programmausdruck 3 enthält eine reguläre-Einschränkung, die nur Ganzzahlen entspricht.</span><span class="sxs-lookup"><span data-stu-id="9efc9-127">The modified Product route in Listing 3 contains a regular expression constraint that only matches integers.</span></span>

<span data-ttu-id="9efc9-128">**Codebeispiel 3 - Global.asax.vb**</span><span class="sxs-lookup"><span data-stu-id="9efc9-128">**Listing 3 - Global.asax.vb**</span></span>

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample3.vb)]

<span data-ttu-id="9efc9-129">Der reguläre Ausdruck \d+ entspricht eine oder mehrere Ganzzahlen.</span><span class="sxs-lookup"><span data-stu-id="9efc9-129">The regular expression \d+ matches one or more integers.</span></span> <span data-ttu-id="9efc9-130">Diese Einschränkung bewirkt, dass die Product-Route mit den folgenden URLs übereinstimmen:</span><span class="sxs-lookup"><span data-stu-id="9efc9-130">This constraint causes the Product route to match the following URLs:</span></span>

- <span data-ttu-id="9efc9-131">/ Produkt/3</span><span class="sxs-lookup"><span data-stu-id="9efc9-131">/Product/3</span></span>
- <span data-ttu-id="9efc9-132">/ Produkt/8999</span><span class="sxs-lookup"><span data-stu-id="9efc9-132">/Product/8999</span></span>

<span data-ttu-id="9efc9-133">Aber nicht die folgenden URLs:</span><span class="sxs-lookup"><span data-stu-id="9efc9-133">But not the following URLs:</span></span>

- <span data-ttu-id="9efc9-134">/ Produkt/apple</span><span class="sxs-lookup"><span data-stu-id="9efc9-134">/Product/apple</span></span>
- <span data-ttu-id="9efc9-135">/ Product</span><span class="sxs-lookup"><span data-stu-id="9efc9-135">/Product</span></span>

<span data-ttu-id="9efc9-136">Diese Browseranforderungen von einer anderen Route verarbeitet werden oder, wenn es keine übereinstimmenden Routen sind, eine *die Ressource konnte nicht gefunden werden* Fehler zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="9efc9-136">These browser requests will be handled by another route or, if there are no matching routes, a *The resource could not be found* error will be returned.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9efc9-137">[Zurück](creating-custom-routes-vb.md)
> [Weiter](creating-a-custom-route-constraint-vb.md)</span><span class="sxs-lookup"><span data-stu-id="9efc9-137">[Previous](creating-custom-routes-vb.md)
[Next](creating-a-custom-route-constraint-vb.md)</span></span>
