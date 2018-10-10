---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'Tutorial: Erste Schritte mit SignalR 2 und MVC 5 | Microsoft-Dokumentation'
author: pfletcher
description: Dieses Tutorial veranschaulicht, wie ASP.NET SignalR 2 zu verwenden, um eine chatanwendung mit Echtzeitfunktionalität erstellen. Sie fügen SignalR zu einer MVC 5-Anwendung und eine Chat-Ansicht erstellen...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.openlocfilehash: a58b95adfb5d0165887b95abd3247d3a829aa882
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912227"
---
<a name="tutorial-getting-started-with-signalr-2-and-mvc-5"></a>Tutorial: Erste Schritte mit SignalR 2 und MVC 5
====================
durch [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

[Abgeschlossenes Projekt herunterladen](http://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

> Dieses Tutorial veranschaulicht, wie ASP.NET SignalR 2 zu verwenden, um eine chatanwendung mit Echtzeitfunktionalität erstellen. Sie SignalR zu einer MVC 5-Anwendung hinzufügen und erstellen Sie eine Chat-Ansicht zum Senden und Meldungen angezeigt werden.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - MVC 5
> - SignalR-Version 2
>
>
>
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>In diesem Tutorial mithilfe von Visual Studio 2012
>
>
> Um Visual Studio 2012 mit diesem Tutorial verwenden möchten, führen Sie folgende Schritte aus:
>
> - Aktualisieren Ihrer [-Paket-Manager](http://docs.nuget.org/docs/start-here/installing-nuget) auf die neueste Version.
> - Installieren Sie die [Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx).
> - Suchen Sie in den Webplattform-Installer, und installieren Sie **ASP.NET und Web Tools 2013.1 für Visual Studio 2012**. Dadurch wird Visual Studio-Vorlagen für SignalR-Klassen wie z. B. installiert **Hub**.
> - Einige Vorlagen (z. B. **OWIN-Startklasse**) nicht zur Verfügung; verwenden Sie für diese stattdessen eine Klassendatei.
>
>
> ## <a name="tutorial-versions"></a>Lernprogramm-Versionen
>
> Weitere Informationen zu früheren Versionen von SignalR, finden Sie unter [ältere Versionen von SignalR](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Fragen und Kommentare
>
> Lassen Sie Feedback, auf wie Ihnen in diesem Tutorial gefallen hat und was wir in den Kommentaren am unteren Rand der Seite verbessern können. Wenn Sie Fragen, die nicht direkt mit dem Tutorial verknüpft sind haben, können Sie sie veröffentlichen das [ASP.NET SignalR-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Übersicht

Dieses Tutorial führt Sie in Echtzeit-Webanwendungsentwicklung mit ASP.NET SignalR 2 und ASP.NET MVC 5. Das Lernprogramm verwendet den gleichen Code der Chat-Anwendung wie den [SignalR, erste Schritte-Lernprogramm](tutorial-getting-started-with-signalr.md), aber zeigt, wie sie eine MVC 5-Anwendung hinzuzufügen.

In diesem Thema lernen Sie die folgenden Entwicklungsaufgaben von SignalR:

- Die SignalR-Bibliothek hinzufügen zu einer MVC 5-Anwendung.
- Erstellen von Hub und OWIN-Startup-Klassen, die Inhalt an Clients zu verteilen.
- Verwenden die SignalR-jQuery-Bibliothek auf einer Webseite zum Senden von Nachrichten und Updates vom Hub angezeigt.

Der folgende Screenshot zeigt die abgeschlossene Chat-Anwendung, die in einem Browser ausgeführt.

![Chatinstanzen](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

Abschnitte:

- [Einrichten des Projekts](#setup)
- [Ausführen des Beispiels](#run)
- [Überprüfen Sie den Code](#code)
- [Nächste Schritte](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Einrichten des Projekts

Erforderliche Komponenten:

- Visual Studio 2013. Wenn Sie nicht über Visual Studio verfügen, finden Sie unter [ASP.NET-Downloads](https://www.asp.net/downloads) um die kostenlose Visual Studio 2013 Express Entwicklungstool zu erhalten.

In diesem Abschnitt veranschaulicht das Erstellen einer ASP.NET MVC 5-Anwendung, die SignalR-Bibliothek hinzufügen und erstellen Sie die chatanwendung.

1. In Visual Studio Erstellen einer c# ASP.NET-Anwendung, die auf .NET Framework 4.5 abzielt, nennen Sie sie SignalRChat, und klicken Sie auf OK.

    ![Erstellen Sie web](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)
2. In der `New ASP.NET Project` Dialogfeld ein, und wählen Sie **MVC**, und klicken Sie auf **Authentifizierung ändern**.

    ![Erstellen Sie web](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)
3. Wählen Sie **keine Authentifizierung** in die **Authentifizierung ändern** Dialogfeld ein, und klicken Sie auf **OK**.

    ![Wählen Sie keine Authentifizierung](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

    > [!NOTE]
    > Wenn Sie einen anderen Authentifizierungsanbieter für Ihre Anwendung auswählen einer `Startup.cs` Klasse wird für Sie erstellt werden; Sie müssen nicht zum Erstellen eines eigenen `Startup.cs` Klasse, die in Schritt 10 unten.
4. Klicken Sie auf **OK** in die **neues ASP.NET-Projekt** Dialogfeld.
5. Öffnen der **Tools > NuGet Paket-Manager > Paket-Manager Console** und führen folgenden Befehl aus. Dieser Schritt fügt dem Projekt hinzu einen Satz von Skriptdateien und Assemblyverweise, die SignalR-Funktionalität zu aktivieren.

    `install-package Microsoft.AspNet.SignalR`
6. In **Projektmappen-Explorer**, erweitern Sie den Ordner "Scripts". Beachten Sie, dass die Skriptbibliotheken für SignalR zum Projekt hinzugefügt wurden.

    ![Ordner "Scripts"](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)
7. In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, wählen Sie **hinzufügen | Neuer Ordner**, und fügen Sie einen neuen Ordner namens **Hubs**.
8. Mit der rechten Maustaste die **Hubs** Ordner, klicken Sie auf **hinzufügen | Neues Element**, wählen die **Visual c# | Web | SignalR** Knoten in der **installiert** wählen Sie im Bereich **SignalR-Hubklasse (v2)** im mittleren Bereich, und erstellen Sie einen neuen Hub, mit dem Namen **ChatHub.cs**. Sie verwenden diese Klasse als einen SignalR-Hub, die Nachrichten an alle Clients sendet.

    ![Erstellen von neuen hub](tutorial-getting-started-with-signalr-and-mvc/_static/image6.png)
9. Ersetzen Sie den Code in die **ChatHub** Klasse durch den folgenden Code.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]
10. Erstellen Sie eine neue Klasse namens "Startup.cs". Ändern Sie den Inhalt der Datei die folgende aus.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]
11. Bearbeiten der `HomeController` Klasse finden Sie im **Controllers/HomeController.cs** und fügen Sie die folgende Methode der Klasse. Diese Methode gibt die **Chat** anzeigen, die Sie in einem späteren Schritt erstellen.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]
12. Mit der rechten Maustaste die **Views/Home** Ordner, und wählen **hinzufügen.... | Ansicht**.
13. In der **Ansicht hinzufügen** Dialogfeld Namen der neuen Sicht **Chat**.

    ![Hinzufügen einer Ansicht](tutorial-getting-started-with-signalr-and-mvc/_static/image7.png)
14. Ersetzen Sie den Inhalt der **Chat.cshtml** durch den folgenden Code.

    > [!IMPORTANT]
    > Wenn Sie SignalR und anderen JavaScript-Bibliotheken zu Ihrem Visual Studio-Projekt hinzufügen, kann der Paket-Manager eine Version der SignalR-Skriptdatei installieren, als die Version, die in diesem Thema gezeigten ist. Stellen Sie sicher, dass der Verweis auf das Skript in Ihrem Code mit der Version der Skriptbibliothek in Ihrem Projekt installiert übereinstimmt.

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]
15. **Speichern Sie alle** für das Projekt.

<a id="run"></a>

## <a name="run-the-sample"></a>Ausführen des Beispiels

1. Drücken Sie F5, um das Projekt im Debugmodus auszuführen.
2. Fügen Sie in der Adresszeile des Browsers **/Home/Chat** an die URL der Standardseite für das Projekt. Die Chat-Seite lädt in eine Instanz eines Webbrowsers und aufforderungen für einen Benutzernamen ein.

    ![Benutzername eingeben](tutorial-getting-started-with-signalr-and-mvc/_static/image8.png)
3. Geben Sie einen Benutzernamen ein.
4. Kopieren Sie die URL aus der Adresszeile des Browsers, und öffnen Sie zwei Browserinstanzen für weitere. Geben Sie in den einzelnen Instanzen Browser einen eindeutigen Benutzernamen ein.
5. Klicken Sie in den einzelnen Instanzen Browser Fügt einen Kommentar hinzu, und klicken Sie auf **senden**. Die Kommentare sollte in alle Browserinstanzen angezeigt werden.

    > [!NOTE]
    > Diese einfache chatanwendung werden den Kontext der Diskussion auf dem Server nicht verwaltet werden. Der Hub sendet, Kommentare, um alle aktuellen Benutzer. Benutzer, die im Chat später zusammenfügen sehen, dass Nachrichten ab dem Zeitpunkt hinzugefügt werden, dass sie aufgenommen werden.
6. Der folgende Screenshot zeigt die chatanwendung, die in einem Browser ausgeführt.

    ![Chat-Browser](tutorial-getting-started-with-signalr-and-mvc/_static/image9.png)
7. In **Projektmappen-Explorer**, überprüfen Sie die **Skriptdokumente** Knoten für die ausgeführte Anwendung. Dieser Knoten ist sichtbar im Debugmodus, wenn Sie Internet Explorer als Browser verwenden. Es gibt eine Skriptdatei namens **Hubs** , die den SignalR-Bibliothek wird zur Laufzeit dynamisch generiert. Diese Datei, verwaltet die Kommunikation zwischen der jQuery-Skripts und serverseitigen Code. Wenn Sie einen Browser als Internet Explorer verwenden, können Sie auch zugreifen, auf den dynamischen **Hubs** Datei navigieren darin direkt, beispielsweise http://mywebsite/signalr/hubs.

<a id="code"></a>

## <a name="examine-the-code"></a>Überprüfen Sie den Code

Der SignalR Chat-Anwendung zeigt zwei grundlegende Aufgaben für die SignalR-Entwicklung: Erstellen eines Hubs wie das wichtigste Coordination-Objekt, auf dem Server, und mithilfe der SignalR-jQuery-Bibliothek zum Senden und Empfangen von Nachrichten.

### <a name="signalr-hubs"></a>SignalR-Hubs

Im Codebeispiel enthält die **ChatHub** Klasse leitet sich von der **Microsoft.AspNet.SignalR.Hub** Klasse. Ableiten von der **Hub** Klasse ist eine gute Möglichkeit, eine SignalR-Anwendung zu erstellen. Sie können öffentliche Methoden auf Ihrem Hub-Klasse erstellen und dann Zugriff auf diese Methoden durch einen Aufruf über Skripts auf einer Webseite.

Im Code Chat Clients rufen die **ChatHub.Send** Methode, um eine neue Nachricht zu senden. Der Hub wiederum sendet die Nachricht an alle Clients durch den Aufruf **Clients.All.addNewMessageToPage**.

Die **senden** -Methode demonstriert, mehrere Hub-Konzepte:

- Deklarieren Sie öffentliche Methoden für einen Hub, damit Clients, die sie aufrufen können.
- Verwenden der **Microsoft.AspNet.SignalR.Hub.Clients** Eigenschaft, um alle Clients zugreifen, die mit diesem Hub verbunden sind.
- Aufrufen einer Funktion auf dem Client (z. B. die `addNewMessageToPage` Funktion) zum Aktualisieren von Clients.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR und jQuery

Die **Chat.cshtml** Ansicht im Codebeispiel zeigt, wie die SignalR-jQuery-Bibliothek, die zur Kommunikation mit einer SignalR-Hub verwenden. Die wesentlichen Aufgaben in der Code erstellen einen Verweis auf den automatisch generierten Proxy für den Hub, Deklarieren einer Funktion, die Inhalt per Pushvorgang an Clients der Server aufrufen können, und starten eine Verbindung zum Senden von Nachrichten an den Hub.

Der folgende Code deklariert einen Verweis auf eine hubproxy-Klasse.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> In JavaScript wird der Verweis auf die Server-Klasse und ihre Member in Camel-Case. Das Codebeispiel verweist auf die C#- **ChatHub** Klasse in JavaScript als **ChatHub**. Wenn Sie verweisen möchten die `ChatHub` Klasse in jQuery mit herkömmlichen Pascal Groß-/Kleinschreibung wie in c#-Datei der ChatHub.cs bearbeiten. Hinzufügen einer `using` Anweisung auf die `Microsoft.AspNet.SignalR.Hubs` Namespace. Fügen Sie dann die `HubName` -Attribut auf die `ChatHub` Klasse, zum Beispiel `[HubName("ChatHub")]`. Zum Schluss aktualisieren Sie den jQuery-Verweis auf die `ChatHub` Klasse.


Der folgende Code zeigt, wie Sie eine Callback-Funktion im Skript zu erstellen. Die hubklasse auf dem Server ruft diese Funktion, um Inhaltsupdates per Pushvorgang an jeden Client übertragen. Der Aufruf optional die `htmlEncode` Funktion zeigt die Möglichkeit, HTML codieren von Inhalt der Nachricht vor der Anzeige auf der Seite als eine Möglichkeit zum Skript-Injection verhindern.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

Der folgende Code zeigt, wie Sie eine Verbindung mit dem Hub zu öffnen. Der Code wird die Verbindung gestartet und leitet diese dann in eine Funktion zum Behandeln der Click-Ereignis auf der **senden** auf der Seite Chat.

> [!NOTE]
> Dieser Ansatz wird sichergestellt, dass die Verbindung hergestellt wurde, bevor der Ereignishandler ausgeführt wird.


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Nächste Schritte

Sie haben gelernt, dass es sich bei SignalR handelt es sich ein Framework zum Erstellen von Echtzeit-Webanwendungen. Sie erfahren auch mehrere Aufgaben bei der Entwicklung von SignalR: wie eine ASP.NET-Anwendung SignalR hinzugefügt, wie zum Erstellen einer hubklasse und das Senden und Empfangen von Nachrichten aus dem Hub.

Eine exemplarische Vorgehensweise, um die SignalR-beispielanwendung in Azure bereitstellen, finden Sie unter [mithilfe von SignalR mit Web-Apps in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md). Ausführliche Informationen dazu, wie Sie ein Visual Studio-Webprojekt auf einer Windows Azure-Website bereitstellen, finden Sie unter [ASP.NET Web-app in Azure App Service erstellen](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

Erweiterte Konzepte der SignalR-Entwicklungen finden Sie unter den folgenden Websites für SignalR-Quellcode und Ressourcen:

- [SignalR-Projekt](http://signalr.net)
- [SignalR Github und Beispiele](https://github.com/SignalR/SignalR)
- [SignalR-Wiki](https://github.com/SignalR/SignalR/wiki)
