---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-server
title: ASP.NET SignalR-Hubs-API-Handbuch - Server (c#) | Microsoft Docs
author: pfletcher
description: Dieses Dokument enthält eine Einführung in die Serverseite der ASP.NET SignalR-Hubs-API für SignalR Version 2, mit Codebeispiele zur Programmierung...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: b19913e5-cd8a-4e4b-a872-5ac7a858a934
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: c2567d4d39a494daf77a23db5dff83c8fae4925d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
ms.locfileid: "28039208"
---
<a name="aspnet-signalr-hubs-api-guide---server-c"></a>ASP.NET SignalR-Hubs-API-Handbuch - Server (c#)
====================
durch [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

> Dieses Dokument enthält eine Einführung in die Programmierung von der Serverseite der ASP.NET SignalR-Hubs-API für SignalR Version 2, mit Codebeispiele zur allgemeine Optionen.
> 
> Die SignalR-Hubs-API können Sie von Remoteprozeduraufrufen (RPCs) von einem Server verbundenen Clients und von den Clients an den Server vornehmen. Im Servercode definieren Sie Methoden, die von Clients aufgerufen werden können, und rufen Sie die Methoden, die auf dem Client ausgeführt. Im Clientcode definieren Sie Methoden, die vom Server aufgerufen werden kann, und rufen Sie die Methoden, die auf dem Server ausgeführt. SignalR ist aller von der Client-zu-Server-Aufgaben, die für Sie übernimmt.
> 
> SignalR bietet außerdem eine technisch anspruchsvolle-API, die dauerhafte Verbindungen aufgerufen. Eine Einführung in SignalR-Hubs und dauerhafte Verbindungen erhalten finden Sie unter [Einführung in SignalR 2](../getting-started/introduction-to-signalr.md).
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
> ## <a name="topic-versions"></a>Thema-Versionen
> 
> Informationen zu früheren Versionen von SignalR finden Sie unter [ältere Versionen von SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Fragen und Kommentare
> 
> Lassen Sie Sie Feedback auf wie in diesem Lernprogramm mögen und was wir in den Kommentaren am unteren Rand der Seite verbessern können. Wenn Sie Fragen, die nicht direkt mit dem Lernprogramm verknüpft sind haben, bereitstellen können, die [ASP.NET SignalR-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Übersicht

Dieses Dokument enthält folgende Abschnitte:

- [Vorgehensweise: Registrieren von SignalR-middleware](#route)

    - [Die /signalr-URL](#signalrurl)
    - [Konfigurieren von SignalR-Optionen](#options)
- [Das Erstellen und Verwenden der Hub-Klassen](#hubclass)

    - [Lebensdauer eines Objekts "Hub"](#transience)
    - [In Kamel-Schreibweise des Hub-Namen in der JavaScript-clients](#hubnames)
    - [Mehrere Hubs](#multiplehubs)
    - [Stark typisierte Hubs](#stronglytypedhubs)
- [Zum Definieren von Methoden in der Hub-Klasse, die Clients aufrufen können](#hubmethods)

    - [Binnenmajuskel Methodennamen in JavaScript-clients](#methodnames)
    - [Beim asynchron ausführen.](#asyncmethods)
    - [Definieren von Überladungen](#overloads)
    - [Statusmeldung von hubmethodenaufrufe](#progress)
- [Gewusst wie: Client Methoden aufrufen, von der Hub-Klasse](#callfromhub)

    - [Auswählen von welchen Clients erhalten die RPC](#selectingclients)
    - [Keine Validierung während der Kompilierung für Methodennamen](#dynamicmethodnames)
    - [Namensübereinstimmung Groß-/Kleinschreibung-Methode](#caseinsensitive)
    - [Asynchrone Ausführung](#asyncclient)
- [Zum Verwalten von Gruppenmitgliedschaften aus der Hub-Klasse](#groupsfromhub)

    - [Asynchrone Ausführung der Add- und Remove-Methoden](#asyncgroupmethods)
    - [Gruppenmitgliedschaft Persistenz](#grouppersistence)
    - [Einzelbenutzer-Gruppen](#singleusergroups)
- [Behandeln von Lebensdauer Verbindungsereignisse in der Hub-Klasse](#connectionlifetime)

    - [Wenn OnConnected OnDisconnected und OnReconnected aufgerufen werden](#onreconnected)
    - [Aufruferstatus nicht aufgefüllt](#nocallerstate)
- [Gewusst wie: Abrufen von Informationen über den Client aus der Kontexteigenschaft](#contextproperty)
- [Gewusst wie: Zustand zwischen Clients und der Hub-Klasse übergeben.](#passstate)
- [Gewusst wie: Behandeln von Fehlern in der Hub-Klasse](#handleErrors)
- [Das Client-Methoden aufrufen und Verwalten von Gruppen von außerhalb der Hub-Klasse](#callfromoutsidehub)

    - [Aufrufen von Clientmethoden](#callingclientsoutsidehub)
    - [Verwalten von Gruppenmitgliedschaften](#managinggroupsoutsidehub)
- [Gewusst wie: Aktivieren der Ablaufverfolgung](#tracing)
- [Gewusst wie: Anpassen die Pipeline Hubs](#hubpipeline)

Dokumentation zum Programm Clients finden Sie in den folgenden Ressourcen:

- [SignalR-Hubs-API-Handbuch - JavaScript-Client](hubs-api-guide-javascript-client.md)
- [SignalR-Hubs-API-Leitfaden – .NET Client](hubs-api-guide-net-client.md)

Die Serverkomponenten für SignalR 2 sind nur in .NET 4.5 verfügbar. Server, die unter .NET 4.0 verwenden, müssen SignalR v1.x.

<a id="route"></a>

## <a name="how-to-register-signalr-middleware"></a>Vorgehensweise: Registrieren von SignalR-middleware

Aufrufen, um die Route definieren, die Clients zur Verbindung mit Ihren Hubs verwendet wird, die `MapSignalR` Methode, wenn die Anwendung gestartet wird. `MapSignalR`ist ein [Erweiterungsmethode](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) für die `OwinExtensions` Klasse. Im folgende Beispiel wird gezeigt, wie die SignalR-Hubs-Route mithilfe einer OWIN-Start-Klasse definiert.

[!code-csharp[Main](hubs-api-guide-server/samples/sample1.cs)]

Wenn Sie SignalR-Funktionen zu einer ASP.NET MVC-Anwendung hinzugefügt werden, stellen Sie sicher, dass die SignalR-Route, bevor die anderen Routen hinzugefügt wird. Weitere Informationen finden Sie unter [Lernprogramm: Erste Schritte mit SignalR-2 und MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a>Die /signalr-URL

Standardmäßig ist die Routen-URL, die von Clients verwendet werden, für die Verbindung mit Ihrem Hub "/ Signalr". (Verwechseln Sie nicht diese URL durch die URL "/ Signalr/Hubs", der für die automatisch generierte JavaScript-Datei ist. Weitere Informationen zu den generierten Proxy, finden Sie unter [SignalR-Hubs für API-Handbuch - JavaScript-Client - generierte Proxy und wozu Sie](hubs-api-guide-javascript-client.md#genproxy).)

Es gibt möglicherweise Ausnahmesituationen, die diese Basis-URL für SignalR nicht verwendbar machen; Angenommen, Sie verfügen über einen Ordner in Ihrem Projekt mit dem Namen *Signalr* und nicht den Namen ändern möchten. In diesem Fall können Sie die base-URL ändern, wie in den folgenden Beispielen gezeigt (ersetzen Sie "/ Signalr" im Code durch die URL Ihres gewünschten).

**Servercode, der die URL angibt.**

[!code-csharp[Main](hubs-api-guide-server/samples/sample2.cs?highlight=1)]

**JavaScript-Clientcode, der angibt, die URL (mit den generierten Proxy)**

[!code-javascript[Main](hubs-api-guide-server/samples/sample3.js?highlight=1)]

**JavaScript-Clientcode, der angibt, die URL (ohne den generierten Proxy)**

[!code-javascript[Main](hubs-api-guide-server/samples/sample4.js?highlight=1)]

**.NET Clientcode, der die URL angibt.**

[!code-csharp[Main](hubs-api-guide-server/samples/sample5.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a>Konfigurieren von SignalR-Optionen

Der Überladungen der `MapSignalR` Methode ermöglichen es Ihnen, eine benutzerdefinierte URL, ein benutzerdefiniertes Abhängigkeitskonfliktlöser und die folgenden Optionen angeben:

- Domänenübergreifende Aufrufe, die über CORS oder JSONP von Browserclients zu aktivieren.

    In der Regel, wenn der Browser eine Seite aus lädt `http://contoso.com`, die SignalR-Verbindung ist auf der gleichen Domäne `http://contoso.com/signalr`. Wenn die Seite `http://contoso.com` stellt eine Verbindung mit `http://fabrikam.com/signalr`, d. h. eine domänenübergreifende-Verbindung. Domänenübergreifende Verbindungen werden aus Gründen der Sicherheit standardmäßig deaktiviert. Weitere Informationen finden Sie unter [Handbuch für ASP.NET SignalR-Hubs-API - JavaScript-Client - Gewusst wie: Herstellen einer Verbindung domänenübergreifende](hubs-api-guide-javascript-client.md#crossdomain).
- Detaillierte Fehlermeldungen zu aktivieren.

    Wenn Fehler auftreten, ist das Standardverhalten des SignalR an Clients senden eine Benachrichtigung ohne Details, was passiert ist. Ausführliche Fehlerinformationen an Clients gesendet wird in der Produktion nicht empfohlen, da böswillige Benutzer die Informationen in Angriffe auf Ihre Anwendung verwenden werden können. Diese Option können Sie für die Problembehandlung um vorübergehend ausführlichere-Fehlerberichterstattung zu aktivieren.
- Deaktivieren Sie die automatisch generierte Dateien der JavaScript-Proxy.

    Standardmäßig wird eine JavaScript-Datei mit Proxys für den Hub Klassen als Antwort auf die URL "/ Signalr/Hubs" generiert. Wenn Sie nicht die JavaScript-Proxys verwenden möchten oder wenn Sie diese Datei manuell zu generieren und zu einer physischen Datei in Ihre Clients verweisen möchten, können Sie diese Option, um Proxygenerierung zu deaktivieren. Weitere Informationen finden Sie unter [SignalR-Hubs für API-Handbuch - JavaScript-Client - Gewusst wie: erstellen eine physische Datei für SignalR generierten Proxy](hubs-api-guide-javascript-client.md#manualproxy).

Im folgende Beispiel wird gezeigt, wie der SignalR-Verbindungs-URL und an diese Optionen in einem Aufruf der `MapSignalR` Methode. Um eine benutzerdefinierte URL anzugeben, ersetzen Sie "/ Signalr" im Beispiel durch die URL, die Sie verwenden möchten.

[!code-csharp[Main](hubs-api-guide-server/samples/sample6.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a>Das Erstellen und Verwenden der Hub-Klassen

Um einen Hub zu erstellen, erstellen Sie eine Klasse, die abgeleitet [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx). Das folgende Beispiel zeigt eine einfache hubklasse für eine Chat-Anwendung.

[!code-csharp[Main](hubs-api-guide-server/samples/sample7.cs)]

In diesem Beispiel wird ein verbundener Client aufrufen kann die `NewContosoChatMessage` -Methode, und wenn dies der Fall ist, wird die empfangenen Daten für alle verbundenen Clients weitergegeben.

<a id="transience"></a>

### <a name="hub-object-lifetime"></a>Lebensdauer eines Objekts "Hub"

Sie nicht die Hub-Klasse instanziieren oder aus Ihrem eigenen Code auf dem Server ihre Methoden aufrufen; Alle, die Sie von der Pipeline SignalR-Hubs erfolgt. SignalR erstellt eine neue Instanz der Klasse Hub jedes Mal, es muss sich um eine Hub-Vorgang, z. B. wenn ein Client eine Verbindung herstellt, trennt die Verbindung oder bewirkt, dass eine Methode aufrufen, mit dem Server zu behandeln.

Da die Instanzen der Klasse Hub vorübergehend sind, können nicht Sie sie zur Beibehaltung des Zustands in einem Methodenaufruf an den nächsten verwenden. Jedes Mal erhält der Server einem Methodenaufruf von einem Client, eine neue Instanz Ihrer Klasse-Prozesse Hub die Nachricht. Um über mehrere Verbindungen und Methodenaufrufe Zustand beibehalten, verwenden Sie eine andere Methode, z. B. eine Datenbank oder eine statische Variable für den Hub-Klasse oder einer anderen Klasse, die nicht von abgeleitet ist `Hub`. Wenn Sie Daten im Arbeitsspeicher beibehalten, mithilfe einer Methode wie z. B. eine statische Variable in der Hub-Klasse, die Daten verloren, wenn die Anwendungsdomäne wiederverwendet wird.

Wenn Sie möchten Clients aus Ihrem eigenen Code Senden von Nachrichten an, die außerhalb der Hub-Klasse ausgeführt wird, dies nicht möglich, durch die Instanziierung einer Hub-Klasseninstanz, jedoch können Sie dies tun, durch einen Verweis auf das Kontextobjekt SignalR für die Klasse Hub abrufen. Weitere Informationen finden Sie unter [wie Client-Methoden aufrufen und Verwalten von Gruppen von außerhalb der Klasse Hub](#callfromoutsidehub) weiter unten in diesem Thema.

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a>In Kamel-Schreibweise des Hub-Namen in der JavaScript-clients

Standardmäßig finden Sie JavaScript-Clients mit einer Version in Kamel-Schreibweise des Klassennamens für Hubs. SignalR verwendet diese Änderung automatisch, sodass JavaScript-Code den JavaScript-Konventionen entsprechen kann. Im vorherige Beispiel würde, die als bezeichnet `contosoChatHub` im JavaScript-Code.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample8.cs?highlight=1)]

**JavaScript-Client generierte Proxy zu verwenden**

[!code-javascript[Main](hubs-api-guide-server/samples/sample9.js?highlight=1)]

Wenn Sie für Clients verwenden, fügen Sie einen anderen Namen geben möchten die `HubName` Attribut. Bei Verwendung einer `HubName` -Attribut angegeben wird, erfolgt keine Namensänderung in Camel-Case für JavaScript-Clients.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample10.cs?highlight=1)]

**JavaScript-Client generierte Proxy zu verwenden**

[!code-javascript[Main](hubs-api-guide-server/samples/sample11.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a>Mehrere Hubs

Sie können mehrere Hub-Klassen in einer Anwendung definieren. Wenn Sie dies tun, die Verbindung freigegeben, jedoch Gruppen getrennt sind:

- Alle Clients verwenden die gleiche URL zum Herstellen einer SignalR-Verbindung mit Ihrem Dienst ("/ Signalr" oder die benutzerdefinierte URL, wenn Sie eine angegeben), und dass für sämtliche Hubs im aktuellen Verbindung verwendet wird, die vom Dienst definierten.

    Es gibt keine Leistungsunterschiede für mehrere Hubs im Vergleich zu allen Hub-Funktionalität in einer einzelnen Klasse definieren.
- Sämtliche Hubs im aktuellen erhalten die gleiche Informationen der HTTP-Anforderung.

    Da sämtliche Hubs im aktuellen auf die gleiche Verbindung verwenden, ist nur HTTP-Anforderungsinformationen, die der Server ruft an, was in der ursprünglichen HTTP-Anforderung stammt, die die SignalR-Verbindung hergestellt wird. Wenn Sie die verbindungsanforderung verwenden, um Informationen vom Client an den Server zu übergeben, indem Sie eine Abfragezeichenfolge angeben, können nicht Sie verschiedene Abfragezeichenfolgen an andere Hubs bereitstellen. Sämtliche Hubs im aktuellen erhalten die gleiche Informationen.
- Die generierte Datei des JavaScript-Proxys wird Proxys für sämtliche Hubs in einer Datei enthalten.

    Weitere Informationen zu JavaScript-Proxys, finden Sie unter [SignalR-Hubs für API-Handbuch - JavaScript-Client - generierte Proxy und wozu Sie](hubs-api-guide-javascript-client.md#genproxy).
- In den Hubs sind Gruppen definiert.

    Mit dem Namen Gruppen aus, um Teilmengen der verbundenen Clients zu senden, in SignalR beziehen, die Sie definieren können. Gruppen werden separat für jede Hub verwaltet. Beispielsweise würde eine Gruppe namens "Administratoren" enthalten einen Satz von Clients für Ihre `ContosoChatHub` -Klasse, und demselben Gruppennamen würde, beziehen sich auf einen anderen Satz von Clients für Ihre `StockTickerHub` Klasse.

<a id="stronglytypedhubs"></a>
### <a name="strongly-typed-hubs"></a>Stark typisierte Hubs

Eine Schnittstelle für den hubmethoden definieren, die der Client kann Referenz (und aktivieren Sie Intellisense auf den hubmethoden), leiten Sie Ihren Hub aus `Hub<T>` (eingeführt in SignalR 2.1) statt `Hub`:

[!code-csharp[Main](hubs-api-guide-server/samples/sample12.cs)]

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a>Zum Definieren von Methoden in der Hub-Klasse, die Clients aufrufen können

Um eine Methode auf dem Hub verfügbar machen, die vom Client aufgerufen werden soll, deklarieren Sie eine öffentliche Methode, wie in den folgenden Beispielen gezeigt.

[!code-csharp[Main](hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](hubs-api-guide-server/samples/sample14.cs?highlight=3)]

Sie können angeben, ein Rückgabetyp und Parameter, einschließlich Arrays und komplexe Typen, wie in einer C#-Methode. Alle Daten, die Sie in Parametern empfangen oder an den Aufrufer zurückgegeben werden zwischen dem Client und dem Server kommuniziert, mithilfe von JSON und SignalR behandelt automatisch die Bindung von komplexen Objekten und Arrays von Objekten.

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a>Binnenmajuskel Methodennamen in JavaScript-clients

Standardmäßig finden Sie JavaScript-Clients in hubmethoden mithilfe einer Camel-Case-Version des Methodennamens ein. SignalR verwendet diese Änderung automatisch, sodass JavaScript-Code den JavaScript-Konventionen entsprechen kann.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample15.cs?highlight=1)]

**JavaScript-Client generierte Proxy zu verwenden**

[!code-javascript[Main](hubs-api-guide-server/samples/sample16.js?highlight=1)]

Wenn Sie für Clients verwenden, fügen Sie einen anderen Namen geben möchten die `HubMethodName` Attribut.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample17.cs?highlight=1)]

**JavaScript-Client generierte Proxy zu verwenden**

[!code-javascript[Main](hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a>Beim asynchron ausführen.

Wenn die Methode wird werden lang andauernde oder muss funktionieren würde, die warten, z. B. einer Datenbanksuche oder einem Webdienstaufruf umfassen, stellen Sie die Hub-Methode asynchrone, wird durch Zurückgeben einer [Aufgabe](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (anstelle von `void` zurückgeben) oder [ Aufgabe&lt;T&gt; ](https://msdn.microsoft.com/library/dd321424.aspx) Objekt (anstelle von `T` Rückgabetyp). Wenn Sie zurückkehren, eine `Task` Objekt aus der SignalR-Methode wartet der `Task` abgeschlossen ist, und sendet es die entpackte Ergebnis zurück an den Client, damit kein Unterschied besteht in wie der Aufruf der Methode im Client code.

Erstellen einer Hub-Methode wird vermieden, asynchrone die Verbindung blockiert, wenn die WebSocket-Transport verwendet. Wenn eine hubmethode synchron wird und WebSocket als Transport verwendet wird, werden nachfolgende Aufrufe von Methoden auf dem Hub vom gleichen Client blockiert, bis zum Abschluss der Hub-Methode.

Das folgende Beispiel zeigt die gleiche Methode codiert, um eine synchrone Ausführung oder asynchron ausgeführt wird, gefolgt von JavaScript-Clientcode, der zum Aufrufen von entweder Version funktioniert.

**Synchronous**

[!code-csharp[Main](hubs-api-guide-server/samples/sample19.cs)]

**Asynchronous**

[!code-csharp[Main](hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

**JavaScript-Client generierte Proxy zu verwenden**

[!code-javascript[Main](hubs-api-guide-server/samples/sample21.js)]

Weitere Informationen zur Verwendung von asynchronen Methoden in ASP.NET 4.5 finden Sie unter [Verwenden von asynchronen Methoden in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).

<a id="overloads"></a>

### <a name="defining-overloads"></a>Definieren von Überladungen

Wenn Sie die Überladungen für eine Methode definieren möchten, muss die Anzahl von Parametern in jede Überladung sich unterscheiden. Wenn Sie eine Überladung unterscheiden, indem Sie verschiedene Parametertypen angeben, die Hub-Klasse kompiliert, aber der SignalR-Dienst löst eine Ausnahme zur Laufzeit, wenn Clients versuchen, auf eine der Überladungen aufzurufen.

<a id="progress"></a>
### <a name="reporting-progress-from-hub-method-invocations"></a>Statusmeldung von hubmethodenaufrufe

SignalR 2.1 bietet Unterstützung für die [Muster in der fortschrittsberichterstellung](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) in .NET 4.5 eingeführt. Um im Zuge der berichterstellung zu implementieren, definieren eine `IProgress<T>` -Parameter für Ihre hubmethode, die der Client zugreifen kann:

[!code-csharp[Main](hubs-api-guide-server/samples/sample22.cs)]

Beim Schreiben einer Servermethode langer ist mit einem asynchronen Programmierung Muster wie Async / Await anstelle der Hub-Thread blockiert.

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a>Gewusst wie: Client Methoden aufrufen, von der Hub-Klasse

Um Client vom Server Methoden aufzurufen, verwenden Sie die `Clients` Eigenschaft in einer Methode in der Hub-Klasse. Das folgende Beispiel zeigt die Servercode, der Aufrufe `addNewMessageToPage` auf allen verbundenen Clients, und Clientcode, der die Methode in einem JavaScript-Client definiert.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample23.cs?highlight=5)]

**JavaScript-Client generierte Proxy zu verwenden**

[!code-html[Main](hubs-api-guide-server/samples/sample24.html?highlight=1)]

Einen Rückgabewert kann nicht von einer Clientmethode abgerufen werden; Diese Syntax sind `int x = Clients.All.add(1,1)` funktioniert nicht.

Sie können komplexe Typen und -Arrays für die Parameter angeben. Im folgende Beispiel wird einen komplexen Typ an dem Client in einem Methodenparameter übergeben.

**Servercode, der eine Clientmethode, die mithilfe eines komplexen Objekts aufruft**

[!code-csharp[Main](hubs-api-guide-server/samples/sample25.cs?highlight=3)]

**Servercode, die das komplexe Objekt definiert.**

[!code-csharp[Main](hubs-api-guide-server/samples/sample26.cs?highlight=1)]

**JavaScript-Client generierte Proxy zu verwenden**

[!code-javascript[Main](hubs-api-guide-server/samples/sample27.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a>Auswählen von welchen Clients erhalten die RPC

Gibt die Clients-Eigenschaft einer [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) -Objekt, das bietet mehrere Optionen zum angeben, welche Clients die RPC empfangen werden:

- Alle verbundenen Clients.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample28.cs)]
- Nur der aufrufende Client.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample29.cs)]
- Alle Clients mit Ausnahme des aufrufenden Clients.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample30.cs)]
- Um einen spezifischen Client identifizierte Verbindungs-ID.

    [!code-css[Main](hubs-api-guide-server/samples/sample31.css)]

    Dieses Beispiel ruft `addContosoChatMessageToPage` auf dem aufrufenden Client und hat dieselbe Wirkung wie das Verwenden von `Clients.Caller`.
- Alle verbundenen Clients mit Ausnahme der angegebenen Clients identifiziert, die vom Verbindungs-ID.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample32.cs)]
- Alle verbundenen Clients in einer angegebenen Gruppe.

    [!code-css[Main](hubs-api-guide-server/samples/sample33.css)]
- Alle verbundenen Clients in einer angegebenen Gruppe mit Ausnahme der angegebenen Clients identifiziert, die vom Verbindungs-ID.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample34.cs)]
- Alle verbundenen Clients in einer angegebenen Gruppe mit Ausnahme des aufrufenden Clients.

    [!code-css[Main](hubs-api-guide-server/samples/sample35.css)]
- Einen bestimmten Benutzer, durch die Benutzer-ID identifiziert.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample36.cs)]

    Standardmäßig ist dies `IPrincipal.Identity.Name`, aber dies kann geändert werden, indem [registrieren eine Implementierung von IUserIdProvider mit dem globalen Host](mapping-users-to-connections.md#IUserIdProvider).
- Alle Clients und Gruppen in einer Liste von Verbindungs-IDs.

    [!code-css[Main](hubs-api-guide-server/samples/sample37.css)]
- Eine Liste der Gruppen.

    [!code-css[Main](hubs-api-guide-server/samples/sample38.css)]
- Ein Benutzer anhand des Namens.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample39.cs)]
- Eine Liste von Benutzernamen (eingeführt in SignalR 2.1).

    [!code-csharp[Main](hubs-api-guide-server/samples/sample40.cs)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a>Keine Validierung während der Kompilierung für Methodennamen

Der Methodenname, die Sie angeben, wird als ein dynamisches Objekt interpretiert dies bedeutet, dass es gibt keine IntelliSense oder Überprüfung der Kompilierzeit dafür. Der Ausdruck wird zur Laufzeit ausgewertet. Wenn der Methodenaufruf ausgeführt wird, werden SignalR den Methodennamen und die Parameterwerte an den Client gesendet, und wenn der Client eine Methode verfügt, die mit dem Namen übereinstimmt, die-Methode aufgerufen wird und die Parameterwerte zu übergeben. Wenn keine übereinstimmende Methode auf dem Client gefunden wird, wird kein Fehler ausgelöst. Informationen zum Format der Daten, die SignalR überträgt an den Client im Hintergrund auf, wenn Sie eine Clientmethode aufrufen, finden Sie unter [Einführung in SignalR](../getting-started/introduction-to-signalr.md).

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a>Namensübereinstimmung Groß-/Kleinschreibung-Methode

Methode-namenszuordnung wird Groß-/Kleinschreibung. Beispielsweise `Clients.All.addContosoChatMessageToPage` auf dem Server führt `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, oder `addContosoChatMessageToPage` auf dem Client.

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a>Asynchrone Ausführung

Die Methode, die Sie aufrufen führt asynchron aus. Jeglicher Code, der nach dem Aufruf einer Methode an einen Client sofort ausgeführt wird, ohne zu warten, für SignalR Daten an Clients übertragen, es sei denn, die Sie angeben, dass die nachfolgenden Zeilen des Codes, auf den Abschluss der Methode warten soll abgeschlossen ist. Im folgenden Codebeispiel wird gezeigt, wie zwei Clientmethoden sequenziell ausgeführt wird.

**Mit "await" (.NET 4.5)**

[!code-csharp[Main](hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

Bei Verwendung von `await` warten, bis eine Clientmethode abgeschlossen ist, bevor die nächste Zeile des Codes ausgeführt wird, dies bedeutet nicht, dass Clients die Nachricht tatsächlich empfangen werden, bevor die nächste Zeile des Codes ausgeführt wird. "Abschluss", der einen clientmethodenaufruf bedeutet lediglich, dass SignalR alles, was Sie zum Senden der Nachricht durchgeführt hat. Wenn Sie Überprüfung, dass Clients die Nachricht empfangen möchten, müssen Sie diesen Mechanismus selbst programmieren. Sie können z. B. code eine `MessageReceived` -Methode auf dem Hub, und klicken Sie in der `addContosoChatMessageToPage` Methode auf dem Client, Sie rufen `MessageReceived` danach nach Belieben Sie arbeiten, müssen auf dem Client. In `MessageReceived` im Hub erreichen Sie tatsächliche Client-Empfang und die Verarbeitung des ursprünglichen Methodenaufrufs Arbeit abhängt.

### <a name="how-to-use-a-string-variable-as-the-method-name"></a>Verwenden Sie eine Zeichenfolgenvariable als Name der Methode

Wenn Sie eine Clientmethode aufrufen, indem Sie eine Zeichenfolgenvariable verwenden, als der Methodenname, wandeln Sie möchten `Clients.All` (oder `Clients.Others`, `Clients.Caller`usw.) zu `IClientProxy` und rufen Sie anschließend [Invoke (MethodName, Args...) ](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).

[!code-csharp[Main](hubs-api-guide-server/samples/sample42.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a>Zum Verwalten von Gruppenmitgliedschaften aus der Hub-Klasse

Gruppen in SignalR bieten eine Methode zum Übertragen von Nachrichten an den angegebenen Teilmengen von verbundenen Clients. Eine Gruppe kann eine beliebige Anzahl von Clients umfassen, und ein Client kann Mitglied einer beliebigen Anzahl von Gruppen sein.

Verwenden Sie zum Verwalten der Gruppenmitgliedschaft die [hinzufügen](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) und [entfernen](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) bereitgestellten Methoden die `Groups` Eigenschaft der Hub-Klasse. Das folgende Beispiel zeigt die `Groups.Add` und `Groups.Remove` Methoden, die in hubmethoden, die vom Client-Code aufgerufen werden, gefolgt von JavaScript-Clientcode, der sie aufruft.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample43.cs?highlight=5,10)]

**JavaScript-Client generierte Proxy zu verwenden**

[!code-javascript[Main](hubs-api-guide-server/samples/sample44.js)]

[!code-javascript[Main](hubs-api-guide-server/samples/sample45.js)]

Sie müssen nicht explizit Gruppen erstellen. Faktisch ist eine Gruppe automatisch beim ersten Geben Sie den Namen in einem Aufruf erstellt `Groups.Add`, und es wird gelöscht, wenn Sie aus der Mitgliedschaft in der sie in der letzten Verbindung entfernen.

Es ist keine API zum Abrufen der Mitgliederliste einer Gruppe oder eine Liste der Gruppen ein. SignalR sendet Nachrichten an Clients und-Gruppen auf Grundlage einer [und Abonnementmodell](http://en.wikipedia.org/wiki/Publish/subscribe), und der Server behält keine Listen von Gruppen und Gruppenmitgliedschaften. Dadurch Maximieren der Skalierbarkeit, da Wenn Sie einen Knoten zu einer Webfarm hinzufügen, muss einem beliebigen Zustand, in der SignalR verwaltet an den neuen Knoten weitergegeben werden.

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a>Asynchrone Ausführung der Add- und Remove-Methoden

Die `Groups.Add` und `Groups.Remove` Methoden asynchron auszuführen. Wenn Sie einen Client zu einer Gruppe hinzufügen und eine Nachricht sofort an den Client senden, mit dem Group möchten, müssen Sie sicherstellen, dass die `Groups.Add` Methode zuerst beendet ist. Im folgenden Codebeispiel wird veranschaulicht, wie nachholen.

**Einen Client zu einer Gruppe hinzufügen und diesen Client dann messaging**

[!code-csharp[Main](hubs-api-guide-server/samples/sample46.cs?highlight=1,3)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a>Gruppenmitgliedschaft Persistenz

SignalR verfolgt Verbindungen, nicht von Benutzern, wenn also einen Benutzer in der gleichen Gruppe werden jedes Mal, wenn der Benutzer richtet eine Verbindung soll, müssen Sie anrufen `Groups.Add` jedes Mal, wenn der Benutzer eine neue Verbindung hergestellt wird.

Nach einem vorübergehenden Dienstausfall Konnektivität kann manchmal SignalR die Verbindung wiederherzustellen automatisch. In diesem Fall SignalR ist die gleiche Verbindung wiederherstellen nicht Herstellen einer neuen Verbindung und Gruppenmitgliedschaft für den Client wird daher automatisch wiederhergestellt. Dies wird dadurch auch, wenn die temporären Unterbrechung einen Neustart des Servers oder einen Fehler, das Ergebnis ist Verbindungsstatus für jeden Client, Gruppenmitgliedschaften, einschließlich Roundtrip an den Client ist. Wenn ein Server ausfällt und durch einen neuen Server ersetzt wird, bevor die Verbindung ein Timeout eintritt, kann ein Client automatische Wiederherstellen der Verbindung mit dem neuen Server und in Ihnen Mitglied der ist Gruppen erneut registrieren.

Wenn eine Verbindung nicht automatisch nach einem Verlust der Verbindung, wiederhergestellt werden kann oder wenn die Verbindung ein Timeout auftritt oder wenn der Client die Verbindung trennt (z. B. bei ein Browser zu einer neuen Seite navigiert) sind Gruppenmitgliedschaften verloren. Das nächste Mal die Benutzer eine Verbindung herstellt, wird eine neue Verbindung. Um die Gruppenmitgliedschaften beizubehalten, wenn derselbe Benutzer eine neue Verbindung hergestellt wird, muss die Anwendung so verfolgen die Zuordnungen zwischen Benutzern und Gruppen und Gruppenmitgliedschaften wiederherstellen, jedes Mal ein Benutzer eine neue Verbindung hergestellt wird.

Weitere Informationen zu Verbindungen und erneute Verbindungen finden Sie unter [wie Lebensdauer Verbindungsereignisse in der Hub-Klasse behandelt](#connectionlifetime) weiter unten in diesem Thema.

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a>Einzelbenutzer-Gruppen

Anwendungen, die in der Regel verwenden Sie SignalR haben zum Nachverfolgen von die Zuordnungen zwischen Benutzern und Verbindungen, damit Sie wissen, welche Benutzer eine Nachricht gesendet hat und welche Benutzer eine Nachricht empfangen werden soll. Gruppen werden in einem der zwei häufig verwendete Muster für auf diese Weise verwendet.

- Einzelbenutzer-Gruppen.

    Sie können Geben Sie den Benutzernamen als der Gruppenname und die aktuelle Verbindungs-ID der Gruppe hinzufügen, jedes Mal, wenn der Benutzer eine Verbindung herstellt, oder die Verbindung wiederherstellt. Zum Senden von Nachrichten an den Benutzer, die Sie der Gruppe gesendet. Ein Nachteil dieser Methode ist, dass die Gruppe Sie eine Möglichkeit bietet, um festzustellen, ob der Benutzer online oder offline ist.
- Nachverfolgen von Zuordnungen zwischen den Benutzernamen und Verbindungs-IDs.

    Sie können eine Zuordnung zwischen jeden Benutzernamen und ein oder mehrere Verbindungs-IDs in einem Wörterbuch oder einer Datenbank speichern und aktualisieren Sie die gespeicherten Daten jedes Mal, die der Benutzer eine Verbindung herstellt, oder die Verbindung trennt. Geben Sie zum Senden von Nachrichten an den Benutzer der Verbindungs-IDs an. Ein Nachteil dieser Methode ist, dass mehr Arbeitsspeicher benötigt.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a>Behandeln von Lebensdauer Verbindungsereignisse in der Hub-Klasse

Gründe für die Behandlung Lebensdauer Verbindungsereignisse werden zum Nachverfolgen, ob ein Benutzer oder nicht verbunden ist, und klicken Sie zum Nachverfolgen der Zuordnung zwischen den Benutzernamen und Verbindungs-IDs. Um Ihren eigenen Code auszuführen, wenn Clients eine Verbindung herstellen oder trennen, überschreiben die `OnConnected`, `OnDisconnected`, und `OnReconnected` virtuelle Methoden des Hubs-Klasse, wie im folgenden Beispiel gezeigt.

[!code-csharp[Main](hubs-api-guide-server/samples/sample47.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a>Wenn OnConnected OnDisconnected und OnReconnected aufgerufen werden

Jedes Mal ein Browser zu einer neuen Seite navigiert eine neue Verbindung muss hergestellt werden, d. h. SignalR führt die `OnDisconnected` Methode gefolgt von der `OnConnected` Methode. SignalR erstellt eine neuen Verbindungs-ID immer, wenn eine neue Verbindung hergestellt wird.

Die `OnReconnected` Methode wird aufgerufen, wenn es eine temporäre Unterbrechung in Verbindung, die automatisch wurde von SignalR wiederherstellen können, z. B. wenn ein Kabel ist vorübergehend getrennt und wiederhergestellt werden, bevor die Verbindung ein Timeout eintritt. Die `OnDisconnected` Methode wird aufgerufen, wenn der Client getrennt ist und SignalR kann nicht automatisch erneut eine Verbindung herstellen, z. B. bei ein Browser zu einer neuen Seite navigiert. Eine mögliche Folge von Ereignissen für einen bestimmten Client also `OnConnected`, `OnReconnected`, `OnDisconnected`; oder `OnConnected`, `OnDisconnected`. Die Sequenz nicht angezeigt `OnConnected`, `OnDisconnected`, `OnReconnected` für eine bestimmte Verbindung.

Die `OnDisconnected` Methode nicht in einigen Szenarien, z. B. beim Ausfall eines Servers aufgerufen wird, oder die Anwendungsdomäne ruft wiederverwendet. Wenn einem anderen Server, auf die Zeile stammen oder der App-Domäne abgeschlossen, die Wiederverwendung ist, einige Clients möglicherweise erneut eine Verbindung herstellen und das Auslösen der `OnReconnected` Ereignis.

Weitere Informationen finden Sie unter [verstehen und Behandeln von Ereignissen für Lebensdauer in SignalR](handling-connection-lifetime-events.md).

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a>Aufruferstatus nicht aufgefüllt

Die Verbindung für die Lebensdauer Ereignishandlermethoden heißen vom Server, d. einem beliebigen Zustand, die Sie aufnehmen h., in der `state` Objekt auf dem Client wird nicht aufgefüllt werden, der `Caller` Eigenschaft auf dem Server. Informationen zu den `state` Objekt und die `Caller` Eigenschaft finden Sie unter [zum Zustand zwischen Clients und der Hub-Klasse übergeben](#passstate) weiter unten in diesem Thema.

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a>Gewusst wie: Abrufen von Informationen über den Client aus der Kontexteigenschaft

Verwenden Sie zum Abrufen von Informationen über den Client die `Context` Eigenschaft der Hub-Klasse. Die `Context` Eigenschaft gibt eine [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) Objekt, das Zugriff auf die folgenden Informationen:

- Der Verbindungs-ID des aufrufenden Clients.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample48.cs?highlight=1)]

    Die Verbindungs-ID ist eine GUID, die von SignalR zugewiesen wird (Sie können nicht den Wert in Ihrem eigenen Code angeben). Es ist ein Verbindungs-ID für jede Verbindung, und die gleiche Verbindung, die ID von sämtliche Hubs im aktuellen verwendet wird, wenn Sie mehrere Hubs in Ihre Anwendung haben.
- HTTP-Header-Daten.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    Sie können auch HTTP-Header aus abrufen `Context.Headers`. Der Grund für mehrere Verweise auf dasselbe ist, dass `Context.Headers` wurde zunächst erstellt der `Context.Request` Eigenschaft später hinzugefügt wurde und `Context.Headers` wurde für Abwärtskompatibilität beibehalten.
- Abfragen von Zeichenfolgendaten.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample50.cs?highlight=1)]

    Sie können auch Zeichenfolgendaten aus Abfrage abrufen `Context.QueryString`.

    Die Abfragezeichenfolge, die Sie in dieser Eigenschaft abgerufen wird, die mit der HTTP-Anforderung verwendet wurde, die die SignalR-Verbindung hergestellt. Sie können Abfragezeichenfolgen-Parameter im Client hinzufügen, konfigurieren Sie die Verbindung, die wodurch eine einfache Möglichkeit, Daten über den Client vom Client an den Server übergeben wird. Das folgende Beispiel zeigt eine Möglichkeit, eine Abfragezeichenfolge in einem JavaScript-Client hinzufügen, wenn Sie den generierten Proxy verwenden.

    [!code-javascript[Main](hubs-api-guide-server/samples/sample51.js?highlight=1)]

    Weitere Informationen zum Festlegen von Abfragezeichenfolgen-Parameter, finden Sie unter den API-Handbüchern für die [JavaScript](hubs-api-guide-javascript-client.md) und [.NET](hubs-api-guide-net-client.md) Clients.

    Sie finden die Transportmethode verwendet für die Verbindung in der Abfrage Zeichenfolgendaten, zusammen mit einigen anderen Werten, die intern vom SignalR verwendet:

    [!code-csharp[Main](hubs-api-guide-server/samples/sample52.cs)]

    Der Wert des `transportMethod` werden "WebSockets", "ServerSentEvents", "ForeverFrame" oder "LongPolling". Beachten Sie, dass, wenn Sie diesen Wert, in Prüfen der `OnConnected` Ereignishandlermethode ist in einigen Szenarien möglicherweise zunächst einen Transportwert, der nicht der endgültigen ausgehandelte Transportmethode für die Verbindung ist abrufen. In diesem Fall wird die Methode löst eine Ausnahme aus, und wird später erneut aufgerufen werden, wenn die letzte Transportmethode hergestellt wird.
- Cookies.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample53.cs?highlight=1)]

    Außerdem erhalten Sie, dass Cookies von `Context.RequestCookies`.
- Benutzerinformationen.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample54.cs?highlight=1)]
- Das HttpContext-Objekt für die Anforderung:

    [!code-csharp[Main](hubs-api-guide-server/samples/sample55.cs?highlight=1)]

    Verwenden Sie diese Methode anstelle `HttpContext.Current` zum Abrufen der `HttpContext` Objekt für die SignalR-Verbindung.

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a>Gewusst wie: Zustand zwischen Clients und der Hub-Klasse übergeben.

Der Clientproxy bietet eine `state` Objekt, in dem Sie Daten, die mit jedem Methodenaufruf an den Server übertragen werden sollen speichern können. Auf dem Server können Sie zugreifen, diese Daten in der `Clients.Caller` Eigenschaft, die in hubmethoden, die von Clients aufgerufen werden. Die `Clients.Caller` Eigenschaft wird nicht angegeben, für die Verbindung für die Lebensdauer Ereignishandlermethoden `OnConnected`, `OnDisconnected`, und `OnReconnected`.

Erstellen oder Aktualisieren von Daten in der `state` Objekt und die `Clients.Caller` Eigenschaft funktioniert in beide Richtungen. Sie können die Werte auf dem Server aktualisieren, und sie zurück an den Client übergeben werden.

Das folgende Beispiel zeigt die JavaScript-Clientcode, mit dem Status für die Übertragung an den Server mit jedem Methodenaufruf speichert.

[!code-javascript[Main](hubs-api-guide-server/samples/sample56.js?highlight=1-2)]

Im folgende Beispiel wird der entsprechenden Code in einer .NET Client.

[!code-csharp[Main](hubs-api-guide-server/samples/sample57.cs?highlight=1-2)]

In der Hub-Klasse, erreichen Sie, diese Daten in der `Clients.Caller` Eigenschaft. Im folgende Beispiel wird gezeigt, Code, der im vorherigen Beispiel genannten Zustand abruft.

[!code-csharp[Main](hubs-api-guide-server/samples/sample58.cs?highlight=3-4)]

> [!NOTE]
> Dieser Mechanismus für den persistenten Zustand dient nicht zur große Mengen von Daten, da alles, was Sie gelagerte der `state` oder `Clients.Caller` Eigenschaft ist mit jedem Methodenaufruf Roundtrip ausgeführt. Es eignet sich für kleinere Elemente wie Benutzernamen oder Leistungsindikatoren.


In VB.NET oder bei einem stark typisierten Hub, das vom Aufrufer Zustandsobjekt, das nicht möglich über `Clients.Caller`; stattdessen verwenden Sie `Clients.CallerState` (eingeführt in SignalR 2.1):

**Verwenden von CallerState in c#**

[!code-csharp[Main](hubs-api-guide-server/samples/sample59.cs?highlight=3-4)]

**Verwenden von CallerState in Visual Basic**

[!code-vb[Main](hubs-api-guide-server/samples/sample60.vb)]

<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a>Gewusst wie: Behandeln von Fehlern in der Hub-Klasse

Verwenden Sie zum Behandeln von Fehlern, die in Ihrer Klasse hubmethoden auftreten, eine oder mehrere der folgenden Methoden:

- Umschließen Sie Ihrem Code im Try / Catch-Blocks, und melden Sie das Ausnahmeobjekt. Zu Debugzwecken können Sie die Ausnahme an den Client senden, aber aus Sicherheitsgründen Gründe, senden detaillierte Informationen für Clients in der Produktion nicht empfohlen.
- Erstellen Sie ein Hubs Pipeline-Modul, das verarbeitet die [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) Methode. Das folgende Beispiel zeigt eine Pipeline-Modul, das protokolliert Fehler, gefolgt vom Code in Startup.cs, in den das Modul in die Pipeline Hubs injiziert.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample61.cs)]

    [!code-csharp[Main](hubs-api-guide-server/samples/sample62.cs?highlight=4)]
- Verwenden der `HubException` Klasse (eingeführt in SignalR-2). Dieser Fehler kann aus jeder hubaufruf ausgelöst werden. Die `HubError` Konstruktor akzeptiert eine zeichenfolgenmeldung und ein Objekt zum Speichern von zusätzliche Fehlerdaten. SignalR wird die Ausnahme automatisch zu serialisieren und senden sie an den Client, in dem sie dienen zum Ablehnen oder Fehlschlagen des Aufrufs der hubmethode.

    Die folgenden Codebeispiele veranschaulichen, wie Sie löst eine `HubException` während eines hubaufrufs sowie zum Behandeln der Ausnahme in JavaScript und .NET Clients.

    **Servercode veranschaulichen die HubException-Klasse**

    [!code-csharp[Main](hubs-api-guide-server/samples/sample63.cs)]

    **JavaScript-Clientcode Demonstrieren der Antwort mit dem Auslösen einer HubException in einem hub**

    [!code-html[Main](hubs-api-guide-server/samples/sample64.html)]

    **Demonstrieren der Antwort mit dem Auslösen einer HubException in einem Hub .NET Client-code**

    [!code-csharp[Main](hubs-api-guide-server/samples/sample65.cs)]

Weitere Informationen zu Hub Pipeline Modulen finden Sie unter [zum Anpassen der Pipeline Hubs](#hubpipeline) weiter unten in diesem Thema.

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a>Gewusst wie: Aktivieren der Ablaufverfolgung

Fügen Sie zum Aktivieren der serverseitigen Ablaufverfolgung system.diagnostics-Element der Datei "Web.config" hinzu, wie im folgenden Beispiel gezeigt:

[!code-html[Main](hubs-api-guide-server/samples/sample66.html?highlight=17-72)]

Wenn Sie die Anwendung in Visual Studio ausführen, sehen Sie die Protokolle in der **Ausgabe** Fenster.

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a>Das Client-Methoden aufrufen und Verwalten von Gruppen von außerhalb der Hub-Klasse

Um den Client von einer anderen Klasse als der Hub-Klasse Methoden aufrufen, rufen Sie einen Verweis auf das Kontextobjekt SignalR für den Hub und verwenden Sie, die zum Aufrufen von Methoden auf dem Client oder Verwalten von Gruppen.

Im folgenden Beispiel `StockTicker` Klasse das Context-Objekt abruft, speichert es in einer Instanz der Klasse, speichert die Klasseninstanz in eine statische Eigenschaft und verwendet Sie den Kontext aus der Singleton-Klasse-Instanz zum Aufrufen der `updateStockPrice` Methode auf, die Clients verbunden mit einem Hub, mit dem Namen `StockTickerHub`.

[!code-csharp[Main](hubs-api-guide-server/samples/sample67.cs?highlight=8,24)]

Wenn Sie den Kontext mehrere-Male in einem langlebige Objekt verwenden möchten, rufen Sie den Verweis einmal, und speichern Sie, anstatt eine Vorgang jedes Mal des Projekts. Einmal Abrufen des Kontexts wird sichergestellt, dass SignalR für Clients in derselben Reihenfolge Senden von Nachrichten in der Ihre hubmethoden Client Methodenaufrufe vornehmen. Ein Lernprogramm, das veranschaulicht, wie die SignalR-Kontext für einen Hub verwenden, finden Sie unter [Broadcast-Server mit ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a>Aufrufen von Clientmethoden

Können Sie angeben, welche Clients die RPC empfangen werden, aber Sie haben weniger Optionen als beim Aufrufen von einer hubklasse. Der Grund dafür ist, dass der Kontext keinen bestimmten Aufruf von einem Client zugeordnet ist, damit keine Methoden, die die aktuelle Verbindungs-ID, wie z. B. bekannt sein `Clients.Others`, oder `Clients.Caller`, oder `Clients.OthersInGroup`, sind nicht verfügbar. Die folgenden Optionen sind verfügbar:

- Alle verbundenen Clients.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample68.cs)]
- Um einen spezifischen Client identifizierte Verbindungs-ID.

    [!code-css[Main](hubs-api-guide-server/samples/sample69.css)]
- Alle verbundenen Clients mit Ausnahme der angegebenen Clients identifiziert, die vom Verbindungs-ID.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample70.cs)]
- Alle verbundenen Clients in einer angegebenen Gruppe.

    [!code-css[Main](hubs-api-guide-server/samples/sample71.css)]
- Alle verbundenen Clients in einer angegebenen Gruppe mit Ausnahme der angegebenen Clients identifiziert, die vom Verbindungs-ID.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample72.cs)]

Wenn Sie Ihre hubklasse von Methoden in der Hub-Klasse aufgerufen werden, können Sie in der aktuellen Verbindungs­id übergeben und verwenden, die mit `Clients.Client`, `Clients.AllExcept`, oder `Clients.Group` simulieren `Clients.Caller`, `Clients.Others`, oder `Clients.OthersInGroup`. Im folgenden Beispiel die `MoveShapeHub` Klasse übergibt die Verbindungs-ID, die `Broadcaster` Klasse, damit die `Broadcaster` Klasse kann simulieren `Clients.Others`.

[!code-csharp[Main](hubs-api-guide-server/samples/sample73.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a>Verwalten von Gruppenmitgliedschaften

Verwalten von Gruppen müssen Sie die gleichen Optionen wie in einer hubklasse.

- Einen Client zu einer Gruppe hinzufügen

    [!code-csharp[Main](hubs-api-guide-server/samples/sample74.cs)]
- Entfernen Sie einen Client aus einer Gruppe

    [!code-css[Main](hubs-api-guide-server/samples/sample75.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a>Gewusst wie: Anpassen die Pipeline Hubs

SignalR ermöglicht es Ihnen, Ihren eigenen Code in der Hubpipeline einzufügen. Das folgende Beispiel zeigt ein benutzerdefiniertes Hub-Pipeline-Modul, das von jedem eingehenden Methodenaufruf empfangen, die vom Client und vom ausgehenden Aufruf der Methode, die aufgerufen wird, auf dem Client protokolliert:

[!code-csharp[Main](hubs-api-guide-server/samples/sample76.cs)]

Der folgende code in der *Startup.cs* Datei registriert das Modul, in der Hub-Pipeline ausgeführt:

[!code-csharp[Main](hubs-api-guide-server/samples/sample77.cs?highlight=3)]

Es gibt viele verschiedene Methoden, die Sie überschreiben können. Eine vollständige Liste finden Sie unter [HubPipelineModule Methoden](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).
