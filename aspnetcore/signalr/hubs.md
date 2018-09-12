---
title: Verwenden von Hubs in ASP.NET Core SignalR
author: tdykstra
description: Erfahren Sie, wie Sie mit Hubs in ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/01/2018
uid: signalr/hubs
ms.openlocfilehash: e583676ab0eed45aeaf6391d8cdf8c1485aa914e
ms.sourcegitcommit: e7e1e531b80b3f4117ff119caadbebf4dcf5dcb7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2018
ms.locfileid: "44510336"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="07db6-103">Verwenden von Hubs in SignalR für ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="07db6-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="07db6-104">Durch [Rachel Appel](https://twitter.com/rachelappel) und [Kevin Griffin](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="07db6-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

<span data-ttu-id="07db6-105">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(Herunterladen von)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="07db6-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="07db6-106">Was ist eine SignalR-Hub</span><span class="sxs-lookup"><span data-stu-id="07db6-106">What is a SignalR hub</span></span>

<span data-ttu-id="07db6-107">Der SignalR-Hubs-API können Sie zum Aufrufen von Methoden auf verbundene Clients vom Server.</span><span class="sxs-lookup"><span data-stu-id="07db6-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="07db6-108">In den Servercode definieren Sie Methoden, die vom Client aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="07db6-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="07db6-109">Im Clientcode definieren Sie Methoden, die vom Server aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="07db6-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="07db6-110">SignalR kümmert sich um alles, was hinter den Kulissen, die in Echtzeit-Client-zu-Server und Server-zu-Client-Kommunikation möglich macht.</span><span class="sxs-lookup"><span data-stu-id="07db6-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="07db6-111">Konfigurieren von SignalR-hubs</span><span class="sxs-lookup"><span data-stu-id="07db6-111">Configure SignalR hubs</span></span>

<span data-ttu-id="07db6-112">Die SignalR-Middleware ist erforderlich, einige Dienste, die durch den Aufruf konfiguriert werden `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="07db6-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

<span data-ttu-id="07db6-113">Beim Hinzufügen von SignalR-Funktionen zu einer ASP.NET Core-app einrichten SignalR-Routen, durch den Aufruf `app.UseSignalR` in die `Startup.Configure` Methode.</span><span class="sxs-lookup"><span data-stu-id="07db6-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="07db6-114">Erstellen und Verwenden von hubs</span><span class="sxs-lookup"><span data-stu-id="07db6-114">Create and use hubs</span></span>

<span data-ttu-id="07db6-115">Erstellen Sie einen Hub durch Deklarieren einer Klasse, die von erbt `Hub`, und fügen Sie öffentliche Methoden hinzu.</span><span class="sxs-lookup"><span data-stu-id="07db6-115">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="07db6-116">Clients können als definierten Methoden aufrufen `public`.</span><span class="sxs-lookup"><span data-stu-id="07db6-116">Clients can call methods that are defined as `public`.</span></span>

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

<span data-ttu-id="07db6-117">Sie können einen Rückgabetyp und Parameter, einschließlich von komplexen Typen und Arrays, wie in jeder C#-Methode angeben.</span><span class="sxs-lookup"><span data-stu-id="07db6-117">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="07db6-118">SignalR verarbeitet die Serialisierung und Deserialisierung von komplexen Objekten und Arrays in Ihre Parameter und Rückgabewerte.</span><span class="sxs-lookup"><span data-stu-id="07db6-118">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

## <a name="the-context-object"></a><span data-ttu-id="07db6-119">Das Context-Objekt</span><span class="sxs-lookup"><span data-stu-id="07db6-119">The Context object</span></span>

<span data-ttu-id="07db6-120">Die `Hub` -Klasse verfügt über eine `Context` Eigenschaft, die mit Informationen über die Verbindung die folgenden Eigenschaften enthält:</span><span class="sxs-lookup"><span data-stu-id="07db6-120">The `Hub` class has a `Context` property that contains the following properties with information about the connection:</span></span>

| <span data-ttu-id="07db6-121">Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="07db6-121">Property</span></span> | <span data-ttu-id="07db6-122">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="07db6-122">Description</span></span> |
| ------ | ----------- |
| `ConnectionId` | <span data-ttu-id="07db6-123">Ruft die eindeutige ID für die Verbindung von SignalR zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="07db6-123">Gets the unique ID for the connection, assigned by SignalR.</span></span> <span data-ttu-id="07db6-124">Eine Verbindungs­id für jede Verbindung ist vorhanden.</span><span class="sxs-lookup"><span data-stu-id="07db6-124">There is one connection ID for each connection.</span></span>|
| `UserIdentifier` | <span data-ttu-id="07db6-125">Ruft die [Benutzerbezeichner](xref:signalr/groups).</span><span class="sxs-lookup"><span data-stu-id="07db6-125">Gets the [user identifier](xref:signalr/groups).</span></span> <span data-ttu-id="07db6-126">SignalR verwendet standardmäßig die `ClaimTypes.NameIdentifier` aus der `ClaimsPrincipal` der Verbindung als die Benutzer-ID zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="07db6-126">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> |
| `User` | <span data-ttu-id="07db6-127">Ruft die `ClaimsPrincipal` mit dem aktuellen Benutzer verknüpft ist.</span><span class="sxs-lookup"><span data-stu-id="07db6-127">Gets the `ClaimsPrincipal` associated with the current user.</span></span> |
| `Items` | <span data-ttu-id="07db6-128">Ruft eine Auflistung von Schlüssel-Wert, die zum Freigeben von Daten innerhalb des Bereichs dieser Verbindung verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="07db6-128">Gets a key/value collection that can be used to share data within the scope of this connection.</span></span> <span data-ttu-id="07db6-129">Daten können in dieser Auflistung gespeichert werden, und bleiben für die Verbindung in anderen hubmethodenaufrufe.</span><span class="sxs-lookup"><span data-stu-id="07db6-129">Data can be stored in this collection and it will persist for the connection across different hub method invocations.</span></span> |
| `Features` | <span data-ttu-id="07db6-130">Ruft die Auflistung der verfügbaren Funktionen für die Verbindung ab.</span><span class="sxs-lookup"><span data-stu-id="07db6-130">Gets the collection of features available on the connection.</span></span> <span data-ttu-id="07db6-131">Vorerst ist nicht dieser Auflistung in den meisten Szenarien erforderlich, damit noch ausführlich dokumentiert ist nicht.</span><span class="sxs-lookup"><span data-stu-id="07db6-131">For now, this collection isn't needed in most scenarios, so it isn't documented in detail yet.</span></span> |
| `ConnectionAborted` | <span data-ttu-id="07db6-132">Ruft eine `CancellationToken` , die benachrichtigt werden, wenn die Verbindung abgebrochen wird.</span><span class="sxs-lookup"><span data-stu-id="07db6-132">Gets a `CancellationToken` that notifies when the connection is aborted.</span></span> |

<span data-ttu-id="07db6-133">`Hub.Context` Außerdem enthält die folgenden Methoden:</span><span class="sxs-lookup"><span data-stu-id="07db6-133">`Hub.Context` also contains the following methods:</span></span>

| <span data-ttu-id="07db6-134">Methode</span><span class="sxs-lookup"><span data-stu-id="07db6-134">Method</span></span> | <span data-ttu-id="07db6-135">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="07db6-135">Description</span></span> |
| ------ | ----------- |
| `GetHttpContext` | <span data-ttu-id="07db6-136">Gibt die `HttpContext` für die Verbindung oder `null` , wenn die Verbindung nicht mit einer HTTP-Anforderung verknüpft ist.</span><span class="sxs-lookup"><span data-stu-id="07db6-136">Returns the `HttpContext` for the connection, or `null` if the connection is not associated with an HTTP request.</span></span> <span data-ttu-id="07db6-137">Für HTTP-Verbindungen können Sie diese Methode zum Abrufen von Informationen wie z. B. HTTP-Header und Abfragezeichenfolgen.</span><span class="sxs-lookup"><span data-stu-id="07db6-137">For HTTP connections, you can use this method to get information such as HTTP headers and query strings.</span></span> |
| `Abort` | <span data-ttu-id="07db6-138">Bricht die Verbindung ab.</span><span class="sxs-lookup"><span data-stu-id="07db6-138">Aborts the connection.</span></span> |

## <a name="the-clients-object"></a><span data-ttu-id="07db6-139">Das Objekt des Clients</span><span class="sxs-lookup"><span data-stu-id="07db6-139">The Clients object</span></span>

<span data-ttu-id="07db6-140">Die `Hub` -Klasse verfügt über eine `Clients` -Eigenschaft, die folgenden Eigenschaften für die Kommunikation zwischen Server und Client enthält:</span><span class="sxs-lookup"><span data-stu-id="07db6-140">The `Hub` class has a `Clients` property that contains the following properties for communication between server and client:</span></span>

| <span data-ttu-id="07db6-141">Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="07db6-141">Property</span></span> | <span data-ttu-id="07db6-142">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="07db6-142">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="07db6-143">Ruft eine Methode für alle verbundenen clients</span><span class="sxs-lookup"><span data-stu-id="07db6-143">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="07db6-144">Ruft eine Methode auf dem Client, der die hubmethode aufgerufen hat.</span><span class="sxs-lookup"><span data-stu-id="07db6-144">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="07db6-145">Ruft eine Methode für alle verbundenen Clients mit Ausnahme des Clients, die die Methode aufgerufen hat.</span><span class="sxs-lookup"><span data-stu-id="07db6-145">Calls a method on all connected clients except the client that invoked the method</span></span> |


<span data-ttu-id="07db6-146">`Hub.Clients` Außerdem enthält die folgenden Methoden:</span><span class="sxs-lookup"><span data-stu-id="07db6-146">`Hub.Clients` also contains the following methods:</span></span>

| <span data-ttu-id="07db6-147">Methode</span><span class="sxs-lookup"><span data-stu-id="07db6-147">Method</span></span> | <span data-ttu-id="07db6-148">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="07db6-148">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="07db6-149">Ruft eine Methode für alle verbundenen Clients mit Ausnahme der angegebenen Verbindungen</span><span class="sxs-lookup"><span data-stu-id="07db6-149">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="07db6-150">Ruft eine Methode auf einem bestimmten verbundenen client</span><span class="sxs-lookup"><span data-stu-id="07db6-150">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="07db6-151">Ruft eine Methode für bestimmte verbundene clients</span><span class="sxs-lookup"><span data-stu-id="07db6-151">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="07db6-152">Ruft eine Methode für alle Verbindungen in der angegebenen Gruppe</span><span class="sxs-lookup"><span data-stu-id="07db6-152">Calls a method to all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="07db6-153">Ruft eine Methode für alle Verbindungen in der angegebenen Gruppe an, mit Ausnahme der angegebenen Verbindungen.</span><span class="sxs-lookup"><span data-stu-id="07db6-153">Calls a method to all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="07db6-154">Ruft eine Methode in mehrere Gruppen von Verbindungen</span><span class="sxs-lookup"><span data-stu-id="07db6-154">Calls a method to multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="07db6-155">Ruft eine Methode für eine Gruppe von Verbindungen mit Ausnahme des Clients, der die hubmethode aufgerufen hat</span><span class="sxs-lookup"><span data-stu-id="07db6-155">Calls a method to a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="07db6-156">Ruft eine Methode, um alle Verbindungen, die einen bestimmten Benutzer zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="07db6-156">Calls a method to all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="07db6-157">Ruft eine Methode, um alle Verbindungen, die die angegebenen Benutzer zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="07db6-157">Calls a method to all connections associated with the specified users</span></span> |

<span data-ttu-id="07db6-158">Jede Eigenschaft oder Methode in den vorhergehenden Tabellen gibt ein Objekt mit einem `SendAsync` Methode.</span><span class="sxs-lookup"><span data-stu-id="07db6-158">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="07db6-159">Die `SendAsync` Methode können Sie den Namen und Parameter, der die Methode aufrufen, angeben.</span><span class="sxs-lookup"><span data-stu-id="07db6-159">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="07db6-160">Senden von Nachrichten an clients</span><span class="sxs-lookup"><span data-stu-id="07db6-160">Send messages to clients</span></span>

<span data-ttu-id="07db6-161">Um Aufrufe von bestimmten Clients zu machen, verwenden Sie die Eigenschaften der `Clients` Objekt.</span><span class="sxs-lookup"><span data-stu-id="07db6-161">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="07db6-162">Im folgenden Beispiel die `SendMessageToCaller` -Methode demonstriert, Senden einer Nachricht an die Verbindung, die die hubmethode aufgerufen hat.</span><span class="sxs-lookup"><span data-stu-id="07db6-162">In the following example, the `SendMessageToCaller` method demonstrates sending a message to the connection that invoked the hub method.</span></span> <span data-ttu-id="07db6-163">Die `SendMessageToGroups` Methode sendet eine Nachricht an die Gruppen, die in gespeicherten eine `List` mit dem Namen `groups`.</span><span class="sxs-lookup"><span data-stu-id="07db6-163">The `SendMessageToGroups` method sends a message to the groups stored in a `List` named `groups`.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="07db6-164">Behandeln von Ereignissen für eine Verbindung</span><span class="sxs-lookup"><span data-stu-id="07db6-164">Handle events for a connection</span></span>

<span data-ttu-id="07db6-165">Die SignalR-Hubs-API bietet die `OnConnectedAsync` und `OnDisconnectedAsync` virtuelle Methoden zum Verwalten und Nachverfolgen von Verbindungen.</span><span class="sxs-lookup"><span data-stu-id="07db6-165">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="07db6-166">Überschreiben der `OnConnectedAsync` virtuelle Methode, um die Aktionen ausführen, wenn einem Client an den Hub zwischen, z. B. zu einer Gruppe hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="07db6-166">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a><span data-ttu-id="07db6-167">Behandeln von Fehlern</span><span class="sxs-lookup"><span data-stu-id="07db6-167">Handle errors</span></span>

<span data-ttu-id="07db6-168">In den hubmethoden ausgelöste Ausnahmen werden an den Client gesendet, die die Methode aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="07db6-168">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="07db6-169">Für JavaScript-Client die `invoke` Methode gibt eine [JavaScript-Zusage](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span><span class="sxs-lookup"><span data-stu-id="07db6-169">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="07db6-170">Wenn der Client einen Fehler mit einem Handler empfängt die Zusicherung mit angefügten `catch`, es wurde aufgerufen, und übergeben Sie als JavaScript- `Error` Objekt.</span><span class="sxs-lookup"><span data-stu-id="07db6-170">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a><span data-ttu-id="07db6-171">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="07db6-171">Related resources</span></span>

* [<span data-ttu-id="07db6-172">Einführung in ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="07db6-172">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="07db6-173">JavaScript-Client</span><span class="sxs-lookup"><span data-stu-id="07db6-173">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="07db6-174">Veröffentlichen in Azure</span><span class="sxs-lookup"><span data-stu-id="07db6-174">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
