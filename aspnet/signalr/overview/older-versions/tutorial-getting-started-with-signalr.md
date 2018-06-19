---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: 'Lernprogramm: Erste Schritte mit SignalR 1.x | Microsoft Docs'
author: pfletcher
description: Verwenden Sie ASP.NET SignalR eine Echtzeit-Chat-Anwendung in eine HTML-Seite erstellt.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: ce4953a0abf64af28ef4dbc5a62bb2d989343d99
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
ms.locfileid: "28044733"
---
<a name="tutorial-getting-started-with-signalr-1x"></a>Lernprogramm: Erste Schritte mit SignalR 1.x
====================
durch [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

> Dieses Tutorial zeigt, wie Sie mithilfe von SignalR eine Chatanwendung mit Echtzeitfunktionalität erstellen. Sie werden eine leere ASP.NET-Webanwendung SignalR hinzu, und erstellen eine HTML-Seite zum Senden und Meldungen angezeigt werden.


## <a name="overview"></a>Übersicht

Dieses Lernprogramm führt SignalR Entwicklung erfahren, wie eine einfache browserbasierte chatanwendung erstellt. Sie werden eine leere ASP.NET-Webanwendung SignalR-Bibliothek hinzugefügt, erstellen Sie eine hubklasse zum Senden von Nachrichten für Clients und erstellen eine HTML-Seite, mit dem Benutzer, senden und Empfangen von Nachrichten. Ein ähnliches Lernprogramm, das zum Erstellen einer chatanwendung in MVC 4 veranschaulicht eine MVC-Ansicht verwenden, finden Sie unter [erste Schritte mit SignalR und MVC 4](index.md).

> [!NOTE]
> Dieses Lernprogramm verwendet die Releaseversion (1.x) der SignalR. Weitere Informationen zu Änderungen zwischen SignalR 1.x und 2.0 finden Sie unter [aktualisieren SignalR 1.x Projekte](../releases/upgrading-signalr-1x-projects-to-20.md).

SignalR ist eine Open Source-.NET Bibliothek zum Erstellen von Webanwendungen, die Live Benutzerinteraktionen oder Daten in Echtzeit-Updates erfordern. Beispiele sind soziale Anwendungen, mehrere Benutzer Spiele, Business-Zusammenarbeit und Neuigkeiten, Wetter oder finanzielle Aktualisierung von Anwendungen. Diese werden häufig in Echtzeit Anwendungen bezeichnet.

SignalR vereinfacht das Erstellen von Anwendungen in Echtzeit. Er enthält eine ASP.NET-Serverbibliothek und eine JavaScript-Client-Bibliothek, damit sie leichter verwalten von Client / Server-Verbindungen und Inhaltsupdates an Clients übertragen. Sie können eine vorhandene ASP.NET-Anwendung nach Echtzeit-Funktionen nutzen zu können die SignalR-Bibliothek hinzufügen.

Im Lernprogramm wird die folgenden SignalR Entwicklungsaufgaben veranschaulicht:

- Eine ASP.NET-Webanwendung hinzugefügt SignalR-Bibliothek.
- Erstellen einer hubklasse, um Inhalt an Clients zu verteilen.
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

Fügen Sie in diesem Abschnitt wird gezeigt, wie eine leere ASP.NET-Webanwendung erstellt SignalR hinzu, und erstellen Sie die chatanwendung.

Erforderliche Komponenten:

- Visual Studio 2010 SP1 oder 2012. Wenn Sie nicht über Visual Studio verfügen, finden Sie unter [ASP.NET Downloads](https://www.asp.net/downloads) das kostenlose Visual Studio 2012 Express Entwicklungstool abgerufen.
- [Microsoft ASP.NET und Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941). Für Visual Studio 2012 fügt dieses Installationsprogramm neue ASP.NET-Funktionen einschließlich SignalR-Vorlagen zu Visual Studio. Für Visual Studio 2010 SP1 in einem Installationsprogramm ist nicht verfügbar, aber Sie können das Lernprogramm abschließen, indem die SignalR-NuGet-Paket installieren, wie in den setupschritten beschrieben.

Die folgenden Schritte verwenden Visual Studio 2012, um eine leere ASP.NET-Webanwendung erstellen und Hinzufügen der SignalR-Bibliothek:

1. Erstellen Sie in Visual Studio eine leere ASP.NET-Webanwendung.

    ![Erstellen Sie leeres web](tutorial-getting-started-with-signalr/_static/image2.png)
2. Öffnen der **Package Manager Console** dazu **Tools | Bibliothek-Paket-Manager | Paket-Manager-Konsole**. Geben Sie den folgenden Befehl in das Konsolenfenster aus:

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    Dieser Befehl installiert die neueste Version von SignalR 1.x.
3. In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, wählen Sie **hinzufügen | Klasse**. Benennen Sie die neue Klasse **ChatHub**.
4. In **Projektmappen-Explorer** erweitern Sie den Knoten des Skripts. Skriptbibliotheken für jQuery und SignalR werden in das Projekt angezeigt.

    ![Verweise auf](tutorial-getting-started-with-signalr/_static/image3.png)
5. Ersetzen Sie den Code in der **ChatHub** Klasse durch den folgenden Code.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. In **Projektmappen-Explorer**Sie mit der rechten Maustaste des Projekts, und klicken Sie auf **hinzufügen | Neues Element**. In der **neues Element hinzufügen** wählen Sie im Dialogfeld **Globale Anwendungsklasse** , und klicken Sie auf **hinzufügen**.

    ![Globale hinzufügen](tutorial-getting-started-with-signalr/_static/image4.png)
7. Fügen Sie die folgenden `using` Anweisungen nach der bereitgestellten `using` Anweisungen in der Global.asax.cs-Klasse.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. Fügen Sie die folgende Codezeile in der `Application_Start` -Methode der globalen Klasse um die Standardroute für SignalR-Hubs zu registrieren.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. In **Projektmappen-Explorer**Sie mit der rechten Maustaste des Projekts, und klicken Sie auf **hinzufügen | Neues Element**. In der **neues Element hinzufügen** Dialogfeld, wählen Sie Html-Seite, und klicken Sie auf **hinzufügen**.
10. In **Projektmappen-Explorer**mit der rechten Maustaste auf das HTML-Seite, die Sie gerade erstellt haben, und klicken Sie auf **als Startseite festlegen**.
11. Ersetzen Sie den Standardcode in der HTML-Seite mit den folgenden Code ein.

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. **Speichern Sie alle** für das Projekt.

<a id="run"></a>

## <a name="run-the-sample"></a>Führen Sie das Beispiel

1. Drücken Sie F5, um das Projekt im Debugmodus auszuführen. Die HTML-Seite lädt in einen Browserinstanz und fordert Sie auf einen Benutzernamen ein.

    ![Benutzername eingeben](tutorial-getting-started-with-signalr/_static/image5.png)
2. Geben Sie einen Benutzernamen ein.
3. Kopieren Sie die URL aus der Adresszeile des Browsers ein, und verwenden Sie, um zwei weitere Browserinstanzen öffnen. Geben Sie in den einzelnen Instanzen Browser einen eindeutigen Benutzernamen aus.
4. Klicken Sie in den einzelnen Instanzen Browser einen Kommentar hinzufügen, und klicken Sie auf **senden**. Die Kommentare sollten in alle Browserinstanzen angezeigt werden.

    > [!NOTE]
    > Diese einfache chatanwendung behält den Kontext der Diskussion auf dem Server. Der Hub, Kommentare, um alle aktuellen Benutzer sendet. Benutzer, die den Chat später beizutreten sehen, dass Nachrichten ab dem Zeitpunkt hinzugefügt werden, dass sie verknüpfen.

    Der folgende Screenshot zeigt die chatanwendung, die in drei Browserinstanzen, die aktualisiert werden, wenn eine Instanz eine Nachricht sendet ausgeführt wird:

    ![Chat-Browser](tutorial-getting-started-with-signalr/_static/image6.png)
5. In **Projektmappen-Explorer**, überprüfen Sie die **Skriptdokumente** Knoten für die ausgeführte Anwendung. Es wird eine Skriptdatei mit dem Namen **Hubs** , die die SignalR-Bibliothek wird zur Laufzeit dynamisch generiert. Diese Datei wird die Kommunikation zwischen jQuery-Skripts und serverseitigen Code verwaltet.

    ![Generierte Hub-Skript](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Überprüfen Sie den Code

Die SignalR-Chat-Anwendung zeigt zwei grundlegende SignalR Entwicklungsaufgaben: Erstellen eines Hubs wie das Hauptfenster Koordinierung-Objekt, auf dem Server und mithilfe der SignalR-jQuery-Bibliothek zum Senden und Empfangen von Nachrichten.

### <a name="signalr-hubs"></a>SignalR Hubs

Im Codebeispiel den **ChatHub** Klasse leitet sich von der **Microsoft.AspNet.SignalR.Hub** Klasse. Ableiten von der **Hub** Klasse ist eine hilfreiche Möglichkeit zum Erstellen einer SignalR-Anwendung. Sie können öffentliche Methoden in der hubklasse erstellen und dann Zugriff auf diese Methoden durch Aufruf aus jQuery-Skripts auf einer Webseite.

Rufen Sie in den Code Chat Clients die **ChatHub.Send** Methode, um eine neue Nachricht zu senden. Der Hub sendet wiederum die Nachricht an alle Clients durch den Aufruf **Clients.All.broadcastMessage**.

Die **senden** einige Konzepte der Hub-Methode veranschaulicht:

- Öffentliche Methoden für einen Hub deklariert werden, damit Clients, die sie aufrufen können.
- Verwenden der **Microsoft.AspNet.SignalR.Hub.Clients** dynamische Eigenschaft auf alle Clients, die mit diesem Hub verbunden sind.
- Aufrufen einer jQuery-Funktion auf dem Client (z. B. die `broadcastMessage` Funktion) so aktualisieren Sie Clients.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR und jQuery

Die HTML-Seite im Codebeispiel zeigt, wie die SignalR-jQuery-Bibliothek für die Kommunikation mit SignalR-Hubs. Die Durchführung wichtiger Aufgaben im Code deklarieren ein Proxys für den Hub, Deklarieren einer Funktion, die der Server auf Push-Inhalte für Clients aufrufen können, und starten eine Verbindung zum Senden von Nachrichten an den Hub zu verweisen.

Der folgende Code deklariert einen Proxy für einen Hub.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> In jQuery wird der Verweis auf die Server-Klasse und ihre Member in Camel-Case. Im Codebeispiel verweist auf die C#- **ChatHub** Klasse in jQuery als **ChatHub**.


Der folgende Code stammt, wie Sie eine Rückruffunktion in das Skript erstellen. Die hubklasse, auf dem Server ruft diese Funktion um Inhaltsupdates an jeden Client per Push übertragen. Die beiden Zeilen an, dass HTML-Codierung den Inhalt vor der Anzeige sind optional, und zeigen eine einfache Möglichkeit zum Skript-Injection verhindern.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

Der folgende Code zeigt, wie Sie eine Verbindung mit dem Hub zu öffnen. Der Code wird die Verbindung gestartet und übergibt diese eine Funktion, um das Click-Ereignis zu behandeln, auf die **senden** Schaltfläche in der HTML-Seite.

> [!NOTE]
> Dieser Ansatz wird sichergestellt, dass die Verbindung hergestellt wird, bevor der Ereignishandler ausgeführt wird.


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Nächste Schritte

Sie erfahren, dass SignalR ein Framework zur Erstellung von Echtzeit-Webanwendungen. Sie haben auch erfahren, mehrere SignalR Entwicklungsaufgaben: zum Hinzufügen von SignalR an eine ASP.NET-Anwendung zum Erstellen einer hubklasse und zum Senden und Empfangen von Nachrichten über den Hub.

Sie können die beispielanwendung in diesem Lernprogramm oder andere SignalR-Anwendungen verfügbar über das Internet, indem sie mit einem Hostinganbieter bereitstellen. Microsoft bietet kostenlose Webhosting für bis zu 10 Websites im ein kostenloses [Windows Azure-Testkonto](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Eine exemplarische Vorgehensweise zum Bereitstellen der SignalR-beispielanwendung finden Sie unter [SignalR erste Schritte Beispiel als ein Windows Azure-Website veröffentlichen](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx). Ausführliche Informationen dazu, wie Sie ein Visual Studio-Webprojekt auf einer Windows Azure-Website bereitstellen, finden Sie unter [Bereitstellung einer ASP.NET-Anwendung auf einem Windows Azure-Website](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet). (Hinweis: die WebSocket-Transport wird derzeit nicht unterstützt für Windows Azure-Websites. Bei der WebSocket-Transport ist nicht verfügbar, SignalR verwendet die andere verfügbare Transporte wie beschrieben im Abschnitt Transporte der [Einführung zu SignalR Thema](index.md).)

Erweiterte Konzepte von SignalR-Entwicklungen finden Sie auf den folgenden Websites für SignalR-Quellcode und Ressourcen:

- [SignalR-Projekt](http://signalr.net)
- [SignalR Github und Beispiele](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
