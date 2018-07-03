---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
title: ASP.NET MVC-Routing – Übersicht (c#) | Microsoft-Dokumentation
author: StephenWalther
description: Stephen Walther wird in diesem Tutorial zeigt, wie ASP.NET MVC-Framework Controlleraktionen Browseranforderungen zugeordnet.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 5b39d2d5-4bf9-4d04-94c7-81b84dfeeb31
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: ff6d7a1540ee7e1de625f692bb5da2c28fdc57f7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388778"
---
<a name="aspnet-mvc-routing-overview-c"></a><span data-ttu-id="fbf0b-103">ASP.NET MVC-Routing – Übersicht (c#)</span><span class="sxs-lookup"><span data-stu-id="fbf0b-103">ASP.NET MVC Routing Overview (C#)</span></span>
====================
<span data-ttu-id="fbf0b-104">durch [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="fbf0b-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="fbf0b-105">Stephen Walther wird in diesem Tutorial zeigt, wie ASP.NET MVC-Framework Controlleraktionen Browseranforderungen zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="fbf0b-105">In this tutorial, Stephen Walther shows how the ASP.NET MVC framework maps browser requests to controller actions.</span></span>


<span data-ttu-id="fbf0b-106">In diesem Tutorial haben Sie eine Einführung in eine wichtige Funktion von jeder ASP.NET MVC-Anwendung namens *ASP.NET-Routing*.</span><span class="sxs-lookup"><span data-stu-id="fbf0b-106">In this tutorial, you are introduced to an important feature of every ASP.NET MVC application called *ASP.NET Routing*.</span></span> <span data-ttu-id="fbf0b-107">Das Modul ASP.NET-Routing ist verantwortlich für die Zuordnung von eingehenden Browseranforderungen zu bestimmten MVC-Controlleraktionen.</span><span class="sxs-lookup"><span data-stu-id="fbf0b-107">The ASP.NET Routing module is responsible for mapping incoming browser requests to particular MVC controller actions.</span></span> <span data-ttu-id="fbf0b-108">Am Ende dieses Lernprogramms werden Sie verstehen, wie die standardmäßige Routingtabelle Controlleraktionen Anforderungen zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="fbf0b-108">By the end of this tutorial, you will understand how the standard route table maps requests to controller actions.</span></span>

## <a name="using-the-default-route-table"></a><span data-ttu-id="fbf0b-109">Mithilfe der Standardroute-Tabelle</span><span class="sxs-lookup"><span data-stu-id="fbf0b-109">Using the Default Route Table</span></span>

<span data-ttu-id="fbf0b-110">Wenn Sie eine neue ASP.NET MVC-Anwendung erstellen, ist die Anwendung bereits konfiguriert, um die Verwendung von ASP.NET-Routing.</span><span class="sxs-lookup"><span data-stu-id="fbf0b-110">When you create a new ASP.NET MVC application, the application is already configured to use ASP.NET Routing.</span></span> <span data-ttu-id="fbf0b-111">ASP.NET-Routing ist Setup an zwei Orten.</span><span class="sxs-lookup"><span data-stu-id="fbf0b-111">ASP.NET Routing is setup in two places.</span></span>

<span data-ttu-id="fbf0b-112">ASP.NET-Routing ist zunächst in Ihrer Anwendung Webkonfigurationsdatei (Web.config-Datei) aktiviert.</span><span class="sxs-lookup"><span data-stu-id="fbf0b-112">First, ASP.NET Routing is enabled in your application's Web configuration file (Web.config file).</span></span> <span data-ttu-id="fbf0b-113">Es gibt vier Abschnitte in der Konfigurationsdatei, die für routing relevant sind: Abschnitt system.web.httpModules, Abschnitt system.web.httpHandlers, Abschnitt system.webserver.modules und Abschnitt system.webserver.handlers.</span><span class="sxs-lookup"><span data-stu-id="fbf0b-113">There are four sections in the configuration file that are relevant to routing: the system.web.httpModules section, the system.web.httpHandlers section, the system.webserver.modules section, and the system.webserver.handlers section.</span></span> <span data-ttu-id="fbf0b-114">Achten Sie darauf, dass Sie nicht in diesen Abschnitten gelöscht werden, da ohne diese Abschnitte routing nicht mehr funktionieren.</span><span class="sxs-lookup"><span data-stu-id="fbf0b-114">Be careful not to delete these sections because without these sections routing will no longer work.</span></span>

<span data-ttu-id="fbf0b-115">Wichtiger ist, und Zweitens wird eine Routingtabelle, in der Datei Global.asax der Anwendung erstellt.</span><span class="sxs-lookup"><span data-stu-id="fbf0b-115">Second, and more importantly, a route table is created in the application's Global.asax file.</span></span> <span data-ttu-id="fbf0b-116">Die Datei "Global.asax" ist eine spezielle Datei, die Ereignishandler für Ereignisse des Anwendungslebenszyklus ASP.NET enthält.</span><span class="sxs-lookup"><span data-stu-id="fbf0b-116">The Global.asax file is a special file that contains event handlers for ASP.NET application lifecycle events.</span></span> <span data-ttu-id="fbf0b-117">Die Routingtabelle wird während des Ereignisses zum Starten der Anwendung erstellt.</span><span class="sxs-lookup"><span data-stu-id="fbf0b-117">The route table is created during the Application Start event.</span></span>

<span data-ttu-id="fbf0b-118">Die Datei in Codebeispiel 1 enthält die Standarddatei "Global.asax" für eine ASP.NET MVC-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="fbf0b-118">The file in Listing 1 contains the default Global.asax file for an ASP.NET MVC application.</span></span>

<span data-ttu-id="fbf0b-119">**Codebeispiel 1 – "Global.asax.cs"**</span><span class="sxs-lookup"><span data-stu-id="fbf0b-119">**Listing 1 - Global.asax.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample1.cs)]

<span data-ttu-id="fbf0b-120">Wenn eine MVC-Anwendung zuerst gestartet wird, die Anwendung\_Start()-Methode wird aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="fbf0b-120">When an MVC application first starts, the Application\_Start() method is called.</span></span> <span data-ttu-id="fbf0b-121">Diese Methode ruft wiederum die RegisterRoutes()-Methode.</span><span class="sxs-lookup"><span data-stu-id="fbf0b-121">This method, in turn, calls the RegisterRoutes() method.</span></span> <span data-ttu-id="fbf0b-122">Die RegisterRoutes()-Methode erstellt die Routentabelle an.</span><span class="sxs-lookup"><span data-stu-id="fbf0b-122">The RegisterRoutes() method creates the route table.</span></span>

<span data-ttu-id="fbf0b-123">Die Standardroutingtabelle enthält eine einzelne Route (mit dem Namen "Default").</span><span class="sxs-lookup"><span data-stu-id="fbf0b-123">The default route table contains a single route (named Default).</span></span> <span data-ttu-id="fbf0b-124">Ordnet die Standardroute das erste Segment der URL einen Controllernamen ein, das zweite Segment einer URL auf eine Controlleraktion durchzuführen und das dritte Segment, das einen Parameter namens **Id**.</span><span class="sxs-lookup"><span data-stu-id="fbf0b-124">The Default route maps the first segment of a URL to a controller name, the second segment of a URL to a controller action, and the third segment to a parameter named **id**.</span></span>

<span data-ttu-id="fbf0b-125">Stellen Sie sich, dass Sie die folgende URL in die Adressleiste des Webbrowsers eingeben:</span><span class="sxs-lookup"><span data-stu-id="fbf0b-125">Imagine that you enter the following URL into your web browser's address bar:</span></span>

<span data-ttu-id="fbf0b-126">/ Home/Index/3</span><span class="sxs-lookup"><span data-stu-id="fbf0b-126">/Home/Index/3</span></span>

<span data-ttu-id="fbf0b-127">Die Standardroute ordnet diese URL die folgenden Parameter:</span><span class="sxs-lookup"><span data-stu-id="fbf0b-127">The Default route maps this URL to the following parameters:</span></span>

- <span data-ttu-id="fbf0b-128">Controller = Home</span><span class="sxs-lookup"><span data-stu-id="fbf0b-128">controller = Home</span></span>

- <span data-ttu-id="fbf0b-129">Aktion = Index</span><span class="sxs-lookup"><span data-stu-id="fbf0b-129">action = Index</span></span>

- <span data-ttu-id="fbf0b-130">id = 3</span><span class="sxs-lookup"><span data-stu-id="fbf0b-130">id = 3</span></span>

<span data-ttu-id="fbf0b-131">Wenn Sie die URL/Home/Index/3 anfordern, wird der folgende Code ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="fbf0b-131">When you request the URL /Home/Index/3, the following code is executed:</span></span>

<span data-ttu-id="fbf0b-132">HomeController.Index(3)</span><span class="sxs-lookup"><span data-stu-id="fbf0b-132">HomeController.Index(3)</span></span>

<span data-ttu-id="fbf0b-133">Die Standardroute enthält Standardwerte für alle drei Parameter.</span><span class="sxs-lookup"><span data-stu-id="fbf0b-133">The Default route includes defaults for all three parameters.</span></span> <span data-ttu-id="fbf0b-134">Wenn Sie einen Controller nicht bereitgestellt wird, klicken Sie dann der Controller Parameter standardmäßig auf den Wert **Startseite**.</span><span class="sxs-lookup"><span data-stu-id="fbf0b-134">If you don't supply a controller, then the controller parameter defaults to the value **Home**.</span></span> <span data-ttu-id="fbf0b-135">Wenn Sie nicht, dass eine Aktion angeben, die Action-Parameter nimmt standardmäßig den Wert **Index**.</span><span class="sxs-lookup"><span data-stu-id="fbf0b-135">If you don't supply an action, the action parameter defaults to the value **Index**.</span></span> <span data-ttu-id="fbf0b-136">Wenn Sie eine Id angeben nicht, standardmäßig der Id-Parameter zum Schluss auf eine leere Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="fbf0b-136">Finally, if you don't supply an id, the id parameter defaults to an empty string.</span></span>

<span data-ttu-id="fbf0b-137">Wir sehen uns einige Beispiele wie die Standardroute Controlleraktionen URLs zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="fbf0b-137">Let's look at a few examples of how the Default route maps URLs to controller actions.</span></span> <span data-ttu-id="fbf0b-138">Stellen Sie sich, dass Sie die folgende URL in die Adressleiste Ihres Browsers eingeben:</span><span class="sxs-lookup"><span data-stu-id="fbf0b-138">Imagine that you enter the following URL into your browser address bar:</span></span>

<span data-ttu-id="fbf0b-139">/ Home</span><span class="sxs-lookup"><span data-stu-id="fbf0b-139">/Home</span></span>

<span data-ttu-id="fbf0b-140">Aufgrund der standardmäßigen Parameter routenstandardwerte wird diese URL eingeben die Index()-Methode der HomeController-Klasse in Liste 2 aufgerufen werden verursachen.</span><span class="sxs-lookup"><span data-stu-id="fbf0b-140">Because of the Default route parameter defaults, entering this URL will cause the Index() method of the HomeController class in Listing 2 to be called.</span></span>

<span data-ttu-id="fbf0b-141">**Codebeispiel 2 - Datei "HomeController.cs"**</span><span class="sxs-lookup"><span data-stu-id="fbf0b-141">**Listing 2 - HomeController.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample2.cs)]

<span data-ttu-id="fbf0b-142">Programmausdruck 2 enthält die HomeController-Klasse eine Methode namens Index(), die einen einzelnen Parameter mit dem Namen Id. akzeptiert Die URL gibt bewirkt, dass die Index()-Methode, die mit einer leeren Zeichenfolge als Wert für den Id-Parameter aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="fbf0b-142">In Listing 2, the HomeController class includes a method named Index() that accepts a single parameter named Id. The URL /Home causes the Index() method to be called with an empty string as the value of the Id parameter.</span></span>

<span data-ttu-id="fbf0b-143">Aufgrund der Art und Weise, dass das MVC-Framework Controlleraktionen aufruft entspricht die URL gibt auch die Index()-Methode der HomeController-Klasse in Programmausdruck 3.</span><span class="sxs-lookup"><span data-stu-id="fbf0b-143">Because of the way that the MVC framework invokes controller actions, the URL /Home also matches the Index() method of the HomeController class in Listing 3.</span></span>

<span data-ttu-id="fbf0b-144">**Codebeispiel 3 - Datei "HomeController.cs" (Index-Aktion ohne Parameter)**</span><span class="sxs-lookup"><span data-stu-id="fbf0b-144">**Listing 3 - HomeController.cs (Index action with no parameter)**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample3.cs)]

<span data-ttu-id="fbf0b-145">Die Methode Index() in Programmausdruck 3 akzeptiert keine Parameter.</span><span class="sxs-lookup"><span data-stu-id="fbf0b-145">The Index() method in Listing 3 does not accept any parameters.</span></span> <span data-ttu-id="fbf0b-146">Die URL gibt, führt dazu, dass diese Index()-Methode aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="fbf0b-146">The URL /Home will cause this Index() method to be called.</span></span> <span data-ttu-id="fbf0b-147">Die URL/Home/Index/3 ruft auch diese Methode (die Id wird ignoriert).</span><span class="sxs-lookup"><span data-stu-id="fbf0b-147">The URL /Home/Index/3 also invokes this method (the Id is ignored).</span></span>

<span data-ttu-id="fbf0b-148">Die URL gibt entspricht auch die Index()-Methode der HomeController-Klasse in Listing 4.</span><span class="sxs-lookup"><span data-stu-id="fbf0b-148">The URL /Home also matches the Index() method of the HomeController class in Listing 4.</span></span>

<span data-ttu-id="fbf0b-149">**Programmausdruck 4 - Datei "HomeController.cs" (Index-Aktion mit NULL-Werte zulassen-Parameter)**</span><span class="sxs-lookup"><span data-stu-id="fbf0b-149">**Listing 4 - HomeController.cs (Index action with nullable parameter)**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample4.cs)]

<span data-ttu-id="fbf0b-150">In Listing 4 hat die Index()-Methode einen ganzzahligen Parameter an.</span><span class="sxs-lookup"><span data-stu-id="fbf0b-150">In Listing 4, the Index() method has one Integer parameter.</span></span> <span data-ttu-id="fbf0b-151">Da der Parameter ein NULL-Werte zulassen-Parameter ist (es kann sich um den Wert Null haben), kann die Index() aufgerufen werden, ohne dass ein Fehler ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="fbf0b-151">Because the parameter is a nullable parameter (can have the value Null), the Index() can be called without raising an error.</span></span>

<span data-ttu-id="fbf0b-152">Zum Schluss Aufrufen der Methode Index() in Listing 5 mit der URL gibt führt dazu, dass eine Ausnahme aus, da der Id-Parameter *nicht* einen Parameter NULL-Werte zulässt.</span><span class="sxs-lookup"><span data-stu-id="fbf0b-152">Finally, invoking the Index() method in Listing 5 with the URL /Home causes an exception since the Id parameter *is not* a nullable parameter.</span></span> <span data-ttu-id="fbf0b-153">Wenn Sie versuchen, das Aufrufen der Methode Index() erhalten Sie den Fehler, die in Abbildung 1 angezeigt.</span><span class="sxs-lookup"><span data-stu-id="fbf0b-153">If you attempt to invoke the Index() method then you get the error displayed in Figure 1.</span></span>

<span data-ttu-id="fbf0b-154">**Programmausdruck 5 - Datei "HomeController.cs" (Index-Aktion mit Id-Parameter)**</span><span class="sxs-lookup"><span data-stu-id="fbf0b-154">**Listing 5 - HomeController.cs (Index action with Id parameter)**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample5.cs)]


<span data-ttu-id="fbf0b-155">[![Eine Controlleraktion aufgerufen wird, die einen Parameterwert erwartet](asp-net-mvc-routing-overview-cs/_static/image1.jpg)](asp-net-mvc-routing-overview-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="fbf0b-155">[![Invoking a controller action that expects a parameter value](asp-net-mvc-routing-overview-cs/_static/image1.jpg)](asp-net-mvc-routing-overview-cs/_static/image1.png)</span></span>

<span data-ttu-id="fbf0b-156">**Abbildung 01**: eine Controlleraktion, die einen Parameterwert erwartet aufrufen ([klicken Sie, um das Bild in voller Größe anzeigen](asp-net-mvc-routing-overview-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="fbf0b-156">**Figure 01**: Invoking a controller action that expects a parameter value ([Click to view full-size image](asp-net-mvc-routing-overview-cs/_static/image2.png))</span></span>


<span data-ttu-id="fbf0b-157">Die URL/Home/Index/3, problemlos auf der anderen Seite mit die Index-Controlleraktion in Listing 5.</span><span class="sxs-lookup"><span data-stu-id="fbf0b-157">The URL /Home/Index/3, on the other hand, works just fine with the Index controller action in Listing 5.</span></span> <span data-ttu-id="fbf0b-158">Die Anforderung /Home/Index/3 bewirkt, dass die Index()-Methode, die mit einem Id-Parameter aufgerufen werden, die den Wert 3 hat.</span><span class="sxs-lookup"><span data-stu-id="fbf0b-158">The request /Home/Index/3 causes the Index() method to be called with an Id parameter that has the value 3.</span></span>

## <a name="summary"></a><span data-ttu-id="fbf0b-159">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="fbf0b-159">Summary</span></span>

<span data-ttu-id="fbf0b-160">Das Ziel in diesem Tutorial wurde eine kurze Einführung in das ASP.NET-Routing bereit.</span><span class="sxs-lookup"><span data-stu-id="fbf0b-160">The goal of this tutorial was to provide you with a brief introduction to ASP.NET Routing.</span></span> <span data-ttu-id="fbf0b-161">Wir untersucht die Standardroutingtabelle, die Sie durch eine neue ASP.NET MVC-Anwendung zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="fbf0b-161">We examined the default route table that you get with a new ASP.NET MVC application.</span></span> <span data-ttu-id="fbf0b-162">Sie erfahren, wie die Standardroute Controlleraktionen URLs zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="fbf0b-162">You learned how the default route maps URLs to controller actions.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="fbf0b-163">Nächste</span><span class="sxs-lookup"><span data-stu-id="fbf0b-163">Next</span></span>](understanding-action-filters-cs.md)
