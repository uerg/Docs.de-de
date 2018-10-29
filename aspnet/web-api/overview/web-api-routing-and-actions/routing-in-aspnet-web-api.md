---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Routing in ASP.NET Web-API | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/29/2018
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: a7bc998fc23c0453fc9cd6ac1e7b9af7bd516225
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207302"
---
<a name="routing-in-aspnet-web-api"></a>Routing in ASP.NET Web-API
====================
durch [Mike Wasson](https://github.com/MikeWasson)

In diesem Artikel wird beschrieben, wie ASP.NET Web-API die HTTP-Anforderungen an Controller weitergeleitet.

> [!NOTE]
> Wenn Sie mit ASP.NET MVC vertraut sind, ist ein routing der Web-API-MVC-routing sehr ähnlich. Der Hauptunterschied besteht darin, dass die Web-API die HTTP-Verb, nicht den URI-Pfad verwendet, um die Aktion auswählen. Sie können auch die MVC-Stil-routing in Web-API verwenden. In diesem Artikel werden keine Kenntnisse von ASP.NET MVC vorausgesetzt.

## <a name="routing-tables"></a>Routingtabellen

In ASP.NET Web-API eine *Controller* ist eine Klasse, die HTTP-Anforderungen verarbeitet. Die öffentlichen Methoden des Controllers aufgerufen werden *Aktionsmethoden* oder einfach *Aktionen*. Wenn die Web-API-Framework eine Anforderung empfängt, leitet sie die Anforderung mit einer Aktion.

Um zu bestimmen, welche Aktion aufrufen, das Framework verwendet eine *Routingtabelle*. Die Visual Studio-Projektvorlage für Web-API erstellt eine Standardroute:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

Diese Route wird definiert, der *WebApiConfig.cs* -Datei, die in platziert wird die *App\_starten* Verzeichnis:

![](routing-in-aspnet-web-api/_static/image1.png)

Weitere Informationen zu den `WebApiConfig` Klasse, finden Sie unter [Konfigurieren von ASP.NET Web-API](../advanced/configuring-aspnet-web-api.md).

Wenn Sie Web-API selbst hosten, müssen Sie die Routingtabelle festlegen, direkt auf die `HttpSelfHostConfiguration` Objekt. Weitere Informationen finden Sie unter [selbst hosten einer Web-API](../older-versions/self-host-a-web-api.md).

Jeder Eintrag in der Routingtabelle enthält eine *routenvorlage*. Die Route-Standardvorlage für Web-API ist &quot;api / {Controller} / {Id}&quot;. In dieser Vorlage &quot;api&quot; ist ein literal OData-Pfadsegment, und klicken Sie auf {Controller} und {Id} Platzhaltervariablen sind.

Wenn die Web-API-Framework eine HTTP-Anforderung empfängt, versucht, den URI für eine der routenvorlagen in der Routingtabelle überein. Wenn keine Route entspricht, erhält der Client einen 404-Fehler. Die folgenden URIs entspricht z. B. die Standardroute:

- / api/contacts
- /API/Contacts/1
- /API/Products/gizmo1

Allerdings der folgende URI stimmt nicht überein, da es fehlen die &quot;api&quot; Segment:

- / Contacts/1

> [!NOTE]
> Der Grund für die Verwendung von "api" in der Route ist Konflikte mit ASP.NET MVC-Routings zu vermeiden. Auf diese Weise kann man &quot;/kontaktiert&quot; wechseln Sie zu einem MVC-Controller und &quot;/api/Contacts&quot; wechseln Sie zu einem Web-API-Controller. Wenn Sie diese Konvention nicht gefällt, können Sie natürlich die Standardroutingtabelle ändern.

Wenn eine übereinstimmende Route gefunden wurde, wählt Web-API-Controller und die Aktion:

- Um den Controller zu suchen, Web-API fügt &quot;Controller&quot; auf den Wert des der *{Controller}* Variable.
- Um die Aktion zu suchen, Web-API untersucht das HTTP-Verb, und sucht dann nach einer Aktion, deren Name mit diesem Namen der HTTP-Verbs beginnt. Eine GET-Anforderung, z. B. Web-API sieht für eine Aktion mit dem Präfix &quot;erhalten&quot;, z. B. &quot;GetContact&quot; oder &quot;GetAllContacts&quot;. Diese Konvention gilt nur für abrufen, POST, PUT, DELETE, HEAD, Optionen und PATCHEN von Verben. Sie können andere HTTP-Verben aktivieren, auf dem Controller mithilfe von Attributen. Wir sehen später ein Beispiel.
- Andere Platzhaltervariablen in der routenvorlage, z. B. *{Id},* Action-Parameter zugeordnet sind.

Sehen wir uns ein Beispiel. Nehmen wir an, dass Sie den folgenden Controller definieren:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

Hier sind einige möglichen HTTP-Anforderungen zusammen mit der Aktion, die für die einzelnen aufgerufen wird:

| HTTP-Verb | URI-Pfad | Aktion | Parameter |
| --- | --- | --- | --- |
| GET | API/Produkte | GetAllProducts | *(keine)* |
| GET | API/Produkte/4 | GetProductById | 4 |
| DELETE | API/Produkte/4 | DeleteProduct | 4 |
| POST | API/Produkte | *(keine Übereinstimmung)* |  |

Beachten Sie, dass die *{Id}* -Segment des URI, falls vorhanden, zugeordnet ist die *Id* Parameter der Aktion. In diesem Beispiel ist der Controller definiert zwei GET-Methoden, mit einem *Id* Parameter und einen ohne Parameter.

Beachten Sie auch, die die POST-Anforderung fehl, da der Controller nicht definiert ist, eine &quot;Post... &quot; Methode.

## <a name="routing-variations"></a>Routing-Varianten

Im vorherigen Abschnitt beschrieben, die grundlegende Routingmechanismus von ASP.NET Web-API. Dieser Abschnitt beschreibt einige Varianten.

### <a name="http-verbs"></a>HTTP-Verben

Anstatt die Namenskonvention für HTTP-Verben zu verwenden, können Sie explizit das HTTP-Verb für eine Aktion angeben, werden, indem die Aktionsmethode mit einem der folgenden Attribute:

- `[HttpGet]`
- `[HttpPut]`
- `[HttpPost]`
- `[HttpDelete]`
- `[HttpHead]`
- `[HttpOptions]`
- `[HttpPatch]`

Im folgenden Beispiel die `FindProduct` Methode GET-Anforderungen zugeordnet ist:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

Um mehrere HTTP-Verben für eine Aktion zu ermöglichen, oder um HTTP-Verben als GET, PUT, POST, DELETE, HEAD, Optionen und PATCH zu ermöglichen, verwenden die `[AcceptVerbs]` -Attribut, das eine Liste der HTTP-Verben akzeptiert.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a>Routing durch den Namen der Aktion

Mit der Standard-routing-Vorlage verwendet die Web-API das HTTP-Verb zum Auswählen der Aktion. Allerdings können Sie auch eine Route erstellen, in dem der Name der Aktion im URI enthalten ist:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

In dieser routenvorlage die *{Action}* Parameternamen die Aktionsmethode im Controller. Verwenden Sie in diesem Format des Routings Attribute, um die zulässigen HTTP-Verben angeben. Nehmen wir beispielsweise an, dass Ihre Controller die folgende Methode verfügt:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

In diesem Fall würde eine GET-Anforderung für "api/Produkte/Details/1" zum Zuordnen der `Details` Methode. Diese Art von routing ist vergleichbar mit ASP.NET MVC, und für eine RPC-Stil API eignet sich möglicherweise.

Sie können den Aktionsnamen überschreiben, indem die `[ActionName]` Attribut. Im folgenden Beispiel sind zwei Aktionen, die den zuordnen &quot;-api/Produkte/Miniaturansicht/*Id*. Eine GET unterstützt, und die andere POST unterstützt:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a>Nicht-Aktionen

Um zu verhindern, dass eine Methode abrufen als Aktion aufgerufen wird, verwenden Sie die `[NonAction]` Attribut. Dies signalisiert das Framework, dass die Methode keine Aktion, auch wenn sie andernfalls die Senderegeln übereinstimmen.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a>Weiterführende Themen

In diesem Thema bereitgestellten einen allgemeinen Überblick über die Weiterleitung an. Weitere Informationen finden Sie unter [Routing- und Aktionsauswahl](routing-and-action-selection.md), das beschreibt, genau wie das Framework einen URI zu einer Route übereinstimmt, wählt einen Controller und wählt dann die aufzurufende Aktion.
