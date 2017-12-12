---
title: Migrieren von HTTP-Handler und Module auf ASP.NET Core authentifizierungsmiddleware beziehen.
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: tdykstra
manager: wpickett
ms.date: 12/07/2016
ms.topic: article
ms.assetid: 9c826a76-fbd2-46b5-978d-6ca6df53531a
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/http-modules
ms.openlocfilehash: f217e5264742826f285444dcbaea4b28b97c4d7e
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/29/2017
---
# <a name="migrating-http-handlers-and-modules-to-aspnet-core-middleware"></a><span data-ttu-id="c4e96-103">Migrieren von HTTP-Handler und Module auf ASP.NET Core authentifizierungsmiddleware beziehen.</span><span class="sxs-lookup"><span data-stu-id="c4e96-103">Migrating HTTP handlers and modules to ASP.NET Core middleware</span></span> 

<span data-ttu-id="c4e96-104">Durch [Matt Perdeck](https://www.linkedin.com/in/mattperdeck)</span><span class="sxs-lookup"><span data-stu-id="c4e96-104">By [Matt Perdeck](https://www.linkedin.com/in/mattperdeck)</span></span>

<span data-ttu-id="c4e96-105">In diesem Artikel wird gezeigt, wie zum Migrieren von vorhandenen ASP.NET [HTTP-Module und Ereignishandler in "System.Webserver"](https://docs.microsoft.com/iis/configuration/system.webserver/) zu ASP.NET Core [Middleware](../fundamentals/middleware.md).</span><span class="sxs-lookup"><span data-stu-id="c4e96-105">This article shows how to migrate existing ASP.NET [HTTP modules and handlers from system.webserver](https://docs.microsoft.com/iis/configuration/system.webserver/) to ASP.NET Core [middleware](../fundamentals/middleware.md).</span></span>

## <a name="modules-and-handlers-revisited"></a><span data-ttu-id="c4e96-106">Module und Handler revisited</span><span class="sxs-lookup"><span data-stu-id="c4e96-106">Modules and handlers revisited</span></span>

<span data-ttu-id="c4e96-107">Pausieren zu ASP.NET Core Middleware wir zunächst kurz zusammengefasst, wie HTTP-Module und Handler funktionieren:</span><span class="sxs-lookup"><span data-stu-id="c4e96-107">Before proceeding to ASP.NET Core middleware, let's first recap how HTTP modules and handlers work:</span></span>

![Module-Handler](http-modules/_static/moduleshandlers.png)

<span data-ttu-id="c4e96-109">**Ereignishandler sind:**</span><span class="sxs-lookup"><span data-stu-id="c4e96-109">**Handlers are:**</span></span>

   * <span data-ttu-id="c4e96-110">Klassen, in denen [IHttpHandler](https://docs.microsoft.com/dotnet/api/system.web.ihttphandler)</span><span class="sxs-lookup"><span data-stu-id="c4e96-110">Classes that implement [IHttpHandler](https://docs.microsoft.com/dotnet/api/system.web.ihttphandler)</span></span>

   * <span data-ttu-id="c4e96-111">Verwendet die Verarbeitung von Anforderungen mit einem angegebenen Dateinamen oder eine Erweiterung, z. B. *.report*</span><span class="sxs-lookup"><span data-stu-id="c4e96-111">Used to handle requests with a given file name or extension, such as *.report*</span></span>

   * <span data-ttu-id="c4e96-112">[Konfiguriert](https://docs.microsoft.com//iis/configuration/system.webserver/handlers/) in *"Web.config"*</span><span class="sxs-lookup"><span data-stu-id="c4e96-112">[Configured](https://docs.microsoft.com//iis/configuration/system.webserver/handlers/) in *Web.config*</span></span>

<span data-ttu-id="c4e96-113">**Module sind:**</span><span class="sxs-lookup"><span data-stu-id="c4e96-113">**Modules are:**</span></span>

   * <span data-ttu-id="c4e96-114">Klassen, in denen [IHttpModule](https://docs.microsoft.com/dotnet/api/system.web.ihttpmodule)</span><span class="sxs-lookup"><span data-stu-id="c4e96-114">Classes that implement [IHttpModule](https://docs.microsoft.com/dotnet/api/system.web.ihttpmodule)</span></span>

   * <span data-ttu-id="c4e96-115">Für jede Anforderung aufgerufen</span><span class="sxs-lookup"><span data-stu-id="c4e96-115">Invoked for every request</span></span>

   * <span data-ttu-id="c4e96-116">Lage Kurzschluss (Weitere Verarbeitung einer Anforderung beenden)</span><span class="sxs-lookup"><span data-stu-id="c4e96-116">Able to short-circuit (stop further processing of a request)</span></span>

   * <span data-ttu-id="c4e96-117">Die HTTP-Antwort hinzuzufügen, oder erstellen Sie ihre eigenen Lage</span><span class="sxs-lookup"><span data-stu-id="c4e96-117">Able to add to the HTTP response, or create their own</span></span>

   * <span data-ttu-id="c4e96-118">[Konfiguriert](https://docs.microsoft.com//iis/configuration/system.webserver/modules/) in *"Web.config"*</span><span class="sxs-lookup"><span data-stu-id="c4e96-118">[Configured](https://docs.microsoft.com//iis/configuration/system.webserver/modules/) in *Web.config*</span></span>

<span data-ttu-id="c4e96-119">**Die Reihenfolge, in der Module eingehende Anforderungen verarbeitet werden, wird durch bestimmt:**</span><span class="sxs-lookup"><span data-stu-id="c4e96-119">**The order in which modules process incoming requests is determined by:**</span></span>

   1. <span data-ttu-id="c4e96-120">Die [Anwendungslebenszyklus](https://msdn.microsoft.com/library/ms227673.aspx), also eine Reihe-Ereignisse, die von ASP.NET ausgelöst: [BeginRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.authenticaterequest)usw. Jedes Modul kann einen Handler für ein oder mehrere Ereignisse erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="c4e96-120">The [application life cycle](https://msdn.microsoft.com/library/ms227673.aspx), which is a series events fired by ASP.NET: [BeginRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.authenticaterequest), etc. Each module can create a handler for one or more events.</span></span>

   2. <span data-ttu-id="c4e96-121">Nach demselben Ereignis, die Reihenfolge, in der sie in konfiguriert sind *"Web.config"*.</span><span class="sxs-lookup"><span data-stu-id="c4e96-121">For the same event, the order in which they are configured in *Web.config*.</span></span>

<span data-ttu-id="c4e96-122">Zusätzlich zu den Modulen, können Sie Handler für die Lebenszyklusereignisse zu hinzufügen Ihrer *Global.asax.cs* Datei.</span><span class="sxs-lookup"><span data-stu-id="c4e96-122">In addition to modules, you can add handlers for the life cycle events to your *Global.asax.cs* file.</span></span> <span data-ttu-id="c4e96-123">Diese Handler führen Sie nach der Handler in der konfigurierten Module.</span><span class="sxs-lookup"><span data-stu-id="c4e96-123">These handlers run after the handlers in the configured modules.</span></span>

## <a name="from-handlers-and-modules-to-middleware"></a><span data-ttu-id="c4e96-124">Vom Handler und Module auf authentifizierungsmiddleware beziehen.</span><span class="sxs-lookup"><span data-stu-id="c4e96-124">From handlers and modules to middleware</span></span>

<span data-ttu-id="c4e96-125">**Middleware sind einfacher als HTTP-Module und Ereignishandler:**</span><span class="sxs-lookup"><span data-stu-id="c4e96-125">**Middleware are simpler than HTTP modules and handlers:**</span></span>

   * <span data-ttu-id="c4e96-126">Module, Handler *Global.asax.cs*, *"Web.config"* (mit Ausnahme von IIS-Konfiguration) und den Lebenszyklus der Anwendung nicht mehr vorhanden sind</span><span class="sxs-lookup"><span data-stu-id="c4e96-126">Modules, handlers, *Global.asax.cs*, *Web.config* (except for IIS configuration) and the application life cycle are gone</span></span>

   * <span data-ttu-id="c4e96-127">Die Rollen von Modulen und Handler haben von Middleware übernommen wurden</span><span class="sxs-lookup"><span data-stu-id="c4e96-127">The roles of both modules and handlers have been taken over by middleware</span></span>

   * <span data-ttu-id="c4e96-128">Middleware konfiguriert sind, mithilfe von Code anstatt in *"Web.config"*</span><span class="sxs-lookup"><span data-stu-id="c4e96-128">Middleware are configured using code rather than in *Web.config*</span></span>

   * <span data-ttu-id="c4e96-129">[Pipeline Verzweigen](../fundamentals/middleware.md#middleware-run-map-use) können Sie Anforderungen an bestimmten Middleware, die basierend auf nicht nur die URL, sondern auch von der Anforderungsheader, Abfragezeichenfolgen usw. gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="c4e96-129">[Pipeline branching](../fundamentals/middleware.md#middleware-run-map-use) lets you send requests to specific middleware, based on not only the URL but also on request headers, query strings, etc.</span></span>

<span data-ttu-id="c4e96-130">**Middleware Module sehr ähnlich sind:**</span><span class="sxs-lookup"><span data-stu-id="c4e96-130">**Middleware are very similar to modules:**</span></span>

   * <span data-ttu-id="c4e96-131">Im Prinzip für jede Anforderung aufgerufen</span><span class="sxs-lookup"><span data-stu-id="c4e96-131">Invoked in principle for every request</span></span>

   * <span data-ttu-id="c4e96-132">Kann eine Anforderung von Kurzschluss [nicht die Anforderung an die nächste Middleware übergeben](#http-modules-shortcircuiting-middleware)</span><span class="sxs-lookup"><span data-stu-id="c4e96-132">Able to short-circuit a request, by [not passing the request to the next middleware](#http-modules-shortcircuiting-middleware)</span></span>

   * <span data-ttu-id="c4e96-133">Erstellen Sie eigene HTTP-Antwort</span><span class="sxs-lookup"><span data-stu-id="c4e96-133">Able to create their own HTTP response</span></span>

<span data-ttu-id="c4e96-134">**Middleware und Module werden in einer anderen Reihenfolge verarbeitet:**</span><span class="sxs-lookup"><span data-stu-id="c4e96-134">**Middleware and modules are processed in a different order:**</span></span>

   * <span data-ttu-id="c4e96-135">Reihenfolge der Middleware basiert auf der Reihenfolge, in dem sie, in der Anforderungspipeline eingefügt werden während Reihenfolge von Modulen hauptsächlich basiert [Anwendungslebenszyklus](https://msdn.microsoft.com/library/ms227673.aspx) Ereignisse</span><span class="sxs-lookup"><span data-stu-id="c4e96-135">Order of middleware is based on the order in which they are inserted into the request pipeline, while order of modules is mainly based on [application life cycle](https://msdn.microsoft.com/library/ms227673.aspx) events</span></span>

   * <span data-ttu-id="c4e96-136">Reihenfolge der Middleware für Antworten ist das Gegenteil aus, die für Anforderungen, während der Reihenfolge von Modulen für Anforderungen und Antworten identisch ist</span><span class="sxs-lookup"><span data-stu-id="c4e96-136">Order of middleware for responses is the reverse from that for requests, while order of modules is the same for requests and responses</span></span>

   * <span data-ttu-id="c4e96-137">Finden Sie unter [erstellen eine middlewarepipeline mit IApplicationBuilder](../fundamentals/middleware.md#creating-a-middleware-pipeline-with-iapplicationbuilder)</span><span class="sxs-lookup"><span data-stu-id="c4e96-137">See [Creating a middleware pipeline with IApplicationBuilder](../fundamentals/middleware.md#creating-a-middleware-pipeline-with-iapplicationbuilder)</span></span>

![Middleware](http-modules/_static/middleware.png)

<span data-ttu-id="c4e96-139">Beachten Sie, wie in der obigen Abbildung die authentifizierungsmiddleware die Anforderung kurzgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="c4e96-139">Note how in the image above, the authentication middleware short-circuited the request.</span></span>

## <a name="migrating-module-code-to-middleware"></a><span data-ttu-id="c4e96-140">Migrieren von Modulcode auf authentifizierungsmiddleware beziehen.</span><span class="sxs-lookup"><span data-stu-id="c4e96-140">Migrating module code to middleware</span></span>

<span data-ttu-id="c4e96-141">Ein vorhandenes HTTP-Modul sieht in etwa wie folgt:</span><span class="sxs-lookup"><span data-stu-id="c4e96-141">An existing HTTP module will look similar to this:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyModule.cs?highlight=6,8,24,31)]

<span data-ttu-id="c4e96-142">Entsprechend der [Middleware](../fundamentals/middleware.md) Seite eine Middleware ASP.NET Core ist eine Klasse, die verfügbar macht eine `Invoke` Methode erstellen eine `HttpContext` und Zurückgeben einer `Task`.</span><span class="sxs-lookup"><span data-stu-id="c4e96-142">As shown in the [Middleware](../fundamentals/middleware.md) page, an ASP.NET Core middleware is a class that exposes an `Invoke` method taking an `HttpContext` and returning a `Task`.</span></span> <span data-ttu-id="c4e96-143">Ihre neue Middleware wird wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="c4e96-143">Your new middleware will look like this:</span></span>

<a name="http-modules-usemiddleware"></a>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddleware.cs?highlight=9,13,20,24,28,30,32)]

<span data-ttu-id="c4e96-144">Die oben genannten Middleware Vorlage stammt aus dem Abschnitt auf [schreiben Middleware](../fundamentals/middleware.md#middleware-writing-middleware).</span><span class="sxs-lookup"><span data-stu-id="c4e96-144">The above middleware template was taken from the section on [writing middleware](../fundamentals/middleware.md#middleware-writing-middleware).</span></span>

<span data-ttu-id="c4e96-145">Die *MyMiddlewareExtensions* Hilfsklasse erleichtert das Konfigurieren Ihrer Middleware in Ihre `Startup` Klasse.</span><span class="sxs-lookup"><span data-stu-id="c4e96-145">The *MyMiddlewareExtensions* helper class makes it easier to configure your middleware in your `Startup` class.</span></span> <span data-ttu-id="c4e96-146">Die `UseMyMiddleware` Methode der Anforderungspipeline die Middleware-Klasse hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="c4e96-146">The `UseMyMiddleware` method adds your middleware class to the request pipeline.</span></span> <span data-ttu-id="c4e96-147">Von der Middleware erforderliche Dienste abrufen in der Middleware-Konstruktor eingefügt.</span><span class="sxs-lookup"><span data-stu-id="c4e96-147">Services required by the middleware get injected in the middleware's constructor.</span></span>

<a name="http-modules-shortcircuiting-middleware"></a>

<span data-ttu-id="c4e96-148">Das Modul wird eine Anforderung, z. B. wenn der Benutzer nicht autorisiert ist möglicherweise beendet:</span><span class="sxs-lookup"><span data-stu-id="c4e96-148">Your module might terminate a request, for example if the user is not authorized:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyTerminatingModule.cs?highlight=9,10,11,12,13&name=snippet_Terminate)]

<span data-ttu-id="c4e96-149">Eine Middleware verarbeitet diese durch Aufrufen von nicht `Invoke` auf die nächste Middleware in der Pipeline.</span><span class="sxs-lookup"><span data-stu-id="c4e96-149">A middleware handles this by not calling `Invoke` on the next middleware in the pipeline.</span></span> <span data-ttu-id="c4e96-150">Behalten Sie denken Sie daran, dass dies die Anforderung nicht vollständig beendet, da die vorherige Middlewares weiterhin aufgerufen wird, wenn die Antwort die Transportadresse wieder über die Pipeline macht.</span><span class="sxs-lookup"><span data-stu-id="c4e96-150">Keep in mind that this does not fully terminate the request, because previous middlewares will still be invoked when the response makes its way back through the pipeline.</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyTerminatingMiddleware.cs?highlight=7,8&name=snippet_Terminate)]

<span data-ttu-id="c4e96-151">Bei der Migration des Moduls Funktionalität zu Ihrer neuen Middleware können möglicherweise der Code Kompilieren nicht, da die `HttpContext` -Klasse in ASP.NET Core erheblich geändert hat.</span><span class="sxs-lookup"><span data-stu-id="c4e96-151">When you migrate your module's functionality to your new middleware, you may find that your code doesn't compile because the `HttpContext` class has significantly changed in ASP.NET Core.</span></span> <span data-ttu-id="c4e96-152">[Später](#migrating-to-the-new-httpcontext), sehen Sie, wie auf den neuen ASP.NET Core HttpContext migriert.</span><span class="sxs-lookup"><span data-stu-id="c4e96-152">[Later on](#migrating-to-the-new-httpcontext), you'll see how to migrate to the new ASP.NET Core HttpContext.</span></span>

## <a name="migrating-module-insertion-into-the-request-pipeline"></a><span data-ttu-id="c4e96-153">Migrieren von Modul Einfügung in der Anforderungspipeline</span><span class="sxs-lookup"><span data-stu-id="c4e96-153">Migrating module insertion into the request pipeline</span></span>

<span data-ttu-id="c4e96-154">HTTP-Module werden in der Regel die Anforderung mithilfe hinzugefügt *"Web.config"*:</span><span class="sxs-lookup"><span data-stu-id="c4e96-154">HTTP modules are typically added to the request pipeline using *Web.config*:</span></span>

[!code-xml[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32-33,36,43,50,101)]

<span data-ttu-id="c4e96-155">Konvertieren von [Hinzufügen Ihrer neuen Middleware](../fundamentals/middleware.md#creating-a-middleware-pipeline-with-iapplicationbuilder) auf der Anforderungspipeline in Ihre `Startup` Klasse:</span><span class="sxs-lookup"><span data-stu-id="c4e96-155">Convert this by [adding your new middleware](../fundamentals/middleware.md#creating-a-middleware-pipeline-with-iapplicationbuilder) to the request pipeline in your `Startup` class:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=16)]

<span data-ttu-id="c4e96-156">Die genaue Stelle in der Pipeline, in dem Sie Ihre neue Middleware einfügen, hängt das Ereignis, das sie als Modul verarbeitet (`BeginRequest`, `EndRequest`usw.) und die Reihenfolge, in der Liste der Module in *"Web.config"*.</span><span class="sxs-lookup"><span data-stu-id="c4e96-156">The exact spot in the pipeline where you insert your new middleware depends on the event that it handled as a module (`BeginRequest`, `EndRequest`, etc.) and its order in your list of modules in *Web.config*.</span></span>

<span data-ttu-id="c4e96-157">Wie bereits erwähnt, besteht keine Anwendungslebenszyklus in ASP.NET Core und die Reihenfolge, in der Antworten von Middleware verarbeitet werden, unterscheidet sich von der Reihenfolge von Modulen verwendet.</span><span class="sxs-lookup"><span data-stu-id="c4e96-157">As previously stated, there is no application life cycle in ASP.NET Core and the order in which responses are processed by middleware differs from the order used by modules.</span></span> <span data-ttu-id="c4e96-158">Dies kann der Reihenfolge Entscheidung anspruchsvoller zugreifen können.</span><span class="sxs-lookup"><span data-stu-id="c4e96-158">This could make your ordering decision more challenging.</span></span>

<span data-ttu-id="c4e96-159">Sollte die Sortierung ein Problem darstellen, konnte Sie Ihr Modul mehrere middlewarekomponenten gemeinsam sind. aufgeteilt, die unabhängig voneinander geordnet werden kann.</span><span class="sxs-lookup"><span data-stu-id="c4e96-159">If ordering becomes a problem, you could split your module into multiple middleware components that can be ordered independently.</span></span>

## <a name="migrating-handler-code-to-middleware"></a><span data-ttu-id="c4e96-160">Migrieren von Handlercode auf authentifizierungsmiddleware beziehen.</span><span class="sxs-lookup"><span data-stu-id="c4e96-160">Migrating handler code to middleware</span></span>

<span data-ttu-id="c4e96-161">Ein HTTP-Handler sieht etwa wie folgt:</span><span class="sxs-lookup"><span data-stu-id="c4e96-161">An HTTP handler looks something like this:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/HttpHandlers/ReportHandler.cs?highlight=5,7,13,14,15,16)]

<span data-ttu-id="c4e96-162">In Ihrem Projekt ASP.NET Core würden Sie dies in eine Middleware, die etwa wie folgt übersetzt:</span><span class="sxs-lookup"><span data-stu-id="c4e96-162">In your ASP.NET Core project, you would translate this to a middleware similar to this:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/ReportHandlerMiddleware.cs?highlight=7,9,13,20,21,22,23,40,42,44)]

<span data-ttu-id="c4e96-163">Diese Middleware ähnelt stark der Middleware Module entspricht.</span><span class="sxs-lookup"><span data-stu-id="c4e96-163">This middleware is very similar to the middleware corresponding to modules.</span></span> <span data-ttu-id="c4e96-164">Der einzige Unterschied ist, wird hier nicht aufgerufen, es `_next.Invoke(context)`.</span><span class="sxs-lookup"><span data-stu-id="c4e96-164">The only real difference is that here there is no call to `_next.Invoke(context)`.</span></span> <span data-ttu-id="c4e96-165">Die dann sinnvoll, da der Handler am Ende der Anforderungspipeline, ist sodass es immer keine nächste Middleware aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="c4e96-165">That makes sense, because the handler is at the end of the request pipeline, so there will be no next middleware to invoke.</span></span>

## <a name="migrating-handler-insertion-into-the-request-pipeline"></a><span data-ttu-id="c4e96-166">Migrieren von Handler-Einfügung in der Anforderungspipeline</span><span class="sxs-lookup"><span data-stu-id="c4e96-166">Migrating handler insertion into the request pipeline</span></span>

<span data-ttu-id="c4e96-167">Konfigurieren einen HTTP-Handler erfolgt in *"Web.config"* und sieht ungefähr so aus:</span><span class="sxs-lookup"><span data-stu-id="c4e96-167">Configuring an HTTP handler is done in *Web.config* and looks something like this:</span></span>

[!code-xml[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32,46-48,50,101)]

<span data-ttu-id="c4e96-168">Sie können dies konvertieren, indem der Anforderungspipeline in Ihrer neuen Handler Middleware hinzugefügt Ihre `Startup` Klasse ähnelt Middleware aus Modulen konvertiert.</span><span class="sxs-lookup"><span data-stu-id="c4e96-168">You could convert this by adding your new handler middleware to the request pipeline in your `Startup` class, similar to middleware converted from modules.</span></span> <span data-ttu-id="c4e96-169">Das Problem bei diesem Ansatz ist, dass er alle Anforderungen an Ihre neuen Handler Middleware senden würde.</span><span class="sxs-lookup"><span data-stu-id="c4e96-169">The problem with that approach is that it would send all requests to your new handler middleware.</span></span> <span data-ttu-id="c4e96-170">Sie möchten jedoch nur Anforderungen mit einer bestimmten Erweiterung zum Erreichen Ihrer Middleware.</span><span class="sxs-lookup"><span data-stu-id="c4e96-170">However, you only want requests with a given extension to reach your middleware.</span></span> <span data-ttu-id="c4e96-171">Die würde Sie die gleiche Funktionalität zur Verfügung stellen, die Sie mit der HTTP-Handler zugewiesen waren.</span><span class="sxs-lookup"><span data-stu-id="c4e96-171">That would give you the same functionality you had with your HTTP handler.</span></span>

<span data-ttu-id="c4e96-172">Eine Lösung besteht darin, die Pipeline für Anforderungen mit einer bestimmten Erweiterung Verzweigung mithilfe der `MapWhen` -Erweiterungsmethode.</span><span class="sxs-lookup"><span data-stu-id="c4e96-172">One solution is to branch the pipeline for requests with a given extension, using the `MapWhen` extension method.</span></span> <span data-ttu-id="c4e96-173">Hierfür verwenden Sie die gleiche `Configure` Methode, in dem Sie die anderen Middleware hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="c4e96-173">You do this in the same `Configure` method where you add the other middleware:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=27-34)]

<span data-ttu-id="c4e96-174">`MapWhen`verwendet diese Parameter an:</span><span class="sxs-lookup"><span data-stu-id="c4e96-174">`MapWhen` takes these parameters:</span></span>

1. <span data-ttu-id="c4e96-175">Ein Lambda-Ausdruck, der akzeptiert die `HttpContext` und gibt `true` Falls Sie die Verzweigung die Anforderung werden soll.</span><span class="sxs-lookup"><span data-stu-id="c4e96-175">A lambda that takes the `HttpContext` and returns `true` if the request should go down the branch.</span></span> <span data-ttu-id="c4e96-176">Dies bedeutet, dass Sie Anforderungen, die nicht nur basierend auf deren Erweiterung, sondern auch auf die Anforderungsheader, Abfragezeichenfolgen-Parameter usw. verzweigen können.</span><span class="sxs-lookup"><span data-stu-id="c4e96-176">This means you can branch requests not just based on their extension, but also on request headers, query string parameters, etc.</span></span>

2. <span data-ttu-id="c4e96-177">Ein Lambda-Ausdruck, der akzeptiert ein `IApplicationBuilder` und fügt die gesamte Middleware für die Verzweigung hinzu.</span><span class="sxs-lookup"><span data-stu-id="c4e96-177">A lambda that takes an `IApplicationBuilder` and adds all the middleware for the branch.</span></span> <span data-ttu-id="c4e96-178">Dies bedeutet, dass Sie zusätzliche Middleware in der Verzweigung vor Ihrer Middleware Handler hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="c4e96-178">This means you can add additional middleware to the branch in front of your handler middleware.</span></span>

<span data-ttu-id="c4e96-179">Die Middleware für die Pipeline hinzugefügt werden, bevor die Verzweigung für alle Anforderungen aufgerufen wird; die Verzweigung wird keine Auswirkungen darauf haben.</span><span class="sxs-lookup"><span data-stu-id="c4e96-179">Middleware added to the pipeline before the branch will be invoked on all requests; the branch will have no impact on them.</span></span>

## <a name="loading-middleware-options-using-the-options-pattern"></a><span data-ttu-id="c4e96-180">Laden mithilfe des Musters Optionen-middlewareoptionen.</span><span class="sxs-lookup"><span data-stu-id="c4e96-180">Loading middleware options using the options pattern</span></span>

<span data-ttu-id="c4e96-181">Einige Module und Handler haben verschiedenen Konfigurationsoptionen, die im rowsetcache *"Web.config"*. In ASP.NET Core wird ein neues Konfigurationsmodell jedoch anstelle von verwendet *"Web.config"*.</span><span class="sxs-lookup"><span data-stu-id="c4e96-181">Some modules and handlers have configuration options that are stored in *Web.config*. However, in ASP.NET Core a new configuration model is used in place of *Web.config*.</span></span>

<span data-ttu-id="c4e96-182">Die neue [Konfigurationssystem](xref:fundamentals/configuration/index) bietet Ihnen diese Optionen, um dieses Problem zu beheben:</span><span class="sxs-lookup"><span data-stu-id="c4e96-182">The new [configuration system](xref:fundamentals/configuration/index) gives you these options to solve this:</span></span>

* <span data-ttu-id="c4e96-183">Die Optionen in der Middleware direkt einfügen, entsprechend der [nächsten Abschnitt](#loading-middleware-options-through-direct-injection).</span><span class="sxs-lookup"><span data-stu-id="c4e96-183">Directly inject the options into the middleware, as shown in the [next section](#loading-middleware-options-through-direct-injection).</span></span>

* <span data-ttu-id="c4e96-184">Verwenden der [Optionen Muster](xref:fundamentals/configuration/options):</span><span class="sxs-lookup"><span data-stu-id="c4e96-184">Use the [options pattern](xref:fundamentals/configuration/options):</span></span>

1.  <span data-ttu-id="c4e96-185">Erstellen Sie eine Klasse, um die middlewareoptionen, z. B. halten:</span><span class="sxs-lookup"><span data-stu-id="c4e96-185">Create a class to hold your middleware options, for example:</span></span>

    [!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Options)]

2.  <span data-ttu-id="c4e96-186">Speichern Sie die Optionswerte</span><span class="sxs-lookup"><span data-stu-id="c4e96-186">Store the option values</span></span>

    <span data-ttu-id="c4e96-187">Das Konfigurationssystem können Sie Optionswerte an einer beliebigen Stelle zu speichern, die Sie möchten.</span><span class="sxs-lookup"><span data-stu-id="c4e96-187">The configuration system allows you to store option values anywhere you want.</span></span> <span data-ttu-id="c4e96-188">Allerdings verwenden die meisten sites *appsettings.json*, damit wir diesen Ansatz werden müssen:</span><span class="sxs-lookup"><span data-stu-id="c4e96-188">However, most sites use *appsettings.json*, so we'll take that approach:</span></span>

    [!code-json[Main](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,14-18)]

    <span data-ttu-id="c4e96-189">*MyMiddlewareOptionsSection* hier ist ein Abschnittsname.</span><span class="sxs-lookup"><span data-stu-id="c4e96-189">*MyMiddlewareOptionsSection* here is a section name.</span></span> <span data-ttu-id="c4e96-190">Er hat keine mit den Namen der Optionsklasse identisch sein.</span><span class="sxs-lookup"><span data-stu-id="c4e96-190">It doesn't have to be the same as the name of your options class.</span></span>

3. <span data-ttu-id="c4e96-191">Ordnen Sie die Optionswerte der Options-Klasse</span><span class="sxs-lookup"><span data-stu-id="c4e96-191">Associate the option values with the options class</span></span>

    <span data-ttu-id="c4e96-192">Das Muster für die Optionen des ASP.NET Core Dependency Injection Framework zum Zuordnen von der Optionstyp den verwendet (z. B. `MyMiddlewareOptions`) mit einer `MyMiddlewareOptions` -Objekt, das die tatsächlichen Optionen hat.</span><span class="sxs-lookup"><span data-stu-id="c4e96-192">The options pattern uses ASP.NET Core's dependency injection framework to associate the options type (such as `MyMiddlewareOptions`) with a `MyMiddlewareOptions` object that has the actual options.</span></span>

    <span data-ttu-id="c4e96-193">Update der `Startup` Klasse:</span><span class="sxs-lookup"><span data-stu-id="c4e96-193">Update your `Startup` class:</span></span>

    1.  <span data-ttu-id="c4e96-194">Bei Verwendung von *appsettings.json*, Hinzufügen zum Konfigurations-Generator in der `Startup` Konstruktor:</span><span class="sxs-lookup"><span data-stu-id="c4e96-194">If you're using *appsettings.json*, add it to the configuration builder in the `Startup` constructor:</span></span>

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Ctor&highlight=5-6)]

    2.  <span data-ttu-id="c4e96-195">Konfigurieren Sie den Dienst für die Optionen:</span><span class="sxs-lookup"><span data-stu-id="c4e96-195">Configure the options service:</span></span>

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

    3.  <span data-ttu-id="c4e96-196">Ordnen Sie die Optionen der Options-Klasse:</span><span class="sxs-lookup"><span data-stu-id="c4e96-196">Associate your options with your options class:</span></span>

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=6-8)]

4.  <span data-ttu-id="c4e96-197">Fügen Sie die Optionen in der middlewarekonstruktor.</span><span class="sxs-lookup"><span data-stu-id="c4e96-197">Inject the options into your middleware constructor.</span></span> <span data-ttu-id="c4e96-198">Dies ähnelt der Optionen in einem Controller injiziert.</span><span class="sxs-lookup"><span data-stu-id="c4e96-198">This is similar to injecting options into a controller.</span></span>

  [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_MiddlewareWithParams&highlight=4,7,10,15-16)]

  <span data-ttu-id="c4e96-199">Die [UseMiddleware](#http-modules-usemiddleware) Erweiterungsmethode, die die Middleware, fügt die `IApplicationBuilder` übernimmt die Abhängigkeitsinjektion.</span><span class="sxs-lookup"><span data-stu-id="c4e96-199">The [UseMiddleware](#http-modules-usemiddleware) extension method that adds your middleware to the `IApplicationBuilder` takes care of dependency injection.</span></span>

  <span data-ttu-id="c4e96-200">Dies ist nicht beschränkt auf `IOptions` Objekte.</span><span class="sxs-lookup"><span data-stu-id="c4e96-200">This is not limited to `IOptions` objects.</span></span> <span data-ttu-id="c4e96-201">Ein anderes Objekt, das die Middleware erfordert, kann auf diese Weise eingegeben werden.</span><span class="sxs-lookup"><span data-stu-id="c4e96-201">Any other object that your middleware requires can be injected this way.</span></span>

## <a name="loading-middleware-options-through-direct-injection"></a><span data-ttu-id="c4e96-202">Laden über direkte Injection-middlewareoptionen.</span><span class="sxs-lookup"><span data-stu-id="c4e96-202">Loading middleware options through direct injection</span></span>

<span data-ttu-id="c4e96-203">Das Muster Optionen hat den Vorteil, den lose Kopplung zwischen Optionen Werte und seinem Consumer erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="c4e96-203">The options pattern has the advantage that it creates loose coupling between options values and their consumers.</span></span> <span data-ttu-id="c4e96-204">Nachdem Sie die tatsächlichen Optionen Werte eine Optionsklasse zugeordnet haben, kann jede andere Klasse Zugriff auf die Optionen über das Dependency Injection Framework abrufen.</span><span class="sxs-lookup"><span data-stu-id="c4e96-204">Once you've associated an options class with the actual options values, any other class can get access to the options through the dependency injection framework.</span></span> <span data-ttu-id="c4e96-205">Es ist nicht erforderlich, um Optionen Werte übergeben.</span><span class="sxs-lookup"><span data-stu-id="c4e96-205">There is no need to pass around options values.</span></span>

<span data-ttu-id="c4e96-206">Dieser Vorgang unterteilt aber wenn Sie die gleiche Middleware zweimal mit verschiedenen Optionen verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="c4e96-206">This breaks down though if you want to use the same middleware twice, with different options.</span></span> <span data-ttu-id="c4e96-207">Zum Beispiel eine Autorisierung verwendete Middleware in anderen Verzweigungen, sodass andere Rollen.</span><span class="sxs-lookup"><span data-stu-id="c4e96-207">For example an authorization middleware used in different branches allowing different roles.</span></span> <span data-ttu-id="c4e96-208">Die eine Optionsklasse können keine Objekte für zwei unterschiedliche Optionen zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="c4e96-208">You can't associate two different options objects with the one options class.</span></span>

<span data-ttu-id="c4e96-209">Die Lösung besteht darin, erhalten die Optionsobjekte die tatsächlichen Optionen Werte in Ihre `Startup` Klasse, und übergeben Sie die direkt für jede Instanz Ihrer Middleware.</span><span class="sxs-lookup"><span data-stu-id="c4e96-209">The solution is to get the options objects with the actual options values in your `Startup` class and pass those directly to each instance of your middleware.</span></span>

1.  <span data-ttu-id="c4e96-210">Fügen Sie einen zweiten Schlüssel zum *appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="c4e96-210">Add a second key to *appsettings.json*</span></span>

    <span data-ttu-id="c4e96-211">Um einen zweiten Satz von Optionen zum Hinzufügen der *appsettings.json* Datei, einen neuen Schlüssel verwenden, um eindeutig zu identifizieren:</span><span class="sxs-lookup"><span data-stu-id="c4e96-211">To add a second set of options to the *appsettings.json* file, use a new key to uniquely identify it:</span></span>

    [!code-json[Main](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,10-18&highlight=2-5)]

2.  <span data-ttu-id="c4e96-212">Optionen-Werte abzurufen, und dann an die Middleware weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="c4e96-212">Retrieve options values and pass them to middleware.</span></span> <span data-ttu-id="c4e96-213">Die `Use...` Erweiterungsmethode (wodurch der Pipeline Ihrer Middleware hinzugefügt) ist ein logischer Ausgangspunkt die Optionswerte übergeben:</span><span class="sxs-lookup"><span data-stu-id="c4e96-213">The `Use...` extension method (which adds your middleware to the pipeline) is a logical place to pass in the option values:</span></span> 

    [!code-csharp[Main](http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=20-23)]

4.  <span data-ttu-id="c4e96-214">Aktiviert die Middleware auszuführenden Options-Parameter.</span><span class="sxs-lookup"><span data-stu-id="c4e96-214">Enable middleware to take an options parameter.</span></span> <span data-ttu-id="c4e96-215">Geben Sie eine Überladung der `Use...` Erweiterungsmethode (, die die Options-Parameter akzeptiert und übergibt es an `UseMiddleware`).</span><span class="sxs-lookup"><span data-stu-id="c4e96-215">Provide an overload of the `Use...` extension method (that takes the options parameter and passes it to `UseMiddleware`).</span></span> <span data-ttu-id="c4e96-216">Wenn `UseMiddleware` heißt mit Parametern, übergibt die Parameter für die middlewarekonstruktor beim instanziiert die Middleware-Objekt.</span><span class="sxs-lookup"><span data-stu-id="c4e96-216">When `UseMiddleware` is called with parameters, it passes the parameters to your middleware constructor when it instantiates the middleware object.</span></span>

    [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Extensions&highlight=9-14)]

    <span data-ttu-id="c4e96-217">Beachten Sie, wie dies in der Options-Objekt umschließt ein `OptionsWrapper` Objekt.</span><span class="sxs-lookup"><span data-stu-id="c4e96-217">Note how this wraps the options object in an `OptionsWrapper` object.</span></span> <span data-ttu-id="c4e96-218">Dies implementiert `IOptions`, wie von der middlewarekonstruktor erwartet.</span><span class="sxs-lookup"><span data-stu-id="c4e96-218">This implements `IOptions`, as expected by the middleware constructor.</span></span>

## <a name="migrating-to-the-new-httpcontext"></a><span data-ttu-id="c4e96-219">Migrieren zu neuen HttpContext</span><span class="sxs-lookup"><span data-stu-id="c4e96-219">Migrating to the new HttpContext</span></span>

<span data-ttu-id="c4e96-220">Sie haben bereits gesehen, die die `Invoke` Methode in Ihrer Middleware nimmt einen Parameter vom Typ `HttpContext`:</span><span class="sxs-lookup"><span data-stu-id="c4e96-220">You saw earlier that the `Invoke` method in your middleware takes a parameter of type `HttpContext`:</span></span>

```csharp
public async Task Invoke(HttpContext context)
```

<span data-ttu-id="c4e96-221">`HttpContext`in ASP.NET Core wurde erheblich geändert werden.</span><span class="sxs-lookup"><span data-stu-id="c4e96-221">`HttpContext` has significantly changed in ASP.NET Core.</span></span> <span data-ttu-id="c4e96-222">In diesem Abschnitt wird gezeigt, wie die am häufigsten verwendeten Eigenschaften der Verschiebung [System.Web.HttpContext](https://docs.microsoft.com/dotnet/api/system.web.httpcontext) mit dem neuen `Microsoft.AspNetCore.Http.HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="c4e96-222">This section shows how to translate the most commonly used properties of [System.Web.HttpContext](https://docs.microsoft.com/dotnet/api/system.web.httpcontext) to the new `Microsoft.AspNetCore.Http.HttpContext`.</span></span>

### <a name="httpcontext"></a><span data-ttu-id="c4e96-223">HttpContext</span><span class="sxs-lookup"><span data-stu-id="c4e96-223">HttpContext</span></span>

<span data-ttu-id="c4e96-224">**HttpContext.Items** übersetzt in:</span><span class="sxs-lookup"><span data-stu-id="c4e96-224">**HttpContext.Items** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Items)]

<span data-ttu-id="c4e96-225">**Eindeutige Anforderungs-ID (kein Gegenstück System.Web.HttpContext)**</span><span class="sxs-lookup"><span data-stu-id="c4e96-225">**Unique request ID (no System.Web.HttpContext counterpart)**</span></span>

<span data-ttu-id="c4e96-226">Bietet Ihnen eine eindeutige Id für jede Anforderung.</span><span class="sxs-lookup"><span data-stu-id="c4e96-226">Gives you a unique id for each request.</span></span> <span data-ttu-id="c4e96-227">Sehr nützlich, um Ihre Protokolle einschließt.</span><span class="sxs-lookup"><span data-stu-id="c4e96-227">Very useful to include in your logs.</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Trace)]

### <a name="httpcontextrequest"></a><span data-ttu-id="c4e96-228">HttpContext.Request</span><span class="sxs-lookup"><span data-stu-id="c4e96-228">HttpContext.Request</span></span>

<span data-ttu-id="c4e96-229">**HttpContext.Request.HttpMethod** übersetzt in:</span><span class="sxs-lookup"><span data-stu-id="c4e96-229">**HttpContext.Request.HttpMethod** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Method)]

<span data-ttu-id="c4e96-230">**HttpContext.Request.QueryString** übersetzt in:</span><span class="sxs-lookup"><span data-stu-id="c4e96-230">**HttpContext.Request.QueryString** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Query)]

<span data-ttu-id="c4e96-231">**HttpContext.Request.Url** und **HttpContext.Request.RawUrl** übersetzen in:</span><span class="sxs-lookup"><span data-stu-id="c4e96-231">**HttpContext.Request.Url** and **HttpContext.Request.RawUrl** translate to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Url)]

<span data-ttu-id="c4e96-232">**HttpContext.Request.IsSecureConnection** übersetzt in:</span><span class="sxs-lookup"><span data-stu-id="c4e96-232">**HttpContext.Request.IsSecureConnection** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Secure)]

<span data-ttu-id="c4e96-233">**HttpContext.Request.UserHostAddress** übersetzt in:</span><span class="sxs-lookup"><span data-stu-id="c4e96-233">**HttpContext.Request.UserHostAddress** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Host)]

<span data-ttu-id="c4e96-234">**HttpContext.Request.Cookies** übersetzt in:</span><span class="sxs-lookup"><span data-stu-id="c4e96-234">**HttpContext.Request.Cookies** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Cookies)]

<span data-ttu-id="c4e96-235">**HttpContext.Request.RequestContext.RouteData** übersetzt in:</span><span class="sxs-lookup"><span data-stu-id="c4e96-235">**HttpContext.Request.RequestContext.RouteData** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Route)]

<span data-ttu-id="c4e96-236">**HttpContext.Request.Headers** übersetzt in:</span><span class="sxs-lookup"><span data-stu-id="c4e96-236">**HttpContext.Request.Headers** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Headers)]

<span data-ttu-id="c4e96-237">**HttpContext.Request.UserAgent** übersetzt in:</span><span class="sxs-lookup"><span data-stu-id="c4e96-237">**HttpContext.Request.UserAgent** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Agent)]

<span data-ttu-id="c4e96-238">**HttpContext.Request.UrlReferrer** übersetzt in:</span><span class="sxs-lookup"><span data-stu-id="c4e96-238">**HttpContext.Request.UrlReferrer** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Referrer)]

<span data-ttu-id="c4e96-239">**HttpContext.Request.ContentType** übersetzt in:</span><span class="sxs-lookup"><span data-stu-id="c4e96-239">**HttpContext.Request.ContentType** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Type)]

<span data-ttu-id="c4e96-240">**HttpContext.Request.Form** übersetzt in:</span><span class="sxs-lookup"><span data-stu-id="c4e96-240">**HttpContext.Request.Form** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Form)]

> [!WARNING]
> <span data-ttu-id="c4e96-241">Lesen Formularwerte nur, wenn der Inhalt Untertyp *X-www-form-urlencoded* oder *Formulardaten*.</span><span class="sxs-lookup"><span data-stu-id="c4e96-241">Read form values only if the content sub type is *x-www-form-urlencoded* or *form-data*.</span></span>

<span data-ttu-id="c4e96-242">**HttpContext.Request.InputStream** übersetzt in:</span><span class="sxs-lookup"><span data-stu-id="c4e96-242">**HttpContext.Request.InputStream** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Input)]

> [!WARNING]
> <span data-ttu-id="c4e96-243">Verwenden Sie diesen Code nur in einem Ereignishandler Typ-Middleware, am Ende einer Pipeline.</span><span class="sxs-lookup"><span data-stu-id="c4e96-243">Use this code only in a handler type middleware, at the end of a pipeline.</span></span>
>
><span data-ttu-id="c4e96-244">Sie können die unformatierten Text lesen, wie oben nur einmal pro Anforderung.</span><span class="sxs-lookup"><span data-stu-id="c4e96-244">You can read the raw body as shown above only once per request.</span></span> <span data-ttu-id="c4e96-245">Middleware, die versuchen, den Nachrichtentext lesen, nachdem der erste Lesevorgang keinen Text gelesen wird.</span><span class="sxs-lookup"><span data-stu-id="c4e96-245">Middleware trying to read the body after the first read will read an empty body.</span></span>
>
><span data-ttu-id="c4e96-246">Dies gilt nicht zum Lesen eines Formulars wie oben dargestellt, da, die aus einem Puffer erfolgt.</span><span class="sxs-lookup"><span data-stu-id="c4e96-246">This does not apply to reading a form as shown earlier, because that is done from a buffer.</span></span>

### <a name="httpcontextresponse"></a><span data-ttu-id="c4e96-247">HttpContext.Response</span><span class="sxs-lookup"><span data-stu-id="c4e96-247">HttpContext.Response</span></span>

<span data-ttu-id="c4e96-248">**HttpContext.Response.Status** und **HttpContext.Response.StatusDescription** übersetzen in:</span><span class="sxs-lookup"><span data-stu-id="c4e96-248">**HttpContext.Response.Status** and **HttpContext.Response.StatusDescription** translate to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Status)]

<span data-ttu-id="c4e96-249">**HttpContext.Response.ContentEncoding** und **HttpContext.Response.ContentType** übersetzen in:</span><span class="sxs-lookup"><span data-stu-id="c4e96-249">**HttpContext.Response.ContentEncoding** and **HttpContext.Response.ContentType** translate to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespType)]

<span data-ttu-id="c4e96-250">**HttpContext.Response.ContentType** auf einem eigenen auch übersetzt in:</span><span class="sxs-lookup"><span data-stu-id="c4e96-250">**HttpContext.Response.ContentType** on its own also translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespTypeOnly)]

<span data-ttu-id="c4e96-251">**HttpContext.Response.Output** übersetzt in:</span><span class="sxs-lookup"><span data-stu-id="c4e96-251">**HttpContext.Response.Output** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Output)]

<span data-ttu-id="c4e96-252">**HttpContext.Response.TransmitFile**</span><span class="sxs-lookup"><span data-stu-id="c4e96-252">**HttpContext.Response.TransmitFile**</span></span>

<span data-ttu-id="c4e96-253">Eine Sicherungskopie der Datei dient wird erläutert [hier](../fundamentals/request-features.md#middleware-and-request-features).</span><span class="sxs-lookup"><span data-stu-id="c4e96-253">Serving up a file is discussed [here](../fundamentals/request-features.md#middleware-and-request-features).</span></span>

<span data-ttu-id="c4e96-254">**HttpContext.Response.Headers**</span><span class="sxs-lookup"><span data-stu-id="c4e96-254">**HttpContext.Response.Headers**</span></span>

<span data-ttu-id="c4e96-255">Senden der Antwortheader durch die Tatsache komplizierter ist, wenn Sie festlegen, nachdem alle Elemente in den Antworttext geschrieben wurde, sie wird nicht gesendet.</span><span class="sxs-lookup"><span data-stu-id="c4e96-255">Sending response headers is complicated by the fact that if you set them after anything has been written to the response body, they will not be sent.</span></span>

<span data-ttu-id="c4e96-256">Die Lösung besteht darin, eine Rückrufmethode festgelegt, die vor dem Schreiben auf die Antwort beginnt rechts aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="c4e96-256">The solution is to set a callback method that will be called right before writing to the response starts.</span></span> <span data-ttu-id="c4e96-257">Dies erfolgt am besten am Anfang der `Invoke` Methode in Ihrer Middleware.</span><span class="sxs-lookup"><span data-stu-id="c4e96-257">This is best done at the start of the `Invoke` method in your middleware.</span></span> <span data-ttu-id="c4e96-258">Es ist diese Rückrufmethode, die die Antwortheader festlegt.</span><span class="sxs-lookup"><span data-stu-id="c4e96-258">It is this callback method that sets your response headers.</span></span>

<span data-ttu-id="c4e96-259">Im folgenden Code wird eine Rückrufmethode aufgerufen `SetHeaders`:</span><span class="sxs-lookup"><span data-stu-id="c4e96-259">The following code sets a callback method called `SetHeaders`:</span></span>

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

<span data-ttu-id="c4e96-260">Die `SetHeaders` Rückrufmethode würde wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="c4e96-260">The `SetHeaders` callback method would look like this:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetHeaders)]

<span data-ttu-id="c4e96-261">**HttpContext.Response.Cookies**</span><span class="sxs-lookup"><span data-stu-id="c4e96-261">**HttpContext.Response.Cookies**</span></span>

<span data-ttu-id="c4e96-262">Cookies übertragen an den Browser in einem *Set-Cookie* Antwortheader.</span><span class="sxs-lookup"><span data-stu-id="c4e96-262">Cookies travel to the browser in a *Set-Cookie* response header.</span></span> <span data-ttu-id="c4e96-263">Senden von Cookies erfordert daher demselben Rückruf verwendet zum Senden der Antwortheader:</span><span class="sxs-lookup"><span data-stu-id="c4e96-263">As a result, sending cookies requires the same callback as used for sending response headers:</span></span>

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetCookies, state: httpContext);
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

<span data-ttu-id="c4e96-264">Die `SetCookies` Rückrufmethode würde wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="c4e96-264">The `SetCookies` callback method would look like the following:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetCookies)]

## <a name="additional-resources"></a><span data-ttu-id="c4e96-265">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="c4e96-265">Additional Resources</span></span>

* [<span data-ttu-id="c4e96-266">HTTP-Handler und HTTP-Module (Übersicht)</span><span class="sxs-lookup"><span data-stu-id="c4e96-266">HTTP Handlers and HTTP Modules Overview</span></span>](https://docs.microsoft.com/iis/configuration/system.webserver/)

* [<span data-ttu-id="c4e96-267">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="c4e96-267">Configuration</span></span>](xref:fundamentals/configuration/index)

* [<span data-ttu-id="c4e96-268">Application Startup (Starten von Anwendungen)</span><span class="sxs-lookup"><span data-stu-id="c4e96-268">Application Startup</span></span>](../fundamentals/startup.md)

* [<span data-ttu-id="c4e96-269">Middleware</span><span class="sxs-lookup"><span data-stu-id="c4e96-269">Middleware</span></span>](../fundamentals/middleware.md)
