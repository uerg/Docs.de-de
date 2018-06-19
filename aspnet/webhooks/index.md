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
# <a name="aspnet-webhooks-overview"></a>ASP.NET WebHooks (Übersicht)

WebHooks ist eine einfache HTTP-Muster, die eine einfache und Abonnementmodell für Verkabelung zusammen Web-APIs und SaaS-Dienste bereitstellen. Wenn ein Ereignis in einem Dienst auftritt, wird eine Benachrichtigung an die registrierten Abonnenten in Form einer HTTP POST-Anforderung gesendet. Die POST-Anforderung enthält Informationen zum Ereignis, wodurch es möglich, dass der Empfänger entsprechend reagieren.

Aufgrund ihrer Einfachheit WebHooks bereits verfügbar gemacht werden eine große Anzahl von Diensten, einschließlich [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp ](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/), und vieles mehr. Ein WebHook kann z. B. anzeigen, eine Datei geändert hat [Dropbox](http://dropbox.com/), hat eine Änderung des Codes in GitHub ein Commit ausgeführt wurde, oder eine Zahlung wurde initiiert [PayPal](http://www.paypal.com/), oder eine Karte erstellt wurde, [ Trello](http://www.trello.com/). Mögliche Werte sind endlos!

Microsoft ASP.NET WebHooks vereinfacht das Senden und Empfangen von WebHooks als Teil Ihrer ASP.NET-Anwendung:

* Auf der Empfangsseite stellt ein allgemeinen Modell zum Empfangen und Verarbeiten von WebHooks, die von einer beliebigen Anzahl von WebHook Anbietern bereit. Es ergibt sich aus der Box mit Unterstützung für [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[ WordPress](http://www.wordpress.com) und [Zendesk](https://www.zendesk.com/) , aber es ist einfach, zum Hinzufügen der Unterstützung für mehrere.

* Auf der sendenden Seite unterstützt verwalten und Speichern von Abonnements sowie wie für ereignisbenachrichtigungen an der richtigen Gruppe von Abonnenten gesendet. Dadurch können Sie Ihren eigenen Satz von Ereignissen zu definieren, geschieht Dinge zu benachrichtigen, dass Abonnenten abonnieren können.

Die beiden Teile können je nach Szenario voneinander oder zusammen verwendet werden. Wenn Sie nur WebHooks von anderen Diensten zu erhalten, können Sie nur die Empfänger Uhrzeitanteil verwenden müssen. Wenn Sie nur WebHooks für andere nutzen verfügbar machen möchten, können Sie genau das tun.

Der Code als Ziel verwendet ASP.NET Web API 2 und ASP.NET MVC 5 und steht als [OSS auf GitHub](https://github.com/aspnet/WebHooks).

## <a name="webhooks-overview"></a>WebHooks (Übersicht)

WebHooks ist ein Muster, was bedeutet, dass es variiert wie er vom Dienst zum Dienst verwendet wird, jedoch die zugrunde liegende Idee gleich. Sie können sich vorstellen WebHooks als einfache Pub/Sub-Modell, in denen ein Benutzer Ereignisse, die an anderer Stelle geschieht abonnieren kann. Die ereignisbenachrichtigungen werden als HTTP POST-Anforderungen, die mit Informationen über das eigentliche Ereignis weitergegeben.

In der Regel enthält die HTTP-POST-Anforderung eine JSON-Objekt oder eine HTML-Formulardaten, die zum Auslösen der WebHook Absender, einschließlich Informationen über das Ereignis verursacht des Webhooks ermittelt. Angenommen, ein Beispiel für einen WebHook POST-Anforderungstext aus [GitHub](http://www.github.com/) als Ergebnis ein neues Problem wird geöffnet, in ein bestimmtes Repository wie folgt aussieht:

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

Um sicherzustellen, dass der WebHook tatsächlich den beabsichtigten Absender stammt, ist die POST-Anforderung in irgendeiner Weise gesichert und dann vom Empfänger bestätigt. Beispielsweise [GitHub WebHooks](https://developer.github.com/webhooks/) enthält ein *X-Hub-Signatur* HTTP-Header mit einem Hash des Anforderungstexts, der von der Implementierung des Empfängers überprüft wird, damit Sie nicht kümmern müssen.

Die WebHook-Fluss geht in der Regel etwa so aussehen:

* Der WebHook Absender Ereignisse verfügbar macht, denen ein Client abonnieren kann. Die Ereignisse beschreiben wahrnehmbare Änderungen am System beispielsweise, die ein neues Datenelement eingefügt, die ein Prozess abgeschlossen ist oder etwas anderes wurde.

* Der WebHook Empfänger abonniert, indem Sie registrieren einen WebHook bestehend aus vier Aspekte:

     1. Ein URI, in dem die ereignisbenachrichtigung in Form einer HTTP POST-Anforderung veröffentlicht werden sollte;

     2. Eine Reihe von Filtern, die Beschreibung der bestimmten Ereignisse, die für die WebHook ausgelöst werden soll;

     3. Ein für den geheimen Schlüssel dient zum Signieren der HTTP POST-Anforderung

     4. Zusätzliche Daten, d. h., in der HTTP POST-Anforderung eingeschlossen werden sollen. Dies kann z. B. zusätzliche HTTP-Header-Felder oder Eigenschaften, die im Hauptteil HTTP POST-Anforderung enthalten sein.

* Sobald ein Ereignis eintritt, wird die übereinstimmenden WebHook Registrierungen gefunden wurden, und HTTP POST-Anforderungen gesendet werden. Die Generierung von HTTP POST-Anforderungen sind in der Regel mehrere Male wiederholt, falls für aus irgendeinem Grund, dass der Empfänger nicht antwortet oder die HTTP POST-Anforderung-Ergebnisse in eine Fehlerantwort.

## <a name="webhooks-processing-pipeline"></a>WebHooks Verarbeitungspipeline

Die Microsoft ASP.NET WebHooks Verarbeitungspipeline für eingehende WebHooks sieht wie folgt:

![ASP.NET WebHooks Verarbeitungspipeline](_static/WebHookReceivers.png)

Die beiden wichtigsten Konzepte hier sind *Empfänger* und *Handler*:

* *Empfänger* sind verantwortlich für die Behandlung der bestimmten WebHook-Typ von einem bestimmten Absender und zum Erzwingen von sicherheitsüberprüfungen, um sicherzustellen, dass die WebHook-Anforderung tatsächlich den beabsichtigten Absender stammt.

* *Handler* sind in der Regel, in denen Benutzercode führt die Verarbeitung aus bestimmten Webhooks.

In den folgenden Knoten werden diese Konzepte ausführlicher beschrieben.
