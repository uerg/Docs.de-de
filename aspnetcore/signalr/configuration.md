---
title: ASP.NET Core SignalR-Konfiguration
author: tdykstra
description: Erfahren Sie, wie Sie ASP.NET Core SignalR-apps konfigurieren.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/30/2018
uid: signalr/configuration
ms.openlocfilehash: fac0226c939f4cf446c876b1c0b359d6c5b9dfd3
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095401"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="01b6e-103">ASP.NET Core SignalR-Konfiguration</span><span class="sxs-lookup"><span data-stu-id="01b6e-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="01b6e-104">JSON/MessagePack Serialisierungsoptionen</span><span class="sxs-lookup"><span data-stu-id="01b6e-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="01b6e-105">ASP.NET Core SignalR unterstützt zwei Protokolle für die Codierung von Nachrichten: [JSON](https://www.json.org/) und [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="01b6e-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="01b6e-106">Jedes Protokoll verfügt über Konfigurationsoptionen für die Serialisierung.</span><span class="sxs-lookup"><span data-stu-id="01b6e-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="01b6e-107">JSON-Serialisierung kann konfiguriert werden, auf dem Server mithilfe der [ `AddJsonProtocol` ](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) Erweiterungsmethode, die nach dem hinzuzufügenden [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in Ihre `Startup.ConfigureServices` Methode.</span><span class="sxs-lookup"><span data-stu-id="01b6e-107">JSON serialization can be configured on the server using the [`AddJsonProtocol`](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="01b6e-108">Die `AddJsonProtocol` Methode verwendet einen Delegaten, der empfängt eine `options` Objekt.</span><span class="sxs-lookup"><span data-stu-id="01b6e-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="01b6e-109">Die [ `PayloadSerializerSettings` ](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) -Eigenschaft für dieses Objekt ist ein JSON.NET `JsonSerializerSettings` -Objekt, das zum Konfigurieren der Serialisierung der Argumente und Rückgabewerte verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="01b6e-109">The [`PayloadSerializerSettings`](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="01b6e-110">Finden Sie unter den [JSON.NET-Dokumentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) Weitere Details.</span><span class="sxs-lookup"><span data-stu-id="01b6e-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="01b6e-111">Verwenden Sie beispielsweise zum Konfigurieren der Serialisierer, verwenden Sie "PascalCase" Eigenschaftennamen, anstelle der Standardnamen "CamelCase" den folgenden Code ein:</span><span class="sxs-lookup"><span data-stu-id="01b6e-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonHubProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

<span data-ttu-id="01b6e-112">In der .NET Client `AddJsonHubProtocol` Erweiterungsmethode vorhanden ist, auf [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="01b6e-112">In the .NET client, the same `AddJsonHubProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="01b6e-113">Die `Microsoft.Extensions.DependencyInjection` Namespace muss importiert werden, um die Erweiterungsmethode zu beheben:</span><span class="sxs-lookup"><span data-stu-id="01b6e-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
.AddJsonHubProtocol(options => {
    options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
});
```

> [!NOTE]
> <span data-ttu-id="01b6e-114">Es ist nicht möglich, das JSON-Serialisierung in der JavaScript-Client zu diesem Zeitpunkt zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="01b6e-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="01b6e-115">MessagePack Serialisierungsoptionen</span><span class="sxs-lookup"><span data-stu-id="01b6e-115">MessagePack serialization options</span></span>

<span data-ttu-id="01b6e-116">MessagePack Serialisierung kann konfiguriert werden, indem ein Delegat, der die [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) aufrufen.</span><span class="sxs-lookup"><span data-stu-id="01b6e-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="01b6e-117">Finden Sie unter [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) Weitere Details.</span><span class="sxs-lookup"><span data-stu-id="01b6e-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="01b6e-118">Es ist nicht möglich, MessagePack Serialisierung zu diesem Zeitpunkt im JavaScript-Client zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="01b6e-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="01b6e-119">Konfigurieren Sie Serveroptionen</span><span class="sxs-lookup"><span data-stu-id="01b6e-119">Configure server options</span></span>

<span data-ttu-id="01b6e-120">Die folgende Tabelle beschreibt die Optionen zum Konfigurieren von SignalR-Hubs:</span><span class="sxs-lookup"><span data-stu-id="01b6e-120">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="01b6e-121">Option</span><span class="sxs-lookup"><span data-stu-id="01b6e-121">Option</span></span> | <span data-ttu-id="01b6e-122">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="01b6e-122">Description</span></span> |
| ------ | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="01b6e-123">Wenn der Client innerhalb dieses Zeitraums eine anfängliche Handshake-Nachricht senden, nicht, wird die Verbindung geschlossen.</span><span class="sxs-lookup"><span data-stu-id="01b6e-123">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="01b6e-124">Wenn der Server eine Nachricht nicht innerhalb dieses Intervalls gesendet noch nicht, wird eine Ping-Nachricht automatisch gesendet, die Verbindung geöffnet bleiben.</span><span class="sxs-lookup"><span data-stu-id="01b6e-124">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> |
| `SupportedProtocols` | <span data-ttu-id="01b6e-125">Von diesen Hub unterstützten Protokolle.</span><span class="sxs-lookup"><span data-stu-id="01b6e-125">Protocols supported by this hub.</span></span> <span data-ttu-id="01b6e-126">Standardmäßig alle Protokolle, die auf dem Server registriert sind zulässig, aber Protokolle können aus dieser Liste Deaktivieren bestimmter Protokolle für individuelle Hubs entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="01b6e-126">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | <span data-ttu-id="01b6e-127">Wenn `true`, detaillierte Ausnahme Nachrichten werden an Clients zurückgegeben, wenn in einer hubmethode eine Ausnahme ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="01b6e-127">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="01b6e-128">Der Standardwert ist `false`, wie diese ausnahmemeldungen können vertraulichen Informationen enthalten.</span><span class="sxs-lookup"><span data-stu-id="01b6e-128">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="01b6e-129">Optionen für alle Hubs konfiguriert werden können, durch die Bereitstellung einer Optionen-Delegat, der die `AddSignalR` Aufrufen in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="01b6e-129">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSignalR(hubOptions =>
    {
        hubOptions.EnableDetailedErrors = true;
        hubOptions.KeepAliveInterval = TimeSpan.FromMinutes(1);
    })
}
```

<span data-ttu-id="01b6e-130">Optionen für einen einzelnen Hub überschreiben die globale Optionen im `AddSignalR` und kann konfiguriert werden, mithilfe von [ `AddHubOptions<T>` ](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span><span class="sxs-lookup"><span data-stu-id="01b6e-130">Options for a single hub override the global options provided in `AddSignalR` and can be configured using [`AddHubOptions<T>`](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
}
```

<span data-ttu-id="01b6e-131">Verwendung `HttpConnectionDispatcherOptions` so konfigurieren Sie erweiterte Einstellungen, die im Zusammenhang mit Transporte und Speicherverwaltung für Puffer.</span><span class="sxs-lookup"><span data-stu-id="01b6e-131">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="01b6e-132">Diese Optionen werden durch Übergeben eines Delegaten, konfiguriert [ `MapHub<T>` ](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span><span class="sxs-lookup"><span data-stu-id="01b6e-132">These options are configured by passing a delegate to [`MapHub<T>`](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span></span>

| <span data-ttu-id="01b6e-133">Option</span><span class="sxs-lookup"><span data-stu-id="01b6e-133">Option</span></span> | <span data-ttu-id="01b6e-134">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="01b6e-134">Description</span></span> |
| ------ | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="01b6e-135">Die maximale Anzahl von Bytes, die vom Client empfangen, die die Server-Puffer.</span><span class="sxs-lookup"><span data-stu-id="01b6e-135">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="01b6e-136">Durch Erhöhen dieses Wertes können den Server, größere Nachrichten zu empfangen, aber arbeitsspeichernutzung negativ beeinträchtigt werden kann.</span><span class="sxs-lookup"><span data-stu-id="01b6e-136">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> <span data-ttu-id="01b6e-137">Der Standardwert beträgt 32KB.</span><span class="sxs-lookup"><span data-stu-id="01b6e-137">The default value is 32KB.</span></span> |
| `AuthorizationData` | <span data-ttu-id="01b6e-138">Eine Liste der [ `IAuthorizeData` ](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) Objekte verwendet, um zu bestimmen, ob ein Client autorisiert ist, eine Verbindung mit dem Hub herstellen.</span><span class="sxs-lookup"><span data-stu-id="01b6e-138">A list of [`IAuthorizeData`](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> <span data-ttu-id="01b6e-139">Standardmäßig wird aufgefüllt durch Werte aus der `Authorize` Attribute, die auf die hubklasse angewendet.</span><span class="sxs-lookup"><span data-stu-id="01b6e-139">By default, this is populated with values from the `Authorize` attributes applied to the Hub class.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="01b6e-140">Die maximale Anzahl von Bytes, die von der app gesendet werden, die die Server-Puffer.</span><span class="sxs-lookup"><span data-stu-id="01b6e-140">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="01b6e-141">Durch Erhöhen dieses Wertes können den Server aus, um größere Nachrichten zu senden, aber arbeitsspeichernutzung negativ beeinträchtigt werden kann.</span><span class="sxs-lookup"><span data-stu-id="01b6e-141">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> <span data-ttu-id="01b6e-142">Der Standardwert beträgt 32KB.</span><span class="sxs-lookup"><span data-stu-id="01b6e-142">The default value is 32KB.</span></span> |
| `Transports` | <span data-ttu-id="01b6e-143">Eine Bitmaske der `HttpTransportType` Werte, die die Transporte einschränken, können ein Client kann die Verbindung verwenden.</span><span class="sxs-lookup"><span data-stu-id="01b6e-143">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> <span data-ttu-id="01b6e-144">Alle Transporte sind standardmäßig aktiviert.</span><span class="sxs-lookup"><span data-stu-id="01b6e-144">All transports are enabled by default.</span></span> |
| `LongPolling` | <span data-ttu-id="01b6e-145">Zusätzliche Optionen, die speziell für den Transport Long Polling.</span><span class="sxs-lookup"><span data-stu-id="01b6e-145">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="01b6e-146">Zusätzliche Optionen, die speziell für die WebSockets-Übertragung.</span><span class="sxs-lookup"><span data-stu-id="01b6e-146">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="01b6e-147">Der Transport Long Polling verfügt über zusätzliche Optionen, die mit konfiguriert werden können die `LongPolling` Eigenschaft:</span><span class="sxs-lookup"><span data-stu-id="01b6e-147">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="01b6e-148">Option</span><span class="sxs-lookup"><span data-stu-id="01b6e-148">Option</span></span> | <span data-ttu-id="01b6e-149">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="01b6e-149">Description</span></span> |
| ------ | ----------- |
| `PollTimeout` | <span data-ttu-id="01b6e-150">Die maximale Zeitspanne wartet der Server eine Nachricht an den Client gesendet werden soll, bevor eine Anforderung für die einzelnen Abfrage beendet.</span><span class="sxs-lookup"><span data-stu-id="01b6e-150">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="01b6e-151">Durch senken dieses Werts bewirkt, dass den Client neue abrufanforderungen häufiger ausgeben.</span><span class="sxs-lookup"><span data-stu-id="01b6e-151">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> <span data-ttu-id="01b6e-152">Der Standardwert ist 90 Sekunden.</span><span class="sxs-lookup"><span data-stu-id="01b6e-152">The default value is 90 seconds.</span></span> |

<span data-ttu-id="01b6e-153">Der WebSocket-Transport verfügt über zusätzliche Optionen, die mit konfiguriert werden können die `WebSockets` Eigenschaft:</span><span class="sxs-lookup"><span data-stu-id="01b6e-153">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="01b6e-154">Option</span><span class="sxs-lookup"><span data-stu-id="01b6e-154">Option</span></span> | <span data-ttu-id="01b6e-155">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="01b6e-155">Description</span></span> |
| ------ | ----------- |
| `CloseTimeout` | <span data-ttu-id="01b6e-156">Nachdem der Server wird geschlossen, wenn der Client nicht innerhalb dieses Zeitraums zu schließen, wird die Verbindung beendet.</span><span class="sxs-lookup"><span data-stu-id="01b6e-156">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | <span data-ttu-id="01b6e-157">Ein Delegat, der verwendet werden kann, Festlegen der `Sec-WebSocket-Protocol` -Header auf einen benutzerdefinierten Wert.</span><span class="sxs-lookup"><span data-stu-id="01b6e-157">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="01b6e-158">Der Delegat empfängt die Werte, die vom Client als Eingabe angefordert werden, und es wird erwartet, um den gewünschten Wert zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="01b6e-158">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="01b6e-159">Konfigurieren Sie Clientoptionen</span><span class="sxs-lookup"><span data-stu-id="01b6e-159">Configure client options</span></span>

<span data-ttu-id="01b6e-160">Clientoptionen können konfiguriert werden, auf die `HubConnectionBuilder` Typ (in .NET- und JavaScript-Clients verfügbar gemacht wird), als auch auf die `HubConnection` selbst.</span><span class="sxs-lookup"><span data-stu-id="01b6e-160">Client options can be configured on the `HubConnectionBuilder` type (available in both .NET and JavaScript clients), as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="01b6e-161">Konfigurieren der Protokollierung</span><span class="sxs-lookup"><span data-stu-id="01b6e-161">Configure logging</span></span>

<span data-ttu-id="01b6e-162">Protokollierung wurde so konfiguriert, der .NET Client die `ConfigureLogging` Methode.</span><span class="sxs-lookup"><span data-stu-id="01b6e-162">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="01b6e-163">Protokollierung von Anbietern und Filter kann auf die gleiche Weise registriert werden, wie sie auf dem Server sind.</span><span class="sxs-lookup"><span data-stu-id="01b6e-163">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="01b6e-164">Finden Sie unter den [Protokollierung in ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) Dokumentation zu informieren.</span><span class="sxs-lookup"><span data-stu-id="01b6e-164">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="01b6e-165">Um Anbieter für die Protokollierung zu registrieren, müssen Sie die erforderlichen Pakete installieren.</span><span class="sxs-lookup"><span data-stu-id="01b6e-165">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="01b6e-166">Finden Sie unter den [integrierte Protokollierungsanbieter](xref:fundamentals/logging/index#built-in-logging-providers) Abschnitt der Dokumentation für eine vollständige Liste.</span><span class="sxs-lookup"><span data-stu-id="01b6e-166">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="01b6e-167">Beispielsweise um konsolenprotokollierung zu aktivieren, installieren die `Microsoft.Extensions.Logging.Console` NuGet-Paket.</span><span class="sxs-lookup"><span data-stu-id="01b6e-167">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="01b6e-168">Rufen Sie die `AddConsole` Erweiterungsmethode:</span><span class="sxs-lookup"><span data-stu-id="01b6e-168">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="01b6e-169">Im JavaScript-Client, eine ähnliche `configureLogging` Methode vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="01b6e-169">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="01b6e-170">Geben Sie einen `LogLevel` Wert, der angibt, der Mindestebene von protokollmeldungen zu erzeugen.</span><span class="sxs-lookup"><span data-stu-id="01b6e-170">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="01b6e-171">Protokolle werden in das Browserfenster für die Konsole geschrieben.</span><span class="sxs-lookup"><span data-stu-id="01b6e-171">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="01b6e-172">Geben Sie zum Deaktivieren der Protokollierung vollständig, `signalR.LogLevel.None` in die `configureLogging` Methode.</span><span class="sxs-lookup"><span data-stu-id="01b6e-172">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="01b6e-173">Für den JavaScript-Client verfügbaren Protokollebenen, sind unten aufgeführt.</span><span class="sxs-lookup"><span data-stu-id="01b6e-173">Log levels available to the JavaScript client are listed below.</span></span> <span data-ttu-id="01b6e-174">Die Protokollebene auf einen der folgenden Werte festlegen aktiviert die Protokollierung von Nachrichten an **oder höher** dieser Ebene.</span><span class="sxs-lookup"><span data-stu-id="01b6e-174">Setting the log level to one of these values enables logging of messages at **or above** that level.</span></span>

| <span data-ttu-id="01b6e-175">Ebene</span><span class="sxs-lookup"><span data-stu-id="01b6e-175">Level</span></span> | <span data-ttu-id="01b6e-176">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="01b6e-176">Description</span></span> |
| ----- | ----------- |
| `None` | <span data-ttu-id="01b6e-177">Es werden keine Meldungen protokolliert.</span><span class="sxs-lookup"><span data-stu-id="01b6e-177">No messages are logged.</span></span> |
| `Critical` | <span data-ttu-id="01b6e-178">Nachrichten, die einen Fehler in der gesamten app angeben.</span><span class="sxs-lookup"><span data-stu-id="01b6e-178">Messages that indicate a failure in the entire app.</span></span> |
| `Error` | <span data-ttu-id="01b6e-179">Nachrichten, die einen Fehler in den aktuellen Vorgang angeben.</span><span class="sxs-lookup"><span data-stu-id="01b6e-179">Messages that indicate a failure in the current operation.</span></span> |
| `Warning` | <span data-ttu-id="01b6e-180">Nachrichten, die ein nicht schwerwiegender Fehler aufgetreten.</span><span class="sxs-lookup"><span data-stu-id="01b6e-180">Messages that indicate a non-fatal problem.</span></span> |
| `Information` | <span data-ttu-id="01b6e-181">Informationsmeldungen.</span><span class="sxs-lookup"><span data-stu-id="01b6e-181">Informational messages.</span></span> |
| `Debug` | <span data-ttu-id="01b6e-182">Diagnosemeldungen für das Debuggen hilfreich.</span><span class="sxs-lookup"><span data-stu-id="01b6e-182">Diagnostic messages useful for debugging.</span></span> |
| `Trace` | <span data-ttu-id="01b6e-183">Sehr detaillierte diagnosemeldungen zum Diagnostizieren von Problemen mit bestimmten entwickelt.</span><span class="sxs-lookup"><span data-stu-id="01b6e-183">Very detailed diagnostic messages designed for diagnosing specific issues.</span></span> |

### <a name="configure-allowed-transports"></a><span data-ttu-id="01b6e-184">Konfigurieren von zulässigen Transporte</span><span class="sxs-lookup"><span data-stu-id="01b6e-184">Configure allowed transports</span></span>

<span data-ttu-id="01b6e-185">Die von SignalR verwendete Transporte können konfiguriert werden, der `WithUrl` aufrufen (`withUrl` in JavaScript).</span><span class="sxs-lookup"><span data-stu-id="01b6e-185">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="01b6e-186">Eine bitweise OR-Operation der Werte von `HttpTransportType` kann verwendet werden, um den Client nur die angegebenen Transporte verwenden zu beschränken.</span><span class="sxs-lookup"><span data-stu-id="01b6e-186">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="01b6e-187">Alle Transporte sind standardmäßig aktiviert.</span><span class="sxs-lookup"><span data-stu-id="01b6e-187">All transports are enabled by default.</span></span>

<span data-ttu-id="01b6e-188">Zulassen Sie z. B. um den Transport Server-Sent-Ereignisse zu deaktivieren, aber WebSockets oder Long Polling Verbindungen:</span><span class="sxs-lookup"><span data-stu-id="01b6e-188">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="01b6e-189">In JavaScript-Client-Transporte werden konfiguriert, durch Festlegen der `transport` Feld für das Options-Objekt, das bereitgestellt, um `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="01b6e-189">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="01b6e-190">Konfigurieren von Bearer-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="01b6e-190">Configure bearer authentication</span></span>

<span data-ttu-id="01b6e-191">Um Authentifizierungsdaten zusammen mit Anforderungen für SignalR bereitzustellen, verwenden Sie die `AccessTokenProvider` Option (`accessTokenFactory` in JavaScript) an eine Funktion, die den gewünschten Zugangs-Token zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="01b6e-191">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="01b6e-192">In der .NET Client dieses Zugriffstoken wird als übergeben, eine HTTP-"Bearer-Authentifizierung" token (mithilfe der `Authorization` Header vom Typ `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="01b6e-192">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="01b6e-193">In JavaScript-Client wird das Zugriffstoken als bearertoken, verwendet **außer** in einigen Fällen, in dem Browser APIs beschränken die Möglichkeit, anzuwenden Header (insbesondere im Server-Sent-Ereignisse und WebSockets-Anforderungen).</span><span class="sxs-lookup"><span data-stu-id="01b6e-193">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="01b6e-194">In diesen Fällen wird das Zugriffstoken als einen Abfragezeichenfolgenwert bereitgestellt `access_token`.</span><span class="sxs-lookup"><span data-stu-id="01b6e-194">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="01b6e-195">In der .NET Client der `AccessTokenProvider` Option kann angegeben werden, mithilfe der Optionen Delegaten in `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="01b6e-195">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        }
    })
    .Build();
```

<span data-ttu-id="01b6e-196">In JavaScript-Client das Zugriffstoken ist konfiguriert, indem die `accessTokenFactory` Feld das Optionsobjekt in `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="01b6e-196">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        accessTokenFactory: () => {
            // Get and return the access token.
            // This function can return a JavaScript Promise if asynchronous
            // logic is required to retrieve the access token.
        }
    })
    .build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="01b6e-197">Timeout und Keep-alive-Optionen konfigurieren</span><span class="sxs-lookup"><span data-stu-id="01b6e-197">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="01b6e-198">Zusätzliche Optionen zum Konfigurieren von Timeouts und Keep-alive-Verhalten befinden sich die `HubConnection` Objekt selbst:</span><span class="sxs-lookup"><span data-stu-id="01b6e-198">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

| <span data-ttu-id="01b6e-199">Option für .NET</span><span class="sxs-lookup"><span data-stu-id="01b6e-199">.NET Option</span></span> | <span data-ttu-id="01b6e-200">JavaScript-Option</span><span class="sxs-lookup"><span data-stu-id="01b6e-200">JavaScript Option</span></span> | <span data-ttu-id="01b6e-201">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="01b6e-201">Description</span></span> |
| ----------- | ----------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | <span data-ttu-id="01b6e-202">Timeout für die Serveraktivität.</span><span class="sxs-lookup"><span data-stu-id="01b6e-202">Timeout for server activity.</span></span> <span data-ttu-id="01b6e-203">Wenn der Server alle Nachrichten in einem bestimmten Intervall gesendet noch nicht, der Client betrachtet, der Server getrennt und Auslösen der `Closed` Ereignis (`onclose` in JavaScript).</span><span class="sxs-lookup"><span data-stu-id="01b6e-203">If the server hasn't sent any message in this interval, the client considers the server disconnected and trigger the `Closed` event (`onclose` in JavaScript).</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="01b6e-204">Kann nicht konfiguriert</span><span class="sxs-lookup"><span data-stu-id="01b6e-204">Not configurable</span></span> | <span data-ttu-id="01b6e-205">Timeout für die erstmalige Server-Handshake.</span><span class="sxs-lookup"><span data-stu-id="01b6e-205">Timeout for initial server handshake.</span></span> <span data-ttu-id="01b6e-206">Wenn der Server eine Antwort Handshake in einem bestimmten Intervall nicht sendet, wird vom Client abgebrochen, der Handshake und Auslösen der `Closed` Ereignis (`onclose` in JavaScript).</span><span class="sxs-lookup"><span data-stu-id="01b6e-206">If the server doesn't send a handshake response in this interval, the client cancels the handshake and trigger the `Closed` event (`onclose` in JavaScript).</span></span> |

<span data-ttu-id="01b6e-207">Im .NET Client Timeoutwerte angegeben sind, als `TimeSpan` Werte.</span><span class="sxs-lookup"><span data-stu-id="01b6e-207">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span> <span data-ttu-id="01b6e-208">In JavaScript-Client werden die Timeoutwerte als Zahlen angegeben.</span><span class="sxs-lookup"><span data-stu-id="01b6e-208">In the JavaScript client, timeout values are specified as numbers.</span></span> <span data-ttu-id="01b6e-209">Die Zahlen stellen die Zeitwerte in Millisekunden dar.</span><span class="sxs-lookup"><span data-stu-id="01b6e-209">The numbers represent time values in milliseconds.</span></span>

### <a name="configure-additional-options"></a><span data-ttu-id="01b6e-210">Konfigurieren zusätzlicher Optionen</span><span class="sxs-lookup"><span data-stu-id="01b6e-210">Configure additional options</span></span>

<span data-ttu-id="01b6e-211">Zusätzliche Optionen können konfiguriert werden, der `WithUrl` (`withUrl` in JavaScript)-Methode `HubConnectionBuilder`:</span><span class="sxs-lookup"><span data-stu-id="01b6e-211">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder`:</span></span>

| <span data-ttu-id="01b6e-212">Option für .NET</span><span class="sxs-lookup"><span data-stu-id="01b6e-212">.NET Option</span></span> | <span data-ttu-id="01b6e-213">JavaScript-Option</span><span class="sxs-lookup"><span data-stu-id="01b6e-213">JavaScript Option</span></span> | <span data-ttu-id="01b6e-214">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="01b6e-214">Description</span></span> |
| ----------- | ----------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | <span data-ttu-id="01b6e-215">Eine Funktion zurückgeben einer Zeichenfolge, die als bearertoken in HTTP-Anforderungen Authentifizierung bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="01b6e-215">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `skipNegotiation` | <span data-ttu-id="01b6e-216">Legen Sie diesen `true` um die Aushandlung Schritt zu überspringen.</span><span class="sxs-lookup"><span data-stu-id="01b6e-216">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="01b6e-217">**Nur unterstützt, wenn die WebSockets-Übertragung der einzige aktiviert-Transport ist**.</span><span class="sxs-lookup"><span data-stu-id="01b6e-217">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="01b6e-218">Diese Einstellung kann nicht aktiviert werden, wenn mithilfe des Azure SignalR Service.</span><span class="sxs-lookup"><span data-stu-id="01b6e-218">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="01b6e-219">Nicht konfigurierbar \*</span><span class="sxs-lookup"><span data-stu-id="01b6e-219">Not configurable \*</span></span> | <span data-ttu-id="01b6e-220">Eine Auflistung von TLS-Zertifikate zum Authentifizieren von Anforderungen senden.</span><span class="sxs-lookup"><span data-stu-id="01b6e-220">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="01b6e-221">Nicht konfigurierbar \*</span><span class="sxs-lookup"><span data-stu-id="01b6e-221">Not configurable \*</span></span> | <span data-ttu-id="01b6e-222">Eine Auflistung von HTTP-Cookies, die mit jeder HTTP-Anforderung gesendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="01b6e-222">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="01b6e-223">Nicht konfigurierbar \*</span><span class="sxs-lookup"><span data-stu-id="01b6e-223">Not configurable \*</span></span> | <span data-ttu-id="01b6e-224">Die Anmeldeinformationen mit jeder HTTP-Anforderung senden.</span><span class="sxs-lookup"><span data-stu-id="01b6e-224">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="01b6e-225">Nicht konfigurierbar \*</span><span class="sxs-lookup"><span data-stu-id="01b6e-225">Not configurable \*</span></span> | <span data-ttu-id="01b6e-226">Nur WebSockets.</span><span class="sxs-lookup"><span data-stu-id="01b6e-226">WebSockets only.</span></span> <span data-ttu-id="01b6e-227">Die maximale Zeitspanne wartet der Client nach dem Schließen für den Server die Anforderung schließende bestätigt.</span><span class="sxs-lookup"><span data-stu-id="01b6e-227">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="01b6e-228">Wenn der Server das Schließen nicht innerhalb dieses Zeitraums bestätigt nicht, trennt den Client.</span><span class="sxs-lookup"><span data-stu-id="01b6e-228">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="01b6e-229">Nicht konfigurierbar \*</span><span class="sxs-lookup"><span data-stu-id="01b6e-229">Not configurable \*</span></span> | <span data-ttu-id="01b6e-230">Ein Wörterbuch von zusätzlichen HTTP-Header mit jeder HTTP-Anforderung senden.</span><span class="sxs-lookup"><span data-stu-id="01b6e-230">A dictionary of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | <span data-ttu-id="01b6e-231">Nicht konfigurierbar \*</span><span class="sxs-lookup"><span data-stu-id="01b6e-231">Not configurable \*</span></span> | <span data-ttu-id="01b6e-232">Ein Delegat, der verwendet werden kann, um zu konfigurieren, oder Ersetzen Sie die `HttpMessageHandler` zum Senden von HTTP-Anforderungen verwendet.</span><span class="sxs-lookup"><span data-stu-id="01b6e-232">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="01b6e-233">WebSocket-Verbindungen verwendet nicht.</span><span class="sxs-lookup"><span data-stu-id="01b6e-233">Not used for WebSocket connections.</span></span> <span data-ttu-id="01b6e-234">Dieser Delegat muss einen Wert ungleich Null zurückgeben, und den Standardwert als Parameter erhält.</span><span class="sxs-lookup"><span data-stu-id="01b6e-234">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="01b6e-235">Ändern Sie die Einstellungen auf dieser Standardwert und zurückgeben oder ein komplett neues zurückgeben `HttpMessageHandler` Instanz.</span><span class="sxs-lookup"><span data-stu-id="01b6e-235">Either modify settings on that default value and return it, or return a completely new `HttpMessageHandler` instance.</span></span> |
| `Proxy` | <span data-ttu-id="01b6e-236">Nicht konfigurierbar \*</span><span class="sxs-lookup"><span data-stu-id="01b6e-236">Not configurable \*</span></span> | <span data-ttu-id="01b6e-237">Ein HTTP-Proxy beim Senden von HTTP-Anforderungen verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="01b6e-237">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | <span data-ttu-id="01b6e-238">Nicht konfigurierbar \*</span><span class="sxs-lookup"><span data-stu-id="01b6e-238">Not configurable \*</span></span> | <span data-ttu-id="01b6e-239">Legen Sie diese boolescher Wert, um die Standardanmeldeinformationen für HTTP und WebSockets-Anforderungen zu senden.</span><span class="sxs-lookup"><span data-stu-id="01b6e-239">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="01b6e-240">Dadurch wird die Verwendung der Windows-Authentifizierung.</span><span class="sxs-lookup"><span data-stu-id="01b6e-240">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | <span data-ttu-id="01b6e-241">Nicht konfigurierbar \*</span><span class="sxs-lookup"><span data-stu-id="01b6e-241">Not configurable \*</span></span> | <span data-ttu-id="01b6e-242">Ein Delegat, der so konfigurieren Sie zusätzliche "WebSocket"-Optionen verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="01b6e-242">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="01b6e-243">Empfängt eine Instanz von [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) , die verwendet werden kann, um Optionen zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="01b6e-243">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

<span data-ttu-id="01b6e-244">Optionen, die mit einem Sternchen (\*) gekennzeichnet sind, nicht im JavaScript-Client aufgrund von Beschränkungen im Browser APIs konfiguriert werden kann.</span><span class="sxs-lookup"><span data-stu-id="01b6e-244">Options marked with an asterisk (\*) aren't configurable in the JavaScript client, due to limitations in browser APIs.</span></span>

<span data-ttu-id="01b6e-245">In der .NET Client können diese Optionen von der Delegat Optionen geändert werden `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="01b6e-245">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="01b6e-246">In JavaScript-Client können in einem JavaScript-Objekt, das bereitgestellt, um diese Optionen bereitgestellt werden `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="01b6e-246">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    });
    .build();
```

## <a name="additional-resources"></a><span data-ttu-id="01b6e-247">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="01b6e-247">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
