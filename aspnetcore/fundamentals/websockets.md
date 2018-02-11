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
# <a name="introduction-to-websockets-in-aspnet-core"></a><span data-ttu-id="febdd-103">Einführung in WebSockets in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="febdd-103">Introduction to WebSockets in ASP.NET Core</span></span>

<span data-ttu-id="febdd-104">Von [Tom Dykstra](https://github.com/tdykstra) und [Andrew Stanton-Nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="febdd-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="febdd-105">In diesem Artikel erfahren Sie, wie Sie mit WebSockets in ASP.NET beginnen.</span><span class="sxs-lookup"><span data-stu-id="febdd-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="febdd-106">Bei [WebSockets](https://wikipedia.org/wiki/WebSocket) handelt es sich um ein Protokoll, das bidirektionale persistente Kommunikationskanäle über TCP-Verbindungen ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="febdd-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="febdd-107">Es wird in Chat-, Börsenticker- und Spiele-Apps verwendet – überall dort, wo Echtzeitfunktionen in einer Web-App benötigt werden.</span><span class="sxs-lookup"><span data-stu-id="febdd-107">It's used for applications such as chat, stock tickers, games, anywhere you want real-time functionality in a web application.</span></span>

<span data-ttu-id="febdd-108">[Zeigen Sie Beispielcode an, oder laden Sie diesen herunter](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="febdd-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="febdd-109">Weitere Informationen finden Sie im Abschnitt [Nächste Schritte](#next-steps).</span><span class="sxs-lookup"><span data-stu-id="febdd-109">See the [Next Steps](#next-steps) section for more information.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="febdd-110">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="febdd-110">Prerequisites</span></span>

* <span data-ttu-id="febdd-111">ASP.NET Core 1.1 (kann nicht in 1.0 ausgeführt werden)</span><span class="sxs-lookup"><span data-stu-id="febdd-111">ASP.NET Core 1.1 (doesn't run on 1.0)</span></span>
* <span data-ttu-id="febdd-112">Jedes Betriebssystem, unter dem ASP.NET Core ausgeführt wird:</span><span class="sxs-lookup"><span data-stu-id="febdd-112">Any OS that ASP.NET Core runs on:</span></span>
  
  * <span data-ttu-id="febdd-113">Windows 7/Windows Server 2008 und höher</span><span class="sxs-lookup"><span data-stu-id="febdd-113">Windows 7 / Windows Server 2008 and later</span></span>
  * <span data-ttu-id="febdd-114">Linux</span><span class="sxs-lookup"><span data-stu-id="febdd-114">Linux</span></span>
  * <span data-ttu-id="febdd-115">macOS</span><span class="sxs-lookup"><span data-stu-id="febdd-115">macOS</span></span>

* <span data-ttu-id="febdd-116">**Ausnahme:** Wenn Ihre App unter Windows mit IIS oder mit WebListener ausgeführt wird, müssen Sie Folgendes verwenden:</span><span class="sxs-lookup"><span data-stu-id="febdd-116">**Exception**: If your app runs on Windows with IIS, or with WebListener, you must use:</span></span>

  * <span data-ttu-id="febdd-117">Windows 8/Windows Server 2012 und höher</span><span class="sxs-lookup"><span data-stu-id="febdd-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="febdd-118">IIS 8/IIS 8 Express</span><span class="sxs-lookup"><span data-stu-id="febdd-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="febdd-119">WebSocket muss in IIS aktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="febdd-119">WebSocket must be enabled in IIS</span></span>

* <span data-ttu-id="febdd-120">Informationen zu unterstützten Browsern finden Sie unter http://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="febdd-120">For supported browsers, see http://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-it"></a><span data-ttu-id="febdd-121">Empfohlene Verwendung</span><span class="sxs-lookup"><span data-stu-id="febdd-121">When to use it</span></span>

<span data-ttu-id="febdd-122">Verwenden Sie WebSockets, wenn Sie direkt mit einer Socketverbindung arbeiten müssen.</span><span class="sxs-lookup"><span data-stu-id="febdd-122">Use WebSockets when you need to work directly with a socket connection.</span></span> <span data-ttu-id="febdd-123">Für ein Echtzeitspiel benötigen Sie z.B. die höchste Leistung.</span><span class="sxs-lookup"><span data-stu-id="febdd-123">For example, you might need the best possible performance for a real-time game.</span></span>

<span data-ttu-id="febdd-124">[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) stellt ein erweitertes Anwendungsmodell für Echtzeitfunktionen bereit, aber es kann nur in ASP.NET und nicht in ASP.NET Core ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="febdd-124">[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) provides a richer application model for real-time functionality, but it runs only on ASP.NET, not ASP.NET Core.</span></span> <span data-ttu-id="febdd-125">Im Moment wird an der Entwicklung einer Core-Version von SignalR gearbeitet. Im [GitHub-Repository für SignalR Core](https://github.com/aspnet/SignalR) können Sie sich den Fortschritt ansehen.</span><span class="sxs-lookup"><span data-stu-id="febdd-125">A Core version of SignalR is under development; to follow its progress, see the [GitHub repository for SignalR Core](https://github.com/aspnet/SignalR).</span></span>

<span data-ttu-id="febdd-126">Wenn Sie nicht auf SignalR warten möchten, können Sie jetzt direkt WebSockets verwenden.</span><span class="sxs-lookup"><span data-stu-id="febdd-126">If you don't want to wait for SignalR Core, you can use WebSockets directly now.</span></span> <span data-ttu-id="febdd-127">Möglicherweise müssen Sie jedoch Features entwickeln, die ansonsten von SignalR bereitgestellt würden. Dazu gehören z.B.:</span><span class="sxs-lookup"><span data-stu-id="febdd-127">But you might have to develop features that SignalR would provide, such as:</span></span>

* <span data-ttu-id="febdd-128">Unterstützung für eine größere Zahl an unterschiedlichen Browserversionen, indem Sie ein automatisches Fallback auf alternative Transportmethoden einsetzen</span><span class="sxs-lookup"><span data-stu-id="febdd-128">Support for a broader range of browser versions by using automatic fallback to alternative transport methods.</span></span>
* <span data-ttu-id="febdd-129">Der automatische Aufbau einer neuen Verbindung, wenn die Verbindung unterbrochen wurde</span><span class="sxs-lookup"><span data-stu-id="febdd-129">Automatic reconnection when a connection drops.</span></span>
* <span data-ttu-id="febdd-130">Unterstützung für das Aufrufen von Methoden auf dem Server durch Clients bzw. andersherum</span><span class="sxs-lookup"><span data-stu-id="febdd-130">Support for clients calling methods on the server or vice versa.</span></span>
* <span data-ttu-id="febdd-131">Unterstützung für die Skalierung auf mehrere Server</span><span class="sxs-lookup"><span data-stu-id="febdd-131">Support for scaling to multiple servers.</span></span>

## <a name="how-to-use-it"></a><span data-ttu-id="febdd-132">Verwendungsweise</span><span class="sxs-lookup"><span data-stu-id="febdd-132">How to use it</span></span>

* <span data-ttu-id="febdd-133">Installieren Sie das Paket [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/).</span><span class="sxs-lookup"><span data-stu-id="febdd-133">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="febdd-134">Konfigurieren Sie die Middleware.</span><span class="sxs-lookup"><span data-stu-id="febdd-134">Configure the middleware.</span></span>
* <span data-ttu-id="febdd-135">Akzeptieren Sie Anforderungen von WebSocket.</span><span class="sxs-lookup"><span data-stu-id="febdd-135">Accept WebSocket requests.</span></span>
* <span data-ttu-id="febdd-136">Senden Sie Nachrichten, und empfangen Sie diese.</span><span class="sxs-lookup"><span data-stu-id="febdd-136">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="febdd-137">Konfigurieren der Middleware</span><span class="sxs-lookup"><span data-stu-id="febdd-137">Configure the middleware</span></span>

<span data-ttu-id="febdd-138">Fügen Sie die WebSockets-Middleware in der `Configure`-Methode der `Startup`-Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="febdd-138">Add the WebSockets middleware in the `Configure` method of the `Startup` class.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

<span data-ttu-id="febdd-139">Sie können die folgenden Einstellungen konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="febdd-139">The following settings can be configured:</span></span>

* <span data-ttu-id="febdd-140">`KeepAliveInterval`: Legt fest, wie oft „Ping“-Frames an den Client gesendet werden, um sicherzustellen, dass Proxys die Verbindung aufrechterhalten</span><span class="sxs-lookup"><span data-stu-id="febdd-140">`KeepAliveInterval` - How frequently to send "ping" frames to the client, to ensure proxies keep the connection open.</span></span>
* <span data-ttu-id="febdd-141">`ReceiveBufferSize`: Legt die Größe des Puffers fest, der zum Empfang von Daten verwendet wird</span><span class="sxs-lookup"><span data-stu-id="febdd-141">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="febdd-142">Nur fortgeschrittene Benutzer müssen diese Angaben ändern, um die Leistung auf Grundlage der Größe ihrer Daten anzupassen.</span><span class="sxs-lookup"><span data-stu-id="febdd-142">Only advanced users would need to change this, for performance tuning based on the size of their data.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a><span data-ttu-id="febdd-143">Akzeptieren der Anforderungen von WebSocket</span><span class="sxs-lookup"><span data-stu-id="febdd-143">Accept WebSocket requests</span></span>

<span data-ttu-id="febdd-144">Prüfen Sie zu einem späteren Zeitpunkt im Lebenszyklus einer Anforderung (z.B. später in der `Configure`-Methode oder in einer MVC-Aktion), ob es sich um eine WebSocket-Anforderung handelt, und akzeptieren Sie diese.</span><span class="sxs-lookup"><span data-stu-id="febdd-144">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="febdd-145">Dieses Beispiel ist ein späterer Auszug der `Configure`-Methode.</span><span class="sxs-lookup"><span data-stu-id="febdd-145">This example is from later in the `Configure` method.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

<span data-ttu-id="febdd-146">WebSocket-Anforderungen können bei jeder URL eingehen. Dieser Beispielcode akzeptiert jedoch nur Anforderungen für `/ws`.</span><span class="sxs-lookup"><span data-stu-id="febdd-146">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="febdd-147">Senden und Empfangen von Nachrichten</span><span class="sxs-lookup"><span data-stu-id="febdd-147">Send and receive messages</span></span>

<span data-ttu-id="febdd-148">Die `AcceptWebSocketAsync`-Methode ändert die TCP-Verbindung in eine WebSocket-Verbindung, und gibt Ihnen ein [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket)-Objekt.</span><span class="sxs-lookup"><span data-stu-id="febdd-148">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and gives you a [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="febdd-149">Verwenden Sie das WebSocket-Objekt, um Nachrichten zu senden und zu empfangen.</span><span class="sxs-lookup"><span data-stu-id="febdd-149">Use the WebSocket object to send and receive messages.</span></span>

<span data-ttu-id="febdd-150">Der weiter oben gezeigte Code, der WebSocket-Anforderungen akzeptiert, übergibt das `WebSocket`-Objekt an eine `Echo`-Methode. Hier sehen Sie die `Echo`-Methode.</span><span class="sxs-lookup"><span data-stu-id="febdd-150">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method; here's the `Echo` method.</span></span> <span data-ttu-id="febdd-151">Der Code empfängt eine Nachricht und sendet diese umgehend wieder zurück.</span><span class="sxs-lookup"><span data-stu-id="febdd-151">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="febdd-152">Diese Schleife bleibt bestehen, bis die Verbindung des Clients unterbrochen wird.</span><span class="sxs-lookup"><span data-stu-id="febdd-152">It stays in a loop doing that until the client closes the connection.</span></span> 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

<span data-ttu-id="febdd-153">Wenn Sie das Websocket vor Beginn dieser Schleife akzeptieren, endet die Middlewarepipeline.</span><span class="sxs-lookup"><span data-stu-id="febdd-153">When you accept the WebSocket before beginning this loop, the middleware pipeline ends.</span></span>  <span data-ttu-id="febdd-154">Wenn Sie das Socket schließen, wird die Pipeline entladen.</span><span class="sxs-lookup"><span data-stu-id="febdd-154">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="febdd-155">Das bedeutet, dass die Anforderung die Pipeline nicht weiter durchläuft, wenn Sie ein Websocket akzeptieren, genauso als wenn Sie auf eine MVC-Aktion treffen.</span><span class="sxs-lookup"><span data-stu-id="febdd-155">That is, the request stops moving forward in the pipeline when you accept a WebSocket, just as it would when you hit an MVC action, for example.</span></span>  <span data-ttu-id="febdd-156">Wenn Sie diese Schleife beenden und das Socket schließen, durchläuft die Anforderung die Pipeline in umgekehrter Reihenfolge.</span><span class="sxs-lookup"><span data-stu-id="febdd-156">But when you finish this loop and close the socket, the request proceeds back up the pipeline.</span></span>

## <a name="next-steps"></a><span data-ttu-id="febdd-157">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="febdd-157">Next steps</span></span>

<span data-ttu-id="febdd-158">Die [Beispielanwendung](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample), die diesen Artikel begleitet, ist eine einfache Echoanwendung.</span><span class="sxs-lookup"><span data-stu-id="febdd-158">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) that accompanies this article is a simple echo application.</span></span> <span data-ttu-id="febdd-159">Sie verfügt über eine Webseite, die WebSocket-Verbindungen herstellt. Der Server schickt alle empfangenen Nachrichten zurück an den Client.</span><span class="sxs-lookup"><span data-stu-id="febdd-159">It has a web page that makes WebSocket connections, and the server just resends back to the client any messages it receives.</span></span> <span data-ttu-id="febdd-160">Führen Sie sie über eine Eingabeaufforderung aus (es ist nicht darauf ausgelegt, von Visual Studio mit IIS Express ausgeführt zu werden), und navigieren Sie zu https://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="febdd-160">Run it from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="febdd-161">Die Webseite zeigt den Verbindungsstatus in der oberen linken Ecke an:</span><span class="sxs-lookup"><span data-stu-id="febdd-161">The web page shows connection status at the upper left:</span></span>

![Erster Zustand der Webseite](websockets/_static/start.png)

<span data-ttu-id="febdd-163">Klicken Sie auf **Connect** (Verbinden), um eine WebSocket-Anforderung an die gezeigte URL zu senden.</span><span class="sxs-lookup"><span data-stu-id="febdd-163">Select **Connect** to send a WebSocket request to the URL shown.</span></span>  <span data-ttu-id="febdd-164">Geben Sie einen Testtext ein, und klicken Sie auf **Send** (Senden).</span><span class="sxs-lookup"><span data-stu-id="febdd-164">Enter a test message and select **Send**.</span></span> <span data-ttu-id="febdd-165">Wenn dies abgeschlossen ist, klicken Sie auf **Close Socket** (Socket schließen).</span><span class="sxs-lookup"><span data-stu-id="febdd-165">When done, select **Close Socket**.</span></span> <span data-ttu-id="febdd-166">Der Abschnitt **Kommunikationsprotokoll** meldet jede open-, send- und close-Aktion, wenn diese durchgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="febdd-166">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Erster Zustand der Webseite](websockets/_static/end.png)
