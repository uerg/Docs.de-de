---
uid: web-api/overview/error-handling/web-api-global-error-handling
title: In der ASP.NET Web API 2 zur globalen Fehlerbehandlung | Microsoft Docs
author: davidmatson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/03/2014
ms.topic: article
ms.assetid: bffd7863-f63b-4b23-a13c-372b5492e9fb
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/error-handling/web-api-global-error-handling
msc.type: authoredcontent
ms.openlocfilehash: c593c56ba3d0ee8ebf6dc425408d2c3b91c83f93
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="global-error-handling-in-aspnet-web-api-2"></a>In der ASP.NET Web API 2 zur globalen Fehlerbehandlung
====================
durch [David Matson](https://github.com/davidmatson), [Rick Anderson](https://github.com/Rick-Anderson)

Heute ist gibt es keine einfache Möglichkeit in Web-API zu melden oder global, Fehler zu behandeln. Einige nicht behandelten Ausnahmen verarbeitet werden können, über [Ausnahmefilter](exception-handling.md), aber es gibt eine Anzahl von Fällen an, die Ausnahmefilter nicht behandeln können. Zum Beispiel:

1. Ausnahmen, die vom Controller Konstruktoren ausgelöst wird.
2. Von Meldungshandler ausgelösten Ausnahmen.
3. Während des Routings ausgelösten Ausnahmen.
4. Während der Inhalt antwortserialisierung ausgelösten Ausnahmen.

Stellt eine einfache, konsistente Möglichkeit, sich anzumelden, und behandeln (sofern möglich) soll diese Ausnahmen. 

Es gibt zwei wichtige Fälle zum Behandeln von Ausnahmen, die Groß-/Kleinschreibung, in denen wir senden eine Fehlerantwort sind und die Groß-/Kleinschreibung, in dem alle wir sonst können, Log die Ausnahme. Ein Beispiel für den letzteren Fall wird beim Auftreten einer Ausnahme in der Mitte der Antwort Streaminginhalt; In diesem Fall ist es zu spät, eine neue Antwortnachricht gesendet werden, da er den Statuscode, der Header und der Teil des Inhalts bereits über das Netz überschritten haben, damit wir einfach die Verbindung abgebrochen. Obwohl die Ausnahme behandelt werden kann, um eine neue Antwortnachricht zu erzeugen, unterstützen wir weiterhin Protokollierung der Ausnahme. In Fällen, in denen einen Fehler erkannt werden kann, kann es eine entsprechende Fehlermeldung Antwort zurückgeben, wie im folgenden gezeigt:

[!code-csharp[Main](web-api-global-error-handling/samples/sample1.cs?highlight=6)]

### <a name="existing-options"></a>Die vorhandenen Optionen

Zusätzlich zu [Ausnahmefilter](exception-handling.md), [message-Handler](../advanced/http-message-handlers.md) können heute verwendet werden, um alle auf 500-Antworten zu beobachten, auf diese Antworten ist jedoch schwierig, wie sie über den ursprünglichen Fehler Kontext fehlender. Meldungshandler haben auch die gleichen Einschränkungen als Ausnahmefilter bezüglich der Fälle, die sie behandeln können. Während der Web-API-Ablaufverfolgungsinfrastruktur verfügt, die möglichen Fehlerzustände erfasst ist die Ablaufverfolgungsinfrastruktur ist für Diagnosezwecke und nicht entwickelt und für die Ausführung in produktionsumgebungen geeignet. Globalen Ausnahmeklassen Fehlerbehandlung und-Protokollierung muss Dienste, die während der Produktion ausführen können, und vorhandene überwachungslösungen angeschlossen werden (z. B. [ELMAH](https://code.google.com/p/elmah/) ).

### <a name="solution-overview"></a>Lösungsübersicht

 Wir bieten zwei neue austauschbare Dienste [IExceptionLogger](../releases/whats-new-in-aspnet-web-api-21.md) und IExceptionHandler, anmelden und nicht behandelte Ausnahmen zu behandeln. Die Dienste sind sehr ähnlich, mit zwei Hauptunterschiede:

1. Wir unterstützen mehrere Ausnahme Protokollierungen jedoch nur einen einzigen Ausnahmehandler registrieren.
2. Ausnahme Protokollierungen erhalten immer aufgerufen, auch wenn wir sind im Begriff, die Verbindung abgebrochen. Ausnahmehandler erhalten nur dann aufgerufen, wenn wir weiterhin auf die Antwortnachricht zu senden sind.

Beide Dienste ermöglichen den Zugriff auf einen Ausnahmekontext enthält relevante Informationen an der Stelle, wo die Ausnahme erkannt wurde, insbesondere die [HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage(v=vs.110).aspx), [HttpRequestContext](https://msdn.microsoft.com/library/system.web.http.controllers.httprequestcontext(v=vs.118).aspx), wird die Ausnahme und die Quelle der Ausnahme (s. unten) ausgelöst.

### <a name="design-principles"></a>Entwurfsprinzipien

1. **Keine aktuellen Änderungen** , da diese Funktionalität in eine kleinere Version eine wichtige Einschränkung, die Auswirkungen auf die Projektmappe ist, die keine wichtige Änderungen in den Typ von Verträgen vorhanden sein oder Verhalten hinzugefügt wird. Diese Einschränkung ausgeschlossen Cleanup, möchten wir im Hinblick auf vorhandenen Catch-Blöcken, die Aktivieren von Ausnahmen in mit 500 Antworten getan haben. Diese zusätzliche Bereinigung ist etwas, das wir für einen nachfolgenden Hauptversion erwägen. Ist dies wichtig, Sie bitte stimmen Sie auf ihn unter [ASP.NET-Web-API benutzermeinungen](http://aspnet.uservoice.com/forums/147201-asp-net-web-api/suggestions/5451321-add-flag-to-enable-iexceptionlogger-and-iexception).
2. **Wahrung der Konsistenz mit Web-API erstellt** Web-API Filterpipeline ist eine hervorragende Möglichkeit, querschnittliche Bedenken die Flexibilität der Anwendung der Logik einen aktionsspezifische, Controller-spezifische oder globalen Gültigkeitsbereich zu behandeln. Filter, einschließlich Ausnahmefilter, haben immer Aktion und Controller-Kontext, selbst wenn im globalen Gültigkeitsbereich registriert. Vertrag ist sinnvoll für Filter, aber dies bedeutet, dass die Ausnahmefilter, sogar global gültiger Beziehungen, sind gut geeignet für einige Ausnahmebehandlung Fällen, z. B. Ausnahmen vom Message-Handler, sofern kein Aktions- oder Controllerebene Kontext nicht vorhanden ist. Wenn wir die flexible Bereichsdefinition von Filtern für die Ausnahmebehandlung Authentifizierungsprozesses verwenden möchten, benötigen wir noch Ausnahmefilter. Aber wenn es außerhalb einer Controllerkontext Ausnahme behandeln müssen, wir auch ein separates Konstrukt für die vollständige globalen Fehlerbehandlung (etwa ohne die Controller und das Aktionsergebnis Kontext Einschränkungen).

### <a name="when-to-use"></a>Wann verwenden

- Protokollierungen Ausnahmen sind die Lösung nicht behandelte Ausnahme von Web-API anzeigen.
- Ausnahmehandler werden die Lösung für alle möglichen Antworten auf nicht behandelte Ausnahmen abgefangen werden, von der Web-API anpassen.
- Ausnahmefilter sind die einfachste Lösung für die Verarbeitung der Teilmenge, die nicht behandelte Ausnahmen, die im Zusammenhang mit der eine bestimmte Aktion oder einen Controller.

### <a name="service-details"></a>Dienstdetails

 Dienstschnittstellen für die Ausnahme Protokollierung und Ereignishandler sind einfache Async-Methoden, die die jeweiligen Kontexte dauert: 

[!code-csharp[Main](web-api-global-error-handling/samples/sample2.cs)]

 Wir bieten auch die Basisklassen für beide Schnittstellen. Überschreiben die Core (Synchronisierung oder Async-Methoden) ist erforderlich ist, anmelden oder auf die empfohlenen behandeln Zeiten. Für die Protokollierung der `ExceptionLogger` Basisklasse wird sichergestellt, dass die zentrale Protokollierung-Methode nur einmal für jede Ausnahme aufgerufen wird (auch wenn es später weitergibt weiter die Aufrufliste nach oben und erneut abgefangen wird). Die `ExceptionHandler` Basisklasse den Kern Ereignisbehandlungsmethode nur für Ausnahmen, die am oberen Rand der Aufrufliste, ignorieren die Vorgängerversion geschachtelt-Blocks catch aufgerufen wird. (Vereinfachte Versionen dieser Basisklassen sind im Anhang unten). Beide `IExceptionLogger` und `IExceptionHandler` erhalten Sie Informationen zur Ausnahme über einen `ExceptionContext`.

[!code-csharp[Main](web-api-global-error-handling/samples/sample3.cs)]

Wenn das Framework eine ausnahmeprotokollierung oder einen Ausnahmehandler aufgerufen wird, geben sie immer ein `Exception` und ein `Request`. Ausnahme stellen Komponententests, bietet es außerdem immer eine `RequestContext`. Selten gebe eine `ControllerContext` und `ActionContext` (nur, wenn die vom Catch-Block für Ausnahmefilter aufgerufen wird). Es bietet eine sehr selten eine `Response`(nur in bestimmten IIS-Fällen, wenn in der Mitte beim Schreiben der Antwort). Beachten Sie, dass, da einige dieser Eigenschaften möglicherweise `null` es liegt im Ermessen der Consumer zu suchende `null` vor dem Zugriff auf Mitglieder der Exception-Klasse.`CatchBlock` ist eine Zeichenfolge, die angibt, welche Catch-Block die Ausnahme gesehen haben. Die Catch-Block-Zeichenfolgen sind möglich:

- HttpServer (SendAsync-Methode)
- HttpControllerDispatcher (SendAsync-Methode)
- HttpBatchHandler (SendAsync-Methode)
- Iexceptionfilter (ApiControllers Verarbeitung der Ausnahme Filterpipeline in ExecuteAsync).
- OWIN-Host:

    - HttpMessageHandlerAdapter.BufferResponseContentAsync (for buffering output)
    - HttpMessageHandlerAdapter.CopyResponseContentAsync (for streaming output)
- Webhost:

    - HttpControllerHandler.WriteBufferedResponseContentAsync (for buffering output)
    - HttpControllerHandler.WriteStreamedResponseContentAsync (for streaming output)
    - HttpControllerHandler.WriteErrorResponseContentAsync (for failures in error recovery under buffered output mode)

Die Liste der Catch-Block Zeichenfolgen steht auch über statische Readonly-Eigenschaften. (Die Core Catch-Block Zeichenfolge befinden sich auf die statische ExceptionCatchBlocks; der Rest auf eine statische Klasse jeden für OWIN und Web Host angezeigt werden).`IsTopLevelCatchBlock` ist nützlich zum Behandeln von Ausnahmen, die nur am Anfang der Aufrufliste das empfohlene Muster folgen. Anstatt aktivieren Ausnahmen in mit 500 Antworten, in denen auch ein geschachtelte Catch-Block auftritt, kann ein Ausnahmehandler Ausnahmen weitergegeben, bis sie sind im Begriff, den Host sichtbar sein können.

Zusätzlich zu den `ExceptionContext`, eine Protokollierung Ruft eine weitere Information über die vollständige `ExceptionLoggerContext`:

[!code-csharp[Main](web-api-global-error-handling/samples/sample4.cs)]

Die zweite Eigenschaft, `CanBeHandled`, können Sie eine Protokollierung, um eine Ausnahme zu identifizieren, die nicht behandelt werden kann. Wenn die Verbindung wird abgebrochen und kann keine neue Antwortnachricht gesendet werden, die Protokollierungen aufgerufen wird, jedoch wird des Handlers ***nicht*** aufgerufen werden, und die Protokollierungen können zu diesem Szenario der von dieser Eigenschaft identifizieren.

In zusätzlich zu den `ExceptionContext`, ein Handler Ruft eine weitere Eigenschaft sie, auf das vollständige festlegen können `ExceptionHandlerContext` zur Behandlung von Ausnahmen:

[!code-csharp[Main](web-api-global-error-handling/samples/sample5.cs)]

Ein Ausnahmehandler gibt an, dass er eine Ausnahme, indem behandelt wurde die `Result` Eigenschaft, um ein Aktionsergebnis (z. B. ein [ExceptionResult](https://msdn.microsoft.com/library/system.web.http.results.exceptionresult(v=vs.118).aspx), [InternalServerErrorResult](https://msdn.microsoft.com/library/system.web.http.results.internalservererrorresult(v=vs.118).aspx), [ StatusCodeResult](https://msdn.microsoft.com/library/system.web.http.results.statuscoderesult(v=vs.118).aspx), oder ein benutzerdefiniertes Ergebnis). Wenn die `Result` -Eigenschaft null ist, die Ausnahme nicht behandelt wird und die ursprüngliche Ausnahme erneut ausgelöst werden.

Für Ausnahmen, die am oberen Rand der Aufrufliste haben wir einen zusätzlichen Schritt, um sicherzustellen, dass die Antwort für API-Aufrufer geeignet ist. Die Ausnahme verteilt werden soll, bis zu dem Host, der Aufrufer würde dem gelben Bildschirm zum Tod finden Sie unter ein anderen Host bereitgestellte oder i. d. r. HTML-Antwort und nicht in der Regel eine entsprechende API-Fehlerantwort. In diesen Fällen ist das Ergebnis beginnt ungleich Null ist, und nur, wenn Sie ein benutzerdefinierten Ausnahmehandler explizit festlegt zurück zum `null` (nicht behandelt) wird die Ausnahme weitergeleitet an den Host. Festlegen von `Result` zu `null` kann in einem solchen Fall für zwei Szenarien nützlich sein:

1. OWIN gehostete Web-API mit benutzerdefinierter Ausnahmebehandlung eine Middleware, die vor und externen-Web-API registriert.
2. Lokales Debuggen über einen Browser, wobei der gelbe Bildschirm zum Tod tatsächlich eine hilfreiche Antwort für eine nicht behandelte Ausnahme ist.

Für Protokollierungen Ausnahmen und Ausnahmehandler wir keine wiederherstellen, wenn die Protokollierung oder der Handler selbst eine Ausnahme auslöst. (Als lassen die Ausnahme weitergeben, Feedback am unteren Rand dieser Seite lassen, wenn müssen Sie einen besseren Ansatz.) Der Vertrag für die Ausnahme protokollierungund Handler ist darin, dass keine Ausnahmen, die bis zu deren Aufrufer weitergeben soll; Andernfalls wird die Ausnahme weitergeleitet nur, häufig ganz auf den Host, der eine HTML-Fehlermeldung (z. B. ASP. NET gelben Bildschirm) an den Client (welcher nicht in der Regel die bevorzugte Option für API-Aufrufer, die JSON oder XML zu erwarten ist) gesendet werden.

## <a name="examples"></a>Beispiele

### <a name="tracing-exception-logger"></a>Protokollierung der Ablaufverfolgung-Ausnahmen

Die folgenden ausnahmeprotokollierung senden Ausnahmedaten an konfigurierte Ablaufverfolgungsquellen (einschließlich der Ausgabe-Debugfenster in Visual Studio).

[!code-csharp[Main](web-api-global-error-handling/samples/sample6.cs)]

### <a name="custom-error-message-exception-handler"></a>Benutzerdefinierte Fehlerseite Nachrichtenhandler-Ausnahme

Die folgenden erzeugt eine benutzerdefinierte Fehlerantwort auf Clients, einschließlich einer e-Mail-Adresse für den Support.

[!code-csharp[Main](web-api-global-error-handling/samples/sample7.cs)]

## <a name="registering-exception-filters"></a>Ausnahmefilter registrieren

Wenn Sie die Projektvorlage "ASP.NET MVC 4-Webanwendung" verwenden, um das Projekt zu erstellen, speichern Sie Ihre Web-API-Konfigurationscode innerhalb der `WebApiConfig` Klasse, in der *App/vir_tuelle starten* Ordner:

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

## <a name="appendix-base-class-details"></a>Anhang: Basisklasse Details

[!code-csharp[Main](web-api-global-error-handling/samples/sample8.cs)]
