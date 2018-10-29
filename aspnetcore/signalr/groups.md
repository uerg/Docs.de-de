---
title: Verwalten von Benutzern und Gruppen in SignalR
author: tdykstra
description: Übersicht über ASP.NET Core SignalR Benutzer-und Gruppenverwaltung.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/groups
ms.openlocfilehash: d3e580dfc42a36762358899892831c8b68f544b0
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207159"
---
# <a name="manage-users-and-groups-in-signalr"></a><span data-ttu-id="64905-103">Verwalten von Benutzern und Gruppen in SignalR</span><span class="sxs-lookup"><span data-stu-id="64905-103">Manage users and groups in SignalR</span></span>

<span data-ttu-id="64905-104">Durch [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="64905-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="64905-105">SignalR ermöglicht, Nachrichten an alle Verbindungen, die einen bestimmten Benutzer zugeordneten gesendet werden, sowie um benannte Gruppen von Verbindungen.</span><span class="sxs-lookup"><span data-stu-id="64905-105">SignalR allows messages to be sent to all connections associated with a specific user, as well as to named groups of connections.</span></span>

<span data-ttu-id="64905-106">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(Herunterladen von)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="64905-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="users-in-signalr"></a><span data-ttu-id="64905-107">Benutzer in SignalR</span><span class="sxs-lookup"><span data-stu-id="64905-107">Users in SignalR</span></span>

<span data-ttu-id="64905-108">SignalR können Sie zum Senden von Nachrichten für alle Verbindungen, die einen bestimmten Benutzer zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="64905-108">SignalR allows you to send messages to all connections associated with a specific user.</span></span> <span data-ttu-id="64905-109">SignalR verwendet standardmäßig die `ClaimTypes.NameIdentifier` aus der `ClaimsPrincipal` der Verbindung als die Benutzer-ID zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="64905-109">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> <span data-ttu-id="64905-110">Ein einzelner Benutzer kann mehrere Verbindungen mit einer SignalR-app haben.</span><span class="sxs-lookup"><span data-stu-id="64905-110">A single user can have multiple connections to a SignalR app.</span></span> <span data-ttu-id="64905-111">Beispielsweise kann ein Benutzer auf ihrem Desktop als auch ihre Rufnummer bestehen.</span><span class="sxs-lookup"><span data-stu-id="64905-111">For example, a user could be connected on their desktop as well as their phone.</span></span> <span data-ttu-id="64905-112">Jedes Gerät verfügt über eine separate SignalR-Verbindung, aber sie alle mit demselben Benutzer zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="64905-112">Each device has a separate SignalR connection, but they're all associated with the same user.</span></span> <span data-ttu-id="64905-113">Wenn eine Nachricht an den Benutzer gesendet wird, empfangen alle Verbindungen mit diesem Benutzer verknüpften der Nachricht.</span><span class="sxs-lookup"><span data-stu-id="64905-113">If a message is sent to the user, all of the connections associated with that user receive the message.</span></span> <span data-ttu-id="64905-114">Die Benutzer-ID für eine Verbindung zugegriffen werden kann, indem die `Context.UserIdentifier` Eigenschaft in Ihrem Hub.</span><span class="sxs-lookup"><span data-stu-id="64905-114">The user identifier for a connection can be accessed by the `Context.UserIdentifier` property in your hub.</span></span>

<span data-ttu-id="64905-115">Senden einer Nachricht an einen bestimmten Benutzer durch die Benutzer-ID zum Übergeben der `User` Funktion in der hubmethode, wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="64905-115">Send a message to a specific user by passing the user identifier to the `User` function in your hub method as shown in the following example:</span></span>

> [!NOTE]
> <span data-ttu-id="64905-116">Die Benutzer-ID wird Groß-/Kleinschreibung beachtet.</span><span class="sxs-lookup"><span data-stu-id="64905-116">The user identifier is case-sensitive.</span></span>

```csharp
public Task SendPrivateMessage(string user, string message)
{
    return Clients.User(user).SendAsync("ReceiveMessage", message);
}
```

<span data-ttu-id="64905-117">Die Benutzer-ID kann angepasst werden, durch das Erstellen einer `IUserIdProvider`, und registrieren ihn in `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="64905-117">The user identifier can be customized by creating an `IUserIdProvider`, and registering it in `ConfigureServices`.</span></span>

[!code-csharp[UserIdProvider](groups/sample/customuseridprovider.cs?range=4-10)]

[!code-csharp[Configure service](groups/sample/startup.cs?range=21-22,39-42)]

> [!NOTE]
> <span data-ttu-id="64905-118">AddSignalR muss vor der Registrierung Ihrer benutzerdefinierten SignalR-Dienste aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="64905-118">AddSignalR must be called before registering your custom SignalR services.</span></span>

## <a name="groups-in-signalr"></a><span data-ttu-id="64905-119">Gruppen in SignalR</span><span class="sxs-lookup"><span data-stu-id="64905-119">Groups in SignalR</span></span>

<span data-ttu-id="64905-120">Eine Gruppe ist eine Auflistung von Verbindungen mit einem Namen zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="64905-120">A group is a collection of connections associated with a name.</span></span> <span data-ttu-id="64905-121">Nachrichten können für alle Verbindungen in einer Gruppe gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="64905-121">Messages can be sent to all connections in a group.</span></span> <span data-ttu-id="64905-122">Gruppen sind die empfohlene Vorgehensweise, um eine Verbindung oder mehrere Verbindungen gesendet werden, da es sich bei die Gruppen von der Anwendung verwaltet werden.</span><span class="sxs-lookup"><span data-stu-id="64905-122">Groups are the recommended way to send to a connection or multiple connections because the groups are managed by the application.</span></span> <span data-ttu-id="64905-123">Eine Verbindung kann Mitglied mehrerer Gruppen sein.</span><span class="sxs-lookup"><span data-stu-id="64905-123">A connection can be a member of multiple groups.</span></span> <span data-ttu-id="64905-124">Dadurch ist Gruppen ideal für Anwendungen wie eine Chat-Anwendung, in dem jeweiligen Raums kann als eine Gruppe dargestellt werden.</span><span class="sxs-lookup"><span data-stu-id="64905-124">This makes groups ideal for something like a chat application, where each room can be represented as a group.</span></span> <span data-ttu-id="64905-125">Verbindungen hinzugefügt oder aus Gruppen über entfernt werden können, die `AddToGroupAsync` und `RemoveFromGroupAsync` Methoden.</span><span class="sxs-lookup"><span data-stu-id="64905-125">Connections can be added to or removed from groups via the `AddToGroupAsync` and `RemoveFromGroupAsync` methods.</span></span>

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

<span data-ttu-id="64905-126">Der Gruppenmitgliedschaft wird nicht beibehalten, wenn eine Verbindung erneut hergestellt.</span><span class="sxs-lookup"><span data-stu-id="64905-126">Group membership isn't preserved when a connection reconnects.</span></span> <span data-ttu-id="64905-127">Die Verbindung muss die Gruppe erneut beitreten, wenn er erneut hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="64905-127">The connection needs to rejoin the group when it's re-established.</span></span> <span data-ttu-id="64905-128">Es ist nicht möglich, um die Mitglieder einer Gruppe zu zählen, da diese Informationen sind nicht verfügbar, wenn die Anwendung auf mehreren Servern skaliert wird.</span><span class="sxs-lookup"><span data-stu-id="64905-128">It's not possible to count the members of a group, since this information is not available if the application is scaled to multiple servers.</span></span>

> [!NOTE]
> <span data-ttu-id="64905-129">Gruppennamen werden Groß-/Kleinschreibung beachtet.</span><span class="sxs-lookup"><span data-stu-id="64905-129">Group names are case-sensitive.</span></span>

## <a name="related-resources"></a><span data-ttu-id="64905-130">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="64905-130">Related resources</span></span>

* [<span data-ttu-id="64905-131">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="64905-131">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="64905-132">Hubs</span><span class="sxs-lookup"><span data-stu-id="64905-132">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="64905-133">Veröffentlichen in Azure</span><span class="sxs-lookup"><span data-stu-id="64905-133">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
