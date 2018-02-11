---
title: "Das Metapaket „Microsoft.AspNetCore.All“ für ASP.NET Core 2.x und höher"
author: Rick-Anderson
description: "Das Metapaket „Microsoft.AspNetCore.All“ enthält alle unterstützten ASP.NET Core- und Entity Framework Core-Pakete zusammen mit ihren Abhängigkeiten."
manager: wpickett
ms.author: riande
ms.date: 09/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/metapackage
ms.openlocfilehash: 07220fdae299723088fa85e452cedff5e5685bd7
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
#<a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a><span data-ttu-id="e916c-103">Das Metapaket „Microsoft.AspNetCore.All“ für ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e916c-103">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x</span></span>

<span data-ttu-id="e916c-104">Für dieses Feature ist ASP.NET Core 2.x für .NET Core 2.x erforderlich.</span><span class="sxs-lookup"><span data-stu-id="e916c-104">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="e916c-105">Das Metapaket [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) für ASP.NET Core enthält:</span><span class="sxs-lookup"><span data-stu-id="e916c-105">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="e916c-106">alle unterstützten Pakete des ASP.NET Core-Teams</span><span class="sxs-lookup"><span data-stu-id="e916c-106">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="e916c-107">alle unterstützten Pakete von Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="e916c-107">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="e916c-108">interne und Drittanbieterabhängigkeiten, die von ASP.NET Core und Entity Framework Core verwendet werden</span><span class="sxs-lookup"><span data-stu-id="e916c-108">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span> 

<span data-ttu-id="e916c-109">In dem Paket `Microsoft.AspNetCore.All` sind alle Features von ASP.NET Core 2.x und Entity Framework Core 2.x enthalten.</span><span class="sxs-lookup"><span data-stu-id="e916c-109">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="e916c-110">In den Standardprojektvorlagen wird dieses Paket verwendet.</span><span class="sxs-lookup"><span data-stu-id="e916c-110">The default project templates use this package.</span></span>

<span data-ttu-id="e916c-111">Die Versionsnummer des Metapakets `Microsoft.AspNetCore.All` stellt die ASP.NET Core-Version und die Entity Framework Core-Version dar (auf die .NET Core-Version ausgerichtet).</span><span class="sxs-lookup"><span data-stu-id="e916c-111">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version (aligned with the .NET Core version).</span></span>

<span data-ttu-id="e916c-112">Anwendungen, die das Metapaket `Microsoft.AspNetCore.All` verwenden, profitieren automatisch von dem [.NET Core-Laufzeitspeicher](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="e916c-112">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the [.NET Core Runtime Store](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="e916c-113">Der Laufzeitspeicher enthält alle Laufzeitobjekte, die für die Ausführung von ASP.NET Core 2.x-Anwendungen erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="e916c-113">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="e916c-114">Bei Verwendung des Metapakets `Microsoft.AspNetCore.All` werden **keine** Objekte aus den referenzierten ASP.NET Core NuGet-Paketen mit der Anwendung &mdash; bereitgestellt. Der .NET Core-Laufzeitspeicher enthält diese Objekte.</span><span class="sxs-lookup"><span data-stu-id="e916c-114">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="e916c-115">Die Objekte im Laufzeitspeicher sind zur Verbesserung der Startzeit der Anwendung vorkompiliert.</span><span class="sxs-lookup"><span data-stu-id="e916c-115">The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="e916c-116">Sie können den Trimmprozess für Pakete verwenden, um nicht verwendete Pakete zu entfernen.</span><span class="sxs-lookup"><span data-stu-id="e916c-116">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="e916c-117">Getrimmte Pakete werden aus der veröffentlichten Anwendungsausgabe ausgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="e916c-117">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="e916c-118">Die folgende *.csproj*-Datei verweist auf das Metapaket `Microsoft.AspNetCore.All` für ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="e916c-118">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[Main](..\mvc\views\view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=9)]
