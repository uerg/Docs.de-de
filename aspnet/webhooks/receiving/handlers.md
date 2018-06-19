---
uid: webhooks/receiving/handlers
title: ASP.NET WebHooks Handler | Microsoft Docs
author: rick-anderson
description: Behandeln von Anforderungen in ASP.NET WebHooks.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.technology: ''
ms.prod: .net-framework
ms.openlocfilehash: 4cf5770a731ef77842eb29b0a66ee0aac5d85d85
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
ms.locfileid: "28883670"
---
# <a name="aspnet-webhooks-handlers"></a><span data-ttu-id="cb5aa-103">ASP.NET WebHooks Handler</span><span class="sxs-lookup"><span data-stu-id="cb5aa-103">ASP.NET WebHooks handlers</span></span>

<span data-ttu-id="cb5aa-104">Sobald WebHooks Anforderungen durch einen WebHook Empfänger bestätigt wurde, kann er von Benutzercode verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="cb5aa-104">Once WebHooks requests has been validated by a WebHook receiver, it is ready to be processed by user code.</span></span> <span data-ttu-id="cb5aa-105">Dies ist, wenn *Handler* in stammen.</span><span class="sxs-lookup"><span data-stu-id="cb5aa-105">This is where *handlers* come in.</span></span> <span data-ttu-id="cb5aa-106">Handler Ableiten der [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) -Schnittstelle, aber in der Regel verwendet die [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) statt direkt aus der Schnittstelle abgeleitete Klasse.</span><span class="sxs-lookup"><span data-stu-id="cb5aa-106">Handlers derive from the [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interface but typically uses the [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) class instead of deriving directly from the interface.</span></span>

<span data-ttu-id="cb5aa-107">Eine WebHook-Anforderung kann durch eine oder mehrere Handler verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="cb5aa-107">A WebHook request can be processed by one or more handlers.</span></span> <span data-ttu-id="cb5aa-108">Ereignishandler werden aufgerufen, in Reihenfolge basierend auf ihren jeweiligen *Reihenfolge* Eigenschaft also vom niedrigsten zum höchsten, in dem Auftrag eine einfache Ganzzahl (zwischen 1 und 100 werden empfohlen):</span><span class="sxs-lookup"><span data-stu-id="cb5aa-108">Handlers are called in order based on their respective *Order* property going from lowest to highest where Order is a simple integer (suggested to be between 1 and 100):</span></span>

![WebHook-Handler Reihenfolge Eigenschaft Diagramm](_static/Handlers.png)

<span data-ttu-id="cb5aa-110">Optional können Sie ein Handler Festlegen der *Antwort* Eigenschaft auf die die Verarbeitung zu beenden und die Antwort wieder als HTTP-Antwort des Webhooks anzeigeleistung WebHookHandlerContext.</span><span class="sxs-lookup"><span data-stu-id="cb5aa-110">A handler can optionally set the *Response* property on the WebHookHandlerContext which will lead the processing to stop and the response to be sent back as the HTTP response to the WebHook.</span></span> <span data-ttu-id="cb5aa-111">Im obigen Fall wird nicht Handler C aufgerufen, weil sie höheren Ordnung als B und B die Antwort legt enthält.</span><span class="sxs-lookup"><span data-stu-id="cb5aa-111">In the case above, Handler C won't get called because it has a higher order than B and B sets the response.</span></span>

<span data-ttu-id="cb5aa-112">Festlegen der Antwort ist in der Regel nur relevant für WebHooks, in denen die Antwort Informationen zurück an den ursprünglichen-API ausführen können.</span><span class="sxs-lookup"><span data-stu-id="cb5aa-112">Setting the response is typically only relevant for WebHooks where the response can carry information back to the originating API.</span></span> <span data-ttu-id="cb5aa-113">Dies ist z. B. der Fall bei Slack WebHooks, in denen die Antwort an den Kanal zurückgesendet wird, in dem der WebHook stammt.</span><span class="sxs-lookup"><span data-stu-id="cb5aa-113">This is for example the case with Slack WebHooks where the response is posted back to the channel where the WebHook came from.</span></span> <span data-ttu-id="cb5aa-114">Ereignishandler können die Empfänger-Eigenschaft festlegen, wenn sie nur WebHooks von diesem bestimmten Empfänger empfangen möchten.</span><span class="sxs-lookup"><span data-stu-id="cb5aa-114">Handlers can set the Receiver property if they only want to receive WebHooks from that particular receiver.</span></span> <span data-ttu-id="cb5aa-115">Wenn sie nicht, dass den Empfänger festlegen, die sie für jede von ihnen aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="cb5aa-115">If they don't set the receiver they are called for all of them.</span></span>

<span data-ttu-id="cb5aa-116">Eine andere häufige Verwendung einer Antwort ist die Verwendung einer *410 Fertig* Antwort, um anzugeben, dass der WebHook nicht mehr aktiv ist und keine weiteren Anforderungen gesendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="cb5aa-116">One other common use of a response is to use a *410 Gone* response to indicate that the WebHook no longer is active and no further requests should be submitted.</span></span>

<span data-ttu-id="cb5aa-117">Standardmäßig wird ein Handler von aller WebHook Empfänger aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="cb5aa-117">By default a handler will be called by all WebHook receivers.</span></span> <span data-ttu-id="cb5aa-118">Jedoch, wenn die *Empfänger* Eigenschaft auf den Namen des einen Handler festgelegt ist, wird dieser Handler WebHook Anfragen nur von diesem Empfänger erhalten.</span><span class="sxs-lookup"><span data-stu-id="cb5aa-118">However, if the *Receiver* property is set to the name of a handler then that handler will only receive WebHook requests from that receiver.</span></span>

## <a name="processing-a-webhook"></a><span data-ttu-id="cb5aa-119">Verarbeiten von WebHook</span><span class="sxs-lookup"><span data-stu-id="cb5aa-119">Processing a WebHook</span></span>

<span data-ttu-id="cb5aa-120">Wenn ein Handler aufgerufen wird, ruft eine [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) mit Informationen über die WebHook-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="cb5aa-120">When a handler is called, it gets a [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) containing information about the WebHook request.</span></span> <span data-ttu-id="cb5aa-121">Die Daten, die in der Regel den HTTP-Anforderungstext, steht über den *Daten* Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="cb5aa-121">The data, typically the HTTP request body, is available from the *Data* property.</span></span>

<span data-ttu-id="cb5aa-122">Der Typ der Daten ist in der Regel JSON oder HTML-Formulardaten, aber es ist möglich, in einen spezifischeren Typ umgewandelt werden soll, falls gewünscht.</span><span class="sxs-lookup"><span data-stu-id="cb5aa-122">The type of the data is typically JSON or HTML form data, but it is possible to cast to a more specific type if desired.</span></span> <span data-ttu-id="cb5aa-123">Angenommen, die benutzerdefinierte WebHooks, die von ASP.NET WebHooks generiert in den Typ umgewandelt werden kann [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) wie folgt:</span><span class="sxs-lookup"><span data-stu-id="cb5aa-123">For example, the custom WebHooks generated by ASP.NET WebHooks can be cast to the type [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) as follows:</span></span>

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

  ## <a name="queued-processing"></a><span data-ttu-id="cb5aa-124">In der Warteschlange verarbeiten</span><span class="sxs-lookup"><span data-stu-id="cb5aa-124">Queued Processing</span></span>

<span data-ttu-id="cb5aa-125">Die meisten WebHook Absender erneut einen WebHook aus, wenn eine Antwort nicht innerhalb von wenigen Sekunden generiert wird.</span><span class="sxs-lookup"><span data-stu-id="cb5aa-125">Most WebHook senders will resend a WebHook if a response is not generated within a handful of seconds.</span></span> <span data-ttu-id="cb5aa-126">Dies bedeutet, dass der Handler die Verarbeitung innerhalb dieses Zeitrahmens in der Reihenfolge, damit er erneut aufgerufen werden, nicht abgeschlossen werden muss.</span><span class="sxs-lookup"><span data-stu-id="cb5aa-126">This means that your handler must complete the processing within that time frame in order not for it to be called again.</span></span>

<span data-ttu-id="cb5aa-127">Wenn die Verarbeitung länger dauert, oder eine bessere Leistung separat behandelt die [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) können verwendet werden, um die WebHook-Anforderung an eine Warteschlange zu senden, z. B. [Azure-Speicherwarteschlange](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span><span class="sxs-lookup"><span data-stu-id="cb5aa-127">If the processing takes longer, or is better handled separately then the [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) can be used to submit the WebHook request to a queue, for example [Azure Storage Queue](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span></span>

<span data-ttu-id="cb5aa-128">Einen Überblick über eine [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) Implementierung wird hier bereitgestellt:</span><span class="sxs-lookup"><span data-stu-id="cb5aa-128">An outline of a [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implementation is provided here:</span></span>

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
