---
title: Verwenden Sie in ASP.NET Core SignalR-streaming
author: tdykstra
description: ''
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/14/2018
uid: signalr/streaming
ms.openlocfilehash: 6d5f707bd2a37e1999c6e87e3cfc369aa0301207
ms.sourcegitcommit: 09bcda59a58019fdf47b2db5259fe87acf19dd38
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708438"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="67396-102">Verwenden Sie in ASP.NET Core SignalR-streaming</span><span class="sxs-lookup"><span data-stu-id="67396-102">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="67396-103">Durch [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="67396-103">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="67396-104">ASP.NET Core SignalR unterstützt streaming Rückgabewerte von Servermethoden.</span><span class="sxs-lookup"><span data-stu-id="67396-104">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="67396-105">Dies ist nützlich für Szenarien, in denen Fragmente von Daten im Laufe der Zeit kommen wird.</span><span class="sxs-lookup"><span data-stu-id="67396-105">This is useful for scenarios where fragments of data will come in over time.</span></span> <span data-ttu-id="67396-106">Wenn ein Wert zurückgegeben, die an den Client gestreamt wird, wird jedes Fragment an den Client gesendet, sobald es wird zur Verfügung, statt alle Daten auf das Freiwerden warten.</span><span class="sxs-lookup"><span data-stu-id="67396-106">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

<span data-ttu-id="67396-107">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([Vorgehensweise zum Herunterladen](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="67396-107">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="set-up-the-hub"></a><span data-ttu-id="67396-108">Richten Sie den hub</span><span class="sxs-lookup"><span data-stu-id="67396-108">Set up the hub</span></span>

<span data-ttu-id="67396-109">Eine hubmethode wird automatisch eine streaming hubmethode, bei der Rückgabe eine `ChannelReader<T>` oder `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="67396-109">A hub method automatically becomes a streaming hub method when it returns a `ChannelReader<T>` or a `Task<ChannelReader<T>>`.</span></span> <span data-ttu-id="67396-110">Im folgenden finden Sie ein Beispiel mit den Grundlagen von streaming-Daten an den Client.</span><span class="sxs-lookup"><span data-stu-id="67396-110">Below is a sample that shows the basics of streaming data to the client.</span></span> <span data-ttu-id="67396-111">Jedes Mal, wenn ein Objekt richtet der `ChannelReader` dieses Objekt wird sofort an den Client gesendet.</span><span class="sxs-lookup"><span data-stu-id="67396-111">Whenever an object is written to the `ChannelReader` that object is immediately sent to the client.</span></span> <span data-ttu-id="67396-112">Am Ende der `ChannelReader` abgeschlossen ist, um dem Client veranlassen der Stream ist geschlossen.</span><span class="sxs-lookup"><span data-stu-id="67396-112">At the end, the `ChannelReader` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> <span data-ttu-id="67396-113">Schreiben in die `ChannelReader` auf einem Hintergrundthread und Rückgabe der `ChannelReader` so bald wie möglich.</span><span class="sxs-lookup"><span data-stu-id="67396-113">Write to the `ChannelReader` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="67396-114">Andere Aufrufe Hub werden blockiert, bis eine `ChannelReader` zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="67396-114">Other hub invocations will be blocked until a `ChannelReader` is returned.</span></span>

::: moniker range="= aspnetcore-2.1"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.aspnetcore21.cs?range=12-36)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?range=11-35)]

> [!NOTE]
> <span data-ttu-id="67396-115">In ASP.NET Core 2.2 oder höher kann streaming-Hub-Methoden akzeptieren eine `CancellationToken` Parameter, der ausgelöst wird, wenn der Client aus dem Stream kündigt das Abonnement.</span><span class="sxs-lookup"><span data-stu-id="67396-115">In ASP.NET Core 2.2 or later, streaming Hub methods can accept a `CancellationToken` parameter that will be triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="67396-116">Verwenden Sie dieses Token beenden Sie den Servervorgang und alle Ressourcen freigeben, wenn der Client vor dem Ende des Streams, der die Verbindung trennt.</span><span class="sxs-lookup"><span data-stu-id="67396-116">Use this token to stop the server operation and release any resources if the client disconnects before the end of the stream.</span></span>

::: moniker-end

## <a name="net-client"></a><span data-ttu-id="67396-117">.NET-Client</span><span class="sxs-lookup"><span data-stu-id="67396-117">.NET client</span></span>

<span data-ttu-id="67396-118">Die `StreamAsChannelAsync` Methode `HubConnection` wird verwendet, um eine streaming-Methode aufrufen.</span><span class="sxs-lookup"><span data-stu-id="67396-118">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a streaming method.</span></span> <span data-ttu-id="67396-119">Übergeben Sie den Namen des Hub-Methode und Argumente, die in die hubmethode, die definiert `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="67396-119">Pass the hub method name, and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="67396-120">Der generische Parameter auf `StreamAsChannelAsync<T>` gibt den Typ der Objekte, die von der streaming-Methode zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="67396-120">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="67396-121">Ein `ChannelReader<T>` , die über den Stream-Aufruf zurückgegeben wird, und den Stream darstellt, auf dem Client.</span><span class="sxs-lookup"><span data-stu-id="67396-121">A `ChannelReader<T>` is returned from the stream invocation, and represents the stream on the client.</span></span> <span data-ttu-id="67396-122">Zum Lesen von Daten ist ein häufiges Muster in einer Schleife über `WaitToReadAsync` , und rufen Sie `TryRead` Wenn Daten verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="67396-122">To read data, a common pattern is to loop over `WaitToReadAsync` and call `TryRead` when data is available.</span></span> <span data-ttu-id="67396-123">Die Schleife endet, wenn der Stream vom Server geschlossen wurde wurde, oder das Abbruchtoken, um übergeben `StreamAsChannelAsync` abgebrochen wird.</span><span class="sxs-lookup"><span data-stu-id="67396-123">The loop will end when the stream has been closed by the server, or the cancellation token passed to `StreamAsChannelAsync` is canceled.</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
// Call "Cancel" on this CancellationTokenSource to send a cancellation message to 
// the server, which will trigger the corresponding token in the Hub method.
var cancellationTokenSource = new CancellationTokenSource();
var channel = await hubConnection.StreamAsChannelAsync<int>(
    "Counter", 10, 500, cancellationTokenSource.Token);

// Wait asynchronously for data to become available
while (await channel.WaitToReadAsync())
{
    // Read all currently available data synchronously, before waiting for more data
    while (channel.TryRead(out var count))
    {
        Console.WriteLine($"{count}");
    }
}

Console.WriteLine("Streaming completed");
```

::: moniker-end

::: moniker range="= aspnetcore-2.1"

```csharp
var channel = await hubConnection
    .StreamAsChannelAsync<int>("Counter", 10, 500, CancellationToken.None);

// Wait asynchronously for data to become available
while (await channel.WaitToReadAsync())
{
    // Read all currently available data synchronously, before waiting for more data
    while (channel.TryRead(out var count))
    {
        Console.WriteLine($"{count}");
    }
}

Console.WriteLine("Streaming completed");
```

::: moniker-end

## <a name="javascript-client"></a><span data-ttu-id="67396-124">JavaScript-client</span><span class="sxs-lookup"><span data-stu-id="67396-124">JavaScript client</span></span>

<span data-ttu-id="67396-125">JavaScript-Clients streamen Methoden aufrufen Hubs mithilfe von `connection.stream`.</span><span class="sxs-lookup"><span data-stu-id="67396-125">JavaScript clients call streaming methods on hubs by using `connection.stream`.</span></span> <span data-ttu-id="67396-126">Die `stream` -Methode akzeptiert zwei Argumente:</span><span class="sxs-lookup"><span data-stu-id="67396-126">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="67396-127">Der Name der hubmethode.</span><span class="sxs-lookup"><span data-stu-id="67396-127">The name of the hub method.</span></span> <span data-ttu-id="67396-128">Im folgenden Beispiel wird der Name der Hub-Methode `Counter`.</span><span class="sxs-lookup"><span data-stu-id="67396-128">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="67396-129">In der hubmethode definierten Argumente.</span><span class="sxs-lookup"><span data-stu-id="67396-129">Arguments defined in the hub method.</span></span> <span data-ttu-id="67396-130">Im folgenden Beispiel werden die Argumente: eine Anzahl für die Anzahl der zu erhalten, und die Verzögerung zwischen Stream-Elemente.</span><span class="sxs-lookup"><span data-stu-id="67396-130">In the following example, the arguments are: a count for the number of stream items to receive, and the delay between stream items.</span></span>

<span data-ttu-id="67396-131">`connection.stream` Gibt eine `IStreamResult` enthält eine `subscribe` Methode.</span><span class="sxs-lookup"><span data-stu-id="67396-131">`connection.stream` returns an `IStreamResult` which contains a `subscribe` method.</span></span> <span data-ttu-id="67396-132">Übergeben einer `IStreamSubscriber` zu `subscribe` und legen Sie die `next`, `error`, und `complete` Rückrufe an die Benachrichtigungen erhalten, die `stream` Aufruf.</span><span class="sxs-lookup"><span data-stu-id="67396-132">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to get notifications from the `stream` invocation.</span></span>

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="67396-133">Um den Datenstrom vom Client zu beenden, rufen die `dispose` Methode für die `ISubscription` zurückgegeben, die von der `subscribe` Methode.</span><span class="sxs-lookup"><span data-stu-id="67396-133">To end the stream from the client, call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="67396-134">Um den Datenstrom vom Client zu beenden, rufen die `dispose` Methode für die `ISubscription` zurückgegeben, die von der `subscribe` Methode.</span><span class="sxs-lookup"><span data-stu-id="67396-134">To end the stream from the client, call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span> <span data-ttu-id="67396-135">Aufrufen dieser Methode bewirkt, dass die `CancellationToken` Parameter der Hub-Methode (sofern Sie vorhanden) abgebrochen werden soll.</span><span class="sxs-lookup"><span data-stu-id="67396-135">Calling this method will cause the `CancellationToken` parameter of the Hub method (if you provided one) to be canceled.</span></span>

::: moniker-end

## <a name="related-resources"></a><span data-ttu-id="67396-136">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="67396-136">Related resources</span></span>

* [<span data-ttu-id="67396-137">Hubs</span><span class="sxs-lookup"><span data-stu-id="67396-137">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="67396-138">.NET-Client</span><span class="sxs-lookup"><span data-stu-id="67396-138">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="67396-139">JavaScript-Client</span><span class="sxs-lookup"><span data-stu-id="67396-139">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="67396-140">Veröffentlichen in Azure</span><span class="sxs-lookup"><span data-stu-id="67396-140">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
