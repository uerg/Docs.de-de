---
title: Neuerungen in ASP.NET Core 1.1
author: rick-anderson
description: Neuerungen in ASP.NET Core 1.1
keywords: ASP.NET Core, Bower
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: 062f8353-d1bc-4e99-a821-c1d1bb162c47
ms.technology: aspnet
ms.prod: aspnet-core
uid: aspnetcore-1.1
ms.openlocfilehash: 7fdb00bc64cb20bd0e658b3a81814059404476d2
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
# <a name="whats-new-in-aspnet-core-11"></a><span data-ttu-id="c7172-104">Neuerungen in ASP.NET Core 1.1</span><span class="sxs-lookup"><span data-stu-id="c7172-104">What's new in ASP.NET Core 1.1</span></span>

<span data-ttu-id="c7172-105">Die folgenden Funktionen sind in ASP.NET Core 1.1 neu:</span><span class="sxs-lookup"><span data-stu-id="c7172-105">ASP.NET Core 1.1 includes the following new features:</span></span>

- [<span data-ttu-id="c7172-106">URL-umschreibende Middleware</span><span class="sxs-lookup"><span data-stu-id="c7172-106">URL Rewriting Middleware</span></span>](https://docs.microsoft.com/aspnet/core/fundamentals/url-rewriting)
- [<span data-ttu-id="c7172-107">Antworten zwischenspeichernde Middleware</span><span class="sxs-lookup"><span data-stu-id="c7172-107">Response Caching Middleware</span></span>](https://docs.microsoft.com/aspnet/core/performance/caching/middleware)
- [<span data-ttu-id="c7172-108">Anzeigen von Komponenten als Taghilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="c7172-108">View Components as Tag Helpers</span></span>](xref:mvc/views/view-components#invoking-a-view-component-as-a-tag-helper)
- [<span data-ttu-id="c7172-109">Middleware als MVC-Filter</span><span class="sxs-lookup"><span data-stu-id="c7172-109">Middleware as MVC filters</span></span>](xref:mvc/controllers/filters#using-middleware-in-the-filter-pipeline)
- [<span data-ttu-id="c7172-110">Cookie-basierter TempData-Anbieter</span><span class="sxs-lookup"><span data-stu-id="c7172-110">Cookie-based TempData provider</span></span>](xref:fundamentals/app-state#cookie-based-tempdata-provider )
- [<span data-ttu-id="c7172-111">Azure App Service-Protokollierungsanbieter</span><span class="sxs-lookup"><span data-stu-id="c7172-111">Azure App Service logging provider</span></span>](xref:fundamentals/logging#appservice)
- [<span data-ttu-id="c7172-112">Azure Key Vault-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="c7172-112">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
- [<span data-ttu-id="c7172-113">Schlüsselrepositorys zum Schutz von Azure und Redis-Speicher</span><span class="sxs-lookup"><span data-stu-id="c7172-113">Azure and Redis Storage Data Protection Key Repositories</span></span>](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis)
- [<span data-ttu-id="c7172-114">WebListener-Server für Windows</span><span class="sxs-lookup"><span data-stu-id="c7172-114">WebListener Server for Windows</span></span>](xref:fundamentals/servers/weblistener)
- [<span data-ttu-id="c7172-115">WebSockets-Unterstützung</span><span class="sxs-lookup"><span data-stu-id="c7172-115">WebSockets support</span></span>](xref:fundamentals/websockets)

## <a name="choosing-between-versions-10-and-11-of-aspnet-core"></a><span data-ttu-id="c7172-116">Wählen zwischen den Versionen 1.0 und 1.1 von ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c7172-116">Choosing between versions 1.0 and 1.1 of ASP.NET Core</span></span>

<span data-ttu-id="c7172-117">ASP.NET Core 1.1 bietet mehr Funktionen als 1.0.</span><span class="sxs-lookup"><span data-stu-id="c7172-117">ASP.NET Core 1.1 has more features than 1.0.</span></span> <span data-ttu-id="c7172-118">Allgemeinen wird empfohlen, die neueste Version zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="c7172-118">In general, we recommend you use the latest version.</span></span>

## <a name="additional-information"></a><span data-ttu-id="c7172-119">Zusätzliche Informationen</span><span class="sxs-lookup"><span data-stu-id="c7172-119">Additional Information</span></span>

- [<span data-ttu-id="c7172-120">ASP.NET Core 1.1.0: Anmerkungen zu dieser Version</span><span class="sxs-lookup"><span data-stu-id="c7172-120">ASP.NET Core 1.1.0 Release Notes</span></span>](https://github.com/aspnet/Home/releases/tag/1.1.0)
- <span data-ttu-id="c7172-121">Wenn Sie über den Fortschritt und die Pläne des ASP.NET Core-Entwicklungsteams auf dem Laufenden bleiben wollen, sehen Sie sich das wöchentliche [ASP.NET Community Standup](https://live.asp.net/) an.</span><span class="sxs-lookup"><span data-stu-id="c7172-121">If you’d like to connect with the ASP.NET Core development team’s progress and plans, tune in to the weekly [ASP.NET Community Standup](https://live.asp.net/).</span></span>
