---
title: Fehlerbehandlung in ASP.NET Core
author: ardalis
description: 'Gewusst wie: Behandeln von Fehlern in ASP.NET Core-Anwendungen zu ermitteln.'
ms.author: tdykstra
manager: wpickett
ms.date: 11/30/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/error-handling
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 49507e90cd659be5da08df17e175297adad0fea1
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-error-handling-in-aspnet-core"></a><span data-ttu-id="c006c-103">Einführung in die Error Handling in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c006c-103">Introduction to Error Handling in ASP.NET Core</span></span>

<span data-ttu-id="c006c-104">Von [Steve Smith](https://ardalis.com/) und [Tom Dykstra](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="c006c-104">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="c006c-105">Dieser Artikel behandelt allgemeine Appoaches zum Behandeln von Fehlern in ASP.NET Core-apps.</span><span class="sxs-lookup"><span data-stu-id="c006c-105">This article covers common appoaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="c006c-106">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c006c-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-developer-exception-page"></a><span data-ttu-id="c006c-107">Die Developer-Ausnahme-Seite</span><span class="sxs-lookup"><span data-stu-id="c006c-107">The developer exception page</span></span>

<span data-ttu-id="c006c-108">Installieren, um eine Anwendung, um eine Seite anzeigen, die zeigt detaillierte Informationen zu Ausnahmen Konfigurieren der `Microsoft.AspNetCore.Diagnostics` NuGet Verpacken, und fügen Sie eine Zeile, um die [Methoden konfigurieren, in die Startklasse](startup.md):</span><span class="sxs-lookup"><span data-stu-id="c006c-108">To configure an app to display a page that shows detailed information about exceptions, install the `Microsoft.AspNetCore.Diagnostics` NuGet package and add a line to the [Configure method in the Startup class](startup.md):</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

<span data-ttu-id="c006c-109">Put `UseDeveloperExceptionPage` vor Middleware, die Sie Ausnahmen, z. B. abfangen möchten `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="c006c-109">Put `UseDeveloperExceptionPage` before any middleware you want to catch exceptions in, such as `app.UseMvc`.</span></span>

>[!WARNING]
> <span data-ttu-id="c006c-110">Aktivieren Sie die Developer-Seite "Ausnahme" **nur, wenn die app in der Entwicklungsumgebung ausgeführt wird**.</span><span class="sxs-lookup"><span data-stu-id="c006c-110">Enable the developer exception page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="c006c-111">Sie möchten detaillierte Ausnahmeinformationen öffentlich freigeben, wenn die app in der produktionsumgebung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="c006c-111">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="c006c-112">[Weitere Informationen zum Konfigurieren von Umgebungen](environments.md).</span><span class="sxs-lookup"><span data-stu-id="c006c-112">[Learn more about configuring environments](environments.md).</span></span>

<span data-ttu-id="c006c-113">Um die Developer-Ausnahme-Seite angezeigt wird, führen Sie die beispielanwendung mit der Umgebung festgelegt `Development`, und fügen `?throw=true` an die Basis-URL der app.</span><span class="sxs-lookup"><span data-stu-id="c006c-113">To see the developer exception page, run the sample application with the environment set to `Development`, and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="c006c-114">Die Seite enthält mehrere Registerkarten mit Informationen über die Ausnahme und die Anforderung.</span><span class="sxs-lookup"><span data-stu-id="c006c-114">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="c006c-115">Die erste Registerkarte enthält eine stapelüberwachung.</span><span class="sxs-lookup"><span data-stu-id="c006c-115">The first tab includes a stack trace.</span></span> 

![Stapelüberwachung](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="c006c-117">Der Registerkarte "Weiter" zeigt die Abfrage Zeichenfolgenparametern, sofern vorhanden.</span><span class="sxs-lookup"><span data-stu-id="c006c-117">The next tab shows the query string parameters, if any.</span></span>

![Abfragezeichenfolgen-Parameter](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="c006c-119">Diese Anforderung keine Cookies haben, aber wenn dem so wäre, würden sie angezeigt, auf die **Cookies** Registerkarte. Sie können die Header, die in der Registerkarte "letzten" übergeben wurden, finden Sie unter.</span><span class="sxs-lookup"><span data-stu-id="c006c-119">This request didn't have any cookies, but if it did, they would appear on the **Cookies** tab. You can see the headers that were passed in the last tab.</span></span>

![Header](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a><span data-ttu-id="c006c-121">Konfigurieren einen benutzerdefinierte Ausnahmebehandlung-Seite</span><span class="sxs-lookup"><span data-stu-id="c006c-121">Configuring a custom exception handling page</span></span>

<span data-ttu-id="c006c-122">Es ist eine gute Idee, eine Ausnahme-Handler-Seite verwenden, wenn die app nicht, in ausgeführt wird "konfigurieren" die `Development` Umgebung.</span><span class="sxs-lookup"><span data-stu-id="c006c-122">It's a good idea to configure an exception handler page to use when the app is not running in the `Development` environment.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

<span data-ttu-id="c006c-123">In einer MVC-app nicht explizit ergänzen die Fehler Handler-Aktionsmethode mit HTTP-Attribute, z. B. `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="c006c-123">In an MVC app, don't explicitly decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="c006c-124">Verwendung von expliziten Verben könnte verhindern, dass manche Anforderungen erreichen die Methode.</span><span class="sxs-lookup"><span data-stu-id="c006c-124">Using explicit verbs could prevent some requests from reaching the method.</span></span>

```csharp
[Route("/Error")]
public IActionResult Index()
{
    // Handle error here
}
```

## <a name="configuring-status-code-pages"></a><span data-ttu-id="c006c-125">Konfigurieren von Status-Codepages</span><span class="sxs-lookup"><span data-stu-id="c006c-125">Configuring status code pages</span></span>

<span data-ttu-id="c006c-126">Standardmäßig wird Ihre app keine rich-Status-Codepage für HTTP-Statuscodes, wie z. B. 500 (Interner Serverfehler) oder 404 (Nichtgefunden) bereit.</span><span class="sxs-lookup"><span data-stu-id="c006c-126">By default, your app will not provide a rich status code page for HTTP status codes such as 500 (Internal Server Error) or 404 (Not Found).</span></span> <span data-ttu-id="c006c-127">Sie können konfigurieren, die `StatusCodePagesMiddleware` durch Hinzufügen einer Zeile auf die `Configure` Methode:</span><span class="sxs-lookup"><span data-stu-id="c006c-127">You can configure the `StatusCodePagesMiddleware` by adding a line to the `Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="c006c-128">Standardmäßig fügt diese Middleware einfache, nur-Text-Handler für die allgemeinen Statuscodes wie 404:</span><span class="sxs-lookup"><span data-stu-id="c006c-128">By default, this middleware adds simple, text-only handlers for common status codes, such as 404:</span></span>

![404-Seite](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="c006c-130">Die Middleware unterstützt mehrere verschiedene Erweiterungsmethoden.</span><span class="sxs-lookup"><span data-stu-id="c006c-130">The middleware supports several different extension methods.</span></span> <span data-ttu-id="c006c-131">Eine Überladung einen Lambda-Ausdruck, anderer eine Art und Format-Inhaltszeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="c006c-131">One takes a lambda expression, another takes a content type and format string.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="c006c-132">Es gibt auch Erweiterungsmethoden umleiten.</span><span class="sxs-lookup"><span data-stu-id="c006c-132">There are also redirect extension methods.</span></span> <span data-ttu-id="c006c-133">Eine einen Statuscode "302" an den Client sendet und eine den ursprünglichen Statuscode an den Client zurückgegeben, aber auch für die umleitungs-URL führt den Handler.</span><span class="sxs-lookup"><span data-stu-id="c006c-133">One sends a 302 status code to the client, and one returns the original status code to the client but also executes the handler for the redirect URL.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="c006c-134">Wenn in diesem Fall müssen Sie eine Status-Codepages für bestimmte Anforderungen zu deaktivieren, können Sie dies tun:</span><span class="sxs-lookup"><span data-stu-id="c006c-134">If you need to disable status code pages for certain requests, you can do so:</span></span>

```csharp
var statusCodePagesFeature = context.Features.Get<IStatusCodePagesFeature>();
if (statusCodePagesFeature != null)
{
  statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="c006c-135">Ausnahmebehandlungscode</span><span class="sxs-lookup"><span data-stu-id="c006c-135">Exception-handling code</span></span>

<span data-ttu-id="c006c-136">Code in Seiten für die Ausnahmebehandlung kann Ausnahmen auslösen.</span><span class="sxs-lookup"><span data-stu-id="c006c-136">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="c006c-137">Es ist häufig eine gute Idee für Produktion Fehlerseiten rein statischer Inhalte bestehen.</span><span class="sxs-lookup"><span data-stu-id="c006c-137">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="c006c-138">Bedenken Sie außerdem, nachdem die Header für eine Antwort gesendet wurden, Statuscode der Antwort kann nicht geändert werden, noch können Ausnahme-Seiten oder der Handler ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="c006c-138">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="c006c-139">Die Antwort muss abgeschlossen werden, oder die Verbindung abgebrochen.</span><span class="sxs-lookup"><span data-stu-id="c006c-139">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="c006c-140">Server-Ausnahmebehandlung</span><span class="sxs-lookup"><span data-stu-id="c006c-140">Server exception handling</span></span>

<span data-ttu-id="c006c-141">Zusätzlich zu der Logik in Ihrer app für die Ausnahmebehandlung der [Server](servers/index.md) hosten Ihre app führt einige Ausnahmebehandlung.</span><span class="sxs-lookup"><span data-stu-id="c006c-141">In addition to the exception handling logic in your app, the [server](servers/index.md) hosting your app performs some exception handling.</span></span> <span data-ttu-id="c006c-142">Wenn der Server eine Ausnahme abfängt, bevor die Header gesendet werden, sendet der Server eine 500 Internal Server Error Antwort ohne Text an.</span><span class="sxs-lookup"><span data-stu-id="c006c-142">If the server catches an exception before the headers are sent, the server sends a 500 Internal Server Error response with no body.</span></span> <span data-ttu-id="c006c-143">Wenn der Server eine Ausnahme abfängt, nachdem die Header gesendet wurden, schließt der Server die Verbindung an.</span><span class="sxs-lookup"><span data-stu-id="c006c-143">If the server catches an exception after the headers have been sent, the server closes the connection.</span></span> <span data-ttu-id="c006c-144">Vom Server werden Anforderungen, die von Ihrer Anwendung behandelt werden nicht behandelt.</span><span class="sxs-lookup"><span data-stu-id="c006c-144">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="c006c-145">Jede Ausnahme, die auftritt, erfolgt durch den Server-Ausnahme behandeln.</span><span class="sxs-lookup"><span data-stu-id="c006c-145">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="c006c-146">Eine benutzerdefinierte Fehlerseiten konfiguriert oder Ausnahmebehandlung Middleware oder Filter wirken sich nicht auf dieses Verhalten.</span><span class="sxs-lookup"><span data-stu-id="c006c-146">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="c006c-147">Start-Ausnahmebehandlung</span><span class="sxs-lookup"><span data-stu-id="c006c-147">Startup exception handling</span></span>

<span data-ttu-id="c006c-148">Nur der Hostebene kann Ausnahmen behandeln, die während des Starts der app erfolgen.</span><span class="sxs-lookup"><span data-stu-id="c006c-148">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="c006c-149">Sie können [konfigurieren, wie der Host als Antwort auf Fehler während des Starts verhält](hosting.md#detailed-errors) mit `captureStartupErrors` und die `detailedErrors` Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="c006c-149">You can [configure how the host behaves in response to errors during startup](hosting.md#detailed-errors) using `captureStartupErrors` and the `detailedErrors` key.</span></span>

<span data-ttu-id="c006c-150">Hosting kann nur anzeigen eine Fehlerseite für den erfassten beim Start ein Fehler tritt der Fehler auf, nachdem der Host-Adresse/Port Bindung.</span><span class="sxs-lookup"><span data-stu-id="c006c-150">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="c006c-151">Wenn eine Bindung aus irgendeinem Grund fehlschlägt, die Hostebene meldet eine kritische Ausnahme, die Dotnet Prozessabstürzen, und keine Fehler angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="c006c-151">If any binding fails for any reason, the hosting layer logs a critical exception, the dotnet process crashes, and no error page is displayed.</span></span>

## <a name="aspnet-mvc-error-handling"></a><span data-ttu-id="c006c-152">ASP.NET MVC-Fehlerbehandlung</span><span class="sxs-lookup"><span data-stu-id="c006c-152">ASP.NET MVC error handling</span></span>

<span data-ttu-id="c006c-153">[MVC](../mvc/index.md) apps haben einige zusätzlichen Optionen zur Behandlung von Fehlern, z. B. Ausnahmefilter konfigurieren und Ausführen von modellvalidierung.</span><span class="sxs-lookup"><span data-stu-id="c006c-153">[MVC](../mvc/index.md) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="c006c-154">Ausnahmefilter</span><span class="sxs-lookup"><span data-stu-id="c006c-154">Exception Filters</span></span>

<span data-ttu-id="c006c-155">Ausnahmefilter können global oder für eine pro Controller oder pro-Aktion in einer MVC-Anwendung konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="c006c-155">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="c006c-156">Diese Filter behandelt jede nicht behandelte Ausnahme, die während der Ausführung eine Controlleraktion oder einen anderen Filter tritt auf, und andernfalls nicht aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="c006c-156">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter, and are not called otherwise.</span></span> <span data-ttu-id="c006c-157">Erfahren Sie mehr über Ausnahmefilter in [Filter](../mvc/controllers/filters.md).</span><span class="sxs-lookup"><span data-stu-id="c006c-157">Learn more about exception filters in [Filters](../mvc/controllers/filters.md).</span></span>

>[!TIP]
> <span data-ttu-id="c006c-158">Ausnahmefilter eignen sich für Auffangen von Ausnahmen, die in MVC Aktionen auftreten, aber sie sind nicht so flexibel wie Middleware für die Fehlerbehandlung.</span><span class="sxs-lookup"><span data-stu-id="c006c-158">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="c006c-159">Middleware für die allgemeine Groß-/Kleinschreibung vorziehen, und verwenden Sie Filter, in denen Sie nur müssen Vorgehensweise Fehlerbehandlung *anders* basierend auf die MVC-Aktion ausgewählt wurde.</span><span class="sxs-lookup"><span data-stu-id="c006c-159">Prefer middleware for the general case, and use filters only where you need to do error handling *differently* based on which MVC action was chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="c006c-160">Modellstatus Behandeln von Fehlern</span><span class="sxs-lookup"><span data-stu-id="c006c-160">Handling Model State Errors</span></span>

<span data-ttu-id="c006c-161">[Modellüberprüfung](../mvc/models/validation.md) vor jeder Controlleraktion aufgerufen wurde, und es wird die Aktionsmethode dafür verantwortlich, um zu überprüfen `ModelState.IsValid` und entsprechend reagieren.</span><span class="sxs-lookup"><span data-stu-id="c006c-161">[Model validation](../mvc/models/validation.md) occurs prior to each controller action being invoked, and it is the action method’s responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="c006c-162">Einige apps werden in diesem Fall eine Standardkonvention für den Umgang mit modellvalidierungsfehler, folgen, wählen Sie eine [Filter](../mvc/controllers/filters.md) eine geeignete Stelle eine solche Richtlinie implementiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="c006c-162">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a [filter](../mvc/controllers/filters.md) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="c006c-163">Sie sollten das Verhalten Ihrer Aktionen mit ungültigen modellzustände testen.</span><span class="sxs-lookup"><span data-stu-id="c006c-163">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="c006c-164">Erfahren Sie mehr [testen Controllerlogik](../mvc/controllers/testing.md).</span><span class="sxs-lookup"><span data-stu-id="c006c-164">Learn more in [Testing controller logic](../mvc/controllers/testing.md).</span></span>



