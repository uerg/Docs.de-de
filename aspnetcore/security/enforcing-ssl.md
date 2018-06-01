---
title: Erzwingen von HTTPS in einer ASP.NET Core
author: rick-anderson
description: Veranschaulicht, wie HTTPS/TLS in einer ASP.NET Core erfordern Web-app.
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 0433ddb3bf1ef0074c683903ad4553cd6a0b4741
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/31/2018
ms.locfileid: "34687817"
---
# <a name="enforce-https-in-an-aspnet-core"></a><span data-ttu-id="0e3f0-103">Erzwingen von HTTPS in einer ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0e3f0-103">Enforce HTTPS in an ASP.NET Core</span></span>

<span data-ttu-id="0e3f0-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0e3f0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0e3f0-105">Dieses Dokument zeigt, wie Sie:</span><span class="sxs-lookup"><span data-stu-id="0e3f0-105">This document shows how to:</span></span>

- <span data-ttu-id="0e3f0-106">HTTPS für alle Anforderungen erforderlich.</span><span class="sxs-lookup"><span data-stu-id="0e3f0-106">Require HTTPS for all requests.</span></span>
- <span data-ttu-id="0e3f0-107">Alle HTTP-Anforderungen auf HTTPS umleiten.</span><span class="sxs-lookup"><span data-stu-id="0e3f0-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="0e3f0-108">Verwenden Sie `RequireHttpsAttribute` **nicht** bei Web-APIs, die vertrauliche Informationen entgegennehmen.</span><span class="sxs-lookup"><span data-stu-id="0e3f0-108">Do **not** use `RequireHttpsAttribute` on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="0e3f0-109">`RequireHttpsAttribute` leitet Browsers per HTTP-Statuscode von HTTP an HTTPS weiter.</span><span class="sxs-lookup"><span data-stu-id="0e3f0-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="0e3f0-110">API-Clients verstehen diese Codes möglicherweise nicht, oder Sie führen keine Weiterleitung von HTTP an HTTPS durch.</span><span class="sxs-lookup"><span data-stu-id="0e3f0-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="0e3f0-111">Dies kann dazu führen, dass solche Clients Daten unverschlüsselt mittels HTTP versenden.</span><span class="sxs-lookup"><span data-stu-id="0e3f0-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="0e3f0-112">Web-APIs sollten daher entweder:</span><span class="sxs-lookup"><span data-stu-id="0e3f0-112">Web APIs should either:</span></span>
>
>* <span data-ttu-id="0e3f0-113">nicht auf HTTP lauschen oder</span><span class="sxs-lookup"><span data-stu-id="0e3f0-113">Not listen on HTTP.</span></span>
>* <span data-ttu-id="0e3f0-114">die Verbindung mit dem Statuscode 400 („Ungültige Anforderung“) schließen und die Anforderung nicht verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="0e3f0-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="0e3f0-115">HTTPS erforderlich</span><span class="sxs-lookup"><span data-stu-id="0e3f0-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="0e3f0-116">Wir empfehlen alle ASP.NET Core Aufruf der Web-apps `UseHttpsRedirection` alle HTTP-Anforderungen an HTTPS umgeleitet.</span><span class="sxs-lookup"><span data-stu-id="0e3f0-116">We recommend all ASP.NET Core web apps call `UseHttpsRedirection` to redirect all HTTP requests to HTTPS.</span></span> <span data-ttu-id="0e3f0-117">Wenn `UseHsts` wird aufgerufen, in der app, sondern muss aufgerufen werden vor dem `UseHttpsRedirection`.</span><span class="sxs-lookup"><span data-stu-id="0e3f0-117">If `UseHsts` is called in the app, it must be called before `UseHttpsRedirection`.</span></span>

<span data-ttu-id="0e3f0-118">Der folgende code ruft `UseHttpsRedirection` in die `Startup` Klasse:</span><span class="sxs-lookup"><span data-stu-id="0e3f0-118">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

<span data-ttu-id="0e3f0-119">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span><span class="sxs-lookup"><span data-stu-id="0e3f0-119">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span></span>


<span data-ttu-id="0e3f0-120">Der folgende Code</span><span class="sxs-lookup"><span data-stu-id="0e3f0-120">The following code:</span></span>

<span data-ttu-id="0e3f0-121">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span><span class="sxs-lookup"><span data-stu-id="0e3f0-121">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span></span>

* <span data-ttu-id="0e3f0-122">Legt `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="0e3f0-122">Sets `RedirectStatusCode`.</span></span>
* <span data-ttu-id="0e3f0-123">Legt den HTTPS-Port auf 5001 fest.</span><span class="sxs-lookup"><span data-stu-id="0e3f0-123">Sets the HTTPS port to 5001.</span></span>

::: moniker-end


::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="0e3f0-124">Die [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) wird verwendet, um HTTPS erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="0e3f0-124">The [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) is used to require HTTPS.</span></span> <span data-ttu-id="0e3f0-125">`[RequireHttpsAttribute]` ergänzen können, Controllern oder Methoden oder global angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="0e3f0-125">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="0e3f0-126">Um das Attribut global anzuwenden, fügen Sie den folgenden Code zum `ConfigureServices` in `Startup`:</span><span class="sxs-lookup"><span data-stu-id="0e3f0-126">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

<span data-ttu-id="0e3f0-127">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span><span class="sxs-lookup"><span data-stu-id="0e3f0-127">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span></span>

<span data-ttu-id="0e3f0-128">Die vorherige hervorgehobene Code erfordert, verwenden alle Anforderungen `HTTPS`daher HTTP-Anforderungen werden ignoriert.</span><span class="sxs-lookup"><span data-stu-id="0e3f0-128">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="0e3f0-129">Die folgende hervorgehobene Code leitet alle HTTP-Anforderungen an HTTPS:</span><span class="sxs-lookup"><span data-stu-id="0e3f0-129">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

<span data-ttu-id="0e3f0-130">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span><span class="sxs-lookup"><span data-stu-id="0e3f0-130">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span></span>

<span data-ttu-id="0e3f0-131">Weitere Informationen finden Sie unter [URL umschreiben Middleware](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="0e3f0-131">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="0e3f0-132">Das globale Erzwingen der Verwendung von HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) ist eine bewährte Sicherheitsmethode.</span><span class="sxs-lookup"><span data-stu-id="0e3f0-132">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="0e3f0-133">Dieser Ansatz gilt im Vergleich zur Anwendung des `[RequireHttps]` -Attributs auf alle Controller und Razor Pages als sicherer.</span><span class="sxs-lookup"><span data-stu-id="0e3f0-133">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="0e3f0-134">denn Sie können nicht gewährleisten, dass das `[RequireHttps]` -Attribut angewendet wird, wenn neue Controller oder Razor Pages hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="0e3f0-134">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="0e3f0-135">HTTP-strikte Sicherheit Transportprotokoll (HSTS)</span><span class="sxs-lookup"><span data-stu-id="0e3f0-135">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="0e3f0-136">Pro [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Sicherheit (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) ist eine opt-in-sicherheitserweiterung, die von einer Webanwendung durch die Verwendung eines speziellen Antwortheaders angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="0e3f0-136">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="0e3f0-137">Erstellt, sobald ein unterstützter Browser dieser Header empfangen wird dieses Browsers zu verhindern, dass die Kommunikation über HTTP gesendet werden, die der angegebenen Domäne und wird stattdessen die gesamte Kommunikation über HTTPS gesendet.</span><span class="sxs-lookup"><span data-stu-id="0e3f0-137">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="0e3f0-138">Es wird verhindert, dass HTTPS auf über aufforderungen auf den Browsern.</span><span class="sxs-lookup"><span data-stu-id="0e3f0-138">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="0e3f0-139">ASP.NET Core 2.1 oder höher implementiert HSTS mit der `UseHsts` -Erweiterungsmethode.</span><span class="sxs-lookup"><span data-stu-id="0e3f0-139">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="0e3f0-140">Der folgende code ruft `UseHsts` Wenn die app nicht im [-Entwicklungsmodus](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="0e3f0-140">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

<span data-ttu-id="0e3f0-141">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="0e3f0-141">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span></span>

<span data-ttu-id="0e3f0-142">`UseHsts` wird nicht empfohlen, bei der Entwicklung, da der Header HSTS hoch von Browsern werden kann.</span><span class="sxs-lookup"><span data-stu-id="0e3f0-142">`UseHsts` is not recommend in development because the HSTS header is highly cachable by browsers.</span></span> <span data-ttu-id="0e3f0-143">Standardmäßig schließt UseHsts aus die lokalen Loopbackadresse.</span><span class="sxs-lookup"><span data-stu-id="0e3f0-143">By default, UseHsts excludes the local loopback address.</span></span>

<span data-ttu-id="0e3f0-144">Der folgende Code</span><span class="sxs-lookup"><span data-stu-id="0e3f0-144">The following code:</span></span>

<span data-ttu-id="0e3f0-145">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span><span class="sxs-lookup"><span data-stu-id="0e3f0-145">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span></span>

* <span data-ttu-id="0e3f0-146">Legt den Teiler Parameter des Strict-Transport-Security-Headers.</span><span class="sxs-lookup"><span data-stu-id="0e3f0-146">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="0e3f0-147">Preload ist nicht Teil der [Spezifikation RFC HSTS](https://tools.ietf.org/html/rfc6797), aber von Webbrowsern zum Vorabladen HSTS Websites Neuinstallation unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="0e3f0-147">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="0e3f0-148">Weitere Informationen finden Sie unter [https://hstspreload.org/](https://hstspreload.org/).</span><span class="sxs-lookup"><span data-stu-id="0e3f0-148">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="0e3f0-149">Ermöglicht [IncludeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), denen Unterdomänen Host die HSTS-Richtlinie gilt.</span><span class="sxs-lookup"><span data-stu-id="0e3f0-149">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="0e3f0-150">Legt explizit den Max-Age-Parameter des Strict-Transport-Security-Headers, der auf 60 Tage.</span><span class="sxs-lookup"><span data-stu-id="0e3f0-150">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="0e3f0-151">Wenn dies nicht festgelegt, der Standardwert ist 30 Tage.</span><span class="sxs-lookup"><span data-stu-id="0e3f0-151">If not set, defaults to 30 days.</span></span> <span data-ttu-id="0e3f0-152">Finden Sie unter der [Max-Age-Direktive](https://tools.ietf.org/html/rfc6797#section-6.1.1) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="0e3f0-152">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="0e3f0-153">Fügt `example.com` zur Liste der Hosts ausschließen.</span><span class="sxs-lookup"><span data-stu-id="0e3f0-153">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="0e3f0-154">`UseHsts` Schließt die folgenden Loopback-Hosts an:</span><span class="sxs-lookup"><span data-stu-id="0e3f0-154">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="0e3f0-155">`localhost` : Die IPv4-Loopback-Adresse.</span><span class="sxs-lookup"><span data-stu-id="0e3f0-155">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="0e3f0-156">`127.0.0.1` : Die IPv4-Loopback-Adresse.</span><span class="sxs-lookup"><span data-stu-id="0e3f0-156">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="0e3f0-157">`[::1]` : Die IPv6-Loopback-Adresse.</span><span class="sxs-lookup"><span data-stu-id="0e3f0-157">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="0e3f0-158">Das vorhergehende Beispiel zeigt, wie Sie weitere Hosts hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="0e3f0-158">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end


::: moniker range=">= aspnetcore-2.1"
<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="0e3f0-159">Opt-Out-of HTTPS bei projekterstellung</span><span class="sxs-lookup"><span data-stu-id="0e3f0-159">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="0e3f0-160">Aktivieren Sie die ASP.NET Core 2.1 und höher Anwendung Webvorlagen (in Visual Studio oder der Befehlszeile Dotnet) [HTTPS-Umleitung](#require) und [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="0e3f0-160">The ASP.NET Core 2.1 and later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="0e3f0-161">Für Bereitstellungen, die nicht HTTPS erforderlich ist, Sie können von HTTPS teilnehmen.</span><span class="sxs-lookup"><span data-stu-id="0e3f0-161">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="0e3f0-162">Z. B. einige Back-End-Dienste, in denen HTTPS extern am Netzwerkrand behandelt wird über HTTPS an jedem Knoten, ist nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="0e3f0-162">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="0e3f0-163">Um von HTTPS teilnehmen:</span><span class="sxs-lookup"><span data-stu-id="0e3f0-163">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0e3f0-164">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0e3f0-164">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="0e3f0-165">Deaktivieren Sie die **für HTTPS konfigurieren** Kontrollkästchen.</span><span class="sxs-lookup"><span data-stu-id="0e3f0-165">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Entitätsdiagramm](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0e3f0-167">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="0e3f0-167">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="0e3f0-168">Verwenden Sie die `--no-https`-Option.</span><span class="sxs-lookup"><span data-stu-id="0e3f0-168">Use the `--no-https` option.</span></span> <span data-ttu-id="0e3f0-169">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0e3f0-169">For example</span></span>

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
## <a name="how-to-setup-a-developer-certificate-for-docker"></a><span data-ttu-id="0e3f0-170">So richten Sie ein entwicklerzertifikat für Docker</span><span class="sxs-lookup"><span data-stu-id="0e3f0-170">How to setup a developer certificate for Docker</span></span>

<span data-ttu-id="0e3f0-171">Finden Sie unter [das GitHub-Problem](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="0e3f0-171">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end
