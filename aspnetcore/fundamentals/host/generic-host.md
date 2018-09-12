---
title: Generischer .NET-Host
author: guardrex
description: Erfahren Sie mehr über den generischen Host in .NET, der für das Starten der App und das Verwalten der Lebensdauer verantwortlich ist.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/generic-host
ms.openlocfilehash: e19a8a78b4c02fbae3d3acd23ee357c6003c35cf
ms.sourcegitcommit: 08bf41d4b3e696ab512b044970e8304816f8cc56
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2018
ms.locfileid: "44039964"
---
# <a name="net-generic-host"></a><span data-ttu-id="a4bec-103">Generischer .NET-Host</span><span class="sxs-lookup"><span data-stu-id="a4bec-103">.NET Generic Host</span></span>

<span data-ttu-id="a4bec-104">Von [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a4bec-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="a4bec-105">Durch .NET Core-Apps kann ein *Host* gestartet und konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="a4bec-105">.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="a4bec-106">Der Host ist verantwortlich für das Starten der App und das Verwalten der Lebensdauer.</span><span class="sxs-lookup"><span data-stu-id="a4bec-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="a4bec-107">In diesem Artikel wird der generische ASP.NET Core-Host ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)) erläutert, der für das Hosten von Apps nützlich ist, die keine HTTP-Anforderungen verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="a4bec-107">This topic covers the ASP.NET Core Generic Host ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), which is useful for hosting apps that don't process HTTP requests.</span></span> <span data-ttu-id="a4bec-108">Weitere Informationen zum Webhost ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)) finden Sie unter <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="a4bec-108">For coverage of the Web Host ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), see <xref:fundamentals/host/web-host>.</span></span>

<span data-ttu-id="a4bec-109">Das Ziel des generischen Hosts besteht darin, die HTTP-Pipeline von der Webhost-API zu entkoppeln, um mehr Szenarios zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="a4bec-109">The goal of the Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="a4bec-110">Messaging, Hintergrundtasks und andere Nicht-HTTP-Workloads, die auf dem generischen Host basieren, profitieren von übergreifenden Funktionen wie der Konfiguration, Dependency Injection (DI) und der Protokollerstellung.</span><span class="sxs-lookup"><span data-stu-id="a4bec-110">Messaging, background tasks, and other non-HTTP workloads based on the Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="a4bec-111">Der generische Host ist neu in ASP.NET Core 2.1 und ist nicht für Webhostingszenarios geeignet.</span><span class="sxs-lookup"><span data-stu-id="a4bec-111">The Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="a4bec-112">Verwenden Sie den [Webhost](xref:fundamentals/host/web-host) für Webhostingszenarios.</span><span class="sxs-lookup"><span data-stu-id="a4bec-112">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="a4bec-113">Der generische Host wird dafür entwickelt, den Webhost in einem zukünftigen Release zu ersetzen und als primäre Host-API in HTTP- und Nicht-HTTP-Szenarios zu fungieren.</span><span class="sxs-lookup"><span data-stu-id="a4bec-113">The Generic Host is under development to replace the Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="a4bec-114">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a4bec-114">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="a4bec-115">Wenn Sie die Beispiel-App in [Visual Studio Code](https://code.visualstudio.com/) ausführen, verwenden Sie ein *externes oder integriertes Terminal*.</span><span class="sxs-lookup"><span data-stu-id="a4bec-115">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="a4bec-116">Führen Sie das Beispiel nicht in `internalConsole` aus.</span><span class="sxs-lookup"><span data-stu-id="a4bec-116">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="a4bec-117">So legen Sie die Konsole in Visual Studio Code fest:</span><span class="sxs-lookup"><span data-stu-id="a4bec-117">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="a4bec-118">Öffnen Sie die Datei *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="a4bec-118">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="a4bec-119">Suchen Sie in der Konfiguration **.NET Core-Start (Konsole)** den Eintrag **Konsole**.</span><span class="sxs-lookup"><span data-stu-id="a4bec-119">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="a4bec-120">Legen Sie den Wert auf `externalTerminal` oder `integratedTerminal` fest.</span><span class="sxs-lookup"><span data-stu-id="a4bec-120">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="a4bec-121">Einführung</span><span class="sxs-lookup"><span data-stu-id="a4bec-121">Introduction</span></span>

<span data-ttu-id="a4bec-122">Die generische Hostbibliothek ist im Namespace [Microsoft.Extensions.Hosting](/dotnet/api/microsoft.extensions.hosting) verfügbar und wird vom Paket [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="a4bec-122">The Generic Host library is available in the [Microsoft.Extensions.Hosting namespace](/dotnet/api/microsoft.extensions.hosting) and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="a4bec-123">Das Paket `Microsoft.Extensions.Hosting` ist im Metapaket [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 oder höher) enthalten.</span><span class="sxs-lookup"><span data-stu-id="a4bec-123">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="a4bec-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) ist der Einstiegspunkt für die Ausführung des Codes.</span><span class="sxs-lookup"><span data-stu-id="a4bec-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) is the entry point to code execution.</span></span> <span data-ttu-id="a4bec-125">Jede Implementierung von `IHostedService` wird in der Reihenfolge der [Dienstregistrierung in ConfigureServices](#configureservices) ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="a4bec-125">Each `IHostedService` implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="a4bec-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) wird in jeder `IHostedService`-Schnittstelle aufgerufen, wenn der Host gestartet wird, und [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) wird in umgekehrter Registrierungsreihenfolge aufgerufen, wenn der Host ordnungsgemäß heruntergefahren wird.</span><span class="sxs-lookup"><span data-stu-id="a4bec-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) is called on each `IHostedService` when the host starts, and [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="a4bec-127">Einrichten eines Hosts</span><span class="sxs-lookup"><span data-stu-id="a4bec-127">Set up a host</span></span>

<span data-ttu-id="a4bec-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) ist die Hauptkomponente, die Bibliotheken und Apps für die Initialisierung, Erstellung und Ausführung des Hosts verwenden:</span><span class="sxs-lookup"><span data-stu-id="a4bec-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="host-configuration"></a><span data-ttu-id="a4bec-129">Konfiguration des Hosts</span><span class="sxs-lookup"><span data-stu-id="a4bec-129">Host configuration</span></span>

<span data-ttu-id="a4bec-130">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) basiert auf folgenden Ansätzen, um die Hostkonfigurationswerte festzulegen:</span><span class="sxs-lookup"><span data-stu-id="a4bec-130">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="a4bec-131">Konfigurations-Generator</span><span class="sxs-lookup"><span data-stu-id="a4bec-131">Configuration builder</span></span>
* <span data-ttu-id="a4bec-132">Konfiguration der Erweiterungsmethode</span><span class="sxs-lookup"><span data-stu-id="a4bec-132">Extension method configuration</span></span>

### <a name="configuration-builder"></a><span data-ttu-id="a4bec-133">Konfigurations-Generator</span><span class="sxs-lookup"><span data-stu-id="a4bec-133">Configuration builder</span></span>

<span data-ttu-id="a4bec-134">Die Konfiguration des Host-Generators wird erstellt, indem [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) aus der Implementierung von [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) abgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="a4bec-134">Host builder configuration is created by calling [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="a4bec-135">`ConfigureHostConfiguration` verwendet eine [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder)-Schnittstelle, um eine [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration)-Schnittstelle für den Host zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="a4bec-135">`ConfigureHostConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the host.</span></span> <span data-ttu-id="a4bec-136">Der Konfigurations-Generator initialisiert die [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment)-Schnittstelle, um diese im Buildprozess der App zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="a4bec-136">The configuration builder initializes the [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) for use in the app's build process.</span></span>

<span data-ttu-id="a4bec-137">Die Umgebungsvariablenkonfiguration wird nicht standardmäßig hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="a4bec-137">Environment variable configuration isn't added by default.</span></span> <span data-ttu-id="a4bec-138">Rufen Sie [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) auf dem Host-Generator auf, um den Host mit Umgebungsvariablen zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="a4bec-138">Call [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) on the host builder to configure the host from environment variables.</span></span> <span data-ttu-id="a4bec-139">`AddEnvironmentVariables` akzeptiert ein optionales benutzerdefiniertes Präfix.</span><span class="sxs-lookup"><span data-stu-id="a4bec-139">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="a4bec-140">Die Beispiel-App verwendet das Präfix `PREFIX_`.</span><span class="sxs-lookup"><span data-stu-id="a4bec-140">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="a4bec-141">Das Präfix wird entfernt, wenn die Umgebungsvariablen gelesen werden.</span><span class="sxs-lookup"><span data-stu-id="a4bec-141">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="a4bec-142">Wenn der Host der Beispiel-App konfiguriert wird, wird der Wert der Umgebungsvariable für `PREFIX_ENVIRONMENT` der Hostkonfigurationswert für den `environment`-Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="a4bec-142">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="a4bec-143">Wenn Sie [Visual Studio](https://www.visualstudio.com/) oder eine App mit `dotnet run` während der Entwicklung verwenden, können Umgebungsvariablen in der Datei *Properties/launchSettings.json* festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="a4bec-143">During development when using [Visual Studio](https://www.visualstudio.com/) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="a4bec-144">In [Visual Studio Code](https://code.visualstudio.com/) können Umgebungsvariablen während der Entwicklung in der Datei *.vscode/launch.json* festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="a4bec-144">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="a4bec-145">Weitere Informationen finden Sie unter <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="a4bec-145">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="a4bec-146">`ConfigureHostConfiguration` kann mehrfach mit additiven Ergebnissen aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="a4bec-146">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="a4bec-147">Der Host verwendet die Option, die zuletzt einen Wert festlegt.</span><span class="sxs-lookup"><span data-stu-id="a4bec-147">The host uses whichever option sets a value last.</span></span>

<span data-ttu-id="a4bec-148">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="a4bec-148">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="a4bec-149">Beispielkonfiguration für `HostBuilder` mithilfe von `ConfigureHostConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="a4bec-149">Example `HostBuilder` configuration using `ConfigureHostConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

> [!NOTE]
> <span data-ttu-id="a4bec-150">Die Erweiterungsmethode [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) kann derzeit keine Konfigurationsabschnitte (z.B. `.AddConfiguration(Configuration.GetSection("section"))`) analysieren, die von [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="a4bec-150">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="a4bec-151">Die `GetSection`-Methode filtert die Konfigurationsschlüssel des angeforderten Abschnitts, behält jedoch den Abschnittsnamen in den Schlüsseln bei (z.B. `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="a4bec-151">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:environment`).</span></span> <span data-ttu-id="a4bec-152">Die `AddConfiguration`-Methode erwartet, dass die Schlüssel den `HostBuilder`-Schlüsseln entsprechen (z.B. `environment`).</span><span class="sxs-lookup"><span data-stu-id="a4bec-152">The `AddConfiguration` method expects the keys to match the `HostBuilder` keys (for example, `environment`).</span></span> <span data-ttu-id="a4bec-153">Das Vorhandensein des Abschnittsnamens in den Schlüsseln verhindert, dass die Werte des Abschnitts den Host konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="a4bec-153">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="a4bec-154">Dieses Problem wird in einer bevorstehenden Version behoben.</span><span class="sxs-lookup"><span data-stu-id="a4bec-154">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="a4bec-155">Weitere Informationen und Problemumgehungen finden Sie unter [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys (Beim Übergeben des Konfigurationsabschnitts an WebHostBuilder.UseConfiguration werden vollständige Schlüssel verwendet)](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="a4bec-155">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

### <a name="extension-method-configuration"></a><span data-ttu-id="a4bec-156">Konfiguration der Erweiterungsmethode</span><span class="sxs-lookup"><span data-stu-id="a4bec-156">Extension method configuration</span></span>

<span data-ttu-id="a4bec-157">Erweiterungsmethoden werden aus der Implementierung von `IHostBuilder` abgerufen, um das Inhaltsstammverzeichnis und die Umgebung zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="a4bec-157">Extension methods are called on the `IHostBuilder` implementation to configure the content root and the environment.</span></span>

#### <a name="application-key-name"></a><span data-ttu-id="a4bec-158">Anwendungsschlüssel (Name)</span><span class="sxs-lookup"><span data-stu-id="a4bec-158">Application Key (Name)</span></span>

<span data-ttu-id="a4bec-159">Die Eigenschaft [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) wird von der Hostkonfiguration während der Hostkonstruktion festgelegt.</span><span class="sxs-lookup"><span data-stu-id="a4bec-159">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is set from host configuration during host construction.</span></span> <span data-ttu-id="a4bec-160">Um den Wert explizit festzulegen, verwenden Sie den [HostDefaults.ApplicationKey](/dotnet/api/microsoft.extensions.hosting.hostdefaults.applicationkey):</span><span class="sxs-lookup"><span data-stu-id="a4bec-160">To set the value explicitly, use the [HostDefaults.ApplicationKey](/dotnet/api/microsoft.extensions.hosting.hostdefaults.applicationkey):</span></span>

<span data-ttu-id="a4bec-161">**Schlüssel:** Anwendungsname</span><span class="sxs-lookup"><span data-stu-id="a4bec-161">**Key**: applicationName</span></span>  
<span data-ttu-id="a4bec-162">**Typ:** *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="a4bec-162">**Type**: *string*</span></span>  
<span data-ttu-id="a4bec-163">**Standardwert:** Der Name der Assembly, die den Einstiegspunkt der App enthält.</span><span class="sxs-lookup"><span data-stu-id="a4bec-163">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="a4bec-164">**Festlegen mit:** `HostBuilderContext.HostingEnvironment.ApplicationName`</span><span class="sxs-lookup"><span data-stu-id="a4bec-164">**Set using**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span></span>  
<span data-ttu-id="a4bec-165">**Umgebungsvariable:** `<PREFIX_>APPLICATIONKEY` (`<PREFIX_>` ist [optional und benutzerdefiniert](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="a4bec-165">**Environment variable**: `<PREFIX_>APPLICATIONKEY` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

```csharp
var host = new HostBuilder()
    .ConfigureAppConfiguration((hostContext, configApp) =>
    {
        hostContext.HostingEnvironment.ApplicationName = "CustomApplicationName";
    })
```

#### <a name="content-root"></a><span data-ttu-id="a4bec-166">Inhaltsstammverzeichnis</span><span class="sxs-lookup"><span data-stu-id="a4bec-166">Content Root</span></span>

<span data-ttu-id="a4bec-167">Diese Einstellung bestimmt, wo der Host mit der Suche nach Inhaltsdateien beginnt.</span><span class="sxs-lookup"><span data-stu-id="a4bec-167">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="a4bec-168">**Schlüssel:** contentRoot</span><span class="sxs-lookup"><span data-stu-id="a4bec-168">**Key**: contentRoot</span></span>  
<span data-ttu-id="a4bec-169">**Typ:** *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="a4bec-169">**Type**: *string*</span></span>  
<span data-ttu-id="a4bec-170">**Standard:** Entspricht standardmäßig dem Ordner, in dem die App-Assembly gespeichert ist.</span><span class="sxs-lookup"><span data-stu-id="a4bec-170">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="a4bec-171">**Festlegen mit:** `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="a4bec-171">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="a4bec-172">**Umgebungsvariable:** `<PREFIX_>CONTENTROOT` (`<PREFIX_>` ist [optional und benutzerdefiniert](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="a4bec-172">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="a4bec-173">Wenn der Pfad nicht vorhanden ist, kann der Host nicht gestartet werden.</span><span class="sxs-lookup"><span data-stu-id="a4bec-173">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

#### <a name="environment"></a><span data-ttu-id="a4bec-174">Umgebung</span><span class="sxs-lookup"><span data-stu-id="a4bec-174">Environment</span></span>

<span data-ttu-id="a4bec-175">Legt die [Umgebung](xref:fundamentals/environments) der App fest.</span><span class="sxs-lookup"><span data-stu-id="a4bec-175">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="a4bec-176">**Schlüssel:** environment</span><span class="sxs-lookup"><span data-stu-id="a4bec-176">**Key**: environment</span></span>  
<span data-ttu-id="a4bec-177">**Typ:** *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="a4bec-177">**Type**: *string*</span></span>  
<span data-ttu-id="a4bec-178">**Standard:** Produktion</span><span class="sxs-lookup"><span data-stu-id="a4bec-178">**Default**: Production</span></span>  
<span data-ttu-id="a4bec-179">**Festlegen mit:** `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="a4bec-179">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="a4bec-180">**Umgebungsvariable:** `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` ist [optional und benutzerdefiniert](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="a4bec-180">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="a4bec-181">Die Umgebung kann auf einen beliebigen Wert festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="a4bec-181">The environment can be set to any value.</span></span> <span data-ttu-id="a4bec-182">Zu den durch Frameworks definierten Werten zählen `Development`, `Staging` und `Production`.</span><span class="sxs-lookup"><span data-stu-id="a4bec-182">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="a4bec-183">Die Groß-/Kleinschreibung wird bei Werten nicht beachtet.</span><span class="sxs-lookup"><span data-stu-id="a4bec-183">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

## <a name="configureappconfiguration"></a><span data-ttu-id="a4bec-184">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="a4bec-184">ConfigureAppConfiguration</span></span>

<span data-ttu-id="a4bec-185">Die Konfiguration des App-Generators wird erstellt, indem [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) aus der Implementierung von [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) abgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="a4bec-185">App builder configuration is created by calling [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="a4bec-186">`ConfigureAppConfiguration` verwendet eine [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder)-Schnittstelle, um eine [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration)-Schnittstelle für die App zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="a4bec-186">`ConfigureAppConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the app.</span></span> <span data-ttu-id="a4bec-187">`ConfigureAppConfiguration` kann mehrfach mit additiven Ergebnissen aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="a4bec-187">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="a4bec-188">Die App verwendet die Option, die zuletzt einen Wert festlegt.</span><span class="sxs-lookup"><span data-stu-id="a4bec-188">The app uses whichever option sets a value last.</span></span> <span data-ttu-id="a4bec-189">Die von `ConfigureAppConfiguration` erstellte Konfiguration ist unter [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) verfügbar und kann für nachfolgende Vorgänge und in [Diensten](/dotnet/api/microsoft.extensions.hosting.ihost.services) verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="a4bec-189">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) for subsequent operations and in [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span></span>

<span data-ttu-id="a4bec-190">Beispielkonfiguration für die App mithilfe von `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="a4bec-190">Example app configuration using `ConfigureAppConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="a4bec-191">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="a4bec-191">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="a4bec-192">*appsettings.Development.json*:</span><span class="sxs-lookup"><span data-stu-id="a4bec-192">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="a4bec-193">*appsettings.Production.json*:</span><span class="sxs-lookup"><span data-stu-id="a4bec-193">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

> [!NOTE]
> <span data-ttu-id="a4bec-194">Die Erweiterungsmethode [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) kann derzeit keine Konfigurationsabschnitte (z.B. `.AddConfiguration(Configuration.GetSection("section"))`) analysieren, die von [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="a4bec-194">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="a4bec-195">Die `GetSection`-Methode filtert die Konfigurationsschlüssel des angeforderten Abschnitts, behält jedoch den Abschnittsnamen in den Schlüsseln bei (z.B. `section:Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="a4bec-195">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:Logging:LogLevel:Default`).</span></span> <span data-ttu-id="a4bec-196">Die `AddConfiguration`-Methode erwartet eine genaue Übereinstimmung mit den Konfigurationsschlüsseln (z.B. `Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="a4bec-196">The `AddConfiguration` method expects an exact match to configuration keys (for example, `Logging:LogLevel:Default`).</span></span> <span data-ttu-id="a4bec-197">Das Vorhandensein des Abschnittsnamens in den Schlüsseln verhindert, dass die Werte des Abschnitts die App konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="a4bec-197">The presence of the section name on the keys prevents the section's values from configuring the app.</span></span> <span data-ttu-id="a4bec-198">Dieses Problem wird in einer bevorstehenden Version behoben.</span><span class="sxs-lookup"><span data-stu-id="a4bec-198">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="a4bec-199">Weitere Informationen und Problemumgehungen finden Sie unter [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys (Beim Übergeben des Konfigurationsabschnitts an WebHostBuilder.UseConfiguration werden vollständige Schlüssel verwendet)](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="a4bec-199">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="a4bec-200">Um Einstellungsdateien in das Ausgabeverzeichnis zu verschieben, geben Sie die Einstellungsdateien als [MSBuild-Projektelemente](/visualstudio/msbuild/common-msbuild-project-items) in der Projektdatei an.</span><span class="sxs-lookup"><span data-stu-id="a4bec-200">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="a4bec-201">Die Beispiel-App verschiebt ihre JSON-App-Einstellungsdateien und *hostsettings.json* mit dem folgenden **&lt;Content&gt;**-Element:</span><span class="sxs-lookup"><span data-stu-id="a4bec-201">The sample app moves its JSON app settings files and *hostsettings.json* with the following **&lt;Content:&gt;** item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

## <a name="configureservices"></a><span data-ttu-id="a4bec-202">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="a4bec-202">ConfigureServices</span></span>

<span data-ttu-id="a4bec-203">Mithilfe von [ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) werden Dienste zum [Dependency Injection](xref:fundamentals/dependency-injection)-Container der App hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="a4bec-203">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="a4bec-204">`ConfigureServices` kann mehrfach mit additiven Ergebnissen aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="a4bec-204">`ConfigureServices` can be called multiple times with additive results.</span></span>

<span data-ttu-id="a4bec-205">Ein gehosteter Dienst ist eine Klasse mit Logik für Hintergrundaufgaben, die die Schnittstelle [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) implementiert.</span><span class="sxs-lookup"><span data-stu-id="a4bec-205">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="a4bec-206">Weitere Informationen finden Sie unter <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="a4bec-206">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

<span data-ttu-id="a4bec-207">Die [Beispiel-App](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) verwendet die Erweiterungsmethode `AddHostedService`, um einen Dienst für Lebensdauerereignisse (`LifetimeEventsHostedService`) und einen zeitgesteuerten Hintergrundtask (`TimedHostedService`) zur App hinzuzufügen:</span><span class="sxs-lookup"><span data-stu-id="a4bec-207">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="a4bec-208">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="a4bec-208">ConfigureLogging</span></span>

<span data-ttu-id="a4bec-209">Durch [ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) wird ein Delegat für die Konfiguration der bereitgestellten [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder)-Schnittstelle bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="a4bec-209">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) adds a delegate for configuring the provided [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder).</span></span> <span data-ttu-id="a4bec-210">`ConfigureLogging` kann mehrfach mit additiven Ergebnissen aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="a4bec-210">`ConfigureLogging` may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="a4bec-211">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="a4bec-211">UseConsoleLifetime</span></span>

<span data-ttu-id="a4bec-212">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) wartet auf `Ctrl+C`/SIGINT oder SIGTERM und ruft [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) auf, um das Herunterfahren zu beginnen.</span><span class="sxs-lookup"><span data-stu-id="a4bec-212">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) listens for `Ctrl+C`/SIGINT or SIGTERM and calls [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) to start the shutdown process.</span></span> <span data-ttu-id="a4bec-213">`UseConsoleLifetime` hebt die Blockierung von Erweiterungen wie [RunAsync](#runasync) und [WaitForShutdownAsync](#waitforshutdownasync) auf.</span><span class="sxs-lookup"><span data-stu-id="a4bec-213">`UseConsoleLifetime` unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="a4bec-214">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) ist vorab als Standardimplementierung für die Lebensdauer registriert.</span><span class="sxs-lookup"><span data-stu-id="a4bec-214">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="a4bec-215">Die letzte registrierte Lebensdauer wird verwendet.</span><span class="sxs-lookup"><span data-stu-id="a4bec-215">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="a4bec-216">Containerkonfiguration</span><span class="sxs-lookup"><span data-stu-id="a4bec-216">Container configuration</span></span>

<span data-ttu-id="a4bec-217">Der Host kann eine [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1)-Schnittstelle verwenden, um die Integration in andere Container zu unterstützten.</span><span class="sxs-lookup"><span data-stu-id="a4bec-217">To support plugging in other containers, the host can accept an [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span></span> <span data-ttu-id="a4bec-218">Das Bereitstellen einer Factory ist kein Teil der DI-Containerregistrierung, sondern ein Host, der systemintern verwendet wird, um den entsprechenden DI-Container zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="a4bec-218">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="a4bec-219">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) überschreibt die Standardfactory, die zum Erstellen des Dienstanbieters der App verwendet wurde.</span><span class="sxs-lookup"><span data-stu-id="a4bec-219">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="a4bec-220">Die benutzerdefinierte Containerkonfiguration wird von der Methode [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) verwaltet.</span><span class="sxs-lookup"><span data-stu-id="a4bec-220">Custom container configuration is managed by the [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) method.</span></span> <span data-ttu-id="a4bec-221">`ConfigureContainer` stellt eine stark typisierte Möglichkeit für das Konfigurieren des Containers auf Basis der zugrunde liegenden Host-API bereit.</span><span class="sxs-lookup"><span data-stu-id="a4bec-221">`ConfigureContainer` provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="a4bec-222">`ConfigureContainer` kann mehrfach mit additiven Ergebnissen aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="a4bec-222">`ConfigureContainer` can be called multiple times with additive results.</span></span>

<span data-ttu-id="a4bec-223">Erstellen eines Dienstcontainers für die App:</span><span class="sxs-lookup"><span data-stu-id="a4bec-223">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="a4bec-224">Bereitstellen einer Factory für den Dienstcontainer:</span><span class="sxs-lookup"><span data-stu-id="a4bec-224">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="a4bec-225">Verwenden Sie die Factory, und konfigurieren Sie den benutzerdefinierten Dienstcontainer für die App:</span><span class="sxs-lookup"><span data-stu-id="a4bec-225">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="a4bec-226">Erweiterungen</span><span class="sxs-lookup"><span data-stu-id="a4bec-226">Extensibility</span></span>

<span data-ttu-id="a4bec-227">Die Erweiterung des Hosts wird mit der Erweiterungsmethode in `IHostBuilder` durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="a4bec-227">Host extensibility is performed with extension methods on `IHostBuilder`.</span></span> <span data-ttu-id="a4bec-228">Im folgenden Beispiel wird dargestellt, wie eine Erweiterungsmethode eine `IHostBuilder`-Implementierung mit dem [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks)-Beispiel erweitert, das in <xref:fundamentals/host/hosted-services> gezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="a4bec-228">The following example shows how an extension method extends an `IHostBuilder` implementation with the [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) example demonstrated in <xref:fundamentals/host/hosted-services>.</span></span>

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

<span data-ttu-id="a4bec-229">Eine App richtet die `UseHostedService`-Erweiterungsmethode zum Registrieren des in `T` übergebenen gehosteten Diensts ein:</span><span class="sxs-lookup"><span data-stu-id="a4bec-229">An app establishes the `UseHostedService` extension method to register the hosted service passed in `T`:</span></span>

```csharp
using System;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;

public static class Extensions
{
    public static IHostBuilder UseHostedService<T>(this IHostBuilder hostBuilder)
        where T : class, IHostedService, IDisposable
    {
        return hostBuilder.ConfigureServices(services =>
            services.AddHostedService<T>());
    }
}
```

## <a name="manage-the-host"></a><span data-ttu-id="a4bec-230">Verwalten des Hosts</span><span class="sxs-lookup"><span data-stu-id="a4bec-230">Manage the host</span></span>

<span data-ttu-id="a4bec-231">Die Implementierung von [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) ist für das Starten und Beenden der Implementierungen von `IHostedService` verantwortlich, die im Dienstcontainer registriert sind.</span><span class="sxs-lookup"><span data-stu-id="a4bec-231">The [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) implementation is responsible for starting and stopping the `IHostedService` implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="a4bec-232">Run</span><span class="sxs-lookup"><span data-stu-id="a4bec-232">Run</span></span>

<span data-ttu-id="a4bec-233">Durch [Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) wird die App gestartet und der aufrufende Thread blockiert, bis der Host heruntergefahren wird:</span><span class="sxs-lookup"><span data-stu-id="a4bec-233">[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) runs the app and blocks the calling thread until the host is shut down:</span></span>

```csharp
public class Program
{
    public void Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        host.Run();
    }
}
```

### <a name="runasync"></a><span data-ttu-id="a4bec-234">RunAsync</span><span class="sxs-lookup"><span data-stu-id="a4bec-234">RunAsync</span></span>

<span data-ttu-id="a4bec-235">Durch [RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) wird die App ausgeführt und eine `Task`-Methode ausgegeben, die abgeschlossen wird, wenn das Abbruchtoken oder das Herunterfahren ausgelöst wird:</span><span class="sxs-lookup"><span data-stu-id="a4bec-235">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) runs the app and returns a `Task` that completes when the cancellation token or shutdown is triggered:</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        await host.RunAsync();
    }
}
```

### <a name="runconsoleasync"></a><span data-ttu-id="a4bec-236">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="a4bec-236">RunConsoleAsync</span></span>

<span data-ttu-id="a4bec-237">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) aktiviert die Unterstützung der Konsole, erstellt und startet den Host und wartet auf `Ctrl+C`/SIGINT oder SIGTERM, um das Herunterfahren auszulösen.</span><span class="sxs-lookup"><span data-stu-id="a4bec-237">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) enables console support, builds and starts the host, and waits for `Ctrl+C`/SIGINT or SIGTERM to shut down.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var hostBuilder = new HostBuilder();

        await hostBuilder.RunConsoleAsync();
    }
}
```

### <a name="start-and-stopasync"></a><span data-ttu-id="a4bec-238">Start und StopAsync</span><span class="sxs-lookup"><span data-stu-id="a4bec-238">Start and StopAsync</span></span>

<span data-ttu-id="a4bec-239">[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) startet den Host synchron.</span><span class="sxs-lookup"><span data-stu-id="a4bec-239">[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) starts the host synchronously.</span></span>

<span data-ttu-id="a4bec-240">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) versucht, den Host innerhalb des angegebenen Timeouts zu beenden.</span><span class="sxs-lookup"><span data-stu-id="a4bec-240">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) attempts to stop the host within the provided timeout.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            host.Start();

            await host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

### <a name="startasync-and-stopasync"></a><span data-ttu-id="a4bec-241">StartAsync und StopAsync</span><span class="sxs-lookup"><span data-stu-id="a4bec-241">StartAsync and StopAsync</span></span>

<span data-ttu-id="a4bec-242">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) startet die App.</span><span class="sxs-lookup"><span data-stu-id="a4bec-242">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) starts the app.</span></span>

<span data-ttu-id="a4bec-243">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) stoppt die App.</span><span class="sxs-lookup"><span data-stu-id="a4bec-243">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) stops the app.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            await host.StartAsync();

            await host.StopAsync();
        }
    }
}
```

### <a name="waitforshutdown"></a><span data-ttu-id="a4bec-244">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="a4bec-244">WaitForShutdown</span></span>

<span data-ttu-id="a4bec-245">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) wird über die [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime)-Schnittstelle ausgelöst (z.B. die [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime)-Klasse, die auf `Ctrl+C`/SIGINT oder SIGTERM wartet).</span><span class="sxs-lookup"><span data-stu-id="a4bec-245">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) is triggered via the [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), such as [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (listens for `Ctrl+C`/SIGINT or SIGTERM).</span></span> <span data-ttu-id="a4bec-246">`WaitForShutdown` ruft [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) auf.</span><span class="sxs-lookup"><span data-stu-id="a4bec-246">`WaitForShutdown` calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

```csharp
public class Program
{
    public void Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            host.Start();

            host.WaitForShutdown();
        }
    }
}
```

### <a name="waitforshutdownasync"></a><span data-ttu-id="a4bec-247">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="a4bec-247">WaitForShutdownAsync</span></span>

<span data-ttu-id="a4bec-248">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) gibt eine `Task`-Methode zurück, die abgeschlossen wird, wenn das Herunterfahren über das angegebene Token ausgelöst wird, und ruft [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) auf.</span><span class="sxs-lookup"><span data-stu-id="a4bec-248">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) returns a `Task` that completes when shutdown is triggered via the given token and calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            await host.StartAsync();

            await host.WaitForShutdownAsync();
        }

    }
}
```

### <a name="external-control"></a><span data-ttu-id="a4bec-249">Externe Steuerung</span><span class="sxs-lookup"><span data-stu-id="a4bec-249">External control</span></span>

<span data-ttu-id="a4bec-250">Die externe Steuerung des Hosts kann mithilfe von Methoden durchgeführt werden, die extern aufgerufen werden können:</span><span class="sxs-lookup"><span data-stu-id="a4bec-250">External control of the host can be achieved using methods that can be called externally:</span></span>

```csharp
public class Program
{
    private IHost _host;

    public Program()
    {
        _host = new HostBuilder()
            .Build();
    }

    public async Task StartAsync()
    {
        _host.StartAsync();
    }

    public async Task StopAsync()
    {
        using (_host)
        {
            await _host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

<span data-ttu-id="a4bec-251">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) wird am Anfang der [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync)-Methode aufgerufen, die den Vorgang erst fortsetzt, wenn sie abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="a4bec-251">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) is called at the start of [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), which waits until it's complete before continuing.</span></span> <span data-ttu-id="a4bec-252">Dies kann verwendet werden, um den Start zu verzögern, bis dieser durch ein externes Ereignis initiiert wird.</span><span class="sxs-lookup"><span data-stu-id="a4bec-252">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="a4bec-253">IHostingEnvironment-Schnittstelle</span><span class="sxs-lookup"><span data-stu-id="a4bec-253">IHostingEnvironment interface</span></span>

<span data-ttu-id="a4bec-254">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) stellt Informationen über die Hostingumgebung der App bereit.</span><span class="sxs-lookup"><span data-stu-id="a4bec-254">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) provides information about the app's hosting environment.</span></span> <span data-ttu-id="a4bec-255">Verwenden Sie [Constructor Injection](xref:fundamentals/dependency-injection) zum Abrufen der `IHostingEnvironment`-Schnittstelle, um deren Eigenschaften und Erweiterungsmethoden zu verwenden:</span><span class="sxs-lookup"><span data-stu-id="a4bec-255">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

```csharp
public class MyClass
{
    private readonly IHostingEnvironment _env;

    public MyClass(IHostingEnvironment env)
    {
        _env = env;
    }

    public void DoSomething()
    {
        var environmentName = _env.EnvironmentName;
    }
}
```

<span data-ttu-id="a4bec-256">Weitere Informationen finden Sie unter <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="a4bec-256">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="a4bec-257">IApplicationLifetime-Schnittstelle</span><span class="sxs-lookup"><span data-stu-id="a4bec-257">IApplicationLifetime interface</span></span>

<span data-ttu-id="a4bec-258">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) ermöglicht Aktivitäten nach dem Starten und beim Herunterfahren (einschließlich Anforderungen für ordnungsgemäßes Herunterfahren).</span><span class="sxs-lookup"><span data-stu-id="a4bec-258">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="a4bec-259">Bei drei der Eigenschaften der Schnittstelle handelt es sich um Abbruchtokens, die zum Registrieren von `Action`-Methoden verwendet werden, durch die Ereignisse beim Starten und Herunterfahren definiert werden.</span><span class="sxs-lookup"><span data-stu-id="a4bec-259">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="a4bec-260">Abbruchtoken</span><span class="sxs-lookup"><span data-stu-id="a4bec-260">Cancellation Token</span></span> | <span data-ttu-id="a4bec-261">Wird ausgelöst, wenn&#8230;</span><span class="sxs-lookup"><span data-stu-id="a4bec-261">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| [<span data-ttu-id="a4bec-262">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="a4bec-262">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="a4bec-263">Der Host vollständig gestartet wurde.</span><span class="sxs-lookup"><span data-stu-id="a4bec-263">The host has fully started.</span></span> |
| [<span data-ttu-id="a4bec-264">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="a4bec-264">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="a4bec-265">Der Host ein ordnungsgemäßes Herunterfahren abschließt.</span><span class="sxs-lookup"><span data-stu-id="a4bec-265">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="a4bec-266">Alle Anforderungen sollten verarbeitet worden sein.</span><span class="sxs-lookup"><span data-stu-id="a4bec-266">All requests should be processed.</span></span> <span data-ttu-id="a4bec-267">Das Herunterfahren wird blockiert, bis das Ereignis abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="a4bec-267">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="a4bec-268">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="a4bec-268">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="a4bec-269">Der Host ein ordnungsgemäßes Herunterfahren ausführt.</span><span class="sxs-lookup"><span data-stu-id="a4bec-269">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="a4bec-270">Anforderungen möglicherweise noch verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="a4bec-270">Requests may still be processing.</span></span> <span data-ttu-id="a4bec-271">Das Herunterfahren wird blockiert, bis das Ereignis abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="a4bec-271">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="a4bec-272">Fügen Sie den `IApplicationLifetime`-Dienst über einen Konstruktor in eine beliebige Klasse ein.</span><span class="sxs-lookup"><span data-stu-id="a4bec-272">Constructor-inject the `IApplicationLifetime` service into any class.</span></span> <span data-ttu-id="a4bec-273">Die [Beispiel-App](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) verwendet die Konstruktorinjektion für eine `LifetimeEventsHostedService`-Klasse (eine `IHostedService`-Implementierung), um die Ereignisse zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="a4bec-273">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an `IHostedService` implementation) to register the events.</span></span>

<span data-ttu-id="a4bec-274">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="a4bec-274">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="a4bec-275">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) fordert das Beenden der Anwendung an.</span><span class="sxs-lookup"><span data-stu-id="a4bec-275">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="a4bec-276">Die folgende Klasse verwendet `StopApplication`, um eine App ordnungsgemäß herunterzufahren, wenn die `Shutdown`-Methode der Klasse aufgerufen wird:</span><span class="sxs-lookup"><span data-stu-id="a4bec-276">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="a4bec-277">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="a4bec-277">Additional resources</span></span>

* <xref:fundamentals/host/hosted-services>
* [<span data-ttu-id="a4bec-278">Hosten von Beispielrepositorys auf GitHub</span><span class="sxs-lookup"><span data-stu-id="a4bec-278">Hosting repo samples on GitHub</span></span>](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
