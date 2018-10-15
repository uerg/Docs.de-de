---
title: Erzwingen von HTTPS in ASP.NET Core
author: rick-anderson
description: Erfahren Sie, wie Sie HTTPS/TLS in einer ASP.NET Core-Web-app benötigen.
ms.author: riande
ms.custom: mvc
ms.date: 10/11/2018
uid: security/enforcing-ssl
ms.openlocfilehash: b4c058d3b4f00276043d9520d756e62ed8cac5d9
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325600"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="474a8-103">Erzwingen von HTTPS in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="474a8-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="474a8-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="474a8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="474a8-105">Dieses Dokument zeigt, wie Sie:</span><span class="sxs-lookup"><span data-stu-id="474a8-105">This document shows how to:</span></span>

* <span data-ttu-id="474a8-106">HTTPS für alle Anforderungen erforderlich.</span><span class="sxs-lookup"><span data-stu-id="474a8-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="474a8-107">Alle HTTP-Anforderungen auf HTTPS umleiten.</span><span class="sxs-lookup"><span data-stu-id="474a8-107">Redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="474a8-108">Keine API kann verhindern, dass einen Client sensible Daten bei der ersten Anforderung senden.</span><span class="sxs-lookup"><span data-stu-id="474a8-108">No API can prevent a client from sending sensitive data on the first request.</span></span>

> [!WARNING]
> <span data-ttu-id="474a8-109">Führen Sie **nicht** verwenden [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) für Web-APIs, die vertraulichen Informationen zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="474a8-109">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="474a8-110">`RequireHttpsAttribute` leitet Browsers per HTTP-Statuscode von HTTP an HTTPS weiter.</span><span class="sxs-lookup"><span data-stu-id="474a8-110">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="474a8-111">API-Clients verstehen diese Codes möglicherweise nicht, oder Sie führen keine Weiterleitung von HTTP an HTTPS durch.</span><span class="sxs-lookup"><span data-stu-id="474a8-111">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="474a8-112">Dies kann dazu führen, dass solche Clients Daten unverschlüsselt mittels HTTP versenden.</span><span class="sxs-lookup"><span data-stu-id="474a8-112">Such clients may send information over HTTP.</span></span> <span data-ttu-id="474a8-113">Web-APIs sollten daher entweder:</span><span class="sxs-lookup"><span data-stu-id="474a8-113">Web APIs should either:</span></span>
>
> * <span data-ttu-id="474a8-114">nicht auf HTTP lauschen oder</span><span class="sxs-lookup"><span data-stu-id="474a8-114">Not listen on HTTP.</span></span>
> * <span data-ttu-id="474a8-115">die Verbindung mit dem Statuscode 400 („Ungültige Anforderung“) schließen und die Anforderung nicht verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="474a8-115">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>

## <a name="require-https"></a><span data-ttu-id="474a8-116">Anforderung von HTTPS</span><span class="sxs-lookup"><span data-stu-id="474a8-116">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="474a8-117">Es empfiehlt sich alle Produktion ASP.NET Core Web-apps-Aufruf:</span><span class="sxs-lookup"><span data-stu-id="474a8-117">We recommend all production ASP.NET Core web apps call:</span></span>

* <span data-ttu-id="474a8-118">Die HTTPS-Umleitung-Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) auf alle HTTP-Anfragen an HTTPS umzuleiten.</span><span class="sxs-lookup"><span data-stu-id="474a8-118">The HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>
* <span data-ttu-id="474a8-119">[UseHsts](#hsts), HTTP Strict Transport Security-Protokoll (HSTS).</span><span class="sxs-lookup"><span data-stu-id="474a8-119">[UseHsts](#hsts), HTTP Strict Transport Security Protocol (HSTS).</span></span>

### <a name="usehttpsredirection"></a><span data-ttu-id="474a8-120">UseHttpsRedirection</span><span class="sxs-lookup"><span data-stu-id="474a8-120">UseHttpsRedirection</span></span>

<span data-ttu-id="474a8-121">Der folgende code ruft `UseHttpsRedirection` in die `Startup` Klasse:</span><span class="sxs-lookup"><span data-stu-id="474a8-121">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="474a8-122">Der oben markierte Code:</span><span class="sxs-lookup"><span data-stu-id="474a8-122">The preceding highlighted code:</span></span>

* <span data-ttu-id="474a8-123">Verwendet die standardmäßige [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span><span class="sxs-lookup"><span data-stu-id="474a8-123">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span></span>
* <span data-ttu-id="474a8-124">Verwendet die standardmäßige [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null), es sei denn, durch Überschreiben der `ASPNETCORE_HTTPS_PORT` Umgebungsvariable oder [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span><span class="sxs-lookup"><span data-stu-id="474a8-124">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) unless overridden by the `ASPNETCORE_HTTPS_PORT` environment variable or [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span></span>

<span data-ttu-id="474a8-125">Es wird empfohlen statt temporäre umleitungen dauerhafte umleitungen, wie die Zwischenspeicherung von Ressourcenlinks instabil Verhalten in entwicklungsumgebungen führen kann.</span><span class="sxs-lookup"><span data-stu-id="474a8-125">We recommend using temporary redirects rather than permanent redirects, as link caching can cause unstable behavior in development environments.</span></span> <span data-ttu-id="474a8-126">Es wird empfohlen, [HSTS](#hsts) , um Clients zu signalisieren, die Ressource nur sichere Anforderungen an die app (nur in der Produktion) gesendet werden sollen.</span><span class="sxs-lookup"><span data-stu-id="474a8-126">We recommend using [HSTS](#hsts) to signal to clients that only secure resource requests should be sent to the app (only in production).</span></span>

> [!WARNING]
> <span data-ttu-id="474a8-127">Ein Port muss für die Middleware verfügbar sein, an HTTPS umgeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="474a8-127">A port must be available for the middleware to redirect to HTTPS.</span></span> <span data-ttu-id="474a8-128">Wenn kein Port verfügbar ist, tritt kein Umleitung auf HTTPS.</span><span class="sxs-lookup"><span data-stu-id="474a8-128">If no port is available, redirection to HTTPS doesn't occur.</span></span> <span data-ttu-id="474a8-129">Der HTTPS-Port kann mit einer der folgenden Methoden angegeben werden:</span><span class="sxs-lookup"><span data-stu-id="474a8-129">The HTTPS port can be specified using any of the following approaches:</span></span>
>
> * <span data-ttu-id="474a8-130">Legen Sie `HttpsRedirectionOptions.HttpsPort`.</span><span class="sxs-lookup"><span data-stu-id="474a8-130">Set `HttpsRedirectionOptions.HttpsPort`.</span></span>
> * <span data-ttu-id="474a8-131">Festlegen der Umgebungsvariable `ASPNETCORE_HTTPS_PORT`.</span><span class="sxs-lookup"><span data-stu-id="474a8-131">Set the `ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
> * <span data-ttu-id="474a8-132">Legen Sie bei der Entwicklung eine HTTPS-URL in *"launchsettings.JSON"*.</span><span class="sxs-lookup"><span data-stu-id="474a8-132">In development, set an HTTPS URL in *launchsettings.json*.</span></span>
> * <span data-ttu-id="474a8-133">Konfigurieren Sie einen HTTPS-URL-Endpunkt für [Kestrel](xref:fundamentals/servers/kestrel) oder [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="474a8-133">Configure an HTTPS URL endpoint for [Kestrel](xref:fundamentals/servers/kestrel) or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>
>
> <span data-ttu-id="474a8-134">Wenn Kestrel oder HTTP.sys als öffentlich zugängliche Edgeserver verwendet wird, muss Kestrel oder HTTP.sys zum Lauschen an beide konfiguriert werden:</span><span class="sxs-lookup"><span data-stu-id="474a8-134">When Kestrel or HTTP.sys is used as a public-facing edge server, Kestrel or HTTP.sys must be configured to listen on both:</span></span>
>
> * <span data-ttu-id="474a8-135">Die, in dem der Client umgeleitet wird, sicherer Port (in der Regel Port 443 in der Produktion und 5001 in der Entwicklung).</span><span class="sxs-lookup"><span data-stu-id="474a8-135">The secure port where the client is redirected (typically, 443 in production and 5001 in development).</span></span>
> * <span data-ttu-id="474a8-136">Der unsichere Port (üblicherweise 80 in der Produktion) und 5000 in der Entwicklung.</span><span class="sxs-lookup"><span data-stu-id="474a8-136">The insecure port (typically, 80 in production and 5000 in development).</span></span>
>
> <span data-ttu-id="474a8-137">Der unsichere Port muss vom Client in der Reihenfolge für die app eine unsichere Anforderung erhalten, und leiten Sie ihn mit dem sicheren Port zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="474a8-137">The insecure port must be accessible by the client in order for the app to receive an insecure request and redirect it to the secure port.</span></span>
>
> <span data-ttu-id="474a8-138">Jeder Firewall zwischen dem Client und Server müssen auch die Ports für den Datenverkehr zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="474a8-138">Any firewall between the client and server must also have the ports open for traffic.</span></span>
>
> <span data-ttu-id="474a8-139">Weitere Informationen finden Sie unter [Kestrel-Endpunktkonfiguration](xref:fundamentals/servers/kestrel#endpoint-configuration) oder <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="474a8-139">For more information, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) or <xref:fundamentals/servers/httpsys>.</span></span>

<span data-ttu-id="474a8-140">Die folgenden hervorgehobenen Code ruft [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) middlewareoptionen konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="474a8-140">The following highlighted code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="474a8-141">Aufrufen von `AddHttpsRedirection` ist nur erforderlich, ändern Sie die Werte der `HttpsPort` oder `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="474a8-141">Calling `AddHttpsRedirection` is only necessary to change the values of `HttpsPort` or `RedirectStatusCode`.</span></span>

<span data-ttu-id="474a8-142">Der oben markierte Code:</span><span class="sxs-lookup"><span data-stu-id="474a8-142">The preceding highlighted code:</span></span>

* <span data-ttu-id="474a8-143">Legt [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) zu [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect), dies ist der Standardwert.</span><span class="sxs-lookup"><span data-stu-id="474a8-143">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect), which is the default value.</span></span> <span data-ttu-id="474a8-144">Verwenden Sie die Felder von der [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) -Klasse für Zuweisungen zu `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="474a8-144">Use the fields of the [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) class for assignments to `RedirectStatusCode`.</span></span>
* <span data-ttu-id="474a8-145">Legt den HTTPS-Port in 5001 fest.</span><span class="sxs-lookup"><span data-stu-id="474a8-145">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="474a8-146">Der Standardwert ist 443.</span><span class="sxs-lookup"><span data-stu-id="474a8-146">The default value is 443.</span></span>

<span data-ttu-id="474a8-147">Die folgenden Mechanismen legen Sie den Port automatisch:</span><span class="sxs-lookup"><span data-stu-id="474a8-147">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="474a8-148">Die Middleware kann ermitteln, die Ports über [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) wenn Folgendes zutrifft:</span><span class="sxs-lookup"><span data-stu-id="474a8-148">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>

  * <span data-ttu-id="474a8-149">Kestrel oder HTTP.sys direkt mit HTTPS-Endpunkte verwendet wird (gilt auch für die app mit Visual Studio Code-Debugger ausgeführt wird).</span><span class="sxs-lookup"><span data-stu-id="474a8-149">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
  * <span data-ttu-id="474a8-150">Nur **eine HTTPS-Port** wird von der app verwendet.</span><span class="sxs-lookup"><span data-stu-id="474a8-150">Only **one HTTPS port** is used by the app.</span></span>

* <span data-ttu-id="474a8-151">Visual Studio wird verwendet:</span><span class="sxs-lookup"><span data-stu-id="474a8-151">Visual Studio is used:</span></span>

  * <span data-ttu-id="474a8-152">IIS Express ist HTTPS-tauglich.</span><span class="sxs-lookup"><span data-stu-id="474a8-152">IIS Express has HTTPS enabled.</span></span>
  * <span data-ttu-id="474a8-153">*"launchsettings.JSON"* legt die `sslPort` für IIS Express.</span><span class="sxs-lookup"><span data-stu-id="474a8-153">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="474a8-154">Wenn eine app ausgeführt wird, hinter einem Reverseproxy (z. B. IIS, IIS Express) `IServerAddressesFeature` ist nicht verfügbar.</span><span class="sxs-lookup"><span data-stu-id="474a8-154">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="474a8-155">Der Port muss manuell konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="474a8-155">The port must be manually configured.</span></span> <span data-ttu-id="474a8-156">Wenn der Port nicht festgelegt ist, werden nicht die Anforderungen umgeleitet.</span><span class="sxs-lookup"><span data-stu-id="474a8-156">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="474a8-157">Der Port kann konfiguriert werden, durch Festlegen der [Https_port Webhost-Konfigurationseinstellung](xref:fundamentals/host/web-host#https-port):</span><span class="sxs-lookup"><span data-stu-id="474a8-157">The port can be configured by setting the [https_port Web Host configuration setting](xref:fundamentals/host/web-host#https-port):</span></span>

<span data-ttu-id="474a8-158">**Schlüssel**: Https_port</span><span class="sxs-lookup"><span data-stu-id="474a8-158">**Key**: https_port</span></span>  
<span data-ttu-id="474a8-159">**Typ:** *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="474a8-159">**Type**: *string*</span></span>  
<span data-ttu-id="474a8-160">**Standardmäßige**: ein Standardwert ist nicht festgelegt.</span><span class="sxs-lookup"><span data-stu-id="474a8-160">**Default**: A default value isn't set.</span></span>  
<span data-ttu-id="474a8-161">**Festlegen mit:** `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="474a8-161">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="474a8-162">**Umgebungsvariable**: `<PREFIX_>HTTPS_PORT` (das Präfix ist `ASPNETCORE_` bei Verwendung der Web-Host.)</span><span class="sxs-lookup"><span data-stu-id="474a8-162">**Environment variable**: `<PREFIX_>HTTPS_PORT` (The prefix is `ASPNETCORE_` when using the Web Host.)</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

> [!NOTE]
> <span data-ttu-id="474a8-163">Der Port kann indirekt konfiguriert werden, durch Festlegen der URL durch die `ASPNETCORE_URLS` -Umgebungsvariablen angegeben.</span><span class="sxs-lookup"><span data-stu-id="474a8-163">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="474a8-164">Die Umgebungsvariable konfiguriert den Server, und klicken Sie dann die Middleware indirekt über den HTTPS-Port ermittelt `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="474a8-164">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="474a8-165">Wenn kein Port festgelegt ist:</span><span class="sxs-lookup"><span data-stu-id="474a8-165">If no port is set:</span></span>

* <span data-ttu-id="474a8-166">Anforderungen werden nicht umgeleitet.</span><span class="sxs-lookup"><span data-stu-id="474a8-166">Requests aren't redirected.</span></span>
* <span data-ttu-id="474a8-167">Die Middleware protokolliert die Warnung "Fehler bei den Https-Port für die Umleitung zu bestimmen."</span><span class="sxs-lookup"><span data-stu-id="474a8-167">The middleware logs the warning "Failed to determine the https port for redirect."</span></span>

> [!NOTE]
> <span data-ttu-id="474a8-168">Eine Alternative zur Verwendung von HTTPS-Umleitung-Middleware (`UseHttpsRedirection`) ist die Verwendung von URL-Umschreibenden Middleware (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="474a8-168">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="474a8-169">`AddRedirectToHttps` können den Statuscode und den Port auch festlegen, wenn die Umleitung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="474a8-169">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="474a8-170">Weitere Informationen finden Sie unter [URL-Umschreibenden Middleware](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="474a8-170">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="474a8-171">Ohne die Notwendigkeit von zusätzlichen umleitungs-Regeln an HTTPS umleiten möchten, empfehlen wir Ihnen unter Verwendung von HTTPS-Umleitung-Middleware (`UseHttpsRedirection`) in diesem Thema beschrieben.</span><span class="sxs-lookup"><span data-stu-id="474a8-171">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="474a8-172">Die [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) wird verwendet, um die HTTPS erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="474a8-172">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="474a8-173">`[RequireHttpsAttribute]` ergänzen können, Controllern oder Methoden oder global angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="474a8-173">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="474a8-174">Um das Attribut global anzuwenden, fügen Sie den folgenden Code zur `ConfigureServices` in `Startup`:</span><span class="sxs-lookup"><span data-stu-id="474a8-174">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="474a8-175">Der oben markierte Code ist erforderlich, alle Anforderungen verwenden `HTTPS`daher HTTP-Anforderungen werden ignoriert.</span><span class="sxs-lookup"><span data-stu-id="474a8-175">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="474a8-176">Der folgende hervorgehobene Code leitet alle HTTP-Anfragen an HTTPS um:</span><span class="sxs-lookup"><span data-stu-id="474a8-176">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="474a8-177">Weitere Informationen finden Sie unter [URL-Umschreibenden Middleware](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="474a8-177">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="474a8-178">Die Middleware kann außerdem die app, die den Statuscode oder den Statuscode sowie den Port festgelegt, wenn die Umleitung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="474a8-178">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="474a8-179">Das globale Erzwingen der Verwendung von HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) ist eine bewährte Sicherheitsmethode.</span><span class="sxs-lookup"><span data-stu-id="474a8-179">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="474a8-180">Dieser Ansatz gilt im Vergleich zur Anwendung des `[RequireHttps]` -Attributs auf alle Controller und Razor Pages als sicherer.</span><span class="sxs-lookup"><span data-stu-id="474a8-180">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="474a8-181">denn Sie können nicht gewährleisten, dass das `[RequireHttps]` -Attribut angewendet wird, wenn neue Controller oder Razor Pages hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="474a8-181">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>

## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="474a8-182">HTTP Strict Transport Security-Protokoll (HSTS)</span><span class="sxs-lookup"><span data-stu-id="474a8-182">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="474a8-183">Pro [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) ist eine optionale sicherheitserweiterung, die von einer Web-app durch die Verwendung eines Antwortheaders angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="474a8-183">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that's specified by a web app through the use of a response header.</span></span> <span data-ttu-id="474a8-184">Wenn eine [Browser mit Unterstützung von HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) diesen Header empfängt:</span><span class="sxs-lookup"><span data-stu-id="474a8-184">When a [browser that supports HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) receives this header:</span></span>

* <span data-ttu-id="474a8-185">Der Browser speichert die Konfiguration für die Domäne, die verhindert, dass eine Kommunikation über HTTP senden.</span><span class="sxs-lookup"><span data-stu-id="474a8-185">The browser stores configuration for the domain that prevents sending any communication over HTTP.</span></span> <span data-ttu-id="474a8-186">Der Browser wird die gesamte Kommunikation über HTTPS erzwungen.</span><span class="sxs-lookup"><span data-stu-id="474a8-186">The browser forces all communication over HTTPS.</span></span>
* <span data-ttu-id="474a8-187">Der Browser wird verhindert, dass der Benutzer mithilfe von Zertifikaten für nicht vertrauenswürdig oder ungültig.</span><span class="sxs-lookup"><span data-stu-id="474a8-187">The browser prevents the user from using untrusted or invalid certificates.</span></span> <span data-ttu-id="474a8-188">Der Browser deaktiviert eingabeaufforderungen, die einen solches Zertifikat vorübergehend vertrauen Benutzer zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="474a8-188">The browser disables prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="474a8-189">Da HSTS vom Client erzwungen wird, hat es einige Einschränkungen:</span><span class="sxs-lookup"><span data-stu-id="474a8-189">Because HSTS is enforced by the client it has some limitations:</span></span>

* <span data-ttu-id="474a8-190">Der Client muss HSTS unterstützen.</span><span class="sxs-lookup"><span data-stu-id="474a8-190">The client must support HSTS.</span></span>
* <span data-ttu-id="474a8-191">HSTS muss mindestens eine erfolgreiche HTTPS-Anforderung zu, um die Richtlinie HSTS herzustellen.</span><span class="sxs-lookup"><span data-stu-id="474a8-191">HSTS requires at least one successful HTTPS request to establish the HSTS policy.</span></span>
* <span data-ttu-id="474a8-192">Die Anwendung muss jede HTTP-Anforderung überprüfen und umleiten oder die HTTP-Anforderung ablehnen.</span><span class="sxs-lookup"><span data-stu-id="474a8-192">The application must check every HTTP request and redirect or reject the HTTP request.</span></span>

<span data-ttu-id="474a8-193">ASP.NET Core 2.1 oder höher implementiert HSTS mit der `UseHsts` -Erweiterungsmethode.</span><span class="sxs-lookup"><span data-stu-id="474a8-193">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="474a8-194">Der folgende code ruft `UseHsts` bei der app nicht im [Entwicklungsmodus](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="474a8-194">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="474a8-195">`UseHsts` ist nicht in der Entwicklung empfohlen, da die Einstellungen HSTS hohem Maße zwischenspeicherbar sind von Browsern.</span><span class="sxs-lookup"><span data-stu-id="474a8-195">`UseHsts` isn't recommended in development because the HSTS settings are highly cacheable by browsers.</span></span> <span data-ttu-id="474a8-196">In der Standardeinstellung `UseHsts` schließt die lokalen Loopback-Adresse.</span><span class="sxs-lookup"><span data-stu-id="474a8-196">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="474a8-197">Für produktionsumgebungen HTTPS zum ersten Mal implementieren wird den Anfangswert von HSTS auf einen kleinen Wert fest.</span><span class="sxs-lookup"><span data-stu-id="474a8-197">For production environments implementing HTTPS for the first time, set the initial HSTS value to a small value.</span></span> <span data-ttu-id="474a8-198">Legen Sie den Wert von Stunden keine mehr als ein Tag für den Fall, dass Sie die HTTP-HTTPS-Infrastruktur wiederherstellen müssen.</span><span class="sxs-lookup"><span data-stu-id="474a8-198">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="474a8-199">Nachdem Sie sich die Nachhaltigkeit der HTTPS-Konfiguration sicher sind, erhöhen Sie den HSTS-Max-Age-Wert; häufig verwendeter Wert ist ein Jahr.</span><span class="sxs-lookup"><span data-stu-id="474a8-199">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span> 

<span data-ttu-id="474a8-200">Der folgende Code</span><span class="sxs-lookup"><span data-stu-id="474a8-200">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="474a8-201">Legt den preload Parameter des Strict-Transport-Security-Headers.</span><span class="sxs-lookup"><span data-stu-id="474a8-201">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="474a8-202">Preload ist nicht Teil der [RFC HSTS Spezifikation](https://tools.ietf.org/html/rfc6797), aber vom Webbrowser zum Vorabladen von HSTS Websites Neuinstallation unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="474a8-202">Preload isn't part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="474a8-203">Weitere Informationen finden Sie unter [https://hstspreload.org/](https://hstspreload.org/).</span><span class="sxs-lookup"><span data-stu-id="474a8-203">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="474a8-204">Ermöglicht [IncludeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), denen Unterdomänen der Host die HSTS-Richtlinie gilt.</span><span class="sxs-lookup"><span data-stu-id="474a8-204">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span>
* <span data-ttu-id="474a8-205">Explizit festlegt den Max-Age-Parameter, der den Strict-Transport-Security-Header auf 60 Tage.</span><span class="sxs-lookup"><span data-stu-id="474a8-205">Explicitly sets the max-age parameter of the Strict-Transport-Security header to 60 days.</span></span> <span data-ttu-id="474a8-206">Wenn nicht festgelegt, der Standardwert ist 30 Tage.</span><span class="sxs-lookup"><span data-stu-id="474a8-206">If not set, defaults to 30 days.</span></span> <span data-ttu-id="474a8-207">Finden Sie unter den [Max-Age-Direktive](https://tools.ietf.org/html/rfc6797#section-6.1.1) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="474a8-207">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="474a8-208">Fügt `example.com` zur Liste der Hosts zu schließen.</span><span class="sxs-lookup"><span data-stu-id="474a8-208">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="474a8-209">`UseHsts` Schließt die folgenden Loopback-Hosts an:</span><span class="sxs-lookup"><span data-stu-id="474a8-209">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="474a8-210">`localhost` : Die IPv4-Loopback-Adresse.</span><span class="sxs-lookup"><span data-stu-id="474a8-210">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="474a8-211">`127.0.0.1` : Die IPv4-Loopback-Adresse.</span><span class="sxs-lookup"><span data-stu-id="474a8-211">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="474a8-212">`[::1]` : Die IPv6-Loopback-Adresse.</span><span class="sxs-lookup"><span data-stu-id="474a8-212">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="474a8-213">Im vorherige Beispiel veranschaulicht das weitere Hosts hinzufügen möchten.</span><span class="sxs-lookup"><span data-stu-id="474a8-213">The preceding example shows how to add additional hosts.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>

## <a name="opt-out-of-httpshsts-on-project-creation"></a><span data-ttu-id="474a8-214">Deaktivieren von HTTPS/HSTS bei projekterstellung</span><span class="sxs-lookup"><span data-stu-id="474a8-214">Opt-out of HTTPS/HSTS on project creation</span></span>

<span data-ttu-id="474a8-215">In einigen Back-End-Dienst-Szenarien, in denen verbindungssicherheit am Rand des Netzwerks öffentlich zugänglichen behandelt wird, ist nicht Konfigurieren der Sicherheit der Serververbindung über jeden Knoten erforderlich.</span><span class="sxs-lookup"><span data-stu-id="474a8-215">In some backend service scenarios where connection security is handled at the public-facing edge of the network, configuring connection security at each node isn't required.</span></span> <span data-ttu-id="474a8-216">Web-apps, die aus den Vorlagen in Visual Studio oder über generiert die [Dotnet neue](/dotnet/core/tools/dotnet-new) Befehls aktivieren [HTTPS-Umleitung](#require) und [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="474a8-216">Web apps generated from the templates in Visual Studio or from the [dotnet new](/dotnet/core/tools/dotnet-new) command enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="474a8-217">Für Bereitstellungen, die diese Szenarien erfordern nicht, Sie können HTTPS/HSTS deaktivieren, wenn die app aus der Vorlage erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="474a8-217">For deployments that don't require these scenarios, you can opt-out of HTTPS/HSTS when the app is created from the template.</span></span>

<span data-ttu-id="474a8-218">Zum Deaktivieren von HTTPS/HSTS:</span><span class="sxs-lookup"><span data-stu-id="474a8-218">To opt-out of HTTPS/HSTS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="474a8-219">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="474a8-219">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="474a8-220">Deaktivieren Sie die **für HTTPS konfigurieren** Kontrollkästchen.</span><span class="sxs-lookup"><span data-stu-id="474a8-220">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Entitätsdiagramm](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="474a8-222">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="474a8-222">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="474a8-223">Verwenden Sie die `--no-https`-Option.</span><span class="sxs-lookup"><span data-stu-id="474a8-223">Use the `--no-https` option.</span></span> <span data-ttu-id="474a8-224">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="474a8-224">For example</span></span>

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a><span data-ttu-id="474a8-225">So richten Sie ein entwicklerzertifikat für Docker</span><span class="sxs-lookup"><span data-stu-id="474a8-225">How to set up a developer certificate for Docker</span></span>

<span data-ttu-id="474a8-226">Finden Sie unter [GitHub-Problem](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="474a8-226">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end

## <a name="additional-information"></a><span data-ttu-id="474a8-227">Zusätzliche Informationen</span><span class="sxs-lookup"><span data-stu-id="474a8-227">Additional information</span></span>

* [<span data-ttu-id="474a8-228">OWASP HSTS-Browserunterstützung</span><span class="sxs-lookup"><span data-stu-id="474a8-228">OWASP HSTS browser support</span></span>](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
