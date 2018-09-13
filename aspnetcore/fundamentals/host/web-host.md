---
title: ASP.NET Core-Webhost
author: guardrex
description: Erfahren Sie mehr über den Webhost in ASP.NET Core, der für das Starten der App und das Verwalten der Lebensdauer verantwortlich ist.
ms.author: riande
ms.custom: mvc
ms.date: 09/01/2018
uid: fundamentals/host/web-host
ms.openlocfilehash: 7440ab26534840b190a346614f645860fc2b7d78
ms.sourcegitcommit: 7211ae2dd702f67d36365831c490d6178c9a46c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44089898"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="fde55-103">ASP.NET Core-Webhost</span><span class="sxs-lookup"><span data-stu-id="fde55-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="fde55-104">Von [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="fde55-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="fde55-105">Durch ASP.NET Core-Apps kann ein *Host* gestartet und konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="fde55-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="fde55-106">Der Host ist verantwortlich für das Starten der App und das Verwalten der Lebensdauer.</span><span class="sxs-lookup"><span data-stu-id="fde55-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="fde55-107">Der Host konfiguriert mindestens einen Server und eine Pipeline für die Anforderungsverarbeitung.</span><span class="sxs-lookup"><span data-stu-id="fde55-107">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="fde55-108">In diesem Artikel wird der ASP.NET Core-Webhost ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)) behandelt, der für das Hosten von Web-Apps nützlich ist.</span><span class="sxs-lookup"><span data-stu-id="fde55-108">This topic covers the ASP.NET Core Web Host ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), which is useful for hosting web apps.</span></span> <span data-ttu-id="fde55-109">Weitere Informationen zum generischen .NET-Host ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)) finden Sie unter <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="fde55-109">For coverage of the .NET Generic Host ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), see <xref:fundamentals/host/generic-host>.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="fde55-110">Einrichten eines Hosts</span><span class="sxs-lookup"><span data-stu-id="fde55-110">Set up a host</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="fde55-111">Erstellen Sie einen Host mithilfe einer Instanz von [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="fde55-111">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="fde55-112">Dies wird üblicherweise am Einstiegspunkt der App (die `Main`-Methode) durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="fde55-112">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="fde55-113">In den Projektvorlagen befindet `Main` sich in *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="fde55-113">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="fde55-114">Eine typische *Program.cs*-Datei ruft [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) auf, um die Einrichtung eines Hosts zu starten:</span><span class="sxs-lookup"><span data-stu-id="fde55-114">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>();
}
```

<span data-ttu-id="fde55-115">`CreateDefaultBuilder` führt folgende Ausgaben aus:</span><span class="sxs-lookup"><span data-stu-id="fde55-115">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="fde55-116">Konfiguration von [Kestrel](xref:fundamentals/servers/kestrel) als Webserver und Konfiguration des Servers mit den Hostkonfigurationsanbietern der App.</span><span class="sxs-lookup"><span data-stu-id="fde55-116">Configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and configures the server using the app's hosting configuration providers.</span></span> <span data-ttu-id="fde55-117">Informationen zu den Kestrel-Standardoptionen finden Sie unter <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="fde55-117">For the Kestrel default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="fde55-118">Legt das Inhaltsstammverzeichnis auf den Pfad fest, der von [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory) zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="fde55-118">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="fde55-119">Lädt [Hostkonfiguration](#host-configuration-values) aus:</span><span class="sxs-lookup"><span data-stu-id="fde55-119">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="fde55-120">Umgebungsvariablen mit dem Präfix `ASPNETCORE_` (z.B. `ASPNETCORE_ENVIRONMENT`)</span><span class="sxs-lookup"><span data-stu-id="fde55-120">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="fde55-121">Befehlszeilenargumenten</span><span class="sxs-lookup"><span data-stu-id="fde55-121">Command-line arguments.</span></span>
* <span data-ttu-id="fde55-122">Lädt App-Konfiguration aus:</span><span class="sxs-lookup"><span data-stu-id="fde55-122">Loads app configuration from:</span></span>
  * <span data-ttu-id="fde55-123">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="fde55-123">*appsettings.json*.</span></span>
  * <span data-ttu-id="fde55-124">*appsettings.{Environment}.json*</span><span class="sxs-lookup"><span data-stu-id="fde55-124">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="fde55-125">[Benutzergeheimnissen](xref:security/app-secrets), wenn die App in der `Development`-Umgebung mit der Einstiegsassembly ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="fde55-125">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="fde55-126">Umgebungsvariablen.</span><span class="sxs-lookup"><span data-stu-id="fde55-126">Environment variables.</span></span>
  * <span data-ttu-id="fde55-127">Befehlszeilenargumenten</span><span class="sxs-lookup"><span data-stu-id="fde55-127">Command-line arguments.</span></span>
* <span data-ttu-id="fde55-128">Konfiguriert die [Protokollierung](xref:fundamentals/logging/index) für die Konsolen- und Debugausgabe</span><span class="sxs-lookup"><span data-stu-id="fde55-128">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="fde55-129">Die Protokollierung umfasst Regeln zur [Protokollfilterung](xref:fundamentals/logging/index#log-filtering), die im Abschnitt für die Protokollierungskonfiguration einer *appsettings.json*- oder *appsettings.{Environment}.json*-Datei angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="fde55-129">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="fde55-130">Wenn diese im Hintergrund von IIS ausgeführt wird, aktivieren Sie die [IIS-Integration](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="fde55-130">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="fde55-131">Konfiguriert den Basispfad und den Basisport, dem der Server bei Verwendung des [ASP.NET Core-Moduls](xref:fundamentals/servers/aspnet-core-module) lauscht.</span><span class="sxs-lookup"><span data-stu-id="fde55-131">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="fde55-132">Das Modul erstellt ein Reverseproxy zwischen IIS und Kestrel.</span><span class="sxs-lookup"><span data-stu-id="fde55-132">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="fde55-133">Es konfiguriert ebenfalls die App für das [Erfassen von Startfehlern](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="fde55-133">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="fde55-134">Informationen zu den IIS-Standardoptionen finden Sie unter <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="fde55-134">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>
* <span data-ttu-id="fde55-135">Legt [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) auf `true` fest, wenn die Umgebung der App „Development“ ist.</span><span class="sxs-lookup"><span data-stu-id="fde55-135">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="fde55-136">Weitere Informationen finden Sie unter [Bereichsvalidierung](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="fde55-136">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="fde55-137">Die durch `CreateDefaultBuilder` definierte Konfiguration kann von [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging) und anderen Methoden sowie Erweiterungsmethoden von [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) überschrieben und erweitert werden.</span><span class="sxs-lookup"><span data-stu-id="fde55-137">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="fde55-138">Es folgen einige Beispiele:</span><span class="sxs-lookup"><span data-stu-id="fde55-138">A few examples follow:</span></span>

* <span data-ttu-id="fde55-139">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) wird zum zusätzlichen Festlegen von `IConfiguration` für die App verwendet.</span><span class="sxs-lookup"><span data-stu-id="fde55-139">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="fde55-140">Der folgende `ConfigureAppConfiguration`-Aufruf fügt einen Delegaten hinzu, um die App-Konfiguration in die Datei *appsettings.xml* einzufügen.</span><span class="sxs-lookup"><span data-stu-id="fde55-140">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="fde55-141">`ConfigureAppConfiguration` kann mehrmals aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="fde55-141">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="fde55-142">Beachten Sie, dass diese Konfiguration nicht für den Host gilt (z.B. Server-URLs oder Umgebungen).</span><span class="sxs-lookup"><span data-stu-id="fde55-142">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="fde55-143">Informationen finden Sie im Abschnitt [Hostkonfigurationswerte](#host-configuration-values).</span><span class="sxs-lookup"><span data-stu-id="fde55-143">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="fde55-144">Der folgende `ConfigureLogging`-Aufruf fügt einen Delegaten hinzu, um den Mindestprotokolliergrad ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) auf [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel) zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="fde55-144">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="fde55-145">Diese Einstellung überschreibt die Einstellungen in *appsettings.Development.json* (`LogLevel.Debug`) und *appsettings.Production.json* (`LogLevel.Error`), die durch `CreateDefaultBuilder` konfiguriert wurden.</span><span class="sxs-lookup"><span data-stu-id="fde55-145">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="fde55-146">`ConfigureLogging` kann mehrmals aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="fde55-146">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="fde55-147">Der folgende Aufruf von `ConfigureKestrel` überschreibt den Standardwert von 30.000.000 Bytes von [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize), der festgelegt wurde, als Kestrel durch `CreateDefaultBuilder` konfiguriert wurde:</span><span class="sxs-lookup"><span data-stu-id="fde55-147">The following call to `ConfigureKestrel` overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
        ...
    ```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

* <span data-ttu-id="fde55-148">Der folgende Aufruf von [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) überschreibt den Standardwert von 30.000.000 Bytes von [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize), der festgelegt wurde, als Kestrel durch `CreateDefaultBuilder` konfiguriert wurde:</span><span class="sxs-lookup"><span data-stu-id="fde55-148">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
        ...
    ```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="fde55-149">Das *Inhaltsstammverzeichnis* bestimmt, wo der Host nach Inhaltsdateien (z.B. MVC-Ansichtsdateien) sucht.</span><span class="sxs-lookup"><span data-stu-id="fde55-149">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="fde55-150">Wenn die App aus dem Stammordner des Projekts gestartet wird, wird dieser als Inhaltsstammverzeichnis verwendet.</span><span class="sxs-lookup"><span data-stu-id="fde55-150">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="fde55-151">Dies ist die Standardeinstellung in [Visual Studio](https://www.visualstudio.com/) und für [neue .NET-Vorlagen](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="fde55-151">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="fde55-152">Weitere Informationen zur App-Konfiguration finden Sie unter <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="fde55-152">For more information on app configuration, see <xref:fundamentals/configuration/index>.</span></span>

> [!NOTE]
> <span data-ttu-id="fde55-153">Als Alternative zur Verwendung der statischen `CreateDefaultBuilder`-Methode ist das Erstellen eines Hosts von [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) ein unterstützter Ansatz mit ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="fde55-153">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="fde55-154">Weitere Informationen finden Sie auf der Registerkarte zu ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="fde55-154">For more information, see the ASP.NET Core 1.x tab.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="fde55-155">Erstellen Sie einen Host mithilfe einer Instanz von [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="fde55-155">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="fde55-156">Das Erstellen eines Hosts wird üblicherweise am Einstiegspunkt der App (die `Main`-Methode) durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="fde55-156">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="fde55-157">In den Projektvorlagen befindet `Main` sich in *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="fde55-157">In the project templates, `Main` is located in *Program.cs*:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>()
            .Build();
}
```

<span data-ttu-id="fde55-158">`WebHostBuilder` erfordert einen [Server, der IServer implementiert](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="fde55-158">`WebHostBuilder` requires a [server that implements IServer](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="fde55-159">Bei den integrierten Servern handelt es sich um [Kestrel](xref:fundamentals/servers/kestrel) und [HTTP.sys](xref:fundamentals/servers/httpsys) (vor dem Release von ASP.NET Core 2.0 hieß „HTTP.sys“ [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="fde55-159">The built-in servers are [Kestrel](xref:fundamentals/servers/kestrel) and [HTTP.sys](xref:fundamentals/servers/httpsys) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="fde55-160">In diesem Beispiel wird der Kestrel-Server von der [UseKestrel-Erweiterungsmethode](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) angegeben.</span><span class="sxs-lookup"><span data-stu-id="fde55-160">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="fde55-161">Das *Inhaltsstammverzeichnis* bestimmt, wo der Host nach Inhaltsdateien (z.B. MVC-Ansichtsdateien) sucht.</span><span class="sxs-lookup"><span data-stu-id="fde55-161">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="fde55-162">Das Standardinhaltsstammverzeichnis für `UseContentRoot` wird von [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1) abgerufen.</span><span class="sxs-lookup"><span data-stu-id="fde55-162">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="fde55-163">Wenn die App aus dem Stammordner des Projekts gestartet wird, wird dieser als Inhaltsstammverzeichnis verwendet.</span><span class="sxs-lookup"><span data-stu-id="fde55-163">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="fde55-164">Dies ist die Standardeinstellung in [Visual Studio](https://www.visualstudio.com/) und für [neue .NET-Vorlagen](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="fde55-164">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="fde55-165">Rufen Sie [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) im Rahmen der Erstellung des Hosts auf, um IIS als Reverseproxy zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="fde55-165">To use IIS as a reverse proxy, call [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="fde55-166">`UseIISIntegration` konfiguriert anders als [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) keinen *Server*.</span><span class="sxs-lookup"><span data-stu-id="fde55-166">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="fde55-167">`UseIISIntegration` konfiguriert den Basispfad und den Basisport, dem der Server bei Verwendung des [ASP.NET Core-Moduls](xref:fundamentals/servers/aspnet-core-module) lauscht, um ein Reverseproxy zwischen Kestrel und IIS zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="fde55-167">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="fde55-168">`UseKestrel` und `UseIISIntegration` müssen festgelegt sein, um IIS mit ASP.NET Core verwenden zu können.</span><span class="sxs-lookup"><span data-stu-id="fde55-168">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="fde55-169">Die `UseIISIntegration`-Methode wird nur aktiviert, wenn diese im Hintergrund von IIS oder IIS Express ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="fde55-169">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="fde55-170">Weitere Informationen finden Sie unter <xref:fundamentals/servers/aspnet-core-module> und <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="fde55-170">For more information, see <xref:fundamentals/servers/aspnet-core-module> and <xref:host-and-deploy/aspnet-core-module>.</span></span>

<span data-ttu-id="fde55-171">Eine minimale Implementierung, die einen Host (und eine ASP.NET Core-App) konfiguriert, umfasst das Festlegen eines Servers und die Konfiguration der Anforderungspipeline der App:</span><span class="sxs-lookup"><span data-stu-id="fde55-171">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

::: moniker-end

<span data-ttu-id="fde55-172">Beim Einrichten eines Hosts können die Methoden [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) und [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="fde55-172">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="fde55-173">Wenn eine `Startup`-Klasse angegeben wird, muss diese eine `Configure`-Methode definieren.</span><span class="sxs-lookup"><span data-stu-id="fde55-173">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="fde55-174">Weitere Informationen finden Sie unter <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="fde55-174">For more information, see <xref:fundamentals/startup>.</span></span> <span data-ttu-id="fde55-175">Wenn `ConfigureServices` mehrmals aufgerufen wird, werden diese Aufrufe einander angefügt.</span><span class="sxs-lookup"><span data-stu-id="fde55-175">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="fde55-176">Durch mehrere Aufrufe von `Configure` oder `UseStartup` in `WebHostBuilder` werden die vorherigen Einstellungen ersetzt.</span><span class="sxs-lookup"><span data-stu-id="fde55-176">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="fde55-177">Hostkonfigurationswerte</span><span class="sxs-lookup"><span data-stu-id="fde55-177">Host configuration values</span></span>

<span data-ttu-id="fde55-178">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) basiert auf folgenden Ansätzen, um die Hostkonfigurationswerte festzulegen:</span><span class="sxs-lookup"><span data-stu-id="fde55-178">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="fde55-179">Hostbuilderkonfigurationen, die Umgebungsvariablen im Format `ASPNETCORE_{configurationKey}` enthalten.</span><span class="sxs-lookup"><span data-stu-id="fde55-179">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="fde55-180">Beispielsweise `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="fde55-180">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="fde55-181">Erweiterungen wie [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) und [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (siehe Abschnitt [Überschreiben der Konfiguration](#override-configuration)).</span><span class="sxs-lookup"><span data-stu-id="fde55-181">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="fde55-182">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) und der zugeordnete Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="fde55-182">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="fde55-183">Wenn Sie einen Wert mit `UseSetting` festlegen, wird dieser unabhängig vom Typ als Zeichenfolge festgelegt.</span><span class="sxs-lookup"><span data-stu-id="fde55-183">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="fde55-184">Der Host verwendet die Option, die zuletzt einen Wert festlegt.</span><span class="sxs-lookup"><span data-stu-id="fde55-184">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="fde55-185">Weitere Informationen finden Sie im nächsten Abschnitt unter [Außerkraftsetzen der Konfiguration](#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="fde55-185">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="application-key-name"></a><span data-ttu-id="fde55-186">Anwendungsschlüssel (Name)</span><span class="sxs-lookup"><span data-stu-id="fde55-186">Application Key (Name)</span></span>

<span data-ttu-id="fde55-187">Die Eigenschaft [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) wird automatisch festgelegt, wenn [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) oder [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) während der Hosterstellung aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="fde55-187">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is automatically set when [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) or [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) is called during host construction.</span></span> <span data-ttu-id="fde55-188">Der Wert wird auf den Namen der Assembly festgelegt, die den Einstiegspunkt der App enthält.</span><span class="sxs-lookup"><span data-stu-id="fde55-188">The value is set to the name of the assembly containing the app's entry point.</span></span> <span data-ttu-id="fde55-189">Um den Wert explizit festzulegen, verwenden Sie den [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span><span class="sxs-lookup"><span data-stu-id="fde55-189">To set the value explicitly, use the [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span></span>

<span data-ttu-id="fde55-190">**Schlüssel:** Anwendungsname</span><span class="sxs-lookup"><span data-stu-id="fde55-190">**Key**: applicationName</span></span>  
<span data-ttu-id="fde55-191">**Typ:** *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="fde55-191">**Type**: *string*</span></span>  
<span data-ttu-id="fde55-192">**Standardwert:** Der Name der Assembly, die den Einstiegspunkt der App enthält.</span><span class="sxs-lookup"><span data-stu-id="fde55-192">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="fde55-193">**Festlegen mit:** `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="fde55-193">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="fde55-194">**Umgebungsvariable:** `ASPNETCORE_APPLICATIONKEY`</span><span class="sxs-lookup"><span data-stu-id="fde55-194">**Environment variable**: `ASPNETCORE_APPLICATIONKEY`</span></span>

::: moniker range=">= aspnetcore-2.1"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

::: moniker-end

::: moniker range="< aspnetcore-2.1"

```csharp
var host = new WebHostBuilder()
    .UseSetting("applicationName", "CustomApplicationName")
```

::: moniker-end

### <a name="capture-startup-errors"></a><span data-ttu-id="fde55-195">Erfassen von Startfehlern</span><span class="sxs-lookup"><span data-stu-id="fde55-195">Capture Startup Errors</span></span>

<span data-ttu-id="fde55-196">Diese Einstellung steuert das Erfassen von Startfehlern.</span><span class="sxs-lookup"><span data-stu-id="fde55-196">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="fde55-197">**Schlüssel:** captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="fde55-197">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="fde55-198">**Typ:** *Boolesch* (`true` or `1`)</span><span class="sxs-lookup"><span data-stu-id="fde55-198">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="fde55-199">**Standard:** Die Standardeinstellung ist `false`, wenn die App nicht mit Kestrel im Hintergrund von IIS ausgeführt wird, dann ist diese `true`.</span><span class="sxs-lookup"><span data-stu-id="fde55-199">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="fde55-200">**Festlegen mit:** `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="fde55-200">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="fde55-201">**Umgebungsvariable:** `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="fde55-201">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="fde55-202">Wenn diese `false` ist, führen Fehler beim Start zum Beenden des Hosts.</span><span class="sxs-lookup"><span data-stu-id="fde55-202">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="fde55-203">Wenn diese `true` ist, erfasst der Host Ausnahmen während des Starts und versucht, den Server zu starten.</span><span class="sxs-lookup"><span data-stu-id="fde55-203">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
```

::: moniker-end

### <a name="content-root"></a><span data-ttu-id="fde55-204">Inhaltsstammverzeichnis</span><span class="sxs-lookup"><span data-stu-id="fde55-204">Content Root</span></span>

<span data-ttu-id="fde55-205">Diese Einstellung bestimmt, wo ASP.NET mit der Suche nach Inhaltsdateien (z.B. MVC-Ansichten) beginnt.</span><span class="sxs-lookup"><span data-stu-id="fde55-205">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="fde55-206">**Schlüssel:** contentRoot</span><span class="sxs-lookup"><span data-stu-id="fde55-206">**Key**: contentRoot</span></span>  
<span data-ttu-id="fde55-207">**Typ:** *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="fde55-207">**Type**: *string*</span></span>  
<span data-ttu-id="fde55-208">**Standard:** Entspricht standardmäßig dem Ordner, in dem die App-Assembly gespeichert ist.</span><span class="sxs-lookup"><span data-stu-id="fde55-208">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="fde55-209">**Festlegen mit:** `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="fde55-209">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="fde55-210">**Umgebungsvariable:** `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="fde55-210">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="fde55-211">Das Inhaltsstammverzeichnis wird ebenfalls als Basispfad für die [Webstammeinstellung](#web-root) verwendet.</span><span class="sxs-lookup"><span data-stu-id="fde55-211">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="fde55-212">Wenn der Pfad nicht vorhanden ist, kann der Host nicht gestartet werden.</span><span class="sxs-lookup"><span data-stu-id="fde55-212">If the path doesn't exist, the host fails to start.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\<content-root>")
```

::: moniker-end

### <a name="detailed-errors"></a><span data-ttu-id="fde55-213">Detaillierte Fehler</span><span class="sxs-lookup"><span data-stu-id="fde55-213">Detailed Errors</span></span>

<span data-ttu-id="fde55-214">Bestimmt, ob detaillierte Fehler erfasst werden sollen.</span><span class="sxs-lookup"><span data-stu-id="fde55-214">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="fde55-215">**Schlüssel:** detailedErrors</span><span class="sxs-lookup"><span data-stu-id="fde55-215">**Key**: detailedErrors</span></span>  
<span data-ttu-id="fde55-216">**Typ:** *Boolesch* (`true` or `1`)</span><span class="sxs-lookup"><span data-stu-id="fde55-216">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="fde55-217">**Standard:** FALSE</span><span class="sxs-lookup"><span data-stu-id="fde55-217">**Default**: false</span></span>  
<span data-ttu-id="fde55-218">**Festlegen mit:** `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="fde55-218">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="fde55-219">**Umgebungsvariable:** `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="fde55-219">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="fde55-220">Wenn diese Einstellung aktiviert ist (oder die <a href="#environment">Umgebung</a> auf `Development` festgelegt ist), erfasst die App detaillierte Ausnahmen.</span><span class="sxs-lookup"><span data-stu-id="fde55-220">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

::: moniker-end

### <a name="environment"></a><span data-ttu-id="fde55-221">Umgebung</span><span class="sxs-lookup"><span data-stu-id="fde55-221">Environment</span></span>

<span data-ttu-id="fde55-222">Legt die Umgebung der App fest.</span><span class="sxs-lookup"><span data-stu-id="fde55-222">Sets the app's environment.</span></span>

<span data-ttu-id="fde55-223">**Schlüssel:** environment</span><span class="sxs-lookup"><span data-stu-id="fde55-223">**Key**: environment</span></span>  
<span data-ttu-id="fde55-224">**Typ:** *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="fde55-224">**Type**: *string*</span></span>  
<span data-ttu-id="fde55-225">**Standard:** Produktion</span><span class="sxs-lookup"><span data-stu-id="fde55-225">**Default**: Production</span></span>  
<span data-ttu-id="fde55-226">**Festlegen mit:** `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="fde55-226">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="fde55-227">**Umgebungsvariable:** `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="fde55-227">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="fde55-228">Die Umgebung kann auf einen beliebigen Wert festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="fde55-228">The environment can be set to any value.</span></span> <span data-ttu-id="fde55-229">Zu den durch Frameworks definierten Werten zählen `Development`, `Staging` und `Production`.</span><span class="sxs-lookup"><span data-stu-id="fde55-229">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="fde55-230">Die Groß-/Kleinschreibung wird bei Werten nicht beachtet.</span><span class="sxs-lookup"><span data-stu-id="fde55-230">Values aren't case sensitive.</span></span> <span data-ttu-id="fde55-231">Standardmäßig wird die *Umgebung* aus der `ASPNETCORE_ENVIRONMENT`-Umgebungsvariablen gelesen.</span><span class="sxs-lookup"><span data-stu-id="fde55-231">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="fde55-232">Wenn Sie [Visual Studio](https://www.visualstudio.com/) verwenden, werden Umgebungsvariablen möglicherweise in der Datei *launchSettings.json* festgelegt.</span><span class="sxs-lookup"><span data-stu-id="fde55-232">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="fde55-233">Weitere Informationen finden Sie unter <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="fde55-233">For more information, see <xref:fundamentals/environments>.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseEnvironment(EnvironmentName.Development)
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="fde55-234">Hostingstartassemblys</span><span class="sxs-lookup"><span data-stu-id="fde55-234">Hosting Startup Assemblies</span></span>

<span data-ttu-id="fde55-235">Legt die Hostingstartassemblys der App fest.</span><span class="sxs-lookup"><span data-stu-id="fde55-235">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="fde55-236">**Schlüssel:** hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="fde55-236">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="fde55-237">**Typ:** *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="fde55-237">**Type**: *string*</span></span>  
<span data-ttu-id="fde55-238">**Standard:** Leere Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="fde55-238">**Default**: Empty string</span></span>  
<span data-ttu-id="fde55-239">**Festlegen mit:** `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="fde55-239">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="fde55-240">**Umgebungsvariable:** `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="fde55-240">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="fde55-241">Eine durch Semikolons getrennte Zeichenfolge der Hostingstartassemblys, die beim Start geladen werden soll.</span><span class="sxs-lookup"><span data-stu-id="fde55-241">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span>

<span data-ttu-id="fde55-242">Obwohl der Konfigurationswert standardmäßig auf eine leere Zeichenfolge festgelegt ist, enthalten die Hostingstartassemblys immer die Assembly der App.</span><span class="sxs-lookup"><span data-stu-id="fde55-242">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="fde55-243">Wenn Hostingstartassemblys bereitgestellt werden, werden diese zur Assembly der App hinzugefügt, damit diese geladen werden, wenn die App während des Starts die allgemeinen Dienste erstellt.</span><span class="sxs-lookup"><span data-stu-id="fde55-243">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="https-port"></a><span data-ttu-id="fde55-244">HTTPS-Anschluss</span><span class="sxs-lookup"><span data-stu-id="fde55-244">HTTPS Port</span></span>

<span data-ttu-id="fde55-245">Legen Sie den HTTPS-Umleitungsport fest.</span><span class="sxs-lookup"><span data-stu-id="fde55-245">Set the HTTPS redirect port.</span></span> <span data-ttu-id="fde55-246">Wird in [Erzwingen von HTTPS](xref:security/enforcing-ssl) verwendet.</span><span class="sxs-lookup"><span data-stu-id="fde55-246">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="fde55-247">**Schlüssel**: https_port **Typ**: *string*
**Standard**: Es ist kein Standardwert festgelegt.</span><span class="sxs-lookup"><span data-stu-id="fde55-247">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="fde55-248">**Festlegen mit**: `UseSetting`
**Umgebungsvariable**: `ASPNETCORE_HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="fde55-248">**Set using**: `UseSetting`
**Environment variable**: `ASPNETCORE_HTTPS_PORT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

### <a name="hosting-startup-exclude-assemblies"></a><span data-ttu-id="fde55-249">Auszuschließende Hostingstartassemblys</span><span class="sxs-lookup"><span data-stu-id="fde55-249">Hosting Startup Exclude Assemblies</span></span>

<span data-ttu-id="fde55-250">DESCRIPTION</span><span class="sxs-lookup"><span data-stu-id="fde55-250">DESCRIPTION</span></span>

<span data-ttu-id="fde55-251">**Schlüssel**: hostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="fde55-251">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="fde55-252">**Typ:** *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="fde55-252">**Type**: *string*</span></span>  
<span data-ttu-id="fde55-253">**Standard:** Leere Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="fde55-253">**Default**: Empty string</span></span>  
<span data-ttu-id="fde55-254">**Festlegen mit:** `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="fde55-254">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="fde55-255">**Umgebungsvariable:** `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="fde55-255">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

<span data-ttu-id="fde55-256">Eine durch Semikolons getrennte Zeichenfolge der Hostingstartassemblys, die beim Start ausgeschlossen werden sollen.</span><span class="sxs-lookup"><span data-stu-id="fde55-256">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="prefer-hosting-urls"></a><span data-ttu-id="fde55-257">Bevorzugen von Hosting-URLs</span><span class="sxs-lookup"><span data-stu-id="fde55-257">Prefer Hosting URLs</span></span>

<span data-ttu-id="fde55-258">Gibt an, ob der Hosts den URLs lauschen soll, die mit `WebHostBuilder` konfiguriert wurden, statt denen, die mit der `IServer`-Implementierung konfiguriert wurden.</span><span class="sxs-lookup"><span data-stu-id="fde55-258">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="fde55-259">**Schlüssel:** preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="fde55-259">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="fde55-260">**Typ:** *Boolesch* (`true` or `1`)</span><span class="sxs-lookup"><span data-stu-id="fde55-260">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="fde55-261">**Standard:** TRUE</span><span class="sxs-lookup"><span data-stu-id="fde55-261">**Default**: true</span></span>  
<span data-ttu-id="fde55-262">**Festlegen mit:** `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="fde55-262">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="fde55-263">**Umgebungsvariable:** `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="fde55-263">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="prevent-hosting-startup"></a><span data-ttu-id="fde55-264">Verhindern des Hostingstarts</span><span class="sxs-lookup"><span data-stu-id="fde55-264">Prevent Hosting Startup</span></span>

<span data-ttu-id="fde55-265">Verhindert das automatische Laden der Hostingstartassemblys, einschließlich denen, die von der Assembly der App konfiguriert wurden.</span><span class="sxs-lookup"><span data-stu-id="fde55-265">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="fde55-266">Weitere Informationen finden Sie unter <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="fde55-266">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="fde55-267">**Schlüssel:** preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="fde55-267">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="fde55-268">**Typ:** *Boolesch* (`true` or `1`)</span><span class="sxs-lookup"><span data-stu-id="fde55-268">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="fde55-269">**Standard:** FALSE</span><span class="sxs-lookup"><span data-stu-id="fde55-269">**Default**: false</span></span>  
<span data-ttu-id="fde55-270">**Festlegen mit:** `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="fde55-270">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="fde55-271">**Umgebungsvariable:** `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="fde55-271">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

::: moniker-end

### <a name="server-urls"></a><span data-ttu-id="fde55-272">Server-URLs</span><span class="sxs-lookup"><span data-stu-id="fde55-272">Server URLs</span></span>

<span data-ttu-id="fde55-273">Gibt die IP-Adressen oder Hostadressen mit Ports und Protokollen an, denen der Server für Anforderungen lauschen soll.</span><span class="sxs-lookup"><span data-stu-id="fde55-273">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="fde55-274">**Schlüssel:** urls</span><span class="sxs-lookup"><span data-stu-id="fde55-274">**Key**: urls</span></span>  
<span data-ttu-id="fde55-275">**Typ:** *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="fde55-275">**Type**: *string*</span></span>  
<span data-ttu-id="fde55-276">**Standard**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="fde55-276">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="fde55-277">**Festlegen mit:** `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="fde55-277">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="fde55-278">**Umgebungsvariable:** `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="fde55-278">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="fde55-279">Legt die Einstellung auf eine durch Semikolons (;) getrennte Liste von URL-Präfixen fest, auf die der Server reagieren soll.</span><span class="sxs-lookup"><span data-stu-id="fde55-279">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="fde55-280">Beispielsweise `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="fde55-280">For example, `http://localhost:123`.</span></span> <span data-ttu-id="fde55-281">Verwenden Sie \*, um anzugeben, dass der Server mithilfe des angegebenen Ports und Protokolls (z.B. `http://*:5000`) den Anfragen aller IP-Adressen oder Hostnamen lauschen soll.</span><span class="sxs-lookup"><span data-stu-id="fde55-281">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="fde55-282">Das Protokoll (`http://` oder `https://`) muss in jeder URL enthalten sein.</span><span class="sxs-lookup"><span data-stu-id="fde55-282">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="fde55-283">Die unterstützten Formate unterscheiden sich bei verschiedenen Servern.</span><span class="sxs-lookup"><span data-stu-id="fde55-283">Supported formats vary between servers.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="fde55-284">Kestrel verfügt über eine eigene API für die Endpunktkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="fde55-284">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="fde55-285">Weitere Informationen finden Sie unter <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="fde55-285">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="shutdown-timeout"></a><span data-ttu-id="fde55-286">Timeout beim Herunterfahren</span><span class="sxs-lookup"><span data-stu-id="fde55-286">Shutdown Timeout</span></span>

<span data-ttu-id="fde55-287">Gibt die Wartezeit an, bis der Webhost heruntergefahren wird.</span><span class="sxs-lookup"><span data-stu-id="fde55-287">Specifies the amount of time to wait for the web host to shut down.</span></span>

<span data-ttu-id="fde55-288">**Schlüssel:** shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="fde55-288">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="fde55-289">**Typ:** *Ganze Zahl*</span><span class="sxs-lookup"><span data-stu-id="fde55-289">**Type**: *int*</span></span>  
<span data-ttu-id="fde55-290">**Standard:** 5</span><span class="sxs-lookup"><span data-stu-id="fde55-290">**Default**: 5</span></span>  
<span data-ttu-id="fde55-291">**Festlegen mit:** `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="fde55-291">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="fde55-292">**Umgebungsvariable:** `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="fde55-292">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="fde55-293">Obwohl der Schlüssel eine *ganze Zahl* mit `UseSetting` (z.B. `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`) akzeptiert, akzeptiert die Erweiterungsmethode [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) ein [TimeSpan](/dotnet/api/system.timespan)-Objekt.</span><span class="sxs-lookup"><span data-stu-id="fde55-293">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span>

<span data-ttu-id="fde55-294">Während des Zeitlimits, verhält sich das Hosting folgendermaßen:</span><span class="sxs-lookup"><span data-stu-id="fde55-294">During the timeout period, hosting:</span></span>

* <span data-ttu-id="fde55-295">Es löst [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping) aus.</span><span class="sxs-lookup"><span data-stu-id="fde55-295">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="fde55-296">Es versucht, gehostete Dienste zu beenden und protokolliert Fehler für Dienste, die nicht beendet werden können.</span><span class="sxs-lookup"><span data-stu-id="fde55-296">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="fde55-297">Wenn das Zeitlimit abläuft bevor alle gehosteten Dienste beendet sind, werden alle verbleibenden aktiven Dienste beendet, wenn die App herunterfährt.</span><span class="sxs-lookup"><span data-stu-id="fde55-297">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="fde55-298">Die Dienste werden beendet, selbst wenn die Verarbeitung noch nicht abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="fde55-298">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="fde55-299">Wenn die Dienste mehr Zeit zum Beenden benötigen, sollten Sie das Zeitlimit erhöhen.</span><span class="sxs-lookup"><span data-stu-id="fde55-299">If services require additional time to stop, increase the timeout.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

::: moniker-end

### <a name="startup-assembly"></a><span data-ttu-id="fde55-300">Startassembly</span><span class="sxs-lookup"><span data-stu-id="fde55-300">Startup Assembly</span></span>

<span data-ttu-id="fde55-301">Bestimmt die Assembly, die nach der `Startup`-Klasse suchen soll.</span><span class="sxs-lookup"><span data-stu-id="fde55-301">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="fde55-302">**Schlüssel:** startupAssembly</span><span class="sxs-lookup"><span data-stu-id="fde55-302">**Key**: startupAssembly</span></span>  
<span data-ttu-id="fde55-303">**Typ:** *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="fde55-303">**Type**: *string*</span></span>  
<span data-ttu-id="fde55-304">**Standard:** Die Assembly der App</span><span class="sxs-lookup"><span data-stu-id="fde55-304">**Default**: The app's assembly</span></span>  
<span data-ttu-id="fde55-305">**Festlegen mit:** `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="fde55-305">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="fde55-306">**Umgebungsvariable:** `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="fde55-306">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="fde55-307">Es kann auf die Assembly nach Name (`string`) oder Typ (`TStartup`) verwiesen werden.</span><span class="sxs-lookup"><span data-stu-id="fde55-307">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="fde55-308">Wenn mehrere `UseStartup`-Methoden aufgerufen werden, hat die letzte Vorrang.</span><span class="sxs-lookup"><span data-stu-id="fde55-308">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
```

::: moniker-end

### <a name="web-root"></a><span data-ttu-id="fde55-309">Webstammverzeichnis</span><span class="sxs-lookup"><span data-stu-id="fde55-309">Web Root</span></span>

<span data-ttu-id="fde55-310">Legt den relativen Pfad für die statischen Objekte der App fest.</span><span class="sxs-lookup"><span data-stu-id="fde55-310">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="fde55-311">**Schlüssel:** webroot</span><span class="sxs-lookup"><span data-stu-id="fde55-311">**Key**: webroot</span></span>  
<span data-ttu-id="fde55-312">**Typ:** *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="fde55-312">**Type**: *string*</span></span>  
<span data-ttu-id="fde55-313">**Standard:** Wenn nichts anderes angegeben und der Pfad vorhanden ist, ist der Standardwert „(Content Root)/wwwroot“.</span><span class="sxs-lookup"><span data-stu-id="fde55-313">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="fde55-314">Wenn der Pfad nicht vorhanden ist, wird ein Dateianbieter ohne Funktion verwendet.</span><span class="sxs-lookup"><span data-stu-id="fde55-314">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="fde55-315">**Festlegen mit:** `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="fde55-315">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="fde55-316">**Umgebungsvariable:** `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="fde55-316">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
```

::: moniker-end

## <a name="override-configuration"></a><span data-ttu-id="fde55-317">Außerkraftsetzen der Konfiguration</span><span class="sxs-lookup"><span data-stu-id="fde55-317">Override configuration</span></span>

<span data-ttu-id="fde55-318">Verwenden Sie die [Konfiguration](xref:fundamentals/configuration/index), um den Webhost zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="fde55-318">Use [Configuration](xref:fundamentals/configuration/index) to configure the web host.</span></span> <span data-ttu-id="fde55-319">Im folgenden Beispiel wird die Hostkonfiguration optional in einer *hostsettings.json*-Datei angegeben.</span><span class="sxs-lookup"><span data-stu-id="fde55-319">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="fde55-320">Jede Konfiguration, die aus der Datei *hostsettings.json* geladen wird, kann von Befehlszeilenargumenten überschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="fde55-320">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="fde55-321">Die erstellte Konfiguration (in `config`) wird verwendet, um den Host mit [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="fde55-321">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="fde55-322">Der Konfiguration der App wird die `IWebHostBuilder`-Konfiguration hinzugefügt, das Gegenteil gilt jedoch nicht. `ConfigureAppConfiguration` hat keinen Einfluss auf die `IWebHostBuilder`-Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="fde55-322">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="fde55-323">Das Überschreiben der Konfiguration wird von `UseUrls` bereitgestellt, hierbei kommt die Konfiguration *hostsettings.json* zuerst und anschließend die Konfiguration der Befehlszeilenargumente:</span><span class="sxs-lookup"><span data-stu-id="fde55-323">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hostsettings.json", optional: true)
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

<span data-ttu-id="fde55-324">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="fde55-324">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="fde55-325">Das Überschreiben der Konfiguration wird von `UseUrls` bereitgestellt, hierbei kommt die Konfiguration *hostsettings.json* zuerst und anschließend die Konfiguration der Befehlszeilenargumente:</span><span class="sxs-lookup"><span data-stu-id="fde55-325">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hostsettings.json", optional: true)
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

<span data-ttu-id="fde55-326">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="fde55-326">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

::: moniker-end

> [!NOTE]
> <span data-ttu-id="fde55-327">Die [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration)-Erweiterungsmethode kann derzeit keine Konfigurationsabschnitte (z.B. `.UseConfiguration(Configuration.GetSection("section"))`) analysieren, die von `GetSection` zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="fde55-327">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="fde55-328">Die `GetSection`-Methode filtert die Konfigurationsschlüssel des angeforderten Abschnitts, behält jedoch den Abschnittsnamen in den Schlüsseln bei (z.B. `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="fde55-328">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="fde55-329">Die `UseConfiguration`-Methode erwartet, dass die Schlüssel den `WebHostBuilder`-Schlüsseln entsprechen (z.B. `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="fde55-329">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="fde55-330">Das Vorhandensein des Abschnittsnamens in den Schlüsseln verhindert, dass die Werte des Abschnitts den Host konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="fde55-330">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="fde55-331">Dieses Problem wird in einer bevorstehenden Version behoben.</span><span class="sxs-lookup"><span data-stu-id="fde55-331">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="fde55-332">Weitere Informationen und Problemumgehungen finden Sie unter [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys (Beim Übergeben des Konfigurationsabschnitts an WebHostBuilder.UseConfiguration werden vollständige Schlüssel verwendet)](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="fde55-332">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="fde55-333">`UseConfiguration` kopiert nur Schlüssel aus der bereitgestellten `IConfiguration` in die Konfiguration des Host-Generators.</span><span class="sxs-lookup"><span data-stu-id="fde55-333">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="fde55-334">Daher hat das Festlegen von `reloadOnChange: true` für JSON-, INI- und XML-Einstellungsdateien keine Auswirkung.</span><span class="sxs-lookup"><span data-stu-id="fde55-334">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="fde55-335">Damit der Host auf einer bestimmten URL ausgeführt wird, kann der gewünschte Wert von der Befehlszeile aus übergeben werden, wenn [dotnet run](/dotnet/core/tools/dotnet-run) ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="fde55-335">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="fde55-336">Das Befehlszeilenargument überschreibt den `urls`-Wert der *hostsettings.json*-Datei, und der Server lauscht Port 8080:</span><span class="sxs-lookup"><span data-stu-id="fde55-336">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="fde55-337">Verwalten des Hosts</span><span class="sxs-lookup"><span data-stu-id="fde55-337">Manage the host</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="fde55-338">**Run**</span><span class="sxs-lookup"><span data-stu-id="fde55-338">**Run**</span></span>

<span data-ttu-id="fde55-339">Die `Run`-Methode startet die Web-App und blockiert den aufrufenden Thread, bis der Host heruntergefahren wird:</span><span class="sxs-lookup"><span data-stu-id="fde55-339">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="fde55-340">**Start**</span><span class="sxs-lookup"><span data-stu-id="fde55-340">**Start**</span></span>

<span data-ttu-id="fde55-341">Führen Sie den Host auf nicht blockierende Weise aus, indem Sie dessen `Start`-Methode aufrufen:</span><span class="sxs-lookup"><span data-stu-id="fde55-341">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="fde55-342">Wenn eine Liste mit URLs an die `Start`-Methode übergeben wird, lauscht diese den angegebenen URLs:</span><span class="sxs-lookup"><span data-stu-id="fde55-342">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="fde55-343">Die App kann einen neuen Host, der die vorab konfigurierten Standardwerte von `CreateDefaultBuilder` verwendet, mithilfe einer statischen Hilfsmethode initialisieren und starten.</span><span class="sxs-lookup"><span data-stu-id="fde55-343">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="fde55-344">Diese Methoden starten den Server ohne Konsolenausgabe und mit der [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown)-Methode, die auf eine Unterbrechung wartet (STRG+C/SIGINT oder SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="fde55-344">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="fde55-345">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="fde55-345">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="fde55-346">Beginnend mit einer `RequestDelegate`-Funktion:</span><span class="sxs-lookup"><span data-stu-id="fde55-346">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="fde55-347">Stellen Sie im Browser eine Anforderung an `http://localhost:5000`, um die Antwort „Hello World!“ zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="fde55-347">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="fde55-348">`WaitForShutdown` blockiert, bis eine Unterbrechung (STRG+C/SIGINT oder SIGTERM) ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="fde55-348">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="fde55-349">Die App zeigt die `Console.WriteLine`-Meldung an und wartet, bis diese durch Tastendruck beendet wird.</span><span class="sxs-lookup"><span data-stu-id="fde55-349">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="fde55-350">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="fde55-350">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="fde55-351">Beginnend mit einer URL und `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="fde55-351">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="fde55-352">Erzeugt das gleiche Ergebnis wie **Start(RequestDelegate app)**, mit dem Unterschied, dass die App auf `http://localhost:8080` reagiert.</span><span class="sxs-lookup"><span data-stu-id="fde55-352">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="fde55-353">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="fde55-353">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="fde55-354">Verwenden Sie eine Instanz von `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)), um Routingmiddleware zu verwenden:</span><span class="sxs-lookup"><span data-stu-id="fde55-354">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="fde55-355">Verwenden Sie folgende Browseranforderung mit dem Beispiel:</span><span class="sxs-lookup"><span data-stu-id="fde55-355">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="fde55-356">Anforderung</span><span class="sxs-lookup"><span data-stu-id="fde55-356">Request</span></span>                                    | <span data-ttu-id="fde55-357">Antwort</span><span class="sxs-lookup"><span data-stu-id="fde55-357">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="fde55-358">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="fde55-358">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="fde55-359">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="fde55-359">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="fde55-360">Löst eine Ausnahme mit der Zeichenfolge „ooops!“ aus.</span><span class="sxs-lookup"><span data-stu-id="fde55-360">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="fde55-361">Löst eine Ausnahme mit der Zeichenfolge „Uh oh!“ aus.</span><span class="sxs-lookup"><span data-stu-id="fde55-361">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="fde55-362">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="fde55-362">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="fde55-363">Hello World!</span><span class="sxs-lookup"><span data-stu-id="fde55-363">Hello World!</span></span>                             |

<span data-ttu-id="fde55-364">`WaitForShutdown` blockiert, bis eine Unterbrechung (STRG+C/SIGINT oder SIGTERM) ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="fde55-364">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="fde55-365">Die App zeigt die `Console.WriteLine`-Meldung an und wartet, bis diese durch Tastendruck beendet wird.</span><span class="sxs-lookup"><span data-stu-id="fde55-365">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="fde55-366">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="fde55-366">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="fde55-367">Verwenden Sie eine URL und eine Instanz von `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="fde55-367">Use a URL and an instance of `IRouteBuilder`:</span></span>

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
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="fde55-368">Erzeugt das gleiche Ergebnis wie **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, mit dem Unterschied, dass die App auf `http://localhost:8080` reagiert.</span><span class="sxs-lookup"><span data-stu-id="fde55-368">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="fde55-369">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="fde55-369">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="fde55-370">Stellen Sie einen Delegaten bereit, um eine `IApplicationBuilder`-Schnittstelle zu konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="fde55-370">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="fde55-371">Stellen Sie im Browser eine Anforderung an `http://localhost:5000`, um die Antwort „Hello World!“ zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="fde55-371">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="fde55-372">`WaitForShutdown` blockiert, bis eine Unterbrechung (STRG+C/SIGINT oder SIGTERM) ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="fde55-372">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="fde55-373">Die App zeigt die `Console.WriteLine`-Meldung an und wartet, bis diese durch Tastendruck beendet wird.</span><span class="sxs-lookup"><span data-stu-id="fde55-373">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="fde55-374">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="fde55-374">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="fde55-375">Stellen Sie eine URL und einen Delegaten bereit, um eine `IApplicationBuilder`-Schnittstelle zu konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="fde55-375">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="fde55-376">Erzeugt das gleiche Ergebnis wie **StartWith(Action&lt;IApplicationBuilder&gt; app)**, mit dem Unterschied, dass die App auf `http://localhost:8080` reagiert.</span><span class="sxs-lookup"><span data-stu-id="fde55-376">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="fde55-377">**Run**</span><span class="sxs-lookup"><span data-stu-id="fde55-377">**Run**</span></span>

<span data-ttu-id="fde55-378">Die `Run`-Methode startet die Web-App und blockiert den aufrufenden Thread, bis der Host heruntergefahren wird:</span><span class="sxs-lookup"><span data-stu-id="fde55-378">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="fde55-379">**Start**</span><span class="sxs-lookup"><span data-stu-id="fde55-379">**Start**</span></span>

<span data-ttu-id="fde55-380">Führen Sie den Host auf nicht blockierende Weise aus, indem Sie dessen `Start`-Methode aufrufen:</span><span class="sxs-lookup"><span data-stu-id="fde55-380">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="fde55-381">Wenn eine Liste mit URLs an die `Start`-Methode übergeben wird, lauscht diese den angegebenen URLs:</span><span class="sxs-lookup"><span data-stu-id="fde55-381">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

::: moniker-end

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="fde55-382">IHostingEnvironment-Schnittstelle</span><span class="sxs-lookup"><span data-stu-id="fde55-382">IHostingEnvironment interface</span></span>

<span data-ttu-id="fde55-383">Die [IHostingEnvironment-Schnittstelle](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) stellt Informationen über die Webhostingumgebung der App bereit.</span><span class="sxs-lookup"><span data-stu-id="fde55-383">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="fde55-384">Verwenden Sie [Constructor Injection](xref:fundamentals/dependency-injection) zum Abrufen der `IHostingEnvironment`-Schnittstelle, um deren Eigenschaften und Erweiterungsmethoden zu verwenden:</span><span class="sxs-lookup"><span data-stu-id="fde55-384">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="fde55-385">Ein [konventionsbasierter Ansatz](xref:fundamentals/environments#environment-based-startup-class-and-methods) kann verwendet werden, um die App beim Start je nach Umgebung zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="fde55-385">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="fde55-386">Fügen Sie alternativ die `IHostingEnvironment`-Schnittstelle in den `Startup`-Konstruktor für die Verwendung in `ConfigureServices` ein:</span><span class="sxs-lookup"><span data-stu-id="fde55-386">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="fde55-387">Zusätzlich zur `IsDevelopment`-Erweiterungsmethode stellt die `IHostingEnvironment`-Schnittstelle die `IsStaging`-, `IsProduction`- und `IsEnvironment(string environmentName)`-Methoden bereit.</span><span class="sxs-lookup"><span data-stu-id="fde55-387">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="fde55-388">Weitere Informationen finden Sie unter <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="fde55-388">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="fde55-389">Der `IHostingEnvironment`-Dienst kann ebenfalls direkt in die `Configure`-Methode eingefügt werden, um die Verarbeitungspipeline einzurichten:</span><span class="sxs-lookup"><span data-stu-id="fde55-389">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="fde55-390">`IHostingEnvironment` kann in die `Invoke`-Methode eingefügt werden, wenn eine benutzerdefinierte [Middleware](xref:fundamentals/middleware/index#write-middleware) erstellt wird:</span><span class="sxs-lookup"><span data-stu-id="fde55-390">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#write-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="fde55-391">IApplicationLifetime-Schnittstelle</span><span class="sxs-lookup"><span data-stu-id="fde55-391">IApplicationLifetime interface</span></span>

<span data-ttu-id="fde55-392">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) ermöglicht Aktivitäten nach dem Starten und beim Herunterfahren.</span><span class="sxs-lookup"><span data-stu-id="fde55-392">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="fde55-393">Bei drei der Eigenschaften der Schnittstelle handelt es sich um Abbruchtokens, die zum Registrieren von `Action`-Methoden verwendet werden, durch die Ereignisse beim Starten und Herunterfahren definiert werden.</span><span class="sxs-lookup"><span data-stu-id="fde55-393">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="fde55-394">Abbruchtoken</span><span class="sxs-lookup"><span data-stu-id="fde55-394">Cancellation Token</span></span>    | <span data-ttu-id="fde55-395">Wird ausgelöst, wenn&#8230;</span><span class="sxs-lookup"><span data-stu-id="fde55-395">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="fde55-396">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="fde55-396">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="fde55-397">Der Host vollständig gestartet wurde.</span><span class="sxs-lookup"><span data-stu-id="fde55-397">The host has fully started.</span></span> |
| [<span data-ttu-id="fde55-398">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="fde55-398">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="fde55-399">Der Host ein ordnungsgemäßes Herunterfahren abschließt.</span><span class="sxs-lookup"><span data-stu-id="fde55-399">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="fde55-400">Alle Anforderungen sollten verarbeitet worden sein.</span><span class="sxs-lookup"><span data-stu-id="fde55-400">All requests should be processed.</span></span> <span data-ttu-id="fde55-401">Das Herunterfahren wird blockiert, bis das Ereignis abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="fde55-401">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="fde55-402">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="fde55-402">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="fde55-403">Der Host ein ordnungsgemäßes Herunterfahren ausführt.</span><span class="sxs-lookup"><span data-stu-id="fde55-403">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="fde55-404">Anforderungen möglicherweise noch verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="fde55-404">Requests may still be processing.</span></span> <span data-ttu-id="fde55-405">Das Herunterfahren wird blockiert, bis das Ereignis abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="fde55-405">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="fde55-406">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) fordert das Beenden der Anwendung an.</span><span class="sxs-lookup"><span data-stu-id="fde55-406">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="fde55-407">Die folgende Klasse verwendet `StopApplication`, um eine App ordnungsgemäß herunterzufahren, wenn die `Shutdown`-Methode der Klasse aufgerufen wird:</span><span class="sxs-lookup"><span data-stu-id="fde55-407">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

```csharp
public class MyClass
{
    private readonly IApplicationLifetime _appLifetime;

    public MyClass(IApplicationLifetime appLifetime)
    {
        _appLifetime = appLifetime;
    }

    public void Shutdown()
    {
        _appLifetime.StopApplication();
    }
}
```

## <a name="scope-validation"></a><span data-ttu-id="fde55-408">Bereichsvalidierung</span><span class="sxs-lookup"><span data-stu-id="fde55-408">Scope validation</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="fde55-409">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) legt [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) auf `true` fest, wenn die Umgebung der App „Entwicklung“ ist.</span><span class="sxs-lookup"><span data-stu-id="fde55-409">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

::: moniker-end

<span data-ttu-id="fde55-410">Wenn `ValidateScopes` auf `true` festgelegt wird, führt der Standarddienstanbieter Überprüfungen durch, um sicherzustellen, dass:</span><span class="sxs-lookup"><span data-stu-id="fde55-410">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="fde55-411">Bereichsbezogene Dienste nicht direkt oder indirekt vom Stammdienstanbieter aufgelöst werden</span><span class="sxs-lookup"><span data-stu-id="fde55-411">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="fde55-412">Bereichsbezogene Dienste nicht direkt oder indirekt in Singletons eingefügt werden</span><span class="sxs-lookup"><span data-stu-id="fde55-412">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="fde55-413">Der Stammdienstanbieter wird erstellt, wenn [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="fde55-413">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="fde55-414">Die Lebensdauer des Stammdienstanbieters entspricht der Lebensdauer der App/des Servers, wenn der Anbieter mit der App erstellt wird und verworfen wird, wenn die App beendet wird.</span><span class="sxs-lookup"><span data-stu-id="fde55-414">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="fde55-415">Bereichsbezogene Dienste werden von dem Container verworfen, der sie erstellt hat.</span><span class="sxs-lookup"><span data-stu-id="fde55-415">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="fde55-416">Wenn ein bereichsbezogener Dienst im Stammcontainer erstellt wird, wird die Lebensdauer effektiv auf Singleton heraufgestuft, da er nur vom Stammcontainer verworfen wird, wenn die App/der Server heruntergefahren wird.</span><span class="sxs-lookup"><span data-stu-id="fde55-416">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="fde55-417">Die Überprüfung bereichsbezogener Dienste erfasst diese Situationen, wenn `BuildServiceProvider` aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="fde55-417">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="fde55-418">Konfigurieren Sie [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) mit [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) auf dem Hostbuilder, um Bereiche immer zu validieren, auch in der Produktionsumgebung:</span><span class="sxs-lookup"><span data-stu-id="fde55-418">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

::: moniker range="= aspnetcore-2.0"

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="fde55-419">Problembehandlung bei System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="fde55-419">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="fde55-420">**Folgendes gilt nur für ASP.NET Core 2.0-Apps, wenn die App nicht `UseStartup` oder `Configure` aufruft.**</span><span class="sxs-lookup"><span data-stu-id="fde55-420">**The following only applies to ASP.NET Core 2.0 apps when the app doesn't call `UseStartup` or `Configure`.**</span></span>

<span data-ttu-id="fde55-421">Ein Host kann erstellt werden, indem `IStartup` direkt in den Dependency Injection-Container eingefügt wird, anstatt `UseStartup` oder `Configure` aufzurufen:</span><span class="sxs-lookup"><span data-stu-id="fde55-421">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="fde55-422">Wenn der Host auf diese Weise erstellt wird, kann folgender Fehler auftreten:</span><span class="sxs-lookup"><span data-stu-id="fde55-422">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="fde55-423">Dieser Fehler tritt auf, da der Name der App (der Name der aktuellen Assembly) erforderlich ist, um nach `HostingStartupAttributes` zu suchen.</span><span class="sxs-lookup"><span data-stu-id="fde55-423">This occurs because the app name (the name of the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="fde55-424">Wenn die App `IStartup` manuell in den Dependency Injection-Container eingefügt wird, fügen Sie folgenden Aufruf mit dem angegebenen Assemblynamen zur `WebHostBuilder`-Klasse hinzu:</span><span class="sxs-lookup"><span data-stu-id="fde55-424">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "AssemblyName")
```

<span data-ttu-id="fde55-425">Fügen Sie alternativ eine `Configure`-Dummymethode zur `WebHostBuilder`-Klasse hinzu, wodurch der App-Name automatisch festgelegt wird:</span><span class="sxs-lookup"><span data-stu-id="fde55-425">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the app name automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
```

<span data-ttu-id="fde55-426">Weitere Informationen finden Sie unter [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment) (Ankündigungen: „Microsoft.Extensions.PlatformAbstractions“ wurde entfernt (Kommentar))](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) und im Beispiel für [StartupInjection](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="fde55-426">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="fde55-427">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="fde55-427">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
