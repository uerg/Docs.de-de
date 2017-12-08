---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: "Grundlegendes zu ASP.NET MVC-Ausführungsprozess | Microsoft Docs"
author: microsoft
description: Erfahren Sie, wie das ASP.NET MVC-Framework eine Browseranforderung schrittweise verarbeitet.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 5837c6e49709d6b86ee52cd88ffd4759c1850544
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="understanding-the-aspnet-mvc-execution-process"></a><span data-ttu-id="b8560-103">Grundlegendes zu ASP.NET MVC-Ausführungsprozess</span><span class="sxs-lookup"><span data-stu-id="b8560-103">Understanding the ASP.NET MVC Execution Process</span></span>
====================
<span data-ttu-id="b8560-104">durch [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="b8560-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="b8560-105">Erfahren Sie, wie das ASP.NET MVC-Framework eine Browseranforderung schrittweise verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="b8560-105">Learn how the ASP.NET MVC framework processes a browser request step-by-step.</span></span>


<span data-ttu-id="b8560-106">Anforderungen an eine ASP.NET-MVC-basierte Webanwendung zuerst pass-through-die **UrlRoutingModule fest** Objekt, das ein HTTP-Modul ist.</span><span class="sxs-lookup"><span data-stu-id="b8560-106">Requests to an ASP.NET MVC-based Web application first pass through the **UrlRoutingModule** object, which is an HTTP module.</span></span> <span data-ttu-id="b8560-107">Dieses Modul analysiert die Anforderung und führt die Routenauswahl aus.</span><span class="sxs-lookup"><span data-stu-id="b8560-107">This module parses the request and performs route selection.</span></span> <span data-ttu-id="b8560-108">Die **UrlRoutingModule fest** Objekt wählt das erste Routenobjekt aus, das der aktuellen Anforderung übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="b8560-108">The **UrlRoutingModule** object selects the first route object that matches the current request.</span></span> <span data-ttu-id="b8560-109">(Ein Routenobjekt ist eine Klasse, die implementiert **RouteBase**, und ist in der Regel eine Instanz der **Route** Klasse.) Wenn keine übereinstimmende Route gefunden, die **UrlRoutingModule fest** Objekt hat keine Auswirkung, und die Anforderung an die reguläre ASP.NET- oder IIS-Anforderung verarbeitetes zurückgreifen können.</span><span class="sxs-lookup"><span data-stu-id="b8560-109">(A route object is a class that implements **RouteBase**, and is typically an instance of the **Route** class.) If no routes match, the **UrlRoutingModule** object does nothing and lets the request fall back to the regular ASP.NET or IIS request processing.</span></span>

<span data-ttu-id="b8560-110">Aus dem ausgewählten **Route** -Objekt, das **UrlRoutingModule fest** -Objekt abruft der **IRouteHandler** -Objekt, das zugeordnet ist die **Route**Objekt.</span><span class="sxs-lookup"><span data-stu-id="b8560-110">From the selected **Route** object, the **UrlRoutingModule** object obtains the **IRouteHandler** object that is associated with the **Route** object.</span></span> <span data-ttu-id="b8560-111">In der Regel in einer MVC-Anwendung, dies wird jeweils eine Instanz des **MvcRouteHandler**.</span><span class="sxs-lookup"><span data-stu-id="b8560-111">Typically, in an MVC application, this will be an instance of **MvcRouteHandler**.</span></span> <span data-ttu-id="b8560-112">Die **IRouteHandler** Instanz erstellt ein **IHttpHandler** -Objekt und übergibt es die **IHttpContext** Objekt.</span><span class="sxs-lookup"><span data-stu-id="b8560-112">The **IRouteHandler** instance creates an **IHttpHandler** object and passes it the **IHttpContext** object.</span></span> <span data-ttu-id="b8560-113">Wird standardmäßig die **IHttpHandler** -Instanz für MVC ist der **MvcHandler** Objekt.</span><span class="sxs-lookup"><span data-stu-id="b8560-113">By default, the **IHttpHandler** instance for MVC is the **MvcHandler** object.</span></span> <span data-ttu-id="b8560-114">Die **MvcHandler** Objekt wählt dann den Controller, der die Anforderung letztlich behandelt.</span><span class="sxs-lookup"><span data-stu-id="b8560-114">The **MvcHandler** object then selects the controller that will ultimately handle the request.</span></span>

> [!NOTE]
> <span data-ttu-id="b8560-115">Wenn eine ASP.NET-MVC-Webanwendung in IIS 7.0 ausgeführt wird, ist keine Dateinamenerweiterung für MVC-Projekte erforderlich.</span><span class="sxs-lookup"><span data-stu-id="b8560-115">When an ASP.NET MVC Web application runs in IIS 7.0, no file name extension is required for MVC projects.</span></span> <span data-ttu-id="b8560-116">In IIS 6.0 erfordert der Handler jedoch, dass Sie die ISAPI-DLL von ASP.NET die Dateinamenerweiterung .mvc zuordnen.</span><span class="sxs-lookup"><span data-stu-id="b8560-116">However, in IIS 6.0, the handler requires that you map the .mvc file name extension to the ASP.NET ISAPI DLL.</span></span>


<span data-ttu-id="b8560-117">Das Modul und die Ereignishandler sind die Einstiegspunkte für ASP.NET MVC-Framework.</span><span class="sxs-lookup"><span data-stu-id="b8560-117">The module and handler are the entry points to the ASP.NET MVC framework.</span></span> <span data-ttu-id="b8560-118">Sie können die folgenden Aktionen ausführen:</span><span class="sxs-lookup"><span data-stu-id="b8560-118">They perform the following actions:</span></span>

- <span data-ttu-id="b8560-119">Auswählen des richtigen Controllers in einer MVC-Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="b8560-119">Select the appropriate controller in an MVC Web application.</span></span>
- <span data-ttu-id="b8560-120">Abrufen einer bestimmten Controllerinstanz.</span><span class="sxs-lookup"><span data-stu-id="b8560-120">Obtain a specific controller instance.</span></span>
- <span data-ttu-id="b8560-121">Rufen Sie des Controllers **Execute** Methode.</span><span class="sxs-lookup"><span data-stu-id="b8560-121">Call the controller's **Execute** method.</span></span>

<span data-ttu-id="b8560-122">Im folgenden werden die verschiedenen Ausführungsstufen für ein MVC-Webprojekt aufgeführt:</span><span class="sxs-lookup"><span data-stu-id="b8560-122">The following lists the stages of execution for an MVC Web project:</span></span>

- <span data-ttu-id="b8560-123">Empfangen der ersten Anforderung für die Anwendung</span><span class="sxs-lookup"><span data-stu-id="b8560-123">Receive first request for the application</span></span> 

    - <span data-ttu-id="b8560-124">In der Datei "Global.asax" **Route** Objekte werden hinzugefügt, um die **RouteTable** Objekt.</span><span class="sxs-lookup"><span data-stu-id="b8560-124">In the Global.asax file, **Route** objects are added to the **RouteTable** object.</span></span>
- <span data-ttu-id="b8560-125">Routing ausführen</span><span class="sxs-lookup"><span data-stu-id="b8560-125">Perform routing</span></span> 

    - <span data-ttu-id="b8560-126">Die **UrlRoutingModule fest** -Modul verwendet die erste übereinstimmende **Route** Objekt in der **RouteTable** Auflistung zum Erstellen der **RouteData** -Objekt, das es dann zum Erstellen verwendet einer **RequestContext** (**IHttpContext**) Objekt.</span><span class="sxs-lookup"><span data-stu-id="b8560-126">The **UrlRoutingModule** module uses the first matching **Route** object in the **RouteTable** collection to create the **RouteData** object, which it then uses to create a **RequestContext** (**IHttpContext**) object.</span></span>
- <span data-ttu-id="b8560-127">Erstellen von MVC-anforderungshandlers</span><span class="sxs-lookup"><span data-stu-id="b8560-127">Create MVC request handler</span></span> 

    - <span data-ttu-id="b8560-128">Die **MvcRouteHandler** Objekt erstellt eine Instanz der **MvcHandler** -Klasse und übergibt sie die **RequestContext** Instanz.</span><span class="sxs-lookup"><span data-stu-id="b8560-128">The **MvcRouteHandler** object creates an instance of the **MvcHandler** class and passes it the **RequestContext** instance.</span></span>
- <span data-ttu-id="b8560-129">Erstellen in der Regel</span><span class="sxs-lookup"><span data-stu-id="b8560-129">Create controller</span></span> 

    - <span data-ttu-id="b8560-130">Die **MvcHandler** -Objekt verwendet die **RequestContext** Instanz zum Identifizieren der **IControllerFactory** Objekt (i. d. r. eine Instanz von der  **DefaultControllerFactory** Klasse) auf die Controllerinstanz erstellt.</span><span class="sxs-lookup"><span data-stu-id="b8560-130">The **MvcHandler** object uses the **RequestContext** instance to identify the **IControllerFactory** object (typically an instance of the **DefaultControllerFactory** class) to create the controller instance with.</span></span>
- <span data-ttu-id="b8560-131">Execute-Controller: die **MvcHandler** -Instanz aufruft, den Controller s **Execute** Methode.</span><span class="sxs-lookup"><span data-stu-id="b8560-131">Execute controller - The **MvcHandler** instance calls the controller s **Execute** method.</span></span> |
- <span data-ttu-id="b8560-132">Aktion aufrufen</span><span class="sxs-lookup"><span data-stu-id="b8560-132">Invoke action</span></span> 

    - <span data-ttu-id="b8560-133">Die meisten Domänencontroller Vererben der **Controller** Basisklasse.</span><span class="sxs-lookup"><span data-stu-id="b8560-133">Most controllers inherit from the **Controller** base class.</span></span> <span data-ttu-id="b8560-134">Für Domänencontroller, auf denen dazu die **ControllerActionInvoker** Objekt, mit dem Controller zugeordnet ist, bestimmt Aktionsmethode der Controllerklasse aufrufen, und ruft diese Methode anschließend.</span><span class="sxs-lookup"><span data-stu-id="b8560-134">For controllers that do so, the **ControllerActionInvoker** object that is associated with the controller determines which action method of the controller class to call, and then calls that method.</span></span>
- <span data-ttu-id="b8560-135">Ausführen des Ergebnisses</span><span class="sxs-lookup"><span data-stu-id="b8560-135">Execute result</span></span> 

    - <span data-ttu-id="b8560-136">Eine typische Aktionsmethode möglicherweise eine Benutzereingabe empfangen, bereiten Sie die entsprechenden Antwortdaten vor und führen Sie dann das Ergebnis, indem ein Ergebnistyp zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="b8560-136">A typical action method might receive user input, prepare the appropriate response data, and then execute the result by returning a result type.</span></span> <span data-ttu-id="b8560-137">Die integrierten Ergebnistypen, die ausgeführt werden können, umfassen Folgendes: **ViewResult** (die rendert eine Ansicht und ist der häufigsten verwendete Ergebnistyp), **RedirectToRouteResult**,  **RedirectResult**, **ContentResult**, **JsonResult**, und **EmptyResult**.</span><span class="sxs-lookup"><span data-stu-id="b8560-137">The built-in result types that can be executed include the following: **ViewResult** (which renders a view and is the most-often used result type), **RedirectToRouteResult**, **RedirectResult**, **ContentResult**, **JsonResult**, and **EmptyResult**.</span></span>
