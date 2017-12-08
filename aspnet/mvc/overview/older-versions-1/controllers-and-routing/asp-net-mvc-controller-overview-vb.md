---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
title: "Übersicht über das ASP.NET MVC-Controller (VB) | Microsoft Docs"
author: StephenWalther
description: "In diesem Lernprogramm führt Sie Stephen Walther in ASP.NET MVC-Controller. Erfahren Sie, wie zum Erstellen von neuen Controller und Aktion Res verschiedene Datentypen zurückgeben..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: 94c3e5d9-a904-445e-a34e-d92fd1ca108a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: 452e2cf771e8b1f298ce9693ec2a707e7c1d4dd1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-controller-overview-vb"></a><span data-ttu-id="dec72-104">Übersicht über das ASP.NET MVC-Controller (VB)</span><span class="sxs-lookup"><span data-stu-id="dec72-104">ASP.NET MVC Controller Overview (VB)</span></span>
====================
<span data-ttu-id="dec72-105">durch [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="dec72-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="dec72-106">In diesem Lernprogramm führt Sie Stephen Walther in ASP.NET MVC-Controller.</span><span class="sxs-lookup"><span data-stu-id="dec72-106">In this tutorial, Stephen Walther introduces you to ASP.NET MVC controllers.</span></span> <span data-ttu-id="dec72-107">Erfahren Sie, wie neue Domänencontroller zu erstellen und unterschiedliche Rückgabetypen von Aktionsergebnisse.</span><span class="sxs-lookup"><span data-stu-id="dec72-107">You learn how to create new controllers and return different types of action results.</span></span>


<span data-ttu-id="dec72-108">In diesem Lernprogramm wird das Thema der ASP.NET MVC-Controller, Controlleraktionen und Aktionsergebnisse erklärt.</span><span class="sxs-lookup"><span data-stu-id="dec72-108">This tutorial explores the topic of ASP.NET MVC controllers, controller actions, and action results.</span></span> <span data-ttu-id="dec72-109">Nachdem Sie dieses Lernprogramm abgeschlossen haben, werden Sie verstehen, wie Domänencontroller verwendet werden, um zu steuern, die ein Besucher mit einer ASP.NET MVC-Website interagiert.</span><span class="sxs-lookup"><span data-stu-id="dec72-109">After you complete this tutorial, you will understand how controllers are used to control the way a visitor interacts with an ASP.NET MVC website.</span></span>

## <a name="understanding-controllers"></a><span data-ttu-id="dec72-110">Grundlegendes zu Controllern</span><span class="sxs-lookup"><span data-stu-id="dec72-110">Understanding Controllers</span></span>

<span data-ttu-id="dec72-111">MVC-Controller sind verantwortlich für die Reaktion auf Anforderungen für eine ASP.NET MVC-Website.</span><span class="sxs-lookup"><span data-stu-id="dec72-111">MVC controllers are responsible for responding to requests made against an ASP.NET MVC website.</span></span> <span data-ttu-id="dec72-112">Jede Browseranforderung wird ein bestimmter Domänencontroller zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="dec72-112">Each browser request is mapped to a particular controller.</span></span> <span data-ttu-id="dec72-113">Angenommen Sie, dass Sie die folgende URL in die Adressleiste des Browsers eingeben:</span><span class="sxs-lookup"><span data-stu-id="dec72-113">For example, imagine that you enter the following URL into the address bar of your browser:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="dec72-114">In diesem Fall wird ein Controller namens ProductController aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="dec72-114">In this case, a controller named ProductController is invoked.</span></span> <span data-ttu-id="dec72-115">Die ProductController ist verantwortlich für die Antwort auf die Browseranforderung zu generieren.</span><span class="sxs-lookup"><span data-stu-id="dec72-115">The ProductController is responsible for generating the response to the browser request.</span></span> <span data-ttu-id="dec72-116">Z. B. der Controller möglicherweise eine bestimmte Ansicht zurück an den Browser zurück, oder der Controller kann den Benutzer weiterleiten, auf einen anderen Controller.</span><span class="sxs-lookup"><span data-stu-id="dec72-116">For example, the controller might return a particular view back to the browser or the controller might redirect the user to another controller.</span></span>

<span data-ttu-id="dec72-117">Auflisten von 1 enthält einen einfachen Controller mit dem Namen ProductController.</span><span class="sxs-lookup"><span data-stu-id="dec72-117">Listing 1 contains a simple controller named ProductController.</span></span>

<span data-ttu-id="dec72-118">**Listing1 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="dec72-118">**Listing1 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample1.vb)]

<span data-ttu-id="dec72-119">Wie Sie im Codebeispiel 1 sehen können, ist ein Controller nur einer Klasse (Visual Basic .NET oder C#-Klasse).</span><span class="sxs-lookup"><span data-stu-id="dec72-119">As you can see from Listing 1, a controller is just a class (a Visual Basic .NET or C# class).</span></span> <span data-ttu-id="dec72-120">Ein Domänencontroller ist eine Klasse, die von der Basisklasse System.Web.Mvc.Controller abgeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="dec72-120">A controller is a class that derives from the base System.Web.Mvc.Controller class.</span></span> <span data-ttu-id="dec72-121">Da es sich bei ein Domänencontroller aus dieser Basisklasse erbt, erbt ein Controller mehrere nützliche Methoden kostenlos (erörtert, diese Methoden in wenigen Augenblicken).</span><span class="sxs-lookup"><span data-stu-id="dec72-121">Because a controller inherits from this base class, a controller inherits several useful methods for free (We discuss these methods in a moment).</span></span>

## <a name="understanding-controller-actions"></a><span data-ttu-id="dec72-122">Grundlegendes zu Controlleraktionen</span><span class="sxs-lookup"><span data-stu-id="dec72-122">Understanding Controller Actions</span></span>

<span data-ttu-id="dec72-123">Ein Controller macht Controlleraktionen verfügbar.</span><span class="sxs-lookup"><span data-stu-id="dec72-123">A controller exposes controller actions.</span></span> <span data-ttu-id="dec72-124">Eine Aktion ist eine Methode auf einem Domänencontroller, der aufgerufen wird, wenn Sie eine bestimmte URL in der Adressleiste des Browsers eingeben.</span><span class="sxs-lookup"><span data-stu-id="dec72-124">An action is a method on a controller that gets called when you enter a particular URL in your browser address bar.</span></span> <span data-ttu-id="dec72-125">Angenommen Sie, dass Sie eine Anforderung für die folgende URL verwenden:</span><span class="sxs-lookup"><span data-stu-id="dec72-125">For example, imagine that you make a request for the following URL:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="dec72-126">In diesem Fall wird die Index()-Methode für die Klasse ProductController aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="dec72-126">In this case, the Index() method is called on the ProductController class.</span></span> <span data-ttu-id="dec72-127">Die Index()-Methode ist ein Beispiel für eine Controlleraktion.</span><span class="sxs-lookup"><span data-stu-id="dec72-127">The Index() method is an example of a controller action.</span></span>

<span data-ttu-id="dec72-128">Eine Controlleraktion muss eine öffentliche Methode einer Controllerklasse sein.</span><span class="sxs-lookup"><span data-stu-id="dec72-128">A controller action must be a public method of a controller class.</span></span> <span data-ttu-id="dec72-129">Visual Basic.NET Methoden in der Standardeinstellung sind die öffentlichen Methoden.</span><span class="sxs-lookup"><span data-stu-id="dec72-129">Visual Basic.NET methods, by default, are public methods.</span></span> <span data-ttu-id="dec72-130">Beachten Sie, dass keine öffentliche Methode, die Sie zu einer Controllerklasse hinzufügen automatisch als eine Controlleraktion bereitgestellt wird (Achten Sie Informationen hierzu, da eine Controlleraktion von einem Benutzer im Universum aufgerufen werden kann, indem Sie einfach die richtige URL in die Adressleiste des Webbrowsers eingeben).</span><span class="sxs-lookup"><span data-stu-id="dec72-130">Realize that any public method that you add to a controller class is exposed as a controller action automatically (You must be careful about this since a controller action can be invoked by anyone in the universe simply by typing the right URL into a browser address bar).</span></span>

<span data-ttu-id="dec72-131">Es gibt einige zusätzlichen Anforderungen, die durch eine Controlleraktion erfüllt werden müssen.</span><span class="sxs-lookup"><span data-stu-id="dec72-131">There are some additional requirements that must be satisfied by a controller action.</span></span> <span data-ttu-id="dec72-132">Eine Methode als eine Controlleraktion verwendet, kann nicht überladen werden.</span><span class="sxs-lookup"><span data-stu-id="dec72-132">A method used as a controller action cannot be overloaded.</span></span> <span data-ttu-id="dec72-133">Darüber hinaus darf keine Controlleraktion eine statische Methode sein.</span><span class="sxs-lookup"><span data-stu-id="dec72-133">Furthermore, a controller action cannot be a static method.</span></span> <span data-ttu-id="dec72-134">Als dem können Sie beliebige andere Methode als eine Controlleraktion.</span><span class="sxs-lookup"><span data-stu-id="dec72-134">Other than that, you can use just about any method as a controller action.</span></span>

## <a name="understanding-action-results"></a><span data-ttu-id="dec72-135">Grundlegendes zu den Ergebnissen der Aktion</span><span class="sxs-lookup"><span data-stu-id="dec72-135">Understanding Action Results</span></span>

<span data-ttu-id="dec72-136">Gibt eine Controlleraktion zurück, so genannte ein *Aktionsergebnis*.</span><span class="sxs-lookup"><span data-stu-id="dec72-136">A controller action returns something called an *action result*.</span></span> <span data-ttu-id="dec72-137">Ein Aktionsergebnis ist, was eine Controlleraktion als Antwort auf eine Browseranforderung zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="dec72-137">An action result is what a controller action returns in response to a browser request.</span></span>

<span data-ttu-id="dec72-138">Das ASP.NET-MVC-Framework unterstützt mehrere Typen von Aktionsergebnisse, einschließlich:</span><span class="sxs-lookup"><span data-stu-id="dec72-138">The ASP.NET MVC framework supports several types of action results including:</span></span>

1. <span data-ttu-id="dec72-139">ViewResult - stellt HTML und Markup.</span><span class="sxs-lookup"><span data-stu-id="dec72-139">ViewResult - Represents HTML and markup.</span></span>
2. <span data-ttu-id="dec72-140">EmptyResult - stellt kein Ergebnis dar.</span><span class="sxs-lookup"><span data-stu-id="dec72-140">EmptyResult - Represents no result.</span></span>
3. <span data-ttu-id="dec72-141">RedirectResult - stellt eine Umleitung an eine neue URL um.</span><span class="sxs-lookup"><span data-stu-id="dec72-141">RedirectResult - Represents a redirection to a new URL.</span></span>
4. <span data-ttu-id="dec72-142">JsonResult - stellt einen JavaScript Object Notation-Ergebnis, das in einer AJAX-Anwendung verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="dec72-142">JsonResult - Represents a JavaScript Object Notation result that can be used in an AJAX application.</span></span>
5. <span data-ttu-id="dec72-143">JavaScriptResult - stellt eine JavaScript-Skript dar.</span><span class="sxs-lookup"><span data-stu-id="dec72-143">JavaScriptResult - Represents a JavaScript script.</span></span>
6. <span data-ttu-id="dec72-144">ContentResult - stellt einen Textergebnis dar.</span><span class="sxs-lookup"><span data-stu-id="dec72-144">ContentResult - Represents a text result.</span></span>
7. <span data-ttu-id="dec72-145">FileContentResult - stellt eine herunterladbare Datei (mit dem binären Inhalt) dar.</span><span class="sxs-lookup"><span data-stu-id="dec72-145">FileContentResult - Represents a downloadable file (with the binary content).</span></span>
8. <span data-ttu-id="dec72-146">FilePathResult - stellt eine herunterladbare Datei (mit einem Pfad).</span><span class="sxs-lookup"><span data-stu-id="dec72-146">FilePathResult - Represents a downloadable file (with a path).</span></span>
9. <span data-ttu-id="dec72-147">FileStreamResult - stellt eine herunterladbare Datei (mit einem Dateistream) dar.</span><span class="sxs-lookup"><span data-stu-id="dec72-147">FileStreamResult - Represents a downloadable file (with a file stream).</span></span>

<span data-ttu-id="dec72-148">Alle diese Aktionsergebnisse erben von der ActionResult-Basisklasse.</span><span class="sxs-lookup"><span data-stu-id="dec72-148">All of these action results inherit from the base ActionResult class.</span></span>

<span data-ttu-id="dec72-149">In den meisten Fällen gibt eine Controlleraktion ein ViewResult zurück.</span><span class="sxs-lookup"><span data-stu-id="dec72-149">In most cases, a controller action returns a ViewResult.</span></span> <span data-ttu-id="dec72-150">Index-Controlleraktion Auflisten von 2 gibt beispielsweise ein ViewResult zurück.</span><span class="sxs-lookup"><span data-stu-id="dec72-150">For example, the Index controller action in Listing 2 returns a ViewResult.</span></span>

<span data-ttu-id="dec72-151">**Auflisten von 2 – Controllers\BookController.vb**</span><span class="sxs-lookup"><span data-stu-id="dec72-151">**Listing 2 - Controllers\BookController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample2.vb)]

<span data-ttu-id="dec72-152">Wenn eine Aktion ein ViewResult zurückgibt, wird HTML an den Browser zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="dec72-152">When an action returns a ViewResult, HTML is returned to the browser.</span></span> <span data-ttu-id="dec72-153">Die Index()-Methode im Codebeispiel 2 Gibt eine Ansicht mit dem Namen Index, an den Browser zurück.</span><span class="sxs-lookup"><span data-stu-id="dec72-153">The Index() method in Listing 2 returns a view named Index to the browser.</span></span>

<span data-ttu-id="dec72-154">Beachten Sie, dass die Aktion Index() Auflisten von 2 eine ViewResult() nicht zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="dec72-154">Notice that the Index() action in Listing 2 does not return a ViewResult().</span></span> <span data-ttu-id="dec72-155">Stattdessen wird die View()-Methode der Basisklasse Controller aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="dec72-155">Instead, the View() method of the Controller base class is called.</span></span> <span data-ttu-id="dec72-156">Normalerweise wird ein Aktionsergebnis nicht direkt zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="dec72-156">Normally, you do not return an action result directly.</span></span> <span data-ttu-id="dec72-157">Stattdessen rufen Sie eine der folgenden Methoden der Basisklasse Controller:</span><span class="sxs-lookup"><span data-stu-id="dec72-157">Instead, you call one of the following methods of the Controller base class:</span></span>

1. <span data-ttu-id="dec72-158">Anzeigen – gibt ein ViewResult-Aktion-Ergebnis zurück.</span><span class="sxs-lookup"><span data-stu-id="dec72-158">View - Returns a ViewResult action result.</span></span>
2. <span data-ttu-id="dec72-159">Umgeleitet – gibt ein RedirectResult-Aktion-Ergebnis zurück.</span><span class="sxs-lookup"><span data-stu-id="dec72-159">Redirect - Returns a RedirectResult action result.</span></span>
3. <span data-ttu-id="dec72-160">RedirectToAction - gibt ein Aktionsergebnis RedirectToRouteResult zurück.</span><span class="sxs-lookup"><span data-stu-id="dec72-160">RedirectToAction - Returns a RedirectToRouteResult action result.</span></span>
4. <span data-ttu-id="dec72-161">RedirectToRoute - gibt ein Aktionsergebnis RedirectToRouteResult zurück.</span><span class="sxs-lookup"><span data-stu-id="dec72-161">RedirectToRoute - Returns a RedirectToRouteResult action result.</span></span>
5. <span data-ttu-id="dec72-162">JSON - gibt ein JsonResult-Aktion-Ergebnis zurück.</span><span class="sxs-lookup"><span data-stu-id="dec72-162">Json - Returns a JsonResult action result.</span></span>
6. <span data-ttu-id="dec72-163">JavaScriptResult - gibt ein JavaScriptResult zurück.</span><span class="sxs-lookup"><span data-stu-id="dec72-163">JavaScriptResult - Returns a JavaScriptResult.</span></span>
7. <span data-ttu-id="dec72-164">Content - gibt ein ContentResult Aktion-Ergebnis zurück.</span><span class="sxs-lookup"><span data-stu-id="dec72-164">Content - Returns a ContentResult action result.</span></span>
8. <span data-ttu-id="dec72-165">Datei - gibt ein FileContentResult, FilePathResult oder FileStreamResult abhängig von den Parametern an die Methode übergeben.</span><span class="sxs-lookup"><span data-stu-id="dec72-165">File - Returns a FileContentResult, FilePathResult, or FileStreamResult depending on the parameters passed to the method.</span></span>

<span data-ttu-id="dec72-166">Wenn Sie eine Sicht an den Browser zurückgeben möchten, rufen Sie deshalb die View()-Methode.</span><span class="sxs-lookup"><span data-stu-id="dec72-166">So, if you want to return a View to the browser, you call the View() method.</span></span> <span data-ttu-id="dec72-167">Wenn Sie den Benutzer eine Controlleraktion in eine andere umleiten möchten, rufen Sie die RedirectToAction()-Methode.</span><span class="sxs-lookup"><span data-stu-id="dec72-167">If you want to redirect the user from one controller action to another, you call the RedirectToAction() method.</span></span> <span data-ttu-id="dec72-168">Z. B. die Aktion Details() auflisten 3 zeigt eine Ansicht oder leitet den Benutzer an die Aktion Index(), je nachdem, ob die Id-Parameter einen Wert aufweist.</span><span class="sxs-lookup"><span data-stu-id="dec72-168">For example, the Details() action in Listing 3 either displays a view or redirects the user to the Index() action depending on whether the Id parameter has a value.</span></span>

<span data-ttu-id="dec72-169">**3 – CustomerController.vb auflisten**</span><span class="sxs-lookup"><span data-stu-id="dec72-169">**Listing 3 - CustomerController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample3.vb)]

<span data-ttu-id="dec72-170">Das Aktionsergebnis ContentResult ist ein Sonderfall.</span><span class="sxs-lookup"><span data-stu-id="dec72-170">The ContentResult action result is special.</span></span> <span data-ttu-id="dec72-171">Das Aktionsergebnis ContentResult können Sie ein Aktionsergebnis als nur-Text zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="dec72-171">You can use the ContentResult action result to return an action result as plain text.</span></span> <span data-ttu-id="dec72-172">Beispielsweise erfolgt die Methodenrückgabe Index() 4 Auflisten einer Nachricht als nur-Text und nicht als HTML.</span><span class="sxs-lookup"><span data-stu-id="dec72-172">For example, the Index() method in Listing 4 returns a message as plain text and not as HTML.</span></span>

<span data-ttu-id="dec72-173">**4 – Controllers\StatusController.vb auflisten**</span><span class="sxs-lookup"><span data-stu-id="dec72-173">**Listing 4 - Controllers\StatusController.vb**</span></span>

> <span data-ttu-id="dec72-174">StatusController</span><span class="sxs-lookup"><span data-stu-id="dec72-174">StatusController</span></span>


> <span data-ttu-id="dec72-175">System.Web.Mvc.Controller</span><span class="sxs-lookup"><span data-stu-id="dec72-175">System.Web.Mvc.Controller</span></span>


[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample4.vb)]

<span data-ttu-id="dec72-176">Wenn die StatusController.Index() Aktion aufgerufen wird, wird eine Sicht nicht zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="dec72-176">When the StatusController.Index() action is invoked, a view is not returned.</span></span> <span data-ttu-id="dec72-177">Stattdessen den unformatierten Text "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="dec72-177">Instead, the raw text "Hello World!"</span></span> <span data-ttu-id="dec72-178">wird an den Browser zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="dec72-178">is returned to the browser.</span></span>

<span data-ttu-id="dec72-179">Wenn eine Controlleraktion als Ergebnis kein Aktionsergebnis - beispielsweise ein Datum oder eine ganze Zahl - zurückgegeben wird das Ergebnis automatisch in eine ContentResult eingebunden.</span><span class="sxs-lookup"><span data-stu-id="dec72-179">If a controller action returns a result that is not an action result - for example, a date or an integer - then the result is wrapped in a ContentResult automatically.</span></span> <span data-ttu-id="dec72-180">Z. B. wenn die Aktion Index() der WorkController auflisten 5 aufgerufen wird, wird das Datum als eine ContentResult automatisch zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="dec72-180">For example, when the Index() action of the WorkController in Listing 5 is invoked, the date is returned as a ContentResult automatically.</span></span>

<span data-ttu-id="dec72-181">**5 – WorkController.vb auflisten**</span><span class="sxs-lookup"><span data-stu-id="dec72-181">**Listing 5 - WorkController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample5.vb)]

<span data-ttu-id="dec72-182">Die Aktion Index() auflisten 5 zurückgegeben ein DateTime-Objekt.</span><span class="sxs-lookup"><span data-stu-id="dec72-182">The Index() action in Listing 5 returns a DateTime object.</span></span> <span data-ttu-id="dec72-183">ASP.NET MVC-Framework die DateTime-Objekt in eine Zeichenfolge konvertiert und dient als Wrapper für den DateTime-Wert in einer ContentResult automatisch.</span><span class="sxs-lookup"><span data-stu-id="dec72-183">The ASP.NET MVC framework converts the DateTime object to a string and wraps the DateTime value in a ContentResult automatically.</span></span> <span data-ttu-id="dec72-184">Der Browser empfängt das Datum und die Uhrzeit als nur-Text.</span><span class="sxs-lookup"><span data-stu-id="dec72-184">The browser receives the date and time as plain text.</span></span>

## <a name="summary"></a><span data-ttu-id="dec72-185">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="dec72-185">Summary</span></span>

<span data-ttu-id="dec72-186">Der Zweck dieses Lernprogramms war, Einführung in die Konzepte der ASP.NET MVC-Controller, Controlleraktionen und Controller Aktionsergebnisse.</span><span class="sxs-lookup"><span data-stu-id="dec72-186">The purpose of this tutorial was to introduce you to the concepts of ASP.NET MVC controllers, controller actions, and controller action results.</span></span> <span data-ttu-id="dec72-187">Im ersten Abschnitt haben Sie gelernt, wie neue Controller einer ASP.NET MVC-Projekt hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="dec72-187">In the first section, you learned how to add new controllers to an ASP.NET MVC project.</span></span> <span data-ttu-id="dec72-188">Als Nächstes haben Sie gelernt, wie öffentliche Methoden eines Controllers für die Universe als Controlleraktionen verfügbar gemacht.</span><span class="sxs-lookup"><span data-stu-id="dec72-188">Next, you learned how public methods of a controller are exposed to the universe as controller actions.</span></span> <span data-ttu-id="dec72-189">Schließlich erläutert die verschiedenen Arten von Aktionsergebnisse, die eine Controlleraktion zurückgegeben werden können.</span><span class="sxs-lookup"><span data-stu-id="dec72-189">Finally, we discussed the different types of action results that can be returned from a controller action.</span></span> <span data-ttu-id="dec72-190">Insbesondere besprochen haben wir wie ein ViewResult, RedirectToActionResult und ContentResult in eine Controlleraktion zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="dec72-190">In particular, we discussed how to return a ViewResult, RedirectToActionResult, and ContentResult from a controller action.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="dec72-191">[Zurück](creating-a-custom-route-constraint-cs.md)
[Weiter](creating-custom-routes-vb.md)</span><span class="sxs-lookup"><span data-stu-id="dec72-191">[Previous](creating-a-custom-route-constraint-cs.md)
[Next](creating-custom-routes-vb.md)</span></span>
