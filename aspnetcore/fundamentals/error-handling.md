---
title: Fehlerbehandlung in ASP.NET Core
author: ardalis
description: Erfahren Sie mehr über die Fehlerbehandlung in ASP.NET Core-Apps.
ms.author: tdykstra
ms.custom: mvc
ms.date: 07/05/2018
uid: fundamentals/error-handling
ms.openlocfilehash: 7ea944bc423001aa47ce684443b96104cf9174bf
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312246"
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="87665-103">Fehlerbehandlung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="87665-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="87665-104">Von [Steve Smith](https://ardalis.com/) und [Tom Dykstra](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="87665-104">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="87665-105">Dieser Artikel beschreibt grundsätzliche Vorgehensweisen zur Behandlung von Fehlern in ASP.NET Core-Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="87665-105">This article covers common approaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="87665-106">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/ErrorHandlingSample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="87665-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/ErrorHandlingSample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-developer-exception-page"></a><span data-ttu-id="87665-107">Die Seite mit Ausnahmen für Entwickler</span><span class="sxs-lookup"><span data-stu-id="87665-107">The Developer Exception Page</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="87665-108">Verwenden Sie die *Seite mit Ausnahmen für Entwickler*, um eine App so zu konfigurieren, dass sie eine Seite anzeigt, die ausführliche Informationen zu Ausnahmen enthält.</span><span class="sxs-lookup"><span data-stu-id="87665-108">To configure an app to display a page that shows detailed information about exceptions, use the *Developer Exception Page*.</span></span> <span data-ttu-id="87665-109">Die Seite wird vom [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/)-Paket zur Verfügung gestellt. Dieses ist im [Microsoft.AspNetCore.App-Metapaket](xref:fundamentals/metapackage-app) enthalten.</span><span class="sxs-lookup"><span data-stu-id="87665-109">The page is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="87665-110">Fügen Sie der `Startup.Configure`-Methode eine Zeile hinzu:</span><span class="sxs-lookup"><span data-stu-id="87665-110">Add a line to the `Startup.Configure` method:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="87665-111">Verwenden Sie die *Seite mit Ausnahmen für Entwickler*, um eine App so zu konfigurieren, dass sie eine Seite anzeigt, die ausführliche Informationen zu Ausnahmen enthält.</span><span class="sxs-lookup"><span data-stu-id="87665-111">To configure an app to display a page that shows detailed information about exceptions, use the *Developer Exception Page*.</span></span> <span data-ttu-id="87665-112">Die Seite wird vom [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/)-Paket zur Verfügung gestellt. Dieses ist im [Microsoft.AspNetCore.All-Metapaket](xref:fundamentals/metapackage) enthalten.</span><span class="sxs-lookup"><span data-stu-id="87665-112">The page is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span> <span data-ttu-id="87665-113">Fügen Sie der `Startup.Configure`-Methode eine Zeile hinzu:</span><span class="sxs-lookup"><span data-stu-id="87665-113">Add a line to the `Startup.Configure` method:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="87665-114">Verwenden Sie die *Seite mit Ausnahmen für Entwickler*, um eine App so zu konfigurieren, dass sie eine Seite anzeigt, die ausführliche Informationen zu Ausnahmen enthält.</span><span class="sxs-lookup"><span data-stu-id="87665-114">To configure an app to display a page that shows detailed information about exceptions, use the *Developer Exception Page*.</span></span> <span data-ttu-id="87665-115">Die Seite wird durch Hinzufügen eines Paketverweises auf das [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/)-Paket in der Projektdatei verfügbar gemacht.</span><span class="sxs-lookup"><span data-stu-id="87665-115">The page is made available by adding a package reference for the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package in the project file.</span></span> <span data-ttu-id="87665-116">Fügen Sie der `Startup.Configure`-Methode eine Zeile hinzu:</span><span class="sxs-lookup"><span data-stu-id="87665-116">Add a line to the `Startup.Configure` method:</span></span>

::: moniker-end

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

<span data-ttu-id="87665-117">Fügen Sie den Aufruf von [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) vor Middleware ein, vor der Sie Ausnahmen abfangen möchten, z.B. `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="87665-117">Place the call to [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) in front of any middleware where you want to catch exceptions, such as `app.UseMvc`.</span></span>

> [!WARNING]
> <span data-ttu-id="87665-118">Aktivieren Sie die Seite mit Ausnahmen für Entwickler **nur dann, wenn die App in der Entwicklungsumgebung ausgeführt wird**.</span><span class="sxs-lookup"><span data-stu-id="87665-118">Enable the Developer Exception Page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="87665-119">Wenn die App in der Produktionsumgebung ausgeführt wird, sollten Sie keine detaillierten Ausnahmeinformationen öffentlich teilen.</span><span class="sxs-lookup"><span data-stu-id="87665-119">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="87665-120">[Weitere Informationen zum Konfigurieren von Umgebungen](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="87665-120">[Learn more about configuring environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="87665-121">Wenn Sie die Seite mit Ausnahmen für Entwickler anzeigen möchten, führen Sie die Beispiel-App mit der auf `Development` festgelegten Umgebung aus, und fügen Sie `?throw=true` zur Basis-URL der App hinzu.</span><span class="sxs-lookup"><span data-stu-id="87665-121">To see the Developer Exception Page, run the sample app with the environment set to `Development` and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="87665-122">Die Seite enthält mehrere Registerkarten mit Informationen zu der Ausnahme und der Anforderung.</span><span class="sxs-lookup"><span data-stu-id="87665-122">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="87665-123">Die erste Registerkarte enthält eine Stapelüberwachung:</span><span class="sxs-lookup"><span data-stu-id="87665-123">The first tab includes a stack trace:</span></span>

![Stapelüberwachung](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="87665-125">Auf der nächsten Registerkarte werden, sofern vorhanden, Abfragezeichenfolgeparameter angezeigt:</span><span class="sxs-lookup"><span data-stu-id="87665-125">The next tab shows the query string parameters, if any:</span></span>

![Abfragezeichenfolgeparameter](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="87665-127">Wenn die Anforderung Cookies besitzt, werden diese auf der Registerkarte **Cookies** angezeigt. Header werden auf der letzten Registerkarte angezeigt:</span><span class="sxs-lookup"><span data-stu-id="87665-127">If the request has cookies, they appear on the **Cookies** tab. Headers are seen in the last tab:</span></span>

![Header](error-handling/_static/developer-exception-page-headers.png)

## <a name="configure-a-custom-exception-handling-page"></a><span data-ttu-id="87665-129">Konfigurieren einer benutzerdefinierten Seite zur Ausnahmebehandlung</span><span class="sxs-lookup"><span data-stu-id="87665-129">Configure a custom exception handling page</span></span>

<span data-ttu-id="87665-130">Konfigurieren Sie eine Seite zur Ausnahmebehandlung, die verwendet werden kann, wenn die App nicht in der `Development`-Umgebung ausgeführt wird:</span><span class="sxs-lookup"><span data-stu-id="87665-130">Configure an exception handler page to use when the app isn't running in the `Development` environment:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

<span data-ttu-id="87665-131">In einer Razor Pages-App enthält die Vorlage [dotnet new](/dotnet/core/tools/dotnet-new) von Razor Pages im Ordner *Pages* eine Fehlerseite und eine `ErrorModel`-Seitenmodellklasse.</span><span class="sxs-lookup"><span data-stu-id="87665-131">In a Razor Pages app, the [dotnet new](/dotnet/core/tools/dotnet-new) Razor Pages template provides an Error page and `ErrorModel` page model class in the *Pages* folder.</span></span>

<span data-ttu-id="87665-132">Die Aktionsmethode zur Fehlerbehandlung wird in einer MVC-App nicht durch HTTP-Methodenattribute wie `HttpGet` ergänzt.</span><span class="sxs-lookup"><span data-stu-id="87665-132">In an MVC app, don't decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="87665-133">Durch explizite Verben könnte bei einigen Anforderungen verhindert werden, dass diese Methode zum Einsatz kommt.</span><span class="sxs-lookup"><span data-stu-id="87665-133">Explicit verbs prevent some requests from reaching the method.</span></span> <span data-ttu-id="87665-134">Lassen Sie den anonymen Zugriff auf die Methode zu, damit nicht authentifizierte Benutzer die Fehleransicht empfangen können.</span><span class="sxs-lookup"><span data-stu-id="87665-134">Allow anonymous access to the method so that unauthenticated users are able to receive the error view.</span></span>

<span data-ttu-id="87665-135">Folgende Fehlerhandlermethode wird beispielsweise in der MVC-Vorlage [dotnet new](/dotnet/core/tools/dotnet-new) bereitgestellt und im Home-Controller angezeigt:</span><span class="sxs-lookup"><span data-stu-id="87665-135">For example, the following error handler method is provided by the [dotnet new](/dotnet/core/tools/dotnet-new) MVC template and appears in the Home controller:</span></span>

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

## <a name="configure-status-code-pages"></a><span data-ttu-id="87665-136">Konfigurieren von Statuscodeseiten</span><span class="sxs-lookup"><span data-stu-id="87665-136">Configure status code pages</span></span>

<span data-ttu-id="87665-137">Ihre App stellt für HTTP-Statuscodes wie *404 – Nicht gefunden* standardmäßig keine ausführliche Statuscodeseite zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="87665-137">By default, an app doesn't provide a rich status code page for HTTP status codes, such as *404 Not Found*.</span></span> <span data-ttu-id="87665-138">Verwenden Sie zum Bereitstellen von Statuscodeseiten Middleware für Statuscodeseiten.</span><span class="sxs-lookup"><span data-stu-id="87665-138">To provide status code pages, use Status Code Pages Middleware.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="87665-139">Die Middleware wird vom [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/)-Paket zur Verfügung gestellt. Dieses ist im [Microsoft.AspNetCore.App-Metapaket](xref:fundamentals/metapackage-app) enthalten.</span><span class="sxs-lookup"><span data-stu-id="87665-139">The middleware is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="87665-140">Die Middleware wird vom [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/)-Paket zur Verfügung gestellt. Dieses ist im [Microsoft.AspNetCore.All-Metapaket](xref:fundamentals/metapackage) enthalten.</span><span class="sxs-lookup"><span data-stu-id="87665-140">The middleware is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="87665-141">Die Middleware wird durch Hinzufügen eines Paketverweises auf das [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/)-Paket in der Projektdatei verfügbar gemacht.</span><span class="sxs-lookup"><span data-stu-id="87665-141">The middleware is made available by adding a package reference for the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package in the project file.</span></span>

::: moniker-end

<span data-ttu-id="87665-142">Fügen Sie der `Startup.Configure`-Methode eine Zeile hinzu:</span><span class="sxs-lookup"><span data-stu-id="87665-142">Add a line to the `Startup.Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="87665-143"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> sollte vor Middleware für die Anforderungsverarbeitung in der Pipeline aufgerufen werden (z.B. Middleware für statische Dateien und MVC-Middleware).</span><span class="sxs-lookup"><span data-stu-id="87665-143"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> should be called before request handling middlewares in the pipeline (for example, Static Files Middleware and MVC Middleware).</span></span>

<span data-ttu-id="87665-144">Standardmäßig fügt die Middleware „Satus Code Pages“ Handler im Textformat für gängige Statuscodes wie 404 hinzu:</span><span class="sxs-lookup"><span data-stu-id="87665-144">By default, Status Code Pages Middleware adds text-only handlers for common status codes, such as 404:</span></span>

![404-Seite](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="87665-146">Die Middleware unterstützt zahlreiche Erweiterungsmethoden.</span><span class="sxs-lookup"><span data-stu-id="87665-146">The middleware supports several extension methods.</span></span> <span data-ttu-id="87665-147">Eine Methode verwendet einen Lambdaausdruck:</span><span class="sxs-lookup"><span data-stu-id="87665-147">One method takes a lambda expression:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="87665-148">Eine andere Methode verwendet einen Inhaltstyp und eine Formatzeichenfolge:</span><span class="sxs-lookup"><span data-stu-id="87665-148">Another method takes a content type and format string:</span></span>

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="87665-149">Es gibt auch Erweiterungsmethoden für Umleitungen und erneutes Ausführen.</span><span class="sxs-lookup"><span data-stu-id="87665-149">There are also redirect and re-execute extension methods.</span></span> <span data-ttu-id="87665-150">Die Umleitungsmethode sendet den Statuscode *302 Found* (Gefunden) an den Client und leitet diesen an die bereitgestellte Vorlage für die Speicherort-URL weiter.</span><span class="sxs-lookup"><span data-stu-id="87665-150">The redirect method sends a *302 Found* status code to the client and redirects the client to the provided location URL template.</span></span> <span data-ttu-id="87665-151">Die Vorlage kann einen `{0}`-Platzhalter für den Statuscode enthalten.</span><span class="sxs-lookup"><span data-stu-id="87665-151">The template may include a `{0}` placeholder for the status code.</span></span> <span data-ttu-id="87665-152">URLs, die mit `~` beginnen, wird der Basispfad vorangestellt.</span><span class="sxs-lookup"><span data-stu-id="87665-152">URLs starting with `~` have the base path prepended.</span></span> <span data-ttu-id="87665-153">URLs, die nicht mit `~` beginnen, werden ohne Änderungen verwendet.</span><span class="sxs-lookup"><span data-stu-id="87665-153">A URL that doesn't start with `~` is used as is.</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="87665-154">Die Methode für die erneute Ausführung gibt den ursprünglichen Statuscode an den Client zurück und legt fest, dass der Antworttext durch die erneute Ausführung der Anforderungspipeline mithilfe eines alternativen Pfads erstellt werden soll.</span><span class="sxs-lookup"><span data-stu-id="87665-154">The re-execute method returns the original status code to the client and specifies that the response body should be generated by re-executing the request pipeline using an alternate path.</span></span> <span data-ttu-id="87665-155">Dieser Pfad kann einen `{0}`-Platzhalter für den Statuscode enthalten:</span><span class="sxs-lookup"><span data-stu-id="87665-155">This path may contain a `{0}` placeholder for the status code:</span></span>

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="87665-156">Statuscodeseiten können für bestimmte Anforderungen in der Handlermethode einer Razor-Seite oder in einem MVC-Controller deaktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="87665-156">Status code pages can be disabled for specific requests in a Razor Pages handler method or in an MVC controller.</span></span> <span data-ttu-id="87665-157">Versuchen Sie, [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) aus der Sammlung [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) der Anforderung abzurufen und das Feature zu deaktivieren (falls es verfügbar ist), um Statuscodeseiten zu deaktivieren:</span><span class="sxs-lookup"><span data-stu-id="87665-157">To disable status code pages, attempt to retrieve the [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) from the request's [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) collection and disable the feature if it's available:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

<span data-ttu-id="87665-158">Bei Verwendung einer `UseStatusCodePages*`-Überladung, die auf einen Endpunkt in der App verweist, müssen Sie eine MVC-Ansicht oder Razor Page für den Endpunkt erstellen.</span><span class="sxs-lookup"><span data-stu-id="87665-158">If using a `UseStatusCodePages*` overload that points to an endpoint within the app, create an MVC view or Razor Page for the endpoint.</span></span> <span data-ttu-id="87665-159">Mit der Vorlage [dotnet new](/dotnet/core/tools/dotnet-new) für eine Razor Pages-App werden beispielsweise die folgende Seite und die folgende Seitenmodellklasse erstellt:</span><span class="sxs-lookup"><span data-stu-id="87665-159">For example, the [dotnet new](/dotnet/core/tools/dotnet-new) template for a Razor Pages app produces the following page and page model class:</span></span>

<span data-ttu-id="87665-160">*Error.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="87665-160">*Error.cshtml*:</span></span>

```cshtml
@page
@model ErrorModel
@{
    ViewData["Title"] = "Error";
}

<h1 class="text-danger">Error.</h1>
<h2 class="text-danger">An error occurred while processing your request.</h2>

@if (Model.ShowRequestId)
{
    <p>
        <strong>Request ID:</strong> <code>@Model.RequestId</code>
    </p>
}

<h3>Development Mode</h3>
<p>
    Swapping to <strong>Development</strong> environment will display more detailed 
    information about the error that occurred.
</p>
<p>
    <strong>Development environment should not be enabled in deployed applications
    </strong>, as it can result in sensitive information from exceptions being 
    displayed to end users. For local debugging, development environment can be 
    enabled by setting the <strong>ASPNETCORE_ENVIRONMENT</strong> environment 
    variable to <strong>Development</strong>, and restarting the application.
</p>
```

<span data-ttu-id="87665-161">*Error.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="87665-161">*Error.cshtml.cs*:</span></span>

```csharp
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, 
        NoStore = true)]
    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="87665-162">Ausnahmebehandlungscode</span><span class="sxs-lookup"><span data-stu-id="87665-162">Exception-handling code</span></span>

<span data-ttu-id="87665-163">Code auf Seiten zur Ausnahmebehandlung kann Ausnahmen auslösen.</span><span class="sxs-lookup"><span data-stu-id="87665-163">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="87665-164">Es ist häufig empfehlenswert, wenn Produktionsfehlerseiten nur statische Inhalte aufweisen.</span><span class="sxs-lookup"><span data-stu-id="87665-164">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="87665-165">Bedenken Sie zudem, dass Sie den Statuscode einer Antwort nicht ändern können, sobald Sie die Header für diese Antwort gesendet haben, und dass weder Ausnahmeseiten noch Handler ausgeführt werden können.</span><span class="sxs-lookup"><span data-stu-id="87665-165">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="87665-166">Die Antwort muss abgeschlossen oder die Verbindung unterbrochen werden.</span><span class="sxs-lookup"><span data-stu-id="87665-166">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="87665-167">Sichere Ausnahmebehandlung</span><span class="sxs-lookup"><span data-stu-id="87665-167">Server exception handling</span></span>

<span data-ttu-id="87665-168">Neben der Ausnahmebehandlungslogik in Ihrer App werden auf dem [Server](xref:fundamentals/servers/index), der Ihre App hostet, einige Ausnahmebehandlungen durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="87665-168">In addition to the exception handling logic in your app, the [server](xref:fundamentals/servers/index) hosting your app performs some exception handling.</span></span> <span data-ttu-id="87665-169">Wenn der Server eine Ausnahme erfasst, bevor die Header gesendet werden, sendet der Server die Antwort *500 – Interner Serverfehler* ohne Text.</span><span class="sxs-lookup"><span data-stu-id="87665-169">If the server catches an exception before the headers are sent, the server sends a *500 Internal Server Error* response with no body.</span></span> <span data-ttu-id="87665-170">Wenn der Server eine Ausnahme auffängt, nachdem die Header gesendet wurden, schließt der Server die Verbindung.</span><span class="sxs-lookup"><span data-stu-id="87665-170">If the server catches an exception after the headers have been sent, the server closes the connection.</span></span> <span data-ttu-id="87665-171">Anforderungen, die nicht von Ihrer App verarbeitet werden, werden vom Server verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="87665-171">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="87665-172">Jede auftretende Ausnahme wird durch die Ausnahmebehandlung des Servers behandelt.</span><span class="sxs-lookup"><span data-stu-id="87665-172">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="87665-173">Konfigurierte benutzerdefinierte Fehlerseiten, Middleware oder Filter zur Fehlerbehandlung haben keine Auswirkungen auf dieses Verhalten.</span><span class="sxs-lookup"><span data-stu-id="87665-173">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="87665-174">Fehlerbehandlung während des Starts</span><span class="sxs-lookup"><span data-stu-id="87665-174">Startup exception handling</span></span>

<span data-ttu-id="87665-175">Nur auf Hostebene können Ausnahmen behandelt werden, die während des Starts einer App auftreten.</span><span class="sxs-lookup"><span data-stu-id="87665-175">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="87665-176">Sie können mithilfe des [Webhosts](xref:fundamentals/host/web-host) [konfigurieren, wie der Host während des Starts auf Fehler reagiert](xref:fundamentals/host/web-host#detailed-errors), indem Sie die Schlüssel `captureStartupErrors` und `detailedErrors` verwenden.</span><span class="sxs-lookup"><span data-stu-id="87665-176">Using the [Web Host](xref:fundamentals/host/web-host), you can [configure how the host behaves in response to errors during startup](xref:fundamentals/host/web-host#detailed-errors) with the `captureStartupErrors` and `detailedErrors` keys.</span></span>

<span data-ttu-id="87665-177">Das Hosting kann nur dann eine Fehlerseite für einen erfassten Fehler beim Start anzeigen, wenn der Fehler nach der Bindung der Hostadresse bzw. des Ports auftritt.</span><span class="sxs-lookup"><span data-stu-id="87665-177">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="87665-178">Wenn eine Bindung aus irgendeinem Grund fehlschlägt, wird auf der Hostebene eine kritische Ausnahme protokolliert, der .NET-Prozess stürzt ab, und es wird keine Fehlerseite angezeigt, wenn die App auf dem [Kestrel](xref:fundamentals/servers/kestrel)-Server ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="87665-178">If any binding fails for any reason, the hosting layer logs a critical exception, the dotnet process crashes, and no error page is displayed when the app is running on the [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

<span data-ttu-id="87665-179">Wenn sie auf [IIS](/iis) oder [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ausgeführt wird, wird ein *Prozessfehler 502.5* vom [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) zurückgegeben, wenn der Prozess nicht gestartet werden kann.</span><span class="sxs-lookup"><span data-stu-id="87665-179">When running on [IIS](/iis) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 Process Failure* is returned by the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) if the process can't be started.</span></span> <span data-ttu-id="87665-180">Weitere Informationen zum Beheben von Startproblemen beim Hosten mit den IIS finden Sie unter <xref:host-and-deploy/iis/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="87665-180">For information on troubleshooting startup issues when hosting with IIS, see <xref:host-and-deploy/iis/troubleshoot>.</span></span> <span data-ttu-id="87665-181">Weitere Informationen zum Beheben von Startproblemen mit Azure App Service finden Sie unter <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="87665-181">For information on troubleshooting startup issues with Azure App Service, see <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>

## <a name="aspnet-core-mvc-error-handling"></a><span data-ttu-id="87665-182">ASP.NET Core MVC-Fehlerbehandlung</span><span class="sxs-lookup"><span data-stu-id="87665-182">ASP.NET Core MVC error handling</span></span>

<span data-ttu-id="87665-183">[MVC](xref:mvc/overview)-Apps verfügen über einige zusätzliche Optionen für die Fehlerbehandlung, z.B. das Konfigurieren von Ausnahmefiltern und das Durchführen einer Modellvalidierung.</span><span class="sxs-lookup"><span data-stu-id="87665-183">[MVC](xref:mvc/overview) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="87665-184">Ausnahmefilter</span><span class="sxs-lookup"><span data-stu-id="87665-184">Exception filters</span></span>

<span data-ttu-id="87665-185">Ausnahmefilter können in einer MVC-App global oder auf der Grundlage eines Controllers oder einer Aktion konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="87665-185">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="87665-186">Diese Filter verarbeiten jede nicht behandelte Ausnahme, die während der Ausführung eine Controlleraktion oder eines anderen Filters auftritt.</span><span class="sxs-lookup"><span data-stu-id="87665-186">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter.</span></span> <span data-ttu-id="87665-187">Diese Dateien werden andernfalls nicht aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="87665-187">These filters aren't called otherwise.</span></span> <span data-ttu-id="87665-188">Weitere Informationen dazu finden Sie unter [Filter](xref:mvc/controllers/filters).</span><span class="sxs-lookup"><span data-stu-id="87665-188">To learn more, see [Filters](xref:mvc/controllers/filters).</span></span>

> [!TIP]
> <span data-ttu-id="87665-189">Ausnahmefilter eignen sich zum Auffangen von Ausnahmen, die in MVC-Aktionen auftreten. Sie sind jedoch nicht so flexibel wie Middleware für die Fehlerbehandlung.</span><span class="sxs-lookup"><span data-stu-id="87665-189">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="87665-190">Ziehen Sie im Allgemeinen die Verwendung der Middleware für allgemeine Fälle vor, und verwenden Sie Filter nur dann, wenn Sie die Fehlerbehandlung *auf einem anderen Weg* angehen müssen, je nachdem, welche MVC-Aktion gewählt wurde.</span><span class="sxs-lookup"><span data-stu-id="87665-190">Generally prefer the use of middleware, and use filters only where you need to perform error handling *differently* based on which MVC action is chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="87665-191">Behandeln von Modellstatusfehlern</span><span class="sxs-lookup"><span data-stu-id="87665-191">Handling model state errors</span></span>

<span data-ttu-id="87665-192">Die [Modellvalidierung](xref:mvc/models/validation) tritt vor jeder ausgelösten Controlleraktion auf. Die Aktionsmethode ist dafür verantwortlich, `ModelState.IsValid` zu untersuchen und angemessen zu reagieren.</span><span class="sxs-lookup"><span data-stu-id="87665-192">[Model validation](xref:mvc/models/validation) occurs prior to invoking each controller action, and it's the action method's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="87665-193">Einige Apps wählen eine Standardkonvention aus, um Modellvalidierungsfehler zu behandeln. Dann kann ein [Filter](xref:mvc/controllers/filters) hilfreich sein, um eine solche Richtlinie zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="87665-193">Some apps choose to follow a standard convention for dealing with model validation errors, in which case a [filter](xref:mvc/controllers/filters) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="87665-194">Sie sollten testen, wie sich Ihre Aktionen mit ungültigen Modellstatus verhalten.</span><span class="sxs-lookup"><span data-stu-id="87665-194">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="87665-195">Weitere Informationen finden Sie unter [Testen von Controllerlogik](xref:mvc/controllers/testing).</span><span class="sxs-lookup"><span data-stu-id="87665-195">Learn more in [Test controller logic](xref:mvc/controllers/testing).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="87665-196">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="87665-196">Additional resources</span></span>

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
