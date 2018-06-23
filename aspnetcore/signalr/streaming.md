---
title: Verwenden Sie in ASP.NET Core SignalR streaming
author: rachelappel
description: ''
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/07/2018
uid: signalr/streaming
ms.openlocfilehash: 08ddea4fb83150bab27a9e2685c75ff34565606b
ms.sourcegitcommit: 79b756ea03eae77a716f500ef88253ee9b1464d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/22/2018
ms.locfileid: "36327492"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="89658-102">Verwenden Sie in ASP.NET Core SignalR streaming</span><span class="sxs-lookup"><span data-stu-id="89658-102">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="89658-103">Durch [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="89658-103">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="89658-104">SignalR für ASP.NET Core unterstützt streaming Rückgabewerte von Server-Standardmethoden.</span><span class="sxs-lookup"><span data-stu-id="89658-104">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="89658-105">Dies ist hilfreich für Szenarien Datenfragmenten Zeitverlauf bieten sich werden.</span><span class="sxs-lookup"><span data-stu-id="89658-105">This is useful for scenarios where fragments of data will come in over time.</span></span> <span data-ttu-id="89658-106">Wenn ein Rückgabewert an den Client übertragen wird, wird jedes Fragment an den Client gesendet, sobald es wird verfügbar, sondern wartet, bis alle Daten zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="89658-106">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

<span data-ttu-id="89658-107">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="89658-107">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="set-up-the-hub"></a><span data-ttu-id="89658-108">Richten Sie den hub</span><span class="sxs-lookup"><span data-stu-id="89658-108">Set up the hub</span></span>

<span data-ttu-id="89658-109">Eine hubmethode wird automatisch eine streaming hubmethode Bindungsprozesses eine `ChannelReader<T>` oder ein `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="89658-109">A hub method automatically becomes a streaming hub method when it returns a `ChannelReader<T>` or a `Task<ChannelReader<T>>`.</span></span> <span data-ttu-id="89658-110">Es folgt ein Beispiel mit den Grundlagen von streaming-Daten an den Client.</span><span class="sxs-lookup"><span data-stu-id="89658-110">Below is a sample that shows the basics of streaming data to the client.</span></span> <span data-ttu-id="89658-111">Wenn ein Objekt geschrieben wird, auf die `ChannelReader` dieses Objekt wird sofort an den Client gesendet.</span><span class="sxs-lookup"><span data-stu-id="89658-111">Whenever an object is written to the `ChannelReader` that object is immediately sent to the client.</span></span> <span data-ttu-id="89658-112">Am Ende der `ChannelReader` abgeschlossen ist, um dem Client anzuweisen, der Stream ist geschlossen.</span><span class="sxs-lookup"><span data-stu-id="89658-112">At the end, the `ChannelReader` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> <span data-ttu-id="89658-113">Schreiben in die `ChannelReader` auf einem Hintergrundthread und der Rückgabewert der `ChannelReader` so bald wie möglich.</span><span class="sxs-lookup"><span data-stu-id="89658-113">Write to the `ChannelReader` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="89658-114">Andere Aufrufe Hub werden blockiert, bis eine `ChannelReader` zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="89658-114">Other hub invocations will be blocked until a `ChannelReader` is returned.</span></span>

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?range=10-34)]

## <a name="net-client"></a><span data-ttu-id="89658-115">.NET-Client</span><span class="sxs-lookup"><span data-stu-id="89658-115">.NET client</span></span>

<span data-ttu-id="89658-116">Die `StreamAsChannelAsync` Methode `HubConnection` wird verwendet, um eine streaming-Methode aufrufen.</span><span class="sxs-lookup"><span data-stu-id="89658-116">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a streaming method.</span></span> <span data-ttu-id="89658-117">Übergibt den Namen der Hub-Methode und Argumente in der hubmethode, um definierte `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="89658-117">Pass the hub method name, and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="89658-118">Der generische Parameter auf `StreamAsChannelAsync<T>` gibt den Typ der Objekte, die von der streaming-Methode zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="89658-118">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="89658-119">Ein `ChannelReader<T>` über den Stream-Aufruf zurückgegeben wird und den Stream auf dem Client darstellt.</span><span class="sxs-lookup"><span data-stu-id="89658-119">A `ChannelReader<T>` is returned from the stream invocation, and represents the stream on the client.</span></span> <span data-ttu-id="89658-120">Um Daten zu lesen, ist ein allgemeines Muster in einer Schleife über `WaitToReadAsync` , und rufen Sie `TryRead` Wenn Daten verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="89658-120">To read data, a common pattern is to loop over `WaitToReadAsync` and call `TryRead` when data is available.</span></span> <span data-ttu-id="89658-121">Die Schleife endet, wenn der Stream vom Server geschlossen wurde wurde, oder das an das Abbruchtoken, das `StreamAsChannelAsync` wird abgebrochen.</span><span class="sxs-lookup"><span data-stu-id="89658-121">The loop will end when the stream has been closed by the server, or the cancellation token passed to `StreamAsChannelAsync` is canceled.</span></span>

```csharp
var channel = await hubConnection.StreamAsChannelAsync<int>("Counter", 10, 500, CancellationToken.None);

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

## <a name="javascript-client"></a><span data-ttu-id="89658-122">JavaScript-client</span><span class="sxs-lookup"><span data-stu-id="89658-122">JavaScript client</span></span>

<span data-ttu-id="89658-123">JavaScript-Clients streaming Methoden aufrufen Hubs mit `connection.stream`.</span><span class="sxs-lookup"><span data-stu-id="89658-123">JavaScript clients call streaming methods on hubs by using `connection.stream`.</span></span> <span data-ttu-id="89658-124">Die `stream` Methode akzeptiert zwei Argumente:</span><span class="sxs-lookup"><span data-stu-id="89658-124">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="89658-125">Der Name des Hub-Methode.</span><span class="sxs-lookup"><span data-stu-id="89658-125">The name of the hub method.</span></span> <span data-ttu-id="89658-126">Im folgenden Beispiel wird der Hub-Methodenname `Counter`.</span><span class="sxs-lookup"><span data-stu-id="89658-126">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="89658-127">In der hubmethode definierten Argumente.</span><span class="sxs-lookup"><span data-stu-id="89658-127">Arguments defined in the hub method.</span></span> <span data-ttu-id="89658-128">Im folgenden Beispiel werden die Argumente: eine Anzahl für die Anzahl der Stream-Elemente zum Empfangen und die Verzögerung zwischen dem Stream-Elemente.</span><span class="sxs-lookup"><span data-stu-id="89658-128">In the following example, the arguments are: a count for the number of stream items to receive, and the delay between stream items.</span></span>

<span data-ttu-id="89658-129">`connection.stream` Gibt eine `IStreamResult` enthält eine `subscribe` Methode.</span><span class="sxs-lookup"><span data-stu-id="89658-129">`connection.stream` returns an `IStreamResult` which contains a `subscribe` method.</span></span> <span data-ttu-id="89658-130">Übergeben ein `IStreamSubscriber` auf `subscribe` und legen Sie die `next`, `error`, und `complete` Rückrufe zum Abrufen von Benachrichtigungen von der `stream` aufrufen.</span><span class="sxs-lookup"><span data-stu-id="89658-130">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to get notifications from the `stream` invocation.</span></span>

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="89658-131">Den Stream, aus der Clientaufruf beendet die `dispose` Methode für die `ISubscription` zurückgegeben wird, aus der `subscribe` Methode.</span><span class="sxs-lookup"><span data-stu-id="89658-131">To end the stream from the client call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span>

## <a name="related-resources"></a><span data-ttu-id="89658-132">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="89658-132">Related resources</span></span>

* [<span data-ttu-id="89658-133">Hubs</span><span class="sxs-lookup"><span data-stu-id="89658-133">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="89658-134">.NET-Client</span><span class="sxs-lookup"><span data-stu-id="89658-134">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="89658-135">JavaScript-Client</span><span class="sxs-lookup"><span data-stu-id="89658-135">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="89658-136">Veröffentlichen in Azure</span><span class="sxs-lookup"><span data-stu-id="89658-136">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
