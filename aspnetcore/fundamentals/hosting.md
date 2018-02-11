---
title: Hosting in ASP.NET Core
author: guardrex
description: "Erfahren Sie mehr über den Webhost in ASP.NET Core, der für das Starten der App und das Verwalten der Lebensdauer verantwortlich ist."
manager: wpickett
ms.author: riande
ms.date: 09/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/hosting
ms.openlocfilehash: 004487aebe5262a515e2375c30ccd2a84844dff6
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="a3423-103">Hosting in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a3423-103">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="a3423-104">Von [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a3423-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="a3423-105">Durch ASP.NET Core-Apps kann ein *Host* gestartet und konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="a3423-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="a3423-106">Der Host ist verantwortlich für das Starten der App und das Verwalten der Lebensdauer.</span><span class="sxs-lookup"><span data-stu-id="a3423-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="a3423-107">Der Host konfiguriert mindestens einen Server und eine Pipeline für die Anforderungsverarbeitung.</span><span class="sxs-lookup"><span data-stu-id="a3423-107">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="a3423-108">Einrichten eines Hosts</span><span class="sxs-lookup"><span data-stu-id="a3423-108">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a3423-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a3423-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a3423-110">Erstellen Sie einen Host mithilfe einer Instanz von [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="a3423-110">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="a3423-111">Dies wird üblicherweise am Einstiegspunkt der App (die `Main`-Methode) durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="a3423-111">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="a3423-112">In den Projektvorlagen befindet `Main` sich in *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="a3423-112">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="a3423-113">Eine typische *Program.cs*-Datei ruft [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) auf, um die Einrichtung eines Hosts zu starten:</span><span class="sxs-lookup"><span data-stu-id="a3423-113">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="a3423-114">`CreateDefaultBuilder` führt folgende Ausgaben aus:</span><span class="sxs-lookup"><span data-stu-id="a3423-114">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="a3423-115">Konfiguriert [Kestrel](servers/kestrel.md) als Webserver.</span><span class="sxs-lookup"><span data-stu-id="a3423-115">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="a3423-116">Weitere Informationen zu den Standardoptionen von Kestrel finden Sie im Abschnitt „Kestrel-Optionen“ unter [Introduction to Kestrel web server implementation in ASP.NET Core (Einführung in die Implementierung des Kestrel-Webservers in ASP.NET Core)](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="a3423-116">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="a3423-117">Legt das Inhaltsstammverzeichnis auf den Pfad fest, der von [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory) zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="a3423-117">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="a3423-118">Lädt optionale Konfigurationen aus:</span><span class="sxs-lookup"><span data-stu-id="a3423-118">Loads optional configuration from:</span></span>
  * <span data-ttu-id="a3423-119">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="a3423-119">*appsettings.json*.</span></span>
  * <span data-ttu-id="a3423-120">*appsettings.{Environment}.json*</span><span class="sxs-lookup"><span data-stu-id="a3423-120">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="a3423-121">[Vertraulichen Benutzerdaten](xref:security/app-secrets), wenn die App in der `Development`-Umgebung ausgeführt wird</span><span class="sxs-lookup"><span data-stu-id="a3423-121">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="a3423-122">Umgebungsvariablen.</span><span class="sxs-lookup"><span data-stu-id="a3423-122">Environment variables.</span></span>
  * <span data-ttu-id="a3423-123">Befehlszeilenargumenten</span><span class="sxs-lookup"><span data-stu-id="a3423-123">Command-line arguments.</span></span>
* <span data-ttu-id="a3423-124">Konfiguriert die [Protokollierung](xref:fundamentals/logging/index) für die Konsolen- und Debugausgabe</span><span class="sxs-lookup"><span data-stu-id="a3423-124">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="a3423-125">Die Protokollierung umfasst Regeln zur [Protokollfilterung](xref:fundamentals/logging/index#log-filtering), die im Abschnitt für die Protokollierungskonfiguration einer *appsettings.json*- oder *appsettings.{Environment}.json*-Datei angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="a3423-125">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="a3423-126">Wenn diese im Hintergrund von IIS ausgeführt wird, aktivieren Sie die [IIS-Integration](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="a3423-126">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="a3423-127">Konfiguriert den Basispfad und den Basisport, dem der Server bei Verwendung des [ASP.NET Core-Moduls](xref:fundamentals/servers/aspnet-core-module) lauscht.</span><span class="sxs-lookup"><span data-stu-id="a3423-127">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="a3423-128">Das Modul erstellt ein Reverseproxy zwischen IIS und Kestrel.</span><span class="sxs-lookup"><span data-stu-id="a3423-128">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="a3423-129">Es konfiguriert ebenfalls die App für das [Erfassen von Startfehlern](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="a3423-129">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="a3423-130">Weitere Informationen zu den IIS-Standardoptionen finden Sie im Abschnitt „IIS-Optionen“ unter [Host ASP.NET Core on Windows with IIS (Hosten von ASP.NET Core unter Windows mit IIS)](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="a3423-130">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="a3423-131">Das *Inhaltsstammverzeichnis* bestimmt, wo der Host nach Inhaltsdateien (z.B. MVC-Ansichtsdateien) sucht.</span><span class="sxs-lookup"><span data-stu-id="a3423-131">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="a3423-132">Wenn die App aus dem Stammordner des Projekts gestartet wird, wird dieser als Inhaltsstammverzeichnis verwendet.</span><span class="sxs-lookup"><span data-stu-id="a3423-132">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="a3423-133">Dies ist die Standardeinstellung in [Visual Studio](https://www.visualstudio.com/) und für [neue .NET-Vorlagen](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="a3423-133">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="a3423-134">Weitere Informationen zur App-Konfiguration finden Sie unter [Konfiguration in ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="a3423-134">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="a3423-135">Als Alternative zur Verwendung der statischen `CreateDefaultBuilder`-Methode ist das Erstellen eines Hosts von [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) ein unterstützter Ansatz mit ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="a3423-135">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="a3423-136">Weitere Informationen finden Sie auf der Registerkarte zu ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="a3423-136">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a3423-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a3423-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a3423-138">Erstellen Sie einen Host mithilfe einer Instanz von [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="a3423-138">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="a3423-139">Das Erstellen eines Hosts wird üblicherweise am Einstiegspunkt der App (die `Main`-Methode) durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="a3423-139">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="a3423-140">In den Projektvorlagen befindet `Main` sich in *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="a3423-140">In the project templates, `Main` is located in *Program.cs*:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="a3423-141">`WebHostBuilder` erfordert einen [Server, der IServer implementiert](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="a3423-141">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="a3423-142">Bei den integrierten Servern handelt es sich um [Kestrel](servers/kestrel.md) und [HTTP.sys](servers/httpsys.md) (vor dem Release von ASP.NET Core 2.0 hieß „HTTP.sys“ [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="a3423-142">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="a3423-143">In diesem Beispiel wird der Kestrel-Server von der [UseKestrel-Erweiterungsmethode](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) angegeben.</span><span class="sxs-lookup"><span data-stu-id="a3423-143">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="a3423-144">Das *Inhaltsstammverzeichnis* bestimmt, wo der Host nach Inhaltsdateien (z.B. MVC-Ansichtsdateien) sucht.</span><span class="sxs-lookup"><span data-stu-id="a3423-144">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="a3423-145">Das Standardinhaltsstammverzeichnis für `UseContentRoot` wird von [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1) abgerufen.</span><span class="sxs-lookup"><span data-stu-id="a3423-145">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="a3423-146">Wenn die App aus dem Stammordner des Projekts gestartet wird, wird dieser als Inhaltsstammverzeichnis verwendet.</span><span class="sxs-lookup"><span data-stu-id="a3423-146">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="a3423-147">Dies ist die Standardeinstellung in [Visual Studio](https://www.visualstudio.com/) und für [neue .NET-Vorlagen](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="a3423-147">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="a3423-148">Rufen Sie [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) im Rahmen der Erstellung des Hosts auf, um IIS als Reverseproxy zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="a3423-148">To use IIS as a reverse proxy, call [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="a3423-149">`UseIISIntegration` konfiguriert anders als [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) keinen *Server*.</span><span class="sxs-lookup"><span data-stu-id="a3423-149">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="a3423-150">`UseIISIntegration` konfiguriert den Basispfad und den Basisport, dem der Server bei Verwendung des [ASP.NET Core-Moduls](xref:fundamentals/servers/aspnet-core-module) lauscht, um ein Reverseproxy zwischen Kestrel und IIS zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="a3423-150">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="a3423-151">`UseKestrel` und `UseIISIntegration` müssen festgelegt sein, um IIS mit ASP.NET Core verwenden zu können.</span><span class="sxs-lookup"><span data-stu-id="a3423-151">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="a3423-152">Die `UseIISIntegration`-Methode wird nur aktiviert, wenn diese im Hintergrund von IIS oder IIS Express ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="a3423-152">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="a3423-153">Weitere Informationen finden Sie unter [Einführung in das ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) und [Konfigurationsreferenz für das ASP.NET Core-Modul](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="a3423-153">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="a3423-154">Eine minimale Implementierung, die einen Host (und eine ASP.NET Core-App) konfiguriert, umfasst das Festlegen eines Servers und die Konfiguration der Anforderungspipeline der App:</span><span class="sxs-lookup"><span data-stu-id="a3423-154">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .Configure(app =>
    {
        app.Run(context => context.Response.WriteAsync("Hello World!"));
    })
    .Build();

host.Run();
```

---

<span data-ttu-id="a3423-155">Beim Einrichten eines Hosts können die Methoden [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) und [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="a3423-155">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="a3423-156">Wenn eine `Startup`-Klasse angegeben wird, muss diese eine `Configure`-Methode definieren.</span><span class="sxs-lookup"><span data-stu-id="a3423-156">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="a3423-157">Weitere Informationen finden Sie unter [Application Startup in ASP.NET Core (Starten von Anwendungen in ASP.NET Core)](startup.md).</span><span class="sxs-lookup"><span data-stu-id="a3423-157">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="a3423-158">Wenn `ConfigureServices` mehrmals aufgerufen wird, werden diese Aufrufe einander angefügt.</span><span class="sxs-lookup"><span data-stu-id="a3423-158">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="a3423-159">Durch mehrere Aufrufe von `Configure` oder `UseStartup` in `WebHostBuilder` werden die vorherigen Einstellungen ersetzt.</span><span class="sxs-lookup"><span data-stu-id="a3423-159">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="a3423-160">Hostkonfigurationswerte</span><span class="sxs-lookup"><span data-stu-id="a3423-160">Host configuration values</span></span>

<span data-ttu-id="a3423-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) basiert auf folgenden Ansätzen, um die Hostkonfigurationswerte festzulegen:</span><span class="sxs-lookup"><span data-stu-id="a3423-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="a3423-162">Hostbuilderkonfigurationen, die Umgebungsvariablen im Format `ASPNETCORE_{configurationKey}` enthalten.</span><span class="sxs-lookup"><span data-stu-id="a3423-162">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="a3423-163">Beispielsweise `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="a3423-163">For example, `ASPNETCORE_URLS`.</span></span>
* <span data-ttu-id="a3423-164">Explizite Methoden wie `CaptureStartupErrors`.</span><span class="sxs-lookup"><span data-stu-id="a3423-164">Explicit methods, such as `CaptureStartupErrors`.</span></span>
* <span data-ttu-id="a3423-165">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) und der zugeordnete Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="a3423-165">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="a3423-166">Wenn Sie einen Wert mit `UseSetting` festlegen, wird dieser unabhängig vom Typ als Zeichenfolge festgelegt.</span><span class="sxs-lookup"><span data-stu-id="a3423-166">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="a3423-167">Der Host verwendet die Option, die zuletzt einen Wert festlegt.</span><span class="sxs-lookup"><span data-stu-id="a3423-167">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="a3423-168">Weitere Informationen finden Sie im nächsten Abschnitt unter [Außerkraftsetzen der Konfiguration](#overriding-configuration).</span><span class="sxs-lookup"><span data-stu-id="a3423-168">For more information, see [Overriding configuration](#overriding-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="a3423-169">Erfassen von Startfehlern</span><span class="sxs-lookup"><span data-stu-id="a3423-169">Capture Startup Errors</span></span>

<span data-ttu-id="a3423-170">Diese Einstellung steuert das Erfassen von Startfehlern.</span><span class="sxs-lookup"><span data-stu-id="a3423-170">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="a3423-171">**Schlüssel:** captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="a3423-171">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="a3423-172">**Typ:** *Boolesch* (`true` or `1`)</span><span class="sxs-lookup"><span data-stu-id="a3423-172">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="a3423-173">**Standard:** Die Standardeinstellung ist `false`, wenn die App nicht mit Kestrel im Hintergrund von IIS ausgeführt wird, dann ist diese `true`.</span><span class="sxs-lookup"><span data-stu-id="a3423-173">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="a3423-174">**Festlegen mit:** `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="a3423-174">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="a3423-175">**Umgebungsvariable:** `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="a3423-175">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="a3423-176">Wenn diese `false` ist, führen Fehler beim Start zum Beenden des Hosts.</span><span class="sxs-lookup"><span data-stu-id="a3423-176">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="a3423-177">Wenn diese `true` ist, erfasst der Host Ausnahmen während des Starts und versucht, den Server zu starten.</span><span class="sxs-lookup"><span data-stu-id="a3423-177">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a3423-178">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a3423-178">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a3423-179">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a3423-179">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="a3423-180">Inhaltsstammverzeichnis</span><span class="sxs-lookup"><span data-stu-id="a3423-180">Content Root</span></span>

<span data-ttu-id="a3423-181">Diese Einstellung bestimmt, wo ASP.NET mit der Suche nach Inhaltsdateien (z.B. MVC-Ansichten) beginnt.</span><span class="sxs-lookup"><span data-stu-id="a3423-181">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="a3423-182">**Schlüssel:** contentRoot</span><span class="sxs-lookup"><span data-stu-id="a3423-182">**Key**: contentRoot</span></span>  
<span data-ttu-id="a3423-183">**Typ:** *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="a3423-183">**Type**: *string*</span></span>  
<span data-ttu-id="a3423-184">**Standard:** Entspricht standardmäßig dem Ordner, in dem die App-Assembly gespeichert ist.</span><span class="sxs-lookup"><span data-stu-id="a3423-184">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="a3423-185">**Festlegen mit:** `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="a3423-185">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="a3423-186">**Umgebungsvariable:** `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="a3423-186">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="a3423-187">Das Inhaltsstammverzeichnis wird ebenfalls als Basispfad für die [Webstammeinstellung](#web-root) verwendet.</span><span class="sxs-lookup"><span data-stu-id="a3423-187">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="a3423-188">Wenn der Pfad nicht vorhanden ist, kann der Host nicht gestartet werden.</span><span class="sxs-lookup"><span data-stu-id="a3423-188">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a3423-189">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a3423-189">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a3423-190">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a3423-190">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="a3423-191">Detaillierte Fehler</span><span class="sxs-lookup"><span data-stu-id="a3423-191">Detailed Errors</span></span>

<span data-ttu-id="a3423-192">Bestimmt, ob detaillierte Fehler erfasst werden sollen.</span><span class="sxs-lookup"><span data-stu-id="a3423-192">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="a3423-193">**Schlüssel:** detailedErrors</span><span class="sxs-lookup"><span data-stu-id="a3423-193">**Key**: detailedErrors</span></span>  
<span data-ttu-id="a3423-194">**Typ:** *Boolesch* (`true` or `1`)</span><span class="sxs-lookup"><span data-stu-id="a3423-194">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="a3423-195">**Standard:** FALSE</span><span class="sxs-lookup"><span data-stu-id="a3423-195">**Default**: false</span></span>  
<span data-ttu-id="a3423-196">**Festlegen mit:** `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="a3423-196">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="a3423-197">**Umgebungsvariable:** `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="a3423-197">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="a3423-198">Wenn diese Einstellung aktiviert ist (oder die <a href="#environment">Umgebung</a> auf `Development` festgelegt ist), erfasst die App detaillierte Ausnahmen.</span><span class="sxs-lookup"><span data-stu-id="a3423-198">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a3423-199">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a3423-199">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a3423-200">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a3423-200">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="a3423-201">Umgebung</span><span class="sxs-lookup"><span data-stu-id="a3423-201">Environment</span></span>

<span data-ttu-id="a3423-202">Legt die Umgebung der App fest.</span><span class="sxs-lookup"><span data-stu-id="a3423-202">Sets the app's environment.</span></span>

<span data-ttu-id="a3423-203">**Schlüssel:** environment</span><span class="sxs-lookup"><span data-stu-id="a3423-203">**Key**: environment</span></span>  
<span data-ttu-id="a3423-204">**Typ:** *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="a3423-204">**Type**: *string*</span></span>  
<span data-ttu-id="a3423-205">**Standard:** Produktion</span><span class="sxs-lookup"><span data-stu-id="a3423-205">**Default**: Production</span></span>  
<span data-ttu-id="a3423-206">**Festlegen mit:** `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="a3423-206">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="a3423-207">**Umgebungsvariable:** `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="a3423-207">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="a3423-208">Die Umgebung kann auf einen beliebigen Wert festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="a3423-208">The environment can be set to any value.</span></span> <span data-ttu-id="a3423-209">Zu den durch Frameworks definierten Werten zählen `Development`, `Staging` und `Production`.</span><span class="sxs-lookup"><span data-stu-id="a3423-209">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="a3423-210">Die Groß-/Kleinschreibung wird bei Werten nicht beachtet.</span><span class="sxs-lookup"><span data-stu-id="a3423-210">Values aren't case sensitive.</span></span> <span data-ttu-id="a3423-211">Standardmäßig wird die *Umgebung* aus der `ASPNETCORE_ENVIRONMENT`-Umgebungsvariablen gelesen.</span><span class="sxs-lookup"><span data-stu-id="a3423-211">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="a3423-212">Wenn Sie [Visual Studio](https://www.visualstudio.com/) verwenden, werden Umgebungsvariablen möglicherweise in der Datei *launchSettings.json* festgelegt.</span><span class="sxs-lookup"><span data-stu-id="a3423-212">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="a3423-213">Weitere Informationen finden Sie unter [Working with Multiple Environments (Verwenden von mehreren Umgebungen)](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="a3423-213">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a3423-214">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a3423-214">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a3423-215">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a3423-215">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="a3423-216">Hostingstartassemblys</span><span class="sxs-lookup"><span data-stu-id="a3423-216">Hosting Startup Assemblies</span></span>

<span data-ttu-id="a3423-217">Legt die Hostingstartassemblys der App fest.</span><span class="sxs-lookup"><span data-stu-id="a3423-217">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="a3423-218">**Schlüssel:** hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="a3423-218">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="a3423-219">**Typ:** *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="a3423-219">**Type**: *string*</span></span>  
<span data-ttu-id="a3423-220">**Standard:** Leere Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="a3423-220">**Default**: Empty string</span></span>  
<span data-ttu-id="a3423-221">**Festlegen mit:** `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="a3423-221">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="a3423-222">**Umgebungsvariable:** `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="a3423-222">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="a3423-223">Eine durch Semikolons getrennte Zeichenfolge der Hostingstartassemblys, die beim Start geladen werden soll.</span><span class="sxs-lookup"><span data-stu-id="a3423-223">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="a3423-224">Dieses Feature ist neu in ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="a3423-224">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="a3423-225">Obwohl der Konfigurationswert standardmäßig auf eine leere Zeichenfolge festgelegt ist, enthalten die Hostingstartassemblys immer die Assembly der App.</span><span class="sxs-lookup"><span data-stu-id="a3423-225">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="a3423-226">Wenn Hostingstartassemblys bereitgestellt werden, werden diese zur Assembly der App hinzugefügt, damit diese geladen werden, wenn die App während des Starts die allgemeinen Dienste erstellt.</span><span class="sxs-lookup"><span data-stu-id="a3423-226">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a3423-227">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a3423-227">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a3423-228">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a3423-228">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a3423-229">Dieses Feature ist in ASP.NET Core 1.x nicht verfügbar.</span><span class="sxs-lookup"><span data-stu-id="a3423-229">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="a3423-230">Bevorzugen von Hosting-URLs</span><span class="sxs-lookup"><span data-stu-id="a3423-230">Prefer Hosting URLs</span></span>

<span data-ttu-id="a3423-231">Gibt an, ob der Hosts den URLs lauschen soll, die mit `WebHostBuilder` konfiguriert wurden, statt denen, die mit der `IServer`-Implementierung konfiguriert wurden.</span><span class="sxs-lookup"><span data-stu-id="a3423-231">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="a3423-232">**Schlüssel:** preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="a3423-232">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="a3423-233">**Typ:** *Boolesch* (`true` or `1`)</span><span class="sxs-lookup"><span data-stu-id="a3423-233">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="a3423-234">**Standard:** TRUE</span><span class="sxs-lookup"><span data-stu-id="a3423-234">**Default**: true</span></span>  
<span data-ttu-id="a3423-235">**Festlegen mit:** `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="a3423-235">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="a3423-236">**Umgebungsvariable:** `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="a3423-236">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="a3423-237">Dieses Feature ist neu in ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="a3423-237">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a3423-238">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a3423-238">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a3423-239">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a3423-239">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a3423-240">Dieses Feature ist in ASP.NET Core 1.x nicht verfügbar.</span><span class="sxs-lookup"><span data-stu-id="a3423-240">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="a3423-241">Verhindern des Hostingstarts</span><span class="sxs-lookup"><span data-stu-id="a3423-241">Prevent Hosting Startup</span></span>

<span data-ttu-id="a3423-242">Verhindert das automatische Laden der Hostingstartassemblys, einschließlich denen, die von der Assembly der App konfiguriert wurden.</span><span class="sxs-lookup"><span data-stu-id="a3423-242">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="a3423-243">Weitere Informationen finden Sie unter [Add app features from an external assembly using IHostingStartup (Hinzufügen von App-Features aus einer externen Assembly mithilfe von IHostingStartup)](xref:host-and-deploy/ihostingstartup).</span><span class="sxs-lookup"><span data-stu-id="a3423-243">See [Add app features from an external assembly using IHostingStartup](xref:host-and-deploy/ihostingstartup) for more information.</span></span>

<span data-ttu-id="a3423-244">**Schlüssel:** preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="a3423-244">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="a3423-245">**Typ:** *Boolesch* (`true` or `1`)</span><span class="sxs-lookup"><span data-stu-id="a3423-245">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="a3423-246">**Standard:** FALSE</span><span class="sxs-lookup"><span data-stu-id="a3423-246">**Default**: false</span></span>  
<span data-ttu-id="a3423-247">**Festlegen mit:** `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="a3423-247">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="a3423-248">**Umgebungsvariable:** `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="a3423-248">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="a3423-249">Dieses Feature ist neu in ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="a3423-249">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a3423-250">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a3423-250">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a3423-251">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a3423-251">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a3423-252">Dieses Feature ist in ASP.NET Core 1.x nicht verfügbar.</span><span class="sxs-lookup"><span data-stu-id="a3423-252">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="a3423-253">Server-URLs</span><span class="sxs-lookup"><span data-stu-id="a3423-253">Server URLs</span></span>

<span data-ttu-id="a3423-254">Gibt die IP-Adressen oder Hostadressen mit Ports und Protokollen an, denen der Server für Anforderungen lauschen soll.</span><span class="sxs-lookup"><span data-stu-id="a3423-254">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="a3423-255">**Schlüssel:** urls</span><span class="sxs-lookup"><span data-stu-id="a3423-255">**Key**: urls</span></span>  
<span data-ttu-id="a3423-256">**Typ:** *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="a3423-256">**Type**: *string*</span></span>  
<span data-ttu-id="a3423-257">**Standard:** http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="a3423-257">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="a3423-258">**Festlegen mit:** `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="a3423-258">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="a3423-259">**Umgebungsvariable:** `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="a3423-259">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="a3423-260">Legt die Einstellung auf eine durch Semikolons (;) getrennte Liste von URL-Präfixen fest, auf die der Server reagieren soll.</span><span class="sxs-lookup"><span data-stu-id="a3423-260">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="a3423-261">Beispielsweise `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="a3423-261">For example, `http://localhost:123`.</span></span> <span data-ttu-id="a3423-262">Verwenden Sie \*, um anzugeben, dass der Server mithilfe des angegebenen Ports und Protokolls (z.B. `http://*:5000`) den Anfragen aller IP-Adressen oder Hostnamen lauschen soll.</span><span class="sxs-lookup"><span data-stu-id="a3423-262">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="a3423-263">Das Protokoll (`http://` oder `https://`) muss in jeder URL enthalten sein.</span><span class="sxs-lookup"><span data-stu-id="a3423-263">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="a3423-264">Die unterstützten Formate unterscheiden sich bei verschiedenen Servern.</span><span class="sxs-lookup"><span data-stu-id="a3423-264">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a3423-265">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a3423-265">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="a3423-266">Kestrel verfügt über eine eigene API für die Endpunktkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="a3423-266">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="a3423-267">Weitere Informationen finden Sie unter [Kestrel web server implementation in ASP.NET Core (Kestrel-Webserverimplementierung in ASP.NET Core)](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="a3423-267">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a3423-268">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a3423-268">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="a3423-269">Timeout beim Herunterfahren</span><span class="sxs-lookup"><span data-stu-id="a3423-269">Shutdown Timeout</span></span>

<span data-ttu-id="a3423-270">Gibt die Wartezeit an, bis der Webhost heruntergefahren wird.</span><span class="sxs-lookup"><span data-stu-id="a3423-270">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="a3423-271">**Schlüssel:** shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="a3423-271">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="a3423-272">**Typ:** *Ganze Zahl*</span><span class="sxs-lookup"><span data-stu-id="a3423-272">**Type**: *int*</span></span>  
<span data-ttu-id="a3423-273">**Standard:** 5</span><span class="sxs-lookup"><span data-stu-id="a3423-273">**Default**: 5</span></span>  
<span data-ttu-id="a3423-274">**Festlegen mit:** `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="a3423-274">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="a3423-275">**Umgebungsvariable:** `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="a3423-275">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="a3423-276">Obwohl der Schlüssel eine *ganze Zahl* mit `UseSetting` (z.B. `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`) akzeptiert, akzeptiert die `UseShutdownTimeout`-Erweiterungsmethode ein `TimeSpan`-Objekt.</span><span class="sxs-lookup"><span data-stu-id="a3423-276">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the `UseShutdownTimeout` extension method takes a `TimeSpan`.</span></span> <span data-ttu-id="a3423-277">Dieses Feature ist neu in ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="a3423-277">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a3423-278">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a3423-278">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a3423-279">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a3423-279">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a3423-280">Dieses Feature ist in ASP.NET Core 1.x nicht verfügbar.</span><span class="sxs-lookup"><span data-stu-id="a3423-280">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="a3423-281">Startassembly</span><span class="sxs-lookup"><span data-stu-id="a3423-281">Startup Assembly</span></span>

<span data-ttu-id="a3423-282">Bestimmt die Assembly, die nach der `Startup`-Klasse suchen soll.</span><span class="sxs-lookup"><span data-stu-id="a3423-282">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="a3423-283">**Schlüssel:** startupAssembly</span><span class="sxs-lookup"><span data-stu-id="a3423-283">**Key**: startupAssembly</span></span>  
<span data-ttu-id="a3423-284">**Typ:** *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="a3423-284">**Type**: *string*</span></span>  
<span data-ttu-id="a3423-285">**Standard:** Die Assembly der App</span><span class="sxs-lookup"><span data-stu-id="a3423-285">**Default**: The app's assembly</span></span>  
<span data-ttu-id="a3423-286">**Festlegen mit:** `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="a3423-286">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="a3423-287">**Umgebungsvariable:** `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="a3423-287">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="a3423-288">Es kann auf die Assembly nach Name (`string`) oder Typ (`TStartup`) verwiesen werden.</span><span class="sxs-lookup"><span data-stu-id="a3423-288">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="a3423-289">Wenn mehrere `UseStartup`-Methoden aufgerufen werden, hat die letzte Vorrang.</span><span class="sxs-lookup"><span data-stu-id="a3423-289">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a3423-290">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a3423-290">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a3423-291">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a3423-291">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
    ...
```

---

### <a name="web-root"></a><span data-ttu-id="a3423-292">Webstammverzeichnis</span><span class="sxs-lookup"><span data-stu-id="a3423-292">Web Root</span></span>

<span data-ttu-id="a3423-293">Legt den relativen Pfad für die statischen Objekte der App fest.</span><span class="sxs-lookup"><span data-stu-id="a3423-293">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="a3423-294">**Schlüssel:** webroot</span><span class="sxs-lookup"><span data-stu-id="a3423-294">**Key**: webroot</span></span>  
<span data-ttu-id="a3423-295">**Typ:** *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="a3423-295">**Type**: *string*</span></span>  
<span data-ttu-id="a3423-296">**Standard:** Wenn nichts anderes angegeben und der Pfad vorhanden ist, ist der Standardwert „(Content Root)/wwwroot“.</span><span class="sxs-lookup"><span data-stu-id="a3423-296">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="a3423-297">Wenn der Pfad nicht vorhanden ist, wird ein Dateianbieter ohne Funktion verwendet.</span><span class="sxs-lookup"><span data-stu-id="a3423-297">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="a3423-298">**Festlegen mit:** `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="a3423-298">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="a3423-299">**Umgebungsvariable:** `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="a3423-299">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a3423-300">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a3423-300">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a3423-301">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a3423-301">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="a3423-302">Außerkraftsetzen der Konfiguration</span><span class="sxs-lookup"><span data-stu-id="a3423-302">Overriding configuration</span></span>

<span data-ttu-id="a3423-303">Verwenden Sie [Konfiguration](xref:fundamentals/configuration/index), um den Host zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="a3423-303">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="a3423-304">Im folgenden Beispiel wird die Hostkonfiguration optional in einer *hosting.json*-Datei angegeben.</span><span class="sxs-lookup"><span data-stu-id="a3423-304">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="a3423-305">Jede Konfiguration, die aus der *hosting.json*-Datei geladen wird, kann von Befehlszeilenargumenten außer Kraft gesetzt werden.</span><span class="sxs-lookup"><span data-stu-id="a3423-305">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="a3423-306">Die erstellte Konfiguration (in `config`) wird verwendet, um den Host mit `UseConfiguration` zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="a3423-306">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a3423-307">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a3423-307">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a3423-308">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="a3423-308">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="a3423-309">Das Außerkraftsetzen der Konfiguration wird von `UseUrls` bereitgestellt, hierbei kommt die Konfiguration *hosting.json* zuerst und anschließend die Konfiguration der Befehlszeilenargumente:</span><span class="sxs-lookup"><span data-stu-id="a3423-309">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();
    }
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a3423-310">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a3423-310">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a3423-311">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="a3423-311">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="a3423-312">Das Außerkraftsetzen der Konfiguration wird von `UseUrls` bereitgestellt, hierbei kommt die Konfiguration *hosting.json* zuerst und anschließend die Konfiguration der Befehlszeilenargumente:</span><span class="sxs-lookup"><span data-stu-id="a3423-312">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
            .AddCommandLine(args)
            .Build();

        var host = new WebHostBuilder()
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .UseKestrel()
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();

        host.Run();
    }
}
```

---

> [!NOTE]
> <span data-ttu-id="a3423-313">Die [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration)-Erweiterungsmethode kann derzeit keine Konfigurationsabschnitte (z.B. `.UseConfiguration(Configuration.GetSection("section"))`) analysieren, die von `GetSection` zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="a3423-313">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="a3423-314">Die `GetSection`-Methode filtert die Konfigurationsschlüssel des angeforderten Abschnitts, behält jedoch den Abschnittsnamen in den Schlüsseln bei (z.B. `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="a3423-314">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="a3423-315">Die `UseConfiguration`-Methode erwartet, dass die Schlüssel den `WebHostBuilder`-Schlüsseln entsprechen (z.B. `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="a3423-315">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="a3423-316">Das Vorhandensein des Abschnittsnamens in den Schlüsseln verhindert, dass die Werte des Abschnitts den Host konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="a3423-316">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="a3423-317">Dieses Problem wird in einer bevorstehenden Version behoben.</span><span class="sxs-lookup"><span data-stu-id="a3423-317">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="a3423-318">Weitere Informationen und Problemumgehungen finden Sie unter [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys (Beim Übergeben des Konfigurationsabschnitts an WebHostBuilder.UseConfiguration werden vollständige Schlüssel verwendet)](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="a3423-318">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="a3423-319">Damit der Host auf einer bestimmten URL ausgeführt wird, kann der gewünschte Wert von der Befehlszeile aus übergeben werden, wenn `dotnet run` ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="a3423-319">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing `dotnet run`.</span></span> <span data-ttu-id="a3423-320">Das Befehlszeilenargument überschreibt den `urls`-Wert der *hosting.json*-Datei, und der Server lauscht Port 8080:</span><span class="sxs-lookup"><span data-stu-id="a3423-320">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="starting-the-host"></a><span data-ttu-id="a3423-321">Starten des Hosts</span><span class="sxs-lookup"><span data-stu-id="a3423-321">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a3423-322">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a3423-322">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a3423-323">**Run**</span><span class="sxs-lookup"><span data-stu-id="a3423-323">**Run**</span></span>

<span data-ttu-id="a3423-324">Die `Run`-Methode startet die Web-App und blockiert den aufrufenden Thread, bis der Host heruntergefahren wird:</span><span class="sxs-lookup"><span data-stu-id="a3423-324">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="a3423-325">**Start**</span><span class="sxs-lookup"><span data-stu-id="a3423-325">**Start**</span></span>

<span data-ttu-id="a3423-326">Führen Sie den Host auf nicht blockierende Weise aus, indem Sie dessen `Start`-Methode aufrufen:</span><span class="sxs-lookup"><span data-stu-id="a3423-326">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="a3423-327">Wenn eine Liste mit URLs an die `Start`-Methode übergeben wird, lauscht diese den angegebenen URLs:</span><span class="sxs-lookup"><span data-stu-id="a3423-327">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

<span data-ttu-id="a3423-328">Die App kann einen neuen Host, der die vorab konfigurierten Standardwerte von `CreateDefaultBuilder` verwendet, mithilfe einer statischen Hilfsmethode initialisieren und starten.</span><span class="sxs-lookup"><span data-stu-id="a3423-328">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="a3423-329">Diese Methoden starten den Server ohne Konsolenausgabe und mit der [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown)-Methode, die auf eine Unterbrechung wartet (STRG+C/SIGINT oder SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="a3423-329">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="a3423-330">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="a3423-330">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="a3423-331">Beginnend mit einer `RequestDelegate`-Funktion:</span><span class="sxs-lookup"><span data-stu-id="a3423-331">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="a3423-332">Stellen Sie im Browser eine Anforderung an `http://localhost:5000`, um die Antwort „Hello World!“ zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="a3423-332">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="a3423-333">`WaitForShutdown` blockiert, bis eine Unterbrechung (STRG+C/SIGINT oder SIGTERM) ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="a3423-333">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="a3423-334">Die App zeigt die `Console.WriteLine`-Meldung an und wartet, bis diese durch Tastendruck beendet wird.</span><span class="sxs-lookup"><span data-stu-id="a3423-334">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="a3423-335">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="a3423-335">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="a3423-336">Beginnend mit einer URL und `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="a3423-336">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="a3423-337">Erzeugt das gleiche Ergebnis wie **Start(RequestDelegate app)**, mit dem Unterschied, dass die App auf `http://localhost:8080` reagiert.</span><span class="sxs-lookup"><span data-stu-id="a3423-337">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="a3423-338">**Start(Action<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="a3423-338">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="a3423-339">Verwenden Sie eine Instanz von `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)), um Routingmiddleware zu verwenden:</span><span class="sxs-lookup"><span data-stu-id="a3423-339">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

```csharp
using (var host = WebHost.Start(router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="a3423-340">Verwenden Sie folgende Browseranforderung mit dem Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a3423-340">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="a3423-341">Anforderung</span><span class="sxs-lookup"><span data-stu-id="a3423-341">Request</span></span>                                    | <span data-ttu-id="a3423-342">Antwort</span><span class="sxs-lookup"><span data-stu-id="a3423-342">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="a3423-343">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="a3423-343">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="a3423-344">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="a3423-344">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="a3423-345">Löst eine Ausnahme mit der Zeichenfolge „ooops!“ aus.</span><span class="sxs-lookup"><span data-stu-id="a3423-345">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="a3423-346">Löst eine Ausnahme mit der Zeichenfolge „Uh oh!“ aus.</span><span class="sxs-lookup"><span data-stu-id="a3423-346">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="a3423-347">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="a3423-347">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="a3423-348">Hello World!</span><span class="sxs-lookup"><span data-stu-id="a3423-348">Hello World!</span></span>                             |

<span data-ttu-id="a3423-349">`WaitForShutdown` blockiert, bis eine Unterbrechung (STRG+C/SIGINT oder SIGTERM) ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="a3423-349">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="a3423-350">Die App zeigt die `Console.WriteLine`-Meldung an und wartet, bis diese durch Tastendruck beendet wird.</span><span class="sxs-lookup"><span data-stu-id="a3423-350">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="a3423-351">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="a3423-351">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="a3423-352">Verwenden Sie eine URL und eine Instanz von `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="a3423-352">Use a URL and an instance of `IRouteBuilder`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="a3423-353">Erzeugt das gleiche Ergebnis wie **Start(Action<IRouteBuilder> routeBuilder)**, mit dem Unterschied, dass die App auf `http://localhost:8080` reagiert.</span><span class="sxs-lookup"><span data-stu-id="a3423-353">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="a3423-354">**StartWith(Action<IApplicationBuilder> app)**</span><span class="sxs-lookup"><span data-stu-id="a3423-354">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="a3423-355">Stellen Sie einen Delegaten bereit, um eine `IApplicationBuilder`-Schnittstelle zu konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="a3423-355">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

```csharp
using (var host = WebHost.StartWith(app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="a3423-356">Stellen Sie im Browser eine Anforderung an `http://localhost:5000`, um die Antwort „Hello World!“ zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="a3423-356">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="a3423-357">`WaitForShutdown` blockiert, bis eine Unterbrechung (STRG+C/SIGINT oder SIGTERM) ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="a3423-357">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="a3423-358">Die App zeigt die `Console.WriteLine`-Meldung an und wartet, bis diese durch Tastendruck beendet wird.</span><span class="sxs-lookup"><span data-stu-id="a3423-358">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="a3423-359">**StartWith(string url, Action<IApplicationBuilder> app)**</span><span class="sxs-lookup"><span data-stu-id="a3423-359">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="a3423-360">Stellen Sie eine URL und einen Delegaten bereit, um eine `IApplicationBuilder`-Schnittstelle zu konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="a3423-360">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

```csharp
using (var host = WebHost.StartWith("http://localhost:8080", app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="a3423-361">Erzeugt das gleiche Ergebnis wie **StartWith(Action<IApplicationBuilder> app)**, mit dem Unterschied, dass die App auf `http://localhost:8080` reagiert.</span><span class="sxs-lookup"><span data-stu-id="a3423-361">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a3423-362">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a3423-362">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a3423-363">**Run**</span><span class="sxs-lookup"><span data-stu-id="a3423-363">**Run**</span></span>

<span data-ttu-id="a3423-364">Die `Run`-Methode startet die Web-App und blockiert den aufrufenden Thread, bis der Host heruntergefahren wird:</span><span class="sxs-lookup"><span data-stu-id="a3423-364">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="a3423-365">**Start**</span><span class="sxs-lookup"><span data-stu-id="a3423-365">**Start**</span></span>

<span data-ttu-id="a3423-366">Führen Sie den Host auf nicht blockierende Weise aus, indem Sie dessen `Start`-Methode aufrufen:</span><span class="sxs-lookup"><span data-stu-id="a3423-366">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="a3423-367">Wenn eine Liste mit URLs an die `Start`-Methode übergeben wird, lauscht diese den angegebenen URLs:</span><span class="sxs-lookup"><span data-stu-id="a3423-367">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>


```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

---

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="a3423-368">IHostingEnvironment-Schnittstelle</span><span class="sxs-lookup"><span data-stu-id="a3423-368">IHostingEnvironment interface</span></span>

<span data-ttu-id="a3423-369">Die [IHostingEnvironment-Schnittstelle](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) stellt Informationen über die Webhostingumgebung der App bereit.</span><span class="sxs-lookup"><span data-stu-id="a3423-369">The [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="a3423-370">Verwenden Sie [Constructor Injection](xref:fundamentals/dependency-injection) zum Abrufen der `IHostingEnvironment`-Schnittstelle, um deren Eigenschaften und Erweiterungsmethoden zu verwenden:</span><span class="sxs-lookup"><span data-stu-id="a3423-370">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

```csharp
public class CustomFileReader
{
    private readonly IHostingEnvironment _env;

    public CustomFileReader(IHostingEnvironment env)
    {
        _env = env;
    }

    public string ReadFile(string filePath)
    {
        var fileProvider = _env.WebRootFileProvider;
        // Process the file here
    }
}
```

<span data-ttu-id="a3423-371">Ein [konventionsbasierter Ansatz](xref:fundamentals/environments#startup-conventions) kann verwendet werden, um die App beim Start je nach Umgebung zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="a3423-371">A [convention-based approach](xref:fundamentals/environments#startup-conventions) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="a3423-372">Fügen Sie alternativ die `IHostingEnvironment`-Schnittstelle in den `Startup`-Konstruktor für die Verwendung in `ConfigureServices` ein:</span><span class="sxs-lookup"><span data-stu-id="a3423-372">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

```csharp
public class Startup
{
    public Startup(IHostingEnvironment env)
    {
        HostingEnvironment = env;
    }

    public IHostingEnvironment HostingEnvironment { get; }

    public void ConfigureServices(IServiceCollection services)
    {
        if (HostingEnvironment.IsDevelopment())
        {
            // Development configuration
        }
        else
        {
            // Staging/Production configuration
        }

        var contentRootPath = HostingEnvironment.ContentRootPath;
    }
}
```

> [!NOTE]
> <span data-ttu-id="a3423-373">Zusätzlich zur `IsDevelopment`-Erweiterungsmethode stellt die `IHostingEnvironment`-Schnittstelle die `IsStaging`-, `IsProduction`- und `IsEnvironment(string environmentName)`-Methoden bereit.</span><span class="sxs-lookup"><span data-stu-id="a3423-373">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="a3423-374">Weitere Informationen finden Sie unter [Arbeiten mit mehreren Umgebungen](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="a3423-374">See [Working with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="a3423-375">Der `IHostingEnvironment`-Dienst kann ebenfalls direkt in die `Configure`-Methode eingefügt werden, um die Verarbeitungspipeline einzurichten:</span><span class="sxs-lookup"><span data-stu-id="a3423-375">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // In Development, use the developer exception page
        app.UseDeveloperExceptionPage();
    }
    else
    {
        // In Staging/Production, route exceptions to /error
        app.UseExceptionHandler("/error");
    }

    var contentRootPath = env.ContentRootPath;
}
```

<span data-ttu-id="a3423-376">`IHostingEnvironment` kann in die `Invoke`-Methode eingefügt werden, wenn eine benutzerdefinierte [Middleware](xref:fundamentals/middleware/index#writing-middleware) erstellt wird:</span><span class="sxs-lookup"><span data-stu-id="a3423-376">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#writing-middleware):</span></span>

```csharp
public async Task Invoke(HttpContext context, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Configure middleware for Development
    }
    else
    {
        // Configure middleware for Staging/Production
    }

    var contentRootPath = env.ContentRootPath;
}
```

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="a3423-377">IApplicationLifetime-Schnittstelle</span><span class="sxs-lookup"><span data-stu-id="a3423-377">IApplicationLifetime interface</span></span>

<span data-ttu-id="a3423-378">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) ermöglicht Aktivitäten nach dem Starten und beim Herunterfahren.</span><span class="sxs-lookup"><span data-stu-id="a3423-378">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="a3423-379">Bei drei der Eigenschaften der Schnittstelle handelt es sich um Abbruchtokens, die zum Registrieren von `Action`-Methoden verwendet werden, durch die Ereignisse beim Starten und Herunterfahren definiert werden.</span><span class="sxs-lookup"><span data-stu-id="a3423-379">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span> <span data-ttu-id="a3423-380">Es gibt ebenfalls eine `StopApplication`-Methode.</span><span class="sxs-lookup"><span data-stu-id="a3423-380">There's also a `StopApplication` method.</span></span>

| <span data-ttu-id="a3423-381">Abbruchtoken</span><span class="sxs-lookup"><span data-stu-id="a3423-381">Cancellation Token</span></span>    | <span data-ttu-id="a3423-382">Wird ausgelöst, wenn&#8230;</span><span class="sxs-lookup"><span data-stu-id="a3423-382">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="a3423-383">Der Host vollständig gestartet wurde.</span><span class="sxs-lookup"><span data-stu-id="a3423-383">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="a3423-384">Der Host ein ordnungsgemäßes Herunterfahren ausführt.</span><span class="sxs-lookup"><span data-stu-id="a3423-384">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="a3423-385">Anforderungen möglicherweise noch verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="a3423-385">Requests may still be processing.</span></span> <span data-ttu-id="a3423-386">Das Herunterfahren wird blockiert, bis das Ereignis abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="a3423-386">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="a3423-387">Der Host ein ordnungsgemäßes Herunterfahren abschließt.</span><span class="sxs-lookup"><span data-stu-id="a3423-387">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="a3423-388">Alle Anforderungen sollten verarbeitet worden sein.</span><span class="sxs-lookup"><span data-stu-id="a3423-388">All requests should be processed.</span></span> <span data-ttu-id="a3423-389">Das Herunterfahren wird blockiert, bis das Ereignis abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="a3423-389">Shutdown blocks until this event completes.</span></span> |

| <span data-ttu-id="a3423-390">Methode</span><span class="sxs-lookup"><span data-stu-id="a3423-390">Method</span></span>            | <span data-ttu-id="a3423-391">Aktion</span><span class="sxs-lookup"><span data-stu-id="a3423-391">Action</span></span>                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | <span data-ttu-id="a3423-392">Fordert das Beenden der aktuellen Anwendung an.</span><span class="sxs-lookup"><span data-stu-id="a3423-392">Requests termination of the current application.</span></span> |

```csharp
public class Startup 
{
    public void Configure(IApplicationBuilder app, IApplicationLifetime appLifetime) 
    {
        appLifetime.ApplicationStarted.Register(OnStarted);
        appLifetime.ApplicationStopping.Register(OnStopping);
        appLifetime.ApplicationStopped.Register(OnStopped);

        Console.CancelKeyPress += (sender, eventArgs) =>
        {
            appLifetime.StopApplication();
            // Don't terminate the process immediately, wait for the Main thread to exit gracefully.
            eventArgs.Cancel = true;
        };
    }

    private void OnStarted()
    {
        // Perform post-startup activities here
    }

    private void OnStopping()
    {
        // Perform on-stopping activities here
    }

    private void OnStopped()
    {
        // Perform post-stopped activities here
    }
}
```

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="a3423-393">Problembehandlung bei System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="a3423-393">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="a3423-394">**Gilt nur für ASP.NET Core 2.0**</span><span class="sxs-lookup"><span data-stu-id="a3423-394">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="a3423-395">Ein Host kann erstellt werden, indem `IStartup` direkt in den Dependency Injection-Container eingefügt wird, anstatt `UseStartup` oder `Configure` aufzurufen:</span><span class="sxs-lookup"><span data-stu-id="a3423-395">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="a3423-396">Wenn der Host auf diese Weise erstellt wird, kann folgender Fehler auftreten:</span><span class="sxs-lookup"><span data-stu-id="a3423-396">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="a3423-397">Dieser Fehler tritt auf, da [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (die aktuelle Assembly) erforderlich ist, um nach `HostingStartupAttributes` zu suchen.</span><span class="sxs-lookup"><span data-stu-id="a3423-397">This occurs because the [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="a3423-398">Wenn die App `IStartup` manuell in den Dependency Injection-Container eingefügt wird, fügen Sie folgenden Aufruf mit dem angegebenen Assemblynamen zur `WebHostBuilder`-Klasse hinzu:</span><span class="sxs-lookup"><span data-stu-id="a3423-398">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="a3423-399">Fügen Sie alternativ eine `Configure`-Dummymethode zur `WebHostBuilder`-Klasse hinzu, wodurch `applicationName` (`ApplicationKey`) automatisch festgelegt wird:</span><span class="sxs-lookup"><span data-stu-id="a3423-399">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="a3423-400">**Hinweis:** Dies ist nur erforderlich, wenn Sie ASP.NET Core 2.0 verwenden und wenn die App nicht `UseStartup` oder `Configure` aufruft.</span><span class="sxs-lookup"><span data-stu-id="a3423-400">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="a3423-401">Weitere Informationen finden Sie unter [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment) (Ankündigungen: „Microsoft.Extensions.PlatformAbstractions“ wurde entfernt (Kommentar))](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) und im Beispiel für [StartupInjection](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="a3423-401">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a3423-402">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="a3423-402">Additional resources</span></span>

* [<span data-ttu-id="a3423-403">Hosten unter Windows mit IIS</span><span class="sxs-lookup"><span data-stu-id="a3423-403">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="a3423-404">Hosten unter Linux mit Nginx</span><span class="sxs-lookup"><span data-stu-id="a3423-404">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
* [<span data-ttu-id="a3423-405">Hosten unter Linux mit Apache</span><span class="sxs-lookup"><span data-stu-id="a3423-405">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
* [<span data-ttu-id="a3423-406">Hosten in einem Windows-Dienst</span><span class="sxs-lookup"><span data-stu-id="a3423-406">Host in a Windows Service</span></span>](xref:host-and-deploy/windows-service)
