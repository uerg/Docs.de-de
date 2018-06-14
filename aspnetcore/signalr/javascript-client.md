---
title: ASP.NET Core SignalR JavaScript-client
author: rachelappel
description: Übersicht über ASP.NET Core SignalR JavaScript-Client.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/29/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/javascript-client
ms.openlocfilehash: 6ff888d3337bb53d435744009f4cc24b327ebcda
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/12/2018
ms.locfileid: "35341937"
---
# <a name="aspnet-core-signalr-javascript-client"></a>ASP.NET Core SignalR JavaScript-client

Von [Rachel Appel](http://twitter.com/rachelappel)

Die ASP.NET Core SignalR JavaScript-Clientbibliothek ermöglicht Entwicklern das Aufrufen der serverseitigen hubmethode Code.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="install-the-signalr-client-package"></a>Installieren Sie das Clientpaket für SignalR

SignalR JavaScript-Clientbibliothek ist im Lieferumfang ein [Npm](https://www.npmjs.com/) Paket. Wenn Sie Visual Studio verwenden, führen Sie `npm install` aus der **Package Manager Console** dagegen im Stammordner. Führen Sie für Visual Studio-Code, den Befehl in der **integrierte Terminaldienste**.

  ```console
   npm init -y
   npm install @aspnet/signalr
  ```

Npm-installiert den Paketinhalt in der *Node_modules\\ @aspnet\signalr\dist\browser*  Ordner. Erstellen Sie einen neuen Ordner namens *Signalr* unter der *"Wwwroot"\\Lib* Ordner. Kopieren der *signalr.js* Datei wird in der *Wwwroot\lib\signalr* Ordner.

## <a name="use-the-signalr-javascript-client"></a>Verwenden Sie den SignalR JavaScript-client

Verweisen auf den SignalR JavaScript-Client in der `<script>` Element.

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a>Herstellen einer Verbindung mit einem hub

Der folgende Code erstellt und startet eine Verbindung. Der Hub-Name wird Groß-/Kleinschreibung nicht beachtet.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12,28)]

### <a name="cross-origin-connections"></a>Cross-Origin-Verbindungen

In der Regel laden Browsern Verbindungen aus der gleichen Domäne wie die angeforderte Seite. Es gibt jedoch Situationen, wenn eine Verbindung mit einer anderen Domäne erforderlich ist.

Um zu verhindern, dass eine bösartige Website lesen vertrauliche Daten von einem anderen Standort [Cross-Origin-Verbindungen](xref:security/cors) sind standardmäßig deaktiviert. Um eine Cross-Origin-Anforderung zu ermöglichen, aktivieren Sie ihn in die `Startup` Klasse.

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a>Aufruf von hubmethoden vom client

JavaScript-Clients öffentliche Methoden Aufrufen von Hubs mit `connection.invoke`. Die `invoke` Methode akzeptiert zwei Argumente:

* Der Name des Hub-Methode. Im folgenden Beispiel wird der Hub-Name `SendMessage`.
* In der hubmethode definierten Argumente. Im folgenden Beispiel wird der Name des Arguments `message`.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a>Aufrufen von Methoden vom Hub für client

Zum Empfangen von Nachrichten vom Hub definieren Sie eine Methode mithilfe der `connection.on` Methode.

* Der Name der JavaScript-Client-Methode. Im folgenden Beispiel wird der Name der Methode `ReceiveMessage`.
* Argumente übergibt der Hub an die Methode an. Im folgenden Beispiel wird der Wert des Arguments `message`.

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

Der vorangehende Code in `connection.on` ausgeführt wird, wenn serverseitige Code aufgerufen wird, mithilfe der `SendAsync` Methode.

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR bestimmt, welche Clientmethode aufgerufen werden entsprechend dem Methodennamen und Argumente definiert, `SendAsync` und `connection.on`.

> [!NOTE]
> Rufen Sie als bewährte Methode `connection.start` nach `connection.on` damit Ihrer Handler registriert werden, bevor alle Nachrichten empfangen werden.

## <a name="error-handling-and-logging"></a>Fehlerbehandlung und-Protokollierung

Kette eine `catch` Methode am Ende der `connection.start` Methode, um die clientseitige Fehler zu behandeln. Verwendung `console.error` Ausgabe Fehlermeldungen an den Browser-Konsole.

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=28)]

Setup-Protokoll für die clientseitige Protokollierung durch Übergeben einer Protokollierung und die Art des Ereignisses zu protokollieren, wenn die Verbindung hergestellt wird. Nachrichten werden mit der angegebenen Protokollierungsebene und höher protokolliert. Verfügbare Protokollebenen lauten wie folgt:

* `signalR.LogLevel.Error` : Fehlermeldungen. Protokolle `Error` nur Nachrichten.
* `signalR.LogLevel.Warning` : Warnung Nachrichten zu möglichen Fehlern. Protokolle `Warning`, und `Error` Nachrichten.
* `signalR.LogLevel.Information` : Status Nachrichten ohne Fehler. Protokolle `Information`, `Warning`, und `Error` Nachrichten.
* `signalR.LogLevel.Trace` : Verfolgen von Nachrichten. Protokolliert alle Elemente einschließlich Daten, die zwischen Nabe -zu-Client transportiert.

Verwenden der `configureLogging` Methode `HubConnectionBuilder` so konfigurieren Sie die Protokollebene. Nachrichten werden an die Browserkonsole protokolliert.

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="related-resources"></a>Weitere Informationen

* [Hubs](xref:signalr/hubs)
* [.NET-Client](xref:signalr/dotnet-client)
* [Veröffentlichen in Azure](xref:signalr/publish-to-azure-web-app)
* [Aktivieren Sie Cross-Origin-Anforderungen (CORS) in ASP.NET Core](xref:security/cors)