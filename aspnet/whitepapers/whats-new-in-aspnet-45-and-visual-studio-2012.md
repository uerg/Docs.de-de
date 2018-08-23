---
uid: whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
title: Was ist neu in ASP.NET 4.5 und Visual Studio 2012 | Microsoft-Dokumentation
author: rick-anderson
description: Dieses Dokument beschreibt die neuen Features und Verbesserungen, die in ASP.NET 4.5 eingeführt wird. Außerdem wird beschrieben, Verbesserungen für die Webentwicklung...
ms.author: riande
ms.date: 02/29/2012
ms.assetid: ba1fabb4-31a3-4ebf-8327-41a6bbba6eaf
msc.legacyurl: /whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
msc.type: content
ms.openlocfilehash: 6bbfb4aa7f29e4c189da4dfdca6f2113c7550b68
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832312"
---
<a name="whats-new-in-aspnet-45-and-visual-studio-2012"></a>Neuerungen in ASP.NET 4.5 und Visual Studio 2012
====================
> Dieses Dokument beschreibt die neuen Features und Verbesserungen, die in ASP.NET 4.5 eingeführt wird. Darüber hinaus werden die Verbesserungen für die Webentwicklung in Visual Studio 2012 beschrieben. In diesem Dokument wurde ursprünglich auf dem 29. Februar 2012 veröffentlicht.


- [ASP.NET Core-Runtime und Framework](#_Toc318097372)

    - [Asynchrones Lesen und Schreiben von HTTP-Anforderungen und Antworten](#_Toc318097373)
    - [Verbesserungen bei der Behandlung von HttpRequest](#_Toc318097374)
    - [Leeren asynchron auf eine Antwort](#_Toc318097375)
    - [Unterstützung für *"await"* und *Aufgabe*-basierte asynchroner Module und Handler](#_Toc318097376)
    - [Asynchroner HTTP-Module](#_Toc318097377)
    - [HTTP-Handler](#_Toc318097378)
    - [Neue ASP.NET-Anforderung Validierungsfunktionen](#_Toc318097379)
    - [Verzögerte Anforderungsvalidierung ("lazy")](#_Toc318097380)
    - [Unterstützung für nicht überprüfter Anforderungen](#_Toc318097381)
    - [AntiXSS-Bibliothek](#_Toc318097382)
    - [Unterstützung für WebSockets-Protokoll](#_Toc318097383)
    - [Bündelung und Minimierung](#_Toc318097384)
    - [Leistungsverbesserungen für Webhosting](#_Toc_perf)

        - [Wichtigsten Leistungsfaktoren](#_Toc_perf_1)
        - [Anforderungen für die neuen Leistungsfeatures](#_Toc_perf_2)
        - [Freigeben von gemeinsamen Assemblys](#_Toc_perf_3)
        - [Verwenden von Mehrkern-JIT-Kompilierung für schnellere Starts](#_Toc_perf_4)
        - [Optimieren der Garbagecollection für den Arbeitsspeicher zu optimieren.](#_Toc_perf_5)
        - [Der Vorabruf für Webanwendungen](#_Toc_perf_6)
- [ASP.NET-Web Forms](#_Toc318097385)

    - [Stark typisierte Datensteuerelemente](#_Toc318097386)
    - [Modellbindung](#_Toc318097387)

        - [Auswählen von Daten](#_Toc318097388)
        - [Wertanbieter](#_Toc318097389)
        - [Filtern nach Werten von einem Steuerelement](#_Toc318097390)
    - [HTML-codierte Datenbindungsausdrücke](#_Toc318097391)
    - [Unaufdringliche Validierung](#_Toc318097392)
    - [HTML5-Updates](#_Toc318097393)
- [ASP.NET MVC 4](#_Toc318097394)
- [ASP.NET Web Pages 2](#_Toc318097395)
- [Visual Studio 2012 Release Candidate](#_Toc318097396)

    - [Projektfreigabe, die zwischen Visual Studio 2010 und Visual Studio 2012 Release Candidate (Projektkompatibilität)](#project-compatibility)
    - [Änderungen an der Konfiguration in ASP.NET 4.5-Websitevorlagen](#Configuration_Changes_In_ASPNET45_Website_Templates)
    - [Systemeigene Unterstützung in IIS 7 für das ASP.NET-Routing](#Native_Support_In_IIS7_For_ASPNET_Routine)
    - [HTML-Editor](#_Toc318097397)

        - [Smarttasks](#_Toc318097398)
        - [WAI-ARIA-Unterstützung](#_Toc318097399)
        - [Neuer HTML5-Ausschnitte](#_Toc318097400)
        - [In Benutzersteuerelement extrahieren](#_Toc318097401)
        - [IntelliSense für codenuggets in Attributen](#_Toc318097402)
        - [Automatisches Umbenennen des übereinstimmenden Tags aus, wenn Sie ein Start- oder Endtags umbenennen](#_Toc318097403)
        - [Generieren von Ereignishandlern](#_Toc318097404)
        - [Intelligenter Einzug](#_Toc318097405)
        - [Automatisch eingrenzende Anweisungsvervollständigung](#_Toc318097406)
    - [JavaScript-Editor](#_Toc318097407)

        - [Codegliederung](#_Toc318097408)
        - [Überprüfung des Klammergleichgewichts](#_Toc318097409)
        - [Gehe zu Definition](#_Toc318097410)
        - [Unterstützung von ECMAScript5](#_Toc318097411)
        - [DOM-IntelliSense](#_Toc318097412)
        - [Überladungen der VSDOC-Signatur](#_Toc318097413)
        - [Implizite Verweise](#_Toc318097414)
    - [CSS-Editor](#_Toc318097415)

        - [Automatisch eingrenzende Anweisungsvervollständigung](#_Toc318097416)
        - [Hierarchischer Einzug.](#_Toc318097417)
        - [Unterstützung von CSS-weichen](#_Toc318097418)
        - [Hersteller bestimmte Schemas (- Moz-, - Webkit)](#_Toc318097419)
        - [Auskommentieren und Aufheben der auskommentierung-Unterstützung](#_Toc318097420)
        - [Farbauswahl](#_Toc318097421)
        - [Snippets (Ausschnitte)](#_Toc318097422)
        - [Benutzerdefinierte Bereiche](#_Toc318097423)
    - [Seitenprüfung](#_Toc318097424)
    - [Veröffentlichen](#_Toc318097425)

        - [Veröffentlichungsprofile](#_Toc318097426)
        - [ASP.NET-Vorkompilierung und merge](#_Toc318097427)
- [IIS Express](#_Toc318097428)
- [Haftungsausschluss](#_Toc318097429)

<a id="_Toc318097372"></a>
## <a name="aspnet-core-runtime-and-framework"></a>ASP.NET Core-Runtime und Framework

<a id="_Toc318097373"></a>
### <a name="asynchronously-reading-and-writing-http-requests-and-responses"></a>Asynchrones Lesen und Schreiben von HTTP-Anforderungen und Antworten

ASP.NET 4 eingeführt, die Möglichkeit, lesen eine HTTP-Anforderungsentität als Datenstrom mithilfe der *HttpRequest.GetBufferlessInputStream* Methode. Diese Methode bereitgestellt, streaming Zugriff auf die Anforderungsentität. Allerdings es synchron ausgeführt, die für die Dauer einer Anforderung einen Thread gebunden.

ASP.NET 4.5 unterstützt die Möglichkeit, Datenströme asynchron auf eine HTTP-Anforderungsentität gelesen und die Möglichkeit, asynchron zu leeren. ASP.NET 4.5 gibt Ihnen außerdem die Möglichkeit, eine HTTP-Anforderungsentität, bietet einfachere Integration mit downstream-HTTP-Handler wie z. B. aspx-Seitenhandler und ASP.NET MVC Controllers Double-Wert-Puffer.

<a id="_Toc318097374"></a>
#### <a name="improvements-to-httprequest-handling"></a>Verbesserungen bei der Behandlung von HttpRequest

Der Stream-Verweis, der von ASP.NET 4.5 zurückgegebenen *HttpRequest.GetBufferlessInputStream* unterstützt synchrone und asynchrone read-Methoden. Die *Stream* von zurückgegebene Objekt *GetBufferlessInputStream* implementiert nun sowohl das BeginRead und EndRead-Methoden. Die asynchrone *Stream* Methoden können Sie die Anforderungsentität in Segmenten asynchron zu lesen, während ASP.NET zwischen jeder Iteration der einen asynchronen read-Schleife den aktuellen Thread frei.

ASP.NET 4.5 wurde eine Begleitmethode für das Lesen der Anforderungsentität gepufferten so auch hinzugefügt: *HttpRequest.GetBufferedInputStream*. Diese neue Überladung funktioniert wie *GetBufferlessInputStream*, synchrone und asynchrone Lesevorgänge unterstützt. Jedoch, wie es liest, *GetBufferedInputStream* kopiert die Entität Bytes in den internen Puffer von ASP.NET, damit downstream-Module und Handler weiterhin Anforderungsentität zugreifen können. Zum Beispiel wenn einige upstream Code in der Pipeline wurde bereits gelesen die Entität mithilfe *GetBufferedInputStream*, können Sie trotzdem verwenden *HttpRequest.Form* oder *: HttpRequest.Files*. Dadurch können Sie asynchrone Verarbeitungen ausführen, auf eine Anforderung (z. B. das streaming eine große Dateiuploads mit einer Datenbank), aber weiterhin ausführen ASPX-Seiten und MVC ASP.NET-Controller danach.

<a id="_Toc318097375"></a>
#### <a name="asynchronously-flushing-a-response"></a>Leeren asynchron auf eine Antwort

Senden von Antworten an einen HTTP-Client kann lange dauern, wenn der Client ist weit weg, oder es muss eine Verbindung mit geringer Bandbreite. Normalerweise puffert ASP.NET die Antwortbytes, wie sie von einer Anwendung erstellt werden. ASP.NET führt dann einen einzelnen Sendevorgang, der die aufgelaufenen Puffer ganz am Ende der anforderungsverarbeitung an.

Wenn die gepufferte Antwort (z. B. eine große Datei auf einen Client streamen) groß ist, müssen Sie in regelmäßigen Abständen Aufrufen *HttpResponse.Flush* gepufferte Ausgabe an den Client gesendet und speicherauslastung unter Kontrolle zu halten. Aber da *leeren* ist ein synchroner Aufruf, iterativ Aufrufen *leeren* belegt weiterhin einen Thread für die Dauer der Anforderungen mit potenziell langer.

ASP.NET 4.5 bietet Unterstützung für das Ausführen von leert asynchron mit dem *BeginFlush* und *EndFlush* Methoden der *HttpResponse* Klasse. Mit diesen Methoden können Sie erstellen asynchroner Module und asynchrone Handler, die Daten inkrementell an einen Client zu senden, ohne das Betriebssystem Threads beschäftigen. In der Zwischenzeit *BeginFlush* und *EndFlush* Aufrufe, ASP.NET gibt den aktuellen Thread frei. Dies verringert erheblich die Gesamtzahl der aktiven Threads, die erforderlich sind, um lang andauernde HTTP-Downloads zu unterstützen.

<a id="_Toc318097376"></a>
### <a name="support-for-await-and-task---based-asynchronous-modules-and-handlers"></a>Unterstützung für *"await"* und *Aufgabe* -basierte asynchroner Module und Handler

.NET Framework 4 eingeführt wurde, eine asynchrone Programmierkonzept, das als ein *Aufgabe*. Aufgaben werden durch dargestellt die *Aufgabe* Typ und verwandte Typen in der *System.Threading.Tasks* Namespace. .NET Framework 4.5 basiert dies mit Verbesserungen des Compilers, die Arbeit mit machen *Aufgabe* Objekte einfach. In .NET Framework 4.5, unterstützt der Compiler zwei neue Schlüsselwörter: *"await"* und *Async*. Die *"await"* -Schlüsselwort ist syntaktische Kurzform zum angeben, die ein Stück Code asynchron auf den anderen Teil des Codes warten soll. Die *Async* Schlüsselwort stellt einen Hinweis, mit denen Sie kennzeichnen Methoden als aufgabenbasierte asynchrone Methoden dar.

Die Kombination von *"await"*, *Async*, und die *Aufgabe* -Objekts macht es viel einfacher Schreiben von asynchronem Code in .NET 4.5. ASP.NET 4.5 unterstützt diese vereinfachungen mit neuen APIs, mit denen Sie die asynchrone HTTP-Module und die neuen Compiler-Erweiterungen mit HTTP-Handler zu schreiben.

<a id="_Toc318097377"></a>
#### <a name="asynchronous-http-modules"></a>Asynchroner HTTP-Module

Nehmen wir an, dass asynchronen Arbeit innerhalb einer Methode auszuführen, die zurückgegeben werden soll eine *Aufgabe* Objekt. Das folgende Codebeispiel definiert eine asynchrone Methode, die einen asynchronen Aufruf an die Startseite von Microsoft herunterladen kann. Beachten Sie die Verwendung der *Async* -Schlüsselwort in der Signatur der Methode und die *"await"* aufrufen, um *DownloadStringTaskAsync*.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample1.cs)]

Das ist alles, was Sie zum Schreiben, .NET Framework übernimmt automatisch das Entladen der Aufrufliste beim Warten auf des Downloads abgeschlossen, als auch die Aufrufliste automatisch wiederhergestellt, nachdem der Download abgeschlossen ist.

Nehmen wir nun an, dass Sie diese asynchrone Methode in einem asynchronen ASP.NET HTTP-Modul verwenden möchten. ASP.NET 4.5 enthält eine Hilfsmethode (*eventhandlertaskasynchelper unterstützt*) und ein neuer Delegattyp (*TaskEventHandler*), die Sie verwenden können, um die aufgabenbasierte asynchrone Methoden mit dem älteren integrieren asynchrone Programmiermodell, das von der ASP.NET-HTTP-Pipeline verfügbar gemacht werden. Dieses Beispiel zeigt, wie:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample2.cs)]

<a id="_Toc318097378"></a>
#### <a name="asynchronous-http-handlers"></a>HTTP-Handler

Der herkömmliche Ansatz zum Schreiben von asynchronen Handler in ASP.NET besteht darin, implementieren die *IHttpAsyncHandler* Schnittstelle. ASP.NET 4.5 führt der *httptaskasynchandler unterstützt* asynchrone Basistyp, der Sie ableiten können, dadurch wird es viel einfacher, asynchrone Handler zu schreiben.

Die *httptaskasynchandler unterstützt* Typ ist abstrakt und erfordert, dass Sie zum Überschreiben der *ProcessRequestAsync* Methode. Intern kümmert sich ASP.NET die Integration der return-Signatur (ein *Aufgabe* Objekt) des *ProcessRequestAsync* mit dem älteren asynchrone Programmiermodell, die von der ASP.NET-Pipeline verwendet.

Das folgende Beispiel zeigt, wie Sie verwenden können *Aufgabe* und *"await"* als Teil der Implementierung von asynchronen HTTP-Handler:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample3.cs)]

<a id="_Toc318097379"></a>
### <a name="new-aspnet-request-validation-features"></a>Neue ASP.NET-Anforderung Validierungsfunktionen

Standardmäßig führt ASP.NET Anforderungsvalidierung – untersucht Anforderungen nach Markup oder Skript in die Felder, Header, Cookies und So weiter gesucht werden soll. Wenn alle erkannt wird, wird von ASP.NET eine Ausnahme ausgelöst. Dies dient als erste Verteidigungslinie gegen potenzielle Cross-Site-scripting-Angriffe.

ASP.NET 4.5 erleichtert die selektiv Lesen nicht überprüfter Anforderungsdaten. ASP.NET 4.5 integriert auch die beliebte AntiXSS-Bibliothek, die zuvor eine externe Bibliothek war.

Entwickler haben die Möglichkeit, selektiv zu deaktivieren, bis die Anforderungsvalidierung für ihre Anwendungen häufig gebeten. Beispielsweise wenn Ihre Anwendung Forum Software ist, empfiehlt, damit Benutzer Beiträge im Forum HTML-Format und Kommentare zu übermitteln, aber immer noch stellen Sie sicher, dass die Anforderungsvalidierung alles überprüft werden.

ASP.NET 4.5 stellt zwei Funktionen, mit denen Sie selektiv arbeiten mit nicht überprüfte Eingabe erleichtert: verzögerte Anforderungsvalidierung ("lazy") und den Zugriff auf nicht überprüfter Anforderungsdaten.

<a id="_Toc318097380"></a>
#### <a name="deferred-lazy-request-validation"></a>Verzögerte Anforderungsvalidierung ("lazy")

In ASP.NET 4.5 haben alle Anforderungsdaten wird standardmäßig gemäß der Request-Überprüfung. Allerdings können Sie konfigurieren, die Anwendung für die Anforderungsvalidierung zu verzögern, bis Sie tatsächlich auf Daten zugreifen. (Dies wird manchmal als verzögerter Anforderungsvalidierung, basierend auf Bedingungen wie lazy Loading für bestimmte Datenszenarien bezeichnet.) Sie können konfigurieren, dass die Anwendung zur Verwendung der verzögerten Validierung in der Datei "Web.config" durch Festlegen der *RequestValidationMode* -Attribut auf 4.5 die *HttpRUntime* Element, wie im folgenden Beispiel gezeigt:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample4.xml)]

Wenn Anforderungsmodus für die Überprüfung auf 4.5 festgelegt ist, wird die Anforderungsvalidierung ausgelöst, nur für eine bestimmte Anforderungswert und nur, wenn Ihr Code auf diesen Wert zugreift. Wenn Ihr Code den Wert der Request.Form["forum erhält beispielsweise\_buchen"], Request-Überprüfung wird nur für dieses Element in der formularauflistung aufgerufen. Keines der anderen Elemente in der *Formular* Auflistung werden überprüft. In früheren Versionen von ASP.NET wurde Anforderungsvalidierung für die Sammlung für die gesamte Anforderung ausgelöst, wenn ein Element in der Auflistung zugegriffen wurde. Das neue Verhalten erleichtert es für verschiedene Anwendungskomponenten in verschiedene Teile der Anforderungsdaten ansehen, ohne die Anforderungsvalidierung für andere Codeelemente.

<a id="_Toc318097381"></a>
#### <a name="support-for-unvalidated-requests"></a>Unterstützung für nicht überprüfter Anforderungen

Verzögerte Anforderungsvalidierung allein problemlösung nicht zur Anforderungsvalidierung selektiv zu umgehen. Der Aufruf von Request.Form["forum\_buchen"] noch Trigger die Anforderungsvalidierung für diese bestimmte Anforderungswert. Möglicherweise möchten jedoch Zugriff auf dieses Feld ohne Validierung auslösen, da Sie Markup in diesem Feld zulassen möchten.

Um dies zu ermöglichen, unterstützt ASP.NET 4.5 jetzt nicht überprüfter Zugriff auf Daten. ASP.NET 4.5 enthält ein neues *Unvalidated* Auflistungseigenschaft, die in der *HttpRequest* Klasse. Diese Auflistung ermöglicht den Zugriff auf alle gemeinsamen Werte der Anforderungsdaten, wie z. B. *Formular*, *QueryString*, *Cookies*, und *Url*.

Verwenden das Forum beispielsweise Lesen nicht überprüfter Anforderungsdaten können, müssen Sie zuerst die Anwendung zur Verwendung des neuen Anforderung Validierungsmodus konfigurieren:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample5.xml)]

Anschließend können Sie die *HttpRequest.Unvalidated* -Eigenschaft in der nicht validierten Formularwert:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample6.cs)]


> [!WARNING]
> Sicherheit – *nicht überprüfter Anforderungsdaten mit Vorsicht verwenden.* ASP.NET 4.5 hinzugefügt, die nicht überprüfte Anforderungseigenschaften und Sammlungen, um spezifische nicht überprüfter Anforderungsdaten den Zugriff auf erleichtern. Sie müssen jedoch weiterhin benutzerdefinierte Validierung ausführen, für die raw-Anforderungsdaten, um sicherzustellen, dass gefährlicher Text nicht für Benutzer gerendert wird.


<a id="_Toc318097382"></a>
### <a name="antixss-library"></a>AntiXSS-Bibliothek

Aufgrund der Beliebtheit von Microsoft AntiXSS Library enthält ASP.NET 4.5 jetzt kerncodierungsroutinen von Version 4.0 von dieser Bibliothek.

Die Codierungsroutinen werden implementiert, indem die *AntiXssEncoder* Typ in der neuen *System.Web.Security.AntiXss* Namespace. Sie können die *AntiXssEncoder* Typ direkt durch das Aufrufen einer der statischen Methoden Codierung, die in den Typ implementiert werden. Der einfachste Ansatz für die Verwendung der neuen Anti-XSS-Routinen ist jedoch so konfigurieren Sie eine ASP.NET-Anwendung zum Verwenden der *AntiXssEncoder* Klasse standardmäßig. Zu diesem Zweck fügen Sie der Datei "Web.config" das folgende Attribut hinzu:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample7.xml)]

Wenn die *EncoderType* -Attribut festgelegt ist, verwendet der *AntiXssEncoder* , alle Ausgabetyp Codierung in ASP.NET automatisch die neuen Codierungsroutinen verwendet.

Hierbei handelt es sich um die Teile der externen AntiXSS-Bibliothek, die in ASP.NET 4.5 integriert haben:

- *"HTMLEncode"*, *HtmlFormUrlEncode*, und *HtmlAttributeEncode*
- *XmlAttributeEncode* und *XmlEncode*
- *Dieses als UrlEncode* und *UrlPathEncode* (neu)
- *CssEncode*

<a id="_Toc318097383"></a>
### <a name="support-for-websockets-protocol"></a>Unterstützung für WebSockets-Protokoll

WebSockets-Protokoll ist ein Netzwerkprotokoll Standards basieren, die definiert, wie Sie sichere, in Echtzeit bidirektionale Kommunikation zwischen einem Client und einem Server über HTTP herzustellen. Microsoft arbeitet mit sowohl die IETF als auch die W3C-Standards als Hilfestellung zum Definieren des Protokolls. Das WebSockets-Protokoll wird von jedem Client (nicht nur Browser), mit Microsoft investieren erhebliche Ressourcen zur Unterstützung von WebSockets-Protokoll sowohl Client als auch mobilen Betriebssystemen unterstützt.

WebSockets-Protokoll vereinfacht es, lang andauernde Datenübertragungen zwischen einem Client und einem Server zu erstellen. Beispielsweise ist das Schreiben einer Chat-Anwendung viel einfacher, da Sie eine "true" langer-Verbindung zwischen einem Client und einem Server herstellen können. Sie müssen keinen problemumgehungen wie z. B. regelmäßig abgerufen oder HTTP-langer Abrufdauer simulieren des Verhaltens eines Sockets verwenden zu können.

ASP.NET 4.5 und IIS 8 gehören die Low-Level WebSockets-Unterstützung, ASP.NET wird Entwicklern ermöglicht, verwalteten APIs zu verwenden, zum asynchronen Lesen und Schreiben von sowohl Zeichenfolge als auch binäre Daten auf eine WebSockets-Objekt. Für ASP.NET 4.5, es gibt eine neue *System.Web.WebSockets* Namespace, die Typen enthält, für die Verwendung von WebSockets-Protokoll.

Ein Browserclient stellt eine WebSockets-Verbindung her, erstellen Sie ein DOM *WebSocket* Objekt, das an eine URL in einer ASP.NET-Anwendung aus, wie im folgenden Beispiel zeigt:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample8.cs)]

Sie können WebSockets-Endpunkte in ASP.NET mit jeder Art von Modul oder Handler erstellen. Im vorherigen Beispiel wurde eine ASHX-Datei verwendet, da die ASHX-Dateien eine schnelle Möglichkeit, einen Ereignishandler zu erstellen sind.

Gemäß der WebSockets-Protokoll akzeptiert eine ASP.NET-Anwendung WebSockets-Anforderung des Clients angeben, dass die Anforderung von einer HTTP GET-Anforderung auf eine WebSockets-Anforderung aktualisiert werden sollten. Im Folgenden ein Beispiel:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample9.cs)]

Die *AcceptWebSocketRequest* -Methode akzeptiert einen Funktionsdelegaten an, da ASP.NET die aktuelle HTTP-Anforderung entlädt, und klicken Sie dann überträgt die Steuerung an den Funktionsdelegaten. Dieser Ansatz ist vom Konzept her ähnelt der Verwendung von *System.Threading.Thread*, dort können Sie einen Thread für die ersten Delegaten definieren in die hintergrundverarbeitung ausgeführt wird.

Nachdem ASP.NET und der Client einen Handshake des WebSockets erfolgreich abgeschlossen wurden, ruft der Delegat für ASP.NET und WebSockets dem Start der Anwendung. Das folgende Codebeispiel zeigt eine einfache echoanwendung, die die integrierte Unterstützung für WebSockets in ASP.NET verwendet:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample10.cs)]

Die Unterstützung in .NET 4.5 für die *"await"* -Schlüsselwort und asynchrone aufgabenbasierte Vorgänge ist ideal für das Schreiben von WebSockets-Anwendungen. Im Codebeispiel wird veranschaulicht, dass eine WebSockets-Anforderung in ASP.NET vollständig asynchron ausgeführt wird. Die Anwendung wartet asynchron eine Nachricht von einem Client gesendet werden, durch den Aufruf *"await" Socket. "ReceiveAsync"*. Sie können auf ähnliche Weise eine asynchrone Meldung an einen Client senden, durch den Aufruf *"await" Socket. SendAsync*.

Im Browser empfängt eine Anwendung WebSockets-Nachrichten über eine *Onmessage* Funktion. Zum Senden einer Nachricht über einen Browser, rufen Sie die *senden* Methode der *WebSocket* DOM-Datentyp, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample11.cs)]

In Zukunft möglicherweise Updates für diese Funktionalität veröffentlichen wir, dass abstrakte sofort einiger, d. h. die Low-Level Codierung in dieser Version für WebSockets Anwendungen erforderlich.

<a id="_Toc318097384"></a>
### <a name="bundling-and-minification"></a>Bündelung und Minimierung

Bündeln können Sie die einzelne JavaScript- und CSS-Dateien in einem Bündel zu kombinieren, die wie eine einzelne Datei behandelt werden kann. Minimierung fasst JavaScript und CSS-Dateien durch das Entfernen von Leerzeichen und andere Zeichen, die nicht erforderlich sind. Diese Features funktionieren mit Web Forms, ASP.NET MVC und Webseiten.

Pakete werden über die Bundle-Klasse oder eine ihrer untergeordneten Klassen, ScriptBundle und "stylebundle" erstellt. Nachdem eine Instanz eines Pakets konfiguriert haben, ist das Paket einfach zu einer globalen BundleCollection-Instanz hinzu, auf eingehende Anforderungen zur Verfügung gestellt. In den Standardvorlagen wird die Paket-Konfiguration in eine BundleConfig-Datei ausgeführt. Diese Standardkonfiguration erstellt Pakete für alle Kerne Skripts und Css-Dateien, die von den Vorlagen verwendeten.

Pakete werden von in Ansichten mithilfe eines der Beispiele für mögliche Hilfsmethoden verwiesen. Um das Rendern von anderen Markups für ein Paket in Debug und Release-Modus zu unterstützen, verfügen die ScriptBundle und "stylebundle"-Klassen die Hilfsmethode, die zu rendern. Wenn Sie sich im Debugmodus befindet, generiert Render-Markup für jede Ressource im Paket. Im Releasemodus generiert Render ein einzelnes Markupelement für das gesamte Paket. Umschalten zwischen Debug- und Releaseversionen Modus ausgeführt werden kann, indem Sie das Debug-Attribut, der das Compilation-Element in der Datei web.config ändern, wie unten dargestellt:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample12.xml)]

Darüber hinaus kann aktivieren oder Deaktivieren der Optimierung direkt über die BundleTable.EnableOptimizations-Eigenschaft festgelegt werden.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample13.cs)]

Wenn Dateien gebündelt werden, diese zuerst alphabetisch sortiert (wie sie, in angezeigt werden **Projektmappen-Explorer**). Sie werden dann so konzipiert, dass Bibliotheken bezeichnet, und ihre benutzerdefinierten Erweiterungen (z. B. jQuery, MooTools und Dojo) zuerst geladen werden. Beispielsweise werden die endgültige Reihenfolge für die Bündelung der Ordner "Scripts" wie oben gezeigt:

1. jquery-1.6.2.js
2. jQuery-ui.js
3. jquery.tools.js
4. a.js

CSS-Dateien sind auch alphabetisch sortiert, und klicken Sie dann neu organisiert, damit reset.css "und" normalize.css vor jeder anderen Datei haben. Die endgültige Sortierung die Bündelung des oben gezeigten Formate Ordners wird dies Folgendes sein:

1. reset.css
2. Content.CSS
3. forms.css
4. globals.css
5. Menu.CSS
6. styles.css

<a id="_Toc_perf"></a>
### <a name="performance-improvements-for-web-hosting"></a>Leistungsverbesserungen für Webhosting

Führen die .NET Framework 4.5 und Windows 8 Funktionen, mit die Sie eine erhebliche Leistungssteigerung bewirken für Web-Server-Workloads erzielen können. Dies schließt eine Reduzierung (bis zu 35 %) in beiden Startzeit und den Speicherbedarf der Web-hosting von Websites, die ASP.NET verwenden.

<a id="_Toc_perf_1"></a>
#### <a name="key-performance-factors"></a>Wichtigsten Leistungsfaktoren

Im Idealfall sollten alle Websites aktiv sein und im Arbeitsspeicher, um die schnelle Antwort auf die nächste Anforderung zu gewährleisten, wenn es darum geht. Faktoren, die websitereaktionsfähigkeit beeinflussen können, gehören:

- Der Zeitaufwand für eine Website neu starten, nachdem ein Anwendungspool wiederverwendet wird. Dies ist der Zeitaufwand für das Starten eines Webserverprozess für den Standort aus, wenn die Website-Assemblys nicht mehr im Arbeitsspeicher befinden. (Die Testplattform-Assemblys sind immer noch im Speicher, da sie von anderen Standorten verwendet werden). Diese Situation wird als "kalte Site, betriebsbereiten Framework Start" bezeichnet oder einfach "Cold Website beim Start."
- Wie viel Arbeitsspeicher belegt, der Standort. Lizenzbestimmungen für diese befinden sich "arbeitsspeichernutzung pro Site" oder "nicht freigegebenes Arbeitssatz".

Die neuen leistungsverbesserungen für beide dieser Faktoren konzentrieren.

<a id="_Toc_perf_2"></a>
#### <a name="requirements-for-new-performance-features"></a>Anforderungen für die neuen Leistungsfeatures

Die Anforderungen für die neuen Funktionen können in folgende Kategorien unterteilt werden:

- Verbesserungen, die auf .NET Framework 4 ausgeführt wird.
- Verbesserungen, die .NET Framework 4.5 erforderlich, aber können auf eine beliebige Version von Windows ausführen.
- Verbesserungen, die nur mit .NET Framework 4.5 unter Windows 8 verfügbar sind.

Leistung nimmt mit jeder Ebene der Verbesserung, die Sie aktivieren können.

Einige der .NET Framework 4.5 Verbesserungen nutzen umfassendere Funktionen, die als auch auf andere Szenarien angewendet werden.

<a id="_Toc_perf_3"></a>
#### <a name="sharing-common-assemblies"></a>Freigeben von gemeinsamen Assemblys

**Anforderung**: .NET Framework 4 und Visual Studio 11 Developer Preview SDK

Unterschiedliche Websites auf einem Server verwenden häufig die gleichen Helper-Assemblys (z. B. Assemblys aus einem Starter Kit oder Beispiel-Anwendung). Jeder Standort hat eine eigene Kopie des diese Assemblys im Verzeichnis "bin". Obwohl der Objektcode für die Assemblys identisch ist, werden sie physisch getrennte Assemblys sind, daher muss jede Assembly während des Starts der kalten Standort separat gelesen werden und separat im Arbeitsspeicher gespeichert.

Die neue Funktionen für die interning diese Ineffizienz zu lösen und reduziert sowohl die RAM-Anforderungen als auch die Auslastung Zeit. Eine Internalisierung können Windows behalten eine einzige Kopie jeder Assembly im Dateisystem, und es werden einzelne Assemblys im Bin-Ordner Website werden durch symbolische Verknüpfungen auf die Kopie ersetzt. Wenn ein einzelnes Standorts eine unterschiedliche Version der Assembly benötigt, die symbolische Verknüpfung wird durch die neue Version der Assembly ersetzt, und nur diesem Standort betroffen ist.

Freigeben von Assemblys mit symbolischen Verknüpfungen erfordert ein neues Tool namens Aspnet\_intern.exe, was Ihnen das Erstellen und Verwalten von im Speicher des internalisierte Assemblys. Es wird als Teil der Visual Studio 11 Developer Preview SDK bereitgestellt. (Dies funktioniert jedoch, wird auf einem System, nur .NET Framework 4 installiert ist, vorausgesetzt, Sie haben die neueste Version installiert hat [aktualisieren](https://support.microsoft.com/kb/2468871).)

Um sicherzustellen, dass alle berechtigten Assemblys haben intern gespeichert wurde, führen Sie Aspnet\_intern.exe in regelmäßigen Abständen (z. B. einmal pro Woche als geplanter Task). Ein typisches Anwendungsbeispiel lautet wie folgt aus:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample14.cmd)]

Um alle Optionen anzuzeigen, führen Sie das Tool ohne Argumente.

<a id="_Toc_perf_4"></a>
#### <a name="using-multi-core-jit-compilation-for-faster-startup"></a>Verwenden von Mehrkern-JIT-Kompilierung für schnellere Starts

**Anforderung**: .NET Framework 4.5

Für ein Startupunternehmen kalte Standort nicht nur müssen Assemblys vom Datenträger gelesen werden, sondern die Site JIT-kompiliert werden muss. Für eine komplexe Website kann dies beträchtlichen Verzögerungen hinzufügen. Ein neues allgemeines Verfahren in .NET Framework 4.5 verringert diese Verzögerungen bei der JIT-Kompilierung auf verfügbaren Prozessorkerne zu verteilen. Dies geschieht so viel und so früh wie möglich mit Informationen, die während der vorherigen startet der Website. Diese Funktion implementiert die [System.Runtime.ProfileOptimization.StartProfile](https://msdn.microsoft.com/library/system.runtime.profileoptimization.startprofile(VS.110).aspx) Methode.

JIT-Kompilierung mit mehreren Kernen ist standardmäßig aktiviert in ASP.NET benötigen Sie nicht, nichts tun, um dieses Feature zu nutzen. Wenn Sie diese Funktion deaktivieren möchten, stellen Sie die folgende Einstellung in der Datei "Web.config" ein:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample15.xml)]

<a id="_Toc_perf_5"></a>
#### <a name="tuning-garbage-collection-to-optimize-for-memory"></a>Optimieren der Garbagecollection für den Arbeitsspeicher zu optimieren.

**Anforderung**: .NET Framework 4.5

Sobald eine Website ausgeführt wird, kann die Verwendung von der Garbage-Collector (GC) Heap ein bedeutender Faktor für die Nutzung des sein. Wie alle Garbage Collector wird die .NET Framework GC ausgewogenes Maß an CPU-Zeit (Häufigkeit und die Bedeutung von Sammlungen) und arbeitsspeichernutzung (zusätzlichen Speicherplatz, der für neue, freigegeben oder freigeben kann Objekte verwendet wird). Früherer Versionen, wir haben eine Anleitung zum Konfigurieren der Garbage Collector, um die richtige Balance zu erzielen (z. B. finden Sie unter [ASP.NET 2.0-bzw. 3.5-Hosting-Freigabekonfiguration](https://www.iis.net/learn/web-hosting/web-server-for-shared-hosting/aspnet-20-35-shared-hosting-configuration)).

Für .NET Framework 4.5, anstelle von Einstellungen für mehrere eigenständige, eine arbeitsauslastung definierte Konfigurationseinstellung ist verfügbar, können alle der oben genannten GC-Einstellungen als auch neue optimieren, die zusätzliche Leistung für die pro-Website bereitstellt. Workingset.

Um GC-Arbeitsspeicher, die Optimierung zu aktivieren, fügen Sie die folgende Einstellung in der Datei Windows\Microsoft.NET\Framework\v4.0.30319\aspnet.config aus:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample16.xml)]

(Wenn Sie mit der vorherigen Anleitung für die Änderungen zu aspnet.config vertraut sind, beachten Sie, dass diese Einstellung die alten Einstellungen ersetzt, z. B. besteht keine Notwendigkeit festzulegende GcServer, GcConcurrent usw. Sie müssen keinen die alten Einstellungen zu entfernen.)

<a id="_Toc_perf_6"></a>
#### <a name="prefetching-for-web-applications"></a>Der Vorabruf für Webanwendungen

**Anforderung**: .NET Framework 4.5 unter Windows 8

Für mehrere Versionen enthalten Windows verfügt über eine Technologie, bekannt als die [Vorabrufs liegt](http://en.wikipedia.org/wiki/Prefetcher) , um die Datenträger-Lese-Kosten für Start der Anwendung reduzieren. Da Kaltstart vorwiegend für Clientanwendungen ein Problem ist, ist diese Technologie nicht in Windows Server enthalten nur Komponenten enthält, die für einen Server unverzichtbar sind. Vorabruf ist jetzt verfügbar in der neuesten Version von Windows Server, in dem sie den Start der einzelnen Websites optimiert werden kann.

Für Windows Server ist die Vorabrufs liegt nicht standardmäßig aktiviert. Zum Aktivieren und konfigurieren die Vorabrufs liegt für mit hoher Dichte Webhosting, führen Sie den folgenden Satz von Befehlen in der Befehlszeile aus:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample17.cmd)]

Fügen Sie dann zum Integrieren der Vorabrufs liegt in ASP.NET-Anwendungen Folgendes in die Datei "Web.config":

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample18.xml)]

<a id="_Toc318097385"></a>
## <a name="aspnet-web-forms"></a>ASP.NET-Web Forms

<a id="_Toc318097386"></a>
### <a name="strongly-typed-data-controls"></a>Stark typisierte Datensteuerelemente

In ASP.NET 4.5 enthält Web Forms einige Verbesserungen für die Arbeit mit Daten. Die erste Verbesserung ist stark typisierte Datensteuerelemente an. Für in früheren Versionen von ASP.NET Web Forms-Steuerelemente, Sie anzeigen, einem datengebundenen Werts mithilfe *Eval* und einen XPath-Datenbindungsausdruck:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample19.aspx)]

Verwenden Sie für die bidirektionale Datenbindung, *binden*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample20.aspx)]

Diese Aufrufe mithilfe von Reflektion zur Laufzeit liest den Wert des angegebenen Elements, und klicken Sie dann das Ergebnis im Markup angezeigt. Dieser Ansatz erleichtert es zur Datenbindung für beliebige, unshaped Daten.

Allerdings unterstützen Datenbindungsausdrücke wie folgt keine Features wie IntelliSense für Elementnamen, Navigations-(z. B. "Gehe zu Definition) oder zur Kompilierzeit für diese Namen.

Um dieses Problem zu beheben, fügt ASP.NET 4.5 die Möglichkeit, den Datentyp der Daten zu deklarieren, die ein Steuerelement gebunden ist. Erreichen Sie dies mit der neuen *ItemType* Eigenschaft. Wenn Sie diese Eigenschaft festlegen, zwei neue typisierte Variablen stehen im Bereich der Datenbindungsausdrücke: *Element* und *BindItem*. Da die Variablen stark typisiert sind, erhalten Sie alle Vorteile von Visual Studio-Entwicklung zu erleichtern.


Verwenden Sie für die bidirektionale Datenbindungsausdrücke der *BindItem* Variable:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample21.aspx)]


Die meisten Steuerelemente in ASP.NET Web Forms-Framework, die die Datenbindung zu unterstützen wurden aktualisiert, um die Unterstützung der *ItemType* Eigenschaft.

<a id="_Toc318097387"></a>
### <a name="model-binding"></a>Modellbindung

Modellbindung erweitert die Datenbindung in ASP.NET Web Forms-Steuerelementen mit Zugriff auf Code fokussierter Daten arbeiten. Er enthält Konzepte aus der *"ObjectDataSource"* Steuerelement und von der modellbindung in ASP.NET MVC.

<a id="_Toc318097388"></a>
#### <a name="selecting-data"></a>Auswählen von Daten

Zum Konfigurieren eines Datensteuerelements, um die modellbindung zu verwenden, um Daten auszuwählen, die Sie festlegen des Steuerelements *SelectMethod* -Eigenschaft auf den Namen einer Methode im Code der Seite. Das Steuerelement ruft die Methode zum geeigneten Zeitpunkt im Lebenszyklus Seite und die zurückgegebenen Daten automatisch gebunden. Es ist nicht erforderlich, explizit aufrufen, die *DataBind* Methode.

Im folgenden Beispiel die *GridView* Steuerelement ist so konfiguriert, dass eine Methode mit dem Namen *GetCategories*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample22.aspx)]

Sie erstellen die *GetCategories* -Methode in der Code der Seite. Für eine einfache select-Vorgänge, die Methode keine Parameter benötigt, und sollte zurückgegeben werden ein *"IEnumerable"* oder *"IQueryable"* Objekt. Wenn die neue *ItemType* festgelegt wird (wodurch Datenbindungsausdrücke, stark typisiert wie unter erläutert [stark typisierte Datensteuerelemente](#_Toc318097386) weiter oben), die generischen Versionen dieser Schnittstellen zurückgegeben werden sollen – *"IEnumerable"&lt;T&gt;*  oder *"IQueryable"&lt;T&gt;*, mit der *T* Parameter, die den Typ der Übereinstimmung der *ItemType* Eigenschaft (z. B. *"IQueryable"&lt;Kategorie&gt;*).

Das folgende Beispiel zeigt den Code für eine *GetCategories* Methode. In diesem Beispiel verwendet das Entity Framework Code First-Modell, mit der Northwind-Beispieldatenbank. Der Code stellt sicher, dass die Abfrage, die zugehörigen Produkte für die einzelnen Kategorien mit Details zurückgibt der *Include* Methode. (Dadurch wird sichergestellt, dass die *TemplateField* Element im Markup zeigt die Anzahl der Produkte in jeder Kategorie an, ohne dass ein [n + 1 wählen](http://stackoverflow.com/questions/97197/what-is-the-n1-selects-problem).)

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample23.cs)]

Wenn die Seite ausgeführt wird, wird die *GridView* kontrollaufrufe der *GetCategories* Methode automatisch auf und rendert die zurückgegebenen Daten mithilfe der konfigurierten Felder:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.png)

Daran, dass die select-Methode zurückgibt. eine *"IQueryable"* -Objekt, das *GridView* Steuerelement kann die Abfrage weiter bearbeiten, vor der Ausführung. Z. B. die *GridView* -Steuerelements können Abfrageausdrücke zum Sortieren und paging auf das zurückgegebene hinzufügen *"IQueryable"* Objekt, bevor er ausgeführt wird, damit, dass diese Vorgänge vom zugrunde liegenden ausgeführt werden LINQ-Anbieter. In diesem Fall wird die Entity Framework sichergestellt, dass diese Vorgänge in der Datenbank ausgeführt werden.

Das folgende Beispiel zeigt die *GridView* Steuerelement geändert wird, um zuzulassen, Sortieren und paging:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample24.aspx)]

Wenn die Seite ausgeführt wird, kann das Steuerelement nun sicher, dass nur der aktuellen Datenseite angezeigt wird und sie nach der ausgewählten Spalte sortiert wird vornehmen:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.png)

Um die zurückgegebenen Daten zu filtern, verfügen über die Parameter der select-Methode hinzugefügt werden. Dieser Parameter werden durch die modellbindung zur Laufzeit ausgefüllt und können Sie Sie verwenden, um die Abfrage vor dem Zurückgeben der Daten zu ändern.

Nehmen wir beispielsweise an, dass Sie Benutzer Filter-Produkte zu lassen, indem Sie ein Schlüsselwort in der Abfragezeichenfolge eingeben möchten. Sie können Hinzufügen eines Parameters der Methode und aktualisieren Sie den Code, um den Parameterwert zu verwenden:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample25.cs)]

Dieser Code umfasst eine *, in denen* Ausdruck, wenn kein Wert angegeben ist, für die *Schlüsselwort* und gibt dann die Ergebnisse der Abfrage zurück.

<a id="_Toc318097389"></a>
#### <a name="value-providers"></a>Wertanbieter

Im vorherige Beispiel war nicht dazu, wo der Wert für die *Schlüsselwort* -Parameter stammen. Um diese Informationen anzugeben, können Sie Parameterattribute verwenden. In diesem Beispiel können Sie die *QueryStringAttribute* -Klasse, die in der *System.Web.ModelBinding* Namespace:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample26.cs)]

Dies weist die modellbindung, um zu versuchen, einen Wert aus der Abfragezeichenfolge zum Binden der *Schlüsselwort* Parameter zur Laufzeit. (Dabei wird z. B. ausführen typkonvertierung, obwohl dies in diesem Fall nicht der Fall.) Wenn ein Wert kann nicht angegeben werden, und der Typ NULL-Werte zulässt ist, wird eine Ausnahme ausgelöst.

Die Quellen der Werte für diese Methoden werden als Wertanbieter bezeichnet, und die Parameterattribute, die angeben, welcher Wertanbieter verwendet werden als Anbieter Wertattribute bezeichnet. Web Forms enthalten Wertanbieter und die zugehörigen Attribute für alle typischen Quellen von Benutzereingaben in Web Forms-Anwendungen, z. B. der Abfragezeichenfolgen, Cookies, Formularwerte, Steuerelemente, Ansichtszustand, Sitzungsstatus und Profileigenschaften. Sie können auch benutzerdefinierte Wertanbieter erstellen.

Standardmäßig ist der Name des Parameters als Schlüssel verwendet, um einen Wert in die wertanbietersammlung zu finden. Im Beispiel wird der Code für einen Abfragezeichenfolgen-Wert, der mit dem Namen Schlüsselwort aussehen (z. B. ~ / default.aspx?keyword=chef). Sie können einen benutzerdefinierten Schlüssel angeben, durch Übergabe als Argument an die Parameter-Attribut. Beispielsweise könnte, den Wert der Abfragezeichenfolgen-Variablen mit dem Namen f verwenden zu können, Sie so vorgehen:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample27.cs)]

Wenn diese Methode im Code der Seite ist, können Benutzer die Ergebnisse filtern, ein Schlüsselwort, das mithilfe der Abfragezeichenfolge übergeben:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.png)

Modellbindung erreicht, viele Aufgaben, die Sie ansonsten manuell codieren müssen: Lesen des Werts, einen null-Wert gesucht, es wird versucht, die sie in den entsprechenden Typ zu konvertieren, überprüfen, ob die Konvertierung erfolgreich war und schließlich können Sie mit dem Wert in der Abfrage. Das Modell Binden von Ergebnissen in sehr viel weniger Code und die Möglichkeit, die die Funktionalität in der gesamten Anwendung wiederverwenden.

<a id="_Toc318097390"></a>
#### <a name="filtering-by-values-from-a-control"></a>Filtern nach Werten von einem Steuerelement

Nehmen wir an, dass Sie das Beispiel, um der Benutzer einen Filterwert aus einer Dropdownliste auswählen kann erweitern möchten. Das Markup der folgenden Dropdown-Liste hinzu, und konfigurieren Sie ihn zum Abrufen von Daten aus einer anderen Methode mithilfe der *SelectMethod* Eigenschaft:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample28.aspx)]

In der Regel würden Sie auch Hinzufügen einer *EmptyDataTemplate* Element, das *GridView* Steuerelement, sodass das Steuerelement eine Meldung angezeigt wird, wenn keine entsprechenden Produkte gefunden werden:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample29.aspx)]

Fügen Sie in den Code der Seite hinzu, dass die neue Methode für das Dropdown-Liste auswählen:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample30.cs)]

Aktualisieren Sie abschließend die *GetProducts* Methode, um einen neuen Parameter mit der ID der ausgewählten Kategorie aus der Dropdown-Liste auswählen:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample31.cs)]

Nachdem die Seite ausgeführt wird, Benutzern eine Kategorie aus der Dropdown-Liste ausgewählt werden können und die *GridView* Steuerelement automatisch erneut an die gefilterten Daten gebunden ist. Dies ist möglich, da die modellbindung verfolgt die Werte der Parameter für die select-Methoden und erkennt, ob jeder Parameterwert nach einem Postback geändert hat. Wenn dies der Fall ist, erzwingt die modellbindung das zugeordnete Datensteuerelement zu binden an die Daten erneut.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.png)

<a id="_Toc318097391"></a>
### <a name="html-encoded-data-binding-expressions"></a>HTML-codierte Datenbindungsausdrücke

Sie können das Ergebnis des Datenbindungsausdrücke jetzt HTML-Codierung. Fügen Sie einen Doppelpunkt (:)) bis zum Ende der &lt;% # Präfix, das die XPath-Datenbindungsausdruck markiert:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample32.aspx)]

<a id="_Toc318097392"></a>
### <a name="unobtrusive-validation"></a>Unaufdringliche Validierung

Sie können jetzt die integrierten Validator-Steuerelemente für die clientseitige Validierungslogik unaufdringliches JavaScript Verwendung konfigurieren. Dies erheblich reduziert die Menge an JavaScript Inline im Markup Seite gerendert, und verringert die Gesamtgröße der Seite. Sie können unaufdringliches JavaScript für die Validator-Steuerelemente in einem der folgenden Methoden konfigurieren:

- Global durch Hinzufügen der folgenden Einstellung aus, um die *&lt;"appSettings"&gt;* Element in der Datei "Web.config": 

    [!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample33.xml)]
- Global durch Einstellen der statischen *System.Web.UI.ValidationSettings.UnobtrusiveValidationMode* Eigenschaft *UnobtrusiveValidationMode.WebForms* (in der Regel in der *Anwendung \_Starten* -Methode in der Datei "Global.asax").
- Einzeln für eine Seite durch Festlegen der neuen *UnobtrusiveValidationMode* Eigenschaft der *Seite* -Klasse *UnobtrusiveValidationMode.WebForms*.

<a id="_Toc318097393"></a>
### <a name="html5-updates"></a>HTML5-Updates

Einige für Web Forms-Steuerelementen zu neuen Funktionen von HTML5 nutzen wurden Verbesserungen vorgenommen:

- Die *TextMode* Eigenschaft der *Textfeld* Steuerelement wurde aktualisiert, um die neuen HTML5-Eingabetypen wie unterstützen *-e-Mail*, *"DateTime"*, und So weiter.
- Die *"FileUpload"* steuern Sie jetzt mehrere Dateiuploads in Browsern mit Unterstützung für diese HTML5-Funktion unterstützt.
- Validierungssteuerelement steuert nun Unterstützung überprüfen HTML5 input-Elemente.
- Neuer HTML5-Elemente mit Attributen, die eine URL jetzt darstellen unterstützen Runat = "Server". Daher können Sie ASP.NET Konventionen in der URL-Pfade, z. B. der ~-Operator zum Stammverzeichnis der Anwendung darstellen (z. B. &lt;video Runat = "Server" src="~/myVideo.wmv" /&gt;).
- Die *UpdatePanel* Steuerelement zur Unterstützung von HTML5-Posting Eingabefelder behoben wurde.

<a id="_Toc318097394"></a>
## <a name="aspnet-mvc-4"></a>ASP.NET MVC 4

ASP.NET MVC 4 Beta ist jetzt in Visual Studio 11 Beta enthalten. ASP.NET MVC ist ein Framework zum Entwickeln einfach zu testender und verwaltbarer Webanwendungen durch Nutzung der Model-View-Controller (MVC)-Muster. ASP.NET MVC 4 erleichtert Ihnen die Erstellung von Anwendungen für mobile Web und enthält die ASP.NET Web API, mit dem Sie HTTP-Dienste erstellen, die jedem Gerät erreichen können. Weitere Informationen finden Sie unter den [ASP.NET MVC 4 – Anmerkungen zu dieser](mvc4-release-notes.md).

<a id="_Toc318097395"></a>
## <a name="aspnet-web-pages-2"></a>ASP.NET-Webseiten 2

Die folgenden: neuen features

- Neue und aktualisierte Websitevorlagen.
- Hinzufügen von serverseitige und clientseitige Validierung mithilfe der *Überprüfung* Helper.
- Die Möglichkeit, Skripts, die über einen Ressourcen-Manager zu registrieren.
- Anmeldungen von Facebook und andere Standorte mithilfe von OAuth und OpenID wird aktiviert.
- Hinzufügen von maps unter Verwendung der *ordnet* Helper.
- Ausführen von Webseiten Anwendungen Seite-an-Seite.
- Rendern von Seiten für mobile Geräte.

Weitere Informationen zu diesen Features und ganzseitige Codebeispiele finden Sie unter [die wichtigsten Funktionen in Web Pages 2 Beta](https://go.microsoft.com/fwlink/?LinkID=227824).

<a id="_Toc318097396"></a>
## <a name="visual-web-developer-11-beta"></a>Visual Web Developer 11 Beta

Dieser Abschnitt enthält Informationen zu den Verbesserungen für die Webentwicklung in Visual Web Developer 11 Beta und Visual Studio 2012 Release Candidate.

<a id="project-compatibility"></a>
### <a name="project-sharing-between-visual-studio-2010-and-visual-studio-2012-release-candidate-project-compatibility"></a>Projektfreigabe, die zwischen Visual Studio 2010 und Visual Studio 2012 Release Candidate (Projektkompatibilität)

Öffnen ein vorhandenes Projekt in einer neueren Version von Visual Studio bis Visual Studio 2012 Release Candidate gestartet des Konvertierungs-Assistenten aus. Dies erzwungen ein Upgrade des Inhalts (Objekte) ein Projekt und Projektmappe in neue Formate, die nicht abwärts kompatibel waren. Aus diesem Grund konnte nach der Konvertierung Sie nicht das Projekt in der früheren Version von Visual Studio geöffnet werden.

Viele Kunden haben uns mitgeteilt, dass dies nicht der richtige Ansatz war. In Visual Studio 11 Beta unterstützen wir jetzt Freigabe Projekte und Projektmappen mit Visual Studio 2010 SP1. Dies bedeutet, dass wenn Sie ein 2010-Projekt in Visual Studio 2012 Release Candidate öffnen, Sie immer noch um das Projekt in Visual Studio 2010 SP1 öffnen können.

> [!NOTE]
> Einige Projekttypen können nicht zwischen Visual Studio 2010 SP1 und Visual Studio 2012 Release Candidate freigegeben werden. Dazu gehören einige ältere Projekte (z. B. ASP.NET MVC 2-Projekte) oder die Projekte für besondere Zwecke (z. B. der Setup-Projekte).

Wenn Sie ein Visual Studio 2010 SP1-Web-Projekt zum ersten Mal in Visual Studio 11 Beta öffnen, werden die folgenden Eigenschaften zur Projektdatei hinzugefügt:

- FileUpgradeFlags
- UpgradeBackupLocation
- OldToolsVersion
- VisualStudioVersion
- VSToolsPath

FileUpgradeFlags UpgradeBackupLocation und OldToolsVersion werden vom Prozess verwendet, die die Projektdatei wird aktualisiert. Sie haben keine Auswirkungen auf das Projekt in Visual Studio 2010 funktioniert.

VisualStudioVersion ist eine neue Eigenschaft ein, die MSBuild 4.5, der die Version von Visual Studio für das aktuelle Projekt angibt. Da diese Eigenschaft in MSBuild 4.0 (die Version von MSBuild, die Visual Studio 2010 SP1 verwendet) vorhanden war, können wir einen Standardwert in der Projektdatei einfügen.

Die VSToolsPath-Eigenschaft wird verwendet, um zu bestimmen, die richtige TARGETS-Datei zum Importieren aus dem Pfad, der durch die Einstellung MSBuildExtensionsPath32 dargestellt wird.

Es gibt auch einige Änderungen im Zusammenhang mit Import-Elemente. Diese Änderungen sind erforderlich, um die Kompatibilität zwischen den beiden Versionen von Visual Studio zu unterstützen.

> [!NOTE]
> Wenn ein Projekt zwischen Visual Studio 2010 SP1 und Visual Studio 11 Beta auf zwei verschiedenen Computern freigegeben wird und das Projekt verfügt über eine lokale Datenbank in der App\_Datenordner, achten Sie darauf, dass die Version der Datenbank verwendete SQL Server auf beiden Computern installiert.

<a id="Configuration_Changes_In_ASPNET45_Website_Templates"></a>
### <a name="configuration-changes-in-aspnet-45-website-templates"></a>Änderungen an der Konfiguration in ASP.NET 4.5-Websitevorlagen

Die folgenden Änderungen wurden auf den Standardwert *"Web.config"* -Datei für die Website, die mithilfe von Websitevorlagen in Visual Studio 2012 Release Candidate erstellt werden:

- In der `<httpRuntime>` -Element, das `encoderType` Attribut ist jetzt standardmäßig so eingestellt, verwenden Sie die AntiXSS-Typen, die ASP.NET hinzugefügt wurden. Weitere Informationen finden Sie unter [AntiXSS-Bibliothek](#_Toc318097382).
- Auch in der `<httpRuntime>` -Element, das `requestValidationMode` "4.5"-Attribut festgelegt ist. Dies bedeutet, dass in der Standardeinstellung Anforderungsvalidierung für die Verwendung von verzögerten ("lazy") Validierung konfiguriert ist. Weitere Informationen finden Sie unter [neue ASP.NET anfordern Validierungsfunktionen](#_Toc318097379).
- Die `<modules>` Element der `<system.webServer>` Abschnitt jedoch nicht enthalten eine `runAllManagedModulesForAllRequests` Attribut. (Der Standardwert ist "false".) Dies bedeutet, dass wenn Sie eine Version von IIS 7 verwenden, die nicht auf SP1 aktualisiert wurde, können Sie Probleme beim routing in einen neuen Standort möglicherweise haben. Weitere Informationen finden Sie unter [systemeigene Unterstützung in IIS 7 für das ASP.NET-Routing](#Native_Support_In_IIS7_For_ASPNET_Routine).

Diese Änderungen wirken sich nicht auf vorhandene Anwendungen aus. Allerdings können sie einen Unterschied im Verhalten zwischen vorhandenen Websites und neuen Websites, die Sie für ASP.NET 4.5 mit den neuen Vorlagen erstellen darstellen.

<a id="Native_Support_In_IIS7_For_ASPNET_Routine"></a>
### <a name="native-support-in-iis-7-for-aspnet-routing"></a>Systemeigene Unterstützung in IIS 7 für das ASP.NET-Routing

Dies ist nicht als solche eine Änderung an ASP.NET und nur eine Änderung in den Vorlagen für neue Websiteprojekte, die Sie betreffen können, wenn Sie eine Version von IIS 7 arbeiten, die nicht über das SP1-Update angewendet wurde.

In ASP.NET können Sie Anwendungen, um das routing zu unterstützen die folgende Konfigurationseinstellung hinzufügen:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample34.xml?highlight=3)]

Wenn **RunAllManagedModulesForAllRequests** ist "true" wird eine URL wie `http://mysite/myapp/home` wird an ASP.NET, auch wenn es keine *aspx*, *MVC*, oder eine ähnliche Erweiterung auf der URL.

Stellt ein Update, das IIS 7 wurde die **RunAllManagedModulesForAllRequests** unnötige festlegen und ASP.NET-ROUTING nativ unterstützt. (Informationen zum Update finden Sie auf der Microsoft Support-Artikel [ist ein Update verfügbar ist, können Sie bestimmte IIS 7.0 oder IIS 7.5-Handler behandelt Anforderungen, deren URLs nicht mit einem Punkt enden](https://support.microsoft.com/kb/980368).)

Wenn Ihre Website auf IIS 7 ausgeführt wird und wenn IIS aktualisiert wurde, Sie nicht festlegen müssen **RunAllManagedModulesForAllRequests** auf "true". In der Tat ist auf True setzen nicht empfohlen, da unnötigen Verarbeitungsaufwand auf Anforderung hinzugefügt. Wenn diese Einstellung "true" ist, alle Anforderungen, einschließlich derjenigen für *.htm*, *jpg*, und andere statische Dateien, auch der ASP.NET-Anforderungspipeline.

Wenn Sie eine neue ASP.NET 4.5-Website unter Verwendung der Vorlagen, die in Visual Studio 2012 RC bereitgestellt werden erstellen, die Konfiguration für die Website umfasst nicht die **RunAllManagedModulesForAllRequests** festlegen. Dies bedeutet, dass standardmäßig die Einstellung "false".

Wenn Sie dann die Website unter Windows 7 ohne SP1 ausführen, wird das erforderliche Update von IIS 7 nicht enthalten. Infolgedessen wird routing funktioniert nicht und werden Fehler angezeigt. Wenn Sie ein Problem haben, in dem routing funktioniert nicht, können Sie entweder die folgenden Aktionen ausführen:

- Aktualisieren Sie Windows 7 auf SP1, wodurch das Update auf IIS 7 hinzugefügt wird.
- Installieren Sie das Update, das beschrieben ist, in der Microsoft Support-Artikel, die zuvor aufgeführten.
- Legen Sie **RunAllManagedModulesForAllRequests** auf "true" in dieser Website die Web.config-Datei. Beachten Sie, dass dies zusätzlichen Aufwand Anforderungen hinzufügen möchten.

<a id="_Toc318097397"></a>
### <a name="html-editor"></a>HTML-Editor

<a id="_Toc318097398"></a>
#### <a name="smart-tasks"></a>Smarttasks

In der Entwurfsansicht haben komplexe Eigenschaften von Serversteuerelementen oft Dialogfelder und Assistenten, um deren Festlegung erleichtern verknüpft. Beispielsweise können ein spezieller Dialogfeld zum Hinzufügen einer Datenquelle zu einem *Repeater* steuern oder Spalten hinzufügen eine *GridView* Steuerelement.

Diese Art von Hilfe zur Benutzeroberfläche für komplexe Eigenschaften sind jedoch nicht in der Quellansicht verfügbar wurde. Aus diesem Grund führt Visual Studio 11 Smarttasks, für die Datenquellensicht an. Smarttasks sind kontextabhängige Standardtastenkombinationen für häufig verwendete Funktionen in den C#- und Visual Basic-Editoren.

Für ASP.NET Web Forms-Steuerelemente angezeigt werden Smarttasks auf Server-Tags als kleines Symbol, wenn die Einfügemarke innerhalb des Elements befindet:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.png)

Die intelligente Aufgabe wird erweitert, wenn Sie auf das Symbol, oder drücken Sie STRG +. (Punkt), wie die Code-Editoren. Es zeigt Verknüpfungen, die in der Entwurfsansicht die intelligente Tasks ähnlich sind.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image7.png)

Die intelligente Aufgabe in der vorherigen Abbildung zeigt beispielsweise die GridView-Aufgaben-Optionen. Wenn Sie auf "Spalten bearbeiten" auswählen, wird das folgende Dialogfeld angezeigt:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image8.png)

Das Dialogfeld legt die gleichen Eigenschaften eingeben, können Sie in der Entwurfsansicht festlegen. Wenn Sie auf "OK" klicken, wird das Markup für das Steuerelement mit den neuen Einstellungen aktualisiert:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image9.png)

<a id="_Toc318097399"></a>
#### <a name="wai-aria-support"></a>WAI-ARIA-Unterstützung

Schreiben von zugänglich Websites wird zunehmend wichtiger. Die [WAI-ARIA Barrierefreiheit standard](http://www.w3.org/WAI/intro/aria) definiert, wie Entwickler zugänglich Websites schreiben soll. Dieser Standard ist jetzt vollständig in Visual Studio unterstützt.

Z. B. die *Rolle* Attribut verfügt jetzt über die vollständige IntelliSense-Funktionalität:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image10.png)

Die WAI-ARIA-Standard führt außerdem Attribute, die mit dem Präfix *Aria -* , mit denen Sie die Semantik einer HTML5-Dokument hinzufügen. Visual Studio unterstützt diese auch vollständig *Aria -* Attribute:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image11.png) ![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image12.png)

<a id="_Toc318097400"></a>
#### <a name="new-html5-snippets"></a>Neuer HTML5-Ausschnitte

Damit schneller und einfacher zu häufig verwendeten HTML5-Markup schreiben können, enthält Visual Studio eine Anzahl von Codeausschnitten. Ein Beispiel ist der video-Codeausschnitt:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image13.png)

Um den Ausschnitt aufzurufen, drücken Sie Tab, zweimal, wenn das Element in IntelliSense ausgewählt ist:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image14.png)

Dies erzeugt einen Ausschnitt aus, dem Sie anpassen können.

<a id="_Toc318097401"></a>
#### <a name="extract-to-user-control"></a>In Benutzersteuerelement extrahieren

In großen Webseiten kann es eine gute Idee, einzelne Teile in Benutzersteuerelemente verschoben sein. Diese Form der Umgestaltung kann die Lesbarkeit der Seite zu verbessern und kann die Seitenstruktur vereinfachen.

Um dies einfacher, wenn Sie Web Forms-Seiten in der Quellansicht bearbeiten, können Sie jetzt Text auf einer Seite auswählen, der rechten Maustaste darauf, und wählen die Extract Benutzersteuerelement:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.jpg)

<a id="_Toc318097402"></a>
#### <a name="intellisense-for-code-nuggets-in-attributes"></a>IntelliSense für codenuggets in Attributen

Visual Studio bietet IntelliSense für serverseitige codenuggets in einer beliebigen Seite oder Steuerelement. Visual Studio enthält jetzt IntelliSense für codenuggets in HTML-Attributen sowie an.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image15.png)

Dies erleichtert die Erstellung von Datenbindungsausdrücken:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image16.png)

<a id="_Toc318097403"></a>
#### <a name="automatic-renaming-of-matching-tag-when-you-rename-an-opening-or-closing-tag"></a>Automatisches Umbenennen des übereinstimmenden Tags aus, wenn Sie ein Start- oder Endtags umbenennen

Wenn Sie ein HTML-Element umbenennen (beispielsweise ändern Sie eine *Div* Tag werden eine *Header* Tag), wird die entsprechende öffnen oder schließendes Tag ändert sich ebenfalls in Echtzeit.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image17.png)

Dadurch werden die Fehler zu vermeiden, in dem Sie vergessen, die einen schließendes Tag ändern oder die falsche Version.

<a id="_Toc318097404"></a>
#### <a name="event-handler-generation"></a>Generieren von Ereignishandlern

Visual Studio bietet nun Features in der Quellansicht können Sie Ereignishandler schreiben, und binden diese manuell. Wenn Sie einen Ereignisnamen in der Quellansicht bearbeiten, zeigt IntelliSense &lt;neues Ereignis erstellen&gt;, der einen Ereignishandler im Code der Seite erstellen werden, die über die richtige Signatur:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.jpg)

Standardmäßig wird der Ereignishandler die ID des Steuerelements für den Namen der Methode zur Verarbeitung von Ereignissen verwendet:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.jpg)

Der resultierende-Ereignishandler wird (in diesem Fall in C# -Referenz) wie folgt aussehen:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image18.png)

<a id="_Toc318097405"></a>
#### <a name="smart-indent"></a>Intelligenter Einzug

Beim Drücken der EINGABETASTE, klicken Sie in ein leeres HTML-Element wird der Editor die Einfügemarke am richtigen Ort platzieren:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image19.png)

Wenn durch Drücken der EINGABETASTE an dieser Stelle ist das schließende Tag nach unten verschoben und entsprechend das Starttag eingerückt. Die Einfügemarke wird auch eingerückt:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image20.png)

<a id="_Toc318097406"></a>
#### <a name="auto-reduce-statement-completion"></a>Automatisch eingrenzende Anweisungsvervollständigung

Die IntelliSense-Liste in Visual Studio jetzt Filtern basierend auf was Sie eingeben, damit nur die relevanten Optionen angezeigt:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image21.png)

IntelliSense auch Filter auf die große Anfangsbuchstaben der einzelnen Wörter in der IntelliSense-Liste basieren. Wenn Sie "dl" eingeben, werden z. B. sowohl "dl" und "Asp: DataList angezeigt:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image22.png)

Dieses Feature ermöglicht es schneller um Anweisungsvervollständigung für bekannte Elemente zu erhalten.

<a id="_Toc318097407"></a>
### <a name="javascript-editor"></a>JavaScript-Editor

Der JavaScript-Editor in Visual Studio 2012 Release Candidate ist vollständig neu, und die Erfahrung mit JavaScript in Visual Studio erheblich verbessert.

<a id="_Toc318097408"></a>
#### <a name="code-outlining"></a>Code outlining

Gliederungsbereiche werden jetzt automatisch erstellt, für alle Funktionen, sodass Sie Teile der Datei zu reduzieren, die nicht für den aktuellen Fokus relevant sind.

<a id="_Toc318097409"></a>
#### <a name="brace-matching"></a>Überprüfung des Klammergleichgewichts

Wenn Sie die Einfügemarke auf eine öffnende oder schließende geschweifte Klammer einfügen, werden in der Editor mit einem hervorgehoben.

<a id="_Toc318097410"></a>
#### <a name="go-to-definition"></a>Zur Definition wechseln

Gehe zu Definition (Befehl) können Sie das Springen an die Quelle für eine Funktion oder Variable.

<a id="_Toc318097411"></a>
#### <a name="ecmascript5-support"></a>Unterstützung von ECMAScript5

Der Editor unterstützt die neue Syntax und APIs in ECMAScript5, die neueste Version des Standards, die JavaScript-Sprache beschreibt.

<a id="_Toc318097412"></a>
#### <a name="dom-intellisense"></a>DOM-IntelliSense

IntelliSense für DOM-APIs wurde verbessert, mit Unterstützung für viele neue HTML5-APIs, darunter *QuerySelector*, DOM-Speicher, dokumentübergreifendes messaging, und *canvas*. DOM-IntelliSense wird jetzt von einer einfachen JavaScript-Datei und nicht durch eine native Bibliothek Typdefinition gesteuert. Dies erleichtert das Erweitern oder ersetzen.

<a id="_Toc318097413"></a>
#### <a name="vsdoc-signature-overloads"></a>Überladungen der VSDOC-Signatur

Detaillierte IntelliSense-Kommentare können jetzt für separate Überladungen von JavaScript-Funktionen deklariert werden, mit dem neuen *&lt;Signatur&gt;* Element, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample35.cs)]

<a id="_Toc318097414"></a>
#### <a name="implicit-references"></a>Implizite Verweise

Sie können nun JavaScript-Dateien an eine zentrale Liste hinzufügen, die implizit in der Liste der Dateien einbezogen werden, dass alle angegebenen JavaScript-Datei oder der Block verweisen, das heißt, Sie IntelliSense für deren Inhalte erhalten. Beispielsweise können Sie die jQuery-Dateien hinzufügen, um die zentrale Liste mit Dateien, und erhalten Sie IntelliSense für jQuery-Funktionen in jedem JavaScript-Block der Datei, ob Sie es explizit darauf verwiesen haben (mit / / / &lt;Verweis /&gt;) oder nicht.

<a id="_Toc318097415"></a>
### <a name="css-editor"></a>CSS-Editor

<a id="_Toc318097416"></a>
#### <a name="auto-reduce-statement-completion"></a>Automatisch eingrenzende Anweisungsvervollständigung

Die IntelliSense-Liste für CSS-jetzt Filtern auf Grundlage der CSS-Eigenschaften und Werte, die durch das ausgewählte Schema unterstützt.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image23.png)

IntelliSense unterstützt auch die Groß-/Kleinschreibung Suchvorgänge Titel:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image24.png)

<a id="_Toc318097417"></a>
#### <a name="hierarchical-indentation"></a>Hierarchischer Einzug

CSS-Editor verwendet, Einzug zum Anzeigen von hierarchischer Regeln, die Sie erhalten einen Überblick darüber, wie die kaskadierenden Regeln logisch organisiert sind. Im folgenden Beispiel die #list ein Selektor ein cascading untergeordnetes Element der Liste und ist daher eingerückt.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image25.png)

Das folgende Beispiel zeigt eine komplexere Vererbung:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image26.png)

Der Einzug der Regel richtet sich nach ihren übergeordneten Regeln. Hierarchischer Einzug ist standardmäßig aktiviert, aber Sie können es im Dialogfeld Optionen (Extras, Optionen auf der Menüleiste) deaktivieren:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image27.png)

<a id="_Toc318097418"></a>
#### <a name="css-hacks-support"></a>Unterstützung von CSS-weichen

Analyse von Hunderten von realen CSS-Dateien zeigt, dass der CSS-weichen sind sehr verbreitet, und Visual Studio unterstützt jetzt die am häufigsten verwendeten werden. Diese Unterstützung umfasst, IntelliSense und Validierung des Sterns (\*) und Unterstriche (\_) Bearbeitung der Eigenschaft:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image28.png)

Typische Auswahl Hacks werden ebenfalls unterstützt, sodass hierarchischer Einzug beibehalten wird, auch wenn sie angewendet werden. Eine typische Selector-Hack in Internet Explorer 7 als Ziel verwendet wird, stellen Sie eine Auswahl mit  *\*: First-Child + html*. Mit dieser Regel wird die hierarchischen Einzug verwalten:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image29.png)

<a id="_Toc318097419"></a>
#### <a name="vendor-specific-schemas--moz---webkit"></a>Hersteller bestimmte Schemas (' - Moz - Webkit-)

CSS3 führt viele Eigenschaften, die von verschiedenen Browsern zu unterschiedlichen Zeiten implementiert wurden. Dadurch mussten Entwickler Code für bestimmte Browser zuvor, mit herstellerspezifischen-Syntax. Diese Browser-spezifischen Eigenschaften sind jetzt in IntelliSense enthalten.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image30.png)

<a id="_Toc318097420"></a>
#### <a name="commenting-and-uncommenting-support"></a>Auskommentieren und Aufheben der auskommentierung-Unterstützung

Sie können jetzt kommentieren, und heben Sie die auskommentierung CSS-Regeln, die über die gleichen Tastenkombinationen, die Sie im Code-Editor (STRG + K, C, um den Kommentar und STRG + K, Sie die auskommentierung) verwenden.

<a id="_Toc318097421"></a>
#### <a name="color-picker"></a>Farbauswahl

In früheren Versionen von Visual Studio bestand aus IntelliSense für Farbe-bezogene Attribute ein Dropdown-Werteliste benannte Farbe. Diese Liste wurde durch ein mit vollem Funktionsumfang Farbauswahlfenster ersetzt wurde.

Wenn Sie einen Farbwert eingeben, wird die Farbauswahl wird automatisch eingeblendet, und zeigt eine Liste der zuvor verwendeten Farben, gefolgt von einer Standardfarbpalette. Sie können eine Farbe, die mit der Maus oder Tastatur auswählen.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image31.png)

Die Liste kann in eine vollständige Farbauswahl erweitert werden. Die Auswahl können Sie den alpha-Kanal steuern, indem eine beliebige Farbe automatisch in RGBA konvertiert wird, wenn Sie den Schieberegler für die Deckkraft verschieben:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image32.png)

<a id="_Toc318097422"></a>
#### <a name="snippets"></a>Codeausschnitte

Codeausschnitte in der CSS-Editor machen es einfacher und schneller zum Erstellen von browserübergreifenden Stilen. Jetzt haben viele CSS3-Eigenschaften, die Browser-spezifischen Einstellungen erfordern in Ausschnitten ausgeführt.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image33.png)

CSS-Ausschnitte unterstützt erweiterte Szenarien (z. B. CSS3-Medienabfragen) Geben Sie die "at"-Symbol (@), zeigt die IntelliSense-Liste.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image34.png)

Bei der Auswahl @media Wert mit der Tab-Taste drücken, und der CSS-Editor fügt des folgenden Codeausschnitts:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.jpg)

Wie bei der Ausschnitte für Code, können Sie Ihre eigenen CSS-Ausschnitte erstellen.

<a id="_Toc318097423"></a>
#### <a name="custom-regions"></a>Benutzerdefinierte Bereiche

Mit dem Namen Codebereiche, die bereits im Code-Editor verfügbar sind, stehen nun aus, für die Bearbeitung von CSS. Dadurch können Sie die entsprechenden Stilblöcken einfachen gruppieren.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image35.png)

Wenn ein Bereich reduziert wird wird der Name des Bereichs angezeigt:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image36.png)

<a id="_Toc318097424"></a>
### <a name="page-inspector"></a>Seitenprüfung

Seitenprüfung ist ein Tool, rendert eine Webseite (HTML, Web Forms, ASP.NET MVC oder Web Pages) in der Visual Studio-IDE, und Sie sowohl den Quellcode und die resultierende Ausgabe untersuchen kann. Bei ASP.NET-Seiten können der Seitenprüfung zu bestimmen, welche serverseitigem Code das HTML-Markup erzeugt hat, das an den Browser gerendert wird.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image37.png)

Weitere Informationen zu der Seitenprüfung finden Sie in den folgenden Tutorials:

- Verwenden der Seitenprüfung in [ASP.NET MVC](../mvc/overview/views/using-page-inspector-in-aspnet-mvc.md)
- Verwenden der Seitenprüfung in [ASP.NET-Web Forms](../web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project.md)

<a id="_Toc318097425"></a>
### <a name="publishing"></a>Veröffentlichen

<a id="_Toc318097426"></a>
#### <a name="publish-profiles"></a>Veröffentlichungsprofile

In Visual Studio 2010 Veröffentlichung von Informationen für Webanwendungsprojekte werden nicht in der Versionskontrolle gespeichert und dient nicht für andere Benutzer freigeben. In Visual Studio 2012 Release Candidate wurde das Format des Veröffentlichungsprofils geändert. Es wurde ein Team-Element, und nun ist es einfach, Builds, die auf MSBuild basierende zu nutzen. Build-Konfigurationsinformationen ist im Dialogfeld "Veröffentlichen", damit Sie die Konfigurationen vor der Veröffentlichung problemlos wechseln können.

Veröffentlichen von Profilen werden im Ordner "PublishProfiles" gespeichert. Der Speicherort des Ordners hängt Programmiersprache, die Sie verwenden:

- C#: Properties\PublishProfiles
- Visual Basic: Meine Project\PublishProfiles

Jedes Profil ist eine MSBuild-Datei. Während der Veröffentlichung wird diese Datei in die Projektdatei MSBuild importiert. Visual Studio 2010 sollten Sie den Prozess veröffentlichen oder Paket ändern müssen Sie in Ihre Anpassungen in eine Datei namens einfügen **ProjectName**. wpp.targets. Dies ist immer noch unterstützt, aber Sie können jetzt Ihre Anpassungen im Veröffentlichungsprofil selbst einfügen. Auf diese Weise werden die Anpassungen für dieses Profil nur verwendet werden.

Sie können jetzt auch nutzen Veröffentlichungsprofile von MSBuild. Zu diesem Zweck verwenden Sie den folgenden Befehl, wenn Sie das Projekt erstellen:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample36.cmd)]

Die project.csproj-Wert ist der Pfad des Projekts, und ProfileName ist der Name des Profils, das veröffentlichen. Sie können auch anstelle der Übergabe der Profilname für die *PublishProfile* -Eigenschaft, können Sie den vollständigen Pfad übergeben, um das Veröffentlichungsprofil.

<a id="_Toc318097427"></a>
#### <a name="aspnet-precompilation-and-merge"></a>ASP.NET-Vorkompilierung und merge

Für Webanwendungsprojekte fügt Visual Studio 2012 Release Candidate eine Option auf der Seite der Web packen/veröffentlichen-Eigenschaften, mit dem Sie das Vorkompilieren und zusammenführen, den Inhalt Ihrer Site beim Veröffentlichen oder Paket dem Projekt hinzu. Um diese Optionen angezeigt, mit der rechten Maustaste in des Projekts im Projektmappen-Explorer, wählen Sie Eigenschaften, und wählen Sie dann auf der Eigenschaftenseite "Web packen/veröffentlichen". Die folgende Abbildung zeigt das Vorkompilieren, diese Anwendung vor dem Veröffentlichungsoption auswählen.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.jpg)

Wenn diese Option ausgewählt ist, kompiliert Visual Studio vor, die Anwendung, sobald Sie veröffentlichen oder dem Paket die Webanwendung. Wenn Sie möchten steuern, wie die Website vorkompiliert wird oder wie Assemblys zusammengeführt werden, klicken Sie auf die Schaltfläche "Erweitert", um diese Optionen zu konfigurieren.

<a id="_Toc318097428"></a>
### <a name="iis-express"></a>IIS Express

Der Standardwebserver für Tests Webprojekte in Visual Studio ist jetzt an, IIS Express. Visual Studio Development Server ist eine Option für den lokalen Webserver während der Entwicklung, aber IIS Express ist jetzt der empfohlene Server. Die Verwendung von IIS Express in Visual Studio 11 Beta ist sehr ähnlich ist, um sie in Visual Studio 2010 SP1 verwenden.

<a id="_Toc318097429"></a>
## <a name="disclaimer"></a>Haftungsausschluss

Dies ist ein vorläufiges Dokument, das vor dem kommerziellen Release der beschriebenen Software ggf. erheblich geändert wird.

Die Informationen in diesem Dokument repräsentieren den aktuellen Standpunkt der Microsoft Corporation zu den erörterten Problemen zum Veröffentlichungstermin. Da Microsoft auf das Ändern von Marktlagen reagieren muss, ist das Dokument keinesfalls als Verpflichtung von Microsoft zu interpretieren, und Microsoft kann die Genauigkeit der Informationen nicht über den Zeitpunkt der Veröffentlichung hinaus garantieren.

Dieses Whitepaper dient ausschließlich Informationszwecken. MICROSOFT ÜBERNIMMT KEINE AUSDRÜCKLICHE, STILLSCHWEIGENDE ODER AUS GESETZ ERWACHSENDE GARANTIE IN BEZUG AUF DIE INFORMATIONEN IN DIESEM DOKUMENT.

Die Benutzer/innen sind verpflichtet, sich an alle anwendbaren Urheberrechtsgesetze zu halten. Unabhängig von der Anwendbarkeit der entsprechenden Urheberrechtsgesetze darf ohne ausdrückliche schriftliche Erlaubnis der Microsoft Corporation kein Teil dieses Dokuments für irgendwelche Zwecke vervielfältigt oder in einem Datenempfangssystem gespeichert oder darin eingelesen werden, unabhängig davon, auf welche Art und Weise oder mit welchen Mitteln (elektronisch, mechanisch, durch Fotokopieren, Aufzeichnen usw.) dies geschieht.

Microsoft Corporation kann Inhaber von Patenten oder Patentanträgen, Marken, Urheberrechten oder anderem geistigen Eigentum sein, die den Inhalt dieses Dokuments betreffen. Die Bereitstellung dieses Dokuments gewährt keinerlei Lizenzrechte an diesen Patenten, Marken, Urheberrechten oder anderem geistigen Eigentum, es sei denn, dies wurde ausdrücklich durch einen schriftlichen Lizenzvertrag mit der Microsoft Corporation vereinbart.

Sofern nicht anders angegeben, die Firmen, Organisationen, Produkte, Domänennamen, e-Mail-Adressen, Logos, Personen, Orte und Ereignisse in diesen Beispielen sind frei erfunden, und mit jeder Firmen, Unternehmen, Produkt, Domänennamen, e-Mail-Adresse Adresse ","-Logo "," Person "," Ort "oder" Ereignis richtet sich an, oder rein zufällig.

© 2012 Microsoft Corporation. Alle Rechte vorbehalten.

Microsoft und Windows sind eingetragene Marken oder Marken der Microsoft Corporation in den USA und/oder anderen Ländern.

Die in diesem Dokument erwähnten Namen von Unternehmen und Produkten können Marken der jeweiligen Eigentümer sein.
