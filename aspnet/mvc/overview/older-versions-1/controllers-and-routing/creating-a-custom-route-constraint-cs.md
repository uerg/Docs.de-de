---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
title: Erstellen eine benutzerdefinierte Route-Einschränkung (c#) | Microsoft Docs
author: StephenWalther
description: Stephen Walther wird veranschaulicht, wie Sie eine benutzerdefinierte routeneinschränkung erstellen können. Implementieren wir eine einfache benutzerdefinierte Einschränkung, die verhindert, dass eine Route wird abgeglichen, w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: a4f4bf4e-abcc-4650-8f43-527e48b52fe6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: 4c120a102b117433b6774f2ea7800f1c4a609f8b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-custom-route-constraint-c"></a><span data-ttu-id="8e67a-104">Erstellen eine benutzerdefinierte Route-Einschränkung (c#)</span><span class="sxs-lookup"><span data-stu-id="8e67a-104">Creating a Custom Route Constraint (C#)</span></span>
====================
<span data-ttu-id="8e67a-105">durch [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="8e67a-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="8e67a-106">Stephen Walther wird veranschaulicht, wie Sie eine benutzerdefinierte routeneinschränkung erstellen können.</span><span class="sxs-lookup"><span data-stu-id="8e67a-106">Stephen Walther demonstrates how you can create a custom route constraint.</span></span> <span data-ttu-id="8e67a-107">Implementieren wir eine einfache benutzerdefinierte Einschränkung, die verhindert, dass eine Route abgeglichen wird, wenn der Browser von einem Remotecomputer angefordert werden.</span><span class="sxs-lookup"><span data-stu-id="8e67a-107">We implement a simple custom constraint that prevents a route from being matched when a browser request is made from a remote computer.</span></span>


<span data-ttu-id="8e67a-108">Das Ziel dieses Lernprogramms wird veranschaulicht, wie Sie eine benutzerdefinierte routeneinschränkung erstellen können.</span><span class="sxs-lookup"><span data-stu-id="8e67a-108">The goal of this tutorial is to demonstrate how you can create a custom route constraint.</span></span> <span data-ttu-id="8e67a-109">Eine benutzerdefinierte routeneinschränkung können Sie verhindern, dass eine Route abgeglichen wird, es sei denn, eine benutzerdefinierte Bedingung verglichen wird.</span><span class="sxs-lookup"><span data-stu-id="8e67a-109">A custom route constraint enables you to prevent a route from being matched unless some custom condition is matched.</span></span>

<span data-ttu-id="8e67a-110">In diesem Lernprogramm erstellen wir eine routeneinschränkung "localhost".</span><span class="sxs-lookup"><span data-stu-id="8e67a-110">In this tutorial, we create a Localhost route constraint.</span></span> <span data-ttu-id="8e67a-111">Die "localhost" routeneinschränkung entspricht nur Anforderungen, die von dem lokalen Computer.</span><span class="sxs-lookup"><span data-stu-id="8e67a-111">The Localhost route constraint only matches requests made from the local computer.</span></span> <span data-ttu-id="8e67a-112">Remoteanforderungen aus über das Internet werden nicht verglichen.</span><span class="sxs-lookup"><span data-stu-id="8e67a-112">Remote requests from across the Internet are not matched.</span></span>

<span data-ttu-id="8e67a-113">Implementieren Sie eine benutzerdefinierte routeneinschränkung durch Implementieren der Schnittstelle IRouteConstraint.</span><span class="sxs-lookup"><span data-stu-id="8e67a-113">You implement a custom route constraint by implementing the IRouteConstraint interface.</span></span> <span data-ttu-id="8e67a-114">Hierbei handelt es sich um eine sehr einfache Schnittstelle die eine einzelne Methode beschreibt:</span><span class="sxs-lookup"><span data-stu-id="8e67a-114">This is an extremely simple interface which describes a single method:</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample1.cs)]

<span data-ttu-id="8e67a-115">Die Methode gibt einen booleschen Wert zurück.</span><span class="sxs-lookup"><span data-stu-id="8e67a-115">The method returns a Boolean value.</span></span> <span data-ttu-id="8e67a-116">Wenn Sie auf "false" zurückgeben, überein nicht die Route mit der Einschränkung verknüpft sind die Browseranforderung.</span><span class="sxs-lookup"><span data-stu-id="8e67a-116">If you return false, the route associated with the constraint won't match the browser request.</span></span>

<span data-ttu-id="8e67a-117">Die Einschränkung "localhost" ist im Codebeispiel 1 enthalten.</span><span class="sxs-lookup"><span data-stu-id="8e67a-117">The Localhost constraint is contained in Listing 1.</span></span>

<span data-ttu-id="8e67a-118">**1 – LocalhostConstraint.cs auflisten**</span><span class="sxs-lookup"><span data-stu-id="8e67a-118">**Listing 1 - LocalhostConstraint.cs**</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample2.cs)]

<span data-ttu-id="8e67a-119">Die Einschränkung in der Liste 1 nutzt die IsLocal-Eigenschaft, die von der HttpRequest-Klasse verfügbar gemacht werden.</span><span class="sxs-lookup"><span data-stu-id="8e67a-119">The constraint in Listing 1 takes advantage of the IsLocal property exposed by the HttpRequest class.</span></span> <span data-ttu-id="8e67a-120">Diese Eigenschaft gibt "true" zurück, wenn die IP-Adresse der Anforderung entweder 127.0.0.1 ist oder wenn die IP-Adresse der Anforderung mit der IP-Adresse des Servers übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="8e67a-120">This property returns true when the IP address of the request is either 127.0.0.1 or when the IP of the request is the same as the server's IP address.</span></span>

<span data-ttu-id="8e67a-121">Sie verwenden eine benutzerdefinierte Einschränkung in einer Route, die in der Datei "Global.asax" definiert.</span><span class="sxs-lookup"><span data-stu-id="8e67a-121">You use a custom constraint within a route defined in the Global.asax file.</span></span> <span data-ttu-id="8e67a-122">Die Datei "Global.asax" in der Liste 2 verwendet die Einschränkung "localhost" um zu verhindern, dass jeder Benutzer eine Seite "Admin" angefordert, es sei denn, sie stellen Sie die Anforderung vom lokalen Server.</span><span class="sxs-lookup"><span data-stu-id="8e67a-122">The Global.asax file in Listing 2 uses the Localhost constraint to prevent anyone from requesting an Admin page unless they make the request from the local server.</span></span> <span data-ttu-id="8e67a-123">Eine Anforderung für /Admin/DeleteAll schlägt beispielsweise fehl, wenn von einem Remoteserver hergestellt.</span><span class="sxs-lookup"><span data-stu-id="8e67a-123">For example, a request for /Admin/DeleteAll will fail when made from a remote server.</span></span>

<span data-ttu-id="8e67a-124">**Auflisten von 2 – "Global.asax"**</span><span class="sxs-lookup"><span data-stu-id="8e67a-124">**Listing 2 - Global.asax**</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample3.cs)]

<span data-ttu-id="8e67a-125">Die Einschränkung "localhost" wird in der Definition der Admin-Route verwendet.</span><span class="sxs-lookup"><span data-stu-id="8e67a-125">The Localhost constraint is used in the definition of the Admin route.</span></span> <span data-ttu-id="8e67a-126">Diese Route wird nicht von einer remote-Browser-Anforderung zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="8e67a-126">This route won't be matched by a remote browser request.</span></span> <span data-ttu-id="8e67a-127">Beachten Sie jedoch, dass andere Routen in Global.asax definierten u. u. mit derselben Anforderung übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="8e67a-127">Realize, however, that other routes defined in Global.asax might match the same request.</span></span> <span data-ttu-id="8e67a-128">Es ist wichtig zu verstehen, dass eine Einschränkung verhindert, dass eine bestimmte Route eine Anforderung gefunden und nicht alle Routen in der Datei "Global.asax" definiert.</span><span class="sxs-lookup"><span data-stu-id="8e67a-128">It is important to understand that a constraint prevents a particular route from matching a request and not all routes defined in the Global.asax file.</span></span>

<span data-ttu-id="8e67a-129">Beachten Sie, dass die Standardroute aus der Datei "Global.asax" auflisten 2 auskommentiert worden ist.</span><span class="sxs-lookup"><span data-stu-id="8e67a-129">Notice that the Default route has been commented out from the Global.asax file in Listing 2.</span></span> <span data-ttu-id="8e67a-130">Wenn Sie die Standardroute einschließen, entsprechen die Standardroute Anforderungen für den Admin-Controller.</span><span class="sxs-lookup"><span data-stu-id="8e67a-130">If you include the Default route, then the Default route would match requests for the Admin controller.</span></span> <span data-ttu-id="8e67a-131">In diesem Fall aufrufen Remotebenutzer, obwohl ihre Anforderungen die Admin-Route übereinstimmen würde nicht weiterhin Aktionen des Admin-Controllers.</span><span class="sxs-lookup"><span data-stu-id="8e67a-131">In that case, remote users could still invoke actions of the Admin controller even though their requests wouldn't match the Admin route.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8e67a-132">[Zurück](creating-a-route-constraint-cs.md)
> [Weiter](asp-net-mvc-controller-overview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="8e67a-132">[Previous](creating-a-route-constraint-cs.md)
[Next](asp-net-mvc-controller-overview-vb.md)</span></span>
