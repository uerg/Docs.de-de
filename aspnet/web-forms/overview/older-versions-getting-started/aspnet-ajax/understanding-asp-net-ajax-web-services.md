---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
title: Grundlegendes zu ASP.NET AJAX-Webdienste | Microsoft-Dokumentation
author: scottcate
description: Webdienste sind ein integraler Bestandteil von .NET Framework, die eine plattformübergreifende Lösung für den Austausch von Daten zwischen verteilten Systemen bereitstellen. Obwohl Web...
ms.author: riande
ms.date: 03/28/2008
ms.assetid: 3332d6e7-e2e1-4144-b805-e71d51e7e415
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
msc.type: authoredcontent
ms.openlocfilehash: 5e59077373b68b907391eff5349e1925222792a3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838390"
---
<a name="understanding-aspnet-ajax-web-services"></a>Grundlegendes zu Webdiensten von ASP.NET AJAX
====================
durch [Scott Cate](https://github.com/scottcate)

[PDF herunterladen](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial05_Web_Services_with_MS_Ajax_cs.pdf)

> Webdienste sind ein integraler Bestandteil von .NET Framework, die eine plattformübergreifende Lösung für den Austausch von Daten zwischen verteilten Systemen bereitstellen. Obwohl Webdienste normalerweise verwendet werden, um verschiedene Betriebssysteme, Objektmodelle und Programmiersprachen, die zum Senden und Empfangen von Daten zu ermöglichen, können sie auch dynamisch Einfügen von Daten in einer ASP.NET AJAX-Seite, oder Senden von Daten aus einer Seite an ein Back-End-System verwendet werden. All dies kann erfolgen, ohne um postback-Vorgänge.


## <a name="calling-web-services-with-aspnet-ajax"></a>Aufrufen von Webdiensten mit ASP.NET-AJAX

Dan Wahlin

Webdienste sind ein integraler Bestandteil von .NET Framework, die eine plattformübergreifende Lösung für den Austausch von Daten zwischen verteilten Systemen bereitstellen. Obwohl Webdienste normalerweise verwendet werden, um verschiedene Betriebssysteme, Objektmodelle und Programmiersprachen, die zum Senden und Empfangen von Daten zu ermöglichen, können sie auch dynamisch Einfügen von Daten in einer ASP.NET AJAX-Seite, oder Senden von Daten aus einer Seite an ein Back-End-System verwendet werden. All dies kann erfolgen, ohne um postback-Vorgänge.

Das UpdatePanel von ASP.NET AJAX-Steuerelement eine einfache Möglichkeit zum AJAX bietet eine ASP.NET-Seite aktivieren, Sie müssen, manchmal, dynamisch auf die Daten auf dem Server zuzugreifen, ohne über ein UpdatePanel reichen möglicherweise. In diesem Artikel sehen Sie, wie Sie dies durch Erstellen und Verwenden von Webdiensten in ASP.NET AJAX-Seiten.

Dieser Artikel konzentriert sich auf Funktionen, die in die wichtigsten ASP.NET AJAX-Erweiterungen als auch für ein Steuerelement aktiviert, in dem ASP.NET AJAX-Toolkit wird aufgerufen, die AutoCompleteExtender verfügbar. Behandelten Themen gehören das Definieren von AJAX-aktivierten Webdiensten, Erstellung von Client-Proxys und Aufrufen von Webdiensten mit JavaScript. Sie werden auch sehen, wie Web-Dienst-Aufrufe direkt an ASP.NET-Seitenmethoden vorgenommen werden können.

## <a name="web-services-configuration"></a>Web Services-Konfiguration

Wenn ein neues Projekt für die Website mit Visual Studio 2008 erstellt wird, wurde die Datei "Web.config" eine Anzahl von Ergänzungen, die möglicherweise für Benutzer von früheren Versionen von Visual Studio nicht vertraut sind. Einige dieser Änderungen zuordnen, damit sie auf Seiten verwendet werden können, während andere erforderliche HttpHandlers und HttpModules definieren das Präfix "Asp" ASP.NET AJAX-Steuerelemente. Codebeispiel 1 zeigt Änderungen an der `<httpHandlers>` Element in "Web.config", die Aufrufe des Webdiensts betroffen sind. Die Standard-HttpHandler verwendet wird, um die ASMX-Anrufe verarbeiten entfernt und durch eine ScriptHandlerFactory-Klasse befindet sich in der Assembly System.Web.Extensions.dll ersetzt. System.Web.Extensions.dll enthält alle die Kernfunktionen, die von ASP.NET AJAX verwendet wird.

**Codebeispiel 1. ASP.NET AJAX-Handlerkonfiguration des Webdiensts**

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample1.xml)]

Diese Ersetzung HttpHandler wird vorgenommen, damit JavaScript Object Notation (JSON) Aufrufe von ASP.NET AJAX-Seiten vorgenommen werden, auf .NET Web Services zu ermöglichen, mit einem JavaScript-Webdienst-Proxy. ASP.NET AJAX sendet JSON-Nachrichten an Webdienste, im Gegensatz zu den standard (SOAP, Simple Object Access Protocol)-Aufrufe, die i. d. r. Webdienste zugeordnet. Dies führt zu kleineren Anforderungs- und Antwortnachrichten, die insgesamt. Außerdem ermöglicht eine effizientere Verarbeitung von clientseitigen Daten, da die ASP.NET AJAX JavaScript-Bibliothek zum Arbeiten mit JSON-Objekten optimiert ist. Auflisten von 2 und Codebeispiel 3 zeigen Beispiele für die Web Service-Anforderung und Antwort-Nachrichten in JSON-Format serialisiert. Die Anforderungsnachricht Programmausdruck 2 übergibt einen Land-Parameter mit einem Wert von "Belgien" aus, während die Antwortnachricht in Programmausdruck 3 ein Array von Customer-Objekten und deren zugeordnete Eigenschaften übergibt.

**Codebeispiel 2. Webdienst-Anforderungsnachricht in JSON serialisiert**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample2.json)]

> *> [!NOTE] der Name des Vorgangs wird als Teil der URL an den Webdienst definiert. Darüber hinaus werden Request-Meldungen nicht immer über JSON gesendet. Webdienste können das ScriptMethod-Attribut verwenden, mit dem UseHttpGet-Parameter auf True festgelegt, wodurch über übergeben von Parametern an eine die Abfragezeichenfolgen-Parameter.*


**Codebeispiel 3. Webdienst-Antwortnachricht in JSON serialisiert**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample3.json)]

Im nächsten Abschnitt sehen Sie, wie Sie Webdienste Verarbeiten von Nachrichten von JSON-Anforderung und antwortet mit einfachen und komplexen Typen kann erstellt werden.

## <a name="creating-ajax-enabled-web-services"></a>Erstellen von AJAX-aktivierten Webdiensten

ASP.NET AJAX-Framework bietet mehrere Möglichkeiten, Webdienste aufzurufen. Sie können das AutoCompleteExtender-Steuerelement (verfügbar in der ASP.NET AJAX-Toolkit) oder die JavaScript verwenden. Allerdings müssen vor dem Aufrufen eines Diensts eine AJAX-Aktivierung es, damit sie von Clientskripts Code aufgerufen werden kann.

Unabhängig davon, ob Sie mit ASP.NET Web Services vertraut sind, finden Sie es einfach zu erstellen und aktivieren Sie AJAX-Dienste. .NET Framework unterstützt die Erstellung von ASP.NET Web Services seit seiner Veröffentlichung im Jahr 2002 und den ASP.NET AJAX-Erweiterungen bieten zusätzliche AJAX-Funktionen, die auf .NET Framework Standardsatz von Features aufbaut. Visual Studio .NET 2008 Beta 2 verfügt über integrierte Unterstützung für das Erstellen von ASMX-Webdienst-Dateien und automatisch die zugehörigen Code-beside-Klassen von der System.Web.Services.WebService-Klasse abgeleitet ist. Hinzufügen von Methoden in der Klasse müssen Sie das WebMethod-Attribut in der Reihenfolge für sie von Webdienst-Consumern aufgerufen werden anwenden.

Programmausdruck 4 zeigt ein Beispiel für das WebMethod-Attribut auf eine Methode namens GetCustomersByCountry() angewendet.

**Programmausdruck 4. Verwenden das WebMethod-Attribut in einem Webdienst**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample4.cs)]

Die GetCustomersByCountry()-Methode akzeptiert einen Parameter für Land und gibt ein Kunde Objektarray. Der Wert für das Land an die Methode übergeben wird weitergeleitet, auf eine Business-Ebene-Klasse die wiederum Aufrufe eine Datenklasse-Ebene zum Abrufen der Daten aus der Datenbank, die Kunden-Objekteigenschaften mit Daten füllen, und gibt das Array zurück.

## <a name="using-the-scriptservice-attribute"></a>Verwenden das ScriptService-Attribut

Beim Hinzufügen der WebMethod-Attribut die GetCustomersByCountry()-Methode ermöglicht, die von Clients aufgerufen werden, die standard-SOAP-Nachrichten an den Webdienst zu senden, ist es JSON-Aufrufe von ASP.NET AJAX-Anwendungen standardmäßig erstellt werden, nicht möglich. Um JSON-müssen Sie die ASP.NET AJAX-Erweiterung anwenden, erfolgen Aufrufe zuzulassen `ScriptService` -Attribut auf die Webdienst-Klasse. Dies ermöglicht einen Webdienst zum Senden von Antwortnachrichten, die mithilfe von JSON formatiert und Client-seitige Skript Aufrufen eines Diensts, indem Sie JSON-Nachrichten senden.

Programmausdruck 5 zeigt ein Beispiel für das ScriptService-Attribut auf einen Webdienst-Klasse, die mit dem Namen CustomersService angewendet.

**Programmausdruck 5 an. Verwenden das ScriptService-Attribut eine AJAX-Aktivierung eines Webdiensts**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample5.cs)]

Das ScriptService-Attribut fungiert als ein Marker, der angibt, dass es von AJAX-Skript-Code aufgerufen werden kann. Es behandeln nicht tatsächlich die JSON-Serialisierung oder Deserialisierung Aufgaben, die hinter den Kulissen ablaufen. Führen die ScriptHandlerFactory (in "Web.config" konfiguriert) und anderen verwandten Klassen den größten Teil der JSON-Verarbeitung.

## <a name="using-the-scriptmethod-attribute"></a>Verwenden das ScriptMethod-Attribut

Das ScriptService-Attribut ist das einzige ASP.NET AJAX-Attribut, die in einem .NET Web-Dienst in der Reihenfolge für sie von ASP.NET AJAX-Seiten verwendet werden, definiert werden. Ein weiteres Attribut mit dem Namen ScriptMethod kann jedoch auch direkt in Webmethoden in einem Dienst angewendet werden. ScriptMethod definiert drei Eigenschaften, einschließlich `UseHttpGet`, `ResponseFormat` und `XmlSerializeString`. Ändern Sie die Werte dieser Eigenschaften kann nützlich sein, in Fällen, in denen der Typ der Anforderung, die von einer Web-Methode akzeptiert muss Get, geändert werden, wenn Sie eine Webmethode unformatierte XML-Daten in Form von zurückgeben muss, ein `XmlDocument` oder `XmlElement` Objekt oder die Rückgabe von Daten aus einem  Dienst sollte immer als XML anstelle von JSON serialisiert werden.

Die UseHttpGet, die Eigenschaft verwendet werden kann, wenn Sie eine Webmethode akzeptieren sollen GET-Anforderungen im Gegensatz zu POST-Anforderungen. Anforderungen werden gesendet, dass über eine URL mit dem Eingabeparameter der Webmethode QueryString-Parameter konvertiert. Die UseHttpGet, Eigenschaft ist standardmäßig auf "false" und sollte nur, festgelegt werden, um `true` wann Vorgänge sind bekanntermaßen sicher und wenn sensible Daten nicht an einen Webdienst übergeben wird. Codebeispiel 6 zeigt ein Beispiel für die UseHttpGet-Eigenschaft mit dem ScriptMethod-Attribut.

**Codebeispiel 6. Verwenden das ScriptMethod-Attribut, mit der UseHttpGet-Eigenschaft.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample6.cs)]

Ein Beispiel für die Header gesendet, wenn die in Codebeispiel 6 gezeigten HttpGetEcho-Web-Methode aufgerufen wird sind weiter dargestellt:

`GET /CustomerViewer/DemoService.asmx/HttpGetEcho?input=%22Input Value%22 HTTP/1.1`

Zusätzlich zum Zulassen von Webmethoden, HTTP GET-Anforderungen zu akzeptieren, kann das ScriptMethod-Attribut auch verwendet werden, wenn XML-Antworten von einem Dienst und nicht als JSON zurückgegeben werden müssen. Beispielsweise kann einen Webdienst abrufen ein RSS-Feeds von einem Remotestandort aus und geben sie als ein XmlDocument oder XmlElement-Objekt zurück. Der XML-Verarbeitung können Daten auf dem Client durchgeführt werden.

Codebeispiel 7 zeigt ein Beispiel für die ResponseFormat-Eigenschaft verwenden, um anzugeben, dass die XML-Daten aus einer Web-Methode zurückgegeben werden sollen.

**Codebeispiel 7 an. Verwenden das ScriptMethod-Attribut, mit der ResponseFormat-Eigenschaft.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample7.cs)]

Die ResponseFormat-Eigenschaft kann auch zusammen mit der XmlSerializeString-Eigenschaft verwendet werden. Die XmlSerializeString-Eigenschaft hat den Standardwert "false", was bedeutet, dass alle zurückgeben Typen mit Ausnahme von einer Web-Methode zurückgegebenen Zeichenfolgen als XML serialisiert werden bei der `ResponseFormat` -Eigenschaftensatz auf `ResponseFormat.Xml`. Wenn `XmlSerializeString` nastaven NA hodnotu `true`, werden alle Typen, die von einer Web-Methode zurückgegebenen im XML-Format, einschließlich der Zeichenfolgentypen serialisiert. Wenn die ResponseFormat-Eigenschaft einen Wert aufweist `ResponseFormat.Json` die XmlSerializeString-Eigenschaft wird ignoriert.

Codebeispiel 8 zeigt ein Beispiel für die Verwendung der XmlSerializeString-Eigenschaft zu erzwingen, dass Zeichenfolgen, die als XML serialisiert werden.

**Codebeispiel 8. Verwenden das ScriptMethod-Attribut, mit der XmlSerializeString-Eigenschaft**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample8.cs)]

Der Rückgabewert des Aufrufs der Webmethode GetXmlString siehe Codebeispiel 8 wird weiter angezeigt:

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample9.cs)]

Obwohl JSON-Standardformat die Gesamtgröße des Anforderungs-und Antwortnachrichten minimiert und leichter von ASP.NET AJAX-Clients in einer Browser-Weise verwendet, können die ResponseFormat und XmlSerializeString-Eigenschaften werden verwendet, wenn Clients Anwendungen, z. B. Internet Explorer 5 oder höher erwarten, dass XML-Daten aus einer Webmethode zurückgegeben wird.

## <a name="working-with-complex-types"></a>Arbeiten mit komplexen Typen

Programmausdruck 5 zeigte ein Beispiel für einen komplexen Typ mit dem Namen Customer, die von einem Webdienst zurückgegeben werden. Die Customer-Klasse definiert verschiedene einfache Typen, die intern als Eigenschaften wie Vorname und Nachname vorhanden sind. Komplexe Typen, die als Eingabeparameter verwendet, oder Rückgabetyp auf ein AJAX-fähigen Web-Methode vor dem Senden an die Client-Seite automatisch in JSON serialisiert werden. Allerdings sind geschachtelte komplexe Typen (die intern in einem anderen Typ definiert sind) nicht an dem Client als eigenständige Objekte standardmäßig verfügbar.

In Fällen, in ein geschachtelter komplexer Typ, der von einem Webdienst verwendet auch in einer Clientseite verwendet werden muss, kann das ASP.NET AJAX GenerateScriptType-Attribut mit dem Webdienst hinzugefügt werden. Die CustomerDetails-Klasse im Codebeispiel 9 enthält beispielsweise die Adresse und Geschlecht Eigenschaften der ***geschachtelte komplexe Typen darstellen.***

**Codebeispiel 9. Die hier gezeigte CustomerDetails-Klasse enthält zwei geschachtelte komplexe Typen.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample10.cs)]

Die Adresse und Gender-Objekte, die innerhalb der CustomerDetails-Klasse, die in Codebeispiel 9 gezeigt definiert wird nicht automatisch zur Verfügung gestellt werden für die Verwendung auf der Clientseite über JavaScript enthalten, werden geschachtelte Typen (Adresse ist eine Klasse und Geschlecht ist eine Enumeration). In Situationen, in denen ein geschachtelter Typ, der verwendet wird, innerhalb eines Webdiensts muss auf der Clientseite verfügbar, die zuvor erwähnte GenerateScriptType-Attribut verwendet werden (siehe Codebeispiel 10). Dieses Attribut kann mehrmals in Fällen hinzugefügt werden, in denen andere geschachtelte komplexe Typen von einem Dienst zurückgegeben werden. Sie können direkt an die Webdienst-Klasse oder über bestimmte Webmethoden angewendet werden.

**Codebeispiel 10. Verwenden die GenerateScriptService-Attributs, um geschachtelte Typen zu definieren, die für den Client verfügbar sein sollen.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample11.cs)]

Durch Anwenden der `GenerateScriptType` Attribut, um den Webdienst, die Adresse und das Geschlecht Typen wird automatisch zur Verfügung gestellt werden für die Verwendung durch die clientseitige ASP.NET AJAX-JavaScript-Code. Ein Beispiel für das JavaScript, das automatisch generiert und an den Client gesendet werden, indem Sie die GenerateScriptType-Attribut für einen Webdienst hinzufügen, wird im Codebeispiel 11 angezeigt. Sie sehen, wie Sie geschachtelte komplexe Typen später in diesem Artikel verwenden.

**Codebeispiel 11. Geschachtelte komplexe Typen, die zu einer ASP.NET AJAX-Seite zur Verfügung gestellt.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample12.cs)]

Nun, Sie, dass das Erstellen von Webdiensten und richten sie den Zugriff auf ASP.NET AJAX-Seiten gesehen haben, werfen wir einen Blick auf das Erstellen und Verwenden von JavaScript-Proxys, damit Daten abgerufen oder an Webdienste gesendet werden können.

## <a name="creating-javascript-proxies"></a>Erstellen von JavaScript-Proxys

Aufrufen von in der Regel einen standardmäßigen Webdienst (.NET oder einer anderen Plattform) umfasst das Erstellen ein Proxyobjekt, das Sie von der Komplexität des Sendens von SOAP-Anforderung und Antwort-Nachrichten schützt. Mit ASP.NET AJAX-Webdienst aufrufen können die JavaScript-Proxys werden erstellt und Dienste ganz einfach aufrufen, ohne sich Gedanken machen zu serialisieren und Deserialisieren von JSON-Nachrichten verwendet werden. JavaScript-Proxys können automatisch generiert werden, mithilfe des ASP.NET AJAX-ScriptManager-Steuerelements.

Erstellen eines JavaScript-Proxys, das Webdienste aufrufen können, wird mithilfe ScriptManagers-Verzeichnisdiensteigenschaft erreicht. Diese Eigenschaft können Sie einen oder mehrere Dienste zu definieren, die eine ASP.NET AJAX-Seite zum Senden oder Empfangen von Daten ohne postback Vorgänge asynchron aufrufen kann. Sie definieren einen Dienst mithilfe von ASP.NET AJAX `ServiceReference` -Steuerelement und Zuweisen der Webdienst-URL des Steuerelements `Path` Eigenschaft. Codebeispiel 12 zeigt ein Beispiel der auf einen Dienst mit dem Namen CustomersService.asmx verweist.

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample13.aspx)]

**Codebeispiel 12. Definieren einen Webdienst in einer ASP.NET AJAX-Seite verwendet.**

Hinzufügen eines Verweises auf die CustomersService.asmx über das ScriptManager-Steuerelement wird einen JavaScript-Proxy für dynamisch generierte und von der Seite verwiesen werden. Der Proxy eingebettet ist, mit der &lt;Skript&gt; markieren und dynamisch geladen werden, indem die CustomersService.asmx-Datei aufrufen, und/js an das Ende angefügt. Das folgende Beispiel zeigt, wie der JavaScript-Proxy auf der Seite eingebettet wird, wenn in "Web.config" Debuggen muss deaktiviert sein:

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample14.html)]

> *> [!NOTE] Wenn Sie möchten, um den tatsächlichen JavaScript-Proxy-Code anzuzeigen, die generiert wird, Sie geben die URL der gewünschten .NET Webdienst in Internet Explorer Adressfeld und/js an Ende anfügen können.*


Wenn das Debuggen in "Web.config" aktiviert ist, die auf der Seite als eine Debugversion des JavaScript-Proxys eingebettet werden dargestellten:

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample15.html)]

Die JavaScript-Proxy erstellt, die von ScriptManager können auch direkt in die Seite eingebettet werden anstatt verweisen, indem Sie die &lt;Skript&gt; Src-Attribut des Tags. Dies kann erfolgen, indem das ServiceReference-Steuerelement InlineScript-Eigenschaft auf "true" (der Standardwert ist "false"). Dies kann nützlich sein, wenn ein Proxy nicht freigegeben werden über mehrere Seiten, und wenn Sie die Anzahl der Netzwerkaufrufe an den Server zu reduzieren möchten. Wenn InlineScript festgelegt ist, auf das Skript "true" wird nicht vom Browser zwischengespeichert werden, sodass der Wert von "false" in Fällen empfohlen wird, in denen der Proxy von mehreren Seiten in einer ASP.NET AJAX-Anwendung verwendet wird. Ein Beispiel der Verwendung der InlineScript-Eigenschaft wird weiter angezeigt:

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample16.aspx)]

## <a name="using-javascript-proxies"></a>Verwenden von JavaScript-Proxys

Nachdem Sie einen Webdienst von einer ASP.NET AJAX-Seite, die über das ScriptManager-Steuerelement verwiesen wird, an den Webdienst kann aufgerufen werden, und die zurückgegebenen Daten mithilfe von Rückruffunktionen verarbeitet werden können. Einen Webdienst wird von Verweisen auf den Namespace, (falls vorhanden), Klassennamen und Name der Webmethode aufgerufen. Alle Parameter an den Webdienst übergeben können zusammen mit einer Callback-Funktion definiert werden, die die zurückgegebenen Daten verarbeitet.

Ein Beispiel für ein JavaScript-Proxy zum Aufrufen einer Web-Methode, die mit dem Namen GetCustomersByCountry() wird im Codebeispiel 13 angezeigt. Die GetCustomersByCountry()-Funktion wird aufgerufen, wenn ein Endbenutzer eine Schaltfläche auf der Seite klickt.

**Codebeispiel 13. Aufrufen eines Webdiensts mit einem JavaScript-Proxy.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample17.js)]

Dieser Aufruf verweist auf den Namespace InterfaceTraining, CustomersService-Klasse und GetCustomersByCountry Webmethode in den Dienst definiert. Sie übergibt einen Wert für Land aus einem Textfeld abgerufen als auch eine Rückruffunktion, die mit dem Namen OnWSRequestComplete, die bei Rückgabe des Aufrufs der asynchronen Webdienst aufgerufen werden soll. OnWSRequestComplete verarbeitet das Array von Customer-Objekten, die vom Dienst zurückgegebenen und wandelt sie in einer Tabelle, die auf der Seite angezeigt wird. Die Ausgabe aus dem Aufruf generiert wird, wird in Abbildung 1 dargestellt.


[![Binden von Daten, die abgerufen, indem Sie einen asynchronen AJAX-Aufruf an einen Webdienst erstellen.](understanding-asp-net-ajax-web-services/_static/image2.png)](understanding-asp-net-ajax-web-services/_static/image1.png)

**Abbildung 1**: Binden von Daten abgerufen, indem Sie einen asynchronen AJAX-Aufruf an einen Webdienst erstellen.  ([Klicken Sie, um das Bild in voller Größe anzeigen](understanding-asp-net-ajax-web-services/_static/image3.png))


JavaScript-Proxys lassen sich auch unidirektionale Aufrufe an Webdienste in Fällen, in denen eine Webmethode aufgerufen werden soll, aber der Proxy sollte nicht auf eine Antwort warten. Beispielsweise empfiehlt es sich zum Aufrufen eines Webdiensts zum Starten eines Prozesses wie ein Workflow jedoch nicht für einen Rückgabewert aus dem Dienst warten. In Fällen, in denen ein unidirektionaler Aufruf an einen Dienst erstellt werden muss, kann die Callback-Funktion, die in Codebeispiel 13 gezeigten einfach ausgelassen werden. Da keine Callback-Funktion definiert ist wartet das Proxyobjekt nicht für den Webdienst, um Daten zurückzugeben.

## <a name="handling-errors"></a>Behandeln von Fehlern

Asynchrone Rückrufe mit Webdiensten können auftreten, verschiedene Arten von Fehlern wie z. B. das Netzwerk ausgefallen, den Webdienst nicht verfügbar ist oder eine Ausnahme zurückgegeben wird. Glücklicherweise können JavaScript-Proxy-Objekte, die vom ScriptManager generiert mehrere Rückrufe definiert werden, um Fehler und Ausfälle zusätzlich zu den oben gezeigten Erfolgsrückruf zu behandeln. Eine Fehlerrückruf-Funktion kann unmittelbar auf die standard-Callback-Funktion im Aufruf an die Webmethode definiert werden, wie in Codebeispiel 14 dargestellt.

**Codebeispiel 14. Definieren eine Fehlerrückruf-Funktion, und Anzeigen von Fehlern.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample18.js)]

Fehler, die auftreten, wenn der Webdienst aufgerufen wird, löst die OnWSRequestFailed() Callback-Funktion, um aufgerufen werden akzeptiert ein Objekt, das den Fehler als einen Parameter darstellt. Die Error-Objekt macht mehrere verschiedene Funktionen, um die Ursache des Fehlers zu bestimmen, sowie davon, ob für das Timeout des Aufrufs. Codebeispiel 14 zeigt ein Beispiel mit den anderen Fehlerfunktionen und Abbildung 2 zeigt ein Beispiel der Ausgabe von den Funktionen generiert.


[![Die Ausgabe von Aufrufen von Funktionen für ASP.NET AJAX-Fehler generiert.](understanding-asp-net-ajax-web-services/_static/image5.png)](understanding-asp-net-ajax-web-services/_static/image4.png)

**Abbildung 2**: Ausgabe von Aufrufen von Funktionen für ASP.NET AJAX-Fehler generiert.  ([Klicken Sie, um das Bild in voller Größe anzeigen](understanding-asp-net-ajax-web-services/_static/image6.png))


## <a name="handling-xml-data-returned-from-a-web-service"></a>Behandeln von einem Webdienst zurückgegebene XML-Daten

Zuvor haben Sie erfahren, wie eine Webmethode unformatierte XML-Daten zurückgeben kann, mit dem ScriptMethod-Attribut zusammen mit seiner ResponseFormat-Eigenschaft. Wenn ResponseFormat ResponseFormat.Xml festgelegt ist, werden vom Webdienst zurückgegebene Daten als XML-und nicht in JSON serialisiert. Dies kann nützlich sein, wenn XML-Daten direkt an den Client übergeben werden, für die Verarbeitung von JavaScript oder XSLT. Zum aktuellen Zeitpunkt stellt InternetExplorer 5 oder höher das beste clientseitige Objektmodell zum Analysieren und Filtern von XML-Daten aufgrund der integrierten Unterstützung für MSXML bereit.

Abrufen von XML-Daten von einem Webdienst unterscheidet sich nicht als das Abrufen von anderen Datentypen. Starten Sie durch den JavaScript-Proxy, um die entsprechende Funktion aufrufen, und definieren eine Callback-Funktion aufrufen. Nach der Rückgabe des Aufrufs können Sie dann die Daten in der Rückruffunktion verarbeiten.

Codebeispiel 15 zeigt ein Beispiel für das Aufrufen einer Web-Methode, die mit dem Namen GetRssFeed(), die ein XmlElement-Objekt zurückgibt. GetRssFeed() akzeptiert einen einzelnen Parameter, die URL des RSS-feed, um das Abrufen von darstellt.

**Codebeispiel 15. Arbeiten mit XML-Daten, die von einem Webdienst zurückgegeben.**

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample19.html)]

In diesem Beispiel übergibt eine URL an einen RSS-feed und verarbeitet die zurückgegebene XML-Daten in der OnWSRequestComplete()-Funktion. OnWSRequestComplete() überprüft zuerst, um festzustellen, ob der Browser Internet Explorer ist, um festzustellen, ob die MSXML-Parser verfügbar ist. Wenn es sich handelt, wird eine XPath-Anweisung verwendet, um alle zu finden &lt;Element&gt; -Tags innerhalb der RSS-feed. Jedes Element wird dann durchlaufen und den zugehörigen &lt;Titel&gt; und &lt;Link&gt; Tags gespeichert und verarbeitet werden, um die entsprechenden Daten anzuzeigen. Abbildung 3 zeigt ein Beispiel der Ausgabe generiert eine ASP.NET AJAX-Aufrufen durch eine JavaScript-Proxy an die Webmethode GetRssFeed() vornehmen.

## <a name="handling-complex-types"></a>Behandlung von komplexen Typen

Komplexe Typen akzeptiert oder von einem Webdienst zurückgegeben werden automatisch über einen JavaScript-Proxy verfügbar gemacht. Geschachtelte komplexe Typen sind jedoch nicht direkt zugegriffen werden kann, auf der Clientseite, es sei denn, mit dem Dienst das GenerateScriptType-Attribut angewendet wird, wie bereits erwähnt. Warum sollten Sie sich mit einem geschachtelten komplexen Typ auf der Clientseite?

Um diese Frage zu beantworten, wird davon ausgegangen Sie, dass eine ASP.NET AJAX-Seite zeigt die Daten der Kunden an und ermöglicht Endbenutzern die Adresse eines Kunden zu aktualisieren. Wenn der Webdienst gibt an, dass der Adresstyp (einem komplexen Typ innerhalb einer Klasse CustomerDetails definiert) an den Client gesendet werden kann, kann der Update-Vorgang in separate Funktionen für eine bessere Wiederverwendungsrate für Code unterteilt werden.


[![Ausgabe erstellen, die von Aufrufen eines Webdiensts, die RSS-Daten zurückgibt.](understanding-asp-net-ajax-web-services/_static/image8.png)](understanding-asp-net-ajax-web-services/_static/image7.png)

**Abbildung 3**: Ausgabe erstellen, die von Aufrufen eines Webdiensts, die RSS-Daten zurückgibt.  ([Klicken Sie, um das Bild in voller Größe anzeigen](understanding-asp-net-ajax-web-services/_static/image9.png))


Codebeispiel 16 zeigt ein Beispiel für Client-Side-Code, der eine Adressobjekt, das in einem Modellnamespace definierten aufruft füllt diese mit aktualisierten Daten und Address-Eigenschaft des Objekts ein CustomerDetails zugewiesen. Die CustomerDetails-Objekt wird dann zur Verarbeitung an den Webdienst übergeben.

**Codebeispiel 16. Verwenden von geschachtelten komplexen Typen**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample20.js)]

## <a name="creating-and-using-page-methods"></a>Erstellen und Verwenden von Seitenmethoden

Webdienste bieten eine hervorragende Möglichkeit, aktionsentwicklung Dienste mit einer Vielzahl von Clients, einschließlich ASP.NET AJAX-Seiten verfügbar zu machen. Allerdings gibt es möglicherweise Fälle, in denen eine Seite Daten abzurufen, die nicht einmal verwendet oder von anderen Seiten gemeinsam verwendet werden muss. In diesem Fall mag erstellen eine ASMX-Datei, um die Seite für den Datenzugriff zu ermöglichen etwas aufgebladen klingen, da der Dienst nur von einer einzelnen Seite verwendet wird.

ASP.NET AJAX bietet es sich um einen anderen Mechanismus zum Ausführen von Web Service-ähnlichen Aufrufe ohne das Erstellen von eigenständigen ASMX-Dateien. Dies erfolgt mithilfe einer Technik, die als "Seitenmethoden" bezeichnet. Seitenmethoden werden statische (freigegebene in VB.NET) direkt in eine Seite oder Code-beside-Datei eingebettet, die das WebMethod-Attribut angewendet werden. Durch Anwenden des WebMethod-Attributs können sie aufgerufen werden mit einem speziellen JavaScript-Objekt, das mit dem Namen PageMethods, die zur Laufzeit dynamisch erstellt wird. Das PageMethods-Objekt fungiert als Proxy, der Sie aus dem JSON-Serialisierung/Deserialisierung Prozess schützt. Beachten Sie, um das PageMethods-Objekt verwenden Sie das ScriptModes EnablePageMethods-Eigenschaft auf "true" festlegen müssen.

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample21.aspx)]

Auflisten von 17 zeigt ein Beispiel der Definition von zwei Seitenmethoden in einer ASP.NET Code-beside-Klasse. Diese Methoden Abrufen von Daten aus einer Geschäftsklasse-Ebene befindet sich in der App\_Ordner "Code" der Website.

**Auflisten von 17. Definieren Seitenmethoden.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample22.cs)]

Bei ScriptManager das Vorhandensein von Webmethoden auf der Seite erkennt, generiert einen dynamischen Verweis auf das zuvor erwähnten PageMethods-Objekt. Aufrufen einer Web-Methode erfolgt durch Verweisen auf die PageMethods-Klasse, gefolgt vom Namen der Methode und Parameter, der Daten, die übergeben werden sollen. Auflisten von 18 zeigt Beispiele für die zwei zuvor gezeigten Seitenmethoden aufrufen.

**Auflisten von 18. Aufrufen von Seitenmethoden mit den PageMethods-JavaScript-Objekt.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample23.js)]

Mit dem PageMethods-Objekt ist ein JavaScript-Proxyobjekt mit sehr ähnlich. Sie zuerst angeben aller Parameter, die an die Methode der Seite übergeben werden sollte, und definieren Sie dann auf die Callback-Funktion, die aufgerufen werden soll, wenn der asynchrone Aufruf zurückgegeben. Failure-Rückruf kann auch angegeben werden (siehe Codebeispiel 14 ein Beispiel für die Behandlung von Fehlern).

## <a name="the-autocompleteextender-and-the-aspnet-ajax-toolkit"></a>Die AutoCompleteExtender und das ASP.NET AJAX-Toolkit

Das ASP.NET AJAX-Toolkit (verfügbar über [ http://ajax.asp.net ](http://ajax.asp.net)) bietet mehrere Steuerelemente, die zum Zugriff auf Webdienste verwendet werden können. Insbesondere enthält das Toolkit ein nützlich Steuerelement namens `AutoCompleteExtender` , Webdienste aufrufen und Anzeigen von Daten in Seiten ohne das Schreiben von Javascriptcode überhaupt verwendet werden kann.

Die AutoCompleteExtender-Steuerelement kann verwendet werden, erweitern vorhandene Funktionalität eines Textfelds aus, und unterstützen von Benutzern, die weitere Daten leicht zu finden, die sie suchen. Wie sie in ein Textfeld eingeben, wird das Steuerelement kann zum Abfragen von einem Webdienst verwendet werden, und zeigt dynamisch die Ergebnisse unterhalb des Textfelds. Abbildung 4 zeigt ein Beispiel für das AutoCompleteExtender-Steuerelement zum Anzeigen der Kunden-Ids für eine Anwendung unterstützt. Wie der Benutzer verschiedene Zeichen in das Textfeld eingibt, werden verschiedene Elemente unter sie basierend auf ihrer Eingabe angezeigt. Benutzer können dann die gewünschten Kunden-Id auswählen.

Verwenden die AutoCompleteExtender innerhalb einer ASP.NET AJAX-Seite erfordert, dass die Assembly AjaxControlToolkit.dll Bin-Ordner der Website hinzugefügt werden. Nachdem die Assembly Toolkit hinzugefügt wurde, sollten Sie sie in "Web.config" verweisen, sodass die darin enthaltenen Steuerelemente für alle Seiten in einer Anwendung zur Verfügung stehen. Dies ist möglich durch das Hinzufügen der folgenden Tags in "Web.config" des &lt;Steuerelemente&gt; Tag:

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample24.xml)]

In Fällen, in dem lediglich die Verwendung des Steuerelements in einer bestimmten Seite, können Sie sie verweisen, durch die Reference-Direktive am Anfang einer Seite hinzufügen, siehe nächsten anstelle Datei "Web.config" wird aktualisiert:

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample25.aspx)]


[![Verwenden des AutoCompleteExtender-Steuerelements.](understanding-asp-net-ajax-web-services/_static/image11.png)](understanding-asp-net-ajax-web-services/_static/image10.png)

**Abbildung 4**: Verwenden des AutoCompleteExtender-Steuerelements.  ([Klicken Sie, um das Bild in voller Größe anzeigen](understanding-asp-net-ajax-web-services/_static/image12.png))


Nach die Website für die Verwendung der ASP.NET AJAX-Toolkit konfiguriert wurde, kann ein AutoCompleteExtender-Steuerelement auf der Seite viel hinzugefügt werden, wie Sie ein reguläre ASP.NET-Serversteuerelement hinzufügen würden. Auflisten von 19 zeigt ein Beispiel für das Steuerelement auf einen Webdienst aufzurufen.

**Auflisten von 19. Verwenden des ASP.NET AJAX-Toolkit AutoCompleteExtender-Steuerelements.**

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample26.aspx)]

Die AutoCompleteExtender verfügt über mehrere verschiedene Eigenschaften, einschließlich der standard-ID und Runat Eigenschaften, die auf Serversteuerelemente gefunden. Außerdem können Sie definieren, wie viele Zeichen, die einem Endbenutzer-Typen, bevor der Webdienst wird für Daten abgefragt. Auflisten von 19 gezeigt MinimumPrefixLength-Eigenschaft bewirkt, dass den Dienst jedes Mal aufgerufen, wenn ein Zeichen in das Textfeld eingegeben wurde. Sie sollten darauf achten, dass das Festlegen dieses Werts, da jedes Mal der Benutzer ein Zeichen des Webdiensts gibt aufgerufen wird, um die Suche nach Werten, die die Zeichen in das Textfeld entsprechen. Den Webdienst aufrufen, als auch das Ziel Webmethode werden jeweils mit die ServicePath und ServiceMethod-Eigenschaft definiert. Schließlich gibt die TargetControlID-Eigenschaft der Textbox, um das Steuerelement AutoCompleteExtender verknüpfen.

Der Webdienst aufgerufen wird, müssen das ScriptService-Attribut angewendet wird, wie bereits erwähnt, und das Ziel Webmethode akzeptieren zwei Parameter namens "PrefixText" und "Count. Der PrefixText Parameter darstellt, die vom Endbenutzer eingegebenen Zeichen und der Count-Parameter darstellt, wie viele Elemente zurückgeben (der Standardwert ist 10). Auflisten von 20 zeigt ein Beispiel der Webmethode GetCustomerIDs wird von den weiter oben in der Auflistung 19 AutoCompleteExtender-Steuerelement aufgerufen. Die Web-Methode ruft eine Business-Ebene-Methode, die wiederum Aufrufe einer Methode Schicht, die verarbeitet werden, die Daten filtern und die übereinstimmenden Ergebnisse zurückgegeben. Der Code für die Data-Layer-Methode wird in auflisten 21 dargestellt.

**Auflisten von 20. Filtern von Daten aus dem AutoCompleteExtender-Steuerelement gesendet.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample27.cs)]

**Auflisten von 21. Filtern der Ergebnisse basierend auf Benutzereingaben End.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample28.cs)]

## <a name="conclusion"></a>Schlussbemerkung

ASP.NET AJAX bietet exzellente Unterstützung für das Aufrufen von Webdiensten ohne das Schreiben umfangreichen benutzerdefinierten JavaScript-Code, der Anforderung und Antwort-Nachrichten zu verarbeiten. In diesem Artikel haben Sie erfahren wie AJAX-Aktivierung .NET Web Services, um sie zum Verarbeiten von JSON-Nachrichten und zum Definieren von JavaScript-Proxys, die über das ScriptManager-Steuerelement zu aktivieren. Sie haben auch gesehen, wie JavaScript-Proxys verwendet werden können, zum Aufrufen von Webdiensten, kümmern sich um einfache und komplexe Typen und Fehler behandeln. Schließlich haben Sie gelernt, wie Seitenmethoden verwendet werden können, um den Prozess des Erstellens und Aufrufe des Webdiensts zu vereinfachen und wie das Steuerelement AutoCompleteExtender Hilfe für Endbenutzer homophone bereitstellen kann. Obwohl das Steuerelement der Wahl für viele AJAX-Programmierer aufgrund seiner Einfachheit sicherlich von UpdatePanel in ASP.NET AJAX verfügbar sein wird, kann das wissen, wie das Aufrufen von Webdiensten über JavaScript-Proxys in vielen Anwendungen nützlich sein.

## <a name="bio"></a>Biografie

Dan Wahlin (Microsoft Most Valuable Professional für ASP.NET und XML Web Services) ist eine Entwicklung "Instructor" und Architektur .NET-Berater auf technische Schulungen-Schnittstelle ([http://www.interfacett.com](http://www.interfacett.com)). Dan gegründet wurde, den XML-Code für ASP.NET Entwickler-Website ([www.XMLforASP.NET](http://www.XMLforASP.NET)), befindet sich auf die INETA-Sprecher Bureau und hält Vorträge auf Konferenzen. Dan Co-Autor Professional Windows DNA (Wrox), ASP.NET: Tipps, Tutorials und Code (Sams), ASP.NET 1.1-Insider-Lösungen, Professional ASP.NET 2.0 AJAX (Wrox), ASP.NET 2.0-MVP-Hacks und erstellte XML für ASP.NET-Entwickler (Sams). Wenn er nicht Code, Artikel oder Bücher schreiben wird, profitiert Dan schreiben und Musik Aufzeichnung und Wiedergabe Golf und Basketball mit seiner Frau und Kinder an.

Scott Cate arbeitet mit Microsoft-Web-Technologien seit 1997 und ist Vorsitzender der myKB.com ([www.myKB.com](http://www.myKB.com)), in dem er spezialisiert sich auf das Schreiben von ASP.NET basierende Anwendungen, die mit dem Schwerpunkt Knowledge Base-softwarelösungen. Scott hergestellt werden kann, per e-Mail unter [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) oder seinen Blog unter [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Zurück](understanding-asp-net-ajax-localization.md)
> [Weiter](understanding-asp-net-ajax-debugging-capabilities.md)
