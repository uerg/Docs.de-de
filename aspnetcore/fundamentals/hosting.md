---
title: Hosten in ASP.NET Core | Microsoft Docs
author: ardalis
description: "Einführung in Web-Hosts in ASP.NET Core."
keywords: ASP.NET Core, Webhost, IWebHost
ms.author: riande
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.assetid: 4e45311d-8d56-46e2-b99d-6f65b648a277
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0725f3d2c130068094792e5ba9e17481155e4294
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2017
---
# <a name="introduction-to-hosting-in-aspnet-core"></a><span data-ttu-id="9cdb1-104">Einführung zum Hosten der in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9cdb1-104">Introduction to hosting in ASP.NET Core</span></span>

<span data-ttu-id="9cdb1-105">Durch [Steve Smith](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="9cdb1-105">By [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="9cdb1-106">Um eine ASP.NET Core-app auszuführen, müssen Sie zum Konfigurieren und starten Sie einen Host mit `WebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-106">To run an ASP.NET Core app, you need to configure and launch a host using `WebHostBuilder`.</span></span>

## <a name="what-is-a-host"></a><span data-ttu-id="9cdb1-107">Was ist ein Host?</span><span class="sxs-lookup"><span data-stu-id="9cdb1-107">What is a Host?</span></span>

<span data-ttu-id="9cdb1-108">ASP.NET Core apps erfordern eine *Host* in den ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-108">ASP.NET Core apps require a *host* in which to execute.</span></span> <span data-ttu-id="9cdb1-109">Ein Host muss implementieren die `IWebHost` -Schnittstelle, die Sammlungen von Funktionen und Diensten verfügbar macht, und eine `Start` Methode.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-109">A host must implement the `IWebHost` interface, which exposes collections of features and services, and a `Start` method.</span></span> <span data-ttu-id="9cdb1-110">Der Host ist in der Regel erstellt eine Instanz von einem `WebHostBuilder`, die erstellt und zurückgibt eine `WebHost` Instanz.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-110">The host is typically created using an instance of a `WebHostBuilder`, which builds and returns a  `WebHost` instance.</span></span> <span data-ttu-id="9cdb1-111">Die `WebHost` verweist auf den Server, die Anforderungen behandelt.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-111">The `WebHost` references the server that will handle requests.</span></span> <span data-ttu-id="9cdb1-112">Erfahren Sie mehr über [Server](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="9cdb1-112">Learn more about [servers](servers/index.md).</span></span>

### <a name="what-is-the-difference-between-a-host-and-a-server"></a><span data-ttu-id="9cdb1-113">Was ist der Unterschied zwischen einem Host und einem Server?</span><span class="sxs-lookup"><span data-stu-id="9cdb1-113">What is the difference between a host and a server?</span></span>

<span data-ttu-id="9cdb1-114">Der Host ist verantwortlich für die anwendungsverwaltung starten sowie seine Lebensdauer.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-114">The host is responsible for application startup and lifetime management.</span></span> <span data-ttu-id="9cdb1-115">Der Server ist verantwortlich für das HTTP-Anforderungen akzeptieren.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-115">The server is responsible for accepting HTTP requests.</span></span> <span data-ttu-id="9cdb1-116">Teil des Hosts Verantwortung umfasst das sicherstellen, dass die Anwendung Dienste und der Server verfügbar und ordnungsgemäß konfiguriert sind.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-116">Part of the host's responsibility includes ensuring the application's services and the server are available and properly configured.</span></span> <span data-ttu-id="9cdb1-117">Sie können den Host als Wrapper um den Server vorstellen.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-117">You can think of the host as being a wrapper around the server.</span></span> <span data-ttu-id="9cdb1-118">Der Host für die Verwendung ein bestimmtes Servers konfiguriert; der Server ist der Host nicht bewusst.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-118">The host is configured to use a particular server; the server is unaware of its host.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="9cdb1-119">Einrichten eines Hosts</span><span class="sxs-lookup"><span data-stu-id="9cdb1-119">Setting up a Host</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9cdb1-120">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9cdb1-120">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9cdb1-121">Erstellen Sie einen Host mit einer Instanz von `WebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-121">You create a host using an instance of `WebHostBuilder`.</span></span> <span data-ttu-id="9cdb1-122">Dies erfolgt in der Regel in der app-Einstiegspunkt: `public static void Main` (in Projektvorlagen befindet sich eine *"Program.cs"* Datei).</span><span class="sxs-lookup"><span data-stu-id="9cdb1-122">This is typically done in your app's entry point: `public static void Main` (which in the project templates is located in a *Program.cs* file).</span></span> <span data-ttu-id="9cdb1-123">Eine typische *"Program.cs"*wie unten veranschaulicht, wie eine `WebHostBuilder` auf einen Host zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-123">A typical *Program.cs*, shown below, demonstrates how to use a `WebHostBuilder` to build a host.</span></span>

<span data-ttu-id="9cdb1-124">[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=14,15,16,17,18,19,20,21)]</span><span class="sxs-lookup"><span data-stu-id="9cdb1-124">[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=14,15,16,17,18,19,20,21)]</span></span>

<span data-ttu-id="9cdb1-125">Die `WebHostBuilder` ist für den Host erstellen, die den Server für die app Bootstrapping verantwortlich.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-125">The `WebHostBuilder` is responsible for creating the host that will bootstrap the server for the app.</span></span> <span data-ttu-id="9cdb1-126">`WebHostBuilder`erfordert, geben Sie einen Server, der implementiert `IServer` (`UseKestrel` im obigen Code).</span><span class="sxs-lookup"><span data-stu-id="9cdb1-126">`WebHostBuilder` requires you provide a server that implements `IServer` (`UseKestrel` in the code above).</span></span> <span data-ttu-id="9cdb1-127">`UseKestrel`Gibt an, dass die Kestrel Server von der app verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-127">`UseKestrel` specifies the Kestrel server will be used by the app.</span></span>

<span data-ttu-id="9cdb1-128">Des Servers *Content Stamm* bestimmt, wo er für Inhaltsdateien wie MVC-Ansicht-Dateien sucht.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-128">The server's *content root* determines where it searches for content files, like MVC View files.</span></span> <span data-ttu-id="9cdb1-129">Der standardmäßige Content-Stamm ist der Ordner, von dem die Anwendung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-129">The default content root is the folder from which the application is run.</span></span>

> [!NOTE]
> <span data-ttu-id="9cdb1-130">Angeben von `Directory.GetCurrentDirectory` wie den Inhalt Stamm Stammordner für das Webprojekt als Inhalt der app-Stammverzeichnis, beim Starten der app aus diesem Ordner verwenden (z. B. durch Aufruf von `dotnet run` aus dem Projektordner Web).</span><span class="sxs-lookup"><span data-stu-id="9cdb1-130">Specifying `Directory.GetCurrentDirectory` as the content root will use the web project's root folder as the app's content root when the app is started from this folder (for example, calling `dotnet run` from the web project folder).</span></span> <span data-ttu-id="9cdb1-131">Dies ist die Standardeinstellung in Visual Studio verwendet und `dotnet new` Vorlagen.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-131">This is the default used in Visual Studio and `dotnet new` templates.</span></span>

<span data-ttu-id="9cdb1-132">Rufen Sie zum Verwenden von IIS als Reverseproxy `UseIISIntegration` im Rahmen der Erstellung des Hosts.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-132">To use IIS as a reverse proxy, call `UseIISIntegration` as part of building the host.</span></span> 

<span data-ttu-id="9cdb1-133">Beachten Sie, dass `UseIISIntegration` nicht Konfigurieren einer *Server*, z. B. `UseKestrel` verfügt.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-133">Note that `UseIISIntegration` doesn't configure a *server*, like `UseKestrel` does.</span></span> <span data-ttu-id="9cdb1-134">Um IIS mit ASP.NET Core verwenden zu können, müssen Sie beide angeben `UseKestrel` und `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-134">To use IIS with ASP.NET Core, you must specify both `UseKestrel` and `UseIISIntegration`.</span></span> <span data-ttu-id="9cdb1-135">`UseKestrel`erstellt den Webserver und hostet die app.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-135">`UseKestrel` creates the web server and hosts the app.</span></span> <span data-ttu-id="9cdb1-136">`UseIISIntegration`überprüft Umgebungsvariablen, die von IIS/IIS Express verwendet und konfiguriert die Einstellungen wie z. B. den Port zu Lauschen und die Header verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-136">`UseIISIntegration` examines environment variables used by IIS/IISExpress and configures settings such as the port to listen on and the headers to use.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9cdb1-137">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9cdb1-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9cdb1-138">Erstellen Sie einen Host mit einer Instanz von `WebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-138">You create a host using an instance of `WebHostBuilder`.</span></span> <span data-ttu-id="9cdb1-139">Dies erfolgt in der Regel in der app-Einstiegspunkt: `public static void Main` (in Projektvorlagen befindet sich eine *"Program.cs"* Datei).</span><span class="sxs-lookup"><span data-stu-id="9cdb1-139">This is typically done in your app's entry point: `public static void Main` (which in the project templates is located in a *Program.cs* file).</span></span> <span data-ttu-id="9cdb1-140">Eine typische *"Program.cs"*, wie unten Aufrufe gezeigt `CreateDefaultbuilder` auf einen Host zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="9cdb1-140">A typical *Program.cs*, shown below, calls `CreateDefaultbuilder` to build a host:</span></span>

<span data-ttu-id="9cdb1-141">[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]</span><span class="sxs-lookup"><span data-stu-id="9cdb1-141">[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]</span></span>

<span data-ttu-id="9cdb1-142">`CreateDefaultbuilder`erstellt eine Instanz des `WebHostBuilder` auf den Host zu erstellen, die den Server für die app gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-142">`CreateDefaultbuilder` creates an instance of `WebHostBuilder` to build the host that bootstraps the server for the app.</span></span> <span data-ttu-id="9cdb1-143">Der Host erfordert eine [Server, die Iserver-c# implementiert](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="9cdb1-143">The host requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="9cdb1-144">Die integrierten Server sind [Kestrel](servers/kestrel.md) und [HTTP.sys](servers/httpsys.md); `CreateDefaultbuilder` Kestrel wird standardmäßig verwendet.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-144">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md); `CreateDefaultbuilder` use Kestrel by default.</span></span>

<span data-ttu-id="9cdb1-145">`CreateDefaultbuilder`führt Installationsaufgaben zusätzlich zum Konfigurieren von Kestrel wie des Webservers:</span><span class="sxs-lookup"><span data-stu-id="9cdb1-145">`CreateDefaultbuilder` performs set-up tasks in addition to configuring Kestrel as the web server:</span></span>

* <span data-ttu-id="9cdb1-146">Legt den Content-Stamm auf `Directory.GetCurrentDirectory`.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-146">Sets the content root to `Directory.GetCurrentDirectory`.</span></span>
* <span data-ttu-id="9cdb1-147">Lädt die Konfiguration aus:</span><span class="sxs-lookup"><span data-stu-id="9cdb1-147">Loads configuration from:</span></span>
  * <span data-ttu-id="9cdb1-148">*AppSettings.JSON*</span><span class="sxs-lookup"><span data-stu-id="9cdb1-148">*appsettings.json*</span></span>
  * <span data-ttu-id="9cdb1-149">*"appSettings". \<EnvironmentName > JSON*.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-149">*appsettings.\<EnvironmentName>.json*.</span></span>
  * <span data-ttu-id="9cdb1-150">Benutzer geheime Schlüssel, wenn die app in der Entwicklungsumgebung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-150">user secrets when the app runs in the Development environment</span></span>
  * <span data-ttu-id="9cdb1-151">Umgebungsvariablen</span><span class="sxs-lookup"><span data-stu-id="9cdb1-151">environment variables</span></span>
  * <span data-ttu-id="9cdb1-152">angegebene Befehlszeile args</span><span class="sxs-lookup"><span data-stu-id="9cdb1-152">supplied command line args</span></span>
* <span data-ttu-id="9cdb1-153">Protokollierung für Konsole und Debug-Ausgabe konfiguriert mit Filtern in einem Konfigurationsabschnitt für die Protokollierung angegebene Regeln.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-153">Configures logging for console and debug output, with filtering rules specified in a Logging configuration section.</span></span>
* <span data-ttu-id="9cdb1-154">Ermöglicht die Integration von IIS.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-154">Enables IIS integration.</span></span>
* <span data-ttu-id="9cdb1-155">Fügt die Developer-Ausnahme-Seite hinzu, wenn die app in der Entwicklungsumgebung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-155">Adds the developer exception page when the app runs in the Development environment.</span></span>

<span data-ttu-id="9cdb1-156">Des Servers *Content Stamm* bestimmt, wo er für Inhaltsdateien wie MVC-Ansicht-Dateien sucht.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-156">The server's *content root* determines where it searches for content files, like MVC View files.</span></span> <span data-ttu-id="9cdb1-157">Der standardmäßige Content-Stamm ist der Ordner, von dem die Anwendung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-157">The default content root is the folder from which the application is run.</span></span>

> [!NOTE]
> <span data-ttu-id="9cdb1-158">Angeben von `Directory.GetCurrentDirectory` wie den Inhalt Stamm Stammordner für das Webprojekt als Inhalt der app-Stammverzeichnis, beim Starten der app aus diesem Ordner verwenden (z. B. durch Aufruf von `dotnet run` aus dem Projektordner Web).</span><span class="sxs-lookup"><span data-stu-id="9cdb1-158">Specifying `Directory.GetCurrentDirectory` as the content root will use the web project's root folder as the app's content root when the app is started from this folder (for example, calling `dotnet run` from the web project folder).</span></span> <span data-ttu-id="9cdb1-159">Dies ist die Standardeinstellung in Visual Studio verwendet und `dotnet new` Vorlagen.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-159">This is the default used in Visual Studio and `dotnet new` templates.</span></span>

<span data-ttu-id="9cdb1-160">Wenn Sie IIS als Reverseproxy verwenden, ruft ASP.NET Core automatisch `UseIISIntegration` im Rahmen der Erstellung des Hosts.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-160">When you use IIS as a reverse proxy, ASP.NET Core automatically calls `UseIISIntegration` as part of building the host.</span></span> <span data-ttu-id="9cdb1-161">Weitere Informationen finden Sie unter [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="9cdb1-161">For more information, see [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span>

<span data-ttu-id="9cdb1-162">Beachten Sie, dass `UseIISIntegration` nicht Konfigurieren einer *Server*, z. B. `UseKestrel` verfügt.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-162">Note that `UseIISIntegration` doesn't configure a *server*, like `UseKestrel` does.</span></span> <span data-ttu-id="9cdb1-163">`UseKestrel`erstellt den Webserver und hostet die app.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-163">`UseKestrel` creates the web server and hosts the app.</span></span> <span data-ttu-id="9cdb1-164">`UseIISIntegration`überprüft Umgebungsvariablen, die von IIS/IIS Express verwendet und konfiguriert die Einstellungen wie z. B. den Port zu Lauschen und die Header verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-164">`UseIISIntegration` examines environment variables used by IIS/IISExpress and configures settings such as the port to listen on and the headers to use.</span></span>

---

<span data-ttu-id="9cdb1-165">Eine minimale Implementierung der Konfiguration eines Hosts (und ASP.NET Core-app) würde nur einen Server und die Konfiguration der Anforderungspipeline die app einschließen:</span><span class="sxs-lookup"><span data-stu-id="9cdb1-165">A minimal implementation of configuring a host (and an ASP.NET Core app) would include just a server and configuration of the app's request pipeline:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .Configure(app =>
    {
        app.Run(async (context) => await context.Response.WriteAsync("Hi!"));
    })
    .Build();

host.Run();
```

> [!NOTE]
> <span data-ttu-id="9cdb1-166">Beim Einrichten von eines Hosts können Sie angeben `Configure` und `ConfigureServices` Methoden stattdessen oder zusätzlich zum Angeben einer `Startup` Klasse (die müssen auch diese Methoden - definieren finden Sie unter [Anwendungsstart](startup.md)).</span><span class="sxs-lookup"><span data-stu-id="9cdb1-166">When setting up a host, you can provide `Configure` and `ConfigureServices` methods, instead of or in addition to specifying a `Startup` class (which must also define these methods - see [Application Startup](startup.md)).</span></span> <span data-ttu-id="9cdb1-167">Mehrere Aufrufe `ConfigureServices` untereinander angefügt wird; Aufrufe von `Configure` oder `UseStartup` ersetzt die vorherige Einstellungen.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-167">Multiple calls to `ConfigureServices` will append to one another; calls to `Configure` or `UseStartup` will replace previous settings.</span></span>

## <a name="configuring-a-host"></a><span data-ttu-id="9cdb1-168">Konfigurieren eines Hosts</span><span class="sxs-lookup"><span data-stu-id="9cdb1-168">Configuring a Host</span></span>

<span data-ttu-id="9cdb1-169">Die `WebHostBuilder` stellt Methoden zum Festlegen der Großteil der verfügbaren Konfigurationswerte für den Host, die auch direkt festgelegt werden kann `UseSetting` und den zugehörigen Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-169">The `WebHostBuilder` provides methods for setting most of the available configuration values for the host, which can also be set directly using `UseSetting` and associated key.</span></span>

### <a name="host-configuration-values"></a><span data-ttu-id="9cdb1-170">Host-Konfigurationswerte</span><span class="sxs-lookup"><span data-stu-id="9cdb1-170">Host Configuration Values</span></span>

<span data-ttu-id="9cdb1-171">**Erfassen von Startfehlern**`bool`</span><span class="sxs-lookup"><span data-stu-id="9cdb1-171">**Capture Startup Errors** `bool`</span></span>

<span data-ttu-id="9cdb1-172">Schlüssel: `captureStartupErrors`.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-172">Key: `captureStartupErrors`.</span></span> <span data-ttu-id="9cdb1-173">Wird standardmäßig auf `false` festgelegt.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-173">Defaults to `false`.</span></span> <span data-ttu-id="9cdb1-174">Wenn `false`, Fehler während des Starts Ergebnis auf dem Host beendet.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-174">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="9cdb1-175">Wenn `true`, der Host werden keine Ausnahmen aus erfasst die `Startup` Klasse, und versuchen, den Server zu starten.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-175">When `true`, the host will capture any exceptions from the `Startup` class and attempt to start the server.</span></span> <span data-ttu-id="9cdb1-176">Es wird eine Fehlerseite angezeigt (allgemeines oder detaillierte, basierend auf der Einstellung der detaillierte Fehler unten) für jede Anforderung.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-176">It will display an error page (generic, or detailed, based on the Detailed Errors setting, below) for every request.</span></span> <span data-ttu-id="9cdb1-177">Legen Sie mithilfe der `CaptureStartupErrors` Methode.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-177">Set using the `CaptureStartupErrors` method.</span></span>

<span data-ttu-id="9cdb1-178">Hinweis: Wenn Ihre app mit Kestrel und IIS ausgeführt wird, ist das Standardverhalten von Startfehlern zu erfassen.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-178">Note: When your app runs with Kestrel and IIS, the default behavior is to capture startup errors.</span></span> 

```csharp
new WebHostBuilder()
    .CaptureStartupErrors(true)
   ```

<span data-ttu-id="9cdb1-179">**Inhalts-Stamm**`string`</span><span class="sxs-lookup"><span data-stu-id="9cdb1-179">**Content Root** `string`</span></span>

<span data-ttu-id="9cdb1-180">Schlüssel: `contentRoot`.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-180">Key: `contentRoot`.</span></span> <span data-ttu-id="9cdb1-181">Der Standardwert ist der Ordner, in dem die Assembly (für Kestrel; befindet IIS wird dem Projektstamm Web wird standardmäßig verwendet).</span><span class="sxs-lookup"><span data-stu-id="9cdb1-181">Defaults to the folder where the application assembly resides (for Kestrel; IIS will use the web project root by default).</span></span> <span data-ttu-id="9cdb1-182">Diese Einstellung bestimmt, in dem ASP.NET Core für Inhaltsdateien, z. B. MVC-Ansichten Suche begonnen wird.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-182">This setting determines where ASP.NET Core will begin searching for content files, such as MVC Views.</span></span> <span data-ttu-id="9cdb1-183">Verwendet auch als den Basispfad für den [Root Webeinstellung](#web-root-setting).</span><span class="sxs-lookup"><span data-stu-id="9cdb1-183">Also used as the base path for the [Web Root Setting](#web-root-setting).</span></span> <span data-ttu-id="9cdb1-184">Legen Sie mithilfe der `UseContentRoot` Methode.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-184">Set using the `UseContentRoot` method.</span></span> <span data-ttu-id="9cdb1-185">Pfad muss vorhanden sein, oder Host kann nicht gestartet.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-185">Path must exist, or host will fail to start.</span></span>

```csharp
new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
   ```

<span data-ttu-id="9cdb1-186">**Detaillierte Fehler**`bool`</span><span class="sxs-lookup"><span data-stu-id="9cdb1-186">**Detailed Errors** `bool`</span></span>

<span data-ttu-id="9cdb1-187">Schlüssel: `detailedErrors`.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-187">Key: `detailedErrors`.</span></span> <span data-ttu-id="9cdb1-188">Wird standardmäßig auf `false` festgelegt.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-188">Defaults to `false`.</span></span> <span data-ttu-id="9cdb1-189">Wenn `true` (oder wenn die Umgebung auf "Entwicklung" festgelegt ist), die app werden Details zu Ausnahmen aus, starten, anstatt nur eine generische Fehlerseite angezeigt.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-189">When `true` (or when Environment is set to "Development"), the app will display details of startup exceptions, instead of just a generic error page.</span></span> <span data-ttu-id="9cdb1-190">Legen Sie mithilfe von `UseSetting`.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-190">Set using `UseSetting`.</span></span>

```csharp
new WebHostBuilder()
    .UseSetting("detailedErrors", "true")
```

<span data-ttu-id="9cdb1-191">Wenn ausführliche Fehler festgelegt ist, um `false` und Erfassen von Startfehlern `true`, eine generische Fehlerseite wird als Antwort auf jede Anforderung an den Server angezeigt.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-191">When Detailed Errors is set to `false` and Capture Startup Errors is `true`, a generic error page is displayed in response to every request to the server.</span></span>

![Generische Fehlerseite angezeigt.](hosting/_static/generic-error-page.png)

<span data-ttu-id="9cdb1-193">Wenn ausführliche Fehler festgelegt ist, um `true` und Erfassen von Startfehlern `true`, eine ausführliche Fehlermeldung-Seite wird als Antwort auf jede Anforderung an den Server angezeigt.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-193">When Detailed Errors is set to `true` and Capture Startup Errors is `true`, a detailed error page is displayed in response to every request to the server.</span></span>

![Detaillierte Fehler (Seite)](hosting/_static/detailed-error-page.png)

<span data-ttu-id="9cdb1-195">**Umgebung**`string`</span><span class="sxs-lookup"><span data-stu-id="9cdb1-195">**Environment** `string`</span></span>

<span data-ttu-id="9cdb1-196">Schlüssel: `environment`.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-196">Key: `environment`.</span></span> <span data-ttu-id="9cdb1-197">Der Standardwert ist "Produktion".</span><span class="sxs-lookup"><span data-stu-id="9cdb1-197">Defaults to "Production".</span></span> <span data-ttu-id="9cdb1-198">Kann auf einen beliebigen Wert festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-198">May be set to any value.</span></span> <span data-ttu-id="9cdb1-199">Framework definierte Werte sind "Entwicklung", "Staging" und "Produktion" definieren.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-199">Framework-defined values include "Development", "Staging", and "Production".</span></span> <span data-ttu-id="9cdb1-200">Werte sind nicht in der Groß-/Kleinschreibung beachtet.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-200">Values are not case sensitive.</span></span> <span data-ttu-id="9cdb1-201">Finden Sie unter [arbeiten mit mehreren Umgebungen](environments.md).</span><span class="sxs-lookup"><span data-stu-id="9cdb1-201">See [Working with Multiple Environments](environments.md).</span></span> <span data-ttu-id="9cdb1-202">Legen Sie mithilfe der `UseEnvironment` Methode.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-202">Set using the `UseEnvironment` method.</span></span>

```csharp
new WebHostBuilder()
    .UseEnvironment("Development")
```

> [!NOTE]
> <span data-ttu-id="9cdb1-203">Standardmäßig wird die Umgebung aus der gelesen der `ASPNETCORE_ENVIRONMENT` -Umgebungsvariablen angegeben.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-203">By default, the environment is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="9cdb1-204">Wenn Sie Visual Studio verwenden, können Umgebungsvariablen festgelegt werden, der *launchSettings.json* Datei.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-204">When using Visual Studio, environment variables may be set in the *launchSettings.json* file.</span></span>

<a id="server-urls"></a>

<span data-ttu-id="9cdb1-205">**Server-URLs**`string`</span><span class="sxs-lookup"><span data-stu-id="9cdb1-205">**Server URLs** `string`</span></span>

<span data-ttu-id="9cdb1-206">Schlüssel: `urls`.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-206">Key: `urls`.</span></span> <span data-ttu-id="9cdb1-207">Legen Sie auf ein Semikolon (;) getrennte Liste von URL-Präfixen, die auf der Server reagieren soll.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-207">Set to a semicolon (;) separated list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="9cdb1-208">Beispielsweise `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-208">For example, `http://localhost:123`.</span></span> <span data-ttu-id="9cdb1-209">Der Domäne/Hostname kann ersetzt werden, mit "\*" an, dass der Server Lauscht auf Anforderungen für eine beliebige IP-Adresse oder host mit dem angegebenen Port und Protokoll sollten (z. B. `http://*:5000` oder `https://*:5001`).</span><span class="sxs-lookup"><span data-stu-id="9cdb1-209">The domain/host name can be replaced with "\*" to indicate the server should listen to requests on any IP address or host using the specified port and protocol (for example, `http://*:5000` or `https://*:5001`).</span></span> <span data-ttu-id="9cdb1-210">Das Protokoll (`http://` oder `https://`) für jede URL eingeschlossen werden müssen.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-210">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="9cdb1-211">Die Präfixe werden durch den konfigurierten Server interpretiert; zu den unterstützten Bildformaten variiert zwischen Servern.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-211">The prefixes are interpreted by the configured server; supported formats will vary between servers.</span></span>

```csharp
new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="9cdb1-212">In ASP.NET Core 2.0 Kestrel verfügt über einen eigenen Endpunkt-Konfigurations-API und unterstützt keine `https://` in die `urls` Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-212">In ASP.NET Core 2.0, Kestrel has its own endpoint configuration API and does not support `https://` in the `urls` string.</span></span> <span data-ttu-id="9cdb1-213">Weitere Informationen finden Sie unter [Einführung in Kestrel](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="9cdb1-213">For more information, see [Introduction to Kestrel](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

<span data-ttu-id="9cdb1-214">**Startassembly**`string`</span><span class="sxs-lookup"><span data-stu-id="9cdb1-214">**Startup Assembly** `string`</span></span>

<span data-ttu-id="9cdb1-215">Schlüssel: `startupAssembly`.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-215">Key: `startupAssembly`.</span></span> <span data-ttu-id="9cdb1-216">Die Assembly zu suchende bestimmt die `Startup` Klasse.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-216">Determines the assembly to search for the `Startup` class.</span></span> <span data-ttu-id="9cdb1-217">Legen Sie mithilfe der `UseStartup` Methode.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-217">Set using the `UseStartup` method.</span></span> <span data-ttu-id="9cdb1-218">Kann stattdessen mit bestimmten Typ verweisen `WebHostBuilder.UseStartup<StartupType>`.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-218">May instead reference specific type using `WebHostBuilder.UseStartup<StartupType>`.</span></span> <span data-ttu-id="9cdb1-219">Wenn mehrere `UseStartup` Methoden aufgerufen werden, das letzte Lesezeichen hat Vorrang vor.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-219">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

<a name=web-root-setting></a>

<span data-ttu-id="9cdb1-220">**Webseiten Stammverzeichnis**`string`</span><span class="sxs-lookup"><span data-stu-id="9cdb1-220">**Web Root** `string`</span></span>

<span data-ttu-id="9cdb1-221">Schlüssel: `webroot`.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-221">Key: `webroot`.</span></span> <span data-ttu-id="9cdb1-222">Wenn nicht angegeben wird, ist die Standardeinstellung `(Content Root Path)\wwwroot`, sofern vorhanden.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-222">If not specified the default is `(Content Root Path)\wwwroot`, if it exists.</span></span> <span data-ttu-id="9cdb1-223">Wenn dieser Pfad nicht vorhanden ist, wird ein ohne-Op-dateisystemanbieter verwendet.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-223">If this path doesn't exist, then a no-op file provider is used.</span></span> <span data-ttu-id="9cdb1-224">Legen Sie mithilfe von `UseWebRoot`.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-224">Set using `UseWebRoot`.</span></span>

```csharp
new WebHostBuilder()
    .UseWebRoot("public")
```

### <a name="overriding-configuration"></a><span data-ttu-id="9cdb1-225">Überschreiben die Konfiguration</span><span class="sxs-lookup"><span data-stu-id="9cdb1-225">Overriding Configuration</span></span>

<span data-ttu-id="9cdb1-226">Verwendung [Konfiguration](configuration.md) zum Festlegen der Konfigurationswerte, die vom Host verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-226">Use [Configuration](configuration.md) to set configuration values to be used by the host.</span></span> <span data-ttu-id="9cdb1-227">Diese Werte können später überschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-227">These values may be subsequently overridden.</span></span> <span data-ttu-id="9cdb1-228">Dadurch wird angegeben, mit `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-228">This is specified using `UseConfiguration`.</span></span>

```csharp
public static void Main(string[] args)
{
    var config = new ConfigurationBuilder()
        .AddJsonFile("hosting.json", optional: true)
        .AddCommandLine(args)
        .Build();

    var host = new WebHostBuilder()
        .UseConfiguration(config)
        .UseKestrel()
        .Configure(app =>
        {
            app.Run(async (context) => await context.Response.WriteAsync("Hi!"));
        })
        .Build();

    host.Run();
}
```

<span data-ttu-id="9cdb1-229">Im obigen Beispiel Befehlszeilenargumente übergeben so konfigurieren Sie den Host oder Konfigurationseinstellungen können optional angegeben werden, einem *hosting.json* Datei.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-229">In the example above, command-line arguments may be passed in to configure the host, or configuration settings may optionally be specified in a *hosting.json* file.</span></span> <span data-ttu-id="9cdb1-230">Um den Host aus, führen Sie auf eine bestimmte URL angeben, könnten Sie den gewünschten Wert aus der Eingabeaufforderung übergeben:</span><span class="sxs-lookup"><span data-stu-id="9cdb1-230">To specify the host run on a particular URL, you could pass in the desired value from a command prompt:</span></span>

```console
dotnet run --urls "http://*:5000"
```

<span data-ttu-id="9cdb1-231">Die `Run` Methode startet die Web-app und blockiert den aufrufenden Thread, bis der Host heruntergefahren wird.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-231">The `Run` method starts the web app and blocks the calling thread until the host is shutdown.</span></span>

```csharp
host.Run();
```

<span data-ttu-id="9cdb1-232">Sie können den Host in eine nicht blockierende Weise ausführen, durch Aufrufen seiner `Start` Methode:</span><span class="sxs-lookup"><span data-stu-id="9cdb1-232">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="9cdb1-233">Übergeben Sie eine Liste von URLs für die `Start` -Methode, und es werden auf die angegebenen URLs überwachen:</span><span class="sxs-lookup"><span data-stu-id="9cdb1-233">Pass a list of URLs to the `Start` method and it will listen on the URLs specified:</span></span>

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

<span data-ttu-id="9cdb1-234">Die URL-Formaten, die hier gelten abhängig sind, auf dem Server, den Sie verwenden.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-234">The URL formats that are valid here depend on the server you're using.</span></span> <span data-ttu-id="9cdb1-235">Weitere Informationen finden Sie unter [Server URLs](#server-urls) weiter oben in diesem Artikel.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-235">For more information, see [Server URLs](#server-urls) earlier in this article.</span></span>

> [!NOTE]
> <span data-ttu-id="9cdb1-236">Die `UseConfiguration` Erweiterungsmethode ist zurzeit der Analyse eines zurückgegebenen Konfigurationsabschnitts kann nicht `GetSection` (z. B. `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-236">The `UseConfiguration` extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="9cdb1-237">Die `GetSection` Methode filtert die Schlüssel der Konfiguration im Abschnitt angefordert, sondern bleibt der Name des Abschnitts auf die Schlüssel (z. B. `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="9cdb1-237">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="9cdb1-238">Die `UseConfiguration` Methode erwartet die Schlüssel entsprechend dem `WebHostBuilder` Schlüssel (z. B. `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="9cdb1-238">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="9cdb1-239">Das Vorhandensein der Abschnittsname den Schlüsseln wird verhindert, dass Werte im Abschnitt konfigurieren den Host.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-239">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="9cdb1-240">Dieses Problem wird in einer bevorstehenden Version behoben.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-240">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="9cdb1-241">Weitere Informationen und problemumgehungen finden Sie unter [vollständige Schlüssel übergeben Konfigurationsabschnitt in WebHostBuilder.UseConfiguration verwendet](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="9cdb1-241">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

### <a name="ordering-importance"></a><span data-ttu-id="9cdb1-242">Reihenfolge der Wichtigkeit</span><span class="sxs-lookup"><span data-stu-id="9cdb1-242">Ordering Importance</span></span>

<span data-ttu-id="9cdb1-243">`WebHostBuilder`Einstellungen werden zuerst vom bestimmte Umgebungsvariablen, gelesen, wenn festgelegt.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-243">`WebHostBuilder` settings are first read from certain environment variables, if set.</span></span> <span data-ttu-id="9cdb1-244">Verwenden Sie diese Umgebungsvariablen müssen das Format `ASPNETCORE_{configurationKey}`, also beispielsweise um URLs festgelegt, die der Server wird standardmäßig auf lauschen, Sie legen würde `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-244">These environment variables must use the format `ASPNETCORE_{configurationKey}`, so for example to set the URLs the server will listen on by default, you would set `ASPNETCORE_URLS`.</span></span>

<span data-ttu-id="9cdb1-245">Sie können eine der folgenden Werte für Umgebungsvariablen überschreiben, indem Konfiguration angeben (mithilfe von `UseConfiguration`) oder indem Sie den Wert explizit festlegen (mit `UseUrls` für die Instanz).</span><span class="sxs-lookup"><span data-stu-id="9cdb1-245">You can override any of these environment variable values by specifying configuration (using `UseConfiguration`) or by setting the value explicitly (using `UseUrls` for instance).</span></span> <span data-ttu-id="9cdb1-246">Der Host wird verwendet, unabhängig davon, welche Option zuletzt legt den Wert fest.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-246">The host will use whichever option sets the value last.</span></span> <span data-ttu-id="9cdb1-247">Aus diesem Grund `UseIISIntegration` muss nach angezeigt werden `UseUrls`, da er die URL mit einem dynamisch von IIS bereitgestellten ersetzt.</span><span class="sxs-lookup"><span data-stu-id="9cdb1-247">For this reason, `UseIISIntegration` must appear after `UseUrls`, because it replaces the URL with one dynamically provided by IIS.</span></span> <span data-ttu-id="9cdb1-248">Wenn Sie programmgesteuert die Standard-URL auf einen Wert festgelegt, aber lassen sie bei der Konfiguration außer Kraft gesetzt werden soll, konnte Sie den Host wie folgt konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="9cdb1-248">If you want to programmatically set the default URL to one value, but allow it to be overridden with configuration, you could configure the host as follows:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddCommandLine(args)
    .Build();

var host = new WebHostBuilder()
    .UseUrls("http://*:1000") // default URL
    .UseConfiguration(config) // override from command line
    .UseKestrel()
    .Build();
```

## <a name="additional-resources"></a><span data-ttu-id="9cdb1-249">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="9cdb1-249">Additional resources</span></span>

* [<span data-ttu-id="9cdb1-250">Veröffentlichen Sie in Windows unter Verwendung von IIS</span><span class="sxs-lookup"><span data-stu-id="9cdb1-250">Publish to Windows using IIS</span></span>](../publishing/iis.md)
* [<span data-ttu-id="9cdb1-251">Veröffentlichen Sie in Linux mit Nginx</span><span class="sxs-lookup"><span data-stu-id="9cdb1-251">Publish to Linux using Nginx</span></span>](../publishing/linuxproduction.md)
* [<span data-ttu-id="9cdb1-252">Veröffentlichen Sie in Linux mit Apache</span><span class="sxs-lookup"><span data-stu-id="9cdb1-252">Publish to Linux using Apache</span></span>](../publishing/apache-proxy.md)
* [<span data-ttu-id="9cdb1-253">Host in einem Windowsdienst</span><span class="sxs-lookup"><span data-stu-id="9cdb1-253">Host in a Windows Service</span></span>](xref:hosting/windows-service)

