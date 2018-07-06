---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
title: Erstellen einer benutzerdefinierten Routeneinschränkung (VB) | Microsoft-Dokumentation
author: StephenWalther
description: Stephen Walther wird veranschaulicht, wie Sie eine benutzerdefinierten routeneinschränkung erstellen können. Implementieren wir ein einfaches benutzerdefiniertes Einschränkung, die verhindert, dass eine Route wird abgeglichen, w...
ms.author: aspnetcontent
ms.date: 02/16/2009
ms.assetid: 892edb27-1cc2-4eaf-8314-dbc2efc6228a
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 72389d11467cbf7baea4cc9452266edb8ab81125
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840467"
---
<a name="creating-a-custom-route-constraint-vb"></a><span data-ttu-id="450fc-104">Erstellen einer benutzerdefinierten Routeneinschränkung (VB)</span><span class="sxs-lookup"><span data-stu-id="450fc-104">Creating a Custom Route Constraint (VB)</span></span>
====================
<span data-ttu-id="450fc-105">durch [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="450fc-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="450fc-106">Stephen Walther wird veranschaulicht, wie Sie eine benutzerdefinierten routeneinschränkung erstellen können.</span><span class="sxs-lookup"><span data-stu-id="450fc-106">Stephen Walther demonstrates how you can create a custom route constraint.</span></span> <span data-ttu-id="450fc-107">Wir implementieren eine einfache benutzerdefinierte Einschränkung, die verhindert, dass eine Route wird eine Übereinstimmung, wenn der Browser von einem Remotecomputer angefordert werden.</span><span class="sxs-lookup"><span data-stu-id="450fc-107">We implement a simple custom constraint that prevents a route from being matched when a browser request is made from a remote computer.</span></span>


<span data-ttu-id="450fc-108">Das Ziel in diesem Tutorial wird veranschaulicht, wie Sie eine benutzerdefinierten routeneinschränkung erstellen können.</span><span class="sxs-lookup"><span data-stu-id="450fc-108">The goal of this tutorial is to demonstrate how you can create a custom route constraint.</span></span> <span data-ttu-id="450fc-109">Eine benutzerdefinierte Route-Einschränkung können Sie verhindern, dass eine Route abgeglichen wird, es sei denn, eine benutzerdefinierte Bedingung erfüllt ist.</span><span class="sxs-lookup"><span data-stu-id="450fc-109">A custom route constraint enables you to prevent a route from being matched unless some custom condition is matched.</span></span>

<span data-ttu-id="450fc-110">In diesem Tutorial erstellen wir eine routeneinschränkung "localhost".</span><span class="sxs-lookup"><span data-stu-id="450fc-110">In this tutorial, we create a Localhost route constraint.</span></span> <span data-ttu-id="450fc-111">Die Localhost-routeneinschränkung entspricht nur Anforderungen, die von dem lokalen Computer.</span><span class="sxs-lookup"><span data-stu-id="450fc-111">The Localhost route constraint only matches requests made from the local computer.</span></span> <span data-ttu-id="450fc-112">Remoteanforderungen aus über das Internet werden nicht verglichen.</span><span class="sxs-lookup"><span data-stu-id="450fc-112">Remote requests from across the Internet are not matched.</span></span>

<span data-ttu-id="450fc-113">Sie implementieren eine benutzerdefinierten routeneinschränkung durch Implementieren der IRouteConstraint-Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="450fc-113">You implement a custom route constraint by implementing the IRouteConstraint interface.</span></span> <span data-ttu-id="450fc-114">Dies ist eine sehr einfache Schnittstelle, die eine einzelne Methode beschrieben:</span><span class="sxs-lookup"><span data-stu-id="450fc-114">This is an extremely simple interface which describes a single method:</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample1.vb)]

<span data-ttu-id="450fc-115">Die Methode gibt einen booleschen Wert zurück.</span><span class="sxs-lookup"><span data-stu-id="450fc-115">The method returns a Boolean value.</span></span> <span data-ttu-id="450fc-116">Wenn Sie auf "false" zurückgeben, wird nicht die Route, die mit der Einschränkung verknüpft sind die Browseranforderung übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="450fc-116">If you return False, the route associated with the constraint won't match the browser request.</span></span>

<span data-ttu-id="450fc-117">Die Einschränkung "localhost", die in Codebeispiel 1 enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="450fc-117">The Localhost constraint is contained in Listing 1.</span></span>

<span data-ttu-id="450fc-118">**1 – LocalhostConstraint.vb auflisten**</span><span class="sxs-lookup"><span data-stu-id="450fc-118">**Listing 1 - LocalhostConstraint.vb**</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample2.vb)]

<span data-ttu-id="450fc-119">Die Einschränkung in Codebeispiel 1 nutzt die IsLocal-Eigenschaft, die von den HttpRequest-Klasse verfügbar gemacht werden.</span><span class="sxs-lookup"><span data-stu-id="450fc-119">The constraint in Listing 1 takes advantage of the IsLocal property exposed by the HttpRequest class.</span></span> <span data-ttu-id="450fc-120">Diese Eigenschaft gibt true zurück, wenn die IP-Adresse der Anforderung entweder 127.0.0.1 ist oder die IP-Adresse der Anforderung, die IP-Adresse des Servers identisch ist.</span><span class="sxs-lookup"><span data-stu-id="450fc-120">This property returns true when the IP address of the request is either 127.0.0.1 or when the IP of the request is the same as the server's IP address.</span></span>

<span data-ttu-id="450fc-121">Sie verwenden eine benutzerdefinierte Einschränkung in einer Route, die in der Datei "Global.asax" definiert.</span><span class="sxs-lookup"><span data-stu-id="450fc-121">You use a custom constraint within a route defined in the Global.asax file.</span></span> <span data-ttu-id="450fc-122">Die Datei "Global.asax" in Liste 2 verwendet die Einschränkung "localhost", um zu verhindern, dass jeder Benutzer eine Seite "Administrator" angefordert, es sei denn, sie die Anforderung vom lokalen Server machen.</span><span class="sxs-lookup"><span data-stu-id="450fc-122">The Global.asax file in Listing 2 uses the Localhost constraint to prevent anyone from requesting an Admin page unless they make the request from the local server.</span></span> <span data-ttu-id="450fc-123">Eine Anforderung für /Admin/DeleteAll schlägt beispielsweise fehl, wenn von einem Remoteserver hergestellt.</span><span class="sxs-lookup"><span data-stu-id="450fc-123">For example, a request for /Admin/DeleteAll will fail when made from a remote server.</span></span>

<span data-ttu-id="450fc-124">**Codebeispiel 2 – "Global.asax"**</span><span class="sxs-lookup"><span data-stu-id="450fc-124">**Listing 2 - Global.asax**</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample3.vb)]

<span data-ttu-id="450fc-125">Die Einschränkung "localhost" wird in der Definition der Admin-Route verwendet.</span><span class="sxs-lookup"><span data-stu-id="450fc-125">The Localhost constraint is used in the definition of the Admin route.</span></span> <span data-ttu-id="450fc-126">Diese Route wird nicht von einer remote-Browser-Anforderung zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="450fc-126">This route won't be matched by a remote browser request.</span></span> <span data-ttu-id="450fc-127">Beachten Sie jedoch, dass andere in Global.asax definierten Routen unter Umständen die gleiche Anforderung übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="450fc-127">Realize, however, that other routes defined in Global.asax might match the same request.</span></span> <span data-ttu-id="450fc-128">Es ist wichtig zu verstehen, dass eine Einschränkung verhindert, dass eine bestimmte Route Abgleich eine Anforderung, und nicht alle Routen, die in der Datei "Global.asax" definiert.</span><span class="sxs-lookup"><span data-stu-id="450fc-128">It is important to understand that a constraint prevents a particular route from matching a request and not all routes defined in the Global.asax file.</span></span>

<span data-ttu-id="450fc-129">Beachten Sie, dass die Standardroute aus der Datei "Global.asax" in Liste 2 auskommentiert worden ist.</span><span class="sxs-lookup"><span data-stu-id="450fc-129">Notice that the Default route has been commented out from the Global.asax file in Listing 2.</span></span> <span data-ttu-id="450fc-130">Wenn Sie die Standardroute einschließen, entsprechen die Standardroute Anforderungen für den Admin-Controller.</span><span class="sxs-lookup"><span data-stu-id="450fc-130">If you include the Default route, then the Default route would match requests for the Admin controller.</span></span> <span data-ttu-id="450fc-131">In diesem Fall können Remotebenutzer weiterhin Aktionen des Controllers Admin aufrufen, obwohl ihre Anforderungen die Admin-Route übereinstimmen würde nicht.</span><span class="sxs-lookup"><span data-stu-id="450fc-131">In that case, remote users could still invoke actions of the Admin controller even though their requests wouldn't match the Admin route.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="450fc-132">Vorherige</span><span class="sxs-lookup"><span data-stu-id="450fc-132">Previous</span></span>](creating-a-route-constraint-vb.md)
