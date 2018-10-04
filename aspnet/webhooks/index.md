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
# <a name="aspnet-webhooks-overview"></a>Übersicht über die ASP.NET WebHooks

WebHooks ist ein einfacher HTTP-Muster, die eine einfache Veröffentlichungs-/Abonnementmodells verbinden zusammen Web-APIs und SaaS-Dienste bereitstellen. Wenn ein Ereignis in einem Dienst auftritt, wird eine Benachrichtigung an die registrierten Abonnenten in Form von HTTP POST-Anforderung gesendet. Die POST-Anforderung enthält Informationen über das Ereignis dadurch ist es möglich, dass der Empfänger entsprechend reagieren.

Aufgrund ihrer Einfachheit WebHooks sind bereits verfügbar gemacht werden durch eine große Anzahl von Diensten, einschließlich [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp ](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/), und vieles mehr. Ein WebHook kann beispielsweise angeben, dass es sich bei in eine Datei geändert hat [Dropbox](http://dropbox.com/), verfügt über eine Änderung des Codes auf GitHub ein Commit ausgeführt wurde, oder eine Zahlung wurde initiiert [PayPal](http://www.paypal.com/), oder in eine Karte erstellt wurde [ Trello](http://www.trello.com/). Die Möglichkeiten sind grenzenlos!

Microsoft ASP.NET WebHooks erleichtert es, senden und Empfangen von WebHooks als Teil Ihrer ASP.NET-Anwendung:

* Auf der Empfangsseite bietet es ein allgemeinen Modell zum Empfangen und Verarbeiten von WebHooks über eine beliebige Anzahl von WebHook-Anbieter. Es geht mit Unterstützung für [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[ WordPress](http://www.wordpress.com) und [Zendesk](https://www.zendesk.com/) , aber es ist einfach, Unterstützung für weitere hinzufügen.

* Auf der Absenderseite bietet Unterstützung für verwalten und speichern die Abonnements für ereignisbenachrichtigungen an den richtigen Satz an Abonnenten gesendet. Dadurch können Sie Ihre eigene Gruppe von Ereignissen definieren, dass Abonnenten abonnieren können, und benachrichtigen sie an, wenn Dinge passiert.

Die beiden Teile können je nach Szenario voneinander oder zusammen verwendet werden. Wenn Sie nur auf WebHooks von anderen Diensten empfangen müssen, können Sie nur den Empfänger Teil verwenden; Wenn Sie nur die WebHooks für andere Personen nutzen verfügbar machen möchten, können Sie genau das tun.

Der Code abzielt, ASP.NET Web API 2 und ASP.NET MVC 5 und steht als [OSS auf GitHub](https://github.com/aspnet/WebHooks).

## <a name="webhooks-overview"></a>Übersicht über die WebHooks

WebHooks ist ein Muster, die bedeutet, dass es variiert, wie sie vom Dienst zu Dienst verwendet werden, aber die grundlegende Idee identisch ist. Sie können sich vorstellen WebHooks als einfache Pub/Sub-Modell, in denen ein Benutzer an anderer Stelle die Ereignisse abonnieren kann. Die ereignisbenachrichtigungen werden als HTTP POST-Anforderungen, die mit Informationen über das Ereignis selbst weitergegeben.

In der Regel enthält die HTTP-POST-Anforderung ein JSON-Objekt oder eine HTML-Formulardaten, die zum Auslösen der WebHook-Absender, einschließlich Informationen über das Ereignis verursacht den WebHook ermittelt. Angenommen, ein Beispiel für eine WebHook-POST-Anforderungstext von [GitHub](http://www.github.com/) durch ein neues Problem geöffnet wird, in ein bestimmtes Repository wie folgt aussieht:

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

Um sicherzustellen, dass der WebHook tatsächlich den beabsichtigten Absender stammt, ist die POST-Anforderung in irgendeiner Weise gesichert und dann vom Empfänger überprüft. Z. B. [GitHub-WebHooks](https://developer.github.com/webhooks/) enthält ein *X-Hub-Signature* HTTP-Header mit einem Hash des Anforderungstexts, der durch die Implementierung für die Empfänger überprüft wird, sodass Sie keine weiteren Änderungen.

Der WebHook-Workflow wechselt in der Regel mit etwa wie folgt:

* Der WebHook-Absender Ereignisse verfügbar macht, denen ein Client abonnieren können. Die Ereignisse beschreiben wahrnehmbare Änderungen für das System, z. B., die ein neues Datenelement eingefügt, das, dass ein Prozess abgeschlossen wurde oder etwas anderes.

* Der WebHook-Empfänger abonniert durch Registrieren eines Webhooks, die vier Arten von Informationen aus:

     1. Ein URI für die, an die ereignisbenachrichtigung in Form von HTTP POST-Anforderung gesendet werden soll;

     2. Einen Satz von Filtern, beschreibt die bestimmte Ereignisse, die für die der WebHook ausgelöst werden soll;

     3. Einen geheimen Schlüssel, der zum Signieren von HTTP POST-Anforderung verwendet wird;

     4. Zusätzlicher Daten, die in der HTTP POST-Anforderung enthalten sein soll. Dies kann z. B. zusätzliche HTTP-Headerfelder oder Eigenschaften, die im Hauptteil HTTP POST-Anforderung enthalten sein.

* Sobald ein Ereignis eintritt, wird der entsprechenden WebHook-Registrierungen gefunden werden, und HTTP POST-Anforderungen gesendet werden. Die Generierung von HTTP POST-Anforderungen werden in der Regel mehrere Male wiederholt, wenn für die aus irgendeinem Grund, dass der Empfänger nicht antwortet oder die HTTP POST-Anforderung zu einer Fehlerantwort.

## <a name="webhooks-processing-pipeline"></a>Pipeline zur Anforderungsverarbeitung WebHooks

Die Microsoft ASP.NET WebHooks Verarbeitungspipeline für eingehende WebHooks sieht folgendermaßen aus:

![Pipeline zur Anforderungsverarbeitung ASP.NET WebHooks](_static/WebHookReceivers.png)

Die zwei wichtigsten Konzepte hier sind *Empfänger* und *Handler*:

* *Empfänger* sind verantwortlich für die Behandlung der speziellen Aspekt des Webhooks von einem bestimmten Absender und zur Durchsetzung von sicherheitsüberprüfungen aus, um sicherzustellen, dass die WebHook-Anforderung tatsächlich den beabsichtigten Absender stammt.

* *Handler* sind in der Regel, in denen Benutzercode führt die Verarbeitung des bestimmten Webhooks.

In den folgenden Knoten werden diese Konzepte ausführlicher beschrieben.
