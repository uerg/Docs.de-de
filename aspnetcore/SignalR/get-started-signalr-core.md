---
title: "Erste Schritte mit SignalR für ASP.NET Core"
author: rachelappel
description: "Grundlagen der Erstellung einer Echtzeit-app mit SignalR für ASP.NET Core."
manager: wpickett
ms.author: rachelap
ms.custom: mvc
ms.date: 03/06/2018
ms.prod: aspnet-core
ms.technology: dotnet-signalr
ms.topic: tutorial
uid: signalr/get-started-signalr-core
ms.openlocfilehash: 79af59fc8c2ada71d764ada95a431e10f4f00f27
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2018
---
# <a name="tutorial-get-started-with-signalr-for-aspnet-core"></a>Tutorial: Erste Schritte mit SignalR für ASP.NET Core

Von [Rachel Appel](https://twitter.com/rachelappel)

Dieses Lernprogramm zeigt die Grundlagen der Erstellung einer Echtzeit-app mit SignalR für ASP.NET Core.

   ![Lösung](get-started-signalr-core/_static/signalr-get-started-finished.png)

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

Dieses Lernprogramm veranschaulicht die folgenden SignalR Entwicklungsaufgaben:

> [!div class="checklist"]
> * Erstellen Sie eine ASP.NET Core-Web-app.
> * Erstellen eines SignalR-Hubs, um Inhalt an Clients zu verteilen.
> * Verwenden Sie die SignalR JavaScript-Bibliothek zum Senden von Nachrichten und Updates vom Hub angezeigt.

## <a name="prerequisites"></a>Erforderliche Komponenten

Installieren Sie die folgende Software:

* [.NET Core 2.1.0 Vorschau 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) oder höher
* [Visual Studio-2017](https://www.visualstudio.com/downloads/) Version 15.6 oder höher, mit der arbeitsauslastung für ASP.NET und Entwicklung
* [npm](https://www.npmjs.com/get-npm)

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a>Erstellen Sie ein Projekt von ASP.NET Core, die SignalR-Client und Server hostet

1. Verwenden der **Datei** > **neues Projekt** Menü aus, und wählen Sie **ASP.NET-Webanwendung für Core**. Benennen Sie das Projekt mit `SignalRChat`.

  ![Dialogfeld "Neues Projekt" in Visual Studio](get-started-signalr-core/_static/signalr-new-project-dialog.png)

2. Wählen Sie **Webanwendung** zum Erstellen eines Projekts mithilfe von Razor-Seiten. Wählen Sie dann **Ok**. Achten Sie darauf, **ASP.NET Core 2.1** aus dem Framework Selektor ausgewählt ist, obwohl SignalR unter älteren Versionen von .NET ausgeführt wird.

  ![Dialogfeld "Neues Projekt" in Visual Studio](get-started-signalr-core/_static/signalr-new-project-choose-type.png)

  Die Bibliotheken, die SignalR serverseitigen Code hosten, sind in der Projektvorlage enthalten. Installieren Sie die clientseitiges JavaScript separat mit [Npm](https://www.npmjs.com/).

  ```console
   npm install @aspnet/signalr
  ```

3. Kopieren der *signalr.js* aus *Node_modules\\ @aspnet\signalr\dist\browser*  auf die *Wwwroot\lib* Ordner des Projekts.

## <a name="create-the-signalr-hub"></a>Erstellen von SignalR-Hubs.

Ein Hub ist eine Klasse, die als eine allgemeine Pipeline dient, die zum Aufrufen von Methoden voneinander Client und Server ermöglicht.

1. Fügen Sie eine Klasse zum Projekt durch Auswahl **Datei** > **neu** > **Datei** auswählen und **Visual C#-Klasse**. 

1. Erben von `Microsoft.AspNetCore.SignalR.Hub`. Die `Hub` Klasse enthält Eigenschaften und Ereignisse für die Verwaltung von Verbindungen und Gruppen sowie senden und Empfangen von Daten.

1. Erstellen der `Send` -Methode, die eine Nachricht an alle verbundenen Chat-Clients sendet. Beachten Sie, es gibt eine `Task`, da SignalR asynchron ist. Asynchronen Code skaliert besser.

  [!code-csharp[Startup](get-started-signalr-core/sample/Hubs/ChatHub.cs?range=7-14)]

## <a name="configure-the-project-to-use-signalr"></a>Konfigurieren Sie das Projekt zur Verwendung von SignalR

Die SignalR-Server muss konfiguriert werden, damit dieser weiß, dass Anforderungen an SignalR übergeben.

1. Ändern Sie zum Konfigurieren einer SignalR-Projekts die `ConfigureServices` Methode von der Anwendungsverzeichnis `Startup` Klasse durch Einfügen eines Aufrufs in `services.AddSignalR`.

  `services.AddSignalR` Fügt der SignalR als Teil der [ASP.NET Core Middleware](xref:fundamentals/middleware/index) Pipeline.

1. Konfigurieren von Routen zu Ihrer Verwendung Hubs `UseSignalR`.

  [!code-csharp[Startup](get-started-signalr-core/sample/Startup.cs?highlight=22,40-43)]

## <a name="create-the-signalr-client-code"></a>Erstellen Sie den SignalR-Clientcode

1. Ersetzen Sie den Inhalt in *Pages\Index.cshtml* durch den folgenden Code:

  [!code-cshtml[Index](get-started-signalr-core/sample/Pages/Index.cshtml)]

  Die vorangehende HTML zeigt Name und Nachrichtenfelder sowie eine Schaltfläche "Absenden". Beachten Sie die Skriptverweise im unteren Bereich: ein Verweis auf SignalR und *chat.js*.

1. Fügen Sie eine JavaScript-Datei, um die *Wwwroot\js* Ordner mit dem Namen *chat.js* und fügen Sie den folgenden Code hinzu:

  [!code-javascript[Index](get-started-signalr-core/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a>Ausführen der App

1. Wählen Sie **Debuggen** > **Starten ohne Debuggen** starten Sie einen Browser, und laden die Website lokal. Kopieren Sie die URL aus der Adressleiste ein.

1. Öffnen Sie eine andere Browserinstanz (alle Browser), und fügen Sie die URL in der Adressleiste.

1. Wählen Sie entweder Browser, geben Sie einen Namen und eine Nachricht, und klicken Sie auf die **senden** Schaltfläche. Der Name und die Meldung werden sofort auf beiden Seiten angezeigt.

  ![Lösung](get-started-signalr-core/_static/signalr-get-started-finished.png)
