---
uid: signalr/overview/older-versions/scaleout-with-sql-server
title: SignalR mit horizontaler Skalierung mit SQLServer (SignalR 1.x) | Microsoft Docs
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2013
ms.topic: article
ms.assetid: 1dca7967-8296-444a-9533-837eb284e78c
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 7a589f5c6a5196444c3c616b39f501f3a7512471
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="signalr-scaleout-with-sql-server-signalr-1x"></a>SignalR mit horizontaler Skalierung mit SQLServer (SignalR 1.x)
====================
durch [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

In diesem Lernprogramm verwenden Sie SQL Server zum Verteilen von Nachrichten auf einer SignalR-Anwendung, die in zwei separaten IIS-Instanzen bereitgestellt wird. Sie können auch in diesem Lernprogramm ausführen, auf einen einzelnen Testcomputer, aber um vollständige Transparenzeffekt zu erzielen, müssen Sie die SignalR-Anwendung auf zwei oder mehr Servern bereitzustellen. Sie müssen SQL Server auch auf einem Server oder auf einem separaten dedizierten Server installieren. Eine andere Möglichkeit ist das Lernprogramm mithilfe von virtuellen Computern in Azure ausgeführt.

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a>Erforderliche Komponenten

Microsoft SQLServer 2005 oder höher. Der Rückwand unterstützt Desktop- und Server-Editionen von SQL Server. Er unterstützt keine SQL Server Compact Edition oder Azure SQL-Datenbank. (Wenn die Anwendung in Azure gehostet wird, empfiehlt es sich die Service Bus-Rückwandplatine stattdessen.)

## <a name="overview"></a>Übersicht

Bevor wir auf die ausführliches Lernprogramm erhalten, ist hier ein schnellen Überblick darüber, was Sie tun können.

1. Erstellen Sie eine neue leere Datenbank ein. Der Rückwand erstellt die erforderlichen Tabellen in dieser Datenbank.
2. Ihre Anwendung diese NuGet-Pakete hinzugefügt werden: 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.SqlServer](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. Erstellen einer SignalR-Anwendung.
4. Fügen Sie den folgenden Code, um "Global.asax" So konfigurieren Sie die Backplane: 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

## <a name="configure-the-database"></a>Konfigurieren Sie die Datenbank

Entscheiden Sie, ob die Anwendung Windows-Authentifizierung oder SQL Server-Authentifizierung für den Datenbankzugriff verwendet werden. Unabhängig davon, stellen Sie sicher, dass der Datenbankbenutzer hat Berechtigungen zum Anmelden, Erstellen von Schemas und Tabellen erstellen.

Erstellen Sie eine neue Datenbank für die Backplane verwenden. Sie können der Datenbank auf einen beliebigen Namen geben. Sie müssen keine Tabellen in der Datenbank zu erstellen. der Rückwand erstellt die erforderlichen Tabellen.

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a>Aktivieren Sie Service Broker

Es wird empfohlen, zum Aktivieren von Service Broker für die Datenbank Rückwandplatine. Service Broker bietet systemeigene Unterstützung für Messaging- und Warteschlangen in SQL Server, letztere der Rückwand Updates noch effizienter zu erhalten. (Allerdings funktioniert der Rückwand auch ohne Service Broker.)

Um zu überprüfen, ob Service Broker aktiviert ist, Fragen Sie die **ist\_Broker\_aktiviert** Spalte in der **sys.databases** -Katalogsicht angezeigt.

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

Um Service Broker zu aktivieren, verwenden Sie die folgende SQL-Abfrage:

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> Wenn diese Abfrage angezeigt wird, ein Deadlock auftreten, stellen Sie sicher, dass keine Programme, die mit der Datenbank verbunden sind.


Wenn Sie die Ablaufverfolgung aktiviert haben, die ablaufverfolgungen ebenfalls angezeigt, ob Service Broker aktiviert ist.

## <a name="create-a-signalr-application"></a>Erstellen einer SignalR-Anwendung

Erstellen einer SignalR-Anwendung entweder mit diesen Lernprogrammen:

- [Erste Schritte mit SignalR](../getting-started/tutorial-getting-started-with-signalr.md)
- [Erste Schritte mit SignalR und MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md)

Ändern Sie als Nächstes die chatanwendung zur Unterstützung von Warteschlangen für horizontale Skalierung mit SQL Server. Fügen Sie zuerst das SignalR.SqlServer NuGet-Paket zum Projekt hinzu. In Visual Studio aus der **Tools** klicken Sie im Menü **Bibliothekspaket-Manager**, und wählen Sie dann **Package Manager Console**. Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

Als Nächstes öffnen Sie die Datei "Global.asax". Fügen Sie folgenden Code, der **Anwendung\_starten** Methode:

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a>Bereitstellen und Ausführen der Anwendung

Vorbereiten der Windows Server-Instanzen, die SignalR-Anwendung bereitstellen.

Fügen Sie die IIS-Rolle hinzu. Schließen Sie "Anwendungsentwicklung"-Funktionen, einschließlich des WebSocket-Protokolls.

![](scaleout-with-sql-server/_static/image4.png)

Außerdem enthalten Sie den Verwaltungsdienst (unter "Verwaltungstools" aufgeführt).

![](scaleout-with-sql-server/_static/image5.png)

**Installieren Sie Web Deploy 3.0.** Wenn Sie die IIS-Manager ausführen, werden Sie zum Installieren von Microsoft-Webplattform aufgefordert, oder Sie können [Herunterladen der Intstaller](https://go.microsoft.com/fwlink/?LinkId=255386). Klicken Sie in der Webplattform-Installer Web Deploy suchen Sie, und installieren Sie Web Deploy 3.0

![](scaleout-with-sql-server/_static/image6.png)

Überprüfen Sie, dass die Web-Management-Dienst ausgeführt wird. Wenn dies nicht der Fall ist, starten Sie den Dienst. (Wenn Webverwaltungsdienst in der Liste der Windows-Dienste nicht angezeigt wird, stellen Sie sicher, dass Sie den Management-Dienst installiert, wenn Sie die IIS-Rolle hinzugefügt.)

Öffnen Sie zum Schluss Port 8172 für TCP. Dies ist der Port, den der Web Deploy-Tool verwendet.

Jetzt sind Sie bereit für die Bereitstellung von Visual Studio-Projekt vom Entwicklungscomputer auf den Server. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in der Projektmappe, und klicken Sie auf **veröffentlichen**.

Ausführlichere Dokumentation zu Web Deploy finden Sie unter [ASP.NET-Bereitstellung für Visual Studio und ASP.NET Web](../../../whitepapers/aspnet-web-deployment-content-map.md).

Wenn Sie die Anwendung mit zwei Servern bereitstellen, können jede Instanz in einem separaten Browserfenster zu öffnen und sehen, dass jeder SignalR-Nachrichten aus den anderen erhält. (Selbstverständlich würden in einer produktionsumgebung die beiden Servern hinter einem Lastenausgleich sit.)

![](scaleout-with-sql-server/_static/image7.png)

Nachdem Sie die Anwendung ausführen, sehen Sie sich, dass SignalR automatisch Tabellen in der Datenbank erstellt wurde:

![](scaleout-with-sql-server/_static/image8.png)

SignalR verwendet die Tabellen. Solange die Anwendung bereitgestellt wurde, keine löschen Sie Zeilen, ändern Sie die Tabelle, usw.
