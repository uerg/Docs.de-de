---
title: ASP.NET Core Middleware
author: rick-anderson
description: Informationen Sie zu ASP.NET Core Middleware und der Anforderungspipeline.
ms.author: riande
manager: wpickett
ms.date: 01/22/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware
ms.openlocfilehash: ef130e736e2f32fa134156d979ce5bfbedcae828
ms.sourcegitcommit: 3f491f887074310fc0f145cd01a670aa63b969e3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2018
---
# <a name="aspnet-core-middleware-fundamentals"></a><span data-ttu-id="e7653-103">ASP.NET Core Middleware-Grundlagen</span><span class="sxs-lookup"><span data-stu-id="e7653-103">ASP.NET Core Middleware Fundamentals</span></span>

<a name="fundamentals-middleware"></a>

<span data-ttu-id="e7653-104">Durch [Rick Anderson](https://twitter.com/RickAndMSFT) und [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="e7653-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="e7653-105">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e7653-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-middleware"></a><span data-ttu-id="e7653-106">Was ist die Middleware?</span><span class="sxs-lookup"><span data-stu-id="e7653-106">What is middleware?</span></span>

<span data-ttu-id="e7653-107">Middleware ist eine Software, die in einer Pipeline der Anwendung zum Verarbeiten von Anforderungen und Antworten eingebaut wird.</span><span class="sxs-lookup"><span data-stu-id="e7653-107">Middleware is software that is assembled into an application pipeline to handle requests and responses.</span></span> <span data-ttu-id="e7653-108">Jede Komponente:</span><span class="sxs-lookup"><span data-stu-id="e7653-108">Each component:</span></span>

* <span data-ttu-id="e7653-109">Wählt aus, ob die Anforderung an die nächste Komponente in der Pipeline übergeben werden sollen.</span><span class="sxs-lookup"><span data-stu-id="e7653-109">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="e7653-110">Kann Aufgaben ausführen, bevor und nachdem die nächste Komponente in der Pipeline aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="e7653-110">Can perform work before and after the next component in the pipeline is invoked.</span></span> 

<span data-ttu-id="e7653-111">Anforderung Delegaten werden verwendet, um die Pipeline zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="e7653-111">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="e7653-112">Die Anforderung Delegaten behandeln jede HTTP-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="e7653-112">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="e7653-113">Delegaten werden mithilfe von konfiguriert anfordern [ausführen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [Zuordnung](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), und [verwenden](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) Erweiterungsmethoden [c#].</span><span class="sxs-lookup"><span data-stu-id="e7653-113">Request delegates are configured using [Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), and [Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) extension methods.</span></span> <span data-ttu-id="e7653-114">Ein einzelne Anforderung Delegaten kann angegebene Zeile als einer anonymen Methode, die (als Inline-Middleware) sein, oder sie kann in einer wiederverwendbaren Klasse definiert werden.</span><span class="sxs-lookup"><span data-stu-id="e7653-114">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="e7653-115">Diese wiederverwendbare Klassen und Inline-anonyme Methoden sind *Middleware*, oder *middlewarekomponenten gemeinsam*.</span><span class="sxs-lookup"><span data-stu-id="e7653-115">These reusable classes and in-line anonymous methods are *middleware*, or *middleware components*.</span></span> <span data-ttu-id="e7653-116">Jede middlewarekomponente in der Anforderungspipeline ist verantwortlich für die nächste Komponente in der Pipeline aufrufen oder die Kette verkürzte, falls zutreffend.</span><span class="sxs-lookup"><span data-stu-id="e7653-116">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline, or short-circuiting the chain if appropriate.</span></span>

<span data-ttu-id="e7653-117">[Migrieren von HTTP-Module auf Middleware](../migration/http-modules.md) erläutert den Unterschied zwischen der Anforderung Pipelines in ASP.NET Core und den Vorgängerversionen und stellt Beispiele für weitere Middleware.</span><span class="sxs-lookup"><span data-stu-id="e7653-117">[Migrating HTTP Modules to Middleware](../migration/http-modules.md) explains the difference between request pipelines in ASP.NET Core and the previous versions and provides more middleware samples.</span></span>

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="e7653-118">Erstellen eine middlewarepipeline mit IApplicationBuilder</span><span class="sxs-lookup"><span data-stu-id="e7653-118">Creating a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="e7653-119">Die Anforderungspipeline ASP.NET Core besteht aus einer Sequenz von Anforderung-Delegaten, die nach der anderen aufgerufen, wie in diesem Diagramm (den Thread der Ausführung folgt der schwarzen Pfeile) gezeigt:</span><span class="sxs-lookup"><span data-stu-id="e7653-119">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other, as this diagram shows (the thread of execution follows the black arrows):</span></span>

![Anforderung verarbeiten des Musters mit der eine Anforderung, die empfangen, die Verarbeitung durch drei Middlewares und die Antwort, die die Anwendung.](middleware/_static/request-delegate-pipeline.png)

<span data-ttu-id="e7653-123">Jeder Delegat kann Vorgänge vor und nach dem nächsten Delegaten ausführen.</span><span class="sxs-lookup"><span data-stu-id="e7653-123">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="e7653-124">Ein Delegat können auch eine Anforderung nicht an dem nächsten Delegaten übergeben Sie die verkürzte der Anforderungspipeline aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="e7653-124">A delegate can also decide to not pass a request to the next delegate, which is called short-circuiting the request pipeline.</span></span> <span data-ttu-id="e7653-125">Verkürzte ist häufig wünschenswert, da unnötige Arbeit vermieden werden.</span><span class="sxs-lookup"><span data-stu-id="e7653-125">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="e7653-126">Die Middleware für statische Dateien kann z. B. eine Anforderung für eine statische Datei zurückgeben und Kurzschluss den Rest der Pipeline.</span><span class="sxs-lookup"><span data-stu-id="e7653-126">For example, the static file middleware can return a request for a static file and short-circuit the rest of the pipeline.</span></span> <span data-ttu-id="e7653-127">Behandlung von Ausnahmen Delegaten müssen zu einem frühen Zeitpunkt in der Pipeline aufgerufen werden, sodass sie Ausnahmen abfangen können, die in späteren Phasen der Pipeline auftreten.</span><span class="sxs-lookup"><span data-stu-id="e7653-127">Exception-handling delegates need to be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="e7653-128">Die einfachste mögliche ASP.NET Core app richtet ein Anfrage-Delegat, der alle Anforderungen verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="e7653-128">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="e7653-129">Eine tatsächliche Anforderungspipeline ist in diesem Fall enthalten.</span><span class="sxs-lookup"><span data-stu-id="e7653-129">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="e7653-130">Stattdessen wird eine anonyme Funktion als Antwort auf jede HTTP-Anforderung aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="e7653-130">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[Main](middleware/sample/Middleware/Startup.cs)]

<span data-ttu-id="e7653-131">Die erste [app. Führen Sie](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) Delegat wird die Pipeline beendet.</span><span class="sxs-lookup"><span data-stu-id="e7653-131">The first [app.Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) delegate terminates the pipeline.</span></span>

<span data-ttu-id="e7653-132">Sie können mehrere Anforderung Delegaten zusammen mit verketten [app. Verwendung](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span><span class="sxs-lookup"><span data-stu-id="e7653-132">You can chain multiple request delegates together with [app.Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span></span> <span data-ttu-id="e7653-133">Die `next` Parameter darstellt, den nächsten Delegaten in der Pipeline.</span><span class="sxs-lookup"><span data-stu-id="e7653-133">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="e7653-134">(Beachten Sie, dass Sie die Pipeline mit Circuit können *nicht* Aufrufen der *Weiter* Parameter.) Sie können in der Regel Aktionen vor und nach der nächsten Delegat ausführen, wie in diesem Beispiel veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="e7653-134">(Remember that you can short-circuit the pipeline by *not* calling the *next* parameter.) You can typically perform actions both before and after the next delegate, as this example demonstrates:</span></span>

[!code-csharp[Main](middleware/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> <span data-ttu-id="e7653-135">Rufen Sie nicht `next.Invoke` nachdem die Antwort an den Client gesendet wurde.</span><span class="sxs-lookup"><span data-stu-id="e7653-135">Do not call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="e7653-136">Änderungen an `HttpResponse` nach dem Start der Antwort wird eine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="e7653-136">Changes to `HttpResponse` after the response has started will throw an exception.</span></span> <span data-ttu-id="e7653-137">Änderungen, z. B. das Festlegen von Headern, Statuscode "" usw., werden z. B. eine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="e7653-137">For example, changes such as setting headers, status code, etc,  will throw an exception.</span></span> <span data-ttu-id="e7653-138">Schreiben in den Antworttext nach dem Aufruf `next`:</span><span class="sxs-lookup"><span data-stu-id="e7653-138">Writing to the response body after calling `next`:</span></span>
> - <span data-ttu-id="e7653-139">Kann ein Protokoll verletzen.</span><span class="sxs-lookup"><span data-stu-id="e7653-139">May cause a protocol violation.</span></span> <span data-ttu-id="e7653-140">Schreiben Sie z. B. mehr als die genannten `content-length`.</span><span class="sxs-lookup"><span data-stu-id="e7653-140">For example, writing more than the stated `content-length`.</span></span>
> - <span data-ttu-id="e7653-141">Das Textformat kann zu einer Beschädigung.</span><span class="sxs-lookup"><span data-stu-id="e7653-141">May corrupt the body format.</span></span> <span data-ttu-id="e7653-142">Schreiben Sie z. B. eine HTML-Fußzeile in einer CSS-Datei.</span><span class="sxs-lookup"><span data-stu-id="e7653-142">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="e7653-143">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) ist ein Hinweis nützlich, um anzugeben, ob der Header gesendet wurden, und/oder der Text wurde in den geschrieben.</span><span class="sxs-lookup"><span data-stu-id="e7653-143">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) is a useful hint to indicate if headers have been sent and/or the body has been written to.</span></span>

## <a name="ordering"></a><span data-ttu-id="e7653-144">Sortieren</span><span class="sxs-lookup"><span data-stu-id="e7653-144">Ordering</span></span>

<span data-ttu-id="e7653-145">Die Reihenfolge, die Middleware Komponenten, in hinzugefügt werden der `Configure` Methode definiert, die Reihenfolge, in dem sie aufgerufen werden für Anforderungen, und die umgekehrte Reihenfolge für die Antwort.</span><span class="sxs-lookup"><span data-stu-id="e7653-145">The order that middleware components are added in the `Configure` method defines the order in which they are invoked on requests, and the reverse order for the response.</span></span> <span data-ttu-id="e7653-146">Diese Reihenfolge ist wichtig für die Sicherheit, Leistung und Funktionalität.</span><span class="sxs-lookup"><span data-stu-id="e7653-146">This ordering is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="e7653-147">Die Configure-Methode (siehe unten) Fügt die folgenden Middleware-Komponenten:</span><span class="sxs-lookup"><span data-stu-id="e7653-147">The Configure method (shown below) adds the following middleware components:</span></span>

1. <span data-ttu-id="e7653-148">Ausnahme/Fehlerbehandlung</span><span class="sxs-lookup"><span data-stu-id="e7653-148">Exception/error handling</span></span>
2. <span data-ttu-id="e7653-149">Statische Dateiserver</span><span class="sxs-lookup"><span data-stu-id="e7653-149">Static file server</span></span>
3. <span data-ttu-id="e7653-150">Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="e7653-150">Authentication</span></span>
4. <span data-ttu-id="e7653-151">MVC</span><span class="sxs-lookup"><span data-stu-id="e7653-151">MVC</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e7653-152">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e7653-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseAuthentication();               // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e7653-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e7653-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseIdentity();                     // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

-----------

<span data-ttu-id="e7653-154">Im obigen Code `UseExceptionHandler` ist die erste middlewarekomponente, die der Pipeline hinzugefügt – aus diesem Grund, fängt Sie alle Ausnahmen, die in späteren Aufrufen auftreten.</span><span class="sxs-lookup"><span data-stu-id="e7653-154">In the code above, `UseExceptionHandler` is the first middleware component added to the pipeline—therefore, it catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="e7653-155">Die Middleware für statische Dateien wird einem frühen Zeitpunkt in der Pipeline aufgerufen, damit Anforderungen verarbeitet und Kurzschluss, ohne die verbleibenden Komponenten durchlaufen werden kann.</span><span class="sxs-lookup"><span data-stu-id="e7653-155">The static file middleware is called early in the pipeline so it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="e7653-156">Stellt die Middleware für statische Dateien **keine** autorisierungsüberprüfungen vorgenommen.</span><span class="sxs-lookup"><span data-stu-id="e7653-156">The static file middleware provides **no** authorization checks.</span></span> <span data-ttu-id="e7653-157">Alle Dateien von bedient, u. a. die *"Wwwroot"*, sind öffentlich verfügbar.</span><span class="sxs-lookup"><span data-stu-id="e7653-157">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="e7653-158">Finden Sie unter [arbeiten mit statischen Dateien](xref:fundamentals/static-files) eine Methode, um statische Dateien zu sichern.</span><span class="sxs-lookup"><span data-stu-id="e7653-158">See [Working with static files](xref:fundamentals/static-files) for an approach to secure static files.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e7653-159">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e7653-159">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


<span data-ttu-id="e7653-160">Wenn die Anforderung nicht von der Middleware für statische Dateien behandelt wird, erfolgt eine Übergabe auf an die Identity-Middleware (`app.UseAuthentication`), die die Authentifizierung durchführt.</span><span class="sxs-lookup"><span data-stu-id="e7653-160">If the request is not handled by the static file middleware, it's passed on to the Identity middleware (`app.UseAuthentication`), which performs authentication.</span></span> <span data-ttu-id="e7653-161">Identität nicht authentifizierte Anforderungen keinen Kurzschluss ausführt.</span><span class="sxs-lookup"><span data-stu-id="e7653-161">Identity does not short-circuit unauthenticated requests.</span></span> <span data-ttu-id="e7653-162">Obwohl Identität Anforderungen authentifiziert hat, tritt auf, Autorisierung (und Ablehnung) erst nach MVC einem bestimmten Razor-Seite oder Controller und Aktion auswählt.</span><span class="sxs-lookup"><span data-stu-id="e7653-162">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific Razor Page or controller and action.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e7653-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e7653-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e7653-164">Wenn die Anforderung nicht von der Middleware für statische Dateien behandelt wird, erfolgt eine Übergabe auf an die Identity-Middleware (`app.UseIdentity`), die die Authentifizierung durchführt.</span><span class="sxs-lookup"><span data-stu-id="e7653-164">If the request is not handled by the static file middleware, it's passed on to the Identity middleware (`app.UseIdentity`), which performs authentication.</span></span> <span data-ttu-id="e7653-165">Identität nicht authentifizierte Anforderungen keinen Kurzschluss ausführt.</span><span class="sxs-lookup"><span data-stu-id="e7653-165">Identity does not short-circuit unauthenticated requests.</span></span> <span data-ttu-id="e7653-166">Obwohl Identität Anforderungen authentifiziert hat, tritt auf, Autorisierung (und Ablehnung) erst nach MVC einen bestimmten Controller und Aktion auswählt.</span><span class="sxs-lookup"><span data-stu-id="e7653-166">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

-----------

<span data-ttu-id="e7653-167">Das folgende Beispiel zeigt eine Middleware, die Reihenfolge, in dem Anforderungen für statische Dateien vom die Middleware für statische Dateien vor der Komprimierung-Middleware Antwort behandelt werden.</span><span class="sxs-lookup"><span data-stu-id="e7653-167">The following example demonstrates a middleware ordering where requests for static files are handled by the static file middleware before the response compression middleware.</span></span> <span data-ttu-id="e7653-168">Statische Dateien werden nicht komprimiert, mit der diese Reihenfolge der Middleware.</span><span class="sxs-lookup"><span data-stu-id="e7653-168">Static files are not compressed with this ordering of the middleware.</span></span> <span data-ttu-id="e7653-169">Das MVC-Antworten von [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) können komprimiert werden.</span><span class="sxs-lookup"><span data-stu-id="e7653-169">The MVC responses from [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseStaticFiles();         // Static files not compressed
                                  // by middleware.
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

<a name="middleware-run-map-use"></a>

### <a name="use-run-and-map"></a><span data-ttu-id="e7653-170">Verwenden Sie, führen Sie aus und zuordnen</span><span class="sxs-lookup"><span data-stu-id="e7653-170">Use, Run, and Map</span></span>

<span data-ttu-id="e7653-171">Sie konfigurieren Sie das HTTP-Pipeline mit `Use`, `Run`, und `Map`.</span><span class="sxs-lookup"><span data-stu-id="e7653-171">You configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="e7653-172">Die `Use` Methode kann die Pipeline Kurzschluss (d. h., wenn er nicht aufgerufen wird eine `next` Anforderung Delegaten).</span><span class="sxs-lookup"><span data-stu-id="e7653-172">The `Use` method can short-circuit the pipeline (that is, if it does not call a `next` request delegate).</span></span> <span data-ttu-id="e7653-173">`Run`ist eine Konvention, und einige Komponenten Middleware können ausgesetzt `Run[Middleware]` Methoden, die am Ende der Pipeline ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="e7653-173">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="e7653-174">`Map*`Erweiterungen werden als eine Konvention verwendet, für die Pipeline zu verzweigen.</span><span class="sxs-lookup"><span data-stu-id="e7653-174">`Map*` extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="e7653-175">[Zuordnung](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) Verzweigungen die Anforderungspipeline auf Übereinstimmungen von den angegebenen Anforderungspfad basierend.</span><span class="sxs-lookup"><span data-stu-id="e7653-175">[Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="e7653-176">Wenn der Anforderungspfad mit dem angegebenen Pfad beginnt, wird die Verzweigung ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="e7653-176">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[Main](middleware/sample/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="e7653-177">Die folgende Tabelle zeigt die Anforderungen und Antworten von `http://localhost:1234` mit dem vorherigen Code:</span><span class="sxs-lookup"><span data-stu-id="e7653-177">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="e7653-178">Anforderung</span><span class="sxs-lookup"><span data-stu-id="e7653-178">Request</span></span> | <span data-ttu-id="e7653-179">Antwort</span><span class="sxs-lookup"><span data-stu-id="e7653-179">Response</span></span> |
| --- | --- |
| <span data-ttu-id="e7653-180">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="e7653-180">localhost:1234</span></span> | <span data-ttu-id="e7653-181">Hallo von nicht-Map-Delegat.</span><span class="sxs-lookup"><span data-stu-id="e7653-181">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="e7653-182">Localhost:1234 / "map1"</span><span class="sxs-lookup"><span data-stu-id="e7653-182">localhost:1234/map1</span></span> | <span data-ttu-id="e7653-183">Test 1 zuordnen</span><span class="sxs-lookup"><span data-stu-id="e7653-183">Map Test 1</span></span> |
| <span data-ttu-id="e7653-184">Localhost:1234 / map2</span><span class="sxs-lookup"><span data-stu-id="e7653-184">localhost:1234/map2</span></span> | <span data-ttu-id="e7653-185">Test 2 für Zuordnung</span><span class="sxs-lookup"><span data-stu-id="e7653-185">Map Test 2</span></span> |
| <span data-ttu-id="e7653-186">Localhost:1234 / map3</span><span class="sxs-lookup"><span data-stu-id="e7653-186">localhost:1234/map3</span></span> | <span data-ttu-id="e7653-187">Hallo von nicht-Map-Delegat.</span><span class="sxs-lookup"><span data-stu-id="e7653-187">Hello from non-Map delegate.</span></span>  |

<span data-ttu-id="e7653-188">Wenn `Map` wird verwendet, werden die übereinstimmenden Pfad Segment(s) aus entfernt `HttpRequest.Path` und auf angefügten `HttpRequest.PathBase` für jede Anforderung.</span><span class="sxs-lookup"><span data-stu-id="e7653-188">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="e7653-189">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) Verzweigungen die Anforderungspipeline auf dem Ergebnis des angegebenen Prädikats basierend.</span><span class="sxs-lookup"><span data-stu-id="e7653-189">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="e7653-190">Jedes Prädikat des Typs `Func<HttpContext, bool>` kann zum Zuordnen von Anforderungen in eine neue Verzweigung der Pipeline verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="e7653-190">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="e7653-191">Im folgenden Beispiel wird ein Prädikat zum Erkennen des Vorhandenseins von Abfragezeichenfolgen-Variable verwendet `branch`:</span><span class="sxs-lookup"><span data-stu-id="e7653-191">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[Main](middleware/sample/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="e7653-192">Die folgende Tabelle zeigt die Anforderungen und Antworten von `http://localhost:1234` mit dem vorherigen Code:</span><span class="sxs-lookup"><span data-stu-id="e7653-192">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="e7653-193">Anforderung</span><span class="sxs-lookup"><span data-stu-id="e7653-193">Request</span></span> | <span data-ttu-id="e7653-194">Antwort</span><span class="sxs-lookup"><span data-stu-id="e7653-194">Response</span></span> |
| --- | --- |
| <span data-ttu-id="e7653-195">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="e7653-195">localhost:1234</span></span> | <span data-ttu-id="e7653-196">Hallo von nicht-Map-Delegat.</span><span class="sxs-lookup"><span data-stu-id="e7653-196">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="e7653-197">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="e7653-197">localhost:1234/?branch=master</span></span> | <span data-ttu-id="e7653-198">Verzweigung verwendet = Master</span><span class="sxs-lookup"><span data-stu-id="e7653-198">Branch used = master</span></span>|

<span data-ttu-id="e7653-199">`Map`unterstützt das schachteln, z. B.:</span><span class="sxs-lookup"><span data-stu-id="e7653-199">`Map` supports nesting, for example:</span></span>

```csharp
app.Map("/level1", level1App => {
       level1App.Map("/level2a", level2AApp => {
           // "/level1/level2a"
           //...
       });
       level1App.Map("/level2b", level2BApp => {
           // "/level1/level2b"
           //...
       });
   });
   ```

<span data-ttu-id="e7653-200">`Map`kann auch mehrere Segmente gleichzeitig, zum Beispiel entsprechen:</span><span class="sxs-lookup"><span data-stu-id="e7653-200">`Map` can also match multiple segments at once, for example:</span></span>

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a><span data-ttu-id="e7653-201">Integrierte middleware</span><span class="sxs-lookup"><span data-stu-id="e7653-201">Built-in middleware</span></span>

<span data-ttu-id="e7653-202">ASP.NET Core enthält die folgenden Middleware-Komponenten sowie eine Beschreibung der Reihenfolge, in der sie hinzugefügt werden:</span><span class="sxs-lookup"><span data-stu-id="e7653-202">ASP.NET Core ships with the following middleware components, as well as a description of the order in which they should be added:</span></span>

| <span data-ttu-id="e7653-203">Middleware</span><span class="sxs-lookup"><span data-stu-id="e7653-203">Middleware</span></span> | <span data-ttu-id="e7653-204">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="e7653-204">Description</span></span> | <span data-ttu-id="e7653-205">Reihenfolge</span><span class="sxs-lookup"><span data-stu-id="e7653-205">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="e7653-206">Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="e7653-206">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="e7653-207">Bietet Unterstützung für die Authentifizierung an.</span><span class="sxs-lookup"><span data-stu-id="e7653-207">Provides authentication support.</span></span> | <span data-ttu-id="e7653-208">Vor dem `HttpContext.User` ist erforderlich.</span><span class="sxs-lookup"><span data-stu-id="e7653-208">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="e7653-209">Terminaldienste für OAuth-Rückrufen.</span><span class="sxs-lookup"><span data-stu-id="e7653-209">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="e7653-210">CORS</span><span class="sxs-lookup"><span data-stu-id="e7653-210">CORS</span></span>](xref:security/cors) | <span data-ttu-id="e7653-211">Konfiguriert die Cross-Origin Resource Sharing.</span><span class="sxs-lookup"><span data-stu-id="e7653-211">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="e7653-212">Bevor Sie Komponenten, die CORS verwenden.</span><span class="sxs-lookup"><span data-stu-id="e7653-212">Before components that use CORS.</span></span> |
| [<span data-ttu-id="e7653-213">Diagnose</span><span class="sxs-lookup"><span data-stu-id="e7653-213">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="e7653-214">Konfiguriert die Diagnose.</span><span class="sxs-lookup"><span data-stu-id="e7653-214">Configures diagnostics.</span></span> | <span data-ttu-id="e7653-215">Bevor Sie Komponenten, die Fehler generieren.</span><span class="sxs-lookup"><span data-stu-id="e7653-215">Before components that generate errors.</span></span> |
| [<span data-ttu-id="e7653-216">ForwardedHeaders/HttpOverrides</span><span class="sxs-lookup"><span data-stu-id="e7653-216">ForwardedHeaders/HttpOverrides</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | <span data-ttu-id="e7653-217">Leitet Proxyanforderungen Header auf die aktuelle Anforderung.</span><span class="sxs-lookup"><span data-stu-id="e7653-217">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="e7653-218">Vor der Komponenten, die die aktualisierten Felder nutzen (Beispiele: Schema, Host, ClientIP-Methode).</span><span class="sxs-lookup"><span data-stu-id="e7653-218">Before components that consume the updated fields (examples: Scheme, Host, ClientIP, Method).</span></span> |
| [<span data-ttu-id="e7653-219">Zwischenspeichern von Antworten</span><span class="sxs-lookup"><span data-stu-id="e7653-219">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="e7653-220">Bietet Unterstützung für das Zwischenspeichern von Antworten.</span><span class="sxs-lookup"><span data-stu-id="e7653-220">Provides support for caching responses.</span></span> | <span data-ttu-id="e7653-221">Bevor Sie Komponenten, die caching erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="e7653-221">Before components that require caching.</span></span> |
| [<span data-ttu-id="e7653-222">Antwort-Komprimierung</span><span class="sxs-lookup"><span data-stu-id="e7653-222">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="e7653-223">Bietet Unterstützung für die Komprimierung von Antworten an.</span><span class="sxs-lookup"><span data-stu-id="e7653-223">Provides support for compressing responses.</span></span> | <span data-ttu-id="e7653-224">Bevor Sie Komponenten, die Komprimierung benötigen.</span><span class="sxs-lookup"><span data-stu-id="e7653-224">Before components that require compression.</span></span> |
| [<span data-ttu-id="e7653-225">RequestLocalization</span><span class="sxs-lookup"><span data-stu-id="e7653-225">RequestLocalization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="e7653-226">Stellt lokalisierungsunterstützung bereit.</span><span class="sxs-lookup"><span data-stu-id="e7653-226">Provides localization support.</span></span> | <span data-ttu-id="e7653-227">Bevor Sie vertrauliche Lokalisierung-Komponenten.</span><span class="sxs-lookup"><span data-stu-id="e7653-227">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="e7653-228">Routing</span><span class="sxs-lookup"><span data-stu-id="e7653-228">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="e7653-229">Definiert, und schränkt die Routen der Anforderung.</span><span class="sxs-lookup"><span data-stu-id="e7653-229">Defines and constrains request routes.</span></span> | <span data-ttu-id="e7653-230">Terminal übereinstimmender Routen.</span><span class="sxs-lookup"><span data-stu-id="e7653-230">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="e7653-231">Sitzung</span><span class="sxs-lookup"><span data-stu-id="e7653-231">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="e7653-232">Bietet Unterstützung für die Verwaltung von benutzersitzungen.</span><span class="sxs-lookup"><span data-stu-id="e7653-232">Provides support for managing user sessions.</span></span> | <span data-ttu-id="e7653-233">Bevor Sie die Komponenten, die Sitzung zu erfordern.</span><span class="sxs-lookup"><span data-stu-id="e7653-233">Before components that require Session.</span></span> |
| [<span data-ttu-id="e7653-234">Statische Dateien</span><span class="sxs-lookup"><span data-stu-id="e7653-234">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="e7653-235">Bietet Unterstützung für statische Dateien und die Verzeichnissuche bedient.</span><span class="sxs-lookup"><span data-stu-id="e7653-235">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="e7653-236">Terminaldienste, wenn Dateien mit einer Anforderung übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="e7653-236">Terminal if a request matches files.</span></span> |
| [<span data-ttu-id="e7653-237">URLs</span><span class="sxs-lookup"><span data-stu-id="e7653-237">URL Rewriting </span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="e7653-238">Bietet Unterstützung für das Umschreiben von URLs und das Umleiten von Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="e7653-238">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="e7653-239">Bevor Sie die Komponenten, die die URL zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="e7653-239">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="e7653-240">WebSockets</span><span class="sxs-lookup"><span data-stu-id="e7653-240">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="e7653-241">Ermöglicht das WebSockets-Protokoll.</span><span class="sxs-lookup"><span data-stu-id="e7653-241">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="e7653-242">Bevor Sie Komponenten, die zum Akzeptieren der WebSocket-Anforderungen erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="e7653-242">Before components that are required to accept WebSocket requests.</span></span> |

<a name="middleware-writing-middleware"></a>

## <a name="writing-middleware"></a><span data-ttu-id="e7653-243">Schreiben von middleware</span><span class="sxs-lookup"><span data-stu-id="e7653-243">Writing middleware</span></span>

<span data-ttu-id="e7653-244">Middleware wird in der Regel in einer Klasse gekapselt und mit einer Erweiterungsmethode verfügbar gemacht.</span><span class="sxs-lookup"><span data-stu-id="e7653-244">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="e7653-245">Betrachten Sie die folgenden Middleware, die aus der Abfragezeichenfolge die Kultur für die aktuelle Anforderung festlegt:</span><span class="sxs-lookup"><span data-stu-id="e7653-245">Consider the following middleware, which sets the culture for the current request from the query string:</span></span>

[!code-csharp[Main](middleware/sample/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="e7653-246">Hinweis: Im obigen Beispielcode dient zur Veranschaulichung erstellen eine middlewarekomponente.</span><span class="sxs-lookup"><span data-stu-id="e7653-246">Note: The sample code above is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="e7653-247">Finden Sie unter [ Globalisierung und Lokalisierung](xref:fundamentals/localization) für ASP.NET Core des integrierten lokalisierungsunterstützung.</span><span class="sxs-lookup"><span data-stu-id="e7653-247">See [ Globalization and localization](xref:fundamentals/localization) for ASP.NET Core's built-in localization support.</span></span>

<span data-ttu-id="e7653-248">Sie können die Middleware testen, indem Sie in der Kultur, z. B. Übergabe `http://localhost:7997/?culture=no`.</span><span class="sxs-lookup"><span data-stu-id="e7653-248">You can test the middleware by passing in the culture, for example `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="e7653-249">Der folgende Code wechselt der Middleware-Delegat in einer Klasse:</span><span class="sxs-lookup"><span data-stu-id="e7653-249">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddleware.cs)]

<span data-ttu-id="e7653-250">Die folgende Erweiterungsmethode macht die Middleware über [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span><span class="sxs-lookup"><span data-stu-id="e7653-250">The following extension method exposes the middleware through [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span></span>

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="e7653-251">Der folgende Code Ruft die Middleware aus `Configure`:</span><span class="sxs-lookup"><span data-stu-id="e7653-251">The following code calls the middleware from `Configure`:</span></span>

[!code-csharp[Main](middleware/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="e7653-252">Middleware befolgen die [expliziten Abhängigkeiten Prinzip](http://deviq.com/explicit-dependencies-principle/) , verfügbar machen, dessen Abhängigkeiten in ihren Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="e7653-252">Middleware should follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="e7653-253">Middleware wird einmal pro erstellt *Lebensdauer der Anwendung*.</span><span class="sxs-lookup"><span data-stu-id="e7653-253">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="e7653-254">Finden Sie unter *Abhängigkeiten pro Anforderung* unten für den Fall müssen Sie Dienste mit Middleware innerhalb einer Anforderung freigeben.</span><span class="sxs-lookup"><span data-stu-id="e7653-254">See *Per-request dependencies* below if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="e7653-255">Middleware-Komponenten können ihre Abhängigkeiten von Abhängigkeitsinjektion über Konstruktorparameter aufgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="e7653-255">Middleware components can resolve their dependencies from dependency injection through constructor parameters.</span></span> <span data-ttu-id="e7653-256">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary)kann auch weitere Parameter direkt akzeptieren.</span><span class="sxs-lookup"><span data-stu-id="e7653-256">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) can also accept additional parameters directly.</span></span>

### <a name="per-request-dependencies"></a><span data-ttu-id="e7653-257">Abhängigkeiten pro Anforderung</span><span class="sxs-lookup"><span data-stu-id="e7653-257">Per-request dependencies</span></span>

<span data-ttu-id="e7653-258">Da Middleware, beim Starten der app, nicht pro Anforderung erstellt wurde, *im Bereich einer* Lebensdauerdienste von Middleware Konstruktoren verwendet werden nicht für andere Typen Abhängigkeit eingefügter freigegeben, während jede Anforderung.</span><span class="sxs-lookup"><span data-stu-id="e7653-258">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors are not  shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="e7653-259">Wenn Sie freigeben müssen eine *im Bereich einer* service zwischen Ihrer Middleware und anderen Projekttypen, fügen Sie diese Dienste für die `Invoke` Methodensignatur.</span><span class="sxs-lookup"><span data-stu-id="e7653-259">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="e7653-260">Die `Invoke` Methode kann zusätzliche Parameter annehmen, die vom Abhängigkeitsinjektion aufgefüllt werden.</span><span class="sxs-lookup"><span data-stu-id="e7653-260">The `Invoke` method can accept additional parameters that are populated by dependency injection.</span></span> <span data-ttu-id="e7653-261">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="e7653-261">For example:</span></span>

```c#
public class MyMiddleware
{
    private readonly RequestDelegate _next;

    public MyMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="resources"></a><span data-ttu-id="e7653-262">Ressourcen</span><span class="sxs-lookup"><span data-stu-id="e7653-262">Resources</span></span>

* [<span data-ttu-id="e7653-263">Beispielcode, der in diesem Dokument verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="e7653-263">Sample code used in this doc</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample)
* [<span data-ttu-id="e7653-264">Migrating HTTP Modules to Middleware (Migration von HTTP-Modulen zu Middleware)</span><span class="sxs-lookup"><span data-stu-id="e7653-264">Migrating HTTP Modules to Middleware</span></span>](../migration/http-modules.md)
* [<span data-ttu-id="e7653-265">Application Startup (Starten von Anwendungen)</span><span class="sxs-lookup"><span data-stu-id="e7653-265">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="e7653-266">Erforderliche Funktionen</span><span class="sxs-lookup"><span data-stu-id="e7653-266">Request Features</span></span>](request-features.md)
