---
title: Erweitern einer App mit einer externen Assembly in ASP.NET Core mit IHostingStartup
author: guardrex
description: Erfahren Sie, wie Sie eine ASP.NET Core-App aus einer externen Assembly mithilfe einer Implementierung von IHostingStartup erweitern.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: 47d3a64ce0cc543162a066eeeaa0aaaf7dc96a5f
ms.sourcegitcommit: 0d6f151e69c159d776ed0142773279e645edbc0a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2018
ms.locfileid: "35415007"
---
# <a name="enhance-an-app-from-an-external-assembly-in-aspnet-core-with-ihostingstartup"></a><span data-ttu-id="0fb57-103">Erweitern einer App mit einer externen Assembly in ASP.NET Core mit IHostingStartup</span><span class="sxs-lookup"><span data-stu-id="0fb57-103">Enhance an app from an external assembly in ASP.NET Core with IHostingStartup</span></span>

<span data-ttu-id="0fb57-104">Von [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0fb57-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="0fb57-105">Mit einer [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup)-Implementierung können einer App beim Start von einer externen Assemblys aus, die sich außerhalb der `Startup`-Klasse der App befindet, Erweiterungen hinzugefügt werden</span><span class="sxs-lookup"><span data-stu-id="0fb57-105">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="0fb57-106">Eine externe Toolbibliothek kann beispielsweise eine Implementierung von `IHostingStartup` verwenden, um zusätzliche Konfigurationsanbieter oder -dienste für eine App bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="0fb57-106">For example, an external tooling library can use an `IHostingStartup` implementation to provide additional configuration providers or services to an app.</span></span> <span data-ttu-id="0fb57-107">`IHostingStartup` *ist in ASP.NET Core 2.0 und höher verfügbar.*</span><span class="sxs-lookup"><span data-stu-id="0fb57-107">`IHostingStartup` *is available in ASP.NET Core 2.0 and later.*</span></span>

<span data-ttu-id="0fb57-108">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0fb57-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="0fb57-109">Ermitteln geladener Hostingstartassemblys</span><span class="sxs-lookup"><span data-stu-id="0fb57-109">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="0fb57-110">Aktivieren Sie die Protokollierung, und überprüfen Sie die Anwendungsprotokolle, um Hostingstartassemblys zu ermitteln, die von der App oder von Bibliotheken geladen wurden.</span><span class="sxs-lookup"><span data-stu-id="0fb57-110">To discover hosting startup assemblies loaded by the app or by libraries, enable logging and check the application logs.</span></span> <span data-ttu-id="0fb57-111">Fehler, die beim Laden von Assemblys auftreten, werden protokolliert.</span><span class="sxs-lookup"><span data-stu-id="0fb57-111">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="0fb57-112">Geladene Hostingstartassemblys werden auf der Debugebene protokolliert. Außerdem werden alle Fehler protokolliert.</span><span class="sxs-lookup"><span data-stu-id="0fb57-112">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

<span data-ttu-id="0fb57-113">Die Beispiel-App liest den [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) in ein `string`-Array und zeigt das Ergebnis auf der Indexseite der App an:</span><span class="sxs-lookup"><span data-stu-id="0fb57-113">The sample app reads the [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) into a `string` array and displays the result in the app's Index page:</span></span>

[!code-csharp[](platform-specific-configuration/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="0fb57-114">Deaktivieren des automatischen Ladens von Hostingstartassemblys</span><span class="sxs-lookup"><span data-stu-id="0fb57-114">Disable automatic loading of hosting startup assemblies</span></span>

<span data-ttu-id="0fb57-115">Es gibt zwei Möglichkeiten zum Deaktivieren des automatischen Ladens von Hostingstartassemblys:</span><span class="sxs-lookup"><span data-stu-id="0fb57-115">There are two ways to disable the automatic loading of hosting startup assemblies:</span></span>

* <span data-ttu-id="0fb57-116">Festlegen der Hostkonfigurationseinstellung [Verhindern des Hostingstarts](xref:fundamentals/host/web-host#prevent-hosting-startup).</span><span class="sxs-lookup"><span data-stu-id="0fb57-116">Set the [Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
* <span data-ttu-id="0fb57-117">Festlegen der Umgebungsvariable `ASPNETCORE_PREVENTHOSTINGSTARTUP`.</span><span class="sxs-lookup"><span data-stu-id="0fb57-117">Set the `ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>

<span data-ttu-id="0fb57-118">Wenn entweder die Hosteinstellung oder die Umgebungsvariable auf `true` oder `1` festgelegt ist, werden Hostingstartassemblys nicht automatisch geladen.</span><span class="sxs-lookup"><span data-stu-id="0fb57-118">When either the host setting or the environment variable is set to `true` or `1`, hosting startup assemblies aren't automatically loaded.</span></span> <span data-ttu-id="0fb57-119">Wenn beide festgelegt sind, wird das Verhalten durch die Hosteinstellung gesteuert.</span><span class="sxs-lookup"><span data-stu-id="0fb57-119">If both are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="0fb57-120">Durch das Deaktivieren von Hostingstartassemblys mithilfe der Hosteinstellung oder der Umgebungsvariable, werden sie global deaktiviert. Einige Eigenschaften einer App können ebenfalls deaktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="0fb57-120">Disabling hosting startup assemblies using the host setting or environment variable disables them globally and may disable several characteristics of an app.</span></span> <span data-ttu-id="0fb57-121">Momentan ist es nicht möglich, eine von einer Bibliothek hinzugefügte Hostingstartassembly gezielt zu deaktivieren, außer die Bibliothek bietet ihre eigene Konfigurationsoption.</span><span class="sxs-lookup"><span data-stu-id="0fb57-121">It isn't currently possible to selectively disable a hosting startup assembly added by a library unless the library offers its own configuration option.</span></span> <span data-ttu-id="0fb57-122">Eine zukünftige Version soll die Möglichkeit bieten, Hostingstartassemblys selektiv zu deaktivieren (siehe [GitHub issue aspnet/Hosting #1243 (GitHub-Problem ASP.NET/Hosting #1243)](https://github.com/aspnet/Hosting/pull/1243)).</span><span class="sxs-lookup"><span data-stu-id="0fb57-122">A future release will offer the ability to selectively disable hosting startup assemblies (see [GitHub issue aspnet/Hosting #1243](https://github.com/aspnet/Hosting/pull/1243)).</span></span>

## <a name="implement-ihostingstartup"></a><span data-ttu-id="0fb57-123">Implementieren von IHostingStartup</span><span class="sxs-lookup"><span data-stu-id="0fb57-123">Implement IHostingStartup</span></span>

### <a name="create-the-assembly"></a><span data-ttu-id="0fb57-124">Erstellen der Assembly</span><span class="sxs-lookup"><span data-stu-id="0fb57-124">Create the assembly</span></span>

<span data-ttu-id="0fb57-125">Eine `IHostingStartup`-Erweiterung wird als eine Assembly bereitgestellt, die auf einer Konsolen-App ohne Einstiegspunkt basiert.</span><span class="sxs-lookup"><span data-stu-id="0fb57-125">An `IHostingStartup` enhancement is deployed as an assembly based on a console app without an entry point.</span></span> <span data-ttu-id="0fb57-126">Die Assembly referenziert das Paket [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/):</span><span class="sxs-lookup"><span data-stu-id="0fb57-126">The assembly references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[](platform-specific-configuration/snapshot_sample/StartupEnhancement.csproj)]

<span data-ttu-id="0fb57-127">Ein [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute)-Attribut identifiziert eine Klasse bei der Erstellung von [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) als eine Implementierung von `IHostingStartup` zum Laden und Ausführen.</span><span class="sxs-lookup"><span data-stu-id="0fb57-127">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span></span> <span data-ttu-id="0fb57-128">Im folgenden Beispiel ist der Namespace `StartupEnhancement` und die Klasse ist `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="0fb57-128">In the following example, the namespace is `StartupEnhancement`, and the class is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="0fb57-129">Eine Klasse implementiert `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="0fb57-129">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="0fb57-130">Die [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure)-Methode der Klasse verwendet [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder), um einer App Erweiterungen hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="0fb57-130">The class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app.</span></span> <span data-ttu-id="0fb57-131">`IHostingStartup.Configure` wird in der Hoststartassembly von der Runtime vor `Startup.Configure` im Benutzercode aufgerufen. Dadurch kann Benutzercode jede Konfiguration der Hoststartassembly überschreiben.</span><span class="sxs-lookup"><span data-stu-id="0fb57-131">`IHostingStartup.Configure` in the hosting startup assembly is called by the runtime before `Startup.Configure` in user code, which allows user code to overwrite any configruation provided by the hosting startup assembly.</span></span>

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="0fb57-132">Die Datei für die Abhängigkeiten (*\*.deps.json*) legt beim Erstellen eine `IHostingStartup`-Projekts den `runtime`-Speicherort der Assembly auf den *bin*-Ordner fest:</span><span class="sxs-lookup"><span data-stu-id="0fb57-132">When building an `IHostingStartup` project, the dependencies file (*\*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="0fb57-133">Nur ein Teil der Datei wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0fb57-133">Only part of the file is shown.</span></span> <span data-ttu-id="0fb57-134">`StartupEnhancement` ist der Name der Assembly im Beispiel.</span><span class="sxs-lookup"><span data-stu-id="0fb57-134">The assembly name in the example is `StartupEnhancement`.</span></span>

### <a name="update-the-dependencies-file"></a><span data-ttu-id="0fb57-135">Aktualisieren der Abhängigkeitsdatei</span><span class="sxs-lookup"><span data-stu-id="0fb57-135">Update the dependencies file</span></span>

<span data-ttu-id="0fb57-136">Der Runtime-Speicherort wird in der *\*.deps.json*-Datei angegeben.</span><span class="sxs-lookup"><span data-stu-id="0fb57-136">The runtime location is specified in the *\*.deps.json* file.</span></span> <span data-ttu-id="0fb57-137">Das `runtime`-Element muss den Speicherort der Runtime-Assembly der Erweiterung angeben, um die Erweiterung zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="0fb57-137">To active the enhancement, the `runtime` element must specify the location of the enhancement's runtime assembly.</span></span> <span data-ttu-id="0fb57-138">Setzen Sie dem `runtime`-Speicherort `lib/<TARGET_FRAMEWORK_MONIKER>/` voran:</span><span class="sxs-lookup"><span data-stu-id="0fb57-138">Prefix the `runtime` location with `lib/<TARGET_FRAMEWORK_MONIKER>/`:</span></span>

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement2.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="0fb57-139">In der Beispiel-App wird die Änderung der *\*.deps.json*-Datei von einem [PowerShell-Skript](/powershell/scripting/powershell-scripting) durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="0fb57-139">In the sample app, modification of the *\*.deps.json* file is performed by a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span> <span data-ttu-id="0fb57-140">Das PowerShell-Skript wird automatisch von einem Buildziel in der Projektdatei gestartet.</span><span class="sxs-lookup"><span data-stu-id="0fb57-140">The PowerShell script is automatically triggered by a build target in the project file.</span></span>

### <a name="enhancement-activation"></a><span data-ttu-id="0fb57-141">Aktivierung der Erweiterung</span><span class="sxs-lookup"><span data-stu-id="0fb57-141">Enhancement activation</span></span>

<span data-ttu-id="0fb57-142">**Platzieren der Assemblydatei**</span><span class="sxs-lookup"><span data-stu-id="0fb57-142">**Place the assembly file**</span></span>

<span data-ttu-id="0fb57-143">Die Assemblydatei der `IHostingStartup`-Implementierung muss über *bin* in der App bereitgestellt oder im [Laufzeitspeicher](/dotnet/core/deploying/runtime-store) platziert werden:</span><span class="sxs-lookup"><span data-stu-id="0fb57-143">The `IHostingStartup` implementation's assembly file must be *bin*-deployed in the app or placed in the [runtime store](/dotnet/core/deploying/runtime-store):</span></span>

<span data-ttu-id="0fb57-144">Platzieren Sie die Assembly im Laufzeitspeicher des Benutzerprofils für die Verwendung pro Benutzer unter:</span><span class="sxs-lookup"><span data-stu-id="0fb57-144">For per-user use, place the assembly in the user profile's runtime store at:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

<span data-ttu-id="0fb57-145">Platzieren Sie die Assembly im Laufzeitspeicher in der .NET Core-Installation für die globale Verwendung unter:</span><span class="sxs-lookup"><span data-stu-id="0fb57-145">For global use, place the assembly in the .NET Core installation's runtime store:</span></span>

```
<DRIVE>\Program Files\dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

<span data-ttu-id="0fb57-146">Beim Bereitstellen der Assembly in den Laufzeitspeicher kann die Symboldatei ebenfalls bereitgestellt werden, was jedoch nicht erforderlich ist, damit die Erweiterung funktioniert.</span><span class="sxs-lookup"><span data-stu-id="0fb57-146">When deploying the assembly to the runtime store, the symbols file may be deployed as well but isn't required for the enhancement to work.</span></span>

<span data-ttu-id="0fb57-147">**Platzieren der Abhängigkeitsdatei**</span><span class="sxs-lookup"><span data-stu-id="0fb57-147">**Place the dependencies file**</span></span>

<span data-ttu-id="0fb57-148">Die *\*.deps.json*-Datei der Implementierung muss sich an einem erreichbaren Speicherort befinden.</span><span class="sxs-lookup"><span data-stu-id="0fb57-148">The implementation's *\*.deps.json* file must be in an accessible location.</span></span>

<span data-ttu-id="0fb57-149">Platzieren Sie die Datei im `additonalDeps`-Ordner der `.dotnet`-Einstellung des Benutzerprofils für die Verwendung pro Benutzer:</span><span class="sxs-lookup"><span data-stu-id="0fb57-149">For per-user use, place the file in the `additonalDeps` folder of the user profile's `.dotnet` settings:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

<span data-ttu-id="0fb57-150">Platzieren Sie die Datei im `additonalDeps`-Ordner der .NET Core-Installation für die globale Verwendung:</span><span class="sxs-lookup"><span data-stu-id="0fb57-150">For global use, place the file in the `additonalDeps` folder of the .NET Core installation:</span></span>

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

<span data-ttu-id="0fb57-151">Beachten Sie, dass die freigegebene Frameworkversion die Version der freigegebenen Runtime angibt, die die Ziel-App verwendet.</span><span class="sxs-lookup"><span data-stu-id="0fb57-151">The shared framework version reflects the version of the shared runtime that the target app uses.</span></span> <span data-ttu-id="0fb57-152">Die freigegebene Runtime wird in der *\*.runtimeconfig.json*-Datei angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0fb57-152">The shared runtime is shown in the *\*.runtimeconfig.json* file.</span></span> <span data-ttu-id="0fb57-153">In der Beispiel-App wird die freigegebene Runtime in der Datei *HostingStartupSample.runtimeconfig.json* angegeben.</span><span class="sxs-lookup"><span data-stu-id="0fb57-153">In the sample app, the shared runtime is specified in the *HostingStartupSample.runtimeconfig.json* file.</span></span>

<span data-ttu-id="0fb57-154">**Festlegen von Umgebungsvariablen**</span><span class="sxs-lookup"><span data-stu-id="0fb57-154">**Set environment variables**</span></span>

<span data-ttu-id="0fb57-155">Legen Sie die folgenden Umgebungsvariablen im Kontext der App fest, die die Erweiterung verwendet.</span><span class="sxs-lookup"><span data-stu-id="0fb57-155">Set the following environment variables in the context of the app that uses the enhancement.</span></span>

<span data-ttu-id="0fb57-156">ASPNETCORE_HOSTINGSTARTUPASSEMBLIES</span><span class="sxs-lookup"><span data-stu-id="0fb57-156">ASPNETCORE_HOSTINGSTARTUPASSEMBLIES</span></span>

<span data-ttu-id="0fb57-157">Nur Hostingstartassemblys werden auf `HostingStartupAttribute` überprüft.</span><span class="sxs-lookup"><span data-stu-id="0fb57-157">Only hosting startup assemblies are scanned for the `HostingStartupAttribute`.</span></span> <span data-ttu-id="0fb57-158">Der Assemblyname der Implementierung wird in dieser Umgebungsvariable bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="0fb57-158">The assembly name of the implementation is provided in this environment variable.</span></span> <span data-ttu-id="0fb57-159">In der Beispiel-App wird dieser Wert auf `StartupDiagnostics` festgelegt.</span><span class="sxs-lookup"><span data-stu-id="0fb57-159">The sample app sets this value to `StartupDiagnostics`.</span></span>

<span data-ttu-id="0fb57-160">Der Wert kann auch mithilfe der Hostkonfigurationseinstellung [Hostingstartassemblys](xref:fundamentals/host/web-host#hosting-startup-assemblies) festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="0fb57-160">The value can also be set using the [Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="0fb57-161">Wenn mehrere Hoststartassemblys vorhanden sind, werden ihre [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure)-Methoden in der Reihenfolge der aufgelisteten Assemblys ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="0fb57-161">When multiple hosting startup assembles are present, their [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) methods are executed in the order that the assemblies are listed.</span></span>

<span data-ttu-id="0fb57-162">DOTNET_ADDITIONAL_DEPS</span><span class="sxs-lookup"><span data-stu-id="0fb57-162">DOTNET_ADDITIONAL_DEPS</span></span>

<span data-ttu-id="0fb57-163">Beschreibt den Speicherort der *\*.deps.json*-Datei der Implementierung.</span><span class="sxs-lookup"><span data-stu-id="0fb57-163">The location of the implementation's *\*.deps.json* file.</span></span>

<span data-ttu-id="0fb57-164">Wenn die Datei im *DOTNET*-Ordner des Benutzerprofils für die Verwendung pro Benutzer platziert ist:</span><span class="sxs-lookup"><span data-stu-id="0fb57-164">If the file is placed in the user profile's *.dotnet* folder for per-user use:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\
```

<span data-ttu-id="0fb57-165">Wenn die Datei in der .NET Core-Installation für die globale Verwendung platziert wird, geben Sie den vollständigen Pfad zur Datei an:</span><span class="sxs-lookup"><span data-stu-id="0fb57-165">If the file is placed in the .NET Core installation for global use, provide the full path to the file:</span></span>

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

<span data-ttu-id="0fb57-166">In der Beispiel-App wird dieser Wert auf Folgendes festgelegt:</span><span class="sxs-lookup"><span data-stu-id="0fb57-166">The sample app sets this value to:</span></span>

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

<span data-ttu-id="0fb57-167">Beispiele zum Festlegen von Umgebungsvariablen für verschiedene Betriebssysteme finden Sie unter [Use multiple environments (Verwenden mehrerer Umgebungen)](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="0fb57-167">For examples of how to set environment variables for various operating systems, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="sample-app"></a><span data-ttu-id="0fb57-168">Beispiel-App</span><span class="sxs-lookup"><span data-stu-id="0fb57-168">Sample app</span></span>

<span data-ttu-id="0fb57-169">In der [Beispiel-App](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample)) wird `IHostingStartup` verwendet, um ein Diagnosetool zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="0fb57-169">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)) uses `IHostingStartup` to create a diagnostics tool.</span></span> <span data-ttu-id="0fb57-170">Das Tool fügt der App zum Start zwei Middlewares hinzu, die Diagnoseinformationen bereitstellen:</span><span class="sxs-lookup"><span data-stu-id="0fb57-170">The tool adds two middlewares to the app at startup that provide diagnostic information:</span></span>

* <span data-ttu-id="0fb57-171">Registrierte Dienste</span><span class="sxs-lookup"><span data-stu-id="0fb57-171">Registered services</span></span>
* <span data-ttu-id="0fb57-172">Adresse: Schema, Host, Basispfad, Pfad, Abfragezeichenfolge</span><span class="sxs-lookup"><span data-stu-id="0fb57-172">Address: scheme, host, path base, path, query string</span></span>
* <span data-ttu-id="0fb57-173">Verbindung: Remote-IP, Remoteport, lokale IP, lokaler Port, Clientzertifikat</span><span class="sxs-lookup"><span data-stu-id="0fb57-173">Connection: remote IP, remote port, local IP, local port, client certificate</span></span>
* <span data-ttu-id="0fb57-174">Anforderungsheader</span><span class="sxs-lookup"><span data-stu-id="0fb57-174">Request headers</span></span>
* <span data-ttu-id="0fb57-175">Umgebungsvariablen</span><span class="sxs-lookup"><span data-stu-id="0fb57-175">Environment variables</span></span>

<span data-ttu-id="0fb57-176">So führen Sie das Beispiel aus:</span><span class="sxs-lookup"><span data-stu-id="0fb57-176">To run the sample:</span></span>

1. <span data-ttu-id="0fb57-177">Im Startdiagnoseprojekt wird [PowerShell](/powershell/scripting/powershell-scripting) verwendet, um die *StartupDiagnostics.deps.json*-Datei zu bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="0fb57-177">The Startup Diagnostic project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="0fb57-178">PowerShell ist standardmäßig auf Windows-Betriebssystemen ab Windows 7 SP1 und Windows Server 2008 R2 SP1 installiert.</span><span class="sxs-lookup"><span data-stu-id="0fb57-178">PowerShell is installed by default on Windows OS starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="0fb57-179">Im Artikel [Installing Windows PowerShell (Installieren von Windows PowerShell)](/powershell/scripting/setup/installing-windows-powershell) erfahren Sie, wie Sie PowerShell auf anderen Plattformen nutzen können.</span><span class="sxs-lookup"><span data-stu-id="0fb57-179">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).</span></span>
2. <span data-ttu-id="0fb57-180">Erstellen Sie das Startdiagnoseprojekt.</span><span class="sxs-lookup"><span data-stu-id="0fb57-180">Build the Startup Diagnostic project.</span></span> <span data-ttu-id="0fb57-181">Ein Buildziel in der Projektdatei:</span><span class="sxs-lookup"><span data-stu-id="0fb57-181">A build target in the project file:</span></span>
   * <span data-ttu-id="0fb57-182">Verschiebt die Assembly und Symboldateien in den Laufzeitspeicher des Benutzerprofils.</span><span class="sxs-lookup"><span data-stu-id="0fb57-182">Moves the assembly and symbols files to the user profile's runtime store.</span></span>
   * <span data-ttu-id="0fb57-183">Löst das PowerShell-Skript aus, um die *StartupDiagnostics.deps.json*-Datei zu ändern.</span><span class="sxs-lookup"><span data-stu-id="0fb57-183">Triggers the PowerShell script to modify the *StartupDiagnostics.deps.json* file.</span></span>
   * <span data-ttu-id="0fb57-184">Verschiebt die *StartupDiagnostics.deps.json*-Datei in der `additionalDeps`-Ordner des Benutzerprofils.</span><span class="sxs-lookup"><span data-stu-id="0fb57-184">Moves the *StartupDiagnostics.deps.json* file to the user profile's `additionalDeps` folder.</span></span>
3. <span data-ttu-id="0fb57-185">Legen Sie die folgenden Umgebungsvariablen fest:</span><span class="sxs-lookup"><span data-stu-id="0fb57-185">Set the environment variables:</span></span>
    * <span data-ttu-id="0fb57-186">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`</span><span class="sxs-lookup"><span data-stu-id="0fb57-186">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`</span></span>
    * <span data-ttu-id="0fb57-187">`DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`</span><span class="sxs-lookup"><span data-stu-id="0fb57-187">`DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`</span></span>
4. <span data-ttu-id="0fb57-188">Führen Sie die Beispiel-App aus.</span><span class="sxs-lookup"><span data-stu-id="0fb57-188">Run the sample app.</span></span>
5. <span data-ttu-id="0fb57-189">Fordern Sie den Endpunkt `/services` an, um die registrierten Dienste der App anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="0fb57-189">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="0fb57-190">Fordern Sie den Endpunkt `/diag` an, um Diagnoseinformationen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="0fb57-190">Request the `/diag` endpoint to see the diagnostic information.</span></span>
