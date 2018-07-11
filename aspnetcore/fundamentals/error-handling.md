---
title: Fehlerbehandlung in ASP.NET Core
author: ardalis
description: Erfahren Sie mehr über die Fehlerbehandlung in ASP.NET Core-Apps.
ms.author: tdykstra
ms.custom: mvc
ms.date: 07/05/2018
uid: fundamentals/error-handling
ms.openlocfilehash: 126a782bfd32f9ecd0596045218371ef5ccc82f2
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894139"
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="9e1d8-103">Fehlerbehandlung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9e1d8-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="9e1d8-104">Von [Steve Smith](https://ardalis.com/) und [Tom Dykstra](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="9e1d8-104">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="9e1d8-105">Dieser Artikel beschreibt grundsätzliche Vorgehensweisen zur Behandlung von Fehlern in ASP.NET Core-Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-105">This article covers common approaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="9e1d8-106">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/ErrorHandlingSample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9e1d8-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/ErrorHandlingSample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-developer-exception-page"></a><span data-ttu-id="9e1d8-107">Die Seite mit Ausnahmen für Entwickler</span><span class="sxs-lookup"><span data-stu-id="9e1d8-107">The developer exception page</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="9e1d8-108">Verwenden Sie die *Seite mit Ausnahmen für Entwickler*, um eine App so zu konfigurieren, dass sie eine Seite anzeigt, die ausführliche Informationen zu Ausnahmen enthält.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-108">To configure an app to display a page that shows detailed information about exceptions, use the *developer exception page*.</span></span> <span data-ttu-id="9e1d8-109">Die Seite wird vom [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/)-Paket zur Verfügung gestellt. Dieses ist im [Microsoft.AspNetCore.App-Metapaket](xref:fundamentals/metapackage-app) enthalten.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-109">The page made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="9e1d8-110">Fügen Sie der `Startup.Configure`-Methode eine Zeile hinzu:</span><span class="sxs-lookup"><span data-stu-id="9e1d8-110">Add a line to the `Startup.Configure` method:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="9e1d8-111">Verwenden Sie die *Seite mit Ausnahmen für Entwickler*, um eine App so zu konfigurieren, dass sie eine Seite anzeigt, die ausführliche Informationen zu Ausnahmen enthält.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-111">To configure an app to display a page that shows detailed information about exceptions, use the *developer exception page*.</span></span> <span data-ttu-id="9e1d8-112">Die Seite wird vom [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/)-Paket zur Verfügung gestellt. Dieses ist im [Microsoft.AspNetCore.All-Metapaket](xref:fundamentals/metapackage) enthalten.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-112">The page is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span> <span data-ttu-id="9e1d8-113">Fügen Sie der `Startup.Configure`-Methode eine Zeile hinzu:</span><span class="sxs-lookup"><span data-stu-id="9e1d8-113">Add a line to the `Startup.Configure` method:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="9e1d8-114">Verwenden Sie die *Seite mit Ausnahmen für Entwickler*, um eine App so zu konfigurieren, dass sie eine Seite anzeigt, die ausführliche Informationen zu Ausnahmen enthält.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-114">To configure an app to display a page that shows detailed information about exceptions, use the *developer exception page*.</span></span> <span data-ttu-id="9e1d8-115">Die Seite wird durch Hinzufügen eines Paketverweises auf das [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/)-Paket in der Projektdatei verfügbar gemacht.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-115">The page is made available by adding a package reference for the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package in the project file.</span></span> <span data-ttu-id="9e1d8-116">Fügen Sie der `Startup.Configure`-Methode eine Zeile hinzu:</span><span class="sxs-lookup"><span data-stu-id="9e1d8-116">Add a line to the `Startup.Configure` method:</span></span>

::: moniker-end

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

<span data-ttu-id="9e1d8-117">Fügen Sie den Aufruf von [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) vor Middleware ein, vor der Sie Ausnahmen abfangen möchten, z.B. `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-117">Place the call to [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) in front of any middleware where you want to catch exceptions, such as `app.UseMvc`.</span></span>

>[!WARNING]
> <span data-ttu-id="9e1d8-118">Aktivieren Sie die Seite mit Ausnahmen für Entwickler **nur, wenn die App in der Entwicklungsumgebung ausgeführt wird**.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-118">Enable the developer exception page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="9e1d8-119">Wenn die App in der Produktionsumgebung ausgeführt wird, sollten Sie keine detaillierten Ausnahmeinformationen öffentlich teilen.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-119">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="9e1d8-120">[Weitere Informationen zum Konfigurieren von Umgebungen](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="9e1d8-120">[Learn more about configuring environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="9e1d8-121">Wenn Sie die Seite mit Ausnahmen für Entwickler anzeigen möchten, führen Sie die Beispiel-App mit der auf `Development` festgelegten Umgebung aus, und fügen Sie `?throw=true` zur Basis-URL der App hinzu.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-121">To see the developer exception page, run the sample app with the environment set to `Development`, and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="9e1d8-122">Die Seite enthält mehrere Registerkarten mit Informationen zu der Ausnahme und der Anforderung.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-122">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="9e1d8-123">Die erste Registerkarte enthält eine Stapelüberwachung:</span><span class="sxs-lookup"><span data-stu-id="9e1d8-123">The first tab includes a stack trace:</span></span>

![Stapelüberwachung](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="9e1d8-125">Auf der nächsten Registerkarte werden, sofern vorhanden, Abfragezeichenfolgeparameter angezeigt:</span><span class="sxs-lookup"><span data-stu-id="9e1d8-125">The next tab shows the query string parameters, if any:</span></span>

![Abfragezeichenfolgeparameter](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="9e1d8-127">Wenn die Anforderung Cookies besitzt, werden diese auf der Registerkarte **Cookies** angezeigt. Header werden auf der letzten Registerkarte angezeigt:</span><span class="sxs-lookup"><span data-stu-id="9e1d8-127">If the request has cookies, they appear on the **Cookies** tab. Headers are seen in the last tab:</span></span>

![Header](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a><span data-ttu-id="9e1d8-129">Konfigurieren einer benutzerdefinierten Seite zur Ausnahmebehandlung</span><span class="sxs-lookup"><span data-stu-id="9e1d8-129">Configuring a custom exception handling page</span></span>

<span data-ttu-id="9e1d8-130">Konfigurieren Sie eine Seite zur Ausnahmebehandlung, die verwendet werden kann, wenn die App nicht in der `Development`-Umgebung ausgeführt wird:</span><span class="sxs-lookup"><span data-stu-id="9e1d8-130">Configure an exception handler page to use when the app isn't running in the `Development` environment:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

<span data-ttu-id="9e1d8-131">In einer Razor Pages-App enthält die Vorlage [dotnet new](/dotnet/core/tools/dotnet-new) von Razor Pages im Ordner *Pages* eine Fehlerseite und eine `ErrorModel`-Seitenmodellklasse.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-131">In a Razor Pages app, the [dotnet new](/dotnet/core/tools/dotnet-new) Razor Pages template provides an Error page and `ErrorModel` page model class in the *Pages* folder.</span></span>

<span data-ttu-id="9e1d8-132">Die Aktionsmethode zur Fehlerbehandlung wird in einer MVC-App nicht durch HTTP-Methodenattribute wie `HttpGet` ergänzt.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-132">In an MVC app, don't decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="9e1d8-133">Durch explizite Verben könnte bei einigen Anforderungen verhindert werden, dass diese Methode zum Einsatz kommt.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-133">Explicit verbs prevent some requests from reaching the method.</span></span> <span data-ttu-id="9e1d8-134">Lassen Sie den anonymen Zugriff auf die Methode zu, damit nicht authentifizierte Benutzer die Fehleransicht empfangen können.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-134">Allow anonymous access to the method so that unauthenticated users are able to receive the error view.</span></span>

<span data-ttu-id="9e1d8-135">Folgende Fehlerhandlermethode wird beispielsweise in der MVC-Vorlage [dotnet new](/dotnet/core/tools/dotnet-new) bereitgestellt und im Home-Controller angezeigt:</span><span class="sxs-lookup"><span data-stu-id="9e1d8-135">For example, the following error handler method is provided by the [dotnet new](/dotnet/core/tools/dotnet-new) MVC template and appears in the Home controller:</span></span>

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

## <a name="configuring-status-code-pages"></a><span data-ttu-id="9e1d8-136">Konfigurieren von Statuscodeseiten</span><span class="sxs-lookup"><span data-stu-id="9e1d8-136">Configuring status code pages</span></span>

<span data-ttu-id="9e1d8-137">Ihre App stellt für HTTP-Statuscodes wie *404 – Nicht gefunden* standardmäßig keine ausführliche Statuscodeseite zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-137">By default, an app doesn't provide a rich status code page for HTTP status codes, such as *404 Not Found*.</span></span> <span data-ttu-id="9e1d8-138">Um Statuscodeseiten bereitzustellen, muss die Middleware „Status Code Pages“ durch Hinzufügen einer Zeile zu `Startup.Configure` konfiguriert werden:</span><span class="sxs-lookup"><span data-stu-id="9e1d8-138">To provide status code pages, configure the Status Code Pages Middleware by adding a line to the `Startup.Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="9e1d8-139">Standardmäßig fügt die Middleware „Satus Code Pages“ Handler im Textformat für gängige Statuscodes wie 404 hinzu:</span><span class="sxs-lookup"><span data-stu-id="9e1d8-139">By default, Status Code Pages Middleware adds text-only handlers for common status codes, such as 404:</span></span>

![404-Seite](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="9e1d8-141">Die Middleware unterstützt zahlreiche Erweiterungsmethoden.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-141">The middleware supports several extension methods.</span></span> <span data-ttu-id="9e1d8-142">Eine Methode verwendet einen Lambdaausdruck:</span><span class="sxs-lookup"><span data-stu-id="9e1d8-142">One method takes a lambda expression:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="9e1d8-143">Eine andere Methode verwendet einen Inhaltstyp und eine Formatzeichenfolge:</span><span class="sxs-lookup"><span data-stu-id="9e1d8-143">Another method takes a content type and format string:</span></span>

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="9e1d8-144">Es gibt auch Erweiterungsmethoden für Umleitungen und erneutes Ausführen.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-144">There's also redirect and re-execute extension methods.</span></span> <span data-ttu-id="9e1d8-145">Die Umleitungsmethode sendet den Statuscode *302 Found* (Gefunden) an den Client:</span><span class="sxs-lookup"><span data-stu-id="9e1d8-145">The redirect method sends a *302 Found* status code to the client:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="9e1d8-146">Die andere Methode gibt den ursprünglichen Statuscode an den Client zurück, führt jedoch auch den Handler für die Umleitungs-URL aus:</span><span class="sxs-lookup"><span data-stu-id="9e1d8-146">The re-execute method returns the original status code to the client but also executes the handler for the redirect URL:</span></span>

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="9e1d8-147">Statuscodeseiten können für bestimmte Anforderungen in der Handlermethode einer Razor-Seite oder in einem MVC-Controller deaktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-147">Status code pages can be disabled for specific requests in a Razor Pages handler method or in an MVC controller.</span></span> <span data-ttu-id="9e1d8-148">Versuchen Sie, [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) aus der Sammlung [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) der Anforderung abzurufen und das Feature zu deaktivieren (falls es verfügbar ist), um Statuscodeseiten zu deaktivieren:</span><span class="sxs-lookup"><span data-stu-id="9e1d8-148">To disable status code pages, attempt to retrieve the [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) from the request's [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) collection and disable the feature if it's available:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

<span data-ttu-id="9e1d8-149">Bei Verwendung einer `UseStatusCodePages*`-Überladung, die auf einen Endpunkt in der App verweist, müssen Sie eine MVC-Ansicht oder Razor Page für den Endpunkt erstellen.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-149">If using a `UseStatusCodePages*` overload that points to an endpoint within the app, create an MVC view or Razor Page for the endpoint.</span></span> <span data-ttu-id="9e1d8-150">Mit der Vorlage [dotnet new](/dotnet/core/tools/dotnet-new) für eine Razor Pages-App werden beispielsweise die folgende Seite und die folgende Seitenmodellklasse erstellt:</span><span class="sxs-lookup"><span data-stu-id="9e1d8-150">For example, the [dotnet new](/dotnet/core/tools/dotnet-new) template for a Razor Pages app produces the following page and page model class:</span></span>

<span data-ttu-id="9e1d8-151">*Error.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="9e1d8-151">*Error.cshtml*:</span></span>

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

<span data-ttu-id="9e1d8-152">*Error.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="9e1d8-152">*Error.cshtml.cs*:</span></span>

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

## <a name="exception-handling-code"></a><span data-ttu-id="9e1d8-153">Ausnahmebehandlungscode</span><span class="sxs-lookup"><span data-stu-id="9e1d8-153">Exception-handling code</span></span>

<span data-ttu-id="9e1d8-154">Code auf Seiten zur Ausnahmebehandlung kann Ausnahmen auslösen.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-154">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="9e1d8-155">Es ist häufig empfehlenswert, wenn Produktionsfehlerseiten nur statische Inhalte aufweisen.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-155">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="9e1d8-156">Bedenken Sie zudem, dass Sie den Statuscode einer Antwort nicht ändern können, sobald Sie die Header für diese Antwort gesendet haben, und dass weder Ausnahmeseiten noch Handler ausgeführt werden können.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-156">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="9e1d8-157">Die Antwort muss abgeschlossen oder die Verbindung unterbrochen werden.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-157">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="9e1d8-158">Sichere Ausnahmebehandlung</span><span class="sxs-lookup"><span data-stu-id="9e1d8-158">Server exception handling</span></span>

<span data-ttu-id="9e1d8-159">Neben der Ausnahmebehandlungslogik in Ihrer App werden auf dem [Server](xref:fundamentals/servers/index), der Ihre App hostet, einige Ausnahmebehandlungen durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-159">In addition to the exception handling logic in your app, the [server](xref:fundamentals/servers/index) hosting your app performs some exception handling.</span></span> <span data-ttu-id="9e1d8-160">Wenn der Server eine Ausnahme erfasst, bevor die Header gesendet werden, sendet der Server die Antwort *500 – Interner Serverfehler* ohne Text.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-160">If the server catches an exception before the headers are sent, the server sends a *500 Internal Server Error* response with no body.</span></span> <span data-ttu-id="9e1d8-161">Wenn der Server eine Ausnahme auffängt, nachdem die Header gesendet wurden, schließt der Server die Verbindung.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-161">If the server catches an exception after the headers have been sent, the server closes the connection.</span></span> <span data-ttu-id="9e1d8-162">Anforderungen, die nicht von Ihrer App verarbeitet werden, werden vom Server verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-162">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="9e1d8-163">Jede auftretende Ausnahme wird durch die Ausnahmebehandlung des Servers behandelt.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-163">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="9e1d8-164">Konfigurierte benutzerdefinierte Fehlerseiten, Middleware oder Filter zur Fehlerbehandlung haben keine Auswirkungen auf dieses Verhalten.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-164">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="9e1d8-165">Fehlerbehandlung während des Starts</span><span class="sxs-lookup"><span data-stu-id="9e1d8-165">Startup exception handling</span></span>

<span data-ttu-id="9e1d8-166">Nur auf Hostebene können Ausnahmen behandelt werden, die während des Starts einer App auftreten.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-166">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="9e1d8-167">Sie können mithilfe des [Webhosts](xref:fundamentals/host/web-host) [konfigurieren, wie der Host während des Starts auf Fehler reagiert](xref:fundamentals/host/web-host#detailed-errors), indem Sie die Schlüssel `captureStartupErrors` und `detailedErrors` verwenden.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-167">Using the [Web Host](xref:fundamentals/host/web-host), you can [configure how the host behaves in response to errors during startup](xref:fundamentals/host/web-host#detailed-errors) with the `captureStartupErrors` and `detailedErrors` keys.</span></span>

<span data-ttu-id="9e1d8-168">Das Hosting kann nur dann eine Fehlerseite für einen erfassten Fehler beim Start anzeigen, wenn der Fehler nach der Bindung der Hostadresse bzw. des Ports auftritt.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-168">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="9e1d8-169">Wenn eine Bindung aus irgendeinem Grund fehlschlägt, wird auf der Hostebene eine kritische Ausnahme protokolliert, der .NET-Prozess stürzt ab, und es wird keine Fehlerseite angezeigt, wenn die App auf dem [Kestrel](xref:fundamentals/servers/kestrel)-Server ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-169">If any binding fails for any reason, the hosting layer logs a critical exception, the dotnet process crashes, and no error page is displayed when the app is running on the [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

<span data-ttu-id="9e1d8-170">Wenn sie auf [IIS](/iis) oder [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ausgeführt wird, wird ein *Prozessfehler 502.5* vom [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) zurückgegeben, wenn der Prozess nicht gestartet werden kann.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-170">When running on [IIS](/iis) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 Process Failure* is returned by the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) if the process can't be started.</span></span> <span data-ttu-id="9e1d8-171">Befolgen Sie die Hinweise zur Fehlerbehebung im Thema [Troubleshoot ASP.NET Core on IIS (Problembehandlung bei ASP.NET Core in IIS)](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="9e1d8-171">Follow the troubleshooting advice in the [Troubleshoot ASP.NET Core on IIS](xref:host-and-deploy/iis/troubleshoot) topic.</span></span>

## <a name="aspnet-mvc-error-handling"></a><span data-ttu-id="9e1d8-172">ASP.NET MVC-Fehlerbehandlung</span><span class="sxs-lookup"><span data-stu-id="9e1d8-172">ASP.NET MVC error handling</span></span>

<span data-ttu-id="9e1d8-173">[MVC](xref:mvc/overview)-Apps verfügen über einige zusätzliche Optionen für die Fehlerbehandlung, z.B. das Konfigurieren von Ausnahmefiltern und das Durchführen einer Modellvalidierung.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-173">[MVC](xref:mvc/overview) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="9e1d8-174">Ausnahmefilter</span><span class="sxs-lookup"><span data-stu-id="9e1d8-174">Exception filters</span></span>

<span data-ttu-id="9e1d8-175">Ausnahmefilter können in einer MVC-App global oder auf der Grundlage eines Controllers oder einer Aktion konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-175">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="9e1d8-176">Diese Filter verarbeiten jede nicht behandelte Ausnahme, die während der Ausführung eine Controlleraktion oder eines anderen Filters auftritt.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-176">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter.</span></span> <span data-ttu-id="9e1d8-177">Diese Dateien werden andernfalls nicht aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-177">These filters aren't called otherwise.</span></span> <span data-ttu-id="9e1d8-178">Weitere Informationen dazu finden Sie unter [Filter](xref:mvc/controllers/filters).</span><span class="sxs-lookup"><span data-stu-id="9e1d8-178">To learn more, see [Filters](xref:mvc/controllers/filters).</span></span>

> [!TIP]
> <span data-ttu-id="9e1d8-179">Ausnahmefilter eignen sich zum Auffangen von Ausnahmen, die in MVC-Aktionen auftreten. Sie sind jedoch nicht so flexibel wie Middleware für die Fehlerbehandlung.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-179">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="9e1d8-180">Ziehen Sie im Allgemeinen die Verwendung der Middleware für allgemeine Fälle vor, und verwenden Sie Filter nur dann, wenn Sie die Fehlerbehandlung *auf einem anderen Weg* angehen müssen, je nachdem, welche MVC-Aktion gewählt wurde.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-180">Generally prefer the use of middleware, and use filters only where you need to perform error handling *differently* based on which MVC action is chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="9e1d8-181">Behandeln von Modellstatusfehlern</span><span class="sxs-lookup"><span data-stu-id="9e1d8-181">Handling model state errors</span></span>

<span data-ttu-id="9e1d8-182">Die [Modellvalidierung](xref:mvc/models/validation) tritt vor jeder ausgelösten Controlleraktion auf. Die Aktionsmethode ist dafür verantwortlich, `ModelState.IsValid` zu untersuchen und angemessen zu reagieren.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-182">[Model validation](xref:mvc/models/validation) occurs prior to invoking each controller action, and it's the action method's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="9e1d8-183">Einige Apps wählen eine Standardkonvention aus, um Modellvalidierungsfehler zu behandeln. Dann kann ein [Filter](xref:mvc/controllers/filters) hilfreich sein, um eine solche Richtlinie zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-183">Some apps choose to follow a standard convention for dealing with model validation errors, in which case a [filter](xref:mvc/controllers/filters) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="9e1d8-184">Sie sollten testen, wie sich Ihre Aktionen mit ungültigen Modellstatus verhalten.</span><span class="sxs-lookup"><span data-stu-id="9e1d8-184">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="9e1d8-185">Weitere Informationen finden Sie unter [Testen von Controllerlogik](xref:mvc/controllers/testing).</span><span class="sxs-lookup"><span data-stu-id="9e1d8-185">Learn more in [Test controller logic](xref:mvc/controllers/testing).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9e1d8-186">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="9e1d8-186">Additional resources</span></span>

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/azure-apps/troubleshoot>
