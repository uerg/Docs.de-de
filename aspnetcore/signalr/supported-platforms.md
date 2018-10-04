---
title: ASP.NET Core SignalR unterstützte Plattformen
author: tdykstra
description: Unterstützte Plattformen für ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/26/2018
uid: signalr/supported-platforms
ms.openlocfilehash: d6d74a55d35ddb34a6f66a171bfe3f343dd61b63
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577625"
---
# <a name="aspnet-core-signalr-supported-platforms"></a><span data-ttu-id="17a2c-103">ASP.NET Core SignalR unterstützte Plattformen</span><span class="sxs-lookup"><span data-stu-id="17a2c-103">ASP.NET Core SignalR supported platforms</span></span>

## <a name="server-system-requirements"></a><span data-ttu-id="17a2c-104">Server – Systemanforderungen</span><span class="sxs-lookup"><span data-stu-id="17a2c-104">Server system requirements</span></span>

<span data-ttu-id="17a2c-105">SignalR für ASP.NET Core unterstützt Server-Plattform, die ASP.NET Core unterstützt.</span><span class="sxs-lookup"><span data-stu-id="17a2c-105">SignalR for ASP.NET Core supports any server platform ASP.NET Core supports.</span></span>

## <a name="javascript-client"></a><span data-ttu-id="17a2c-106">JavaScript-client</span><span class="sxs-lookup"><span data-stu-id="17a2c-106">JavaScript client</span></span>

<span data-ttu-id="17a2c-107">Die [JavaScript-Client](https://www.npmjs.com/package/@aspnet/signalr) auf NodeJS-8 und höheren Versionen und den folgenden Browsern ausgeführt wird:</span><span class="sxs-lookup"><span data-stu-id="17a2c-107">The [JavaScript client](https://www.npmjs.com/package/@aspnet/signalr) runs on NodeJS 8 and later versions and the following browsers:</span></span>

| <span data-ttu-id="17a2c-108">Browser</span><span class="sxs-lookup"><span data-stu-id="17a2c-108">Browser</span></span> | <span data-ttu-id="17a2c-109">Version</span><span class="sxs-lookup"><span data-stu-id="17a2c-109">Version</span></span> |
| ------- | ------- |
| <span data-ttu-id="17a2c-110">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="17a2c-110">Microsoft Edge</span></span> | <span data-ttu-id="17a2c-111">Aktuell</span><span class="sxs-lookup"><span data-stu-id="17a2c-111">current</span></span> |
| <span data-ttu-id="17a2c-112">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="17a2c-112">Mozilla Firefox</span></span> | <span data-ttu-id="17a2c-113">Aktuell</span><span class="sxs-lookup"><span data-stu-id="17a2c-113">current</span></span> |
| <span data-ttu-id="17a2c-114">Google Chrome; Android enthält</span><span class="sxs-lookup"><span data-stu-id="17a2c-114">Google Chrome; includes Android</span></span> | <span data-ttu-id="17a2c-115">Aktuell</span><span class="sxs-lookup"><span data-stu-id="17a2c-115">current</span></span> |
| <span data-ttu-id="17a2c-116">Safari; iOS enthält</span><span class="sxs-lookup"><span data-stu-id="17a2c-116">Safari; includes iOS</span></span> | <span data-ttu-id="17a2c-117">Aktuell</span><span class="sxs-lookup"><span data-stu-id="17a2c-117">current</span></span> |
| <span data-ttu-id="17a2c-118">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="17a2c-118">Microsoft Internet Explorer</span></span> | <span data-ttu-id="17a2c-119">11</span><span class="sxs-lookup"><span data-stu-id="17a2c-119">11</span></span> |
 
## <a name="net-client"></a><span data-ttu-id="17a2c-120">.NET-Client</span><span class="sxs-lookup"><span data-stu-id="17a2c-120">.NET client</span></span>

<span data-ttu-id="17a2c-121">Die [.NET Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) ausgeführt, die auf alle Server-Plattform, die von ASP.NET Core unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="17a2c-121">The [.NET client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) runs on any server platform supported by ASP.NET Core.</span></span>

<span data-ttu-id="17a2c-122">Wenn der Server mit IIS ausgeführt wird, erfordert die WebSockets-Übertragung IIS 8.0 oder höher unter Windows Server 2012 oder höher.</span><span class="sxs-lookup"><span data-stu-id="17a2c-122">When the server runs IIS, the WebSockets transport requires IIS 8.0 or higher, on Windows Server 2012 or higher.</span></span> <span data-ttu-id="17a2c-123">Andere Transporte werden auf allen Plattformen unterstützt.</span><span class="sxs-lookup"><span data-stu-id="17a2c-123">Other transports are supported on all platforms.</span></span>

## <a name="java-client"></a><span data-ttu-id="17a2c-124">Java-client</span><span class="sxs-lookup"><span data-stu-id="17a2c-124">Java client</span></span>

<span data-ttu-id="17a2c-125">Die [Java-Client](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) Java 8 und höheren Versionen unterstützt.</span><span class="sxs-lookup"><span data-stu-id="17a2c-125">The [Java client](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) supports Java 8 and later versions.</span></span>

## <a name="unsupported-clients"></a><span data-ttu-id="17a2c-126">Nicht unterstützte clients</span><span class="sxs-lookup"><span data-stu-id="17a2c-126">Unsupported clients</span></span>

<span data-ttu-id="17a2c-127">Die folgenden Clients sind verfügbar, aber es sind experimentelle oder inoffizielle.</span><span class="sxs-lookup"><span data-stu-id="17a2c-127">The following clients are available but are experimental or unofficial.</span></span> <span data-ttu-id="17a2c-128">Sie werden jetzt unterstützt und möglicherweise nicht immer unterstützt.</span><span class="sxs-lookup"><span data-stu-id="17a2c-128">They are not supported now and may not ever be supported.</span></span>

* [<span data-ttu-id="17a2c-129">C++-client</span><span class="sxs-lookup"><span data-stu-id="17a2c-129">C++ client</span></span>](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [<span data-ttu-id="17a2c-130">SWIFT-client</span><span class="sxs-lookup"><span data-stu-id="17a2c-130">Swift client</span></span>](https://github.com/moozzyk/SignalR-Client-Swift)
