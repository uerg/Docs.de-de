---
title: Webserverimplementierungen in ASP.NET Core
author: guardrex
description: Ermitteln Sie die Webserver Kestrel und HTTP.sys für ASP.NET Core. Erfahren Sie mehr über das Auswählen eines Servers und darüber, wann ein Reverseproxyserver zu verwenden ist.
ms.author: tdykstra
ms.custom: mvc
ms.date: 12/01/2018
uid: fundamentals/servers/index
ms.openlocfilehash: 965d69dd071ec71d283284d58e6e1a6e78604f90
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861355"
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="81b2e-104">Webserverimplementierungen in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="81b2e-104">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="81b2e-105">Von [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73) und [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="81b2e-105">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="81b2e-106">Eine ASP.NET Core-App wird über eine In-Process-Implementierung eines HTTP-Servers ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="81b2e-106">An ASP.NET Core app runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="81b2e-107">Die Serverimplementierung lauscht auf HTTP-Anforderungen und leitet diese als Gruppen von [Anforderungsfunktionen](xref:fundamentals/request-features), die in einem <xref:Microsoft.AspNetCore.Http.HttpContext> zusammengefasst werden, an die App weiter.</span><span class="sxs-lookup"><span data-stu-id="81b2e-107">The server implementation listens for HTTP requests and surfaces them to the app as sets of [request features](xref:fundamentals/request-features) composed into an <xref:Microsoft.AspNetCore.Http.HttpContext>.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="81b2e-108">Windows</span><span class="sxs-lookup"><span data-stu-id="81b2e-108">Windows</span></span>](#tab/windows)

<span data-ttu-id="81b2e-109">ASP.NET Core wird mit folgendem Umfang ausgeliefert:</span><span class="sxs-lookup"><span data-stu-id="81b2e-109">ASP.NET Core ships with the following:</span></span>

* <span data-ttu-id="81b2e-110">[Kestrel Server](xref:fundamentals/servers/kestrel) ist der plattformübergreifende HTTP-Standardserver.</span><span class="sxs-lookup"><span data-stu-id="81b2e-110">[Kestrel server](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server.</span></span>
* <span data-ttu-id="81b2e-111">IIS-HTTP-Server (`IISHttpServer`) ist eine [In-Process-IIS-Server](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model)-Implementierung, die mit dem [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="81b2e-111">IIS HTTP Server (`IISHttpServer`) is an [IIS in-process server](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model) implementation used with the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span>
* <span data-ttu-id="81b2e-112">[HTTP.sys Server](xref:fundamentals/servers/httpsys) ist ein nur für Windows verfügbarer HTTP-Server, der auf dem [Http.sys-Kerneltreiber und der HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx) basiert.</span><span class="sxs-lookup"><span data-stu-id="81b2e-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="81b2e-113">HTTP.sys wird in ASP.NET Core 1.x als [WebListener](xref:fundamentals/servers/weblistener) bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="81b2e-113">HTTP.sys is called [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="81b2e-114">macOS</span><span class="sxs-lookup"><span data-stu-id="81b2e-114">macOS</span></span>](#tab/macos)

<span data-ttu-id="81b2e-115">ASP.NET Core wird mit [Kestrel Server](xref:fundamentals/servers/kestrel) ausgeliefert, der der plattformübergreifende HTTP-Standardserver ist.</span><span class="sxs-lookup"><span data-stu-id="81b2e-115">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="81b2e-116">Linux</span><span class="sxs-lookup"><span data-stu-id="81b2e-116">Linux</span></span>](#tab/linux)

<span data-ttu-id="81b2e-117">ASP.NET Core wird mit [Kestrel Server](xref:fundamentals/servers/kestrel) ausgeliefert, der der plattformübergreifende HTTP-Standardserver ist.</span><span class="sxs-lookup"><span data-stu-id="81b2e-117">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="81b2e-118">Windows</span><span class="sxs-lookup"><span data-stu-id="81b2e-118">Windows</span></span>](#tab/windows)

<span data-ttu-id="81b2e-119">ASP.NET Core wird mit folgendem Umfang ausgeliefert:</span><span class="sxs-lookup"><span data-stu-id="81b2e-119">ASP.NET Core ships with the following:</span></span>

* <span data-ttu-id="81b2e-120">[Kestrel Server](xref:fundamentals/servers/kestrel) ist der plattformübergreifende HTTP-Standardserver.</span><span class="sxs-lookup"><span data-stu-id="81b2e-120">[Kestrel server](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server.</span></span>
* <span data-ttu-id="81b2e-121">[HTTP.sys Server](xref:fundamentals/servers/httpsys) ist ein nur für Windows verfügbarer HTTP-Server, der auf dem [Http.sys-Kerneltreiber und der HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx) basiert.</span><span class="sxs-lookup"><span data-stu-id="81b2e-121">[HTTP.sys server](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="81b2e-122">HTTP.sys wird in ASP.NET Core 1.x als [WebListener](xref:fundamentals/servers/weblistener) bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="81b2e-122">HTTP.sys is called [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="81b2e-123">macOS</span><span class="sxs-lookup"><span data-stu-id="81b2e-123">macOS</span></span>](#tab/macos)

<span data-ttu-id="81b2e-124">ASP.NET Core wird mit [Kestrel Server](xref:fundamentals/servers/kestrel) ausgeliefert, der der plattformübergreifende HTTP-Standardserver ist.</span><span class="sxs-lookup"><span data-stu-id="81b2e-124">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="81b2e-125">Linux</span><span class="sxs-lookup"><span data-stu-id="81b2e-125">Linux</span></span>](#tab/linux)

<span data-ttu-id="81b2e-126">ASP.NET Core wird mit [Kestrel Server](xref:fundamentals/servers/kestrel) ausgeliefert, der der plattformübergreifende HTTP-Standardserver ist.</span><span class="sxs-lookup"><span data-stu-id="81b2e-126">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

---

::: moniker-end

## <a name="kestrel"></a><span data-ttu-id="81b2e-127">Kestrel</span><span class="sxs-lookup"><span data-stu-id="81b2e-127">Kestrel</span></span>

<span data-ttu-id="81b2e-128">Kestrel ist der Standardwebserver, der in ASP.NET Core-Projektvorlagen enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="81b2e-128">Kestrel is the default web server included in ASP.NET Core project templates.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="81b2e-129">Kestrel eignet sich für folgende Zwecke:</span><span class="sxs-lookup"><span data-stu-id="81b2e-129">Kestrel can be used:</span></span>

* <span data-ttu-id="81b2e-130">Eigenständig als Edge-Server zur Verarbeitung von Anforderungen, die direkt aus einem Netzwerk, auch über das Internet, kommen.</span><span class="sxs-lookup"><span data-stu-id="81b2e-130">By itself as an edge server processing requests directly from a network, including the Internet.</span></span>
* <span data-ttu-id="81b2e-131">Mit einem *Reverseproxyserver* wie [IIS (Internetinformationsdienste)](https://www.iis.net/), [Nginx](http://nginx.org) oder [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="81b2e-131">With a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="81b2e-132">Ein Reverseproxyserver empfängt HTTP-Anforderungen aus dem Internet und leitet diese an Kestrel weiter.</span><span class="sxs-lookup"><span data-stu-id="81b2e-132">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel.</span></span>

![Kestrel kommuniziert direkt und ohne Reverseproxyserver mit dem Internet](kestrel/_static/kestrel-to-internet2.png)

![Kestrel kommuniziert indirekt mit dem Internet über einen Reverseproxyserver wie IIS, Nginx oder Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="81b2e-135">Jede der beiden Konfigurationen &mdash;mit oder ohne einen Reverseproxyserver&mdash; ist eine gültige und unterstützte Hostingkonfiguration für ASP.NET Core 2.0 oder neuere Apps.</span><span class="sxs-lookup"><span data-stu-id="81b2e-135">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="81b2e-136">Weitere Informationen finden Sie unter [When to use Kestrel with a reverse proxy (Verwenden von Kestrel mit einem Reverseproxy)](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="81b2e-136">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="81b2e-137">Wenn die App nur Anforderungen aus einem internen Netzwerk akzeptiert, kann Kestrel eigenständig verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="81b2e-137">If the app only accepts requests from an internal network, Kestrel can be used by itself.</span></span>

![Kestrel kommuniziert direkt mit dem internen Netzwerk.](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="81b2e-139">Wenn die App für das Internet verfügbar gemacht ist, muss Kestrel einen *Reverseproxyserver* wie [IIS (Internetinformationsdienste)](https://www.iis.net/), [Nginx](http://nginx.org) oder [Apache](https://httpd.apache.org/) verwenden.</span><span class="sxs-lookup"><span data-stu-id="81b2e-139">If the app is exposed to the Internet, Kestrel must use a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="81b2e-140">Ein Reverseproxyserver empfängt HTTP-Anforderungen aus dem Internet und leitet diese an Kestrel weiter.</span><span class="sxs-lookup"><span data-stu-id="81b2e-140">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel.</span></span>

![Kestrel kommuniziert indirekt mit dem Internet über einen Reverseproxyserver wie IIS, Nginx oder Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="81b2e-142">Der wichtigste Grund für die Verwendung eines Reverseproxys für öffentlich zugängliche Edge-Server-Bereitstellungen, die direkt im Internet verfügbar gemacht werden, ist die Sicherheit.</span><span class="sxs-lookup"><span data-stu-id="81b2e-142">The most important reason for using a reverse proxy for public-facing edge server deployments that are exposed directly the Internet is security.</span></span> <span data-ttu-id="81b2e-143">Die 1.x-Versionen von Kestrel enthalten keine wesentlichen Sicherheitsfeatures zum Schutz vor Angriffen aus dem Internet.</span><span class="sxs-lookup"><span data-stu-id="81b2e-143">The 1.x versions of Kestrel don't include important security features to defend against attacks from the Internet.</span></span> <span data-ttu-id="81b2e-144">So sind z. B. keine geeigneten Timeouts, Größenlimits für Anforderungen sowie Beschränkungen der Anzahl gleichzeitiger Verbindungen vorhanden.</span><span class="sxs-lookup"><span data-stu-id="81b2e-144">This includes, but isn't limited to, appropriate timeouts, request size limits, and concurrent connection limits.</span></span>

<span data-ttu-id="81b2e-145">Weitere Informationen finden Sie unter [When to use Kestrel with a reverse proxy (Verwenden von Kestrel mit einem Reverseproxy)](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="81b2e-145">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

### <a name="iis-with-kestrel"></a><span data-ttu-id="81b2e-146">IIS und Kestrel</span><span class="sxs-lookup"><span data-stu-id="81b2e-146">IIS with Kestrel</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="81b2e-147">Bei Verwendung von [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) oder [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) wird die ASP.NET Core-App entweder im selben Prozess wie der IIS-Workerprozess ausgeführt (das *In-Process*-Hostingmodell) oder in einem vom IIS-Workerprozess getrennten Prozess (das *Out-of-Process*-Hostingmodell).</span><span class="sxs-lookup"><span data-stu-id="81b2e-147">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the ASP.NET Core app either runs in the same process as the IIS worker process (the *in-process* hosting model) or in a process separate from the IIS worker process (the *out-of-process* hosting model).</span></span>

<span data-ttu-id="81b2e-148">Das [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) ist ein natives IIS-Modul, das native IIS-Anforderungen zwischen dem In-Process-IIS-HTTP-Server oder dem Out-of-Process-Kestrel-Server verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="81b2e-148">The [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) is a native IIS module that handles native IIS requests between either the in-process IIS HTTP Server or the out-of-process Kestrel server.</span></span> <span data-ttu-id="81b2e-149">Weitere Informationen finden Sie unter <xref:fundamentals/servers/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="81b2e-149">For more information, see <xref:fundamentals/servers/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="81b2e-150">Wenn [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) oder [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) als Reverseproxy für ASP.NET Core verwendet wird, wird die ASP.NET Core-App in einem Prozess ausgeführt, der vom IIS-Workerprozess getrennt ist.</span><span class="sxs-lookup"><span data-stu-id="81b2e-150">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) as a reverse proxy for ASP.NET Core, the ASP.NET Core app runs in a process separate from the IIS worker process.</span></span> <span data-ttu-id="81b2e-151">Im IIS-Prozess steuert das [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) die Beziehung zum Reverseproxy.</span><span class="sxs-lookup"><span data-stu-id="81b2e-151">In the IIS process, the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) coordinates the reverse proxy relationship.</span></span> <span data-ttu-id="81b2e-152">Die Hauptfunktionen des ASP.NET Core-Moduls bestehen darin, die App zu starten, sie nach einem Absturz neu zu starten und HTTP-Datenverkehr an die App weiterzuleiten.</span><span class="sxs-lookup"><span data-stu-id="81b2e-152">The primary functions of the ASP.NET Core Module are to start the app, restart the app when it crashes, and forward HTTP traffic to the app.</span></span> <span data-ttu-id="81b2e-153">Weitere Informationen finden Sie unter <xref:fundamentals/servers/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="81b2e-153">For more information, see <xref:fundamentals/servers/aspnet-core-module>.</span></span>

::: moniker-end

### <a name="nginx-with-kestrel"></a><span data-ttu-id="81b2e-154">Nginx und Kestrel</span><span class="sxs-lookup"><span data-stu-id="81b2e-154">Nginx with Kestrel</span></span>

<span data-ttu-id="81b2e-155">Informationen dazu, wie Nginx unter Linux als Reverseproxyserver für Kestrel verwendet wird, finden Sie unter <xref:host-and-deploy/linux-nginx>.</span><span class="sxs-lookup"><span data-stu-id="81b2e-155">For information on how to use Nginx on Linux as a reverse proxy server for Kestrel, see <xref:host-and-deploy/linux-nginx>.</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="81b2e-156">Apache und Kestrel</span><span class="sxs-lookup"><span data-stu-id="81b2e-156">Apache with Kestrel</span></span>

<span data-ttu-id="81b2e-157">Informationen dazu, wie Apache unter Linux als Reverseproxyserver für Kestrel verwendet wird, finden Sie unter <xref:host-and-deploy/linux-apache>.</span><span class="sxs-lookup"><span data-stu-id="81b2e-157">For information on how to use Apache on Linux as a reverse proxy server for Kestrel, see <xref:host-and-deploy/linux-apache>.</span></span>

## <a name="httpsys"></a><span data-ttu-id="81b2e-158">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="81b2e-158">HTTP.sys</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="81b2e-159">Wenn ASP.NET Core-Apps unter Windows ausgeführt werden, ist HTTP.sys eine Alternative zu Kestrel.</span><span class="sxs-lookup"><span data-stu-id="81b2e-159">If ASP.NET Core apps are run on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="81b2e-160">Grundsätzlich empfiehlt sich Kestrel, um die beste Leistung zu erzielen.</span><span class="sxs-lookup"><span data-stu-id="81b2e-160">Kestrel is generally recommended for best performance.</span></span> <span data-ttu-id="81b2e-161">HTTP.sys kann in Szenarien verwendet werden, in denen die App mit dem Internet verbunden ist und erforderliche Funktionen von HTTP.sys, aber nicht von Kestrel unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="81b2e-161">HTTP.sys can be used in scenarios where the app is exposed to the Internet and required capabilities are supported by HTTP.sys but not Kestrel.</span></span> <span data-ttu-id="81b2e-162">Weitere Informationen finden Sie unter <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="81b2e-162">For more information, see <xref:fundamentals/servers/httpsys>.</span></span>

![HTTP.sys kommuniziert direkt mit dem Internet](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="81b2e-164">Außerdem kann HTTP.sys auch bei Apss zum Einsatz kommen, die nur in einem internen Netzwerk verfügbar gemacht werden.</span><span class="sxs-lookup"><span data-stu-id="81b2e-164">HTTP.sys can also be used for apps that are only exposed to an internal network.</span></span>

![HTTP.sys kommuniziert direkt mit dem internen Netzwerk](httpsys/_static/httpsys-to-internal.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="81b2e-166">HTTP.sys wird in ASP.NET Core 1.x [WebListener](xref:fundamentals/servers/weblistener) genannt.</span><span class="sxs-lookup"><span data-stu-id="81b2e-166">HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.</span></span> <span data-ttu-id="81b2e-167">Wenn ASP.NET Core-Apps unter Windows ausgeführt werden, ist WebListener eine Alternative für Szenarien, in denen Apps nicht von IIS gehostet werden können.</span><span class="sxs-lookup"><span data-stu-id="81b2e-167">If ASP.NET Core apps are run on Windows, WebListener is an alternative for scenarios where IIS isn't available to host apps.</span></span>

![WebListener kommuniziert direkt mit dem Internet](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="81b2e-169">WebListener kann auch anstelle von Kestrel für Apps verwendet werden, die nur in einem internen Netzwerk bereitgestellt werden, wenn erforderliche Funktionen durch WebListener, aber nicht durch Kestrel unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="81b2e-169">WebListener can also be used in place of Kestrel for apps that are only exposed to an internal network, if required capabilities are supported by WebListener but not Kestrel.</span></span> <span data-ttu-id="81b2e-170">Informationen zu WebListener finden Sie unter [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="81b2e-170">For information on WebListener, see [WebListener](xref:fundamentals/servers/weblistener).</span></span>

![WebListener kommuniziert direkt mit dem internen Netzwerk](weblistener/_static/weblistener-to-internal.png)

::: moniker-end

## <a name="aspnet-core-server-infrastructure"></a><span data-ttu-id="81b2e-172">ASP.NET Core-Serverinfrastruktur</span><span class="sxs-lookup"><span data-stu-id="81b2e-172">ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="81b2e-173">Die [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder)-Schnittstelle, sie in der `Startup.Configure`-Methode verfügbar ist, macht die [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures)-Eigenschaft des Typs [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection) verfügbar.</span><span class="sxs-lookup"><span data-stu-id="81b2e-173">The [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) available in the `Startup.Configure` method exposes the [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) property of type [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span></span> <span data-ttu-id="81b2e-174">Kestrel und HTTP.sys (WebListener in ASP.NET Core 1.x) machen nur jeweils eine einzelne Funktion, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), verfügbar, andere Serverimplementierungen machen jedoch möglicherweise weitere Funktionalität verfügbar.</span><span class="sxs-lookup"><span data-stu-id="81b2e-174">Kestrel and HTTP.sys (WebListener in ASP.NET Core 1.x) only expose a single feature each, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="81b2e-175">Mit `IServerAddressesFeature` kann ermittelt werden, an welchen Port die Serverimplementierung zur Laufzeit gebunden ist.</span><span class="sxs-lookup"><span data-stu-id="81b2e-175">`IServerAddressesFeature` can be used to find out which port the server implementation has bound at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="81b2e-176">Benutzerdefinierte Server</span><span class="sxs-lookup"><span data-stu-id="81b2e-176">Custom servers</span></span>

<span data-ttu-id="81b2e-177">Wenn die integrierten Server nicht den Anforderungen der App entsprechen, kann eine benutzerdefinierte Serverimplementierung erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="81b2e-177">If the built-in servers don't meet the app's requirements, a custom server implementation can be created.</span></span> <span data-ttu-id="81b2e-178">In der [Anleitung zu Open Web Interface for .NET (OWIN)](xref:fundamentals/owin) wird erläutert, wie eine auf [Nowin](https://github.com/Bobris/Nowin) basierende [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver)-Implementierung erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="81b2e-178">The [Open Web Interface for .NET (OWIN) guide](xref:fundamentals/owin) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) implementation.</span></span> <span data-ttu-id="81b2e-179">Nur die Feature-Schnittstellen, die von der App verwendet werden, erfordern Implementierung , wobei mindestens [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) und [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) unterstützt werden müssen.</span><span class="sxs-lookup"><span data-stu-id="81b2e-179">Only the feature interfaces that the app uses require implementation, though at a minimum [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) and [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) must be supported.</span></span>

## <a name="server-startup"></a><span data-ttu-id="81b2e-180">Serverstart</span><span class="sxs-lookup"><span data-stu-id="81b2e-180">Server startup</span></span>

<span data-ttu-id="81b2e-181">Wenn [Visual Studio](https://www.visualstudio.com/vs/), [Visual Studio für Mac](https://www.visualstudio.com/vs/mac/) oder [Visual Studio Code](https://code.visualstudio.com/) verwendet wird, wird der Server gestartet, wenn die App durch die integrierte Entwicklungsumgebung gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="81b2e-181">When using [Visual Studio](https://www.visualstudio.com/vs/), [Visual Studio for Mac](https://www.visualstudio.com/vs/mac/), or [Visual Studio Code](https://code.visualstudio.com/), the server is launched when the app is started by the Integrated Development Environment (IDE).</span></span> <span data-ttu-id="81b2e-182">In Visual Studio unter Windows können Startprofile verwendet werden, um die App und den Server mit [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) oder mit der Konsole zu starten.</span><span class="sxs-lookup"><span data-stu-id="81b2e-182">In Visual Studio on Windows, launch profiles can be used to start the app and server with either [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) or the console.</span></span> <span data-ttu-id="81b2e-183">In Visual Studio Code werden die App und der Server durch [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode) gestartet, das den CoreCLR-Debugger aktiviert.</span><span class="sxs-lookup"><span data-stu-id="81b2e-183">In Visual Studio Code, the app and server are started by [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), which activates the CoreCLR debugger.</span></span> <span data-ttu-id="81b2e-184">Wird Visual Studio für Mac verwendet, werden die App und der Server durch den [Mono Soft-Mode Debugger](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/) gestartet.</span><span class="sxs-lookup"><span data-stu-id="81b2e-184">Using Visual Studio for Mac, the app and server are started by the [Mono Soft-Mode Debugger](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span></span>

<span data-ttu-id="81b2e-185">Wird eine App über eine Eingabeaufforderung im Ordner des Projekts gestartet, startet [dotnet run](/dotnet/core/tools/dotnet-run) die App und den Server (nur Kestrel und HTTP.sys).</span><span class="sxs-lookup"><span data-stu-id="81b2e-185">When launching an app from a command prompt in the project's folder, [dotnet run](/dotnet/core/tools/dotnet-run) launches the app and server (Kestrel and HTTP.sys only).</span></span> <span data-ttu-id="81b2e-186">Die Konfiguration wird durch die Option `-c|--configuration` angegeben, die entweder auf `Debug` (Standardwert) oder `Release` festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="81b2e-186">The configuration is specified by the `-c|--configuration` option, which is set to either `Debug` (default) or `Release`.</span></span> <span data-ttu-id="81b2e-187">Sind in der Datei *launchSettings.json* Startprofile vorhanden, verwenden Sie die Option `--launch-profile <NAME>`, um das Startprofil festzulegen (z. B. `Development` oder `Production`).</span><span class="sxs-lookup"><span data-stu-id="81b2e-187">If launch profiles are present in a *launchSettings.json* file, use the `--launch-profile <NAME>` option to set the launch profile (for example, `Development` or `Production`).</span></span> <span data-ttu-id="81b2e-188">Weitere Informationen hierzu finden Sie in den Themen [dotnet run](/dotnet/core/tools/dotnet-run) und [Verpacken einer Verteilung von .NET Core](/dotnet/core/build/distribution-packaging).</span><span class="sxs-lookup"><span data-stu-id="81b2e-188">For more information, see the [dotnet run](/dotnet/core/tools/dotnet-run) and [.NET Core distribution packaging](/dotnet/core/build/distribution-packaging) topics.</span></span>

## <a name="http2-support"></a><span data-ttu-id="81b2e-189">HTTP/2-Unterstützung</span><span class="sxs-lookup"><span data-stu-id="81b2e-189">HTTP/2 support</span></span>

<span data-ttu-id="81b2e-190">[HTTP/2](https://httpwg.org/specs/rfc7540.html) wird mit ASP.NET Core in den folgenden Bereitstellungsszenarien unterstützt:</span><span class="sxs-lookup"><span data-stu-id="81b2e-190">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following deployment scenarios:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="81b2e-191">Kestrel</span><span class="sxs-lookup"><span data-stu-id="81b2e-191">Kestrel</span></span>](xref:fundamentals/servers/kestrel#http2-support)
  * <span data-ttu-id="81b2e-192">Betriebssystem</span><span class="sxs-lookup"><span data-stu-id="81b2e-192">Operating system</span></span>
    * <span data-ttu-id="81b2e-193">Windows Server 2016/Windows 10 oder höher&dagger;</span><span class="sxs-lookup"><span data-stu-id="81b2e-193">Windows Server 2016/Windows 10 or later&dagger;</span></span>
    * <span data-ttu-id="81b2e-194">Linux mit OpenSSL 1.0.2 oder höher (z.B. Ubuntu 16.04 oder höher)</span><span class="sxs-lookup"><span data-stu-id="81b2e-194">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
    * <span data-ttu-id="81b2e-195">HTTP/2 wird unter macOS in einem zukünftigen Release unterstützt.</span><span class="sxs-lookup"><span data-stu-id="81b2e-195">HTTP/2 will be supported on macOS in a future release.</span></span>
  * <span data-ttu-id="81b2e-196">Zielframework: .NET Core 2.2 oder höher</span><span class="sxs-lookup"><span data-stu-id="81b2e-196">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="81b2e-197">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="81b2e-197">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="81b2e-198">Windows Server 2016/Windows 10 oder höher</span><span class="sxs-lookup"><span data-stu-id="81b2e-198">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="81b2e-199">Zielframework: Gilt nicht für HTTP.sys-Bereitstellungen.</span><span class="sxs-lookup"><span data-stu-id="81b2e-199">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="81b2e-200">IIS (In-Process)</span><span class="sxs-lookup"><span data-stu-id="81b2e-200">IIS (in-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="81b2e-201">Windows Server 2016/Windows 10 oder höher, IIS 10 oder höher</span><span class="sxs-lookup"><span data-stu-id="81b2e-201">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="81b2e-202">Zielframework: .NET Core 2.2 oder höher</span><span class="sxs-lookup"><span data-stu-id="81b2e-202">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="81b2e-203">IIS (Out-of-Process)</span><span class="sxs-lookup"><span data-stu-id="81b2e-203">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="81b2e-204">Windows Server 2016/Windows 10 oder höher, IIS 10 oder höher</span><span class="sxs-lookup"><span data-stu-id="81b2e-204">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="81b2e-205">Öffentlich zugängliche Edge-Server-Verbindungen verwenden HTTP/2, aber die Reverseproxyverbindung mit Kestrel verwendet HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="81b2e-205">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="81b2e-206">Zielframework: Gilt nicht für Out-of-Process-Bereitstellungen von IIS.</span><span class="sxs-lookup"><span data-stu-id="81b2e-206">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

<span data-ttu-id="81b2e-207">&dagger;Kestrel bietet eingeschränkte Unterstützung für HTTP/2 unter Windows Server 2012 R2 und Windows 8.1.</span><span class="sxs-lookup"><span data-stu-id="81b2e-207">&dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="81b2e-208">Die Unterstützung ist eingeschränkt, weil die Liste der unterstützten TLS-Verschlüsselungssammlungen unter diesen Betriebssystemen begrenzt ist.</span><span class="sxs-lookup"><span data-stu-id="81b2e-208">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="81b2e-209">Zum Sichern von TLS-Verbindungen ist möglicherweise ein durch einen Elliptic Curve Digital Signature Algorithm (ECDSA) generiertes Zertifikat erforderlich.</span><span class="sxs-lookup"><span data-stu-id="81b2e-209">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [<span data-ttu-id="81b2e-210">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="81b2e-210">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="81b2e-211">Windows Server 2016/Windows 10 oder höher</span><span class="sxs-lookup"><span data-stu-id="81b2e-211">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="81b2e-212">Zielframework: Gilt nicht für HTTP.sys-Bereitstellungen.</span><span class="sxs-lookup"><span data-stu-id="81b2e-212">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="81b2e-213">IIS (Out-of-Process)</span><span class="sxs-lookup"><span data-stu-id="81b2e-213">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="81b2e-214">Windows Server 2016/Windows 10 oder höher, IIS 10 oder höher</span><span class="sxs-lookup"><span data-stu-id="81b2e-214">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="81b2e-215">Öffentlich zugängliche Edge-Server-Verbindungen verwenden HTTP/2, aber die Reverseproxyverbindung mit Kestrel verwendet HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="81b2e-215">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="81b2e-216">Zielframework: Gilt nicht für Out-of-Process-Bereitstellungen von IIS.</span><span class="sxs-lookup"><span data-stu-id="81b2e-216">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

::: moniker-end

<span data-ttu-id="81b2e-217">Eine HTTP/2-Verbindung muss [ALPN (Application-Layer Protocol Negotiation)](https://tools.ietf.org/html/rfc7301#section-3) und TLS 1.2 oder höher verwenden.</span><span class="sxs-lookup"><span data-stu-id="81b2e-217">An HTTP/2 connection must use [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) and TLS 1.2 or later.</span></span> <span data-ttu-id="81b2e-218">Weitere Informationen finden Sie in den Themen, die Ihre Serverbereitstellungsszenarien betreffen.</span><span class="sxs-lookup"><span data-stu-id="81b2e-218">For more information, see the topics that pertain to your server deployment scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="81b2e-219">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="81b2e-219">Additional resources</span></span>

* <xref:fundamentals/servers/kestrel>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <span data-ttu-id="81b2e-220"><xref:fundamentals/servers/httpsys> (für ASP.NET Core 1.x siehe<xref:fundamentals/servers/weblistener>)</span><span class="sxs-lookup"><span data-stu-id="81b2e-220"><xref:fundamentals/servers/httpsys> (for ASP.NET Core 1.x, see <xref:fundamentals/servers/weblistener>)</span></span>
