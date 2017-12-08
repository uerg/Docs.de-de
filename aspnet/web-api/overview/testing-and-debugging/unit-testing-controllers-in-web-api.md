---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: UnitTests Controller in ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: "Dieses Thema beschreibt einige spezifischen Verfahren für die Komponententests Controller in Web-API 2 an. Bevor Sie dieses Thema lesen, sollten Sie das Lernprogramm Einheit gelesen..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/11/2014
ms.topic: article
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 167cd24d27977c3652f6a8903054654f5edf7756
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="unit-testing-controllers-in-aspnet-web-api-2"></a><span data-ttu-id="a517e-104">UnitTests Controller in ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="a517e-104">Unit Testing Controllers in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="a517e-105">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a517e-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="a517e-106">Dieses Thema beschreibt einige spezifischen Verfahren für die Komponententests Controller in Web-API 2 an.</span><span class="sxs-lookup"><span data-stu-id="a517e-106">This topic describes some specific techniques for unit testing controllers in Web API 2.</span></span> <span data-ttu-id="a517e-107">Bevor Sie dieses Thema lesen, sollten, lesen das Lernprogramm [Einheit Testen von ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), die zeigt, wie ein Komponententest Projekt zur Projektmappe hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="a517e-107">Before reading this topic, you might want to read the tutorial [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), which shows how to add a unit-test project to your solution.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="a517e-108">In diesem Lernprogramm verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="a517e-108">Software versions used in the tutorial</span></span>
> 
> - [<span data-ttu-id="a517e-109">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="a517e-109">Visual Studio 2017</span></span>](https://www.visualstudio.com/vs/)
> - <span data-ttu-id="a517e-110">Web-API 2</span><span class="sxs-lookup"><span data-stu-id="a517e-110">Web API 2</span></span>
> - <span data-ttu-id="a517e-111">[Moq](https://github.com/Moq) 4.5.30</span><span class="sxs-lookup"><span data-stu-id="a517e-111">[Moq](https://github.com/Moq) 4.5.30</span></span>

> [!NOTE]
> <span data-ttu-id="a517e-112">Ich Moq verwendet, aber das gleiche Prinzip gilt für alle pseudoframework.</span><span class="sxs-lookup"><span data-stu-id="a517e-112">I used Moq, but the same idea applies to any mocking framework.</span></span> <span data-ttu-id="a517e-113">Moq 4.5.30 (und höher) unterstützt Visual Studio 2017, Roslyn und .NET 4.5 und höher.</span><span class="sxs-lookup"><span data-stu-id="a517e-113">Moq 4.5.30 (and later) supports Visual Studio 2017, Roslyn and .NET 4.5 and later versions.</span></span>

<span data-ttu-id="a517e-114">Ist ein allgemeines Muster in Komponententests &quot;anordnen-Act-assert&quot;:</span><span class="sxs-lookup"><span data-stu-id="a517e-114">A common pattern in unit tests is &quot;arrange-act-assert&quot;:</span></span>

- <span data-ttu-id="a517e-115">Anordnen: Richten Sie alle erforderlichen Komponenten für den Test ausführen.</span><span class="sxs-lookup"><span data-stu-id="a517e-115">Arrange: Set up any prerequisites for the test to run.</span></span>
- <span data-ttu-id="a517e-116">ACT: Führen Sie den Test.</span><span class="sxs-lookup"><span data-stu-id="a517e-116">Act: Perform the test.</span></span>
- <span data-ttu-id="a517e-117">Assert-: Stellen Sie sicher, dass der Test erfolgreich war.</span><span class="sxs-lookup"><span data-stu-id="a517e-117">Assert: Verify that the test succeeded.</span></span>

<span data-ttu-id="a517e-118">In den Diagrammfelder Schritt wird häufig Mock oder stub-Objekte.</span><span class="sxs-lookup"><span data-stu-id="a517e-118">In the arrange step, you will often use mock or stub objects.</span></span> <span data-ttu-id="a517e-119">Die minimiert die Anzahl der Abhängigkeiten, damit der Test, zum Testen von eins ausgerichtet ist.</span><span class="sxs-lookup"><span data-stu-id="a517e-119">That minimizes the number of dependencies, so the test is focused on testing one thing.</span></span>

<span data-ttu-id="a517e-120">Hier sind einige Punkte sollten Sie Komponententests in Ihre Web-API-Controller aus:</span><span class="sxs-lookup"><span data-stu-id="a517e-120">Here are some things that you should unit test in your Web API controllers:</span></span>

- <span data-ttu-id="a517e-121">Die Aktion gibt den richtigen Typ der Antwort zurück.</span><span class="sxs-lookup"><span data-stu-id="a517e-121">The action returns the correct type of response.</span></span>
- <span data-ttu-id="a517e-122">Ungültige Parameter zurück, die richtige Fehlerantwort.</span><span class="sxs-lookup"><span data-stu-id="a517e-122">Invalid parameters return the correct error response.</span></span>
- <span data-ttu-id="a517e-123">Die Aktion ruft die richtige Methode auf der Ebene Repositorys oder Diensts an.</span><span class="sxs-lookup"><span data-stu-id="a517e-123">The action calls the correct method on the repository or service layer.</span></span>
- <span data-ttu-id="a517e-124">Enthält die Antwort ein Domänenmodell, überprüfen Sie den Modelltyp.</span><span class="sxs-lookup"><span data-stu-id="a517e-124">If the response includes a domain model, verify the model type.</span></span>

<span data-ttu-id="a517e-125">Dies sind einige der allgemeinen Schritte zum Testen, aber die Einzelheiten hängen von der Controller-Implementierung.</span><span class="sxs-lookup"><span data-stu-id="a517e-125">These are some of the general things to test, but the specifics depend on your controller implementation.</span></span> <span data-ttu-id="a517e-126">Insbesondere vereinfacht einen großen Unterschied, ob Ihre Controlleraktionen zurückgeben **HttpResponseMessage** oder **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="a517e-126">In particular, it makes a big difference whether your controller actions return **HttpResponseMessage** or **IHttpActionResult**.</span></span> <span data-ttu-id="a517e-127">Weitere Informationen zu dieser Ergebnistypen, finden Sie unter [Aktionsergebnisse in Web-Api 2](../getting-started-with-aspnet-web-api/action-results.md).</span><span class="sxs-lookup"><span data-stu-id="a517e-127">For more information about these result types, see [Action Results in Web Api 2](../getting-started-with-aspnet-web-api/action-results.md).</span></span>

## <a name="testing-actions-that-return-httpresponsemessage"></a><span data-ttu-id="a517e-128">Testen von Aktionen, die HttpResponseMessage zurück.</span><span class="sxs-lookup"><span data-stu-id="a517e-128">Testing Actions that Return HttpResponseMessage</span></span>

<span data-ttu-id="a517e-129">Hier ist ein Beispiel eines Controllers, deren Rückgabetyp Aktionen **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="a517e-129">Here is an example of a controller whose actions return **HttpResponseMessage**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

<span data-ttu-id="a517e-130">Beachten Sie, dass der Controller verwendet Abhängigkeitsinjektion zum Einfügen einer `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="a517e-130">Notice the controller uses dependency injection to inject an `IProductRepository`.</span></span> <span data-ttu-id="a517e-131">Stellt den Controller mehr getestet werden können, da Sie eine simulierte Repository einfügen können.</span><span class="sxs-lookup"><span data-stu-id="a517e-131">That makes the controller more testable, because you can inject a mock repository.</span></span> <span data-ttu-id="a517e-132">Im folgenden UnitTest stellt sicher, dass die `Get` -Methode schreibt eine `Product` in den Antworttext.</span><span class="sxs-lookup"><span data-stu-id="a517e-132">The following unit test verifies that the `Get` method writes a `Product` to the response body.</span></span> <span data-ttu-id="a517e-133">Angenommen, `repository` ist ein Mock `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="a517e-133">Assume that `repository` is a mock `IProductRepository`.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

<span data-ttu-id="a517e-134">Es ist wichtig, legen Sie **anfordern** und **Konfiguration** auf dem Controller.</span><span class="sxs-lookup"><span data-stu-id="a517e-134">It's important to set **Request** and **Configuration** on the controller.</span></span> <span data-ttu-id="a517e-135">Andernfalls schlägt der Test fehl, mit einer **ArgumentNullException** oder **InvalidOperationException**.</span><span class="sxs-lookup"><span data-stu-id="a517e-135">Otherwise, the test will fail with an **ArgumentNullException** or **InvalidOperationException**.</span></span>

## <a name="testing-link-generation"></a><span data-ttu-id="a517e-136">Testen die Linkgenerierung</span><span class="sxs-lookup"><span data-stu-id="a517e-136">Testing Link Generation</span></span>

<span data-ttu-id="a517e-137">Die `Post` Methodenaufrufe **UrlHelper.Link** zum Erstellen von Links in der Antwort.</span><span class="sxs-lookup"><span data-stu-id="a517e-137">The `Post` method calls **UrlHelper.Link** to create links in the response.</span></span> <span data-ttu-id="a517e-138">Dies erfordert ein wenig mehr Setup im Komponententest:</span><span class="sxs-lookup"><span data-stu-id="a517e-138">This requires a little more setup in the unit test:</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

<span data-ttu-id="a517e-139">Die **UrlHelper** Klasse benötigt der Anforderungsdaten URL und die Routenwerte verwendet werden, weshalb der Test für diese Werte festgelegt.</span><span class="sxs-lookup"><span data-stu-id="a517e-139">The **UrlHelper** class needs the request URL and route data, so the test has to set values for these.</span></span> <span data-ttu-id="a517e-140">Eine andere Möglichkeit besteht, Mock oder Stub **UrlHelper**.</span><span class="sxs-lookup"><span data-stu-id="a517e-140">Another option is mock or stub **UrlHelper**.</span></span> <span data-ttu-id="a517e-141">Bei diesem Ansatz, ersetzen Sie den Standardwert [ApiController.Url](https://msdn.microsoft.com/en-us/library/system.web.http.apicontroller.url.aspx) mit einer Mock oder Stub-Version, die einen festen Wert zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="a517e-141">With this approach, you replace the default value of [ApiController.Url](https://msdn.microsoft.com/en-us/library/system.web.http.apicontroller.url.aspx) with a mock or stub version that returns a fixed value.</span></span>

<span data-ttu-id="a517e-142">Wir schreiben Sie den Test mit der [Moq](https://github.com/Moq) Framework.</span><span class="sxs-lookup"><span data-stu-id="a517e-142">Let's rewrite the test using the [Moq](https://github.com/Moq) framework.</span></span> <span data-ttu-id="a517e-143">Installieren der `Moq` NuGet-Paket im Testprojekt befindet.</span><span class="sxs-lookup"><span data-stu-id="a517e-143">Install the `Moq` NuGet package in the test project.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

<span data-ttu-id="a517e-144">In dieser Version nicht müssen Sie zum Einrichten der Routendaten, da das Mock **UrlHelper** gibt eine Konstante Zeichenfolge zurück.</span><span class="sxs-lookup"><span data-stu-id="a517e-144">In this version, you don't need to set up any route data, because the mock **UrlHelper** returns a constant string.</span></span>


## <a name="testing-actions-that-return-ihttpactionresult"></a><span data-ttu-id="a517e-145">Testen von Aktionen, die IHttpActionResult zurückgeben</span><span class="sxs-lookup"><span data-stu-id="a517e-145">Testing Actions that Return IHttpActionResult</span></span>

<span data-ttu-id="a517e-146">In der Web-API 2, kann eine Controlleraktion zurückgeben **IHttpActionResult**, dies ist analog zu **ActionResult** in ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="a517e-146">In Web API 2, a controller action can return **IHttpActionResult**, which is analogous to **ActionResult** in ASP.NET MVC.</span></span> <span data-ttu-id="a517e-147">Die **IHttpActionResult** Schnittstelle definiert einen Befehlsmuster zum Erstellen von HTTP-Antworten.</span><span class="sxs-lookup"><span data-stu-id="a517e-147">The **IHttpActionResult** interface defines a command pattern for creating HTTP responses.</span></span> <span data-ttu-id="a517e-148">Anstelle die Antwort direkt erstellen, gibt der Controller eine **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="a517e-148">Instead of creating the response directly, the controller returns an **IHttpActionResult**.</span></span> <span data-ttu-id="a517e-149">Die Pipeline später, ruft der **IHttpActionResult** um die Antwort zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="a517e-149">Later, the pipeline invokes the **IHttpActionResult** to create the response.</span></span> <span data-ttu-id="a517e-150">Dieser Ansatz erleichtert es, Schreiben von Komponententests, da Sie viele der Setup-, die überspringen können für die benötigte **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="a517e-150">This approach makes it easier to write unit tests, because you can skip a lot of the setup that is needed for **HttpResponseMessage**.</span></span>

<span data-ttu-id="a517e-151">Hier ist ein Beispiel-Controller, deren Rückgabetyp Aktionen **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="a517e-151">Here is an example controller whose actions return **IHttpActionResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

<span data-ttu-id="a517e-152">Dieses Beispiel zeigt einige allgemeine Muster mit **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="a517e-152">This example shows some common patterns using **IHttpActionResult**.</span></span> <span data-ttu-id="a517e-153">Sehen wir, wie Einheit testen.</span><span class="sxs-lookup"><span data-stu-id="a517e-153">Let's see how to unit test them.</span></span>

### <a name="action-returns-200-ok-with-a-response-body"></a><span data-ttu-id="a517e-154">Aktion gibt 200 (OK) mit einem Antworttext zurück.</span><span class="sxs-lookup"><span data-stu-id="a517e-154">Action returns 200 (OK) with a response body</span></span>

<span data-ttu-id="a517e-155">Die `Get` Methodenaufrufe `Ok(product)` Wenn das Produkt gefunden wurde.</span><span class="sxs-lookup"><span data-stu-id="a517e-155">The `Get` method calls `Ok(product)` if the product is found.</span></span> <span data-ttu-id="a517e-156">Im Komponententest, stellen Sie sicher, dass der Rückgabetyp ist **OkNegotiatedContentResult** und das zurückgegebene Produkt verfügt über die richtigen-ID.</span><span class="sxs-lookup"><span data-stu-id="a517e-156">In the unit test, make sure the return type is **OkNegotiatedContentResult** and the returned product has the right ID.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

<span data-ttu-id="a517e-157">Beachten Sie, dass der Komponententest das Aktionsergebnis ausgeführt wird nicht.</span><span class="sxs-lookup"><span data-stu-id="a517e-157">Notice that the unit test doesn't execute the action result.</span></span> <span data-ttu-id="a517e-158">Sie können davon ausgehen, dass das Aktionsergebnis die HTTP-Antwort ordnungsgemäß erstellt.</span><span class="sxs-lookup"><span data-stu-id="a517e-158">You can assume the action result creates the HTTP response correctly.</span></span> <span data-ttu-id="a517e-159">(Diesem Grund wird die Web-API-Framework verfügt über einen eigenen Komponententests!)</span><span class="sxs-lookup"><span data-stu-id="a517e-159">(That's why the Web API framework has its own unit tests!)</span></span>

### <a name="action-returns-404-not-found"></a><span data-ttu-id="a517e-160">Aktion gibt 404 (Nichtgefunden) zurück.</span><span class="sxs-lookup"><span data-stu-id="a517e-160">Action returns 404 (Not Found)</span></span>

<span data-ttu-id="a517e-161">Die `Get` Methodenaufrufe `NotFound()` Wenn das Produkt nicht gefunden wird.</span><span class="sxs-lookup"><span data-stu-id="a517e-161">The `Get` method calls `NotFound()` if the product is not found.</span></span> <span data-ttu-id="a517e-162">Für diesen Fall den Komponententest nur überprüft, wenn der Rückgabetyp ist **NotFoundResult**.</span><span class="sxs-lookup"><span data-stu-id="a517e-162">For this case, the unit test just checks if the return type is **NotFoundResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a><span data-ttu-id="a517e-163">Aktion gibt 200 (OK) mit keinen Antworttext zurück.</span><span class="sxs-lookup"><span data-stu-id="a517e-163">Action returns 200 (OK) with no response body</span></span>

<span data-ttu-id="a517e-164">Die `Delete` Methodenaufrufe `Ok()` leerantwort HTTP 200 zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="a517e-164">The `Delete` method calls `Ok()` to return an empty HTTP 200 response.</span></span> <span data-ttu-id="a517e-165">Wie im vorherigen Beispiel überprüft der Komponententest den Rückgabetyp, in diesem Fall **OkResult**.</span><span class="sxs-lookup"><span data-stu-id="a517e-165">Like the previous example, the unit test checks the return type, in this case **OkResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a><span data-ttu-id="a517e-166">Aktion gibt 201 (erstellt) mit einem Location-Header zurück.</span><span class="sxs-lookup"><span data-stu-id="a517e-166">Action returns 201 (Created) with a Location header</span></span>

<span data-ttu-id="a517e-167">Die `Post` Methodenaufrufe `CreatedAtRoute` eine Antwort "HTTP 201" mit einem URI im Location-Header zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="a517e-167">The `Post` method calls `CreatedAtRoute` to return an HTTP 201 response with a URI in the Location header.</span></span> <span data-ttu-id="a517e-168">In den Komponententest stellen Sie sicher, dass die Aktion die richtigen routing Werte festlegt.</span><span class="sxs-lookup"><span data-stu-id="a517e-168">In the unit test, verify that the action sets the correct routing values.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a><span data-ttu-id="a517e-169">Aktion gibt eine andere 2xx mit einen Antworttext zurück.</span><span class="sxs-lookup"><span data-stu-id="a517e-169">Action returns another 2xx with a response body</span></span>

<span data-ttu-id="a517e-170">Die `Put` Methodenaufrufe `Content` zum Zurückgeben einer Antwort HTTP 202 (akzeptiert) mit einen Antworttext.</span><span class="sxs-lookup"><span data-stu-id="a517e-170">The `Put` method calls `Content` to return an HTTP 202 (Accepted) response with a response body.</span></span> <span data-ttu-id="a517e-171">Dies der Fall ist ähnlich wie 200 (OK) zurückgegeben, sondern der Komponententest sollte auch den Statuscode überprüfen.</span><span class="sxs-lookup"><span data-stu-id="a517e-171">This case is similar to returning 200 (OK), but the unit test should also check the status code.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a><span data-ttu-id="a517e-172">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="a517e-172">Additional Resources</span></span>

- [<span data-ttu-id="a517e-173">Simulieren von Entity Framework bei Komponententests für ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="a517e-173">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- <span data-ttu-id="a517e-174">[Schreiben von Tests für ASP.NET Web API-Dienst](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (Blogbeitrag vom Youssef Moussaoui).</span><span class="sxs-lookup"><span data-stu-id="a517e-174">[Writing tests for an ASP.NET Web API service](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (blog post by Youssef Moussaoui).</span></span>
- [<span data-ttu-id="a517e-175">Debuggen von ASP.NET Web-API mit dem Routendebugger</span><span class="sxs-lookup"><span data-stu-id="a517e-175">Debugging ASP.NET Web API with Route Debugger</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
