---
uid: signalr/overview/performance/scaleout-in-signalr
title: Einführung in Warteschlangen für horizontale Skalierung in SignalR | Microsoft Docs
author: MikeWasson
description: Versionen der Software verwendete in diesem Thema Visual Studio 2013 .NET 4.5 SignalR Version 2-Vorgängerversionen des in diesem Thema Informationen zu früheren Versionen von...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 7e781fc1-1c1f-45a8-bc1d-338e96dbe9c9
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-in-signalr
msc.type: authoredcontent
ms.openlocfilehash: f1d15250682305f6d0512b72bd2e40cb4a8a18e5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
ms.locfileid: "28034593"
---
<a name="introduction-to-scaleout-in-signalr"></a>Einführung in Warteschlangen für horizontale Skalierung in SignalR
====================
durch [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

> ## <a name="software-versions-used-in-this-topic"></a>In diesem Thema verwendeten Versionen der Software
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR Version 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Frühere Versionen dieses Themas
> 
> Informationen zu früheren Versionen von SignalR finden Sie unter [ältere Versionen von SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Fragen und Kommentare
> 
> Lassen Sie Sie Feedback auf wie in diesem Lernprogramm mögen und was wir in den Kommentaren am unteren Rand der Seite verbessern können. Wenn Sie Fragen, die nicht direkt mit dem Lernprogramm verknüpft sind haben, bereitstellen können, die [ASP.NET SignalR-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder [StackOverflow.com](http://stackoverflow.com/).


Im Allgemeinen stehen zwei Möglichkeiten, eine Webanwendung skalieren: *hochskalieren* und *skalieren*.

- Verwenden einen größeren Server (oder eine umfangreichere VM) mit mehr Arbeitsspeicher, CPUs usw. bedeutet zentral hochskalieren.
- Horizontales Skalieren bedeutet das Hinzufügen weiterer Server, um die Last zu bewältigen.

Das Problem mit dem Zentrales Skalieren ist, dass Sie schnell eine Begrenzung der Größe des Computers drücken. Darüber hinaus müssen Sie zu skalieren. Jedoch, wenn Sie dezentral skalieren, Clients können auf verschiedenen Servern weitergeleitet werden. Ein Client, der mit einem Server verbunden ist wird von einem anderen Server gesendete Nachrichten nicht empfangen.

![](scaleout-in-signalr/_static/image1.png)

Eine Lösung besteht darin, Weiterleiten von Nachrichten zwischen Servern mit einer Komponente mit dem Namen einer *Rückwandplatine*. Mit einer Rückwandplatine aktiviert ist jede Anwendungsinstanz sendet Nachrichten an die Backplane und der Rückwand Weiterleitung an den anderen Anwendungsinstanzen. (Im Bereich der Elektronik ist eine Rückwandplatine eine Gruppe von parallelen Connectors. Entsprechend verbindet eine SignalR-Rückwandplatine mehrerer Server.)

![](scaleout-in-signalr/_static/image2.png)

SignalR stellt derzeit drei Backplanes bereit:

- **Azure Service Bus**. Service Bus ist eine messaging-Infrastruktur, die Komponenten zum Senden von Nachrichten in einer lose gekoppelten Weise zulässt.
- **Redis**. Redis ist ein in-Memory-Schlüssel / Wert-Speicher. Redis unterstützt ein ("Pub/Sub") veröffentlichen/abonnieren-Muster für das Senden von Nachrichten.
- **SQL Server**. Die SQL Server-Rückwandplatine schreibt Nachrichten in SQL-Tabellen. Der Rückwand verwendet Service Broker für effiziente messaging. Sie funktioniert aber auch, wenn Service Broker nicht aktiviert ist.

Wenn Sie Ihre Anwendung in Azure bereitstellen, sollten Sie mit der Redis-Rückwandplatine mit [Azure Redis Cache](https://azure.microsoft.com/services/cache/). Wenn Sie mit Ihren eigenen Serverfarm bereitstellen, sollten Sie die SQL Server- oder Redis Backplanes.

Die folgenden Themen enthalten schrittweise aufgebaute Lernprogramme für jede Rückwandplatine:

- [Horizontale Skalierung in SignalR mit dem Azure Service Bus](scaleout-with-windows-azure-service-bus.md)
- [Horizontale Skalierung in SignalR mit Redis](scaleout-with-redis.md)
- [Horizontale Skalierung in SignalR mit SQL Server](scaleout-with-sql-server.md)

## <a name="implementation"></a>Implementierung

In SignalR beziehen wird jede Nachricht über einen Nachrichtenbus gesendet. Implementiert ein Nachrichtenbus der [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) -Schnittstelle, die eine Abstraktion zum Veröffentlichen/Abonnieren bereitstellt. Die Backplanes arbeiten möchten, indem Sie durch das Ersetzen der standardmäßigen **IMessageBus** mit einem Bus für diese Rückwandplatine konzipiert. Der Nachrichtenbus für Redis ist z. B. [RedisMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.redis.redismessagebus(v=vs.100).aspx), und nutzt die Redis [Pub/Sub-](http://redis.io/topics/pubsub) Mechanismus zum Senden und Empfangen von Nachrichten.

Jede Instanz eines Servers eine Verbindung mit der Rückwand über den Bus. Wenn eine Nachricht gesendet wird, geht Sie an der Rückwand, und der Rückwand sendet sie an jedem Server. Wenn ein Server eine Nachricht aus der Rückwand abruft, legt er die Nachricht im lokalen Cache. Der Server übermittelt dann Nachrichten für Clients aus dem lokalen Cache.

Für jede Clientverbindung wird der Client-Status im Lesen des Streams Nachricht mithilfe eines Cursors nachverfolgt. (Für ein Cursor stellt eine Position im Nachrichtenstream dar.) Wenn ein Client die Verbindung trennt, und klicken Sie dann erneut eine Verbindung herstellt, fordert er den Bus für alle Nachrichten, die nach dem Wert für den Client-Cursor eingetroffen sind. Das gleiche passiert, wenn eine Verbindung verwendet [lange](../getting-started/introduction-to-signalr.md#transports). Nachdem eine lange Abfrage-Anforderung abgeschlossen wurde, wird der Client öffnet eine neue Verbindung und fragt nach Nachrichten, die nach dem Cursor eingetroffen sind.

Die Cursor Mechanismus funktioniert auch, wenn ein Client auf einem anderen Server weitergeleitet wird erneut eine Verbindung herzustellen. Der Rückwand ist bekannt, dass alle Server, und es spielt keine Rolle, welche Server, die ein Client mit verbunden.

## <a name="limitations"></a>Einschränkungen

Durch Verwendung einer Rückwandplatine, ist der maximale Nachrichtendurchsatz kleiner ist, wenn Clients direkt mit einem Serverknoten kommunizieren. Ist, dass der Rückwand jede Nachricht auf jedem Knoten weiterleitet, damit der Rückwand zu einem Engpass werden kann. Ob diese Einschränkung ein Problem aufgetreten ist, hängt von der Anwendung ab. Hier sind z. B. einige typische SignalR-Szenarios:

- [Server-Broadcast](../getting-started/tutorial-server-broadcast-with-signalr.md) (z. B. Börsenticker): Backplanes eignen sich gut für dieses Szenario, da der Server die Rate kontrolliert, an dem Nachrichten gesendet werden.
- [Client-zu-Client](../getting-started/tutorial-getting-started-with-signalr.md) (z. B. chat): In diesem Szenario kann der Rückwand einen Engpass sein, wenn die Anzahl der Nachrichten mit der Anzahl der Clients skaliert; d. h., wenn die Anzahl der Nachrichten vergrößert wird als proportional mehr Clients verknüpfen.
- [Hochfrequente Echtzeit](../getting-started/tutorial-high-frequency-realtime-with-signalr.md) (z. B. Echtzeit Spiele): Rückwandplatine für dieses Szenario nicht empfohlen wird.

## <a name="enabling-tracing-for-signalr-scaleout"></a>Aktivieren der Ablaufverfolgung für SignalR mit horizontaler Skalierung

Fügen Sie zum Aktivieren der Ablaufverfolgung für die Backplanes in den folgenden Abschnitten hinzu, unter dem Stammverzeichnis der Datei "Web.config" **Konfiguration** Element:

[!code-html[Main](scaleout-in-signalr/samples/sample1.html)]
