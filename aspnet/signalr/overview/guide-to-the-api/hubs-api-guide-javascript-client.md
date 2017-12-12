---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
title: ASP.NET SignalR-Hubs-API-Handbuch - JavaScript-Client | Microsoft Docs
author: pfletcher
description: "Dieses Dokument enthält eine Einführung zur Verwendung der API-Hubs für SignalR Version 2 im JavaScript-Clients, z. B. Browsern und Windows Store (WinJS) Application..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/28/2015
ms.topic: article
ms.assetid: a9fd4dc0-1b96-4443-82ca-932a5b4a8ea4
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 65e369a393a8c5d2d1bba11b5c71347df8f9c69d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-signalr-hubs-api-guide---javascript-client"></a>ASP.NET SignalR-Hubs-API-Handbuch - JavaScript-Client
====================
durch [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

> Dieses Dokument enthält eine Einführung zur Verwendung der API-Hubs für SignalR Version 2 im JavaScript-Clients, z. B. Browsern und Anwendungen für Windows Store (WinJS).
> 
> Die SignalR-Hubs-API können Sie von Remoteprozeduraufrufen (RPCs) von einem Server verbundenen Clients und von den Clients an den Server vornehmen. Im Servercode definieren Sie Methoden, die von Clients aufgerufen werden können, und rufen Sie die Methoden, die auf dem Client ausgeführt. Im Clientcode definieren Sie Methoden, die vom Server aufgerufen werden kann, und rufen Sie die Methoden, die auf dem Server ausgeführt. SignalR ist aller von der Client-zu-Server-Aufgaben, die für Sie übernimmt.
> 
> SignalR bietet außerdem eine technisch anspruchsvolle-API, die dauerhafte Verbindungen aufgerufen. Eine Einführung in SignalR-Hubs und dauerhafte Verbindungen erhalten finden Sie unter [Einführung in SignalR](../getting-started/introduction-to-signalr.md).
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

- [Den generierten Proxy und was es für Sie erledigt.](#genproxy)

    - [Verwenden Sie den generierten proxy](#cantusegenproxy)
- [Client-Setup](#clientsetup)

    - [Gewusst wie: den dynamisch generierten Proxy verweisen.](#dynamicproxy)
    - [Vorgehensweise: erstellen eine physische Datei für SignalR generierter proxy](#manualproxy)
- [Gewusst wie: Herstellen einer Verbindung](#establishconnection)

    - [$. connection.hub ist das gleiche Objekt erstellt, $.hubConnection()](#connequivalence)
    - [Asynchrone Ausführung der Start-Methode](#asyncstart)
- [Gewusst wie: Herstellen einer Verbindung domänenübergreifende](#crossdomain)
- [Gewusst wie: Konfigurieren der Verbindung](#configureconnection)

    - [Gewusst wie: Angeben von Abfragezeichenfolgen-Parameter](#querystring)
    - [Zum Angeben der Transportmethode](#transport)
- [Gewusst wie: Abrufen ein Proxys für eine hubklasse](#getproxy)
- [Zum Definieren von Methoden auf dem Client, die der Server aufrufen können](#callclient)
- [Gewusst wie: Aufrufen von Servermethoden vom client](#callserver)
- [Behandeln von Lebensdauer Verbindungsereignisse](#connectionlifetime)
- [Gewusst wie: Behandeln von Fehlern](#handleerrors)
- [Clientseitige Protokollierung aktivieren](#logging)

Dokumentation über die Programmierung des Servers oder Clients .NET finden Sie in den folgenden Ressourcen:

- [SignalR-Hubs-API-Leitfaden – Server](hubs-api-guide-server.md)
- [SignalR-Hubs-API-Leitfaden – .NET Client](hubs-api-guide-net-client.md)

Die Serverkomponente von SignalR-2 ist nur verfügbar, auf .NET 4.5, (obwohl es ein .NET Client für SignalR-2 unter .NET 4.0 ist).

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a>Den generierten Proxy und was es für Sie erledigt.

Sie können einen JavaScript-Client für die Kommunikation mit einer SignalR-Dienst mit oder ohne einen Proxy aus, dem SignalR für Sie generiert programmieren. Leistungsumfang der Proxy für Sie ist die Syntax des Codes zu vereinfachen, die Verbindung verwenden, Write-Methoden, die der Server aufgerufen wird, und rufen Methoden auf dem Server.

Beim Schreiben von Code zum Aufrufen von Servermethoden, der generierte Proxy können Sie Syntax zu verwenden, das aussieht, als wären Sie eine lokale Funktion ausgeführt wurden: Sie können schreiben `serverMethod(arg1, arg2)` anstelle von `invoke('serverMethod', arg1, arg2)`. Die generierten Proxy-Syntax kann auch Fehler clientseitige sofortigen und verständlicher, wenn Sie einen Server-Methodennamen richtig eingegeben. Und wenn Sie manuell die Datei erstellen, die die Proxys definiert außerdem erhalten Sie IntelliSense-Unterstützung für das Schreiben von Code, der Servermethoden aufruft.

Nehmen wir beispielsweise an, dass Sie die folgenden hubklasse auf dem Server verfügen:

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

Die folgenden Codebeispiele zeigen, wie sieht der JavaScript-Code zum Aufrufen der `NewContosoChatMessage` Methode auf dem Server und die Aufrufe der Empfang der `addContosoChatMessageToPage` Methode vom Server.

**Mit den generierten proxy**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

**Ohne den generierten proxy**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a>Verwenden Sie den generierten proxy

Wenn Sie mehrere Ereignishandler für eine Clientmethode zu registrieren, die der Server ruft möchten, können nicht Sie den generierten Proxy verwenden. Andernfalls können auch den generierten Proxy verwenden oder nicht auf die Einstellung für die Codierung basieren. Wenn Sie nicht verwenden, müssen Sie keine URL / "Signalr-Hubs" in verweisen eine `script` Element im Clientcode.

<a id="clientsetup"></a>

## <a name="client-setup"></a>Client-setup

Ein JavaScript-Client erfordert Verweise auf jQuery und der SignalR Core JavaScript-Datei. Die jQuery-Version muss es sich um 1.6.4 oder höher Hauptversionen, z. B. 1.7.2, 1.8.2 oder 1.9.1 sein. Wenn Sie den generierten Proxy verwenden möchten, benötigen Sie auch einen Verweis auf den Proxy generiert SignalR JavaScript-Datei. Das folgende Beispiel zeigt, wie die Verweise in einer HTML-Seite aussehen könnte, die den generierten Proxy verwendet.

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample4.html)]

Diese Verweise in dieser Reihenfolge enthalten sein müssen: jQuery SignalR Core danach und SignalR-Proxys First-und last.

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a>Gewusst wie: den dynamisch generierten Proxy verweisen.

Im vorherigen Beispiel wird der Verweis auf den Proxy SignalR generiert für dynamisch generierte JavaScript-Code, nicht zu einer physischen Datei. SignalR JavaScript-Code für den Proxy bei Bedarf erstellt und an dem Client als Antwort auf die URL "/ Signalr/Hubs" dient. Bei Angabe eine andere base-URL für SignalR-Verbindungen auf dem Server in Ihrer `MapSignalR` -Methode, die URL für die dynamisch generierte Proxydatei ist Ihre benutzerdefinierte URL mit "/ Hubs" angefügt ist.

> [!NOTE]
> Verwenden Sie für Windows 8 (Windows Store) JavaScript-Clients die physische Proxydatei statt der dynamisch generierten. Weitere Informationen finden Sie unter [zum Erstellen einer physischen Datei für SignalR generierten Proxy](#manualproxy) weiter unten in diesem Thema.


Verwenden Sie die Tilde zum Verweisen auf das Stammverzeichnis der Anwendung in Ihrem Proxy Dateiverweis, in einer ASP.NET MVC 4 oder 5 Razor-Ansicht:

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample5.html)]

Weitere Informationen zur Verwendung von SignalR in MVC 5 finden Sie unter [erste Schritte mit SignalR und MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).

Verwenden Sie in einer ASP.NET MVC 3 Razor-Ansicht `Url.Content` für den Verweis der Proxy-Datei:

[!code-cshtml[Main](hubs-api-guide-javascript-client/samples/sample6.cshtml)]

Verwenden Sie in einer ASP.NET Web Forms-Anwendung `ResolveClientUrl` für die Proxys Verweis der Datei, oder registrieren Sie ihn über das ScriptManager mit einem relativen Pfad Stamm app (beginnend mit einer Tilde):

[!code-aspx[Main](hubs-api-guide-javascript-client/samples/sample7.aspx)]

Verwenden Sie als allgemeine Regel die gleiche Methode zum Angeben der URL "/ Signalr/Hubs", die Sie für CSS oder JavaScript-Dateien verwenden. Wenn Sie eine URL angeben, ohne mit einer Tilde, wird in einigen Szenarien Ihrer Anwendung fehlerfrei, wenn Sie in Visual Studio mit IIS Express testen schlägt jedoch fehl mit Fehler 404 während der Bereitstellung auf vollständige IIS. Weitere Informationen finden Sie unter **Auflösen von Verweisen auf Ressourcen auf der Stammebene** in [Webserver in Visual Studio für ASP.NET-Webprojekte](https://msdn.microsoft.com/en-us/library/58wxa9w5.aspx) auf der MSDN-Website.

Wenn Sie ein Webprojekt in Visual Studio 2013 im Debugmodus ausgeführt, und wenn Sie Internet Explorer als Ihren Browser verwenden, Sie die Proxydatei in sehen **Projektmappen-Explorer** unter **Skriptdokumente**, entsprechend der folgende Abbildung.

![JavaScript generiert eine Proxyklasse im Projektmappen-Explorer](hubs-api-guide-javascript-client/_static/image1.png)

Um den Inhalt der Datei anzuzeigen, doppelklicken Sie auf **Hubs**. Wenn Sie Visual Studio 2012 oder 2013 und Internet Explorer nicht verwenden oder wenn Sie nicht im Debugmodus befinden, können Sie auch den Inhalt der Datei zu suchen und die URL "/ SignalR/Hubs" abrufen. Beispielsweise wird bei der Ausführung des Standorts auf `http://localhost:56699`, wechseln Sie zu `http://localhost:56699/SignalR/hubs` in Ihrem Browser.

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a>Vorgehensweise: erstellen eine physische Datei für SignalR generierter proxy

Als Alternative zum dynamisch generierten Proxy können Sie erstellen eine physische Datei, die den Proxycode verfügt und diese Datei verweisen. Sie sollten sich zur Steuerung von Inhalte zwischengespeichert oder bundling Verhalten dazu oder IntelliSense abzurufen, bei der Codierung Aufrufe von Servermethoden.

Um eine Proxydatei zu erstellen, führen Sie die folgenden Schritte aus:

1. Installieren der [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet-Paket.
2. Öffnen Sie ein Eingabeaufforderungsfenster, und navigieren Sie zu der *Tools* Ordner, der die SignalR.exe-Datei enthält. Ordner "Tools" wird am folgenden Speicherort:

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.2.1.0\tools`
3. Geben Sie folgenden Befehl ein:

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    Der Pfad zu Ihrer *DLL* ist in der Regel die *"bin"* Ordner im Projektordner.

    Dieser Befehl erstellt eine Datei namens *server.js* im gleichen Ordner wie *signalr.exe*.
4. Versetzen der *server.js* Datei in eine entsprechende Ordner im Projekt, benennen Sie sie nach Bedarf für Ihre Anwendung und einen Verweis darauf anstelle der / "Signalr-Hubs" Verweis hinzufügen.

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Gewusst wie: Herstellen einer Verbindung

Bevor Sie eine Verbindung herstellen können, müssen Sie ein Verbindungsobjekt zu erstellen, erstellen einen Proxy und registrieren Sie Ereignishandler für Methoden, die vom Server aufgerufen werden können. Wenn Sie den Proxy und Ereignishandler eingerichtet wurden, Herstellung einer Verbindung durch Aufrufen der `start` Methode.

Wenn Sie den generierten Proxy verwenden, müssen Sie das Verbindungsobjekt in Ihrem eigenen Code erstellt werden, da der generierte Proxycode für Sie erledigt.

<a id="nogenconnection"></a>

**Herstellen einer Verbindungs (mit den generierten Proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

**Herstellen einer Verbindungs (ohne den generierten Proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

Im Beispielcode wird die Standardeinstellung "/ Signalr" die URL für die Verbindung mit dem SignalR-Dienst. Informationen über das Angeben einer anderen Basis-URL finden Sie unter [ASP.NET SignalR-Hubs API-Handbuch - Server - URL /signalr](hubs-api-guide-server.md#signalrurl).

Standardmäßig ist der Hub-Speicherort des aktuellen Servers; Wenn Sie eine Verbindung mit einem anderen Server herstellen, geben Sie die URL vor dem Aufruf der `start` Methode, wie im folgenden Beispiel gezeigt:

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample10.js)]

> [!NOTE]
> Normalerweise registrieren Sie Ereignishandler vor dem Aufruf der `start` Methode zum Herstellen der Verbindung. Wenn einige Ereignishandler zu registrieren, nach dem Herstellen der Verbindung werden sollen, können Sie dies tun, aber Sie müssen mindestens eine Ihrer Ereignis einer oder mehrere Ereignishandler vor dem Aufruf Registrieren der `start` Methode. Ein Grund dafür ist, dass in einer Anwendung können viele Hubs vorhanden sein, aber nicht empfehlenswert Auslösen der `OnConnected` Ereignis für jedes Hub, wenn Sie nur eines davon verwenden möchten. Wenn die Verbindung hergestellt wird, das Vorhandensein einer Client-Methode auf einem Hub-Proxy ist womit SignalR zum Auslösen der `OnConnected` Ereignis. Alle Ereignishandler vor dem Aufruf registriert die `start` -Methode, Sie werden zum Aufrufen von Methoden der Hub, aber auf des Hubs `OnConnected` Methode wird nicht aufgerufen werden und keine Clientmethoden werden vom Server aufgerufen werden.


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a>$. connection.hub ist das gleiche Objekt erstellt, $.hubConnection()

Wie Sie aus den Beispielen sehen können, wenn Sie den generierten Proxy verwenden `$.connection.hub` bezieht sich auf das Verbindungsobjekt. Dies ist das gleiche Objekt, die Sie durch den Aufruf erhalten `$.hubConnection()` Sie sind nicht bei den generierten Proxy verwenden. Der generierte Proxycode erstellt die Verbindung für Sie durch Ausführen der folgenden Anweisung:

![Erstellen eine Verbindung in die generierte Proxydatei](hubs-api-guide-javascript-client/_static/image3.png)

Wenn Sie den generierten Proxy verwenden, können Sie alle mit Aktionen `$.connection.hub` , dass Sie mit einem Verbindungsobjekt ausführen können, wenn Sie den generierten Proxy nicht verwenden.

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a>Asynchrone Ausführung der Start-Methode

Die `start` Methode asynchron ausgeführt wird. Gibt eine [jQuery zurückgestellt Objekt](http://api.jquery.com/category/deferred-object/), was bedeutet, dass Sie Rückruffunktionen hinzufügen können, durch Aufrufen von Methoden wie z. B. `pipe`, `done`, und `fail`. Wenn Sie über Code, die verfügen nach dem Herstellen der Verbindung ausgeführt werden soll, z. B. einen Aufruf einer Servermethode, legen Sie diesen Code in eine Rückruffunktion oder aus eine Rückruffunktion aufgerufen Sie wird. Die `.done` Rückrufmethode wird ausgeführt, nachdem die Verbindung hergestellt wurde, und nach der code Ihrer `OnConnected` Ereignishandlermethode auf dem Server die Ausführung abgeschlossen ist.

Wenn Sie die Anweisung "Jetzt verbunden" aus dem vorherigen Beispiel als die nächste Zeile des Codes nach dem Einfügen der `start` Methodenaufruf (nicht in einer `.done` Rückruf), die `console.log` Zeile ausgeführt, bevor die Verbindung hergestellt wird, wie das folgende Beispiel zeigt Beispiel:

![Falsches vorgehen, Code zu schreiben, der ausgeführt wird, nachdem die Verbindung hergestellt wird](hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a>Gewusst wie: Herstellen einer Verbindung domänenübergreifende

In der Regel, wenn der Browser eine Seite aus lädt `http://contoso.com`, die SignalR-Verbindung ist auf der gleichen Domäne `http://contoso.com/signalr`. Wenn die Seite `http://contoso.com` stellt eine Verbindung mit `http://fabrikam.com/signalr`, d. h. eine domänenübergreifende-Verbindung. Domänenübergreifende Verbindungen werden aus Gründen der Sicherheit standardmäßig deaktiviert.

Im SignalR wurden von einem einzelnen EnableCrossDomain Flag 1.x, domänenübergreifende Anforderungen gesteuert. Dieses Flag gesteuert, JSONP und CORS-Anforderungen. Für größere Flexibilität, die alle CORS-Unterstützung die Serverkomponente von SignalR entzogen wurde (JavaScript-Clients weiterhin verwenden CORS normalerweise, wenn es erkannt wird, dass der Browser unterstützt), und neue OWIN-Middleware zur Unterstützung dieser Szenarien verfügbar gemacht wurde.

Wenn JSONP auf dem Client (zur Unterstützung von älteren Browsern domänenübergreifende Anforderungen) erforderlich ist, müssen sie explizit aktiviert werden, indem die Einstellung `EnableJSONP` auf die `HubConfiguration` -Objekt `true`, wie unten dargestellt. JSONP ist standardmäßig deaktiviert, da es weniger sicher als CORS ist.

**Hinzufügen von Microsoft.Owin.Cors zu Ihrem Projekt:** um diese Bibliothek zu installieren, führen Sie den folgenden Befehl in der Paket-Manager-Konsole:

`Install-Package Microsoft.Owin.Cors`

Mit diesem Befehl wird die 2.1.0 hinzugefügt Version des Pakets zu Ihrem Projekt.

### <a name="calling-usecors"></a>UseCors aufrufen

 Der folgende Codeausschnitt veranschaulicht, wie domänenübergreifende Verbindungen in SignalR 2 implementiert wird. 

**Implementieren von domänenübergreifende Anforderungen in SignalR 2**

Der folgende Code veranschaulicht, wie zum Aktivieren von CORS oder JSONP in einer SignalR-2-Projekt. Dieses Codebeispiel verwendet `Map` und `RunSignalR` anstelle von `MapSignalR`, sodass nur für die SignalR-Anforderungen, die CORS-Unterstützung erfordern, die CORS-Middleware ausgeführt wird (und nicht für den gesamten Datenverkehr im angegebenen Pfad `MapSignalR`.) Zuordnung kann auch für andere Middleware, die verwendet werden, die für eine bestimmte URL-Präfix, nicht für die gesamte Anwendung ausgeführt werden muss.

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample11.cs)]

> [!NOTE] 
> 
> - Legen Sie die keine `jQuery.support.cors` auf "true" in Ihrem Code.
> 
>     ![Setzen Sie jQuery.support.cors nicht auf "true"](hubs-api-guide-javascript-client/_static/image7.png)
> 
>     SignalR behandelt die Verwendung von CORS. Festlegen von `jQuery.support.cors` auf "true" JSONP deaktiviert, da er bewirkt, dass SignalR anzunehmen, der Browser unterstützt CORS.
> - Wenn Sie eine Verbindung zu einer URL "localhost" herstellen, wird nicht Internet Explorer 10 eine Verbindung domänenübergreifende beachtet werden, damit die Anwendung lokal mit Internet Explorer 10 verwendet werden kann, selbst wenn Sie die domänenübergreifende Verbindungen auf dem Server aktiviert haben.
> - Weitere Informationen zur Verwendung von domänenübergreifende Verbindungen mit Internet Explorer 9, finden Sie unter [dieser Thread StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).
> - Informationen zur Verwendung von domänenübergreifende Verbindungen mit Chrome finden Sie unter [dieser Thread StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).
> - Im Beispielcode wird die Standardeinstellung "/ Signalr" die URL für die Verbindung mit dem SignalR-Dienst. Informationen über das Angeben einer anderen Basis-URL finden Sie unter [ASP.NET SignalR-Hubs API-Handbuch - Server - URL /signalr](hubs-api-guide-server.md#signalrurl).


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Gewusst wie: Konfigurieren der Verbindung

Bevor Sie eine Verbindung herstellen, können Sie Abfragezeichenfolgen-Parameter angeben oder geben Sie die Transportmethode.

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Gewusst wie: Angeben von Abfragezeichenfolgen-Parameter

Wenn Sie möchten Daten an den Server senden, wenn der Client eine Verbindung herstellt, können Sie Abfragezeichenfolgen-Parameter an das Verbindungsobjekt hinzufügen. Die folgenden Beispiele zeigen, wie einen Abfragezeichenfolgenparameter in Clientcode festgelegt.

**Einen Zeichenfolgenwert für die Abfrage vor dem Aufrufen der Start-Methode (mit den generierten Proxy) festgelegt**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

**Legen Sie einen Zeichenfolgenwert für die Abfrage vor dem Aufrufen der Start-Methode (ohne den generierten Proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample13.js?highlight=2)]

Im folgende Beispiel wird gezeigt, wie einen Abfragezeichenfolgenparameter in Servercode gelesen wird.

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample14.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Zum Angeben der Transportmethode

Im Rahmen des Prozesses eine Verbindung herstellen verhandelt SignalR-Client normalerweise mit dem Server zu den bewährten Transport ermitteln kann, der unterstützt wird, indem sowohl Server-als auch. Wenn Sie bereits wissen, dass der Transportmethode verwenden möchten, können Sie die dabei Aushandlung umgehen, indem die Transportmethode angeben, beim Aufrufen der `start` Methode.

**Clientcode, der angibt, die Transportmethode (mit den generierten Proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample15.js?highlight=1)]

**Clientcode, der angibt, die Transportmethode (ohne den generierten Proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample16.js?highlight=2)]

Als Alternative können Sie mehrere Transportmethoden in der Reihenfolge angeben, in denen Sie diese ausprobieren SignalR werden soll:

**Clientcode, der angibt, ein benutzerdefiniertes fallback Transportschema (mit den generierten Proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

**Clientcode, der angibt, ein benutzerdefiniertes fallback Transportschema (ohne den generierten Proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

Sie können die folgenden Werte zur Angabe der Transportmethode verwenden:

- "WebSockets"
- "ForeverFrame"
- "ServerSentEvents"
- "LongPolling"

Die folgenden Beispiele zeigen, wie Sie herausfinden, welche Transportmethode von eine Verbindung verwendet wird.

**Clientcode, der die Transportmethode, die von einer Verbindung (mit den generierten Proxy) verwendete zeigt**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample19.js?highlight=2)]

**Clientcode, der die Transportmethode, die von einer Verbindung (ohne den generierten Proxy) verwendete zeigt**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample20.js?highlight=3)]

Informationen zum Überprüfen der Transportmethode in Servercode finden Sie unter [Handbuch für ASP.NET SignalR-Hubs-API - Server - zum Abrufen von Informationen über den Client aus der Kontexteigenschaft](hubs-api-guide-server.md#contextproperty). Weitere Informationen zu Transporten und Zugriffe, finden Sie unter [Einführung in die SignalR - Transporte und Zugriffe](../getting-started/introduction-to-signalr.md#transports).

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a>Gewusst wie: Abrufen ein Proxys für eine hubklasse

Jedes Verbindungsobjekt, das Sie erstellen kapselt Informationen über eine Verbindung mit einer SignalR-Dienst, der eine oder mehrere Hub-Klassen enthält. Verwenden Sie für die Kommunikation mit einem hubklasse ein Proxyobjekt aus, die Sie erstellen selbst (sofern Sie nicht den generierten Proxy verwenden) oder der für Sie generiert.

Auf dem Client ist der Proxyname einer Version in Kamel-Schreibweise des Klassennamens Hub. SignalR verwendet diese Änderung automatisch, sodass JavaScript-Code den JavaScript-Konventionen entsprechen kann.

**Hubklasse auf server**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample21.cs?highlight=1)]

**Rufen Sie einen Verweis auf den generierten Client-Proxy für den Hub**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample22.js?highlight=1)]

**Erstellen des Clientproxys für den Hub-Klasse (ohne generierten Proxy)**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

Wenn Sie ergänzen die Hub-Klasse mit einem `HubName` -Attribut angegeben wird, verwenden Sie den genauen Namen ohne die Groß-/Kleinschreibung ändern.

**Hubklasse auf Server mit HubName-Attribut**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample24.cs?highlight=1)]

**Rufen Sie einen Verweis auf den generierten Client-Proxy für den Hub**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample25.js?highlight=1)]

**Erstellen des Clientproxys für den Hub-Klasse (ohne generierten Proxy)**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Zum Definieren von Methoden auf dem Client, die der Server aufrufen können

Um eine Methode definieren, die der Server mit einem Hub aufrufen können, fügen Sie einen Ereignishandler auf den Hub-Proxy mithilfe der `client` Eigenschaft des generierten Proxys oder der Aufruf der `on` Methode, wenn Sie den generierten Proxy verwenden. Die Parameter können komplexe Objekte sein.

Fügen Sie dem Ereignishandler hinzu, vor dem Aufrufen der `start` Methode zum Herstellen der Verbindung. (Wenn Sie nach dem Aufruf Ereignishandler hinzufügen möchten die `start` -Methode finden Sie im Hinweis in [zum Herstellen eine Verbindung](#establishconnection) weiter oben in diesem Dokument, und verwenden Sie die Syntax für die Definition einer Methode ohne Verwendung des generierten Proxys veranschaulicht.)

Methode-namenszuordnung wird Groß-/Kleinschreibung. Beispielsweise `Clients.All.addContosoChatMessageToPage` auf dem Server führt `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, oder `addcontosochatmessagetopage` auf dem Client.

**Definieren Sie die Methode auf dem Client (mit den generierten Proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample27.js?highlight=2)]

**Alternative Möglichkeit zum Definieren der Methode auf dem Client (mit den generierten Proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample28.js?highlight=1-2)]

**Definieren Sie die Methode auf dem Client (ohne den generierten Proxy oder beim Hinzufügen von nach dem Aufrufen der Start-Methode)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample29.js?highlight=3)]

**Server-Code, der die Methode aufruft.**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample30.cs?highlight=5)]

Im folgenden Beispiel werden ein komplexes Objekt als Parameter einer Methode enthalten.

**Definieren Sie die Methode auf dem Client, der ein komplexes Objekt (mit den generierten Proxy) akzeptiert.**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample31.js?highlight=2-3)]

**Definieren Sie die Methode auf dem Client, der ein komplexes Objekt (ohne den generierten Proxy) akzeptiert.**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample32.js?highlight=3-4)]

**Servercode, die das komplexe Objekt definiert.**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample33.cs?highlight=1)]

**Server-Code, der die Methode mithilfe eines komplexen Objekts aufruft.**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample34.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Gewusst wie: Aufrufen von Servermethoden vom client

Verwenden Sie zum Aufrufen einer Servermethode vom Client die `server` Eigenschaft des generierten Proxys oder `invoke` Methode für die hubproxy, wenn Sie den generierten Proxy verwenden. Der Rückgabewert oder Parameter können komplexe Objekte sein.

Eine Camel-Case-Version des Methodennamens auf dem Hub übergeben. SignalR verwendet diese Änderung automatisch, sodass JavaScript-Code den JavaScript-Konventionen entsprechen kann.

Den folgenden Beispielen wird veranschaulicht, wie eine Servermethode aufrufen, die nicht über einen Rückgabewert verfügt und eine Servermethode aufrufen, die einen Wert zurückgegeben.

**Server-Methode ohne HubMethodName-Attribut**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample35.cs?highlight=3)]

**Servercode, der definiert, das komplexe Objekt in einen Parameter übergeben**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample36.cs)]

**Clientcode, mit dem die Servermethode (mit den generierten Proxy) wird aufgerufen**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample37.js?highlight=1)]

**Clientcode, mit dem die Servermethode (ohne den generierten Proxy) wird aufgerufen**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample38.js?highlight=1)]

Wenn Sie ergänzt die Hub-Methode mit einem `HubMethodName` -Attribut angegeben wird, verwenden Sie diesen Namen, ohne die Groß-/Kleinschreibung ändern.

**Servermethode** mit einem HubMethodName-Attribut

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample39.cs?highlight=3)]

**Clientcode, mit dem die Servermethode (mit den generierten Proxy) wird aufgerufen**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

**Clientcode, mit dem die Servermethode (ohne den generierten Proxy) wird aufgerufen**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample41.js?highlight=1)]

Siehe vorstehende Beispiele zeigen, wie eine Servermethode aufrufen, die keinen zurückgibt Wert. Die folgenden Beispiele zeigen, wie eine Servermethode aufrufen, die über einen Rückgabewert verfügt.

**Servercode für eine Methode, die über einen Rückgabewert verfügt.**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample42.cs?highlight=3)]

**Die Stock-Klasse, die verwendet werden, für die** Rückgabewert

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample43.cs?highlight=1)]

**Clientcode, mit dem die Servermethode (mit den generierten Proxy) wird aufgerufen**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample44.js?highlight=2,4-5)]

**Clientcode, mit dem die Servermethode (ohne den generierten Proxy) wird aufgerufen**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample45.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Behandeln von Lebensdauer Verbindungsereignisse

SignalR bietet die folgenden Verbindung Lebensdauerereignisse, die Sie behandeln können:

- `starting`: Wird ausgelöst, bevor alle Daten über die Verbindung gesendet werden.
- `received`: Wird ausgelöst, wenn keine Daten für die Verbindung empfangen werden. Stellt die empfangenen Daten.
- `connectionSlow`: Wird ausgelöst, wenn der Client eine Verbindung langsame oder häufig löschen erkennt.
- `reconnecting`: Wird ausgelöst, wenn die zugrunde liegenden Transport erneut zu verbinden beginnt.
- `reconnected`: Wird ausgelöst, wenn die zugrunde liegenden Transport die Verbindung wiederhergestellt wurde.
- `stateChanged`: Wird ausgelöst, wenn der Verbindungsstatus ändert. Enthält der alte Status und den neuen Zustand (Herstellen einer Verbindung, verbunden, Reconnecting oder Disconnected).
- `disconnected`: Wird ausgelöst, wenn die Verbindung getrennt wurde.

Z. B. wenn sollen Warnmeldungen angezeigt wird, wenn Verbindungsprobleme, die merkliche Verzögerungen verursachen können, behandelt der `connectionSlow` Ereignis.

**Behandeln des Ereignisses von ConnectionSlow (mit den generierten Proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample46.js?highlight=1)]

**Behandeln des Ereignisses ConnectionSlow (ohne den generierten Proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample47.js?highlight=2)]

Weitere Informationen finden Sie unter [verstehen und Behandeln von Ereignissen für Lebensdauer in SignalR](handling-connection-lifetime-events.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Gewusst wie: Behandeln von Fehlern

Der SignalR JavaScript-Client stellt eine `error` Ereignis, das Sie einen Handler für hinzufügen können. Sie können auch die Fail-Methode verwenden, einen Handler für Fehler hinzufügen, die aus einem Methodenaufruf Server resultieren.

Wenn Sie ausführliche Fehlermeldungen auf dem Server nicht explizit aktivieren, enthält das Ausnahmeobjekt, das SignalR nach einem Fehler gibt wenige Informationen über den Fehler. Angenommen, wenn ein Aufruf von `newContosoChatMessage` ein Fehler auftritt, der die Fehlermeldung in die Error-Objekt enthält "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" senden, detaillierte Fehlermeldungen für Clients in einer produktionsumgebung wird nicht empfohlen aus Sicherheitsgründen aber wenn Sie möchten detaillierte Fehlermeldungen für aktivieren Problembehandlung, verwenden Sie folgenden Code auf dem Server.

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample48.cs?highlight=2)]

Das folgende Beispiel zeigt, wie einen Handler für das Fehlerereignis hinzufügen.

**Fügen Sie einen Fehlerhandler (mit den generierten Proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample49.js?highlight=1)]

**Hinzufügen eines fehlerhandlers (ohne den generierten Proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample50.js?highlight=2)]

Im folgende Beispiel wird gezeigt, wie einen Fehler von einem Methodenaufruf behandelt wird.

**Behandeln Sie einen Fehler aus einem Methodenaufruf (mit den generierten Proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample51.js?highlight=2)]

**Behandeln Sie einen Fehler aus einem Methodenaufruf (ohne den generierten Proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

Ausfall ein Methodenaufrufs der `error` Ereignis wird auch ausgelöst, sodass Code in der `error` Methodenhandler und klicken Sie in der `.fail` würde dieser Rückruf Methode ausgeführt.

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Clientseitige Protokollierung aktivieren

Um die clientseitige Protokollierung für eine Verbindung zu aktivieren, legen Sie die `logging` Eigenschaft für das Verbindungsobjekt vor dem Aufrufen der `start` Methode zum Herstellen der Verbindung.

**Aktivieren Sie die Protokollierung (mit den generierten Proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample53.js?highlight=1)]

**Aktivieren Sie die Protokollierung (ohne den generierten Proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

Um die Protokolle anzuzeigen, öffnen Sie Entwicklertools von Ihrem Browser, und wechseln Sie zur Registerkarte "Console". Ein Lernprogramm, schrittweise Anweisungen und Bildschirm Screenshots enthält, die Vorgehensweise veranschaulichen, finden Sie unter [Broadcast-Server mit ASP.NET Signalr - Enable Logging](../getting-started/tutorial-server-broadcast-with-signalr.md#enablelogging).
