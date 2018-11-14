---
title: Konfigurationsreferenz für das ASP.NET Core-Modul
author: guardrex
description: Erfahren Sie, wie Sie das ASP.NET Core-Modul so konfigurieren, dass es ASP.NET Core-Apps hosten kann.
ms.author: riande
ms.custom: mvc
ms.date: 09/21/2018
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: ca86b1548c7c28a64fd391617b2e8290c1c264cf
ms.sourcegitcommit: 09affee3d234cb27ea6fe33bc113b79e68900d22
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2018
ms.locfileid: "51191359"
---
# <a name="aspnet-core-module-configuration-reference"></a><span data-ttu-id="d7646-103">Konfigurationsreferenz für das ASP.NET Core-Modul</span><span class="sxs-lookup"><span data-stu-id="d7646-103">ASP.NET Core Module configuration reference</span></span>

<span data-ttu-id="d7646-104">Von [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti) und [Justin Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="d7646-104">By [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), and [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="d7646-105">Diese Dokumentation bietet Anweisungen dazu, wie Sie das ASP.NET Core-Modul so konfigurieren, dass es ASP.NET Core-Apps hosten kann.</span><span class="sxs-lookup"><span data-stu-id="d7646-105">This document provides instructions on how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span> <span data-ttu-id="d7646-106">Eine Einführung in das ASP.NET Core-Modul und Installationsanweisungen finden Sie unter [ASP.NET Core Module overview (ASP.NET Core-Modulübersicht)](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="d7646-106">For an introduction to the ASP.NET Core Module and installation instructions, see the [ASP.NET Core Module overview](xref:fundamentals/servers/aspnet-core-module).</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="hosting-model"></a><span data-ttu-id="d7646-107">Hostmodell</span><span class="sxs-lookup"><span data-stu-id="d7646-107">Hosting model</span></span>

<span data-ttu-id="d7646-108">Für Apps, die in .NET Core 2.2 oder höher ausgeführt werden, unterstützt das Modul ein In-Process-Hostingmodell zur Verbesserung der Leistung, gemessen an Reverse-Proxy-Hosting (Out-of-Process).</span><span class="sxs-lookup"><span data-stu-id="d7646-108">For apps running on .NET Core 2.2 or later, the module supports an in-process hosting model for improved performance compared to reverse-proxy (out-of-process) hosting.</span></span> <span data-ttu-id="d7646-109">Weitere Informationen finden Sie unter <xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>.</span><span class="sxs-lookup"><span data-stu-id="d7646-109">For more information, see <xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>.</span></span>

<span data-ttu-id="d7646-110">In-Process-Hosting ist eine wählbare Option für vorhandene Apps, die [Dotnet neu](/dotnet/core/tools/dotnet-new)-Vorlagen sehen das In-Process-Hostingmodell aber als Standard für alle IIS- und IIS Express-Szenarien vor.</span><span class="sxs-lookup"><span data-stu-id="d7646-110">In-procsess hosting is opt-in for existing apps, but [dotnet new](/dotnet/core/tools/dotnet-new) templates default to the in-process hosting model for all IIS and IIS Express scenarios.</span></span>

<span data-ttu-id="d7646-111">Um eine App für In-Process-Hosting zu konfigurieren, fügen Sie der Projektdatei der App (z.B. *MyApp.csproj*) die Eigenschaft `<AspNetCoreHostingModel>` mit dem Wert `inprocess` hinzu (Out-of-Process-Hosting wird mit `outofprocess` festgelegt):</span><span class="sxs-lookup"><span data-stu-id="d7646-111">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file (for example, *MyApp.csproj*) with a value of `inprocess` (out-of-process hosting is set with `outofprocess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>inprocess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="d7646-112">Die folgenden Merkmale treffen für In-Process-Hosting zu:</span><span class="sxs-lookup"><span data-stu-id="d7646-112">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="d7646-113">Der [Kestrel-Server](xref:fundamentals/servers/kestrel) wird nicht verwendet.</span><span class="sxs-lookup"><span data-stu-id="d7646-113">The [Kestrel server](xref:fundamentals/servers/kestrel) isn't used.</span></span> <span data-ttu-id="d7646-114">Eine benutzerdefinierte Implementierung von <xref:Microsoft.AspNetCore.Hosting.Server.IServer>, `IISHttpServer`, wird als Server der App verwendet.</span><span class="sxs-lookup"><span data-stu-id="d7646-114">A custom <xref:Microsoft.AspNetCore.Hosting.Server.IServer> implementation, `IISHttpServer` acts as the app's server.</span></span>

* <span data-ttu-id="d7646-115">Das [RequestTimeout-Attribut](#attributes-of-the-aspnetcore-element) trifft auf In-Process-Hosting nicht zu.</span><span class="sxs-lookup"><span data-stu-id="d7646-115">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="d7646-116">Das gemeinsame Verwenden eines Anwendungspools durch mehrere Apps wird nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="d7646-116">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="d7646-117">Verwenden Sie einen Anwendungspool pro App.</span><span class="sxs-lookup"><span data-stu-id="d7646-117">Use one app pool per app.</span></span>

* <span data-ttu-id="d7646-118">Wenn [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) verwendet oder eine [app_offline.htm-Datei manuell in der Bereitstellung platziert wird](xref:host-and-deploy/iis/index#locked-deployment-files), wird die App möglicherweise nicht sofort beendet, wenn eine offene Verbindung vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="d7646-118">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="d7646-119">Beispielsweise kann eine Websocketverbindung eine Verzögerung beim Herunterfahren der App bewirken.</span><span class="sxs-lookup"><span data-stu-id="d7646-119">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="d7646-120">Die Architektur (Bitbreite) der App und der installierten Runtime (x64 oder x86) müssen mit der Architektur des Anwendungspools übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="d7646-120">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="d7646-121">Wenn der Host der App manuell mit `WebHostBuilder` eingerichtet wird (auf die Verwendung von [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) verzichtet wird) und die App jemals direkt auf dem Kestrel-Server ausgeführt wird (selbstgehostet), rufen Sie `UseKestrel` auf, bevor Sie `UseIISIntegration` aufrufen.</span><span class="sxs-lookup"><span data-stu-id="d7646-121">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="d7646-122">Bei umgekehrter Reihenfolge tritt beim Starten des Hosts ein Fehler auf.</span><span class="sxs-lookup"><span data-stu-id="d7646-122">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="d7646-123">Verbindungstrennungen von Clients werden erkannt.</span><span class="sxs-lookup"><span data-stu-id="d7646-123">Client disconnects are detected.</span></span> <span data-ttu-id="d7646-124">Das Abbruchtoken [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) wird abgebrochen, wenn die Verbindung mit dem Client getrennt wird.</span><span class="sxs-lookup"><span data-stu-id="d7646-124">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="d7646-125">`Directory.GetCurrentDirectory()` gibt anstelle des Anwendungsverzeichnisses das Workerverzeichnis des Prozesses zurück, der von den IIS gestartet wurde (z.B. *C:\Windows\System32\inetsrv* für *w3wp.exe*).</span><span class="sxs-lookup"><span data-stu-id="d7646-125">`Directory.GetCurrentDirectory()` returns the worker directory of the process started by IIS rather than the application directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="d7646-126">Änderungen am Hostmodell</span><span class="sxs-lookup"><span data-stu-id="d7646-126">Hosting model changes</span></span>

<span data-ttu-id="d7646-127">Wenn die Einstellung `hostingModel` in der Datei *web.config* geändert wird (im Abschnitt [Konfiguration mit web.config](#configuration-with-webconfig) erläutert), verwendet das Modul den Arbeitsprozess von IIS erneut.</span><span class="sxs-lookup"><span data-stu-id="d7646-127">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="d7646-128">Bei IIS Express verwendet das Modul den Arbeitsprozess nicht erneut, sondern löst stattdessen ein ordnungsgemäßes Herunterfahren des aktuellen IIS Express-Prozesses aus.</span><span class="sxs-lookup"><span data-stu-id="d7646-128">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="d7646-129">Mit der nächsten Anforderung an die App wird ein neuer IIS Express-Prozess erzeugt.</span><span class="sxs-lookup"><span data-stu-id="d7646-129">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="d7646-130">Prozessname</span><span class="sxs-lookup"><span data-stu-id="d7646-130">Process name</span></span>

<span data-ttu-id="d7646-131">`Process.GetCurrentProcess().ProcessName` meldet `w3wp`/`iisexpress` (In-Process) oder `dotnet` (Out-of-Process).</span><span class="sxs-lookup"><span data-stu-id="d7646-131">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

## <a name="configuration-with-webconfig"></a><span data-ttu-id="d7646-132">Konfiguration mit der Datei „web.config“</span><span class="sxs-lookup"><span data-stu-id="d7646-132">Configuration with web.config</span></span>

<span data-ttu-id="d7646-133">Das ASP.NET Core-Modul wurde mit dem `aspNetCore`-Abschnitt des `system.webServer`-Knotens in der Datei *web.config* der Site konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="d7646-133">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="d7646-134">Die folgende *web.config*-Datei wird für eine [Framework-abhängige Bereitstellung](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) veröffentlicht und konfiguriert für da sASP.NET Core-Modul die Handhabung von Siteanforderungen:</span><span class="sxs-lookup"><span data-stu-id="d7646-134">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath="dotnet" 
                  arguments=".\MyApp.dll" 
                  stdoutLogEnabled="false" 
                  stdoutLogFile=".\logs\stdout" 
                  hostingModel="inprocess" />
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
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="dotnet" 
                arguments=".\MyApp.dll" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

<span data-ttu-id="d7646-135">Die folgende *web.config*-Datei wird für eine [eigenständige Bereitstellung](/dotnet/articles/core/deploying/#self-contained-deployments-scd) veröffentlicht:</span><span class="sxs-lookup"><span data-stu-id="d7646-135">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath=".\MyApp.exe" 
                  stdoutLogEnabled="false" 
                  stdoutLogFile=".\logs\stdout" 
                  hostingModel="inprocess" />
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
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

<span data-ttu-id="d7646-136">Wenn eine App für [Azure App Service](https://azure.microsoft.com/services/app-service/) bereitgestellt wird, wird der Pfad `stdoutLogFile` auf `\\?\%home%\LogFiles\stdout` gesetzt.</span><span class="sxs-lookup"><span data-stu-id="d7646-136">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="d7646-137">Der Pfad speichert stdout-Protokolle zum Ordner *LogFiles*, einem Speicherort, der automatisch vom Dienst erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="d7646-137">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="d7646-138">Einen wichtigen Hinweis zur Konfiguration von *web.config*-Dateien in untergeordneten Apps finden Sie unter [Konfiguration von untergeordneten Anwendungen](xref:host-and-deploy/iis/index#sub-application-configuration).</span><span class="sxs-lookup"><span data-stu-id="d7646-138">See [Sub-application configuration](xref:host-and-deploy/iis/index#sub-application-configuration) for an important note pertaining to the configuration of *web.config* files in sub-apps.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="d7646-139">Attribute des aspNetCore-Elements</span><span class="sxs-lookup"><span data-stu-id="d7646-139">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="d7646-140">Attribut</span><span class="sxs-lookup"><span data-stu-id="d7646-140">Attribute</span></span> | <span data-ttu-id="d7646-141">Beschreibung </span><span class="sxs-lookup"><span data-stu-id="d7646-141">Description</span></span> | <span data-ttu-id="d7646-142">Standard</span><span class="sxs-lookup"><span data-stu-id="d7646-142">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="d7646-143">Optionales Zeichenfolgeattribut.</span><span class="sxs-lookup"><span data-stu-id="d7646-143">Optional string attribute.</span></span></p><p><span data-ttu-id="d7646-144">Argumente zur ausführbaren Datei, die in **processPath** angegeben wurde.</span><span class="sxs-lookup"><span data-stu-id="d7646-144">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="d7646-145">Optionales boolesches Attribut.</span><span class="sxs-lookup"><span data-stu-id="d7646-145">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="d7646-146">Wenn TRUE, wird die Seite **502.5: Prozessfehler** unterdrückt, und die in der Datei *web.config* konfigurierte Seite für den Statuscode 502 hat Vorrang.</span><span class="sxs-lookup"><span data-stu-id="d7646-146">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="d7646-147">Optionales boolesches Attribut.</span><span class="sxs-lookup"><span data-stu-id="d7646-147">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="d7646-148">Wenn TRUE, wird das Token an den untergeordneten Prozess weitergeleitet, der an %ASPNETCORE_PORT% als Header 'MS-ASPNETCORE-WINAUTHTOKEN' pro Anforderung lauscht.</span><span class="sxs-lookup"><span data-stu-id="d7646-148">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="d7646-149">Es liegt in der Verantwortung des entsprechenden Prozesses, CloseHandle auf dem Token pro Anforderung aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="d7646-149">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="d7646-150">Optionales Zeichenfolgeattribut.</span><span class="sxs-lookup"><span data-stu-id="d7646-150">Optional string attribute.</span></span></p><p><span data-ttu-id="d7646-151">Gibt das Hostingmodell als In-Process (`inprocess`) oder Out-of-Process (`outofprocess`) an.</span><span class="sxs-lookup"><span data-stu-id="d7646-151">Specifies the hosting model as in-process (`inprocess`) or out-of-process (`outofprocess`).</span></span></p> | `outofprocess` |
| `processesPerApplication` | <p><span data-ttu-id="d7646-152">Optionales Ganzzahlattribut.</span><span class="sxs-lookup"><span data-stu-id="d7646-152">Optional integer attribute.</span></span></p><p><span data-ttu-id="d7646-153">Gibt die Anzahl der Instanzen des Prozesses aus der Einstellung **processPath** an, die aus der App gestartet werden können.</span><span class="sxs-lookup"><span data-stu-id="d7646-153">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="d7646-154">&dagger;Für In-Process-Hosting ist dieser Wert auf `1` beschränkt.</span><span class="sxs-lookup"><span data-stu-id="d7646-154">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p> | <span data-ttu-id="d7646-155">Standardwert: `1`</span><span class="sxs-lookup"><span data-stu-id="d7646-155">Default: `1`</span></span><br><span data-ttu-id="d7646-156">Min.: `1`</span><span class="sxs-lookup"><span data-stu-id="d7646-156">Min: `1`</span></span><br><span data-ttu-id="d7646-157">Max.: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="d7646-157">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="d7646-158">Erforderliches Zeichenfolgenattribut.</span><span class="sxs-lookup"><span data-stu-id="d7646-158">Required string attribute.</span></span></p><p><span data-ttu-id="d7646-159">Pfad zur ausführbaren Datei, die einen Prozess startet, der auf HTTP-Anforderungen lauscht.</span><span class="sxs-lookup"><span data-stu-id="d7646-159">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="d7646-160">Relative Pfade werden unterstützt.</span><span class="sxs-lookup"><span data-stu-id="d7646-160">Relative paths are supported.</span></span> <span data-ttu-id="d7646-161">Wenn der Pfad mit `.` beginnt, gilt er als relativ zum Stammverzeichnis.</span><span class="sxs-lookup"><span data-stu-id="d7646-161">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="d7646-162">Optionales Ganzzahlattribut.</span><span class="sxs-lookup"><span data-stu-id="d7646-162">Optional integer attribute.</span></span></p><p><span data-ttu-id="d7646-163">Gibt an, wie viele Abstürze des in **ProcessPath** angegebenen Prozesses pro Minute zulässig sind.</span><span class="sxs-lookup"><span data-stu-id="d7646-163">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="d7646-164">Wenn dieses Limit überschritten wird, beendet das Modul das Starten des Prozesses für den Rest der Minute.</span><span class="sxs-lookup"><span data-stu-id="d7646-164">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="d7646-165">Bei In-Process-Hosting nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="d7646-165">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="d7646-166">Standardwert: `10`</span><span class="sxs-lookup"><span data-stu-id="d7646-166">Default: `10`</span></span><br><span data-ttu-id="d7646-167">Min.: `0`</span><span class="sxs-lookup"><span data-stu-id="d7646-167">Min: `0`</span></span><br><span data-ttu-id="d7646-168">Max.: `100`</span><span class="sxs-lookup"><span data-stu-id="d7646-168">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="d7646-169">Optionales TimeSpan-Attribut.</span><span class="sxs-lookup"><span data-stu-id="d7646-169">Optional timespan attribute.</span></span></p><p><span data-ttu-id="d7646-170">Gibt an, wie lange das ASP.NET Core-Modul auf eine Antwort des Prozesses wartet, der auf „% ASPNETCORE_PORT“ lauscht.</span><span class="sxs-lookup"><span data-stu-id="d7646-170">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="d7646-171">In den Versionen des ASP.NET Core-Moduls, die mit Version ASP.NET Core 2.1 oder später ausgeliefert wurden, wird `requestTimeout` in Stunden, Minuten und Sekunden angegeben.</span><span class="sxs-lookup"><span data-stu-id="d7646-171">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="d7646-172">Trifft auf In-Process-Hosting nicht zu.</span><span class="sxs-lookup"><span data-stu-id="d7646-172">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="d7646-173">Bei In-Process-Hosting wartet das Modul darauf, dass die App die Anforderung verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="d7646-173">For in-process hosting, the module waits for the app to process the request.</span></span></p> | <span data-ttu-id="d7646-174">Standardwert: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="d7646-174">Default: `00:02:00`</span></span><br><span data-ttu-id="d7646-175">Min.: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="d7646-175">Min: `00:00:00`</span></span><br><span data-ttu-id="d7646-176">Max.: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="d7646-176">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="d7646-177">Optionales Ganzzahlattribut.</span><span class="sxs-lookup"><span data-stu-id="d7646-177">Optional integer attribute.</span></span></p><p><span data-ttu-id="d7646-178">Gibt in Sekunden an, wie lange das Modul darauf wartet, dass die ausführbare Datei ordnungsgemäß beendet wird, wenn die Datei *app_offline.htm* erkannt wird.</span><span class="sxs-lookup"><span data-stu-id="d7646-178">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="d7646-179">Standardwert: `10`</span><span class="sxs-lookup"><span data-stu-id="d7646-179">Default: `10`</span></span><br><span data-ttu-id="d7646-180">Min.: `0`</span><span class="sxs-lookup"><span data-stu-id="d7646-180">Min: `0`</span></span><br><span data-ttu-id="d7646-181">Max.: `600`</span><span class="sxs-lookup"><span data-stu-id="d7646-181">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="d7646-182">Optionales Ganzzahlattribut.</span><span class="sxs-lookup"><span data-stu-id="d7646-182">Optional integer attribute.</span></span></p><p><span data-ttu-id="d7646-183">Gibt in Sekunden an, wie lange das Modul darauf wartet, dass die ausführbare Datei einen Prozess startet, der an dem Port lauscht.</span><span class="sxs-lookup"><span data-stu-id="d7646-183">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="d7646-184">Wenn dieses Zeitlimit überschritten wird, beendet das Modul den Prozess.</span><span class="sxs-lookup"><span data-stu-id="d7646-184">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="d7646-185">Das Modul versucht, den Prozess neu zu starten, wenn es eine neue Anforderung erhält, und versucht weiterhin, den Prozess bei nachfolgenden eingehenden Anforderungen neu zu starten, es sei denn, die App kann **RapidFailsPerMinute**-Anzahl in der letzten fortlaufenden Minute nicht starten.</span><span class="sxs-lookup"><span data-stu-id="d7646-185">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="d7646-186">Der Wert 0 (null) wird **nicht** als unendliches Timeout angesehen.</span><span class="sxs-lookup"><span data-stu-id="d7646-186">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="d7646-187">Standardwert: `120`</span><span class="sxs-lookup"><span data-stu-id="d7646-187">Default: `120`</span></span><br><span data-ttu-id="d7646-188">Min.: `0`</span><span class="sxs-lookup"><span data-stu-id="d7646-188">Min: `0`</span></span><br><span data-ttu-id="d7646-189">Max.: `3600`</span><span class="sxs-lookup"><span data-stu-id="d7646-189">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="d7646-190">Optionales boolesches Attribut.</span><span class="sxs-lookup"><span data-stu-id="d7646-190">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="d7646-191">Wenn TRUE, werden **stdout** und **stderr** für den Prozess, der in **processPath** angegeben wurde, zu der Datei weitergeleitet, die in **stdoutLogFile** angegeben wurde.</span><span class="sxs-lookup"><span data-stu-id="d7646-191">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="d7646-192">Optionales Zeichenfolgeattribut.</span><span class="sxs-lookup"><span data-stu-id="d7646-192">Optional string attribute.</span></span></p><p><span data-ttu-id="d7646-193">Gibt den relativen oder absoluten Pfad an, für den **stdout** und **stderr** aus dem in **ProcessPath** angegebenen Prozess protokolliert wurden.</span><span class="sxs-lookup"><span data-stu-id="d7646-193">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="d7646-194">Relative Pfade sind relativ zum Stamm der Site.</span><span class="sxs-lookup"><span data-stu-id="d7646-194">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="d7646-195">Jeder mit `.` beginnende Pfad ist zum Stammverzeichnis relativ, und alle anderen Pfade werden als absolute Pfade behandelt.</span><span class="sxs-lookup"><span data-stu-id="d7646-195">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="d7646-196">Alle im Pfad angegebenen Ordner müssen vorhanden sein, damit das Modul die Protokolldatei erstellt.</span><span class="sxs-lookup"><span data-stu-id="d7646-196">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="d7646-197">Mithilfe von Unterstrichtrennzeichen werden ein Zeitstempel, eine Prozess-ID und eine Dateierweiterung (*.log*) dem letzten Segment des Pfads **stdoutlogfile** hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="d7646-197">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="d7646-198">Wenn `.\logs\stdout` als Wert angegeben wird, wird ein stdout-Beispielprotokoll als *stdout_20180205194132_1934.log* im Ordner *logs* gespeichert, sofern es am 2.5.2018 um 19:41:32 mit Prozess-ID 1934 gespeichert wurde.</span><span class="sxs-lookup"><span data-stu-id="d7646-198">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="d7646-199">Attribut</span><span class="sxs-lookup"><span data-stu-id="d7646-199">Attribute</span></span> | <span data-ttu-id="d7646-200">Beschreibung </span><span class="sxs-lookup"><span data-stu-id="d7646-200">Description</span></span> | <span data-ttu-id="d7646-201">Standard</span><span class="sxs-lookup"><span data-stu-id="d7646-201">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="d7646-202">Optionales Zeichenfolgeattribut.</span><span class="sxs-lookup"><span data-stu-id="d7646-202">Optional string attribute.</span></span></p><p><span data-ttu-id="d7646-203">Argumente zur ausführbaren Datei, die in **processPath** angegeben wurde.</span><span class="sxs-lookup"><span data-stu-id="d7646-203">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="d7646-204">Optionales boolesches Attribut.</span><span class="sxs-lookup"><span data-stu-id="d7646-204">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="d7646-205">Wenn TRUE, wird die Seite **502.5: Prozessfehler** unterdrückt, und die in der Datei *web.config* konfigurierte Seite für den Statuscode 502 hat Vorrang.</span><span class="sxs-lookup"><span data-stu-id="d7646-205">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="d7646-206">Optionales boolesches Attribut.</span><span class="sxs-lookup"><span data-stu-id="d7646-206">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="d7646-207">Wenn TRUE, wird das Token an den untergeordneten Prozess weitergeleitet, der an %ASPNETCORE_PORT% als Header 'MS-ASPNETCORE-WINAUTHTOKEN' pro Anforderung lauscht.</span><span class="sxs-lookup"><span data-stu-id="d7646-207">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="d7646-208">Es liegt in der Verantwortung des entsprechenden Prozesses, CloseHandle auf dem Token pro Anforderung aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="d7646-208">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="d7646-209">Optionales Ganzzahlattribut.</span><span class="sxs-lookup"><span data-stu-id="d7646-209">Optional integer attribute.</span></span></p><p><span data-ttu-id="d7646-210">Gibt die Anzahl der Instanzen des Prozesses aus der Einstellung **processPath** an, die aus der App gestartet werden können.</span><span class="sxs-lookup"><span data-stu-id="d7646-210">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="d7646-211">Standardwert: `1`</span><span class="sxs-lookup"><span data-stu-id="d7646-211">Default: `1`</span></span><br><span data-ttu-id="d7646-212">Min.: `1`</span><span class="sxs-lookup"><span data-stu-id="d7646-212">Min: `1`</span></span><br><span data-ttu-id="d7646-213">Max.: `100`</span><span class="sxs-lookup"><span data-stu-id="d7646-213">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="d7646-214">Erforderliches Zeichenfolgenattribut.</span><span class="sxs-lookup"><span data-stu-id="d7646-214">Required string attribute.</span></span></p><p><span data-ttu-id="d7646-215">Pfad zur ausführbaren Datei, die einen Prozess startet, der auf HTTP-Anforderungen lauscht.</span><span class="sxs-lookup"><span data-stu-id="d7646-215">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="d7646-216">Relative Pfade werden unterstützt.</span><span class="sxs-lookup"><span data-stu-id="d7646-216">Relative paths are supported.</span></span> <span data-ttu-id="d7646-217">Wenn der Pfad mit `.` beginnt, gilt er als relativ zum Stammverzeichnis.</span><span class="sxs-lookup"><span data-stu-id="d7646-217">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="d7646-218">Optionales Ganzzahlattribut.</span><span class="sxs-lookup"><span data-stu-id="d7646-218">Optional integer attribute.</span></span></p><p><span data-ttu-id="d7646-219">Gibt an, wie viele Abstürze des in **ProcessPath** angegebenen Prozesses pro Minute zulässig sind.</span><span class="sxs-lookup"><span data-stu-id="d7646-219">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="d7646-220">Wenn dieses Limit überschritten wird, beendet das Modul das Starten des Prozesses für den Rest der Minute.</span><span class="sxs-lookup"><span data-stu-id="d7646-220">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="d7646-221">Standardwert: `10`</span><span class="sxs-lookup"><span data-stu-id="d7646-221">Default: `10`</span></span><br><span data-ttu-id="d7646-222">Min.: `0`</span><span class="sxs-lookup"><span data-stu-id="d7646-222">Min: `0`</span></span><br><span data-ttu-id="d7646-223">Max.: `100`</span><span class="sxs-lookup"><span data-stu-id="d7646-223">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="d7646-224">Optionales TimeSpan-Attribut.</span><span class="sxs-lookup"><span data-stu-id="d7646-224">Optional timespan attribute.</span></span></p><p><span data-ttu-id="d7646-225">Gibt an, wie lange das ASP.NET Core-Modul auf eine Antwort des Prozesses wartet, der auf „% ASPNETCORE_PORT“ lauscht.</span><span class="sxs-lookup"><span data-stu-id="d7646-225">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="d7646-226">In den Versionen des ASP.NET Core-Moduls, die mit Version ASP.NET Core 2.1 oder später ausgeliefert wurden, wird `requestTimeout` in Stunden, Minuten und Sekunden angegeben.</span><span class="sxs-lookup"><span data-stu-id="d7646-226">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="d7646-227">Standardwert: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="d7646-227">Default: `00:02:00`</span></span><br><span data-ttu-id="d7646-228">Min.: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="d7646-228">Min: `00:00:00`</span></span><br><span data-ttu-id="d7646-229">Max.: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="d7646-229">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="d7646-230">Optionales Ganzzahlattribut.</span><span class="sxs-lookup"><span data-stu-id="d7646-230">Optional integer attribute.</span></span></p><p><span data-ttu-id="d7646-231">Gibt in Sekunden an, wie lange das Modul darauf wartet, dass die ausführbare Datei ordnungsgemäß beendet wird, wenn die Datei *app_offline.htm* erkannt wird.</span><span class="sxs-lookup"><span data-stu-id="d7646-231">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="d7646-232">Standardwert: `10`</span><span class="sxs-lookup"><span data-stu-id="d7646-232">Default: `10`</span></span><br><span data-ttu-id="d7646-233">Min.: `0`</span><span class="sxs-lookup"><span data-stu-id="d7646-233">Min: `0`</span></span><br><span data-ttu-id="d7646-234">Max.: `600`</span><span class="sxs-lookup"><span data-stu-id="d7646-234">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="d7646-235">Optionales Ganzzahlattribut.</span><span class="sxs-lookup"><span data-stu-id="d7646-235">Optional integer attribute.</span></span></p><p><span data-ttu-id="d7646-236">Gibt in Sekunden an, wie lange das Modul darauf wartet, dass die ausführbare Datei einen Prozess startet, der an dem Port lauscht.</span><span class="sxs-lookup"><span data-stu-id="d7646-236">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="d7646-237">Wenn dieses Zeitlimit überschritten wird, beendet das Modul den Prozess.</span><span class="sxs-lookup"><span data-stu-id="d7646-237">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="d7646-238">Das Modul versucht, den Prozess neu zu starten, wenn es eine neue Anforderung erhält, und versucht weiterhin, den Prozess bei nachfolgenden eingehenden Anforderungen neu zu starten, es sei denn, die App kann **RapidFailsPerMinute**-Anzahl in der letzten fortlaufenden Minute nicht starten.</span><span class="sxs-lookup"><span data-stu-id="d7646-238">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="d7646-239">Der Wert 0 (null) wird **nicht** als unendliches Timeout angesehen.</span><span class="sxs-lookup"><span data-stu-id="d7646-239">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="d7646-240">Standardwert: `120`</span><span class="sxs-lookup"><span data-stu-id="d7646-240">Default: `120`</span></span><br><span data-ttu-id="d7646-241">Min.: `0`</span><span class="sxs-lookup"><span data-stu-id="d7646-241">Min: `0`</span></span><br><span data-ttu-id="d7646-242">Max.: `3600`</span><span class="sxs-lookup"><span data-stu-id="d7646-242">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="d7646-243">Optionales boolesches Attribut.</span><span class="sxs-lookup"><span data-stu-id="d7646-243">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="d7646-244">Wenn TRUE, werden **stdout** und **stderr** für den Prozess, der in **processPath** angegeben wurde, zu der Datei weitergeleitet, die in **stdoutLogFile** angegeben wurde.</span><span class="sxs-lookup"><span data-stu-id="d7646-244">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="d7646-245">Optionales Zeichenfolgeattribut.</span><span class="sxs-lookup"><span data-stu-id="d7646-245">Optional string attribute.</span></span></p><p><span data-ttu-id="d7646-246">Gibt den relativen oder absoluten Pfad an, für den **stdout** und **stderr** aus dem in **ProcessPath** angegebenen Prozess protokolliert wurden.</span><span class="sxs-lookup"><span data-stu-id="d7646-246">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="d7646-247">Relative Pfade sind relativ zum Stamm der Site.</span><span class="sxs-lookup"><span data-stu-id="d7646-247">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="d7646-248">Jeder mit `.` beginnende Pfad ist zum Stammverzeichnis relativ, und alle anderen Pfade werden als absolute Pfade behandelt.</span><span class="sxs-lookup"><span data-stu-id="d7646-248">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="d7646-249">Alle im Pfad angegebenen Ordner müssen vorhanden sein, damit das Modul die Protokolldatei erstellt.</span><span class="sxs-lookup"><span data-stu-id="d7646-249">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="d7646-250">Mithilfe von Unterstrichtrennzeichen werden ein Zeitstempel, eine Prozess-ID und eine Dateierweiterung (*.log*) dem letzten Segment des Pfads **stdoutlogfile** hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="d7646-250">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="d7646-251">Wenn `.\logs\stdout` als Wert angegeben wird, wird ein stdout-Beispielprotokoll als *stdout_20180205194132_1934.log* im Ordner *logs* gespeichert, sofern es am 2.5.2018 um 19:41:32 mit Prozess-ID 1934 gespeichert wurde.</span><span class="sxs-lookup"><span data-stu-id="d7646-251">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| <span data-ttu-id="d7646-252">Attribut</span><span class="sxs-lookup"><span data-stu-id="d7646-252">Attribute</span></span> | <span data-ttu-id="d7646-253">Beschreibung </span><span class="sxs-lookup"><span data-stu-id="d7646-253">Description</span></span> | <span data-ttu-id="d7646-254">Standard</span><span class="sxs-lookup"><span data-stu-id="d7646-254">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="d7646-255">Optionales Zeichenfolgeattribut.</span><span class="sxs-lookup"><span data-stu-id="d7646-255">Optional string attribute.</span></span></p><p><span data-ttu-id="d7646-256">Argumente zur ausführbaren Datei, die in **processPath** angegeben wurde.</span><span class="sxs-lookup"><span data-stu-id="d7646-256">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="d7646-257">Optionales boolesches Attribut.</span><span class="sxs-lookup"><span data-stu-id="d7646-257">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="d7646-258">Wenn TRUE, wird die Seite **502.5: Prozessfehler** unterdrückt, und die in der Datei *web.config* konfigurierte Seite für den Statuscode 502 hat Vorrang.</span><span class="sxs-lookup"><span data-stu-id="d7646-258">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="d7646-259">Optionales boolesches Attribut.</span><span class="sxs-lookup"><span data-stu-id="d7646-259">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="d7646-260">Wenn TRUE, wird das Token an den untergeordneten Prozess weitergeleitet, der an %ASPNETCORE_PORT% als Header 'MS-ASPNETCORE-WINAUTHTOKEN' pro Anforderung lauscht.</span><span class="sxs-lookup"><span data-stu-id="d7646-260">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="d7646-261">Es liegt in der Verantwortung des entsprechenden Prozesses, CloseHandle auf dem Token pro Anforderung aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="d7646-261">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="d7646-262">Optionales Ganzzahlattribut.</span><span class="sxs-lookup"><span data-stu-id="d7646-262">Optional integer attribute.</span></span></p><p><span data-ttu-id="d7646-263">Gibt die Anzahl der Instanzen des Prozesses aus der Einstellung **processPath** an, die aus der App gestartet werden können.</span><span class="sxs-lookup"><span data-stu-id="d7646-263">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="d7646-264">Standardwert: `1`</span><span class="sxs-lookup"><span data-stu-id="d7646-264">Default: `1`</span></span><br><span data-ttu-id="d7646-265">Min.: `1`</span><span class="sxs-lookup"><span data-stu-id="d7646-265">Min: `1`</span></span><br><span data-ttu-id="d7646-266">Max.: `100`</span><span class="sxs-lookup"><span data-stu-id="d7646-266">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="d7646-267">Erforderliches Zeichenfolgenattribut.</span><span class="sxs-lookup"><span data-stu-id="d7646-267">Required string attribute.</span></span></p><p><span data-ttu-id="d7646-268">Pfad zur ausführbaren Datei, die einen Prozess startet, der auf HTTP-Anforderungen lauscht.</span><span class="sxs-lookup"><span data-stu-id="d7646-268">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="d7646-269">Relative Pfade werden unterstützt.</span><span class="sxs-lookup"><span data-stu-id="d7646-269">Relative paths are supported.</span></span> <span data-ttu-id="d7646-270">Wenn der Pfad mit `.` beginnt, gilt er als relativ zum Stammverzeichnis.</span><span class="sxs-lookup"><span data-stu-id="d7646-270">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="d7646-271">Optionales Ganzzahlattribut.</span><span class="sxs-lookup"><span data-stu-id="d7646-271">Optional integer attribute.</span></span></p><p><span data-ttu-id="d7646-272">Gibt an, wie viele Abstürze des in **ProcessPath** angegebenen Prozesses pro Minute zulässig sind.</span><span class="sxs-lookup"><span data-stu-id="d7646-272">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="d7646-273">Wenn dieses Limit überschritten wird, beendet das Modul das Starten des Prozesses für den Rest der Minute.</span><span class="sxs-lookup"><span data-stu-id="d7646-273">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="d7646-274">Standardwert: `10`</span><span class="sxs-lookup"><span data-stu-id="d7646-274">Default: `10`</span></span><br><span data-ttu-id="d7646-275">Min.: `0`</span><span class="sxs-lookup"><span data-stu-id="d7646-275">Min: `0`</span></span><br><span data-ttu-id="d7646-276">Max.: `100`</span><span class="sxs-lookup"><span data-stu-id="d7646-276">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="d7646-277">Optionales TimeSpan-Attribut.</span><span class="sxs-lookup"><span data-stu-id="d7646-277">Optional timespan attribute.</span></span></p><p><span data-ttu-id="d7646-278">Gibt an, wie lange das ASP.NET Core-Modul auf eine Antwort des Prozesses wartet, der auf „% ASPNETCORE_PORT“ lauscht.</span><span class="sxs-lookup"><span data-stu-id="d7646-278">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="d7646-279">In den Versionen des ASP.NET Core-Moduls, die im Lieferumfang der Version ASP.NET Core 2.0 oder früher enthalten waren, darf der `requestTimeout` nur in ganzen Minuten angegeben werden. Andernfalls wird standardmäßig der Wert 2 Minuten angenommen.</span><span class="sxs-lookup"><span data-stu-id="d7646-279">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.0 or earlier, the `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | <span data-ttu-id="d7646-280">Standardwert: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="d7646-280">Default: `00:02:00`</span></span><br><span data-ttu-id="d7646-281">Min.: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="d7646-281">Min: `00:00:00`</span></span><br><span data-ttu-id="d7646-282">Max.: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="d7646-282">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="d7646-283">Optionales Ganzzahlattribut.</span><span class="sxs-lookup"><span data-stu-id="d7646-283">Optional integer attribute.</span></span></p><p><span data-ttu-id="d7646-284">Gibt in Sekunden an, wie lange das Modul darauf wartet, dass die ausführbare Datei ordnungsgemäß beendet wird, wenn die Datei *app_offline.htm* erkannt wird.</span><span class="sxs-lookup"><span data-stu-id="d7646-284">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="d7646-285">Standardwert: `10`</span><span class="sxs-lookup"><span data-stu-id="d7646-285">Default: `10`</span></span><br><span data-ttu-id="d7646-286">Min.: `0`</span><span class="sxs-lookup"><span data-stu-id="d7646-286">Min: `0`</span></span><br><span data-ttu-id="d7646-287">Max.: `600`</span><span class="sxs-lookup"><span data-stu-id="d7646-287">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="d7646-288">Optionales Ganzzahlattribut.</span><span class="sxs-lookup"><span data-stu-id="d7646-288">Optional integer attribute.</span></span></p><p><span data-ttu-id="d7646-289">Gibt in Sekunden an, wie lange das Modul darauf wartet, dass die ausführbare Datei einen Prozess startet, der an dem Port lauscht.</span><span class="sxs-lookup"><span data-stu-id="d7646-289">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="d7646-290">Wenn dieses Zeitlimit überschritten wird, beendet das Modul den Prozess.</span><span class="sxs-lookup"><span data-stu-id="d7646-290">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="d7646-291">Das Modul versucht, den Prozess neu zu starten, wenn es eine neue Anforderung erhält, und versucht weiterhin, den Prozess bei nachfolgenden eingehenden Anforderungen neu zu starten, es sei denn, die App kann **RapidFailsPerMinute**-Anzahl in der letzten fortlaufenden Minute nicht starten.</span><span class="sxs-lookup"><span data-stu-id="d7646-291">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | <span data-ttu-id="d7646-292">Standardwert: `120`</span><span class="sxs-lookup"><span data-stu-id="d7646-292">Default: `120`</span></span><br><span data-ttu-id="d7646-293">Min.: `0`</span><span class="sxs-lookup"><span data-stu-id="d7646-293">Min: `0`</span></span><br><span data-ttu-id="d7646-294">Max.: `3600`</span><span class="sxs-lookup"><span data-stu-id="d7646-294">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="d7646-295">Optionales boolesches Attribut.</span><span class="sxs-lookup"><span data-stu-id="d7646-295">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="d7646-296">Wenn TRUE, werden **stdout** und **stderr** für den Prozess, der in **processPath** angegeben wurde, zu der Datei weitergeleitet, die in **stdoutLogFile** angegeben wurde.</span><span class="sxs-lookup"><span data-stu-id="d7646-296">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="d7646-297">Optionales Zeichenfolgeattribut.</span><span class="sxs-lookup"><span data-stu-id="d7646-297">Optional string attribute.</span></span></p><p><span data-ttu-id="d7646-298">Gibt den relativen oder absoluten Pfad an, für den **stdout** und **stderr** aus dem in **ProcessPath** angegebenen Prozess protokolliert wurden.</span><span class="sxs-lookup"><span data-stu-id="d7646-298">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="d7646-299">Relative Pfade sind relativ zum Stamm der Site.</span><span class="sxs-lookup"><span data-stu-id="d7646-299">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="d7646-300">Jeder mit `.` beginnende Pfad ist zum Stammverzeichnis relativ, und alle anderen Pfade werden als absolute Pfade behandelt.</span><span class="sxs-lookup"><span data-stu-id="d7646-300">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="d7646-301">Alle im Pfad angegebenen Ordner müssen vorhanden sein, damit das Modul die Protokolldatei erstellt.</span><span class="sxs-lookup"><span data-stu-id="d7646-301">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="d7646-302">Mithilfe von Unterstrichtrennzeichen werden ein Zeitstempel, eine Prozess-ID und eine Dateierweiterung (*.log*) dem letzten Segment des Pfads **stdoutlogfile** hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="d7646-302">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="d7646-303">Wenn `.\logs\stdout` als Wert angegeben wird, wird ein stdout-Beispielprotokoll als *stdout_20180205194132_1934.log* im Ordner *logs* gespeichert, sofern es am 2.5.2018 um 19:41:32 mit Prozess-ID 1934 gespeichert wurde.</span><span class="sxs-lookup"><span data-stu-id="d7646-303">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="d7646-304">Festlegen von Umgebungsvariablen</span><span class="sxs-lookup"><span data-stu-id="d7646-304">Setting environment variables</span></span>

<span data-ttu-id="d7646-305">Umgebungsvariablen können für den Prozess im Attribut `processPath` angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="d7646-305">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="d7646-306">Geben Sie eine Umgebungsvariable mit dem untergeordneten Element `environmentVariable` eines `environmentVariables`-Auflistungselements an.</span><span class="sxs-lookup"><span data-stu-id="d7646-306">Specify an environment variable with the `environmentVariable` child element of an `environmentVariables` collection element.</span></span> <span data-ttu-id="d7646-307">In diesem Abschnitt festgelegte Umgebungsvariablen haben Vorrang vor Systemumgebungsvariablen.</span><span class="sxs-lookup"><span data-stu-id="d7646-307">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="d7646-308">Im folgenden Beispiel werden zwei Umgebungsvariablen festgelegt.</span><span class="sxs-lookup"><span data-stu-id="d7646-308">The following example sets two environment variables.</span></span> <span data-ttu-id="d7646-309">`ASPNETCORE_ENVIRONMENT` konfiguriert die Umgebung der App als `Development`.</span><span class="sxs-lookup"><span data-stu-id="d7646-309">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="d7646-310">Ein Entwickler setzt diesen Wert möglicherweise vorübergehend in der Datei *web.config*, um das Laden der [Seite mit Ausnahmen für Entwickler](xref:fundamentals/error-handling) beim Debugging einer App-Ausnahme zu erzwingen.</span><span class="sxs-lookup"><span data-stu-id="d7646-310">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="d7646-311">`CONFIG_DIR` ist ein Beispiel für eine benutzerdefinierte Umgebungsvariable, wobei der Entwickler Code geschrieben hat, der den Wert beim Start liest, um einen Pfad zum Laden der Konfigurationsdatei der App zu bilden.</span><span class="sxs-lookup"><span data-stu-id="d7646-311">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout"
      hostingModel="inprocess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

> [!WARNING]
> <span data-ttu-id="d7646-312">Legen Sie die Umgebungsvariable `ASPNETCORE_ENVIRONMENT` nur auf Staging- und Testservern auf `Development` fest, auf die nicht vertrauenswürdige Netzwerke, z.B. das Internet, nicht zugreifen können.</span><span class="sxs-lookup"><span data-stu-id="d7646-312">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="d7646-313">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="d7646-313">app_offline.htm</span></span>

<span data-ttu-id="d7646-314">Wenn eine Datei mit dem Namen *app_offline.htm* im Stammverzeichnis einer App erkannt wird, versucht das ASP.NET Core-Modul, die App ordnungsgemäß zu beenden und die Verarbeitung eingehender Anforderungen zu stoppen.</span><span class="sxs-lookup"><span data-stu-id="d7646-314">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="d7646-315">Wenn die App nach der Anzahl von Sekunden, die in `shutdownTimeLimit` definiert wurden, noch ausgeführt wird, beendet das ASP.NET Core-Modul den laufenden Prozess.</span><span class="sxs-lookup"><span data-stu-id="d7646-315">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="d7646-316">Wenn die Datei *app_offline.htm* vorhanden ist, reagiert das ASP.NET Core-Modul auf Anforderungen, indem es den Inhalt der *app_offline.htm*-Datei zurücksendet.</span><span class="sxs-lookup"><span data-stu-id="d7646-316">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="d7646-317">Wenn die Datei *app_offline.htm* entfernt wurde, wird die App von der nächsten Anforderung gestartet.</span><span class="sxs-lookup"><span data-stu-id="d7646-317">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="d7646-318">Beim Verwenden des Out-of-Process-Hostingmodells wird die App möglicherweise nicht sofort heruntergefahren, wenn eine offene Verbindung besteht.</span><span class="sxs-lookup"><span data-stu-id="d7646-318">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="d7646-319">Beispielsweise kann eine Websocketverbindung eine Verzögerung beim Herunterfahren der App bewirken.</span><span class="sxs-lookup"><span data-stu-id="d7646-319">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="d7646-320">Startfehler-Seite</span><span class="sxs-lookup"><span data-stu-id="d7646-320">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="d7646-321">Sowohl In-Process- als auch Out-of-Process-Hosting erzeugen benutzerdefinierte Fehlerseiten, wenn die App nicht gestartet werden kann.</span><span class="sxs-lookup"><span data-stu-id="d7646-321">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="d7646-322">Wenn das ASP.NET Core-Modul weder den In-Process- noch den Out-of-Process-Anforderungshandler finden kann, wird die Statuscodeseite *500.0: Fehler beim Laden des In-Process-/Out-Of-Process-Handlers* angezeigt.</span><span class="sxs-lookup"><span data-stu-id="d7646-322">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="d7646-323">Wenn das ASP.NET Core-Modul beim In-Process-Hosting die App nicht starten kann, wird die Statuscodeseite *500.30: Fehler beim Starten* angezeigt.</span><span class="sxs-lookup"><span data-stu-id="d7646-323">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="d7646-324">Wenn das ASP.NET Core-Modul beim Out-of-Process-Hosting den Back-End-Prozess nicht starten kann oder der Back-End-Prozess gestartet wird, aber nicht am konfigurierten Port lauscht, wird die Statuscodeseite *502.5: Prozessfehler* angezeigt.</span><span class="sxs-lookup"><span data-stu-id="d7646-324">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="d7646-325">Um diese Seite zu unterdrücken und zur standardmäßigen IIS-5xx-Statuscodeseite zurückzukehren, verwenden Sie das Attribut `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="d7646-325">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="d7646-326">Weitere Informationen zum Konfigurieren von benutzerdefinierten Fehlermeldungen finden Sie unter [HTTP-Fehler &lt;httpErrors&gt;](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="d7646-326">For more information on configuring custom error messages, see [HTTP Errors &lt;httpErrors&gt;](/iis/configuration/system.webServer/httpErrors/).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="d7646-327">Wenn das ASP.NET Core-Modul den Back-End-Prozess nicht starten kann oder der Back-End-Prozess gestartet wird, aber nicht am konfigurierten Port lauscht, wird die Statuscodeseite *502.5: Prozessfehler* angezeigt.</span><span class="sxs-lookup"><span data-stu-id="d7646-327">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="d7646-328">Um diese Seite zu unterdrücken und zur IIS-502-Sandardstatuscodeseite zurückzukehren, verwenden Sie das Attribut `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="d7646-328">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="d7646-329">Weitere Informationen zum Konfigurieren von benutzerdefinierten Fehlermeldungen finden Sie unter [HTTP-Fehler &lt;httpErrors&gt;](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="d7646-329">For more information on configuring custom error messages, see [HTTP Errors &lt;httpErrors&gt;](/iis/configuration/system.webServer/httpErrors/).</span></span>

![Statuscodeseite „502.5: Prozessfehler“](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a><span data-ttu-id="d7646-331">Protokollerstellung und Weiterleitung</span><span class="sxs-lookup"><span data-stu-id="d7646-331">Log creation and redirection</span></span>

<span data-ttu-id="d7646-332">Das ASP.NET Core-Modul leitet die Konsolenausgabe „stdout“ und „stderr“ auf den Datenträger weiter, wenn die Attribute `stdoutLogEnabled` und `stdoutLogFile` des `aspNetCore`-Elements festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="d7646-332">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="d7646-333">Alle im `stdoutLogFile`-Pfad angegebenen Ordner müssen vorhanden sein, damit das Modul die Protokolldatei erstellt.</span><span class="sxs-lookup"><span data-stu-id="d7646-333">Any folders in the `stdoutLogFile` path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="d7646-334">Der App-Pool muss über Schreibzugriff auf den Speicherort verfügen, an dem die Protokolle geschrieben werden (verwenden Sie `IIS AppPool\<app_pool_name>`, um die Schreibberechtigung bereitzustellen).</span><span class="sxs-lookup"><span data-stu-id="d7646-334">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="d7646-335">Protokolle werden nur dann rotiert, wenn die Prozesswiederverwendung/der Prozessneustart stattfindet.</span><span class="sxs-lookup"><span data-stu-id="d7646-335">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="d7646-336">Der Hoster ist für die Begrenzung des Speicherplatzes zuständig, den die Protokolle nutzen.</span><span class="sxs-lookup"><span data-stu-id="d7646-336">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="d7646-337">Die Verwendung des „stdout“-Protokolls wird zur Problembehandlung bei Startproblemen der App empfohlen.</span><span class="sxs-lookup"><span data-stu-id="d7646-337">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="d7646-338">Verwenden Sie das Protokoll „stdout“ nicht zu allgemeinen App-Protokollierungszwecken.</span><span class="sxs-lookup"><span data-stu-id="d7646-338">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="d7646-339">Verwenden Sie für die routinemäßige Protokollierung in einer ASP.NET Core-App eine Protokollierungsbibliothek, die die Protokolldateigröße beschränkt und die Protokolle rotiert.</span><span class="sxs-lookup"><span data-stu-id="d7646-339">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="d7646-340">Weitere Informationen finden Sie im Artikel zur [Protokollierung von Drittanbietern](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="d7646-340">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="d7646-341">Ein Zeitstempel und eine Dateierweiterung werden automatisch hinzugefügt, wenn die Protokolldatei erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="d7646-341">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="d7646-342">Ein Protokolldateiname wird erstellt, indem der Zeitstempel, die Prozess-ID und die Dateierweiterung (*.log*) an das letzte Segment des `stdoutLogFile`-Pfads (in der Regel *stdout*), getrennt durch Unterstriche, angehängt werden.</span><span class="sxs-lookup"><span data-stu-id="d7646-342">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="d7646-343">Wenn der `stdoutLogFile`-Pfad mit *stdout* endet, hat ein Protokoll für eine App mit der PID 1934, erstellt am 2.5.2018 um 19:42:32, den Dateinamen *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="d7646-343">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="d7646-344">Wenn `stdoutLogEnabled` „false“ ist, werden Fehler beim App-Start erfasst und in das Ereignisprotokoll mit bis zu 30 KB ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="d7646-344">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="d7646-345">Nach dem Start werden alle zusätzlichen Protokolle verworfen.</span><span class="sxs-lookup"><span data-stu-id="d7646-345">After startup, all additional logs are discarded.</span></span>

::: moniker-end

<span data-ttu-id="d7646-346">Das folgende `aspNetCore`-Beispielelement konfiguriert die stdout-Protokollierung für eine in Azure App Service gehostete App.</span><span class="sxs-lookup"><span data-stu-id="d7646-346">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="d7646-347">Ein lokaler Pfad oder ein Netzwerkfreigabepfad ist für die lokale Protokollierung zulässig.</span><span class="sxs-lookup"><span data-stu-id="d7646-347">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="d7646-348">Vergewissern Sie sich, dass die Identität des AppPool-Benutzers über die Berechtigung zum Schreiben in den angegebenen Pfad verfügt.</span><span class="sxs-lookup"><span data-stu-id="d7646-348">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="d7646-349">Erweiterte Diagnoseprotokolle</span><span class="sxs-lookup"><span data-stu-id="d7646-349">Enhanced diagnostic logs</span></span>

<span data-ttu-id="d7646-350">Das ASP.NET Core-Modul kann so konfiguriert werden, dass es erweiterte Diagnoseprotokolle bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="d7646-350">The ASP.NET Core Module provides is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="d7646-351">Fügen Sie dem `<aspNetCore>`-Element in der *web.config*-Datei das `<handlerSettings>`-Element hinzu. Wenn `debugLevel` auf `TRACE` festgelegt wird, werden genauere Diagnoseinformationen zur Verfügung gestellt:</span><span class="sxs-lookup"><span data-stu-id="d7646-351">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
  <handlerSettings>
    <handlerSetting name="debugFile" value="aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="d7646-352">`debugLevel`-Werte können sowohl die Ebene als auch den Speicherort enthalten.</span><span class="sxs-lookup"><span data-stu-id="d7646-352">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="d7646-353">Ebenen (von der geringsten zur höchsten Genauigkeit):</span><span class="sxs-lookup"><span data-stu-id="d7646-353">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="d7646-354">ERROR</span><span class="sxs-lookup"><span data-stu-id="d7646-354">ERROR</span></span>
* <span data-ttu-id="d7646-355">WARNING</span><span class="sxs-lookup"><span data-stu-id="d7646-355">WARNING</span></span>
* <span data-ttu-id="d7646-356">INFO</span><span class="sxs-lookup"><span data-stu-id="d7646-356">INFO</span></span>
* <span data-ttu-id="d7646-357">TRACE</span><span class="sxs-lookup"><span data-stu-id="d7646-357">TRACE</span></span>

<span data-ttu-id="d7646-358">Speicherorte (mehrere Speicherorte sind zulässig):</span><span class="sxs-lookup"><span data-stu-id="d7646-358">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="d7646-359">CONSOLE</span><span class="sxs-lookup"><span data-stu-id="d7646-359">CONSOLE</span></span>
* <span data-ttu-id="d7646-360">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="d7646-360">EVENTLOG</span></span>
* <span data-ttu-id="d7646-361">DATEI</span><span class="sxs-lookup"><span data-stu-id="d7646-361">FILE</span></span>

<span data-ttu-id="d7646-362">Die Handlereinstellungen können auch über Umgebungsvariablen angegeben werden:</span><span class="sxs-lookup"><span data-stu-id="d7646-362">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="d7646-363">`ASPNETCORE_MODULE_DEBUG_FILE`: Der Pfad zur Debugprotokolldatei</span><span class="sxs-lookup"><span data-stu-id="d7646-363">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="d7646-364">(Standard: *aspnetcore-debug.log*)</span><span class="sxs-lookup"><span data-stu-id="d7646-364">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="d7646-365">`ASPNETCORE_MODULE_DEBUG`: Einstellung der Debugebene</span><span class="sxs-lookup"><span data-stu-id="d7646-365">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="d7646-366">Lassen Sie die Debugprotokollierung **nicht** länger als erforderlich für die Bereitstellung aktiviert, wenn Sie einen Fehler beheben.</span><span class="sxs-lookup"><span data-stu-id="d7646-366">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="d7646-367">Die Größe des Protokolls ist nicht beschränkt.</span><span class="sxs-lookup"><span data-stu-id="d7646-367">The size of the log isn't limited.</span></span> <span data-ttu-id="d7646-368">Wenn die Debugprotokollierung aktiviert bleibt, kann der verfügbare Speicherplatz aufgebraucht werden, und der Server- oder App-Dienst können abstürzen.</span><span class="sxs-lookup"><span data-stu-id="d7646-368">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="d7646-369">[Konfiguration mit der Datei „web.config“](#configuration-with-webconfig) enthält ein Beispiel für das `aspNetCore`-Element in der Datei *web.config*.</span><span class="sxs-lookup"><span data-stu-id="d7646-369">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="d7646-370">Die Proxykonfiguration verwendet das HTTP-Protokoll und ein Paarbildungstoken</span><span class="sxs-lookup"><span data-stu-id="d7646-370">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="d7646-371">*Gilt nur für Out-of-Process-Hosting.*</span><span class="sxs-lookup"><span data-stu-id="d7646-371">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="d7646-372">Der Proxy, der zwischen dem ASP.NET Core-Modul und Kestrel erstellt wurde, verwendet das HTTP-Protokoll.</span><span class="sxs-lookup"><span data-stu-id="d7646-372">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="d7646-373">Die Verwendung von HTTP ist eine Leistungsoptimierung, bei der der Datenverkehr zwischen dem Modul und Kestrel in einer Loopbackadresse außerhalb der Netzwerkschnittstelle stattfindet.</span><span class="sxs-lookup"><span data-stu-id="d7646-373">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="d7646-374">Es gibt kein Risiko für Lauschangriffe auf den Datenverkehr zwischen dem Modul und Kestrel von einem Speicherort außerhalb des Servers.</span><span class="sxs-lookup"><span data-stu-id="d7646-374">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="d7646-375">Ein Paarbildungstoken wird verwendet, um sicherzustellen, dass die von Kestrel empfangenen Anfragen von IIS über einen Proxy gesendet wurden und nicht von einer anderen Quelle stammen.</span><span class="sxs-lookup"><span data-stu-id="d7646-375">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="d7646-376">Das Paarbildungstoken wurde durch das Modul erstellt und in einer Umgebungsvariablen (`ASPNETCORE_TOKEN`) festgelegt.</span><span class="sxs-lookup"><span data-stu-id="d7646-376">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="d7646-377">Das Paarbildungstoken ist auch bei jeder Proxyanforderung in einem Header (`MSAspNetCoreToken`) festgelegt.</span><span class="sxs-lookup"><span data-stu-id="d7646-377">The pairing token is also set into a header (`MSAspNetCoreToken`) on every proxied request.</span></span> <span data-ttu-id="d7646-378">IIS-Middleware überprüft jede erhaltene Anforderung, um sicherzustellen, dass der Headerwert des Paarbildungstokens dem Wert der Umgebungsvariablen entspricht.</span><span class="sxs-lookup"><span data-stu-id="d7646-378">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="d7646-379">Wenn die Tokenwerte nicht übereinstimmen, wird die Anforderung protokolliert und abgelehnt.</span><span class="sxs-lookup"><span data-stu-id="d7646-379">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="d7646-380">Es kann nicht von einem Speicherort außerhalb des Servers auf die Umgebungsvariablen des Paarbildungstokens und den Datenverkehr zwischen dem Modul und Kestrel zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="d7646-380">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="d7646-381">Wenn ein Angreifer den Wert des Paarbildungstokens nicht kennt, kann er keine Anforderungen einreichen, die die IIS-Middleware-Prüfung umgehen.</span><span class="sxs-lookup"><span data-stu-id="d7646-381">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="d7646-382">ASP.NET Core-Modul mit einer IIS-Freigabekonfiguration</span><span class="sxs-lookup"><span data-stu-id="d7646-382">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="d7646-383">Das Installationsprogramm des ASP.NET Core-Moduls wird mit den Berechtigungen des **SYSTEM**-Kontos ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="d7646-383">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="d7646-384">Da das lokale Systemkonto keine Berechtigung zum Ändern für den von der IIS-Freigabekonfiguration verwendeten Freigabepfad hat, stößt der Installer beim Versuch, die Moduleinstellungen in *applicationHost.config* auf der Freigabe zu konfigurieren, auf einen „Zugriff verweigert“-Fehler.</span><span class="sxs-lookup"><span data-stu-id="d7646-384">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="d7646-385">Wenn Sie eine IIS-Freigabekonfiguration verwenden, gehen Sie folgendermaßen vor:</span><span class="sxs-lookup"><span data-stu-id="d7646-385">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="d7646-386">Deaktivieren Sie die IIS-Freigabekonfiguration.</span><span class="sxs-lookup"><span data-stu-id="d7646-386">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="d7646-387">Führen Sie den Installer aus.</span><span class="sxs-lookup"><span data-stu-id="d7646-387">Run the installer.</span></span>
1. <span data-ttu-id="d7646-388">Exportieren Sie die aktualisierte Datei *applicationHost.config* auf die Freigabe.</span><span class="sxs-lookup"><span data-stu-id="d7646-388">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="d7646-389">Aktivieren Sie die IIS-Freigabekonfiguration erneut.</span><span class="sxs-lookup"><span data-stu-id="d7646-389">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="d7646-390">Version des Moduls und Installerprotokolle des Hostingpakets</span><span class="sxs-lookup"><span data-stu-id="d7646-390">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="d7646-391">So ermitteln Sie die Version des installierten ASP.NET Core-Moduls:</span><span class="sxs-lookup"><span data-stu-id="d7646-391">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="d7646-392">Navigieren Sie auf dem Hostsystem zu *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="d7646-392">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="d7646-393">Suchen Sie die Datei *aspnetcore.dll*.</span><span class="sxs-lookup"><span data-stu-id="d7646-393">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="d7646-394">Klicken Sie mit der rechten Maustaste auf die Datei, und wählen Sie im Dropdownmenü die Option **Eigenschaften** aus.</span><span class="sxs-lookup"><span data-stu-id="d7646-394">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="d7646-395">Wählen Sie die Registerkarte **Details** aus. Die **Dateiversion** und die **Produktversion** stellen die installierte Version des Moduls dar.</span><span class="sxs-lookup"><span data-stu-id="d7646-395">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="d7646-396">Die Installer-Protokolle des Hostingpakets für das Modul finden Sie unter *C:\\Benutzer\\%UserName%\\AppData\\Local\\Temp*. Die Datei heißt *dd_DotNetCoreWinSvrHosting__\<Zeitstempel>_000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="d7646-396">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="d7646-397">Dateispeicherorte für Modul, Schema und Konfiguration</span><span class="sxs-lookup"><span data-stu-id="d7646-397">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="d7646-398">Modul</span><span class="sxs-lookup"><span data-stu-id="d7646-398">Module</span></span>

<span data-ttu-id="d7646-399">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="d7646-399">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="d7646-400">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="d7646-400">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="d7646-401">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="d7646-401">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="d7646-402">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="d7646-402">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="d7646-403">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="d7646-403">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

<span data-ttu-id="d7646-404">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="d7646-404">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="d7646-405">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="d7646-405">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="d7646-406">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="d7646-406">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="d7646-407">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="d7646-407">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="d7646-408">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="d7646-408">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

### <a name="schema"></a><span data-ttu-id="d7646-409">Schema</span><span class="sxs-lookup"><span data-stu-id="d7646-409">Schema</span></span>

<span data-ttu-id="d7646-410">**IIS**</span><span class="sxs-lookup"><span data-stu-id="d7646-410">**IIS**</span></span>

   * <span data-ttu-id="d7646-411">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="d7646-411">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="d7646-412">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="d7646-412">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end
<span data-ttu-id="d7646-413">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="d7646-413">**IIS Express**</span></span>

   * <span data-ttu-id="d7646-414">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="d7646-414">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="d7646-415">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="d7646-415">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="d7646-416">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="d7646-416">Configuration</span></span>

<span data-ttu-id="d7646-417">**IIS**</span><span class="sxs-lookup"><span data-stu-id="d7646-417">**IIS**</span></span>

   * <span data-ttu-id="d7646-418">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="d7646-418">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="d7646-419">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="d7646-419">**IIS Express**</span></span>

   * <span data-ttu-id="d7646-420">%ProgramFiles%\IIS Express\config\templates\PersonalWebServer\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="d7646-420">%ProgramFiles%\IIS Express\config\templates\PersonalWebServer\applicationHost.config</span></span>

<span data-ttu-id="d7646-421">Den Speicherort dieser Dateien finden Sie, indem Sie *aspnetcore* in der Datei *applicationHost.config* suchen.</span><span class="sxs-lookup"><span data-stu-id="d7646-421">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>