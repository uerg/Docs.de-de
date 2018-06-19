---
uid: webhooks/index
title: Übersicht über ASP.NET-WebHooks | Microsoft Docs
author: rick-anderson
description: Einführung in ASP.NET WebHooks.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5e2843f0-f499-448f-a712-33d4e9858321
ms.technology: ''
ms.prod: .net-framework
ms.openlocfilehash: 52399c23cdf393a2f7f94661fd48098ced65948c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530049"
---
# <a name="aspnet-webhooks-overview"></a><span data-ttu-id="f37e9-103">ASP.NET WebHooks (Übersicht)</span><span class="sxs-lookup"><span data-stu-id="f37e9-103">ASP.NET WebHooks overview</span></span>

<span data-ttu-id="f37e9-104">WebHooks ist eine einfache HTTP-Muster, die eine einfache und Abonnementmodell für Verkabelung zusammen Web-APIs und SaaS-Dienste bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="f37e9-104">WebHooks is a lightweight HTTP pattern providing a simple pub/sub model for wiring together Web APIs and SaaS services.</span></span> <span data-ttu-id="f37e9-105">Wenn ein Ereignis in einem Dienst auftritt, wird eine Benachrichtigung an die registrierten Abonnenten in Form einer HTTP POST-Anforderung gesendet.</span><span class="sxs-lookup"><span data-stu-id="f37e9-105">When an event happens in a service, a notification is sent in the form of an HTTP POST request to registered subscribers.</span></span> <span data-ttu-id="f37e9-106">Die POST-Anforderung enthält Informationen zum Ereignis, wodurch es möglich, dass der Empfänger entsprechend reagieren.</span><span class="sxs-lookup"><span data-stu-id="f37e9-106">The POST request contains information about the event which makes it possible for the receiver to act accordingly.</span></span>

<span data-ttu-id="f37e9-107">Aufgrund ihrer Einfachheit WebHooks bereits verfügbar gemacht werden eine große Anzahl von Diensten, einschließlich [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp ](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/), und vieles mehr.</span><span class="sxs-lookup"><span data-stu-id="f37e9-107">Because of their simplicity, WebHooks are already exposed by a large number of services including [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/), and many more.</span></span> <span data-ttu-id="f37e9-108">Ein WebHook kann z. B. anzeigen, eine Datei geändert hat [Dropbox](http://dropbox.com/), hat eine Änderung des Codes in GitHub ein Commit ausgeführt wurde, oder eine Zahlung wurde initiiert [PayPal](http://www.paypal.com/), oder eine Karte erstellt wurde, [ Trello](http://www.trello.com/).</span><span class="sxs-lookup"><span data-stu-id="f37e9-108">For example, a WebHook can indicate that a file has changed in [Dropbox](http://dropbox.com/), or a code change has been committed in GitHub, or a payment has been initiated in [PayPal](http://www.paypal.com/), or a card has been created in [Trello](http://www.trello.com/).</span></span> <span data-ttu-id="f37e9-109">Mögliche Werte sind endlos!</span><span class="sxs-lookup"><span data-stu-id="f37e9-109">The possibilities are endless!</span></span>

<span data-ttu-id="f37e9-110">Microsoft ASP.NET WebHooks vereinfacht das Senden und Empfangen von WebHooks als Teil Ihrer ASP.NET-Anwendung:</span><span class="sxs-lookup"><span data-stu-id="f37e9-110">Microsoft ASP.NET WebHooks makes it easier to both send and receive WebHooks as part of your ASP.NET application:</span></span>

* <span data-ttu-id="f37e9-111">Auf der Empfangsseite stellt ein allgemeinen Modell zum Empfangen und Verarbeiten von WebHooks, die von einer beliebigen Anzahl von WebHook Anbietern bereit.</span><span class="sxs-lookup"><span data-stu-id="f37e9-111">On the receiving side, it provides a common model for receiving and processing WebHooks from any number of WebHook providers.</span></span> <span data-ttu-id="f37e9-112">Es ergibt sich aus der Box mit Unterstützung für [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[ WordPress](http://www.wordpress.com) und [Zendesk](https://www.zendesk.com/) , aber es ist einfach, zum Hinzufügen der Unterstützung für mehrere.</span><span class="sxs-lookup"><span data-stu-id="f37e9-112">It comes out of the box with support for [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) and [Zendesk](https://www.zendesk.com/) but it is easy to add support for more.</span></span>

* <span data-ttu-id="f37e9-113">Auf der sendenden Seite unterstützt verwalten und Speichern von Abonnements sowie wie für ereignisbenachrichtigungen an der richtigen Gruppe von Abonnenten gesendet.</span><span class="sxs-lookup"><span data-stu-id="f37e9-113">On the sending side it provides support for managing and storing subscriptions as well as for sending event notifications to the right set of subscribers.</span></span> <span data-ttu-id="f37e9-114">Dadurch können Sie Ihren eigenen Satz von Ereignissen zu definieren, geschieht Dinge zu benachrichtigen, dass Abonnenten abonnieren können.</span><span class="sxs-lookup"><span data-stu-id="f37e9-114">This allows you to define your own set of events that subscribers can subscribe to and notify them when things happens.</span></span>

<span data-ttu-id="f37e9-115">Die beiden Teile können je nach Szenario voneinander oder zusammen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="f37e9-115">The two parts can be used together or apart depending on your scenario.</span></span> <span data-ttu-id="f37e9-116">Wenn Sie nur WebHooks von anderen Diensten zu erhalten, können Sie nur die Empfänger Uhrzeitanteil verwenden müssen. Wenn Sie nur WebHooks für andere nutzen verfügbar machen möchten, können Sie genau das tun.</span><span class="sxs-lookup"><span data-stu-id="f37e9-116">If you only need to receive WebHooks from other services then you can use just the receiver part; if you only want to expose WebHooks for others to consume, then you can do just that.</span></span>

<span data-ttu-id="f37e9-117">Der Code als Ziel verwendet ASP.NET Web API 2 und ASP.NET MVC 5 und steht als [OSS auf GitHub](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="f37e9-117">The code targets ASP.NET Web API 2 and ASP.NET MVC 5 and is available as [OSS on GitHub](https://github.com/aspnet/WebHooks).</span></span>

## <a name="webhooks-overview"></a><span data-ttu-id="f37e9-118">WebHooks (Übersicht)</span><span class="sxs-lookup"><span data-stu-id="f37e9-118">WebHooks Overview</span></span>

<span data-ttu-id="f37e9-119">WebHooks ist ein Muster, was bedeutet, dass es variiert wie er vom Dienst zum Dienst verwendet wird, jedoch die zugrunde liegende Idee gleich.</span><span class="sxs-lookup"><span data-stu-id="f37e9-119">WebHooks is a pattern which means that it varies how it is used from service to service but the basic idea is the same.</span></span> <span data-ttu-id="f37e9-120">Sie können sich vorstellen WebHooks als einfache Pub/Sub-Modell, in denen ein Benutzer Ereignisse, die an anderer Stelle geschieht abonnieren kann.</span><span class="sxs-lookup"><span data-stu-id="f37e9-120">You can think of WebHooks as a simple pub/sub model where a user can subscribe to events happening elsewhere.</span></span> <span data-ttu-id="f37e9-121">Die ereignisbenachrichtigungen werden als HTTP POST-Anforderungen, die mit Informationen über das eigentliche Ereignis weitergegeben.</span><span class="sxs-lookup"><span data-stu-id="f37e9-121">The event notifications are propagated as HTTP POST requests containing information about the event itself.</span></span>

<span data-ttu-id="f37e9-122">In der Regel enthält die HTTP-POST-Anforderung eine JSON-Objekt oder eine HTML-Formulardaten, die zum Auslösen der WebHook Absender, einschließlich Informationen über das Ereignis verursacht des Webhooks ermittelt.</span><span class="sxs-lookup"><span data-stu-id="f37e9-122">Typically the HTTP POST request contains a JSON object or HTML form data determined by the WebHook sender including information about the event causing the WebHook to trigger.</span></span> <span data-ttu-id="f37e9-123">Angenommen, ein Beispiel für einen WebHook POST-Anforderungstext aus [GitHub](http://www.github.com/) als Ergebnis ein neues Problem wird geöffnet, in ein bestimmtes Repository wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="f37e9-123">For example, an example of a WebHook POST request body from [GitHub](http://www.github.com/) looks like this as a result of a new issue being opened in a particular repository:</span></span>

```json
{
  "action": "opened",
  "issue": {
      "url": "https://api.github.com/repos/octocat/Hello-World/issues/1347",
      "number": 1347,
      ...
  },
  "repository": {
      "id": 1296269,
      "full_name": "octocat/Hello-World",
      "owner": {
          "login": "octocat",
          "id": 1
          ...
      },
      ...
  },
  "sender": {
      "login": "octocat",
      "id": 1,
      ...
  }
}
```

<span data-ttu-id="f37e9-124">Um sicherzustellen, dass der WebHook tatsächlich den beabsichtigten Absender stammt, ist die POST-Anforderung in irgendeiner Weise gesichert und dann vom Empfänger bestätigt.</span><span class="sxs-lookup"><span data-stu-id="f37e9-124">To ensure that the WebHook is indeed from the intended sender, the POST request is secured in some way and then verified by the receiver.</span></span> <span data-ttu-id="f37e9-125">Beispielsweise [GitHub WebHooks](https://developer.github.com/webhooks/) enthält ein *X-Hub-Signatur* HTTP-Header mit einem Hash des Anforderungstexts, der von der Implementierung des Empfängers überprüft wird, damit Sie nicht kümmern müssen.</span><span class="sxs-lookup"><span data-stu-id="f37e9-125">For example, [GitHub WebHooks](https://developer.github.com/webhooks/) includes an *X-Hub-Signature* HTTP header with a hash of the request body which is checked by the receiver implementation so you don't have to worry about it.</span></span>

<span data-ttu-id="f37e9-126">Die WebHook-Fluss geht in der Regel etwa so aussehen:</span><span class="sxs-lookup"><span data-stu-id="f37e9-126">The WebHook flow generally goes something like this:</span></span>

* <span data-ttu-id="f37e9-127">Der WebHook Absender Ereignisse verfügbar macht, denen ein Client abonnieren kann.</span><span class="sxs-lookup"><span data-stu-id="f37e9-127">The WebHook sender exposes events that a client can subscribe to.</span></span> <span data-ttu-id="f37e9-128">Die Ereignisse beschreiben wahrnehmbare Änderungen am System beispielsweise, die ein neues Datenelement eingefügt, die ein Prozess abgeschlossen ist oder etwas anderes wurde.</span><span class="sxs-lookup"><span data-stu-id="f37e9-128">The events describe observable changes to the system, for example that a new data item has been inserted, that a process has completed, or something else.</span></span>

* <span data-ttu-id="f37e9-129">Der WebHook Empfänger abonniert, indem Sie registrieren einen WebHook bestehend aus vier Aspekte:</span><span class="sxs-lookup"><span data-stu-id="f37e9-129">The WebHook receiver subscribes by registering a WebHook consisting of four things:</span></span>

     1. <span data-ttu-id="f37e9-130">Ein URI, in dem die ereignisbenachrichtigung in Form einer HTTP POST-Anforderung veröffentlicht werden sollte;</span><span class="sxs-lookup"><span data-stu-id="f37e9-130">A URI for where the event notification should be posted in the form of an HTTP POST request;</span></span>

     2. <span data-ttu-id="f37e9-131">Eine Reihe von Filtern, die Beschreibung der bestimmten Ereignisse, die für die WebHook ausgelöst werden soll;</span><span class="sxs-lookup"><span data-stu-id="f37e9-131">A set of filters describing the particular events for which the WebHook should be fired;</span></span>

     3. <span data-ttu-id="f37e9-132">Ein für den geheimen Schlüssel dient zum Signieren der HTTP POST-Anforderung</span><span class="sxs-lookup"><span data-stu-id="f37e9-132">A secret key which is used to sign the HTTP POST request;</span></span>

     4. <span data-ttu-id="f37e9-133">Zusätzliche Daten, d. h., in der HTTP POST-Anforderung eingeschlossen werden sollen.</span><span class="sxs-lookup"><span data-stu-id="f37e9-133">Additional data which is to be included in the HTTP POST request.</span></span> <span data-ttu-id="f37e9-134">Dies kann z. B. zusätzliche HTTP-Header-Felder oder Eigenschaften, die im Hauptteil HTTP POST-Anforderung enthalten sein.</span><span class="sxs-lookup"><span data-stu-id="f37e9-134">This can for example be additional HTTP header fields or properties included in the HTTP POST request body.</span></span>

* <span data-ttu-id="f37e9-135">Sobald ein Ereignis eintritt, wird die übereinstimmenden WebHook Registrierungen gefunden wurden, und HTTP POST-Anforderungen gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="f37e9-135">Once an event happens, the matching WebHook registrations are found and HTTP POST requests are submitted.</span></span> <span data-ttu-id="f37e9-136">Die Generierung von HTTP POST-Anforderungen sind in der Regel mehrere Male wiederholt, falls für aus irgendeinem Grund, dass der Empfänger nicht antwortet oder die HTTP POST-Anforderung-Ergebnisse in eine Fehlerantwort.</span><span class="sxs-lookup"><span data-stu-id="f37e9-136">Typically, the generation of the HTTP POST requests are retried several times if for some reason the recipient is not responding or the HTTP POST request results in an error response.</span></span>

## <a name="webhooks-processing-pipeline"></a><span data-ttu-id="f37e9-137">WebHooks Verarbeitungspipeline</span><span class="sxs-lookup"><span data-stu-id="f37e9-137">WebHooks Processing Pipeline</span></span>

<span data-ttu-id="f37e9-138">Die Microsoft ASP.NET WebHooks Verarbeitungspipeline für eingehende WebHooks sieht wie folgt:</span><span class="sxs-lookup"><span data-stu-id="f37e9-138">The Microsoft ASP.NET WebHooks processing pipeline for incoming WebHooks looks like this:</span></span>

![ASP.NET WebHooks Verarbeitungspipeline](_static/WebHookReceivers.png)

<span data-ttu-id="f37e9-140">Die beiden wichtigsten Konzepte hier sind *Empfänger* und *Handler*:</span><span class="sxs-lookup"><span data-stu-id="f37e9-140">The two key concepts here are *Receivers* and *Handlers*:</span></span>

* <span data-ttu-id="f37e9-141">*Empfänger* sind verantwortlich für die Behandlung der bestimmten WebHook-Typ von einem bestimmten Absender und zum Erzwingen von sicherheitsüberprüfungen, um sicherzustellen, dass die WebHook-Anforderung tatsächlich den beabsichtigten Absender stammt.</span><span class="sxs-lookup"><span data-stu-id="f37e9-141">*Receivers* are responsible for handling the particular flavor of WebHook from a given sender and for enforcing security checks to ensure that the WebHook request indeed is from the intended sender.</span></span>

* <span data-ttu-id="f37e9-142">*Handler* sind in der Regel, in denen Benutzercode führt die Verarbeitung aus bestimmten Webhooks.</span><span class="sxs-lookup"><span data-stu-id="f37e9-142">*Handlers* are typically where user code runs processing the particular WebHook.</span></span>

<span data-ttu-id="f37e9-143">In den folgenden Knoten werden diese Konzepte ausführlicher beschrieben.</span><span class="sxs-lookup"><span data-stu-id="f37e9-143">In the following nodes these concepts are described in more details.</span></span>
