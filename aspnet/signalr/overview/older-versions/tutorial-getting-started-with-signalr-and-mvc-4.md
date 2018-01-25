---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
title: 'Lernprogramm: Erste Schritte mit SignalR 1.x und MVC 4 | Microsoft Docs'
author: pfletcher
description: Verwenden Sie zum Erstellen einer chatanwendung in Echtzeit ASP.NET SignalR und ASP.NET MVC 4.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/29/2013
ms.topic: article
ms.assetid: eeef9f73-6de3-49f9-b50b-9af22108f2ce
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 632e6098a03eae02f2367c6dc1c293dbdb6b6170
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-getting-started-with-signalr-1x-and-mvc-4"></a>Lernprogramm: Erste Schritte mit SignalR 1.x und MVC 4
====================
durch [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

> Dieses Lernprogramm zeigt, wie ASP.NET SignalR zum Erstellen einer Anwendung in Echtzeit chatten. Sie werden eine MVC 4-Anwendung SignalR hinzu, und erstellen eine Chat-Ansicht zum Senden und Meldungen angezeigt werden.


## <a name="overview"></a>Übersicht

Dieses Lernprogramm führt Sie in Echtzeit-Anwendungsentwicklung ASP.NET MVC 4 mit ASP.NET SignalR. Das Lernprogramm verwendet den gleichen Chat-Anwendungscode als die [SignalR Getting Started Tutorial](tutorial-getting-started-with-signalr.md), jedoch wird gezeigt, wie sie eine MVC 4-Anwendung, die basierend auf der Internet-Vorlage hinzugefügt.

In diesem Thema erfahren Sie die folgenden SignalR Entwicklungsaufgaben:

- Eine MVC 4-Anwendung hinzugefügt die SignalR-Bibliothek.
- Erstellen einer hubklasse, um Inhalt an Clients zu verteilen.
- Verwenden die SignalR-jQuery-Bibliothek auf einer Webseite zum Senden von Nachrichten und Updates vom Hub angezeigt.

Der folgende Screenshot zeigt die abgeschlossenen chatanwendung, die in einem Browser ausgeführt.

![Chatinstanzen](tutorial-getting-started-with-signalr-and-mvc-4/_static/image2.png)

Abschnitte:

- [Richten Sie das Projekt](#setup)
- [Führen Sie das Beispiel](#run)
- [Überprüfen Sie den Code](#code)
- [Nächste Schritte](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Richten Sie das Projekt

Erforderliche Komponenten:

- Visual Studio 2010 SP1, Visual Studio 2012 oder Visual Studio 2012 Express. Wenn Sie nicht über Visual Studio verfügen, finden Sie unter [ASP.NET Downloads](https://www.asp.net/downloads) das kostenlose Visual Studio 2012 Express Entwicklungstool abgerufen.
- Für Visual Studio 2010 installieren [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).

In diesem Abschnitt veranschaulicht das Erstellen einer ASP.NET MVC 4-Anwendung, die SignalR-Bibliothek hinzufügen und die chatanwendung erstellen.

1. 1. Erstellen Sie in Visual Studio eine ASP.NET MVC 4-Anwendung, nennen Sie sie SignalRChat, und klicken Sie auf OK.

        > [!NOTE]
        > Wählen Sie in Visual Studio 2010 **.NET Framework 4** die Dropdown-Steuerelement die Framework-Version. SignalR-Code, die auf .NET Framework-Versionen 4 und 4.5 ausgeführt wird.

        ![Erstellen von Mvc-Webanwendung](tutorial-getting-started-with-signalr-and-mvc-4/_static/image3.png)
    2. Wählen Sie die Internet-Anwendungsvorlage, deaktivieren Sie die Option zum **erstellen ein Komponententestprojekts**, und klicken Sie auf OK.

        ![Erstellen von Mvc-Internet-Website](tutorial-getting-started-with-signalr-and-mvc-4/_static/image4.png)
    3. Öffnen der **Tools | Bibliothek-Paket-Manager | Paket-Manager-Konsole** und führen Sie den folgenden Befehl. Dieser Schritt fügt dem Projekt eine Reihe von Skriptdateien und Assemblyverweisen, die SignalR-Funktionen zu aktivieren.

        `install-package Microsoft.AspNet.SignalR -Version 1.1.3`
    4. In **Projektmappen-Explorer** erweitern Sie den Ordner "Skripts". Beachten Sie, dass das Projekt Skriptbibliotheken für SignalR hinzugefügt wurde.

        ![Verweise auf](tutorial-getting-started-with-signalr-and-mvc-4/_static/image6.png)
    5. In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, wählen Sie **hinzufügen | Neuer Ordner**, und fügen Sie einen neuen Ordner namens **Hubs**.
    6. Mit der rechten Maustaste die **Hubs** Ordner, klicken Sie auf **hinzufügen | Klasse**, und erstellen Sie eine neue c#-Klasse, mit dem Namen **ChatHub.cs**. Sie verwenden diese Klasse als einen SignalR-Hub, die Nachrichten für alle Clients sendet.

> [!NOTE]
> Wenn Sie Visual Studio 2012 verwenden, und installiert die [ASP.NET und Web Tools 2012.2 Update](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), können Sie die neue Elementvorlage SignalR zum Erstellen der hubklasse. Zu diesem Zweck Maustaste die **Hubs** Ordner, klicken Sie auf **hinzufügen | Neues Element**Option **SignalR-Hub-Klasse (v1)**, und benennen Sie die Klasse **ChatHub.cs**.


1. Ersetzen Sie den Code in der **ChatHub** Klasse durch den folgenden Code.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample1.cs)]
2. Öffnen der **"Global.asax"** Datei für das Projekt, und fügen Sie einen Aufruf an die Methode `RouteTable.Routes.MapHubs();` als erste Zeile des Codes in der `Application_Start` Methode. Dieser Code die Standardroute für SignalR-Hubs registriert und muss aufgerufen werden, bevor Sie eine beliebige andere Routen registrieren. Das abgeschlossene `Application_Start` -Methode sieht wie im folgenden Beispiel.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample2.cs)]
3. Bearbeiten der `HomeController` Klasse gefunden **Controllers/HomeController.cs** und fügen Sie die folgende Methode der Klasse. Diese Methode gibt die **Chat** anzeigen, die Sie in einem späteren Schritt erstellen.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample3.cs)]
4. Mit der rechten Maustaste innerhalb der `Chat` Methode, die Sie gerade erstellt haben, und klicken Sie auf **Ansicht hinzufügen** um eine neue Datei zu erstellen.
5. In der **Ansicht hinzufügen** Dialogfeld, stellen Sie sicher, dass das Kontrollkästchen aktiviert ist **mithilfe eine Seite Layout- oder Masterseite** (löschen Sie die anderen Kontrollkästchen), und klicken Sie dann auf **hinzufügen**.

    ![Hinzufügen einer Ansicht](tutorial-getting-started-with-signalr-and-mvc-4/_static/image8.png)
6. Bearbeiten Sie die neue Ansichtsdatei mit dem Namen **Chat.cshtml**. Nach der &lt;h2&gt; zu kennzeichnen, fügen Sie die folgende &lt;Div&gt; Abschnitt und `@section scripts` Codeblock in die Seite. Dieses Skript kann die Seite Chatnachrichten senden und Nachrichten vom Server angezeigt. Der vollständige Code für die Chat-Sicht, die in den folgenden Codeblock wird angezeigt.

    > [!IMPORTANT]
    > Wenn Sie Visual Studio-Projekts SignalR und andere Skriptbibliotheken hinzugefügt haben, könnten die Paket-Manager Versionen der Skripts installieren, die jünger als die Versionen, die in diesem Thema dargestellt sind. Stellen Sie sicher, dass die Skriptverweise im Code die Version des Skriptbibliotheken, die in Ihrem Projekt installiert überein.

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample4.cshtml)]
7. **Speichern Sie alle** für das Projekt.

<a id="run"></a>

## <a name="run-the-sample"></a>Führen Sie das Beispiel

1. Drücken Sie F5, um das Projekt im Debugmodus auszuführen.
2. Fügen Sie in der Adresszeile des Browsers **/Home/Chat** an die URL der Standardseite für das Projekt. Chat-Seite lädt in einen Browserinstanz und fordert Sie auf einen Benutzernamen ein.

    ![Benutzername eingeben](tutorial-getting-started-with-signalr-and-mvc-4/_static/image9.png)
3. Geben Sie einen Benutzernamen ein.
4. Kopieren Sie die URL aus der Adresszeile des Browsers ein, und verwenden Sie, um zwei weitere Browserinstanzen öffnen. Geben Sie in den einzelnen Instanzen Browser einen eindeutigen Benutzernamen aus.
5. Klicken Sie in den einzelnen Instanzen Browser einen Kommentar hinzufügen, und klicken Sie auf **senden**. Die Kommentare sollten in alle Browserinstanzen angezeigt werden.

    > [!NOTE]
    > Diese einfache chatanwendung behält den Kontext der Diskussion auf dem Server. Der Hub, Kommentare, um alle aktuellen Benutzer sendet. Benutzer, die den Chat später beizutreten sehen, dass Nachrichten ab dem Zeitpunkt hinzugefügt werden, dass sie verknüpfen.
6. Der folgende Screenshot zeigt die chatanwendung, die in einem Browser ausgeführt.

    ![Chat-Browser](tutorial-getting-started-with-signalr-and-mvc-4/_static/image11.png)
7. In **Projektmappen-Explorer**, überprüfen Sie die **Skriptdokumente** Knoten für die ausgeführte Anwendung. Dieser Knoten ist sichtbar, im Debugmodus befindet, bei Verwendung von Internet Explorer als Ihren Browser. Es wird eine Skriptdatei mit dem Namen **Hubs** , die die SignalR-Bibliothek wird zur Laufzeit dynamisch generiert. Diese Datei wird die Kommunikation zwischen jQuery-Skripts und serverseitigen Code verwaltet. Wenn Sie einen Browser als Internet Explorer verwenden, können Sie auch den dynamischen zugreifen **Hubs** Datei zu suchen und ihn direkt, z. B. http://mywebsite/signalr/hubs.

    ![Generierte Hub-Skript](tutorial-getting-started-with-signalr-and-mvc-4/_static/image13.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Überprüfen Sie den Code

Die SignalR-Chat-Anwendung zeigt zwei grundlegende SignalR Entwicklungsaufgaben: Erstellen eines Hubs wie das Hauptfenster Koordinierung-Objekt, auf dem Server und mithilfe der SignalR-jQuery-Bibliothek zum Senden und Empfangen von Nachrichten.

### <a name="signalr-hubs"></a>SignalR Hubs

Im Codebeispiel den **ChatHub** Klasse leitet sich von der **Microsoft.AspNet.SignalR.Hub** Klasse. Ableiten von der **Hub** Klasse ist eine hilfreiche Möglichkeit zum Erstellen einer SignalR-Anwendung. Sie können öffentliche Methoden in der hubklasse erstellen und dann Zugriff auf diese Methoden durch Aufruf aus jQuery-Skripts auf einer Webseite.

Rufen Sie in den Code Chat Clients die **ChatHub.Send** Methode, um eine neue Nachricht zu senden. Der Hub sendet wiederum die Nachricht an alle Clients durch den Aufruf **Clients.All.addNewMessageToPage**.

Die **senden** einige Konzepte der Hub-Methode veranschaulicht:

- Öffentliche Methoden für einen Hub deklariert werden, damit Clients, die sie aufrufen können.
- Verwenden der **Microsoft.AspNet.SignalR.Hub.Clients** Eigenschaft auf alle Clients, die mit diesem Hub verbunden sind.
- Aufrufen einer jQuery-Funktion auf dem Client (z. B. die `addNewMessageToPage` Funktion) so aktualisieren Sie Clients.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR und jQuery

Die **Chat.cshtml** Sichtdatei im Codebeispiel zeigt, wie die SignalR-jQuery-Bibliothek für die Kommunikation mit SignalR-Hubs. Die Durchführung wichtiger Aufgaben im Code werden einen Verweis auf den automatisch generierten Proxy für den Hub Deklarieren einer Funktion, die der Server auf Push-Inhalte für Clients aufrufen können, und starten eine Verbindung zum Senden von Nachrichten an den Hub erstellt.

Der folgende Code deklariert einen Proxy für einen Hub.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample6.js)]

> [!NOTE]
> In jQuery wird der Verweis auf die Server-Klasse und ihre Member in Camel-Case. Im Codebeispiel verweist auf die C#- **ChatHub** Klasse in jQuery als **ChatHub**. Wenn Sie verweisen möchten die `ChatHub` Klasse in jQuery mit konventionellen Pascal Groß-/Kleinschreibung wie in C#-Klassendatei ChatHub.cs bearbeiten. Hinzufügen einer `using` -Anweisung verweisen die `Microsoft.AspNet.SignalR.Hubs` Namespace. Fügen Sie dann die `HubName` -Attribut auf die `ChatHub` Klasse, zum Beispiel `[HubName("ChatHub")]`. Aktualisieren Sie schließlich den jQuery-Verweis auf die `ChatHub` Klasse.


Der folgende Code zeigt, wie eine Rückruffunktion in das Skript erstellt wird. Die hubklasse, auf dem Server ruft diese Funktion um Inhaltsupdates an jeden Client per Push übertragen. Der optionale Aufruf der `htmlEncode` Funktion zeigt eine Möglichkeit, HTML zu codieren den Nachrichteninhalt vor der Anzeige auf der Seite als eine Möglichkeit, Skript-Injection verhindern.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample7.html)]

Der folgende Code zeigt, wie Sie eine Verbindung mit dem Hub zu öffnen. Der Code wird die Verbindung gestartet und übergibt diese eine Funktion, um das Click-Ereignis zu behandeln, auf die **senden** auf der Seite Chat.

> [!NOTE]
> Dieser Ansatz wird sichergestellt, dass die Verbindung hergestellt wird, bevor der Ereignishandler ausgeführt wird.


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Nächste Schritte

Sie erfahren, dass SignalR ein Framework zur Erstellung von Echtzeit-Webanwendungen. Sie haben auch erfahren, mehrere SignalR Entwicklungsaufgaben: zum Hinzufügen von SignalR an eine ASP.NET-Anwendung zum Erstellen einer hubklasse und zum Senden und Empfangen von Nachrichten über den Hub.

Erweiterte Konzepte von SignalR-Entwicklungen finden Sie auf den folgenden Websites für SignalR-Quellcode und Ressourcen:

- [SignalR-Projekt](http://signalr.net)
- [SignalR Github und Beispiele](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
