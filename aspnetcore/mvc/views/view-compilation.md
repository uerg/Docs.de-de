---
title: Kompilierung und Vorkompilierung einer Razor-Datei in ASP.NET Core
author: rick-anderson
description: Erfahren Sie mehr über die Vorteile der Vorkompilierung von Razor-Dateien und über das Erreichen der Vorkompilierung einer Razor-Datei in einer ASP.NET Core-App.
manager: wpickett
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-compilation
ms.openlocfilehash: 03b11116a15c291452acd878e32cd015dc553dcc
ms.sourcegitcommit: 24c32648ab0c6f0be15333d7c23c1bf680858c43
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
ms.locfileid: "34336277"
---
# <a name="razor-file-compilation-in-aspnet-core"></a><span data-ttu-id="d360c-103">Kompilieren einer Razor-Datei in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d360c-103">Razor file compilation in ASP.NET Core</span></span>

<span data-ttu-id="d360c-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d360c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-1.1"
<span data-ttu-id="d360c-105">Eine Razor-Datei wird zur Laufzeit kompiliert, wenn die zugeordnete MVC-Ansicht aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="d360c-105">A Razor file is compiled at runtime, when the associated MVC view is invoked.</span></span> <span data-ttu-id="d360c-106">Die Veröffentlichung von Razor-Dateien zum Zeitpunkt der Erstellung wird nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="d360c-106">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="d360c-107">Razor-Dateien können optional zur Veröffentlichung kompiliert und mit der App über das Vorkompilierungstool bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="d360c-107">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>
::: moniker-end
::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="d360c-108">Eine Razor-Datei wird zur Laufzeit kompiliert, wenn die zugeordnete Razor-Seite oder MVC-Ansicht aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="d360c-108">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="d360c-109">Die Veröffentlichung von Razor-Dateien zum Zeitpunkt der Erstellung wird nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="d360c-109">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="d360c-110">Razor-Dateien können optional zur Veröffentlichung kompiliert und mit der App über das Vorkompilierungstool bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="d360c-110">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="d360c-111">Eine Razor-Datei wird zur Laufzeit kompiliert, wenn die zugeordnete Razor-Seite oder MVC-Ansicht aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="d360c-111">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="d360c-112">Razor-Dateien werden mit dem [Razor SDK](xref:mvc/razor-pages/sdk) zur Erstellung und zur Veröffentlichung kompiliert.</span><span class="sxs-lookup"><span data-stu-id="d360c-112">Razor files are compiled at both build and publish time using the [Razor SDK](xref:mvc/razor-pages/sdk).</span></span>
::: moniker-end

## <a name="precompilation-considerations"></a><span data-ttu-id="d360c-113">Was vor der Kompilierung beachtet werden muss</span><span class="sxs-lookup"><span data-stu-id="d360c-113">Precompilation considerations</span></span>

<span data-ttu-id="d360c-114">Im Folgenden werden Nebeneffekte der Vorkompilierung von Razor-Dateien aufgeführt:</span><span class="sxs-lookup"><span data-stu-id="d360c-114">The following are side effects of precompiling Razor files:</span></span>

* <span data-ttu-id="d360c-115">Kleineres veröffentlichtes Paket</span><span class="sxs-lookup"><span data-stu-id="d360c-115">A smaller published bundle</span></span>
* <span data-ttu-id="d360c-116">Schnellere Startzeit</span><span class="sxs-lookup"><span data-stu-id="d360c-116">A faster startup time</span></span>
* <span data-ttu-id="d360c-117">Razor-Dateien können nicht bearbeitet werden. Der zugehörige Inhalt fehlt im veröffentlichten Paket.</span><span class="sxs-lookup"><span data-stu-id="d360c-117">You can't edit Razor files&mdash;the associated content is absent from the published bundle.</span></span>

## <a name="deploy-precompiled-files"></a><span data-ttu-id="d360c-118">Bereitstellen vorkompilierter Dateien</span><span class="sxs-lookup"><span data-stu-id="d360c-118">Deploy precompiled files</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="d360c-119">Die Kompilierung zur Erstellung und Veröffentlichung von Razor-Dateien wird standardmäßig vom Razor SDK aktiviert.</span><span class="sxs-lookup"><span data-stu-id="d360c-119">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="d360c-120">Das Bearbeiten von Razor-Dateien, nachdem sie aktualisiert wurden, wird zum Zeitpunkt der Erstellung unterstützt.</span><span class="sxs-lookup"><span data-stu-id="d360c-120">Editing Razor files after they're updated is supported at build time.</span></span> <span data-ttu-id="d360c-121">Standardmäßig wird nur die kompilierte Datei *Views.dll*, ohne *CSHTML*-Dateien, mit Ihrer App bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="d360c-121">By default, only the compiled *Views.dll* and no *.cshtml* files are deployed with your app.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d360c-122">Das Razor SDK ist nur dann wirksam, wenn keine für die Vorkompilierung spezifischen Eigenschaften in der Projektdatei festgelegt sind.</span><span class="sxs-lookup"><span data-stu-id="d360c-122">The Razor SDK is effective only when no precompilation-specific properties are set in the project file.</span></span> <span data-ttu-id="d360c-123">Durch Festlegen der Eigenschaft `MvcRazorCompileOnPublish` der *CSPROJ*-Datei auf `true` wird das Razor SDK beispielsweise deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="d360c-123">For instance, setting the *.csproj* file's `MvcRazorCompileOnPublish` property to `true` disables the Razor SDK.</span></span>
::: moniker-end

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="d360c-124">Wenn Ihr Projekt .NET Framework als Ziel verwendet, installieren Sie das NuGet-Paket [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span><span class="sxs-lookup"><span data-stu-id="d360c-124">If your project targets .NET Framework, install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package:</span></span>

<span data-ttu-id="d360c-125">[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]</span><span class="sxs-lookup"><span data-stu-id="d360c-125">[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]</span></span>

<span data-ttu-id="d360c-126">Wenn Ihr Paket für .NET Core gedacht ist, sind keine Änderungen erforderlich.</span><span class="sxs-lookup"><span data-stu-id="d360c-126">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="d360c-127">Mit den Projektvorlagen von ASP.NET Core 2.x wird die Eigenschaft `MvcRazorCompileOnPublish` standardmäßig explizit auf `true` festgelegt.</span><span class="sxs-lookup"><span data-stu-id="d360c-127">The ASP.NET Core 2.x project templates implicitly set the `MvcRazorCompileOnPublish` property to `true` by default.</span></span> <span data-ttu-id="d360c-128">Dieses Element kann folglich sicher aus der *CSPROJ*-Datei entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="d360c-128">Consequently, this element can be safely removed from the *.csproj* file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d360c-129">Die Vorkompilierung von Razor-Dateien steht aktuell beim Durchführen einer [eigenständigen Bereitstellung (Self-Contained Deployment, SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0 nicht zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="d360c-129">Razor file precompilation is unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span>
::: moniker-end

::: moniker range="= aspnetcore-1.1"
<span data-ttu-id="d360c-130">Legen Sie die Eigenschaft `MvcRazorCompileOnPublish` auf `true` fest und installieren Sie das NuGet-Paket [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/).</span><span class="sxs-lookup"><span data-stu-id="d360c-130">Set the `MvcRazorCompileOnPublish` property to `true`, and install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package.</span></span> <span data-ttu-id="d360c-131">Das folgende *CSPROJ*-Beispiel veranschaulicht diese Einstellungen:</span><span class="sxs-lookup"><span data-stu-id="d360c-131">The following *.csproj* sample highlights these settings:</span></span>

<span data-ttu-id="d360c-132">[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]</span><span class="sxs-lookup"><span data-stu-id="d360c-132">[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]</span></span>
::: moniker-end

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="d360c-133">Bereiten Sie die App mit dem [Veröffentlichungsbefehl der .NET Core-CLI](/dotnet/core/tools/dotnet-publish) für eine [Framework-abhängige Bereitstellung](/dotnet/core/deploying/#framework-dependent-deployments-fdd) vor.</span><span class="sxs-lookup"><span data-stu-id="d360c-133">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) with the [.NET Core CLI publish command](/dotnet/core/tools/dotnet-publish).</span></span> <span data-ttu-id="d360c-134">Führen Sie z. B. den folgenden Befehl auf der Stammebene des Projekts aus:</span><span class="sxs-lookup"><span data-stu-id="d360c-134">For example, execute the following command at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="d360c-135">Die Datei *<project_name>.PrecompiledViews.dll*, welche die kompilierten Razor-Dateien enthält, wird erzeugt, wenn die Vorkompilierung erfolgreich abgeschlossen wurde.</span><span class="sxs-lookup"><span data-stu-id="d360c-135">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor files, is produced when precompilation succeeds.</span></span> <span data-ttu-id="d360c-136">Im unten stehenden Screenshot wird beispielsweise der Inhalt von *Index.cshtml* in *WebApplication1.PrecompiledViews.dll* dargestellt:</span><span class="sxs-lookup"><span data-stu-id="d360c-136">For example, the screenshot below depicts the contents of *Index.cshtml* within *WebApplication1.PrecompiledViews.dll*:</span></span>

![Razor-Ansichten in einer DLL](view-compilation/_static/razor-views-in-dll.png)
::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="d360c-138">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="d360c-138">Additional resources</span></span>

::: moniker range="= aspnetcore-1.1"
* <xref:mvc/views/overview>
::: moniker-end

::: moniker range="= aspnetcore-2.0"
* <xref:mvc/razor-pages/index>
* <xref:mvc/views/overview>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
* <xref:mvc/razor-pages/index>
* <xref:mvc/views/overview>
* <xref:mvc/razor-pages/sdk>
::: moniker-end