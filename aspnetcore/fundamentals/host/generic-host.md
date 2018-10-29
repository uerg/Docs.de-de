---
title: Generischer .NET-Host
author: guardrex
description: Erfahren Sie mehr über den generischen Host in .NET, der für das Starten der App und das Verwalten der Lebensdauer verantwortlich ist.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/generic-host
ms.openlocfilehash: e5f91ed64b7f8402dfe938f0fa8a0d94755d15c6
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207718"
---
# <a name="net-generic-host"></a><span data-ttu-id="94a9f-103">Generischer .NET-Host</span><span class="sxs-lookup"><span data-stu-id="94a9f-103">.NET Generic Host</span></span>

<span data-ttu-id="94a9f-104">Von [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="94a9f-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="94a9f-105">Durch .NET Core-Apps kann ein *Host* gestartet und konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="94a9f-105">.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="94a9f-106">Der Host ist verantwortlich für das Starten der App und das Verwalten der Lebensdauer.</span><span class="sxs-lookup"><span data-stu-id="94a9f-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="94a9f-107">In diesem Artikel wird der generische ASP.NET Core-Host ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)) erläutert, der für das Hosten von Apps nützlich ist, die keine HTTP-Anforderungen verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="94a9f-107">This topic covers the ASP.NET Core Generic Host ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), which is useful for hosting apps that don't process HTTP requests.</span></span> <span data-ttu-id="94a9f-108">Weitere Informationen zum Webhost ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)) finden Sie unter <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="94a9f-108">For coverage of the Web Host ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), see <xref:fundamentals/host/web-host>.</span></span>

<span data-ttu-id="94a9f-109">Das Ziel des generischen Hosts besteht darin, die HTTP-Pipeline von der Webhost-API zu entkoppeln, um mehr Szenarios zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="94a9f-109">The goal of the Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="94a9f-110">Messaging, Hintergrundtasks und andere Nicht-HTTP-Workloads, die auf dem generischen Host basieren, profitieren von übergreifenden Funktionen wie der Konfiguration, Dependency Injection (DI) und der Protokollerstellung.</span><span class="sxs-lookup"><span data-stu-id="94a9f-110">Messaging, background tasks, and other non-HTTP workloads based on the Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="94a9f-111">Der generische Host ist neu in ASP.NET Core 2.1 und ist nicht für Webhostingszenarios geeignet.</span><span class="sxs-lookup"><span data-stu-id="94a9f-111">The Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="94a9f-112">Verwenden Sie den [Webhost](xref:fundamentals/host/web-host) für Webhostingszenarios.</span><span class="sxs-lookup"><span data-stu-id="94a9f-112">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="94a9f-113">Der generische Host wird dafür entwickelt, den Webhost in einem zukünftigen Release zu ersetzen und als primäre Host-API in HTTP- und Nicht-HTTP-Szenarios zu fungieren.</span><span class="sxs-lookup"><span data-stu-id="94a9f-113">The Generic Host is under development to replace the Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="94a9f-114">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([Vorgehensweise zum Herunterladen](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="94a9f-114">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="94a9f-115">Wenn Sie die Beispiel-App in [Visual Studio Code](https://code.visualstudio.com/) ausführen, verwenden Sie ein *externes oder integriertes Terminal*.</span><span class="sxs-lookup"><span data-stu-id="94a9f-115">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="94a9f-116">Führen Sie das Beispiel nicht in `internalConsole` aus.</span><span class="sxs-lookup"><span data-stu-id="94a9f-116">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="94a9f-117">So legen Sie die Konsole in Visual Studio Code fest:</span><span class="sxs-lookup"><span data-stu-id="94a9f-117">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="94a9f-118">Öffnen Sie die Datei *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="94a9f-118">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="94a9f-119">Suchen Sie in der Konfiguration **.NET Core-Start (Konsole)** den Eintrag **Konsole**.</span><span class="sxs-lookup"><span data-stu-id="94a9f-119">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="94a9f-120">Legen Sie den Wert auf `externalTerminal` oder `integratedTerminal` fest.</span><span class="sxs-lookup"><span data-stu-id="94a9f-120">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="94a9f-121">Einführung</span><span class="sxs-lookup"><span data-stu-id="94a9f-121">Introduction</span></span>

<span data-ttu-id="94a9f-122">Die generische Hostbibliothek ist im Namespace [Microsoft.Extensions.Hosting](/dotnet/api/microsoft.extensions.hosting) verfügbar und wird vom Paket [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="94a9f-122">The Generic Host library is available in the [Microsoft.Extensions.Hosting namespace](/dotnet/api/microsoft.extensions.hosting) and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="94a9f-123">Das Paket `Microsoft.Extensions.Hosting` ist im Metapaket [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 oder höher) enthalten.</span><span class="sxs-lookup"><span data-stu-id="94a9f-123">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="94a9f-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) ist der Einstiegspunkt für die Ausführung des Codes.</span><span class="sxs-lookup"><span data-stu-id="94a9f-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) is the entry point to code execution.</span></span> <span data-ttu-id="94a9f-125">Jede Implementierung von `IHostedService` wird in der Reihenfolge der [Dienstregistrierung in ConfigureServices](#configureservices) ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="94a9f-125">Each `IHostedService` implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="94a9f-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) wird in jeder `IHostedService`-Schnittstelle aufgerufen, wenn der Host gestartet wird, und [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) wird in umgekehrter Registrierungsreihenfolge aufgerufen, wenn der Host ordnungsgemäß heruntergefahren wird.</span><span class="sxs-lookup"><span data-stu-id="94a9f-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) is called on each `IHostedService` when the host starts, and [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="94a9f-127">Einrichten eines Hosts</span><span class="sxs-lookup"><span data-stu-id="94a9f-127">Set up a host</span></span>

<span data-ttu-id="94a9f-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) ist die Hauptkomponente, die Bibliotheken und Apps für die Initialisierung, Erstellung und Ausführung des Hosts verwenden:</span><span class="sxs-lookup"><span data-stu-id="94a9f-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="default-services"></a><span data-ttu-id="94a9f-129">Standarddienste</span><span class="sxs-lookup"><span data-stu-id="94a9f-129">Default services</span></span>

<span data-ttu-id="94a9f-130">Die folgenden Dienste werden bei der Hostinitialisierung registriert:</span><span class="sxs-lookup"><span data-stu-id="94a9f-130">The following services are registered during host initialization:</span></span>

* <span data-ttu-id="94a9f-131">[Umgebung](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span><span class="sxs-lookup"><span data-stu-id="94a9f-131">[Environment](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span></span>
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* <span data-ttu-id="94a9f-132">[Konfiguration](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span><span class="sxs-lookup"><span data-stu-id="94a9f-132">[Configuration](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span></span>
* <span data-ttu-id="94a9f-133"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span><span class="sxs-lookup"><span data-stu-id="94a9f-133"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span></span>
* <span data-ttu-id="94a9f-134"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span><span class="sxs-lookup"><span data-stu-id="94a9f-134"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span></span>
* <xref:Microsoft.Extensions.Hosting.IHost>
* <span data-ttu-id="94a9f-135">[Optionen](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span><span class="sxs-lookup"><span data-stu-id="94a9f-135">[Options](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span></span>
* <span data-ttu-id="94a9f-136">[Protokollierung](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span><span class="sxs-lookup"><span data-stu-id="94a9f-136">[Logging](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span></span>

## <a name="host-configuration"></a><span data-ttu-id="94a9f-137">Konfiguration des Hosts</span><span class="sxs-lookup"><span data-stu-id="94a9f-137">Host configuration</span></span>

<span data-ttu-id="94a9f-138">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) basiert auf folgenden Ansätzen, um die Hostkonfigurationswerte festzulegen:</span><span class="sxs-lookup"><span data-stu-id="94a9f-138">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="94a9f-139">Konfigurations-Generator</span><span class="sxs-lookup"><span data-stu-id="94a9f-139">Configuration builder</span></span>
* <span data-ttu-id="94a9f-140">Konfiguration der Erweiterungsmethode</span><span class="sxs-lookup"><span data-stu-id="94a9f-140">Extension method configuration</span></span>

### <a name="configuration-builder"></a><span data-ttu-id="94a9f-141">Konfigurations-Generator</span><span class="sxs-lookup"><span data-stu-id="94a9f-141">Configuration builder</span></span>

<span data-ttu-id="94a9f-142">Die Konfiguration des Host-Generators wird erstellt, indem [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) aus der Implementierung von [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) abgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="94a9f-142">Host builder configuration is created by calling [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="94a9f-143">`ConfigureHostConfiguration` verwendet eine [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder)-Schnittstelle, um eine [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration)-Schnittstelle für den Host zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="94a9f-143">`ConfigureHostConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the host.</span></span> <span data-ttu-id="94a9f-144">Der Konfigurations-Generator initialisiert die [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment)-Schnittstelle, um diese im Buildprozess der App zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="94a9f-144">The configuration builder initializes the [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) for use in the app's build process.</span></span>

<span data-ttu-id="94a9f-145">Die Umgebungsvariablenkonfiguration wird nicht standardmäßig hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="94a9f-145">Environment variable configuration isn't added by default.</span></span> <span data-ttu-id="94a9f-146">Rufen Sie [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) auf dem Host-Generator auf, um den Host mit Umgebungsvariablen zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="94a9f-146">Call [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) on the host builder to configure the host from environment variables.</span></span> <span data-ttu-id="94a9f-147">`AddEnvironmentVariables` akzeptiert ein optionales benutzerdefiniertes Präfix.</span><span class="sxs-lookup"><span data-stu-id="94a9f-147">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="94a9f-148">Die Beispiel-App verwendet das Präfix `PREFIX_`.</span><span class="sxs-lookup"><span data-stu-id="94a9f-148">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="94a9f-149">Das Präfix wird entfernt, wenn die Umgebungsvariablen gelesen werden.</span><span class="sxs-lookup"><span data-stu-id="94a9f-149">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="94a9f-150">Wenn der Host der Beispiel-App konfiguriert wird, wird der Wert der Umgebungsvariable für `PREFIX_ENVIRONMENT` der Hostkonfigurationswert für den `environment`-Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="94a9f-150">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="94a9f-151">Wenn Sie [Visual Studio](https://www.visualstudio.com/) oder eine App mit `dotnet run` während der Entwicklung verwenden, können Umgebungsvariablen in der Datei *Properties/launchSettings.json* festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="94a9f-151">During development when using [Visual Studio](https://www.visualstudio.com/) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="94a9f-152">In [Visual Studio Code](https://code.visualstudio.com/) können Umgebungsvariablen während der Entwicklung in der Datei *.vscode/launch.json* festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="94a9f-152">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="94a9f-153">Weitere Informationen finden Sie unter <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="94a9f-153">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="94a9f-154">`ConfigureHostConfiguration` kann mehrfach mit additiven Ergebnissen aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="94a9f-154">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="94a9f-155">Der Host verwendet die Option, die zuletzt einen Wert festlegt.</span><span class="sxs-lookup"><span data-stu-id="94a9f-155">The host uses whichever option sets a value last.</span></span>

<span data-ttu-id="94a9f-156">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="94a9f-156">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="94a9f-157">Beispielkonfiguration für `HostBuilder` mithilfe von `ConfigureHostConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="94a9f-157">Example `HostBuilder` configuration using `ConfigureHostConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

> [!NOTE]
> <span data-ttu-id="94a9f-158">Die Erweiterungsmethode [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) kann derzeit keine Konfigurationsabschnitte (z.B. `.AddConfiguration(Configuration.GetSection("section"))`) analysieren, die von [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="94a9f-158">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="94a9f-159">Die `GetSection`-Methode filtert die Konfigurationsschlüssel des angeforderten Abschnitts, behält jedoch den Abschnittsnamen in den Schlüsseln bei (z.B. `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="94a9f-159">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:environment`).</span></span> <span data-ttu-id="94a9f-160">Die `AddConfiguration`-Methode erwartet, dass die Schlüssel den `HostBuilder`-Schlüsseln entsprechen (z.B. `environment`).</span><span class="sxs-lookup"><span data-stu-id="94a9f-160">The `AddConfiguration` method expects the keys to match the `HostBuilder` keys (for example, `environment`).</span></span> <span data-ttu-id="94a9f-161">Das Vorhandensein des Abschnittsnamens in den Schlüsseln verhindert, dass die Werte des Abschnitts den Host konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="94a9f-161">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="94a9f-162">Dieses Problem wird in einer bevorstehenden Version behoben.</span><span class="sxs-lookup"><span data-stu-id="94a9f-162">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="94a9f-163">Weitere Informationen und Problemumgehungen finden Sie unter [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys (Beim Übergeben des Konfigurationsabschnitts an WebHostBuilder.UseConfiguration werden vollständige Schlüssel verwendet)](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="94a9f-163">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

### <a name="extension-method-configuration"></a><span data-ttu-id="94a9f-164">Konfiguration der Erweiterungsmethode</span><span class="sxs-lookup"><span data-stu-id="94a9f-164">Extension method configuration</span></span>

<span data-ttu-id="94a9f-165">Erweiterungsmethoden werden aus der Implementierung von `IHostBuilder` abgerufen, um das Inhaltsstammverzeichnis und die Umgebung zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="94a9f-165">Extension methods are called on the `IHostBuilder` implementation to configure the content root and the environment.</span></span>

#### <a name="application-key-name"></a><span data-ttu-id="94a9f-166">Anwendungsschlüssel (Name)</span><span class="sxs-lookup"><span data-stu-id="94a9f-166">Application Key (Name)</span></span>

<span data-ttu-id="94a9f-167">Die Eigenschaft [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) wird von der Hostkonfiguration während der Hostkonstruktion festgelegt.</span><span class="sxs-lookup"><span data-stu-id="94a9f-167">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is set from host configuration during host construction.</span></span> <span data-ttu-id="94a9f-168">Um den Wert explizit festzulegen, verwenden Sie den [HostDefaults.ApplicationKey](/dotnet/api/microsoft.extensions.hosting.hostdefaults.applicationkey):</span><span class="sxs-lookup"><span data-stu-id="94a9f-168">To set the value explicitly, use the [HostDefaults.ApplicationKey](/dotnet/api/microsoft.extensions.hosting.hostdefaults.applicationkey):</span></span>

<span data-ttu-id="94a9f-169">**Schlüssel:** Anwendungsname</span><span class="sxs-lookup"><span data-stu-id="94a9f-169">**Key**: applicationName</span></span>  
<span data-ttu-id="94a9f-170">**Typ:** *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="94a9f-170">**Type**: *string*</span></span>  
<span data-ttu-id="94a9f-171">**Standardwert:** Der Name der Assembly, die den Einstiegspunkt der App enthält.</span><span class="sxs-lookup"><span data-stu-id="94a9f-171">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="94a9f-172">**Festlegen mit:** `HostBuilderContext.HostingEnvironment.ApplicationName`</span><span class="sxs-lookup"><span data-stu-id="94a9f-172">**Set using**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span></span>  
<span data-ttu-id="94a9f-173">**Umgebungsvariable:** `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` ist [optional und benutzerdefiniert](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="94a9f-173">**Environment variable**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

```csharp
var host = new HostBuilder()
    .ConfigureAppConfiguration((hostContext, configApp) =>
    {
        hostContext.HostingEnvironment.ApplicationName = "CustomApplicationName";
    })
```

#### <a name="content-root"></a><span data-ttu-id="94a9f-174">Inhaltsstammverzeichnis</span><span class="sxs-lookup"><span data-stu-id="94a9f-174">Content Root</span></span>

<span data-ttu-id="94a9f-175">Diese Einstellung bestimmt, wo der Host mit der Suche nach Inhaltsdateien beginnt.</span><span class="sxs-lookup"><span data-stu-id="94a9f-175">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="94a9f-176">**Schlüssel:** contentRoot</span><span class="sxs-lookup"><span data-stu-id="94a9f-176">**Key**: contentRoot</span></span>  
<span data-ttu-id="94a9f-177">**Typ:** *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="94a9f-177">**Type**: *string*</span></span>  
<span data-ttu-id="94a9f-178">**Standard:** Entspricht standardmäßig dem Ordner, in dem die App-Assembly gespeichert ist.</span><span class="sxs-lookup"><span data-stu-id="94a9f-178">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="94a9f-179">**Festlegen mit:** `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="94a9f-179">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="94a9f-180">**Umgebungsvariable:** `<PREFIX_>CONTENTROOT` (`<PREFIX_>` ist [optional und benutzerdefiniert](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="94a9f-180">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="94a9f-181">Wenn der Pfad nicht vorhanden ist, kann der Host nicht gestartet werden.</span><span class="sxs-lookup"><span data-stu-id="94a9f-181">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

#### <a name="environment"></a><span data-ttu-id="94a9f-182">Umgebung</span><span class="sxs-lookup"><span data-stu-id="94a9f-182">Environment</span></span>

<span data-ttu-id="94a9f-183">Legt die [Umgebung](xref:fundamentals/environments) der App fest.</span><span class="sxs-lookup"><span data-stu-id="94a9f-183">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="94a9f-184">**Schlüssel:** environment</span><span class="sxs-lookup"><span data-stu-id="94a9f-184">**Key**: environment</span></span>  
<span data-ttu-id="94a9f-185">**Typ:** *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="94a9f-185">**Type**: *string*</span></span>  
<span data-ttu-id="94a9f-186">**Standard:** Produktion</span><span class="sxs-lookup"><span data-stu-id="94a9f-186">**Default**: Production</span></span>  
<span data-ttu-id="94a9f-187">**Festlegen mit:** `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="94a9f-187">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="94a9f-188">**Umgebungsvariable:** `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` ist [optional und benutzerdefiniert](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="94a9f-188">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="94a9f-189">Die Umgebung kann auf einen beliebigen Wert festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="94a9f-189">The environment can be set to any value.</span></span> <span data-ttu-id="94a9f-190">Zu den durch Frameworks definierten Werten zählen `Development`, `Staging` und `Production`.</span><span class="sxs-lookup"><span data-stu-id="94a9f-190">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="94a9f-191">Die Groß-/Kleinschreibung wird bei Werten nicht beachtet.</span><span class="sxs-lookup"><span data-stu-id="94a9f-191">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

## <a name="configureappconfiguration"></a><span data-ttu-id="94a9f-192">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="94a9f-192">ConfigureAppConfiguration</span></span>

<span data-ttu-id="94a9f-193">Die Konfiguration des App-Generators wird erstellt, indem [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) aus der Implementierung von [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) abgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="94a9f-193">App builder configuration is created by calling [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="94a9f-194">`ConfigureAppConfiguration` verwendet eine [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder)-Schnittstelle, um eine [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration)-Schnittstelle für die App zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="94a9f-194">`ConfigureAppConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the app.</span></span> <span data-ttu-id="94a9f-195">`ConfigureAppConfiguration` kann mehrfach mit additiven Ergebnissen aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="94a9f-195">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="94a9f-196">Die App verwendet die Option, die zuletzt einen Wert festlegt.</span><span class="sxs-lookup"><span data-stu-id="94a9f-196">The app uses whichever option sets a value last.</span></span> <span data-ttu-id="94a9f-197">Die von `ConfigureAppConfiguration` erstellte Konfiguration ist unter [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) verfügbar und kann für nachfolgende Vorgänge und in [Diensten](/dotnet/api/microsoft.extensions.hosting.ihost.services) verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="94a9f-197">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) for subsequent operations and in [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span></span>

<span data-ttu-id="94a9f-198">Beispielkonfiguration für die App mithilfe von `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="94a9f-198">Example app configuration using `ConfigureAppConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="94a9f-199">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="94a9f-199">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="94a9f-200">*appsettings.Development.json*:</span><span class="sxs-lookup"><span data-stu-id="94a9f-200">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="94a9f-201">*appsettings.Production.json*:</span><span class="sxs-lookup"><span data-stu-id="94a9f-201">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

> [!NOTE]
> <span data-ttu-id="94a9f-202">Die Erweiterungsmethode [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) kann derzeit keine Konfigurationsabschnitte (z.B. `.AddConfiguration(Configuration.GetSection("section"))`) analysieren, die von [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="94a9f-202">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="94a9f-203">Die `GetSection`-Methode filtert die Konfigurationsschlüssel des angeforderten Abschnitts, behält jedoch den Abschnittsnamen in den Schlüsseln bei (z.B. `section:Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="94a9f-203">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:Logging:LogLevel:Default`).</span></span> <span data-ttu-id="94a9f-204">Die `AddConfiguration`-Methode erwartet eine genaue Übereinstimmung mit den Konfigurationsschlüsseln (z.B. `Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="94a9f-204">The `AddConfiguration` method expects an exact match to configuration keys (for example, `Logging:LogLevel:Default`).</span></span> <span data-ttu-id="94a9f-205">Das Vorhandensein des Abschnittsnamens in den Schlüsseln verhindert, dass die Werte des Abschnitts die App konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="94a9f-205">The presence of the section name on the keys prevents the section's values from configuring the app.</span></span> <span data-ttu-id="94a9f-206">Dieses Problem wird in einer bevorstehenden Version behoben.</span><span class="sxs-lookup"><span data-stu-id="94a9f-206">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="94a9f-207">Weitere Informationen und Problemumgehungen finden Sie unter [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys (Beim Übergeben des Konfigurationsabschnitts an WebHostBuilder.UseConfiguration werden vollständige Schlüssel verwendet)](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="94a9f-207">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="94a9f-208">Um Einstellungsdateien in das Ausgabeverzeichnis zu verschieben, geben Sie die Einstellungsdateien als [MSBuild-Projektelemente](/visualstudio/msbuild/common-msbuild-project-items) in der Projektdatei an.</span><span class="sxs-lookup"><span data-stu-id="94a9f-208">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="94a9f-209">Die Beispiel-App verschiebt ihre JSON-App-Einstellungsdateien und *hostsettings.json* mit dem folgenden **&lt;Content&gt;**-Element:</span><span class="sxs-lookup"><span data-stu-id="94a9f-209">The sample app moves its JSON app settings files and *hostsettings.json* with the following **&lt;Content:&gt;** item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

## <a name="configureservices"></a><span data-ttu-id="94a9f-210">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="94a9f-210">ConfigureServices</span></span>

<span data-ttu-id="94a9f-211">Mithilfe von [ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) werden Dienste zum [Dependency Injection](xref:fundamentals/dependency-injection)-Container der App hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="94a9f-211">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="94a9f-212">`ConfigureServices` kann mehrfach mit additiven Ergebnissen aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="94a9f-212">`ConfigureServices` can be called multiple times with additive results.</span></span>

<span data-ttu-id="94a9f-213">Ein gehosteter Dienst ist eine Klasse mit Logik für Hintergrundaufgaben, die die Schnittstelle [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) implementiert.</span><span class="sxs-lookup"><span data-stu-id="94a9f-213">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="94a9f-214">Weitere Informationen finden Sie unter <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="94a9f-214">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

<span data-ttu-id="94a9f-215">Die [Beispiel-App](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) verwendet die Erweiterungsmethode `AddHostedService`, um einen Dienst für Lebensdauerereignisse (`LifetimeEventsHostedService`) und einen zeitgesteuerten Hintergrundtask (`TimedHostedService`) zur App hinzuzufügen:</span><span class="sxs-lookup"><span data-stu-id="94a9f-215">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="94a9f-216">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="94a9f-216">ConfigureLogging</span></span>

<span data-ttu-id="94a9f-217">Durch [ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) wird ein Delegat für die Konfiguration der bereitgestellten [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder)-Schnittstelle bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="94a9f-217">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) adds a delegate for configuring the provided [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder).</span></span> <span data-ttu-id="94a9f-218">`ConfigureLogging` kann mehrfach mit additiven Ergebnissen aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="94a9f-218">`ConfigureLogging` may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="94a9f-219">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="94a9f-219">UseConsoleLifetime</span></span>

<span data-ttu-id="94a9f-220">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) wartet auf `Ctrl+C`/SIGINT oder SIGTERM und ruft [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) auf, um das Herunterfahren zu beginnen.</span><span class="sxs-lookup"><span data-stu-id="94a9f-220">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) listens for `Ctrl+C`/SIGINT or SIGTERM and calls [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) to start the shutdown process.</span></span> <span data-ttu-id="94a9f-221">`UseConsoleLifetime` hebt die Blockierung von Erweiterungen wie [RunAsync](#runasync) und [WaitForShutdownAsync](#waitforshutdownasync) auf.</span><span class="sxs-lookup"><span data-stu-id="94a9f-221">`UseConsoleLifetime` unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="94a9f-222">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) ist vorab als Standardimplementierung für die Lebensdauer registriert.</span><span class="sxs-lookup"><span data-stu-id="94a9f-222">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="94a9f-223">Die letzte registrierte Lebensdauer wird verwendet.</span><span class="sxs-lookup"><span data-stu-id="94a9f-223">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="94a9f-224">Containerkonfiguration</span><span class="sxs-lookup"><span data-stu-id="94a9f-224">Container configuration</span></span>

<span data-ttu-id="94a9f-225">Der Host kann eine [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1)-Schnittstelle verwenden, um die Integration in andere Container zu unterstützten.</span><span class="sxs-lookup"><span data-stu-id="94a9f-225">To support plugging in other containers, the host can accept an [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span></span> <span data-ttu-id="94a9f-226">Das Bereitstellen einer Factory ist kein Teil der DI-Containerregistrierung, sondern ein Host, der systemintern verwendet wird, um den entsprechenden DI-Container zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="94a9f-226">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="94a9f-227">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) überschreibt die Standardfactory, die zum Erstellen des Dienstanbieters der App verwendet wurde.</span><span class="sxs-lookup"><span data-stu-id="94a9f-227">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="94a9f-228">Die benutzerdefinierte Containerkonfiguration wird von der Methode [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) verwaltet.</span><span class="sxs-lookup"><span data-stu-id="94a9f-228">Custom container configuration is managed by the [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) method.</span></span> <span data-ttu-id="94a9f-229">`ConfigureContainer` stellt eine stark typisierte Möglichkeit für das Konfigurieren des Containers auf Basis der zugrunde liegenden Host-API bereit.</span><span class="sxs-lookup"><span data-stu-id="94a9f-229">`ConfigureContainer` provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="94a9f-230">`ConfigureContainer` kann mehrfach mit additiven Ergebnissen aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="94a9f-230">`ConfigureContainer` can be called multiple times with additive results.</span></span>

<span data-ttu-id="94a9f-231">Erstellen eines Dienstcontainers für die App:</span><span class="sxs-lookup"><span data-stu-id="94a9f-231">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="94a9f-232">Bereitstellen einer Factory für den Dienstcontainer:</span><span class="sxs-lookup"><span data-stu-id="94a9f-232">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="94a9f-233">Verwenden Sie die Factory, und konfigurieren Sie den benutzerdefinierten Dienstcontainer für die App:</span><span class="sxs-lookup"><span data-stu-id="94a9f-233">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="94a9f-234">Erweiterungen</span><span class="sxs-lookup"><span data-stu-id="94a9f-234">Extensibility</span></span>

<span data-ttu-id="94a9f-235">Die Erweiterung des Hosts wird mit der Erweiterungsmethode in `IHostBuilder` durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="94a9f-235">Host extensibility is performed with extension methods on `IHostBuilder`.</span></span> <span data-ttu-id="94a9f-236">Im folgenden Beispiel wird dargestellt, wie eine Erweiterungsmethode eine `IHostBuilder`-Implementierung mit dem [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks)-Beispiel erweitert, das in <xref:fundamentals/host/hosted-services> gezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="94a9f-236">The following example shows how an extension method extends an `IHostBuilder` implementation with the [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) example demonstrated in <xref:fundamentals/host/hosted-services>.</span></span>

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

<span data-ttu-id="94a9f-237">Eine App richtet die `UseHostedService`-Erweiterungsmethode zum Registrieren des in `T` übergebenen gehosteten Diensts ein:</span><span class="sxs-lookup"><span data-stu-id="94a9f-237">An app establishes the `UseHostedService` extension method to register the hosted service passed in `T`:</span></span>

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

## <a name="manage-the-host"></a><span data-ttu-id="94a9f-238">Verwalten des Hosts</span><span class="sxs-lookup"><span data-stu-id="94a9f-238">Manage the host</span></span>

<span data-ttu-id="94a9f-239">Die Implementierung von [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) ist für das Starten und Beenden der Implementierungen von `IHostedService` verantwortlich, die im Dienstcontainer registriert sind.</span><span class="sxs-lookup"><span data-stu-id="94a9f-239">The [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) implementation is responsible for starting and stopping the `IHostedService` implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="94a9f-240">Run</span><span class="sxs-lookup"><span data-stu-id="94a9f-240">Run</span></span>

<span data-ttu-id="94a9f-241">Durch [Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) wird die App gestartet und der aufrufende Thread blockiert, bis der Host heruntergefahren wird:</span><span class="sxs-lookup"><span data-stu-id="94a9f-241">[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="94a9f-242">RunAsync</span><span class="sxs-lookup"><span data-stu-id="94a9f-242">RunAsync</span></span>

<span data-ttu-id="94a9f-243">Durch [RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) wird die App ausgeführt und eine `Task`-Methode ausgegeben, die abgeschlossen wird, wenn das Abbruchtoken oder das Herunterfahren ausgelöst wird:</span><span class="sxs-lookup"><span data-stu-id="94a9f-243">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) runs the app and returns a `Task` that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="94a9f-244">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="94a9f-244">RunConsoleAsync</span></span>

<span data-ttu-id="94a9f-245">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) aktiviert die Unterstützung der Konsole, erstellt und startet den Host und wartet auf `Ctrl+C`/SIGINT oder SIGTERM, um das Herunterfahren auszulösen.</span><span class="sxs-lookup"><span data-stu-id="94a9f-245">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) enables console support, builds and starts the host, and waits for `Ctrl+C`/SIGINT or SIGTERM to shut down.</span></span>

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

### <a name="start-and-stopasync"></a><span data-ttu-id="94a9f-246">Start und StopAsync</span><span class="sxs-lookup"><span data-stu-id="94a9f-246">Start and StopAsync</span></span>

<span data-ttu-id="94a9f-247">[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) startet den Host synchron.</span><span class="sxs-lookup"><span data-stu-id="94a9f-247">[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) starts the host synchronously.</span></span>

<span data-ttu-id="94a9f-248">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) versucht, den Host innerhalb des angegebenen Timeouts zu beenden.</span><span class="sxs-lookup"><span data-stu-id="94a9f-248">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="94a9f-249">StartAsync und StopAsync</span><span class="sxs-lookup"><span data-stu-id="94a9f-249">StartAsync and StopAsync</span></span>

<span data-ttu-id="94a9f-250">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) startet die App.</span><span class="sxs-lookup"><span data-stu-id="94a9f-250">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) starts the app.</span></span>

<span data-ttu-id="94a9f-251">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) stoppt die App.</span><span class="sxs-lookup"><span data-stu-id="94a9f-251">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="94a9f-252">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="94a9f-252">WaitForShutdown</span></span>

<span data-ttu-id="94a9f-253">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) wird über die [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime)-Schnittstelle ausgelöst (z.B. die [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime)-Klasse, die auf `Ctrl+C`/SIGINT oder SIGTERM wartet).</span><span class="sxs-lookup"><span data-stu-id="94a9f-253">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) is triggered via the [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), such as [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (listens for `Ctrl+C`/SIGINT or SIGTERM).</span></span> <span data-ttu-id="94a9f-254">`WaitForShutdown` ruft [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) auf.</span><span class="sxs-lookup"><span data-stu-id="94a9f-254">`WaitForShutdown` calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="94a9f-255">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="94a9f-255">WaitForShutdownAsync</span></span>

<span data-ttu-id="94a9f-256">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) gibt eine `Task`-Methode zurück, die abgeschlossen wird, wenn das Herunterfahren über das angegebene Token ausgelöst wird, und ruft [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) auf.</span><span class="sxs-lookup"><span data-stu-id="94a9f-256">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) returns a `Task` that completes when shutdown is triggered via the given token and calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="external-control"></a><span data-ttu-id="94a9f-257">Externe Steuerung</span><span class="sxs-lookup"><span data-stu-id="94a9f-257">External control</span></span>

<span data-ttu-id="94a9f-258">Die externe Steuerung des Hosts kann mithilfe von Methoden durchgeführt werden, die extern aufgerufen werden können:</span><span class="sxs-lookup"><span data-stu-id="94a9f-258">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="94a9f-259">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) wird am Anfang der [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync)-Methode aufgerufen, die den Vorgang erst fortsetzt, wenn sie abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="94a9f-259">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) is called at the start of [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), which waits until it's complete before continuing.</span></span> <span data-ttu-id="94a9f-260">Dies kann verwendet werden, um den Start zu verzögern, bis dieser durch ein externes Ereignis initiiert wird.</span><span class="sxs-lookup"><span data-stu-id="94a9f-260">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="94a9f-261">IHostingEnvironment-Schnittstelle</span><span class="sxs-lookup"><span data-stu-id="94a9f-261">IHostingEnvironment interface</span></span>

<span data-ttu-id="94a9f-262">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) stellt Informationen über die Hostingumgebung der App bereit.</span><span class="sxs-lookup"><span data-stu-id="94a9f-262">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) provides information about the app's hosting environment.</span></span> <span data-ttu-id="94a9f-263">Verwenden Sie [Constructor Injection](xref:fundamentals/dependency-injection) zum Abrufen der `IHostingEnvironment`-Schnittstelle, um deren Eigenschaften und Erweiterungsmethoden zu verwenden:</span><span class="sxs-lookup"><span data-stu-id="94a9f-263">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="94a9f-264">Weitere Informationen finden Sie unter <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="94a9f-264">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="94a9f-265">IApplicationLifetime-Schnittstelle</span><span class="sxs-lookup"><span data-stu-id="94a9f-265">IApplicationLifetime interface</span></span>

<span data-ttu-id="94a9f-266">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) ermöglicht Aktivitäten nach dem Starten und beim Herunterfahren (einschließlich Anforderungen für ordnungsgemäßes Herunterfahren).</span><span class="sxs-lookup"><span data-stu-id="94a9f-266">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="94a9f-267">Bei drei der Eigenschaften der Schnittstelle handelt es sich um Abbruchtokens, die zum Registrieren von `Action`-Methoden verwendet werden, durch die Ereignisse beim Starten und Herunterfahren definiert werden.</span><span class="sxs-lookup"><span data-stu-id="94a9f-267">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="94a9f-268">Abbruchtoken</span><span class="sxs-lookup"><span data-stu-id="94a9f-268">Cancellation Token</span></span> | <span data-ttu-id="94a9f-269">Wird ausgelöst, wenn&#8230;</span><span class="sxs-lookup"><span data-stu-id="94a9f-269">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| [<span data-ttu-id="94a9f-270">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="94a9f-270">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="94a9f-271">Der Host vollständig gestartet wurde.</span><span class="sxs-lookup"><span data-stu-id="94a9f-271">The host has fully started.</span></span> |
| [<span data-ttu-id="94a9f-272">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="94a9f-272">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="94a9f-273">Der Host ein ordnungsgemäßes Herunterfahren abschließt.</span><span class="sxs-lookup"><span data-stu-id="94a9f-273">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="94a9f-274">Alle Anforderungen sollten verarbeitet worden sein.</span><span class="sxs-lookup"><span data-stu-id="94a9f-274">All requests should be processed.</span></span> <span data-ttu-id="94a9f-275">Das Herunterfahren wird blockiert, bis das Ereignis abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="94a9f-275">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="94a9f-276">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="94a9f-276">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="94a9f-277">Der Host ein ordnungsgemäßes Herunterfahren ausführt.</span><span class="sxs-lookup"><span data-stu-id="94a9f-277">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="94a9f-278">Anforderungen möglicherweise noch verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="94a9f-278">Requests may still be processing.</span></span> <span data-ttu-id="94a9f-279">Das Herunterfahren wird blockiert, bis das Ereignis abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="94a9f-279">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="94a9f-280">Fügen Sie den `IApplicationLifetime`-Dienst über einen Konstruktor in eine beliebige Klasse ein.</span><span class="sxs-lookup"><span data-stu-id="94a9f-280">Constructor-inject the `IApplicationLifetime` service into any class.</span></span> <span data-ttu-id="94a9f-281">Die [Beispiel-App](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) verwendet die Konstruktorinjektion für eine `LifetimeEventsHostedService`-Klasse (eine `IHostedService`-Implementierung), um die Ereignisse zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="94a9f-281">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an `IHostedService` implementation) to register the events.</span></span>

<span data-ttu-id="94a9f-282">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="94a9f-282">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="94a9f-283">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) fordert das Beenden der Anwendung an.</span><span class="sxs-lookup"><span data-stu-id="94a9f-283">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="94a9f-284">Die folgende Klasse verwendet `StopApplication`, um eine App ordnungsgemäß herunterzufahren, wenn die `Shutdown`-Methode der Klasse aufgerufen wird:</span><span class="sxs-lookup"><span data-stu-id="94a9f-284">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="94a9f-285">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="94a9f-285">Additional resources</span></span>

* <xref:fundamentals/host/hosted-services>
* [<span data-ttu-id="94a9f-286">Hosten von Beispielrepositorys auf GitHub</span><span class="sxs-lookup"><span data-stu-id="94a9f-286">Hosting repo samples on GitHub</span></span>](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
