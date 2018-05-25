---
title: Fehlerbehandlung in ASP.NET Core
author: ardalis
description: Erfahren Sie mehr über die Fehlerbehandlung in ASP.NET Core-Anwendungen.
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 11/30/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/error-handling
ms.openlocfilehash: 3ff3a17d14d9ed7c438399191ffe3cf93d555d49
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/17/2018
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="f1e44-103">Fehlerbehandlung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f1e44-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="f1e44-104">Von [Steve Smith](https://ardalis.com/) und [Tom Dykstra](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="f1e44-104">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="f1e44-105">Dieser Artikel beschreibt grundsätzliche Vorgehensweisen zur Behandlung von Fehlern in ASP.NET Core-Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="f1e44-105">This article covers common appoaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="f1e44-106">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f1e44-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-developer-exception-page"></a><span data-ttu-id="f1e44-107">Die Seite mit Ausnahmen für Entwickler</span><span class="sxs-lookup"><span data-stu-id="f1e44-107">The developer exception page</span></span>

<span data-ttu-id="f1e44-108">Wenn Sie eine App so konfigurieren möchten, dass eine Seite mit detaillierten Informationen zu Ausnahmen angezeigt wird, müssen Sie das NuGet-Paket `Microsoft.AspNetCore.Diagnostics` installieren und eine Zeile zur [Configure-Methode in der Startklasse](xref:fundamentals/startup) hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="f1e44-108">To configure an app to display a page that shows detailed information about exceptions, install the `Microsoft.AspNetCore.Diagnostics` NuGet package and add a line to the [Configure method in the Startup class](xref:fundamentals/startup):</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

<span data-ttu-id="f1e44-109">Fügen Sie `UseDeveloperExceptionPage` vor Middleware ein, in der Sie Ausnahmen abfangen möchten, wie z.B. `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="f1e44-109">Put `UseDeveloperExceptionPage` before any middleware you want to catch exceptions in, such as `app.UseMvc`.</span></span>

>[!WARNING]
> <span data-ttu-id="f1e44-110">Aktivieren Sie die Seite mit Ausnahmen für Entwickler **nur, wenn die App in der Entwicklungsumgebung ausgeführt wird**.</span><span class="sxs-lookup"><span data-stu-id="f1e44-110">Enable the developer exception page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="f1e44-111">Wenn die App in der Produktionsumgebung ausgeführt wird, sollten Sie keine detaillierten Ausnahmeinformationen öffentlich teilen.</span><span class="sxs-lookup"><span data-stu-id="f1e44-111">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="f1e44-112">[Weitere Informationen zum Konfigurieren von Umgebungen](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="f1e44-112">[Learn more about configuring environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="f1e44-113">Wenn Sie die Seite mit Ausnahmen für Entwickler anzeigen möchten, führen Sie die Beispielanwendung mit der auf `Development` festgelegten Umgebung aus, und fügen Sie `?throw=true` zur Basis-URL der App hinzu.</span><span class="sxs-lookup"><span data-stu-id="f1e44-113">To see the developer exception page, run the sample application with the environment set to `Development`, and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="f1e44-114">Die Seite enthält mehrere Registerkarten mit Informationen zu der Ausnahme und der Anforderung.</span><span class="sxs-lookup"><span data-stu-id="f1e44-114">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="f1e44-115">Die erste Registerkarte enthält eine Stapelüberwachung.</span><span class="sxs-lookup"><span data-stu-id="f1e44-115">The first tab includes a stack trace.</span></span> 

![Stapelüberwachung](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="f1e44-117">Auf der nächsten Registerkarte werden, sofern vorhanden, Abfragezeichenfolgeparameter angezeigt.</span><span class="sxs-lookup"><span data-stu-id="f1e44-117">The next tab shows the query string parameters, if any.</span></span>

![Abfragezeichenfolgeparameter](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="f1e44-119">Diese Anforderung enthielt keine Cookies. Andernfalls würden Sie auf der Registerkarte **Cookies** angezeigt. Sie können die Header sehen, die auf der letzten Registerkarte übergeben wurden.</span><span class="sxs-lookup"><span data-stu-id="f1e44-119">This request didn't have any cookies, but if it did, they would appear on the **Cookies** tab. You can see the headers that were passed in the last tab.</span></span>

![Header](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a><span data-ttu-id="f1e44-121">Konfigurieren einer benutzerdefinierten Seite zur Ausnahmebehandlung</span><span class="sxs-lookup"><span data-stu-id="f1e44-121">Configuring a custom exception handling page</span></span>

<span data-ttu-id="f1e44-122">Es ist eine gute Idee, eine Seite zur Ausnahmebehandlung zu konfigurieren, die verwendet werden kann, wenn die App nicht in der `Development`-Umgebung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="f1e44-122">It's a good idea to configure an exception handler page to use when the app isn't running in the `Development` environment.</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

<span data-ttu-id="f1e44-123">Die Aktionsmethode zur Fehlerbehandlung wird in einer MVC-App nicht explizit durch HTTP-Methodenattribute wie `HttpGet` ergänzt.</span><span class="sxs-lookup"><span data-stu-id="f1e44-123">In an MVC app, don't explicitly decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="f1e44-124">Durch die Verwendung expliziter Verben könnte bei einigen Anforderungen verhindert werden, dass diese Methode zum Einsatz kommt.</span><span class="sxs-lookup"><span data-stu-id="f1e44-124">Using explicit verbs could prevent some requests from reaching the method.</span></span>

```csharp
[Route("/Error")]
public IActionResult Index()
{
    // Handle error here
}
```

## <a name="configuring-status-code-pages"></a><span data-ttu-id="f1e44-125">Konfigurieren von Statuscodeseiten</span><span class="sxs-lookup"><span data-stu-id="f1e44-125">Configuring status code pages</span></span>

<span data-ttu-id="f1e44-126">Ihre App stellt für HTTP-Statuscodes wie *404 – Nicht gefunden* standardmäßig keine ausführliche Statuscodeseite zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="f1e44-126">By default, an app doesn't provide a rich status code page for HTTP status codes, such as *404 Not Found*.</span></span> <span data-ttu-id="f1e44-127">Um Statuscodeseiten bereitzustellen, muss die Middleware „Status Code Pages“ durch Hinzufügen einer Zeile zu `Startup.Configure` konfiguriert werden:</span><span class="sxs-lookup"><span data-stu-id="f1e44-127">To provide status code pages, configure the Status Code Pages Middleware by adding a line to the `Startup.Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="f1e44-128">Standardmäßig fügt die Middleware „Satus Code Pages“ für gängige Statuscodes wie 404 einfache Handler im Textformat hinzu:</span><span class="sxs-lookup"><span data-stu-id="f1e44-128">By default, Status Code Pages Middleware adds simple, text-only handlers for common status codes, such as 404:</span></span>

![404-Seite](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="f1e44-130">Die Middleware unterstützt zahlreiche Erweiterungsmethoden.</span><span class="sxs-lookup"><span data-stu-id="f1e44-130">The middleware supports several extension methods.</span></span> <span data-ttu-id="f1e44-131">Eine Methode verwendet einen Lambdaausdruck:</span><span class="sxs-lookup"><span data-stu-id="f1e44-131">One method takes a lambda expression:</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="f1e44-132">Eine andere Methode verwendet einen Inhaltstyp und eine Formatzeichenfolge:</span><span class="sxs-lookup"><span data-stu-id="f1e44-132">Another method takes a content type and format string:</span></span>

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="f1e44-133">Es gibt auch Erweiterungsmethoden für Umleitungen und erneutes Ausführen.</span><span class="sxs-lookup"><span data-stu-id="f1e44-133">There are also redirect and re-execute extension methods.</span></span> <span data-ttu-id="f1e44-134">Die Umleitungsmethode sendet den Statuscode 302 an den Client:</span><span class="sxs-lookup"><span data-stu-id="f1e44-134">The redirect method sends a 302 status code to the client:</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="f1e44-135">Die andere Methode gibt den ursprünglichen Statuscode an den Client zurück, führt jedoch auch den Handler für die Umleitungs-URL aus:</span><span class="sxs-lookup"><span data-stu-id="f1e44-135">The re-execute method returns the original status code to the client but also executes the handler for the redirect URL:</span></span>

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="f1e44-136">Statuscodeseiten können für bestimmte Anforderungen in der Handlermethode einer Razor-Seite oder in einem MVC-Controller deaktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="f1e44-136">Status code pages can be disabled for specific requests in a Razor Pages handler method or in an MVC controller.</span></span> <span data-ttu-id="f1e44-137">Versuchen Sie, [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) aus der Sammlung [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) der Anforderung abzurufen und das Feature zu deaktivieren (falls es verfügbar ist), um Statuscodeseiten zu deaktivieren:</span><span class="sxs-lookup"><span data-stu-id="f1e44-137">To disable status code pages, attempt to retrieve the [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) from the request's [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) collection and disable the feature if it's available:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="f1e44-138">Ausnahmebehandlungscode</span><span class="sxs-lookup"><span data-stu-id="f1e44-138">Exception-handling code</span></span>

<span data-ttu-id="f1e44-139">Code auf Seiten zur Ausnahmebehandlung kann Ausnahmen auslösen.</span><span class="sxs-lookup"><span data-stu-id="f1e44-139">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="f1e44-140">Es ist häufig empfehlenswert, wenn Produktionsfehlerseiten nur statische Inhalte aufweisen.</span><span class="sxs-lookup"><span data-stu-id="f1e44-140">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="f1e44-141">Bedenken Sie zudem, dass Sie den Statuscode einer Antwort nicht ändern können, sobald Sie die Header für diese Antwort gesendet haben, und dass weder Ausnahmeseiten noch Handler ausgeführt werden können.</span><span class="sxs-lookup"><span data-stu-id="f1e44-141">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="f1e44-142">Die Antwort muss abgeschlossen oder die Verbindung unterbrochen werden.</span><span class="sxs-lookup"><span data-stu-id="f1e44-142">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="f1e44-143">Sichere Ausnahmebehandlung</span><span class="sxs-lookup"><span data-stu-id="f1e44-143">Server exception handling</span></span>

<span data-ttu-id="f1e44-144">Neben der Ausnahmebehandlungslogik in Ihrer App werden auf dem [Server](xref:fundamentals/servers/index), der Ihre App hostet, einige Ausnahmebehandlungen durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="f1e44-144">In addition to the exception handling logic in your app, the [server](xref:fundamentals/servers/index) hosting your app performs some exception handling.</span></span> <span data-ttu-id="f1e44-145">Wenn der Server eine Ausnahme erfasst, bevor die Header gesendet werden, sendet der Server die Antwort *500 – Interner Serverfehler* ohne Text.</span><span class="sxs-lookup"><span data-stu-id="f1e44-145">If the server catches an exception before the headers are sent, the server sends a *500 Internal Server Error* response with no body.</span></span> <span data-ttu-id="f1e44-146">Wenn der Server eine Ausnahme auffängt, nachdem die Header gesendet wurden, schließt der Server die Verbindung.</span><span class="sxs-lookup"><span data-stu-id="f1e44-146">If the server catches an exception after the headers have been sent, the server closes the connection.</span></span> <span data-ttu-id="f1e44-147">Anforderungen, die nicht von Ihrer App verarbeitet werden, werden vom Server verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="f1e44-147">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="f1e44-148">Jede auftretende Ausnahme wird durch die Ausnahmebehandlung des Servers behandelt.</span><span class="sxs-lookup"><span data-stu-id="f1e44-148">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="f1e44-149">Konfigurierte benutzerdefinierte Fehlerseiten, Middleware oder Filter zur Fehlerbehandlung haben keine Auswirkungen auf dieses Verhalten.</span><span class="sxs-lookup"><span data-stu-id="f1e44-149">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="f1e44-150">Fehlerbehandlung während des Starts</span><span class="sxs-lookup"><span data-stu-id="f1e44-150">Startup exception handling</span></span>

<span data-ttu-id="f1e44-151">Nur auf Hostebene können Ausnahmen behandelt werden, die während des Starts einer App auftreten.</span><span class="sxs-lookup"><span data-stu-id="f1e44-151">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="f1e44-152">Sie können mithilfe des [Webhosts](xref:fundamentals/host/web-host) [konfigurieren, wie der Host während des Starts auf Fehler reagiert](xref:fundamentals/host/web-host#detailed-errors), indem Sie die Schlüssel `captureStartupErrors` und `detailedErrors` verwenden.</span><span class="sxs-lookup"><span data-stu-id="f1e44-152">Using the [Web Host](xref:fundamentals/host/web-host), you can [configure how the host behaves in response to errors during startup](xref:fundamentals/host/web-host#detailed-errors) with the `captureStartupErrors` and `detailedErrors` keys.</span></span>

<span data-ttu-id="f1e44-153">Das Hosting kann nur dann eine Fehlerseite für einen erfassten Fehler beim Start anzeigen, wenn der Fehler nach der Bindung der Hostadresse bzw. des Ports auftritt.</span><span class="sxs-lookup"><span data-stu-id="f1e44-153">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="f1e44-154">Wenn eine Bindung aus irgendeinem Grund fehlschlägt, wird auf der Hostebene eine kritische Ausnahme protokolliert, der .NET-Prozess stürzt ab, und es wird keine Fehlerseite angezeigt, wenn die App auf dem [Kestrel](xref:fundamentals/servers/kestrel)-Server ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="f1e44-154">If any binding fails for any reason, the hosting layer logs a critical exception, the dotnet process crashes, and no error page is displayed when the app is running on the [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

<span data-ttu-id="f1e44-155">Wenn sie auf [IIS](/iis) oder [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ausgeführt wird, wird ein *Prozessfehler 502.5* vom [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) zurückgegeben, wenn der Prozess nicht gestartet werden kann.</span><span class="sxs-lookup"><span data-stu-id="f1e44-155">When running on [IIS](/iis) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 Process Failure* is returned by the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) if the process can't be started.</span></span> <span data-ttu-id="f1e44-156">Befolgen Sie die Hinweise zur Fehlerbehebung im Thema [Troubleshoot ASP.NET Core on IIS (Problembehandlung bei ASP.NET Core in IIS)](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="f1e44-156">Follow the troubleshooting advice in the [Troubleshoot ASP.NET Core on IIS](xref:host-and-deploy/iis/troubleshoot) topic.</span></span>

## <a name="aspnet-mvc-error-handling"></a><span data-ttu-id="f1e44-157">ASP.NET MVC-Fehlerbehandlung</span><span class="sxs-lookup"><span data-stu-id="f1e44-157">ASP.NET MVC error handling</span></span>

<span data-ttu-id="f1e44-158">[MVC](xref:mvc/overview)-Apps verfügen über einige zusätzliche Optionen für die Fehlerbehandlung, z.B. das Konfigurieren von Ausnahmefiltern und das Durchführen einer Modellvalidierung.</span><span class="sxs-lookup"><span data-stu-id="f1e44-158">[MVC](xref:mvc/overview) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="f1e44-159">Ausnahmefilter</span><span class="sxs-lookup"><span data-stu-id="f1e44-159">Exception Filters</span></span>

<span data-ttu-id="f1e44-160">Ausnahmefilter können in einer MVC-App global oder auf der Grundlage eines Controllers oder einer Aktion konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="f1e44-160">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="f1e44-161">Diese Filter verarbeiten jede nicht behandelte Ausnahme, die während der Ausführung eine Controlleraktion oder eines anderen Filters auftritt, und werden andernfalls nicht aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="f1e44-161">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter, and are not called otherwise.</span></span> <span data-ttu-id="f1e44-162">Weitere Informationen zu Ausnahmefiltern finden Sie unter [Filter](xref:mvc/controllers/filters).</span><span class="sxs-lookup"><span data-stu-id="f1e44-162">Learn more about exception filters in [Filters](xref:mvc/controllers/filters).</span></span>

>[!TIP]
> <span data-ttu-id="f1e44-163">Ausnahmefilter eignen sich zum Auffangen von Ausnahmen, die in MVC-Aktionen auftreten. Sie sind jedoch nicht so flexibel wie Middleware für die Fehlerbehandlung.</span><span class="sxs-lookup"><span data-stu-id="f1e44-163">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="f1e44-164">Es wird empfohlen, dass Sie Middleware für allgemeine Fälle vorziehen und Filter nur dann verwenden, wenn Sie die Fehlerbehandlung *auf einem anderen Weg* angehen müssen, je nachdem, welche MVC-Aktion gewählt wurde.</span><span class="sxs-lookup"><span data-stu-id="f1e44-164">Prefer middleware for the general case, and use filters only where you need to do error handling *differently* based on which MVC action was chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="f1e44-165">Behandeln von Modellstatusfehlern</span><span class="sxs-lookup"><span data-stu-id="f1e44-165">Handling Model State Errors</span></span>

<span data-ttu-id="f1e44-166">Die [Modellvalidierung](xref:mvc/models/validation) tritt vor jeder ausgelösten Controlleraktion auf. Die Aktionsmethode ist dafür verantwortlich, `ModelState.IsValid` zu untersuchen und angemessen zu reagieren.</span><span class="sxs-lookup"><span data-stu-id="f1e44-166">[Model validation](xref:mvc/models/validation) occurs prior to invoking each controller action, and it's the action method's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="f1e44-167">Einige Apps wählen eine Standardkonvention aus, um Modellvalidierungsfehler zu behandeln. Dann kann ein [Filter](xref:mvc/controllers/filters) hilfreich sein, um eine solche Richtlinie zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="f1e44-167">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a [filter](xref:mvc/controllers/filters) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="f1e44-168">Sie sollten testen, wie sich Ihre Aktionen mit ungültigen Modellstatus verhalten.</span><span class="sxs-lookup"><span data-stu-id="f1e44-168">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="f1e44-169">Weitere Informationen finden Sie unter [Testen von Controllerlogik](xref:mvc/controllers/testing).</span><span class="sxs-lookup"><span data-stu-id="f1e44-169">Learn more in [Test controller logic](xref:mvc/controllers/testing).</span></span>



