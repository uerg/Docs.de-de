---
title: Aktivieren Sie Ursprungsübergreifender Anforderungen (CORS) in ASP.NET Core
author: rick-anderson
description: Erfahren Sie, wie CORS als Standard zum Zulassen oder ablehnen von ursprungsübergreifenden Anforderungen in einer ASP.NET Core-app.
ms.author: riande
ms.custom: mvc
ms.date: 11/27/2018
uid: security/cors
ms.openlocfilehash: f0e01cfa618184d8a3b19c06212dc3914183a2e4
ms.sourcegitcommit: e7fafb153b9de7595c2558a0133f8d1c33a3bddb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2018
ms.locfileid: "52458542"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="be22d-103">Aktivieren Sie Ursprungsübergreifender Anforderungen (CORS) in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="be22d-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="be22d-104">Durch [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), und [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="be22d-104">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="be22d-105">Browsersicherheit verhindert, dass eine Webseite auf Anfragen zu einer anderen Domäne als der, die die Webseite verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="be22d-105">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="be22d-106">Diese Einschränkung wird aufgerufen, die *Richtlinie desselben Ursprungs*.</span><span class="sxs-lookup"><span data-stu-id="be22d-106">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="be22d-107">Die Richtlinie desselben Ursprungs wird verhindert, dass eine schädliche Website sensible Daten von einem anderen Standort lesen.</span><span class="sxs-lookup"><span data-stu-id="be22d-107">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="be22d-108">In einigen Fällen empfiehlt es sich um zuzulassen, dass andere Standorte Cross-Origin-Anforderungen zu Ihrer app durchführen.</span><span class="sxs-lookup"><span data-stu-id="be22d-108">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span>

<span data-ttu-id="be22d-109">[Cross-Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS) ist ein W3C-Standard, der einem Server zu lockern die Richtlinie des gleichen Ursprungs ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="be22d-109">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="be22d-110">Mit CORS kann ein Server explizit einige ursprungsübergreifende Anforderungen zulassen und andere ablehnen.</span><span class="sxs-lookup"><span data-stu-id="be22d-110">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="be22d-111">CORS ist sicherer und flexibler als frühere Techniken wie z. B. [JSONP](https://wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="be22d-111">CORS is safer and more flexible than earlier techniques, such as [JSONP](https://wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="be22d-112">In diesem Thema veranschaulicht das Aktivieren von CORS in einer ASP.NET Core-app.</span><span class="sxs-lookup"><span data-stu-id="be22d-112">This topic shows how to enable CORS in an ASP.NET Core app.</span></span>

## <a name="same-origin"></a><span data-ttu-id="be22d-113">Denselben Ursprung</span><span class="sxs-lookup"><span data-stu-id="be22d-113">Same origin</span></span>

<span data-ttu-id="be22d-114">Zwei URLs haben denselben Ursprung ggf. identische Schemas, Hosts und -Ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="be22d-114">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="be22d-115">Diese zwei URLs haben denselben Ursprung an:</span><span class="sxs-lookup"><span data-stu-id="be22d-115">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="be22d-116">Diese URLs haben verschiedene Ursprünge als die vorherigen beiden URLs:</span><span class="sxs-lookup"><span data-stu-id="be22d-116">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="be22d-117">`https://example.net` &ndash; Andere Domäne</span><span class="sxs-lookup"><span data-stu-id="be22d-117">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="be22d-118">`https://www.example.com/foo.html` &ndash; Andere Unterdomäne</span><span class="sxs-lookup"><span data-stu-id="be22d-118">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="be22d-119">`http://example.com/foo.html` &ndash; Anderes Schema</span><span class="sxs-lookup"><span data-stu-id="be22d-119">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="be22d-120">`https://example.com:9000/foo.html` &ndash; Portnummer</span><span class="sxs-lookup"><span data-stu-id="be22d-120">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

> [!NOTE]
> <span data-ttu-id="be22d-121">Internet Explorer berücksichtigen nicht den Port Ursprünge zu vergleichen.</span><span class="sxs-lookup"><span data-stu-id="be22d-121">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="register-cors-services"></a><span data-ttu-id="be22d-122">CORS-Dienste registrieren</span><span class="sxs-lookup"><span data-stu-id="be22d-122">Register CORS services</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="be22d-123">Referenz der [Microsoft.AspNetCore.App metapaket](xref:fundamentals/metapackage-app) oder fügen Sie einen Paketverweis auf die [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) Paket.</span><span class="sxs-lookup"><span data-stu-id="be22d-123">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="be22d-124">Referenz der [metapaket "Microsoft.aspnetcore.All"](xref:fundamentals/metapackage) oder fügen Sie einen Paketverweis auf die [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) Paket.</span><span class="sxs-lookup"><span data-stu-id="be22d-124">Reference the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) or add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="be22d-125">Fügen Sie einen Paketverweis auf die [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) Paket.</span><span class="sxs-lookup"><span data-stu-id="be22d-125">Add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

<span data-ttu-id="be22d-126">Rufen Sie <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> in `Startup.ConfigureServices` CORS-Dienste in der app Service-Container hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="be22d-126">Call <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> in `Startup.ConfigureServices` to add CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors&highlight=3)]

## <a name="enable-cors"></a><span data-ttu-id="be22d-127">Aktivieren von CORS</span><span class="sxs-lookup"><span data-stu-id="be22d-127">Enable CORS</span></span>

<span data-ttu-id="be22d-128">Verwenden Sie nach der Registrierung CORS-Dienste einen der folgenden Ansätze zum Aktivieren von CORS in einer ASP.NET Core-app aus:</span><span class="sxs-lookup"><span data-stu-id="be22d-128">After registering CORS services, use either of the following approaches to enable CORS in an ASP.NET Core app:</span></span>

* <span data-ttu-id="be22d-129">[CORS-Middleware](#enable-cors-with-cors-middleware) &ndash; Richtlinien für CORS gelten global für die app über Middleware.</span><span class="sxs-lookup"><span data-stu-id="be22d-129">[CORS Middleware](#enable-cors-with-cors-middleware) &ndash; Apply CORS policies globally to the app via middleware.</span></span>
* <span data-ttu-id="be22d-130">[Von CORS in MVC](#enable-cors-in-mvc) &ndash; CORS gelten Richtlinien pro Aktion oder pro Controller.</span><span class="sxs-lookup"><span data-stu-id="be22d-130">[CORS in MVC](#enable-cors-in-mvc) &ndash; Apply CORS policies per action or per controller.</span></span> <span data-ttu-id="be22d-131">CORS-Middleware wird nicht verwendet.</span><span class="sxs-lookup"><span data-stu-id="be22d-131">CORS Middleware isn't used.</span></span>

### <a name="enable-cors-with-cors-middleware"></a><span data-ttu-id="be22d-132">Aktivieren von CORS mit CORS-Middleware</span><span class="sxs-lookup"><span data-stu-id="be22d-132">Enable CORS with CORS Middleware</span></span>

<span data-ttu-id="be22d-133">CORS-Middleware verarbeitet Cross-Origin-Anforderungen an die app.</span><span class="sxs-lookup"><span data-stu-id="be22d-133">CORS Middleware handles cross-origin requests to the app.</span></span> <span data-ttu-id="be22d-134">Rufen Sie zum Aktivieren von CORS-Middleware in der Anforderungsverarbeitungspipeline der <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> Erweiterungsmethode in `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="be22d-134">To enable CORS Middleware in the request processing pipeline, call the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method in `Startup.Configure`.</span></span>

<span data-ttu-id="be22d-135">CORS-Middleware muss vor stehen Endpunkte definiert in Ihrer app, wo Sie möchten für die Unterstützung von Cross-Origin-Anfragen (z. B. vor dem Aufruf von `UseMvc` für MVC-Razor-Seiten-Middleware).</span><span class="sxs-lookup"><span data-stu-id="be22d-135">CORS Middleware must precede any defined endpoints in your app where you want to support cross-origin requests (for example, before the call to `UseMvc` for MVC/Razor Pages Middleware).</span></span>

<span data-ttu-id="be22d-136">Ein *ursprungsübergreifende Richtlinie* kann angegeben werden, beim Hinzufügen der CORS-Middleware mithilfe der <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> Klasse.</span><span class="sxs-lookup"><span data-stu-id="be22d-136">A *cross-origin policy* can be specified when adding the CORS Middleware using the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> class.</span></span> <span data-ttu-id="be22d-137">Es gibt zwei Ansätze für eine CORS-Richtlinie definieren:</span><span class="sxs-lookup"><span data-stu-id="be22d-137">There are two approaches for defining a CORS policy:</span></span>

* <span data-ttu-id="be22d-138">Rufen Sie `UseCors` mit einem Lambda-Ausdruck:</span><span class="sxs-lookup"><span data-stu-id="be22d-138">Call `UseCors` with a lambda:</span></span>

  [!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

  <span data-ttu-id="be22d-139">Das Lambda akzeptiert ein <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> Objekt.</span><span class="sxs-lookup"><span data-stu-id="be22d-139">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="be22d-140">[Optionen für die Konfiguration](#cors-policy-options), z. B. `WithOrigins`, werden weiter unten in diesem Thema.</span><span class="sxs-lookup"><span data-stu-id="be22d-140">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this topic.</span></span> <span data-ttu-id="be22d-141">Im vorherigen Beispiel kann die Richtlinie Cross-Origin-Anforderungen von `https://example.com` und keine anderen Quellen.</span><span class="sxs-lookup"><span data-stu-id="be22d-141">In the preceding example, the policy allows cross-origin requests from `https://example.com` and no other origins.</span></span>

  <span data-ttu-id="be22d-142">Die URL muss angegeben werden, ohne nachgestellten Schrägstrich (`/`).</span><span class="sxs-lookup"><span data-stu-id="be22d-142">The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="be22d-143">Wenn die URL mit beendet `/`, gibt der Vergleich `false` und es wird kein Header zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="be22d-143">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

  <span data-ttu-id="be22d-144">`CorsPolicyBuilder` verfügt über eine fluent-API, damit Sie die Methodenaufrufe verketten können:</span><span class="sxs-lookup"><span data-stu-id="be22d-144">`CorsPolicyBuilder` has a fluent API, so you can chain method calls:</span></span>

  [!code-csharp[](cors/sample/CorsExample3/Startup.cs?highlight=2-3&range=29-32)]

* <span data-ttu-id="be22d-145">Definieren Sie mindestens eine benannte CORS-Richtlinie, und wählen Sie die Richtlinie anhand des Namens zur Laufzeit.</span><span class="sxs-lookup"><span data-stu-id="be22d-145">Define one or more named CORS policies and select the policy by name at runtime.</span></span> <span data-ttu-id="be22d-146">Im folgenden Beispiel wird eine benutzerdefiniertes CORS-Richtlinie mit dem Namen *AllowSpecificOrigin*.</span><span class="sxs-lookup"><span data-stu-id="be22d-146">The following example adds a user-defined CORS policy named *AllowSpecificOrigin*.</span></span> <span data-ttu-id="be22d-147">Um die Richtlinie auswählen, übergeben Sie den Namen in `UseCors`:</span><span class="sxs-lookup"><span data-stu-id="be22d-147">To select the policy, pass the name to `UseCors`:</span></span>

  [!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin&highlight=5-6,21)]

### <a name="enable-cors-in-mvc"></a><span data-ttu-id="be22d-148">Aktivieren von CORS in MVC</span><span class="sxs-lookup"><span data-stu-id="be22d-148">Enable CORS in MVC</span></span>

<span data-ttu-id="be22d-149">Sie können alternativ MVC verwenden, um Richtlinien für bestimmte CORS pro Aktion oder pro Controller anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="be22d-149">You can alternatively use MVC to apply specific CORS policies per action or per controller.</span></span> <span data-ttu-id="be22d-150">Wenn Sie MVC verwenden, um CORS zu aktivieren, werden die registrierten CORS-Dienste verwendet.</span><span class="sxs-lookup"><span data-stu-id="be22d-150">When using MVC to enable CORS, the registered CORS services are used.</span></span> <span data-ttu-id="be22d-151">Die CORS-Middleware wird nicht verwendet.</span><span class="sxs-lookup"><span data-stu-id="be22d-151">The CORS Middleware isn't used.</span></span>

### <a name="per-action"></a><span data-ttu-id="be22d-152">Pro Aktion</span><span class="sxs-lookup"><span data-stu-id="be22d-152">Per action</span></span>

<span data-ttu-id="be22d-153">Um eine CORS-Richtlinie für eine bestimmte Aktion anzugeben, fügen die [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) -Attribut auf die Aktion.</span><span class="sxs-lookup"><span data-stu-id="be22d-153">To specify a CORS policy for a specific action, add the [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute to the action.</span></span> <span data-ttu-id="be22d-154">Geben Sie den Namen der Richtlinie.</span><span class="sxs-lookup"><span data-stu-id="be22d-154">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction&highlight=2)]

### <a name="per-controller"></a><span data-ttu-id="be22d-155">Pro controller</span><span class="sxs-lookup"><span data-stu-id="be22d-155">Per controller</span></span>

<span data-ttu-id="be22d-156">Fügen Sie zum Angeben der CORS-Richtlinie für einen bestimmten Controller die [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) -Attribut auf die Controllerklasse.</span><span class="sxs-lookup"><span data-stu-id="be22d-156">To specify the CORS policy for a specific controller, add the [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute to the controller class.</span></span> <span data-ttu-id="be22d-157">Geben Sie den Namen der Richtlinie.</span><span class="sxs-lookup"><span data-stu-id="be22d-157">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController&highlight=2)]

<span data-ttu-id="be22d-158">Die Rangfolge ist:</span><span class="sxs-lookup"><span data-stu-id="be22d-158">The precedence order is:</span></span>

1. <span data-ttu-id="be22d-159">action</span><span class="sxs-lookup"><span data-stu-id="be22d-159">action</span></span>
1. <span data-ttu-id="be22d-160">Controller</span><span class="sxs-lookup"><span data-stu-id="be22d-160">controller</span></span>

### <a name="disable-cors"></a><span data-ttu-id="be22d-161">Deaktivieren von CORS</span><span class="sxs-lookup"><span data-stu-id="be22d-161">Disable CORS</span></span>

<span data-ttu-id="be22d-162">Wenn Sie um CORS für einen Controller oder die Aktion zu deaktivieren, verwenden die [ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) Attribut:</span><span class="sxs-lookup"><span data-stu-id="be22d-162">To disable CORS for a controller or action, use the [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute:</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction&highlight=2)]

## <a name="cors-policy-options"></a><span data-ttu-id="be22d-163">CORS-Richtlinie-Optionen</span><span class="sxs-lookup"><span data-stu-id="be22d-163">CORS policy options</span></span>

<span data-ttu-id="be22d-164">Dieser Abschnitt beschreibt die verschiedenen Optionen, die Sie in einer CORS-Richtlinie festlegen können.</span><span class="sxs-lookup"><span data-stu-id="be22d-164">This section describes the various options that you can set in a CORS policy.</span></span> <span data-ttu-id="be22d-165">Die <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> Methode wird aufgerufen, `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="be22d-165">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> method is called in `Startup.ConfigureServices`.</span></span>

* [<span data-ttu-id="be22d-166">Legen Sie die zulässigen Ursprünge</span><span class="sxs-lookup"><span data-stu-id="be22d-166">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="be22d-167">Legen Sie die zulässigen HTTP-Methoden</span><span class="sxs-lookup"><span data-stu-id="be22d-167">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="be22d-168">Legt die zulässigen Anforderungsheader fest.</span><span class="sxs-lookup"><span data-stu-id="be22d-168">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="be22d-169">Legen Sie die verfügbar gemachten Antwortheader</span><span class="sxs-lookup"><span data-stu-id="be22d-169">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="be22d-170">Anmeldeinformationen im Cross-Origin-Anforderungen</span><span class="sxs-lookup"><span data-stu-id="be22d-170">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="be22d-171">Die Preflight-Ablaufzeit festlegen</span><span class="sxs-lookup"><span data-stu-id="be22d-171">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="be22d-172">Für einige Optionen, es kann hilfreich sein, lesen Sie die [funktioniert wie CORS](#how-cors-works) im ersten Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="be22d-172">For some options, it may be helpful to read the [How CORS works](#how-cors-works) section first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="be22d-173">Legen Sie die zulässigen Ursprünge</span><span class="sxs-lookup"><span data-stu-id="be22d-173">Set the allowed origins</span></span>

<span data-ttu-id="be22d-174">Die CORS-Middleware in ASP.NET Core MVC verfügt über einige Möglichkeiten zum Angeben der zulässigen Ursprünge:</span><span class="sxs-lookup"><span data-stu-id="be22d-174">The CORS middleware in ASP.NET Core MVC has a few ways to specify allowed origins:</span></span>

* <span data-ttu-id="be22d-175"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*> &ndash; Ermöglicht die Angabe einer oder mehrerer URLs.</span><span class="sxs-lookup"><span data-stu-id="be22d-175"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*> &ndash; Allows specifying one or more URLs.</span></span> <span data-ttu-id="be22d-176">Die URL kann es sich um das Schema, Host-Name und Port ohne Pfadinformationen enthalten.</span><span class="sxs-lookup"><span data-stu-id="be22d-176">The URL may include the scheme, host name, and port without any path information.</span></span> <span data-ttu-id="be22d-177">Beispielsweise `https://example.com`.</span><span class="sxs-lookup"><span data-stu-id="be22d-177">For example, `https://example.com`.</span></span> <span data-ttu-id="be22d-178">Die URL muss angegeben werden, ohne nachgestellten Schrägstrich (`/`).</span><span class="sxs-lookup"><span data-stu-id="be22d-178">The URL must be specified without a trailing slash (`/`).</span></span>

  [!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=20-25&highlight=4-5)]

* <span data-ttu-id="be22d-179"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; CORS-Anforderungen von der alle Ursprünge mit einem beliebigen Schema zulässig (`http` oder `https`).</span><span class="sxs-lookup"><span data-stu-id="be22d-179"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span>

  [!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=29-33&highlight=4)]

  <span data-ttu-id="be22d-180">Wägen Sie sorgfältig, bevor Sie die Anforderungen von einem beliebigen Ursprung zulassen.</span><span class="sxs-lookup"><span data-stu-id="be22d-180">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="be22d-181">Dadurch können Anforderungen von einem beliebigen Ursprung bedeutet, dass *eine beliebige Website* Cross-Origin-Anfragen an Ihre app vornehmen können.</span><span class="sxs-lookup"><span data-stu-id="be22d-181">Allowing requests from any origin means that *any website* can make cross-origin requests to your app.</span></span>

  ::: moniker range=">= aspnetcore-2.2"

  > [!NOTE]
  > <span data-ttu-id="be22d-182">Angeben von `AllowAnyOrigin` und `AllowCredentials` ist eine unsichere Konfiguration und kann dazu führen, siteübergreifende anforderungsfälschung.</span><span class="sxs-lookup"><span data-stu-id="be22d-182">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="be22d-183">Der CORS-Dienst gibt eine ungültige CORS-Antwort zurück, wenn eine app mit beiden Methoden konfiguriert ist.</span><span class="sxs-lookup"><span data-stu-id="be22d-183">The CORS service returns an invalid CORS response when an app is configured with both methods.</span></span>

  ::: moniker-end

  ::: moniker range="< aspnetcore-2.2"

  > [!NOTE]
  > <span data-ttu-id="be22d-184">Angeben von `AllowAnyOrigin` und `AllowCredentials` ist eine unsichere Konfiguration und kann dazu führen, siteübergreifende anforderungsfälschung.</span><span class="sxs-lookup"><span data-stu-id="be22d-184">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="be22d-185">Geben Sie ggf. eine genaue Liste der Ursprünge, wenn der Client selbst auf Serverressourcen autorisieren muss.</span><span class="sxs-lookup"><span data-stu-id="be22d-185">Consider specifying an exact list of origins if the client must authorize itself to access server resources.</span></span>

  ::: moniker-end

  <span data-ttu-id="be22d-186">Diese Einstellung wirkt sich auf die preflight-Anforderungen und die `Access-Control-Allow-Origin` Header.</span><span class="sxs-lookup"><span data-stu-id="be22d-186">This setting affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="be22d-187">Weitere Informationen finden Sie unter den [Preflight-Anforderungen](#preflight-requests) Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="be22d-187">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="be22d-188"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Legt die <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> Eigenschaft der Richtlinie, die eine Funktion sein, ermöglicht Ursprünge zu eine Domäne konfigurierten mit Platzhaltern entsprechen, bei der Auswertung, wenn der Ursprung zulässig ist.</span><span class="sxs-lookup"><span data-stu-id="be22d-188"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcarded domain when evaluating if the origin is allowed.</span></span>

  [!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-104&highlight=4)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="be22d-189">Legen Sie die zulässigen HTTP-Methoden</span><span class="sxs-lookup"><span data-stu-id="be22d-189">Set the allowed HTTP methods</span></span>

<span data-ttu-id="be22d-190">Um alle HTTP-Methoden zu ermöglichen, rufen <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="be22d-190">To allow all HTTP methods, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=46-51&highlight=5)]

<span data-ttu-id="be22d-191">Diese Einstellung wirkt sich auf die preflight-Anforderungen und die `Access-Control-Allow-Methods` Header.</span><span class="sxs-lookup"><span data-stu-id="be22d-191">This setting affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="be22d-192">Weitere Informationen finden Sie unter den [Preflight-Anforderungen](#preflight-requests) Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="be22d-192">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="be22d-193">Legt die zulässigen Anforderungsheader fest.</span><span class="sxs-lookup"><span data-stu-id="be22d-193">Set the allowed request headers</span></span>

<span data-ttu-id="be22d-194">Wird aufgerufen, um bestimmte Header in einer CORS-Anforderung gesendet werden können *erstellen Anforderungsheader*, rufen Sie <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> , und geben Sie die erlaubten Header:</span><span class="sxs-lookup"><span data-stu-id="be22d-194">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="be22d-195">Um zuzulassen, die alle erstellen Anforderungsheader, rufen Sie <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="be22d-195">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="be22d-196">Diese Einstellung wirkt sich auf die preflight-Anforderungen und die `Access-Control-Request-Headers` Header.</span><span class="sxs-lookup"><span data-stu-id="be22d-196">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="be22d-197">Weitere Informationen finden Sie unter den [Preflight-Anforderungen](#preflight-requests) Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="be22d-197">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="be22d-198">Eine Übereinstimmung der CORS-Middleware-Richtlinien auf bestimmte Header gemäß `WithHeaders` ist nur möglich, wenn die Header gesendet wird, `Access-Control-Request-Headers` genau die Header in angegebenen `WithHeaders`.</span><span class="sxs-lookup"><span data-stu-id="be22d-198">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="be22d-199">Betrachten Sie beispielsweise eine app wie folgt konfiguriert:</span><span class="sxs-lookup"><span data-stu-id="be22d-199">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="be22d-200">CORS-Middleware eine preflight-Anforderung mit der folgenden Anforderungsheader ablehnt, da `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) ist nicht aufgeführt, `WithHeaders`:</span><span class="sxs-lookup"><span data-stu-id="be22d-200">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="be22d-201">Die app-gibt ein *200 OK* Antwort jedoch nicht wieder die CORS-Header sendet.</span><span class="sxs-lookup"><span data-stu-id="be22d-201">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="be22d-202">Aus diesem Grund versucht der Browser nicht die cors-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="be22d-202">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="be22d-203">CORS-Middleware können immer vier Header in der `Access-Control-Request-Headers` unabhängig von den Werten in CorsPolicy.Headers konfiguriert gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="be22d-203">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="be22d-204">Diese Liste der Header enthält:</span><span class="sxs-lookup"><span data-stu-id="be22d-204">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="be22d-205">Betrachten Sie beispielsweise eine app wie folgt konfiguriert:</span><span class="sxs-lookup"><span data-stu-id="be22d-205">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="be22d-206">CORS-Middleware erfolgreich beantwortet eine preflight-Anforderung mit der folgenden Anforderungsheader da `Content-Language` ist immer in der Whitelist enthalten:</span><span class="sxs-lookup"><span data-stu-id="be22d-206">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="be22d-207">Legen Sie die verfügbar gemachten Antwortheader</span><span class="sxs-lookup"><span data-stu-id="be22d-207">Set the exposed response headers</span></span>

<span data-ttu-id="be22d-208">Standardmäßig nicht im Browser alle Antwortheader für die app verfügbar machen.</span><span class="sxs-lookup"><span data-stu-id="be22d-208">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="be22d-209">Weitere Informationen finden Sie unter [W3C Cross-Origin Resource Sharing (Terminologie): einfache Antwortheader](https://www.w3.org/TR/cors/#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="be22d-209">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="be22d-210">Die Antwortheader, die standardmäßig verfügbar sind:</span><span class="sxs-lookup"><span data-stu-id="be22d-210">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="be22d-211">CORS-Spezifikation ruft diese Header *Header für die einfache Antwort*.</span><span class="sxs-lookup"><span data-stu-id="be22d-211">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="be22d-212">Um weitere Header für die app verfügbar zu machen, rufen Sie <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="be22d-212">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="be22d-213">Anmeldeinformationen im Cross-Origin-Anforderungen</span><span class="sxs-lookup"><span data-stu-id="be22d-213">Credentials in cross-origin requests</span></span>

<span data-ttu-id="be22d-214">Anmeldeinformationen erfordern besondere Behandlung in eine CORS-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="be22d-214">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="be22d-215">Standardmäßig sendet der Browser keine Anmeldeinformationen mit einer cors-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="be22d-215">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="be22d-216">Anmeldeinformationen enthalten, Cookies und HTTP-Authentifizierungsschemas.</span><span class="sxs-lookup"><span data-stu-id="be22d-216">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="be22d-217">Um die Anmeldeinformationen mit einer cors-Anforderung zu senden, muss der Client festgelegt `XMLHttpRequest.withCredentials` zu `true`.</span><span class="sxs-lookup"><span data-stu-id="be22d-217">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="be22d-218">Mithilfe von `XMLHttpRequest` direkt:</span><span class="sxs-lookup"><span data-stu-id="be22d-218">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="be22d-219">In jQuery:</span><span class="sxs-lookup"><span data-stu-id="be22d-219">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'https://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="be22d-220">Darüber hinaus muss der Server die Anmeldeinformationen zulassen.</span><span class="sxs-lookup"><span data-stu-id="be22d-220">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="be22d-221">Um ursprungsübergreifende Anmeldeinformationen ermöglichen möchten, rufen <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span><span class="sxs-lookup"><span data-stu-id="be22d-221">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="be22d-222">Die HTTP-Antwort enthält ein `Access-Control-Allow-Credentials` -Header, der dem Browser darüber informiert, dass der Server die Anmeldeinformationen für eine Anforderung zwischen verschiedenen Ursprüngen zulässt.</span><span class="sxs-lookup"><span data-stu-id="be22d-222">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="be22d-223">Wenn der Browser sendet die Anmeldeinformationen, aber die Antwort auf eine gültige beinhaltet keine `Access-Control-Allow-Credentials` -Header, der Browser nicht die Antwort auf die app verfügbar zu machen, und die cors-Anforderung schlägt fehl.</span><span class="sxs-lookup"><span data-stu-id="be22d-223">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="be22d-224">Seien Sie vorsichtig beim Cross-Origin-Anmeldeinformationen zulassen.</span><span class="sxs-lookup"><span data-stu-id="be22d-224">Be careful when allowing cross-origin credentials.</span></span> <span data-ttu-id="be22d-225">Eine Website in einer anderen Domäne kann die Anmeldeinformationen eines angemeldeten Benutzers an die app auf den Namen des Benutzers, ohne das Wissen des Benutzers senden.</span><span class="sxs-lookup"><span data-stu-id="be22d-225">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span>

<span data-ttu-id="be22d-226">CORS-Spezifikation gibt auch an dieser Einstellung Ursprüngen `"*"` (alle Ursprünge) ist ungültig. wenn die `Access-Control-Allow-Credentials` Header vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="be22d-226">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="be22d-227">Preflight-Anforderungen</span><span class="sxs-lookup"><span data-stu-id="be22d-227">Preflight requests</span></span>

<span data-ttu-id="be22d-228">Für einige CORS-Anforderungen sendet der Browser eine zusätzliche Anforderung vor der tatsächlichen Anforderung an.</span><span class="sxs-lookup"><span data-stu-id="be22d-228">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="be22d-229">Diese Anforderung wird aufgerufen, eine *preflight-Anforderung*.</span><span class="sxs-lookup"><span data-stu-id="be22d-229">This request is called a *preflight request*.</span></span> <span data-ttu-id="be22d-230">Der Browser kann die preflight-Anforderung überspringen, wenn die folgenden Bedingungen erfüllt sind:</span><span class="sxs-lookup"><span data-stu-id="be22d-230">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="be22d-231">Die Anforderungsmethode ist GET, HEAD oder POST.</span><span class="sxs-lookup"><span data-stu-id="be22d-231">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="be22d-232">Die app keine Anforderungsheader außer festlegen `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, oder `Last-Event-ID`.</span><span class="sxs-lookup"><span data-stu-id="be22d-232">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="be22d-233">Die `Content-Type` -Header, wenn festgelegt ist, verfügt über mindestens einen der folgenden Werte:</span><span class="sxs-lookup"><span data-stu-id="be22d-233">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="be22d-234">Die Regel auf die Anforderungsheader festzulegen, für die Header die Anforderung des Clients gilt, die die app durch den Aufruf festlegt `setRequestHeader` auf die `XMLHttpRequest` Objekt.</span><span class="sxs-lookup"><span data-stu-id="be22d-234">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="be22d-235">CORS-Spezifikation ruft diese Header *erstellen Anforderungsheader*.</span><span class="sxs-lookup"><span data-stu-id="be22d-235">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="be22d-236">Die Regel trifft nicht auf der Browser, wie z. B. festlegen kann Header `User-Agent`, `Host`, oder `Content-Length`.</span><span class="sxs-lookup"><span data-stu-id="be22d-236">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="be22d-237">Im folgenden finden ein Beispiel für eine preflight-Anforderung:</span><span class="sxs-lookup"><span data-stu-id="be22d-237">The following is an example of a preflight request:</span></span>

```
OPTIONS https://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: https://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

<span data-ttu-id="be22d-238">Die Pre-Flight-Anforderung verwendet die HTTP OPTIONS-Methode.</span><span class="sxs-lookup"><span data-stu-id="be22d-238">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="be22d-239">Es enthält zwei spezielle Header:</span><span class="sxs-lookup"><span data-stu-id="be22d-239">It includes two special headers:</span></span>

* <span data-ttu-id="be22d-240">`Access-Control-Request-Method`: Die HTTP-Methode, die für die tatsächliche Anforderung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="be22d-240">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="be22d-241">`Access-Control-Request-Headers`: Eine Liste der Anforderungsheader, die die app für die tatsächliche Anforderung festlegt.</span><span class="sxs-lookup"><span data-stu-id="be22d-241">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="be22d-242">Wie bereits erwähnt, Dies beinhaltet keine Header, die der Browser legt, wie z. B. fest `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="be22d-242">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="be22d-243">Eine CORS-preflight-Anforderung sind zum Beispiel eine `Access-Control-Request-Headers` -Header, der auf dem Server die Header gibt an, die mit der tatsächlichen Anforderung gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="be22d-243">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="be22d-244">Um bestimmte Header zu ermöglichen, rufen <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="be22d-244">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="be22d-245">Um zuzulassen, die alle erstellen Anforderungsheader, rufen Sie <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="be22d-245">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="be22d-246">Browser sind nicht vollständig konsistent in diese Festlegung `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="be22d-246">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="be22d-247">Wenn Sie Header nicht festgelegt `"*"` (oder verwenden Sie <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), Sie sollten mindestens einschließen `Accept`, `Content-Type`, und `Origin`, sowie alle benutzerdefinierten Header, die Sie unterstützen möchten.</span><span class="sxs-lookup"><span data-stu-id="be22d-247">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="be22d-248">Im folgenden finden eine Beispielantwort auf die preflight-Anforderung (vorausgesetzt, dass der Server die Anforderung zulässt):</span><span class="sxs-lookup"><span data-stu-id="be22d-248">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

<span data-ttu-id="be22d-249">Die Antwort enthält ein `Access-Control-Allow-Methods` Header, der die zulässigen Methoden enthält und optional eine `Access-Control-Allow-Headers` -Header, der die erlaubten Header aufgeführt sind.</span><span class="sxs-lookup"><span data-stu-id="be22d-249">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="be22d-250">Wenn die preflight-Anforderung erfolgreich ist, sendet der Browser die tatsächliche Anforderung an.</span><span class="sxs-lookup"><span data-stu-id="be22d-250">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="be22d-251">Wenn die preflight-Anforderung abgelehnt wird, gibt die app eine *200 OK* Antwort jedoch nicht wieder die CORS-Header sendet.</span><span class="sxs-lookup"><span data-stu-id="be22d-251">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="be22d-252">Aus diesem Grund versucht der Browser nicht die cors-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="be22d-252">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="be22d-253">Die Preflight-Ablaufzeit festlegen</span><span class="sxs-lookup"><span data-stu-id="be22d-253">Set the preflight expiration time</span></span>

<span data-ttu-id="be22d-254">Die `Access-Control-Max-Age` Header gibt an, wie lange die Antwort auf die preflight-Anforderung zwischengespeichert werden kann.</span><span class="sxs-lookup"><span data-stu-id="be22d-254">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="be22d-255">Rufen Sie zum Festlegen dieser Header <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span><span class="sxs-lookup"><span data-stu-id="be22d-255">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

## <a name="how-cors-works"></a><span data-ttu-id="be22d-256">Funktionsweise von CORS</span><span class="sxs-lookup"><span data-stu-id="be22d-256">How CORS works</span></span>

<span data-ttu-id="be22d-257">In diesem Abschnitt wird beschrieben, was geschieht, in einer CORS-Anforderung auf der Ebene der HTTP-Nachrichten.</span><span class="sxs-lookup"><span data-stu-id="be22d-257">This section describes what happens in a CORS request at the level of the HTTP messages.</span></span> <span data-ttu-id="be22d-258">Es ist wichtig zum Verständnis der Funktionsweise von CORS, damit die CORS-Richtlinie ordnungsgemäß konfiguriert und debuggt werden, wenn die zu unerwarteten Ergebnissen führen kann.</span><span class="sxs-lookup"><span data-stu-id="be22d-258">It's important to understand how CORS works so that the CORS policy can be configured correctly and debugged when unexpected behaviors occur.</span></span>

<span data-ttu-id="be22d-259">CORS-Spezifikation führt mehrere neue HTTP-Header, die Cross-Origin-Anforderungen zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="be22d-259">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="be22d-260">Wenn ein Browser CORS unterstützt, wird diese Header automatisch für Cross-Origin-Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="be22d-260">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="be22d-261">Benutzerdefinierte JavaScript-Code ist nicht erforderlich, um CORS zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="be22d-261">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="be22d-262">Folgendes ist ein Beispiel für eine cors-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="be22d-262">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="be22d-263">Die `Origin` Header enthält, die Domäne der Website, die die Anforderung gesendet wird:</span><span class="sxs-lookup"><span data-stu-id="be22d-263">The `Origin` header provides the domain of the site that's making the request:</span></span>

```
GET https://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: https://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: https://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

<span data-ttu-id="be22d-264">Wenn der Server die Anforderung zulässt, wird die `Access-Control-Allow-Origin` Header in der Antwort.</span><span class="sxs-lookup"><span data-stu-id="be22d-264">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="be22d-265">Entweder mit dem Wert dieses Headers übereinstimmt der `Origin` Header aus der Anforderung oder der Platzhalterwert `"*"`, Bedeutung, die einen beliebigen Ursprung zulässig ist:</span><span class="sxs-lookup"><span data-stu-id="be22d-265">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

<span data-ttu-id="be22d-266">Wenn die Antwort enthalten nicht die `Access-Control-Allow-Origin` -Header, die Cross-Origin-Anforderung schlägt fehl.</span><span class="sxs-lookup"><span data-stu-id="be22d-266">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="be22d-267">Der Browser lässt, die die Anforderung.</span><span class="sxs-lookup"><span data-stu-id="be22d-267">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="be22d-268">Selbst wenn der Server eine erfolgreiche Antwort gibt zurück, machen nicht im Browser die Antwort für die Client-app verfügbar.</span><span class="sxs-lookup"><span data-stu-id="be22d-268">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="be22d-269">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="be22d-269">Additional resources</span></span>

* [<span data-ttu-id="be22d-270">Cross-Origin Resource Sharing (CORS)</span><span class="sxs-lookup"><span data-stu-id="be22d-270">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
