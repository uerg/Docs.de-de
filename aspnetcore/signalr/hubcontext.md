---
title: SignalR HubContext
author: rachelappel
description: Informationen Sie zum Verwenden des ASP.NET Core SignalR HubContext-Diensts zum Senden von Benachrichtigungen an Clients von außerhalb eines Hubs.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/13/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/hubcontext
ms.openlocfilehash: 79b91a776a38a2e6810cc89ff0b8d15fe836ce66
ms.sourcegitcommit: 9a35906446af7ffd4ccfc18daec38874b5abbef7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2018
ms.locfileid: "35726084"
---
# <a name="send-messages-from-outside-a-hub"></a>Senden von Nachrichten von außerhalb eines Hubs

Durch [Mikael Mengistu](https://twitter.com/MikaelM_12)

Die SignalR-Hubs ist die Core-Abstraktion zum Senden von Nachrichten für Clients, die mit dem SignalR-Server verbunden. Es ist auch möglich, zum Senden von Nachrichten an anderen Stellen in Ihrer app mit dem `IHubContext` Dienst. In diesem Artikel wird erläutert, wie eine SignalR Zugriff `IHubContext` zum Senden von Benachrichtigungen an Clients von außerhalb eines Hubs.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(Gewusst wie: herunterladen)](xref:tutorials/index#how-to-download-a-sample)

## <a name="get-an-instance-of-ihubcontext"></a>Abrufen einer Instanz von `IHubContext`

In ASP.NET Core SignalR, erreichen Sie eine Instanz von `IHubContext` über Abhängigkeitsinjektion. Sie können eine Instanz von einfügen `IHubContext` in einem Controller, Middleware oder anderen DI-Dienst. Verwenden Sie die Instanz zum Senden von Nachrichten für Clients an.

> [!NOTE]
> Dies unterscheidet sich von ASP.NET SignalR die GlobalHost verwendet wird, für den Zugriff auf die `IHubContext`. ASP.NET Core verfügt über ein Dependency Injection-Framework, das die Notwendigkeit für diese globale Singleton entfernt.

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a>Einfügen eine Instanz von `IHubContext` in einem Controller

Sie können eine Instanz von einfügen `IHubContext` in einen Controller aus, indem sie Sie dem Konstruktor hinzufügen:

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

Nun durch Zugriff auf eine Instanz des `IHubContext`, Sie können hubmethoden aufrufen, als wären Sie auf dem Hub selbst.

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a>Rufen Sie eine Instanz des `IHubContext` in Middleware

Zugriff die `IHubContext` innerhalb der middlewarepipeline wie folgt:

```csharp
app.Use(next => (context) =>
{
    var hubContext = (IHubContext<MyHub>)context
                        .RequestServices
                        .GetServices<IHubContext<MyHub>>();
    //...
});
```

> [!NOTE]
> Wenn hubmethoden außerhalb von aus aufgerufen werden der `Hub` Klasse, die keine Aufrufer, den Aufruf zugeordnet ist. Es gibt daher keinen Zugriff auf die `ConnectionId`, `Caller`, und `Others` Eigenschaften.

## <a name="related-resources"></a>Weitere Informationen

* [Erste Schritte](xref:signalr/get-started)
* [Hubs](xref:signalr/hubs)
* [Veröffentlichen in Azure](xref:signalr/publish-to-azure-web-app)
