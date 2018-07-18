---
title: ASP.NET Core SignalR .NET Client
author: tdykstra
description: Informationen zum ASP.NET Core SignalR .NET Client
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/29/2018
uid: signalr/dotnet-client
ms.openlocfilehash: ce5be911e67831cbf6c09e24744111e73ffdbe63
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095033"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="c1532-103">ASP.NET Core SignalR .NET Client</span><span class="sxs-lookup"><span data-stu-id="c1532-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="c1532-104">Von [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="c1532-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="c1532-105">Die .NET für ASP.NET Core SignalR-Client kann von Xamarin, WPF, Windows Forms, Konsole und .NET Core-apps verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="c1532-105">The ASP.NET Core SignalR .NET client can be used by Xamarin, WPF, Windows Forms, Console, and .NET Core apps.</span></span> <span data-ttu-id="c1532-106">Wie die [JavaScript-Client](xref:signalr/javascript-client), der .NET Client können Sie empfangen und senden und Empfangen von Nachrichten mit einem Hub in Echtzeit.</span><span class="sxs-lookup"><span data-stu-id="c1532-106">Like the [JavaScript client](xref:signalr/javascript-client), the .NET client enables you to receive and send and receive messages to a hub in real time.</span></span>

<span data-ttu-id="c1532-107">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c1532-107">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="c1532-108">Das Codebeispiel in diesem Artikel ist eine WPF-app, die ASP.NET Core SignalR .NET Client verwendet.</span><span class="sxs-lookup"><span data-stu-id="c1532-108">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="c1532-109">Installieren Sie das SignalR .NET Client-Paket</span><span class="sxs-lookup"><span data-stu-id="c1532-109">Install the SignalR .NET client package</span></span>

<span data-ttu-id="c1532-110">Die `Microsoft.AspNetCore.SignalR.Client` für .NET-Clients zur Verbindung mit SignalR-Hubs ist ein Paket erforderlich.</span><span class="sxs-lookup"><span data-stu-id="c1532-110">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="c1532-111">Um die Clientbibliothek zu installieren, führen Sie den folgenden Befehl der **-Paket-Manager-Konsole** Fenster:</span><span class="sxs-lookup"><span data-stu-id="c1532-111">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="c1532-112">Verbinden mit einem hub</span><span class="sxs-lookup"><span data-stu-id="c1532-112">Connect to a hub</span></span>

<span data-ttu-id="c1532-113">Um eine Verbindung herzustellen, erstellen Sie eine `HubConnectionBuilder` , und rufen Sie `Build`.</span><span class="sxs-lookup"><span data-stu-id="c1532-113">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="c1532-114">Die Hub-URL, Protokoll, Transporttyp, Protokollebene, Header und anderen Optionen können beim Erstellen einer Verbindungs konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="c1532-114">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="c1532-115">Konfigurieren Sie alle erforderlichen Optionen durch Einfügen eines der `HubConnectionBuilder` Methoden `Build`.</span><span class="sxs-lookup"><span data-stu-id="c1532-115">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="c1532-116">Starten Sie die Verbindung mit `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="c1532-116">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?highlight=15-17,33)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="c1532-117">Aufrufen von Hub-Methoden von client</span><span class="sxs-lookup"><span data-stu-id="c1532-117">Call hub methods from client</span></span>

<span data-ttu-id="c1532-118">`InvokeAsync` Ruft Methoden für den Hub.</span><span class="sxs-lookup"><span data-stu-id="c1532-118">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="c1532-119">Übergeben Sie den Namen des Hubs-Methode und alle Argumente, die in die hubmethode, die definiert `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="c1532-119">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="c1532-120">SignalR ist asynchron, verwenden Sie daher `async` und `await` bei den aufrufen.</span><span class="sxs-lookup"><span data-stu-id="c1532-120">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=48-49)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="c1532-121">Rufen Sie Client-Methoden von Hub-Instanz</span><span class="sxs-lookup"><span data-stu-id="c1532-121">Call client methods from hub</span></span>

<span data-ttu-id="c1532-122">Definieren Sie Methoden, die der Hub Aufrufe mithilfe von `connection.On` nach der Erstellung, jedoch vor dem Starten der Verbindung.</span><span class="sxs-lookup"><span data-stu-id="c1532-122">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=22-29)]

<span data-ttu-id="c1532-123">Der vorangehende Code in `connection.On` ausgeführt wird, wenn Sie serverseitigen Code aufruft, mit der `SendAsync` Methode.</span><span class="sxs-lookup"><span data-stu-id="c1532-123">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?range=8-11)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="c1532-124">Fehlerbehandlung und Protokollierung</span><span class="sxs-lookup"><span data-stu-id="c1532-124">Error handling and logging</span></span>

<span data-ttu-id="c1532-125">Behandeln von Fehlern mit einem Try / Catch-Anweisung.</span><span class="sxs-lookup"><span data-stu-id="c1532-125">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="c1532-126">Überprüfen Sie die `Exception` Objekt, um zu bestimmen, die entsprechende Aktion aus, um aufzuzeichnen, wenn ein Fehler auftritt.</span><span class="sxs-lookup"><span data-stu-id="c1532-126">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=46-54)]

## <a name="additional-resources"></a><span data-ttu-id="c1532-127">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="c1532-127">Additional resources</span></span>

* [<span data-ttu-id="c1532-128">Hubs</span><span class="sxs-lookup"><span data-stu-id="c1532-128">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="c1532-129">JavaScript-Client</span><span class="sxs-lookup"><span data-stu-id="c1532-129">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="c1532-130">Veröffentlichen in Azure</span><span class="sxs-lookup"><span data-stu-id="c1532-130">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
