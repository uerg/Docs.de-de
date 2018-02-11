---
title: "WebSockets-Unterstützung in ASP.NET Core"
author: tdykstra
description: Erfahren Sie, wie Sie mit WebSockets in ASP.NET beginnen.
manager: wpickett
ms.author: tdykstra
ms.date: 03/25/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/websockets
ms.openlocfilehash: 306eca28b9f1f66e1ccaf185ccae87db8dea1b01
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-websockets-in-aspnet-core"></a>Einführung in WebSockets in ASP.NET Core

Von [Tom Dykstra](https://github.com/tdykstra) und [Andrew Stanton-Nurse](https://github.com/anurse)

In diesem Artikel erfahren Sie, wie Sie mit WebSockets in ASP.NET beginnen. Bei [WebSockets](https://wikipedia.org/wiki/WebSocket) handelt es sich um ein Protokoll, das bidirektionale persistente Kommunikationskanäle über TCP-Verbindungen ermöglicht. Es wird in Chat-, Börsenticker- und Spiele-Apps verwendet – überall dort, wo Echtzeitfunktionen in einer Web-App benötigt werden.

[Zeigen Sie Beispielcode an, oder laden Sie diesen herunter](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample)). Weitere Informationen finden Sie im Abschnitt [Nächste Schritte](#next-steps).


## <a name="prerequisites"></a>Erforderliche Komponenten

* ASP.NET Core 1.1 (kann nicht in 1.0 ausgeführt werden)
* Jedes Betriebssystem, unter dem ASP.NET Core ausgeführt wird:
  
  * Windows 7/Windows Server 2008 und höher
  * Linux
  * macOS

* **Ausnahme:** Wenn Ihre App unter Windows mit IIS oder mit WebListener ausgeführt wird, müssen Sie Folgendes verwenden:

  * Windows 8/Windows Server 2012 und höher
  * IIS 8/IIS 8 Express
  * WebSocket muss in IIS aktiviert werden.

* Informationen zu unterstützten Browsern finden Sie unter http://caniuse.com/#feat=websockets.

## <a name="when-to-use-it"></a>Empfohlene Verwendung

Verwenden Sie WebSockets, wenn Sie direkt mit einer Socketverbindung arbeiten müssen. Für ein Echtzeitspiel benötigen Sie z.B. die höchste Leistung.

[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) stellt ein erweitertes Anwendungsmodell für Echtzeitfunktionen bereit, aber es kann nur in ASP.NET und nicht in ASP.NET Core ausgeführt werden. Im Moment wird an der Entwicklung einer Core-Version von SignalR gearbeitet. Im [GitHub-Repository für SignalR Core](https://github.com/aspnet/SignalR) können Sie sich den Fortschritt ansehen.

Wenn Sie nicht auf SignalR warten möchten, können Sie jetzt direkt WebSockets verwenden. Möglicherweise müssen Sie jedoch Features entwickeln, die ansonsten von SignalR bereitgestellt würden. Dazu gehören z.B.:

* Unterstützung für eine größere Zahl an unterschiedlichen Browserversionen, indem Sie ein automatisches Fallback auf alternative Transportmethoden einsetzen
* Der automatische Aufbau einer neuen Verbindung, wenn die Verbindung unterbrochen wurde
* Unterstützung für das Aufrufen von Methoden auf dem Server durch Clients bzw. andersherum
* Unterstützung für die Skalierung auf mehrere Server

## <a name="how-to-use-it"></a>Verwendungsweise

* Installieren Sie das Paket [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/).
* Konfigurieren Sie die Middleware.
* Akzeptieren Sie Anforderungen von WebSocket.
* Senden Sie Nachrichten, und empfangen Sie diese.

### <a name="configure-the-middleware"></a>Konfigurieren der Middleware

Fügen Sie die WebSockets-Middleware in der `Configure`-Methode der `Startup`-Klasse hinzu.

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

Sie können die folgenden Einstellungen konfigurieren:

* `KeepAliveInterval`: Legt fest, wie oft „Ping“-Frames an den Client gesendet werden, um sicherzustellen, dass Proxys die Verbindung aufrechterhalten
* `ReceiveBufferSize`: Legt die Größe des Puffers fest, der zum Empfang von Daten verwendet wird Nur fortgeschrittene Benutzer müssen diese Angaben ändern, um die Leistung auf Grundlage der Größe ihrer Daten anzupassen.

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a>Akzeptieren der Anforderungen von WebSocket

Prüfen Sie zu einem späteren Zeitpunkt im Lebenszyklus einer Anforderung (z.B. später in der `Configure`-Methode oder in einer MVC-Aktion), ob es sich um eine WebSocket-Anforderung handelt, und akzeptieren Sie diese.

Dieses Beispiel ist ein späterer Auszug der `Configure`-Methode.

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

WebSocket-Anforderungen können bei jeder URL eingehen. Dieser Beispielcode akzeptiert jedoch nur Anforderungen für `/ws`.

### <a name="send-and-receive-messages"></a>Senden und Empfangen von Nachrichten

Die `AcceptWebSocketAsync`-Methode ändert die TCP-Verbindung in eine WebSocket-Verbindung, und gibt Ihnen ein [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket)-Objekt. Verwenden Sie das WebSocket-Objekt, um Nachrichten zu senden und zu empfangen.

Der weiter oben gezeigte Code, der WebSocket-Anforderungen akzeptiert, übergibt das `WebSocket`-Objekt an eine `Echo`-Methode. Hier sehen Sie die `Echo`-Methode. Der Code empfängt eine Nachricht und sendet diese umgehend wieder zurück. Diese Schleife bleibt bestehen, bis die Verbindung des Clients unterbrochen wird. 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

Wenn Sie das Websocket vor Beginn dieser Schleife akzeptieren, endet die Middlewarepipeline.  Wenn Sie das Socket schließen, wird die Pipeline entladen. Das bedeutet, dass die Anforderung die Pipeline nicht weiter durchläuft, wenn Sie ein Websocket akzeptieren, genauso als wenn Sie auf eine MVC-Aktion treffen.  Wenn Sie diese Schleife beenden und das Socket schließen, durchläuft die Anforderung die Pipeline in umgekehrter Reihenfolge.

## <a name="next-steps"></a>Nächste Schritte

Die [Beispielanwendung](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample), die diesen Artikel begleitet, ist eine einfache Echoanwendung. Sie verfügt über eine Webseite, die WebSocket-Verbindungen herstellt. Der Server schickt alle empfangenen Nachrichten zurück an den Client. Führen Sie sie über eine Eingabeaufforderung aus (es ist nicht darauf ausgelegt, von Visual Studio mit IIS Express ausgeführt zu werden), und navigieren Sie zu https://localhost:5000. Die Webseite zeigt den Verbindungsstatus in der oberen linken Ecke an:

![Erster Zustand der Webseite](websockets/_static/start.png)

Klicken Sie auf **Connect** (Verbinden), um eine WebSocket-Anforderung an die gezeigte URL zu senden.  Geben Sie einen Testtext ein, und klicken Sie auf **Send** (Senden). Wenn dies abgeschlossen ist, klicken Sie auf **Close Socket** (Socket schließen). Der Abschnitt **Kommunikationsprotokoll** meldet jede open-, send- und close-Aktion, wenn diese durchgeführt wird.

![Erster Zustand der Webseite](websockets/_static/end.png)
