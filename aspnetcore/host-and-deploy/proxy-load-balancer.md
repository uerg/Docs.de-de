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
ms.openlocfilehash: f18a5c518edc739e0fe667f3aef6ffd38c06366c
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/03/2018
---
# <a name="configure-aspnet-core-to-work-with-proxy-servers-and-load-balancers"></a>Konfigurieren von ASP.NET Core zum Arbeiten mit Proxyservern und Lastenausgleich

Durch [Luke Latham](https://github.com/guardrex) und [Chris Ross](https://github.com/Tratcher)

In die empfohlene Konfiguration für ASP.NET Core wird die app mit IIS/ASP.NET Core-Modul, Nginx oder Apache gehostet. Proxy-Servern, Lastenausgleichsmodule und anderen Netzwerkgeräten überaus häufig Informationen zur Anforderung, vor dem Erreichen der app:

* Wenn HTTPS-Anforderungen über-HTTP-Proxy ausgeführt werden, wird das ursprüngliche Schema (HTTPS) verloren gegangen ist und in einem Header weitergeleitet werden muss.
* Da eine app über den Proxy und nicht als "true" Quelle im Internet oder Unternehmensnetzwerk eine Anforderung empfängt, muss auch die ursprüngliche Client-IP-Adresse in einem Header weitergeleitet werden.

Diese Informationen können in den Anfrageprozess, z. B. umleitungen, Authentifizierung, linkgenerierung, richtlinienauswertung und Client Geoloation wichtig sein.

## <a name="forwarded-headers"></a>Weitergeleitete Header

Gemäß der Konvention weiterleiten Proxys Informationen in den HTTP-Header.

| Header | Beschreibung |
| ------ | ----------- |
| X – weitergeleitet-für | Enthält Informationen zu dem Client, der die Anforderung und die nachfolgenden Proxys in einer Kette von Proxys initiiert. Dieser Parameter kann die IP-Adressen (und optional Portnummern) enthalten. Der erste Parameter in einer Kette von Proxyservern gibt an, dass der Client, auf dem die Anforderung zuerst gesendet wurde. Nachfolgende Proxy entsprechen. Der letzte Proxy in der Kette ist in der Liste der Parameter nicht. IP-Adresse der letzten Proxys und optional eine Portnummer stehen die remote IP-Adresse auf der Transportschicht. |
| X weitergeleitet Proto | Der Wert des ursprünglichen Schemas (HTTP/HTTPS). Der Wert kann auch eine Liste von Schemas sein, wenn die Anforderung mehrere Proxys durchlaufen hat. |
| X-weitergeleitet-Host | Der ursprüngliche Wert des Feld "Host-Header". In der Regel ändern nicht Proxys den Hostheader. Finden Sie unter [Microsoft Security Advisory CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) Informationen zu einer Erhöhung von Berechtigungen-Schwachstelle, die Systems betroffen sind, in dem der Proxy überprüft nicht, oder Restict Hostheader auf bekannte Werte. |

Die Middleware weitergeleitet Header aus der [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) packen, liest diese Header und füllt die zugeordneten Felder im [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext). 

Die Middleware-Updates:

* [HttpContext.Connection.RemoteIpAddress](/dotnet/api/microsoft.aspnetcore.http.connectioninfo.remoteipaddress) &ndash; Set using the `X-Forwarded-For` header value. Zusätzliche Einstellungen beeinflussen, wie die Middleware festlegt `RemoteIpAddress`. Einzelheiten finden Sie in der [weitergeleitet Header-middlewareoptionen](#forwarded-headers-middleware-options).
* [HttpContext.Request.Scheme](/dotnet/api/microsoft.aspnetcore.http.httprequest.scheme) &ndash; legen Sie mithilfe der `X-Forwarded-Proto` Headerwert.
* [HttpContext.Request.Host](/dotnet/api/microsoft.aspnetcore.http.httprequest.host) &ndash; legen Sie mithilfe der `X-Forwarded-Host` Headerwert.

Beachten Sie, die nicht alle Netzwerkgeräte Hinzufügen der `X-Forwarded-For` und `X-Forwarded-Proto` Header ohne zusätzliche Konfiguration. Wenn der Proxy Anforderungen diese Header nicht enthalten, wenn sie die app erreichen, wenden Sie sich an der Appliance Hersteller Anleitungen.

Header-Middleware weitergeleitet [Standardeinstellungen](#forwarded-headers-middleware-options) kann so konfiguriert werden. Die Standardeinstellungen lauten:

* Es ist nur *Proxy* zwischen der app und die Quelle der Anforderungen.
* Loopback-Adressen sind für bekannte Proxys konfiguriert und Netzwerke bezeichnet.

## <a name="iisiis-express-and-aspnet-core-module"></a>IIS-/IIS Express und ASP.NET Core-Modul

Weitergeleitete Header Middleware ist standardmäßig aktiviert, von IIS Integration Middleware beim Ausführen der app IIS und ASP.NET Core-Modul zugrunde liegen. Weitergeleitete Header Middleware wird aktiviert, um auf das ASP.NET Core-Modul Gründen Vertrauensstellung mit weitergeleitete Header in der middlewarepipeline mit einer bestimmten eingeschränkte Konfiguration zuerst ausgeführt werden (z. B. [IP-spoofing](https://www.iplocation.net/ip-spoofing)). Die Middleware wird für die Weiterleitung konfiguriert die `X-Forwarded-For` und `X-Forwarded-Proto` Header beschränkt, die auf einen Proxy für die einzelnen "localhost". Wenn zusätzliche Konfiguration erforderlich ist, finden Sie unter der [weitergeleitet Header-middlewareoptionen](#forwarded-headers-middleware-options).

## <a name="other-proxy-server-and-load-balancer-scenarios"></a>Andere Proxyserver und Load Balancer-Szenarien

Header-Middleware weitergeleitet wird nicht außerhalb von IIS Integration Middleware verwendet wird, standardmäßig aktiviert. Weitergeleitete Header Middleware muss aktiviert sein, damit eine app Prozess weitergeleitet-Header mit [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders). Nach der Aktivierung der Middleware, wenn kein [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) angegeben werden, um die Middleware, die Standardeinstellung [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) sind [ForwardedHeaders.None ](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).

Konfigurieren Sie die Middleware mit [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) zum Weiterleiten der `X-Forwarded-For` und `X-Forwarded-Proto` Header in `Startup.ConfigureServices`. Aufrufen der [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) Methode in `Startup.Configure` vor dem Aufruf anderer Middleware:

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
> Wenn kein [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) werden in angegeben `Startup.ConfigureServices` oder direkt für die Erweiterungsmethode mit [UseForwardedHeaders (IApplicationBuilder, ForwardedHeadersOptions)](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ForwardedHeadersExtensions_UseForwardedHeaders_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_ForwardedHeadersOptions_), Standard der Header weitergeleitet werden [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders). Die [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) Eigenschaft konfiguriert werden muss, mit der Header weitergeleitet.

## <a name="forwarded-headers-middleware-options"></a>Weitergeleitete Header-middlewareoptionen.

[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) steuern das Verhalten der Middleware Header weitergeleitet:

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardLimit = 2;
    options.KnownProxies.Add(IPAddress.Parse("127.0.10.1"));
    options.ForwardedForHeaderName = "X-Forwarded-For-Custom-Header-Name";
});
```

::: moniker range="<= aspnetcore-2.0"
| Option | Beschreibung |
| ------ | ----------- |
| [ForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) | Verwenden Sie den Header, die von dieser Eigenschaft anstelle der angegebenen [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername).<br><br>Die Standardeinstellung ist `X-Forwarded-For`. |
| [ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) | Gibt an, welche Weiterleitungen verarbeitet werden sollen. Finden Sie unter der [ForwardedHeaders Enum](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) für die Liste der Felder, die angewendet werden. Typische Werte dieser Eigenschaft zugewiesene sind <code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>.<br><br>Der Standardwert ist [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders). |
| [ForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedhostheadername) | Verwenden Sie den Header, die von dieser Eigenschaft anstelle der angegebenen [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername).<br><br>Die Standardeinstellung ist `X-Forwarded-Host`. |
| [ForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) | Verwenden Sie den Header, die von dieser Eigenschaft anstelle der angegebenen [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername).<br><br>Die Standardeinstellung ist `X-Forwarded-Proto`. |
| [ForwardLimit](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardlimit) | Schränkt die Anzahl der Einträge in den Headern, die verarbeitet werden. Legen Sie auf `null` So deaktivieren Sie das Limit, aber dies sollte nur erfolgen, wenn `KnownProxies` oder `KnownNetworks` konfiguriert sind.<br><br>Der Standard ist 1. |
| [KnownNetworks](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownnetworks) | Der Adressbereich des bekannten Proxys weitergeleitete Header aus zu akzeptieren. Geben Sie IP-Adressbereiche, die mithilfe der Notation für klassenloses Interdomain Routing (CIDR) an.<br><br>Der Standardwert ist eine [IList](/dotnet/api/system.collections.generic.ilist-1)\<[gewöhnlich](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> enthält einen einzelnen Eintrag für `IPAddress.Loopback`. |
| [KnownProxies](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownproxies) | Die Adressen der bekannten Proxys weitergeleitete Header aus akzeptieren. Verwendung `KnownProxies` entspricht genaue IP-Adresse angeben.<br><br>Der Standardwert ist eine [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IP-Adresse](/dotnet/api/system.net.ipaddress)> enthält einen einzelnen Eintrag für `IPAddress.IPv6Loopback`. |
| [OriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalforheadername) | Verwenden Sie den Header, die von dieser Eigenschaft anstelle der angegebenen [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername).<br><br>Die Standardeinstellung ist `X-Original-For`. |
| [OriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalhostheadername) | Verwenden Sie den Header, die von dieser Eigenschaft anstelle der angegebenen [ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername).<br><br>Die Standardeinstellung ist `X-Original-Host`. |
| [OriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalprotoheadername) | Verwenden Sie den Header, die von dieser Eigenschaft anstelle der angegebenen [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername).<br><br>Die Standardeinstellung ist `X-Original-Proto`. |
| [RequireHeaderSymmetry](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.requireheadersymmetry) | Benötigen Sie die Anzahl der Headerwerte in die Synchronisierung zwischen den [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) verarbeitet werden.<br><br>Die Standardeinstellung in ASP.NET Core 1.x ist `true`. Die Standardeinstellung in ASP.NET Core 2.0 oder höher ist `false`. |
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
| Option | Beschreibung |
| ------ | ----------- |
| AllowedHosts | Schränkt die wenigsten wichtige Hosts nach dem `X-Forwarded-Host` -Header auf den angegebenen Werten.<ul><li>Werte werden verglichen mit der Ordnungszahl--Groß-/Kleinschreibung ignorieren.</li><li>Portnummern müssen ausgeschlossen werden.</li><li>Wenn die Liste leer ist, sind alle Hosts zulässig.</li><li>Ein auf oberster Ebene Platzhalter `*` können alle nicht leeren Hosts.</li><li>Unterdomäne Platzhalter sind zulässig, aber die Stammdomäne stimmen nicht überein. Beispielsweise `*.contoso.com` entspricht die Unterdomäne `foo.contoso.com` jedoch nicht in der Stammdomäne `contoso.com`.</li><li>Unicode-Hostnamen sind zulässig, jedoch werden in konvertiert [Punycode](https://tools.ietf.org/html/rfc3492) für den Abgleich.</li><li>[IPv6-Adressen](https://tools.ietf.org/html/rfc4291) müssen umgebende Klammern enthalten und werden [herkömmliche Form](https://tools.ietf.org/html/rfc4291#section-2.2) (z. B. `[ABCD:EF01:2345:6789:ABCD:EF01:2345:6789]`). IPv6-Adressen sind nicht einen Sonderfall logische Gleichheit zwischen verschiedenen Formaten überprüfen, und keine Kanonisierung erfolgt.</li><li>Fehler beim Einschränken der zulässigen Hosts kann ein Angreifer zum Fälschen von Links, die vom Dienst erstellt wird.</li></ul>Der Standardwert ist eine leere [IList\<Zeichenfolge >](/dotnet/api/system.collections.generic.ilist-1). |
| [ForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) | Verwenden Sie den Header, die von dieser Eigenschaft anstelle der angegebenen [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername).<br><br>Die Standardeinstellung ist `X-Forwarded-For`. |
| [ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) | Gibt an, welche Weiterleitungen verarbeitet werden sollen. Finden Sie unter der [ForwardedHeaders Enum](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) für die Liste der Felder, die angewendet werden. Typische Werte dieser Eigenschaft zugewiesene sind <code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>.<br><br>Der Standardwert ist [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders). |
| [ForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedhostheadername) | Verwenden Sie den Header, die von dieser Eigenschaft anstelle der angegebenen [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername).<br><br>Die Standardeinstellung ist `X-Forwarded-Host`. |
| [ForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) | Verwenden Sie den Header, die von dieser Eigenschaft anstelle der angegebenen [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername).<br><br>Die Standardeinstellung ist `X-Forwarded-Proto`. |
| [ForwardLimit](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardlimit) | Schränkt die Anzahl der Einträge in den Headern, die verarbeitet werden. Legen Sie auf `null` So deaktivieren Sie das Limit, aber dies sollte nur erfolgen, wenn `KnownProxies` oder `KnownNetworks` konfiguriert sind.<br><br>Der Standard ist 1. |
| [KnownNetworks](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownnetworks) | Der Adressbereich des bekannten Proxys weitergeleitete Header aus zu akzeptieren. Geben Sie IP-Adressbereiche, die mithilfe der Notation für klassenloses Interdomain Routing (CIDR) an.<br><br>Der Standardwert ist eine [IList](/dotnet/api/system.collections.generic.ilist-1)\<[gewöhnlich](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> enthält einen einzelnen Eintrag für `IPAddress.Loopback`. |
| [KnownProxies](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownproxies) | Die Adressen der bekannten Proxys weitergeleitete Header aus akzeptieren. Verwendung `KnownProxies` entspricht genaue IP-Adresse angeben.<br><br>Der Standardwert ist eine [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IP-Adresse](/dotnet/api/system.net.ipaddress)> enthält einen einzelnen Eintrag für `IPAddress.IPv6Loopback`. |
| [OriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalforheadername) | Verwenden Sie den Header, die von dieser Eigenschaft anstelle der angegebenen [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername).<br><br>Die Standardeinstellung ist `X-Original-For`. |
| [OriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalhostheadername) | Verwenden Sie den Header, die von dieser Eigenschaft anstelle der angegebenen [ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername).<br><br>Die Standardeinstellung ist `X-Original-Host`. |
| [OriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalprotoheadername) | Verwenden Sie den Header, die von dieser Eigenschaft anstelle der angegebenen [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername).<br><br>Die Standardeinstellung ist `X-Original-Proto`. |
| [RequireHeaderSymmetry](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.requireheadersymmetry) | Benötigen Sie die Anzahl der Headerwerte in die Synchronisierung zwischen den [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) verarbeitet werden.<br><br>Die Standardeinstellung in ASP.NET Core 1.x ist `true`. Die Standardeinstellung in ASP.NET Core 2.0 oder höher ist `false`. |
::: moniker-end

## <a name="scenarios-and-use-cases"></a>Szenarien und Anwendungsfälle

### <a name="when-it-isnt-possible-to-add-forwarded-headers-and-all-requests-are-secure"></a>Wenn Sie ist nicht möglich, weitergeleitete hinzuzufügen, sind Kopf- und alle Anforderungen sicher

In einigen Fällen möglicherweise es nicht möglich, die Anforderungen an die app über einen Proxy weitergeleitete Header hinzuzufügen. Wenn der Proxy erzwingt, führt, dass alle Öffentliche externen Anforderungen HTTPS sind, das Schema auch manuell festgelegt werden `Startup.Configure` bevor Sie einen beliebigen Typ von Middleware verwenden:

```csharp
app.Use((context, next) =>
{
    context.Request.Scheme = "https";
    return next();
});
```

Dieser Code kann mit einer Umgebungsvariablen oder andere-Konfigurationseinstellung in der Entwicklungs- oder Stagingumgebung deaktiviert werden.

### <a name="deal-with-path-base-and-proxies-that-change-the-request-path"></a>Umgang mit Pfad Basis und Proxys, die den Anforderungspfad ändern

Einige Proxys übergeben Sie den Pfad intakt, aber mit einer app Basispfad, der entfernt werden soll, sodass routing funktioniert ordnungsgemäß. [UsePathBaseExtensions.UsePathBase](/dotnet/api/microsoft.aspnetcore.builder.usepathbaseextensions.usepathbase) Middleware teilt den Pfad in [HttpRequest.Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) und in der app-Basispfad [HttpRequest.PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase).

Wenn `/foo` ist der app-Basispfad für ein Proxy-Pfad als übergeben `/foo/api/1`, die Middleware Mengen `Request.PathBase` auf `/foo` und `Request.Path` zu `/api/1` mit den folgenden Befehl aus:

```csharp
app.UsePathBase("/foo");
```

Den ursprünglichen Pfad und den Pfad, die Basis werden erneut angewendet, wenn die Middleware in umgekehrter Reihenfolge erneut aufgerufen wird. Weitere Informationen zur Verarbeitung von Middleware Reihenfolge finden Sie unter [Middleware](xref:fundamentals/middleware/index).

Wenn der Proxy den Pfad entfernt (z. B. Weiterleitung `/foo/api/1` auf `/api/1`), korrigiert leitet und durch Festlegen der Anforderung verknüpft [PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase) Eigenschaft:

```csharp
app.Use((context, next) =>
{
    context.Request.PathBase = new PathString("/foo");
    return next();
});
```

Wenn der Proxy Pfaddaten hinzugefügt wird, verwerfen Sie Teil des Pfads für die Behebung von umleitungen und Links mit [StartsWithSegments (PathString-Wert, PathString-Wert)](/dotnet/api/microsoft.aspnetcore.http.pathstring.startswithsegments#Microsoft_AspNetCore_Http_PathString_StartsWithSegments_Microsoft_AspNetCore_Http_PathString_Microsoft_AspNetCore_Http_PathString__) und Zuweisen der [Pfad](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) Eigenschaft:

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

## <a name="troubleshoot"></a>Problembehandlung

Wenn der Header weitergeleitet werden nicht wie erwartet, aktivieren [Protokollierung](xref:fundamentals/logging/index). Wenn die Protokolle nicht genügend Informationen zur Problembehandlung bereitstellen, die Anforderungsheader, die vom Server empfangenen aufgelistet werden. Die Header können in einer app-Antwort, die mithilfe von Inline-Middleware geschrieben werden:

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

Stellen Sie sicher, dass das "X" - Weiterleitungsdatenverkehr-* Header vom Server mit den erwarteten Werten empfangen werden. Wenn mehrere Werte in ein bestimmter Header vorhanden sind, beachten Sie die Header Middleware weitergeleitet Prozesse Header in umgekehrter Reihenfolge von rechts nach links.

Die Anforderung ursprünglichen remote-IP-übereinstimmen einen Eintrag in der `KnownProxies` oder `KnownNetworks` listet vor X-weitergeleitet – bei der Verarbeitung. Dadurch wird der Header von akzeptiert keine Weiterleitungen von nicht vertrauenswürdigen Proxys spoofing beschränkt.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Microsoft Security Advisory CVE-2018-0787: ASP.NET Core Erhöhung des Berechtigungssicherheitsrisikos](https://github.com/aspnet/Announcements/issues/295)
