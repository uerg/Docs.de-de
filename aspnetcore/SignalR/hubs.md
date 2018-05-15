---
title: Verwenden von Hubs in ASP.NET Core SignalR
author: rachelappel
description: Erfahren Sie, wie Hubs in ASP.NET Core SignalR verwendet.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/01/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/hubs
ms.openlocfilehash: cfe9f7a7321094b8f901687d91745df2247e1da6
ms.sourcegitcommit: 3d071fabaf90e32906df97b08a8d00e602db25c0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="cde5d-103">Verwenden von Hubs in SignalR für ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cde5d-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="cde5d-104">Durch [Rachel Appel](https://twitter.com/rachelappel) und [Kevin Griffin](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="cde5d-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

<span data-ttu-id="cde5d-105">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(Gewusst wie: herunterladen)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="cde5d-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="cde5d-106">Was eine SignalR-Hubs ist</span><span class="sxs-lookup"><span data-stu-id="cde5d-106">What is a SignalR hub</span></span>

<span data-ttu-id="cde5d-107">Die SignalR-Hubs-API können Sie Methoden für verbundene Clients vom Server aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="cde5d-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="cde5d-108">In den Servercode definieren Sie die Methoden, die vom Client aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="cde5d-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="cde5d-109">Im Clientcode definieren Sie die Methoden, die vom Server aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="cde5d-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="cde5d-110">SignalR kümmert sich um alles, was im Hintergrund, die in Echtzeit-Client-zu-Server und Server zu Client-Kommunikation möglich macht.</span><span class="sxs-lookup"><span data-stu-id="cde5d-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="cde5d-111">Konfigurieren von SignalR-hubs</span><span class="sxs-lookup"><span data-stu-id="cde5d-111">Configure SignalR hubs</span></span>

<span data-ttu-id="cde5d-112">Die SignalR-Middleware erfordert einige Dienste, die durch den Aufruf konfiguriert werden `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="cde5d-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

<span data-ttu-id="cde5d-113">Wenn eine app ASP.NET Core SignalR-Funktionalität hinzufügen, richten Sie SignalR Routen durch Aufrufen `app.UseSignalR` in die `Startup.Configure` Methode.</span><span class="sxs-lookup"><span data-stu-id="cde5d-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="cde5d-114">Erstellen und Verwenden von hubs</span><span class="sxs-lookup"><span data-stu-id="cde5d-114">Create and use hubs</span></span>

<span data-ttu-id="cde5d-115">Erstellen Sie einen Hub durch Deklarieren einer Klasse, die von erben `Hub`, und fügen Sie öffentliche Methoden hinzu.</span><span class="sxs-lookup"><span data-stu-id="cde5d-115">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="cde5d-116">Clients können als definierten Methoden aufrufen `public`.</span><span class="sxs-lookup"><span data-stu-id="cde5d-116">Clients can call methods that are defined as `public`.</span></span>

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

<span data-ttu-id="cde5d-117">Sie können angeben, ein Rückgabetyp und Parameter, einschließlich Arrays und komplexe Typen, wie in einer C#-Methode.</span><span class="sxs-lookup"><span data-stu-id="cde5d-117">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="cde5d-118">SignalR verarbeitet die Serialisierung und Deserialisierung von komplexen Objekten und Arrays in Ihre Parameter und Rückgabewerte.</span><span class="sxs-lookup"><span data-stu-id="cde5d-118">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

## <a name="the-clients-object"></a><span data-ttu-id="cde5d-119">Das Objekt für Clients</span><span class="sxs-lookup"><span data-stu-id="cde5d-119">The Clients object</span></span>

<span data-ttu-id="cde5d-120">Jede Instanz von der `Hub` -Klasse verfügt über eine Eigenschaft namens `Clients` , enthält die folgenden Elemente für die Kommunikation zwischen Server und Client:</span><span class="sxs-lookup"><span data-stu-id="cde5d-120">Each instance of the `Hub` class has a property named `Clients` that contains the following members for communication between server and client:</span></span>

| <span data-ttu-id="cde5d-121">Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="cde5d-121">Property</span></span> | <span data-ttu-id="cde5d-122">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="cde5d-122">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="cde5d-123">Ruft eine Methode auf alle verbundenen clients</span><span class="sxs-lookup"><span data-stu-id="cde5d-123">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="cde5d-124">Ruft eine Methode auf dem Client, der die hubmethode aufgerufen hat</span><span class="sxs-lookup"><span data-stu-id="cde5d-124">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="cde5d-125">Ruft eine Methode auf alle verbundenen Clients mit Ausnahme des Clients, die die Methode aufgerufen hat.</span><span class="sxs-lookup"><span data-stu-id="cde5d-125">Calls a method on all connected clients except the client that invoked the method</span></span> |

<span data-ttu-id="cde5d-126">Darüber hinaus die `Hub` Klasse enthält die folgenden Methoden:</span><span class="sxs-lookup"><span data-stu-id="cde5d-126">Additionally, the `Hub` class contains the following methods:</span></span>

| <span data-ttu-id="cde5d-127">Methode</span><span class="sxs-lookup"><span data-stu-id="cde5d-127">Method</span></span> | <span data-ttu-id="cde5d-128">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="cde5d-128">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="cde5d-129">Ruft eine Methode auf alle verbundenen Clients mit Ausnahme der angegebenen Verbindungen.</span><span class="sxs-lookup"><span data-stu-id="cde5d-129">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="cde5d-130">Ruft eine Methode auf einem bestimmten verbundenen client</span><span class="sxs-lookup"><span data-stu-id="cde5d-130">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="cde5d-131">Ruft eine Methode auf bestimmte verbundene clients</span><span class="sxs-lookup"><span data-stu-id="cde5d-131">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="cde5d-132">Sendet eine Nachricht an alle Verbindungen in der angegebenen Gruppe</span><span class="sxs-lookup"><span data-stu-id="cde5d-132">Sends a message to all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="cde5d-133">Sendet eine Nachricht an alle Verbindungen in der angegebenen Gruppe ist, mit Ausnahme der angegebenen Verbindungen.</span><span class="sxs-lookup"><span data-stu-id="cde5d-133">Sends a message to all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="cde5d-134">Sendet eine Nachricht in mehrere Gruppen von Verbindungen</span><span class="sxs-lookup"><span data-stu-id="cde5d-134">Sends a message to multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="cde5d-135">Sendet eine Nachricht an eine Gruppe von Verbindungen mit Ausnahme des Clients, der die hubmethode aufgerufen hat</span><span class="sxs-lookup"><span data-stu-id="cde5d-135">Sends a message to a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="cde5d-136">Sendet eine Nachricht an alle Verbindungen, die einem bestimmten Benutzer zugeordnet</span><span class="sxs-lookup"><span data-stu-id="cde5d-136">Sends a message to all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="cde5d-137">Sendet eine Nachricht an alle Verbindungen, die die angegebenen Benutzer zugeordneten</span><span class="sxs-lookup"><span data-stu-id="cde5d-137">Sends a message to all connections associated with the specified users</span></span> |

<span data-ttu-id="cde5d-138">Jede Eigenschaft oder Methode in den vorherigen Tabellen gibt ein Objekt mit einer `SendAsync` Methode.</span><span class="sxs-lookup"><span data-stu-id="cde5d-138">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="cde5d-139">Die `SendAsync` Methode können Sie den Namen und den Parametern der aufzurufenden Clientmethode bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="cde5d-139">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="cde5d-140">Senden von Nachrichten an clients</span><span class="sxs-lookup"><span data-stu-id="cde5d-140">Send messages to clients</span></span>

<span data-ttu-id="cde5d-141">Um für bestimmte Clients aufrufen können, verwenden Sie die Eigenschaften des der `Clients` Objekt.</span><span class="sxs-lookup"><span data-stu-id="cde5d-141">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="cde5d-142">Im folgenden Beispiel die `SendMessageToCaller` -Methode veranschaulicht das Senden einer Nachricht an die Verbindung, die die hubmethode aufgerufen hat.</span><span class="sxs-lookup"><span data-stu-id="cde5d-142">In the following example, the `SendMessageToCaller` method demonstrates sending a message to the connection that invoked the hub method.</span></span> <span data-ttu-id="cde5d-143">Die `SendMessageToGroups` Methode sendet eine Nachricht an die Gruppen, die in gespeicherten eine `List` mit dem Namen `groups`.</span><span class="sxs-lookup"><span data-stu-id="cde5d-143">The `SendMessageToGroups` method sends a message to the groups stored in a `List` named `groups`.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="cde5d-144">Behandeln von Ereignissen für eine Verbindung</span><span class="sxs-lookup"><span data-stu-id="cde5d-144">Handle events for a connection</span></span>

<span data-ttu-id="cde5d-145">Die SignalR-Hubs-API bietet die `OnConnectedAsync` und `OnDisconnectedAsync` virtuelle Methoden zum Verwalten und Nachverfolgen von Verbindungen.</span><span class="sxs-lookup"><span data-stu-id="cde5d-145">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="cde5d-146">Überschreiben Sie die `OnConnectedAsync` virtuelle Methode, um Aktionen durchzuführen, wenn ein Client eine, auf den Hub Verbindung, z. B. eine Gruppe hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="cde5d-146">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a><span data-ttu-id="cde5d-147">Behandeln von Fehlern</span><span class="sxs-lookup"><span data-stu-id="cde5d-147">Handle errors</span></span>

<span data-ttu-id="cde5d-148">In den hubmethoden ausgelösten Ausnahmen werden an den Client gesendet, die die Methode aufgerufen hat.</span><span class="sxs-lookup"><span data-stu-id="cde5d-148">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="cde5d-149">Auf der JavaScript-Client die `invoke` Methode gibt ein [JavaScript-Zusicherung](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span><span class="sxs-lookup"><span data-stu-id="cde5d-149">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="cde5d-150">Wenn der Client einen Fehler mit einem Handler empfängt die Zusage mit angefügt `catch`, sie wird aufgerufen, und als JavaScript-bestanden `Error` Objekt.</span><span class="sxs-lookup"><span data-stu-id="cde5d-150">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a><span data-ttu-id="cde5d-151">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="cde5d-151">Related resources</span></span>

* [<span data-ttu-id="cde5d-152">Einführung in ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="cde5d-152">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="cde5d-153">JavaScript-Client</span><span class="sxs-lookup"><span data-stu-id="cde5d-153">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="cde5d-154">Veröffentlichen in Azure</span><span class="sxs-lookup"><span data-stu-id="cde5d-154">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
