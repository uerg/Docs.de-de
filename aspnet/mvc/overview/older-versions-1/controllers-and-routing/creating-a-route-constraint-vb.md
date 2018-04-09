---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
title: Erstellen einer Routeneinschränkung (VB) | Microsoft Docs
author: StephenWalther
description: In diesem Lernprogramm wird Stephen Walther veranschaulicht, wie Sie steuern können, wie Browser Übereinstimmung Routen anfordert, indem routeneinschränkungen mit regulären Ausdrücken erstellen.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: b7cce113-c82c-45bf-b97b-357e5d9f7f56
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 2f50b371ac679218b06c4848e6d33516d29d3a82
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-route-constraint-vb"></a><span data-ttu-id="e8d75-103">Erstellen einer Routeneinschränkung (VB)</span><span class="sxs-lookup"><span data-stu-id="e8d75-103">Creating a Route Constraint (VB)</span></span>
====================
<span data-ttu-id="e8d75-104">durch [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="e8d75-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="e8d75-105">In diesem Lernprogramm wird Stephen Walther veranschaulicht, wie Sie steuern können, wie Browser Übereinstimmung Routen anfordert, indem routeneinschränkungen mit regulären Ausdrücken erstellen.</span><span class="sxs-lookup"><span data-stu-id="e8d75-105">In this tutorial, Stephen Walther demonstrates how you can control how browser requests match routes by creating route constraints with regular expressions.</span></span>


<span data-ttu-id="e8d75-106">Verwenden Sie routeneinschränkungen, um die Browseranforderungen einzuschränken, die mit eine bestimmte Route übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="e8d75-106">You use route constraints to restrict the browser requests that match a particular route.</span></span> <span data-ttu-id="e8d75-107">Einen regulären Ausdruck können Sie eine routeneinschränkung angeben.</span><span class="sxs-lookup"><span data-stu-id="e8d75-107">You can use a regular expression to specify a route constraint.</span></span>

<span data-ttu-id="e8d75-108">Angenommen Sie, dass Sie die Route in Codebeispiel 1 in der Datei "Global.asax" definiert haben.</span><span class="sxs-lookup"><span data-stu-id="e8d75-108">For example, imagine that you have defined the route in Listing 1 in your Global.asax file.</span></span>

<span data-ttu-id="e8d75-109">**Listing 1 - Global.asax.vb**</span><span class="sxs-lookup"><span data-stu-id="e8d75-109">**Listing 1 - Global.asax.vb**</span></span>

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample1.vb)]

<span data-ttu-id="e8d75-110">Auflisten von 1 enthält eine Route mit dem Namen Product.</span><span class="sxs-lookup"><span data-stu-id="e8d75-110">Listing 1 contains a route named Product.</span></span> <span data-ttu-id="e8d75-111">Die Produkt-Route können Sie die ProductController in auflisten 2 enthaltene Browseranforderungen zuordnen.</span><span class="sxs-lookup"><span data-stu-id="e8d75-111">You can use the Product route to map browser requests to the ProductController contained in Listing 2.</span></span>

<span data-ttu-id="e8d75-112">**Auflisten von 2 – Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="e8d75-112">**Listing 2 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample2.vb)]

<span data-ttu-id="e8d75-113">Beachten Sie, dass die Details()-Aktion, die von der Produkt-Controller verfügbar gemacht werden, einen einzelnen Parameter mit dem Namen "ProductID" akzeptiert.</span><span class="sxs-lookup"><span data-stu-id="e8d75-113">Notice that the Details() action exposed by the Product controller accepts a single parameter named productId.</span></span> <span data-ttu-id="e8d75-114">Dieser Parameter ist ein Integer-Parameter.</span><span class="sxs-lookup"><span data-stu-id="e8d75-114">This parameter is an integer parameter.</span></span>

<span data-ttu-id="e8d75-115">Im Codebeispiel 1 definierten Route entspricht eine der folgenden URLs:</span><span class="sxs-lookup"><span data-stu-id="e8d75-115">The route defined in Listing 1 will match any of the following URLs:</span></span>

- <span data-ttu-id="e8d75-116">/Product/23</span><span class="sxs-lookup"><span data-stu-id="e8d75-116">/Product/23</span></span>
- <span data-ttu-id="e8d75-117">/Product/7</span><span class="sxs-lookup"><span data-stu-id="e8d75-117">/Product/7</span></span>

<span data-ttu-id="e8d75-118">Leider wird die Route auch die folgenden URLs übereinstimmen:</span><span class="sxs-lookup"><span data-stu-id="e8d75-118">Unfortunately, the route will also match the following URLs:</span></span>

- <span data-ttu-id="e8d75-119">/Product/blah</span><span class="sxs-lookup"><span data-stu-id="e8d75-119">/Product/blah</span></span>
- <span data-ttu-id="e8d75-120">/Product/apple</span><span class="sxs-lookup"><span data-stu-id="e8d75-120">/Product/apple</span></span>

<span data-ttu-id="e8d75-121">Da die Details() Aktion einen Ganzzahlparameter erwartet, verursacht der eine Anforderung, die etwas anderes als einen ganzzahligen Wert enthält einen Fehler aus.</span><span class="sxs-lookup"><span data-stu-id="e8d75-121">Because the Details() action expects an integer parameter, making a request that contains something other than an integer value will cause an error.</span></span> <span data-ttu-id="e8d75-122">Beispielsweise bei der Eingabe der URL-/Product/apple in Ihrem Browser erhalten die Seite "Fehler" in Abbildung 1 Sie.</span><span class="sxs-lookup"><span data-stu-id="e8d75-122">For example, if you type the URL /Product/apple into your browser then you will get the error page in Figure 1.</span></span>


<span data-ttu-id="e8d75-123">[![Das Dialogfeld "Neues Projekt"](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e8d75-123">[![The New Project dialog box](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)</span></span>

<span data-ttu-id="e8d75-124">**Abbildung 01**: Anzeigen einer Seite auflösen ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-route-constraint-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="e8d75-124">**Figure 01**: Seeing a page explode ([Click to view full-size image](creating-a-route-constraint-vb/_static/image2.png))</span></span>


<span data-ttu-id="e8d75-125">Was Sie tatsächlich möchten ist nur mit URLs übereinstimmen, die eine richtige ganze Zahl "ProductID" enthalten.</span><span class="sxs-lookup"><span data-stu-id="e8d75-125">What you really want to do is only match URLs that contain a proper integer productId.</span></span> <span data-ttu-id="e8d75-126">Eine Einschränkung können beim Definieren einer Route die URLs einzuschränken, die die Route entsprechen.</span><span class="sxs-lookup"><span data-stu-id="e8d75-126">You can use a constraint when defining a route to restrict the URLs that match the route.</span></span> <span data-ttu-id="e8d75-127">Die geänderte Produkt Route auflisten 3 enthält eine reguläre-Einschränkung, die nur ganze Zahlen übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="e8d75-127">The modified Product route in Listing 3 contains a regular expression constraint that only matches integers.</span></span>

<span data-ttu-id="e8d75-128">**3 – Global.asax.vb auflisten**</span><span class="sxs-lookup"><span data-stu-id="e8d75-128">**Listing 3 - Global.asax.vb**</span></span>

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample3.vb)]

<span data-ttu-id="e8d75-129">Der reguläre Ausdruck \d+ entspricht mindestens einem ganzen Zahlen.</span><span class="sxs-lookup"><span data-stu-id="e8d75-129">The regular expression \d+ matches one or more integers.</span></span> <span data-ttu-id="e8d75-130">Diese Einschränkung bewirkt, dass die Route Produkt entsprechend den folgenden URLs:</span><span class="sxs-lookup"><span data-stu-id="e8d75-130">This constraint causes the Product route to match the following URLs:</span></span>

- <span data-ttu-id="e8d75-131">/Product/3</span><span class="sxs-lookup"><span data-stu-id="e8d75-131">/Product/3</span></span>
- <span data-ttu-id="e8d75-132">/Product/8999</span><span class="sxs-lookup"><span data-stu-id="e8d75-132">/Product/8999</span></span>

<span data-ttu-id="e8d75-133">Aber nicht die folgenden URLs:</span><span class="sxs-lookup"><span data-stu-id="e8d75-133">But not the following URLs:</span></span>

- <span data-ttu-id="e8d75-134">/Product/apple</span><span class="sxs-lookup"><span data-stu-id="e8d75-134">/Product/apple</span></span>
- <span data-ttu-id="e8d75-135">/ Produkt</span><span class="sxs-lookup"><span data-stu-id="e8d75-135">/Product</span></span>

<span data-ttu-id="e8d75-136">Diese Browseranforderungen von einer anderen Route verarbeitet werden oder, wenn keine übereinstimmenden Routen vorhanden sind eine *die Ressource konnte nicht gefunden werden* Fehler zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="e8d75-136">These browser requests will be handled by another route or, if there are no matching routes, a *The resource could not be found* error will be returned.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e8d75-137">[Zurück](creating-custom-routes-vb.md)
> [Weiter](creating-a-custom-route-constraint-vb.md)</span><span class="sxs-lookup"><span data-stu-id="e8d75-137">[Previous](creating-custom-routes-vb.md)
[Next](creating-a-custom-route-constraint-vb.md)</span></span>
