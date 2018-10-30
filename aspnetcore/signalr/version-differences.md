---
title: Unterschiede zwischen SignalR und ASP.NET Core SignalR
author: tdykstra
description: Unterschiede zwischen SignalR und ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.date: 09/10/2018
uid: signalr/version-differences
ms.openlocfilehash: 3cec37719b743b3c805ada77249f526278e44599
ms.sourcegitcommit: 2ef32676c16f76282f7c23154d13affce8c8bf35
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50234604"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="658d7-103">Unterschiede zwischen SignalR für ASP.NET und ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="658d7-103">Differences between ASP.NET SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="658d7-104">ASP.NET Core SignalR nicht kompatibel mit Clients oder Servern für ASP.NET SignalR.</span><span class="sxs-lookup"><span data-stu-id="658d7-104">ASP.NET Core SignalR isn't compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="658d7-105">In diesem Artikel werden die Funktionen, die in ASP.NET Core SignalR geändert oder entfernt wurden.</span><span class="sxs-lookup"><span data-stu-id="658d7-105">This article details features which have been removed or changed in ASP.NET Core SignalR.</span></span>

## <a name="how-to-identify-the-signalr-version"></a><span data-ttu-id="658d7-106">So identifizieren Sie die SignalR-version</span><span class="sxs-lookup"><span data-stu-id="658d7-106">How to identify the SignalR version</span></span>

|                      | <span data-ttu-id="658d7-107">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="658d7-107">ASP.NET SignalR</span></span> | <span data-ttu-id="658d7-108">ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="658d7-108">ASP.NET Core SignalR</span></span> |
| -------------------- | --------------- | -------------------- |
| <span data-ttu-id="658d7-109">Server-NuGet-Paket</span><span class="sxs-lookup"><span data-stu-id="658d7-109">Server NuGet Package</span></span> | [<span data-ttu-id="658d7-110">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="658d7-110">Microsoft.AspNet.SignalR</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | <span data-ttu-id="658d7-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) ((.NET Core)</span><span class="sxs-lookup"><span data-stu-id="658d7-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span></span><br><span data-ttu-id="658d7-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) ((.NET Framework)</span><span class="sxs-lookup"><span data-stu-id="658d7-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span></span> |
| <span data-ttu-id="658d7-113">Die NuGet-Pakete</span><span class="sxs-lookup"><span data-stu-id="658d7-113">Client NuGet Packages</span></span> | [<span data-ttu-id="658d7-114">Microsoft.AspNet.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="658d7-114">Microsoft.AspNet.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[<span data-ttu-id="658d7-115">Microsoft.AspNet.SignalR.JS</span><span class="sxs-lookup"><span data-stu-id="658d7-115">Microsoft.AspNet.SignalR.JS</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [<span data-ttu-id="658d7-116">Microsoft.AspNetCore.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="658d7-116">Microsoft.AspNetCore.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| <span data-ttu-id="658d7-117">Client-Npm-Pakets</span><span class="sxs-lookup"><span data-stu-id="658d7-117">Client npm Package</span></span> | [<span data-ttu-id="658d7-118">SignalR</span><span class="sxs-lookup"><span data-stu-id="658d7-118">signalr</span></span>](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| <span data-ttu-id="658d7-119">Server App-Typ</span><span class="sxs-lookup"><span data-stu-id="658d7-119">Server App Type</span></span> | <span data-ttu-id="658d7-120">ASP.NET ("System.Web") oder Selbstgehostetem</span><span class="sxs-lookup"><span data-stu-id="658d7-120">ASP.NET (System.Web) or OWIN Self-Host</span></span> | <span data-ttu-id="658d7-121">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="658d7-121">ASP.NET Core</span></span> |
| <span data-ttu-id="658d7-122">Unterstützte Plattformen</span><span class="sxs-lookup"><span data-stu-id="658d7-122">Supported Server Platforms</span></span> | <span data-ttu-id="658d7-123">.NET Framework 4.5 oder höher</span><span class="sxs-lookup"><span data-stu-id="658d7-123">.NET Framework 4.5 or later</span></span> | <span data-ttu-id="658d7-124">.NET Framework 4.6.1 oder höher</span><span class="sxs-lookup"><span data-stu-id="658d7-124">.NET Framework 4.6.1 or later</span></span><br><span data-ttu-id="658d7-125">.NET Core 2.1 oder höher</span><span class="sxs-lookup"><span data-stu-id="658d7-125">.NET Core 2.1 or later</span></span> |

## <a name="feature-differences"></a><span data-ttu-id="658d7-126">Funktionsunterschiede</span><span class="sxs-lookup"><span data-stu-id="658d7-126">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="658d7-127">Automatische Verbindungen</span><span class="sxs-lookup"><span data-stu-id="658d7-127">Automatic reconnects</span></span>

<span data-ttu-id="658d7-128">Automatische Verbindungen werden in ASP.NET Core SignalR nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="658d7-128">Automatic reconnects aren't supported in ASP.NET Core SignalR.</span></span> <span data-ttu-id="658d7-129">Wenn der Client getrennt wird, muss der Benutzer eine neue Verbindung explizit starten, wenn sie erneut eine Verbindung herstellen möchten.</span><span class="sxs-lookup"><span data-stu-id="658d7-129">If the client is disconnected, the user must explicitly start a new connection if they want to reconnect.</span></span> <span data-ttu-id="658d7-130">SignalR versucht, in ASP.NET SignalR zum Server hergestellt werden soll, wenn die Verbindung getrennt wird.</span><span class="sxs-lookup"><span data-stu-id="658d7-130">In ASP.NET SignalR, SignalR attempts to reconnect to the server if the connection is dropped.</span></span> 

### <a name="protocol-support"></a><span data-ttu-id="658d7-131">Unterstützung des Protokolls</span><span class="sxs-lookup"><span data-stu-id="658d7-131">Protocol support</span></span>

<span data-ttu-id="658d7-132">SignalR für ASP.NET Core unterstützt JSON als auch ein neues binäres Protokoll, die basierend auf [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="658d7-132">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="658d7-133">Darüber hinaus können benutzerdefinierte Protokolle erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="658d7-133">Additionally, custom protocols can be created.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="658d7-134">Unterschiede, auf dem server</span><span class="sxs-lookup"><span data-stu-id="658d7-134">Differences on the server</span></span>

<span data-ttu-id="658d7-135">Die ASP.NET Core SignalR serverseitige Bibliotheken befinden sich der [Microsoft.AspNetCore.App metapaket](xref:fundamentals/metapackage-app) Paket, das Teil der **ASP.NET Core-Webanwendung** Vorlage für Razor und MVC Projekte.</span><span class="sxs-lookup"><span data-stu-id="658d7-135">The ASP.NET Core SignalR server-side libraries are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) package that's part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="658d7-136">ASP.NET Core SignalR ist eine ASP.NET Core-Middleware, sodass es durch Aufrufen von konfiguriert werden muss [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="658d7-136">ASP.NET Core SignalR is an ASP.NET Core middleware, so it must be configured by calling [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR()
```

<span data-ttu-id="658d7-137">Zum Konfigurieren des Routings, ordnen Sie Routen für Hubs innerhalb der [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) -Methodenaufruf in der `Startup.Configure` Methode.</span><span class="sxs-lookup"><span data-stu-id="658d7-137">To configure routing, map routes to hubs inside the [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions"></a><span data-ttu-id="658d7-138">Persistente Sitzungen</span><span class="sxs-lookup"><span data-stu-id="658d7-138">Sticky sessions</span></span>

<span data-ttu-id="658d7-139">Das Modell mit horizontaler Skalierung für ASP.NET SignalR kann Clients erneut eine Verbindung herzustellen, und Senden von Nachrichten an einem beliebigen Server in der Farm.</span><span class="sxs-lookup"><span data-stu-id="658d7-139">The scaleout model for ASP.NET SignalR allows clients to reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="658d7-140">In ASP.NET Core SignalR muss der Client für die Dauer der Verbindung mit dem gleichen Server interagieren.</span><span class="sxs-lookup"><span data-stu-id="658d7-140">In ASP.NET Core SignalR, the client must interact with the same server for the duration of the connection.</span></span> <span data-ttu-id="658d7-141">Für die horizontale Skalierung mit Redis bedeutet dies, dass es sich bei persistente Sitzungen erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="658d7-141">For scaleout using Redis, that means sticky sessions are required.</span></span> <span data-ttu-id="658d7-142">Für die Verwendung der Sendewarteschlange für horizontale Skalierung [Azure SignalR Service](/azure/azure-signalr/), persistente Sitzungen sind nicht erforderlich, da der Dienst übernimmt die Verbindungen mit Clients.</span><span class="sxs-lookup"><span data-stu-id="658d7-142">For scaleout using [Azure SignalR Service](/azure/azure-signalr/), sticky sessions are not required because the service handles connections to clients.</span></span> 

### <a name="single-hub-per-connection"></a><span data-ttu-id="658d7-143">Ein Hub pro Verbindung</span><span class="sxs-lookup"><span data-stu-id="658d7-143">Single hub per connection</span></span>

<span data-ttu-id="658d7-144">In ASP.NET Core SignalR wurde das Verbindungsmodell vereinfacht.</span><span class="sxs-lookup"><span data-stu-id="658d7-144">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="658d7-145">Verbindungen werden direkt an eine einzelne Verbindung, die Zugriff auf mehrere Hubs freigeben verwendet wird, anstatt einen einzelnen Hub hergestellt werden.</span><span class="sxs-lookup"><span data-stu-id="658d7-145">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="658d7-146">Streaming</span><span class="sxs-lookup"><span data-stu-id="658d7-146">Streaming</span></span>

<span data-ttu-id="658d7-147">Unterstützt nun das ASP.NET Core SignalR [Streamingdaten](xref:signalr/streaming) aus dem Hub an den Client.</span><span class="sxs-lookup"><span data-stu-id="658d7-147">ASP.NET Core SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="658d7-148">Zustand</span><span class="sxs-lookup"><span data-stu-id="658d7-148">State</span></span>

<span data-ttu-id="658d7-149">Die Möglichkeit, die oft ausgegebene Befehlszeilen Zustand zwischen Clients und dem Hub (oft als HubState bezeichnet) übergeben wurde, sowie Unterstützung für Meldungen zum Druckfortschritt aufgehoben.</span><span class="sxs-lookup"><span data-stu-id="658d7-149">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="658d7-150">Es gibt keine Entsprechung der Hub-Proxys im Moment ein.</span><span class="sxs-lookup"><span data-stu-id="658d7-150">There is no counterpart of hub proxies at the moment.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="658d7-151">Unterschiede, auf dem client</span><span class="sxs-lookup"><span data-stu-id="658d7-151">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="658d7-152">TypeScript</span><span class="sxs-lookup"><span data-stu-id="658d7-152">TypeScript</span></span>

<span data-ttu-id="658d7-153">Der ASP.NET Core SignalR-Client hat [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="658d7-153">The ASP.NET Core SignalR client is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="658d7-154">Sie können in JavaScript oder TypeScript schreiben, bei Verwendung der [JavaScript-Client](xref:signalr/javascript-client).</span><span class="sxs-lookup"><span data-stu-id="658d7-154">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="658d7-155">Der JavaScript-Client befindet sich am [Npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="658d7-155">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="658d7-156">In früheren Versionen wurde der JavaScript-Client über NuGet-Paket in Visual Studio abgerufen.</span><span class="sxs-lookup"><span data-stu-id="658d7-156">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="658d7-157">Für die Core-Versionen die [ @aspnet/signalr ](https://www.npmjs.com/package/@aspnet/signalr) Npm-Paket enthält die JavaScript-Bibliotheken.</span><span class="sxs-lookup"><span data-stu-id="658d7-157">For the Core versions, the [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) npm package contains the JavaScript libraries.</span></span> <span data-ttu-id="658d7-158">Dieses Paket nicht enthalten, der **ASP.NET Core-Webanwendung** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="658d7-158">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="658d7-159">Mithilfe von Npm abrufen und Installieren der `@aspnet/signalr` Npm-Paket.</span><span class="sxs-lookup"><span data-stu-id="658d7-159">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="658d7-160">jQuery</span><span class="sxs-lookup"><span data-stu-id="658d7-160">jQuery</span></span>

<span data-ttu-id="658d7-161">Die Abhängigkeit von jQuery wurde entfernt, aber Projekte jQuery weiterhin verwenden können.</span><span class="sxs-lookup"><span data-stu-id="658d7-161">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="658d7-162">JavaScript-Client-Methodensyntax</span><span class="sxs-lookup"><span data-stu-id="658d7-162">JavaScript client method syntax</span></span>

<span data-ttu-id="658d7-163">Die JavaScript-Syntax wurde aus der vorherigen Version von SignalR geändert.</span><span class="sxs-lookup"><span data-stu-id="658d7-163">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="658d7-164">Anstatt die `$connection` Objekt, erstellen Sie eine Verbindung mit der [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span><span class="sxs-lookup"><span data-stu-id="658d7-164">Rather than using the `$connection` object, create a connection using the [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="658d7-165">Verwenden der [auf](/javascript/api/@aspnet/signalr/HubConnection#on) Methode, um die Clientmethoden angeben, die der Hub aufgerufen werden kann.</span><span class="sxs-lookup"><span data-stu-id="658d7-165">Use the [on](/javascript/api/@aspnet/signalr/HubConnection#on) method to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="658d7-166">Starten Sie nach dem Erstellen der-Methode, die Hub-Verbindung ein.</span><span class="sxs-lookup"><span data-stu-id="658d7-166">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="658d7-167">Kette eine [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) Methode zum Anmelden oder Fehler zu beheben.</span><span class="sxs-lookup"><span data-stu-id="658d7-167">Chain a [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="658d7-168">Hub-Proxys</span><span class="sxs-lookup"><span data-stu-id="658d7-168">Hub proxies</span></span>

<span data-ttu-id="658d7-169">Hub-Proxys werden nicht mehr automatisch generiert.</span><span class="sxs-lookup"><span data-stu-id="658d7-169">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="658d7-170">Stattdessen wird der Name der Methode übergeben, in der [Aufrufen](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API als Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="658d7-170">Instead, the method name is passed into the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="658d7-171">.NET und andere clients</span><span class="sxs-lookup"><span data-stu-id="658d7-171">.NET and other clients</span></span>

<span data-ttu-id="658d7-172">Die `Microsoft.AspNetCore.SignalR.Client` NuGet-Paket enthält die .NET Client-Bibliotheken für ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="658d7-172">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="658d7-173">Verwenden der [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) erstellen und entwickeln eine Instanz einer Verbindung mit einem Hub.</span><span class="sxs-lookup"><span data-stu-id="658d7-173">Use the [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a><span data-ttu-id="658d7-174">Horizontale Skalierung Unterschiede</span><span class="sxs-lookup"><span data-stu-id="658d7-174">Scaleout differences</span></span>

<span data-ttu-id="658d7-175">ASP.NET SignalR unterstützt SQL Server und Redis.</span><span class="sxs-lookup"><span data-stu-id="658d7-175">ASP.NET SignalR supports SQL Server and Redis.</span></span> <span data-ttu-id="658d7-176">ASP.NET Core SignalR unterstützt Azure SignalR Service und Redis.</span><span class="sxs-lookup"><span data-stu-id="658d7-176">ASP.NET Core SignalR supports Azure SignalR Service and Redis.</span></span>

### <a name="aspnet"></a><span data-ttu-id="658d7-177">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="658d7-177">ASP.NET</span></span>

* [<span data-ttu-id="658d7-178">Horizontale Skalierung in SignalR mit Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="658d7-178">SignalR scaleout with Azure Service Bus</span></span>](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [<span data-ttu-id="658d7-179">Horizontale Skalierung in SignalR mit Redis</span><span class="sxs-lookup"><span data-stu-id="658d7-179">SignalR scaleout with Redis</span></span>](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [<span data-ttu-id="658d7-180">Horizontale Skalierung in SignalR mit SQL Server</span><span class="sxs-lookup"><span data-stu-id="658d7-180">SignalR scaleout with SQL Server</span></span>](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a><span data-ttu-id="658d7-181">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="658d7-181">ASP.NET Core</span></span>

* [<span data-ttu-id="658d7-182">Der Azure SignalR Service</span><span class="sxs-lookup"><span data-stu-id="658d7-182">Azure SignalR Service</span></span>](/azure/azure-signalr/)

## <a name="additional-resources"></a><span data-ttu-id="658d7-183">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="658d7-183">Additional resources</span></span>

* [<span data-ttu-id="658d7-184">Hubs</span><span class="sxs-lookup"><span data-stu-id="658d7-184">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="658d7-185">JavaScript-Client</span><span class="sxs-lookup"><span data-stu-id="658d7-185">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="658d7-186">.NET-Client</span><span class="sxs-lookup"><span data-stu-id="658d7-186">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="658d7-187">Unterstützte Plattformen</span><span class="sxs-lookup"><span data-stu-id="658d7-187">Supported platforms</span></span>](xref:signalr/supported-platforms)
