---
uid: signalr/overview/older-versions/troubleshooting
title: Problembehandlung für SignalR (SignalR 1.x) | Microsoft Docs
author: pfletcher
description: Dieser Artikel beschreibt häufig auftretende Probleme mit der Entwicklung von SignalR-Anwendungen.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/05/2013
ms.topic: article
ms.assetid: 347210ba-c452-4feb-886f-b51d89f58971
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: eb739f806eb57d09008fa0bf04b5a40721dab73d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="signalr-troubleshooting-signalr-1x"></a>Problembehandlung für SignalR (SignalR 1.x)
====================
durch [Patrick Fletcher](https://github.com/pfletcher)

> Dieses Dokument beschreibt häufig auftretender Probleme mit SignalR.


Dieses Dokument enthält die folgenden Abschnitte.

- [Aufrufen von Methoden, die zwischen Client und Server im Hintergrund ein Fehler auftritt](#connection)
- [Andere Verbindungsprobleme](#other)
- [Fehler bei der Kompilierung und serverseitige](#server)
- [Visual Studio-Probleme](#vs)
- [Internetinformationsdienste (IIS)-Probleme](#iis)
- [Azure-Probleme](#azure)

<a id="connection"></a>

## <a name="calling-methods-between-the-client-and-server-silently-fails"></a>Aufrufen von Methoden, die zwischen Client und Server im Hintergrund ein Fehler auftritt

Dieser Abschnitt beschreibt mögliche Ursachen für einen Methodenaufruf zwischen Client und Server ohne sinnvolle Fehlermeldung fehl. In einer SignalR-Anwendung hat der Server keine Informationen über die Methoden, die vom Client implementierten; Wenn der Server eine Clientmethode aufruft, Methodendaten Namen und Parameter werden an den Client gesendet, und die Methode ausgeführt wird, nur dann, wenn es in das Format vorhanden ist, die der Server angegeben. Wenn auf dem Client keine übereinstimmende Methode gefunden wird, geschieht nichts, und auf dem Server wird keine Fehlermeldung ausgegeben.

Zur Clientmethoden nicht aufgerufen, Abrufen von weiteren Untersuchung zu können, können Sie Protokollierung vor dem Aufrufen die Start-Methode auf den Hub, um welche Aufrufe finden Sie unter stammen aus dem Server aktivieren. Zum Aktivieren der Protokollierung in einer JavaScript-Anwendung finden Sie unter [aktivieren die clientseitige Protokollierung (JavaScript-Clientversion)](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging). Zum Aktivieren der Protokollierung in einer .NET Client-Anwendung, finden Sie unter [aktivieren die clientseitige Protokollierung (.NET Client-Version)](../guide-to-the-api/hubs-api-guide-net-client.md#logging).

### <a name="misspelled-method-incorrect-method-signature-or-incorrect-hub-name"></a>Falsch geschriebene Methode, falschen Methodensignatur oder falsche Hub-name

Wenn Sie den Namen oder die Signatur einer aufgerufene Methode nicht genau mit eine geeignete Methode auf dem Client übereinstimmt, schlägt der Aufruf fehl. Stellen Sie sicher, dass der vom Server aufgerufene Methode den Namen der Methode auf dem Client übereinstimmt. SignalR erstellt außerdem die hubproxy mithilfe der Methoden in Kamel-Schreibweise wie in JavaScript geeignet ist, also eine Methode namens `SendMessage` auf dem Server aufgerufen `sendMessage` in den Clientproxy. Bei Verwendung der `HubName` -Attribut im serverseitigen Code, stellen Sie sicher, dass der Name, der zum Erstellen des Hubs auf dem Client übereinstimmt. Wenn Sie nicht verwenden die `HubName` -Attribut, stellen Sie sicher, dass der Name des Hubs in einem JavaScript-Client in Kamel-Schreibweise, z. B. ChatHub statt ChatHub.

### <a name="duplicate-method-name-on-client"></a>Doppelte Methodennamen auf client

Stellen Sie sicher, dass Sie eine doppelte Methode nicht auf dem Client verfügen, die sich nur in Groß-bzw. Kleinschreibung unterscheidet. Verfügt Ihre Client-Anwendung eine Methode namens `sendMessage`, stellen Sie sicher, dass es nicht auch eine Methode namens `SendMessage` ebenfalls.

### <a name="missing-json-parser-on-the-client"></a>Fehlender JSON-Parser auf dem client

SignalR benötigt einen JSON-Parser zum Serialisieren der Aufrufe zwischen dem Server und dem Client vorhanden sein. Wenn der Client einen integrierten JSON-Parser (z. B. Internet Explorer 7) besitzt, müssen Sie in Ihrer Anwendung enthalten. Sie können die JSON-Parser herunterladen [hier](http://nuget.org/packages/json2).

### <a name="mixing-hub-and-persistentconnection-syntax"></a>Gemischte Verwendung von Hub- und "persistentconnection"-syntax

SignalR verwendet zwei Kommunikationsmodelle: Hubs und PersistentConnections. Die Syntax zum Aufrufen dieser beiden Kommunikationsmodelle unterscheidet sich im Clientcode. Wenn Sie einen Hub in Ihrem Servercode hinzugefügt haben, stellen Sie sicher, dass alle Clientcode die richtigen Hub-Syntax verwendet.

**JavaScript-Clientcode, der in einem JavaScript-Client eine "persistentconnection" wird erstellt**

[!code-javascript[Main](troubleshooting/samples/sample1.js)]

**JavaScript-Clientcode, der einen Hub-Proxy in einem Javascript-Client erstellt**

[!code-javascript[Main](troubleshooting/samples/sample2.js)]

**C#-Server-Code, der eine Route einer "persistentconnection" zugeordnet**

[!code-csharp[Main](troubleshooting/samples/sample3.cs)]

**C#-Server-Code, der eine Route mit einem Hub oder auf mehreren Hubs zugeordnet, wenn Sie mehrere Anwendungen haben**

[!code-csharp[Main](troubleshooting/samples/sample4.cs)]

### <a name="connection-started-before-subscriptions-are-added"></a>Verbindung gestartet, bevor Abonnements hinzugefügt werden

Wenn der Hub-Verbindung gestartet wurde, bevor Methoden, die vom Server aufgerufen werden können, die auf den Proxy hinzugefügt werden, werden keine Nachrichten empfangen werden. Der folgende JavaScript-Code kann nicht den Hub ordnungsgemäß gestartet werden:

**Falsche JavaScript-Clientcode, der keine Hubs, die zu empfangenden Nachrichten zulassen**

[!code-javascript[Main](troubleshooting/samples/sample5.js)]

Fügen Sie stattdessen die Methode Abonnements vor dem Aufrufen der Start hinzu:

**JavaScript-Clientcode, die Abonnements mit einem Hub ordnungsgemäß hinzufügt**

[!code-javascript[Main](troubleshooting/samples/sample6.js)]

### <a name="missing-method-name-on-the-hub-proxy"></a>Fehlende Methodennamen auf dem hubproxy

Stellen Sie sicher, dass die auf dem Server definierte Methode auf dem Client abonniert wird. Obwohl der Server die Methode definiert, muss es weiterhin den Clientproxy hinzugefügt. Methoden können den Clientproxy auf folgende Weise hinzugefügt werden (Beachten Sie, das die Methode hinzugefügt wird, die `client` Member des Hubs, nicht den Hub direkt):

**JavaScript-Clientcode, der eine hubproxy Methoden hinzufügt**

[!code-javascript[Main](troubleshooting/samples/sample7.js)]

### <a name="hub-or-hub-methods-not-declared-as-public"></a>Hub oder hubmethoden, die nicht als öffentlich deklariert.

Um auf dem Client sichtbar zu sein, müssen als der Hub-Implementierung und Methoden deklariert werden `public`.

### <a name="accessing-hub-from-a-different-application"></a>Beim Zugriff auf den Hub aus einer anderen Anwendung

SignalR-Hubs können nur über Anwendungen zugegriffen werden, die SignalR-Clients zu implementieren. SignalR kann nicht mit anderen Bibliotheken Kommunikation (wie SOAP oder WCF-Webdienste.) zusammenarbeiten. Wenn keine SignalR-Client für die Zielplattform verfügbar ist, können nicht Sie das Server-Endpunkt direkt zugreifen.

### <a name="manually-serializing-data"></a>Manuell Serialisieren von Daten

SignalR werden automatisch JSON verwendet zum Serialisieren Ihrer Methode Parameter-dort nicht erforderlich, selbst ausführen.

### <a name="remote-hub-method-not-executed-on-client-in-ondisconnected-function"></a>Hub-Remotemethode auf Clients in OnDisconnected-Funktion nicht ausgeführt

Dieses Verhalten ist vorgesehen. Wenn `OnDisconnected` wird aufgerufen, die Hub bereits eingegeben der `Disconnected` Status, die hubmethoden, die aufgerufen wird, werden keine weiteren zulässt.

**C#-Servercode, der Code im Ereignis OnDisconnected ordnungsgemäß ausführt**

[!code-csharp[Main](troubleshooting/samples/sample8.cs)]

### <a name="connection-limit-reached"></a>Verbindungslimit erreicht

Wenn Sie die Vollversion von IIS auf einem Clientbetriebssystem wie Windows 7 verwenden, ist ein 10-Verbindung Beschränkung. Wenn Sie ein Clientbetriebssystem verwenden zu können, verwenden Sie IIS Express stattdessen dieses Limit zu vermeiden.

### <a name="cross-domain-connection-not-set-up-properly"></a>Domänenübergreifende-Verbindung nicht ordnungsgemäß eingerichtet ist

Wenn eine domänenübergreifende-Verbindung (eine Verbindung für die die SignalR-URL nicht in der gleichen Domäne wie die hosting-Seite ist) nicht ordnungsgemäß eingerichtet, die Verbindung möglicherweise fehl, ohne eine Fehlermeldung angezeigt. Informationen zum domänenübergreifenden Kommunikation zu ermöglichen, finden Sie unter [Gewusst wie: Herstellen einer Verbindung domänenübergreifende](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).

### <a name="connection-using-ntlm-active-directory-not-working-in-net-client"></a>Verbindung mithilfe von NTLM (Active Directory) funktioniert nicht in .NET client

Eine Verbindung in einer .NET Client-Anwendung, die Sicherheit der Domäne verwendet kann fehlschlagen, wenn die Verbindung nicht ordnungsgemäß konfiguriert ist. Um SignalR in einer domänenumgebung zu verwenden, legen Sie die erforderlichen Verbindungseigenschaft wie folgt:

**C#-Clientcode, der Anmeldeinformationen für die Verbindung implementiert.**

[!code-csharp[Main](troubleshooting/samples/sample9.cs)]

<a id="other"></a>

## <a name="other-connection-issues"></a>Andere Verbindungsprobleme

In diesem Abschnitt wird beschrieben, die Ursachen und Lösungen für bestimmte Symptome oder Fehlermeldungen, die während einer Verbindung auftreten.

### <a name="start-must-be-called-before-data-can-be-sent-error"></a>Fehler "Start muss aufgerufen werden, bevor Daten gesendet werden können"

Dieser Fehler wird häufig angezeigt, wenn Code SignalR-Objekten verweist, bevor die Verbindung gestartet wird. Das Verbinden für Ereignishandler und Like, dass Methoden aufrufen, die auf dem Server definierten hinzugefügt werden muss, nach Abschluss die Verbindung. Beachten Sie, dass der Aufruf von `Start` ist asynchron, damit der Code nach der Aufruf kann, bevor er ausgeführt werden abgeschlossen ist. Die beste Möglichkeit, fügen Sie Ereignishandler hinzu, nachdem eine Verbindung vollständig gestartet darin, diese in eine Rückruffunktion, die als Parameter an die Start-Methode übergeben wird:

**JavaScript-Clientcode ordnungsgemäß-Ereignishandler hinzu, die SignalR-Objekte zu verweisen**

[!code-javascript[Main](troubleshooting/samples/sample10.js?highlight=1)]

Dieser Fehler wird ebenfalls angezeigt werden, wenn eine Verbindung beendet wird, während die SignalR-Objekte immer noch verwiesen wird.

### <a name="301-moved-permanently-or-302-moved-temporarily-error"></a>"301 Permanent verschoben" oder "302 vorübergehend verschoben"-Fehler

Dieser Fehler kann auftreten, wenn das Projekt enthält einen Ordner namens SignalR, die automatisch erstellte Proxy behindern wird. Um diesen Fehler zu vermeiden, verwenden Sie keinen Ordner namens `SignalR` in Ihrer Anwendung, oder aktivieren Sie automatische Proxygenerierung deaktiviert. Finden Sie unter [der generierten Proxy und wozu Sie](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy) Weitere Details.

### <a name="403-forbidden-error-in-net-or-silverlight-client"></a>"403 – Verboten" Fehler im .NET- oder Silverlight-client

Dieser Fehler kann in domänenübergreifenden Umgebungen auftreten, in denen domänenübergreifende Kommunikation nicht ordnungsgemäß aktiviert ist. Informationen zum domänenübergreifenden Kommunikation zu ermöglichen, finden Sie unter [Gewusst wie: Herstellen einer Verbindung domänenübergreifende](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain). Um eine domänenübergreifende Verbindung in einem Silverlight-Client herzustellen, finden Sie unter [domänenübergreifende Verbindungen von Silverlight-Clients](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain).

### <a name="404-not-found-error"></a>Fehler "404 nicht gefunden"

Es gibt mehrere Ursachen für dieses Problem. Überprüfen Sie die folgenden:

- **Hub-Proxy-Adresse Verweis nicht korrekt formatiert:** dieser Fehler wird häufig angezeigt, wenn der Verweis auf die generierten Hub Proxyadresse nicht ordnungsgemäß formatiert ist. Stellen Sie sicher, dass der Verweis auf die Adresse Hub ordnungsgemäß erstellt wurde. Finden Sie unter [wie der dynamisch generierten Proxy Verweise](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy) Details.
- **Hinzufügen von Routen für die Anwendung vor dem Hinzufügen der Route Hub:** Wenn Ihre Anwendung andere Routen verwendet wird, überprüfen Sie, ob die erste Route hinzugefügt der Aufruf von `MapHubs`.

### <a name="500-internal-server-error"></a>"500 Interner Serverfehler"

Dies ist ein sehr generischer Fehler, der eine Vielzahl von Ursachen haben kann. Die Details des Fehlers sollte angezeigt werden, im Ereignisprotokoll des Servers oder über das Debuggen des Servers befinden. Ausführlichere Fehlerinformationen kann durch das Einschalten ausführliche Fehler auf dem Server abgerufen werden. Weitere Informationen finden Sie unter [Gewusst wie: Behandeln von Fehlern in die hubklasse](../guide-to-the-api/hubs-api-guide-server.md#handleErrors).

### <a name="typeerror-lthubtypegt-is-undefined-error"></a>"TypeError: &lt;" HubType "festgelegt&gt; ist nicht definiert" Fehler

Dieser Fehler führt, wenn der Aufruf von `MapHubs` erfolgt nicht ordnungsgemäß. Finden Sie unter [die SignalR-Route zu registrieren und Konfigurieren von SignalR-Optionen](../guide-to-the-api/hubs-api-guide-server.md#route) für Weitere Informationen.

### <a name="jsonserializationexception-was-unhandled-by-user-code"></a>JsonSerializationException wurde nicht vom Benutzercode behandelt.

Stellen Sie sicher, dass die Parameter, die Sie an die Methoden senden nicht serialisierbarer Typen (z. B. Dateihandles oder Datenbankverbindungen) nicht enthalten. Wenn die gewünschte Member für einen serverseitigen Objekts verwenden, die nicht an den Client (entweder aus Sicherheitsgründen oder aufgrund von Serialisierung), verwenden gesendet werden sollen die `JSONIgnore` Attribut.

### <a name="protocol-error-unknown-transport-error"></a>"Protokollfehler: Unbekannter Transport" Fehler

Dieser Fehler kann auftreten, wenn der Client die Transporte, die SignalR verwendet, nicht unterstützt wird. Finden Sie unter [Transporte und Zugriffe](../getting-started/introduction-to-signalr.md#transports) Informationen, die auf dem Browser mit SignalR verwendet werden können.

### <a name="javascript-hub-proxy-generation-has-been-disabled"></a>"JavaScript Hub-Proxygenerierung wurde deaktiviert."

Dieser Fehler tritt auf, wenn `DisableJavaScriptProxies` festgelegt ist, während Sie auch einen Verweis auf den dynamisch generierten Proxy am `signalr/hubs`. Weitere Informationen zu den Proxy manuell erstellen, finden Sie unter [des generierten Proxys und wozu Sie](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy).

### <a name="the-connection-id-is-in-the-incorrect-format-or-the-user-identity-cannot-change-during-an-active-signalr-connection-error"></a>"Die Verbindungs-ID ist im falschen Format" oder "die Identität des Benutzers nicht ändern, während einer aktiven SignalR-Verbindungs" Fehler

Dieser Fehler kann auftreten, wenn die Authentifizierung verwendet wird, und der Client wird abgemeldet, bevor die Verbindung beendet wird. Die Lösung besteht darin, die SignalR-Verbindung, bevor der Client die Abmeldung zu beenden.

### <a name="uncaught-error-signalr-jquery-not-found-please-ensure-jquery-is-referenced-before-the-signalrjs-file-error"></a>"Unerwarteter Fehler: SignalR: jQuery wurde nicht gefunden. Stellen Sie sicher, dass jQuery referenziert wird, bevor Sie die Datei SignalR.js"-Fehler

Der SignalR JavaScript-Client benötigt jQuery ausgeführt. Stellen Sie sicher, dass der Verweis auf jQuery richtig ist, dass der verwendete Pfad gültig ist und dass der Verweis auf jQuery vor dem Verweis auf SignalR ist.

### <a name="uncaught-typeerror-cannot-read-property-ltpropertygt-of-undefined-error"></a>"Nicht abgefangene TypeError: Eigenschaft kann nicht gelesen werden."&lt;Eigenschaft&gt;"undefined" Fehler

Dieser Fehler ergibt sich aus jQuery oder Hubs Proxy ordnungsgemäß verwiesen werden muss. Stellen Sie sicher, dass der Verweis auf jQuery und der Proxy Hubs richtig ist, dass der verwendete Pfad gültig ist und dass der Verweis auf jQuery vor dem Verweis auf den Proxy Hubs ist. Der Standardverweis auf die Hubs Proxy sollte wie folgt aussehen:

**Die clientseitige HTML-Code, die auf den Proxy Hubs ordnungsgemäß verweist**

[!code-html[Main](troubleshooting/samples/sample11.html)]

### <a name="runtimebinderexception-was-unhandled-by-user-code-error"></a>Fehler "RuntimeBinderException wurde nicht vom Benutzercode behandelt"

Dieser Fehler kann auftreten, wenn die falsche Überladung der `Hub.On` verwendet wird. Wenn die Methode einen Rückgabewert verfügt, muss der Rückgabetyp als einen generischen Typparameter angegeben werden:

**Auf dem Client (ohne generierten Proxy) definierte Methode**

[!code-html[Main](troubleshooting/samples/sample12.html?highlight=1)]

### <a name="connection-id-is-inconsistent-or-connection-breaks-between-page-loads"></a>Verbindungs-ID ist inkonsistent oder Verbindungsabbruch zwischen Seite geladen

Dieses Verhalten ist vorgesehen. Da der Hub-Objekt in der Page-Objekt gehostet wird, wird die Hubs zerstört, sobald die Seitenansicht aktualisiert. Eine Anwendung mit mehreren Seite muss die Zuordnung zwischen Benutzern und Verbindungs-IDs zu verwalten, damit sie konsistent Seite geladen werden. Der Verbindungs-IDs kann gespeichert werden, auf dem Server entweder in eine `ConcurrentDictionary` Objekts oder einer Datenbank.

### <a name="value-cannot-be-null-error"></a>Fehler "Wert darf nicht null sein"

Serverseitige Methoden mit optionalen Parametern werden derzeit nicht unterstützt. Wenn der optionale Parameter weggelassen wird, schlägt die Methode fehl. Weitere Informationen finden Sie unter [optionale Parameter](https://github.com/SignalR/SignalR/issues/324).

### <a name="firefox-cant-establish-a-connection-to-the-server-at-ltaddressgt-error-in-firebug"></a>"Firefox kann keine Verbindung mit dem Server herstellen &lt;Adresse&gt;" Fehler im Firebug

Diese Fehlermeldung kann im Firebug angezeigt werden, wenn der WebSocket-Transport ausgehandelt schlägt fehl, und einen anderen Transport wird stattdessen verwendet. Dieses Verhalten ist vorgesehen.

### <a name="the-remote-certificate-is-invalid-according-to-the-validation-procedure-error-in-net-client-application"></a>"Das Remotezertifikat ist laut Validierungsverfahren ungültig" Fehler in .NET Client-Anwendung

Wenn der Server benutzerdefinierte Clientzertifikate erfordert, können Sie eine x509certificate für die Verbindung hinzufügen, bevor die Anforderung erfolgt. Fügen Sie das Zertifikat mit der Verbindung `Connection.AddClientCertificate`.

### <a name="connection-drops-after-authentication-times-out"></a>Verbindung gelöscht, nachdem die Authentifizierung ein Timeout auftritt

Dieses Verhalten ist vorgesehen. Anmeldeinformationen für die Authentifizierung können nicht geändert werden, während eine Verbindung aktiv ist; um Anmeldeinformationen zu aktualisieren, muss die Verbindung beendet und neu gestartet werden.

### <a name="onconnected-gets-called-twice-when-using-jquery-mobile"></a>OnConnected wird aufgerufen, zweimal Verwendung von jQuery Mobile

jQuery Mobile `initializePage` -Funktion erzwingt die Skripts auf den einzelnen Seiten erneut ausgeführt werden, wodurch eine zweite Verbindung. Lösungen für dieses Problem sind:

- Schließen Sie den Verweis auf jQuery Mobile, bevor Sie die JavaScript-Datei.
- Deaktivieren der `initializePage` Funktion durch Festlegen von `$.mobile.autoInitializePage = false`.
- Warten Sie, bis die Seite, um den Vorgang abzuschließen, initialisieren vor dem Starten der Verbindung.

### <a name="messages-are-delayed-in-silverlight-applications-using-server-sent-events"></a>Nachrichten werden in Silverlight-Anwendungen, die mithilfe von Server gesendete Ereignisse verzögert.

Nachrichten werden verzögert, wenn Ereignisse mit der Server auf Silverlight gesendet werden. Um lange des Abrufs zum stattdessen verwendet werden zu erzwingen, verwenden Sie die folgende, beim Starten der Verbindungs:

[!code-css[Main](troubleshooting/samples/sample13.css)]

### <a name="permission-denied-using-forever-frame-protocol"></a>Mithilfe von "Berechtigung verweigert" Forever Frame-Protokoll

Dies ist ein bekanntes Problem, beschriebenen [hier](https://github.com/SignalR/SignalR/issues/1963). Dieses Problem kann auftreten, mithilfe der neuesten JQuery-Bibliothek; die problemumgehung besteht darin Ihre Anwendung JQuery 1.8.2 herabstufen.

<a id="server"></a>

## <a name="compilation-and-server-side-errors"></a>Fehler bei der Kompilierung und serverseitige

 Der folgende Abschnitt enthält die mögliche Lösungen für Compiler und serverseitige Laufzeitfehler. 

### <a name="reference-to-hub-instance-is-null"></a>Verweis auf den Hub-Instanz ist null.

Da eine hubinstanz für jede Verbindung erstellt wird, können nicht Sie im Code selbst eine Instanz von einem Hub erstellen. Zum Aufrufen von Methoden für einen Hub von außerhalb der Hub selbst finden Sie unter [wie Client-Methoden aufrufen und Verwalten von Gruppen von außerhalb der Klasse Hub](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub) zum Abrufen eines Verweises auf den hubkontext.

### <a name="httpcontextcurrentsession-is-null"></a>HTTPContext.Current.Session ist null.

Dieses Verhalten ist vorgesehen. SignalR unterstützt nicht den ASP.NET-Sitzungsstatus, weil durch Aktivieren des Sitzungsstatus duplexnachrichten unterbrochen würde.

### <a name="no-suitable-method-to-override"></a>Keine geeignete Methode zum Überschreiben

Dieser Fehler kann angezeigt werden, wenn Sie Code aus älteren-Dokumentation oder Blogs verwenden. Stellen Sie sicher, dass Sie keine Namen von Methoden verweisen, die als veraltet markiert oder geändert wurden (z. B. `OnConnectedAsync`).

### <a name="hostcontextextensionswebsocketserverurl-is-null"></a>HostContextExtensions.WebSocketServerUrl ist null.

Dieses Verhalten ist vorgesehen. Dieser Member ist veraltet und sollte nicht verwendet werden.

### <a name="a-route-named-signalrhubs-is-already-in-the-route-collection-error"></a>Fehler "eine Route mit dem Namen"signalr.hubs"ist bereits in der routenauflistung"

Dieser Fehler wird angezeigt, wenn `MapHubs` zweimal von der Anwendung aufgerufen wird. Einige Beispiel-Anwendungen rufen `MapHubs` direkt in der globalen Anwendungsdatei; andere führen Sie den Aufruf in eine Wrapperklasse. Stellen Sie sicher, dass Ihre Anwendung nicht beide Aufgaben.

<a id="vs"></a>

## <a name="visual-studio-issues"></a>Visual Studio-Probleme

Dieser Abschnitt beschreibt auftretende Probleme in Visual Studio.

### <a name="script-documents-node-does-not-appear-in-solution-explorer"></a>Knoten "Dokumente" Skript wird im Projektmappen-Explorer nicht angezeigt.

Einige der unseren Tutorials, weisen Sie auf den Knoten "Skriptdokumente" im Projektmappen-Explorer während des Debuggens. Dieser Knoten wird erzeugt, indem der JavaScript-Debugger und wird nur während des Debuggens von Browser-Clients im Internet Explorer angezeigt; der Knoten wird nicht angezeigt, wenn Chrome oder Firefox verwendet werden. Der JavaScript-Debugger wird auch nicht ausgeführt werden, wenn ein anderer Client-Debugger, z. B. der Silverlight-Debugger ausgeführt wird.

### <a name="signalr-does-not-work-on-visual-studio-2008-or-earlier"></a>SignalR funktioniert nicht für Visual Studio 2008 oder früher

Dieses Verhalten ist vorgesehen. SignalR erfordert .NET Framework 4 oder höher. Dies erfordert, dass die SignalR-Anwendungen in Visual Studio 2010 oder höher entwickelt werden.

<a id="iis"></a>

## <a name="iis-issues"></a>IIS-Problemen

Dieser Abschnitt enthält die Probleme mit Internetinformationsdienste (IIS).

### <a name="web-site-crashes-after-maphubs-call"></a>Website stürzt ab, nach dem Aufruf von MapHubs

Dieses Problem wurde in der neuesten Version von SignalR behoben. Stellen Sie sicher, dass Sie die neueste veröffentlichte Version der SignalR durch Aktualisieren Ihrer Installation mithilfe von NuGet verwenden.

<a id="azure"></a>

## <a name="azure-issues"></a>Azure-Probleme

Dieser Abschnitt enthält die Probleme mit Microsoft Azure.

### <a name="messages-are-not-received-through-the-azure-backplane-after-altering-topic-names"></a>Nachrichten werden nicht über die Azure Backplane empfangen, nach Thema Namen ändern

In den Themen, die von der Azure-Rückwandplatine verwendet werden sollen nicht BenutzerKonfigurierbar.
