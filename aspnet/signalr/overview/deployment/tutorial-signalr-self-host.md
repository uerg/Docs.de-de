---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: 'Lernprogramm: SignalR Selbsthosting | Microsoft Docs'
author: pfletcher
description: Dieses Lernprogramm zeigt, wie beim Erstellen eines selbst gehosteten SignalR-2-Servers und mit einem JavaScript-Client zum Herstellen der Verbindung. Softwareversionen, die im Lernprogramm V verwendet...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: 997756ff8d48e41da981491d6154f3107ec7a051
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="tutorial-signalr-self-host"></a>Lernprogramm: SignalR Selbsthosting
====================
durch [Patrick Fletcher](https://github.com/pfletcher)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> Dieses Lernprogramm zeigt, wie beim Erstellen eines selbst gehosteten SignalR-2-Servers und mit einem JavaScript-Client zum Herstellen der Verbindung.
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
> ## <a name="questions-and-comments"></a>Fragen und Kommentare
> 
> Lassen Sie Sie Feedback auf wie in diesem Lernprogramm mögen und was wir in den Kommentaren am unteren Rand der Seite verbessern können. Wenn Sie Fragen, die nicht direkt mit dem Lernprogramm verknüpft sind haben, bereitstellen können, die [ASP.NET SignalR-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Übersicht

Ein SignalR-Server in der Regel in einer ASP.NET-Anwendung in IIS gehostet wird, aber sie können auch selbst gehostet sein (z. B. in einer Konsolenanwendung oder Windows-Dienst) mithilfe der Selbsthosting-Bibliothek. Dieser Bibliothek ist wie bei allen SignalR-2 baut auf OWIN ([Open Web-Schnittstelle für .NET](http://owin.org)). OWIN definiert eine Abstraktion zwischen .NET Webservern und Webanwendungen. OWIN entkoppelt die Webanwendung aus dem Server, wodurch OWIN ideal für Selbsthosting einer Webanwendung in einen eigenen Prozess, außerhalb von IIS.

Gründe für das nicht in IIS hosten:

- Umgebungen, in denen IIS nicht verfügbar oder wünschenswert sein, z. B. einer vorhandenen Serverfarm ohne IIS.
- Die Beeinträchtigung der Systemleistung von IIS muss vermieden werden.
- SignalR-Funktion ist eine vorhandene Anwendung hinzugefügt werden, die im Windows-Dienst, Azure-Worker-Rolle oder ein anderer Prozess ausgeführt wird.

Wenn als Selbsthosting zur Verbesserung der Leistung eine Lösung entwickelt wird, wurde auch Test der Anwendung in IIS gehostete den Leistungsvorteil bestimmen empfohlen.

Dieses Lernprogramm enthält folgende Abschnitte:

- [Erstellen des Servers](#server)
- [Zugriff auf den Server mit einem JavaScript-client](#js)

<a id="server"></a>

## <a name="creating-the-server"></a>Erstellen des Servers

In diesem Lernprogramm erstellen Sie einen Server, der gehostet wird in einer Konsolenanwendung, aber der Server kann in jede Art von Prozess, z. B. eine Windows-Dienst oder die Azure-workerrolle gehostet werden. Beispielcode für das Hosten eines SignalR-Servers in einem Windows-Dienst, finden Sie unter [Self-Hosting SignalR in einem Windows-Dienst](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).

1. Öffnen Sie Visual Studio 2013 mit Administratorrechten aus. Wählen Sie **Datei**, **neues Projekt**. Wählen Sie **Windows** unter der **Visual C#-** Knoten in der **Vorlagen** , und wählen die **Konsolenanwendung** Vorlage. Nennen Sie das neue Projekt "SignalRSelfHost", und klicken Sie auf **OK**.

    ![](tutorial-signalr-self-host/_static/image1.png)
2. Öffnen Sie die Bibliothek-Paket-Manager-Konsole dazu **Tools**, **Bibliothekspaket-Manager**, **Package Manager Console**.
3. Geben Sie in der Paket-Manager-Konsole den folgenden Befehl aus:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    Mit diesem Befehl wird dem Projekt die Bibliotheken SignalR 2 Self-Host hinzugefügt.
4. Geben Sie in der Paket-Manager-Konsole den folgenden Befehl aus:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    Mit diesem Befehl wird dem Projekt die Microsoft.Owin.Cors-Bibliothek hinzugefügt. Diese Bibliothek wird für die Unterstützung der domänenübergreifenden verwendet werden, die für Anwendungen erforderlich ist, die SignalR und einem Client Webseite in unterschiedlichen Domänen hosten. Da Sie dem SignalR-Server und den WebClient auf unterschiedliche Ports hosten werden, bedeutet dies, dass es sich bei dieser domänenübergreifende für die Kommunikation zwischen diesen Komponenten aktiviert werden muss.
5. Ersetzen Sie den Inhalt von Program.cs durch den folgenden Code.

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    Der obige Code umfasst drei Klassen:

    - **Programm**, einschließlich der **Main** Methode primären Ausführungspfad definieren. Bei dieser Methode wird eine Webanwendung vom Typ **Start** wird an der angegebenen URL gestartet (`http://localhost:8080`). Wenn die Sicherheit für den Endpunkt erforderlich ist, kann SSL implementiert werden. Finden Sie unter [Vorgehensweise: Konfigurieren eines Anschlusses mit einem SSL-Zertifikat](https://msdn.microsoft.com/en-us/library/ms733791.aspx) für Weitere Informationen.
    - **Start**, Klasse, die die Konfiguration für den SignalR-Server enthält (die einzige Konfiguration, die in diesem Lernprogramm wird verwendet, ist der Aufruf an `UseCors`), und der Aufruf von `MapSignalR`, die Routen für Hub Objekte im Projekt erstellt.
    - **MyHub**, die SignalR-Hub-Klasse, die die Anwendung für Clients bereitstellt. Diese Klasse verfügt über eine einzelne Methode **senden**, dass Clients aufgerufen werden, um eine Nachricht an alle anderen verbundenen Clients zu übertragen.
6. Kompilieren Sie die Anwendung, und führen Sie sie aus. Die Adresse, die der Server ausgeführt wird, sollte in einem Konsolenfenster angezeigt.

    ![](tutorial-signalr-self-host/_static/image2.png)
7. Wenn die Ausführung mit der Ausnahme fehlschlägt `System.Reflection.TargetInvocationException was unhandled`, müssen Sie Visual Studio mit Administratorrechten neu starten.
8. Beenden Sie die Anwendung vor dem Fortfahren mit dem nächsten Abschnitt.

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a>Zugriff auf den Server mit einem JavaScript-client

In diesem Abschnitt verwenden Sie den gleichen JavaScript-Client aus der [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md). Wir stellen nur eine Änderung an den Client dem besteht darin, die Hub-URL explizit zu definieren. Mit einer selbst gehosteten Anwendung der Server unbedingt an dieselbe Adresse wie der Verbindungs-URL (aufgrund von Reverseproxys und Lastenausgleichssysteme), also die URL muss explizit definiert werden möglicherweise nicht.

1. In **Projektmappen-Explorer**mit der rechten Maustaste auf die Projektmappe, und wählen Sie **hinzufügen**, **neues Projekt**. Wählen Sie die **Web** Knoten, und wählen die **ASP.NET-Webanwendung** Vorlage. Nennen Sie das Projekt "JavascriptClient", und klicken Sie auf **OK**.

    ![](tutorial-signalr-self-host/_static/image3.png)
2. Wählen Sie die **leere** Vorlage und die übrigen Optionen deaktiviert lassen. Wählen Sie **erstellen Projekt**.

    ![](tutorial-signalr-self-host/_static/image4.png)
3. Wählen Sie in der Paket-Manager-Konsole das Projekt "JavascriptClient" in der **Standardprojekt** Dropdownliste aus, und führen Sie den folgenden Befehl:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    Dieser Befehl installiert SignalR und JQuery-Bibliotheken, die Sie benötigen, auf dem Client.
4. Mit der rechten Maustaste auf Ihr Projekt, und wählen **hinzufügen**, **neues Element**. Wählen Sie die **Web** Knoten, und wählen Sie HTML-Seite. Nennen Sie die Seite **"default.HTML"**.

    ![](tutorial-signalr-self-host/_static/image5.png)
5. Ersetzen Sie den Inhalt für die neue HTML-Seite mit den folgenden Code ein. Stellen Sie sicher, dass die Skriptverweise hier die Skripts im Ordner Scripts des Projekts entsprechen.

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    Der folgende Code (im obigen Codebeispiel hervorgehoben) ist das Hinzufügen, das Sie an den Client verwendet, die im Lernprogramm Stared abrufen (zusätzlich zum Aktualisieren des Codes auf SignalR-Betaversion 2) vorgenommen haben. Diese Codezeile legt explizit die Basis-URL für SignalR fest, auf dem Server.

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. Mit der rechten Maustaste auf die Projektmappe, und wählen Sie **Startprojekte festlegen...** . Wählen Sie die **mehrere Startprojekte** Optionsfeld aus, und legen Sie beide Projekte **Aktion** auf **starten**.

    ![](tutorial-signalr-self-host/_static/image6.png)
7. Mit der rechten Maustaste auf "Default.html", und wählen Sie **als Startseite festlegen**.
8. Führen Sie die Anwendung aus. Die Server und die Seite werden gestartet. Möglicherweise müssen Sie auf der Webseite erneut laden (oder wählen Sie **Fortfahren** im Debugger) Wenn die Seite geladen werden, bevor der Server gestartet wird.
9. Geben Sie einen Benutzernamen an, wenn Sie aufgefordert werden, in den Browser. Kopieren Sie die URL der Seite in eine andere Browserregisterkarte oder Fenster, und geben Sie einen anderen Benutzernamen. Sie werden können zum Senden von Nachrichten aus einem Browserbereich zu "other" wie das erste Schritte-Lernprogramm.
