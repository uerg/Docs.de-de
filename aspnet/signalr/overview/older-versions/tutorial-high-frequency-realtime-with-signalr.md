---
uid: signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
title: Hochfrequente Realtime mit SignalR 1.x | Microsoft Docs
author: pfletcher
description: Dieses Lernprogramm veranschaulicht, wie eine Webanwendung zu erstellen, die ASP.NET SignalR verwendet wird, um hochfrequente Messagingfunktionen bereitzustellen. Hochfrequenz messaging in...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/16/2013
ms.topic: article
ms.assetid: ad2a5da5-2e79-40ea-bc84-028d327f5982
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 0c680a7d8b911b2734647948b683d5ff6e47aec4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
ms.locfileid: "26505839"
---
<a name="high-frequency-realtime-with-signalr-1x"></a>Hochfrequente Realtime mit SignalR 1.x
====================
durch [Patrick Fletcher](https://github.com/pfletcher)

> Dieses Lernprogramm veranschaulicht, wie eine Webanwendung zu erstellen, die ASP.NET SignalR verwendet wird, um hochfrequente Messagingfunktionen bereitzustellen. Hochfrequente messaging bedeutet in diesem Fall Updates, die mit einer festen Rate gesendet werden. Bei dieser Anwendung werden bis zu 10 Nachrichten pro Sekunde.
> 
> Die Anwendung, die in diesem Lernprogramm Sie erstellen zeigt eine Form, die Benutzer gezogen werden können. Die Position der Form in allen anderen verbundenen Browsern werden entsprechend die Position der gezogene Form über zeitgesteuerte Updates aktualisiert werden.
> 
> In diesem Lernprogramm eingeführte Konzepte haben Anwendungen in Echtzeit Spiele und anderen Simulation.
> 
> Kommentare in das Lernprogramm sind Willkommen. Wenn Sie Fragen, die nicht direkt mit dem Lernprogramm verknüpft sind haben, bereitstellen können, die [ASP.NET SignalR-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder [StackOverflow.com](http://stackoverflow.com).


## <a name="overview"></a>Übersicht

Dieses Lernprogramm veranschaulicht, wie eine Anwendung erstellen, der den Zustand eines Objekts mit anderen Browsern in Echtzeit. Die Anwendung, die wir erstellen müssen, wird MoveShape aufgerufen. Der Seite "MoveShape" wird ein HTML-Div-Element angezeigt, die der Benutzer ziehen kann. der Benutzer das DIV-Element zieht, wird seiner neue Position an den Server gesendet werden, die informieren dann anderen verbundenen Clients die Position der Form entsprechend aktualisiert wird.

![Das Anwendungsfenster](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

Die Anwendung, die in diesem Lernprogramm erstellten basiert auf einer Demo von Damian Edwards. Ein Video, enthält diese Demo überwachungsarbeitsbereich [hier](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR).

Im Lernprogramm wird vorgeführt, wie zum Senden von Nachrichten von SignalR aus jedes Ereignis, das ausgelöst wird, wie die Form gezogen wird gestartet. Jedem verbundenen Client wird dann die Position der lokalen Version der Form aktualisiert, sobald eine Nachricht empfangen wird.

Während die Anwendung diese Methode verwenden funktioniert, ist dies nicht empfohlene Programmiermodell, da gäbe es keine obere Grenze für die Anzahl der Nachrichten, Abrufen von gesendet werden, damit die Clients und Server mit Nachrichten überlastet abrufen konnte, und würde die Leistung beeinträchtigen . Die angezeigte Animation auf dem Client wäre auch separaten, wie die Form durch jede Methode sofort verschoben werden würde, statt zu jeder neuen Speicherort verschieben. In späteren Abschnitten des Lernprogramms werden gezeigt, wie eine Zeitgeber-Funktion erstellt, die die maximale Übertragungsrate einschränkt, mit der Nachrichten vom Client oder Server gesendet werden, und wie Sie die Form reibungslos zwischen Standorten zu verschieben. Die endgültige Version der Anwendung, die in diesem Lernprogramm erstellten kann von heruntergeladen werden [Code Gallery](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6).

Dieses Lernprogramm enthält folgende Abschnitte:

- [Erforderliche Komponenten](#prerequisites)
- [Erstellen des Projekts](#createtheproject)
- [Die ASP.NET SignalR und JQuery.UI NuGet-Pakete hinzufügen](#nugetpackages)
- [Die Basis-Anwendung erstellen](#baseapp)
- [Fügen Sie die Client-Schleife hinzu](#clientloop)
- [Fügen Sie die Server-Schleife hinzu](#serverloop)
- [Hinzufügen von Animationen zum Glätten auf dem client](#animation)
- [Weitere Schritte](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Erforderliche Komponenten

Dieses Lernprogramm erfordert Visual Studio 2012 oder Visual Studio 2010. Wenn Visual Studio 2010 verwendet wird, wird das Projekt .NET Framework 4 und .NET Framework 4.5 verwenden.

Wenn Sie Visual Studio 2012 verwenden, wird empfohlen, Sie installieren die [Update für ASP.NET und Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650). Dieses Update enthält neue Features wie Erweiterungen zu veröffentlichen, werden neue Funktionen und neue Vorlagen.

Wenn Sie Visual Studio 2010 haben, stellen sicher, dass [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installiert ist.

<a id="createtheproject"></a>

## <a name="create-the-project"></a>Erstellen eines Projekts

In diesem Abschnitt erstellen wir das Projekt in Visual Studio.

1. Aus der **Datei** klicken Sie im Menü **neues Projekt**.
2. In der **neues Projekt** Dialogfeld erweitern Sie **c#** unter **Vorlagen** , und wählen Sie **Web**.
3. Wählen Sie die **leere ASP.NET-Webanwendung** Vorlage, nennen Sie das Projekt *MoveShapeDemo*, und klicken Sie auf **OK**.

    ![Erstellen des neuen Projekts](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

<a id="nugetpackages"></a>

## <a name="add-the-signalr-and-jqueryui-nuget-packages"></a>Fügen Sie die SignalR und JQuery.UI NuGet-Pakete

Sie können die SignalR-Funktionalität zu einem Projekt hinzufügen, durch die Installation von NuGet-Paket. In diesem Lernprogramm verwenden ebenfalls das JQuery.UI-Paket zugelassen wird, dass die Form gezogen und animiert werden kann.

1. Klicken Sie auf **Tools | Bibliothek-Paket-Manager | Paket-Manager-Konsole**.
2. Geben Sie den folgenden Befehl in der Paket-Manager aus.

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.ps1)]

    Die SignalR-Paket installiert eine Reihe von anderen NuGet-Pakete als Abhängigkeiten. Wenn die Installation abgeschlossen ist, müssen Sie alle Server und Client erforderlichen Komponenten für SignalR in einer ASP.NET-Anwendung zu verwenden.
3. Geben Sie den folgenden Befehl in der Paket-Manager-Konsole zum Installieren der Pakete JQuery und JQuery.UI aus.

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.ps1)]

<a id="baseapp"></a>

## <a name="create-the-base-application"></a>Die Basis-Anwendung erstellen

In diesem Abschnitt erstellen wir eine Browseranwendung, die den Speicherort der Form an den Server, während jede Mauszeigerposition ausgelöste Ereignis sendet. Daten beim Empfang, sendet der Server dann diese Informationen für alle anderen verbundenen Clients. Es wird auf diese Anwendung in späteren Abschnitten erweitern.

1. In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **hinzufügen**, **Klasse...** . Benennen Sie die Klasse **MoveShapeHub** , und klicken Sie auf **hinzufügen**.
2. Ersetzen Sie den Code in der neuen **MoveShapeHub** Klasse durch den folgenden Code.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.cs)]

    Die `MoveShapeHub` Klasse oben ist eine Implementierung eines SignalR-Hubs. Wie in der [erste Schritte mit SignalR](index.md) Lernprogramm Hub verfügt über eine Methode, die die Clients direkt aufruft. In diesem Fall sendet der Client ein Objekt mit der neuen X- und Y-Koordinaten der Form auf dem Server, der dann an alle anderen verbundenen Clients weitergegeben ruft. SignalR wird automatisch dieses Objekts unter Verwendung von JSON serialisiert werden.

    Das Objekt, das an den Client gesendet werden sollen (`ShapeModel`) enthält Elemente, um die Position der Form zu speichern. Die Version des Objekts auf dem Server enthält außerdem einen Member zum Nachverfolgen von der Client Daten gespeichert werden, sodass ein Client gesendet werden, ihre eigenen Daten wird nicht. Dieses Element verwendet die `JsonIgnore` Attribut und verhindert, serialisiert und an den Client gesendet wird.
3. Als Nächstes führen wir den Hub eingerichtet, beim Starten der Anwendung. In **Projektmappen-Explorer**Sie mit der rechten Maustaste des Projekts, und klicken Sie auf **hinzufügen | Globale Anwendungsklasse**. Übernehmen Sie den Standardnamen *Global* , und klicken Sie auf **OK**.

    ![Globale Anwendungsklasse hinzufügen](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
4. Fügen Sie die folgenden `using` Anweisung nach der bereitgestellten **mit** Anweisungen in der Global.asax.cs-Klasse.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.cs)]
5. Fügen Sie die folgende Codezeile in der `Application_Start` Methode der globalen so registrieren Sie die Standardroute für SignalR-Klasse.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    Die Datei "global.asax" sollte wie folgt aussehen:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.cs)]
6. Fügen Sie anschließend den Client. In **Projektmappen-Explorer**Sie mit der rechten Maustaste des Projekts, und klicken Sie auf **hinzufügen | Neues Element**. In der **neues Element hinzufügen** wählen Sie im Dialogfeld **Html-Seite**. Geben Sie die Seite einen passenden Namen (z. B. **"default.HTML"**), und klicken Sie auf **hinzufügen**.
7. In **Projektmappen-Explorer**mit der rechten Maustaste auf die Seite, die Sie gerade erstellt haben, und klicken Sie auf **als Startseite festlegen**.
8. Ersetzen Sie den Standardcode in der HTML-Seite mit dem folgenden Codeausschnitt an.

    > [!NOTE]
    > Stellen Sie sicher, dass das Skript unten Übereinstimmung, die Pakete dem Projekt im Ordner "Skripts" hinzugefügt verweist. In Visual Studio 2010 kann die Version von JQuery und SignalR, die dem Projekt hinzugefügt, die folgenden Versionsnummern nicht überein.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample7.html)]

    Die oben genannten HTML und JavaScript-Code erstellt ein rotes Div Form aufgerufen, können Sie die Form ziehen Verhalten mithilfe der jQuery-Bibliothek und verwendet die Form vom Typ `drag` Ereignis, um die Position der Form an den Server gesendet.
9. Starten Sie die Anwendung durch Drücken von F5. Kopieren Sie die URL der Seite, und fügen Sie ihn in einem zweiten Browserfenster. Ziehen Sie die Form in einem Browserfenster. die Form, in dem anderen Browserfenster sollten verschieben.

    ![Das Anwendungsfenster](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a>Fügen Sie die Client-Schleife hinzu

Da den Speicherort der Form für jede Mauszeigerposition ausgelöste Ereignis senden eine ist ggf. unnötig Umfang des Netzwerkverkehrs erstellen, müssen die Nachrichten vom Client gedrosselt werden. Verwenden wir die Javascript `setInterval` Funktion So richten Sie eine Schleife ein, die Informationen zur neuen Position in einer festen Rate auf dem Server sendet. Diese Schleife ist eine sehr einfache Darstellung der eine "Spiel Schleife", eine wiederholt aufgerufene Funktion, die die Funktionalität eines Spiels oder andere Simulation Laufwerke.

1. Aktualisieren Sie den Clientcode in der HTML-Seite entsprechend den folgenden Codeausschnitt an.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample8.html)]

    Die oben genannten Update fügt die `updateServerModel` -Funktion, die auf einer festen Frequenz aufgerufen wird. Diese Funktion sendet die Positionsdaten, an den Server immer die `moved` Flag gibt an, dass neue die zu sendenden Positionsdaten.
2. Starten Sie die Anwendung durch Drücken von F5. Kopieren Sie die URL der Seite, und fügen Sie ihn in einem zweiten Browserfenster. Ziehen Sie die Form in einem Browserfenster. die Form, in dem anderen Browserfenster sollten verschieben. Die Animation wird nicht so reibungslos wie im vorherigen Abschnitt angezeigt, da die Anzahl der Nachrichten, die an den Server gesendet werden können gedrosselt werden, wird.

    ![Das Anwendungsfenster](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a>Fügen Sie die Server-Schleife hinzu

Wechseln in der aktuellen Anwendung Nachrichten, die vom Server an den Client gesendet so oft, wie sie empfangen werden. Dies stellt ein ähnliches Problem wie auf dem Client dargestellt wurde; Nachrichten können häufiger werden bei Bedarf, und die Verbindung konnte daher überflutet werden gesendet werden. In diesem Abschnitt wird beschrieben, wie beim Aktualisieren des Servers zum Implementieren eines Zeitgebers, das die Rate der ausgehenden Nachrichten schränkt wird.

1. Ersetzen Sie den Inhalt des `MoveShapeHub.cs` mit dem folgenden Codeausschnitt.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample9.cs)]

    Der obige Code erweitert den Client hinzufügen die `Broadcaster` -Klasse, die schränkt den ausgehenden Nachrichten mithilfe der `Timer` Klasse von .NET Framework.

    Seit der Hub selbst um ein vorübergehendes handelt (er ist erstellt jedes Mal, wenn er benötigt wird), die `Broadcaster` wird als Singleton erstellt werden. Verzögerter Initialisierung (eingeführt in .NET 4) wird verwendet, um zu seiner Erstellung zu verzögern, bis diese benötigt wird, um sicherzustellen, dass die erste hubinstanz vollständig erstellt wird, bevor der Timer gestartet wird.

    Der Aufruf an die Clients' `UpdateShape` Funktion wird dann aus des Hubs verschoben `UpdateModel` Methode, sodass die It nicht mehr aufgerufen wird, sofort, wenn eingehende Nachrichten empfangen werden. Stattdessen werden die Nachrichten an die Clients gesendet werden, mit einer Rate von 25 Aufrufe pro Sekunde, von verwaltet die `_broadcastLoop` Timer innerhalb der `Broadcaster` Klasse.

    Schließlich anstelle von Aufrufen der Methode vom Hub direkt die `Broadcaster` Klasse muss einen Verweis auf den aktuell ausgeführten Hub abrufen (`_hubContext`) mithilfe der `GlobalHost`.
2. Starten Sie die Anwendung durch Drücken von F5. Kopieren Sie die URL der Seite, und fügen Sie ihn in einem zweiten Browserfenster. Ziehen Sie die Form in einem Browserfenster. die Form, in dem anderen Browserfenster sollten verschieben. Es werden ein Unterschied im Browser aus dem vorherigen Abschnitt, aber gedrosselt die Anzahl der Nachrichten, die an den Client gesendet werden können.

    ![Das Anwendungsfenster](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a>Hinzufügen von Animationen zum Glätten auf dem client

Die Anwendung ist fast abgeschlossen, aber stellen wir eine weitere Verbesserung, die während des Verschiebens der Form auf dem Client beim Server Nachrichten verschieben. Anstatt das Festlegen der Position der Form an den neuen Speicherort, der vom Server erhält, verwenden wir der JQuery UI-Bibliotheks `animate` Funktion, um die Form problemlos zwischen der aktuellen und der neuen Position verschieben.

1. Aktualisieren Sie den Client `updateShape` Methode gesucht werden soll, wie z. B. den folgenden hervorgehobenen Code:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample10.html?highlight=35-42)]

    Der obige Code verschiebt die Form an der ursprünglichen Stelle in das neue Projekt, das vom Server im Verlauf der Animation Intervall (in diesem Fall 100 Millisekunden) angegeben. Alle vorherige Animation unter der Form "wird gelöscht, bevor die neue Animation gestartet wird.
2. Starten Sie die Anwendung durch Drücken von F5. Kopieren Sie die URL der Seite, und fügen Sie ihn in einem zweiten Browserfenster. Ziehen Sie die Form in einem Browserfenster. die Form, in dem anderen Browserfenster sollten verschieben. Die Verschiebung der Form in das andere Fenster sollte weniger holprig, wie ihre Bewegung über wird einmal pro eingehender Nachricht festgelegt, anstatt Zeit interpoliert wird angezeigt.

    ![Das Anwendungsfenster](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a>Weitere Schritte

In diesem Lernprogramm haben Sie gelernt, die eine SignalR-Anwendung zu programmieren, in denen hochfrequente Nachrichten zwischen Clients und Servern an. Dieses Paradigma Kommunikation eignet sich für die Entwicklung von Onlinespielen und andere Simulationen, z. B. [ShootR Spiel erstellt, die mit SignalR](http://shootr.signalr.net).

Die vollständige Anwendung, die in diesem Lernprogramm erstellten heruntergeladen werden kann, aus [Code Gallery](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6).

Besuchen Sie die folgenden Websites für SignalR-Quellcode und Ressourcen, um weitere Informationen zu SignalR Entwicklung Konzepte:

- [SignalR-Projekt](http://signalr.net)
- [SignalR Github und Beispiele](https://github.com/SignalR/SignalR)
- [SignalR-Wiki](https://github.com/SignalR/SignalR/wiki)
