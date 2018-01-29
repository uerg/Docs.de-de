---
title: "Hinzufügen von app-Funktionen aus einer externen Assembly IHostingStartup in ASP.NET Core"
author: guardrex
description: "Zum Hinzufügen von Funktionen zu einer ASP.NET Core-app aus einer externen Assembly mit einer Implementierung IHostingStartup zu ermitteln."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 12/07/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/ihostingstartup
ms.openlocfilehash: 6013091d94302f0f9ecbc777d3ef64774b49aebe
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2018
---
# <a name="add-app-features-from-an-external-assembly-using-ihostingstartup-in-aspnet-core"></a><span data-ttu-id="deecd-103">Hinzufügen von app-Funktionen aus einer externen Assembly IHostingStartup in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="deecd-103">Add app features from an external assembly using IHostingStartup in ASP.NET Core</span></span>

<span data-ttu-id="deecd-104">Von [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="deecd-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="deecd-105">Ein [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) Implementierung ermöglicht das Hinzufügen von Funktionen zu einer app beim Start von außerhalb der app `Startup` Klasse.</span><span class="sxs-lookup"><span data-stu-id="deecd-105">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementation allows adding features to an app at startup from outside of the app's `Startup` class.</span></span> <span data-ttu-id="deecd-106">Eine Bibliothek von externen Tools können z. B. eine `IHostingStartup` Implementierung, um zusätzliche Konfigurationsanbieter oder Dienste für eine app bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="deecd-106">For example, an external tooling library can use an `IHostingStartup` implementation to provide additional configuration providers or services to an app.</span></span> <span data-ttu-id="deecd-107">`IHostingStartup`*in ASP.NET Core 2.0 und höher verfügbar ist.*</span><span class="sxs-lookup"><span data-stu-id="deecd-107">`IHostingStartup` *is available in ASP.NET Core 2.0 and later.*</span></span>

<span data-ttu-id="deecd-108">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/ihostingstartup/sample/) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="deecd-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/ihostingstartup/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="deecd-109">Ermitteln der geladenen Startassemblys für hosting</span><span class="sxs-lookup"><span data-stu-id="deecd-109">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="deecd-110">Um zu ermitteln, ob von der app oder von Bibliotheken hosting Startassemblys geladen werden, aktivieren Sie die Protokollierung, und überprüfen Sie die Anwendungsprotokolle.</span><span class="sxs-lookup"><span data-stu-id="deecd-110">To discover hosting startup assemblies loaded by the app or by libraries, enable logging and check the application logs.</span></span> <span data-ttu-id="deecd-111">Fehler beim Laden von Assemblys werden protokolliert.</span><span class="sxs-lookup"><span data-stu-id="deecd-111">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="deecd-112">Geladene hosting Autostart-Assemblys werden auf der Debug-Ebene protokolliert, und alle Fehler werden protokolliert.</span><span class="sxs-lookup"><span data-stu-id="deecd-112">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

<span data-ttu-id="deecd-113">Die Beispiel-app lautet der der [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) in einer `string` array erstellt und das Ergebnis wird in der app-Indexseite angezeigt:</span><span class="sxs-lookup"><span data-stu-id="deecd-113">The sample app reads the the [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) into a `string` array and displays the result in the app's Index page:</span></span>

[!code-csharp[Main](ihostingstartup/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="deecd-114">Deaktivieren Sie automatische Laden von hosting Autostart-Assemblys</span><span class="sxs-lookup"><span data-stu-id="deecd-114">Disable automatic loading of hosting startup assemblies</span></span>

<span data-ttu-id="deecd-115">Es gibt zwei Möglichkeiten, um das automatische Laden von hosting Startassemblys zu deaktivieren:</span><span class="sxs-lookup"><span data-stu-id="deecd-115">There are two ways to disable the automatic loading of hosting startup assemblies:</span></span>

* <span data-ttu-id="deecd-116">Legen Sie die [Hosting-Start zu verhindern, dass](xref:fundamentals/hosting#prevent-hosting-startup) Konfigurationseinstellung hosten.</span><span class="sxs-lookup"><span data-stu-id="deecd-116">Set the [Prevent Hosting Startup](xref:fundamentals/hosting#prevent-hosting-startup) host configuration setting.</span></span>
* <span data-ttu-id="deecd-117">Legen Sie die `ASPNETCORE_preventHostingStartup` -Umgebungsvariablen angegeben.</span><span class="sxs-lookup"><span data-stu-id="deecd-117">Set the `ASPNETCORE_preventHostingStartup` environment variable.</span></span>

<span data-ttu-id="deecd-118">Wenn die Host-Einstellung oder die Umgebungsvariable auf festgelegt ist `true` oder `1`, hosting Autostart-Assemblys werden nicht automatisch geladen.</span><span class="sxs-lookup"><span data-stu-id="deecd-118">When either the host setting or the environment variable is set to `true` or `1`, hosting startup assemblies aren't automatically loaded.</span></span> <span data-ttu-id="deecd-119">Wenn beide festgelegt sind, steuert die hosteinstellung das Verhalten an.</span><span class="sxs-lookup"><span data-stu-id="deecd-119">If both are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="deecd-120">Deaktivieren hosting Startassemblys, die mit der Einstellung oder Umgebung Hostvariable sie global deaktiviert und kann mehrere Funktionen eine app zu deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="deecd-120">Disabling hosting startup assemblies using the host setting or environment variable disables them globally and may disable several features of an app.</span></span> <span data-ttu-id="deecd-121">Es ist nicht aktuell möglich, einem hosting Startassembly, die von einer Bibliothek hinzugefügt werden, es sei denn, die Bibliothek einen eigenen Konfigurationsoption bietet selektiv zu deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="deecd-121">It isn't currently possible to selectively disable a hosting startup assembly added by a library unless the library offers its own configuration option.</span></span> <span data-ttu-id="deecd-122">Eine zukünftige Version wird bieten die Möglichkeit, hosting Startassemblys selektiv zu deaktivieren (siehe [GitHub ausstellen Aspnet/Hosting-1243](https://github.com/aspnet/Hosting/pull/1243)).</span><span class="sxs-lookup"><span data-stu-id="deecd-122">A future release will offer the ability to selectively disable hosting startup assemblies (see [GitHub issue aspnet/Hosting #1243](https://github.com/aspnet/Hosting/pull/1243)).</span></span>

## <a name="implement-ihostingstartup-features"></a><span data-ttu-id="deecd-123">Implementieren Sie IHostingStartup-Funktionen</span><span class="sxs-lookup"><span data-stu-id="deecd-123">Implement IHostingStartup features</span></span>

### <a name="create-the-assembly"></a><span data-ttu-id="deecd-124">Erstellen Sie die assembly</span><span class="sxs-lookup"><span data-stu-id="deecd-124">Create the assembly</span></span>

<span data-ttu-id="deecd-125">Ein `IHostingStartup` Funktion wird als eine Assembly basierend auf einer Konsolen-app ohne einen Einstiegspunkt bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="deecd-125">An `IHostingStartup` feature is deployed as an assembly based on a console app without an entry point.</span></span> <span data-ttu-id="deecd-126">Die Assemblyverweise der [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) Paket:</span><span class="sxs-lookup"><span data-stu-id="deecd-126">The assembly references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[Main](ihostingstartup/snapshot_sample/StartupFeature.csproj)]

<span data-ttu-id="deecd-127">Ein [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) Attribut identifiziert eine Klasse als eine Implementierung der `IHostingStartup` für das Laden und ausführen, die beim Erstellen der [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span><span class="sxs-lookup"><span data-stu-id="deecd-127">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span></span> <span data-ttu-id="deecd-128">Im folgenden Beispiel wird der Namespace `StartupFeature`, und die Klasse ist `StartupFeatureHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="deecd-128">In the following example, the namespace is `StartupFeature`, and the class is `StartupFeatureHostingStartup`:</span></span>

[!code-csharp[Main](ihostingstartup/snapshot_sample/StartupFeature.cs?name=snippet1)]

<span data-ttu-id="deecd-129">Eine Klasse implementiert `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="deecd-129">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="deecd-130">Der Klasse [konfigurieren](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) -Methode verwendet ein [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) Funktionen eine app hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="deecd-130">The class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add features to an app:</span></span>

[!code-csharp[Main](ihostingstartup/snapshot_sample/StartupFeature.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="deecd-131">Beim Erstellen eine `IHostingStartup` -Projekt die Abhängigkeitsdatei (*\*. deps.json*) legt die `runtime` Speicherort der Assembly, die die *"bin"* Ordner:</span><span class="sxs-lookup"><span data-stu-id="deecd-131">When building an `IHostingStartup` project, the dependencies file (*\*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[Main](ihostingstartup/snapshot_sample/StartupFeature1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="deecd-132">Es wird nur ein Teil der Datei angezeigt.</span><span class="sxs-lookup"><span data-stu-id="deecd-132">Only part of the file is shown.</span></span> <span data-ttu-id="deecd-133">Der Name der Assembly im Beispiel wird `StartupFeature`.</span><span class="sxs-lookup"><span data-stu-id="deecd-133">The assembly name in the example is `StartupFeature`.</span></span>

### <a name="update-the-dependencies-file"></a><span data-ttu-id="deecd-134">Aktualisieren Sie die Abhängigkeitsdatei</span><span class="sxs-lookup"><span data-stu-id="deecd-134">Update the dependencies file</span></span>

<span data-ttu-id="deecd-135">Die Common Language Runtime-Speicherort angegeben, der  *\*. deps.json* Datei.</span><span class="sxs-lookup"><span data-stu-id="deecd-135">The runtime location is specified in the *\*.deps.json* file.</span></span> <span data-ttu-id="deecd-136">Aktiviert die Funktion der `runtime` Element muss den Speicherort des Features Common Language Runtime-Assembly angeben.</span><span class="sxs-lookup"><span data-stu-id="deecd-136">To active the feature, the `runtime` element must specify the location of the feature's runtime assembly.</span></span> <span data-ttu-id="deecd-137">Präfix der `runtime` Speicherort mit `lib/netcoreapp2.0/`:</span><span class="sxs-lookup"><span data-stu-id="deecd-137">Prefix the `runtime` location with `lib/netcoreapp2.0/`:</span></span>

[!code-json[Main](ihostingstartup/snapshot_sample/StartupFeature2.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="deecd-138">In der Beispiel-app, Änderung von der  *\*. deps.json* Datei erfolgt durch eine [PowerShell](/powershell/scripting/powershell-scripting) Skript.</span><span class="sxs-lookup"><span data-stu-id="deecd-138">In the sample app, modification of the *\*.deps.json* file is performed by a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span> <span data-ttu-id="deecd-139">Das PowerShell-Skript wird automatisch durch einen Build-Ziel in der Projektdatei ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="deecd-139">The PowerShell script is automatically triggered by a build target in the project file.</span></span>

### <a name="feature-activation"></a><span data-ttu-id="deecd-140">Die Aktivierung der Funktion</span><span class="sxs-lookup"><span data-stu-id="deecd-140">Feature activation</span></span>

<span data-ttu-id="deecd-141">**Legen Sie die Assemblydatei**</span><span class="sxs-lookup"><span data-stu-id="deecd-141">**Place the assembly file**</span></span>

<span data-ttu-id="deecd-142">Die `IHostingStartup` der Implementierung Assemblydatei muss *"bin"*-in die app bereitgestellt oder in der [Common Language Runtime Speicher](/dotnet/core/deploying/runtime-store):</span><span class="sxs-lookup"><span data-stu-id="deecd-142">The `IHostingStartup` implementation's assembly file must be *bin*-deployed in the app or placed in the [runtime store](/dotnet/core/deploying/runtime-store):</span></span>

<span data-ttu-id="deecd-143">Platzieren Sie für pro-Benutzer verwenden die Assembly in das Benutzerprofil Common Language Runtime Speicher an:</span><span class="sxs-lookup"><span data-stu-id="deecd-143">For per-user use, place the assembly in the user profile's runtime store at:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\netcoreapp2.0\<FEATURE_ASSEMBLY_NAME>\<FEATURE_VERSION>\lib\netcoreapp2.0\
```

<span data-ttu-id="deecd-144">Platzieren Sie die Assembly in der .NET Core-Installation Common Language Runtime Speicher, für die globale Verwendung:</span><span class="sxs-lookup"><span data-stu-id="deecd-144">For global use, place the assembly in the .NET Core installation's runtime store:</span></span>

```
<DRIVE>\Program Files\dotnet\store\x64\netcoreapp2.0\<FEATURE_ASSEMBLY_NAME>\<FEATURE_VERSION>\lib\netcoreapp2.0\
```

<span data-ttu-id="deecd-145">Beim Bereitstellen der Assemblys auf die Common Language Runtime Speicher wird die Symboldatei möglicherweise ebenfalls bereitgestellt werden, jedoch nicht erforderlich, damit die Funktion ordnungsgemäß arbeitet.</span><span class="sxs-lookup"><span data-stu-id="deecd-145">When deploying the assembly to the runtime store, the symbols file may be deployed as well but isn't required for the feature to work.</span></span>

<span data-ttu-id="deecd-146">**Legen Sie die Abhängigkeitsdatei**</span><span class="sxs-lookup"><span data-stu-id="deecd-146">**Place the dependencies file**</span></span>

<span data-ttu-id="deecd-147">Der Implementierung  *\*. deps.json* Datei muss unter einem zugänglichen Pfad sein.</span><span class="sxs-lookup"><span data-stu-id="deecd-147">The implementation's *\*.deps.json* file must be in an accessible location.</span></span>

<span data-ttu-id="deecd-148">Legen Sie für die Verwendung von pro Benutzer, die Datei in die `additonalDeps` Ordner, der des Benutzerprofils `.dotnet` Einstellungen:</span><span class="sxs-lookup"><span data-stu-id="deecd-148">For per-user use, place the file in the `additonalDeps` folder of the user profile's `.dotnet` settings:</span></span> 

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

<span data-ttu-id="deecd-149">Für globale verwenden, legen Sie die Datei in die `additonalDeps` Ordner der .NET Core-Installation:</span><span class="sxs-lookup"><span data-stu-id="deecd-149">For global use, place the file in the `additonalDeps` folder of the .NET Core installation:</span></span>

```
<DRIVE>\Program Files\dotnet\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

<span data-ttu-id="deecd-150">Beachten Sie die Version `2.0.0`, gibt die Version der freigegebenen Runtime, die die Ziel-app verwendet.</span><span class="sxs-lookup"><span data-stu-id="deecd-150">Note the version, `2.0.0`, reflects the version of the shared runtime that the target app uses.</span></span> <span data-ttu-id="deecd-151">Die freigegebene Runtime wird angezeigt, der  *\*. runtimeconfig.json* Datei.</span><span class="sxs-lookup"><span data-stu-id="deecd-151">The shared runtime is shown in the *\*.runtimeconfig.json* file.</span></span> <span data-ttu-id="deecd-152">In der Beispiel-app-freigegebene Runtime wird angegeben, der *HostingStartupSample.runtimeconfig.json* Datei.</span><span class="sxs-lookup"><span data-stu-id="deecd-152">In the sample app, the shared runtime is specified in the *HostingStartupSample.runtimeconfig.json* file.</span></span>

<span data-ttu-id="deecd-153">**Umgebungsvariablen festlegen**</span><span class="sxs-lookup"><span data-stu-id="deecd-153">**Set environment variables**</span></span>

<span data-ttu-id="deecd-154">Legen Sie die folgenden Umgebungsvariablen im Kontext der app, die die Funktion verwendet.</span><span class="sxs-lookup"><span data-stu-id="deecd-154">Set the following environment variables in the context of the app that uses the feature.</span></span>

<span data-ttu-id="deecd-155">ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES</span><span class="sxs-lookup"><span data-stu-id="deecd-155">ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES</span></span>

<span data-ttu-id="deecd-156">Nur hosting Autostart-Assemblys werden gescannt, für die `HostingStartupAttribute`.</span><span class="sxs-lookup"><span data-stu-id="deecd-156">Only hosting startup assemblies are scanned for the `HostingStartupAttribute`.</span></span> <span data-ttu-id="deecd-157">Der Assemblyname der Implementierung wird in dieser Umgebungsvariable bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="deecd-157">The assembly name of the implementation is provided in this environment variable.</span></span> <span data-ttu-id="deecd-158">Die Beispiel-app wird dieser Wert auf `StartupDiagnostics`.</span><span class="sxs-lookup"><span data-stu-id="deecd-158">The sample app sets this value to `StartupDiagnostics`.</span></span>

<span data-ttu-id="deecd-159">Der Wert kann auch festgelegt werden mithilfe der [Hosting Startassemblys](xref:fundamentals/hosting#hosting-startup-assemblies) Konfigurationseinstellung hosten.</span><span class="sxs-lookup"><span data-stu-id="deecd-159">The value can also be set using the [Hosting Startup Assemblies](xref:fundamentals/hosting#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="deecd-160">DOTNET\_ZUSÄTZLICHE\_EINZAHLGN</span><span class="sxs-lookup"><span data-stu-id="deecd-160">DOTNET\_ADDITIONAL\_DEPS</span></span>

<span data-ttu-id="deecd-161">Der Speicherort für der Implementierung  *\*. deps.json* Datei.</span><span class="sxs-lookup"><span data-stu-id="deecd-161">The location of the implementation's *\*.deps.json* file.</span></span>

<span data-ttu-id="deecd-162">Wenn die Datei in des Benutzerprofils befindet *.dotnet* Ordners zur Verwendung von pro Benutzer:</span><span class="sxs-lookup"><span data-stu-id="deecd-162">If the file is placed in the user profile's *.dotnet* folder for per-user use:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\
```

<span data-ttu-id="deecd-163">Wenn die Datei in der .NET Core-Installation für die Verwendung von globalen platziert wird, geben Sie den vollständigen Pfad zur Datei:</span><span class="sxs-lookup"><span data-stu-id="deecd-163">If the file is placed in the .NET Core installation for global use, provide the full path to the file:</span></span>

```
<DRIVE>\Program Files\dotnet\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\<FEATURE_ASSEMBLY_NAME>.deps.json
```

<span data-ttu-id="deecd-164">Die Beispiel-app wird dieser Wert auf:</span><span class="sxs-lookup"><span data-stu-id="deecd-164">The sample app sets this value to:</span></span>

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

<span data-ttu-id="deecd-165">Beispiele zum Festlegen von Umgebungsvariablen für verschiedene Betriebssysteme finden Sie in [arbeiten mit mehreren Umgebungen](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="deecd-165">For examples of how to set environment variables for various operating systems, see [Working with multiple environments](xref:fundamentals/environments).</span></span>

## <a name="sample-app"></a><span data-ttu-id="deecd-166">Beispiel-app</span><span class="sxs-lookup"><span data-stu-id="deecd-166">Sample app</span></span>

<span data-ttu-id="deecd-167">Die [Beispielapp](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/ihostingstartup/sample/) ([zum Herunterladen von](xref:tutorials/index#how-to-download-a-sample)) verwendet `IHostingStartup` So erstellen ein Diagnosetool.</span><span class="sxs-lookup"><span data-stu-id="deecd-167">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/ihostingstartup/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)) uses `IHostingStartup` to create a diagnostics tool.</span></span> <span data-ttu-id="deecd-168">Das Tool hinzugefügt die app starten, die Diagnoseinformationen bieten zwei Middlewares:</span><span class="sxs-lookup"><span data-stu-id="deecd-168">The tool adds two middlewares to the app at startup that provide diagnostic information:</span></span>

* <span data-ttu-id="deecd-169">Registrierte Dienste</span><span class="sxs-lookup"><span data-stu-id="deecd-169">Registered services</span></span>
* <span data-ttu-id="deecd-170">Adresse: Schema, Host, Pfad Basis, Pfad, einer Abfragezeichenfolge</span><span class="sxs-lookup"><span data-stu-id="deecd-170">Address: scheme, host, path base, path, query string</span></span>
* <span data-ttu-id="deecd-171">Verbindung: remote-IP-, der Remoteport, lokale IP-Adresse Lokaler Port, Clientzertifikat</span><span class="sxs-lookup"><span data-stu-id="deecd-171">Connection: remote IP, remote port, local IP, local port, client certificate</span></span>
* <span data-ttu-id="deecd-172">Anforderungsheader</span><span class="sxs-lookup"><span data-stu-id="deecd-172">Request headers</span></span>
* <span data-ttu-id="deecd-173">Umgebungsvariablen</span><span class="sxs-lookup"><span data-stu-id="deecd-173">Environment variables</span></span>

<span data-ttu-id="deecd-174">Um das Beispiel auszuführen:</span><span class="sxs-lookup"><span data-stu-id="deecd-174">To run the sample:</span></span>

1. <span data-ttu-id="deecd-175">Das Start-Diagnose Projekt verwendet [PowerShell](/powershell/scripting/powershell-scripting) so ändern Sie die *StartupDiagnostics.deps.json* Datei.</span><span class="sxs-lookup"><span data-stu-id="deecd-175">The Startup Diagnostic project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="deecd-176">PowerShell ist standardmäßig auf Windows-Betriebssystemen ab Windows 7 SP1 und Windows Server 2008 R2 SP1 installiert.</span><span class="sxs-lookup"><span data-stu-id="deecd-176">PowerShell is installed by default on Windows OS starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="deecd-177">Um PowerShell auf anderen Plattformen zu erhalten, finden Sie unter [Installieren von Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).</span><span class="sxs-lookup"><span data-stu-id="deecd-177">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).</span></span>
2. <span data-ttu-id="deecd-178">Buildprojekt Diagnose starten.</span><span class="sxs-lookup"><span data-stu-id="deecd-178">Build the Startup Diagnostic project.</span></span> <span data-ttu-id="deecd-179">Ein Build-Ziel in der Projektdatei:</span><span class="sxs-lookup"><span data-stu-id="deecd-179">A build target in the project file:</span></span>
   * <span data-ttu-id="deecd-180">Verschiebt die Assembly und Symboldateien das Benutzerprofil Common Language Runtime Speicher.</span><span class="sxs-lookup"><span data-stu-id="deecd-180">Moves the assembly and symbols files to the user profile's runtime store.</span></span>
   * <span data-ttu-id="deecd-181">Löst das PowerShell-Skript zum Ändern der *StartupDiagnostics.deps.json* Datei.</span><span class="sxs-lookup"><span data-stu-id="deecd-181">Triggers the PowerShell script to modify the *StartupDiagnostics.deps.json* file.</span></span>
   * <span data-ttu-id="deecd-182">Verschiebt die *StartupDiagnostics.deps.json* Datei am Benutzerprofil `additionalDeps` Ordner.</span><span class="sxs-lookup"><span data-stu-id="deecd-182">Moves the *StartupDiagnostics.deps.json* file to the user profile's `additionalDeps` folder.</span></span>
3. <span data-ttu-id="deecd-183">Festlegen der Umgebungsvariablen:</span><span class="sxs-lookup"><span data-stu-id="deecd-183">Set the environment variables:</span></span>
    * <span data-ttu-id="deecd-184">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`</span><span class="sxs-lookup"><span data-stu-id="deecd-184">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`</span></span>
    * <span data-ttu-id="deecd-185">`DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`</span><span class="sxs-lookup"><span data-stu-id="deecd-185">`DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`</span></span>
4. <span data-ttu-id="deecd-186">Führen Sie die Beispiel-app.</span><span class="sxs-lookup"><span data-stu-id="deecd-186">Run the sample app.</span></span>
5. <span data-ttu-id="deecd-187">Anfordern der `/services` finden in der app-Endpunkt registriert Dienste.</span><span class="sxs-lookup"><span data-stu-id="deecd-187">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="deecd-188">Anfordern der `/diag` Endpunkt, um die Diagnoseinformationen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="deecd-188">Request the `/diag` endpoint to see the diagnostic information.</span></span>
