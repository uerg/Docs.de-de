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
# <a name="high-performance-logging-with-loggermessage-in-aspnet-core"></a><span data-ttu-id="ab850-103">Hochleistungsprotokollierung mit LoggerMessage in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ab850-103">High-performance logging with LoggerMessage in ASP.NET Core</span></span>

<span data-ttu-id="ab850-104">Von [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ab850-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ab850-105">[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage)-Features erstellen Delegate, die zwischengespeichert werden können und die im Vergleich zu [Methoden zur Protokollierungserweiterung](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions) (wie z.B. `LogInformation`, `LogDebug` und `LogError`) weniger Objektzuweisungen benötigen und den Rechenaufwand reduzieren.</span><span class="sxs-lookup"><span data-stu-id="ab850-105">[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) features create cacheable delegates that require fewer object allocations and reduced computational overhead compared to [logger extension methods](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), such as `LogInformation`, `LogDebug`, and `LogError`.</span></span> <span data-ttu-id="ab850-106">Verwenden Sie das `LoggerMessage`-Muster für Hochleistungsprotokollierungen.</span><span class="sxs-lookup"><span data-stu-id="ab850-106">For high-performance logging scenarios, use the `LoggerMessage` pattern.</span></span>

<span data-ttu-id="ab850-107">`LoggerMessage` hat im Vergleich zu Protokollierungserweiterungsmethoden die folgenden Vorzüge:</span><span class="sxs-lookup"><span data-stu-id="ab850-107">`LoggerMessage` provides the following performance advantages over Logger extension methods:</span></span>

* <span data-ttu-id="ab850-108">Protokollierungserweiterungsmethoden erfordern das Konvertieren von Werttypen wie `int` in `object` (sogenanntes Boxing).</span><span class="sxs-lookup"><span data-stu-id="ab850-108">Logger extension methods require "boxing" (converting) value types, such as `int`, into `object`.</span></span> <span data-ttu-id="ab850-109">Das `LoggerMessage`-Muster verhindert das Konvertieren mithilfe von statischen `Action`-Feldern und mithilfe von Erweiterungsmethoden mit stark typisierten Parametern.</span><span class="sxs-lookup"><span data-stu-id="ab850-109">The `LoggerMessage` pattern avoids boxing by using static `Action` fields and extension methods with strongly-typed parameters.</span></span>
* <span data-ttu-id="ab850-110">Protokollierungserweiterungsmethoden müssen die Meldungsvorlagen (sogenannte Formatzeichenfolgen) jedes Mal analysieren, wenn eine Protokollmeldung geschrieben wird.</span><span class="sxs-lookup"><span data-stu-id="ab850-110">Logger extension methods must parse the message template (named format string) every time a log message is written.</span></span> <span data-ttu-id="ab850-111">`LoggerMessage` erfordert das Analysieren einer Vorlage nur einmal beim Festlegen der Meldung.</span><span class="sxs-lookup"><span data-stu-id="ab850-111">`LoggerMessage` only requires parsing a template once when the message is defined.</span></span>

<span data-ttu-id="ab850-112">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ab850-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="ab850-113">Die Beispiel-App veranschaulicht `LoggerMessage`-Features mit einem einfachen System zur Zitatnachverfolgung.</span><span class="sxs-lookup"><span data-stu-id="ab850-113">The sample app demonstrates `LoggerMessage` features with a basic quote tracking system.</span></span> <span data-ttu-id="ab850-114">Die App fügt Zitate hinzu und löscht diese mithilfe einer speicherinternen Datenbank.</span><span class="sxs-lookup"><span data-stu-id="ab850-114">The app adds and deletes quotes using an in-memory database.</span></span> <span data-ttu-id="ab850-115">Während dieser Vorgänge werden Protokollmeldungen mit dem `LoggerMessage`-Muster erstellt.</span><span class="sxs-lookup"><span data-stu-id="ab850-115">As these operations occur, log messages are generated using the `LoggerMessage` pattern.</span></span>

## <a name="loggermessagedefine"></a><span data-ttu-id="ab850-116">LoggerMessage.Define</span><span class="sxs-lookup"><span data-stu-id="ab850-116">LoggerMessage.Define</span></span>

<span data-ttu-id="ab850-117">[Define(LogLevel, EventId, String)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) erstellt einen `Action`-Delegaten zum Protokollieren von Meldungen.</span><span class="sxs-lookup"><span data-stu-id="ab850-117">[Define(LogLevel, EventId, String)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) creates an `Action` delegate for logging a message.</span></span> <span data-ttu-id="ab850-118">`Define` überlädt die Zulassung und übergibt bis zu sechs Typparameter an eine benannte Formatzeichenfolge (Vorlage).</span><span class="sxs-lookup"><span data-stu-id="ab850-118">`Define` overloads permit passing up to six type parameters to a named format string (template).</span></span>

<span data-ttu-id="ab850-119">Die für die Methode `Define` bereitgestellte Zeichenfolge ist eine Vorlage und keine interpolierte Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="ab850-119">The string provided to the `Define` method is a template and not an interpolated string.</span></span> <span data-ttu-id="ab850-120">Platzhalter werden in der Reihenfolge der angegebenen Typen ersetzt.</span><span class="sxs-lookup"><span data-stu-id="ab850-120">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="ab850-121">Die Platzhalternamen in den Vorlagen sollten eindeutig und in allen Vorlagen einheitlich sein.</span><span class="sxs-lookup"><span data-stu-id="ab850-121">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="ab850-122">Sie fungieren als Eigenschaftennamen in strukturierten Protokolldaten.</span><span class="sxs-lookup"><span data-stu-id="ab850-122">They serve as property names within structured log data.</span></span> <span data-ttu-id="ab850-123">Es wird empfohlen, für Platzhalternamen die [Pascal-Schreibweise](/dotnet/standard/design-guidelines/capitalization-conventions) zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="ab850-123">We recommend [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) for placeholder names.</span></span> <span data-ttu-id="ab850-124">Platzhalter in einer derartigen Schreibweise sind z.B. `{Count}` und `{FirstName}`.</span><span class="sxs-lookup"><span data-stu-id="ab850-124">For example, `{Count}`, `{FirstName}`.</span></span>

<span data-ttu-id="ab850-125">Jede Protokollmeldung ist ein `Action`-Objekt, das in einem von `LoggerMessage.Define` erstellten statischen Feld enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="ab850-125">Each log message is an `Action` held in a static field created by `LoggerMessage.Define`.</span></span> <span data-ttu-id="ab850-126">Beispiel: Die Beispiel-App erstellt ein Feld, das eine Protokollmeldung für eine GET-Anforderung für die Indexseite beschreibt (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="ab850-126">For example, the sample app creates a field to describe a log message for a GET request for the Index page (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet1)]

<span data-ttu-id="ab850-127">Geben Sie für das `Action`-Objekt Folgendes an:</span><span class="sxs-lookup"><span data-stu-id="ab850-127">For the `Action`, specify:</span></span>

* <span data-ttu-id="ab850-128">die Protokollierungsebene</span><span class="sxs-lookup"><span data-stu-id="ab850-128">The log level.</span></span>
* <span data-ttu-id="ab850-129">eine eindeutige Ereignis-ID ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) mit dem Namen der statischen Erweiterungsmethode</span><span class="sxs-lookup"><span data-stu-id="ab850-129">A unique event identifier ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) with the name of the static extension method.</span></span>
* <span data-ttu-id="ab850-130">die Meldungsvorlage (eine benannte Formatzeichenfolge)</span><span class="sxs-lookup"><span data-stu-id="ab850-130">The message template (named format string).</span></span> 

<span data-ttu-id="ab850-131">Eine Anforderung der Indexseite der Beispiel-App legt Folgendes fest:</span><span class="sxs-lookup"><span data-stu-id="ab850-131">A request for the Index page of the sample app sets the:</span></span>

* <span data-ttu-id="ab850-132">Die Protokollierungsebene ist `Information`.</span><span class="sxs-lookup"><span data-stu-id="ab850-132">Log level to `Information`.</span></span>
* <span data-ttu-id="ab850-133">Die Ereignis-ID ist `1` mit dem Namen der `IndexPageRequested`-Methode.</span><span class="sxs-lookup"><span data-stu-id="ab850-133">Event id to `1` with the name of the `IndexPageRequested` method.</span></span>
* <span data-ttu-id="ab850-134">Die Meldungsvorlage (eine benannte Formatzeichenfolge) ist eine Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="ab850-134">Message template (named format string) to a string.</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet5)]

<span data-ttu-id="ab850-135">Strukturierte Protokollspeicher verwenden möglicherweise den Ereignisnamen wenn dieser mit der Ereignis-ID bereitgestellt wird. Dadurch wird die Protokollierung ergänzt.</span><span class="sxs-lookup"><span data-stu-id="ab850-135">Structured logging stores may use the event name when it's supplied with the event id to enrich logging.</span></span> <span data-ttu-id="ab850-136">[Serilog](https://github.com/serilog/serilog-extensions-logging) verwendet z.B. den Ereignisnamen.</span><span class="sxs-lookup"><span data-stu-id="ab850-136">For example, [Serilog](https://github.com/serilog/serilog-extensions-logging) uses the event name.</span></span>

<span data-ttu-id="ab850-137">Das `Action`-Objekt wird mit einer stark typisierten Erweiterungsmethode aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="ab850-137">The `Action` is invoked through a strongly-typed extension method.</span></span> <span data-ttu-id="ab850-138">Die `IndexPageRequested`-Methode protokolliert eine Meldung für eine GET-Anforderung der Indexseite in der Beispiel-App:</span><span class="sxs-lookup"><span data-stu-id="ab850-138">The `IndexPageRequested` method logs a message for an Index page GET request in the sample app:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet9)]

<span data-ttu-id="ab850-139">`IndexPageRequested` wird in der Protokollierung in der `OnGetAsync`-Methode in *Pages/Index.cshtml.cs* aufgerufen:</span><span class="sxs-lookup"><span data-stu-id="ab850-139">`IndexPageRequested` is called on the logger in the `OnGetAsync` method in *Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

<span data-ttu-id="ab850-140">Sehen Sie sich folgende Konsolenausgabe der App an:</span><span class="sxs-lookup"><span data-stu-id="ab850-140">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

<span data-ttu-id="ab850-141">Definieren Sie bis zu sechs Typen, wenn Sie das statische Feld erstellen, um Parameter an eine Protokollmeldung zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="ab850-141">To pass parameters to a log message, define up to six types when creating the static field.</span></span> <span data-ttu-id="ab850-142">Die Beispiel-App protokolliert eine Zeichenfolge beim Hinzufügen eines Zitats durch die Definition eines `string`-Typs für das `Action`-Feld:</span><span class="sxs-lookup"><span data-stu-id="ab850-142">The sample app logs a string when adding a quote by defining a `string` type for the `Action` field:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet2)]

<span data-ttu-id="ab850-143">Die Protokollmeldungsvorlage des Delegaten erhält ihre Platzhalterwerte von den bereitgestellten Typen.</span><span class="sxs-lookup"><span data-stu-id="ab850-143">The delegate's log message template receives its placeholder values from the types provided.</span></span> <span data-ttu-id="ab850-144">Die Beispiel-App definiert einen Delegaten zu Hinzufügen eines Zitats, wo der Zitatparameter ein `string`-Objekt ist:</span><span class="sxs-lookup"><span data-stu-id="ab850-144">The sample app defines a delegate for adding a quote where the quote parameter is a `string`:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet6)]

<span data-ttu-id="ab850-145">`QuoteAdded`, die statische Erweiterungsmethode zum Hinzufügen von Zitaten, erhält den Wert des Zitatarguments und übergibt diesen an den `Action`-Delegaten:</span><span class="sxs-lookup"><span data-stu-id="ab850-145">The static extension method for adding a quote, `QuoteAdded`, receives the quote argument value and passes it to the `Action` delegate:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet10)]

<span data-ttu-id="ab850-146">Im Seitenmodell der Indexseite (*Pages/Index.cshtml.cs*) wird `QuoteAdded` aufgerufen, um die Meldung zu protokollieren:</span><span class="sxs-lookup"><span data-stu-id="ab850-146">In the Index page's page model (*Pages/Index.cshtml.cs*), `QuoteAdded` is called to log the message:</span></span>

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

<span data-ttu-id="ab850-147">Sehen Sie sich folgende Konsolenausgabe der App an:</span><span class="sxs-lookup"><span data-stu-id="ab850-147">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand')
```

<span data-ttu-id="ab850-148">Die Beispiel-App implementiert ein `try`&ndash;`catch`-Muster zum Löschen von Zitaten.</span><span class="sxs-lookup"><span data-stu-id="ab850-148">The sample app implements a `try`&ndash;`catch` pattern for quote deletion.</span></span> <span data-ttu-id="ab850-149">Es wird eine Meldung protokolliert, die angibt, dass der Löschvorgang erfolgreich war.</span><span class="sxs-lookup"><span data-stu-id="ab850-149">An informational message is logged for a successful delete operation.</span></span> <span data-ttu-id="ab850-150">Es wird eine Fehlermeldung protokolliert, die angibt, dass bei einem Löschvorgang eine Ausnahme ausgelöst wurde.</span><span class="sxs-lookup"><span data-stu-id="ab850-150">An error message is logged for a delete operation when an exception is thrown.</span></span> <span data-ttu-id="ab850-151">Die Protokollmeldung für den fehlgeschlagenen Löschvorgang enthält die Ausnahmenprotokollnachverfolgung (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="ab850-151">The log message for the unsuccessful delete operation includes the exception stack trace (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet7)]

<span data-ttu-id="ab850-152">Beachten Sie, wie die Ausnahme an den Delegaten in `QuoteDeleteFailed` übergeben wird:</span><span class="sxs-lookup"><span data-stu-id="ab850-152">Note how the exception is passed to the delegate in `QuoteDeleteFailed`:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet11)]

<span data-ttu-id="ab850-153">Im Seitenmodell für die Indexseite ruft ein erfolgreicher Zitatlöschvorgang die `QuoteDeleted`-Methode in der Protokollierung auf:</span><span class="sxs-lookup"><span data-stu-id="ab850-153">In the page model for the Index page, a successful quote deletion calls the `QuoteDeleted` method on the logger.</span></span> <span data-ttu-id="ab850-154">Wenn kein zu löschendes Zitat gefunden wird, wir eine `ArgumentNullException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="ab850-154">When a quote isn't found for deletion, an `ArgumentNullException` is thrown.</span></span> <span data-ttu-id="ab850-155">Die Ausnahme wird von der `try`&ndash;`catch`-Anweisung aufgefangen und durch einen Aufruf der `QuoteDeleteFailed`-Methode in der Protokollierung im `catch`-Block protokolliert (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="ab850-155">The exception is trapped by the `try`&ndash;`catch` statement and logged by calling the `QuoteDeleteFailed` method on the logger in the `catch` block (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet5&highlight=14,18)]

<span data-ttu-id="ab850-156">Wenn ein Zitat erfolgreich gelöscht wurde, sehen Sie sich die Konsolenausgabe der App an:</span><span class="sxs-lookup"><span data-stu-id="ab850-156">When a quote is successfully deleted, inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand' Id = 1)
```

<span data-ttu-id="ab850-157">Wenn der Zitatlöschvorgang fehlschlägt, sehen Sie sich die Konsolenausgabe der App an.</span><span class="sxs-lookup"><span data-stu-id="ab850-157">When quote deletion fails, inspect the app's console output.</span></span> <span data-ttu-id="ab850-158">Beachten Sie, dass die Ausnahme in der Protokollmeldung beinhaltet ist:</span><span class="sxs-lookup"><span data-stu-id="ab850-158">Note that the exception is included in the log message:</span></span>

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

## <a name="loggermessagedefinescope"></a><span data-ttu-id="ab850-159">LoggerMessage.DefineScope</span><span class="sxs-lookup"><span data-stu-id="ab850-159">LoggerMessage.DefineScope</span></span>

<span data-ttu-id="ab850-160">[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) erstellt einen `Func`-Delegaten zum Definieren eines [Protokollbereichs](xref:fundamentals/logging/index#log-scopes).</span><span class="sxs-lookup"><span data-stu-id="ab850-160">[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) creates a `Func` delegate for defining a [log scope](xref:fundamentals/logging/index#log-scopes).</span></span> <span data-ttu-id="ab850-161">`DefineScope` überlädt die Zulassung und übergibt bis zu drei Typparameter an eine benannte Formatzeichenfolge (Vorlage).</span><span class="sxs-lookup"><span data-stu-id="ab850-161">`DefineScope` overloads permit passing up to three type parameters to a named format string (template).</span></span>

<span data-ttu-id="ab850-162">Wie bei der Methode `Define` ist die Zeichenfolge,die für die Methode `DefineScope` bereitgestellt wurde, eine Vorlage und keine interpolierte Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="ab850-162">As is the case with the `Define` method, the string provided to the `DefineScope` method is a template and not an interpolated string.</span></span> <span data-ttu-id="ab850-163">Platzhalter werden in der Reihenfolge der angegebenen Typen ersetzt.</span><span class="sxs-lookup"><span data-stu-id="ab850-163">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="ab850-164">Die Platzhalternamen in den Vorlagen sollten eindeutig und in allen Vorlagen einheitlich sein.</span><span class="sxs-lookup"><span data-stu-id="ab850-164">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="ab850-165">Sie fungieren als Eigenschaftennamen in strukturierten Protokolldaten.</span><span class="sxs-lookup"><span data-stu-id="ab850-165">They serve as property names within structured log data.</span></span> <span data-ttu-id="ab850-166">Es wird empfohlen, für Platzhalternamen die [Pascal-Schreibweise](/dotnet/standard/design-guidelines/capitalization-conventions) zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="ab850-166">We recommend [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) for placeholder names.</span></span> <span data-ttu-id="ab850-167">Platzhalter in einer derartigen Schreibweise sind z.B. `{Count}` und `{FirstName}`.</span><span class="sxs-lookup"><span data-stu-id="ab850-167">For example, `{Count}`, `{FirstName}`.</span></span>

<span data-ttu-id="ab850-168">Definieren Sie einen [Protokollbereich](xref:fundamentals/logging/index#log-scopes), um mehrere Protokollmeldungen mit der [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope)-Methode anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="ab850-168">Define a [log scope](xref:fundamentals/logging/index#log-scopes) to apply to a series of log messages using the [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) method.</span></span>

<span data-ttu-id="ab850-169">Die Beispiel-App verfügt über die Schaltfläche **Clear all** (Alles löschen) zum Löschen aller Zitate in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="ab850-169">The sample app has a **Clear All** button for deleting all of the quotes in the database.</span></span> <span data-ttu-id="ab850-170">Die Zitate werden nacheinander entfernt und gelöscht.</span><span class="sxs-lookup"><span data-stu-id="ab850-170">The quotes are deleted by removing them one at a time.</span></span> <span data-ttu-id="ab850-171">Bei jedem Zitatlöschvorgang wird die `QuoteDeleted`-Methode in der Protokollierung aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="ab850-171">Each time a quote is deleted, the `QuoteDeleted` method is called on the logger.</span></span> <span data-ttu-id="ab850-172">Diesen Protokollmeldung wird ein Protokollbereich hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="ab850-172">A log scope is added to these log messages.</span></span>

<span data-ttu-id="ab850-173">Aktivieren Sie `IncludeScopes` in der Datei *appsettings.json* im Abschnitt „Konsolenprotokollierung“:</span><span class="sxs-lookup"><span data-stu-id="ab850-173">Enable `IncludeScopes` in the console logger section of *appsettings.json*:</span></span>

[!code-csharp[](loggermessage/sample/appsettings.json?highlight=3-5)]

<span data-ttu-id="ab850-174">Fügen Sie zum Erstellen eines Protokollbereichs ein Feld hinzu, dass einen `Func`-Delegaten für den Bereich enthält.</span><span class="sxs-lookup"><span data-stu-id="ab850-174">To create a log scope, add a field to hold a `Func` delegate for the scope.</span></span> <span data-ttu-id="ab850-175">Die Beispiel-App erstellt ein Feld mit dem Namen `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="ab850-175">The sample app creates a field called `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet4)]

<span data-ttu-id="ab850-176">Verwenden Sie `DefineScope` zum Erstellen eines Delegaten.</span><span class="sxs-lookup"><span data-stu-id="ab850-176">Use `DefineScope` to create the delegate.</span></span> <span data-ttu-id="ab850-177">Sie können bis zu drei Typen als Vorlagenargumente festlegen, die verwendet werden können, wenn der Delegat aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="ab850-177">Up to three types can be specified for use as template arguments when the delegate is invoked.</span></span> <span data-ttu-id="ab850-178">Die Beispiel-App verwendet eine Meldungsvorlage, die die Anzahl der gelöschten Zitate enthält (ein `int`-Typ):</span><span class="sxs-lookup"><span data-stu-id="ab850-178">The sample app uses a message template that includes the number of deleted quotes (an `int` type):</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet8)]

<span data-ttu-id="ab850-179">Stellen Sie eine statische Erweiterungsmethode für die Protokollmeldung bereit.</span><span class="sxs-lookup"><span data-stu-id="ab850-179">Provide a static extension method for the log message.</span></span> <span data-ttu-id="ab850-180">Beziehen Sie Typparameter für benannte Eigenschaften mit ein, die in der Meldungsvorlage vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="ab850-180">Include any type parameters for named properties that appear in the message template.</span></span> <span data-ttu-id="ab850-181">Die Beispiel-App erstellt ein `count` der zu löschenden Zitate und gibt `_allQuotesDeletedScope` zurück:</span><span class="sxs-lookup"><span data-stu-id="ab850-181">The sample app takes in a `count` of quotes to delete and returns `_allQuotesDeletedScope`:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet12)]

<span data-ttu-id="ab850-182">Der Bereich umschließt die Protokollerweiterungsaufrufe in einem `using`-Block:</span><span class="sxs-lookup"><span data-stu-id="ab850-182">The scope wraps the logging extension calls in a `using` block:</span></span>

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

<span data-ttu-id="ab850-183">Sehen Sie sich die Protokollmeldungen in der Konsolenausgabe der App an.</span><span class="sxs-lookup"><span data-stu-id="ab850-183">Inspect the log messages in the app's console output.</span></span> <span data-ttu-id="ab850-184">Das folgende Ergebnis zeigt drei gelöschte Zitate sowie die Protokollbereichsmeldung:</span><span class="sxs-lookup"><span data-stu-id="ab850-184">The following result shows three quotes deleted with the log scope message included:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="ab850-185">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="ab850-185">Additional resources</span></span>

* [<span data-ttu-id="ab850-186">Logging (Protokollierung)</span><span class="sxs-lookup"><span data-stu-id="ab850-186">Logging</span></span>](xref:fundamentals/logging/index)
