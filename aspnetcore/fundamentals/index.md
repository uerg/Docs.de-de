---
title: ASP.NET Core – Grundlagen
author: rick-anderson
description: Lernen Sie die grundlegenden Konzepte zum Erstellen von ASP.NET Core-Apps kennen.
ms.author: riande
ms.custom: mvc
ms.date: 10/25/2018
uid: fundamentals/index
ms.openlocfilehash: 56344315acc59003248ffaf1e61455b94a93a545
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090718"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="1ca12-103">ASP.NET Core – Grundlagen</span><span class="sxs-lookup"><span data-stu-id="1ca12-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="1ca12-104">Eine ASP.NET Core-App ist eine Konsolen-App, in deren `Program.Main`-Methode ein Webserver erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="1ca12-104">An ASP.NET Core app is a console app that creates a web server in its `Program.Main` method.</span></span> <span data-ttu-id="1ca12-105">Die `Main`-Methode ist der *verwaltete Einstiegspunkt* der App:</span><span class="sxs-lookup"><span data-stu-id="1ca12-105">The `Main` method is the app's *managed entry point*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs)]

<span data-ttu-id="1ca12-106">Der .NET Core-Host:</span><span class="sxs-lookup"><span data-stu-id="1ca12-106">The .NET Core Host:</span></span>

* <span data-ttu-id="1ca12-107">Lädt die [.NET Core-Runtime](https://github.com/dotnet/coreclr).</span><span class="sxs-lookup"><span data-stu-id="1ca12-107">Loads the [.NET Core runtime](https://github.com/dotnet/coreclr).</span></span>
* <span data-ttu-id="1ca12-108">Verwendet das erste Argument in der Befehlszeile als Pfad zur verwalteten Binärdatei, die den Einstiegspunkt (`Main`) enthält, und beginnt mit der Ausführung des Codes.</span><span class="sxs-lookup"><span data-stu-id="1ca12-108">Uses the first command-line argument as the path to the managed binary that contains the entry point (`Main`) and begins code execution.</span></span>

<span data-ttu-id="1ca12-109">Die `Main`-Methode ruft die [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*)[-Methode auf, die nach dem ](https://wikipedia.org/wiki/Builder_pattern)Builder-Muster einen Webhost erstellt.</span><span class="sxs-lookup"><span data-stu-id="1ca12-109">The `Main` method invokes [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web host.</span></span> <span data-ttu-id="1ca12-110">Der Builder verfügt über Methoden, die den Webserver (z.B. <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) und die Startklasse (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>) definieren.</span><span class="sxs-lookup"><span data-stu-id="1ca12-110">The builder has methods that define the web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="1ca12-111">Im obigen Beispiel wird der [Kestrel](xref:fundamentals/servers/kestrel)-Webserver automatisch zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="1ca12-111">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="1ca12-112">Wenn IIS verfügbar ist, wird versucht, den Webhost von ASP.NET Core auf IIS auszuführen.</span><span class="sxs-lookup"><span data-stu-id="1ca12-112">ASP.NET Core's web host attempts to run on IIS, if available.</span></span> <span data-ttu-id="1ca12-113">Andere Webserver wie [HTTP.sys](xref:fundamentals/servers/httpsys) können durch den Aufruf der entsprechenden Erweiterungsmethode verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="1ca12-113">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="1ca12-114">`UseStartup` wird im nächsten Abschnitt näher erläutert.</span><span class="sxs-lookup"><span data-stu-id="1ca12-114">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="1ca12-115">Der Rückgabetyp <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> des `WebHost.CreateDefaultBuilder`-Aufrufs stellt viele optionale Methoden zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="1ca12-115"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="1ca12-116">Zu diesen Methoden gehören `UseHttpSys` für das Hosten der Anwendung in HTTP.sys und <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> für das Festlegen des Stamminhaltsverzeichnisses.</span><span class="sxs-lookup"><span data-stu-id="1ca12-116">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="1ca12-117">Mit den Methoden <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> und <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> wird das <xref:Microsoft.AspNetCore.Hosting.IWebHost>-Objekt erstellt, das die Anwendung hostet und auf HTTP-Anforderungen lauscht.</span><span class="sxs-lookup"><span data-stu-id="1ca12-117">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs)]

<span data-ttu-id="1ca12-118">Der .NET Core-Host:</span><span class="sxs-lookup"><span data-stu-id="1ca12-118">The .NET Core Host:</span></span>

* <span data-ttu-id="1ca12-119">Lädt die [.NET Core-Runtime](https://github.com/dotnet/coreclr).</span><span class="sxs-lookup"><span data-stu-id="1ca12-119">Loads the [.NET Core runtime](https://github.com/dotnet/coreclr).</span></span>
* <span data-ttu-id="1ca12-120">Verwendet das erste Argument in der Befehlszeile als Pfad zur verwalteten Binärdatei, die den Einstiegspunkt (`Main`) enthält, und beginnt mit der Ausführung des Codes.</span><span class="sxs-lookup"><span data-stu-id="1ca12-120">Uses the first command-line argument as the path to the managed binary that contains the entry point (`Main`) and begins code execution.</span></span>

<span data-ttu-id="1ca12-121">Die `Main`-Methode verwendet eine Instanz von <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, die nach dem [Builder-Muster](https://wikipedia.org/wiki/Builder_pattern) einen Web-App-Host erstellt.</span><span class="sxs-lookup"><span data-stu-id="1ca12-121">The `Main` method uses <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web app host.</span></span> <span data-ttu-id="1ca12-122">Der Builder verfügt über Methoden, die den Webserver (z.B. <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) und die Startklasse (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>) definieren.</span><span class="sxs-lookup"><span data-stu-id="1ca12-122">The builder has methods that define the web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="1ca12-123">Im obigen Beispiel wird der [Kestrel](xref:fundamentals/servers/kestrel)-Webserver verwendet.</span><span class="sxs-lookup"><span data-stu-id="1ca12-123">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="1ca12-124">Andere Webserver wie [WebListener](xref:fundamentals/servers/weblistener) können durch den Aufruf der entsprechenden Erweiterungsmethode verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="1ca12-124">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="1ca12-125">`UseStartup` wird im nächsten Abschnitt näher erläutert.</span><span class="sxs-lookup"><span data-stu-id="1ca12-125">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="1ca12-126">`WebHostBuilder` stellt viele optionale Methoden wie z.B. <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> für das Hosten in IIS und IIS Express und <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> für das Festlegen des Inhaltsstammverzeichnisses zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="1ca12-126">`WebHostBuilder` provides many optional methods, including <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> for hosting in IIS and IIS Express and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="1ca12-127">Mit den Methoden <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> und <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> wird das <xref:Microsoft.AspNetCore.Hosting.IWebHost>-Objekt erstellt, das die Anwendung hostet und auf HTTP-Anforderungen lauscht.</span><span class="sxs-lookup"><span data-stu-id="1ca12-127">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

## <a name="startup"></a><span data-ttu-id="1ca12-128">Start</span><span class="sxs-lookup"><span data-stu-id="1ca12-128">Startup</span></span>

<span data-ttu-id="1ca12-129">Die `UseStartup`-Methode in `WebHostBuilder` gibt die `Startup`-Klasse für Ihre Anwendung an:</span><span class="sxs-lookup"><span data-stu-id="1ca12-129">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs?highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs?highlight=7)]

::: moniker-end

<span data-ttu-id="1ca12-130">In der `Startup`-Klasse definieren Sie die Pipeline zur Anforderungsverarbeitung. Außerdem werden in dieser Klasse alle von der Anwendung benötigten Dienste konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="1ca12-130">The `Startup` class is where you define the request handling pipeline and where any services needed by the app are configured.</span></span> <span data-ttu-id="1ca12-131">Die `Startup`-Klasse muss öffentlich sein und die folgenden Methoden enthalten:</span><span class="sxs-lookup"><span data-stu-id="1ca12-131">The `Startup` class must be public and contain the following methods:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Startup.cs)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Startup.cs)]

::: moniker-end

<span data-ttu-id="1ca12-132"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> definiert die [Dienste](#dependency-injection-services) Ihrer Anwendung (z.B. ASP.NET Core MVC, Entity Framework Core, Identity).</span><span class="sxs-lookup"><span data-stu-id="1ca12-132"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="1ca12-133"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> definiert die [Middleware](xref:fundamentals/middleware/index), die in der Anforderungspipeline aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="1ca12-133"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> defines the [middleware](xref:fundamentals/middleware/index) called in the request pipeline.</span></span>

<span data-ttu-id="1ca12-134">Weitere Informationen finden Sie unter <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="1ca12-134">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="content-root"></a><span data-ttu-id="1ca12-135">Inhaltsstammverzeichnis</span><span class="sxs-lookup"><span data-stu-id="1ca12-135">Content root</span></span>

<span data-ttu-id="1ca12-136">Das Inhaltsstammverzeichnis ist der Basispfad zu allen von der Anwendung verwendeten Inhalten wie, [Razor Pages](xref:razor-pages/index), MVC-Ansichten und statischen Objekten.</span><span class="sxs-lookup"><span data-stu-id="1ca12-136">The content root is the base path to any content used by the app, such as [Razor Pages](xref:razor-pages/index), MVC views, and static assets.</span></span> <span data-ttu-id="1ca12-137">Standardmäßig entspricht das Inhaltsstammverzeichnis dem App-Basispfad der ausführbaren Datei, mit der die App gehostet wird.</span><span class="sxs-lookup"><span data-stu-id="1ca12-137">By default, the content root is the same location as the app base path for the executable hosting the app.</span></span>

## <a name="web-root-webroot"></a><span data-ttu-id="1ca12-138">Webstamm (webroot)</span><span class="sxs-lookup"><span data-stu-id="1ca12-138">Web root (webroot)</span></span>

<span data-ttu-id="1ca12-139">Das Webstammverzeichnis einer Anwendung ist das Projektverzeichnis, in dem sich öffentliche statische Ressourcen wie etwa CSS-, JavaScript- und Imagedateien befinden.</span><span class="sxs-lookup"><span data-stu-id="1ca12-139">The webroot of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span> <span data-ttu-id="1ca12-140">*wwwroot* ist standardmäßig der Webstamm.</span><span class="sxs-lookup"><span data-stu-id="1ca12-140">By default, *wwwroot* is the webroot.</span></span>

<span data-ttu-id="1ca12-141">Für Razor-Dateien (*.cshtml*) zeigen die Tilde und der Schrägstrich `~/` auf den Webstamm.</span><span class="sxs-lookup"><span data-stu-id="1ca12-141">For Razor (*.cshtml*) files, the tilde-slash  `~/` points to the webroot.</span></span> <span data-ttu-id="1ca12-142">Pfade, die mit `~/` beginnen, werden als virtuelle Pfade bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="1ca12-142">Paths beginning with `~/` are referred to as virtual paths.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="1ca12-143">Abhängigkeitsinjektion (Dienste)</span><span class="sxs-lookup"><span data-stu-id="1ca12-143">Dependency injection (services)</span></span>

<span data-ttu-id="1ca12-144">Ein *Dienst* ist eine Komponente, die für die gemeinsame Nutzung in einer App vorgesehen ist.</span><span class="sxs-lookup"><span data-stu-id="1ca12-144">A *service* is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="1ca12-145">Dienste werden über die [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) zur Verfügung gestellt.</span><span class="sxs-lookup"><span data-stu-id="1ca12-145">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="1ca12-146">ASP.NET Core enthält einen nativen IoC-Container (Inversion of Control, Steuerungsumkehr), der die [Konstruktorinjektion](xref:mvc/controllers/dependency-injection#constructor-injection) standardmäßig unterstützt.</span><span class="sxs-lookup"><span data-stu-id="1ca12-146">ASP.NET Core includes a native Inversion of Control (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="1ca12-147">Bei Bedarf können Sie den Standardcontainer durch einen anderen ersetzen.</span><span class="sxs-lookup"><span data-stu-id="1ca12-147">You can replace the default container if you wish.</span></span> <span data-ttu-id="1ca12-148">Neben der [losen Kopplung](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) besteht ein weiterer Vorteil darin, dass von der DI Dienste in der gesamten App zur Verfügung gestellt werden (z.B. die [Protokollierung](xref:fundamentals/logging/index)).</span><span class="sxs-lookup"><span data-stu-id="1ca12-148">In addition to its [loose coupling benefit](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), DI makes services, such as [logging](xref:fundamentals/logging/index), available throughout your app.</span></span>

<span data-ttu-id="1ca12-149">Weitere Informationen finden Sie unter <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="1ca12-149">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="1ca12-150">Middleware</span><span class="sxs-lookup"><span data-stu-id="1ca12-150">Middleware</span></span>

<span data-ttu-id="1ca12-151">In ASP.NET Core erstellen Sie Ihre Anforderungspipeline mithilfe von [Middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="1ca12-151">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="1ca12-152">ASP.NET Core-Middleware führt asynchrone Operationen im `HttpContext` aus und ruft anschließend die nächste Middlewareanwendung in der Pipeline auf oder beendet die Anforderung.</span><span class="sxs-lookup"><span data-stu-id="1ca12-152">ASP.NET Core middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="1ca12-153">Eine Middlewarekomponente mit dem Namen „XYZ“ wird gemäß der Konvention durch den Aufruf einer `UseXYZ`-Erweiterungsmethode in der `Configure`-Methode der Pipeline hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="1ca12-153">By convention, a middleware component called "XYZ" is added to the pipeline by invoking a `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="1ca12-154">ASP.NET Core enthält eine Reihe umfangreicher Middleware, und außerdem können Sie benutzerdefinierte Middleware erstellen.</span><span class="sxs-lookup"><span data-stu-id="1ca12-154">ASP.NET Core includes a rich set of built-in middleware, and you can write your own custom middleware.</span></span> <span data-ttu-id="1ca12-155">[Open Web Interface for .NET (OWIN)](xref:fundamentals/owin) ermöglicht Web-Apps das Entkoppeln von Webservern und wird in ASP.NET Core unterstützt.</span><span class="sxs-lookup"><span data-stu-id="1ca12-155">[Open Web Interface for .NET (OWIN)](xref:fundamentals/owin), which allows web apps to be decoupled from web servers, is supported in ASP.NET Core apps.</span></span>

<span data-ttu-id="1ca12-156">Weitere Informationen finden Sie unter <xref:fundamentals/middleware/index> und <xref:fundamentals/owin>.</span><span class="sxs-lookup"><span data-stu-id="1ca12-156">For more information, see <xref:fundamentals/middleware/index> and <xref:fundamentals/owin>.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a><span data-ttu-id="1ca12-157">Initiieren von HTTP-Anforderungen</span><span class="sxs-lookup"><span data-stu-id="1ca12-157">Initiate HTTP requests</span></span>

<span data-ttu-id="1ca12-158"><xref:System.Net.Http.IHttpClientFactory> ist für den Zugriff auf <xref:System.Net.Http.HttpClient>-Instanzen verfügbar, um HTTP-Anforderung durchzuführen.</span><span class="sxs-lookup"><span data-stu-id="1ca12-158"><xref:System.Net.Http.IHttpClientFactory> is available to access <xref:System.Net.Http.HttpClient> instances to make HTTP requests.</span></span>

<span data-ttu-id="1ca12-159">Weitere Informationen finden Sie unter <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="1ca12-159">For more information, see <xref:fundamentals/http-requests>.</span></span>

::: moniker-end

## <a name="environments"></a><span data-ttu-id="1ca12-160">Umgebungen</span><span class="sxs-lookup"><span data-stu-id="1ca12-160">Environments</span></span>

<span data-ttu-id="1ca12-161">Umgebungen wie *Entwicklung* und *Produktion* sind in ASP.NET Core von besonderer Bedeutung und können über Umgebungsvariablen, die Einstellungsdatei sowie das Befehlszeilenargument festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="1ca12-161">Environments, such as *Development* and *Production*, are a first-class notion in ASP.NET Core and can be set using an environment variable, settings file, and command-line argument.</span></span>

<span data-ttu-id="1ca12-162">Weitere Informationen finden Sie unter <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="1ca12-162">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="hosting"></a><span data-ttu-id="1ca12-163">Hosting</span><span class="sxs-lookup"><span data-stu-id="1ca12-163">Hosting</span></span>

<span data-ttu-id="1ca12-164">ASP.NET Core-Apps konfigurieren und starten einen *Host*, der für das Starten der App und das Verwalten deren Lebensdauer verantwortlich ist.</span><span class="sxs-lookup"><span data-stu-id="1ca12-164">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="1ca12-165">Weitere Informationen finden Sie unter <xref:fundamentals/host/index>.</span><span class="sxs-lookup"><span data-stu-id="1ca12-165">For more information, see <xref:fundamentals/host/index>.</span></span>

## <a name="servers"></a><span data-ttu-id="1ca12-166">Server</span><span class="sxs-lookup"><span data-stu-id="1ca12-166">Servers</span></span>

<span data-ttu-id="1ca12-167">Das Hostingmodell von ASP.NET Core lauscht nicht direkt auf Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="1ca12-167">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="1ca12-168">Das Hostingmodell ist darauf angewiesen, dass eine HTTP-Serverimplementierung die Anforderungen an die App weiterleitet.</span><span class="sxs-lookup"><span data-stu-id="1ca12-168">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span> <span data-ttu-id="1ca12-169">Die weitergeleitete Anforderung wird von Funktionsobjekten umschlossen, auf die Sie über Schnittstellen zugreifen können.</span><span class="sxs-lookup"><span data-stu-id="1ca12-169">The forwarded request is wrapped as a set of feature objects that can be accessed through interfaces.</span></span> <span data-ttu-id="1ca12-170">ASP.NET Core enthält den verwalteten, plattformübergreifenden Webserver [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="1ca12-170">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="1ca12-171">Kestrel wird in der Regel hinter einem Produktionswebserver ausgeführt, z.B. [IIS](https://www.iis.net/) oder [Nginx](http://nginx.org) in einer Reverseproxykonfiguration.</span><span class="sxs-lookup"><span data-stu-id="1ca12-171">Kestrel is commonly run behind a production web server, such as [IIS](https://www.iis.net/) or [Nginx](http://nginx.org) in a reverse proxy configuration.</span></span> <span data-ttu-id="1ca12-172">Kestrel kann ebenfalls als öffentlich zugänglicher Edge-Server ausgeführt werden, der direkt mit dem Internet in ASP.NET Core 2.0 oder höher verknüpft ist.</span><span class="sxs-lookup"><span data-stu-id="1ca12-172">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

<span data-ttu-id="1ca12-173">Weitere Informationen finden Sie unter <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="1ca12-173">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="1ca12-174">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="1ca12-174">Configuration</span></span>

<span data-ttu-id="1ca12-175">ASP.NET Core verwendet ein Konfigurationsmodell, das auf Name/Wert-Paaren basiert.</span><span class="sxs-lookup"><span data-stu-id="1ca12-175">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="1ca12-176">Das Konfigurationsmodell basiert weder auf <xref:System.Configuration> noch auf *web.config*. Die Konfiguration erhält Einstellungen von einer geordneten Menge an Konfigurationsanbietern.</span><span class="sxs-lookup"><span data-stu-id="1ca12-176">The configuration model isn't based on <xref:System.Configuration> or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="1ca12-177">Die integrierten Konfigurationsanbieter unterstützen zahlreiche Dateiformate (XML, JSON, INI), Umgebungsvariablen und Befehlszeilenargumente.</span><span class="sxs-lookup"><span data-stu-id="1ca12-177">The built-in configuration providers support a variety of file formats (XML, JSON, INI), environment variables, and command-line arguments.</span></span> <span data-ttu-id="1ca12-178">Sie können auch benutzerdefinierte Konfigurationsanbieter erstellen.</span><span class="sxs-lookup"><span data-stu-id="1ca12-178">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="1ca12-179">Weitere Informationen finden Sie unter <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="1ca12-179">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="logging"></a><span data-ttu-id="1ca12-180">Protokollierung</span><span class="sxs-lookup"><span data-stu-id="1ca12-180">Logging</span></span>

<span data-ttu-id="1ca12-181">ASP.NET Core unterstützt eine Protokollierungs-API, die mit mehreren verschiedenen Protokollanbietern funktioniert.</span><span class="sxs-lookup"><span data-stu-id="1ca12-181">ASP.NET Core supports a Logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="1ca12-182">Integrierte Anbieter unterstützen das Senden von Protokollen an mindestens einen Zielanbieter.</span><span class="sxs-lookup"><span data-stu-id="1ca12-182">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="1ca12-183">Protokollierungsframeworks von Drittanbietern sind ebenso zulässig.</span><span class="sxs-lookup"><span data-stu-id="1ca12-183">Third-party logging frameworks can be used.</span></span>

<span data-ttu-id="1ca12-184">Weitere Informationen finden Sie unter <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="1ca12-184">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="1ca12-185">Fehlerbehandlung</span><span class="sxs-lookup"><span data-stu-id="1ca12-185">Error handling</span></span>

<span data-ttu-id="1ca12-186">ASP.NET Core verfügt über integrierte Szenarios zur Fehlerbehandlung in Apps. Dazu zählen die Ausnahmenseite für Entwickler, Seiten für benutzerdefinierte Fehler, Seiten für statischen Statuscode sowie die Startausnahmebehandlung.</span><span class="sxs-lookup"><span data-stu-id="1ca12-186">ASP.NET Core has built-in scenarios for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="1ca12-187">Weitere Informationen finden Sie unter <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="1ca12-187">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="routing"></a><span data-ttu-id="1ca12-188">Routing</span><span class="sxs-lookup"><span data-stu-id="1ca12-188">Routing</span></span>

<span data-ttu-id="1ca12-189">ASP.NET Core verfügt über Szenarios zum Routing von App-Anforderung an Routenhandler.</span><span class="sxs-lookup"><span data-stu-id="1ca12-189">ASP.NET Core offers scenarios for routing of app requests to route handlers.</span></span>

<span data-ttu-id="1ca12-190">Weitere Informationen finden Sie unter <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="1ca12-190">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="file-providers"></a><span data-ttu-id="1ca12-191">Dateianbieter</span><span class="sxs-lookup"><span data-stu-id="1ca12-191">File Providers</span></span>

<span data-ttu-id="1ca12-192">ASP.NET Core abstrahiert den Dateisystemzugriff mithilfe von Dateianbietern, wodurch eine einfache Schnittstelle zum plattformübergreifenden Arbeiten mit Dateien zur Verfügung gestellt wird.</span><span class="sxs-lookup"><span data-stu-id="1ca12-192">ASP.NET Core abstracts file system access through the use of File Providers, which offers a common interface for working with files across platforms.</span></span>

<span data-ttu-id="1ca12-193">Weitere Informationen finden Sie unter <xref:fundamentals/file-providers>.</span><span class="sxs-lookup"><span data-stu-id="1ca12-193">For more information, see <xref:fundamentals/file-providers>.</span></span>

## <a name="static-files"></a><span data-ttu-id="1ca12-194">Statische Dateien</span><span class="sxs-lookup"><span data-stu-id="1ca12-194">Static files</span></span>

<span data-ttu-id="1ca12-195">Middleware für statische Dateien kümmert sich um statische Dateien wie etwa HTML-, CSS-, Image- und JavaScript-Dateien.</span><span class="sxs-lookup"><span data-stu-id="1ca12-195">Static Files Middleware serves static files, such as HTML, CSS, image, and JavaScript files.</span></span>

<span data-ttu-id="1ca12-196">Weitere Informationen finden Sie unter <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="1ca12-196">For more information, see <xref:fundamentals/static-files>.</span></span>

## <a name="session-and-app-state"></a><span data-ttu-id="1ca12-197">Sitzungs- und Anwendungszustand</span><span class="sxs-lookup"><span data-stu-id="1ca12-197">Session and app state</span></span>

<span data-ttu-id="1ca12-198">ASP.NET Core bietet verschiedene Ansätze zum Beibehalten des Sitzungs- und Anwendungszustand, während der Benutzer eine Web-App durchsucht.</span><span class="sxs-lookup"><span data-stu-id="1ca12-198">ASP.NET Core offers several approaches to preserve session and app state while a user browses a web app.</span></span>

<span data-ttu-id="1ca12-199">Weitere Informationen finden Sie unter <xref:fundamentals/app-state>.</span><span class="sxs-lookup"><span data-stu-id="1ca12-199">For more information, see <xref:fundamentals/app-state>.</span></span>

## <a name="globalization-and-localization"></a><span data-ttu-id="1ca12-200">Globalisierung und Lokalisierung</span><span class="sxs-lookup"><span data-stu-id="1ca12-200">Globalization and localization</span></span>

<span data-ttu-id="1ca12-201">Wenn Sie eine mehrsprachige Website mit ASP.NET Core erstellen, können Sie ein breiteres Publikum erreichen.</span><span class="sxs-lookup"><span data-stu-id="1ca12-201">Creating a multilingual website with ASP.NET Core allows your site to reach a wider audience.</span></span> <span data-ttu-id="1ca12-202">ASP.NET Core stellt Dienste und Middleware für das Lokalisieren von Inhalten in verschiedene Sprachen und Kulturen bereit.</span><span class="sxs-lookup"><span data-stu-id="1ca12-202">ASP.NET Core provides services and middleware for localizing content into different languages and cultures.</span></span>

<span data-ttu-id="1ca12-203">Weitere Informationen finden Sie unter <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="1ca12-203">For more information, see <xref:fundamentals/localization>.</span></span>

## <a name="request-features"></a><span data-ttu-id="1ca12-204">Anforderungsfeatures</span><span class="sxs-lookup"><span data-stu-id="1ca12-204">Request features</span></span>

<span data-ttu-id="1ca12-205">Ausführliche Informationen zur Webserverimplementierung, die mit HTTP-Anforderungen und -antworten in Verbindung stehen, werden in Schnittstellen definiert.</span><span class="sxs-lookup"><span data-stu-id="1ca12-205">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="1ca12-206">Diese Schnittstellen werden von Serverimplementierungen und Middleware verwendet, um die Hostingpipeline der App zu erstellen und anzupassen.</span><span class="sxs-lookup"><span data-stu-id="1ca12-206">These interfaces are used by server implementations and middleware to create and modify the app's hosting pipeline.</span></span>

<span data-ttu-id="1ca12-207">Weitere Informationen finden Sie unter <xref:fundamentals/request-features>.</span><span class="sxs-lookup"><span data-stu-id="1ca12-207">For more information, see <xref:fundamentals/request-features>.</span></span>

## <a name="background-tasks"></a><span data-ttu-id="1ca12-208">Hintergrundaufgaben</span><span class="sxs-lookup"><span data-stu-id="1ca12-208">Background tasks</span></span>

<span data-ttu-id="1ca12-209">Hintergrundaufgaben werden als *gehostete Dienste* implementiert.</span><span class="sxs-lookup"><span data-stu-id="1ca12-209">Background tasks are implemented as *hosted services*.</span></span> <span data-ttu-id="1ca12-210">Ein gehosteter Dienst ist eine Klasse mit Logik für Hintergrundaufgaben, die die Schnittstelle <xref:Microsoft.Extensions.Hosting.IHostedService> implementiert.</span><span class="sxs-lookup"><span data-stu-id="1ca12-210">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span>

<span data-ttu-id="1ca12-211">Weitere Informationen finden Sie unter <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="1ca12-211">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="access-httpcontext"></a><span data-ttu-id="1ca12-212">Zugriff auf HttpContext</span><span class="sxs-lookup"><span data-stu-id="1ca12-212">Access HttpContext</span></span>

<span data-ttu-id="1ca12-213">`HttpContext` ist automatisch verfügbar, wenn Anforderungen mit Razor Pages und MVC verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="1ca12-213">`HttpContext` is automatically available when processing requests with Razor Pages and MVC.</span></span> <span data-ttu-id="1ca12-214">In Fällen, in denen `HttpContext` nicht unmittelbar verfügbar ist, können Sie auf `HttpContext` über die <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor>-Schnittstelle und deren Standardimplementierung, <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>, zugreifen.</span><span class="sxs-lookup"><span data-stu-id="1ca12-214">In circumstances where `HttpContext` isn't readily available, you can access the `HttpContext` through the <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> interface and its default implementation, <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span></span>

<span data-ttu-id="1ca12-215">Weitere Informationen finden Sie unter <xref:fundamentals/httpcontext>.</span><span class="sxs-lookup"><span data-stu-id="1ca12-215">For more information, see <xref:fundamentals/httpcontext>.</span></span>

## <a name="websockets"></a><span data-ttu-id="1ca12-216">WebSockets</span><span class="sxs-lookup"><span data-stu-id="1ca12-216">WebSockets</span></span>

<span data-ttu-id="1ca12-217">Bei [WebSockets](https://wikipedia.org/wiki/WebSocket) handelt es sich um ein Protokoll, das bidirektionale persistente Kommunikationskanäle über TCP-Verbindungen ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="1ca12-217">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="1ca12-218">Es wird in Chat-, Börsenticker- und Spiele-Apps sowie überall dort verwendet, wo Echtzeitfunktionen in einer Web-App benötigt werden.</span><span class="sxs-lookup"><span data-stu-id="1ca12-218">It's used for apps such as chat, stock tickers, games, and anywhere you desire real-time functionality in a web app.</span></span> <span data-ttu-id="1ca12-219">ASP.NET Core unterstützt WebSockets-Szenarios.</span><span class="sxs-lookup"><span data-stu-id="1ca12-219">ASP.NET Core supports web socket scenarios.</span></span>

<span data-ttu-id="1ca12-220">Weitere Informationen finden Sie unter <xref:fundamentals/websockets>.</span><span class="sxs-lookup"><span data-stu-id="1ca12-220">For more information, see <xref:fundamentals/websockets>.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="microsoftaspnetcoreapp-metapackage"></a><span data-ttu-id="1ca12-221">Metapaket „Microsoft.AspNetCore.App“</span><span class="sxs-lookup"><span data-stu-id="1ca12-221">Microsoft.AspNetCore.App metapackage</span></span>

<span data-ttu-id="1ca12-222">Mit dem Metapaket [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) wird die Paketverwaltung vereinfacht.</span><span class="sxs-lookup"><span data-stu-id="1ca12-222">The [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) metapackage simplifies package management.</span></span>

<span data-ttu-id="1ca12-223">Weitere Informationen finden Sie unter <xref:fundamentals/metapackage-app>.</span><span class="sxs-lookup"><span data-stu-id="1ca12-223">For more information, see <xref:fundamentals/metapackage-app>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="microsoftaspnetcoreall-metapackage"></a><span data-ttu-id="1ca12-224">Metapaket „Microsoft.AspNetCore.All“</span><span class="sxs-lookup"><span data-stu-id="1ca12-224">Microsoft.AspNetCore.All metapackage</span></span>

<span data-ttu-id="1ca12-225">Das Metapaket [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) für ASP.NET Core enthält:</span><span class="sxs-lookup"><span data-stu-id="1ca12-225">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="1ca12-226">alle unterstützten Pakete des ASP.NET Core-Teams</span><span class="sxs-lookup"><span data-stu-id="1ca12-226">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="1ca12-227">alle unterstützten Pakete von Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="1ca12-227">All supported packages by Entity Framework Core.</span></span>
* <span data-ttu-id="1ca12-228">interne und Drittanbieterabhängigkeiten, die von ASP.NET Core und Entity Framework Core verwendet werden</span><span class="sxs-lookup"><span data-stu-id="1ca12-228">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="1ca12-229">Weitere Informationen finden Sie unter <xref:fundamentals/metapackage>.</span><span class="sxs-lookup"><span data-stu-id="1ca12-229">For more information, see <xref:fundamentals/metapackage>.</span></span>

::: moniker-end

## <a name="net-core-vs-net-framework-runtime"></a><span data-ttu-id="1ca12-230">.NET Core-Runtime im Vergleich zur .NET Framework-Laufzeit</span><span class="sxs-lookup"><span data-stu-id="1ca12-230">.NET Core vs. .NET Framework runtime</span></span>

<span data-ttu-id="1ca12-231">Eine ASP.NET Core-Anwendung kann für die .NET Core- oder .NET Framework-Laufzeit entwickelt werden.</span><span class="sxs-lookup"><span data-stu-id="1ca12-231">An ASP.NET Core app can target the .NET Core or .NET Framework runtime.</span></span>

<span data-ttu-id="1ca12-232">Weitere Informationen finden Sie unter [Wahl zwischen .NET Core und .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="1ca12-232">For more information, see [Choosing between .NET Core and .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="choose-between-aspnet-core-and-aspnet"></a><span data-ttu-id="1ca12-233">Wählen zwischen ASP.NET und ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1ca12-233">Choose between ASP.NET Core and ASP.NET</span></span>

<span data-ttu-id="1ca12-234">Weitere Informationen, die Ihnen die Wahl zwischen ASP.NET Core und ASP.NET erleichtern, finden Sie unter <xref:fundamentals/choose-between-aspnet-and-aspnetcore></span><span class="sxs-lookup"><span data-stu-id="1ca12-234">For more information on choosing between ASP.NET Core and ASP.NET, see <xref:fundamentals/choose-between-aspnet-and-aspnetcore>.</span></span>
