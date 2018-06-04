---
title: Neuerungen in ASP.NET Core 1.1
author: rick-anderson
description: Informationen zu den neuen Features in ASP.NET Core 1.1.
manager: wpickett
monikerRange: = aspnetcore-1.1
ms.author: riande
ms.date: 02/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: aspnetcore-1.1
ms.openlocfilehash: 6042fa2d5d05f37923adde1be7f8ce57b00196b9
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34566853"
---
# <a name="whats-new-in-aspnet-core-11"></a><span data-ttu-id="45a7a-103">Neuerungen in ASP.NET Core 1.1</span><span class="sxs-lookup"><span data-stu-id="45a7a-103">What's new in ASP.NET Core 1.1</span></span>

<span data-ttu-id="45a7a-104">Die folgenden Funktionen sind in ASP.NET Core 1.1 neu:</span><span class="sxs-lookup"><span data-stu-id="45a7a-104">ASP.NET Core 1.1 includes the following new features:</span></span>

- [<span data-ttu-id="45a7a-105">URL-umschreibende Middleware</span><span class="sxs-lookup"><span data-stu-id="45a7a-105">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting)
- [<span data-ttu-id="45a7a-106">Antworten zwischenspeichernde Middleware</span><span class="sxs-lookup"><span data-stu-id="45a7a-106">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
- [<span data-ttu-id="45a7a-107">Anzeigen von Komponenten als Taghilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="45a7a-107">View Components as Tag Helpers</span></span>](xref:mvc/views/view-components#invoking-a-view-component-as-a-tag-helper)
- [<span data-ttu-id="45a7a-108">Middleware als MVC-Filter</span><span class="sxs-lookup"><span data-stu-id="45a7a-108">Middleware as MVC filters</span></span>](xref:mvc/controllers/filters#using-middleware-in-the-filter-pipeline)
- [<span data-ttu-id="45a7a-109">Cookie-basierter TempData-Anbieter</span><span class="sxs-lookup"><span data-stu-id="45a7a-109">Cookie-based TempData provider</span></span>](xref:fundamentals/app-state#tempdata)
- [<span data-ttu-id="45a7a-110">Azure App Service-Protokollierungsanbieter</span><span class="sxs-lookup"><span data-stu-id="45a7a-110">Azure App Service logging provider</span></span>](xref:fundamentals/logging/index#azure-app-service-provider)
- [<span data-ttu-id="45a7a-111">Azure Key Vault-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="45a7a-111">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
- [<span data-ttu-id="45a7a-112">Schlüsselrepositorys zum Schutz von Azure und Redis-Speicher</span><span class="sxs-lookup"><span data-stu-id="45a7a-112">Azure and Redis Storage Data Protection Key Repositories</span></span>](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis)
- [<span data-ttu-id="45a7a-113">WebListener-Server für Windows</span><span class="sxs-lookup"><span data-stu-id="45a7a-113">WebListener Server for Windows</span></span>](xref:fundamentals/servers/weblistener)
- [<span data-ttu-id="45a7a-114">WebSockets-Unterstützung</span><span class="sxs-lookup"><span data-stu-id="45a7a-114">WebSockets support</span></span>](xref:fundamentals/websockets)

## <a name="choosing-between-versions-10-and-11-of-aspnet-core"></a><span data-ttu-id="45a7a-115">Wählen zwischen den Versionen 1.0 und 1.1 von ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="45a7a-115">Choosing between versions 1.0 and 1.1 of ASP.NET Core</span></span>

<span data-ttu-id="45a7a-116">ASP.NET Core 1.1 bietet mehr Funktionen als 1.0.</span><span class="sxs-lookup"><span data-stu-id="45a7a-116">ASP.NET Core 1.1 has more features than 1.0.</span></span> <span data-ttu-id="45a7a-117">Allgemeinen wird empfohlen, die neueste Version zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="45a7a-117">In general, we recommend you use the latest version.</span></span>

## <a name="additional-information"></a><span data-ttu-id="45a7a-118">Zusätzliche Informationen</span><span class="sxs-lookup"><span data-stu-id="45a7a-118">Additional Information</span></span>

- [<span data-ttu-id="45a7a-119">ASP.NET Core 1.1.0: Anmerkungen zu dieser Version</span><span class="sxs-lookup"><span data-stu-id="45a7a-119">ASP.NET Core 1.1.0 Release Notes</span></span>](https://github.com/aspnet/Home/releases/tag/1.1.0)
- <span data-ttu-id="45a7a-120">Wenn Sie über den Fortschritt und die Pläne des ASP.NET Core-Entwicklungsteams auf dem Laufenden bleiben möchten, sehen Sie sich das wöchentliche [ASP.NET Community Standup](https://live.asp.net/) an.</span><span class="sxs-lookup"><span data-stu-id="45a7a-120">To connect with the ASP.NET Core development team's progress and plans, tune in to the [ASP.NET Community Standup](https://live.asp.net/).</span></span>
