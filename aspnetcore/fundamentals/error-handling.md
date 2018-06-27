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
ms.openlocfilehash: 86041cf58dd88bea153eefed63a1985b6ddcacd8
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34566710"
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="6b0e6-103">Fehlerbehandlung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6b0e6-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="6b0e6-104">Von [Steve Smith](https://ardalis.com/) und [Tom Dykstra](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="6b0e6-104">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="6b0e6-105">Dieser Artikel beschreibt grundsätzliche Vorgehensweisen zur Behandlung von Fehlern in ASP.NET Core-Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-105">This article covers common appoaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="6b0e6-106">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6b0e6-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-developer-exception-page"></a><span data-ttu-id="6b0e6-107">Die Seite mit Ausnahmen für Entwickler</span><span class="sxs-lookup"><span data-stu-id="6b0e6-107">The developer exception page</span></span>

<span data-ttu-id="6b0e6-108">Wenn Sie eine App so konfigurieren möchten, dass eine Seite mit detaillierten Informationen zu Ausnahmen angezeigt wird, müssen Sie das NuGet-Paket `Microsoft.AspNetCore.Diagnostics` installieren und eine Zeile zur [Configure-Methode in der Startklasse](xref:fundamentals/startup) hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="6b0e6-108">To configure an app to display a page that shows detailed information about exceptions, install the `Microsoft.AspNetCore.Diagnostics` NuGet package and add a line to the [Configure method in the Startup class](xref:fundamentals/startup):</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

<span data-ttu-id="6b0e6-109">Fügen Sie `UseDeveloperExceptionPage` vor Middleware ein, in der Sie Ausnahmen abfangen möchten, wie z.B. `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-109">Put `UseDeveloperExceptionPage` before any middleware you want to catch exceptions in, such as `app.UseMvc`.</span></span>

>[!WARNING]
> <span data-ttu-id="6b0e6-110">Aktivieren Sie die Seite mit Ausnahmen für Entwickler **nur, wenn die App in der Entwicklungsumgebung ausgeführt wird**.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-110">Enable the developer exception page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="6b0e6-111">Wenn die App in der Produktionsumgebung ausgeführt wird, sollten Sie keine detaillierten Ausnahmeinformationen öffentlich teilen.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-111">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="6b0e6-112">[Weitere Informationen zum Konfigurieren von Umgebungen](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="6b0e6-112">[Learn more about configuring environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="6b0e6-113">Wenn Sie die Seite mit Ausnahmen für Entwickler anzeigen möchten, führen Sie die Beispielanwendung mit der auf `Development` festgelegten Umgebung aus, und fügen Sie `?throw=true` zur Basis-URL der App hinzu.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-113">To see the developer exception page, run the sample application with the environment set to `Development`, and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="6b0e6-114">Die Seite enthält mehrere Registerkarten mit Informationen zu der Ausnahme und der Anforderung.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-114">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="6b0e6-115">Die erste Registerkarte enthält eine Stapelüberwachung.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-115">The first tab includes a stack trace.</span></span> 

![Stapelüberwachung](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="6b0e6-117">Auf der nächsten Registerkarte werden, sofern vorhanden, Abfragezeichenfolgeparameter angezeigt.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-117">The next tab shows the query string parameters, if any.</span></span>

![Abfragezeichenfolgeparameter](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="6b0e6-119">Diese Anforderung enthielt keine Cookies. Andernfalls würden Sie auf der Registerkarte **Cookies** angezeigt. Sie können die Header sehen, die auf der letzten Registerkarte übergeben wurden.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-119">This request didn't have any cookies, but if it did, they would appear on the **Cookies** tab. You can see the headers that were passed in the last tab.</span></span>

![Header](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a><span data-ttu-id="6b0e6-121">Konfigurieren einer benutzerdefinierten Seite zur Ausnahmebehandlung</span><span class="sxs-lookup"><span data-stu-id="6b0e6-121">Configuring a custom exception handling page</span></span>

<span data-ttu-id="6b0e6-122">Konfigurieren Sie eine Seite zur Ausnahmebehandlung, die verwendet werden kann, wenn die App nicht in der `Development`-Umgebung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-122">Configure an exception handler page to use when the app isn't running in the `Development` environment.</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

<span data-ttu-id="6b0e6-123">In einer Razor Pages-App enthält die Vorlage [dotnet new](/dotnet/core/tools/dotnet-new) von Razor Pages im Ordner *Pages* eine Fehlerseite und eine `ErrorModel`-Seitenmodellklasse.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-123">In a Razor Pages app, the [dotnet new](/dotnet/core/tools/dotnet-new) Razor Pages template provides an Error page and `ErrorModel` page model class in the *Pages* folder.</span></span>

<span data-ttu-id="6b0e6-124">Die Aktionsmethode zur Fehlerbehandlung wird in einer MVC-App nicht durch HTTP-Methodenattribute wie `HttpGet` ergänzt.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-124">In an MVC app, don't decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="6b0e6-125">Durch explizite Verben könnte bei einigen Anforderungen verhindert werden, dass diese Methode zum Einsatz kommt.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-125">Explicit verbs prevent some requests from reaching the method.</span></span> <span data-ttu-id="6b0e6-126">Lassen Sie den anonymen Zugriff auf die Methode zu, damit nicht authentifizierte Benutzer die Fehleransicht empfangen können.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-126">Allow anonymous access to the method so that unauthenticated users are able to receive the error view.</span></span>

<span data-ttu-id="6b0e6-127">Folgende Fehlerhandlermethode wird beispielsweise in der MVC-Vorlage [dotnet new](/dotnet/core/tools/dotnet-new) bereitgestellt und im Home-Controller angezeigt:</span><span class="sxs-lookup"><span data-stu-id="6b0e6-127">For example, the following error handler method is provided by the [dotnet new](/dotnet/core/tools/dotnet-new) MVC template and appears in the Home controller:</span></span>

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

## <a name="configuring-status-code-pages"></a><span data-ttu-id="6b0e6-128">Konfigurieren von Statuscodeseiten</span><span class="sxs-lookup"><span data-stu-id="6b0e6-128">Configuring status code pages</span></span>

<span data-ttu-id="6b0e6-129">Ihre App stellt für HTTP-Statuscodes wie *404 – Nicht gefunden* standardmäßig keine ausführliche Statuscodeseite zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-129">By default, an app doesn't provide a rich status code page for HTTP status codes, such as *404 Not Found*.</span></span> <span data-ttu-id="6b0e6-130">Um Statuscodeseiten bereitzustellen, muss die Middleware „Status Code Pages“ durch Hinzufügen einer Zeile zu `Startup.Configure` konfiguriert werden:</span><span class="sxs-lookup"><span data-stu-id="6b0e6-130">To provide status code pages, configure the Status Code Pages Middleware by adding a line to the `Startup.Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="6b0e6-131">Standardmäßig fügt die Middleware „Satus Code Pages“ für gängige Statuscodes wie 404 einfache Handler im Textformat hinzu:</span><span class="sxs-lookup"><span data-stu-id="6b0e6-131">By default, Status Code Pages Middleware adds simple, text-only handlers for common status codes, such as 404:</span></span>

![404-Seite](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="6b0e6-133">Die Middleware unterstützt zahlreiche Erweiterungsmethoden.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-133">The middleware supports several extension methods.</span></span> <span data-ttu-id="6b0e6-134">Eine Methode verwendet einen Lambdaausdruck:</span><span class="sxs-lookup"><span data-stu-id="6b0e6-134">One method takes a lambda expression:</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="6b0e6-135">Eine andere Methode verwendet einen Inhaltstyp und eine Formatzeichenfolge:</span><span class="sxs-lookup"><span data-stu-id="6b0e6-135">Another method takes a content type and format string:</span></span>

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="6b0e6-136">Es gibt auch Erweiterungsmethoden für Umleitungen und erneutes Ausführen.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-136">There are also redirect and re-execute extension methods.</span></span> <span data-ttu-id="6b0e6-137">Die Umleitungsmethode sendet den Statuscode 302 an den Client:</span><span class="sxs-lookup"><span data-stu-id="6b0e6-137">The redirect method sends a 302 status code to the client:</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="6b0e6-138">Die andere Methode gibt den ursprünglichen Statuscode an den Client zurück, führt jedoch auch den Handler für die Umleitungs-URL aus:</span><span class="sxs-lookup"><span data-stu-id="6b0e6-138">The re-execute method returns the original status code to the client but also executes the handler for the redirect URL:</span></span>

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="6b0e6-139">Statuscodeseiten können für bestimmte Anforderungen in der Handlermethode einer Razor-Seite oder in einem MVC-Controller deaktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-139">Status code pages can be disabled for specific requests in a Razor Pages handler method or in an MVC controller.</span></span> <span data-ttu-id="6b0e6-140">Versuchen Sie, [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) aus der Sammlung [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) der Anforderung abzurufen und das Feature zu deaktivieren (falls es verfügbar ist), um Statuscodeseiten zu deaktivieren:</span><span class="sxs-lookup"><span data-stu-id="6b0e6-140">To disable status code pages, attempt to retrieve the [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) from the request's [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) collection and disable the feature if it's available:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

<span data-ttu-id="6b0e6-141">Bei Verwendung einer `UseStatusCodePages*`-Überladung, die auf einen Endpunkt in der App verweist, müssen Sie eine MVC-Ansicht oder Razor Page für den Endpunkt erstellen.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-141">If using a `UseStatusCodePages*` overload that points to an endpoint within the app, create an MVC view or Razor Page for the endpoint.</span></span> <span data-ttu-id="6b0e6-142">Mit der Vorlage [dotnet new](/dotnet/core/tools/dotnet-new) für eine Razor Pages-App werden beispielsweise die folgende Seite und die folgende Seitenmodellklasse erstellt:</span><span class="sxs-lookup"><span data-stu-id="6b0e6-142">For example, the [dotnet new](/dotnet/core/tools/dotnet-new) template for a Razor Pages app produces the following page and page model class:</span></span>

<span data-ttu-id="6b0e6-143">*Error.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6b0e6-143">*Error.cshtml*:</span></span>

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
    Swapping to <strong>Development</strong> environment will display more detailed information about the error that occurred.
</p>
<p>
    <strong>Development environment should not be enabled in deployed applications</strong>, as it can result in sensitive information from exceptions being displayed to end users. For local debugging, development environment can be enabled by setting the <strong>ASPNETCORE_ENVIRONMENT</strong> environment variable to <strong>Development</strong>, and restarting the application.
</p>
```

<span data-ttu-id="6b0e6-144">*Error.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="6b0e6-144">*Error.cshtml.cs*:</span></span>

```csharp
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)]
    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="6b0e6-145">Ausnahmebehandlungscode</span><span class="sxs-lookup"><span data-stu-id="6b0e6-145">Exception-handling code</span></span>

<span data-ttu-id="6b0e6-146">Code auf Seiten zur Ausnahmebehandlung kann Ausnahmen auslösen.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-146">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="6b0e6-147">Es ist häufig empfehlenswert, wenn Produktionsfehlerseiten nur statische Inhalte aufweisen.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-147">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="6b0e6-148">Bedenken Sie zudem, dass Sie den Statuscode einer Antwort nicht ändern können, sobald Sie die Header für diese Antwort gesendet haben, und dass weder Ausnahmeseiten noch Handler ausgeführt werden können.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-148">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="6b0e6-149">Die Antwort muss abgeschlossen oder die Verbindung unterbrochen werden.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-149">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="6b0e6-150">Sichere Ausnahmebehandlung</span><span class="sxs-lookup"><span data-stu-id="6b0e6-150">Server exception handling</span></span>

<span data-ttu-id="6b0e6-151">Neben der Ausnahmebehandlungslogik in Ihrer App werden auf dem [Server](xref:fundamentals/servers/index), der Ihre App hostet, einige Ausnahmebehandlungen durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-151">In addition to the exception handling logic in your app, the [server](xref:fundamentals/servers/index) hosting your app performs some exception handling.</span></span> <span data-ttu-id="6b0e6-152">Wenn der Server eine Ausnahme erfasst, bevor die Header gesendet werden, sendet der Server die Antwort *500 – Interner Serverfehler* ohne Text.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-152">If the server catches an exception before the headers are sent, the server sends a *500 Internal Server Error* response with no body.</span></span> <span data-ttu-id="6b0e6-153">Wenn der Server eine Ausnahme auffängt, nachdem die Header gesendet wurden, schließt der Server die Verbindung.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-153">If the server catches an exception after the headers have been sent, the server closes the connection.</span></span> <span data-ttu-id="6b0e6-154">Anforderungen, die nicht von Ihrer App verarbeitet werden, werden vom Server verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-154">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="6b0e6-155">Jede auftretende Ausnahme wird durch die Ausnahmebehandlung des Servers behandelt.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-155">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="6b0e6-156">Konfigurierte benutzerdefinierte Fehlerseiten, Middleware oder Filter zur Fehlerbehandlung haben keine Auswirkungen auf dieses Verhalten.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-156">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="6b0e6-157">Fehlerbehandlung während des Starts</span><span class="sxs-lookup"><span data-stu-id="6b0e6-157">Startup exception handling</span></span>

<span data-ttu-id="6b0e6-158">Nur auf Hostebene können Ausnahmen behandelt werden, die während des Starts einer App auftreten.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-158">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="6b0e6-159">Sie können mithilfe des [Webhosts](xref:fundamentals/host/web-host) [konfigurieren, wie der Host während des Starts auf Fehler reagiert](xref:fundamentals/host/web-host#detailed-errors), indem Sie die Schlüssel `captureStartupErrors` und `detailedErrors` verwenden.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-159">Using the [Web Host](xref:fundamentals/host/web-host), you can [configure how the host behaves in response to errors during startup](xref:fundamentals/host/web-host#detailed-errors) with the `captureStartupErrors` and `detailedErrors` keys.</span></span>

<span data-ttu-id="6b0e6-160">Das Hosting kann nur dann eine Fehlerseite für einen erfassten Fehler beim Start anzeigen, wenn der Fehler nach der Bindung der Hostadresse bzw. des Ports auftritt.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-160">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="6b0e6-161">Wenn eine Bindung aus irgendeinem Grund fehlschlägt, wird auf der Hostebene eine kritische Ausnahme protokolliert, der .NET-Prozess stürzt ab, und es wird keine Fehlerseite angezeigt, wenn die App auf dem [Kestrel](xref:fundamentals/servers/kestrel)-Server ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-161">If any binding fails for any reason, the hosting layer logs a critical exception, the dotnet process crashes, and no error page is displayed when the app is running on the [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

<span data-ttu-id="6b0e6-162">Wenn sie auf [IIS](/iis) oder [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ausgeführt wird, wird ein *Prozessfehler 502.5* vom [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) zurückgegeben, wenn der Prozess nicht gestartet werden kann.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-162">When running on [IIS](/iis) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 Process Failure* is returned by the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) if the process can't be started.</span></span> <span data-ttu-id="6b0e6-163">Befolgen Sie die Hinweise zur Fehlerbehebung im Thema [Troubleshoot ASP.NET Core on IIS (Problembehandlung bei ASP.NET Core in IIS)](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="6b0e6-163">Follow the troubleshooting advice in the [Troubleshoot ASP.NET Core on IIS](xref:host-and-deploy/iis/troubleshoot) topic.</span></span>

## <a name="aspnet-mvc-error-handling"></a><span data-ttu-id="6b0e6-164">ASP.NET MVC-Fehlerbehandlung</span><span class="sxs-lookup"><span data-stu-id="6b0e6-164">ASP.NET MVC error handling</span></span>

<span data-ttu-id="6b0e6-165">[MVC](xref:mvc/overview)-Apps verfügen über einige zusätzliche Optionen für die Fehlerbehandlung, z.B. das Konfigurieren von Ausnahmefiltern und das Durchführen einer Modellvalidierung.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-165">[MVC](xref:mvc/overview) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="6b0e6-166">Ausnahmefilter</span><span class="sxs-lookup"><span data-stu-id="6b0e6-166">Exception Filters</span></span>

<span data-ttu-id="6b0e6-167">Ausnahmefilter können in einer MVC-App global oder auf der Grundlage eines Controllers oder einer Aktion konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-167">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="6b0e6-168">Diese Filter verarbeiten jede nicht behandelte Ausnahme, die während der Ausführung eine Controlleraktion oder eines anderen Filters auftritt, und werden andernfalls nicht aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-168">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter, and are not called otherwise.</span></span> <span data-ttu-id="6b0e6-169">Weitere Informationen zu Ausnahmefiltern finden Sie unter [Filter](xref:mvc/controllers/filters).</span><span class="sxs-lookup"><span data-stu-id="6b0e6-169">Learn more about exception filters in [Filters](xref:mvc/controllers/filters).</span></span>

> [!TIP]
> <span data-ttu-id="6b0e6-170">Ausnahmefilter eignen sich zum Auffangen von Ausnahmen, die in MVC-Aktionen auftreten. Sie sind jedoch nicht so flexibel wie Middleware für die Fehlerbehandlung.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-170">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="6b0e6-171">Es wird empfohlen, dass Sie Middleware für allgemeine Fälle vorziehen und Filter nur dann verwenden, wenn Sie die Fehlerbehandlung *auf einem anderen Weg* angehen müssen, je nachdem, welche MVC-Aktion gewählt wurde.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-171">Prefer middleware for the general case, and use filters only where you need to do error handling *differently* based on which MVC action was chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="6b0e6-172">Behandeln von Modellstatusfehlern</span><span class="sxs-lookup"><span data-stu-id="6b0e6-172">Handling Model State Errors</span></span>

<span data-ttu-id="6b0e6-173">Die [Modellvalidierung](xref:mvc/models/validation) tritt vor jeder ausgelösten Controlleraktion auf. Die Aktionsmethode ist dafür verantwortlich, `ModelState.IsValid` zu untersuchen und angemessen zu reagieren.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-173">[Model validation](xref:mvc/models/validation) occurs prior to invoking each controller action, and it's the action method's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="6b0e6-174">Einige Apps wählen eine Standardkonvention aus, um Modellvalidierungsfehler zu behandeln. Dann kann ein [Filter](xref:mvc/controllers/filters) hilfreich sein, um eine solche Richtlinie zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-174">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a [filter](xref:mvc/controllers/filters) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="6b0e6-175">Sie sollten testen, wie sich Ihre Aktionen mit ungültigen Modellstatus verhalten.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-175">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="6b0e6-176">Weitere Informationen finden Sie unter [Testen von Controllerlogik](xref:mvc/controllers/testing).</span><span class="sxs-lookup"><span data-stu-id="6b0e6-176">Learn more in [Test controller logic](xref:mvc/controllers/testing).</span></span>
