---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: 'Lernprogramm: Hochfrequente Realtime mit SignalR 2 | Microsoft Docs'
author: pfletcher
description: Dieses Lernprogramm veranschaulicht, wie eine Webanwendung zu erstellen, die ASP.NET SignalR verwendet wird, um hochfrequente Messagingfunktionen bereitzustellen. Hochfrequenz messaging in...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: ab051b2ab85d1aac1e7179f342f22f470b1d1cc7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-high-frequency-realtime-with-signalr-2"></a>Lernprogramm: Hochfrequente Realtime mit SignalR 2
====================
durch [Patrick Fletcher](https://github.com/pfletcher)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

> Dieses Lernprogramm veranschaulicht, wie eine Webanwendung zu erstellen, die ASP.NET SignalR 2 verwendet wird, um hochfrequente Messagingfunktionen bereitzustellen. Hochfrequente messaging bedeutet in diesem Fall Updates, die mit einer festen Rate gesendet werden. Bei dieser Anwendung werden bis zu 10 Nachrichten pro Sekunde.
> 
> Die Anwendung, die in diesem Lernprogramm Sie erstellen zeigt eine Form, die Benutzer gezogen werden können. Die Position der Form in allen anderen verbundenen Browsern werden entsprechend die Position der gezogene Form über zeitgesteuerte Updates aktualisiert werden.
> 
> In diesem Lernprogramm eingeführte Konzepte haben Anwendungen in Echtzeit Spiele und anderen Simulation.
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

Dieses Lernprogramm veranschaulicht, wie eine Anwendung erstellen, der den Zustand eines Objekts mit anderen Browsern in Echtzeit. Die Anwendung, die wir erstellen müssen, wird MoveShape aufgerufen. Der Seite "MoveShape" wird ein HTML-Div-Element angezeigt, die der Benutzer ziehen kann. der Benutzer das DIV-Element zieht, wird seiner neue Position an den Server gesendet werden, die informieren dann anderen verbundenen Clients die Position der Form entsprechend aktualisiert wird.

![Das Anwendungsfenster](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

Die Anwendung, die in diesem Lernprogramm erstellten basiert auf einer Demo von Damian Edwards. Ein Video, enthält diese Demo überwachungsarbeitsbereich [hier](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR).

Im Lernprogramm wird vorgeführt, wie zum Senden von Nachrichten von SignalR aus jedes Ereignis, das ausgelöst wird, wie die Form gezogen wird gestartet. Jedem verbundenen Client wird dann die Position der lokalen Version der Form aktualisiert, sobald eine Nachricht empfangen wird.

Während die Anwendung diese Methode verwenden funktioniert, ist dies nicht empfohlene Programmiermodell, da gäbe es keine obere Grenze für die Anzahl der Nachrichten, Abrufen von gesendet werden, damit die Clients und Server mit Nachrichten überlastet abrufen konnte, und würde die Leistung beeinträchtigen . Die angezeigte Animation auf dem Client wäre auch separaten, wie die Form durch jede Methode sofort verschoben werden würde, statt zu jeder neuen Speicherort verschieben. In späteren Abschnitten des Lernprogramms werden gezeigt, wie eine Zeitgeber-Funktion erstellt, die die maximale Übertragungsrate einschränkt, mit der Nachrichten vom Client oder Server gesendet werden, und wie Sie die Form reibungslos zwischen Standorten zu verschieben. Die endgültige Version der Anwendung, die in diesem Lernprogramm erstellten kann von heruntergeladen werden [Code Gallery](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a).

Dieses Lernprogramm enthält folgende Abschnitte:

- [Erforderliche Komponenten](#prerequisites)
- [Erstellen Sie das Projekt und fügen Sie das SignalR und JQuery.UI NuGet-Paket](#createtheproject2013)
- [Die Basis-Anwendung erstellen](#baseapp)
- [Starten Sie den Hub, beim Starten der Anwendung](#startup2013)
- [Fügen Sie die Client-Schleife hinzu](#clientloop)
- [Fügen Sie die Server-Schleife hinzu](#serverloop)
- [Hinzufügen von Animationen zum Glätten auf dem client](#animation)
- [Weitere Schritte](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Erforderliche Komponenten

Dieses Lernprogramm erfordert Visual Studio 2013.

<a id="createtheproject2013"></a>

## <a name="create-the-project-and-add-the-signalr-and-jqueryui-nuget-package"></a>Erstellen Sie das Projekt und fügen Sie das SignalR und JQuery.UI NuGet-Paket

In diesem Abschnitt erstellen wir das Projekt in Visual Studio 2013.

Die folgenden Schritte verwenden Visual Studio 2013 erstellen eine leere ASP.NET-Webanwendung und SignalR und jQuery.UI-Bibliotheken hinzufügen:

1. Erstellen Sie in Visual Studio eine ASP.NET-Webanwendung.

    ![Erstellen von web](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)
2. In der **neues ASP.NET-Projekt** Fenster verlassen **leere** ausgewählt, und klicken Sie auf **Projekt erstellen**.

    ![Erstellen Sie leeres web](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
3. In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, wählen Sie **hinzufügen | SignalR-Hub-Klasse (v2)**. Benennen Sie die Klasse **MoveShapeHub.cs** und dem Projekt hinzugefügt. In diesem Schritt wird die **MoveShapeHub** -Klasse und fügt Sie dem Projekt eine Reihe von Skriptdateien und Assemblyverweisen, die SignalR unterstützen.

    > [!NOTE]
    > Sie können auch SignalR zu einem Projekt hinzufügen, indem Sie auf **Tools | Bibliothek-Paket-Manager | Paket-Manager-Konsole** und einen Befehl ausführen:

    `install-package Microsoft.AspNet.SignalR` 

    Wenn Sie die Konsole verwenden, um SignalR hinzuzufügen, erstellen Sie die SignalR-Hub-Klasse als separater Schritt nach dem Hinzufügen von SignalR.
4. Klicken Sie auf **Tools | Bibliothek-Paket-Manager | Paket-Manager-Konsole**. Führen Sie in der Paket-Manager-Fenster den folgenden Befehl ein:

    `Install-Package jQuery.UI.Combined`

    Hiermit werden die jQuery UI-Bibliothek, die Sie verwenden zum Animieren der Form "installiert.
5. In **Projektmappen-Explorer** erweitern Sie den Knoten des Skripts. Skriptbibliotheken für SignalR, jQuery und jQueryUI werden in das Projekt angezeigt.

    ![Bibliothek Skriptverweise](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="baseapp"></a>

## <a name="create-the-base-application"></a>Die Basis-Anwendung erstellen

In diesem Abschnitt erstellen wir eine Browseranwendung, die den Speicherort der Form an den Server, während jede Mauszeigerposition ausgelöste Ereignis sendet. Daten beim Empfang, sendet der Server dann diese Informationen für alle anderen verbundenen Clients. Es wird auf diese Anwendung in späteren Abschnitten erweitern.

1. Wenn Sie die Klasse MoveShapeHub.cs in bereits erstellt haben **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **hinzufügen**, **Klasse...** . Benennen Sie die Klasse **MoveShapeHub** , und klicken Sie auf **hinzufügen**.
2. Ersetzen Sie den Code in der neuen **MoveShapeHub** Klasse durch den folgenden Code.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

    Die `MoveShapeHub` Klasse oben ist eine Implementierung eines SignalR-Hubs. Wie in der [erste Schritte mit SignalR](tutorial-getting-started-with-signalr.md) Lernprogramm Hub verfügt über eine Methode, die die Clients direkt aufruft. In diesem Fall sendet der Client ein Objekt mit der neuen X- und Y-Koordinaten der Form auf dem Server, der dann an alle anderen verbundenen Clients weitergegeben ruft. SignalR wird automatisch dieses Objekts unter Verwendung von JSON serialisiert werden.

    Das Objekt, das an den Client gesendet werden sollen (`ShapeModel`) enthält Elemente, um die Position der Form zu speichern. Die Version des Objekts auf dem Server enthält außerdem einen Member zum Nachverfolgen von der Client Daten gespeichert werden, sodass ein Client gesendet werden, ihre eigenen Daten wird nicht. Dieses Element verwendet die `JsonIgnore` Attribut und verhindert, serialisiert und an den Client gesendet wird.

<a id="startup2013"></a>
## <a name="starting-the-hub-when-the-application-starts"></a>Starten Sie den Hub, beim Starten der Anwendung

1. Wir fügen richten Sie als Nächstes Zuordnung auf den Hub beim Starten der Anwendung. In SignalR-2, dies erfolgt durch Hinzufügen einer OWIN-Start-Klasse, die angerufen `MapSignalR` Wenn der Startklasse `Configuration` Methode ausgeführt wird, wenn OWIN gestartet wird. Die Klasse wird hinzugefügt, mit dem Start des OWIN-Prozess unter Verwendung der `OwinStartup` Assembly-Attribut.

    In **Projektmappen-Explorer**Sie mit der rechten Maustaste des Projekts, und klicken Sie auf **hinzufügen | OWIN-Startklasse**. Benennen Sie die Klasse *Start* , und klicken Sie auf **OK**.
2. Ändern Sie den Inhalt Startup.cs wie folgt aus:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

<a id="client"></a>
## <a name="adding-the-client"></a>Hinzufügen des Clients

1. Fügen Sie anschließend den Client. In **Projektmappen-Explorer**Sie mit der rechten Maustaste des Projekts, und klicken Sie auf **hinzufügen | Neues Element**. In der **neues Element hinzufügen** wählen Sie im Dialogfeld **Html-Seite**. Nennen Sie die Seite **"default.HTML"** , und klicken Sie auf **hinzufügen**.
2. In **Projektmappen-Explorer**mit der rechten Maustaste auf die Seite, die Sie gerade erstellt haben, und klicken Sie auf **als Startseite festlegen**.
3. Ersetzen Sie den Standardcode in der HTML-Seite mit dem folgenden Codeausschnitt an.

    > [!NOTE]
    > Stellen Sie sicher, dass das Skript unten Übereinstimmung, die Pakete dem Projekt im Ordner "Skripts" hinzugefügt verweist.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html)]

    Die oben genannten HTML und JavaScript-Code erstellt ein rotes Div Form aufgerufen, können Sie die Form ziehen Verhalten mithilfe der jQuery-Bibliothek und verwendet die Form vom Typ `drag` Ereignis, um die Position der Form an den Server gesendet.
4. Starten Sie die Anwendung durch Drücken von F5. Kopieren Sie die URL der Seite, und fügen Sie ihn in einem zweiten Browserfenster. Ziehen Sie die Form in einem Browserfenster. die Form, in dem anderen Browserfenster sollten verschieben.

    ![Das Anwendungsfenster](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a>Fügen Sie die Client-Schleife hinzu

Da den Speicherort der Form für jede Mauszeigerposition ausgelöste Ereignis senden eine ist ggf. unnötig Umfang des Netzwerkverkehrs erstellen, müssen die Nachrichten vom Client gedrosselt werden. Verwenden wir die Javascript `setInterval` Funktion So richten Sie eine Schleife ein, die Informationen zur neuen Position in einer festen Rate auf dem Server sendet. Diese Schleife ist eine sehr einfache Darstellung der eine "Spiel Schleife", eine wiederholt aufgerufene Funktion, die die Funktionalität eines Spiels oder andere Simulation Laufwerke.

1. Aktualisieren Sie den Clientcode in der HTML-Seite entsprechend den folgenden Codeausschnitt an.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html)]

    Die oben genannten Update fügt die `updateServerModel` -Funktion, die auf einer festen Frequenz aufgerufen wird. Diese Funktion sendet die Positionsdaten, an den Server immer die `moved` Flag gibt an, dass neue die zu sendenden Positionsdaten.
2. Starten Sie die Anwendung durch Drücken von F5. Kopieren Sie die URL der Seite, und fügen Sie ihn in einem zweiten Browserfenster. Ziehen Sie die Form in einem Browserfenster. die Form, in dem anderen Browserfenster sollten verschieben. Die Animation wird nicht so reibungslos wie im vorherigen Abschnitt angezeigt, da die Anzahl der Nachrichten, die an den Server gesendet werden können gedrosselt werden, wird.

    ![Das Anwendungsfenster](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a>Fügen Sie die Server-Schleife hinzu

Wechseln in der aktuellen Anwendung Nachrichten, die vom Server an den Client gesendet so oft, wie sie empfangen werden. Dies stellt ein ähnliches Problem wie auf dem Client dargestellt wurde; Nachrichten können häufiger werden bei Bedarf, und die Verbindung konnte daher überflutet werden gesendet werden. In diesem Abschnitt wird beschrieben, wie beim Aktualisieren des Servers zum Implementieren eines Zeitgebers, das die Rate der ausgehenden Nachrichten schränkt wird.

1. Ersetzen Sie den Inhalt des `MoveShapeHub.cs` mit dem folgenden Codeausschnitt.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    Der obige Code erweitert den Client hinzufügen die `Broadcaster` -Klasse, die schränkt den ausgehenden Nachrichten mithilfe der `Timer` Klasse von .NET Framework.

    Seit der Hub selbst um ein vorübergehendes handelt (er ist erstellt jedes Mal, wenn er benötigt wird), die `Broadcaster` wird als Singleton erstellt werden. Verzögerter Initialisierung (eingeführt in .NET 4) wird verwendet, um zu seiner Erstellung zu verzögern, bis diese benötigt wird, um sicherzustellen, dass die erste hubinstanz vollständig erstellt wird, bevor der Timer gestartet wird.

    Der Aufruf an die Clients' `UpdateShape` Funktion wird dann aus des Hubs verschoben `UpdateModel` Methode, sodass die It nicht mehr aufgerufen wird, sofort, wenn eingehende Nachrichten empfangen werden. Stattdessen werden die Nachrichten an die Clients gesendet werden, mit einer Rate von 25 Aufrufe pro Sekunde, von verwaltet die `_broadcastLoop` Timer innerhalb der `Broadcaster` Klasse.

    Schließlich anstelle von Aufrufen der Methode vom Hub direkt die `Broadcaster` Klasse muss einen Verweis auf den aktuell ausgeführten Hub abrufen (`_hubContext`) mithilfe der `GlobalHost`.
2. Starten Sie die Anwendung durch Drücken von F5. Kopieren Sie die URL der Seite, und fügen Sie ihn in einem zweiten Browserfenster. Ziehen Sie die Form in einem Browserfenster. die Form, in dem anderen Browserfenster sollten verschieben. Es werden ein Unterschied im Browser aus dem vorherigen Abschnitt, aber gedrosselt die Anzahl der Nachrichten, die an den Client gesendet werden können.

    ![Das Anwendungsfenster](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a>Hinzufügen von Animationen zum Glätten auf dem client

Die Anwendung ist fast abgeschlossen, aber stellen wir eine weitere Verbesserung, die während des Verschiebens der Form auf dem Client beim Server Nachrichten verschieben. Anstatt das Festlegen der Position der Form an den neuen Speicherort, der vom Server erhält, verwenden wir der JQuery UI-Bibliotheks `animate` Funktion, um die Form problemlos zwischen der aktuellen und der neuen Position verschieben.

1. Aktualisieren Sie den Client `updateShape` Methode gesucht werden soll, wie z. B. den folgenden hervorgehobenen Code:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

    Der obige Code verschiebt die Form an der ursprünglichen Stelle in das neue Projekt, das vom Server im Verlauf der Animation Intervall (in diesem Fall 100 Millisekunden) angegeben. Alle vorherige Animation unter der Form "wird gelöscht, bevor die neue Animation gestartet wird.
2. Starten Sie die Anwendung durch Drücken von F5. Kopieren Sie die URL der Seite, und fügen Sie ihn in einem zweiten Browserfenster. Ziehen Sie die Form in einem Browserfenster. die Form, in dem anderen Browserfenster sollten verschieben. Die Verschiebung der Form in das andere Fenster sollte weniger holprig, wie ihre Bewegung über wird einmal pro eingehender Nachricht festgelegt, anstatt Zeit interpoliert wird angezeigt.

    ![Das Anwendungsfenster](tutorial-high-frequency-realtime-with-signalr/_static/image8.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a>Weitere Schritte

In diesem Lernprogramm haben Sie gelernt, die eine SignalR-Anwendung zu programmieren, in denen hochfrequente Nachrichten zwischen Clients und Servern an. Dieses Paradigma Kommunikation eignet sich für die Entwicklung von Onlinespielen und andere Simulationen, z. B. [ShootR Spiel erstellt, die mit SignalR](http://shootr.signalr.net).

Die vollständige Anwendung, die in diesem Lernprogramm erstellten heruntergeladen werden kann, aus [Code Gallery](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a).

Besuchen Sie die folgenden Websites für SignalR-Quellcode und Ressourcen, um weitere Informationen zu SignalR Entwicklung Konzepte:

- [SignalR-Projekt](http://signalr.net)
- [SignalR Github und Beispiele](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)

Eine exemplarische Vorgehensweise zum Bereitstellen einer SignalR-Anwendung in Azure finden Sie unter [SignalR mit Web-Apps in Azure App Service mithilfe von](../deployment/using-signalr-with-azure-web-sites.md). Ausführliche Informationen dazu, wie Sie ein Visual Studio-Webprojekt auf einer Windows Azure-Website bereitstellen, finden Sie unter [erstellen eine ASP.NET Web-app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).
