---
title: Kestrel webserverimplementierung in ASP.NET Core
author: tdykstra
description: "Führt ein Kestrel, die plattformübergreifende Webserver für ASP.NET Core auf Libuv basieren."
keywords: "ASP.NET Core, Kestrel, Libuv, Url-Präfixe"
ms.author: tdykstra
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.assetid: 560bd66f-7dd0-4e68-b5fb-f31477e4b785
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/kestrel
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 451c36fc9095b6e076e5287c992b6163205c523b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="823a7-104">Einführung in die Kestrel webserverimplementierung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="823a7-104">Introduction to Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="823a7-105">Durch [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), und [Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="823a7-105">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="823a7-106">Kestrel ist eine plattformübergreifende [Webserver für ASP.NET Core](index.md) basierend auf [Libuv](https://github.com/libuv/libuv), eine plattformübergreifende asynchrone e/a-Bibliothek.</span><span class="sxs-lookup"><span data-stu-id="823a7-106">Kestrel is a cross-platform [web server for ASP.NET Core](index.md) based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span> <span data-ttu-id="823a7-107">Kestrel ist der Webserver, der standardmäßig in ASP.NET Core-Projektvorlagen enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="823a7-107">Kestrel is the web server that is included by default in ASP.NET Core project templates.</span></span> 

<span data-ttu-id="823a7-108">Kestrel unterstützt die folgenden Funktionen:</span><span class="sxs-lookup"><span data-stu-id="823a7-108">Kestrel supports the following features:</span></span>

  * <span data-ttu-id="823a7-109">HTTPS</span><span class="sxs-lookup"><span data-stu-id="823a7-109">HTTPS</span></span>
  * <span data-ttu-id="823a7-110">Nicht transparente Upgrade zur Aktivierung [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="823a7-110">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
  * <span data-ttu-id="823a7-111">UNIX-Sockets für hohe Leistung hinter Nginx</span><span class="sxs-lookup"><span data-stu-id="823a7-111">Unix sockets for high performance behind Nginx</span></span> 

<span data-ttu-id="823a7-112">Kestrel wird unterstützt, auf allen Plattformen und Versionen, die .NET Core unterstützt.</span><span class="sxs-lookup"><span data-stu-id="823a7-112">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="823a7-113">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="823a7-113">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="823a7-114">[Anzeigen oder Herunterladen von Beispielcode für 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([zum Herunterladen von](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="823a7-114">[View or download sample code for 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="823a7-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="823a7-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="823a7-116">[Anzeigen oder Herunterladen von Beispielcode für 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([zum Herunterladen von](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="823a7-116">[View or download sample code for 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="823a7-117">Verwendung von Kestrel mit einer reverse-proxy</span><span class="sxs-lookup"><span data-stu-id="823a7-117">When to use Kestrel with a reverse proxy</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="823a7-118">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="823a7-118">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="823a7-119">Sie können Kestrel als eigenständigen Webserver oder mit einem *Reverseproxyserver* wie IIS, Nginx oder Apache verwenden.</span><span class="sxs-lookup"><span data-stu-id="823a7-119">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="823a7-120">Ein Reverseproxyserver empfängt HTTP-Anforderungen aus dem Internet und leitet diese nach einer vorbereitenden Verarbeitung an Kestrel weiter.</span><span class="sxs-lookup"><span data-stu-id="823a7-120">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel kommuniziert direkt und ohne Reverseproxyserver mit dem Internet](kestrel/_static/kestrel-to-internet2.png)

![Kestrel kommuniziert indirekt mit dem Internet über einen Reverseproxyserver wie IIS, Nginx oder Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="823a7-123">Jede der beiden Konfigurationen &mdash;, egal ob mit oder ohne Reverseproxyserver &mdash;, kann auch dann verwendet werden, wenn Kestrel nur in einem internen Netzwerk verfügbar gemacht wird.</span><span class="sxs-lookup"><span data-stu-id="823a7-123">Either configuration &mdash; with or without a reverse proxy server &mdash; can also be used if Kestrel is exposed only to an internal network.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="823a7-124">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="823a7-124">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="823a7-125">Wenn Ihre Anwendung nur Anforderungen aus einem internen Netzwerk akzeptiert, können Sie Kestrel als eigenständigen Webserver verwenden.</span><span class="sxs-lookup"><span data-stu-id="823a7-125">If your application accepts requests only from an internal network, you can use Kestrel by itself.</span></span>

![Kestrel kommuniziert direkt mit Ihrem internen Netzwerk](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="823a7-127">Wenn Sie Ihre Anwendung im Internet verfügbar machen, müssen Sie IIS, Nginx oder Apache als *Reverseproxyserver* verwenden.</span><span class="sxs-lookup"><span data-stu-id="823a7-127">If you expose your application to the Internet, you must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="823a7-128">Ein Reverseproxyserver empfängt HTTP-Anforderungen aus dem Internet und leitet diese nach einer vorbereitenden Verarbeitung an Kestrel weiter.</span><span class="sxs-lookup"><span data-stu-id="823a7-128">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel kommuniziert indirekt mit dem Internet über einen Reverseproxyserver wie IIS, Nginx oder Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="823a7-130">Reverse-Proxy ist für Edge-Bereitstellungen (für den Datenverkehr aus dem Internet ausgesetzt) aus Sicherheitsgründen erforderlich.</span><span class="sxs-lookup"><span data-stu-id="823a7-130">A reverse proxy is required for edge deployments (exposed to traffic from the Internet) for security reasons.</span></span> <span data-ttu-id="823a7-131">Die Kestrel-Versionen 1.x besitzen keine umfassenden Schutzmaßnahmen gegenüber Angriffen.</span><span class="sxs-lookup"><span data-stu-id="823a7-131">The 1.x versions of Kestrel don't have a full complement of defenses against attacks.</span></span> <span data-ttu-id="823a7-132">Dies schließt jedoch nicht auf die entsprechenden Timeouts, Grenzwerte für sammlungsgröße und Limits für gleichzeitige Verbindungen beschränkt.</span><span class="sxs-lookup"><span data-stu-id="823a7-132">This includes but isn't limited to appropriate timeouts, size limits, and concurrent connection limits.</span></span>

---

<span data-ttu-id="823a7-133">Reverseproxy erforderlich ist, wenn Sie mehrere Anwendungen verfügen, die gemeinsam dieselbe IP und Ports auf einem einzelnen Server ausführen.</span><span class="sxs-lookup"><span data-stu-id="823a7-133">A scenario that requires a reverse proxy is when you have multiple applications that share the same IP and port running on a single server.</span></span> <span data-ttu-id="823a7-134">Die nicht direkt mit Kestrel arbeiten, da Kestrel Freigeben von derselben IP- und Port zwischen mehreren Prozessen nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="823a7-134">That doesn't work with Kestrel directly because Kestrel doesn't support sharing the same IP and port between multiple processes.</span></span> <span data-ttu-id="823a7-135">Wenn Sie Kestrel zum Lauschen an einem Port konfigurieren, verarbeitet sie Datenverkehr für diesen Port unabhängig von der Hostheader.</span><span class="sxs-lookup"><span data-stu-id="823a7-135">When you configure Kestrel to listen on a port, it handles all traffic for that port regardless of host header.</span></span> <span data-ttu-id="823a7-136">Ein reverse-Proxy, der Ports gemeinsam verwenden, kann muss dann an Kestrel auf eine eindeutige IP- und -Port weiterleiten.</span><span class="sxs-lookup"><span data-stu-id="823a7-136">A reverse proxy that can share ports must then forward to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="823a7-137">Auch wenn ein reverse-Proxy-Server nicht erforderlich ist, kann mithilfe einer eine gute Wahl aus anderen Gründen sein:</span><span class="sxs-lookup"><span data-stu-id="823a7-137">Even if a reverse proxy server isn't required, using one might be a good choice for other reasons:</span></span>

* <span data-ttu-id="823a7-138">Sie können die verfügbar gemachten Oberfläche einschränken.</span><span class="sxs-lookup"><span data-stu-id="823a7-138">It can limit your exposed surface area.</span></span>
* <span data-ttu-id="823a7-139">Es bietet eine optionale zusätzliche Ebene der Konfiguration und den gründlichen Schutz.</span><span class="sxs-lookup"><span data-stu-id="823a7-139">It provides an optional additional layer of configuration and defense.</span></span>
* <span data-ttu-id="823a7-140">Es kann eine bessere Leistung in die vorhandene Infrastruktur integrieren.</span><span class="sxs-lookup"><span data-stu-id="823a7-140">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="823a7-141">Sie vereinfacht den Lastenausgleich und SSL-Einrichtung.</span><span class="sxs-lookup"><span data-stu-id="823a7-141">It simplifies load balancing and SSL set-up.</span></span> <span data-ttu-id="823a7-142">Nur der reverse-Proxy-Server erfordert ein SSL-Zertifikat, und dieser Server mit Ihrer Anwendungsserver, auf dem internen Netzwerk über einfaches HTTP kommunizieren kann.</span><span class="sxs-lookup"><span data-stu-id="823a7-142">Only your reverse proxy server requires an SSL certificate, and that server can communicate with your application servers on the internal network using plain HTTP.</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="823a7-143">Gewusst wie: Kestrel in ASP.NET Core-apps verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="823a7-143">How to use Kestrel in ASP.NET Core apps</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="823a7-144">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="823a7-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="823a7-145">Die [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) Paket enthalten ist, der [Microsoft.AspNetCore.All Metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="823a7-145">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="823a7-146">ASP.NET Core-Projektvorlagen Kestrel wird standardmäßig verwendet.</span><span class="sxs-lookup"><span data-stu-id="823a7-146">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="823a7-147">In *"Program.cs"*, ruft die Vorlage `CreateDefaultBuilder`, welche Aufrufe [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) im Hintergrund.</span><span class="sxs-lookup"><span data-stu-id="823a7-147">In *Program.cs*, the template code calls `CreateDefaultBuilder`, which calls [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) behind the scenes.</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="823a7-148">Wenn Sie Optionen für die Kestrel konfigurieren müssen, rufen Sie `UseKestrel` in *"Program.cs"* wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="823a7-148">If you need to configure Kestrel options, call `UseKestrel` in *Program.cs* as shown in the following example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="823a7-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="823a7-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="823a7-150">Installieren der [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet-Paket.</span><span class="sxs-lookup"><span data-stu-id="823a7-150">Install the [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet package.</span></span>

<span data-ttu-id="823a7-151">Rufen Sie die [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) Erweiterungsmethode auf `WebHostBuilder` in Ihrer `Main` Methode, dabei werden alle [Kestrel Optionen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) , die Sie benötigen, wie im nächsten Abschnitt gezeigt.</span><span class="sxs-lookup"><span data-stu-id="823a7-151">Call the [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) extension method on `WebHostBuilder` in your `Main` method, specifying any [Kestrel options](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) that you need, as shown in the next section.</span></span>

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a><span data-ttu-id="823a7-152">Kestrel-Optionen</span><span class="sxs-lookup"><span data-stu-id="823a7-152">Kestrel options</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="823a7-153">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="823a7-153">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="823a7-154">Der Webserver Kestrel hat Einschränkung Konfigurationsoptionen, die vor allem hilfreich bei Bereitstellungen mit Internetzugriff sind.</span><span class="sxs-lookup"><span data-stu-id="823a7-154">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span> <span data-ttu-id="823a7-155">Hier sind einige der Grenzen, die Sie festlegen können:</span><span class="sxs-lookup"><span data-stu-id="823a7-155">Here are some of the limits you can set:</span></span>

- <span data-ttu-id="823a7-156">Maximale Anzahl der Clientverbindungen</span><span class="sxs-lookup"><span data-stu-id="823a7-156">Maximum client connections</span></span>
- <span data-ttu-id="823a7-157">Maximale Größe des Anforderungstexts</span><span class="sxs-lookup"><span data-stu-id="823a7-157">Maximum request body size</span></span>
- <span data-ttu-id="823a7-158">Minimale Datenrate des Anforderungstexts</span><span class="sxs-lookup"><span data-stu-id="823a7-158">Minimum request body data rate</span></span>

<span data-ttu-id="823a7-159">Sie legen diese Einschränkungen und andere in der `Limits` Eigenschaft von der [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) Klasse.</span><span class="sxs-lookup"><span data-stu-id="823a7-159">You set these constraints and others in the `Limits` property of the [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) class.</span></span> <span data-ttu-id="823a7-160">Die `Limits` Eigenschaft enthält eine Instanz der dem [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) Klasse.</span><span class="sxs-lookup"><span data-stu-id="823a7-160">The `Limits` property holds an instance of the [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) class.</span></span> 

<span data-ttu-id="823a7-161">**Maximaler Client-Verbindungen**</span><span class="sxs-lookup"><span data-stu-id="823a7-161">**Maximum client connections**</span></span>

<span data-ttu-id="823a7-162">Die maximale Anzahl von gleichzeitig geöffneten TCP-Verbindungen kann für die gesamte Anwendung durch den folgenden Code festgelegt werden:</span><span class="sxs-lookup"><span data-stu-id="823a7-162">The maximum number of concurrent open TCP connections can be set for the entire application with the following code:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="823a7-163">Es wird eine separate Anzahl an Verbindungen, die auf ein anderes Protokoll (z. B. auf eine Anforderung WebSockets) über HTTP oder HTTPS aktualisiert wurden.</span><span class="sxs-lookup"><span data-stu-id="823a7-163">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="823a7-164">Nachdem eine Verbindung aktualisiert wird, wird nicht gezählt gegen die `MaxConcurrentConnections` Grenzwert.</span><span class="sxs-lookup"><span data-stu-id="823a7-164">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span> 

<span data-ttu-id="823a7-165">Die maximale Anzahl von Verbindungen ist standardmäßig unbegrenzt (Null).</span><span class="sxs-lookup"><span data-stu-id="823a7-165">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="823a7-166">**Maximale Anforderungsgröße-Text**</span><span class="sxs-lookup"><span data-stu-id="823a7-166">**Maximum request body size**</span></span>

<span data-ttu-id="823a7-167">Die Standardgröße der Höchstwert für die Anforderung Text ist 30,000,000 Bytes, also ungefähr 28.6 MB.</span><span class="sxs-lookup"><span data-stu-id="823a7-167">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span> 

<span data-ttu-id="823a7-168">Die empfohlene Methode zum Überschreiben Sie des Grenzwert von in einer ASP.NET-MVC-Anwendung Core ist die Verwendung der [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) Attribut auf eine Aktionsmethode:</span><span class="sxs-lookup"><span data-stu-id="823a7-168">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="823a7-169">Hier ist ein Beispiel, das veranschaulicht, wie die Einschränkung für die gesamte Anwendung, jede Anforderung zu konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="823a7-169">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="823a7-170">Sie können die Einstellung für eine bestimmte Anforderung in Middleware überschreiben:</span><span class="sxs-lookup"><span data-stu-id="823a7-170">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
<span data-ttu-id="823a7-171">Eine Ausnahme wird ausgelöst, wenn Sie versuchen, den Grenzwert für eine Anforderung zu konfigurieren, nach dem Start der Anwendung Lesen der Anforderung.</span><span class="sxs-lookup"><span data-stu-id="823a7-171">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="823a7-172">Ist ein `IsReadOnly` -Eigenschaft, die Aufschluss darüber gibt Wenn die `MaxRequestBodySize` Eigenschaft befindet sich im schreibgeschützten Zustand, d. h., es ist zu spät so konfigurieren Sie den Grenzwert.</span><span class="sxs-lookup"><span data-stu-id="823a7-172">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="823a7-173">**Minimale Text Data-Anforderungsrate**</span><span class="sxs-lookup"><span data-stu-id="823a7-173">**Minimum request body data rate**</span></span>

<span data-ttu-id="823a7-174">Kestrel überprüft pro Sekunde an, wenn Daten mit der angegebenen Rate in Bytes/Sekunde in stammt.</span><span class="sxs-lookup"><span data-stu-id="823a7-174">Kestrel checks every second if data is coming in at the specified rate in bytes/second.</span></span> <span data-ttu-id="823a7-175">Wenn die Rate der Minimum unterschreitet, wird die Verbindung ein Timeout. Der Toleranzperiode ist die Zeitspanne, Kestrel den Client seine Senderate bis zu mindestens erhöhen kann. die Rate wird während dieses Zeitraums nicht überprüft.</span><span class="sxs-lookup"><span data-stu-id="823a7-175">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate is not checked during that time.</span></span> <span data-ttu-id="823a7-176">Die Toleranzperiode wird vermieden, Verbindungen, die anfänglich für das Senden von Daten mit einer langsamen Rate aufgrund TCP langsam gestartet.</span><span class="sxs-lookup"><span data-stu-id="823a7-176">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="823a7-177">Der minimale Standardwert ist 240 Bytes/Sekunde, mit der eine Toleranzperiode von fünf Sekunden.</span><span class="sxs-lookup"><span data-stu-id="823a7-177">The default minimum rate is 240 bytes/second, with a 5 second grace period.</span></span>

<span data-ttu-id="823a7-178">Ein Mindestprozentsatz gilt auch für die Antwort.</span><span class="sxs-lookup"><span data-stu-id="823a7-178">A minimum rate also applies to the response.</span></span> <span data-ttu-id="823a7-179">Der Code aus, um die "Anforderungslimit" und die Obergrenze für ist identisch, außer dass `RequestBody` oder `Response` im Namen Eigenschaft und -Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="823a7-179">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span> 

<span data-ttu-id="823a7-180">Hier ist ein Beispiel, das zeigt, wie so konfigurieren Sie die minimale Datenraten in *"Program.cs"*:</span><span class="sxs-lookup"><span data-stu-id="823a7-180">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="823a7-181">Sie können die Sätze pro Anforderung in Middleware konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="823a7-181">You can configure the rates per request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

<span data-ttu-id="823a7-182">Informationen zu den weiteren Optionen Kestrel finden Sie die folgenden Klassen:</span><span class="sxs-lookup"><span data-stu-id="823a7-182">For information about other Kestrel options, see the following classes:</span></span>

* [<span data-ttu-id="823a7-183">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="823a7-183">KestrelServerOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [<span data-ttu-id="823a7-184">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="823a7-184">KestrelServerLimits</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [<span data-ttu-id="823a7-185">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="823a7-185">ListenOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="823a7-186">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="823a7-186">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="823a7-187">Informationen zu Kestrel-Optionen finden Sie unter [KestrelServerOptions Klasse](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span><span class="sxs-lookup"><span data-stu-id="823a7-187">For information about Kestrel options, see [KestrelServerOptions class](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span></span>

---

### <a name="endpoint-configuration"></a><span data-ttu-id="823a7-188">Endpunktkonfiguration</span><span class="sxs-lookup"><span data-stu-id="823a7-188">Endpoint configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="823a7-189">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="823a7-189">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="823a7-190">Standardmäßig ASP.NET Core bindet an `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="823a7-190">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="823a7-191">Konfigurieren von URL-Präfixe und Ports für Kestrel Abhören durch Aufrufen von `Listen` oder `ListenUnixSocket` Methoden auf `KestrelServerOptions`.</span><span class="sxs-lookup"><span data-stu-id="823a7-191">You configure URL prefixes and ports for Kestrel to listen on by calling `Listen` or `ListenUnixSocket` methods on `KestrelServerOptions`.</span></span> <span data-ttu-id="823a7-192">(`UseUrls`, `urls` Befehlszeilenargument und die Umgebungsvariable ASPNETCORE_URLS funktionieren auch bieten jedoch die angegeben Einschränkungen [weiter unten in diesem Artikel](#useurls-limitations).)</span><span class="sxs-lookup"><span data-stu-id="823a7-192">(`UseUrls`, the `urls` command-line argument, and the ASPNETCORE_URLS environment variable also work but have the limitations noted [later in this article](#useurls-limitations).)</span></span>

<span data-ttu-id="823a7-193">**Binden Sie an einen TCP-socket**</span><span class="sxs-lookup"><span data-stu-id="823a7-193">**Bind to a TCP socket**</span></span>

<span data-ttu-id="823a7-194">Die `Listen` -Methode bindet ein TCP-Socket, und das Lambda einer Optionen können Sie ein SSL-Zertifikat zu konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="823a7-194">The `Listen` method binds to a TCP socket, and an options lambda lets you configure an SSL certificate:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

<span data-ttu-id="823a7-195">Beachten Sie, wie in diesem Beispiel wird mithilfe von SSL für einen bestimmten Endpunkt konfiguriert [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="823a7-195">Notice how this example configures SSL for a particular endpoint by using [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span></span> <span data-ttu-id="823a7-196">Sie können die gleiche API verwenden, andere Kestrel für bestimmte Endpunkte konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="823a7-196">You can use the same API to configure other Kestrel settings for particular endpoints.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

<span data-ttu-id="823a7-197">**Binden Sie an ein Unix-socket**</span><span class="sxs-lookup"><span data-stu-id="823a7-197">**Bind to a Unix socket**</span></span>

<span data-ttu-id="823a7-198">Sie können auf einem Unix-Socket für verbesserte Leistung mit Nginx, Lauschen wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="823a7-198">You can listen on a Unix socket for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

<span data-ttu-id="823a7-199">**Port 0**</span><span class="sxs-lookup"><span data-stu-id="823a7-199">**Port 0**</span></span>

<span data-ttu-id="823a7-200">Wenn Sie die Portnummer 0 angeben, wird die Kestrel dynamisch auf einen verfügbaren Port gebunden.</span><span class="sxs-lookup"><span data-stu-id="823a7-200">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="823a7-201">Das folgende Beispiel zeigt, wie Sie den Port ermittelt haben Kestrel tatsächlich zur Laufzeit gebunden:</span><span class="sxs-lookup"><span data-stu-id="823a7-201">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

<span data-ttu-id="823a7-202">**UseUrls Einschränkungen**</span><span class="sxs-lookup"><span data-stu-id="823a7-202">**UseUrls limitations**</span></span>

<span data-ttu-id="823a7-203">Konfigurieren Sie Endpunkte durch Aufrufen der `UseUrls` -Methode oder mithilfe der `urls` Befehlszeilenargument oder die Umgebungsvariable ASPNETCORE_URLS.</span><span class="sxs-lookup"><span data-stu-id="823a7-203">You can configure endpoints by calling the `UseUrls` method or using the `urls` command-line argument or the ASPNETCORE_URLS environment variable.</span></span> <span data-ttu-id="823a7-204">Diese Methoden sind nützlich, wenn Sie Code mit anderen Servern als Kestrel arbeiten möchten.</span><span class="sxs-lookup"><span data-stu-id="823a7-204">These methods are useful if you want your code to work with servers other than Kestrel.</span></span> <span data-ttu-id="823a7-205">Bedenken Sie jedoch, die diese Einschränkungen:</span><span class="sxs-lookup"><span data-stu-id="823a7-205">However, be aware of these limitations:</span></span>

* <span data-ttu-id="823a7-206">Sie können keine SSL mit diesen Methoden.</span><span class="sxs-lookup"><span data-stu-id="823a7-206">You can't use SSL with these methods.</span></span>
* <span data-ttu-id="823a7-207">Wenn Sie beides verwenden die `Listen` Methode und `UseUrls`, die `Listen` Endpunkte überschreiben die `UseUrls` Endpunkte.</span><span class="sxs-lookup"><span data-stu-id="823a7-207">If you use both the `Listen` method and `UseUrls`, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

<span data-ttu-id="823a7-208">**Dienstendpunkt-Konfiguration für IIS**</span><span class="sxs-lookup"><span data-stu-id="823a7-208">**Endpoint configuration for IIS**</span></span>

<span data-ttu-id="823a7-209">Wenn Sie IIS verwenden, wird die URL-Bindungen für IIS außer Kraft setzen alle Bindungen, die Sie festgelegt werden, indem entweder die `Listen` oder `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="823a7-209">If you use IIS, the URL bindings for IIS override any bindings that you set by calling either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="823a7-210">Weitere Informationen finden Sie unter [Einführung in ASP.NET Core-Modul](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="823a7-210">For more information, see [Introduction to ASP.NET Core Module](aspnet-core-module.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="823a7-211">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="823a7-211">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="823a7-212">Standardmäßig ASP.NET Core bindet an `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="823a7-212">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="823a7-213">Sie können konfigurieren, URL-Präfixe und Ports für Kestrel Abhören mithilfe der `UseUrls` Erweiterungsmethode, die `urls` Befehlszeilenargument oder das Konfigurationssystem ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="823a7-213">You can configure URL prefixes and ports for Kestrel to listen on by using the `UseUrls` extension method, the `urls` command-line argument, or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="823a7-214">Weitere Informationen zu diesen Methoden finden Sie unter [Hosting](../../fundamentals/hosting.md).</span><span class="sxs-lookup"><span data-stu-id="823a7-214">For more information about these methods, see [Hosting](../../fundamentals/hosting.md).</span></span> <span data-ttu-id="823a7-215">Informationen zur Funktionsweise der URL-Bindung bei der Verwendung von IIS als Reverseproxy finden Sie unter [ASP.NET Core-Modul](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="823a7-215">For information about how URL binding works when you use IIS as a reverse proxy, see [ASP.NET Core Module](aspnet-core-module.md).</span></span> 

---

### <a name="url-prefixes"></a><span data-ttu-id="823a7-216">URL-Präfixe</span><span class="sxs-lookup"><span data-stu-id="823a7-216">URL prefixes</span></span>

<span data-ttu-id="823a7-217">Beim Aufrufen `UseUrls` , oder verwenden Sie die `urls` Befehlszeilenargument oder ASPNETCORE_URLS-Umgebungsvariablen angegeben, können die URL-Präfixe in einem der folgenden Formate sein.</span><span class="sxs-lookup"><span data-stu-id="823a7-217">If you call `UseUrls` or use the `urls` command-line argument or ASPNETCORE_URLS environment variable, the URL prefixes can be in any of the following formats.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="823a7-218">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="823a7-218">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="823a7-219">Nur HTTP-URL-Präfixe sind gültig. Kestrel unterstützt keine SSL beim Konfigurieren von URL-Bindungen mit `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="823a7-219">Only HTTP URL prefixes are valid; Kestrel does not support SSL when you configure URL bindings by using `UseUrls`.</span></span>

* <span data-ttu-id="823a7-220">IPv4-Adresse mit Port-Nummer</span><span class="sxs-lookup"><span data-stu-id="823a7-220">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="823a7-221">0.0.0.0 ist ein Sonderfall, der an alle IPv4-Adressen gebunden.</span><span class="sxs-lookup"><span data-stu-id="823a7-221">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="823a7-222">IPv6-Adresse mit Port-Nummer</span><span class="sxs-lookup"><span data-stu-id="823a7-222">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  <span data-ttu-id="823a7-223">[::] ist das Äquivalent IPv6 IPv4 0.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="823a7-223">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="823a7-224">Hostname und Portnummer</span><span class="sxs-lookup"><span data-stu-id="823a7-224">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="823a7-225">Hostnamen, *, und + sind nicht spezifisch.</span><span class="sxs-lookup"><span data-stu-id="823a7-225">Host names, *, and +, are not special.</span></span> <span data-ttu-id="823a7-226">Elemente, die keinen erkannten IP-Adresse oder "Localhost" wird für alle IPv4- und IPv6-IP-Adressen gebunden.</span><span class="sxs-lookup"><span data-stu-id="823a7-226">Anything that is not a recognized IP address or "localhost" will bind to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="823a7-227">Wenn Sie unterschiedliche Hostnamen an verschiedenen ASP.NET Core-Anwendungen auf demselben Port binden, verwenden [HTTP.sys](httpsys.md) oder einen reverse-Proxy-Server wie IIS, Nginx oder Apache.</span><span class="sxs-lookup"><span data-stu-id="823a7-227">If you need to bind different host names to different ASP.NET Core applications on the same port, use [HTTP.sys](httpsys.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="823a7-228">Name von "Localhost" mit Portnummer Port Nummer oder Loopback-IP</span><span class="sxs-lookup"><span data-stu-id="823a7-228">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="823a7-229">Wenn `localhost` angegeben wird, Kestrel versucht, IPv4 und IPv6-Loopback-Schnittstellen zu binden.</span><span class="sxs-lookup"><span data-stu-id="823a7-229">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="823a7-230">Wenn es sich bei der angeforderte Port verwendet, die von einem anderen Dienst entweder Loopback-Schnittstelle wird, nicht Kestrel gestartet.</span><span class="sxs-lookup"><span data-stu-id="823a7-230">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="823a7-231">Wenn der Loopback-Schnittstellen für andere Zwecke nicht verfügbar ist (die meisten häufig verwendet werden, weil IPv6 nicht unterstützt wird), Kestrel protokolliert eine Warnung.</span><span class="sxs-lookup"><span data-stu-id="823a7-231">If either loopback interface is unavailable for any other reason (most commonly because IPv6 is not supported), Kestrel logs a warning.</span></span> 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="823a7-232">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="823a7-232">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="823a7-233">IPv4-Adresse mit Port-Nummer</span><span class="sxs-lookup"><span data-stu-id="823a7-233">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  <span data-ttu-id="823a7-234">0.0.0.0 ist ein Sonderfall, der an alle IPv4-Adressen gebunden.</span><span class="sxs-lookup"><span data-stu-id="823a7-234">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="823a7-235">IPv6-Adresse mit Port-Nummer</span><span class="sxs-lookup"><span data-stu-id="823a7-235">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  <span data-ttu-id="823a7-236">[::] ist das Äquivalent IPv6 IPv4 0.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="823a7-236">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="823a7-237">Hostname und Portnummer</span><span class="sxs-lookup"><span data-stu-id="823a7-237">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  <span data-ttu-id="823a7-238">Hostnamen, \*, und + sind spezielle nicht.</span><span class="sxs-lookup"><span data-stu-id="823a7-238">Host names, \*, and + aren't special.</span></span> <span data-ttu-id="823a7-239">Einen erkannten IP-Adresse oder "Localhost" nicht für alle IPv4- und IPv6-IP-Adressen gebunden.</span><span class="sxs-lookup"><span data-stu-id="823a7-239">Anything that isn't a recognized IP address or "localhost" binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="823a7-240">Wenn Sie unterschiedliche Hostnamen an verschiedenen ASP.NET Core-Anwendungen auf demselben Port binden, verwenden [WebListener](weblistener.md) oder einen reverse-Proxy-Server wie IIS, Nginx oder Apache.</span><span class="sxs-lookup"><span data-stu-id="823a7-240">If you need to bind different host names to different ASP.NET Core applications on the same port, use [WebListener](weblistener.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="823a7-241">Name von "Localhost" mit Portnummer Port Nummer oder Loopback-IP</span><span class="sxs-lookup"><span data-stu-id="823a7-241">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="823a7-242">Wenn `localhost` angegeben wird, Kestrel versucht, IPv4 und IPv6-Loopback-Schnittstellen zu binden.</span><span class="sxs-lookup"><span data-stu-id="823a7-242">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="823a7-243">Wenn es sich bei der angeforderte Port verwendet, die von einem anderen Dienst entweder Loopback-Schnittstelle wird, nicht Kestrel gestartet.</span><span class="sxs-lookup"><span data-stu-id="823a7-243">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="823a7-244">Wenn der Loopback-Schnittstellen für andere Zwecke nicht verfügbar ist (die meisten häufig verwendet werden, weil IPv6 nicht unterstützt wird), Kestrel protokolliert eine Warnung.</span><span class="sxs-lookup"><span data-stu-id="823a7-244">If either loopback interface is unavailable for any other reason (most commonly because IPv6 is not supported), Kestrel logs a warning.</span></span> 

* <span data-ttu-id="823a7-245">UNIX-socket</span><span class="sxs-lookup"><span data-stu-id="823a7-245">Unix socket</span></span>

  ```
  http://unix:/run/dan-live.sock  
  ```

<span data-ttu-id="823a7-246">**Port 0**</span><span class="sxs-lookup"><span data-stu-id="823a7-246">**Port 0**</span></span>

<span data-ttu-id="823a7-247">Wenn Sie die Portnummer 0 angeben, wird die Kestrel dynamisch auf einen verfügbaren Port gebunden.</span><span class="sxs-lookup"><span data-stu-id="823a7-247">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="823a7-248">Bindung an port 0 kann für alle Hostnamen oder die IP-Adresse außer für `localhost` Name.</span><span class="sxs-lookup"><span data-stu-id="823a7-248">Binding to port 0 is allowed for any host name or IP except for `localhost` name.</span></span>

<span data-ttu-id="823a7-249">Das folgende Beispiel zeigt, wie Sie den Port ermittelt haben Kestrel tatsächlich zur Laufzeit gebunden:</span><span class="sxs-lookup"><span data-stu-id="823a7-249">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

<span data-ttu-id="823a7-250">**URL-Präfixe für SSL**</span><span class="sxs-lookup"><span data-stu-id="823a7-250">**URL prefixes for SSL**</span></span>

<span data-ttu-id="823a7-251">Achten Sie darauf, dass Sie mit URL-Präfixe enthalten `https:` beim Aufrufen der `UseHttps` Erweiterungsmethode, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="823a7-251">Be sure to include URL prefixes with `https:` if you call the `UseHttps` extension method, as shown below.</span></span>

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
> <span data-ttu-id="823a7-252">HTTPS und HTTP darf nicht auf demselben Port gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="823a7-252">HTTPS and HTTP cannot be hosted on the same port.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

---
## <a name="next-steps"></a><span data-ttu-id="823a7-253">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="823a7-253">Next steps</span></span>

<span data-ttu-id="823a7-254">Weitere Informationen finden Sie in den folgenden Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="823a7-254">For more information, see the following resources:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="823a7-255">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="823a7-255">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* [<span data-ttu-id="823a7-256">Beispiel-app für 2.x</span><span class="sxs-lookup"><span data-stu-id="823a7-256">Sample app for 2.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [<span data-ttu-id="823a7-257">Kestrel-Quellcode</span><span class="sxs-lookup"><span data-stu-id="823a7-257">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="823a7-258">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="823a7-258">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* [<span data-ttu-id="823a7-259">Beispiel-app für 1.x</span><span class="sxs-lookup"><span data-stu-id="823a7-259">Sample app for 1.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [<span data-ttu-id="823a7-260">Kestrel-Quellcode</span><span class="sxs-lookup"><span data-stu-id="823a7-260">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

---
