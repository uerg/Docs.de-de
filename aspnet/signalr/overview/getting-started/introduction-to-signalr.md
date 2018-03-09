---
uid: signalr/overview/getting-started/introduction-to-signalr
title: "Einführung in SignalR | Microsoft Docs"
author: pfletcher
description: "In diesem Artikel wird beschrieben, was eine SignalR ist und einige der Lösungen, die dieser Standard entwickelt wurde, um zu erstellen."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 0fab5e35-8c1f-43d4-8635-b8aba8766a71
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/introduction-to-signalr
msc.type: authoredcontent
ms.openlocfilehash: 5bb49c9c2405d232ba5e067d99f8879b3bc99361
ms.sourcegitcommit: 53ee14b9c8200f44705d8997c3619fa874192d45
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2018
---
<a name="introduction-to-signalr"></a>Einführung in SignalR
====================
durch [Patrick Fletcher](https://github.com/pfletcher)

> In diesem Artikel wird beschrieben, was eine SignalR ist und einige der Lösungen, die dieser Standard entwickelt wurde, um zu erstellen. 
> 
> ## <a name="questions-and-comments"></a>Fragen und Kommentare
> 
> Lassen Sie Sie Feedback auf wie in diesem Lernprogramm mögen und was wir in den Kommentaren am unteren Rand der Seite verbessern können. Wenn Sie Fragen, die nicht direkt mit dem Lernprogramm verknüpft sind haben, bereitstellen können, die [ASP.NET SignalR-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder [StackOverflow.com](https://stackoverflow.com/questions/tagged/signalr).


## <a name="what-is-signalr"></a>Was eine SignalR ist?

ASP.NET SignalR ist eine Bibliothek für ASP.NET-Entwickler,, die den Prozess des Hinzufügens von Echtzeit-Webfunktionen zu Anwendungen vereinfacht. Echtzeit-Webfunktionen ist die Möglichkeit, Server Code Inhalte mithilfe von Push sofort an verbundene Clients sobald sie verfügbar sind, statt den Server für einen Client zum Anfordern von neuer Daten warten.

SignalR kann verwendet werden, Ihre ASP.NET-Anwendung jegliche Art von "in Echtzeit"-Webfunktionen hinzu. Während der Chat häufig als Beispiel verwendet wird, können Sie viel mehr tun. Jedes Mal ein Benutzer aktualisiert eine Webseite, um neue Daten anzuzeigen, oder die Seite implementiert [lange](http://en.wikipedia.org/wiki/Push_technology#Long_polling) um neue Daten abzurufen, ist es ein Kandidat für die Verwendung von SignalR. Beispiele für Dashboards und Überwachen von Anwendungen, die Anwendungen (z. B. die gleichzeitige Bearbeitung von Dokumenten), für die Zusammenarbeit Auftrag Statusupdates und Formulare in Echtzeit.

SignalR ermöglicht auch völlig neue Arten von Webanwendungen, die sehr häufig Updates vom Server, erfordern z. B. Echtzeit spielen.

SignalR stellt eine einfache API zum Erstellen von Server-zu-Client von Remoteprozeduraufrufen (RPC), die JavaScript-Funktionen im Client Browsern (und andere Clientplattformen) vom serverseitigen .NET Code aufrufen. SignalR enthält außerdem eine API für die verbindungsverwaltung (z. B. eine Verbindung herstellen, und Ereignisse zum Trennen), und Gruppieren von Verbindungen.

![Aufrufen von Methoden mit SignalR](introduction-to-signalr/_static/image1.png)

SignalR verbindungsverwaltung automatisch behandelt, und Ihnen Broadcastmeldungen für alle verbundenen Clients gleichzeitig, z. B. einem Chatraum. Sie können auch Nachrichten für bestimmte Clients senden. Die Verbindung zwischen Client und Server ist persistent, im Gegensatz zu einer klassischen HTTP-Verbindung, die für die einzelnen mitteillungen wieder hergestellt ist.

SignalR unterstützt "Serverpush"-Funktionalität, die in der Servercode Clientcode in der Browser über Remote Procedure Calls (RPC), statt über allgemeine Anforderung / Antwort-Modell im Web heute aufrufen kann.

SignalR-Anwendungen können Tausende von Clients mithilfe von Service Bus, SQL Server horizontal oder [Redis](http://redis.io).

SignalR ist Open Source, über [GitHub](https://github.com/signalr).

## <a name="signalr-and-websocket"></a>SignalR und WebSocket

SignalR wird der neue WebSocket-Transport verwendet, sofern verfügbar, und Sie auf ältere Transporte zurückgegriffen, wenn nötig. Während Sie sicherlich schreiben, können Ihre Anwendung direkt mit dem WebSocket, mithilfe von SignalR-Mitteln, die ein Großteil der zusätzliche Funktionalität, die Sie implementieren müssten Sie durchgeführt haben wird. Am wichtigsten ist, bedeutet dies, dass Sie Ihre Anwendung WebSocket nutzen, ohne dass Gedanken machen, erstellen einen separaten Codepfad für ältere Clients schreiben können. SignalR schützt auch vom Vorhandensein Updates WebSocket, kümmern, da weiterhin aktualisiert werden, um die Änderungen in den zugrunde liegenden Transport unterstützen SignalR Ihrer Anwendung eine konsistente Schnittstelle zwischen Versionen der WebSocket bereit.

Während Sie eine Lösung mit WebSocket allein sicherlich erstellen können, bietet SignalR alle Funktionen, die Sie selbst, z. B. Fallback auf andere Transporte und Überarbeiten die Anwendung für Updates WebSocket-Implementierungen schreiben müssen.

<a id="transports"></a>

## <a name="transports-and-fallbacks"></a>Transporte und Zugriffe

SignalR ist eine Abstraktion über einige der verwendete Transporte, die in Echtzeit Arbeit zwischen Client und Server erforderlich sind. Eine SignalR-Verbindung HTTP startet und wird dann um eine WebSocket-Verbindung heraufgestuft, sofern dieser verfügbar ist. WebSocket ist der idealen Transport für SignalR, da es macht die effizienteste Nutzung des Serverspeichers, hat die niedrigste Latenz und verfügt über die meisten zugrunde liegenden Funktionen (z. B. Vollduplex-Kommunikation zwischen Client und Server), aber sie auch den strengsten hat Anforderungen: WebSocket der Server mit Windows Server 2012 oder Windows 8 und .NET Framework 4.5 werden muss. Wenn diese Anforderungen nicht erfüllt sind, versucht SignalR andere Transporte verwenden, um dessen Verbindungen zu treffen.

### <a name="html-5-transports"></a>HTML 5-Transporte

Diese Transporte richten sich nach Unterstützung für [HTML 5](http://en.wikipedia.org/wiki/HTML5). Wenn der Clientbrowser HTML 5-Standard nicht unterstützt, werden ältere Transporte verwendet werden.

- **WebSocket** (wenn die dem Server und dem Browser anzugeben, können sie die Websocket unterstützen). WebSocket ist die einzige Transport, der eine "true" persistente, bidirektionale Verbindung zwischen Client und Server hergestellt wird. Allerdings gelten WebSocket auch strengsten; Es wird nur in den neuesten Versionen von Microsoft Internet Explorer und Google Chrome, Mozilla Firefox vollständig unterstützt, und nur partielle Implementierung hat, in anderen Browsern, z. B. Opera und Safari.
- **Server gesendete Ereignisse**, auch bekannt als EventSource (wenn der Browser Server gesendete Ereignisse, die im Grunde wird von allen Browsern mit Ausnahme von Internet Explorer unterstützt.)

### <a name="comet-transports"></a>Comet-Transporte

Die folgenden Transportoptionen basieren auf der [Comet](http://en.wikipedia.org/wiki/Comet_(programming)) webanwendungsmodells, in der ein Browser oder einem anderen Client eine lang aufrechterhalten HTTP-Anforderung, die der Server verwenden kann beibehält, insbesondere Daten an den Client ohne den Client pushen soll Sie anfordern.

- **Forever Frame** (für nur in Internet Explorer). Unbegrenzte erstellt Frame ein versteckten IFRAMEs, wodurch eine Anforderung an einen Endpunkt auf dem Server, die nicht abgeschlossen wird. Der Server sendet dann fortlaufend Skript an den Client die sofort ausgeführt wird, stellt eine unidirektionale Echtzeit-Verbindung vom Server an Client. Die Verbindung vom Client zum Server verwendet eine separate Verbindung vom Server zu Clientverbindung, und wie eine standard-HTTP-Anforderung eine neue Verbindung erstellt wird, für jedes Datenelement, die gesendet werden muss.
- **Lange abrufen AJAX**. Langes Abrufintervall erstellt keine dauerhafte Verbindung jedoch stattdessen fragt den Server mit einer Anforderung, die geöffnet bleibt, bis der Server antwortet, an welchem Punkt die Verbindung wird geschlossen, und eine neue Verbindung sofort angefordert wird. Dies kann einige Latenz führen, während die Verbindung zurückgesetzt.

Weitere Informationen, welche Transporte unter welche Konfigurationen unterstützt werden, finden Sie unter [unterstützte Plattformen](supported-platforms.md).

### <a name="transport-selection-process"></a>Transport Auswahlprozesses verwendet werden

Die folgende Liste enthält die Schritte, die SignalR verwendet, um zu entscheiden, welcher transport.

1. Wenn der Browser InternetExplorer 8 oder niedriger ist, wird die langen Abfragen verwendet.
2. Wenn JSONP konfiguriert ist (d. h. die `jsonp` Parameter auf festgelegt ist `true` Wenn die Verbindung gestartet ist), lange Abruf verwendet wird.
3. Wenn eine domänenübergreifende-Verbindung hergestellt wird (d. h., wenn der SignalR-Endpunkt nicht in derselben Domäne wie der Hostseite ist), wird WebSocket verwendet werden, wenn die folgenden Kriterien erfüllt sind:

    - Der Client unterstützt die CORS (Cross-Origin Resource Sharing). Details zu den Clients CORS unterstützen, finden Sie unter [CORS am caniuse.com](http://www.caniuse.com/CORS).
    - Der Client unterstützt WebSocket
    - Der Server unterstützt WebSocket

    Wenn keines diese Kriterien nicht erfüllt sind, wird die langen Abfragen verwendet. Weitere Informationen für domänenübergreifende Verbindungen finden Sie unter [Gewusst wie: Herstellen einer Verbindung domänenübergreifende](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).
4. Wenn JSONP ist nicht konfiguriert, und die Verbindung keine domänenübergreifende ist, werden WebSocket verwendet werden, wenn Client und Server unterstützen.
5. Wenn der Client oder Server WebSocket nicht unterstützt wird, wird Server gesendete Ereignisse verwendet, sofern dieser verfügbar ist.
6. Wenn Server gesendete Ereignisse nicht verfügbar ist, wird eine unbegrenzte Zeitdauer aufbewahrt Frame versucht.
7. Wenn Forever Frame ein Fehler auftritt, wird die langen Abfragen verwendet.

<a id="MonitoringTransports"></a>
### <a name="monitoring-transports"></a>Überwachung von Transporten

Sie können bestimmen, welcher Transport Ihrer Anwendung verwendet wird, durch Aktivieren der Protokollierung für Ihren Hub, und öffnen das Konsolenfenster in Ihrem Browser.

Fügen Sie den folgenden Befehl zum Aktivieren der Protokollierung für Ihren Hub-Ereignisse in einem Browser an die Clientanwendung:

`$.connection.hub.logging = true;`

- Klicken Sie in Internet Explorer öffnen Sie der Entwicklertools durch Drücken von F12, und klicken Sie auf der Registerkarte "Console".

    ![-Konsole in Microsoft Internet Explorer](introduction-to-signalr/_static/image2.png)
- Öffnen Sie die Konsole in Chrome durch Drücken von STRG + UMSCHALT + J.

    ![-Konsole im Google Chrome](introduction-to-signalr/_static/image3.png)

Die Konsole öffnen und die Protokollierung aktiviert müssen Sie möglicherweise finden Sie unter welcher Transport von SignalR verwendet wird.

![In Internet Explorer mit der WebSocket-Transport-Konsole](introduction-to-signalr/_static/image4.png)

### <a name="specifying-a-transport"></a>Angeben eines Transports

Aushandeln der Transport hat eine bestimmte Menge an Zeit und Client/Server-Ressourcen. Wenn Sie die Clientfunktionen bekannt sind, kann ein Transport angegeben werden, wenn die Clientverbindung gestartet wird. Der folgende Codeausschnitt veranschaulicht, starten eine Verbindung über den Transport Ajax lange abrufen, das verwendet werden würde, wenn bekannt war, dass der Client ein anderes Protokoll nicht unterstützt:

`connection.start({ transport: 'longPolling' });`

Sie können eine fallback-Reihenfolge angeben, wenn einen Client bestimmte Transporte in der Reihenfolge versuchen soll. Der folgende Codeausschnitt veranschaulicht beim WebSocket und, die fehlschlagen, navigieren Sie direkt zu lange abrufen.

`connection.start({ transport: ['webSockets','longPolling'] });`

Zeichenfolgenkonstanten zum Angeben von Transporten sind folgendermaßen definiert:

- `webSockets`
- `foreverFrame`
- `serverSentEvents`
- `longPolling`

## <a name="connections-and-hubs"></a>Verbindungen und-Hubs

Die SignalR-API enthält zwei Modelle für die Kommunikation zwischen Clients und Server: dauerhafte Verbindungen und Hubs.

Eine Verbindung stellt einen einfachen Endpunkt für die einzelnen Empfänger, gruppierter oder broadcast Senden von Nachrichten dar. Die dauerhafte Verbindung-API (in .NET Code von der Klasse "persistentconnection" dargestellt) ermöglicht es der Entwickler direkten Zugriff auf die SignalR stellt Low-Level-Kommunikationsprotokoll. Mit den Verbindungen Kommunikationsmodell werden-Entwicklern vertraut, die Verbindung basierende APIs wie z. B. Windows Communication Foundation verwendet haben.

Ein Hub ist eine grobe Pipeline, die auf den Verbindungs-API, die der Client-als auch direkt aufrufen von Methoden voneinander ermöglicht basiert. SignalR übernimmt die die Verteilung über Computergrenzen hinweg wie durch Magic, sodass Clients zum Aufrufen von Methoden auf dem Server als einfach als lokalen Methoden (und umgekehrt). Mithilfe der Hubs Kommunikationsmodell werden-Entwicklern vertraut, die Remoteaufruf APIs wie .NET Remoting verwendet haben. Mit einem Hub können Sie auch stark typisierte Parameter an die Methoden, wurden die modellbindung ermöglichen übergeben.

### <a name="architecture-diagram"></a>Architekturdiagramm

Das folgende Diagramm zeigt die Beziehung zwischen Hubs, permanente Verbindungen und den zugrunde liegenden Technologien für Transporte verwendet.

![SignalR-Architekturdiagramm, die mit APIs, Transporte und clients](introduction-to-signalr/_static/image5.png)

### <a name="how-hubs-work"></a>Funktionsweise von Hubs

Bei der serverseitigen Code auf dem Client eine Methode aufruft, wird ein Paket gesendet, in der aktive Transport, der den Namen und den Parametern der aufzurufenden Methode enthält (wenn ein Objekt als Parameter der Methode gesendet wird, wird es serialisiert mit JSON). Der Client vergleicht dann den Methodennamen an Methoden, die im clientseitigen Code definiert. Wenn eine Übereinstimmung vorhanden ist, wird die Methode verwendet die deserialisierte Parameterdaten ausgeführt werden.

Der Methodenaufruf kann überwacht werden, mithilfe von Tools wie [Fiddler.](http://fiddler2.com/) Die folgende Abbildung zeigt einen Methodenaufruf von einem SignalR-Server an einem Webbrowserclient im Bereich "Protokolle" des Fiddler gesendet. Aufruf der Methode mit einem Hub aufgerufen gesendet wird `MoveShapeHub`, und die aufgerufene Methode wird aufgerufen, `updateShape`.

![Überblick über Fiddler-Protokoll, die mit SignalR-Datenverkehr](introduction-to-signalr/_static/image6.png)

In diesem Beispiel wird der Hub-Name identifiziert, mit der `H` Parameter; die Methode mit Namen identifiziert die `M` Parameter und die Daten gesendet werden, an die Methode wird mit identifiziert die `A` Parameter. Die Anwendung, die diese Nachricht generiert wird erstellt, der [hochfrequente Echtzeit](tutorial-high-frequency-realtime-with-signalr.md) Lernprogramm.

### <a name="choosing-a-communication-model"></a>Auswählen eines Modells für die Kommunikation

Die meisten Anwendungen sollten die Hubs-API verwenden. Die API Verbindungen konnte in den folgenden Situationen verwendet werden:

- Das Format der tatsächlichen Nachricht muss angegeben werden.
- Der Entwickler bevorzugt mit einem Messaging- und verteilen-Modell statt mit einem Remoteaufruf-Modell arbeiten.
- Eine vorhandene Anwendung, die eine e-Mail-Modell verwendet wird SignalR verwenden portiert werden.
