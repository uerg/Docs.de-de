---
title: Unterschiede zwischen SignalR und ASP.NET Core SignalR
author: tdykstra
description: Unterschiede zwischen SignalR und ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.date: 06/30/2018
uid: signalr/version-differences
ms.openlocfilehash: 6ed7e2e1ecadef08d71c4d7a7c3469738d07bcda
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095007"
---
# <a name="differences-between-signalr-and-aspnet-core-signalr"></a>Unterschiede zwischen SignalR und ASP.NET Core SignalR

ASP.NET Core SignalR ist nicht kompatibel mit Clients oder Servern für ASP.NET SignalR. In diesem Artikel werden die Funktionen, die in der ASP.NET Core SignalR geändert oder entfernt wurden.

## <a name="feature-differences"></a>Funktionsunterschiede

### <a name="automatic-reconnects"></a>Automatische Verbindungen

Automatische Verbindungen werden nicht mehr unterstützt. SignalR wollten zuvor zum Server hergestellt werden soll, wenn die Verbindung getrennt wurde. Wenn der Client getrennt wird nun der Benutzer muss eine neue Verbindung explizit starten, wenn sie erneut eine Verbindung herstellen möchten.

### <a name="protocol-support"></a>Unterstützung des Protokolls

SignalR für ASP.NET Core unterstützt JSON als auch ein neues binäres Protokoll, die basierend auf [MessagePack](xref:signalr/messagepackhubprotocol). Darüber hinaus können benutzerdefinierte Protokolle erstellt werden.

## <a name="differences-on-the-server"></a>Unterschiede, auf dem server

Die serverseitige SignalR-Bibliotheken befinden sich im die `Microsoft.AspNetCore.App` Paket, das Teil der **ASP.NET Core-Webanwendung** Vorlage für sowohl Razor und MVC-Projekte.

SignalR ist eine ASP.NET Core-Middleware, sodass es durch Aufrufen von konfiguriert werden muss `AddSignalR` in `Startup.ConfigureServices`.

```csharp
services.AddSignalR();
```

Zum Konfigurieren des Routings, ordnen Sie Routen für Hubs innerhalb der `UseSignalR` -Methodenaufruf in der `Startup.Configure` Methode.

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a>Persistente Sitzungen jetzt erforderlich.

Aufgrund wie horizontale Skalierung in früheren Versionen von SignalR gearbeitet haben können Clients erneut eine Verbindung herstellen und Senden von Nachrichten an einem beliebigen Server in der Farm. Aufgrund von Änderungen an das Modell mit horizontaler hochskalierung sowie Verbindungen nicht unterstützt wird dies nicht mehr unterstützt. Jetzt, nachdem der Client die Verbindung mit dem Server muss für die Dauer der Verbindung mit dem gleichen Server zu interagieren.

### <a name="single-hub-per-connection"></a>Ein Hub pro Verbindung

In ASP.NET Core SignalR wurde das Verbindungsmodell vereinfacht. Verbindungen werden direkt an eine einzelne Verbindung, die Zugriff auf mehrere Hubs freigeben verwendet wird, anstatt einen einzelnen Hub hergestellt werden.

### <a name="streaming"></a>Streaming

Unterstützt nun die SignalR [Streamingdaten](xref:signalr/streaming) aus dem Hub an den Client.

### <a name="state"></a>Zustand

Die Möglichkeit, die oft ausgegebene Befehlszeilen Zustand zwischen Clients und dem Hub (oft als HubState bezeichnet) übergeben wurde, sowie Unterstützung für Meldungen zum Druckfortschritt aufgehoben. Es gibt keine Entsprechung der Hub-Proxys im Moment ein.

## <a name="differences-on-the-client"></a>Unterschiede, auf dem client

### <a name="typescript"></a>TypeScript

Die ASP.NET Core-Version von SignalR hat [TypeScript](https://www.typescriptlang.org/). Sie können in JavaScript oder TypeScript schreiben, bei Verwendung der [JavaScript-Client](xref:signalr/javascript-client).

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a>Der JavaScript-Client befindet sich am [Npm](https://www.npmjs.com/)

In früheren Versionen wurde der JavaScript-Client über NuGet-Paket in Visual Studio abgerufen. Für die Core-Versionen die [ @aspnet/signalr Npm-Paket](https://www.npmjs.com/package/@aspnet/signalr) enthält die JavaScript-Bibliotheken. Dieses Paket nicht enthalten, der **ASP.NET Core-Webanwendung** Vorlage. Mithilfe von Npm abrufen und Installieren der `@aspnet/signalr` Npm-Paket.

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a>jQuery

Die Abhängigkeit von jQuery wurde entfernt, aber Projekte jQuery weiterhin verwenden können.

### <a name="javascript-client-method-syntax"></a>JavaScript-Client-Methodensyntax

Die JavaScript-Syntax wurde aus der vorherigen Version von SignalR geändert. Anstatt die `$connection` Objekt, erstellen Sie eine Verbindung mit der `HubConnectionBuilder` API.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

Verwendung `connection.on` Clientmethoden angeben, die der Hub aufgerufen werden kann.

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

Starten Sie nach dem Erstellen der-Methode, die Hub-Verbindung ein. Kette eine `catch` Methode zum Anmelden oder Fehler zu beheben.

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a>Hub-Proxys

Hub-Proxys werden nicht mehr automatisch generiert. Stattdessen wird der Name der Methode übergeben, in der `invoke` API als Zeichenfolge.

### <a name="net-and-other-clients"></a>.NET und andere clients

Die `Microsoft.AspNetCore.SignalR.Client` NuGet-Paket enthält die .NET Client-Bibliotheken für ASP.NET Core SignalR.

Verwenden der `HubConnectionBuilder` erstellen und entwickeln eine Instanz einer Verbindung mit einem Hub.

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Hubs](xref:signalr/hubs)
* [JavaScript-Client](xref:signalr/javascript-client)
* [.NET-Client](xref:signalr/dotnet-client)
* [Unterstützte Plattformen](xref:signalr/supported-platforms)
