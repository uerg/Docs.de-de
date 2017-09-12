---
title: "WebSockets-Unterstützung in ASP.NET Core"
author: tdykstra
description: "Was WebSockets ist in ASP.NET Core und zu dessen Verwendung zu unterstützen."
keywords: ASP.NET Core, WebSockets
ms.author: tdykstra
manager: wpickett
ms.date: 03/25/2017
ms.topic: article
ms.assetid: 0e0fedcd-a7b4-4479-8ae0-36eab0229d7e
ms.technology: aspnet
ms.prod: aspnet-core
uid: fundamentals/websockets
ms.openlocfilehash: a5914ad0d1056db993fcbfa1f00dd3422fcc4b6e
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2017
---
# <a name="introduction-to-websockets-in-aspnet-core"></a><span data-ttu-id="58bfe-104">Einführung in ASP.NET Core WebSockets</span><span class="sxs-lookup"><span data-stu-id="58bfe-104">Introduction to WebSockets in ASP.NET Core</span></span>

<span data-ttu-id="58bfe-105">Durch [Tom Dykstra](https://github.com/tdykstra) und [Andrew Stanton-Versicherungsfachleuten](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="58bfe-105">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="58bfe-106">In diesem Artikel erläutert, wie zum Einstieg in WebSockets in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="58bfe-106">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="58bfe-107">[WebSocket](https://wikipedia.org/wiki/WebSocket) ist ein Protokoll, das bidirektionale persistenten Kommunikationskanäle über TCP-Verbindungen ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="58bfe-107">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="58bfe-108">Es wird für Anwendungen wie beispielsweise Chat, Börsenticker, Spiele, an einer beliebigen Stelle Funktionen in einer Webanwendung in Echtzeit angezeigt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="58bfe-108">It is used for applications such as chat, stock tickers, games, anywhere you want real-time functionality in a web application.</span></span>

<span data-ttu-id="58bfe-109">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample).</span><span class="sxs-lookup"><span data-stu-id="58bfe-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample).</span></span> <span data-ttu-id="58bfe-110">Finden Sie unter der [Arbeitsschritte](#next-steps) Abschnitt, um weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="58bfe-110">See the [Next Steps](#next-steps) section for more information.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="58bfe-111">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="58bfe-111">Prerequisites</span></span>

* <span data-ttu-id="58bfe-112">ASP.NET Core 1.1 (wird nicht auf 1.0 ausgeführt)</span><span class="sxs-lookup"><span data-stu-id="58bfe-112">ASP.NET Core 1.1 (does not run on 1.0)</span></span>
* <span data-ttu-id="58bfe-113">Ein Betriebssystem, das auf ASP.NET Core ausgeführt wird:</span><span class="sxs-lookup"><span data-stu-id="58bfe-113">Any OS that ASP.NET Core runs on:</span></span>
  
  * <span data-ttu-id="58bfe-114">Windows 7 / WindowsServer 2008 und höher</span><span class="sxs-lookup"><span data-stu-id="58bfe-114">Windows 7 / Windows Server 2008 and later</span></span>
  * <span data-ttu-id="58bfe-115">Linux</span><span class="sxs-lookup"><span data-stu-id="58bfe-115">Linux</span></span>
  * <span data-ttu-id="58bfe-116">MacOS</span><span class="sxs-lookup"><span data-stu-id="58bfe-116">macOS</span></span>

* <span data-ttu-id="58bfe-117">**Ausnahme**: Wenn Ihre app unter Windows mit IIS ausgeführt wird, oder Sie müssen mit WebListener, verwenden:</span><span class="sxs-lookup"><span data-stu-id="58bfe-117">**Exception**: If your app runs on Windows with IIS, or with WebListener, you must use:</span></span>

  * <span data-ttu-id="58bfe-118">Windows 8 / WindowsServer 2012 oder höher</span><span class="sxs-lookup"><span data-stu-id="58bfe-118">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="58bfe-119">IIS 8 / Express IIS 8</span><span class="sxs-lookup"><span data-stu-id="58bfe-119">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="58bfe-120">WebSocket muss in IIS aktiviert sein</span><span class="sxs-lookup"><span data-stu-id="58bfe-120">WebSocket must be enabled in IIS</span></span>

* <span data-ttu-id="58bfe-121">Unterstützten Browsern finden Sie unter http://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="58bfe-121">For supported browsers, see http://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-it"></a><span data-ttu-id="58bfe-122">Empfohlene Verwendung</span><span class="sxs-lookup"><span data-stu-id="58bfe-122">When to use it</span></span>

<span data-ttu-id="58bfe-123">WebSockets zu verwenden, wenn Sie direkt mit einer Socketverbindung arbeiten müssen.</span><span class="sxs-lookup"><span data-stu-id="58bfe-123">Use WebSockets when you need to work directly with a socket connection.</span></span> <span data-ttu-id="58bfe-124">Beispielsweise benötigen Sie möglicherweise die bestmögliche Leistung für ein Spiel in Echtzeit.</span><span class="sxs-lookup"><span data-stu-id="58bfe-124">For example, you might need the best possible performance for a real-time game.</span></span>

<span data-ttu-id="58bfe-125">[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) bietet eine umfangreichere Anwendungsmodell für Echtzeit-Funktionalität, sondern nur auf ASP.NET, nicht ASP.NET Core ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="58bfe-125">[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) provides a richer application model for real-time functionality, but it runs only on ASP.NET, not ASP.NET Core.</span></span> <span data-ttu-id="58bfe-126">Eine Version des SignalR Core ist in der Entwicklung. um den Fortschritt folgen zu können, finden Sie unter der [GitHub-Repository für SignalR Core](https://github.com/aspnet/SignalR).</span><span class="sxs-lookup"><span data-stu-id="58bfe-126">A Core version of SignalR is under development; to follow its progress, see the [GitHub repository for SignalR Core](https://github.com/aspnet/SignalR).</span></span>

<span data-ttu-id="58bfe-127">Wenn Sie nicht für SignalR Core warten möchten, können Sie WebSockets direkt jetzt verwenden.</span><span class="sxs-lookup"><span data-stu-id="58bfe-127">If you don't want to wait for SignalR Core, you can use WebSockets directly now.</span></span> <span data-ttu-id="58bfe-128">Aber möglicherweise müssen Sie Funktionen zu entwickeln, die SignalR, z. B. bieten würden:</span><span class="sxs-lookup"><span data-stu-id="58bfe-128">But you might have to develop features that SignalR would provide, such as:</span></span>

* <span data-ttu-id="58bfe-129">Unterstützung für eine breitere Palette von Browserversionen Automatisches Fallback auf alternativen Transportmethoden mit.</span><span class="sxs-lookup"><span data-stu-id="58bfe-129">Support for a broader range of browser versions by using automatic fallback to alternative transport methods.</span></span>
* <span data-ttu-id="58bfe-130">Automatische erneute Verbindung, wenn eine Verbindung gelöscht.</span><span class="sxs-lookup"><span data-stu-id="58bfe-130">Automatic reconnection when a connection drops.</span></span>
* <span data-ttu-id="58bfe-131">Unterstützung für Clients Aufrufen von Methoden, die auf dem Server oder umgekehrt.</span><span class="sxs-lookup"><span data-stu-id="58bfe-131">Support for clients calling methods on the server or vice versa.</span></span>
* <span data-ttu-id="58bfe-132">Unterstützung für die Skalierung auf mehreren Servern.</span><span class="sxs-lookup"><span data-stu-id="58bfe-132">Support for scaling to multiple servers.</span></span>

## <a name="how-to-use-it"></a><span data-ttu-id="58bfe-133">Art der Verwendung</span><span class="sxs-lookup"><span data-stu-id="58bfe-133">How to use it</span></span>

* <span data-ttu-id="58bfe-134">Installieren der [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) Paket.</span><span class="sxs-lookup"><span data-stu-id="58bfe-134">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="58bfe-135">Konfigurieren Sie die Middleware.</span><span class="sxs-lookup"><span data-stu-id="58bfe-135">Configure the middleware.</span></span>
* <span data-ttu-id="58bfe-136">Akzeptieren Sie WebSocket-Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="58bfe-136">Accept WebSocket requests.</span></span>
* <span data-ttu-id="58bfe-137">Senden und Empfangen von Nachrichten.</span><span class="sxs-lookup"><span data-stu-id="58bfe-137">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="58bfe-138">Konfigurieren Sie die middleware</span><span class="sxs-lookup"><span data-stu-id="58bfe-138">Configure the middleware</span></span>

<span data-ttu-id="58bfe-139">Fügen Sie die WebSockets-Middleware in der `Configure` Methode der `Startup` Klasse.</span><span class="sxs-lookup"><span data-stu-id="58bfe-139">Add the WebSockets middleware in the `Configure` method of the `Startup` class.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

<span data-ttu-id="58bfe-140">Die folgenden Einstellungen können konfiguriert werden:</span><span class="sxs-lookup"><span data-stu-id="58bfe-140">The following settings can be configured:</span></span>

* <span data-ttu-id="58bfe-141">`KeepAliveInterval`– Wie häufig gesendet "Ping" Frames an den Client, um sicherzustellen, dass Proxys die Verbindung geöffnet lassen.</span><span class="sxs-lookup"><span data-stu-id="58bfe-141">`KeepAliveInterval` - How frequently to send "ping" frames to the client, to ensure proxies keep the connection open.</span></span>
* <span data-ttu-id="58bfe-142">`ReceiveBufferSize`– Die Größe des Puffers zum Empfangen von Daten verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="58bfe-142">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="58bfe-143">Nur erfahrene Benutzer müssen für die leistungsoptimierung ändern, basierend auf der Größe ihrer Daten.</span><span class="sxs-lookup"><span data-stu-id="58bfe-143">Only advanced users would need to change this, for performance tuning based on the size of their data.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a><span data-ttu-id="58bfe-144">Akzeptieren der WebSocket-Anforderungen</span><span class="sxs-lookup"><span data-stu-id="58bfe-144">Accept WebSocket requests</span></span>

<span data-ttu-id="58bfe-145">Einem späteren Zeitpunkt im Lebenszyklus Anforderung (weiter unten in der `Configure` Methode oder in einer MVC-Aktion, z. B.) überprüfen Sie, ob es sich um eine websocketanforderung ist, und akzeptieren die WebSocket-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="58bfe-145">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="58bfe-146">Dieses Beispiel stammt aus einem späteren Zeitpunkt in der `Configure` Methode.</span><span class="sxs-lookup"><span data-stu-id="58bfe-146">This example is from later in the `Configure` method.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

<span data-ttu-id="58bfe-147">Eine websocketanforderung konnte stammen, auf eine beliebige URL, aber in diesem Beispielcode akzeptiert nur Anforderungen für `/ws`.</span><span class="sxs-lookup"><span data-stu-id="58bfe-147">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="58bfe-148">Senden und Empfangen von Nachrichten</span><span class="sxs-lookup"><span data-stu-id="58bfe-148">Send and receive messages</span></span>

<span data-ttu-id="58bfe-149">Die `AcceptWebSocketAsync` Methode aktualisiert die TCP-Verbindung, um eine WebSocket-Verbindung, und bietet Ihnen eine [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) Objekt.</span><span class="sxs-lookup"><span data-stu-id="58bfe-149">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and gives you a [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="58bfe-150">Verwenden Sie das WebSocket-Objekt zum Senden und Empfangen von Nachrichten.</span><span class="sxs-lookup"><span data-stu-id="58bfe-150">Use the WebSocket object to send and receive messages.</span></span>

<span data-ttu-id="58bfe-151">Übergibt, akzeptiert die WebSocket-Anforderung den zuvor aufgeführten Code die `WebSocket` -Objekt an eine `Echo` Methode; hier ist die `Echo` Methode.</span><span class="sxs-lookup"><span data-stu-id="58bfe-151">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method; here's the `Echo` method.</span></span> <span data-ttu-id="58bfe-152">Der Code empfängt eine Nachricht und die gleiche Nachricht sofort sendet.</span><span class="sxs-lookup"><span data-stu-id="58bfe-152">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="58bfe-153">Es bleibt in einer Schleife, die auf diese Weise bis der Client die Verbindung geschlossen wird.</span><span class="sxs-lookup"><span data-stu-id="58bfe-153">It stays in a loop doing that until the client closes the connection.</span></span> 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

<span data-ttu-id="58bfe-154">Wenn Sie die WebSocket vor Beginn dieser Schleife annehmen, endet die middlewarepipeline.</span><span class="sxs-lookup"><span data-stu-id="58bfe-154">When you accept the WebSocket before beginning this loop, the middleware pipeline ends.</span></span>  <span data-ttu-id="58bfe-155">Beim Schließen des Sockets an, die entlädt der Pipelines aus.</span><span class="sxs-lookup"><span data-stu-id="58bfe-155">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="58bfe-156">D. h. würde der Anforderung beendet wird, die in der Pipeline Hostdaten, wenn Sie annehmen, dass ein WebSocket, genau wie bei eine MVC-Aktion, z. B. erreichen.</span><span class="sxs-lookup"><span data-stu-id="58bfe-156">That is, the request stops moving forward in the pipeline when you accept a WebSocket, just as it would when you hit an MVC action, for example.</span></span>  <span data-ttu-id="58bfe-157">Aber wenn Sie diese Schleife beendet, und den Socket schließt, wird die Anforderung sichern Sie die Pipeline fortgesetzt.</span><span class="sxs-lookup"><span data-stu-id="58bfe-157">But when you finish this loop and close the socket, the request proceeds back up the pipeline.</span></span>

## <a name="next-steps"></a><span data-ttu-id="58bfe-158">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="58bfe-158">Next steps</span></span>

<span data-ttu-id="58bfe-159">Die [beispielanwendung](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) , die begleitet dieser Artikel ist eine einfache Echo-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="58bfe-159">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) that accompanies this article is a simple echo application.</span></span> <span data-ttu-id="58bfe-160">Es wurde eine Webseite, die WebSocket-Verbindungen herstellt, und der Server sendet nur an den Client alle empfangenen Nachrichten.</span><span class="sxs-lookup"><span data-stu-id="58bfe-160">It has a web page that makes WebSocket connections, and the server just resends back to the client any messages it receives.</span></span> <span data-ttu-id="58bfe-161">Führen Sie es über eine Eingabeaufforderung (es hat nicht Einrichten von Visual Studio mit IIS Express ausgeführt), und navigieren Sie zu http://localhost: 5000.</span><span class="sxs-lookup"><span data-stu-id="58bfe-161">Run it from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="58bfe-162">Die Webseite zeigt den Verbindungsstatus auf der linken oberen Ecke:</span><span class="sxs-lookup"><span data-stu-id="58bfe-162">The web page shows connection status at the upper left:</span></span>

![Anfangszustand der Webseite](websockets/_static/start.png)

<span data-ttu-id="58bfe-164">Wählen Sie **verbinden** um eine websocketanforderung an die gezeigte URL senden.</span><span class="sxs-lookup"><span data-stu-id="58bfe-164">Select **Connect** to send a WebSocket request to the URL shown.</span></span>  <span data-ttu-id="58bfe-165">Geben Sie eine Testnachricht, und wählen Sie **senden**.</span><span class="sxs-lookup"><span data-stu-id="58bfe-165">Enter a test message and select **Send**.</span></span> <span data-ttu-id="58bfe-166">Aus und klicken Sie **schließen Socket**.</span><span class="sxs-lookup"><span data-stu-id="58bfe-166">When done, select **Close Socket**.</span></span> <span data-ttu-id="58bfe-167">Die **Kommunikation Protokoll** Abschnitt meldet jede öffnen, senden und schließen sie die Aktion durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="58bfe-167">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Anfangszustand der Webseite](websockets/_static/end.png)
