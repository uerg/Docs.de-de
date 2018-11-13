---
title: Hosten von ASP.NET Core unter Windows mit IIS
author: guardrex
description: Erfahren Sie, wie ASP.NET Core-Apps in Windows Server Internet Information Services (IIS) gehostet werden.
ms.author: riande
ms.custom: mvc
ms.date: 11/10/2018
uid: host-and-deploy/iis/index
ms.openlocfilehash: 1b34195dc51ca8dab5e8eda10f05ff6678fbc78c
ms.sourcegitcommit: 408921a932448f66cb46fd53c307a864f5323fe5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2018
ms.locfileid: "51570164"
---
# <a name="host-aspnet-core-on-windows-with-iis"></a><span data-ttu-id="187cc-103">Hosten von ASP.NET Core unter Windows mit IIS</span><span class="sxs-lookup"><span data-stu-id="187cc-103">Host ASP.NET Core on Windows with IIS</span></span>

<span data-ttu-id="187cc-104">Von [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="187cc-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[<span data-ttu-id="187cc-105">Installieren des .NET Core Hosting-Pakets</span><span class="sxs-lookup"><span data-stu-id="187cc-105">Install the .NET Core Hosting Bundle</span></span>](#install-the-net-core-hosting-bundle)

## <a name="supported-operating-systems"></a><span data-ttu-id="187cc-106">Unterstützte Betriebssysteme</span><span class="sxs-lookup"><span data-stu-id="187cc-106">Supported operating systems</span></span>

<span data-ttu-id="187cc-107">Die folgenden Betriebssysteme werden unterstützt:</span><span class="sxs-lookup"><span data-stu-id="187cc-107">The following operating systems are supported:</span></span>

* <span data-ttu-id="187cc-108">Windows 7 oder höher</span><span class="sxs-lookup"><span data-stu-id="187cc-108">Windows 7 or later</span></span>
* <span data-ttu-id="187cc-109">Windows Server 2008 R2 oder höher</span><span class="sxs-lookup"><span data-stu-id="187cc-109">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="187cc-110">Der [HTTP.SYS-Server](xref:fundamentals/servers/httpsys) (zuvor [WebListener](xref:fundamentals/servers/weblistener) genannt) funktioniert nicht in einer Reverseproxykonfiguration mit IIS.</span><span class="sxs-lookup"><span data-stu-id="187cc-110">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)) doesn't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="187cc-111">Verwenden Sie den [Kestrel-Server](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="187cc-111">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="187cc-112">Weitere Informationen zum Hosten in Azure finden Sie unter <xref:host-and-deploy/azure-apps/index>.</span><span class="sxs-lookup"><span data-stu-id="187cc-112">For information on hosting in Azure, see <xref:host-and-deploy/azure-apps/index>.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="187cc-113">Anwendungskonfiguration</span><span class="sxs-lookup"><span data-stu-id="187cc-113">Application configuration</span></span>

### <a name="enable-the-iisintegration-components"></a><span data-ttu-id="187cc-114">Aktivieren der IISIntegration-Komponenten</span><span class="sxs-lookup"><span data-stu-id="187cc-114">Enable the IISIntegration components</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="187cc-115">Eine typische *Program.cs*-Datei ruft <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> auf, um mit der Einrichtung eines Hosts zu beginnen:</span><span class="sxs-lookup"><span data-stu-id="187cc-115">A typical *Program.cs* calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to begin setting up a host:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="187cc-116">Eine typische *Program.cs*-Datei ruft <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> auf, um mit der Einrichtung eines Hosts zu beginnen:</span><span class="sxs-lookup"><span data-stu-id="187cc-116">A typical *Program.cs* calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to begin setting up a host:</span></span>

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="187cc-117">**In-Process-Hostingmodell**</span><span class="sxs-lookup"><span data-stu-id="187cc-117">**In-process hosting model**</span></span>

<span data-ttu-id="187cc-118">`CreateDefaultBuilder` ruft die `UseIIS`-Methode auf, um die [CoreCLR](/dotnet/standard/glossary#coreclr) zu starten und die App im IIS-Workerprozess zu hosten (*w3wp.exe* oder *iisexpress.exe*).</span><span class="sxs-lookup"><span data-stu-id="187cc-118">`CreateDefaultBuilder` calls the `UseIIS` method to boot the [CoreCLR](/dotnet/standard/glossary#coreclr) and host the app inside of the IIS worker process (*w3wp.exe* or *iisexpress.exe*).</span></span> <span data-ttu-id="187cc-119">Leistungstests weisen darauf hin, dass das In-Process-Hosting einer .NET Core-App einen weitaus höheren Anforderungsdurchsatz im Vergleich zum Out-of-Process-Hosting der App mit Weiterleitung der Anforderungen über einen Proxy an [Kestrel](xref:fundamentals/servers/kestrel) bietet.</span><span class="sxs-lookup"><span data-stu-id="187cc-119">Performance tests indicate that hosting a .NET Core app in-process delivers significantly higher request throughput compared to hosting the app out-of-process and proxying requests to [Kestrel](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="187cc-120">**Out-of-Process-Hostingmodell**</span><span class="sxs-lookup"><span data-stu-id="187cc-120">**Out-of-process hosting model**</span></span>

<span data-ttu-id="187cc-121">Für das Out-of-Process-Hosting mit IIS konfiguriert `CreateDefaultBuilder` [Kestrel](xref:fundamentals/servers/kestrel) als Webserver und aktiviert die IIS-Integration durch Konfigurieren des Basispfads und -ports für das [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="187cc-121">For out-of-process hosting with IIS, `CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and enables IIS integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span>

<span data-ttu-id="187cc-122">Das ASP.NET Core-Modul generiert einen dynamischen Port, der dem Back-End-Prozess zugewiesen wird.</span><span class="sxs-lookup"><span data-stu-id="187cc-122">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="187cc-123">`CreateDefaultBuilder` ruft die <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*>-Methode auf.</span><span class="sxs-lookup"><span data-stu-id="187cc-123">`CreateDefaultBuilder` calls the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> method.</span></span> <span data-ttu-id="187cc-124">`UseIISIntegration` konfiguriert Kestrel so, dass an dem dynamischen Port an der Localhost-IP-Adresse (`127.0.0.1`) gelauscht wird.</span><span class="sxs-lookup"><span data-stu-id="187cc-124">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`127.0.0.1`).</span></span> <span data-ttu-id="187cc-125">Wenn der dynamische Port 1234 ist, lauscht Kestrel an `127.0.0.1:1234`.</span><span class="sxs-lookup"><span data-stu-id="187cc-125">If the dynamic port is 1234, Kestrel listens at `127.0.0.1:1234`.</span></span> <span data-ttu-id="187cc-126">Diese Konfiguration ersetzt andere Konfigurationen von:</span><span class="sxs-lookup"><span data-stu-id="187cc-126">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* [<span data-ttu-id="187cc-127">der Listen-API von Kestrel</span><span class="sxs-lookup"><span data-stu-id="187cc-127">Kestrel's Listen API</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* <span data-ttu-id="187cc-128">der [Konfiguration](xref:fundamentals/configuration/index) (oder [der Befehlszeilenoption „--urls](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="187cc-128">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="187cc-129">Aufrufe von `UseUrls` oder der `Listen`-API von Kestrel sind nicht erforderlich, wenn das Modul verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="187cc-129">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="187cc-130">Wenn `UseUrls` oder `Listen` aufgerufen wird, lauscht Kestrel nur an die Ports, die bei Ausführung der App ohne IIS angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="187cc-130">If `UseUrls` or `Listen` is called, Kestrel listens on the ports specified only when running the app without IIS.</span></span>

<span data-ttu-id="187cc-131">Weitere Informationen zu In-Process- und Out-of-Process-Hostingmodellen finden Sie im [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) und in der [Referenz zur ASP.NET Core-Modulkonfiguration](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="187cc-131">For more information on the in-process and out-of-process hosting models, see [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="187cc-132">`CreateDefaultBuilder` konfiguriert [Kestrel](xref:fundamentals/servers/kestrel) als Webserver und aktiviert die IIS-Integration durch Konfigurierung des Basispfads und Ports für das [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="187cc-132">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and enables IIS integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span>

<span data-ttu-id="187cc-133">Das ASP.NET Core-Modul generiert einen dynamischen Port, der dem Back-End-Prozess zugewiesen wird.</span><span class="sxs-lookup"><span data-stu-id="187cc-133">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="187cc-134">`CreateDefaultBuilder` ruft die [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration)-Methode auf.</span><span class="sxs-lookup"><span data-stu-id="187cc-134">`CreateDefaultBuilder` calls the [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) method.</span></span> <span data-ttu-id="187cc-135">`UseIISIntegration` konfiguriert Kestrel so, dass an dem dynamischen Port an der Localhost-IP-Adresse (`127.0.0.1`) gelauscht wird.</span><span class="sxs-lookup"><span data-stu-id="187cc-135">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`127.0.0.1`).</span></span> <span data-ttu-id="187cc-136">Wenn der dynamische Port 1234 ist, lauscht Kestrel an `127.0.0.1:1234`.</span><span class="sxs-lookup"><span data-stu-id="187cc-136">If the dynamic port is 1234, Kestrel listens at `127.0.0.1:1234`.</span></span> <span data-ttu-id="187cc-137">Diese Konfiguration ersetzt andere Konfigurationen von:</span><span class="sxs-lookup"><span data-stu-id="187cc-137">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* [<span data-ttu-id="187cc-138">der Listen-API von Kestrel</span><span class="sxs-lookup"><span data-stu-id="187cc-138">Kestrel's Listen API</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* <span data-ttu-id="187cc-139">der [Konfiguration](xref:fundamentals/configuration/index) (oder [der Befehlszeilenoption „--urls](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="187cc-139">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="187cc-140">Aufrufe von `UseUrls` oder der `Listen`-API von Kestrel sind nicht erforderlich, wenn das Modul verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="187cc-140">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="187cc-141">Wenn `UseUrls` oder `Listen` aufgerufen wird, lauscht Kestrel an dem Port, der bei der bei Ausführung der App ohne den IIS angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="187cc-141">If `UseUrls` or `Listen` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="187cc-142">`CreateDefaultBuilder` konfiguriert [Kestrel](xref:fundamentals/servers/kestrel) als Webserver und aktiviert die IIS-Integration durch Konfigurierung des Basispfads und Ports für das [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="187cc-142">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and enables IIS integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span>

<span data-ttu-id="187cc-143">Das ASP.NET Core-Modul generiert einen dynamischen Port, der dem Back-End-Prozess zugewiesen wird.</span><span class="sxs-lookup"><span data-stu-id="187cc-143">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="187cc-144">`CreateDefaultBuilder` ruft die [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration)-Methode auf.</span><span class="sxs-lookup"><span data-stu-id="187cc-144">`CreateDefaultBuilder` calls the [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) method.</span></span> <span data-ttu-id="187cc-145">`UseIISIntegration` konfiguriert Kestrel so, dass an dem dynamischen Port an der Localhost-IP-Adresse (`localhost`) gelauscht wird.</span><span class="sxs-lookup"><span data-stu-id="187cc-145">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`localhost`).</span></span> <span data-ttu-id="187cc-146">Wenn der dynamische Port 1234 ist, lauscht Kestrel an `localhost:1234`.</span><span class="sxs-lookup"><span data-stu-id="187cc-146">If the dynamic port is 1234, Kestrel listens at `localhost:1234`.</span></span> <span data-ttu-id="187cc-147">Diese Konfiguration ersetzt andere Konfigurationen von:</span><span class="sxs-lookup"><span data-stu-id="187cc-147">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* [<span data-ttu-id="187cc-148">der Listen-API von Kestrel</span><span class="sxs-lookup"><span data-stu-id="187cc-148">Kestrel's Listen API</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* <span data-ttu-id="187cc-149">der [Konfiguration](xref:fundamentals/configuration/index) (oder [der Befehlszeilenoption „--urls](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="187cc-149">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="187cc-150">Aufrufe von `UseUrls` oder der `Listen`-API von Kestrel sind nicht erforderlich, wenn das Modul verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="187cc-150">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="187cc-151">Wenn `UseUrls` oder `Listen` aufgerufen wird, lauscht Kestrel an dem Port, der bei der bei Ausführung der App ohne den IIS angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="187cc-151">If `UseUrls` or `Listen` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="187cc-152">Nehmen Sie eine Abhängigkeit vom Paket [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) in die App-Abhängigkeiten auf.</span><span class="sxs-lookup"><span data-stu-id="187cc-152">Include a dependency on the [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) package in the app's dependencies.</span></span> <span data-ttu-id="187cc-153">Verwenden Sie Middleware für die Integration von IIS, indem Sie die Erweiterungsmethode [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) zu [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="187cc-153">Use IIS Integration middleware by adding the [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) extension method to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder):</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

<span data-ttu-id="187cc-154">Sowohl [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) als auch [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) sind erforderlich.</span><span class="sxs-lookup"><span data-stu-id="187cc-154">Both [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) and [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) are required.</span></span> <span data-ttu-id="187cc-155">Ein Code, der `UseIISIntegration` aufruft, hat keinen Einfluss auf die Codeportabilität.</span><span class="sxs-lookup"><span data-stu-id="187cc-155">Code calling `UseIISIntegration` doesn't affect code portability.</span></span> <span data-ttu-id="187cc-156">Wenn die App nicht hinter IIS ausgeführt wird (die App wird z.B. direkt auf Kestrel ausgeführt), hat `UseIISIntegration` keine Funktion.</span><span class="sxs-lookup"><span data-stu-id="187cc-156">If the app isn't run behind IIS (for example, the app is run directly on Kestrel), `UseIISIntegration` doesn't operate.</span></span>

<span data-ttu-id="187cc-157">Das ASP.NET Core-Modul generiert einen dynamischen Port, der dem Back-End-Prozess zugewiesen wird.</span><span class="sxs-lookup"><span data-stu-id="187cc-157">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="187cc-158">`UseIISIntegration` konfiguriert Kestrel so, dass an dem dynamischen Port an der Localhost-IP-Adresse (`localhost`) gelauscht wird.</span><span class="sxs-lookup"><span data-stu-id="187cc-158">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`localhost`).</span></span> <span data-ttu-id="187cc-159">Wenn der dynamische Port 1234 ist, lauscht Kestrel an `localhost:1234`.</span><span class="sxs-lookup"><span data-stu-id="187cc-159">If the dynamic port is 1234, Kestrel listens at `localhost:1234`.</span></span> <span data-ttu-id="187cc-160">Diese Konfiguration ersetzt andere Konfigurationen von:</span><span class="sxs-lookup"><span data-stu-id="187cc-160">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* <span data-ttu-id="187cc-161">der [Konfiguration](xref:fundamentals/configuration/index) (oder [der Befehlszeilenoption „--urls](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="187cc-161">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="187cc-162">Ein Aufruf von `UseUrls` ist nicht erforderlich, wenn das Modul verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="187cc-162">A call to `UseUrls` isn't required when using the module.</span></span> <span data-ttu-id="187cc-163">Wenn `UseUrls` aufgerufen wird, lauscht Kestrel an den Port, der nur bei der Ausführung der App ohne den IIS angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="187cc-163">If `UseUrls` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

<span data-ttu-id="187cc-164">Wenn `UseUrls` in einer ASP.NET Core 1.0-App aufgerufen wird, rufen Sie es **vor** Aufrufen von `UseIISIntegration` auf, damit der vom Modul konfigurierte Port nicht überschrieben wird.</span><span class="sxs-lookup"><span data-stu-id="187cc-164">If `UseUrls` is called in an ASP.NET Core 1.0 app, call it **before** calling `UseIISIntegration` so that the module-configured port isn't overwritten.</span></span> <span data-ttu-id="187cc-165">Diese Aufrufreihenfolge ist in ASP.NET Core 1.1 nicht erforderlich, weil die Moduleinstellung `UseUrls` überschreibt.</span><span class="sxs-lookup"><span data-stu-id="187cc-165">This calling order isn't required with ASP.NET Core 1.1 because the module setting overrides `UseUrls`.</span></span>

::: moniker-end

<span data-ttu-id="187cc-166">Weitere Informationen zum Hosten finden Sie unter [Hosten in ASP.NET Core](xref:fundamentals/host/index).</span><span class="sxs-lookup"><span data-stu-id="187cc-166">For more information on hosting, see [Host in ASP.NET Core](xref:fundamentals/host/index).</span></span>

### <a name="iis-options"></a><span data-ttu-id="187cc-167">IIS-Optionen</span><span class="sxs-lookup"><span data-stu-id="187cc-167">IIS options</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="187cc-168">**In-Process-Hostingmodell**</span><span class="sxs-lookup"><span data-stu-id="187cc-168">**In-process hosting model**</span></span>

<span data-ttu-id="187cc-169">Um IIS Server-Optionen zu konfigurieren, beziehen Sie eine Dienstkonfiguration für [IISServerOptions](/dotnet/api/microsoft.aspnetcore.builder.iisserveroptions) in [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices) ein.</span><span class="sxs-lookup"><span data-stu-id="187cc-169">To configure IIS Server options, include a service configuration for [IISServerOptions](/dotnet/api/microsoft.aspnetcore.builder.iisserveroptions) in [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices).</span></span> <span data-ttu-id="187cc-170">Mit dem folgenden Beispiel wird AutomaticAuthentication deaktiviert:</span><span class="sxs-lookup"><span data-stu-id="187cc-170">The following example disables AutomaticAuthentication:</span></span>

```csharp
services.Configure<IISServerOptions>(options => 
{
    options.AutomaticAuthentication = false;
});
```

| <span data-ttu-id="187cc-171">Option</span><span class="sxs-lookup"><span data-stu-id="187cc-171">Option</span></span>                         | <span data-ttu-id="187cc-172">Standard</span><span class="sxs-lookup"><span data-stu-id="187cc-172">Default</span></span> | <span data-ttu-id="187cc-173">Einstellung</span><span class="sxs-lookup"><span data-stu-id="187cc-173">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="187cc-174">Bei Festlegung auf `true` legt der Server den per [Windows-Authentifizierung](xref:security/authentication/windowsauth) authentifizierten `HttpContext.User` fest.</span><span class="sxs-lookup"><span data-stu-id="187cc-174">If `true`, IIS Server sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="187cc-175">Bei Festlegung auf `false` stellt der Server nur eine Identität für `HttpContext.User` bereit und antwortet auf explizite Anforderungen durch `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="187cc-175">If `false`, the server only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="187cc-176">Die Windows-Authentifizierung muss in IIS aktiviert sein, damit `AutomaticAuthentication` funktioniert.</span><span class="sxs-lookup"><span data-stu-id="187cc-176">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="187cc-177">Weitere Informationen finden Sie unter [Windows-Authentifizierung](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="187cc-177">For more information, see [Windows Authentication](xref:security/authentication/windowsauth).</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="187cc-178">Legt den Anzeigename fest, der Benutzern auf Anmeldungsseiten angezeigt wird</span><span class="sxs-lookup"><span data-stu-id="187cc-178">Sets the display name shown to users on login pages.</span></span> |

<span data-ttu-id="187cc-179">**Out-of-Process-Hostingmodell**</span><span class="sxs-lookup"><span data-stu-id="187cc-179">**Out-of-process hosting model**</span></span>

::: moniker-end

<span data-ttu-id="187cc-180">Um IIS-Optionen zu konfigurieren, beziehen Sie eine Dienstkonfiguration für [IISOptions](/dotnet/api/microsoft.aspnetcore.builder.iisoptions) in [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices) ein.</span><span class="sxs-lookup"><span data-stu-id="187cc-180">To configure IIS options, include a service configuration for [IISOptions](/dotnet/api/microsoft.aspnetcore.builder.iisoptions) in [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices).</span></span> <span data-ttu-id="187cc-181">Im folgenden Beispiel wird Verhindert, dass die App `HttpContext.Connection.ClientCertificate` auffüllt:</span><span class="sxs-lookup"><span data-stu-id="187cc-181">The following example prevents the app from populating `HttpContext.Connection.ClientCertificate`:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| <span data-ttu-id="187cc-182">Option</span><span class="sxs-lookup"><span data-stu-id="187cc-182">Option</span></span>                         | <span data-ttu-id="187cc-183">Standard</span><span class="sxs-lookup"><span data-stu-id="187cc-183">Default</span></span> | <span data-ttu-id="187cc-184">Einstellung</span><span class="sxs-lookup"><span data-stu-id="187cc-184">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="187cc-185">Bei Festlegung auf `true` legt die Middleware für die IIS-Integration den per [Windows-Authentifizierung](xref:security/authentication/windowsauth) authentifizierten `HttpContext.User` fest.</span><span class="sxs-lookup"><span data-stu-id="187cc-185">If `true`, IIS Integration Middleware sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="187cc-186">Bei Festlegung auf `false` stellt die Middleware nur eine Identität für `HttpContext.User` bereit und antwortet auf explizite Anforderungen durch `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="187cc-186">If `false`, the middleware only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="187cc-187">Die Windows-Authentifizierung muss in IIS aktiviert sein, damit `AutomaticAuthentication` funktioniert.</span><span class="sxs-lookup"><span data-stu-id="187cc-187">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="187cc-188">Weitere Informationen finden Sie im Thema [Windows-Authentifizierung](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="187cc-188">For more information, see the [Windows Authentication](xref:security/authentication/windowsauth) topic.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="187cc-189">Legt den Anzeigename fest, der Benutzern auf Anmeldungsseiten angezeigt wird</span><span class="sxs-lookup"><span data-stu-id="187cc-189">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="187cc-190">Wenn diese Option `true` ist und der Anforderungsheader `MS-ASPNETCORE-CLIENTCERT` vorhanden ist, wird das `HttpContext.Connection.ClientCertificate` aufgefüllt.</span><span class="sxs-lookup"><span data-stu-id="187cc-190">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="187cc-191">Proxyserver und Lastenausgleichsszenarien</span><span class="sxs-lookup"><span data-stu-id="187cc-191">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="187cc-192">Die Middleware für die Integration von IIS, die ForwardedHeadersMiddleware konfiguriert, und das ASP.NET Core-Modul sind so konfiguriert, dass sie das Schema (HTTP/HTTPS) und die Remote-IP-Adresse an die Stelle weiterleiten, von der die Anforderung stammte.</span><span class="sxs-lookup"><span data-stu-id="187cc-192">The IIS Integration Middleware, which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="187cc-193">Möglicherweise ist zusätzliche Konfiguration für Apps erforderlich, die hinter weiteren Proxyservern und Lastenausgleichsmodulen (Load Balancer) gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="187cc-193">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="187cc-194">Weitere Informationen hierzu feinden Sie unter [Konfigurieren von ASP.NET Core zur Verwendung mit Proxyservern und Lastenausgleich](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="187cc-194">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="webconfig-file"></a><span data-ttu-id="187cc-195">Datei „web.config“</span><span class="sxs-lookup"><span data-stu-id="187cc-195">web.config file</span></span>

<span data-ttu-id="187cc-196">Mit der Datei *web.config* wird das [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="187cc-196">The *web.config* file configures the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="187cc-197">Die Erstellung, Transformation und Veröffentlichung der Datei *web.config* wird bei der Projektveröffentlichung von einem MSBuild-Ziel (`_TransformWebConfig`) abgewickelt.</span><span class="sxs-lookup"><span data-stu-id="187cc-197">Creating, transforming, and publishing the *web.config* file is handled by an MSBuild target (`_TransformWebConfig`) when the project is published.</span></span> <span data-ttu-id="187cc-198">Dieses Ziel ist in den Web SDK-Zielen (`Microsoft.NET.Sdk.Web`) vorhanden.</span><span class="sxs-lookup"><span data-stu-id="187cc-198">This target is present in the Web SDK targets (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="187cc-199">Das SDK wird am Anfang der Projektdatei festgelegt:</span><span class="sxs-lookup"><span data-stu-id="187cc-199">The SDK is set at the top of the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="187cc-200">Wenn in der Projektdatei keine Datei namens *web.config* vorhanden ist, wird sie zur Konfiguration des [ASP.NET Core-Moduls](xref:fundamentals/servers/aspnet-core-module) mit dem richtigen *processPath*- und *arguments*-Attribut erstellt und in die [veröffentlichte Ausgabe](xref:host-and-deploy/directory-structure) verschoben.</span><span class="sxs-lookup"><span data-stu-id="187cc-200">If a *web.config* file isn't present in the project, the file is created with the correct *processPath* and *arguments* to configure the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and moved to [published output](xref:host-and-deploy/directory-structure).</span></span>

<span data-ttu-id="187cc-201">Ist eine Datei namens *web.config* im Projekt vorhanden, wird sie zur Konfiguration des ASP.NET Core-Moduls mit dem richtigen *processPath*- und *arguments*-Attribut transformiert und in die veröffentlichte Ausgabe verschoben.</span><span class="sxs-lookup"><span data-stu-id="187cc-201">If a *web.config* file is present in the project, the file is transformed with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to published output.</span></span> <span data-ttu-id="187cc-202">Die Transformation ändert die IIS-Konfigurationseinstellungen in der Datei nicht.</span><span class="sxs-lookup"><span data-stu-id="187cc-202">The transformation doesn't modify IIS configuration settings in the file.</span></span>

<span data-ttu-id="187cc-203">Die Datei *web.config* kann zusätzliche IIS-Konfigurationseinstellungen zum Steuern der aktiven IIS-Module bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="187cc-203">The *web.config* file may provide additional IIS configuration settings that control active IIS modules.</span></span> <span data-ttu-id="187cc-204">Informationen zu IIS-Modulen, die Anforderungen mit ASP.NET Core-Apps verarbeiten können, finden Sie im Thema [IIS-Module](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="187cc-204">For information on IIS modules that are capable of processing requests with ASP.NET Core apps, see the [IIS modules](xref:host-and-deploy/iis/modules) topic.</span></span>

<span data-ttu-id="187cc-205">Um zu verhindern, dass das Web-SDK die Datei *web.config* transformiert, verwenden Sie die Eigenschaft **\<IsTransformWebConfigDisabled>** in der Projektdatei:</span><span class="sxs-lookup"><span data-stu-id="187cc-205">To prevent the Web SDK from transforming the *web.config* file, use the **\<IsTransformWebConfigDisabled>** property in the project file:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="187cc-206">Wenn die Transformation der Datei durch das Web-SDK deaktiviert wird, müssen die Attribute *processPath* und *arguments* manuell durch den Entwickler festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="187cc-206">When disabling the Web SDK from transforming the file, the *processPath* and *arguments* should be manually set by the developer.</span></span> <span data-ttu-id="187cc-207">Weitere Informationen finden Sie unter [Konfigurationsreferenz für das ASP.NET Core-Modul](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="187cc-207">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

### <a name="webconfig-file-location"></a><span data-ttu-id="187cc-208">Speicherort der Datei „web.config“</span><span class="sxs-lookup"><span data-stu-id="187cc-208">web.config file location</span></span>

<span data-ttu-id="187cc-209">Zum Erstellen des [ASP.NET Core-Moduls](xref:host-and-deploy/aspnet-core-module) muss die Datei *web.config* im Stammpfad des Inhalts (normalerweise dem App-Basispfad) der bereitgestellten App vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="187cc-209">In order to set up the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) correctly, the *web.config* file must be present at the content root path (typically the app base path) of the deployed app.</span></span> <span data-ttu-id="187cc-210">Dies ist der physische Pfad der Website, der in IIS bereitgestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="187cc-210">This is the same location as the website physical path provided to IIS.</span></span> <span data-ttu-id="187cc-211">Die Datei *Web.config* wird im Stammverzeichnis der App benötigt, um die Veröffentlichung mehrerer Apps mit Web Deploy zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="187cc-211">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="187cc-212">Im physischen Pfad der App sind vertrauliche Dateien vorhanden, wie z.B. *\<assembly_name>.runtimeconfig.json*, *\<assembly_name>.xml* (XML-Dokumentationskommentare) und *\<assembly_name>.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="187cc-212">Sensitive files exist on the app's physical path, such as *\<assembly>.runtimeconfig.json*, *\<assembly>.xml* (XML Documentation comments), and *\<assembly>.deps.json*.</span></span> <span data-ttu-id="187cc-213">Wenn die Datei *web.config* vorhanden ist und die Website normal startet, werden diese vertraulichen Dateien von IIS bei Anforderung nicht offengelegt.</span><span class="sxs-lookup"><span data-stu-id="187cc-213">When the *web.config* file is present and and the site starts normally, IIS doesn't serve these sensitive files if they're requested.</span></span> <span data-ttu-id="187cc-214">Wenn die Datei *web.config* fehlt, falsch benannt wurde oder die Website nicht für den normalen Start konfigurieren kann, macht IIS vertrauliche Dateien möglicherweise öffentlich verfügbar.</span><span class="sxs-lookup"><span data-stu-id="187cc-214">If the *web.config* file is missing, incorrectly named, or unable to configure the site for normal startup, IIS may serve sensitive files publicly.</span></span>

<span data-ttu-id="187cc-215">**Die Datei *web.config* muss immer in der Bereitstellung vorhanden, richtig benannt und in der Lage sein, die Website für einen normalen Start zu konfigurieren. Entfernen Sie die Datei *web.config* niemals aus einer Produktionsbereitstellung.**</span><span class="sxs-lookup"><span data-stu-id="187cc-215">**The *web.config* file must be present in the deployment at all times, correctly named, and able to configure the site for normal start up. Never remove the *web.config* file from a production deployment.**</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="187cc-216">IIS-Konfiguration</span><span class="sxs-lookup"><span data-stu-id="187cc-216">IIS configuration</span></span>

<span data-ttu-id="187cc-217">**Windows Server-Betriebssysteme**</span><span class="sxs-lookup"><span data-stu-id="187cc-217">**Windows Server operating systems**</span></span>

<span data-ttu-id="187cc-218">Aktivieren Sie die Serverrolle **Webserver (IIS)**, und richten Sie Rollendienste ein.</span><span class="sxs-lookup"><span data-stu-id="187cc-218">Enable the **Web Server (IIS)** server role and establish role services.</span></span>

1. <span data-ttu-id="187cc-219">Verwenden Sie den Assistenten **Rollen und Features hinzufügen** im Menü **Verwalten** oder den Link in **Server-Manager**.</span><span class="sxs-lookup"><span data-stu-id="187cc-219">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="187cc-220">Aktivieren Sie im Schritt **Serverrollen** das Kontrollkästchen für **Webserver (IIS)**.</span><span class="sxs-lookup"><span data-stu-id="187cc-220">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

   ![Die Rolle „Webserver (IIS)“ wird im Schritt „Serverrollen auswählen“ ausgewählt.](index/_static/server-roles-ws2016.png)

1. <span data-ttu-id="187cc-222">Nach dem Schritt **Features** wird der Schritt **Rollendienste** für Webserver (IIS) geladen.</span><span class="sxs-lookup"><span data-stu-id="187cc-222">After the **Features** step, the **Role services** step loads for Web Server (IIS).</span></span> <span data-ttu-id="187cc-223">Wählen Sie die gewünschten IIS-Rollendienste aus, oder übernehmen Sie die bereitgestellten Standardrollendienste.</span><span class="sxs-lookup"><span data-stu-id="187cc-223">Select the IIS role services desired or accept the default role services provided.</span></span>

   ![Die Standardrollendienste werden im Schritt „Rollendienste auswählen“ ausgewählt.](index/_static/role-services-ws2016.png)

   <span data-ttu-id="187cc-225">**Windows-Authentifizierung (optional)**</span><span class="sxs-lookup"><span data-stu-id="187cc-225">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="187cc-226">Um die Windows-Authentifizierung zu aktivieren, erweitern Sie die folgenden Knoten: **Webserver** > **Sicherheit**.</span><span class="sxs-lookup"><span data-stu-id="187cc-226">To enable Windows Authentication, expand the following nodes: **Web Server** > **Security**.</span></span> <span data-ttu-id="187cc-227">Wählen Sie das Feature **Windows-Authentifizierung** aus.</span><span class="sxs-lookup"><span data-stu-id="187cc-227">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="187cc-228">Weitere Informationen finden Sie unter [Windows-Authentifizierung \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) und [Konfigurieren der Windows-Authentifizierung](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="187cc-228">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="187cc-229">**WebSockets (optional)**</span><span class="sxs-lookup"><span data-stu-id="187cc-229">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="187cc-230">WebSockets wird mit ASP.NET Core 1.1 oder höher unterstützt.</span><span class="sxs-lookup"><span data-stu-id="187cc-230">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="187cc-231">Um WebSockets zu aktivieren, erweitern Sie die folgenden Knoten: **Webserver** > **Anwendungsentwicklung**.</span><span class="sxs-lookup"><span data-stu-id="187cc-231">To enable WebSockets, expand the following nodes: **Web Server** > **Application Development**.</span></span> <span data-ttu-id="187cc-232">Wählen Sie das Feature **WebSocket-Protokoll** aus.</span><span class="sxs-lookup"><span data-stu-id="187cc-232">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="187cc-233">Weitere Informationen finden Sie unter [WebSockets](xref:fundamentals/websockets).</span><span class="sxs-lookup"><span data-stu-id="187cc-233">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="187cc-234">Fahren Sie mit dem Schritt **Bestätigung** fort, um die Webserverrolle und die Dienste zu installieren.</span><span class="sxs-lookup"><span data-stu-id="187cc-234">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="187cc-235">Ein Server-/IIS-Neustart ist nach der Installation der Rolle **Webserver (IIS)** nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="187cc-235">A server/IIS restart isn't required after installing the **Web Server (IIS)** role.</span></span>

<span data-ttu-id="187cc-236">**Windows-Desktopbetriebssysteme**</span><span class="sxs-lookup"><span data-stu-id="187cc-236">**Windows desktop operating systems**</span></span>

<span data-ttu-id="187cc-237">Aktivieren Sie die **IIS-Verwaltungskonsole** und die **WWW-Dienste**.</span><span class="sxs-lookup"><span data-stu-id="187cc-237">Enable the **IIS Management Console** and **World Wide Web Services**.</span></span>

1. <span data-ttu-id="187cc-238">Navigieren Sie zu **Systemsteuerung** > **Programme** > **Programme und Features** > **Windows-Features aktivieren oder deaktivieren** (links auf dem Bildschirm).</span><span class="sxs-lookup"><span data-stu-id="187cc-238">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>

1. <span data-ttu-id="187cc-239">Öffnen Sie den Knoten **Internetinformationsdienste**.</span><span class="sxs-lookup"><span data-stu-id="187cc-239">Open the **Internet Information Services** node.</span></span> <span data-ttu-id="187cc-240">Öffnen Sie den Knoten **Webverwaltungstools**.</span><span class="sxs-lookup"><span data-stu-id="187cc-240">Open the **Web Management Tools** node.</span></span>

1. <span data-ttu-id="187cc-241">Aktivieren Sie das Kontrollkästchen für **IIS-Verwaltungskonsole**.</span><span class="sxs-lookup"><span data-stu-id="187cc-241">Check the box for **IIS Management Console**.</span></span>

1. <span data-ttu-id="187cc-242">Aktivieren Sie das Kontrollkästchen für **WWW-Dienste**.</span><span class="sxs-lookup"><span data-stu-id="187cc-242">Check the box for **World Wide Web Services**.</span></span>

1. <span data-ttu-id="187cc-243">Akzeptieren Sie die Standardfeatures für **WWW-Dienste**, oder passen Sie die IIS-Features an.</span><span class="sxs-lookup"><span data-stu-id="187cc-243">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

   <span data-ttu-id="187cc-244">**Windows-Authentifizierung (optional)**</span><span class="sxs-lookup"><span data-stu-id="187cc-244">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="187cc-245">Um die Windows-Authentifizierung zu aktivieren, erweitern Sie die folgenden Knoten: **WWW-Dienste** > **Sicherheit**.</span><span class="sxs-lookup"><span data-stu-id="187cc-245">To enable Windows Authentication, expand the following nodes: **World Wide Web Services** > **Security**.</span></span> <span data-ttu-id="187cc-246">Wählen Sie das Feature **Windows-Authentifizierung** aus.</span><span class="sxs-lookup"><span data-stu-id="187cc-246">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="187cc-247">Weitere Informationen finden Sie unter [Windows-Authentifizierung \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) und [Konfigurieren der Windows-Authentifizierung](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="187cc-247">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="187cc-248">**WebSockets (optional)**</span><span class="sxs-lookup"><span data-stu-id="187cc-248">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="187cc-249">WebSockets wird mit ASP.NET Core 1.1 oder höher unterstützt.</span><span class="sxs-lookup"><span data-stu-id="187cc-249">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="187cc-250">Um WebSockets zu aktivieren, erweitern Sie die folgenden Knoten: **WWW-Dienste** > **Anwendungsentwicklungsfeatures**.</span><span class="sxs-lookup"><span data-stu-id="187cc-250">To enable WebSockets, expand the following nodes: **World Wide Web Services** > **Application Development Features**.</span></span> <span data-ttu-id="187cc-251">Wählen Sie das Feature **WebSocket-Protokoll** aus.</span><span class="sxs-lookup"><span data-stu-id="187cc-251">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="187cc-252">Weitere Informationen finden Sie unter [WebSockets](xref:fundamentals/websockets).</span><span class="sxs-lookup"><span data-stu-id="187cc-252">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="187cc-253">Wenn die IIS-Installation einen Neustart erfordert, starten Sie das System neu.</span><span class="sxs-lookup"><span data-stu-id="187cc-253">If the IIS installation requires a restart, restart the system.</span></span>

![Die IIS-Verwaltungskonsole und WWW-Dienste werden in Windows-Features ausgewählt.](index/_static/windows-features-win10.png)

## <a name="install-the-net-core-hosting-bundle"></a><span data-ttu-id="187cc-255">Installieren des .NET Core Hosting-Pakets</span><span class="sxs-lookup"><span data-stu-id="187cc-255">Install the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="187cc-256">Installieren Sie das *Paket „.NET Core Hosting“* im Hostsystem.</span><span class="sxs-lookup"><span data-stu-id="187cc-256">Install the *.NET Core Hosting Bundle* on the hosting system.</span></span> <span data-ttu-id="187cc-257">Das Paket installiert die .NET Core-Runtime, die .NET Core-Bibliothek und das [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="187cc-257">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="187cc-258">Mit dem Modul können ASP.NET Core-Apps hinter IIS ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="187cc-258">The module allows ASP.NET Core apps to run behind IIS.</span></span> <span data-ttu-id="187cc-259">Wenn das System nicht über eine Internetverbindung verfügt, beziehen und installieren Sie [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840), bevor Sie das Paket „.NET Core Hosting“ installieren.</span><span class="sxs-lookup"><span data-stu-id="187cc-259">If the system doesn't have an Internet connection, obtain and install the [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) before installing the .NET Core Hosting Bundle.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="187cc-260">Wenn das Hosting-Paket vor IIS installiert wird, muss die Paketinstallation repariert werden.</span><span class="sxs-lookup"><span data-stu-id="187cc-260">If the Hosting Bundle is installed before IIS, the bundle installation must be repaired.</span></span> <span data-ttu-id="187cc-261">Führen Sie nach der Installation von IIS erneut den Installer des Hosting-Pakets aus.</span><span class="sxs-lookup"><span data-stu-id="187cc-261">Run the Hosting Bundle installer again after installing IIS.</span></span>

### <a name="direct-download-current-version"></a><span data-ttu-id="187cc-262">Direkter Download (aktuelle Version)</span><span class="sxs-lookup"><span data-stu-id="187cc-262">Direct download (current version)</span></span>

<span data-ttu-id="187cc-263">Laden Sie den Installer über folgenden Link herunter:</span><span class="sxs-lookup"><span data-stu-id="187cc-263">Download the installer using the following link:</span></span>

[<span data-ttu-id="187cc-264">Aktueller Installer für das .NET Core Hosting-Paket (direkter Download)</span><span class="sxs-lookup"><span data-stu-id="187cc-264">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

### <a name="earlier-versions-of-the-installer"></a><span data-ttu-id="187cc-265">Frühere Versionen des Installers</span><span class="sxs-lookup"><span data-stu-id="187cc-265">Earlier versions of the installer</span></span>

<span data-ttu-id="187cc-266">So erhalten Sie eine frühere Version des Installers:</span><span class="sxs-lookup"><span data-stu-id="187cc-266">To obtain an earlier version of the installer:</span></span>

1. <span data-ttu-id="187cc-267">Navigieren Sie zu den[.NET-Downloadarchiven](https://www.microsoft.com/net/download/archives).</span><span class="sxs-lookup"><span data-stu-id="187cc-267">Navigate to the [.NET download archives](https://www.microsoft.com/net/download/archives).</span></span>
1. <span data-ttu-id="187cc-268">Wählen Sie unter **.NET Core** die .NET Core-Version aus.</span><span class="sxs-lookup"><span data-stu-id="187cc-268">Under **.NET Core**, select the .NET Core version.</span></span>
1. <span data-ttu-id="187cc-269">Suchen Sie in der Spalte **Run apps - Runtime** (Apps ausführen – Runtime) die Zeile mit der gewünschten .NET Core-RuntimeRuntime-Version.</span><span class="sxs-lookup"><span data-stu-id="187cc-269">In the **Run apps - Runtime** column, find the row of the .NET Core runtime version desired.</span></span>
1. <span data-ttu-id="187cc-270">Laden Sie den Installer über den Link **Runtime & Hosting Bundle** (Runtime- und Hosting-Paket) herunter.</span><span class="sxs-lookup"><span data-stu-id="187cc-270">Download the installer using the **Runtime & Hosting Bundle** link.</span></span>

> [!WARNING]
> <span data-ttu-id="187cc-271">Einige Installer enthalten Releaseversionen, die das Ende ihres Lebenszyklus erreicht haben und nicht mehr von Microsoft unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="187cc-271">Some installers contain release versions that have reached their end of life (EOL) and are no longer supported by Microsoft.</span></span> <span data-ttu-id="187cc-272">Weitere Informationen finden Sie in den [Unterstützungsrichtlinien](https://www.microsoft.com/net/download/dotnet-core/2.0).</span><span class="sxs-lookup"><span data-stu-id="187cc-272">For more information, see the [support policy](https://www.microsoft.com/net/download/dotnet-core/2.0).</span></span>

### <a name="install-the-hosting-bundle"></a><span data-ttu-id="187cc-273">Installieren des Hosting-Pakets</span><span class="sxs-lookup"><span data-stu-id="187cc-273">Install the Hosting Bundle</span></span>

1. <span data-ttu-id="187cc-274">Führen Sie das Installationsprogramm auf dem Server aus.</span><span class="sxs-lookup"><span data-stu-id="187cc-274">Run the installer on the server.</span></span> <span data-ttu-id="187cc-275">Die folgenden Schalter sind bei der Ausführung des Installers über eine Administratoreingabeaufforderung verfügbar:</span><span class="sxs-lookup"><span data-stu-id="187cc-275">The following switches are available when running the installer from an administrator command prompt:</span></span>

   * <span data-ttu-id="187cc-276">`OPT_NO_ANCM=1` &ndash; Überspringen Sie die Installation des ASP.NET Core-Moduls.</span><span class="sxs-lookup"><span data-stu-id="187cc-276">`OPT_NO_ANCM=1` &ndash; Skip installing the ASP.NET Core Module.</span></span>
   * <span data-ttu-id="187cc-277">`OPT_NO_RUNTIME=1` &ndash; Überspringen Sie die Installation der .NET Core Runtime.</span><span class="sxs-lookup"><span data-stu-id="187cc-277">`OPT_NO_RUNTIME=1` &ndash; Skip installing the .NET Core runtime.</span></span>
   * <span data-ttu-id="187cc-278">`OPT_NO_SHAREDFX=1` &ndash; Überspringen Sie die Installation des geteilten ASP.NET Frameworks (ASP.NET Runtime).</span><span class="sxs-lookup"><span data-stu-id="187cc-278">`OPT_NO_SHAREDFX=1` &ndash; Skip installing the ASP.NET Shared Framework (ASP.NET runtime).</span></span>
   * <span data-ttu-id="187cc-279">`OPT_NO_X86=1` &ndash; Überspringen Sie die Installation von X86 Runtimes.</span><span class="sxs-lookup"><span data-stu-id="187cc-279">`OPT_NO_X86=1` &ndash; Skip installing x86 runtimes.</span></span> <span data-ttu-id="187cc-280">Verwenden Sie diese Option, wenn Sie wissen, dass Sie keine 32-Bit-Apps hosten.</span><span class="sxs-lookup"><span data-stu-id="187cc-280">Use this switch when you know that you won't be hosting 32-bit apps.</span></span> <span data-ttu-id="187cc-281">Sollte die Möglichkeit bestehen, dass Sie sowohl 32-Bit- als auch 64-Bit-Apps hosten könnten, verwenden Sie diese Option nicht, und installieren Sie beide Runtimes.</span><span class="sxs-lookup"><span data-stu-id="187cc-281">If there's any chance that you will host both 32-bit and 64-bit apps in the future, don't use this switch and install both runtimes.</span></span>
1. <span data-ttu-id="187cc-282">Starten Sie das System neu, oder führen Sie **net stop was /y** gefolgt von **net start w3svc** über eine Eingabeaufforderung aus.</span><span class="sxs-lookup"><span data-stu-id="187cc-282">Restart the system or execute **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span> <span data-ttu-id="187cc-283">Durch den Neustart von IIS wird eine Änderung an der PATH-Systemeinstellung – einer Umgebungsvariable – angewendet, die durch den Installer vorgenommen wurde.</span><span class="sxs-lookup"><span data-stu-id="187cc-283">Restarting IIS picks up a change to the system PATH, which is an environment variable, made by the installer.</span></span>

<span data-ttu-id="187cc-284">Wenn der Windows Hosting Bundle-Installer feststellt, dass für IIS ein Zurücksetzen erforderlich ist, um die Installation abzuschließen, setzt der Installer IIS zurück.</span><span class="sxs-lookup"><span data-stu-id="187cc-284">If the Windows Hosting Bundle installer detects that IIS requires a reset in order to complete installation, the installer resets IIS.</span></span> <span data-ttu-id="187cc-285">Wenn der Installer eine IIS-Zurücksetzung auslöst, werden alle IIS-App-Pools und -Websites neu gestartet.</span><span class="sxs-lookup"><span data-stu-id="187cc-285">If the installer triggers an IIS reset, all of the IIS app pools and websites are restarted.</span></span>

> [!NOTE]
> <span data-ttu-id="187cc-286">Informationen zur Verwendung einer IIS-Freigabekonfiguration finden Sie unter [ASP.NET Core-Modul mit IIS-Freigabekonfiguration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span><span class="sxs-lookup"><span data-stu-id="187cc-286">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="187cc-287">Installieren von Web Deploy beim Veröffentlichen mit Visual Studio</span><span class="sxs-lookup"><span data-stu-id="187cc-287">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="187cc-288">Wenn Sie Apps auf Servern mit [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) bereitstellen, installieren Sie die neueste Version von Web Deploy auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="187cc-288">When deploying apps to servers with [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="187cc-289">Um Web Deploy zu installieren, verwenden Sie den [Webplattform-Installer (Web PI)](https://www.microsoft.com/web/downloads/platform.aspx) oder rufen einen Installer direkt aus dem [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717) ab.</span><span class="sxs-lookup"><span data-stu-id="187cc-289">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="187cc-290">Die bevorzugte Methode ist die Verwendung von WebPI.</span><span class="sxs-lookup"><span data-stu-id="187cc-290">The preferred method is to use WebPI.</span></span> <span data-ttu-id="187cc-291">WebPI bietet ein eigenständiges Setup und eine Konfiguration für Hostinganbieter.</span><span class="sxs-lookup"><span data-stu-id="187cc-291">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="create-the-iis-site"></a><span data-ttu-id="187cc-292">Erstellen der IIS-Website</span><span class="sxs-lookup"><span data-stu-id="187cc-292">Create the IIS site</span></span>

1. <span data-ttu-id="187cc-293">Erstellen Sie auf dem Hostingsystem einen Ordner zum Speichern der veröffentlichten Ordner und Dateien der App.</span><span class="sxs-lookup"><span data-stu-id="187cc-293">On the hosting system, create a folder to contain the app's published folders and files.</span></span> <span data-ttu-id="187cc-294">Das Layout der App-Bereitstellung wird im Thema [Verzeichnisstruktur](xref:host-and-deploy/directory-structure) beschrieben.</span><span class="sxs-lookup"><span data-stu-id="187cc-294">An app's deployment layout is described in the [Directory Structure](xref:host-and-deploy/directory-structure) topic.</span></span>

1. <span data-ttu-id="187cc-295">Erstellen Sie innerhalb des neuen Ordners einen Ordner *Protokolle*, um darin stdout-Protokolle von ASP.NET Core zu speichern, wenn die stdout-Protokollierung aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="187cc-295">Within the new folder, create a *logs* folder to hold ASP.NET Core Module stdout logs when stdout logging is enabled.</span></span> <span data-ttu-id="187cc-296">Wenn die App mit einem Ordner *Protokolle* in der Payload bereitgestellt wird, können Sie diesen Schritt überspringen.</span><span class="sxs-lookup"><span data-stu-id="187cc-296">If the app is deployed with a *logs* folder in the payload, skip this step.</span></span> <span data-ttu-id="187cc-297">Anweisungen zum Aktivieren von MSBuild zum automatischen Erstellen des Ordners *Protokolle* bei der Erstellung des lokalen Projekts finden Sie im Thema zur [Verzeichnisstruktur](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="187cc-297">For instructions on how to enable MSBuild to create the *logs* folder automatically when the project is built locally, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="187cc-298">Verwenden Sie das stdout-Protokoll, um Probleme beim App-Start zu behandeln.</span><span class="sxs-lookup"><span data-stu-id="187cc-298">Only use the stdout log to troubleshoot app startup failures.</span></span> <span data-ttu-id="187cc-299">Verwenden Sie die stdout-Protokollierung nicht für die routinemäßige App-Protokollierung.</span><span class="sxs-lookup"><span data-stu-id="187cc-299">Never use stdout logging for routine app logging.</span></span> <span data-ttu-id="187cc-300">Für die Protokollgröße oder die Anzahl von erstellten Protokolldateien ist kein Grenzwert festgelegt.</span><span class="sxs-lookup"><span data-stu-id="187cc-300">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="187cc-301">Der App-Pool muss über Schreibberechtigungen für den Speicherort verfügen, an dem die Protokolle geschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="187cc-301">The app pool must have write access to the location where the logs are written.</span></span> <span data-ttu-id="187cc-302">Alle Ordner im Pfad des Protokollspeicherorts müssen vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="187cc-302">All of the folders on the path to the log location must exist.</span></span> <span data-ttu-id="187cc-303">Weitere Informationen zum stdout-Protokoll finden Sie im Thema zur [Protokollerstellung und Umleitung](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span><span class="sxs-lookup"><span data-stu-id="187cc-303">For more information on the stdout log, see [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span></span> <span data-ttu-id="187cc-304">Informationen zur Protokollierung in einer ASP.NET Core-App finden Sie im Thema [Protokollierung](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="187cc-304">For information on logging in an ASP.NET Core app, see the [Logging](xref:fundamentals/logging/index) topic.</span></span>

1. <span data-ttu-id="187cc-305">Öffnen Sie im **IIS-Manager** den Serverknoten im Bereich **Verbindungen**.</span><span class="sxs-lookup"><span data-stu-id="187cc-305">In **IIS Manager**, open the server's node in the **Connections** panel.</span></span> <span data-ttu-id="187cc-306">Klicken Sie mit der rechten Maustaste auf den Ordner **Websites**.</span><span class="sxs-lookup"><span data-stu-id="187cc-306">Right-click the **Sites** folder.</span></span> <span data-ttu-id="187cc-307">Klicken Sie im Kontextmenü auf **Website hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="187cc-307">Select **Add Website** from the contextual menu.</span></span>

1. <span data-ttu-id="187cc-308">Geben Sie einen **Websitenamen** an, und legen Sie den **physischen Pfad** auf den Bereitstellungsordner der App fest.</span><span class="sxs-lookup"><span data-stu-id="187cc-308">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="187cc-309">Geben Sie die Konfiguration unter **Bindung** an, und erstellen Sie die Website, indem Sie auf **OK** klicken:</span><span class="sxs-lookup"><span data-stu-id="187cc-309">Provide the **Binding** configuration and create the website by selecting **OK**:</span></span>

   ![Geben Sie den Websitenamen, den physischen Pfad und den Hostnamen im Schritt „Website hinzufügen“ an.](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > <span data-ttu-id="187cc-311">Allgemeine Platzhalterbindungen (`http://*:80/` und `http://+:80`) dürfen **nicht** verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="187cc-311">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="187cc-312">Platzhalterbindungen auf oberster Ebene gefährden die Sicherheit Ihrer App.</span><span class="sxs-lookup"><span data-stu-id="187cc-312">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="187cc-313">Dies gilt für starke und schwache Platzhalter.</span><span class="sxs-lookup"><span data-stu-id="187cc-313">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="187cc-314">Verwenden Sie statt Platzhaltern explizite Hostnamen.</span><span class="sxs-lookup"><span data-stu-id="187cc-314">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="187cc-315">Platzhalterbindungen in untergeordneten Domänen (z.B. `*.mysub.com`) verursachen kein Sicherheitsrisiko, wenn Sie die gesamte übergeordnete Domäne steuern (im Gegensatz zu `*.com`, das angreifbar ist).</span><span class="sxs-lookup"><span data-stu-id="187cc-315">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="187cc-316">Weitere Informationen finden Sie unter [rfc7230 im Abschnitt 5.4](https://tools.ietf.org/html/rfc7230#section-5.4).</span><span class="sxs-lookup"><span data-stu-id="187cc-316">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

1. <span data-ttu-id="187cc-317">Wählen Sie unter dem Serverknoten **Anwendungspools** aus.</span><span class="sxs-lookup"><span data-stu-id="187cc-317">Under the server's node, select **Application Pools**.</span></span>

1. <span data-ttu-id="187cc-318">Klicken Sie mit der rechten Maustaste auf den App-Pool der Website, und wählen Sie im Kontextmenü **Grundeinstellungen** aus.</span><span class="sxs-lookup"><span data-stu-id="187cc-318">Right-click the site's app pool and select **Basic Settings** from the contextual menu.</span></span>

1. <span data-ttu-id="187cc-319">Legen Sie im Fenster **Anwendungspool bearbeiten** die **.NET CLR-Version** auf **Kein verwalteter Code** fest:</span><span class="sxs-lookup"><span data-stu-id="187cc-319">In the **Edit Application Pool** window, set the **.NET CLR version** to **No Managed Code**:</span></span>

   ![Legen Sie „Kein verwalteter Code“ für die .NET CLR-Version fest.](index/_static/edit-apppool-ws2016.png)

    <span data-ttu-id="187cc-321">ASP.NET Core wird in einem separaten Prozess ausgeführt und verwaltet die Runtime.</span><span class="sxs-lookup"><span data-stu-id="187cc-321">ASP.NET Core runs in a separate process and manages the runtime.</span></span> <span data-ttu-id="187cc-322">Für ASP.NET Core ist das Laden der Desktop-CLR nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="187cc-322">ASP.NET Core doesn't rely on loading the desktop CLR.</span></span> <span data-ttu-id="187cc-323">Das Festlegen der **.NET CLR-Version** auf **Kein verwalteter Code** ist optional.</span><span class="sxs-lookup"><span data-stu-id="187cc-323">Setting the **.NET CLR version** to **No Managed Code** is optional.</span></span>

1. <span data-ttu-id="187cc-324">Vergewissern Sie sich, dass die Prozessmodellidentität über die richtigen Berechtigungen verfügt.</span><span class="sxs-lookup"><span data-stu-id="187cc-324">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="187cc-325">Wenn die Standardidentität des App-Pools (**Prozessmodell** > **Identität**) von **ApplicationPoolIdentity** in eine andere Identität geändert wird, stellen Sie sicher, dass die neue Identität über die erforderlichen Berechtigungen zum Zugriff auf den Ordner, die Datenbank und andere erforderliche Ressourcen der App verfügt.</span><span class="sxs-lookup"><span data-stu-id="187cc-325">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span> <span data-ttu-id="187cc-326">Der App-Pool benötigt beispielsweise Lese- und Schreibzugriff für Ordner, in denen die App Lese- und Schreibvorgänge für Dateien ausführt.</span><span class="sxs-lookup"><span data-stu-id="187cc-326">For example, the app pool requires read and write access to folders where the app reads and writes files.</span></span>

<span data-ttu-id="187cc-327">**Konfigurieren der Windows-Authentifizierung (optional)**</span><span class="sxs-lookup"><span data-stu-id="187cc-327">**Windows Authentication configuration (Optional)**</span></span>  
<span data-ttu-id="187cc-328">Weitere Informationen finden Sie unter [Konfigurieren der Windows-Authentifizierung](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="187cc-328">For more information, see [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

## <a name="deploy-the-app"></a><span data-ttu-id="187cc-329">Bereitstellen der App</span><span class="sxs-lookup"><span data-stu-id="187cc-329">Deploy the app</span></span>

<span data-ttu-id="187cc-330">Stellen Sie die App in dem Ordner bereit, den Sie im Hostingsystem erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="187cc-330">Deploy the app to the folder created on the hosting system.</span></span> <span data-ttu-id="187cc-331">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) ist die empfohlene Methode zur Bereitstellung.</span><span class="sxs-lookup"><span data-stu-id="187cc-331">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment.</span></span>

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="187cc-332">Web Deploy mit Visual Studio</span><span class="sxs-lookup"><span data-stu-id="187cc-332">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="187cc-333">Informationen zum Erstellen eines Veröffentlichungsprofils zur Verwendung mit Web Deploy finden Sie im Thema [Visual Studio publish profiles for ASP.NET Core app deployment (Visual Studio-Veröffentlichungsprofile zum Bereitstellen von ASP.NET Core-Apps)](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="187cc-333">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="187cc-334">Wenn der Hostinganbieter ein Veröffentlichungsprofil oder Unterstützung für das Erstellen eines solchen Profils bereitstellt, laden Sie dessen Profil herunter, und importieren Sie es mithilfe des Visual Studio-Dialogfelds **Veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="187cc-334">If the hosting provider provides a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog.</span></span>

![Dialogfeld „Veröffentlichen“](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="187cc-336">Web Deploy außerhalb von Visual Studio</span><span class="sxs-lookup"><span data-stu-id="187cc-336">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="187cc-337">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) kann über die Befehlszeile auch außerhalb von Visual Studio verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="187cc-337">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="187cc-338">Weitere Informationen finden Sie unter [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool) (Webbereitstellungstool).</span><span class="sxs-lookup"><span data-stu-id="187cc-338">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="187cc-339">Alternativen zu Web Deploy</span><span class="sxs-lookup"><span data-stu-id="187cc-339">Alternatives to Web Deploy</span></span>

<span data-ttu-id="187cc-340">Verwenden Sie eine der folgenden Methoden, um die App zum Hostingsystem zu migrieren: manuelles Kopieren, XCopy, Robocopy oder PowerShell.</span><span class="sxs-lookup"><span data-stu-id="187cc-340">Use any of several methods to move the app to the hosting system, such as manual copy, Xcopy, Robocopy, or PowerShell.</span></span>

<span data-ttu-id="187cc-341">Weitere Informationen zum Bereitstellen von ASP.NET Core in IIS finden Sie im nachfolgenden Abschnitt [Bereitstellungsressourcen für IIS-Administratoren](#deployment-resources-for-iis-administrators).</span><span class="sxs-lookup"><span data-stu-id="187cc-341">For more information on ASP.NET Core deployment to IIS, see the [Deployment resources for IIS administrators](#deployment-resources-for-iis-administrators) section.</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="187cc-342">Navigieren auf der Website</span><span class="sxs-lookup"><span data-stu-id="187cc-342">Browse the website</span></span>

![Der Microsoft Edge-Browser hat die IIS-Startseite geladen.](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a><span data-ttu-id="187cc-344">Gesperrte Bereitstellungsdateien</span><span class="sxs-lookup"><span data-stu-id="187cc-344">Locked deployment files</span></span>

<span data-ttu-id="187cc-345">Die Dateien im Bereitstellungsordner werden gesperrt, wenn die App ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="187cc-345">Files in the deployment folder are locked when the app is running.</span></span> <span data-ttu-id="187cc-346">Gesperrte Dateien können während der Bereitstellung nicht überschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="187cc-346">Locked files can't be overwritten during deployment.</span></span> <span data-ttu-id="187cc-347">Um gesperrte Dateien in einer Bereitstellung freizugeben, beenden Sie den App-Pool mit **einer** der folgenden Methoden:</span><span class="sxs-lookup"><span data-stu-id="187cc-347">To release locked files in a deployment, stop the app pool using **one** of the following approaches:</span></span>

* <span data-ttu-id="187cc-348">Verwenden Sie Web Deploy, und verweisen Sie auf `Microsoft.NET.Sdk.Web` in der Projektdatei.</span><span class="sxs-lookup"><span data-stu-id="187cc-348">Use Web Deploy and reference `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="187cc-349">Eine Datei namens *app_offline.htm* befindet sich im Stammverzeichnis des Web-App-Verzeichnisses.</span><span class="sxs-lookup"><span data-stu-id="187cc-349">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="187cc-350">Wenn die Datei vorhanden ist, fährt das ASP.NET Core Module die App ordnungsgemäß herunter und verarbeitet die Datei *app_offline.htm* während der Bereitstellung.</span><span class="sxs-lookup"><span data-stu-id="187cc-350">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="187cc-351">Weitere Informationen finden Sie unter [Konfigurationsreferenz für das ASP.NET Core-Modul](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span><span class="sxs-lookup"><span data-stu-id="187cc-351">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>
* <span data-ttu-id="187cc-352">Beenden Sie den App-Pool im IIS-Manager auf dem Server manuell.</span><span class="sxs-lookup"><span data-stu-id="187cc-352">Manually stop the app pool in the IIS Manager on the server.</span></span>
* <span data-ttu-id="187cc-353">Verwenden Sie PowerShell, um *app_offline.html* abzulegen (erfordert PowerShell 5 oder höher):</span><span class="sxs-lookup"><span data-stu-id="187cc-353">Use PowerShell to drop *app_offline.html* (requires PowerShell 5 or later):</span></span>

  ```PowerShell
  $pathToApp = 'PATH_TO_APP'

  # Stop the AppPool
  New-Item -Path $pathToApp app_offline.htm

  # Provide script commands here to deploy the app

  # Restart the AppPool
  Remove-Item -Path $pathToApp app_offline.htm

  ```

## <a name="data-protection"></a><span data-ttu-id="187cc-354">Schutz von Daten</span><span class="sxs-lookup"><span data-stu-id="187cc-354">Data protection</span></span>

<span data-ttu-id="187cc-355">Der [ASP.NET Core-Stapel zum Schutz von Daten](xref:security/data-protection/introduction) wird von mehreren ASP.NET-[Middlewarekomponenten](xref:fundamentals/middleware/index) genutzt, darunter auch von Middleware, die bei der Authentifizierung verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="187cc-355">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including middleware used in authentication.</span></span> <span data-ttu-id="187cc-356">Selbst wenn die APIs zum Schutz von Daten nicht aus dem Benutzercode aufgerufen werden, sollte der Schutz von Daten mit einem Bereitstellungsskript oder im Benutzercode konfiguriert werden, um einen persistenten kryptografischen [Schlüsselspeicher](xref:security/data-protection/implementation/key-management) zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="187cc-356">Even if Data Protection APIs aren't called by user code, data protection should be configured with a deployment script or in user code to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="187cc-357">Wenn der Schutz von Daten nicht konfiguriert ist, werden die Schlüssel beim Neustarten der App im Arbeitsspeicher gespeichert und verworfen.</span><span class="sxs-lookup"><span data-stu-id="187cc-357">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="187cc-358">Falls der Schlüsselbund im Arbeitsspeicher gespeichert wird, wenn die App neu gestartet wird, gilt Folgendes:</span><span class="sxs-lookup"><span data-stu-id="187cc-358">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="187cc-359">Alle cookiebasierten Authentifizierungstoken für ungültig erklärt.</span><span class="sxs-lookup"><span data-stu-id="187cc-359">All cookie-based authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="187cc-360">Benutzer müssen sich bei ihrer nächsten Anforderung erneut anmelden.</span><span class="sxs-lookup"><span data-stu-id="187cc-360">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="187cc-361">Alle mit dem Schlüsselbund geschützte Daten können nicht mehr entschlüsselt werden.</span><span class="sxs-lookup"><span data-stu-id="187cc-361">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="187cc-362">Dies kann [CSRF-Token](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) und [ASP.NET Core-MVC-TempData-Cookies](xref:fundamentals/app-state#tempdata) einschließen.</span><span class="sxs-lookup"><span data-stu-id="187cc-362">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="187cc-363">Zum Konfigurieren des Schutzes von Daten unter IIS mithilfe des persistenten Schlüsselbunds verwenden Sie **einen** der folgenden Ansätze:</span><span class="sxs-lookup"><span data-stu-id="187cc-363">To configure data protection under IIS to persist the key ring, use **one** of the following approaches:</span></span>

* <span data-ttu-id="187cc-364">**Erstellen einer Registrierungsstruktur für den Schutz von Daten**</span><span class="sxs-lookup"><span data-stu-id="187cc-364">**Create Data Protection Registry Keys**</span></span>

  <span data-ttu-id="187cc-365">Schlüssel für den Schutz von Daten, die von ASP.NET Core-Apps verwendet werden, werden in der Registrierung außerhalb der Apps gespeichert.</span><span class="sxs-lookup"><span data-stu-id="187cc-365">Data protection keys used by ASP.NET Core apps are stored in the registry external to the apps.</span></span> <span data-ttu-id="187cc-366">Um die Schlüssel für eine bestimmte App zu dauerhaft zu speichern, müssen Sie Registrierungsschlüssel für den App-Pool erstellen.</span><span class="sxs-lookup"><span data-stu-id="187cc-366">To persist the keys for a given app, create registry keys for the app pool.</span></span>

  <span data-ttu-id="187cc-367">Bei eigenständigen IIS-Installationen, die ohne Webfarm vorgesehen sind, kann das [PowerShell-Skript „Provision-AutoGenKeys.ps1“ für den Schutz von Daten](https://github.com/aspnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) für jeden App-Pool genutzt werden, das mit einer ASP.NET Core-App verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="187cc-367">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/aspnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="187cc-368">Dieses Skript erstellt einen Registrierungsschlüssel in der HKLM-Registrierung, der nur für das Workerprozesskonto des App-Pools der App zugänglich ist.</span><span class="sxs-lookup"><span data-stu-id="187cc-368">This script creates a registry key in the HKLM registry that's accessible only to the worker process account of the app's app pool.</span></span> <span data-ttu-id="187cc-369">Schlüssel werden in ruhendem Zustand mit DPAPI mit einem computerweiten Schlüssel verschlüsselt.</span><span class="sxs-lookup"><span data-stu-id="187cc-369">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

  <span data-ttu-id="187cc-370">In Webfarmszenarios kann eine App so konfiguriert werden, dass sie einen UNC-Pfad verwendet, um den Schlüsselbund für den Schutz von Daten zu speichern.</span><span class="sxs-lookup"><span data-stu-id="187cc-370">In web farm scenarios, an app can be configured to use a UNC path to store its data protection key ring.</span></span> <span data-ttu-id="187cc-371">Standardmäßig werden die Schlüssel für den Schutz von Daten nicht verschlüsselt.</span><span class="sxs-lookup"><span data-stu-id="187cc-371">By default, the data protection keys aren't encrypted.</span></span> <span data-ttu-id="187cc-372">Stellen Sie sicher, dass die Dateiberechtigungen für die Netzwerkfreigabe auf das Windows-Konto beschränkt sind, mit dem die App ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="187cc-372">Ensure that the file permissions for the network share are limited to the Windows account the app runs under.</span></span> <span data-ttu-id="187cc-373">Ein X.509-Zertifikat kann zum Schutz von Schlüsseln im ruhenden Zustand verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="187cc-373">An X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="187cc-374">Ziehen Sie einen Mechanismus in Erwägung, um es Benutzern zu ermöglichen, Zertifikate hochzuladen: Platzieren Sie Zertifikate im Zertifikatspeicher des Benutzers für vertrauenswürdige Anbieter, und stellen Sie sicher, dass sie auf allen Computern verfügbar sind, auf denen die App des Benutzers ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="187cc-374">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="187cc-375">Details finden Sie unter [Konfigurieren des Schutzes von Daten in ASP.NET Core](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="187cc-375">See [Configure ASP.NET Core Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

* <span data-ttu-id="187cc-376">**Konfigurieren des IIS-Anwendungspools zum Laden des Benutzerprofils**</span><span class="sxs-lookup"><span data-stu-id="187cc-376">**Configure the IIS Application Pool to load the user profile**</span></span>

  <span data-ttu-id="187cc-377">Diese Einstellung befindet sich im Abschnitt **Prozessmodell** unter **Erweiterte Einstellungen** für den App-Pool.</span><span class="sxs-lookup"><span data-stu-id="187cc-377">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="187cc-378">Legen Sie „Benutzerprofil laden“ auf `True` fest.</span><span class="sxs-lookup"><span data-stu-id="187cc-378">Set Load User Profile to `True`.</span></span> <span data-ttu-id="187cc-379">Dadurch werden Schlüssel im Benutzerprofilverzeichnis gespeichert und mit DPAPI mit einem Schlüssel geschützt, der nur für das Benutzerkonto gilt, das vom App-Pool verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="187cc-379">This stores keys under the user profile directory and protects them using DPAPI with a key specific to the user account used by the app pool.</span></span>

* <span data-ttu-id="187cc-380">**Verwenden des Dateisystems als Schlüsselbundspeicher**</span><span class="sxs-lookup"><span data-stu-id="187cc-380">**Use the file system as a key ring store**</span></span>

  <span data-ttu-id="187cc-381">Passen Sie den App-Code so an, dass er [das Dateisystem als Schlüsselbundspeicher verwendet](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="187cc-381">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="187cc-382">Verwenden Sie ein X.509-Zertifikat, um den Schlüsselbund zu schützen, und stellen Sie sicher, dass es sich bei dem Zertifikat um ein vertrauenswürdiges Zertifikat handelt.</span><span class="sxs-lookup"><span data-stu-id="187cc-382">Use an X509 certificate to protect the key ring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="187cc-383">Wenn es sich um ein selbstsigniertes Zertifikat handelt, müssen Sie es im vertrauenswürdigen Stammspeicher platzieren.</span><span class="sxs-lookup"><span data-stu-id="187cc-383">If the certificate is self-signed, place the certificate in the Trusted Root store.</span></span>

  <span data-ttu-id="187cc-384">Wenn IIS in einer Webfarm verwendet wird:</span><span class="sxs-lookup"><span data-stu-id="187cc-384">When using IIS in a web farm:</span></span>

  * <span data-ttu-id="187cc-385">Verwenden Sie eine Dateifreigabe, auf die alle Computer zugreifen können.</span><span class="sxs-lookup"><span data-stu-id="187cc-385">Use a file share that all machines can access.</span></span>
  * <span data-ttu-id="187cc-386">Stellen Sie ein X509-Zertifikat auf jedem Computer bereit.</span><span class="sxs-lookup"><span data-stu-id="187cc-386">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="187cc-387">Konfigurieren Sie den [Schutz von Daten im Code](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="187cc-387">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

* <span data-ttu-id="187cc-388">**Festlegen einer computerweiten Richtlinie für den Schutz von Daten**</span><span class="sxs-lookup"><span data-stu-id="187cc-388">**Set a machine-wide policy for data protection**</span></span>

  <span data-ttu-id="187cc-389">Das System zum Schutz von Daten verfügt über eine eingeschränkte Unterstützung zum Festlegen einer [computerweiten Standardrichtlinie](xref:security/data-protection/configuration/machine-wide-policy) für alle Apps, die die Datenschutz-APIs nutzen.</span><span class="sxs-lookup"><span data-stu-id="187cc-389">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="187cc-390">Weitere Informationen finden Sie unter <xref:security/data-protection/introduction>.</span><span class="sxs-lookup"><span data-stu-id="187cc-390">For more information, see <xref:security/data-protection/introduction>.</span></span>

## <a name="sub-application-configuration"></a><span data-ttu-id="187cc-391">Konfiguration von untergeordneten Anwendungen</span><span class="sxs-lookup"><span data-stu-id="187cc-391">Sub-application configuration</span></span>

<span data-ttu-id="187cc-392">Untergeordnete Apps, die unter der Stamm-App hinzugefügt werden, sollten kein ASP.NET Core-Modul als Handler enthalten.</span><span class="sxs-lookup"><span data-stu-id="187cc-392">Sub-apps added under the root app shouldn't include the ASP.NET Core Module as a handler.</span></span> <span data-ttu-id="187cc-393">Wenn das Modul als Handler in einer *web.config* einer untergeordneten App hinzugefügt wird, erhalten Sie beim Versuch, zur untergeordneten App zu navigieren, den Fehler *500.19: Interner Serverfehler*. Dieser verweist auf die fehlerhafte Konfigurationsdatei.</span><span class="sxs-lookup"><span data-stu-id="187cc-393">If the module is added as a handler in a sub-app's *web.config* file, a *500.19 Internal Server Error* referencing the faulty config file is received when attempting to browse the sub-app.</span></span>

<span data-ttu-id="187cc-394">Das folgende Beispiel zeigt den Inhalt einer veröffentlichten Datei *web.config* für eine untergeordnete ASP.NET Core-App:</span><span class="sxs-lookup"><span data-stu-id="187cc-394">The following example shows a published *web.config* file for an ASP.NET Core sub-app:</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <aspNetCore processPath="dotnet" 
        arguments=".\MyApp.dll" 
        stdoutLogEnabled="false" 
        stdoutLogFile=".\logs\stdout" />
    </system.webServer>
  </location>
</configuration>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="187cc-395">Wenn Sie eine untergeordnete App ohne ASP.NET Core unterhalb einer ASP.NET Core-App hosten, entfernen Sie den geerbten Handler ausdrücklich aus der Datei *Web.config* der untergeordneten App:</span><span class="sxs-lookup"><span data-stu-id="187cc-395">When hosting a non-ASP.NET Core sub-app underneath an ASP.NET Core app, explicitly remove the inherited handler in the sub-app *web.config* file:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore" />
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

<span data-ttu-id="187cc-396">Weitere Informationen zum Konfigurieren des ASP.NET Core-Moduls finden Sie im Thema [Einführung in das ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) und in der [Konfigurationsreferenz für das ASP.NET Core-Modul](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="187cc-396">For more information on configuring the ASP.NET Core Module, see the [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic and the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="187cc-397">Konfiguration von IIS mit der Datei „web.config“</span><span class="sxs-lookup"><span data-stu-id="187cc-397">Configuration of IIS with web.config</span></span>

<span data-ttu-id="187cc-398">Die IIS-Konfiguration wird im Hinblick auf IIS-Features, die für eine Reverseproxykonfiguration gelten, vom Abschnitt **\<system.webServer>** der Datei *Web.config* beeinflusst.</span><span class="sxs-lookup"><span data-stu-id="187cc-398">IIS configuration is influenced by the **\<system.webServer>** section of *web.config* for those IIS features that apply to a reverse proxy configuration.</span></span> <span data-ttu-id="187cc-399">Wenn IIS auf Systemebene für die Verwendung der dynamischen Komprimierung konfiguriert ist, kann diese Einstellung mit dem Element **\<urlCompression>** in der Datei *web.config* der App deaktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="187cc-399">If IIS is configured at the server level to use dynamic compression, the **\<urlCompression>** element in the app's *web.config* file can disable it.</span></span>

<span data-ttu-id="187cc-400">Weitere Informationen finden Sie in der [Konfigurationsreferenz für \<system.webServer>](/iis/configuration/system.webServer/), der [Konfigurationsreferenz für das ASP.NET Core-Modul](xref:host-and-deploy/aspnet-core-module) und unter [IIS-Module mit ASP.NET Core](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="187cc-400">For more information, see the [configuration reference for \<system.webServer>](/iis/configuration/system.webServer/), [ASP.NET Core Module Configuration Reference](xref:host-and-deploy/aspnet-core-module), and [IIS Modules with ASP.NET Core](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="187cc-401">Um Umgebungsvariablen für einzelne Apps festzulegen, die in isolierten App-Pools ausgeführt werden (unterstützt für IIS 10.0 oder höher), lesen Sie den Abschnitt zum *Befehl „AppCmd.exe“* im Thema [Umgebungsvariablen \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) in der IIS-Referenzdokumentation.</span><span class="sxs-lookup"><span data-stu-id="187cc-401">To set environment variables for individual apps running in isolated app pools (supported for IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="187cc-402">Konfigurationsabschnitte von „web.config“</span><span class="sxs-lookup"><span data-stu-id="187cc-402">Configuration sections of web.config</span></span>

<span data-ttu-id="187cc-403">Konfigurationsabschnitte von ASP.NET 4.x-Apps in der Datei *web.config* werden von ASP.NET Core-Apps nicht zur Konfiguration verwendet:</span><span class="sxs-lookup"><span data-stu-id="187cc-403">Configuration sections of ASP.NET 4.x apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* <span data-ttu-id="187cc-404">**\<system.web>**</span><span class="sxs-lookup"><span data-stu-id="187cc-404">**\<system.web>**</span></span>
* <span data-ttu-id="187cc-405">**\<appSettings>**</span><span class="sxs-lookup"><span data-stu-id="187cc-405">**\<appSettings>**</span></span>
* <span data-ttu-id="187cc-406">**\<connectionStrings>**</span><span class="sxs-lookup"><span data-stu-id="187cc-406">**\<connectionStrings>**</span></span>
* <span data-ttu-id="187cc-407">**\<location>**</span><span class="sxs-lookup"><span data-stu-id="187cc-407">**\<location>**</span></span>

<span data-ttu-id="187cc-408">ASP.NET Core-Apps werden mit anderen Konfigurationsanbietern konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="187cc-408">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="187cc-409">Weitere Informationen finden Sie unter [Konfiguration](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="187cc-409">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="187cc-410">Anwendungspools</span><span class="sxs-lookup"><span data-stu-id="187cc-410">Application Pools</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="187cc-411">Die App-Poolisolation wird durch das Hostingmodell bestimmt.</span><span class="sxs-lookup"><span data-stu-id="187cc-411">App pool isolation is determined by the hosting model:</span></span>

* <span data-ttu-id="187cc-412">In-Process-Hosting: Apps müssen in separaten App-Pools ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="187cc-412">In-process hosting &ndash; Apps are required to run in separate app pools.</span></span>
* <span data-ttu-id="187cc-413">Out-of-Process-Hosting: Es wird empfohlen, die Apps voneinander zu isolieren, indem Sie jede App in ihrem eigenen App-Pool ausführen.</span><span class="sxs-lookup"><span data-stu-id="187cc-413">Out-of-process hosting &ndash; We recommend isolating the apps from each other by running each app in its own app pool.</span></span>

<span data-ttu-id="187cc-414">Im IIS-Dialogfeld **Website hinzufügen** wird standardmäßig ein einzelner App-Pool pro App eingesetzt.</span><span class="sxs-lookup"><span data-stu-id="187cc-414">The IIS **Add Website** dialog defaults to a single app pool per app.</span></span> <span data-ttu-id="187cc-415">Wenn ein **Websitename** angegeben ist, wird der Text automatisch in das Textfeld **Anwendungspool** übertragen.</span><span class="sxs-lookup"><span data-stu-id="187cc-415">When a **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="187cc-416">Ein neuer App-Pool mit dem Namen der Website wird erstellt, wenn die Website hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="187cc-416">A new app pool is created using the site name when the site is added.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="187cc-417">Wenn Sie mehrere Websites auf einem Server hosten, wird empfohlen, die Apps voneinander zu isolieren, indem Sie jede App in ihrem eigenen App-Pool ausführen.</span><span class="sxs-lookup"><span data-stu-id="187cc-417">When hosting multiple websites on a server, we recommend isolating the apps from each other by running each app in its own app pool.</span></span> <span data-ttu-id="187cc-418">Im IIS-Dialogfeld **Website hinzufügen** wird standardmäßig diese Konfiguration eingesetzt.</span><span class="sxs-lookup"><span data-stu-id="187cc-418">The IIS **Add Website** dialog defaults to this configuration.</span></span> <span data-ttu-id="187cc-419">Wenn ein **Websitename** angegeben ist, wird der Text automatisch in das Textfeld **Anwendungspool** übertragen.</span><span class="sxs-lookup"><span data-stu-id="187cc-419">When a **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="187cc-420">Ein neuer App-Pool mit dem Namen der Website wird erstellt, wenn die Website hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="187cc-420">A new app pool is created using the site name when the site is added.</span></span>

::: moniker-end

## <a name="application-pool-identity"></a><span data-ttu-id="187cc-421">Identität des Anwendungspools</span><span class="sxs-lookup"><span data-stu-id="187cc-421">Application Pool Identity</span></span>

<span data-ttu-id="187cc-422">Mit einem Konto für die Identität des App-Pools können Sie eine App unter einem eindeutigen Konto ausführen, ohne Domänen oder lokale Konten erstellen und verwalten zu müssen.</span><span class="sxs-lookup"><span data-stu-id="187cc-422">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="187cc-423">Unter IIS 8.0 oder höher erstellt der IIS-Administratorworkerprozess (WAS) ein virtuelles Konto mit dem Namen des neuen App-Pools und führt die Workerprozesse des App-Pools standardmäßig unter diesem Konto aus.</span><span class="sxs-lookup"><span data-stu-id="187cc-423">On IIS 8.0 or later, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="187cc-424">Stellen Sie sicher, dass in der IIS-Verwaltungskonsole unter **Erweiterte Einstellungen** für den App-Pool die **Identität** auf **ApplicationPoolIdentity** festgelegt ist:</span><span class="sxs-lookup"><span data-stu-id="187cc-424">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![Dialogfeld „Erweiterte Einstellungen“ für den Anwendungspool](index/_static/apppool-identity.png)

<span data-ttu-id="187cc-426">Der IIS-Verwaltungsprozess erstellt im Windows-Sicherheitssystem einen sicheren Bezeichner mit dem Namen des App-Pools.</span><span class="sxs-lookup"><span data-stu-id="187cc-426">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="187cc-427">Ressourcen können mithilfe dieser Identität geschützt werden.</span><span class="sxs-lookup"><span data-stu-id="187cc-427">Resources can be secured using this identity.</span></span> <span data-ttu-id="187cc-428">Allerdings ist diese Identität kein echtes Benutzerkonto und wird in der Windows-Benutzerverwaltungskonsole nicht angezeigt.</span><span class="sxs-lookup"><span data-stu-id="187cc-428">However, this identity isn't a real user account and doesn't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="187cc-429">Wenn der IIS-Workerprozess erhöhte Rechte für den Zugriff auf Ihre Anwendung erfordert, ändern Sie die Zugriffssteuerungsliste (ACL) für das Verzeichnis mit der App:</span><span class="sxs-lookup"><span data-stu-id="187cc-429">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="187cc-430">Öffnen Sie Windows Explorer, und navigieren Sie zum Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="187cc-430">Open Windows Explorer and navigate to the directory.</span></span>

1. <span data-ttu-id="187cc-431">Klicken Sie mit der rechten Maustaste auf das Verzeichnis, und klicken Sie auf **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="187cc-431">Right-click on the directory and select **Properties**.</span></span>

1. <span data-ttu-id="187cc-432">Klicken Sie auf der Registerkarte **Sicherheit** auf die Schaltfläche **Bearbeiten** und dann auf die Schaltfläche **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="187cc-432">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

1. <span data-ttu-id="187cc-433">Wählen Sie die Schaltfläche **Speicherorte** aus, und stellen Sie sicher, dass das System ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="187cc-433">Select the **Locations** button and make sure the system is selected.</span></span>

1. <span data-ttu-id="187cc-434">Geben Sie im Bereich **Geben Sie die Namen der auszuwählenden Objekte ein** den Wert **IIS AppPool\\<Name_des_AppPools>** ein.</span><span class="sxs-lookup"><span data-stu-id="187cc-434">Enter **IIS AppPool\\<app_pool_name>** in **Enter the object names to select** area.</span></span> <span data-ttu-id="187cc-435">Klicken Sie auf die Schaltfläche **Namen überprüfen**.</span><span class="sxs-lookup"><span data-stu-id="187cc-435">Select the **Check Names** button.</span></span> <span data-ttu-id="187cc-436">Überprüfen Sie für *DefaultAppPool* die Namen mit **IIS AppPool\DefaultAppPool**.</span><span class="sxs-lookup"><span data-stu-id="187cc-436">For the *DefaultAppPool* check the names using **IIS AppPool\DefaultAppPool**.</span></span> <span data-ttu-id="187cc-437">Bei Auswahl der Schaltfläche **Namen überprüfen** wird im Bereich für Objektnamen der Wert **DefaultAppPool** angegeben.</span><span class="sxs-lookup"><span data-stu-id="187cc-437">When the **Check Names** button is selected, a value of **DefaultAppPool** is indicated in the object names area.</span></span> <span data-ttu-id="187cc-438">Es ist nicht möglich, den Namen des App-Pools direkt in den Bereich für Objektnamen einzugeben.</span><span class="sxs-lookup"><span data-stu-id="187cc-438">It isn't possible to enter the app pool name directly into the object names area.</span></span> <span data-ttu-id="187cc-439">Verwenden Sie das Format **IIS AppPool\\<Name_des_AppPools>**, wenn Sie die Objektnamen überprüfen.</span><span class="sxs-lookup"><span data-stu-id="187cc-439">Use the **IIS AppPool\\<app_pool_name>** format when checking for the object name.</span></span>

   ![Auswahl des Dialogfelds für Benutzer oder Gruppen für den App-Ordner: Der Name des App-Pools „DefaultAppPool“ wird an „IIS AppPool\"“ im Bereich der Objektnamen angehängt, bevor „Namen überprüfen“ ausgewählt wird.](index/_static/select-users-or-groups-1.png)

1. <span data-ttu-id="187cc-441">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="187cc-441">Select **OK**.</span></span>

   ![Auswahl des Dialogfelds für Benutzer oder Gruppen für den App-Ordner: Nach der Auswahl von „Namen überprüfen“ wird der Objektname „DefaultAppPool“ im Bereich der Objektnamen angezeigt.](index/_static/select-users-or-groups-2.png)

1. <span data-ttu-id="187cc-443">Standardmäßig sollten Lese- und Schreibberechtigungen gewährt werden.</span><span class="sxs-lookup"><span data-stu-id="187cc-443">Read &amp; execute permissions should be granted by default.</span></span> <span data-ttu-id="187cc-444">Erteilen Sie weitere Berechtigungen, sofern erforderlich.</span><span class="sxs-lookup"><span data-stu-id="187cc-444">Provide additional permissions as needed.</span></span>

<span data-ttu-id="187cc-445">Zugriff kann auch über eine Eingabeaufforderung mit dem Tool **ICACLS** gewährt werden.</span><span class="sxs-lookup"><span data-stu-id="187cc-445">Access can also be granted at a command prompt using the **ICACLS** tool.</span></span> <span data-ttu-id="187cc-446">Im folgenden Befehl wird als Beispiel *DefaultAppPool* verwendet:</span><span class="sxs-lookup"><span data-stu-id="187cc-446">Using the *DefaultAppPool* as an example, the following command is used:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

<span data-ttu-id="187cc-447">Weitere Informationen finden Sie im Thema [icacls](/windows-server/administration/windows-commands/icacls).</span><span class="sxs-lookup"><span data-stu-id="187cc-447">For more information, see the [icacls](/windows-server/administration/windows-commands/icacls) topic.</span></span>

## <a name="http2-support"></a><span data-ttu-id="187cc-448">HTTP/2-Unterstützung</span><span class="sxs-lookup"><span data-stu-id="187cc-448">HTTP/2 support</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="187cc-449">[HTTP/2](https://httpwg.org/specs/rfc7540.html) wird mit ASP.NET Core in den folgenden IIS-Bereitstellungsszenarien unterstützt:</span><span class="sxs-lookup"><span data-stu-id="187cc-449">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following IIS deployment scenarios:</span></span>

* <span data-ttu-id="187cc-450">In-Process</span><span class="sxs-lookup"><span data-stu-id="187cc-450">In-process</span></span>
  * <span data-ttu-id="187cc-451">Windows Server 2016/Windows 10 oder höher, IIS 10 oder höher</span><span class="sxs-lookup"><span data-stu-id="187cc-451">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="187cc-452">TLS 1.2-Verbindung oder höher</span><span class="sxs-lookup"><span data-stu-id="187cc-452">TLS 1.2 or later connection</span></span>
* <span data-ttu-id="187cc-453">Out-of-Process</span><span class="sxs-lookup"><span data-stu-id="187cc-453">Out-of-process</span></span>
  * <span data-ttu-id="187cc-454">Windows Server 2016/Windows 10 oder höher, IIS 10 oder höher</span><span class="sxs-lookup"><span data-stu-id="187cc-454">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="187cc-455">Öffentlich zugängliche Edge-Server-Verbindungen verwenden HTTP/2, aber die Reverseproxyverbindung mit dem [Kestrel-Server](xref:fundamentals/servers/kestrel) verwendet HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="187cc-455">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
  * <span data-ttu-id="187cc-456">TLS 1.2-Verbindung oder höher</span><span class="sxs-lookup"><span data-stu-id="187cc-456">TLS 1.2 or later connection</span></span>

<span data-ttu-id="187cc-457">Für eine In-Process-Bereitstellung, wenn eine HTTP/2-Verbindung hergestellt wurde, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*)-Berichte `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="187cc-457">For an in-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span> <span data-ttu-id="187cc-458">Für eine Out-of-Process-Bereitstellung. wenn eine HTTP/2-Verbindung hergestellt wurde, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*)-Berichte `HTTP/1.1`.</span><span class="sxs-lookup"><span data-stu-id="187cc-458">For an out-of-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

<span data-ttu-id="187cc-459">Weitere Informationen zu den In-Process- und Out-of-Process-Hostingmodellen finden Sie unter dem Thema <xref:fundamentals/servers/aspnet-core-module> und dem <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="187cc-459">For more information on the in-process and out-of-process hosting models, see the <xref:fundamentals/servers/aspnet-core-module> topic and the <xref:host-and-deploy/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="187cc-460">[HTTP/2](https://httpwg.org/specs/rfc7540.html) wird für Out-of-Process-Bereitstellungen unterstützt, die die folgenden Grundanforderungen erfüllen:</span><span class="sxs-lookup"><span data-stu-id="187cc-460">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported for out-of-process deployments that meet the following base requirements:</span></span>

* <span data-ttu-id="187cc-461">Windows Server 2016/Windows 10 oder höher, IIS 10 oder höher</span><span class="sxs-lookup"><span data-stu-id="187cc-461">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
* <span data-ttu-id="187cc-462">Öffentlich zugängliche Edge-Server-Verbindungen verwenden HTTP/2, aber die Reverseproxyverbindung mit dem [Kestrel-Server](xref:fundamentals/servers/kestrel) verwendet HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="187cc-462">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
* <span data-ttu-id="187cc-463">Zielframework: Nicht zutreffend für Out-of-Process-Bereitstellungen, da die HTTP/2-Verbindung vollständig von IIS verarbeitet wird.</span><span class="sxs-lookup"><span data-stu-id="187cc-463">Target framework: Not applicable to out-of-process deployments, since the HTTP/2 connection is handled entirely by IIS.</span></span>
* <span data-ttu-id="187cc-464">TLS 1.2-Verbindung oder höher</span><span class="sxs-lookup"><span data-stu-id="187cc-464">TLS 1.2 or later connection</span></span>

<span data-ttu-id="187cc-465">Wenn eine HTTP/2-Verbindung hergestellt wurde, meldet [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) `HTTP/1.1`.</span><span class="sxs-lookup"><span data-stu-id="187cc-465">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

::: moniker-end

<span data-ttu-id="187cc-466">HTTP/2 ist standardmäßig aktiviert.</span><span class="sxs-lookup"><span data-stu-id="187cc-466">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="187cc-467">Verbindungen führen ein Fallback auf HTTP/1.1 aus, wenn keine HTTP/2-Verbindung hergestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="187cc-467">Connections fall back to HTTP/1.1 if an HTTP/2 connection isn't established.</span></span> <span data-ttu-id="187cc-468">Weitere Informationen zur HTTP/2-Konfiguration mit IIS-Bereitstellungen finden Sie unter [HTTP/2 unter IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis).</span><span class="sxs-lookup"><span data-stu-id="187cc-468">For more information on HTTP/2 configuration with IIS deployments, see [HTTP/2 on IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis).</span></span>

## <a name="deployment-resources-for-iis-administrators"></a><span data-ttu-id="187cc-469">Bereitstellungsressourcen für IIS-Administratoren</span><span class="sxs-lookup"><span data-stu-id="187cc-469">Deployment resources for IIS administrators</span></span>

<span data-ttu-id="187cc-470">Ausführliche Informationen zu IIS erhalten Sie in der IIS-Dokumentation.</span><span class="sxs-lookup"><span data-stu-id="187cc-470">Learn about IIS in-depth in the IIS documentation.</span></span>  
[<span data-ttu-id="187cc-471">IIS-Dokumentation</span><span class="sxs-lookup"><span data-stu-id="187cc-471">IIS documentation</span></span>](/iis)

<span data-ttu-id="187cc-472">Erfahren Sie mehr über .NET Core-App-Bereitstellungsmodelle.</span><span class="sxs-lookup"><span data-stu-id="187cc-472">Learn about .NET Core app deployment models.</span></span>  
[<span data-ttu-id="187cc-473">.NET Core-Anwendungsbereitstellung</span><span class="sxs-lookup"><span data-stu-id="187cc-473">.NET Core application deployment</span></span>](/dotnet/core/deploying/)

<span data-ttu-id="187cc-474">Erfahren Sie, wie der Kestrel-Webserver durch das ASP.NET Core-Modul IIS oder IIS Express als Reverseproxyserver zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="187cc-474">Learn how the ASP.NET Core Module allows the Kestrel web server to use IIS or IIS Express as a reverse proxy server.</span></span>  
[<span data-ttu-id="187cc-475">ASP.NET Core-Modul</span><span class="sxs-lookup"><span data-stu-id="187cc-475">ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)

<span data-ttu-id="187cc-476">Erfahren Sie, wie Sie das ASP.NET Core-Modul so konfigurieren, dass es ASP.NET Core-Apps hosten kann.</span><span class="sxs-lookup"><span data-stu-id="187cc-476">Learn how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span>  
[<span data-ttu-id="187cc-477">Konfigurationsreferenz für das ASP.NET Core-Modul</span><span class="sxs-lookup"><span data-stu-id="187cc-477">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)

<span data-ttu-id="187cc-478">Erfahren Sie mehr über die Verzeichnisstruktur veröffentlichter ASP.NET Core-Apps.</span><span class="sxs-lookup"><span data-stu-id="187cc-478">Learn about the directory structure of published ASP.NET Core apps.</span></span>  
[<span data-ttu-id="187cc-479">Verzeichnisstruktur</span><span class="sxs-lookup"><span data-stu-id="187cc-479">Directory structure</span></span>](xref:host-and-deploy/directory-structure)

<span data-ttu-id="187cc-480">Lernen Sie aktive und inaktive IIS-Module für ASP.NET Core-Apps kennen, und erfahren Sie, wie Sie diese verwalten können.</span><span class="sxs-lookup"><span data-stu-id="187cc-480">Discover active and inactive IIS modules for ASP.NET Core apps and how to manage IIS modules.</span></span>  
[<span data-ttu-id="187cc-481">IIS-Module</span><span class="sxs-lookup"><span data-stu-id="187cc-481">IIS modules</span></span>](xref:host-and-deploy/iis/troubleshoot)

<span data-ttu-id="187cc-482">Erfahren Sie, wie Sie Probleme mit IIS-Bereitstellungen von ASP.NET Core-Apps diagnostizieren können.</span><span class="sxs-lookup"><span data-stu-id="187cc-482">Learn how to diagnose problems with IIS deployments of ASP.NET Core apps.</span></span>  
[<span data-ttu-id="187cc-483">Problembehandlung</span><span class="sxs-lookup"><span data-stu-id="187cc-483">Troubleshoot</span></span>](xref:host-and-deploy/iis/troubleshoot)

<span data-ttu-id="187cc-484">Erfahren Sie, wie Sie häufige Fehler beim Hosten von ASP.NET Core-Apps in IIS erkennen.</span><span class="sxs-lookup"><span data-stu-id="187cc-484">Distinguish common errors when hosting ASP.NET Core apps on IIS.</span></span>  
[<span data-ttu-id="187cc-485">Referenz zu häufigen Fehlern bei Azure App Service und IIS</span><span class="sxs-lookup"><span data-stu-id="187cc-485">Common errors reference for Azure App Service and IIS</span></span>](xref:host-and-deploy/azure-iis-errors-reference)

## <a name="additional-resources"></a><span data-ttu-id="187cc-486">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="187cc-486">Additional resources</span></span>

* [<span data-ttu-id="187cc-487">Einführung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="187cc-487">Introduction to ASP.NET Core</span></span>](xref:index)
* [<span data-ttu-id="187cc-488">Die offizielle Microsoft IIS-Website</span><span class="sxs-lookup"><span data-stu-id="187cc-488">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="187cc-489">Bibliothek mit technischem Inhalt zu Windows Server</span><span class="sxs-lookup"><span data-stu-id="187cc-489">Windows Server technical content library</span></span>](/windows-server/windows-server)
* [<span data-ttu-id="187cc-490">HTTP/2 unter IIS</span><span class="sxs-lookup"><span data-stu-id="187cc-490">HTTP/2 on IIS</span></span>](/iis/get-started/whats-new-in-iis-10/http2-on-iis)
