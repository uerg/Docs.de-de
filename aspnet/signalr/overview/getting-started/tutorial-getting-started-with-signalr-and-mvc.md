---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'Lernprogramm: Erste Schritte mit SignalR 2 und MVC 5 | Microsoft Docs'
author: pfletcher
description: Dieses Lernprogramm zeigt, wie ASP.NET SignalR 2 eine Echtzeit-Chat-Anwendung erstellen. Fügen SignalR zu einer MVC 5-Anwendung und erstellen Sie eine Ansicht Chat...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.openlocfilehash: ca0471114da7363c5df9d459308708e7ab4f8b84
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-getting-started-with-signalr-2-and-mvc-5"></a>Lernprogramm: Erste Schritte mit SignalR 2 und MVC 5
====================
durch [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

> Dieses Lernprogramm zeigt, wie ASP.NET SignalR 2 eine Echtzeit-Chat-Anwendung erstellen. Sie werden einer MVC 5-Anwendung SignalR hinzu, und erstellen eine Chat-Ansicht zum Senden und Meldungen angezeigt werden. 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - MVC 5
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

Dieses Lernprogramm führt Sie in Echtzeit-Anwendungsentwicklung ASP.NET SignalR 2 mit ASP.NET MVC 5. Das Lernprogramm verwendet den gleichen Chat-Anwendungscode als die [SignalR Getting Started Tutorial](tutorial-getting-started-with-signalr.md), jedoch wird gezeigt, wie sie einer MVC 5-Anwendung hinzufügen.

In diesem Thema erfahren Sie die folgenden SignalR Entwicklungsaufgaben:

- Einer MVC 5-Anwendung hinzugefügt die SignalR-Bibliothek.
- Erstellen Hub- und OWIN-Start-Klassen, die Inhalt an Clients zu verteilen.
- Verwenden die SignalR-jQuery-Bibliothek auf einer Webseite zum Senden von Nachrichten und Updates vom Hub angezeigt.

Der folgende Screenshot zeigt die abgeschlossenen chatanwendung, die in einem Browser ausgeführt.

![Chatinstanzen](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

Abschnitte:

- [Richten Sie das Projekt](#setup)
- [Führen Sie das Beispiel](#run)
- [Überprüfen Sie den Code](#code)
- [Nächste Schritte](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Richten Sie das Projekt

Erforderliche Komponenten:

- Visual Studio 2013. Wenn Sie nicht über Visual Studio verfügen, finden Sie unter [ASP.NET Downloads](https://www.asp.net/downloads) das kostenlose Visual Studio 2013 Express Entwicklungstool abgerufen.

In diesem Abschnitt veranschaulicht das Erstellen einer ASP.NET MVC 5-Anwendung, die SignalR-Bibliothek hinzufügen und die chatanwendung erstellen.

1. Klicken Sie in Visual Studio Erstellen einer c# ASP.NET-Anwendung mit der Zielversion .NET Framework 4.5, nennen Sie sie SignalRChat, und klicken Sie auf OK.

    ![Erstellen von web](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)
2. In der `New ASP.NET Project` Dialogfeld, und wählen Sie **MVC**, und klicken Sie auf **Authentifizierung ändern**.

    ![Erstellen von web](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)
3. Wählen Sie **keine Authentifizierung** in der **Authentifizierung ändern** Dialogfeld ein, und klicken Sie auf **OK**.

    ![Wählen Sie die keine Authentifizierung](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

    > [!NOTE]
    > Bei Auswahl einen anderen Authentifizierungsanbieter für Ihre Anwendung eine `Startup.cs` Klasse wird für Sie erstellt werden; Sie müssen keine Erstellen eigener `Startup.cs` Klasse in Schritt 10 unten.
4. Klicken Sie auf **OK** in der **neues ASP.NET-Projekt** Dialogfeld.
5. Öffnen der **Tools | Bibliothek-Paket-Manager | Paket-Manager-Konsole** und führen Sie den folgenden Befehl. Dieser Schritt fügt dem Projekt eine Reihe von Skriptdateien und Assemblyverweisen, die SignalR-Funktionen zu aktivieren.

    `install-package Microsoft.AspNet.SignalR`
6. In **Projektmappen-Explorer**, erweitern Sie den Ordner "Skripts". Beachten Sie, dass das Projekt Skriptbibliotheken für SignalR hinzugefügt wurde.

    ![Ordner "Skripts"](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)
7. In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, wählen Sie **hinzufügen | Neuer Ordner**, und fügen Sie einen neuen Ordner namens **Hubs**.
8. Mit der rechten Maustaste die **Hubs** Ordner, klicken Sie auf **hinzufügen | Neues Element**, wählen die **Visual C# | Web | SignalR** Knoten in der **installiert** klicken Sie im Bereich **SignalR-Hub-Klasse (v2)** aus der Mitte, und erstellen Sie einen neuen Hub mit dem Namen **ChatHub.cs**. Sie verwenden diese Klasse als einen SignalR-Hub, die Nachrichten für alle Clients sendet.

    ![Erstellen des neuen Hubs](tutorial-getting-started-with-signalr-and-mvc/_static/image6.png)
9. Ersetzen Sie den Code in der **ChatHub** Klasse durch den folgenden Code.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]
10. Erstellen Sie eine neue Klasse namens Startup.cs. Ändern Sie den Inhalt der Datei in den folgenden.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]
11. Bearbeiten der `HomeController` Klasse gefunden **Controllers/HomeController.cs** und fügen Sie die folgende Methode der Klasse. Diese Methode gibt die **Chat** anzeigen, die Sie in einem späteren Schritt erstellen.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]
12. Mit der rechten Maustaste die **Ansichten/Start** Ordner, und wählen **hinzufügen... | Ansicht**.
13. In der **Ansicht hinzufügen** im Dialogfeld die Namen der neuen Sicht **Chat**.

    ![Hinzufügen einer Ansicht](tutorial-getting-started-with-signalr-and-mvc/_static/image7.png)
14. Ersetzen Sie den Inhalt des **Chat.cshtml** durch den folgenden Code.

    > [!IMPORTANT]
    > Wenn Sie Visual Studio-Projekts SignalR und andere Skriptbibliotheken hinzugefügt haben, könnten die Paket-Manager eine Version der SignalR-Skriptdatei installieren, die aktueller als die Version, die in diesem Thema dargestellt sind. Stellen Sie sicher, dass im Code wird der Verweis auf das Skript der Version der Skriptbibliothek, die in Ihrem Projekt installiert entspricht.

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]
15. **Speichern Sie alle** für das Projekt.

<a id="run"></a>

## <a name="run-the-sample"></a>Führen Sie das Beispiel

1. Drücken Sie F5, um das Projekt im Debugmodus auszuführen.
2. Fügen Sie in der Adresszeile des Browsers **/Home/Chat** an die URL der Standardseite für das Projekt. Chat-Seite lädt in einen Browserinstanz und fordert Sie auf einen Benutzernamen ein.

    ![Benutzername eingeben](tutorial-getting-started-with-signalr-and-mvc/_static/image8.png)
3. Geben Sie einen Benutzernamen ein.
4. Kopieren Sie die URL aus der Adresszeile des Browsers ein, und verwenden Sie, um zwei weitere Browserinstanzen öffnen. Geben Sie in den einzelnen Instanzen Browser einen eindeutigen Benutzernamen aus.
5. Klicken Sie in den einzelnen Instanzen Browser einen Kommentar hinzufügen, und klicken Sie auf **senden**. Die Kommentare sollten in alle Browserinstanzen angezeigt werden.

    > [!NOTE]
    > Diese einfache chatanwendung behält den Kontext der Diskussion auf dem Server. Der Hub, Kommentare, um alle aktuellen Benutzer sendet. Benutzer, die den Chat später beizutreten sehen, dass Nachrichten ab dem Zeitpunkt hinzugefügt werden, dass sie verknüpfen.
6. Der folgende Screenshot zeigt die chatanwendung, die in einem Browser ausgeführt.

    ![Chat-Browser](tutorial-getting-started-with-signalr-and-mvc/_static/image9.png)
7. In **Projektmappen-Explorer**, überprüfen Sie die **Skriptdokumente** Knoten für die ausgeführte Anwendung. Dieser Knoten ist sichtbar, im Debugmodus befindet, bei Verwendung von Internet Explorer als Ihren Browser. Es wird eine Skriptdatei mit dem Namen **Hubs** , die die SignalR-Bibliothek wird zur Laufzeit dynamisch generiert. Diese Datei wird die Kommunikation zwischen jQuery-Skripts und serverseitigen Code verwaltet. Wenn Sie einen Browser als Internet Explorer verwenden, können Sie auch den dynamischen zugreifen **Hubs** Datei zu suchen und ihn direkt, z. B. http://mywebsite/signalr/hubs.

<a id="code"></a>

## <a name="examine-the-code"></a>Überprüfen Sie den Code

Die SignalR-Chat-Anwendung zeigt zwei grundlegende SignalR Entwicklungsaufgaben: Erstellen eines Hubs wie das Hauptfenster Koordinierung-Objekt, auf dem Server und mithilfe der SignalR-jQuery-Bibliothek zum Senden und Empfangen von Nachrichten.

### <a name="signalr-hubs"></a>SignalR Hubs

Im Codebeispiel den **ChatHub** Klasse leitet sich von der **Microsoft.AspNet.SignalR.Hub** Klasse. Ableiten von der **Hub** Klasse ist eine hilfreiche Möglichkeit zum Erstellen einer SignalR-Anwendung. Sie können öffentliche Methoden in der hubklasse erstellen und dann Zugriff auf diese Methoden durch Aufruf aus Skripts auf einer Webseite.

Rufen Sie in den Code Chat Clients die **ChatHub.Send** Methode, um eine neue Nachricht zu senden. Der Hub sendet wiederum die Nachricht an alle Clients durch den Aufruf **Clients.All.addNewMessageToPage**.

Die **senden** einige Konzepte der Hub-Methode veranschaulicht:

- Öffentliche Methoden für einen Hub deklariert werden, damit Clients, die sie aufrufen können.
- Verwenden der **Microsoft.AspNet.SignalR.Hub.Clients** Eigenschaft auf alle Clients, die mit diesem Hub verbunden sind.
- Aufrufen einer Funktion auf dem Client (z. B. die `addNewMessageToPage` Funktion) so aktualisieren Sie Clients.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR und jQuery

Die **Chat.cshtml** Sichtdatei im Codebeispiel zeigt, wie die SignalR-jQuery-Bibliothek für die Kommunikation mit SignalR-Hubs. Die Durchführung wichtiger Aufgaben im Code werden einen Verweis auf den automatisch generierten Proxy für den Hub Deklarieren einer Funktion, die der Server auf Push-Inhalte für Clients aufrufen können, und starten eine Verbindung zum Senden von Nachrichten an den Hub erstellt.

Der folgende Code deklariert einen Verweis auf einen hubproxy.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> In JavaScript wird der Verweis auf die Server-Klasse und ihre Member in Camel-Case. Im Codebeispiel verweist auf die C#- **ChatHub** Klasse in JavaScript als **ChatHub**. Wenn Sie verweisen möchten die `ChatHub` Klasse in jQuery mit konventionellen Pascal Groß-/Kleinschreibung wie in C#-Klassendatei ChatHub.cs bearbeiten. Hinzufügen einer `using` -Anweisung verweisen die `Microsoft.AspNet.SignalR.Hubs` Namespace. Fügen Sie dann die `HubName` -Attribut auf die `ChatHub` Klasse, zum Beispiel `[HubName("ChatHub")]`. Aktualisieren Sie schließlich den jQuery-Verweis auf die `ChatHub` Klasse.


Der folgende Code zeigt, wie eine Rückruffunktion in das Skript erstellt wird. Die hubklasse, auf dem Server ruft diese Funktion um Inhaltsupdates an jeden Client per Push übertragen. Der optionale Aufruf der `htmlEncode` Funktion zeigt eine Möglichkeit, HTML zu codieren den Nachrichteninhalt vor der Anzeige auf der Seite als eine Möglichkeit, Skript-Injection verhindern.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

Der folgende Code zeigt, wie Sie eine Verbindung mit dem Hub zu öffnen. Der Code wird die Verbindung gestartet und übergibt diese eine Funktion, um das Click-Ereignis zu behandeln, auf die **senden** auf der Seite Chat.

> [!NOTE]
> Dieser Ansatz wird sichergestellt, dass die Verbindung hergestellt wird, bevor der Ereignishandler ausgeführt wird.


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Nächste Schritte

Sie erfahren, dass SignalR ein Framework zur Erstellung von Echtzeit-Webanwendungen. Sie haben auch erfahren, mehrere SignalR Entwicklungsaufgaben: zum Hinzufügen von SignalR an eine ASP.NET-Anwendung zum Erstellen einer hubklasse und zum Senden und Empfangen von Nachrichten über den Hub.

Eine exemplarische Vorgehensweise, die SignalR-beispielanwendung in Azure bereitzustellen, finden Sie unter [SignalR mit Web-Apps in Azure App Service mithilfe von](../deployment/using-signalr-with-azure-web-sites.md). Ausführliche Informationen dazu, wie Sie ein Visual Studio-Webprojekt auf einer Windows Azure-Website bereitstellen, finden Sie unter [erstellen eine ASP.NET Web-app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

Erweiterte Konzepte von SignalR-Entwicklungen finden Sie auf den folgenden Websites für SignalR-Quellcode und Ressourcen:

- [SignalR-Projekt](http://signalr.net)
- [SignalR Github und Beispiele](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
