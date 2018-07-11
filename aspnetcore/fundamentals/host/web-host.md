---
title: ASP.NET Core-Webhost
author: guardrex
description: Erfahren Sie mehr über den Webhost in ASP.NET Core, der für das Starten der App und das Verwalten der Lebensdauer verantwortlich ist.
ms.author: riande
ms.custom: mvc
ms.date: 06/19/2018
uid: fundamentals/host/web-host
ms.openlocfilehash: 8b72376ae4cb608c4df0cf516288188cff862b36
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894243"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="1445e-103">ASP.NET Core-Webhost</span><span class="sxs-lookup"><span data-stu-id="1445e-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="1445e-104">Von [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1445e-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="1445e-105">Durch ASP.NET Core-Apps kann ein *Host* gestartet und konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="1445e-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="1445e-106">Der Host ist verantwortlich für das Starten der App und das Verwalten der Lebensdauer.</span><span class="sxs-lookup"><span data-stu-id="1445e-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="1445e-107">Der Host konfiguriert mindestens einen Server und eine Pipeline für die Anforderungsverarbeitung.</span><span class="sxs-lookup"><span data-stu-id="1445e-107">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="1445e-108">In diesem Artikel wird der ASP.NET Core-Webhost ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)) behandelt, der für das Hosten von Web-Apps nützlich ist.</span><span class="sxs-lookup"><span data-stu-id="1445e-108">This topic covers the ASP.NET Core Web Host ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), which is useful for hosting web apps.</span></span> <span data-ttu-id="1445e-109">Weitere Informationen zum generischen .NET-Host ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)) finden Sie unter <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="1445e-109">For coverage of the .NET Generic Host ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), see <xref:fundamentals/host/generic-host>.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="1445e-110">Einrichten eines Hosts</span><span class="sxs-lookup"><span data-stu-id="1445e-110">Set up a host</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="1445e-111">Erstellen Sie einen Host mithilfe einer Instanz von [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="1445e-111">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="1445e-112">Dies wird üblicherweise am Einstiegspunkt der App (die `Main`-Methode) durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="1445e-112">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="1445e-113">In den Projektvorlagen befindet `Main` sich in *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="1445e-113">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="1445e-114">Eine typische *Program.cs*-Datei ruft [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) auf, um die Einrichtung eines Hosts zu starten:</span><span class="sxs-lookup"><span data-stu-id="1445e-114">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

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

<span data-ttu-id="1445e-115">`CreateDefaultBuilder` führt folgende Ausgaben aus:</span><span class="sxs-lookup"><span data-stu-id="1445e-115">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="1445e-116">Konfiguration von [Kestrel](xref:fundamentals/servers/kestrel) als Webserver und Konfiguration des Servers mit den Hostkonfigurationsanbietern der App.</span><span class="sxs-lookup"><span data-stu-id="1445e-116">Configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and configures the server using the app's hosting configuration providers.</span></span> <span data-ttu-id="1445e-117">Informationen zu den Kestrel-Standardoptionen finden Sie unter <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="1445e-117">For the Kestrel default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="1445e-118">Legt das Inhaltsstammverzeichnis auf den Pfad fest, der von [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory) zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="1445e-118">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="1445e-119">Lädt [Hostkonfiguration](#host-configuration-values) aus:</span><span class="sxs-lookup"><span data-stu-id="1445e-119">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="1445e-120">Umgebungsvariablen mit dem Präfix `ASPNETCORE_` (z.B. `ASPNETCORE_ENVIRONMENT`)</span><span class="sxs-lookup"><span data-stu-id="1445e-120">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="1445e-121">Befehlszeilenargumenten</span><span class="sxs-lookup"><span data-stu-id="1445e-121">Command-line arguments.</span></span>
* <span data-ttu-id="1445e-122">Lädt App-Konfiguration aus:</span><span class="sxs-lookup"><span data-stu-id="1445e-122">Loads app configuration from:</span></span>
  * <span data-ttu-id="1445e-123">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="1445e-123">*appsettings.json*.</span></span>
  * <span data-ttu-id="1445e-124">*appsettings.{Environment}.json*</span><span class="sxs-lookup"><span data-stu-id="1445e-124">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="1445e-125">[Benutzergeheimnissen](xref:security/app-secrets), wenn die App in der `Development`-Umgebung mit der Einstiegsassembly ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="1445e-125">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="1445e-126">Umgebungsvariablen.</span><span class="sxs-lookup"><span data-stu-id="1445e-126">Environment variables.</span></span>
  * <span data-ttu-id="1445e-127">Befehlszeilenargumenten</span><span class="sxs-lookup"><span data-stu-id="1445e-127">Command-line arguments.</span></span>
* <span data-ttu-id="1445e-128">Konfiguriert die [Protokollierung](xref:fundamentals/logging/index) für die Konsolen- und Debugausgabe</span><span class="sxs-lookup"><span data-stu-id="1445e-128">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="1445e-129">Die Protokollierung umfasst Regeln zur [Protokollfilterung](xref:fundamentals/logging/index#log-filtering), die im Abschnitt für die Protokollierungskonfiguration einer *appsettings.json*- oder *appsettings.{Environment}.json*-Datei angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="1445e-129">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="1445e-130">Wenn diese im Hintergrund von IIS ausgeführt wird, aktivieren Sie die [IIS-Integration](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="1445e-130">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="1445e-131">Konfiguriert den Basispfad und den Basisport, dem der Server bei Verwendung des [ASP.NET Core-Moduls](xref:fundamentals/servers/aspnet-core-module) lauscht.</span><span class="sxs-lookup"><span data-stu-id="1445e-131">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="1445e-132">Das Modul erstellt ein Reverseproxy zwischen IIS und Kestrel.</span><span class="sxs-lookup"><span data-stu-id="1445e-132">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="1445e-133">Es konfiguriert ebenfalls die App für das [Erfassen von Startfehlern](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="1445e-133">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="1445e-134">Informationen zu den IIS-Standardoptionen finden Sie unter <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="1445e-134">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>
* <span data-ttu-id="1445e-135">Legt [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) auf `true` fest, wenn die Umgebung der App „Development“ ist.</span><span class="sxs-lookup"><span data-stu-id="1445e-135">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="1445e-136">Weitere Informationen finden Sie unter [Bereichsvalidierung](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="1445e-136">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="1445e-137">Die durch `CreateDefaultBuilder` definierte Konfiguration kann von [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging) und anderen Methoden sowie Erweiterungsmethoden von [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) überschrieben und erweitert werden.</span><span class="sxs-lookup"><span data-stu-id="1445e-137">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="1445e-138">Es folgen einige Beispiele:</span><span class="sxs-lookup"><span data-stu-id="1445e-138">A few examples follow:</span></span>

* <span data-ttu-id="1445e-139">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) wird zum zusätzlichen Festlegen von `IConfiguration` für die App verwendet.</span><span class="sxs-lookup"><span data-stu-id="1445e-139">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="1445e-140">Der folgende `ConfigureAppConfiguration`-Aufruf fügt einen Delegaten hinzu, um die App-Konfiguration in die Datei *appsettings.xml* einzufügen.</span><span class="sxs-lookup"><span data-stu-id="1445e-140">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="1445e-141">`ConfigureAppConfiguration` kann mehrmals aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="1445e-141">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="1445e-142">Beachten Sie, dass diese Konfiguration nicht für den Host gilt (z.B. Server-URLs oder Umgebungen).</span><span class="sxs-lookup"><span data-stu-id="1445e-142">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="1445e-143">Informationen finden Sie im Abschnitt [Hostkonfigurationswerte](#host-configuration-values).</span><span class="sxs-lookup"><span data-stu-id="1445e-143">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="1445e-144">Der folgende `ConfigureLogging`-Aufruf fügt einen Delegaten hinzu, um den Mindestprotokolliergrad ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) auf [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel) zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="1445e-144">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="1445e-145">Diese Einstellung überschreibt die Einstellungen in *appsettings.Development.json* (`LogLevel.Debug`) und *appsettings.Production.json* (`LogLevel.Error`), die durch `CreateDefaultBuilder` konfiguriert wurden.</span><span class="sxs-lookup"><span data-stu-id="1445e-145">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="1445e-146">`ConfigureLogging` kann mehrmals aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="1445e-146">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

* <span data-ttu-id="1445e-147">Der folgende Aufruf von [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) überschreibt den Standardwert von 30.000.000 Bytes von [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize), der festgelegt wurde, als Kestrel durch `CreateDefaultBuilder` konfiguriert wurde:</span><span class="sxs-lookup"><span data-stu-id="1445e-147">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
        ...
    ```

<span data-ttu-id="1445e-148">Das *Inhaltsstammverzeichnis* bestimmt, wo der Host nach Inhaltsdateien (z.B. MVC-Ansichtsdateien) sucht.</span><span class="sxs-lookup"><span data-stu-id="1445e-148">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="1445e-149">Wenn die App aus dem Stammordner des Projekts gestartet wird, wird dieser als Inhaltsstammverzeichnis verwendet.</span><span class="sxs-lookup"><span data-stu-id="1445e-149">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="1445e-150">Dies ist die Standardeinstellung in [Visual Studio](https://www.visualstudio.com/) und für [neue .NET-Vorlagen](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="1445e-150">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="1445e-151">Weitere Informationen zur App-Konfiguration finden Sie unter <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="1445e-151">For more information on app configuration, see <xref:fundamentals/configuration/index>.</span></span>

> [!NOTE]
> <span data-ttu-id="1445e-152">Als Alternative zur Verwendung der statischen `CreateDefaultBuilder`-Methode ist das Erstellen eines Hosts von [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) ein unterstützter Ansatz mit ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="1445e-152">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="1445e-153">Weitere Informationen finden Sie auf der Registerkarte zu ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="1445e-153">For more information, see the ASP.NET Core 1.x tab.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="1445e-154">Erstellen Sie einen Host mithilfe einer Instanz von [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="1445e-154">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="1445e-155">Das Erstellen eines Hosts wird üblicherweise am Einstiegspunkt der App (die `Main`-Methode) durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="1445e-155">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="1445e-156">In den Projektvorlagen befindet `Main` sich in *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="1445e-156">In the project templates, `Main` is located in *Program.cs*:</span></span>

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

<span data-ttu-id="1445e-157">`WebHostBuilder` erfordert einen [Server, der IServer implementiert](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="1445e-157">`WebHostBuilder` requires a [server that implements IServer](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="1445e-158">Bei den integrierten Servern handelt es sich um [Kestrel](xref:fundamentals/servers/kestrel) und [HTTP.sys](xref:fundamentals/servers/httpsys) (vor dem Release von ASP.NET Core 2.0 hieß „HTTP.sys“ [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="1445e-158">The built-in servers are [Kestrel](xref:fundamentals/servers/kestrel) and [HTTP.sys](xref:fundamentals/servers/httpsys) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="1445e-159">In diesem Beispiel wird der Kestrel-Server von der [UseKestrel-Erweiterungsmethode](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) angegeben.</span><span class="sxs-lookup"><span data-stu-id="1445e-159">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="1445e-160">Das *Inhaltsstammverzeichnis* bestimmt, wo der Host nach Inhaltsdateien (z.B. MVC-Ansichtsdateien) sucht.</span><span class="sxs-lookup"><span data-stu-id="1445e-160">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="1445e-161">Das Standardinhaltsstammverzeichnis für `UseContentRoot` wird von [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1) abgerufen.</span><span class="sxs-lookup"><span data-stu-id="1445e-161">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="1445e-162">Wenn die App aus dem Stammordner des Projekts gestartet wird, wird dieser als Inhaltsstammverzeichnis verwendet.</span><span class="sxs-lookup"><span data-stu-id="1445e-162">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="1445e-163">Dies ist die Standardeinstellung in [Visual Studio](https://www.visualstudio.com/) und für [neue .NET-Vorlagen](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="1445e-163">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="1445e-164">Rufen Sie [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) im Rahmen der Erstellung des Hosts auf, um IIS als Reverseproxy zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="1445e-164">To use IIS as a reverse proxy, call [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="1445e-165">`UseIISIntegration` konfiguriert anders als [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) keinen *Server*.</span><span class="sxs-lookup"><span data-stu-id="1445e-165">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="1445e-166">`UseIISIntegration` konfiguriert den Basispfad und den Basisport, dem der Server bei Verwendung des [ASP.NET Core-Moduls](xref:fundamentals/servers/aspnet-core-module) lauscht, um ein Reverseproxy zwischen Kestrel und IIS zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="1445e-166">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="1445e-167">`UseKestrel` und `UseIISIntegration` müssen festgelegt sein, um IIS mit ASP.NET Core verwenden zu können.</span><span class="sxs-lookup"><span data-stu-id="1445e-167">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="1445e-168">Die `UseIISIntegration`-Methode wird nur aktiviert, wenn diese im Hintergrund von IIS oder IIS Express ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="1445e-168">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="1445e-169">Weitere Informationen finden Sie unter <xref:fundamentals/servers/aspnet-core-module> und <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="1445e-169">For more information, see <xref:fundamentals/servers/aspnet-core-module> and <xref:host-and-deploy/aspnet-core-module>.</span></span>

<span data-ttu-id="1445e-170">Eine minimale Implementierung, die einen Host (und eine ASP.NET Core-App) konfiguriert, umfasst das Festlegen eines Servers und die Konfiguration der Anforderungspipeline der App:</span><span class="sxs-lookup"><span data-stu-id="1445e-170">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="1445e-171">Beim Einrichten eines Hosts können die Methoden [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) und [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="1445e-171">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="1445e-172">Wenn eine `Startup`-Klasse angegeben wird, muss diese eine `Configure`-Methode definieren.</span><span class="sxs-lookup"><span data-stu-id="1445e-172">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="1445e-173">Weitere Informationen finden Sie unter <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="1445e-173">For more information, see <xref:fundamentals/startup>.</span></span> <span data-ttu-id="1445e-174">Wenn `ConfigureServices` mehrmals aufgerufen wird, werden diese Aufrufe einander angefügt.</span><span class="sxs-lookup"><span data-stu-id="1445e-174">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="1445e-175">Durch mehrere Aufrufe von `Configure` oder `UseStartup` in `WebHostBuilder` werden die vorherigen Einstellungen ersetzt.</span><span class="sxs-lookup"><span data-stu-id="1445e-175">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="1445e-176">Hostkonfigurationswerte</span><span class="sxs-lookup"><span data-stu-id="1445e-176">Host configuration values</span></span>

<span data-ttu-id="1445e-177">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) basiert auf folgenden Ansätzen, um die Hostkonfigurationswerte festzulegen:</span><span class="sxs-lookup"><span data-stu-id="1445e-177">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="1445e-178">Hostbuilderkonfigurationen, die Umgebungsvariablen im Format `ASPNETCORE_{configurationKey}` enthalten.</span><span class="sxs-lookup"><span data-stu-id="1445e-178">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="1445e-179">Beispielsweise `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="1445e-179">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="1445e-180">Erweiterungen wie [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) und [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (siehe Abschnitt [Überschreiben der Konfiguration](#override-configuration)).</span><span class="sxs-lookup"><span data-stu-id="1445e-180">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="1445e-181">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) und der zugeordnete Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="1445e-181">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="1445e-182">Wenn Sie einen Wert mit `UseSetting` festlegen, wird dieser unabhängig vom Typ als Zeichenfolge festgelegt.</span><span class="sxs-lookup"><span data-stu-id="1445e-182">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="1445e-183">Der Host verwendet die Option, die zuletzt einen Wert festlegt.</span><span class="sxs-lookup"><span data-stu-id="1445e-183">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="1445e-184">Weitere Informationen finden Sie im nächsten Abschnitt unter [Außerkraftsetzen der Konfiguration](#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="1445e-184">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="application-key-name"></a><span data-ttu-id="1445e-185">Anwendungsschlüssel (Name)</span><span class="sxs-lookup"><span data-stu-id="1445e-185">Application Key (Name)</span></span>

<span data-ttu-id="1445e-186">Die Eigenschaft [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) wird automatisch festgelegt, wenn [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) oder [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) während der Hosterstellung aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="1445e-186">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is automatically set when [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) or [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) is called during host construction.</span></span> <span data-ttu-id="1445e-187">Der Wert wird auf den Namen der Assembly festgelegt, die den Einstiegspunkt der App enthält.</span><span class="sxs-lookup"><span data-stu-id="1445e-187">The value is set to the name of the assembly containing the app's entry point.</span></span> <span data-ttu-id="1445e-188">Um den Wert explizit festzulegen, verwenden Sie den [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span><span class="sxs-lookup"><span data-stu-id="1445e-188">To set the value explicitly, use the [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span></span>

<span data-ttu-id="1445e-189">**Schlüssel:** Anwendungsname</span><span class="sxs-lookup"><span data-stu-id="1445e-189">**Key**: applicationName</span></span>  
<span data-ttu-id="1445e-190">**Typ:** *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="1445e-190">**Type**: *string*</span></span>  
<span data-ttu-id="1445e-191">**Standardwert:** Der Name der Assembly, die den Einstiegspunkt der App enthält.</span><span class="sxs-lookup"><span data-stu-id="1445e-191">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="1445e-192">**Festlegen mit:** `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="1445e-192">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="1445e-193">**Umgebungsvariable:** `ASPNETCORE_APPLICATIONKEY`</span><span class="sxs-lookup"><span data-stu-id="1445e-193">**Environment variable**: `ASPNETCORE_APPLICATIONKEY`</span></span>

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

### <a name="capture-startup-errors"></a><span data-ttu-id="1445e-194">Erfassen von Startfehlern</span><span class="sxs-lookup"><span data-stu-id="1445e-194">Capture Startup Errors</span></span>

<span data-ttu-id="1445e-195">Diese Einstellung steuert das Erfassen von Startfehlern.</span><span class="sxs-lookup"><span data-stu-id="1445e-195">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="1445e-196">**Schlüssel:** captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="1445e-196">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="1445e-197">**Typ:** *Boolesch* (`true` or `1`)</span><span class="sxs-lookup"><span data-stu-id="1445e-197">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="1445e-198">**Standard:** Die Standardeinstellung ist `false`, wenn die App nicht mit Kestrel im Hintergrund von IIS ausgeführt wird, dann ist diese `true`.</span><span class="sxs-lookup"><span data-stu-id="1445e-198">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="1445e-199">**Festlegen mit:** `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="1445e-199">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="1445e-200">**Umgebungsvariable:** `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="1445e-200">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="1445e-201">Wenn diese `false` ist, führen Fehler beim Start zum Beenden des Hosts.</span><span class="sxs-lookup"><span data-stu-id="1445e-201">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="1445e-202">Wenn diese `true` ist, erfasst der Host Ausnahmen während des Starts und versucht, den Server zu starten.</span><span class="sxs-lookup"><span data-stu-id="1445e-202">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

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

### <a name="content-root"></a><span data-ttu-id="1445e-203">Inhaltsstammverzeichnis</span><span class="sxs-lookup"><span data-stu-id="1445e-203">Content Root</span></span>

<span data-ttu-id="1445e-204">Diese Einstellung bestimmt, wo ASP.NET mit der Suche nach Inhaltsdateien (z.B. MVC-Ansichten) beginnt.</span><span class="sxs-lookup"><span data-stu-id="1445e-204">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="1445e-205">**Schlüssel:** contentRoot</span><span class="sxs-lookup"><span data-stu-id="1445e-205">**Key**: contentRoot</span></span>  
<span data-ttu-id="1445e-206">**Typ:** *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="1445e-206">**Type**: *string*</span></span>  
<span data-ttu-id="1445e-207">**Standard:** Entspricht standardmäßig dem Ordner, in dem die App-Assembly gespeichert ist.</span><span class="sxs-lookup"><span data-stu-id="1445e-207">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="1445e-208">**Festlegen mit:** `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="1445e-208">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="1445e-209">**Umgebungsvariable:** `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="1445e-209">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="1445e-210">Das Inhaltsstammverzeichnis wird ebenfalls als Basispfad für die [Webstammeinstellung](#web-root) verwendet.</span><span class="sxs-lookup"><span data-stu-id="1445e-210">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="1445e-211">Wenn der Pfad nicht vorhanden ist, kann der Host nicht gestartet werden.</span><span class="sxs-lookup"><span data-stu-id="1445e-211">If the path doesn't exist, the host fails to start.</span></span>

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

### <a name="detailed-errors"></a><span data-ttu-id="1445e-212">Detaillierte Fehler</span><span class="sxs-lookup"><span data-stu-id="1445e-212">Detailed Errors</span></span>

<span data-ttu-id="1445e-213">Bestimmt, ob detaillierte Fehler erfasst werden sollen.</span><span class="sxs-lookup"><span data-stu-id="1445e-213">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="1445e-214">**Schlüssel:** detailedErrors</span><span class="sxs-lookup"><span data-stu-id="1445e-214">**Key**: detailedErrors</span></span>  
<span data-ttu-id="1445e-215">**Typ:** *Boolesch* (`true` or `1`)</span><span class="sxs-lookup"><span data-stu-id="1445e-215">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="1445e-216">**Standard:** FALSE</span><span class="sxs-lookup"><span data-stu-id="1445e-216">**Default**: false</span></span>  
<span data-ttu-id="1445e-217">**Festlegen mit:** `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="1445e-217">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="1445e-218">**Umgebungsvariable:** `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="1445e-218">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="1445e-219">Wenn diese Einstellung aktiviert ist (oder die <a href="#environment">Umgebung</a> auf `Development` festgelegt ist), erfasst die App detaillierte Ausnahmen.</span><span class="sxs-lookup"><span data-stu-id="1445e-219">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

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

### <a name="environment"></a><span data-ttu-id="1445e-220">Umgebung</span><span class="sxs-lookup"><span data-stu-id="1445e-220">Environment</span></span>

<span data-ttu-id="1445e-221">Legt die Umgebung der App fest.</span><span class="sxs-lookup"><span data-stu-id="1445e-221">Sets the app's environment.</span></span>

<span data-ttu-id="1445e-222">**Schlüssel:** environment</span><span class="sxs-lookup"><span data-stu-id="1445e-222">**Key**: environment</span></span>  
<span data-ttu-id="1445e-223">**Typ:** *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="1445e-223">**Type**: *string*</span></span>  
<span data-ttu-id="1445e-224">**Standard:** Produktion</span><span class="sxs-lookup"><span data-stu-id="1445e-224">**Default**: Production</span></span>  
<span data-ttu-id="1445e-225">**Festlegen mit:** `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="1445e-225">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="1445e-226">**Umgebungsvariable:** `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="1445e-226">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="1445e-227">Die Umgebung kann auf einen beliebigen Wert festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="1445e-227">The environment can be set to any value.</span></span> <span data-ttu-id="1445e-228">Zu den durch Frameworks definierten Werten zählen `Development`, `Staging` und `Production`.</span><span class="sxs-lookup"><span data-stu-id="1445e-228">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="1445e-229">Die Groß-/Kleinschreibung wird bei Werten nicht beachtet.</span><span class="sxs-lookup"><span data-stu-id="1445e-229">Values aren't case sensitive.</span></span> <span data-ttu-id="1445e-230">Standardmäßig wird die *Umgebung* aus der `ASPNETCORE_ENVIRONMENT`-Umgebungsvariablen gelesen.</span><span class="sxs-lookup"><span data-stu-id="1445e-230">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="1445e-231">Wenn Sie [Visual Studio](https://www.visualstudio.com/) verwenden, werden Umgebungsvariablen möglicherweise in der Datei *launchSettings.json* festgelegt.</span><span class="sxs-lookup"><span data-stu-id="1445e-231">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="1445e-232">Weitere Informationen finden Sie unter <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="1445e-232">For more information, see <xref:fundamentals/environments>.</span></span>

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

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="1445e-233">Hostingstartassemblys</span><span class="sxs-lookup"><span data-stu-id="1445e-233">Hosting Startup Assemblies</span></span>

<span data-ttu-id="1445e-234">Legt die Hostingstartassemblys der App fest.</span><span class="sxs-lookup"><span data-stu-id="1445e-234">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="1445e-235">**Schlüssel:** hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="1445e-235">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="1445e-236">**Typ:** *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="1445e-236">**Type**: *string*</span></span>  
<span data-ttu-id="1445e-237">**Standard:** Leere Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="1445e-237">**Default**: Empty string</span></span>  
<span data-ttu-id="1445e-238">**Festlegen mit:** `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="1445e-238">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="1445e-239">**Umgebungsvariable:** `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="1445e-239">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="1445e-240">Eine durch Semikolons getrennte Zeichenfolge der Hostingstartassemblys, die beim Start geladen werden soll.</span><span class="sxs-lookup"><span data-stu-id="1445e-240">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span>

<span data-ttu-id="1445e-241">Obwohl der Konfigurationswert standardmäßig auf eine leere Zeichenfolge festgelegt ist, enthalten die Hostingstartassemblys immer die Assembly der App.</span><span class="sxs-lookup"><span data-stu-id="1445e-241">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="1445e-242">Wenn Hostingstartassemblys bereitgestellt werden, werden diese zur Assembly der App hinzugefügt, damit diese geladen werden, wenn die App während des Starts die allgemeinen Dienste erstellt.</span><span class="sxs-lookup"><span data-stu-id="1445e-242">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="prefer-hosting-urls"></a><span data-ttu-id="1445e-243">Bevorzugen von Hosting-URLs</span><span class="sxs-lookup"><span data-stu-id="1445e-243">Prefer Hosting URLs</span></span>

<span data-ttu-id="1445e-244">Gibt an, ob der Hosts den URLs lauschen soll, die mit `WebHostBuilder` konfiguriert wurden, statt denen, die mit der `IServer`-Implementierung konfiguriert wurden.</span><span class="sxs-lookup"><span data-stu-id="1445e-244">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="1445e-245">**Schlüssel:** preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="1445e-245">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="1445e-246">**Typ:** *Boolesch* (`true` or `1`)</span><span class="sxs-lookup"><span data-stu-id="1445e-246">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="1445e-247">**Standard:** TRUE</span><span class="sxs-lookup"><span data-stu-id="1445e-247">**Default**: true</span></span>  
<span data-ttu-id="1445e-248">**Festlegen mit:** `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="1445e-248">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="1445e-249">**Umgebungsvariable:** `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="1445e-249">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="prevent-hosting-startup"></a><span data-ttu-id="1445e-250">Verhindern des Hostingstarts</span><span class="sxs-lookup"><span data-stu-id="1445e-250">Prevent Hosting Startup</span></span>

<span data-ttu-id="1445e-251">Verhindert das automatische Laden der Hostingstartassemblys, einschließlich denen, die von der Assembly der App konfiguriert wurden.</span><span class="sxs-lookup"><span data-stu-id="1445e-251">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="1445e-252">Weitere Informationen finden Sie unter <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="1445e-252">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="1445e-253">**Schlüssel:** preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="1445e-253">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="1445e-254">**Typ:** *Boolesch* (`true` or `1`)</span><span class="sxs-lookup"><span data-stu-id="1445e-254">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="1445e-255">**Standard:** FALSE</span><span class="sxs-lookup"><span data-stu-id="1445e-255">**Default**: false</span></span>  
<span data-ttu-id="1445e-256">**Festlegen mit:** `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="1445e-256">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="1445e-257">**Umgebungsvariable:** `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="1445e-257">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

::: moniker-end

### <a name="server-urls"></a><span data-ttu-id="1445e-258">Server-URLs</span><span class="sxs-lookup"><span data-stu-id="1445e-258">Server URLs</span></span>

<span data-ttu-id="1445e-259">Gibt die IP-Adressen oder Hostadressen mit Ports und Protokollen an, denen der Server für Anforderungen lauschen soll.</span><span class="sxs-lookup"><span data-stu-id="1445e-259">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="1445e-260">**Schlüssel:** urls</span><span class="sxs-lookup"><span data-stu-id="1445e-260">**Key**: urls</span></span>  
<span data-ttu-id="1445e-261">**Typ:** *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="1445e-261">**Type**: *string*</span></span>  
<span data-ttu-id="1445e-262">**Standard**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="1445e-262">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="1445e-263">**Festlegen mit:** `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="1445e-263">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="1445e-264">**Umgebungsvariable:** `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="1445e-264">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="1445e-265">Legt die Einstellung auf eine durch Semikolons (;) getrennte Liste von URL-Präfixen fest, auf die der Server reagieren soll.</span><span class="sxs-lookup"><span data-stu-id="1445e-265">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="1445e-266">Beispielsweise `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="1445e-266">For example, `http://localhost:123`.</span></span> <span data-ttu-id="1445e-267">Verwenden Sie \*, um anzugeben, dass der Server mithilfe des angegebenen Ports und Protokolls (z.B. `http://*:5000`) den Anfragen aller IP-Adressen oder Hostnamen lauschen soll.</span><span class="sxs-lookup"><span data-stu-id="1445e-267">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="1445e-268">Das Protokoll (`http://` oder `https://`) muss in jeder URL enthalten sein.</span><span class="sxs-lookup"><span data-stu-id="1445e-268">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="1445e-269">Die unterstützten Formate unterscheiden sich bei verschiedenen Servern.</span><span class="sxs-lookup"><span data-stu-id="1445e-269">Supported formats vary between servers.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="1445e-270">Kestrel verfügt über eine eigene API für die Endpunktkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="1445e-270">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="1445e-271">Weitere Informationen finden Sie unter <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="1445e-271">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="shutdown-timeout"></a><span data-ttu-id="1445e-272">Timeout beim Herunterfahren</span><span class="sxs-lookup"><span data-stu-id="1445e-272">Shutdown Timeout</span></span>

<span data-ttu-id="1445e-273">Gibt die Wartezeit an, bis der Webhost heruntergefahren wird.</span><span class="sxs-lookup"><span data-stu-id="1445e-273">Specifies the amount of time to wait for the web host to shut down.</span></span>

<span data-ttu-id="1445e-274">**Schlüssel:** shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="1445e-274">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="1445e-275">**Typ:** *Ganze Zahl*</span><span class="sxs-lookup"><span data-stu-id="1445e-275">**Type**: *int*</span></span>  
<span data-ttu-id="1445e-276">**Standard:** 5</span><span class="sxs-lookup"><span data-stu-id="1445e-276">**Default**: 5</span></span>  
<span data-ttu-id="1445e-277">**Festlegen mit:** `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="1445e-277">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="1445e-278">**Umgebungsvariable:** `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="1445e-278">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="1445e-279">Obwohl der Schlüssel eine *ganze Zahl* mit `UseSetting` (z.B. `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`) akzeptiert, akzeptiert die Erweiterungsmethode [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) ein [TimeSpan](/dotnet/api/system.timespan)-Objekt.</span><span class="sxs-lookup"><span data-stu-id="1445e-279">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span>

<span data-ttu-id="1445e-280">Während des Zeitlimits, verhält sich das Hosting folgendermaßen:</span><span class="sxs-lookup"><span data-stu-id="1445e-280">During the timeout period, hosting:</span></span>

* <span data-ttu-id="1445e-281">Es löst [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping) aus.</span><span class="sxs-lookup"><span data-stu-id="1445e-281">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="1445e-282">Es versucht, gehostete Dienste zu beenden und protokolliert Fehler für Dienste, die nicht beendet werden können.</span><span class="sxs-lookup"><span data-stu-id="1445e-282">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="1445e-283">Wenn das Zeitlimit abläuft bevor alle gehosteten Dienste beendet sind, werden alle verbleibenden aktiven Dienste beendet, wenn die App herunterfährt.</span><span class="sxs-lookup"><span data-stu-id="1445e-283">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="1445e-284">Die Dienste werden beendet, selbst wenn die Verarbeitung noch nicht abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="1445e-284">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="1445e-285">Wenn die Dienste mehr Zeit zum Beenden benötigen, sollten Sie das Zeitlimit erhöhen.</span><span class="sxs-lookup"><span data-stu-id="1445e-285">If services require additional time to stop, increase the timeout.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

::: moniker-end

### <a name="startup-assembly"></a><span data-ttu-id="1445e-286">Startassembly</span><span class="sxs-lookup"><span data-stu-id="1445e-286">Startup Assembly</span></span>

<span data-ttu-id="1445e-287">Bestimmt die Assembly, die nach der `Startup`-Klasse suchen soll.</span><span class="sxs-lookup"><span data-stu-id="1445e-287">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="1445e-288">**Schlüssel:** startupAssembly</span><span class="sxs-lookup"><span data-stu-id="1445e-288">**Key**: startupAssembly</span></span>  
<span data-ttu-id="1445e-289">**Typ:** *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="1445e-289">**Type**: *string*</span></span>  
<span data-ttu-id="1445e-290">**Standard:** Die Assembly der App</span><span class="sxs-lookup"><span data-stu-id="1445e-290">**Default**: The app's assembly</span></span>  
<span data-ttu-id="1445e-291">**Festlegen mit:** `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="1445e-291">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="1445e-292">**Umgebungsvariable:** `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="1445e-292">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="1445e-293">Es kann auf die Assembly nach Name (`string`) oder Typ (`TStartup`) verwiesen werden.</span><span class="sxs-lookup"><span data-stu-id="1445e-293">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="1445e-294">Wenn mehrere `UseStartup`-Methoden aufgerufen werden, hat die letzte Vorrang.</span><span class="sxs-lookup"><span data-stu-id="1445e-294">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

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

### <a name="web-root"></a><span data-ttu-id="1445e-295">Webstammverzeichnis</span><span class="sxs-lookup"><span data-stu-id="1445e-295">Web Root</span></span>

<span data-ttu-id="1445e-296">Legt den relativen Pfad für die statischen Objekte der App fest.</span><span class="sxs-lookup"><span data-stu-id="1445e-296">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="1445e-297">**Schlüssel:** webroot</span><span class="sxs-lookup"><span data-stu-id="1445e-297">**Key**: webroot</span></span>  
<span data-ttu-id="1445e-298">**Typ:** *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="1445e-298">**Type**: *string*</span></span>  
<span data-ttu-id="1445e-299">**Standard:** Wenn nichts anderes angegeben und der Pfad vorhanden ist, ist der Standardwert „(Content Root)/wwwroot“.</span><span class="sxs-lookup"><span data-stu-id="1445e-299">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="1445e-300">Wenn der Pfad nicht vorhanden ist, wird ein Dateianbieter ohne Funktion verwendet.</span><span class="sxs-lookup"><span data-stu-id="1445e-300">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="1445e-301">**Festlegen mit:** `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="1445e-301">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="1445e-302">**Umgebungsvariable:** `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="1445e-302">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

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

## <a name="override-configuration"></a><span data-ttu-id="1445e-303">Außerkraftsetzen der Konfiguration</span><span class="sxs-lookup"><span data-stu-id="1445e-303">Override configuration</span></span>

<span data-ttu-id="1445e-304">Verwenden Sie die [Konfiguration](xref:fundamentals/configuration/index), um den Webhost zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="1445e-304">Use [Configuration](xref:fundamentals/configuration/index) to configure the web host.</span></span> <span data-ttu-id="1445e-305">Im folgenden Beispiel wird die Hostkonfiguration optional in einer *hostsettings.json*-Datei angegeben.</span><span class="sxs-lookup"><span data-stu-id="1445e-305">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="1445e-306">Jede Konfiguration, die aus der Datei *hostsettings.json* geladen wird, kann von Befehlszeilenargumenten überschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="1445e-306">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="1445e-307">Die erstellte Konfiguration (in `config`) wird verwendet, um den Host mit [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="1445e-307">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="1445e-308">Der Konfiguration der App wird die `IWebHostBuilder`-Konfiguration hinzugefügt, das Gegenteil gilt jedoch nicht. `ConfigureAppConfiguration` hat keinen Einfluss auf die `IWebHostBuilder`-Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="1445e-308">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="1445e-309">Das Überschreiben der Konfiguration wird von `UseUrls` bereitgestellt, hierbei kommt die Konfiguration *hostsettings.json* zuerst und anschließend die Konfiguration der Befehlszeilenargumente:</span><span class="sxs-lookup"><span data-stu-id="1445e-309">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

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

<span data-ttu-id="1445e-310">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="1445e-310">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="1445e-311">Das Überschreiben der Konfiguration wird von `UseUrls` bereitgestellt, hierbei kommt die Konfiguration *hostsettings.json* zuerst und anschließend die Konfiguration der Befehlszeilenargumente:</span><span class="sxs-lookup"><span data-stu-id="1445e-311">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

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

<span data-ttu-id="1445e-312">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="1445e-312">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

::: moniker-end

> [!NOTE]
> <span data-ttu-id="1445e-313">Die [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration)-Erweiterungsmethode kann derzeit keine Konfigurationsabschnitte (z.B. `.UseConfiguration(Configuration.GetSection("section"))`) analysieren, die von `GetSection` zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="1445e-313">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="1445e-314">Die `GetSection`-Methode filtert die Konfigurationsschlüssel des angeforderten Abschnitts, behält jedoch den Abschnittsnamen in den Schlüsseln bei (z.B. `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="1445e-314">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="1445e-315">Die `UseConfiguration`-Methode erwartet, dass die Schlüssel den `WebHostBuilder`-Schlüsseln entsprechen (z.B. `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="1445e-315">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="1445e-316">Das Vorhandensein des Abschnittsnamens in den Schlüsseln verhindert, dass die Werte des Abschnitts den Host konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="1445e-316">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="1445e-317">Dieses Problem wird in einer bevorstehenden Version behoben.</span><span class="sxs-lookup"><span data-stu-id="1445e-317">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="1445e-318">Weitere Informationen und Problemumgehungen finden Sie unter [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys (Beim Übergeben des Konfigurationsabschnitts an WebHostBuilder.UseConfiguration werden vollständige Schlüssel verwendet)](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="1445e-318">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="1445e-319">`UseConfiguration` kopiert nur Schlüssel aus der bereitgestellten `IConfiguration` in die Konfiguration des Host-Generators.</span><span class="sxs-lookup"><span data-stu-id="1445e-319">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="1445e-320">Daher hat das Festlegen von `reloadOnChange: true` für JSON-, INI- und XML-Einstellungsdateien keine Auswirkung.</span><span class="sxs-lookup"><span data-stu-id="1445e-320">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="1445e-321">Damit der Host auf einer bestimmten URL ausgeführt wird, kann der gewünschte Wert von der Befehlszeile aus übergeben werden, wenn [dotnet run](/dotnet/core/tools/dotnet-run) ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="1445e-321">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="1445e-322">Das Befehlszeilenargument überschreibt den `urls`-Wert der *hostsettings.json*-Datei, und der Server lauscht Port 8080:</span><span class="sxs-lookup"><span data-stu-id="1445e-322">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="1445e-323">Verwalten des Hosts</span><span class="sxs-lookup"><span data-stu-id="1445e-323">Manage the host</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="1445e-324">**Run**</span><span class="sxs-lookup"><span data-stu-id="1445e-324">**Run**</span></span>

<span data-ttu-id="1445e-325">Die `Run`-Methode startet die Web-App und blockiert den aufrufenden Thread, bis der Host heruntergefahren wird:</span><span class="sxs-lookup"><span data-stu-id="1445e-325">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="1445e-326">**Start**</span><span class="sxs-lookup"><span data-stu-id="1445e-326">**Start**</span></span>

<span data-ttu-id="1445e-327">Führen Sie den Host auf nicht blockierende Weise aus, indem Sie dessen `Start`-Methode aufrufen:</span><span class="sxs-lookup"><span data-stu-id="1445e-327">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="1445e-328">Wenn eine Liste mit URLs an die `Start`-Methode übergeben wird, lauscht diese den angegebenen URLs:</span><span class="sxs-lookup"><span data-stu-id="1445e-328">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="1445e-329">Die App kann einen neuen Host, der die vorab konfigurierten Standardwerte von `CreateDefaultBuilder` verwendet, mithilfe einer statischen Hilfsmethode initialisieren und starten.</span><span class="sxs-lookup"><span data-stu-id="1445e-329">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="1445e-330">Diese Methoden starten den Server ohne Konsolenausgabe und mit der [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown)-Methode, die auf eine Unterbrechung wartet (STRG+C/SIGINT oder SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="1445e-330">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="1445e-331">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="1445e-331">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="1445e-332">Beginnend mit einer `RequestDelegate`-Funktion:</span><span class="sxs-lookup"><span data-stu-id="1445e-332">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="1445e-333">Stellen Sie im Browser eine Anforderung an `http://localhost:5000`, um die Antwort „Hello World!“ zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="1445e-333">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="1445e-334">`WaitForShutdown` blockiert, bis eine Unterbrechung (STRG+C/SIGINT oder SIGTERM) ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="1445e-334">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="1445e-335">Die App zeigt die `Console.WriteLine`-Meldung an und wartet, bis diese durch Tastendruck beendet wird.</span><span class="sxs-lookup"><span data-stu-id="1445e-335">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="1445e-336">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="1445e-336">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="1445e-337">Beginnend mit einer URL und `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="1445e-337">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="1445e-338">Erzeugt das gleiche Ergebnis wie **Start(RequestDelegate app)**, mit dem Unterschied, dass die App auf `http://localhost:8080` reagiert.</span><span class="sxs-lookup"><span data-stu-id="1445e-338">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="1445e-339">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="1445e-339">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="1445e-340">Verwenden Sie eine Instanz von `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)), um Routingmiddleware zu verwenden:</span><span class="sxs-lookup"><span data-stu-id="1445e-340">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="1445e-341">Verwenden Sie folgende Browseranforderung mit dem Beispiel:</span><span class="sxs-lookup"><span data-stu-id="1445e-341">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="1445e-342">Anforderung</span><span class="sxs-lookup"><span data-stu-id="1445e-342">Request</span></span>                                    | <span data-ttu-id="1445e-343">Antwort</span><span class="sxs-lookup"><span data-stu-id="1445e-343">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="1445e-344">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="1445e-344">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="1445e-345">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="1445e-345">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="1445e-346">Löst eine Ausnahme mit der Zeichenfolge „ooops!“ aus.</span><span class="sxs-lookup"><span data-stu-id="1445e-346">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="1445e-347">Löst eine Ausnahme mit der Zeichenfolge „Uh oh!“ aus.</span><span class="sxs-lookup"><span data-stu-id="1445e-347">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="1445e-348">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="1445e-348">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="1445e-349">Hello World!</span><span class="sxs-lookup"><span data-stu-id="1445e-349">Hello World!</span></span>                             |

<span data-ttu-id="1445e-350">`WaitForShutdown` blockiert, bis eine Unterbrechung (STRG+C/SIGINT oder SIGTERM) ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="1445e-350">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="1445e-351">Die App zeigt die `Console.WriteLine`-Meldung an und wartet, bis diese durch Tastendruck beendet wird.</span><span class="sxs-lookup"><span data-stu-id="1445e-351">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="1445e-352">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="1445e-352">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="1445e-353">Verwenden Sie eine URL und eine Instanz von `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="1445e-353">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="1445e-354">Erzeugt das gleiche Ergebnis wie **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, mit dem Unterschied, dass die App auf `http://localhost:8080` reagiert.</span><span class="sxs-lookup"><span data-stu-id="1445e-354">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="1445e-355">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="1445e-355">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="1445e-356">Stellen Sie einen Delegaten bereit, um eine `IApplicationBuilder`-Schnittstelle zu konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="1445e-356">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="1445e-357">Stellen Sie im Browser eine Anforderung an `http://localhost:5000`, um die Antwort „Hello World!“ zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="1445e-357">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="1445e-358">`WaitForShutdown` blockiert, bis eine Unterbrechung (STRG+C/SIGINT oder SIGTERM) ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="1445e-358">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="1445e-359">Die App zeigt die `Console.WriteLine`-Meldung an und wartet, bis diese durch Tastendruck beendet wird.</span><span class="sxs-lookup"><span data-stu-id="1445e-359">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="1445e-360">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="1445e-360">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="1445e-361">Stellen Sie eine URL und einen Delegaten bereit, um eine `IApplicationBuilder`-Schnittstelle zu konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="1445e-361">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="1445e-362">Erzeugt das gleiche Ergebnis wie **StartWith(Action&lt;IApplicationBuilder&gt; app)**, mit dem Unterschied, dass die App auf `http://localhost:8080` reagiert.</span><span class="sxs-lookup"><span data-stu-id="1445e-362">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="1445e-363">**Run**</span><span class="sxs-lookup"><span data-stu-id="1445e-363">**Run**</span></span>

<span data-ttu-id="1445e-364">Die `Run`-Methode startet die Web-App und blockiert den aufrufenden Thread, bis der Host heruntergefahren wird:</span><span class="sxs-lookup"><span data-stu-id="1445e-364">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="1445e-365">**Start**</span><span class="sxs-lookup"><span data-stu-id="1445e-365">**Start**</span></span>

<span data-ttu-id="1445e-366">Führen Sie den Host auf nicht blockierende Weise aus, indem Sie dessen `Start`-Methode aufrufen:</span><span class="sxs-lookup"><span data-stu-id="1445e-366">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="1445e-367">Wenn eine Liste mit URLs an die `Start`-Methode übergeben wird, lauscht diese den angegebenen URLs:</span><span class="sxs-lookup"><span data-stu-id="1445e-367">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="1445e-368">IHostingEnvironment-Schnittstelle</span><span class="sxs-lookup"><span data-stu-id="1445e-368">IHostingEnvironment interface</span></span>

<span data-ttu-id="1445e-369">Die [IHostingEnvironment-Schnittstelle](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) stellt Informationen über die Webhostingumgebung der App bereit.</span><span class="sxs-lookup"><span data-stu-id="1445e-369">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="1445e-370">Verwenden Sie [Constructor Injection](xref:fundamentals/dependency-injection) zum Abrufen der `IHostingEnvironment`-Schnittstelle, um deren Eigenschaften und Erweiterungsmethoden zu verwenden:</span><span class="sxs-lookup"><span data-stu-id="1445e-370">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="1445e-371">Ein [konventionsbasierter Ansatz](xref:fundamentals/environments#environment-based-startup-class-and-methods) kann verwendet werden, um die App beim Start je nach Umgebung zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="1445e-371">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="1445e-372">Fügen Sie alternativ die `IHostingEnvironment`-Schnittstelle in den `Startup`-Konstruktor für die Verwendung in `ConfigureServices` ein:</span><span class="sxs-lookup"><span data-stu-id="1445e-372">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="1445e-373">Zusätzlich zur `IsDevelopment`-Erweiterungsmethode stellt die `IHostingEnvironment`-Schnittstelle die `IsStaging`-, `IsProduction`- und `IsEnvironment(string environmentName)`-Methoden bereit.</span><span class="sxs-lookup"><span data-stu-id="1445e-373">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="1445e-374">Weitere Informationen finden Sie unter <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="1445e-374">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="1445e-375">Der `IHostingEnvironment`-Dienst kann ebenfalls direkt in die `Configure`-Methode eingefügt werden, um die Verarbeitungspipeline einzurichten:</span><span class="sxs-lookup"><span data-stu-id="1445e-375">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="1445e-376">`IHostingEnvironment` kann in die `Invoke`-Methode eingefügt werden, wenn eine benutzerdefinierte [Middleware](xref:fundamentals/middleware/index#writing-middleware) erstellt wird:</span><span class="sxs-lookup"><span data-stu-id="1445e-376">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="1445e-377">IApplicationLifetime-Schnittstelle</span><span class="sxs-lookup"><span data-stu-id="1445e-377">IApplicationLifetime interface</span></span>

<span data-ttu-id="1445e-378">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) ermöglicht Aktivitäten nach dem Starten und beim Herunterfahren.</span><span class="sxs-lookup"><span data-stu-id="1445e-378">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="1445e-379">Bei drei der Eigenschaften der Schnittstelle handelt es sich um Abbruchtokens, die zum Registrieren von `Action`-Methoden verwendet werden, durch die Ereignisse beim Starten und Herunterfahren definiert werden.</span><span class="sxs-lookup"><span data-stu-id="1445e-379">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="1445e-380">Abbruchtoken</span><span class="sxs-lookup"><span data-stu-id="1445e-380">Cancellation Token</span></span>    | <span data-ttu-id="1445e-381">Wird ausgelöst, wenn&#8230;</span><span class="sxs-lookup"><span data-stu-id="1445e-381">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="1445e-382">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="1445e-382">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="1445e-383">Der Host vollständig gestartet wurde.</span><span class="sxs-lookup"><span data-stu-id="1445e-383">The host has fully started.</span></span> |
| [<span data-ttu-id="1445e-384">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="1445e-384">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="1445e-385">Der Host ein ordnungsgemäßes Herunterfahren abschließt.</span><span class="sxs-lookup"><span data-stu-id="1445e-385">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="1445e-386">Alle Anforderungen sollten verarbeitet worden sein.</span><span class="sxs-lookup"><span data-stu-id="1445e-386">All requests should be processed.</span></span> <span data-ttu-id="1445e-387">Das Herunterfahren wird blockiert, bis das Ereignis abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="1445e-387">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="1445e-388">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="1445e-388">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="1445e-389">Der Host ein ordnungsgemäßes Herunterfahren ausführt.</span><span class="sxs-lookup"><span data-stu-id="1445e-389">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="1445e-390">Anforderungen möglicherweise noch verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="1445e-390">Requests may still be processing.</span></span> <span data-ttu-id="1445e-391">Das Herunterfahren wird blockiert, bis das Ereignis abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="1445e-391">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="1445e-392">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) fordert das Beenden der Anwendung an.</span><span class="sxs-lookup"><span data-stu-id="1445e-392">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="1445e-393">Die folgende Klasse verwendet `StopApplication`, um eine App ordnungsgemäß herunterzufahren, wenn die `Shutdown`-Methode der Klasse aufgerufen wird:</span><span class="sxs-lookup"><span data-stu-id="1445e-393">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="1445e-394">Bereichsvalidierung</span><span class="sxs-lookup"><span data-stu-id="1445e-394">Scope validation</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="1445e-395">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) legt [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) auf `true` fest, wenn die Umgebung der App „Entwicklung“ ist.</span><span class="sxs-lookup"><span data-stu-id="1445e-395">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

::: moniker-end

<span data-ttu-id="1445e-396">Wenn `ValidateScopes` auf `true` festgelegt wird, führt der Standarddienstanbieter Überprüfungen durch, um sicherzustellen, dass:</span><span class="sxs-lookup"><span data-stu-id="1445e-396">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="1445e-397">Bereichsbezogene Dienste nicht direkt oder indirekt vom Stammdienstanbieter aufgelöst werden</span><span class="sxs-lookup"><span data-stu-id="1445e-397">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="1445e-398">Bereichsbezogene Dienste nicht direkt oder indirekt in Singletons eingefügt werden</span><span class="sxs-lookup"><span data-stu-id="1445e-398">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="1445e-399">Der Stammdienstanbieter wird erstellt, wenn [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="1445e-399">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="1445e-400">Die Lebensdauer des Stammdienstanbieters entspricht der Lebensdauer der App/des Servers, wenn der Anbieter mit der App erstellt wird und verworfen wird, wenn die App beendet wird.</span><span class="sxs-lookup"><span data-stu-id="1445e-400">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="1445e-401">Bereichsbezogene Dienste werden von dem Container verworfen, der sie erstellt hat.</span><span class="sxs-lookup"><span data-stu-id="1445e-401">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="1445e-402">Wenn ein bereichsbezogener Dienst im Stammcontainer erstellt wird, wird die Lebensdauer effektiv auf Singleton heraufgestuft, da er nur vom Stammcontainer verworfen wird, wenn die App/der Server heruntergefahren wird.</span><span class="sxs-lookup"><span data-stu-id="1445e-402">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="1445e-403">Die Überprüfung bereichsbezogener Dienste erfasst diese Situationen, wenn `BuildServiceProvider` aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="1445e-403">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="1445e-404">Konfigurieren Sie [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) mit [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) auf dem Hostbuilder, um Bereiche immer zu validieren, auch in der Produktionsumgebung:</span><span class="sxs-lookup"><span data-stu-id="1445e-404">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

::: moniker range="= aspnetcore-2.0"

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="1445e-405">Problembehandlung bei System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="1445e-405">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="1445e-406">**Folgendes gilt nur für ASP.NET Core 2.0-Apps, wenn die App nicht `UseStartup` oder `Configure` aufruft.**</span><span class="sxs-lookup"><span data-stu-id="1445e-406">**The following only applies to ASP.NET Core 2.0 apps when the app doesn't call `UseStartup` or `Configure`.**</span></span>

<span data-ttu-id="1445e-407">Ein Host kann erstellt werden, indem `IStartup` direkt in den Dependency Injection-Container eingefügt wird, anstatt `UseStartup` oder `Configure` aufzurufen:</span><span class="sxs-lookup"><span data-stu-id="1445e-407">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="1445e-408">Wenn der Host auf diese Weise erstellt wird, kann folgender Fehler auftreten:</span><span class="sxs-lookup"><span data-stu-id="1445e-408">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="1445e-409">Dieser Fehler tritt auf, da der Name der App (der Name der aktuellen Assembly) erforderlich ist, um nach `HostingStartupAttributes` zu suchen.</span><span class="sxs-lookup"><span data-stu-id="1445e-409">This occurs because the app name (the name of the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="1445e-410">Wenn die App `IStartup` manuell in den Dependency Injection-Container eingefügt wird, fügen Sie folgenden Aufruf mit dem angegebenen Assemblynamen zur `WebHostBuilder`-Klasse hinzu:</span><span class="sxs-lookup"><span data-stu-id="1445e-410">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "AssemblyName")
```

<span data-ttu-id="1445e-411">Fügen Sie alternativ eine `Configure`-Dummymethode zur `WebHostBuilder`-Klasse hinzu, wodurch der App-Name automatisch festgelegt wird:</span><span class="sxs-lookup"><span data-stu-id="1445e-411">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the app name automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
```

<span data-ttu-id="1445e-412">Weitere Informationen finden Sie unter [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment) (Ankündigungen: „Microsoft.Extensions.PlatformAbstractions“ wurde entfernt (Kommentar))](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) und im Beispiel für [StartupInjection](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="1445e-412">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="1445e-413">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="1445e-413">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
