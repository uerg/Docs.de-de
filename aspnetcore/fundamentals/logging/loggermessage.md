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
# <a name="high-performance-logging-with-loggermessage-in-aspnet-core"></a><span data-ttu-id="ebb30-103">Hohe Leistung Protokollierung mit LoggerMessage in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ebb30-103">High-performance logging with LoggerMessage in ASP.NET Core</span></span>

<span data-ttu-id="ebb30-104">Von [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ebb30-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ebb30-105">[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) Funktionen erstellen zwischengespeichert werden Delegaten, in denen weniger objektzuweisungen und rechenleistung erfordern als reduziert [Protokollierung Erweiterungsmethoden](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), wie z. B. `LogInformation`, `LogDebug`, und `LogError`.</span><span class="sxs-lookup"><span data-stu-id="ebb30-105">[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) features create cacheable delegates that require fewer object allocations and reduced computational overhead than [logger extension methods](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), such as `LogInformation`, `LogDebug`, and `LogError`.</span></span> <span data-ttu-id="ebb30-106">Verwenden Sie für hohe Leistung Protokollierungsszenarien, die `LoggerMessage` Muster.</span><span class="sxs-lookup"><span data-stu-id="ebb30-106">For high-performance logging scenarios, use the `LoggerMessage` pattern.</span></span>

<span data-ttu-id="ebb30-107">`LoggerMessage`bietet die folgenden Leistungsvorteile über Erweiterungsmethoden für die Protokollierung:</span><span class="sxs-lookup"><span data-stu-id="ebb30-107">`LoggerMessage` provides the following performance advantages over Logger extension methods:</span></span>

* <span data-ttu-id="ebb30-108">Erweiterungsmethoden für die Protokollierung erfordern "Boxing" (die Konvertierung von) Werttypen, wie z. B. `int`, in `object`.</span><span class="sxs-lookup"><span data-stu-id="ebb30-108">Logger extension methods require "boxing" (converting) value types, such as `int`, into `object`.</span></span> <span data-ttu-id="ebb30-109">Die `LoggerMessage` Muster vermeidet Boxing mit statischen `Action` Felder und Erweiterungsmethoden, mit stark typisierten Parametern.</span><span class="sxs-lookup"><span data-stu-id="ebb30-109">The `LoggerMessage` pattern avoids boxing by using static `Action` fields and extension methods with strongly-typed parameters.</span></span>
* <span data-ttu-id="ebb30-110">Erweiterungsmethoden für die Protokollierung müssen die Nachrichtenvorlage (benannte Formatzeichenfolge) analysieren, jedes Mal eine protokollmeldung geschrieben wird.</span><span class="sxs-lookup"><span data-stu-id="ebb30-110">Logger extension methods must parse the message template (named format string) every time a log message is written.</span></span> <span data-ttu-id="ebb30-111">`LoggerMessage`nur erfordert eine Vorlage einmal analysieren, wenn die Nachricht definiert ist.</span><span class="sxs-lookup"><span data-stu-id="ebb30-111">`LoggerMessage` only requires parsing a template once when the message is defined.</span></span>

<span data-ttu-id="ebb30-112">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ebb30-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="ebb30-113">Die Beispiel-app veranschaulicht `LoggerMessage` Funktionen mit einem einfachen Anführungszeichen Überwachungssystem.</span><span class="sxs-lookup"><span data-stu-id="ebb30-113">The sample app demonstrates `LoggerMessage` features with a basic quote tracking system.</span></span> <span data-ttu-id="ebb30-114">Die app hinzufügt und löscht Anführungszeichen, die unter Verwendung einer Datenbank im Arbeitsspeicher.</span><span class="sxs-lookup"><span data-stu-id="ebb30-114">The app adds and deletes quotes using an in-memory database.</span></span> <span data-ttu-id="ebb30-115">Bei diesen Vorgängen auftreten, protokollmeldungen werden generiert, mit der `LoggerMessage` Muster.</span><span class="sxs-lookup"><span data-stu-id="ebb30-115">As these operations occur, log messages are generated using the `LoggerMessage` pattern.</span></span>

## <a name="loggermessagedefine"></a><span data-ttu-id="ebb30-116">LoggerMessage.Define</span><span class="sxs-lookup"><span data-stu-id="ebb30-116">LoggerMessage.Define</span></span>

<span data-ttu-id="ebb30-117">[Definieren von (LogLevel, EventId, String)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) erstellt eine `Action` delegate für eine Nachricht protokollieren.</span><span class="sxs-lookup"><span data-stu-id="ebb30-117">[Define(LogLevel, EventId, String)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) creates an `Action` delegate for logging a message.</span></span> <span data-ttu-id="ebb30-118">`Define`Überladungen ist die Übergabe von bis zu sechs Typparameter auf eine benannte Formatzeichenfolge (Vorlage) zulässig.</span><span class="sxs-lookup"><span data-stu-id="ebb30-118">`Define` overloads permit passing up to six type parameters to a named format string (template).</span></span>

## <a name="loggermessagedefinescope"></a><span data-ttu-id="ebb30-119">LoggerMessage.DefineScope</span><span class="sxs-lookup"><span data-stu-id="ebb30-119">LoggerMessage.DefineScope</span></span>

<span data-ttu-id="ebb30-120">[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) erstellt eine `Func` delegieren Sie zum Definieren einer [protokollieren Bereich](xref:fundamentals/logging/index#log-scopes).</span><span class="sxs-lookup"><span data-stu-id="ebb30-120">[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) creates a `Func` delegate for defining a [log scope](xref:fundamentals/logging/index#log-scopes).</span></span> <span data-ttu-id="ebb30-121">`DefineScope`Überladungen ist die Übergabe von bis zu drei Parameter auf eine benannte Formatzeichenfolge (Vorlage) zulässig.</span><span class="sxs-lookup"><span data-stu-id="ebb30-121">`DefineScope` overloads permit passing up to three type parameters to a named format string (template).</span></span>

## <a name="message-template-named-format-string"></a><span data-ttu-id="ebb30-122">Nachrichtenvorlage (mit dem Namen Formatzeichenfolge)</span><span class="sxs-lookup"><span data-stu-id="ebb30-122">Message template (named format string)</span></span>

<span data-ttu-id="ebb30-123">Die Zeichenfolge bereitgestellt, um die `Define` und `DefineScope` Methoden ist eine Vorlage und nicht für eine interpolierte Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="ebb30-123">The string provided to the `Define` and `DefineScope` methods is a template and not an interpolated string.</span></span> <span data-ttu-id="ebb30-124">Platzhalter werden in der Reihenfolge gefüllt, dass die Typen angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="ebb30-124">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="ebb30-125">Platzhalternamen in der Vorlage sollte in Vorlagen beschreibende und konsistent sein.</span><span class="sxs-lookup"><span data-stu-id="ebb30-125">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="ebb30-126">Sie dienen als Eigenschaftsnamen in strukturierten Protokolldaten.</span><span class="sxs-lookup"><span data-stu-id="ebb30-126">They serve as property names within structured log data.</span></span> <span data-ttu-id="ebb30-127">Wir empfehlen [Pascal Schreibweise](/dotnet/standard/design-guidelines/capitalization-conventions) für Platzhalternamen.</span><span class="sxs-lookup"><span data-stu-id="ebb30-127">We recommend [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) for placeholder names.</span></span> <span data-ttu-id="ebb30-128">Beispielsweise `{Count}`, `{FirstName}`.</span><span class="sxs-lookup"><span data-stu-id="ebb30-128">For example, `{Count}`, `{FirstName}`.</span></span>

## <a name="implementing-loggermessagedefine"></a><span data-ttu-id="ebb30-129">Implementieren von LoggerMessage.Define</span><span class="sxs-lookup"><span data-stu-id="ebb30-129">Implementing LoggerMessage.Define</span></span>

<span data-ttu-id="ebb30-130">Jede Nachricht Protokoll ist ein `Action` frei, die in einem statischen Feld erstellt, indem `LoggerMessage.Define`.</span><span class="sxs-lookup"><span data-stu-id="ebb30-130">Each log message is an `Action` held in a static field created by `LoggerMessage.Define`.</span></span> <span data-ttu-id="ebb30-131">Die Beispiel-app erstellt z. B. ein Feld, um eine protokollmeldung für eine GET-Anforderung für die Indexseite beschreiben (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="ebb30-131">For example, the sample app creates a field to describe a log message for a GET request for the Index page (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet1)]

<span data-ttu-id="ebb30-132">Für die `Action`, angeben:</span><span class="sxs-lookup"><span data-stu-id="ebb30-132">For the `Action`, specify:</span></span>

* <span data-ttu-id="ebb30-133">Die Protokollebene.</span><span class="sxs-lookup"><span data-stu-id="ebb30-133">The log level.</span></span>
* <span data-ttu-id="ebb30-134">Ein eindeutiger Bezeichner ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) mit dem Namen der statischen Erweiterungsmethode.</span><span class="sxs-lookup"><span data-stu-id="ebb30-134">A unique event identifier ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) with the name of the static extension method.</span></span>
* <span data-ttu-id="ebb30-135">Die Message-Vorlage (mit dem Namen Formatzeichenfolge).</span><span class="sxs-lookup"><span data-stu-id="ebb30-135">The message template (named format string).</span></span> 

<span data-ttu-id="ebb30-136">Eine Anforderung für die Indexseite, der im Beispiel-app wird die:</span><span class="sxs-lookup"><span data-stu-id="ebb30-136">A request for the Index page of the sample app sets the:</span></span>

* <span data-ttu-id="ebb30-137">Die Protokollebene auf `Information`.</span><span class="sxs-lookup"><span data-stu-id="ebb30-137">Log level to `Information`.</span></span>
* <span data-ttu-id="ebb30-138">Ereignis-Id für `1` durch den Namen der `IndexPageRequested` Methode.</span><span class="sxs-lookup"><span data-stu-id="ebb30-138">Event id to `1` with the name of the `IndexPageRequested` method.</span></span>
* <span data-ttu-id="ebb30-139">Nachrichtenvorlage (mit dem Namen Formatzeichenfolge) in eine Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="ebb30-139">Message template (named format string) to a string.</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet5)]

<span data-ttu-id="ebb30-140">Speichert strukturierte Protokollierung können den Namen des Ereignisses verwenden, wenn sie mit der Ereignis-Id für die Protokollierung anzureichern bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="ebb30-140">Structured logging stores may use the event name when it's supplied with the event id to enrich logging.</span></span> <span data-ttu-id="ebb30-141">Beispielsweise [Serilog](https://github.com/serilog/serilog-extensions-logging) verwendet den Namen des Ereignisses.</span><span class="sxs-lookup"><span data-stu-id="ebb30-141">For example, [Serilog](https://github.com/serilog/serilog-extensions-logging) uses the event name.</span></span>

<span data-ttu-id="ebb30-142">Die `Action` wird durch eine stark typisierte Erweiterungsmethode aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="ebb30-142">The `Action` is invoked through a strongly-typed extension method.</span></span> <span data-ttu-id="ebb30-143">Die `IndexPageRequested` -Methode protokolliert eine Meldung für die GET-Anforderung eine Index-Seite in der Beispiel-app:</span><span class="sxs-lookup"><span data-stu-id="ebb30-143">The `IndexPageRequested` method logs a message for an Index page GET request in the sample app:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet9)]

<span data-ttu-id="ebb30-144">`IndexPageRequested`wird aufgerufen, für die Protokollierung in die `OnGetAsync` Methode im *Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="ebb30-144">`IndexPageRequested` is called on the logger in the `OnGetAsync` method in *Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

<span data-ttu-id="ebb30-145">Überprüfen Sie die app-Konsolenausgabe:</span><span class="sxs-lookup"><span data-stu-id="ebb30-145">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

<span data-ttu-id="ebb30-146">Zum Übergeben von Parametern zu einer protokollmeldung, können definieren Sie bis zu sechs Typen, wenn das statische Feld zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="ebb30-146">To pass parameters to a log message, define up to six types when creating the static field.</span></span> <span data-ttu-id="ebb30-147">Die Beispiel-app protokolliert eine Zeichenfolge beim Hinzufügen eines Angebots durch Definieren einer `string` Geben Sie für die `Action` Feld:</span><span class="sxs-lookup"><span data-stu-id="ebb30-147">The sample app logs a string when adding a quote by defining a `string` type for the `Action` field:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet2)]

<span data-ttu-id="ebb30-148">Der Delegat Protokoll Nachrichtenvorlage empfängt die Platzhalterwerte von bereitgestellten Typen.</span><span class="sxs-lookup"><span data-stu-id="ebb30-148">The delegate's log message template receives its placeholder values from the types provided.</span></span> <span data-ttu-id="ebb30-149">Die Beispiel-app definiert einen Delegaten für das Hinzufügen eines Angebots, in dem der Angebots-Parameter ist, ein `string`:</span><span class="sxs-lookup"><span data-stu-id="ebb30-149">The sample app defines a delegate for adding a quote where the quote parameter is a `string`:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet6)]

<span data-ttu-id="ebb30-150">Die statische Erweiterungsmethode zum Hinzufügen eines Angebots `QuoteAdded`, empfängt den Argumentwert Angebot und übergibt es an die `Action` delegieren:</span><span class="sxs-lookup"><span data-stu-id="ebb30-150">The static extension method for adding a quote, `QuoteAdded`, receives the quote argument value and passes it to the `Action` delegate:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet10)]

<span data-ttu-id="ebb30-151">In der Indexseite Code-Behind-Datei (*Pages/Index.cshtml.cs*), `QuoteAdded` wird aufgerufen, um die Protokollierung der Meldung:</span><span class="sxs-lookup"><span data-stu-id="ebb30-151">In the Index page's code-behind file (*Pages/Index.cshtml.cs*), `QuoteAdded` is called to log the message:</span></span>

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

<span data-ttu-id="ebb30-152">Überprüfen Sie die app-Konsolenausgabe:</span><span class="sxs-lookup"><span data-stu-id="ebb30-152">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand')
```

<span data-ttu-id="ebb30-153">Implementiert die Beispiel-app eine `try` &ndash; `catch` Muster zum Angebot löschen.</span><span class="sxs-lookup"><span data-stu-id="ebb30-153">The sample app implements a `try`&ndash;`catch` pattern for quote deletion.</span></span> <span data-ttu-id="ebb30-154">Für eine erfolgreiche Löschvorgang ist eine informative Meldung protokolliert.</span><span class="sxs-lookup"><span data-stu-id="ebb30-154">An informational message is logged for a successful delete operation.</span></span> <span data-ttu-id="ebb30-155">Für einen Löschvorgang an wird eine Fehlermeldung protokolliert, wenn eine Ausnahme ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="ebb30-155">An error message is logged for a delete operation when an exception is thrown.</span></span> <span data-ttu-id="ebb30-156">Die protokollmeldung für den fehlgeschlagenen Vorgang löschen enthält die stapelüberwachung der Ausnahme (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="ebb30-156">The log message for the unsuccessful delete operation includes the exception stack trace (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet7)]

<span data-ttu-id="ebb30-157">Beachten Sie, wie die Ausnahme übergeben wird, an den Delegaten in `QuoteDeleteFailed`:</span><span class="sxs-lookup"><span data-stu-id="ebb30-157">Note how the exception is passed to the delegate in `QuoteDeleteFailed`:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet11)]

<span data-ttu-id="ebb30-158">In den Index Seite Code-Behind, Löschvorgang erfolgreich Angebot Ruft die `QuoteDeleted` Methode für die Protokollierung.</span><span class="sxs-lookup"><span data-stu-id="ebb30-158">In the Index page code-behind, a successful quote deletion calls the `QuoteDeleted` method on the logger.</span></span> <span data-ttu-id="ebb30-159">Wenn ein Angebot zum Löschen, nicht gefunden wird eine `ArgumentNullException` ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="ebb30-159">When a quote isn't found for deletion, an `ArgumentNullException` is thrown.</span></span> <span data-ttu-id="ebb30-160">Die Ausnahme abgefangen wird, durch die `try` &ndash; `catch` Anweisung und protokolliert Sie durch Aufrufen der `QuoteDeleteFailed` Methode für die Protokollierung in die `catch` Block (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="ebb30-160">The exception is trapped by the `try`&ndash;`catch` statement and logged by calling the `QuoteDeleteFailed` method on the logger in the `catch` block (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet5&highlight=14,18)]

<span data-ttu-id="ebb30-161">Wenn ein Angebot wurde erfolgreich gelöscht wird, überprüfen Sie die app-Konsolenausgabe:</span><span class="sxs-lookup"><span data-stu-id="ebb30-161">When a quote is successfully deleted, inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand' Id = 1)
```

<span data-ttu-id="ebb30-162">Beim Angebot löschen ein Fehler auftritt, überprüfen Sie die app-Konsolenausgabe.</span><span class="sxs-lookup"><span data-stu-id="ebb30-162">When quote deletion fails, inspect the app's console output.</span></span> <span data-ttu-id="ebb30-163">Beachten Sie, dass die Ausnahme in der Protokoll-Nachricht enthalten ist:</span><span class="sxs-lookup"><span data-stu-id="ebb30-163">Note that the exception is included in the log message:</span></span>

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

## <a name="implementing-loggermessagedefinescope"></a><span data-ttu-id="ebb30-164">Implementieren von LoggerMessage.DefineScope</span><span class="sxs-lookup"><span data-stu-id="ebb30-164">Implementing LoggerMessage.DefineScope</span></span>

<span data-ttu-id="ebb30-165">Definieren einer [protokollieren Bereich](xref:fundamentals/logging/index#log-scopes) anwenden auf eine Reihe von protokollmeldungen, die mit der [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) Methode.</span><span class="sxs-lookup"><span data-stu-id="ebb30-165">Define a [log scope](xref:fundamentals/logging/index#log-scopes) to apply to a series of log messages using the [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) method.</span></span>

<span data-ttu-id="ebb30-166">Die Beispiel-app hat einen **alle löschen** Schaltfläche für alle Anführungszeichen in der Datenbank zu löschen.</span><span class="sxs-lookup"><span data-stu-id="ebb30-166">The sample app has a **Clear All** button for deleting all of the quotes in the database.</span></span> <span data-ttu-id="ebb30-167">Die Anführungszeichen werden gelöscht, indem Sie sie entfernen eine zu einem Zeitpunkt.</span><span class="sxs-lookup"><span data-stu-id="ebb30-167">The quotes are deleted by removing them one at a time.</span></span> <span data-ttu-id="ebb30-168">Jedes Mal ein Angebot gelöscht wird, die `QuoteDeleted` Methode für die Protokollierung aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="ebb30-168">Each time a quote is deleted, the `QuoteDeleted` method is called on the logger.</span></span> <span data-ttu-id="ebb30-169">Diese protokollmeldungen wird ein Protokoll-Bereich hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="ebb30-169">A log scope is added to these log messages.</span></span>

<span data-ttu-id="ebb30-170">Aktivieren Sie `IncludeScopes` in die Konsole Logger-Optionen:</span><span class="sxs-lookup"><span data-stu-id="ebb30-170">Enable `IncludeScopes` in the console logger options:</span></span>

[!code-csharp[Main](loggermessage/sample/Program.cs?name=snippet1&highlight=22)]

<span data-ttu-id="ebb30-171">Festlegen von `IncludeScopes` Protokoll Bereiche Aktivierung in ASP.NET Core 2.0-apps erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="ebb30-171">Setting `IncludeScopes` is required in ASP.NET Core 2.0 apps to enable log scopes.</span></span> <span data-ttu-id="ebb30-172">Festlegen von `IncludeScopes` über *"appSettings"* Konfigurationsdateien ist ein Feature, das für die Version 2.1 von ASP.NET Core geplant hat.</span><span class="sxs-lookup"><span data-stu-id="ebb30-172">Setting `IncludeScopes` via *appsettings* configuration files is a feature that's planned for the ASP.NET Core 2.1 release.</span></span>

<span data-ttu-id="ebb30-173">Die Beispiel-app löscht von anderen Anbietern und fügt Filter, um die Protokollausgabe zu reduzieren.</span><span class="sxs-lookup"><span data-stu-id="ebb30-173">The sample app clears other providers and adds filters to reduce the logging output.</span></span> <span data-ttu-id="ebb30-174">Dies erleichtert es, des Beispiels protokollmeldungen, die veranschaulichen, finden Sie unter `LoggerMessage` Funktionen.</span><span class="sxs-lookup"><span data-stu-id="ebb30-174">This makes it easier to see the sample's log messages that demonstrate `LoggerMessage` features.</span></span>

<span data-ttu-id="ebb30-175">Um einen Bereich des Protokolls zu erstellen, fügen Sie ein Feld zum Speichern einer `Func` für den Bereich zu delegieren.</span><span class="sxs-lookup"><span data-stu-id="ebb30-175">To create a log scope, add a field to hold a `Func` delegate for the scope.</span></span> <span data-ttu-id="ebb30-176">Die Beispiel-app erstellt, ein Feld namens `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="ebb30-176">The sample app creates a field called `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet4)]

<span data-ttu-id="ebb30-177">Verwendung `DefineScope` an den Delegaten zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="ebb30-177">Use `DefineScope` to create the delegate.</span></span> <span data-ttu-id="ebb30-178">Bis zu drei Typen können angegeben werden für die Verwendung als Vorlagenargumente beim Aufrufen des Delegaten.</span><span class="sxs-lookup"><span data-stu-id="ebb30-178">Up to three types can be specified for use as template arguments when the delegate is invoked.</span></span> <span data-ttu-id="ebb30-179">Die Beispiel-app verwendet eine Nachrichtenvorlage, die die Anzahl der gelöschten Anführungszeichen enthält (eine `int` Typ):</span><span class="sxs-lookup"><span data-stu-id="ebb30-179">The sample app uses a message template that includes the number of deleted quotes (an `int` type):</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet8)]

<span data-ttu-id="ebb30-180">Geben Sie eine statische Erweiterungsmethode für die protokollmeldung.</span><span class="sxs-lookup"><span data-stu-id="ebb30-180">Provide a static extension method for the log message.</span></span> <span data-ttu-id="ebb30-181">Schließen Sie Typparameter für den benannten Eigenschaften, die in der Meldungsvorlage ein</span><span class="sxs-lookup"><span data-stu-id="ebb30-181">Include any type parameters for named properties that appear in the message template.</span></span> <span data-ttu-id="ebb30-182">Die Beispiel-app nimmt eine `count` Anführungszeichen löschen und gibt `_allQuotesDeletedScope`:</span><span class="sxs-lookup"><span data-stu-id="ebb30-182">The sample app takes in a `count` of quotes to delete and returns `_allQuotesDeletedScope`:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet12)]

<span data-ttu-id="ebb30-183">Der Bereich umschließt, die die Erweiterung für die Protokollierung von in Aufrufen eine `using` blockieren:</span><span class="sxs-lookup"><span data-stu-id="ebb30-183">The scope wraps the logging extension calls in a `using` block:</span></span>

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

<span data-ttu-id="ebb30-184">Überprüfen Sie die protokollmeldungen in die app-Konsolenausgabe an.</span><span class="sxs-lookup"><span data-stu-id="ebb30-184">Inspect the log messages in the app's console output.</span></span> <span data-ttu-id="ebb30-185">Das folgende Ergebnis zeigt drei Angebote, die mit dem die protokollmeldung zum Bereich enthalten gelöscht:</span><span class="sxs-lookup"><span data-stu-id="ebb30-185">The following result shows three quotes deleted with the log scope message included:</span></span>

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

## <a name="see-also"></a><span data-ttu-id="ebb30-186">Siehe auch</span><span class="sxs-lookup"><span data-stu-id="ebb30-186">See also</span></span>

* [<span data-ttu-id="ebb30-187">Logging (Protokollierung)</span><span class="sxs-lookup"><span data-stu-id="ebb30-187">Logging</span></span>](xref:fundamentals/logging/index)
