---
uid: signalr/overview/getting-started/supported-platforms
title: "Unterstützte Plattformen | Microsoft Docs"
author: pfletcher
description: "In diesem Artikel wird beschrieben, welche Clients und Servern von SignalR unterstützt werden."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: eac31beb-0f46-4afa-9def-e80904dea4f0
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/supported-platforms
msc.type: authoredcontent
ms.openlocfilehash: 7f41017a2a8c058c01fe6f89a2503eb5fa77048e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="supported-platforms"></a>Unterstützte Plattformen
====================
durch [Patrick Fletcher](https://github.com/pfletcher)

> In diesem Artikel wird beschrieben, welche Clients und Servern von SignalR unterstützt werden. 
> 
> ## <a name="questions-and-comments"></a>Fragen und Kommentare
> 
> Lassen Sie Sie Feedback auf wie in diesem Lernprogramm mögen und was wir in den Kommentaren am unteren Rand der Seite verbessern können. Wenn Sie Fragen, die nicht direkt mit dem Lernprogramm verknüpft sind haben, bereitstellen können, die [ASP.NET SignalR-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder [StackOverflow.com](http://stackoverflow.com/).


SignalR ist unter einer Vielzahl von Server- und Clientkonfigurationen unterstützt. Darüber hinaus weist jede Transportoption einen Satz von Anforderungen eigene; Wenn die Systemanforderungen für einen Transport nicht verfügbar sind, werden SignalR ordnungsgemäß Failover auf andere Transporte. Weitere Informationen über die Transporte, die SignalR unterstützt, finden Sie unter [Transporte und Zugriffe](introduction-to-signalr.md#transports).

## <a name="server-system-requirements"></a>Systemanforderungen

Die SignalR-Serverkomponente kann auf einer Vielzahl von Serverkonfigurationen gehostet werden. Dieser Abschnitt beschreibt die unterstützten Versionen von Betriebssystemen, .NET Framework, Internet Information Server und anderen Komponenten.

### <a name="supported-server-operating-systems"></a>Unterstützte Serverbetriebssysteme

In den folgenden Server oder Client-Betriebssystemen kann die SignalR-Serverkomponente gehostet werden. Beachten Sie, dass für SignalR verwendet WebSockets, Windows Server 2012 oder Windows 8 erforderlich ist (WebSocket genutzt werden auf Windows Azure-Websites, solange der Website .NET Framework, Version 4.5 festgelegt ist, und WebSockets Konfigurationsseite für den Standort aktiviert ist).

- Windows Server 2012
- Windows Server 2008 r2
- Windows 8
- Windows 7
- Windows Azure

### <a name="supported-server-net-framework-version"></a>Unterstützte Version von .NET Framework

SignalR-2 wird nur in .NET Framework 4.5 unterstützt. Finden Sie unter der [empfohlene Updates](#updates) Abschnitt, um Updates, die Zuverlässigkeit, Kompatibilität, Stabilität und Leistung zu verbessern.

### <a name="supported-server-iis-versions"></a>Unterstützte Serverversionen für IIS

Wenn SignalR in IIS gehostet wird, werden die folgenden Versionen unterstützt. Wenn ein Client-Betriebssystem verwendet wird, z. B. für die Entwicklung Vollversionen von IIS oder Cassini (Windows 8 oder Windows 7), nicht verwendet werden sollte, da es werden maximal 10 gleichzeitige Verbindungen auferlegt werden, die seit Verbindungen sehr schnell erreicht wird, vorübergehend, häufig neu eingerichtet, und werden werden nicht sofort freigegeben nach nicht mehr verwendet wird. IIS Express unter Clientbetriebssystemen vorgesehen.

Beachten Sie außerdem, dass für SignalR WebSocket, verwendet IIS 8 oder Express von IIS 8 verwendet werden muss, den der Server Windows 8, WindowsServer 2012 oder höher verwendet werden muss und WebSocket in IIS aktiviert werden muss. Informationen zum Aktivieren von WebSocket in IIS finden Sie unter [WebSocket-Protokoll-Unterstützung für IIS 8.0](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support).

- IIS 8 oder IIS 8 Express.
- IIS 7 und 7.5. Unterstützung für [URLs ohne Erweiterung](https://support.microsoft.com/kb/980368) ist erforderlich.
- IIS muss im integrierten Modus ausgeführt werden; im klassischer Modus wird nicht unterstützt. Nachricht Verzögerungen von bis zu 30 Sekunden möglicherweise auftreten können, wenn IIS im klassischen Modus unter Verwendung des Transports Server-Sent Ereignisse ausgeführt wird.
- Die hostanwendung muss im Modus mit vollständiger Vertrauenswürdigkeit ausgeführt werden.

## <a name="client-system-requirements"></a>Systemanforderungen für Clients

SignalR kann in einer Vielzahl von Client-Plattformen verwendet werden. Dieser Abschnitt beschreibt die Systemanforderungen für die Verwendung von SignalR in Webbrowsern, Windows-desktopanwendungen, Silverlight-Anwendungen und mobile Geräte.

### <a name="web-browsers"></a>Webbrowser

SignalR kann in einer Vielzahl von Webbrowsern verwendet werden, aber in der Regel nur die letzten beiden Versionen unterstützt werden.

Anwendungen, die in Browsern SignalR verwenden müssen jQuery-Version 1.6.4 oder höher Hauptversionen (z. B. 1.7.2, 1.8.2 oder 1.9.1) verwenden.

SignalR kann in den folgenden Browsern verwendet werden:

- Microsoft Internet Explorer-Versionen, 8, 9, 10 und 11. Modernes werden Desktop- und Mobile-Versionen unterstützt.
- Mozilla Firefox: aktuelle Version - 1, Windows und Mac-Versionen.
- Google Chrome: aktuelle Version - 1, Windows und Mac-Versionen.
- Safari: aktuelle Version - 1, Mac und iOS-Versionen.
- Opera: aktuelle Version - 1, nur Windows.
- Android-browser

Zusätzlich zu, die bestimmte Browsern erfordern, haben die verschiedene Transporte, die SignalR verwendet Ihren eigenen. Die folgenden Transportoptionen werden unter den folgenden Konfigurationen unterstützt:

<a id="browser"></a>

**Transport Anforderungen an Webbrowser**

| Transport | Internet Explorer | Chrome (Windows oder iOS) | Firefox | Safari (OSX oder iOS) | Android |
| --- | --- | --- | --- | --- | --- |
| WebSockets | 10+ | aktuelle - 1 | aktuelle - 1 | aktuelle - 1 | Nicht zutreffend |
| Vom Server gesendeten Ereignisse | Nicht zutreffend | aktuelle - 1 | aktuelle - 1 | aktuelle - 1 | Nicht zutreffend |
| ForeverFrame | 8+ | Nicht zutreffend | Nicht zutreffend | Nicht zutreffend | 4.1 |
| Langen Abruftransports | 8+ | aktuelle - 1 | aktuelle - 1 | aktuelle - 1 | 4.1 |

\*: 6 und höher für die vollständige Funktionalität erforderlich sind.

#### <a name="unsupported-browsers"></a>Nicht unterstützte Browser

Während der SignalR *möglicherweise* ausführen, ohne wichtige Aspekte, die in älteren Browserversionen, wir testen SignalR nicht aktiv darin und im Allgemeinen Fehlern, die in ihnen möglicherweise nicht behoben.

### <a name="windows-desktop-and-silverlight-applications"></a>Windows-Desktop und Silverlight-Anwendungen

Zusätzlich zu den in einem Webbrowser ausführen, kann SignalR in eigenständigen Windows-Client oder Silverlight-Anwendungen gehostet werden. Windows-Desktop und Silverlight SignalR-Anwendungen haben die folgenden Systemanforderungen.

- Anwendungen, die mit .NET 4 werden unter Windows XP SP3 oder höher unterstützt.
- Anwendungen mit .NET Framework 4.5 werden unter Windows Vista oder höher unterstützt.

Zusätzlich zu Betriebssystem und .NET Framework-Anforderungen gelten die Transporte, die für SignalR verfügbar eigene. Die folgenden Transportoptionen werden unter den folgenden Konfigurationen unterstützt:

**Windows-Desktop und Silverlight-Transport-Anforderungen**

| Transport | Anwendung für .NET | Silverlight |
| --- | --- | --- |
| WebSockets | Windows 8 und höher und .NET 4.5 + | Nicht zutreffend |
| Forever Frame | Nicht zutreffend | Nicht zutreffend |
| Vom Server gesendeten Ereignisse | .NET 4 + | 5+ |
| Langen Abruftransports | .NET 4 + | 5+ |

<a id="android"></a>

### <a name="windows-store-and-windows-phone-applications"></a>Windows Store und Windows Phone-Anwendungen

SignalR kann in Windows Store- und Windows Phone 8-Anwendungen verwendet werden. Die folgenden Transportoptionen werden unter den folgenden Konfigurationen unterstützt:

**Windows Store und Windows Phone-Transport-Anforderungen**

| Transport | Windows Store / .NET | Windows Store / JavaScript | Windows Phone / IE | Windows Phone / .NET |
| --- | --- | --- | --- | --- |
| WebSockets | Nicht zutreffend | Windows 8 + | 8+ | Nicht zutreffend |
| Forever Frame | Nicht zutreffend | Windows 8 + | 7.5+ | Nicht zutreffend |
| Vom Server gesendeten Ereignisse | Windows 8 + | Nicht zutreffend | Nicht zutreffend | 8+ |
| Langen Abruftransports | Windows 8 + | Windows 8 + | 7.5+ | 8+ |

<a id="updates"></a>

## <a name="recommended-updates"></a>Empfohlene Updates

Die folgenden Updates werden für SignalR-Server empfohlen:

- Ist ein Update für .NET Framework 4.5 verfügbar [hier](https://support.microsoft.com/kb/2750149).
- Microsoft wird in regelmäßigen Abständen QFEs für ASP.NET freizugeben. Diese sollten als verfügbar angewendet werden.
