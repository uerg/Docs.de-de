---
uid: signalr/overview/getting-started/real-time-web-applications-with-signalr
title: 'Praktische Übungseinheiten: Echtzeit-Webanwendungen mit SignalR | Microsoft Docs'
author: rick-anderson
description: Echtzeit-Webanwendungen bieten die Möglichkeit, serverseitige an verbundene Clients Inhalte, sobald sie auftreten, können Sie in Echtzeit mithilfe von push. Für ASP.NET-Entwickler,, ASP...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: ba07958c-42e1-4da0-81db-ba6925ed6db0
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/real-time-web-applications-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 5a2bc120ded18ad2302fd6c5cde65a5323e86ca8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878047"
---
<a name="hands-on-lab-real-time-web-applications-with-signalr"></a>Praktische Übungseinheiten: Echtzeit-Webanwendungen mit SignalR
====================
Durch [Web Lager Team](https://twitter.com/webcamps)

[Herunterladen von Web-Lager Training Kit](http://aka.ms/webcamps-training-kit)

> Echtzeit-Webanwendungen bieten die Möglichkeit, serverseitige an verbundene Clients Inhalte, sobald sie auftreten, können Sie in Echtzeit mithilfe von push. Für ASP.NET-Entwickler, **ASP.NET SignalR** ist eine Bibliothek Echtzeit-Webfunktionen zu ihren Anwendungen hinzufügen. Er nutzt verschiedene Transporte, die automatisch die am besten verfügbaren Transport erhält der Client und des Servers am besten verfügbaren Transport auswählen. Nutzt die Vorteile der **WebSocket**, eine HTML5-API, die bidirektionale Kommunikation zwischen dem Browser und dem Server ermöglicht.
> 
> **SignalR** auch bietet eine einfache, allgemeine API auf diese Weise Server zum Client RPC (rufen Sie JavaScript-Funktionen in Ihrer Kunden Browsern aus serverseitigem .NET Code) in Ihrer ASP.NET-Anwendung sowie nützliche Hooks für die verbindungsverwaltung, hinzufügen z. B. eine Verbindung herstellen/trennen Sie Ereignisse, Gruppierung Verbindungen und Autorisierung.
> 
> **SignalR** ist eine Abstraktion über einige der verwendete Transporte, die in Echtzeit Arbeit zwischen Client und Server erforderlich sind. Ein **SignalR** Verbindung HTTP startet, und klicken Sie dann auf heraufgestuft wird eine **WebSocket** Verbindung, falls verfügbar. **WebSocket** ist die ideale Transport für **SignalR**, da die effizienteste Nutzung des Serverspeichers vereinfacht, hat die niedrigste Latenz und die meisten zugrunde liegenden Funktionen (z. B. die Vollduplex-Kommunikation zwischen Client und Server), aber sie hat auch die strengsten Anforderungen: **WebSocket** erfordert, dass der Server verwenden **Windows Server 2012** oder **Windows 8**, zusammen mit **.NET Framework 4.5**. Wenn diese Anforderungen nicht erfüllt sind, **SignalR** wird versucht, andere Transporte verwenden, um dessen Verbindungen zu machen (z. B. *Ajax lange abrufen*).
> 
> Die **SignalR** -API enthält zwei Modelle für die Kommunikation zwischen Clients und Server: **dauerhafte Verbindungen** und **Hubs**. Ein **Verbindung** stellt einen einfachen Endpunkt dar, für die einzelnen-Empfänger senden gruppiert oder Senden von Nachrichten. Ein **Hub** ist eine grobe Pipeline, die auf den Verbindungs-API, die der Client-als auch direkt aufrufen von Methoden voneinander ermöglicht basiert.
> 
> ![SignalR-Architektur](real-time-web-applications-with-signalr/_static/image1.png)
> 
> Alle Beispielcode und Codeausschnitte sind im Web Lager Training Kit unter enthalten [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).


<a id="Overview"></a>
## <a name="overview"></a>Übersicht

<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In dieser praktischen Übungseinheit erfahren Sie, wie Sie:

- Senden von Benachrichtigungen von Server über SignalR Client.
- Dezentrales Skalieren Ihrer Anwendung mit SignalR **SQL Server**.

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Erforderliche Komponenten

Folgendes ist erforderlich, um diese praktische Übungseinheit:

- [Visual Studio Express 2013 für Web](https://www.microsoft.com/visualstudio/) oder höher

<a id="Setup"></a>
### <a name="setup"></a>Setup

Um die Übungen in dieser praktischen Übungseinheit auszuführen, müssen Sie zunächst die Umgebung einrichten.

1. Öffnen Sie ein Windows-Explorer-Fenster, und navigieren Sie in des Labors **Quelle** Ordner.
2. Mit der rechten Maustaste **"Setup.cmd"** , und wählen Sie **als Administrator ausführen** um den Setupvorgang zu starten, die Ihrer Umgebung konfigurieren und installieren die Visual Studio-Codeausschnitte für diese Übungseinheit.
3. Wenn das Dialogfeld Benutzerkontensteuerung angezeigt wird, bestätigen Sie die Aktion aus, um den Vorgang fortzusetzen.

> [!NOTE]
> Stellen Sie sicher, dass Sie alle Abhängigkeiten für diese Umgebung aktiviert haben, bevor Sie das Setup ausführen.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Verwenden die Codeausschnitte

In der Lab-Dokument werden Sie aufgefordert, Codeblöcke einfügen. Der Einfachheit halber die meisten dieser Code dient als Visual Studio-Codeausschnitte, die Sie in Visual Studio 2013 zu vermeiden, dass sie manuell hinzufügen aus zugreifen können.

> [!NOTE]
> Jede Übung wird eine beginnend Projektmappe befindet sich im beiliegen der **beginnen** Ordner der Übung, die Ihnen ermöglicht, jede Übung unabhängig von den anderen folgen. Denken Sie daran, dass die Codeausschnitte, die während einer Übung hinzugefügt werden in diese Lösungen starten fehlen, werden und funktionieren möglicherweise nicht, bis die Übung abgeschlossen ist. In den Quellcode für eine Übung, finden Sie auch eine **End** Ordner, der durch den Code, der aus den Schritten in der entsprechenden Übung führt Visual Studio-Projektmappe enthält. Sie können diese Lösungen als Leitfaden verwenden, wenn Sie weitere Hilfe benötigen, wie Sie mithilfe dieser praktischen Übungseinheit arbeiten.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Übungen

Diese praktische Übungseinheit enthält die folgenden Übungen durcharbeiten:

1. [Arbeiten mit Daten in Echtzeit mit SignalR](#Exercise1)
2. [Dezentrales Skalieren mithilfe von SQLServer](#Exercise2)

Geschätzte Zeit zum Abschließen dieser testumgebung: **60 Minuten**

> [!NOTE]
> Wenn Sie Visual Studio zum ersten Mal starten, müssen Sie eine der vordefinierten Einstellungen Sammlungen auswählen. Jede vordefinierte Sammlung dient zur einer bestimmten Entwicklungsstil und Fensterlayouts, Editor-Verhalten, IntelliSense-Codeausschnitte und Optionen des Dialogfelds bestimmt. In dieser Übung wird beschrieben, die Aktionen erforderlich, um eine bestimmte Aufgabe in Visual Studio auszuführen, bei Verwendung der **allgemeine Entwicklungseinstellungen** Auflistung. Wählen Sie eine Auflistung von unterschiedlichen Einstellungen für Ihre Entwicklungsumgebung möglicherweise Unterschiede in den Schritten, die Sie berücksichtigen sollten.


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-real-time-data-using-signalr"></a>Übung 1: Arbeiten mit Daten in Echtzeit mit SignalR

Während der Chat häufig als Beispiel verwendet wird, erreichen Sie eine ganze noch viel mehr mit Echtzeit-Webfunktionen. Jedes Mal ein Benutzer eine Webseite, um neue Daten oder die Seite implementiert Ajax lange des Abrufs zum Abrufen von neuer Daten finden Sie unter aktualisiert, können Sie SignalR verwenden.

SignalR unterstützt **Serverpush** oder **Broadcasting** Funktionalität; verbindungsverwaltung automatisch verarbeitet. Im klassischen HTTP-Verbindungen für die Kommunikation zwischen Client und Server-Verbindung ist für jede Anforderung wieder hergestellt, SignalR bietet jedoch dauerhafte Verbindung zwischen dem Client und Server. In SignalR beziehen, die der Servercode eine Client-Code im Browser mit Remote Procedure Calls (RPC) aufruft, wissen Sie statt der Anforderungs-/ antwortmodells heute.

In dieser Übung konfigurieren Sie die **Meister Quiz** Anwendung SignalR verwenden Sie das Dashboard Statistiken mit der aktualisierten Metriken ohne die Notwendigkeit, aktualisieren Sie die gesamte Seite anzeigen.

<a id="Ex1Task1"></a>
#### <a name="task-1--exploring-the-geek-quiz-statistics-page"></a>Aufgabe 1: Untersuchen der Seite "Meister Quiz Statistiken"

In dieser Aufgabe wird Sie die Anwendung durchlaufen und überprüfen Sie, wie die Seite Statistiken angezeigt wird, und wie Sie die Art der Informationen verbessern können werden aktualisiert.

1. Öffnen Sie **Visual Studio Express 2013 für Web** , und öffnen Sie die **GeekQuiz.sln** Projektmappe befindet sich in der **Source\Ex1 WorkingWithRealTimeData\Begin** Ordner.
2. Drücken Sie **F5** um die Projektmappe auszuführen. Die **melden Sie sich** Seite sollte im Browser angezeigt werden.

    ![Ausführen der Projektmappe](real-time-web-applications-with-signalr/_static/image2.png "Ausführen der Projektmappe")

    *Ausführen der Projektmappe*
3. Klicken Sie auf **registrieren** in der oberen rechten Ecke Rand der Seite zum Erstellen eines neuen Benutzers in der Anwendung.

    ![Registrieren von Link](real-time-web-applications-with-signalr/_static/image3.png "Register Link")

    *Link zu registrieren*
4. In der **registrieren** geben eine **Benutzername** und **Kennwort**, und klicken Sie dann auf **registrieren**.

    ![Registrieren eines Benutzers](real-time-web-applications-with-signalr/_static/image4.png "Registrieren eines Benutzers")

    *Registrieren eines Benutzers*
5. Die Anwendung registriert das neue Konto und der Benutzer authentifiziert und zurück an die Startseite zeigt die erste Quiz Frage umgeleitet wird.
6. Öffnen der **Statistiken** Seite in einem neuen Fenster, und fügen Sie der **Home** Seite und **Statistiken** Seite von Seite-an-Seite.

    ![Seite-an-Seite Windows](real-time-web-applications-with-signalr/_static/image5.png "Seite von Seite Windows")

    *Windows-Seite-an-Seite*
7. In der **Home** Seite, indem Sie eine der Optionen auf die Frage beantwortet wird.

    ![Um eine Frage zu beantworten](real-time-web-applications-with-signalr/_static/image6.png "um eine Frage beantworten.")

    *Um eine Frage beantworten.*
8. Nach dem Klicken auf eine der Schaltflächen klicken, sollte der Antwort angezeigt werden.

    ![Frage richtig beantwortet](real-time-web-applications-with-signalr/_static/image7.png "Frage richtig beantwortet")

    *Frage richtig beantwortet*
9. Beachten Sie, dass die Informationen in die Seite Statistiken veraltet sind. Aktualisieren Sie die Seite, um die aktualisierten Ergebnisse anzuzeigen.

    ![Seite "STATISTICS"](real-time-web-applications-with-signalr/_static/image8.png "Seite Statistiken")

    *Statistiken-Seite*
10. Wechseln Sie zurück zu Visual Studio, und beenden Sie des Debuggens.

<a id="Ex1Task2"></a>
#### <a name="task-2--adding-signalr-to-geek-quiz-to-show-online-charts"></a>Aufgabe 2 – Hinzufügen von SignalR zu Meister Quiz Online Diagramme anzeigen

In dieser Aufgabe erstellen Sie SignalR zur Projektmappe hinzufügen und Senden von Aktualisierungen an die Clients automatisch, wenn eine neue Antwort an den Server gesendet wird.

1. Aus der **Tools** in Visual Studio, wählen Sie im Menü **Bibliothekspaket-Manager**, und klicken Sie dann auf **Package Manager Console**.
2. In der **Package Manager Console** Fenster, führen Sie den folgenden Befehl:

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample1.ps1)]

    ![SignalR-Paketinstallation](real-time-web-applications-with-signalr/_static/image9.png "SignalR-Paketinstallation")

    *SignalR-Paketinstallation*

   > [!NOTE]
   > Bei der Installation von **SignalR** NuGet-Pakete Version 2.0.2 aus einer neuen MVC 5-Anwendung, müssen manuell aktualisieren **OWIN** -Paketen zur Version 2.0.1 (oder höher) vor der Installation von SignalR. Zu diesem Zweck können Sie das folgende Skript im Ausführen der **Package Manager Console**:
   > 
   > [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample2.ps1)]
   > 
   > In einer zukünftigen Version von SignalR wird die OWIN-Abhängigkeiten werden automatisch aktualisiert.
3. In **Projektmappen-Explorer**, erweitern Sie die **Skripts** Ordner, und beachten Sie, die SignalR *Js* Dateien der Projektmappe hinzugefügt wurden.

    ![SignalR JavaScript verweist auf](real-time-web-applications-with-signalr/_static/image10.png "SignalR JavaScript verweist.")

    *SignalR JavaScript-Verweise*
4. In **Projektmappen-Explorer**, mit der rechten Maustaste die **GeekQuiz** -Projekt, wählen **hinzufügen** | **neuer Ordner**, und nennen Sie sie  **Hubs**.
5. Mit der rechten Maustaste die **Hubs** Ordner, und wählen **hinzufügen | Neues Element**.

    ![Neues Element hinzufügen](real-time-web-applications-with-signalr/_static/image11.png "neues Element hinzufügen")

    *Neues Element hinzufügen*
6. In der **neues Element hinzufügen** wählen Sie im Dialogfeld die **Visual C# | Web | SignalR** Knoten im linken Bereich auf **SignalR-Hub-Klasse (v2)** aus der Mitte, nennen Sie die Datei **StatisticsHub.cs** , und klicken Sie auf **hinzufügen**.

    ![Im Dialogfeld für neues Element hinzufügen](real-time-web-applications-with-signalr/_static/image12.png "im Dialogfeld für neues Element hinzufügen")

    *Fügen Sie im Dialogfeld Neues Element hinzu.*
7. Ersetzen Sie den Code in der **StatisticsHub** Klasse durch den folgenden Code.

    (Codeausschnitt - *RealTimeSignalR - Ex1 - StatisticsHubClass*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample3.cs)]
8. Open **Startup.cs** und fügen Sie die folgende Zeile am Ende der **Konfiguration** Methode.

    (Codeausschnitt - *RealTimeSignalR - Ex1 - MapSignalR*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample4.cs)]
9. Öffnen der **StatisticsService.cs** Seite innerhalb der **Services** Ordner, und fügen Sie die folgenden using-Direktiven.

    (Codeausschnitt - *RealTimeSignalR - Ex1 - UsingDirectives*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample5.cs)]
10. Damit verbundenen Clients Updates benachrichtigt werden, rufen Sie zuerst eine **Kontext** Objekt für die aktuelle Verbindung. Die **Hub** -Objekt enthält Methoden zum Senden von Nachrichten an einen einzelnen Client oder broadcast an alle verbundenen Clients. Fügen Sie die folgende Methode, die **StatisticsService** Klasse, um die statistische Daten zu übertragen.

    (Codeausschnitt - *RealTimeSignalR - Ex1 - NotifyUpdatesMethod*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample6.cs)]

    > [!NOTE]
    > Im obigen Code, verwenden Sie eine beliebige Methodennamen Aufrufen eine Funktion auf dem Client (d. h.: *UpdateStatistics*). Der Methodenname, die Sie angeben, wird als ein dynamisches Objekt interpretiert dies bedeutet, dass es gibt keine IntelliSense oder Überprüfung der Kompilierzeit dafür. Der Ausdruck wird zur Laufzeit ausgewertet. Wenn der Methodenaufruf ausgeführt wird, sendet SignalR dem Methodennamen und die Parameterwerte an den Client. Wenn der Client eine Methode, die mit dem Namen übereinstimmt verfügt, wird diese Methode aufgerufen und die Parameterwerte sind übergeben wird. Wenn keine übereinstimmende Methode auf dem Client gefunden wird, wird kein Fehler ausgelöst. Weitere Informationen finden Sie unter [ASP.NET SignalR-Hubs API-Handbuch](../guide-to-the-api/hubs-api-guide-server.md).
11. Öffnen der **TriviaController.cs** Seite innerhalb der **Controller** Ordner, und fügen Sie die folgenden using-Direktiven.

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample7.cs)]
12. Fügen Sie den folgenden hervorgehobenen Code in die **Post** Aktionsmethode.

    (Codeausschnitt - *RealTimeSignalR - Ex1 - NotifyUpdatesCall*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample8.cs)]
13. Öffnen der **Statistics.cshtml** Seite innerhalb der **Ansichten | Startseite** Ordner. Suchen Sie die **Skripts** Abschnitt und fügen Sie die folgenden Skriptverweise am Anfang des Abschnitts.

    (Codeausschnitt - *RealTimeSignalR - Ex1 - SignalRScriptReferences*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample9.cshtml)]

    > [!NOTE]
    > Wenn Sie Visual Studio-Projekts SignalR und andere Skriptbibliotheken hinzugefügt haben, könnten die Paket-Manager eine Version der SignalR-Skriptdatei installieren, die aktueller als die Version, die in diesem Thema dargestellt sind. Stellen Sie sicher, dass im Code wird der Verweis auf das Skript der Version der Skriptbibliothek, die in Ihrem Projekt installiert entspricht.
14. Fügen Sie den folgenden hervorgehobenen Code zum Herstellen einer des Clients auf den SignalR-Hub und aktualisieren Sie die Statistikdaten ein, wenn eine neue Nachricht vom Hub empfangen wird.

    (Codeausschnitt - *RealTimeSignalR - Ex1 - SignalRClientCode*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample10.cshtml)]

    In diesem Code wird eine Hub-Proxy erstellen und registrieren einen Ereignishandler zum Abhören von Nachrichten, die vom Server gesendet werden. In diesem Fall Sie Lauschen für nachrichtenen, die über die *UpdateStatistics* Methode.

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>Aufgabe 3: Ausführen der Projektmappe

In dieser Aufgabe führen Sie die Projektmappe aus, um sicherzustellen, dass die Statistiken Ansicht automatisch mit SignalR aktualisiert wird nach, um eine neue Frage zu beantworten.

1. Drücken Sie **F5** um die Projektmappe auszuführen.

    > [!NOTE]
    > Wenn nicht bereits an die Anwendung angemeldet, melden Sie sich mit dem Benutzer, den Sie in Aufgabe 1 erstellt haben.
2. Öffnen der **Statistiken** Seite in einem neuen Fenster, und fügen Sie der **Home** Seite und **Statistiken** Seite von Seite-an-Seite, wie Sie in Aufgabe 1.
3. In der **Home** Seite, indem Sie eine der Optionen auf die Frage beantwortet wird.

    ![Um eine andere Frage zu beantworten](real-time-web-applications-with-signalr/_static/image13.png "um eine andere Frage beantworten.")

    *Um eine andere Frage beantworten.*
4. Nach dem Klicken auf eine der Schaltflächen klicken, sollte der Antwort angezeigt werden. Beachten Sie, dass die Statistikinformationen auf der Seite automatisch aktualisiert wird, nach der Beantwortung der Frage mit den aktualisierten Informationen, ohne dass die gesamte Seite aktualisieren müssen.

    ![Seite Statistiken aktualisiert, nachdem die Antwort](real-time-web-applications-with-signalr/_static/image14.png "Seite Statistiken aktualisiert, nachdem die Antwort")

    *Seite "STATISTICS" nach einer Antwort aktualisiert*

<a id="Exercise2"></a>
### <a name="exercise-2-scaling-out-using-sql-server"></a>Übung 2: Dezentrales Skalieren Verwenden von SQLServer

Wenn eine Webanwendung zu skalieren, können Sie in der Regel auswählen zwischen *hochskalieren* und *das horizontale Skalieren* Optionen. *Vertikales Skalieren* bedeutet mehr Ressourcen (CPU, Arbeitsspeicher usw.) während der einen größeren Server mit *skalieren* bedeutet, dass das Hinzufügen weiterer Server zur Bewältigung der Arbeitslast. Das Problem mit dem der zweite Wert ist, dass die Clients auf verschiedenen Servern weitergeleitet werden können. Ein Client, der mit einem Server verbunden ist wird von einem anderen Server gesendete Nachrichten nicht empfangen.

Sie können diese Probleme beheben, indem Sie mithilfe einer Komponente namens *Rückwandplatine*, Nachrichten zwischen Servern weitergeleitet. Mit einer Rückwandplatine aktiviert ist jede Anwendungsinstanz sendet Nachrichten an die Backplane und der Rückwand Weiterleitung an den anderen Anwendungsinstanzen.

Es gibt derzeit drei Typen von Backplanes für SignalR zur Verfügung:

- **Windows Azure Service Bus**. Service Bus ist eine messaging-Infrastruktur, die Komponenten zum Senden von lose verbundenen Nachrichten ermöglicht.
- **SQL Server**. Die SQL Server-Rückwandplatine schreibt Nachrichten in SQL-Tabellen. Der Rückwand verwendet Service Broker für effiziente messaging. Sie funktioniert aber auch, wenn Service Broker nicht aktiviert ist.
- **Redis**. Redis ist ein in-Memory-Schlüssel / Wert-Speicher. Redis unterstützt ein ("Pub/Sub") veröffentlichen/abonnieren-Muster für das Senden von Nachrichten.

Jede Nachricht wird über einen Nachrichtenbus gesendet. Implementiert ein Nachrichtenbus der [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) -Schnittstelle, die eine Abstraktion zum Veröffentlichen/Abonnieren bereitstellt. Die Backplanes arbeiten möchten, indem Sie durch das Ersetzen der standardmäßigen **IMessageBus** mit einem Bus für diese Rückwandplatine konzipiert.

Jede Instanz eines Servers eine Verbindung mit der Rückwand über den Bus. Wenn eine Nachricht gesendet wird, geht Sie an der Rückwand, und der Rückwand sendet sie an jedem Server. Wenn ein Server eine Nachricht von der Backplane empfängt, speichert er die Nachricht im lokalen Cache. Der Server übermittelt dann Nachrichten für Clients aus dem lokalen Cache.

Weitere Informationen zur Funktionsweise der SignalR-Rückwandplatine diesem [Artikel](../performance/scaleout-in-signalr.md).

> [!NOTE]
> Es gibt einige Szenarios, in denen eine Rückwandplatine zu einem Engpass werden kann. Hier sind einige typische SignalR-Szenarios:
> 
> - [Server-Broadcast](tutorial-server-broadcast-with-signalr.md) (z. B. Börsenticker): Backplanes eignen sich gut für dieses Szenario, da der Server die Rate kontrolliert, an dem Nachrichten gesendet werden.
> - [Client-zu-Client](tutorial-getting-started-with-signalr.md) (z. B. chat): In diesem Szenario kann der Rückwand einen Engpass sein, wenn die Anzahl der Nachrichten mit der Anzahl der Clients skaliert; d. h., wenn die Anzahl der Nachrichten vergrößert wird als proportional mehr Clients verknüpfen.
> - [Hochfrequente Echtzeit](tutorial-high-frequency-realtime-with-signalr.md) (z. B. Echtzeit Spiele): Rückwandplatine für dieses Szenario nicht empfohlen wird.


In dieser Übung verwenden Sie **SQL Server** zum Verteilen von Nachrichten über die **Meister Quiz** Anwendung. Führen Sie diese Aufgaben auf einen einzelnen Testcomputer, um zu erfahren, wie die Konfiguration einzurichten, sondern um die vollständigen Auswirkungen zu erhalten, müssen Sie die SignalR-Anwendung auf zwei oder mehr Servern bereitzustellen. Sie müssen SQL Server auch auf einem Server oder auf einem separaten dedizierten Server installieren.

![Dezentrales Skalieren mit SQL Server-Diagramm](real-time-web-applications-with-signalr/_static/image15.png)

<a id="Ex2Task1"></a>
#### <a name="task-1---understanding-the-scenario"></a>Aufgabe 1 – Grundlegendes zum Szenario

In dieser Aufgabe führen Sie 2 Instanzen **Meister Quiz** simulieren mehrere IIS-Instanzen auf dem lokalen Computer. In diesem Szenario, bei der Beantwortung von Fragen faszinierende zu einer Anwendung werden nicht auf die Seite für die Statistik der zweiten Instanz Update benachrichtigt. Diese Simulation ähnelt eine Umgebung, in dem die Anwendung auf mehreren Instanzen bereitgestellt wurde, und mit ihnen kommunizieren mit einem Lastenausgleichsmodul.

1. Öffnen der **Begin.sln** Projektmappe befindet sich in der **Quell-/Ex2-ScalingOutWithSQLServer/Anfang** Ordner. Nach dem Laden, werden Sie feststellen, auf die **Server-Explorer** , dass die Lösung, zwei Projekte mit identischer bietet Strukturen aber unterschiedliche Namen. Dadurch wird simuliert, zwei Instanzen der gleichen Anwendung auf dem lokalen Computer ausgeführt wird.

    ![Lösung 2 Instanzen Meister Quiz simulieren beginnen](real-time-web-applications-with-signalr/_static/image16.png "Lösung simulieren 2 Instanzen Meister Quiz beginnen")

    *Beginnen Sie Lösung 2 Instanzen Meister Quiz simulieren*
2. Öffnen Sie die Eigenschaftenseite der Projektmappe, indem Sie mit der rechten Maustaste des Knotens Projektmappe, und wählen **Eigenschaften**. Unter **Startprojekt**Option **mehrere Startprojekte** , und ändern Sie die **Aktion** Wert für beide Projekte auf *starten*.

    ![Mehrere Projekte starten](real-time-web-applications-with-signalr/_static/image17.png "mehrere Projekte starten")

    *Mehrere Projekte starten*
3. Drücken Sie **F5** um die Projektmappe auszuführen. Starten der Anwendung zwei Instanzen von **Meister Quiz** simulieren Sie mehrere Instanzen derselben Anwendung in unterschiedliche Ports. Heften Sie einen Browser auf der linken Seite und die andere auf der rechten Seite des Bildschirms. Melden Sie sich mit Ihren Anmeldeinformationen oder registrieren Sie einen neuen Benutzer. Wenn Sie angemeldet sind, behalten Sie auf der linken Seite der Seite "faszinierende", und fahren Sie mit der **Statistiken** Seite im Browser auf der rechten Seite.

    ![Meister Quiz nebeneinander angezeigt werden](real-time-web-applications-with-signalr/_static/image18.png)

    *Meister Quiz nebeneinander angezeigt werden*

    ![Meister Quiz in unterschiedliche Ports](real-time-web-applications-with-signalr/_static/image19.png)

    *Meister Quiz in unterschiedliche Ports*
4. Beantwortung von Fragen in den linken Browser neu starten, und Sie werden feststellen, dass die **Statistiken** Seite in der richtigen Browser wird nicht aktualisiert. Grund hierfür ist, **SignalR** verwendet eine lokalen cache auf um Nachrichten auf ihren Clients zu verteilen und mehrere Instanzen dieses Szenario simuliert, daher wird der Cache nicht gemeinsam genutzt werden. Sie können sicherstellen, dass **SignalR** testen die gleichen Schritte jedoch mithilfe einer einzigen Apps funktioniert. Konfigurieren Sie in den folgenden Aufgaben Rückwandplatine zum Replizieren von Nachrichten über Instanzen.
5. Wechseln Sie zurück zu Visual Studio, und beenden Sie des Debuggens.

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-sql-server-backplane"></a>Aufgabe 2: Erstellen der SQL Server-Rückwandplatine

In dieser Aufgabe erstellen Sie eine Datenbank, die als eine Rückwandplatine für dient der **Meister Quiz** Anwendung. Verwenden Sie **Objekt-Explorer von SQL Server** auf Ihrem Server durchsuchen und initialisieren Sie die Datenbank. Aktivieren Sie darüber hinaus die **Service Broker**.

1. In **Visual Studio**, öffnen Sie im Menü **Ansicht** , und wählen Sie **Objekt-Explorer von SQL Server**.
2. Verbinden mit der LocalDB-Instanz, indem Sie mit der rechten Maustaste die **SQL Server** Knoten klicken und auswählen **SQL-Server hinzufügen...**  Option.

    ![Hinzufügen einer SQL Server-Instanz](real-time-web-applications-with-signalr/_static/image20.png "eine SQL Server-Instanz hinzufügen")

    *Hinzufügen einer SQL Server-Instanz auf SQL Server-Objekt-Explorer*
3. Legen Sie die **Servernamen** auf *(Localdb) \v11.0* und lassen Sie **Windows-Authentifizierung** als Authentifizierungsmodus. Klicken Sie auf **verbinden** um den Vorgang fortzusetzen.

    ![Herstellen einer Verbindung mit LocalDB](real-time-web-applications-with-signalr/_static/image21.png "Herstellen einer Verbindung mit LocalDB")

    *Herstellen einer Verbindung mit LocalDB*
4. Nun, dass Sie mit der LocalDB-Instanz verbunden sind, müssen Sie eine Datenbank zu erstellen, die die SQL Server-Rückwandplatine für SignalR darstellen. Zu diesem Zweck Maustaste die **Datenbanken** Knoten, und wählen **Hinzufügen von neuen Datenbanken**.

    ![Hinzufügen einer neuen Datenbank](real-time-web-applications-with-signalr/_static/image22.png "Hinzufügen einer neuen Datenbank")

    *Hinzufügen einer neuen Datenbank*
5. Legen Sie den Datenbanknamen auf *SignalR* , und klicken Sie auf **OK** , ihn zu erstellen.

    ![Erstellen der Datenbank SignalR](real-time-web-applications-with-signalr/_static/image23.png "die SignalR-Datenbank erstellen")

    *Erstellen der SignalR-Datenbank*

    > [!NOTE]
    > Sie können einen beliebigen Namen für die Datenbank auswählen.
6. Um Updates von der Backplane effizienter zu erhalten, wird empfohlen, Service Broker für die Datenbank zu aktivieren. Service Broker bietet systemeigene Unterstützung für Messaging- und Warteschlangen in SQL Server. Der Rückwand funktioniert auch ohne Service Broker. Öffnen Sie eine neue Abfrage, indem Sie mit der rechten Maustaste die Datenbank, und wählen **neue Abfrage**.

    ![Öffnen eine neue Abfrage](real-time-web-applications-with-signalr/_static/image24.png "Sie eine neue Abfrage öffnen")

    *Öffnen eine neue Abfrage*
7. Um zu überprüfen, ob Service Broker aktiviert ist, Fragen Sie die **ist\_Broker\_aktiviert** Spalte in der **sys.databases** -Katalogsicht angezeigt. Führen Sie das folgende Skript in der zuletzt geöffneten Abfragefenster.

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample11.sql)]

    ![Der Status von Service Broker Abfragen](real-time-web-applications-with-signalr/_static/image25.png "Abfragen des Service Broker-Status")

    *Abfragen von den Service Broker-Status*
8. Wenn der Wert der **ist\_Broker\_aktiviert** Spalte in der Datenbank ist &quot;0&quot;, verwenden Sie den folgenden Befehl, um ihn zu aktivieren. Ersetzen Sie **&lt;Ihre Datenbank&gt;** mit dem Namen, die Sie beim Erstellen der Datenbank festlegen (z. B.: SignalR).

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample12.sql)]

    ![Aktivieren von Service Broker](real-time-web-applications-with-signalr/_static/image26.png "Aktivieren von Service Broker")

    *Aktivieren von Service Broker*

    > [!NOTE]
    > Wenn diese Abfrage angezeigt wird, ein Deadlock auftreten, stellen Sie sicher, dass keine Programme, die mit der Datenbank verbunden sind.

<a id="Ex2Task3"></a>
#### <a name="task-3--configuring-the-signalr-application"></a>Aufgabe 3: Konfigurieren der SignalR-Anwendung

In dieser Aufgabe Konfigurieren Sie **Meister Quiz** zur Verbindung mit der SQL Server-Rückwandplatine. Fügen Sie zuerst die **SignalR.SqlServer** NuGet-Paket, und legen Sie die Verbindung mit Ihrer Datenbank Rückwandplatine Zeichenfolge.

1. Öffnen der **Package Manager Console** aus **Tools** | **Bibliothekspaket-Manager**. Stellen Sie sicher, dass **GeekQuiz** Projekt ausgewählt ist, der **Standardprojekt** Dropdown-Liste. Geben Sie den folgenden Befehl zum Installieren der **Microsoft.AspNet.SignalR.SqlServer** NuGet-Paket.

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample13.ps1)]
2. Wiederholen Sie dieses Mal jedoch den vorherigen Schritt für Projekt **GeekQuiz2**.
3. Öffnen Sie zum Konfigurieren der SQL Server-Rückwandplatine der **Startup.cs** Datei von der **GeekQuiz** Projekt, und fügen Sie den folgenden Code hinzu der **konfigurieren** Methode. Ersetzen Sie **&lt;Ihre Datenbank&gt;** durch Ihren Datenbanknamen, die Sie beim Erstellen der SQL Server-Rückwandplatine verwendet. Wiederholen Sie diesen Schritt für die **GeekQuiz2** Projekt.

    (Codeausschnitt - *RealTimeSignalR - Ex2 - StartupConfiguration*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample14.cs)]
4. Nun, dass beide Projekte für die Verwendung der SQL Server-Rückwandplatine konfiguriert sind, drücken Sie **F5** gleichzeitig ausführen.
5. Erneut **Visual Studio** startet zwei Instanzen von **Meister Quiz** in unterschiedliche Ports. Heften Sie einen Browser auf der linken Seite und die andere auf der rechten Seite des Bildschirms, und melden Sie sich mit Ihren Anmeldeinformationen. Behalten Sie auf der linken Seite der Seite "faszinierende", und wechseln Sie zu **Statistiken** eingehende Seite rechts Browser.
6. Starten Sie die Beantwortung von Fragen in den linken Browser. Dieses Mal die **Statistiken** Seite Dank der Rückwand aktualisiert wird. Wechseln zwischen Anwendungen (**Statistiken** befindet sich jetzt auf der linken Seite und **faszinierende** wird auf der rechten Seite), und wiederholen Sie den Test, um zu überprüfen, dass für beide Instanzen ausgeführt wird. Der Rückwand dient als ein *shared Cache* von Nachrichten für jede verbundenen Servers und auf jedem Server werden die Nachrichten in ihre eigenen lokalen Cache zur Verteilung an verbundene Clients gespeichert.
7. Wechseln Sie zurück zu Visual Studio, und beenden Sie des Debuggens.
8. Die SQL Server-Rückwandplatine-Komponente generiert automatisch die erforderlichen Tabellen für die angegebene Datenbank. In der **Objekt-Explorer von SQL Server** Bereich, öffnen Sie die Datenbank, die Sie für die Backplane erstellt haben (z. B.: SignalR), und erweitern Sie die Tabellen. In den folgenden Tabellen sollte angezeigt werden:

    ![Rückwand generierten Tabellen](real-time-web-applications-with-signalr/_static/image27.png)

    *Rückwand generierten Tabellen*
9. Mit der rechten Maustaste die **SignalR.Messages\_0** Tabelle, und wählen Sie **Ansichtsdaten**.

    ![Tabelle für die SignalR-Rückwandplatine Nachrichten](real-time-web-applications-with-signalr/_static/image28.png)

    *Tabelle für die SignalR-Rückwandplatine Nachrichten*
10. Sehen Sie die verschiedenen Nachrichten gesendet, um die **Hub** beim Beantworten der Fragen faszinierende. Der Rückwand verteilt diese Nachrichten in einer beliebigen Instanz verbunden.

    ![Rückwand Nachrichtentabelle](real-time-web-applications-with-signalr/_static/image29.png)

    *Rückwand Nachrichtentabelle*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

In dieser praktischen Übungseinheit haben Sie gelernt, wie hinzuzufügende **SignalR** an Ihre Anwendung und sendet Benachrichtigungen vom Server an die verbundenen Clients mit **Hubs**. Darüber hinaus haben Sie gelernt, skalieren Sie Ihre Anwendung mit einem *Rückwandplatine* Komponente auf, wenn die Anwendung in mehreren Instanzen von IIS bereitgestellt wurde.
