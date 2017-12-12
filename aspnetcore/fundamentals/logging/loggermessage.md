---
title: Hohe Leistung Protokollierung mit LoggerMessage in ASP.NET Core
author: guardrex
description: "Erfahren Sie, wie LoggerMessage Funktionen zum Übertragen von zwischenspeicherbaren Delegaten zu erstellen, die müssen weniger objektzuweisungen als Erweiterungsmethoden für die Protokollierung für Protokollierungsszenarien mit hoher Leistung verwenden."
ms.author: riande
manager: wpickett
ms.date: 11/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/logging/loggermessage
ms.openlocfilehash: defba75c6c9ea13d24af4cd8515d82d9e7cf9853
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="high-performance-logging-with-loggermessage-in-aspnet-core"></a>Hohe Leistung Protokollierung mit LoggerMessage in ASP.NET Core

Von [Luke Latham](https://github.com/guardrex)

[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) Funktionen erstellen zwischengespeichert werden Delegaten, in denen weniger objektzuweisungen und rechenleistung erfordern als reduziert [Protokollierung Erweiterungsmethoden](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), wie z. B. `LogInformation`, `LogDebug`, und `LogError`. Verwenden Sie für hohe Leistung Protokollierungsszenarien, die `LoggerMessage` Muster.

`LoggerMessage`bietet die folgenden Leistungsvorteile über Erweiterungsmethoden für die Protokollierung:

* Erweiterungsmethoden für die Protokollierung erfordern "Boxing" (die Konvertierung von) Werttypen, wie z. B. `int`, in `object`. Die `LoggerMessage` Muster vermeidet Boxing mit statischen `Action` Felder und Erweiterungsmethoden, mit stark typisierten Parametern.
* Erweiterungsmethoden für die Protokollierung müssen die Nachrichtenvorlage (benannte Formatzeichenfolge) analysieren, jedes Mal eine protokollmeldung geschrieben wird. `LoggerMessage`nur erfordert eine Vorlage einmal analysieren, wenn die Nachricht definiert ist.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

Die Beispiel-app veranschaulicht `LoggerMessage` Funktionen mit einem einfachen Anführungszeichen Überwachungssystem. Die app hinzufügt und löscht Anführungszeichen, die unter Verwendung einer Datenbank im Arbeitsspeicher. Bei diesen Vorgängen auftreten, protokollmeldungen werden generiert, mit der `LoggerMessage` Muster.

## <a name="loggermessagedefine"></a>LoggerMessage.Define

[Definieren von (LogLevel, EventId, String)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) erstellt eine `Action` delegate für eine Nachricht protokollieren. `Define`Überladungen ist die Übergabe von bis zu sechs Typparameter auf eine benannte Formatzeichenfolge (Vorlage) zulässig.

## <a name="loggermessagedefinescope"></a>LoggerMessage.DefineScope

[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) erstellt eine `Func` delegieren Sie zum Definieren einer [protokollieren Bereich](xref:fundamentals/logging/index#log-scopes). `DefineScope`Überladungen ist die Übergabe von bis zu drei Parameter auf eine benannte Formatzeichenfolge (Vorlage) zulässig.

## <a name="message-template-named-format-string"></a>Nachrichtenvorlage (mit dem Namen Formatzeichenfolge)

Die Zeichenfolge bereitgestellt, um die `Define` und `DefineScope` Methoden ist eine Vorlage und nicht für eine interpolierte Zeichenfolge. Platzhalter werden in der Reihenfolge gefüllt, dass die Typen angegeben werden. Platzhalternamen in der Vorlage sollte in Vorlagen beschreibende und konsistent sein. Sie dienen als Eigenschaftsnamen in strukturierten Protokolldaten. Wir empfehlen [Pascal Schreibweise](/dotnet/standard/design-guidelines/capitalization-conventions) für Platzhalternamen. Beispielsweise `{Count}`, `{FirstName}`.

## <a name="implementing-loggermessagedefine"></a>Implementieren von LoggerMessage.Define

Jede Nachricht Protokoll ist ein `Action` frei, die in einem statischen Feld erstellt, indem `LoggerMessage.Define`. Die Beispiel-app erstellt z. B. ein Feld, um eine protokollmeldung für eine GET-Anforderung für die Indexseite beschreiben (*Internal/LoggerExtensions.cs*):

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet1)]

Für die `Action`, angeben:

* Die Protokollebene.
* Ein eindeutiger Bezeichner ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) mit dem Namen der statischen Erweiterungsmethode.
* Die Message-Vorlage (mit dem Namen Formatzeichenfolge). 

Eine Anforderung für die Indexseite, der im Beispiel-app wird die:

* Die Protokollebene auf `Information`.
* Ereignis-Id für `1` durch den Namen der `IndexPageRequested` Methode.
* Nachrichtenvorlage (mit dem Namen Formatzeichenfolge) in eine Zeichenfolge.

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet5)]

Speichert strukturierte Protokollierung können den Namen des Ereignisses verwenden, wenn sie mit der Ereignis-Id für die Protokollierung anzureichern bereitgestellt wird. Beispielsweise [Serilog](https://github.com/serilog/serilog-extensions-logging) verwendet den Namen des Ereignisses.

Die `Action` wird durch eine stark typisierte Erweiterungsmethode aufgerufen. Die `IndexPageRequested` -Methode protokolliert eine Meldung für die GET-Anforderung eine Index-Seite in der Beispiel-app:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet9)]

`IndexPageRequested`wird aufgerufen, für die Protokollierung in die `OnGetAsync` Methode im *Pages/Index.cshtml.cs*:

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

Überprüfen Sie die app-Konsolenausgabe:

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

Zum Übergeben von Parametern zu einer protokollmeldung, können definieren Sie bis zu sechs Typen, wenn das statische Feld zu erstellen. Die Beispiel-app protokolliert eine Zeichenfolge beim Hinzufügen eines Angebots durch Definieren einer `string` Geben Sie für die `Action` Feld:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet2)]

Der Delegat Protokoll Nachrichtenvorlage empfängt die Platzhalterwerte von bereitgestellten Typen. Die Beispiel-app definiert einen Delegaten für das Hinzufügen eines Angebots, in dem der Angebots-Parameter ist, ein `string`:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet6)]

Die statische Erweiterungsmethode zum Hinzufügen eines Angebots `QuoteAdded`, empfängt den Argumentwert Angebot und übergibt es an die `Action` delegieren:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet10)]

In der Indexseite Code-Behind-Datei (*Pages/Index.cshtml.cs*), `QuoteAdded` wird aufgerufen, um die Protokollierung der Meldung:

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

Überprüfen Sie die app-Konsolenausgabe:

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand')
```

Implementiert die Beispiel-app eine `try` &ndash; `catch` Muster zum Angebot löschen. Für eine erfolgreiche Löschvorgang ist eine informative Meldung protokolliert. Für einen Löschvorgang an wird eine Fehlermeldung protokolliert, wenn eine Ausnahme ausgelöst wird. Die protokollmeldung für den fehlgeschlagenen Vorgang löschen enthält die stapelüberwachung der Ausnahme (*Internal/LoggerExtensions.cs*):

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet7)]

Beachten Sie, wie die Ausnahme übergeben wird, an den Delegaten in `QuoteDeleteFailed`:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet11)]

In den Index Seite Code-Behind, Löschvorgang erfolgreich Angebot Ruft die `QuoteDeleted` Methode für die Protokollierung. Wenn ein Angebot zum Löschen, nicht gefunden wird eine `ArgumentNullException` ausgelöst wird. Die Ausnahme abgefangen wird, durch die `try` &ndash; `catch` Anweisung und protokolliert Sie durch Aufrufen der `QuoteDeleteFailed` Methode für die Protokollierung in die `catch` Block (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet5&highlight=14,18)]

Wenn ein Angebot wurde erfolgreich gelöscht wird, überprüfen Sie die app-Konsolenausgabe:

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand' Id = 1)
```

Beim Angebot löschen ein Fehler auftritt, überprüfen Sie die app-Konsolenausgabe. Beachten Sie, dass die Ausnahme in der Protokoll-Nachricht enthalten ist:

```console
fail: LoggerMessageSample.Pages.IndexModel[5]
      => RequestId:0HL90M6E7PHK5:00000010 RequestPath:/ => /Index
      Quote delete failed (Id = 999)
System.ArgumentNullException: Value cannot be null.
Parameter name: entity
   at Microsoft.EntityFrameworkCore.Utilities.Check.NotNull[T](T value, String parameterName)
   at Microsoft.EntityFrameworkCore.DbContext.Remove[TEntity](TEntity entity)
   at Microsoft.EntityFrameworkCore.Internal.InternalDbSet`1.Remove(TEntity entity)
   at LoggerMessageSample.Pages.IndexModel.<OnPostDeleteQuoteAsync>d__14.MoveNext() in 
      <PATH>\sample\Pages\Index.cshtml.cs:line 87
```

## <a name="implementing-loggermessagedefinescope"></a>Implementieren von LoggerMessage.DefineScope

Definieren einer [protokollieren Bereich](xref:fundamentals/logging/index#log-scopes) anwenden auf eine Reihe von protokollmeldungen, die mit der [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) Methode.

Die Beispiel-app hat einen **alle löschen** Schaltfläche für alle Anführungszeichen in der Datenbank zu löschen. Die Anführungszeichen werden gelöscht, indem Sie sie entfernen eine zu einem Zeitpunkt. Jedes Mal ein Angebot gelöscht wird, die `QuoteDeleted` Methode für die Protokollierung aufgerufen wird. Diese protokollmeldungen wird ein Protokoll-Bereich hinzugefügt.

Aktivieren Sie `IncludeScopes` in die Konsole Logger-Optionen:

[!code-csharp[Main](loggermessage/sample/Program.cs?name=snippet1&highlight=22)]

Festlegen von `IncludeScopes` Protokoll Bereiche Aktivierung in ASP.NET Core 2.0-apps erforderlich ist. Festlegen von `IncludeScopes` über *"appSettings"* Konfigurationsdateien ist ein Feature, das für die Version 2.1 von ASP.NET Core geplant hat.

Die Beispiel-app löscht von anderen Anbietern und fügt Filter, um die Protokollausgabe zu reduzieren. Dies erleichtert es, des Beispiels protokollmeldungen, die veranschaulichen, finden Sie unter `LoggerMessage` Funktionen.

Um einen Bereich des Protokolls zu erstellen, fügen Sie ein Feld zum Speichern einer `Func` für den Bereich zu delegieren. Die Beispiel-app erstellt, ein Feld namens `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet4)]

Verwendung `DefineScope` an den Delegaten zu erstellen. Bis zu drei Typen können angegeben werden für die Verwendung als Vorlagenargumente beim Aufrufen des Delegaten. Die Beispiel-app verwendet eine Nachrichtenvorlage, die die Anzahl der gelöschten Anführungszeichen enthält (eine `int` Typ):

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet8)]

Geben Sie eine statische Erweiterungsmethode für die protokollmeldung. Schließen Sie Typparameter für den benannten Eigenschaften, die in der Meldungsvorlage ein Die Beispiel-app nimmt eine `count` Anführungszeichen löschen und gibt `_allQuotesDeletedScope`:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet12)]

Der Bereich umschließt, die die Erweiterung für die Protokollierung von in Aufrufen eine `using` blockieren:

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

Überprüfen Sie die protokollmeldungen in die app-Konsolenausgabe an. Das folgende Ergebnis zeigt drei Angebote, die mit dem die protokollmeldung zum Bereich enthalten gelöscht:

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 1' Id = 2)
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 2' Id = 3)
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 3' Id = 4)
```

## <a name="see-also"></a>Siehe auch

* [Logging (Protokollierung)](xref:fundamentals/logging/index)
