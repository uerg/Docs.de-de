---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: "Aktion führt in Web-API 2 | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/03/2014
ms.topic: article
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: 68b82661b97434795e1c306b168033dfcde529bc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="action-results-in-web-api-2"></a>Aktionsergebnisse in Web-API 2
====================
durch [Mike Wasson](https://github.com/MikeWasson)

In diesem Thema wird beschrieben, wie ASP.NET Web-API den Rückgabewert von eine Controlleraktion in einer HTTP-Antwort-Nachricht konvertiert.

Eine Controlleraktion der Web-API kann Folgendes zurückgeben:

1. void
2. **Httpresponsemessage zurück**
3. **IHttpActionResult**
4. Ein anderer Typ

Je nachdem, welche davon wird zurückgegeben, Web-API einen anderen Mechanismus verwendet, um die HTTP-Antwort zu erstellen.

| Rückgabetyp | Wie Web-API die Antwort erstellt |
| --- | --- |
| void | Zurückgeben von leeren 204 (kein Inhalt) |
| **Httpresponsemessage zurück** | Konvertieren Sie direkt in ein HTTP-Antwortnachricht. |
| **IHttpActionResult** | Rufen Sie **ExecuteAsync** zum Erstellen einer **HttpResponseMessage**, dann die Umstellung auf eine HTTP-Antwortnachricht. |
| Andere Typ | Schreiben Sie den serialisierten Rückgabewert in den Antworttext. 200 (OK) zurück |

Im weiteren Verlauf dieses Themas werden alle im Detail beschrieben.

## <a name="void"></a>void

Wenn der Rückgabetyp ist `void`, Web-API gibt einfach eine leere HTTP-Antwort mit dem Statuscode 204 (kein Inhalt).

Beispiel-Controller:

[!code-csharp[Main](action-results/samples/sample1.cs)]

HTTP-Antwort:

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a>Httpresponsemessage zurück

Wenn die Aktion gibt eine [HttpResponseMessage](https://msdn.microsoft.com/en-us/library/system.net.http.httpresponsemessage.aspx), Web-API konvertiert den zurückgegeben Wert direkt in einen HTTP-Antwortnachricht mithilfe der Eigenschaften von der **HttpResponseMessage** zu füllende Objekt die Antwort.

Diese Option bietet Sie ein hohes Maß an Kontrolle über die Antwortnachricht. Beispielsweise legt die folgenden Controlleraktion Cache-Control-Headers.

[!code-csharp[Main](action-results/samples/sample3.cs)]

Antwort:

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

Wenn Sie ein Domänenmodell, übergeben die **CreateResponse** -Methode, die Web-API verwendet einen [medienformatierer](../formats-and-model-binding/media-formatters.md) zum Schreiben des serialisierten Modells in den Antworttext.

[!code-csharp[Main](action-results/samples/sample5.cs)]

Web-API verwendet die Accept-Header in der Anforderung, um den Formatierer auszuwählen. Weitere Informationen finden Sie unter [Inhaltsaushandlung](../formats-and-model-binding/content-negotiation.md).

## <a name="ihttpactionresult"></a>IHttpActionResult

Die **IHttpActionResult** Schnittstelle in Web-API 2 eingeführt wurde. Im Wesentlichen definiert ein **HttpResponseMessage** Factory. Hier sind einige Vorteile der Verwendung der **IHttpActionResult** Schnittstelle:

- Vereinfacht die [UnitTests](../testing-and-debugging/unit-testing-controllers-in-web-api.md) Controller.
- Verschiebt die allgemeine Logik für das Erstellen von HTTP-Antworten in gesonderte Klassen.
- Macht die Absicht Controlleraktion klarer, indem Sie die Details auf niedriger Ebene beim Erstellen der Antwort ausblenden an.

**IHttpActionResult** enthält eine einzelne Methode **ExecuteAsync**, erstellt der asynchron eine **HttpResponseMessage** Instanz.

[!code-csharp[Main](action-results/samples/sample6.cs)]

Wenn eine Controlleraktion zurückgibt ein **IHttpActionResult**, Web-API-Aufrufe der **ExecuteAsync** Methode zum Erstellen einer **HttpResponseMessage**. Er konvertiert die **HttpResponseMessage** in eine HTTP-Antwortnachricht gesendet.

Hier ist eine einfache Erfassungsgruppen des **IHttpActionResult** , der eine nur-Text-Antwort erstellt:

[!code-csharp[Main](action-results/samples/sample7.cs)]

Beispiel-Controlleraktion:

[!code-csharp[Main](action-results/samples/sample8.cs)]

Antwort:

[!code-console[Main](action-results/samples/sample9.cmd)]

Je öfter, verwenden Sie die **IHttpActionResult** Implementierungen, die definiert, der  **[System.Web.Http.Results](https://msdn.microsoft.com/en-us/library/system.web.http.results.aspx)**  Namespace. Die **ApiController** Klasse definiert Hilfsmethoden, die diese integrierte Aktionsergebnisse zurückgeben.

Im folgenden Beispiel die Anforderung eine vorhandenes Produkt-ID stimmt nicht überein der Controller aufruft [ApiController.NotFound](https://msdn.microsoft.com/en-us/library/system.web.http.apicontroller.notfound.aspx) zum Erstellen einer Antwort 404 (Nichtgefunden). Der Controller, andernfalls ruft [ApiController.OK](https://msdn.microsoft.com/en-us/library/dn314591.aspx), die eine Antwort mit 200 (OK), erstellt das Produkt enthält.

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a>Andere Rückgabetypen

Für alle anderen Rückgabetypen Web-API verwendet einen [medienformatierer](../formats-and-model-binding/media-formatters.md) zum Serialisieren des Rückgabewerts. Web-API schreibt den serialisierten Wert in den Antworttext. Statuscode der Antwort ist 200 (OK).

[!code-csharp[Main](action-results/samples/sample11.cs)]

Ein Nachteil dieses Ansatzes ist, dass Sie direkt einen Fehlercode, z. B. 404 zurückgeben können. Sie können jedoch Auslösen einer **HttpResponseException** für Fehlercodes. Weitere Informationen finden Sie unter [Exception Handling in ASP.NET Web API](../error-handling/exception-handling.md).

Web-API verwendet die Accept-Header in der Anforderung, um den Formatierer auszuwählen. Weitere Informationen finden Sie unter [Inhaltsaushandlung](../formats-and-model-binding/content-negotiation.md).

Beispiel für eine Anforderung

[!code-console[Main](action-results/samples/sample12.cmd)]

Beispielantwort:

[!code-console[Main](action-results/samples/sample13.cmd)]
