---
title: Hosten in ASP.NET Core
author: guardrex
description: "Informationen Sie zu den Webhost in ASP.NET Core, die für die app starten und die Verwaltung zuständig ist."
ms.author: riande
manager: wpickett
ms.date: 09/21/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.openlocfilehash: 7f6712073002b73ca4ddd7586718c81e62cacbc2
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="61950-103">Hosten in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="61950-103">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="61950-104">Von [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="61950-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="61950-105">ASP.NET Core apps konfigurieren und starten Sie eine *Host*.</span><span class="sxs-lookup"><span data-stu-id="61950-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="61950-106">Der Host ist verantwortlich für die Verwaltung von Apps starten sowie seine Lebensdauer.</span><span class="sxs-lookup"><span data-stu-id="61950-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="61950-107">Mindestens wird der Host einem Server und einem Anforderungsverarbeitungspipeline konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="61950-107">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="61950-108">Einrichten eines Hosts</span><span class="sxs-lookup"><span data-stu-id="61950-108">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="61950-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="61950-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="61950-110">Erstellen Sie einen Host mit einer Instanz von [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="61950-110">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="61950-111">Dies erfolgt in der Regel am Einstiegspunkt der Anwendung, die `Main` Methode.</span><span class="sxs-lookup"><span data-stu-id="61950-111">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="61950-112">In den Vorlagen für Projekte `Main` befindet sich im *"Program.cs"*.</span><span class="sxs-lookup"><span data-stu-id="61950-112">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="61950-113">Eine typische *"Program.cs"* Aufrufe [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) zum Einrichten eines Hosts zu starten:</span><span class="sxs-lookup"><span data-stu-id="61950-113">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="61950-114">`CreateDefaultBuilder`führt die folgenden Aufgaben:</span><span class="sxs-lookup"><span data-stu-id="61950-114">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="61950-115">Konfiguriert die [Kestrel](servers/kestrel.md) wie der Webserver.</span><span class="sxs-lookup"><span data-stu-id="61950-115">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="61950-116">Die Standardoptionen Kestrel finden Sie unter [der Kestrel Optionen im Abschnitt Kestrel webserverimplementierung in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="61950-116">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="61950-117">Legt die Inhalten-Stamm auf den Pfad zurückgegebenes [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="61950-117">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="61950-118">Lädt die optionale Konfiguration aus:</span><span class="sxs-lookup"><span data-stu-id="61950-118">Loads optional configuration from:</span></span>
  * <span data-ttu-id="61950-119">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="61950-119">*appsettings.json*.</span></span>
  * <span data-ttu-id="61950-120">*appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="61950-120">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="61950-121">[Vertrauliche Benutzerdaten](xref:security/app-secrets) Wenn die app ausgeführt wird, der `Development` Umgebung.</span><span class="sxs-lookup"><span data-stu-id="61950-121">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="61950-122">Umgebungsvariablen.</span><span class="sxs-lookup"><span data-stu-id="61950-122">Environment variables.</span></span>
  * <span data-ttu-id="61950-123">Befehlszeilenargumente.</span><span class="sxs-lookup"><span data-stu-id="61950-123">Command-line arguments.</span></span>
* <span data-ttu-id="61950-124">Konfiguriert die [Protokollierung](xref:fundamentals/logging/index) für Konsole und Debug-Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="61950-124">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="61950-125">Protokollierung umfasst [Protokoll filtern](xref:fundamentals/logging/index#log-filtering) Regeln in einem Konfigurationsabschnitt Protokollierung ein *appsettings.json* oder *"appSettings". {} Umgebung} JSON* Datei.</span><span class="sxs-lookup"><span data-stu-id="61950-125">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="61950-126">Wenn hinter dem IIS ausgeführt wird, ermöglicht [IIS Integration](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="61950-126">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="61950-127">Konfiguriert den Basispfad und Port der Server lauscht bei Verwendung der [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="61950-127">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="61950-128">Das Modul erstellt einen Reverseproxy zwischen IIS und Kestrel.</span><span class="sxs-lookup"><span data-stu-id="61950-128">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="61950-129">Außerdem konfiguriert die app, [Erfassen von Startfehlern](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="61950-129">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="61950-130">Die Standardoptionen von IIS finden Sie unter [der IIS-Optionen im Abschnitt Host ASP.NET Core unter Windows mit IIS](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="61950-130">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="61950-131">Die *Content Stamm* bestimmt, an denen der Host für Inhaltsdateien, z. B. MVC-Ansicht-Dateien sucht.</span><span class="sxs-lookup"><span data-stu-id="61950-131">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="61950-132">Wenn die app aus dem Stammordner des Projekts gestartet wird, wird als Inhalt Stamm Stammordner des Projekts verwendet.</span><span class="sxs-lookup"><span data-stu-id="61950-132">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="61950-133">Dies ist die Standardeinstellung verwendet [Visual Studio](https://www.visualstudio.com/) und [Dotnet neue Vorlagen](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="61950-133">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="61950-134">Weitere Informationen zur app-Konfiguration finden Sie unter [Konfiguration in ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="61950-134">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="61950-135">Als Alternative zur Verwendung der statischen `CreateDefaultBuilder` -Methode, erstellen einen Host aus [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) ist ein unterstützten Ansatz mit ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="61950-135">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="61950-136">Weitere Informationen finden Sie unter der Registerkarte "ASP.NET Core 1.x".</span><span class="sxs-lookup"><span data-stu-id="61950-136">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="61950-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="61950-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="61950-138">Erstellen Sie einen Host mit einer Instanz von [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="61950-138">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="61950-139">Erstellen einen Host erfolgt in der Regel am Einstiegspunkt der Anwendung, die `Main` Methode.</span><span class="sxs-lookup"><span data-stu-id="61950-139">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="61950-140">In den Vorlagen für Projekte `Main` befindet sich im *"Program.cs"*:</span><span class="sxs-lookup"><span data-stu-id="61950-140">In the project templates, `Main` is located in *Program.cs*:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="61950-141">`WebHostBuilder`erfordert eine [Server, die Iserver-c# implementiert](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="61950-141">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="61950-142">Die integrierten Server sind [Kestrel](servers/kestrel.md) und [HTTP.sys](servers/httpsys.md) (vor der Veröffentlichung von ASP.NET Core 2.0, HTTP.sys hieß [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="61950-142">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="61950-143">In diesem Beispiel wird die [UseKestrel Erweiterungsmethode](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) gibt den Kestrel-Server.</span><span class="sxs-lookup"><span data-stu-id="61950-143">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="61950-144">Die *Content Stamm* bestimmt, an denen der Host für Inhaltsdateien, z. B. MVC-Ansicht-Dateien sucht.</span><span class="sxs-lookup"><span data-stu-id="61950-144">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="61950-145">Der standardmäßige Content Stamm für abgerufen wird `UseContentRoot` von [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="61950-145">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="61950-146">Wenn die app aus dem Stammordner des Projekts gestartet wird, wird als Inhalt Stamm Stammordner des Projekts verwendet.</span><span class="sxs-lookup"><span data-stu-id="61950-146">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="61950-147">Dies ist die Standardeinstellung verwendet [Visual Studio](https://www.visualstudio.com/) und [Dotnet neue Vorlagen](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="61950-147">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="61950-148">Rufen Sie zum Verwenden von IIS als Reverseproxy [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) im Rahmen der Erstellung des Hosts.</span><span class="sxs-lookup"><span data-stu-id="61950-148">To use IIS as a reverse proxy, call [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="61950-149">`UseIISIntegration`Konfigurieren Sie keine *Server*, z. B. [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) verfügt.</span><span class="sxs-lookup"><span data-stu-id="61950-149">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="61950-150">`UseIISIntegration`konfiguriert den Basispfad und Port der Server lauscht bei Verwendung der [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) Reverseproxy zwischen Kestrel und IIS zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="61950-150">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="61950-151">Für die Verwendung von IIS mit ASP.NET Core, `UseKestrel` und `UseIISIntegration` muss angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="61950-151">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="61950-152">`UseIISIntegration`nur aktiviert, wenn hinter der IIS- oder IIS Express ausführen.</span><span class="sxs-lookup"><span data-stu-id="61950-152">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="61950-153">Weitere Informationen finden Sie unter [Einführung in ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) und [ASP.NET Core-Modul Konfigurationsverweis](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="61950-153">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="61950-154">Eine minimale Implementierung, die einen Host (und ASP.NET Core-app) konfiguriert, enthält einen Server und die Konfiguration der Anforderungspipeline die app angeben:</span><span class="sxs-lookup"><span data-stu-id="61950-154">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="61950-155">Beim Einrichten von eines Hosts [konfigurieren](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) und [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) Methoden bereitgestellt werden können.</span><span class="sxs-lookup"><span data-stu-id="61950-155">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="61950-156">Wenn eine `Startup` Klasse angegeben wird, muss er definieren eine `Configure` Methode.</span><span class="sxs-lookup"><span data-stu-id="61950-156">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="61950-157">Weitere Informationen finden Sie unter [Start der Anwendung in ASP.NET Core](startup.md).</span><span class="sxs-lookup"><span data-stu-id="61950-157">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="61950-158">Mehrere Aufrufe `ConfigureServices` untereinander anfügen.</span><span class="sxs-lookup"><span data-stu-id="61950-158">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="61950-159">Mehrere Aufrufe `Configure` oder `UseStartup` auf die `WebHostBuilder` ersetzen die vorherige Einstellungen.</span><span class="sxs-lookup"><span data-stu-id="61950-159">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="61950-160">Host-Konfigurationswerte</span><span class="sxs-lookup"><span data-stu-id="61950-160">Host configuration values</span></span>

<span data-ttu-id="61950-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) basiert auf der folgenden Ansätze, um die Konfigurationswerte für den Host festgelegt:</span><span class="sxs-lookup"><span data-stu-id="61950-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="61950-162">Host-Generator-Konfiguration, die Umgebungsvariablen mit dem Format enthält `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="61950-162">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="61950-163">Beispielsweise `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="61950-163">For example, `ASPNETCORE_URLS`.</span></span>
* <span data-ttu-id="61950-164">Explizite Methoden wie z. B. `CaptureStartupErrors`.</span><span class="sxs-lookup"><span data-stu-id="61950-164">Explicit methods, such as `CaptureStartupErrors`.</span></span>
* <span data-ttu-id="61950-165">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) und den zugehörigen Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="61950-165">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="61950-166">Beim Festlegen eines Werts mit `UseSetting`, der Wert wird als Zeichenfolge unabhängig vom Typ festgelegt.</span><span class="sxs-lookup"><span data-stu-id="61950-166">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="61950-167">Vom Host verwendet wird, unabhängig davon, welche Option zuletzt legt einen Wert fest.</span><span class="sxs-lookup"><span data-stu-id="61950-167">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="61950-168">Weitere Informationen finden Sie unter [überschreiben Konfiguration](#overriding-configuration) im nächsten Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="61950-168">For more information, see [Overriding configuration](#overriding-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="61950-169">Erfassen von Startfehlern</span><span class="sxs-lookup"><span data-stu-id="61950-169">Capture Startup Errors</span></span>

<span data-ttu-id="61950-170">Diese Einstellung steuert die Erfassung von Startfehlern.</span><span class="sxs-lookup"><span data-stu-id="61950-170">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="61950-171">**Schlüssel**: CaptureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="61950-171">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="61950-172">**Typ**: *Bool* (`true` oder `1`)</span><span class="sxs-lookup"><span data-stu-id="61950-172">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="61950-173">**Standardmäßige**: standardmäßig `false` , wenn die app mit Kestrel hinter dem IIS ausgeführt wird, in dem die Standardeinstellung ist `true`.</span><span class="sxs-lookup"><span data-stu-id="61950-173">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="61950-174">**Legen Sie mithilfe von**:`CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="61950-174">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="61950-175">**Umgebungsvariable**:`ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="61950-175">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="61950-176">Wenn `false`, Fehler während des Starts Ergebnis auf dem Host beendet.</span><span class="sxs-lookup"><span data-stu-id="61950-176">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="61950-177">Wenn `true`, der Host erfasst Ausnahmen während des Starts und versucht, den Server zu starten.</span><span class="sxs-lookup"><span data-stu-id="61950-177">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="61950-178">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="61950-178">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="61950-179">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="61950-179">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="61950-180">Inhalts-Stamm</span><span class="sxs-lookup"><span data-stu-id="61950-180">Content Root</span></span>

<span data-ttu-id="61950-181">Diese Einstellung bestimmt, in dem ASP.NET Core beginnt die Suche nach Inhaltsdateien gespeichert, z. B. MVC-Ansichten.</span><span class="sxs-lookup"><span data-stu-id="61950-181">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="61950-182">**Schlüssel**: ContentRoot</span><span class="sxs-lookup"><span data-stu-id="61950-182">**Key**: contentRoot</span></span>  
<span data-ttu-id="61950-183">**Typ**: *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="61950-183">**Type**: *string*</span></span>  
<span data-ttu-id="61950-184">**Standard**: standardmäßig auf den Ordner, in dem die app-Assembly gespeichert ist.</span><span class="sxs-lookup"><span data-stu-id="61950-184">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="61950-185">**Legen Sie mithilfe von**:`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="61950-185">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="61950-186">**Umgebungsvariable**:`ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="61950-186">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="61950-187">Die Inhalte-Stamm dient außerdem als den Basispfad für den [Stammweb-Einstellung](#web-root).</span><span class="sxs-lookup"><span data-stu-id="61950-187">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="61950-188">Wenn der Pfad nicht vorhanden ist, kann der Host nicht gestartet.</span><span class="sxs-lookup"><span data-stu-id="61950-188">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="61950-189">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="61950-189">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="61950-190">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="61950-190">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="61950-191">Ausführliche Fehler</span><span class="sxs-lookup"><span data-stu-id="61950-191">Detailed Errors</span></span>

<span data-ttu-id="61950-192">Bestimmt, ob ausführliche Fehler erfasst werden sollen.</span><span class="sxs-lookup"><span data-stu-id="61950-192">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="61950-193">**Schlüssel**: DetailedErrors</span><span class="sxs-lookup"><span data-stu-id="61950-193">**Key**: detailedErrors</span></span>  
<span data-ttu-id="61950-194">**Typ**: *Bool* (`true` oder `1`)</span><span class="sxs-lookup"><span data-stu-id="61950-194">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="61950-195">**Standard**: "false"</span><span class="sxs-lookup"><span data-stu-id="61950-195">**Default**: false</span></span>  
<span data-ttu-id="61950-196">**Legen Sie mithilfe von**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="61950-196">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="61950-197">**Umgebungsvariable**:`ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="61950-197">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="61950-198">Wenn aktiviert (oder wenn die <a href="#environment">Umgebung</a> auf festgelegt ist `Development`), die app erfasst ausführliche Ausnahmen.</span><span class="sxs-lookup"><span data-stu-id="61950-198">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="61950-199">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="61950-199">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="61950-200">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="61950-200">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="61950-201">Umgebung</span><span class="sxs-lookup"><span data-stu-id="61950-201">Environment</span></span>

<span data-ttu-id="61950-202">Legt fest, die app-Umgebung.</span><span class="sxs-lookup"><span data-stu-id="61950-202">Sets the app's environment.</span></span>

<span data-ttu-id="61950-203">**Schlüssel**: Umgebung</span><span class="sxs-lookup"><span data-stu-id="61950-203">**Key**: environment</span></span>  
<span data-ttu-id="61950-204">**Typ**: *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="61950-204">**Type**: *string*</span></span>  
<span data-ttu-id="61950-205">**Standard**: Produktion</span><span class="sxs-lookup"><span data-stu-id="61950-205">**Default**: Production</span></span>  
<span data-ttu-id="61950-206">**Legen Sie mithilfe von**:`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="61950-206">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="61950-207">**Umgebungsvariable**:`ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="61950-207">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="61950-208">Die Umgebung kann auf einen beliebigen Wert festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="61950-208">The environment can be set to any value.</span></span> <span data-ttu-id="61950-209">Framework definierte Werte sind `Development`, `Staging`, und `Production`.</span><span class="sxs-lookup"><span data-stu-id="61950-209">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="61950-210">Werte sind keine Groß-/Kleinschreibung beachtet.</span><span class="sxs-lookup"><span data-stu-id="61950-210">Values aren't case sensitive.</span></span> <span data-ttu-id="61950-211">Wird standardmäßig die *Umgebung* auslesen ist die `ASPNETCORE_ENVIRONMENT` -Umgebungsvariablen angegeben.</span><span class="sxs-lookup"><span data-stu-id="61950-211">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="61950-212">Bei Verwendung [Visual Studio](https://www.visualstudio.com/), Umgebungsvariablen festgelegt werden können, der *launchSettings.json* Datei.</span><span class="sxs-lookup"><span data-stu-id="61950-212">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="61950-213">Weitere Informationen finden Sie unter [Working with Multiple Environments (Verwenden von mehreren Umgebungen)](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="61950-213">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="61950-214">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="61950-214">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="61950-215">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="61950-215">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="61950-216">Hosten Startassemblys</span><span class="sxs-lookup"><span data-stu-id="61950-216">Hosting Startup Assemblies</span></span>

<span data-ttu-id="61950-217">Legt die app hosten Startassemblys fest.</span><span class="sxs-lookup"><span data-stu-id="61950-217">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="61950-218">**Schlüssel**: HostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="61950-218">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="61950-219">**Typ**: *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="61950-219">**Type**: *string*</span></span>  
<span data-ttu-id="61950-220">**Standard**: leere Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="61950-220">**Default**: Empty string</span></span>  
<span data-ttu-id="61950-221">**Legen Sie mithilfe von**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="61950-221">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="61950-222">**Umgebungsvariable**:`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="61950-222">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="61950-223">Eine durch Semikolons getrennte Zeichenfolge hosten Startassemblys beim Start geladen werden soll.</span><span class="sxs-lookup"><span data-stu-id="61950-223">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="61950-224">Dieses Feature ist neu in ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="61950-224">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="61950-225">Obwohl die Konfiguration auf eine leere Zeichenfolge wird als Standardwert, enthalten hosting Autostart-Assemblys immer der app-Assembly.</span><span class="sxs-lookup"><span data-stu-id="61950-225">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="61950-226">Beim hosting Startassemblys bereitgestellt werden, werden diese hinzugefügt auf die app-Assembly für das Laden, wenn die app während des Starts der gemeinsamen Dienste erstellt.</span><span class="sxs-lookup"><span data-stu-id="61950-226">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="61950-227">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="61950-227">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="61950-228">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="61950-228">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="61950-229">Diese Funktion steht in ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="61950-229">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="61950-230">Bevorzugen Sie Hosten von URLs</span><span class="sxs-lookup"><span data-stu-id="61950-230">Prefer Hosting URLs</span></span>

<span data-ttu-id="61950-231">Gibt an, ob der Host werden, auf die URLs abgehört soll mit konfiguriert die `WebHostBuilder` nicht die dafür konfigurierten mit der `IServer` Implementierung.</span><span class="sxs-lookup"><span data-stu-id="61950-231">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="61950-232">**Schlüssel**: PreferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="61950-232">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="61950-233">**Typ**: *Bool* (`true` oder `1`)</span><span class="sxs-lookup"><span data-stu-id="61950-233">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="61950-234">**Standard**: "true"</span><span class="sxs-lookup"><span data-stu-id="61950-234">**Default**: true</span></span>  
<span data-ttu-id="61950-235">**Legen Sie mithilfe von**:`PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="61950-235">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="61950-236">**Umgebungsvariable**:`ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="61950-236">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="61950-237">Dieses Feature ist neu in ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="61950-237">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="61950-238">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="61950-238">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="61950-239">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="61950-239">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="61950-240">Diese Funktion steht in ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="61950-240">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="61950-241">Zu verhindern, dass Hosting starten</span><span class="sxs-lookup"><span data-stu-id="61950-241">Prevent Hosting Startup</span></span>

<span data-ttu-id="61950-242">Verhindert, dass das automatische Laden von hosting Startassemblys, einschließlich hosting Autostart-Assemblys, die von der app-Assembly konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="61950-242">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="61950-243">Finden Sie unter [Hinzufügen von app-Funktionen aus einer externen Assembly IHostingStartup](xref:host-and-deploy/ihostingstartup) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="61950-243">See [Add app features from an external assembly using IHostingStartup](xref:host-and-deploy/ihostingstartup) for more information.</span></span>

<span data-ttu-id="61950-244">**Schlüssel**: PreventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="61950-244">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="61950-245">**Typ**: *Bool* (`true` oder `1`)</span><span class="sxs-lookup"><span data-stu-id="61950-245">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="61950-246">**Standard**: "false"</span><span class="sxs-lookup"><span data-stu-id="61950-246">**Default**: false</span></span>  
<span data-ttu-id="61950-247">**Legen Sie mithilfe von**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="61950-247">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="61950-248">**Umgebungsvariable**:`ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="61950-248">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="61950-249">Dieses Feature ist neu in ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="61950-249">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="61950-250">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="61950-250">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="61950-251">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="61950-251">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="61950-252">Diese Funktion steht in ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="61950-252">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="61950-253">Server-URLs</span><span class="sxs-lookup"><span data-stu-id="61950-253">Server URLs</span></span>

<span data-ttu-id="61950-254">Gibt die IP-Adressen oder Adressen mit Ports und Protokolle, die der Server auf Anforderungen Lauschen soll.</span><span class="sxs-lookup"><span data-stu-id="61950-254">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="61950-255">**Schlüssel**: Urls</span><span class="sxs-lookup"><span data-stu-id="61950-255">**Key**: urls</span></span>  
<span data-ttu-id="61950-256">**Typ**: *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="61950-256">**Type**: *string*</span></span>  
<span data-ttu-id="61950-257">**Standard**: http://localhost: 5000</span><span class="sxs-lookup"><span data-stu-id="61950-257">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="61950-258">**Legen Sie mithilfe von**:`UseUrls`</span><span class="sxs-lookup"><span data-stu-id="61950-258">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="61950-259">**Umgebungsvariable**:`ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="61950-259">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="61950-260">Legen Sie auf eine durch Semikolon getrennt (;) Liste von URL-Präfixen, die auf der Server reagieren soll.</span><span class="sxs-lookup"><span data-stu-id="61950-260">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="61950-261">Beispielsweise `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="61950-261">For example, `http://localhost:123`.</span></span> <span data-ttu-id="61950-262">Mit "\*", um anzugeben, dass der Server nach Anforderungen für alle IP-Adresse oder Hostname mit dem angegebenen Port und Protokoll abgehört werden soll (z. B. `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="61950-262">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="61950-263">Das Protokoll (`http://` oder `https://`) für jede URL eingeschlossen werden müssen.</span><span class="sxs-lookup"><span data-stu-id="61950-263">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="61950-264">Zu den unterstützten Bildformaten variieren zwischen Servern.</span><span class="sxs-lookup"><span data-stu-id="61950-264">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="61950-265">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="61950-265">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="61950-266">Kestrel verfügt über einen eigenen Endpunkt-Konfigurations-API.</span><span class="sxs-lookup"><span data-stu-id="61950-266">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="61950-267">Weitere Informationen finden Sie unter [Kestrel web server implementation in ASP.NET Core (Kestrel-Webserverimplementierung in ASP.NET Core)](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="61950-267">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="61950-268">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="61950-268">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="61950-269">Timeout beim Herunterfahren</span><span class="sxs-lookup"><span data-stu-id="61950-269">Shutdown Timeout</span></span>

<span data-ttu-id="61950-270">Gibt den Umfang der Wartezeit für den Webhost zum Herunterfahren.</span><span class="sxs-lookup"><span data-stu-id="61950-270">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="61950-271">**Schlüssel**: ShutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="61950-271">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="61950-272">**Typ**: *Int*</span><span class="sxs-lookup"><span data-stu-id="61950-272">**Type**: *int*</span></span>  
<span data-ttu-id="61950-273">**Standard**: 5</span><span class="sxs-lookup"><span data-stu-id="61950-273">**Default**: 5</span></span>  
<span data-ttu-id="61950-274">**Legen Sie mithilfe von**:`UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="61950-274">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="61950-275">**Umgebungsvariable**:`ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="61950-275">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="61950-276">Obwohl der Schlüssel akzeptiert ein *Int* mit `UseSetting` (z. B. `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), die `UseShutdownTimeout` Erweiterungsmethode akzeptiert eine `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="61950-276">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the `UseShutdownTimeout` extension method takes a `TimeSpan`.</span></span> <span data-ttu-id="61950-277">Dieses Feature ist neu in ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="61950-277">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="61950-278">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="61950-278">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="61950-279">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="61950-279">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="61950-280">Diese Funktion steht in ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="61950-280">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="61950-281">Startassembly</span><span class="sxs-lookup"><span data-stu-id="61950-281">Startup Assembly</span></span>

<span data-ttu-id="61950-282">Die Assembly zu suchende bestimmt die `Startup` Klasse.</span><span class="sxs-lookup"><span data-stu-id="61950-282">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="61950-283">**Schlüssel**: StartupAssembly</span><span class="sxs-lookup"><span data-stu-id="61950-283">**Key**: startupAssembly</span></span>  
<span data-ttu-id="61950-284">**Typ**: *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="61950-284">**Type**: *string*</span></span>  
<span data-ttu-id="61950-285">**Standard**: die app-Assembly</span><span class="sxs-lookup"><span data-stu-id="61950-285">**Default**: The app's assembly</span></span>  
<span data-ttu-id="61950-286">**Legen Sie mithilfe von**:`UseStartup`</span><span class="sxs-lookup"><span data-stu-id="61950-286">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="61950-287">**Umgebungsvariable**:`ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="61950-287">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="61950-288">Die Assembly anhand des Namens (`string`) oder (`TStartup`) verwiesen werden kann.</span><span class="sxs-lookup"><span data-stu-id="61950-288">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="61950-289">Wenn mehrere `UseStartup` Methoden aufgerufen werden, das letzte Lesezeichen hat Vorrang vor.</span><span class="sxs-lookup"><span data-stu-id="61950-289">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="61950-290">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="61950-290">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="61950-291">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="61950-291">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

### <a name="web-root"></a><span data-ttu-id="61950-292">Webstamm</span><span class="sxs-lookup"><span data-stu-id="61950-292">Web Root</span></span>

<span data-ttu-id="61950-293">Legt den relativen Pfad auf die app statische Objekte fest.</span><span class="sxs-lookup"><span data-stu-id="61950-293">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="61950-294">**Schlüssel**: Webroot-Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="61950-294">**Key**: webroot</span></span>  
<span data-ttu-id="61950-295">**Typ**: *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="61950-295">**Type**: *string*</span></span>  
<span data-ttu-id="61950-296">**Standardmäßige**: Wenn nicht angegeben, wird der Standardwert ist ""(Content Root)/wwwroot "", wenn der Pfad vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="61950-296">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="61950-297">Wenn der Pfad nicht vorhanden ist, wird ein ohne-Op-dateisystemanbieter verwendet.</span><span class="sxs-lookup"><span data-stu-id="61950-297">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="61950-298">**Legen Sie mithilfe von**:`UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="61950-298">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="61950-299">**Umgebungsvariable**:`ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="61950-299">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="61950-300">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="61950-300">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="61950-301">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="61950-301">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="61950-302">Überschreiben die Konfiguration</span><span class="sxs-lookup"><span data-stu-id="61950-302">Overriding configuration</span></span>

<span data-ttu-id="61950-303">Verwendung [Konfiguration](xref:fundamentals/configuration/index) auf den Host konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="61950-303">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="61950-304">Im folgenden Beispiel wird optional Hostkonfiguration angegeben ein *hosting.json* Datei.</span><span class="sxs-lookup"><span data-stu-id="61950-304">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="61950-305">Eine Konfiguration geladen wird, aus der *hosting.json* Datei überschrieben werden kann, durch die Befehlszeilenargumente.</span><span class="sxs-lookup"><span data-stu-id="61950-305">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="61950-306">Die Konfiguration erstellt (in `config`) wird verwendet, um den Host mit konfigurieren `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="61950-306">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="61950-307">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="61950-307">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="61950-308">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="61950-308">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="61950-309">Überschreiben die Konfiguration von bereitgestellten `UseUrls` mit *hosting.json* Config erste Befehlszeilen Argument Config zweiten:</span><span class="sxs-lookup"><span data-stu-id="61950-309">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="61950-310">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="61950-310">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="61950-311">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="61950-311">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="61950-312">Überschreiben die Konfiguration von bereitgestellten `UseUrls` mit *hosting.json* Config erste Befehlszeilen Argument Config zweiten:</span><span class="sxs-lookup"><span data-stu-id="61950-312">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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
> <span data-ttu-id="61950-313">Die [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) Erweiterungsmethode ist zurzeit der Analyse eines zurückgegebenen Konfigurationsabschnitts kann nicht `GetSection` (z. B. `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="61950-313">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="61950-314">Die `GetSection` Methode filtert die Schlüssel der Konfiguration im Abschnitt angefordert, sondern bleibt der Name des Abschnitts auf die Schlüssel (z. B. `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="61950-314">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="61950-315">Die `UseConfiguration` Methode erwartet die Schlüssel entsprechend dem `WebHostBuilder` Schlüssel (z. B. `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="61950-315">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="61950-316">Das Vorhandensein der Abschnittsname den Schlüsseln wird verhindert, dass Werte im Abschnitt konfigurieren den Host.</span><span class="sxs-lookup"><span data-stu-id="61950-316">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="61950-317">Dieses Problem wird in einer bevorstehenden Version behoben.</span><span class="sxs-lookup"><span data-stu-id="61950-317">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="61950-318">Weitere Informationen und problemumgehungen finden Sie unter [vollständige Schlüssel übergeben Konfigurationsabschnitt in WebHostBuilder.UseConfiguration verwendet](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="61950-318">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="61950-319">Um den Host aus, führen Sie auf eine bestimmte URL angeben, der gewünschte Wert übergeben werden kann über eine Eingabeaufforderung für die Ausführung `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="61950-319">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing `dotnet run`.</span></span> <span data-ttu-id="61950-320">Das Befehlszeilenargument überschreibt die `urls` Wert aus der *hosting.json* Datei und der Server lauscht an Port 8080:</span><span class="sxs-lookup"><span data-stu-id="61950-320">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="starting-the-host"></a><span data-ttu-id="61950-321">Der Host wird gestartet</span><span class="sxs-lookup"><span data-stu-id="61950-321">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="61950-322">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="61950-322">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="61950-323">**Run**</span><span class="sxs-lookup"><span data-stu-id="61950-323">**Run**</span></span>

<span data-ttu-id="61950-324">Die `Run` Methode startet die Web-app und blockiert den aufrufenden Thread, bis der Host heruntergefahren wird:</span><span class="sxs-lookup"><span data-stu-id="61950-324">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="61950-325">**Start**</span><span class="sxs-lookup"><span data-stu-id="61950-325">**Start**</span></span>

<span data-ttu-id="61950-326">Führen Sie den Host in eine nicht blockierende Weise durch Aufrufen seiner `Start` Methode:</span><span class="sxs-lookup"><span data-stu-id="61950-326">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="61950-327">Wenn eine Liste mit URLs zu übergeben, wird die `Start` -Methode, auf die angegebenen URLs überwacht:</span><span class="sxs-lookup"><span data-stu-id="61950-327">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="61950-328">Die app kann initialisieren und starten Sie einen neuen Host mit den Standardwerten vorkonfiguriert, dass `CreateDefaultBuilder` mithilfe einer statischen Hilfsmethode.</span><span class="sxs-lookup"><span data-stu-id="61950-328">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="61950-329">Diese Methoden starten Sie den Server ohne Konsolenausgabe und [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) warten Sie, bis eine Unterbrechung (STRG-C/SIGINT oder SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="61950-329">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="61950-330">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="61950-330">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="61950-331">Beginnen Sie mit einem `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="61950-331">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="61950-332">Führen Sie im Browser, um eine Anforderung `http://localhost:5000` zum Empfangen der Antwort "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="61950-332">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="61950-333">`WaitForShutdown`blockiert, bis eine Unterbrechung (STRG-C/SIGINT oder SIGTERM) ausgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="61950-333">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="61950-334">Die app zeigt die `Console.WriteLine` Nachricht und wartet, bis eine Keypress zu beenden.</span><span class="sxs-lookup"><span data-stu-id="61950-334">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="61950-335">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="61950-335">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="61950-336">Beginnen Sie mit einer URL und `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="61950-336">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="61950-337">Führt zum gleiche Ergebnis wie **starten (RequestDelegate app)**, außer die Anwendung reagiert auf `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="61950-337">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="61950-338">**Starten (Aktion<IRouteBuilder> RouteBuilder)**</span><span class="sxs-lookup"><span data-stu-id="61950-338">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="61950-339">Verwenden Sie eine Instanz des `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) routing Middleware verwenden:</span><span class="sxs-lookup"><span data-stu-id="61950-339">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="61950-340">Verwenden Sie die folgenden Browseranforderungen mit dem Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61950-340">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="61950-341">Anforderung</span><span class="sxs-lookup"><span data-stu-id="61950-341">Request</span></span>                                    | <span data-ttu-id="61950-342">Antwort</span><span class="sxs-lookup"><span data-stu-id="61950-342">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="61950-343">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="61950-343">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="61950-344">Buenos Massenzahlungsverkehrssysteme, Catrina!</span><span class="sxs-lookup"><span data-stu-id="61950-344">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="61950-345">Löst eine Ausnahme mit der Zeichenfolge "Ooops!"</span><span class="sxs-lookup"><span data-stu-id="61950-345">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="61950-346">Löst eine Ausnahme mit der Zeichenfolge "Uh oh!"</span><span class="sxs-lookup"><span data-stu-id="61950-346">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="61950-347">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="61950-347">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="61950-348">Hello World!</span><span class="sxs-lookup"><span data-stu-id="61950-348">Hello World!</span></span>                             |

<span data-ttu-id="61950-349">`WaitForShutdown`blockiert, bis eine Unterbrechung (STRG-C/SIGINT oder SIGTERM) ausgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="61950-349">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="61950-350">Die app zeigt die `Console.WriteLine` Nachricht und wartet, bis eine Keypress zu beenden.</span><span class="sxs-lookup"><span data-stu-id="61950-350">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="61950-351">**Starten (string Url, Aktion<IRouteBuilder> RouteBuilder)**</span><span class="sxs-lookup"><span data-stu-id="61950-351">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="61950-352">Verwenden Sie eine URL und eine Instanz von `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="61950-352">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="61950-353">Führt zum gleiche Ergebnis wie **starten (Aktion<IRouteBuilder> RouteBuilder)**, außer die Anwendung reagiert auf `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="61950-353">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="61950-354">**StartWith(Action<IApplicationBuilder> app)**</span><span class="sxs-lookup"><span data-stu-id="61950-354">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="61950-355">Geben Sie ein Delegat zum Konfigurieren einer `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="61950-355">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="61950-356">Führen Sie im Browser, um eine Anforderung `http://localhost:5000` zum Empfangen der Antwort "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="61950-356">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="61950-357">`WaitForShutdown`blockiert, bis eine Unterbrechung (STRG-C/SIGINT oder SIGTERM) ausgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="61950-357">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="61950-358">Die app zeigt die `Console.WriteLine` Nachricht und wartet, bis eine Keypress zu beenden.</span><span class="sxs-lookup"><span data-stu-id="61950-358">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="61950-359">**StartWith (string Url, Aktion<IApplicationBuilder> app)**</span><span class="sxs-lookup"><span data-stu-id="61950-359">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="61950-360">Geben Sie eine URL und ein Delegat zum Konfigurieren einer `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="61950-360">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="61950-361">Führt zum gleiche Ergebnis wie **StartWith (Aktion<IApplicationBuilder> app)**, außer die Anwendung reagiert auf `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="61950-361">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="61950-362">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="61950-362">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="61950-363">**Run**</span><span class="sxs-lookup"><span data-stu-id="61950-363">**Run**</span></span>

<span data-ttu-id="61950-364">Die `Run` Methode startet die Web-app und blockiert den aufrufenden Thread, bis der Host heruntergefahren wird:</span><span class="sxs-lookup"><span data-stu-id="61950-364">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="61950-365">**Start**</span><span class="sxs-lookup"><span data-stu-id="61950-365">**Start**</span></span>

<span data-ttu-id="61950-366">Führen Sie den Host in eine nicht blockierende Weise durch Aufrufen seiner `Start` Methode:</span><span class="sxs-lookup"><span data-stu-id="61950-366">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="61950-367">Wenn eine Liste mit URLs zu übergeben, wird die `Start` -Methode, auf die angegebenen URLs überwacht:</span><span class="sxs-lookup"><span data-stu-id="61950-367">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>


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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="61950-368">IHostingEnvironment-Schnittstelle</span><span class="sxs-lookup"><span data-stu-id="61950-368">IHostingEnvironment interface</span></span>

<span data-ttu-id="61950-369">Die [IHostingEnvironment Schnittstelle](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) enthält Informationen zu den app-Web-hosting-Umgebung.</span><span class="sxs-lookup"><span data-stu-id="61950-369">The [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="61950-370">Verwenden Sie [Konstruktoreinfügung](xref:fundamentals/dependency-injection) zum Abrufen der `IHostingEnvironment` um die Eigenschaften und die Erweiterungsmethoden verwenden:</span><span class="sxs-lookup"><span data-stu-id="61950-370">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="61950-371">Ein [konventionsbasierte Ansatz](xref:fundamentals/environments#startup-conventions) können verwendet werden, um die Anwendung beim Start auf Grundlage der Umgebung zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="61950-371">A [convention-based approach](xref:fundamentals/environments#startup-conventions) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="61950-372">Fügen Sie alternativ die `IHostingEnvironment` in der `Startup` Konstruktor für die Verwendung in `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="61950-372">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="61950-373">Zusätzlich zu den `IsDevelopment` Erweiterungsmethode, `IHostingEnvironment` bietet `IsStaging`, `IsProduction`, und `IsEnvironment(string environmentName)` Methoden.</span><span class="sxs-lookup"><span data-stu-id="61950-373">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="61950-374">Finden Sie unter [arbeiten mit mehreren Umgebungen](xref:fundamentals/environments) Details.</span><span class="sxs-lookup"><span data-stu-id="61950-374">See [Working with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="61950-375">Die `IHostingEnvironment` Service kann auch direkt in injiziert die `Configure` Methode zum Einrichten der Verarbeitungspipeline:</span><span class="sxs-lookup"><span data-stu-id="61950-375">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="61950-376">`IHostingEnvironment`können eingegeben werden, in der `Invoke` Methode, wenn benutzerdefinierte [Middleware](xref:fundamentals/middleware#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="61950-376">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="61950-377">IApplicationLifetime-Schnittstelle</span><span class="sxs-lookup"><span data-stu-id="61950-377">IApplicationLifetime interface</span></span>

<span data-ttu-id="61950-378">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) nach dem Starten und Herunterfahren Aktivitäten ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="61950-378">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="61950-379">Drei Eigenschaften auf der Schnittstelle sind Abbruchtoken verwendet, um registrieren `Action` Methoden, die zum Starten und Herunterfahren Ereignisse zu definieren.</span><span class="sxs-lookup"><span data-stu-id="61950-379">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span> <span data-ttu-id="61950-380">Es gibt auch eine `StopApplication` Methode.</span><span class="sxs-lookup"><span data-stu-id="61950-380">There's also a `StopApplication` method.</span></span>

| <span data-ttu-id="61950-381">Abbruchtoken</span><span class="sxs-lookup"><span data-stu-id="61950-381">Cancellation Token</span></span>    | <span data-ttu-id="61950-382">Wird ausgelöst, wenn &#8230;</span><span class="sxs-lookup"><span data-stu-id="61950-382">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="61950-383">Der Host wurde vollständig gestartet.</span><span class="sxs-lookup"><span data-stu-id="61950-383">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="61950-384">Der Host führt ein ordnungsgemäßes Herunterfahren.</span><span class="sxs-lookup"><span data-stu-id="61950-384">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="61950-385">Anforderungen möglicherweise noch verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="61950-385">Requests may still be processing.</span></span> <span data-ttu-id="61950-386">Shutdown ist blockiert, bis diese abgeschlossen wird.</span><span class="sxs-lookup"><span data-stu-id="61950-386">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="61950-387">Der Host wird ein ordnungsgemäßes Herunterfahren abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="61950-387">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="61950-388">Alle Anforderungen verarbeitet werden soll.</span><span class="sxs-lookup"><span data-stu-id="61950-388">All requests should be processed.</span></span> <span data-ttu-id="61950-389">Shutdown ist blockiert, bis diese abgeschlossen wird.</span><span class="sxs-lookup"><span data-stu-id="61950-389">Shutdown blocks until this event completes.</span></span> |

| <span data-ttu-id="61950-390">Methode</span><span class="sxs-lookup"><span data-stu-id="61950-390">Method</span></span>            | <span data-ttu-id="61950-391">Aktion</span><span class="sxs-lookup"><span data-stu-id="61950-391">Action</span></span>                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | <span data-ttu-id="61950-392">Anforderungen Beendigung der aktuellen Anwendung.</span><span class="sxs-lookup"><span data-stu-id="61950-392">Requests termination of the current application.</span></span> |

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

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="61950-393">Problembehandlung bei System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="61950-393">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="61950-394">**Gilt für nur ASP.NET Core 2.0**</span><span class="sxs-lookup"><span data-stu-id="61950-394">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="61950-395">Ein Host kann erstellt werden, von Räumen `IStartup` direkt in die abhängigkeitseinschleusungscontainer anstelle von Aufrufen `UseStartup` oder `Configure`:</span><span class="sxs-lookup"><span data-stu-id="61950-395">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="61950-396">Wenn der Host auf diese Weise erstellt wird, kann der folgende Fehler auftreten:</span><span class="sxs-lookup"><span data-stu-id="61950-396">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="61950-397">Dies geschieht, weil die [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (die aktuelle Assembly) ist erforderlich, um die Überprüfung `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="61950-397">This occurs because the [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="61950-398">Wenn die app manuell injiziert `IStartup` in der abhängigkeitseinschleusungscontainer hinzufügen den folgenden Aufruf `WebHostBuilder` mit dem Assemblynamen angegeben:</span><span class="sxs-lookup"><span data-stu-id="61950-398">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="61950-399">Fügen Sie alternativ eine Dummy `Configure` auf die `WebHostBuilder`, welche die `applicationName`(`ApplicationKey`) automatisch:</span><span class="sxs-lookup"><span data-stu-id="61950-399">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="61950-400">**Hinweis**: Dies ist nur erforderlich, mit der Version 2.0 von ASP.NET Core und nur wenn die app Aufrufen nicht `UseStartup` oder `Configure`.</span><span class="sxs-lookup"><span data-stu-id="61950-400">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="61950-401">Weitere Informationen finden Sie unter [Ankündigungen: Microsoft.Extensions.PlatformAbstractions wurde entfernt / / (Kommentar)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) und [StartupInjection Beispiel](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="61950-401">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="61950-402">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="61950-402">Additional resources</span></span>

* [<span data-ttu-id="61950-403">Hosten unter Windows mit IIS</span><span class="sxs-lookup"><span data-stu-id="61950-403">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="61950-404">Hosten unter Linux mit Nginx</span><span class="sxs-lookup"><span data-stu-id="61950-404">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
* [<span data-ttu-id="61950-405">Hosten unter Linux mit Apache</span><span class="sxs-lookup"><span data-stu-id="61950-405">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
* [<span data-ttu-id="61950-406">Host in einem Windowsdienst</span><span class="sxs-lookup"><span data-stu-id="61950-406">Host in a Windows Service</span></span>](xref:host-and-deploy/windows-service)
