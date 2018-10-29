---
title: ASP.NET Core SignalR .NET Client
author: tdykstra
description: Informationen zum ASP.NET Core SignalR .NET Client
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/10/2018
uid: signalr/dotnet-client
ms.openlocfilehash: 488d2ec31ce71534eeff4b9428262cc9ca00d992
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2018
ms.locfileid: "50205951"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="8c281-103">ASP.NET Core SignalR .NET Client</span><span class="sxs-lookup"><span data-stu-id="8c281-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="8c281-104">Der ASP.NET Core SignalR .NET Client Library können Sie die Kommunikation mit SignalR-Hubs in .NET apps.</span><span class="sxs-lookup"><span data-stu-id="8c281-104">The ASP.NET Core SignalR .NET client library lets you communicate with SignalR hubs from .NET apps.</span></span>

> [!NOTE]
> <span data-ttu-id="8c281-105">Xamarin stellt besondere Anforderungen an die Visual Studio-Version.</span><span class="sxs-lookup"><span data-stu-id="8c281-105">Xamarin has special requirements for Visual Studio version.</span></span> <span data-ttu-id="8c281-106">Weitere Informationen finden Sie unter [SignalR-Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).</span><span class="sxs-lookup"><span data-stu-id="8c281-106">For more information, see [SignalR Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).</span></span>

<span data-ttu-id="8c281-107">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([Vorgehensweise zum Herunterladen](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8c281-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="8c281-108">Das Codebeispiel in diesem Artikel ist eine WPF-app, die ASP.NET Core SignalR .NET Client verwendet.</span><span class="sxs-lookup"><span data-stu-id="8c281-108">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="8c281-109">Installieren Sie das SignalR .NET Client-Paket</span><span class="sxs-lookup"><span data-stu-id="8c281-109">Install the SignalR .NET client package</span></span>

<span data-ttu-id="8c281-110">Die `Microsoft.AspNetCore.SignalR.Client` für .NET-Clients zur Verbindung mit SignalR-Hubs ist ein Paket erforderlich.</span><span class="sxs-lookup"><span data-stu-id="8c281-110">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="8c281-111">Um die Clientbibliothek zu installieren, führen Sie den folgenden Befehl der **-Paket-Manager-Konsole** Fenster:</span><span class="sxs-lookup"><span data-stu-id="8c281-111">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="8c281-112">Verbinden mit einem hub</span><span class="sxs-lookup"><span data-stu-id="8c281-112">Connect to a hub</span></span>

<span data-ttu-id="8c281-113">Um eine Verbindung herzustellen, erstellen Sie eine `HubConnectionBuilder` , und rufen Sie `Build`.</span><span class="sxs-lookup"><span data-stu-id="8c281-113">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="8c281-114">Die Hub-URL, Protokoll, Transporttyp, Protokollebene, Header und anderen Optionen können beim Erstellen einer Verbindungs konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="8c281-114">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="8c281-115">Konfigurieren Sie alle erforderlichen Optionen durch Einfügen eines der `HubConnectionBuilder` Methoden `Build`.</span><span class="sxs-lookup"><span data-stu-id="8c281-115">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="8c281-116">Starten Sie die Verbindung mit `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="8c281-116">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a><span data-ttu-id="8c281-117">Behandeln Sie die Verbindung wurde unterbrochen</span><span class="sxs-lookup"><span data-stu-id="8c281-117">Handle lost connection</span></span>

<span data-ttu-id="8c281-118">Verwenden der <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> Ereignis reagieren, wenn die Verbindung wurde unterbrochen.</span><span class="sxs-lookup"><span data-stu-id="8c281-118">Use the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event to respond to a lost connection.</span></span> <span data-ttu-id="8c281-119">Beispielsweise empfiehlt es sich zum erneuten Herstellen einer Verbindung zu automatisieren.</span><span class="sxs-lookup"><span data-stu-id="8c281-119">For example, you might want to automate reconnection.</span></span>

<span data-ttu-id="8c281-120">Die `Closed` Ereignis erfordert ein Delegat, der zurückgegeben eine `Task`, wodurch Async-Code für die Ausführung ohne Verwendung `async void`.</span><span class="sxs-lookup"><span data-stu-id="8c281-120">The `Closed` event requires a delegate that returns a `Task`, which allows async code to run without using `async void`.</span></span> <span data-ttu-id="8c281-121">Erfüllen Sie die Signatur des Delegaten in einen `Closed` -Ereignishandler, der ausgeführt wird synchron zurückgeben `Task.CompletedTask`:</span><span class="sxs-lookup"><span data-stu-id="8c281-121">To satisfy the delegate signature in a `Closed` event handler that runs synchronously, return `Task.CompletedTask`:</span></span>

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

<span data-ttu-id="8c281-122">Der Hauptgrund für die Async-Unterstützung ist, damit Sie die Verbindung neu starten können.</span><span class="sxs-lookup"><span data-stu-id="8c281-122">The main reason for the async support is so you can restart the connection.</span></span> <span data-ttu-id="8c281-123">Starten eine Verbindung ist eine asynchrone Aktion.</span><span class="sxs-lookup"><span data-stu-id="8c281-123">Starting a connection is an async action.</span></span>

<span data-ttu-id="8c281-124">In einem `Closed` Handler, der die Verbindung neu gestartet. warten Sie einige zufällige Verzögerung, um zu verhindern, Überlastung des Servers, berücksichtigen, wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="8c281-124">In a `Closed` handler that restarts the connection, consider waiting for some random delay to prevent overloading the server, as shown in the following example:</span></span>

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="8c281-125">Aufrufen von Hub-Methoden von client</span><span class="sxs-lookup"><span data-stu-id="8c281-125">Call hub methods from client</span></span>

<span data-ttu-id="8c281-126">`InvokeAsync` Ruft Methoden für den Hub.</span><span class="sxs-lookup"><span data-stu-id="8c281-126">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="8c281-127">Übergeben Sie den Namen des Hubs-Methode und alle Argumente, die in die hubmethode, die definiert `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="8c281-127">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="8c281-128">SignalR ist asynchron, verwenden Sie daher `async` und `await` bei den aufrufen.</span><span class="sxs-lookup"><span data-stu-id="8c281-128">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="8c281-129">Rufen Sie Client-Methoden von Hub-Instanz</span><span class="sxs-lookup"><span data-stu-id="8c281-129">Call client methods from hub</span></span>

<span data-ttu-id="8c281-130">Definieren Sie Methoden, die der Hub Aufrufe mithilfe von `connection.On` nach der Erstellung, jedoch vor dem Starten der Verbindung.</span><span class="sxs-lookup"><span data-stu-id="8c281-130">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

<span data-ttu-id="8c281-131">Der vorangehende Code in `connection.On` ausgeführt wird, wenn Sie serverseitigen Code aufruft, mit der `SendAsync` Methode.</span><span class="sxs-lookup"><span data-stu-id="8c281-131">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="8c281-132">Fehlerbehandlung und Protokollierung</span><span class="sxs-lookup"><span data-stu-id="8c281-132">Error handling and logging</span></span>

<span data-ttu-id="8c281-133">Behandeln von Fehlern mit einem Try / Catch-Anweisung.</span><span class="sxs-lookup"><span data-stu-id="8c281-133">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="8c281-134">Überprüfen Sie die `Exception` Objekt, um zu bestimmen, die entsprechende Aktion aus, um aufzuzeichnen, wenn ein Fehler auftritt.</span><span class="sxs-lookup"><span data-stu-id="8c281-134">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a><span data-ttu-id="8c281-135">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="8c281-135">Additional resources</span></span>

* [<span data-ttu-id="8c281-136">Hubs</span><span class="sxs-lookup"><span data-stu-id="8c281-136">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="8c281-137">JavaScript-Client</span><span class="sxs-lookup"><span data-stu-id="8c281-137">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="8c281-138">Veröffentlichen in Azure</span><span class="sxs-lookup"><span data-stu-id="8c281-138">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
