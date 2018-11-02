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
# <a name="aspnet-core-signalr-supported-platforms"></a>ASP.NET Core SignalR unterstützte Plattformen

## <a name="server-system-requirements"></a>Server – Systemanforderungen

SignalR für ASP.NET Core unterstützt Serverplattform, die ASP.NET Core unterstützt.

## <a name="javascript-client"></a>JavaScript-client

Die [JavaScript-Client](https://www.npmjs.com/package/@aspnet/signalr) auf NodeJS-8 und höheren Versionen und den folgenden Browsern ausgeführt wird:

| Browser                         | Version |
| ------------------------------- | ------- |
| Microsoft Edge                  | Aktuell |
| Mozilla Firefox                 | Aktuell |
| Google Chrome; Android enthält | Aktuell |
| Safari; iOS enthält            | Aktuell |
| Microsoft Internet Explorer     | 11      |
 
## <a name="net-client"></a>.NET-Client

Die [.NET Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) ausgeführt, die auf alle Server-Plattform, die von ASP.NET Core unterstützt wird.

Wenn der Server mit IIS ausgeführt wird, ist die WebSockets-Übertragung IIS 8.0 oder höher auf Windows Server 2012 oder höher erforderlich. Andere Transporte werden auf allen Plattformen unterstützt.

## <a name="java-client"></a>Java-client

Die [Java-Client](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) Java 8 und höheren Versionen unterstützt.

## <a name="unsupported-clients"></a>Nicht unterstützte clients

Die folgenden Clients sind verfügbar, aber es sind experimentelle oder inoffizielle. Sie werden derzeit nicht unterstützt und möglicherweise nie sein.

* [C++-client](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [SWIFT-client](https://github.com/moozzyk/SignalR-Client-Swift)
