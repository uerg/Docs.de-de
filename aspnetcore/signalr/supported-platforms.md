---
title: ASP.NET Core SignalR unterstützte Plattformen
author: tdykstra
description: Informationen Sie zu den unterstützten Plattformen für ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/12/2018
uid: signalr/supported-platforms
ms.openlocfilehash: be3d4d0049395fb2499bd0b4aac126e953ce7910
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861718"
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

Die [.NET Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) ausgeführt, die auf jeder Plattform, die von ASP.NET Core unterstützt wird. Z. B. [Xamarin-Entwickler können SignalR](https://github.com/aspnet/Announcements/issues/305) zum Erstellen von Android-apps mithilfe von Xamarin.Android 8.4.0.1 und höher sowie iOS-apps unter Verwendung von Xamarin.iOS 11.14.0.4 und höher.

Wenn der Server mit IIS ausgeführt wird, ist die WebSockets-Übertragung IIS 8.0 oder höher auf Windows Server 2012 oder höher erforderlich. Andere Transporte werden auf allen Plattformen unterstützt.

## <a name="java-client"></a>Java-client

Die [Java-Client](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) Java 8 und höheren Versionen unterstützt.

## <a name="unsupported-clients"></a>Nicht unterstützte clients

Die folgenden Clients sind verfügbar, aber es sind experimentelle oder inoffizielle. Sie werden derzeit nicht unterstützt und möglicherweise nie sein.

* [C++-client](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [SWIFT-client](https://github.com/moozzyk/SignalR-Client-Swift)
