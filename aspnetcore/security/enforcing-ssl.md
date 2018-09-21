---
title: Erzwingen von HTTPS in ASP.NET Core
author: rick-anderson
description: Zeigt die Vorgehensweise zum Erzwingen einer HTTPS/TLS, in einer ASP.NET Core-Web-app.
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: 6e16191b1a4627e683fd2281e5556b2a6e84c082
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523142"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="e8a63-103">Erzwingen von HTTPS in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e8a63-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="e8a63-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e8a63-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e8a63-105">Dieses Dokument zeigt, wie Sie:</span><span class="sxs-lookup"><span data-stu-id="e8a63-105">This document shows how to:</span></span>

* <span data-ttu-id="e8a63-106">HTTPS für alle Anforderungen erforderlich.</span><span class="sxs-lookup"><span data-stu-id="e8a63-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="e8a63-107">Alle HTTP-Anforderungen auf HTTPS umleiten.</span><span class="sxs-lookup"><span data-stu-id="e8a63-107">Redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="e8a63-108">Keine API kann verhindern, dass einen Client sensible Daten bei der ersten Anforderung senden.</span><span class="sxs-lookup"><span data-stu-id="e8a63-108">No API can prevent a client from sending sensitive data on the first request.</span></span>

> [!WARNING]
> <span data-ttu-id="e8a63-109">Führen Sie **nicht** verwenden [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) für Web-APIs, die vertraulichen Informationen zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="e8a63-109">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="e8a63-110">`RequireHttpsAttribute` leitet Browsers per HTTP-Statuscode von HTTP an HTTPS weiter.</span><span class="sxs-lookup"><span data-stu-id="e8a63-110">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="e8a63-111">API-Clients verstehen diese Codes möglicherweise nicht, oder Sie führen keine Weiterleitung von HTTP an HTTPS durch.</span><span class="sxs-lookup"><span data-stu-id="e8a63-111">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="e8a63-112">Dies kann dazu führen, dass solche Clients Daten unverschlüsselt mittels HTTP versenden.</span><span class="sxs-lookup"><span data-stu-id="e8a63-112">Such clients may send information over HTTP.</span></span> <span data-ttu-id="e8a63-113">Web-APIs sollten daher entweder:</span><span class="sxs-lookup"><span data-stu-id="e8a63-113">Web APIs should either:</span></span>
>
> * <span data-ttu-id="e8a63-114">nicht auf HTTP lauschen oder</span><span class="sxs-lookup"><span data-stu-id="e8a63-114">Not listen on HTTP.</span></span>
> * <span data-ttu-id="e8a63-115">die Verbindung mit dem Statuscode 400 („Ungültige Anforderung“) schließen und die Anforderung nicht verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="e8a63-115">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="e8a63-116">Anforderung von HTTPS</span><span class="sxs-lookup"><span data-stu-id="e8a63-116">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="e8a63-117">Es empfiehlt sich alle Produktion ASP.NET Core Web-apps-Aufruf:</span><span class="sxs-lookup"><span data-stu-id="e8a63-117">We recommend all production ASP.NET Core web apps call:</span></span>

* <span data-ttu-id="e8a63-118">Die HTTPS-Umleitung-Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) auf alle HTTP-Anfragen an HTTPS umzuleiten.</span><span class="sxs-lookup"><span data-stu-id="e8a63-118">The HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>
* <span data-ttu-id="e8a63-119">[UseHsts](#hsts), HTTP Strict Transport Security-Protokoll (HSTS).</span><span class="sxs-lookup"><span data-stu-id="e8a63-119">[UseHsts](#hsts), HTTP Strict Transport Security Protocol (HSTS).</span></span>

### <a name="usehttpsredirection"></a><span data-ttu-id="e8a63-120">UseHttpsRedirection</span><span class="sxs-lookup"><span data-stu-id="e8a63-120">UseHttpsRedirection</span></span>

<span data-ttu-id="e8a63-121">Der folgende code ruft `UseHttpsRedirection` in die `Startup` Klasse:</span><span class="sxs-lookup"><span data-stu-id="e8a63-121">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="e8a63-122">Der oben markierte Code:</span><span class="sxs-lookup"><span data-stu-id="e8a63-122">The preceding highlighted code:</span></span>

* <span data-ttu-id="e8a63-123">Verwendet die standardmäßige [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span><span class="sxs-lookup"><span data-stu-id="e8a63-123">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span></span>
* <span data-ttu-id="e8a63-124">Verwendet die standardmäßige [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null), es sei denn, durch Überschreiben der `ASPNETCORE_HTTPS_PORT` Umgebungsvariable oder [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span><span class="sxs-lookup"><span data-stu-id="e8a63-124">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) unless overridden by the `ASPNETCORE_HTTPS_PORT` environment variable or [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span></span>

> [!WARNING] 
><span data-ttu-id="e8a63-125">Ein Port muss für die Middleware verfügbar sein, an HTTPS umgeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="e8a63-125">A port must be available for the middleware to redirect to HTTPS.</span></span> <span data-ttu-id="e8a63-126">Wenn kein Port verfügbar ist, erfolgt die Umleitung zu HTTPS nicht.</span><span class="sxs-lookup"><span data-stu-id="e8a63-126">If no port is available, redirection to HTTPS does not occur.</span></span> <span data-ttu-id="e8a63-127">Der HTTPS-Port kann von einem der folgenden Einstellung angegeben werden:</span><span class="sxs-lookup"><span data-stu-id="e8a63-127">The HTTPS port can be specified by any of the following setting:</span></span>
> 
>* `HttpsRedirectionOptions.HttpsPort` 
>* <span data-ttu-id="e8a63-128">Die `ASPNETCORE_HTTPS_PORT` -Umgebungsvariablen angegeben.</span><span class="sxs-lookup"><span data-stu-id="e8a63-128">The `ASPNETCORE_HTTPS_PORT` environment variable.</span></span> 
>* <span data-ttu-id="e8a63-129">Bei der Entwicklung einer HTTPS-Url im *"launchsettings.JSON"*.</span><span class="sxs-lookup"><span data-stu-id="e8a63-129">In development, an HTTPS url in *launchsettings.json*.</span></span> 
>* <span data-ttu-id="e8a63-130">Eine HTTPS-Url direkt auf Kestrel oder HttpSys konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="e8a63-130">An HTTPS url configured directly on Kestrel or HttpSys.</span></span> 

<span data-ttu-id="e8a63-131">Die folgenden hervorgehobenen Code ruft [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) middlewareoptionen konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="e8a63-131">The following highlighted code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="e8a63-132">Aufrufen von `AddHttpsRedirection` ist nur erforderlich, ändern Sie die Werte der ` HttpsPort` oder ` RedirectStatusCode`;</span><span class="sxs-lookup"><span data-stu-id="e8a63-132">Calling `AddHttpsRedirection` is only necessary to change the values of ` HttpsPort` or ` RedirectStatusCode`;</span></span>

<span data-ttu-id="e8a63-133">Der oben markierte Code:</span><span class="sxs-lookup"><span data-stu-id="e8a63-133">The preceding highlighted code:</span></span>

* <span data-ttu-id="e8a63-134">Legt [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) zu `Status307TemporaryRedirect`, dies ist der Standardwert.</span><span class="sxs-lookup"><span data-stu-id="e8a63-134">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to `Status307TemporaryRedirect`, which is the default value.</span></span>
* <span data-ttu-id="e8a63-135">Legt den HTTPS-Port in 5001 fest.</span><span class="sxs-lookup"><span data-stu-id="e8a63-135">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="e8a63-136">Der Standardwert ist 443.</span><span class="sxs-lookup"><span data-stu-id="e8a63-136">The default value is 443.</span></span>

<span data-ttu-id="e8a63-137">Die folgenden Mechanismen legen Sie den Port automatisch:</span><span class="sxs-lookup"><span data-stu-id="e8a63-137">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="e8a63-138">Die Middleware kann ermitteln, die Ports über [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) wenn Folgendes zutrifft:</span><span class="sxs-lookup"><span data-stu-id="e8a63-138">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>

   * <span data-ttu-id="e8a63-139">Kestrel oder HTTP.sys direkt mit HTTPS-Endpunkte verwendet wird (gilt auch für die app mit Visual Studio Code-Debugger ausgeführt wird).</span><span class="sxs-lookup"><span data-stu-id="e8a63-139">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
   * <span data-ttu-id="e8a63-140">Nur **eine HTTPS-Port** wird von der app verwendet.</span><span class="sxs-lookup"><span data-stu-id="e8a63-140">Only **one HTTPS port** is used by the app.</span></span>

* <span data-ttu-id="e8a63-141">Visual Studio wird verwendet:</span><span class="sxs-lookup"><span data-stu-id="e8a63-141">Visual Studio is used:</span></span>
   * <span data-ttu-id="e8a63-142">IIS Express ist HTTPS-tauglich.</span><span class="sxs-lookup"><span data-stu-id="e8a63-142">IIS Express has HTTPS enabled.</span></span>
   * <span data-ttu-id="e8a63-143">*"launchsettings.JSON"* legt die `sslPort` für IIS Express.</span><span class="sxs-lookup"><span data-stu-id="e8a63-143">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="e8a63-144">Wenn eine app ausgeführt wird, hinter einem Reverseproxy (z. B. IIS, IIS Express) `IServerAddressesFeature` ist nicht verfügbar.</span><span class="sxs-lookup"><span data-stu-id="e8a63-144">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="e8a63-145">Der Port muss manuell konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="e8a63-145">The port must be manually configured.</span></span> <span data-ttu-id="e8a63-146">Wenn der Port nicht festgelegt ist, werden nicht die Anforderungen umgeleitet.</span><span class="sxs-lookup"><span data-stu-id="e8a63-146">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="e8a63-147">Der Port kann konfiguriert werden, durch Festlegen der [Https_port Webhost-Konfigurationseinstellung](xref:fundamentals/host/web-host#https-port):</span><span class="sxs-lookup"><span data-stu-id="e8a63-147">The port can be configured by setting the [https_port Web Host configuration setting](xref:fundamentals/host/web-host#https-port):</span></span>

<span data-ttu-id="e8a63-148">**Schlüssel**: Https_port</span><span class="sxs-lookup"><span data-stu-id="e8a63-148">**Key**: https_port</span></span>  
<span data-ttu-id="e8a63-149">**Typ:** *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="e8a63-149">**Type**: *string*</span></span>  
<span data-ttu-id="e8a63-150">**Standardmäßige**: ein Standardwert ist nicht festgelegt.</span><span class="sxs-lookup"><span data-stu-id="e8a63-150">**Default**: A default value isn't set.</span></span>  
<span data-ttu-id="e8a63-151">**Festlegen mit:** `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="e8a63-151">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="e8a63-152">**Umgebungsvariable**: `<PREFIX_>HTTPS_PORT` (das Präfix ist `ASPNETCORE_` bei Verwendung der Web-Host.)</span><span class="sxs-lookup"><span data-stu-id="e8a63-152">**Environment variable**: `<PREFIX_>HTTPS_PORT` (The prefix is `ASPNETCORE_` when using the Web Host.)</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

> [!NOTE]
> <span data-ttu-id="e8a63-153">Der Port kann indirekt konfiguriert werden, durch Festlegen der URL durch die `ASPNETCORE_URLS` -Umgebungsvariablen angegeben.</span><span class="sxs-lookup"><span data-stu-id="e8a63-153">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="e8a63-154">Die Umgebungsvariable konfiguriert den Server, und klicken Sie dann die Middleware indirekt über den HTTPS-Port ermittelt `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="e8a63-154">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="e8a63-155">Wenn kein Port festgelegt ist:</span><span class="sxs-lookup"><span data-stu-id="e8a63-155">If no port is set:</span></span>

* <span data-ttu-id="e8a63-156">Anforderungen werden nicht umgeleitet.</span><span class="sxs-lookup"><span data-stu-id="e8a63-156">Requests aren't redirected.</span></span>
* <span data-ttu-id="e8a63-157">Die Middleware protokolliert die Warnung "Fehler bei den Https-Port für die Umleitung zu bestimmen."</span><span class="sxs-lookup"><span data-stu-id="e8a63-157">The middleware logs the warning "Failed to determine the https port for redirect."</span></span>

> [!NOTE]
> <span data-ttu-id="e8a63-158">Eine Alternative zur Verwendung von HTTPS-Umleitung-Middleware (`UseHttpsRedirection`) ist die Verwendung von URL-Umschreibenden Middleware (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="e8a63-158">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="e8a63-159">`AddRedirectToHttps` können den Statuscode und den Port auch festlegen, wenn die Umleitung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="e8a63-159">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="e8a63-160">Weitere Informationen finden Sie unter [URL-Umschreibenden Middleware](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="e8a63-160">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="e8a63-161">Ohne die Notwendigkeit von zusätzlichen umleitungs-Regeln an HTTPS umleiten möchten, empfehlen wir Ihnen unter Verwendung von HTTPS-Umleitung-Middleware (`UseHttpsRedirection`) in diesem Thema beschrieben.</span><span class="sxs-lookup"><span data-stu-id="e8a63-161">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="e8a63-162">Die [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) wird verwendet, um die HTTPS erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="e8a63-162">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="e8a63-163">`[RequireHttpsAttribute]` ergänzen können, Controllern oder Methoden oder global angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="e8a63-163">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="e8a63-164">Um das Attribut global anzuwenden, fügen Sie den folgenden Code zur `ConfigureServices` in `Startup`:</span><span class="sxs-lookup"><span data-stu-id="e8a63-164">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="e8a63-165">Der oben markierte Code ist erforderlich, alle Anforderungen verwenden `HTTPS`daher HTTP-Anforderungen werden ignoriert.</span><span class="sxs-lookup"><span data-stu-id="e8a63-165">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="e8a63-166">Der folgende hervorgehobene Code leitet alle HTTP-Anfragen an HTTPS um:</span><span class="sxs-lookup"><span data-stu-id="e8a63-166">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="e8a63-167">Weitere Informationen finden Sie unter [URL-Umschreibenden Middleware](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="e8a63-167">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="e8a63-168">Die Middleware kann außerdem die app, die den Statuscode oder den Statuscode sowie den Port festgelegt, wenn die Umleitung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="e8a63-168">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="e8a63-169">Das globale Erzwingen der Verwendung von HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) ist eine bewährte Sicherheitsmethode.</span><span class="sxs-lookup"><span data-stu-id="e8a63-169">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="e8a63-170">Dieser Ansatz gilt im Vergleich zur Anwendung des `[RequireHttps]` -Attributs auf alle Controller und Razor Pages als sicherer.</span><span class="sxs-lookup"><span data-stu-id="e8a63-170">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="e8a63-171">denn Sie können nicht gewährleisten, dass das `[RequireHttps]` -Attribut angewendet wird, wenn neue Controller oder Razor Pages hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="e8a63-171">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="e8a63-172">HTTP Strict Transport Security-Protokoll (HSTS)</span><span class="sxs-lookup"><span data-stu-id="e8a63-172">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="e8a63-173">Pro [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) ist eine optionale sicherheitserweiterung, die von einer Web-app durch die Verwendung eines Antwortheaders angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="e8a63-173">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that's specified by a web app through the use of a response header.</span></span> <span data-ttu-id="e8a63-174">Wenn eine [Browser mit Unterstützung von HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) diesen Header empfängt:</span><span class="sxs-lookup"><span data-stu-id="e8a63-174">When a [browser that supports HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) receives this header:</span></span>

* <span data-ttu-id="e8a63-175">Der Browser speichert die Konfiguration für die Domäne, die verhindert, dass eine Kommunikation über HTTP senden.</span><span class="sxs-lookup"><span data-stu-id="e8a63-175">The browser stores configuration for the domain that prevents sending any communication over HTTP.</span></span> <span data-ttu-id="e8a63-176">Der Browser wird die gesamte Kommunikation über HTTPS erzwungen.</span><span class="sxs-lookup"><span data-stu-id="e8a63-176">The browser forces all communication over HTTPS.</span></span>
* <span data-ttu-id="e8a63-177">Der Browser wird verhindert, dass der Benutzer mithilfe von Zertifikaten für nicht vertrauenswürdig oder ungültig.</span><span class="sxs-lookup"><span data-stu-id="e8a63-177">The browser prevents the user from using untrusted or invalid certificates.</span></span> <span data-ttu-id="e8a63-178">Der Browser deaktiviert eingabeaufforderungen, die einen solches Zertifikat vorübergehend vertrauen Benutzer zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="e8a63-178">The browser disables prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="e8a63-179">Da HSTS vom Client erzwungen wird, hat es einige Einschränkungen:</span><span class="sxs-lookup"><span data-stu-id="e8a63-179">Because HSTS is enforced by the client it has some limitations:</span></span>

* <span data-ttu-id="e8a63-180">Der Client muss HSTS unterstützen.</span><span class="sxs-lookup"><span data-stu-id="e8a63-180">The client must support HSTS.</span></span>
* <span data-ttu-id="e8a63-181">HSTS muss mindestens eine erfolgreiche HTTPS-Anforderung zu, um die Richtlinie HSTS herzustellen.</span><span class="sxs-lookup"><span data-stu-id="e8a63-181">HSTS requires at least one successful HTTPS request to establish the HSTS policy.</span></span>
* <span data-ttu-id="e8a63-182">Die Anwendung muss jede HTTP-Anforderung überprüfen und umleiten oder die HTTP-Anforderung ablehnen.</span><span class="sxs-lookup"><span data-stu-id="e8a63-182">The application must check every HTTP request and redirect or reject the HTTP request.</span></span>

<span data-ttu-id="e8a63-183">ASP.NET Core 2.1 oder höher implementiert HSTS mit der `UseHsts` -Erweiterungsmethode.</span><span class="sxs-lookup"><span data-stu-id="e8a63-183">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="e8a63-184">Der folgende code ruft `UseHsts` bei der app nicht im [Entwicklungsmodus](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="e8a63-184">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="e8a63-185">`UseHsts` ist nicht in der Entwicklung empfohlen, da die Einstellungen HSTS hohem Maße zwischenspeicherbar sind von Browsern.</span><span class="sxs-lookup"><span data-stu-id="e8a63-185">`UseHsts` isn't recommended in development because the HSTS settings are highly cacheable by browsers.</span></span> <span data-ttu-id="e8a63-186">In der Standardeinstellung `UseHsts` schließt die lokalen Loopback-Adresse.</span><span class="sxs-lookup"><span data-stu-id="e8a63-186">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="e8a63-187">Für produktionsumgebungen HTTPS zum ersten Mal implementieren wird den Anfangswert von HSTS auf einen kleinen Wert fest.</span><span class="sxs-lookup"><span data-stu-id="e8a63-187">For production environments implementing HTTPS for the first time, set the initial HSTS value to a small value.</span></span> <span data-ttu-id="e8a63-188">Legen Sie den Wert von Stunden keine mehr als ein Tag für den Fall, dass Sie die HTTP-HTTPS-Infrastruktur wiederherstellen müssen.</span><span class="sxs-lookup"><span data-stu-id="e8a63-188">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="e8a63-189">Nachdem Sie sich die Nachhaltigkeit der HTTPS-Konfiguration sicher sind, erhöhen Sie den HSTS-Max-Age-Wert; häufig verwendeter Wert ist ein Jahr.</span><span class="sxs-lookup"><span data-stu-id="e8a63-189">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span> 

<span data-ttu-id="e8a63-190">Der folgende Code</span><span class="sxs-lookup"><span data-stu-id="e8a63-190">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="e8a63-191">Legt den preload Parameter des Strict-Transport-Security-Headers.</span><span class="sxs-lookup"><span data-stu-id="e8a63-191">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="e8a63-192">Preload ist nicht Teil der [RFC HSTS Spezifikation](https://tools.ietf.org/html/rfc6797), aber vom Webbrowser zum Vorabladen von HSTS Websites Neuinstallation unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="e8a63-192">Preload isn't part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="e8a63-193">Weitere Informationen finden Sie unter [https://hstspreload.org/](https://hstspreload.org/).</span><span class="sxs-lookup"><span data-stu-id="e8a63-193">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="e8a63-194">Ermöglicht [IncludeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), denen Unterdomänen der Host die HSTS-Richtlinie gilt.</span><span class="sxs-lookup"><span data-stu-id="e8a63-194">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="e8a63-195">Explizit festlegt den Max-Age-Parameter, der den Strict-Transport-Security-Header auf 60 Tage.</span><span class="sxs-lookup"><span data-stu-id="e8a63-195">Explicitly sets the max-age parameter of the Strict-Transport-Security header to 60 days.</span></span> <span data-ttu-id="e8a63-196">Wenn nicht festgelegt, der Standardwert ist 30 Tage.</span><span class="sxs-lookup"><span data-stu-id="e8a63-196">If not set, defaults to 30 days.</span></span> <span data-ttu-id="e8a63-197">Finden Sie unter den [Max-Age-Direktive](https://tools.ietf.org/html/rfc6797#section-6.1.1) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="e8a63-197">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="e8a63-198">Fügt `example.com` zur Liste der Hosts zu schließen.</span><span class="sxs-lookup"><span data-stu-id="e8a63-198">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="e8a63-199">`UseHsts` Schließt die folgenden Loopback-Hosts an:</span><span class="sxs-lookup"><span data-stu-id="e8a63-199">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="e8a63-200">`localhost` : Die IPv4-Loopback-Adresse.</span><span class="sxs-lookup"><span data-stu-id="e8a63-200">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="e8a63-201">`127.0.0.1` : Die IPv4-Loopback-Adresse.</span><span class="sxs-lookup"><span data-stu-id="e8a63-201">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="e8a63-202">`[::1]` : Die IPv6-Loopback-Adresse.</span><span class="sxs-lookup"><span data-stu-id="e8a63-202">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="e8a63-203">Im vorherige Beispiel veranschaulicht das weitere Hosts hinzufügen möchten.</span><span class="sxs-lookup"><span data-stu-id="e8a63-203">The preceding example shows how to add additional hosts.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="e8a63-204">Deaktivieren von HTTPS bei projekterstellung</span><span class="sxs-lookup"><span data-stu-id="e8a63-204">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="e8a63-205">Aktivieren Sie die ASP.NET Core 2.1 oder höher Webanwendungsvorlagen (von Visual Studio oder der Dotnet-Befehlszeile) [HTTPS-Umleitung](#require) und [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="e8a63-205">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="e8a63-206">Für Bereitstellungen, die keine HTTPS erforderlich ist, Sie können HTTPS deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="e8a63-206">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="e8a63-207">Z. B. einige Back-End-Dienste, in denen HTTPS extern am Netzwerkrand behandelt wird über HTTPS an jedem Knoten, ist nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="e8a63-207">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node isn't needed.</span></span>

<span data-ttu-id="e8a63-208">Zum Deaktivieren von HTTPS:</span><span class="sxs-lookup"><span data-stu-id="e8a63-208">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e8a63-209">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8a63-209">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="e8a63-210">Deaktivieren Sie die **für HTTPS konfigurieren** Kontrollkästchen.</span><span class="sxs-lookup"><span data-stu-id="e8a63-210">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Entitätsdiagramm](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e8a63-212">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="e8a63-212">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="e8a63-213">Verwenden Sie die `--no-https`-Option.</span><span class="sxs-lookup"><span data-stu-id="e8a63-213">Use the `--no-https` option.</span></span> <span data-ttu-id="e8a63-214">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="e8a63-214">For example</span></span>

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a><span data-ttu-id="e8a63-215">So richten Sie ein entwicklerzertifikat für Docker</span><span class="sxs-lookup"><span data-stu-id="e8a63-215">How to set up a developer certificate for Docker</span></span>

<span data-ttu-id="e8a63-216">Finden Sie unter [GitHub-Problem](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="e8a63-216">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end

## <a name="additional-information"></a><span data-ttu-id="e8a63-217">Zusätzliche Informationen</span><span class="sxs-lookup"><span data-stu-id="e8a63-217">Additional information</span></span>

* [<span data-ttu-id="e8a63-218">OWASP HSTS-Browserunterstützung</span><span class="sxs-lookup"><span data-stu-id="e8a63-218">OWASP HSTS browser support</span></span>](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
