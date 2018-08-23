---
uid: webhooks/receiving/handlers
title: ASP.NET WebHooks Handler | Microsoft-Dokumentation
author: rick-anderson
description: Informationen zur Verarbeitung von Anforderungen in ASP.NET WebHooks.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.openlocfilehash: 01c9a283d105c4a0973ff88c8de646c5f49a34db
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834741"
---
# <a name="aspnet-webhooks-handlers"></a><span data-ttu-id="aa9e4-103">ASP.NET WebHooks-Handler</span><span class="sxs-lookup"><span data-stu-id="aa9e4-103">ASP.NET WebHooks handlers</span></span>

<span data-ttu-id="aa9e4-104">Nach der WebHooks Anforderungen durch einen webhookempfänger bestätigt wurde, ist es für die Verarbeitung von Benutzercode bereit.</span><span class="sxs-lookup"><span data-stu-id="aa9e4-104">Once WebHooks requests has been validated by a WebHook receiver, it is ready to be processed by user code.</span></span> <span data-ttu-id="aa9e4-105">Hier kommt *Handler* eingehen.</span><span class="sxs-lookup"><span data-stu-id="aa9e4-105">This is where *handlers* come in.</span></span> <span data-ttu-id="aa9e4-106">Handler zu leiten, von der [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) -Schnittstelle, aber in der Regel verwendet der [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) Klasse nicht direkt über die Benutzeroberfläche.</span><span class="sxs-lookup"><span data-stu-id="aa9e4-106">Handlers derive from the [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interface but typically uses the [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) class instead of deriving directly from the interface.</span></span>

<span data-ttu-id="aa9e4-107">Eine webhookanforderung kann von ein oder mehrere Handler verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="aa9e4-107">A WebHook request can be processed by one or more handlers.</span></span> <span data-ttu-id="aa9e4-108">Ereignishandler werden aufgerufen, in der Reihenfolge, basierend auf den jeweiligen *Reihenfolge* Eigenschaft vor sich geht von der niedrigsten zur höchsten, in denen Reihenfolge einer einfachen ganzen Zahl (zwischen 1 und 100 liegen empfohlen):</span><span class="sxs-lookup"><span data-stu-id="aa9e4-108">Handlers are called in order based on their respective *Order* property going from lowest to highest where Order is a simple integer (suggested to be between 1 and 100):</span></span>

![Diagramm für Webhookhandler Order-Eigenschaft](_static/Handlers.png)

<span data-ttu-id="aa9e4-110">Optional können Sie ein Handler Festlegen der *Antwort* Eigenschaft für die WebHookHandlerContext die führt die Verarbeitung zu beenden und die Antwort wieder als HTTP-Antwort an den WebHook gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="aa9e4-110">A handler can optionally set the *Response* property on the WebHookHandlerContext which will lead the processing to stop and the response to be sent back as the HTTP response to the WebHook.</span></span> <span data-ttu-id="aa9e4-111">Im obigen Fall Handler C wird nicht aufgerufen, da höheren Ordnung sein als die Antwort legt, B und B fest hat.</span><span class="sxs-lookup"><span data-stu-id="aa9e4-111">In the case above, Handler C won't get called because it has a higher order than B and B sets the response.</span></span>

<span data-ttu-id="aa9e4-112">Festlegen der Antwort ist in der Regel nur relevant für WebHooks, in dem die Antwort Informationen zurück an die Ursprungs-API ausführen können.</span><span class="sxs-lookup"><span data-stu-id="aa9e4-112">Setting the response is typically only relevant for WebHooks where the response can carry information back to the originating API.</span></span> <span data-ttu-id="aa9e4-113">Dies ist beispielsweise der Fall bei Slack-WebHooks, in dem die Antwort zurück an den Kanal gesendet wird, in dem der WebHook stammt.</span><span class="sxs-lookup"><span data-stu-id="aa9e4-113">This is for example the case with Slack WebHooks where the response is posted back to the channel where the WebHook came from.</span></span> <span data-ttu-id="aa9e4-114">Handler können die Empfänger-Eigenschaft festlegen, wenn sie nur die WebHooks von diesem speziellen Empfänger empfangen möchten.</span><span class="sxs-lookup"><span data-stu-id="aa9e4-114">Handlers can set the Receiver property if they only want to receive WebHooks from that particular receiver.</span></span> <span data-ttu-id="aa9e4-115">Wenn sie nicht, dass den Empfänger festlegen, die sie für alle von ihnen aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="aa9e4-115">If they don't set the receiver they are called for all of them.</span></span>

<span data-ttu-id="aa9e4-116">Eine andere allgemeine Verwendung einer Antwort ist die Verwendung einer *410 – Fehlend* Antwort, um anzugeben, dass der WebHook nicht mehr aktiv ist und keine weiteren Anforderungen gesendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="aa9e4-116">One other common use of a response is to use a *410 Gone* response to indicate that the WebHook no longer is active and no further requests should be submitted.</span></span>

<span data-ttu-id="aa9e4-117">Standardmäßig wird ein Handler von allen Empfängern der WebHook aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="aa9e4-117">By default a handler will be called by all WebHook receivers.</span></span> <span data-ttu-id="aa9e4-118">Aber wenn die *Empfänger* -Eigenschaftensatz auf den Namen eines Handlers, und klicken Sie dann diesen Handler webhookanforderungen nur von diesem Empfänger erhalten.</span><span class="sxs-lookup"><span data-stu-id="aa9e4-118">However, if the *Receiver* property is set to the name of a handler then that handler will only receive WebHook requests from that receiver.</span></span>

## <a name="processing-a-webhook"></a><span data-ttu-id="aa9e4-119">Verarbeiten eines Webhooks</span><span class="sxs-lookup"><span data-stu-id="aa9e4-119">Processing a WebHook</span></span>

<span data-ttu-id="aa9e4-120">Wenn ein Handler aufgerufen wird, wird eine [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) mit Informationen über die WebHook-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="aa9e4-120">When a handler is called, it gets a [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) containing information about the WebHook request.</span></span> <span data-ttu-id="aa9e4-121">Die Daten in der Regel HTTP-Anforderungstext, steht die *Daten* Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="aa9e4-121">The data, typically the HTTP request body, is available from the *Data* property.</span></span>

<span data-ttu-id="aa9e4-122">Der Typ der Daten ist in der Regel JSON oder HTML-Formulardaten, aber es ist möglich, in einen spezifischeren Typ umgewandelt werden soll, falls gewünscht.</span><span class="sxs-lookup"><span data-stu-id="aa9e4-122">The type of the data is typically JSON or HTML form data, but it is possible to cast to a more specific type if desired.</span></span> <span data-ttu-id="aa9e4-123">Beispielsweise benutzerdefinierte WebHooks von ASP.NET WebHooks generiert in den Typ umgewandelt werden kann [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) wie folgt:</span><span class="sxs-lookup"><span data-stu-id="aa9e4-123">For example, the custom WebHooks generated by ASP.NET WebHooks can be cast to the type [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) as follows:</span></span>

```csharp
public class MyWebHookHandler : WebHookHandler
{
    public MyWebHookHandler()
    {
        this.Receiver = "custom";
    }

    public override Task ExecuteAsync(string generator, WebHookHandlerContext context)
    {
        CustomNotifications notifications = context.GetDataOrDefault<CustomNotifications>();
        foreach (var notification in notifications.Notifications)
        {
            ...
        }
        return Task.FromResult(true);
    }
}
```

  ## <a name="queued-processing"></a><span data-ttu-id="aa9e4-124">In der Warteschlange verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="aa9e4-124">Queued Processing</span></span>

<span data-ttu-id="aa9e4-125">Die meisten Absender der WebHook werden einen WebHook erneut senden, wenn eine Antwort innerhalb von ein paar Sekunden nicht generiert werden.</span><span class="sxs-lookup"><span data-stu-id="aa9e4-125">Most WebHook senders will resend a WebHook if a response is not generated within a handful of seconds.</span></span> <span data-ttu-id="aa9e4-126">Dies bedeutet, dass der Handler die Verarbeitung innerhalb dieses Zeitrahmens, in der Reihenfolge, damit er erneut aufgerufen werden, nicht abgeschlossen werden muss.</span><span class="sxs-lookup"><span data-stu-id="aa9e4-126">This means that your handler must complete the processing within that time frame in order not for it to be called again.</span></span>

<span data-ttu-id="aa9e4-127">Wenn die Verarbeitung länger dauert, oder besser getrennt verarbeitet die [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) kann verwendet werden, um die WebHook-Anforderung an eine Warteschlange, z. B. senden [Azure Storage-Warteschlange](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span><span class="sxs-lookup"><span data-stu-id="aa9e4-127">If the processing takes longer, or is better handled separately then the [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) can be used to submit the WebHook request to a queue, for example [Azure Storage Queue](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span></span>

<span data-ttu-id="aa9e4-128">Einen Überblick über eine [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) Implementierung finden Sie hier:</span><span class="sxs-lookup"><span data-stu-id="aa9e4-128">An outline of a [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implementation is provided here:</span></span>

```csharp
public class QueueHandler : WebHookQueueHandler
{
    public override Task EnqueueAsync(WebHookQueueContext context)
    {

        // Enqueue WebHookQueueContext to your queuing system of choice

        return Task.FromResult(true);
    }
}
```
