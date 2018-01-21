---
title: "WebSockets-Unterstützung in ASP.NET Core"
author: tdykstra
description: Informationen Sie zum Einstieg in ASP.NET Core WebSockets.
ms.author: tdykstra
manager: wpickett
ms.date: 03/25/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: fundamentals/websockets
ms.openlocfilehash: 46c1f42b925a43df470d7491a1e259ab51ea5f50
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-websockets-in-aspnet-core"></a><span data-ttu-id="bf717-103">Einführung in ASP.NET Core WebSockets</span><span class="sxs-lookup"><span data-stu-id="bf717-103">Introduction to WebSockets in ASP.NET Core</span></span>

<span data-ttu-id="bf717-104">Durch [Tom Dykstra](https://github.com/tdykstra) und [Andrew Stanton-Versicherungsfachleuten](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="bf717-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="bf717-105">In diesem Artikel erläutert, wie zum Einstieg in WebSockets in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bf717-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="bf717-106">Bei [WebSockets](https://wikipedia.org/wiki/WebSocket) handelt es sich um ein Protokoll, das bidirektionale persistente Kommunikationskanäle über TCP-Verbindungen ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="bf717-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="bf717-107">Es wird für Anwendungen wie beispielsweise Chat, Börsenticker, Spiele, an einer beliebigen Stelle Funktionen in einer Webanwendung in Echtzeit angezeigt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="bf717-107">It is used for applications such as chat, stock tickers, games, anywhere you want real-time functionality in a web application.</span></span>

<span data-ttu-id="bf717-108">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([zum Herunterladen von](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="bf717-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="bf717-109">Finden Sie unter der [Arbeitsschritte](#next-steps) Abschnitt, um weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="bf717-109">See the [Next Steps](#next-steps) section for more information.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="bf717-110">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="bf717-110">Prerequisites</span></span>

* <span data-ttu-id="bf717-111">ASP.NET Core 1.1 (wird nicht auf 1.0 ausgeführt)</span><span class="sxs-lookup"><span data-stu-id="bf717-111">ASP.NET Core 1.1 (does not run on 1.0)</span></span>
* <span data-ttu-id="bf717-112">Ein Betriebssystem, das auf ASP.NET Core ausgeführt wird:</span><span class="sxs-lookup"><span data-stu-id="bf717-112">Any OS that ASP.NET Core runs on:</span></span>
  
  * <span data-ttu-id="bf717-113">Windows 7 / WindowsServer 2008 und höher</span><span class="sxs-lookup"><span data-stu-id="bf717-113">Windows 7 / Windows Server 2008 and later</span></span>
  * <span data-ttu-id="bf717-114">Linux</span><span class="sxs-lookup"><span data-stu-id="bf717-114">Linux</span></span>
  * <span data-ttu-id="bf717-115">macOS</span><span class="sxs-lookup"><span data-stu-id="bf717-115">macOS</span></span>

* <span data-ttu-id="bf717-116">**Ausnahme**: Wenn Ihre app unter Windows mit IIS ausgeführt wird, oder Sie müssen mit WebListener, verwenden:</span><span class="sxs-lookup"><span data-stu-id="bf717-116">**Exception**: If your app runs on Windows with IIS, or with WebListener, you must use:</span></span>

  * <span data-ttu-id="bf717-117">Windows 8 / WindowsServer 2012 oder höher</span><span class="sxs-lookup"><span data-stu-id="bf717-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="bf717-118">IIS 8 / Express IIS 8</span><span class="sxs-lookup"><span data-stu-id="bf717-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="bf717-119">WebSocket muss in IIS aktiviert sein</span><span class="sxs-lookup"><span data-stu-id="bf717-119">WebSocket must be enabled in IIS</span></span>

* <span data-ttu-id="bf717-120">Unterstützten Browsern finden Sie unter http://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="bf717-120">For supported browsers, see http://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-it"></a><span data-ttu-id="bf717-121">Empfohlene Verwendung</span><span class="sxs-lookup"><span data-stu-id="bf717-121">When to use it</span></span>

<span data-ttu-id="bf717-122">WebSockets zu verwenden, wenn Sie direkt mit einer Socketverbindung arbeiten müssen.</span><span class="sxs-lookup"><span data-stu-id="bf717-122">Use WebSockets when you need to work directly with a socket connection.</span></span> <span data-ttu-id="bf717-123">Beispielsweise benötigen Sie möglicherweise die bestmögliche Leistung für ein Spiel in Echtzeit.</span><span class="sxs-lookup"><span data-stu-id="bf717-123">For example, you might need the best possible performance for a real-time game.</span></span>

<span data-ttu-id="bf717-124">[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) bietet eine umfangreichere Anwendungsmodell für Echtzeit-Funktionalität, sondern nur auf ASP.NET, nicht ASP.NET Core ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="bf717-124">[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) provides a richer application model for real-time functionality, but it runs only on ASP.NET, not ASP.NET Core.</span></span> <span data-ttu-id="bf717-125">Eine Version des SignalR Core ist in der Entwicklung. um den Fortschritt folgen zu können, finden Sie unter der [GitHub-Repository für SignalR Core](https://github.com/aspnet/SignalR).</span><span class="sxs-lookup"><span data-stu-id="bf717-125">A Core version of SignalR is under development; to follow its progress, see the [GitHub repository for SignalR Core](https://github.com/aspnet/SignalR).</span></span>

<span data-ttu-id="bf717-126">Wenn Sie nicht für SignalR Core warten möchten, können Sie WebSockets direkt jetzt verwenden.</span><span class="sxs-lookup"><span data-stu-id="bf717-126">If you don't want to wait for SignalR Core, you can use WebSockets directly now.</span></span> <span data-ttu-id="bf717-127">Aber möglicherweise müssen Sie Funktionen zu entwickeln, die SignalR, z. B. bieten würden:</span><span class="sxs-lookup"><span data-stu-id="bf717-127">But you might have to develop features that SignalR would provide, such as:</span></span>

* <span data-ttu-id="bf717-128">Unterstützung für eine breitere Palette von Browserversionen Automatisches Fallback auf alternativen Transportmethoden mit.</span><span class="sxs-lookup"><span data-stu-id="bf717-128">Support for a broader range of browser versions by using automatic fallback to alternative transport methods.</span></span>
* <span data-ttu-id="bf717-129">Automatische erneute Verbindung, wenn eine Verbindung gelöscht.</span><span class="sxs-lookup"><span data-stu-id="bf717-129">Automatic reconnection when a connection drops.</span></span>
* <span data-ttu-id="bf717-130">Unterstützung für Clients Aufrufen von Methoden, die auf dem Server oder umgekehrt.</span><span class="sxs-lookup"><span data-stu-id="bf717-130">Support for clients calling methods on the server or vice versa.</span></span>
* <span data-ttu-id="bf717-131">Unterstützung für die Skalierung auf mehreren Servern.</span><span class="sxs-lookup"><span data-stu-id="bf717-131">Support for scaling to multiple servers.</span></span>

## <a name="how-to-use-it"></a><span data-ttu-id="bf717-132">Verwendungsweise</span><span class="sxs-lookup"><span data-stu-id="bf717-132">How to use it</span></span>

* <span data-ttu-id="bf717-133">Installieren der [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) Paket.</span><span class="sxs-lookup"><span data-stu-id="bf717-133">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="bf717-134">Konfigurieren Sie die Middleware.</span><span class="sxs-lookup"><span data-stu-id="bf717-134">Configure the middleware.</span></span>
* <span data-ttu-id="bf717-135">Akzeptieren Sie WebSocket-Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="bf717-135">Accept WebSocket requests.</span></span>
* <span data-ttu-id="bf717-136">Senden und Empfangen von Nachrichten.</span><span class="sxs-lookup"><span data-stu-id="bf717-136">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="bf717-137">Konfigurieren Sie die middleware</span><span class="sxs-lookup"><span data-stu-id="bf717-137">Configure the middleware</span></span>

<span data-ttu-id="bf717-138">Fügen Sie die WebSockets-Middleware in der `Configure` Methode der `Startup` Klasse.</span><span class="sxs-lookup"><span data-stu-id="bf717-138">Add the WebSockets middleware in the `Configure` method of the `Startup` class.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

<span data-ttu-id="bf717-139">Die folgenden Einstellungen können konfiguriert werden:</span><span class="sxs-lookup"><span data-stu-id="bf717-139">The following settings can be configured:</span></span>

* <span data-ttu-id="bf717-140">`KeepAliveInterval`– Wie häufig gesendet "Ping" Frames an den Client, um sicherzustellen, dass Proxys die Verbindung geöffnet lassen.</span><span class="sxs-lookup"><span data-stu-id="bf717-140">`KeepAliveInterval` - How frequently to send "ping" frames to the client, to ensure proxies keep the connection open.</span></span>
* <span data-ttu-id="bf717-141">`ReceiveBufferSize`– Die Größe des Puffers zum Empfangen von Daten verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="bf717-141">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="bf717-142">Nur erfahrene Benutzer müssen für die leistungsoptimierung ändern, basierend auf der Größe ihrer Daten.</span><span class="sxs-lookup"><span data-stu-id="bf717-142">Only advanced users would need to change this, for performance tuning based on the size of their data.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a><span data-ttu-id="bf717-143">Akzeptieren der WebSocket-Anforderungen</span><span class="sxs-lookup"><span data-stu-id="bf717-143">Accept WebSocket requests</span></span>

<span data-ttu-id="bf717-144">Einem späteren Zeitpunkt im Lebenszyklus Anforderung (weiter unten in der `Configure` Methode oder in einer MVC-Aktion, z. B.) überprüfen Sie, ob es sich um eine websocketanforderung ist, und akzeptieren die WebSocket-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="bf717-144">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="bf717-145">Dieses Beispiel stammt aus einem späteren Zeitpunkt in der `Configure` Methode.</span><span class="sxs-lookup"><span data-stu-id="bf717-145">This example is from later in the `Configure` method.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

<span data-ttu-id="bf717-146">Eine websocketanforderung konnte stammen, auf eine beliebige URL, aber in diesem Beispielcode akzeptiert nur Anforderungen für `/ws`.</span><span class="sxs-lookup"><span data-stu-id="bf717-146">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="bf717-147">Senden und Empfangen von Nachrichten</span><span class="sxs-lookup"><span data-stu-id="bf717-147">Send and receive messages</span></span>

<span data-ttu-id="bf717-148">Die `AcceptWebSocketAsync` Methode aktualisiert die TCP-Verbindung, um eine WebSocket-Verbindung, und bietet Ihnen eine [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) Objekt.</span><span class="sxs-lookup"><span data-stu-id="bf717-148">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and gives you a [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="bf717-149">Verwenden Sie das WebSocket-Objekt zum Senden und Empfangen von Nachrichten.</span><span class="sxs-lookup"><span data-stu-id="bf717-149">Use the WebSocket object to send and receive messages.</span></span>

<span data-ttu-id="bf717-150">Übergibt, akzeptiert die WebSocket-Anforderung den zuvor aufgeführten Code die `WebSocket` -Objekt an eine `Echo` Methode; hier ist die `Echo` Methode.</span><span class="sxs-lookup"><span data-stu-id="bf717-150">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method; here's the `Echo` method.</span></span> <span data-ttu-id="bf717-151">Der Code empfängt eine Nachricht und die gleiche Nachricht sofort sendet.</span><span class="sxs-lookup"><span data-stu-id="bf717-151">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="bf717-152">Es bleibt in einer Schleife, die auf diese Weise bis der Client die Verbindung geschlossen wird.</span><span class="sxs-lookup"><span data-stu-id="bf717-152">It stays in a loop doing that until the client closes the connection.</span></span> 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

<span data-ttu-id="bf717-153">Wenn Sie die WebSocket vor Beginn dieser Schleife annehmen, endet die middlewarepipeline.</span><span class="sxs-lookup"><span data-stu-id="bf717-153">When you accept the WebSocket before beginning this loop, the middleware pipeline ends.</span></span>  <span data-ttu-id="bf717-154">Beim Schließen des Sockets an, die entlädt der Pipelines aus.</span><span class="sxs-lookup"><span data-stu-id="bf717-154">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="bf717-155">D. h. würde der Anforderung beendet wird, die in der Pipeline Hostdaten, wenn Sie annehmen, dass ein WebSocket, genau wie bei eine MVC-Aktion, z. B. erreichen.</span><span class="sxs-lookup"><span data-stu-id="bf717-155">That is, the request stops moving forward in the pipeline when you accept a WebSocket, just as it would when you hit an MVC action, for example.</span></span>  <span data-ttu-id="bf717-156">Aber wenn Sie diese Schleife beendet, und den Socket schließt, wird die Anforderung sichern Sie die Pipeline fortgesetzt.</span><span class="sxs-lookup"><span data-stu-id="bf717-156">But when you finish this loop and close the socket, the request proceeds back up the pipeline.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bf717-157">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="bf717-157">Next steps</span></span>

<span data-ttu-id="bf717-158">Die [beispielanwendung](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) , die begleitet dieser Artikel ist eine einfache Echo-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="bf717-158">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) that accompanies this article is a simple echo application.</span></span> <span data-ttu-id="bf717-159">Es wurde eine Webseite, die WebSocket-Verbindungen herstellt, und der Server sendet nur an den Client alle empfangenen Nachrichten.</span><span class="sxs-lookup"><span data-stu-id="bf717-159">It has a web page that makes WebSocket connections, and the server just resends back to the client any messages it receives.</span></span> <span data-ttu-id="bf717-160">Führen Sie es über eine Eingabeaufforderung (es hat nicht Einrichten von Visual Studio mit IIS Express ausgeführt), und navigieren Sie zu http://localhost: 5000.</span><span class="sxs-lookup"><span data-stu-id="bf717-160">Run it from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="bf717-161">Die Webseite zeigt den Verbindungsstatus auf der linken oberen Ecke:</span><span class="sxs-lookup"><span data-stu-id="bf717-161">The web page shows connection status at the upper left:</span></span>

![Anfangszustand der Webseite](websockets/_static/start.png)

<span data-ttu-id="bf717-163">Wählen Sie **verbinden** um eine websocketanforderung an die gezeigte URL senden.</span><span class="sxs-lookup"><span data-stu-id="bf717-163">Select **Connect** to send a WebSocket request to the URL shown.</span></span>  <span data-ttu-id="bf717-164">Geben Sie eine Testnachricht, und wählen Sie **senden**.</span><span class="sxs-lookup"><span data-stu-id="bf717-164">Enter a test message and select **Send**.</span></span> <span data-ttu-id="bf717-165">Aus und klicken Sie **schließen Socket**.</span><span class="sxs-lookup"><span data-stu-id="bf717-165">When done, select **Close Socket**.</span></span> <span data-ttu-id="bf717-166">Die **Kommunikation Protokoll** Abschnitt meldet jede öffnen, senden und schließen sie die Aktion durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="bf717-166">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Anfangszustand der Webseite](websockets/_static/end.png)
