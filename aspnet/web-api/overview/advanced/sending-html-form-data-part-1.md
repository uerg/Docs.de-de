---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: 'Senden von HTML-Formulardaten in ASP.NET Web-API: Form-Urlencoded-Daten | Microsoft-Dokumentation'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/15/2012
ms.topic: article
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 24410c92df828d4aaaa3b91dd3e9fa14575fd300
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399869"
---
<a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a>Senden von HTML-Formulardaten in ASP.NET Web-API: Form-Urlencoded-Daten
====================
durch [Mike Wasson](https://github.com/MikeWasson)

## <a name="part-1-form-urlencoded-data"></a>Teil 1: Form-Urlencoded-Daten

In diesem Artikel veranschaulicht das Form-Urlencoded-Daten an einen Web-API-Controller zu veröffentlichen.

- [Übersicht über die HTML-Formularen](#overview_of_html_forms)
- [Senden von komplexen Typen](#sending_complex_types)
- [Senden von Formulardaten per AJAX](#sending_form_data_via_ajax)
- [Senden von einfachen Typen](#sending_simple_types)

> [!NOTE]
> [Herunterladen des abgeschlossenen Projekts](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).


<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a>Übersicht über die HTML-Formularen

HTML-Formulare verwenden wird entweder GET oder POST zum Senden von Daten an den Server. Die **Methode** Attribut der **Formular** -Element können die HTTP-Methode:

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

Die Standardmethode ist GET. Wenn das Formular verwendet zu erhalten, das Formular, das Daten in den URI als Zeichenfolge codiert ist. Wenn das Formular POST verwendet wird, werden die Formulardaten im Anforderungstext platziert. Für Gebucht-Daten die **Enctype** Attribut gibt an, das Format des Anforderungstexts:

| Enctype | Beschreibung |
| --- | --- |
| application/x-www-form-urlencoded | Als Name/Wert-Paare, ähnlich wie eine URI-Abfragezeichenfolge ist die Formulardaten codiert. Dies ist das Standardformat für POST-Methode. |
| Multipart/Form-data | Formulardaten werden als eine mehrteilige MIME-Nachricht codiert. Verwenden Sie dieses Format, wenn Sie eine Datei mit dem Server hochladen. |

Teil 1 dieses Artikels untersucht die X-www-form-urlencoded-Format. [Teil 2](sending-html-form-data-part-2.md) mehrteiligen MIME-Nachrichten beschrieben.

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a>Senden von komplexen Typen

Senden Sie in der Regel einen komplexen Typ, bestehend aus Werten aus mehrere Steuerelemente des Formulars. Betrachten Sie das folgende Modell, das eine statusaktualisierung darstellt:

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

Hier ist ein Web-API-Controller, die akzeptiert eine `Update` Objekt über POST.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> Dieser Controller verwendet [Aktion-basiertes routing](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), sodass die routenvorlage &quot;api / {Controller} / {Action} / {Id}&quot;. Der Client wird die Daten zu buchen &quot;/api/updates/complex&quot;.


Jetzt schreiben wir nun ein HTML-Formular für Benutzer, eine statusaktualisierung zu übermitteln.

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

Beachten Sie, dass die **Aktion** Attribut des Formulars ist der URI des unsere Controlleraktion. So sieht das Formular mit einigen in eingegebenen Werten aus:

![](sending-html-form-data-part-1/_static/image1.png)

Klickt der Benutzer senden, sendet der Browser eine HTTP-Anforderung ähnelt dem folgenden:

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

Beachten Sie, dass der Anforderungstext Daten aus dem Formular, formatiert als Name/Wert-Paare enthält. Web-API konvertiert automatisch Name/Wert-Paare in einer Instanz von der `Update` Klasse.

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a>Senden von Formulardaten per AJAX

Wenn ein Benutzer ein Formular übermittelt, wird der Browser von der aktuellen Seite weg navigiert und rendert den Text der Antwortnachricht. Das ist in Ordnung Wenn die Antwort auf eine HTML-Seite ist. Mit einer Web-API, der Antworttext ist jedoch in der Regel entweder leer ist oder strukturierte Daten, z.B. JSON enthält. In diesem Fall macht es mehr Sinn, die zum Senden von Daten aus dem Formular mit einer AJAX-Anforderung, damit an, dass die Antwort von die Seite verarbeitet werden kann.

Der folgende Code zeigt, wie unter Verwendung von jQuery Formulardaten übermittelt wird.

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

Die jQuery **übermitteln** -Funktion ersetzt die Formularaktion mit einer neuen Funktion. Dies überschreibt das Standardverhalten der Senden-Schaltfläche. Die **Serialisieren** Funktion serialisiert Daten aus dem Formular in Name/Wert-Paaren. Rufen Sie zum Senden von Daten aus dem Formular an den Server `$.post()`.

Wenn die Anforderung abgeschlossen ist, die `.success()` oder `.error()` Handler dem Benutzer eine entsprechende Meldung angezeigt.

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a>Senden von einfachen Typen

In den vorherigen Abschnitten haben wir einen komplexen Typ, der Web-API mit einer Instanz von einer Modellklasse deserialisiert gesendet. Sie können auch einfache Typen, z. B. eine Zeichenfolge senden.

> [!NOTE]
> Senden einen einfachen Typ, berücksichtigen Sie stattdessen den Wert in einem komplexen Typ umschließt. Dies bietet Ihnen die Vorteile der modellvalidierung auf der Serverseite und erleichtert es, Ihr Modell zu erweitern, wenn erforderlich.


Die grundlegenden Schritte zum Senden von eines einfachen Typs sind identisch, aber es gibt zwei feine Unterschiede. Zunächst im Controller muss, ergänzen Sie den Namen des Parameters mit dem **FromBody** Attribut.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

Web-API versucht standardmäßig einfache Typen aus der Anforderungs-URI abrufen. Die **FromBody** -Attribut weist die Web-API zum Lesen des Werts aus dem Anforderungstext.

> [!NOTE]
> Web-API liest den Antworttext nur einen Parameter einer Aktion aus dem Anforderungstext stammen kann wird höchstens einmal zurückgegeben. Wenn Sie mehrere Werte aus dem Anforderungstext abrufen müssen, definieren Sie einen komplexen Typ.


Zweitens muss der Client zum Senden von des Wert mit dem folgenden Format ein:

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

Insbesondere muss der Namensteil von Name/Wert-Paar für einen einfachen Typ leer sein. Nicht alle Browser unterstützen diese für die HTML-Formularen, aber Sie erstellen dieses Format im Skript wie folgt:

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

Hier ist ein Beispielformular:

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

Und hier ist das Skript aus, um den Formularwert zu übermitteln. Der einzige Unterschied besteht aus dem vorherigen Skript wird das übergebene Argument der **Posten** Funktion.

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

Sie können den gleichen Ansatz verwenden, um ein Array von einfachen Typen zu senden:

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Teil 2: Dateiupload und mehrteiligen MIME-](sending-html-form-data-part-2.md)
