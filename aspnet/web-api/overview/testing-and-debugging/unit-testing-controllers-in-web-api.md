---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: UnitTests Controller in ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: "Dieses Thema beschreibt einige spezifischen Verfahren für die Komponententests Controller in Web-API 2 an. Bevor Sie dieses Thema lesen, sollten Sie das Lernprogramm Einheit gelesen..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/11/2014
ms.topic: article
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: bda5148a4c1553d70f3173de66371fbb8576e83f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="unit-testing-controllers-in-aspnet-web-api-2"></a>UnitTests Controller in ASP.NET Web API 2
====================
durch [Mike Wasson](https://github.com/MikeWasson)

> Dieses Thema beschreibt einige spezifischen Verfahren für die Komponententests Controller in Web-API 2 an. Bevor Sie dieses Thema lesen, sollten, lesen das Lernprogramm [Einheit Testen von ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), die zeigt, wie ein Komponententest Projekt zur Projektmappe hinzufügen.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> - [Visual Studio 2017](https://www.visualstudio.com/vs/)
> - Web-API 2
> - [Moq](https://github.com/Moq) 4.5.30

> [!NOTE]
> Ich Moq verwendet, aber das gleiche Prinzip gilt für alle pseudoframework. Moq 4.5.30 (und höher) unterstützt Visual Studio 2017, Roslyn und .NET 4.5 und höher.

Ist ein allgemeines Muster in Komponententests &quot;anordnen-Act-assert&quot;:

- Anordnen: Richten Sie alle erforderlichen Komponenten für den Test ausführen.
- ACT: Führen Sie den Test.
- Assert-: Stellen Sie sicher, dass der Test erfolgreich war.

In den Diagrammfelder Schritt wird häufig Mock oder stub-Objekte. Die minimiert die Anzahl der Abhängigkeiten, damit der Test, zum Testen von eins ausgerichtet ist.

Hier sind einige Punkte sollten Sie Komponententests in Ihre Web-API-Controller aus:

- Die Aktion gibt den richtigen Typ der Antwort zurück.
- Ungültige Parameter zurück, die richtige Fehlerantwort.
- Die Aktion ruft die richtige Methode auf der Ebene Repositorys oder Diensts an.
- Enthält die Antwort ein Domänenmodell, überprüfen Sie den Modelltyp.

Dies sind einige der allgemeinen Schritte zum Testen, aber die Einzelheiten hängen von der Controller-Implementierung. Insbesondere vereinfacht einen großen Unterschied, ob Ihre Controlleraktionen zurückgeben **HttpResponseMessage** oder **IHttpActionResult**. Weitere Informationen zu dieser Ergebnistypen, finden Sie unter [Aktionsergebnisse in Web-Api 2](../getting-started-with-aspnet-web-api/action-results.md).

## <a name="testing-actions-that-return-httpresponsemessage"></a>Testen von Aktionen, die HttpResponseMessage zurück.

Hier ist ein Beispiel eines Controllers, deren Rückgabetyp Aktionen **HttpResponseMessage**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

Beachten Sie, dass der Controller verwendet Abhängigkeitsinjektion zum Einfügen einer `IProductRepository`. Stellt den Controller mehr getestet werden können, da Sie eine simulierte Repository einfügen können. Im folgenden UnitTest stellt sicher, dass die `Get` -Methode schreibt eine `Product` in den Antworttext. Angenommen, `repository` ist ein Mock `IProductRepository`.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

Es ist wichtig, legen Sie **anfordern** und **Konfiguration** auf dem Controller. Andernfalls schlägt der Test fehl, mit einer **ArgumentNullException** oder **InvalidOperationException**.

## <a name="testing-link-generation"></a>Testen die Linkgenerierung

Die `Post` Methodenaufrufe **UrlHelper.Link** zum Erstellen von Links in der Antwort. Dies erfordert ein wenig mehr Setup im Komponententest:

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

Die **UrlHelper** Klasse benötigt der Anforderungsdaten URL und die Routenwerte verwendet werden, weshalb der Test für diese Werte festgelegt. Eine andere Möglichkeit besteht, Mock oder Stub **UrlHelper**. Bei diesem Ansatz, ersetzen Sie den Standardwert [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) mit einer Mock oder Stub-Version, die einen festen Wert zurückgibt.

Wir schreiben Sie den Test mit der [Moq](https://github.com/Moq) Framework. Installieren der `Moq` NuGet-Paket im Testprojekt befindet.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

In dieser Version nicht müssen Sie zum Einrichten der Routendaten, da das Mock **UrlHelper** gibt eine Konstante Zeichenfolge zurück.


## <a name="testing-actions-that-return-ihttpactionresult"></a>Testen von Aktionen, die IHttpActionResult zurückgeben

In der Web-API 2, kann eine Controlleraktion zurückgeben **IHttpActionResult**, dies ist analog zu **ActionResult** in ASP.NET MVC. Die **IHttpActionResult** Schnittstelle definiert einen Befehlsmuster zum Erstellen von HTTP-Antworten. Anstelle die Antwort direkt erstellen, gibt der Controller eine **IHttpActionResult**. Die Pipeline später, ruft der **IHttpActionResult** um die Antwort zu erstellen. Dieser Ansatz erleichtert es, Schreiben von Komponententests, da Sie viele der Setup-, die überspringen können für die benötigte **HttpResponseMessage**.

Hier ist ein Beispiel-Controller, deren Rückgabetyp Aktionen **IHttpActionResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

Dieses Beispiel zeigt einige allgemeine Muster mit **IHttpActionResult**. Sehen wir, wie Einheit testen.

### <a name="action-returns-200-ok-with-a-response-body"></a>Aktion gibt 200 (OK) mit einem Antworttext zurück.

Die `Get` Methodenaufrufe `Ok(product)` Wenn das Produkt gefunden wurde. Im Komponententest, stellen Sie sicher, dass der Rückgabetyp ist **OkNegotiatedContentResult** und das zurückgegebene Produkt verfügt über die richtigen-ID.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

Beachten Sie, dass der Komponententest das Aktionsergebnis ausgeführt wird nicht. Sie können davon ausgehen, dass das Aktionsergebnis die HTTP-Antwort ordnungsgemäß erstellt. (Diesem Grund wird die Web-API-Framework verfügt über einen eigenen Komponententests!)

### <a name="action-returns-404-not-found"></a>Aktion gibt 404 (Nichtgefunden) zurück.

Die `Get` Methodenaufrufe `NotFound()` Wenn das Produkt nicht gefunden wird. Für diesen Fall den Komponententest nur überprüft, wenn der Rückgabetyp ist **NotFoundResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a>Aktion gibt 200 (OK) mit keinen Antworttext zurück.

Die `Delete` Methodenaufrufe `Ok()` leerantwort HTTP 200 zurückgegeben. Wie im vorherigen Beispiel überprüft der Komponententest den Rückgabetyp, in diesem Fall **OkResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a>Aktion gibt 201 (erstellt) mit einem Location-Header zurück.

Die `Post` Methodenaufrufe `CreatedAtRoute` eine Antwort "HTTP 201" mit einem URI im Location-Header zurückgegeben. In den Komponententest stellen Sie sicher, dass die Aktion die richtigen routing Werte festlegt.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a>Aktion gibt eine andere 2xx mit einen Antworttext zurück.

Die `Put` Methodenaufrufe `Content` zum Zurückgeben einer Antwort HTTP 202 (akzeptiert) mit einen Antworttext. Dies der Fall ist ähnlich wie 200 (OK) zurückgegeben, sondern der Komponententest sollte auch den Statuscode überprüfen.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Simulieren von Entity Framework bei Komponententests für ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- [Schreiben von Tests für ASP.NET Web API-Dienst](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (Blogbeitrag vom Youssef Moussaoui).
- [Debuggen von ASP.NET Web-API mit dem Routendebugger](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
