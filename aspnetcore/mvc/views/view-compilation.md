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
ms.openlocfilehash: 013ca0d149c6415b5e6825aa5a48e93ae48f6728
ms.sourcegitcommit: 0063338c2e130409081bb60fcffa0c3f190cd46a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2018
---
# <a name="razor-file-cshtml-compilation-in-aspnet-core"></a><span data-ttu-id="5886f-103">Kompilieren einer Razor-Datei (CSHTML) in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5886f-103">Razor file (.cshtml) compilation in ASP.NET Core</span></span>

<span data-ttu-id="5886f-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5886f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5886f-105">Razor-Ansichten werden zur Laufzeit kompiliert, wenn die Ansicht aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="5886f-105">Razor views are compiled at runtime when the view is invoked.</span></span> <span data-ttu-id="5886f-106">Mit ASP.NET Core 2.1.0 oder höher können Sie Ansichten zum Zeitpunkt der Erstellung und Veröffentlichung mithilfe vom [Razor SDK](/aspnetcore/mvc/razor-pages/sdk) kompilieren.</span><span class="sxs-lookup"><span data-stu-id="5886f-106">ASP.NET Core 2.1.0 or later compile views at build and publish time using [Razor Sdk](/aspnetcore/mvc/razor-pages/sdk).</span></span> <span data-ttu-id="5886f-107">Mithilfe des Tools für die Vorkompilierung können Ansichten in ASP.NET Core 1.1 und ASP.NET Core 2.0 optional zur Veröffentlichung oder Bereitstellung mit der App kompiliert werden.</span><span class="sxs-lookup"><span data-stu-id="5886f-107">In ASP.NET Core 1.1, and ASP.NET Core 2.0, views can optionally be compiled at publish and deployed with the app &mdash; using the precompilation tool.</span></span> 



<span data-ttu-id="5886f-108">Was vor der Kompilierung beachtet werden muss:</span><span class="sxs-lookup"><span data-stu-id="5886f-108">Precompilation considerations:</span></span>

* <span data-ttu-id="5886f-109">Das Vorkompilieren von Ansichten führt dazu, dass ein kleineres Bundle veröffentlicht wird und der Start schneller erfolgt.</span><span class="sxs-lookup"><span data-stu-id="5886f-109">Precompiling views results in a smaller published bundle and faster startup time.</span></span>
* <span data-ttu-id="5886f-110">Sie können Razor-Dateien nicht mehr bearbeiten, nachdem Sie Ansichten vorkompiliert haben.</span><span class="sxs-lookup"><span data-stu-id="5886f-110">You can't edit Razor files after you precompile views.</span></span> <span data-ttu-id="5886f-111">Die bearbeiteten Ansichten sind im veröffentlichten Bundle nicht vorhanden.</span><span class="sxs-lookup"><span data-stu-id="5886f-111">The edited views won't be present in the published bundle.</span></span> 

<span data-ttu-id="5886f-112">So stellen Sie vorkompilierte Ansichten bereit:</span><span class="sxs-lookup"><span data-stu-id="5886f-112">To deploy precompiled views:</span></span>

# <a name="aspnet-core-21tabaspnetcore21"></a>[<span data-ttu-id="5886f-113">ASP.NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="5886f-113">ASP.NET Core 2.1</span></span>](#tab/aspnetcore21/)
<span data-ttu-id="5886f-114">Die Kompilierung zur Erstellung und Veröffentlichung von Razor-Dateien wird standardmäßig vom Razor SDK aktiviert.</span><span class="sxs-lookup"><span data-stu-id="5886f-114">Build and publish time compilation of Razor files is enabled by default by the Razor Sdk.</span></span> <span data-ttu-id="5886f-115">Das Bearbeiten von Razor-Dateien, nachdem sie aktualisiert wurden, wird zum Zeitpunkt der Erstellung unterstützt.</span><span class="sxs-lookup"><span data-stu-id="5886f-115">Editing Razor files after they are updated is supported at build time.</span></span> <span data-ttu-id="5886f-116">Standardmäßig wird nur die kompilierte *Views.dll*, ohne CSHTML-Dateien, mit Ihrer Anwendung bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="5886f-116">By default, only the compiled *Views.dll* and no cshtml files are deployed with your application.</span></span> 
    
> [!IMPORTANT]
> <span data-ttu-id="5886f-117">Das Razor SDK ist nur wirksam, wenn keine für die Vorkompilierung spezifischen Eigenschaften in Ihrer Projektdatei festgelegt sind.</span><span class="sxs-lookup"><span data-stu-id="5886f-117">The Razor Sdk is only effective when no precompilation specific properties are set in your project file.</span></span> <span data-ttu-id="5886f-118">Das Razor SDK wird beispielsweise deaktiviert, wenn Sie in Ihrer *CSPROJ*-Datei `MvcRazorCompileOnPublish` festlegen.</span><span class="sxs-lookup"><span data-stu-id="5886f-118">For instance, setting `MvcRazorCompileOnPublish` in your *.csproj* file will disable the Razor Sdk.</span></span>

# <a name="aspnet-core-20tabaspnetcore20"></a>[<span data-ttu-id="5886f-119">ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="5886f-119">ASP.NET Core 2.0</span></span>](#tab/aspnetcore20/)

<span data-ttu-id="5886f-120">Wenn Ihr Projekt .NET Framework als Ziel verwendet, beziehen Sie einen Paketverweis auf [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) mit ein:</span><span class="sxs-lookup"><span data-stu-id="5886f-120">If your project targets .NET Framework, include a package reference to [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

<span data-ttu-id="5886f-121">Wenn Ihr Paket für .NET Core gedacht ist, sind keine Änderungen erforderlich.</span><span class="sxs-lookup"><span data-stu-id="5886f-121">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="5886f-122">Die ASP.NET Core 2.x-Projektvorlagen legen `MvcRazorCompileOnPublish` standardmäßig implizit auf `true` fest. Dies bedeutet, dass dieser Knoten sicher aus der *CSPROJ*-Datei entfernt werden kann.</span><span class="sxs-lookup"><span data-stu-id="5886f-122">The ASP.NET Core 2.x project templates implicitly sets `MvcRazorCompileOnPublish` to `true` by default, which means this node can be safely removed from the *.csproj* file.</span></span>
    
> [!IMPORTANT]
> <span data-ttu-id="5886f-123">Die Vorkompilierung von Razor-Ansichten steht beim Durchführen einer [eigenständigen Bereitstellung (Self-Contained Deployment, SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0 nicht zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="5886f-123">Razor view precompilation in not available when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span> 

<span data-ttu-id="5886f-124">Bereiten Sie die App mit dem [Veröffentlichungsbefehl der .NET Core-CLI](/dotnet/core/tools/dotnet-publish) für eine [Framework-abhängige Bereitstellung](/dotnet/core/deploying/#framework-dependent-deployments-fdd) vor.</span><span class="sxs-lookup"><span data-stu-id="5886f-124">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) with the [.NET Core CLI publish command](/dotnet/core/tools/dotnet-publish).</span></span> <span data-ttu-id="5886f-125">Führen Sie z. B. den folgenden Befehl auf der Stammebene des Projekts aus:</span><span class="sxs-lookup"><span data-stu-id="5886f-125">For example, execute the following command at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="5886f-126">Die Datei *<projektname>.PrecompiledViews.dll*, die die kompilierten Razor-Ansichten enthält, wird erzeugt, wenn die Vorkompilierung erfolgreich abgeschlossen wurde.</span><span class="sxs-lookup"><span data-stu-id="5886f-126">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor views, is produced when precompilation succeeds.</span></span> <span data-ttu-id="5886f-127">Der unten stehende Screenshot zeigt beispielsweise den Inhalt von *Index.cshtml* innerhalb von *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="5886f-127">For example, the screenshot below depicts the contents of *Index.cshtml* inside of *WebApplication1.PrecompiledViews.dll*:</span></span>

![Razor-Ansichten in einer DLL](view-compilation/_static/razor-views-in-dll.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5886f-129">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5886f-129">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="5886f-130">Legen Sie `MvcRazorCompileOnPublish` auf `true` fest, und ziehen Sie einen Paketverweis auf `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation` ein.</span><span class="sxs-lookup"><span data-stu-id="5886f-130">Set `MvcRazorCompileOnPublish` to `true` and include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span></span> <span data-ttu-id="5886f-131">Das folgende *CSPROJ*-Beispiel veranschaulicht diese Einstellungen:</span><span class="sxs-lookup"><span data-stu-id="5886f-131">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

