---
title: ASP.NET Core SignalR unterstützte Plattformen
author: tdykstra
description: Informationen Sie zu den unterstützten Plattformen für ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/31/2018
uid: signalr/supported-platforms
ms.openlocfilehash: 773f6c020dbb2982911e177b55855473c750d52a
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758179"
---
# <a name="aspnet-core-signalr-supported-platforms"></a><span data-ttu-id="1466e-103">ASP.NET Core SignalR unterstützte Plattformen</span><span class="sxs-lookup"><span data-stu-id="1466e-103">ASP.NET Core SignalR supported platforms</span></span>

## <a name="server-system-requirements"></a><span data-ttu-id="1466e-104">Server – Systemanforderungen</span><span class="sxs-lookup"><span data-stu-id="1466e-104">Server system requirements</span></span>

<span data-ttu-id="1466e-105">SignalR für ASP.NET Core unterstützt Serverplattform, die ASP.NET Core unterstützt.</span><span class="sxs-lookup"><span data-stu-id="1466e-105">SignalR for ASP.NET Core supports any server platform that ASP.NET Core supports.</span></span>

## <a name="javascript-client"></a><span data-ttu-id="1466e-106">JavaScript-client</span><span class="sxs-lookup"><span data-stu-id="1466e-106">JavaScript client</span></span>

<span data-ttu-id="1466e-107">Die [JavaScript-Client](https://www.npmjs.com/package/@aspnet/signalr) auf NodeJS-8 und höheren Versionen und den folgenden Browsern ausgeführt wird:</span><span class="sxs-lookup"><span data-stu-id="1466e-107">The [JavaScript client](https://www.npmjs.com/package/@aspnet/signalr) runs on NodeJS 8 and later versions and the following browsers:</span></span>

| <span data-ttu-id="1466e-108">Browser</span><span class="sxs-lookup"><span data-stu-id="1466e-108">Browser</span></span>                         | <span data-ttu-id="1466e-109">Version</span><span class="sxs-lookup"><span data-stu-id="1466e-109">Version</span></span> |
| ------------------------------- | ------- |
| <span data-ttu-id="1466e-110">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="1466e-110">Microsoft Edge</span></span>                  | <span data-ttu-id="1466e-111">Aktuell</span><span class="sxs-lookup"><span data-stu-id="1466e-111">current</span></span> |
| <span data-ttu-id="1466e-112">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="1466e-112">Mozilla Firefox</span></span>                 | <span data-ttu-id="1466e-113">Aktuell</span><span class="sxs-lookup"><span data-stu-id="1466e-113">current</span></span> |
| <span data-ttu-id="1466e-114">Google Chrome; Android enthält</span><span class="sxs-lookup"><span data-stu-id="1466e-114">Google Chrome; includes Android</span></span> | <span data-ttu-id="1466e-115">Aktuell</span><span class="sxs-lookup"><span data-stu-id="1466e-115">current</span></span> |
| <span data-ttu-id="1466e-116">Safari; iOS enthält</span><span class="sxs-lookup"><span data-stu-id="1466e-116">Safari; includes iOS</span></span>            | <span data-ttu-id="1466e-117">Aktuell</span><span class="sxs-lookup"><span data-stu-id="1466e-117">current</span></span> |
| <span data-ttu-id="1466e-118">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="1466e-118">Microsoft Internet Explorer</span></span>     | <span data-ttu-id="1466e-119">11</span><span class="sxs-lookup"><span data-stu-id="1466e-119">11</span></span>      |
 
## <a name="net-client"></a><span data-ttu-id="1466e-120">.NET-Client</span><span class="sxs-lookup"><span data-stu-id="1466e-120">.NET client</span></span>

<span data-ttu-id="1466e-121">Die [.NET Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) ausgeführt, die auf alle Server-Plattform, die von ASP.NET Core unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="1466e-121">The [.NET client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) runs on any server platform supported by ASP.NET Core.</span></span>

<span data-ttu-id="1466e-122">Wenn der Server mit IIS ausgeführt wird, ist die WebSockets-Übertragung IIS 8.0 oder höher auf Windows Server 2012 oder höher erforderlich.</span><span class="sxs-lookup"><span data-stu-id="1466e-122">If the server runs IIS, the WebSockets transport requires IIS 8.0 or higher on Windows Server 2012 or higher.</span></span> <span data-ttu-id="1466e-123">Andere Transporte werden auf allen Plattformen unterstützt.</span><span class="sxs-lookup"><span data-stu-id="1466e-123">Other transports are supported on all platforms.</span></span>

## <a name="java-client"></a><span data-ttu-id="1466e-124">Java-client</span><span class="sxs-lookup"><span data-stu-id="1466e-124">Java client</span></span>

<span data-ttu-id="1466e-125">Die [Java-Client](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) Java 8 und höheren Versionen unterstützt.</span><span class="sxs-lookup"><span data-stu-id="1466e-125">The [Java client](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) supports Java 8 and later versions.</span></span>

## <a name="unsupported-clients"></a><span data-ttu-id="1466e-126">Nicht unterstützte clients</span><span class="sxs-lookup"><span data-stu-id="1466e-126">Unsupported clients</span></span>

<span data-ttu-id="1466e-127">Die folgenden Clients sind verfügbar, aber es sind experimentelle oder inoffizielle.</span><span class="sxs-lookup"><span data-stu-id="1466e-127">The following clients are available but are experimental or unofficial.</span></span> <span data-ttu-id="1466e-128">Sie werden derzeit nicht unterstützt und möglicherweise nie sein.</span><span class="sxs-lookup"><span data-stu-id="1466e-128">They aren't currently supported and may never be.</span></span>

* [<span data-ttu-id="1466e-129">C++-client</span><span class="sxs-lookup"><span data-stu-id="1466e-129">C++ client</span></span>](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [<span data-ttu-id="1466e-130">SWIFT-client</span><span class="sxs-lookup"><span data-stu-id="1466e-130">Swift client</span></span>](https://github.com/moozzyk/SignalR-Client-Swift)
