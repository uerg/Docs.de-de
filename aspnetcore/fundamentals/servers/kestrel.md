---
title: Implementierung des Webservers Kestrel in ASP.NET Core
author: tdykstra
description: "Einführung in Kestrel, der plattformübergreifende Webserver für ASP.NET Core, der auf libuv basiert."
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/kestrel
ms.openlocfilehash: bfe7644891296c7c3485c9a870d90951ba87e9e8
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="f0dde-103">Einführung in die Implementierung des Webservers Kestrel in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f0dde-103">Introduction to Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="f0dde-104">Von [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher) und [Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="f0dde-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="f0dde-105">Kestrel ist ein plattformübergreifender [Webserver für ASP.NET Core](index.md), der auf der plattformübergreifenden asynchronen E/A-Bibliothek [libuv](https://github.com/libuv/libuv) basiert.</span><span class="sxs-lookup"><span data-stu-id="f0dde-105">Kestrel is a cross-platform [web server for ASP.NET Core](index.md) based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span> <span data-ttu-id="f0dde-106">Kestrel ist der Webserver, der standardmäßig in ASP.NET Core-Projektvorlagen enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="f0dde-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span> 

<span data-ttu-id="f0dde-107">Kestrel unterstützt die folgenden Features:</span><span class="sxs-lookup"><span data-stu-id="f0dde-107">Kestrel supports the following features:</span></span>

  * <span data-ttu-id="f0dde-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="f0dde-108">HTTPS</span></span>
  * <span data-ttu-id="f0dde-109">Nicht transparente Upgrades, die zum Aktivieren von [WebSockets](https://github.com/aspnet/websockets) verwendet werden</span><span class="sxs-lookup"><span data-stu-id="f0dde-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
  * <span data-ttu-id="f0dde-110">Unix-Sockets für eine hohe Leistung im Hintergrund von Nginx</span><span class="sxs-lookup"><span data-stu-id="f0dde-110">Unix sockets for high performance behind Nginx</span></span> 

<span data-ttu-id="f0dde-111">Kestrel wird auf allen Plattformen und für alle Versionen unterstützt, die .NET Core unterstützt.</span><span class="sxs-lookup"><span data-stu-id="f0dde-111">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f0dde-112">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f0dde-112">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f0dde-113">[Anzeigen oder Herunterladen von Beispielcode für 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f0dde-113">[View or download sample code for 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f0dde-114">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f0dde-114">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f0dde-115">[Anzeigen oder Herunterladen von Beispielcode für 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f0dde-115">[View or download sample code for 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="f0dde-116">Verwenden von Kestrel mit einem Reverseproxy</span><span class="sxs-lookup"><span data-stu-id="f0dde-116">When to use Kestrel with a reverse proxy</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f0dde-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f0dde-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f0dde-118">Sie können Kestrel als eigenständigen Webserver oder mit einem *Reverseproxyserver* wie IIS, Nginx oder Apache verwenden.</span><span class="sxs-lookup"><span data-stu-id="f0dde-118">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="f0dde-119">Ein Reverseproxyserver empfängt HTTP-Anforderungen aus dem Internet und leitet diese nach einer vorbereitenden Verarbeitung an Kestrel weiter.</span><span class="sxs-lookup"><span data-stu-id="f0dde-119">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel kommuniziert direkt und ohne Reverseproxyserver mit dem Internet](kestrel/_static/kestrel-to-internet2.png)

![Kestrel kommuniziert indirekt mit dem Internet über einen Reverseproxyserver wie IIS, Nginx oder Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="f0dde-122">Jede der beiden Konfigurationen &mdash;, egal ob mit oder ohne Reverseproxyserver &mdash;, kann auch dann verwendet werden, wenn Kestrel nur in einem internen Netzwerk verfügbar gemacht wird.</span><span class="sxs-lookup"><span data-stu-id="f0dde-122">Either configuration &mdash; with or without a reverse proxy server &mdash; can also be used if Kestrel is exposed only to an internal network.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f0dde-123">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f0dde-123">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f0dde-124">Wenn Ihre Anwendung nur Anforderungen aus einem internen Netzwerk akzeptiert, können Sie Kestrel als eigenständigen Webserver verwenden.</span><span class="sxs-lookup"><span data-stu-id="f0dde-124">If your application accepts requests only from an internal network, you can use Kestrel by itself.</span></span>

![Kestrel kommuniziert direkt mit Ihrem internen Netzwerk](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="f0dde-126">Wenn Sie Ihre Anwendung im Internet verfügbar machen, müssen Sie IIS, Nginx oder Apache als *Reverseproxyserver* verwenden.</span><span class="sxs-lookup"><span data-stu-id="f0dde-126">If you expose your application to the Internet, you must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="f0dde-127">Ein Reverseproxyserver empfängt HTTP-Anforderungen aus dem Internet und leitet diese nach einer vorbereitenden Verarbeitung an Kestrel weiter.</span><span class="sxs-lookup"><span data-stu-id="f0dde-127">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel kommuniziert indirekt mit dem Internet über einen Reverseproxyserver wie IIS, Nginx oder Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="f0dde-129">Ein Reverseproxy ist aus Sicherheitsgründen für Edge-Bereitstellungen erforderlich, die Datenverkehr aus dem Internet ausgesetzt sind.</span><span class="sxs-lookup"><span data-stu-id="f0dde-129">A reverse proxy is required for edge deployments (exposed to traffic from the Internet) for security reasons.</span></span> <span data-ttu-id="f0dde-130">Die Kestrel-Versionen 1.x besitzen keine umfassenden Schutzmaßnahmen gegenüber Angriffen.</span><span class="sxs-lookup"><span data-stu-id="f0dde-130">The 1.x versions of Kestrel don't have a full complement of defenses against attacks.</span></span> <span data-ttu-id="f0dde-131">Geeignete Timeouts und Größenlimits sowie eine Beschränkung der Anzahl gleichzeitiger Verbindungen sind beispielsweise nicht vorhanden.</span><span class="sxs-lookup"><span data-stu-id="f0dde-131">This includes but isn't limited to appropriate timeouts, size limits, and concurrent connection limits.</span></span>

---

<span data-ttu-id="f0dde-132">Wenn mehrere Anwendungen auf einem einzigen Server ausgeführt werden, die über die gleiche IP und den gleichen Port verfügen, ist ein Reverseproxy erforderlich.</span><span class="sxs-lookup"><span data-stu-id="f0dde-132">A scenario that requires a reverse proxy is when you have multiple applications that share the same IP and port running on a single server.</span></span> <span data-ttu-id="f0dde-133">Dieses Szenario kann nicht direkt mit Kestrel ausgeführt werden, da Kestrel das Freigeben der gleichen IP und des gleichen Ports für mehrere Prozesse nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="f0dde-133">That doesn't work with Kestrel directly because Kestrel doesn't support sharing the same IP and port between multiple processes.</span></span> <span data-ttu-id="f0dde-134">Wenn Sie Kestrel dafür konfigurieren, einem Port zu lauschen, wird der gesamte Datenverkehr dieses Ports unabhängig vom Hostheader verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="f0dde-134">When you configure Kestrel to listen on a port, it handles all traffic for that port regardless of host header.</span></span> <span data-ttu-id="f0dde-135">Ein Reverseproxy, der Ports freigeben kann, muss eine eindeutige IP und einen eindeutigen Port an Kestrel weiterleiten.</span><span class="sxs-lookup"><span data-stu-id="f0dde-135">A reverse proxy that can share ports must then forward to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="f0dde-136">Auch wenn kein Reverseproxyserver erforderlich ist, kann die Verwendung eines solchen aus anderen Gründen empfehlenswert sein:</span><span class="sxs-lookup"><span data-stu-id="f0dde-136">Even if a reverse proxy server isn't required, using one might be a good choice for other reasons:</span></span>

* <span data-ttu-id="f0dde-137">Er kann die verfügbar gemachte Oberfläche einschränken.</span><span class="sxs-lookup"><span data-stu-id="f0dde-137">It can limit your exposed surface area.</span></span>
* <span data-ttu-id="f0dde-138">Er stellt eine optionale zusätzliche Ebene für Konfiguration und Schutz bereit.</span><span class="sxs-lookup"><span data-stu-id="f0dde-138">It provides an optional additional layer of configuration and defense.</span></span>
* <span data-ttu-id="f0dde-139">Er kann sich besser in die vorhandene Infrastruktur integrieren.</span><span class="sxs-lookup"><span data-stu-id="f0dde-139">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="f0dde-140">Er vereinfacht den Lastenausgleich und das Setup von SSL.</span><span class="sxs-lookup"><span data-stu-id="f0dde-140">It simplifies load balancing and SSL set-up.</span></span> <span data-ttu-id="f0dde-141">Nur der Reverseproxyserver erfordert ein SSL-Zertifikat. Dieser Server kann mithilfe von HTTP mit Ihren Anwendungsservern auf dem internen Netzwerk kommunizieren.</span><span class="sxs-lookup"><span data-stu-id="f0dde-141">Only your reverse proxy server requires an SSL certificate, and that server can communicate with your application servers on the internal network using plain HTTP.</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="f0dde-142">Verwenden von Kestrel in ASP.NET Core-Apps</span><span class="sxs-lookup"><span data-stu-id="f0dde-142">How to use Kestrel in ASP.NET Core apps</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f0dde-143">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f0dde-143">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f0dde-144">Das Paket [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) ist im Metapaket [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) enthalten.</span><span class="sxs-lookup"><span data-stu-id="f0dde-144">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="f0dde-145">ASP.NET Core-Projektvorlagen verwenden Kestrel standardmäßig.</span><span class="sxs-lookup"><span data-stu-id="f0dde-145">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="f0dde-146">Der Vorlagencode in *Program.cs* ruft `CreateDefaultBuilder` auf, wodurch [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) im Hintergrund aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="f0dde-146">In *Program.cs*, the template code calls `CreateDefaultBuilder`, which calls [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) behind the scenes.</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="f0dde-147">Wenn Sie die Kestrel-Optionen konfigurieren müssen, rufen Sie wie im folgenden Beispiel dargestellt `UseKestrel` in *Program.cs* auf:</span><span class="sxs-lookup"><span data-stu-id="f0dde-147">If you need to configure Kestrel options, call `UseKestrel` in *Program.cs* as shown in the following example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f0dde-148">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f0dde-148">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f0dde-149">Installieren Sie das NuGet-Paket [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/).</span><span class="sxs-lookup"><span data-stu-id="f0dde-149">Install the [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet package.</span></span>

<span data-ttu-id="f0dde-150">Rufen Sie die Erweiterungsmethode [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) unter `WebHostBuilder` in Ihrer `Main`-Methode auf, und geben Sie dabei wie im folgenden Abschnitt dargestellt alle erforderlichen [Kestrel-Optionen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) an.</span><span class="sxs-lookup"><span data-stu-id="f0dde-150">Call the [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) extension method on `WebHostBuilder` in your `Main` method, specifying any [Kestrel options](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) that you need, as shown in the next section.</span></span>

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a><span data-ttu-id="f0dde-151">Kestrel-Optionen</span><span class="sxs-lookup"><span data-stu-id="f0dde-151">Kestrel options</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f0dde-152">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f0dde-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f0dde-153">Der Kestrel-Webserver verfügt über einschränkende Konfigurationsoptionen, die besonders nützlich bei Bereitstellungen mit Internetzugriff sind.</span><span class="sxs-lookup"><span data-stu-id="f0dde-153">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span> <span data-ttu-id="f0dde-154">Sie können folgende Grenzwerte festlegen:</span><span class="sxs-lookup"><span data-stu-id="f0dde-154">Here are some of the limits you can set:</span></span>

- <span data-ttu-id="f0dde-155">Maximale Anzahl der Clientverbindungen</span><span class="sxs-lookup"><span data-stu-id="f0dde-155">Maximum client connections</span></span>
- <span data-ttu-id="f0dde-156">Maximale Größe des Anforderungstexts</span><span class="sxs-lookup"><span data-stu-id="f0dde-156">Maximum request body size</span></span>
- <span data-ttu-id="f0dde-157">Minimale Datenrate des Anforderungstexts</span><span class="sxs-lookup"><span data-stu-id="f0dde-157">Minimum request body data rate</span></span>

<span data-ttu-id="f0dde-158">Sie können diese und andere Einschränkungen in der `Limits`-Eigenschaft der [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)-Klasse festlegen.</span><span class="sxs-lookup"><span data-stu-id="f0dde-158">You set these constraints and others in the `Limits` property of the [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) class.</span></span> <span data-ttu-id="f0dde-159">Die `Limits`-Eigenschaft enthält eine Instanz der [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)-Klasse.</span><span class="sxs-lookup"><span data-stu-id="f0dde-159">The `Limits` property holds an instance of the [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) class.</span></span> 

<span data-ttu-id="f0dde-160">**Maximale Anzahl von Clientverbindungen**</span><span class="sxs-lookup"><span data-stu-id="f0dde-160">**Maximum client connections**</span></span>

<span data-ttu-id="f0dde-161">Die maximale Anzahl von gleichzeitig geöffneten TCP-Verbindungen kann mithilfe von folgendem Code für die gesamte Anwendung festgelegt werden:</span><span class="sxs-lookup"><span data-stu-id="f0dde-161">The maximum number of concurrent open TCP connections can be set for the entire application with the following code:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="f0dde-162">Es gibt einen separaten Grenzwert für Verbindungen, die von HTTP oder HTTPS auf ein anderes Protokoll aktualisiert wurden (z.B. auf eine WebSockets-Anforderung).</span><span class="sxs-lookup"><span data-stu-id="f0dde-162">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="f0dde-163">Nachdem eine Verbindung aktualisiert wurde, zählt diese nicht mehr für den `MaxConcurrentConnections`-Grenzwert.</span><span class="sxs-lookup"><span data-stu-id="f0dde-163">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span> 

<span data-ttu-id="f0dde-164">Die maximale Anzahl von Verbindungen ist standardmäßig nicht begrenzt (NULL).</span><span class="sxs-lookup"><span data-stu-id="f0dde-164">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="f0dde-165">**Maximale Größe des Anforderungstexts**</span><span class="sxs-lookup"><span data-stu-id="f0dde-165">**Maximum request body size**</span></span>

<span data-ttu-id="f0dde-166">Die maximale Größe des Anforderungstexts beträgt standardmäßig 30.000.000 Byte, also ungefähr 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="f0dde-166">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span> 

<span data-ttu-id="f0dde-167">Die empfohlene Methode zur Außerkraftsetzung des Grenzwerts in einer ASP.NET Core-MVC-App besteht im Verwenden des [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs)-Attributs in einer Aktionsmethode:</span><span class="sxs-lookup"><span data-stu-id="f0dde-167">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="f0dde-168">Im folgenden Beispiel wird veranschaulicht, wie die Einschränkung, die für die gesamte Anwendung und sämltiche Anforderungen gültig ist, konfiguriert wird.</span><span class="sxs-lookup"><span data-stu-id="f0dde-168">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="f0dde-169">Sie können diese Einstellung für eine bestimmte Anforderung in Middleware außer Kraft setzen:</span><span class="sxs-lookup"><span data-stu-id="f0dde-169">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
<span data-ttu-id="f0dde-170">Eine Ausnahme wird ausgelöst, wenn Sie versuchen, den Grenzwert einer Anforderung zu konfigurieren, nachdem die Anwendung bereits mit dem Lesen der Anforderung begonnen hat.</span><span class="sxs-lookup"><span data-stu-id="f0dde-170">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="f0dde-171">Es gibt eine `IsReadOnly`-Eigenschaft, die Ihnen mitteilt, wenn sich die `MaxRequestBodySize`-Eigenschaft im schreibgeschützten Zustand befindet, also wenn der Grenzwert nicht mehr konfiguriert werden kann.</span><span class="sxs-lookup"><span data-stu-id="f0dde-171">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="f0dde-172">**Minimale Datenrate des Anforderungstexts**</span><span class="sxs-lookup"><span data-stu-id="f0dde-172">**Minimum request body data rate**</span></span>

<span data-ttu-id="f0dde-173">Kestrel überprüft sekündlich, ob Daten mit der angegebenen Rate in Bytes/Sekunde eingehen.</span><span class="sxs-lookup"><span data-stu-id="f0dde-173">Kestrel checks every second if data is coming in at the specified rate in bytes/second.</span></span> <span data-ttu-id="f0dde-174">Wenn die Rate den mindestens erforderlichen Wert unterschreitet, wird für die Verbindung wegen Timeout getrennt. Bei der Toleranzperiode handelt es sich um die Zeitspanne, die Kestrel dem Client gewährt, um die Senderate auf den mindestens erforderlichen Wert zu erhöhen. Die Rate wird währenddessen nicht überprüft.</span><span class="sxs-lookup"><span data-stu-id="f0dde-174">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="f0dde-175">Diese Toleranzperiode beugt dem Trennen von Verbindungen vor, die Daten aufgrund eines langsamen TCP-Starts anfänglich mit einer niedrigen Rate senden.</span><span class="sxs-lookup"><span data-stu-id="f0dde-175">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="f0dde-176">Die mindestens erforderliche Rate beträgt standardmäßig 240 Bytes/Sekunde mit einer Toleranzperiode von 5 Sekunden.</span><span class="sxs-lookup"><span data-stu-id="f0dde-176">The default minimum rate is 240 bytes/second, with a 5 second grace period.</span></span>

<span data-ttu-id="f0dde-177">Für die Antwort gilt ebenfalls eine mindestens erforderliche Rate.</span><span class="sxs-lookup"><span data-stu-id="f0dde-177">A minimum rate also applies to the response.</span></span> <span data-ttu-id="f0dde-178">Der Code zum Festlegen des Grenzwerts für Anforderung und Antwort ist abgesehen von `RequestBody` oder `Response` in den Namen von Eigenschaften und Schnittstelle identisch.</span><span class="sxs-lookup"><span data-stu-id="f0dde-178">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span> 

<span data-ttu-id="f0dde-179">In diesem Beispiel wird veranschaulicht, wie Sie die mindestens erforderlichen Datenraten in *Program.cs* konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="f0dde-179">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="f0dde-180">Sie können die Raten pro Anforderung in Middleware konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="f0dde-180">You can configure the rates per request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

<span data-ttu-id="f0dde-181">Weitere Informationen zu anderen Kestrel-Optionen finden Sie in folgenden Klassen:</span><span class="sxs-lookup"><span data-stu-id="f0dde-181">For information about other Kestrel options, see the following classes:</span></span>

* [<span data-ttu-id="f0dde-182">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="f0dde-182">KestrelServerOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [<span data-ttu-id="f0dde-183">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="f0dde-183">KestrelServerLimits</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [<span data-ttu-id="f0dde-184">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="f0dde-184">ListenOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f0dde-185">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f0dde-185">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f0dde-186">Weitere Informationen zu Kestrel-Optionen finden Sie unter [KestrelServerOptions class (KestrelServerOptions-Klasse)](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span><span class="sxs-lookup"><span data-stu-id="f0dde-186">For information about Kestrel options, see [KestrelServerOptions class](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span></span>

---

### <a name="endpoint-configuration"></a><span data-ttu-id="f0dde-187">Endpunktkonfiguration</span><span class="sxs-lookup"><span data-stu-id="f0dde-187">Endpoint configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f0dde-188">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f0dde-188">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f0dde-189">Standardmäßig ist ASP.NET Core an `http://localhost:5000` gebunden.</span><span class="sxs-lookup"><span data-stu-id="f0dde-189">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="f0dde-190">Sie können URL-Präfixe und Ports konfigurieren, denen Kestrel lauschen soll, indem Sie die `Listen`- oder `ListenUnixSocket`-Methode unter `KestrelServerOptions` aufrufen.</span><span class="sxs-lookup"><span data-stu-id="f0dde-190">You configure URL prefixes and ports for Kestrel to listen on by calling `Listen` or `ListenUnixSocket` methods on `KestrelServerOptions`.</span></span> <span data-ttu-id="f0dde-191">(`UseUrls`, das Befehlszeilenargument `urls` und die Umgebungsvariable „ASPNETCORE_URLS“ funktionieren ebenfalls, verfügen jedoch über Einschränkungen, die [im Verlauf dieses Artikels](#useurls-limitations) erläutert werden.)</span><span class="sxs-lookup"><span data-stu-id="f0dde-191">(`UseUrls`, the `urls` command-line argument, and the ASPNETCORE_URLS environment variable also work but have the limitations noted [later in this article](#useurls-limitations).)</span></span>

<span data-ttu-id="f0dde-192">**Binden an einen TCP-Socket**</span><span class="sxs-lookup"><span data-stu-id="f0dde-192">**Bind to a TCP socket**</span></span>

<span data-ttu-id="f0dde-193">Durch die `Listen`-Methode können Sie eine Bindung an einen TCP-Socket erstellen, und durch den Lambdaausdruck einer Option können Sie ein SSL-Zertifikat konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="f0dde-193">The `Listen` method binds to a TCP socket, and an options lambda lets you configure an SSL certificate:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

<span data-ttu-id="f0dde-194">Beachten Sie, wie SSL in diesem Beispiel mithilfe von [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs) für einen bestimmten Endpunkt konfiguriert wird.</span><span class="sxs-lookup"><span data-stu-id="f0dde-194">Notice how this example configures SSL for a particular endpoint by using [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span></span> <span data-ttu-id="f0dde-195">Sie können die gleiche API verwenden, um andere Kestrel-Einstellungen für bestimmte Endpunkte zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="f0dde-195">You can use the same API to configure other Kestrel settings for particular endpoints.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

<span data-ttu-id="f0dde-196">**Binden an einen Unix-Socket**</span><span class="sxs-lookup"><span data-stu-id="f0dde-196">**Bind to a Unix socket**</span></span>

<span data-ttu-id="f0dde-197">Wie in diesem Beispiel dargestellt können Sie einem Unix-Socket lauschen, um eine verbesserte Leistung mit Nginx zu erzielen:</span><span class="sxs-lookup"><span data-stu-id="f0dde-197">You can listen on a Unix socket for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

<span data-ttu-id="f0dde-198">**Port 0**</span><span class="sxs-lookup"><span data-stu-id="f0dde-198">**Port 0**</span></span>

<span data-ttu-id="f0dde-199">Wenn Sie die Portnummer 0 angeben, erstellt Kestrel eine dynamische Bindung an einen verfügbaren Port.</span><span class="sxs-lookup"><span data-stu-id="f0dde-199">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="f0dde-200">Das folgende Beispiel veranschaulicht, wie bestimmt werden kann, für welchen Port Kestrel zur Laufzeit eine Bindung erstellt hat:</span><span class="sxs-lookup"><span data-stu-id="f0dde-200">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

<span data-ttu-id="f0dde-201">**UseUrls-Einschränkungen**</span><span class="sxs-lookup"><span data-stu-id="f0dde-201">**UseUrls limitations**</span></span>

<span data-ttu-id="f0dde-202">Sie können Endpunkte konfigurieren, indem Sie die `UseUrls`-Methode aufrufen oder das Befehlszeilenargument `urls` oder die Umgebungsvariable „ASPNETCORE_URLS“ verwenden.</span><span class="sxs-lookup"><span data-stu-id="f0dde-202">You can configure endpoints by calling the `UseUrls` method or using the `urls` command-line argument or the ASPNETCORE_URLS environment variable.</span></span> <span data-ttu-id="f0dde-203">Diese Methoden sind nützlich, wenn Ihr Code mit anderen Servern als Kestrel funktionieren soll.</span><span class="sxs-lookup"><span data-stu-id="f0dde-203">These methods are useful if you want your code to work with servers other than Kestrel.</span></span> <span data-ttu-id="f0dde-204">Beachten Sie jedoch folgende Einschränkungen:</span><span class="sxs-lookup"><span data-stu-id="f0dde-204">However, be aware of these limitations:</span></span>

* <span data-ttu-id="f0dde-205">Sie können SSL mit diesen Methoden nicht verwenden.</span><span class="sxs-lookup"><span data-stu-id="f0dde-205">You can't use SSL with these methods.</span></span>
* <span data-ttu-id="f0dde-206">Wenn Sie die `Listen`-Methode und `UseUrls` verwenden, setzt der `Listen`-Endpunkt die `UseUrls`-Endpunkte außer Kraft.</span><span class="sxs-lookup"><span data-stu-id="f0dde-206">If you use both the `Listen` method and `UseUrls`, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

<span data-ttu-id="f0dde-207">**Endpunktkonfiguration für IIS**</span><span class="sxs-lookup"><span data-stu-id="f0dde-207">**Endpoint configuration for IIS**</span></span>

<span data-ttu-id="f0dde-208">Wenn Sie IIS verwenden, setzen die URL-Bindungen für IIS alle Bindungen außer Kraft, die Sie durch Aufrufen von `Listen` oder `UseUrls` festlegen.</span><span class="sxs-lookup"><span data-stu-id="f0dde-208">If you use IIS, the URL bindings for IIS override any bindings that you set by calling either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="f0dde-209">Weitere Informationen finden Sie unter [Introduction to ASP.NET Core Module (Einführung in ASP.NET Core-Module)](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="f0dde-209">For more information, see [Introduction to ASP.NET Core Module](aspnet-core-module.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f0dde-210">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f0dde-210">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f0dde-211">Standardmäßig ist ASP.NET Core an `http://localhost:5000` gebunden.</span><span class="sxs-lookup"><span data-stu-id="f0dde-211">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="f0dde-212">Sie können URL-Präfixe und Ports konfigurieren, denen Kestrel lauschen soll, indem Sie die Erweiterungsmethode `UseUrls`, das Befehlszeilenargument `urls` oder das ASP.NET Core-Konfigurationssystem verwenden.</span><span class="sxs-lookup"><span data-stu-id="f0dde-212">You can configure URL prefixes and ports for Kestrel to listen on by using the `UseUrls` extension method, the `urls` command-line argument, or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="f0dde-213">Weitere Informationen zu diesen Methoden finden Sie unter [Hosting](../../fundamentals/hosting.md).</span><span class="sxs-lookup"><span data-stu-id="f0dde-213">For more information about these methods, see [Hosting](../../fundamentals/hosting.md).</span></span> <span data-ttu-id="f0dde-214">Weitere Informationen zur Funktionsweise der URL-Bindung bei der Verwendung von IIS als Reverseproxy finden Sie unter [ASP.NET Core-Module](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="f0dde-214">For information about how URL binding works when you use IIS as a reverse proxy, see [ASP.NET Core Module](aspnet-core-module.md).</span></span> 

---

### <a name="url-prefixes"></a><span data-ttu-id="f0dde-215">URL-Präfixe</span><span class="sxs-lookup"><span data-stu-id="f0dde-215">URL prefixes</span></span>

<span data-ttu-id="f0dde-216">Wenn Sie `UseUrls` aufrufen oder das Befehlszeilenargument `urls` oder die Umgebungsvariable „ASPNETCORE_URLS“ verwenden, kann das URL-Präfix in folgenden Formaten vorliegen.</span><span class="sxs-lookup"><span data-stu-id="f0dde-216">If you call `UseUrls` or use the `urls` command-line argument or ASPNETCORE_URLS environment variable, the URL prefixes can be in any of the following formats.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f0dde-217">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f0dde-217">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f0dde-218">Nur HTTP-URL-Präfixe sind gültig. Kestrel unterstützt SSL nicht, wenn Sie URL-Bindungen mithilfe von `UseUrls` konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="f0dde-218">Only HTTP URL prefixes are valid; Kestrel doesn't support SSL when you configure URL bindings by using `UseUrls`.</span></span>

* <span data-ttu-id="f0dde-219">IPv4-Adresse mit Portnummer</span><span class="sxs-lookup"><span data-stu-id="f0dde-219">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="f0dde-220">Bei 0.0.0.0 handelt es sich um einen Sonderfall, für den eine Bindung an alle IPv4-Adressen erfolgt.</span><span class="sxs-lookup"><span data-stu-id="f0dde-220">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="f0dde-221">IPv6-Adresse mit Portnummer</span><span class="sxs-lookup"><span data-stu-id="f0dde-221">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  <span data-ttu-id="f0dde-222">[::] stellt das Äquivalent von IPv6 zu 0.0.0.0 für IPv4 dar.</span><span class="sxs-lookup"><span data-stu-id="f0dde-222">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="f0dde-223">Hostname mit Portnummer</span><span class="sxs-lookup"><span data-stu-id="f0dde-223">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="f0dde-224">Hostnamen, \* und + sind nicht spezifisch.</span><span class="sxs-lookup"><span data-stu-id="f0dde-224">Host names, \*, and +, are not special.</span></span> <span data-ttu-id="f0dde-225">Für alle Elemente, die nicht als IP-Adresse oder „localhost“ erkannt werden, wird eine Bindung an alle IPv4- und IPv6-IP-Adressen erstellt.</span><span class="sxs-lookup"><span data-stu-id="f0dde-225">Anything that's not a recognized IP address or "localhost" will bind to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="f0dde-226">Wenn Sie verschiedene Hostnamen an verschiedene ASP.NET Core-Anwendung auf demselben Port binden, verwenden Sie [HTTP.sys](httpsys.md) oder einen Reverseproxyserver wie IIS, Nginx oder Apache.</span><span class="sxs-lookup"><span data-stu-id="f0dde-226">If you need to bind different host names to different ASP.NET Core applications on the same port, use [HTTP.sys](httpsys.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="f0dde-227">„localhost“ als Name mit Portnummer oder Loopback-IP mit Portnummer</span><span class="sxs-lookup"><span data-stu-id="f0dde-227">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="f0dde-228">Wenn `localhost` angegeben ist, versucht Kestrel, eine Bindung zu IPv4- und IPv6-Loopback-Schnittstellen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="f0dde-228">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="f0dde-229">Wenn der erforderliche Port von einem anderen Dienst auf einer der Loopback-Schnittstellen verwendet wird, tritt beim Starten von Kestrel ein Fehler auf.</span><span class="sxs-lookup"><span data-stu-id="f0dde-229">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="f0dde-230">Wenn eine der Loopback-Schnittstellen aus anderen Gründen nicht verfügbar ist (meistens durch die fehlende Unterstützung von IPv6), protokolliert Kestrel eine Warnung.</span><span class="sxs-lookup"><span data-stu-id="f0dde-230">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span> 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f0dde-231">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f0dde-231">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="f0dde-232">IPv4-Adresse mit Portnummer</span><span class="sxs-lookup"><span data-stu-id="f0dde-232">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  <span data-ttu-id="f0dde-233">Bei 0.0.0.0 handelt es sich um einen Sonderfall, für den eine Bindung an alle IPv4-Adressen erfolgt.</span><span class="sxs-lookup"><span data-stu-id="f0dde-233">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="f0dde-234">IPv6-Adresse mit Portnummer</span><span class="sxs-lookup"><span data-stu-id="f0dde-234">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  <span data-ttu-id="f0dde-235">[::] stellt das Äquivalent von IPv6 zu 0.0.0.0 für IPv4 dar.</span><span class="sxs-lookup"><span data-stu-id="f0dde-235">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="f0dde-236">Hostname mit Portnummer</span><span class="sxs-lookup"><span data-stu-id="f0dde-236">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  <span data-ttu-id="f0dde-237">Hostnamen, \* und + sind nicht spezifisch.</span><span class="sxs-lookup"><span data-stu-id="f0dde-237">Host names, \*, and + aren't special.</span></span> <span data-ttu-id="f0dde-238">Für alle Elemente, die nicht als IP-Adresse oder „localhost“ erkannt werden, wird eine Bindung an alle IPv4- und IPv6-IP-Adressen erstellt.</span><span class="sxs-lookup"><span data-stu-id="f0dde-238">Anything that isn't a recognized IP address or "localhost" binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="f0dde-239">Wenn Sie verschiedene Hostnamen an verschiedene ASP.NET Core-Anwendung auf demselben Port binden, verwenden Sie [WebListener](weblistener.md) oder einen Reverseproxyserver wie IIS, Nginx oder Apache.</span><span class="sxs-lookup"><span data-stu-id="f0dde-239">If you need to bind different host names to different ASP.NET Core applications on the same port, use [WebListener](weblistener.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="f0dde-240">„localhost“ als Name mit Portnummer oder Loopback-IP mit Portnummer</span><span class="sxs-lookup"><span data-stu-id="f0dde-240">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="f0dde-241">Wenn `localhost` angegeben ist, versucht Kestrel, eine Bindung zu IPv4- und IPv6-Loopback-Schnittstellen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="f0dde-241">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="f0dde-242">Wenn der erforderliche Port von einem anderen Dienst auf einer der Loopback-Schnittstellen verwendet wird, tritt beim Starten von Kestrel ein Fehler auf.</span><span class="sxs-lookup"><span data-stu-id="f0dde-242">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="f0dde-243">Wenn eine der Loopback-Schnittstellen aus anderen Gründen nicht verfügbar ist (meistens durch die fehlende Unterstützung von IPv6), protokolliert Kestrel eine Warnung.</span><span class="sxs-lookup"><span data-stu-id="f0dde-243">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span> 

* <span data-ttu-id="f0dde-244">Unix-Socket</span><span class="sxs-lookup"><span data-stu-id="f0dde-244">Unix socket</span></span>

  ```
  http://unix:/run/dan-live.sock  
  ```

<span data-ttu-id="f0dde-245">**Port 0**</span><span class="sxs-lookup"><span data-stu-id="f0dde-245">**Port 0**</span></span>

<span data-ttu-id="f0dde-246">Wenn Sie die Portnummer 0 angeben, erstellt Kestrel eine dynamische Bindung an einen verfügbaren Port.</span><span class="sxs-lookup"><span data-stu-id="f0dde-246">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="f0dde-247">Die Bindung an Port 0 ist für jeden Hostnamen oder jede IP-Adresse zulässig, außer für solche mit dem Namen `localhost`.</span><span class="sxs-lookup"><span data-stu-id="f0dde-247">Binding to port 0 is allowed for any host name or IP except for `localhost` name.</span></span>

<span data-ttu-id="f0dde-248">Das folgende Beispiel veranschaulicht, wie bestimmt werden kann, für welchen Port Kestrel zur Laufzeit eine Bindung erstellt hat:</span><span class="sxs-lookup"><span data-stu-id="f0dde-248">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

<span data-ttu-id="f0dde-249">**URL-Präfixe für SSL**</span><span class="sxs-lookup"><span data-stu-id="f0dde-249">**URL prefixes for SSL**</span></span>

<span data-ttu-id="f0dde-250">Achten Sie wie im Folgenden dargestellt darauf, URL-Präfixe mit `https:` einzuschließen, wenn Sie die Erweiterungsmethode `UseHttps` aufrufen.</span><span class="sxs-lookup"><span data-stu-id="f0dde-250">Be sure to include URL prefixes with `https:` if you call the `UseHttps` extension method, as shown below.</span></span>

```csharp
var host = new WebHostBuilder() 
    .UseKestrel(options => 
    { 
        options.UseHttps("testCert.pfx", "testPassword"); 
    }) 
   .UseUrls("http://localhost:5000", "https://localhost:5001") 
   .UseContentRoot(Directory.GetCurrentDirectory()) 
   .UseStartup<Startup>() 
   .Build(); 
```

> [!NOTE]
> <span data-ttu-id="f0dde-251">HTTPS und HTTP können nicht auf demselben Port gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="f0dde-251">HTTPS and HTTP cannot be hosted on the same port.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

---
## <a name="next-steps"></a><span data-ttu-id="f0dde-252">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="f0dde-252">Next steps</span></span>

<span data-ttu-id="f0dde-253">Weitere Informationen finden Sie in den folgenden Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="f0dde-253">For more information, see the following resources:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f0dde-254">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f0dde-254">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* [<span data-ttu-id="f0dde-255">Beispiel-App für 2.x</span><span class="sxs-lookup"><span data-stu-id="f0dde-255">Sample app for 2.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [<span data-ttu-id="f0dde-256">Kestrel-Quellcode</span><span class="sxs-lookup"><span data-stu-id="f0dde-256">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f0dde-257">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f0dde-257">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* [<span data-ttu-id="f0dde-258">Beispiel-App für 1.x</span><span class="sxs-lookup"><span data-stu-id="f0dde-258">Sample app for 1.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [<span data-ttu-id="f0dde-259">Kestrel-Quellcode</span><span class="sxs-lookup"><span data-stu-id="f0dde-259">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

---
