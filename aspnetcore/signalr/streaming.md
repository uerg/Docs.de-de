---
title: Verwenden Sie in ASP.NET Core SignalR-streaming
author: tdykstra
description: ''
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/07/2018
uid: signalr/streaming
ms.openlocfilehash: 0001eed830249ac46ba35331759187bb4e7e8fd3
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095261"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a>Verwenden Sie in ASP.NET Core SignalR-streaming

Durch [Brennan Conroy](https://github.com/BrennanConroy)

ASP.NET Core SignalR unterstützt streaming Rückgabewerte von Servermethoden. Dies ist nützlich für Szenarien, in denen Fragmente von Daten im Laufe der Zeit kommen wird. Wenn ein Wert zurückgegeben, die an den Client gestreamt wird, wird jedes Fragment an den Client gesendet, sobald es wird zur Verfügung, statt alle Daten auf das Freiwerden warten.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="set-up-the-hub"></a>Richten Sie den hub

Eine hubmethode wird automatisch eine streaming hubmethode, bei der Rückgabe eine `ChannelReader<T>` oder `Task<ChannelReader<T>>`. Im folgenden finden Sie ein Beispiel mit den Grundlagen von streaming-Daten an den Client. Jedes Mal, wenn ein Objekt richtet der `ChannelReader` dieses Objekt wird sofort an den Client gesendet. Am Ende der `ChannelReader` abgeschlossen ist, um dem Client veranlassen der Stream ist geschlossen.

> [!NOTE]
> Schreiben in die `ChannelReader` auf einem Hintergrundthread und Rückgabe der `ChannelReader` so bald wie möglich. Andere Aufrufe Hub werden blockiert, bis eine `ChannelReader` zurückgegeben wird.

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?range=10-34)]

## <a name="net-client"></a>.NET-Client

Die `StreamAsChannelAsync` Methode `HubConnection` wird verwendet, um eine streaming-Methode aufrufen. Übergeben Sie den Namen des Hub-Methode und Argumente, die in die hubmethode, die definiert `StreamAsChannelAsync`. Der generische Parameter auf `StreamAsChannelAsync<T>` gibt den Typ der Objekte, die von der streaming-Methode zurückgegeben. Ein `ChannelReader<T>` , die über den Stream-Aufruf zurückgegeben wird, und den Stream darstellt, auf dem Client. Zum Lesen von Daten ist ein häufiges Muster in einer Schleife über `WaitToReadAsync` , und rufen Sie `TryRead` Wenn Daten verfügbar ist. Die Schleife endet, wenn der Stream vom Server geschlossen wurde wurde, oder das Abbruchtoken, um übergeben `StreamAsChannelAsync` abgebrochen wird.

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

## <a name="javascript-client"></a>JavaScript-client

JavaScript-Clients streamen Methoden aufrufen Hubs mithilfe von `connection.stream`. Die `stream` -Methode akzeptiert zwei Argumente:

* Der Name der hubmethode. Im folgenden Beispiel wird der Name der Hub-Methode `Counter`.
* In der hubmethode definierten Argumente. Im folgenden Beispiel werden die Argumente: eine Anzahl für die Anzahl der zu erhalten, und die Verzögerung zwischen Stream-Elemente.

`connection.stream` Gibt eine `IStreamResult` enthält eine `subscribe` Methode. Übergeben einer `IStreamSubscriber` zu `subscribe` und legen Sie die `next`, `error`, und `complete` Rückrufe an die Benachrichtigungen erhalten, die `stream` Aufruf.

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

Den Stream, aus der Client-Anruf beendet die `dispose` Methode für die `ISubscription` von zurückgegebene der `subscribe` Methode.

## <a name="related-resources"></a>Weitere Informationen

* [Hubs](xref:signalr/hubs)
* [.NET-Client](xref:signalr/dotnet-client)
* [JavaScript-Client](xref:signalr/javascript-client)
* [Veröffentlichen in Azure](xref:signalr/publish-to-azure-web-app)
