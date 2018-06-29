---
title: Unterschiede zwischen SignalR und ASP.NET Core SignalR
author: rachelappel
description: Unterschiede zwischen SignalR und ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.date: 06/30/2018
uid: signalr/version-differences
ms.openlocfilehash: fd314d93333bd159aef4bb4863be50c646809cf0
ms.sourcegitcommit: 1faf2525902236428dae6a59e375519bafd5d6d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2018
ms.locfileid: "37090063"
---
# <a name="differences-between-signalr-and-aspnet-core-signalr"></a>Unterschiede zwischen SignalR und ASP.NET Core SignalR

SignalR für ASP.NET Core ist nicht kompatibel mit Clients oder Server für ASP.NET SignalR. In diesem Artikel werden die Funktionen in ASP.NET Core SignalR geändert oder entfernt wurden.

## <a name="feature-differences"></a>Funktionelle Unterschiede

### <a name="automatic-reconnects"></a>Automatische wiederherstellt.

Automatische wiederherstellt werden nicht mehr unterstützt. SignalR versucht zuvor zum Server hergestellt werden soll, wenn die Verbindung getrennt wurde. Wenn der Client getrennt wird nun der Benutzer muss eine neue Verbindung explizit starten, wenn sie erneut eine Verbindung herstellen möchten.

### <a name="protocol-support"></a>Protokollunterstützung

ASP.NET Core SignalR unterstützt JSON als auch eine neue binären Protokolls basierend auf [MessagePack](xref:signalr/messagepackhubprotocol). Darüber hinaus können benutzerdefinierte Protokolle erstellt werden.

## <a name="differences-on-the-server"></a>Unterschiede auf dem server

Die SignalR-serverseitige-Bibliotheken sind in enthalten die `Microsoft.AspNetCore.App` Package, die Teil der **ASP.NET-Webanwendung für Core** Vorlage für Razor und MVC-Projekte.

SignalR ist eine Middleware ASP.NET Core, damit sie durch den Aufruf konfiguriert werden muss `AddSignalR` in `Startup.ConfigureServices`.

```csharp
services.AddSignalR();
```

Ordnen Sie die Routen zum Konfigurieren des Routings, Hubs innerhalb der `UseSignalR` -Methodenaufruf in die `Startup.Configure` Methode.

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a>Persistente Sitzungen jetzt erforderlich.

Aufgrund wie mit horizontaler Skalierung in früheren Versionen von SignalR gearbeitet haben konnten Clients erneut eine Verbindung herzustellen, und Senden von Nachrichten an einem beliebigen Server in der Farm. Aufgrund von Änderungen auf das Modell mit horizontaler Skalierung sowie die unterstützenden nicht wiederherstellt wird dies nicht mehr unterstützt. Jetzt, nachdem der Client die Verbindung mit dem Server muss für die Dauer der Verbindung mit dem gleichen Server zu interagieren.

### <a name="single-hub-per-connection"></a>Einzelnen Hub pro Verbindung

In ASP.NET SignalR Core wurde das Verbindungsmodell vereinfacht. Verbindungen werden direkt auf eine einzelne Verbindung verwendet wird, um den Zugriff auf mehrere Hubs freigeben, statt einem einzelnen Hub hergestellt.

### <a name="streaming"></a>Streaming

SignalR unterstützt jetzt auch [Streamingdaten](xref:signalr/streaming) vom Hub an den Client.

### <a name="state"></a>Zustand

Die Möglichkeit, die oft ausgegebene Befehlszeilen Zustand zwischen Clients und dem Hub (häufig als HubState bezeichnet) übergeben wurde, sowie Unterstützung für statusmeldungen aufgehoben. Es gibt keine Entsprechung der Hub-Proxys im Moment ein.

## <a name="differences-on-the-client"></a>Unterschiede auf dem client

### <a name="typescript"></a>TypeScript

Die ASP.NET Core-Version von SignalR geschrieben wird [TypeScript](https://www.typescriptlang.org/). Sie können in JavaScript oder TypeScript schreiben, bei Verwendung der [JavaScript-Client](xref:signalr/javascript-client).

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a>Der JavaScript-Client befindet sich am [Npm](https://www.npmjs.com/)

In früheren Versionen wurde der JavaScript-Client über ein NuGet-Paket in Visual Studio abgerufen. Für die Core-Versionen der [ @aspnet/signalr Npm-Paket](https://www.npmjs.com/package/@aspnet/signalr) enthält JavaScript-Bibliotheken. Dieses Paket ist nicht enthalten, der **ASP.NET-Webanwendung für Core** Vorlage. Verwenden von Npm beziehen und Installieren der `@aspnet/signalr` Npm-Paket.

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a>jQuery

Die Abhängigkeit von jQuery wurde entfernt, jedoch Projekte jQuery weiterhin verwenden können.

### <a name="javascript-client-method-syntax"></a>JavaScript-Client-Methodensyntax

Die JavaScript-Syntax wurde gegenüber der Vorgängerversion von SignalR geändert. Anstatt die `$connection` Objekt, erstellen Sie eine Verbindung mit der `HubConnectionBuilder` API.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

Verwendung `connection.on` Clientmethoden angeben, die der Hub aufrufen können.

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

Starten Sie nach dem Erstellen der Methode, die hubverbindung. Kette eine `catch` Methode zum Anmelden oder Behandeln von Fehlern.

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a>Hub-Proxys

Hub-Proxys werden nicht mehr automatisch generiert. Stattdessen wird der Name der Methode übergeben, in der `invoke` API als Zeichenfolge.

### <a name="net-and-other-clients"></a>.NET und andere clients

Die `Microsoft.AspNetCore.SignalR.Client` NuGet-Paket enthält die .NET Client-Bibliotheken für SignalR für ASP.NET Core.

Verwenden der `HubConnectionBuilder` zu erstellen, erstellen eine Instanz einer Verbindung mit einem Hub.

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
