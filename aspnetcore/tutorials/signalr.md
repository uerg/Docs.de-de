---
title: 'Tutorial: Erste Schritte mit SignalR unter ASP.NET Core'
author: tdykstra
description: In diesem Tutorial erstellen Sie eine Chat-App, die SignalR für ASP.NET Core verwendet.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/20/2018
uid: tutorials/signalr
ms.openlocfilehash: db7f31963f6a4280069f1f4f82a547e2879e64bb
ms.sourcegitcommit: d27317c16f113e7c111583042ec7e4c5a26adf6f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2018
ms.locfileid: "41751717"
---
# <a name="tutorial-get-started-with-signalr-on-aspnet-core"></a>Tutorial: Erste Schritte mit SignalR unter ASP.NET Core

In diesem Tutorial werden die Grundlagen zur Erstellung einer Echtzeit-App mit SignalR beschrieben. Sie lernen Folgendes:

> [!div class="checklist"]
> * Erstellen einer Web-App, die SignalR für ASP.NET Core verwendet
> * Erstellen eines SignalR-Hubs auf dem Server
> * Herstellen einer Verbindung zwischen JavaScript-Clients und dem SignalR-Hub
> * Verwenden des Hubs zum Senden von Nachrichten von jedem Client an alle verbundenen Clients

Am Ende verfügen Sie über eine funktionierende Chat-App:

![SignalR-Beispiel-App](signalr/_static/signalr-get-started-finished.png)

[Zeigen Sie Beispielcode an, oder laden Sie diesen herunter](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample)).

## <a name="prerequisites"></a>Erforderliche Komponenten

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Version 15.7.3 oder höher von Visual Studio 2017](https://www.visualstudio.com/downloads/) mit der Workload für **ASP.NET und Webentwicklung**
* [.NET Core SDK 2.1 oder höher](https://www.microsoft.com/net/download/all)
* [npm](https://www.npmjs.com/get-npm) (Paket-Manager für Node.js, der für die JavaScript-Clientbibliothek für SignalR verwendet wird.)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [Visual Studio Code](https://code.visualstudio.com/download)
* [.NET Core SDK 2.1 oder höher](https://www.microsoft.com/net/download/all)
* [C# für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [npm](https://www.npmjs.com/get-npm) (Paket-Manager für Node.js, der für die JavaScript-Clientbibliothek für SignalR verwendet wird.)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio für Mac](#tab/visual-studio-mac)

* [Visual Studio für Mac Version 7.5.4 oder höher](https://www.visualstudio.com/downloads/)
* [.NET Core SDK 2.1 oder höher](https://www.microsoft.com/net/download/all) (in der Visual Studio-Installation enthalten)
* [npm](https://www.npmjs.com/get-npm) (Paket-Manager für Node.js, der für die JavaScript-Clientbibliothek für SignalR verwendet wird.)

---

## <a name="create-the-project"></a>Erstellen eines Projekts

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* Klicken Sie im Menü auf **Datei > Neues Projekt**.

* Wählen Sie im Dialogfeld **Neues Projekt** **Installiert > Visual C# > Web > ASP.NET Core-Webanwendung** aus. Geben Sie als Projektname *SignalRChat* ein.

  ![Dialogfeld „Neues Projekt“ in Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* Klicken Sie auf **Webanwendung**, um ein Projekt zu erstellen, das Razor Pages verwendet.

* Wählen Sie ein Zielframework von **.NET Core** aus, und klicken Sie anschließend auf **ASP.NET Core 2.1** und **OK**.

  ![Dialogfeld „Neues Projekt“ in Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

* Öffnen Sie einen Ordner, den Sie für ein neues Projekt verwenden können.

* Führen Sie über das **integrierte Terminal** den folgenden Befehl aus:

   ```console
   dotnet new webapp -o SignalRChat
   ```

   [!INCLUDE[](~/includes/webapp-alias-notice.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio für Mac](#tab/visual-studio-mac)

* Klicken Sie im Menü auf **Datei > Neue Projektmappe**.

* Klicken Sie auf **.NET Core > App > ASP.NET Core-Web-App** (Wählen Sie nicht **ASP.NET Core-Web-App (MVC)** aus).

* Klicken Sie auf **Weiter**.

* Nennen Sie das Projekt *SignalRChat*, und wählen Sie dann **Erstellen** aus.

---

## <a name="add-the-signalr-client-library"></a>Hinzufügen der SignalR-Clientbibliothek

Die SignalR-Serverbibliothek ist im [Metapaket Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) enthalten. Sie müssen jedoch die JavaScript-Clientbibliothek über npm, den Node.js-Paket-Manager, abrufen.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* Wechseln Sie in der **Paket-Manager-Konsole** (PMC) zum Projektordner (derjenige, der die Datei *SignalRChat.csproj* enthält).

  ```console
  cd SignalRChat
  ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

2. Wechseln Sie zum neuen Projektordner.

  ```console
  cd SignalRChat
  ``` 

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio für Mac](#tab/visual-studio-mac)

* Navigieren Sie im **Terminal** zum Projektordner (derjenige, der die Datei *SignalRChat.csproj enthält).

---

* Führen Sie den npm-Initialisierer aus, um eine *package.json*-Datei zu erstellen:

  ```console
  npm init -y
  ```

  Dieser Befehl erzeugt eine Ausgabe wie die folgende:

  ```console
  Wrote to C:\tmp\SignalRChat\package.json:
  {
    "name": "SignalRChat",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1"
    },
    "keywords": [],
    "author": "",
    "license": "ISC"0
  }
  ```

* Installieren Sie das Clientbibliothekspaket:

  ```console
  npm install @aspnet/signalr
  ```

  Dieser Befehl erzeugt eine Ausgabe wie die folgende:

  ```
  npm notice created a lockfile as package-lock.json. You should commit this file.
  npm WARN signalrchat@1.0.0 No description
  npm WARN signalrchat@1.0.0 No repository field.

  + @aspnet/signalr@1.0.2
  added 1 package in 0.98s
  ```

Über den `npm install`-Befehl wurde die JavaScript-Clientbibliothek in einen Unterordner unter *node_modules* geladen. Kopieren Sie sie von dort in einen Ordner unter *wwwroot*, auf den Sie von der Chat-App-Webseite verweisen können.

* Erstellen Sie einen *signalr*-Ordner unter *wwwroot/lib*.

* Kopieren Sie die *signalr.js*-Datei von *node_modules/@aspnet/signalr/dist/browser* in den neuen *wwwroot/lib/signalr*-Ordner.

## <a name="create-the-signalr-hub"></a>Erstellen des SignalR-Hubs

Ein [Hub](xref:signalr/hubs) ist eine Klasse, die als grundlegende Pipeline verwendet wird, mit der Client/Server-Kommunikation verarbeitet wird.

* Erstellen Sie im SignalRChat-Projektordner einen *Hubs*-Ordner.

* Erstellen Sie im *Hubs*-Ordner eine *ChatHub.cs*-Datei mit folgendem Code:

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  Die `ChatHub`-Klasse erbt von der [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub)-Klasse von SignalR. Die `Hub`-Klasse verwaltet Verbindungen, Gruppen und Messaging.

  Die `SendMessage`-Methode kann von jedem verbundenen Client aufgerufen werden. Die empfangenen Nachrichten werden an alle Clients gesendet. SignalR-Code ist asynchron, damit die maximale Skalierbarkeit gewährleistet werden kann.

## <a name="configure-the-project-to-use-signalr"></a>Konfigurieren des Projekts zur Verwendung von SignalR

Der SignalR-Server muss zunächst konfiguriert werden, um Anforderungen an SignalR zu senden.

* Fügen Sie der *Startup.cs*-Datei den folgenden hervorgehobenen Code zu.

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  Durch diese Änderungen wird SignalR zum [Dependency Injection](xref:fundamentals/dependency-injection)-System sowie der [Middlewarepipeline](xref:fundamentals/middleware/index) hinzugefügt.

## <a name="create-the-signalr-client-code"></a>Erstellen des SignalR-Clientcodes

* Ersetzen Sie den Inhalt in *Pages\Index.cshtml* durch Folgendes:

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  Der vorangehende Code:

  * erstellt Textfelder für den Namen und den Nachrichtentext sowie eine Senden-Schaltfläche
  * erstellt eine Liste mit `id="messagesList"` zum Anzeigen von Nachrichten, die vom SignalR-Hub empfangen werden
  * schließt Skriptverweise sowie den *chat.js*-Anwendungscode, den Sie im folgenden Schritt erstellen, in SignalR ein

* Erstellen Sie im *wwwroot/js*-Ordner eine *chat.js*-Datei mit folgendem Code:

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  Der vorangehende Code:

  * erstellt und startet eine Verbindung
  * fügt einen Handler zur Senden-Schaltfläche hinzu, der Nachrichten an den Hub sendet
  * fügt einen Handler zum Verbindungsobjekt hinzu, der Nachrichten vom Hub empfängt und diese der Liste hinzufügt

## <a name="run-the-app"></a>Ausführen der App

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Drücken Sie **STRG+F5**, um die App ohne Debugging zu starten.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Drücken Sie **STRG+F5**, um die App ohne Debugging zu starten.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio für Mac](#tab/visual-studio-mac)

* Wählen Sie im Menü **Ausführen > Starten ohne Debuggen** aus.

---

* Kopieren Sie die URL aus der Adressleiste, öffnen Sie eine neue Browserinstanz oder Registerkarte, und fügen Sie die URL in die Adressleiste ein.

* Geben Sie in einem der beiden Browser einen Namen und eine Nachricht ein, und klicken sie auf **Send** (Senden).

  Der Name und die Nachricht werden sofort auf beiden Seiten angezeigt.

  ![SignalR-Beispiel-App](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> Wenn die App nicht funktioniert, öffnen Sie die Browser-Entwicklungstools (F12), und wechseln Sie zur Konsole. Möglicherweise werden Fehler in Bezug auf Ihren HTML- und JavaScript-Code angezeigt. Nehmen wir an, dass Sie z.B. *signalr.js* in einen anderen Ordner als vorgeschrieben platziert haben. In diesem Fall funktioniert der Verweis auf diese Datei nicht, und Ihnen wird der Fehler 404 in der Konsole angezeigt.
> ![Fehler „signalr.js nicht gefunden“](signalr/_static/f12-console.png)

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie möchten, dass Clients eine Verbindung mit einer SignalR-App von unterschiedlichen Domänen herstellt, müssen Sie die Ressourcenfreigabe zwischen verschiedenen Ursprüngen aktivieren (Cross-Origin Resource Sharing, CORS). Weiter Informationen finden Sie unter [Cross-origin resource sharing (Ressourcenfreigabe zwischen verschiedenen Ursprüngen)](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).

Weitere Informationen zu SignalR, Hubs sowie JavaScript-Clients finden Sie in folgenden Artikeln:

* [Introduction to SignalR for ASP.NET Core (Einführung in SignalR für ASP.NET Core)](xref:signalr/introduction)
* [Verwenden von Hubs in SignalR für ASP.NET Core](xref:signalr/hubs)
* [ASP.NET Core SignalR JavaScript client (JavaScript-Client in SignalR für ASP.NET Core)](xref:signalr/javascript-client)
