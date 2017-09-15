---
title: Migrieren von ASP.NET Core 1.x zu 2.0
author: scottaddie
description: "In diesem Artikel werden die Voraussetzungen und üblichen Schritte zum Migrieren eines ASP.NET Core 1.x-Projekts zu ASP.NET Core 2.0 behandelt."
keywords: ASP.NET Core, migrieren
ms.author: scaddie
manager: wpickett
ms.date: 08/01/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/1x-to-2x/index
ms.openlocfilehash: db277ee6b079eab973a565983d6661bf95fce2e3
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2017
---
# <a name="migrating-from-aspnet-core-1x-to-aspnet-core-20"></a><span data-ttu-id="974fd-104">Migrieren von ASP.NET Core 1.x zu ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="974fd-104">Migrating from ASP.NET Core 1.x to ASP.NET Core 2.0</span></span>

<span data-ttu-id="974fd-105">Von [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="974fd-105">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="974fd-106">In diesem Artikel aktualisieren wir exemplarisch ein vorhandenes ASP.NET Core 1.x-Projekt auf ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="974fd-106">In this article, we'll walk you through updating an existing ASP.NET Core 1.x project to ASP.NET Core 2.0.</span></span> <span data-ttu-id="974fd-107">Durch Migrieren der Anwendung zu ASP.NET Core 2.0 kommen Sie in den Genuss [vieler neuer Funktionen und Leistungsverbesserungen](https://docs.microsoft.com/aspnet/core/aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="974fd-107">Migrating your application to ASP.NET Core 2.0 enables you to take advantage of [many new features and performance improvements](https://docs.microsoft.com/aspnet/core/aspnetcore-2.0).</span></span> 

<span data-ttu-id="974fd-108">Vorhandene ASP.NET Core 1.x-Anwendungen basieren auf versionsspezifischen Projektvorlagen.</span><span class="sxs-lookup"><span data-stu-id="974fd-108">Existing ASP.NET Core 1.x applications are based off of version-specific project templates.</span></span> <span data-ttu-id="974fd-109">Doch das ASP.NET Core-Framework entwickelt sich weiter. Gleiches gilt für die darin enthaltenen Projektvorlagen und den Startercode.</span><span class="sxs-lookup"><span data-stu-id="974fd-109">As the ASP.NET Core framework evolves, so do the project templates and the starter code contained within them.</span></span> <span data-ttu-id="974fd-110">Zusätzlich zum Aktualisieren des ASP.NET Core-Frameworks müssen Sie den Code für Ihre Anwendung aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="974fd-110">In addition to updating the ASP.NET Core framework, you need to update the code for your application.</span></span>

<a name="prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="974fd-111">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="974fd-111">Prerequisites</span></span>
<span data-ttu-id="974fd-112">Siehe [Erste Schritte mit ASP.NET Core](xref:getting-started).</span><span class="sxs-lookup"><span data-stu-id="974fd-112">Please see [Getting Started with ASP.NET Core](xref:getting-started).</span></span>

<a name="tfm"></a>

## <a name="update-target-framework-moniker-tfm"></a><span data-ttu-id="974fd-113">Aktualisieren des Zielframeworkmonikers (Target Framework Moniker, TFM)</span><span class="sxs-lookup"><span data-stu-id="974fd-113">Update Target Framework Moniker (TFM)</span></span>
<span data-ttu-id="974fd-114">Auf .NET Core ausgelegte Projekte müssen den [TFM](/dotnet/standard/frameworks#referring-to-frameworks) einer Version größer gleich .NET Core 2.0 verwenden.</span><span class="sxs-lookup"><span data-stu-id="974fd-114">Projects targeting .NET Core should use the [TFM](/dotnet/standard/frameworks#referring-to-frameworks) of a version greater than or equal to .NET Core 2.0.</span></span> <span data-ttu-id="974fd-115">Suchen Sie den Knoten `<TargetFramework>` in der *CSPROJ*-Datei, und ersetzen Sie dessen inneren Text durch `netcoreapp2.0`:</span><span class="sxs-lookup"><span data-stu-id="974fd-115">Search for the `<TargetFramework>` node in the *.csproj* file, and replace its inner text with `netcoreapp2.0`:</span></span>

<span data-ttu-id="974fd-116">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=3)]</span><span class="sxs-lookup"><span data-stu-id="974fd-116">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=3)]</span></span>

<span data-ttu-id="974fd-117">Auf .NET Framework ausgelegte Projekte müssen den TFM einer Version größer gleich .NET Framework 4.6.1 verwenden.</span><span class="sxs-lookup"><span data-stu-id="974fd-117">Projects targeting .NET Framework should use the TFM of a version greater than or equal to .NET Framework 4.6.1.</span></span> <span data-ttu-id="974fd-118">Suchen Sie den Knoten `<TargetFramework>` in der *CSPROJ*-Datei, und ersetzen Sie dessen inneren Text durch `net461`:</span><span class="sxs-lookup"><span data-stu-id="974fd-118">Search for the `<TargetFramework>` node in the *.csproj* file, and replace its inner text with `net461`:</span></span>

<span data-ttu-id="974fd-119">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=4)]</span><span class="sxs-lookup"><span data-stu-id="974fd-119">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=4)]</span></span>

> [!NOTE]
> <span data-ttu-id="974fd-120">.NET Core 2.0 bietet eine viel größere Oberfläche als .NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="974fd-120">.NET Core 2.0 offers a much larger surface area than .NET Core 1.x.</span></span> <span data-ttu-id="974fd-121">Wenn Sie mit .NET Framework entwickeln, nur weil APIs in .NET Core 1.x fehlen, wird das Entwickeln mit .NET Core 2.0 wahrscheinlich funktionieren.</span><span class="sxs-lookup"><span data-stu-id="974fd-121">If you're targeting .NET Framework solely because of missing APIs in .NET Core 1.x, targeting .NET Core 2.0 is likely to work.</span></span>

<a name="global-json"></a>

## <a name="update-net-core-sdk-version-in-globaljson"></a><span data-ttu-id="974fd-122">Aktualisieren der .NET Core SDK-Version in „global.json“</span><span class="sxs-lookup"><span data-stu-id="974fd-122">Update .NET Core SDK version in global.json</span></span>
<span data-ttu-id="974fd-123">Wenn Ihre Projektmappe von einer Datei des Typs [*global.json*](https://docs.microsoft.com/dotnet/core/tools/global-json) abhängig ist, um auf eine bestimmte .NET Core SDK-Version abzuzielen, aktualisieren Sie deren `version`-Eigenschaft so, dass die auf Ihrem Computer installierte Version 2.0 verwendet wird:</span><span class="sxs-lookup"><span data-stu-id="974fd-123">If your solution relies upon a [*global.json*](https://docs.microsoft.com/dotnet/core/tools/global-json) file to target a specific .NET Core SDK version, update its `version` property to use the 2.0 version installed on your machine:</span></span>

<span data-ttu-id="974fd-124">[!code-json[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/global.json?highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="974fd-124">[!code-json[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/global.json?highlight=3)]</span></span>

<a name="package-reference"></a>

## <a name="update-package-references"></a><span data-ttu-id="974fd-125">Aktualisieren von Paketverweisen</span><span class="sxs-lookup"><span data-stu-id="974fd-125">Update package references</span></span>
<span data-ttu-id="974fd-126">In der *CSPROJ*-Datei in einem Projekt der Version 1.x sind alle NuGet-Pakete aufgeführt, die vom Projekt verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="974fd-126">The *.csproj* file in a 1.x project lists each NuGet package used by the project.</span></span>

<span data-ttu-id="974fd-127">In einem ASP.NET Core 2.0-Projekt für .NET Core 2.0 ersetzt ein einzelner Verweis des Typs [metapackage](xref:fundamentals/metapackage) in der *CSPROJ*-Datei die Paketsammlung:</span><span class="sxs-lookup"><span data-stu-id="974fd-127">In an ASP.NET Core 2.0 project targeting .NET Core 2.0, a single [metapackage](xref:fundamentals/metapackage) reference in the *.csproj* file replaces the collection of packages:</span></span>

<span data-ttu-id="974fd-128">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=9-11)]</span><span class="sxs-lookup"><span data-stu-id="974fd-128">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=9-11)]</span></span>

<span data-ttu-id="974fd-129">Alle Funktionen von ASP.NET Core 2.0 und Entity Framework Core 2.0 sind in „metapackage“ enthalten.</span><span class="sxs-lookup"><span data-stu-id="974fd-129">All the features of ASP.NET Core 2.0 and Entity Framework Core 2.0 are included in the metapackage.</span></span>

<span data-ttu-id="974fd-130">ASP.NET Core 2.0-Projekte für .NET Framework müssen weiterhin auf einzelne NuGet-Pakete verweisen.</span><span class="sxs-lookup"><span data-stu-id="974fd-130">ASP.NET Core 2.0 projects targeting .NET Framework should continue to reference individual NuGet packages.</span></span> <span data-ttu-id="974fd-131">Aktualisieren Sie das `Version`-Attribut aller Knoten des Typs `<PackageReference />` auf 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="974fd-131">Update the `Version` attribute of each `<PackageReference />` node to 2.0.0.</span></span>

<span data-ttu-id="974fd-132">Hier ist z.B. die Liste der `<PackageReference />`-Knoten in einem typischen ASP.NET Core 2.0-Projekt für .NET Framework:</span><span class="sxs-lookup"><span data-stu-id="974fd-132">For example, here's the list of `<PackageReference />` nodes used in a typical ASP.NET Core 2.0 project targeting .NET Framework:</span></span>

<span data-ttu-id="974fd-133">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=9-22)]</span><span class="sxs-lookup"><span data-stu-id="974fd-133">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=9-22)]</span></span>

<a name="dot-net-cli-tool-reference"></a>

## <a name="update-net-core-cli-tools"></a><span data-ttu-id="974fd-134">Aktualisieren von .NET Core CLI-Tools</span><span class="sxs-lookup"><span data-stu-id="974fd-134">Update .NET Core CLI tools</span></span>
<span data-ttu-id="974fd-135">Aktualisieren Sie in der *CSPROJ*-Datei das `Version`-Attribut jedes `<DotNetCliToolReference />`-Knotens auf 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="974fd-135">In the *.csproj* file, update the `Version` attribute of each `<DotNetCliToolReference />` node to 2.0.0.</span></span>

<span data-ttu-id="974fd-136">Hier ist z.B. die Liste der CLI-Tools in einem typischen ASP.NET Core 2.0-Projekt für .NET Core 2.0:</span><span class="sxs-lookup"><span data-stu-id="974fd-136">For example, here's the list of CLI tools used in a typical ASP.NET Core 2.0 project targeting .NET Core 2.0:</span></span>

<span data-ttu-id="974fd-137">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=13-17)]</span><span class="sxs-lookup"><span data-stu-id="974fd-137">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=13-17)]</span></span>

<a name="package-target-fallback"></a>

## <a name="rename-package-target-fallback-property"></a><span data-ttu-id="974fd-138">Umbenennen der „TargetFallback“-Eigenschaft des Pakets</span><span class="sxs-lookup"><span data-stu-id="974fd-138">Rename Package Target Fallback property</span></span>
<span data-ttu-id="974fd-139">Die *CSPROJ*-Datei eines 1.x-Projekts hat einen Knoten des Typs `PackageTargetFallback` und eine Variable verwendet:</span><span class="sxs-lookup"><span data-stu-id="974fd-139">The *.csproj* file of a 1.x project used a `PackageTargetFallback` node and variable:</span></span>

<span data-ttu-id="974fd-140">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App.csproj?range=5)]</span><span class="sxs-lookup"><span data-stu-id="974fd-140">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App.csproj?range=5)]</span></span>

<span data-ttu-id="974fd-141">Benennen Sie den Knoten und die Variable in `AssetTargetFallback` um:</span><span class="sxs-lookup"><span data-stu-id="974fd-141">Rename both the node and variable to `AssetTargetFallback`:</span></span>

<span data-ttu-id="974fd-142">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=5)]</span><span class="sxs-lookup"><span data-stu-id="974fd-142">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=5)]</span></span>

<a name="program-cs"></a>

## <a name="update-main-method-in-programcs"></a><span data-ttu-id="974fd-143">Aktualisieren der „Main“-Methode in „Program.cs“</span><span class="sxs-lookup"><span data-stu-id="974fd-143">Update Main method in Program.cs</span></span>
<span data-ttu-id="974fd-144">In 1.x-Projekten sah die `Main`-Methode von *Program.cs* wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="974fd-144">In 1.x projects, the `Main` method of *Program.cs* looked like this:</span></span>

<span data-ttu-id="974fd-145">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Program.cs?name=snippet_ProgramCs&highlight=8-19)]</span><span class="sxs-lookup"><span data-stu-id="974fd-145">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Program.cs?name=snippet_ProgramCs&highlight=8-19)]</span></span>

<span data-ttu-id="974fd-146">In 2.0-Projekten wurde die `Main`-Methode von *Program.cs* vereinfacht:</span><span class="sxs-lookup"><span data-stu-id="974fd-146">In 2.0 projects, the `Main` method of *Program.cs* has been simplified:</span></span>

<span data-ttu-id="974fd-147">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Program.cs?highlight=8-11)]</span><span class="sxs-lookup"><span data-stu-id="974fd-147">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Program.cs?highlight=8-11)]</span></span>

<span data-ttu-id="974fd-148">Das Übernehmen dieses neuen 2.0-Musters wird dringend empfohlen und ist für Produktfunktionen wie [Entity Framework Core-Migrationen](xref:data/ef-mvc/migrations) erforderlich.</span><span class="sxs-lookup"><span data-stu-id="974fd-148">The adoption of this new 2.0 pattern is highly recommended and is required for product features like [Entity Framework Core Migrations](xref:data/ef-mvc/migrations) to work.</span></span> <span data-ttu-id="974fd-149">Bei Ausführung von `Update-Database` im Paket-Manager-Konsolenfenster oder von `dotnet ef database update` an der Befehlszeile (für in ASP.NET Core 2.0 konvertierte Projekte) wird der folgende Fehler generiert:</span><span class="sxs-lookup"><span data-stu-id="974fd-149">For example, running `Update-Database` from the Package Manager Console window or `dotnet ef database update` from the command line (on projects converted to ASP.NET Core 2.0) generates the following error:</span></span>

```
Unable to create an object of type '<Context>'. Add an implementation of 'IDesignTimeDbContextFactory<Context>' to the project, or see https://go.microsoft.com/fwlink/?linkid=851728 for additional patterns supported at design time.
```

<a name="view-compilation"></a>

## <a name="review-your-razor-view-compilation-setting"></a><span data-ttu-id="974fd-150">Überprüfen der Einstellung für die Kompilierung der Razor-Ansicht</span><span class="sxs-lookup"><span data-stu-id="974fd-150">Review your Razor View Compilation setting</span></span>
<span data-ttu-id="974fd-151">Eine schnellere Anwendungsstartzeit und kleinere veröffentlichte Pakete sind für Sie von höchster Wichtigkeit.</span><span class="sxs-lookup"><span data-stu-id="974fd-151">Faster application startup time and smaller published bundles are of utmost importance to you.</span></span> <span data-ttu-id="974fd-152">Aus diesen Gründen ist [Razor-Ansichtskompilierung](xref:mvc/views/view-compilation) in ASP.NET Core 2.0 standardmäßig aktiviert.</span><span class="sxs-lookup"><span data-stu-id="974fd-152">For these reasons, [Razor view compilation](xref:mvc/views/view-compilation) is enabled by default in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="974fd-153">Das Festlegen der `MvcRazorCompileOnPublish`-Eigenschaft auf „true“ ist nicht mehr erforderlich.</span><span class="sxs-lookup"><span data-stu-id="974fd-153">Setting the `MvcRazorCompileOnPublish` property to true is no longer required.</span></span> <span data-ttu-id="974fd-154">Außer wenn Sie die Ansichtskompilierung deaktivieren, kann die Eigenschaft aus der *CSPROJ*-Datei entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="974fd-154">Unless you're disabling view compilation, the property may be removed from the *.csproj* file.</span></span>

<span data-ttu-id="974fd-155">Bei Entwicklung für .NET Framework müssen Sie weiter explizit auf das NuGet-Paket [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) in Ihrer *CSPROJ*-Datei verweisen:</span><span class="sxs-lookup"><span data-stu-id="974fd-155">When targeting .NET Framework, you still need to explicitly reference the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) NuGet package in your *.csproj* file:</span></span>

<span data-ttu-id="974fd-156">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=15)]</span><span class="sxs-lookup"><span data-stu-id="974fd-156">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=15)]</span></span>

<a name="app-insights"></a>

## <a name="rely-on-application-insights-light-up-features"></a><span data-ttu-id="974fd-157">Arbeiten mit den Startfeatures von Application Insights</span><span class="sxs-lookup"><span data-stu-id="974fd-157">Rely on Application Insights "Light-Up" features</span></span>
<span data-ttu-id="974fd-158">Die mühelose Einrichtung der Instrumentierung der Anwendungsleistung ist wichtig.</span><span class="sxs-lookup"><span data-stu-id="974fd-158">Effortless setup of application performance instrumentation is important.</span></span> <span data-ttu-id="974fd-159">Hierfür können Sie nun mit den neuen Startfeatures von [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) in den Visual Studio 2017-Tools arbeiten.</span><span class="sxs-lookup"><span data-stu-id="974fd-159">You can now rely on the new [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) "light-up" features available in the Visual Studio 2017 tooling.</span></span>

<span data-ttu-id="974fd-160">Bei in Visual Studio 2017 erstellten ASP.NET Core 1.1-Projekten wurde Application Insights standardmäßig hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="974fd-160">ASP.NET Core 1.1 projects created in Visual Studio 2017 added Application Insights by default.</span></span> <span data-ttu-id="974fd-161">Wenn Sie das Application Insights SDK nicht direkt verwenden, gehen Sie außerhalb von *Program.cs* und *Startup.cs* folgendermaßen vor:</span><span class="sxs-lookup"><span data-stu-id="974fd-161">If you're not using the Application Insights SDK directly, outside of *Program.cs* and *Startup.cs*, follow these steps:</span></span>

1. <span data-ttu-id="974fd-162">Entfernen Sie den folgenden `<PackageReference />`-Knoten aus der *CSPROJ*-Datei:</span><span class="sxs-lookup"><span data-stu-id="974fd-162">Remove the following `<PackageReference />` node from the *.csproj* file:</span></span>
    
    <span data-ttu-id="974fd-163">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App.csproj?range=10)]</span><span class="sxs-lookup"><span data-stu-id="974fd-163">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App.csproj?range=10)]</span></span>

2. <span data-ttu-id="974fd-164">Entfernen Sie den Aufruf der Erweiterungsmethode `UseApplicationInsights` aus *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="974fd-164">Remove the `UseApplicationInsights` extension method invocation from *Program.cs*:</span></span>

    <span data-ttu-id="974fd-165">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Program.cs?name=snippet_ProgramCsMain&highlight=8)]</span><span class="sxs-lookup"><span data-stu-id="974fd-165">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Program.cs?name=snippet_ProgramCsMain&highlight=8)]</span></span>

3. <span data-ttu-id="974fd-166">Entfernen Sie aus *_Layout.cshtml* den clientseitigen API-Aufruf von Application Insights.</span><span class="sxs-lookup"><span data-stu-id="974fd-166">Remove the Application Insights client-side API call from *_Layout.cshtml*.</span></span> <span data-ttu-id="974fd-167">Dieser umfasst die beiden folgenden Codezeilen:</span><span class="sxs-lookup"><span data-stu-id="974fd-167">It comprises the following two lines of code:</span></span>

    <span data-ttu-id="974fd-168">[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Views/Shared/_Layout.cshtml?range=1,19)]</span><span class="sxs-lookup"><span data-stu-id="974fd-168">[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Views/Shared/_Layout.cshtml?range=1,19)]</span></span>

<span data-ttu-id="974fd-169">Sie können das Application Insights SDK weiterhin direkt verwenden.</span><span class="sxs-lookup"><span data-stu-id="974fd-169">If you are using the Application Insights SDK directly, continue to do so.</span></span> <span data-ttu-id="974fd-170">[metapackage](xref:fundamentals/metapackage) der Version 2.0 enthält die neueste Version von Application Insights, weshalb ein Fehler zum Downgrade eines Pakets angezeigt wird, wenn Sie auf eine ältere Version verweisen.</span><span class="sxs-lookup"><span data-stu-id="974fd-170">The 2.0 [metapackage](xref:fundamentals/metapackage) includes the latest version of Application Insights, so a package downgrade error appears if you're referencing an older version.</span></span>

<a name="auth-and-identity"></a>

## <a name="adopt-authentication--identity-improvements"></a><span data-ttu-id="974fd-171">Übernehmen verbesserter Authentifizierungs- und Identitätsfunktionen</span><span class="sxs-lookup"><span data-stu-id="974fd-171">Adopt Authentication / Identity Improvements</span></span>
<span data-ttu-id="974fd-172">ASP.NET Core 2.0 verfügt über ein neues Authentifizierungsmodell und bietet verschiedene Änderungen an ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="974fd-172">ASP.NET Core 2.0 has a new authentication model and a number of significant changes to ASP.NET Core Identity.</span></span> <span data-ttu-id="974fd-173">Wenn Sie Ihr Projekt mit aktivierter Option „Einzelne Benutzerkonten“ erstellt oder Authentifizierungs- oder Identitätsfunktionen manuell hinzugefügt haben, finden Sie unter [Migrieren von Authentifizierungs- und Identitätseinstellungen nach ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x) weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="974fd-173">If you created your project with Individual User Accounts enabled, or if you have manually added authentication or Identity, see [Migrating Authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="974fd-174">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="974fd-174">Additional Resources</span></span>
- [<span data-ttu-id="974fd-175">Wichtige Änderungen in ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="974fd-175">Breaking Changes in ASP.NET Core 2.0</span></span>](https://github.com/aspnet/announcements/issues?page=1&q=is%3Aissue+is%3Aopen+label%3A2.0.0+label%3A%22Breaking+change%22&utf8=%E2%9C%93)
