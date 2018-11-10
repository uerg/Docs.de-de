---
title: Aktivieren Sie Ursprungsübergreifender Anforderungen (CORS) in ASP.NET Core
author: rick-anderson
description: Erfahren Sie, wie CORS als Standard zum Zulassen oder ablehnen von ursprungsübergreifenden Anforderungen in einer ASP.NET Core-app.
ms.author: riande
ms.custom: mvc
ms.date: 11/05/2018
uid: security/cors
ms.openlocfilehash: 8e5056b448d47d75272e9394b03ce8a58b05a0f4
ms.sourcegitcommit: 09affee3d234cb27ea6fe33bc113b79e68900d22
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2018
ms.locfileid: "51191320"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="7b6c9-103">Aktivieren Sie Ursprungsübergreifender Anforderungen (CORS) in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7b6c9-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="7b6c9-104">Durch [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), und [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="7b6c9-104">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="7b6c9-105">Browsersicherheit verhindert, dass eine Webseite auf Anfragen zu einer anderen Domäne als der, die die Webseite verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-105">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="7b6c9-106">Diese Einschränkung wird aufgerufen, die *Richtlinie desselben Ursprungs*.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-106">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="7b6c9-107">Die Richtlinie desselben Ursprungs wird verhindert, dass eine schädliche Website sensible Daten von einem anderen Standort lesen.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-107">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="7b6c9-108">In einigen Fällen empfiehlt es sich um zuzulassen, dass andere Standorte Cross-Origin-Anforderungen zu Ihrer app durchführen.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-108">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span>

<span data-ttu-id="7b6c9-109">[Cross-Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS) ist ein W3C-Standard, der einem Server zu lockern die Richtlinie des gleichen Ursprungs ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-109">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="7b6c9-110">Mit CORS kann ein Server explizit einige ursprungsübergreifende Anforderungen zulassen und andere ablehnen.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-110">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="7b6c9-111">CORS ist sicherer und flexibler als frühere Techniken wie z. B. [JSONP](https://wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="7b6c9-111">CORS is safer and more flexible than earlier techniques, such as [JSONP](https://wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="7b6c9-112">In diesem Thema veranschaulicht das Aktivieren von CORS in einer ASP.NET Core-app.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-112">This topic shows how to enable CORS in an ASP.NET Core app.</span></span>

## <a name="same-origin"></a><span data-ttu-id="7b6c9-113">Denselben Ursprung</span><span class="sxs-lookup"><span data-stu-id="7b6c9-113">Same origin</span></span>

<span data-ttu-id="7b6c9-114">Zwei URLs haben denselben Ursprung ggf. identische Schemas, Hosts und -Ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="7b6c9-114">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="7b6c9-115">Diese zwei URLs haben denselben Ursprung an:</span><span class="sxs-lookup"><span data-stu-id="7b6c9-115">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="7b6c9-116">Diese URLs haben verschiedene Ursprünge als die vorherigen beiden URLs:</span><span class="sxs-lookup"><span data-stu-id="7b6c9-116">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="7b6c9-117">`https://example.net` &ndash; Andere Domäne</span><span class="sxs-lookup"><span data-stu-id="7b6c9-117">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="7b6c9-118">`https://www.example.com/foo.html` &ndash; Andere Unterdomäne</span><span class="sxs-lookup"><span data-stu-id="7b6c9-118">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="7b6c9-119">`http://example.com/foo.html` &ndash; Anderes Schema</span><span class="sxs-lookup"><span data-stu-id="7b6c9-119">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="7b6c9-120">`https://example.com:9000/foo.html` &ndash; Portnummer</span><span class="sxs-lookup"><span data-stu-id="7b6c9-120">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

> [!NOTE]
> <span data-ttu-id="7b6c9-121">Internet Explorer berücksichtigen nicht den Port Ursprünge zu vergleichen.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-121">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="register-cors-services"></a><span data-ttu-id="7b6c9-122">CORS-Dienste registrieren</span><span class="sxs-lookup"><span data-stu-id="7b6c9-122">Register CORS services</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="7b6c9-123">Referenz der [Microsoft.AspNetCore.App metapaket](xref:fundamentals/metapackage-app) oder fügen Sie einen Paketverweis auf die [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) Paket.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-123">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="7b6c9-124">Referenz der [metapaket "Microsoft.aspnetcore.All"](xref:fundamentals/metapackage) oder fügen Sie einen Paketverweis auf die [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) Paket.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-124">Reference the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) or add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="7b6c9-125">Fügen Sie einen Paketverweis auf die [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) Paket.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-125">Add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

<span data-ttu-id="7b6c9-126">Rufen Sie <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> in `Startup.ConfigureServices` CORS-Dienste in der app Service-Container hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="7b6c9-126">Call <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> in `Startup.ConfigureServices` to add CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors&highlight=3)]

## <a name="enable-cors"></a><span data-ttu-id="7b6c9-127">Aktivieren von CORS</span><span class="sxs-lookup"><span data-stu-id="7b6c9-127">Enable CORS</span></span>

<span data-ttu-id="7b6c9-128">Verwenden Sie nach der Registrierung CORS-Dienste einen der folgenden Ansätze zum Aktivieren von CORS in einer ASP.NET Core-app aus:</span><span class="sxs-lookup"><span data-stu-id="7b6c9-128">After registering CORS services, use either of the following approaches to enable CORS in an ASP.NET Core app:</span></span>

* <span data-ttu-id="7b6c9-129">[CORS-Middleware](#enable-cors-with-cors-middleware) &ndash; Richtlinien für CORS gelten global für die app über Middleware.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-129">[CORS Middleware](#enable-cors-with-cors-middleware) &ndash; Apply CORS policies globally to the app via middleware.</span></span>
* <span data-ttu-id="7b6c9-130">[Von CORS in MVC](#enable-cors-in-mvc) &ndash; CORS gelten Richtlinien pro Aktion oder pro Controller.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-130">[CORS in MVC](#enable-cors-in-mvc) &ndash; Apply CORS policies per action or per controller.</span></span> <span data-ttu-id="7b6c9-131">CORS-Middleware wird nicht verwendet.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-131">CORS Middleware isn't used.</span></span>

### <a name="enable-cors-with-cors-middleware"></a><span data-ttu-id="7b6c9-132">Aktivieren von CORS mit CORS-Middleware</span><span class="sxs-lookup"><span data-stu-id="7b6c9-132">Enable CORS with CORS Middleware</span></span>

<span data-ttu-id="7b6c9-133">CORS-Middleware verarbeitet Cross-Origin-Anforderungen an die app.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-133">CORS Middleware handles cross-origin requests to the app.</span></span> <span data-ttu-id="7b6c9-134">Rufen Sie zum Aktivieren von CORS-Middleware in der Anforderungsverarbeitungspipeline der <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> Erweiterungsmethode in `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-134">To enable CORS Middleware in the request processing pipeline, call the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method in `Startup.Configure`.</span></span>

<span data-ttu-id="7b6c9-135">CORS-Middleware muss vor stehen Endpunkte definiert in Ihrer app, wo Sie möchten für die Unterstützung von Cross-Origin-Anfragen (z. B. vor dem Aufruf von `UseMvc` für MVC-Razor-Seiten-Middleware).</span><span class="sxs-lookup"><span data-stu-id="7b6c9-135">CORS Middleware must precede any defined endpoints in your app where you want to support cross-origin requests (for example, before the call to `UseMvc` for MVC/Razor Pages Middleware).</span></span>

<span data-ttu-id="7b6c9-136">Ein *ursprungsübergreifende Richtlinie* kann angegeben werden, beim Hinzufügen der CORS-Middleware mithilfe der <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> Klasse.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-136">A *cross-origin policy* can be specified when adding the CORS Middleware using the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> class.</span></span> <span data-ttu-id="7b6c9-137">Es gibt zwei Ansätze für eine CORS-Richtlinie definieren:</span><span class="sxs-lookup"><span data-stu-id="7b6c9-137">There are two approaches for defining a CORS policy:</span></span>

* <span data-ttu-id="7b6c9-138">Rufen Sie `UseCors` mit einem Lambda-Ausdruck:</span><span class="sxs-lookup"><span data-stu-id="7b6c9-138">Call `UseCors` with a lambda:</span></span>

  [!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

  <span data-ttu-id="7b6c9-139">Das Lambda akzeptiert ein <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> Objekt.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-139">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="7b6c9-140">[Optionen für die Konfiguration](#cors-policy-options), z. B. `WithOrigins`, werden weiter unten in diesem Thema.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-140">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this topic.</span></span> <span data-ttu-id="7b6c9-141">Im vorherigen Beispiel kann die Richtlinie Cross-Origin-Anforderungen von `https://example.com` und keine anderen Quellen.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-141">In the preceding example, the policy allows cross-origin requests from `https://example.com` and no other origins.</span></span>

  <span data-ttu-id="7b6c9-142">Die URL muss angegeben werden, ohne nachgestellten Schrägstrich (`/`).</span><span class="sxs-lookup"><span data-stu-id="7b6c9-142">The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="7b6c9-143">Wenn die URL mit beendet `/`, gibt der Vergleich `false` und es wird kein Header zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-143">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

  <span data-ttu-id="7b6c9-144">`CorsPolicyBuilder` verfügt über eine fluent-API, damit Sie die Methodenaufrufe verketten können:</span><span class="sxs-lookup"><span data-stu-id="7b6c9-144">`CorsPolicyBuilder` has a fluent API, so you can chain method calls:</span></span>

  [!code-csharp[](cors/sample/CorsExample3/Startup.cs?highlight=2-3&range=29-32)]

* <span data-ttu-id="7b6c9-145">Definieren Sie mindestens eine benannte CORS-Richtlinie, und wählen Sie die Richtlinie anhand des Namens zur Laufzeit.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-145">Define one or more named CORS policies and select the policy by name at runtime.</span></span> <span data-ttu-id="7b6c9-146">Im folgenden Beispiel wird eine benutzerdefiniertes CORS-Richtlinie mit dem Namen *AllowSpecificOrigin*.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-146">The following example adds a user-defined CORS policy named *AllowSpecificOrigin*.</span></span> <span data-ttu-id="7b6c9-147">Um die Richtlinie auswählen, übergeben Sie den Namen in `UseCors`:</span><span class="sxs-lookup"><span data-stu-id="7b6c9-147">To select the policy, pass the name to `UseCors`:</span></span>

  [!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin&highlight=5-6,21)]

### <a name="enable-cors-in-mvc"></a><span data-ttu-id="7b6c9-148">Aktivieren von CORS in MVC</span><span class="sxs-lookup"><span data-stu-id="7b6c9-148">Enable CORS in MVC</span></span>

<span data-ttu-id="7b6c9-149">Sie können alternativ MVC verwenden, um Richtlinien für bestimmte CORS pro Aktion oder pro Controller anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-149">You can alternatively use MVC to apply specific CORS policies per action or per controller.</span></span> <span data-ttu-id="7b6c9-150">Wenn Sie MVC verwenden, um CORS zu aktivieren, werden die registrierten CORS-Dienste verwendet.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-150">When using MVC to enable CORS, the registered CORS services are used.</span></span> <span data-ttu-id="7b6c9-151">Die CORS-Middleware wird nicht verwendet.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-151">The CORS Middleware isn't used.</span></span>

### <a name="per-action"></a><span data-ttu-id="7b6c9-152">Pro Aktion</span><span class="sxs-lookup"><span data-stu-id="7b6c9-152">Per action</span></span>

<span data-ttu-id="7b6c9-153">Um eine CORS-Richtlinie für eine bestimmte Aktion anzugeben, fügen die [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) -Attribut auf die Aktion.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-153">To specify a CORS policy for a specific action, add the [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute to the action.</span></span> <span data-ttu-id="7b6c9-154">Geben Sie den Namen der Richtlinie.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-154">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction&highlight=2)]

### <a name="per-controller"></a><span data-ttu-id="7b6c9-155">Pro controller</span><span class="sxs-lookup"><span data-stu-id="7b6c9-155">Per controller</span></span>

<span data-ttu-id="7b6c9-156">Fügen Sie zum Angeben der CORS-Richtlinie für einen bestimmten Controller die [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) -Attribut auf die Controllerklasse.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-156">To specify the CORS policy for a specific controller, add the [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute to the controller class.</span></span> <span data-ttu-id="7b6c9-157">Geben Sie den Namen der Richtlinie.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-157">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController&highlight=2)]

<span data-ttu-id="7b6c9-158">Die Rangfolge ist:</span><span class="sxs-lookup"><span data-stu-id="7b6c9-158">The precedence order is:</span></span>

1. <span data-ttu-id="7b6c9-159">action</span><span class="sxs-lookup"><span data-stu-id="7b6c9-159">action</span></span>
1. <span data-ttu-id="7b6c9-160">Controller</span><span class="sxs-lookup"><span data-stu-id="7b6c9-160">controller</span></span>

### <a name="disable-cors"></a><span data-ttu-id="7b6c9-161">Deaktivieren von CORS</span><span class="sxs-lookup"><span data-stu-id="7b6c9-161">Disable CORS</span></span>

<span data-ttu-id="7b6c9-162">Wenn Sie um CORS für einen Controller oder die Aktion zu deaktivieren, verwenden die [ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) Attribut:</span><span class="sxs-lookup"><span data-stu-id="7b6c9-162">To disable CORS for a controller or action, use the [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute:</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction&highlight=2)]

## <a name="cors-policy-options"></a><span data-ttu-id="7b6c9-163">CORS-Richtlinie-Optionen</span><span class="sxs-lookup"><span data-stu-id="7b6c9-163">CORS policy options</span></span>

<span data-ttu-id="7b6c9-164">Dieser Abschnitt beschreibt die verschiedenen Optionen, die Sie in einer CORS-Richtlinie festlegen können.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-164">This section describes the various options that you can set in a CORS policy.</span></span>

* [<span data-ttu-id="7b6c9-165">Legen Sie die zulässigen Ursprünge</span><span class="sxs-lookup"><span data-stu-id="7b6c9-165">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="7b6c9-166">Legen Sie die zulässigen HTTP-Methoden</span><span class="sxs-lookup"><span data-stu-id="7b6c9-166">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="7b6c9-167">Legt die zulässigen Anforderungsheader fest.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-167">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="7b6c9-168">Legen Sie die verfügbar gemachten Antwortheader</span><span class="sxs-lookup"><span data-stu-id="7b6c9-168">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="7b6c9-169">Anmeldeinformationen im Cross-Origin-Anforderungen</span><span class="sxs-lookup"><span data-stu-id="7b6c9-169">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="7b6c9-170">Die Preflight-Ablaufzeit festlegen</span><span class="sxs-lookup"><span data-stu-id="7b6c9-170">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="7b6c9-171">Für einige Optionen, es kann hilfreich sein, lesen Sie die [funktioniert wie CORS](#how-cors-works) im ersten Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-171">For some options, it may be helpful to read the [How CORS works](#how-cors-works) section first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="7b6c9-172">Legen Sie die zulässigen Ursprünge</span><span class="sxs-lookup"><span data-stu-id="7b6c9-172">Set the allowed origins</span></span>

<span data-ttu-id="7b6c9-173">Die CORS-Middleware in ASP.NET Core MVC verfügt über einige Möglichkeiten zum Angeben der zulässigen Ursprünge:</span><span class="sxs-lookup"><span data-stu-id="7b6c9-173">The CORS middleware in ASP.NET Core MVC has a few ways to specify allowed origins:</span></span>

* <span data-ttu-id="7b6c9-174"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*>: Ermöglicht die Angabe einer oder mehrerer URLs.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-174"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*>: Allows specifying one or more URLs.</span></span> <span data-ttu-id="7b6c9-175">Die URL kann es sich um das Schema, Host-Name und Port ohne Pfadinformationen enthalten.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-175">The URL may include the scheme, host name, and port without any path information.</span></span> <span data-ttu-id="7b6c9-176">Beispielsweise `https://example.com`.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-176">For example, `https://example.com`.</span></span> <span data-ttu-id="7b6c9-177">Die URL muss angegeben werden, ohne nachgestellten Schrägstrich (`/`).</span><span class="sxs-lookup"><span data-stu-id="7b6c9-177">The URL must be specified without a trailing slash (`/`).</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=20-24&highlight=4)]

* <span data-ttu-id="7b6c9-178"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>: Ermöglicht die CORS-Anforderungen aus allen Ursprüngen mit einem beliebigen Schema (`http` oder `https`).</span><span class="sxs-lookup"><span data-stu-id="7b6c9-178"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>: Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=28-32&highlight=4)]

<span data-ttu-id="7b6c9-179">Wägen Sie sorgfältig, bevor Sie die Anforderungen von einem beliebigen Ursprung zulassen.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-179">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="7b6c9-180">Dadurch können Anforderungen von einem beliebigen Ursprung bedeutet, dass *eine beliebige Website* Cross-Origin-Anfragen an Ihre app vornehmen können.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-180">Allowing requests from any origin means that *any website* can make cross-origin requests to your app.</span></span>

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="7b6c9-181">Angeben von `AllowAnyOrigin` und `AllowCredentials` ist eine unsichere Konfiguration und kann dazu führen, siteübergreifende anforderungsfälschung.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-181">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="7b6c9-182">Der CORS-Dienst gibt eine ungültige CORS-Antwort zurück, wenn eine app mit den beiden konfiguriert ist.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-182">The CORS service returns an invalid CORS response when an app is configured with the two.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="7b6c9-183">Angeben von `AllowAnyOrigin` und `AllowCredentials` ist eine unsichere Konfiguration und kann dazu führen, siteübergreifende anforderungsfälschung.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-183">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="7b6c9-184">Geben Sie ggf. eine genaue Liste der Ursprünge, wenn der Client autorisiert werden, auf Serverressourcen zugreifen muss.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-184">Consider specifying an exact list of origins if your client needs to authorize to access server resources.</span></span>

::: moniker-end

<span data-ttu-id="7b6c9-185">Diese Einstellung wirkt sich auf [preflight-Anforderungen und der Access-Control-Allow-Origin-Header](#preflight-requests) (weiter unten in diesem Thema beschrieben).</span><span class="sxs-lookup"><span data-stu-id="7b6c9-185">This setting affects [preflight requests and the Access-Control-Allow-Origin header](#preflight-requests) (described later in this topic).</span></span>

* <span data-ttu-id="7b6c9-186"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> – Ermöglicht das CORS-Anforderungen aus allen untergeordneten Domänen einer Domäne.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-186"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> - Allows CORS requests from any sub-domain of a given domain.</span></span> <span data-ttu-id="7b6c9-187">Das Schema darf nicht mit einem Platzhalter sein.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-187">The scheme cannot be a wildcard.</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=98-104&highlight=4)]

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="7b6c9-188">Legen Sie die zulässigen HTTP-Methoden</span><span class="sxs-lookup"><span data-stu-id="7b6c9-188">Set the allowed HTTP methods</span></span>

<span data-ttu-id="7b6c9-189">Um alle HTTP-Methoden zu ermöglichen, rufen <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="7b6c9-189">To allow all HTTP methods, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=45-50&highlight=5)]

<span data-ttu-id="7b6c9-190">Diese Einstellung wirkt sich auf [preflight-Anforderungen und der Access-Control-Allow-Methods-Header](#preflight-requests) (weiter unten in diesem Thema beschrieben).</span><span class="sxs-lookup"><span data-stu-id="7b6c9-190">This setting affects [preflight requests and the Access-Control-Allow-Methods header](#preflight-requests) (described later in this topic).</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="7b6c9-191">Legt die zulässigen Anforderungsheader fest.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-191">Set the allowed request headers</span></span>

<span data-ttu-id="7b6c9-192">Wird aufgerufen, um bestimmte Header in einer CORS-Anforderung gesendet werden können *erstellen Anforderungsheader*, rufen Sie <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> , und geben Sie die erlaubten Header:</span><span class="sxs-lookup"><span data-stu-id="7b6c9-192">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=54-59&highlight=5)]

<span data-ttu-id="7b6c9-193">Um zuzulassen, die alle erstellen Anforderungsheader, rufen Sie <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="7b6c9-193">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=63-68&highlight=5)]

<span data-ttu-id="7b6c9-194">Diese Einstellung wirkt sich auf [preflight-Anforderungen und der Access-Control-Request-Headers-Header](#preflight-requests) (weiter unten in diesem Thema beschrieben).</span><span class="sxs-lookup"><span data-stu-id="7b6c9-194">This setting affects [preflight requests and the Access-Control-Request-Headers header](#preflight-requests) (described later in this topic).</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="7b6c9-195">Eine Übereinstimmung der CORS-Middleware-Richtlinien auf bestimmte Header gemäß `WithHeaders` ist nur möglich, wenn die Header gesendet wird, `Access-Control-Request-Headers` genau die Header in angegebenen `WithHeaders`.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-195">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="7b6c9-196">Betrachten Sie beispielsweise eine app wie folgt konfiguriert:</span><span class="sxs-lookup"><span data-stu-id="7b6c9-196">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="7b6c9-197">CORS-Middleware eine preflight-Anforderung mit der folgenden Anforderungsheader ablehnt, da `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) ist nicht aufgeführt, `WithHeaders`:</span><span class="sxs-lookup"><span data-stu-id="7b6c9-197">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="7b6c9-198">Die app-gibt ein *200 OK* Antwort jedoch nicht wieder die CORS-Header sendet.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-198">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="7b6c9-199">Aus diesem Grund versucht der Browser nicht die cors-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-199">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="7b6c9-200">CORS-Middleware können immer vier Header in der `Access-Control-Request-Headers` unabhängig von den Werten in CorsPolicy.Headers konfiguriert gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-200">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="7b6c9-201">Diese Liste der Header enthält:</span><span class="sxs-lookup"><span data-stu-id="7b6c9-201">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="7b6c9-202">Betrachten Sie beispielsweise eine app wie folgt konfiguriert:</span><span class="sxs-lookup"><span data-stu-id="7b6c9-202">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="7b6c9-203">CORS-Middleware erfolgreich beantwortet eine preflight-Anforderung mit der folgenden Anforderungsheader da `Content-Language` ist immer in der Whitelist enthalten:</span><span class="sxs-lookup"><span data-stu-id="7b6c9-203">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="7b6c9-204">Legen Sie die verfügbar gemachten Antwortheader</span><span class="sxs-lookup"><span data-stu-id="7b6c9-204">Set the exposed response headers</span></span>

<span data-ttu-id="7b6c9-205">Standardmäßig nicht im Browser alle Antwortheader für die app verfügbar machen.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-205">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="7b6c9-206">Weitere Informationen finden Sie unter [W3C Cross-Origin Resource Sharing (Terminologie): einfache Antwortheader](https://www.w3.org/TR/cors/#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="7b6c9-206">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="7b6c9-207">Die Antwortheader, die standardmäßig verfügbar sind:</span><span class="sxs-lookup"><span data-stu-id="7b6c9-207">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="7b6c9-208">CORS-Spezifikation ruft diese Header *Header für die einfache Antwort*.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-208">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="7b6c9-209">Um weitere Header für die app verfügbar zu machen, rufen Sie <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="7b6c9-209">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=72-77&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="7b6c9-210">Anmeldeinformationen im Cross-Origin-Anforderungen</span><span class="sxs-lookup"><span data-stu-id="7b6c9-210">Credentials in cross-origin requests</span></span>

<span data-ttu-id="7b6c9-211">Anmeldeinformationen erfordern besondere Behandlung in eine CORS-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-211">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="7b6c9-212">Standardmäßig sendet der Browser keine Anmeldeinformationen mit einer cors-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-212">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="7b6c9-213">Anmeldeinformationen enthalten, Cookies und HTTP-Authentifizierungsschemas.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-213">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="7b6c9-214">Um die Anmeldeinformationen mit einer cors-Anforderung zu senden, muss der Client festgelegt `XMLHttpRequest.withCredentials` zu `true`.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-214">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="7b6c9-215">Mithilfe von `XMLHttpRequest` direkt:</span><span class="sxs-lookup"><span data-stu-id="7b6c9-215">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="7b6c9-216">In jQuery:</span><span class="sxs-lookup"><span data-stu-id="7b6c9-216">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'https://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="7b6c9-217">Darüber hinaus muss der Server die Anmeldeinformationen zulassen.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-217">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="7b6c9-218">Um ursprungsübergreifende Anmeldeinformationen ermöglichen möchten, rufen <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span><span class="sxs-lookup"><span data-stu-id="7b6c9-218">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=81-86&highlight=5)]

<span data-ttu-id="7b6c9-219">Die HTTP-Antwort enthält ein `Access-Control-Allow-Credentials` -Header, der dem Browser darüber informiert, dass der Server die Anmeldeinformationen für eine Anforderung zwischen verschiedenen Ursprüngen zulässt.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-219">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="7b6c9-220">Wenn der Browser sendet die Anmeldeinformationen, aber die Antwort auf eine gültige beinhaltet keine `Access-Control-Allow-Credentials` -Header, der Browser nicht die Antwort auf die app verfügbar zu machen, und die cors-Anforderung schlägt fehl.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-220">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="7b6c9-221">Seien Sie vorsichtig beim Cross-Origin-Anmeldeinformationen zulassen.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-221">Be careful when allowing cross-origin credentials.</span></span> <span data-ttu-id="7b6c9-222">Eine Website in einer anderen Domäne kann die Anmeldeinformationen eines angemeldeten Benutzers an die app auf den Namen des Benutzers, ohne das Wissen des Benutzers senden.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-222">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span>

<span data-ttu-id="7b6c9-223">CORS-Spezifikation gibt auch an dieser Einstellung Ursprüngen `"*"` (alle Ursprünge) ist ungültig. wenn die `Access-Control-Allow-Credentials` Header vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-223">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="7b6c9-224">Preflight-Anforderungen</span><span class="sxs-lookup"><span data-stu-id="7b6c9-224">Preflight requests</span></span>

<span data-ttu-id="7b6c9-225">Für einige CORS-Anforderungen sendet der Browser eine zusätzliche Anforderung vor der tatsächlichen Anforderung an.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-225">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="7b6c9-226">Diese Anforderung wird aufgerufen, eine *preflight-Anforderung*.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-226">This request is called a *preflight request*.</span></span> <span data-ttu-id="7b6c9-227">Der Browser kann die preflight-Anforderung überspringen, wenn die folgenden Bedingungen erfüllt sind:</span><span class="sxs-lookup"><span data-stu-id="7b6c9-227">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="7b6c9-228">Die Anforderungsmethode ist GET, HEAD oder POST.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-228">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="7b6c9-229">Die app keine Anforderungsheader außer festlegen `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, oder `Last-Event-ID`.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-229">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="7b6c9-230">Die `Content-Type` -Header, wenn festgelegt ist, verfügt über mindestens einen der folgenden Werte:</span><span class="sxs-lookup"><span data-stu-id="7b6c9-230">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="7b6c9-231">Die Regel auf die Anforderungsheader festzulegen, für die Header die Anforderung des Clients gilt, die die app durch den Aufruf festlegt `setRequestHeader` auf die `XMLHttpRequest` Objekt.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-231">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="7b6c9-232">CORS-Spezifikation ruft diese Header *erstellen Anforderungsheader*.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-232">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="7b6c9-233">Die Regel trifft nicht auf der Browser, wie z. B. festlegen kann Header `User-Agent`, `Host`, oder `Content-Length`.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-233">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="7b6c9-234">Im folgenden finden ein Beispiel für eine preflight-Anforderung:</span><span class="sxs-lookup"><span data-stu-id="7b6c9-234">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="7b6c9-235">Die Pre-Flight-Anforderung verwendet die HTTP OPTIONS-Methode.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-235">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="7b6c9-236">Es enthält zwei spezielle Header:</span><span class="sxs-lookup"><span data-stu-id="7b6c9-236">It includes two special headers:</span></span>

* <span data-ttu-id="7b6c9-237">`Access-Control-Request-Method`: Die HTTP-Methode, die für die tatsächliche Anforderung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-237">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="7b6c9-238">`Access-Control-Request-Headers`: Eine Liste der Anforderungsheader, die die app für die tatsächliche Anforderung festlegt.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-238">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="7b6c9-239">Wie bereits erwähnt, Dies beinhaltet keine Header, die der Browser legt, wie z. B. fest `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-239">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="7b6c9-240">Eine CORS-preflight-Anforderung sind zum Beispiel eine `Access-Control-Request-Headers` -Header, der auf dem Server die Header gibt an, die mit der tatsächlichen Anforderung gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-240">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="7b6c9-241">Um bestimmte Header zu ermöglichen, rufen <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="7b6c9-241">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=54-59&highlight=5)]

<span data-ttu-id="7b6c9-242">Um zuzulassen, die alle erstellen Anforderungsheader, rufen Sie <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="7b6c9-242">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=63-68&highlight=5)]

<span data-ttu-id="7b6c9-243">Browser sind nicht vollständig konsistent in diese Festlegung `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-243">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="7b6c9-244">Wenn Sie Header nicht festgelegt `"*"` (oder verwenden Sie <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), Sie sollten mindestens einschließen `Accept`, `Content-Type`, und `Origin`, sowie alle benutzerdefinierten Header, die Sie unterstützen möchten.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-244">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="7b6c9-245">Im folgenden finden eine Beispielantwort auf die preflight-Anforderung (vorausgesetzt, dass der Server die Anforderung zulässt):</span><span class="sxs-lookup"><span data-stu-id="7b6c9-245">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="7b6c9-246">Die Antwort enthält ein `Access-Control-Allow-Methods` Header, der die zulässigen Methoden enthält und optional eine `Access-Control-Allow-Headers` -Header, der die erlaubten Header aufgeführt sind.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-246">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="7b6c9-247">Wenn die preflight-Anforderung erfolgreich ist, sendet der Browser die tatsächliche Anforderung an.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-247">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="7b6c9-248">Wenn die preflight-Anforderung abgelehnt wird, gibt die app eine *200 OK* Antwort jedoch nicht wieder die CORS-Header sendet.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-248">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="7b6c9-249">Aus diesem Grund versucht der Browser nicht die cors-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-249">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="7b6c9-250">Die Preflight-Ablaufzeit festlegen</span><span class="sxs-lookup"><span data-stu-id="7b6c9-250">Set the preflight expiration time</span></span>

<span data-ttu-id="7b6c9-251">Die `Access-Control-Max-Age` Header gibt an, wie lange die Antwort auf die preflight-Anforderung zwischengespeichert werden kann.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-251">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="7b6c9-252">Rufen Sie zum Festlegen dieser Header <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span><span class="sxs-lookup"><span data-stu-id="7b6c9-252">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=90-95&highlight=5)]

## <a name="how-cors-works"></a><span data-ttu-id="7b6c9-253">Funktionsweise von CORS</span><span class="sxs-lookup"><span data-stu-id="7b6c9-253">How CORS works</span></span>

<span data-ttu-id="7b6c9-254">In diesem Abschnitt wird beschrieben, was geschieht, in einer CORS-Anforderung auf der Ebene der HTTP-Nachrichten.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-254">This section describes what happens in a CORS request at the level of the HTTP messages.</span></span> <span data-ttu-id="7b6c9-255">Es ist wichtig zum Verständnis der Funktionsweise von CORS, damit die CORS-Richtlinie ordnungsgemäß konfiguriert und debuggt werden, wenn die zu unerwarteten Ergebnissen führen kann.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-255">It's important to understand how CORS works so that the CORS policy can be configured correctly and debugged when unexpected behaviors occur.</span></span>

<span data-ttu-id="7b6c9-256">CORS-Spezifikation führt mehrere neue HTTP-Header, die Cross-Origin-Anforderungen zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-256">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="7b6c9-257">Wenn ein Browser CORS unterstützt, wird diese Header automatisch für Cross-Origin-Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-257">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="7b6c9-258">Benutzerdefinierte JavaScript-Code ist nicht erforderlich, um CORS zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-258">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="7b6c9-259">Folgendes ist ein Beispiel für eine cors-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-259">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="7b6c9-260">Die `Origin` Header enthält, die Domäne der Website, die die Anforderung gesendet wird:</span><span class="sxs-lookup"><span data-stu-id="7b6c9-260">The `Origin` header provides the domain of the site that's making the request:</span></span>

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

<span data-ttu-id="7b6c9-261">Wenn der Server die Anforderung zulässt, wird die `Access-Control-Allow-Origin` Header in der Antwort.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-261">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="7b6c9-262">Entweder mit dem Wert dieses Headers übereinstimmt der `Origin` Header aus der Anforderung oder der Platzhalterwert `"*"`, Bedeutung, die einen beliebigen Ursprung zulässig ist:</span><span class="sxs-lookup"><span data-stu-id="7b6c9-262">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="7b6c9-263">Wenn die Antwort enthalten nicht die `Access-Control-Allow-Origin` -Header, die Cross-Origin-Anforderung schlägt fehl.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-263">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="7b6c9-264">Der Browser lässt, die die Anforderung.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-264">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="7b6c9-265">Selbst wenn der Server eine erfolgreiche Antwort gibt zurück, machen nicht im Browser die Antwort für die Client-app verfügbar.</span><span class="sxs-lookup"><span data-stu-id="7b6c9-265">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7b6c9-266">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="7b6c9-266">Additional resources</span></span>

* [<span data-ttu-id="7b6c9-267">Cross-Origin Resource Sharing (CORS)</span><span class="sxs-lookup"><span data-stu-id="7b6c9-267">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
