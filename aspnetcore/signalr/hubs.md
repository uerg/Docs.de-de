---
title: Verwenden von Hubs in ASP.NET Core SignalR
author: tdykstra
description: Erfahren Sie, wie Sie mit Hubs in ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/01/2018
uid: signalr/hubs
ms.openlocfilehash: e583676ab0eed45aeaf6391d8cdf8c1485aa914e
ms.sourcegitcommit: e7e1e531b80b3f4117ff119caadbebf4dcf5dcb7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2018
ms.locfileid: "44510336"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a>Verwenden von Hubs in SignalR für ASP.NET Core

Durch [Rachel Appel](https://twitter.com/rachelappel) und [Kevin Griffin](https://twitter.com/1kevgriff)

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(Herunterladen von)](xref:tutorials/index#how-to-download-a-sample)

## <a name="what-is-a-signalr-hub"></a>Was ist eine SignalR-Hub

Der SignalR-Hubs-API können Sie zum Aufrufen von Methoden auf verbundene Clients vom Server. In den Servercode definieren Sie Methoden, die vom Client aufgerufen werden. Im Clientcode definieren Sie Methoden, die vom Server aufgerufen werden. SignalR kümmert sich um alles, was hinter den Kulissen, die in Echtzeit-Client-zu-Server und Server-zu-Client-Kommunikation möglich macht.

## <a name="configure-signalr-hubs"></a>Konfigurieren von SignalR-hubs

Die SignalR-Middleware ist erforderlich, einige Dienste, die durch den Aufruf konfiguriert werden `services.AddSignalR`.

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

Beim Hinzufügen von SignalR-Funktionen zu einer ASP.NET Core-app einrichten SignalR-Routen, durch den Aufruf `app.UseSignalR` in die `Startup.Configure` Methode.

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a>Erstellen und Verwenden von hubs

Erstellen Sie einen Hub durch Deklarieren einer Klasse, die von erbt `Hub`, und fügen Sie öffentliche Methoden hinzu. Clients können als definierten Methoden aufrufen `public`.

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

Sie können einen Rückgabetyp und Parameter, einschließlich von komplexen Typen und Arrays, wie in jeder C#-Methode angeben. SignalR verarbeitet die Serialisierung und Deserialisierung von komplexen Objekten und Arrays in Ihre Parameter und Rückgabewerte.

## <a name="the-context-object"></a>Das Context-Objekt

Die `Hub` -Klasse verfügt über eine `Context` Eigenschaft, die mit Informationen über die Verbindung die folgenden Eigenschaften enthält:

| Eigenschaft | Beschreibung |
| ------ | ----------- |
| `ConnectionId` | Ruft die eindeutige ID für die Verbindung von SignalR zugewiesen. Eine Verbindungs­id für jede Verbindung ist vorhanden.|
| `UserIdentifier` | Ruft die [Benutzerbezeichner](xref:signalr/groups). SignalR verwendet standardmäßig die `ClaimTypes.NameIdentifier` aus der `ClaimsPrincipal` der Verbindung als die Benutzer-ID zugeordnet. |
| `User` | Ruft die `ClaimsPrincipal` mit dem aktuellen Benutzer verknüpft ist. |
| `Items` | Ruft eine Auflistung von Schlüssel-Wert, die zum Freigeben von Daten innerhalb des Bereichs dieser Verbindung verwendet werden kann. Daten können in dieser Auflistung gespeichert werden, und bleiben für die Verbindung in anderen hubmethodenaufrufe. |
| `Features` | Ruft die Auflistung der verfügbaren Funktionen für die Verbindung ab. Vorerst ist nicht dieser Auflistung in den meisten Szenarien erforderlich, damit noch ausführlich dokumentiert ist nicht. |
| `ConnectionAborted` | Ruft eine `CancellationToken` , die benachrichtigt werden, wenn die Verbindung abgebrochen wird. |

`Hub.Context` Außerdem enthält die folgenden Methoden:

| Methode | Beschreibung |
| ------ | ----------- |
| `GetHttpContext` | Gibt die `HttpContext` für die Verbindung oder `null` , wenn die Verbindung nicht mit einer HTTP-Anforderung verknüpft ist. Für HTTP-Verbindungen können Sie diese Methode zum Abrufen von Informationen wie z. B. HTTP-Header und Abfragezeichenfolgen. |
| `Abort` | Bricht die Verbindung ab. |

## <a name="the-clients-object"></a>Das Objekt des Clients

Die `Hub` -Klasse verfügt über eine `Clients` -Eigenschaft, die folgenden Eigenschaften für die Kommunikation zwischen Server und Client enthält:

| Eigenschaft | Beschreibung |
| ------ | ----------- |
| `All` | Ruft eine Methode für alle verbundenen clients |
| `Caller` | Ruft eine Methode auf dem Client, der die hubmethode aufgerufen hat. |
| `Others` | Ruft eine Methode für alle verbundenen Clients mit Ausnahme des Clients, die die Methode aufgerufen hat. |


`Hub.Clients` Außerdem enthält die folgenden Methoden:

| Methode | Beschreibung |
| ------ | ----------- |
| `AllExcept` | Ruft eine Methode für alle verbundenen Clients mit Ausnahme der angegebenen Verbindungen |
| `Client` | Ruft eine Methode auf einem bestimmten verbundenen client |
| `Clients` | Ruft eine Methode für bestimmte verbundene clients |
| `Group` | Ruft eine Methode für alle Verbindungen in der angegebenen Gruppe  |
| `GroupExcept` | Ruft eine Methode für alle Verbindungen in der angegebenen Gruppe an, mit Ausnahme der angegebenen Verbindungen. |
| `Groups` | Ruft eine Methode in mehrere Gruppen von Verbindungen  |
| `OthersInGroup` | Ruft eine Methode für eine Gruppe von Verbindungen mit Ausnahme des Clients, der die hubmethode aufgerufen hat  |
| `User` | Ruft eine Methode, um alle Verbindungen, die einen bestimmten Benutzer zugeordnet sind. |
| `Users` | Ruft eine Methode, um alle Verbindungen, die die angegebenen Benutzer zugeordnet sind. |

Jede Eigenschaft oder Methode in den vorhergehenden Tabellen gibt ein Objekt mit einem `SendAsync` Methode. Die `SendAsync` Methode können Sie den Namen und Parameter, der die Methode aufrufen, angeben.

## <a name="send-messages-to-clients"></a>Senden von Nachrichten an clients

Um Aufrufe von bestimmten Clients zu machen, verwenden Sie die Eigenschaften der `Clients` Objekt. Im folgenden Beispiel die `SendMessageToCaller` -Methode demonstriert, Senden einer Nachricht an die Verbindung, die die hubmethode aufgerufen hat. Die `SendMessageToGroups` Methode sendet eine Nachricht an die Gruppen, die in gespeicherten eine `List` mit dem Namen `groups`.

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a>Behandeln von Ereignissen für eine Verbindung

Die SignalR-Hubs-API bietet die `OnConnectedAsync` und `OnDisconnectedAsync` virtuelle Methoden zum Verwalten und Nachverfolgen von Verbindungen. Überschreiben der `OnConnectedAsync` virtuelle Methode, um die Aktionen ausführen, wenn einem Client an den Hub zwischen, z. B. zu einer Gruppe hinzufügen.

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a>Behandeln von Fehlern

In den hubmethoden ausgelöste Ausnahmen werden an den Client gesendet, die die Methode aufgerufen. Für JavaScript-Client die `invoke` Methode gibt eine [JavaScript-Zusage](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises). Wenn der Client einen Fehler mit einem Handler empfängt die Zusicherung mit angefügten `catch`, es wurde aufgerufen, und übergeben Sie als JavaScript- `Error` Objekt.

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a>Weitere Informationen

* [Einführung in ASP.NET Core SignalR](xref:signalr/introduction)
* [JavaScript-Client](xref:signalr/javascript-client)
* [Veröffentlichen in Azure](xref:signalr/publish-to-azure-web-app)
