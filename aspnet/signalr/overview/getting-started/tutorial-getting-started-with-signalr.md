---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Tutorial: Erste Schritte mit SignalR 2 | Microsoft-Dokumentation'
author: pfletcher
description: Dieses Tutorial zeigt, wie Sie mithilfe von SignalR eine Chatanwendung mit Echtzeitfunktionalität erstellen. Sie werden eine leere ASP.NET-Webanwendung SignalR hinzu, und erstellen eine HTML-Pa...
ms.author: aspnetcontent
ms.date: 06/10/2014
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 798838af099cceb12652b7c6c66633a03a73e538
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841841"
---
<a name="tutorial-getting-started-with-signalr-2"></a>Tutorial: Erste Schritte mit SignalR 2
====================
durch [Patrick Fletcher](https://github.com/pfletcher)

[Abgeschlossenes Projekt herunterladen](http://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

> Dieses Tutorial zeigt, wie Sie mithilfe von SignalR eine Chatanwendung mit Echtzeitfunktionalität erstellen. Sie werden eine leere ASP.NET-Webanwendung SignalR hinzu, und erstellen Sie eine HTML-Seite zum Senden und Meldungen angezeigt werden. 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
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

In diesem Tutorial wird SignalR-Entwicklung eingeführt, wie eine einfache browserbasierte Chat-Anwendung zu erstellen. Sie werden die SignalR-Bibliothek mit einer leeren ASP.NET-Webanwendung hinzufügen, erstellen Sie eine hubklasse für das Senden von Nachrichten für Clients und erstellen Sie eine HTML-Seite, mit dem Benutzer, senden und Empfangen von Chatnachrichten. Ein ähnliches Tutorial, das Erstellen eine chatanwendung in MVC 5 zeigt die Verwendung einer MVC-Ansicht, finden Sie unter [erste Schritte mit SignalR 2 und MVC 5](tutorial-getting-started-with-signalr-and-mvc.md).

> [!NOTE]
> Dieses Tutorial veranschaulicht das Erstellen von SignalR-Anwendungen in Version 2. Weitere Informationen zu Änderungen zwischen SignalR 1.x und 2 finden Sie unter [aktualisieren SignalR 1.x-Projekte](../releases/upgrading-signalr-1x-projects-to-20.md) und [Versionsanmerkungen von Visual Studio 2013](../../../visual-studio/overview/2013/release-notes.md#TOC13).

SignalR ist eine Open Source-.NET Bibliothek zum Erstellen von Webanwendungen, die live Benutzerinteraktionen oder datenaktualisierungen in Echtzeit erfordern. Beispiele hierfür sind soziale Anwendungen, mehrere Benutzer Spiele, Business-Zusammenarbeit und Neuigkeiten, Wetter oder finanzielle Anwendungsupdates an. Diese werden häufig in Echtzeit Anwendungen bezeichnet.

SignalR vereinfacht das Erstellen von Echtzeit-Anwendungen. Es enthält eine Server-Bibliothek von ASP.NET und eine JavaScript-Clientbibliothek zum Verwalten von Client / Server-Verbindungen ein, und Übertragen von Inhaltsupdates an Clients zu erleichtern. Sie können eine vorhandene ASP.NET-Anwendung um Echtzeitfunktionen zu erhalten. die SignalR-Bibliothek hinzufügen.

Das Tutorial veranschaulicht die folgenden Entwicklungsaufgaben von SignalR:

- Eine ASP.NET-Webanwendung wird die SignalR-Bibliothek hinzugefügt.
- Erstellen einer hubklasse, um Inhalt an Clients zu verteilen.
- Erstellen eine OWIN-Startklasse zum Konfigurieren der Anwendung.
- Verwenden die SignalR-jQuery-Bibliothek auf einer Webseite zum Senden von Nachrichten und Updates vom Hub angezeigt.

Der folgende Screenshot zeigt die chatanwendung, die in einem Browser ausgeführt. Jeden neuer Benutzer kann Kommentare Posten und Kommentare hinzugefügt, nachdem der Benutzer im Chat verknüpft.

![Chatinstanzen](tutorial-getting-started-with-signalr/_static/image1.png)

Abschnitte:

- [Einrichten des Projekts](#setup)
- [Ausführen des Beispiels](#run)
- [Überprüfen Sie den Code](#code)
- [Nächste Schritte](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Einrichten des Projekts

Fügen Sie in diesem Abschnitt zeigt, wie Sie Visual Studio 2013 und SignalR Version 2 zu verwenden, um eine leere ASP.NET-Webanwendung erstellen SignalR hinzu, und erstellen Sie die chatanwendung.

Erforderliche Komponenten:

- Visual Studio 2013. Wenn Sie nicht über Visual Studio verfügen, finden Sie unter [ASP.NET-Downloads](https://www.asp.net/downloads) um die kostenlose Visual Studio 2013 Express Entwicklungstool zu erhalten.

Verwenden Sie die folgenden Schritte aus Visual Studio 2013 erstellen eine leere ASP.NET-Webanwendung und Hinzufügen von SignalR-Bibliothek:

1. Erstellen Sie eine ASP.NET-Webanwendung in Visual Studio.

    ![Erstellen Sie web](tutorial-getting-started-with-signalr/_static/image2.png)
2. In der **neues ASP.NET-Projekt** Fenster verlassen **leere** ausgewählt, und klicken Sie auf **Erstellen eines Projekts**.

    ![Leeres Web erstellen](tutorial-getting-started-with-signalr/_static/image3.png)
3. In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, wählen Sie **hinzufügen | SignalR-Hubklasse (v2)**. Nennen Sie die Klasse **ChatHub.cs** und fügen sie dem Projekt hinzu. Dieser Schritt erstellt der **ChatHub** -Klasse und fügt Sie dem Projekt einen Satz von Skriptdateien und Assemblyverweise, die SignalR unterstützen.

    > [!NOTE]
    > Sie können auch SignalR zu einem Projekt hinzufügen, indem Sie öffnen die **Tools | Bibliothekspaket-Manager | Paket-Manager-Konsole** und einen Befehl ausführen:

    `install-package Microsoft.AspNet.SignalR`

    Wenn Sie die Konsole verwenden, um SignalR hinzuzufügen, erstellen Sie die SignalR-hubklasse als separater Schritt nach dem Hinzufügen von SignalR.

    > [!NOTE]
    > Wenn Sie Visual Studio 2012, verwenden die **SignalR-Hubklasse (v2)** Vorlage nicht zur Verfügung. Sie können ein einfacher hinzufügen **Klasse** namens `ChatHub` stattdessen.
4. In **Projektmappen-Explorer**, erweitern Sie den Knoten des Skripts. Skriptbibliotheken für jQuery und SignalR werden in das Projekt angezeigt.
5. Ersetzen Sie den Code in der neuen **ChatHub** Klasse durch den folgenden Code.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und klicken Sie auf **hinzufügen | OWIN-Startklasse**. Nennen Sie die neue Klasse `Startup` , und klicken Sie auf OK.

    > [!NOTE]
    > Wenn Sie Visual Studio 2012, verwenden die **OWIN-Startklasse** Vorlage nicht zur Verfügung. Sie können ein einfacher hinzufügen **Klasse** namens `Startup` stattdessen.
7. Ändern Sie den Inhalt der neuen Startup-Klasse, die der folgenden.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und klicken Sie auf **hinzufügen | HTML-Seite**. Nennen Sie die neue Seite `index.html`.
    >[!NOTE]
    >Sie müssen möglicherweise die Versionsnummern für die Verweise auf JQuery und SignalR-Bibliotheken zu ändern
9. In **Projektmappen-Explorer**mit der rechten Maustaste auf die HTML-Seite, die Sie gerade erstellt haben, und klicken Sie auf **als Startseite festlegen**.
10. Ersetzen Sie den Standardcode in der HTML-Seite mit den folgenden Code ein.

    > [!NOTE]
    > Eine höhere Version von SignalR-Skripts kann durch den Paket-Manager installiert werden. Stellen Sie sicher, dass die folgenden Skriptverweise zu den Versionen der Skriptdateien im Projekt entsprechen (sie werden anders, wenn Sie SignalR, einen Hub hinzufügen, sondern mithilfe von NuGet hinzugefügt haben.)

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]
11. **Speichern Sie alle** für das Projekt.

<a id="run"></a>

## <a name="run-the-sample"></a>Ausführen des Beispiels

1. Drücken Sie F5, um das Projekt im Debugmodus auszuführen. Die HTML-Seite lädt in eine Instanz eines Webbrowsers und aufforderungen für einen Benutzernamen ein.

    ![Benutzername eingeben](tutorial-getting-started-with-signalr/_static/image4.png)
2. Geben Sie einen Benutzernamen ein.
3. Kopieren Sie die URL aus der Adresszeile des Browsers, und öffnen Sie zwei Browserinstanzen für weitere. Geben Sie in den einzelnen Instanzen Browser einen eindeutigen Benutzernamen ein.
4. Klicken Sie in den einzelnen Instanzen Browser Fügt einen Kommentar hinzu, und klicken Sie auf **senden**. Die Kommentare sollte in alle Browserinstanzen angezeigt werden.

    > [!NOTE]
    > Diese einfache chatanwendung werden den Kontext der Diskussion auf dem Server nicht verwaltet werden. Der Hub sendet, Kommentare, um alle aktuellen Benutzer. Benutzer, die im Chat später zusammenfügen sehen, dass Nachrichten ab dem Zeitpunkt hinzugefügt werden, dass sie aufgenommen werden.

    Der folgende Screenshot zeigt die Chat-Anwendung, die auf drei Browserinstanzen, die aktualisiert werden, wenn eine Instanz eine Nachricht sendet:

    ![Chat-Browser](tutorial-getting-started-with-signalr/_static/image5.png)
5. In **Projektmappen-Explorer**, überprüfen Sie die **Skriptdokumente** Knoten für die ausgeführte Anwendung. Es gibt eine Skriptdatei namens **Hubs** , die den SignalR-Bibliothek wird zur Laufzeit dynamisch generiert. Diese Datei, verwaltet die Kommunikation zwischen der jQuery-Skripts und serverseitigen Code.

    ![](tutorial-getting-started-with-signalr/_static/image6.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Überprüfen Sie den Code

Der SignalR Chat-Anwendung zeigt zwei grundlegende Aufgaben für die SignalR-Entwicklung: Erstellen eines Hubs wie das wichtigste Coordination-Objekt, auf dem Server, und mithilfe der SignalR-jQuery-Bibliothek zum Senden und Empfangen von Nachrichten.

### <a name="signalr-hubs"></a>SignalR-Hubs

Im Codebeispiel enthält die **ChatHub** Klasse leitet sich von der **Microsoft.AspNet.SignalR.Hub** Klasse. Ableiten von der **Hub** Klasse ist eine gute Möglichkeit, eine SignalR-Anwendung zu erstellen. Sie können öffentliche Methoden auf Ihrem Hub-Klasse erstellen und dann Zugriff auf diese Methoden durch einen Aufruf über Skripts auf einer Webseite.

Im Code Chat Clients rufen die **ChatHub.Send** Methode, um eine neue Nachricht zu senden. Der Hub wiederum sendet die Nachricht an alle Clients durch den Aufruf **Clients.All.broadcastMessage**.

Die **senden** -Methode demonstriert, mehrere Hub-Konzepte:

- Deklarieren Sie öffentliche Methoden für einen Hub, damit Clients, die sie aufrufen können.
- Verwenden der **Microsoft.AspNet.SignalR.Hub.Clients** dynamische Eigenschaft alle Clients den Zugriff auf, die mit diesem Hub verbunden sind.
- Aufrufen einer Funktion auf dem Client (z. B. die `broadcastMessage` Funktion) zum Aktualisieren von Clients.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery"></a>SignalR und jQuery

Die HTML-Seite im Codebeispiel veranschaulicht, wie die SignalR-jQuery-Bibliothek für die Kommunikation mit einer SignalR-Hub. Die wesentlichen Aufgaben in den Code deklarieren einen Proxy für den Hub, Deklarieren einer Funktion, die Inhalt per Pushvorgang an Clients der Server aufrufen können, und starten eine Verbindung zum Senden von Nachrichten an den Hub zu verweisen.

Der folgende Code deklariert einen Verweis auf eine hubproxy-Klasse.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> In JavaScript wird der Verweis auf die Server-Klasse und ihre Member in Camel-Case. Das Codebeispiel verweist auf die C#- **ChatHub** Klasse in JavaScript als **ChatHub**.


Der folgende Code ist, wie Sie eine Callback-Funktion im Skript erstellen. Die hubklasse auf dem Server ruft diese Funktion, um Inhaltsupdates per Pushvorgang an jeden Client übertragen. Die beiden Zeilen an, dass die HTML-Codierung vor der Anzeige des Inhalts sind optional, und zeigen eine einfache Möglichkeit zum Skript-Injection verhindern.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

Der folgende Code zeigt, wie Sie eine Verbindung mit dem Hub zu öffnen. Der Code wird die Verbindung gestartet und leitet diese dann in eine Funktion zum Behandeln der Click-Ereignis auf der **senden** auf der HTML-Seite.

> [!NOTE]
> Dieser Ansatz wird sichergestellt, dass die Verbindung hergestellt wurde, bevor der Ereignishandler ausgeführt wird.


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

<a id="next"></a>

## <a name="next-steps"></a>Nächste Schritte

Sie haben gelernt, dass es sich bei SignalR handelt es sich ein Framework zum Erstellen von Echtzeit-Webanwendungen. Sie erfahren auch mehrere Aufgaben bei der Entwicklung von SignalR: wie eine ASP.NET-Anwendung SignalR hinzugefügt, wie zum Erstellen einer hubklasse und das Senden und Empfangen von Nachrichten aus dem Hub.

Eine exemplarische Vorgehensweise, um die SignalR-beispielanwendung in Azure bereitstellen, finden Sie unter [mithilfe von SignalR mit Web-Apps in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md). Ausführliche Informationen dazu, wie Sie ein Visual Studio-Webprojekt auf einer Windows Azure-Website bereitstellen, finden Sie unter [ASP.NET Web-app in Azure App Service erstellen](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

Erweiterte Konzepte der SignalR-Entwicklungen finden Sie unter den folgenden Websites für SignalR-Quellcode und Ressourcen:

- [SignalR-Projekt](http://signalr.net)
- [SignalR Github und Beispiele](https://github.com/SignalR/SignalR)
- [SignalR-Wiki](https://github.com/SignalR/SignalR/wiki)
