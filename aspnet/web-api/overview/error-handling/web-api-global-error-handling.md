---
uid: web-api/overview/error-handling/web-api-global-error-handling
title: Globale Fehlerbehandlung in ASP.NET Web-API 2 | Microsoft-Dokumentation
author: davidmatson
description: ''
ms.author: riande
ms.date: 02/03/2014
ms.assetid: bffd7863-f63b-4b23-a13c-372b5492e9fb
msc.legacyurl: /web-api/overview/error-handling/web-api-global-error-handling
msc.type: authoredcontent
ms.openlocfilehash: a52c2a1589327421b7f498ff551145676c80e3e8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835788"
---
<a name="global-error-handling-in-aspnet-web-api-2"></a>Globale Fehlerbehandlung in ASP.NET Web-API 2
====================
durch [David Matson](https://github.com/davidmatson), [Rick Anderson](https://github.com/Rick-Anderson)

Heute ist gibt es keine einfache Möglichkeit in Web-API, um zu protokollieren oder Behandeln von Fehlern Global. Eine nicht behandelten Ausnahmen verarbeitet werden können, über [Ausnahmefilter](exception-handling.md), aber es gibt eine Anzahl von Fällen, in denen Ausnahmefilter nicht verarbeiten können. Zum Beispiel:

1. Von controllerkonstruktoren ausgelöste Ausnahmen.
2. Von meldungshandlern ausgelöste Ausnahmen.
3. Während des Routings ausgelöste Ausnahmen.
4. Während der Serialisierung von Antwortinhalten ausgelöste Ausnahmen.

Wir bieten eine einfache, konsistente Möglichkeit, sich anzumelden und (sofern möglich) zu behandeln möchten diese Ausnahmen. 

Es gibt zwei wichtige Fälle für die Ausnahmebehandlung, der Fall, in dem wir senden eine Fehlerantwort sind und der Fall, in dem alle wir tun können, Log die Ausnahme. Ein Beispiel für letzteren Fall ist, wenn eine Ausnahme ausgelöst wird, in der Mitte der streaming-Inhalts der Antwort. In diesem Fall ist es zu spät, eine neue Antwortnachricht gesendet werden, da er den Statuscode, Header und Teil des Inhalts bereits über das Netzwerk, durchgeführt haben, damit wir einfach die Verbindung abzubrechen. Obwohl die Ausnahme kann nicht verarbeitet werden, um eine neue Antwortnachricht zu erzeugen, unterstützen wir weiterhin Protokollierung der Ausnahme. In Fällen, in denen einen Fehler erkannt werden können, können wir eine entsprechende Fehlerantwort zurückgeben, wie im folgenden dargestellt:

[!code-csharp[Main](web-api-global-error-handling/samples/sample1.cs?highlight=6)]

### <a name="existing-options"></a>Vorhandene Optionen

Zusätzlich zu [Ausnahmefilter](exception-handling.md), [message-Handler](../advanced/http-message-handlers.md) kann heute verwendet werden, um alle auf 500-Antworten zu beobachten, aber auf diese Antworten ist schwierig, da ihnen Kontextdaten zu der ursprüngliche Fehler fehlt. Meldungshandler haben auch einige der dieselben Einschränkungen wie die Ausnahmefilter in Bezug auf die Fälle, die sie behandeln können. Während der Web-API-Ablaufverfolgungsinfrastruktur verfügen, die möglichen Fehlerzustände erfasst ist die Infrastruktur für ereignisablaufverfolgung wird zu Diagnosezwecken und nicht entwickelt und für die Ausführung in produktionsumgebungen. Globalen Ausnahmeklassen Fehlerbehandlung und-Protokollierung muss Dienste, die während der Produktion ausführen können und in bestehende überwachungslösungen angeschlossen werden (z. B. [ELMAH](https://code.google.com/p/elmah/) ).

### <a name="solution-overview"></a>Übersicht über die Lösung

 Wir bieten zwei neue Benutzer vor Ort austauschbaren Dienste [' iexceptionlogger '](../releases/whats-new-in-aspnet-web-api-21.md) und IExceptionHandler, zu melden und behandeln unbehandelte Ausnahmen. Die Dienste sind sehr ähnlich, zwei Hauptunterschiede:

1. Wir unterstützen die Registrierung der Protokollierungen für mehrere Ausnahmen aber nur einen einzigen Ausnahmehandler.
2. Protokollierungen Ausnahmen erhalten immer aufgerufen, selbst wenn wir sind dabei, die Verbindung abzubrechen. Ausnahmehandler nur aufgerufen werden, wenn wir weiterhin wählen Sie die Response-Nachricht senden können.

Beide Dienste bieten Zugriff auf einen Ausnahmekontext mit relevanten Informationen an der Stelle, wo die Ausnahme erkannt wurde, insbesondere die [HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage(v=vs.110).aspx), [HttpRequestContext](https://msdn.microsoft.com/library/system.web.http.controllers.httprequestcontext(v=vs.118).aspx), wird die ausgelöste Ausnahme und die Quelle der Ausnahme (Details unten).

### <a name="design-principles"></a>Entwurfsprinzipien

1. **Keine aktuellen Änderungen** , da diese Funktion in eine Nebenversion, eine wichtige Einschränkung, die Auswirkungen auf die Lösung ist, die es keine grundlegenden Änderungen, entweder in den Typ der Verträge oder Verhalten hinzugefügt wird. Diese Einschränkung ausgeschlossen wird einige Bereinigungsschritte an, die wir im Hinblick auf vorhandene Catch-Blöcke, die Aktivieren von Ausnahmen in mit 500 Antworten getan haben möchten. Diese zusätzliche Bereinigung ist etwas, das wir für eine nachfolgende Hauptversion erwägen. Ist dies wichtig, Sie stimmen Sie dafür auf [ASP.NET Web-API-uservoice](http://aspnet.uservoice.com/forums/147201-asp-net-web-api/suggestions/5451321-add-flag-to-enable-iexceptionlogger-and-iexception).
2. **Aufrechterhaltung der Konsistenz mit Web-API erstellt** Filterpipeline Web-API ist eine hervorragende Möglichkeit, querschnittliche Belange mit der Flexibilität der Anwendung der Logik an eine spezifische Aktion, Controller-spezifische oder im globalen Gültigkeitsbereich zu behandeln. Filter, einschließlich der Ausnahmefilter, haben immer Aktion und Controller-Kontexten, selbst wenn auf den globalen Bereich registriert. Dass der Vertrag ist sinnvoll für Filter, aber dies bedeutet, dass die Ausnahmefilter auch global bereichsbezogenen werden, eine gute Wahl für eine Ausnahme, die Behandlung von Fällen, z. B. Ausnahmen von meldungshandlern, wenn kein Kontext Aktions- oder Controllerebene sind nicht vorhanden ist. Wenn wir verwenden die verfügbaren Filter für die Ausnahmebehandlung flexible Bereichsdefinition möchten, benötigen wir weiterhin Ausnahmefilter. Aber wenn wir Ausnahme außerhalb einer Controllerkontext behandeln müssen, wir benötigen auch ein separates Konstrukt für vollständigen globalen Fehlerbehandlung (etwas ohne den Controller und das Aktionsergebnis Kontext Einschränkungen).

### <a name="when-to-use"></a>Verwendung

- Protokollierungen Ausnahmen sind die Lösung für alle nicht behandelten Ausnahme von Web-API angezeigt.
- Ausnahmehandler sind die Lösung für alle möglichen Antworten auf nicht behandelte Ausnahmen abgefangen, die von Web-API anpassen.
- Ausnahmefilter sind die einfachste Lösung für die Verarbeitung der Teilmenge, die nicht behandelte Ausnahmen im Zusammenhang mit der eine bestimmte Aktion oder einen Controller.

### <a name="service-details"></a>Dienstdetails

 Die Ausnahme Protokollierung und -Handler die Dienstschnittstellen sind einfache Async-Methoden, die die jeweiligen Kontexte dauert: 

[!code-csharp[Main](web-api-global-error-handling/samples/sample2.cs)]

 Wir bieten auch die Basisklassen für diese beiden Schnittstellen. Überschreiben der Methoden Core, (synchron oder asynchron), ist alles, was erforderlich ist, um zu protokollieren oder verarbeiten, auf die empfohlene Häufigkeit. Für die Protokollierung der `ExceptionLogger` Basisklasse wird sichergestellt, dass die zentrale Protokollierung-Methode nur einmal für jede Ausnahme aufgerufen wird (selbst wenn Sie es später noch Mal verteilt weiter die Aufrufliste nach oben und erneut erfasst). Die `ExceptionHandler` Basisklasse aufrufen, werden die wichtigsten Behandlung Methode nur Ausnahmen am Anfang der Aufrufliste, ignoriert der Vorgängerversion, die geschachtelte catch-Blocks. (Vereinfachte Versionen von diesen Basisklassen finden Sie im Anhang unten.) Beide `IExceptionLogger` und `IExceptionHandler` erhalten Sie Informationen zur Ausnahme über einen `ExceptionContext`.

[!code-csharp[Main](web-api-global-error-handling/samples/sample3.cs)]

Wenn das Framework eine ausnahmeprotokollierung oder einen Ausnahmehandler aufgerufen wird, geben sie immer eine `Exception` und `Request`. Hier sind auch immer bereitgestellt, mit Ausnahme der Komponententests eine `RequestContext`. Sie erhalten nur selten eine `ControllerContext` und `ActionContext` (nur, wenn die vom Catch-Block für Ausnahmefilter aufgerufen wird). Sie erhalten nur sehr selten eine `Response`(nur in bestimmten IIS-Fällen, wenn in der Mitte beim Schreiben der Antwort). Beachten Sie, dass einige dieser Eigenschaften möglicherweise `null` es obliegt der Consumer zu suchende `null` vor dem Zugriff auf Mitglieder der Exception-Klasse.`CatchBlock` ist eine Zeichenfolge, der angibt, welche Catch-Block die Ausnahme gesehen haben. Die Catch-Block Zeichenfolgen lauten wie folgt aus:

- HttpServer (SendAsync-Methode)
- HttpControllerDispatcher (SendAsync-Methode)
- HttpBatchHandler (SendAsync-Methode)
- Iexceptionfilter (des "ApiController" Verarbeitung der Ausnahme Filterpipeline in ExecuteAsync).
- OWIN-Host:

    - HttpMessageHandlerAdapter.BufferResponseContentAsync (for buffering output)
    - HttpMessageHandlerAdapter.CopyResponseContentAsync (for streaming output)
- Web-Host:

    - HttpControllerHandler.WriteBufferedResponseContentAsync (for buffering output)
    - HttpControllerHandler.WriteStreamedResponseContentAsync (für streaming-Ausgabe)
    - HttpControllerHandler.WriteErrorResponseContentAsync (für Fehler in der Fehlerbehebung im Modus für gepufferte Ausgabe)

Die Liste der Zeichenfolgen der Catch-Block ist auch über statische, schreibgeschützte Eigenschaften verfügbar. (Die Core-Catch-Block Zeichenfolge befinden sich auf die statische ExceptionCatchBlocks; im weiteren Verlauf angezeigt werden, auf eine statische Klasse jedes für OWIN und Web-Host).`IsTopLevelCatchBlock` ist hilfreich für die folgenden des empfohlenen Musters der Ausnahmebehandlung nur am Anfang der Aufrufliste. Anstatt Ausnahmen in 500-Antworten an jeder Stelle, die ein geschachtelten Catch-Block auftritt, können ein Ausnahmehandler Ausnahmen weitergegeben werden, bis sie sind im Begriff, die vom Host angezeigt werden.

Zusätzlich zu den `ExceptionContext`, eine Protokollierung Ruft eine weitere Information über die vollständige `ExceptionLoggerContext`:

[!code-csharp[Main](web-api-global-error-handling/samples/sample4.cs)]

Die zweite Eigenschaft, `CanBeHandled`, ermöglicht es einem Protokollierungsmodul eine Ausnahme zu ermitteln, die nicht behandelt werden kann. Wenn die Verbindung wird abgebrochen wird oder keine neue Antwortnachricht gesendet werden kann, die Protokollierungen werden aufgerufen, aber wird des Handlers ***nicht*** aufgerufen werden, und die Protokollierungen dieses Szenario der von dieser Eigenschaft identifizieren können.

Im weiteren der `ExceptionContext`, ein Handler Ruft eine weitere Eigenschaft sie, auf das vollständige festlegen können `ExceptionHandlerContext` zur Behandlung von Ausnahmen:

[!code-csharp[Main](web-api-global-error-handling/samples/sample5.cs)]

Ein Ausnahmehandler gibt an, dass es eine Ausnahme, indem behandelt wurde die `Result` Eigenschaft, um ein Aktionsergebnis (z. B. eine [ExceptionResult](https://msdn.microsoft.com/library/system.web.http.results.exceptionresult(v=vs.118).aspx), [InternalServerErrorResult](https://msdn.microsoft.com/library/system.web.http.results.internalservererrorresult(v=vs.118).aspx), [ StatusCodeResult](https://msdn.microsoft.com/library/system.web.http.results.statuscoderesult(v=vs.118).aspx), oder ein benutzerdefiniertes Ergebnis). Wenn die `Result` -Eigenschaft ist null, der die Ausnahme nicht behandelt wird und die ursprüngliche Ausnahme erneut ausgelöst werden.

Für Ausnahmen, die am oberen Rand der Aufrufliste haben wir einen zusätzlichen Schritt, um sicherzustellen, dass die Antwort für API-Aufrufer geeignet ist. Die Ausnahme verteilt, auf dem Host, der Aufrufer sähen die gelbe Bildschirm fehl oder ein anderen Host bereitgestellt wird, in der Regel HTML-Antwort und nicht in der Regel eine entsprechende Antwort der API-Fehler. In diesen Fällen beginnt der Ergebnis ungleich Null ist, und nur dann, wenn Sie ein benutzerdefinierten Ausnahmehandler explizit festlegt zurück, in `null` (nicht behandelte) wird die Ausnahme weitergeleitet an den Host. Festlegen von `Result` zu `null` in solchen Fällen kann für zwei Szenarien sinnvoll sein:

1. OWIN gehosteten Web-API mit benutzerdefinierter Ausnahmebehandlung eine Middleware vor und externen Web-API registriert.
2. Lokales Debuggen über einen Browser, in denen die gelbe Bildschirm fehl tatsächlich eine hilfreiche Antwort für eine nicht behandelte Ausnahme ist.

Für Protokollierungen Ausnahmen und Ausnahmehandler führen wir keine Alles wiederherstellen, wenn die Protokollierung oder der Handler eine Ausnahme auslöst. (Als lassen die Ausnahme weitergeben, Feedback unten auf dieser Seite lassen, wenn müssen Sie einen besseren Ansatz.) Der Vertrag für Ausnahme protokollierungund Handler ist darin, dass keine Ausnahmen, die bis zu den Aufrufer weitergegeben werden sollen; Andernfalls wird die Ausnahme weitergeleitet nur, häufig bis hin zu dem Host, der eine HTML-Fehlermeldung angezeigt (z. B. die ASP. NET Gelb-Bildschirm) an den Client (die in der Regel die bevorzugte Option für, die erwarten, JSON oder XML dass-API-Aufrufer ist nicht) gesendet werden.

## <a name="examples"></a>Beispiele

### <a name="tracing-exception-logger"></a>Protokollierung der Ablaufverfolgung-Ausnahmen

Die folgenden ausnahmeprotokollierung senden Ausnahmedaten an konfigurierte Ablaufverfolgungsquellen (einschließlich der Debug-Ausgabe-Fenster in Visual Studio).

[!code-csharp[Main](web-api-global-error-handling/samples/sample6.cs)]

### <a name="custom-error-message-exception-handler"></a>Benutzerdefinierte Fehler Meldungshandler-Ausnahme

Die folgenden unten erzeugt eine benutzerdefinierte Fehlerantwort an Clients, einschließlich einer e-Mail-Adresse für Kontaktaufnahme mit dem Support.

[!code-csharp[Main](web-api-global-error-handling/samples/sample7.cs)]

## <a name="registering-exception-filters"></a>Ausnahmefilter registrieren

Wenn Sie die Projektvorlage "ASP.NET MVC 4-Webanwendung" verwenden, um Ihr Projekt erstellen, fügen Sie Ihrer Web-API-Konfigurationscode in die `WebApiConfig` -Klasse in der *App/_starten* Ordner:

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

## <a name="appendix-base-class-details"></a>Anhang: Basisklasse Details

[!code-csharp[Main](web-api-global-error-handling/samples/sample8.cs)]
