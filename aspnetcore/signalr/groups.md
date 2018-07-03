---
title: Verwalten von Benutzern und Gruppen in SignalR
author: rachelappel
description: Übersicht über ASP.NET Core SignalR Benutzer-und Gruppenverwaltung.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/groups
ms.openlocfilehash: 3e5e310c84bc3ed5790d5b67a917bd54162ea163
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402524"
---
# <a name="manage-users-and-groups-in-signalr"></a>Verwalten von Benutzern und Gruppen in SignalR

Durch [Brennan Conroy](https://github.com/BrennanConroy)

SignalR ermöglicht, Nachrichten an alle Verbindungen, die einen bestimmten Benutzer zugeordneten gesendet werden, sowie um benannte Gruppen von Verbindungen.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(Herunterladen von)](xref:tutorials/index#how-to-download-a-sample)

## <a name="users-in-signalr"></a>Benutzer in SignalR

SignalR können Sie zum Senden von Nachrichten für alle Verbindungen, die einen bestimmten Benutzer zugeordnet. SignalR verwendet standardmäßig die `ClaimTypes.NameIdentifier` aus der `ClaimsPrincipal` der Verbindung als die Benutzer-ID zugeordnet. Ein einzelner Benutzer kann mehrere Verbindungen mit einer SignalR-app haben. Beispielsweise kann ein Benutzer auf ihrem Desktop als auch ihre Rufnummer bestehen. Jedes Gerät verfügt über eine separate SignalR-Verbindung, aber sie alle mit demselben Benutzer zugeordnet sind. Wenn eine Nachricht an den Benutzer gesendet wird, empfangen alle Verbindungen mit diesem Benutzer verknüpften der Nachricht. Die Benutzer-ID für eine Verbindung zugegriffen werden kann, indem die `Context.UserIdentifier` Eigenschaft in Ihrem Hub.

Senden einer Nachricht an einen bestimmten Benutzer durch die Benutzer-ID zum Übergeben der `User` Funktion in der hubmethode, wie im folgenden Beispiel gezeigt:

> [!NOTE]
> Die Benutzer-ID wird Groß-/Kleinschreibung beachtet.

```csharp
public Task SendPrivateMessage(string user, string message)
{
    return Clients.User(user).SendAsync("ReceiveMessage", message);
}
```

Die Benutzer-ID kann angepasst werden, durch das Erstellen einer `IUserIdProvider`, und registrieren ihn in `ConfigureServices`.

[!code-csharp[UserIdProvider](groups/sample/customuseridprovider.cs?range=4-10)]

[!code-csharp[Configure service](groups/sample/startup.cs?range=21-22,39-42)]

> [!NOTE]
> AddSignalR muss vor der Registrierung Ihrer benutzerdefinierten SignalR-Dienste aufgerufen werden.

## <a name="groups-in-signalr"></a>Gruppen in SignalR

Eine Gruppe ist eine Auflistung von Verbindungen mit einem Namen zugeordnet. Nachrichten können für alle Verbindungen in einer Gruppe gesendet werden. Gruppen sind die empfohlene Vorgehensweise, um eine Verbindung oder mehrere Verbindungen gesendet werden, da es sich bei die Gruppen von der Anwendung verwaltet werden. Eine Verbindung kann Mitglied mehrerer Gruppen sein. Dadurch ist Gruppen ideal für Anwendungen wie eine Chat-Anwendung, in dem jeweiligen Raums kann als eine Gruppe dargestellt werden. Verbindungen hinzugefügt oder aus Gruppen über entfernt werden können, die `AddToGroupAsync` und `RemoveFromGroupAsync` Methoden.

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

Der Gruppenmitgliedschaft wird nicht beibehalten, wenn eine Verbindung erneut hergestellt. Die Verbindung muss die Gruppe erneut beitreten, wenn er erneut hergestellt wird. Es ist nicht möglich, um die Mitglieder einer Gruppe zu zählen, da diese Informationen sind nicht verfügbar, wenn die Anwendung auf mehreren Servern skaliert wird.

> [!NOTE]
> Gruppennamen werden Groß-/Kleinschreibung beachtet.

## <a name="related-resources"></a>Weitere Informationen

* [Erste Schritte](xref:tutorials/signalr)
* [Hubs](xref:signalr/hubs)
* [Veröffentlichen in Azure](xref:signalr/publish-to-azure-web-app)
