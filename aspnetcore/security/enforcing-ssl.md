---
title: Erzwingen von HTTPS in ASP.NET Core
author: rick-anderson
description: Zeigt die Vorgehensweise zum Erzwingen einer HTTPS/TLS, in einer ASP.NET Core-Web-app.
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: 3bea8661e17fec5128e822d98741d1f8ed7434e5
ms.sourcegitcommit: 028ad28c546de706ace98066c76774de33e4ad20
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2018
ms.locfileid: "39655497"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="b842a-103">Erzwingen von HTTPS in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b842a-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="b842a-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b842a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b842a-105">Dieses Dokument zeigt, wie Sie:</span><span class="sxs-lookup"><span data-stu-id="b842a-105">This document shows how to:</span></span>

* <span data-ttu-id="b842a-106">HTTPS für alle Anforderungen erforderlich.</span><span class="sxs-lookup"><span data-stu-id="b842a-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="b842a-107">Alle HTTP-Anforderungen auf HTTPS umleiten.</span><span class="sxs-lookup"><span data-stu-id="b842a-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="b842a-108">Führen Sie **nicht** verwenden [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) für Web-APIs, die vertraulichen Informationen zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="b842a-108">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="b842a-109">`RequireHttpsAttribute` leitet Browsers per HTTP-Statuscode von HTTP an HTTPS weiter.</span><span class="sxs-lookup"><span data-stu-id="b842a-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="b842a-110">API-Clients verstehen diese Codes möglicherweise nicht, oder Sie führen keine Weiterleitung von HTTP an HTTPS durch.</span><span class="sxs-lookup"><span data-stu-id="b842a-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="b842a-111">Dies kann dazu führen, dass solche Clients Daten unverschlüsselt mittels HTTP versenden.</span><span class="sxs-lookup"><span data-stu-id="b842a-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="b842a-112">Web-APIs sollten daher entweder:</span><span class="sxs-lookup"><span data-stu-id="b842a-112">Web APIs should either:</span></span>
>
> * <span data-ttu-id="b842a-113">nicht auf HTTP lauschen oder</span><span class="sxs-lookup"><span data-stu-id="b842a-113">Not listen on HTTP.</span></span>
> * <span data-ttu-id="b842a-114">die Verbindung mit dem Statuscode 400 („Ungültige Anforderung“) schließen und die Anforderung nicht verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="b842a-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="b842a-115">Anforderung von HTTPS</span><span class="sxs-lookup"><span data-stu-id="b842a-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="b842a-116">Es wird empfohlen, alle ASP.NET Core-Web-apps rufen die HTTPS-Umleitung-Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) auf alle HTTP-Anfragen an HTTPS umzuleiten.</span><span class="sxs-lookup"><span data-stu-id="b842a-116">We recommend all ASP.NET Core web apps call HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="b842a-117">Der folgende code ruft `UseHttpsRedirection` in die `Startup` Klasse:</span><span class="sxs-lookup"><span data-stu-id="b842a-117">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="b842a-118">Der oben markierte Code:</span><span class="sxs-lookup"><span data-stu-id="b842a-118">The preceding highlighted code:</span></span>

* <span data-ttu-id="b842a-119">Verwendet die standardmäßige [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span><span class="sxs-lookup"><span data-stu-id="b842a-119">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span></span> <span data-ttu-id="b842a-120">Produktions-apps sollten Aufrufen [UseHsts](#hsts).</span><span class="sxs-lookup"><span data-stu-id="b842a-120">Production apps should call [UseHsts](#hsts).</span></span>
* <span data-ttu-id="b842a-121">Verwendet die standardmäßige [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (443).</span><span class="sxs-lookup"><span data-stu-id="b842a-121">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (443).</span></span>

<span data-ttu-id="b842a-122">Der folgende code ruft [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) middlewareoptionen konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="b842a-122">The following code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="b842a-123">Der oben markierte Code:</span><span class="sxs-lookup"><span data-stu-id="b842a-123">The preceding highlighted code:</span></span>

* <span data-ttu-id="b842a-124">Legt [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) zu `Status307TemporaryRedirect`, dies ist der Standardwert.</span><span class="sxs-lookup"><span data-stu-id="b842a-124">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to `Status307TemporaryRedirect`, which is the default value.</span></span> <span data-ttu-id="b842a-125">Produktions-apps sollten Aufrufen [UseHsts](#hsts).</span><span class="sxs-lookup"><span data-stu-id="b842a-125">Production apps should call [UseHsts](#hsts).</span></span>
* <span data-ttu-id="b842a-126">Legt den HTTPS-Port in 5001 fest.</span><span class="sxs-lookup"><span data-stu-id="b842a-126">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="b842a-127">Der Standardwert ist 443.</span><span class="sxs-lookup"><span data-stu-id="b842a-127">The default value is 443.</span></span>

<span data-ttu-id="b842a-128">Die folgenden Mechanismen legen Sie den Port automatisch:</span><span class="sxs-lookup"><span data-stu-id="b842a-128">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="b842a-129">Die Middleware kann ermitteln, die Ports über [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) wenn Folgendes zutrifft:</span><span class="sxs-lookup"><span data-stu-id="b842a-129">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>
  - <span data-ttu-id="b842a-130">Kestrel oder HTTP.sys direkt mit HTTPS-Endpunkte verwendet wird (gilt auch für die app mit Visual Studio Code-Debugger ausgeführt wird).</span><span class="sxs-lookup"><span data-stu-id="b842a-130">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
  - <span data-ttu-id="b842a-131">Nur **eine HTTPS-Port** wird von der app verwendet.</span><span class="sxs-lookup"><span data-stu-id="b842a-131">Only **one HTTPS port** is used by the app.</span></span>
* <span data-ttu-id="b842a-132">Visual Studio wird verwendet:</span><span class="sxs-lookup"><span data-stu-id="b842a-132">Visual Studio is used:</span></span>
  - <span data-ttu-id="b842a-133">IIS Express ist HTTPS-tauglich.</span><span class="sxs-lookup"><span data-stu-id="b842a-133">IIS Express has HTTPS enabled.</span></span>
  - <span data-ttu-id="b842a-134">*"launchsettings.JSON"* legt die `sslPort` für IIS Express.</span><span class="sxs-lookup"><span data-stu-id="b842a-134">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="b842a-135">Wenn eine app ausgeführt wird, hinter einem Reverseproxy (z. B. IIS, IIS Express) `IServerAddressesFeature` ist nicht verfügbar.</span><span class="sxs-lookup"><span data-stu-id="b842a-135">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="b842a-136">Der Port muss manuell konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="b842a-136">The port must be manually configured.</span></span> <span data-ttu-id="b842a-137">Wenn der Port nicht festgelegt ist, werden nicht die Anforderungen umgeleitet.</span><span class="sxs-lookup"><span data-stu-id="b842a-137">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="b842a-138">Der Port kann konfiguriert werden, durch Festlegen der [Https_port Webhost-Konfigurationseinstellung](xref:fundamentals/host/web-host#https-port):</span><span class="sxs-lookup"><span data-stu-id="b842a-138">The port can be configured by setting the [https_port Web Host configuration setting](xref:fundamentals/host/web-host#https-port):</span></span>

<span data-ttu-id="b842a-139">**Schlüssel**: https_port **Typ**: *string*
**Standard**: Es ist kein Standardwert festgelegt.</span><span class="sxs-lookup"><span data-stu-id="b842a-139">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="b842a-140">**Legen Sie mithilfe von**: `UseSetting` 
 **Umgebungsvariable**: `<PREFIX_>HTTPS_PORT` (das Präfix ist `ASPNETCORE_` bei Verwendung der Web-Host.)</span><span class="sxs-lookup"><span data-stu-id="b842a-140">**Set using**: `UseSetting`
**Environment variable**: `<PREFIX_>HTTPS_PORT` (The prefix is `ASPNETCORE_` when using the Web Host.)</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

> [!NOTE]
> <span data-ttu-id="b842a-141">Der Port kann indirekt konfiguriert werden, durch Festlegen der URL durch die `ASPNETCORE_URLS` -Umgebungsvariablen angegeben.</span><span class="sxs-lookup"><span data-stu-id="b842a-141">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="b842a-142">Die Umgebungsvariable konfiguriert den Server, und klicken Sie dann die Middleware indirekt über den HTTPS-Port ermittelt `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="b842a-142">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="b842a-143">Wenn kein Port festgelegt ist:</span><span class="sxs-lookup"><span data-stu-id="b842a-143">If no port is set:</span></span>

* <span data-ttu-id="b842a-144">Anforderungen werden nicht umgeleitet.</span><span class="sxs-lookup"><span data-stu-id="b842a-144">Requests aren't redirected.</span></span>
* <span data-ttu-id="b842a-145">Die Middleware protokolliert eine Warnung.</span><span class="sxs-lookup"><span data-stu-id="b842a-145">The middleware logs a warning.</span></span>

> [!NOTE]
> <span data-ttu-id="b842a-146">Eine Alternative zur Verwendung von HTTPS-Umleitung-Middleware (`UseHttpsRedirection`) ist die Verwendung von URL-Umschreibenden Middleware (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="b842a-146">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="b842a-147">`AddRedirectToHttps` können den Statuscode und den Port auch festlegen, wenn die Umleitung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="b842a-147">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="b842a-148">Weitere Informationen finden Sie unter [URL-Umschreibenden Middleware](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="b842a-148">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="b842a-149">Ohne die Notwendigkeit von zusätzlichen umleitungs-Regeln an HTTPS umleiten möchten, empfehlen wir Ihnen unter Verwendung von HTTPS-Umleitung-Middleware (`UseHttpsRedirection`) in diesem Thema beschrieben.</span><span class="sxs-lookup"><span data-stu-id="b842a-149">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="b842a-150">Die [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) wird verwendet, um die HTTPS erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="b842a-150">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="b842a-151">`[RequireHttpsAttribute]` ergänzen können, Controllern oder Methoden oder global angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="b842a-151">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="b842a-152">Um das Attribut global anzuwenden, fügen Sie den folgenden Code zur `ConfigureServices` in `Startup`:</span><span class="sxs-lookup"><span data-stu-id="b842a-152">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="b842a-153">Der oben markierte Code ist erforderlich, alle Anforderungen verwenden `HTTPS`daher HTTP-Anforderungen werden ignoriert.</span><span class="sxs-lookup"><span data-stu-id="b842a-153">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="b842a-154">Der folgende hervorgehobene Code leitet alle HTTP-Anfragen an HTTPS um:</span><span class="sxs-lookup"><span data-stu-id="b842a-154">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="b842a-155">Weitere Informationen finden Sie unter [URL-Umschreibenden Middleware](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="b842a-155">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="b842a-156">Die Middleware kann außerdem die app, die den Statuscode oder den Statuscode sowie den Port festgelegt, wenn die Umleitung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="b842a-156">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="b842a-157">Das globale Erzwingen der Verwendung von HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) ist eine bewährte Sicherheitsmethode.</span><span class="sxs-lookup"><span data-stu-id="b842a-157">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="b842a-158">Dieser Ansatz gilt im Vergleich zur Anwendung des `[RequireHttps]` -Attributs auf alle Controller und Razor Pages als sicherer.</span><span class="sxs-lookup"><span data-stu-id="b842a-158">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="b842a-159">denn Sie können nicht gewährleisten, dass das `[RequireHttps]` -Attribut angewendet wird, wenn neue Controller oder Razor Pages hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="b842a-159">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="b842a-160">HTTP Strict Transport Security-Protokoll (HSTS)</span><span class="sxs-lookup"><span data-stu-id="b842a-160">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="b842a-161">Pro [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) ist eine optionale sicherheitserweiterung, die von einer Web-app durch die Verwendung eines Antwortheaders angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="b842a-161">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that's specified by a web app through the use of a response header.</span></span> <span data-ttu-id="b842a-162">Wenn ein Browser, der mit Unterstützung von HSTS diesen Header empfängt:</span><span class="sxs-lookup"><span data-stu-id="b842a-162">When a browser that supports HSTS receives this header:</span></span>

* <span data-ttu-id="b842a-163">Der Browser speichert die Konfiguration für die Domäne, die verhindert, dass eine Kommunikation über HTTP senden.</span><span class="sxs-lookup"><span data-stu-id="b842a-163">The browser stores configuration for the domain that prevents sending any communication over HTTP.</span></span> <span data-ttu-id="b842a-164">Der Browser wird die gesamte Kommunikation über HTTPS erzwungen.</span><span class="sxs-lookup"><span data-stu-id="b842a-164">The browser forces all communication over HTTPS.</span></span> 
* <span data-ttu-id="b842a-165">Der Browser wird verhindert, dass der Benutzer mithilfe von Zertifikaten für nicht vertrauenswürdig oder ungültig.</span><span class="sxs-lookup"><span data-stu-id="b842a-165">The browser prevents the user from using untrusted or invalid certificates.</span></span> <span data-ttu-id="b842a-166">Der Browser deaktiviert eingabeaufforderungen, die einen solches Zertifikat vorübergehend vertrauen Benutzer zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="b842a-166">The browser disables prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="b842a-167">ASP.NET Core 2.1 oder höher implementiert HSTS mit der `UseHsts` -Erweiterungsmethode.</span><span class="sxs-lookup"><span data-stu-id="b842a-167">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="b842a-168">Der folgende code ruft `UseHsts` bei der app nicht im [Entwicklungsmodus](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="b842a-168">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="b842a-169">`UseHsts` ist nicht in der Entwicklung empfohlen, da der Header des HSTS hohem Maße zwischenspeicherbar ist von Browsern.</span><span class="sxs-lookup"><span data-stu-id="b842a-169">`UseHsts` isn't recommended in development because the HSTS header is highly cacheable by browsers.</span></span> <span data-ttu-id="b842a-170">In der Standardeinstellung `UseHsts` schließt die lokalen Loopback-Adresse.</span><span class="sxs-lookup"><span data-stu-id="b842a-170">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="b842a-171">Für produktionsumgebungen HTTPS zum ersten Mal implementieren wird den Anfangswert von HSTS auf einen kleinen Wert fest.</span><span class="sxs-lookup"><span data-stu-id="b842a-171">For production environments implementing HTTPS for the first time, set the initial HSTS value to a small value.</span></span> <span data-ttu-id="b842a-172">Legen Sie den Wert von Stunden keine mehr als ein Tag für den Fall, dass Sie die HTTP-HTTPS-Infrastruktur wiederherstellen müssen.</span><span class="sxs-lookup"><span data-stu-id="b842a-172">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="b842a-173">Nachdem Sie sich die Nachhaltigkeit der HTTPS-Konfiguration sicher sind, erhöhen Sie den HSTS-Max-Age-Wert; häufig verwendeter Wert ist ein Jahr.</span><span class="sxs-lookup"><span data-stu-id="b842a-173">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span> 

<span data-ttu-id="b842a-174">Der folgende Code</span><span class="sxs-lookup"><span data-stu-id="b842a-174">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="b842a-175">Legt den preload Parameter des Strict-Transport-Security-Headers.</span><span class="sxs-lookup"><span data-stu-id="b842a-175">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="b842a-176">Preload ist nicht Teil der [RFC HSTS Spezifikation](https://tools.ietf.org/html/rfc6797), aber vom Webbrowser zum Vorabladen von HSTS Websites Neuinstallation unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="b842a-176">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="b842a-177">Weitere Informationen finden Sie unter [https://hstspreload.org/](https://hstspreload.org/).</span><span class="sxs-lookup"><span data-stu-id="b842a-177">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="b842a-178">Ermöglicht [IncludeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), denen Unterdomänen der Host die HSTS-Richtlinie gilt.</span><span class="sxs-lookup"><span data-stu-id="b842a-178">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="b842a-179">Explizit festlegt den Max-Age-Parameter des Strict-Transport-Security-Headers auf 60 Tage.</span><span class="sxs-lookup"><span data-stu-id="b842a-179">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="b842a-180">Wenn nicht festgelegt, der Standardwert ist 30 Tage.</span><span class="sxs-lookup"><span data-stu-id="b842a-180">If not set, defaults to 30 days.</span></span> <span data-ttu-id="b842a-181">Finden Sie unter den [Max-Age-Direktive](https://tools.ietf.org/html/rfc6797#section-6.1.1) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="b842a-181">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="b842a-182">Fügt `example.com` zur Liste der Hosts zu schließen.</span><span class="sxs-lookup"><span data-stu-id="b842a-182">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="b842a-183">`UseHsts` Schließt die folgenden Loopback-Hosts an:</span><span class="sxs-lookup"><span data-stu-id="b842a-183">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="b842a-184">`localhost` : Die IPv4-Loopback-Adresse.</span><span class="sxs-lookup"><span data-stu-id="b842a-184">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="b842a-185">`127.0.0.1` : Die IPv4-Loopback-Adresse.</span><span class="sxs-lookup"><span data-stu-id="b842a-185">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="b842a-186">`[::1]` : Die IPv6-Loopback-Adresse.</span><span class="sxs-lookup"><span data-stu-id="b842a-186">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="b842a-187">Im vorherige Beispiel veranschaulicht das weitere Hosts hinzufügen möchten.</span><span class="sxs-lookup"><span data-stu-id="b842a-187">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="b842a-188">Deaktivieren von HTTPS bei projekterstellung</span><span class="sxs-lookup"><span data-stu-id="b842a-188">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="b842a-189">Aktivieren Sie die ASP.NET Core 2.1 oder höher Webanwendungsvorlagen (von Visual Studio oder der Dotnet-Befehlszeile) [HTTPS-Umleitung](#require) und [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="b842a-189">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="b842a-190">Für Bereitstellungen, die keine HTTPS erforderlich ist, Sie können HTTPS deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="b842a-190">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="b842a-191">Z. B. einige Back-End-Dienste, in denen HTTPS extern am Netzwerkrand behandelt wird über HTTPS an jedem Knoten, ist nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="b842a-191">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="b842a-192">Zum Deaktivieren von HTTPS:</span><span class="sxs-lookup"><span data-stu-id="b842a-192">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b842a-193">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b842a-193">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="b842a-194">Deaktivieren Sie die **für HTTPS konfigurieren** Kontrollkästchen.</span><span class="sxs-lookup"><span data-stu-id="b842a-194">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Entitätsdiagramm](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b842a-196">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="b842a-196">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="b842a-197">Verwenden Sie die `--no-https`-Option.</span><span class="sxs-lookup"><span data-stu-id="b842a-197">Use the `--no-https` option.</span></span> <span data-ttu-id="b842a-198">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b842a-198">For example</span></span>

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a><span data-ttu-id="b842a-199">Zum Einrichten von einem entwicklerzertifikat für Docker</span><span class="sxs-lookup"><span data-stu-id="b842a-199">How to setup a developer certificate for Docker</span></span>

<span data-ttu-id="b842a-200">Finden Sie unter [GitHub-Problem](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="b842a-200">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end
