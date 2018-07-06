---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
title: ASP.NET MVC-Controller – Übersicht (VB) | Microsoft-Dokumentation
author: StephenWalther
description: In diesem Tutorial führt Sie Stephen Walther in ASP.NET MVC-Controller. Erfahren Sie, wie erstellen neue Domänencontroller und Zurückgeben von verschiedenen Arten von Aktion Res...
ms.author: aspnetcontent
ms.date: 02/16/2008
ms.assetid: 94c3e5d9-a904-445e-a34e-d92fd1ca108a
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: ac5e9242f494b8472e582bc76a6f4805db2f770f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809222"
---
<a name="aspnet-mvc-controller-overview-vb"></a><span data-ttu-id="b2269-104">ASP.NET MVC-Controller – Übersicht (VB)</span><span class="sxs-lookup"><span data-stu-id="b2269-104">ASP.NET MVC Controller Overview (VB)</span></span>
====================
<span data-ttu-id="b2269-105">durch [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="b2269-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="b2269-106">In diesem Tutorial führt Sie Stephen Walther in ASP.NET MVC-Controller.</span><span class="sxs-lookup"><span data-stu-id="b2269-106">In this tutorial, Stephen Walther introduces you to ASP.NET MVC controllers.</span></span> <span data-ttu-id="b2269-107">Erfahren Sie, wie erstellen neue Domänencontroller und Zurückgeben von verschiedenen Arten von Aktionsergebnissen.</span><span class="sxs-lookup"><span data-stu-id="b2269-107">You learn how to create new controllers and return different types of action results.</span></span>


<span data-ttu-id="b2269-108">Dieses Tutorial wird beschrieben, das Thema der ASP.NET MVC-Controllern, Controlleraktionen und Aktionsergebnisse.</span><span class="sxs-lookup"><span data-stu-id="b2269-108">This tutorial explores the topic of ASP.NET MVC controllers, controller actions, and action results.</span></span> <span data-ttu-id="b2269-109">Nachdem Sie dieses Tutorial abgeschlossen haben, wissen Sie, wie der Controller verwendet werden, um zu steuern, die ein Besucher mit einer ASP.NET MVC-Website interagiert.</span><span class="sxs-lookup"><span data-stu-id="b2269-109">After you complete this tutorial, you will understand how controllers are used to control the way a visitor interacts with an ASP.NET MVC website.</span></span>

## <a name="understanding-controllers"></a><span data-ttu-id="b2269-110">Grundlegendes zu Controllern</span><span class="sxs-lookup"><span data-stu-id="b2269-110">Understanding Controllers</span></span>

<span data-ttu-id="b2269-111">MVC-Controller sind verantwortlich für die Reaktion auf Anforderungen für eine ASP.NET MVC-Website.</span><span class="sxs-lookup"><span data-stu-id="b2269-111">MVC controllers are responsible for responding to requests made against an ASP.NET MVC website.</span></span> <span data-ttu-id="b2269-112">Jede Browseranforderung wird ein bestimmter Domänencontroller zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="b2269-112">Each browser request is mapped to a particular controller.</span></span> <span data-ttu-id="b2269-113">Beispiel: Angenommen Sie, dass Sie die folgende URL in die Adressleiste des Browsers eingeben:</span><span class="sxs-lookup"><span data-stu-id="b2269-113">For example, imagine that you enter the following URL into the address bar of your browser:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="b2269-114">In diesem Fall wird ein Controller, mit der Bezeichnung ProductController aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="b2269-114">In this case, a controller named ProductController is invoked.</span></span> <span data-ttu-id="b2269-115">ProductController ist verantwortlich für das Generieren der Antwort auf die Browseranforderung.</span><span class="sxs-lookup"><span data-stu-id="b2269-115">The ProductController is responsible for generating the response to the browser request.</span></span> <span data-ttu-id="b2269-116">Z. B. der Controller möglicherweise eine bestimmte Ansicht zurück an den Browser zurück, oder der Controller kann leiten Sie den Benutzer an einen anderen Controller.</span><span class="sxs-lookup"><span data-stu-id="b2269-116">For example, the controller might return a particular view back to the browser or the controller might redirect the user to another controller.</span></span>

<span data-ttu-id="b2269-117">Codebeispiel 1 enthält einen einfachen Controller, mit der Bezeichnung ProductController.</span><span class="sxs-lookup"><span data-stu-id="b2269-117">Listing 1 contains a simple controller named ProductController.</span></span>

<span data-ttu-id="b2269-118">**Listing1 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="b2269-118">**Listing1 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample1.vb)]

<span data-ttu-id="b2269-119">Wie in Codebeispiel 1 sehen ist, ist ein Controller nur eine Klasse (Visual Basic .NET oder C#-Klasse).</span><span class="sxs-lookup"><span data-stu-id="b2269-119">As you can see from Listing 1, a controller is just a class (a Visual Basic .NET or C# class).</span></span> <span data-ttu-id="b2269-120">Ein Controller ist eine Klasse, die von der Basisklasse von System.Web.Mvc.Controller abgeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="b2269-120">A controller is a class that derives from the base System.Web.Mvc.Controller class.</span></span> <span data-ttu-id="b2269-121">Da ein Controller von dieser Basisklasse erbt, erbt ein Controller mehrere nützliche Methoden für kostenlose (wir werden diese Methoden weiter unten erörtert).</span><span class="sxs-lookup"><span data-stu-id="b2269-121">Because a controller inherits from this base class, a controller inherits several useful methods for free (We discuss these methods in a moment).</span></span>

## <a name="understanding-controller-actions"></a><span data-ttu-id="b2269-122">Grundlegendes zu Controlleraktionen</span><span class="sxs-lookup"><span data-stu-id="b2269-122">Understanding Controller Actions</span></span>

<span data-ttu-id="b2269-123">Ein Controller verfügbar macht, Controlleraktionen.</span><span class="sxs-lookup"><span data-stu-id="b2269-123">A controller exposes controller actions.</span></span> <span data-ttu-id="b2269-124">Eine Aktion ist eine Methode für einen Controller, der aufgerufen wird, wenn Sie eine bestimmte URL in die Adressleiste Ihres Browsers eingeben.</span><span class="sxs-lookup"><span data-stu-id="b2269-124">An action is a method on a controller that gets called when you enter a particular URL in your browser address bar.</span></span> <span data-ttu-id="b2269-125">Beispiel: Angenommen Sie, dass Sie eine Anforderung für die folgende URL ausführen:</span><span class="sxs-lookup"><span data-stu-id="b2269-125">For example, imagine that you make a request for the following URL:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="b2269-126">In diesem Fall wird die Index()-Methode für die ProductController-Klasse aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="b2269-126">In this case, the Index() method is called on the ProductController class.</span></span> <span data-ttu-id="b2269-127">Die Index()-Methode ist ein Beispiel für eine Controlleraktion.</span><span class="sxs-lookup"><span data-stu-id="b2269-127">The Index() method is an example of a controller action.</span></span>

<span data-ttu-id="b2269-128">Eine Controlleraktion muss eine öffentliche Methode von einem Controller-Klasse.</span><span class="sxs-lookup"><span data-stu-id="b2269-128">A controller action must be a public method of a controller class.</span></span> <span data-ttu-id="b2269-129">Visual Basic.NET Methoden in der Standardeinstellung sind die öffentlichen Methoden.</span><span class="sxs-lookup"><span data-stu-id="b2269-129">Visual Basic.NET methods, by default, are public methods.</span></span> <span data-ttu-id="b2269-130">Beachten Sie, dass jede öffentliche Methode, die Sie zu einer Controllerklasse hinzufügen als eine Controlleraktion automatisch bereitgestellt wird (Achten Sie Informationen dazu, da eine Controlleraktion von jedem Benutzer im Universum aufgerufen werden kann, indem Sie einfach die richtige URL in die Adressleiste des Browsers eingeben).</span><span class="sxs-lookup"><span data-stu-id="b2269-130">Realize that any public method that you add to a controller class is exposed as a controller action automatically (You must be careful about this since a controller action can be invoked by anyone in the universe simply by typing the right URL into a browser address bar).</span></span>

<span data-ttu-id="b2269-131">Es gibt einige zusätzlichen Anforderungen, die durch eine Controlleraktion erfüllt werden müssen.</span><span class="sxs-lookup"><span data-stu-id="b2269-131">There are some additional requirements that must be satisfied by a controller action.</span></span> <span data-ttu-id="b2269-132">Eine Methode als eine Controlleraktion kann nicht überladen werden.</span><span class="sxs-lookup"><span data-stu-id="b2269-132">A method used as a controller action cannot be overloaded.</span></span> <span data-ttu-id="b2269-133">Darüber hinaus darf keine Controlleraktion auf eine statische Methode sein.</span><span class="sxs-lookup"><span data-stu-id="b2269-133">Furthermore, a controller action cannot be a static method.</span></span> <span data-ttu-id="b2269-134">Davon abgesehen können Sie beliebige andere Methode als eine Controlleraktion.</span><span class="sxs-lookup"><span data-stu-id="b2269-134">Other than that, you can use just about any method as a controller action.</span></span>

## <a name="understanding-action-results"></a><span data-ttu-id="b2269-135">Grundlegendes zu Aktionsergebnisse</span><span class="sxs-lookup"><span data-stu-id="b2269-135">Understanding Action Results</span></span>

<span data-ttu-id="b2269-136">Eine Controlleraktion gibt so genannte ein *Aktionsergebnis*.</span><span class="sxs-lookup"><span data-stu-id="b2269-136">A controller action returns something called an *action result*.</span></span> <span data-ttu-id="b2269-137">Ein Aktionsergebnis ist, was eine Controlleraktion als Reaktion auf eine Browseranforderung zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="b2269-137">An action result is what a controller action returns in response to a browser request.</span></span>

<span data-ttu-id="b2269-138">ASP.NET MVC-Framework unterstützt verschiedene Typen von Aktionsergebnissen, einschließlich:</span><span class="sxs-lookup"><span data-stu-id="b2269-138">The ASP.NET MVC framework supports several types of action results including:</span></span>

1. <span data-ttu-id="b2269-139">ViewResult - stellt HTML und Markup.</span><span class="sxs-lookup"><span data-stu-id="b2269-139">ViewResult - Represents HTML and markup.</span></span>
2. <span data-ttu-id="b2269-140">EmptyResult - stellt kein Ergebnis dar.</span><span class="sxs-lookup"><span data-stu-id="b2269-140">EmptyResult - Represents no result.</span></span>
3. <span data-ttu-id="b2269-141">RedirectResult - stellt eine Umleitung an eine neue URL dar.</span><span class="sxs-lookup"><span data-stu-id="b2269-141">RedirectResult - Represents a redirection to a new URL.</span></span>
4. <span data-ttu-id="b2269-142">JsonResult - stellt eine JavaScript Object Notation-Ergebnis, das in einer AJAX-Anwendung verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="b2269-142">JsonResult - Represents a JavaScript Object Notation result that can be used in an AJAX application.</span></span>
5. <span data-ttu-id="b2269-143">JavaScriptResult - stellt eine JavaScript-Skript dar.</span><span class="sxs-lookup"><span data-stu-id="b2269-143">JavaScriptResult - Represents a JavaScript script.</span></span>
6. <span data-ttu-id="b2269-144">ContentResult - stellt einen Textergebnis dar.</span><span class="sxs-lookup"><span data-stu-id="b2269-144">ContentResult - Represents a text result.</span></span>
7. <span data-ttu-id="b2269-145">FileContentResult - stellt eine herunterladbare Datei (mit den binären Inhalt) dar.</span><span class="sxs-lookup"><span data-stu-id="b2269-145">FileContentResult - Represents a downloadable file (with the binary content).</span></span>
8. <span data-ttu-id="b2269-146">FilePathResult - stellt eine herunterladbare Datei (mit einem Pfad).</span><span class="sxs-lookup"><span data-stu-id="b2269-146">FilePathResult - Represents a downloadable file (with a path).</span></span>
9. <span data-ttu-id="b2269-147">FileStreamResult - stellt eine herunterladbare Datei (mit einem Dateistream) dar.</span><span class="sxs-lookup"><span data-stu-id="b2269-147">FileStreamResult - Represents a downloadable file (with a file stream).</span></span>

<span data-ttu-id="b2269-148">Alle diese Aktionsergebnisse erben von der ActionResult-Basisklasse.</span><span class="sxs-lookup"><span data-stu-id="b2269-148">All of these action results inherit from the base ActionResult class.</span></span>

<span data-ttu-id="b2269-149">In den meisten Fällen gibt eine Controlleraktion ein ViewResult zurück.</span><span class="sxs-lookup"><span data-stu-id="b2269-149">In most cases, a controller action returns a ViewResult.</span></span> <span data-ttu-id="b2269-150">Beispielsweise gibt die Index-Controlleraktion Programmausdruck 2 ein ViewResult zurück.</span><span class="sxs-lookup"><span data-stu-id="b2269-150">For example, the Index controller action in Listing 2 returns a ViewResult.</span></span>

<span data-ttu-id="b2269-151">**Codebeispiel 2 - Controllers\BookController.vb**</span><span class="sxs-lookup"><span data-stu-id="b2269-151">**Listing 2 - Controllers\BookController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample2.vb)]

<span data-ttu-id="b2269-152">Wenn eine Aktion ein ViewResult zurückgibt, wird HTML im Browser zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="b2269-152">When an action returns a ViewResult, HTML is returned to the browser.</span></span> <span data-ttu-id="b2269-153">Programmausdruck 2 die Index()-Methode gibt eine Ansicht namens Index, an den Browser zurück.</span><span class="sxs-lookup"><span data-stu-id="b2269-153">The Index() method in Listing 2 returns a view named Index to the browser.</span></span>

<span data-ttu-id="b2269-154">Beachten Sie, dass die Aktion Index() Programmausdruck 2 eine ViewResult() nicht zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="b2269-154">Notice that the Index() action in Listing 2 does not return a ViewResult().</span></span> <span data-ttu-id="b2269-155">Stattdessen wird die View()-Methode der Basisklasse Controller aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="b2269-155">Instead, the View() method of the Controller base class is called.</span></span> <span data-ttu-id="b2269-156">Sie geben ein Aktionsergebnis normalerweise nicht direkt zurück.</span><span class="sxs-lookup"><span data-stu-id="b2269-156">Normally, you do not return an action result directly.</span></span> <span data-ttu-id="b2269-157">Stattdessen rufen Sie eine der folgenden Methoden der Basisklasse Controller:</span><span class="sxs-lookup"><span data-stu-id="b2269-157">Instead, you call one of the following methods of the Controller base class:</span></span>

1. <span data-ttu-id="b2269-158">Anzeigen – gibt ein ViewResult-Aktion-Ergebnis zurück.</span><span class="sxs-lookup"><span data-stu-id="b2269-158">View - Returns a ViewResult action result.</span></span>
2. <span data-ttu-id="b2269-159">Umleiten: Gibt ein RedirectResult-Aktion-Ergebnis zurück.</span><span class="sxs-lookup"><span data-stu-id="b2269-159">Redirect - Returns a RedirectResult action result.</span></span>
3. <span data-ttu-id="b2269-160">RedirectToAction - gibt ein Aktionsergebnis RedirectToRouteResult zurück.</span><span class="sxs-lookup"><span data-stu-id="b2269-160">RedirectToAction - Returns a RedirectToRouteResult action result.</span></span>
4. <span data-ttu-id="b2269-161">RedirectToRoute - gibt ein Aktionsergebnis RedirectToRouteResult zurück.</span><span class="sxs-lookup"><span data-stu-id="b2269-161">RedirectToRoute - Returns a RedirectToRouteResult action result.</span></span>
5. <span data-ttu-id="b2269-162">JSON - gibt ein JsonResult-Aktion-Ergebnis zurück.</span><span class="sxs-lookup"><span data-stu-id="b2269-162">Json - Returns a JsonResult action result.</span></span>
6. <span data-ttu-id="b2269-163">JavaScriptResult - gibt ein JavaScriptResult zurück.</span><span class="sxs-lookup"><span data-stu-id="b2269-163">JavaScriptResult - Returns a JavaScriptResult.</span></span>
7. <span data-ttu-id="b2269-164">Content - gibt ein ContentResult-Aktion-Ergebnis zurück.</span><span class="sxs-lookup"><span data-stu-id="b2269-164">Content - Returns a ContentResult action result.</span></span>
8. <span data-ttu-id="b2269-165">Datei - gibt ein FileContentResult, FilePathResult oder FileStreamResult abhängig von den Parametern an die Methode übergeben.</span><span class="sxs-lookup"><span data-stu-id="b2269-165">File - Returns a FileContentResult, FilePathResult, or FileStreamResult depending on the parameters passed to the method.</span></span>

<span data-ttu-id="b2269-166">Wenn eine Ansicht an den Browser zurückgegeben werden sollen, rufen Sie daher die View()-Methode.</span><span class="sxs-lookup"><span data-stu-id="b2269-166">So, if you want to return a View to the browser, you call the View() method.</span></span> <span data-ttu-id="b2269-167">Wenn Sie den Benutzer eine Controlleraktion auf einen anderen umleiten möchten, rufen Sie die RedirectToAction()-Methode.</span><span class="sxs-lookup"><span data-stu-id="b2269-167">If you want to redirect the user from one controller action to another, you call the RedirectToAction() method.</span></span> <span data-ttu-id="b2269-168">Z. B. die Aktion Details() in Programmausdruck 3 zeigt eine Ansicht oder leitet den Benutzer an die Aktion Index(), je nachdem, ob der Id-Parameter einen Wert hat.</span><span class="sxs-lookup"><span data-stu-id="b2269-168">For example, the Details() action in Listing 3 either displays a view or redirects the user to the Index() action depending on whether the Id parameter has a value.</span></span>

<span data-ttu-id="b2269-169">**Codebeispiel 3 - CustomerController.vb**</span><span class="sxs-lookup"><span data-stu-id="b2269-169">**Listing 3 - CustomerController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample3.vb)]

<span data-ttu-id="b2269-170">Das Aktionsergebnis ContentResult ist etwas Besonderes.</span><span class="sxs-lookup"><span data-stu-id="b2269-170">The ContentResult action result is special.</span></span> <span data-ttu-id="b2269-171">Sie können das Aktionsergebnis ContentResult verwenden, ein Aktionsergebnis als nur-Text zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="b2269-171">You can use the ContentResult action result to return an action result as plain text.</span></span> <span data-ttu-id="b2269-172">Die Index()-Methode in Listing 4 gibt z. B. eine Meldung zurück, als nur-Text und nicht als HTML.</span><span class="sxs-lookup"><span data-stu-id="b2269-172">For example, the Index() method in Listing 4 returns a message as plain text and not as HTML.</span></span>

<span data-ttu-id="b2269-173">**Programmausdruck 4 - Controllers\StatusController.vb**</span><span class="sxs-lookup"><span data-stu-id="b2269-173">**Listing 4 - Controllers\StatusController.vb**</span></span>

> <span data-ttu-id="b2269-174">StatusController</span><span class="sxs-lookup"><span data-stu-id="b2269-174">StatusController</span></span>
> 
> 
> <span data-ttu-id="b2269-175">System.Web.Mvc.Controller</span><span class="sxs-lookup"><span data-stu-id="b2269-175">System.Web.Mvc.Controller</span></span>


[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample4.vb)]

<span data-ttu-id="b2269-176">Wenn die Aktion StatusController.Index() aufgerufen wird, wird eine Sicht nicht zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="b2269-176">When the StatusController.Index() action is invoked, a view is not returned.</span></span> <span data-ttu-id="b2269-177">Stattdessen der unformatierte Text "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="b2269-177">Instead, the raw text "Hello World!"</span></span> <span data-ttu-id="b2269-178">wird an den Browser zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="b2269-178">is returned to the browser.</span></span>

<span data-ttu-id="b2269-179">Wenn eine Controlleraktion ein Ergebnis, das kein Aktionsergebnis – z. B. ein Datum oder eine ganze Zahl – zurückgibt wird dann das Ergebnis in eine ContentResult automatisch eingebunden.</span><span class="sxs-lookup"><span data-stu-id="b2269-179">If a controller action returns a result that is not an action result - for example, a date or an integer - then the result is wrapped in a ContentResult automatically.</span></span> <span data-ttu-id="b2269-180">Z. B. wenn die Aktion Index() der WorkController in Listing 5 aufgerufen wird, wird das Datum als eine ContentResult automatisch zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="b2269-180">For example, when the Index() action of the WorkController in Listing 5 is invoked, the date is returned as a ContentResult automatically.</span></span>

<span data-ttu-id="b2269-181">**Programmausdruck 5 - WorkController.vb**</span><span class="sxs-lookup"><span data-stu-id="b2269-181">**Listing 5 - WorkController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample5.vb)]

<span data-ttu-id="b2269-182">Die Aktion Index() in Listing 5 gibt einen DateTime-Objekt zurück.</span><span class="sxs-lookup"><span data-stu-id="b2269-182">The Index() action in Listing 5 returns a DateTime object.</span></span> <span data-ttu-id="b2269-183">ASP.NET MVC-Framework das DateTime-Objekt in eine Zeichenfolge konvertiert und dient als Wrapper für "DateTime"-Werts in einen ContentResult automatisch.</span><span class="sxs-lookup"><span data-stu-id="b2269-183">The ASP.NET MVC framework converts the DateTime object to a string and wraps the DateTime value in a ContentResult automatically.</span></span> <span data-ttu-id="b2269-184">Der Browser empfängt das Datum und die Uhrzeit, als nur-Text.</span><span class="sxs-lookup"><span data-stu-id="b2269-184">The browser receives the date and time as plain text.</span></span>

## <a name="summary"></a><span data-ttu-id="b2269-185">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="b2269-185">Summary</span></span>

<span data-ttu-id="b2269-186">Der Zweck dieses Lernprogramms wurde, führen Sie die Konzepte von ASP.NET MVC-Controllern, Controlleraktionen und Ergebnisse von Controlleraktionen ein.</span><span class="sxs-lookup"><span data-stu-id="b2269-186">The purpose of this tutorial was to introduce you to the concepts of ASP.NET MVC controllers, controller actions, and controller action results.</span></span> <span data-ttu-id="b2269-187">Im ersten Abschnitt haben Sie gelernt, wie neue Domänencontroller zu einer ASP.NET MVC-Projekt hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="b2269-187">In the first section, you learned how to add new controllers to an ASP.NET MVC project.</span></span> <span data-ttu-id="b2269-188">Anschließend haben Sie erfahren, wie öffentliche Methoden in einem Controller für das Universum als Controlleraktionen verfügbar gemacht.</span><span class="sxs-lookup"><span data-stu-id="b2269-188">Next, you learned how public methods of a controller are exposed to the universe as controller actions.</span></span> <span data-ttu-id="b2269-189">Zum Schluss erläutert die verschiedenen Typen von Aktionsergebnissen, die eine Controlleraktion zurückgegeben werden können.</span><span class="sxs-lookup"><span data-stu-id="b2269-189">Finally, we discussed the different types of action results that can be returned from a controller action.</span></span> <span data-ttu-id="b2269-190">Insbesondere erläutert wie ein ViewResult RedirectToActionResult und ContentResult von eine Controlleraktion zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="b2269-190">In particular, we discussed how to return a ViewResult, RedirectToActionResult, and ContentResult from a controller action.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b2269-191">[Zurück](creating-a-custom-route-constraint-cs.md)
> [Weiter](creating-custom-routes-vb.md)</span><span class="sxs-lookup"><span data-stu-id="b2269-191">[Previous](creating-a-custom-route-constraint-cs.md)
[Next](creating-custom-routes-vb.md)</span></span>
