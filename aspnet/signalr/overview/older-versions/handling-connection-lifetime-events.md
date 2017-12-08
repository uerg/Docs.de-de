---
uid: signalr/overview/older-versions/handling-connection-lifetime-events
title: Verstehen und Behandeln von Lebensdauer Verbindungsereignisse in SignalR 1.x | Microsoft Docs
author: pfletcher
description: "Dieser Artikel beschreibt, wie von der Hubs-API verfügbar gemachten Ereignisse beschrieben."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/05/2013
ms.topic: article
ms.assetid: e608e263-264d-448b-b0eb-6eeb77713b22
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/handling-connection-lifetime-events
msc.type: authoredcontent
ms.openlocfilehash: db29c3382895ef4d7efc3a686fa558189c8788de
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="understanding-and-handling-connection-lifetime-events-in-signalr-1x"></a>Verstehen und Behandeln von Lebensdauer Verbindungsereignisse in SignalR 1.x
====================
durch [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

> Dieser Artikel enthält eine Übersicht über die SignalR erneuten Herstellen einer Verbindung und die Trennung der Ereignisse, die Sie behandeln können, und Timeouts und Keepalive-Einstellungen, die Sie konfigurieren können.
> 
> Der Artikel wird davon ausgegangen, dass Sie bereits einige Kenntnisse SignalR und Lebensdauer Verbindungsereignisse haben. Eine Einführung zu SignalR finden Sie unter [SignalR - Übersicht – erste Schritte](index.md). Listen von Verbindungsereignissen-Lebensdauer finden Sie unter den folgenden Ressourcen:
> 
> - [Behandeln von Lebensdauer Verbindungsereignisse in der Hub-Klasse](index.md)
> - [Behandeln von Lebensdauer Verbindungsereignisse in JavaScript-clients](index.md)
> - [Behandeln von Lebensdauer Verbindungsereignisse in .NET clients](index.md)


## <a name="overview"></a>Übersicht

Dieser Artikel enthält folgende Abschnitte:

- [Verbindung Lebensdauer Terminologie und Szenarien](#terminology)

    - [SignalR-Verbindungen, transportverbindungen und physische Verbindungen](#signalrvstransport)
    - [Transport Trennung Szenarien](#transportdisconnect)
    - [Trennung von Clientszenarien](#clientdisconnect)
    - [Szenarien für die Trennung von Server](#serverdisconnect)
- [Einstellungen für Timeouts und "Keepalive"](#timeoutkeepalive)

    - [ConnectionTimeout](#connectiontimeout)
    - ["Disconnecttimeout"](#disconnecttimeout)
    - ["Keepalive"](#keepalive)
    - [So ändern Sie die Einstellungen für Timeouts und "Keepalive"](#changetimeout)
- [Gewusst wie: Benachrichtigung des Benutzers zu trennen](#notifydisconnect)
- [Wiederherstellen der Verbindung fortlaufend](#continuousreconnect)
- [Einen Client im Servercode Trennen der Verbindung](#disconnectclientfromserver)

Links zu API-Referenzthemen sind auf die .NET 4.5-Version der API. Wenn Sie .NET 4 verwenden, finden Sie unter [.NET 4-Version der API-Themen](https://msdn.microsoft.com/en-us/library/jj891075(v=vs.100).aspx).

<a id="terminology"></a>

## <a name="connection-lifetime-terminology-and-scenarios"></a>Verbindung Lebensdauer Terminologie und Szenarien

Die `OnReconnected` -Ereignishandler in einer SignalR-Hubs kann direkt nach dem Ausführen `OnConnected` aber nicht nach `OnDisconnected` für einen angegebenen Client. Der Grund, dass Sie eine erneute Verbindung ohne eine Trennung haben können ist, dass es gibt mehrere Möglichkeiten, in denen das Wort "Verbindung", die in SignalR verwendet wird.

<a id="signalrvstransport"></a>

### <a name="signalr-connections-transport-connections-and-physical-connections"></a>SignalR-Verbindungen, transportverbindungen und physische Verbindungen

In diesem Artikel wird unterschieden zwischen *SignalR-Verbindungen*, *transport Verbindungen*, und *physische Verbindungen*:

- **SignalR-Verbindung** bezieht sich auf eine logische Beziehung zwischen einem Client und Server-URL, von der SignalR-API verwaltet und durch eine Verbindungs­ID eindeutig identifiziert. Die Daten über diese Beziehung werden von SignalR verwaltet und werden verwendet, um keine transportverbindung herstellen. Die Beziehungsenden und SignalR verwirft der Daten, wenn der Client Ruft die `Stop` -Methode oder ein Zeitlimit erreicht ist, während SignalR versucht, eine transportverbindung getrennt wiederherstellen.
- **Transportieren von Verbindung** bezieht sich auf eine logische Beziehung zwischen einem Client und einem Server, die von einer der vier Transport APIs verwaltet: WebSockets, vom Server gesendeten Ereignisse forever frame oder lange abrufen. SignalR verwendet den Transport-API, um eine transportverbindung zu erstellen, und die Transport-API, hängt das Vorhandensein einer physischen Netzwerk-Verbindung für die transportverbindung zu erstellen. Die transportverbindung endet, wenn SignalR diese beendet wird oder wenn der Transport API erkennt, dass die physische Verbindung unterbrochen wird.
- **Physische Verbindung** bezieht sich auf die physischen Netzwerklinks--Kabel, WLAN-Signale, Router usw. –, die die Kommunikation zwischen einem Clientcomputer und einem Servercomputer zu erleichtern. Muss die physische Verbindung vorhanden, damit keine transportverbindung herstellen, und eine transportverbindung hergestellt werden muss, um eine SignalR-Verbindung herstellen. Allerdings endet unterbrechen die physische Verbindung immer sofort die transportverbindung oder SignalR-Verbindung nicht wie weiter unten in diesem Thema erläutert werden.

In der folgenden Abbildung die SignalR-Verbindung wird durch die Hubs-API und die Ebene "persistentconnection" API SignalR dargestellt, die transportverbindung wird durch die Transporte Ebene dargestellt und die physische Verbindung wird dargestellt, durch die Linien zwischen dem server und den Clients.

![SignalR-Architekturdiagramm](handling-connection-lifetime-events/_static/image1.png)

Beim Aufrufen der `Start` Methode in einer SignalR-Client bereitgestellt SignalR-Clientcode mit den Informationen, die es benötigt, um eine physische Verbindung mit einem Server herzustellen. SignalR-Clientcode verwendet diese Informationen stellen eine HTTP-Anforderung, und stellen eine physische Verbindung, die eines der vier Transportmethoden verwendet. Wenn der transportverbindung ein Fehler auftritt oder der Server ausfällt, behoben nicht die SignalR-Verbindung sofort, da der Client immer die Informationen, die es muss sich noch um eine neue transportverbindung auf dieselbe URL SignalR automatisch erneut herzustellen. In diesem Szenario kein Eingriff seitens der Anwendung beteiligt ist, und wenn die SignalR-Clientcode eine neue transportverbindung hergestellt wird, wird es eine neue SignalR-Verbindung nicht gestartet. Die Kontinuität der SignalR-Verbindung wird in der Faktentabelle berücksichtigt, die die Verbindungs-ID, die erstellt wird, beim Aufrufen der `Start` -Methode, ändert sich nicht.

Die `OnReconnected` -Ereignishandler auf dem Hub, das ausgeführt wird, wenn eine transportverbindung automatisch wieder hergestellt ist, nach der unterbrochen wurde. Die `OnDisconnected` -Ereignishandler ausgeführt wird, am Ende einer SignalR-Verbindung. Eine SignalR-Verbindung kann in einem der folgenden Methoden beenden:

- Wenn der Client Ruft die `Stop` -Methode, wird eine abbruchmeldung an den Server gesendet, und sowohl Client-als auch die SignalR-Verbindung sofort beendet.
- Nach der Verbindung zwischen Client und Server unterbrochen wird, versucht der Client eine Verbindung herzustellen, und der Server wartet des Clients eine Verbindung herzustellen. Wenn die Versuche der verbindungsherstellung nicht erfolgreich sind, und das Timeout für die Trennung der Verbindung beendet, beenden Sie sowohl Client-als auch die SignalR-Verbindung. Der Client beendet, erneut eine Verbindung herstellen möchten, und der Server frei, der seine Darstellung des SignalR-Verbindung.
- Wenn der Client beendet ohne Möglichkeit zum Aufrufen der `Stop` -Methode, die der Server wartet, für den Client erneut eine Verbindung herzustellen, und beendet dann die SignalR-Verbindung, nachdem das Timeout für die Verbindung trennen.
- Wenn der Server beendet wird, versucht der Client eine Verbindung herzustellen (die transportverbindung neu erstellen), und beendet dann die SignalR-Verbindung, nachdem das Timeout für die Verbindung trennen.

Wenn keine Verbindung Probleme auftreten, und die benutzeranwendung beendet die SignalR-Verbindung durch Aufrufen der `Stop` -Methode, die SignalR-Verbindung und die transportverbindung beginnen und enden an zur gleichen Zeit. In den folgenden Abschnitten werden bei der anderen Szenarien ausführlicher beschrieben.

<a id="transportdisconnect"></a>

### <a name="transport-disconnection-scenarios"></a>Transport Trennung Szenarien

Physische Verbindungen möglicherweise langsam, oder es liegen Unterbrechungen in Verbindung. Abhängig von Faktoren wie z. B. die Länge der Unterbrechung könnten die transportverbindung abgelegt werden. SignalR versucht, die transportverbindung erneut herzustellen. In einigen Fällen die transportverbindung API erkennt die Unterbrechung und löscht die transportverbindung und SignalR in Erfahrung gebracht, sofort, dass die Verbindung unterbrochen wird. In anderen Szenarien sofort weder die transportverbindung API noch SignalR Beachten Sie, dass die Konnektivität unterbrochen wurde. Für alle Transporte mit Ausnahme von langen abruftransports SignalR-Client verwendet eine Funktion namens *"Keepalive"* zum Verlust der Verbindung zu überprüfen, die der Transport-API kann nicht erkannt wird. Informationen zu lange Abruf-Verbindungen finden Sie unter [Einstellungen für Timeouts und Keepalive](#timeoutkeepalive) weiter unten in diesem Thema.

Wenn eine Verbindung inaktiv ist, sendet in regelmäßigen Abständen der Server ein Keepalive-Paket an den Client. Ab dem Datum, an das in diesem Artikel geschrieben wird, ist das Standardintervall 10 Sekunden. Durch diese Pakete überwachen, können Clients erkennen, liegt ein Verbindungsproblem. Wenn ein Keepalive-Paket nicht empfangen wird, wenn davon ausgegangen, vorausgesetzt nach kurzer Zeit des Clients Verbindungsprobleme wie dies darauf oder Unterbrechungen. Wenn "Keepalive" noch nicht nach einem längeren Zeitraum empfangen wird, nimmt der Client an, dass die Verbindung wurde gelöscht, und sie beginnt, erneut eine Verbindung herstellen möchten.

Das folgende Diagramm veranschaulicht die Client-als auch Ereignisse, die in einem typischen Szenario ausgelöst werden, wenn Probleme mit der physischen Verbindung, die vom Transport API sofort erkannt werden. Das Diagramm bezieht sich auf den folgenden Umständen:

- Die Transportmethode ist WebSockets, forever Frame oder Server gesendete Ereignisse.
- Es gibt unterschiedliche Zeiträume, in die physische Netzwerkverbindung unterbrochen.
- Der Transport-API wird, beachten Sie die Unterbrechung daher SignalR auf der Keepalive-Funktionalität zu erkennen.

![Transport Trennvorgänge](handling-connection-lifetime-events/_static/image2.png)

Wenn der Client wechselt in den Modus Verbindung jedoch eine transportverbindung innerhalb der Timeoutgrenze Trennen der Verbindung kann nicht erstellt werden, beendet der Server die SignalR-Verbindung an. In diesem Fall wird der Server führt des Hubs `OnDisconnected` -Methode und Warteschlangen ein Trennen-Nachricht an den Client gesendet werden soll, für den Fall, dass der Client verwaltet werden, zu einem späteren Zeitpunkt eine Verbindung herstellen. Wenn der Client dann verbunden, erhält Sie den trennungsbefehl und ruft die `Stop` Methode. In diesem Szenario `OnReconnected` wird nicht ausgeführt werden, wenn der Client erneut eine Verbindung herstellt, und `OnDisconnected` wird nicht ausgeführt werden, wenn der Client ruft `Stop`. Das folgende Diagramm veranschaulicht dieses Szenario.

![Transport-Unterbrechungen - Server-Timeouts](handling-connection-lifetime-events/_static/image3.png)

Die SignalR-Verbindung Lebensdauer-Ereignisse, die möglicherweise auf dem Client ausgelöst werden, sind die folgenden:

- `ConnectionSlow`Client-Ereignis.

    Ausgelöst, wenn Sie ein vordefinierten Anteil des Timeoutzeitraums "Keepalive" wurde seit der letzten Meldung übergeben oder "Keepalive" Ping empfangen wurde. Das "Keepalive" Warnung Standardtimeout ist 2/3 des Timeouts "Keepalive". Das "Keepalive" Timeout beträgt 20 Sekunden, damit bei etwa 13 Sekunden die Warnung auftritt.

    Standardmäßig sendet der Server "Keepalive" Pings alle 10 Sekunden, und wird vom Client nach "Keepalive" Ping über alle 2 Sekunden (ein Drittel des Unterschieds zwischen den Timeoutwert "Keepalive" und die Warnung "Keepalive" Timeoutwert).

    Wenn der API-Transport eine Trennung bewusst ist, möglicherweise SignalR die Trennung der Verbindung informiert werden, bevor das Timeout Warnung für die "Keepalive" übergeben. In diesem Fall die `ConnectionSlow` Ereignis nicht ausgelöst, und SignalR wurde direkt an die `Reconnecting` Ereignis.
- `Reconnecting`Client-Ereignis.

    Wird ausgelöst, wenn (a) der Transport API erkennt, dass die Verbindung unterbrochen ist, wird das Timeout für (b) die Keepalive seit der letzten Nachricht verstrichen oder "Keepalive" Ping wurde empfangen. Der Clientcode SignalR beginnt, erneut eine Verbindung herstellen möchten. Sie können dieses Ereignis behandeln, wenn Sie möchten die Anwendung bestimmte Aktionen ausführen, wenn der transportverbindung getrennt wird. Das Standardtimeout für "Keepalive" ist derzeit 20 Sekunden.

    Wenn der Clientcode versucht, eine hubmethode aufrufen, während SignalR befindet sich im Modus erneut zu verbinden, versucht der SignalR beim Senden des Befehls. In den meisten Fällen, solche ist nicht möglich, aber unter bestimmten Umständen sie zwar erfolgreich sein. Für verwendet den Server gesendete Ereignisse, forever Frame, und lange Abruf Transporte SignalR zwei Kommunikationskanäle eine, die vom Client verwendet, um Nachrichten zu senden und eine, die zum Empfangen von Nachrichten verwendet. Der Kanal, der für den Empfang verwendet ist die dauerhaft geöffnet, und ist dasjenige, das geschlossen wird, wenn die physische Verbindung unterbrochen wird. Der Kanal für das Senden bleibt verfügbar ist, verwendet werden, wenn physische Verbindung wiederhergestellt wird, ein Methodenaufruf vom Client zum Server erfolgreich möglicherweise vor der Receive-Kanal hergestellt wurde. Der Rückgabewert würde nicht empfangen werden, bis den Kanal, der zum Empfangen von SignalR erneut geöffnet.
- `Reconnected`Client-Ereignis.

    Wird ausgelöst, wenn die transportverbindung hergestellt wird. Die `OnReconnected` -Ereignishandler auf dem Hub wird ausgeführt.
- `Closed`Clientereignis (`disconnected` Ereignis in JavaScript).

    Wird ausgelöst, wenn der Disconnect-Timeout abläuft, während die SignalR-Clientcode versucht, erneut eine Verbindung herstellen, nachdem die transportverbindung verloren ging. Trennen Sie die Standardeinstellung beträgt 30 Sekunden. (Dieses Ereignis wird auch ausgelöst, wenn die Verbindung beendet, da die `Stop` -Methode aufgerufen wird.)

Transport Verbindung Unterbrechungen, die nicht vom Transport API erkannt werden und nicht den Empfang von "Keepalive" Pings vom Server länger als das "Keepalive" Warnung Zeitlimit verzögern können beliebige Verbindung Lebensdauerereignisse ausgelöst werden nicht führen.

Einigen netzwerkumgebungen absichtlich Schließen von Verbindungen im Leerlauf, und eine Funktion mit dem Keepalive-Pakete, zu unterstützen, dies durch Infrastrukturcode zu verhindern, die diese Netzwerke darüber informiert, dass eine SignalR-Verbindung verwendet wird. In Extremfällen der Standardrate von Keepalive Pings genug, um zu verhindern, dass geschlossene Verbindungen möglicherweise nicht. In diesem Fall können Sie "Keepalive" Pings häufiger Versand konfigurieren. Weitere Informationen finden Sie unter [Einstellungen für Timeouts und Keepalive](#timeoutkeepalive) weiter unten in diesem Thema.

> [!NOTE] 
> 
> [!IMPORTANT]
> Die Abfolge der Ereignisse, die hier beschriebenen ist nicht gewährleistet. SignalR wird jeder Versuch unternommen, Verbindung Lebensdauer Auslösen von Ereignissen in einer vorhersagbaren Weise gemäß diesem Schema, aber es gibt viele Variationen von Netzwerkereignisse und viele Möglichkeiten, die in denen zugrunde liegenden Communications-Frameworks wie z. B. Transport APIs handhaben. Z. B. die `Reconnected` Ereignis möglicherweise nicht ausgelöst, wenn der Client die Verbindung wiederherstellt, oder die `OnConnected` Ereignishandler auf dem Server kann ausgeführt werden, wenn der Versuch zum Herstellen einer Verbindung nicht erfolgreich ist. Dieses Thema beschreibt nur die Effekte, die normalerweise durch bestimmte normalen Umständen erstellt werden würde.


<a id="clientdisconnect"></a>

### <a name="client-disconnection-scenarios"></a>Trennung von Clientszenarien

Führt in ein Browserclient SignalR-Clientcode, der eine SignalR-Verbindung verwaltet in der JavaScript-Kontext einer Webseite. Die hat, warum die SignalR-Verbindung wurde die to beim Navigieren von einer Seite zu einem anderen und des Warum stehen Ihnen mehrere Verbindungen mit mehreren Verbindungs-IDs, wenn Sie mehrere Browserfenstern oder Registerkarten eine Verbindung herstellen. Wenn der Benutzer ein Browserfenster oder Tabstopp schließt, oder zu einer neuen Seite navigiert oder die Seite aktualisiert wird, die SignalR-Verbindung sofort beendet, da SignalR-Clientcode für Sie und ruft diese Browser-Ereignis behandelt die `Stop` Methode. In diesen Szenarien oder in einer beliebigen Clientplattform, wenn die Anwendung aufruft der `Stop` -Methode, die `OnDisconnected` -Ereignishandler wird sofort ausgeführt, auf dem Server und der Client löst die `Closed` Ereignis (das Ereignis wird mit dem Namen `disconnected` in JavaScript).

Wenn eine Clientanwendung oder der Computer, den Ausführung unter abstürzt oder in den Ruhezustand versetzt (z. B. wenn der Benutzer auf den Laptop schließt), wird der Server nicht zu, was passiert ist informiert. Im Hinblick auf der Server bekannt ist, der Verlust des Clients möglicherweise aufgrund einer Unterbrechung der Netzwerkverbindung, und der Client eine Verbindung herzustellen versucht möglicherweise. Aus diesem Grund in diesen Szenarien, die der Server wartet, um dem Client eine Möglichkeit, erneut eine Verbindung herzustellen, geben und `OnDisconnected` wird nicht ausgeführt, bis das Timeout für die Verbindung trennen (ungefähr 30 Sekunden in der Standardeinstellung) abläuft. Das folgende Diagramm veranschaulicht dieses Szenario.

![Fehler bei der Computer.](handling-connection-lifetime-events/_static/image4.png)

<a id="serverdisconnect"></a>

### <a name="server-disconnection-scenarios"></a>Szenarien für die Trennung von Server

Wenn ein Server ausfällt--neu gestartet wurde, ein Fehler auftritt, wird die app-Domäne verwendet, usw. – möglicherweise ist das Ergebnis ähnelt eine unterbrochene Verbindung oder API-Transport und SignalR möglicherweise sofort zu wissen, dass der Server nicht mehr vorhanden ist, und SignalR kann beispielsweise versuchen, ohne eine Verbindung zu beginnen. durch das Auslösen der `ConnectionSlow` Ereignis. Wenn der Client wechselt in den Modus Wiederherstellen der Verbindung und der Server wird wiederhergestellt oder erneut startet oder einen neuen Server online geschaltet wird vor Ablauf des Timeouts trennen, wird der Client mit dem wiederhergestellten oder neuen Verbindung wiederherstellen. In diesem Fall weiterhin den SignalR-Verbindung auf dem Client und dem `Reconnected` Ereignis wird ausgelöst. Auf dem ersten Server `OnDisconnected` nie ausgeführt wird, und klicken Sie auf dem neuen Server `OnReconnected` ausgeführt wird, obwohl `OnConnected` nie für diesen Client auf diesem Server, bevor Sie ausgeführt wurde. (Der Effekt ist gleich, wenn der Client mit dem gleichen Server nach einem Neustart oder das app-Anwendungsdomänen-Wiederverwendung, erneut eine Verbindung herstellt, da beim Neustart des Servers es keine vorherige Verbindungsaktivität Hauptspeicher.) Im folgende Diagramm wird davon ausgegangen, dass der Transport API sofort, beachten Sie die Verbindung unterbrochen wird daher die `ConnectionSlow` Ereignis wird nicht ausgelöst.

![Serverfehler und erneuten Herstellen einer Verbindung](handling-connection-lifetime-events/_static/image5.png)

Wenn ein Server nicht innerhalb des Zeitlimits Disconnect verfügbar ist, endet die SignalR-Verbindung. In diesem Szenario die `Closed` Ereignis (`disconnected` in JavaScript-Clients) für den Client ausgelöst wird, aber `OnDisconnected` wird nie aufgerufen werden, auf dem Server. Im folgende Diagramm wird davon ausgegangen, dass der Transport API aktuell ist nicht beachten Sie die Verbindung unterbrochen ist, damit es von SignalR Keepalive-Funktionalität erkannt wird und die `ConnectionSlow` Ereignis wird ausgelöst.

![Serverfehler und timeout](handling-connection-lifetime-events/_static/image6.png)

<a id="timeoutkeepalive"></a>

## <a name="timeout-and-keepalive-settings"></a>Einstellungen für Timeouts und "Keepalive"

Die Standardeinstellung `ConnectionTimeout`, `DisconnectTimeout`, und `KeepAlive` Werte in den meisten Szenarien geeignet sind, jedoch kann geändert werden, wenn Ihre Umgebung spezielle Anforderungen hat. Wenn Ihre Netzwerkumgebung Verbindungen geschlossen werden, die 5 Sekunden lang im Leerlauf befinden, müssen Sie z. B. möglicherweise den Keepalive-Wert zu verringern.

<a id="connectiontimeout"></a>

### <a name="connectiontimeout"></a>ConnectionTimeout

Diese Einstellung steht für die Zeitspanne, um eine transportverbindung zu öffnen und wartet auf eine Antwort vor dem Schließen und eine neue Verbindung öffnen lassen. Der Standardwert ist 110 Sekunden.

Diese Einstellung gilt nur beim Keepalive-Funktionalität deaktiviert die bezieht sich normalerweise nur auf den langen, abruftransport. Das folgende Diagramm veranschaulicht die Auswirkungen dieser Einstellung auf einen Long-Wert abrufen transportverbindung.

![Lange Abruf-transportverbindung](handling-connection-lifetime-events/_static/image7.png)

<a id="disconnecttimeout"></a>

### <a name="disconnecttimeout"></a>"Disconnecttimeout"

Diese Einstellung steht für die Zeitspanne, die gewartet wird, nachdem eine transportverbindung verloren, vor dem Auslösen gegangen ist der `Disconnected` Ereignis. Der Standardwert ist 30 Sekunden. Bei Festlegung `DisconnectTimeout`, `KeepAlive` automatisch auf 1/3 des festgelegt die `DisconnectTimeout` Wert.

<a id="keepalive"></a>

### <a name="keepalive"></a>"Keepalive"

Diese Einstellung steht für die Zeitdauer vor dem Senden eines Keepalive-Pakets über eine Verbindung im Leerlauf gewartet. Der Standardwert ist 10 Sekunden. Dieser Wert darf nicht mehr als 1/3 des sein der `DisconnectTimeout` Wert.

Wenn Sie beide Optionen festlegen möchten `DisconnectTimeout` und `KeepAlive`legen `KeepAlive` nach `DisconnectTimeout`. Andernfalls Ihre `KeepAlive` Einstellung werden überschrieben Wenn `DisconnectTimeout` automatisch `KeepAlive` 1/3 des Werts Timeout.

Wenn Sie Keepalive-Funktion deaktivieren möchten, legen Sie `KeepAlive` auf Null. Keepalive-Funktionalität wird automatisch deaktiviert, für den langen abruftransport.

<a id="changetimeout"></a>

### <a name="how-to-change-timeout-and-keepalive-settings"></a>So ändern Sie die Einstellungen für Timeouts und "Keepalive"

Um die Standardwerte für diese Einstellungen zu ändern, legen Sie diese `Application_Start` in Ihrer *"Global.asax"* Datei, wie im folgenden Beispiel gezeigt. Die Werte, die im Beispielcode gezeigt sind identisch mit den Standardwerten.

[!code-csharp[Main](handling-connection-lifetime-events/samples/sample1.cs)]

<a id="notifydisconnect"></a>

## <a name="how-to-notify-the-user-about-disconnections"></a>Gewusst wie: Benachrichtigung des Benutzers zu trennen

In einigen Anwendungen empfiehlt es sich um eine Nachricht an den Benutzer angezeigt wird, wenn Verbindungsprobleme vorliegen. Sie haben verschiedene Optionen, wie und wann dies erfolgen. Die folgenden Codebeispiele sind für einen JavaScript-Client mit den generierten Proxy.

- Behandeln der `connectionSlow` Ereignis, um eine Meldung angezeigt, sobald SignalR bewusst behoben werden, ist bevor es in Verbindung Modus wechselt.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample2.js)]
- Behandeln der `reconnecting` Ereignis, um eine Meldung angezeigt wird, wenn SignalR eine Trennung beachten ist und wird in Verbindung Modus.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample3.js)]
- Behandeln der `disconnected` Timeout Ereignis zum Anzeigen einer Meldung, die bei einem fehlgeschlagenen Versuch, eine Verbindung herzustellen. In diesem Szenario ist die einzige Möglichkeit, eine Verbindung mit dem Server wiederherstellen, die einen Neustart der SignalR-Verbindung durch Aufrufen der `Start` -Methode, die erstellen eine neuen Verbindungs-ID. Im folgenden Codebeispiel wird mit einem Flag um sicherzustellen, dass Sie die Benachrichtigung nur nach einem Timeout führt erneut eine Verbindung herstellt, nicht nach einem normalen End für die SignalR-Verbindung durch einen Aufruf verursacht ausgeben der `Stop` Methode.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample4.js)]

<a id="continuousreconnect"></a>

## <a name="how-to-continuously-reconnect"></a>Wiederherstellen der Verbindung fortlaufend

In einigen Anwendungen empfiehlt es sich um automatisch eine Verbindung erneut herstellen, nachdem verloren gegangen ist und des Versuch, eine Verbindung herzustellen Timeout. Zu diesem Zweck rufen Sie die `Start` Methode aus Ihrer `Closed` Ereignishandler (`disconnected` Ereignishandler auf JavaScript-Clients). Möglicherweise möchten Sie einen Zeitraum vor dem Aufruf warten `Start` , um zu vermeiden, dies zu häufig bei der Server oder die physische Verbindung sind nicht verfügbar. Im folgenden Codebeispiel ist ein JavaScript-Client mit den generierten Proxy.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample5.js)]

Ein mögliches Problem in mobilen Clients berücksichtigen ist, dass fortlaufende erneuten Herstellen einer Verbindung versucht, wenn der Server oder die physische Verbindung verfügbar ist, unnötige Akku Ausgleichsmodus verursachen könnte.

<a id="disconnectclientfromserver"></a>

## <a name="how-to-disconnect-a-client-in-server-code"></a>Einen Client im Servercode Trennen der Verbindung

SignalR Version 1.1.1 keinen integrierte Server-API für Clientverbindungen werden getrennt. Es gibt [Pläne für das Hinzufügen dieser Funktionalität in der Zukunft](https://github.com/SignalR/SignalR/issues/2101). Die einfachste Methode zum Trennen von eines Clients vom Server werden in der aktuellen SignalR-Version aus eine Disconnect-Methode auf dem Client implementieren und Aufrufen dieser Methode vom Server. Das folgende Codebeispiel zeigt eine Disconnect-Methode für einen JavaScript-Client mit den generierten Proxy.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample6.js)]

> [!WARNING]
> Sicherheit – weder diese Methode zum Trennen der Clientverbindungen noch die vorgeschlagene integrierte API geht es um das Szenario der gehackten Clients, die bösartigen Code ausgeführt werden, da die Clients erneut Verbindungen hergestellt konnte oder der gehackte Code möglicherweise entfernt die `stopClient` -Methode, oder ändern welche Aktion er ausführt. Die entsprechende Stelle um statusbehafteten Denial-of-Service (DOS)-Schutz zu implementieren ist nicht in das Framework oder der Server-Ebene, sondern im Front-End-Infrastruktur.
