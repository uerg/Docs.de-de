---
title: Aktivieren Sie Ursprungsübergreifender Anforderungen (CORS) in ASP.NET Core
author: rick-anderson
description: Erfahren Sie, wie CORS als Standard zum Zulassen oder ablehnen von ursprungsübergreifenden Anforderungen in einer ASP.NET Core-app.
ms.author: riande
ms.date: 08/17/2018
uid: security/cors
ms.openlocfilehash: 0dbb7933c76bb0d1d0cab519ea08c6c8f0ebedfd
ms.sourcegitcommit: 64c2ca86fff445944b155635918126165ee0f8aa
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/18/2018
ms.locfileid: "41839039"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="6370a-103">Aktivieren Sie Ursprungsübergreifender Anforderungen (CORS) in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6370a-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="6370a-104">Durch [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), und [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="6370a-104">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="6370a-105">Browsersicherheit verhindert, dass eine Webseite AJAX-Anforderungen in eine andere Domäne.</span><span class="sxs-lookup"><span data-stu-id="6370a-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="6370a-106">Diese Einschränkung wird aufgerufen, die *Richtlinie desselben Ursprungs*, und verhindert, dass eine schädliche Website sensible Daten von einer anderen Website liest.</span><span class="sxs-lookup"><span data-stu-id="6370a-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="6370a-107">Aber möchten manchmal andere Standorte, die Cross-Origin-Anforderungen an Ihre Web-API senden können.</span><span class="sxs-lookup"><span data-stu-id="6370a-107">However, sometimes you might want to let other sites make cross-origin requests to your web API.</span></span>

<span data-ttu-id="6370a-108">[Cross-Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) ist ein W3C-Standard, der einem Server zu lockern die Richtlinie des gleichen Ursprungs ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="6370a-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="6370a-109">Mit CORS kann ein Server explizit einige ursprungsübergreifende Anforderungen zulassen und andere ablehnen.</span><span class="sxs-lookup"><span data-stu-id="6370a-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="6370a-110">CORS ist sicherer und flexibler als frühere Techniken wie z. B. [JSONP](https://wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="6370a-110">CORS is safer and more flexible than earlier techniques such as [JSONP](https://wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="6370a-111">In diesem Thema veranschaulicht das Aktivieren von CORS in ASP.NET Core-Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="6370a-111">This topic shows how to enable CORS in an ASP.NET Core application.</span></span>

## <a name="what-is-same-origin"></a><span data-ttu-id="6370a-112">Was ist "denselben Ursprung"?</span><span class="sxs-lookup"><span data-stu-id="6370a-112">What is "same origin"?</span></span>

<span data-ttu-id="6370a-113">Zwei URLs haben denselben Ursprung ggf. identische Schemas, Hosts und -Ports.</span><span class="sxs-lookup"><span data-stu-id="6370a-113">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="6370a-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="6370a-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="6370a-115">Diese zwei URLs haben denselben Ursprung an:</span><span class="sxs-lookup"><span data-stu-id="6370a-115">These two URLs have the same origin:</span></span>

* `http://example.com/foo.html`

* `http://example.com/bar.html`

<span data-ttu-id="6370a-116">Diese URLs haben verschiedene Ursprünge als die vorherige zwei:</span><span class="sxs-lookup"><span data-stu-id="6370a-116">These URLs have different origins than the previous two:</span></span>

* <span data-ttu-id="6370a-117">`http://example.net` -Andere Domäne</span><span class="sxs-lookup"><span data-stu-id="6370a-117">`http://example.net` - Different domain</span></span>

* <span data-ttu-id="6370a-118">`http://www.example.com/foo.html` -Andere Unterdomäne</span><span class="sxs-lookup"><span data-stu-id="6370a-118">`http://www.example.com/foo.html` - Different subdomain</span></span>

* <span data-ttu-id="6370a-119">`https://example.com/foo.html` -Andere Partitionsschema</span><span class="sxs-lookup"><span data-stu-id="6370a-119">`https://example.com/foo.html` - Different scheme</span></span>

* <span data-ttu-id="6370a-120">`http://example.com:9000/foo.html` -Portnummer</span><span class="sxs-lookup"><span data-stu-id="6370a-120">`http://example.com:9000/foo.html` - Different port</span></span>

> [!NOTE]
> <span data-ttu-id="6370a-121">Internet Explorer berücksichtigen nicht den Port Ursprünge zu vergleichen.</span><span class="sxs-lookup"><span data-stu-id="6370a-121">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="enable-cors"></a><span data-ttu-id="6370a-122">Aktivieren von CORS</span><span class="sxs-lookup"><span data-stu-id="6370a-122">Enable CORS</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="6370a-123">Richten Sie CORS für Ihre Anwendung hinzufügen der `Microsoft.AspNetCore.Cors` Paket Ihrem Projekt.</span><span class="sxs-lookup"><span data-stu-id="6370a-123">To set up CORS for your application add the `Microsoft.AspNetCore.Cors` package to your project.</span></span>

::: moniker-end

<span data-ttu-id="6370a-124">Rufen Sie [AddCors](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="6370a-124">Call [AddCors](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a><span data-ttu-id="6370a-125">Aktivieren von CORS mit middleware</span><span class="sxs-lookup"><span data-stu-id="6370a-125">Enabling CORS with middleware</span></span>

<span data-ttu-id="6370a-126">Um CORS zu aktivieren, fügen Sie die CORS-Middleware hinzu, um die Anforderungspipeline mithilfe der `UseCors` -Erweiterungsmethode.</span><span class="sxs-lookup"><span data-stu-id="6370a-126">To enable CORS, add the CORS middleware to the request pipeline using the `UseCors` extension method.</span></span> <span data-ttu-id="6370a-127">Die CORS-Middleware muss vor stehen Endpunkte definiert in Ihrer app, wo Sie möchten für die Unterstützung von Cross-Origin-Anfragen (z. B. vor dem Aufruf `UseMvc`).</span><span class="sxs-lookup"><span data-stu-id="6370a-127">The CORS middleware must precede any defined endpoints in your app where you want to support cross-origin requests (For example, before any call to `UseMvc`).</span></span>

<span data-ttu-id="6370a-128">Eine Cross-Origin-Richtlinie kann angegeben werden, beim Hinzufügen der CORS-Middleware mithilfe der [CorsPolicyBuilder](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) Klasse.</span><span class="sxs-lookup"><span data-stu-id="6370a-128">A cross-origin policy can be specified when adding the CORS middleware using the [CorsPolicyBuilder](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) class.</span></span> <span data-ttu-id="6370a-129">Hierfür gibt es zwei Möglichkeiten.</span><span class="sxs-lookup"><span data-stu-id="6370a-129">There are two ways to do this.</span></span> <span data-ttu-id="6370a-130">Die erste besteht darin, rufen Sie `UseCors` mit einem Lambda-Ausdruck:</span><span class="sxs-lookup"><span data-stu-id="6370a-130">The first is to call `UseCors` with a lambda:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

<span data-ttu-id="6370a-131">**Hinweis:** die URL muss angegeben werden, ohne nachgestellten Schrägstrich (`/`).</span><span class="sxs-lookup"><span data-stu-id="6370a-131">**Note:** The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="6370a-132">Wenn die URL mit beendet `/`, der Vergleich zurück `false` und kein Header zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="6370a-132">If the URL terminates with `/`, the comparison will return `false` and no header will be returned.</span></span>

<span data-ttu-id="6370a-133">Das Lambda akzeptiert ein `CorsPolicyBuilder` Objekt.</span><span class="sxs-lookup"><span data-stu-id="6370a-133">The lambda takes a `CorsPolicyBuilder` object.</span></span> <span data-ttu-id="6370a-134">Sie finden eine Liste mit den [Konfigurationsoptionen](#cors-policy-options) weiter unten in diesem Thema.</span><span class="sxs-lookup"><span data-stu-id="6370a-134">You'll find a list of the [configuration options](#cors-policy-options) later in this topic.</span></span> <span data-ttu-id="6370a-135">In diesem Beispiel kann die Richtlinie für Cross-Origin-Anforderungen von `http://example.com` und keine anderen Quellen.</span><span class="sxs-lookup"><span data-stu-id="6370a-135">In this example, the policy allows cross-origin requests from `http://example.com` and no other origins.</span></span>

<span data-ttu-id="6370a-136">CorsPolicyBuilder hat eine fluent-API, damit Sie die Methodenaufrufe verketten können:</span><span class="sxs-lookup"><span data-stu-id="6370a-136">CorsPolicyBuilder has a fluent API, so you can chain method calls:</span></span>

[!code-csharp[](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

<span data-ttu-id="6370a-137">Der zweite Ansatz ist auf eine oder mehrere benannte CORS-Richtlinien zu definieren, und wählen Sie dann zur Laufzeit anhand des Namens der Richtlinie.</span><span class="sxs-lookup"><span data-stu-id="6370a-137">The second approach is to define one or more named CORS policies, and then select the policy by name at run time.</span></span>

[!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

<span data-ttu-id="6370a-138">In diesem Beispiel wird eine CORS-Richtlinie, die mit dem Namen "AllowSpecificOrigin".</span><span class="sxs-lookup"><span data-stu-id="6370a-138">This example adds a CORS policy named "AllowSpecificOrigin".</span></span> <span data-ttu-id="6370a-139">Um die Richtlinie auswählen, übergeben Sie den Namen in `UseCors`.</span><span class="sxs-lookup"><span data-stu-id="6370a-139">To select the policy, pass the name to `UseCors`.</span></span>

## <a name="enabling-cors-in-mvc"></a><span data-ttu-id="6370a-140">Aktivieren von CORS in MVC</span><span class="sxs-lookup"><span data-stu-id="6370a-140">Enabling CORS in MVC</span></span>

<span data-ttu-id="6370a-141">Sie können alternativ MVC verwenden, um bestimmte CORS pro Aktion, pro Controller oder global für alle Controller anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="6370a-141">You can alternatively use MVC to apply specific CORS per action, per controller, or globally for all controllers.</span></span> <span data-ttu-id="6370a-142">Die gleichen CORS-Dienste werden verwendet, wenn Sie MVC verwenden, um CORS zu aktivieren, aber die CORS-Middleware nicht ist.</span><span class="sxs-lookup"><span data-stu-id="6370a-142">When using MVC to enable CORS the same CORS services are used, but the CORS middleware isn't.</span></span>

### <a name="per-action"></a><span data-ttu-id="6370a-143">Pro Aktion</span><span class="sxs-lookup"><span data-stu-id="6370a-143">Per action</span></span>

<span data-ttu-id="6370a-144">Geben Sie eine CORS-Richtlinie für eine bestimmte Aktion hinzufügen der `[EnableCors]` -Attribut auf die Aktion.</span><span class="sxs-lookup"><span data-stu-id="6370a-144">To specify a CORS policy for a specific action add the `[EnableCors]` attribute to the action.</span></span> <span data-ttu-id="6370a-145">Geben Sie den Namen der Richtlinie.</span><span class="sxs-lookup"><span data-stu-id="6370a-145">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a><span data-ttu-id="6370a-146">Pro controller</span><span class="sxs-lookup"><span data-stu-id="6370a-146">Per controller</span></span>

<span data-ttu-id="6370a-147">Geben Sie die CORS-Richtlinie für einen bestimmten Controller Hinzufügen der `[EnableCors]` -Attribut auf die Controllerklasse.</span><span class="sxs-lookup"><span data-stu-id="6370a-147">To specify the CORS policy for a specific controller add the `[EnableCors]` attribute to the controller class.</span></span> <span data-ttu-id="6370a-148">Geben Sie den Namen der Richtlinie.</span><span class="sxs-lookup"><span data-stu-id="6370a-148">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a><span data-ttu-id="6370a-149">Global</span><span class="sxs-lookup"><span data-stu-id="6370a-149">Globally</span></span>

<span data-ttu-id="6370a-150">Sie können CORS global für alle Controller durch Hinzufügen der `CorsAuthorizationFilterFactory` Filter, um die globale filterauflistung:</span><span class="sxs-lookup"><span data-stu-id="6370a-150">You can enable CORS globally for all controllers by adding the `CorsAuthorizationFilterFactory` filter to the global filter collection:</span></span>

[!code-csharp[](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

<span data-ttu-id="6370a-151">Die Rangfolge ist: Aktion, Controller, globale.</span><span class="sxs-lookup"><span data-stu-id="6370a-151">The precedence order is: Action, controller, global.</span></span> <span data-ttu-id="6370a-152">Aktion-basierten Richtlinien haben Vorrang vor auf Controllerebene Richtlinien, und auf Controllerebene Richtlinien haben Vorrang vor globalen Richtlinien.</span><span class="sxs-lookup"><span data-stu-id="6370a-152">Action-level policies take precedence over controller-level policies, and controller-level policies take precedence over global policies.</span></span>

### <a name="disable-cors"></a><span data-ttu-id="6370a-153">Deaktivieren von CORS</span><span class="sxs-lookup"><span data-stu-id="6370a-153">Disable CORS</span></span>

<span data-ttu-id="6370a-154">Wenn Sie um CORS für einen Controller oder die Aktion zu deaktivieren, verwenden die `[DisableCors]` Attribut.</span><span class="sxs-lookup"><span data-stu-id="6370a-154">To disable CORS for a controller or action, use the `[DisableCors]` attribute.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a><span data-ttu-id="6370a-155">CORS-Richtlinie-Optionen</span><span class="sxs-lookup"><span data-stu-id="6370a-155">CORS policy options</span></span>

<span data-ttu-id="6370a-156">Dieser Abschnitt beschreibt die verschiedenen Optionen, die Sie in einer CORS-Richtlinie festlegen können.</span><span class="sxs-lookup"><span data-stu-id="6370a-156">This section describes the various options that you can set in a CORS policy.</span></span>

* [<span data-ttu-id="6370a-157">Legen Sie die zulässigen Ursprünge</span><span class="sxs-lookup"><span data-stu-id="6370a-157">Set the allowed origins</span></span>](#set-the-allowed-origins)

* [<span data-ttu-id="6370a-158">Legen Sie die zulässigen HTTP-Methoden</span><span class="sxs-lookup"><span data-stu-id="6370a-158">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)

* [<span data-ttu-id="6370a-159">Legt die zulässigen Anforderungsheader fest.</span><span class="sxs-lookup"><span data-stu-id="6370a-159">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)

* [<span data-ttu-id="6370a-160">Legen Sie die verfügbar gemachten Antwortheader</span><span class="sxs-lookup"><span data-stu-id="6370a-160">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)

* [<span data-ttu-id="6370a-161">Anmeldeinformationen im Cross-Origin-Anforderungen</span><span class="sxs-lookup"><span data-stu-id="6370a-161">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)

* [<span data-ttu-id="6370a-162">Die Preflight-Ablaufzeit festlegen</span><span class="sxs-lookup"><span data-stu-id="6370a-162">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="6370a-163">Für einige Optionen, es kann hilfreich sein, lesen Sie [funktioniert wie CORS](#how-cors-works) erste.</span><span class="sxs-lookup"><span data-stu-id="6370a-163">For some options, it may be helpful to read [How CORS works](#how-cors-works) first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="6370a-164">Legen Sie die zulässigen Ursprünge</span><span class="sxs-lookup"><span data-stu-id="6370a-164">Set the allowed origins</span></span>

<span data-ttu-id="6370a-165">Um eine oder mehrere bestimmte Ursprünge zuzulassen:</span><span class="sxs-lookup"><span data-stu-id="6370a-165">To allow one or more specific origins:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=19-23)]

<span data-ttu-id="6370a-166">Alle Ursprünge erlaubt:</span><span class="sxs-lookup"><span data-stu-id="6370a-166">To allow all origins:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs??range=27-31)]

<span data-ttu-id="6370a-167">Wägen Sie sorgfältig, bevor Sie die Anforderungen von einem beliebigen Ursprung zulassen.</span><span class="sxs-lookup"><span data-stu-id="6370a-167">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="6370a-168">Es bedeutet, dass praktisch jeder Website AJAX-Aufrufe Ihrer API kann.</span><span class="sxs-lookup"><span data-stu-id="6370a-168">It means that literally any website can make AJAX calls to your API.</span></span>

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="6370a-169">Legen Sie die zulässigen HTTP-Methoden</span><span class="sxs-lookup"><span data-stu-id="6370a-169">Set the allowed HTTP methods</span></span>

<span data-ttu-id="6370a-170">Um alle HTTP-Methoden zu ermöglichen:</span><span class="sxs-lookup"><span data-stu-id="6370a-170">To allow all HTTP methods:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=44-49)]

<span data-ttu-id="6370a-171">Dies wirkt sich auf preflightanforderungen und Access-Control-Allow-Methods-Header.</span><span class="sxs-lookup"><span data-stu-id="6370a-171">This affects pre-flight requests and Access-Control-Allow-Methods header.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="6370a-172">Legt die zulässigen Anforderungsheader fest.</span><span class="sxs-lookup"><span data-stu-id="6370a-172">Set the allowed request headers</span></span>

<span data-ttu-id="6370a-173">Eine CORS-preflight-Anforderung sind zum Beispiel einen Access-Control-Request-Headers-Header, die HTTP-Header, die von der Anwendung festgelegte auflisten (den so genannten "author-Anforderungsheader").</span><span class="sxs-lookup"><span data-stu-id="6370a-173">A CORS preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span>

<span data-ttu-id="6370a-174">Auf eine Whitelist bestimmte Header:</span><span class="sxs-lookup"><span data-stu-id="6370a-174">To whitelist specific headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=53-58)]

<span data-ttu-id="6370a-175">Damit können erstellen alle Anforderungsheader:</span><span class="sxs-lookup"><span data-stu-id="6370a-175">To allow all author request headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=62-67)]

<span data-ttu-id="6370a-176">Browser sind nicht vollständig konsistent, in wie sie Access-Control-Request-Headers festgelegt.</span><span class="sxs-lookup"><span data-stu-id="6370a-176">Browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="6370a-177">Wenn Sie Header nicht festgelegt "\*", aufzunehmen mindestens "accept", "Content-Type" und "Origin" sowie alle benutzerdefinierten Header, die Sie unterstützen möchten.</span><span class="sxs-lookup"><span data-stu-id="6370a-177">If you set headers to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="6370a-178">Legen Sie die verfügbar gemachten Antwortheader</span><span class="sxs-lookup"><span data-stu-id="6370a-178">Set the exposed response headers</span></span>

<span data-ttu-id="6370a-179">Standardmäßig nicht im Browser alle Antwortheader für die Anwendung verfügbar machen.</span><span class="sxs-lookup"><span data-stu-id="6370a-179">By default, the browser doesn't expose all of the response headers to the application.</span></span> <span data-ttu-id="6370a-180">(Finden Sie unter [ http://www.w3.org/TR/cors/#simple-response-header ](http://www.w3.org/TR/cors/#simple-response-header).) Die Antwortheader, die standardmäßig verfügbar sind:</span><span class="sxs-lookup"><span data-stu-id="6370a-180">(See [http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header).) The response headers that are available by default are:</span></span>

* <span data-ttu-id="6370a-181">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="6370a-181">Cache-Control</span></span>

* <span data-ttu-id="6370a-182">Content-Language</span><span class="sxs-lookup"><span data-stu-id="6370a-182">Content-Language</span></span>

* <span data-ttu-id="6370a-183">Inhaltstyp</span><span class="sxs-lookup"><span data-stu-id="6370a-183">Content-Type</span></span>

* <span data-ttu-id="6370a-184">Läuft ab</span><span class="sxs-lookup"><span data-stu-id="6370a-184">Expires</span></span>

* <span data-ttu-id="6370a-185">Zuletzt geändert</span><span class="sxs-lookup"><span data-stu-id="6370a-185">Last-Modified</span></span>

* <span data-ttu-id="6370a-186">Pragma</span><span class="sxs-lookup"><span data-stu-id="6370a-186">Pragma</span></span>

<span data-ttu-id="6370a-187">Die CORS-Spezifikation ruft diese *Header für die einfache Antwort*.</span><span class="sxs-lookup"><span data-stu-id="6370a-187">The CORS spec calls these *simple response headers*.</span></span> <span data-ttu-id="6370a-188">Um weitere Header an die Anwendung verfügbar zu machen:</span><span class="sxs-lookup"><span data-stu-id="6370a-188">To make other headers available to the application:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="6370a-189">Anmeldeinformationen im Cross-Origin-Anforderungen</span><span class="sxs-lookup"><span data-stu-id="6370a-189">Credentials in cross-origin requests</span></span>

<span data-ttu-id="6370a-190">Anmeldeinformationen erfordern besondere Behandlung in eine CORS-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="6370a-190">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="6370a-191">Standardmäßig sendet der Browser keine Anmeldeinformationen mit einer cors-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="6370a-191">By default, the browser doesn't send any credentials with a cross-origin request.</span></span> <span data-ttu-id="6370a-192">Anmeldeinformationen enthalten, Cookies sowie HTTP-Authentifizierungsschemas.</span><span class="sxs-lookup"><span data-stu-id="6370a-192">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="6370a-193">Um die Anmeldeinformationen mit einer cors-Anforderung zu senden, muss der Client XMLHttpRequest.withCredentials auf "true" festgelegt.</span><span class="sxs-lookup"><span data-stu-id="6370a-193">To send credentials with a cross-origin request, the client must set XMLHttpRequest.withCredentials to true.</span></span>

<span data-ttu-id="6370a-194">Mithilfe von XMLHttpRequest direkt:</span><span class="sxs-lookup"><span data-stu-id="6370a-194">Using XMLHttpRequest directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="6370a-195">In jQuery:</span><span class="sxs-lookup"><span data-stu-id="6370a-195">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="6370a-196">Darüber hinaus muss der Server die Anmeldeinformationen zulassen.</span><span class="sxs-lookup"><span data-stu-id="6370a-196">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="6370a-197">So ermöglichen Sie Cross-Origin-Anmeldeinformationen:</span><span class="sxs-lookup"><span data-stu-id="6370a-197">To allow cross-origin credentials:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=80-85)]

<span data-ttu-id="6370a-198">Jetzt umfasst die HTTP-Antwort einen Access-Control-Allow-Credentials-Header, der dem Browser darüber informiert, dass der Server die Anmeldeinformationen für eine Anforderung zwischen verschiedenen Ursprüngen zulässt.</span><span class="sxs-lookup"><span data-stu-id="6370a-198">Now the HTTP response will include an Access-Control-Allow-Credentials header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="6370a-199">Wenn der Browser sendet die Anmeldeinformationen, aber die Antwort keinen gültigen Access-Control-Allow-Credentials-Header enthalten, wird der Browser wird nicht die Antwort an die Anwendung verfügbar machen, und die AJAX-Anforderung ein Fehler auftritt.</span><span class="sxs-lookup"><span data-stu-id="6370a-199">If the browser sends credentials, but the response doesn't include a valid Access-Control-Allow-Credentials header, the browser won't expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="6370a-200">Seien Sie vorsichtig beim Cross-Origin-Anmeldeinformationen zulassen.</span><span class="sxs-lookup"><span data-stu-id="6370a-200">Be careful when allowing cross-origin credentials.</span></span> <span data-ttu-id="6370a-201">Eine Website in einer anderen Domäne kann die Anmeldeinformationen eines angemeldeten Benutzers an die app auf den Namen des Benutzers, ohne das Wissen des Benutzers senden.</span><span class="sxs-lookup"><span data-stu-id="6370a-201">A website at another domain can send a logged-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <span data-ttu-id="6370a-202">CORS-Spezifikation gibt auch an dieser Einstellung Ursprüngen `"*"` (alle Ursprünge) ist ungültig. wenn die `Access-Control-Allow-Credentials` Header vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="6370a-202">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="6370a-203">Die Preflight-Ablaufzeit festlegen</span><span class="sxs-lookup"><span data-stu-id="6370a-203">Set the preflight expiration time</span></span>

<span data-ttu-id="6370a-204">Die Access-Control-Max-Age-Header gibt an, wie lange die Antwort auf die preflight-Anforderung zwischengespeichert werden kann.</span><span class="sxs-lookup"><span data-stu-id="6370a-204">The Access-Control-Max-Age header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="6370a-205">So legen Sie diesen Header fest:</span><span class="sxs-lookup"><span data-stu-id="6370a-205">To set this header:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a><span data-ttu-id="6370a-206">Funktionsweise von CORS</span><span class="sxs-lookup"><span data-stu-id="6370a-206">How CORS works</span></span>

<span data-ttu-id="6370a-207">In diesem Abschnitt wird beschrieben, was geschieht, in einer CORS-Anforderung auf der Ebene der HTTP-Nachrichten.</span><span class="sxs-lookup"><span data-stu-id="6370a-207">This section describes what happens in a CORS request at the level of the HTTP messages.</span></span> <span data-ttu-id="6370a-208">Es ist wichtig zum Verständnis der Funktionsweise von CORS, damit die CORS-Richtlinie ordnungsgemäß konfiguriert und debuggt werden, wenn die zu unerwarteten Ergebnissen führen kann.</span><span class="sxs-lookup"><span data-stu-id="6370a-208">It's important to understand how CORS works so that the CORS policy can be configured correctly and debugged when unexpected behaviors occur.</span></span>

<span data-ttu-id="6370a-209">CORS-Spezifikation führt mehrere neue HTTP-Header, die Cross-Origin-Anforderungen zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="6370a-209">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="6370a-210">Wenn ein Browser CORS unterstützt, wird diese Header automatisch für Cross-Origin-Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="6370a-210">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="6370a-211">Benutzerdefinierte JavaScript-Code ist nicht erforderlich, um CORS zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="6370a-211">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="6370a-212">Hier ist ein Beispiel für eine cors-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="6370a-212">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="6370a-213">Die `Origin` Header enthält, die Domäne der Website, die die Anforderung gesendet wird:</span><span class="sxs-lookup"><span data-stu-id="6370a-213">The `Origin` header provides the domain of the site that's making the request:</span></span>

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

<span data-ttu-id="6370a-214">Wenn der Server die Anforderung zulässt, wird der Access-Control-Allow-Origin-Header in der Antwort.</span><span class="sxs-lookup"><span data-stu-id="6370a-214">If the server allows the request, it sets the Access-Control-Allow-Origin header in the response.</span></span> <span data-ttu-id="6370a-215">Der Wert dieses Headers entspricht den Origin-Header aus der Anforderung oder ist Sie den Platzhalterwert "\*", Bedeutung, die einen beliebigen Ursprung zulässig ist:</span><span class="sxs-lookup"><span data-stu-id="6370a-215">The value of this header either matches the Origin header from the request, or is the wildcard value "\*", meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="6370a-216">Die AJAX-Anforderung schlägt fehl, wenn die Antwort den Access-Control-Allow-Origin-Header enthalten, nicht.</span><span class="sxs-lookup"><span data-stu-id="6370a-216">If the response doesn't include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="6370a-217">Der Browser lässt, die die Anforderung.</span><span class="sxs-lookup"><span data-stu-id="6370a-217">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="6370a-218">Auch wenn der Server eine erfolgreiche Antwort zurückgibt, stellen nicht im Browser die Antwort an die Clientanwendung zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="6370a-218">Even if the server returns a successful response, the browser doesn't make the response available to the client application.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="6370a-219">Preflight-Anforderungen</span><span class="sxs-lookup"><span data-stu-id="6370a-219">Preflight Requests</span></span>

<span data-ttu-id="6370a-220">Für einige CORS-Anforderungen sendet der Browser eine zusätzliche Anforderung, die eine "preflight-Anforderung", aufgerufen, bevor die tatsächliche Anforderung für die Ressource gesendet.</span><span class="sxs-lookup"><span data-stu-id="6370a-220">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span> <span data-ttu-id="6370a-221">Der Browser kann die preflight-Anforderung überspringen, wenn die folgenden Bedingungen erfüllt sind:</span><span class="sxs-lookup"><span data-stu-id="6370a-221">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="6370a-222">Die Anforderungsmethode ist GET, HEAD oder POST, und</span><span class="sxs-lookup"><span data-stu-id="6370a-222">The request method is GET, HEAD, or POST, and</span></span>

* <span data-ttu-id="6370a-223">Die Anwendung nicht als Accept, Accept-Language, Inhaltssprache, Anforderungsheader festlegen Content-Type oder letzten-Ereignis-ID, und</span><span class="sxs-lookup"><span data-stu-id="6370a-223">The application doesn't set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, and</span></span>

* <span data-ttu-id="6370a-224">Der Content-Type-Header (falls festgelegt) ist eine der folgenden:</span><span class="sxs-lookup"><span data-stu-id="6370a-224">The Content-Type header (if set) is one of the following:</span></span>

  * <span data-ttu-id="6370a-225">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="6370a-225">application/x-www-form-urlencoded</span></span>

  * <span data-ttu-id="6370a-226">Multipart/Form-data</span><span class="sxs-lookup"><span data-stu-id="6370a-226">multipart/form-data</span></span>

  * <span data-ttu-id="6370a-227">Text/plain</span><span class="sxs-lookup"><span data-stu-id="6370a-227">text/plain</span></span>

<span data-ttu-id="6370a-228">Die Regel zu Anforderungsheader gilt für Header, die Anwendung wird durch Aufrufen von SetRequestHeader für das XMLHttpRequest-Objekt.</span><span class="sxs-lookup"><span data-stu-id="6370a-228">The rule about request headers applies to headers that the application sets by calling setRequestHeader on the XMLHttpRequest object.</span></span> <span data-ttu-id="6370a-229">(Die CORS-Spezifikation als diese "Author-Anforderungsheader" bezeichnet). Die Regel gilt nicht für Header, die der Browser, z. B. Benutzer-Agent, Host und Content-Length festlegen kann haben.</span><span class="sxs-lookup"><span data-stu-id="6370a-229">(The CORS specification calls these "author request headers".) The rule doesn't apply to headers the browser can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="6370a-230">Hier ist ein Beispiel für eine preflight-Anforderung:</span><span class="sxs-lookup"><span data-stu-id="6370a-230">Here is an example of a preflight request:</span></span>

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

<span data-ttu-id="6370a-231">Die Pre-Flight-Anforderung verwendet die HTTP OPTIONS-Methode.</span><span class="sxs-lookup"><span data-stu-id="6370a-231">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="6370a-232">Es enthält zwei spezielle Header:</span><span class="sxs-lookup"><span data-stu-id="6370a-232">It includes two special headers:</span></span>

* <span data-ttu-id="6370a-233">Access-Control-Request-Method: Die HTTP-Methode, die für die tatsächliche Anforderung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="6370a-233">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>

* <span data-ttu-id="6370a-234">Access-Control-Request-Headers: Eine Liste der Anforderungsheader, die die Anwendung für die tatsächliche Anforderung festgelegt.</span><span class="sxs-lookup"><span data-stu-id="6370a-234">Access-Control-Request-Headers: A list of request headers that the application set on the actual request.</span></span> <span data-ttu-id="6370a-235">(In diesem Fall nicht dazu gehören Header, die der Browser legt diese fest.)</span><span class="sxs-lookup"><span data-stu-id="6370a-235">(Again, this doesn't include headers that the browser sets.)</span></span>

<span data-ttu-id="6370a-236">Hier ist eine Beispielantwort, vorausgesetzt, dass der Server die Anforderung zulässt:</span><span class="sxs-lookup"><span data-stu-id="6370a-236">Here is an example response, assuming that the server allows the request:</span></span>

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

<span data-ttu-id="6370a-237">Die Antwort enthält eine Access-Control-Allow-Methods-Header, der die zulässigen Methoden aufgeführt, und optional eine Access-Control-Allow-Headers-Header, der die erlaubten Header aufgeführt sind.</span><span class="sxs-lookup"><span data-stu-id="6370a-237">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="6370a-238">Wenn die preflight-Anforderung erfolgreich ist, sendet der Browser die tatsächliche Anforderung an, wie oben beschrieben.</span><span class="sxs-lookup"><span data-stu-id="6370a-238">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>
