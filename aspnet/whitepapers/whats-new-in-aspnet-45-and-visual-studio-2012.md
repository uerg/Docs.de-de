---
uid: whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
title: Neues in ASP.NET 4.5 und Visual Studio 2012 | Microsoft Docs
author: rick-anderson
description: Dieses Dokument beschreibt die neuen Funktionen und Verbesserungen, die in ASP.NET 4.5 eingeführt werden. Es beschreibt auch für die Webentwicklung erfolgten Verbesserungen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/29/2012
ms.topic: article
ms.assetid: ba1fabb4-31a3-4ebf-8327-41a6bbba6eaf
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
msc.type: content
ms.openlocfilehash: 16a2fa4b1b6617430703853543cbf29e18ba1103
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
ms.locfileid: "28886441"
---
<a name="whats-new-in-aspnet-45-and-visual-studio-2012"></a>Was ist neu in ASP.NET 4.5 und Visual Studio 2012
====================
> Dieses Dokument beschreibt die neuen Funktionen und Verbesserungen, die in ASP.NET 4.5 eingeführt werden. Es beschreibt auch Verbesserungen für die Webentwicklung in Visual Studio 2012 vorgenommen wird. Dieses Dokument wurde ursprünglich auf dem 29. Februar 2012 veröffentlicht.


- [ASP.NET Core-Laufzeit und Framework](#_Toc318097372)

    - [Asynchrones Lesen und Schreiben von HTTP-Anforderungen und Antworten](#_Toc318097373)
    - [Verbesserungen bei der Behandlung von HttpRequest](#_Toc318097374)
    - [Eine Antwort leeren asynchron](#_Toc318097375)
    - [Unterstützung für *"await"* und *Aufgabe*-basierte asynchrone Modules and Handlers](#_Toc318097376)
    - [Asynchrone HTTP-Module](#_Toc318097377)
    - [Asynchrone HTTP-Handler](#_Toc318097378)
    - [Neue Funktionen von ASP.NET Anforderung Überprüfung](#_Toc318097379)
    - [Verzögerte Anforderungsvalidierung ("lazy")](#_Toc318097380)
    - [Unterstützung für nicht überprüfte Anforderungen](#_Toc318097381)
    - [AntiXSS-Bibliothek](#_Toc318097382)
    - [Unterstützung für WebSockets-Protokoll](#_Toc318097383)
    - [Bündelung und Minimierung](#_Toc318097384)
    - [Verbesserte Leistung beim Webhosting](#_Toc_perf)

        - [Wichtige Leistungsfaktoren](#_Toc_perf_1)
        - [Anforderungen für die neuen Leistungsfunktionen](#_Toc_perf_2)
        - [Freigeben von gemeinsamen Assemblys](#_Toc_perf_3)
        - [Verwenden von Multikern-JIT-Kompilierung für schnellere-Starts](#_Toc_perf_4)
        - [Optimieren der Garbagecollection für den Speicher zu optimieren.](#_Toc_perf_5)
        - [Vorabruf für Webanwendungen](#_Toc_perf_6)
- [ASP.NET Web Forms](#_Toc318097385)

    - [Stark typisierte Datensteuerelemente](#_Toc318097386)
    - [Modellbindung](#_Toc318097387)

        - [Auswählen von Daten](#_Toc318097388)
        - [Wertanbieter](#_Toc318097389)
        - [Filtern nach Werten aus einem Steuerelement](#_Toc318097390)
    - [HTML-codierte Datenbindungsausdrücke](#_Toc318097391)
    - [Unaufdringlichen Überprüfung](#_Toc318097392)
    - [Updates HTML5](#_Toc318097393)
- [ASP.NET MVC 4](#_Toc318097394)
- [ASP.NET Web Pages 2](#_Toc318097395)
- [Visual Studio 2012 Release Candidate](#_Toc318097396)

    - [Projekt freigeben zwischen Visual Studio 2010 und Visual Studio 2012 Release Candidate (Projektkompatibilität)](#project-compatibility)
    - [Änderungen an der Konfiguration in ASP.NET 4.5 Websitevorlagen](#Configuration_Changes_In_ASPNET45_Website_Templates)
    - [Systemeigene Unterstützung in IIS 7 für das ASP.NET-Routing](#Native_Support_In_IIS7_For_ASPNET_Routine)
    - [HTML-Editor](#_Toc318097397)

        - [Intelligente Aufgaben](#_Toc318097398)
        - [WAI ARIA-Unterstützung](#_Toc318097399)
        - [Neue HTML5-Ausschnitte](#_Toc318097400)
        - [In Benutzersteuerelement extrahieren](#_Toc318097401)
        - [IntelliSense für Code Brocken in Attributen](#_Toc318097402)
        - [Automatische Umbenennung von übereinstimmenden Tag, wenn Sie ein Start- oder Endtag umbenennen](#_Toc318097403)
        - [Handler-Generierung von Ereignissen](#_Toc318097404)
        - [Intelligenten Einzug](#_Toc318097405)
        - [Anweisungsvervollständigung Auto / reduzieren](#_Toc318097406)
    - [JavaScript Editor](#_Toc318097407)

        - [Gliedern von Code](#_Toc318097408)
        - [Zugehörige Klammer](#_Toc318097409)
        - [Gehe zu Definition](#_Toc318097410)
        - [ECMAScript5-Unterstützung](#_Toc318097411)
        - [DOM IntelliSense](#_Toc318097412)
        - [Überladungen für das VSDOC-Signatur](#_Toc318097413)
        - [Impliziten verweisen](#_Toc318097414)
    - [CSS-Editor](#_Toc318097415)

        - [Anweisungsvervollständigung Auto / reduzieren](#_Toc318097416)
        - [Hierarchischer Einzug.](#_Toc318097417)
        - [CSS hacks Unterstützung](#_Toc318097418)
        - [Hersteller bestimmte Schemas (- Moz-, - Webkit)](#_Toc318097419)
        - [Kommentare und uncommenting-Unterstützung](#_Toc318097420)
        - [Farbauswahl](#_Toc318097421)
        - [Snippets (Ausschnitte)](#_Toc318097422)
        - [Benutzerdefinierte Regionen](#_Toc318097423)
    - [Seitenprüfung](#_Toc318097424)
    - [Veröffentlichen](#_Toc318097425)

        - [Veröffentlichungsprofile](#_Toc318097426)
        - [ASP.NET-Vorkompilierung und Zusammenführen](#_Toc318097427)
- [IIS Express](#_Toc318097428)
- [Haftungsausschluss](#_Toc318097429)

<a id="_Toc318097372"></a>
## <a name="aspnet-core-runtime-and-framework"></a>ASP.NET Core-Laufzeit und Framework

<a id="_Toc318097373"></a>
### <a name="asynchronously-reading-and-writing-http-requests-and-responses"></a>Asynchrones Lesen und Schreiben von HTTP-Anforderungen und Antworten

ASP.NET 4 eingeführt, die Möglichkeit, lesen eine HTTP-Anforderungsentität als Datenstrom mithilfe der *HttpRequest.GetBufferlessInputStream* Methode. Diese Methode bereitgestellt Streamingzugriff auf die Anforderungseinheit. Allerdings es synchron ausgeführt, die für die Dauer einer Anforderung einen Thread gebunden.

ASP.NET 4.5 unterstützt die Möglichkeit zum Lesen des Streams asynchron auf eine HTTP-Anforderungsentität und die Möglichkeit, die asynchron zu leeren. ASP.NET 4.5 gibt Ihnen außerdem die Möglichkeit, Double-Puffer für eine HTTP-Anforderung-Entität, die einfachere Integration mit downstream HTTP-Handler, wie z. B. aspx-Seite-Handler und ASP.NET MVC Controllers bereitstellt.

<a id="_Toc318097374"></a>
#### <a name="improvements-to-httprequest-handling"></a>Verbesserungen bei der Behandlung von HttpRequest

Referenz zur Stream zurückgegeben, die für ASP.NET 4.5 *HttpRequest.GetBufferlessInputStream* unterstützt synchrone und asynchrone read-Methoden. Die *Stream* Objekt, das von *GetBufferlessInputStream* jetzt BeginRead- und EndRead-Methoden implementiert. Der asynchrone *Stream* -Methoden können Sie die Anforderungseinheit segmentweise asynchron zu lesen, während ASP.NET den aktuellen Thread zwischen jeder Iteration einer Schleife für asynchrone read frei.

ASP.NET 4.5 wurde ebenfalls hinzugefügt. eine Begleitmethode zum Lesen der Anforderungsentität gepufferte so: *HttpRequest.GetBufferedInputStream*. Diese neue Überladung funktioniert wie *GetBufferlessInputStream*, synchrone und asynchrone Lesevorgänge zu unterstützen. Jedoch, wie er liest, *GetBufferedInputStream* auch die Entität Bytes in ASP.NET internen Puffer kopiert, sodass downstream-Module und Handler weiterhin die Anforderungseinheit zugreifen können. Z. B. wenn ein upstreamsortierungsziel Code in der Pipeline wurde bereits gelesen Entität für die Anforderung mit *GetBufferedInputStream*, noch können Sie *HttpRequest.Form* oder *HttpRequest.Files*. Dadurch können Sie asynchrone Verarbeitungen ausführen, auf eine Anforderung (z. B. das streaming große Dateiuploads in einer Datenbank), aber dennoch ausführen ASPX-Seiten und ASP.NET MVC-Controller anschließend.

<a id="_Toc318097375"></a>
#### <a name="asynchronously-flushing-a-response"></a>Eine Antwort leeren asynchron

Senden von Antworten an einen HTTP-Client kann sehr viel Zeit dauern, wenn der Client ist weit oder verfügt über eine Verbindung mit geringer Bandbreite. ASP.NET puffert normalerweise die Antwortbytes, wie sie von einer Anwendung erstellt werden. ASP.NET führt dann einen einzelnen Sendevorgang der aufgelaufene Puffer am Ende der anforderungsverarbeitung.

Wenn die gepufferte Antwort (z. B. eine große Datei an einen Client streaming) groß ist, müssen Sie in regelmäßigen Abständen Aufrufen *HttpResponse.Flush* gepufferte Ausgabe an den Client zu senden und speicherauslastung unter Kontrolle zu behalten. Allerdings da *leeren* ist ein synchroner Aufruf, iterativ Aufrufen *leeren* weiterhin einen Thread für die Dauer der potenziell langer Anforderungen verarbeitet.

ASP.NET 4.5 bietet Unterstützung für das Ausführen von leert asynchron mit dem *BeginFlush* und *EndFlush* Methoden die *HttpResponse* Klasse. Mit diesen Methoden können Sie die asynchrone Module und asynchrone Handler, die Daten inkrementell an einen Client zu senden, ohne das Betriebssystem Threads binden erstellen. In der Zwischenzeit *BeginFlush* und *EndFlush* Aufrufe, ASP.NET frei, den aktuellen Thread. Dies reduziert erheblich die Gesamtanzahl der aktiven Threads, die erforderlich sind, um die lang ausgeführte HTTP-Downloads zu unterstützen.

<a id="_Toc318097376"></a>
### <a name="support-for-await-and-task---based-asynchronous-modules-and-handlers"></a>Unterstützung für *"await"* und *Aufgabe* -basierte asynchrone Modules and Handlers

.NET Framework 4 eingeführt, eine asynchrone Programmierkonzept als bezeichnet eine *Aufgabe*. Aufgaben werden durch dargestellt die *Aufgabe* Typ und verwandte Typen in der *System.Threading.Tasks* Namespace. .NET Framework 4.5 baut auf dieser mit Compiler-Erweiterungen, die Arbeiten mit *Aufgabe* Objekte einfach. In .NET Framework 4.5 unterstützen die Compiler zwei neue Schlüsselwörter: *"await"* und *Async*. Die *"await"* -Schlüsselwort ist syntaktische Kurzform für, der angibt, die ein Stück Code asynchron auf einen anderen Codeabschnitt warten soll. Die *Async* Schlüsselwort stellt einen Hinweis an, die Sie verwenden können, um Methoden als aufgabenbasierten asynchronen Methoden zu kennzeichnen.

Die Kombination von *"await"*, *Async*, und die *Aufgabe* Objekt kann es viel einfacher Schreiben von asynchronem Code in .NET 4.5. ASP.NET 4.5 unterstützt diese vereinfachungen mit neuen APIs, mit denen Sie das Schreiben von asynchronen HTTP-Module und asynchrone HTTP-Handler mit der neuen Compiler-Erweiterungen.

<a id="_Toc318097377"></a>
#### <a name="asynchronous-http-modules"></a>Asynchrone HTTP-Module

Nehmen wir an, dass der asynchrone Vorgang in einer Methode ausführen, die zurückgegeben werden sollen eine *Aufgabe* Objekt. Das folgende Codebeispiel definiert eine asynchrone Methode, die einen asynchronen Aufruf der Microsoft-Startseite herunterladen kann. Beachten Sie die Verwendung der *Async* Schlüsselwort in der Methodensignatur und die *"await"* aufrufen, um *DownloadStringTaskAsync*.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample1.cs)]

Das ist alles in Sie schreiben müssen, – .NET Framework behandelt automatisch das Entladen der Aufrufliste beim Warten auf des Downloads abgeschlossen, als auch die Aufrufliste automatisch wiederherstellen, nachdem der Download abgeschlossen ist.

Nehmen wir jetzt an, dass Sie diese asynchrone Methode in einem asynchronen HTTP-ASP.NET-HTTP-Modul verwenden möchten. ASP.NET 4.5 umfasst eine Hilfsmethode (*EventHandlerTaskAsyncHelper*) und einen neuen Delegattyp (*TaskEventHandler*), mit der älteren aufgabenbasierte asynchrone Methoden integriert werden können asynchrone Programmiermodell verfügbar gemacht werden, indem Sie die ASP.NET HTTP-Pipeline. Dieses Beispiel zeigt, wie:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample2.cs)]

<a id="_Toc318097378"></a>
#### <a name="asynchronous-http-handlers"></a>Asynchrone HTTP-Handler

Der herkömmliche Ansatz schreiben Sie asynchrone Handler in ASP.NET besteht darin, implementieren die *IHttpAsyncHandler* Schnittstelle. ASP.NET 4.5 führt die *HttpTaskAsyncHandler* asynchrone Basistyp, der Sie ableiten können, dem wesentlich leichter, asynchrone Handler schreiben kann.

Die *HttpTaskAsyncHandler* Typ abstrakt ist und fordert Sie zum Überschreiben der *ProcessRequestAsync* Methode. Intern ASP.NET übernimmt die Integration der return-Signatur (ein *Aufgabe* Objekt) der *ProcessRequestAsync* mit der älteren asynchronen Programmiermodells von der ASP.NET-Pipeline verwendet.

Das folgende Beispiel zeigt, wie Sie verwenden können *Aufgabe* und *"await"* als Teil der Implementierung von asynchronen HTTP-Handler:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample3.cs)]

<a id="_Toc318097379"></a>
### <a name="new-aspnet-request-validation-features"></a>Neue Funktionen von ASP.NET Anforderung Überprüfung

Standardmäßig führt ASP.NET Anforderungsvalidierung – untersucht Anforderungen nach Markup oder Skript in die Felder, Header, Cookies und So weiter gesucht werden soll. Wenn alle erkannt wird, wird von ASP.NET eine Ausnahme ausgelöst. Dies dient als erste Verteidigungslinie gegen potenzielle Cross-Site scripting-Angriffe.

ASP.NET 4.5 erleichtert das nicht überprüfte Anforderungsdaten selektiv zu lesen. ASP.NET 4.5 wird auch die beliebte AntiXSS-Bibliothek, die zuvor war von einer externen Bibliothek integriert.

Entwickler haben FAQs für den Zugriff auf die Anforderungsvalidierung für ihre Anwendungen selektiv zu deaktivieren. Wenn Ihre Anwendung Forum Software ist, sollten Sie z. B. ermöglichen Benutzern die HTML-formatierte Forumsbeiträge und Kommentare senden, aber dennoch stellen sicher, dass die anforderungsüberprüfung alles überprüft werden.

ASP.NET 4.5 führt zwei Funktionen, mit denen Sie selektiv arbeiten mit nicht überprüften Eingabe erleichtert: verzögerte Anforderungsvalidierung ("lazy") und den Zugriff auf nicht überprüfte Anforderungsdaten verwendet werden.

<a id="_Toc318097380"></a>
#### <a name="deferred-lazy-request-validation"></a>Verzögerte Anforderungsvalidierung ("lazy")

In ASP.NET 4.5 wird standardmäßig alle Anforderungsdaten der Anforderungsvalidierung unterliegt. Allerdings können Sie die Anwendung für die Anforderungsvalidierung zu verzögern, bis Sie tatsächlich Anforderungsdaten zugreifen konfigurieren. (Dies wird manchmal als lazy anforderungsüberprüfung, basierend auf Begriffe wie verzögertes Laden für bestimmte Datenszenarien bezeichnet.) Sie können konfigurieren, dass die Anwendung zur Verwendung der verzögerten Überprüfung in der Datei "Web.config" durch Festlegen der *RequestValidationMode* -Attribut auf 4.5 in der *HttpRUntime* Element, wie im folgenden Beispiel:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample4.xml)]

Nach dem Anforderung Validierungsmodus 4.5 festgelegt ist, wird die Anforderungsvalidierung ausgelöst, nur für eine bestimmte Anforderungswert und nur, wenn Ihr Code den Wert zugreift. Angenommen, Ihr Code den Wert der Request.Form["forum abruft\_post"], Anforderungsvalidierung wird nur für dieses Element in der formularauflistung aufgerufen. Keines der anderen Elemente in der *Formular* Auflistung überprüft werden. In früheren Versionen von ASP.NET wurde anforderungsüberprüfung für die Sammlung der gesamte Anforderung ausgelöst, wenn jedes Element in der Auflistung zugegriffen wurde. Das neue Verhalten erleichtert es verschiedene Anwendungskomponenten betrachten unterschiedliche Arten von Anforderungsdaten verwendet werden, ohne die Anforderungsvalidierung auf andere Teile.

<a id="_Toc318097381"></a>
#### <a name="support-for-unvalidated-requests"></a>Unterstützung für nicht überprüfte Anforderungen

Verzögerte Anforderungsvalidierung allein Problem nicht das der anforderungsüberprüfung selektiv umgehen. Der Aufruf von Request.Form["forum\_post"] noch Trigger die Anforderungsvalidierung für diese bestimmte Anforderungswert. Sie möchten jedoch Zugriff auf dieses Feld ohne Validierung auslösen, da Sie in diesem Feld Markup zulassen möchten.

Um dies zu ermöglichen, unterstützt ASP.NET 4.5 jetzt nicht überprüfte Zugriff auf Anforderungsdaten verwendet werden. ASP.NET 4.5 umfasst ein neues *Unvalidated* Auflistungseigenschaft in der *HttpRequest* Klasse. Diese Auflistung bietet Zugriff auf alle häufig verwendeten Werte von Anforderungsdaten verwendet werden, z. B. *Formular*, *QueryString*, *Cookies*, und *Url*.

Verwenden im Beispiel Forum, um nicht überprüfte Daten lesen können, müssen Sie zunächst die Anwendung zur Verwendung des neuen Anforderung Validierungsmodus konfigurieren:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample5.xml)]

Anschließend können Sie die *HttpRequest.Unvalidated* Eigenschaft, die der nicht validierten Formularwert gelesen:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample6.cs)]


> [!WARNING]
> Sicherheit – *nicht überprüfte Daten verwenden, mit Vorsicht zu verwenden!* ASP.NET 4.5 hinzugefügt, die nicht überprüfte Anforderungseigenschaften und Sammlungen an, die Sie Zugriff auf spezifische nicht überprüfte Anforderungsdaten erleichtern. Sie müssen jedoch weiterhin benutzerdefinierte Validierung ausführen, auf die unformatierten Daten um sicherzustellen, dass für Benutzer nicht gefährlich Text gerendert wird.


<a id="_Toc318097382"></a>
### <a name="antixss-library"></a>AntiXSS-Bibliothek

Aufgrund der Beliebtheit der Microsoft AntiXSS Library enthält die ASP.NET 4.5 jetzt Core Codierung Routinen Version 4.0 von der Bibliothek.

Die codieren Routinen, die von implementiert die *AntiXssEncoder* Typ in der neuen *System.Web.Security.AntiXss* Namespace. Sie können die *AntiXssEncoder* Typ direkt durch das Aufrufen einer der statischen Methoden codieren, die in den Typ implementiert werden. Der einfachste Ansatz für die Verwendung der neuen Anti-XSS-Routinen ist jedoch so konfigurieren Sie eine ASP.NET-Anwendung zum Verwenden der *AntiXssEncoder* Klasse standardmäßig. Zu diesem Zweck fügen Sie das folgende Attribut in der Datei "Web.config" hinzu:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample7.xml)]

Wenn die *EncoderType* -Attribut festgelegt ist, verwenden die *AntiXssEncoder* Typ, alle Ausgabedateien Codierung in ASP.NET automatisch die neue codieren Routinen verwendet.

Dies sind die Teile der externen AntiXSS-Bibliothek, die in ASP.NET 4.5 integriert wurde, haben:

- *HtmlEncode*, *HtmlFormUrlEncode*, und *HtmlAttributeEncode*
- *XmlAttributeEncode* und *XmlEncode*
- *Dieses als UrlEncode* und *UrlPathEncode* (neu)
- *CssEncode*

<a id="_Toc318097383"></a>
### <a name="support-for-websockets-protocol"></a>Unterstützung für WebSockets-Protokoll

WebSockets-Protokoll ist eine standardbasierte Windows-Netzwerkprotokoll, das definiert, wie sichere, Echtzeit bidirektionale Kommunikation zwischen einem Client und einem Server über HTTP hergestellt. Microsoft arbeitet mit der IETF und die W3C-Standards stellen helfen beim Definieren des Protokolls. WebSockets-Protokoll wird von jedem Client (nicht nur Browser), mit Microsoft investieren erhebliche Ressourcen, die Unterstützung der WebSockets-Protokoll auf Client und mobilen Betriebssystemen unterstützt.

WebSockets-Protokoll kann es viel einfacher langer Datenübertragungen zwischen einem Client und einem Server zu erstellen. Beispielsweise ist das Schreiben Chat viel einfacher, da Sie eine "true" langer-Verbindung zwischen einem Client und einem Server herstellen können. Sie müssen keinen problemumgehungen wie regelmäßigen Abruf oder HTTP-langer Abrufdauer simulieren des Verhaltens eines Sockets verwenden zu können.

ASP.NET 4.5 und IIS 8 gehören WebSockets-Unterstützung auf niedriger Ebene, das Aktivieren von ASP.NET-Entwickler, verwalteten APIs asynchron lesen und Schreiben von Zeichenfolge und Binärdaten für ein Objekt WebSockets zu verwenden. Für ASP.NET 4.5, es gibt eine neue *System.Web.WebSockets* Namespace, der Typen enthält, für die Arbeit mit WebSockets-Protokoll.

Ein Browserclient stellt eine WebSockets-Verbindung her, erstellen Sie ein DOM *WebSocket* -Objekt, das auf eine URL in einer ASP.NET-Anwendung, wie im folgenden Beispiel zeigt:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample8.cs)]

Sie können WebSockets-Endpunkte in ASP.NET mithilfe beliebiger Modul oder Handler erstellen. Im vorherigen Beispiel wurde eine ASHX-Datei verwendet, da ASHX-Dateien eine schnelle Möglichkeit, einen Ereignishandler zu erstellen sind.

Nach dem WebSockets-Protokoll akzeptiert eine ASP.NET-Anwendung eine Clientanforderung WebSockets Angabe, dass die Anforderung von einer HTTP GET-Anforderung auf eine WebSockets-Anforderung aktualisiert werden soll. Im Folgenden ein Beispiel:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample9.cs)]

Die *AcceptWebSocketRequest* Methode akzeptiert eine zu delegierende Funktion, da ASP.NET die aktuelle HTTP-Anforderung entlädt, und klicken Sie dann überträgt die Steuerung an den Funktionsdelegaten. Dieser Ansatz ist grundsätzlich ähnelt der Verwendung von *System.Threading.Thread*, in dem Sie einen Thread Boot-Delegaten definieren in die hintergrundverarbeitung ausgeführt wird.

Wenn ASP.NET und der Client einen Handshake des WebSockets erfolgreich abgeschlossen haben, ASP.NET ruft der Delegat und WebSockets dem Start der Anwendung. Das folgende Codebeispiel zeigt eine einfache Echo-Anwendung, die die integrierte Unterstützung für WebSockets in ASP.NET verwendet:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample10.cs)]

Die Unterstützung in .NET 4.5 für die *"await"* -Schlüsselwort und asynchrone aufgabenbasierte Vorgänge wird ein natürliches Maß für das Schreiben von WebSockets-Anwendungen. Im Codebeispiel wird veranschaulicht, dass eine Anforderung WebSockets innerhalb von ASP.NET vollständig asynchron ausgeführt wird. Die Anwendung wartet asynchron auf eine Nachricht von einem Client gesendet werden, durch den Aufruf *"await" Socket. ReceiveAsync*. Sie können auf ähnliche Weise eine asynchrone Meldung an einen Client senden, durch den Aufruf *"await" Socket. "SendAsync"*.

Im Browser empfängt eine Anwendung WebSockets-Nachrichten über eine *Onmessage* Funktion. Rufen Sie zum Senden einer Nachricht in einem Browser die *senden* Methode der *WebSocket* DOM-Datentyp, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample11.cs)]

In der Zukunft möglicherweise Updates für diese Funktionalität veröffentlichen wir, dass abstrakte sofort Teil, d. h. die Low-Level Codierung in dieser Version für WebSockets Anwendungen erforderlich.

<a id="_Toc318097384"></a>
### <a name="bundling-and-minification"></a>Bundling und Minimierung

M bündeln, können Sie die einzelne JavaScript- und CSS-Dateien zu einem Paket zu kombinieren, die wie eine einzelne Datei behandelt werden kann. Minimierung verdichtet JavaScript und CSS-Dateien durch das Entfernen von Leerzeichen und andere Zeichen, die nicht erforderlich sind. Diese Features funktionieren mit Web Forms, ASP.NET MVC und Webseiten.

Pakete werden mithilfe der Paket-Klasse oder einer ihrer untergeordneten Klassen, ScriptBundle und StyleBundle erstellt. Nach dem Konfigurieren einer Instanz eines Pakets, wird das Paket, indem Sie einfach eine globale Instanz von BundleCollection hinzugefügt auf eingehende Anforderungen zur Verfügung gestellt. In den Standardvorlagen wird Bundle-Konfiguration in einer Datei BundleConfig ausgeführt. Diese Standardkonfiguration erstellt Pakete für alle Skripts Core und Css-Dateien, die von den Vorlagen verwendet.

Pakete werden von in Ansichten mithilfe einer der einige mögliche Hilfsmethoden verwiesen. Um das Rendern von verschiedenen Markups für ein Paket im Debugmodus im Vergleich zu Releasemodus zu unterstützen, verfügen die ScriptBundle und StyleBundle-Klassen die Hilfsmethode rendern. Wenn Sie sich im Debugmodus befindet, generiert Render-Markup für jede Ressource im Paket. Wenn im Releasemodus, generiert Render ein einzelnes Markupelement für das gesamte Paket. Umschalten zwischen Debug- und Modus ausgeführt werden kann, indem Sie das Debugattribut, der das Compilation-Element in der Datei "Web.config" ändern, wie unten dargestellt:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample12.xml)]

Aktivieren oder Deaktivieren der Optimierung kann darüber hinaus direkt über die BundleTable.EnableOptimizations-Eigenschaft festgelegt werden.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample13.cs)]

Wenn Dateien gebündelt werden, diese zuerst alphabetisch sortiert (wie sie, in angezeigt werden **Projektmappen-Explorer**). Sie sind dann organisiert, sodass Bibliotheken bezeichnet, und ihre benutzerdefinierten Erweiterungen (z. B. jQuery, MooTools und Dojo) zuerst geladen werden. Beispielsweise wird die endgültige Reihenfolge für die Bündelung der Ordner "Skripts", wie oben dargestellt werden:

1. jquery-1.6.2.js
2. jquery-ui.js
3. jquery.tools.js
4. a.js

CSS-Dateien werden ebenfalls alphabetisch sortiert, und klicken Sie dann neu organisiert, damit jede andere Datei reset.css und normalize.css vorangestellt. Die endgültige Sortierung der die Bündelung der oben angezeigten Ordner Styles wird dies Folgendes sein:

1. reset.css
2. content.css
3. forms.css
4. globals.css
5. menu.css
6. styles.css

<a id="_Toc_perf"></a>
### <a name="performance-improvements-for-web-hosting"></a>Verbesserte Leistung beim Webhosting

Die .NET Framework 4.5 und Windows 8 eingeführt, mit denen Sie eine erhebliche Leistungssteigerung bewirken für Web-Server-arbeitsauslastungen zu erreichen können. Dies schließt eine Reduzierung (bis zu 35 %) in beiden Startzeit und den Speicherbedarf der Web hosting von Websites, die ASP.NET verwenden.

<a id="_Toc_perf_1"></a>
#### <a name="key-performance-factors"></a>Wichtige Leistungsfaktoren

Im Idealfall sollten alle Websites aktiv sein, und im Arbeitsspeicher, um schnelle Antworten auf die nächste Anforderung zu gewährleisten, wenn es darum geht. Faktoren, die Standort-Reaktionsfähigkeit beeinflussen können umfassen:

- Der Zeitaufwand für eine Website neu starten, nachdem ein Anwendungspool wiederverwendet wird. Dies ist der Zeitaufwand für das um einen Webserver-Prozess für den Standort zu starten, wenn die Website-Assemblys nicht mehr im Speicher abgelegt sind. (Die Plattformassemblys werden immer noch im Arbeitsspeicher, da sie von anderen Standorten verwendet werden.) Diese Situation wird bezeichnet als "kalte Standort betriebsbereiten Framework Start" oder einfach "kalt Start der site."
- Wie viel Arbeitsspeicher belegen, der Standort. Für diese Begriffe sind "standortabhängigen Speicherverbrauch" oder "Arbeitsseiten ohne Freigabe."

Die neue leistungsverbesserungen konzentrieren sich auf beide dieser Faktoren.

<a id="_Toc_perf_2"></a>
#### <a name="requirements-for-new-performance-features"></a>Anforderungen für die neuen Leistungsfunktionen

Die Anforderungen für die neuen Funktionen können in folgende Kategorien unterteilt werden:

- Es wurden Verbesserungen, die auf .NET Framework 4 ausgeführt.
- Verbesserungen, die .NET Framework 4.5 erforderlich, jedoch können auf eine beliebige Version von Windows ausgeführt wird.
- Es wurden Verbesserungen, die nur mit .NET Framework 4.5 unter Windows 8 verfügbar sind.

Leistung nimmt mit jeder Ebene der Verbesserung, die Sie aktivieren können.

Einige der .NET Framework 4.5-Verbesserungen Nutzen der größeren Leistungsfunktionen, die für andere Szenarien sowie gelten.

<a id="_Toc_perf_3"></a>
#### <a name="sharing-common-assemblies"></a>Freigeben von gemeinsamen Assemblys

**Anforderung**: .NET Framework 4 und Visual Studio 11 Developer Preview SDK

Verschiedene Standorte auf einem Server verwenden häufig die gleichen Hilfsassemblys (z. B. Assemblys aus einem Starter Kit oder Beispiel-Anwendung). Jeder Standort hat eine eigene Kopie des diese Assemblys im Verzeichnis "bin". Obwohl der Objektcode für die Assemblys identisch ist, sie physisch getrennten Assemblys damit jede Assembly muss separat während des Starts der kalten Standort gelesen werden und separat im Arbeitsspeicher beibehalten.

Die neue interning Funktionalität löst diese Ineffizienz und reduziert die RAM-Anforderungen und Load Zeit. Internalisierung können Windows eine einzige Kopie jeder Assembly im Dateisystem beibehalten, und einzelne Assemblys im Ordner "bin" Website mit symbolischen Verknüpfungen auf die einzelne Kopie ersetzt. Wenn ein einzelnes Standorts eine unterschiedliche Version der Assembly benötigt, das die symbolische Verknüpfung wird durch die neue Version der Assembly ersetzt, und nur diesem Standort ist betroffen.

Freigegebene Assemblys mit symbolischen Verknüpfungen erfordert ein neues Tool, mit dem Namen Aspnet\_intern.exe, was Ihnen das Erstellen und Verwalten des Speichers für Assemblys im Internpool vorhanden. Diese wird als Teil der Visual Studio 11 Developer Preview-SDKS angegeben. (Dies funktioniert jedoch, wird auf einem System mit nur .NET Framework 4 installiert ist, vorausgesetzt, Sie haben die neuesten installiert [aktualisieren](https://support.microsoft.com/kb/2468871).)

Um sicherzustellen, dass alle geeigneten Assemblys haben Internpool wurde, führen Sie Aspnet\_intern.exe in regelmäßigen Abständen (z. B. einmal pro Woche als geplanter Task). Eine typische Verwendung lautet wie folgt:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample14.cmd)]

Um alle Optionen anzuzeigen, führen Sie das Tool ohne Argumente ein.

<a id="_Toc_perf_4"></a>
#### <a name="using-multi-core-jit-compilation-for-faster-startup"></a>Verwenden von Multikern-JIT-Kompilierung für schnellere-Starts

**Anforderung**: .NET Framework 4.5

Für ein Start kalte Standort nicht nur müssen Assemblys vom Datenträger gelesen werden, sondern die Site JIT-kompiliert werden muss. Für einen komplexen Standort kann dies zu erhebliche Verzögerungen hinzufügen. Ein neues allgemeines Verfahren in .NET Framework 4.5 wird verringert, dass diese Verzögerungen bei der JIT-Kompilierung über verfügbare Prozessorkerne verbreiten. Dies geschieht möglichst alle Aufgaben und so früh wie möglich mit Informationen, die während der vorherigen startet des Standorts. Diese Funktionalität implementiert werden, indem Sie die [System.Runtime.ProfileOptimization.StartProfile](https://msdn.microsoft.com/library/system.runtime.profileoptimization.startprofile(VS.110).aspx) Methode.

JIT-Kompilierung mit mehreren Kernen ist standardmäßig aktiviert, in ASP.NET, daher Sie kein gar nichts Unternehmen, um dieses Feature nutzen müssen. Wenn Sie diese Funktion deaktivieren möchten, stellen Sie die folgende Einstellung in der Datei "Web.config" ein:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample15.xml)]

<a id="_Toc_perf_5"></a>
#### <a name="tuning-garbage-collection-to-optimize-for-memory"></a>Optimieren der Garbagecollection für den Speicher zu optimieren.

**Anforderung**: .NET Framework 4.5

Sobald eine Website ausgeführt wird, kann die Verwendungsmöglichkeiten des Heaps der Garbage-Collector (GC) ein bedeutender Faktor für ihren Arbeitsspeicherverbrauch sein. Wie alle Garbage Collector wird die .NET Framework GC vor-und Nachteile zwischen CPU-Zeit (Häufigkeit und Bedeutung der Sammlungen) und arbeitsspeichernutzung (zusätzlichen Speicherplatz, der für neue, freigegeben oder freigeben können Objekte verwendet wird). Frühere Versionen enthalten Anleitungen zum Konfigurieren der globale Katalogserver, um das richtige Gleichgewicht zu erreichen (z. B. finden Sie unter [ASP.NET 2.0/3.5 Hosting-Freigabekonfiguration](https://www.iis.net/learn/web-hosting/web-server-for-shared-hosting/aspnet-20-35-shared-hosting-configuration)).

Für die .NET Framework 4.5, anstatt mehrere eigenständige Einstellungen, eine arbeitsauslastung definierte Konfigurationseinstellung wird ermöglicht, die alle der oben genannten GC-Einstellungen als auch neue optimieren, die zusätzlichen Leistung für die pro-Website bietet Workingset.

Um GC-Speicher, die Optimierung zu aktivieren, fügen Sie die folgende Einstellung in die Windows\Microsoft.NET\Framework\v4.0.30319\aspnet.config-Datei ein:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample16.xml)]

(Wenn Sie mit der vorherigen Anleitung für die Änderungen zu aspnet.config vertraut sind, beachten Sie, dass diese Einstellung die alten Einstellungen ersetzt – z. B. besteht keine Notwendigkeit GcServer, GcConcurrent usw. festlegen. Sie verfügen nicht über die alten Einstellungen zu entfernen.)

<a id="_Toc_perf_6"></a>
#### <a name="prefetching-for-web-applications"></a>Vorabruf für Webanwendungen

**Anforderung**: .NET Framework 4.5 auf Windows 8 ausgeführt wird

Für mehrere Versionen noch Windows eine Technologie, der als Bestandteil der [Prefetcher](http://en.wikipedia.org/wiki/Prefetcher) , die reduziert der Kosten-Datenträgerlese Start der Anwendung. Da Kaltstart vorwiegend für Clientanwendungen ein Problem darstellt, ist diese Technologie nicht in Windows Server enthalten nur Komponenten enthält, die auf einem Server unverzichtbar sind. Vorabruf ist jetzt verfügbar in der neuesten Version von Windows Server, auf dem den Start der einzelnen Websites optimiert werden kann.

Für Windows-Server ist die Prefetcher nicht standardmäßig aktiviert. Zum Aktivieren und konfigurieren die Prefetcher für PHP-Webhosting, führen Sie den folgenden Satz von Befehlen in der Befehlszeile ein:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample17.cmd)]

Fügen Sie dann zum Integrieren der Prefetcher in ASP.NET-Anwendungen Folgendes in die Datei "Web.config":

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample18.xml)]

<a id="_Toc318097385"></a>
## <a name="aspnet-web-forms"></a>ASP.NET-Web Forms

<a id="_Toc318097386"></a>
### <a name="strongly-typed-data-controls"></a>Stark typisierte Daten-Steuerelemente

In ASP.NET 4.5 bietet Web Forms einige Verbesserungen für die Arbeit mit Daten. Die erste Verbesserung ist stark typisierte Datensteuerelemente. Für in früheren Versionen von ASP.NET Web Forms-Steuerelemente, Sie anzeigen, einen datengebundenen mit *Eval* und einen Datenbindungsausdruck:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample19.aspx)]

Verwenden Sie für die bidirektionale Datenbindung, *binden*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample20.aspx)]

Diese Aufrufe mithilfe der Reflektion zur Laufzeit lesen den Wert des angegebenen Elements, und klicken Sie dann das Ergebnis in das Markup angezeigt. Dieser Ansatz vereinfacht das Binden von Daten für beliebige, unshaped Daten.

Allerdings unterstützen Datenbindungsausdrücke sieht keine Funktionen wie IntelliSense für Elementnamen, Navigation, (wie Gehe zu Definition) oder der Kompilierung für diese Namen.

Um dieses Problem zu beheben, fügt ASP.NET 4.5 die Möglichkeit, den Datentyp der Daten zu deklarieren, die ein Steuerelement gebunden ist. Erreichen Sie dies mit der neuen *ItemType* Eigenschaft. Wenn Sie diese Eigenschaft festlegen, zwei neue typisierte Variablen stehen im Bereich des Datenbindungsausdrücke: *Element* und *BindItem*. Da die Variablen stark typisiert sind, erhalten Sie von allen Vorteilen des Visual Studio-Entwicklungsumgebung.


Für bidirektionale Datenbindungsausdrücke verwenden die *BindItem* Variablen:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample21.aspx)]


Die meisten Steuerelemente in ASP.NET Web Forms-Framework, die die Datenbindung unterstützen wurden aktualisiert, um die Unterstützung der *ItemType* Eigenschaft.

<a id="_Toc318097387"></a>
### <a name="model-binding"></a>Wurden die Modellbindung

Wurden die modellbindung erweitert die Datenbindung in ASP.NET Web Forms-Steuerelementen zur Bearbeitung von codeorientierten Datenzugriff. Er beinhaltet Konzepte aus der *ObjectDataSource* Steuerelements und von der modellbindung in ASP.NET MVC.

<a id="_Toc318097388"></a>
#### <a name="selecting-data"></a>Auswählen von Daten

So konfigurieren ein Steuerelement zum modellbindung verwenden, um Daten auszuwählen, legen Sie des Steuerelements *SelectMethod* -Eigenschaft auf den Namen einer Methode im Code der Seite. Das Steuerelement ruft die Methode zum geeigneten Zeitpunkt im Lebenszyklus Seite und die zurückgegebenen Daten automatisch gebunden. Es ist nicht erforderlich, explizit aufrufen, die *DataBind* Methode.

Im folgenden Beispiel die *GridView* Steuerelement ist so konfiguriert, dass eine Methode mit dem Namen verwenden *GetCategories*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample22.aspx)]

Sie erstellen die *GetCategories* Methode im Code der Seite. Für eine einfache select-Vorgang die Methode keine Parameter erforderlich und sollte Zurückgeben einer *IEnumerable* oder *IQueryable* Objekt. Wenn die neue *ItemType* festgelegt wird (wodurch Datenbindungsausdrücke, stark typisiert wie unter erläutert [Datensteuerelemente stark typisierte](#_Toc318097386) weiter oben), die generischen Versionen dieser Schnittstellen zurückgegeben werden soll – *IEnumerable&lt;T&gt;*  oder *IQueryable&lt;T&gt;*, mit der *T* Parameter, die den Typ der Abgleich der *ItemType* Eigenschaft (z. B. *IQueryable&lt;Kategorie&gt;*).

Das folgende Beispiel zeigt den Code für eine *GetCategories* Methode. Dieses Beispiel verwendet das Entity Framework Code First-Modell mit der Northwind-Beispieldatenbank. Der Code stellt sicher, dass die Abfrage Details für jede Kategorie über verwandte Produkte zurück die *Include* Methode. (Dadurch wird sichergestellt, dass die *TemplateField* Element im Markup zeigt die Anzahl der Produkte in jeder Kategorie an, ohne dass ein [n + 1 wählen](http://stackoverflow.com/questions/97197/what-is-the-n1-selects-problem).)

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample23.cs)]

Wenn die Seite ausgeführt wird, die *GridView* Aufrufe steuern die *GetCategories* Methode automatisch und rendert die zurückgegebenen Daten mit den konfigurierten Feldern:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.png)

Da die select-Methode gibt ein *IQueryable* -Objekt, der *GridView* Steuerelement kann die Abfrage weiter bearbeiten, vor der Ausführung. Z. B. die *GridView* Steuerelement kann Abfrageausdrücke für das Sortieren und paging auf das zurückgegebene hinzufügen *IQueryable* bevor er ausgeführt wird, Objekt, sodass diese Vorgänge von den zugrunde liegenden ausgeführt werden LINQ-Anbieter. In diesem Fall wird Entity Framework sichergestellt, dass diese Vorgänge in der Datenbank ausgeführt werden.

Das folgende Beispiel zeigt die *GridView* Steuerelement geändert wird, um Sortierung und paging zu ermöglichen:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample24.aspx)]

Wenn die Seite ausgeführt wird, kann das Steuerelement nun sicher, dass nur die aktuelle Seite der Daten angezeigt wird und er von der ausgewählten Spalte sortiert wird treffen:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.png)

Um die zurückgegebenen Daten zu filtern, müssen Parameter an die ausgewählte Methode hinzugefügt werden. Dieser Parameter werden von der modellbindung zur Laufzeit aufgefüllt und können Sie Sie verwenden, um die Abfrage zu ändern, bevor die Daten zurückgegeben.

Nehmen wir beispielsweise an, dass Benutzer Filter Produkte können durch Eingabe eines Schlüsselwortes in der Abfragezeichenfolge verwendet werden soll. Sie können die Methode einen Parameter hinzu, und aktualisieren Sie den Code, um den Parameterwert zu verwenden:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample25.cs)]

Dieser Code umfasst eine *, in denen* Ausdruck, wenn kein Wert angegeben ist, für die *Schlüsselwort* und gibt dann die Ergebnisse der Abfrage zurück.

<a id="_Toc318097389"></a>
#### <a name="value-providers"></a>Wertanbieter

Im vorherige Beispiel war nicht dazu, wo bestimmte den Wert für die *Schlüsselwort* Parameter stammt. Wenn Sie diese Informationen anzeigen möchten, können Sie eine Parameterattribut verwenden. In diesem Beispiel können Sie die *QueryStringAttribute* Klasse, die in der *System.Web.ModelBinding* Namespace:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample26.cs)]

Dies weist wurden die modellbindung zu dem Versuch, einen Wert aus der Abfragezeichenfolge zum Binden der *Schlüsselwort* Parameter zur Laufzeit. (Dies kann das umfassen Typ Konvertierung, obwohl dies in diesem Fall nicht der Fall.) Wenn ein Wert kann nicht angegeben werden und des Typs NULL-Werte zulässt, wird eine Ausnahme ausgelöst.

Die Quellen der Werte für diese Methoden werden als Wertanbieter bezeichnet, und die Parameterattribute, die angeben, welche Wertanbieter zu verwenden, werden als Anbieter Wertattribute bezeichnet. WebForms umfasst Wertanbieter und die entsprechenden Attribute für alle typischen Quellen von Benutzereingaben in einer Web Forms-Anwendung, z. B. die Abfragezeichenfolge, die Cookies, Formularwerte, Steuerelemente, Ansichtszustand, Sitzungsstatus und Profileigenschaften. Sie können auch benutzerdefinierte Wertanbieter schreiben.

Standardmäßig ist der Name des Parameters als Schlüssel verwendet, einen Wert in die Auflistung der Wert gefunden. Im Beispiel wird der Code Abfragezeichenfolgen-Wert namens Schlüsselwort aussehen (z. B. ~ / default.aspx?keyword=chef). Sie können einen benutzerdefinierten Schlüssel angeben, mit dem Parameterattribut als Argument übergeben. Beispielsweise können für die Verwendung den Wert der Abfragezeichenfolgen-Variablen mit dem Namen Q Sie erfolgen:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample27.cs)]

Wenn diese Methode im Code der Seite ist, können Benutzer die Ergebnisse filtern, indem Sie ein Schlüsselwort, das mithilfe der Abfragezeichenfolge übergeben:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.png)

Wurden die modellbindung lassen sich zahlreiche Aufgaben, die Sie sonst manuell codieren: Lesen des Werts, Überprüfung auf einen null-Wert, bei dem Versuch, die sie in den entsprechenden Typ zu konvertieren, wird geprüft, ob die Konvertierung erfolgreich war und schließlich mithilfe des Werts in der Abfrage. Das Modell Binden von Ergebnissen in viel weniger Code und die Fähigkeit, die Funktionalität in der gesamten Anwendung wiederzuverwenden.

<a id="_Toc318097390"></a>
#### <a name="filtering-by-values-from-a-control"></a>Filtern nach Werten aus einem Steuerelement

Angenommen Sie, Sie der Benutzer einen Filterwert aus einer Dropdownliste auswählen kann anhand des Beispiels erweitern möchten. Das Markup der folgenden Dropdown-Liste hinzu, und konfigurieren, um die Daten abzurufen, aus einer anderen Methode mithilfe der *SelectMethod* Eigenschaft:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample28.aspx)]

In der Regel auch Hinzufügen einer *EmptyDataTemplate* Element an der *GridView* steuern, sodass das Steuerelement eine Meldung angezeigt wird, wenn keine übereinstimmenden Produkte gefunden werden:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample29.aspx)]

Fügen Sie in den Seitencode hinzu, dass die neue Methode für die Dropdown-Liste auswählen:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample30.cs)]

Aktualisieren Sie schließlich die *GetProducts* Methode, um einen neuen Parameter mit der ID der ausgewählten Kategorie aus der Dropdown-Liste auswählen:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample31.cs)]

Nachdem die Seite ausgeführt wird, Benutzer eine Kategorie aus der Dropdown-Liste auswählen können und die *GridView* Steuerelement automatisch erneut an die gefilterten Daten gebunden ist. Dies ist möglich, da wurden die modellbindung die Werte der Parameter für Methoden wählen verfolgt und erkennt, ob ein Parameterwert nach einem Postback geändert wurde. Wenn dies der Fall ist, erzwingt wurden die modellbindung zugeordneten Steuerelements an die Daten erneut binden.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.png)

<a id="_Toc318097391"></a>
### <a name="html-encoded-data-binding-expressions"></a>HTML-codierte Datenbindungsausdrücke

Sie können das Ergebnis des Datenbindungsausdrücke jetzt HTML-codiert. Fügen Sie einen Doppelpunkt (:)) bis zum Ende der &lt;% # Präfix, das den Datenbindungsausdruck markiert:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample32.aspx)]

<a id="_Toc318097392"></a>
### <a name="unobtrusive-validation"></a>Unaufdringlichen Überprüfung

Sie können jetzt die integrierte Validierungssteuerelemente um unaufdringliches JavaScript für die clientseitige Validierung Anwendungslogik nutzen konfigurieren. Dies erheblich reduziert die Menge an JavaScript Inline im Markup Seite gerendert, und verringert die Gesamtgröße der Seite. Sie können in einem der folgenden Weisen unaufdringliches JavaScript für Validierungssteuerelemente konfigurieren:

- Global, indem Sie die folgende Einstellung zum Hinzufügen der  *&lt;"appSettings"&gt;*  Element in der Datei "Web.config": 

    [!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample33.xml)]
- Global durch die Festlegung der *System.Web.UI.ValidationSettings.UnobtrusiveValidationMode* Eigenschaft *UnobtrusiveValidationMode.WebForms* (in der Regel in der *Anwendung \_Starten* Methode in der Datei "Global.asax").
- Einzeln für eine Seite durch Festlegen der neuen *UnobtrusiveValidationMode* Eigenschaft von der *Seite* Klasse *UnobtrusiveValidationMode.WebForms*.

<a id="_Toc318097393"></a>
### <a name="html5-updates"></a>Updates HTML5

Einige in Web Forms Nutzen der neuen Funktionen von HTML5-Steuerelemente wurden Verbesserungen vorgenommen:

- Die *TextMode* Eigenschaft von der *Textfeld* Steuerelement wurde aktualisiert, um die Unterstützung der neuen HTML5-Eingabetypen wie *e-Mail*, *"DateTime"*, und So weiter.
- Die *FileUpload* jetzt unterstützt, die mehrere Dateiuploads in Browsern mit Unterstützung für diese HTML5-Funktion zu steuern.
- Validierungssteuerelement steuert jetzt Unterstützung validierenden HTML5 Eingabeelemente.
- Neue HTML5-Elemente, die Attribute verfügen, die eine URL jetzt darstellen unterstützen Runat = "Server". Daher können Sie ASP.NET Konventionen in der URL-Pfade sein, z. B. der ~-Operator zum Stammverzeichnis der Anwendung darstellen (z. B. &lt;video Runat = "Server" src="~/myVideo.wmv" /&gt;).
- Die *UpdatePanel* Steuerelement zur Unterstützung der Buchung HTML5 Eingabefelder behoben wurde.

<a id="_Toc318097394"></a>
## <a name="aspnet-mvc-4"></a>ASP.NET MVC 4

ASP.NET MVC 4 Beta ist jetzt mit Visual Studio 11 Beta enthalten. ASP.NET MVC ist ein Framework zum Entwickeln einfach zu testender und verwaltbar Webanwendungen durch Nutzung der Model-View-Controller (MVC)-Muster. ASP.NET MVC 4 erleichtert das Erstellen von Anwendungen für mobile Web und enthält von ASP.NET Web API, mit dem Sie HTTP-Dienste erstellen, die jedem Gerät erreichen können. Weitere Informationen finden Sie unter der [ASP.NET MVC 4-Versionsanmerkungen](mvc4-release-notes.md).

<a id="_Toc318097395"></a>
## <a name="aspnet-web-pages-2"></a>ASP.NET-Webseiten 2

Die folgenden neuen Features aufgeführt:

- Neue und aktualisierte Websitevorlagen.
- Hinzufügen von serverseitige und clientseitige Validierung mit dem *Überprüfung* Helper.
- Die Fähigkeit zum Registrieren von Skripts, die über einen Ressourcen-Manager.
- Aktivieren Anmeldungen von Facebook und andere Standorte mithilfe von OAuth und OpenID.
- Hinzufügen von maps unter Verwendung der *ordnet* Helper.
- Ausführen von Webseiten Anwendungen Seite-an-Seite.
- Rendern von Seiten für mobile Geräte.

Weitere Informationen zu diesen Features und Ganzseitenmodus Codebeispiele finden Sie unter [der Top-Funktionen in Web Pages 2 Beta](https://go.microsoft.com/fwlink/?LinkID=227824).

<a id="_Toc318097396"></a>
## <a name="visual-web-developer-11-beta"></a>Visual Web Developer 11 Beta

Dieser Abschnitt enthält Informationen zu den Verbesserungen für die Webentwicklung in Visual Web Developer 11 Beta und Visual Studio 2012 Release Candidate.

<a id="project-compatibility"></a>
### <a name="project-sharing-between-visual-studio-2010-and-visual-studio-2012-release-candidate-project-compatibility"></a>Projekt freigeben zwischen Visual Studio 2010 und Visual Studio 2012 Release Candidate (Projektkompatibilität)

Öffnen ein vorhandenes Projekt in einer neueren Version von Visual Studio bis Visual Studio 2012 Release Candidate (RC) gestartet des Konvertierungs-Assistenten aus. Dies erzwungen ein Upgrade des Inhalts (Objekte) eine Projekt- und Projektmappendateien, in die neuen Formate, die nicht abwärtskompatibel waren. Aus diesem Grund konnte nach der Konvertierung Sie nicht das Projekt in der früheren Version von Visual Studio geöffnet werden.

Viele Kunden haben uns mitgeteilt, dass dies nicht der richtige Ansatz war. In Visual Studio 11 Beta unterstützen wir jetzt Freigabe Projekte und Projektmappen mit Visual Studio 2010 SP1. Dies bedeutet, dass wenn Sie ein 2010-Projekt in Visual Studio 2012 Release Candidate öffnen, Sie weiterhin auf das Projekt in Visual Studio 2010 SP1 öffnen können.

> [!NOTE]
> Einige Typen von Projekten können nicht in Visual Studio 2010 SP1 und Visual Studio 2012 Release Candidate gemeinsam genutzt werden. Dazu gehören einige ältere Projekte (z. B. ASP.NET MVC 2 Projekte) oder die Projekte für spezielle Zwecke (z. B. Setupprojekten).

Wenn Sie ein Visual Studio 2010 SP1 Web-Projekt zum ersten Mal in Visual Studio 11 Beta öffnen, werden die folgenden Eigenschaften der Projektdatei hinzugefügt:

- FileUpgradeFlags
- UpgradeBackupLocation
- OldToolsVersion
- VisualStudioVersion
- VSToolsPath

FileUpgradeFlags UpgradeBackupLocation und OldToolsVersion werden vom Prozess verwendet, mit die die Projektdatei aktualisiert. Sie haben keinen Einfluss auf das Projekt in Visual Studio 2010 verwenden.

VisualStudioVersion ist eine neue Eigenschaft verwendet, die durch MSBuild 4.5, der die Version von Visual Studio für das aktuelle Projekt angibt. Da diese Eigenschaft in MSBuild 4.0 (die Version von MSBuild, die mithilfe von Visual Studio 2010 SP1) noch nicht gab, Einfügen wir einen Standardwert in die Projektdatei.

Die VSToolsPath-Eigenschaft wird verwendet, um zu bestimmen, die richtige TARGETS-Datei zum Importieren aus dem Pfad, der durch die Einstellung MSBuildExtensionsPath32 dargestellt.

Es gibt auch einige Änderungen in Bezug auf die Import-Elemente. Diese Änderungen sind erforderlich, um die Kompatibilität zwischen den beiden Versionen von Visual Studio zu unterstützen.

> [!NOTE]
> Wenn ein Projekt auf zwei unterschiedlichen Computern zwischen Visual Studio 2010 SP1 und Visual Studio 11 Beta freigegeben ist und das Projekt eine lokale Datenbank in die App einbindet\_Datenordner, müssen Sie sich vergewissern, dass die Version von SQL Server, die von der Datenbank verwendet wird auf beiden Computern installiert.

<a id="Configuration_Changes_In_ASPNET45_Website_Templates"></a>
### <a name="configuration-changes-in-aspnet-45-website-templates"></a>Änderungen an der Konfiguration in ASP.NET 4.5 Websitevorlagen

Die folgenden Änderungen wurden auf den Standardwert *"Web.config"* Datei für die Website, die mit Website-Projektvorlagen in Visual Studio 2012 Release Candidate erstellt werden:

- In der `<httpRuntime>` Element, das `encoderType` Attribut wird jetzt standardmäßig festgelegt, um die AntiXSS-Typen zu verwenden, die ASP.NET hinzugefügt wurden. Weitere Informationen finden Sie unter [AntiXSS-Bibliothek](#_Toc318097382).
- Auch in der `<httpRuntime>` Element, das `requestValidationMode` -Attribut auf "4,5" festgelegt ist. Dies bedeutet, dass standardmäßig anforderungsüberprüfung für die Verwendung der verzögerten ("lazy") Überprüfung konfiguriert ist. Weitere Informationen finden Sie unter [neue Anforderung Überprüfung ASP.NET-Funktionen](#_Toc318097379).
- Die `<modules>` Element von der `<system.webServer>` Abschnitt jedoch nicht enthalten eine `runAllManagedModulesForAllRequests` Attribut. (Der Standardwert ist "false".) Dies bedeutet, dass wenn Sie eine Version von IIS 7 verwenden, die nicht auf SP1 aktualisiert wurde, können Sie Probleme mit routing in einen neuen Standort haben. Weitere Informationen finden Sie unter [systemeigene Unterstützung in IIS 7 für das ASP.NET-Routing](#Native_Support_In_IIS7_For_ASPNET_Routine).

Diese Änderungen wirken sich nicht auf vorhandene Anwendungen aus. Sie repräsentieren jedoch möglicherweise einen Unterschied im Verhalten zwischen vorhandenen Websites und neuen Websites, die Sie für ASP.NET 4.5 mit dem neuen Vorlagen zu erstellen.

<a id="Native_Support_In_IIS7_For_ASPNET_Routine"></a>
### <a name="native-support-in-iis-7-for-aspnet-routing"></a>Systemeigene Unterstützung in IIS 7 für das ASP.NET-Routing

Dies ist nicht als solche eine Änderung an ASP.NET und nur eine Änderung in den Vorlagen für neue Websiteprojekte, die Sie betreffen können, wenn Sie eine Version von IIS 7 arbeiten, die nicht das SP1-Update angewendet wurde.

In ASP.NET können Sie die folgende Konfigurationseinstellung von Anwendungen zur Unterstützung des routing hinzufügen:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample34.xml?highlight=3)]

Wenn **RunAllManagedModulesForAllRequests** ist "true", eine URL wie `http://mysite/myapp/home` wechselt zu ASP.NET, auch wenn keine *aspx*, *MVC*, oder ähnliche Erweiterung auf der URL.

Ein Update, das IIS 7 erfolgte macht die **RunAllManagedModulesForAllRequests** unnötige festlegen und ASP.NET-ROUTING nativ unterstützt. (Informationen zum Update finden Sie unter den Microsoft Support-Artikel [ein Update ist verfügbar, können bestimmte IIS 7.0 oder IIS 7.5-Handler behandelt Anforderungen, deren URLs nicht mit einem Punkt enden](https://support.microsoft.com/kb/980368).)

Wenn Ihre Website auf IIS 7 ausgeführt wird und wenn IIS aktualisiert wurde, Sie nicht festlegen müssen **RunAllManagedModulesForAllRequests** auf "true". Tatsächlich ist Festlegung auf "true" nicht empfohlen, da er fügt unnötige Verarbeitungsaufwand auf Anforderung. Wenn diese Einstellung "true" ist, alle Anforderungen, einschließlich derjenigen für *htm*, *jpg*, und andere statischen Dateien auch Durcharbeiten der ASP.NET-Pipeline für die Anforderung.

Wenn Sie eine neue ASP.NET 4.5-Website unter Verwendung der Vorlagen, die in Visual Studio 2012 RC bereitgestellt werden erstellen, die Konfiguration für die Website enthält keine der **RunAllManagedModulesForAllRequests** Einstellung. Dies bedeutet, dass standardmäßig die Einstellung "false" ist.

Wenn Sie dann die Website unter Windows 7 ohne installierte Service Pack 1 ausführen, wird IIS 7 nicht das erforderliche Update enthalten. Daher routing funktioniert nicht, und sehen Sie Fehler. Wenn Sie ein Problem vorliegt, in dem routing funktioniert nicht, können Sie entweder die folgenden Aktionen ausführen:

- Aktualisieren Sie Windows 7 auf SP1, einschließlich der IIS 7 Update hinzugefügt wird.
- Installieren Sie das Update, das beschrieben ist, in der Microsoft Support-Artikel, die zuvor genannten.
- Legen Sie **RunAllManagedModulesForAllRequests** auf "true" in "Web.config"-Datei von dieser Website. Beachten Sie, dass dadurch ein gewisser Aufwand Anforderungen hinzugefügt wird.

<a id="_Toc318097397"></a>
### <a name="html-editor"></a>HTML-Editor

<a id="_Toc318097398"></a>
#### <a name="smart-tasks"></a>Intelligente Aufgaben

In der Entwurfsansicht sind komplexe Eigenschaften von Serversteuerelementen häufig Dialogfeldern und Assistenten, um es einfach, sie zu versetzen können zugeordnet. Sie können z. B. einen speziellen (Dialogfeld) verwenden, um eine Datenquelle hinzugefügt eine *Repeater* steuern, oder fügen Sie Spalten aus, die eine *GridView* Steuerelement.

Diese Art von – Hilfe zur Benutzeroberfläche für komplexe Eigenschaften sind jedoch nicht in der Quellansicht verfügbar wurde. Aus diesem Grund stellt Visual Studio 11 intelligenten Aufgaben für die Datenquellensicht an. Intelligente Aufgaben sind kontextabhängige Tastenkombinationen für häufig verwendete Funktionen in c# und Visual Basic-Editoren.

Für ASP.NET Web Forms-Steuerelemente auftreten intelligente Aufgaben für Servertags als kleine Symbol beim die Einfügemarke innerhalb des Elements befindet:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.png)

Die Smarttask erweitert wird, wenn Sie auf das Symbol, oder drücken Sie STRG +. (Punkt), wie in der Code-Editor. Sie zeigt dann die Tastenkombinationen, die in der Entwurfsansicht die intelligente Aufgaben ähnlich sind.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image7.png)

Der intelligente Task in der obigen Abbildung zeigt z. B. die Optionen GridView-Aufgaben. Bei Auswahl von Spalten bearbeiten, wird das folgende Dialogfeld angezeigt:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image8.png)

Füllen in das Dialogfeld legt die gleichen Eigenschaften, können Sie in der Entwurfsansicht festlegen. Wenn Sie auf "OK" klicken, wird das Markup für das Steuerelement mit den neuen Einstellungen aktualisiert:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image9.png)

<a id="_Toc318097399"></a>
#### <a name="wai-aria-support"></a>WAI ARIA-Unterstützung

Schreiben zugegriffen werden Websites wird immer wichtiger. Die [WAI ARIA Eingabehilfen standard](http://www.w3.org/WAI/intro/aria) definiert, wie Entwickler zugänglich Websites schreiben soll. Dieser Standard ist jetzt vollständig in Visual Studio unterstützt.

Z. B. die *Rolle* Attribut verfügt jetzt über vollständiger IntelliSense-Unterstützung:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image10.png)

Der Standard WAI ARIA führt außerdem Attribute, die mit dem Präfix *ARIA-* , mit deren Hilfe Sie die Semantik einer HTML5-Dokument hinzufügen. Visual Studio unterstützt diese auch vollständig *ARIA-* Attribute:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image11.png) ![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image12.png)

<a id="_Toc318097400"></a>
#### <a name="new-html5-snippets"></a>Neue HTML5-Ausschnitte

Um schneller und einfacher schreiben häufig verwendete HTML5-Markup zu vereinfachen, umfasst Visual Studio eine Reihe von Ausschnitten. Ein Beispiel ist der video-Codeausschnitt an:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image13.png)

Um den Ausschnitt aufzurufen, drücken Sie Tab, zwei Mal, wenn das Element in IntelliSense aktiviert ist:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image14.png)

Dies erzeugt einen Ausschnitt aus, dem Sie anpassen können.

<a id="_Toc318097401"></a>
#### <a name="extract-to-user-control"></a>In Benutzersteuerelement extrahieren

In großen Webseiten kann es eine gute Idee, die einzelnen Teile in Benutzersteuerelemente verschoben werden. Diese Form der Umgestaltung kann die Lesbarkeit der Seite zu verbessern und die Seitenstruktur vereinfachen kann.

Um dies zu vereinfachen, machen Sie Web Forms-Seiten in der Quellansicht bearbeiten, können Sie nun Text auf einer Seite auswählen, der rechten Maustaste darauf und wählen Sie dann auf "in Benutzersteuerelement extrahieren":

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.jpg)

<a id="_Toc318097402"></a>
#### <a name="intellisense-for-code-nuggets-in-attributes"></a>IntelliSense für Code Brocken in Attributen

Visual Studio bereitgestellt immer IntelliSense für serverseitigen Code Brocken in einer beliebigen Seite oder Steuerelement. Visual Studio enthält jetzt IntelliSense für Code Brocken in HTML-Attribute als auch an.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image15.png)

Dies erleichtert die Datenbindungsausdrücke erstellen:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image16.png)

<a id="_Toc318097403"></a>
#### <a name="automatic-renaming-of-matching-tag-when-you-rename-an-opening-or-closing-tag"></a>Automatische Umbenennung von übereinstimmenden Tag, wenn Sie ein Start- oder Endtag umbenennen

Wenn Sie ein HTML-Element umbenennen (beispielsweise ändern Sie eine *Div* Tag werden eine *Header* Tag), wird die entsprechende öffnen oder das Endtag ändert sich ebenfalls in Echtzeit.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image17.png)

Dadurch wird den Fehler vermieden, in dem Sie vergessen haben, ändern ein Endtag, oder ändern die falsche Datei.

<a id="_Toc318097404"></a>
#### <a name="event-handler-generation"></a>Handler-Generierung von Ereignissen

Visual Studio enthält nun Funktionen in der Quellansicht helfen Ihnen beim Schreiben von Ereignishandlern und binden diese manuell. Wenn Sie ein Ereignis namens in der Quellansicht bearbeiten, zeigt IntelliSense &lt;neues Ereignis erstellen&gt;, der einen Ereignishandler im Code der Seite erstellen werden, die über die richtige Signatur:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.jpg)

Standardmäßig wird der Ereignishandler die Steuerelement-ID für den Namen der Methode für die Ereignisbehandlung verwendet:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.jpg)

Der resultierende Ereignishandler wird (in diesem Fall in c#) wie folgt aussehen:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image18.png)

<a id="_Toc318097405"></a>
#### <a name="smart-indent"></a>Intelligenten Einzug

Beim Drücken der EINGABETASTE, klicken Sie in eine leere HTML-Element wird der Editor die Einfügemarke an der richtigen Stelle platzieren:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image19.png)

Wenn durch Drücken der EINGABETASTE an diesem Speicherort ist das Endtag weiter nach unten verschoben und entsprechend das Starttag eingezogen. Die Einfügemarke wird auch mit Einzug dargestellt:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image20.png)

<a id="_Toc318097406"></a>
#### <a name="auto-reduce-statement-completion"></a>Anweisungsvervollständigung Auto / reduzieren

Die IntelliSense-Liste in Visual Studio jetzt Filter auf Grundlage, was Sie eingeben, sodass er nur relevante Optionen angezeigt:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image21.png)

IntelliSense auch Filter auf die Anfangsbuchstaben der einzelnen Wörter in der IntelliSense-Liste basieren. Wenn Sie "dl" eingeben, werden z. B. dl und Asp: DataList angezeigt:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image22.png)

Dieses Feature ermöglicht es schneller Anweisungsvervollständigung für bekannte Elemente abzurufen.

<a id="_Toc318097407"></a>
### <a name="javascript-editor"></a>JavaScript-Editor

Der JavaScript-Editor in Visual Studio 2012 Release Candidate ist vollständig neu, und die Erfahrung der Arbeit mit JavaScript in Visual Studio erheblich verbessert.

<a id="_Toc318097408"></a>
#### <a name="code-outlining"></a>Code outlining

Gliederungsbereiche werden jetzt automatisch erstellt, für alle Funktionen, sodass Sie Teile der Datei zu reduzieren, die für Ihre aktuelle Fokus relevant sind.

<a id="_Toc318097409"></a>
#### <a name="brace-matching"></a>Zugehörige Klammer

Wenn Sie eine öffnende oder schließende geschweifte Klammer die Einfügemarke auferlegt, markiert der Editor mit einem an.

<a id="_Toc318097410"></a>
#### <a name="go-to-definition"></a>Zur Definition wechseln

Gehe zu Definition (Befehl) können Sie die Quelle für eine Funktion oder Variable springen.

<a id="_Toc318097411"></a>
#### <a name="ecmascript5-support"></a>ECMAScript5-Unterstützung

Der Editor unterstützt die neue Syntax und die APIs im ECMAScript5, die neueste Version des Standards, die die JavaScript-Sprache beschreibt.

<a id="_Toc318097412"></a>
#### <a name="dom-intellisense"></a>DOM IntelliSense

IntelliSense für DOM-APIs wurde verbessert, mit Unterstützung für viele neue HTML5-APIs, darunter *QuerySelector*, dokumentübergreifendes messaging, DOM-Speicher und *Zeichenbereich*. DOM-IntelliSense wird jetzt eine einzelne einfache JavaScript-Datei, statt von einer systemeigenen Bibliothek Typdefinition gesteuert. Dies vereinfacht das Erweitern oder ersetzen.

<a id="_Toc318097413"></a>
#### <a name="vsdoc-signature-overloads"></a>Überladungen für das VSDOC-Signatur

Ausführliche IntelliSense Kommentare können jetzt für separate Überladungen der JavaScript-Funktionen deklariert werden, mithilfe des neuen  *&lt;Signatur&gt;*  Element, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample35.cs)]

<a id="_Toc318097414"></a>
#### <a name="implicit-references"></a>Impliziten verweisen

Sie können jetzt eine zentrale Liste JavaScript-Dateien hinzufügen, die implizit in der Liste der Dateien aufgenommen werden, dass alle angegebenen JavaScript-Datei oder der Block Verweise, d. h. Sie IntelliSense für ihre Inhalte erhalten. Beispielsweise können Sie jQuery-Dateien hinzufügen, um die zentralen Liste der Dateien, und Sie erhalten IntelliSense für jQuery-Funktionen in einen beliebigen TextBlock JavaScript-Datei, ob Sie explizit verwiesen haben (mithilfe von / / / &lt;Verweis /&gt;) oder nicht.

<a id="_Toc318097415"></a>
### <a name="css-editor"></a>CSS-Editor

<a id="_Toc318097416"></a>
#### <a name="auto-reduce-statement-completion"></a>Anweisungsvervollständigung Auto / reduzieren

Die IntelliSense-Liste für die CSS-jetzt Filter auf Grundlage der CSS-Eigenschaften und Werte, die durch das ausgewählte Schema unterstützt.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image23.png)

IntelliSense unterstützt außerdem die Groß-/Kleinschreibung Suchvorgänge Titel:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image24.png)

<a id="_Toc318097417"></a>
#### <a name="hierarchical-indentation"></a>Hierarchischer Einzug

CSS-Editor verwendet Einzug zur Anzeige hierarchischer Regeln, wodurch Sie erhalten einen Überblick darüber, wie die kaskadierenden Regeln logisch organisiert sind. Im folgenden Beispiel die #list Auswahl ist kaskadierende ein untergeordnetes Element der Liste und wird daher eingerückt.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image25.png)

Das folgende Beispiel zeigt eine komplexere Vererbung:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image26.png)

Der Einzug der Regel richtet sich nach ihren übergeordneten Regeln. Hierarchischer Einzug ist standardmäßig aktiviert, aber Sie können sie die Optionen (Dialogfeld) (Extras, Optionen in der Menüleiste) deaktivieren:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image27.png)

<a id="_Toc318097418"></a>
#### <a name="css-hacks-support"></a>CSS hacks Unterstützung

Analyse von Hunderten von realen CSS-Dateien zeigt, dass CSS Hackerangriffe häufig werden, und Visual Studio jetzt die am häufigsten verwendeten Konfigurationen unterstützt. Diese Unterstützung umfasst, IntelliSense und zur Validierung des Sterns (\*) und Unterstrich (\_)-Eigenschaft Hackerangriffe:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image28.png)

Typische Selektor Hackerangriffe werden ebenfalls unterstützt, sodass hierarchischer Einzug beibehalten wird, selbst wenn sie angewendet werden. Eine typische Selektor Hack verwendet, um Internet Explorer 7-Ziel ist eine-Auswahl durch voranstellen  *\*: First-Child + html*. Mit dieser Regel wird die hierarchischen Einzug verwalten:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image29.png)

<a id="_Toc318097419"></a>
#### <a name="vendor-specific-schemas--moz---webkit"></a>Hersteller bestimmte Schemas (' - Moz - Webkit-)

CSS3 führt viele Eigenschaften, die von verschiedenen Browsern zu unterschiedlichen Zeiten implementiert wurden. Dies erzwungen zuvor Entwicklern, Code für bestimmte Browser mit herstellerspezifischen-Syntax. Dieser Browser-spezifische Eigenschaften sind nun in IntelliSense enthalten.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image30.png)

<a id="_Toc318097420"></a>
#### <a name="commenting-and-uncommenting-support"></a>Kommentare und uncommenting-Unterstützung

Sie können jetzt Kommentar kennzeichnen und auskommentieren CSS-Regeln, die mit den gleichen Tastenkombinationen, die Sie im Codeeditor (STRG + K, C, um den Kommentar und STRG + K, Sie kommentieren) verwenden.

<a id="_Toc318097421"></a>
#### <a name="color-picker"></a>Farbauswahl

In früheren Versionen von Visual Studio umfasste IntelliSense für Attribute farbbezogene ein Dropdown-Werteliste benannte Farbe. Diese Liste wurde durch eine voll ausgestattete Farbauswahl ersetzt.

Wenn Sie einen Farbwert eingeben, wird die Farbauswahl wird automatisch angezeigt und zeigt eine Liste der zuvor verwendeten Farben, gefolgt von einer Standardfarbpalette. Sie können mit der Maus oder Tastatur eine Farbe auswählen.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image31.png)

Die Liste kann in eine vollständige Farbauswahl erweitert werden. Die Auswahl einer können Sie den alpha-Kanal zu steuern, indem automatisch jede Farbe in RGBA konvertiert, wenn Sie die Deckkraft Schieberegler bewegen:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image32.png)

<a id="_Toc318097422"></a>
#### <a name="snippets"></a>Codeausschnitte

Codeausschnitte in der CSS-Editor können sie einfacher und schneller mit Formatvorlagen für browserübergreifende erstellen. Viele CSS3-Eigenschaften, die Browser-spezifischen Einstellungen erfordern haben jetzt in Ausschnitten ein Rollback ausgeführt wurde.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image33.png)

CSS-Codeausschnitten unterstützt erweiterte Szenarien (z. B. CSS3-Medienabfragen) durch Eingabe der am Symbol (@), es zeigt die IntelliSense-Liste.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image34.png)

Bei Auswahl @media Wert, und drücken Sie Tab, CSS-Editor wird der folgenden Ausschnitts:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.jpg)

Wie bei Ausschnitte für Code, können Sie eigene CSS-Ausschnitte erstellen.

<a id="_Toc318097423"></a>
#### <a name="custom-regions"></a>Benutzerdefinierte Regionen

Mit dem Namen Codebereiche, die bereits im Code-Editor verfügbar sind, stehen nun aus, für die Bearbeitung von CSS. Dadurch können Sie die zugehörigen Stilblöcken einfach zu gruppieren.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image35.png)

Wenn ein Bereich reduziert wird der Name des Bereichs angezeigt:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image36.png)

<a id="_Toc318097424"></a>
### <a name="page-inspector"></a>Seitenprüfung

Seitenprüfung ist ein Tool, rendert eine Webseite (HTML, Web Forms, ASP.NET MVC oder Web Pages) in der Visual Studio-IDE und können Sie den Quellcode einfügen und die resultierende Ausgabe untersuchen. Für die ASP.NET-Seiten können mit Page Inspector zu bestimmen, welche serverseitigen Code das HTML-Markup erzeugt hat, das an den Browser gerendert wird.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image37.png)

Weitere Informationen zu Page Inspector finden Sie unter den folgenden Lernprogrammen:

- Mit Page Inspector in [ASP.NET MVC](../mvc/overview/views/using-page-inspector-in-aspnet-mvc.md)
- Mit Page Inspector in [ASP.NET-Web Forms](../web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project.md)

<a id="_Toc318097425"></a>
### <a name="publishing"></a>Veröffentlichen

<a id="_Toc318097426"></a>
#### <a name="publish-profiles"></a>Veröffentlichungsprofile

In Visual Studio 2010 Veröffentlichung von Informationen für Webanwendungsprojekte ist nicht in der Versionskontrolle gespeichert und dient nicht zur Freigabe an andere Benutzer. In Visual Studio 2012 Release Candidate (RC) wurde das Format des Veröffentlichungsprofils geändert. Es wurde ein Artefakt Team, und es ist nun einfach von Builds basierend auf MSBuild nutzen. Build-Konfigurationsinformationen ist im Dialogfeld "Veröffentlichen", sodass Sie Projektmappenbuild-Konfigurationen vor der Veröffentlichung problemlos wechseln können.

Veröffentlichen von Profilen werden im Ordner "PublishProfiles" gespeichert. Der Speicherort des Ordners hängt Programmiersprache, die Sie verwenden:

- C#: Properties\PublishProfiles
- Visual Basic: Meine Project\PublishProfiles

Jedes Profil ist eine MSBuild-Datei. Während der Veröffentlichung, wird diese Datei in MSBuild-Projektdatei importiert. In Visual Studio 2010, wenn Sie Änderungen an den Prozess veröffentlichen oder ein Paket vornehmen möchten stehen Ihnen Ihre Anpassungen in eine Datei namens gelegt **Projektname**. wpp.targets. Dies wird weiterhin unterstützt, aber Sie können jetzt Ihre Anpassungen im Veröffentlichungsprofil selbst einfügen. Auf diese Weise werden die Anpassungen nur für dieses Profil verwendet werden.

Sie können nun auch nutzen Veröffentlichungsprofile von MSBuild. Zu diesem Zweck verwenden Sie beim Erstellen des Projekts den folgenden Befehl aus:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample36.cmd)]

Der Wert project.csproj ist der Pfad des Projekts und ProfileName ist der Name des Profils, das veröffentlichen. Alternativ können Sie anstelle der Übergabe der Profilname für den *"publishprofile"* -Eigenschaft, können Sie den vollständigen Pfad übergeben, um das Veröffentlichungsprofil.

<a id="_Toc318097427"></a>
#### <a name="aspnet-precompilation-and-merge"></a>ASP.NET-Vorkompilierung und Zusammenführen

Für Webanwendungsprojekte fügt Visual Studio 2012 Release Candidate eine Option auf der Eigenschaftenseite für Web packen/veröffentlichen, mit dem Sie die vorkompilieren und Ihrer Website-Inhalte beim Veröffentlichen oder Paket, das Projekt zusammenführen. Um diese Optionen nicht angezeigt, mit der rechten Maustaste des Projekts im Projektmappen-Explorer, wählen Sie Eigenschaften, und wählen Sie dann auf der Eigenschaftenseite Web packen/veröffentlichen. Die folgende Abbildung zeigt die Precompile diese Anwendung, bevor die Veröffentlichungsoption auswählen.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.jpg)

Wenn diese Option aktiviert ist, kompiliert Visual Studio die Anwendung, sobald Sie veröffentlichen oder ein Paket die Webanwendung vor. Wenn Sie steuern, wie die Website vorkompiliert ist, oder wie Assemblys zusammengeführt werden sollen, klicken Sie auf die Schaltfläche "Erweitert", um diese Optionen zu konfigurieren.

<a id="_Toc318097428"></a>
### <a name="iis-express"></a>IIS Express

Der Standardwebserver für Tests Webprojekte in Visual Studio ist jetzt IIS Express. Visual Studio Development Server ist eine Option für lokalen Webserver während der Entwicklung, aber IIS Express ist nun der empfohlenen Server. Die Verwendung von IIS Express in Visual Studio 11 Beta ist sehr ähnlich, die sie in Visual Studio 2010 SP1 verwenden.

<a id="_Toc318097429"></a>
## <a name="disclaimer"></a>Haftungsausschluss

Dies ist ein vorläufiges Dokument, das vor dem kommerziellen Release der beschriebenen Software ggf. erheblich geändert wird.

Die Informationen in diesem Dokument repräsentieren den aktuellen Standpunkt der Microsoft Corporation zu den erörterten Problemen zum Veröffentlichungstermin. Da Microsoft auf das Ändern von Marktlagen reagieren muss, ist das Dokument keinesfalls als Verpflichtung von Microsoft zu interpretieren, und Microsoft kann die Genauigkeit der Informationen nicht über den Zeitpunkt der Veröffentlichung hinaus garantieren.

Dieses Whitepaper dient ausschließlich Informationszwecken. MICROSOFT ÜBERNIMMT KEINE AUSDRÜCKLICHE, STILLSCHWEIGENDE ODER AUS GESETZ ERWACHSENDE GARANTIE IN BEZUG AUF DIE INFORMATIONEN IN DIESEM DOKUMENT.

Die Benutzer/innen sind verpflichtet, sich an alle anwendbaren Urheberrechtsgesetze zu halten. Unabhängig von der Anwendbarkeit der entsprechenden Urheberrechtsgesetze darf ohne ausdrückliche schriftliche Erlaubnis der Microsoft Corporation kein Teil dieses Dokuments für irgendwelche Zwecke vervielfältigt oder in einem Datenempfangssystem gespeichert oder darin eingelesen werden, unabhängig davon, auf welche Art und Weise oder mit welchen Mitteln (elektronisch, mechanisch, durch Fotokopieren, Aufzeichnen usw.) dies geschieht.

Microsoft Corporation kann Inhaber von Patenten oder Patentanträgen, Marken, Urheberrechten oder anderem geistigen Eigentum sein, die den Inhalt dieses Dokuments betreffen. Die Bereitstellung dieses Dokuments gewährt keinerlei Lizenzrechte an diesen Patenten, Marken, Urheberrechten oder anderem geistigen Eigentum, es sei denn, dies wurde ausdrücklich durch einen schriftlichen Lizenzvertrag mit der Microsoft Corporation vereinbart.

Sofern nicht anders angegeben, einer der Firmen, Organisationen, Produkte, Domänennamen, e-Mail-Adressen, Logos, Personen, Orte und Ereignisse in diesem Dokument dargestellten sind frei erfunden und keine Zuordnung zu echten Unternehmen, Organisation, Produkt, Domänennamen, e-Mail Adresse Logo "," Person "," Ort "oder" Ereignis dient, noch ableitbar.

© 2012 Microsoft Corporation. Alle Rechte vorbehalten.

Microsoft und Windows sind eingetragene Marken oder Marken der Microsoft Corporation in den USA und/oder anderen Ländern.

Die in diesem Dokument erwähnten Namen von Unternehmen und Produkten können Marken der jeweiligen Eigentümer sein.
