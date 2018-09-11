---
title: Erweitern einer App mit einer externen Assembly in ASP.NET Core mit IHostingStartup
author: guardrex
description: Erfahren Sie, wie Sie eine ASP.NET Core-App aus einer externen Assembly mithilfe einer Implementierung von IHostingStartup erweitern.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2018
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: 2eddfa03b28564fcca7cc098e353b05e23b7c6f6
ms.sourcegitcommit: a669c4e3f42e387e214a354ac4143555602e6f66
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336255"
---
# <a name="enhance-an-app-from-an-external-assembly-in-aspnet-core-with-ihostingstartup"></a><span data-ttu-id="1584d-103">Erweitern einer App mit einer externen Assembly in ASP.NET Core mit IHostingStartup</span><span class="sxs-lookup"><span data-stu-id="1584d-103">Enhance an app from an external assembly in ASP.NET Core with IHostingStartup</span></span>

<span data-ttu-id="1584d-104">Von [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1584d-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="1584d-105">Mit einer [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup)-Implementierung (Hostingstart) werden Verbesserungen an einer App beim Start von einer externen Assemblys aus vorgenommen.</span><span class="sxs-lookup"><span data-stu-id="1584d-105">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) (hosting startup) implementation adds enhancements to an app at startup from an external assembly.</span></span> <span data-ttu-id="1584d-106">Eine externe Bibliothek kann beispielsweise eine Hostingstartimplementierung verwenden, um zusätzliche Konfigurationsanbieter oder -dienste für eine App bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="1584d-106">For example, an external library can use a hosting startup implementation to provide additional configuration providers or services to an app.</span></span> <span data-ttu-id="1584d-107">`IHostingStartup` *ist in ASP.NET Core 2.0 und höher verfügbar*.</span><span class="sxs-lookup"><span data-stu-id="1584d-107">`IHostingStartup` *is available in ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="1584d-108">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1584d-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="hostingstartup-attribute"></a><span data-ttu-id="1584d-109">Das Attribut HostingStartup</span><span class="sxs-lookup"><span data-stu-id="1584d-109">HostingStartup attribute</span></span>

<span data-ttu-id="1584d-110">Ein [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute)-Attribut gibt an, ob eine Hostingstartassembly vorhanden ist, die zur Laufzeit aktiviert werden kann.</span><span class="sxs-lookup"><span data-stu-id="1584d-110">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute indicates the presence of a hosting startup assembly to activate at runtime.</span></span>

<span data-ttu-id="1584d-111">Bei der Einstiegsassembly oder bei der Assembly, die die Klasse `Startup` enthält, wird automatisch geprüft, ob sie das Attribut `HostingStartup` enthält.</span><span class="sxs-lookup"><span data-stu-id="1584d-111">The entry assembly or the assembly containing the `Startup` class is automatically scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="1584d-112">Die Liste der Assemblys für die Suche nach `HostingStartup`-Attributen wird zur Laufzeit aus der Konfiguration in [WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) geladen.</span><span class="sxs-lookup"><span data-stu-id="1584d-112">The list of assemblies to search for `HostingStartup` attributes is loaded at runtime from configuration in the [WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey).</span></span> <span data-ttu-id="1584d-113">Die Liste der Assemblys, die aus der Ermittlung ausgeschlossen werden sollen, wird aus [WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey) geladen.</span><span class="sxs-lookup"><span data-stu-id="1584d-113">The list of assemblies to exclude from discovery is loaded from the [WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey).</span></span> <span data-ttu-id="1584d-114">Weitere Informationen finden Sie unter [Webhost: Hostingstartassemblys](xref:fundamentals/host/web-host#hosting-startup-assemblies) und [Web Host: Hosting Startup Exclude Assemblies (Webhost: Hostingstartausschlussassemblys)](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span><span class="sxs-lookup"><span data-stu-id="1584d-114">For more information, see [Web Host: Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) and [Web Host: Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span></span>

<span data-ttu-id="1584d-115">Im folgenden Beispiel lautet der Namespace der Hostingstartassembly `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="1584d-115">In the following example, the namespace of the hosting startup assembly is `StartupEnhancement`.</span></span> <span data-ttu-id="1584d-116">`StartupEnhancementHostingStartup` ist die Klasse mit dem Hostingstartcode:</span><span class="sxs-lookup"><span data-stu-id="1584d-116">The class containing the hosting startup code is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="1584d-117">Das Attribut `HostingStartup` befindet sich in der Regel in der `IHostingStartup`-Implementierungsklassendatei der Hostingstartassembly.</span><span class="sxs-lookup"><span data-stu-id="1584d-117">The `HostingStartup` attribute is typically located in the hosting startup assembly's `IHostingStartup` implementation class file.</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="1584d-118">Ermitteln geladener Hostingstartassemblys</span><span class="sxs-lookup"><span data-stu-id="1584d-118">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="1584d-119">Zum Ermitteln von geladenen Hostingstartassemblys aktivieren Sie die Protokollierung und überprüfen die Protokolle der App.</span><span class="sxs-lookup"><span data-stu-id="1584d-119">To discover loaded hosting startup assemblies, enable logging and check the app's logs.</span></span> <span data-ttu-id="1584d-120">Fehler, die beim Laden von Assemblys auftreten, werden protokolliert.</span><span class="sxs-lookup"><span data-stu-id="1584d-120">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="1584d-121">Geladene Hostingstartassemblys werden auf der Debugebene protokolliert. Außerdem werden alle Fehler protokolliert.</span><span class="sxs-lookup"><span data-stu-id="1584d-121">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="1584d-122">Deaktivieren des automatischen Ladens von Hostingstartassemblys</span><span class="sxs-lookup"><span data-stu-id="1584d-122">Disable automatic loading of hosting startup assemblies</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="1584d-123">Um das automatische Laden von Hostingstartassemblys zu deaktivieren, verwenden Sie einen der folgenden Ansätze:</span><span class="sxs-lookup"><span data-stu-id="1584d-123">To disable automatic loading of hosting startup assemblies, use one of the following approaches:</span></span>

* <span data-ttu-id="1584d-124">Um zu verhindern, dass alle Hostingstartassemblys geladen werden, legen Sie eine der folgenden Einstellungen auf `true` oder `1` fest:</span><span class="sxs-lookup"><span data-stu-id="1584d-124">To prevent all hosting startup assemblies from loading, set one of the following to `true` or `1`:</span></span>
  * <span data-ttu-id="1584d-125">Hostkonfigurationseinstellung [Verhindern des Hostingstarts](xref:fundamentals/host/web-host#prevent-hosting-startup).</span><span class="sxs-lookup"><span data-stu-id="1584d-125">[Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
  * <span data-ttu-id="1584d-126">Die Umgebungsvariable `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="1584d-126">`ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>
* <span data-ttu-id="1584d-127">Um zu verhindern, dass bestimmte Hostingstartassemblys geladen werden, legen Sie eine der folgenden Einstellungen auf eine durch Semikolons getrennte Zeichenfolge mit Hostingstartassemblys fest, die beim Starten ausgeschlossen werden sollen:</span><span class="sxs-lookup"><span data-stu-id="1584d-127">To prevent specific hosting startup assemblies from loading, set one of the following to a semicolon-delimited string of hosting startup assemblies to exclude at startup:</span></span>
  * <span data-ttu-id="1584d-128">Hostkonfigurationseinstellung [Hostingstartausschlussassemblys](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span><span class="sxs-lookup"><span data-stu-id="1584d-128">[Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies) host configuration setting.</span></span>
  * <span data-ttu-id="1584d-129">Die Umgebungsvariable `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="1584d-129">`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` environment variable.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="1584d-130">Um das automatische Laden von Hostingstartassemblys zu deaktivieren, legen Sie eine der folgenden Einstellungen auf `true` oder `1` fest:</span><span class="sxs-lookup"><span data-stu-id="1584d-130">To disable automatic loading of hosting startup assemblies, set one of the following to `true` or `1`:</span></span>

* <span data-ttu-id="1584d-131">Hostkonfigurationseinstellung [Verhindern des Hostingstarts](xref:fundamentals/host/web-host#prevent-hosting-startup).</span><span class="sxs-lookup"><span data-stu-id="1584d-131">[Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
* <span data-ttu-id="1584d-132">Die Umgebungsvariable `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="1584d-132">`ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>

::: moniker-end

<span data-ttu-id="1584d-133">Wenn sowohl die Konfigurationseinstellung für den Host als auch die Umgebungsvariable festgelegt werden, wird das Verhalten durch die Hosteinstellung gesteuert.</span><span class="sxs-lookup"><span data-stu-id="1584d-133">If both the host configuration setting and the environment variable are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="1584d-134">Durch das Deaktivieren von Hostingstartassemblys mithilfe der Hosteinstellung oder der Umgebungsvariable wird die Assembly global deaktiviert. Einige Eigenschaften einer App können ebenfalls deaktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="1584d-134">Disabling hosting startup assemblies using the host setting or environment variable disables the assembly globally and may disable several characteristics of an app.</span></span>

## <a name="project"></a><span data-ttu-id="1584d-135">Projekt</span><span class="sxs-lookup"><span data-stu-id="1584d-135">Project</span></span>

<span data-ttu-id="1584d-136">Erstellen Sie einen Hostingstart mit einem der folgenden Projekttypen:</span><span class="sxs-lookup"><span data-stu-id="1584d-136">Create a hosting startup with either of the following project types:</span></span>

* [<span data-ttu-id="1584d-137">Klassenbibliothek</span><span class="sxs-lookup"><span data-stu-id="1584d-137">Class library</span></span>](#class-library)
* [<span data-ttu-id="1584d-138">Konsolen-App ohne Einstiegspunkt</span><span class="sxs-lookup"><span data-stu-id="1584d-138">Console app without an entry point</span></span>](#console-app-without-an-entry-point)

### <a name="class-library"></a><span data-ttu-id="1584d-139">Klassenbibliothek</span><span class="sxs-lookup"><span data-stu-id="1584d-139">Class library</span></span>

<span data-ttu-id="1584d-140">In einer Klassenbibliothek kann eine Hostingstarterweiterung bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="1584d-140">A hosting startup enhancement can be provided in a class library.</span></span> <span data-ttu-id="1584d-141">Die Bibliothek enthält ein `HostingStartup`-Attribut.</span><span class="sxs-lookup"><span data-stu-id="1584d-141">The library contains a `HostingStartup` attribute.</span></span>

<span data-ttu-id="1584d-142">Der [Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) enthält die Razor Pages-App *HostingStartupApp* und die Klassenbibliothek *HostingStartupLibrary*.</span><span class="sxs-lookup"><span data-stu-id="1584d-142">The [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) includes a Razor Pages app, *HostingStartupApp*, and a class library, *HostingStartupLibrary*.</span></span> <span data-ttu-id="1584d-143">Die Klassenbibliothek:</span><span class="sxs-lookup"><span data-stu-id="1584d-143">The class library:</span></span>

* <span data-ttu-id="1584d-144">Enthält die Hostingstartklasse `ServiceKeyInjection`, die `IHostingStartup` implementiert.</span><span class="sxs-lookup"><span data-stu-id="1584d-144">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="1584d-145">`ServiceKeyInjection` fügt mithilfe des Anbieters der Konfiguration im Arbeitsspeicher ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection)) der App-Konfiguration zwei Dienstzeichenfolgen hinzu.</span><span class="sxs-lookup"><span data-stu-id="1584d-145">`ServiceKeyInjection` adds a pair of service strings to the app's configuration using the in-memory configuration provider ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection)).</span></span>
* <span data-ttu-id="1584d-146">Enthält ein `HostingStartup`-Attribut, das den Namespace und die Klasse des Hostingstarts angibt.</span><span class="sxs-lookup"><span data-stu-id="1584d-146">Includes a `HostingStartup` attribute that identifies the hosting startup's namespace and class.</span></span>

<span data-ttu-id="1584d-147">Die [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure)-Methode der Klasse `ServiceKeyInjection` verwendet [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder), um einer App Erweiterungen hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="1584d-147">The `ServiceKeyInjection` class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app.</span></span> <span data-ttu-id="1584d-148">`IHostingStartup.Configure` wird in der Hoststartassembly von der Runtime vor `Startup.Configure` im Benutzercode aufgerufen. Dadurch kann Benutzercode jede Konfiguration der Hoststartassembly überschreiben.</span><span class="sxs-lookup"><span data-stu-id="1584d-148">`IHostingStartup.Configure` in the hosting startup assembly is called by the runtime before `Startup.Configure` in user code, which allows user code to overwrite any configuration provided by the hosting startup assembly.</span></span>

<span data-ttu-id="1584d-149">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span><span class="sxs-lookup"><span data-stu-id="1584d-149">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupLibrary/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="1584d-150">Die Indexseite der App liest und rendert die Konfigurationswerte für die beiden Schlüssel, die von der Hostingstartassembly der Klassenbibliothek festgelegt werden:</span><span class="sxs-lookup"><span data-stu-id="1584d-150">The app's Index page reads and renders the configuration values for the two keys set by the class library's hosting startup assembly:</span></span>

<span data-ttu-id="1584d-151">*HostingStartupApp/Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="1584d-151">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=5-6,11-12)]

<span data-ttu-id="1584d-152">Der [Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) enthält darüber hinaus auch ein NuGet-Paketprojekt, das den separaten Hostingstart *HostingStartupPackage* bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="1584d-152">The [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) also includes a NuGet package project that provides a separate hosting startup, *HostingStartupPackage*.</span></span> <span data-ttu-id="1584d-153">Das Paket weist dieselben Merkmale wie die bereits beschriebene Klassenbibliothek auf.</span><span class="sxs-lookup"><span data-stu-id="1584d-153">The package has the same characteristics of the class library described earlier.</span></span> <span data-ttu-id="1584d-154">Das Paket:</span><span class="sxs-lookup"><span data-stu-id="1584d-154">The package:</span></span>

* <span data-ttu-id="1584d-155">Enthält die Hostingstartklasse `ServiceKeyInjection`, die `IHostingStartup` implementiert.</span><span class="sxs-lookup"><span data-stu-id="1584d-155">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="1584d-156">`ServiceKeyInjection` fügt der App-Konfiguration zwei Dienstzeichenfolgen hinzu.</span><span class="sxs-lookup"><span data-stu-id="1584d-156">`ServiceKeyInjection` adds a pair of service strings to the app's configuration.</span></span>
* <span data-ttu-id="1584d-157">Enthält ein `HostingStartup`-Attribut.</span><span class="sxs-lookup"><span data-stu-id="1584d-157">Includes a `HostingStartup` attribute.</span></span>

<span data-ttu-id="1584d-158">*HostingStartupPackage/ServiceKeyInjection.cs*:</span><span class="sxs-lookup"><span data-stu-id="1584d-158">*HostingStartupPackage/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupPackage/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="1584d-159">Die Indexseite der App liest und rendert die Konfigurationswerte für die beiden Schlüssel, die von der Hostingstartassembly der Bibliothek festgelegt werden:</span><span class="sxs-lookup"><span data-stu-id="1584d-159">The app's Index page reads and renders the configuration values for the two keys set by the package's hosting startup assembly:</span></span>

<span data-ttu-id="1584d-160">*HostingStartupApp/Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="1584d-160">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=7-8,13-14)]

### <a name="console-app-without-an-entry-point"></a><span data-ttu-id="1584d-161">Konsolen-App ohne Einstiegspunkt</span><span class="sxs-lookup"><span data-stu-id="1584d-161">Console app without an entry point</span></span>

<span data-ttu-id="1584d-162">*Dieses Verfahren ist nur für .NET Core-Apps, nicht jedoch für .NET Framework verfügbar.*</span><span class="sxs-lookup"><span data-stu-id="1584d-162">*This approach is only available for .NET Core apps, not .NET Framework.*</span></span>

<span data-ttu-id="1584d-163">Eine dynamische Hostingstarterweiterung, die zur Aktivierung keinen Kompilierungszeitverweis erfordert, kann in einer Konsolen-App ohne Einstiegspunkt bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="1584d-163">A dynamic hosting startup enhancement that doesn't require a compile-time reference for activation can be provided in a console app without an entry point.</span></span> <span data-ttu-id="1584d-164">Die App enthält ein `HostingStartup`-Attribut.</span><span class="sxs-lookup"><span data-stu-id="1584d-164">The app contains a `HostingStartup` attribute.</span></span> <span data-ttu-id="1584d-165">So erstellen Sie einen dynamischen Hostingstart:</span><span class="sxs-lookup"><span data-stu-id="1584d-165">To create a dynamic hosting startup:</span></span>

1. <span data-ttu-id="1584d-166">Aus der Klasse, die die `IHostingStartup`-Implementierung enthält wird eine Implementierungsbibliothek erstellt.</span><span class="sxs-lookup"><span data-stu-id="1584d-166">An implementation library is created from the class that contains the `IHostingStartup` implementation.</span></span> <span data-ttu-id="1584d-167">Die Implementierungsbibliothek wird wie ein gewöhnliches Paket behandelt.</span><span class="sxs-lookup"><span data-stu-id="1584d-167">The implementation library is treated as a normal package.</span></span>
1. <span data-ttu-id="1584d-168">Eine Konsolen-App ohne Einstiegspunkt verweist auf das Paket der Implementierungsbibliothek.</span><span class="sxs-lookup"><span data-stu-id="1584d-168">A console app without an entry point references the implementation library package.</span></span> <span data-ttu-id="1584d-169">Eine Konsolen-App wird aus folgenden Gründen verwendet:</span><span class="sxs-lookup"><span data-stu-id="1584d-169">A console app is used because:</span></span>
   * <span data-ttu-id="1584d-170">Eine Abhängigkeitendatei ist ein ausführbares App-Objekt, sodass eine Bibliothek keine Abhängigkeitendateien bereitstellen kann.</span><span class="sxs-lookup"><span data-stu-id="1584d-170">A dependencies file is a runnable app asset, so a library can't furnish a dependencies file.</span></span>
   * <span data-ttu-id="1584d-171">Eine Bibliothek kann dem [Laufzeitpaketspeicher](/dotnet/core/deploying/runtime-store), der ein ausführbares Projekt benötigt, das die freigegebene Laufzeit als Ziel verwendet, nicht direkt hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="1584d-171">A library can't be added directly to the [runtime package store](/dotnet/core/deploying/runtime-store), which requires a runnable project that targets the shared runtime.</span></span>
1. <span data-ttu-id="1584d-172">Die Konsolen-App wird veröffentlicht, um die Abhängigkeiten des Hostingstarts abzurufen.</span><span class="sxs-lookup"><span data-stu-id="1584d-172">The console app is published to obtain the hosting startup's dependencies.</span></span> <span data-ttu-id="1584d-173">Die Veröffentlichung der Konsolen-App hat unter anderem zur Folge, dass nicht verwendete Abhängigkeiten aus der Abhängigkeitendatei entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="1584d-173">A consequence of publishing the console app is that unused dependencies are trimmed from the dependencies file.</span></span>
1. <span data-ttu-id="1584d-174">Die App und die dazu gehörende Abhängigkeitendatei werden im Laufzeitpaketspeicher abgelegt.</span><span class="sxs-lookup"><span data-stu-id="1584d-174">The app and its dependencies file is placed into the runtime package store.</span></span> <span data-ttu-id="1584d-175">Damit die Hostingstartassembly und die entsprechende Abhängigkeitendatei erkannt werden, wird auf sie in zwei Umgebungsvariablen verwiesen.</span><span class="sxs-lookup"><span data-stu-id="1584d-175">To discover the hosting startup assembly and its dependencies file, they're referenced in a pair of environment variables.</span></span>

<span data-ttu-id="1584d-176">Die Konsolen-App verweist auf das Paket [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/):</span><span class="sxs-lookup"><span data-stu-id="1584d-176">The console app references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.csproj)]

<span data-ttu-id="1584d-177">Ein [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute)-Attribut identifiziert eine Klasse bei der Erstellung von [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) als eine Implementierung von `IHostingStartup` zum Laden und Ausführen.</span><span class="sxs-lookup"><span data-stu-id="1584d-177">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span></span> <span data-ttu-id="1584d-178">Im folgenden Beispiel ist der Namespace `StartupEnhancement` und die Klasse ist `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="1584d-178">In the following example, the namespace is `StartupEnhancement`, and the class is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="1584d-179">Eine Klasse implementiert `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="1584d-179">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="1584d-180">Die [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure)-Methode der Klasse verwendet [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder), um einer App Erweiterungen hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="1584d-180">The class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app.</span></span> <span data-ttu-id="1584d-181">`IHostingStartup.Configure` wird in der Hoststartassembly von der Runtime vor `Startup.Configure` im Benutzercode aufgerufen. Dadurch kann Benutzercode jede Konfiguration der Hoststartassembly überschreiben.</span><span class="sxs-lookup"><span data-stu-id="1584d-181">`IHostingStartup.Configure` in the hosting startup assembly is called by the runtime before `Startup.Configure` in user code, which allows user code to overwrite any configuration provided by the hosting startup assembly.</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="1584d-182">Die Datei für die Abhängigkeiten (*\*.deps.json*) legt beim Erstellen eine `IHostingStartup`-Projekts den `runtime`-Speicherort der Assembly auf den *bin*-Ordner fest:</span><span class="sxs-lookup"><span data-stu-id="1584d-182">When building an `IHostingStartup` project, the dependencies file (*\*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="1584d-183">Nur ein Teil der Datei wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="1584d-183">Only part of the file is shown.</span></span> <span data-ttu-id="1584d-184">`StartupEnhancement` ist der Name der Assembly im Beispiel.</span><span class="sxs-lookup"><span data-stu-id="1584d-184">The assembly name in the example is `StartupEnhancement`.</span></span>

## <a name="specify-the-hosting-startup-assembly"></a><span data-ttu-id="1584d-185">Angeben der Hostingstartassembly</span><span class="sxs-lookup"><span data-stu-id="1584d-185">Specify the hosting startup assembly</span></span>

<span data-ttu-id="1584d-186">Geben Sie für einen von der Klassenbibliothek oder von der Konsolen-App bereitgestellten Hostingstart den Namen der Hostingstartassembly in der Umgebungsvariablen `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` an.</span><span class="sxs-lookup"><span data-stu-id="1584d-186">For either a class library- or console app-supplied hosting startup, specify the hosting startup assembly's name in the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span> <span data-ttu-id="1584d-187">Die Umgebungsvariable ist eine durch Semikolons getrennte Liste von Assemblys.</span><span class="sxs-lookup"><span data-stu-id="1584d-187">The environment variable is a semicolon-delimited list of assemblies.</span></span>

<span data-ttu-id="1584d-188">Nur in Hostingstartassemblys wird nach dem `HostingStartup`-Attribut gesucht.</span><span class="sxs-lookup"><span data-stu-id="1584d-188">Only hosting startup assemblies are scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="1584d-189">Damit die Beispiel-App *HostingStartupApp* die weiter oben beschriebenen Hostingstarts erkennt, wird für die Umgebungsvariable der folgende Wert festgelegt:</span><span class="sxs-lookup"><span data-stu-id="1584d-189">For the sample app, *HostingStartupApp*, to discover the hosting startups described earlier, the environment variable is set to the following value:</span></span>

```
HostingStartupLibrary;HostingStartupPackage;StartupDiagnostics
```

<span data-ttu-id="1584d-190">Eine Hostingstartassembly kann auch mithilfe der Hostkonfigurationseinstellung [Hostingstartassemblys](xref:fundamentals/host/web-host#hosting-startup-assemblies) festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="1584d-190">A hosting startup assembly can also be set using the [Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="1584d-191">Wenn mehrere Hoststartassemblys vorhanden sind, werden ihre [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure)-Methoden in der Reihenfolge der aufgelisteten Assemblys ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="1584d-191">When multiple hosting startup assembles are present, their [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) methods are executed in the order that the assemblies are listed.</span></span>

## <a name="activation"></a><span data-ttu-id="1584d-192">Aktivierung</span><span class="sxs-lookup"><span data-stu-id="1584d-192">Activation</span></span>

<span data-ttu-id="1584d-193">Optionen für die Hostingstartaktivierung:</span><span class="sxs-lookup"><span data-stu-id="1584d-193">Options for hosting startup activation are:</span></span>

* <span data-ttu-id="1584d-194">[Laufzeitspeicher](#runtime-store) &ndash; Für die Aktivierung ist kein Kompilierzeitverweis erforderlich.</span><span class="sxs-lookup"><span data-stu-id="1584d-194">[Runtime store](#runtime-store) &ndash; Activation doesn't require a compile-time reference for activation.</span></span> <span data-ttu-id="1584d-195">Die Beispiel-App legt die Hostingstartassembly und die Abhängigkeitendateien im Ordner *deployment* ab, um die Bereitstellung des Hostingstarts in einer Umgebung mit mehreren Computern zu erleichtern.</span><span class="sxs-lookup"><span data-stu-id="1584d-195">The sample app places the hosting startup assembly and dependencies files into a folder, *deployment*, to facilitate deployment of the hosting startup in a multimachine environment.</span></span> <span data-ttu-id="1584d-196">Der Ordner *deployment* enthält darüber hinaus auch ein PowerShell-Skript, das im Bereitstellungssystem Umgebungsvariablen erstellt oder ändert, um den Hostingstart zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="1584d-196">The *deployment* folder also includes a PowerShell script that creates or modifies environment variables on the deployment system to enable the hosting startup.</span></span>
* <span data-ttu-id="1584d-197">Kompilierzeitverweis für die Aktivierung erforderlich</span><span class="sxs-lookup"><span data-stu-id="1584d-197">Compile-time reference required for activation</span></span>
  * [<span data-ttu-id="1584d-198">NuGet-Paket</span><span class="sxs-lookup"><span data-stu-id="1584d-198">NuGet package</span></span>](#nuget-package)
  * [<span data-ttu-id="1584d-199">Ordner „Bin“ des Projekts</span><span class="sxs-lookup"><span data-stu-id="1584d-199">Project bin folder</span></span>](#project-bin-folder)

### <a name="runtime-store"></a><span data-ttu-id="1584d-200">Laufzeitspeicher</span><span class="sxs-lookup"><span data-stu-id="1584d-200">Runtime store</span></span>

<span data-ttu-id="1584d-201">Die Hostingstartimplementierung wird im [Laufzeitspeicher](/dotnet/core/deploying/runtime-store) abgelegt.</span><span class="sxs-lookup"><span data-stu-id="1584d-201">The hosting startup implementation is placed in the [runtime store](/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="1584d-202">Ein Kompilierzeitverweis auf die Assembly wird von der erweiterten App nicht benötigt.</span><span class="sxs-lookup"><span data-stu-id="1584d-202">A compile-time reference to the assembly isn't required by the enhanced app.</span></span>

<span data-ttu-id="1584d-203">Nach dem Erstellen des Hostingstarts dient die Projektdatei des Hostingstarts als Manifestdatei für den Befehl [dotnet store](/dotnet/core/tools/dotnet-store).</span><span class="sxs-lookup"><span data-stu-id="1584d-203">After the hosting startup is built, the hosting startup's project file serves as the manifest file for the [dotnet store](/dotnet/core/tools/dotnet-store) command.</span></span>

```console
dotnet store --manifest <PROJECT_FILE> --runtime <RUNTIME_IDENTIFIER>
```

<span data-ttu-id="1584d-204">Mit diesem Befehl werden die Hostingstartassembly und andere Abhängigkeiten, die nicht Teil des freigegebenen Frameworks sind, im Laufzeitspeicher des Benutzerprofils an der folgenden Stelle abgelegt:</span><span class="sxs-lookup"><span data-stu-id="1584d-204">This command places the hosting startup assembly and other dependencies that aren't part of the shared framework in the user profile's runtime store at:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="1584d-205">Windows</span><span class="sxs-lookup"><span data-stu-id="1584d-205">Windows</span></span>](#tab/windows)

```
%USERPROFILE%\.dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="1584d-206">macOS</span><span class="sxs-lookup"><span data-stu-id="1584d-206">macOS</span></span>](#tab/macos)

```
/Users/<USER>/.dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="1584d-207">Linux</span><span class="sxs-lookup"><span data-stu-id="1584d-207">Linux</span></span>](#tab/linux)

```
/Users/<USER>/.dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

---

<span data-ttu-id="1584d-208">Wenn Sie die Assembly und die Abhängigkeiten zur globalen Verwendung ablegen möchten, fügen Sie dem `dotnet store`-Befehl die `-o|--output`-Option mit dem folgenden Pfad hinzu:</span><span class="sxs-lookup"><span data-stu-id="1584d-208">If you desire to place the assembly and dependencies for global use, add the `-o|--output` option to the `dotnet store` command with the following path:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="1584d-209">Windows</span><span class="sxs-lookup"><span data-stu-id="1584d-209">Windows</span></span>](#tab/windows)

```
%PROGRAMFILES%\dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="1584d-210">macOS</span><span class="sxs-lookup"><span data-stu-id="1584d-210">macOS</span></span>](#tab/macos)

```
/usr/local/share/dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="1584d-211">Linux</span><span class="sxs-lookup"><span data-stu-id="1584d-211">Linux</span></span>](#tab/linux)

```
/usr/local/share/dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

---

<span data-ttu-id="1584d-212">**Ändern und Ablegen der Abhängigkeitendatei des Hostingstarts**</span><span class="sxs-lookup"><span data-stu-id="1584d-212">**Modify and place the hosting startup's dependencies file**</span></span>

<span data-ttu-id="1584d-213">Der Runtime-Speicherort wird in der *\*.deps.json*-Datei angegeben.</span><span class="sxs-lookup"><span data-stu-id="1584d-213">The runtime location is specified in the *\*.deps.json* file.</span></span> <span data-ttu-id="1584d-214">Das `runtime`-Element muss den Speicherort der Laufzeitassembly der Erweiterung angeben, um die Erweiterung zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="1584d-214">To activate the enhancement, the `runtime` element must specify the location of the enhancement's runtime assembly.</span></span> <span data-ttu-id="1584d-215">Setzen Sie dem `runtime`-Speicherort `lib/<TARGET_FRAMEWORK_MONIKER>/` voran:</span><span class="sxs-lookup"><span data-stu-id="1584d-215">Prefix the `runtime` location with `lib/<TARGET_FRAMEWORK_MONIKER>/`:</span></span>

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement2.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="1584d-216">Im Beispielcode (*StartupDiagnostics*-Projekt) wird die Änderung der *\*.deps.json*-Datei von einem [PowerShell](/powershell/scripting/powershell-scripting)-Skript durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="1584d-216">In the sample code (*StartupDiagnostics* project), modification of the *\*.deps.json* file is performed by a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span> <span data-ttu-id="1584d-217">Das PowerShell-Skript wird automatisch von einem Buildziel in der Projektdatei gestartet.</span><span class="sxs-lookup"><span data-stu-id="1584d-217">The PowerShell script is automatically triggered by a build target in the project file.</span></span>

<span data-ttu-id="1584d-218">Die *\*.deps.json*-Datei der Implementierung muss sich an einem erreichbaren Speicherort befinden.</span><span class="sxs-lookup"><span data-stu-id="1584d-218">The implementation's *\*.deps.json* file must be in an accessible location.</span></span>

<span data-ttu-id="1584d-219">Platzieren Sie die Datei im Ordner *additonalDeps* der `.dotnet`-Einstellung des Benutzerprofils für die Verwendung pro Benutzer:</span><span class="sxs-lookup"><span data-stu-id="1584d-219">For per-user use, place the file in the *additonalDeps* folder of the user profile's `.dotnet` settings:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="1584d-220">Windows</span><span class="sxs-lookup"><span data-stu-id="1584d-220">Windows</span></span>](#tab/windows)

```
%USERPROFILE%\.dotnet\x64\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="1584d-221">macOS</span><span class="sxs-lookup"><span data-stu-id="1584d-221">macOS</span></span>](#tab/macos)

```
/Users/<USER>/.dotnet/x64/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="1584d-222">Linux</span><span class="sxs-lookup"><span data-stu-id="1584d-222">Linux</span></span>](#tab/linux)

```
/Users/<USER>/.dotnet/x64/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

---

<span data-ttu-id="1584d-223">Platzieren Sie die Datei im *additonalDeps*-Ordner der .NET Core-Installation für die globale Verwendung:</span><span class="sxs-lookup"><span data-stu-id="1584d-223">For global use, place the file in the *additonalDeps* folder of the .NET Core installation:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="1584d-224">Windows</span><span class="sxs-lookup"><span data-stu-id="1584d-224">Windows</span></span>](#tab/windows)

```
%PROGRAMFILES%\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="1584d-225">macOS</span><span class="sxs-lookup"><span data-stu-id="1584d-225">macOS</span></span>](#tab/macos)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="1584d-226">Linux</span><span class="sxs-lookup"><span data-stu-id="1584d-226">Linux</span></span>](#tab/linux)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

---

<span data-ttu-id="1584d-227">Beachten Sie, dass die freigegebene Frameworkversion die Version der freigegebenen Runtime angibt, die die Ziel-App verwendet.</span><span class="sxs-lookup"><span data-stu-id="1584d-227">The shared framework version reflects the version of the shared runtime that the target app uses.</span></span> <span data-ttu-id="1584d-228">Die freigegebene Runtime wird in der *\*.runtimeconfig.json*-Datei angezeigt.</span><span class="sxs-lookup"><span data-stu-id="1584d-228">The shared runtime is shown in the *\*.runtimeconfig.json* file.</span></span> <span data-ttu-id="1584d-229">In der Beispiel-App (*HostingStartupApp*) wird die freigegebene Laufzeit in der Datei *HostingStartupApp.runtimeconfig.json* angegeben.</span><span class="sxs-lookup"><span data-stu-id="1584d-229">In the sample app (*HostingStartupApp*), the shared runtime is specified in the *HostingStartupApp.runtimeconfig.json* file.</span></span>

<span data-ttu-id="1584d-230">**Angeben der Abhängigkeitendatei des Hostingstarts**</span><span class="sxs-lookup"><span data-stu-id="1584d-230">**List the hosting startup's dependencies file**</span></span>

<span data-ttu-id="1584d-231">Der Speicherort der *\*.deps.json*-Datei der Implementierung ist in der Umgebungsvariablen `DOTNET_ADDITIONAL_DEPS` angegeben.</span><span class="sxs-lookup"><span data-stu-id="1584d-231">The location of the implementation's *\*.deps.json* file is listed in the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>

<span data-ttu-id="1584d-232">Wenn die Datei im Ordner *.dotnet* des Benutzerprofils abgelegt wird, legen Sie den Wert der Umgebungsvariablen wie folgt fest:</span><span class="sxs-lookup"><span data-stu-id="1584d-232">If the file is placed in the user profile's *.dotnet* folder, set the environment variable's value to:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="1584d-233">Windows</span><span class="sxs-lookup"><span data-stu-id="1584d-233">Windows</span></span>](#tab/windows)

```
%USERPROFILE%\.dotnet\x64\additionalDeps\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="1584d-234">macOS</span><span class="sxs-lookup"><span data-stu-id="1584d-234">macOS</span></span>](#tab/macos)

```
/Users/<USER>/.dotnet/x64/additionalDeps/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="1584d-235">Linux</span><span class="sxs-lookup"><span data-stu-id="1584d-235">Linux</span></span>](#tab/linux)

```
/Users/<USER>/.dotnet/x64/additionalDeps/
```

---

<span data-ttu-id="1584d-236">Wenn die Datei in der .NET Core-Installation für die globale Verwendung platziert wird, geben Sie den vollständigen Pfad zur Datei an:</span><span class="sxs-lookup"><span data-stu-id="1584d-236">If the file is placed in the .NET Core installation for global use, provide the full path to the file:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="1584d-237">Windows</span><span class="sxs-lookup"><span data-stu-id="1584d-237">Windows</span></span>](#tab/windows)

```
%PROGRAMFILES%\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

# <a name="macostabmacos"></a>[<span data-ttu-id="1584d-238">macOS</span><span class="sxs-lookup"><span data-stu-id="1584d-238">macOS</span></span>](#tab/macos)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="1584d-239">Linux</span><span class="sxs-lookup"><span data-stu-id="1584d-239">Linux</span></span>](#tab/linux)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

---

<span data-ttu-id="1584d-240">Damit die Beispiel-App (*HostingStartupApp*) die Abhängigkeitendatei (*HostingStartupApp.runtimeconfig.json*) findet, wird die Abhängigkeitendatei im Profil des Benutzers abgelegt.</span><span class="sxs-lookup"><span data-stu-id="1584d-240">For the sample app (*HostingStartupApp*) to find the dependencies file (*HostingStartupApp.runtimeconfig.json*), the dependencies file is placed in the user's profile.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="1584d-241">Windows</span><span class="sxs-lookup"><span data-stu-id="1584d-241">Windows</span></span>](#tab/windows)

<span data-ttu-id="1584d-242">Legen Sie für die Umgebungsvariable `DOTNET_ADDITIONAL_DEPS` den folgenden Wert fest:</span><span class="sxs-lookup"><span data-stu-id="1584d-242">Set the `DOTNET_ADDITIONAL_DEPS` environment variable to the following value:</span></span>

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="1584d-243">macOS</span><span class="sxs-lookup"><span data-stu-id="1584d-243">macOS</span></span>](#tab/macos)

<span data-ttu-id="1584d-244">Legen Sie für die Umgebungsvariable `DOTNET_ADDITIONAL_DEPS` den folgenden Wert fest:</span><span class="sxs-lookup"><span data-stu-id="1584d-244">Set the `DOTNET_ADDITIONAL_DEPS` environment variable to the following value:</span></span>

```
/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="1584d-245">Linux</span><span class="sxs-lookup"><span data-stu-id="1584d-245">Linux</span></span>](#tab/linux)

<span data-ttu-id="1584d-246">Legen Sie für die Umgebungsvariable `DOTNET_ADDITIONAL_DEPS` den folgenden Wert fest:</span><span class="sxs-lookup"><span data-stu-id="1584d-246">Set the `DOTNET_ADDITIONAL_DEPS` environment variable to the following value:</span></span>

```
/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/
```

---

<span data-ttu-id="1584d-247">Beispiele zum Festlegen von Umgebungsvariablen für verschiedene Betriebssysteme finden Sie unter [Use multiple environments (Verwenden mehrerer Umgebungen)](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="1584d-247">For examples of how to set environment variables for various operating systems, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="1584d-248">**Bereitstellung**</span><span class="sxs-lookup"><span data-stu-id="1584d-248">**Deployment**</span></span>

<span data-ttu-id="1584d-249">Um die Bereitstellung eines Hostingstarts in einer Umgebung mit mehreren Computern zu erleichtern, erstellt die Beispiel-App in der veröffentlichten Ausgabe einen *deployment*-Ordner, der Folgendes enthält:</span><span class="sxs-lookup"><span data-stu-id="1584d-249">To facilitate the deployment of a hosting startup in a multimachine environment, the sample app creates a *deployment* folder in published output that contains:</span></span>

* <span data-ttu-id="1584d-250">Die Hostingstartassembly.</span><span class="sxs-lookup"><span data-stu-id="1584d-250">The hosting startup assembly.</span></span>
* <span data-ttu-id="1584d-251">Die Abhängigkeitendatei des Hostingstarts.</span><span class="sxs-lookup"><span data-stu-id="1584d-251">The hosting startup dependencies file.</span></span>
* <span data-ttu-id="1584d-252">Ein PowerShell-Skript, das die `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` und `DOTNET_ADDITIONAL_DEPS` erstellt oder ändert, um die Aktivierung des Hostingstarts zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="1584d-252">A PowerShell script that creates or modifies the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` and `DOTNET_ADDITIONAL_DEPS` to support the activation of the hosting startup.</span></span> <span data-ttu-id="1584d-253">Führen Sie das Skript über eine PowerShell-Administratoreingabeaufforderung auf dem Bereitstellungssystem aus.</span><span class="sxs-lookup"><span data-stu-id="1584d-253">Run the script from an administrative PowerShell command prompt on the deployment system.</span></span>

### <a name="nuget-package"></a><span data-ttu-id="1584d-254">NuGet-Paket</span><span class="sxs-lookup"><span data-stu-id="1584d-254">NuGet package</span></span>

<span data-ttu-id="1584d-255">In einem NuGet-Paket kann eine Hostingstarterweiterung bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="1584d-255">A hosting startup enhancement can be provided in a NuGet package.</span></span> <span data-ttu-id="1584d-256">Das Paket enthält ein `HostingStartup`-Attribut.</span><span class="sxs-lookup"><span data-stu-id="1584d-256">The package has a `HostingStartup` attribute.</span></span> <span data-ttu-id="1584d-257">Die vom Paket bereitgestellten Hostingstarttypen werden für die App mit einem der folgenden Verfahren verfügbar gemacht:</span><span class="sxs-lookup"><span data-stu-id="1584d-257">The hosting startup types provided by the package are made available to the app using either of the following approaches:</span></span>

* <span data-ttu-id="1584d-258">Die Projektdatei der erweiterten App erstellt einen Paketverweis für den Hostingstart in der Projektdatei der App (Kompilierzeitverweis).</span><span class="sxs-lookup"><span data-stu-id="1584d-258">The enhanced app's project file makes a package reference for the hosting startup in the app's project file (a compile-time reference).</span></span> <span data-ttu-id="1584d-259">Wenn ein Kompilierzeitverweis eingerichtet ist, werden die Hostingstartassembly und alle dazu gehörenden Abhängigkeiten in die Abhängigkeitendatei der App (*\*.deps.json*) eingebunden.</span><span class="sxs-lookup"><span data-stu-id="1584d-259">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*\*.deps.json*).</span></span> <span data-ttu-id="1584d-260">Dieses Verfahren gilt für ein in [nuget.org](https://www.nuget.org/) veröffentlichtes Hostingstartassembly-Paket.</span><span class="sxs-lookup"><span data-stu-id="1584d-260">This approach applies to a hosting startup assembly package published to [nuget.org](https://www.nuget.org/).</span></span>
* <span data-ttu-id="1584d-261">Die Abhängigkeitendatei des Hostingstarts wird für die erweiterte App wie im Abschnitt [Laufzeitspeicher](#runtime-store) beschrieben (ohne Kompilierzeitverweis) verfügbar gemacht.</span><span class="sxs-lookup"><span data-stu-id="1584d-261">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>

<span data-ttu-id="1584d-262">Weitere Informationen zu NuGet-Paketen und den Laufzeitspeicher finden Sie in den folgenden Themen:</span><span class="sxs-lookup"><span data-stu-id="1584d-262">For more information on NuGet packages and the runtime store, see the following topics:</span></span>

* [<span data-ttu-id="1584d-263">So erstellen Sie ein NuGet-Paket mit plattformübergreifenden Tools</span><span class="sxs-lookup"><span data-stu-id="1584d-263">How to Create a NuGet Package with Cross Platform Tools</span></span>](/dotnet/core/deploying/creating-nuget-packages)
* [<span data-ttu-id="1584d-264">Veröffentlichen von Paketen</span><span class="sxs-lookup"><span data-stu-id="1584d-264">Publishing packages</span></span>](/nuget/create-packages/publish-a-package)
* [<span data-ttu-id="1584d-265">Laufzeitpaketspeicher</span><span class="sxs-lookup"><span data-stu-id="1584d-265">Runtime package store</span></span>](/dotnet/core/deploying/runtime-store)

### <a name="project-bin-folder"></a><span data-ttu-id="1584d-266">Ordner „Bin“ des Projekts</span><span class="sxs-lookup"><span data-stu-id="1584d-266">Project bin folder</span></span>

<span data-ttu-id="1584d-267">Eine Hostingstarterweiterung kann durch eine mittels *bin* bereitgestellte Assembly in der erweiterten App bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="1584d-267">A hosting startup enhancement can be provided by a *bin*-deployed assembly in the enhanced app.</span></span> <span data-ttu-id="1584d-268">Die von der Assembly bereitgestellten Hostingstarttypen werden für die App mit einem der folgenden Verfahren verfügbar gemacht:</span><span class="sxs-lookup"><span data-stu-id="1584d-268">The hosting startup types provided by the assembly are made available to the app using either of the following approaches:</span></span>

* <span data-ttu-id="1584d-269">Die Projektdatei der erweiterten App enthält einen Assemblyverweis auf den Hostingstart (Kompilierzeitverweis).</span><span class="sxs-lookup"><span data-stu-id="1584d-269">The enhanced app's project file makes an assembly reference to the hosting startup (a compile-time reference).</span></span> <span data-ttu-id="1584d-270">Wenn ein Kompilierzeitverweis eingerichtet ist, werden die Hostingstartassembly und alle dazu gehörenden Abhängigkeiten in die Abhängigkeitendatei der App (*\*.deps.json*) eingebunden.</span><span class="sxs-lookup"><span data-stu-id="1584d-270">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*\*.deps.json*).</span></span> <span data-ttu-id="1584d-271">Dieses Verfahren wird angewendet, wenn das Bereitstellungsszenario verlangt, dass die kompilierte Assembly der Hostingstartbibliothek (DLL-Datei) in das verwendete Projekt oder an einen Speicherort verschoben wird, auf den das verwendete Projekt zugreifen kann, und ein Kompilierzeitverweis auf die Assembly des Hostingstarts erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="1584d-271">This approach applies when the deployment scenario calls for moving the compiled hosting startup library's assembly (DLL file) to the consuming project or to a location accessible by the consuming project and a compile-time reference is made to the hosting startup's assembly.</span></span>
* <span data-ttu-id="1584d-272">Die Abhängigkeitendatei des Hostingstarts wird für die erweiterte App wie im Abschnitt [Laufzeitspeicher](#runtime-store) beschrieben (ohne Kompilierzeitverweis) verfügbar gemacht.</span><span class="sxs-lookup"><span data-stu-id="1584d-272">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>

## <a name="sample-code"></a><span data-ttu-id="1584d-273">Beispielcode</span><span class="sxs-lookup"><span data-stu-id="1584d-273">Sample code</span></span>

<span data-ttu-id="1584d-274">Der [Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([Herunterladen](xref:tutorials/index#how-to-download-a-sample)) zeigt Szenarios für die Hostingstartimplementierung:</span><span class="sxs-lookup"><span data-stu-id="1584d-274">The [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample)) demonstrates hosting startup implementation scenarios:</span></span>

* <span data-ttu-id="1584d-275">Zwei Hostingstartassemblys (Klassenbibliotheken) legen zwei Schlüssel-Wert-Paare der Konfiguration im Arbeitsspeicher fest:</span><span class="sxs-lookup"><span data-stu-id="1584d-275">Two hosting startup assemblies (class libraries) set a pair of in-memory configuration key-value pairs each:</span></span>
  * <span data-ttu-id="1584d-276">NuGet-Paket (*HostingStartupPackage*)</span><span class="sxs-lookup"><span data-stu-id="1584d-276">NuGet package (*HostingStartupPackage*)</span></span>
  * <span data-ttu-id="1584d-277">Klassenbibliothek (*HostingStartupLibrary*)</span><span class="sxs-lookup"><span data-stu-id="1584d-277">Class library (*HostingStartupLibrary*)</span></span>
* <span data-ttu-id="1584d-278">Ein Hostingstart wird über eine im Laufzeitspeicher bereitgestellte Assembly (*StartupDiagnostics*) aktiviert.</span><span class="sxs-lookup"><span data-stu-id="1584d-278">A hosting startup is activated from a runtime store-deployed assembly (*StartupDiagnostics*).</span></span> <span data-ttu-id="1584d-279">Die Assembly fügt der App zum Start zwei Middlewares hinzu, die Diagnoseinformationen zu folgenden Komponenten bereitstellen:</span><span class="sxs-lookup"><span data-stu-id="1584d-279">The assembly adds two middlewares to the app at startup that provide diagnostic information on:</span></span>
  * <span data-ttu-id="1584d-280">Registrierte Dienste</span><span class="sxs-lookup"><span data-stu-id="1584d-280">Registered services</span></span>
  * <span data-ttu-id="1584d-281">Adresse (Schema, Host, Basispfad, Pfad, Abfragezeichenfolge)</span><span class="sxs-lookup"><span data-stu-id="1584d-281">Address (scheme, host, path base, path, query string)</span></span>
  * <span data-ttu-id="1584d-282">Verbindung (Remote-IP, Remoteport, lokale IP, lokaler Port, Clientzertifikat)</span><span class="sxs-lookup"><span data-stu-id="1584d-282">Connection (remote IP, remote port, local IP, local port, client certificate)</span></span>
  * <span data-ttu-id="1584d-283">Anforderungsheader</span><span class="sxs-lookup"><span data-stu-id="1584d-283">Request headers</span></span>
  * <span data-ttu-id="1584d-284">Umgebungsvariablen</span><span class="sxs-lookup"><span data-stu-id="1584d-284">Environment variables</span></span>

<span data-ttu-id="1584d-285">So führen Sie das Beispiel aus:</span><span class="sxs-lookup"><span data-stu-id="1584d-285">To run the sample:</span></span>

<span data-ttu-id="1584d-286">**Aktivierung über ein NuGet-Paket**</span><span class="sxs-lookup"><span data-stu-id="1584d-286">**Activation from a NuGet package**</span></span>

1. <span data-ttu-id="1584d-287">Kompilieren Sie das Paket *HostingStartupPackage* mit dem Befehl [dotnet pack](/dotnet/core/tools/dotnet-pack).</span><span class="sxs-lookup"><span data-stu-id="1584d-287">Compile the *HostingStartupPackage* package with the [dotnet pack](/dotnet/core/tools/dotnet-pack) command.</span></span>
1. <span data-ttu-id="1584d-288">Fügen Sie den Assemblynamen des Pakets *HostingStartupPackage* zur Umgebungsvariablen `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` hinzu.</span><span class="sxs-lookup"><span data-stu-id="1584d-288">Add the package's assembly name of the *HostingStartupPackage* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="1584d-289">Kompilieren Sie die App, und führen Sie diese aus.</span><span class="sxs-lookup"><span data-stu-id="1584d-289">Compile and run the app.</span></span> <span data-ttu-id="1584d-290">In der erweiterten App ist ein Paketverweis vorhanden (Kompilierzeitverweis).</span><span class="sxs-lookup"><span data-stu-id="1584d-290">A package reference is present in the enhanced app (a compile-time reference).</span></span> <span data-ttu-id="1584d-291">Eine `<PropertyGroup>` in der Projektdatei der App gibt die Ausgabe des Paketprojekts (*../HostingStartupPackage/bin/Debug*) als Paketquelle an.</span><span class="sxs-lookup"><span data-stu-id="1584d-291">A `<PropertyGroup>` in the app's project file specifies the package project's output (*../HostingStartupPackage/bin/Debug*) as a package source.</span></span> <span data-ttu-id="1584d-292">Dadurch kann die Anwendung das Paket verwenden, ohne das Paket in [nuget.org](https://www.nuget.org/) hochzuladen. Weitere Informationen finden Sie in den Hinweisen in der Projektdatei von HostingStartupApp.</span><span class="sxs-lookup"><span data-stu-id="1584d-292">This allows the app to use the package without uploading the package to [nuget.org](https://www.nuget.org/). For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <PropertyGroup>
     <RestoreSources>$(RestoreSources);https://api.nuget.org/v3/index.json;../HostingStartupPackage/bin/Debug</RestoreSources>
   </PropertyGroup>
   ```
1. <span data-ttu-id="1584d-293">Die von der Indexseite gerenderten Schlüsselwerte der Dienstkonfiguration entsprechen den durch die `ServiceKeyInjection.Configure`-Methode des Pakets festgelegten Werten.</span><span class="sxs-lookup"><span data-stu-id="1584d-293">Observe that the service configuration key values rendered by the Index page match the values set by the package's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="1584d-294">Wenn Sie am Projekt *HostingStartupPackage* Änderungen vornehmen und das Projekt erneut kompilieren, bereinigen Sie die lokalen NuGet-Paketcaches, um sicherzustellen, dass die *HostingStartupApp* das aktualisierte Paket erhält und kein veraltetes Paket aus dem lokalen Cache.</span><span class="sxs-lookup"><span data-stu-id="1584d-294">If you make changes to the *HostingStartupPackage* project and recompile it, clear the local NuGet package caches to ensure that the *HostingStartupApp* receives the updated package and not a stale package from the local cache.</span></span> <span data-ttu-id="1584d-295">Um die lokalen NuGet-Caches zu bereinigen, führen Sie den folgenden [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals)-Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="1584d-295">To clear the local NuGet caches, execute the following [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals) command:</span></span>

```console
dotnet nuget locals all --clear
```

<span data-ttu-id="1584d-296">**Aktivierung über eine Klassenbibliothek**</span><span class="sxs-lookup"><span data-stu-id="1584d-296">**Activation from a class library**</span></span>

1. <span data-ttu-id="1584d-297">Kompilieren Sie die Klassenbibliothek *HostingStartupLibrary* mit dem Befehl [dotnet build](/dotnet/core/tools/dotnet-build).</span><span class="sxs-lookup"><span data-stu-id="1584d-297">Compile the *HostingStartupLibrary* class library with the [dotnet build](/dotnet/core/tools/dotnet-build) command.</span></span>
1. <span data-ttu-id="1584d-298">Fügen Sie der Umgebungsvariablen `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` den Assemblynamen *HostingStartupLibrary* der Klassenbibliothek hinzu.</span><span class="sxs-lookup"><span data-stu-id="1584d-298">Add the class library's assembly name of *HostingStartupLibrary* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="1584d-299">Stellen Sie die Assembly der Klassenbibliothek mittels *bin* für die App bereit, indem Sie die Datei *HostingStartupLibrary.dll* aus der kompilierten Ausgabe der Klassenbibliothek in den Ordner *bin/Debug* der App kopieren.</span><span class="sxs-lookup"><span data-stu-id="1584d-299">*bin*-deploy the class library's assembly to the app by copying the *HostingStartupLibrary.dll* file from the class library's compiled output to the app's *bin/Debug* folder.</span></span>
1. <span data-ttu-id="1584d-300">Kompilieren Sie die App, und führen Sie diese aus.</span><span class="sxs-lookup"><span data-stu-id="1584d-300">Compile and run the app.</span></span> <span data-ttu-id="1584d-301">Eine `<ItemGroup>` in der Projektdatei der App verweist auf die Assembly der Klassenbibliothek (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (Kompilierzeitverweis).</span><span class="sxs-lookup"><span data-stu-id="1584d-301">An `<ItemGroup>` in the app's project file references the class library's assembly (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (a compile-time reference).</span></span> <span data-ttu-id="1584d-302">Weitere Informationen finden Sie in den Hinweisen in der Projektdatei von HostingStartupApp.</span><span class="sxs-lookup"><span data-stu-id="1584d-302">For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <ItemGroup>
     <Reference Include=".\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll">
       <HintPath>.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll</HintPath>
       <SpecificVersion>False</SpecificVersion>
     </Reference>
   </ItemGroup>
   ```
1. <span data-ttu-id="1584d-303">Die von der Indexseite gerenderten Schlüsselwerte der Dienstkonfiguration entsprechen den durch die `ServiceKeyInjection.Configure`-Methode der Klassenbibliothek festgelegten Werten.</span><span class="sxs-lookup"><span data-stu-id="1584d-303">Observe that the service configuration key values rendered by the Index page match the values set by the class library's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="1584d-304">**Aktivierung über eine mittels Laufzeitspeicher bereitgestellten Assembly**</span><span class="sxs-lookup"><span data-stu-id="1584d-304">**Activation from a runtime store-deployed assembly**</span></span>

1. <span data-ttu-id="1584d-305">Im Projekt *StartupDiagnostics* wird [PowerShell](/powershell/scripting/powershell-scripting) verwendet, um die Datei *StartupDiagnostics.deps.json* zu bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="1584d-305">The *StartupDiagnostics* project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="1584d-306">PowerShell ist standardmäßig auf Windows-Betriebssystemen ab Windows 7 SP1 und Windows Server 2008 R2 SP1 installiert.</span><span class="sxs-lookup"><span data-stu-id="1584d-306">PowerShell is installed by default on Windows starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="1584d-307">Im Artikel [Installing Windows PowerShell (Installieren von Windows PowerShell)](/powershell/scripting/setup/installing-powershell#powershell-core) erfahren Sie, wie Sie PowerShell auf anderen Plattformen nutzen können.</span><span class="sxs-lookup"><span data-stu-id="1584d-307">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core).</span></span>
1. <span data-ttu-id="1584d-308">Erstellen Sie das Projekt *StartupDiagnostics*.</span><span class="sxs-lookup"><span data-stu-id="1584d-308">Build the *StartupDiagnostics* project.</span></span> <span data-ttu-id="1584d-309">Nachdem das Projekt erstellt wurde, werden von einem Buildziel in der Projektdatei automatisch folgende Aktionen ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="1584d-309">After the project is built, a build target in the project file automatically:</span></span>
   * <span data-ttu-id="1584d-310">Löst das PowerShell-Skript aus, um die *StartupDiagnostics.deps.json*-Datei zu ändern.</span><span class="sxs-lookup"><span data-stu-id="1584d-310">Triggers the PowerShell script to modify the *StartupDiagnostics.deps.json* file.</span></span>
   * <span data-ttu-id="1584d-311">Verschiebt die Datei *StartupDiagnostics.deps.json* in den Ordner *additionalDeps* des Benutzerprofils.</span><span class="sxs-lookup"><span data-stu-id="1584d-311">Moves the *StartupDiagnostics.deps.json* file to the user profile's *additionalDeps* folder.</span></span>
1. <span data-ttu-id="1584d-312">Führen Sie den Befehl `dotnet store` über eine Eingabeaufforderung im Verzeichnis des Hostingstarts aus, um die Assembly und die dazu gehörenden Abhängigkeiten im Laufzeitspeicher des Benutzerprofils zu speichern:</span><span class="sxs-lookup"><span data-stu-id="1584d-312">Execute the `dotnet store` command at a command prompt in the hosting startup's directory to store the assembly and its dependencies in the user profile's runtime store:</span></span>

   ```console
   dotnet store --manifest StartupDiagnostics.csproj --runtime <RID>
   ```
   <span data-ttu-id="1584d-313">Bei Windows wird für den Befehl der `win7-x64` [Laufzeitbezeichner (Runtime Identifier (RID))](/dotnet/core/rid-catalog) verwendet.</span><span class="sxs-lookup"><span data-stu-id="1584d-313">For Windows, the command uses the `win7-x64` [runtime identifier (RID)](/dotnet/core/rid-catalog).</span></span> <span data-ttu-id="1584d-314">Wenn der Hostingstart für eine andere Laufzeit bereitgestellt wird, muss die entsprechende RID eingegeben werden.</span><span class="sxs-lookup"><span data-stu-id="1584d-314">When providing the hosting startup for a different runtime, substitute the correct RID.</span></span>
1. <span data-ttu-id="1584d-315">Legen Sie die folgenden Umgebungsvariablen fest:</span><span class="sxs-lookup"><span data-stu-id="1584d-315">Set the environment variables:</span></span>
   * <span data-ttu-id="1584d-316">Fügen Sie der Umgebungsvariablen `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` den Assemblynamen *StartupDiagnostics* hinzu.</span><span class="sxs-lookup"><span data-stu-id="1584d-316">Add the assembly name of *StartupDiagnostics* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
   * <span data-ttu-id="1584d-317">Legen Sie bei Windows die Umgebungsvariable `DOTNET_ADDITIONAL_DEPS` auf `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\` fest.</span><span class="sxs-lookup"><span data-stu-id="1584d-317">On Windows, set the `DOTNET_ADDITIONAL_DEPS` environment variable to `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`.</span></span> <span data-ttu-id="1584d-318">Legen Sie bei macOS/Linux die Umgebungsvariable `DOTNET_ADDITIONAL_DEPS` auf `/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/`, wobei `<USER>` das Benutzerprofil ist, das den Hostingstart enthält.</span><span class="sxs-lookup"><span data-stu-id="1584d-318">On macOS/Linux, set the `DOTNET_ADDITIONAL_DEPS` environment variable to `/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/`, where `<USER>` is the user profile that contains the hosting startup.</span></span>
1. <span data-ttu-id="1584d-319">Führen Sie die Beispiel-App aus.</span><span class="sxs-lookup"><span data-stu-id="1584d-319">Run the sample app.</span></span>
1. <span data-ttu-id="1584d-320">Fordern Sie den Endpunkt `/services` an, um die registrierten Dienste der App anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="1584d-320">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="1584d-321">Fordern Sie den Endpunkt `/diag` an, um Diagnoseinformationen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="1584d-321">Request the `/diag` endpoint to see the diagnostic information.</span></span>
