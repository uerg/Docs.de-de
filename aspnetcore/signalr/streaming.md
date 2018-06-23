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
# <a name="use-streaming-in-aspnet-core-signalr"></a>Verwenden Sie in ASP.NET Core SignalR streaming

Durch [Brennan Conroy](https://github.com/BrennanConroy)

SignalR für ASP.NET Core unterstützt streaming Rückgabewerte von Server-Standardmethoden. Dies ist hilfreich für Szenarien Datenfragmenten Zeitverlauf bieten sich werden. Wenn ein Rückgabewert an den Client übertragen wird, wird jedes Fragment an den Client gesendet, sobald es wird verfügbar, sondern wartet, bis alle Daten zur Verfügung.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="set-up-the-hub"></a>Richten Sie den hub

Eine hubmethode wird automatisch eine streaming hubmethode Bindungsprozesses eine `ChannelReader<T>` oder ein `Task<ChannelReader<T>>`. Es folgt ein Beispiel mit den Grundlagen von streaming-Daten an den Client. Wenn ein Objekt geschrieben wird, auf die `ChannelReader` dieses Objekt wird sofort an den Client gesendet. Am Ende der `ChannelReader` abgeschlossen ist, um dem Client anzuweisen, der Stream ist geschlossen.

> [!NOTE]
> Schreiben in die `ChannelReader` auf einem Hintergrundthread und der Rückgabewert der `ChannelReader` so bald wie möglich. Andere Aufrufe Hub werden blockiert, bis eine `ChannelReader` zurückgegeben.

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?range=10-34)]

## <a name="net-client"></a>.NET-Client

Die `StreamAsChannelAsync` Methode `HubConnection` wird verwendet, um eine streaming-Methode aufrufen. Übergibt den Namen der Hub-Methode und Argumente in der hubmethode, um definierte `StreamAsChannelAsync`. Der generische Parameter auf `StreamAsChannelAsync<T>` gibt den Typ der Objekte, die von der streaming-Methode zurückgegeben. Ein `ChannelReader<T>` über den Stream-Aufruf zurückgegeben wird und den Stream auf dem Client darstellt. Um Daten zu lesen, ist ein allgemeines Muster in einer Schleife über `WaitToReadAsync` , und rufen Sie `TryRead` Wenn Daten verfügbar ist. Die Schleife endet, wenn der Stream vom Server geschlossen wurde wurde, oder das an das Abbruchtoken, das `StreamAsChannelAsync` wird abgebrochen.

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

JavaScript-Clients streaming Methoden aufrufen Hubs mit `connection.stream`. Die `stream` Methode akzeptiert zwei Argumente:

* Der Name des Hub-Methode. Im folgenden Beispiel wird der Hub-Methodenname `Counter`.
* In der hubmethode definierten Argumente. Im folgenden Beispiel werden die Argumente: eine Anzahl für die Anzahl der Stream-Elemente zum Empfangen und die Verzögerung zwischen dem Stream-Elemente.

`connection.stream` Gibt eine `IStreamResult` enthält eine `subscribe` Methode. Übergeben ein `IStreamSubscriber` auf `subscribe` und legen Sie die `next`, `error`, und `complete` Rückrufe zum Abrufen von Benachrichtigungen von der `stream` aufrufen.

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

Den Stream, aus der Clientaufruf beendet die `dispose` Methode für die `ISubscription` zurückgegeben wird, aus der `subscribe` Methode.

## <a name="related-resources"></a>Weitere Informationen

* [Hubs](xref:signalr/hubs)
* [.NET-Client](xref:signalr/dotnet-client)
* [JavaScript-Client](xref:signalr/javascript-client)
* [Veröffentlichen in Azure](xref:signalr/publish-to-azure-web-app)
