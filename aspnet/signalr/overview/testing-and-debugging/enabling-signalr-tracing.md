---
uid: signalr/overview/testing-and-debugging/enabling-signalr-tracing
title: "Aktivieren der Ablaufverfolgung für SignalR | Microsoft Docs"
author: tfitzmac
description: "Dieses Dokument beschreibt das Aktivieren und Konfigurieren der Ablaufverfolgung für SignalR-Server und Clients. Ablaufverfolgung können Sie Diagnoseinformationen zu Ereignissen anzeigen..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/08/2014
ms.topic: article
ms.assetid: 30060acb-be3e-4347-996f-3870f0c37829
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/enabling-signalr-tracing
msc.type: authoredcontent
ms.openlocfilehash: 2f01ab5d66e44cd82634f1b3df1ca6c78b7fd9d5
ms.sourcegitcommit: c07fb5cb5df0a12f9fe6735fcbc90964608fa687
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2017
---
<a name="enabling-signalr-tracing"></a>Aktivieren der Ablaufverfolgung für SignalR
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Dieses Dokument beschreibt das Aktivieren und Konfigurieren der Ablaufverfolgung für SignalR-Server und Clients. Ablaufverfolgung können Sie Diagnoseinformationen zu Ereignissen in der SignalR-Anwendung anzeigen.
> 
> Dieses Thema wurde ursprünglich von Patrick Fletcher geschrieben.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET Framework 4.5
> - SignalR Version 2
>   
> 
> 
> ## <a name="questions-and-comments"></a>Fragen und Kommentare
> 
> Lassen Sie Sie Feedback auf wie in diesem Lernprogramm mögen und was wir in den Kommentaren am unteren Rand der Seite verbessern können. Wenn Sie Fragen, die nicht direkt mit dem Lernprogramm verknüpft sind haben, bereitstellen können, die [ASP.NET SignalR-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder [StackOverflow.com](http://stackoverflow.com/).


Wenn Ablaufverfolgung aktiviert ist, erstellt eine Anwendung SignalR Protokolleinträge für Ereignisse an. Sie können vom Client und dem Server protokolliert werden. Auf die Serververbindung für die Protokolle, Scaleout-Anbieter und Bus nachrichtenereignisse verfolgen. Auf dem Client Protokolle Verbindungsereignisse Tracing. In SignalR 2.1 und höher protokolliert Ablaufverfolgung auf dem Client den vollständigen Inhalt von Nachrichten für Hub-Aufruf.

## <a name="contents"></a>Inhalt

- [Aktivieren der Ablaufverfolgung auf dem server](#server)

    - [Protokollierung Serverereignisse in Textdateien](#server_text)
    - [Serverereignisse in das Ereignisprotokoll zu protokollieren.](#server_eventlog)
- [Aktivieren der Ablaufverfolgung in der .NET Client (Windows-Desktop-apps)](#net_client)

    - [Protokollieren von Ereignissen an die Konsole Desktopclient](#desktop_console)
    - [Protokollieren von Ereignissen in eine Textdatei Desktopclient](#desktop_text)
- [Aktivieren der Ablaufverfolgung in Windows Phone 8-clients](#phone)

    - [Windows Phone-Client-Ereignisse zu protokollieren, auf die Benutzeroberfläche](#phone_ui)
    - [Windows Phone-Client-Ereignisse zu protokollieren, auf der Debugkonsole](#phone_debug)
- [Aktivieren der Ablaufverfolgung in der JavaScript-client](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a>Aktivieren der Ablaufverfolgung auf dem server

Sie aktivieren die Protokollierung auf dem Server innerhalb der Anwendung-Konfigurationsdatei ("App.config" oder "Web.config", die vom Projekttyp abhängig.) Sie geben die Kategorien der Ereignisse protokolliert werden soll. In der Konfigurationsdatei Sie auch angeben, ob die Ereignisse zu protokollieren, um eine Textdatei, die Windows-Ereignisprotokoll oder ein benutzerdefiniertes Protokoll, das mit einer Implementierung der [TraceListener](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracelistener(v=vs.110).aspx).

Die Ereigniskategorien Server umfassen die folgenden Arten von Nachrichten:

| Quelle | Mitteilungen |
| --- | --- |
| SignalR.SqlMessageBus | Anbieter-Setup für SQL-Nachrichtenbus mit horizontaler Skalierung, Datenbankvorgang, Fehler und Timeout-Ereignisse |
| SignalR.ServiceBusMessageBus | Servicebus-Warteschlangen für horizontale Skalierung Anbieter Topic erstellen und Abonnements, Fehler und messagingereignisse |
| SignalR.RedisMessageBus | Redis-Verbindung trennen und Fehler Anbieterereignisse des mit horizontaler Skalierung |
| SignalR.ScaleoutMessageBus | Messagingereignisse mit horizontaler Skalierung |
| SignalR.Transports.WebSocketTransport | WebSocket-Transport-Verbindung trennen, messaging und Fehler-Ereignisse |
| SignalR.Transports.ServerSentEventsTransport | ServerSentEvents transport Verbindung, trennen, messaging und Fehlerereignisse |
| SignalR.Transports.ForeverFrameTransport | ForeverFrame Transport-Verbindung trennen, messaging und Fehler-Ereignisse |
| SignalR.Transports.LongPollingTransport | LongPolling Transport-Verbindung trennen, messaging und Fehler-Ereignisse |
| SignalR.Transports.TransportHeartBeat | Übertragen Sie die Verbindung trennen und Keepalive-Ereignisse |
| SignalR.ReflectedHubDescriptorProvider | Hub-Ermittlungsereignisse |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a>Protokollierung Serverereignisse in Textdateien

Der folgende Code zeigt, wie Sie zum Aktivieren der Ablaufverfolgung für jede Kategorie des Ereignisses. In diesem Beispiel konfiguriert die Anwendung zum Protokollieren von Ereignissen in Textdateien.

**XML-Server-Code für das Aktivieren der Ablaufverfolgung**

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

Im obigen Code die `SignalRSwitch` Eintrag gibt an, die [TraceLevel](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracelevel(v=vs.110).aspx) für Ereignisse, die gesendet, um das angegebene Protokoll verwendet. In diesem Fall auf festgelegt `Verbose` also alle Debug- und Ablaufverfolgungsmeldungen werden protokolliert.

Die folgende Ausgabe zeigt Einträge aus der `transports.log.txt` -Datei für eine Anwendung mithilfe der oben genannten Konfigurationsdatei. Es zeigt eine neue Verbindung, einer entfernten Verbindungs- und frequenzermittlungsereignisse Transport.

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a>Serverereignisse in das Ereignisprotokoll zu protokollieren.

Um die Ereignisse im Ereignisprotokoll anstelle einer Textdatei protokollieren möchten, ändern Sie die Werte für die Einträge in der `sharedListeners` Knoten. Der folgende Code zeigt, wie Sie Serverereignisse in das Ereignisprotokoll zu protokollieren:

**XML-Servercode zum Protokollieren von Ereignissen im Ereignisprotokoll**

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

Die Ereignisse werden im Windows-Anwendungsprotokoll protokolliert und sind über die Ereignisanzeige zur Verfügung, wie unten dargestellt:

![Ereignisanzeige mit SignalR-Protokolle](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> Wenn Sie das Ereignisprotokoll verwenden, legen Sie die **TraceLevel** auf **Fehler** auf die Anzahl der Nachrichten überschaubar zu halten.

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a>Aktivieren der Ablaufverfolgung in der .NET Client (Windows-Desktop-apps)

Der .NET Client kann Ereignisse protokollieren, an die Konsole, eine Textdatei oder in ein benutzerdefiniertes Protokoll, das mit einer Implementierung der ["TextWriter"](https://msdn.microsoft.com/en-us/library/system.io.textwriter.aspx).

Legen Sie zum Aktivieren der Protokollierung in der .NET Client der Verbindungs `TraceLevel` Eigenschaft, um eine [TraceLevels](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) Wert, und die `TraceWriter` -Eigenschaft eine gültige ["TextWriter"](https://msdn.microsoft.com/en-us/library/system.io.textwriter.aspx) Instanz.

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a>Protokollieren von Ereignissen an die Konsole Desktopclient

Der folgende C#-Code zeigt, wie Ereignisse in der .NET Client an die Konsole zu protokollieren:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a>Protokollieren von Ereignissen in eine Textdatei Desktopclient

Der folgende C#-Code zeigt, wie Ereignisse in der .NET Client in eine Textdatei zu protokollieren:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

Die folgende Ausgabe zeigt Einträge aus der `ClientLog.txt` -Datei für eine Anwendung mithilfe der oben genannten Konfigurationsdatei. Es zeigt, dass des Clients eine Verbindung mit dem Server und der Hub Aufrufen einer Clientmethode wird aufgerufen, `addMessage`:

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a>Aktivieren der Ablaufverfolgung in Windows Phone 8-clients

SignalR-Anwendungen für Windows Phone-apps verwenden den gleichen .NET Client als desktop-apps, aber [Console.Out](https://msdn.microsoft.com/en-us/library/system.console.out(v=vs.110).aspx) der und Schreiben in eine Datei mit [StreamWriter](https://msdn.microsoft.com/en-us/library/system.io.streamwriter(v=vs.110).aspx) sind nicht verfügbar. Stattdessen müssen Sie erstellen eine benutzerdefinierte Implementierung von ["TextWriter"](https://msdn.microsoft.com/en-us/library/system.io.textwriter(v=vs.110).aspx) für die Ablaufverfolgung. 

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a>Windows Phone-Client-Ereignisse zu protokollieren, auf die Benutzeroberfläche

Die [SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip) enthält ein Beispiel für Windows Phone, die Ablaufverfolgungsausgabe an schreibt eine [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) mithilfe einer benutzerdefinierten ["TextWriter"](https://msdn.microsoft.com/en-us/library/system.io.textwriter(v=vs.110).aspx) -Implementierung mit der Bezeichnung `TextBlockWriter`. Diese Klasse finden Sie in der **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** Projekt. Beim Erstellen einer Instanz von `TextBlockWriter`, übergeben Sie in der aktuellen [SynchronizationContext](https://msdn.microsoft.com/en-us/library/system.threading.synchronizationcontext(v=vs.110).aspx), und ein [StackPanel](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) , in dem er erstellt eine [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) für Ablaufverfolgung Ausgabe:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

Die Trace-Ausgabe wird dann in ein neues geschrieben werden [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) erstellt, der [StackPanel](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) im übergeben:

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a>Windows Phone-Client-Ereignisse zu protokollieren, auf der Debugkonsole

Zum Senden der Ausgabe auf der Debugkonsole statt der Benutzeroberfläche, erstellen Sie eine Implementierung von ["TextWriter"](https://msdn.microsoft.com/en-us/library/system.io.textwriter(v=vs.110).aspx) , die schreibt in das Debug-Fenster, und weisen Sie sie für Ihre Verbindungs [TraceWriter](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) Eigenschaft:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

Ablaufverfolgungsinformationen werden dann in die Debugfenster in Visual Studio geschrieben:

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a>Aktivieren der Ablaufverfolgung in der JavaScript-client

Um die clientseitige Protokollierung für eine Verbindung zu aktivieren, legen Sie die `logging` Eigenschaft für das Verbindungsobjekt vor dem Aufrufen der `start` Methode zum Herstellen der Verbindung.

**Client-JavaScript-Code für das Aktivieren der Ablaufverfolgung in die Browserkonsole "(mit den generierten Proxy)**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

**Client-JavaScript-Code für das Aktivieren der Ablaufverfolgung in die Browserkonsole "(ohne den generierten Proxy)**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

Wenn Ablaufverfolgung aktiviert ist, protokolliert der JavaScript-Client Ereignisse in der Browserkonsole. Für den Zugriff auf die Browserkonsole finden Sie unter [Überwachung Transporte](../getting-started/introduction-to-signalr.md#MonitoringTransports).

Der folgende Screenshot zeigt einen SignalR JavaScript-Client, ohne die Ablaufverfolgung aktiviert. Sie zeigt Verbindungs- und Hub-Aufruf-Ereignisse in der Browserkonsole:

![SignalR-Ablaufverfolgungsereignisse in der Browserkonsole.](enabling-signalr-tracing/_static/image4.png)
