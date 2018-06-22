---
title: Verwenden von MessagePack Hub-Protokoll in SignalR für ASP.NET Core
author: rachelappel
description: MessagePack Hub-Protokoll zu ASP.NET Core SignalR hinzufügen.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: 702c77502868d6666cb2634b6959f029e036d14e
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274988"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a>Verwenden von MessagePack Hub-Protokoll in SignalR für ASP.NET Core

Durch [Brennan Conroy](https://github.com/BrennanConroy)

In diesem Artikel geht davon aus, der Reader wird mit den Themen vertraut [Einstieg in die](xref:tutorials/signalr).

## <a name="what-is-messagepack"></a>Was ist MessagePack?

[MessagePack](https://msgpack.org/index.html) ist eine binäre Serialisierung-Format, das schnell und compact ist. Es ist hilfreich, wenn die Leistung und Bandbreite relevant sind, da dadurch entstehen, dass kleinere Nachrichten, die im Vergleich zu [JSON](https://www.json.org/). Da es sich um ein binäres Format handelt, sind Nachrichten nicht lesbar, wenn netzwerkablaufverfolgungen und Protokolle anzeigen, wenn die Bytes durch einen MessagePack-Parser übergeben werden. SignalR verfügt über integrierte Unterstützung für das Format MessagePack und stellt APIs für Client und Server zu verwenden.

## <a name="configure-messagepack-on-the-server"></a>Konfigurieren von MessagePack auf dem server

Um die MessagePack Hub-Protokoll auf dem Server zu aktivieren, installieren Sie die `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` Paket in Ihrer app. Fügen Sie in der Datei Startup.cs `AddMessagePackProtocol` auf die `AddSignalR` Aufruf MessagePack-Unterstützung auf dem Server zu aktivieren.

> [!NOTE]
> JSON ist standardmäßig aktiviert. Hinzufügen von MessagePack ermöglicht die Unterstützung für JSON und MessagePack-Clients.

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

Anpassen, wie MessagePack Ihre Daten formatiert `AddMessagePackProtocol` akzeptiert einen Delegaten zum Konfigurieren von Optionen. In diesen Delegaten dem `FormatterResolvers` Eigenschaft kann verwendet werden, um MessagePack Serialisierungsoptionen konfigurieren. Weitere Informationen zur Funktionsweise der Konfliktlöser finden Sie auf der MessagePack-Bibliothek unter [MessagePack CSharp](https://github.com/neuecc/MessagePack-CSharp). Attribute können auf die Objekte zu serialisieren, um zu definieren, wie sie behandelt werden sollen, verwendet werden.

```csharp
services.AddSignalR()
    .AddMessagePackProtocol(options =>
    {
        options.FormatterResolvers = new List<MessagePack.IFormatterResolver>()
        {
            MessagePack.Resolvers.StandardResolver.Instance
        };
    });
```

## <a name="configure-messagepack-on-the-client"></a>Konfigurieren Sie MessagePack auf dem Client.

### <a name="net-client"></a>.NET-Client

Um MessagePack in der .NET Client zu aktivieren, installieren Sie die `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` Paket, und rufen `AddMessagePackProtocol` auf `HubConnectionBuilder`.

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> Dies `AddMessagePackProtocol` Aufruf einen Delegaten zum Konfigurieren von Optionen, wie der Server akzeptiert.

### <a name="javascript-client"></a>JavaScript-client

MessagePack-Unterstützung für den Javascript-Client wird bereitgestellt, indem die `@aspnet/signalr-protocol-msgpack` NPM-Paket.

```console
npm install @aspnet/signalr-protocol-msgpack
```

Nach der Installation der Npm-Paket, das Modul direkt über eine JavaScript-Modul-Ladeprogramm verwendet oder in den Browser durch Verweisen auf importiert werden kann die *Node_modules\\ @aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*  Datei. In einem Browser die `msgpack5` Bibliothek muss auch verwiesen werden. Verwenden einer `<script>` -Tag, um einen Verweis zu erstellen. Die Bibliothek finden Sie unter *node_modules\msgpack5\dist\msgpack5.js*.

> [!NOTE]
> Bei Verwendung der `<script>` -Element, das die Reihenfolge ist wichtig. Wenn *Signalr-Protokoll-msgpack.js* verwiesen wird, bevor Sie *msgpack5.js*, ein Fehler auftritt, wenn beim Herstellen der Verbindung mit MessagePack. *SignalR.js* ist auch erforderlich, bevor *Signalr-Protokoll-msgpack.js*.

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

Hinzufügen von `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` auf die `HubConnectionBuilder` wird Konfigurieren des Clients zum Verwenden der MessagePack-Protokoll, wenn eine Verbindung mit einem Server herstellen.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> Zu diesem Zeitpunkt stehen keine Konfigurationsoptionen für das MessagePack-Protokoll auf dem JavaScript-Client zur Verfügung.

## <a name="related-resources"></a>Weitere Informationen

* [Erste Schritte](xref:tutorials/signalr)
* [.NET-Client](xref:signalr/dotnet-client)
* [JavaScript-Client](xref:signalr/javascript-client)
