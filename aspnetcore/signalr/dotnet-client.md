---
title: ASP.NET Core SignalR .NET Client
author: tdykstra
description: Informationen zum ASP.NET Core SignalR .NET Client
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/07/2018
uid: signalr/dotnet-client
ms.openlocfilehash: 970888a410b2486a20f98ce77a8674f8ec357f50
ms.sourcegitcommit: 028ad28c546de706ace98066c76774de33e4ad20
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2018
ms.locfileid: "39655251"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="b19d5-103">ASP.NET Core SignalR .NET Client</span><span class="sxs-lookup"><span data-stu-id="b19d5-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="b19d5-104">Von [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="b19d5-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="b19d5-105">Die .NET für ASP.NET Core SignalR-Client kann von Xamarin, WPF, Windows Forms, Konsole und .NET Core-apps verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="b19d5-105">The ASP.NET Core SignalR .NET client can be used by Xamarin, WPF, Windows Forms, Console, and .NET Core apps.</span></span> <span data-ttu-id="b19d5-106">Wie die [JavaScript-Client](xref:signalr/javascript-client), der .NET Client können Sie empfangen und senden und Empfangen von Nachrichten mit einem Hub in Echtzeit.</span><span class="sxs-lookup"><span data-stu-id="b19d5-106">Like the [JavaScript client](xref:signalr/javascript-client), the .NET client enables you to receive and send and receive messages to a hub in real time.</span></span>

> [!NOTE]
> <span data-ttu-id="b19d5-107">Xamarin stellt besondere Anforderungen an die Visual Studio-Version.</span><span class="sxs-lookup"><span data-stu-id="b19d5-107">Xamarin has special requirements for Visual Studio version.</span></span> <span data-ttu-id="b19d5-108">Weitere Informationen finden Sie unter [SignalR-Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).</span><span class="sxs-lookup"><span data-stu-id="b19d5-108">For more information, see [SignalR Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).</span></span>

<span data-ttu-id="b19d5-109">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b19d5-109">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="b19d5-110">Das Codebeispiel in diesem Artikel ist eine WPF-app, die ASP.NET Core SignalR .NET Client verwendet.</span><span class="sxs-lookup"><span data-stu-id="b19d5-110">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="b19d5-111">Installieren Sie das SignalR .NET Client-Paket</span><span class="sxs-lookup"><span data-stu-id="b19d5-111">Install the SignalR .NET client package</span></span>

<span data-ttu-id="b19d5-112">Die `Microsoft.AspNetCore.SignalR.Client` für .NET-Clients zur Verbindung mit SignalR-Hubs ist ein Paket erforderlich.</span><span class="sxs-lookup"><span data-stu-id="b19d5-112">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="b19d5-113">Um die Clientbibliothek zu installieren, führen Sie den folgenden Befehl der **-Paket-Manager-Konsole** Fenster:</span><span class="sxs-lookup"><span data-stu-id="b19d5-113">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="b19d5-114">Verbinden mit einem hub</span><span class="sxs-lookup"><span data-stu-id="b19d5-114">Connect to a hub</span></span>

<span data-ttu-id="b19d5-115">Um eine Verbindung herzustellen, erstellen Sie eine `HubConnectionBuilder` , und rufen Sie `Build`.</span><span class="sxs-lookup"><span data-stu-id="b19d5-115">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="b19d5-116">Die Hub-URL, Protokoll, Transporttyp, Protokollebene, Header und anderen Optionen können beim Erstellen einer Verbindungs konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="b19d5-116">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="b19d5-117">Konfigurieren Sie alle erforderlichen Optionen durch Einfügen eines der `HubConnectionBuilder` Methoden `Build`.</span><span class="sxs-lookup"><span data-stu-id="b19d5-117">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="b19d5-118">Starten Sie die Verbindung mit `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="b19d5-118">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?highlight=15-17,33)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="b19d5-119">Aufrufen von Hub-Methoden von client</span><span class="sxs-lookup"><span data-stu-id="b19d5-119">Call hub methods from client</span></span>

<span data-ttu-id="b19d5-120">`InvokeAsync` Ruft Methoden für den Hub.</span><span class="sxs-lookup"><span data-stu-id="b19d5-120">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="b19d5-121">Übergeben Sie den Namen des Hubs-Methode und alle Argumente, die in die hubmethode, die definiert `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="b19d5-121">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="b19d5-122">SignalR ist asynchron, verwenden Sie daher `async` und `await` bei den aufrufen.</span><span class="sxs-lookup"><span data-stu-id="b19d5-122">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=48-49)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="b19d5-123">Rufen Sie Client-Methoden von Hub-Instanz</span><span class="sxs-lookup"><span data-stu-id="b19d5-123">Call client methods from hub</span></span>

<span data-ttu-id="b19d5-124">Definieren Sie Methoden, die der Hub Aufrufe mithilfe von `connection.On` nach der Erstellung, jedoch vor dem Starten der Verbindung.</span><span class="sxs-lookup"><span data-stu-id="b19d5-124">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=22-29)]

<span data-ttu-id="b19d5-125">Der vorangehende Code in `connection.On` ausgeführt wird, wenn Sie serverseitigen Code aufruft, mit der `SendAsync` Methode.</span><span class="sxs-lookup"><span data-stu-id="b19d5-125">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?range=8-11)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="b19d5-126">Fehlerbehandlung und Protokollierung</span><span class="sxs-lookup"><span data-stu-id="b19d5-126">Error handling and logging</span></span>

<span data-ttu-id="b19d5-127">Behandeln von Fehlern mit einem Try / Catch-Anweisung.</span><span class="sxs-lookup"><span data-stu-id="b19d5-127">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="b19d5-128">Überprüfen Sie die `Exception` Objekt, um zu bestimmen, die entsprechende Aktion aus, um aufzuzeichnen, wenn ein Fehler auftritt.</span><span class="sxs-lookup"><span data-stu-id="b19d5-128">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=46-54)]

## <a name="additional-resources"></a><span data-ttu-id="b19d5-129">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="b19d5-129">Additional resources</span></span>

* [<span data-ttu-id="b19d5-130">Hubs</span><span class="sxs-lookup"><span data-stu-id="b19d5-130">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="b19d5-131">JavaScript-Client</span><span class="sxs-lookup"><span data-stu-id="b19d5-131">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="b19d5-132">Veröffentlichen in Azure</span><span class="sxs-lookup"><span data-stu-id="b19d5-132">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
