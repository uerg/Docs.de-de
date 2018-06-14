---
title: Verwalten von Benutzern und Gruppen in SignalR
author: rachelappel
description: Übersicht über ASP.NET Core SignalR Benutzer-und Gruppenverwaltung.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/groups
ms.openlocfilehash: 2a2f129863cf7d5cdfa3c0e5b2d901af52144671
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/12/2018
ms.locfileid: "35358438"
---
# <a name="manage-users-and-groups-in-signalr"></a><span data-ttu-id="31467-103">Verwalten von Benutzern und Gruppen in SignalR</span><span class="sxs-lookup"><span data-stu-id="31467-103">Manage users and groups in SignalR</span></span>

<span data-ttu-id="31467-104">Durch [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="31467-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="31467-105">SignalR kann Nachrichten an alle Verbindungen, die einem bestimmten Benutzer zugeordnet gesendet werden, als auch benannte Gruppen von Verbindungen.</span><span class="sxs-lookup"><span data-stu-id="31467-105">SignalR allows messages to be sent to all connections associated with a specific user, as well as to named groups of connections.</span></span>

<span data-ttu-id="31467-106">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(Gewusst wie: herunterladen)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="31467-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="users-in-signalr"></a><span data-ttu-id="31467-107">Benutzer, die in SignalR</span><span class="sxs-lookup"><span data-stu-id="31467-107">Users in SignalR</span></span>

<span data-ttu-id="31467-108">SignalR bietet die Möglichkeit zum Senden von Nachrichten an alle Verbindungen, die einem bestimmten Benutzer zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="31467-108">SignalR allows you to send messages to all connections associated with a specific user.</span></span> <span data-ttu-id="31467-109">SignalR verwendet standardmäßig die `ClaimTypes.NameIdentifier` aus der `ClaimsPrincipal` der Verbindung mit der die Benutzer-ID zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="31467-109">By default SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> <span data-ttu-id="31467-110">Ein einzelner Benutzer kann mehrere Verbindungen zu einer SignalR-Anwendung hat.</span><span class="sxs-lookup"><span data-stu-id="31467-110">A single user can have multiple connections to a SignalR application.</span></span> <span data-ttu-id="31467-111">Beispielsweise kann ein Benutzer auf Ihrem eigenen Schreibtisch als auch seinen Anschluss verbunden sein.</span><span class="sxs-lookup"><span data-stu-id="31467-111">For example, a user could be connected on their desktop as well as their phone.</span></span> <span data-ttu-id="31467-112">Jedes Gerät verfügt über eine separate SignalR-Verbindung, aber sie sind alle mit dem gleichen Benutzerkonto verknüpft.</span><span class="sxs-lookup"><span data-stu-id="31467-112">Each device has a separate SignalR connection, but they are all associated with the same user.</span></span> <span data-ttu-id="31467-113">Wenn eine Nachricht an den Benutzer gesendet wird, erhalten alle Verbindungen mit diesem Benutzer verknüpften die Nachricht.</span><span class="sxs-lookup"><span data-stu-id="31467-113">If a message is sent to the user, all of the connections associated with that user will receive the message.</span></span>

<span data-ttu-id="31467-114">Senden einer Nachricht mit einem bestimmten Benutzer durch die Benutzer-ID zum Übergeben der `User` Funktion in der hubmethode, wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="31467-114">Send a message to a specific user by passing the user identifier to the `User` function in your hub method as shown in the following example:</span></span>

> [!NOTE]
> <span data-ttu-id="31467-115">Der Bezeichner des Benutzers wird die Groß-/Kleinschreibung beachtet.</span><span class="sxs-lookup"><span data-stu-id="31467-115">The user identifier is case-sensitive.</span></span>

```csharp
public Task SendPrivateMessage(string user, string message)
{
    return Clients.User(user).SendAsync("ReceiveMessage", message);
}
```

<span data-ttu-id="31467-116">Der Bezeichner des Benutzers kann angepasst werden, durch das Erstellen einer `IUserIdProvider`, und registrieren ihn in `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="31467-116">The user identifier can be customized by creating an `IUserIdProvider`, and registering it in `ConfigureServices`.</span></span>

[!code-csharp[UserIdProvider](groups/sample/customuseridprovider.cs?range=4-10)]

[!code-csharp[Configure service](groups/sample/startup.cs?range=21-22,39-42)]

> [!NOTE]
> <span data-ttu-id="31467-117">AddSignalR muss aufgerufen werden, bevor Sie Ihre benutzerdefinierte SignalR-Dienste registrieren.</span><span class="sxs-lookup"><span data-stu-id="31467-117">AddSignalR must be called before registering your custom SignalR services.</span></span>

## <a name="groups-in-signalr"></a><span data-ttu-id="31467-118">Gruppen in SignalR</span><span class="sxs-lookup"><span data-stu-id="31467-118">Groups in SignalR</span></span>

<span data-ttu-id="31467-119">Eine Gruppe ist eine Auflistung von Verbindungen mit einem Namen verknüpft sind.</span><span class="sxs-lookup"><span data-stu-id="31467-119">A group is a collection of connections associated with a name.</span></span> <span data-ttu-id="31467-120">Nachrichten können für alle Verbindungen in einer Gruppe gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="31467-120">Messages can be sent to all connections in a group.</span></span> <span data-ttu-id="31467-121">Gruppen sind die empfohlene Methode zum an eine oder mehrere Verbindungen zu senden, da die Gruppen von der Anwendung verwaltet werden.</span><span class="sxs-lookup"><span data-stu-id="31467-121">Groups are the recommended way to send to a connection or multiple connections because the groups are managed by the application.</span></span> <span data-ttu-id="31467-122">Eine Verbindung kann Mitglied mehrerer Gruppen sein.</span><span class="sxs-lookup"><span data-stu-id="31467-122">A connection can be a member of multiple groups.</span></span> <span data-ttu-id="31467-123">Dadurch Gruppen ideal für etwa eine Chat-Anwendung, in dem jeweiligen als Gruppe dargestellt werden.</span><span class="sxs-lookup"><span data-stu-id="31467-123">This makes groups ideal for something like a chat application, where each room can be represented as a group.</span></span> <span data-ttu-id="31467-124">Verbindungen können hinzugefügt oder entfernt Sie aus Gruppen über die `AddToGroupAsync` und `RemoveFromGroupAsync` Methoden.</span><span class="sxs-lookup"><span data-stu-id="31467-124">Connections can be added to or removed from groups via the `AddToGroupAsync` and `RemoveFromGroupAsync` methods.</span></span>

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

<span data-ttu-id="31467-125">Der Gruppenmitgliedschaft wird nicht beibehalten, wenn eine Verbindung erneut hergestellt.</span><span class="sxs-lookup"><span data-stu-id="31467-125">Group membership isn't preserved when a connection reconnects.</span></span> <span data-ttu-id="31467-126">Die Verbindung muss die Gruppe erneut beitreten, wenn er erneut hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="31467-126">The connection needs to rejoin the group when it's re-established.</span></span> <span data-ttu-id="31467-127">Es ist nicht möglich, um die Mitglieder einer Gruppe zu zählen, da diese Informationen sind nicht verfügbar, wenn die Anwendung mit mehreren Servern skaliert wird.</span><span class="sxs-lookup"><span data-stu-id="31467-127">It's not possible to count the members of a group, since this information is not available if the application is scaled to multiple servers.</span></span>

> [!NOTE]
> <span data-ttu-id="31467-128">Die Namen von Groß-/Kleinschreibung beachtet.</span><span class="sxs-lookup"><span data-stu-id="31467-128">Group names are case-sensitive.</span></span>

## <a name="related-resources"></a><span data-ttu-id="31467-129">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="31467-129">Related resources</span></span>

* [<span data-ttu-id="31467-130">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="31467-130">Get started</span></span>](xref:signalr/get-started)
* [<span data-ttu-id="31467-131">Hubs</span><span class="sxs-lookup"><span data-stu-id="31467-131">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="31467-132">Veröffentlichen in Azure</span><span class="sxs-lookup"><span data-stu-id="31467-132">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
