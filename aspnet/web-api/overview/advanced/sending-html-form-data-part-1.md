---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: 'Senden von HTML-Formulardaten in ASP.NET Web-API: Formular codierte Daten | Microsoft Docs'
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/15/2012
ms.topic: article
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 0ed339c4f9d5854ab5a21cdd077a4d494987101f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a>Senden von HTML-Formulardaten in ASP.NET Web-API: Formular codierte Daten
====================
durch [Mike Wasson](https://github.com/MikeWasson)

## <a name="part-1-form-urlencoded-data"></a>Teil 1: Formular codierte Daten

Dieser Artikel zeigt, wie Formular codierte Daten an einen Web-API-Controller.

- [Übersicht über die HTML-Formularen](#overview_of_html_forms)
- [Senden von komplexen Typen](#sending_complex_types)
- [Senden von Daten mittels AJAX](#sending_form_data_via_ajax)
- [Senden von einfachen Typen](#sending_simple_types)

> [!NOTE]
> [Herunterladen des abgeschlossenen Projekts](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).


<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a>Übersicht über die HTML-Formularen

HTML-Formularen verwenden ist entweder GET oder POST zum Senden von Daten an den Server. Die **Methode** Attribut von der **Formular** Element gibt die HTTP-Methode:

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

Die Standardmethode ist GET. Wenn das Formular verwendet abzurufen Sie, das Formular, das Daten in den URI als Abfragezeichenfolge codiert ist. Wenn das Formular POST verwendet wird, werden die Formulardaten im Anforderungstext platziert. Für gebuchte Daten die **Enctype** Attribut gibt das Format des Anforderungstexts an:

| Enctype | Beschreibung |
| --- | --- |
| application/x-www-form-urlencoded | Als Name/Wert-Paare, ähnlich wie eine URI-Abfragezeichenfolge werden Formulardaten codiert. Dies ist das Standardformat für POST. |
| Multipart/Form-data | Formulardaten werden als eine mehrteilige MIME-Nachricht codiert. Verwenden Sie dieses Format, wenn Sie eine Datei mit dem Server hochladen. |

Teil 1 dieses Artikels untersucht die X-www-form-urlencoded Format. [Teil 2](sending-html-form-data-part-2.md) MIME multipart beschreibt.

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a>Senden von komplexen Typen

Senden Sie in der Regel einen komplexen Typ, der Werte, die mehrere Steuerelemente für Arbeitsaufgabenformulare entnommen besteht. Betrachten Sie das folgende Modell, das eine statusaktualisierung darstellt:

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

Hier ist ein Web-API-Controller, die akzeptiert ein `Update` Objekt über POST.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> Dieser Controller verwendet [aktionsbasierten routing](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), sodass die routenvorlage &quot;api / {Controller} / {Aktion} / {Id}&quot;. Der Client sendet die Daten &quot;/api/updates/complex&quot;.


Jetzt ein HTML-Formular für Benutzern das Senden einer statusaktualisierung schreiben.

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

Beachten Sie, dass die **Aktion** auf dem Formular Attributs ist der URI des unsere Controlleraktion. Hier wird das Formular mit einigen in eingegebenen Werte:

![](sending-html-form-data-part-1/_static/image1.png)

Wenn der Benutzer auf Absenden klickt, sendet der Browser eine HTTP-Anforderung ähnlich der folgenden:

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

Beachten Sie, dass der Anforderungstext die Formulardaten, formatiert als Name/Wert-Paare enthält. Web-API konvertiert automatisch die Name/Wert-Paare in einer Instanz von der `Update` Klasse.

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a>Senden von Daten mittels AJAX

Wenn ein Benutzer ein Formular sendet, wird der Browser navigiert Weg von der aktuellen Seite und rendert den Text der Antwortnachricht. Das ist in Ordnung, wenn die Antwort auf eine HTML-Seite ist. Mit einer Web-API, der Antworttext ist jedoch in der Regel entweder leer oder strukturierte Daten, z. B. JSON enthält. In diesem Fall wird es sinnvoller sein, dass die Formulardaten, die mithilfe einer AJAX anzufordern, senden, damit die Seite die Antwort verarbeiten kann.

Der folgende Code zeigt, wie unter Verwendung von jQuery Formulardaten gesendet wird.

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

Die jQuery **übermitteln** Funktion ersetzt die formaktion durch eine neue Funktion. Dies überschreibt das Standardverhalten der Schaltfläche "Absenden". Die **Serialisieren** Funktion serialisiert die Formulardaten in Name/Wert-Paaren. Aufrufen, um die Daten des Formulars an den Server zu senden, `$.post()`.

Klicken Sie nach Abschluss die Anforderung der `.success()` oder `.error()` Handler dem Benutzer eine entsprechende Meldung angezeigt.

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a>Senden von einfachen Typen

In den vorherigen Abschnitten haben wir einen komplexen Typ, der Web-API mit einer Instanz von eine Modellklasse deserialisiert gesendet. Sie können auch einfache Typen, z. B. eine Zeichenfolge senden.

> [!NOTE]
> Berücksichtigen Sie vor dem Senden eines einfachen Typs, stattdessen den Wert in einem komplexen Typ umschließt. Dies bietet Ihnen die Vorteile der modellvalidierung auf Serverseite und erleichtert es, Ihr Modell ggf. zu erweitern.


Die grundlegenden Schritte zum Senden von eines einfachen Typs entsprechen, aber es gibt zwei feine Unterschiede. Zunächst im Controller muss, ergänzen Sie die Parameternamen mit dem **FromBody** Attribut.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

Standardmäßig versucht Web-API zum Abrufen von einfachen Typen vom Anforderungs-URI. Die **FromBody** -Attribut weist die Web-API zum Lesen des Werts aus dem Anforderungstext.

> [!NOTE]
> Web-API liest den Antworttext nur einen Parameter, der eine Aktion aus dem Anforderungstext stammen kann wird höchstens einmal zurückgegeben. Wenn Sie mehrere Werte aus dem Anforderungstext abrufen müssen, definieren Sie einen komplexen Typ.


Zweitens muss der Client zum Senden des Wert mit dem folgenden Format ein:

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

Insbesondere muss der Namensteil der Name/Wert-Paar für einen einfachen Typ leer sein. Nicht von allen Browsern unterstützt diese für HTML-Formulare, aber Sie erstellen dieses Format im Skript wie folgt:

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

Hier ist eine Beispiel-Form:

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

Und hier ist das Skript, um den Formularwert zu senden. Der einzige Unterschied aus dem vorherigen Skript ist das übergebene Argument der **post** Funktion.

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

Den gleichen Ansatz können Sie ein Array von einfachen Typen senden:

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Teil 2: Datei hochladen und Multipart / MIME-](sending-html-form-data-part-2.md)
