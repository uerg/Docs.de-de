---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-net-client
title: "ASP.NET SignalR-Hubs-API-Guide – .NET Client (c#) | Microsoft Docs"
author: pfletcher
description: "Dieses Dokument enthält eine Einführung zur Verwendung der API-Hubs für SignalR Version 2 in .NET Clients, z. B. Windows Store (WinRT), WPF, Silverlight und Nachteile..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 6d02d9f7-94e5-4140-9f51-5a6040f274f6
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: a03c8c42622a768d706acf5ac1f23b37a830d426
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-signalr-hubs-api-guide---net-client-c"></a>ASP.NET SignalR-Hubs-API-Guide – .NET Client (c#)
====================
durch [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

> Dieses Dokument enthält eine Einführung zur Verwendung der API-Hubs für SignalR Version 2 in .NET Clients, z. B. Windows Store (WinRT), WPF, Silverlight und konsolenanwendungen.
> 
> Die SignalR-Hubs-API können Sie von Remoteprozeduraufrufen (RPCs) von einem Server verbundenen Clients und von den Clients an den Server vornehmen. Im Servercode definieren Sie Methoden, die von Clients aufgerufen werden können, und rufen Sie die Methoden, die auf dem Client ausgeführt. Im Clientcode definieren Sie Methoden, die vom Server aufgerufen werden kann, und rufen Sie die Methoden, die auf dem Server ausgeführt. SignalR ist aller von der Client-zu-Server-Aufgaben, die für Sie übernimmt.
> 
> SignalR bietet außerdem eine technisch anspruchsvolle-API, die dauerhafte Verbindungen aufgerufen. Eine Einführung in die SignalR-Hubs und dauerhafte Verbindungen oder ein Lernprogramm, das zeigt, wie Sie eine vollständige SignalR-Anwendung erstellen, finden Sie in [SignalR - erste Schritte](../getting-started/index.md).
> 
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


## <a name="overview"></a>Übersicht

Dieses Dokument enthält folgende Abschnitte:

- [Client-Setup](#clientsetup)
- [Gewusst wie: Herstellen einer Verbindung](#establishconnection)

    - [Domänenübergreifende Verbindungen von Silverlight-clients](#slcrossdomain)
- [Gewusst wie: Konfigurieren der Verbindung](#configureconnection)

    - [Festlegen der maximalen Anzahl gleichzeitiger Verbindungen in WPF-clients](#maxconnections)
    - [Gewusst wie: Angeben von Abfragezeichenfolgen-Parameter](#querystring)
    - [Zum Angeben der Transportmethode](#transport)
    - [Zum Angeben der HTTP-Header](#httpheaders)
    - [Gewusst wie: Geben Sie Clientzertifikate](#clientcertificate)
- [Vorgehensweise: Erstellen Sie die hubproxy](#proxy)
- [Zum Definieren von Methoden auf dem Client, die der Server aufrufen können](#callclient)

    - [Methoden ohne Parameter](#clientmethodswithoutparms)
    - [Methoden mit Parametern, Parametertypen angeben](#clientmethodswithparmtypes)
    - [Methoden mit Parametern, dynamische Objekte für die Parameter angeben](#clientmethodswithdynamparms)
    - [Gewusst wie: einen Handler zu entfernen](#removehandler)
- [Gewusst wie: Aufrufen von Servermethoden vom client](#callserver)
- [Behandeln von Lebensdauer Verbindungsereignisse](#connectionlifetime)
- [Gewusst wie: Behandeln von Fehlern](#handleerrors)
- [Clientseitige Protokollierung aktivieren](#logging)
- [WPF und Silverlight Konsolenanwendung Codebeispiele für Clientmethoden, die der Server aufrufen können](#wpfsl)

Ein Beispiel .NET Client-Projekten finden Sie unter den folgenden Ressourcen:

- [Gustavo Armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) auf "github.com" (WinRT, Silverlight, Konsole app-Beispiele).
- [DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) auf "github.com" (WPF-Beispiel).
- [SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) auf "github.com" (Konsole-app-Beispiel).

Dokumentation über die Programmierung des Servers oder JavaScript-Clients finden Sie in den folgenden Ressourcen:

- [SignalR-Hubs-API-Leitfaden – Server](hubs-api-guide-server.md)
- [SignalR-Hubs-API-Handbuch - JavaScript-Client](hubs-api-guide-javascript-client.md)

Links zu API-Referenzthemen sind auf die .NET 4.5-Version der API. Wenn Sie .NET 4 verwenden, finden Sie unter [.NET 4-Version der API-Themen](https://msdn.microsoft.com/en-us/library/jj891075(v=vs.100).aspx).

<a id="clientsetup"></a>

## <a name="client-setup"></a>Client-setup

Installieren der [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet-Paket (nicht die [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) Paket). Dieses Paket unterstützt WinRT, Silverlight, WPF, Konsolenanwendung und Windows Phone-Clients für .NET 4 und .NET 4.5.

Wenn die Version des SignalR, die Sie auf dem Client müssen von der Version, die Sie auf dem Server verfügen unterscheidet, kann SignalR häufig der Differenz angepasst werden kann. Ein Server mit SignalR Version 2 unterstützt z. B. Clients, die 1.1.x installiert haben, sowie Clients, die Version 2 installiert haben. Wenn der Unterschied zwischen der Version auf dem Server und die Version auf dem Client zu groß ist oder SignalR löst aus, wenn der Client neuer als der Server befindet, eine `InvalidOperationException` -Ausnahme aus, wenn der Client versucht, eine Verbindung herzustellen. Die Fehlermeldung lautet "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Gewusst wie: Herstellen einer Verbindung

Bevor Sie eine Verbindung herstellen können, müssen Sie erstellen eine `HubConnection` Objekt, und erstellen Sie ein Proxykonto. Rufen Sie zum Herstellen einer Verbindung, die `Start` Methode für die `HubConnection` Objekt.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> Bei JavaScript-Clients müssen Sie mindestens ein Ereignishandler vor dem Aufruf registriert die `Start` Methode zum Herstellen der Verbindung. Dies ist nicht für .NET-Clients erforderlich. Für JavaScript-Clients, erstellt des generierte Proxycodes Proxys für sämtliche Hubs, die vorhanden sind automatisch auf dem Server und registrieren einen Ereignishandler ist wie die Hubs angeben möchte, dass der Client verwenden. Aber für einen Client .NET Hub Proxys manuell erstellt, damit SignalR wird davon ausgegangen, dass Sie alle Hub verwendet werden, die Sie einen Proxy für erstellen.


Im Beispielcode wird die Standardeinstellung "/ Signalr" die URL für die Verbindung mit dem SignalR-Dienst. Informationen über das Angeben einer anderen Basis-URL finden Sie unter [ASP.NET SignalR-Hubs API-Handbuch - Server - URL /signalr](hubs-api-guide-server.md#signalrurl).

Die `Start` Methode asynchron ausgeführt wird. Um sicherzustellen, dass nachfolgende Codezeilen nicht erst ausführen, nachdem die Verbindung hergestellt wird, verwenden Sie `await` in einer asynchronen Methode von ASP.NET 4.5 oder `.Wait()` in eine synchrone Methode. Verwenden Sie keine `.Wait()` in einem WinRT-Client.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a>Domänenübergreifende Verbindungen von Silverlight-clients

Informationen zum Aktivieren der domänenübergreifende Verbindungen von Silverlight-Clients finden Sie unter [machen einen Service Available Across Domain Boundaries](https://msdn.microsoft.com/en-us/library/cc197955(v=vs.95).aspx).

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Gewusst wie: Konfigurieren der Verbindung

Bevor Sie eine Verbindung herstellen, können Sie eine der folgenden Optionen angeben:

- Gleichzeitige Verbindungen einschränken.
- Abfragezeichenfolgenparameter.
- Die Transportmethode.
- HTTP-Header.
- Clientzertifikate.

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a>Festlegen der maximalen Anzahl gleichzeitiger Verbindungen in WPF-clients

In WPF-Clients müssen Sie möglicherweise die maximale Anzahl gleichzeitiger Verbindungen von seinem Standardwert 2 erhöhen. Der empfohlene Wert ist 10.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

Weitere Informationen finden Sie unter [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/en-us/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Gewusst wie: Angeben von Abfragezeichenfolgen-Parameter

Wenn Sie möchten Daten an den Server senden, wenn der Client eine Verbindung herstellt, können Sie Abfragezeichenfolgen-Parameter an das Verbindungsobjekt hinzufügen. Im folgende Beispiel wird gezeigt, wie einen Abfragezeichenfolgen-Parameter im Clientcode festgelegt wird.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample5.cs)]

Im folgende Beispiel wird gezeigt, wie einen Abfragezeichenfolgenparameter in Servercode gelesen wird.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Zum Angeben der Transportmethode

Im Rahmen des Prozesses eine Verbindung herstellen verhandelt SignalR-Client normalerweise mit dem Server zu den bewährten Transport ermitteln kann, der unterstützt wird, indem sowohl Server-als auch. Wenn Sie bereits wissen, dass der Transportmethode verwenden möchten, können Sie diese Aushandlung-Prozess umgehen. Zum Angeben der Transportmethode übergeben Sie einen Transport-Objekt an die Start-Methode. Das folgende Beispiel zeigt, wie die Transportmethode im Clientcode an.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

Die [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/en-us/library/jj918090(v=vs.111).aspx) Namespace enthält die folgenden Klassen, die Sie verwenden können, um die Übertragung angegeben.

- [LongPollingTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)
- [ServerSentEventsTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)
- [WebSocketTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (nur verfügbar, wenn sowohl Server-als auch .NET 4.5 verwenden.)
- [AutoTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (wählt automatisch den besten Transport, der vom Client und dem Server unterstützt wird. Dies ist der Standardtransport. Übergeben dies in der `Start` Methode hat denselben Effekt wie die Elemente nicht erfolgreich abgeschlossen.)

Der Transport ForeverFrame ist nicht in dieser Liste enthalten, da er nur von Browsern verwendet wird.

Informationen zum Überprüfen der Transportmethode in Servercode finden Sie unter [Handbuch für ASP.NET SignalR-Hubs-API - Server - zum Abrufen von Informationen über den Client aus der Kontexteigenschaft](hubs-api-guide-server.md#contextproperty). Weitere Informationen zu Transporten und Zugriffe, finden Sie unter [Einführung in die SignalR - Transporte und Zugriffe](../getting-started/introduction-to-signalr.md#transports).

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a>Zum Angeben der HTTP-Header

Verwenden Sie zum Festlegen von HTTP-Header der `Headers` Eigenschaft für das Verbindungsobjekt. Im folgende Beispiel wird gezeigt, wie einen HTTP-Header hinzugefügt wird.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a>Gewusst wie: Geben Sie Clientzertifikate

Verwenden Sie zum Hinzufügen von Clientzertifikaten die `AddClientCertificate` Methode für das Verbindungsobjekt.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a>Vorgehensweise: Erstellen Sie die hubproxy

Um Methoden auf dem Client zu definieren, die ein Hub vom Server aufgerufen werden kann, und zum Aufrufen von Methoden für einen Hub auf dem Server, einen Proxy für den Hub erstellen, durch den Aufruf `CreateHubProxy` für das Verbindungsobjekt. Die Zeichenfolge übergebenen `CreateHubProxy` ist der Name der Klasse Hub oder indem der angegebene Name der `HubName` Attribut, wenn eine auf dem Server verwendet wurde. Die namenszuordnung wird Groß-/Kleinschreibung.

**Hubklasse auf server**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

**Erstellen des Clientproxys für den Hub-Klasse**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

Wenn Sie ergänzen die Hub-Klasse mit einem `HubName` -Attribut angegeben wird, verwenden Sie diesen Namen.

**Hubklasse auf server**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample12.cs)]

**Erstellen des Clientproxys für den Hub-Klasse**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

Beim Aufrufen `HubConnection.CreateHubProxy` mehrmals mit dem gleichen `hubName`, erhalten Sie dieselbe zwischengespeichert `IHubProxy` Objekt.

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Zum Definieren von Methoden auf dem Client, die der Server aufrufen können

Um eine Methode definieren, die der Server aufrufen können, verwenden Sie des Proxys `On` Methode, um einen Ereignishandler zu registrieren.

Methode-namenszuordnung wird Groß-/Kleinschreibung. Beispielsweise `Clients.All.UpdateStockPrice` auf dem Server führt `updateStockPrice`, `updatestockprice`, oder `UpdateStockPrice` auf dem Client.

Andere Client-Plattformen haben unterschiedliche Anforderungen an wie Sie den Code schreiben, die Benutzeroberfläche zu aktualisieren. Die dargestellten Beispiele gelten für WinRT (.NET für Windows Store)-Clients. WPF und Silverlight sowie Konsole Anwendungsbeispiele dienen [einem separaten Abschnitt weiter unten in diesem Thema](#wpfsl).

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a>Methoden ohne Parameter

Wenn die Methode, die Sie behandeln können nicht über Parameter verfügt, verwenden Sie die nicht generische Überladung von der `On` Methode:

**Servercode Clientmethode ohne Parameter aufrufen**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

**WinRT-Clientcode für die Methode, die vom Server ohne Parameter aufgerufen ([finden Sie in WPF und Silverlight-Beispielen weiter unten in diesem Thema](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Methoden mit Parametern, die Parametertypen angeben

Wenn die Methode, die Sie behandeln können Parameter verfügt, geben Sie die Typen der Parameter als generischen Typen von der `On` Methode. Es gibt generische Überladung von der `On` Methode zum Aktivieren von bis zu 8 Parameter (4 unter Windows Phone 7) angeben. Im folgenden Beispiel einen Parameter gesendet wird, zu der `UpdateStockPrice` Methode.

**Servercode-Methode mit einem Parameter aufrufen**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

**Der Stock-Klasse, die für den Parameter verwendet**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample17.cs)]

**WinRT-Clientcode für eine Methode aufgerufen wird, vom Server mit einem Parameter ([finden Sie in WPF und Silverlight-Beispielen weiter unten in diesem Thema](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Methoden mit Parametern, dynamische Objekte für die Parameter angeben

Als Alternative zum Angeben von Parametern als generischen Typen von der `On` -Methode, können Sie Parameter als dynamische Objekte angeben:

**Servercode-Methode mit einem Parameter aufrufen**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

**Der Stock-Klasse, die für den Parameter verwendet**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample20.cs)]

**WinRT-Clientcode für eine Methode aufgerufen wird, vom Server mit einem Parameter, verwenden ein dynamisches Objekt für den Parameter ([finden Sie in WPF und Silverlight-Beispielen weiter unten in diesem Thema](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a>Gewusst wie: einen Handler zu entfernen

Um einen Handler zu entfernen, rufen Sie die `Dispose` Methode.

**Clientcode für einen vom Server aufgerufene Methode**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

**Client-Code, um den Ereignishandler zu entfernen**

[!code-css[Main](hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Gewusst wie: Aufrufen von Servermethoden vom client

Verwenden Sie zum Aufrufen einer Methode auf dem Server die `Invoke` Methode für die hubproxy.

Verfügt die Server-Methode keinen Wert zurückgibt, verwenden Sie die nicht generische Überladung von der `Invoke` Methode.

**Servercode für eine Methode, die keinen zurückgibt Wert**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

**Clientcode Aufrufen einer Methode, die keinen zurückgibt Wert**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

Wenn die Server-Methode einen Rückgabewert verfügt, den Rückgabetyp angeben, wie der generische Typ, der die `Invoke` Methode.

**Servercode für eine Methode, die einen Wert zurückgibt und nimmt den Parameter ein komplexer Typ**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

**Der Stock-Klasse, die für den Parameter und Rückgabewert verwendet**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample27.cs)]

**Clientcode Aufrufen einer Methode, die einen Wert zurückgibt und akzeptiert einen Parameter mit komplexen Typ in einer ASP.NET 4.5 Async-Methode**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

**Aufrufen einer Methode, die einen Wert zurückgibt und nimmt einen Parameter mit komplexen Typ in eine synchrone Methode Clientcode**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

Die `Invoke` Methode asynchron ausgeführt wird, und gibt eine `Task` Objekt. Wenn Sie keinen `await` oder `.Wait()`, die nächste Codezeile ausgeführt, bevor die Methode, die Sie aufrufen Ausführung beendet wurde.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Behandeln von Lebensdauer Verbindungsereignisse

SignalR bietet die folgenden Verbindung Lebensdauerereignisse, die Sie behandeln können:

- `Received`: Wird ausgelöst, wenn keine Daten für die Verbindung empfangen werden. Stellt die empfangenen Daten.
- `ConnectionSlow`: Wird ausgelöst, wenn der Client eine Verbindung langsame oder häufig löschen erkennt.
- `Reconnecting`: Wird ausgelöst, wenn die zugrunde liegenden Transport erneut zu verbinden beginnt.
- `Reconnected`: Wird ausgelöst, wenn die zugrunde liegenden Transport die Verbindung wiederhergestellt wurde.
- `StateChanged`: Wird ausgelöst, wenn der Verbindungsstatus ändert. Enthält der alte Status und den Zustand "Neu". Informationen zur Verbindung Zustandswerte finden Sie unter [ConnectionState-Enumeration](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).
- `Closed`: Wird ausgelöst, wenn die Verbindung getrennt wurde.

Z. B. Wenn Sie Warnmeldungen für Fehler anzeigen, die nicht schwerwiegend sind, aber dazu führen, dass vorübergehende Verbindungsprobleme möchten, wie dies darauf oder häufige Löschen der Verbindung, behandeln die `ConnectionSlow` Ereignis.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample30.cs)]

Weitere Informationen finden Sie unter [verstehen und Behandeln von Ereignissen für Lebensdauer in SignalR](handling-connection-lifetime-events.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Gewusst wie: Behandeln von Fehlern

Wenn Sie ausführliche Fehlermeldungen auf dem Server nicht explizit aktivieren, enthält das Ausnahmeobjekt, das SignalR nach einem Fehler gibt wenige Informationen über den Fehler. Angenommen, wenn ein Aufruf von `newContosoChatMessage` ein Fehler auftritt, der die Fehlermeldung in die Error-Objekt enthält "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" senden, detaillierte Fehlermeldungen für Clients in einer produktionsumgebung wird nicht empfohlen aus Sicherheitsgründen aber wenn Sie möchten detaillierte Fehlermeldungen für aktivieren Problembehandlung, verwenden Sie folgenden Code auf dem Server.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

Zum Behandeln von Fehlern, die SignalR auslöst, können Sie hinzufügen, einen Handler für das `Error` Ereignis für das Verbindungsobjekt.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample32.cs)]

Umschließen Sie den Code in einem Try / Catch-Block, um Fehler von Methodenaufrufen zu behandeln.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Clientseitige Protokollierung aktivieren

Um die clientseitige Protokollierung zu aktivieren, legen die `TraceLevel` und `TraceWriter` Eigenschaften für das Verbindungsobjekt.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a>WPF und Silverlight Konsolenanwendung Codebeispiele für Clientmethoden, die der Server aufrufen können

Die Codebeispiele, die weiter oben dargestellten zum Definieren von Clientmethoden, die der Server aufrufen können, gelten für WinRT-Clients. Den folgenden Beispielen wird den entsprechenden Code für WPF und Silverlight webanwendungsclients Konsole.

### <a name="methods-without-parameters"></a>Methoden ohne Parameter

**WPF-Clientcode für die Methode, die vom Server ohne Parameter aufgerufen**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

**Silverlight-Client-Code für die Methode, die vom Server ohne Parameter aufgerufen**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

**Client-Anwendungscode für die Methode, die vom Server ohne Parameter aufgerufen-Konsole**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Methoden mit Parametern, die Parametertypen angeben

**WPF-Clientcode für eine Methode vom Server mit einem Parameter namens**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

**Silverlight-Client-Code für eine Methode aufgerufen wird, vom Server mit einem parameter**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

**Client-Anwendungscode für eine Methode vom Server mit einem Parameter namens-Konsole**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Methoden mit Parametern, dynamische Objekte für die Parameter angeben

**WPF-Clientcode für eine Methode vom Server mit einem Parameter, verwenden für den Parameter ein dynamisches Objekt namens**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

**Silverlight-Client-Code für eine Methode aufgerufen wird, vom Server mit einem Parameter, verwenden ein dynamisches Objekt für den parameter**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

**Client-Anwendungscode für eine Methode aufgerufen wird, vom Server mit einem Parameter, verwenden ein dynamisches Objekt für den Parameter-Konsole**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
