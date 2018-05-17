---
title: WebSockets-Unterstützung in ASP.NET Core
author: tdykstra
description: Erfahren Sie, wie Sie mit WebSockets in ASP.NET beginnen.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/websockets
ms.openlocfilehash: e744ab5b81ff85f48edb012a86b55003cc74929c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
---
# <a name="websockets-support-in-aspnet-core"></a><span data-ttu-id="7f214-103">WebSockets-Unterstützung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7f214-103">WebSockets support in ASP.NET Core</span></span>

<span data-ttu-id="7f214-104">Von [Tom Dykstra](https://github.com/tdykstra) und [Andrew Stanton-Nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="7f214-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="7f214-105">In diesem Artikel erfahren Sie, wie Sie mit WebSockets in ASP.NET beginnen.</span><span class="sxs-lookup"><span data-stu-id="7f214-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="7f214-106">Bei [WebSockets](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) handelt es sich um ein Protokoll, das bidirektionale persistente Kommunikationskanäle über TCP-Verbindungen ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="7f214-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="7f214-107">Es wird in Apps verwendet, die von schneller Kommunikation in Echtzeit profitieren, z.B. Chats, Dashboards und Spiele-Apps.</span><span class="sxs-lookup"><span data-stu-id="7f214-107">It's used in apps that benefit from fast, real-time communication, such as chat, dashboard, and game apps.</span></span>

<span data-ttu-id="7f214-108">[Zeigen Sie Beispielcode an, oder laden Sie diesen herunter](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="7f214-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="7f214-109">Weitere Informationen finden Sie im Abschnitt [Nächste Schritte](#next-steps).</span><span class="sxs-lookup"><span data-stu-id="7f214-109">See the [Next steps](#next-steps) section for more information.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7f214-110">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="7f214-110">Prerequisites</span></span>

* <span data-ttu-id="7f214-111">ASP.NET Core 1.1 oder höher</span><span class="sxs-lookup"><span data-stu-id="7f214-111">ASP.NET Core 1.1 or later</span></span>
* <span data-ttu-id="7f214-112">Jedes Betriebssystem, das ASP.NET Core unterstützt:</span><span class="sxs-lookup"><span data-stu-id="7f214-112">Any OS that supports ASP.NET Core:</span></span>
  
  * <span data-ttu-id="7f214-113">Windows 7/Windows Server 2008 und höher</span><span class="sxs-lookup"><span data-stu-id="7f214-113">Windows 7 / Windows Server 2008 or later</span></span>
  * <span data-ttu-id="7f214-114">Linux</span><span class="sxs-lookup"><span data-stu-id="7f214-114">Linux</span></span>
  * <span data-ttu-id="7f214-115">macOS</span><span class="sxs-lookup"><span data-stu-id="7f214-115">macOS</span></span>
  
* <span data-ttu-id="7f214-116">Wenn die App unter Windows mit IIS ausgeführt wird:</span><span class="sxs-lookup"><span data-stu-id="7f214-116">If the app runs on Windows with IIS:</span></span>

  * <span data-ttu-id="7f214-117">Windows 8/Windows Server 2012 und höher</span><span class="sxs-lookup"><span data-stu-id="7f214-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="7f214-118">IIS 8/IIS 8 Express</span><span class="sxs-lookup"><span data-stu-id="7f214-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="7f214-119">WebSockets müssen in IIS aktiviert sein (weitere Informationen finden Sie im Abschnitt [Unterstützung für IIS und IIS Express](#iisiis-express-support)).</span><span class="sxs-lookup"><span data-stu-id="7f214-119">WebSockets must be enabled in IIS (See the [IIS/IIS Express support](#iisiis-express-support) section.)</span></span>
  
* <span data-ttu-id="7f214-120">Wenn die App unter [HTTP.sys](xref:fundamentals/servers/httpsys) ausgeführt wird:</span><span class="sxs-lookup"><span data-stu-id="7f214-120">If the app runs on [HTTP.sys](xref:fundamentals/servers/httpsys):</span></span>

  * <span data-ttu-id="7f214-121">Windows 8/Windows Server 2012 und höher</span><span class="sxs-lookup"><span data-stu-id="7f214-121">Windows 8 / Windows Server 2012 or later</span></span>

* <span data-ttu-id="7f214-122">Weitere Informationen zu unterstützten Browsern finden Sie unter https://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="7f214-122">For supported browsers, see https://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-websockets"></a><span data-ttu-id="7f214-123">Verwendungszweck von WebSockets</span><span class="sxs-lookup"><span data-stu-id="7f214-123">When to use WebSockets</span></span>

<span data-ttu-id="7f214-124">Verwenden Sie WebSockets, um direkt mit einer Socketverbindung zu arbeiten.</span><span class="sxs-lookup"><span data-stu-id="7f214-124">Use WebSockets to work directly with a socket connection.</span></span> <span data-ttu-id="7f214-125">Verwenden Sie WebSockets beispielsweise für die bestmögliche Leistung bei einem Echtzeitspiel.</span><span class="sxs-lookup"><span data-stu-id="7f214-125">For example, use WebSockets for the best possible performance with a real-time game.</span></span>

<span data-ttu-id="7f214-126">[ASP.NET SignalR](/aspnet/signalr/overview/getting-started/introduction-to-signalr) stellt ein erweitertes App-Modell für Echtzeitfunktionen bereit, aber es kann nur in ASP.NET 4.x und nicht in ASP.NET Core ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="7f214-126">[ASP.NET SignalR](/aspnet/signalr/overview/getting-started/introduction-to-signalr) provides a richer app model for real-time functionality, but it only runs on ASP.NET 4.x, not ASP.NET Core.</span></span> <span data-ttu-id="7f214-127">Eine ASP.NET Core-Version von SignalR ist für das Release von ASP.NET Core 2.1 geplant.</span><span class="sxs-lookup"><span data-stu-id="7f214-127">An ASP.NET Core version of SignalR is scheduled for release with ASP.NET Core 2.1.</span></span> <span data-ttu-id="7f214-128">Weitere Informationen finden Sie unter [ASP.NET Core 2.1 high-level planning (Grundlegende Planung von ASP.NET Core 2.1)](https://github.com/aspnet/Announcements/issues/288).</span><span class="sxs-lookup"><span data-stu-id="7f214-128">See [ASP.NET Core 2.1 high-level planning](https://github.com/aspnet/Announcements/issues/288).</span></span>

<span data-ttu-id="7f214-129">Vor dem Release von SignalR Core können WebSockets verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="7f214-129">Until SignalR Core is released, WebSockets can be used.</span></span> <span data-ttu-id="7f214-130">Die Features von SignalR müssen jedoch vom Entwickler bereitgestellt und unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="7f214-130">However, features that SignalR provides must be provided and supported by the developer.</span></span> <span data-ttu-id="7f214-131">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="7f214-131">For example:</span></span>

* <span data-ttu-id="7f214-132">Unterstützung für eine größere Zahl an unterschiedlichen Browserversionen, indem Sie ein automatisches Fallback auf alternative Transportmethoden einsetzen</span><span class="sxs-lookup"><span data-stu-id="7f214-132">Support for a broader range of browser versions by using automatic fallback to alternative transport methods.</span></span>
* <span data-ttu-id="7f214-133">Der automatische Aufbau einer neuen Verbindung, wenn die Verbindung unterbrochen wurde</span><span class="sxs-lookup"><span data-stu-id="7f214-133">Automatic reconnection when a connection drops.</span></span>
* <span data-ttu-id="7f214-134">Unterstützung für das Aufrufen von Methoden auf dem Server durch Clients bzw. andersherum</span><span class="sxs-lookup"><span data-stu-id="7f214-134">Support for clients calling methods on the server or vice versa.</span></span>
* <span data-ttu-id="7f214-135">Unterstützung für die Skalierung auf mehrere Server</span><span class="sxs-lookup"><span data-stu-id="7f214-135">Support for scaling to multiple servers.</span></span>

## <a name="how-to-use-it"></a><span data-ttu-id="7f214-136">Verwendungsweise</span><span class="sxs-lookup"><span data-stu-id="7f214-136">How to use it</span></span>

* <span data-ttu-id="7f214-137">Installieren Sie das Paket [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/).</span><span class="sxs-lookup"><span data-stu-id="7f214-137">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="7f214-138">Konfigurieren Sie die Middleware.</span><span class="sxs-lookup"><span data-stu-id="7f214-138">Configure the middleware.</span></span>
* <span data-ttu-id="7f214-139">Akzeptieren Sie Anforderungen von WebSocket.</span><span class="sxs-lookup"><span data-stu-id="7f214-139">Accept WebSocket requests.</span></span>
* <span data-ttu-id="7f214-140">Senden Sie Nachrichten, und empfangen Sie diese.</span><span class="sxs-lookup"><span data-stu-id="7f214-140">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="7f214-141">Konfigurieren der Middleware</span><span class="sxs-lookup"><span data-stu-id="7f214-141">Configure the middleware</span></span>

<span data-ttu-id="7f214-142">Fügen Sie die WebSockets-Middleware zur `Configure`-Methode der `Startup`-Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="7f214-142">Add the WebSockets middleware in the `Configure` method of the `Startup` class:</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

<span data-ttu-id="7f214-143">Sie können die folgenden Einstellungen konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="7f214-143">The following settings can be configured:</span></span>

* <span data-ttu-id="7f214-144">`KeepAliveInterval`: Legt fest, wie oft Ping-Frames an den Client gesendet werden, um sicherzustellen, dass Proxys die Verbindung aufrechterhalten</span><span class="sxs-lookup"><span data-stu-id="7f214-144">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span>
* <span data-ttu-id="7f214-145">`ReceiveBufferSize`: Legt die Größe des Puffers fest, der zum Empfang von Daten verwendet wird</span><span class="sxs-lookup"><span data-stu-id="7f214-145">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="7f214-146">Fortgeschrittene Benutzer müssen diese Angaben möglicherweise ändern, um die Leistung auf Grundlage der Größe ihrer Daten anzupassen.</span><span class="sxs-lookup"><span data-stu-id="7f214-146">Advanced users may need to change this for performance tuning based on the size of the data.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a><span data-ttu-id="7f214-147">Akzeptieren der Anforderungen von WebSocket</span><span class="sxs-lookup"><span data-stu-id="7f214-147">Accept WebSocket requests</span></span>

<span data-ttu-id="7f214-148">Prüfen Sie zu einem späteren Zeitpunkt im Lebenszyklus einer Anforderung (z.B. später in der `Configure`-Methode oder in einer MVC-Aktion), ob es sich um eine WebSocket-Anforderung handelt, und akzeptieren Sie diese.</span><span class="sxs-lookup"><span data-stu-id="7f214-148">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="7f214-149">Das folgende Beispiel ist ein späterer Auszug der `Configure`-Methode.</span><span class="sxs-lookup"><span data-stu-id="7f214-149">The following example is from later in the `Configure` method:</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

<span data-ttu-id="7f214-150">WebSocket-Anforderungen können bei jeder URL eingehen. Dieser Beispielcode akzeptiert jedoch nur Anforderungen für `/ws`.</span><span class="sxs-lookup"><span data-stu-id="7f214-150">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="7f214-151">Senden und Empfangen von Nachrichten</span><span class="sxs-lookup"><span data-stu-id="7f214-151">Send and receive messages</span></span>

<span data-ttu-id="7f214-152">Die `AcceptWebSocketAsync`-Methode ändert die TCP-Verbindung in eine WebSocket-Verbindung und stellt ein [WebSocket](/dotnet/core/api/system.net.websockets.websocket)-Objekt bereit.</span><span class="sxs-lookup"><span data-stu-id="7f214-152">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and provides a [WebSocket](/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="7f214-153">Verwenden Sie das `WebSocket`-Objekt, um Nachrichten zu senden und zu empfangen.</span><span class="sxs-lookup"><span data-stu-id="7f214-153">Use the `WebSocket` object to send and receive messages.</span></span>

<span data-ttu-id="7f214-154">Der weiter oben gezeigte Code, der WebSocket-Anforderungen akzeptiert, übergibt das `WebSocket`-Objekt an eine `Echo`-Methode.</span><span class="sxs-lookup"><span data-stu-id="7f214-154">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method.</span></span> <span data-ttu-id="7f214-155">Der Code empfängt eine Nachricht und sendet diese umgehend wieder zurück.</span><span class="sxs-lookup"><span data-stu-id="7f214-155">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="7f214-156">Nachrichten werden in einer Schleife gesendet und empfangen, bis der Client die Verbindung schließt:</span><span class="sxs-lookup"><span data-stu-id="7f214-156">Messages are sent and received in a loop until the client closes the connection:</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

<span data-ttu-id="7f214-157">Wenn Sie die WebSocket-Verbindung vor Beginn der Schleife akzeptieren, endet die Middlewarepipeline.</span><span class="sxs-lookup"><span data-stu-id="7f214-157">When accepting the WebSocket connection before beginning the loop, the middleware pipeline ends.</span></span> <span data-ttu-id="7f214-158">Wenn Sie das Socket schließen, wird die Pipeline entladen.</span><span class="sxs-lookup"><span data-stu-id="7f214-158">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="7f214-159">Das bedeutet, dass sich die Anforderung in der Pipeline nicht mehr weiter bewegt, wenn der WebSocket akzeptiert wird.</span><span class="sxs-lookup"><span data-stu-id="7f214-159">That is, the request stops moving forward in the pipeline when the WebSocket is accepted.</span></span> <span data-ttu-id="7f214-160">Wenn die Schleife beendet und der Socket geschlossen ist, wird die Anforderung in der Pipeline weiter verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="7f214-160">When the loop is finished and the socket is closed, the request proceeds back up the pipeline.</span></span>

## <a name="iisiis-express-support"></a><span data-ttu-id="7f214-161">Unterstützung für IIS und IIS Express</span><span class="sxs-lookup"><span data-stu-id="7f214-161">IIS/IIS Express support</span></span>

<span data-ttu-id="7f214-162">In Windows Server 2012 oder höher und in Windows 8 oder höher mit IIS 8 oder IIS Express 8 oder höher ist die Unterstützung für das WebSocket-Protokoll enthalten.</span><span class="sxs-lookup"><span data-stu-id="7f214-162">Windows Server 2012 or later and Windows 8 or later with IIS/IIS Express 8 or later has support for the WebSocket protocol.</span></span>

<span data-ttu-id="7f214-163">So aktivieren Sie die Unterstützung für das WebSocket-Protokoll unter Windows Server 2012 oder höher:</span><span class="sxs-lookup"><span data-stu-id="7f214-163">To enable support for the WebSocket protocol on Windows Server 2012 or later:</span></span>

1. <span data-ttu-id="7f214-164">Verwenden Sie den Assistenten **Rollen und Features hinzufügen** im Menü **Verwalten** oder den Link in **Server-Manager**.</span><span class="sxs-lookup"><span data-stu-id="7f214-164">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span>
1. <span data-ttu-id="7f214-165">Klicken Sie auf **Rollenbasierte oder featurebasierte Installation**.</span><span class="sxs-lookup"><span data-stu-id="7f214-165">Select **Role-based or Feature-based Installation**.</span></span> <span data-ttu-id="7f214-166">Klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="7f214-166">Select **Next**.</span></span>
1. <span data-ttu-id="7f214-167">Wählen Sie den entsprechenden Server aus (standardmäßig ist der lokale Server ausgewählt).</span><span class="sxs-lookup"><span data-stu-id="7f214-167">Select the appropriate server (the local server is selected by default).</span></span> <span data-ttu-id="7f214-168">Klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="7f214-168">Select **Next**.</span></span>
1. <span data-ttu-id="7f214-169">Erweitern Sie **Webserver (IIS)** in der Struktur **Rollen**, und erweitern Sie **Webserver** und anschließend **Anwendungsentwicklung**.</span><span class="sxs-lookup"><span data-stu-id="7f214-169">Expand **Web Server (IIS)** in the **Roles** tree, expand **Web Server**, and then expand **Application Development**.</span></span>
1. <span data-ttu-id="7f214-170">Wählen Sie **WebSocket-Protokoll** aus.</span><span class="sxs-lookup"><span data-stu-id="7f214-170">Select **WebSocket Protocol**.</span></span> <span data-ttu-id="7f214-171">Klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="7f214-171">Select **Next**.</span></span>
1. <span data-ttu-id="7f214-172">Wenn keine zusätzlichen Features erforderlich sind, klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="7f214-172">If additional features aren't needed, select **Next**.</span></span>
1. <span data-ttu-id="7f214-173">Klicken Sie auf **Installieren**.</span><span class="sxs-lookup"><span data-stu-id="7f214-173">Select **Install**.</span></span>
1. <span data-ttu-id="7f214-174">Wenn die Installation abgeschlossen ist, klicken Sie auf **Schließen**, um den Assistenten zu beenden.</span><span class="sxs-lookup"><span data-stu-id="7f214-174">When the installation completes, select **Close** to exit the wizard.</span></span>

<span data-ttu-id="7f214-175">So aktivieren Sie die Unterstützung für das WebSocket-Protokoll unter Windows 8 oder höher:</span><span class="sxs-lookup"><span data-stu-id="7f214-175">To enable support for the WebSocket protocol on Windows 8 or later:</span></span>

1. <span data-ttu-id="7f214-176">Navigieren Sie zu **Systemsteuerung** > **Programme** > **Programme und Features** > **Windows-Features aktivieren oder deaktivieren** (links auf dem Bildschirm).</span><span class="sxs-lookup"><span data-stu-id="7f214-176">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="7f214-177">Öffnen Sie folgende Knoten: **Internetinformationsdienste** > **WWW-Dienste** > **Anwendungsentwicklungsfeatures**.</span><span class="sxs-lookup"><span data-stu-id="7f214-177">Open the following nodes: **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="7f214-178">Wählen Sie das Feature **WebSocket-Protokoll** aus.</span><span class="sxs-lookup"><span data-stu-id="7f214-178">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="7f214-179">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="7f214-179">Select **OK**.</span></span>

<span data-ttu-id="7f214-180">**Deaktivieren von WebSocket bei Verwendung von „socket.io“ in „Node.js“**</span><span class="sxs-lookup"><span data-stu-id="7f214-180">**Disable WebSocket when using socket.io on node.js**</span></span>

<span data-ttu-id="7f214-181">Wenn Sie die WebSocket-Unterstützung in [socket.io](https://socket.io/) in [Node.js](https://nodejs.org/) verwenden, deaktivieren Sie das IIS-WebSocket-Standardmodul mithilfe des `webSocket`-Elements in *web.config* oder *applicationHost.config*. Wenn dieser Schritt nicht durchgeführt wird, versucht das IIS-WebSocket-Modul, die WebSocket-Kommunikation statt Node.js und der App zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="7f214-181">If using the WebSocket support in [socket.io](https://socket.io/) on [Node.js](https://nodejs.org/), disable the default IIS WebSocket module using the `webSocket` element in *web.config* or *applicationHost.config*. If this step isn't performed, the IIS WebSocket module attempts to handle the WebSocket communication rather than Node.js and the app.</span></span>

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a><span data-ttu-id="7f214-182">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="7f214-182">Next steps</span></span>

<span data-ttu-id="7f214-183">Die [Beispiel-App](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample), die in diesem Artikel verwendet wird, ist eine Echo-App.</span><span class="sxs-lookup"><span data-stu-id="7f214-183">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) that accompanies this article is an echo app.</span></span> <span data-ttu-id="7f214-184">Sie verfügt über eine Webseite, die WebSocket-Verbindungen herstellt. Der Server schickt alle empfangenen Nachrichten zurück an den Client.</span><span class="sxs-lookup"><span data-stu-id="7f214-184">It has a web page that makes WebSocket connections, and the server resends any messages it receives back to the client.</span></span> <span data-ttu-id="7f214-185">Führen Sie die App über eine Eingabeaufforderung aus (sie ist nicht darauf ausgelegt, von Visual Studio mit IIS Express ausgeführt zu werden), und navigieren Sie zu https://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="7f214-185">Run the app from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="7f214-186">Die Webseite zeigt den Verbindungsstatus in der oberen linken Ecke an:</span><span class="sxs-lookup"><span data-stu-id="7f214-186">The web page shows the connection status in the upper left:</span></span>

![Erster Zustand der Webseite](websockets/_static/start.png)

<span data-ttu-id="7f214-188">Klicken Sie auf **Connect** (Verbinden), um eine WebSocket-Anforderung an die gezeigte URL zu senden.</span><span class="sxs-lookup"><span data-stu-id="7f214-188">Select **Connect** to send a WebSocket request to the URL shown.</span></span> <span data-ttu-id="7f214-189">Geben Sie einen Testtext ein, und klicken Sie auf **Send** (Senden).</span><span class="sxs-lookup"><span data-stu-id="7f214-189">Enter a test message and select **Send**.</span></span> <span data-ttu-id="7f214-190">Wenn dies abgeschlossen ist, klicken Sie auf **Close Socket** (Socket schließen).</span><span class="sxs-lookup"><span data-stu-id="7f214-190">When done, select **Close Socket**.</span></span> <span data-ttu-id="7f214-191">Der Abschnitt **Kommunikationsprotokoll** meldet jede open-, send- und close-Aktion, wenn diese durchgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="7f214-191">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Erster Zustand der Webseite](websockets/_static/end.png)
