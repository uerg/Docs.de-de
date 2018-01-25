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
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 12acae0883c12698a8f9c2150623ba792303e7ef
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
# <a name="aspnet-webhooks-handlers"></a>ASP.NET WebHooks Handler

Sobald WebHooks Anforderungen durch einen WebHook Empfänger bestätigt wurde, kann er von Benutzercode verarbeitet werden. Dies ist, wenn *Handler* in stammen. Handler Ableiten der [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) -Schnittstelle, aber in der Regel verwendet die [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) statt direkt aus der Schnittstelle abgeleitete Klasse.

Eine WebHook-Anforderung kann durch eine oder mehrere Handler verarbeitet werden. Ereignishandler werden aufgerufen, in Reihenfolge basierend auf ihren jeweiligen *Reihenfolge* Eigenschaft also vom niedrigsten zum höchsten, in dem Auftrag eine einfache Ganzzahl (zwischen 1 und 100 werden empfohlen):

![WebHook-Handler Reihenfolge Eigenschaft Diagramm](_static/Handlers.png)

Optional können Sie ein Handler Festlegen der *Antwort* Eigenschaft auf die die Verarbeitung zu beenden und die Antwort wieder als HTTP-Antwort des Webhooks anzeigeleistung WebHookHandlerContext. Im obigen Fall wird nicht Handler C aufgerufen, weil sie höheren Ordnung als B und B die Antwort legt enthält.

Festlegen der Antwort ist in der Regel nur relevant für WebHooks, in denen die Antwort Informationen zurück an den ursprünglichen-API ausführen können. Dies ist z. B. der Fall bei Slack WebHooks, in denen die Antwort an den Kanal zurückgesendet wird, in dem der WebHook stammt. Ereignishandler können die Empfänger-Eigenschaft festlegen, wenn sie nur WebHooks von diesem bestimmten Empfänger empfangen möchten. Wenn sie nicht, dass den Empfänger festlegen, die sie für jede von ihnen aufgerufen werden.

Eine andere häufige Verwendung einer Antwort ist die Verwendung einer *410 Fertig* Antwort, um anzugeben, dass der WebHook nicht mehr aktiv ist und keine weiteren Anforderungen gesendet werden soll.

Standardmäßig wird ein Handler von aller WebHook Empfänger aufgerufen werden. Jedoch, wenn die *Empfänger* Eigenschaft auf den Namen des einen Handler festgelegt ist, wird dieser Handler WebHook Anfragen nur von diesem Empfänger erhalten.

## <a name="processing-a-webhook"></a>Verarbeiten von WebHook

Wenn ein Handler aufgerufen wird, ruft eine [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) mit Informationen über die WebHook-Anforderung. Die Daten, die in der Regel den HTTP-Anforderungstext, steht über den *Daten* Eigenschaft.

Der Typ der Daten ist in der Regel JSON oder HTML-Formulardaten, aber es ist möglich, in einen spezifischeren Typ umgewandelt werden soll, falls gewünscht. Angenommen, die benutzerdefinierte WebHooks, die von ASP.NET WebHooks generiert in den Typ umgewandelt werden kann [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) wie folgt:

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

  ## <a name="queued-processing"></a>In der Warteschlange verarbeiten

Die meisten WebHook Absender erneut einen WebHook aus, wenn eine Antwort nicht innerhalb von wenigen Sekunden generiert wird. Dies bedeutet, dass der Handler die Verarbeitung innerhalb dieses Zeitrahmens in der Reihenfolge, damit er erneut aufgerufen werden, nicht abgeschlossen werden muss.

Wenn die Verarbeitung länger dauert, oder eine bessere Leistung separat behandelt die [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) können verwendet werden, um die WebHook-Anforderung an eine Warteschlange zu senden, z. B. [Azure-Speicherwarteschlange](https://msdn.microsoft.com/library/azure/dd179353.aspx).

Einen Überblick über eine [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) Implementierung wird hier bereitgestellt:

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
