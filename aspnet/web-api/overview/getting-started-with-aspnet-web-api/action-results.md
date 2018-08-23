---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: Dies führt Web-API 2 | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 02/03/2014
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: b2b5ae5e5cef19e75a184aa28ac838a31e5ef1fd
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828036"
---
<a name="action-results-in-web-api-2"></a>Aktionsergebnisse in Web-API 2
====================
durch [Mike Wasson](https://github.com/MikeWasson)

In diesem Thema wird beschrieben, wie ASP.NET Web-API den Rückgabewert von eine Controlleraktion in eine HTTP-Response-Nachricht konvertiert.

Eine Controlleraktion der Web-API kann Folgendes zurückgeben:

1. void
2. **HttpResponseMessage**
3. **IHttpActionResult**
4. Eine andere Art

Je nachdem, welche dieser zurückgegeben wird, Web-API einen anderen Mechanismus verwendet, um die HTTP-Antwort zu erstellen.

| Rückgabetyp | Wie die Antwort erstellt Web-API |
| --- | --- |
| void | Leere 204 (No Content) zurückgeben |
| **HttpResponseMessage** | Konvertieren Sie direkt in HTTP-Antwortnachricht. |
| **IHttpActionResult** | Rufen Sie **ExecuteAsync** zum Erstellen einer **HttpResponseMessage**, dann in HTTP-Antwortnachricht zu konvertieren. |
| Andere Art | Schreiben Sie den serialisierten Rückgabewert in den Antworttext. 200 (OK) zurück |

Im weiteren Verlauf dieses Themas wird jede Option ausführlicher beschrieben.

## <a name="void"></a>void

Wenn der Rückgabetyp ist `void`, Web-API gibt einfach eine leere HTTP-Antwort mit dem Statuscode 204 (No Content) zurück.

Beispiel-Controller:

[!code-csharp[Main](action-results/samples/sample1.cs)]

HTTP-Antwort:

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a>HttpResponseMessage

Wenn die Aktion gibt ein [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), Web-API konvertiert den Rückgabewert direkt in eine HTTP-Antwortnachricht mithilfe der Eigenschaften des der **HttpResponseMessage** zu füllende Objekt die die Antwort.

Diese Option erhalten Sie umfassende Kontrolle über die Response-Nachricht. Beispielsweise legt die folgende Controlleraktion Cache-Control-Headers.

[!code-csharp[Main](action-results/samples/sample3.cs)]

Antwort:

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

Wenn Sie ein Domänenmodell, übergeben die **CreateResponse** -Methode, die Web-API verwendet einen [medienformatierer](../formats-and-model-binding/media-formatters.md) das serialisierte Modell in den Antworttext schreiben.

[!code-csharp[Main](action-results/samples/sample5.cs)]

Web-API verwendet den Accept-Header in der Anforderung verwendet, um den Formatierer auszuwählen. Weitere Informationen finden Sie unter [Content Negotiation](../formats-and-model-binding/content-negotiation.md).

## <a name="ihttpactionresult"></a>IHttpActionResult

Die **IHttpActionResult** -Schnittstelle wurde in der Web-API 2 eingeführt. Im Wesentlichen definiert es ein **HttpResponseMessage** Factory. Hier sind einige Vorteile der Verwendung der **IHttpActionResult** Schnittstelle:

- Vereinfacht die [Komponententests](../testing-and-debugging/unit-testing-controllers-in-web-api.md) Ihren Controllern.
- Verschiebt die allgemeine Logik für das Erstellen von HTTP-Antworten in separate Klassen.
- Macht die Absicht der Controlleraktion klarer, durch das Ausblenden der Details auf niedriger Ebene, der die Antwort zu erstellen.

**IHttpActionResult** enthält eine einzelne Methode, **ExecuteAsync**, erstellt der asynchron eine **HttpResponseMessage** Instanz.

[!code-csharp[Main](action-results/samples/sample6.cs)]

Wenn eine Controlleraktion zurückgibt ein **IHttpActionResult**, Web-API-Aufrufe der **ExecuteAsync** Methode zum Erstellen einer **HttpResponseMessage**. Anschließend konvertiert der **HttpResponseMessage** in HTTP-Antwortnachricht.

Hier ist eine einfache Erfassungsgruppen des **IHttpActionResult** eine nur-Text-Antwort erstellt:

[!code-csharp[Main](action-results/samples/sample7.cs)]

Beispiel-Controlleraktion:

[!code-csharp[Main](action-results/samples/sample8.cs)]

Antwort:

[!code-console[Main](action-results/samples/sample9.cmd)]

Häufiger, verwenden Sie die **IHttpActionResult** Implementierungen, die definiert, der **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** Namespace. Die **ApiController** -Klasse definiert Hilfsmethoden, die diese integrierte Aktionsergebnisse zurückgeben.

Im folgenden Beispiel, wenn die Anforderung keine vorhandenen Produkt-ID übereinstimmt der Controller ruft [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) zum Erstellen einer 404 (nicht gefunden). Der Controller, andernfalls ruft [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), die einer Antwort 200 (OK), erstellt das Produkt enthält.

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a>Andere Rückgabetypen

Für alle anderen Rückgabetypen, Web-API verwendet einen [medienformatierer](../formats-and-model-binding/media-formatters.md) zum Serialisieren des zurückgegeben Wert. Web-API schreibt den serialisierten Wert in den Antworttext. Statuscode der Antwort ist 200 (OK).

[!code-csharp[Main](action-results/samples/sample11.cs)]

Ein Nachteil dieses Ansatzes ist, dass Sie direkt einen Fehlercode, z. B. 404 zurückgeben können. Sie können jedoch Folgendes Auslösen einer **HttpResponseException** Fehlercodes. Weitere Informationen finden Sie unter [Exception Handling in ASP.NET Web-API](../error-handling/exception-handling.md).

Web-API verwendet den Accept-Header in der Anforderung verwendet, um den Formatierer auszuwählen. Weitere Informationen finden Sie unter [Content Negotiation](../formats-and-model-binding/content-negotiation.md).

Beispiel für eine Anforderung

[!code-console[Main](action-results/samples/sample12.cmd)]

Beispielantwort:

[!code-console[Main](action-results/samples/sample13.cmd)]
