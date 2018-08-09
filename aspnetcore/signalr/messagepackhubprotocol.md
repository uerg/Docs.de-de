---
title: Verwenden von MessagePack Hub-Protokoll in SignalR für ASP.NET Core
author: tdykstra
description: Fügen Sie in ASP.NET Core SignalR MessagePack Hub-Protokoll hinzu.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: 78b708c50ce7a8101c9eaa558171540e61c0d7f0
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2018
ms.locfileid: "39094994"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a><span data-ttu-id="6b49c-103">Verwenden von MessagePack Hub-Protokoll in SignalR für ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6b49c-103">Use MessagePack Hub Protocol in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="6b49c-104">Durch [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="6b49c-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="6b49c-105">In diesem Artikel geht davon aus, der Reader befindet sich mit den Themen vertraut [Einstieg](xref:tutorials/signalr).</span><span class="sxs-lookup"><span data-stu-id="6b49c-105">This article assumes the reader is familiar with the topics covered in [Get Started](xref:tutorials/signalr).</span></span>

## <a name="what-is-messagepack"></a><span data-ttu-id="6b49c-106">Was ist MessagePack?</span><span class="sxs-lookup"><span data-stu-id="6b49c-106">What is MessagePack?</span></span>

<span data-ttu-id="6b49c-107">[MessagePack](https://msgpack.org/index.html) ist ein binäres Serialisierungsformat, das schnell und kompakt ist.</span><span class="sxs-lookup"><span data-stu-id="6b49c-107">[MessagePack](https://msgpack.org/index.html) is a binary serialization format that is fast and compact.</span></span> <span data-ttu-id="6b49c-108">Es ist hilfreich, wenn die Leistung und relevant sind, da kleinere Nachrichten, die im Vergleich zu erstellt [JSON](https://www.json.org/).</span><span class="sxs-lookup"><span data-stu-id="6b49c-108">It's useful when performance and bandwidth are a concern because it creates smaller messages compared to [JSON](https://www.json.org/).</span></span> <span data-ttu-id="6b49c-109">Da es sich um ein binäres Format handelt, sind Nachrichten unlesbar, wenn es sich bei netzwerkablaufverfolgungen und die Protokolle ansehen, wenn die Bytes, die über einen MessagePack-Parser übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="6b49c-109">Because it's a binary format, messages are unreadable when looking at network traces and logs unless the bytes are passed through a MessagePack parser.</span></span> <span data-ttu-id="6b49c-110">SignalR verfügt über integrierte Unterstützung für das Format MessagePack und stellt APIs für Client und Server zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="6b49c-110">SignalR has built-in support for the MessagePack format, and provides APIs for the client and server to use.</span></span>

## <a name="configure-messagepack-on-the-server"></a><span data-ttu-id="6b49c-111">Konfigurieren Sie MessagePack, auf dem server</span><span class="sxs-lookup"><span data-stu-id="6b49c-111">Configure MessagePack on the server</span></span>

<span data-ttu-id="6b49c-112">Um das MessagePack-Hub-Protokoll auf dem Server zu aktivieren, installieren die `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` Paket in Ihrer app.</span><span class="sxs-lookup"><span data-stu-id="6b49c-112">To enable the MessagePack Hub Protocol on the server, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package in your app.</span></span> <span data-ttu-id="6b49c-113">Fügen Sie in der Datei "Startup.cs" `AddMessagePackProtocol` auf die `AddSignalR` Aufruf MessagePack-Unterstützung auf dem Server zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="6b49c-113">In the Startup.cs file add `AddMessagePackProtocol` to the `AddSignalR` call to enable MessagePack support on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="6b49c-114">JSON ist standardmäßig aktiviert.</span><span class="sxs-lookup"><span data-stu-id="6b49c-114">JSON is enabled by default.</span></span> <span data-ttu-id="6b49c-115">Hinzufügen von MessagePack bietet Unterstützung für JSON- und MessagePack-Clients.</span><span class="sxs-lookup"><span data-stu-id="6b49c-115">Adding MessagePack enables support for both JSON and MessagePack clients.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

<span data-ttu-id="6b49c-116">Zum Anpassen, wie MessagePack Ihrer Daten, formatieren soll `AddMessagePackProtocol` verwendet einen Delegaten für das Konfigurieren von Optionen.</span><span class="sxs-lookup"><span data-stu-id="6b49c-116">To customize how MessagePack will format your data, `AddMessagePackProtocol` takes a delegate for configuring options.</span></span> <span data-ttu-id="6b49c-117">In diesen Delegaten dem `FormatterResolvers` Eigenschaft kann verwendet werden, um MessagePack Serialisierungsoptionen konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="6b49c-117">In that delegate, the `FormatterResolvers` property can be used to configure MessagePack serialization options.</span></span> <span data-ttu-id="6b49c-118">Weitere Informationen zur Funktionsweise der Konfliktlöser finden Sie auf der MessagePack-Bibliothek unter [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span><span class="sxs-lookup"><span data-stu-id="6b49c-118">For more information on how the resolvers work, visit the MessagePack library at [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span></span> <span data-ttu-id="6b49c-119">Attribute können für die Objekte zu serialisieren, die zum definieren, wie sie behandelt werden sollen, verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="6b49c-119">Attributes can be used on the objects you want to serialize to define how they should be handled.</span></span>

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

## <a name="configure-messagepack-on-the-client"></a><span data-ttu-id="6b49c-120">Konfigurieren Sie MessagePack auf dem Client.</span><span class="sxs-lookup"><span data-stu-id="6b49c-120">Configure MessagePack on the client</span></span>

### <a name="net-client"></a><span data-ttu-id="6b49c-121">.NET-Client</span><span class="sxs-lookup"><span data-stu-id="6b49c-121">.NET client</span></span>

<span data-ttu-id="6b49c-122">Um MessagePack im .NET-Client zu aktivieren, installieren die `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` -Paket, und rufen `AddMessagePackProtocol` auf `HubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="6b49c-122">To enable MessagePack in the .NET Client, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package and call `AddMessagePackProtocol` on `HubConnectionBuilder`.</span></span>

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> <span data-ttu-id="6b49c-123">Dies `AddMessagePackProtocol` Aufruf nimmt einen Delegaten für das Konfigurieren von Optionen wie den Server.</span><span class="sxs-lookup"><span data-stu-id="6b49c-123">This `AddMessagePackProtocol` call takes a delegate for configuring options just like the server.</span></span>

### <a name="javascript-client"></a><span data-ttu-id="6b49c-124">JavaScript-client</span><span class="sxs-lookup"><span data-stu-id="6b49c-124">JavaScript client</span></span>

<span data-ttu-id="6b49c-125">MessagePack-Unterstützung für Javascript-Client wird bereitgestellt, durch die `@aspnet/signalr-protocol-msgpack` NPM-Paket.</span><span class="sxs-lookup"><span data-stu-id="6b49c-125">MessagePack support for the Javascript client is provided by the `@aspnet/signalr-protocol-msgpack` NPM package.</span></span>

```console
npm install @aspnet/signalr-protocol-msgpack
```

<span data-ttu-id="6b49c-126">Nach der Installation des Npm-Pakets, das Modul direkt über ein JavaScript-Modul-Ladeprogramm verwendet oder in den Browser durch Verweisen auf importiert werden kann die *"node_modules"\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* Datei.</span><span class="sxs-lookup"><span data-stu-id="6b49c-126">After installing the npm package, the module can be used directly via a JavaScript module loader or imported into the browser by referencing the *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* file.</span></span> <span data-ttu-id="6b49c-127">In einem Browser die `msgpack5` Bibliothek muss auch verwiesen werden.</span><span class="sxs-lookup"><span data-stu-id="6b49c-127">In a browser the `msgpack5` library must also be referenced.</span></span> <span data-ttu-id="6b49c-128">Verwenden einer `<script>` Tag, um einen Verweis zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="6b49c-128">Use a `<script>` tag to create a reference.</span></span> <span data-ttu-id="6b49c-129">Die Bibliothek finden Sie unter *node_modules\msgpack5\dist\msgpack5.js*.</span><span class="sxs-lookup"><span data-stu-id="6b49c-129">The library can be found at *node_modules\msgpack5\dist\msgpack5.js*.</span></span>

> [!NOTE]
> <span data-ttu-id="6b49c-130">Bei Verwendung der `<script>` -Element, das die Reihenfolge ist wichtig.</span><span class="sxs-lookup"><span data-stu-id="6b49c-130">When using the `<script>` element, the order is important.</span></span> <span data-ttu-id="6b49c-131">Wenn *Signalr-Protokoll-msgpack.js* verwiesen wird, bevor Sie *msgpack5.js*, ein Fehler tritt auf, wenn es sich bei mit MessagePack eine Verbindung herstellen möchten.</span><span class="sxs-lookup"><span data-stu-id="6b49c-131">If *signalr-protocol-msgpack.js* is referenced before *msgpack5.js*, an error occurs when trying to connect with MessagePack.</span></span> <span data-ttu-id="6b49c-132">*SignalR.js* ist auch erforderlich, bevor *Signalr-Protokoll-msgpack.js*.</span><span class="sxs-lookup"><span data-stu-id="6b49c-132">*signalr.js* is also required before *signalr-protocol-msgpack.js*.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

<span data-ttu-id="6b49c-133">Hinzufügen von `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` auf die `HubConnectionBuilder` konfiguriert den Client das Protokoll MessagePack beim Verbinden mit einem Server zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="6b49c-133">Adding `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` to the `HubConnectionBuilder` will configure the client to use the MessagePack protocol when connecting to a server.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> <span data-ttu-id="6b49c-134">Zu diesem Zeitpunkt stehen keine Konfigurationsoptionen für das Protokoll MessagePack auf dem JavaScript-Client zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="6b49c-134">At this time, there are no configuration options for the MessagePack protocol on the JavaScript client.</span></span>

## <a name="related-resources"></a><span data-ttu-id="6b49c-135">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="6b49c-135">Related resources</span></span>

* [<span data-ttu-id="6b49c-136">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="6b49c-136">Get Started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="6b49c-137">.NET-Client</span><span class="sxs-lookup"><span data-stu-id="6b49c-137">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="6b49c-138">JavaScript-Client</span><span class="sxs-lookup"><span data-stu-id="6b49c-138">JavaScript client</span></span>](xref:signalr/javascript-client)
