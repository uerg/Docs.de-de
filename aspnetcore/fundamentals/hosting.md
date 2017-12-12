---
title: Hosten in ASP.NET Core
author: guardrex
description: "Informationen Sie zu den Webhost in ASP.NET Core, die für die app starten und die Verwaltung zuständig ist."
keywords: ASP.NET Core, web Host IWebHost, WebHostBuilder, IHostingEnvironment, IApplicationLifetime
ms.author: riande
manager: wpickett
ms.date: 09/21/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.openlocfilehash: 7deccf135ddd21729206ebed58ddc8aca52c1deb
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/29/2017
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="1b438-104">Hosten in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1b438-104">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="1b438-105">Von [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1b438-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="1b438-106">ASP.NET Core-Apps konfigurieren und starten einen *Host*, der für das Starten der App und das Verwalten deren Lebensdauer verantwortlich ist.</span><span class="sxs-lookup"><span data-stu-id="1b438-106">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="1b438-107">Mindestens wird der Host einem Server und einem Anforderungsverarbeitungspipeline konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="1b438-107">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="1b438-108">Einrichten eines Hosts</span><span class="sxs-lookup"><span data-stu-id="1b438-108">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1b438-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1b438-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="1b438-110">Erstellen Sie einen Host mit einer Instanz von [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="1b438-110">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="1b438-111">Dies erfolgt in der Regel am Einstiegspunkt für Ihre app, die `Main` Methode.</span><span class="sxs-lookup"><span data-stu-id="1b438-111">This is typically performed in your app's entry point, the `Main` method.</span></span> <span data-ttu-id="1b438-112">In den Vorlagen für Projekte `Main` befindet sich im *"Program.cs"*.</span><span class="sxs-lookup"><span data-stu-id="1b438-112">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="1b438-113">Eine typische *"Program.cs"* Aufrufe [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) zum Einrichten eines Hosts zu starten:</span><span class="sxs-lookup"><span data-stu-id="1b438-113">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="1b438-114">`CreateDefaultBuilder`führt die folgenden Aufgaben:</span><span class="sxs-lookup"><span data-stu-id="1b438-114">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="1b438-115">Konfiguriert die [Kestrel](servers/kestrel.md) wie der Webserver.</span><span class="sxs-lookup"><span data-stu-id="1b438-115">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="1b438-116">Die Standardoptionen Kestrel finden Sie unter [der Kestrel Optionen im Abschnitt Kestrel webserverimplementierung in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="1b438-116">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="1b438-117">Legt den Content-Stamm auf [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="1b438-117">Sets the content root to [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="1b438-118">Lädt die optionale Konfiguration aus:</span><span class="sxs-lookup"><span data-stu-id="1b438-118">Loads optional configuration from:</span></span>
  * <span data-ttu-id="1b438-119">*AppSettings.JSON*.</span><span class="sxs-lookup"><span data-stu-id="1b438-119">*appsettings.json*.</span></span>
  * <span data-ttu-id="1b438-120">*"appSettings". {Umgebung} JSON*.</span><span class="sxs-lookup"><span data-stu-id="1b438-120">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="1b438-121">[Vertrauliche Benutzerdaten](xref:security/app-secrets) Wenn die app ausgeführt wird, der `Development` Umgebung.</span><span class="sxs-lookup"><span data-stu-id="1b438-121">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="1b438-122">Umgebungsvariablen.</span><span class="sxs-lookup"><span data-stu-id="1b438-122">Environment variables.</span></span>
  * <span data-ttu-id="1b438-123">Befehlszeilenargumente.</span><span class="sxs-lookup"><span data-stu-id="1b438-123">Command-line arguments.</span></span>
* <span data-ttu-id="1b438-124">Konfiguriert [Protokollierung](xref:fundamentals/logging/index) für die Konsole und Debug-Ausgabe mit [Protokoll filtern](xref:fundamentals/logging/index#log-filtering) Regeln in einem Konfigurationsabschnitt Protokollierung ein *appsettings.json* oder *"appSettings". {Umgebung} JSON* Datei.</span><span class="sxs-lookup"><span data-stu-id="1b438-124">Configures [logging](xref:fundamentals/logging/index) for console and debug output with [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="1b438-125">Wenn hinter dem IIS ausgeführt wird, ermöglicht [IIS Integration](xref:publishing/iis) durch Konfigurieren der Basispfad und Port der Server überwacht werden soll bei Verwendung der [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="1b438-125">When running behind IIS, enables [IIS integration](xref:publishing/iis) by configuring the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="1b438-126">Das Modul erstellt einen Reverse-Proxy zwischen IIS und Kestrel.</span><span class="sxs-lookup"><span data-stu-id="1b438-126">The module creates a reverse-proxy between IIS and Kestrel.</span></span> <span data-ttu-id="1b438-127">Außerdem konfiguriert die app, [Erfassen von Startfehlern](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="1b438-127">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="1b438-128">Die Standardoptionen von IIS finden Sie unter [der IIS-Optionen im Abschnitt Host ASP.NET Core unter Windows mit IIS](xref:publishing/iis#iis-options).</span><span class="sxs-lookup"><span data-stu-id="1b438-128">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:publishing/iis#iis-options).</span></span>

<span data-ttu-id="1b438-129">Die *Content Stamm* bestimmt, an denen der Host für Inhaltsdateien, z. B. MVC-Ansicht-Dateien sucht.</span><span class="sxs-lookup"><span data-stu-id="1b438-129">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="1b438-130">Der Standard-Content-Stamm ist [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="1b438-130">The default content root is [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span> <span data-ttu-id="1b438-131">Dies führt zu Stammordner für das Webprojekt als Inhalt Stamm verwenden, wenn die app aus dem Stammordner gestartet wird (z. B. durch Aufruf von [Dotnet ausführen](/dotnet/core/tools/dotnet-run) aus dem Projektordner).</span><span class="sxs-lookup"><span data-stu-id="1b438-131">This results in using the web project's root folder as the content root when the app is started from the root folder (for example, calling [dotnet run](/dotnet/core/tools/dotnet-run) from the project folder).</span></span> <span data-ttu-id="1b438-132">Dies ist die Standardeinstellung verwendet [Visual Studio](https://www.visualstudio.com/) und [Dotnet neue Vorlagen](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="1b438-132">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="1b438-133">Finden Sie unter [Konfiguration in ASP.NET Core](xref:fundamentals/configuration/index) für Weitere Informationen zu app-Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="1b438-133">See [Configuration in ASP.NET Core](xref:fundamentals/configuration/index) for more information on app configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="1b438-134">Als Alternative zur Verwendung der statischen `CreateDefaultBuilder` -Methode, erstellen einen Host aus [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) ist ein unterstützten Ansatz mit ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="1b438-134">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="1b438-135">Finden Sie unter der Registerkarte "1.x ASP.NET Core" für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="1b438-135">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1b438-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1b438-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="1b438-137">Erstellen Sie einen Host mit einer Instanz von [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="1b438-137">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="1b438-138">Dies erfolgt in der Regel am Einstiegspunkt für Ihre app, die `Main` Methode.</span><span class="sxs-lookup"><span data-stu-id="1b438-138">This is typically performed in your app's entry point, the `Main` method.</span></span> <span data-ttu-id="1b438-139">In den Vorlagen für Projekte `Main` befindet sich im *"Program.cs"*.</span><span class="sxs-lookup"><span data-stu-id="1b438-139">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="1b438-140">Die folgenden *"Program.cs"* veranschaulicht, wie `WebHostBuilder` auf den Host zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="1b438-140">The following *Program.cs* demonstrates how to use `WebHostBuilder` to build the host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="1b438-141">`WebHostBuilder`erfordert eine [Server, die Iserver-c# implementiert](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="1b438-141">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="1b438-142">Die integrierten Server sind [Kestrel](servers/kestrel.md) und [HTTP.sys](servers/httpsys.md) (vor der Veröffentlichung von ASP.NET Core 2.0, HTTP.sys hieß [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="1b438-142">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="1b438-143">In diesem Beispiel wird die [UseKestrel Erweiterungsmethode](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) gibt den Kestrel-Server.</span><span class="sxs-lookup"><span data-stu-id="1b438-143">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="1b438-144">Die *Content Stamm* bestimmt, an denen der Host für Inhaltsdateien, z. B. MVC-Ansicht-Dateien sucht.</span><span class="sxs-lookup"><span data-stu-id="1b438-144">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="1b438-145">Des Standardstammverzeichnisses Content bereitgestellten `UseContentRoot` ist [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="1b438-145">The default content root supplied to `UseContentRoot` is [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="1b438-146">Dies führt zu Stammordner für das Webprojekt als Inhalt Stamm verwenden, wenn die app aus dem Stammordner gestartet wird (z. B. durch Aufruf von [Dotnet ausführen](/dotnet/core/tools/dotnet-run) aus dem Projektordner).</span><span class="sxs-lookup"><span data-stu-id="1b438-146">This results in using the web project's root folder as the content root when the app is started from the root folder (for example, calling [dotnet run](/dotnet/core/tools/dotnet-run) from the project folder).</span></span> <span data-ttu-id="1b438-147">Dies ist die Standardeinstellung verwendet [Visual Studio](https://www.visualstudio.com/) und [Dotnet neue Vorlagen](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="1b438-147">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="1b438-148">Rufen Sie zum Verwenden von IIS als Reverseproxy [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) im Rahmen der Erstellung des Hosts.</span><span class="sxs-lookup"><span data-stu-id="1b438-148">To use IIS as a reverse proxy, call [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="1b438-149">`UseIISIntegration`Konfigurieren Sie keine *Server*, z. B. [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) verfügt.</span><span class="sxs-lookup"><span data-stu-id="1b438-149">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="1b438-150">`UseIISIntegration`konfiguriert den Basispfad und der Port, auf der Server überwacht werden soll bei Verwendung der [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) einen Reverse-Proxy zwischen Kestrel und IIS zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="1b438-150">`UseIISIntegration` configures the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse-proxy between Kestrel and IIS.</span></span> <span data-ttu-id="1b438-151">Um IIS mit ASP.NET Core verwenden zu können, müssen Sie beide angeben `UseKestrel` und `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="1b438-151">To use IIS with ASP.NET Core, you must specify both `UseKestrel` and `UseIISIntegration`.</span></span> <span data-ttu-id="1b438-152">`UseIISIntegration`nur aktiviert, wenn hinter der IIS- oder IIS Express ausführen.</span><span class="sxs-lookup"><span data-stu-id="1b438-152">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="1b438-153">Weitere Informationen finden Sie unter [Einführung in ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) und [ASP.NET Core-Modul Konfigurationsverweis](xref:hosting/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="1b438-153">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:hosting/aspnet-core-module).</span></span>

<span data-ttu-id="1b438-154">Eine minimale Implementierung, die einen Host (und ASP.NET Core-app) konfiguriert, enthält einen Server und die Konfiguration der Anforderungspipeline die app angeben:</span><span class="sxs-lookup"><span data-stu-id="1b438-154">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="1b438-155">Beim Einrichten von eines Hosts bieten [konfigurieren](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) und [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) Methoden.</span><span class="sxs-lookup"><span data-stu-id="1b438-155">When setting up a host, you can provide [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods.</span></span> <span data-ttu-id="1b438-156">Bei Angabe einer `Startup` -Klasse, müssen sie definieren eine `Configure` Methode.</span><span class="sxs-lookup"><span data-stu-id="1b438-156">If you specify a `Startup` class, it must define a `Configure` method.</span></span> <span data-ttu-id="1b438-157">Weitere Informationen finden Sie unter [Start der Anwendung in ASP.NET Core](startup.md).</span><span class="sxs-lookup"><span data-stu-id="1b438-157">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="1b438-158">Mehrere Aufrufe `ConfigureServices` untereinander anfügen.</span><span class="sxs-lookup"><span data-stu-id="1b438-158">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="1b438-159">Mehrere Aufrufe `Configure` oder `UseStartup` auf die `WebHostBuilder` ersetzen die vorherige Einstellungen.</span><span class="sxs-lookup"><span data-stu-id="1b438-159">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="1b438-160">Host-Konfigurationswerte</span><span class="sxs-lookup"><span data-stu-id="1b438-160">Host configuration values</span></span>

<span data-ttu-id="1b438-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) stellt Methoden zum Festlegen der Großteil der verfügbaren Konfigurationswerte, damit des Hosts die direkt mit auch festgelegt werden können [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) und den zugehörigen Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="1b438-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) provides methods for setting most of the available configuration values for the host, which can also be set directly with [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="1b438-162">Beim Festlegen eines Werts mit `UseSetting`, der Wert wird als Zeichenfolge (in Anführungszeichen) unabhängig vom Typ festgelegt.</span><span class="sxs-lookup"><span data-stu-id="1b438-162">When setting a value with `UseSetting`, the value is set as a string (in quotes) regardless of the type.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="1b438-163">Erfassen von Startfehlern</span><span class="sxs-lookup"><span data-stu-id="1b438-163">Capture Startup Errors</span></span>

<span data-ttu-id="1b438-164">Diese Einstellung steuert die Erfassung von Startfehlern.</span><span class="sxs-lookup"><span data-stu-id="1b438-164">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="1b438-165">**Schlüssel**: CaptureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="1b438-165">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="1b438-166">**Typ**: *Bool* (`true` oder `1`)</span><span class="sxs-lookup"><span data-stu-id="1b438-166">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="1b438-167">**Standardmäßige**: standardmäßig `false` , wenn die app mit Kestrel hinter dem IIS ausgeführt wird, in dem die Standardeinstellung ist `true`.</span><span class="sxs-lookup"><span data-stu-id="1b438-167">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="1b438-168">**Legen Sie mithilfe von**:`CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="1b438-168">**Set using**: `CaptureStartupErrors`</span></span>

<span data-ttu-id="1b438-169">Wenn `false`, Fehler während des Starts Ergebnis auf dem Host beendet.</span><span class="sxs-lookup"><span data-stu-id="1b438-169">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="1b438-170">Wenn `true`, der Host erfasst Ausnahmen während des Starts und versucht, den Server zu starten.</span><span class="sxs-lookup"><span data-stu-id="1b438-170">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1b438-171">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1b438-171">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1b438-172">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1b438-172">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="1b438-173">Inhalts-Stamm</span><span class="sxs-lookup"><span data-stu-id="1b438-173">Content Root</span></span>

<span data-ttu-id="1b438-174">Diese Einstellung bestimmt, in dem ASP.NET Core beginnt die Suche nach Inhaltsdateien gespeichert, z. B. MVC-Ansichten.</span><span class="sxs-lookup"><span data-stu-id="1b438-174">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="1b438-175">**Schlüssel**: ContentRoot</span><span class="sxs-lookup"><span data-stu-id="1b438-175">**Key**: contentRoot</span></span>  
<span data-ttu-id="1b438-176">**Typ**: *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="1b438-176">**Type**: *string*</span></span>  
<span data-ttu-id="1b438-177">**Standard**: standardmäßig auf den Ordner, in dem die app-Assembly gespeichert ist.</span><span class="sxs-lookup"><span data-stu-id="1b438-177">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="1b438-178">**Legen Sie mithilfe von**:`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="1b438-178">**Set using**: `UseContentRoot`</span></span>

<span data-ttu-id="1b438-179">Die Inhalte-Stamm dient außerdem als den Basispfad für den [Stammweb-Einstellung](#web-root).</span><span class="sxs-lookup"><span data-stu-id="1b438-179">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="1b438-180">Wenn der Pfad nicht vorhanden ist, kann der Host nicht gestartet.</span><span class="sxs-lookup"><span data-stu-id="1b438-180">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1b438-181">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1b438-181">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1b438-182">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1b438-182">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="1b438-183">Ausführliche Fehler</span><span class="sxs-lookup"><span data-stu-id="1b438-183">Detailed Errors</span></span>

<span data-ttu-id="1b438-184">Bestimmt, ob ausführliche Fehler erfasst werden sollen.</span><span class="sxs-lookup"><span data-stu-id="1b438-184">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="1b438-185">**Schlüssel**: DetailedErrors</span><span class="sxs-lookup"><span data-stu-id="1b438-185">**Key**: detailedErrors</span></span>  
<span data-ttu-id="1b438-186">**Typ**: *Bool* (`true` oder `1`)</span><span class="sxs-lookup"><span data-stu-id="1b438-186">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="1b438-187">**Standard**: "false"</span><span class="sxs-lookup"><span data-stu-id="1b438-187">**Default**: false</span></span>  
<span data-ttu-id="1b438-188">**Legen Sie mithilfe von**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="1b438-188">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="1b438-189">Wenn aktiviert (oder wenn die <a href="#environment">Umgebung</a> auf festgelegt ist `Development`), die app erfasst ausführliche Ausnahmen.</span><span class="sxs-lookup"><span data-stu-id="1b438-189">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1b438-190">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1b438-190">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1b438-191">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1b438-191">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="1b438-192">Umgebung</span><span class="sxs-lookup"><span data-stu-id="1b438-192">Environment</span></span>

<span data-ttu-id="1b438-193">Legt fest, die app-Umgebung.</span><span class="sxs-lookup"><span data-stu-id="1b438-193">Sets the app's environment.</span></span>

<span data-ttu-id="1b438-194">**Schlüssel**: Umgebung</span><span class="sxs-lookup"><span data-stu-id="1b438-194">**Key**: environment</span></span>  
<span data-ttu-id="1b438-195">**Typ**: *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="1b438-195">**Type**: *string*</span></span>  
<span data-ttu-id="1b438-196">**Standard**: Produktion</span><span class="sxs-lookup"><span data-stu-id="1b438-196">**Default**: Production</span></span>  
<span data-ttu-id="1b438-197">**Legen Sie mithilfe von**:`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="1b438-197">**Set using**: `UseEnvironment`</span></span>

<span data-ttu-id="1b438-198">Sie können festlegen, die *Umgebung* auf einen beliebigen Wert.</span><span class="sxs-lookup"><span data-stu-id="1b438-198">You can set the *Environment* to any value.</span></span> <span data-ttu-id="1b438-199">Framework definierte Werte sind `Development`, `Staging`, und `Production`.</span><span class="sxs-lookup"><span data-stu-id="1b438-199">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="1b438-200">Werte sind keine Groß-/Kleinschreibung beachtet.</span><span class="sxs-lookup"><span data-stu-id="1b438-200">Values aren't case sensitive.</span></span> <span data-ttu-id="1b438-201">Wird standardmäßig die *Umgebung* auslesen ist die `ASPNETCORE_ENVIRONMENT` -Umgebungsvariablen angegeben.</span><span class="sxs-lookup"><span data-stu-id="1b438-201">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="1b438-202">Bei Verwendung [Visual Studio](https://www.visualstudio.com/), Umgebungsvariablen festgelegt werden können, der *launchSettings.json* Datei.</span><span class="sxs-lookup"><span data-stu-id="1b438-202">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="1b438-203">Weitere Informationen finden Sie unter [Working with Multiple Environments (Verwenden von mehreren Umgebungen)](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="1b438-203">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1b438-204">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1b438-204">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1b438-205">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1b438-205">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="1b438-206">Hosten Startassemblys</span><span class="sxs-lookup"><span data-stu-id="1b438-206">Hosting Startup Assemblies</span></span>

<span data-ttu-id="1b438-207">Legt die app hosten Startassemblys fest.</span><span class="sxs-lookup"><span data-stu-id="1b438-207">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="1b438-208">**Schlüssel**: HostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="1b438-208">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="1b438-209">**Typ**: *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="1b438-209">**Type**: *string*</span></span>  
<span data-ttu-id="1b438-210">**Standard**: leere Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="1b438-210">**Default**: Empty string</span></span>  
<span data-ttu-id="1b438-211">**Legen Sie mithilfe von**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="1b438-211">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="1b438-212">Eine durch Semikolons getrennte Zeichenfolge hosten Startassemblys beim Start geladen werden soll.</span><span class="sxs-lookup"><span data-stu-id="1b438-212">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="1b438-213">Dieses Feature ist neu in ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="1b438-213">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="1b438-214">Obwohl die Konfiguration auf eine leere Zeichenfolge wird als Standardwert, enthalten hosting Autostart-Assemblys immer der app-Assembly.</span><span class="sxs-lookup"><span data-stu-id="1b438-214">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="1b438-215">Wenn Sie hosting Startassemblys angeben, werden diese hinzugefügt auf die app-Assembly für das Laden, wenn die app während des Starts der gemeinsamen Dienste erstellt.</span><span class="sxs-lookup"><span data-stu-id="1b438-215">When you provide hosting startup assemblies, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1b438-216">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1b438-216">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1b438-217">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1b438-217">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="1b438-218">Diese Funktion steht in ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="1b438-218">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="1b438-219">Bevorzugen Sie Hosten von URLs</span><span class="sxs-lookup"><span data-stu-id="1b438-219">Prefer Hosting URLs</span></span>

<span data-ttu-id="1b438-220">Gibt an, ob der Host werden, auf die URLs abgehört soll mit konfiguriert die `WebHostBuilder` nicht die dafür konfigurierten mit der `IServer` Implementierung.</span><span class="sxs-lookup"><span data-stu-id="1b438-220">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="1b438-221">**Schlüssel**: PreferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="1b438-221">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="1b438-222">**Typ**: *Bool* (`true` oder `1`)</span><span class="sxs-lookup"><span data-stu-id="1b438-222">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="1b438-223">**Standard**: "true"</span><span class="sxs-lookup"><span data-stu-id="1b438-223">**Default**: true</span></span>  
<span data-ttu-id="1b438-224">**Legen Sie mithilfe von**:`PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="1b438-224">**Set using**: `PreferHostingUrls`</span></span>

<span data-ttu-id="1b438-225">Dieses Feature ist neu in ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="1b438-225">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1b438-226">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1b438-226">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1b438-227">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1b438-227">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="1b438-228">Diese Funktion steht in ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="1b438-228">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="1b438-229">Zu verhindern, dass Hosting starten</span><span class="sxs-lookup"><span data-stu-id="1b438-229">Prevent Hosting Startup</span></span>

<span data-ttu-id="1b438-230">Verhindert, dass das automatische Laden von hosting Startassemblys, einschließlich der app-Assembly.</span><span class="sxs-lookup"><span data-stu-id="1b438-230">Prevents the automatic loading of hosting startup assemblies, including the app's assembly.</span></span>

<span data-ttu-id="1b438-231">**Schlüssel**: PreventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="1b438-231">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="1b438-232">**Typ**: *Bool* (`true` oder `1`)</span><span class="sxs-lookup"><span data-stu-id="1b438-232">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="1b438-233">**Standard**: "false"</span><span class="sxs-lookup"><span data-stu-id="1b438-233">**Default**: false</span></span>  
<span data-ttu-id="1b438-234">**Legen Sie mithilfe von**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="1b438-234">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="1b438-235">Dieses Feature ist neu in ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="1b438-235">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1b438-236">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1b438-236">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1b438-237">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1b438-237">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="1b438-238">Diese Funktion steht in ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="1b438-238">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="1b438-239">Server-URLs</span><span class="sxs-lookup"><span data-stu-id="1b438-239">Server URLs</span></span>

<span data-ttu-id="1b438-240">Gibt die IP-Adressen oder Adressen mit Ports und Protokolle, die der Server auf Anforderungen Lauschen soll.</span><span class="sxs-lookup"><span data-stu-id="1b438-240">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="1b438-241">**Schlüssel**: Urls</span><span class="sxs-lookup"><span data-stu-id="1b438-241">**Key**: urls</span></span>  
<span data-ttu-id="1b438-242">**Typ**: *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="1b438-242">**Type**: *string*</span></span>  
<span data-ttu-id="1b438-243">**Standard**: http://localhost: 5000</span><span class="sxs-lookup"><span data-stu-id="1b438-243">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="1b438-244">**Legen Sie mithilfe von**:`UseUrls`</span><span class="sxs-lookup"><span data-stu-id="1b438-244">**Set using**: `UseUrls`</span></span>

<span data-ttu-id="1b438-245">Legen Sie auf eine durch Semikolon getrennt (;) Liste von URL-Präfixen, die auf der Server reagieren soll.</span><span class="sxs-lookup"><span data-stu-id="1b438-245">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="1b438-246">Beispielsweise `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="1b438-246">For example, `http://localhost:123`.</span></span> <span data-ttu-id="1b438-247">Mit "\*", um anzugeben, dass der Server nach Anforderungen für alle IP-Adresse oder Hostname mit dem angegebenen Port und Protokoll abgehört werden soll (z. B. `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="1b438-247">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="1b438-248">Das Protokoll (`http://` oder `https://`) für jede URL eingeschlossen werden müssen.</span><span class="sxs-lookup"><span data-stu-id="1b438-248">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="1b438-249">Zu den unterstützten Bildformaten variieren zwischen Servern.</span><span class="sxs-lookup"><span data-stu-id="1b438-249">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1b438-250">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1b438-250">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="1b438-251">Kestrel verfügt über einen eigenen Endpunkt-Konfigurations-API.</span><span class="sxs-lookup"><span data-stu-id="1b438-251">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="1b438-252">Weitere Informationen finden Sie unter [Kestrel web server implementation in ASP.NET Core (Kestrel-Webserverimplementierung in ASP.NET Core)](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="1b438-252">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1b438-253">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1b438-253">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="1b438-254">Timeout beim Herunterfahren</span><span class="sxs-lookup"><span data-stu-id="1b438-254">Shutdown Timeout</span></span>

<span data-ttu-id="1b438-255">Gibt den Umfang der Wartezeit für den Webhost zum Herunterfahren.</span><span class="sxs-lookup"><span data-stu-id="1b438-255">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="1b438-256">**Schlüssel**: ShutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="1b438-256">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="1b438-257">**Typ**: *Int*</span><span class="sxs-lookup"><span data-stu-id="1b438-257">**Type**: *int*</span></span>  
<span data-ttu-id="1b438-258">**Standard**: 5</span><span class="sxs-lookup"><span data-stu-id="1b438-258">**Default**: 5</span></span>  
<span data-ttu-id="1b438-259">**Legen Sie mithilfe von**:`UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="1b438-259">**Set using**: `UseShutdownTimeout`</span></span>

<span data-ttu-id="1b438-260">Obwohl der Schlüssel akzeptiert ein *Int* mit `UseSetting` (z. B. `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), die `UseShutdownTimeout` Erweiterungsmethode akzeptiert eine `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="1b438-260">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the `UseShutdownTimeout` extension method takes a `TimeSpan`.</span></span> <span data-ttu-id="1b438-261">Dieses Feature ist neu in ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="1b438-261">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1b438-262">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1b438-262">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1b438-263">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1b438-263">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="1b438-264">Diese Funktion steht in ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="1b438-264">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="1b438-265">Startassembly</span><span class="sxs-lookup"><span data-stu-id="1b438-265">Startup Assembly</span></span>

<span data-ttu-id="1b438-266">Die Assembly zu suchende bestimmt die `Startup` Klasse.</span><span class="sxs-lookup"><span data-stu-id="1b438-266">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="1b438-267">**Schlüssel**: StartupAssembly</span><span class="sxs-lookup"><span data-stu-id="1b438-267">**Key**: startupAssembly</span></span>  
<span data-ttu-id="1b438-268">**Typ**: *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="1b438-268">**Type**: *string*</span></span>  
<span data-ttu-id="1b438-269">**Standard**: die app-Assembly</span><span class="sxs-lookup"><span data-stu-id="1b438-269">**Default**: The app's assembly</span></span>  
<span data-ttu-id="1b438-270">**Legen Sie mithilfe von**:`UseStartup`</span><span class="sxs-lookup"><span data-stu-id="1b438-270">**Set using**: `UseStartup`</span></span>

<span data-ttu-id="1b438-271">Sie können auf die Assembly anhand des Namens verweisen (`string`) oder (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="1b438-271">You can reference the assembly by name (`string`) or type (`TStartup`).</span></span> <span data-ttu-id="1b438-272">Wenn mehrere `UseStartup` Methoden aufgerufen werden, das letzte Lesezeichen hat Vorrang vor.</span><span class="sxs-lookup"><span data-stu-id="1b438-272">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1b438-273">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1b438-273">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1b438-274">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1b438-274">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

### <a name="web-root"></a><span data-ttu-id="1b438-275">Webstamm</span><span class="sxs-lookup"><span data-stu-id="1b438-275">Web Root</span></span>

<span data-ttu-id="1b438-276">Legt den relativen Pfad auf die app statische Objekte fest.</span><span class="sxs-lookup"><span data-stu-id="1b438-276">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="1b438-277">**Schlüssel**: Webroot-Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="1b438-277">**Key**: webroot</span></span>  
<span data-ttu-id="1b438-278">**Typ**: *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="1b438-278">**Type**: *string*</span></span>  
<span data-ttu-id="1b438-279">**Standardmäßige**: Wenn nicht angegeben, wird der Standardwert ist ""(Content Root)/wwwroot "", wenn der Pfad vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="1b438-279">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="1b438-280">Wenn der Pfad nicht vorhanden ist, wird ein ohne-Op-dateisystemanbieter verwendet.</span><span class="sxs-lookup"><span data-stu-id="1b438-280">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="1b438-281">**Legen Sie mithilfe von**:`UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="1b438-281">**Set using**: `UseWebRoot`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1b438-282">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1b438-282">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1b438-283">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1b438-283">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="1b438-284">Überschreiben die Konfiguration</span><span class="sxs-lookup"><span data-stu-id="1b438-284">Overriding configuration</span></span>

<span data-ttu-id="1b438-285">Verwendung [Konfiguration](xref:fundamentals/configuration/index) auf den Host konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="1b438-285">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="1b438-286">Im folgenden Beispiel wird optional Hostkonfiguration angegeben ein *hosting.json* Datei.</span><span class="sxs-lookup"><span data-stu-id="1b438-286">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="1b438-287">Eine Konfiguration geladen wird, aus der *hosting.json* Datei überschrieben werden kann, durch die Befehlszeilenargumente.</span><span class="sxs-lookup"><span data-stu-id="1b438-287">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="1b438-288">Die Konfiguration erstellt (in `config`) wird verwendet, um den Host mit konfigurieren `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="1b438-288">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1b438-289">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1b438-289">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="1b438-290">*Hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="1b438-290">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="1b438-291">Überschreiben die Konfiguration von bereitgestellten `UseUrls` mit *hosting.json* Config erste Befehlszeilen Argument Config zweiten:</span><span class="sxs-lookup"><span data-stu-id="1b438-291">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1b438-292">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1b438-292">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="1b438-293">*Hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="1b438-293">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="1b438-294">Überschreiben die Konfiguration von bereitgestellten `UseUrls` mit *hosting.json* Config erste Befehlszeilen Argument Config zweiten:</span><span class="sxs-lookup"><span data-stu-id="1b438-294">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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
> <span data-ttu-id="1b438-295">Die `UseConfiguration` Erweiterungsmethode ist zurzeit der Analyse eines zurückgegebenen Konfigurationsabschnitts kann nicht `GetSection` (z. B. `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="1b438-295">The `UseConfiguration` extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="1b438-296">Die `GetSection` Methode filtert die Schlüssel der Konfiguration im Abschnitt angefordert, sondern bleibt der Name des Abschnitts auf die Schlüssel (z. B. `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="1b438-296">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="1b438-297">Die `UseConfiguration` Methode erwartet die Schlüssel entsprechend dem `WebHostBuilder` Schlüssel (z. B. `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="1b438-297">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="1b438-298">Das Vorhandensein der Abschnittsname den Schlüsseln wird verhindert, dass Werte im Abschnitt konfigurieren den Host.</span><span class="sxs-lookup"><span data-stu-id="1b438-298">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="1b438-299">Dieses Problem wird in einer bevorstehenden Version behoben.</span><span class="sxs-lookup"><span data-stu-id="1b438-299">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="1b438-300">Weitere Informationen und problemumgehungen finden Sie unter [vollständige Schlüssel übergeben Konfigurationsabschnitt in WebHostBuilder.UseConfiguration verwendet](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="1b438-300">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="1b438-301">Um den Host aus, führen Sie auf eine bestimmte URL angeben, Sie könnten übergeben in den gewünschten Wert an einer Eingabeaufforderung Ausführung `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="1b438-301">To specify the host run on a particular URL, you could pass in the desired value from a command prompt when executing `dotnet run`.</span></span> <span data-ttu-id="1b438-302">Das Befehlszeilenargument überschreibt die `urls` Wert aus der *hosting.json* Datei und der Server lauscht an Port 8080:</span><span class="sxs-lookup"><span data-stu-id="1b438-302">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="ordering-importance"></a><span data-ttu-id="1b438-303">Reihenfolge der Wichtigkeit</span><span class="sxs-lookup"><span data-stu-id="1b438-303">Ordering importance</span></span>

<span data-ttu-id="1b438-304">Einige der `WebHostBuilder` Einstellungen werden zuerst aus der Umgebungsvariablen, gelesen, wenn festgelegt.</span><span class="sxs-lookup"><span data-stu-id="1b438-304">Some of the `WebHostBuilder` settings are first read from environment variables, if set.</span></span> <span data-ttu-id="1b438-305">Diese Umgebungsvariablen verwenden Sie das Format `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="1b438-305">These environment variables use the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="1b438-306">Legen Sie zum Festlegen der URLs, die der Server standardmäßig überwacht `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="1b438-306">To set the URLs that the server listens on by default, you set `ASPNETCORE_URLS`.</span></span>

<span data-ttu-id="1b438-307">Sie können eine der folgenden Werte für Umgebungsvariablen überschreiben, indem Konfiguration angeben (mit `UseConfiguration`) oder indem Sie den Wert explizit festlegen (mit `UseSetting` oder eine der expliziten Erweiterungsmethoden, wie z. B. `UseUrls`).</span><span class="sxs-lookup"><span data-stu-id="1b438-307">You can override any of these environment variable values by specifying configuration (using `UseConfiguration`) or by setting the value explicitly (using `UseSetting` or one of the explicit extension methods, such as `UseUrls`).</span></span> <span data-ttu-id="1b438-308">Vom Host verwendet wird, unabhängig davon, welche Option zuletzt legt den Wert fest.</span><span class="sxs-lookup"><span data-stu-id="1b438-308">The host uses whichever option sets the value last.</span></span> <span data-ttu-id="1b438-309">Wenn Sie programmgesteuert die Standard-URL auf einen Wert festgelegt, aber lassen sie bei der Konfiguration außer Kraft gesetzt werden möchten, können Sie eine Befehlszeilenkonfiguration nach dem Festlegen der URL.</span><span class="sxs-lookup"><span data-stu-id="1b438-309">If you want to programmatically set the default URL to one value but allow it to be overridden with configuration, you can use command-line configuration after setting the URL.</span></span> <span data-ttu-id="1b438-310">Finden Sie unter [überschreiben Konfiguration](#overriding-configuration).</span><span class="sxs-lookup"><span data-stu-id="1b438-310">See [Overriding configuration](#overriding-configuration).</span></span>

## <a name="starting-the-host"></a><span data-ttu-id="1b438-311">Der Host wird gestartet</span><span class="sxs-lookup"><span data-stu-id="1b438-311">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1b438-312">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1b438-312">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="1b438-313">**Run**</span><span class="sxs-lookup"><span data-stu-id="1b438-313">**Run**</span></span>

<span data-ttu-id="1b438-314">Die `Run` Methode startet die Web-app und blockiert den aufrufenden Thread, bis der Host heruntergefahren wird:</span><span class="sxs-lookup"><span data-stu-id="1b438-314">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="1b438-315">**Start**</span><span class="sxs-lookup"><span data-stu-id="1b438-315">**Start**</span></span>

<span data-ttu-id="1b438-316">Sie können den Host in eine nicht blockierende Weise ausführen, durch Aufrufen seiner `Start` Methode:</span><span class="sxs-lookup"><span data-stu-id="1b438-316">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="1b438-317">Wenn Sie eine Liste mit URLs zum Übergeben der `Start` -Methode, auf die angegebenen URLs überwacht:</span><span class="sxs-lookup"><span data-stu-id="1b438-317">If you pass a list of URLs to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="1b438-318">Sie initialisieren und starten Sie einen neuen Host mit den Standardwerten vorkonfiguriert, dass `CreateDefaultBuilder` mithilfe einer statischen Hilfsmethode.</span><span class="sxs-lookup"><span data-stu-id="1b438-318">You can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="1b438-319">Diese Methoden starten Sie den Server ohne Konsolenausgabe und [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) warten Sie, bis eine Unterbrechung (STRG-C/SIGINT oder SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="1b438-319">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="1b438-320">**Start (RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="1b438-320">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="1b438-321">Beginnen Sie mit einem `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="1b438-321">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="1b438-322">Führen Sie im Browser, um eine Anforderung `http://localhost:5000` zum Empfangen der Antwort "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="1b438-322">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="1b438-323">`WaitForShutdown`blockiert, bis eine Unterbrechung (STRG-C/SIGINT oder SIGTERM) ausgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="1b438-323">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="1b438-324">Die app zeigt die `Console.WriteLine` Nachricht und wartet, bis eine Keypress zu beenden.</span><span class="sxs-lookup"><span data-stu-id="1b438-324">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="1b438-325">**Start (String Url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="1b438-325">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="1b438-326">Beginnen Sie mit einer URL und `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="1b438-326">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="1b438-327">Führt zum gleiche Ergebnis wie **starten (RequestDelegate app)**, außer die Anwendung reagiert auf `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="1b438-327">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="1b438-328">**Starten (Aktion<IRouteBuilder> RouteBuilder)**</span><span class="sxs-lookup"><span data-stu-id="1b438-328">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="1b438-329">Verwenden Sie eine Instanz des `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) routing Middleware verwenden:</span><span class="sxs-lookup"><span data-stu-id="1b438-329">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="1b438-330">Verwenden Sie die folgenden Browseranforderungen mit dem Beispiel:</span><span class="sxs-lookup"><span data-stu-id="1b438-330">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="1b438-331">Anforderung</span><span class="sxs-lookup"><span data-stu-id="1b438-331">Request</span></span>                                    | <span data-ttu-id="1b438-332">Antwort</span><span class="sxs-lookup"><span data-stu-id="1b438-332">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="1b438-333">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="1b438-333">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="1b438-334">Buenos Massenzahlungsverkehrssysteme, Catrina!</span><span class="sxs-lookup"><span data-stu-id="1b438-334">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="1b438-335">Löst eine Ausnahme mit der Zeichenfolge "Ooops!"</span><span class="sxs-lookup"><span data-stu-id="1b438-335">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="1b438-336">Löst eine Ausnahme mit der Zeichenfolge "Uh oh!"</span><span class="sxs-lookup"><span data-stu-id="1b438-336">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="1b438-337">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="1b438-337">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="1b438-338">Hello World!</span><span class="sxs-lookup"><span data-stu-id="1b438-338">Hello World!</span></span>                             |

<span data-ttu-id="1b438-339">`WaitForShutdown`blockiert, bis eine Unterbrechung (STRG-C/SIGINT oder SIGTERM) ausgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="1b438-339">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="1b438-340">Die app zeigt die `Console.WriteLine` Nachricht und wartet, bis eine Keypress zu beenden.</span><span class="sxs-lookup"><span data-stu-id="1b438-340">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="1b438-341">**Starten (string Url, Aktion<IRouteBuilder> RouteBuilder)**</span><span class="sxs-lookup"><span data-stu-id="1b438-341">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="1b438-342">Verwenden Sie eine URL und eine Instanz von `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="1b438-342">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="1b438-343">Führt zum gleiche Ergebnis wie **starten (Aktion<IRouteBuilder> RouteBuilder)**, außer die Anwendung reagiert auf `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="1b438-343">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="1b438-344">**StartWith (Aktion<IApplicationBuilder> app)**</span><span class="sxs-lookup"><span data-stu-id="1b438-344">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="1b438-345">Geben Sie ein Delegat zum Konfigurieren einer `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="1b438-345">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="1b438-346">Führen Sie im Browser, um eine Anforderung `http://localhost:5000` zum Empfangen der Antwort "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="1b438-346">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="1b438-347">`WaitForShutdown`blockiert, bis eine Unterbrechung (STRG-C/SIGINT oder SIGTERM) ausgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="1b438-347">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="1b438-348">Die app zeigt die `Console.WriteLine` Nachricht und wartet, bis eine Keypress zu beenden.</span><span class="sxs-lookup"><span data-stu-id="1b438-348">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="1b438-349">**StartWith (string Url, Aktion<IApplicationBuilder> app)**</span><span class="sxs-lookup"><span data-stu-id="1b438-349">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="1b438-350">Geben Sie eine URL und ein Delegat zum Konfigurieren einer `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="1b438-350">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="1b438-351">Führt zum gleiche Ergebnis wie **StartWith (Aktion<IApplicationBuilder> app)**, außer die Anwendung reagiert auf `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="1b438-351">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1b438-352">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1b438-352">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="1b438-353">**Run**</span><span class="sxs-lookup"><span data-stu-id="1b438-353">**Run**</span></span>

<span data-ttu-id="1b438-354">Die `Run` Methode startet die Web-app und blockiert den aufrufenden Thread, bis der Host heruntergefahren wird:</span><span class="sxs-lookup"><span data-stu-id="1b438-354">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="1b438-355">**Start**</span><span class="sxs-lookup"><span data-stu-id="1b438-355">**Start**</span></span>

<span data-ttu-id="1b438-356">Sie können den Host in eine nicht blockierende Weise ausführen, durch Aufrufen seiner `Start` Methode:</span><span class="sxs-lookup"><span data-stu-id="1b438-356">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="1b438-357">Wenn Sie eine Liste mit URLs zum Übergeben der `Start` -Methode, auf die angegebenen URLs überwacht:</span><span class="sxs-lookup"><span data-stu-id="1b438-357">If you pass a list of URLs to the `Start` method, it listens on the URLs specified:</span></span>


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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="1b438-358">IHostingEnvironment-Schnittstelle</span><span class="sxs-lookup"><span data-stu-id="1b438-358">IHostingEnvironment interface</span></span>

<span data-ttu-id="1b438-359">Die [IHostingEnvironment Schnittstelle](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) enthält Informationen zu den app-Web-hosting-Umgebung.</span><span class="sxs-lookup"><span data-stu-id="1b438-359">The [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="1b438-360">Können Sie [Konstruktoreinfügung](xref:fundamentals/dependency-injection) zum Abrufen der `IHostingEnvironment` um die Eigenschaften und die Erweiterungsmethoden verwenden:</span><span class="sxs-lookup"><span data-stu-id="1b438-360">You can use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="1b438-361">Können Sie eine [konventionsbasierte Ansatz](xref:fundamentals/environments#startup-conventions) zum Konfigurieren Ihrer Anwendung beim Start auf Grundlage der Umgebung.</span><span class="sxs-lookup"><span data-stu-id="1b438-361">You can use a [convention-based approach](xref:fundamentals/environments#startup-conventions) to configure your app at startup based on the environment.</span></span> <span data-ttu-id="1b438-362">Alternativ können Sie Einfügen der `IHostingEnvironment` in der `Startup` Konstruktor für die Verwendung in `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="1b438-362">Alternatively, you can inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="1b438-363">Zusätzlich zu den `IsDevelopment` Erweiterungsmethode, `IHostingEnvironment` bietet `IsStaging`, `IsProduction`, und `IsEnvironment(string environmentName)` Methoden.</span><span class="sxs-lookup"><span data-stu-id="1b438-363">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="1b438-364">Finden Sie unter [arbeiten mit mehreren Umgebungen](xref:fundamentals/environments) Details.</span><span class="sxs-lookup"><span data-stu-id="1b438-364">See [Working with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="1b438-365">Die `IHostingEnvironment` Service kann auch direkt in injiziert die `Configure` Methode zum Einrichten der Verarbeitungspipeline:</span><span class="sxs-lookup"><span data-stu-id="1b438-365">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up your processing pipeline:</span></span>

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

<span data-ttu-id="1b438-366">Sie können einfügen `IHostingEnvironment` in der `Invoke` Methode, wenn benutzerdefinierte [Middleware](xref:fundamentals/middleware#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="1b438-366">You can inject `IHostingEnvironment` into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="1b438-367">IApplicationLifetime-Schnittstelle</span><span class="sxs-lookup"><span data-stu-id="1b438-367">IApplicationLifetime interface</span></span>

<span data-ttu-id="1b438-368">Die [IApplicationLifetime Schnittstelle](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) können Sie nach dem Starten und Herunterfahren Aktivitäten ausführen.</span><span class="sxs-lookup"><span data-stu-id="1b438-368">The [IApplicationLifetime interface](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows you to perform post-startup and shutdown activities.</span></span> <span data-ttu-id="1b438-369">Drei Eigenschaften auf der Schnittstelle sind Abbruchtoken, das Sie beim registrieren können `Action` Methoden zum Starten und Herunterfahren Ereignisse zu definieren.</span><span class="sxs-lookup"><span data-stu-id="1b438-369">Three properties on the interface are cancellation tokens that you can register with `Action` methods to define startup and shutdown events.</span></span> <span data-ttu-id="1b438-370">Es gibt auch eine `StopApplication` Methode.</span><span class="sxs-lookup"><span data-stu-id="1b438-370">There's also a `StopApplication` method.</span></span>

| <span data-ttu-id="1b438-371">Abbruchtoken</span><span class="sxs-lookup"><span data-stu-id="1b438-371">Cancellation Token</span></span>    | <span data-ttu-id="1b438-372">Wird ausgelöst, wenn &#8230;</span><span class="sxs-lookup"><span data-stu-id="1b438-372">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="1b438-373">Der Host wurde vollständig gestartet.</span><span class="sxs-lookup"><span data-stu-id="1b438-373">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="1b438-374">Der Host führt ein ordnungsgemäßes Herunterfahren.</span><span class="sxs-lookup"><span data-stu-id="1b438-374">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="1b438-375">Anforderungen möglicherweise noch verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="1b438-375">Requests may still be processing.</span></span> <span data-ttu-id="1b438-376">Shutdown ist blockiert, bis diese abgeschlossen wird.</span><span class="sxs-lookup"><span data-stu-id="1b438-376">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="1b438-377">Der Host wird ein ordnungsgemäßes Herunterfahren abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="1b438-377">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="1b438-378">Alle Anforderungen sollten vollständig verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="1b438-378">All requests should be completely processed.</span></span> <span data-ttu-id="1b438-379">Shutdown ist blockiert, bis diese abgeschlossen wird.</span><span class="sxs-lookup"><span data-stu-id="1b438-379">Shutdown blocks until this event completes.</span></span> |

| <span data-ttu-id="1b438-380">Methode</span><span class="sxs-lookup"><span data-stu-id="1b438-380">Method</span></span>            | <span data-ttu-id="1b438-381">Aktion</span><span class="sxs-lookup"><span data-stu-id="1b438-381">Action</span></span>                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | <span data-ttu-id="1b438-382">Anforderungen Beendigung der aktuellen Anwendung.</span><span class="sxs-lookup"><span data-stu-id="1b438-382">Requests termination of the current application.</span></span> |

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

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="1b438-383">Problembehandlung bei System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="1b438-383">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="1b438-384">**Gilt für nur ASP.NET Core 2.0**</span><span class="sxs-lookup"><span data-stu-id="1b438-384">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="1b438-385">Wenn Sie den Host von Räumen erstellen `IStartup` direkt in die abhängigkeitseinschleusungscontainer anstelle von Aufrufen `UseStartup` oder `Configure`, treten möglicherweise die folgenden Fehler: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.</span><span class="sxs-lookup"><span data-stu-id="1b438-385">If you build the host by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`, you may encounter the following error: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.</span></span>

<span data-ttu-id="1b438-386">Dies geschieht, weil die [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (die aktuelle Assembly) ist erforderlich, um die Überprüfung `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="1b438-386">This occurs because the [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="1b438-387">Wenn Sie manuell einfügen `IStartup` in der abhängigkeitseinschleusungscontainer hinzufügen den folgenden Aufruf Ihrer `WebHostBuilder` mit dem Assemblynamen angegeben:</span><span class="sxs-lookup"><span data-stu-id="1b438-387">If you manually inject `IStartup` into the dependency injection container, add the following call to your `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="1b438-388">Fügen Sie alternativ eine Dummy `Configure` auf Ihre `WebHostBuilder`, welche die `applicationName`(`ApplicationKey`) automatisch:</span><span class="sxs-lookup"><span data-stu-id="1b438-388">Alternatively, add a dummy `Configure` to your `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="1b438-389">**Hinweis**: Dies ist nur erforderlich, mit der Version 2.0 von ASP.NET Core und nur beim Aufrufen nicht `UseStartup` oder `Configure`.</span><span class="sxs-lookup"><span data-stu-id="1b438-389">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when you don't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="1b438-390">Weitere Informationen finden Sie unter [Ankündigungen: Microsoft.Extensions.PlatformAbstractions wurde entfernt / / (Kommentar)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) und [StartupInjection Beispiel](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="1b438-390">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1b438-391">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="1b438-391">Additional resources</span></span>

* [<span data-ttu-id="1b438-392">Veröffentlichen Sie in Windows unter Verwendung von IIS</span><span class="sxs-lookup"><span data-stu-id="1b438-392">Publish to Windows using IIS</span></span>](../publishing/iis.md)
* [<span data-ttu-id="1b438-393">Veröffentlichen Sie in Linux mit Nginx</span><span class="sxs-lookup"><span data-stu-id="1b438-393">Publish to Linux using Nginx</span></span>](../publishing/linuxproduction.md)
* [<span data-ttu-id="1b438-394">Veröffentlichen Sie in Linux mit Apache</span><span class="sxs-lookup"><span data-stu-id="1b438-394">Publish to Linux using Apache</span></span>](../publishing/apache-proxy.md)
* [<span data-ttu-id="1b438-395">Host in einem Windowsdienst</span><span class="sxs-lookup"><span data-stu-id="1b438-395">Host in a Windows Service</span></span>](xref:hosting/windows-service)
