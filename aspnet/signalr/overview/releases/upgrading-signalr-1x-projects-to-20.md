---
uid: signalr/overview/releases/upgrading-signalr-1x-projects-to-20
title: Aktualisieren von SignalR 1.x-Projekten auf Version 2 | Microsoft Docs
author: pfletcher
description: "In diesem Thema wird beschrieben, wie ein vorhandenes SignalR 1.x-Projekt auf SignalR aktualisiert 2.x sowie zum Behandeln von Problemen, die auftreten können, während des Upgradevorgangs..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: adcfef99-9bc5-489d-a91b-9b7c2bc35e04
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/releases/upgrading-signalr-1x-projects-to-20
msc.type: authoredcontent
ms.openlocfilehash: e372275ae5dd4bbf354db2d02e4407f8c513b7a3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="upgrading-signalr-1x-projects-to-version-2"></a>Aktualisieren von SignalR 1.x-Projekten auf Version 2
====================
durch [Patrick Fletcher](https://github.com/pfletcher)

> In diesem Thema wird beschrieben, wie ein vorhandenes SignalR 1.x-Projekt auf SignalR aktualisiert 2.x sowie zum Behandeln von Problemen, die während des Upgrades auftreten können.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR-Version 1 und 2
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


SignalR 2 bietet eine konsistente entwicklungserfahrung über Server-Plattformen mit [OWIN](http://owin.org). Dieser Artikel beschreibt die Schritte, die erforderlich sind, eine SignalR-1.x-Anwendung auf Version 2 aktualisiert.

Während es empfohlen wird, um Anwendungen für SignalR 2 zu aktualisieren, werden SignalR 1.x weiterhin unterstützt.

In diesem Lernprogramm beschreibt, wie eine im Internet gehostete Anwendung mit SignalR-2 zu aktualisieren. Selbst gehostete Anwendungen (diejenigen, die ein Server in einer Konsolenanwendung, Windows-Dienst oder ein anderer Prozess hosten) werden nun unter SignalR-2 unterstützt. Informationen zu den ersten Schritten beim Erstellen einer selbst gehosteten Anwendung mit SignalR-2 finden Sie unter [Lernprogramm: SignalR Selbsthosting](../deployment/tutorial-signalr-self-host.md).

## <a name="contents"></a>Inhalt

Den folgenden Abschnitten werden die Aufgaben im Zusammenhang mit der Aktualisierung von SignalR-Projekte und Behandlung von Problemen, die auftreten können.

- [Beispiel: Das erste Schritte-Lernprogramm auf SignalR 2 aktualisieren](#example)
- [Problembehandlung bei Fehler während des Upgrades](#troubleshooting)

<a id="example"></a>

## <a name="example-upgrading-the-getting-started-tutorial-application-to-signalr-2"></a>Beispiel: Aktualisieren der Getting Started Tutorial Anwendung SignalR 2

In diesem Abschnitt Aktualisieren Sie die Anwendung in der [Version 1.x SignalR das Lernprogramm für erste Schritte](../older-versions/index.md) SignalR 2 verwenden.

1. Nachdem Sie das erste Schritte-Lernprogramm abgeschlossen haben, mit der rechten Maustaste auf das Projekt, und wählen Sie **Eigenschaften**. Überprüfen Sie, ob die **Zielframework** festgelegt ist, um **.NET Framework 4.5.**
2. Öffnen Sie die Paket-Manager-Konsole. Entfernen von SignalR 1.x aus dem Projekt mit dem folgenden Befehl:

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample1.ps1)]
3. Installieren Sie die SignalR-2, mithilfe des folgenden Befehls:

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample2.ps1)]
4. Aktualisieren Sie in der HTML-Seite für SignalR entsprechend der Version des jetzt im Projekt enthaltenen Skripts wird der Verweis auf das Skript ein.

    [!code-html[Main](upgrading-signalr-1x-projects-to-20/samples/sample3.html)]
5. Entfernen Sie den Aufruf MapHubs, in der Globale Anwendungsklasse.

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample4.cs)]
6. Maustaste auf die Projektmappe, und wählen Sie **hinzufügen**, **neues Element...** . Wählen Sie im Dialogfeld **Owin-Startklasse**. Benennen Sie die neue Klasse **Startup.cs**.

    ![](upgrading-signalr-1x-projects-to-20/_static/image1.png)
7. Ersetzen Sie den Inhalt der tritt durch den folgenden Code ein:

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample5.cs)]

    Das Assemblyattribut Owins-Startprozess, die ausgeführt wird die Klasse hinzugefügt die `Configuration` Methode, wenn die Owin-gestartet wird. Dies wiederum ruft die `MapSignalR` -Methode, die Routen für alle SignalR-Hubs in der Anwendung erstellt.
8. Führen Sie das Projekt, und kopieren Sie die URL des der Hauptseite in einen anderen Browser oder Browserbereich als vor. Jeder Seite wird ein Benutzername angefordert, und Nachrichten für jede Seite in beide Bereiche Browser angezeigt werden.

<a id="troubleshooting"></a>

## <a name="troubleshooting-errors-encountered-during-upgrading"></a>Problembehandlung bei Fehler während des Upgrades

Dieser Abschnitt beschreibt die Probleme, die während des Upgrades auftreten können. Eine umfangreichere Liste von Fehlern und Problemen, die mit einer SignalR-Anwendung auftreten können, finden Sie unter [SignalR Problembehandlung](../testing-and-debugging/troubleshooting.md).

### <a name="the-call-is-ambiguous-between-the-following-methods-or-properties"></a>"Der Aufruf ist zwischen den folgenden Methoden oder Eigenschaften"

Dieser Fehler tritt auf, wenn ein Verweis auf `Microsoft.AspNet.SignalR.Owin` wird nicht entfernt. Dieses Paket ist veraltet. der Verweis muss entfernt werden, und die 1.x-Version des Pakets SelfHost muss deinstalliert werden.

### <a name="hub-methods-fail-silently"></a>Hubmethoden fehl automatisch.

Überprüfen Sie, ob die Skriptverweise in Ihrem Client bis zum Datum, und dass die `OwinStartup` -Attribut für die Startklasse. die richtige Klasse und den Assemblynamen für das Projekt wurde. Versuchen Sie außerdem, die Hubs-Adresse (/ Signalr/Hubs) in Ihrem Browser öffne; Jeder Fehler, der angezeigt wird bietet weitere Informationen zu können, wo der Fehler ist.
