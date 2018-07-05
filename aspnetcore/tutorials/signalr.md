---
title: Erste Schritte mit SignalR in ASP.NET Core
author: rachelappel
description: In diesem Tutorial erstellen Sie eine ASP.NET Core-App, die SignalR verwendet.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/22/2018
uid: tutorials/signalr
ms.openlocfilehash: ca9145d9e16c23e34bbc1d84ff01ce02709187ce
ms.sourcegitcommit: 08f1a9baa97060da5168840b332c9c0805b5f901
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2018
ms.locfileid: "37144871"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a>Erste Schritte mit SignalR in ASP.NET Core

Von [Rachel Appel](https://twitter.com/rachelappel)

In diesem Tutorial werden die Grundlagen zur Erstellung einer Echtzeit-App mit SignalR für ASP.NET Core beschrieben.

   ![Lösung](signalr/_static/signalr-get-started-finished.png)

In diesem Tutorial werden die folgenden SignalR-Entwicklungsaufgaben veranschaulicht:

> [!div class="checklist"]
> * Erstellen einer SignalR-Web-App für ASP.NET Core
> * Erstellen eines SignalR-Hubs zur Übermittlung von Inhalten per Push an Clients
> * Anpassen der `Startup`-Klasse und Konfigurieren der App

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="prerequisites"></a>Erforderliche Komponenten

Installieren Sie die folgenden Softwarekomponenten:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [.NET Core SDK 2.1 oder höher](https://www.microsoft.com/net/download/all)
* Version 15.7 oder höher von [Visual Studio 2017](https://www.visualstudio.com/downloads/) mit der Workload für **ASP.NET und Webentwicklung**
* [npm](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [.NET Core SDK 2.1 oder höher](https://www.microsoft.com/net/download/all)
* [Visual Studio Code](https://code.visualstudio.com/download)
* [C# für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [npm](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a>Erstellen eines ASP.NET Core-Projekts zum Hosten eines SignalR-Clients und -Servers

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

1. Klicken Sie zuerst auf die Menüoption **Datei** > **Neues Projekt** und anschließend auf **ASP.NET Core-Web-App**. Geben Sie als Projektname *SignalRChat* ein.

   ![Dialogfeld „Neues Projekt“ in Visual Studio](signalr/_static/signalr-new-project-dialog.png)

2. Klicken Sie auf **Webanwendung**, um ein Projekt mit Razor Pages zu erstellen. Klicken Sie anschließend auf **OK**. Obwohl SignalR auch unter älteren Versionen von .NET ausgeführt werden kann, sollte Sie darauf achten, **ASP.NET Core 2.1** als Framework aus der Dropdownliste auszuwählen.

   ![Dialogfeld „Neues Projekt“ in Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

Visual Studio enthält das `Microsoft.AspNetCore.SignalR`-Paket mit den zugehörigen Serverbibliotheken als Teil der **ASP.NET Core-Web-App**-Vorlage. Die JavaScript-Clientbibliothek für SignalR muss allerdings mit *npm* installiert werden.

3. Führen Sie auf der Ebene des Projektstamms die folgenden Befehle im Fenster **Paket-Manager-Konsole** aus:

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

4. Erstellen Sie einen neuen Ordner mit dem Namen „signalr“ im Ordner *lib* Ihres Projekts. Kopieren Sie die Datei *signalr.js* aus *node_modules\\@aspnet\signalr\dist\browser* in diesen Ordner.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

1. Führen Sie über das **integrierte Terminal** den folgenden Befehl aus:

    ```console
    dotnet new webapp -o SignalRChat
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

2. Installieren Sie die JavaScript-Clientbibliothek mithilfe von *npm*.

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. Erstellen Sie einen neuen Ordner mit dem Namen „signalr“ im Ordner *lib* Ihres Projekts. Kopieren Sie die Datei *signalr.js* aus *node_modules\\@aspnet\signalr\dist\browser* in diesen Ordner.

---

## <a name="create-the-signalr-hub"></a>Erstellen des SignalR-Hubs

Ein Hub ist eine Klasse, die als grundlegende Pipeline verwendet wird. Mit einer solchen Pipeline können Client und Server Methoden der jeweils anderen Komponente aufrufen.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

1. Klicken Sie zuerst auf **Datei** > **Neu** > **Datei** und anschließend auf **Visual C#-Klasse**, um dem Projekt eine Klasse hinzuzufügen. Nennen Sie die Klasse `ChatHub`und die Datei *ChatHub.cs*.

2. Stellen Sie sicher, dass Ihre Klasse von der Klasse `Microsoft.AspNetCore.SignalR.Hub` erbt. Die `Hub`-Klasse enthält Eigenschaften und Ereignisse zur Verwaltung von Verbindungen und Gruppen sowie zum Senden und Empfangen von Daten.

3. Erstellen Sie die Methode `SendMessage`. Diese sendet Nachrichten an alle verbundenen Chatclients. Beachten Sie, dass diese ein [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx)-Objekt zurückgibt, da SignalR asynchron arbeitet. Asynchroner Code erleichtert die Skalierung.

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

1. Öffnen Sie in Visual Studio Code den Ordner *SignalRChat*.

2. Klicken Sie im Menü auf **Datei** > **Neue Datei**, um dem Projekt eine Klasse hinzuzufügen. Nennen Sie die Klasse `ChatHub`und die Datei *ChatHub.cs*.

3. Stellen Sie sicher, dass Ihre Klasse von der Klasse `Microsoft.AspNetCore.SignalR.Hub` erbt. Die `Hub`-Klasse enthält Eigenschaften und Ereignisse zur Verwaltung von Verbindungen und Gruppen sowie zum Senden von Daten an Clients und zum Empfangen von Clientdaten.

4. Fügen Sie der Klasse eine `SendMessage`-Method hinzu. Die `SendMessage`-Methode sendet Nachrichten an alle verbundenen Chatclients. Beachten Sie, dass diese ein [Task](/dotnet/api/system.threading.tasks.task)-Objekt zurückgibt, da SignalR asynchron arbeitet. Asynchroner Code erleichtert die Skalierung.

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

-----

## <a name="configure-the-project-to-use-signalr"></a>Konfigurieren des Projekts zur Verwendung von SignalR

Der SignalR-Server muss zunächst konfiguriert werden, damit dieser Anforderungen an SignalR sendet.

1. Zum Konfigurieren eines SignalR-Projekts müssen Sie die Methode `Startup.ConfigureServices` des Projekts anpassen.

   `services.AddSignalR` stellt SignalR-Dienste für das [Dependency Injection](xref:fundamentals/dependency-injection)-System zur Verfügung.

1. Mit `UseSignalR` in der `Configure`-Methode konfigurieren Sie Routen zu Ihren Hubs. Durch `app.UseSignalR` wird SignalR der [Middlewarepipeline](xref:fundamentals/middleware/index) hinzugefügt.

   [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=37,57-60)]

## <a name="create-the-signalr-client-code"></a>Erstellen des SignalR-Clientcodes

1. Fügen Sie dem Ordner *wwwroot\js* eine JavaScript-Datei mit der Bezeichnung *chat.js* hinzu. Fügen Sie ihr folgenden Code hinzu:

   [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

2. Ersetzen Sie den Inhalt in *Pages\Index.cshtml* durch den folgenden Code:

   [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

   Durch den oben gezeigten HTML-Code werden Namens- und Nachrichtenfelder sowie eine Sendeschaltfläche angezeigt. Beachten Sie, dass sich im unteren Skriptbereich ein Verweis auf SignalR und *chat.js* befindet.

## <a name="run-the-app"></a>Ausführen der App

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Klicken Sie auf **Debuggen** > **Starten ohne Debuggen**, um einen Browser zu starten und die Website lokal zu laden. Kopieren Sie die URL aus der Adressleiste.

1. Öffnen Sie eine andere Instanz eines beliebigen Browsers, und fügen Sie die URL in die Adressleiste ein.

1. Geben Sie in einem der beiden Browser einen Namen und eine Nachricht ein, und klicken sie auf **Send** (Senden). Der Name und die Nachricht werden sofort auf beiden Seiten angezeigt.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Drücken Sie **Debuggen** (F5), um das Programm zu erstellen und auszuführen. Durch das Ausführen des Programms wird ein Browserfenster geöffnet.

1. Öffnen Sie ein anderes Browserfenster, und laden Sie in diesem die Website lokal.

1. Geben Sie in einem der beiden Browser einen Namen und eine Nachricht ein, und klicken sie auf **Send** (Senden). Der Name und die Nachricht werden sofort auf beiden Seiten angezeigt.

---

  ![Lösung](signalr/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a>Weitere Informationen

[Einführung in ASP.NET Core SignalR](xref:signalr/introduction)
