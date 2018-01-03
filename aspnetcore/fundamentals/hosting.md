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
ms.openlocfilehash: 0ee8827ad3d5464e1645a40d453054b9e23641cf
ms.sourcegitcommit: 281f0c614543a6c3db565ea4655b70fe49b61d84
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/03/2018
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="81fb7-104">Hosten in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="81fb7-104">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="81fb7-105">Von [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="81fb7-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="81fb7-106">ASP.NET Core apps konfigurieren und starten Sie eine *Host*.</span><span class="sxs-lookup"><span data-stu-id="81fb7-106">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="81fb7-107">Der Host ist verantwortlich für die Verwaltung von Apps starten sowie seine Lebensdauer.</span><span class="sxs-lookup"><span data-stu-id="81fb7-107">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="81fb7-108">Mindestens wird der Host einem Server und einem Anforderungsverarbeitungspipeline konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="81fb7-108">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="81fb7-109">Einrichten eines Hosts</span><span class="sxs-lookup"><span data-stu-id="81fb7-109">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="81fb7-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="81fb7-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="81fb7-111">Erstellen Sie einen Host mit einer Instanz von [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="81fb7-111">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="81fb7-112">Dies erfolgt in der Regel am Einstiegspunkt der Anwendung, die `Main` Methode.</span><span class="sxs-lookup"><span data-stu-id="81fb7-112">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="81fb7-113">In den Vorlagen für Projekte `Main` befindet sich im *"Program.cs"*.</span><span class="sxs-lookup"><span data-stu-id="81fb7-113">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="81fb7-114">Eine typische *"Program.cs"* Aufrufe [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) zum Einrichten eines Hosts zu starten:</span><span class="sxs-lookup"><span data-stu-id="81fb7-114">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="81fb7-115">`CreateDefaultBuilder`führt die folgenden Aufgaben:</span><span class="sxs-lookup"><span data-stu-id="81fb7-115">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="81fb7-116">Konfiguriert die [Kestrel](servers/kestrel.md) wie der Webserver.</span><span class="sxs-lookup"><span data-stu-id="81fb7-116">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="81fb7-117">Die Standardoptionen Kestrel finden Sie unter [der Kestrel Optionen im Abschnitt Kestrel webserverimplementierung in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="81fb7-117">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="81fb7-118">Legt die Inhalten-Stamm auf den Pfad zurückgegebenes [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="81fb7-118">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="81fb7-119">Lädt die optionale Konfiguration aus:</span><span class="sxs-lookup"><span data-stu-id="81fb7-119">Loads optional configuration from:</span></span>
  * <span data-ttu-id="81fb7-120">*AppSettings.JSON*.</span><span class="sxs-lookup"><span data-stu-id="81fb7-120">*appsettings.json*.</span></span>
  * <span data-ttu-id="81fb7-121">*"appSettings". {Umgebung} JSON*.</span><span class="sxs-lookup"><span data-stu-id="81fb7-121">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="81fb7-122">[Vertrauliche Benutzerdaten](xref:security/app-secrets) Wenn die app ausgeführt wird, der `Development` Umgebung.</span><span class="sxs-lookup"><span data-stu-id="81fb7-122">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="81fb7-123">Umgebungsvariablen.</span><span class="sxs-lookup"><span data-stu-id="81fb7-123">Environment variables.</span></span>
  * <span data-ttu-id="81fb7-124">Befehlszeilenargumente.</span><span class="sxs-lookup"><span data-stu-id="81fb7-124">Command-line arguments.</span></span>
* <span data-ttu-id="81fb7-125">Konfiguriert die [Protokollierung](xref:fundamentals/logging/index) für Konsole und Debug-Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="81fb7-125">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="81fb7-126">Protokollierung umfasst [Protokoll filtern](xref:fundamentals/logging/index#log-filtering) Regeln in einem Konfigurationsabschnitt Protokollierung ein *appsettings.json* oder *"appSettings". {} Umgebung} JSON* Datei.</span><span class="sxs-lookup"><span data-stu-id="81fb7-126">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="81fb7-127">Wenn hinter dem IIS ausgeführt wird, ermöglicht [IIS Integration](xref:publishing/iis).</span><span class="sxs-lookup"><span data-stu-id="81fb7-127">When running behind IIS, enables [IIS integration](xref:publishing/iis).</span></span> <span data-ttu-id="81fb7-128">Konfiguriert den Basispfad und der Port, auf der Server überwacht werden soll bei Verwendung der [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="81fb7-128">Configures the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="81fb7-129">Das Modul erstellt einen Reverse-Proxy zwischen IIS und Kestrel.</span><span class="sxs-lookup"><span data-stu-id="81fb7-129">The module creates a reverse-proxy between IIS and Kestrel.</span></span> <span data-ttu-id="81fb7-130">Außerdem konfiguriert die app, [Erfassen von Startfehlern](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="81fb7-130">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="81fb7-131">Die Standardoptionen von IIS finden Sie unter [der IIS-Optionen im Abschnitt Host ASP.NET Core unter Windows mit IIS](xref:publishing/iis#iis-options).</span><span class="sxs-lookup"><span data-stu-id="81fb7-131">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:publishing/iis#iis-options).</span></span>

<span data-ttu-id="81fb7-132">Die *Content Stamm* bestimmt, an denen der Host für Inhaltsdateien, z. B. MVC-Ansicht-Dateien sucht.</span><span class="sxs-lookup"><span data-stu-id="81fb7-132">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="81fb7-133">Wenn die app aus dem Stammordner des Projekts gestartet wird, wird als Inhalt Stamm Stammordner des Projekts verwendet.</span><span class="sxs-lookup"><span data-stu-id="81fb7-133">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="81fb7-134">Dies ist die Standardeinstellung verwendet [Visual Studio](https://www.visualstudio.com/) und [Dotnet neue Vorlagen](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="81fb7-134">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="81fb7-135">Weitere Informationen zur app-Konfiguration finden Sie unter [Konfiguration in ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="81fb7-135">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="81fb7-136">Als Alternative zur Verwendung der statischen `CreateDefaultBuilder` -Methode, erstellen einen Host aus [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) ist ein unterstützten Ansatz mit ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="81fb7-136">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="81fb7-137">Weitere Informationen finden Sie unter der Registerkarte "ASP.NET Core 1.x".</span><span class="sxs-lookup"><span data-stu-id="81fb7-137">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="81fb7-138">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="81fb7-138">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="81fb7-139">Erstellen Sie einen Host mit einer Instanz von [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="81fb7-139">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="81fb7-140">Erstellen einen Host erfolgt in der Regel am Einstiegspunkt der Anwendung, die `Main` Methode.</span><span class="sxs-lookup"><span data-stu-id="81fb7-140">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="81fb7-141">In den Vorlagen für Projekte `Main` befindet sich im *"Program.cs"*:</span><span class="sxs-lookup"><span data-stu-id="81fb7-141">In the project templates, `Main` is located in *Program.cs*:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="81fb7-142">`WebHostBuilder`erfordert eine [Server, die Iserver-c# implementiert](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="81fb7-142">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="81fb7-143">Die integrierten Server sind [Kestrel](servers/kestrel.md) und [HTTP.sys](servers/httpsys.md) (vor der Veröffentlichung von ASP.NET Core 2.0, HTTP.sys hieß [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="81fb7-143">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="81fb7-144">In diesem Beispiel wird die [UseKestrel Erweiterungsmethode](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) gibt den Kestrel-Server.</span><span class="sxs-lookup"><span data-stu-id="81fb7-144">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="81fb7-145">Die *Content Stamm* bestimmt, an denen der Host für Inhaltsdateien, z. B. MVC-Ansicht-Dateien sucht.</span><span class="sxs-lookup"><span data-stu-id="81fb7-145">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="81fb7-146">Der standardmäßige Content Stamm für abgerufen wird `UseContentRoot` von [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="81fb7-146">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="81fb7-147">Wenn die app aus dem Stammordner des Projekts gestartet wird, wird als Inhalt Stamm Stammordner des Projekts verwendet.</span><span class="sxs-lookup"><span data-stu-id="81fb7-147">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="81fb7-148">Dies ist die Standardeinstellung verwendet [Visual Studio](https://www.visualstudio.com/) und [Dotnet neue Vorlagen](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="81fb7-148">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="81fb7-149">Rufen Sie zum Verwenden von IIS als Reverseproxy [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) im Rahmen der Erstellung des Hosts.</span><span class="sxs-lookup"><span data-stu-id="81fb7-149">To use IIS as a reverse proxy, call [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="81fb7-150">`UseIISIntegration`Konfigurieren Sie keine *Server*, z. B. [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) verfügt.</span><span class="sxs-lookup"><span data-stu-id="81fb7-150">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="81fb7-151">`UseIISIntegration`konfiguriert den Basispfad und der Port, auf der Server überwacht werden soll bei Verwendung der [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) einen Reverse-Proxy zwischen Kestrel und IIS zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="81fb7-151">`UseIISIntegration` configures the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse-proxy between Kestrel and IIS.</span></span> <span data-ttu-id="81fb7-152">Für die Verwendung von IIS mit ASP.NET Core, `UseKestrel` und `UseIISIntegration` muss angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="81fb7-152">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="81fb7-153">`UseIISIntegration`nur aktiviert, wenn hinter der IIS- oder IIS Express ausführen.</span><span class="sxs-lookup"><span data-stu-id="81fb7-153">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="81fb7-154">Weitere Informationen finden Sie unter [Einführung in ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) und [ASP.NET Core-Modul Konfigurationsverweis](xref:hosting/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="81fb7-154">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:hosting/aspnet-core-module).</span></span>

<span data-ttu-id="81fb7-155">Eine minimale Implementierung, die einen Host (und ASP.NET Core-app) konfiguriert, enthält einen Server und die Konfiguration der Anforderungspipeline die app angeben:</span><span class="sxs-lookup"><span data-stu-id="81fb7-155">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="81fb7-156">Beim Einrichten von eines Hosts [konfigurieren](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) und [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) Methoden bereitgestellt werden können.</span><span class="sxs-lookup"><span data-stu-id="81fb7-156">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="81fb7-157">Wenn eine `Startup` Klasse angegeben wird, muss er definieren eine `Configure` Methode.</span><span class="sxs-lookup"><span data-stu-id="81fb7-157">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="81fb7-158">Weitere Informationen finden Sie unter [Start der Anwendung in ASP.NET Core](startup.md).</span><span class="sxs-lookup"><span data-stu-id="81fb7-158">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="81fb7-159">Mehrere Aufrufe `ConfigureServices` untereinander anfügen.</span><span class="sxs-lookup"><span data-stu-id="81fb7-159">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="81fb7-160">Mehrere Aufrufe `Configure` oder `UseStartup` auf die `WebHostBuilder` ersetzen die vorherige Einstellungen.</span><span class="sxs-lookup"><span data-stu-id="81fb7-160">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="81fb7-161">Host-Konfigurationswerte</span><span class="sxs-lookup"><span data-stu-id="81fb7-161">Host configuration values</span></span>

<span data-ttu-id="81fb7-162">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) basiert auf der folgenden Ansätze, um die Konfigurationswerte für den Host festgelegt:</span><span class="sxs-lookup"><span data-stu-id="81fb7-162">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="81fb7-163">Host-Generator-Konfiguration, die Umgebungsvariablen mit dem Format enthält `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="81fb7-163">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="81fb7-164">Beispielsweise `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="81fb7-164">For example, `ASPNETCORE_URLS`.</span></span>
* <span data-ttu-id="81fb7-165">Explizite Methoden wie z. B. `CaptureStartupErrors`.</span><span class="sxs-lookup"><span data-stu-id="81fb7-165">Explicit methods, such as `CaptureStartupErrors`.</span></span>
* <span data-ttu-id="81fb7-166">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) und den zugehörigen Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="81fb7-166">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="81fb7-167">Beim Festlegen eines Werts mit `UseSetting`, der Wert wird als Zeichenfolge unabhängig vom Typ festgelegt.</span><span class="sxs-lookup"><span data-stu-id="81fb7-167">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="81fb7-168">Vom Host verwendet wird, unabhängig davon, welche Option zuletzt legt einen Wert fest.</span><span class="sxs-lookup"><span data-stu-id="81fb7-168">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="81fb7-169">Weitere Informationen finden Sie unter [überschreiben Konfiguration](#overriding-configuration) im nächsten Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="81fb7-169">For more information, see [Overriding configuration](#overriding-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="81fb7-170">Erfassen von Startfehlern</span><span class="sxs-lookup"><span data-stu-id="81fb7-170">Capture Startup Errors</span></span>

<span data-ttu-id="81fb7-171">Diese Einstellung steuert die Erfassung von Startfehlern.</span><span class="sxs-lookup"><span data-stu-id="81fb7-171">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="81fb7-172">**Schlüssel**: CaptureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="81fb7-172">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="81fb7-173">**Typ**: *Bool* (`true` oder `1`)</span><span class="sxs-lookup"><span data-stu-id="81fb7-173">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="81fb7-174">**Standardmäßige**: standardmäßig `false` , wenn die app mit Kestrel hinter dem IIS ausgeführt wird, in dem die Standardeinstellung ist `true`.</span><span class="sxs-lookup"><span data-stu-id="81fb7-174">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="81fb7-175">**Legen Sie mithilfe von**:`CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="81fb7-175">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="81fb7-176">**Umgebungsvariable**:`ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="81fb7-176">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="81fb7-177">Wenn `false`, Fehler während des Starts Ergebnis auf dem Host beendet.</span><span class="sxs-lookup"><span data-stu-id="81fb7-177">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="81fb7-178">Wenn `true`, der Host erfasst Ausnahmen während des Starts und versucht, den Server zu starten.</span><span class="sxs-lookup"><span data-stu-id="81fb7-178">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="81fb7-179">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="81fb7-179">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="81fb7-180">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="81fb7-180">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="81fb7-181">Inhalts-Stamm</span><span class="sxs-lookup"><span data-stu-id="81fb7-181">Content Root</span></span>

<span data-ttu-id="81fb7-182">Diese Einstellung bestimmt, in dem ASP.NET Core beginnt die Suche nach Inhaltsdateien gespeichert, z. B. MVC-Ansichten.</span><span class="sxs-lookup"><span data-stu-id="81fb7-182">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="81fb7-183">**Schlüssel**: ContentRoot</span><span class="sxs-lookup"><span data-stu-id="81fb7-183">**Key**: contentRoot</span></span>  
<span data-ttu-id="81fb7-184">**Typ**: *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="81fb7-184">**Type**: *string*</span></span>  
<span data-ttu-id="81fb7-185">**Standard**: standardmäßig auf den Ordner, in dem die app-Assembly gespeichert ist.</span><span class="sxs-lookup"><span data-stu-id="81fb7-185">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="81fb7-186">**Legen Sie mithilfe von**:`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="81fb7-186">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="81fb7-187">**Umgebungsvariable**:`ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="81fb7-187">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="81fb7-188">Die Inhalte-Stamm dient außerdem als den Basispfad für den [Stammweb-Einstellung](#web-root).</span><span class="sxs-lookup"><span data-stu-id="81fb7-188">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="81fb7-189">Wenn der Pfad nicht vorhanden ist, kann der Host nicht gestartet.</span><span class="sxs-lookup"><span data-stu-id="81fb7-189">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="81fb7-190">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="81fb7-190">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="81fb7-191">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="81fb7-191">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="81fb7-192">Ausführliche Fehler</span><span class="sxs-lookup"><span data-stu-id="81fb7-192">Detailed Errors</span></span>

<span data-ttu-id="81fb7-193">Bestimmt, ob ausführliche Fehler erfasst werden sollen.</span><span class="sxs-lookup"><span data-stu-id="81fb7-193">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="81fb7-194">**Schlüssel**: DetailedErrors</span><span class="sxs-lookup"><span data-stu-id="81fb7-194">**Key**: detailedErrors</span></span>  
<span data-ttu-id="81fb7-195">**Typ**: *Bool* (`true` oder `1`)</span><span class="sxs-lookup"><span data-stu-id="81fb7-195">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="81fb7-196">**Standard**: "false"</span><span class="sxs-lookup"><span data-stu-id="81fb7-196">**Default**: false</span></span>  
<span data-ttu-id="81fb7-197">**Legen Sie mithilfe von**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="81fb7-197">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="81fb7-198">**Umgebungsvariable**:`ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="81fb7-198">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="81fb7-199">Wenn aktiviert (oder wenn die <a href="#environment">Umgebung</a> auf festgelegt ist `Development`), die app erfasst ausführliche Ausnahmen.</span><span class="sxs-lookup"><span data-stu-id="81fb7-199">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="81fb7-200">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="81fb7-200">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="81fb7-201">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="81fb7-201">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="81fb7-202">Umgebung</span><span class="sxs-lookup"><span data-stu-id="81fb7-202">Environment</span></span>

<span data-ttu-id="81fb7-203">Legt fest, die app-Umgebung.</span><span class="sxs-lookup"><span data-stu-id="81fb7-203">Sets the app's environment.</span></span>

<span data-ttu-id="81fb7-204">**Schlüssel**: Umgebung</span><span class="sxs-lookup"><span data-stu-id="81fb7-204">**Key**: environment</span></span>  
<span data-ttu-id="81fb7-205">**Typ**: *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="81fb7-205">**Type**: *string*</span></span>  
<span data-ttu-id="81fb7-206">**Standard**: Produktion</span><span class="sxs-lookup"><span data-stu-id="81fb7-206">**Default**: Production</span></span>  
<span data-ttu-id="81fb7-207">**Legen Sie mithilfe von**:`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="81fb7-207">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="81fb7-208">**Umgebungsvariable**:`ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="81fb7-208">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="81fb7-209">Die Umgebung kann auf einen beliebigen Wert festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="81fb7-209">The environment can be set to any value.</span></span> <span data-ttu-id="81fb7-210">Framework definierte Werte sind `Development`, `Staging`, und `Production`.</span><span class="sxs-lookup"><span data-stu-id="81fb7-210">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="81fb7-211">Werte sind keine Groß-/Kleinschreibung beachtet.</span><span class="sxs-lookup"><span data-stu-id="81fb7-211">Values aren't case sensitive.</span></span> <span data-ttu-id="81fb7-212">Wird standardmäßig die *Umgebung* auslesen ist die `ASPNETCORE_ENVIRONMENT` -Umgebungsvariablen angegeben.</span><span class="sxs-lookup"><span data-stu-id="81fb7-212">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="81fb7-213">Bei Verwendung [Visual Studio](https://www.visualstudio.com/), Umgebungsvariablen festgelegt werden können, der *launchSettings.json* Datei.</span><span class="sxs-lookup"><span data-stu-id="81fb7-213">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="81fb7-214">Weitere Informationen finden Sie unter [Working with Multiple Environments (Verwenden von mehreren Umgebungen)](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="81fb7-214">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="81fb7-215">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="81fb7-215">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="81fb7-216">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="81fb7-216">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="81fb7-217">Hosten Startassemblys</span><span class="sxs-lookup"><span data-stu-id="81fb7-217">Hosting Startup Assemblies</span></span>

<span data-ttu-id="81fb7-218">Legt die app hosten Startassemblys fest.</span><span class="sxs-lookup"><span data-stu-id="81fb7-218">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="81fb7-219">**Schlüssel**: HostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="81fb7-219">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="81fb7-220">**Typ**: *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="81fb7-220">**Type**: *string*</span></span>  
<span data-ttu-id="81fb7-221">**Standard**: leere Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="81fb7-221">**Default**: Empty string</span></span>  
<span data-ttu-id="81fb7-222">**Legen Sie mithilfe von**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="81fb7-222">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="81fb7-223">**Umgebungsvariable**:`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="81fb7-223">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="81fb7-224">Eine durch Semikolons getrennte Zeichenfolge hosten Startassemblys beim Start geladen werden soll.</span><span class="sxs-lookup"><span data-stu-id="81fb7-224">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="81fb7-225">Dieses Feature ist neu in ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="81fb7-225">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="81fb7-226">Obwohl die Konfiguration auf eine leere Zeichenfolge wird als Standardwert, enthalten hosting Autostart-Assemblys immer der app-Assembly.</span><span class="sxs-lookup"><span data-stu-id="81fb7-226">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="81fb7-227">Beim hosting Startassemblys bereitgestellt werden, werden diese hinzugefügt auf die app-Assembly für das Laden, wenn die app während des Starts der gemeinsamen Dienste erstellt.</span><span class="sxs-lookup"><span data-stu-id="81fb7-227">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="81fb7-228">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="81fb7-228">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="81fb7-229">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="81fb7-229">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="81fb7-230">Diese Funktion steht in ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="81fb7-230">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="81fb7-231">Bevorzugen Sie Hosten von URLs</span><span class="sxs-lookup"><span data-stu-id="81fb7-231">Prefer Hosting URLs</span></span>

<span data-ttu-id="81fb7-232">Gibt an, ob der Host werden, auf die URLs abgehört soll mit konfiguriert die `WebHostBuilder` nicht die dafür konfigurierten mit der `IServer` Implementierung.</span><span class="sxs-lookup"><span data-stu-id="81fb7-232">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="81fb7-233">**Schlüssel**: PreferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="81fb7-233">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="81fb7-234">**Typ**: *Bool* (`true` oder `1`)</span><span class="sxs-lookup"><span data-stu-id="81fb7-234">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="81fb7-235">**Standard**: "true"</span><span class="sxs-lookup"><span data-stu-id="81fb7-235">**Default**: true</span></span>  
<span data-ttu-id="81fb7-236">**Legen Sie mithilfe von**:`PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="81fb7-236">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="81fb7-237">**Umgebungsvariable**:`ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="81fb7-237">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="81fb7-238">Dieses Feature ist neu in ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="81fb7-238">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="81fb7-239">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="81fb7-239">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="81fb7-240">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="81fb7-240">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="81fb7-241">Diese Funktion steht in ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="81fb7-241">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="81fb7-242">Zu verhindern, dass Hosting starten</span><span class="sxs-lookup"><span data-stu-id="81fb7-242">Prevent Hosting Startup</span></span>

<span data-ttu-id="81fb7-243">Verhindert, dass das automatische Laden von hosting Startassemblys, einschließlich hosting Autostart-Assemblys, die von der app-Assembly konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="81fb7-243">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="81fb7-244">Finden Sie unter [Hinzufügen von app-Funktionen aus einer externen Assembly IHostingStartup](xref:hosting/ihostingstartup) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="81fb7-244">See [Add app features from an external assembly using IHostingStartup](xref:hosting/ihostingstartup) for more information.</span></span>

<span data-ttu-id="81fb7-245">**Schlüssel**: PreventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="81fb7-245">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="81fb7-246">**Typ**: *Bool* (`true` oder `1`)</span><span class="sxs-lookup"><span data-stu-id="81fb7-246">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="81fb7-247">**Standard**: "false"</span><span class="sxs-lookup"><span data-stu-id="81fb7-247">**Default**: false</span></span>  
<span data-ttu-id="81fb7-248">**Legen Sie mithilfe von**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="81fb7-248">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="81fb7-249">**Umgebungsvariable**:`ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="81fb7-249">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="81fb7-250">Dieses Feature ist neu in ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="81fb7-250">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="81fb7-251">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="81fb7-251">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="81fb7-252">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="81fb7-252">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="81fb7-253">Diese Funktion steht in ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="81fb7-253">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="81fb7-254">Server-URLs</span><span class="sxs-lookup"><span data-stu-id="81fb7-254">Server URLs</span></span>

<span data-ttu-id="81fb7-255">Gibt die IP-Adressen oder Adressen mit Ports und Protokolle, die der Server auf Anforderungen Lauschen soll.</span><span class="sxs-lookup"><span data-stu-id="81fb7-255">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="81fb7-256">**Schlüssel**: Urls</span><span class="sxs-lookup"><span data-stu-id="81fb7-256">**Key**: urls</span></span>  
<span data-ttu-id="81fb7-257">**Typ**: *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="81fb7-257">**Type**: *string*</span></span>  
<span data-ttu-id="81fb7-258">**Standard**: http://localhost: 5000</span><span class="sxs-lookup"><span data-stu-id="81fb7-258">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="81fb7-259">**Legen Sie mithilfe von**:`UseUrls`</span><span class="sxs-lookup"><span data-stu-id="81fb7-259">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="81fb7-260">**Umgebungsvariable**:`ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="81fb7-260">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="81fb7-261">Legen Sie auf eine durch Semikolon getrennt (;) Liste von URL-Präfixen, die auf der Server reagieren soll.</span><span class="sxs-lookup"><span data-stu-id="81fb7-261">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="81fb7-262">Beispielsweise `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="81fb7-262">For example, `http://localhost:123`.</span></span> <span data-ttu-id="81fb7-263">Mit "\*", um anzugeben, dass der Server nach Anforderungen für alle IP-Adresse oder Hostname mit dem angegebenen Port und Protokoll abgehört werden soll (z. B. `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="81fb7-263">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="81fb7-264">Das Protokoll (`http://` oder `https://`) für jede URL eingeschlossen werden müssen.</span><span class="sxs-lookup"><span data-stu-id="81fb7-264">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="81fb7-265">Zu den unterstützten Bildformaten variieren zwischen Servern.</span><span class="sxs-lookup"><span data-stu-id="81fb7-265">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="81fb7-266">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="81fb7-266">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="81fb7-267">Kestrel verfügt über einen eigenen Endpunkt-Konfigurations-API.</span><span class="sxs-lookup"><span data-stu-id="81fb7-267">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="81fb7-268">Weitere Informationen finden Sie unter [Kestrel web server implementation in ASP.NET Core (Kestrel-Webserverimplementierung in ASP.NET Core)](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="81fb7-268">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="81fb7-269">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="81fb7-269">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="81fb7-270">Timeout beim Herunterfahren</span><span class="sxs-lookup"><span data-stu-id="81fb7-270">Shutdown Timeout</span></span>

<span data-ttu-id="81fb7-271">Gibt den Umfang der Wartezeit für den Webhost zum Herunterfahren.</span><span class="sxs-lookup"><span data-stu-id="81fb7-271">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="81fb7-272">**Schlüssel**: ShutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="81fb7-272">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="81fb7-273">**Typ**: *Int*</span><span class="sxs-lookup"><span data-stu-id="81fb7-273">**Type**: *int*</span></span>  
<span data-ttu-id="81fb7-274">**Standard**: 5</span><span class="sxs-lookup"><span data-stu-id="81fb7-274">**Default**: 5</span></span>  
<span data-ttu-id="81fb7-275">**Legen Sie mithilfe von**:`UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="81fb7-275">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="81fb7-276">**Umgebungsvariable**:`ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="81fb7-276">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="81fb7-277">Obwohl der Schlüssel akzeptiert ein *Int* mit `UseSetting` (z. B. `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), die `UseShutdownTimeout` Erweiterungsmethode akzeptiert eine `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="81fb7-277">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the `UseShutdownTimeout` extension method takes a `TimeSpan`.</span></span> <span data-ttu-id="81fb7-278">Dieses Feature ist neu in ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="81fb7-278">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="81fb7-279">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="81fb7-279">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="81fb7-280">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="81fb7-280">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="81fb7-281">Diese Funktion steht in ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="81fb7-281">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="81fb7-282">Startassembly</span><span class="sxs-lookup"><span data-stu-id="81fb7-282">Startup Assembly</span></span>

<span data-ttu-id="81fb7-283">Die Assembly zu suchende bestimmt die `Startup` Klasse.</span><span class="sxs-lookup"><span data-stu-id="81fb7-283">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="81fb7-284">**Schlüssel**: StartupAssembly</span><span class="sxs-lookup"><span data-stu-id="81fb7-284">**Key**: startupAssembly</span></span>  
<span data-ttu-id="81fb7-285">**Typ**: *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="81fb7-285">**Type**: *string*</span></span>  
<span data-ttu-id="81fb7-286">**Standard**: die app-Assembly</span><span class="sxs-lookup"><span data-stu-id="81fb7-286">**Default**: The app's assembly</span></span>  
<span data-ttu-id="81fb7-287">**Legen Sie mithilfe von**:`UseStartup`</span><span class="sxs-lookup"><span data-stu-id="81fb7-287">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="81fb7-288">**Umgebungsvariable**:`ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="81fb7-288">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="81fb7-289">Die Assembly anhand des Namens (`string`) oder (`TStartup`) verwiesen werden kann.</span><span class="sxs-lookup"><span data-stu-id="81fb7-289">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="81fb7-290">Wenn mehrere `UseStartup` Methoden aufgerufen werden, das letzte Lesezeichen hat Vorrang vor.</span><span class="sxs-lookup"><span data-stu-id="81fb7-290">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="81fb7-291">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="81fb7-291">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="81fb7-292">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="81fb7-292">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

### <a name="web-root"></a><span data-ttu-id="81fb7-293">Webstamm</span><span class="sxs-lookup"><span data-stu-id="81fb7-293">Web Root</span></span>

<span data-ttu-id="81fb7-294">Legt den relativen Pfad auf die app statische Objekte fest.</span><span class="sxs-lookup"><span data-stu-id="81fb7-294">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="81fb7-295">**Schlüssel**: Webroot-Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="81fb7-295">**Key**: webroot</span></span>  
<span data-ttu-id="81fb7-296">**Typ**: *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="81fb7-296">**Type**: *string*</span></span>  
<span data-ttu-id="81fb7-297">**Standardmäßige**: Wenn nicht angegeben, wird der Standardwert ist ""(Content Root)/wwwroot "", wenn der Pfad vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="81fb7-297">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="81fb7-298">Wenn der Pfad nicht vorhanden ist, wird ein ohne-Op-dateisystemanbieter verwendet.</span><span class="sxs-lookup"><span data-stu-id="81fb7-298">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="81fb7-299">**Legen Sie mithilfe von**:`UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="81fb7-299">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="81fb7-300">**Umgebungsvariable**:`ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="81fb7-300">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="81fb7-301">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="81fb7-301">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="81fb7-302">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="81fb7-302">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="81fb7-303">Überschreiben die Konfiguration</span><span class="sxs-lookup"><span data-stu-id="81fb7-303">Overriding configuration</span></span>

<span data-ttu-id="81fb7-304">Verwendung [Konfiguration](xref:fundamentals/configuration/index) auf den Host konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="81fb7-304">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="81fb7-305">Im folgenden Beispiel wird optional Hostkonfiguration angegeben ein *hosting.json* Datei.</span><span class="sxs-lookup"><span data-stu-id="81fb7-305">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="81fb7-306">Eine Konfiguration geladen wird, aus der *hosting.json* Datei überschrieben werden kann, durch die Befehlszeilenargumente.</span><span class="sxs-lookup"><span data-stu-id="81fb7-306">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="81fb7-307">Die Konfiguration erstellt (in `config`) wird verwendet, um den Host mit konfigurieren `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="81fb7-307">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="81fb7-308">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="81fb7-308">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="81fb7-309">*Hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="81fb7-309">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="81fb7-310">Überschreiben die Konfiguration von bereitgestellten `UseUrls` mit *hosting.json* Config erste Befehlszeilen Argument Config zweiten:</span><span class="sxs-lookup"><span data-stu-id="81fb7-310">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="81fb7-311">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="81fb7-311">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="81fb7-312">*Hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="81fb7-312">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="81fb7-313">Überschreiben die Konfiguration von bereitgestellten `UseUrls` mit *hosting.json* Config erste Befehlszeilen Argument Config zweiten:</span><span class="sxs-lookup"><span data-stu-id="81fb7-313">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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
> <span data-ttu-id="81fb7-314">Die [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) Erweiterungsmethode ist zurzeit der Analyse eines zurückgegebenen Konfigurationsabschnitts kann nicht `GetSection` (z. B. `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="81fb7-314">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="81fb7-315">Die `GetSection` Methode filtert die Schlüssel der Konfiguration im Abschnitt angefordert, sondern bleibt der Name des Abschnitts auf die Schlüssel (z. B. `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="81fb7-315">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="81fb7-316">Die `UseConfiguration` Methode erwartet die Schlüssel entsprechend dem `WebHostBuilder` Schlüssel (z. B. `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="81fb7-316">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="81fb7-317">Das Vorhandensein der Abschnittsname den Schlüsseln wird verhindert, dass Werte im Abschnitt konfigurieren den Host.</span><span class="sxs-lookup"><span data-stu-id="81fb7-317">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="81fb7-318">Dieses Problem wird in einer bevorstehenden Version behoben.</span><span class="sxs-lookup"><span data-stu-id="81fb7-318">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="81fb7-319">Weitere Informationen und problemumgehungen finden Sie unter [vollständige Schlüssel übergeben Konfigurationsabschnitt in WebHostBuilder.UseConfiguration verwendet](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="81fb7-319">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="81fb7-320">Um den Host aus, führen Sie auf eine bestimmte URL angeben, der gewünschte Wert übergeben werden kann über eine Eingabeaufforderung für die Ausführung `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="81fb7-320">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing `dotnet run`.</span></span> <span data-ttu-id="81fb7-321">Das Befehlszeilenargument überschreibt die `urls` Wert aus der *hosting.json* Datei und der Server lauscht an Port 8080:</span><span class="sxs-lookup"><span data-stu-id="81fb7-321">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="starting-the-host"></a><span data-ttu-id="81fb7-322">Der Host wird gestartet</span><span class="sxs-lookup"><span data-stu-id="81fb7-322">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="81fb7-323">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="81fb7-323">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="81fb7-324">**Run**</span><span class="sxs-lookup"><span data-stu-id="81fb7-324">**Run**</span></span>

<span data-ttu-id="81fb7-325">Die `Run` Methode startet die Web-app und blockiert den aufrufenden Thread, bis der Host heruntergefahren wird:</span><span class="sxs-lookup"><span data-stu-id="81fb7-325">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="81fb7-326">**Start**</span><span class="sxs-lookup"><span data-stu-id="81fb7-326">**Start**</span></span>

<span data-ttu-id="81fb7-327">Führen Sie den Host in eine nicht blockierende Weise durch Aufrufen seiner `Start` Methode:</span><span class="sxs-lookup"><span data-stu-id="81fb7-327">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="81fb7-328">Wenn eine Liste mit URLs zu übergeben, wird die `Start` -Methode, auf die angegebenen URLs überwacht:</span><span class="sxs-lookup"><span data-stu-id="81fb7-328">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="81fb7-329">Die app kann initialisieren und starten Sie einen neuen Host mit den Standardwerten vorkonfiguriert, dass `CreateDefaultBuilder` mithilfe einer statischen Hilfsmethode.</span><span class="sxs-lookup"><span data-stu-id="81fb7-329">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="81fb7-330">Diese Methoden starten Sie den Server ohne Konsolenausgabe und [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) warten Sie, bis eine Unterbrechung (STRG-C/SIGINT oder SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="81fb7-330">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="81fb7-331">**Start (RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="81fb7-331">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="81fb7-332">Beginnen Sie mit einem `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="81fb7-332">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="81fb7-333">Führen Sie im Browser, um eine Anforderung `http://localhost:5000` zum Empfangen der Antwort "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="81fb7-333">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="81fb7-334">`WaitForShutdown`blockiert, bis eine Unterbrechung (STRG-C/SIGINT oder SIGTERM) ausgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="81fb7-334">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="81fb7-335">Die app zeigt die `Console.WriteLine` Nachricht und wartet, bis eine Keypress zu beenden.</span><span class="sxs-lookup"><span data-stu-id="81fb7-335">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="81fb7-336">**Start (String Url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="81fb7-336">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="81fb7-337">Beginnen Sie mit einer URL und `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="81fb7-337">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="81fb7-338">Führt zum gleiche Ergebnis wie **starten (RequestDelegate app)**, außer die Anwendung reagiert auf `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="81fb7-338">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="81fb7-339">**Starten (Aktion<IRouteBuilder> RouteBuilder)**</span><span class="sxs-lookup"><span data-stu-id="81fb7-339">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="81fb7-340">Verwenden Sie eine Instanz des `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) routing Middleware verwenden:</span><span class="sxs-lookup"><span data-stu-id="81fb7-340">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="81fb7-341">Verwenden Sie die folgenden Browseranforderungen mit dem Beispiel:</span><span class="sxs-lookup"><span data-stu-id="81fb7-341">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="81fb7-342">Anforderung</span><span class="sxs-lookup"><span data-stu-id="81fb7-342">Request</span></span>                                    | <span data-ttu-id="81fb7-343">Antwort</span><span class="sxs-lookup"><span data-stu-id="81fb7-343">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="81fb7-344">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="81fb7-344">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="81fb7-345">Buenos Massenzahlungsverkehrssysteme, Catrina!</span><span class="sxs-lookup"><span data-stu-id="81fb7-345">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="81fb7-346">Löst eine Ausnahme mit der Zeichenfolge "Ooops!"</span><span class="sxs-lookup"><span data-stu-id="81fb7-346">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="81fb7-347">Löst eine Ausnahme mit der Zeichenfolge "Uh oh!"</span><span class="sxs-lookup"><span data-stu-id="81fb7-347">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="81fb7-348">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="81fb7-348">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="81fb7-349">Hello World!</span><span class="sxs-lookup"><span data-stu-id="81fb7-349">Hello World!</span></span>                             |

<span data-ttu-id="81fb7-350">`WaitForShutdown`blockiert, bis eine Unterbrechung (STRG-C/SIGINT oder SIGTERM) ausgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="81fb7-350">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="81fb7-351">Die app zeigt die `Console.WriteLine` Nachricht und wartet, bis eine Keypress zu beenden.</span><span class="sxs-lookup"><span data-stu-id="81fb7-351">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="81fb7-352">**Starten (string Url, Aktion<IRouteBuilder> RouteBuilder)**</span><span class="sxs-lookup"><span data-stu-id="81fb7-352">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="81fb7-353">Verwenden Sie eine URL und eine Instanz von `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="81fb7-353">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="81fb7-354">Führt zum gleiche Ergebnis wie **starten (Aktion<IRouteBuilder> RouteBuilder)**, außer die Anwendung reagiert auf `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="81fb7-354">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="81fb7-355">**StartWith (Aktion<IApplicationBuilder> app)**</span><span class="sxs-lookup"><span data-stu-id="81fb7-355">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="81fb7-356">Geben Sie ein Delegat zum Konfigurieren einer `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="81fb7-356">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="81fb7-357">Führen Sie im Browser, um eine Anforderung `http://localhost:5000` zum Empfangen der Antwort "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="81fb7-357">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="81fb7-358">`WaitForShutdown`blockiert, bis eine Unterbrechung (STRG-C/SIGINT oder SIGTERM) ausgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="81fb7-358">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="81fb7-359">Die app zeigt die `Console.WriteLine` Nachricht und wartet, bis eine Keypress zu beenden.</span><span class="sxs-lookup"><span data-stu-id="81fb7-359">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="81fb7-360">**StartWith (string Url, Aktion<IApplicationBuilder> app)**</span><span class="sxs-lookup"><span data-stu-id="81fb7-360">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="81fb7-361">Geben Sie eine URL und ein Delegat zum Konfigurieren einer `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="81fb7-361">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="81fb7-362">Führt zum gleiche Ergebnis wie **StartWith (Aktion<IApplicationBuilder> app)**, außer die Anwendung reagiert auf `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="81fb7-362">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="81fb7-363">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="81fb7-363">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="81fb7-364">**Run**</span><span class="sxs-lookup"><span data-stu-id="81fb7-364">**Run**</span></span>

<span data-ttu-id="81fb7-365">Die `Run` Methode startet die Web-app und blockiert den aufrufenden Thread, bis der Host heruntergefahren wird:</span><span class="sxs-lookup"><span data-stu-id="81fb7-365">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="81fb7-366">**Start**</span><span class="sxs-lookup"><span data-stu-id="81fb7-366">**Start**</span></span>

<span data-ttu-id="81fb7-367">Führen Sie den Host in eine nicht blockierende Weise durch Aufrufen seiner `Start` Methode:</span><span class="sxs-lookup"><span data-stu-id="81fb7-367">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="81fb7-368">Wenn eine Liste mit URLs zu übergeben, wird die `Start` -Methode, auf die angegebenen URLs überwacht:</span><span class="sxs-lookup"><span data-stu-id="81fb7-368">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>


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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="81fb7-369">IHostingEnvironment-Schnittstelle</span><span class="sxs-lookup"><span data-stu-id="81fb7-369">IHostingEnvironment interface</span></span>

<span data-ttu-id="81fb7-370">Die [IHostingEnvironment Schnittstelle](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) enthält Informationen zu den app-Web-hosting-Umgebung.</span><span class="sxs-lookup"><span data-stu-id="81fb7-370">The [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="81fb7-371">Verwenden Sie [Konstruktoreinfügung](xref:fundamentals/dependency-injection) zum Abrufen der `IHostingEnvironment` um die Eigenschaften und die Erweiterungsmethoden verwenden:</span><span class="sxs-lookup"><span data-stu-id="81fb7-371">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="81fb7-372">Ein [konventionsbasierte Ansatz](xref:fundamentals/environments#startup-conventions) können verwendet werden, um die Anwendung beim Start auf Grundlage der Umgebung zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="81fb7-372">A [convention-based approach](xref:fundamentals/environments#startup-conventions) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="81fb7-373">Fügen Sie alternativ die `IHostingEnvironment` in der `Startup` Konstruktor für die Verwendung in `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="81fb7-373">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="81fb7-374">Zusätzlich zu den `IsDevelopment` Erweiterungsmethode, `IHostingEnvironment` bietet `IsStaging`, `IsProduction`, und `IsEnvironment(string environmentName)` Methoden.</span><span class="sxs-lookup"><span data-stu-id="81fb7-374">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="81fb7-375">Finden Sie unter [arbeiten mit mehreren Umgebungen](xref:fundamentals/environments) Details.</span><span class="sxs-lookup"><span data-stu-id="81fb7-375">See [Working with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="81fb7-376">Die `IHostingEnvironment` Service kann auch direkt in injiziert die `Configure` Methode zum Einrichten der Verarbeitungspipeline:</span><span class="sxs-lookup"><span data-stu-id="81fb7-376">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="81fb7-377">`IHostingEnvironment`können eingegeben werden, in der `Invoke` Methode, wenn benutzerdefinierte [Middleware](xref:fundamentals/middleware#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="81fb7-377">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="81fb7-378">IApplicationLifetime-Schnittstelle</span><span class="sxs-lookup"><span data-stu-id="81fb7-378">IApplicationLifetime interface</span></span>

<span data-ttu-id="81fb7-379">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) nach dem Starten und Herunterfahren Aktivitäten ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="81fb7-379">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="81fb7-380">Drei Eigenschaften auf der Schnittstelle sind Abbruchtoken verwendet, um registrieren `Action` Methoden, die zum Starten und Herunterfahren Ereignisse zu definieren.</span><span class="sxs-lookup"><span data-stu-id="81fb7-380">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span> <span data-ttu-id="81fb7-381">Es gibt auch eine `StopApplication` Methode.</span><span class="sxs-lookup"><span data-stu-id="81fb7-381">There's also a `StopApplication` method.</span></span>

| <span data-ttu-id="81fb7-382">Abbruchtoken</span><span class="sxs-lookup"><span data-stu-id="81fb7-382">Cancellation Token</span></span>    | <span data-ttu-id="81fb7-383">Wird ausgelöst, wenn &#8230;</span><span class="sxs-lookup"><span data-stu-id="81fb7-383">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="81fb7-384">Der Host wurde vollständig gestartet.</span><span class="sxs-lookup"><span data-stu-id="81fb7-384">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="81fb7-385">Der Host führt ein ordnungsgemäßes Herunterfahren.</span><span class="sxs-lookup"><span data-stu-id="81fb7-385">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="81fb7-386">Anforderungen möglicherweise noch verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="81fb7-386">Requests may still be processing.</span></span> <span data-ttu-id="81fb7-387">Shutdown ist blockiert, bis diese abgeschlossen wird.</span><span class="sxs-lookup"><span data-stu-id="81fb7-387">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="81fb7-388">Der Host wird ein ordnungsgemäßes Herunterfahren abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="81fb7-388">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="81fb7-389">Alle Anforderungen verarbeitet werden soll.</span><span class="sxs-lookup"><span data-stu-id="81fb7-389">All requests should be processed.</span></span> <span data-ttu-id="81fb7-390">Shutdown ist blockiert, bis diese abgeschlossen wird.</span><span class="sxs-lookup"><span data-stu-id="81fb7-390">Shutdown blocks until this event completes.</span></span> |

| <span data-ttu-id="81fb7-391">Methode</span><span class="sxs-lookup"><span data-stu-id="81fb7-391">Method</span></span>            | <span data-ttu-id="81fb7-392">Aktion</span><span class="sxs-lookup"><span data-stu-id="81fb7-392">Action</span></span>                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | <span data-ttu-id="81fb7-393">Anforderungen Beendigung der aktuellen Anwendung.</span><span class="sxs-lookup"><span data-stu-id="81fb7-393">Requests termination of the current application.</span></span> |

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

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="81fb7-394">Problembehandlung bei System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="81fb7-394">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="81fb7-395">**Gilt für nur ASP.NET Core 2.0**</span><span class="sxs-lookup"><span data-stu-id="81fb7-395">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="81fb7-396">Wenn der Host von Räumen basiert `IStartup` direkt in die abhängigkeitseinschleusungscontainer anstelle von Aufrufen `UseStartup` oder `Configure`, der folgende Fehler auftreten: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.</span><span class="sxs-lookup"><span data-stu-id="81fb7-396">If the host is built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`, the following error may occur: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.</span></span>

<span data-ttu-id="81fb7-397">Dies geschieht, weil die [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (die aktuelle Assembly) ist erforderlich, um die Überprüfung `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="81fb7-397">This occurs because the [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="81fb7-398">Wenn die app manuell injiziert `IStartup` in der abhängigkeitseinschleusungscontainer hinzufügen den folgenden Aufruf `WebHostBuilder` mit dem Assemblynamen angegeben:</span><span class="sxs-lookup"><span data-stu-id="81fb7-398">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="81fb7-399">Fügen Sie alternativ eine Dummy `Configure` auf die `WebHostBuilder`, welche die `applicationName`(`ApplicationKey`) automatisch:</span><span class="sxs-lookup"><span data-stu-id="81fb7-399">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="81fb7-400">**Hinweis**: Dies ist nur erforderlich, mit der Version 2.0 von ASP.NET Core und nur wenn die app Aufrufen nicht `UseStartup` oder `Configure`.</span><span class="sxs-lookup"><span data-stu-id="81fb7-400">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="81fb7-401">Weitere Informationen finden Sie unter [Ankündigungen: Microsoft.Extensions.PlatformAbstractions wurde entfernt / / (Kommentar)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) und [StartupInjection Beispiel](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="81fb7-401">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="81fb7-402">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="81fb7-402">Additional resources</span></span>

* [<span data-ttu-id="81fb7-403">Veröffentlichen Sie in Windows unter Verwendung von IIS</span><span class="sxs-lookup"><span data-stu-id="81fb7-403">Publish to Windows using IIS</span></span>](../publishing/iis.md)
* [<span data-ttu-id="81fb7-404">Veröffentlichen Sie in Linux mit Nginx</span><span class="sxs-lookup"><span data-stu-id="81fb7-404">Publish to Linux using Nginx</span></span>](../publishing/linuxproduction.md)
* [<span data-ttu-id="81fb7-405">Veröffentlichen Sie in Linux mit Apache</span><span class="sxs-lookup"><span data-stu-id="81fb7-405">Publish to Linux using Apache</span></span>](../publishing/apache-proxy.md)
* [<span data-ttu-id="81fb7-406">Host in einem Windowsdienst</span><span class="sxs-lookup"><span data-stu-id="81fb7-406">Host in a Windows Service</span></span>](xref:hosting/windows-service)
