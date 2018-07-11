---
title: ASP.NET Core SignalR-JavaScript-client
author: rachelappel
description: Übersicht über ASP.NET Core SignalR JavaScript-Client.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/29/2018
uid: signalr/javascript-client
ms.openlocfilehash: d0eba401b3152d510b9db092169e83f6da2a0523
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938043"
---
# <a name="aspnet-core-signalr-javascript-client"></a>ASP.NET Core SignalR-JavaScript-client

Von [Rachel Appel](http://twitter.com/rachelappel)

Die ASP.NET Core SignalR JavaScript-Clientbibliothek ermöglicht Entwicklern von serverseitigen hubcode aufrufen.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="install-the-signalr-client-package"></a>Installieren Sie das Paket der SignalR-client

Die SignalR-JavaScript-Clientbibliothek wird bereitgestellt, als ein [Npm](https://www.npmjs.com/) Paket. Wenn Sie Visual Studio verwenden, führen Sie `npm install` aus der **-Paket-Manager-Konsole** während Sie sich in den Stammordner. Führen Sie für Visual Studio Code, den Befehl in der **integriertes Terminal**.

  ```console
   npm init -y
   npm install @aspnet/signalr
  ```

Npm installiert den Inhalt des Pakets in der *"node_modules"\\ @aspnet\signalr\dist\browser*  Ordner. Erstellen Sie einen neuen Ordner namens *Signalr* unter der *"Wwwroot"\\Lib* Ordner. Kopieren der *signalr.js* -Datei in die *Wwwroot\lib\signalr* Ordner.

## <a name="use-the-signalr-javascript-client"></a>Verwenden Sie den SignalR JavaScript-client

Verweisen auf den SignalR JavaScript-Client in der `<script>` Element.

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a>Verbinden mit einem hub

Der folgende Code erstellt und startet eine Verbindung. Die Hub-Name ist Groß-/Kleinschreibung.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12,28)]

### <a name="cross-origin-connections"></a>Cross-Origin-Verbindungen

Browser werden in der Regel Verbindungen aus der gleichen Domäne wie die angeforderte Seite laden. Es gibt jedoch Situationen, wenn eine Verbindung mit einer anderen Domäne erforderlich ist.

Um zu verhindern, dass eine schädliche Website sensible Daten von einem anderen Standort lesen [Cross-Origin-Verbindungen](xref:security/cors) sind standardmäßig deaktiviert. Um eine cors-Anforderung zu ermöglichen, aktivieren Sie ihn in das `Startup` Klasse.

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a>Aufrufen von Hub-Methoden von client

JavaScript-Clients öffentliche Methoden aufrufen Hubs durch die Verwendung `connection.invoke`. Die `invoke` -Methode akzeptiert zwei Argumente:

* Der Name der hubmethode. Im folgenden Beispiel wird der Hub-Name `SendMessage`.
* In der hubmethode definierten Argumente. Im folgenden Beispiel wird der Name des Arguments `message`.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a>Rufen Sie Client-Methoden von Hub-Instanz

Zum Empfangen von Nachrichten aus dem Hub Definieren einer Methode mit dem `connection.on` Methode.

* Der Name des Client-JavaScript-Methode. Im folgenden Beispiel wird der Name der Methode `ReceiveMessage`.
* Argumente übergibt der Hub, an die Methode. Im folgenden Beispiel wird den Wert des Arguments `message`.

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

Der vorangehende Code in `connection.on` ausgeführt wird, wenn Sie serverseitigen Code aufruft, mit der `SendAsync` Methode.

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR bestimmt, welche Clientmethode aufrufen, die nach übereinstimmenden Namen der Methode und Argumente, die in definierten `SendAsync` und `connection.on`.

> [!NOTE]
> Rufen Sie als bewährte Methode `connection.start` nach `connection.on` damit Ihre Handler registriert werden, bevor alle Nachrichten empfangen werden.

## <a name="error-handling-and-logging"></a>Fehlerbehandlung und Protokollierung

Kette eine `catch` Methode am Ende der `connection.start` Methode zum Behandeln von clientseitigen Fehlern. Verwendung `console.error` zu Ausgabefehler in der Browserkonsole.

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=28)]

Einrichten von clientseitigen Protokollierung, Ablaufverfolgung durch Übergeben einer Protokollierung und der Typ des Ereignisses zu protokollieren, wenn die Verbindung hergestellt wird. Nachrichten werden mit der angegebenen Ebene oder höher protokolliert. Verfügbaren Protokollebenen lauten wie folgt aus:

* `signalR.LogLevel.Error` : Error-Meldungen. Protokolle `Error` nur Nachrichten.
* `signalR.LogLevel.Warning` : Warnung Nachrichten zu potenziellen Fehlern. Protokolle `Warning`, und `Error` Nachrichten.
* `signalR.LogLevel.Information` : Status-Nachrichten ohne Fehler. Protokolle `Information`, `Warning`, und `Error` Nachrichten.
* `signalR.LogLevel.Trace` : Ablaufverfolgungsmeldungen Sie aus. Protokolliert alle Elemente einschließlich Daten, die zwischen Hub und dem Client übertragen.

Verwenden der `configureLogging` Methode `HubConnectionBuilder` so konfigurieren Sie die Protokollebene. Nachrichten werden in der Browserkonsole protokolliert.

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="related-resources"></a>Weitere Informationen

* [Hubs](xref:signalr/hubs)
* [.NET-Client](xref:signalr/dotnet-client)
* [Veröffentlichen in Azure](xref:signalr/publish-to-azure-web-app)
* [Aktivieren Sie Ursprungsübergreifender Anforderungen (CORS) in ASP.NET Core](xref:security/cors)
