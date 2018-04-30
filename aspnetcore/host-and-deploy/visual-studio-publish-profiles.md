---
title: Visual Studio die Veröffentlichungsprofile für ASP.NET Core-app-Bereitstellung
author: rick-anderson
description: Informationen zum Erstellen in Visual Studio die Veröffentlichungsprofile und verwenden sie zum Verwalten von ASP.NET Core-app-Bereitstellungen an verschiedene Ziele.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 3dc858793cd4ddb2630d05a5084f4b7caeaa30eb
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2018
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="d0a32-103">Visual Studio die Veröffentlichungsprofile für ASP.NET Core-app-Bereitstellung</span><span class="sxs-lookup"><span data-stu-id="d0a32-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="d0a32-104">Von [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d0a32-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d0a32-105">Dieses Dokument konzentriert sich auf das Erstellen und Verwenden von Visual Studio 2017 mit Veröffentlichungsprofile.</span><span class="sxs-lookup"><span data-stu-id="d0a32-105">This document focuses on using Visual Studio 2017 to create and use publish profiles.</span></span> <span data-ttu-id="d0a32-106">Die Veröffentlichungsprofile, die mit Visual Studio erstellt werden, können von MSBuild und Visual Studio 2017 ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="d0a32-106">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio 2017.</span></span> <span data-ttu-id="d0a32-107">Anweisungen zum Veröffentlichen in Azure finden Sie unter [Veröffentlichen einer ASP.NET Core-Web-App in Azure App Service mit Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs).</span><span class="sxs-lookup"><span data-stu-id="d0a32-107">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="d0a32-108">Die folgende Projektdatei erstellt wurde, mit dem Befehl `dotnet new mvc`:</span><span class="sxs-lookup"><span data-stu-id="d0a32-108">The following project file was created with the command `dotnet new mvc`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d0a32-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d0a32-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
  </ItemGroup>

  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.0.0" />
  </ItemGroup>

</Project>
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d0a32-110">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d0a32-110">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.5" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.6" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.3" />
  </ItemGroup>

</Project>
```

---

<span data-ttu-id="d0a32-111">Die `<Project>` des Elements `Sdk` Attribut führt die folgenden Aufgaben:</span><span class="sxs-lookup"><span data-stu-id="d0a32-111">The `<Project>` element's `Sdk` attribute accomplishes the following tasks:</span></span>

* <span data-ttu-id="d0a32-112">Importiert die Eigenschaftendatei aus *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* am Anfang.</span><span class="sxs-lookup"><span data-stu-id="d0a32-112">Imports the properties file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="d0a32-113">Es importiert am Ende die Zieledatei von *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets*.</span><span class="sxs-lookup"><span data-stu-id="d0a32-113">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="d0a32-114">Der Standardspeicherort für `MSBuildSDKsPath` (mit Visual Studio 2017 Enterprise) ist der Ordner *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks*.</span><span class="sxs-lookup"><span data-stu-id="d0a32-114">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="d0a32-115">Die `Microsoft.NET.Sdk.Web` SDK abhängig:</span><span class="sxs-lookup"><span data-stu-id="d0a32-115">The `Microsoft.NET.Sdk.Web` SDK depends on:</span></span>

* <span data-ttu-id="d0a32-116">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="d0a32-116">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="d0a32-117">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="d0a32-117">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="d0a32-118">Dadurch werden die folgenden Eigenschaften und Ziele, die importiert werden:</span><span class="sxs-lookup"><span data-stu-id="d0a32-118">Which causes the following properties and targets to be imported:</span></span>

* <span data-ttu-id="d0a32-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="d0a32-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="d0a32-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="d0a32-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span></span>
* <span data-ttu-id="d0a32-121">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="d0a32-121">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="d0a32-122">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="d0a32-122">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span></span>

<span data-ttu-id="d0a32-123">Veröffentlichen Sie Targets-Import, legen Sie rechts von Zielen, die auf der Grundlage der Publish-Methode verwendet.</span><span class="sxs-lookup"><span data-stu-id="d0a32-123">Publish targets import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="d0a32-124">Wenn MSBuild oder Visual Studio ein Projekt geladen wird, werden folgende allgemeinen Aktionen ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="d0a32-124">When MSBuild or Visual Studio loads a project, the following high-level actions occur:</span></span>

* <span data-ttu-id="d0a32-125">Erstellen des Projekts</span><span class="sxs-lookup"><span data-stu-id="d0a32-125">Build project</span></span>
* <span data-ttu-id="d0a32-126">Berechnen der zu veröffentlichenden Dateien</span><span class="sxs-lookup"><span data-stu-id="d0a32-126">Compute files to publish</span></span>
* <span data-ttu-id="d0a32-127">Veröffentlichen der Dateien auf dem Ziel</span><span class="sxs-lookup"><span data-stu-id="d0a32-127">Publish files to destination</span></span>

## <a name="compute-project-items"></a><span data-ttu-id="d0a32-128">Berechnen der Projektelemente</span><span class="sxs-lookup"><span data-stu-id="d0a32-128">Compute project items</span></span>

<span data-ttu-id="d0a32-129">Wenn das Projekt geladen ist, werden die Projektelemente (Dateien) berechnet.</span><span class="sxs-lookup"><span data-stu-id="d0a32-129">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="d0a32-130">Die `item type`-Attribut bestimmt, wie die Datei verarbeitet wird.</span><span class="sxs-lookup"><span data-stu-id="d0a32-130">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="d0a32-131">Standardmäßig sind *CS*-Dateien in der `Compile`-Elementliste enthalten.</span><span class="sxs-lookup"><span data-stu-id="d0a32-131">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="d0a32-132">Dateien in der `Compile`-Elementliste werden kompiliert.</span><span class="sxs-lookup"><span data-stu-id="d0a32-132">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="d0a32-133">Die `Content` Elementliste enthält Dateien, die zusätzlich zu den Buildausgaben veröffentlicht werden.</span><span class="sxs-lookup"><span data-stu-id="d0a32-133">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="d0a32-134">Standardmäßig Dateien gemäß dem Muster `wwwroot/**` befinden sich die `Content` Element.</span><span class="sxs-lookup"><span data-stu-id="d0a32-134">By default, files matching the pattern `wwwroot/**` are included in the `Content` item.</span></span> <span data-ttu-id="d0a32-135">Die `wwwroot/\*\*` [Globmodus Muster](https://gruntjs.com/configuring-tasks#globbing-patterns) entspricht allen Dateien in der *"Wwwroot"* Ordner **und** Unterordner.</span><span class="sxs-lookup"><span data-stu-id="d0a32-135">The `wwwroot/\*\*` [globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) matches all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="d0a32-136">Um die Veröffentlichungsliste explizit eine Datei hinzuzufügen, fügen Sie die Datei direkt in die *csproj* Datei wie gezeigt in [Includedateien](#include-files).</span><span class="sxs-lookup"><span data-stu-id="d0a32-136">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Include Files](#include-files).</span></span>

<span data-ttu-id="d0a32-137">Bei der Auswahl der **veröffentlichen** Schaltfläche in Visual Studio oder beim Veröffentlichen von der Befehlszeile aus:</span><span class="sxs-lookup"><span data-stu-id="d0a32-137">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="d0a32-138">Die Eigenschaften und Elemente werden berechnet (die Dateien, die für den Build benötigt werden).</span><span class="sxs-lookup"><span data-stu-id="d0a32-138">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="d0a32-139">**Visual Studio nur**: NuGet-Pakete werden wiederhergestellt.</span><span class="sxs-lookup"><span data-stu-id="d0a32-139">**Visual Studio only**: NuGet packages are restored.</span></span> <span data-ttu-id="d0a32-140">(Die Wiederherstellung muss explizit vom Benutzer auf der CLI durchgeführt werden.)</span><span class="sxs-lookup"><span data-stu-id="d0a32-140">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="d0a32-141">Das Projekt wird erstellt.</span><span class="sxs-lookup"><span data-stu-id="d0a32-141">The project builds.</span></span>
* <span data-ttu-id="d0a32-142">Die zu veröffentlichenden Elemente werden berechnet (die Dateien, die für die Veröffentlichung benötigt werden).</span><span class="sxs-lookup"><span data-stu-id="d0a32-142">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="d0a32-143">Das Projekt veröffentlicht wird (die berechnete Dateien werden in der Veröffentlichung kopiert).</span><span class="sxs-lookup"><span data-stu-id="d0a32-143">The project is published (the computed files are copied to the publish destination).</span></span>

<span data-ttu-id="d0a32-144">Wenn eine ASP.NET Core-Projekt verweist auf `Microsoft.NET.Sdk.Web` in der Projektdatei ein *app_offline.htm* Datei befindet sich im Stammverzeichnis des Verzeichnisses der Web-app.</span><span class="sxs-lookup"><span data-stu-id="d0a32-144">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="d0a32-145">Wenn die Datei vorhanden ist, fährt das ASP.NET Core Module die App ordnungsgemäß herunter und verarbeitet die Datei *app_offline.htm* während der Bereitstellung.</span><span class="sxs-lookup"><span data-stu-id="d0a32-145">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="d0a32-146">Weitere Informationen finden Sie unter [Konfigurationsreferenz für das ASP.NET Core-Modul](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span><span class="sxs-lookup"><span data-stu-id="d0a32-146">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="d0a32-147">Grundlegende Befehlszeile veröffentlichen</span><span class="sxs-lookup"><span data-stu-id="d0a32-147">Basic command-line publishing</span></span>

<span data-ttu-id="d0a32-148">Befehlszeilen-Veröffentlichung funktioniert auf allen Plattformen mit .NET Core-Unterstützung und erfordert nicht das Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d0a32-148">Command-line publishing works on all .NET Core-supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="d0a32-149">In den Beispielen unten die [Dotnet veröffentlichen](/dotnet/core/tools/dotnet-publish) aus dem Projektverzeichnis ausgeführt wird (enthält die *csproj* Datei).</span><span class="sxs-lookup"><span data-stu-id="d0a32-149">In the samples below, the [dotnet publish](/dotnet/core/tools/dotnet-publish) command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="d0a32-150">Wenn dies nicht explizit in den Projektordner übergeben, in der Projektdateipfad.</span><span class="sxs-lookup"><span data-stu-id="d0a32-150">If not in the project folder, explicitly pass in the project file path.</span></span> <span data-ttu-id="d0a32-151">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="d0a32-151">For example:</span></span>

```console
dotnet publish C:\Webs\Web1
```

<span data-ttu-id="d0a32-152">Führen Sie die folgenden Befehle zum Erstellen und Veröffentlichen eine Web-App aus:</span><span class="sxs-lookup"><span data-stu-id="d0a32-152">Run the following commands to create and publish a web app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d0a32-153">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d0a32-153">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d0a32-154">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d0a32-154">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

---

<span data-ttu-id="d0a32-155">Die [Dotnet veröffentlichen](/dotnet/core/tools/dotnet-publish) Befehl erzeugt eine Ausgabe ähnlich der folgenden:</span><span class="sxs-lookup"><span data-stu-id="d0a32-155">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="d0a32-156">Der Standardveröffentlichungsordner ist `bin\$(Configuration)\netcoreapp<version>\publish`.</span><span class="sxs-lookup"><span data-stu-id="d0a32-156">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="d0a32-157">Die Standardeinstellung für `$(Configuration)` ist *Debuggen*.</span><span class="sxs-lookup"><span data-stu-id="d0a32-157">The default for `$(Configuration)` is *Debug*.</span></span> <span data-ttu-id="d0a32-158">Im vorhergehenden Beispiel die `<TargetFramework>` ist `netcoreapp2.0`.</span><span class="sxs-lookup"><span data-stu-id="d0a32-158">In the preceding sample, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="d0a32-159">`dotnet publish -h` zeigt Hilfeinformationen zum Veröffentlichen an.</span><span class="sxs-lookup"><span data-stu-id="d0a32-159">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="d0a32-160">Der folgende Befehl gibt einen `Release`-Build und das Veröffentlichungsverzeichnis an:</span><span class="sxs-lookup"><span data-stu-id="d0a32-160">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:\MyWebs\test
```

<span data-ttu-id="d0a32-161">Die [Dotnet veröffentlichen](/dotnet/core/tools/dotnet-publish) Befehl ruft MSBuild das aufruft der `Publish` Ziel.</span><span class="sxs-lookup"><span data-stu-id="d0a32-161">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command calls MSBuild, which invokes the `Publish` target.</span></span> <span data-ttu-id="d0a32-162">Übergeben von Parametern `dotnet publish` an MSBuild übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="d0a32-162">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="d0a32-163">Der Parameter `-c` wird der MSBuild-Eigenschaft `Configuration` zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="d0a32-163">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="d0a32-164">Der Parameter `-o` wird `OutputPath` zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="d0a32-164">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="d0a32-165">MSBuild-Eigenschaften können mithilfe einer der folgenden Formate übergeben werden:</span><span class="sxs-lookup"><span data-stu-id="d0a32-165">MSBuild properties can be passed using either of the following formats:</span></span>

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="d0a32-166">Der folgende Befehl veröffentlicht einen `Release`-Build auf einer Netzwerkfreigabe:</span><span class="sxs-lookup"><span data-stu-id="d0a32-166">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="d0a32-167">Die Netzwerkfreigabe wird durch führende Schrägstriche (*//r8/*) angegeben und funktioniert auf allen Plattformen, die von .NET Core unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="d0a32-167">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="d0a32-168">Versichern Sie sich, dass die veröffentlichte App für die Bereitstellung nicht ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="d0a32-168">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="d0a32-169">Die Dateien im Ordner *publish* sind gesperrt, wenn die App ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="d0a32-169">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="d0a32-170">Die Bereitstellung kann nicht erfolgen, da die gesperrten Dateien nicht kopiert werden können.</span><span class="sxs-lookup"><span data-stu-id="d0a32-170">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="d0a32-171">Veröffentlichungsprofile</span><span class="sxs-lookup"><span data-stu-id="d0a32-171">Publish profiles</span></span>

<span data-ttu-id="d0a32-172">Dieser Abschnitt verwendet Visual Studio 2017 ein Veröffentlichungsprofil zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d0a32-172">This section uses Visual Studio 2017 to create a publishing profile.</span></span> <span data-ttu-id="d0a32-173">Nach der Erstellung ist die Veröffentlichung aus Visual Studio oder der Befehlszeile zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="d0a32-173">Once created, publishing from Visual Studio or the command line is available.</span></span>

<span data-ttu-id="d0a32-174">Veröffentlichen von Profilen können während der Veröffentlichung vereinfachen, und eine beliebige Anzahl von Profilen kann vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="d0a32-174">Publish profiles can simplify the publishing process, and any number of profiles can exist.</span></span> <span data-ttu-id="d0a32-175">Erstellen eines Veröffentlichungsprofils in Visual Studio durch Auswahl einer der folgenden Pfade:</span><span class="sxs-lookup"><span data-stu-id="d0a32-175">Create a publish profile in Visual Studio by choosing one of the following paths:</span></span>

* <span data-ttu-id="d0a32-176">Mit der rechten Maustaste des Projekts im Projektmappen-Explorer, und wählen Sie **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="d0a32-176">Right-click the project in Solution Explorer and select **Publish**.</span></span>
* <span data-ttu-id="d0a32-177">Wählen Sie **veröffentlichen &lt;Project_name&gt;**  aus der **erstellen** Menü.</span><span class="sxs-lookup"><span data-stu-id="d0a32-177">Select **Publish &lt;project_name&gt;** from the **Build** menu.</span></span>

<span data-ttu-id="d0a32-178">Die **veröffentlichen** Registerkarte der Seite "app Kapazitäten" wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="d0a32-178">The **Publish** tab of the app capacities page is displayed.</span></span> <span data-ttu-id="d0a32-179">Wenn das Projekt ein Veröffentlichungsprofil fehlt, wird die folgende Seite angezeigt:</span><span class="sxs-lookup"><span data-stu-id="d0a32-179">If the project lacks a publish profile, the following page is displayed:</span></span>

![Die Registerkarte "Veröffentlichen" auf der Seite "app Kapazitäten"](visual-studio-publish-profiles/_static/az.png)

<span data-ttu-id="d0a32-181">Wenn **Ordner** ist ausgewählt, geben Sie einen Ordnerpfad ein, um die veröffentlichten Objekte zu speichern.</span><span class="sxs-lookup"><span data-stu-id="d0a32-181">When **Folder** is selected, specify a folder path to store the published assets.</span></span> <span data-ttu-id="d0a32-182">Der Standardordner ist *Bin\Release\PublishOutput*.</span><span class="sxs-lookup"><span data-stu-id="d0a32-182">The default folder is *bin\Release\PublishOutput*.</span></span> <span data-ttu-id="d0a32-183">Klicken Sie auf die **Profil erstellen** Schaltfläche, um den Vorgang abzuschließen.</span><span class="sxs-lookup"><span data-stu-id="d0a32-183">Click the **Create Profile** button to finish.</span></span>

<span data-ttu-id="d0a32-184">Sobald ein Veröffentlichungsprofil erstellt wurde, die **veröffentlichen** Registerkarte ändert.</span><span class="sxs-lookup"><span data-stu-id="d0a32-184">Once a publish profile is created, the **Publish** tab changes.</span></span> <span data-ttu-id="d0a32-185">Das neu erstellte Profil wird in einer Dropdownliste angezeigt.</span><span class="sxs-lookup"><span data-stu-id="d0a32-185">The newly created profile appears in a drop-down list.</span></span> <span data-ttu-id="d0a32-186">Klicken Sie auf **neues Profil erstellen** ein anderes neues Profil erstellen.</span><span class="sxs-lookup"><span data-stu-id="d0a32-186">Click **Create new profile** to create another new profile.</span></span>

![Die Registerkarte "Veröffentlichen" mit FolderProfile Seite "app Kapazitäten"](visual-studio-publish-profiles/_static/create_new.png)

<span data-ttu-id="d0a32-188">Der Webpublishing-Assistent unterstützt folgende Veröffentlichungsziele:</span><span class="sxs-lookup"><span data-stu-id="d0a32-188">The Publish wizard supports the following publish targets:</span></span>

* <span data-ttu-id="d0a32-189">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="d0a32-189">Azure App Service</span></span>
* <span data-ttu-id="d0a32-190">Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="d0a32-190">Azure Virtual Machines</span></span>
* <span data-ttu-id="d0a32-191">IIS, FTP usw. (für alle Webserver)</span><span class="sxs-lookup"><span data-stu-id="d0a32-191">IIS, FTP, etc. (for any web server)</span></span>
* <span data-ttu-id="d0a32-192">Ordner</span><span class="sxs-lookup"><span data-stu-id="d0a32-192">Folder</span></span>
* <span data-ttu-id="d0a32-193">Profil importieren</span><span class="sxs-lookup"><span data-stu-id="d0a32-193">Import Profile</span></span>

<span data-ttu-id="d0a32-194">Weitere Informationen finden Sie unter [welche Veröffentlichungsoptionen für mich geeignet sind](/visualstudio/ide/not-in-toc/web-publish-options).</span><span class="sxs-lookup"><span data-stu-id="d0a32-194">For more information, see [What publishing options are right for me](/visualstudio/ide/not-in-toc/web-publish-options).</span></span>

<span data-ttu-id="d0a32-195">Beim Erstellen eines Veröffentlichungsprofils mit Visual Studio eine *Eigenschaften/PublishProfiles/&lt;Profile_name&gt;pubxml* MSBuild-Datei wird erstellt.</span><span class="sxs-lookup"><span data-stu-id="d0a32-195">When creating a publish profile with Visual Studio, a *Properties/PublishProfiles/&lt;profile_name&gt;.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="d0a32-196">Die *pubxml* -Datei ist eine MSBuild-Datei und enthält Konfigurationseinstellungen zu veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="d0a32-196">The *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="d0a32-197">Diese Datei kann geändert werden, um den Build anpassen und Veröffentlichen von Prozess.</span><span class="sxs-lookup"><span data-stu-id="d0a32-197">This file can be changed to customize the build and publish process.</span></span> <span data-ttu-id="d0a32-198">Diese Datei wird vom Veröffentlichungsprozess gelesen.</span><span class="sxs-lookup"><span data-stu-id="d0a32-198">This file is read by the publishing process.</span></span> <span data-ttu-id="d0a32-199">`<LastUsedBuildConfiguration>` ist ein Sonderfall, da er eine globale Eigenschaft und sollte nicht in jeder Datei, die im Build importiert werden.</span><span class="sxs-lookup"><span data-stu-id="d0a32-199">`<LastUsedBuildConfiguration>` is special because it's a global property and shouldn't be in any file that's imported in the build.</span></span> <span data-ttu-id="d0a32-200">Finden Sie unter [MSBuild: Gewusst wie: festlegen die Konfigurationseigenschaft](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="d0a32-200">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more information.</span></span>

<span data-ttu-id="d0a32-201">Beim Veröffentlichen in einer Azure-Ziel der *pubxml* -Datei enthält Ihre Azure-Abonnement-ID.</span><span class="sxs-lookup"><span data-stu-id="d0a32-201">When publishing to an Azure target, the *.pubxml* file contains your Azure subscription identifier.</span></span> <span data-ttu-id="d0a32-202">Mit diesem Zieltyp wird diese Datei zur quellcodeverwaltung hinzufügen abgeraten.</span><span class="sxs-lookup"><span data-stu-id="d0a32-202">With that target type, adding this file to source control is discouraged.</span></span> <span data-ttu-id="d0a32-203">Beim Veröffentlichen auf einem nicht-Azure-Ziel ist sicher ist, überprüfen Sie der *pubxml* Datei.</span><span class="sxs-lookup"><span data-stu-id="d0a32-203">When publishing to a non-Azure target, it's safe to check in the *.pubxml* file.</span></span>

<span data-ttu-id="d0a32-204">Vertrauliche Informationen (z. B. das Kennwort für die Veröffentlichung) verschlüsselt wird, auf eine pro Benutzer/Computerebene zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="d0a32-204">Sensitive information (like the publish password) is encrypted on a per user/machine level.</span></span> <span data-ttu-id="d0a32-205">Befindet sich in der *Eigenschaften/PublishProfiles/&lt;Profile_name&gt;. pubxml.user* Datei.</span><span class="sxs-lookup"><span data-stu-id="d0a32-205">It's stored in the *Properties/PublishProfiles/&lt;profile_name&gt;.pubxml.user* file.</span></span> <span data-ttu-id="d0a32-206">Da diese Datei vertraulichen Informationen gespeichert werden kann, sollten nicht in die quellcodeverwaltung überprüft werden.</span><span class="sxs-lookup"><span data-stu-id="d0a32-206">Because this file can store sensitive information, it shouldn't be checked into source control.</span></span>

<span data-ttu-id="d0a32-207">Einen Überblick über die Vorgehensweise beim Veröffentlichen einer Web-app auf ASP.NET Core finden Sie unter [Host und Bereitstellen von](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="d0a32-207">For an overview of how to publish a web app on ASP.NET Core, see [Host and deploy](xref:host-and-deploy/index).</span></span> <span data-ttu-id="d0a32-208">Die MSBuild-Aufgaben und Ziele, die zum Veröffentlichen einer app ASP.NET Core werden Open Source-am https://github.com/aspnet/websdk.</span><span class="sxs-lookup"><span data-stu-id="d0a32-208">The MSBuild tasks and targets necessary to publish an ASP.NET Core app are open-source at https://github.com/aspnet/websdk.</span></span>

<span data-ttu-id="d0a32-209">`dotnet publish` können MSDeploy, Ordner und [Kudu](https://github.com/projectkudu/kudu/wiki) Veröffentlichungsprofile:</span><span class="sxs-lookup"><span data-stu-id="d0a32-209">`dotnet publish` can use folder, MSDeploy, and [Kudu](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>

<span data-ttu-id="d0a32-210">Ordner (funktioniert plattformübergreifende):</span><span class="sxs-lookup"><span data-stu-id="d0a32-210">Folder (works cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

<span data-ttu-id="d0a32-211">MSDeploy (derzeit dieser funktioniert nur unter Windows seit MSDeploy plattformübergreifende ist nicht):</span><span class="sxs-lookup"><span data-stu-id="d0a32-211">MSDeploy (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

<span data-ttu-id="d0a32-212">MSDeploy-Paket (derzeit dieser funktioniert nur unter Windows seit MSDeploy plattformübergreifende ist nicht):</span><span class="sxs-lookup"><span data-stu-id="d0a32-212">MSDeploy package (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

<span data-ttu-id="d0a32-213">In den vorherigen Beispielen **nicht** übergeben `deployonbuild` auf `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="d0a32-213">In the preceding samples, **don't** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="d0a32-214">Weitere Informationen finden Sie unter [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span><span class="sxs-lookup"><span data-stu-id="d0a32-214">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="d0a32-215">`dotnet publish` unterstützt die Kudu-APIs für die Veröffentlichung in Azure aus einer beliebigen Plattform.</span><span class="sxs-lookup"><span data-stu-id="d0a32-215">`dotnet publish` supports Kudu APIs to publish to Azure from any platform.</span></span> <span data-ttu-id="d0a32-216">Visual Studio veröffentlichen unterstützt die Kudu-APIs, sondern von WebSDK unterstützt wird, für die plattformübergreifende in Azure veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="d0a32-216">Visual Studio publish supports the Kudu APIs, but it's supported by WebSDK for cross-platform publish to Azure.</span></span>

<span data-ttu-id="d0a32-217">Hinzufügen ein Veröffentlichungsprofils, das *Eigenschaften/PublishProfiles* Ordner mit dem folgenden Inhalt:</span><span class="sxs-lookup"><span data-stu-id="d0a32-217">Add a publish profile to the *Properties/PublishProfiles* folder with the following content:</span></span>

```xml
<Project>
  <PropertyGroup>
    <PublishProtocol>Kudu</PublishProtocol>
    <PublishSiteName>nodewebapp</PublishSiteName>
    <UserName>username</UserName>
    <Password>password</Password>
  </PropertyGroup>
</Project>
```

<span data-ttu-id="d0a32-218">Führen Sie den folgenden Befehl zum Komprimieren der Inhalte veröffentlichen und mithilfe der Kudu-APIs in Azure zu veröffentlichen:</span><span class="sxs-lookup"><span data-stu-id="d0a32-218">Run the following command to zip up the publish contents and publish it to Azure using the Kudu APIs:</span></span>

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

<span data-ttu-id="d0a32-219">Legen Sie die folgenden MSBuild-Eigenschaften fest, wenn Sie ein Veröffentlichungsprofil verwenden:</span><span class="sxs-lookup"><span data-stu-id="d0a32-219">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

<span data-ttu-id="d0a32-220">Bei der Veröffentlichung mit einem Profil mit dem Namen *FolderProfile*, einen der folgenden Befehle können ausgeführt werden:</span><span class="sxs-lookup"><span data-stu-id="d0a32-220">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="d0a32-221">Beim Aufrufen von [Dotnet Build](/dotnet/core/tools/dotnet-build), ruft er `msbuild` führen Sie den Build, und Veröffentlichen von Prozess.</span><span class="sxs-lookup"><span data-stu-id="d0a32-221">When invoking [dotnet build](/dotnet/core/tools/dotnet-build), it calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="d0a32-222">Aufruf eines `dotnet build` oder `msbuild` entspricht, wenn in einem Ordner Profil übergeben.</span><span class="sxs-lookup"><span data-stu-id="d0a32-222">Calling either `dotnet build` or `msbuild` is equivalent when passing in a folder profile.</span></span> <span data-ttu-id="d0a32-223">Beim Aufrufen von MSBuild direkt unter Windows wird die .NET Framework-Version von MSBuild verwendet.</span><span class="sxs-lookup"><span data-stu-id="d0a32-223">When calling MSBuild directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="d0a32-224">Die Veröffentlichung mit MSDeploy ist aktuell auf Windows-Computer beschränkt.</span><span class="sxs-lookup"><span data-stu-id="d0a32-224">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="d0a32-225">Wenn Sie `dotnet build` von einem Profil ohne Ordner aufrufen, wird MSBuild aufgerufen. MSBuild verwendet MSDeploy für Profile ohne Ordner.</span><span class="sxs-lookup"><span data-stu-id="d0a32-225">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="d0a32-226">Wenn Sie `dotnet build` von einem Profil ohne Ordner aufrufen, wird MSBuild (mithilfe von MSDeploy) aufgerufen. Dies führt zu einem Fehler (auch beim Ausführen auf einer Windows-Plattform).</span><span class="sxs-lookup"><span data-stu-id="d0a32-226">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="d0a32-227">Rufen Sie MSBuild direkt auf, um mit einem Profil ohne Ordner zu veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="d0a32-227">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="d0a32-228">Der folgende Ordner „Veröffentlichungsprofil“ wurde mit Visual Studio erstellt und veröffentlicht in eine Netzwerkfreigabe.</span><span class="sxs-lookup"><span data-stu-id="d0a32-228">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework>netcoreapp1.1</PublishFramework>
    <ProjectGuid>c30c453c-312e-40c4-aec9-394a145dee0b</ProjectGuid>
    <publishUrl>\\r8\Release\AdminWeb</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
</Project>
```

<span data-ttu-id="d0a32-229">Beachten Sie, dass `<LastUsedBuildConfiguration>` auf `Release` festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="d0a32-229">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span> <span data-ttu-id="d0a32-230">Bei der Veröffentlichung von Visual Studio wird der `<LastUsedBuildConfiguration>`-Wert für die Konfigurationseigenschaft mithilfe des Werts festgelegt, der beim Starten des Veröffentlichungsprozesses vorliegt.</span><span class="sxs-lookup"><span data-stu-id="d0a32-230">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="d0a32-231">Die `<LastUsedBuildConfiguration>` Konfigurationseigenschaft Sonderregeln und sollte nicht in einer importierten MSBuild-Datei überschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="d0a32-231">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="d0a32-232">Diese Eigenschaft kann über die Befehlszeile überschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="d0a32-232">This property can be overridden from the command line.</span></span>

<span data-ttu-id="d0a32-233">Verwenden die .NET Core CLI:</span><span class="sxs-lookup"><span data-stu-id="d0a32-233">Using the .NET Core CLI:</span></span>

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

<span data-ttu-id="d0a32-234">Verwenden von MSBuild:</span><span class="sxs-lookup"><span data-stu-id="d0a32-234">Using MSBuild:</span></span>

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="d0a32-235">Veröffentlichen auf einen MSDeploy-Endpunkt von der Befehlszeile</span><span class="sxs-lookup"><span data-stu-id="d0a32-235">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="d0a32-236">Mithilfe der .NET Core CLI oder MSBuild kann die Publishing erreicht werden.</span><span class="sxs-lookup"><span data-stu-id="d0a32-236">Publishing can be accomplished using the .NET Core CLI or MSBuild.</span></span> <span data-ttu-id="d0a32-237">`dotnet publish` wird im Kontext von .NET Core ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="d0a32-237">`dotnet publish` runs in the context of .NET Core.</span></span> <span data-ttu-id="d0a32-238">Die `msbuild` -Befehl erfordert .NET Framework, die sie auf Windows-Umgebungen beschränkt.</span><span class="sxs-lookup"><span data-stu-id="d0a32-238">The `msbuild` command requires .NET Framework, which limits it to Windows environments.</span></span>

<span data-ttu-id="d0a32-239">Die einfachste Möglichkeit zum Veröffentlichen mit MSDeploy ist die, zunächst ein Veröffentlichungsprofil in Visual Studio 2017 zu erstellen und das Profil von der Befehlszeile aus zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="d0a32-239">The easiest way to publish with MSDeploy is to first create a publish profile in Visual Studio 2017 and use the profile from the command line.</span></span>

<span data-ttu-id="d0a32-240">Im folgenden Beispiel wird eine ASP.NET Core-Web-app erstellt (mit `dotnet new mvc`), und ein Azure-Veröffentlichungsprofil mit Visual Studio hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="d0a32-240">In the following sample, an ASP.NET Core web app is created (using `dotnet new mvc`), and an Azure publish profile is added with Visual Studio.</span></span>

<span data-ttu-id="d0a32-241">Führen Sie `msbuild` aus einem **Developer-Eingabeaufforderung für VS 2017**.</span><span class="sxs-lookup"><span data-stu-id="d0a32-241">Run `msbuild` from a **Developer Command Prompt for VS 2017**.</span></span> <span data-ttu-id="d0a32-242">Die Developer-Eingabeaufforderung hat den richtigen *msbuild.exe* in der Pfadangabe mit einem Satz der MSBuild-Variablen.</span><span class="sxs-lookup"><span data-stu-id="d0a32-242">The Developer Command Prompt has the correct *msbuild.exe* in its path with some MSBuild variables set.</span></span>

<span data-ttu-id="d0a32-243">MSBuild verwendet die folgende Syntax:</span><span class="sxs-lookup"><span data-stu-id="d0a32-243">MSBuild uses the following syntax:</span></span>

```console
msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>
```

<span data-ttu-id="d0a32-244">Abrufen der `Password` aus der  *\<Publish-Name >. PublishSettings* Datei.</span><span class="sxs-lookup"><span data-stu-id="d0a32-244">Get the `Password` from the *\<Publish name>.PublishSettings* file.</span></span> <span data-ttu-id="d0a32-245">Herunterladen der *. PublishSettings* Datei aus:</span><span class="sxs-lookup"><span data-stu-id="d0a32-245">Download the *.PublishSettings* file from either:</span></span>

* <span data-ttu-id="d0a32-246">Projektmappen-Explorer: Mit der rechten Maustaste auf die Web-App, und wählen Sie **Veröffentlichungsprofil herunterladen**.</span><span class="sxs-lookup"><span data-stu-id="d0a32-246">Solution Explorer: Right-click on the Web App and select **Download Publish Profile**.</span></span>
* <span data-ttu-id="d0a32-247">Azure-Portal: Klicken Sie auf **Get Veröffentlichungsprofil** für die Web-App **Übersicht** Bereich.</span><span class="sxs-lookup"><span data-stu-id="d0a32-247">Azure portal: Click **Get publish profile** on the Web App's **Overview** panel.</span></span>

<span data-ttu-id="d0a32-248">`Username` kann im Veröffentlichungsprofil gefunden werden.</span><span class="sxs-lookup"><span data-stu-id="d0a32-248">`Username` can be found in the publish profile.</span></span>

<span data-ttu-id="d0a32-249">Das folgende Beispiel verwendet die *Web11112 - Web Deploy* Veröffentlichungsprofil:</span><span class="sxs-lookup"><span data-stu-id="d0a32-249">The following sample uses the *Web11112 - Web Deploy* publish profile:</span></span>

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="exclude-files"></a><span data-ttu-id="d0a32-250">Ausschließen von Dateien</span><span class="sxs-lookup"><span data-stu-id="d0a32-250">Exclude files</span></span>

<span data-ttu-id="d0a32-251">Beim Veröffentlichen von ASP.NET Core-Apps sind die Buildartefakte und die Inhalte des Ordners *wwwroot* enthalten.</span><span class="sxs-lookup"><span data-stu-id="d0a32-251">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="d0a32-252">`msbuild` unterstützt [Globmuster](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="d0a32-252">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="d0a32-253">Beispielsweise die folgenden `<Content>` Element schließt den gesamten Text (*".txt"*)-Dateien aus der *"Wwwroot" / Content* Ordner und allen Unterordnern.</span><span class="sxs-lookup"><span data-stu-id="d0a32-253">For example, the following `<Content>` element excludes all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="d0a32-254">Das vorhergehende Markup kann ein Veröffentlichungsprofil hinzugefügt werden oder die *csproj* Datei.</span><span class="sxs-lookup"><span data-stu-id="d0a32-254">The preceding markup can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="d0a32-255">Wenn es zu der *CSPROJ*-Datei hinzugefügt wird, wird die Regel zu allen Veröffentlichungsprofilen im Projekt hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="d0a32-255">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="d0a32-256">Die folgenden `<MsDeploySkipRules>` Element schließt alle Dateien aus dem *"Wwwroot" / Content* Ordner:</span><span class="sxs-lookup"><span data-stu-id="d0a32-256">The following `<MsDeploySkipRules>` element excludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="d0a32-257">`<MsDeploySkipRules>` Löschen wird nicht die *überspringen* Ziele vom Bereitstellungsstandort.</span><span class="sxs-lookup"><span data-stu-id="d0a32-257">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="d0a32-258">`<Content>` Ziel-Dateien und Ordner werden vom Bereitstellungsstandort gelöscht.</span><span class="sxs-lookup"><span data-stu-id="d0a32-258">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="d0a32-259">Nehmen Sie beispielsweise an, dass eine bereitgestellte Web-app die folgenden Dateien wurde:</span><span class="sxs-lookup"><span data-stu-id="d0a32-259">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="d0a32-260">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="d0a32-260">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="d0a32-261">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="d0a32-261">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="d0a32-262">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="d0a32-262">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="d0a32-263">Wenn die folgenden `<MsDeploySkipRules>` Elemente hinzugefügt werden, werden diese Dateien wäre nicht am Bereitstellungsstandort gelöscht.</span><span class="sxs-lookup"><span data-stu-id="d0a32-263">If the following `<MsDeploySkipRules>` elements are added, those files wouldn't be deleted on the deployment site.</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About1.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About2.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About3.cshtml</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="d0a32-264">Die vorangehenden `<MsDeploySkipRules>` Elemente zu verhindern, dass die *übersprungen* Dateien bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="d0a32-264">The preceding `<MsDeploySkipRules>` elements prevent the *skipped* files from being deployed.</span></span> <span data-ttu-id="d0a32-265">Es wird nicht die Dateien löschen, nachdem sie bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="d0a32-265">It won't delete those files once they're deployed.</span></span>

<span data-ttu-id="d0a32-266">Die folgenden `<Content>` Element löscht Zieldateien am Bereitstellungsstandort:</span><span class="sxs-lookup"><span data-stu-id="d0a32-266">The following `<Content>` element deletes the targeted files at the deployment site:</span></span>

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="d0a32-267">Mit der Bereitstellung über die Befehlszeile mit dem vorangehenden `<Content>` Element führt zu folgender Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="d0a32-267">Using command-line deployment with the preceding `<Content>` element yields the following output:</span></span>

```console
MSDeployPublish:
  Starting Web deployment task from source: manifest(C:\Webs\Web1\obj\Release\netcoreapp1.1\PubTmp\Web1.SourceManifest.
  xml) to Destination: auto().
  Deleting file (Web11112\Views\Home\About1.cshtml).
  Deleting file (Web11112\Views\Home\About2.cshtml).
  Deleting file (Web11112\Views\Home\About3.cshtml).
  Updating file (Web11112\web.config).
  Updating file (Web11112\Web1.deps.json).
  Updating file (Web11112\Web1.dll).
  Updating file (Web11112\Web1.pdb).
  Updating file (Web11112\Web1.runtimeconfig.json).
  Successfully executed Web deployment task.
  Publish Succeeded.
Done Building Project "C:\Webs\Web1\Web1.csproj" (default targets).
```

## <a name="include-files"></a><span data-ttu-id="d0a32-268">Includedateien</span><span class="sxs-lookup"><span data-stu-id="d0a32-268">Include files</span></span>

<span data-ttu-id="d0a32-269">Das folgende Markup enthält ein *Bilder* Ordner außerhalb des Projektverzeichnisses auf die *"Wwwroot"-Images* Ordner der Website veröffentlichen:</span><span class="sxs-lookup"><span data-stu-id="d0a32-269">The following markup includes an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site:</span></span>

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="d0a32-270">Das Markup kann der *CSPROJ*-Datei oder dem Veröffentlichungsprofil hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="d0a32-270">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="d0a32-271">Wenn es hinzugefügt wird die *csproj* -Datei, die sie in jedes Veröffentlichungsprofil im Projekt enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="d0a32-271">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>

<span data-ttu-id="d0a32-272">Das folgende hervorgehobene Markup zeigt Folgendes:</span><span class="sxs-lookup"><span data-stu-id="d0a32-272">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="d0a32-273">Kopieren einer Datei von außerhalb des Projekts in den Ordner *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="d0a32-273">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="d0a32-274">Ausschließen des Ordners *wwwroot\Content*.</span><span class="sxs-lookup"><span data-stu-id="d0a32-274">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="d0a32-275">Ausschließen von *Views\Home\About2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d0a32-275">Exclude *Views\Home\About2.cshtml*.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework />
    <ProjectGuid>afa9f185-7ce0-4935-9da1-ab676229d68a</ProjectGuid>
    <publishUrl>bin\Release\PublishOutput</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
  <ItemGroup>
    <ResolvedFileToPublish Include="..\ReadMe2.MD">
      <RelativePath>wwwroot\ReadMe2.MD</RelativePath>
    </ResolvedFileToPublish>

    <Content Update="wwwroot\Content\**\*" CopyToPublishDirectory="Never" />
    <Content Update="Views\Home\About2.cshtml" CopyToPublishDirectory="Never" />

  </ItemGroup>
</Project>
```

<span data-ttu-id="d0a32-276">Weitere Bereitstellungsbeispiele finden Sie unter [WebSDK Readme (WebSDK-Infodatei)](https://github.com/aspnet/websdk).</span><span class="sxs-lookup"><span data-stu-id="d0a32-276">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

## <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="d0a32-277">Führen Sie ein Ziel vor oder nach der Veröffentlichung aus.</span><span class="sxs-lookup"><span data-stu-id="d0a32-277">Run a target before or after publishing</span></span>

<span data-ttu-id="d0a32-278">Die integrierte `BeforePublish` und `AfterPublish` Ziele führen Sie ein Ziel, vor oder nach dem Veröffentlichungsziel.</span><span class="sxs-lookup"><span data-stu-id="d0a32-278">The built-in `BeforePublish` and `AfterPublish` targets execute a target before or after the publish target.</span></span> <span data-ttu-id="d0a32-279">Fügen Sie die folgenden Elemente hinzu, um das Veröffentlichungsprofil Konsole Nachrichten vor und nach der Veröffentlichung protokolliert:</span><span class="sxs-lookup"><span data-stu-id="d0a32-279">Add the following elements to the publish profile to log console messages both before and after publishing:</span></span>

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a><span data-ttu-id="d0a32-280">Veröffentlichen Sie auf einem Server unter Verwendung eines nicht vertrauenswürdigen Zertifikats</span><span class="sxs-lookup"><span data-stu-id="d0a32-280">Publish to a server using an untrusted certificate</span></span>

<span data-ttu-id="d0a32-281">Hinzufügen der `<AllowUntrustedCertificate>` Eigenschaft mit einem Wert von `True` auf dem Veröffentlichungsprofil ein:</span><span class="sxs-lookup"><span data-stu-id="d0a32-281">Add the `<AllowUntrustedCertificate>` property with a value of `True` to the publish profile:</span></span>

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a><span data-ttu-id="d0a32-282">Der Kudu-Dienst</span><span class="sxs-lookup"><span data-stu-id="d0a32-282">The Kudu service</span></span>

<span data-ttu-id="d0a32-283">Verwenden Sie zum Anzeigen der Dateien in einer Azure App Service-Web-app-Bereitstellung die [Kudu Service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="d0a32-283">To view the files in an Azure App Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="d0a32-284">Anfügen der `scm` -Token genutzt, um die Web-app-Name.</span><span class="sxs-lookup"><span data-stu-id="d0a32-284">Append the `scm` token to the web app name.</span></span> <span data-ttu-id="d0a32-285">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="d0a32-285">For example:</span></span>

| <span data-ttu-id="d0a32-286">URL</span><span class="sxs-lookup"><span data-stu-id="d0a32-286">URL</span></span>                                    | <span data-ttu-id="d0a32-287">Ergebnis</span><span class="sxs-lookup"><span data-stu-id="d0a32-287">Result</span></span>       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="d0a32-288">Webanwendung</span><span class="sxs-lookup"><span data-stu-id="d0a32-288">Web App</span></span>      |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="d0a32-289">Kudu-Dienst</span><span class="sxs-lookup"><span data-stu-id="d0a32-289">Kudu service</span></span> |

<span data-ttu-id="d0a32-290">Wählen Sie die [Debug-Konsole](https://github.com/projectkudu/kudu/wiki/Kudu-console) Menüelement zum Anzeigen, bearbeiten, löschen oder Hinzufügen von Dateien.</span><span class="sxs-lookup"><span data-stu-id="d0a32-290">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view, edit, delete, or add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d0a32-291">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="d0a32-291">Additional resources</span></span>

* <span data-ttu-id="d0a32-292">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) vereinfacht die Bereitstellung der Web-apps und Websites so, dass IIS-Servern.</span><span class="sxs-lookup"><span data-stu-id="d0a32-292">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="d0a32-293">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): Datei Probleme, und fordern Sie Funktionen für die Bereitstellung.</span><span class="sxs-lookup"><span data-stu-id="d0a32-293">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
