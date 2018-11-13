---
title: Webserverimplementierungen in ASP.NET Core
author: guardrex
description: Ermitteln Sie die Webserver Kestrel und HTTP.sys für ASP.NET Core. Erfahren Sie mehr über das Auswählen eines Servers und darüber, wann ein Reverseproxyserver zu verwenden ist.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/21/2018
uid: fundamentals/servers/index
ms.openlocfilehash: 06d4bf09b07fc70a10b3e260e78c29fe189486c5
ms.sourcegitcommit: edb9d2d78c9a4d68b397e74ae2aff088b325a143
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2018
ms.locfileid: "51505725"
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="c2a71-104">Webserverimplementierungen in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c2a71-104">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="c2a71-105">Von [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73) und [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="c2a71-105">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="c2a71-106">Eine ASP.NET Core-App wird über eine In-Process-Implementierung eines HTTP-Servers ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="c2a71-106">An ASP.NET Core app runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="c2a71-107">Die Serverimplementierung lauscht auf HTTP-Anforderungen und leitet diese als Gruppen von [Anforderungsfunktionen](xref:fundamentals/request-features), die in einem <xref:Microsoft.AspNetCore.Http.HttpContext> zusammengefasst werden, an die App weiter.</span><span class="sxs-lookup"><span data-stu-id="c2a71-107">The server implementation listens for HTTP requests and surfaces them to the app as sets of [request features](xref:fundamentals/request-features) composed into an <xref:Microsoft.AspNetCore.Http.HttpContext>.</span></span>

<span data-ttu-id="c2a71-108">Die folgenden Serverimplementierungen sind im Lieferumfang von ASP.NET Core enthalten:</span><span class="sxs-lookup"><span data-stu-id="c2a71-108">ASP.NET Core ships with the following server implementations:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="c2a71-109">[Kestrel](xref:fundamentals/servers/kestrel) ist der plattformübergreifende HTTP-Standardserver für ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c2a71-109">[Kestrel](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server for ASP.NET Core.</span></span>
* <span data-ttu-id="c2a71-110">`IISHttpServer` wird mit dem [In-Process-Hostingmodell](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model) und dem [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) unter Windows verwendet.</span><span class="sxs-lookup"><span data-stu-id="c2a71-110">`IISHttpServer` is used with the [in-process hosting model](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model) and the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) on Windows.</span></span>
* <span data-ttu-id="c2a71-111">[HTTP.sys](xref:fundamentals/servers/httpsys) ist ein nur für Windows verfügbarer HTTP-Server, der auf dem [Http.sys-Kerneltreiber und der HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx) basiert.</span><span class="sxs-lookup"><span data-stu-id="c2a71-111">[HTTP.sys](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="c2a71-112">(HTTP.sys wird in ASP.NET Core 1.x als [WebListener](xref:fundamentals/servers/weblistener) bezeichnet.)</span><span class="sxs-lookup"><span data-stu-id="c2a71-112">(HTTP.sys is called [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="c2a71-113">[Kestrel](xref:fundamentals/servers/kestrel) ist der plattformübergreifende HTTP-Standardserver für ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c2a71-113">[Kestrel](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server for ASP.NET Core.</span></span>
* <span data-ttu-id="c2a71-114">[HTTP.sys](xref:fundamentals/servers/httpsys) ist ein nur für Windows verfügbarer HTTP-Server, der auf dem [Http.sys-Kerneltreiber und der HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx) basiert.</span><span class="sxs-lookup"><span data-stu-id="c2a71-114">[HTTP.sys](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="c2a71-115">(HTTP.sys wird in ASP.NET Core 1.x als [WebListener](xref:fundamentals/servers/weblistener) bezeichnet.)</span><span class="sxs-lookup"><span data-stu-id="c2a71-115">(HTTP.sys is called [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.)</span></span>

::: moniker-end

## <a name="kestrel"></a><span data-ttu-id="c2a71-116">Kestrel</span><span class="sxs-lookup"><span data-stu-id="c2a71-116">Kestrel</span></span>

<span data-ttu-id="c2a71-117">Kestrel ist der Standardwebserver, der in ASP.NET Core-Projektvorlagen enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="c2a71-117">Kestrel is the default web server included in ASP.NET Core project templates.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="c2a71-118">Kestrel kann eigenständig oder mit einem *Reverseproxyserver* wie IIS, Nginx oder Apache verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="c2a71-118">Kestrel can be used by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="c2a71-119">Ein Reverseproxyserver empfängt HTTP-Anforderungen aus dem Internet und leitet diese nach einer vorbereitenden Verarbeitung an Kestrel weiter.</span><span class="sxs-lookup"><span data-stu-id="c2a71-119">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel kommuniziert direkt und ohne Reverseproxyserver mit dem Internet](kestrel/_static/kestrel-to-internet2.png)

![Kestrel kommuniziert indirekt mit dem Internet über einen Reverseproxyserver wie IIS, Nginx oder Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="c2a71-122">Jede der beiden Konfigurationen &mdash;mit oder ohne einen Reverseproxyserver&mdash; ist eine gültige und unterstützte Hostingkonfiguration für ASP.NET Core 2.0 oder neuere Apps.</span><span class="sxs-lookup"><span data-stu-id="c2a71-122">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="c2a71-123">Weitere Informationen finden Sie unter [When to use Kestrel with a reverse proxy (Verwenden von Kestrel mit einem Reverseproxy)](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="c2a71-123">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="c2a71-124">Wenn die App nur Anforderungen aus einem internen Netzwerk akzeptiert, kann Kestrel eigenständig verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="c2a71-124">If the app only accepts requests from an internal network, Kestrel can be used by itself.</span></span>

![Kestrel kommuniziert direkt mit dem internen Netzwerk.](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="c2a71-126">Wenn die App für das Internet verfügbar gemacht ist, muss Kestrel IIS, Nginx oder Apache als *Reverseproxyserver* verwenden.</span><span class="sxs-lookup"><span data-stu-id="c2a71-126">If the app is exposed to the Internet, Kestrel must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="c2a71-127">Ein Reverseproxyserver empfängt HTTP-Anforderungen aus dem Internet und leitet diese wie im folgenden Diagramm dargestellt nach einer vorbereitenden Verarbeitung an Kestrel weiter:</span><span class="sxs-lookup"><span data-stu-id="c2a71-127">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling, as shown in the following diagram:</span></span>

![Kestrel kommuniziert indirekt mit dem Internet über einen Reverseproxyserver wie IIS, Nginx oder Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="c2a71-129">Der wichtigste Grund für die Verwendung eines Reverseproxys für öffentlich zugängliche Edge-Server-Bereitstellungen, die direkt im Internet verfügbar gemacht werden, ist die Sicherheit.</span><span class="sxs-lookup"><span data-stu-id="c2a71-129">The most important reason for using a reverse proxy for public-facing edge server deployments that are exposed directly the Internet is security.</span></span> <span data-ttu-id="c2a71-130">Die 1.x-Versionen von Kestrel haben keine wesentlichen Sicherheitsfeatures zum Schutz vor Angriffen aus dem Internet.</span><span class="sxs-lookup"><span data-stu-id="c2a71-130">The 1.x versions of Kestrel don't have important security features to defend against attacks from the Internet.</span></span> <span data-ttu-id="c2a71-131">So sind z. B. keine geeigneten Timeouts, Größenlimits für Anforderungen sowie Beschränkungen der Anzahl gleichzeitiger Verbindungen vorhanden.</span><span class="sxs-lookup"><span data-stu-id="c2a71-131">This includes, but isn't limited to, appropriate timeouts, request size limits, and concurrent connection limits.</span></span>

<span data-ttu-id="c2a71-132">Weitere Informationen finden Sie unter [When to use Kestrel with a reverse proxy (Verwenden von Kestrel mit einem Reverseproxy)](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="c2a71-132">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

<span data-ttu-id="c2a71-133">IIS, Nginx oder Apache können nicht ohne Kestrel oder eine [benutzerdefinierte Serverimplementierung](#custom-servers) verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="c2a71-133">IIS, Nginx, and Apache can't be used without Kestrel or a [custom server implementation](#custom-servers).</span></span> <span data-ttu-id="c2a71-134">ASP.NET Core wurde entwickelt, um in einem eigenen Prozess ausgeführt werden zu können, sodass plattformübergreifend konsistentes Verhalten gewährleistet wird.</span><span class="sxs-lookup"><span data-stu-id="c2a71-134">ASP.NET Core was designed to run in its own process so that it can behave consistently across platforms.</span></span> <span data-ttu-id="c2a71-135">IIS, Nginx und Apache geben ihre eigene Startprozedur und Umgebung vor.</span><span class="sxs-lookup"><span data-stu-id="c2a71-135">IIS, Nginx, and Apache dictate their own startup procedure and environment.</span></span> <span data-ttu-id="c2a71-136">Um diese Servertechnologien direkt nutzen zu können, muss ASP.NET Core an die Anforderungen jedes Servers angepasst werden.</span><span class="sxs-lookup"><span data-stu-id="c2a71-136">To use these server technologies directly, ASP.NET Core would need to adapt to the requirements of each server.</span></span> <span data-ttu-id="c2a71-137">Durch eine Webserverimplementierung wie Kestrel kann ASP.NET Core den Startprozess und die Umgebung steuern, wenn es auf anderen Servertechnologien gehostet wird.</span><span class="sxs-lookup"><span data-stu-id="c2a71-137">Using a web server implementation, such as Kestrel, ASP.NET Core has control over the startup process and environment when hosted on different server technologies.</span></span>

### <a name="iis-with-kestrel"></a><span data-ttu-id="c2a71-138">IIS und Kestrel</span><span class="sxs-lookup"><span data-stu-id="c2a71-138">IIS with Kestrel</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="c2a71-139">Bei Verwendung von [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) oder [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) wird die ASP.NET Core-App entweder im selben Prozess wie der IIS-Workerprozess ausgeführt (das *In-Process*-Hostingmodell) oder in einem vom IIS-Workerprozess getrennten Prozess (das *Out-of-Process*-Hostingmodell).</span><span class="sxs-lookup"><span data-stu-id="c2a71-139">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the ASP.NET Core app either runs in the same process as the IIS worker process (the *in-process* hosting model) or in a process separate from the IIS worker process (the *out-of-process* hosting model).</span></span>

<span data-ttu-id="c2a71-140">Das [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) ist ein natives IIS-Modul, das native IIS-Anforderungen zwischen dem In-Process-IIS-HTTP-Server oder dem Out-of-Process-Kestrel-Server verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="c2a71-140">The [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) is a native IIS module that handles native IIS requests between either the in-process IIS Http Server or the out-of-process Kestrel server.</span></span> <span data-ttu-id="c2a71-141">Weitere Informationen finden Sie unter <xref:fundamentals/servers/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="c2a71-141">For more information, see <xref:fundamentals/servers/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="c2a71-142">Wenn [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) oder [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) als Reverseproxy für ASP.NET Core verwendet wird, wird die ASP.NET Core-App in einem Prozess ausgeführt, der vom IIS-Workerprozess getrennt ist.</span><span class="sxs-lookup"><span data-stu-id="c2a71-142">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) as a reverse proxy for ASP.NET Core, the ASP.NET Core app runs in a process separate from the IIS worker process.</span></span> <span data-ttu-id="c2a71-143">Im IIS-Prozess steuert das [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) die Beziehung zum Reverseproxy.</span><span class="sxs-lookup"><span data-stu-id="c2a71-143">In the IIS process, the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) coordinates the reverse proxy relationship.</span></span> <span data-ttu-id="c2a71-144">Die Hauptfunktionen des ASP.NET Core-Moduls bestehen darin, die ASP.NET Core-App zu starten, die App nach einem Absturz neu zu starten und HTTP-Datenverkehr an die App weiterzuleiten.</span><span class="sxs-lookup"><span data-stu-id="c2a71-144">The primary functions of the ASP.NET Core Module are to start the ASP.NET Core app, restart the app when it crashes, and forward HTTP traffic to the app.</span></span> <span data-ttu-id="c2a71-145">Weitere Informationen finden Sie unter <xref:fundamentals/servers/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="c2a71-145">For more information, see <xref:fundamentals/servers/aspnet-core-module>.</span></span>

::: moniker-end

### <a name="nginx-with-kestrel"></a><span data-ttu-id="c2a71-146">Nginx und Kestrel</span><span class="sxs-lookup"><span data-stu-id="c2a71-146">Nginx with Kestrel</span></span>

<span data-ttu-id="c2a71-147">Informationen dazu, wie Nginx unter Linux als Reverseproxyserver für Kestrel verwendet wird, finden Sie unter [Hosten unter Linux mit Nginx](xref:host-and-deploy/linux-nginx).</span><span class="sxs-lookup"><span data-stu-id="c2a71-147">For information on how to use Nginx on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Nginx](xref:host-and-deploy/linux-nginx).</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="c2a71-148">Apache und Kestrel</span><span class="sxs-lookup"><span data-stu-id="c2a71-148">Apache with Kestrel</span></span>

<span data-ttu-id="c2a71-149">Informationen dazu, wie Apache unter Linux als Reverseproxyserver für Kestrel verwendet wird, finden Sie unter [Hosten unter Linux mit Apache](xref:host-and-deploy/linux-apache).</span><span class="sxs-lookup"><span data-stu-id="c2a71-149">For information on how to use Apache on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Apache](xref:host-and-deploy/linux-apache).</span></span>

## <a name="httpsys"></a><span data-ttu-id="c2a71-150">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="c2a71-150">HTTP.sys</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="c2a71-151">Wenn ASP.NET Core-Apps unter Windows ausgeführt werden, ist HTTP.sys eine Alternative zu Kestrel.</span><span class="sxs-lookup"><span data-stu-id="c2a71-151">If ASP.NET Core apps are run on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="c2a71-152">Grundsätzlich empfiehlt sich Kestrel, um die beste Leistung zu erzielen.</span><span class="sxs-lookup"><span data-stu-id="c2a71-152">Kestrel is generally recommended for best performance.</span></span> <span data-ttu-id="c2a71-153">HTTP.sys kann in Szenarien verwendet werden, in denen die App mit dem Internet verbunden ist und erforderliche Funktionen von HTTP.sys, aber nicht von Kestrel unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="c2a71-153">HTTP.sys can be used in scenarios where the app is exposed to the Internet and required capabilities are supported by HTTP.sys but not Kestrel.</span></span> <span data-ttu-id="c2a71-154">Informationen zu HTTP.sys finden Sie unter [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="c2a71-154">For information on HTTP.sys, see [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

![HTTP.sys kommuniziert direkt mit dem Internet](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="c2a71-156">Außerdem kann HTTP.sys auch bei Apss zum Einsatz kommen, die nur in einem internen Netzwerk verfügbar gemacht werden.</span><span class="sxs-lookup"><span data-stu-id="c2a71-156">HTTP.sys can also be used for apps that are only exposed to an internal network.</span></span>

![HTTP.sys kommuniziert direkt mit dem internen Netzwerk](httpsys/_static/httpsys-to-internal.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="c2a71-158">HTTP.sys wird in ASP.NET Core 1.x [WebListener](xref:fundamentals/servers/weblistener) genannt.</span><span class="sxs-lookup"><span data-stu-id="c2a71-158">HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.</span></span> <span data-ttu-id="c2a71-159">Wenn ASP.NET Core-Apps unter Windows ausgeführt werden, ist WebListener eine Alternative für Szenarien, in denen Apps nicht von IIS gehostet werden können.</span><span class="sxs-lookup"><span data-stu-id="c2a71-159">If ASP.NET Core apps are run on Windows, WebListener is an alternative for scenarios where IIS isn't available to host apps.</span></span>

![WebListener kommuniziert direkt mit dem Internet](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="c2a71-161">WebListener kann auch anstelle von Kestrel für Apps verwendet werden, die nur in einem internen Netzwerk bereitgestellt werden, wenn erforderliche Funktionen durch WebListener, aber nicht durch Kestrel unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="c2a71-161">WebListener can also be used in place of Kestrel for apps that are only exposed to an internal network, if required capabilities are supported by WebListener but not Kestrel.</span></span> <span data-ttu-id="c2a71-162">Informationen zu WebListener finden Sie unter [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="c2a71-162">For information on WebListener, see [WebListener](xref:fundamentals/servers/weblistener).</span></span>

![WebListener kommuniziert direkt mit dem internen Netzwerk](weblistener/_static/weblistener-to-internal.png)

::: moniker-end

## <a name="aspnet-core-server-infrastructure"></a><span data-ttu-id="c2a71-164">ASP.NET Core-Serverinfrastruktur</span><span class="sxs-lookup"><span data-stu-id="c2a71-164">ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="c2a71-165">Die [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder)-Schnittstelle, sie in der `Startup.Configure`-Methode verfügbar ist, macht die [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures)-Eigenschaft des Typs [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection) verfügbar.</span><span class="sxs-lookup"><span data-stu-id="c2a71-165">The [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) available in the `Startup.Configure` method exposes the [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) property of type [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span></span> <span data-ttu-id="c2a71-166">Kestrel und HTTP.sys (WebListener in ASP.NET Core 1.x) machen nur jeweils eine einzelne Funktion, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), verfügbar, andere Serverimplementierungen machen jedoch möglicherweise weitere Funktionalität verfügbar.</span><span class="sxs-lookup"><span data-stu-id="c2a71-166">Kestrel and HTTP.sys (WebListener in ASP.NET Core 1.x) only expose a single feature each, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="c2a71-167">Mit `IServerAddressesFeature` kann ermittelt werden, an welchen Port die Serverimplementierung zur Laufzeit gebunden ist.</span><span class="sxs-lookup"><span data-stu-id="c2a71-167">`IServerAddressesFeature` can be used to find out which port the server implementation has bound at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="c2a71-168">Benutzerdefinierte Server</span><span class="sxs-lookup"><span data-stu-id="c2a71-168">Custom servers</span></span>

<span data-ttu-id="c2a71-169">Wenn die integrierten Server nicht den Anforderungen der App entsprechen, kann eine benutzerdefinierte Serverimplementierung erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="c2a71-169">If the built-in servers don't meet the app's requirements, a custom server implementation can be created.</span></span> <span data-ttu-id="c2a71-170">In der [Anleitung zu Open Web Interface for .NET (OWIN)](xref:fundamentals/owin) wird erläutert, wie eine auf [Nowin](https://github.com/Bobris/Nowin) basierende [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver)-Implementierung erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="c2a71-170">The [Open Web Interface for .NET (OWIN) guide](xref:fundamentals/owin) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) implementation.</span></span> <span data-ttu-id="c2a71-171">Nur die Feature-Schnittstellen, die von der App verwendet werden, erfordern Implementierung , wobei mindestens [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) und [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) unterstützt werden müssen.</span><span class="sxs-lookup"><span data-stu-id="c2a71-171">Only the feature interfaces that the app uses require implementation, though at a minimum [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) and [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) must be supported.</span></span>

## <a name="server-startup"></a><span data-ttu-id="c2a71-172">Serverstart</span><span class="sxs-lookup"><span data-stu-id="c2a71-172">Server startup</span></span>

<span data-ttu-id="c2a71-173">Wenn [Visual Studio](https://www.visualstudio.com/vs/), [Visual Studio für Mac](https://www.visualstudio.com/vs/mac/) oder [Visual Studio Code](https://code.visualstudio.com/) verwendet wird, wird der Server gestartet, wenn die App durch die integrierte Entwicklungsumgebung gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="c2a71-173">When using [Visual Studio](https://www.visualstudio.com/vs/), [Visual Studio for Mac](https://www.visualstudio.com/vs/mac/), or [Visual Studio Code](https://code.visualstudio.com/), the server is launched when the app is started by the Integrated Development Environment (IDE).</span></span> <span data-ttu-id="c2a71-174">In Visual Studio unter Windows können Startprofile verwendet werden, um die App und den Server mit [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) oder mit der Konsole zu starten.</span><span class="sxs-lookup"><span data-stu-id="c2a71-174">In Visual Studio on Windows, launch profiles can be used to start the app and server with either [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) or the console.</span></span> <span data-ttu-id="c2a71-175">In Visual Studio Code werden die App und der Server durch [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode) gestartet, das den CoreCLR-Debugger aktiviert.</span><span class="sxs-lookup"><span data-stu-id="c2a71-175">In Visual Studio Code, the app and server are started by [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), which activates the CoreCLR debugger.</span></span> <span data-ttu-id="c2a71-176">Wird Visual Studio für Mac verwendet, werden die App und der Server durch den [Mono Soft-Mode Debugger](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/) gestartet.</span><span class="sxs-lookup"><span data-stu-id="c2a71-176">Using Visual Studio for Mac, the app and server are started by the [Mono Soft-Mode Debugger](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span></span>

<span data-ttu-id="c2a71-177">Wird eine App über eine Eingabeaufforderung im Ordner des Projekts gestartet, startet [dotnet run](/dotnet/core/tools/dotnet-run) die App und den Server (nur Kestrel und HTTP.sys).</span><span class="sxs-lookup"><span data-stu-id="c2a71-177">When launching an app from a command prompt in the project's folder, [dotnet run](/dotnet/core/tools/dotnet-run) launches the app and server (Kestrel and HTTP.sys only).</span></span> <span data-ttu-id="c2a71-178">Die Konfiguration wird durch die Option `-c|--configuration` angegeben, die entweder auf `Debug` (Standardwert) oder `Release` festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="c2a71-178">The configuration is specified by the `-c|--configuration` option, which is set to either `Debug` (default) or `Release`.</span></span> <span data-ttu-id="c2a71-179">Sind in der Datei *launchSettings.json* Startprofile vorhanden, verwenden Sie die Option `--launch-profile <NAME>`, um das Startprofil festzulegen (z. B. `Development` oder `Production`).</span><span class="sxs-lookup"><span data-stu-id="c2a71-179">If launch profiles are present in a *launchSettings.json* file, use the `--launch-profile <NAME>` option to set the launch profile (for example, `Development` or `Production`).</span></span> <span data-ttu-id="c2a71-180">Weitere Informationen hierzu finden Sie in den Themen [dotnet run](/dotnet/core/tools/dotnet-run) und [Verpacken einer Verteilung von .NET Core](/dotnet/core/build/distribution-packaging).</span><span class="sxs-lookup"><span data-stu-id="c2a71-180">For more information, see the [dotnet run](/dotnet/core/tools/dotnet-run) and [.NET Core distribution packaging](/dotnet/core/build/distribution-packaging) topics.</span></span>

## <a name="http2-support"></a><span data-ttu-id="c2a71-181">HTTP/2-Unterstützung</span><span class="sxs-lookup"><span data-stu-id="c2a71-181">HTTP/2 support</span></span>

<span data-ttu-id="c2a71-182">[HTTP/2](https://httpwg.org/specs/rfc7540.html) wird mit ASP.NET Core in den folgenden Bereitstellungsszenarien unterstützt:</span><span class="sxs-lookup"><span data-stu-id="c2a71-182">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following deployment scenarios:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="c2a71-183">Kestrel</span><span class="sxs-lookup"><span data-stu-id="c2a71-183">Kestrel</span></span>](xref:fundamentals/servers/kestrel#http2-support)
  * <span data-ttu-id="c2a71-184">Betriebssystem</span><span class="sxs-lookup"><span data-stu-id="c2a71-184">Operating system</span></span>
    * <span data-ttu-id="c2a71-185">Windows Server 2016/Windows 10 oder höher&dagger;</span><span class="sxs-lookup"><span data-stu-id="c2a71-185">Windows Server 2016/Windows 10 or later&dagger;</span></span>
    * <span data-ttu-id="c2a71-186">Linux mit OpenSSL 1.0.2 oder höher (z.B. Ubuntu 16.04 oder höher)</span><span class="sxs-lookup"><span data-stu-id="c2a71-186">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
    * <span data-ttu-id="c2a71-187">HTTP/2 wird unter macOS in einem zukünftigen Release unterstützt.</span><span class="sxs-lookup"><span data-stu-id="c2a71-187">HTTP/2 will be supported on macOS in a future release.</span></span>
  * <span data-ttu-id="c2a71-188">Zielframework: .NET Core 2.2 oder höher</span><span class="sxs-lookup"><span data-stu-id="c2a71-188">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="c2a71-189">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="c2a71-189">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="c2a71-190">Windows Server 2016/Windows 10 oder höher</span><span class="sxs-lookup"><span data-stu-id="c2a71-190">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="c2a71-191">Zielframework: Gilt nicht für HTTP.sys-Bereitstellungen.</span><span class="sxs-lookup"><span data-stu-id="c2a71-191">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="c2a71-192">IIS (In-Process)</span><span class="sxs-lookup"><span data-stu-id="c2a71-192">IIS (in-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="c2a71-193">Windows Server 2016/Windows 10 oder höher, IIS 10 oder höher</span><span class="sxs-lookup"><span data-stu-id="c2a71-193">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="c2a71-194">Zielframework: .NET Core 2.2 oder höher</span><span class="sxs-lookup"><span data-stu-id="c2a71-194">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="c2a71-195">IIS (Out-of-Process)</span><span class="sxs-lookup"><span data-stu-id="c2a71-195">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="c2a71-196">Windows Server 2016/Windows 10 oder höher, IIS 10 oder höher</span><span class="sxs-lookup"><span data-stu-id="c2a71-196">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="c2a71-197">Öffentlich zugängliche Edge-Server-Verbindungen verwenden HTTP/2, aber die Reverseproxyverbindung mit Kestrel verwendet HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="c2a71-197">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="c2a71-198">Zielframework: Gilt nicht für Out-of-Process-Bereitstellungen von IIS.</span><span class="sxs-lookup"><span data-stu-id="c2a71-198">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

<span data-ttu-id="c2a71-199">&dagger;Kestrel bietet eingeschränkte Unterstützung für HTTP/2 unter Windows Server 2012 R2 und Windows 8.1.</span><span class="sxs-lookup"><span data-stu-id="c2a71-199">&dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="c2a71-200">Die Unterstützung ist eingeschränkt, weil die Liste der unterstützten TLS-Verschlüsselungssammlungen unter diesen Betriebssystemen begrenzt ist.</span><span class="sxs-lookup"><span data-stu-id="c2a71-200">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="c2a71-201">Zum Sichern von TLS-Verbindungen ist möglicherweise ein durch einen Elliptic Curve Digital Signature Algorithm (ECDSA) generiertes Zertifikat erforderlich.</span><span class="sxs-lookup"><span data-stu-id="c2a71-201">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [<span data-ttu-id="c2a71-202">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="c2a71-202">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="c2a71-203">Windows Server 2016/Windows 10 oder höher</span><span class="sxs-lookup"><span data-stu-id="c2a71-203">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="c2a71-204">Zielframework: Gilt nicht für HTTP.sys-Bereitstellungen.</span><span class="sxs-lookup"><span data-stu-id="c2a71-204">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="c2a71-205">IIS (Out-of-Process)</span><span class="sxs-lookup"><span data-stu-id="c2a71-205">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="c2a71-206">Windows Server 2016/Windows 10 oder höher, IIS 10 oder höher</span><span class="sxs-lookup"><span data-stu-id="c2a71-206">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="c2a71-207">Öffentlich zugängliche Edge-Server-Verbindungen verwenden HTTP/2, aber die Reverseproxyverbindung mit Kestrel verwendet HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="c2a71-207">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="c2a71-208">Zielframework: Gilt nicht für Out-of-Process-Bereitstellungen von IIS.</span><span class="sxs-lookup"><span data-stu-id="c2a71-208">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

::: moniker-end

<span data-ttu-id="c2a71-209">Eine HTTP/2-Verbindung muss [ALPN (Application-Layer Protocol Negotiation)](https://tools.ietf.org/html/rfc7301#section-3) und TLS 1.2 oder höher verwenden.</span><span class="sxs-lookup"><span data-stu-id="c2a71-209">An HTTP/2 connection must use [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) and TLS 1.2 or later.</span></span> <span data-ttu-id="c2a71-210">Weitere Informationen finden Sie in den Themen, die Ihre Serverbereitstellungsszenarien betreffen.</span><span class="sxs-lookup"><span data-stu-id="c2a71-210">For more information, see the topics that pertain to your server deployment scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c2a71-211">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="c2a71-211">Additional resources</span></span>

* <xref:fundamentals/servers/kestrel>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <span data-ttu-id="c2a71-212"><xref:fundamentals/servers/httpsys> (für ASP.NET Core 1.x siehe<xref:fundamentals/servers/weblistener>)</span><span class="sxs-lookup"><span data-stu-id="c2a71-212"><xref:fundamentals/servers/httpsys> (for ASP.NET Core 1.x, see <xref:fundamentals/servers/weblistener>)</span></span>
