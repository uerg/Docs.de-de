---
title: Verwalten von Benutzern und Gruppen in SignalR
author: rachelappel
description: Übersicht über ASP.NET Core SignalR Benutzer-und Gruppenverwaltung.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/groups
ms.openlocfilehash: f7d60a906fc238f79c76fd2a4ee693417a348825
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272080"
---
# <a name="manage-users-and-groups-in-signalr"></a>Verwalten von Benutzern und Gruppen in SignalR

Durch [Brennan Conroy](https://github.com/BrennanConroy)

SignalR kann Nachrichten an alle Verbindungen, die einem bestimmten Benutzer zugeordnet gesendet werden, als auch benannte Gruppen von Verbindungen.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(Gewusst wie: herunterladen)](xref:tutorials/index#how-to-download-a-sample)

## <a name="users-in-signalr"></a>Benutzer, die in SignalR

SignalR bietet die Möglichkeit zum Senden von Nachrichten an alle Verbindungen, die einem bestimmten Benutzer zugeordnet. SignalR verwendet standardmäßig die `ClaimTypes.NameIdentifier` aus der `ClaimsPrincipal` der Verbindung mit der die Benutzer-ID zugeordnet. Ein einzelner Benutzer kann mehrere Verbindungen zu einer SignalR-Anwendung hat. Beispielsweise kann ein Benutzer auf Ihrem eigenen Schreibtisch als auch seinen Anschluss verbunden sein. Jedes Gerät verfügt über eine separate SignalR-Verbindung, aber sie sind alle mit dem gleichen Benutzerkonto verknüpft. Wenn eine Nachricht an den Benutzer gesendet wird, erhalten alle Verbindungen mit diesem Benutzer verknüpften die Nachricht.

Senden einer Nachricht mit einem bestimmten Benutzer durch die Benutzer-ID zum Übergeben der `User` Funktion in der hubmethode, wie im folgenden Beispiel gezeigt:

> [!NOTE]
> Der Bezeichner des Benutzers wird die Groß-/Kleinschreibung beachtet.

```csharp
public Task SendPrivateMessage(string user, string message)
{
    return Clients.User(user).SendAsync("ReceiveMessage", message);
}
```

Der Bezeichner des Benutzers kann angepasst werden, durch das Erstellen einer `IUserIdProvider`, und registrieren ihn in `ConfigureServices`.

[!code-csharp[UserIdProvider](groups/sample/customuseridprovider.cs?range=4-10)]

[!code-csharp[Configure service](groups/sample/startup.cs?range=21-22,39-42)]

> [!NOTE]
> AddSignalR muss aufgerufen werden, bevor Sie Ihre benutzerdefinierte SignalR-Dienste registrieren.

## <a name="groups-in-signalr"></a>Gruppen in SignalR

Eine Gruppe ist eine Auflistung von Verbindungen mit einem Namen verknüpft sind. Nachrichten können für alle Verbindungen in einer Gruppe gesendet werden. Gruppen sind die empfohlene Methode zum an eine oder mehrere Verbindungen zu senden, da die Gruppen von der Anwendung verwaltet werden. Eine Verbindung kann Mitglied mehrerer Gruppen sein. Dadurch Gruppen ideal für etwa eine Chat-Anwendung, in dem jeweiligen als Gruppe dargestellt werden. Verbindungen können hinzugefügt oder entfernt Sie aus Gruppen über die `AddToGroupAsync` und `RemoveFromGroupAsync` Methoden.

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

Der Gruppenmitgliedschaft wird nicht beibehalten, wenn eine Verbindung erneut hergestellt. Die Verbindung muss die Gruppe erneut beitreten, wenn er erneut hergestellt wird. Es ist nicht möglich, um die Mitglieder einer Gruppe zu zählen, da diese Informationen sind nicht verfügbar, wenn die Anwendung mit mehreren Servern skaliert wird.

> [!NOTE]
> Die Namen von Groß-/Kleinschreibung beachtet.

## <a name="related-resources"></a>Weitere Informationen

* [Erste Schritte](xref:tutorials/signalr)
* [Hubs](xref:signalr/hubs)
* [Veröffentlichen in Azure](xref:signalr/publish-to-azure-web-app)
