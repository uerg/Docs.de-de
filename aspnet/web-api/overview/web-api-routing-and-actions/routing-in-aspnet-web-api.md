---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Routing in ASP.NET Web-API | Microsoft Docs
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/11/2012
ms.topic: article
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: aa0ecc96029051fef6a81ac08f7fcf52a24de59c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="routing-in-aspnet-web-api"></a>Routing in ASP.NET Web-API
====================
durch [Mike Wasson](https://github.com/MikeWasson)

In diesem Artikel wird beschrieben, wie ASP.NET Web-API-HTTP-Anfragen an Domänencontroller weiterleitet.

> [!NOTE]
> Wenn Sie mit ASP.NET MVC vertraut sind, ist das routing der Web-API MVC-routing sehr ähnlich. Der Hauptunterschied besteht darin, dass Web-API den HTTP-Methode, die nicht den URI-Pfad verwendet, um die Aktion auswählen. Sie können auch die MVC-Stil routing in der Web-API verwenden. In diesem Artikel geht keine Kenntnisse über ASP.NET MVC.


## <a name="routing-tables"></a>Routingtabellen

In ASP.NET Web-API eine *Controller* ist eine Klasse, die HTTP-Anforderungen verarbeitet. Die öffentlichen Methoden des Controllers heißen *Aktionsmethoden* oder einfach *Aktionen*. Wenn die Web-API-Framework eine Anforderung empfängt, leitet die Anforderung an eine Aktion weiter.

Um zu bestimmen, welche Aktion aufrufen, das Framework verwendet eine *Routingtabelle*. Die Visual Studio-Projektvorlage für Web-API erstellt eine Standardroute an:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

Diese Route wird definiert, in der WebApiConfig.cs-Datei, die in der App abgelegt wird\_Startverzeichnis:

![](routing-in-aspnet-web-api/_static/image1.png)

Weitere Informationen zu den **"webapiconfig"** Klasse, finden Sie unter [Konfigurieren von ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).

Wenn Sie Web-API selbst hosten, müssen Sie die Routingtabelle festlegen, direkt auf die **HttpSelfHostConfiguration** Objekt. Weitere Informationen finden Sie unter [Selbsthosting einer Web-API](../older-versions/self-host-a-web-api.md).

Jeder Eintrag in der Routingtabelle enthält eine *routenvorlage*. Die Standardvorlage für die Route für die Web-API ist &quot;api / {Controller} / {Id}&quot;. In dieser Vorlage &quot;api&quot; ist ein literal Pfadsegment und {Controller} und {Id} Platzhaltervariablen sind.

Wenn die Web-API-Framework eine HTTP-Anforderung empfängt, wird versucht, den URI für eine der routenvorlagen in der Routingtabelle zuzuordnen. Wenn keine Route übereinstimmt, empfängt der Client einen 404-Fehler. Z. B. die folgenden URIs entspricht die Standardroute:

- / api/Kontakte
- /API/Contacts/1
- /API/Products/gizmo1

Allerdings der folgende URI stimmt nicht überein, da es fehlt die &quot;api&quot; Segment:

- / Contacts/1

> [!NOTE]
> Der Grund für die Verwendung von "-api" in der Route ist mit ASP.NET MVC-Routings Konflikte zu vermeiden. Auf diese Weise haben Sie &quot;/Kontakte&quot; wechseln Sie zu einer MVC-Controller und &quot;/api/Kontakte&quot; wechseln Sie zu einem Web-API-Controller. Wenn Sie diese Konvention nicht mögen, können Sie natürlich die Routentabelle Standardeinstellung ändern.

Sobald eine übereinstimmende Route gefunden wird, wählt Web-API der Controller und die Aktion an:

- Um den Controller zu suchen, Web-API fügt &quot;Controller&quot; auf den Wert von der *{Controller}* Variable.
- Um die Aktion zu suchen, Web-API prüft die HTTP-Methode, und sucht dann nach einer Aktion mit diesem Namen der HTTP-Methode, deren Name beginnt. Z. B. mit einer GET-Anforderung Web-API sucht eine Aktion, die mit beginnt &quot;abrufen... &quot;, wie z. B. &quot;GetContact&quot; oder &quot;GetAllContacts&quot;. Diese Konvention gilt nur für GET, POST, PUT- und DELETE-Methoden. Sie können andere HTTP-Methoden mithilfe von Attributen auf dem Controller aktivieren. Wir sehen später ein Beispiel an.
- Andere Platzhaltervariablen in der routenvorlage, wie z. B. *{Id},* Aktionsparameter zugeordnet sind.

Sehen wir uns ein Beispiel. Nehmen wir an, dass Sie die folgenden Controller definieren:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

Hier sind einige möglichen HTTP-Anforderungen, zusammen mit der Aktion, die für die einzelnen aufgerufen wird:

| HTTP-Methode | URI-Pfad | Aktion | Parameter |
| --- | --- | --- | --- |
| GET | API-Produkte | GetAllProducts | *(keine)* |
| GET | -API/Produkte/4 | GetProductById | 4 |
| DELETE | -API/Produkte/4 | DeleteProduct | 4 |
| BEREITSTELLEN | API-Produkte | *(keine Übereinstimmung)* |  |

Beachten Sie, dass die *{Id}* Segment des URIS, falls vorhanden, zugeordnet ist die *Id* Parameter der Aktion. In diesem Beispiel wird der Controller definiert zwei GET-Methoden, mit einer *Id* Parameter und eine ohne Parameter.

Beachten Sie auch, die die POST-Anforderung fehl, weil der Domänencontroller nicht definiert wird, eine &quot;Post... &quot; Methode.

## <a name="routing-variations"></a>Routing Varianten

Im vorherigen Abschnitt beschrieben, die grundlegende Routingmechanismus von ASP.NET Web-API. Dieser Abschnitt beschreibt einige Varianten.

### <a name="http-methods"></a>HTTP-Methoden

Anstatt die Namenskonvention für HTTP-Methoden zu verwenden, können Sie explizit die HTTP-Methode für eine Aktion angeben, werden, indem die Aktionsmethode mit den **HttpGet**, **HttpPut**, **HttpPost** , oder **HttpDelete** Attribut.

Im folgenden Beispiel wird die Methode FindProduct GET-Anforderungen zugeordnet:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

Um mehrere HTTP-Methoden für eine Aktion, oder um einen HTTP-Methoden als GET, PUT, POST und DELETE, verwenden Sie die **AcceptVerbs** Attribut, das eine Liste der HTTP-Methoden behandelt.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a>Weiterleitung und der Aktionsname

Mit dem routing Standardvorlage verwendet Web-API die HTTP-Methode, um die Aktion auswählen. Allerdings können Sie auch eine Route erstellen, in dem der Aktionsname im URI enthalten ist:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

In dieser routenvorlage die *{Aktion}* Parameter den Namen der Aktionsmethode auf dem Controller. Verwenden Sie in diesem Format Routing Attribute, um die zulässigen HTTP-Methoden angeben. Angenommen Sie, dass der Controller die folgende Methode hat:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

In diesem Fall würde eine GET-Anforderung für "api/Produkte/Informationen/1" die Details-Methode zuordnen. Diese Art der routing ähnelt dem ASP.NET MVC und kann für eine RPC-Stil API geeignet sein.

Sie können den Aktionsnamen überschreiben, indem die **ActionName** Attribut. Im folgenden Beispiel sind zwei Aktionen, die zugeordnet &quot;-api/Produkte/Miniaturansicht/*Id*. GET unterstützt, und die andere POST unterstützt:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a>Nicht-Aktionen

Um zu verhindern, dass eine Methode erste als Aktion aufgerufen wird, verwenden die **NonAction** Attribut. Dies signalisiert das Framework, dass die Methode nicht auf eine Aktion ist, selbst wenn andernfalls die Senderegeln abgeglichen werden würden.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a>Weiterführende Themen

In diesem Thema bereitgestellten einen allgemeinen Überblick über das routing. Weitere Informationen finden Sie unter [Routing und Aktionsauswahl](routing-and-action-selection.md), das genau wie das Framework einen URI zu einer Route übereinstimmt, wählt einen Controller aus und wählt dann die aufzurufende Aktion beschreibt.
