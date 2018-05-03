---
title: Kompilierung und Vorkompilierung einer Razor-Ansicht in ASP.NET Core
author: rick-anderson
description: Erfahren Sie, wie Sie die Kompilierung und Vorkompilierung einer MVC-Razor-Ansicht in ASP.NET Core-Apps aktivieren.
manager: wpickett
ms.author: riande
ms.date: 12/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-compilation
ms.openlocfilehash: 5d971645106a79497a9902063c7774dc6d546395
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a><span data-ttu-id="44979-103">Kompilierung und Vorkompilierung einer Razor-Ansicht in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="44979-103">Razor view compilation and precompilation in ASP.NET Core</span></span>

<span data-ttu-id="44979-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="44979-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="44979-105">Razor-Ansichten werden zur Laufzeit kompiliert, wenn die Ansicht aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="44979-105">Razor views are compiled at runtime when the view is invoked.</span></span> <span data-ttu-id="44979-106">ASP.NET Core 1.1.0 und höher kann optional Razor-Ansichten kompilieren und diese mit der App bereitstellen &mdash; dieser Prozess wird als Vorkompilierung bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="44979-106">ASP.NET Core 1.1.0 and higher can optionally compile Razor views and deploy them with the app&mdash;a process known as precompilation.</span></span> <span data-ttu-id="44979-107">Die ASP.NET Core 2.x-Projektvorlagen aktivieren die Vorkompilierung standardmäßig.</span><span class="sxs-lookup"><span data-stu-id="44979-107">The ASP.NET Core 2.x project templates enable precompilation by default.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="44979-108">Die Vorkompilierung von Razor-Ansichten steht aktuell beim Durchführen einer [eigenständigen Bereitstellung (Self-Contained Deployment, SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0 nicht zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="44979-108">Razor view precompilation is currently unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span> <span data-ttu-id="44979-109">Mit Release 2.1 wird dieses Feature für SCDs verfügbar.</span><span class="sxs-lookup"><span data-stu-id="44979-109">The feature will be available for SCDs when 2.1 releases.</span></span> <span data-ttu-id="44979-110">Weitere Informationen finden Sie unter [View compilation fails when cross-compiling for Linux on Windows (Die Ansichtskompilierung schlägt bei der übergreifenden Kompilierung für Linux unter Windows fehl)](https://github.com/aspnet/MvcPrecompilation/issues/102).</span><span class="sxs-lookup"><span data-stu-id="44979-110">For more information, see [View compilation fails when cross-compiling for Linux on Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).</span></span>

<span data-ttu-id="44979-111">Was vor der Kompilierung beachtet werden muss:</span><span class="sxs-lookup"><span data-stu-id="44979-111">Precompilation considerations:</span></span>

* <span data-ttu-id="44979-112">Das Vorkompilieren von Ansichten führt dazu, dass ein kleineres Bundle veröffentlicht wird und der Start schneller erfolgt.</span><span class="sxs-lookup"><span data-stu-id="44979-112">Precompiling views results in a smaller published bundle and faster startup time.</span></span>
* <span data-ttu-id="44979-113">Sie können Razor-Dateien nicht mehr bearbeiten, nachdem Sie Ansichten vorkompiliert haben.</span><span class="sxs-lookup"><span data-stu-id="44979-113">You can't edit Razor files after you precompile views.</span></span> <span data-ttu-id="44979-114">Die bearbeiteten Ansichten sind im veröffentlichten Bundle nicht vorhanden.</span><span class="sxs-lookup"><span data-stu-id="44979-114">The edited views won't be present in the published bundle.</span></span> 

<span data-ttu-id="44979-115">So stellen Sie vorkompilierte Ansichten bereit:</span><span class="sxs-lookup"><span data-stu-id="44979-115">To deploy precompiled views:</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="44979-116">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="44979-116">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
<span data-ttu-id="44979-117">Wenn Ihr Projekt .NET Framework als Ziel verwendet, beziehen Sie einen Paketverweis auf [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) mit ein:</span><span class="sxs-lookup"><span data-stu-id="44979-117">If your project targets .NET Framework, include a package reference to [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

<span data-ttu-id="44979-118">Wenn Ihr Paket für .NET Core gedacht ist, sind keine Änderungen erforderlich.</span><span class="sxs-lookup"><span data-stu-id="44979-118">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="44979-119">Die ASP.NET Core 2.x-Projektvorlagen legen `MvcRazorCompileOnPublish` standardmäßig implizit auf `true` fest. Dies bedeutet, dass dieser Knoten sicher aus der *CSPROJ*-Datei entfernt werden kann.</span><span class="sxs-lookup"><span data-stu-id="44979-119">The ASP.NET Core 2.x project templates implicitly set `MvcRazorCompileOnPublish` to `true` by default, which means this node can be safely removed from the *.csproj* file.</span></span> <span data-ttu-id="44979-120">Wenn Sie das explizite Festlegen vorziehen, können Sie die `MvcRazorCompileOnPublish`-Eigenschaft auch auf `true` festlegen.</span><span class="sxs-lookup"><span data-stu-id="44979-120">If you prefer to be explicit, there's no harm in setting the `MvcRazorCompileOnPublish` property to `true`.</span></span> <span data-ttu-id="44979-121">Das folgende *CSPROJ*-Beispiel veranschaulicht diese Eigenschaft:</span><span class="sxs-lookup"><span data-stu-id="44979-121">The following *.csproj* sample highlights this setting:</span></span>

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish2.csproj?highlight=5)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="44979-122">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="44979-122">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
<span data-ttu-id="44979-123">Legen Sie `MvcRazorCompileOnPublish` auf `true` fest, und ziehen Sie einen Paketverweis auf `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation` ein.</span><span class="sxs-lookup"><span data-stu-id="44979-123">Set `MvcRazorCompileOnPublish` to `true`, and include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span></span> <span data-ttu-id="44979-124">Das folgende *CSPROJ*-Beispiel veranschaulicht diese Einstellungen:</span><span class="sxs-lookup"><span data-stu-id="44979-124">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=5,12)]

<span data-ttu-id="44979-125">Bereiten Sie die App mit dem [Veröffentlichungsbefehl der .NET Core-CLI](/dotnet/core/tools/dotnet-publish) für eine [Framework-abhängige Bereitstellung](/dotnet/core/deploying/#framework-dependent-deployments-fdd) vor.</span><span class="sxs-lookup"><span data-stu-id="44979-125">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) with the [.NET Core CLI publish command](/dotnet/core/tools/dotnet-publish).</span></span> <span data-ttu-id="44979-126">Führen Sie z. B. den folgenden Befehl auf der Stammebene des Projekts aus:</span><span class="sxs-lookup"><span data-stu-id="44979-126">For example, execute the following command at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="44979-127">Die Datei *<projektname>.PrecompiledViews.dll*, die die kompilierten Razor-Ansichten enthält, wird erzeugt, wenn die Vorkompilierung erfolgreich abgeschlossen wurde.</span><span class="sxs-lookup"><span data-stu-id="44979-127">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor views, is produced when precompilation succeeds.</span></span> <span data-ttu-id="44979-128">Der unten stehende Screenshot zeigt beispielsweise den Inhalt von *Index.cshtml* innerhalb von *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="44979-128">For example, the screenshot below depicts the contents of *Index.cshtml* inside of *WebApplication1.PrecompiledViews.dll*:</span></span>

![Razor-Ansichten in einer DLL](view-compilation/_static/razor-views-in-dll.png)
