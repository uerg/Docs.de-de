---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: Komponententests für Controller in ASP.NET Web-API 2 | Microsoft-Dokumentation
author: MikeWasson
description: Dieses Thema beschreibt, welche Techniken für Komponententests für Controller in Web-API 2. Bevor Sie dieses Thema lesen, sollten Sie das Tutorial Einheit zu lesen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/11/2014
ms.topic: article
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 1a3cfa1962a5f914fd2393088bec4424f6453d07
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389382"
---
<a name="unit-testing-controllers-in-aspnet-web-api-2"></a>Komponententests für Controller in ASP.NET Web-API 2
====================
durch [Mike Wasson](https://github.com/MikeWasson)

> Dieses Thema beschreibt, welche Techniken für Komponententests für Controller in Web-API 2. Bevor Sie dieses Thema lesen, sollten, lesen das Tutorial [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), erfahren, wie Sie ein Komponententestprojekt der Projektmappe hinzufügen.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> - [Visual Studio 2017](https://www.visualstudio.com/vs/)
> - Web-API 2
> - [Moq](https://github.com/Moq) 4.5.30

> [!NOTE]
> Ich habe die Moq verwendet, aber das gleiche Prinzip gilt für alle Mockframework. Moq 4.5.30 (und höher) unterstützt Visual Studio 2017, Roslyn und .NET 4.5 und höheren Versionen.

Ein häufiges Muster in Komponententests ist &quot;anordnen-Act-assert&quot;:

- Anordnen: Richten Sie alle Voraussetzungen für den Test ausführen.
- ACT: Führen Sie den Test.
- Assert-: Stellen Sie sicher, dass der Test erfolgreich war.

Im Schritt anordnen wird häufig Mock oder stub-Objekte. Das minimiert der Anzahl der Abhängigkeiten, damit der Test auf einen Aspekt orientiert.

Hier sind einige Dinge, die Sie in Ihren Web-API-Controllern Komponententests sollten:

- Die Aktion gibt die richtige Art der Antwort zurück.
- Ungültige Parameter geben die richtige Fehlermeldung.
- Die Aktion ruft die richtige Methode auf der Ebene Repository oder den Dienst an.
- Wenn die Antwort ein Domänenmodell enthält, überprüfen Sie den Typ des Modells.

Dies sind einige allgemeine Dinge zu testen, aber die Details hängen von der controllerimplementierung. Insbesondere macht es einen großen Unterschied, ob Ihre Controlleraktionen zurückgeben **HttpResponseMessage** oder **IHttpActionResult**. Weitere Informationen zu dieser Ergebnistypen, finden Sie unter [Aktionsergebnisse in Web-Api 2](../getting-started-with-aspnet-web-api/action-results.md).

## <a name="testing-actions-that-return-httpresponsemessage"></a>Testen von Aktionen, die HttpResponseMessage zurückgegeben

Hier ist ein Beispiel für einen Controller, deren Rückgabetyp Aktionen **HttpResponseMessage**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

Beachten Sie, dass der Controller verwendet abhängigkeitseinfügung zum Einfügen einer `IProductRepository`. Das kann für den Controller besser getestet werden, da Sie eine simulierte Repository einfügen können. Im folgenden UnitTest überprüft, ob die `Get` -Methode schreibt eine `Product` in den Antworttext. Vorausgesetzt, dass `repository` ist ein Mock `IProductRepository`.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

Es ist wichtig, legen Sie **anfordern** und **Konfiguration** auf dem Controller. Andernfalls schlägt der Test fehl, mit einem **"ArgumentNullException"** oder **"InvalidOperationException"**.

## <a name="testing-link-generation"></a>Erstellen von links testen

Die `Post` Methodenaufrufe **UrlHelper.Link** Links in der Antwort zu erstellen. Dies erfordert etwas mehr Einstellungen im Komponententest:

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

Die **UrlHelper** Klasse benötigt die Anforderungsdaten-URL und der Route, damit der Test Werte für diese festlegen muss. Eine andere Möglichkeit besteht, Mock oder Stub **UrlHelper**. Bei diesem Ansatz ersetzen Sie den Standardwert [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) mit einer Simulation oder Stub-Version, die einen festen Wert zurückgibt.

Ändern wir den Test mit der [Moq](https://github.com/Moq) Framework. Installieren Sie die `Moq` NuGet-Paket im Projekt.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

In dieser Version nicht erforderlich, um alle Routendaten einzurichten da das Mock **UrlHelper** gibt eine Konstante Zeichenfolge zurück.


## <a name="testing-actions-that-return-ihttpactionresult"></a>Testen von Aktionen, die IHttpActionResult zurückgeben

In Web-API 2, kann eine Controlleraktion zurückgeben **IHttpActionResult**, dies ist analog zu **ActionResult** in ASP.NET MVC. Die **IHttpActionResult** Schnittstelle definiert ein Befehlsmuster zum Erstellen von HTTP-Antworten. Anstatt die Antwort direkt erstellen, der Controller gibt eine **IHttpActionResult**. Später, der die Pipeline aufruft der **IHttpActionResult** zum Erstellen der Antwort. Dieser Ansatz erleichtert es, Schreiben von Komponententests, da Sie einen Großteil der Einrichtung, die überspringen können für die benötigte **HttpResponseMessage**.

Hier ist ein Beispiel-Controller, deren Rückgabetyp Aktionen **IHttpActionResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

Dieses Beispiel zeigt einige allgemeine Muster, die mit **IHttpActionResult**. Sehen wir uns an wie Einheit testen.

### <a name="action-returns-200-ok-with-a-response-body"></a>Aktion gibt 200 (OK) mit einem Antworttext zurück.

Die `Get` Methodenaufrufe `Ok(product)` Wenn das Produkt gefunden wird. Stellen Sie sicher, dass der Rückgabetyp ist im Komponententest, **OkNegotiatedContentResult** und das zurückgegebene Produkt verfügt über die richtigen-ID.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

Beachten Sie, dass der Komponententest nicht das Aktionsergebnis ausführt. Sie können davon ausgehen, dass das Aktionsergebnis die HTTP-Antwort ordnungsgemäß erstellt. (Daher der Web-API-Framework verfügt über eine eigene Komponententests!)

### <a name="action-returns-404-not-found"></a>Aktion gibt 404 (nicht gefunden) zurück.

Die `Get` Methodenaufrufe `NotFound()` Wenn das Produkt nicht gefunden wird. Für diesen Fall den Komponententest nur überprüft, wenn der Rückgabetyp ist **NotFoundResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a>Aktion gibt 200 (OK) ohne Antworttext zurück.

Die `Delete` Methodenaufrufe `Ok()` eine leere HTTP 200-Antwort zurückgegeben. Wie im vorherigen Beispiel, überprüft der UnitTest den Rückgabetyp, in diesem Fall **OkResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a>Aktion gibt 201 (erstellt) mit einem Location-Header zurück.

Die `Post` Methodenaufrufe `CreatedAtRoute` eine HTTP 201-Antwort mit einem URI im Location-Header zurückgegeben. Im Komponententest stellen Sie sicher, dass die Aktion, die richtigen routing Werte festlegt.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a>Aktion gibt eine andere 2xx mit einem Antworttext zurück.

Die `Put` Methodenaufrufe `Content` eine HTTP 202 (akzeptiert)-Antwort mit einem Antworttext zurückgegeben. Diesen Fall ähnelt der 200 (OK) zurückgeben, aber der Komponententest sollten auch den Statuscode überprüfen.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Simulieren des Entitätsframework bei Komponententests für ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- [Schreiben von Tests für eine ASP.NET Web-API-Dienst](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (Blogbeitrag vom Youssef Moussaoui).
- [Debuggen von ASP.NET Web-API mit dem Routendebugger](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
