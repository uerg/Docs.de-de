---
title: Erste Schritte mit SignalR für ASP.NET Core
author: rachelappel
description: In diesem Lernprogramm erstellen Sie eine app mit SignalR für ASP.NET Core.
manager: wpickett
ms.author: rachelap
ms.custom: mvc
ms.date: 03/16/2018
ms.prod: aspnet-core
ms.topic: tutorial
ms.technology: aspnet
uid: signalr/get-started
ms.openlocfilehash: cf120d535c85c7871f5b1f27039018ea2405b9cb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-signalr-on-aspnet-core"></a>Erste Schritte mit SignalR für ASP.NET Core

Von [Rachel Appel](https://twitter.com/rachelappel)

[!INCLUDE [Version notice](../includes/signalr-version-notice.md)]

Dieses Lernprogramm zeigt die Grundlagen der Erstellung einer Echtzeit-app mit SignalR für ASP.NET Core.

   ![Lösung](get-started/_static/signalr-get-started-finished.png)

Dieses Lernprogramm veranschaulicht die folgenden SignalR Entwicklungsaufgaben:

> [!div class="checklist"]
> * Erstellen Sie eine SignalR für ASP.NET Core Web-app.
> * Erstellen eines SignalR-Hubs, um Inhalt an Clients zu verteilen.
> * Ändern der `Startup` -Klasse und die app konfigurieren.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

# <a name="prerequisites"></a>Erforderliche Komponenten

Installieren Sie die folgende Software:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [.NET Core 2.1.0 Vorschau 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) oder höher
* [Visual Studio-2017](https://www.visualstudio.com/downloads/) Version 15.6 oder höher, mit der **ASP.NET und zur Webentwicklung** arbeitsauslastung
* [npm](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [.NET Core 2.1.0 Vorschau 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) oder höher
* [Visual Studio Code](https://code.visualstudio.com/download) 
* [C# für Visual Studio-Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [npm](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a>Erstellen Sie ein Projekt von ASP.NET Core, die SignalR-Client und Server hostet

#### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)
1. Verwenden der **Datei** > **neues Projekt** Menü aus, und wählen Sie **ASP.NET-Webanwendung für Core**. Nennen Sie das Projekt *SignalRChat*.

   ![Dialogfeld "Neues Projekt" in Visual Studio](get-started/_static/signalr-new-project-dialog.png)

2. Wählen Sie **Webanwendung** zum Erstellen eines Projekts mithilfe von Razor-Seiten. Wählen Sie dann **OK**. Achten Sie darauf, **ASP.NET Core 2.1** aus dem Framework Selektor ausgewählt ist, obwohl SignalR unter älteren Versionen von .NET ausgeführt wird.

   ![Dialogfeld "Neues Projekt" in Visual Studio](get-started/_static/signalr-new-project-choose-type.png)

3. Mit der rechten Maustaste des Projekts im **Projektmappen-Explorer** > **hinzufügen** > **neues Element** > **Npm-Konfigurationsdatei** . Nennen Sie die Datei *"Package.JSON"*.

4. Führen den folgenden Befehl der **Package Manager Console** Fenster aus dem Projektstamm:

    ```console
      npm install @aspnet/signalr
    ```
5. Kopieren der <em>signalr.js</em> aus der Datei <em>Node_modules\\ @aspnet\signalr\dist\browser</em>  auf die <em>Wwwroot\lib</em> Ordner des Projekts.

#### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)
1. Aus der **integrierte Terminaldienste**, führen Sie den folgenden Befehl:

    ```console
      dotnet new razor -o SignalRChat
    ```

2. Installieren Sie die JavaScript-Client Library verwenden *Npm*.

    ```
      npm init -y
      npm install @aspnet/signalr
    ```

* * *
## <a name="create-the-signalr-hub"></a>Erstellen von SignalR-Hubs.

Ein Hub ist eine Klasse, die als eine allgemeine Pipeline dient, die zum Aufrufen von Methoden voneinander Client und Server ermöglicht.

#### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)
1. Fügen Sie eine Klasse zum Projekt durch Auswahl **Datei** > **neu** > **Datei** auswählen und **Visual C#-Klasse**.

2. Erben von `Microsoft.AspNetCore.SignalR.Hub`. Die `Hub` Klasse enthält Eigenschaften und Ereignisse für die Verwaltung von Verbindungen und Gruppen sowie senden und Empfangen von Daten.

3. Erstellen der `SendMessage` -Methode, die eine Nachricht an alle verbundenen Chat-Clients sendet. Beachten Sie, es gibt eine [Aufgabe](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task(v=vs.110).aspx), da SignalR asynchron ist. Asynchronen Code skaliert besser.

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs?range=7-14)]

#### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)
1. Öffnen der *SignalRChat* Ordner in Visual Studio-Code.

2. Fügen Sie eine Klasse zum Projekt dazu **Datei** > **neue Datei** aus dem Menü.

3. Erben von `Microsoft.AspNetCore.SignalR.Hub`. Die `Hub` Klasse enthält Eigenschaften und Ereignisse für die Verwaltung von Verbindungen und Gruppen sowie senden und Empfangen von Daten an Clients.

4. Fügen Sie der Klasse eine `SendMessage`-Method hinzu. Die `SendMessage` Methode sendet eine Nachricht an alle verbundenen Chat-Clients. Beachten Sie, es gibt eine [Aufgabe](/dotnet/api/system.threading.tasks.task), da SignalR asynchron ist. Asynchronen Code skaliert besser.

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs?range=7-14)]

* * *
## <a name="configure-the-project-to-use-signalr"></a>Konfigurieren Sie das Projekt zur Verwendung von SignalR

Die SignalR-Server muss konfiguriert werden, damit dieser weiß, dass Anforderungen an SignalR übergeben.

1. Ändern Sie des Projekts zum Konfigurieren einer SignalR-Projekts `Startup.ConfigureServices` Methode.

   `services.AddSignalR` Fügt der SignalR als Teil der [Middleware](xref:fundamentals/middleware/index) Pipeline.

2. Konfigurieren von Routen zu Ihrer Verwendung Hubs `UseSignalR`.

   [!code-csharp[Startup](get-started/sample/Startup.cs?highlight=22,40-43)]

## <a name="create-the-signalr-client-code"></a>Erstellen Sie den SignalR-Clientcode

1. Ersetzen Sie den Inhalt in *Pages\Index.cshtml* durch den folgenden Code:

   [!code-cshtml[Index](get-started/sample/Pages/Index.cshtml)]

   Die vorangehende HTML zeigt Name und Nachrichtenfelder sowie eine Schaltfläche "Absenden". Beachten Sie die Skriptverweise im unteren Bereich: ein Verweis auf SignalR und *chat.js*.

2. Fügen Sie eine JavaScript-Datei, mit dem Namen *chat.js*, zu der *Wwwroot\js* Ordner. Fügen Sie ihr folgenden Code hinzu:

   [!code-javascript[Index](get-started/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a>Ausführen der App

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Wählen Sie **Debuggen** > **Starten ohne Debuggen** starten Sie einen Browser, und laden die Website lokal. Kopieren Sie die URL aus der Adressleiste ein.

1. Öffnen Sie eine andere Browserinstanz (alle Browser), und fügen Sie die URL in der Adressleiste.

1. Wählen Sie entweder Browser, geben Sie einen Namen und eine Nachricht, und klicken Sie auf die **senden** Schaltfläche. Der Name und die Meldung werden sofort auf beiden Seiten angezeigt.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Drücken Sie **Debuggen** (F5), um das Programm zu erstellen und auszuführen. Das Programm ausführen, wird ein Browserfenster geöffnet.

1. Öffnen Sie einen anderen Browserfenster, und Laden Sie die Website lokal in.

1. Wählen Sie entweder Browser, geben Sie einen Namen und eine Nachricht, und klicken Sie auf die **senden** Schaltfläche. Der Name und die Meldung werden sofort auf beiden Seiten angezeigt.

-----

  ![Lösung](get-started/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a>Weitere Informationen

[Einführung in ASP.NET Core SignalR](introduction.md)