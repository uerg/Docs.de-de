---
title: Konfigurieren von ASP.NET Core zum Arbeiten mit Proxyservern und Lastenausgleich
author: guardrex
description: Informationen Sie zur Konfiguration für apps gehostet, Proxyserver und Lastenausgleichsmodule, die häufig wichtigen Anforderungsinformationen verdecken zugrunde liegen.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/proxy-load-balancer
ms.openlocfilehash: b153a7406ae1b31a2aa453135c6bd0e5ce0b2997
ms.sourcegitcommit: d45d766504c2c5aad2453f01f089bc6b696b5576
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2018
---
# <a name="configure-aspnet-core-to-work-with-proxy-servers-and-load-balancers"></a><span data-ttu-id="f24ba-103">Konfigurieren von ASP.NET Core zum Arbeiten mit Proxyservern und Lastenausgleich</span><span class="sxs-lookup"><span data-stu-id="f24ba-103">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>

<span data-ttu-id="f24ba-104">Durch [Luke Latham](https://github.com/guardrex) und [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="f24ba-104">By [Luke Latham](https://github.com/guardrex) and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="f24ba-105">In die empfohlene Konfiguration für ASP.NET Core wird die app mit IIS/ASP.NET Core-Modul, Nginx oder Apache gehostet.</span><span class="sxs-lookup"><span data-stu-id="f24ba-105">In the recommended configuration for ASP.NET Core, the app is hosted using IIS/ASP.NET Core Module, Nginx, or Apache.</span></span> <span data-ttu-id="f24ba-106">Proxy-Servern, Lastenausgleichsmodule und anderen Netzwerkgeräten überaus häufig Informationen zur Anforderung, vor dem Erreichen der app:</span><span class="sxs-lookup"><span data-stu-id="f24ba-106">Proxy servers, load balancers, and other network appliances often obscure information about the request before it reaches the app:</span></span>

* <span data-ttu-id="f24ba-107">Wenn HTTPS-Anforderungen über-HTTP-Proxy ausgeführt werden, wird das ursprüngliche Schema (HTTPS) verloren gegangen ist und in einem Header weitergeleitet werden muss.</span><span class="sxs-lookup"><span data-stu-id="f24ba-107">When HTTPS requests are proxied over HTTP, the original scheme (HTTPS) is lost and must be forwarded in a header.</span></span>
* <span data-ttu-id="f24ba-108">Da eine app über den Proxy und nicht als "true" Quelle im Internet oder Unternehmensnetzwerk eine Anforderung empfängt, muss auch die ursprüngliche Client-IP-Adresse in einem Header weitergeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="f24ba-108">Because an app receives a request from the proxy and not its true source on the Internet or corporate network, the originating client IP address must also be forwarded in a header.</span></span>

<span data-ttu-id="f24ba-109">Diese Informationen können in den Anfrageprozess, z. B. umleitungen, Authentifizierung, linkgenerierung, richtlinienauswertung und Client Geoloation wichtig sein.</span><span class="sxs-lookup"><span data-stu-id="f24ba-109">This information may be important in request processing, for example in redirects, authentication, link generation, policy evaluation, and client geoloation.</span></span>

## <a name="forwarded-headers"></a><span data-ttu-id="f24ba-110">Weitergeleitete Header</span><span class="sxs-lookup"><span data-stu-id="f24ba-110">Forwarded headers</span></span>

<span data-ttu-id="f24ba-111">Gemäß der Konvention weiterleiten Proxys Informationen in den HTTP-Header.</span><span class="sxs-lookup"><span data-stu-id="f24ba-111">By convention, proxies forward information in HTTP headers.</span></span>

| <span data-ttu-id="f24ba-112">Header</span><span class="sxs-lookup"><span data-stu-id="f24ba-112">Header</span></span> | <span data-ttu-id="f24ba-113">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="f24ba-113">Description</span></span> |
| ------ | ----------- |
| <span data-ttu-id="f24ba-114">X – weitergeleitet-für</span><span class="sxs-lookup"><span data-stu-id="f24ba-114">X-Forwarded-For</span></span> | <span data-ttu-id="f24ba-115">Enthält Informationen zu dem Client, der die Anforderung und die nachfolgenden Proxys in einer Kette von Proxys initiiert.</span><span class="sxs-lookup"><span data-stu-id="f24ba-115">Holds information about the client that initiated the request and subsequent proxies in a chain of proxies.</span></span> <span data-ttu-id="f24ba-116">Dieser Parameter kann die IP-Adressen (und optional Portnummern) enthalten.</span><span class="sxs-lookup"><span data-stu-id="f24ba-116">This parameter may contain IP addresses (and, optionally, port numbers).</span></span> <span data-ttu-id="f24ba-117">Der erste Parameter in einer Kette von Proxyservern gibt an, dass der Client, auf dem die Anforderung zuerst gesendet wurde.</span><span class="sxs-lookup"><span data-stu-id="f24ba-117">In a chain of proxy servers, the first parameter indicates the client where the request was first made.</span></span> <span data-ttu-id="f24ba-118">Nachfolgende Proxy entsprechen.</span><span class="sxs-lookup"><span data-stu-id="f24ba-118">Subsequent proxy identifiers follow.</span></span> <span data-ttu-id="f24ba-119">Der letzte Proxy in der Kette ist in der Liste der Parameter nicht.</span><span class="sxs-lookup"><span data-stu-id="f24ba-119">The last proxy in the chain isn't in the list of parameters.</span></span> <span data-ttu-id="f24ba-120">IP-Adresse der letzten Proxys und optional eine Portnummer stehen die remote IP-Adresse auf der Transportschicht.</span><span class="sxs-lookup"><span data-stu-id="f24ba-120">The last proxy's IP address, and optionally a port number, are available as the remote IP address at the transport layer.</span></span> |
| <span data-ttu-id="f24ba-121">X-Forwarded-Proto</span><span class="sxs-lookup"><span data-stu-id="f24ba-121">X-Forwarded-Proto</span></span> | <span data-ttu-id="f24ba-122">Der Wert des ursprünglichen Schemas (HTTP/HTTPS).</span><span class="sxs-lookup"><span data-stu-id="f24ba-122">The value of the originating scheme (HTTP/HTTPS).</span></span> <span data-ttu-id="f24ba-123">Der Wert kann auch eine Liste von Schemas sein, wenn die Anforderung mehrere Proxys durchlaufen hat.</span><span class="sxs-lookup"><span data-stu-id="f24ba-123">The value may also be a list of schemes if the request has traversed multiple proxies.</span></span> |
| <span data-ttu-id="f24ba-124">X-weitergeleitet-Host</span><span class="sxs-lookup"><span data-stu-id="f24ba-124">X-Forwarded-Host</span></span> | <span data-ttu-id="f24ba-125">Der ursprüngliche Wert des Feld "Host-Header".</span><span class="sxs-lookup"><span data-stu-id="f24ba-125">The original value of the Host header field.</span></span> <span data-ttu-id="f24ba-126">In der Regel ändern nicht Proxys den Hostheader.</span><span class="sxs-lookup"><span data-stu-id="f24ba-126">Usually, proxies don't modify the Host header.</span></span> <span data-ttu-id="f24ba-127">Finden Sie unter [Microsoft Security Advisory CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) Informationen zu einer Erhöhung von Berechtigungen-Schwachstelle, die Systems betroffen sind, in dem der Proxy überprüft nicht, oder Restict Hostheader auf bekannte Werte.</span><span class="sxs-lookup"><span data-stu-id="f24ba-127">See [Microsoft Security Advisory CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) for information on an elevation-of-privileges vulnerability that affects systems where the proxy doesn't validate or restict Host headers to known good values.</span></span> |

<span data-ttu-id="f24ba-128">Die Middleware weitergeleitet Header aus der [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) packen, liest diese Header und füllt die zugeordneten Felder im [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span><span class="sxs-lookup"><span data-stu-id="f24ba-128">The Forwarded Headers Middleware, from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package, reads these headers and fills in the associated fields on [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span></span> 

<span data-ttu-id="f24ba-129">Die Middleware-Updates:</span><span class="sxs-lookup"><span data-stu-id="f24ba-129">The middleware updates:</span></span>

* <span data-ttu-id="f24ba-130">[HttpContext.Connection.RemoteIpAddress](/dotnet/api/microsoft.aspnetcore.http.connectioninfo.remoteipaddress) &ndash; Set using the `X-Forwarded-For` header value.</span><span class="sxs-lookup"><span data-stu-id="f24ba-130">[HttpContext.Connection.RemoteIpAddress](/dotnet/api/microsoft.aspnetcore.http.connectioninfo.remoteipaddress) &ndash; Set using the `X-Forwarded-For` header value.</span></span> <span data-ttu-id="f24ba-131">Zusätzliche Einstellungen beeinflussen, wie die Middleware festlegt `RemoteIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="f24ba-131">Additional settings influence how the middleware sets `RemoteIpAddress`.</span></span> <span data-ttu-id="f24ba-132">Einzelheiten finden Sie in der [weitergeleitet Header-middlewareoptionen](#forwarded-headers-middleware-options).</span><span class="sxs-lookup"><span data-stu-id="f24ba-132">For details, see the [Forwarded Headers Middleware options](#forwarded-headers-middleware-options).</span></span>
* <span data-ttu-id="f24ba-133">[HttpContext.Request.Scheme](/dotnet/api/microsoft.aspnetcore.http.httprequest.scheme) &ndash; legen Sie mithilfe der `X-Forwarded-Proto` Headerwert.</span><span class="sxs-lookup"><span data-stu-id="f24ba-133">[HttpContext.Request.Scheme](/dotnet/api/microsoft.aspnetcore.http.httprequest.scheme) &ndash; Set using the `X-Forwarded-Proto` header value.</span></span>
* <span data-ttu-id="f24ba-134">[HttpContext.Request.Host](/dotnet/api/microsoft.aspnetcore.http.httprequest.host) &ndash; legen Sie mithilfe der `X-Forwarded-Host` Headerwert.</span><span class="sxs-lookup"><span data-stu-id="f24ba-134">[HttpContext.Request.Host](/dotnet/api/microsoft.aspnetcore.http.httprequest.host) &ndash; Set using the `X-Forwarded-Host` header value.</span></span>

<span data-ttu-id="f24ba-135">Beachten Sie, die nicht alle Netzwerkgeräte Hinzufügen der `X-Forwarded-For` und `X-Forwarded-Proto` Header ohne zusätzliche Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="f24ba-135">Note that not all network appliances add the `X-Forwarded-For` and `X-Forwarded-Proto` headers without additional configuration.</span></span> <span data-ttu-id="f24ba-136">Wenn der Proxy Anforderungen diese Header nicht enthalten, wenn sie die app erreichen, wenden Sie sich an der Appliance Hersteller Anleitungen.</span><span class="sxs-lookup"><span data-stu-id="f24ba-136">Consult your appliance manufacturer's guidance if the proxied requests don't contain these headers when they reach the app.</span></span>

<span data-ttu-id="f24ba-137">Header-Middleware weitergeleitet [Standardeinstellungen](#forwarded-headers-middleware-options) kann so konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="f24ba-137">Forwarded Headers Middleware [default settings](#forwarded-headers-middleware-options) can be configured.</span></span> <span data-ttu-id="f24ba-138">Die Standardeinstellungen lauten:</span><span class="sxs-lookup"><span data-stu-id="f24ba-138">The default settings are:</span></span>

* <span data-ttu-id="f24ba-139">Es ist nur *Proxy* zwischen der app und die Quelle der Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="f24ba-139">There is only *one proxy* between the app and the source of the requests.</span></span>
* <span data-ttu-id="f24ba-140">Loopback-Adressen sind für bekannte Proxys konfiguriert und Netzwerke bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="f24ba-140">Only loopback addresses are configured for known proxies and known networks.</span></span>

## <a name="iisiis-express-and-aspnet-core-module"></a><span data-ttu-id="f24ba-141">IIS-/IIS Express und ASP.NET Core-Modul</span><span class="sxs-lookup"><span data-stu-id="f24ba-141">IIS/IIS Express and ASP.NET Core Module</span></span>

<span data-ttu-id="f24ba-142">Weitergeleitete Header Middleware ist standardmäßig aktiviert, von IIS Integration Middleware beim Ausführen der app IIS und ASP.NET Core-Modul zugrunde liegen.</span><span class="sxs-lookup"><span data-stu-id="f24ba-142">Forwarded Headers Middleware is enabled by default by IIS Integration Middleware when the app is run behind IIS and the ASP.NET Core Module.</span></span> <span data-ttu-id="f24ba-143">Weitergeleitete Header Middleware wird aktiviert, um auf das ASP.NET Core-Modul Gründen Vertrauensstellung mit weitergeleitete Header in der middlewarepipeline mit einer bestimmten eingeschränkte Konfiguration zuerst ausgeführt werden (z. B. [IP-spoofing](https://www.iplocation.net/ip-spoofing)).</span><span class="sxs-lookup"><span data-stu-id="f24ba-143">Forwarded Headers Middleware is activated to run first in the middleware pipeline with a restricted configuration specific to the ASP.NET Core Module due to trust concerns with forwarded headers (for example, [IP spoofing](https://www.iplocation.net/ip-spoofing)).</span></span> <span data-ttu-id="f24ba-144">Die Middleware wird für die Weiterleitung konfiguriert die `X-Forwarded-For` und `X-Forwarded-Proto` Header beschränkt, die auf einen Proxy für die einzelnen "localhost".</span><span class="sxs-lookup"><span data-stu-id="f24ba-144">The middleware is configured to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers and is restricted to a single localhost proxy.</span></span> <span data-ttu-id="f24ba-145">Wenn zusätzliche Konfiguration erforderlich ist, finden Sie unter der [weitergeleitet Header-middlewareoptionen](#forwarded-headers-middleware-options).</span><span class="sxs-lookup"><span data-stu-id="f24ba-145">If additional configuration is required, see the [Forwarded Headers Middleware options](#forwarded-headers-middleware-options).</span></span>

## <a name="other-proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="f24ba-146">Andere Proxyserver und Load Balancer-Szenarien</span><span class="sxs-lookup"><span data-stu-id="f24ba-146">Other proxy server and load balancer scenarios</span></span>

<span data-ttu-id="f24ba-147">Header-Middleware weitergeleitet wird nicht außerhalb von IIS Integration Middleware verwendet wird, standardmäßig aktiviert.</span><span class="sxs-lookup"><span data-stu-id="f24ba-147">Outside of using IIS Integration Middleware, Forwarded Headers Middleware isn't enabled by default.</span></span> <span data-ttu-id="f24ba-148">Weitergeleitete Header Middleware muss aktiviert sein, damit eine app Prozess weitergeleitet-Header mit [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders).</span><span class="sxs-lookup"><span data-stu-id="f24ba-148">Forwarded Headers Middleware must be enabled for an app to process forwarded headers with [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders).</span></span> <span data-ttu-id="f24ba-149">Nach der Aktivierung der Middleware, wenn kein [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) angegeben werden, um die Middleware, die Standardeinstellung [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) sind [ForwardedHeaders.None ](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span><span class="sxs-lookup"><span data-stu-id="f24ba-149">After enabling the middleware if no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) are [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span></span>

<span data-ttu-id="f24ba-150">Konfigurieren Sie die Middleware mit [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) zum Weiterleiten der `X-Forwarded-For` und `X-Forwarded-Proto` Header in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f24ba-150">Configure the middleware with [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="f24ba-151">Aufrufen der [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) Methode in `Startup.Configure` vor dem Aufruf anderer Middleware:</span><span class="sxs-lookup"><span data-stu-id="f24ba-151">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling other middleware:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    
    services.Configure<ForwardedHeadersOptions>(options =>
    {
        options.ForwardedHeaders = 
            ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto;
    });
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseForwardedHeaders();

    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseStaticFiles();
    // In ASP.NET Core 1.x, replace the following line with: app.UseIdentity();
    app.UseAuthentication();
    app.UseMvc();
}
```

> [!NOTE]
> <span data-ttu-id="f24ba-152">Wenn kein [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) werden in angegeben `Startup.ConfigureServices` oder direkt für die Erweiterungsmethode mit [UseForwardedHeaders (IApplicationBuilder, ForwardedHeadersOptions)](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ForwardedHeadersExtensions_UseForwardedHeaders_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_ForwardedHeadersOptions_), Standard der Header weitergeleitet werden [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span><span class="sxs-lookup"><span data-stu-id="f24ba-152">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified in `Startup.ConfigureServices` or directly to the extension method with [UseForwardedHeaders(IApplicationBuilder, ForwardedHeadersOptions)](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ForwardedHeadersExtensions_UseForwardedHeaders_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_ForwardedHeadersOptions_), the default headers to forward are [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span></span> <span data-ttu-id="f24ba-153">Die [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) Eigenschaft konfiguriert werden muss, mit der Header weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="f24ba-153">The [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) property must be configured with the headers to forward.</span></span>

## <a name="forwarded-headers-middleware-options"></a><span data-ttu-id="f24ba-154">Weitergeleitete Header-middlewareoptionen.</span><span class="sxs-lookup"><span data-stu-id="f24ba-154">Forwarded Headers Middleware options</span></span>

<span data-ttu-id="f24ba-155">[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) steuern das Verhalten der Middleware Header weitergeleitet:</span><span class="sxs-lookup"><span data-stu-id="f24ba-155">[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) control the behavior of the Forwarded Headers Middleware:</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardLimit = 2;
    options.KnownProxies.Add(IPAddress.Parse("127.0.10.1"));
    options.ForwardedForHeaderName = "X-Forwarded-For-Custom-Header-Name";
});
```

| <span data-ttu-id="f24ba-156">Option</span><span class="sxs-lookup"><span data-stu-id="f24ba-156">Option</span></span> | <span data-ttu-id="f24ba-157">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="f24ba-157">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="f24ba-158">ForwardedForHeaderName</span><span class="sxs-lookup"><span data-stu-id="f24ba-158">ForwardedForHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) | <span data-ttu-id="f24ba-159">Verwenden Sie den Header, die von dieser Eigenschaft anstelle der angegebenen [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername).</span><span class="sxs-lookup"><span data-stu-id="f24ba-159">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername).</span></span><br><br><span data-ttu-id="f24ba-160">Die Standardeinstellung ist `X-Forwarded-For`.</span><span class="sxs-lookup"><span data-stu-id="f24ba-160">The default is `X-Forwarded-For`.</span></span> |
| [<span data-ttu-id="f24ba-161">ForwardedHeaders</span><span class="sxs-lookup"><span data-stu-id="f24ba-161">ForwardedHeaders</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) | <span data-ttu-id="f24ba-162">Gibt an, welche Weiterleitungen verarbeitet werden sollen.</span><span class="sxs-lookup"><span data-stu-id="f24ba-162">Identifies which forwarders should be processed.</span></span> <span data-ttu-id="f24ba-163">Finden Sie unter der [ForwardedHeaders Enum](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) für die Liste der Felder, die angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="f24ba-163">See the [ForwardedHeaders Enum](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) for the list of fields that apply.</span></span> <span data-ttu-id="f24ba-164">Typische Werte dieser Eigenschaft zugewiesene sind <code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>.</span><span class="sxs-lookup"><span data-stu-id="f24ba-164">Typical values assigned to this property are <code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>.</span></span><br><br><span data-ttu-id="f24ba-165">Der Standardwert ist [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span><span class="sxs-lookup"><span data-stu-id="f24ba-165">The default value is [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span></span> |
| [<span data-ttu-id="f24ba-166">ForwardedHostHeaderName</span><span class="sxs-lookup"><span data-stu-id="f24ba-166">ForwardedHostHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedhostheadername) | <span data-ttu-id="f24ba-167">Verwenden Sie den Header, die von dieser Eigenschaft anstelle der angegebenen [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername).</span><span class="sxs-lookup"><span data-stu-id="f24ba-167">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername).</span></span><br><br><span data-ttu-id="f24ba-168">Die Standardeinstellung ist `X-Forwarded-Host`.</span><span class="sxs-lookup"><span data-stu-id="f24ba-168">The default is `X-Forwarded-Host`.</span></span> |
| [<span data-ttu-id="f24ba-169">ForwardedProtoHeaderName</span><span class="sxs-lookup"><span data-stu-id="f24ba-169">ForwardedProtoHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) | <span data-ttu-id="f24ba-170">Verwenden Sie den Header, die von dieser Eigenschaft anstelle der angegebenen [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername).</span><span class="sxs-lookup"><span data-stu-id="f24ba-170">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername).</span></span><br><br><span data-ttu-id="f24ba-171">Die Standardeinstellung ist `X-Forwarded-Proto`.</span><span class="sxs-lookup"><span data-stu-id="f24ba-171">The default is `X-Forwarded-Proto`.</span></span> |
| [<span data-ttu-id="f24ba-172">ForwardLimit</span><span class="sxs-lookup"><span data-stu-id="f24ba-172">ForwardLimit</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardlimit) | <span data-ttu-id="f24ba-173">Schränkt die Anzahl der Einträge in den Headern, die verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="f24ba-173">Limits the number of entries in the headers that are processed.</span></span> <span data-ttu-id="f24ba-174">Legen Sie auf `null` So deaktivieren Sie das Limit, aber dies sollte nur erfolgen, wenn `KnownProxies` oder `KnownNetworks` konfiguriert sind.</span><span class="sxs-lookup"><span data-stu-id="f24ba-174">Set to `null` to disable the limit, but this should only be done if `KnownProxies` or `KnownNetworks` are configured.</span></span><br><br><span data-ttu-id="f24ba-175">Der Standard ist 1.</span><span class="sxs-lookup"><span data-stu-id="f24ba-175">The default is 1.</span></span> |
| [<span data-ttu-id="f24ba-176">KnownNetworks</span><span class="sxs-lookup"><span data-stu-id="f24ba-176">KnownNetworks</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownnetworks) | <span data-ttu-id="f24ba-177">Der Adressbereich des bekannten Proxys weitergeleitete Header aus zu akzeptieren.</span><span class="sxs-lookup"><span data-stu-id="f24ba-177">Address ranges of known proxies to accept forwarded headers from.</span></span> <span data-ttu-id="f24ba-178">Geben Sie IP-Adressbereiche, die mithilfe der Notation für klassenloses Interdomain Routing (CIDR) an.</span><span class="sxs-lookup"><span data-stu-id="f24ba-178">Provide IP ranges using Classless Interdomain Routing (CIDR) notation.</span></span><br><br><span data-ttu-id="f24ba-179">Der Standardwert ist eine [IList](/dotnet/api/system.collections.generic.ilist-1)\<[gewöhnlich](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> enthält einen einzelnen Eintrag für `IPAddress.Loopback`.</span><span class="sxs-lookup"><span data-stu-id="f24ba-179">The default is an [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPNetwork](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> containing a single entry for `IPAddress.Loopback`.</span></span> |
| [<span data-ttu-id="f24ba-180">KnownProxies</span><span class="sxs-lookup"><span data-stu-id="f24ba-180">KnownProxies</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownproxies) | <span data-ttu-id="f24ba-181">Die Adressen der bekannten Proxys weitergeleitete Header aus akzeptieren.</span><span class="sxs-lookup"><span data-stu-id="f24ba-181">Addresses of known proxies to accept forwarded headers from.</span></span> <span data-ttu-id="f24ba-182">Verwendung `KnownProxies` entspricht genaue IP-Adresse angeben.</span><span class="sxs-lookup"><span data-stu-id="f24ba-182">Use `KnownProxies` to specify exact IP address matches.</span></span><br><br><span data-ttu-id="f24ba-183">Der Standardwert ist eine [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IP-Adresse](/dotnet/api/system.net.ipaddress)> enthält einen einzelnen Eintrag für `IPAddress.IPv6Loopback`.</span><span class="sxs-lookup"><span data-stu-id="f24ba-183">The default is an [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPAddress](/dotnet/api/system.net.ipaddress)> containing a single entry for `IPAddress.IPv6Loopback`.</span></span> |
| [<span data-ttu-id="f24ba-184">OriginalForHeaderName</span><span class="sxs-lookup"><span data-stu-id="f24ba-184">OriginalForHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalforheadername) | <span data-ttu-id="f24ba-185">Verwenden Sie den Header, die von dieser Eigenschaft anstelle der angegebenen [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername).</span><span class="sxs-lookup"><span data-stu-id="f24ba-185">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername).</span></span><br><br><span data-ttu-id="f24ba-186">Die Standardeinstellung ist `X-Original-For`.</span><span class="sxs-lookup"><span data-stu-id="f24ba-186">The default is `X-Original-For`.</span></span> |
| [<span data-ttu-id="f24ba-187">OriginalHostHeaderName</span><span class="sxs-lookup"><span data-stu-id="f24ba-187">OriginalHostHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalhostheadername) | <span data-ttu-id="f24ba-188">Verwenden Sie den Header, die von dieser Eigenschaft anstelle der angegebenen [ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername).</span><span class="sxs-lookup"><span data-stu-id="f24ba-188">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername).</span></span><br><br><span data-ttu-id="f24ba-189">Die Standardeinstellung ist `X-Original-Host`.</span><span class="sxs-lookup"><span data-stu-id="f24ba-189">The default is `X-Original-Host`.</span></span> |
| [<span data-ttu-id="f24ba-190">OriginalProtoHeaderName</span><span class="sxs-lookup"><span data-stu-id="f24ba-190">OriginalProtoHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalprotoheadername) | <span data-ttu-id="f24ba-191">Verwenden Sie den Header, die von dieser Eigenschaft anstelle der angegebenen [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername).</span><span class="sxs-lookup"><span data-stu-id="f24ba-191">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername).</span></span><br><br><span data-ttu-id="f24ba-192">Die Standardeinstellung ist `X-Original-Proto`.</span><span class="sxs-lookup"><span data-stu-id="f24ba-192">The default is `X-Original-Proto`.</span></span> |
| [<span data-ttu-id="f24ba-193">RequireHeaderSymmetry</span><span class="sxs-lookup"><span data-stu-id="f24ba-193">RequireHeaderSymmetry</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.requireheadersymmetry) | <span data-ttu-id="f24ba-194">Benötigen Sie die Anzahl der Headerwerte in die Synchronisierung zwischen den [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="f24ba-194">Require the number of header values to be in sync between the [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) being processed.</span></span><br><br><span data-ttu-id="f24ba-195">Die Standardeinstellung in ASP.NET Core 1.x ist `true`.</span><span class="sxs-lookup"><span data-stu-id="f24ba-195">The default in ASP.NET Core 1.x is `true`.</span></span> <span data-ttu-id="f24ba-196">Die Standardeinstellung in ASP.NET Core 2.0 oder höher ist `false`.</span><span class="sxs-lookup"><span data-stu-id="f24ba-196">The default in ASP.NET Core 2.0 or later is `false`.</span></span> |

## <a name="scenarios-and-use-cases"></a><span data-ttu-id="f24ba-197">Szenarien und Anwendungsfälle</span><span class="sxs-lookup"><span data-stu-id="f24ba-197">Scenarios and use cases</span></span>

### <a name="when-it-isnt-possible-to-add-forwarded-headers-and-all-requests-are-secure"></a><span data-ttu-id="f24ba-198">Wenn Sie ist nicht möglich, weitergeleitete hinzuzufügen, sind Kopf- und alle Anforderungen sicher</span><span class="sxs-lookup"><span data-stu-id="f24ba-198">When it isn't possible to add forwarded headers and all requests are secure</span></span>

<span data-ttu-id="f24ba-199">In einigen Fällen möglicherweise es nicht möglich, die Anforderungen an die app über einen Proxy weitergeleitete Header hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="f24ba-199">In some cases, it might not be possible to add forwarded headers to the requests proxied to the app.</span></span> <span data-ttu-id="f24ba-200">Wenn der Proxy erzwingt, führt, dass alle Öffentliche externen Anforderungen HTTPS sind, das Schema auch manuell festgelegt werden `Startup.Configure` bevor Sie einen beliebigen Typ von Middleware verwenden:</span><span class="sxs-lookup"><span data-stu-id="f24ba-200">If the proxy is enforcing that all public external requests are HTTPS, the scheme can be manually set in `Startup.Configure` before using any type of middleware:</span></span>

```csharp
app.Use((context, next) =>
{
    context.Request.Scheme = "https";
    return next();
});
```

<span data-ttu-id="f24ba-201">Dieser Code kann mit einer Umgebungsvariablen oder andere-Konfigurationseinstellung in der Entwicklungs- oder Stagingumgebung deaktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="f24ba-201">This code can be disabled with an environment variable or other configuration setting in a development or staging environment.</span></span>

### <a name="deal-with-path-base-and-proxies-that-change-the-request-path"></a><span data-ttu-id="f24ba-202">Umgang mit Pfad Basis und Proxys, die den Anforderungspfad ändern</span><span class="sxs-lookup"><span data-stu-id="f24ba-202">Deal with path base and proxies that change the request path</span></span>

<span data-ttu-id="f24ba-203">Einige Proxys übergeben Sie den Pfad intakt, aber mit einer app Basispfad, der entfernt werden soll, sodass routing funktioniert ordnungsgemäß.</span><span class="sxs-lookup"><span data-stu-id="f24ba-203">Some proxies pass the path intact but with an app base path that should be removed so that routing works properly.</span></span> <span data-ttu-id="f24ba-204">[UsePathBaseExtensions.UsePathBase](/dotnet/api/microsoft.aspnetcore.builder.usepathbaseextensions.usepathbase) Middleware teilt den Pfad in [HttpRequest.Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) und in der app-Basispfad [HttpRequest.PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase).</span><span class="sxs-lookup"><span data-stu-id="f24ba-204">[UsePathBaseExtensions.UsePathBase](/dotnet/api/microsoft.aspnetcore.builder.usepathbaseextensions.usepathbase) middleware splits the path into [HttpRequest.Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) and the app base path into [HttpRequest.PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase).</span></span>

<span data-ttu-id="f24ba-205">Wenn `/foo` ist der app-Basispfad für ein Proxy-Pfad als übergeben `/foo/api/1`, die Middleware Mengen `Request.PathBase` auf `/foo` und `Request.Path` zu `/api/1` mit den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="f24ba-205">If `/foo` is the app base path for a proxy path passed as `/foo/api/1`, the middleware sets `Request.PathBase` to `/foo` and `Request.Path` to `/api/1` with the following command:</span></span>

```csharp
app.UsePathBase("/foo");
```

<span data-ttu-id="f24ba-206">Den ursprünglichen Pfad und den Pfad, die Basis werden erneut angewendet, wenn die Middleware in umgekehrter Reihenfolge erneut aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="f24ba-206">The original path and path base are reapplied when the middleware is called again in reverse.</span></span> <span data-ttu-id="f24ba-207">Weitere Informationen zur Verarbeitung von Middleware Reihenfolge finden Sie unter [Middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="f24ba-207">For more information on middleware order processing, see [Middleware](xref:fundamentals/middleware/index).</span></span>

<span data-ttu-id="f24ba-208">Wenn der Proxy den Pfad entfernt (z. B. Weiterleitung `/foo/api/1` auf `/api/1`), korrigiert leitet und durch Festlegen der Anforderung verknüpft [PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase) Eigenschaft:</span><span class="sxs-lookup"><span data-stu-id="f24ba-208">If the proxy trims the path (for example, forwarding `/foo/api/1` to `/api/1`), fix redirects and links by setting the request's [PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase) property:</span></span>

```csharp
app.Use((context, next) =>
{
    context.Request.PathBase = new PathString("/foo");
    return next();
});
```

<span data-ttu-id="f24ba-209">Wenn der Proxy Pfaddaten hinzugefügt wird, verwerfen Sie Teil des Pfads für die Behebung von umleitungen und Links mit [StartsWithSegments (PathString-Wert, PathString-Wert)](/dotnet/api/microsoft.aspnetcore.http.pathstring.startswithsegments#Microsoft_AspNetCore_Http_PathString_StartsWithSegments_Microsoft_AspNetCore_Http_PathString_Microsoft_AspNetCore_Http_PathString__) und Zuweisen der [Pfad](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) Eigenschaft:</span><span class="sxs-lookup"><span data-stu-id="f24ba-209">If the proxy is adding path data, discard part of the path to fix redirects and links by using [StartsWithSegments(PathString, PathString)](/dotnet/api/microsoft.aspnetcore.http.pathstring.startswithsegments#Microsoft_AspNetCore_Http_PathString_StartsWithSegments_Microsoft_AspNetCore_Http_PathString_Microsoft_AspNetCore_Http_PathString__) and assigning to the [Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) property:</span></span>

```csharp
app.Use((context, next) =>
{
    if (context.Request.Path.StartsWithSegments("/foo", out var remainder))
    {
        context.Request.Path = remainder;
    }

    return next();
});
```

## <a name="troubleshoot"></a><span data-ttu-id="f24ba-210">Problembehandlung</span><span class="sxs-lookup"><span data-stu-id="f24ba-210">Troubleshoot</span></span>

<span data-ttu-id="f24ba-211">Wenn der Header weitergeleitet werden nicht wie erwartet, aktivieren [Protokollierung](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="f24ba-211">When headers aren't forwarded as expected, enable [logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="f24ba-212">Wenn die Protokolle nicht genügend Informationen zur Problembehandlung bereitstellen, die Anforderungsheader, die vom Server empfangenen aufgelistet werden.</span><span class="sxs-lookup"><span data-stu-id="f24ba-212">If the logs don't provide sufficient information to troubleshoot the problem, enumerate the request headers received by the server.</span></span> <span data-ttu-id="f24ba-213">Die Header können in einer app-Antwort, die mithilfe von Inline-Middleware geschrieben werden:</span><span class="sxs-lookup"><span data-stu-id="f24ba-213">The headers can be written to an app response using inline middleware:</span></span>

```csharp
public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory)
{
    app.Run(async (context) =>
    {
        context.Response.ContentType = "text/plain";

        // Request method, scheme, and path
        await context.Response.WriteAsync(
            $"Request Method: {context.Request.Method}{Environment.NewLine}");
        await context.Response.WriteAsync(
            $"Request Scheme: {context.Request.Scheme}{Environment.NewLine}");
        await context.Response.WriteAsync(
            $"Request Path: {context.Request.Path}{Environment.NewLine}");

        // Headers
        await context.Response.WriteAsync($"Request Headers:{Environment.NewLine}");

        foreach (var header in context.Request.Headers)
        {
            await context.Response.WriteAsync($"{header.Key}: " +
                $"{header.Value}{Environment.NewLine}");
        }

        await context.Response.WriteAsync(Environment.NewLine);

        // Connection: RemoteIp
        await context.Response.WriteAsync(
            $"Request RemoteIp: {context.Connection.RemoteIpAddress}");
    }
}
```

<span data-ttu-id="f24ba-214">Stellen Sie sicher, dass das "X" - Weiterleitungsdatenverkehr-\* Header vom Server mit den erwarteten Werten empfangen werden.</span><span class="sxs-lookup"><span data-stu-id="f24ba-214">Ensure that the X-Forwarded-\* headers are received by the server with the expected values.</span></span> <span data-ttu-id="f24ba-215">Wenn mehrere Werte in ein bestimmter Header vorhanden sind, beachten Sie die Header Middleware weitergeleitet Prozesse Header in umgekehrter Reihenfolge von rechts nach links.</span><span class="sxs-lookup"><span data-stu-id="f24ba-215">If there are multiple values in a given header, note Forwarded Headers Middleware processes headers in reverse order from right to left.</span></span>

<span data-ttu-id="f24ba-216">Die Anforderung ursprünglichen remote-IP-übereinstimmen einen Eintrag in der `KnownProxies` oder `KnownNetworks` listet vor X-weitergeleitet – bei der Verarbeitung.</span><span class="sxs-lookup"><span data-stu-id="f24ba-216">The request's original remote IP must match an entry in the `KnownProxies` or `KnownNetworks` lists before X-Forwarded-For is processed.</span></span> <span data-ttu-id="f24ba-217">Dadurch wird der Header von akzeptiert keine Weiterleitungen von nicht vertrauenswürdigen Proxys spoofing beschränkt.</span><span class="sxs-lookup"><span data-stu-id="f24ba-217">This limits header spoofing by not accepting forwarders from untrusted proxies.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f24ba-218">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="f24ba-218">Additional resources</span></span>

* [<span data-ttu-id="f24ba-219">Microsoft Security Advisory CVE-2018-0787: ASP.NET Core Erhöhung des Berechtigungssicherheitsrisikos</span><span class="sxs-lookup"><span data-stu-id="f24ba-219">Microsoft Security Advisory CVE-2018-0787: ASP.NET Core Elevation Of Privilege Vulnerability</span></span>](https://github.com/aspnet/Announcements/issues/295)
