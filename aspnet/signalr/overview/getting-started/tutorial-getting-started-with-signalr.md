---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Lernprogramm: Erste Schritte mit SignalR 2 | Microsoft Docs'
author: pfletcher
description: "Dieses Tutorial zeigt, wie Sie mithilfe von SignalR eine Chatanwendung mit Echtzeitfunktionalität erstellen. Sie werden eine leere ASP.NET-Webanwendung SignalR hinzu, und erstellen eine HTML-Pa..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 3bec32b9b21325cde461541d7a313f401a0cfce7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="tutorial-getting-started-with-signalr-2"></a>Lernprogramm: Erste Schritte mit SignalR 2
====================
durch [Patrick Fletcher](https://github.com/pfletcher)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

> Dieses Tutorial zeigt, wie Sie mithilfe von SignalR eine Chatanwendung mit Echtzeitfunktionalität erstellen. Sie werden eine leere ASP.NET-Webanwendung SignalR hinzu, und erstellen eine HTML-Seite zum Senden und Meldungen angezeigt werden. 
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

Dieses Lernprogramm führt SignalR Entwicklung erfahren, wie eine einfache browserbasierte chatanwendung erstellt. Sie werden eine leere ASP.NET-Webanwendung SignalR-Bibliothek hinzugefügt, erstellen Sie eine hubklasse zum Senden von Nachrichten für Clients und erstellen eine HTML-Seite, mit dem Benutzer, senden und Empfangen von Nachrichten. Ein ähnliches Lernprogramm, das zum Erstellen einer chatanwendung in MVC 5 veranschaulicht eine MVC-Ansicht verwenden, finden Sie unter [erste Schritte mit SignalR-2 und MVC 5](tutorial-getting-started-with-signalr-and-mvc.md).

> [!NOTE]
> Dieses Lernprogramm veranschaulicht das Erstellen von SignalR-Anwendungen in Version 2. Weitere Informationen zu Änderungen zwischen SignalR 1.x und 2, finden Sie unter [aktualisieren SignalR 1.x Projekte](../releases/upgrading-signalr-1x-projects-to-20.md) und [Versionshinweisen zu Visual Studio 2013](../../../visual-studio/overview/2013/release-notes.md#TOC13).

SignalR ist eine Open Source-.NET Bibliothek zum Erstellen von Webanwendungen, die Live Benutzerinteraktionen oder Daten in Echtzeit-Updates erfordern. Beispiele sind soziale Anwendungen, mehrere Benutzer Spiele, Business-Zusammenarbeit und Neuigkeiten, Wetter oder finanzielle Aktualisierung von Anwendungen. Diese werden häufig in Echtzeit Anwendungen bezeichnet.

SignalR vereinfacht das Erstellen von Anwendungen in Echtzeit. Er enthält eine ASP.NET-Serverbibliothek und eine JavaScript-Client-Bibliothek, damit sie leichter verwalten von Client / Server-Verbindungen und Inhaltsupdates an Clients übertragen. Sie können eine vorhandene ASP.NET-Anwendung nach Echtzeit-Funktionen nutzen zu können die SignalR-Bibliothek hinzufügen.

Im Lernprogramm wird die folgenden SignalR Entwicklungsaufgaben veranschaulicht:

- Eine ASP.NET-Webanwendung hinzugefügt SignalR-Bibliothek.
- Erstellen einer hubklasse, um Inhalt an Clients zu verteilen.
- Erstellen eine OWIN-Start-Klasse zum Konfigurieren der Anwendung.
- Verwenden die SignalR-jQuery-Bibliothek auf einer Webseite zum Senden von Nachrichten und Updates vom Hub angezeigt.

Der folgende Screenshot zeigt die chatanwendung, die in einem Browser ausgeführt. Jeder neuer Benutzer kann senden Kommentare und Kommentare hinzugefügt, nachdem der Benutzer über den Chat verknüpft.

![Chatinstanzen](tutorial-getting-started-with-signalr/_static/image1.png)

Abschnitte:

- [Richten Sie das Projekt](#setup)
- [Führen Sie das Beispiel](#run)
- [Überprüfen Sie den Code](#code)
- [Nächste Schritte](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Richten Sie das Projekt

Fügen Sie in diesem Abschnitt wird gezeigt, wie Visual Studio 2013 und SignalR Version 2 Erstellen Sie eine leere ASP.NET-Webanwendung mit SignalR hinzu, und erstellen Sie die chatanwendung.

Erforderliche Komponenten:

- Visual Studio 2013. Wenn Sie nicht über Visual Studio verfügen, finden Sie unter [ASP.NET Downloads](https://www.asp.net/downloads) das kostenlose Visual Studio 2013 Express Entwicklungstool abgerufen.

Verwenden Sie die folgenden Schritte aus Visual Studio 2013 erstellen eine leere ASP.NET-Webanwendung und Hinzufügen der SignalR-Bibliothek:

1. Erstellen Sie in Visual Studio eine ASP.NET-Webanwendung.

    ![Erstellen von web](tutorial-getting-started-with-signalr/_static/image2.png)
2. In der **neues ASP.NET-Projekt** Fenster verlassen **leere** ausgewählt, und klicken Sie auf **Projekt erstellen**.

    ![Erstellen Sie leeres web](tutorial-getting-started-with-signalr/_static/image3.png)
3. In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, wählen Sie **hinzufügen | SignalR-Hub-Klasse (v2)**. Benennen Sie die Klasse **ChatHub.cs** und dem Projekt hinzugefügt. In diesem Schritt wird die **ChatHub** -Klasse und fügt Sie dem Projekt eine Reihe von Skriptdateien und Assemblyverweisen, die SignalR unterstützen.

    > [!NOTE]
    > Sie können auch SignalR zu einem Projekt hinzufügen, indem Sie öffnen die **Tools | Bibliothek-Paket-Manager | Paket-Manager-Konsole** und einen Befehl ausführen:

    `install-package Microsoft.AspNet.SignalR`

    Wenn Sie die Konsole verwenden, um SignalR hinzuzufügen, erstellen Sie die SignalR-Hub-Klasse als separater Schritt nach dem Hinzufügen von SignalR.

    > [!NOTE]
    > Bei Verwendung von Visual Studio 2012 die **SignalR-Hub-Klasse (v2)** Vorlage ist nicht verfügbar. Sie können ein einfachen hinzufügen **Klasse** aufgerufen `ChatHub` stattdessen.
4. In **Projektmappen-Explorer**, erweitern Sie den Knoten des Skripts. Skriptbibliotheken für jQuery und SignalR werden in das Projekt angezeigt.
5. Ersetzen Sie den Code in der neuen **ChatHub** Klasse durch den folgenden Code.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. In **Projektmappen-Explorer**Sie mit der rechten Maustaste des Projekts, und klicken Sie auf **hinzufügen | OWIN-Startklasse**. Benennen Sie die neue Klasse `Startup` , und klicken Sie auf OK.

    > [!NOTE]
    > Bei Verwendung von Visual Studio 2012 die **OWIN-Startklasse** Vorlage ist nicht verfügbar. Sie können ein einfachen hinzufügen **Klasse** aufgerufen `Startup` stattdessen.
7. Ändern Sie den Inhalt der neuen Startklasse folgt.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. In **Projektmappen-Explorer**Sie mit der rechten Maustaste des Projekts, und klicken Sie auf **hinzufügen | HTML-Seite**. Nennen Sie die neue Seite `index.html`.
    >[!NOTE]
    >Sie müssen möglicherweise die Versionsnummern für die Verweise auf Bibliotheken JQuery und SignalR ändern
9. In **Projektmappen-Explorer**mit der rechten Maustaste auf das HTML-Seite, die Sie gerade erstellt haben, und klicken Sie auf **als Startseite festlegen**.
10. Ersetzen Sie den Standardcode in der HTML-Seite mit den folgenden Code ein.

    > [!NOTE]
    > Eine höhere Version der SignalR-Skripts kann von den Paket-Manager installiert werden. Stellen Sie sicher, dass die Versionen der Skriptdateien im Projekt die folgenden Skriptverweise entsprechen (sie werden anders, wenn Sie SignalR mithilfe von NuGet, anstatt einen Hub hinzugefügt.)

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]
11. **Speichern Sie alle** für das Projekt.

<a id="run"></a>

## <a name="run-the-sample"></a>Führen Sie das Beispiel

1. Drücken Sie F5, um das Projekt im Debugmodus auszuführen. Die HTML-Seite lädt in einen Browserinstanz und fordert Sie auf einen Benutzernamen ein.

    ![Benutzername eingeben](tutorial-getting-started-with-signalr/_static/image4.png)
2. Geben Sie einen Benutzernamen ein.
3. Kopieren Sie die URL aus der Adresszeile des Browsers ein, und verwenden Sie, um zwei weitere Browserinstanzen öffnen. Geben Sie in den einzelnen Instanzen Browser einen eindeutigen Benutzernamen aus.
4. Klicken Sie in den einzelnen Instanzen Browser einen Kommentar hinzufügen, und klicken Sie auf **senden**. Die Kommentare sollten in alle Browserinstanzen angezeigt werden.

    > [!NOTE]
    > Diese einfache chatanwendung behält den Kontext der Diskussion auf dem Server. Der Hub, Kommentare, um alle aktuellen Benutzer sendet. Benutzer, die den Chat später beizutreten sehen, dass Nachrichten ab dem Zeitpunkt hinzugefügt werden, dass sie verknüpfen.

    Der folgende Screenshot zeigt die chatanwendung, die in drei Browserinstanzen, die aktualisiert werden, wenn eine Instanz eine Nachricht sendet ausgeführt wird:

    ![Chat-Browser](tutorial-getting-started-with-signalr/_static/image5.png)
5. In **Projektmappen-Explorer**, überprüfen Sie die **Skriptdokumente** Knoten für die ausgeführte Anwendung. Es wird eine Skriptdatei mit dem Namen **Hubs** , die die SignalR-Bibliothek wird zur Laufzeit dynamisch generiert. Diese Datei wird die Kommunikation zwischen jQuery-Skripts und serverseitigen Code verwaltet.

    ![](tutorial-getting-started-with-signalr/_static/image6.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Überprüfen Sie den Code

Die SignalR-Chat-Anwendung zeigt zwei grundlegende SignalR Entwicklungsaufgaben: Erstellen eines Hubs wie das Hauptfenster Koordinierung-Objekt, auf dem Server und mithilfe der SignalR-jQuery-Bibliothek zum Senden und Empfangen von Nachrichten.

### <a name="signalr-hubs"></a>SignalR-Hubs

Im Codebeispiel den **ChatHub** Klasse leitet sich von der **Microsoft.AspNet.SignalR.Hub** Klasse. Ableiten von der **Hub** Klasse ist eine hilfreiche Möglichkeit zum Erstellen einer SignalR-Anwendung. Sie können öffentliche Methoden in der hubklasse erstellen und dann Zugriff auf diese Methoden durch Aufruf aus Skripts auf einer Webseite.

Rufen Sie in den Code Chat Clients die **ChatHub.Send** Methode, um eine neue Nachricht zu senden. Der Hub sendet wiederum die Nachricht an alle Clients durch den Aufruf **Clients.All.broadcastMessage**.

Die **senden** einige Konzepte der Hub-Methode veranschaulicht:

- Öffentliche Methoden für einen Hub deklariert werden, damit Clients, die sie aufrufen können.
- Verwenden der **Microsoft.AspNet.SignalR.Hub.Clients** dynamische Eigenschaft auf alle Clients, die mit diesem Hub verbunden sind.
- Aufrufen einer Funktion auf dem Client (z. B. die `broadcastMessage` Funktion) so aktualisieren Sie Clients.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery"></a>SignalR und jQuery

Die HTML-Seite im Codebeispiel zeigt, wie die SignalR-jQuery-Bibliothek für die Kommunikation mit SignalR-Hubs. Die Durchführung wichtiger Aufgaben im Code deklarieren ein Proxys für den Hub, Deklarieren einer Funktion, die der Server auf Push-Inhalte für Clients aufrufen können, und starten eine Verbindung zum Senden von Nachrichten an den Hub zu verweisen.

Der folgende Code deklariert einen Verweis auf einen hubproxy.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> In JavaScript wird der Verweis auf die Server-Klasse und ihre Member in Camel-Case. Im Codebeispiel verweist auf die C#- **ChatHub** Klasse in JavaScript als **ChatHub**.


Der folgende Code stammt, wie Sie eine Rückruffunktion in das Skript erstellen. Die hubklasse, auf dem Server ruft diese Funktion um Inhaltsupdates an jeden Client per Push übertragen. Die beiden Zeilen an, dass HTML-Codierung den Inhalt vor der Anzeige sind optional, und zeigen eine einfache Möglichkeit zum Skript-Injection verhindern.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

Der folgende Code zeigt, wie Sie eine Verbindung mit dem Hub zu öffnen. Der Code wird die Verbindung gestartet und übergibt diese eine Funktion, um das Click-Ereignis zu behandeln, auf die **senden** Schaltfläche in der HTML-Seite.

> [!NOTE]
> Dieser Ansatz wird sichergestellt, dass die Verbindung hergestellt wird, bevor der Ereignishandler ausgeführt wird.


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

<a id="next"></a>

## <a name="next-steps"></a>Nächste Schritte

Sie erfahren, dass SignalR ein Framework zur Erstellung von Echtzeit-Webanwendungen. Sie haben auch erfahren, mehrere SignalR Entwicklungsaufgaben: zum Hinzufügen von SignalR an eine ASP.NET-Anwendung zum Erstellen einer hubklasse und zum Senden und Empfangen von Nachrichten über den Hub.

Eine exemplarische Vorgehensweise, die SignalR-beispielanwendung in Azure bereitzustellen, finden Sie unter [SignalR mit Web-Apps in Azure App Service mithilfe von](../deployment/using-signalr-with-azure-web-sites.md). Ausführliche Informationen dazu, wie Sie ein Visual Studio-Webprojekt auf einer Windows Azure-Website bereitstellen, finden Sie unter [erstellen eine ASP.NET Web-app in Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-get-started/).

Erweiterte Konzepte von SignalR-Entwicklungen finden Sie auf den folgenden Websites für SignalR-Quellcode und Ressourcen:

- [SignalR-Projekt](http://signalr.net)
- [SignalR Github und Beispiele](https://github.com/SignalR/SignalR)
- [SignalR-Wiki](https://github.com/SignalR/SignalR/wiki)
