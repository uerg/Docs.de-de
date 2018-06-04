---
title: Das Metapaket „Microsoft.AspNetCore.All“ für ASP.NET Core 2.0 und höher
author: Rick-Anderson
description: Das Metapaket „Microsoft.AspNetCore.All“ enthält alle unterstützten ASP.NET Core- und Entity Framework Core-Pakete zusammen mit ihren Abhängigkeiten.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/metapackage
ms.openlocfilehash: fbb76f41f3178ddc4e51faa14edece1869a30cd0
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729076"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a><span data-ttu-id="9a724-103">Das Metapaket „Microsoft.AspNetCore.All“ für ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="9a724-103">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.0</span></span>

> [!NOTE]
> <span data-ttu-id="9a724-104">Es wird empfohlen, für Anwendungen, die ASP.NET Core 2.1 und höher anzielen, das Paket [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) statt diesem Paket zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="9a724-104">We recommend applications targeting ASP.NET Core 2.1 and later use the [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) rather than this package.</span></span> <span data-ttu-id="9a724-105">Weitere Informationen finden Sie in diesem Artikel unter [Migrieren von Microsoft.AspNetCore.All zu Microsoft.AspNetCore.App](#migrate).</span><span class="sxs-lookup"><span data-stu-id="9a724-105">See [Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App](#migrate) in this article.</span></span>

<span data-ttu-id="9a724-106">Für dieses Feature ist ASP.NET Core 2.x für .NET Core 2.x erforderlich.</span><span class="sxs-lookup"><span data-stu-id="9a724-106">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="9a724-107">Das Metapaket [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) für ASP.NET Core enthält:</span><span class="sxs-lookup"><span data-stu-id="9a724-107">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="9a724-108">alle unterstützten Pakete des ASP.NET Core-Teams</span><span class="sxs-lookup"><span data-stu-id="9a724-108">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="9a724-109">alle unterstützten Pakete von Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="9a724-109">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="9a724-110">interne und Drittanbieterabhängigkeiten, die von ASP.NET Core und Entity Framework Core verwendet werden</span><span class="sxs-lookup"><span data-stu-id="9a724-110">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span> 

<span data-ttu-id="9a724-111">In dem Paket `Microsoft.AspNetCore.All` sind alle Features von ASP.NET Core 2.x und Entity Framework Core 2.x enthalten.</span><span class="sxs-lookup"><span data-stu-id="9a724-111">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="9a724-112">Die Standardprojektvorlagen für ASP.NET Core 2.0 verwenden dieses Paket.</span><span class="sxs-lookup"><span data-stu-id="9a724-112">The default project templates targeting ASP.NET Core 2.0 use this package.</span></span>

<span data-ttu-id="9a724-113">Die Versionsnummer des Metapakets `Microsoft.AspNetCore.All` stellt die ASP.NET Core-Version und die Entity Framework Core-Version dar.</span><span class="sxs-lookup"><span data-stu-id="9a724-113">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version.</span></span>

<span data-ttu-id="9a724-114">Anwendungen, die das Metapaket `Microsoft.AspNetCore.All` verwenden, profitieren automatisch von dem [.NET Core-Laufzeitspeicher](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="9a724-114">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the [.NET Core Runtime Store](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="9a724-115">Der Laufzeitspeicher enthält alle Laufzeitobjekte, die für die Ausführung von ASP.NET Core 2.x-Anwendungen erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="9a724-115">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="9a724-116">Bei Verwendung des Metapakets `Microsoft.AspNetCore.All` werden **keine** Objekte aus den referenzierten ASP.NET Core NuGet-Paketen mit der Anwendung &mdash; bereitgestellt. Der .NET Core-Laufzeitspeicher enthält diese Objekte.</span><span class="sxs-lookup"><span data-stu-id="9a724-116">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="9a724-117">Die Objekte im Laufzeitspeicher sind zur Verbesserung der Startzeit der Anwendung vorkompiliert.</span><span class="sxs-lookup"><span data-stu-id="9a724-117">The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="9a724-118">Sie können den Trimmprozess für Pakete verwenden, um nicht verwendete Pakete zu entfernen.</span><span class="sxs-lookup"><span data-stu-id="9a724-118">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="9a724-119">Getrimmte Pakete werden aus der veröffentlichten Anwendungsausgabe ausgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="9a724-119">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="9a724-120">Die folgende *.csproj*-Datei verweist auf das Metapaket `Microsoft.AspNetCore.All` für ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="9a724-120">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=6)]

<a name="migrate"></a>
## <a name="migrating-from-microsoftaspnetcoreall-to-microsoftaspnetcoreapp"></a><span data-ttu-id="9a724-121">Migrieren von Microsoft.AspNetCore.All zu Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="9a724-121">Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App</span></span>

<span data-ttu-id="9a724-122">Folgende Pakete sind in `Microsoft.AspNetCore.All`, aber nicht in `Microsoft.AspNetCore.App` enthalten.</span><span class="sxs-lookup"><span data-stu-id="9a724-122">The following packages are included in `Microsoft.AspNetCore.All` but not the `Microsoft.AspNetCore.App` package.</span></span> 

* `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServices.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServicesIntegration`
* `Microsoft.AspNetCore.DataProtection.AzureKeyVault`
* `Microsoft.AspNetCore.DataProtection.AzureStorage`
* `Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv`
* `Microsoft.AspNetCore.SignalR.Redis`
* `Microsoft.Data.Sqlite`
* `Microsoft.Data.Sqlite.Core`
* `Microsoft.EntityFrameworkCore.Sqlite`
* `Microsoft.EntityFrameworkCore.Sqlite.Core`
* `Microsoft.Extensions.Caching.Redis`
* `Microsoft.Extensions.Configuration.AzureKeyVault`
* `Microsoft.Extensions.Logging.AzureAppServices`
* `Microsoft.VisualStudio.Web.BrowserLink`

<span data-ttu-id="9a724-123">Wenn Sie von `Microsoft.AspNetCore.All` zu `Microsoft.AspNetCore.App` migrieren möchten und Ihre App APIs aus den oben aufgeführten Paketen verwendet, fügen Sie in Ihrem Projekt Verweise auf diese Pakete hinzu.</span><span class="sxs-lookup"><span data-stu-id="9a724-123">To move from `Microsoft.AspNetCore.All` to `Microsoft.AspNetCore.App`, if your app uses any APIs from the above packages, or packages brought in by those packages, add references to those packages in your project.</span></span>

<span data-ttu-id="9a724-124">Alle Abhängigkeiten der vorangehenden Pakete, die keine Abhängigkeiten von `Microsoft.AspNetCore.App` sind, sind nicht implizit enthalten.</span><span class="sxs-lookup"><span data-stu-id="9a724-124">Any dependencies of the preceding packages that otherwise aren't dependencies of `Microsoft.AspNetCore.App` are not included implicitly.</span></span> <span data-ttu-id="9a724-125">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="9a724-125">For example:</span></span>

* <span data-ttu-id="9a724-126">`StackExchange.Redis` als Abhängigkeit von `Microsoft.Extensions.Caching.Redis`</span><span class="sxs-lookup"><span data-stu-id="9a724-126">`StackExchange.Redis` as a dependency of `Microsoft.Extensions.Caching.Redis`</span></span>
* <span data-ttu-id="9a724-127">`Microsoft.ApplicationInsights` als Abhängigkeit von `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span><span class="sxs-lookup"><span data-stu-id="9a724-127">`Microsoft.ApplicationInsights` as a dependency of `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span></span>
