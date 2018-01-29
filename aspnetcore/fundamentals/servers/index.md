---
title: Webserverimplementierungen in ASP.NET Core
author: tdykstra
description: "Enthält eine einführende Beschreibung der Webserver Kestrel und WebListener für ASP.NET Core. Enthält Hinweise zur Auswahl eines Webservers und zu dessen Verwendung mit einem Reverseproxyserver."
ms.author: tdykstra
manager: wpickett
ms.date: 08/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/index
ms.openlocfilehash: 750b81c2c715598dbcfb6671e34016e30c1878d6
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="4e610-104">Webserverimplementierungen in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4e610-104">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="4e610-105">Von [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73) und [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="4e610-105">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="4e610-106">Eine ASP.NET Core-Anwendung wird über eine In-Process-Implementierung eines HTTP-Servers ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="4e610-106">An ASP.NET Core application runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="4e610-107">Die Serverimplementierung lauscht auf HTTP-Anforderungen und leitet diese als [Anforderungsfunktionen](https://docs.microsoft.com/aspnet/core/fundamentals/request-features), die in einem `HttpContext` zusammengefasst werden, an die Anwendung weiter.</span><span class="sxs-lookup"><span data-stu-id="4e610-107">The server implementation listens for HTTP requests and surfaces them to the application as sets of [request features](https://docs.microsoft.com/aspnet/core/fundamentals/request-features) composed into an `HttpContext`.</span></span>

<span data-ttu-id="4e610-108">ASP.NET Core stellt zwei Serverimplementierungen zur Verfügung:</span><span class="sxs-lookup"><span data-stu-id="4e610-108">ASP.NET Core ships two server implementations:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4e610-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4e610-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="4e610-110">[Kestrel](kestrel.md) ist ein plattformübergreifender HTTP-Server, der auf der plattformübergreifenden asynchronen E/A-Bibliothek [libuv](https://github.com/libuv/libuv) basiert.</span><span class="sxs-lookup"><span data-stu-id="4e610-110">[Kestrel](kestrel.md) is a cross-platform HTTP server based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span>

* <span data-ttu-id="4e610-111">[HTTP.sys](httpsys.md) ist ein nur für Windows verfügbarer HTTP-Server, der auf dem [Http.sys-Kerneltreiber ](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx) basiert.</span><span class="sxs-lookup"><span data-stu-id="4e610-111">[HTTP.sys](httpsys.md) is a Windows-only HTTP server based on the [Http.Sys kernel driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4e610-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4e610-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="4e610-113">[Kestrel](kestrel.md) ist ein plattformübergreifender HTTP-Server, der auf der plattformübergreifenden asynchronen E/A-Bibliothek [libuv](https://github.com/libuv/libuv) basiert.</span><span class="sxs-lookup"><span data-stu-id="4e610-113">[Kestrel](kestrel.md) is a cross-platform HTTP server based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span>

* <span data-ttu-id="4e610-114">[WebListener](weblistener.md) ist ein nur für Windows verfügbarer HTTP-Server, der auf dem [Http.sys-Kerneltreiber ](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx) basiert.</span><span class="sxs-lookup"><span data-stu-id="4e610-114">[WebListener](weblistener.md) is a Windows-only HTTP server based on the [Http.Sys kernel driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span>

---

## <a name="kestrel"></a><span data-ttu-id="4e610-115">Kestrel</span><span class="sxs-lookup"><span data-stu-id="4e610-115">Kestrel</span></span>

<span data-ttu-id="4e610-116">Kestrel ist der Webserver, der standardmäßig in ASP.NET Core in Vorlagen für neue Projekte enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="4e610-116">Kestrel is the web server that's included by default in ASP.NET Core new-project templates.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4e610-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4e610-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="4e610-118">Sie können Kestrel als eigenständigen Webserver oder mit einem *Reverseproxyserver* wie IIS, Nginx oder Apache verwenden.</span><span class="sxs-lookup"><span data-stu-id="4e610-118">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="4e610-119">Ein Reverseproxyserver empfängt HTTP-Anforderungen aus dem Internet und leitet diese nach einer vorbereitenden Verarbeitung an Kestrel weiter.</span><span class="sxs-lookup"><span data-stu-id="4e610-119">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel kommuniziert direkt und ohne Reverseproxyserver mit dem Internet](kestrel/_static/kestrel-to-internet2.png)

![Kestrel kommuniziert indirekt mit dem Internet über einen Reverseproxyserver wie IIS, Nginx oder Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="4e610-122">Jede der beiden Konfigurationen &mdash;, egal ob mit oder ohne Reverseproxyserver &mdash;, kann auch dann verwendet werden, wenn Kestrel nur in einem internen Netzwerk verfügbar gemacht wird.</span><span class="sxs-lookup"><span data-stu-id="4e610-122">Either configuration &mdash; with or without a reverse proxy server &mdash; can also be used if Kestrel is exposed only to an internal network.</span></span>

<span data-ttu-id="4e610-123">Informationen zur sinnvollen Verwendung von Kestrel mit einem Reverseproxy finden Sie unter [Introduction to Kestrel (Einführung in Kestrel)](kestrel.md).</span><span class="sxs-lookup"><span data-stu-id="4e610-123">For information about when to use Kestrel with a reverse proxy, see [Introduction to Kestrel](kestrel.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4e610-124">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4e610-124">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4e610-125">Wenn Ihre Anwendung nur Anforderungen aus einem internen Netzwerk akzeptiert, können Sie Kestrel als eigenständigen Webserver verwenden.</span><span class="sxs-lookup"><span data-stu-id="4e610-125">If your application accepts requests only from an internal network, you can use Kestrel by itself.</span></span>

![Kestrel kommuniziert direkt mit Ihrem internen Netzwerk](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="4e610-127">Wenn Sie Ihre Anwendung im Internet verfügbar machen, müssen Sie IIS, Nginx oder Apache als *Reverseproxyserver* verwenden.</span><span class="sxs-lookup"><span data-stu-id="4e610-127">If you expose your application to the Internet, you must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="4e610-128">Ein Reverseproxyserver empfängt HTTP-Anforderungen aus dem Internet und leitet diese wie im folgenden Diagramm dargestellt nach einer vorbereitenden Verarbeitung an Kestrel weiter.</span><span class="sxs-lookup"><span data-stu-id="4e610-128">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling, as shown in the following diagram.</span></span>

![Kestrel kommuniziert indirekt mit dem Internet über einen Reverseproxyserver wie IIS, Nginx oder Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="4e610-130">Der wichtigste Grund für die Verwendung eines Reverseproxys für Edge-Bereitstellungen, die im Internet verfügbar gemacht werden, ist die IT-Sicherheit.</span><span class="sxs-lookup"><span data-stu-id="4e610-130">The most important reason for using a reverse proxy for edge deployments (exposed to traffic from the Internet) is security.</span></span> <span data-ttu-id="4e610-131">Die Kestrel-Versionen 1.x besitzen keine umfassenden Schutzmaßnahmen gegenüber Angriffen.</span><span class="sxs-lookup"><span data-stu-id="4e610-131">The 1.x versions of Kestrel don't have a full complement of defenses against attacks.</span></span> <span data-ttu-id="4e610-132">Geeignete Timeouts und Größenlimits sowie eine Beschränkung der Anzahl gleichzeitiger Verbindungen sind beispielsweise nicht vorhanden.</span><span class="sxs-lookup"><span data-stu-id="4e610-132">This includes, but isn't limited to, appropriate timeouts, size limits, and concurrent connection limits.</span></span>

<span data-ttu-id="4e610-133">Informationen zur sinnvollen Verwendung von Kestrel mit einem Reverseproxy finden Sie unter [Introduction to Kestrel (Einführung in Kestrel)](kestrel.md).</span><span class="sxs-lookup"><span data-stu-id="4e610-133">For information about when to use Kestrel with a reverse proxy, see [Introduction to Kestrel](kestrel.md).</span></span>

---

<span data-ttu-id="4e610-134">Sie können IIS, Nginx oder Apache nicht ohne Kestrel oder eine [benutzerdefinierte Serverimplementierung](#custom-servers) verwenden.</span><span class="sxs-lookup"><span data-stu-id="4e610-134">You can't use IIS, Nginx, or Apache without Kestrel or a [custom server implementation](#custom-servers).</span></span> <span data-ttu-id="4e610-135">ASP.NET Core wurde entwickelt, um in einem eigenen Prozess ausgeführt werden zu können, sodass plattformübergreifend konsistentes Verhalten gewährleistet wird.</span><span class="sxs-lookup"><span data-stu-id="4e610-135">ASP.NET Core was designed to run in its own process so that it can behave consistently across platforms.</span></span> <span data-ttu-id="4e610-136">IIS, Nginx und Apache legen ihren eigenen Startprozess und ihre eigene Umgebung fest. Zur direkten Verwendung dieser Webserver müsste ASP.NET Core an die jeweilige Webserversoftware angepasst werden.</span><span class="sxs-lookup"><span data-stu-id="4e610-136">IIS, Nginx, and Apache dictate their own startup process and environment; to use them directly, ASP.NET Core would have to adapt to the needs of each one.</span></span> <span data-ttu-id="4e610-137">Durch eine Webserverimplementierung wie Kestrel kann ASP.NET Core den Startprozess und die Umgebung steuern.</span><span class="sxs-lookup"><span data-stu-id="4e610-137">Using a web server implementation such as Kestrel gives ASP.NET Core control over the startup process and environment.</span></span> <span data-ttu-id="4e610-138">Sie müssen daher nicht ASP.NET Core an IIS, Nginx oder Apache anpassen, sondern die Webserver nur so einrichten, dass sie als Proxy Anforderungen an Kestrel weiterleiten.</span><span class="sxs-lookup"><span data-stu-id="4e610-138">So rather than trying to adapt ASP.NET Core to IIS, Nginx, or Apache, you just set up those web servers to proxy requests to Kestrel.</span></span> <span data-ttu-id="4e610-139">Mit dieser Konfiguration können fast identische `Program.Main`- und `Startup`-Klassen unabhängig vom Bereitstellungsszenario verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="4e610-139">This arrangement allows your `Program.Main` and `Startup` classes to be essentially the same no matter where you deploy.</span></span>

### <a name="iis-with-kestrel"></a><span data-ttu-id="4e610-140">IIS und Kestrel</span><span class="sxs-lookup"><span data-stu-id="4e610-140">IIS with Kestrel</span></span>

<span data-ttu-id="4e610-141">Wenn Sie IIS oder IIS Express als Reverseproxy für ASP.NET Core verwenden, wird die ASP.NET Core-Anwendung in einem Prozess ausgeführt, der vom IIS-Workerprozess getrennt ist.</span><span class="sxs-lookup"><span data-stu-id="4e610-141">When you use IIS or IIS Express as a reverse proxy for ASP.NET Core, the ASP.NET Core application runs in a process separate from the IIS worker process.</span></span> <span data-ttu-id="4e610-142">Im IIS-Prozess wird ein spezielles IIS-Modul ausgeführt, das die Beziehung zum Reverseproxy steuert.</span><span class="sxs-lookup"><span data-stu-id="4e610-142">In the IIS process, a special IIS module runs to coordinate the reverse proxy relationship.</span></span>  <span data-ttu-id="4e610-143">Hierbei handelt es sich um das *ASP.NET Core-Modul*.</span><span class="sxs-lookup"><span data-stu-id="4e610-143">This is the *ASP.NET Core Module*.</span></span> <span data-ttu-id="4e610-144">Die Hauptfunktionen des ASP.NET Core-Moduls bestehen darin, die ASP.NET Core-Anwendung zu starten, bei einem Absturz neu zu starten und HTTP-Datenverkehr an sie weiterzuleiten.</span><span class="sxs-lookup"><span data-stu-id="4e610-144">The primary functions of the ASP.NET Core Module are to start the ASP.NET Core application, restart it when it crashes, and forward HTTP traffic to it.</span></span> <span data-ttu-id="4e610-145">Weitere Informationen finden Sie unter [ASP.NET Core Module (ASP.NET Core-Modul)](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="4e610-145">For more information, see [ASP.NET Core Module](aspnet-core-module.md).</span></span> 

### <a name="nginx-with-kestrel"></a><span data-ttu-id="4e610-146">Nginx und Kestrel</span><span class="sxs-lookup"><span data-stu-id="4e610-146">Nginx with Kestrel</span></span>

<span data-ttu-id="4e610-147">Informationen zur Verwendung von Nginx unter Linux als Reverseproxyserver für Kestrel finden Sie unter [Hosten unter Linux mit Nginx](xref:host-and-deploy/linux-nginx).</span><span class="sxs-lookup"><span data-stu-id="4e610-147">For information about how to use Nginx on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Nginx](xref:host-and-deploy/linux-nginx).</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="4e610-148">Apache und Kestrel</span><span class="sxs-lookup"><span data-stu-id="4e610-148">Apache with Kestrel</span></span>

<span data-ttu-id="4e610-149">Informationen zur Verwendung von Apache unter Linux als Reverseproxyserver für Kestrel finden Sie unter [Hosten unter Linux mit Apache](xref:host-and-deploy/linux-apache).</span><span class="sxs-lookup"><span data-stu-id="4e610-149">For information about how to use Apache on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Apache](xref:host-and-deploy/linux-apache).</span></span>

## <a name="httpsys"></a><span data-ttu-id="4e610-150">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="4e610-150">HTTP.sys</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4e610-151">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4e610-151">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="4e610-152">Wenn Sie eine ASP.NET Core-Anwendung unter Windows ausführen, ist HTTP.sys eine Alternative zu Kestrel.</span><span class="sxs-lookup"><span data-stu-id="4e610-152">If you run your ASP.NET Core app on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="4e610-153">Sie können HTTP.sys in Szenarios einsetzen, in denen Sie Ihre Anwendung über das Internet verfügbar machen und HTTP.sys-Funktionen benötigen, die Kestrel nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="4e610-153">You can use HTTP.sys for scenarios where you expose your app to the Internet and you need HTTP.sys features that Kestrel doesn't support.</span></span> 

![HTTP.sys kommuniziert direkt mit dem Internet](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="4e610-155">Außerdem kann HTTP.sys auch bei Anwendungen zum Einsatz kommen, die nur in einem internen Netzwerk verfügbar gemacht werden.</span><span class="sxs-lookup"><span data-stu-id="4e610-155">HTTP.sys can also be used for applications that are exposed only to an internal network.</span></span> 

![HTTP.sys kommuniziert direkt mit Ihrem internen Netzwerk](httpsys/_static/httpsys-to-internal.png)

<span data-ttu-id="4e610-157">Für Szenarios mit einem internen Netzwerk wird im Allgemeinen Kestrel für eine optimale Leistung empfohlen. In einigen Szenarios sollten Sie jedoch eine Funktion verwenden, die nur HTTP.sys zur Verfügung stellt.</span><span class="sxs-lookup"><span data-stu-id="4e610-157">For internal network scenarios, Kestrel is generally recommended for best performance; but in some scenarios, you might want to use a feature that only HTTP.sys offers.</span></span> <span data-ttu-id="4e610-158">Informationen zu HTTP.sys-Funktionen finden Sie unter [HTTP.sys](httpsys.md).</span><span class="sxs-lookup"><span data-stu-id="4e610-158">For information about HTTP.sys features, see [HTTP.sys](httpsys.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4e610-159">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4e610-159">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4e610-160">HTTP.sys wird in ASP.NET Core 1.x WebListener genannt.</span><span class="sxs-lookup"><span data-stu-id="4e610-160">HTTP.sys is named WebListener in ASP.NET Core 1.x.</span></span> <span data-ttu-id="4e610-161">Wenn Sie eine ASP.NET Core-Anwendung unter Windows ausführen, können Sie alternativ WebListener in Szenarios verwenden, in denen Sie Ihre Anwendung über das Internet verfügbar machen möchten, jedoch nicht IIS verwenden können.</span><span class="sxs-lookup"><span data-stu-id="4e610-161">If you run your ASP.NET Core app on Windows, WebListener is an alternative that you can use for scenarios where you want to expose your app to the Internet but you can't use IIS.</span></span>

![WebListener kommuniziert direkt mit dem Internet](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="4e610-163">WebListener kann auch anstelle von Kestrel für Anwendungen verwendet werden, die nur in einem internen Netzwerk verfügbar gemacht werden, wenn Sie WebListener-Funktionen benötigen, die Kestrel nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="4e610-163">WebListener can also be used in place of Kestrel for applications that are exposed only to an internal network, if you need WebListener features that Kestrel doesn't support.</span></span> 

![WebListener kommuniziert direkt mit Ihrem internen Netzwerk](weblistener/_static/weblistener-to-internal.png)

<span data-ttu-id="4e610-165">Für Szenarios mit einem interne Netzwerk wird im Allgemeinen Kestrel für eine optimale Leistung empfohlen. In einigen Szenarios sollten Sie jedoch eine Funktion verwenden, die nur WebListener zur Verfügung stellt.</span><span class="sxs-lookup"><span data-stu-id="4e610-165">For internal network scenarios, Kestrel is generally recommended for best performance, but in some scenarios you might want to use a feature that only WebListener offers.</span></span> <span data-ttu-id="4e610-166">Informationen zu WebListener-Funktionen finden Sie unter [WebListener](weblistener.md).</span><span class="sxs-lookup"><span data-stu-id="4e610-166">For information about WebListener features, see [WebListener](weblistener.md).</span></span>

---

## <a name="notes-about-aspnet-core-server-infrastructure"></a><span data-ttu-id="4e610-167">Hinweise zur ASP.NET Core-Serverinfrastruktur</span><span class="sxs-lookup"><span data-stu-id="4e610-167">Notes about ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="4e610-168">Die Schnittstelle [`IApplicationBuilder`](/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder), die in der `Startup`-Klasse in der `Configure`-Methode vorhanden ist, macht die Eigenschaft `ServerFeatures` vom Typ [`IFeatureCollection`](/aspnet/core/api/microsoft.aspnetcore.http.features.ifeaturecollection) verfügbar.</span><span class="sxs-lookup"><span data-stu-id="4e610-168">The [`IApplicationBuilder`](/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder) available in the `Startup` class `Configure` method exposes the `ServerFeatures` property of type [`IFeatureCollection`](/aspnet/core/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span></span> <span data-ttu-id="4e610-169">Kestrel und WebListener machen nur die Funktion [`IServerAddressesFeature`](/aspnet/core/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) verfügbar. Unterschiedliche Serverimplementierungen können jedoch möglicherweise zusätzliche Funktionalitäten verfügbar machen.</span><span class="sxs-lookup"><span data-stu-id="4e610-169">Kestrel and WebListener both expose only a single feature, [`IServerAddressesFeature`](/aspnet/core/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="4e610-170">Mit `IServerAddressesFeature` kann ermittelt werden, an welchen Port die Serverimplementierung zur Laufzeit gebunden ist.</span><span class="sxs-lookup"><span data-stu-id="4e610-170">`IServerAddressesFeature` can be used to find out which port the server implementation has bound to at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="4e610-171">Benutzerdefinierte Server</span><span class="sxs-lookup"><span data-stu-id="4e610-171">Custom servers</span></span>

<span data-ttu-id="4e610-172">Wenn die integrierten Server Ihren Anforderungen nicht gerecht werden, können Sie eine benutzerdefinierte Serverimplementierung erstellen.</span><span class="sxs-lookup"><span data-stu-id="4e610-172">If the built-in servers don't meet your needs, you can create a custom server implementation.</span></span> <span data-ttu-id="4e610-173">In der [Anleitung zu Open Web Interface for .NET (OWIN)](../owin.md) wird erläutert, wie eine auf [Nowin](https://github.com/Bobris/Nowin) basierende [IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver)-Implementierung erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="4e610-173">The [Open Web Interface for .NET (OWIN) guide](../owin.md) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based [IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) implementation.</span></span> <span data-ttu-id="4e610-174">Sie haben selbstverständlich die Möglichkeit, nur die Funktionsschnittstellen zu implementieren, die für Ihre Anwendung erforderlich sind. [IHttpRequestFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttprequestfeature) und [IHttpResponseFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) müssen jedoch auf jeden Fall unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="4e610-174">You're free to implement just the feature interfaces your application needs, though at a minimum you must support [IHttpRequestFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttprequestfeature) and [IHttpResponseFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpresponsefeature).</span></span>

## <a name="next-steps"></a><span data-ttu-id="4e610-175">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="4e610-175">Next steps</span></span>

<span data-ttu-id="4e610-176">Weitere Informationen finden Sie in den folgenden Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="4e610-176">For more information, see the following resources:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4e610-177">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4e610-177">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

- [<span data-ttu-id="4e610-178">Kestrel</span><span class="sxs-lookup"><span data-stu-id="4e610-178">Kestrel</span></span>](kestrel.md)
- [<span data-ttu-id="4e610-179">Kestrel with IIS (Kestrel und IIS)</span><span class="sxs-lookup"><span data-stu-id="4e610-179">Kestrel with IIS</span></span>](aspnet-core-module.md)
- [<span data-ttu-id="4e610-180">Hosten unter Linux mit Nginx</span><span class="sxs-lookup"><span data-stu-id="4e610-180">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
- [<span data-ttu-id="4e610-181">Hosten unter Linux mit Apache</span><span class="sxs-lookup"><span data-stu-id="4e610-181">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
- [<span data-ttu-id="4e610-182">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="4e610-182">HTTP.sys</span></span>](httpsys.md)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4e610-183">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4e610-183">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

- [<span data-ttu-id="4e610-184">Kestrel</span><span class="sxs-lookup"><span data-stu-id="4e610-184">Kestrel</span></span>](kestrel.md)
- [<span data-ttu-id="4e610-185">Kestrel with IIS (Kestrel und IIS)</span><span class="sxs-lookup"><span data-stu-id="4e610-185">Kestrel with IIS</span></span>](aspnet-core-module.md)
- [<span data-ttu-id="4e610-186">Hosten unter Linux mit Nginx</span><span class="sxs-lookup"><span data-stu-id="4e610-186">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
- [<span data-ttu-id="4e610-187">Hosten unter Linux mit Apache</span><span class="sxs-lookup"><span data-stu-id="4e610-187">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
- [<span data-ttu-id="4e610-188">WebListener</span><span class="sxs-lookup"><span data-stu-id="4e610-188">WebListener</span></span>](weblistener.md)

---
