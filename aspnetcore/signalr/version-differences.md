---
title: Unterschiede zwischen SignalR und ASP.NET Core SignalR
author: tdykstra
description: Unterschiede zwischen SignalR und ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.date: 09/10/2018
uid: signalr/version-differences
ms.openlocfilehash: 4ac7952f26500285fc1c8f9453feb3ea8b33851a
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50089827"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="c4db3-103">Unterschiede zwischen SignalR für ASP.NET und ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="c4db3-103">Differences between ASP.NET SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="c4db3-104">ASP.NET Core SignalR nicht kompatibel mit Clients oder Servern für ASP.NET SignalR.</span><span class="sxs-lookup"><span data-stu-id="c4db3-104">ASP.NET Core SignalR isn't compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="c4db3-105">In diesem Artikel werden die Funktionen, die in ASP.NET Core SignalR geändert oder entfernt wurden.</span><span class="sxs-lookup"><span data-stu-id="c4db3-105">This article details features which have been removed or changed in ASP.NET Core SignalR.</span></span>

## <a name="how-to-identify-the-signalr-version"></a><span data-ttu-id="c4db3-106">So identifizieren Sie die SignalR-version</span><span class="sxs-lookup"><span data-stu-id="c4db3-106">How to identify the SignalR version</span></span>

|                      | <span data-ttu-id="c4db3-107">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="c4db3-107">ASP.NET SignalR</span></span> | <span data-ttu-id="c4db3-108">ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="c4db3-108">ASP.NET Core SignalR</span></span> |
| -------------------- | --------------- | -------------------- |
| <span data-ttu-id="c4db3-109">Server-NuGet-Paket</span><span class="sxs-lookup"><span data-stu-id="c4db3-109">Server NuGet Package</span></span> | [<span data-ttu-id="c4db3-110">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="c4db3-110">Microsoft.AspNet.SignalR</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | <span data-ttu-id="c4db3-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) ((.NET Core)</span><span class="sxs-lookup"><span data-stu-id="c4db3-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span></span><br><span data-ttu-id="c4db3-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) ((.NET Framework)</span><span class="sxs-lookup"><span data-stu-id="c4db3-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span></span> |
| <span data-ttu-id="c4db3-113">Die NuGet-Pakete</span><span class="sxs-lookup"><span data-stu-id="c4db3-113">Client NuGet Packages</span></span> | [<span data-ttu-id="c4db3-114">Microsoft.AspNet.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="c4db3-114">Microsoft.AspNet.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[<span data-ttu-id="c4db3-115">Microsoft.AspNet.SignalR.JS</span><span class="sxs-lookup"><span data-stu-id="c4db3-115">Microsoft.AspNet.SignalR.JS</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [<span data-ttu-id="c4db3-116">Microsoft.AspNetCore.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="c4db3-116">Microsoft.AspNetCore.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| <span data-ttu-id="c4db3-117">Client-Npm-Pakets</span><span class="sxs-lookup"><span data-stu-id="c4db3-117">Client npm Package</span></span> | [<span data-ttu-id="c4db3-118">SignalR</span><span class="sxs-lookup"><span data-stu-id="c4db3-118">signalr</span></span>](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| <span data-ttu-id="c4db3-119">Server App-Typ</span><span class="sxs-lookup"><span data-stu-id="c4db3-119">Server App Type</span></span> | <span data-ttu-id="c4db3-120">ASP.NET ("System.Web") oder Selbstgehostetem</span><span class="sxs-lookup"><span data-stu-id="c4db3-120">ASP.NET (System.Web) or OWIN Self-Host</span></span> | <span data-ttu-id="c4db3-121">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c4db3-121">ASP.NET Core</span></span> |
| <span data-ttu-id="c4db3-122">Unterstützte Plattformen</span><span class="sxs-lookup"><span data-stu-id="c4db3-122">Supported Server Platforms</span></span> | <span data-ttu-id="c4db3-123">.NET Framework 4.5 oder höher</span><span class="sxs-lookup"><span data-stu-id="c4db3-123">.NET Framework 4.5 or later</span></span> | <span data-ttu-id="c4db3-124">.NET Framework 4.6.1 oder höher</span><span class="sxs-lookup"><span data-stu-id="c4db3-124">.NET Framework 4.6.1 or later</span></span><br><span data-ttu-id="c4db3-125">.NET Core 2.1 oder höher</span><span class="sxs-lookup"><span data-stu-id="c4db3-125">.NET Core 2.1 or later</span></span> |

## <a name="feature-differences"></a><span data-ttu-id="c4db3-126">Funktionsunterschiede</span><span class="sxs-lookup"><span data-stu-id="c4db3-126">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="c4db3-127">Automatische Verbindungen</span><span class="sxs-lookup"><span data-stu-id="c4db3-127">Automatic reconnects</span></span>

<span data-ttu-id="c4db3-128">Automatische Verbindungen werden in ASP.NET Core SignalR nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="c4db3-128">Automatic reconnects aren't supported in ASP.NET Core SignalR.</span></span> <span data-ttu-id="c4db3-129">Wenn der Client getrennt wird, muss der Benutzer eine neue Verbindung explizit starten, wenn sie erneut eine Verbindung herstellen möchten.</span><span class="sxs-lookup"><span data-stu-id="c4db3-129">If the client is disconnected, the user must explicitly start a new connection if they want to reconnect.</span></span> <span data-ttu-id="c4db3-130">SignalR versucht, in ASP.NET SignalR zum Server hergestellt werden soll, wenn die Verbindung getrennt wird.</span><span class="sxs-lookup"><span data-stu-id="c4db3-130">In ASP.NET SignalR, SignalR attempts to reconnect to the server if the connection is dropped.</span></span> 

### <a name="protocol-support"></a><span data-ttu-id="c4db3-131">Unterstützung des Protokolls</span><span class="sxs-lookup"><span data-stu-id="c4db3-131">Protocol support</span></span>

<span data-ttu-id="c4db3-132">SignalR für ASP.NET Core unterstützt JSON als auch ein neues binäres Protokoll, die basierend auf [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="c4db3-132">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="c4db3-133">Darüber hinaus können benutzerdefinierte Protokolle erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="c4db3-133">Additionally, custom protocols can be created.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="c4db3-134">Unterschiede, auf dem server</span><span class="sxs-lookup"><span data-stu-id="c4db3-134">Differences on the server</span></span>

<span data-ttu-id="c4db3-135">Die ASP.NET Core SignalR serverseitige Bibliotheken befinden sich der [Microsoft.AspNetCore.App metapaket](xref:fundamentals/metapackage-app) Paket, das Teil der **ASP.NET Core-Webanwendung** Vorlage für Razor und MVC Projekte.</span><span class="sxs-lookup"><span data-stu-id="c4db3-135">The ASP.NET Core SignalR server-side libraries are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) package that's part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="c4db3-136">ASP.NET Core SignalR ist eine ASP.NET Core-Middleware, sodass es durch Aufrufen von konfiguriert werden muss [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="c4db3-136">ASP.NET Core SignalR is an ASP.NET Core middleware, so it must be configured by calling [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR()
```

<span data-ttu-id="c4db3-137">Zum Konfigurieren des Routings, ordnen Sie Routen für Hubs innerhalb der [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) -Methodenaufruf in der `Startup.Configure` Methode.</span><span class="sxs-lookup"><span data-stu-id="c4db3-137">To configure routing, map routes to hubs inside the [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a><span data-ttu-id="c4db3-138">Persistente Sitzungen jetzt erforderlich.</span><span class="sxs-lookup"><span data-stu-id="c4db3-138">Sticky sessions now required</span></span>

<span data-ttu-id="c4db3-139">Aufgrund wie horizontale Skalierung in SignalR für ASP.NET gearbeitet haben können Clients erneut eine Verbindung herstellen und Senden von Nachrichten an einem beliebigen Server in der Farm.</span><span class="sxs-lookup"><span data-stu-id="c4db3-139">Because of how scale-out worked in ASP.NET SignalR, clients could reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="c4db3-140">Aufgrund von Änderungen an das Modell mit horizontaler hochskalierung sowie Verbindungen nicht unterstützt wird dies nicht mehr unterstützt.</span><span class="sxs-lookup"><span data-stu-id="c4db3-140">Due to changes to the scale-out model, as well as not supporting reconnects, this is no longer supported.</span></span> <span data-ttu-id="c4db3-141">Nachdem der Client mit dem Server verbunden ist, muss er für die Dauer der Verbindung mit dem gleichen Server interagieren.</span><span class="sxs-lookup"><span data-stu-id="c4db3-141">Once the client connects to the server, it must interact with the same server for the duration of the connection.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="c4db3-142">Ein Hub pro Verbindung</span><span class="sxs-lookup"><span data-stu-id="c4db3-142">Single hub per connection</span></span>

<span data-ttu-id="c4db3-143">In ASP.NET Core SignalR wurde das Verbindungsmodell vereinfacht.</span><span class="sxs-lookup"><span data-stu-id="c4db3-143">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="c4db3-144">Verbindungen werden direkt an eine einzelne Verbindung, die Zugriff auf mehrere Hubs freigeben verwendet wird, anstatt einen einzelnen Hub hergestellt werden.</span><span class="sxs-lookup"><span data-stu-id="c4db3-144">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="c4db3-145">Streaming</span><span class="sxs-lookup"><span data-stu-id="c4db3-145">Streaming</span></span>

<span data-ttu-id="c4db3-146">Unterstützt nun das ASP.NET Core SignalR [Streamingdaten](xref:signalr/streaming) aus dem Hub an den Client.</span><span class="sxs-lookup"><span data-stu-id="c4db3-146">ASP.NET Core SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="c4db3-147">Zustand</span><span class="sxs-lookup"><span data-stu-id="c4db3-147">State</span></span>

<span data-ttu-id="c4db3-148">Die Möglichkeit, die oft ausgegebene Befehlszeilen Zustand zwischen Clients und dem Hub (oft als HubState bezeichnet) übergeben wurde, sowie Unterstützung für Meldungen zum Druckfortschritt aufgehoben.</span><span class="sxs-lookup"><span data-stu-id="c4db3-148">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="c4db3-149">Es gibt keine Entsprechung der Hub-Proxys im Moment ein.</span><span class="sxs-lookup"><span data-stu-id="c4db3-149">There is no counterpart of hub proxies at the moment.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="c4db3-150">Unterschiede, auf dem client</span><span class="sxs-lookup"><span data-stu-id="c4db3-150">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="c4db3-151">TypeScript</span><span class="sxs-lookup"><span data-stu-id="c4db3-151">TypeScript</span></span>

<span data-ttu-id="c4db3-152">Der ASP.NET Core SignalR-Client hat [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="c4db3-152">The ASP.NET Core SignalR client is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="c4db3-153">Sie können in JavaScript oder TypeScript schreiben, bei Verwendung der [JavaScript-Client](xref:signalr/javascript-client).</span><span class="sxs-lookup"><span data-stu-id="c4db3-153">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="c4db3-154">Der JavaScript-Client befindet sich am [Npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="c4db3-154">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="c4db3-155">In früheren Versionen wurde der JavaScript-Client über NuGet-Paket in Visual Studio abgerufen.</span><span class="sxs-lookup"><span data-stu-id="c4db3-155">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="c4db3-156">Für die Core-Versionen die [ @aspnet/signalr ](https://www.npmjs.com/package/@aspnet/signalr) Npm-Paket enthält die JavaScript-Bibliotheken.</span><span class="sxs-lookup"><span data-stu-id="c4db3-156">For the Core versions, the [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) npm package contains the JavaScript libraries.</span></span> <span data-ttu-id="c4db3-157">Dieses Paket nicht enthalten, der **ASP.NET Core-Webanwendung** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="c4db3-157">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="c4db3-158">Mithilfe von Npm abrufen und Installieren der `@aspnet/signalr` Npm-Paket.</span><span class="sxs-lookup"><span data-stu-id="c4db3-158">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="c4db3-159">jQuery</span><span class="sxs-lookup"><span data-stu-id="c4db3-159">jQuery</span></span>

<span data-ttu-id="c4db3-160">Die Abhängigkeit von jQuery wurde entfernt, aber Projekte jQuery weiterhin verwenden können.</span><span class="sxs-lookup"><span data-stu-id="c4db3-160">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="c4db3-161">JavaScript-Client-Methodensyntax</span><span class="sxs-lookup"><span data-stu-id="c4db3-161">JavaScript client method syntax</span></span>

<span data-ttu-id="c4db3-162">Die JavaScript-Syntax wurde aus der vorherigen Version von SignalR geändert.</span><span class="sxs-lookup"><span data-stu-id="c4db3-162">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="c4db3-163">Anstatt die `$connection` Objekt, erstellen Sie eine Verbindung mit der [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span><span class="sxs-lookup"><span data-stu-id="c4db3-163">Rather than using the `$connection` object, create a connection using the [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="c4db3-164">Verwenden der [auf](/javascript/api/@aspnet/signalr/HubConnection#on) Methode, um die Clientmethoden angeben, die der Hub aufgerufen werden kann.</span><span class="sxs-lookup"><span data-stu-id="c4db3-164">Use the [on](/javascript/api/@aspnet/signalr/HubConnection#on) method to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="c4db3-165">Starten Sie nach dem Erstellen der-Methode, die Hub-Verbindung ein.</span><span class="sxs-lookup"><span data-stu-id="c4db3-165">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="c4db3-166">Kette eine [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) Methode zum Anmelden oder Fehler zu beheben.</span><span class="sxs-lookup"><span data-stu-id="c4db3-166">Chain a [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="c4db3-167">Hub-Proxys</span><span class="sxs-lookup"><span data-stu-id="c4db3-167">Hub proxies</span></span>

<span data-ttu-id="c4db3-168">Hub-Proxys werden nicht mehr automatisch generiert.</span><span class="sxs-lookup"><span data-stu-id="c4db3-168">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="c4db3-169">Stattdessen wird der Name der Methode übergeben, in der [Aufrufen](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API als Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="c4db3-169">Instead, the method name is passed into the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="c4db3-170">.NET und andere clients</span><span class="sxs-lookup"><span data-stu-id="c4db3-170">.NET and other clients</span></span>

<span data-ttu-id="c4db3-171">Die `Microsoft.AspNetCore.SignalR.Client` NuGet-Paket enthält die .NET Client-Bibliotheken für ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="c4db3-171">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="c4db3-172">Verwenden der [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) erstellen und entwickeln eine Instanz einer Verbindung mit einem Hub.</span><span class="sxs-lookup"><span data-stu-id="c4db3-172">Use the [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a><span data-ttu-id="c4db3-173">Horizontale Skalierung Unterschiede</span><span class="sxs-lookup"><span data-stu-id="c4db3-173">Scaleout differences</span></span>

<span data-ttu-id="c4db3-174">ASP.NET SignalR unterstützt SQL Server und Redis.</span><span class="sxs-lookup"><span data-stu-id="c4db3-174">ASP.NET SignalR supports SQL Server and Redis.</span></span> <span data-ttu-id="c4db3-175">ASP.NET Core SignalR unterstützt Azure SignalR Service und Redis.</span><span class="sxs-lookup"><span data-stu-id="c4db3-175">ASP.NET Core SignalR supports Azure SignalR Service and Redis.</span></span>

### <a name="aspnet"></a><span data-ttu-id="c4db3-176">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c4db3-176">ASP.NET</span></span>

* [<span data-ttu-id="c4db3-177">Horizontale Skalierung in SignalR mit Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="c4db3-177">SignalR scaleout with Azure Service Bus</span></span>](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [<span data-ttu-id="c4db3-178">Horizontale Skalierung in SignalR mit Redis</span><span class="sxs-lookup"><span data-stu-id="c4db3-178">SignalR scaleout with Redis</span></span>](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [<span data-ttu-id="c4db3-179">Horizontale Skalierung in SignalR mit SQL Server</span><span class="sxs-lookup"><span data-stu-id="c4db3-179">SignalR scaleout with SQL Server</span></span>](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a><span data-ttu-id="c4db3-180">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c4db3-180">ASP.NET Core</span></span>

* [<span data-ttu-id="c4db3-181">Der Azure SignalR Service</span><span class="sxs-lookup"><span data-stu-id="c4db3-181">Azure SignalR Service</span></span>](/azure/azure-signalr/)

## <a name="additional-resources"></a><span data-ttu-id="c4db3-182">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="c4db3-182">Additional resources</span></span>

* [<span data-ttu-id="c4db3-183">Hubs</span><span class="sxs-lookup"><span data-stu-id="c4db3-183">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="c4db3-184">JavaScript-Client</span><span class="sxs-lookup"><span data-stu-id="c4db3-184">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="c4db3-185">.NET-Client</span><span class="sxs-lookup"><span data-stu-id="c4db3-185">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="c4db3-186">Unterstützte Plattformen</span><span class="sxs-lookup"><span data-stu-id="c4db3-186">Supported platforms</span></span>](xref:signalr/supported-platforms)
