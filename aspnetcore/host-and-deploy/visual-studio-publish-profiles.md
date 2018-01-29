---
title: "Visual Studio die Veröffentlichungsprofile für ASP.NET Core-app-Bereitstellung"
author: rick-anderson
description: "Erfahren Sie, wie erstellen Veröffentlichungsprofile für ASP.NET Core-apps in Visual Studio."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 09/26/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 1f403447c39db4ebfe3dafda591602f0dc9db8c3
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="1634b-103">Visual Studio die Veröffentlichungsprofile für ASP.NET Core-app-Bereitstellung</span><span class="sxs-lookup"><span data-stu-id="1634b-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="1634b-104">Von [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1634b-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1634b-105">Dieser Artikel konzertiert sich auf die Verwendung von Visual Studio 2017 zum Erstellen von Veröffentlichungsprofilen.</span><span class="sxs-lookup"><span data-stu-id="1634b-105">This article focuses on using Visual Studio 2017 to create publish profiles.</span></span> <span data-ttu-id="1634b-106">Die Veröffentlichungsprofile, die mit Visual Studio erstellt werden, können von MSBuild und Visual Studio 2017 ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="1634b-106">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio 2017.</span></span> <span data-ttu-id="1634b-107">Dieser Artikel enthält Informationen zum Veröffentlichungsprozess.</span><span class="sxs-lookup"><span data-stu-id="1634b-107">The article provides details of the publishing process.</span></span> <span data-ttu-id="1634b-108">Anweisungen zum Veröffentlichen in Azure finden Sie unter [Veröffentlichen einer ASP.NET Core-Web-App in Azure App Service mit Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs).</span><span class="sxs-lookup"><span data-stu-id="1634b-108">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="1634b-109">Die folgende *CSPROJ*-Datei wurde mit dem Befehl `dotnet new mvc` erstellt:</span><span class="sxs-lookup"><span data-stu-id="1634b-109">The following *.csproj* file was created with the command `dotnet new mvc`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1634b-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1634b-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1634b-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1634b-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="1634b-112">Das `Sdk`-Attribut im `<Project>`-Element (in der ersten Zeile) des obenstehenden Markups bewirkt Folgendes:</span><span class="sxs-lookup"><span data-stu-id="1634b-112">The `Sdk` attribute in the `<Project>` element (in the first line) of the markup above does the following:</span></span>

* <span data-ttu-id="1634b-113">Importiert die Eigenschaftendatei aus *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* am Anfang.</span><span class="sxs-lookup"><span data-stu-id="1634b-113">Imports the properties file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="1634b-114">Es importiert am Ende die Zieledatei von *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets*.</span><span class="sxs-lookup"><span data-stu-id="1634b-114">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="1634b-115">Der Standardspeicherort für `MSBuildSDKsPath` (mit Visual Studio 2017 Enterprise) ist der Ordner *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks*.</span><span class="sxs-lookup"><span data-stu-id="1634b-115">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="1634b-116">`Microsoft.NET.Sdk.Web` ist abhängig von:</span><span class="sxs-lookup"><span data-stu-id="1634b-116">`Microsoft.NET.Sdk.Web` depends on:</span></span>

* <span data-ttu-id="1634b-117">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="1634b-117">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="1634b-118">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="1634b-118">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="1634b-119">Dadurch werden die folgenden Eigenschaften und Ziele, die importiert werden:</span><span class="sxs-lookup"><span data-stu-id="1634b-119">Which causes the following properties and targets to be imported:</span></span>

* <span data-ttu-id="1634b-120">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="1634b-120">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="1634b-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="1634b-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span></span>
* <span data-ttu-id="1634b-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="1634b-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="1634b-123">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="1634b-123">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span></span>

<span data-ttu-id="1634b-124">Veröffentlichen Sie Targets-Import, legen Sie rechts von Zielen, die auf der Grundlage der Publish-Methode verwendet.</span><span class="sxs-lookup"><span data-stu-id="1634b-124">Publish targets import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="1634b-125">Wenn in MSBuild oder Visual Studio ein Projekt geladen wird, werden die folgenden Aktionen auf hoher Ebene ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="1634b-125">When MSBuild or Visual Studio loads a project, the following high level actions are performed:</span></span>

* <span data-ttu-id="1634b-126">Erstellen des Projekts</span><span class="sxs-lookup"><span data-stu-id="1634b-126">Build project</span></span>
* <span data-ttu-id="1634b-127">Berechnen der zu veröffentlichenden Dateien</span><span class="sxs-lookup"><span data-stu-id="1634b-127">Compute files to publish</span></span>
* <span data-ttu-id="1634b-128">Veröffentlichen der Dateien auf dem Ziel</span><span class="sxs-lookup"><span data-stu-id="1634b-128">Publish files to destination</span></span>

### <a name="compute-project-items"></a><span data-ttu-id="1634b-129">Berechnen der Projektelemente</span><span class="sxs-lookup"><span data-stu-id="1634b-129">Compute project items</span></span>

<span data-ttu-id="1634b-130">Wenn das Projekt geladen ist, werden die Projektelemente (Dateien) berechnet.</span><span class="sxs-lookup"><span data-stu-id="1634b-130">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="1634b-131">Die `item type`-Attribut bestimmt, wie die Datei verarbeitet wird.</span><span class="sxs-lookup"><span data-stu-id="1634b-131">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="1634b-132">Standardmäßig sind *CS*-Dateien in der `Compile`-Elementliste enthalten.</span><span class="sxs-lookup"><span data-stu-id="1634b-132">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="1634b-133">Dateien in der `Compile`-Elementliste werden kompiliert.</span><span class="sxs-lookup"><span data-stu-id="1634b-133">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="1634b-134">Die `Content` Elementliste enthält Dateien, die zusätzlich zu den Buildausgaben veröffentlicht werden.</span><span class="sxs-lookup"><span data-stu-id="1634b-134">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="1634b-135">Standardmäßig Dateien gemäß dem Muster `wwwroot/**` befinden sich die `Content` Element.</span><span class="sxs-lookup"><span data-stu-id="1634b-135">By default, files matching the pattern `wwwroot/**` are included in the `Content` item.</span></span> <span data-ttu-id="1634b-136">["Wwwroot" /\* \* ist ein Muster Globmodus](https://gruntjs.com/configuring-tasks#globbing-patterns) , die angibt, dass alle Dateien in der *"Wwwroot"* Ordner **und** Unterordner.</span><span class="sxs-lookup"><span data-stu-id="1634b-136">[wwwroot/\*\* is a globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) that specifies all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="1634b-137">Um eine Datei explizit der Veröffentlichungsliste hinzuzufügen, fügen Sie die Datei wie in [Einfügen von Dateien](#including-files) gezeigt direkt der *CSPROJ*-Datei hinzu.</span><span class="sxs-lookup"><span data-stu-id="1634b-137">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Including Files](#including-files).</span></span>

<span data-ttu-id="1634b-138">Bei der Auswahl der **veröffentlichen** Schaltfläche in Visual Studio oder beim Veröffentlichen von der Befehlszeile aus:</span><span class="sxs-lookup"><span data-stu-id="1634b-138">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="1634b-139">Die Eigenschaften und Elemente werden berechnet (die Dateien, die für den Build benötigt werden).</span><span class="sxs-lookup"><span data-stu-id="1634b-139">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="1634b-140">Nur Visual Studio: NuGet-Pakete werden wiederhergestellt.</span><span class="sxs-lookup"><span data-stu-id="1634b-140">Visual Studio only: NuGet packages are restored.</span></span> <span data-ttu-id="1634b-141">(Die Wiederherstellung muss explizit vom Benutzer auf der CLI durchgeführt werden.)</span><span class="sxs-lookup"><span data-stu-id="1634b-141">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="1634b-142">Das Projekt wird erstellt.</span><span class="sxs-lookup"><span data-stu-id="1634b-142">The project builds.</span></span>
* <span data-ttu-id="1634b-143">Die zu veröffentlichenden Elemente werden berechnet (die Dateien, die für die Veröffentlichung benötigt werden).</span><span class="sxs-lookup"><span data-stu-id="1634b-143">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="1634b-144">Das Projekt wird veröffentlicht.</span><span class="sxs-lookup"><span data-stu-id="1634b-144">The project is published.</span></span> <span data-ttu-id="1634b-145">(Die berechneten Dateien werden in das Veröffentlichungsziel kopiert.)</span><span class="sxs-lookup"><span data-stu-id="1634b-145">(The computed files are copied to the publish destination.)</span></span>

<span data-ttu-id="1634b-146">Wenn eine ASP.NET Core-Projekt verweist auf `Microsoft.NET.Sdk.Web` in der Projektdatei ein *app_offline.htm* Datei befindet sich im Stammverzeichnis des Verzeichnisses der Web-app.</span><span class="sxs-lookup"><span data-stu-id="1634b-146">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="1634b-147">Wenn die Datei vorhanden ist, fährt das ASP.NET Core Module die App ordnungsgemäß herunter und verarbeitet die Datei *app_offline.htm* während der Bereitstellung.</span><span class="sxs-lookup"><span data-stu-id="1634b-147">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="1634b-148">Weitere Informationen finden Sie unter [Konfigurationsreferenz für das ASP.NET Core-Modul](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span><span class="sxs-lookup"><span data-stu-id="1634b-148">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="1634b-149">Grundlegende Befehlszeile veröffentlichen</span><span class="sxs-lookup"><span data-stu-id="1634b-149">Basic command-line publishing</span></span>

<span data-ttu-id="1634b-150">Befehlszeilen-Veröffentlichung funktioniert auf allen .NET Core unterstützten Plattformen und erfordert nicht das Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1634b-150">Command-line publishing works on all .NET Core supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="1634b-151">In den untenstehenden Beispielen wird der Befehl `dotnet publish` aus dem Projektverzeichnis (das die *CSPROJ*-Datei enthält) ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="1634b-151">In the samples below, the `dotnet publish` command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="1634b-152">Wenn dies nicht explizit in den Projektordner übergeben, in der Projektdateipfad.</span><span class="sxs-lookup"><span data-stu-id="1634b-152">If not in the project folder, explicitly pass in the project file path.</span></span> <span data-ttu-id="1634b-153">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="1634b-153">For example:</span></span>

```console
dotnet publish c:/webs/web1
```

<span data-ttu-id="1634b-154">Führen Sie die folgenden Befehle zum Erstellen und Veröffentlichen eine Web-App aus:</span><span class="sxs-lookup"><span data-stu-id="1634b-154">Run the following commands to create and publish a web app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1634b-155">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1634b-155">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1634b-156">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1634b-156">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

---

<span data-ttu-id="1634b-157">Durch `dotnet publish` wird eine Ausgabe erzeugt, die Folgender ähnelt:</span><span class="sxs-lookup"><span data-stu-id="1634b-157">The `dotnet publish` produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="1634b-158">Der Standardveröffentlichungsordner ist `bin\$(Configuration)\netcoreapp<version>\publish`.</span><span class="sxs-lookup"><span data-stu-id="1634b-158">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="1634b-159">Der Standardwert für `$(Configuration)` ist „Debuggen“.</span><span class="sxs-lookup"><span data-stu-id="1634b-159">The default for `$(Configuration)` is Debug.</span></span> <span data-ttu-id="1634b-160">Im obigen Beispiel ist `<TargetFramework>` `netcoreapp2.0`.</span><span class="sxs-lookup"><span data-stu-id="1634b-160">In the sample above, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="1634b-161">`dotnet publish -h` zeigt Hilfeinformationen zum Veröffentlichen an.</span><span class="sxs-lookup"><span data-stu-id="1634b-161">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="1634b-162">Der folgende Befehl gibt einen `Release`-Build und das Veröffentlichungsverzeichnis an:</span><span class="sxs-lookup"><span data-stu-id="1634b-162">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:/MyWebs/test
```

<span data-ttu-id="1634b-163">Die `dotnet publish` Befehl ruft die ruft MSBuild den `Publish` Ziel.</span><span class="sxs-lookup"><span data-stu-id="1634b-163">The `dotnet publish` command calls MSBuild which invokes the `Publish` target.</span></span> <span data-ttu-id="1634b-164">Übergeben von Parametern `dotnet publish` an MSBuild übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="1634b-164">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="1634b-165">Der Parameter `-c` wird der MSBuild-Eigenschaft `Configuration` zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="1634b-165">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="1634b-166">Der Parameter `-o` wird `OutputPath` zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="1634b-166">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="1634b-167">MSBuild-Eigenschaften können mithilfe einer der folgenden Formate übergeben werden:</span><span class="sxs-lookup"><span data-stu-id="1634b-167">MSBuild properties can be passed using either of the following formats:</span></span>

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="1634b-168">Der folgende Befehl veröffentlicht einen `Release`-Build auf einer Netzwerkfreigabe:</span><span class="sxs-lookup"><span data-stu-id="1634b-168">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="1634b-169">Die Netzwerkfreigabe wird durch führende Schrägstriche (*//r8/*) angegeben und funktioniert auf allen Plattformen, die von .NET Core unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="1634b-169">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="1634b-170">Versichern Sie sich, dass die veröffentlichte App für die Bereitstellung nicht ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="1634b-170">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="1634b-171">Die Dateien im Ordner *publish* sind gesperrt, wenn die App ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="1634b-171">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="1634b-172">Die Bereitstellung kann nicht erfolgen, da die gesperrten Dateien nicht kopiert werden können.</span><span class="sxs-lookup"><span data-stu-id="1634b-172">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="1634b-173">Veröffentlichungsprofile</span><span class="sxs-lookup"><span data-stu-id="1634b-173">Publish profiles</span></span>

<span data-ttu-id="1634b-174">In diesem Abschnitt wird Visual Studio 2017 und höher verwendet, um Veröffentlichungsprofile zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="1634b-174">This section uses Visual Studio 2017 and higher to create publishing profiles.</span></span> <span data-ttu-id="1634b-175">Nach der Erstellung ist die Veröffentlichung aus Visual Studio oder der Befehlszeile zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="1634b-175">Once created, publishing from Visual Studio or the command line is available.</span></span>

<span data-ttu-id="1634b-176">Veröffentlichungsprofile können den Veröffentlichungsprozess vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="1634b-176">Publish profiles can simplify the publishing process.</span></span> <span data-ttu-id="1634b-177">Mehrere Veröffentlichungsprofile können vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="1634b-177">Multiple publish profiles can exist.</span></span> <span data-ttu-id="1634b-178">Klicken Sie zum Erstellen eines Veröffentlichungsprofils in Visual Studio mit der rechten Maustaste auf das Projekt im Projektmappen-Explorer, und wählen Sie **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="1634b-178">To create a publish profile in Visual Studio, right-click on the project in Solution Explorer and select **Publish**.</span></span> <span data-ttu-id="1634b-179">Wählen Sie alternativ **veröffentlichen \<Projektname >** im Menü erstellen.</span><span class="sxs-lookup"><span data-stu-id="1634b-179">Alternatively, select **Publish \<project name>** from the build menu.</span></span> <span data-ttu-id="1634b-180">Die Registerkarte **Veröffentlichen** der Seite „Anwendungsfunktionen“ wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="1634b-180">The **Publish** tab of the application capacities page is displayed.</span></span> <span data-ttu-id="1634b-181">Wenn das Projekt kein Veröffentlichungsprofil enthält, wird die folgende Seite angezeigt:</span><span class="sxs-lookup"><span data-stu-id="1634b-181">If the project doesn't contain a publish profile, the following page is displayed:</span></span>

![Die Registerkarte "Veröffentlichen" auf der Anwendungsseite-Kapazitäten mit Azure "," IIS "," FTB "," Ordner mit Azure ausgewählt.](visual-studio-publish-profiles/_static/az.png)

<span data-ttu-id="1634b-184">Wenn **Ordner** ausgewählt ist, erstellt die Schaltfläche **Veröffentlichen** den Ordner „Veröffentlichungsprofil“ und veröffentlicht.</span><span class="sxs-lookup"><span data-stu-id="1634b-184">When **Folder** is selected, the **Publish** button creates a folder publish profile and publishes.</span></span>

![Die Registerkarte „**Veröffentlichen**“ der Seite „Anwendungsfunktionen“, die Azure, IIS, FTB und Ordner anzeigt.](visual-studio-publish-profiles/_static/pub1.png)

<span data-ttu-id="1634b-186">Sobald ein Veröffentlichungsprofil erstellt wurde, die **veröffentlichen** Änderungen, und wählen Sie **neues Profil erstellen** um ein neues Profil zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="1634b-186">Once a publish profile is created, the **Publish** tab changes, and select **Create new profile** to create a new profile.</span></span>

![Die Registerkarte „**Veröffentlichen**“ der Seite „Anwendungsfunktionen“, die das im letzten Schritt erstellte „FolderProfile“ und den Link „Neues Profil erstellen“ anzeigt.](visual-studio-publish-profiles/_static/create_new.png)

<span data-ttu-id="1634b-188">Der Webpublishing-Assistent unterstützt folgende Veröffentlichungsziele:</span><span class="sxs-lookup"><span data-stu-id="1634b-188">The Publish wizard supports the following publish targets:</span></span>

* <span data-ttu-id="1634b-189">Microsoft Azure App Service</span><span class="sxs-lookup"><span data-stu-id="1634b-189">Microsoft Azure App Service</span></span>
* <span data-ttu-id="1634b-190">IIS, FTP usw. (für alle Webserver)</span><span class="sxs-lookup"><span data-stu-id="1634b-190">IIS, FTP, etc (for any web server)</span></span>
* <span data-ttu-id="1634b-191">Ordner</span><span class="sxs-lookup"><span data-stu-id="1634b-191">Folder</span></span>
* <span data-ttu-id="1634b-192">Der Profil importieren (Import des Benutzerprofils ermöglicht).</span><span class="sxs-lookup"><span data-stu-id="1634b-192">Import profile (allows profile import).</span></span>
* <span data-ttu-id="1634b-193">Microsoft Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="1634b-193">Microsoft Azure Virtual Machines</span></span>

<span data-ttu-id="1634b-194">Weitere Informationen finden Sie unter [What publishing options are right for me? (Welche Optionen für die Veröffentlichung sind für mich geeignet?)](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options)</span><span class="sxs-lookup"><span data-stu-id="1634b-194">See [What publishing options are right for me?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options) for more information.</span></span>

<span data-ttu-id="1634b-195">Beim Erstellen eines Veröffentlichungsprofils mit Visual Studio eine *Eigenschaften/PublishProfiles/\<Veröffentlichungsname > pubxml* MSBuild-Datei wird erstellt.</span><span class="sxs-lookup"><span data-stu-id="1634b-195">When creating a publish profile with Visual Studio, a *Properties/PublishProfiles/\<publish name>.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="1634b-196">Diese *PUBXML*-Datei ist eine MSBuild-Datei und enthält die Konfigurationseinstellungen für die Veröffentlichung.</span><span class="sxs-lookup"><span data-stu-id="1634b-196">This *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="1634b-197">Diese Datei kann geändert werden, um den Build anpassen und Veröffentlichen von Prozess.</span><span class="sxs-lookup"><span data-stu-id="1634b-197">This file can be changed to customize the build and publish process.</span></span> <span data-ttu-id="1634b-198">Diese Datei wird vom Veröffentlichungsprozess gelesen.</span><span class="sxs-lookup"><span data-stu-id="1634b-198">This file is read by the publishing process.</span></span> <span data-ttu-id="1634b-199">`<LastUsedBuildConfiguration>`ist ein Sonderfall, da er eine globale Eigenschaft und sollte nicht in jeder Datei, die im Build importiert werden.</span><span class="sxs-lookup"><span data-stu-id="1634b-199">`<LastUsedBuildConfiguration>` is special because it's a global property and shouldn't be in any file that's imported in the build.</span></span> <span data-ttu-id="1634b-200">Weitere Informationen finden Sie unter [MSBuild: how to set the configuration property (MSBuild: Festlegen der Konfigurationseigenschaft)](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx).</span><span class="sxs-lookup"><span data-stu-id="1634b-200">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more info.</span></span>
<span data-ttu-id="1634b-201">Die *pubxml* Datei darf nicht in die quellcodeverwaltung überprüft werden, da er abhängt der *User* Datei.</span><span class="sxs-lookup"><span data-stu-id="1634b-201">The *.pubxml* file shouldn't be checked into source control because it depends on the *.user* file.</span></span> <span data-ttu-id="1634b-202">Die *USER*-Datei sollte niemals in die Quellcodeverwaltung eingecheckt sein, da sie vertrauliche Informationen enthalten kann und nur für einen Benutzer und einen Computer gültig ist.</span><span class="sxs-lookup"><span data-stu-id="1634b-202">The *.user* file should never be checked into source control because it can contain sensitive information and it's only valid for one user and machine.</span></span>

<span data-ttu-id="1634b-203">Vertrauliche Informationen (z.B. das Verschlüsselungskennwort) werden auf einer Ebene pro Benutzer/Computer verschlüsselt und in der Datei *Properties/PublishProfiles/\<publish name>.pubxml.user* gespeichert.</span><span class="sxs-lookup"><span data-stu-id="1634b-203">Sensitive information (like the publish password) is encrypted on a per user/machine level and stored in the *Properties/PublishProfiles/\<publish name>.pubxml.user* file.</span></span> <span data-ttu-id="1634b-204">Da diese Datei vertrauliche Informationen enthalten kann, sollte sie **nicht** in die Quellverwaltung eingecheckt werden.</span><span class="sxs-lookup"><span data-stu-id="1634b-204">Because this file can contain sensitive information, it should **not** be checked into source control.</span></span>

<span data-ttu-id="1634b-205">Einen Überblick über die Vorgehensweise beim Veröffentlichen einer Web-app auf ASP.NET Core finden Sie unter [Host und Bereitstellen von](index.md).</span><span class="sxs-lookup"><span data-stu-id="1634b-205">For an overview of how to publish a web app on ASP.NET Core see [Host and deploy](index.md).</span></span> <span data-ttu-id="1634b-206">[Hosten und Bereitstellen von](index.md) ist ein open-Source-Projekt am https://github.com/aspnet/websdk.</span><span class="sxs-lookup"><span data-stu-id="1634b-206">[Host and deploy](index.md) is an open source project at https://github.com/aspnet/websdk.</span></span>

<span data-ttu-id="1634b-207">`dotnet publish`können MSDeploy, Ordner und [KUDU](https://github.com/projectkudu/kudu/wiki) Veröffentlichungsprofile:</span><span class="sxs-lookup"><span data-stu-id="1634b-207">`dotnet publish` can use folder, MSDeploy, and [KUDU](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>
 
<span data-ttu-id="1634b-208">Ordner (funktioniert plattformübergreifende):`dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`</span><span class="sxs-lookup"><span data-stu-id="1634b-208">Folder (works cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`</span></span>

<span data-ttu-id="1634b-209">MSDeploy (derzeit dieser funktioniert nur unter Windows seit MSDeploy plattformübergreifende ist nicht):`dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`</span><span class="sxs-lookup"><span data-stu-id="1634b-209">MSDeploy (currently this only works in windows since MSDeploy isn't cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`</span></span>

<span data-ttu-id="1634b-210">MSDeploy-Paket (derzeit dieser funktioniert nur unter Windows seit MSDeploy plattformübergreifende ist nicht):`dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`</span><span class="sxs-lookup"><span data-stu-id="1634b-210">MSDeploy package(currently this only works in windows since MSDeploy isn't cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`</span></span>

<span data-ttu-id="1634b-211">In den vorherigen Beispielen **nicht** übergeben `deployonbuild` auf `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="1634b-211">In the preceeding samples, **don't** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="1634b-212">Weitere Informationen finden Sie unter [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span><span class="sxs-lookup"><span data-stu-id="1634b-212">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="1634b-213">`dotnet publish` unterstützt KUDU-APIs zur Veröffentlichung in Azure von jeder Plattform aus.</span><span class="sxs-lookup"><span data-stu-id="1634b-213">`dotnet publish` supports KUDU apis to publish to Azure from any platform.</span></span> <span data-ttu-id="1634b-214">Visual Studio veröffentlichen unterstützt die KUDU-APIs, aber es wird unterstützt von Websdk für Cross-Plattform in Azure veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="1634b-214">Visual Studio publish does support the KUDU APIs but it's supported by websdk for cross plat publish to Azure.</span></span>

<span data-ttu-id="1634b-215">Fügen Sie dem Ordner *Properties/PublishProfiles* ein Veröffentlichungsprofil mit folgendem Inhalt hinzu:</span><span class="sxs-lookup"><span data-stu-id="1634b-215">Add a publish profile to *Properties/PublishProfiles* folder with the following content:</span></span>

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

<span data-ttu-id="1634b-216">Ausführen des folgenden Befehls komprimiert, um die Inhalte veröffentlichen und mithilfe der KUDU-APIs in Azure zu veröffentlichen:</span><span class="sxs-lookup"><span data-stu-id="1634b-216">Running the following command zips up the publish contents and publish it to Azure using the KUDU APIs:</span></span>

`dotnet publish /p:PublishProfile=Azure /p:Configuration=Release`

<span data-ttu-id="1634b-217">Legen Sie die folgenden MSBuild-Eigenschaften fest, wenn Sie ein Veröffentlichungsprofil verwenden:</span><span class="sxs-lookup"><span data-stu-id="1634b-217">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

<span data-ttu-id="1634b-218">Bei der Veröffentlichung mit einem Profil mit dem Namen *FolderProfile*, einen der folgenden Befehle können ausgeführt werden:</span><span class="sxs-lookup"><span data-stu-id="1634b-218">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="1634b-219">Beim Aufrufen von `dotnet build`, ruft er `msbuild` führen Sie den Build, und Veröffentlichen von Prozess.</span><span class="sxs-lookup"><span data-stu-id="1634b-219">When invoking `dotnet build`, it calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="1634b-220">Aufrufen von `dotnet build` oder `msbuild` entspricht im Wesentlichen bei der Übergabe in einem Ordner-Profil.</span><span class="sxs-lookup"><span data-stu-id="1634b-220">Calling `dotnet build` or `msbuild` is essentially equivalent when passing in a folder profile.</span></span> <span data-ttu-id="1634b-221">Beim Aufrufen von MSBuild direkt unter Windows wird die .NET Framework-Version von MSBuild verwendet.</span><span class="sxs-lookup"><span data-stu-id="1634b-221">When calling MSBuild directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="1634b-222">Die Veröffentlichung mit MSDeploy ist aktuell auf Windows-Computer beschränkt.</span><span class="sxs-lookup"><span data-stu-id="1634b-222">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="1634b-223">Wenn Sie `dotnet build` von einem Profil ohne Ordner aufrufen, wird MSBuild aufgerufen. MSBuild verwendet MSDeploy für Profile ohne Ordner.</span><span class="sxs-lookup"><span data-stu-id="1634b-223">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="1634b-224">Wenn Sie `dotnet build` von einem Profil ohne Ordner aufrufen, wird MSBuild (mithilfe von MSDeploy) aufgerufen. Dies führt zu einem Fehler (auch beim Ausführen auf einer Windows-Plattform).</span><span class="sxs-lookup"><span data-stu-id="1634b-224">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="1634b-225">Rufen Sie MSBuild direkt auf, um mit einem Profil ohne Ordner zu veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="1634b-225">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="1634b-226">Der folgende Ordner „Veröffentlichungsprofil“ wurde mit Visual Studio erstellt und veröffentlicht in eine Netzwerkfreigabe.</span><span class="sxs-lookup"><span data-stu-id="1634b-226">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

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

<span data-ttu-id="1634b-227">Beachten Sie, dass `<LastUsedBuildConfiguration>` auf `Release` festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="1634b-227">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span> <span data-ttu-id="1634b-228">Bei der Veröffentlichung von Visual Studio wird der `<LastUsedBuildConfiguration>`-Wert für die Konfigurationseigenschaft mithilfe des Werts festgelegt, der beim Starten des Veröffentlichungsprozesses vorliegt.</span><span class="sxs-lookup"><span data-stu-id="1634b-228">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="1634b-229">Die `<LastUsedBuildConfiguration>` Konfigurationseigenschaft Sonderregeln und sollte nicht in einer importierten MSBuild-Datei überschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="1634b-229">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="1634b-230">Diese Eigenschaft kann über die Befehlszeile überschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="1634b-230">This property can be overridden from the command line.</span></span> <span data-ttu-id="1634b-231">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="1634b-231">For example:</span></span>

`dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="1634b-232">Verwenden von MSBuild:</span><span class="sxs-lookup"><span data-stu-id="1634b-232">Using MSBuild:</span></span>

`msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="1634b-233">Veröffentlichen auf einen MSDeploy-Endpunkt von der Befehlszeile</span><span class="sxs-lookup"><span data-stu-id="1634b-233">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="1634b-234">Wie bereits erwähnt, Veröffentlichung kann erreicht werden, mithilfe von `dotnet publish` oder `msbuild` Befehl.</span><span class="sxs-lookup"><span data-stu-id="1634b-234">As previously mentioned, publishing can be accomplished using `dotnet publish` or the `msbuild` command.</span></span> <span data-ttu-id="1634b-235">`dotnet publish` wird im Kontext von .NET Core ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="1634b-235">`dotnet publish` runs in the context of .NET Core.</span></span> <span data-ttu-id="1634b-236">Die `msbuild` Befehl erfordert mindestens .NET Framework und ist daher auf Windows-Umgebungen beschränkt.</span><span class="sxs-lookup"><span data-stu-id="1634b-236">The `msbuild` command requires .NET framework, and is therefore limited to Windows environments.</span></span>

<span data-ttu-id="1634b-237">Die einfachste Möglichkeit zum Veröffentlichen mit MSDeploy ist die, zunächst ein Veröffentlichungsprofil in Visual Studio 2017 zu erstellen und das Profil von der Befehlszeile aus zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="1634b-237">The easiest way to publish with MSDeploy is to first create a publish profile in Visual Studio 2017 and use the profile from the command line.</span></span>

<span data-ttu-id="1634b-238">Im folgenden Beispiel wird eine ASP.NET Core-Web-app erstellt (mit `dotnet new mvc`), und ein Azure-Veröffentlichungsprofil mit Visual Studio hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="1634b-238">In the following sample, an ASP.NET Core web app is created (using `dotnet new mvc`), and an Azure publish profile is added with Visual Studio.</span></span>

<span data-ttu-id="1634b-239">Führen Sie `msbuild` aus einem **Developer-Eingabeaufforderung für VS 2017**.</span><span class="sxs-lookup"><span data-stu-id="1634b-239">Run `msbuild` from a **Developer Command Prompt for VS 2017**.</span></span> <span data-ttu-id="1634b-240">Die Developer-Eingabeaufforderung hat den richtigen *msbuild.exe* in der Pfadangabe mit einem Satz der MSBuild-Variablen.</span><span class="sxs-lookup"><span data-stu-id="1634b-240">The Developer Command Prompt has the correct *msbuild.exe* in its path with some MSBuild variables set.</span></span>

<span data-ttu-id="1634b-241">MSBuild verwendet die folgende Syntax:</span><span class="sxs-lookup"><span data-stu-id="1634b-241">MSBuild uses the following syntax:</span></span>

`msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>`

<span data-ttu-id="1634b-242">Abrufen der `Password` aus der  *\<Publish-Name >. PublishSettings* Datei.</span><span class="sxs-lookup"><span data-stu-id="1634b-242">Get the `Password` from the *\<Publish name>.PublishSettings* file.</span></span> <span data-ttu-id="1634b-243">Herunterladen der *. PublishSettings* Datei aus:</span><span class="sxs-lookup"><span data-stu-id="1634b-243">Download the *.PublishSettings* file from either:</span></span>

* <span data-ttu-id="1634b-244">Projektmappen-Explorer: Mit der rechten Maustaste auf die Web-App, und wählen Sie **Veröffentlichungsprofil herunterladen**.</span><span class="sxs-lookup"><span data-stu-id="1634b-244">Solution Explorer: Right-click on the Web App and select **Download Publish Profile**.</span></span>
* <span data-ttu-id="1634b-245">Im Azure-Verwaltungsportal: Wählen Sie **Get Veröffentlichungsprofil** aus dem Blatt "Web-App".</span><span class="sxs-lookup"><span data-stu-id="1634b-245">The Azure Management Portal: Select **Get publish profile** from the Web App blade.</span></span>

<span data-ttu-id="1634b-246">`Username` kann im Veröffentlichungsprofil gefunden werden.</span><span class="sxs-lookup"><span data-stu-id="1634b-246">`Username` can be found in the publish profile.</span></span>

<span data-ttu-id="1634b-247">Das folgende Beispiel verwendet das Veröffentlichungsprofil „Web11112 – Web Deploy“:</span><span class="sxs-lookup"><span data-stu-id="1634b-247">The following sample uses the "Web11112 - Web Deploy" publish profile:</span></span>

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="excluding-files"></a><span data-ttu-id="1634b-248">Ausschließen von Dateien</span><span class="sxs-lookup"><span data-stu-id="1634b-248">Excluding files</span></span>

<span data-ttu-id="1634b-249">Beim Veröffentlichen von ASP.NET Core-Apps sind die Buildartefakte und die Inhalte des Ordners *wwwroot* enthalten.</span><span class="sxs-lookup"><span data-stu-id="1634b-249">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="1634b-250">`msbuild` unterstützt [Globmuster](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="1634b-250">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="1634b-251">Beispielsweise die folgenden `<Content>` Elementknoten dem Elementmarkup schließt den gesamten Text (*".txt"*)-Dateien aus der *"Wwwroot" / Content* Ordner und allen Unterordnern.</span><span class="sxs-lookup"><span data-stu-id="1634b-251">For example, the following `<Content>` element markup excludes all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="1634b-252">Das obenstehende Markup kann zu einem Veröffentlichungsprofil oder der *CSPROJ*-Datei hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="1634b-252">The markup above can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="1634b-253">Wenn es zu der *CSPROJ*-Datei hinzugefügt wird, wird die Regel zu allen Veröffentlichungsprofilen im Projekt hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="1634b-253">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="1634b-254">Das folgende `<MsDeploySkipRules>`-Elementmarkup schließt alle Dateien aus dem Ordner *wwwroot/content* aus:</span><span class="sxs-lookup"><span data-stu-id="1634b-254">The following `<MsDeploySkipRules>` element markup exludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="1634b-255">`<MsDeploySkipRules>`Löschen wird nicht die *überspringen* Ziele vom Bereitstellungsstandort.</span><span class="sxs-lookup"><span data-stu-id="1634b-255">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="1634b-256">`<Content>`Ziel-Dateien und Ordner werden vom Bereitstellungsstandort gelöscht.</span><span class="sxs-lookup"><span data-stu-id="1634b-256">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="1634b-257">Nehmen Sie beispielsweise an, dass eine bereitgestellte Web-app die folgenden Dateien wurde:</span><span class="sxs-lookup"><span data-stu-id="1634b-257">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="1634b-258">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="1634b-258">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="1634b-259">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="1634b-259">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="1634b-260">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="1634b-260">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="1634b-261">Wenn die folgenden `<MsDeploySkipRules>` Markup hinzugefügt wird, wird diese Dateien wäre nicht am Bereitstellungsstandort gelöscht werden.</span><span class="sxs-lookup"><span data-stu-id="1634b-261">If the following `<MsDeploySkipRules>` markup is added, those files wouldn't be deleted on the deployment site.</span></span>

``` xml
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

<span data-ttu-id="1634b-262">Die `<MsDeploySkipRules>` Markup oben gezeigten wird verhindert, dass die *übersprungen* Dateien nicht Depoyed, aber diese Dateien wird nicht löschen, nachdem sie bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="1634b-262">The `<MsDeploySkipRules>` markup shown above prevents the *skipped* files from being depoyed but won't delete those files once they're deployed.</span></span>

<span data-ttu-id="1634b-263">Die folgenden `<Content>` Markup löscht Zieldateien am Bereitstellungsstandort:</span><span class="sxs-lookup"><span data-stu-id="1634b-263">The following `<Content>` markup deletes the targeted files at the deployment site:</span></span>

``` xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="1634b-264">Verwenden die Bereitstellung über die Befehlszeile mit der `<Content>` Markup oben in der Ausgabe ähnlich der folgenden Ergebnisse:</span><span class="sxs-lookup"><span data-stu-id="1634b-264">Using command-line deployment with the `<Content>` markup above results in output similar to the following:</span></span>

``` console
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

## <a name="including-files"></a><span data-ttu-id="1634b-265">Einfügen von Dateien</span><span class="sxs-lookup"><span data-stu-id="1634b-265">Including files</span></span>

<span data-ttu-id="1634b-266">Das folgende Markup enthält ein *Bilder* Ordner außerhalb des Projektverzeichnisses auf die *"Wwwroot"-Images* Ordner der Website veröffentlichen:</span><span class="sxs-lookup"><span data-stu-id="1634b-266">The following markup includes an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site:</span></span>

``` xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="1634b-267">Das Markup kann der *CSPROJ*-Datei oder dem Veröffentlichungsprofil hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="1634b-267">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="1634b-268">Wenn es hinzugefügt wird die *csproj* -Datei, die sie in jedes Veröffentlichungsprofil im Projekt enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="1634b-268">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>

<span data-ttu-id="1634b-269">Das folgende hervorgehobene Markup zeigt Folgendes:</span><span class="sxs-lookup"><span data-stu-id="1634b-269">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="1634b-270">Kopieren einer Datei von außerhalb des Projekts in den Ordner *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="1634b-270">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="1634b-271">Ausschließen des Ordners *wwwroot\Content*.</span><span class="sxs-lookup"><span data-stu-id="1634b-271">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="1634b-272">Ausschließen von *Views\Home\About2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1634b-272">Exclude *Views\Home\About2.cshtml*.</span></span>

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

<span data-ttu-id="1634b-273">Weitere Bereitstellungsbeispiele finden Sie unter [WebSDK Readme (WebSDK-Infodatei)](https://github.com/aspnet/websdk).</span><span class="sxs-lookup"><span data-stu-id="1634b-273">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

### <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="1634b-274">Führen Sie ein Ziel vor oder nach der Veröffentlichung aus.</span><span class="sxs-lookup"><span data-stu-id="1634b-274">Run a target before or after publishing</span></span>

<span data-ttu-id="1634b-275">Die integrierte `BeforePublish` und `AfterPublish` Ziele können verwendet werden, um ein Ziel vor oder nach dem Veröffentlichungsziel auszuführen.</span><span class="sxs-lookup"><span data-stu-id="1634b-275">The built-in `BeforePublish` and `AfterPublish` targets can be used to execute a target before or after the publish target.</span></span> <span data-ttu-id="1634b-276">Das folgende Markup kann dem Veröffentlichungsprofil hinzugefügt werden, um Nachrichten an die Konsolenausgabe vor und nach der Veröffentlichung zu protokollieren:</span><span class="sxs-lookup"><span data-stu-id="1634b-276">The following markup can be added to the publish profile to log messages to the console output before and after publishing:</span></span>

``` xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="the-kudu-service"></a><span data-ttu-id="1634b-277">Der Kudu-Dienst</span><span class="sxs-lookup"><span data-stu-id="1634b-277">The Kudu service</span></span>

<span data-ttu-id="1634b-278">Zum Anzeigen der Dateien in der ein Azure-Apps Service Web-app-Bereitstellung, verwenden die [Kudu Service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="1634b-278">To view the files in the an Azure Apps Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="1634b-279">Anfügen der `scm` -Token genutzt, um den Namen der Web-app.</span><span class="sxs-lookup"><span data-stu-id="1634b-279">Append the `scm` token to the name of the web app.</span></span> <span data-ttu-id="1634b-280">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="1634b-280">For example:</span></span>

| <span data-ttu-id="1634b-281">URL</span><span class="sxs-lookup"><span data-stu-id="1634b-281">URL</span></span>                                    | <span data-ttu-id="1634b-282">Ergebnis</span><span class="sxs-lookup"><span data-stu-id="1634b-282">Result</span></span>      |
| -------------------------------------- | ----------- |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="1634b-283">Webanwendung</span><span class="sxs-lookup"><span data-stu-id="1634b-283">Web App</span></span>     |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="1634b-284">Kudu-Dienst</span><span class="sxs-lookup"><span data-stu-id="1634b-284">Kudu sevice</span></span> |

<span data-ttu-id="1634b-285">Klicken Sie auf das Menüelement [Debugging-Konsole](https://github.com/projectkudu/kudu/wiki/Kudu-console), um Dateien anzuzeigen, zu bearbeiten, zu löschen oder hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="1634b-285">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view/edit/delete/add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1634b-286">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="1634b-286">Additional resources</span></span>

* <span data-ttu-id="1634b-287">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) vereinfacht die Bereitstellung der Web-apps und Websites so, dass IIS-Servern.</span><span class="sxs-lookup"><span data-stu-id="1634b-287">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="1634b-288">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): Probleme melden und Features für die Bereitstellung anfordern.</span><span class="sxs-lookup"><span data-stu-id="1634b-288">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
