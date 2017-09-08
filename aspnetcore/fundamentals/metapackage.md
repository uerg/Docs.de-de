---
title: "Microsoft.AspNetCore.All Metapackage für ASP.NET Core 2.x und höher"
author: Rick-Anderson
description: "Microsoft.AspNetCore.All Metapackage umfasst alle unterstützten Pakete."
keywords: ASP.NET Core, die NuGet-Paket, Microsoft.AspNetCore.All, Metapackage
ms.author: riande
manager: wpickett
ms.date: 07/16/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/metapackage
ms.openlocfilehash: 255438a4ce36ce4978f8c8ee298388a25ac00d17
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
#<a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a><span data-ttu-id="c03a1-104">Microsoft.AspNetCore.All Metapackage für ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c03a1-104">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x</span></span>

<span data-ttu-id="c03a1-105">Diese Funktion erfordert ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="c03a1-105">This feature requires ASP.NET Core 2.x.</span></span>

<span data-ttu-id="c03a1-106">Die [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) Metapackage für ASP.NET Core enthält:</span><span class="sxs-lookup"><span data-stu-id="c03a1-106">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="c03a1-107">Alle unterstützten Pakete vom Team ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c03a1-107">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="c03a1-108">Alle unterstützten Pakete durch das Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c03a1-108">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="c03a1-109">Interne und 3rd Party Abhängigkeiten von ASP.NET Core und Entity Framework Core verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="c03a1-109">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span> 

<span data-ttu-id="c03a1-110">Alle Funktionen von ASP.NET Core 2.x und Entity Framework Core 2.x befinden sich die `Microsoft.AspNetCore.All` Paket.</span><span class="sxs-lookup"><span data-stu-id="c03a1-110">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="c03a1-111">Die Standard-Projektvorlagen verwenden dieses Paket an.</span><span class="sxs-lookup"><span data-stu-id="c03a1-111">The default project templates use this package.</span></span>

<span data-ttu-id="c03a1-112">Die Versionsnummer der `Microsoft.AspNetCore.All` Metapackage darstellt, die ASP.NET Core und Entity Framework Core-Version (mit der Version von .NET Core ausgerichtet).</span><span class="sxs-lookup"><span data-stu-id="c03a1-112">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version (aligned with the .NET Core version).</span></span>

<span data-ttu-id="c03a1-113">Anwendungen, die die `Microsoft.AspNetCore.All` Metapackage profitieren Sie automatisch von der .NET Core-Runtime-Speicher.</span><span class="sxs-lookup"><span data-stu-id="c03a1-113">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the .NET Core Runtime Store.</span></span> <span data-ttu-id="c03a1-114">Der Common Language Runtime-Speicher enthält alle Common Language Runtime-Objekte, die zum Ausführen von ASP.NET Core 2.x-Clientanwendungen erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="c03a1-114">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="c03a1-115">Bei Verwendung der `Microsoft.AspNetCore.All` Metapackage, **keine** Objekte aus der referenzierten ASP.NET Core NuGet-Pakete werden mit der Anwendung bereitgestellt &mdash; .NET Core-Runtime-Store enthält diese Ressourcen.</span><span class="sxs-lookup"><span data-stu-id="c03a1-115">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="c03a1-116"><!-- todo add link to Runtime store -->Die Ressourcen in der Common Language Runtime Speicher vorkompiliert um Anwendungsstartzeit zu verbessern.</span><span class="sxs-lookup"><span data-stu-id="c03a1-116"><!-- todo add link to Runtime store --> The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="c03a1-117">Sie können das Trimming gliedert verwenden, so entfernen Sie Pakete, die Sie nicht verwenden.</span><span class="sxs-lookup"><span data-stu-id="c03a1-117">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="c03a1-118">Zugeschnittene Pakete werden in der veröffentlichten Anwendungsausgabe ausgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="c03a1-118">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="c03a1-119">Die folgenden *csproj* Dateiverweise der `Microsoft.AspNetCore.All` Metapackage für ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="c03a1-119">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

<span data-ttu-id="c03a1-120">[!code-xml[Main](..\mvc\views\view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=9)]</span><span class="sxs-lookup"><span data-stu-id="c03a1-120">[!code-xml[Main](..\mvc\views\view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=9)]</span></span>
