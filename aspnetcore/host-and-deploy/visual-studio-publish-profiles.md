---
title: Visual Studio-Veröffentlichungsprofile für die Bereitstellung von ASP.NET Core-Apps
author: rick-anderson
description: Informationen zum Erstellen von Veröffentlichungsprofilen in Visual Studio und zu deren Verwendung zum Verwalten von Bereitstellungen von ASP.NET Core-Apps an verschiedene Ziele.
ms.author: riande
ms.custom: mvc
ms.date: 04/10/2018
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 2958b83de13207b93790004a4fa60b0509af3cd2
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2018
ms.locfileid: "41902585"
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="b1e71-103">Visual Studio-Veröffentlichungsprofile für die Bereitstellung von ASP.NET Core-Apps</span><span class="sxs-lookup"><span data-stu-id="b1e71-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="b1e71-104">Von [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b1e71-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b1e71-105">Dieser Artikel konzertiert sich auf die Verwendung von Visual Studio 2017 zum Erstellen von Veröffentlichungsprofilen.</span><span class="sxs-lookup"><span data-stu-id="b1e71-105">This document focuses on using Visual Studio 2017 to create and use publish profiles.</span></span> <span data-ttu-id="b1e71-106">Die Veröffentlichungsprofile, die mit Visual Studio erstellt werden, können von MSBuild und Visual Studio 2017 ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="b1e71-106">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio 2017.</span></span> <span data-ttu-id="b1e71-107">Anweisungen zum Veröffentlichen in Azure finden Sie unter [Veröffentlichen einer ASP.NET Core-Web-App in Azure App Service mit Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs).</span><span class="sxs-lookup"><span data-stu-id="b1e71-107">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="b1e71-108">Die folgende Projektdatei wurde mit dem Befehl `dotnet new mvc` erstellt:</span><span class="sxs-lookup"><span data-stu-id="b1e71-108">The following project file was created with the command `dotnet new mvc`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b1e71-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b1e71-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b1e71-110">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b1e71-110">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="b1e71-111">Das Attribut `Sdk` des `<Project>`-Elements führt die folgenden Tasks durch:</span><span class="sxs-lookup"><span data-stu-id="b1e71-111">The `<Project>` element's `Sdk` attribute accomplishes the following tasks:</span></span>

* <span data-ttu-id="b1e71-112">Es importiert am Anfang die Eigenschaftendatei aus *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props*.</span><span class="sxs-lookup"><span data-stu-id="b1e71-112">Imports the properties file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="b1e71-113">Es importiert am Ende die Zieledatei von *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets*.</span><span class="sxs-lookup"><span data-stu-id="b1e71-113">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="b1e71-114">Der Standardspeicherort für `MSBuildSDKsPath` (mit Visual Studio 2017 Enterprise) ist der Ordner *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks*.</span><span class="sxs-lookup"><span data-stu-id="b1e71-114">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="b1e71-115">Das `Microsoft.NET.Sdk.Web` SDK hängt von Folgendem ab:</span><span class="sxs-lookup"><span data-stu-id="b1e71-115">The `Microsoft.NET.Sdk.Web` SDK depends on:</span></span>

* <span data-ttu-id="b1e71-116">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="b1e71-116">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="b1e71-117">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="b1e71-117">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="b1e71-118">Dadurch werden die folgenden Eigenschaften und Ziele importiert:</span><span class="sxs-lookup"><span data-stu-id="b1e71-118">Which causes the following properties and targets to be imported:</span></span>

* <span data-ttu-id="b1e71-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="b1e71-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="b1e71-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="b1e71-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span></span>
* <span data-ttu-id="b1e71-121">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="b1e71-121">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="b1e71-122">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="b1e71-122">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span></span>

<span data-ttu-id="b1e71-123">Veröffentlichungsziele importieren basierend auf der verwendeten Veröffentlichungsmethode die richtige Gruppe von Zielen.</span><span class="sxs-lookup"><span data-stu-id="b1e71-123">Publish targets import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="b1e71-124">Wenn in MSBuild oder Visual Studio ein Projekt geladen wird, werden die folgenden Aktionen auf hoher Ebene ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="b1e71-124">When MSBuild or Visual Studio loads a project, the following high-level actions occur:</span></span>

* <span data-ttu-id="b1e71-125">Erstellen des Projekts</span><span class="sxs-lookup"><span data-stu-id="b1e71-125">Build project</span></span>
* <span data-ttu-id="b1e71-126">Berechnen der zu veröffentlichenden Dateien</span><span class="sxs-lookup"><span data-stu-id="b1e71-126">Compute files to publish</span></span>
* <span data-ttu-id="b1e71-127">Veröffentlichen der Dateien auf dem Ziel</span><span class="sxs-lookup"><span data-stu-id="b1e71-127">Publish files to destination</span></span>

## <a name="compute-project-items"></a><span data-ttu-id="b1e71-128">Berechnen der Projektelemente</span><span class="sxs-lookup"><span data-stu-id="b1e71-128">Compute project items</span></span>

<span data-ttu-id="b1e71-129">Wenn das Projekt geladen ist, werden die Projektelemente (Dateien) berechnet.</span><span class="sxs-lookup"><span data-stu-id="b1e71-129">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="b1e71-130">Die `item type`-Attribut bestimmt, wie die Datei verarbeitet wird.</span><span class="sxs-lookup"><span data-stu-id="b1e71-130">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="b1e71-131">Standardmäßig sind *CS*-Dateien in der `Compile`-Elementliste enthalten.</span><span class="sxs-lookup"><span data-stu-id="b1e71-131">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="b1e71-132">Dateien in der `Compile`-Elementliste werden kompiliert.</span><span class="sxs-lookup"><span data-stu-id="b1e71-132">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="b1e71-133">Die `Content`-Elementliste enthält Dateien, die zusätzlich zu den Buildausgaben veröffentlicht werden.</span><span class="sxs-lookup"><span data-stu-id="b1e71-133">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="b1e71-134">Standardmäßig sind Dateien, die mit dem Muster `wwwroot/**` übereinstimmen, im Element `Content` enthalten.</span><span class="sxs-lookup"><span data-stu-id="b1e71-134">By default, files matching the pattern `wwwroot/**` are included in the `Content` item.</span></span> <span data-ttu-id="b1e71-135">Das `wwwroot/\*\*` [Globmuster](https://gruntjs.com/configuring-tasks#globbing-patterns) stimmt mit allen Dateien im *wwwroot*-Ordner **und**  in den Unterordnern überein.</span><span class="sxs-lookup"><span data-stu-id="b1e71-135">The `wwwroot/\*\*` [globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) matches all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="b1e71-136">Um eine Datei explizit der Veröffentlichungsliste hinzuzufügen, fügen Sie die Datei wie in [Includedateien](#include-files) gezeigt direkt der *CSPROJ*-Datei hinzu.</span><span class="sxs-lookup"><span data-stu-id="b1e71-136">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Include Files](#include-files).</span></span>

<span data-ttu-id="b1e71-137">Wenn Sie auf die Schaltfläche **Veröffentlichen** in Visual Studio klicken oder aus der Befehlszeile veröffentlichen:</span><span class="sxs-lookup"><span data-stu-id="b1e71-137">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="b1e71-138">Die Eigenschaften und Elemente werden berechnet (die Dateien, die für den Build benötigt werden).</span><span class="sxs-lookup"><span data-stu-id="b1e71-138">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="b1e71-139">**Nur Visual Studio**: NuGet-Pakete werden wiederhergestellt.</span><span class="sxs-lookup"><span data-stu-id="b1e71-139">**Visual Studio only**: NuGet packages are restored.</span></span> <span data-ttu-id="b1e71-140">(Die Wiederherstellung muss explizit vom Benutzer auf der CLI durchgeführt werden.)</span><span class="sxs-lookup"><span data-stu-id="b1e71-140">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="b1e71-141">Das Projekt wird erstellt.</span><span class="sxs-lookup"><span data-stu-id="b1e71-141">The project builds.</span></span>
* <span data-ttu-id="b1e71-142">Die zu veröffentlichenden Elemente werden berechnet (die Dateien, die für die Veröffentlichung benötigt werden).</span><span class="sxs-lookup"><span data-stu-id="b1e71-142">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="b1e71-143">Das Projekt wird veröffentlicht (die berechneten Dateien werden in das Veröffentlichungsziel kopiert).</span><span class="sxs-lookup"><span data-stu-id="b1e71-143">The project is published (the computed files are copied to the publish destination).</span></span>

<span data-ttu-id="b1e71-144">Wenn ein ASP.NET Core-Projekt auf `Microsoft.NET.Sdk.Web` in der Projektdatei verweist, wird eine *app_offline.htm*-Datei im Stammverzeichnis des Verzeichnisses der Web-App abgelegt.</span><span class="sxs-lookup"><span data-stu-id="b1e71-144">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="b1e71-145">Wenn die Datei vorhanden ist, fährt das ASP.NET Core Module die App ordnungsgemäß herunter und verarbeitet die Datei *app_offline.htm* während der Bereitstellung.</span><span class="sxs-lookup"><span data-stu-id="b1e71-145">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="b1e71-146">Weitere Informationen finden Sie unter [Konfigurationsreferenz für das ASP.NET Core-Modul](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span><span class="sxs-lookup"><span data-stu-id="b1e71-146">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="b1e71-147">Grundlegendes zur Veröffentlichung über die Befehlszeile</span><span class="sxs-lookup"><span data-stu-id="b1e71-147">Basic command-line publishing</span></span>

<span data-ttu-id="b1e71-148">Die Veröffentlichung über die Befehlszeile funktioniert auf allen von .NET Core unterstützten Plattformen und setzt Visual Studio nicht voraus.</span><span class="sxs-lookup"><span data-stu-id="b1e71-148">Command-line publishing works on all .NET Core-supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="b1e71-149">In den unten stehenden Beispielen wird der Befehl [dotnet publish](/dotnet/core/tools/dotnet-publish) über das Projektverzeichnis ausgeführt (das die Datei *CSPROJ* enthält).</span><span class="sxs-lookup"><span data-stu-id="b1e71-149">In the samples below, the [dotnet publish](/dotnet/core/tools/dotnet-publish) command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="b1e71-150">Wenn Sie nicht im Projektordner sind, können Sie explizit in den Projektdateipfad übergeben.</span><span class="sxs-lookup"><span data-stu-id="b1e71-150">If not in the project folder, explicitly pass in the project file path.</span></span> <span data-ttu-id="b1e71-151">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b1e71-151">For example:</span></span>

```console
dotnet publish C:\Webs\Web1
```

<span data-ttu-id="b1e71-152">Führen Sie die folgenden Befehle zum Erstellen und Veröffentlichen eine Web-App aus:</span><span class="sxs-lookup"><span data-stu-id="b1e71-152">Run the following commands to create and publish a web app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b1e71-153">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b1e71-153">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b1e71-154">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b1e71-154">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

---

<span data-ttu-id="b1e71-155">Der Befehl [dotnet publish](/dotnet/core/tools/dotnet-publish) erzeugt eine Ausgabe, die der folgenden ähnelt:</span><span class="sxs-lookup"><span data-stu-id="b1e71-155">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="b1e71-156">Der Standardveröffentlichungsordner ist `bin\$(Configuration)\netcoreapp<version>\publish`.</span><span class="sxs-lookup"><span data-stu-id="b1e71-156">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="b1e71-157">Der Standardwert für `$(Configuration)` ist *Debuggen*.</span><span class="sxs-lookup"><span data-stu-id="b1e71-157">The default for `$(Configuration)` is *Debug*.</span></span> <span data-ttu-id="b1e71-158">Im vorgehenden Beispiel ist `<TargetFramework>` gleich `netcoreapp2.0`.</span><span class="sxs-lookup"><span data-stu-id="b1e71-158">In the preceding sample, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="b1e71-159">`dotnet publish -h` zeigt Hilfeinformationen zum Veröffentlichen an.</span><span class="sxs-lookup"><span data-stu-id="b1e71-159">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="b1e71-160">Der folgende Befehl gibt einen `Release`-Build und das Veröffentlichungsverzeichnis an:</span><span class="sxs-lookup"><span data-stu-id="b1e71-160">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:\MyWebs\test
```

<span data-ttu-id="b1e71-161">Der Befehl [dotnet publish](/dotnet/core/tools/dotnet-publish) ruft MSBuild auf, das das `Publish`-Ziel aufruft.</span><span class="sxs-lookup"><span data-stu-id="b1e71-161">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command calls MSBuild, which invokes the `Publish` target.</span></span> <span data-ttu-id="b1e71-162">Alle Parameter, die an `dotnet publish` übergeben werden, werden an MSBuild übergeben.</span><span class="sxs-lookup"><span data-stu-id="b1e71-162">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="b1e71-163">Der Parameter `-c` wird der MSBuild-Eigenschaft `Configuration` zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="b1e71-163">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="b1e71-164">Der Parameter `-o` wird `OutputPath` zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="b1e71-164">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="b1e71-165">MSBuild-Eigenschaften können mithilfe eines der folgenden Formate übergeben werden:</span><span class="sxs-lookup"><span data-stu-id="b1e71-165">MSBuild properties can be passed using either of the following formats:</span></span>

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="b1e71-166">Der folgende Befehl veröffentlicht einen `Release`-Build auf einer Netzwerkfreigabe:</span><span class="sxs-lookup"><span data-stu-id="b1e71-166">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="b1e71-167">Die Netzwerkfreigabe wird durch führende Schrägstriche (*//r8/*) angegeben und funktioniert auf allen Plattformen, die von .NET Core unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="b1e71-167">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="b1e71-168">Versichern Sie sich, dass die veröffentlichte App für die Bereitstellung nicht ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="b1e71-168">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="b1e71-169">Die Dateien im Ordner *publish* sind gesperrt, wenn die App ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="b1e71-169">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="b1e71-170">Die Bereitstellung kann nicht erfolgen, da die gesperrten Dateien nicht kopiert werden können.</span><span class="sxs-lookup"><span data-stu-id="b1e71-170">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="b1e71-171">Veröffentlichungsprofile</span><span class="sxs-lookup"><span data-stu-id="b1e71-171">Publish profiles</span></span>

<span data-ttu-id="b1e71-172">In diesem Abschnitt wird Visual Studio 2017 verwendet, um ein Veröffentlichungsprofil zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="b1e71-172">This section uses Visual Studio 2017 to create a publishing profile.</span></span> <span data-ttu-id="b1e71-173">Nach dem Erstellen kann von Visual Studio oder von der Befehlszeile aus veröffentlicht werden.</span><span class="sxs-lookup"><span data-stu-id="b1e71-173">Once created, publishing from Visual Studio or the command line is available.</span></span>

<span data-ttu-id="b1e71-174">Veröffentlichungsprofile können den Veröffentlichungsprozess vereinfachen, und eine beliebige Anzahl von Profilen kann vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="b1e71-174">Publish profiles can simplify the publishing process, and any number of profiles can exist.</span></span> <span data-ttu-id="b1e71-175">Erstellen Sie ein Veröffentlichungsprofil in Visual Studio durch Auswahl eines der folgenden Pfade:</span><span class="sxs-lookup"><span data-stu-id="b1e71-175">Create a publish profile in Visual Studio by choosing one of the following paths:</span></span>

* <span data-ttu-id="b1e71-176">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie **Veröffentlichen** aus.</span><span class="sxs-lookup"><span data-stu-id="b1e71-176">Right-click the project in Solution Explorer and select **Publish**.</span></span>
* <span data-ttu-id="b1e71-177">Wählen Sie **Veröffentlichen &lt;Projektname&gt;** im Menü **Build** aus.</span><span class="sxs-lookup"><span data-stu-id="b1e71-177">Select **Publish &lt;project_name&gt;** from the **Build** menu.</span></span>

<span data-ttu-id="b1e71-178">Die Registerkarte **Veröffentlichen** der Seite für Appfunktionen wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="b1e71-178">The **Publish** tab of the app capacities page is displayed.</span></span> <span data-ttu-id="b1e71-179">Wenn das Projekt kein Veröffentlichungsprofil enthält, wird die folgende Seite angezeigt:</span><span class="sxs-lookup"><span data-stu-id="b1e71-179">If the project lacks a publish profile, the following page is displayed:</span></span>

![Die Registerkarte „Veröffentlichen“ der Seite für Appfunktionen wird angezeigt](visual-studio-publish-profiles/_static/az.png)

<span data-ttu-id="b1e71-181">Wenn **Ordner** ausgewählt ist, geben Sie einen Ordnerpfad ein, um die veröffentlichten Objekte zu speichern.</span><span class="sxs-lookup"><span data-stu-id="b1e71-181">When **Folder** is selected, specify a folder path to store the published assets.</span></span> <span data-ttu-id="b1e71-182">Der Standardordner ist *bin\Release\PublishOutput*.</span><span class="sxs-lookup"><span data-stu-id="b1e71-182">The default folder is *bin\Release\PublishOutput*.</span></span> <span data-ttu-id="b1e71-183">Klicken Sie auf die Schaltfläche **Profil erstellen**, um den Vorgang abzuschließen.</span><span class="sxs-lookup"><span data-stu-id="b1e71-183">Click the **Create Profile** button to finish.</span></span>

<span data-ttu-id="b1e71-184">Sobald ein Veröffentlichungsprofil erstellt wurde, ändert sich die Registerkarte **Veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="b1e71-184">Once a publish profile is created, the **Publish** tab changes.</span></span> <span data-ttu-id="b1e71-185">Das neu erstellte Profil wird in einer Dropdownliste angezeigt.</span><span class="sxs-lookup"><span data-stu-id="b1e71-185">The newly created profile appears in a drop-down list.</span></span> <span data-ttu-id="b1e71-186">Klicken Sie auf **Neues Profil erstellen**, um ein anderes neues Profil zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="b1e71-186">Click **Create new profile** to create another new profile.</span></span>

![Die Registerkarte „Veröffentlichen“ der Seite für Appfunktionen zeigt das Ordnerprofil an](visual-studio-publish-profiles/_static/create_new.png)

<span data-ttu-id="b1e71-188">Der Webpublishing-Assistent unterstützt folgende Veröffentlichungsziele:</span><span class="sxs-lookup"><span data-stu-id="b1e71-188">The Publish wizard supports the following publish targets:</span></span>

* <span data-ttu-id="b1e71-189">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="b1e71-189">Azure App Service</span></span>
* <span data-ttu-id="b1e71-190">Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="b1e71-190">Azure Virtual Machines</span></span>
* <span data-ttu-id="b1e71-191">IIS, FTP usw. (für alle Webserver)</span><span class="sxs-lookup"><span data-stu-id="b1e71-191">IIS, FTP, etc. (for any web server)</span></span>
* <span data-ttu-id="b1e71-192">Ordner</span><span class="sxs-lookup"><span data-stu-id="b1e71-192">Folder</span></span>
* <span data-ttu-id="b1e71-193">Profil importieren</span><span class="sxs-lookup"><span data-stu-id="b1e71-193">Import Profile</span></span>

<span data-ttu-id="b1e71-194">Weitere Informationen finden Sie unter [Welche Optionen für die Veröffentlichung sind für mich geeignet?](/visualstudio/ide/not-in-toc/web-publish-options).</span><span class="sxs-lookup"><span data-stu-id="b1e71-194">For more information, see [What publishing options are right for me](/visualstudio/ide/not-in-toc/web-publish-options).</span></span>

<span data-ttu-id="b1e71-195">Beim Erstellen eines Veröffentlichungsprofils mit Visual Studio wird eine *Properties/PublishProfiles/&lt;Profilname>&gt;PUBXML*-MSBuild-Datei erstellt.</span><span class="sxs-lookup"><span data-stu-id="b1e71-195">When creating a publish profile with Visual Studio, a *Properties/PublishProfiles/&lt;profile_name&gt;.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="b1e71-196">Diese *PUBXML*-Datei ist eine MSBuild-Datei und enthält die Konfigurationseinstellungen für die Veröffentlichung.</span><span class="sxs-lookup"><span data-stu-id="b1e71-196">The *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="b1e71-197">Die Datei kann geändert werden, um den Build- und Veröffentlichungsprozess anzupassen.</span><span class="sxs-lookup"><span data-stu-id="b1e71-197">This file can be changed to customize the build and publish process.</span></span> <span data-ttu-id="b1e71-198">Diese Datei wird vom Veröffentlichungsprozess gelesen.</span><span class="sxs-lookup"><span data-stu-id="b1e71-198">This file is read by the publishing process.</span></span> <span data-ttu-id="b1e71-199">`<LastUsedBuildConfiguration>` ist ein Sonderfall, da es sich dabei um eine globale Eigenschaft handelt, die nicht in einer Datei enthalten sein sollte, die in den Build importiert wurde.</span><span class="sxs-lookup"><span data-stu-id="b1e71-199">`<LastUsedBuildConfiguration>` is special because it's a global property and shouldn't be in any file that's imported in the build.</span></span> <span data-ttu-id="b1e71-200">Weitere Informationen finden Sie unter [MSBuild: how to set the configuration property (MSBuild: Festlegen der Konfigurationseigenschaft)](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx).</span><span class="sxs-lookup"><span data-stu-id="b1e71-200">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more information.</span></span>

<span data-ttu-id="b1e71-201">Beim Veröffentlichen in einem Azure-Ziel enthält die *PUBXML*-Datei Ihre Azure-Abonnement-ID.</span><span class="sxs-lookup"><span data-stu-id="b1e71-201">When publishing to an Azure target, the *.pubxml* file contains your Azure subscription identifier.</span></span> <span data-ttu-id="b1e71-202">Bei diesem Zieltyp wird vom Hinzufügen der Datei zur Quellkontrolle abgeraten.</span><span class="sxs-lookup"><span data-stu-id="b1e71-202">With that target type, adding this file to source control is discouraged.</span></span> <span data-ttu-id="b1e71-203">Beim Veröffentlichen auf einem Nicht-Azure-Ziel kann die *PUBXML*-Datei eingecheckt werden.</span><span class="sxs-lookup"><span data-stu-id="b1e71-203">When publishing to a non-Azure target, it's safe to check in the *.pubxml* file.</span></span>

<span data-ttu-id="b1e71-204">Vertrauliche Informationen (z.B. das Kennwort für die Veröffentlichung) werden pro Benutzer/Computer verschlüsselt.</span><span class="sxs-lookup"><span data-stu-id="b1e71-204">Sensitive information (like the publish password) is encrypted on a per user/machine level.</span></span> <span data-ttu-id="b1e71-205">Sie werden in der Datei *Properties/PublishProfiles/&lt;Profilname&gt;.pubxml.user* gespeichert.</span><span class="sxs-lookup"><span data-stu-id="b1e71-205">It's stored in the *Properties/PublishProfiles/&lt;profile_name&gt;.pubxml.user* file.</span></span> <span data-ttu-id="b1e71-206">Da diese Datei vertrauliche Informationen enthalten kann, sollte sie nicht in die Quellverwaltung eingecheckt werden.</span><span class="sxs-lookup"><span data-stu-id="b1e71-206">Because this file can store sensitive information, it shouldn't be checked into source control.</span></span>

<span data-ttu-id="b1e71-207">Einen Überblick über das Veröffentlichen einer Web-App auf ASP.NET Core finden Sie unter [Hosten und Bereitstellen](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="b1e71-207">For an overview of how to publish a web app on ASP.NET Core, see [Host and deploy](xref:host-and-deploy/index).</span></span> <span data-ttu-id="b1e71-208">Die MSBuild-Aufgaben und -Ziele, die zum Veröffentlichen einer ASP.NET Core-App erforderlich sind, werden als Open Source in https://github.com/aspnet/websdk bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="b1e71-208">The MSBuild tasks and targets necessary to publish an ASP.NET Core app are open-source at https://github.com/aspnet/websdk.</span></span>

<span data-ttu-id="b1e71-209">`dotnet publish` kann den Ordner, MSDeploy und [Kudu](https://github.com/projectkudu/kudu/wiki)-Veröffentlichungsprofile verwenden:</span><span class="sxs-lookup"><span data-stu-id="b1e71-209">`dotnet publish` can use folder, MSDeploy, and [Kudu](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>

<span data-ttu-id="b1e71-210">Ordner (funktioniert plattformübergreifend):</span><span class="sxs-lookup"><span data-stu-id="b1e71-210">Folder (works cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

<span data-ttu-id="b1e71-211">MSDeploy (funktioniert zurzeit nur in Windows, da MSDdeploy nicht plattformübergreifend ist):</span><span class="sxs-lookup"><span data-stu-id="b1e71-211">MSDeploy (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

<span data-ttu-id="b1e71-212">MSDeploy-Paket (funktioniert zurzeit nur in Windows, da MSDdeploy nicht plattformübergreifend ist):</span><span class="sxs-lookup"><span data-stu-id="b1e71-212">MSDeploy package (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

<span data-ttu-id="b1e71-213">Übergeben Sie in den vorherigen Beispielen `deployonbuild` **nicht** an `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="b1e71-213">In the preceding samples, **don't** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="b1e71-214">Weitere Informationen finden Sie unter [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span><span class="sxs-lookup"><span data-stu-id="b1e71-214">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="b1e71-215">`dotnet publish` unterstützt Kudu-APIs zur Veröffentlichung in Azure von jeder Plattform aus.</span><span class="sxs-lookup"><span data-stu-id="b1e71-215">`dotnet publish` supports Kudu APIs to publish to Azure from any platform.</span></span> <span data-ttu-id="b1e71-216">Die Visual Studio-Veröffentlichung unterstützt die Kudu-APIs, wird für die plattformübergreifende Veröffentlichung in Azure aber vom WebSDK unterstützt.</span><span class="sxs-lookup"><span data-stu-id="b1e71-216">Visual Studio publish supports the Kudu APIs, but it's supported by WebSDK for cross-platform publish to Azure.</span></span>

<span data-ttu-id="b1e71-217">Fügen Sie dem Ordner *Properties/PublishProfiles* ein Veröffentlichungsprofil mit folgendem Inhalt hinzu:</span><span class="sxs-lookup"><span data-stu-id="b1e71-217">Add a publish profile to the *Properties/PublishProfiles* folder with the following content:</span></span>

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

<span data-ttu-id="b1e71-218">Führen Sie den folgenden Befehl aus, um die Veröffentlichungsinhalte zu zippen und über die Kudu-APIs in Azure zu veröffentlichen:</span><span class="sxs-lookup"><span data-stu-id="b1e71-218">Run the following command to zip up the publish contents and publish it to Azure using the Kudu APIs:</span></span>

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

<span data-ttu-id="b1e71-219">Legen Sie die folgenden MSBuild-Eigenschaften fest, wenn Sie ein Veröffentlichungsprofil verwenden:</span><span class="sxs-lookup"><span data-stu-id="b1e71-219">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

<span data-ttu-id="b1e71-220">Beispielsweise können Sie einen der unten stehenden Befehle ausführen, wenn Sie mit einem Profil namens *FolderProfile* veröffentlichen:</span><span class="sxs-lookup"><span data-stu-id="b1e71-220">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="b1e71-221">Beim Aufrufen von [dotnet build](/dotnet/core/tools/dotnet-build) wird `msbuild` aufgerufen, um den Build- und Veröffentlichungsprozess auszuführen.</span><span class="sxs-lookup"><span data-stu-id="b1e71-221">When invoking [dotnet build](/dotnet/core/tools/dotnet-build), it calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="b1e71-222">Der Aufruf von `dotnet build` oder `msbuild` ist bei Übergabe in einem Ordnerprofil gleichwertig.</span><span class="sxs-lookup"><span data-stu-id="b1e71-222">Calling either `dotnet build` or `msbuild` is equivalent when passing in a folder profile.</span></span> <span data-ttu-id="b1e71-223">Wenn Sie MSBuild direkt in Windows aufrufen, wird die .NET Framework-Version von MSBuild verwendet.</span><span class="sxs-lookup"><span data-stu-id="b1e71-223">When calling MSBuild directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="b1e71-224">Die Veröffentlichung mit MSDeploy ist aktuell auf Windows-Computer beschränkt.</span><span class="sxs-lookup"><span data-stu-id="b1e71-224">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="b1e71-225">Wenn Sie `dotnet build` von einem Profil ohne Ordner aufrufen, wird MSBuild aufgerufen. MSBuild verwendet MSDeploy für Profile ohne Ordner.</span><span class="sxs-lookup"><span data-stu-id="b1e71-225">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="b1e71-226">Wenn Sie `dotnet build` von einem Profil ohne Ordner aufrufen, wird MSBuild (mithilfe von MSDeploy) aufgerufen. Dies führt zu einem Fehler (auch beim Ausführen auf einer Windows-Plattform).</span><span class="sxs-lookup"><span data-stu-id="b1e71-226">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="b1e71-227">Rufen Sie MSBuild direkt auf, um mit einem Profil ohne Ordner zu veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="b1e71-227">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="b1e71-228">Der folgende Ordner „Veröffentlichungsprofil“ wurde mit Visual Studio erstellt und veröffentlicht in eine Netzwerkfreigabe.</span><span class="sxs-lookup"><span data-stu-id="b1e71-228">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

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

<span data-ttu-id="b1e71-229">Beachten Sie, dass `<LastUsedBuildConfiguration>` auf `Release` festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="b1e71-229">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span> <span data-ttu-id="b1e71-230">Bei der Veröffentlichung von Visual Studio wird der `<LastUsedBuildConfiguration>`-Wert für die Konfigurationseigenschaft mithilfe des Werts festgelegt, der beim Starten des Veröffentlichungsprozesses vorliegt.</span><span class="sxs-lookup"><span data-stu-id="b1e71-230">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="b1e71-231">Die `<LastUsedBuildConfiguration>`-Konfigurationseigenschaft ist ein Sonderfall und sollte nicht in einer importierte MSBuild-Datei überschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="b1e71-231">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="b1e71-232">Diese Eigenschaft kann von der Befehlszeile aus überschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="b1e71-232">This property can be overridden from the command line.</span></span>

<span data-ttu-id="b1e71-233">Mit der .NET Core-CLI:</span><span class="sxs-lookup"><span data-stu-id="b1e71-233">Using the .NET Core CLI:</span></span>

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

<span data-ttu-id="b1e71-234">Verwenden von MSBuild:</span><span class="sxs-lookup"><span data-stu-id="b1e71-234">Using MSBuild:</span></span>

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="b1e71-235">Veröffentlichen auf einen MSDeploy-Endpunkt von der Befehlszeile</span><span class="sxs-lookup"><span data-stu-id="b1e71-235">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="b1e71-236">Die Veröffentlichung kann mithilfe der .NET Core-CLI oder mithilfe von MSBuild durchgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="b1e71-236">Publishing can be accomplished using the .NET Core CLI or MSBuild.</span></span> <span data-ttu-id="b1e71-237">`dotnet publish` wird im Kontext von .NET Core ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="b1e71-237">`dotnet publish` runs in the context of .NET Core.</span></span> <span data-ttu-id="b1e71-238">Der `msbuild`-Befehl erfordert .NET Framework, wodurch er auf Windows-Umgebungen beschränkt wird.</span><span class="sxs-lookup"><span data-stu-id="b1e71-238">The `msbuild` command requires .NET Framework, which limits it to Windows environments.</span></span>

<span data-ttu-id="b1e71-239">Die einfachste Möglichkeit zum Veröffentlichen mit MSDeploy ist die, zunächst ein Veröffentlichungsprofil in Visual Studio 2017 zu erstellen und das Profil von der Befehlszeile aus zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="b1e71-239">The easiest way to publish with MSDeploy is to first create a publish profile in Visual Studio 2017 and use the profile from the command line.</span></span>

<span data-ttu-id="b1e71-240">Im folgenden Beispiel wurde eine ASP.NET Core-App (mithilfe von `dotnet new mvc`) erstellt und ein Azure-Veröffentlichungsprofil mit Visual Studio hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="b1e71-240">In the following sample, an ASP.NET Core web app is created (using `dotnet new mvc`), and an Azure publish profile is added with Visual Studio.</span></span>

<span data-ttu-id="b1e71-241">Führen Sie `msbuild` von einer **Developer-Eingabeaufforderung für VS 2017** aus.</span><span class="sxs-lookup"><span data-stu-id="b1e71-241">Run `msbuild` from a **Developer Command Prompt for VS 2017**.</span></span> <span data-ttu-id="b1e71-242">Die Developer-Eingabeaufforderung verfügt in ihrem Pfad über die richtige *msbuild.exe*-Datei und über einige festgelegte MSBuild-Variablen.</span><span class="sxs-lookup"><span data-stu-id="b1e71-242">The Developer Command Prompt has the correct *msbuild.exe* in its path with some MSBuild variables set.</span></span>

<span data-ttu-id="b1e71-243">MSBuild verwendet die folgende Syntax:</span><span class="sxs-lookup"><span data-stu-id="b1e71-243">MSBuild uses the following syntax:</span></span>

```console
msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>
```

<span data-ttu-id="b1e71-244">Sie erhalten das `Password` von der Datei *\<Veröffentlichungsname>.PublishSettings*.</span><span class="sxs-lookup"><span data-stu-id="b1e71-244">Get the `Password` from the *\<Publish name>.PublishSettings* file.</span></span> <span data-ttu-id="b1e71-245">Laden Sie die Datei *.PublishSettings* von einem der folgenden Speicherorte herunter:</span><span class="sxs-lookup"><span data-stu-id="b1e71-245">Download the *.PublishSettings* file from either:</span></span>

* <span data-ttu-id="b1e71-246">Projektmappen-Explorer: Klicken Sie mit der rechten Maustaste auf die Web-App, und wählen Sie **Veröffentlichungsprofil herunterladen** aus.</span><span class="sxs-lookup"><span data-stu-id="b1e71-246">Solution Explorer: Right-click on the Web App and select **Download Publish Profile**.</span></span>
* <span data-ttu-id="b1e71-247">Azure-Portal: Klicken Sie auf **Veröffentlichungsprofil abrufen** im Bereich **Übersicht** der Web-App.</span><span class="sxs-lookup"><span data-stu-id="b1e71-247">Azure portal: Click **Get publish profile** on the Web App's **Overview** panel.</span></span>

<span data-ttu-id="b1e71-248">`Username` kann im Veröffentlichungsprofil gefunden werden.</span><span class="sxs-lookup"><span data-stu-id="b1e71-248">`Username` can be found in the publish profile.</span></span>

<span data-ttu-id="b1e71-249">Das folgende Beispiel verwendet das Veröffentlichungsprofil *Web11112 – Web Deploy*:</span><span class="sxs-lookup"><span data-stu-id="b1e71-249">The following sample uses the *Web11112 - Web Deploy* publish profile:</span></span>

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="exclude-files"></a><span data-ttu-id="b1e71-250">Dateien ausschließen</span><span class="sxs-lookup"><span data-stu-id="b1e71-250">Exclude files</span></span>

<span data-ttu-id="b1e71-251">Beim Veröffentlichen von ASP.NET Core-Apps sind die Buildartefakte und die Inhalte des Ordners *wwwroot* enthalten.</span><span class="sxs-lookup"><span data-stu-id="b1e71-251">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="b1e71-252">`msbuild` unterstützt [Globmuster](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="b1e71-252">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="b1e71-253">Das folgende `<Content>`-Element beispielsweise schließt alle Textdateien (*TXT*) aus dem Ordner *wwwroot/content* und all seinen Unterordnern aus.</span><span class="sxs-lookup"><span data-stu-id="b1e71-253">For example, the following `<Content>` element excludes all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="b1e71-254">Das vorstehende Markup kann einem Veröffentlichungsprofil oder der *CSPROJ*-Datei hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="b1e71-254">The preceding markup can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="b1e71-255">Wenn es zu der *CSPROJ*-Datei hinzugefügt wird, wird die Regel zu allen Veröffentlichungsprofilen im Projekt hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="b1e71-255">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="b1e71-256">Das folgende `<MsDeploySkipRules>`-Element schließt alle Dateien aus dem Ordner *wwwroot/content* aus:</span><span class="sxs-lookup"><span data-stu-id="b1e71-256">The following `<MsDeploySkipRules>` element excludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="b1e71-257">`<MsDeploySkipRules>` löscht die *Skip*-Ziele von der Bereitstellungsseite nicht.</span><span class="sxs-lookup"><span data-stu-id="b1e71-257">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="b1e71-258">`<Content>`-Zieldateien und -ordner werden von der Bereitstellungsseite gelöscht.</span><span class="sxs-lookup"><span data-stu-id="b1e71-258">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="b1e71-259">Nehmen wir beispielsweise an, dass eine bereitgestellte Web-App folgende Dateien enthält:</span><span class="sxs-lookup"><span data-stu-id="b1e71-259">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="b1e71-260">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="b1e71-260">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="b1e71-261">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="b1e71-261">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="b1e71-262">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="b1e71-262">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="b1e71-263">Wenn folgende `<MsDeploySkipRules>`-Elemente hinzugefügt werden, würden diese Dateien nicht von der Bereitstellungsseite gelöscht werden.</span><span class="sxs-lookup"><span data-stu-id="b1e71-263">If the following `<MsDeploySkipRules>` elements are added, those files wouldn't be deleted on the deployment site.</span></span>

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

<span data-ttu-id="b1e71-264">Die vorangehenden `<MsDeploySkipRules>`-Elemente verhindern, dass *übersprungene* Dateien bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="b1e71-264">The preceding `<MsDeploySkipRules>` elements prevent the *skipped* files from being deployed.</span></span> <span data-ttu-id="b1e71-265">Diese Dateien werden nach ihrer Bereitstellung nicht gelöscht.</span><span class="sxs-lookup"><span data-stu-id="b1e71-265">It won't delete those files once they're deployed.</span></span>

<span data-ttu-id="b1e71-266">Das folgende `<Content>`-Element löscht die Zieldateien auf der Bereitstellungsseite:</span><span class="sxs-lookup"><span data-stu-id="b1e71-266">The following `<Content>` element deletes the targeted files at the deployment site:</span></span>

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="b1e71-267">Die Bereitstellung über die Befehlszeile mit dem vorangehenden `<Content>`-Element führt zu folgender Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="b1e71-267">Using command-line deployment with the preceding `<Content>` element yields the following output:</span></span>

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

## <a name="include-files"></a><span data-ttu-id="b1e71-268">Includedateien</span><span class="sxs-lookup"><span data-stu-id="b1e71-268">Include files</span></span>

<span data-ttu-id="b1e71-269">Das folgende Markup schließt einen Ordner *images* außerhalb des Projektverzeichnisses vom Ordner *wwwroot/images* auf der Veröffentlichungssite ein:</span><span class="sxs-lookup"><span data-stu-id="b1e71-269">The following markup includes an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site:</span></span>

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="b1e71-270">Das Markup kann der *CSPROJ*-Datei oder dem Veröffentlichungsprofil hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="b1e71-270">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="b1e71-271">Wenn es der *CSPROJ*-Datei hinzugefügt wird, wird es in jedes Veröffentlichungsprofil im Projekt eingeschlossen.</span><span class="sxs-lookup"><span data-stu-id="b1e71-271">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>

<span data-ttu-id="b1e71-272">Das folgende hervorgehobene Markup zeigt Folgendes:</span><span class="sxs-lookup"><span data-stu-id="b1e71-272">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="b1e71-273">Kopieren einer Datei von außerhalb des Projekts in den Ordner *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="b1e71-273">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="b1e71-274">Ausschließen des Ordners *wwwroot\Content*.</span><span class="sxs-lookup"><span data-stu-id="b1e71-274">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="b1e71-275">Ausschließen von *Views\Home\About2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b1e71-275">Exclude *Views\Home\About2.cshtml*.</span></span>

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

<span data-ttu-id="b1e71-276">Weitere Bereitstellungsbeispiele finden Sie unter [WebSDK Readme (WebSDK-Infodatei)](https://github.com/aspnet/websdk).</span><span class="sxs-lookup"><span data-stu-id="b1e71-276">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

## <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="b1e71-277">Führen Sie ein Ziel vor oder nach der Veröffentlichung aus.</span><span class="sxs-lookup"><span data-stu-id="b1e71-277">Run a target before or after publishing</span></span>

<span data-ttu-id="b1e71-278">Die integrierten Ziele `BeforePublish` und `AfterPublish` führen ein Ziel vor oder nach dem Veröffentlichungsziel aus.</span><span class="sxs-lookup"><span data-stu-id="b1e71-278">The built-in `BeforePublish` and `AfterPublish` targets execute a target before or after the publish target.</span></span> <span data-ttu-id="b1e71-279">Fügen Sie die folgenden Elemente dem Veröffentlichungsprofil hinzu, um Konsolenmeldungen vor und nach der Veröffentlichung zu protokollieren:</span><span class="sxs-lookup"><span data-stu-id="b1e71-279">Add the following elements to the publish profile to log console messages both before and after publishing:</span></span>

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a><span data-ttu-id="b1e71-280">Veröffentlichen auf einem Server unter Verwendung eines nicht vertrauenswürdigen Zertifikats</span><span class="sxs-lookup"><span data-stu-id="b1e71-280">Publish to a server using an untrusted certificate</span></span>

<span data-ttu-id="b1e71-281">Fügen Sie die Eigenschaft `<AllowUntrustedCertificate>` mit dem Wert `True` auf dem Veröffentlichungsprofil hinzu:</span><span class="sxs-lookup"><span data-stu-id="b1e71-281">Add the `<AllowUntrustedCertificate>` property with a value of `True` to the publish profile:</span></span>

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a><span data-ttu-id="b1e71-282">Der Kudu-Dienst</span><span class="sxs-lookup"><span data-stu-id="b1e71-282">The Kudu service</span></span>

<span data-ttu-id="b1e71-283">Verwenden Sie zum Anzeigen der Dateien in einer Web-App-Bereitstellung von Azure App Service den [Kudu-Dienst](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="b1e71-283">To view the files in an Azure App Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="b1e71-284">Fügen Sie das Token `scm` an den Namen Ihrer Web-App an.</span><span class="sxs-lookup"><span data-stu-id="b1e71-284">Append the `scm` token to the web app name.</span></span> <span data-ttu-id="b1e71-285">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b1e71-285">For example:</span></span>

| <span data-ttu-id="b1e71-286">URL</span><span class="sxs-lookup"><span data-stu-id="b1e71-286">URL</span></span>                                    | <span data-ttu-id="b1e71-287">Ergebnis</span><span class="sxs-lookup"><span data-stu-id="b1e71-287">Result</span></span>       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="b1e71-288">Webanwendung</span><span class="sxs-lookup"><span data-stu-id="b1e71-288">Web App</span></span>      |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="b1e71-289">Der Kudu-Dienst</span><span class="sxs-lookup"><span data-stu-id="b1e71-289">Kudu service</span></span> |

<span data-ttu-id="b1e71-290">Wählen Sie das Menüelement [Debugging-Konsole](https://github.com/projectkudu/kudu/wiki/Kudu-console), aus, um Dateien anzuzeigen, zu bearbeiten, zu löschen oder hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="b1e71-290">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view, edit, delete, or add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b1e71-291">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="b1e71-291">Additional resources</span></span>

* <span data-ttu-id="b1e71-292">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) vereinfacht die Bereitstellung von Web-Apps und Websites auf IIS-Servern.</span><span class="sxs-lookup"><span data-stu-id="b1e71-292">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="b1e71-293">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): Probleme melden und Features für die Bereitstellung anfordern.</span><span class="sxs-lookup"><span data-stu-id="b1e71-293">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
* [<span data-ttu-id="b1e71-294">Veröffentlichen einer ASP.NET-Web-App auf einer Azure-VM aus Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b1e71-294">Publish an ASP.NET Web App to an Azure VM from Visual Studio</span></span>](/azure/virtual-machines/windows/publish-web-app-from-visual-studio)
