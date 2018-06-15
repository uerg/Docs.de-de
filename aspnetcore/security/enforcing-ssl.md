---
title: Erzwingen von HTTPS in ASP.NET Core
author: rick-anderson
description: Veranschaulicht, wie HTTPS/TLS in einer ASP.NET Core erfordern Web-app.
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 48a25b7ba7affe84cfa6fe16096409239c510221
ms.sourcegitcommit: 40b102ecf88e53d9d872603ce6f3f7044bca95ce
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/15/2018
ms.locfileid: "35652187"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="334c9-103">Erzwingen von HTTPS in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="334c9-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="334c9-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="334c9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="334c9-105">Dieses Dokument zeigt, wie Sie:</span><span class="sxs-lookup"><span data-stu-id="334c9-105">This document shows how to:</span></span>

* <span data-ttu-id="334c9-106">HTTPS für alle Anforderungen erforderlich.</span><span class="sxs-lookup"><span data-stu-id="334c9-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="334c9-107">Alle HTTP-Anforderungen auf HTTPS umleiten.</span><span class="sxs-lookup"><span data-stu-id="334c9-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="334c9-108">Führen Sie **nicht** verwenden [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) auf Web-APIs, die vertraulichen Informationen zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="334c9-108">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="334c9-109">`RequireHttpsAttribute` leitet Browsers per HTTP-Statuscode von HTTP an HTTPS weiter.</span><span class="sxs-lookup"><span data-stu-id="334c9-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="334c9-110">API-Clients verstehen diese Codes möglicherweise nicht, oder Sie führen keine Weiterleitung von HTTP an HTTPS durch.</span><span class="sxs-lookup"><span data-stu-id="334c9-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="334c9-111">Dies kann dazu führen, dass solche Clients Daten unverschlüsselt mittels HTTP versenden.</span><span class="sxs-lookup"><span data-stu-id="334c9-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="334c9-112">Web-APIs sollten daher entweder:</span><span class="sxs-lookup"><span data-stu-id="334c9-112">Web APIs should either:</span></span>
>
> * <span data-ttu-id="334c9-113">nicht auf HTTP lauschen oder</span><span class="sxs-lookup"><span data-stu-id="334c9-113">Not listen on HTTP.</span></span>
> * <span data-ttu-id="334c9-114">die Verbindung mit dem Statuscode 400 („Ungültige Anforderung“) schließen und die Anforderung nicht verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="334c9-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="334c9-115">HTTPS erforderlich</span><span class="sxs-lookup"><span data-stu-id="334c9-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="334c9-116">Es wird empfohlen, alle Web-apps mit ASP.NET Core aufrufen HTTPS-Umleitung Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) alle HTTP-Anforderungen an HTTPS umgeleitet.</span><span class="sxs-lookup"><span data-stu-id="334c9-116">We recommend all ASP.NET Core web apps call HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="334c9-117">Der folgende code ruft `UseHttpsRedirection` in die `Startup` Klasse:</span><span class="sxs-lookup"><span data-stu-id="334c9-117">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

<span data-ttu-id="334c9-118">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span><span class="sxs-lookup"><span data-stu-id="334c9-118">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span></span>

<span data-ttu-id="334c9-119">Der folgende code ruft [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) middlewareoptionen konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="334c9-119">The following code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

<span data-ttu-id="334c9-120">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span><span class="sxs-lookup"><span data-stu-id="334c9-120">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span></span>

<span data-ttu-id="334c9-121">Die vorangehenden hervorgehobenen Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="334c9-121">The preceding highlighted code:</span></span>

* <span data-ttu-id="334c9-122">Legt [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) zu `Status307TemporaryRedirect`, dies ist der Standardwert.</span><span class="sxs-lookup"><span data-stu-id="334c9-122">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to `Status307TemporaryRedirect`, which is the default value.</span></span> <span data-ttu-id="334c9-123">Produktion apps sollten Aufrufen [UseHsts](#hsts).</span><span class="sxs-lookup"><span data-stu-id="334c9-123">Production apps should call [UseHsts](#hsts).</span></span>
* <span data-ttu-id="334c9-124">Legt den HTTPS-Port auf 5001 fest.</span><span class="sxs-lookup"><span data-stu-id="334c9-124">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="334c9-125">Der Standardwert ist 443.</span><span class="sxs-lookup"><span data-stu-id="334c9-125">The default value is 443.</span></span>

<span data-ttu-id="334c9-126">Die folgenden Mechanismen legen Sie den Port automatisch:</span><span class="sxs-lookup"><span data-stu-id="334c9-126">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="334c9-127">Die Middleware erkennen, die Ports über [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) wenn Folgendes zutrifft:</span><span class="sxs-lookup"><span data-stu-id="334c9-127">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>
  - <span data-ttu-id="334c9-128">Kestrel oder HTTP.sys direkt mit HTTPS-Endpunkte verwendet wird (gilt auch zum Ausführen der app mit Visual Studio Code-Debugger).</span><span class="sxs-lookup"><span data-stu-id="334c9-128">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
  - <span data-ttu-id="334c9-129">Nur **eine HTTPS-Port** wird von der app verwendet.</span><span class="sxs-lookup"><span data-stu-id="334c9-129">Only **one HTTPS port** is used by the app.</span></span>
* <span data-ttu-id="334c9-130">Visual Studio wird verwendet:</span><span class="sxs-lookup"><span data-stu-id="334c9-130">Visual Studio is used:</span></span>
  - <span data-ttu-id="334c9-131">IIS Express umfasst HTTPS aktiviert.</span><span class="sxs-lookup"><span data-stu-id="334c9-131">IIS Express has HTTPS enabled.</span></span>
  - <span data-ttu-id="334c9-132">*launchSettings.json* legt die `sslPort` für IIS Express.</span><span class="sxs-lookup"><span data-stu-id="334c9-132">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="334c9-133">Beim Ausführen einer app hinter einem reverse-Proxy (z. B. IIS, IIS Express) `IServerAddressesFeature` ist nicht verfügbar.</span><span class="sxs-lookup"><span data-stu-id="334c9-133">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="334c9-134">Der Port muss manuell konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="334c9-134">The port must be manually configured.</span></span> <span data-ttu-id="334c9-135">Wenn der Port nicht festgelegt ist, werden nicht Anforderungen umgeleitet.</span><span class="sxs-lookup"><span data-stu-id="334c9-135">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="334c9-136">Der Port kann konfiguriert werden, durch Festlegen der:</span><span class="sxs-lookup"><span data-stu-id="334c9-136">The port can be configured by setting the:</span></span>

* <span data-ttu-id="334c9-137">Die Umgebungsvariable `ASPNETCORE_HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="334c9-137">`ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
* <span data-ttu-id="334c9-138">`http_port` Host-Konfigurationsschlüssel (z. B. über *hostsettings.json* oder ein Befehlszeilenargument).</span><span class="sxs-lookup"><span data-stu-id="334c9-138">`http_port` host configuration key (for example, via *hostsettings.json* or a command line argument).</span></span>
* <span data-ttu-id="334c9-139">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span><span class="sxs-lookup"><span data-stu-id="334c9-139">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span></span> <span data-ttu-id="334c9-140">Finden Sie in vorhergehenden Beispiel, das zeigt, wie für den Port 5001 festgelegt.</span><span class="sxs-lookup"><span data-stu-id="334c9-140">See the preceding example that shows how to set the port to 5001.</span></span>

> [!NOTE]
> <span data-ttu-id="334c9-141">Der Port kann indirekt konfiguriert werden, durch Festlegen der URL mit der `ASPNETCORE_URLS` -Umgebungsvariablen angegeben.</span><span class="sxs-lookup"><span data-stu-id="334c9-141">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="334c9-142">Die Umgebungsvariable konfiguriert den Server, und dann die Middleware indirekt über den HTTPS-Port ermittelt `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="334c9-142">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="334c9-143">Wenn kein Port festgelegt ist:</span><span class="sxs-lookup"><span data-stu-id="334c9-143">If no port is set:</span></span>

* <span data-ttu-id="334c9-144">Anforderungen werden nicht umgeleitet.</span><span class="sxs-lookup"><span data-stu-id="334c9-144">Requests aren't redirected.</span></span>
* <span data-ttu-id="334c9-145">Die Middleware protokolliert eine Warnung.</span><span class="sxs-lookup"><span data-stu-id="334c9-145">The middleware logs a warning.</span></span>

> [!NOTE]
> <span data-ttu-id="334c9-146">Eine Alternative zur Verwendung von HTTPS-Umleitung Middleware (`UseHttpsRedirection`) ist die Verwendung der Neuerstellen von URL-Middleware (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="334c9-146">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="334c9-147">`AddRedirectToHttps` können den Statuscode und den Port auch festlegen, wenn die Umleitung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="334c9-147">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="334c9-148">Weitere Informationen finden Sie unter [URL umschreiben Middleware](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="334c9-148">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="334c9-149">Umleiten von ohne zusätzliche umleitungs-Regeln auf HTTPS, empfehlen wir Ihnen mithilfe von HTTPS-Umleitung Middleware (`UseHttpsRedirection`) in diesem Thema beschrieben.</span><span class="sxs-lookup"><span data-stu-id="334c9-149">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="334c9-150">Die [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) wird verwendet, um HTTPS erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="334c9-150">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="334c9-151">`[RequireHttpsAttribute]` ergänzen können, Controllern oder Methoden oder global angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="334c9-151">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="334c9-152">Um das Attribut global anzuwenden, fügen Sie den folgenden Code zum `ConfigureServices` in `Startup`:</span><span class="sxs-lookup"><span data-stu-id="334c9-152">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

<span data-ttu-id="334c9-153">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span><span class="sxs-lookup"><span data-stu-id="334c9-153">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span></span>

<span data-ttu-id="334c9-154">Die vorherige hervorgehobene Code erfordert, verwenden alle Anforderungen `HTTPS`daher HTTP-Anforderungen werden ignoriert.</span><span class="sxs-lookup"><span data-stu-id="334c9-154">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="334c9-155">Die folgende hervorgehobene Code leitet alle HTTP-Anforderungen an HTTPS:</span><span class="sxs-lookup"><span data-stu-id="334c9-155">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

<span data-ttu-id="334c9-156">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span><span class="sxs-lookup"><span data-stu-id="334c9-156">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span></span>

<span data-ttu-id="334c9-157">Weitere Informationen finden Sie unter [URL umschreiben Middleware](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="334c9-157">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="334c9-158">Die Middleware ermöglicht darüber hinaus die app den Statuscode oder den Statuscode und den Port festlegen, wenn die Umleitung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="334c9-158">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="334c9-159">Das globale Erzwingen der Verwendung von HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) ist eine bewährte Sicherheitsmethode.</span><span class="sxs-lookup"><span data-stu-id="334c9-159">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="334c9-160">Dieser Ansatz gilt im Vergleich zur Anwendung des `[RequireHttps]` -Attributs auf alle Controller und Razor Pages als sicherer.</span><span class="sxs-lookup"><span data-stu-id="334c9-160">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="334c9-161">denn Sie können nicht gewährleisten, dass das `[RequireHttps]` -Attribut angewendet wird, wenn neue Controller oder Razor Pages hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="334c9-161">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="334c9-162">HTTP-strikte Sicherheit Transportprotokoll (HSTS)</span><span class="sxs-lookup"><span data-stu-id="334c9-162">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="334c9-163">Pro [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Sicherheit (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) ist eine opt-in-sicherheitserweiterung, die von einer Webanwendung durch die Verwendung eines speziellen Antwortheaders angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="334c9-163">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="334c9-164">Erstellt, sobald ein unterstützter Browser dieser Header empfangen wird dieses Browsers zu verhindern, dass die Kommunikation über HTTP gesendet werden, die der angegebenen Domäne und wird stattdessen die gesamte Kommunikation über HTTPS gesendet.</span><span class="sxs-lookup"><span data-stu-id="334c9-164">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="334c9-165">Es wird verhindert, dass HTTPS auf über aufforderungen auf den Browsern.</span><span class="sxs-lookup"><span data-stu-id="334c9-165">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="334c9-166">ASP.NET Core 2.1 oder höher implementiert HSTS mit der `UseHsts` -Erweiterungsmethode.</span><span class="sxs-lookup"><span data-stu-id="334c9-166">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="334c9-167">Der folgende code ruft `UseHsts` Wenn die app nicht im [-Entwicklungsmodus](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="334c9-167">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

<span data-ttu-id="334c9-168">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="334c9-168">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span></span>

<span data-ttu-id="334c9-169">`UseHsts` wird nicht empfohlen, bei der Entwicklung, da der Header HSTS hoch von Browsern werden kann.</span><span class="sxs-lookup"><span data-stu-id="334c9-169">`UseHsts` is not recommend in development because the HSTS header is highly cachable by browsers.</span></span> <span data-ttu-id="334c9-170">Standardmäßig schließt UseHsts aus die lokalen Loopbackadresse.</span><span class="sxs-lookup"><span data-stu-id="334c9-170">By default, UseHsts excludes the local loopback address.</span></span>

<span data-ttu-id="334c9-171">Der folgende Code</span><span class="sxs-lookup"><span data-stu-id="334c9-171">The following code:</span></span>

<span data-ttu-id="334c9-172">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span><span class="sxs-lookup"><span data-stu-id="334c9-172">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span></span>

* <span data-ttu-id="334c9-173">Legt den Teiler Parameter des Strict-Transport-Security-Headers.</span><span class="sxs-lookup"><span data-stu-id="334c9-173">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="334c9-174">Preload ist nicht Teil der [Spezifikation RFC HSTS](https://tools.ietf.org/html/rfc6797), aber von Webbrowsern zum Vorabladen HSTS Websites Neuinstallation unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="334c9-174">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="334c9-175">Weitere Informationen finden Sie unter [https://hstspreload.org/](https://hstspreload.org/).</span><span class="sxs-lookup"><span data-stu-id="334c9-175">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="334c9-176">Ermöglicht [IncludeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), denen Unterdomänen Host die HSTS-Richtlinie gilt.</span><span class="sxs-lookup"><span data-stu-id="334c9-176">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="334c9-177">Legt explizit den Max-Age-Parameter des Strict-Transport-Security-Headers, der auf 60 Tage.</span><span class="sxs-lookup"><span data-stu-id="334c9-177">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="334c9-178">Wenn dies nicht festgelegt, der Standardwert ist 30 Tage.</span><span class="sxs-lookup"><span data-stu-id="334c9-178">If not set, defaults to 30 days.</span></span> <span data-ttu-id="334c9-179">Finden Sie unter der [Max-Age-Direktive](https://tools.ietf.org/html/rfc6797#section-6.1.1) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="334c9-179">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="334c9-180">Fügt `example.com` zur Liste der Hosts ausschließen.</span><span class="sxs-lookup"><span data-stu-id="334c9-180">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="334c9-181">`UseHsts` Schließt die folgenden Loopback-Hosts an:</span><span class="sxs-lookup"><span data-stu-id="334c9-181">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="334c9-182">`localhost` : Die IPv4-Loopback-Adresse.</span><span class="sxs-lookup"><span data-stu-id="334c9-182">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="334c9-183">`127.0.0.1` : Die IPv4-Loopback-Adresse.</span><span class="sxs-lookup"><span data-stu-id="334c9-183">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="334c9-184">`[::1]` : Die IPv6-Loopback-Adresse.</span><span class="sxs-lookup"><span data-stu-id="334c9-184">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="334c9-185">Das vorhergehende Beispiel zeigt, wie Sie weitere Hosts hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="334c9-185">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="334c9-186">Opt-Out-of HTTPS bei projekterstellung</span><span class="sxs-lookup"><span data-stu-id="334c9-186">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="334c9-187">Aktivieren Sie die ASP.NET Core 2.1 oder höher Anwendung Webvorlagen (in Visual Studio oder der Befehlszeile Dotnet) [HTTPS-Umleitung](#require) und [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="334c9-187">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="334c9-188">Für Bereitstellungen, die nicht HTTPS erforderlich ist, Sie können von HTTPS teilnehmen.</span><span class="sxs-lookup"><span data-stu-id="334c9-188">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="334c9-189">Z. B. einige Back-End-Dienste, in denen HTTPS extern am Netzwerkrand behandelt wird über HTTPS an jedem Knoten, ist nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="334c9-189">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="334c9-190">Um von HTTPS teilnehmen:</span><span class="sxs-lookup"><span data-stu-id="334c9-190">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="334c9-191">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="334c9-191">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="334c9-192">Deaktivieren Sie die **für HTTPS konfigurieren** Kontrollkästchen.</span><span class="sxs-lookup"><span data-stu-id="334c9-192">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Entitätsdiagramm](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="334c9-194">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="334c9-194">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="334c9-195">Verwenden Sie die `--no-https`-Option.</span><span class="sxs-lookup"><span data-stu-id="334c9-195">Use the `--no-https` option.</span></span> <span data-ttu-id="334c9-196">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="334c9-196">For example</span></span>

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a><span data-ttu-id="334c9-198">So richten Sie ein entwicklerzertifikat für Docker</span><span class="sxs-lookup"><span data-stu-id="334c9-198">How to setup a developer certificate for Docker</span></span>

<span data-ttu-id="334c9-199">Finden Sie unter [das GitHub-Problem](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="334c9-199">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end
