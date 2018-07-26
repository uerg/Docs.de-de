---
title: Erzwingen von HTTPS in ASP.NET Core
author: rick-anderson
description: Zeigt die Vorgehensweise zum Erzwingen einer HTTPS/TLS, in einer ASP.NET Core-Web-app.
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: c3d92994c0331b1408e246953454910ca1f4dc43
ms.sourcegitcommit: c8e62aa766641aa55105f7db79cdf2b27a6e5977
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2018
ms.locfileid: "39254830"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="0d27d-103">Erzwingen von HTTPS in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0d27d-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="0d27d-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0d27d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0d27d-105">Dieses Dokument zeigt, wie Sie:</span><span class="sxs-lookup"><span data-stu-id="0d27d-105">This document shows how to:</span></span>

* <span data-ttu-id="0d27d-106">HTTPS für alle Anforderungen erforderlich.</span><span class="sxs-lookup"><span data-stu-id="0d27d-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="0d27d-107">Alle HTTP-Anforderungen auf HTTPS umleiten.</span><span class="sxs-lookup"><span data-stu-id="0d27d-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="0d27d-108">Führen Sie **nicht** verwenden [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) für Web-APIs, die vertraulichen Informationen zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="0d27d-108">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="0d27d-109">`RequireHttpsAttribute` leitet Browsers per HTTP-Statuscode von HTTP an HTTPS weiter.</span><span class="sxs-lookup"><span data-stu-id="0d27d-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="0d27d-110">API-Clients verstehen diese Codes möglicherweise nicht, oder Sie führen keine Weiterleitung von HTTP an HTTPS durch.</span><span class="sxs-lookup"><span data-stu-id="0d27d-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="0d27d-111">Dies kann dazu führen, dass solche Clients Daten unverschlüsselt mittels HTTP versenden.</span><span class="sxs-lookup"><span data-stu-id="0d27d-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="0d27d-112">Web-APIs sollten daher entweder:</span><span class="sxs-lookup"><span data-stu-id="0d27d-112">Web APIs should either:</span></span>
>
> * <span data-ttu-id="0d27d-113">nicht auf HTTP lauschen oder</span><span class="sxs-lookup"><span data-stu-id="0d27d-113">Not listen on HTTP.</span></span>
> * <span data-ttu-id="0d27d-114">die Verbindung mit dem Statuscode 400 („Ungültige Anforderung“) schließen und die Anforderung nicht verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="0d27d-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="0d27d-115">Anforderung von HTTPS</span><span class="sxs-lookup"><span data-stu-id="0d27d-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="0d27d-116">Es wird empfohlen, alle ASP.NET Core-Web-apps rufen die HTTPS-Umleitung-Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) auf alle HTTP-Anfragen an HTTPS umzuleiten.</span><span class="sxs-lookup"><span data-stu-id="0d27d-116">We recommend all ASP.NET Core web apps call HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="0d27d-117">Der folgende code ruft `UseHttpsRedirection` in die `Startup` Klasse:</span><span class="sxs-lookup"><span data-stu-id="0d27d-117">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="0d27d-118">Der oben markierte Code:</span><span class="sxs-lookup"><span data-stu-id="0d27d-118">The preceding highlighted code:</span></span>

* <span data-ttu-id="0d27d-119">Verwendet die standardmäßige [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span><span class="sxs-lookup"><span data-stu-id="0d27d-119">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span></span> <span data-ttu-id="0d27d-120">Produktions-apps sollten Aufrufen [UseHsts](#hsts).</span><span class="sxs-lookup"><span data-stu-id="0d27d-120">Production apps should call [UseHsts](#hsts).</span></span>
* <span data-ttu-id="0d27d-121">Verwendet die standardmäßige [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (443).</span><span class="sxs-lookup"><span data-stu-id="0d27d-121">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (443).</span></span>

<span data-ttu-id="0d27d-122">Der folgende code ruft [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) middlewareoptionen konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="0d27d-122">The following code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="0d27d-123">Der oben markierte Code:</span><span class="sxs-lookup"><span data-stu-id="0d27d-123">The preceding highlighted code:</span></span>

* <span data-ttu-id="0d27d-124">Legt [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) zu `Status307TemporaryRedirect`, dies ist der Standardwert.</span><span class="sxs-lookup"><span data-stu-id="0d27d-124">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to `Status307TemporaryRedirect`, which is the default value.</span></span> <span data-ttu-id="0d27d-125">Produktions-apps sollten Aufrufen [UseHsts](#hsts).</span><span class="sxs-lookup"><span data-stu-id="0d27d-125">Production apps should call [UseHsts](#hsts).</span></span>
* <span data-ttu-id="0d27d-126">Legt den HTTPS-Port in 5001 fest.</span><span class="sxs-lookup"><span data-stu-id="0d27d-126">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="0d27d-127">Der Standardwert ist 443.</span><span class="sxs-lookup"><span data-stu-id="0d27d-127">The default value is 443.</span></span>

<span data-ttu-id="0d27d-128">Die folgenden Mechanismen legen Sie den Port automatisch:</span><span class="sxs-lookup"><span data-stu-id="0d27d-128">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="0d27d-129">Die Middleware kann ermitteln, die Ports über [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) wenn Folgendes zutrifft:</span><span class="sxs-lookup"><span data-stu-id="0d27d-129">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>
  - <span data-ttu-id="0d27d-130">Kestrel oder HTTP.sys direkt mit HTTPS-Endpunkte verwendet wird (gilt auch für die app mit Visual Studio Code-Debugger ausgeführt wird).</span><span class="sxs-lookup"><span data-stu-id="0d27d-130">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
  - <span data-ttu-id="0d27d-131">Nur **eine HTTPS-Port** wird von der app verwendet.</span><span class="sxs-lookup"><span data-stu-id="0d27d-131">Only **one HTTPS port** is used by the app.</span></span>
* <span data-ttu-id="0d27d-132">Visual Studio wird verwendet:</span><span class="sxs-lookup"><span data-stu-id="0d27d-132">Visual Studio is used:</span></span>
  - <span data-ttu-id="0d27d-133">IIS Express ist HTTPS-tauglich.</span><span class="sxs-lookup"><span data-stu-id="0d27d-133">IIS Express has HTTPS enabled.</span></span>
  - <span data-ttu-id="0d27d-134">*"launchsettings.JSON"* legt die `sslPort` für IIS Express.</span><span class="sxs-lookup"><span data-stu-id="0d27d-134">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="0d27d-135">Wenn eine app ausgeführt wird, hinter einem Reverseproxy (z. B. IIS, IIS Express) `IServerAddressesFeature` ist nicht verfügbar.</span><span class="sxs-lookup"><span data-stu-id="0d27d-135">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="0d27d-136">Der Port muss manuell konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="0d27d-136">The port must be manually configured.</span></span> <span data-ttu-id="0d27d-137">Wenn der Port nicht festgelegt ist, werden nicht die Anforderungen umgeleitet.</span><span class="sxs-lookup"><span data-stu-id="0d27d-137">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="0d27d-138">Der Port kann konfiguriert werden, durch Festlegen der:</span><span class="sxs-lookup"><span data-stu-id="0d27d-138">The port can be configured by setting the:</span></span>

* <span data-ttu-id="0d27d-139">Die Umgebungsvariable `ASPNETCORE_HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="0d27d-139">`ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
* <span data-ttu-id="0d27d-140">`http_port` Host-Konfigurationsschlüssel (z. B. über *hostsettings.json* oder ein Befehlszeilenargument).</span><span class="sxs-lookup"><span data-stu-id="0d27d-140">`http_port` host configuration key (for example, via *hostsettings.json* or a command line argument).</span></span>
* <span data-ttu-id="0d27d-141">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span><span class="sxs-lookup"><span data-stu-id="0d27d-141">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span></span> <span data-ttu-id="0d27d-142">Finden Sie im vorherige Beispiel, das zeigt, wie Sie den Port in 5001 festgelegt.</span><span class="sxs-lookup"><span data-stu-id="0d27d-142">See the preceding example that shows how to set the port to 5001.</span></span>

> [!NOTE]
> <span data-ttu-id="0d27d-143">Der Port kann indirekt konfiguriert werden, durch Festlegen der URL durch die `ASPNETCORE_URLS` -Umgebungsvariablen angegeben.</span><span class="sxs-lookup"><span data-stu-id="0d27d-143">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="0d27d-144">Die Umgebungsvariable konfiguriert den Server, und klicken Sie dann die Middleware indirekt über den HTTPS-Port ermittelt `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="0d27d-144">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="0d27d-145">Wenn kein Port festgelegt ist:</span><span class="sxs-lookup"><span data-stu-id="0d27d-145">If no port is set:</span></span>

* <span data-ttu-id="0d27d-146">Anforderungen werden nicht umgeleitet.</span><span class="sxs-lookup"><span data-stu-id="0d27d-146">Requests aren't redirected.</span></span>
* <span data-ttu-id="0d27d-147">Die Middleware protokolliert eine Warnung.</span><span class="sxs-lookup"><span data-stu-id="0d27d-147">The middleware logs a warning.</span></span>

> [!NOTE]
> <span data-ttu-id="0d27d-148">Eine Alternative zur Verwendung von HTTPS-Umleitung-Middleware (`UseHttpsRedirection`) ist die Verwendung von URL-Umschreibenden Middleware (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="0d27d-148">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="0d27d-149">`AddRedirectToHttps` können den Statuscode und den Port auch festlegen, wenn die Umleitung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="0d27d-149">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="0d27d-150">Weitere Informationen finden Sie unter [URL-Umschreibenden Middleware](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="0d27d-150">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="0d27d-151">Ohne die Notwendigkeit von zusätzlichen umleitungs-Regeln an HTTPS umleiten möchten, empfehlen wir Ihnen unter Verwendung von HTTPS-Umleitung-Middleware (`UseHttpsRedirection`) in diesem Thema beschrieben.</span><span class="sxs-lookup"><span data-stu-id="0d27d-151">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="0d27d-152">Die [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) wird verwendet, um die HTTPS erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="0d27d-152">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="0d27d-153">`[RequireHttpsAttribute]` ergänzen können, Controllern oder Methoden oder global angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="0d27d-153">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="0d27d-154">Um das Attribut global anzuwenden, fügen Sie den folgenden Code zur `ConfigureServices` in `Startup`:</span><span class="sxs-lookup"><span data-stu-id="0d27d-154">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="0d27d-155">Der oben markierte Code ist erforderlich, alle Anforderungen verwenden `HTTPS`daher HTTP-Anforderungen werden ignoriert.</span><span class="sxs-lookup"><span data-stu-id="0d27d-155">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="0d27d-156">Der folgende hervorgehobene Code leitet alle HTTP-Anfragen an HTTPS um:</span><span class="sxs-lookup"><span data-stu-id="0d27d-156">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="0d27d-157">Weitere Informationen finden Sie unter [URL-Umschreibenden Middleware](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="0d27d-157">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="0d27d-158">Die Middleware kann außerdem die app, die den Statuscode oder den Statuscode sowie den Port festgelegt, wenn die Umleitung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="0d27d-158">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="0d27d-159">Das globale Erzwingen der Verwendung von HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) ist eine bewährte Sicherheitsmethode.</span><span class="sxs-lookup"><span data-stu-id="0d27d-159">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="0d27d-160">Dieser Ansatz gilt im Vergleich zur Anwendung des `[RequireHttps]` -Attributs auf alle Controller und Razor Pages als sicherer.</span><span class="sxs-lookup"><span data-stu-id="0d27d-160">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="0d27d-161">denn Sie können nicht gewährleisten, dass das `[RequireHttps]` -Attribut angewendet wird, wenn neue Controller oder Razor Pages hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="0d27d-161">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="0d27d-162">HTTP Strict Transport Security-Protokoll (HSTS)</span><span class="sxs-lookup"><span data-stu-id="0d27d-162">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="0d27d-163">Pro [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) ist eine optionale sicherheitserweiterung, die von einer Webanwendung mithilfe eines speziellen Antwortheaders angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="0d27d-163">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="0d27d-164">Sobald ein unterstützter Browser diesen Header empfängt, diesen Browser wird verhindert, dass Kommunikation über HTTP gesendet werden, die der angegebenen Domäne und sendet die gesamte Kommunikation stattdessen über HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0d27d-164">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="0d27d-165">Es wird verhindert, dass HTTPS clickthrough-aufforderungen von Browsern.</span><span class="sxs-lookup"><span data-stu-id="0d27d-165">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="0d27d-166">ASP.NET Core 2.1 oder höher implementiert HSTS mit der `UseHsts` -Erweiterungsmethode.</span><span class="sxs-lookup"><span data-stu-id="0d27d-166">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="0d27d-167">Der folgende code ruft `UseHsts` bei der app nicht im [Entwicklungsmodus](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="0d27d-167">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="0d27d-168">`UseHsts` ist nicht in der Entwicklung empfohlen, da der Header des HSTS hohem Maße zwischenspeicherbar ist von Browsern.</span><span class="sxs-lookup"><span data-stu-id="0d27d-168">`UseHsts` isn't recommended in development because the HSTS header is highly cacheable by browsers.</span></span> <span data-ttu-id="0d27d-169">In der Standardeinstellung `UseHsts` schließt die lokalen Loopback-Adresse.</span><span class="sxs-lookup"><span data-stu-id="0d27d-169">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="0d27d-170">Der folgende Code</span><span class="sxs-lookup"><span data-stu-id="0d27d-170">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="0d27d-171">Legt den preload Parameter des Strict-Transport-Security-Headers.</span><span class="sxs-lookup"><span data-stu-id="0d27d-171">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="0d27d-172">Preload ist nicht Teil der [RFC HSTS Spezifikation](https://tools.ietf.org/html/rfc6797), aber vom Webbrowser zum Vorabladen von HSTS Websites Neuinstallation unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="0d27d-172">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="0d27d-173">Weitere Informationen finden Sie unter [https://hstspreload.org/](https://hstspreload.org/).</span><span class="sxs-lookup"><span data-stu-id="0d27d-173">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="0d27d-174">Ermöglicht [IncludeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), denen Unterdomänen der Host die HSTS-Richtlinie gilt.</span><span class="sxs-lookup"><span data-stu-id="0d27d-174">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="0d27d-175">Explizit festlegt den Max-Age-Parameter des Strict-Transport-Security-Headers auf 60 Tage.</span><span class="sxs-lookup"><span data-stu-id="0d27d-175">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="0d27d-176">Wenn nicht festgelegt, der Standardwert ist 30 Tage.</span><span class="sxs-lookup"><span data-stu-id="0d27d-176">If not set, defaults to 30 days.</span></span> <span data-ttu-id="0d27d-177">Finden Sie unter den [Max-Age-Direktive](https://tools.ietf.org/html/rfc6797#section-6.1.1) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="0d27d-177">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="0d27d-178">Fügt `example.com` zur Liste der Hosts zu schließen.</span><span class="sxs-lookup"><span data-stu-id="0d27d-178">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="0d27d-179">`UseHsts` Schließt die folgenden Loopback-Hosts an:</span><span class="sxs-lookup"><span data-stu-id="0d27d-179">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="0d27d-180">`localhost` : Die IPv4-Loopback-Adresse.</span><span class="sxs-lookup"><span data-stu-id="0d27d-180">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="0d27d-181">`127.0.0.1` : Die IPv4-Loopback-Adresse.</span><span class="sxs-lookup"><span data-stu-id="0d27d-181">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="0d27d-182">`[::1]` : Die IPv6-Loopback-Adresse.</span><span class="sxs-lookup"><span data-stu-id="0d27d-182">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="0d27d-183">Im vorherige Beispiel veranschaulicht das weitere Hosts hinzufügen möchten.</span><span class="sxs-lookup"><span data-stu-id="0d27d-183">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="0d27d-184">Deaktivieren von HTTPS bei projekterstellung</span><span class="sxs-lookup"><span data-stu-id="0d27d-184">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="0d27d-185">Aktivieren Sie die ASP.NET Core 2.1 oder höher Webanwendungsvorlagen (von Visual Studio oder der Dotnet-Befehlszeile) [HTTPS-Umleitung](#require) und [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="0d27d-185">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="0d27d-186">Für Bereitstellungen, die keine HTTPS erforderlich ist, Sie können HTTPS deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="0d27d-186">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="0d27d-187">Z. B. einige Back-End-Dienste, in denen HTTPS extern am Netzwerkrand behandelt wird über HTTPS an jedem Knoten, ist nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="0d27d-187">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="0d27d-188">Zum Deaktivieren von HTTPS:</span><span class="sxs-lookup"><span data-stu-id="0d27d-188">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0d27d-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0d27d-189">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="0d27d-190">Deaktivieren Sie die **für HTTPS konfigurieren** Kontrollkästchen.</span><span class="sxs-lookup"><span data-stu-id="0d27d-190">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Entitätsdiagramm](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0d27d-192">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="0d27d-192">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="0d27d-193">Verwenden Sie die `--no-https`-Option.</span><span class="sxs-lookup"><span data-stu-id="0d27d-193">Use the `--no-https` option.</span></span> <span data-ttu-id="0d27d-194">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0d27d-194">For example</span></span>

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a><span data-ttu-id="0d27d-195">Zum Einrichten von einem entwicklerzertifikat für Docker</span><span class="sxs-lookup"><span data-stu-id="0d27d-195">How to setup a developer certificate for Docker</span></span>

<span data-ttu-id="0d27d-196">Finden Sie unter [GitHub-Problem](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="0d27d-196">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end
