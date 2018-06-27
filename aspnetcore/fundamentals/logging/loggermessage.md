---
title: Hochleistungsprotokollierung mit LoggerMessage in ASP.NET Core
author: guardrex
description: Erfahren Sie, wie Sie LoggerMessage verwenden, um Delegate zu erstellen, die zwischengespeichert werden können und die weniger Objektzuweisungen für Hochleistungsprotokollierungen benötigen.
ms.author: riande
ms.date: 11/03/2017
uid: fundamentals/logging/loggermessage
ms.openlocfilehash: e952591bac29868d87d765820e88c74b50a1fe88
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272434"
---
# <a name="high-performance-logging-with-loggermessage-in-aspnet-core"></a>Hochleistungsprotokollierung mit LoggerMessage in ASP.NET Core

Von [Luke Latham](https://github.com/guardrex)

[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage)-Features erstellen Delegate, die zwischengespeichert werden können und die im Vergleich zu [Methoden zur Protokollierungserweiterung](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions) (wie z.B. `LogInformation`, `LogDebug` und `LogError`) weniger Objektzuweisungen benötigen und den Rechenaufwand reduzieren. Verwenden Sie das `LoggerMessage`-Muster für Hochleistungsprotokollierungen.

`LoggerMessage` hat im Vergleich zu Protokollierungserweiterungsmethoden die folgenden Vorzüge:

* Protokollierungserweiterungsmethoden erfordern das Konvertieren von Werttypen wie `int` in `object` (sogenanntes Boxing). Das `LoggerMessage`-Muster verhindert das Konvertieren mithilfe von statischen `Action`-Feldern und mithilfe von Erweiterungsmethoden mit stark typisierten Parametern.
* Protokollierungserweiterungsmethoden müssen die Meldungsvorlagen (sogenannte Formatzeichenfolgen) jedes Mal analysieren, wenn eine Protokollmeldung geschrieben wird. `LoggerMessage` erfordert das Analysieren einer Vorlage nur einmal beim Festlegen der Meldung.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

Die Beispiel-App veranschaulicht `LoggerMessage`-Features mit einem einfachen System zur Zitatnachverfolgung. Die App fügt Zitate hinzu und löscht diese mithilfe einer speicherinternen Datenbank. Während dieser Vorgänge werden Protokollmeldungen mit dem `LoggerMessage`-Muster erstellt.

## <a name="loggermessagedefine"></a>LoggerMessage.Define

[Define(LogLevel, EventId, String)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) erstellt einen `Action`-Delegaten zum Protokollieren von Meldungen. `Define` überlädt die Zulassung und übergibt bis zu sechs Typparameter an eine benannte Formatzeichenfolge (Vorlage).

Die für die Methode `Define` bereitgestellte Zeichenfolge ist eine Vorlage und keine interpolierte Zeichenfolge. Platzhalter werden in der Reihenfolge der angegebenen Typen ersetzt. Die Platzhalternamen in den Vorlagen sollten eindeutig und in allen Vorlagen einheitlich sein. Sie fungieren als Eigenschaftennamen in strukturierten Protokolldaten. Es wird empfohlen, für Platzhalternamen die [Pascal-Schreibweise](/dotnet/standard/design-guidelines/capitalization-conventions) zu verwenden. Platzhalter in einer derartigen Schreibweise sind z.B. `{Count}` und `{FirstName}`.

Jede Protokollmeldung ist ein `Action`-Objekt, das in einem von `LoggerMessage.Define` erstellten statischen Feld enthalten ist. Beispiel: Die Beispiel-App erstellt ein Feld, das eine Protokollmeldung für eine GET-Anforderung für die Indexseite beschreibt (*Internal/LoggerExtensions.cs*):

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet1)]

Geben Sie für das `Action`-Objekt Folgendes an:

* die Protokollierungsebene
* eine eindeutige Ereignis-ID ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) mit dem Namen der statischen Erweiterungsmethode
* die Meldungsvorlage (eine benannte Formatzeichenfolge) 

Eine Anforderung der Indexseite der Beispiel-App legt Folgendes fest:

* Die Protokollierungsebene ist `Information`.
* Die Ereignis-ID ist `1` mit dem Namen der `IndexPageRequested`-Methode.
* Die Meldungsvorlage (eine benannte Formatzeichenfolge) ist eine Zeichenfolge.

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet5)]

Strukturierte Protokollspeicher verwenden möglicherweise den Ereignisnamen wenn dieser mit der Ereignis-ID bereitgestellt wird. Dadurch wird die Protokollierung ergänzt. [Serilog](https://github.com/serilog/serilog-extensions-logging) verwendet z.B. den Ereignisnamen.

Das `Action`-Objekt wird mit einer stark typisierten Erweiterungsmethode aufgerufen. Die `IndexPageRequested`-Methode protokolliert eine Meldung für eine GET-Anforderung der Indexseite in der Beispiel-App:

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet9)]

`IndexPageRequested` wird in der Protokollierung in der `OnGetAsync`-Methode in *Pages/Index.cshtml.cs* aufgerufen:

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

Sehen Sie sich folgende Konsolenausgabe der App an:

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

Definieren Sie bis zu sechs Typen, wenn Sie das statische Feld erstellen, um Parameter an eine Protokollmeldung zu übergeben. Die Beispiel-App protokolliert eine Zeichenfolge beim Hinzufügen eines Zitats durch die Definition eines `string`-Typs für das `Action`-Feld:

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet2)]

Die Protokollmeldungsvorlage des Delegaten erhält ihre Platzhalterwerte von den bereitgestellten Typen. Die Beispiel-App definiert einen Delegaten zu Hinzufügen eines Zitats, wo der Zitatparameter ein `string`-Objekt ist:

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet6)]

`QuoteAdded`, die statische Erweiterungsmethode zum Hinzufügen von Zitaten, erhält den Wert des Zitatarguments und übergibt diesen an den `Action`-Delegaten:

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet10)]

Im Seitenmodell der Indexseite (*Pages/Index.cshtml.cs*) wird `QuoteAdded` aufgerufen, um die Meldung zu protokollieren:

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

Sehen Sie sich folgende Konsolenausgabe der App an:

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand')
```

Die Beispiel-App implementiert ein `try`&ndash;`catch`-Muster zum Löschen von Zitaten. Es wird eine Meldung protokolliert, die angibt, dass der Löschvorgang erfolgreich war. Es wird eine Fehlermeldung protokolliert, die angibt, dass bei einem Löschvorgang eine Ausnahme ausgelöst wurde. Die Protokollmeldung für den fehlgeschlagenen Löschvorgang enthält die Ausnahmenprotokollnachverfolgung (*Internal/LoggerExtensions.cs*):

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet7)]

Beachten Sie, wie die Ausnahme an den Delegaten in `QuoteDeleteFailed` übergeben wird:

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet11)]

Im Seitenmodell für die Indexseite ruft ein erfolgreicher Zitatlöschvorgang die `QuoteDeleted`-Methode in der Protokollierung auf: Wenn kein zu löschendes Zitat gefunden wird, wir eine `ArgumentNullException` ausgelöst. Die Ausnahme wird von der `try`&ndash;`catch`-Anweisung aufgefangen und durch einen Aufruf der `QuoteDeleteFailed`-Methode in der Protokollierung im `catch`-Block protokolliert (*Pages/Index.cshtml.cs*).

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet5&highlight=14,18)]

Wenn ein Zitat erfolgreich gelöscht wurde, sehen Sie sich die Konsolenausgabe der App an:

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand' Id = 1)
```

Wenn der Zitatlöschvorgang fehlschlägt, sehen Sie sich die Konsolenausgabe der App an. Beachten Sie, dass die Ausnahme in der Protokollmeldung beinhaltet ist:

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

## <a name="loggermessagedefinescope"></a>LoggerMessage.DefineScope

[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) erstellt einen `Func`-Delegaten zum Definieren eines [Protokollbereichs](xref:fundamentals/logging/index#log-scopes). `DefineScope` überlädt die Zulassung und übergibt bis zu drei Typparameter an eine benannte Formatzeichenfolge (Vorlage).

Wie bei der Methode `Define` ist die Zeichenfolge,die für die Methode `DefineScope` bereitgestellt wurde, eine Vorlage und keine interpolierte Zeichenfolge. Platzhalter werden in der Reihenfolge der angegebenen Typen ersetzt. Die Platzhalternamen in den Vorlagen sollten eindeutig und in allen Vorlagen einheitlich sein. Sie fungieren als Eigenschaftennamen in strukturierten Protokolldaten. Es wird empfohlen, für Platzhalternamen die [Pascal-Schreibweise](/dotnet/standard/design-guidelines/capitalization-conventions) zu verwenden. Platzhalter in einer derartigen Schreibweise sind z.B. `{Count}` und `{FirstName}`.

Definieren Sie einen [Protokollbereich](xref:fundamentals/logging/index#log-scopes), um mehrere Protokollmeldungen mit der [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope)-Methode anzuwenden.

Die Beispiel-App verfügt über die Schaltfläche **Clear all** (Alles löschen) zum Löschen aller Zitate in der Datenbank. Die Zitate werden nacheinander entfernt und gelöscht. Bei jedem Zitatlöschvorgang wird die `QuoteDeleted`-Methode in der Protokollierung aufgerufen. Diesen Protokollmeldung wird ein Protokollbereich hinzugefügt.

Aktivieren Sie `IncludeScopes` in der Datei *appsettings.json* im Abschnitt „Konsolenprotokollierung“:

[!code-csharp[](loggermessage/sample/appsettings.json?highlight=3-5)]

Fügen Sie zum Erstellen eines Protokollbereichs ein Feld hinzu, dass einen `Func`-Delegaten für den Bereich enthält. Die Beispiel-App erstellt ein Feld mit dem Namen `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet4)]

Verwenden Sie `DefineScope` zum Erstellen eines Delegaten. Sie können bis zu drei Typen als Vorlagenargumente festlegen, die verwendet werden können, wenn der Delegat aufgerufen wird. Die Beispiel-App verwendet eine Meldungsvorlage, die die Anzahl der gelöschten Zitate enthält (ein `int`-Typ):

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet8)]

Stellen Sie eine statische Erweiterungsmethode für die Protokollmeldung bereit. Beziehen Sie Typparameter für benannte Eigenschaften mit ein, die in der Meldungsvorlage vorhanden sind. Die Beispiel-App erstellt ein `count` der zu löschenden Zitate und gibt `_allQuotesDeletedScope` zurück:

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet12)]

Der Bereich umschließt die Protokollerweiterungsaufrufe in einem `using`-Block:

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

Sehen Sie sich die Protokollmeldungen in der Konsolenausgabe der App an. Das folgende Ergebnis zeigt drei gelöschte Zitate sowie die Protokollbereichsmeldung:

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

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Logging (Protokollierung)](xref:fundamentals/logging/index)
