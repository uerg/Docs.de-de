---
title: "WebSockets-Unterstützung in ASP.NET Core"
author: tdykstra
description: Informationen Sie zum Einstieg in ASP.NET Core WebSockets.
keywords: ASP.NET Core, WebSockets
ms.author: tdykstra
manager: wpickett
ms.date: 03/25/2017
ms.topic: article
ms.assetid: 0e0fedcd-a7b4-4479-8ae0-36eab0229d7e
ms.technology: aspnet
ms.prod: aspnet-core
uid: fundamentals/websockets
ms.openlocfilehash: 114d52d831668e5facd1142b5f9e5f68e7456f7e
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2017
---
# <a name="introduction-to-websockets-in-aspnet-core"></a>Einführung in ASP.NET Core WebSockets

Durch [Tom Dykstra](https://github.com/tdykstra) und [Andrew Stanton-Versicherungsfachleuten](https://github.com/anurse)

In diesem Artikel erläutert, wie zum Einstieg in WebSockets in ASP.NET Core. [WebSocket](https://wikipedia.org/wiki/WebSocket) ist ein Protokoll, das bidirektionale persistenten Kommunikationskanäle über TCP-Verbindungen ermöglicht. Es wird für Anwendungen wie beispielsweise Chat, Börsenticker, Spiele, an einer beliebigen Stelle Funktionen in einer Webanwendung in Echtzeit angezeigt werden sollen.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([zum Herunterladen von](xref:tutorials/index#how-to-download-a-sample)). Finden Sie unter der [Arbeitsschritte](#next-steps) Abschnitt, um weitere Informationen.


## <a name="prerequisites"></a>Erforderliche Komponenten

* ASP.NET Core 1.1 (wird nicht auf 1.0 ausgeführt)
* Ein Betriebssystem, das auf ASP.NET Core ausgeführt wird:
  
  * Windows 7 / WindowsServer 2008 und höher
  * Linux
  * MacOS

* **Ausnahme**: Wenn Ihre app unter Windows mit IIS ausgeführt wird, oder Sie müssen mit WebListener, verwenden:

  * Windows 8 / WindowsServer 2012 oder höher
  * IIS 8 / Express IIS 8
  * WebSocket muss in IIS aktiviert sein

* Unterstützten Browsern finden Sie unter http://caniuse.com/#feat=websockets.

## <a name="when-to-use-it"></a>Empfohlene Verwendung

WebSockets zu verwenden, wenn Sie direkt mit einer Socketverbindung arbeiten müssen. Beispielsweise benötigen Sie möglicherweise die bestmögliche Leistung für ein Spiel in Echtzeit.

[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) bietet eine umfangreichere Anwendungsmodell für Echtzeit-Funktionalität, sondern nur auf ASP.NET, nicht ASP.NET Core ausgeführt wird. Eine Version des SignalR Core ist in der Entwicklung. um den Fortschritt folgen zu können, finden Sie unter der [GitHub-Repository für SignalR Core](https://github.com/aspnet/SignalR).

Wenn Sie nicht für SignalR Core warten möchten, können Sie WebSockets direkt jetzt verwenden. Aber möglicherweise müssen Sie Funktionen zu entwickeln, die SignalR, z. B. bieten würden:

* Unterstützung für eine breitere Palette von Browserversionen Automatisches Fallback auf alternativen Transportmethoden mit.
* Automatische erneute Verbindung, wenn eine Verbindung gelöscht.
* Unterstützung für Clients Aufrufen von Methoden, die auf dem Server oder umgekehrt.
* Unterstützung für die Skalierung auf mehreren Servern.

## <a name="how-to-use-it"></a>Art der Verwendung

* Installieren der [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) Paket.
* Konfigurieren Sie die Middleware.
* Akzeptieren Sie WebSocket-Anforderungen.
* Senden und Empfangen von Nachrichten.

### <a name="configure-the-middleware"></a>Konfigurieren Sie die middleware

Fügen Sie die WebSockets-Middleware in der `Configure` Methode der `Startup` Klasse.

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

Die folgenden Einstellungen können konfiguriert werden:

* `KeepAliveInterval`– Wie häufig gesendet "Ping" Frames an den Client, um sicherzustellen, dass Proxys die Verbindung geöffnet lassen.
* `ReceiveBufferSize`– Die Größe des Puffers zum Empfangen von Daten verwendet werden soll. Nur erfahrene Benutzer müssen für die leistungsoptimierung ändern, basierend auf der Größe ihrer Daten.

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a>Akzeptieren der WebSocket-Anforderungen

Einem späteren Zeitpunkt im Lebenszyklus Anforderung (weiter unten in der `Configure` Methode oder in einer MVC-Aktion, z. B.) überprüfen Sie, ob es sich um eine websocketanforderung ist, und akzeptieren die WebSocket-Anforderung.

Dieses Beispiel stammt aus einem späteren Zeitpunkt in der `Configure` Methode.

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

Eine websocketanforderung konnte stammen, auf eine beliebige URL, aber in diesem Beispielcode akzeptiert nur Anforderungen für `/ws`.

### <a name="send-and-receive-messages"></a>Senden und Empfangen von Nachrichten

Die `AcceptWebSocketAsync` Methode aktualisiert die TCP-Verbindung, um eine WebSocket-Verbindung, und bietet Ihnen eine [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) Objekt. Verwenden Sie das WebSocket-Objekt zum Senden und Empfangen von Nachrichten.

Übergibt, akzeptiert die WebSocket-Anforderung den zuvor aufgeführten Code die `WebSocket` -Objekt an eine `Echo` Methode; hier ist die `Echo` Methode. Der Code empfängt eine Nachricht und die gleiche Nachricht sofort sendet. Es bleibt in einer Schleife, die auf diese Weise bis der Client die Verbindung geschlossen wird. 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

Wenn Sie die WebSocket vor Beginn dieser Schleife annehmen, endet die middlewarepipeline.  Beim Schließen des Sockets an, die entlädt der Pipelines aus. D. h. würde der Anforderung beendet wird, die in der Pipeline Hostdaten, wenn Sie annehmen, dass ein WebSocket, genau wie bei eine MVC-Aktion, z. B. erreichen.  Aber wenn Sie diese Schleife beendet, und den Socket schließt, wird die Anforderung sichern Sie die Pipeline fortgesetzt.

## <a name="next-steps"></a>Nächste Schritte

Die [beispielanwendung](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) , die begleitet dieser Artikel ist eine einfache Echo-Anwendung. Es wurde eine Webseite, die WebSocket-Verbindungen herstellt, und der Server sendet nur an den Client alle empfangenen Nachrichten. Führen Sie es über eine Eingabeaufforderung (es hat nicht Einrichten von Visual Studio mit IIS Express ausgeführt), und navigieren Sie zu http://localhost: 5000. Die Webseite zeigt den Verbindungsstatus auf der linken oberen Ecke:

![Anfangszustand der Webseite](websockets/_static/start.png)

Wählen Sie **verbinden** um eine websocketanforderung an die gezeigte URL senden.  Geben Sie eine Testnachricht, und wählen Sie **senden**. Aus und klicken Sie **schließen Socket**. Die **Kommunikation Protokoll** Abschnitt meldet jede öffnen, senden und schließen sie die Aktion durchgeführt.

![Anfangszustand der Webseite](websockets/_static/end.png)
