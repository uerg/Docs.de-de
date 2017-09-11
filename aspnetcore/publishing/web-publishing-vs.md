---
title: "Erstellen von Veröffentlichungsprofilen für Visual Studio und MSBuild"
author: rick-anderson
description: "Erläutert das Webpublishing in Visual Studio."
keywords: "ASP.NET Core, Webpublishing, Veröffentlichung, MSBuild, Webbereitstellung, dotnet publish, Visual Studio 2017"
ms.author: riande
manager: wpickett
ms.date: 03/14/2017
ms.topic: article
ms.assetid: 0377a02d-8fda-47a5-929a-24a16e1d2c93
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/web-publishing-vs
ms.openlocfilehash: 872e8c99734a0913770281d9d87b1e30c250ab0a
ms.sourcegitcommit: bd05f7ea8f87ad076ef6e8b704698ebcba5ca80c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2017
---
# <a name="create-publish-profiles-for-visual-studio-and-msbuild-to-deploy-aspnet-core-apps"></a><span data-ttu-id="223a5-104">Erstellen von Veröffentlichungsprofilen für Visual Studio und MSBuild zum Bereitstellen von ASP.NET Core-Apps</span><span class="sxs-lookup"><span data-stu-id="223a5-104">Create publish profiles for Visual Studio and MSBuild, to deploy ASP.NET Core apps</span></span>

<span data-ttu-id="223a5-105">Von [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="223a5-105">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="223a5-106">Dieser Artikel konzertiert sich auf die Verwendung von Visual Studio 2017 zum Erstellen von Veröffentlichungsprofilen.</span><span class="sxs-lookup"><span data-stu-id="223a5-106">This article focuses on using Visual Studio 2017 to create publish profiles.</span></span> <span data-ttu-id="223a5-107">Die Veröffentlichungsprofile, die mit Visual Studio erstellt werden, können von MSBuild und Visual Studio 2017 ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="223a5-107">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio 2017.</span></span>

<span data-ttu-id="223a5-108">Die folgende *CSPROJ*-Datei wurde mit dem Befehl `dotnet new mvc` erstellt:</span><span class="sxs-lookup"><span data-stu-id="223a5-108">The following *.csproj* file was created with the command `dotnet new mvc`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="223a5-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="223a5-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="223a5-110">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="223a5-110">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.1" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.2" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.1" />
  </ItemGroup>

</Project>
```

---

<span data-ttu-id="223a5-111">Das `Sdk`-Attribut im `<Project>`-Element (in der ersten Zeile) des obenstehenden Markups bewirkt Folgendes:</span><span class="sxs-lookup"><span data-stu-id="223a5-111">The `Sdk` attribute in the `<Project>` element (in the first line) of the markup above does the following:</span></span>

* <span data-ttu-id="223a5-112">Es importiert am Anfang die `props`-Datei von *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props*.</span><span class="sxs-lookup"><span data-stu-id="223a5-112">Imports the `props` file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="223a5-113">Es importiert am Ende die Zieledatei von *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets*.</span><span class="sxs-lookup"><span data-stu-id="223a5-113">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="223a5-114">Der Standardspeicherort für `MSBuildSDKsPath` (mit Visual Studio 2017 Enterprise) ist der Ordner *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks*.</span><span class="sxs-lookup"><span data-stu-id="223a5-114">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="223a5-115">`Microsoft.NET.Sdk.Web` ist abhängig von:</span><span class="sxs-lookup"><span data-stu-id="223a5-115">`Microsoft.NET.Sdk.Web` depends on:</span></span>

* <span data-ttu-id="223a5-116">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="223a5-116">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="223a5-117">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="223a5-117">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="223a5-118">Dadurch werden folgende `props` und `targets` importiert:</span><span class="sxs-lookup"><span data-stu-id="223a5-118">Which causes the following `props` and `targets` to be imported:</span></span>

* <span data-ttu-id="223a5-119">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="223a5-119">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="223a5-120">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="223a5-120">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span></span>
* <span data-ttu-id="223a5-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="223a5-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="223a5-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="223a5-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span></span>

<span data-ttu-id="223a5-123">Durch „publish targets“ wird basierend auf der verwendeten „publish“-Methode die richtige Gruppe von Zielen importiert.</span><span class="sxs-lookup"><span data-stu-id="223a5-123">Publish targets will again import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="223a5-124">Wenn in MSBuild oder Visual Studio ein Projekt geladen wird, werden die folgenden Aktionen auf hoher Ebene ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="223a5-124">When MSBuild or Visual Studio loads a project, the following high level actions are performed:</span></span>

* <span data-ttu-id="223a5-125">Erstellen des Projekts</span><span class="sxs-lookup"><span data-stu-id="223a5-125">Build project</span></span>
* <span data-ttu-id="223a5-126">Berechnen der zu veröffentlichenden Dateien</span><span class="sxs-lookup"><span data-stu-id="223a5-126">Compute files to publish</span></span>
* <span data-ttu-id="223a5-127">Veröffentlichen der Dateien auf dem Ziel</span><span class="sxs-lookup"><span data-stu-id="223a5-127">Publish files to destination</span></span>

### <a name="compute-project-items"></a><span data-ttu-id="223a5-128">Berechnen der Projektelemente</span><span class="sxs-lookup"><span data-stu-id="223a5-128">Compute project items</span></span>

<span data-ttu-id="223a5-129">Wenn das Projekt geladen ist, werden die Projektelemente (Dateien) berechnet.</span><span class="sxs-lookup"><span data-stu-id="223a5-129">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="223a5-130">Die `item type`-Attribut bestimmt, wie die Datei verarbeitet wird.</span><span class="sxs-lookup"><span data-stu-id="223a5-130">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="223a5-131">Standardmäßig sind *CS*-Dateien in der `Compile`-Elementliste enthalten.</span><span class="sxs-lookup"><span data-stu-id="223a5-131">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="223a5-132">Dateien in der `Compile`-Elementliste werden kompiliert.</span><span class="sxs-lookup"><span data-stu-id="223a5-132">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="223a5-133">Die `Content`-Elementliste enthält Dateien, die zusätzlich zu den Buildausgaben veröffentlicht werden.</span><span class="sxs-lookup"><span data-stu-id="223a5-133">The `Content` item list contains files that will be published in addition to the build outputs.</span></span> <span data-ttu-id="223a5-134">Standardmäßig sind Dateien, die mit dem Muster „wwwroot/**“ übereinstimmen, im `Content`-Element enthalten.</span><span class="sxs-lookup"><span data-stu-id="223a5-134">By default, files matching the pattern wwwroot/** will be included in the `Content` item.</span></span> <span data-ttu-id="223a5-135">[wwwroot/** ist ein Globmuster](https://gruntjs.com/configuring-tasks#globbing-patterns), das alle Dateien im *wwwroot*-Ordner **und** in Unterordnern angibt.</span><span class="sxs-lookup"><span data-stu-id="223a5-135">[wwwroot/** is a globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) that specifies all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="223a5-136">Wenn Sie eine Datei explizit der Veröffentlichungsliste hinzufügen müssen, können Sie die Datei wie in [Einfügen von Dateien](#including-files) gezeigt direkt der *CSPROJ*-Datei hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="223a5-136">If you need to explicitly add a file to the publish list you can add the file directly in the *.csproj* file as shown in [Including Files](#including-files).</span></span>

<span data-ttu-id="223a5-137">Wenn Sie auf die Schaltfläche **Veröffentlichen** in Visual Studio klicken oder aus der Befehlszeile veröffentlichen:</span><span class="sxs-lookup"><span data-stu-id="223a5-137">When you select the **Publish** button in Visual Studio or when you publish from command line:</span></span>

- <span data-ttu-id="223a5-138">Die Eigenschaften und Elemente werden berechnet (die Dateien, die für den Build benötigt werden).</span><span class="sxs-lookup"><span data-stu-id="223a5-138">The properties/items are computed (the files that are needed to build).</span></span>
- <span data-ttu-id="223a5-139">Nur Visual Studio: NuGet-Pakete werden wiederhergestellt.</span><span class="sxs-lookup"><span data-stu-id="223a5-139">Visual Studio only: NuGet packages are restored.</span></span>  <span data-ttu-id="223a5-140">(Die Wiederherstellung muss explizit vom Benutzer auf der CLI durchgeführt werden.)</span><span class="sxs-lookup"><span data-stu-id="223a5-140">(Restore needs to be explicit by the user on the CLI.)</span></span>
- <span data-ttu-id="223a5-141">Das Projekt wird erstellt.</span><span class="sxs-lookup"><span data-stu-id="223a5-141">The project builds.</span></span>
- <span data-ttu-id="223a5-142">Die zu veröffentlichenden Elemente werden berechnet (die Dateien, die für die Veröffentlichung benötigt werden).</span><span class="sxs-lookup"><span data-stu-id="223a5-142">The publish items are computed (the files that are needed to publish).</span></span>
- <span data-ttu-id="223a5-143">Das Projekt wird veröffentlicht.</span><span class="sxs-lookup"><span data-stu-id="223a5-143">The project is published.</span></span> <span data-ttu-id="223a5-144">(Die berechneten Dateien werden in das Veröffentlichungsziel kopiert.)</span><span class="sxs-lookup"><span data-stu-id="223a5-144">(The computed files are copied to the publish destination.)</span></span>

## <a name="simple-command-line-publishing"></a><span data-ttu-id="223a5-145">Einfaches Veröffentlichen über die Befehlszeile</span><span class="sxs-lookup"><span data-stu-id="223a5-145">Simple command line publishing</span></span>

<span data-ttu-id="223a5-146">Dieser Abschnitt funktioniert auf allen von .NET Core unterstützten Plattformen und erfordert nicht Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="223a5-146">This section works on all .NET Core supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="223a5-147">In den untenstehenden Beispielen wird der Befehl `dotnet publish` aus dem Projektverzeichnis (das die *CSPROJ*-Datei enthält) ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="223a5-147">In the samples below, the `dotnet publish` command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="223a5-148">Wenn Sie nicht im Projektordner sind, können Sie explizit den Projektdateipfad übergeben.</span><span class="sxs-lookup"><span data-stu-id="223a5-148">If you're not in the project folder, you can explicitly pass in the project file path.</span></span> <span data-ttu-id="223a5-149">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="223a5-149">For example:</span></span>

```console
dotnet publish  c:/webs/web1
```

<span data-ttu-id="223a5-150">Führen Sie die folgenden Befehle zum Erstellen und Veröffentlichen eine Web-App aus:</span><span class="sxs-lookup"><span data-stu-id="223a5-150">Run the following commands to create and publish a web app:</span></span>

```console
dotnet new mvc
dotnet restore
dotnet publish
```

<span data-ttu-id="223a5-151">Durch `dotnet publish` wird eine Ausgabe erzeugt, die Folgender ähnelt:</span><span class="sxs-lookup"><span data-stu-id="223a5-151">The `dotnet publish` produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.1.548.43366
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp1.1\Web1.dll
```

<span data-ttu-id="223a5-152">Der Standardveröffentlichungsordner ist `bin\$(Configuration)\netcoreapp<version>\publish`.</span><span class="sxs-lookup"><span data-stu-id="223a5-152">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="223a5-153">Der Standardwert für `$(Configuration)` ist „Debuggen“.</span><span class="sxs-lookup"><span data-stu-id="223a5-153">The default for `$(Configuration)` is Debug.</span></span> <span data-ttu-id="223a5-154">Im obigen Beispiel ist `<TargetFramework>` `netcoreapp1.1`.</span><span class="sxs-lookup"><span data-stu-id="223a5-154">In the sample above, the `<TargetFramework>` is `netcoreapp1.1`.</span></span> <span data-ttu-id="223a5-155">Der tatsächliche Pfad im obigen Beispiel ist *bin\Debug\netcoreapp1.1\publish*.</span><span class="sxs-lookup"><span data-stu-id="223a5-155">The actual path in the sample above is *bin\Debug\netcoreapp1.1\publish*.</span></span>

<span data-ttu-id="223a5-156">`dotnet publish -h` zeigt Hilfeinformationen zum Veröffentlichen an.</span><span class="sxs-lookup"><span data-stu-id="223a5-156">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="223a5-157">Der folgende Befehl gibt einen `Release`-Build und das Veröffentlichungsverzeichnis an:</span><span class="sxs-lookup"><span data-stu-id="223a5-157">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:/MyWebs/test
```

<span data-ttu-id="223a5-158">Der Befehl `dotnet publish` ruft `MSBuild` auf, welches das `Publish`-Ziel aufruft.</span><span class="sxs-lookup"><span data-stu-id="223a5-158">The `dotnet publish` command calls `MSBuild` which invokes the `Publish` target.</span></span> <span data-ttu-id="223a5-159">Alle Parameter, die an `dotnet publish` übergeben werden, werden an `MSBuild` übergeben.</span><span class="sxs-lookup"><span data-stu-id="223a5-159">Any parameters passed to `dotnet publish` are passed to `MSBuild`.</span></span> <span data-ttu-id="223a5-160">Der Parameter `-c` wird der MSBuild-Eigenschaft `Configuration` zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="223a5-160">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="223a5-161">Der Parameter `-o` wird `OutputPath` zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="223a5-161">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="223a5-162">Sie können MSBuild-Eigenschaften mithilfe von einem der folgenden Formate übergeben:</span><span class="sxs-lookup"><span data-stu-id="223a5-162">You can pass MSBuild properties using either of the following formats:</span></span>

- ` p:<NAME>=<VALUE>`
- `/p:<NAME>=<VALUE>`

<span data-ttu-id="223a5-163">Der folgende Befehl veröffentlicht einen `Release`-Build auf einer Netzwerkfreigabe:</span><span class="sxs-lookup"><span data-stu-id="223a5-163">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="223a5-164">Die Netzwerkfreigabe wird durch führende Schrägstriche (*//r8/*) angegeben und funktioniert auf allen Plattformen, die von .NET Core unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="223a5-164">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="223a5-165">Versichern Sie sich, dass die veröffentlichte App für die Bereitstellung nicht ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="223a5-165">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="223a5-166">Die Dateien im Ordner *publish* sind gesperrt, wenn die App ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="223a5-166">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="223a5-167">Die Bereitstellung kann nicht erfolgen, da die gesperrten Dateien nicht kopiert werden können.</span><span class="sxs-lookup"><span data-stu-id="223a5-167">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="223a5-168">Veröffentlichungsprofile</span><span class="sxs-lookup"><span data-stu-id="223a5-168">Publish profiles</span></span>

<span data-ttu-id="223a5-169">In diesem Abschnitt wird Visual Studio 2017 und höher verwendet, um Veröffentlichungsprofile zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="223a5-169">This section uses Visual Studio 2017 and higher to create publishing profiles.</span></span> <span data-ttu-id="223a5-170">Nach dem Erstellen können Sie von Visual Studio oder der Befehlszeile veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="223a5-170">Once created, you can publish from Visual Studio or the command line.</span></span>

<span data-ttu-id="223a5-171">Veröffentlichungsprofile können den Veröffentlichungsprozess vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="223a5-171">Publish profiles can simplify the publishing process.</span></span> <span data-ttu-id="223a5-172">Sie können mehrere Veröffentlichungsprofile besitzen.</span><span class="sxs-lookup"><span data-stu-id="223a5-172">You can have multiple publish profiles.</span></span> <span data-ttu-id="223a5-173">Klicken Sie zum Erstellen eines Veröffentlichungsprofils in Visual Studio mit der rechten Maustaste auf das Projekt im Projektmappen-Explorer, und klicken Sie auf **Veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="223a5-173">To create a publish profile in Visual Studio, right click on the project in Solution Explore and select **Publish**.</span></span> <span data-ttu-id="223a5-174">Alternativ können Sie auf **Veröffentlichen\<Projektname>** im Buildmenü klicken.</span><span class="sxs-lookup"><span data-stu-id="223a5-174">Alternatively, you can select **Publish     \<project name>** from the build menu.</span></span> <span data-ttu-id="223a5-175">Die Registerkarte **Veröffentlichen** der Seite „Anwendungsfunktionen“ wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="223a5-175">The **Publish** tab of the application capacities page is displayed.</span></span> <span data-ttu-id="223a5-176">Wenn das Projekt kein Veröffentlichungsprofil enthält, wird die folgende Seite angezeigt:</span><span class="sxs-lookup"><span data-stu-id="223a5-176">If the project doesn't contain a publish profile, the following page is displayed:</span></span>

![Die Registerkarte „**Veröffentlichen**“ der Seite „Anwendungsfunktionen“, die Azure (ausgewählt), IIS, FTB und Ordner anzeigt.](web-publishing-vs/_static/az.png)

<span data-ttu-id="223a5-179">Wenn **Ordner** ausgewählt ist, erstellt die Schaltfläche **Veröffentlichen** den Ordner „Veröffentlichungsprofil“ und veröffentlicht.</span><span class="sxs-lookup"><span data-stu-id="223a5-179">When **Folder** is selected, the **Publish** button creates a folder publish profile and publishes.</span></span>

![Die Registerkarte „**Veröffentlichen**“ der Seite „Anwendungsfunktionen“, die Azure, IIS, FTB und Ordner anzeigt.](web-publishing-vs/_static/pub1.png)

<span data-ttu-id="223a5-181">Sobald ein Veröffentlichungsprofil erstellt ist, ändert sich die Registerkarte **Veröffentlichen**, und Sie können auf **Neues Profil erstellen** klicken, um ein neues Profil zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="223a5-181">Once a publish profile is created, the **Publish** tab changes, and you select **Create new profile** to create a new profile.</span></span>

![Die Registerkarte „**Veröffentlichen**“ der Seite „Anwendungsfunktionen“, die das im letzten Schritt erstellte „FolderProfile“ und den Link „Neues Profil erstellen“ anzeigt.](web-publishing-vs/_static/create_new.png)

<span data-ttu-id="223a5-183">Der Webpublishing-Assistent unterstützt folgende Veröffentlichungsziele:</span><span class="sxs-lookup"><span data-stu-id="223a5-183">The Publish wizard supports the following publish targets:</span></span>

- <span data-ttu-id="223a5-184">Microsoft Azure App Service</span><span class="sxs-lookup"><span data-stu-id="223a5-184">Microsoft Azure App Service</span></span>
- <span data-ttu-id="223a5-185">IIS, FTP usw. (für alle Webserver)</span><span class="sxs-lookup"><span data-stu-id="223a5-185">IIS, FTP, etc (for any web server)</span></span>
- <span data-ttu-id="223a5-186">Ordner</span><span class="sxs-lookup"><span data-stu-id="223a5-186">Folder</span></span>
- <span data-ttu-id="223a5-187">Profil importieren (ermöglicht Ihnen das Importieren eines Profils).</span><span class="sxs-lookup"><span data-stu-id="223a5-187">Import profile (allows you to import a profile).</span></span>
- <span data-ttu-id="223a5-188">Microsoft Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="223a5-188">Microsoft Azure Virtual Machines</span></span>

<span data-ttu-id="223a5-189">Weitere Informationen finden Sie unter [What publishing options are right for me? (Welche Optionen für die Veröffentlichung sind für mich geeignet?)](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options)</span><span class="sxs-lookup"><span data-stu-id="223a5-189">See [What publishing options are right for me?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options) for more information.</span></span>

<span data-ttu-id="223a5-190">Wenn Sie ein Veröffentlichungsprofil mit Visual Studio erstellen, wird eine *Properties/PublishProfiles/\<publish name>.pubxml*-MSBuild-Datei erstellt.</span><span class="sxs-lookup"><span data-stu-id="223a5-190">When you create a publish profile with Visual Studio, a *Properties/PublishProfiles/\<publish name>.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="223a5-191">Diese *PUBXML*-Datei ist eine MSBuild-Datei und enthält die Konfigurationseinstellungen für die Veröffentlichung.</span><span class="sxs-lookup"><span data-stu-id="223a5-191">This *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="223a5-192">Sie können diese Datei ändern, um den Build- und Veröffentlichungsprozess anzupassen.</span><span class="sxs-lookup"><span data-stu-id="223a5-192">You can change this file to customize the build and publish process.</span></span> <span data-ttu-id="223a5-193">Diese Datei wird vom Veröffentlichungsprozess gelesen.</span><span class="sxs-lookup"><span data-stu-id="223a5-193">This file is read by the publishing process.</span></span> <span data-ttu-id="223a5-194">`<LastUsedBuildConfiguration>` ist ein Sonderfall, da es sich dabei um eine globale Eigenschaft handelt, die nicht in einer Datei enthalten sein sollte, die in den Build importiert wurde.</span><span class="sxs-lookup"><span data-stu-id="223a5-194">`<LastUsedBuildConfiguration>` is special because it’s a global property and shouldn’t be in any file that’s imported in the build.</span></span> <span data-ttu-id="223a5-195">Weitere Informationen finden Sie unter [MSBuild: how to set the configuration property (MSBuild: Festlegen der Konfigurationseigenschaft)](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx).</span><span class="sxs-lookup"><span data-stu-id="223a5-195">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more info.</span></span>
<span data-ttu-id="223a5-196">Die *PUBXML*-Datei sollte nicht in die Quellcodeverwaltung eingecheckt sein, da sie von der *USER*-Datei abhängig ist.</span><span class="sxs-lookup"><span data-stu-id="223a5-196">The *.pubxml* file should not be checked into source control because it depends on the *.user* file.</span></span> <span data-ttu-id="223a5-197">Die *USER*-Datei sollte niemals in die Quellcodeverwaltung eingecheckt sein, da sie vertrauliche Informationen enthalten kann und nur für einen Benutzer und einen Computer gültig ist.</span><span class="sxs-lookup"><span data-stu-id="223a5-197">The *.user* file should never be checked into source control because it can contain sensitive information and it's only valid for one user and machine.</span></span>

<span data-ttu-id="223a5-198">Vertrauliche Informationen (z.B. das Verschlüsselungskennwort) werden auf einer Ebene pro Benutzer/Computer verschlüsselt und in der Datei *Properties/PublishProfiles/\<publish name>.pubxml.user* gespeichert.</span><span class="sxs-lookup"><span data-stu-id="223a5-198">Sensitive information (like the publish password) is encrypted on a per user/machine level and stored in the *Properties/PublishProfiles/\<publish name>.pubxml.user* file.</span></span> <span data-ttu-id="223a5-199">Da diese Datei vertrauliche Informationen enthalten kann, sollte sie **nicht** in die Quellverwaltung eingecheckt werden.</span><span class="sxs-lookup"><span data-stu-id="223a5-199">Because this file can contain sensitive information, it should **not** be checked into source control.</span></span>

<span data-ttu-id="223a5-200">Einen Überblick über das Veröffentlichen einer Web-App auf ASP.NET Core finden Sie unter [Publishing and Deployment (Veröffentlichung und Bereitstellung)](index.md).</span><span class="sxs-lookup"><span data-stu-id="223a5-200">For an overview of how to publish a web app on ASP.NET Core see [Publishing and Deployment](index.md).</span></span> <span data-ttu-id="223a5-201">Bei [Publishing and Deployment (Veröffentlichung und Bereitstellung)](index.md) handelt es sich um ein Open Source-Projekt auf https://github.com/aspnet/websdk.</span><span class="sxs-lookup"><span data-stu-id="223a5-201">[Publishing and Deployment](index.md) is an open source project at https://github.com/aspnet/websdk.</span></span>

<span data-ttu-id="223a5-202">Aktuell kann `dotnet publish` keine Veröffentlichungsprofile verwenden.</span><span class="sxs-lookup"><span data-stu-id="223a5-202">Currently `dotnet publish` doesn’t have the ability to use publish profiles.</span></span> <span data-ttu-id="223a5-203">Verwenden Sie `dotnet build` zum Verwenden von Veröffentlichungsprofilen.</span><span class="sxs-lookup"><span data-stu-id="223a5-203">To use publish profiles, use `dotnet build`.</span></span> <span data-ttu-id="223a5-204">`dotnet build` ruft MSBuild im Projekt auf.</span><span class="sxs-lookup"><span data-stu-id="223a5-204">`dotnet build` invokes MSBuild on the project.</span></span> <span data-ttu-id="223a5-205">Rufen Sie alternativ direkt `msbuild` auf.</span><span class="sxs-lookup"><span data-stu-id="223a5-205">Alternatively, call `msbuild` directly.</span></span>

<span data-ttu-id="223a5-206">Legen Sie die folgenden MSBuild-Eigenschaften fest, wenn Sie ein Veröffentlichungsprofil verwenden:</span><span class="sxs-lookup"><span data-stu-id="223a5-206">Set the following MSBuild properties when using a publish profile:</span></span>

- `DeployOnBuild=true`
- `PublishProfile=<Publish profile name>`

<span data-ttu-id="223a5-207">Beispielsweise können Sie einen der untenstehenden Befehle ausführen, wenn Sie mit einem Profil namens *FolderProfile* veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="223a5-207">For example, when publishing with a profile named *FolderProfile* you can execute either of the commands below.</span></span>

- `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
- `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="223a5-208">Wenn Sie `dotnet build` aufrufen, wird dieses `msbuild` aufrufen, um den Build- und Veröffentlichungsprozess auszuführen.</span><span class="sxs-lookup"><span data-stu-id="223a5-208">When you invoke `dotnet build` it will call `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="223a5-209">Das Aufrufen von `dotnet build` oder `msbuild` ist im Wesentlichen äquivalent, wenn Sie in ein Ordnerprofil übergeben.</span><span class="sxs-lookup"><span data-stu-id="223a5-209">Calling `dotnet build` or `msbuild` is essentially equivalent when you pass in a folder profile.</span></span> <span data-ttu-id="223a5-210">Wenn Sie MSBuild direkt in Windows aufrufen, erhalten Sie die .NET Framework-Version von MSBuild.</span><span class="sxs-lookup"><span data-stu-id="223a5-210">When calling MSBuild directly on Windows you get the .NET Framework version of MSBuild.</span></span>  <span data-ttu-id="223a5-211">Die Veröffentlichung mit MSDeploy ist aktuell auf Windows-Computer beschränkt.</span><span class="sxs-lookup"><span data-stu-id="223a5-211">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="223a5-212">Wenn Sie `dotnet build` von einem Profil ohne Ordner aufrufen, wird MSBuild aufgerufen. MSBuild verwendet MSDeploy für Profile ohne Ordner.</span><span class="sxs-lookup"><span data-stu-id="223a5-212">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="223a5-213">Wenn Sie `dotnet build` von einem Profil ohne Ordner aufrufen, wird MSBuild (mithilfe von MSDeploy) aufgerufen. Dies führt zu einem Fehler (auch beim Ausführen auf einer Windows-Plattform).</span><span class="sxs-lookup"><span data-stu-id="223a5-213">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="223a5-214">Rufen Sie MSBuild direkt auf, um mit einem Profil ohne Ordner zu veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="223a5-214">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="223a5-215">Der folgende Ordner „Veröffentlichungsprofil“ wurde mit Visual Studio erstellt und veröffentlicht in eine Netzwerkfreigabe.</span><span class="sxs-lookup"><span data-stu-id="223a5-215">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

<span data-ttu-id="223a5-216">[!code-xml[Main](web-publishing-vs/sample/FolderProfile.pubxml?highlight=5,9,11,18)]</span><span class="sxs-lookup"><span data-stu-id="223a5-216">[!code-xml[Main](web-publishing-vs/sample/FolderProfile.pubxml?highlight=5,9,11,18)]</span></span>

<span data-ttu-id="223a5-217">Beachten Sie, dass `<LastUsedBuildConfiguration>` auf `Release` festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="223a5-217">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span>  <span data-ttu-id="223a5-218">Bei der Veröffentlichung von Visual Studio wird der `<LastUsedBuildConfiguration>`-Wert für die Konfigurationseigenschaft mithilfe des Werts festgelegt, der beim Starten des Veröffentlichungsprozesses vorliegt.</span><span class="sxs-lookup"><span data-stu-id="223a5-218">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="223a5-219">Die `<LastUsedBuildConfiguration>`-Konfigurationseigenschaft ist ein Sonderfall und sollte nicht in einer importierte MSBuild-Datei überschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="223a5-219">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn’t be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="223a5-220">Sie können diese Eigenschaft von der Befehlszeile aus überschreiben.</span><span class="sxs-lookup"><span data-stu-id="223a5-220">You can override this property from the command line.</span></span> <span data-ttu-id="223a5-221">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="223a5-221">For example:</span></span>

`dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="223a5-222">Verwenden von MSBuild:</span><span class="sxs-lookup"><span data-stu-id="223a5-222">Using MSBuild:</span></span>

`msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="223a5-223">Veröffentlichen auf einen MSDeploy-Endpunkt von der Befehlszeile</span><span class="sxs-lookup"><span data-stu-id="223a5-223">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="223a5-224">Wie bereits erwähnt können Sie mithilfe des Befehls `dotnet publish` oder `msbuild` veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="223a5-224">As previously mentioned, you can publish using `dotnet publish` or the `msbuild` command.</span></span> <span data-ttu-id="223a5-225">`dotnet publish` wird im Kontext von .NET Core ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="223a5-225">`dotnet publish` runs in the context of .NET Core.</span></span> <span data-ttu-id="223a5-226">`msbuild` erfordert .NET Framework und ist daher auf Windows-Umgebungen beschränkt.</span><span class="sxs-lookup"><span data-stu-id="223a5-226">`msbuild` requires .NET framework, and is therefore limited to Windows environments.</span></span>

<span data-ttu-id="223a5-227">Die einfachste Möglichkeit zum Veröffentlichen mit MSDeploy ist die, zunächst ein Veröffentlichungsprofil in Visual Studio 2017 zu erstellen und das Profil von der Befehlszeile aus zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="223a5-227">The easiest way to publish with MSDeploy is to first create a publish profile in Visual Studio 2017 and use the profile from the command line.</span></span>

<span data-ttu-id="223a5-228">Im folgenden Beispiel wurde eine ASP.NET Core-App (mithilfe von `dotnet new mvc`) erstellt und ein Azure-Veröffentlichungsprofil mit Visual Studio hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="223a5-228">In the following sample, I created an ASP.NET Core web app ( using `dotnet new mvc`) and added an Azure publish profile with Visual Studio.</span></span>

<span data-ttu-id="223a5-229">Führen Sie `msbuild` von einer **Developer-Eingabeaufforderung für VS 2017** aus.</span><span class="sxs-lookup"><span data-stu-id="223a5-229">You run `msbuild` from a **Developer Command Prompt for VS 2017**.</span></span> <span data-ttu-id="223a5-230">Die Developer-Eingabeaufforderung verfügt in ihrem Pfad über die richtige *msbuild.exe*-Datei und über einige festgelegte MSBuild-Variablen.</span><span class="sxs-lookup"><span data-stu-id="223a5-230">The Developer Command Prompt will have the correct *msbuild.exe* in its path and set some MSBuild variables.</span></span>

<span data-ttu-id="223a5-231">MSBuild verwendet die folgende Syntax:</span><span class="sxs-lookup"><span data-stu-id="223a5-231">MSBuild uses the following syntax:</span></span>

`msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile>  /p:Username=<USERNAME> /p:Password=<PASSWORD>`

<span data-ttu-id="223a5-232">Sie erhalten das `Password` von der Datei *\<Publish name>.PublishSettings*.</span><span class="sxs-lookup"><span data-stu-id="223a5-232">You can get the `Password` from the *\<Publish name>.PublishSettings* file.</span></span> <span data-ttu-id="223a5-233">Hier können Sie die Datei *.PublishSettings* herunterladen:</span><span class="sxs-lookup"><span data-stu-id="223a5-233">You can download the *.PublishSettings* file from:</span></span>

- <span data-ttu-id="223a5-234">Projektmappen-Explorer: Klicken Sie mit der rechten Maustaste auf Web-App, und klicken Sie auf **Veröffentlichungsprofil herunterladen**.</span><span class="sxs-lookup"><span data-stu-id="223a5-234">Solution Explorer: Right click on the Web App and select **Download Publish Profile**.</span></span>
- <span data-ttu-id="223a5-235">Im Azure-Verwaltungsportal: Klicken Sie im Blatt „Web-App“ auf **Veröffentlichungsprofil abrufen**.</span><span class="sxs-lookup"><span data-stu-id="223a5-235">The Azure Management Portal: Select **Get publish profile** from the  Web App blade.</span></span>

<span data-ttu-id="223a5-236">`Username` kann im Veröffentlichungsprofil gefunden werden.</span><span class="sxs-lookup"><span data-stu-id="223a5-236">`Username` can be found in the publish profile.</span></span>

<span data-ttu-id="223a5-237">Das folgende Beispiel verwendet das Veröffentlichungsprofil „Web11112 – Web Deploy“:</span><span class="sxs-lookup"><span data-stu-id="223a5-237">The following sample uses the "Web11112 - Web Deploy" publish profile:</span></span>

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="excluding-files"></a><span data-ttu-id="223a5-238">Ausschließen von Dateien</span><span class="sxs-lookup"><span data-stu-id="223a5-238">Excluding files</span></span>

<span data-ttu-id="223a5-239">Beim Veröffentlichen von ASP.NET Core-Apps sind die Buildartefakte und die Inhalte des Ordners *wwwroot* enthalten.</span><span class="sxs-lookup"><span data-stu-id="223a5-239">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="223a5-240">`msbuild` unterstützt [Globmuster](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="223a5-240">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="223a5-241">Das folgende `<Content>`-Elementmarkup schließt zum Beispiel alle Textdateien (*TXT*) aus dem Ordner *wwwroot/content* und all seinen Unterordnern aus.</span><span class="sxs-lookup"><span data-stu-id="223a5-241">For example, the following `<Content>` element markup will exclude all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="223a5-242">Das obenstehende Markup kann zu einem Veröffentlichungsprofil oder der *CSPROJ*-Datei hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="223a5-242">The markup above can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="223a5-243">Wenn es zu der *CSPROJ*-Datei hinzugefügt wird, wird die Regel zu allen Veröffentlichungsprofilen im Projekt hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="223a5-243">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="223a5-244">Das folgende `<MsDeploySkipRules>`-Elementmarkup schließt alle Dateien aus dem Ordner *wwwroot/content* aus:</span><span class="sxs-lookup"><span data-stu-id="223a5-244">The following `<MsDeploySkipRules>` element markup exludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="223a5-245">`<MsDeploySkipRules>` löscht nicht die *Skip*-Ziele von der Bereitstellungsseite.</span><span class="sxs-lookup"><span data-stu-id="223a5-245">`<MsDeploySkipRules>`  will not delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="223a5-246">Von `<Content>` angezielte Dateien und Ordner werden von der Bereitstellungsseite gelöscht.</span><span class="sxs-lookup"><span data-stu-id="223a5-246">`<Content>` targeted files and folders will be deleted from the deployment site.</span></span> <span data-ttu-id="223a5-247">Nehmen Sie beispielsweise an, dass Sie eine Web-App mit den folgenden Dateien bereitgestellt hätten:</span><span class="sxs-lookup"><span data-stu-id="223a5-247">For example, suppose you had deployed a web app with the following files:</span></span>

- <span data-ttu-id="223a5-248">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="223a5-248">*Views/Home/About1.cshtml*</span></span>
- <span data-ttu-id="223a5-249">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="223a5-249">*Views/Home/About2.cshtml*</span></span>
- <span data-ttu-id="223a5-250">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="223a5-250">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="223a5-251">Wenn Sie das folgende `<MsDeploySkipRules>`-Markup hinzufügen, würden diese Dateien nicht von der Bereitstellungsseite gelöscht werden.</span><span class="sxs-lookup"><span data-stu-id="223a5-251">If you added the following `<MsDeploySkipRules>` markup, those files would not be deleted on the deployment site.</span></span>

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

<span data-ttu-id="223a5-252">Das oben gezeigte `<MsDeploySkipRules>`-Markup verhindert, dass die *übersprungenen* Dateien bereitgestellt werden. Es löscht diese Dateien jedoch nicht, nachdem sie bereitgestellt wurden.</span><span class="sxs-lookup"><span data-stu-id="223a5-252">The `<MsDeploySkipRules>` markup shown above prevents the *skipped* files from being depoyed, but will not delete those files once they are deployed.</span></span>

<span data-ttu-id="223a5-253">Das folgende `<Content>`-Markup löscht die angezielten Dateien auf der Bereitstellungsseite:</span><span class="sxs-lookup"><span data-stu-id="223a5-253">The following `<Content>` markup would delete the targeted files at the deployment site:</span></span>

``` xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="223a5-254">Das Verwenden der Befehlszeilenbereitstellung mit dem obenstehenden `<Content>`-Markup würde zu einer Ausgabe ähnlich der Folgenden führen:</span><span class="sxs-lookup"><span data-stu-id="223a5-254">Using command line deployment with the `<Content>` markup above would result in output similar to the following:</span></span>

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

## <a name="including-files"></a><span data-ttu-id="223a5-255">Einfügen von Dateien</span><span class="sxs-lookup"><span data-stu-id="223a5-255">Including files</span></span>

<span data-ttu-id="223a5-256">Das folgende Markup kann verwendet werden, um einen *Bilder*-Ordner außerhalb des Projektverzeichnisses in den Ordner *wwwroot/images* auf der Website einzuschließen.</span><span class="sxs-lookup"><span data-stu-id="223a5-256">The following markup can be used to include an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site.</span></span>

``` xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="223a5-257">Das Markup kann der *CSPROJ*-Datei oder dem Veröffentlichungsprofil hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="223a5-257">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="223a5-258">Wenn es der *CSPROJ*-Datei hinzugefügt wird, wird es in jedes Veröffentlichungsprofil im Projekt eingeschlossen.</span><span class="sxs-lookup"><span data-stu-id="223a5-258">If it's added to the *.csproj* file, it will be included in each publish profile in the project.</span></span>

<span data-ttu-id="223a5-259">Das folgende hervorgehobene Markup zeigt Folgendes:</span><span class="sxs-lookup"><span data-stu-id="223a5-259">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="223a5-260">Kopieren einer Datei von außerhalb des Projekts in den Ordner *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="223a5-260">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="223a5-261">Ausschließen des Ordners *wwwroot\Content*.</span><span class="sxs-lookup"><span data-stu-id="223a5-261">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="223a5-262">Ausschließen von *Views\Home\About2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="223a5-262">Exclude *Views\Home\About2.cshtml*.</span></span>

<span data-ttu-id="223a5-263">[!code-xml[Main](web-publishing-vs/sample/FolderProfile2.pubxml?highlight=21-29)]</span><span class="sxs-lookup"><span data-stu-id="223a5-263">[!code-xml[Main](web-publishing-vs/sample/FolderProfile2.pubxml?highlight=21-29)]</span></span>

<span data-ttu-id="223a5-264">Weitere Bereitstellungsbeispiele finden Sie unter [WebSDK Readme (WebSDK-Infodatei)](https://github.com/aspnet/websdk).</span><span class="sxs-lookup"><span data-stu-id="223a5-264">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

### <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="223a5-265">Führen Sie ein Ziel vor oder nach der Veröffentlichung aus.</span><span class="sxs-lookup"><span data-stu-id="223a5-265">Run a target before or after publishing</span></span>

<span data-ttu-id="223a5-266">Die integrierten Ziele `BeforePublish` und `AfterPublish` können verwendet werden, um ein Ziel vor oder nach dem Veröffentlichungsziel auszuführen.</span><span class="sxs-lookup"><span data-stu-id="223a5-266">The builtin `BeforePublish` and `AfterPublish` targets can be used to execute a target before or after the publish target.</span></span> <span data-ttu-id="223a5-267">Das folgende Markup kann dem Veröffentlichungsprofil hinzugefügt werden, um Nachrichten an die Konsolenausgabe vor und nach der Veröffentlichung zu protokollieren:</span><span class="sxs-lookup"><span data-stu-id="223a5-267">The following markup can be added to the publish profile to log messages to the console output before and after publishing:</span></span>

``` xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="the-kudu-service"></a><span data-ttu-id="223a5-268">Der Kudu-Dienst</span><span class="sxs-lookup"><span data-stu-id="223a5-268">The Kudu service</span></span>

<span data-ttu-id="223a5-269">Verwenden Sie zum Anzeigen der Dateien in Ihrer Azure-Web-App den [Kudu-Dienst](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="223a5-269">To view the files on your Azure Web App, use the [kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="223a5-270">Fügen Sie den Token `scm` an den Namen Ihrer Web-App an.</span><span class="sxs-lookup"><span data-stu-id="223a5-270">Append the `scm` token to the name or your Web App.</span></span> <span data-ttu-id="223a5-271">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="223a5-271">For example:</span></span>

| <span data-ttu-id="223a5-272">URL</span><span class="sxs-lookup"><span data-stu-id="223a5-272">URL</span></span>               | <span data-ttu-id="223a5-273">Ergebnis</span><span class="sxs-lookup"><span data-stu-id="223a5-273">Result</span></span>|
| ----------------- | ------------ |
| `http://mysite.azurewebsites.net/` | <span data-ttu-id="223a5-274">Webanwendung</span><span class="sxs-lookup"><span data-stu-id="223a5-274">Web App</span></span> |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="223a5-275">Kudu-Dienst</span><span class="sxs-lookup"><span data-stu-id="223a5-275">Kudu sevice</span></span> |

<span data-ttu-id="223a5-276">Klicken Sie auf das Menüelement [Debugging-Konsole](https://github.com/projectkudu/kudu/wiki/Kudu-console), um Dateien anzuzeigen, zu bearbeiten, zu löschen oder hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="223a5-276">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view/edit/delete/add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="223a5-277">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="223a5-277">Additional resources</span></span>

- <span data-ttu-id="223a5-278">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) erleichtert das Bereitstellen von Webanwendungen und Websites auf IIS-Server.</span><span class="sxs-lookup"><span data-stu-id="223a5-278">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy)  (msdeploy) simplifies deployment of Web applications and Web sites to IIS servers.</span></span>

- <span data-ttu-id="223a5-279">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): Probleme melden und Features für die Bereitstellung anfordern.</span><span class="sxs-lookup"><span data-stu-id="223a5-279">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
