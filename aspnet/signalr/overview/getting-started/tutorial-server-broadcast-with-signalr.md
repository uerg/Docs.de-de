---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: 'Lernprogramm: Broadcast-Server mit SignalR 2 | Microsoft Docs'
author: tdykstra
description: Dieses Lernprogramm veranschaulicht, wie eine Webanwendung zu erstellen, die ASP.NET SignalR 2 verwendet wird, um broadcast Serverfunktionen bereitzustellen. Server-Broadcast bedeutet, dass diese Commun...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2014
ms.topic: article
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 98a7ce4991d58181177cf56976888e9fd1526987
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-server-broadcast-with-signalr-2"></a>Lernprogramm: Broadcast-Server mit SignalR 2
====================
durch [Tom Dykstra](https://github.com/tdykstra), [Tom FitzMacken](https://github.com/tfitzmac)

> Dieses Lernprogramm veranschaulicht, wie eine Webanwendung zu erstellen, die ASP.NET SignalR 2 verwendet wird, um broadcast Serverfunktionen bereitzustellen. Server-Broadcast also an Clients gesendete Kommunikation vom Server initiiert werden. Dieses Szenario erfordert einen anderen Programmieransatz als Peer-zu-Peer-Szenarien, z. B. Chat-Anwendungen, die an Clients gesendete Kommunikation von einer oder mehreren Clients initiiert werden.
> 
> Die Anwendung, die in diesem Lernprogramm Sie erstellen simuliert einen Börsenticker ein typisches Szenario für Broadcast-Serverfunktionen.
> 
> Dieses Thema wurde ursprünglich von Patrick Fletcher geschrieben.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR Version 2
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>Verwenden von Visual Studio 2012 mit diesem Lernprogramm
> 
> 
> Um Visual Studio 2012 mit diesem Lernprogramm verwenden, führen Sie folgende Schritte aus:
> 
> - Update der [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) auf die neueste Version.
> - Installieren der [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).
> - Suchen Sie in den Webplattform-Installer, und installieren Sie **ASP.NET und 2013.1 von Web Tools für Visual Studio 2012**. Hierbei werden z. B. Visual Studio-Vorlagen für SignalR-Klassen installiert **Hub**.
> - Einige Vorlagen (z. B. **OWIN-Startklasse**) ist nicht verfügbar; verwenden Sie für diese stattdessen eine Klassendatei.
> 
> 
> ## <a name="tutorial-versions"></a>Lernprogramm-Versionen
> 
> Informationen zu früheren Versionen von SignalR finden Sie unter [ältere Versionen von SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Fragen und Kommentare
> 
> Lassen Sie Sie Feedback auf wie in diesem Lernprogramm mögen und was wir in den Kommentaren am unteren Rand der Seite verbessern können. Wenn Sie Fragen, die nicht direkt mit dem Lernprogramm verknüpft sind haben, bereitstellen können, die [ASP.NET SignalR-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Übersicht

Sie müssen in diesem Lernprogramm erstellen Sie eine Börsenticker-Anwendung, die repräsentativ für Echtzeit-Anwendungen werden in der soll in regelmäßigen Abständen "push" oder Benachrichtigungen vom Server für alle verbundenen Clients zu senden. Im ersten Teil dieses Lernprogramms erstellen Sie eine vereinfachte Version der Anwendung von Grund auf neu. Im weiteren Verlauf des Lernprogramms Sie installieren ein NuGet-Paket, das zusätzliche Funktionen enthält, und überprüfen Sie den Code für diese Funktionen.

Die Anwendung, die Sie in den ersten Teil dieses Lernprogramms erstellen zeigt ein Raster mit vordefinierten Daten.

![StockTicker Anfangsversion](tutorial-server-broadcast-with-signalr/_static/image1.png)

Der Server wird in regelmäßigen Abständen nach dem Zufallsprinzip Aktienkurse aktualisiert, und der legt die Updates für alle verbundenen Clients. Im Browser die Zahlen und Symbole in der **ändern** und  **%**  Spalten ändern sich dynamisch in Reaktion auf Benachrichtigungen vom Server. Wenn Sie zusätzliche Browsern auf die gleiche URL öffnen, zeigen sie alle die gleichen Daten und die gleichen Änderungen an den Daten gleichzeitig.

Dieses Lernprogramm enthält folgende Abschnitte:

- [Erforderliche Komponenten](#prerequisites)
- [Erstellen des Projekts](#createproject)
- [Richten Sie den Servercode](#server)
- [Der Clientcode einrichten](#client)
- [Testen der Anwendung](#test)
- [Aktivieren der Protokollierung](#enablelogging)
- [Installieren Sie und überprüfen Sie die vollständige StockTicker-Beispiel](#fullsample)
- [Nächste Schritte](#nextsteps)

Die [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) NuGet-Paket installiert eine Beispiel simuliert Börsenticker-Anwendung in einem Visual Studio-Projekt.

> [!NOTE]
> Wenn Sie nicht über die Schritte zum Erstellen der Anwendung arbeiten möchten, können Sie das Paket SignalR.Sample in eine neue leere ASP.NET-Webanwendung Projekt installieren. Wenn Sie das NuGet-Paket installieren, ohne die Schritte in diesem Lernprogramm ausführen **müssen Sie die Anweisungen in der Datei "Readme.txt" befolgen**. Zum Ausführen des Pakets müssen Sie eine OWIN-Startklasse hinzufügen, der die ConfigureSignalR-Methode aufgerufen wird, in dem Paket. Sie erhalten einen Fehler, wenn Sie nicht die OWIN-Startklasse hinzufügen.


<a id="prerequisites"></a>

## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie beginnen, stellen Sie sicher, dass Sie Visual Studio 2013 auf Ihrem Computer installiert haben. Wenn Sie Visual Studio nicht haben, finden Sie unter [ASP.NET Downloads](https://www.asp.net/downloads) zum Abrufen der kostenlose Visual Studio 2013 Express.

<a id="createproject"></a>

## <a name="create-the-project"></a>Erstellen eines Projekts

1. Aus der **Datei** Menü klicken Sie auf **neues Projekt**.
2. In der **neues Projekt** Dialogfeld erweitern Sie **c#** unter **Vorlagen** , und wählen Sie **Web**.
3. Wählen Sie die **leere ASP.NET-Webanwendung** Vorlage, nennen Sie das Projekt *SignalR.StockTicker*, und klicken Sie auf **OK**.

    ![Dialogfeld "Neues Projekt"](tutorial-server-broadcast-with-signalr/_static/image2.png)
4. In der **neue ASP.NET** Projektfenster, lassen Sie **leere** ausgewählt, und klicken Sie auf **Projekt erstellen**.

    ![Neue ASP-Projekt (Dialogfeld)](tutorial-server-broadcast-with-signalr/_static/image3.png)

<a id="server"></a>

## <a name="set-up-the-server-code"></a>Richten Sie den Servercode

In diesem Abschnitt richten Sie den Code, der auf dem Server ausgeführt wird.

### <a name="create-the-stock-class"></a>Erstellen Sie die Stock-Klasse

Sie beginnen mit dem Erstellen von der Stock-Modell-Klasse, die Sie verwenden zum Speichern und Übertragen von Informationen zu einer Aktie.

1. Erstellen Sie eine neue Klassendatei im Projektordner, benennen Sie sie *Stock.cs*, und Ersetzen Sie den Code durch folgenden Code:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    Die beiden Eigenschaften, die Sie festlegen müssen, bei der Erstellung von Aktien werden das Symbol (z. B. MSFT für Microsoft) und den Preis. Die anderen Eigenschaften hängen davon ab, wie und wann Sie Preis festlegen. Festlegen der Preis, zum ersten Mal ruft der Wert an DayOpen weitergegeben. Nachfolgende Zeiten, wenn Sie festlegen, Preis, die Änderung und Prozent Eigenschaftswerte werden berechnet, basierend auf den Unterschied zwischen Preis- und DayOpen.

### <a name="create-the-stockticker-and-stocktickerhub-classes"></a>Erstellen Sie die StockTicker und StockTickerHub-Klassen

Die SignalR-Hub-API verwenden, behandeln Server-zu-Client-Aktivität. Eine StockTickerHub-Klasse, die von der SignalR-Hub-Klasse abgeleitet wird verarbeitet, Verbindungen und Methodenaufrufe aus Clients empfängt. Sie müssen auch Börsendaten verwalten, und führen ein Timer-Objekt, um regelmäßig Updates der Preis, unabhängig von Clientverbindungen auszulösen. Diese Funktionen können nicht in einer hubklasse hinzugefügt werden, da hubinstanzen vorübergehend sind. Eine Hub-Klasseninstanz wird für jeden Vorgang auf dem Hub, wie Verbindungen und Aufrufe vom Client an den Server erstellt. Daher muss der Mechanismus, der behält Börsendaten aktualisiert Preise und überträgt die Price-Updates in einer separaten Klasse, führen Sie dies bedeutet, Sie StockTicker müssen.

![Senden von StockTicker](tutorial-server-broadcast-with-signalr/_static/image5.png)

Sie möchten nur eine Instanz der Klasse StockTicker auf dem Server ausgeführt wird, daher Sie einen Verweis aus jeder Instanz StockTickerHub auf die Singletoninstanz des StockTicker einrichten müssen. Die StockTicker-Klasse hat in der Lage, die an Clients übertragen werden, da die Börsendaten weist auf und löst Aktualisierungen, StockTicker ist jedoch keiner hubklasse. Deshalb hat die StockTicker-Klasse zum Abrufen eines Verweises auf die SignalR-Hubs Verbindung Context-Objekt. Sie können dann das Kontextobjekt für SignalR-Verbindung verwenden, an Clients übertragen.

1. In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und klicken Sie auf **hinzufügen | SignalR-Hub-Klasse (v2)**.
2. Name des neuen Hubs *StockTickerHub.cs*, und klicken Sie dann auf **hinzufügen**. SignalR-NuGet-Pakete werden dem Projekt hinzugefügt werden.
3. Ersetzen Sie den Vorlagencode, durch den folgenden Code:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

    Die [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) Klasse wird verwendet, um Methoden definieren, die Clients auf dem Server aufrufen können. Definieren Sie eine Methode: `GetAllStocks()`. Wenn ein Client zunächst mit dem Server verbunden ist, wird diese Methode, um eine Liste aller von der Aktien mit ihrer aktuellen Preise abzurufen aufgerufen. Die Methode synchron ausgeführt und zurückgeben kann `IEnumerable<Stock>` , da er Daten aus dem Arbeitsspeicher zurückgibt. Wenn die Methode die Daten abrufen, indem Sie die Aktionen ausgeführt werden, die warten, z. B. einer Datenbanksuche oder einem Webdienstaufruf umfassen würde musste würden Sie angeben `Task<IEnumerable<Stock>>` als den Rückgabewert in die asynchrone Verarbeitung zu ermöglichen. Weitere Informationen finden Sie unter [Handbuch für ASP.NET SignalR-Hubs-API - Server - beim asynchron ausführen](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).

    Der HubName-Attribut gibt an, wie der Hub im JavaScript-Code auf dem Client verwiesen wird. Auf dem Client, wenn Sie dieses Attribut verwenden, nicht der Standardnamen ist eine Version in Kamel-Schreibweise des Klassennamens, was in diesem Fall StockTickerHub wäre.

    Wie Sie später sehen, wenn Sie die StockTicker-Klasse erstellen, wird eine Singleton-Instanz dieser Klasse in der statischen Instance-Eigenschaft erstellt. Singletoninstanz des StockTicker verbleibt im Arbeitsspeicher, unabhängig davon, wie viele Clients eine Verbindung herstellen oder trennen, und diese Instanz wird die GetAllStocks-Methode verwendet wird, die aktuelle Börseninformationen zurückgeben.
4. Erstellen Sie eine neue Klassendatei im Projektordner, benennen Sie sie *StockTicker.cs*, und Ersetzen Sie den Code durch folgenden Code:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

    Da mehrere Threads dieselbe Instanz von StockTicker Code ausgeführt werden, muss die Klasse StockTicker hinsichtlich sein.

    ### <a name="storing-the-singleton-instance-in-a-static-field"></a>Speichern die Singleton-Instanz in einem statischen Feld

    Im Code initialisiert die statische \_Instanzenfeld, die die Instance-Eigenschaft mit einer Instanz der Klasse, und dies sichert ist die einzige Instanz der Klasse, die erstellt werden kann, da der Konstruktor als privat gekennzeichnet ist. [Verzögerte Initialisierung](https://msdn.microsoft.com/library/dd997286.aspx) wird verwendet, für die \_Instanzenfeld, nicht für aus Leistungsgründen jedoch sicher, dass die instanzerstellung hinsichtlich ist.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

    Jedes Mal, wenn ein Client mit dem Server verbindet Ruft eine neue Instanz der Klasse StockTickerHub in einem separaten Thread ausgeführt StockTicker Singleton-Instanz aus der statischen StockTicker.Instance-Eigenschaft, wie Sie weiter oben in der Klasse StockTickerHub gesehen haben.

    ### <a name="storing-stock-data-in-a-concurrentdictionary"></a>Das Speichern von vordefinierten Daten in einem ConcurrentDictionary

    Der Konstruktor initialisiert die \_Aktien-Auflistung mit einigen vordefinierten Beispieldaten und GetAllStocks zurückgibt, die Aktien. Wie weiter oben gezeigt, wird diese Auflistung Aktien wiederum von StockTickerHub.GetAllStocks zurückgegeben, einer Servermethode in der Hub-Klasse, die Clients aufrufen können.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

    Die Aktien-Auflistung ist definiert als eine [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) Typ für die Threadsicherheit. Als Alternative können Sie eine [Wörterbuch](https://msdn.microsoft.com/library/xfhwa508.aspx) -Objekt und das Wörterbuch explizit zu sperren, wenn Sie Änderungen vornehmen.

    Diese beispielanwendung ist es OK zum Speichern von Anwendungsdaten im Arbeitsspeicher und Daten zu verlieren, wenn die Instanz StockTicker verworfen wird. In einer echten Anwendung würden Sie mit einer Back-End-Datenspeicher, z. B. einer Datenbank arbeiten.

    ### <a name="periodically-updating-stock-prices"></a>In regelmäßigen Abständen aktualisieren Aktienkurse

    Der Konstruktor wird ein Timer-Objekt, das in regelmäßigen Abständen Methoden aufruft, die Aktienkurse werden nach dem Zufallsprinzip aktualisieren gestartet.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

    UpdateStockPrices wird von den Zeitgeber aufgerufen, der auf Null in den Statusparameter übergibt. Vor dem Aktualisieren die Preise, wird eine Sperre auf die \_UpdateStockPricesLock-Objekt. Der Code überprüft, ob ein anderer Thread bereits Preise aktualisiert, und dann TryUpdateStockPrice auf jede Aktie in der Liste ruft. Die Methode TryUpdateStockPrice entscheidet, ob den Aktienkurs ändern und wie viel, um ihn zu ändern. Wenn der Aktienkurs geändert wird, wird BroadcastStockPrice aufgerufen, um die Änderung Aktienkurs für alle verbundenen Clients zu senden.

    Die \_UpdatingStockPrices Flag markiert, als [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) um sicherzustellen, dass der Zugriff darauf hinsichtlich ist.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

    In einer realen Anwendung würde TryUpdateStockPrice rufen die-Methode einen Webdienst, um den Preis zu suchen. In diesem Code wird durch einen Zufallszahlengenerator nach dem Zufallsprinzip Änderungen vornehmen.

    ### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a>Abrufen des SignalR-Kontexts, damit die Klasse StockTicker an Clients übertragen werden können

    Da die preisänderungen hier in dem Objekt StockTicker stammen, ist dies das Objekt, das eine UpdateStockPrice-Methode auf alle verbundenen Clients aufrufen muss. In einer hubklasse haben Sie eine API für Clientmethoden aufrufen, aber StockTicker ist nicht von der Hub-Klasse abgeleitet und verfügt nicht über einen Verweis auf ein beliebiges Objekt Hub. Aus diesem Grund bietet, um an verbundene Clients gesendet wird, die StockTicker-Klasse die Kontextinstanz SignalR für die Klasse StockTickerHub abrufen und verwenden, die zum Aufrufen von Methoden auf Clients.

    Der Code Ruft einen Verweis auf den SignalR-Kontext ab, wenn der Singleton-Instanz, übergibt, die auf Verweisen im Konstruktor erstellt, und der Konstruktor speichert sie in der Eigenschaft des Clients.

    Es gibt zwei Gründe, warum den Kontext nur einmal abgerufen werden soll: Abrufen des Kontexts ist ein teurer Vorgang, und einmal des Projekts wird sichergestellt, dass die beabsichtigte Reihenfolge der Nachrichten, die an Clients gesendeten beibehalten wird.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

    Abrufen der Eigenschaft des Kontexts für Clients und zu deren in der Eigenschaft StockTickerClient ermöglicht das Schreiben von Code zum Client Aufrufen von Methoden, die die gleiche aussieht wie in einer hubklasse. Um für alle Clients übertragen können Sie z. B. Clients.All.updateStockPrice(stock) schreiben.

    Die UpdateStockPrice-Methode, die Sie in BroadcastStockPrice aufrufen vorhanden nicht noch. Sie müssen es später hinzufügen, wenn Sie Code schreiben, der auf dem Client ausgeführt wird. Sie können auf UpdateStockPrice hier verweisen, da Clients.All dynamisch "," ist. Dies bedeutet, dass der Ausdruck zur Laufzeit ausgewertet werden soll. Bei der Ausführung dieser Methodenaufruf SignalR wird der Name der Methode und der Wert des Parameters an den Client gesendet, und wenn der Client eine Methode namens UpdateStockPrice verfügt, wird diese Methode aufgerufen werden und der Wert des Parameters wird an sie übergeben werden.

    Clients.All bedeutet an alle Clients zu senden. SignalR bietet Ihnen weitere Optionen an, welche Clients oder Gruppen von Clients zum Senden an. Weitere Informationen finden Sie unter [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).

### <a name="register-the-signalr-route"></a>Registrieren Sie die SignalR-route

Der Server muss wissen, welche URL abfangen und Weiterleiten auf SignalR. Die Sie hinzufügen möchten und OWIN-Start-Klasse.

1. In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und klicken Sie dann auf **hinzufügen | OWIN-Startklasse**. Benennen Sie die Klasse **Startup.cs**.
2. Ersetzen Sie den Code in **Startup.cs** durch Folgendes.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

Sie haben nun das Einrichten der Servercode abgeschlossen. Im nächsten Abschnitt richten Sie den Client.

<a id="client"></a>

## <a name="set-up-the-client-code"></a>Der Clientcode einrichten

1. Erstellen Sie eine neue HTML-Datei im Projektordner, und nennen sie *StockTicker.html*.
2. Ersetzen Sie den Vorlagencode durch den folgenden Code ein.

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html)]

    Der HTML-Code erstellt eine Tabelle mit 5 Spalten, eine Kopfzeile und einer Datenzeile mit einer einzigen Zelle, die alle 5 Spalten umfasst. Die Datenzeile zeigt "laden" und wird nur vorübergehend, beim Starten der Anwendung angezeigt werden. JavaScript-Code wird diese Zeile zu entfernen, und fügen in seinen Zeilen vorhanden, mit Börsendaten vom Server abgerufen.

    Die Skripttags geben die jQuery-Skriptdatei, die SignalR Core-Skriptdatei der Skriptdatei für SignalR-Proxys und einer StockTicker-Skriptdatei, die Sie später erstellen. Der SignalR Proxys-Skriptdatei, die die URL "/ Signalr/Hubs" angegeben wird, wird dynamisch generiert und Proxymethoden für die Methoden in der Hub-Klasse, in diesem Fall für StockTickerHub.GetAllStocks definiert. Wenn Sie dies bevorzugen, können Sie diese JavaScript-Datei manuell generieren, mit [SignalR-Hilfsprogramme](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) und dynamische Erstellung im Methodenaufruf MapHubs deaktivieren.
3. > [!IMPORTANT]
 > Stellen Sie sicher, dass in die JavaScript-Datei verweist auf *StockTicker.html* richtig sind. D. h. Stellen Sie sicher, dass die jQuery-Version in Ihr Skripttag (1.10.2 im Beispiel) identisch mit dem jQuery-Version in Ihrem Projekts ist *Skripts* Ordner, und stellen Sie sicher, dass die SignalR-Version in Ihr Skripttag identisch mit SignalR ist die Version in Ihrem Projekts *Skripts* Ordner. Ändern Sie die Dateinamen in den Skripttags bei Bedarf.
4. In **Projektmappen-Explorer**, mit der rechten Maustaste *StockTicker.html*, und klicken Sie dann auf **als Startseite festlegen**.
5. Erstellen Sie eine neue JavaScript-Datei im Projektordner, und nennen Sie sie *StockTicker.js*...
6. Ersetzen Sie den Vorlagencode, durch den folgenden Code:

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

    $.connection bezieht sich auf die SignalR-Proxys. Der Code Ruft einen Verweis auf den Proxy für die Klasse StockTickerHub und fügt sie in die Ticker-Variable. Der Proxyname ist der Name, der durch das Attribut [HubName] festgelegt wurde:

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

    Nachdem alle Variablen und Funktionen definiert wurden, initialisiert die letzte Codezeile in der Datei die SignalR-Verbindung durch Aufrufen der SignalR-Start-Funktion. Die Startfunktion führt asynchron aus und gibt eine [jQuery zurückgestellt Objekt](http://api.jquery.com/category/deferred-object/), das bedeutet, dass kann rufen Sie die "Fertig" ändert Funktion, um anzugeben, die Funktion, die aufgerufen wird, wenn der asynchrone Vorgang abgeschlossen ist...

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

    Die Initialisierungsfunktion Ruft die GetAllStocks-Funktion auf dem Server und verwendet die Informationen, die der Server zum Aktualisieren der Lagerbestandstabelle zurückgibt. Beachten Sie, dass standardmäßig Kamel-Schreibweise auf dem Client, obwohl der Methodenname auf dem Server Pascal-Schreibweise ist verwendet. Die Regel in Kamel-Schreibweise gilt nur für Methoden, die keine Objekte. Beispielsweise finden Sie in den Bestand. Symbol "und" Stock ". Preis, nicht stock.symbol oder stock.price.

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

    Wenn auf dem Client wie in Pascal-Schreibweise verwendet, oder wenn Sie einen ganz anderen Methodennamen verwenden möchten, Sie die Hub-Methode mit dem Attribut HubMethodName die gleiche Weise ergänzen konnte ergänzt Sie den Hub-Klasse selbst mit dem Attribut HubName.

    HTML-Code für eine Tabellenzeile wird erstellt in der Init-Methode für die einzelnen vordefinierten Objekte, die vom aufrufenden FormatStock um Formateigenschaften des vordefinierten Objekts vom Server empfangen, und klicken Sie dann durch den Aufruf bereitgestellt (die am oberen Rand definiert wird *StockTicker.js*) um Platzhalter in der Variablen RowTemplate durch die vordefinierten objekteigenschaftswerte zu ersetzen. Der resultierende HTML-Code wird dann an die Lagerbestandstabelle angefügt.

    Sie rufen Init als eine Rückruffunktion, die ausgeführt wird, nach Abschluss der asynchronen Start '-Funktion übergibt. Wenn Sie als separate JavaScript-Anweisung nach dem Aufrufen der Start Init aufgerufen, wird die Funktion fehl, weil er sofort ausgeführt würde, ohne zu warten, für die Startfunktion zum Herstellen der Verbindung abzuschließen. In diesem Fall versucht die Init-Funktion würde die GetAllStocks-Funktion aufrufen, bevor die Server-Verbindung hergestellt wird.

    Wenn der Server einen Kurs geändert wird, ruft er die UpdateStockPrice auf verbundenen Clients. Die Funktion wird der Client-Eigenschaft des Proxys StockTicker hinzugefügt, damit er auf Aufrufe vom Server zur Verfügung.

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

    Die Funktion UpdateStockPrice formatiert ein vordefiniertes Objekt, das vom Server empfangene in einer Zeile einer Tabelle die gleiche Weise wie in der Init-Funktion. Allerdings es anstelle von Anhängen der zeilenupdates der Tabelle, einer Gruppe sucht die Stock aktuelle Zeile in der Tabelle und ersetzt diese Zeile durch die neue Version.

<a id="test"></a>

## <a name="test-the-application"></a>Testen der Anwendung

1. Drücken Sie F5, um die Anwendung im Debugmodus auszuführen.

    Die Lagerbestandstabelle anfänglich zeigt die "wird geladen..." Zeile, klicken Sie dann nach einer kurzen Verzögerung, die die vordefinierten Anfangsdaten angezeigt werden, und starten Sie die Aktienkurse ändern.

    ![Laden](tutorial-server-broadcast-with-signalr/_static/image6.png)

    ![Anfängliche Lagerbestandstabelle](tutorial-server-broadcast-with-signalr/_static/image7.png)

    ![Lagerbestandstabelle Änderungen vom Server empfangen.](tutorial-server-broadcast-with-signalr/_static/image8.png)
2. Kopieren Sie die URL aus der Adressleiste des Browsers ein, und fügen Sie ihn in eine oder mehrere neue Browser die Dauer.

    Die erste vordefinierte Anzeige ist identisch mit der ersten Browser aus, und Änderungen gleichzeitig ausgeführt.
3. Schließen von allen Browsern, öffnen Sie ein neues Browserfenster, und wechseln Sie zur gleichen URL.

    StockTicker Singleton-Objekt weiterhin auf dem Server ausgeführt werden, damit der Lagerbestandstabelle zeigt, dass die Aktien weiterhin geändert haben. (Die ersten Tabelle mit 0 (null) Zahlen ändern nicht angezeigt werden.)
4. Schließen Sie den Browser.

<a id="enablelogging"></a>

## <a name="enable-logging"></a>Protokollierung aktivieren

SignalR ist eine integrierte Protokollierungsfunktion, die Sie auf dem Client aktivieren können, um bei der Problembehandlung. In diesem Abschnitt werden die Protokollierung aktivieren und finden Sie Beispiele, die zeigen, wie Protokolle Aufschluss darüber, welche der folgenden Transportmethoden SignalR verwendet wird:

- [WebSockets](http://en.wikipedia.org/wiki/WebSocket), wird von IIS 8 und aktuellen Browsern unterstützt.
- [Vom Server gesendeten Ereignisse](http://en.wikipedia.org/wiki/Server-sent_events), wird vom Browser als Internet Explorer unterstützt.
- [Forever frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), wird von Internet Explorer unterstützt.
- [Lange abrufen AJAX](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), wird von allen Browsern unterstützt.

Für eine bestimmte Verbindung wählt SignalR die beste Transportmethode, die dem Server und dem Client zu unterstützen.

1. Open *StockTicker.js* und fügen Sie eine Codezeile zum Aktivieren der Protokollierung unmittelbar vor dem Code, der die Verbindung am Ende der Datei initialisiert:

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js)]
2. Drücken Sie F5, um das Projekt auszuführen.
3. Öffnen Sie Ihres Browsers Developer Tools-Fenster zu, und wählen Sie die Konsole, um die Protokolle finden Sie unter. Sie müssen möglicherweise aktualisieren Sie die Seite, um die Protokolle der Transportmethode für eine neue Verbindung aushandeln Signalr finden Sie unter.

    Wenn Sie Internet Explorer 10 unter Windows 8 (IIS 8) ausgeführt werden, ist die Transportmethode WebSockets.

    ![IE 10 IIS 8 Console](tutorial-server-broadcast-with-signalr/_static/image9.png)

    Wenn Sie Internet Explorer 10 unter Windows 7 (IIS 7.5) ausgeführt werden, ist die Transportmethode Iframe.

    ![IE 10 Console, IIS 7.5](tutorial-server-broadcast-with-signalr/_static/image10.png)

    Installieren Sie in Firefox das Firebug-add-in zum Abrufen eines Konsolenfensters. Wenn Sie Firefox 19 unter Windows 8 (IIS 8) ausgeführt werden, ist die Transportmethode WebSockets.

    ![Firefox 19 IIS 8 Websockets](tutorial-server-broadcast-with-signalr/_static/image11.png)

    Wenn Sie Firefox 19 unter Windows 7 (IIS 7.5) ausgeführt werden, ist die Transportmethode Server gesendete Ereignisse.

    ![Firefox 19 IIS 7.5 Console](tutorial-server-broadcast-with-signalr/_static/image12.png)

<a id="fullsample"></a>

## <a name="install-and-review-the-full-stockticker-sample"></a>Installieren Sie und überprüfen Sie die vollständige StockTicker-Beispiel

Die StockTicker-Anwendung, die installiert ist, indem die [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) NuGet-Paket bietet mehr Funktionen als die vereinfachte Version, die Sie gerade von Grund auf neu erstellt. In diesem Abschnitt des Lernprogramms müssen Sie das NuGet-Paket installieren und überprüfen Sie die neuen Funktionen und den Code, der sie implementiert. Wenn Sie das Paket installieren, ohne den früheren Schritten dieses Lernprogramms auszuführen, müssen Sie eine OWIN-Startklasse zu Ihrem Projekt hinzufügen. Dieser Schritt wird in der Datei "Readme.txt" des NuGet-Pakets erläutert.

### <a name="install-the-signalrsample-nuget-package"></a>Installieren Sie das SignalR.Sample NuGet-Paket

1. In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und klicken Sie auf **NuGet-Pakete verwalten**.
2. In der **NuGet-Pakete verwalten** (Dialogfeld), klicken Sie auf **Online**, geben Sie *SignalR.Sample* in der **Online suchen** Feld, und klicken Sie dann auf  **Installieren Sie** in der **SignalR.Sample** Paket.

    ![SignalR.Sample-Paket installieren](tutorial-server-broadcast-with-signalr/_static/image13.png)
3. In **Projektmappen-Explorer**, erweitern Sie die *SignalR.Sample* Ordner, der durch die Installation des Pakets SignalR.Sample erstellt wurde.
4. In der *SignalR.Sample* Ordner mit der rechten Maustaste *StockTicker.html*, und klicken Sie dann auf **als Startseite festlegen**.

    > [!NOTE]
    > Installieren der SignalR.Sample NuGet-Paket möglicherweise ändern Sie die Version von jQuery, die Sie in Ihrem *Skripts* Ordner. Die neue *StockTicker.html* Datei zur Installation des Pakets in der *SignalR.Sample* Ordner wird mit der jQuery-Version, die vom Paket wird installiert, aber wenn Sie Ihre ursprüngliche ausführenmöchten,synchronisiertsein*StockTicker.html* Datei erneut, die Sie möglicherweise zuerst aktualisieren den jQuery-Verweis in das Skript-Tag.

### <a name="run-the-application"></a>Ausführen der Anwendung

1. Drücken Sie F5, um die Anwendung auszuführen.

    Zusätzlich zu den im Raster, das Sie zuvor gesehen haben, zeigt die vollständige Börsenticker-Anwendung horizontal scrollbaren Fensters, das die gleichen Börsendaten anzeigt. Wenn Sie die Anwendung zum ersten Mal ausführen, der "Markt" ist "geschlossen", und sehen eine statische Raster und ein Börsenticker-Fenster, das Durchführen eines Bildlaufs ist nicht.

    ![StockTicker Bildschirm starten](tutorial-server-broadcast-with-signalr/_static/image14.png)

    Beim Klicken auf **Open Market**, **Börsenticker Live** Feld einen horizontalen Bildlauf durchführen, und der Server startet Aktienkurs Änderungen werden nach dem Zufallsprinzip in regelmäßigen Abständen zu übertragen. Jedes Mal, wenn einen Aktienkurs ändert, sowohl die **Live Stock Tabelle** Raster und **Börsenticker Live** Feld aktualisiert werden. Ein Aktienkurs Preisänderung positiv ist, wird die Stock mit einem grünen Hintergrund angezeigt, und wenn die Änderung negativ ist, wird die Stock mit einem roten Hintergrund angezeigt.

    ![StockTicker-app, um Zugang zum Markt öffnen](tutorial-server-broadcast-with-signalr/_static/image15.png)

    Die **schließen Markt** Schaltfläche verhindert die Änderungen und Ticker Bildlauf beendet wird und die **zurücksetzen** Schaltfläche setzt alle vordefinierte Daten an den Ausgangszustand zurück, bevor preisänderungen gestartet. Wenn Sie weitere Browserfenster zu öffnen, und wechseln Sie zur gleichen URL, sehen Sie die gleichen Daten, die gleichzeitig in jedem Browser dynamisch aktualisiert. Wenn Sie eine der Schaltflächen klicken, Antworten von allen Browsern die gleiche Weise zur gleichen Zeit.

### <a name="live-stock-ticker-display"></a>Live Börsenticker anzeigen

Die **Börsenticker Live** Anzeige ist eine ungeordnete Liste in ein Div-Element, das zu einer einzigen Zeile von CSS-Formatvorlagen formatiert ist. Die Ticker initialisiert und die gleiche Weise wie in der Tabelle aktualisiert: durch Ersetzen der Platzhalter in einer &lt;li&gt; Vorlagenzeichenfolge und dynamisch hinzufügen der &lt;li&gt; Elemente, die die &lt;Ul&gt; Element. Das Durchführen eines Bildlaufs erfolgt über die animieren jQuery-Funktion die Margin-Left der ungeordnete Liste innerhalb der div variieren

Die Börsenticker HTML:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

Die Börsenticker CSS:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

Der jQuery-Code, der es macht einen Bildlauf durchführen:

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a>Zusätzliche Methoden auf dem Server, den der Client aufrufen kann.

Die Klasse StockTickerHub definiert vier zusätzliche Methoden, die der Client aufrufen kann:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

Als Antwort auf die Schaltflächen am oberen Rand der Seite "heißen OpenMarket CloseMarket und zurücksetzen. Sie veranschaulichen das Muster der ein Client Auslösen von Zustandsübergängen, die sofort auf alle Clients weitergegeben wird. Jede dieser Methoden Ruft eine Methode in der Klasse StockTicker, Effekte, die der Zugang zum Markt Status ändern und anschließend broadcasts Zustand "Neu".

In der Klasse StockTicker wird der Status der Zugang zum Markt über eine Eigenschaft MarketState verwaltet, die einen MarketState Enum-Wert zurückgibt:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

Jede der Methoden, die dem Markt Statuswechsel dazu innerhalb eines Blocks sperren, da die StockTicker-Klasse verfügt über hinsichtlich sein:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

Um sicherzustellen, dass dieser Code hinsichtlich, ist die \_MarketState hinter der MarketState-Eigenschaft liegenden Felds wird als veränderlich, markiert

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

Die Methoden BroadcastMarketStateChange und BroadcastMarketReset ähneln BroadcastStockPrice-Methode, die Sie bereits gesehen haben, außer sie verschiedene Methoden, die auf dem Client definierten aufrufen:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a>Zusätzliche Funktionen auf dem Client, den der Server aufrufen können

Die UpdateStockPrice-Funktion verarbeitet jetzt Raster und die Anzeige Ticker und jQuery.Color blinkt, Farben rote und grüne verwendet.

Neue Funktionen in *SignalR.StockTicker.js* aktivieren oder deaktivieren Sie die Schaltflächen auf der Grundlage vermarkten Zustand, und sie die horizontale Bildlaufleiste für Ticker Fenster starten oder beenden. Da ticker.client, mehrere Funktionen hinzugefügt werden die [jQuery erweitern Funktion](http://api.jquery.com/jQuery.extend/) wird verwendet, um sie hinzuzufügen.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a>Zusätzliche Client-Setup nach dem Herstellen der Verbindung

Nachdem der Client die Verbindung herstellt, weist Sie zusätzliche Arbeit leisten, Vorgehensweise: ermitteln, ob der Markt ist offen oder geschlossen ist, um die entsprechenden MarketOpened oder MarketClosed eine Funktion aufrufen, und fügen die Server-Methodenaufrufe, die Schaltflächen.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

Die Servermethoden sind nicht auf die Schaltflächen bis verkabelt. nach dem Herstellen der Verbindung, damit der Code versuchen kann nicht auf die Servermethoden aufrufen, damit sie verfügbar sind.

<a id="nextsteps"></a>

## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben Sie gelernt, die eine SignalR-Anwendung zu programmieren, in denen Nachrichten vom Server für alle verbundenen Clients sowohl in regelmäßigen Abständen und Reaktion auf Benachrichtigungen von jedem beliebigen Client überträgt. Das Muster der Verwendung einer Multithread-Singleton-Instanz zur Beibehaltung des Zustands der Server kann auch auch in mehrere Spieler online game-Szenarien verwendet werden. Ein Beispiel finden Sie unter [ShootR-Spiels, die sich auf SignalR basierenden](https://github.com/NTaylorMullen/ShootR).

Lernprogramme, die Peer-zu-Peer-Kommunikationsszenarien zeigen, finden Sie unter [erste Schritte mit SignalR](introduction-to-signalr.md) und [in Echtzeit aktualisiert, mit SignalR](tutorial-high-frequency-realtime-with-signalr.md).

Erweiterte Konzepte von SignalR-Entwicklung finden Sie auf den folgenden Websites für SignalR-Quellcode und Ressourcen:

- [ASP.NET SignalR](../../index.md)
- [SignalR-Projekt](http://signalr.net/)
- [SignalR Github und Beispiele](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)

Eine exemplarische Vorgehensweise zum Bereitstellen einer SignalR-Anwendung in Azure finden Sie unter [SignalR mit Web-Apps in Azure App Service mithilfe von](../deployment/using-signalr-with-azure-web-sites.md). Ausführliche Informationen dazu, wie Sie ein Visual Studio-Webprojekt auf einer Windows Azure-Website bereitstellen, finden Sie unter [erstellen eine ASP.NET Web-app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).
