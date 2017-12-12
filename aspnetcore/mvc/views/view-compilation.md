---
title: Razor-Ansicht-Kompilierung und Vorkompilierung
author: rick-anderson
description: "Ein Verweis Dokument erläutert, wie die MVC Razor-Ansicht-Kompilierung und Vorkompilierung in ASP.NET Core-Anwendungen zu ermöglichen."
keywords: ASP.NET Core, Razor-Ansicht-Kompilierung, Razor vor der Kompilierung, Razor Vorkompilierung
ms.author: riande
manager: wpickett
ms.date: 12/05/2017
ms.topic: article
ms.assetid: ab4705b7-1638-1638-bc97-ea7f292fe92a
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-compilation
ms.openlocfilehash: 873f6203f9e7b5bb14968dcec3f8d8e5548bd834
ms.sourcegitcommit: 282f69e8dd63c39bde97a6d72783af2970d92040
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2017
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a><span data-ttu-id="92698-104">Razor-Ansicht-Kompilierung und Vorkompilierung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="92698-104">Razor view compilation and precompilation in ASP.NET Core</span></span>

<span data-ttu-id="92698-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="92698-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="92698-106">Razor-Ansichten werden zur Laufzeit kompiliert, wenn die Ansicht aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="92698-106">Razor views are compiled at runtime when the view is invoked.</span></span> <span data-ttu-id="92698-107">ASP.NET Core 1.1.0 und höheren Versionen kann optional kompilieren Razor-Ansichten und diese mit der app bereitstellen&mdash;genannten Vorkompilierung.</span><span class="sxs-lookup"><span data-stu-id="92698-107">ASP.NET Core 1.1.0 and higher can optionally compile Razor views and deploy them with the app&mdash;a process known as precompilation.</span></span> <span data-ttu-id="92698-108">Die Projektvorlagen für ASP.NET Core 2.x aktiviert Vorkompilierung standardmäßig.</span><span class="sxs-lookup"><span data-stu-id="92698-108">The ASP.NET Core 2.x project templates enable precompilation by default.</span></span>

> [!NOTE]
> <span data-ttu-id="92698-109">Vorkompilierung der Razor-Ansicht ist zurzeit nicht verfügbar, beim Ausführen einer [eigenständige Bereitstellung (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="92698-109">Razor view precompilation is currently unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span> <span data-ttu-id="92698-110">Die Funktion wird für SCDs verfügbar sein, wenn 2.1 freigibt.</span><span class="sxs-lookup"><span data-stu-id="92698-110">The feature will be available for SCDs when 2.1 releases.</span></span> <span data-ttu-id="92698-111">Weitere Informationen finden Sie unter [Ansicht Kompilierung schlägt fehl, beim übergreifenden für Linux auf Windows Kompilieren](https://github.com/aspnet/MvcPrecompilation/issues/102).</span><span class="sxs-lookup"><span data-stu-id="92698-111">For more information, see [View compilation fails when cross-compiling for Linux on Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).</span></span>

<span data-ttu-id="92698-112">Vorkompilierung Aspekte:</span><span class="sxs-lookup"><span data-stu-id="92698-112">Precompilation considerations:</span></span>

* <span data-ttu-id="92698-113">Vorkompilieren von Ansichten führt zu einer kleineren veröffentlichte Paket und schneller gestartet.</span><span class="sxs-lookup"><span data-stu-id="92698-113">Precompiling views results in a smaller published bundle and faster startup time.</span></span>
* <span data-ttu-id="92698-114">Razor-Dateien kann nicht bearbeitet werden, nachdem Sie Ansichten vorkompilieren.</span><span class="sxs-lookup"><span data-stu-id="92698-114">You can't edit Razor files after you precompile views.</span></span> <span data-ttu-id="92698-115">Die bearbeiteten Sichten wird nicht in das veröffentlichte Paket vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="92698-115">The edited views won't be present in the published bundle.</span></span> 

<span data-ttu-id="92698-116">Vorkompilierte Sichten bereitstellen:</span><span class="sxs-lookup"><span data-stu-id="92698-116">To deploy precompiled views:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="92698-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="92698-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="92698-118">Wenn Ihr Projekt auf .NET Framework abzielt, enthalten einen Verweis Paket auf [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span><span class="sxs-lookup"><span data-stu-id="92698-118">If your project targets .NET Framework, include a package reference to [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

<span data-ttu-id="92698-119">Wenn das Projekt .NET Core als Ziel verwendet werden, sind keine Änderungen erforderlich.</span><span class="sxs-lookup"><span data-stu-id="92698-119">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="92698-120">Legen Sie die Projektvorlagen von ASP.NET Core 2.x implizit `MvcRazorCompileOnPublish` auf `true` standardmäßig, d. h. dieser Knoten kann sicher entfernt werden, aus der *csproj* Datei.</span><span class="sxs-lookup"><span data-stu-id="92698-120">The ASP.NET Core 2.x project templates implicitly set `MvcRazorCompileOnPublish` to `true` by default, which means this node can be safely removed from the *.csproj* file.</span></span> <span data-ttu-id="92698-121">Wenn Sie es vorziehen, die explizit sein, besteht keine Schäden in der Setting der `MvcRazorCompileOnPublish` Eigenschaft `true`.</span><span class="sxs-lookup"><span data-stu-id="92698-121">If you prefer to be explicit, there's no harm in setting the `MvcRazorCompileOnPublish` property to `true`.</span></span> <span data-ttu-id="92698-122">Die folgenden *csproj* Beispiel beschreibt diese Einstellung:</span><span class="sxs-lookup"><span data-stu-id="92698-122">The following *.csproj* sample highlights this setting:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="92698-123">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="92698-123">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="92698-124">Legen Sie `MvcRazorCompileOnPublish` auf `true`, und schließen Sie einen Paket Verweis auf `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span><span class="sxs-lookup"><span data-stu-id="92698-124">Set `MvcRazorCompileOnPublish` to `true`, and include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span></span> <span data-ttu-id="92698-125">Die folgenden *csproj* Beispiel beschreibt diese Einstellungen:</span><span class="sxs-lookup"><span data-stu-id="92698-125">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

<span data-ttu-id="92698-126">Ein *< Project_name >. PrecompiledViews.dll* Datei, mit der kompilierten Razor-Ansichten wird ausgelöst, wenn Vorkompilierung erfolgreich ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="92698-126">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor views, is produced when precompilation succeeds.</span></span> <span data-ttu-id="92698-127">Der folgende Screenshot zeigt z. B. den Inhalt des *Index.cshtml* innerhalb eines *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="92698-127">For example, the screenshot below depicts the contents of *Index.cshtml* inside of *WebApplication1.PrecompiledViews.dll*:</span></span>

![Razor-Ansichten in DLL-Datei](view-compilation/_static/razor-views-in-dll.png)
