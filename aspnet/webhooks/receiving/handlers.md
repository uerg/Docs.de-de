---
uid: webhooks/receiving/handlers
title: ASP.NET WebHooks Handler | Microsoft-Dokumentation
author: rick-anderson
description: Informationen zur Verarbeitung von Anforderungen in ASP.NET WebHooks.
ms.author: aspnetcontent
ms.date: 01/17/2012
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.openlocfilehash: bc8f4ef3f4ade775b395d73dfa8d73fec92fba3f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841421"
---
# <a name="aspnet-webhooks-handlers"></a>ASP.NET WebHooks-Handler

Nach der WebHooks Anforderungen durch einen webhookempfänger bestätigt wurde, ist es für die Verarbeitung von Benutzercode bereit. Hier kommt *Handler* eingehen. Handler zu leiten, von der [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) -Schnittstelle, aber in der Regel verwendet der [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) Klasse nicht direkt über die Benutzeroberfläche.

Eine webhookanforderung kann von ein oder mehrere Handler verarbeitet werden. Ereignishandler werden aufgerufen, in der Reihenfolge, basierend auf den jeweiligen *Reihenfolge* Eigenschaft vor sich geht von der niedrigsten zur höchsten, in denen Reihenfolge einer einfachen ganzen Zahl (zwischen 1 und 100 liegen empfohlen):

![Diagramm für Webhookhandler Order-Eigenschaft](_static/Handlers.png)

Optional können Sie ein Handler Festlegen der *Antwort* Eigenschaft für die WebHookHandlerContext die führt die Verarbeitung zu beenden und die Antwort wieder als HTTP-Antwort an den WebHook gesendet werden. Im obigen Fall Handler C wird nicht aufgerufen, da höheren Ordnung sein als die Antwort legt, B und B fest hat.

Festlegen der Antwort ist in der Regel nur relevant für WebHooks, in dem die Antwort Informationen zurück an die Ursprungs-API ausführen können. Dies ist beispielsweise der Fall bei Slack-WebHooks, in dem die Antwort zurück an den Kanal gesendet wird, in dem der WebHook stammt. Handler können die Empfänger-Eigenschaft festlegen, wenn sie nur die WebHooks von diesem speziellen Empfänger empfangen möchten. Wenn sie nicht, dass den Empfänger festlegen, die sie für alle von ihnen aufgerufen werden.

Eine andere allgemeine Verwendung einer Antwort ist die Verwendung einer *410 – Fehlend* Antwort, um anzugeben, dass der WebHook nicht mehr aktiv ist und keine weiteren Anforderungen gesendet werden soll.

Standardmäßig wird ein Handler von allen Empfängern der WebHook aufgerufen werden. Aber wenn die *Empfänger* -Eigenschaftensatz auf den Namen eines Handlers, und klicken Sie dann diesen Handler webhookanforderungen nur von diesem Empfänger erhalten.

## <a name="processing-a-webhook"></a>Verarbeiten eines Webhooks

Wenn ein Handler aufgerufen wird, wird eine [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) mit Informationen über die WebHook-Anforderung. Die Daten in der Regel HTTP-Anforderungstext, steht die *Daten* Eigenschaft.

Der Typ der Daten ist in der Regel JSON oder HTML-Formulardaten, aber es ist möglich, in einen spezifischeren Typ umgewandelt werden soll, falls gewünscht. Beispielsweise benutzerdefinierte WebHooks von ASP.NET WebHooks generiert in den Typ umgewandelt werden kann [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) wie folgt:

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

  ## <a name="queued-processing"></a>In der Warteschlange verarbeiten.

Die meisten Absender der WebHook werden einen WebHook erneut senden, wenn eine Antwort innerhalb von ein paar Sekunden nicht generiert werden. Dies bedeutet, dass der Handler die Verarbeitung innerhalb dieses Zeitrahmens, in der Reihenfolge, damit er erneut aufgerufen werden, nicht abgeschlossen werden muss.

Wenn die Verarbeitung länger dauert, oder besser getrennt verarbeitet die [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) kann verwendet werden, um die WebHook-Anforderung an eine Warteschlange, z. B. senden [Azure Storage-Warteschlange](https://msdn.microsoft.com/library/azure/dd179353.aspx).

Einen Überblick über eine [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) Implementierung finden Sie hier:

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
