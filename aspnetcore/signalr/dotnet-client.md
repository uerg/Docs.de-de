---
title: ASP.NET Core SignalR .NET Client
author: tdykstra
description: Informationen zum ASP.NET Core SignalR .NET Client
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/10/2018
uid: signalr/dotnet-client
ms.openlocfilehash: 205ca8ca228dcc2cc77f7e9b6431943851a3b152
ms.sourcegitcommit: 1a2fc47fb5d3da0f2a3c3269613ab20eb3b0da2c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2018
ms.locfileid: "44373318"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="e3dbb-103">ASP.NET Core SignalR .NET Client</span><span class="sxs-lookup"><span data-stu-id="e3dbb-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="e3dbb-104">Von [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="e3dbb-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="e3dbb-105">Der ASP.NET Core SignalR .NET Client Library können Sie die Kommunikation mit SignalR-Hubs in .NET apps.</span><span class="sxs-lookup"><span data-stu-id="e3dbb-105">The ASP.NET Core SignalR .NET client library lets you communicate with SignalR hubs from .NET apps.</span></span>

> [!NOTE]
> <span data-ttu-id="e3dbb-106">Xamarin stellt besondere Anforderungen an die Visual Studio-Version.</span><span class="sxs-lookup"><span data-stu-id="e3dbb-106">Xamarin has special requirements for Visual Studio version.</span></span> <span data-ttu-id="e3dbb-107">Weitere Informationen finden Sie unter [SignalR-Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).</span><span class="sxs-lookup"><span data-stu-id="e3dbb-107">For more information, see [SignalR Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).</span></span>

<span data-ttu-id="e3dbb-108">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e3dbb-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="e3dbb-109">Das Codebeispiel in diesem Artikel ist eine WPF-app, die ASP.NET Core SignalR .NET Client verwendet.</span><span class="sxs-lookup"><span data-stu-id="e3dbb-109">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="e3dbb-110">Installieren Sie das SignalR .NET Client-Paket</span><span class="sxs-lookup"><span data-stu-id="e3dbb-110">Install the SignalR .NET client package</span></span>

<span data-ttu-id="e3dbb-111">Die `Microsoft.AspNetCore.SignalR.Client` für .NET-Clients zur Verbindung mit SignalR-Hubs ist ein Paket erforderlich.</span><span class="sxs-lookup"><span data-stu-id="e3dbb-111">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="e3dbb-112">Um die Clientbibliothek zu installieren, führen Sie den folgenden Befehl der **-Paket-Manager-Konsole** Fenster:</span><span class="sxs-lookup"><span data-stu-id="e3dbb-112">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="e3dbb-113">Verbinden mit einem hub</span><span class="sxs-lookup"><span data-stu-id="e3dbb-113">Connect to a hub</span></span>

<span data-ttu-id="e3dbb-114">Um eine Verbindung herzustellen, erstellen Sie eine `HubConnectionBuilder` , und rufen Sie `Build`.</span><span class="sxs-lookup"><span data-stu-id="e3dbb-114">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="e3dbb-115">Die Hub-URL, Protokoll, Transporttyp, Protokollebene, Header und anderen Optionen können beim Erstellen einer Verbindungs konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="e3dbb-115">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="e3dbb-116">Konfigurieren Sie alle erforderlichen Optionen durch Einfügen eines der `HubConnectionBuilder` Methoden `Build`.</span><span class="sxs-lookup"><span data-stu-id="e3dbb-116">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="e3dbb-117">Starten Sie die Verbindung mit `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="e3dbb-117">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=14-16,32)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="e3dbb-118">Aufrufen von Hub-Methoden von client</span><span class="sxs-lookup"><span data-stu-id="e3dbb-118">Call hub methods from client</span></span>

<span data-ttu-id="e3dbb-119">`InvokeAsync` Ruft Methoden für den Hub.</span><span class="sxs-lookup"><span data-stu-id="e3dbb-119">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="e3dbb-120">Übergeben Sie den Namen des Hubs-Methode und alle Argumente, die in die hubmethode, die definiert `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="e3dbb-120">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="e3dbb-121">SignalR ist asynchron, verwenden Sie daher `async` und `await` bei den aufrufen.</span><span class="sxs-lookup"><span data-stu-id="e3dbb-121">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="e3dbb-122">Rufen Sie Client-Methoden von Hub-Instanz</span><span class="sxs-lookup"><span data-stu-id="e3dbb-122">Call client methods from hub</span></span>

<span data-ttu-id="e3dbb-123">Definieren Sie Methoden, die der Hub Aufrufe mithilfe von `connection.On` nach der Erstellung, jedoch vor dem Starten der Verbindung.</span><span class="sxs-lookup"><span data-stu-id="e3dbb-123">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

<span data-ttu-id="e3dbb-124">Der vorangehende Code in `connection.On` ausgeführt wird, wenn Sie serverseitigen Code aufruft, mit der `SendAsync` Methode.</span><span class="sxs-lookup"><span data-stu-id="e3dbb-124">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="e3dbb-125">Fehlerbehandlung und Protokollierung</span><span class="sxs-lookup"><span data-stu-id="e3dbb-125">Error handling and logging</span></span>

<span data-ttu-id="e3dbb-126">Behandeln von Fehlern mit einem Try / Catch-Anweisung.</span><span class="sxs-lookup"><span data-stu-id="e3dbb-126">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="e3dbb-127">Überprüfen Sie die `Exception` Objekt, um zu bestimmen, die entsprechende Aktion aus, um aufzuzeichnen, wenn ein Fehler auftritt.</span><span class="sxs-lookup"><span data-stu-id="e3dbb-127">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a><span data-ttu-id="e3dbb-128">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="e3dbb-128">Additional resources</span></span>

* [<span data-ttu-id="e3dbb-129">Hubs</span><span class="sxs-lookup"><span data-stu-id="e3dbb-129">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="e3dbb-130">JavaScript-Client</span><span class="sxs-lookup"><span data-stu-id="e3dbb-130">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="e3dbb-131">Veröffentlichen in Azure</span><span class="sxs-lookup"><span data-stu-id="e3dbb-131">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
