---
uid: mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
title: Übersicht über ASP.NET MVC-Controller (c#) | Microsoft Docs
author: StephenWalther
description: In diesem Lernprogramm führt Sie Stephen Walther in ASP.NET MVC-Controller. Erfahren Sie, wie zum Erstellen von neuen Controller und Aktion Res verschiedene Datentypen zurückgeben...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: b985c49a-3668-455c-a366-f85f6bc64b12
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: 95e7c555a52c8c3b765a6fffab15276491cf5714
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869087"
---
<a name="aspnet-mvc-controller-overview-c"></a><span data-ttu-id="5240b-104">Übersicht über ASP.NET MVC-Controller (c#)</span><span class="sxs-lookup"><span data-stu-id="5240b-104">ASP.NET MVC Controller Overview (C#)</span></span>
====================
<span data-ttu-id="5240b-105">durch [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="5240b-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="5240b-106">In diesem Lernprogramm führt Sie Stephen Walther in ASP.NET MVC-Controller.</span><span class="sxs-lookup"><span data-stu-id="5240b-106">In this tutorial, Stephen Walther introduces you to ASP.NET MVC controllers.</span></span> <span data-ttu-id="5240b-107">Erfahren Sie, wie neue Domänencontroller zu erstellen und unterschiedliche Rückgabetypen von Aktionsergebnisse.</span><span class="sxs-lookup"><span data-stu-id="5240b-107">You learn how to create new controllers and return different types of action results.</span></span>


<span data-ttu-id="5240b-108">In diesem Lernprogramm wird das Thema der ASP.NET MVC-Controller, Controlleraktionen und Aktionsergebnisse erklärt.</span><span class="sxs-lookup"><span data-stu-id="5240b-108">This tutorial explores the topic of ASP.NET MVC controllers, controller actions, and action results.</span></span> <span data-ttu-id="5240b-109">Nachdem Sie dieses Lernprogramm abgeschlossen haben, werden Sie verstehen, wie Domänencontroller verwendet werden, um zu steuern, die ein Besucher mit einer ASP.NET MVC-Website interagiert.</span><span class="sxs-lookup"><span data-stu-id="5240b-109">After you complete this tutorial, you will understand how controllers are used to control the way a visitor interacts with an ASP.NET MVC website.</span></span>

## <a name="understanding-controllers"></a><span data-ttu-id="5240b-110">Grundlegendes zu Controllern</span><span class="sxs-lookup"><span data-stu-id="5240b-110">Understanding Controllers</span></span>

<span data-ttu-id="5240b-111">MVC-Controller sind verantwortlich für die Reaktion auf Anforderungen für eine ASP.NET MVC-Website.</span><span class="sxs-lookup"><span data-stu-id="5240b-111">MVC controllers are responsible for responding to requests made against an ASP.NET MVC website.</span></span> <span data-ttu-id="5240b-112">Jede Browseranforderung wird ein bestimmter Domänencontroller zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="5240b-112">Each browser request is mapped to a particular controller.</span></span> <span data-ttu-id="5240b-113">Angenommen Sie, dass Sie die folgende URL in die Adressleiste des Browsers eingeben:</span><span class="sxs-lookup"><span data-stu-id="5240b-113">For example, imagine that you enter the following URL into the address bar of your browser:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="5240b-114">In diesem Fall wird ein Controller namens ProductController aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="5240b-114">In this case, a controller named ProductController is invoked.</span></span> <span data-ttu-id="5240b-115">Die ProductController ist verantwortlich für die Antwort auf die Browseranforderung zu generieren.</span><span class="sxs-lookup"><span data-stu-id="5240b-115">The ProductController is responsible for generating the response to the browser request.</span></span> <span data-ttu-id="5240b-116">Z. B. der Controller möglicherweise eine bestimmte Ansicht zurück an den Browser zurück, oder der Controller kann den Benutzer weiterleiten, auf einen anderen Controller.</span><span class="sxs-lookup"><span data-stu-id="5240b-116">For example, the controller might return a particular view back to the browser or the controller might redirect the user to another controller.</span></span>

<span data-ttu-id="5240b-117">Auflisten von 1 enthält einen einfachen Controller mit dem Namen ProductController.</span><span class="sxs-lookup"><span data-stu-id="5240b-117">Listing 1 contains a simple controller named ProductController.</span></span>

<span data-ttu-id="5240b-118">**Listing1 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="5240b-118">**Listing1 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample1.cs)]

<span data-ttu-id="5240b-119">Wie Sie im Codebeispiel 1 sehen können, ist ein Controller nur einer Klasse (Visual Basic .NET oder C#-Klasse).</span><span class="sxs-lookup"><span data-stu-id="5240b-119">As you can see from Listing 1, a controller is just a class (a Visual Basic .NET or C# class).</span></span> <span data-ttu-id="5240b-120">Ein Domänencontroller ist eine Klasse, die von der Basisklasse System.Web.Mvc.Controller abgeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="5240b-120">A controller is a class that derives from the base System.Web.Mvc.Controller class.</span></span> <span data-ttu-id="5240b-121">Da es sich bei ein Domänencontroller aus dieser Basisklasse erbt, erbt ein Controller mehrere nützliche Methoden kostenlos (erörtert, diese Methoden in wenigen Augenblicken).</span><span class="sxs-lookup"><span data-stu-id="5240b-121">Because a controller inherits from this base class, a controller inherits several useful methods for free (We discuss these methods in a moment).</span></span>

## <a name="understanding-controller-actions"></a><span data-ttu-id="5240b-122">Grundlegendes zu Controlleraktionen</span><span class="sxs-lookup"><span data-stu-id="5240b-122">Understanding Controller Actions</span></span>

<span data-ttu-id="5240b-123">Ein Controller macht Controlleraktionen verfügbar.</span><span class="sxs-lookup"><span data-stu-id="5240b-123">A controller exposes controller actions.</span></span> <span data-ttu-id="5240b-124">Eine Aktion ist eine Methode auf einem Domänencontroller, der aufgerufen wird, wenn Sie eine bestimmte URL in der Adressleiste des Browsers eingeben.</span><span class="sxs-lookup"><span data-stu-id="5240b-124">An action is a method on a controller that gets called when you enter a particular URL in your browser address bar.</span></span> <span data-ttu-id="5240b-125">Angenommen Sie, dass Sie eine Anforderung für die folgende URL verwenden:</span><span class="sxs-lookup"><span data-stu-id="5240b-125">For example, imagine that you make a request for the following URL:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="5240b-126">In diesem Fall wird die Index()-Methode für die Klasse ProductController aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="5240b-126">In this case, the Index() method is called on the ProductController class.</span></span> <span data-ttu-id="5240b-127">Die Index()-Methode ist ein Beispiel für eine Controlleraktion.</span><span class="sxs-lookup"><span data-stu-id="5240b-127">The Index() method is an example of a controller action.</span></span>

<span data-ttu-id="5240b-128">Eine Controlleraktion muss eine öffentliche Methode einer Controllerklasse sein.</span><span class="sxs-lookup"><span data-stu-id="5240b-128">A controller action must be a public method of a controller class.</span></span> <span data-ttu-id="5240b-129">C#-Methoden, in der Standardeinstellung werden private.</span><span class="sxs-lookup"><span data-stu-id="5240b-129">C# methods, by default, are private methods.</span></span> <span data-ttu-id="5240b-130">Beachten Sie, dass keine öffentliche Methode, die Sie zu einer Controllerklasse hinzufügen automatisch als eine Controlleraktion bereitgestellt wird (Achten Sie Informationen hierzu, da eine Controlleraktion von einem Benutzer im Universum aufgerufen werden kann, indem Sie einfach die richtige URL in die Adressleiste des Webbrowsers eingeben).</span><span class="sxs-lookup"><span data-stu-id="5240b-130">Realize that any public method that you add to a controller class is exposed as a controller action automatically (You must be careful about this since a controller action can be invoked by anyone in the universe simply by typing the right URL into a browser address bar).</span></span>

<span data-ttu-id="5240b-131">Es gibt einige zusätzlichen Anforderungen, die durch eine Controlleraktion erfüllt werden müssen.</span><span class="sxs-lookup"><span data-stu-id="5240b-131">There are some additional requirements that must be satisfied by a controller action.</span></span> <span data-ttu-id="5240b-132">Eine Methode als eine Controlleraktion verwendet, kann nicht überladen werden.</span><span class="sxs-lookup"><span data-stu-id="5240b-132">A method used as a controller action cannot be overloaded.</span></span> <span data-ttu-id="5240b-133">Darüber hinaus darf keine Controlleraktion eine statische Methode sein.</span><span class="sxs-lookup"><span data-stu-id="5240b-133">Furthermore, a controller action cannot be a static method.</span></span> <span data-ttu-id="5240b-134">Als dem können Sie beliebige andere Methode als eine Controlleraktion.</span><span class="sxs-lookup"><span data-stu-id="5240b-134">Other than that, you can use just about any method as a controller action.</span></span>

## <a name="understanding-action-results"></a><span data-ttu-id="5240b-135">Grundlegendes zu den Ergebnissen der Aktion</span><span class="sxs-lookup"><span data-stu-id="5240b-135">Understanding Action Results</span></span>

<span data-ttu-id="5240b-136">Gibt eine Controlleraktion zurück, so genannte ein *Aktionsergebnis*.</span><span class="sxs-lookup"><span data-stu-id="5240b-136">A controller action returns something called an *action result*.</span></span> <span data-ttu-id="5240b-137">Ein Aktionsergebnis ist, was eine Controlleraktion als Antwort auf eine Browseranforderung zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="5240b-137">An action result is what a controller action returns in response to a browser request.</span></span>

<span data-ttu-id="5240b-138">Das ASP.NET-MVC-Framework unterstützt mehrere Typen von Aktionsergebnisse, einschließlich:</span><span class="sxs-lookup"><span data-stu-id="5240b-138">The ASP.NET MVC framework supports several types of action results including:</span></span>

1. <span data-ttu-id="5240b-139">ViewResult - stellt HTML und Markup.</span><span class="sxs-lookup"><span data-stu-id="5240b-139">ViewResult - Represents HTML and markup.</span></span>
2. <span data-ttu-id="5240b-140">EmptyResult - stellt kein Ergebnis dar.</span><span class="sxs-lookup"><span data-stu-id="5240b-140">EmptyResult - Represents no result.</span></span>
3. <span data-ttu-id="5240b-141">RedirectResult - stellt eine Umleitung an eine neue URL um.</span><span class="sxs-lookup"><span data-stu-id="5240b-141">RedirectResult - Represents a redirection to a new URL.</span></span>
4. <span data-ttu-id="5240b-142">JsonResult - stellt einen JavaScript Object Notation-Ergebnis, das in einer AJAX-Anwendung verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="5240b-142">JsonResult - Represents a JavaScript Object Notation result that can be used in an AJAX application.</span></span>
5. <span data-ttu-id="5240b-143">JavaScriptResult - stellt eine JavaScript-Skript dar.</span><span class="sxs-lookup"><span data-stu-id="5240b-143">JavaScriptResult - Represents a JavaScript script.</span></span>
6. <span data-ttu-id="5240b-144">ContentResult - stellt einen Textergebnis dar.</span><span class="sxs-lookup"><span data-stu-id="5240b-144">ContentResult - Represents a text result.</span></span>
7. <span data-ttu-id="5240b-145">FileContentResult - stellt eine herunterladbare Datei (mit dem binären Inhalt) dar.</span><span class="sxs-lookup"><span data-stu-id="5240b-145">FileContentResult - Represents a downloadable file (with the binary content).</span></span>
8. <span data-ttu-id="5240b-146">FilePathResult - stellt eine herunterladbare Datei (mit einem Pfad).</span><span class="sxs-lookup"><span data-stu-id="5240b-146">FilePathResult - Represents a downloadable file (with a path).</span></span>
9. <span data-ttu-id="5240b-147">FileStreamResult - stellt eine herunterladbare Datei (mit einem Dateistream) dar.</span><span class="sxs-lookup"><span data-stu-id="5240b-147">FileStreamResult - Represents a downloadable file (with a file stream).</span></span>

<span data-ttu-id="5240b-148">Alle diese Aktionsergebnisse erben von der ActionResult-Basisklasse.</span><span class="sxs-lookup"><span data-stu-id="5240b-148">All of these action results inherit from the base ActionResult class.</span></span>

<span data-ttu-id="5240b-149">In den meisten Fällen gibt eine Controlleraktion ein ViewResult zurück.</span><span class="sxs-lookup"><span data-stu-id="5240b-149">In most cases, a controller action returns a ViewResult.</span></span> <span data-ttu-id="5240b-150">Index-Controlleraktion Auflisten von 2 gibt beispielsweise ein ViewResult zurück.</span><span class="sxs-lookup"><span data-stu-id="5240b-150">For example, the Index controller action in Listing 2 returns a ViewResult.</span></span>

<span data-ttu-id="5240b-151">**Auflisten von 2 – Controllers\BookController.cs**</span><span class="sxs-lookup"><span data-stu-id="5240b-151">**Listing 2 - Controllers\BookController.cs**</span></span>

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample2.cs)]

<span data-ttu-id="5240b-152">Wenn eine Aktion ein ViewResult zurückgibt, wird HTML an den Browser zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="5240b-152">When an action returns a ViewResult, HTML is returned to the browser.</span></span> <span data-ttu-id="5240b-153">Die Index()-Methode im Codebeispiel 2 Gibt eine Ansicht mit dem Namen Index, an den Browser zurück.</span><span class="sxs-lookup"><span data-stu-id="5240b-153">The Index() method in Listing 2 returns a view named Index to the browser.</span></span>

<span data-ttu-id="5240b-154">Beachten Sie, dass die Aktion Index() Auflisten von 2 eine ViewResult() nicht zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="5240b-154">Notice that the Index() action in Listing 2 does not return a ViewResult().</span></span> <span data-ttu-id="5240b-155">Stattdessen wird die View()-Methode der Basisklasse Controller aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="5240b-155">Instead, the View() method of the Controller base class is called.</span></span> <span data-ttu-id="5240b-156">Normalerweise wird ein Aktionsergebnis nicht direkt zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="5240b-156">Normally, you do not return an action result directly.</span></span> <span data-ttu-id="5240b-157">Stattdessen rufen Sie eine der folgenden Methoden der Basisklasse Controller:</span><span class="sxs-lookup"><span data-stu-id="5240b-157">Instead, you call one of the following methods of the Controller base class:</span></span>

1. <span data-ttu-id="5240b-158">Anzeigen – gibt ein ViewResult-Aktion-Ergebnis zurück.</span><span class="sxs-lookup"><span data-stu-id="5240b-158">View - Returns a ViewResult action result.</span></span>
2. <span data-ttu-id="5240b-159">Umgeleitet – gibt ein RedirectResult-Aktion-Ergebnis zurück.</span><span class="sxs-lookup"><span data-stu-id="5240b-159">Redirect - Returns a RedirectResult action result.</span></span>
3. <span data-ttu-id="5240b-160">RedirectToAction - gibt ein Aktionsergebnis RedirectToRouteResult zurück.</span><span class="sxs-lookup"><span data-stu-id="5240b-160">RedirectToAction - Returns a RedirectToRouteResult action result.</span></span>
4. <span data-ttu-id="5240b-161">RedirectToRoute - gibt ein Aktionsergebnis RedirectToRouteResult zurück.</span><span class="sxs-lookup"><span data-stu-id="5240b-161">RedirectToRoute - Returns a RedirectToRouteResult action result.</span></span>
5. <span data-ttu-id="5240b-162">JSON - gibt ein JsonResult-Aktion-Ergebnis zurück.</span><span class="sxs-lookup"><span data-stu-id="5240b-162">Json - Returns a JsonResult action result.</span></span>
6. <span data-ttu-id="5240b-163">JavaScriptResult - gibt ein JavaScriptResult zurück.</span><span class="sxs-lookup"><span data-stu-id="5240b-163">JavaScriptResult - Returns a JavaScriptResult.</span></span>
7. <span data-ttu-id="5240b-164">Content - Returns a ContentResult action result.</span><span class="sxs-lookup"><span data-stu-id="5240b-164">Content - Returns a ContentResult action result.</span></span>
8. <span data-ttu-id="5240b-165">Datei - gibt ein FileContentResult, FilePathResult oder FileStreamResult abhängig von den Parametern an die Methode übergeben.</span><span class="sxs-lookup"><span data-stu-id="5240b-165">File - Returns a FileContentResult, FilePathResult, or FileStreamResult depending on the parameters passed to the method.</span></span>

<span data-ttu-id="5240b-166">Wenn Sie eine Sicht an den Browser zurückgeben möchten, rufen Sie deshalb die View()-Methode.</span><span class="sxs-lookup"><span data-stu-id="5240b-166">So, if you want to return a View to the browser, you call the View() method.</span></span> <span data-ttu-id="5240b-167">Wenn Sie den Benutzer eine Controlleraktion in eine andere umleiten möchten, rufen Sie die RedirectToAction()-Methode.</span><span class="sxs-lookup"><span data-stu-id="5240b-167">If you want to redirect the user from one controller action to another, you call the RedirectToAction() method.</span></span> <span data-ttu-id="5240b-168">Z. B. die Aktion Details() auflisten 3 zeigt eine Ansicht oder leitet den Benutzer an die Aktion Index(), je nachdem, ob die Id-Parameter einen Wert aufweist.</span><span class="sxs-lookup"><span data-stu-id="5240b-168">For example, the Details() action in Listing 3 either displays a view or redirects the user to the Index() action depending on whether the Id parameter has a value.</span></span>

<span data-ttu-id="5240b-169">**3 – CustomerController.cs auflisten**</span><span class="sxs-lookup"><span data-stu-id="5240b-169">**Listing 3 - CustomerController.cs**</span></span>

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample3.cs)]

<span data-ttu-id="5240b-170">Das Aktionsergebnis ContentResult ist ein Sonderfall.</span><span class="sxs-lookup"><span data-stu-id="5240b-170">The ContentResult action result is special.</span></span> <span data-ttu-id="5240b-171">Das Aktionsergebnis ContentResult können Sie ein Aktionsergebnis als nur-Text zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="5240b-171">You can use the ContentResult action result to return an action result as plain text.</span></span> <span data-ttu-id="5240b-172">Beispielsweise erfolgt die Methodenrückgabe Index() 4 Auflisten einer Nachricht als nur-Text und nicht als HTML.</span><span class="sxs-lookup"><span data-stu-id="5240b-172">For example, the Index() method in Listing 4 returns a message as plain text and not as HTML.</span></span>

<span data-ttu-id="5240b-173">**4 – Controllers\StatusController.cs auflisten**</span><span class="sxs-lookup"><span data-stu-id="5240b-173">**Listing 4 - Controllers\StatusController.cs**</span></span>

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample4.cs)]

<span data-ttu-id="5240b-174">Wenn die StatusController.Index() Aktion aufgerufen wird, wird eine Sicht nicht zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="5240b-174">When the StatusController.Index() action is invoked, a view is not returned.</span></span> <span data-ttu-id="5240b-175">Stattdessen den unformatierten Text "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="5240b-175">Instead, the raw text "Hello World!"</span></span> <span data-ttu-id="5240b-176">wird an den Browser zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="5240b-176">is returned to the browser.</span></span>

<span data-ttu-id="5240b-177">Wenn eine Controlleraktion als Ergebnis kein Aktionsergebnis - beispielsweise ein Datum oder eine ganze Zahl - zurückgegeben wird das Ergebnis automatisch in eine ContentResult eingebunden.</span><span class="sxs-lookup"><span data-stu-id="5240b-177">If a controller action returns a result that is not an action result - for example, a date or an integer - then the result is wrapped in a ContentResult automatically.</span></span> <span data-ttu-id="5240b-178">Z. B. wenn die Aktion Index() der WorkController auflisten 5 aufgerufen wird, wird das Datum als eine ContentResult automatisch zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="5240b-178">For example, when the Index() action of the WorkController in Listing 5 is invoked, the date is returned as a ContentResult automatically.</span></span>

<span data-ttu-id="5240b-179">**5 – WorkController.cs auflisten**</span><span class="sxs-lookup"><span data-stu-id="5240b-179">**Listing 5 - WorkController.cs**</span></span>

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample5.cs)]

<span data-ttu-id="5240b-180">Die Aktion Index() auflisten 5 zurückgegeben ein DateTime-Objekt.</span><span class="sxs-lookup"><span data-stu-id="5240b-180">The Index() action in Listing 5 returns a DateTime object.</span></span> <span data-ttu-id="5240b-181">ASP.NET MVC-Framework die DateTime-Objekt in eine Zeichenfolge konvertiert und dient als Wrapper für den DateTime-Wert in einer ContentResult automatisch.</span><span class="sxs-lookup"><span data-stu-id="5240b-181">The ASP.NET MVC framework converts the DateTime object to a string and wraps the DateTime value in a ContentResult automatically.</span></span> <span data-ttu-id="5240b-182">Der Browser empfängt das Datum und die Uhrzeit als nur-Text.</span><span class="sxs-lookup"><span data-stu-id="5240b-182">The browser receives the date and time as plain text.</span></span>

## <a name="summary"></a><span data-ttu-id="5240b-183">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="5240b-183">Summary</span></span>

<span data-ttu-id="5240b-184">Der Zweck dieses Lernprogramms war, Einführung in die Konzepte der ASP.NET MVC-Controller, Controlleraktionen und Controller Aktionsergebnisse.</span><span class="sxs-lookup"><span data-stu-id="5240b-184">The purpose of this tutorial was to introduce you to the concepts of ASP.NET MVC controllers, controller actions, and controller action results.</span></span> <span data-ttu-id="5240b-185">Im ersten Abschnitt haben Sie gelernt, wie neue Controller einer ASP.NET MVC-Projekt hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="5240b-185">In the first section, you learned how to add new controllers to an ASP.NET MVC project.</span></span> <span data-ttu-id="5240b-186">Als Nächstes haben Sie gelernt, wie öffentliche Methoden eines Controllers für die Universe als Controlleraktionen verfügbar gemacht.</span><span class="sxs-lookup"><span data-stu-id="5240b-186">Next, you learned how public methods of a controller are exposed to the universe as controller actions.</span></span> <span data-ttu-id="5240b-187">Schließlich erläutert die verschiedenen Arten von Aktionsergebnisse, die eine Controlleraktion zurückgegeben werden können.</span><span class="sxs-lookup"><span data-stu-id="5240b-187">Finally, we discussed the different types of action results that can be returned from a controller action.</span></span> <span data-ttu-id="5240b-188">Insbesondere besprochen haben wir wie ein ViewResult, RedirectToActionResult und ContentResult in eine Controlleraktion zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="5240b-188">In particular, we discussed how to return a ViewResult, RedirectToActionResult, and ContentResult from a controller action.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5240b-189">[Zurück](creating-an-action-vb.md)
> [Weiter](creating-custom-routes-cs.md)</span><span class="sxs-lookup"><span data-stu-id="5240b-189">[Previous](creating-an-action-vb.md)
[Next](creating-custom-routes-cs.md)</span></span>
