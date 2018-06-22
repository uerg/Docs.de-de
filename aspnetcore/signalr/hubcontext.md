---
title: SignalR HubContext
author: rachelappel
description: Informationen Sie zum Verwenden des ASP.NET Core SignalR HubContext-Diensts zum Senden von Benachrichtigungen an Clients von außerhalb eines Hubs.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/13/2018
uid: signalr/hubcontext
ms.openlocfilehash: ccfcdc8337275fd26e09c1a43db36cf9ab90cf46
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277760"
---
# <a name="send-messages-from-outside-a-hub"></a><span data-ttu-id="cbbdc-103">Senden von Nachrichten von außerhalb eines Hubs</span><span class="sxs-lookup"><span data-stu-id="cbbdc-103">Send messages from outside a hub</span></span>

<span data-ttu-id="cbbdc-104">Durch [Mikael Mengistu](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="cbbdc-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="cbbdc-105">Die SignalR-Hubs ist die Core-Abstraktion zum Senden von Nachrichten für Clients, die mit dem SignalR-Server verbunden.</span><span class="sxs-lookup"><span data-stu-id="cbbdc-105">The SignalR hub is the core abstraction for sending messages to clients connected to the SignalR server.</span></span> <span data-ttu-id="cbbdc-106">Es ist auch möglich, zum Senden von Nachrichten an anderen Stellen in Ihrer app mit dem `IHubContext` Dienst.</span><span class="sxs-lookup"><span data-stu-id="cbbdc-106">It's also possible to send messages from other places in your app using the `IHubContext` service.</span></span> <span data-ttu-id="cbbdc-107">In diesem Artikel wird erläutert, wie eine SignalR Zugriff `IHubContext` zum Senden von Benachrichtigungen an Clients von außerhalb eines Hubs.</span><span class="sxs-lookup"><span data-stu-id="cbbdc-107">This article explains how to access a SignalR `IHubContext` to send notifications to clients from outside a hub.</span></span>

<span data-ttu-id="cbbdc-108">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(Gewusst wie: herunterladen)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="cbbdc-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="get-an-instance-of-ihubcontext"></a><span data-ttu-id="cbbdc-109">Abrufen einer Instanz von `IHubContext`</span><span class="sxs-lookup"><span data-stu-id="cbbdc-109">Get an instance of `IHubContext`</span></span>

<span data-ttu-id="cbbdc-110">In ASP.NET Core SignalR, erreichen Sie eine Instanz von `IHubContext` über Abhängigkeitsinjektion.</span><span class="sxs-lookup"><span data-stu-id="cbbdc-110">In ASP.NET Core SignalR, you can access an instance of `IHubContext` via dependency injection.</span></span> <span data-ttu-id="cbbdc-111">Sie können eine Instanz von einfügen `IHubContext` in einem Controller, Middleware oder anderen DI-Dienst.</span><span class="sxs-lookup"><span data-stu-id="cbbdc-111">You can inject an instance of `IHubContext` into a controller, middleware, or other DI service.</span></span> <span data-ttu-id="cbbdc-112">Verwenden Sie die Instanz zum Senden von Nachrichten für Clients an.</span><span class="sxs-lookup"><span data-stu-id="cbbdc-112">Use the instance to send messages to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="cbbdc-113">Dies unterscheidet sich von ASP.NET SignalR die GlobalHost verwendet wird, für den Zugriff auf die `IHubContext`.</span><span class="sxs-lookup"><span data-stu-id="cbbdc-113">This differs from ASP.NET SignalR which used GlobalHost to provide access to the `IHubContext`.</span></span> <span data-ttu-id="cbbdc-114">ASP.NET Core verfügt über ein Dependency Injection-Framework, das die Notwendigkeit für diese globale Singleton entfernt.</span><span class="sxs-lookup"><span data-stu-id="cbbdc-114">ASP.NET Core has a dependency injection framework that removes the need for this global singleton.</span></span>

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a><span data-ttu-id="cbbdc-115">Einfügen eine Instanz von `IHubContext` in einem Controller</span><span class="sxs-lookup"><span data-stu-id="cbbdc-115">Inject an instance of `IHubContext` in a controller</span></span>

<span data-ttu-id="cbbdc-116">Sie können eine Instanz von einfügen `IHubContext` in einen Controller aus, indem sie Sie dem Konstruktor hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="cbbdc-116">You can inject an instance of `IHubContext` into a controller by adding it to your constructor:</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

<span data-ttu-id="cbbdc-117">Nun durch Zugriff auf eine Instanz des `IHubContext`, Sie können hubmethoden aufrufen, als wären Sie auf dem Hub selbst.</span><span class="sxs-lookup"><span data-stu-id="cbbdc-117">Now, with access to an instance of `IHubContext`, you can call hub methods as if you were in the hub itself.</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a><span data-ttu-id="cbbdc-118">Rufen Sie eine Instanz des `IHubContext` in Middleware</span><span class="sxs-lookup"><span data-stu-id="cbbdc-118">Get an instance of `IHubContext` in middleware</span></span>

<span data-ttu-id="cbbdc-119">Zugriff die `IHubContext` innerhalb der middlewarepipeline wie folgt:</span><span class="sxs-lookup"><span data-stu-id="cbbdc-119">Access the `IHubContext` within the middleware pipeline like so:</span></span>

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
> <span data-ttu-id="cbbdc-120">Wenn hubmethoden außerhalb von aus aufgerufen werden der `Hub` Klasse, die keine Aufrufer, den Aufruf zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="cbbdc-120">When hub methods are called from outside of the `Hub` class, there's no caller associated with the invocation.</span></span> <span data-ttu-id="cbbdc-121">Es gibt daher keinen Zugriff auf die `ConnectionId`, `Caller`, und `Others` Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="cbbdc-121">Therefore, there's no access to the `ConnectionId`, `Caller`, and `Others` properties.</span></span>

## <a name="related-resources"></a><span data-ttu-id="cbbdc-122">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="cbbdc-122">Related resources</span></span>

* [<span data-ttu-id="cbbdc-123">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="cbbdc-123">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="cbbdc-124">Hubs</span><span class="sxs-lookup"><span data-stu-id="cbbdc-124">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="cbbdc-125">Veröffentlichen in Azure</span><span class="sxs-lookup"><span data-stu-id="cbbdc-125">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
