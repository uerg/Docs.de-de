---
uid: webhooks/index
title: Übersicht über ASP.NET-WebHooks | Microsoft-Dokumentation
author: rick-anderson
description: Eine Einführung in ASP.NET WebHooks.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5e2843f0-f499-448f-a712-33d4e9858321
ms.openlocfilehash: 702cc0bf0d0bb887c64bec19e1faf249bd96617a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "48253681"
---
# <a name="aspnet-webhooks-overview"></a><span data-ttu-id="d80a8-103">Übersicht über die ASP.NET WebHooks</span><span class="sxs-lookup"><span data-stu-id="d80a8-103">ASP.NET WebHooks overview</span></span>

<span data-ttu-id="d80a8-104">WebHooks ist ein einfacher HTTP-Muster, die eine einfache Veröffentlichungs-/Abonnementmodells verbinden zusammen Web-APIs und SaaS-Dienste bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="d80a8-104">WebHooks is a lightweight HTTP pattern providing a simple pub/sub model for wiring together Web APIs and SaaS services.</span></span> <span data-ttu-id="d80a8-105">Wenn ein Ereignis in einem Dienst auftritt, wird eine Benachrichtigung an die registrierten Abonnenten in Form von HTTP POST-Anforderung gesendet.</span><span class="sxs-lookup"><span data-stu-id="d80a8-105">When an event happens in a service, a notification is sent in the form of an HTTP POST request to registered subscribers.</span></span> <span data-ttu-id="d80a8-106">Die POST-Anforderung enthält Informationen über das Ereignis dadurch ist es möglich, dass der Empfänger entsprechend reagieren.</span><span class="sxs-lookup"><span data-stu-id="d80a8-106">The POST request contains information about the event which makes it possible for the receiver to act accordingly.</span></span>

<span data-ttu-id="d80a8-107">Aufgrund ihrer Einfachheit WebHooks sind bereits verfügbar gemacht werden durch eine große Anzahl von Diensten, einschließlich [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp ](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/), und vieles mehr.</span><span class="sxs-lookup"><span data-stu-id="d80a8-107">Because of their simplicity, WebHooks are already exposed by a large number of services including [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/), and many more.</span></span> <span data-ttu-id="d80a8-108">Ein WebHook kann beispielsweise angeben, dass es sich bei in eine Datei geändert hat [Dropbox](http://dropbox.com/), verfügt über eine Änderung des Codes auf GitHub ein Commit ausgeführt wurde, oder eine Zahlung wurde initiiert [PayPal](http://www.paypal.com/), oder in eine Karte erstellt wurde [ Trello](http://www.trello.com/).</span><span class="sxs-lookup"><span data-stu-id="d80a8-108">For example, a WebHook can indicate that a file has changed in [Dropbox](http://dropbox.com/), or a code change has been committed in GitHub, or a payment has been initiated in [PayPal](http://www.paypal.com/), or a card has been created in [Trello](http://www.trello.com/).</span></span> <span data-ttu-id="d80a8-109">Die Möglichkeiten sind grenzenlos!</span><span class="sxs-lookup"><span data-stu-id="d80a8-109">The possibilities are endless!</span></span>

<span data-ttu-id="d80a8-110">Microsoft ASP.NET WebHooks erleichtert es, senden und Empfangen von WebHooks als Teil Ihrer ASP.NET-Anwendung:</span><span class="sxs-lookup"><span data-stu-id="d80a8-110">Microsoft ASP.NET WebHooks makes it easier to both send and receive WebHooks as part of your ASP.NET application:</span></span>

* <span data-ttu-id="d80a8-111">Auf der Empfangsseite bietet es ein allgemeinen Modell zum Empfangen und Verarbeiten von WebHooks über eine beliebige Anzahl von WebHook-Anbieter.</span><span class="sxs-lookup"><span data-stu-id="d80a8-111">On the receiving side, it provides a common model for receiving and processing WebHooks from any number of WebHook providers.</span></span> <span data-ttu-id="d80a8-112">Es geht mit Unterstützung für [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[ WordPress](http://www.wordpress.com) und [Zendesk](https://www.zendesk.com/) , aber es ist einfach, Unterstützung für weitere hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="d80a8-112">It comes out of the box with support for [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) and [Zendesk](https://www.zendesk.com/) but it is easy to add support for more.</span></span>

* <span data-ttu-id="d80a8-113">Auf der Absenderseite bietet Unterstützung für verwalten und speichern die Abonnements für ereignisbenachrichtigungen an den richtigen Satz an Abonnenten gesendet.</span><span class="sxs-lookup"><span data-stu-id="d80a8-113">On the sending side it provides support for managing and storing subscriptions as well as for sending event notifications to the right set of subscribers.</span></span> <span data-ttu-id="d80a8-114">Dadurch können Sie Ihre eigene Gruppe von Ereignissen definieren, dass Abonnenten abonnieren können, und benachrichtigen sie an, wenn Dinge passiert.</span><span class="sxs-lookup"><span data-stu-id="d80a8-114">This allows you to define your own set of events that subscribers can subscribe to and notify them when things happens.</span></span>

<span data-ttu-id="d80a8-115">Die beiden Teile können je nach Szenario voneinander oder zusammen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="d80a8-115">The two parts can be used together or apart depending on your scenario.</span></span> <span data-ttu-id="d80a8-116">Wenn Sie nur auf WebHooks von anderen Diensten empfangen müssen, können Sie nur den Empfänger Teil verwenden; Wenn Sie nur die WebHooks für andere Personen nutzen verfügbar machen möchten, können Sie genau das tun.</span><span class="sxs-lookup"><span data-stu-id="d80a8-116">If you only need to receive WebHooks from other services then you can use just the receiver part; if you only want to expose WebHooks for others to consume, then you can do just that.</span></span>

<span data-ttu-id="d80a8-117">Der Code abzielt, ASP.NET Web API 2 und ASP.NET MVC 5 und steht als [OSS auf GitHub](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="d80a8-117">The code targets ASP.NET Web API 2 and ASP.NET MVC 5 and is available as [OSS on GitHub](https://github.com/aspnet/WebHooks).</span></span>

## <a name="webhooks-overview"></a><span data-ttu-id="d80a8-118">Übersicht über die WebHooks</span><span class="sxs-lookup"><span data-stu-id="d80a8-118">WebHooks Overview</span></span>

<span data-ttu-id="d80a8-119">WebHooks ist ein Muster, die bedeutet, dass es variiert, wie sie vom Dienst zu Dienst verwendet werden, aber die grundlegende Idee identisch ist.</span><span class="sxs-lookup"><span data-stu-id="d80a8-119">WebHooks is a pattern which means that it varies how it is used from service to service but the basic idea is the same.</span></span> <span data-ttu-id="d80a8-120">Sie können sich vorstellen WebHooks als einfache Pub/Sub-Modell, in denen ein Benutzer an anderer Stelle die Ereignisse abonnieren kann.</span><span class="sxs-lookup"><span data-stu-id="d80a8-120">You can think of WebHooks as a simple pub/sub model where a user can subscribe to events happening elsewhere.</span></span> <span data-ttu-id="d80a8-121">Die ereignisbenachrichtigungen werden als HTTP POST-Anforderungen, die mit Informationen über das Ereignis selbst weitergegeben.</span><span class="sxs-lookup"><span data-stu-id="d80a8-121">The event notifications are propagated as HTTP POST requests containing information about the event itself.</span></span>

<span data-ttu-id="d80a8-122">In der Regel enthält die HTTP-POST-Anforderung ein JSON-Objekt oder eine HTML-Formulardaten, die zum Auslösen der WebHook-Absender, einschließlich Informationen über das Ereignis verursacht den WebHook ermittelt.</span><span class="sxs-lookup"><span data-stu-id="d80a8-122">Typically the HTTP POST request contains a JSON object or HTML form data determined by the WebHook sender including information about the event causing the WebHook to trigger.</span></span> <span data-ttu-id="d80a8-123">Angenommen, ein Beispiel für eine WebHook-POST-Anforderungstext von [GitHub](http://www.github.com/) durch ein neues Problem geöffnet wird, in ein bestimmtes Repository wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="d80a8-123">For example, an example of a WebHook POST request body from [GitHub](http://www.github.com/) looks like this as a result of a new issue being opened in a particular repository:</span></span>

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

<span data-ttu-id="d80a8-124">Um sicherzustellen, dass der WebHook tatsächlich den beabsichtigten Absender stammt, ist die POST-Anforderung in irgendeiner Weise gesichert und dann vom Empfänger überprüft.</span><span class="sxs-lookup"><span data-stu-id="d80a8-124">To ensure that the WebHook is indeed from the intended sender, the POST request is secured in some way and then verified by the receiver.</span></span> <span data-ttu-id="d80a8-125">Z. B. [GitHub-WebHooks](https://developer.github.com/webhooks/) enthält ein *X-Hub-Signature* HTTP-Header mit einem Hash des Anforderungstexts, der durch die Implementierung für die Empfänger überprüft wird, sodass Sie keine weiteren Änderungen.</span><span class="sxs-lookup"><span data-stu-id="d80a8-125">For example, [GitHub WebHooks](https://developer.github.com/webhooks/) includes an *X-Hub-Signature* HTTP header with a hash of the request body which is checked by the receiver implementation so you don't have to worry about it.</span></span>

<span data-ttu-id="d80a8-126">Der WebHook-Workflow wechselt in der Regel mit etwa wie folgt:</span><span class="sxs-lookup"><span data-stu-id="d80a8-126">The WebHook flow generally goes something like this:</span></span>

* <span data-ttu-id="d80a8-127">Der WebHook-Absender Ereignisse verfügbar macht, denen ein Client abonnieren können.</span><span class="sxs-lookup"><span data-stu-id="d80a8-127">The WebHook sender exposes events that a client can subscribe to.</span></span> <span data-ttu-id="d80a8-128">Die Ereignisse beschreiben wahrnehmbare Änderungen für das System, z. B., die ein neues Datenelement eingefügt, das, dass ein Prozess abgeschlossen wurde oder etwas anderes.</span><span class="sxs-lookup"><span data-stu-id="d80a8-128">The events describe observable changes to the system, for example that a new data item has been inserted, that a process has completed, or something else.</span></span>

* <span data-ttu-id="d80a8-129">Der WebHook-Empfänger abonniert durch Registrieren eines Webhooks, die vier Arten von Informationen aus:</span><span class="sxs-lookup"><span data-stu-id="d80a8-129">The WebHook receiver subscribes by registering a WebHook consisting of four things:</span></span>

     1. <span data-ttu-id="d80a8-130">Ein URI für die, an die ereignisbenachrichtigung in Form von HTTP POST-Anforderung gesendet werden soll;</span><span class="sxs-lookup"><span data-stu-id="d80a8-130">A URI for where the event notification should be posted in the form of an HTTP POST request;</span></span>

     2. <span data-ttu-id="d80a8-131">Einen Satz von Filtern, beschreibt die bestimmte Ereignisse, die für die der WebHook ausgelöst werden soll;</span><span class="sxs-lookup"><span data-stu-id="d80a8-131">A set of filters describing the particular events for which the WebHook should be fired;</span></span>

     3. <span data-ttu-id="d80a8-132">Einen geheimen Schlüssel, der zum Signieren von HTTP POST-Anforderung verwendet wird;</span><span class="sxs-lookup"><span data-stu-id="d80a8-132">A secret key which is used to sign the HTTP POST request;</span></span>

     4. <span data-ttu-id="d80a8-133">Zusätzlicher Daten, die in der HTTP POST-Anforderung enthalten sein soll.</span><span class="sxs-lookup"><span data-stu-id="d80a8-133">Additional data which is to be included in the HTTP POST request.</span></span> <span data-ttu-id="d80a8-134">Dies kann z. B. zusätzliche HTTP-Headerfelder oder Eigenschaften, die im Hauptteil HTTP POST-Anforderung enthalten sein.</span><span class="sxs-lookup"><span data-stu-id="d80a8-134">This can for example be additional HTTP header fields or properties included in the HTTP POST request body.</span></span>

* <span data-ttu-id="d80a8-135">Sobald ein Ereignis eintritt, wird der entsprechenden WebHook-Registrierungen gefunden werden, und HTTP POST-Anforderungen gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="d80a8-135">Once an event happens, the matching WebHook registrations are found and HTTP POST requests are submitted.</span></span> <span data-ttu-id="d80a8-136">Die Generierung von HTTP POST-Anforderungen werden in der Regel mehrere Male wiederholt, wenn für die aus irgendeinem Grund, dass der Empfänger nicht antwortet oder die HTTP POST-Anforderung zu einer Fehlerantwort.</span><span class="sxs-lookup"><span data-stu-id="d80a8-136">Typically, the generation of the HTTP POST requests are retried several times if for some reason the recipient is not responding or the HTTP POST request results in an error response.</span></span>

## <a name="webhooks-processing-pipeline"></a><span data-ttu-id="d80a8-137">Pipeline zur Anforderungsverarbeitung WebHooks</span><span class="sxs-lookup"><span data-stu-id="d80a8-137">WebHooks Processing Pipeline</span></span>

<span data-ttu-id="d80a8-138">Die Microsoft ASP.NET WebHooks Verarbeitungspipeline für eingehende WebHooks sieht folgendermaßen aus:</span><span class="sxs-lookup"><span data-stu-id="d80a8-138">The Microsoft ASP.NET WebHooks processing pipeline for incoming WebHooks looks like this:</span></span>

![Pipeline zur Anforderungsverarbeitung ASP.NET WebHooks](_static/WebHookReceivers.png)

<span data-ttu-id="d80a8-140">Die zwei wichtigsten Konzepte hier sind *Empfänger* und *Handler*:</span><span class="sxs-lookup"><span data-stu-id="d80a8-140">The two key concepts here are *Receivers* and *Handlers*:</span></span>

* <span data-ttu-id="d80a8-141">*Empfänger* sind verantwortlich für die Behandlung der speziellen Aspekt des Webhooks von einem bestimmten Absender und zur Durchsetzung von sicherheitsüberprüfungen aus, um sicherzustellen, dass die WebHook-Anforderung tatsächlich den beabsichtigten Absender stammt.</span><span class="sxs-lookup"><span data-stu-id="d80a8-141">*Receivers* are responsible for handling the particular flavor of WebHook from a given sender and for enforcing security checks to ensure that the WebHook request indeed is from the intended sender.</span></span>

* <span data-ttu-id="d80a8-142">*Handler* sind in der Regel, in denen Benutzercode führt die Verarbeitung des bestimmten Webhooks.</span><span class="sxs-lookup"><span data-stu-id="d80a8-142">*Handlers* are typically where user code runs processing the particular WebHook.</span></span>

<span data-ttu-id="d80a8-143">In den folgenden Knoten werden diese Konzepte ausführlicher beschrieben.</span><span class="sxs-lookup"><span data-stu-id="d80a8-143">In the following nodes these concepts are described in more details.</span></span>
