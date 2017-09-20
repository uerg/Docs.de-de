---
title: Razor-Ansicht-Kompilierung und Vorkompilierung
author: rick-anderson
description: "Ein Verweis Dokument erläutert, wie die MVC Razor-Ansicht-Kompilierung und Vorkompilierung in ASP.NET Core-Anwendungen zu ermöglichen."
keywords: ASP.NET Core, Razor-Ansicht-Kompilierung, Razor vor der Kompilierung, Razor Vorkompilierung
ms.author: riande
manager: wpickett
ms.date: 08/16/2017
ms.topic: article
ms.assetid: ab4705b7-1638-1638-bc97-ea7f292fe92a
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-compilation
ms.openlocfilehash: 0cb61315916d1b38f7cab3339e150c446fb69d98
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/20/2017
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a><span data-ttu-id="c78ef-104">Razor-Ansicht-Kompilierung und Vorkompilierung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c78ef-104">Razor view compilation and precompilation in ASP.NET Core</span></span>

<span data-ttu-id="c78ef-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c78ef-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c78ef-106">Razor-Ansichten werden zur Laufzeit kompiliert, wenn die Ansicht aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="c78ef-106">Razor views are compiled at runtime when the view is invoked.</span></span> <span data-ttu-id="c78ef-107">ASP.NET Core 1.1.0 und höheren Versionen kann optional kompilieren Razor-Ansichten und diese mit der app bereitstellen &mdash; genannten Vorkompilierung.</span><span class="sxs-lookup"><span data-stu-id="c78ef-107">ASP.NET Core 1.1.0 and higher can optionally compile Razor views and deploy them with the app &mdash; a process known as precompilation.</span></span> <span data-ttu-id="c78ef-108">Die Projektvorlagen für ASP.NET Core 2.x aktiviert Vorkompilierung standardmäßig.</span><span class="sxs-lookup"><span data-stu-id="c78ef-108">The ASP.NET Core 2.x project templates enable precompilation by default.</span></span>

> [!NOTE]
> <span data-ttu-id="c78ef-109">Razor-Ansicht Vorkompilierung ist nicht verfügbar, wenn auf diese Weise eine [Self-Contained Bereitstellung](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core-Version 2.0.0 und früher.</span><span class="sxs-lookup"><span data-stu-id="c78ef-109">Razor view precompilation is unavailable when doing a [Self-Contained Deployment](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core versions 2.0.0 and earlier.</span></span>

<span data-ttu-id="c78ef-110">Vorkompilierung Aspekte:</span><span class="sxs-lookup"><span data-stu-id="c78ef-110">Precompilation considerations:</span></span>

* <span data-ttu-id="c78ef-111">Vorkompilieren von Ansichten führt zu einer kleineren veröffentlichte Paket und schneller gestartet.</span><span class="sxs-lookup"><span data-stu-id="c78ef-111">Precompiling views results in a smaller published bundle and faster startup time.</span></span>
* <span data-ttu-id="c78ef-112">Razor-Dateien kann nicht bearbeitet werden, nachdem Sie Ansichten vorkompilieren.</span><span class="sxs-lookup"><span data-stu-id="c78ef-112">You can't edit Razor files after you precompile views.</span></span> <span data-ttu-id="c78ef-113">Die bearbeiteten Sichten wird nicht in das veröffentlichte Paket vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="c78ef-113">The edited views won't be present in the published bundle.</span></span> 

<span data-ttu-id="c78ef-114">Vorkompilierte Sichten bereitstellen:</span><span class="sxs-lookup"><span data-stu-id="c78ef-114">To deploy precompiled views:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c78ef-115">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c78ef-115">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c78ef-116">Wenn Ihr Projekt auf .NET Framework abzielt, enthalten einen Verweis Paket auf `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`:</span><span class="sxs-lookup"><span data-stu-id="c78ef-116">If your project targets .NET Framework, include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

<span data-ttu-id="c78ef-117">Wenn das Projekt .NET Core als Ziel verwendet werden, sind keine Änderungen erforderlich.</span><span class="sxs-lookup"><span data-stu-id="c78ef-117">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="c78ef-118">Legen Sie die Projektvorlagen von ASP.NET Core 2.x implizit `MvcRazorCompileOnPublish` auf `true` standardmäßig, d. h. dieser Knoten kann sicher entfernt werden, aus der *csproj* Datei.</span><span class="sxs-lookup"><span data-stu-id="c78ef-118">The ASP.NET Core 2.x project templates implicitly set `MvcRazorCompileOnPublish` to `true` by default, which means this node can be safely removed from the *.csproj* file.</span></span> <span data-ttu-id="c78ef-119">Wenn Sie es vorziehen, die explizit sein, besteht keine Schäden in der Setting der `MvcRazorCompileOnPublish` Eigenschaft `true`.</span><span class="sxs-lookup"><span data-stu-id="c78ef-119">If you prefer to be explicit, there's no harm in setting the `MvcRazorCompileOnPublish` property to `true`.</span></span> <span data-ttu-id="c78ef-120">Die folgenden *csproj* Beispiel beschreibt diese Einstellung:</span><span class="sxs-lookup"><span data-stu-id="c78ef-120">The following *.csproj* sample highlights this setting:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c78ef-121">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c78ef-121">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c78ef-122">Legen Sie `MvcRazorCompileOnPublish` auf `true`, und schließen Sie einen Paket Verweis auf `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span><span class="sxs-lookup"><span data-stu-id="c78ef-122">Set `MvcRazorCompileOnPublish` to `true`, and include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span></span> <span data-ttu-id="c78ef-123">Die folgenden *csproj* Beispiel beschreibt diese Einstellungen:</span><span class="sxs-lookup"><span data-stu-id="c78ef-123">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---
