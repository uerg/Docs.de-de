---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
title: Grundlegendes zu ASP.NET AJAX-Webdienste | Microsoft Docs
author: scottcate
description: Webdienste sind ein wesentlicher Bestandteil von .NET Framework, die eine plattformübergreifende-Lösung zum Austauschen von Daten zwischen verteilten Systemen zu erstellen. Obwohl Web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/28/2008
ms.topic: article
ms.assetid: 3332d6e7-e2e1-4144-b805-e71d51e7e415
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
msc.type: authoredcontent
ms.openlocfilehash: 0b9f61f895fea1960ebd25780454b86d5c3ba1bb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889341"
---
<a name="understanding-aspnet-ajax-web-services"></a>Grundlegendes zu ASP.NET AJAX-Webdienste
====================
durch [Scott Kate](https://github.com/scottcate)

[PDF herunterladen](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial05_Web_Services_with_MS_Ajax_cs.pdf)

> Webdienste sind ein wesentlicher Bestandteil von .NET Framework, die eine plattformübergreifende-Lösung zum Austauschen von Daten zwischen verteilten Systemen zu erstellen. Obwohl Webdienste normalerweise verwendet werden, um unterschiedliche Betriebssysteme, Objektmodelle und Programmiersprachen zum Senden und Empfangen von Daten zu ermöglichen, können sie auch dynamisch Einfügen von Daten in einer ASP.NET AJAX-Seite oder Senden von Daten von einer Seite mit einem Back-End-System verwendet werden. All dies kann erfolgen, ohne zurückzugreifen, um Vorgänge postback.


## <a name="calling-web-services-with-aspnet-ajax"></a>Aufrufen von Webdiensten mit ASP.NET-AJAX

Dan Wahlin

Webdienste sind ein wesentlicher Bestandteil von .NET Framework, die eine plattformübergreifende-Lösung zum Austauschen von Daten zwischen verteilten Systemen zu erstellen. Obwohl Webdienste normalerweise verwendet werden, um unterschiedliche Betriebssysteme, Objektmodelle und Programmiersprachen zum Senden und Empfangen von Daten zu ermöglichen, können sie auch dynamisch Einfügen von Daten in einer ASP.NET AJAX-Seite oder Senden von Daten von einer Seite mit einem Back-End-System verwendet werden. All dies kann erfolgen, ohne zurückzugreifen, um Vorgänge postback.

UpdatePanel von ASP.NET AJAX-Steuerelement eine einfache Möglichkeit, AJAX bietet eine ASP.NET-Seite zu aktivieren, möglicherweise gibt es Zeiten, wenn Sie dynamisch auf die Daten auf dem Server zuzugreifen, ohne Verwendung eines UpdatePanels müssen. In diesem Artikel wird erläutert, wie dies ausgeführt, durch das Erstellen und Verwenden von Webdiensten in ASP.NET AJAX-Seiten angezeigt werden.

Dieser Artikel konzentriert sich auf Funktionen, die in die wichtigsten ASP.NET AJAX-Erweiterungen als auch ein Webdienst aktiviert-Steuerelement in der ASP.NET AJAX-Toolkit wird aufgerufen, die AutoCompleteExtender verfügbar. Zu den behandelten Themen gehören AJAX-aktivierten Webdiensten definieren, Erstellen von Proxys und Aufrufen von Webdiensten mit JavaScript. Sie sehen auch, wie Webdienstaufrufe direkt mit ASP.NET-Seitenmethoden hergestellt werden können.

## <a name="web-services-configuration"></a>Web Services-Konfiguration

Wenn ein neues Projekt für die Website mit Visual Studio 2008 erstellt wird, wurde die Datei "Web.config" diverse neue Mitglieder, die für Benutzer früherer Versionen von Visual Studio nicht vertraut sind. Einige dieser Änderungen zuordnen das Präfix "Asp" für ASP.NET AJAX-Steuerelemente, damit diese Seiten verwendet werden können, während andere erforderliche HttpHandlers und HttpModules definieren. Änderungen an den programmbeispiel 1 zeigt die `<httpHandlers>` Element in "Web.config", die Webdienstaufrufe betroffen sind. Die Standard-HttpHandler zum Verarbeiten von ASMX-Aufrufe verwendet entfernt und durch eine ScriptHandlerFactory-Klasse befindet sich in der Assembly System.Web.Extensions.dll ersetzt. System.Web.Extensions.dll enthält alle die Kernfunktionen von ASP.NET AJAX verwendet wird.

**Programmbeispiel 1. ASP.NET AJAX Webdienstkonfiguration Handler**

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample1.xml)]

Diese Ersetzung HttpHandler wird hergestellt, damit JavaScript Object Notation (JSON) Aufrufe von ASP.NET AJAX-Seiten mit .NET Web Services erfolgen kann, mit einem JavaScript-Webdienst-Proxy. ASP.NET AJAX sendet JSON-Nachrichten mit Web Services, im Gegensatz zu den standard (SOAP, Simple Object Access Protocol)-Aufrufe, die in der Regel zugeordneten Webdienste. Dies führt zu kleineren Anforderungs- und Antwortnachrichten, die insgesamt. Darüber hinaus können für eine effizientere Verarbeitung von clientseitigen Daten seit die ASP.NET AJAX JavaScript-Bibliothek zum Arbeiten mit JSON-Objekten optimiert ist. Auflisten von 2 und 3 auflisten zeigen Beispiele für Web Service-Anforderung und Antwort-Nachrichten in JSON-Format serialisiert. Die Anforderungsnachricht in auflisten 2 veranschaulichte übergibt einen Country-Parameter mit einem Wert von "Belgien" aus, während die Antwortnachricht in 3 auflisten ein Array von Customer-Objekten und die zugehörigen Eigenschaften übergeben.

**Auflisten von 2. Webdienst-Anforderungsnachricht in JSON serialisiert**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample2.json)]

> *> [!NOTE] der Name des Vorgangs wird als Teil der URL zum Webdienst definiert. Darüber hinaus sind Anforderungsnachrichten nicht immer über JSON übermittelt. Web Services können ScriptMethod-Attributs nutzen, mit dem UseHttpGet-Parameter auf True festgelegt, wodurch über zu übergebenden Parameter an eine die Abfragezeichenfolgen-Parameter.*


**Auflisten von 3. Webdienst-Antwortnachricht in JSON serialisiert**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample3.json)]

Im nächsten Abschnitt sehen Sie, wie Webdienste der Behandlung von JSON-Anforderungsnachrichten und antwortet mit einfachen und komplexen Typen kann erstellt werden.

## <a name="creating-ajax-enabled-web-services"></a>Erstellen von AJAX-aktivierten Webdiensten

Das ASP.NET AJAX-Framework bietet mehrere Möglichkeiten zum Aufrufen von Webdiensten. Sie können die AutoCompleteExtender-Steuerelement (verfügbar in der ASP.NET AJAX-Toolkit) oder für JavaScript verwenden. Sie müssen jedoch vor dem Aufrufen eines Diensts AJAX-fähigen es so, dass er von Client-Skriptcode aufgerufen werden kann.

Unabhängig davon, ob Sie mit ASP.NET Web Services vertraut sind, finden Sie es einfach zu erstellen und aktivieren Sie AJAX-Dienste. .NET Framework unterstützt die Erstellung von ASP.NET-Webdiensten seit der Erstveröffentlichung 2002 und ASP.NET AJAX-Erweiterungen bieten zusätzliche AJAX-Funktionen, die auf .NET Framework Reihe von Funktionen aufbaut. Visual Studio .NET 2008 Beta 2 verfügt über integrierte Unterstützung für ASMX-Webdienst-Dateien zu erstellen und automatisch die zugehörigen Code-beside-Klassen von der System.Web.Services.WebService-Klasse abgeleitet ist. Hinzufügen von Methoden in der Klasse müssen Sie das WebMethod-Attribut in der Reihenfolge für die sie durch den Webdienst Consumer aufgerufen werden anwenden.

Auflisten von 4 zeigt ein Beispiel für das WebMethod-Attribut an eine Methode mit dem Namen GetCustomersByCountry() anwenden.

**Auflisten von 4. Verwenden das WebMethod-Attribut in einem Webdienst**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample4.cs)]

Die Methode GetCustomersByCountry() einen Land-Parameter akzeptiert und gibt ein Kunde Objektarray zurück. Der Wert für das Land an die Methode übergebenen ist weitergeleitet, auf eine Business-Ebenen-Klasse die wiederum ruft eine Ebene Datenklasse zum Abrufen der Daten aus der Datenbank füllen mit Daten, die Kunden-Objekteigenschaften und gibt das Array zurück.

## <a name="using-the-scriptservice-attribute"></a>Mithilfe des Attributs ScriptService

Beim Hinzufügen der WebMethod-Attribut ermöglicht, die GetCustomersByCountry()-Methode dass, die von Clients aufgerufen werden, die standard-SOAP-Nachrichten an den Webdienst zu senden, lässt es nicht JSON-Aufrufe an von ASP.NET AJAX-Anwendungen gebrauchsfertigen erfolgen. Um JSON-Aufrufe auf Sie anwenden, die ASP.NET AJAX-Erweiterung erfolgen ermöglichen `ScriptService` -Attribut auf die Webdienst-Klasse. Dies ermöglicht eine Web-Dienst zum Senden von Antwortnachrichten, die mit JSON formatiert und clientseitigem Skript zum Aufrufen des Diensts durch Senden von JSON-Nachrichten.

Auflisten von 5 zeigt ein Beispiel für das ScriptService-Attribut auf einen Webdienst-Klasse, die mit dem Namen CustomersService angewendet.

**Auflisten von 5. Mithilfe des Attributs ScriptService AJAX-Aktivieren eines Webdiensts**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample5.cs)]

Das Attribut ScriptService fungiert als ein Marker, der angibt, dass es von AJAX-Skript-Code aufgerufen werden kann. Es werden keine tatsächlich die JSON-Serialisierung oder Deserialisierung Aufgaben verarbeitet, die im Hintergrund auftreten. Führen den Großteil der JSON-Verarbeitung, die ScriptHandlerFactory (in "Web.config" konfiguriert) und anderen verwandten Klassen.

## <a name="using-the-scriptmethod-attribute"></a>Verwenden das ScriptMethod-Attribut

Das ScriptService-Attribut ist das einzige ASP.NET AJAX-Attribut, das in einem Webdienst .NET nacheinander dafür von ASP.NET AJAX-Seiten zu verwendende definiert werden. Ein weiteres Attribut mit dem Namen ScriptMethod kann jedoch auch direkt für Webmethoden in einem Dienst angewendet werden. ScriptMethod definiert drei Eigenschaften einschließlich `UseHttpGet`, `ResponseFormat` und `XmlSerializeString`. Ändern der Werte dieser Eigenschaften kann nützlich sein, in Fällen, in denen der Typ der Anforderung akzeptiert werden, indem eine Webmethode in "GET", geändert werden, wenn eine Webmethode in Form von unformatierten XML-Daten zurückgeben muss ein `XmlDocument` oder `XmlElement` Objekt oder bei Rückgabe von Daten aus einer  Dienst sollte immer als XML-Objekt statt JSON serialisiert werden.

Die Eigenschaft kann verwendet werden, wenn eine Webmethode annehmen soll UseHttpGet GET-Anforderungen im Gegensatz zur POST-Anforderungen. Anforderungen werden gesendet, dass die Eingabeparameter der Webmethode eine URL mit Abfragezeichenfolgen-Parameter konvertiert. Die UseHttpGet-Eigenschaft standardmäßig auf "false" und sollte nur festgelegt werden, um `true` bei Operationen sind bekanntermaßen Safe und wenn vertrauliche Daten nicht an einen Webdienst übergeben wird. Auflisten von 6 zeigt ein Beispiel mit dem ScriptMethod-Attribut mit der UseHttpGet-Eigenschaft.

**Auflisten von 6. Verwenden die ScriptMethod-Attributs, mit der UseHttpGet-Eigenschaft.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample6.cs)]

Ein Beispiel für die Header gesendet, wenn in 6 auflisten dargestellten HttpGetEcho Webmethode aufgerufen wird werden weiter angezeigt:

`GET /CustomerViewer/DemoService.asmx/HttpGetEcho?input=%22Input Value%22 HTTP/1.1`

Zusätzlich zum Zulassen von Webmethoden, HTTP-GET-Anforderungen zu akzeptieren, kann auch ScriptMethod-Attribut verwendet werden, wenn XML-Antworten aus einem Dienst anstelle von JSON zurückgegeben werden müssen. Beispielsweise kann einen Webdienst abrufen ein RSS-Feeds von einem Remotestandort aus und gibt es als ein XmlDocument oder XmlElement-Objekt. Verarbeiten der XML-können Daten dann auf dem Client auftreten.

Auflisten von 7 zeigt ein Beispiel Antwortformat-Eigenschaft verwenden, um anzugeben, dass die XML-Daten von einer Web-Methode zurückgegeben werden soll.

**Auflisten von 7. Verwenden die ScriptMethod-Attributs, mit der Eigenschaft Antwortformat.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample7.cs)]

Das Antwortformat-Eigenschaft kann auch zusammen mit der XmlSerializeString-Eigenschaft verwendet werden. Die XmlSerializeString-Eigenschaft hat den Standardwert "false", was bedeutet, dass alle zurückgeben, mit Ausnahme von Zeichenfolgen, die von einer Web-Methode zurückgegebenen Typen als XML serialisiert werden bei der `ResponseFormat` -Eigenschaftensatz auf `ResponseFormat.Xml`. Wenn `XmlSerializeString` festgelegt ist, um `true`, alle Typen, die von einer Web-Methode zurückgegeben werden als Zeichenfolgentypen einschließlich XML serialisiert. Wenn das Antwortformat-Eigenschaft einen Wert aufweist `ResponseFormat.Json` XmlSerializeString-Eigenschaft wird ignoriert.

Auflisten von 8 wird gezeigt, wie die Eigenschaft XmlSerializeString gezwungen Zeichenfolgen als XML serialisiert werden soll.

**Auflisten von 8. Verwenden das ScriptMethod-Attribut mit XmlSerializeString-Eigenschaft**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample8.cs)]

Der Rückgabewert des Aufrufs der Webmethode GetXmlString Siehe auflisten von 8 wird weiter angezeigt:

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample9.cs)]

Obwohl das JSON-Standardformat die Gesamtgröße des Anforderungs-und Antwortnachrichten minimiert und schneller durch ASP.NET AJAX-Clients in einer Weise browserübergreifende belegt, kann das Antwortformat und XmlSerializeString Eigenschaften verwendet, wenn Client Anwendungen, z. B. Internet Explorer 5 oder höher erwarten, dass XML-Daten aus einer Webmethode zurückgegeben werden.

## <a name="working-with-complex-types"></a>Arbeiten mit komplexen Typen

Auflisten von 5 angezeigt werden. ein Beispiel für einen komplexen Typ mit dem Namen Customer, die von einem Webdienst zurückgegeben. Die Customer-Klasse definiert verschiedene einfache Typen intern als Eigenschaften, z. B. FirstName und LastName. Komplexe Typen als Eingabeparameter verwendet, oder Rückgabetyp auf eine Webmethode AJAX-aktivierten vor dem Senden an die Clientseite automatisch in JSON serialisiert werden. Allerdings sind geschachtelte komplexe Typen (die intern in einem anderen Typ definiert) nicht an dem Client als eigenständige Objekte standardmäßig verfügbar.

In Fällen, in ein geschachtelter komplexer Typ, der von einem Webdienst verwendet auch in einer Registerkarte "Client" verwendet werden muss, kann das ASP.NET AJAX-GenerateScriptType-Attribut mit dem Webdienst hinzugefügt werden. Die CustomerDetails-Klasse, die im Codebeispiel 9 gezeigt enthält z. B. Adresse und Gender-Eigenschaften die ***geschachtelte komplexe Typen darstellen.***

**Auflisten von 9. Die hier gezeigte CustomerDetails-Klasse enthält zwei geschachtelte komplexe Typen.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample10.cs)]

Die Adresse und Gender-Objekte, die innerhalb der CustomerDetails-Klasse, die im Codebeispiel 9 gezeigt definiert wird nicht automatisch zur Verfügung gestellt werden für die Verwendung auf der Clientseite über JavaScript sie geschachtelte Typen sind (Adresse ist eine Klasse und Geschlecht ist eine Enumeration). In Situationen, in denen ein geschachtelter Typ, der innerhalb eines Webdiensts muss auf der Clientseite verfügbar, die zuvor erwähnte GenerateScriptType-Attribut verwendet werden kann (Siehe auflisten von 10). Dieses Attribut kann mehrere Male in Fällen hinzugefügt werden, in denen unterschiedliche geschachtelte komplexe Typen von einem Dienst zurückgegeben werden. Sie können auf die Webdienst-Klasse direkt oder über bestimmte Webmethoden angewendet werden.

**Auflisten von 10. Verwenden die GenerateScriptService-Attributs, um geschachtelte Typen definieren, die an den Client verfügbar sein sollen.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample11.cs)]

Durch Anwenden der `GenerateScriptType` Attribut, um den Webdienst, die Adresse und das Geschlecht Typen wird automatisch zur Verfügung gestellt werden für die Verwendung durch die clientseitige ASP.NET AJAX JavaScript-Code. Auflisten von 11 ist ein Beispiel für den JavaScript-Code, die automatisch generiert und an den Client gesendet werden, durch Hinzufügen des GenerateScriptType-Attributs auf einen Webdienst angezeigt. Sie sehen, wie Sie geschachtelte komplexe Typen später in diesem Artikel.

**Auflisten von 11. Geschachtelte komplexe Typen, die zu einer ASP.NET AJAX-Seite zur Verfügung gestellt.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample12.cs)]

Nun, dass Sie zum Erstellen von Webdiensten und richten sie den Zugriff auf ASP.NET AJAX-Seiten gesehen haben, werfen wir einen Blick auf das Erstellen und Verwenden von JavaScript-Proxys, damit Daten abgerufen oder an Webdienste gesendet werden können.

## <a name="creating-javascript-proxies"></a>Erstellen von JavaScript-Proxys

Aufrufen von einer standardmäßigen Webdienst (.NET oder eine andere Plattform) in der Regel umfasst das Erstellen ein Proxyobjekt, das Sie von der Komplexität des Sendens von SOAP-Anforderung und Antwort-Nachrichten schützt. Mit ASP.NET AJAX-Webdienst aufrufen können JavaScript-Proxys werden erstellt und problemlos Dienste aufrufen, ohne befürchten serialisieren und Deserialisieren von JSON-Nachrichten verwendet. JavaScript-Proxys können automatisch generiert werden, mithilfe von ASP.NET AJAX ScriptManager-Steuerelement.

Erstellen einen JavaScript-Proxy, der Webdienste aufrufen kann erfolgt mithilfe der ScriptManager-Services-Eigenschaft. Diese Eigenschaft ermöglicht Ihnen, einen oder mehrere Dienste zu definieren, die eine ASP.NET AJAX-Seite zum Senden oder Empfangen von Daten ohne postback Vorgänge asynchron aufrufen können. Definieren Sie einen Dienst mithilfe von ASP.NET AJAX `ServiceReference` Steuerelement und weisen die Webdienst-URL an des Steuerelements `Path` Eigenschaft. Auflisten von 12 zeigt ein Beispiel für einen Dienst namens CustomersService.asmx verweisen.

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample13.aspx)]

**Auflisten von 12. Definieren einen Webdienst in einer ASP.NET AJAX-Seite verwendet.**

Hinzufügen eines Verweises auf die CustomersService.asmx über das ScriptManager-Steuerelement bewirkt, dass einen JavaScript-Proxy auf dynamisch generierte und von der Seite verwiesen werden. Der Proxy mit eingebettet ist die &lt;Skript&gt; kennzeichnen und dynamisch geladen wird, in der Datei CustomersService.asmx aufrufen und dem Anhängen js bis zum Ende des Zertifikats. Das folgende Beispiel zeigt, wie der JavaScript-Proxy auf der Seite eingebettet ist, beim Debuggen in "Web.config" muss deaktiviert sein:

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample14.html)]

> *> [!NOTE] Wenn Sie möchten, um den eigentlichen Code der JavaScript-Proxy anzuzeigen, die generiert wird, Sie geben die an die gewünschte .NET Webdienst-URL in Internet Explorer-Adressfeld und js es am Ende anfügen können.*


Wenn debugging in "Web.config" aktiviert ist, die auf der Seite als eine Debugversion des JavaScript-Proxys eingebettet werden gezeigt weiter:

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample15.html)]

Der JavaScript-Proxy erstellt, indem die ScriptManager können auch direkt in die Seite eingebettet werden statt einen Verweis auf die &lt;Skript&gt; Tag Src-Attribut. Dies kann durch Festlegen des ServiceReference-Steuerelements InlineScript-Eigenschaft auf "true" (der Standardwert ist "false") erfolgen. Dies kann nützlich sein, wenn ein Proxy für den gemeinsamen ist nicht über mehrere Seiten, und wenn Sie die Anzahl der Netzwerkaufrufe an den Server zu reduzieren möchten. Wenn InlineScript festgelegt ist, auf die Proxy-Skript "true" wird nicht vom Browser zwischengespeichert werden, sodass der Wert von "false" in Fällen empfohlen wird, in dem der Proxy von mehreren Seiten in einer ASP.NET AJAX-Anwendung verwendet wird. Ein Beispiel für die Verwendung der InlineScript-Eigenschaft wird weiter angezeigt:

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample16.aspx)]

## <a name="using-javascript-proxies"></a>Verwenden von JavaScript-Proxys

Sobald eine ASP.NET AJAX-Seite, die über das ScriptManager-Steuerelement einen Webdienst verweist, ein Aufruf an den Webdienst vorgenommen werden kann und die zurückgegebenen Daten mithilfe von Rückruffunktionen verarbeitet werden können. Ein Webdienst wird von Verweisen auf den Namespace (falls vorhanden), Klassennamen und Name der Webmethode aufgerufen. Alle Parameter, die an den Webdienst übergeben können zusammen mit einer Rückruffunktion definiert werden, die die zurückgegebenen Daten behandelt.

Auflisten von 13 wird gezeigt, wie einen JavaScript-Proxy zum Aufrufen einer Web-Methode, die mit dem Namen GetCustomersByCountry() angezeigt. Die GetCustomersByCountry()-Funktion wird aufgerufen, wenn ein Endbenutzer eine Schaltfläche auf der Seite klickt.

**Auflisten von 13. Aufruf eines Webdiensts mit einem JavaScript-Proxy.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample17.js)]

Dieser Aufruf verweist auf den Namespace InterfaceTraining, CustomersService Klassen- und GetCustomersByCountry Web im Dienst definiert. Übergibt einen Land-Wert, der aus einem Textfeld als auch eine Rückruffunktion, die mit dem Namen OnWSRequestComplete, die aufgerufen werden soll, wenn der asynchrone Aufruf für den Webdienst zurückgibt. OnWSRequestComplete verarbeitet das Array von Customer-Objekten, die vom Dienst zurückgegebenen und wandelt sie in eine Tabelle, die auf der Seite angezeigt wird. Die aus dem Aufruf generierte Ausgabe wird in Abbildung 1 dargestellt.


[![Binden von Daten erhalten, indem ein asynchroner AJAX-Aufruf an einen Webdienst herstellen.](understanding-asp-net-ajax-web-services/_static/image2.png)](understanding-asp-net-ajax-web-services/_static/image1.png)

**Abbildung 1**: Binden von Daten erhalten, indem ein asynchroner AJAX-Aufruf an einen Webdienst herstellen.  ([Klicken Sie hier, um das Bild in voller Größe angezeigt](understanding-asp-net-ajax-web-services/_static/image3.png))


JavaScript-Proxys lassen sich auch unidirektionale Aufrufe an die Webdienste in Fällen, in denen eine Webmethode aufgerufen werden, jedoch der Proxy darf nicht auf eine Antwort warten. Beispielsweise empfiehlt es sich zum Aufrufen eines Webdiensts zum Starten eines Prozesses, z. B. ein Ablauf, aber nicht nach dem Rückgabewert aus dem Dienst warten. In Fällen, in denen ein unidirektionaler Aufruf an einen Dienst vorgenommen werden muss, kann die Rückruffunktion auflisten 13 gezeigt einfach ausgelassen werden. Da keine Rückruffunktion definiert ist wartet das Proxyobjekt nicht für den Webdienst, um Daten zurückzugeben.

## <a name="handling-errors"></a>Behandeln von Fehlern

Asynchrone Rückrufe mit Web Services können auftreten, verschiedene Arten von Fehlern, z. B. das Netzwerk nicht verfügbar, die Web-Dienst nicht verfügbar ist oder eine Ausnahme zurückgegeben wird. Glücklicherweise können JavaScript-Proxy-Objekte von ScriptManager generiert mehrere Rückrufe definiert werden, um Fehler und Ausfälle neben den weiter oben dargestellten Erfolgsrückruf zu behandeln. Eine Fehlerrückruf-Funktion kann unmittelbar auf die standard-Rückruffunktion im Aufruf der Webmethode definiert werden, wie im Codebeispiel 14 gezeigt.

**Auflisten von 14. Definieren eine Rückruffunktion für Fehler, und Anzeigen von Fehlern.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample18.js)]

Alle Fehler, die auftreten, wenn der Webdienst aufgerufen wird werden ausgelöst, die Rückruffunktion OnWSRequestFailed() zum Aufruf der akzeptiert, dass ein Objekt, das den Fehler als Parameter darstellt. Die Error-Objekt stellt mehrere unterschiedliche Funktionen, um die Ursache des Fehlers zu bestimmen, sowie davon, ob für das Timeout des Aufrufs. Auflisten von 14 zeigt ein Beispiel der Verwendung der verschiedenen Fehlerfunktionen und Abbildung 2 zeigt ein Beispiel für die Ausgabe von den Funktionen generiert.


[![Durch Aufrufen von ASP.NET AJAX-Fehlerfunktionen generierte Ausgabe.](understanding-asp-net-ajax-web-services/_static/image5.png)](understanding-asp-net-ajax-web-services/_static/image4.png)

**Abbildung 2**: Ausgabe, die durch Aufrufen von ASP.NET AJAX-Fehlerfunktionen generiert.  ([Klicken Sie hier, um das Bild in voller Größe angezeigt](understanding-asp-net-ajax-web-services/_static/image6.png))


## <a name="handling-xml-data-returned-from-a-web-service"></a>Verarbeiten von XML-Daten von einem Webdienst zurückgegeben

Zuvor haben Sie gesehen, wie eine Webmethode unformatierte XML-Daten zurückgeben kann, mit dem Attribut "ScriptMethod" zusammen mit seiner Antwortformat-Eigenschaft. Wenn Antwortformat auf ResponseFormat.Xml festgelegt ist, werden vom Webdienst zurückgegebene Daten als XML und nicht in JSON serialisiert. Dies kann nützlich sein, wenn XML-Daten direkt an den Client für die mit JavaScript oder XSLT-Verarbeitung übergeben werden müssen. Zum aktuellen Zeitpunkt enthält InternetExplorer 5 oder höher das bewährte clientseitige Objektmodell für das Analysieren und Filtern von XML-Daten aufgrund der integrierten Unterstützung für MSXML.

Abrufen von XML-Daten von einem Webdienst unterscheidet sich nicht als andere Datentypen abrufen. Starten Sie durch den Aufruf des JavaScript-Proxys, um die entsprechende Funktion aufrufen und eine Rückruffunktion zu definieren. Nach der Rückkehr des Aufrufs können Sie dann die Daten in der Rückruffunktion verarbeiten.

Auflisten von 15 zeigt ein Beispiel des Aufrufs einer Webmethode mit dem Namen GetRssFeed(), die ein XmlElement-Objekt zurückgibt. GetRssFeed() akzeptiert einen einzelnen Parameter, die die URL für den RSS-feed zum Abrufen von darstellen.

**Auflisten von 15. Arbeiten mit XML-Daten von einem Webdienst zurückgegeben.**

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample19.html)]

In diesem Beispiel wird eine URL an einen RSS-feed übergeben und die zurückgegebenen XML-Daten in der OnWSRequestComplete()-Funktion verarbeitet. OnWSRequestComplete() überprüft zuerst, um festzustellen, ob der Browser Internet Explorer ist kennen, und zwar unabhängig davon, ob die MSXML-Parser verfügbar ist. Wenn dies der Fall, wird eine XPath-Anweisung verwendet, alle suchen &lt;Element&gt; Tags innerhalb des RSS-Feeds. Jedes Element wird dann durchlaufen und den zugehörigen &lt;Titel&gt; und &lt;Link&gt; -Tags gespeichert und verarbeitet werden, um Daten für jedes Element angezeigt. Abbildung 3 zeigt ein Beispiel für die Ausgabe generiert gehindert ein ASP.NET AJAX über eine JavaScript-Proxy der Webmethode GetRssFeed() aufrufen.

## <a name="handling-complex-types"></a>Behandlung von komplexen Typen

Komplexe Typen akzeptiert oder von einem Webdienst zurückgegeben werden automatisch über eine JavaScript-Proxy verfügbar gemacht. Geschachtelte komplexe Typen sind jedoch nicht direkt auf der Clientseite zugegriffen werden, es sei denn, das Attribut GenerateScriptType an den Dienst angewendet wird, wie bereits erwähnt. Warum sollten Sie einen geschachtelten komplexen Typ auf der Clientseite verwenden?

Um diese Frage zu beantworten, wird davon ausgegangen Sie, dass eine ASP.NET AJAX-Seite zeigt der Kundendaten und Benutzern ermöglicht, die Adresse eines Kunden zu aktualisieren. Wenn der Webdienst gibt an, dass der Adresstyp (einem komplexen Typ innerhalb einer Klasse CustomerDetails definiert) an den Client gesendet werden kann, kann der Updatevorgang dahingehend in separate Funktionen für besseren Code erneut verwenden unterteilt werden.


[![Ausgabe erstellen aus den Aufruf eines Webdiensts, die RSS-Daten zurückgibt.](understanding-asp-net-ajax-web-services/_static/image8.png)](understanding-asp-net-ajax-web-services/_static/image7.png)

**Abbildung 3**: Ausgabe erstellen aus den Aufruf eines Webdiensts, die RSS-Daten zurückgibt.  ([Klicken Sie hier, um das Bild in voller Größe angezeigt](understanding-asp-net-ajax-web-services/_static/image9.png))


Auflisten von 16 zeigt ein Beispiel für Client-Side-Code, der ein Adressobjekt in einem Modellnamespace definierten aufruft füllt diese mit aktualisierten Daten und die Adresseigenschaft des CustomerDetails-Objekts zugewiesen. Die CustomerDetails-Objekt wird dann zur Verarbeitung an den Webdienst übergeben.

**Auflisten von 16. Verwenden geschachtelte komplexe Typen**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample20.js)]

## <a name="creating-and-using-page-methods"></a>Erstellen und Verwenden von Seitenmethoden

Webdienste bieten eine hervorragende Möglichkeit, die mit einer Vielzahl von Clients einschließlich ASP.NET AJAX-Seiten aktionsentwicklung Dienste verfügbar zu machen. Allerdings gibt es möglicherweise Fällen, in denen eine Seite zum Abrufen von Daten, die nicht einmal verwendet oder von anderen Seiten gemeinsam verwendet werden. In diesem Fall mag erstellen eine ASMX-Datei, um die Seite für den Datenzugriff zu ermöglichen Ausgangsstruktur wie seit der Dienst nur von einer einzelnen Seite verwendet wird.

ASP.NET AJAX bietet es sich um einen anderen Mechanismus zum Erstellen von Web Service-ähnliche Aufrufe ohne eigenständige ASMX-Dateien erstellen. Dies erfolgt mithilfe eines Verfahrens als "Seitenmethoden" bezeichnet. Seitenmethoden werden statische (freigegebene in VB.NET) direkt in einer Seite oder Code-beside-Datei eingebettet, die das WebMethod-angewendet werden Attribut. Durch Anwenden der WebMethod-Attribut können sie aufgerufen werden mithilfe eines speziellen JavaScript-Objekts, das mit dem Namen PageMethods, die zur Laufzeit dynamisch erstellt wird. Das Objekt PageMethods fungiert als Proxy, der Sie im Rahmen JSON-Serialisierung/Deserialisierung schützt. Beachten Sie, dass um verwenden jedoch stattdessen das PageMethods der ScriptManager EnablePageMethods-Eigenschaft auf "true" festgelegt werden muss.

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample21.aspx)]

Auflisten von 17 zeigt ein Beispiel der Definition von zwei Seitenmethoden in einer ASP.NET Code-beside-Klasse. Diese Methoden rufen Sie Daten aus einer Business-Ebene-Klasse befindet sich in der App\_Codeordner der Website.

**Auflisten von 17. Seitenmethoden definieren.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample22.cs)]

Wenn ScriptManager auf der Seite das Vorhandensein von Webmethoden erkennt generiert einen dynamischen Verweis auf den zuvor erwähnten PageMethods-Objekt. Aufrufen einer Web-Methode erfolgt durch Verweisen auf die PageMethods-Klasse, gefolgt vom Namen der Methode und alle erforderlichen Parameterdaten, die übergeben werden sollen. Auflisten von 18 zeigt Beispiele für die beiden weiter oben dargestellten Seitenmethoden aufrufen.

**Auflisten von 18. Aufrufen von Seitenmethoden mit dem PageMethods JavaScript-Objekt.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample23.js)]

Mit dem PageMethods-Objekt ist sehr ähnlich, mit der ein JavaScript-Proxy-Objekt. Sie geben zuerst alle der Parameterdaten, die auf der Seitenmethode übergeben werden sollte, und definieren Sie dann die Rückruffunktion, die aufgerufen werden soll, wenn der asynchrone Aufruf zurückgegeben. Fehler beim Rückruf kann auch angegeben werden (Siehe auflisten von 14 für ein Beispiel für die Behandlung von Fehlern).

## <a name="the-autocompleteextender-and-the-aspnet-ajax-toolkit"></a>Die AutoCompleteExtender und das ASP.NET AJAX-Toolkit

Das ASP.NET AJAX-Toolkit (Nächten [ http://ajax.asp.net ](http://ajax.asp.net)) bietet mehrere Steuerelemente, die zum Zugriff auf Web-Dienste verwendet werden können. Insbesondere das Toolkit enthält ein nützlich-Steuerelement namens `AutoCompleteExtender` , die zum Aufrufen von Webdiensten und Anzeigen von Daten in Seiten ohne überhaupt Schreiben von JavaScript-Code verwendet werden kann.

Das Steuerelement AutoCompleteExtender kann verwendet werden, eines Textfelds vorhandene Funktionalität erweitern und unterstützen von Benutzern, die weitere Daten einfach zu suchen, die sie suchen. Wie sie in ein Textfeld eingeben, wird das Steuerelement kann verwendet werden, um einen Webdienst abzufragen und zeigt die Ergebnisse unterhalb des Textfelds, dynamisch. Abbildung 4 zeigt ein Beispiel mithilfe des Steuerelements AutoCompleteExtender Kunden-Ids für eine Anwendung Unterstützung anzuzeigen. Wie der Benutzer in das Textfeld ein andere Zeichen eingibt, werden verschiedene Elemente darunter auf Grundlage der Eingabe angezeigt. Benutzer können dann die gewünschten Kunden-Id auswählen.

Mithilfe der AutoCompleteExtender innerhalb einer ASP.NET AJAX-Seite erfordert, dass die Assembly AjaxControlToolkit.dll Website Ordner "Bin" hinzugefügt werden. Sobald die Toolkit-Assembly hinzugefügt wurde, sollten Sie in "Web.config" zu verweisen, sodass die darin enthaltenen Steuerelemente für alle Seiten in einer Anwendung zur Verfügung stehen. Dies kann geschehen, indem die folgenden Tag in "Web.config" des hinzufügen &lt;Steuerelemente&gt; Tag:

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample24.xml)]

In Fällen, in denen müssen Sie nur das Steuerelement in einer bestimmten Seite verwenden, können Sie darauf verweisen, durch die Reference-Direktive am Anfang einer Seite hinzufügen, als Nächstes, anstatt die Datei "Web.config" Aktualisieren dargestellt:

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample25.aspx)]


[![Verwenden des AutoCompleteExtender-Steuerelements an.](understanding-asp-net-ajax-web-services/_static/image11.png)](understanding-asp-net-ajax-web-services/_static/image10.png)

**Abbildung 4**: Verwenden des AutoCompleteExtender-Steuerelements.  ([Klicken Sie hier, um das Bild in voller Größe angezeigt](understanding-asp-net-ajax-web-services/_static/image12.png))


Nach die Website für die Verwendung von ASP.NET AJAX-Toolkit konfiguriert wurde, kann ein AutoCompleteExtender-Steuerelement in die Seite viel hinzugefügt, wie Sie ein reguläres ASP.NET-Serversteuerelement hinzufügen würden. Auflisten von 19 zeigt ein Beispiel der Verwendung des Steuerelements an einen Webdienst aufzurufen.

**Auflisten von 19. Verwenden des ASP.NET AJAX-Toolkit AutoCompleteExtender-Steuerelements an.**

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample26.aspx)]

Die AutoCompleteExtender verfügt über mehrere verschiedene Eigenschaften sowie die standard-ID und das Runat-Eigenschaften auf Serversteuerelemente gefunden. Außerdem können Sie definieren, wie viele Zeichen, die ein Endbenutzer-Typen, bevor der Webdienst für Daten abgefragt wird. Auflisten von 19 gezeigt MinimumPrefixLength-Eigenschaft führt dazu, dass den Dienst aufgerufen werden, wenn ein Zeichen in das Textfeld eingegeben wurde. Sie sollten darauf achten, dass das Festlegen dieses Werts, da jedes Mal der Benutzer, ein Zeichen der Webdienst eingibt aufgerufen wird, um nach Werten gesucht, die mit den Zeichen im Textfeld übereinstimmen. Der Webdienst aufzurufende als auch das Ziel Webmethode werden bzw. mithilfe von ServicePath und ServiceMethod Eigenschaften definiert. Schließlich identifiziert die TargetControlID-Eigenschaft welche Textbox AutoCompleteExtender Steuerelements zu verknüpfen.

Der Webdienst aufgerufen werden muss das ScriptService-Attribut angewendet wird, wie bereits erwähnt und das Ziel Webmethode muss zwei Parameter, die mit dem Namen PrefixText und Count annehmen. Die PrefixText Parameter darstellt, die vom Benutzer eingegebenen Zeichen und der Count-Parameter darstellt, wie viele Elemente zum zurückgeben (der Standard ist 10). Auflisten von 20 zeigt ein Beispiel der Webmethode GetCustomerIDs vom Auflisten von 19 weiter oben gezeigten AutoCompleteExtender Steuerelement aufgerufen. Die Webmethode Ruft eine Business-Ebenen-Methode, die wiederum Aufrufe einer Methode Schicht, die verarbeitet werden, die Daten filtern und die übereinstimmenden Ergebnisse zurückgeben. Der Code für die Ebene Datenmethode ist 21 auflisten gezeigt.

**Auflisten von 20. Filtern von Daten aus dem Steuerelement AutoCompleteExtender gesendet.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample27.cs)]

**Auflisten von 21. Filtern der Ergebnisse basierend auf Benutzereingaben Ende.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample28.cs)]

## <a name="conclusion"></a>Schlussbemerkung

ASP.NET AJAX bietet hervorragende Unterstützung für Webdienste aufrufen, ohne das Schreiben umfangreichen benutzerdefinierten JavaScript-Code zur Behandlung der Anforderung und Antwort-Nachrichten. In diesem Artikel haben Sie gesehen wie AJAX-fähigen .NET Web Services, um sie zum Verarbeiten von JSON-Nachrichten und zum Definieren von JavaScript-Proxys, die über das ScriptManager-Steuerelement zu aktivieren. Sie haben auch erfahren, wie die JavaScript-Proxys verwendet werden können, zum Aufrufen von Webdiensten, einfache und komplexe Typen verarbeiten, und Fehler behandeln. Schließlich haben Sie gesehen, wie Seitenmethoden verwendet werden können, um den Prozess des Erstellens und Aufrufe des Webdiensts zu vereinfachen und wie das Steuerelement AutoCompleteExtender Hilfe für Endbenutzer bereitstellen kann, wie sie geben. Zwar in ASP.NET AJAX verfügbar UpdatePanel sicherlich das Steuerelement Wahl für viele AJAX-Programmierer aufgrund seiner Einfachheit, kann das zu wissen, wie das Aufrufen von Webdiensten über JavaScript-Proxys in vielen Anwendungen nützlich sein.

## <a name="bio"></a>Lebenslauf

Dan Wahlin (Microsoft Most Valuable Professional für ASP.NET und XML-Webdienste) ist .NET Development Dozenten und Architektur Berater zur technischen Training-Schnittstelle ([http://www.interfacett.com](http://www.interfacett.com)). Dan gegründet XML für Entwickler ASP.NET-Website ([www.XMLforASP.NET](http://www.XMLforASP.NET)), befindet sich auf die INETA Referent und spricht auf mehrere Konferenzen. Dan Mitautor Professional Windows DNA (Wrox), ASP.NET: Tipps, Lernprogramme und Code (Sams), ASP.NET 1.1 Insider-Lösungen, Professional ASP.NET 2.0 AJAX (Wrox), ASP.NET 2.0 MVP Hacks und erstellte XML für ASP.NET-Entwickler, (Sams). Wenn er nicht Code "," Artikel "oder" Bücher geschrieben wird, hat Dan schreiben und Musik aufzeichnen und Wiedergeben von Golf und Basketball mit seiner Frau und Kinder.

Scott Cate arbeitet mit Microsoft-Web-Technologien seit 1997 und Präsidenten des myKB.com ist ([www.myKB.com](http://www.myKB.com)), in dem er zum Schreiben von ASP.NET spezialisiert-basierten Anwendungen, die Wissensdatenbank softwarelösungen konzentriert. Scott hergestellt werden kann, per e-Mail an [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) oder seinen Blog unter [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Zurück](understanding-asp-net-ajax-localization.md)
> [Weiter](understanding-asp-net-ajax-debugging-capabilities.md)
