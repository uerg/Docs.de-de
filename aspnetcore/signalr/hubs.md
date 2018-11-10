---
title: Verwenden von Hubs in ASP.NET Core SignalR
author: tdykstra
description: Erfahren Sie, wie Sie mit Hubs in ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/12/2018
uid: signalr/hubs
ms.openlocfilehash: 27aedc5b2f2060d961070fbd1ff5304eaa3956d1
ms.sourcegitcommit: fc7eb4243188950ae1f1b52669edc007e9d0798d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51225355"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="b6e7e-103">Verwenden von Hubs in SignalR für ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b6e7e-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="b6e7e-104">Durch [Rachel Appel](https://twitter.com/rachelappel) und [Kevin Griffin](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="b6e7e-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

<span data-ttu-id="b6e7e-105">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(Herunterladen von)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="b6e7e-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="b6e7e-106">Was ist eine SignalR-Hub</span><span class="sxs-lookup"><span data-stu-id="b6e7e-106">What is a SignalR hub</span></span>

<span data-ttu-id="b6e7e-107">Der SignalR-Hubs-API können Sie zum Aufrufen von Methoden auf verbundene Clients vom Server.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="b6e7e-108">In den Servercode definieren Sie Methoden, die vom Client aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="b6e7e-109">Im Clientcode definieren Sie Methoden, die vom Server aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="b6e7e-110">SignalR kümmert sich um alles, was hinter den Kulissen, die in Echtzeit-Client-zu-Server und Server-zu-Client-Kommunikation möglich macht.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="b6e7e-111">Konfigurieren von SignalR-hubs</span><span class="sxs-lookup"><span data-stu-id="b6e7e-111">Configure SignalR hubs</span></span>

<span data-ttu-id="b6e7e-112">Die SignalR-Middleware ist erforderlich, einige Dienste, die durch den Aufruf konfiguriert werden `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

<span data-ttu-id="b6e7e-113">Beim Hinzufügen von SignalR-Funktionen zu einer ASP.NET Core-app einrichten SignalR-Routen, durch den Aufruf `app.UseSignalR` in die `Startup.Configure` Methode.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="b6e7e-114">Erstellen und Verwenden von hubs</span><span class="sxs-lookup"><span data-stu-id="b6e7e-114">Create and use hubs</span></span>

<span data-ttu-id="b6e7e-115">Erstellen Sie einen Hub durch Deklarieren einer Klasse, die von erbt `Hub`, und fügen Sie öffentliche Methoden hinzu.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-115">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="b6e7e-116">Clients können als definierten Methoden aufrufen `public`.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-116">Clients can call methods that are defined as `public`.</span></span>

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

<span data-ttu-id="b6e7e-117">Sie können einen Rückgabetyp und Parameter, einschließlich von komplexen Typen und Arrays, wie in jeder C#-Methode angeben.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-117">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="b6e7e-118">SignalR verarbeitet die Serialisierung und Deserialisierung von komplexen Objekten und Arrays in Ihre Parameter und Rückgabewerte.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-118">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

> [!NOTE]
> <span data-ttu-id="b6e7e-119">Hubs sind vorübergehend:</span><span class="sxs-lookup"><span data-stu-id="b6e7e-119">Hubs are transient:</span></span>
> * <span data-ttu-id="b6e7e-120">Speichern Sie Status nicht in einer Eigenschaft für die hubklasse.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-120">Don't store state in a property on the hub class.</span></span> <span data-ttu-id="b6e7e-121">Jeder Aufruf der Hub-Methode wird eine neue hubinstanz ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-121">Every hub method call is executed on a new hub instance.</span></span>  
> * <span data-ttu-id="b6e7e-122">Verwendung `await` beim Aufrufen von asynchroner Methoden, die auf dem Hub bleiben aktiv abhängig sind.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-122">Use `await` when calling asynchronous methods that depend on the hub staying alive.</span></span> <span data-ttu-id="b6e7e-123">Zum Beispiel eine Methode, z. B. `Clients.All.SendAsync(...)` kann fehlschlagen, wenn er, ohne aufgerufen wird `await` und die hubmethode abgeschlossen wird, bevor Sie `SendAsync` abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-123">For example, a method such as `Clients.All.SendAsync(...)` can fail if it's called without `await` and the hub method completes before `SendAsync` finishes.</span></span>

## <a name="the-context-object"></a><span data-ttu-id="b6e7e-124">Das Context-Objekt</span><span class="sxs-lookup"><span data-stu-id="b6e7e-124">The Context object</span></span>

<span data-ttu-id="b6e7e-125">Die `Hub` -Klasse verfügt über eine `Context` Eigenschaft, die mit Informationen über die Verbindung die folgenden Eigenschaften enthält:</span><span class="sxs-lookup"><span data-stu-id="b6e7e-125">The `Hub` class has a `Context` property that contains the following properties with information about the connection:</span></span>

| <span data-ttu-id="b6e7e-126">Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="b6e7e-126">Property</span></span> | <span data-ttu-id="b6e7e-127">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="b6e7e-127">Description</span></span> |
| ------ | ----------- |
| `ConnectionId` | <span data-ttu-id="b6e7e-128">Ruft die eindeutige ID für die Verbindung von SignalR zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-128">Gets the unique ID for the connection, assigned by SignalR.</span></span> <span data-ttu-id="b6e7e-129">Eine Verbindungs­id für jede Verbindung ist vorhanden.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-129">There is one connection ID for each connection.</span></span>|
| `UserIdentifier` | <span data-ttu-id="b6e7e-130">Ruft die [Benutzerbezeichner](xref:signalr/groups).</span><span class="sxs-lookup"><span data-stu-id="b6e7e-130">Gets the [user identifier](xref:signalr/groups).</span></span> <span data-ttu-id="b6e7e-131">SignalR verwendet standardmäßig die `ClaimTypes.NameIdentifier` aus der `ClaimsPrincipal` der Verbindung als die Benutzer-ID zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-131">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> |
| `User` | <span data-ttu-id="b6e7e-132">Ruft die `ClaimsPrincipal` mit dem aktuellen Benutzer verknüpft ist.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-132">Gets the `ClaimsPrincipal` associated with the current user.</span></span> |
| `Items` | <span data-ttu-id="b6e7e-133">Ruft eine Auflistung von Schlüssel-Wert, die zum Freigeben von Daten innerhalb des Bereichs dieser Verbindung verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-133">Gets a key/value collection that can be used to share data within the scope of this connection.</span></span> <span data-ttu-id="b6e7e-134">Daten können in dieser Auflistung gespeichert werden, und bleiben für die Verbindung in anderen hubmethodenaufrufe.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-134">Data can be stored in this collection and it will persist for the connection across different hub method invocations.</span></span> |
| `Features` | <span data-ttu-id="b6e7e-135">Ruft die Auflistung der verfügbaren Funktionen für die Verbindung ab.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-135">Gets the collection of features available on the connection.</span></span> <span data-ttu-id="b6e7e-136">Vorerst ist nicht dieser Auflistung in den meisten Szenarien erforderlich, damit noch ausführlich dokumentiert ist nicht.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-136">For now, this collection isn't needed in most scenarios, so it isn't documented in detail yet.</span></span> |
| `ConnectionAborted` | <span data-ttu-id="b6e7e-137">Ruft eine `CancellationToken` , die benachrichtigt werden, wenn die Verbindung abgebrochen wird.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-137">Gets a `CancellationToken` that notifies when the connection is aborted.</span></span> |

<span data-ttu-id="b6e7e-138">`Hub.Context` Außerdem enthält die folgenden Methoden:</span><span class="sxs-lookup"><span data-stu-id="b6e7e-138">`Hub.Context` also contains the following methods:</span></span>

| <span data-ttu-id="b6e7e-139">Methode</span><span class="sxs-lookup"><span data-stu-id="b6e7e-139">Method</span></span> | <span data-ttu-id="b6e7e-140">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="b6e7e-140">Description</span></span> |
| ------ | ----------- |
| `GetHttpContext` | <span data-ttu-id="b6e7e-141">Gibt die `HttpContext` für die Verbindung oder `null` , wenn die Verbindung nicht mit einer HTTP-Anforderung verknüpft ist.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-141">Returns the `HttpContext` for the connection, or `null` if the connection is not associated with an HTTP request.</span></span> <span data-ttu-id="b6e7e-142">Für HTTP-Verbindungen können Sie diese Methode zum Abrufen von Informationen wie z. B. HTTP-Header und Abfragezeichenfolgen.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-142">For HTTP connections, you can use this method to get information such as HTTP headers and query strings.</span></span> |
| `Abort` | <span data-ttu-id="b6e7e-143">Bricht die Verbindung ab.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-143">Aborts the connection.</span></span> |

## <a name="the-clients-object"></a><span data-ttu-id="b6e7e-144">Das Objekt des Clients</span><span class="sxs-lookup"><span data-stu-id="b6e7e-144">The Clients object</span></span>

<span data-ttu-id="b6e7e-145">Die `Hub` -Klasse verfügt über eine `Clients` -Eigenschaft, die folgenden Eigenschaften für die Kommunikation zwischen Server und Client enthält:</span><span class="sxs-lookup"><span data-stu-id="b6e7e-145">The `Hub` class has a `Clients` property that contains the following properties for communication between server and client:</span></span>

| <span data-ttu-id="b6e7e-146">Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="b6e7e-146">Property</span></span> | <span data-ttu-id="b6e7e-147">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="b6e7e-147">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="b6e7e-148">Ruft eine Methode für alle verbundenen clients</span><span class="sxs-lookup"><span data-stu-id="b6e7e-148">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="b6e7e-149">Ruft eine Methode auf dem Client, der die hubmethode aufgerufen hat.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-149">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="b6e7e-150">Ruft eine Methode für alle verbundenen Clients mit Ausnahme des Clients, die die Methode aufgerufen hat.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-150">Calls a method on all connected clients except the client that invoked the method</span></span> |


<span data-ttu-id="b6e7e-151">`Hub.Clients` Außerdem enthält die folgenden Methoden:</span><span class="sxs-lookup"><span data-stu-id="b6e7e-151">`Hub.Clients` also contains the following methods:</span></span>

| <span data-ttu-id="b6e7e-152">Methode</span><span class="sxs-lookup"><span data-stu-id="b6e7e-152">Method</span></span> | <span data-ttu-id="b6e7e-153">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="b6e7e-153">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="b6e7e-154">Ruft eine Methode für alle verbundenen Clients mit Ausnahme der angegebenen Verbindungen</span><span class="sxs-lookup"><span data-stu-id="b6e7e-154">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="b6e7e-155">Ruft eine Methode auf einem bestimmten verbundenen client</span><span class="sxs-lookup"><span data-stu-id="b6e7e-155">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="b6e7e-156">Ruft eine Methode für bestimmte verbundene clients</span><span class="sxs-lookup"><span data-stu-id="b6e7e-156">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="b6e7e-157">Ruft eine Methode für alle Verbindungen in der angegebenen Gruppe</span><span class="sxs-lookup"><span data-stu-id="b6e7e-157">Calls a method to all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="b6e7e-158">Ruft eine Methode für alle Verbindungen in der angegebenen Gruppe an, mit Ausnahme der angegebenen Verbindungen.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-158">Calls a method to all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="b6e7e-159">Ruft eine Methode in mehrere Gruppen von Verbindungen</span><span class="sxs-lookup"><span data-stu-id="b6e7e-159">Calls a method to multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="b6e7e-160">Ruft eine Methode für eine Gruppe von Verbindungen mit Ausnahme des Clients, der die hubmethode aufgerufen hat</span><span class="sxs-lookup"><span data-stu-id="b6e7e-160">Calls a method to a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="b6e7e-161">Ruft eine Methode, um alle Verbindungen, die einen bestimmten Benutzer zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-161">Calls a method to all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="b6e7e-162">Ruft eine Methode, um alle Verbindungen, die die angegebenen Benutzer zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-162">Calls a method to all connections associated with the specified users</span></span> |

<span data-ttu-id="b6e7e-163">Jede Eigenschaft oder Methode in den vorhergehenden Tabellen gibt ein Objekt mit einem `SendAsync` Methode.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-163">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="b6e7e-164">Die `SendAsync` Methode können Sie den Namen und Parameter, der die Methode aufrufen, angeben.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-164">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="b6e7e-165">Senden von Nachrichten an clients</span><span class="sxs-lookup"><span data-stu-id="b6e7e-165">Send messages to clients</span></span>

<span data-ttu-id="b6e7e-166">Um Aufrufe von bestimmten Clients zu machen, verwenden Sie die Eigenschaften der `Clients` Objekt.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-166">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="b6e7e-167">Im folgenden Beispiel die `SendMessageToCaller` -Methode demonstriert, Senden einer Nachricht an die Verbindung, die die hubmethode aufgerufen hat.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-167">In the following example, the `SendMessageToCaller` method demonstrates sending a message to the connection that invoked the hub method.</span></span> <span data-ttu-id="b6e7e-168">Die `SendMessageToGroups` Methode sendet eine Nachricht an die Gruppen, die in gespeicherten eine `List` mit dem Namen `groups`.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-168">The `SendMessageToGroups` method sends a message to the groups stored in a `List` named `groups`.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="strongly-typed-hubs"></a><span data-ttu-id="b6e7e-169">Stark typisierte hubs</span><span class="sxs-lookup"><span data-stu-id="b6e7e-169">Strongly typed hubs</span></span>

<span data-ttu-id="b6e7e-170">Ein Nachteil der Verwendung von `SendAsync` besteht darin, dass dabei auf magische Zeichenfolge an die Methode aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-170">A drawback of using `SendAsync` is that it relies on a magic string to specify the client method to be called.</span></span> <span data-ttu-id="b6e7e-171">Dadurch wird Code öffnen, um Laufzeitfehler, wenn Sie der Methodennamen falsch geschrieben ist, oder auf dem Client fehlt.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-171">This leaves code open to runtime errors if the method name is misspelled or missing from the client.</span></span>

<span data-ttu-id="b6e7e-172">Eine Alternative zur Verwendung `SendAsync` stark typisiert werden die `Hub` mit <xref:Microsoft.AspNetCore.SignalR.Hub`1>.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-172">An alternative to using `SendAsync` is to strongly type the `Hub` with <xref:Microsoft.AspNetCore.SignalR.Hub`1>.</span></span> <span data-ttu-id="b6e7e-173">Im folgenden Beispiel die `ChatHub` Clientmethoden wurden, in eine Schnittstelle namens extrahiert `IChatClient`.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-173">In the following example, the `ChatHub` client methods have been extracted out into an interface called `IChatClient`.</span></span>  

[!code-csharp[Interface for IChatClient](hubs/sample/hubs/ichatclient.cs?name=snippet_IChatClient)]

<span data-ttu-id="b6e7e-174">Diese Schnittstelle kann verwendet werden, auf der vorherigen Umgestalten `ChatHub` Beispiel.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-174">This interface can be used to refactor the preceding `ChatHub` example.</span></span>

[!code-csharp[Strongly typed ChatHub](hubs/sample/hubs/StronglyTypedChatHub.cs?range=8-18,36)]

<span data-ttu-id="b6e7e-175">Mithilfe von `Hub<IChatClient>` ermöglicht während der Kompilierung der Clientmethoden überprüfen.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-175">Using `Hub<IChatClient>` enables compile-time checking of the client methods.</span></span> <span data-ttu-id="b6e7e-176">Dies verhindert, dass Probleme verursacht werden, da mit der Magic-Zeichenfolgen, `Hub<T>` können nur Zugriff auf die in der Schnittstelle definierten Methoden bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-176">This prevents issues caused by using magic strings, since `Hub<T>` can only provide access to the methods defined in the interface.</span></span>

<span data-ttu-id="b6e7e-177">Verwenden eines stark typisierten `Hub<T>` deaktiviert die Möglichkeit, verwenden Sie `SendAsync`.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-177">Using a strongly typed `Hub<T>` disables the ability to use `SendAsync`.</span></span>

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="b6e7e-178">Behandeln von Ereignissen für eine Verbindung</span><span class="sxs-lookup"><span data-stu-id="b6e7e-178">Handle events for a connection</span></span>

<span data-ttu-id="b6e7e-179">Die SignalR-Hubs-API bietet die `OnConnectedAsync` und `OnDisconnectedAsync` virtuelle Methoden zum Verwalten und Nachverfolgen von Verbindungen.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-179">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="b6e7e-180">Überschreiben der `OnConnectedAsync` virtuelle Methode, um die Aktionen ausführen, wenn einem Client an den Hub zwischen, z. B. zu einer Gruppe hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-180">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a><span data-ttu-id="b6e7e-181">Behandeln von Fehlern</span><span class="sxs-lookup"><span data-stu-id="b6e7e-181">Handle errors</span></span>

<span data-ttu-id="b6e7e-182">In den hubmethoden ausgelöste Ausnahmen werden an den Client gesendet, die die Methode aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-182">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="b6e7e-183">Für JavaScript-Client die `invoke` Methode gibt eine [JavaScript-Zusage](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span><span class="sxs-lookup"><span data-stu-id="b6e7e-183">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="b6e7e-184">Wenn der Client einen Fehler mit einem Handler empfängt die Zusicherung mit angefügten `catch`, es wurde aufgerufen, und übergeben Sie als JavaScript- `Error` Objekt.</span><span class="sxs-lookup"><span data-stu-id="b6e7e-184">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a><span data-ttu-id="b6e7e-185">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="b6e7e-185">Related resources</span></span>

* [<span data-ttu-id="b6e7e-186">Einführung in ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="b6e7e-186">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="b6e7e-187">JavaScript-Client</span><span class="sxs-lookup"><span data-stu-id="b6e7e-187">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="b6e7e-188">Veröffentlichen in Azure</span><span class="sxs-lookup"><span data-stu-id="b6e7e-188">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
