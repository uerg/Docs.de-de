---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: Komponententests für Controller in ASP.NET Web-API 2 | Microsoft-Dokumentation
author: MikeWasson
description: Dieses Thema beschreibt, welche Techniken für Komponententests für Controller in Web-API 2. Bevor Sie dieses Thema lesen, sollten Sie das Tutorial Einheit zu lesen...
ms.author: riande
ms.date: 06/11/2014
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: e1bb1aa120ced95db7674eae1831f2a2c7356fc0
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48794823"
---
<a name="unit-testing-controllers-in-aspnet-web-api-2"></a><span data-ttu-id="1abb6-104">Komponententests für Controller in ASP.NET Web-API 2</span><span class="sxs-lookup"><span data-stu-id="1abb6-104">Unit Testing Controllers in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="1abb6-105">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1abb6-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="1abb6-106">Dieses Thema beschreibt, welche Techniken für Komponententests für Controller in Web-API 2.</span><span class="sxs-lookup"><span data-stu-id="1abb6-106">This topic describes some specific techniques for unit testing controllers in Web API 2.</span></span> <span data-ttu-id="1abb6-107">Bevor Sie dieses Thema lesen, sollten, lesen das Tutorial [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), erfahren, wie Sie ein Komponententestprojekt der Projektmappe hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="1abb6-107">Before reading this topic, you might want to read the tutorial [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), which shows how to add a unit-test project to your solution.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="1abb6-108">Softwareversionen, die in diesem Tutorial verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="1abb6-108">Software versions used in the tutorial</span></span>
>
> - [<span data-ttu-id="1abb6-109">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="1abb6-109">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - <span data-ttu-id="1abb6-110">Web-API 2</span><span class="sxs-lookup"><span data-stu-id="1abb6-110">Web API 2</span></span>
> - <span data-ttu-id="1abb6-111">[Moq](https://github.com/Moq) 4.5.30</span><span class="sxs-lookup"><span data-stu-id="1abb6-111">[Moq](https://github.com/Moq) 4.5.30</span></span>

> [!NOTE]
> <span data-ttu-id="1abb6-112">Ich habe die Moq verwendet, aber das gleiche Prinzip gilt für alle Mockframework.</span><span class="sxs-lookup"><span data-stu-id="1abb6-112">I used Moq, but the same idea applies to any mocking framework.</span></span> <span data-ttu-id="1abb6-113">Moq 4.5.30 (und höher) unterstützt Visual Studio 2017, Roslyn und .NET 4.5 und höheren Versionen.</span><span class="sxs-lookup"><span data-stu-id="1abb6-113">Moq 4.5.30 (and later) supports Visual Studio 2017, Roslyn and .NET 4.5 and later versions.</span></span>

<span data-ttu-id="1abb6-114">Ein häufiges Muster in Komponententests ist &quot;anordnen-Act-assert&quot;:</span><span class="sxs-lookup"><span data-stu-id="1abb6-114">A common pattern in unit tests is &quot;arrange-act-assert&quot;:</span></span>

- <span data-ttu-id="1abb6-115">Anordnen: Richten Sie alle Voraussetzungen für den Test ausführen.</span><span class="sxs-lookup"><span data-stu-id="1abb6-115">Arrange: Set up any prerequisites for the test to run.</span></span>
- <span data-ttu-id="1abb6-116">ACT: Führen Sie den Test.</span><span class="sxs-lookup"><span data-stu-id="1abb6-116">Act: Perform the test.</span></span>
- <span data-ttu-id="1abb6-117">Assert-: Stellen Sie sicher, dass der Test erfolgreich war.</span><span class="sxs-lookup"><span data-stu-id="1abb6-117">Assert: Verify that the test succeeded.</span></span>

<span data-ttu-id="1abb6-118">Im Schritt anordnen wird häufig Mock oder stub-Objekte.</span><span class="sxs-lookup"><span data-stu-id="1abb6-118">In the arrange step, you will often use mock or stub objects.</span></span> <span data-ttu-id="1abb6-119">Das minimiert der Anzahl der Abhängigkeiten, damit der Test auf einen Aspekt orientiert.</span><span class="sxs-lookup"><span data-stu-id="1abb6-119">That minimizes the number of dependencies, so the test is focused on testing one thing.</span></span>

<span data-ttu-id="1abb6-120">Hier sind einige Dinge, die Sie in Ihren Web-API-Controllern Komponententests sollten:</span><span class="sxs-lookup"><span data-stu-id="1abb6-120">Here are some things that you should unit test in your Web API controllers:</span></span>

- <span data-ttu-id="1abb6-121">Die Aktion gibt die richtige Art der Antwort zurück.</span><span class="sxs-lookup"><span data-stu-id="1abb6-121">The action returns the correct type of response.</span></span>
- <span data-ttu-id="1abb6-122">Ungültige Parameter geben die richtige Fehlermeldung.</span><span class="sxs-lookup"><span data-stu-id="1abb6-122">Invalid parameters return the correct error response.</span></span>
- <span data-ttu-id="1abb6-123">Die Aktion ruft die richtige Methode auf der Ebene Repository oder den Dienst an.</span><span class="sxs-lookup"><span data-stu-id="1abb6-123">The action calls the correct method on the repository or service layer.</span></span>
- <span data-ttu-id="1abb6-124">Wenn die Antwort ein Domänenmodell enthält, überprüfen Sie den Typ des Modells.</span><span class="sxs-lookup"><span data-stu-id="1abb6-124">If the response includes a domain model, verify the model type.</span></span>

<span data-ttu-id="1abb6-125">Dies sind einige allgemeine Dinge zu testen, aber die Details hängen von der controllerimplementierung.</span><span class="sxs-lookup"><span data-stu-id="1abb6-125">These are some of the general things to test, but the specifics depend on your controller implementation.</span></span> <span data-ttu-id="1abb6-126">Insbesondere macht es einen großen Unterschied, ob Ihre Controlleraktionen zurückgeben **HttpResponseMessage** oder **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="1abb6-126">In particular, it makes a big difference whether your controller actions return **HttpResponseMessage** or **IHttpActionResult**.</span></span> <span data-ttu-id="1abb6-127">Weitere Informationen zu dieser Ergebnistypen, finden Sie unter [Aktionsergebnisse in Web-Api 2](../getting-started-with-aspnet-web-api/action-results.md).</span><span class="sxs-lookup"><span data-stu-id="1abb6-127">For more information about these result types, see [Action Results in Web Api 2](../getting-started-with-aspnet-web-api/action-results.md).</span></span>

## <a name="testing-actions-that-return-httpresponsemessage"></a><span data-ttu-id="1abb6-128">Testen von Aktionen, die HttpResponseMessage zurückgegeben</span><span class="sxs-lookup"><span data-stu-id="1abb6-128">Testing Actions that Return HttpResponseMessage</span></span>

<span data-ttu-id="1abb6-129">Hier ist ein Beispiel für einen Controller, deren Rückgabetyp Aktionen **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="1abb6-129">Here is an example of a controller whose actions return **HttpResponseMessage**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

<span data-ttu-id="1abb6-130">Beachten Sie, dass der Controller verwendet abhängigkeitseinfügung zum Einfügen einer `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="1abb6-130">Notice the controller uses dependency injection to inject an `IProductRepository`.</span></span> <span data-ttu-id="1abb6-131">Das kann für den Controller besser getestet werden, da Sie eine simulierte Repository einfügen können.</span><span class="sxs-lookup"><span data-stu-id="1abb6-131">That makes the controller more testable, because you can inject a mock repository.</span></span> <span data-ttu-id="1abb6-132">Im folgenden UnitTest überprüft, ob die `Get` -Methode schreibt eine `Product` in den Antworttext.</span><span class="sxs-lookup"><span data-stu-id="1abb6-132">The following unit test verifies that the `Get` method writes a `Product` to the response body.</span></span> <span data-ttu-id="1abb6-133">Vorausgesetzt, dass `repository` ist ein Mock `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="1abb6-133">Assume that `repository` is a mock `IProductRepository`.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

<span data-ttu-id="1abb6-134">Es ist wichtig, legen Sie **anfordern** und **Konfiguration** auf dem Controller.</span><span class="sxs-lookup"><span data-stu-id="1abb6-134">It's important to set **Request** and **Configuration** on the controller.</span></span> <span data-ttu-id="1abb6-135">Andernfalls schlägt der Test fehl, mit einem **"ArgumentNullException"** oder **"InvalidOperationException"**.</span><span class="sxs-lookup"><span data-stu-id="1abb6-135">Otherwise, the test will fail with an **ArgumentNullException** or **InvalidOperationException**.</span></span>

## <a name="testing-link-generation"></a><span data-ttu-id="1abb6-136">Erstellen von links testen</span><span class="sxs-lookup"><span data-stu-id="1abb6-136">Testing Link Generation</span></span>

<span data-ttu-id="1abb6-137">Die `Post` Methodenaufrufe **UrlHelper.Link** Links in der Antwort zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="1abb6-137">The `Post` method calls **UrlHelper.Link** to create links in the response.</span></span> <span data-ttu-id="1abb6-138">Dies erfordert etwas mehr Einstellungen im Komponententest:</span><span class="sxs-lookup"><span data-stu-id="1abb6-138">This requires a little more setup in the unit test:</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

<span data-ttu-id="1abb6-139">Die **UrlHelper** Klasse benötigt die Anforderungsdaten-URL und der Route, damit der Test Werte für diese festlegen muss.</span><span class="sxs-lookup"><span data-stu-id="1abb6-139">The **UrlHelper** class needs the request URL and route data, so the test has to set values for these.</span></span> <span data-ttu-id="1abb6-140">Eine andere Möglichkeit besteht, Mock oder Stub **UrlHelper**.</span><span class="sxs-lookup"><span data-stu-id="1abb6-140">Another option is mock or stub **UrlHelper**.</span></span> <span data-ttu-id="1abb6-141">Bei diesem Ansatz ersetzen Sie den Standardwert [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) mit einer Simulation oder Stub-Version, die einen festen Wert zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="1abb6-141">With this approach, you replace the default value of [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) with a mock or stub version that returns a fixed value.</span></span>

<span data-ttu-id="1abb6-142">Ändern wir den Test mit der [Moq](https://github.com/Moq) Framework.</span><span class="sxs-lookup"><span data-stu-id="1abb6-142">Let's rewrite the test using the [Moq](https://github.com/Moq) framework.</span></span> <span data-ttu-id="1abb6-143">Installieren Sie die `Moq` NuGet-Paket im Projekt.</span><span class="sxs-lookup"><span data-stu-id="1abb6-143">Install the `Moq` NuGet package in the test project.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

<span data-ttu-id="1abb6-144">In dieser Version nicht erforderlich, um alle Routendaten einzurichten da das Mock **UrlHelper** gibt eine Konstante Zeichenfolge zurück.</span><span class="sxs-lookup"><span data-stu-id="1abb6-144">In this version, you don't need to set up any route data, because the mock **UrlHelper** returns a constant string.</span></span>


## <a name="testing-actions-that-return-ihttpactionresult"></a><span data-ttu-id="1abb6-145">Testen von Aktionen, die IHttpActionResult zurückgeben</span><span class="sxs-lookup"><span data-stu-id="1abb6-145">Testing Actions that Return IHttpActionResult</span></span>

<span data-ttu-id="1abb6-146">In Web-API 2, kann eine Controlleraktion zurückgeben **IHttpActionResult**, dies ist analog zu **ActionResult** in ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="1abb6-146">In Web API 2, a controller action can return **IHttpActionResult**, which is analogous to **ActionResult** in ASP.NET MVC.</span></span> <span data-ttu-id="1abb6-147">Die **IHttpActionResult** Schnittstelle definiert ein Befehlsmuster zum Erstellen von HTTP-Antworten.</span><span class="sxs-lookup"><span data-stu-id="1abb6-147">The **IHttpActionResult** interface defines a command pattern for creating HTTP responses.</span></span> <span data-ttu-id="1abb6-148">Anstatt die Antwort direkt erstellen, der Controller gibt eine **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="1abb6-148">Instead of creating the response directly, the controller returns an **IHttpActionResult**.</span></span> <span data-ttu-id="1abb6-149">Später, der die Pipeline aufruft der **IHttpActionResult** zum Erstellen der Antwort.</span><span class="sxs-lookup"><span data-stu-id="1abb6-149">Later, the pipeline invokes the **IHttpActionResult** to create the response.</span></span> <span data-ttu-id="1abb6-150">Dieser Ansatz erleichtert es, Schreiben von Komponententests, da Sie einen Großteil der Einrichtung, die überspringen können für die benötigte **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="1abb6-150">This approach makes it easier to write unit tests, because you can skip a lot of the setup that is needed for **HttpResponseMessage**.</span></span>

<span data-ttu-id="1abb6-151">Hier ist ein Beispiel-Controller, deren Rückgabetyp Aktionen **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="1abb6-151">Here is an example controller whose actions return **IHttpActionResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

<span data-ttu-id="1abb6-152">Dieses Beispiel zeigt einige allgemeine Muster, die mit **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="1abb6-152">This example shows some common patterns using **IHttpActionResult**.</span></span> <span data-ttu-id="1abb6-153">Sehen wir uns an wie Einheit testen.</span><span class="sxs-lookup"><span data-stu-id="1abb6-153">Let's see how to unit test them.</span></span>

### <a name="action-returns-200-ok-with-a-response-body"></a><span data-ttu-id="1abb6-154">Aktion gibt 200 (OK) mit einem Antworttext zurück.</span><span class="sxs-lookup"><span data-stu-id="1abb6-154">Action returns 200 (OK) with a response body</span></span>

<span data-ttu-id="1abb6-155">Die `Get` Methodenaufrufe `Ok(product)` Wenn das Produkt gefunden wird.</span><span class="sxs-lookup"><span data-stu-id="1abb6-155">The `Get` method calls `Ok(product)` if the product is found.</span></span> <span data-ttu-id="1abb6-156">Stellen Sie sicher, dass der Rückgabetyp ist im Komponententest, **OkNegotiatedContentResult** und das zurückgegebene Produkt verfügt über die richtigen-ID.</span><span class="sxs-lookup"><span data-stu-id="1abb6-156">In the unit test, make sure the return type is **OkNegotiatedContentResult** and the returned product has the right ID.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

<span data-ttu-id="1abb6-157">Beachten Sie, dass der Komponententest nicht das Aktionsergebnis ausführt.</span><span class="sxs-lookup"><span data-stu-id="1abb6-157">Notice that the unit test doesn't execute the action result.</span></span> <span data-ttu-id="1abb6-158">Sie können davon ausgehen, dass das Aktionsergebnis die HTTP-Antwort ordnungsgemäß erstellt.</span><span class="sxs-lookup"><span data-stu-id="1abb6-158">You can assume the action result creates the HTTP response correctly.</span></span> <span data-ttu-id="1abb6-159">(Daher der Web-API-Framework verfügt über eine eigene Komponententests!)</span><span class="sxs-lookup"><span data-stu-id="1abb6-159">(That's why the Web API framework has its own unit tests!)</span></span>

### <a name="action-returns-404-not-found"></a><span data-ttu-id="1abb6-160">Aktion gibt 404 (nicht gefunden) zurück.</span><span class="sxs-lookup"><span data-stu-id="1abb6-160">Action returns 404 (Not Found)</span></span>

<span data-ttu-id="1abb6-161">Die `Get` Methodenaufrufe `NotFound()` Wenn das Produkt nicht gefunden wird.</span><span class="sxs-lookup"><span data-stu-id="1abb6-161">The `Get` method calls `NotFound()` if the product is not found.</span></span> <span data-ttu-id="1abb6-162">Für diesen Fall den Komponententest nur überprüft, wenn der Rückgabetyp ist **NotFoundResult**.</span><span class="sxs-lookup"><span data-stu-id="1abb6-162">For this case, the unit test just checks if the return type is **NotFoundResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a><span data-ttu-id="1abb6-163">Aktion gibt 200 (OK) ohne Antworttext zurück.</span><span class="sxs-lookup"><span data-stu-id="1abb6-163">Action returns 200 (OK) with no response body</span></span>

<span data-ttu-id="1abb6-164">Die `Delete` Methodenaufrufe `Ok()` eine leere HTTP 200-Antwort zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="1abb6-164">The `Delete` method calls `Ok()` to return an empty HTTP 200 response.</span></span> <span data-ttu-id="1abb6-165">Wie im vorherigen Beispiel, überprüft der UnitTest den Rückgabetyp, in diesem Fall **OkResult**.</span><span class="sxs-lookup"><span data-stu-id="1abb6-165">Like the previous example, the unit test checks the return type, in this case **OkResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a><span data-ttu-id="1abb6-166">Aktion gibt 201 (erstellt) mit einem Location-Header zurück.</span><span class="sxs-lookup"><span data-stu-id="1abb6-166">Action returns 201 (Created) with a Location header</span></span>

<span data-ttu-id="1abb6-167">Die `Post` Methodenaufrufe `CreatedAtRoute` eine HTTP 201-Antwort mit einem URI im Location-Header zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="1abb6-167">The `Post` method calls `CreatedAtRoute` to return an HTTP 201 response with a URI in the Location header.</span></span> <span data-ttu-id="1abb6-168">Im Komponententest stellen Sie sicher, dass die Aktion, die richtigen routing Werte festlegt.</span><span class="sxs-lookup"><span data-stu-id="1abb6-168">In the unit test, verify that the action sets the correct routing values.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a><span data-ttu-id="1abb6-169">Aktion gibt eine andere 2xx mit einem Antworttext zurück.</span><span class="sxs-lookup"><span data-stu-id="1abb6-169">Action returns another 2xx with a response body</span></span>

<span data-ttu-id="1abb6-170">Die `Put` Methodenaufrufe `Content` eine HTTP 202 (akzeptiert)-Antwort mit einem Antworttext zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="1abb6-170">The `Put` method calls `Content` to return an HTTP 202 (Accepted) response with a response body.</span></span> <span data-ttu-id="1abb6-171">Diesen Fall ähnelt der 200 (OK) zurückgeben, aber der Komponententest sollten auch den Statuscode überprüfen.</span><span class="sxs-lookup"><span data-stu-id="1abb6-171">This case is similar to returning 200 (OK), but the unit test should also check the status code.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a><span data-ttu-id="1abb6-172">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="1abb6-172">Additional Resources</span></span>

- [<span data-ttu-id="1abb6-173">Simulieren des Entitätsframework bei Komponententests für ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="1abb6-173">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- <span data-ttu-id="1abb6-174">[Schreiben von Tests für eine ASP.NET Web-API-Dienst](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (Blogbeitrag vom Youssef Moussaoui).</span><span class="sxs-lookup"><span data-stu-id="1abb6-174">[Writing tests for an ASP.NET Web API service](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (blog post by Youssef Moussaoui).</span></span>
- [<span data-ttu-id="1abb6-175">Debuggen von ASP.NET Web-API mit dem Routendebugger</span><span class="sxs-lookup"><span data-stu-id="1abb6-175">Debugging ASP.NET Web API with Route Debugger</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
