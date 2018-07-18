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
# <a name="differences-between-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="66239-103">Unterschiede zwischen SignalR und ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="66239-103">Differences between SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="66239-104">ASP.NET Core SignalR ist nicht kompatibel mit Clients oder Servern für ASP.NET SignalR.</span><span class="sxs-lookup"><span data-stu-id="66239-104">ASP.NET Core SignalR is not compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="66239-105">In diesem Artikel werden die Funktionen, die in der ASP.NET Core SignalR geändert oder entfernt wurden.</span><span class="sxs-lookup"><span data-stu-id="66239-105">This article details the features which have been removed or changed in the ASP.NET Core SignalR.</span></span>

## <a name="feature-differences"></a><span data-ttu-id="66239-106">Funktionsunterschiede</span><span class="sxs-lookup"><span data-stu-id="66239-106">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="66239-107">Automatische Verbindungen</span><span class="sxs-lookup"><span data-stu-id="66239-107">Automatic reconnects</span></span>

<span data-ttu-id="66239-108">Automatische Verbindungen werden nicht mehr unterstützt.</span><span class="sxs-lookup"><span data-stu-id="66239-108">Automatic reconnects are no longer supported.</span></span> <span data-ttu-id="66239-109">SignalR wollten zuvor zum Server hergestellt werden soll, wenn die Verbindung getrennt wurde.</span><span class="sxs-lookup"><span data-stu-id="66239-109">Previously, SignalR tried to reconnect to the server if the connection was dropped.</span></span> <span data-ttu-id="66239-110">Wenn der Client getrennt wird nun der Benutzer muss eine neue Verbindung explizit starten, wenn sie erneut eine Verbindung herstellen möchten.</span><span class="sxs-lookup"><span data-stu-id="66239-110">Now, if the client is disconnected the user must explicitly start a new connection if they want to reconnect.</span></span>

### <a name="protocol-support"></a><span data-ttu-id="66239-111">Unterstützung des Protokolls</span><span class="sxs-lookup"><span data-stu-id="66239-111">Protocol support</span></span>

<span data-ttu-id="66239-112">SignalR für ASP.NET Core unterstützt JSON als auch ein neues binäres Protokoll, die basierend auf [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="66239-112">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="66239-113">Darüber hinaus können benutzerdefinierte Protokolle erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="66239-113">Additionally, custom protocols can be created.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="66239-114">Unterschiede, auf dem server</span><span class="sxs-lookup"><span data-stu-id="66239-114">Differences on the server</span></span>

<span data-ttu-id="66239-115">Die serverseitige SignalR-Bibliotheken befinden sich im die `Microsoft.AspNetCore.App` Paket, das Teil der **ASP.NET Core-Webanwendung** Vorlage für sowohl Razor und MVC-Projekte.</span><span class="sxs-lookup"><span data-stu-id="66239-115">The SignalR server-side libraries are included in the `Microsoft.AspNetCore.App` package that is part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="66239-116">SignalR ist eine ASP.NET Core-Middleware, sodass es durch Aufrufen von konfiguriert werden muss `AddSignalR` in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="66239-116">SignalR is an ASP.NET Core middleware, so it must be configured by calling `AddSignalR` in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR();
```

<span data-ttu-id="66239-117">Zum Konfigurieren des Routings, ordnen Sie Routen für Hubs innerhalb der `UseSignalR` -Methodenaufruf in der `Startup.Configure` Methode.</span><span class="sxs-lookup"><span data-stu-id="66239-117">To configure routing, map routes to hubs inside the `UseSignalR` method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a><span data-ttu-id="66239-118">Persistente Sitzungen jetzt erforderlich.</span><span class="sxs-lookup"><span data-stu-id="66239-118">Sticky sessions now required</span></span>

<span data-ttu-id="66239-119">Aufgrund wie horizontale Skalierung in früheren Versionen von SignalR gearbeitet haben können Clients erneut eine Verbindung herstellen und Senden von Nachrichten an einem beliebigen Server in der Farm.</span><span class="sxs-lookup"><span data-stu-id="66239-119">Because of how scale-out worked in the previous versions of SignalR, clients could reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="66239-120">Aufgrund von Änderungen an das Modell mit horizontaler hochskalierung sowie Verbindungen nicht unterstützt wird dies nicht mehr unterstützt.</span><span class="sxs-lookup"><span data-stu-id="66239-120">Due to changes to the scale-out model, as well as not supporting reconnects, this is no longer supported.</span></span> <span data-ttu-id="66239-121">Jetzt, nachdem der Client die Verbindung mit dem Server muss für die Dauer der Verbindung mit dem gleichen Server zu interagieren.</span><span class="sxs-lookup"><span data-stu-id="66239-121">Now, once the client connects to the server it needs to interact with the same server for the duration of the connection.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="66239-122">Ein Hub pro Verbindung</span><span class="sxs-lookup"><span data-stu-id="66239-122">Single hub per connection</span></span>

<span data-ttu-id="66239-123">In ASP.NET Core SignalR wurde das Verbindungsmodell vereinfacht.</span><span class="sxs-lookup"><span data-stu-id="66239-123">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="66239-124">Verbindungen werden direkt an eine einzelne Verbindung, die Zugriff auf mehrere Hubs freigeben verwendet wird, anstatt einen einzelnen Hub hergestellt werden.</span><span class="sxs-lookup"><span data-stu-id="66239-124">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="66239-125">Streaming</span><span class="sxs-lookup"><span data-stu-id="66239-125">Streaming</span></span>

<span data-ttu-id="66239-126">Unterstützt nun die SignalR [Streamingdaten](xref:signalr/streaming) aus dem Hub an den Client.</span><span class="sxs-lookup"><span data-stu-id="66239-126">SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="66239-127">Zustand</span><span class="sxs-lookup"><span data-stu-id="66239-127">State</span></span>

<span data-ttu-id="66239-128">Die Möglichkeit, die oft ausgegebene Befehlszeilen Zustand zwischen Clients und dem Hub (oft als HubState bezeichnet) übergeben wurde, sowie Unterstützung für Meldungen zum Druckfortschritt aufgehoben.</span><span class="sxs-lookup"><span data-stu-id="66239-128">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="66239-129">Es gibt keine Entsprechung der Hub-Proxys im Moment ein.</span><span class="sxs-lookup"><span data-stu-id="66239-129">There is no counterpart of hub proxies at the moment.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="66239-130">Unterschiede, auf dem client</span><span class="sxs-lookup"><span data-stu-id="66239-130">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="66239-131">TypeScript</span><span class="sxs-lookup"><span data-stu-id="66239-131">TypeScript</span></span>

<span data-ttu-id="66239-132">Die ASP.NET Core-Version von SignalR hat [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="66239-132">The ASP.NET Core version of SignalR is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="66239-133">Sie können in JavaScript oder TypeScript schreiben, bei Verwendung der [JavaScript-Client](xref:signalr/javascript-client).</span><span class="sxs-lookup"><span data-stu-id="66239-133">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="66239-134">Der JavaScript-Client befindet sich am [Npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="66239-134">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="66239-135">In früheren Versionen wurde der JavaScript-Client über NuGet-Paket in Visual Studio abgerufen.</span><span class="sxs-lookup"><span data-stu-id="66239-135">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="66239-136">Für die Core-Versionen die [ @aspnet/signalr Npm-Paket](https://www.npmjs.com/package/@aspnet/signalr) enthält die JavaScript-Bibliotheken.</span><span class="sxs-lookup"><span data-stu-id="66239-136">For the Core versions, the [@aspnet/signalr npm package](https://www.npmjs.com/package/@aspnet/signalr) contains the JavaScript libraries.</span></span> <span data-ttu-id="66239-137">Dieses Paket nicht enthalten, der **ASP.NET Core-Webanwendung** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="66239-137">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="66239-138">Mithilfe von Npm abrufen und Installieren der `@aspnet/signalr` Npm-Paket.</span><span class="sxs-lookup"><span data-stu-id="66239-138">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="66239-139">jQuery</span><span class="sxs-lookup"><span data-stu-id="66239-139">jQuery</span></span>

<span data-ttu-id="66239-140">Die Abhängigkeit von jQuery wurde entfernt, aber Projekte jQuery weiterhin verwenden können.</span><span class="sxs-lookup"><span data-stu-id="66239-140">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="66239-141">JavaScript-Client-Methodensyntax</span><span class="sxs-lookup"><span data-stu-id="66239-141">JavaScript client method syntax</span></span>

<span data-ttu-id="66239-142">Die JavaScript-Syntax wurde aus der vorherigen Version von SignalR geändert.</span><span class="sxs-lookup"><span data-stu-id="66239-142">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="66239-143">Anstatt die `$connection` Objekt, erstellen Sie eine Verbindung mit der `HubConnectionBuilder` API.</span><span class="sxs-lookup"><span data-stu-id="66239-143">Rather than using the `$connection` object, create a connection using the `HubConnectionBuilder` API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="66239-144">Verwendung `connection.on` Clientmethoden angeben, die der Hub aufgerufen werden kann.</span><span class="sxs-lookup"><span data-stu-id="66239-144">Use `connection.on` to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="66239-145">Starten Sie nach dem Erstellen der-Methode, die Hub-Verbindung ein.</span><span class="sxs-lookup"><span data-stu-id="66239-145">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="66239-146">Kette eine `catch` Methode zum Anmelden oder Fehler zu beheben.</span><span class="sxs-lookup"><span data-stu-id="66239-146">Chain a `catch` method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="66239-147">Hub-Proxys</span><span class="sxs-lookup"><span data-stu-id="66239-147">Hub proxies</span></span>

<span data-ttu-id="66239-148">Hub-Proxys werden nicht mehr automatisch generiert.</span><span class="sxs-lookup"><span data-stu-id="66239-148">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="66239-149">Stattdessen wird der Name der Methode übergeben, in der `invoke` API als Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="66239-149">Instead, the method name is passed into the `invoke` API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="66239-150">.NET und andere clients</span><span class="sxs-lookup"><span data-stu-id="66239-150">.NET and other clients</span></span>

<span data-ttu-id="66239-151">Die `Microsoft.AspNetCore.SignalR.Client` NuGet-Paket enthält die .NET Client-Bibliotheken für ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="66239-151">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="66239-152">Verwenden der `HubConnectionBuilder` erstellen und entwickeln eine Instanz einer Verbindung mit einem Hub.</span><span class="sxs-lookup"><span data-stu-id="66239-152">Use the `HubConnectionBuilder` to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="additional-resources"></a><span data-ttu-id="66239-153">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="66239-153">Additional resources</span></span>

* [<span data-ttu-id="66239-154">Hubs</span><span class="sxs-lookup"><span data-stu-id="66239-154">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="66239-155">JavaScript-Client</span><span class="sxs-lookup"><span data-stu-id="66239-155">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="66239-156">.NET-Client</span><span class="sxs-lookup"><span data-stu-id="66239-156">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="66239-157">Unterstützte Plattformen</span><span class="sxs-lookup"><span data-stu-id="66239-157">Supported platforms</span></span>](xref:signalr/supported-platforms)
