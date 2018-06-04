---
title: Generischer .NET-Host
author: guardrex
description: Erfahren Sie mehr über den generischen Host in .NET, der für das Starten der App und das Verwalten der Lebensdauer verantwortlich ist.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/generic-host
ms.openlocfilehash: 15ce81a4226921ce053096751d7678ada36235c0
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34728972"
---
# <a name="net-generic-host"></a><span data-ttu-id="7f7cd-103">Generischer .NET-Host</span><span class="sxs-lookup"><span data-stu-id="7f7cd-103">.NET Generic Host</span></span>

<span data-ttu-id="7f7cd-104">Von [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="7f7cd-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="7f7cd-105">.NET-Apps konfigurieren und starten einen *Host*.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-105">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="7f7cd-106">Der Host ist verantwortlich für das Starten der App und das Verwalten der Lebensdauer.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="7f7cd-107">In diesem Artikel wird der generische ASP.NET Core-Host ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)) erläutert, der für das Hosten von Apps nützlich ist, die keine HTTP-Anforderungen verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-107">This topic covers the ASP.NET Core Generic Host ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), which is useful for hosting apps that don't process HTTP requests.</span></span> <span data-ttu-id="7f7cd-108">Weitere Informationen zum Webhost ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)) finden Sie im Artikel [Webhost](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="7f7cd-108">For coverage of the Web Host ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>

<span data-ttu-id="7f7cd-109">Das Ziel des generischen Hosts besteht darin, die HTTP-Pipeline von der Webhost-API zu entkoppeln, um mehr Szenarios zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-109">The goal of the Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="7f7cd-110">Messaging, Hintergrundtasks und andere Nicht-HTTP-Workloads, die auf dem generischen Host basieren, profitieren von übergreifenden Funktionen wie der Konfiguration, Dependency Injection (DI) und der Protokollerstellung.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-110">Messaging, background tasks, and other non-HTTP workloads based on the Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="7f7cd-111">Der generische Host ist neu in ASP.NET Core 2.1 und ist nicht für Webhostingszenarios geeignet.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-111">The Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="7f7cd-112">Verwenden Sie den [Webhost](xref:fundamentals/host/web-host) für Webhostingszenarios.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-112">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="7f7cd-113">Der generische Host wird dafür entwickelt, den Webhost in einem zukünftigen Release zu ersetzen und als primäre Host-API in HTTP- und Nicht-HTTP-Szenarios zu fungieren.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-113">The Generic Host is under development to replace the Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="7f7cd-114">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7f7cd-114">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="7f7cd-115">Wenn Sie die Beispiel-App in [Visual Studio Code](https://code.visualstudio.com/) ausführen, verwenden Sie ein *externes oder integriertes Terminal*.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-115">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="7f7cd-116">Führen Sie das Beispiel nicht in `internalConsole` aus.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-116">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="7f7cd-117">So legen Sie die Konsole in Visual Studio Code fest:</span><span class="sxs-lookup"><span data-stu-id="7f7cd-117">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="7f7cd-118">Öffnen Sie die Datei *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-118">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="7f7cd-119">Suchen Sie in der Konfiguration **.NET Core-Start (Konsole)** den Eintrag **Konsole**.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-119">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="7f7cd-120">Legen Sie den Wert auf `externalTerminal` oder `integratedTerminal` fest.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-120">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="7f7cd-121">Einführung</span><span class="sxs-lookup"><span data-stu-id="7f7cd-121">Introduction</span></span>

<span data-ttu-id="7f7cd-122">Die generische Hostbibliothek ist im Namespace [Microsoft.Extensions.Hosting](/dotnet/api/microsoft.extensions.hosting) verfügbar und wird vom NuGet-Paket [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-122">The Generic Host library is available in the [Microsoft.Extensions.Hosting namespace](/dotnet/api/microsoft.extensions.hosting) and provided by the [Microsoft.Extensions.Hosting NuGet package](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/).</span></span> <span data-ttu-id="7f7cd-123">Das Paket `Microsoft.Extensions.Hosting` ist im Metapaket [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) enthalten.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-123">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) metapackage.</span></span>

<span data-ttu-id="7f7cd-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) ist der Einstiegspunkt für die Ausführung des Codes.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) is the entry point to code execution.</span></span> <span data-ttu-id="7f7cd-125">Jede Implementierung von `IHostedService` wird in der Reihenfolge der [Dienstregistrierung in ConfigureServices](#configureservices) ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-125">Each `IHostedService` implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="7f7cd-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) wird in jeder `IHostedService`-Schnittstelle aufgerufen, wenn der Host gestartet wird, und [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) wird in umgekehrter Registrierungsreihenfolge aufgerufen, wenn der Host ordnungsgemäß heruntergefahren wird.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) is called on each `IHostedService` when the host starts, and [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="7f7cd-127">Einrichten eines Hosts</span><span class="sxs-lookup"><span data-stu-id="7f7cd-127">Set up a host</span></span>

<span data-ttu-id="7f7cd-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) ist die Hauptkomponente, die Bibliotheken und Apps für die Initialisierung, Erstellung und Ausführung des Hosts verwenden:</span><span class="sxs-lookup"><span data-stu-id="7f7cd-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="host-configuration"></a><span data-ttu-id="7f7cd-129">Konfiguration des Hosts</span><span class="sxs-lookup"><span data-stu-id="7f7cd-129">Host configuration</span></span>

<span data-ttu-id="7f7cd-130">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) basiert auf folgenden Ansätzen, um die Hostkonfigurationswerte festzulegen:</span><span class="sxs-lookup"><span data-stu-id="7f7cd-130">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="7f7cd-131">Konfigurations-Generator</span><span class="sxs-lookup"><span data-stu-id="7f7cd-131">Configuration builder</span></span>
* <span data-ttu-id="7f7cd-132">Konfiguration der Erweiterungsmethode</span><span class="sxs-lookup"><span data-stu-id="7f7cd-132">Extension method configuration</span></span>

### <a name="configuration-builder"></a><span data-ttu-id="7f7cd-133">Konfigurations-Generator</span><span class="sxs-lookup"><span data-stu-id="7f7cd-133">Configuration builder</span></span>

<span data-ttu-id="7f7cd-134">Die Konfiguration des Host-Generators wird erstellt, indem [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) aus der Implementierung von [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) abgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-134">Host builder configuration is created by calling [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="7f7cd-135">`ConfigureHostConfiguration` verwendet eine [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder)-Schnittstelle, um eine [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration)-Schnittstelle für den Host zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-135">`ConfigureHostConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the host.</span></span> <span data-ttu-id="7f7cd-136">Der Konfigurations-Generator initialisiert die [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment)-Schnittstelle, um diese im Buildprozess der App zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-136">The configuration builder initializes the [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) for use in the app's build process.</span></span> <span data-ttu-id="7f7cd-137">`ConfigureHostConfiguration` kann mehrfach mit additiven Ergebnissen aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-137">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="7f7cd-138">Der Host verwendet die Option, die zuletzt einen Wert festlegt.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-138">The host uses whichever option sets a value last.</span></span>

<span data-ttu-id="7f7cd-139">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="7f7cd-139">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="7f7cd-140">Beispielkonfiguration für `HostBuilder` mithilfe von `ConfigureHostConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="7f7cd-140">Example `HostBuilder` configuration using `ConfigureHostConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

> [!NOTE]
> <span data-ttu-id="7f7cd-141">Die Erweiterungsmethode [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) kann derzeit keine Konfigurationsabschnitte (z.B. `.AddConfiguration(Configuration.GetSection("section"))`) analysieren, die von [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-141">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="7f7cd-142">Die `GetSection`-Methode filtert die Konfigurationsschlüssel des angeforderten Abschnitts, behält jedoch den Abschnittsnamen in den Schlüsseln bei (z.B. `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="7f7cd-142">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:environment`).</span></span> <span data-ttu-id="7f7cd-143">Die `AddConfiguration`-Methode erwartet, dass die Schlüssel den `HostBuilder`-Schlüsseln entsprechen (z.B. `environment`).</span><span class="sxs-lookup"><span data-stu-id="7f7cd-143">The `AddConfiguration` method expects the keys to match the `HostBuilder` keys (for example, `environment`).</span></span> <span data-ttu-id="7f7cd-144">Das Vorhandensein des Abschnittsnamens in den Schlüsseln verhindert, dass die Werte des Abschnitts den Host konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-144">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="7f7cd-145">Dieses Problem wird in einer bevorstehenden Version behoben.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-145">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="7f7cd-146">Weitere Informationen und Problemumgehungen finden Sie unter [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys (Beim Übergeben des Konfigurationsabschnitts an WebHostBuilder.UseConfiguration werden vollständige Schlüssel verwendet)](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="7f7cd-146">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

### <a name="extension-method-configuration"></a><span data-ttu-id="7f7cd-147">Konfiguration der Erweiterungsmethode</span><span class="sxs-lookup"><span data-stu-id="7f7cd-147">Extension method configuration</span></span>

<span data-ttu-id="7f7cd-148">Erweiterungsmethoden werden aus der Implementierung von `IHostBuilder` abgerufen, um das Inhaltsstammverzeichnis und die Umgebung zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-148">Extension methods are called on the `IHostBuilder` implementation to configure the content root and the environment.</span></span>

#### <a name="content-root"></a><span data-ttu-id="7f7cd-149">Inhaltsstammverzeichnis</span><span class="sxs-lookup"><span data-stu-id="7f7cd-149">Content Root</span></span>

<span data-ttu-id="7f7cd-150">Diese Einstellung bestimmt, wo der Host mit der Suche nach Inhaltsdateien beginnt.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-150">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="7f7cd-151">**Schlüssel:** contentRoot</span><span class="sxs-lookup"><span data-stu-id="7f7cd-151">**Key**: contentRoot</span></span>  
<span data-ttu-id="7f7cd-152">**Typ:** *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="7f7cd-152">**Type**: *string*</span></span>  
<span data-ttu-id="7f7cd-153">**Standard:** Entspricht standardmäßig dem Ordner, in dem die App-Assembly gespeichert ist.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-153">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="7f7cd-154">**Festlegen mit:** `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="7f7cd-154">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="7f7cd-155">**Umgebungsvariable:** `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="7f7cd-155">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="7f7cd-156">Wenn der Pfad nicht vorhanden ist, kann der Host nicht gestartet werden.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-156">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

#### <a name="environment"></a><span data-ttu-id="7f7cd-157">Umgebung</span><span class="sxs-lookup"><span data-stu-id="7f7cd-157">Environment</span></span>

<span data-ttu-id="7f7cd-158">Legt die [Umgebung](xref:fundamentals/environments) der App fest.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-158">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="7f7cd-159">**Schlüssel:** environment</span><span class="sxs-lookup"><span data-stu-id="7f7cd-159">**Key**: environment</span></span>  
<span data-ttu-id="7f7cd-160">**Typ:** *Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="7f7cd-160">**Type**: *string*</span></span>  
<span data-ttu-id="7f7cd-161">**Standard:** Produktion</span><span class="sxs-lookup"><span data-stu-id="7f7cd-161">**Default**: Production</span></span>  
<span data-ttu-id="7f7cd-162">**Festlegen mit:** `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="7f7cd-162">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="7f7cd-163">**Umgebungsvariable:** `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="7f7cd-163">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="7f7cd-164">Die Umgebung kann auf einen beliebigen Wert festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-164">The environment can be set to any value.</span></span> <span data-ttu-id="7f7cd-165">Zu den durch Frameworks definierten Werten zählen `Development`, `Staging` und `Production`.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-165">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="7f7cd-166">Die Groß-/Kleinschreibung wird bei Werten nicht beachtet.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-166">Values aren't case sensitive.</span></span> <span data-ttu-id="7f7cd-167">Standardmäßig wird die *Umgebung* aus der `ASPNETCORE_ENVIRONMENT`-Umgebungsvariablen gelesen.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-167">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="7f7cd-168">Wenn Sie [Visual Studio](https://www.visualstudio.com/) verwenden, werden Umgebungsvariablen möglicherweise in der Datei *launchSettings.json* festgelegt.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-168">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="7f7cd-169">Weitere Informationen finden Sie unter [Verwenden mehrerer Umgebungen](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="7f7cd-169">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

## <a name="configureappconfiguration"></a><span data-ttu-id="7f7cd-170">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="7f7cd-170">ConfigureAppConfiguration</span></span>

<span data-ttu-id="7f7cd-171">Die Konfiguration des App-Generators wird erstellt, indem [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) aus der Implementierung von [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) abgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-171">App builder configuration is created by calling [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="7f7cd-172">`ConfigureAppConfiguration` verwendet eine [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder)-Schnittstelle, um eine [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration)-Schnittstelle für die App zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-172">`ConfigureAppConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the app.</span></span> <span data-ttu-id="7f7cd-173">`ConfigureAppConfiguration` kann mehrfach mit additiven Ergebnissen aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-173">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="7f7cd-174">Die App verwendet die Option, die zuletzt einen Wert festlegt.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-174">The app uses whichever option sets a value last.</span></span> <span data-ttu-id="7f7cd-175">Die von `ConfigureAppConfiguration` erstellte Konfiguration ist unter [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) verfügbar und kann für nachfolgende Vorgänge und in [Diensten](/dotnet/api/microsoft.extensions.hosting.ihost.services) verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-175">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) for subsequent operations and in [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span></span>

<span data-ttu-id="7f7cd-176">Beispielkonfiguration für die App mithilfe von `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="7f7cd-176">Example app configuration using `ConfigureAppConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="7f7cd-177">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="7f7cd-177">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="7f7cd-178">*appsettings.Development.json*:</span><span class="sxs-lookup"><span data-stu-id="7f7cd-178">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="7f7cd-179">*appsettings.Production.json*:</span><span class="sxs-lookup"><span data-stu-id="7f7cd-179">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

> [!NOTE]
> <span data-ttu-id="7f7cd-180">Die Erweiterungsmethode [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) kann derzeit keine Konfigurationsabschnitte (z.B. `.AddConfiguration(Configuration.GetSection("section"))`) analysieren, die von [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-180">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="7f7cd-181">Die `GetSection`-Methode filtert die Konfigurationsschlüssel des angeforderten Abschnitts, behält jedoch den Abschnittsnamen in den Schlüsseln bei (z.B. `section:Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="7f7cd-181">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:Logging:LogLevel:Default`).</span></span> <span data-ttu-id="7f7cd-182">Die `AddConfiguration`-Methode erwartet eine genaue Übereinstimmung mit den Konfigurationsschlüsseln (z.B. `Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="7f7cd-182">The `AddConfiguration` method expects an exact match to configuration keys (for example, `Logging:LogLevel:Default`).</span></span> <span data-ttu-id="7f7cd-183">Das Vorhandensein des Abschnittsnamens in den Schlüsseln verhindert, dass die Werte des Abschnitts die App konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-183">The presence of the section name on the keys prevents the section's values from configuring the app.</span></span> <span data-ttu-id="7f7cd-184">Dieses Problem wird in einer bevorstehenden Version behoben.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-184">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="7f7cd-185">Weitere Informationen und Problemumgehungen finden Sie unter [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys (Beim Übergeben des Konfigurationsabschnitts an WebHostBuilder.UseConfiguration werden vollständige Schlüssel verwendet)](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="7f7cd-185">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

## <a name="configureservices"></a><span data-ttu-id="7f7cd-186">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="7f7cd-186">ConfigureServices</span></span>

<span data-ttu-id="7f7cd-187">Mithilfe von [ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) werden Dienste zum [Dependency Injection](xref:fundamentals/dependency-injection)-Container der App hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-187">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="7f7cd-188">`ConfigureServices` kann mehrfach mit additiven Ergebnissen aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-188">`ConfigureServices` can be called multiple times with additive results.</span></span>

<span data-ttu-id="7f7cd-189">Ein gehosteter Dienst ist eine Klasse mit Logik für Hintergrundaufgaben, die die Schnittstelle [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) implementiert.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-189">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="7f7cd-190">Weitere Informationen finden Sie im Artikel [Hintergrundtasks mit gehosteten Diensten](xref:fundamentals/host/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="7f7cd-190">For more information, see the [Background tasks with hosted services](xref:fundamentals/host/hosted-services) topic.</span></span>

<span data-ttu-id="7f7cd-191">Die [Beispiel-App](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) verwendet die Erweiterungsmethode `AddHostedService`, um einen Dienst für Lebensdauerereignisse (`LifetimeEventsHostedService`) und einen zeitgesteuerten Hintergrundtask (`TimedHostedService`) zur App hinzuzufügen:</span><span class="sxs-lookup"><span data-stu-id="7f7cd-191">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="7f7cd-192">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="7f7cd-192">ConfigureLogging</span></span>

<span data-ttu-id="7f7cd-193">Durch [ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) wird ein Delegat für die Konfiguration der bereitgestellten [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder)-Schnittstelle bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-193">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) adds a delegate for configuring the provided [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder).</span></span> <span data-ttu-id="7f7cd-194">`ConfigureLogging` kann mehrfach mit additiven Ergebnissen aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-194">`ConfigureLogging` may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="7f7cd-195">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="7f7cd-195">UseConsoleLifetime</span></span>

<span data-ttu-id="7f7cd-196">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) wartet auf `Ctrl+C`/SIGINT oder SIGTERM und ruft [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) auf, um das Herunterfahren zu beginnen.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-196">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) listens for `Ctrl+C`/SIGINT or SIGTERM and calls [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) to start the shutdown process.</span></span> <span data-ttu-id="7f7cd-197">`UseConsoleLifetime` hebt die Blockierung von Erweiterungen wie [RunAsync](#runasync) und [WaitForShutdownAsync](#waitforshutdownasync) auf.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-197">`UseConsoleLifetime` unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="7f7cd-198">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) ist vorab als Standardimplementierung für die Lebensdauer registriert.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-198">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="7f7cd-199">Die letzte registrierte Lebensdauer wird verwendet.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-199">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="7f7cd-200">Containerkonfiguration</span><span class="sxs-lookup"><span data-stu-id="7f7cd-200">Container configuration</span></span>

<span data-ttu-id="7f7cd-201">Der Host kann eine [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1)-Schnittstelle verwenden, um die Integration in andere Container zu unterstützten.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-201">To support plugging in other containers, the host can accept an [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span></span> <span data-ttu-id="7f7cd-202">Das Bereitstellen einer Factory ist kein Teil der DI-Containerregistrierung, sondern ein Host, der systemintern verwendet wird, um den entsprechenden DI-Container zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-202">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="7f7cd-203">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) überschreibt die Standardfactory, die zum Erstellen des Dienstanbieters der App verwendet wurde.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-203">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="7f7cd-204">Die benutzerdefinierte Containerkonfiguration wird von der Methode [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) verwaltet.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-204">Custom container configuration is managed by the [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) method.</span></span> <span data-ttu-id="7f7cd-205">`ConfigureContainer` stellt eine stark typisierte Möglichkeit für das Konfigurieren des Containers auf Basis der zugrunde liegenden Host-API bereit.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-205">`ConfigureContainer` provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="7f7cd-206">`ConfigureContainer` kann mehrfach mit additiven Ergebnissen aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-206">`ConfigureContainer` can be called multiple times with additive results.</span></span>

<span data-ttu-id="7f7cd-207">Erstellen eines Dienstcontainers für die App:</span><span class="sxs-lookup"><span data-stu-id="7f7cd-207">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="7f7cd-208">Bereitstellen einer Factory für den Dienstcontainer:</span><span class="sxs-lookup"><span data-stu-id="7f7cd-208">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="7f7cd-209">Verwenden Sie die Factory, und konfigurieren Sie den benutzerdefinierten Dienstcontainer für die App:</span><span class="sxs-lookup"><span data-stu-id="7f7cd-209">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="7f7cd-210">Erweiterungen</span><span class="sxs-lookup"><span data-stu-id="7f7cd-210">Extensibility</span></span>

<span data-ttu-id="7f7cd-211">Die Erweiterung des Hosts wird mit der Erweiterungsmethode in `IHostBuilder` durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-211">Host extensibility is performed with extension methods on `IHostBuilder`.</span></span> <span data-ttu-id="7f7cd-212">Im folgenden Beispiel wird dargestellt, wie eine Erweiterungsmethode eine `IHostBuilder`-Implementierung mit [RabbitMQ](https://www.rabbitmq.com/) erweitert.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-212">The following example shows how an extension method extends an `IHostBuilder` implementation with [RabbitMQ](https://www.rabbitmq.com/).</span></span> <span data-ttu-id="7f7cd-213">Die Erweiterungsmethode (an anderer Stelle in der App) registriert eine `IHostedService`-Schnittstelle von RabbitMQ:</span><span class="sxs-lookup"><span data-stu-id="7f7cd-213">The extension method (elsewhere in the app) registers a RabbitMQ `IHostedService`:</span></span>

```csharp
// UseRabbitMq is an extension method that sets up RabbitMQ to handle incoming
// messages.
var host = new HostBuilder()
    .UseRabbitMq<MyMessageHandler>()
    .Build();

await host.StartAsync();
```

## <a name="manage-the-host"></a><span data-ttu-id="7f7cd-214">Verwalten des Hosts</span><span class="sxs-lookup"><span data-stu-id="7f7cd-214">Manage the host</span></span>

<span data-ttu-id="7f7cd-215">Die Implementierung von [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) ist für das Starten und Beenden der Implementierungen von `IHostedService` verantwortlich, die im Dienstcontainer registriert sind.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-215">The [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) implementation is responsible for starting and stopping the `IHostedService` implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="7f7cd-216">Run</span><span class="sxs-lookup"><span data-stu-id="7f7cd-216">Run</span></span>

<span data-ttu-id="7f7cd-217">Durch [Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) wird die App gestartet und der aufrufende Thread blockiert, bis der Host heruntergefahren wird:</span><span class="sxs-lookup"><span data-stu-id="7f7cd-217">[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="7f7cd-218">RunAsync</span><span class="sxs-lookup"><span data-stu-id="7f7cd-218">RunAsync</span></span>

<span data-ttu-id="7f7cd-219">Durch [RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) wird die App ausgeführt und eine `Task`-Methode ausgegeben, die abgeschlossen wird, wenn das Abbruchtoken oder das Herunterfahren ausgelöst wird:</span><span class="sxs-lookup"><span data-stu-id="7f7cd-219">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) runs the app and returns a `Task` that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="7f7cd-220">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="7f7cd-220">RunConsoleAsync</span></span>

<span data-ttu-id="7f7cd-221">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) aktiviert die Unterstützung der Konsole, erstellt und startet den Host und wartet auf `Ctrl+C`/SIGINT oder SIGTERM, um das Herunterfahren auszulösen.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-221">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) enables console support, builds and starts the host, and waits for `Ctrl+C`/SIGINT or SIGTERM to shut down.</span></span>

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

### <a name="start-and-stopasync"></a><span data-ttu-id="7f7cd-222">Start und StopAsync</span><span class="sxs-lookup"><span data-stu-id="7f7cd-222">Start and StopAsync</span></span>

<span data-ttu-id="7f7cd-223">[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) startet den Host synchron.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-223">[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) starts the host synchronously.</span></span>

<span data-ttu-id="7f7cd-224">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) versucht, den Host innerhalb des angegebenen Timeouts zu beenden.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-224">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="7f7cd-225">StartAsync und StopAsync</span><span class="sxs-lookup"><span data-stu-id="7f7cd-225">StartAsync and StopAsync</span></span>

<span data-ttu-id="7f7cd-226">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) startet die App.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-226">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) starts the app.</span></span>

<span data-ttu-id="7f7cd-227">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) stoppt die App.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-227">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="7f7cd-228">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="7f7cd-228">WaitForShutdown</span></span>

<span data-ttu-id="7f7cd-229">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) wird über die [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime)-Schnittstelle ausgelöst (z.B. die [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime)-Klasse, die auf `Ctrl+C`/SIGINT oder SIGTERM wartet).</span><span class="sxs-lookup"><span data-stu-id="7f7cd-229">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) is triggered via the [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), such as [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (listens for `Ctrl+C`/SIGINT or SIGTERM).</span></span> <span data-ttu-id="7f7cd-230">`WaitForShutdown` ruft [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) auf.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-230">`WaitForShutdown` calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="7f7cd-231">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="7f7cd-231">WaitForShutdownAsync</span></span>

<span data-ttu-id="7f7cd-232">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) gibt eine `Task`-Methode zurück, die abgeschlossen wird, wenn das Herunterfahren über das angegebene Token ausgelöst wird, und ruft [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) auf.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-232">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) returns a `Task` that completes when shutdown is triggered via the given token and calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="external-control"></a><span data-ttu-id="7f7cd-233">Externe Steuerung</span><span class="sxs-lookup"><span data-stu-id="7f7cd-233">External control</span></span>

<span data-ttu-id="7f7cd-234">Die externe Steuerung des Hosts kann mithilfe von Methoden durchgeführt werden, die extern aufgerufen werden können:</span><span class="sxs-lookup"><span data-stu-id="7f7cd-234">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="7f7cd-235">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) wird am Anfang der [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync)-Methode aufgerufen, die den Vorgang erst fortsetzt, wenn sie abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-235">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) is called at the start of [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), which waits until it's complete before continuing.</span></span> <span data-ttu-id="7f7cd-236">Dies kann verwendet werden, um den Start zu verzögern, bis dieser durch ein externes Ereignis initiiert wird.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-236">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="7f7cd-237">IHostingEnvironment-Schnittstelle</span><span class="sxs-lookup"><span data-stu-id="7f7cd-237">IHostingEnvironment interface</span></span>

<span data-ttu-id="7f7cd-238">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) stellt Informationen über die Hostingumgebung der App bereit.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-238">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) provides information about the app's hosting environment.</span></span> <span data-ttu-id="7f7cd-239">Verwenden Sie [Constructor Injection](xref:fundamentals/dependency-injection) zum Abrufen der `IHostingEnvironment`-Schnittstelle, um deren Eigenschaften und Erweiterungsmethoden zu verwenden:</span><span class="sxs-lookup"><span data-stu-id="7f7cd-239">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="7f7cd-240">Weitere Informationen finden Sie unter [Verwenden mehrerer Umgebungen](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="7f7cd-240">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="7f7cd-241">IApplicationLifetime-Schnittstelle</span><span class="sxs-lookup"><span data-stu-id="7f7cd-241">IApplicationLifetime interface</span></span>

<span data-ttu-id="7f7cd-242">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) ermöglicht Aktivitäten nach dem Starten und beim Herunterfahren (einschließlich Anforderungen für ordnungsgemäßes Herunterfahren).</span><span class="sxs-lookup"><span data-stu-id="7f7cd-242">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="7f7cd-243">Bei drei der Eigenschaften der Schnittstelle handelt es sich um Abbruchtokens, die zum Registrieren von `Action`-Methoden verwendet werden, durch die Ereignisse beim Starten und Herunterfahren definiert werden.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-243">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="7f7cd-244">Abbruchtoken</span><span class="sxs-lookup"><span data-stu-id="7f7cd-244">Cancellation Token</span></span> | <span data-ttu-id="7f7cd-245">Wird ausgelöst, wenn&#8230;</span><span class="sxs-lookup"><span data-stu-id="7f7cd-245">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| [<span data-ttu-id="7f7cd-246">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="7f7cd-246">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="7f7cd-247">Der Host vollständig gestartet wurde.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-247">The host has fully started.</span></span> |
| [<span data-ttu-id="7f7cd-248">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="7f7cd-248">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="7f7cd-249">Der Host ein ordnungsgemäßes Herunterfahren abschließt.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-249">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="7f7cd-250">Alle Anforderungen sollten verarbeitet worden sein.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-250">All requests should be processed.</span></span> <span data-ttu-id="7f7cd-251">Das Herunterfahren wird blockiert, bis das Ereignis abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-251">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="7f7cd-252">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="7f7cd-252">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="7f7cd-253">Der Host ein ordnungsgemäßes Herunterfahren ausführt.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-253">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="7f7cd-254">Anforderungen möglicherweise noch verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-254">Requests may still be processing.</span></span> <span data-ttu-id="7f7cd-255">Das Herunterfahren wird blockiert, bis das Ereignis abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-255">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="7f7cd-256">Fügen Sie den `IApplicationLifetime`-Dienst über einen Konstruktor in eine beliebige Klasse ein.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-256">Constructor-inject the `IApplicationLifetime` service into any class.</span></span> <span data-ttu-id="7f7cd-257">Die [Beispiel-App](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) verwendet die Konstruktorinjektion für eine `LifetimeEventsHostedService`-Klasse (eine `IHostedService`-Implementierung), um die Ereignisse zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-257">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses contructor injection into a `LifetimeEventsHostedService` class (an `IHostedService` implementation) to register the events.</span></span>

<span data-ttu-id="7f7cd-258">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="7f7cd-258">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="7f7cd-259">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) fordert das Beenden der Anwendung an.</span><span class="sxs-lookup"><span data-stu-id="7f7cd-259">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="7f7cd-260">Die folgende Klasse verwendet `StopApplication`, um eine App ordnungsgemäß herunterzufahren, wenn die `Shutdown`-Methode der Klasse aufgerufen wird:</span><span class="sxs-lookup"><span data-stu-id="7f7cd-260">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="7f7cd-261">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="7f7cd-261">Additional resources</span></span>

* [<span data-ttu-id="7f7cd-262">Hintergrundaufgaben mit gehosteten Diensten</span><span class="sxs-lookup"><span data-stu-id="7f7cd-262">Background tasks with hosted services</span></span>](xref:fundamentals/host/hosted-services)
* [<span data-ttu-id="7f7cd-263">Hosten von Beispielrepositorys auf GitHub</span><span class="sxs-lookup"><span data-stu-id="7f7cd-263">Hosting repo samples on GitHub</span></span>](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
