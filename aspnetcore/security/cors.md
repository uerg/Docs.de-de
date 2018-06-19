---
title: Aktivieren Sie Cross-Origin-Anforderungen (CORS) in ASP.NET Core
author: rick-anderson
description: Erfahren Sie, wie CORS als Standard für das zulassen oder ablehnen von Cross-Origin-Anforderungen in einer ASP.NET Core-app.
manager: wpickett
ms.author: riande
ms.date: 05/17/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/cors
ms.openlocfilehash: 3c5d0840426c7ed52353a7832a1a1959027121de
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
ms.locfileid: "30077547"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="4160b-103">Aktivieren Sie Cross-Origin-Anforderungen (CORS) in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4160b-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="4160b-104">Durch [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), und [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="4160b-104">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="4160b-105">Browsersicherheit wird verhindert, dass eine Webseite, die AJAX-Anforderungen in eine andere Domäne.</span><span class="sxs-lookup"><span data-stu-id="4160b-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="4160b-106">Diese Einschränkung wird aufgerufen, die *gleichen Origin-Richtlinie*, und verhindert, dass eine bösartige Website vertrauliche Daten von einem anderen Standort lesen.</span><span class="sxs-lookup"><span data-stu-id="4160b-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="4160b-107">Allerdings sollten Sie in einigen Fällen an anderen Standorten Cross-Origin-Anforderungen für Ihre Web-API ausführen können.</span><span class="sxs-lookup"><span data-stu-id="4160b-107">However, sometimes you might want to let other sites make cross-origin requests to your web API.</span></span>

<span data-ttu-id="4160b-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) ist ein W3C-Standard, der einem Server ermöglicht, die gleichen-Origin-Richtlinie weniger streng gehandhabt.</span><span class="sxs-lookup"><span data-stu-id="4160b-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="4160b-109">Verwenden CORS, können ein Servers explizit einige Cross-Origin-Anforderungen beim Ablehnen von anderen Benutzern.</span><span class="sxs-lookup"><span data-stu-id="4160b-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="4160b-110">CORS ist eine sicherere und flexibler als die früheren Methoden wie z. B. [JSONP](https://wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="4160b-110">CORS is safer and more flexible than earlier techniques such as [JSONP](https://wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="4160b-111">Dieses Thema veranschaulicht das Aktivieren von CORS in einer ASP.NET Core-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="4160b-111">This topic shows how to enable CORS in an ASP.NET Core application.</span></span>

## <a name="what-is-same-origin"></a><span data-ttu-id="4160b-112">Was ist "gleichen Origin"?</span><span class="sxs-lookup"><span data-stu-id="4160b-112">What is "same origin"?</span></span>

<span data-ttu-id="4160b-113">Zwei URLs müssen denselben Ursprung aus, wenn sie identische Schemas, Hosts und Ports verfügen.</span><span class="sxs-lookup"><span data-stu-id="4160b-113">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="4160b-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="4160b-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="4160b-115">Diese beiden URLs haben die gleichen Ursprungs:</span><span class="sxs-lookup"><span data-stu-id="4160b-115">These two URLs have the same origin:</span></span>

* `http://example.com/foo.html`

* `http://example.com/bar.html`

<span data-ttu-id="4160b-116">Diese URLs haben unterschiedliche Ursprünge als den vorherigen zwei:</span><span class="sxs-lookup"><span data-stu-id="4160b-116">These URLs have different origins than the previous two:</span></span>

* <span data-ttu-id="4160b-117">`http://example.net` -Der anderen Domäne</span><span class="sxs-lookup"><span data-stu-id="4160b-117">`http://example.net` - Different domain</span></span>

* <span data-ttu-id="4160b-118">`http://www.example.com/foo.html` -Andere Unterdomäne</span><span class="sxs-lookup"><span data-stu-id="4160b-118">`http://www.example.com/foo.html` - Different subdomain</span></span>

* <span data-ttu-id="4160b-119">`https://example.com/foo.html` -Anderes Schema</span><span class="sxs-lookup"><span data-stu-id="4160b-119">`https://example.com/foo.html` - Different scheme</span></span>

* <span data-ttu-id="4160b-120">`http://example.com:9000/foo.html` -Anschluss</span><span class="sxs-lookup"><span data-stu-id="4160b-120">`http://example.com:9000/foo.html` - Different port</span></span>

> [!NOTE]
> <span data-ttu-id="4160b-121">Internet Explorer berücksichtigen nicht den Port Ursprünge zu vergleichen.</span><span class="sxs-lookup"><span data-stu-id="4160b-121">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="setting-up-cors"></a><span data-ttu-id="4160b-122">CORS einrichten</span><span class="sxs-lookup"><span data-stu-id="4160b-122">Setting up CORS</span></span>

<span data-ttu-id="4160b-123">Richten Sie CORS für Ihre Anwendung hinzufügen der `Microsoft.AspNetCore.Cors` Paket Ihrem Projekt.</span><span class="sxs-lookup"><span data-stu-id="4160b-123">To set up CORS for your application add the `Microsoft.AspNetCore.Cors` package to your project.</span></span>

<span data-ttu-id="4160b-124">Fügen Sie die CORS-Dienste im Startup.cs hinzu:</span><span class="sxs-lookup"><span data-stu-id="4160b-124">Add the CORS services in Startup.cs:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a><span data-ttu-id="4160b-125">Aktivieren von CORS mit middleware</span><span class="sxs-lookup"><span data-stu-id="4160b-125">Enabling CORS with middleware</span></span>

<span data-ttu-id="4160b-126">So aktivieren Sie CORS für die gesamte Anwendung fügen Sie die CORS-Middleware mit Ihrer Anforderung Pipeline die `UseCors` Erweiterungsmethode.</span><span class="sxs-lookup"><span data-stu-id="4160b-126">To enable CORS for your entire application add the CORS middleware to your request pipeline using the `UseCors` extension method.</span></span> <span data-ttu-id="4160b-127">Beachten Sie, dass die CORS-Middleware definierten Endpunkte in Ihrer app vorausgehen muss, die Cross-Origin-Anforderungen (z.B. unterstützt werden soll</span><span class="sxs-lookup"><span data-stu-id="4160b-127">Note that the CORS middleware must precede any defined endpoints in your app that you want to support cross-origin requests (ex.</span></span> <span data-ttu-id="4160b-128">vor jeder Aufruf von `UseMvc`).</span><span class="sxs-lookup"><span data-stu-id="4160b-128">before any call to `UseMvc`).</span></span>

<span data-ttu-id="4160b-129">Sie können eine Cross-Origin-Richtlinie angeben, beim Hinzufügen von den CORS-Middleware mithilfe der `CorsPolicyBuilder` Klasse.</span><span class="sxs-lookup"><span data-stu-id="4160b-129">You can specify a cross-origin policy when adding the CORS middleware using the `CorsPolicyBuilder` class.</span></span> <span data-ttu-id="4160b-130">Hierfür gibt es zwei Möglichkeiten.</span><span class="sxs-lookup"><span data-stu-id="4160b-130">There are two ways to do this.</span></span> <span data-ttu-id="4160b-131">Der erste Schritt besteht UseCors mit einem Lambda-Ausdruck aufrufen:</span><span class="sxs-lookup"><span data-stu-id="4160b-131">The first is to call UseCors with a lambda:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

<span data-ttu-id="4160b-132">**Hinweis:** die URL muss angegeben werden, ohne einen nachstehenden Schrägstrich (`/`).</span><span class="sxs-lookup"><span data-stu-id="4160b-132">**Note:** The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="4160b-133">Wenn die URL mit beendet `/`, der Vergleich zurück `false` und keine Header zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="4160b-133">If the URL terminates with `/`, the comparison will return `false` and no header will be returned.</span></span>

<span data-ttu-id="4160b-134">Der Lambda-Ausdruck akzeptiert eine `CorsPolicyBuilder` Objekt.</span><span class="sxs-lookup"><span data-stu-id="4160b-134">The lambda takes a `CorsPolicyBuilder` object.</span></span> <span data-ttu-id="4160b-135">Sie finden eine Liste mit den [Konfigurationsoptionen](#cors-policy-options) weiter unten in diesem Thema.</span><span class="sxs-lookup"><span data-stu-id="4160b-135">You'll find a list of the [configuration options](#cors-policy-options) later in this topic.</span></span> <span data-ttu-id="4160b-136">In diesem Beispiel lässt die Richtlinie Cross-Origin-Anforderungen von `http://example.com` und keine anderen Quellen.</span><span class="sxs-lookup"><span data-stu-id="4160b-136">In this example, the policy allows cross-origin requests from `http://example.com` and no other origins.</span></span>

<span data-ttu-id="4160b-137">Beachten Sie, dass CorsPolicyBuilder eine fluent-API verfügt, damit Sie die Methodenaufrufe verketten können:</span><span class="sxs-lookup"><span data-stu-id="4160b-137">Note that CorsPolicyBuilder has a fluent API, so you can chain method calls:</span></span>

[!code-csharp[](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

<span data-ttu-id="4160b-138">Der zweite Ansatz ist, definieren ein oder mehrere benannte CORS-Richtlinien, und wählen Sie die Richtlinie anhand des Namens zur Laufzeit.</span><span class="sxs-lookup"><span data-stu-id="4160b-138">The second approach is to define one or more named CORS policies, and then select the policy by name at run time.</span></span>

[!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

<span data-ttu-id="4160b-139">In diesem Beispiel wird eine CORS-Richtlinie mit dem Namen "AllowSpecificOrigin".</span><span class="sxs-lookup"><span data-stu-id="4160b-139">This example adds a CORS policy named "AllowSpecificOrigin".</span></span> <span data-ttu-id="4160b-140">Um die Richtlinie auswählen, übergeben Sie den Namen zu `UseCors`.</span><span class="sxs-lookup"><span data-stu-id="4160b-140">To select the policy, pass the name to `UseCors`.</span></span>

## <a name="enabling-cors-in-mvc"></a><span data-ttu-id="4160b-141">Aktivieren von CORS in MVC</span><span class="sxs-lookup"><span data-stu-id="4160b-141">Enabling CORS in MVC</span></span>

<span data-ttu-id="4160b-142">Sie können alternativ MVC verwenden, um bestimmte CORS pro Aktion, die pro Controller oder global für alle Controller anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="4160b-142">You can alternatively use MVC to apply specific CORS per action, per controller, or globally for all controllers.</span></span> <span data-ttu-id="4160b-143">Verwendung von MVC CORS aktivieren, werden die gleichen CORS-Dienste verwendet, jedoch nicht von den CORS-Middleware.</span><span class="sxs-lookup"><span data-stu-id="4160b-143">When using MVC to enable CORS the same CORS services are used, but the CORS middleware isn't.</span></span>

### <a name="per-action"></a><span data-ttu-id="4160b-144">Pro Aktion</span><span class="sxs-lookup"><span data-stu-id="4160b-144">Per action</span></span>

<span data-ttu-id="4160b-145">Geben Sie eine CORS-Richtlinie für eine bestimmte Aktion hinzufügen der `[EnableCors]` -Attribut auf die Aktion.</span><span class="sxs-lookup"><span data-stu-id="4160b-145">To specify a CORS policy for a specific action add the `[EnableCors]` attribute to the action.</span></span> <span data-ttu-id="4160b-146">Geben Sie den Richtliniennamen an.</span><span class="sxs-lookup"><span data-stu-id="4160b-146">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a><span data-ttu-id="4160b-147">Pro controller</span><span class="sxs-lookup"><span data-stu-id="4160b-147">Per controller</span></span>

<span data-ttu-id="4160b-148">Geben Sie die CORS-Richtlinie für einen bestimmten Controller Hinzufügen der `[EnableCors]` -Attribut auf die Controllerklasse.</span><span class="sxs-lookup"><span data-stu-id="4160b-148">To specify the CORS policy for a specific controller add the `[EnableCors]` attribute to the controller class.</span></span> <span data-ttu-id="4160b-149">Geben Sie den Richtliniennamen an.</span><span class="sxs-lookup"><span data-stu-id="4160b-149">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a><span data-ttu-id="4160b-150">Global</span><span class="sxs-lookup"><span data-stu-id="4160b-150">Globally</span></span>

<span data-ttu-id="4160b-151">Sie können CORS global für alle Domänencontroller durch Hinzufügen der `CorsAuthorizationFilterFactory` Filter, um die Auflistung der globalen Filter:</span><span class="sxs-lookup"><span data-stu-id="4160b-151">You can enable CORS globally for all controllers by adding the `CorsAuthorizationFilterFactory` filter to the global filter collection:</span></span>

[!code-csharp[](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

<span data-ttu-id="4160b-152">Die Rangfolge wird: Aktion, Controller, global.</span><span class="sxs-lookup"><span data-stu-id="4160b-152">The precedence order is: Action, controller, global.</span></span> <span data-ttu-id="4160b-153">Sicherheitsrichtlinien auf Unternehmensebene Aktion haben Vorrang vor Controllerebene Richtlinien sowie Sicherheitsrichtlinien auf Unternehmensebene Controller haben Vorrang vor globalen Richtlinien.</span><span class="sxs-lookup"><span data-stu-id="4160b-153">Action-level policies take precedence over controller-level policies, and controller-level policies take precedence over global policies.</span></span>

### <a name="disable-cors"></a><span data-ttu-id="4160b-154">Deaktivieren Sie CORS</span><span class="sxs-lookup"><span data-stu-id="4160b-154">Disable CORS</span></span>

<span data-ttu-id="4160b-155">Verwenden Sie zum Deaktivieren von CORS für einen Controller oder die Aktion der `[DisableCors]` Attribut.</span><span class="sxs-lookup"><span data-stu-id="4160b-155">To disable CORS for a controller or action, use the `[DisableCors]` attribute.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a><span data-ttu-id="4160b-156">Optionen für CORS-Richtlinie</span><span class="sxs-lookup"><span data-stu-id="4160b-156">CORS policy options</span></span>

<span data-ttu-id="4160b-157">Dieser Abschnitt beschreibt die verschiedenen Optionen, die Sie in einer CORS-Richtlinie festlegen können.</span><span class="sxs-lookup"><span data-stu-id="4160b-157">This section describes the various options that you can set in a CORS policy.</span></span>

* [<span data-ttu-id="4160b-158">Legen Sie die zulässigen Ursprünge</span><span class="sxs-lookup"><span data-stu-id="4160b-158">Set the allowed origins</span></span>](#set-the-allowed-origins)

* [<span data-ttu-id="4160b-159">Legen Sie die zulässigen HTTP-Methoden</span><span class="sxs-lookup"><span data-stu-id="4160b-159">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)

* [<span data-ttu-id="4160b-160">Legt die zulässige Anforderungsheader fest.</span><span class="sxs-lookup"><span data-stu-id="4160b-160">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)

* [<span data-ttu-id="4160b-161">Legen Sie die verfügbar gemachten Antwortheader</span><span class="sxs-lookup"><span data-stu-id="4160b-161">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)

* [<span data-ttu-id="4160b-162">Anmeldeinformationen in Cross-Origin-Anforderungen</span><span class="sxs-lookup"><span data-stu-id="4160b-162">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)

* [<span data-ttu-id="4160b-163">Legen Sie die Preflight-Ablaufzeit</span><span class="sxs-lookup"><span data-stu-id="4160b-163">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="4160b-164">Für einige Optionen kann es hilfreich, zu lesen sein [funktioniert wie CORS](#how-cors-works) erste.</span><span class="sxs-lookup"><span data-stu-id="4160b-164">For some options it may be helpful to read [How CORS works](#how-cors-works) first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="4160b-165">Legen Sie die zulässigen Ursprünge</span><span class="sxs-lookup"><span data-stu-id="4160b-165">Set the allowed origins</span></span>

<span data-ttu-id="4160b-166">Um eine oder mehrere bestimmte Ursprünge zuzulassen:</span><span class="sxs-lookup"><span data-stu-id="4160b-166">To allow one or more specific origins:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=19-23)]

<span data-ttu-id="4160b-167">Alle Ursprünge erlaubt:</span><span class="sxs-lookup"><span data-stu-id="4160b-167">To allow all origins:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs??range=27-31)]

<span data-ttu-id="4160b-168">Sollten Sie sorgfältig überlegen, bevor Anforderungen über einen beliebigen Ursprung zugelassen.</span><span class="sxs-lookup"><span data-stu-id="4160b-168">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="4160b-169">Dies bedeutet, dass es sich bei jeder beliebige Website AJAX-Aufrufe auf Ihre API vornehmen kann.</span><span class="sxs-lookup"><span data-stu-id="4160b-169">It means that literally any website can make AJAX calls to your API.</span></span>

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="4160b-170">Legen Sie die zulässigen HTTP-Methoden</span><span class="sxs-lookup"><span data-stu-id="4160b-170">Set the allowed HTTP methods</span></span>

<span data-ttu-id="4160b-171">Um alle HTTP-Methoden zu ermöglichen:</span><span class="sxs-lookup"><span data-stu-id="4160b-171">To allow all HTTP methods:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=44-49)]

<span data-ttu-id="4160b-172">Dies wirkt sich auf die Preflight-Anforderungen und Access-Control-Allow-Methods-Header.</span><span class="sxs-lookup"><span data-stu-id="4160b-172">This affects pre-flight requests and Access-Control-Allow-Methods header.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="4160b-173">Legt die zulässige Anforderungsheader fest.</span><span class="sxs-lookup"><span data-stu-id="4160b-173">Set the allowed request headers</span></span>

<span data-ttu-id="4160b-174">Eine CORS-preflight-Anforderung kann einen Access-Control-Request-Headers-Header, die HTTP-Header, die von der Anwendung festgelegtes auflisten enthalten (den so genannten "author Anforderungsheader").</span><span class="sxs-lookup"><span data-stu-id="4160b-174">A CORS preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span>

<span data-ttu-id="4160b-175">Auf bestimmte Header der Positivliste:</span><span class="sxs-lookup"><span data-stu-id="4160b-175">To whitelist specific headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=53-58)]

<span data-ttu-id="4160b-176">Damit können erstellen alle Anforderungsheader:</span><span class="sxs-lookup"><span data-stu-id="4160b-176">To allow all author request headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=62-67)]

<span data-ttu-id="4160b-177">Browser sind nicht vollständig in diese Festlegung von Access-Control-Request-Headers konsistent.</span><span class="sxs-lookup"><span data-stu-id="4160b-177">Browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="4160b-178">Wenn der Header zu beliebiegen Dokumentbestandteilen außer festlegen "\*", Sie sollten berücksichtigen mindestens "Annehmen", "Content-Type" und "Origin" sowie alle benutzerdefinierten Header, die Sie unterstützen möchten.</span><span class="sxs-lookup"><span data-stu-id="4160b-178">If you set headers to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="4160b-179">Legen Sie die verfügbar gemachten Antwortheader</span><span class="sxs-lookup"><span data-stu-id="4160b-179">Set the exposed response headers</span></span>

<span data-ttu-id="4160b-180">Standardmäßig nicht im Browser aller die Antwortheader für die Anwendung verfügbar machen.</span><span class="sxs-lookup"><span data-stu-id="4160b-180">By default, the browser doesn't expose all of the response headers to the application.</span></span> <span data-ttu-id="4160b-181">(See [http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header).) Die Antwortheader, die standardmäßig verfügbar sind:</span><span class="sxs-lookup"><span data-stu-id="4160b-181">(See [http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header).) The response headers that are available by default are:</span></span>

* <span data-ttu-id="4160b-182">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="4160b-182">Cache-Control</span></span>

* <span data-ttu-id="4160b-183">Content-Language</span><span class="sxs-lookup"><span data-stu-id="4160b-183">Content-Language</span></span>

* <span data-ttu-id="4160b-184">Content-Type</span><span class="sxs-lookup"><span data-stu-id="4160b-184">Content-Type</span></span>

* <span data-ttu-id="4160b-185">Läuft ab</span><span class="sxs-lookup"><span data-stu-id="4160b-185">Expires</span></span>

* <span data-ttu-id="4160b-186">Zuletzt geändert</span><span class="sxs-lookup"><span data-stu-id="4160b-186">Last-Modified</span></span>

* <span data-ttu-id="4160b-187">Pragma</span><span class="sxs-lookup"><span data-stu-id="4160b-187">Pragma</span></span>

<span data-ttu-id="4160b-188">Die CORS-Spezifikation ruft diese *einfache Antwortheader*.</span><span class="sxs-lookup"><span data-stu-id="4160b-188">The CORS spec calls these *simple response headers*.</span></span> <span data-ttu-id="4160b-189">Um weitere Header für die Anwendung verfügbar zu machen:</span><span class="sxs-lookup"><span data-stu-id="4160b-189">To make other headers available to the application:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="4160b-190">Anmeldeinformationen in Cross-Origin-Anforderungen</span><span class="sxs-lookup"><span data-stu-id="4160b-190">Credentials in cross-origin requests</span></span>

<span data-ttu-id="4160b-191">Anmeldeinformationen erfordern eine besondere Behandlung in einer CORS-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="4160b-191">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="4160b-192">Standardmäßig senden nicht im Browser keine Anmeldeinformationen mit einem Cross-Origin-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="4160b-192">By default, the browser doesn't send any credentials with a cross-origin request.</span></span> <span data-ttu-id="4160b-193">Anmeldeinformationen werden Cookies sowie HTTP-Authentifizierungsschemas einschließen.</span><span class="sxs-lookup"><span data-stu-id="4160b-193">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="4160b-194">Der Client muss zum Senden von Anmeldeinformationen mit einem Cross-Origin-Anforderung XMLHttpRequest.withCredentials auf "true" festlegen.</span><span class="sxs-lookup"><span data-stu-id="4160b-194">To send credentials with a cross-origin request, the client must set XMLHttpRequest.withCredentials to true.</span></span>

<span data-ttu-id="4160b-195">Mithilfe von XMLHttpRequest direkt:</span><span class="sxs-lookup"><span data-stu-id="4160b-195">Using XMLHttpRequest directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="4160b-196">In jQuery:</span><span class="sxs-lookup"><span data-stu-id="4160b-196">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="4160b-197">Darüber hinaus muss der Server die Anmeldeinformationen zulassen.</span><span class="sxs-lookup"><span data-stu-id="4160b-197">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="4160b-198">Um Cross-Origin-Anmeldeinformationen zu ermöglichen:</span><span class="sxs-lookup"><span data-stu-id="4160b-198">To allow cross-origin credentials:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=80-85)]

<span data-ttu-id="4160b-199">Die HTTP-Antwort enthalten nun einen Access-Control-Allow-Credentials-Header, der wodurch dem Browser angewiesen wird, dass der Server die Anmeldeinformationen für eine Anforderung Cross-Origin zulässt.</span><span class="sxs-lookup"><span data-stu-id="4160b-199">Now the HTTP response will include an Access-Control-Allow-Credentials header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="4160b-200">Wenn der Browser Anmeldeinformationen sendet, aber die Antwort keinen gültigen Access-Control-Allow-Credentials-Header, der Browser wird nicht die Antwort an die Anwendung verfügbar machen, und die AJAX-Anforderung ein Fehler auftritt.</span><span class="sxs-lookup"><span data-stu-id="4160b-200">If the browser sends credentials, but the response doesn't include a valid Access-Control-Allow-Credentials header, the browser won't expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="4160b-201">Seien Sie vorsichtig bei Cross-Origin-Anmeldeinformationen zulassen.</span><span class="sxs-lookup"><span data-stu-id="4160b-201">Be careful when allowing cross-origin credentials.</span></span> <span data-ttu-id="4160b-202">Eine Website finden Sie unter einer anderen Domäne kann Anmeldeinformationen eines angemeldeten Benutzers an die app im Auftrag des Benutzers, ohne Wissen des Benutzers gesendet.</span><span class="sxs-lookup"><span data-stu-id="4160b-202">A website at another domain can send a logged-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <span data-ttu-id="4160b-203">CORS-Spezifikation besagt ebenfalls diese Einstellung Ursprünge auf "\*" (alle Ursprünge) ist ungültig. wenn die `Access-Control-Allow-Credentials` Header vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="4160b-203">The CORS specification also states that setting origins to "\*" (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="4160b-204">Legen Sie die Preflight-Ablaufzeit</span><span class="sxs-lookup"><span data-stu-id="4160b-204">Set the preflight expiration time</span></span>

<span data-ttu-id="4160b-205">Der Access-Control-Max-Age-Header gibt an, wie lange die Antwort auf die preflight-Anforderung zwischengespeichert werden kann.</span><span class="sxs-lookup"><span data-stu-id="4160b-205">The Access-Control-Max-Age header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="4160b-206">Auf diese Header festgelegt werden:</span><span class="sxs-lookup"><span data-stu-id="4160b-206">To set this header:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a><span data-ttu-id="4160b-207">Funktionsweise von CORS</span><span class="sxs-lookup"><span data-stu-id="4160b-207">How CORS works</span></span>

<span data-ttu-id="4160b-208">In diesem Abschnitt wird beschrieben, was geschieht, in einer CORS-Anforderung auf der Ebene der HTTP-Nachrichten.</span><span class="sxs-lookup"><span data-stu-id="4160b-208">This section describes what happens in a CORS request at the level of the HTTP messages.</span></span> <span data-ttu-id="4160b-209">Es ist wichtig zu verstehen, wie die CORS zusammen, sodass die CORS-Richtlinie ordnungsgemäß konfiguriert werden kann und eine beim zu unerwarteten Ergebnissen führen.</span><span class="sxs-lookup"><span data-stu-id="4160b-209">It's important to understand how CORS works so that the CORS policy can be configured correctly and troubleshooted when unexpected behaviors occur.</span></span>

<span data-ttu-id="4160b-210">CORS-Spezifikation führt mehrere neue HTTP-Header, die Cross-Origin-Anforderungen zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="4160b-210">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="4160b-211">Wenn ein Browser CORS unterstützt, wird diese Header automatisch für Cross-Origin-Anforderungen festgelegt.</span><span class="sxs-lookup"><span data-stu-id="4160b-211">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="4160b-212">Benutzerdefinierte JavaScript-Code ist nicht erforderlich, um CORS zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="4160b-212">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="4160b-213">Hier ist ein Beispiel einer Cross-Origin-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="4160b-213">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="4160b-214">Die `Origin` Header bietet die Domäne des Standorts, der die Anforderung gesendet wird:</span><span class="sxs-lookup"><span data-stu-id="4160b-214">The `Origin` header provides the domain of the site that's making the request:</span></span>

```
GET http://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: http://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: http://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

<span data-ttu-id="4160b-215">Wenn der Server die Anforderung zulässt, wird die Access-Control-Allow-Origin-Header in der Antwort festgelegt.</span><span class="sxs-lookup"><span data-stu-id="4160b-215">If the server allows the request, it sets the Access-Control-Allow-Origin header in the response.</span></span> <span data-ttu-id="4160b-216">Der Wert dieses Headers entspricht den Origin-Header in der Anforderung oder Wert für die Platzhalterzeichen "\*", Bedeutung, die einen beliebigen Ursprung zulässig ist:</span><span class="sxs-lookup"><span data-stu-id="4160b-216">The value of this header either matches the Origin header from the request, or is the wildcard value "\*", meaning that any origin is allowed:</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

<span data-ttu-id="4160b-217">Wenn die Antwort den Access-Control-Allow-Origin-Header enthält, schlägt die AJAX-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="4160b-217">If the response doesn't include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="4160b-218">Der Browser lässt, die die Anforderung.</span><span class="sxs-lookup"><span data-stu-id="4160b-218">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="4160b-219">Auch wenn der Server eine erfolgreiche Antwort zurückgibt, machen nicht im Browser die Antwort an die Clientanwendung verfügbar.</span><span class="sxs-lookup"><span data-stu-id="4160b-219">Even if the server returns a successful response, the browser doesn't make the response available to the client application.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="4160b-220">Preflight-Anforderungen</span><span class="sxs-lookup"><span data-stu-id="4160b-220">Preflight Requests</span></span>

<span data-ttu-id="4160b-221">Für einige CORS-Anforderungen sendet der Browser eine zusätzliche Anforderung, eine "preflight-Anforderung", aufgerufen, bevor die tatsächliche Anforderung für die Ressource gesendet.</span><span class="sxs-lookup"><span data-stu-id="4160b-221">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span> <span data-ttu-id="4160b-222">Der Browser kann die preflight-Anforderung überspringen, wenn Folgendes zutrifft:</span><span class="sxs-lookup"><span data-stu-id="4160b-222">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="4160b-223">Die Anforderungsmethode ist GET, HEAD oder POST, und</span><span class="sxs-lookup"><span data-stu-id="4160b-223">The request method is GET, HEAD, or POST, and</span></span>

* <span data-ttu-id="4160b-224">Die Anwendung keine Anforderungsheader als Accept, Accept-Language-Content-Language, festzulegen, die Content-Type oder letzten-Ereignis-ID und</span><span class="sxs-lookup"><span data-stu-id="4160b-224">The application doesn't set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, and</span></span>

* <span data-ttu-id="4160b-225">Der Content-Type-Header (falls festgelegt) ist eines der folgenden:</span><span class="sxs-lookup"><span data-stu-id="4160b-225">The Content-Type header (if set) is one of the following:</span></span>

  * <span data-ttu-id="4160b-226">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="4160b-226">application/x-www-form-urlencoded</span></span>

  * <span data-ttu-id="4160b-227">Multipart/Form-data</span><span class="sxs-lookup"><span data-stu-id="4160b-227">multipart/form-data</span></span>

  * <span data-ttu-id="4160b-228">Text/plain</span><span class="sxs-lookup"><span data-stu-id="4160b-228">text/plain</span></span>

<span data-ttu-id="4160b-229">Die Regel zu Anforderungsheader gilt für Header, die Anwendung wird durch Aufrufen von SetRequestHeader für das XMLHttpRequest-Objekt.</span><span class="sxs-lookup"><span data-stu-id="4160b-229">The rule about request headers applies to headers that the application sets by calling setRequestHeader on the XMLHttpRequest object.</span></span> <span data-ttu-id="4160b-230">(Die CORS-Spezifikation ruft diese "Autor Anforderungsheader".) Die Regel gilt nicht für Header, die der Browser "Benutzer-Agent-Host" oder "Content-Length festlegen kann.</span><span class="sxs-lookup"><span data-stu-id="4160b-230">(The CORS specification calls these "author request headers".) The rule doesn't apply to headers the browser can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="4160b-231">Hier ist ein Beispiel für eine preflight-Anforderung:</span><span class="sxs-lookup"><span data-stu-id="4160b-231">Here is an example of a preflight request:</span></span>

```
OPTIONS http://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: http://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

<span data-ttu-id="4160b-232">Die Preflight-Anforderung mithilfe der HTTP OPTIONS-Methode.</span><span class="sxs-lookup"><span data-stu-id="4160b-232">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="4160b-233">Sie schließt zwei spezielle Header:</span><span class="sxs-lookup"><span data-stu-id="4160b-233">It includes two special headers:</span></span>

* <span data-ttu-id="4160b-234">Access-Control-Request-Method: Die HTTP-Methode, die für die tatsächliche Anforderung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="4160b-234">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>

* <span data-ttu-id="4160b-235">Access-Control-Request-Headers: Eine Liste der Anforderungsheader, die die Anwendung für die tatsächliche Anforderung festgelegt.</span><span class="sxs-lookup"><span data-stu-id="4160b-235">Access-Control-Request-Headers: A list of request headers that the application set on the actual request.</span></span> <span data-ttu-id="4160b-236">(Erneut nicht dazu gehören Header, die der Browser legt sie fest.)</span><span class="sxs-lookup"><span data-stu-id="4160b-236">(Again, this doesn't include headers that the browser sets.)</span></span>

<span data-ttu-id="4160b-237">Hier ist eine Beispielantwort, vorausgesetzt, dass der Server die Anforderung zulässt:</span><span class="sxs-lookup"><span data-stu-id="4160b-237">Here is an example response, assuming that the server allows the request:</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

<span data-ttu-id="4160b-238">Die Antwort enthält eine Access-Control-Allow-Methods-Header, der die zulässigen Methoden aufgelistet, und optional eine Access-Control-Allow-Headers-Header, der Listet die erlaubten Header.</span><span class="sxs-lookup"><span data-stu-id="4160b-238">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="4160b-239">Wenn die preflight-Anforderung erfolgreich ist, sendet der Browser die tatsächliche Anforderung an, wie zuvor beschrieben.</span><span class="sxs-lookup"><span data-stu-id="4160b-239">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>
