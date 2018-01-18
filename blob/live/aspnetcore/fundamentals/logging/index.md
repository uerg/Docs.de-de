---
title: Protokollierung in ASP.NET Core
author: ardalis
description: "Erfahren Sie mehr über das Protokollierungsframework in ASP.NET Core. Lernen Sie die integrierten Anbieter für die Protokollierung kennen, und erfahren Sie mehr über beliebte Anbieter von Drittanbietern."
keywords: ASP.NET Core,Protokollierung,Protokollierungsanbieter,Microsoft.Extensions.Logging,ILogger,ILoggerFactory,LogLevel,WithFilter,TraceSource,EventLog,EventSource,Bereiche
ms.author: tdykstra
manager: wpickett
ms.date: 12/15/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/logging/index
ms.openlocfilehash: 3eb167c961b8d089d508ef5622db6ae1cdd99088
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2018
---
# <a name="introduction-to-logging-in-aspnet-core"></a><span data-ttu-id="9f3b0-105">Einführung in die Protokollierung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9f3b0-105">Introduction to logging in ASP.NET Core</span></span>

<span data-ttu-id="9f3b0-106">Von [Steve Smith](https://ardalis.com/) und [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="9f3b0-106">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="9f3b0-107">ASP.NET Core unterstützt eine Protokollierungs-API, die mit mehreren verschiedenen Protokollanbietern funktioniert.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-107">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="9f3b0-108">Mit den integrierten Anbietern können Sie Protokolle an verschiedene Ziele senden, und Sie können ein Protokollierungsframework eines Drittanbieters einbinden.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-108">Built-in providers let you send logs to one or more destinations, and you can plug in a third-party logging framework.</span></span> <span data-ttu-id="9f3b0-109">Dieser Artikel zeigt, wie Sie die integrierte Protokollierungs-API und die Anbieter in Ihrem Code verwenden.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-109">This article shows how to use the built-in logging API and providers in your code.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9f3b0-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9f3b0-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9f3b0-111">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9f3b0-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9f3b0-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9f3b0-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9f3b0-113">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9f3b0-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

---

## <a name="how-to-create-logs"></a><span data-ttu-id="9f3b0-114">Erstellen von Protokollen</span><span class="sxs-lookup"><span data-stu-id="9f3b0-114">How to create logs</span></span>

<span data-ttu-id="9f3b0-115">Um Protokolle zu erstellen, rufen Sie ein `ILogger`-Objekt aus dem Container für die [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) ab:</span><span class="sxs-lookup"><span data-stu-id="9f3b0-115">To create logs, get an `ILogger` object from the [dependency injection](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="9f3b0-116">Dann rufen Sie Protokollierungsmethoden für dieses Protokollierungsobjekt auf:</span><span class="sxs-lookup"><span data-stu-id="9f3b0-116">Then call logging methods on that logger object:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="9f3b0-117">In diesem Beispiel werden Protokolle mit der Klasse `TodoController` als *Kategorie* erstellt.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-117">This example creates logs with the `TodoController` class as the *category*.</span></span> <span data-ttu-id="9f3b0-118">Kategorien werden [weiter unten in diesem Artikel](#log-category) erklärt.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-118">Categories are explained [later in this article](#log-category).</span></span>

<span data-ttu-id="9f3b0-119">ASP.NET Core stellt keine asynchronen Protokollierungsmethoden zur Verfügung, weil eine Protokollierung so schnell erfolgen sollte, dass der Aufwand einer asynchronen Implementierung nicht gerechtfertigt ist.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-119">ASP.NET Core does not provide async logger methods because logging should be so fast that it isn't worth the cost of using async.</span></span> <span data-ttu-id="9f3b0-120">Wenn dies für Sie nicht zutrifft, können Sie eine Änderung der Protokollierungsmethode in Erwägung ziehen.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-120">If you're in a situation where that's not true, consider changing the way you log.</span></span> <span data-ttu-id="9f3b0-121">Wenn Sie einen langsamen Datenspeicher verwenden, schreiben Sie die Protokollmeldungen zuerst in einen schnellen Speicher, und verschieben Sie sie dann später in einen langsamen Speicher.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-121">If your data store is slow, write the log messages to a fast store first, then move them to a slow store later.</span></span> <span data-ttu-id="9f3b0-122">Führen Sie die Protokollierung beispielsweise in einer Nachrichtenwarteschlange durch, die von einem anderen Prozess gelesen und in einem langsamen Speicher persistiert wird.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-122">For example, log to a message queue that is read and persisted to slow storage by another process.</span></span>

## <a name="how-to-add-providers"></a><span data-ttu-id="9f3b0-123">Hinzufügen von Anbietern</span><span class="sxs-lookup"><span data-stu-id="9f3b0-123">How to add providers</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9f3b0-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9f3b0-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9f3b0-125">Ein Protokollierungsanbieter ruft die mit einem `ILogger`-Objekt erstellte Meldung ab und zeigt sie an oder speichert sie.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-125">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="9f3b0-126">Beispielsweise zeigt der Konsolenanbieter Meldungen auf der Konsole an, und der Azure App Service-Anbieter kann Meldungen in Azure Blob Storage speichern.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-126">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="9f3b0-127">Um einen Anbieter zu verwenden, rufen Sie die `Add<ProviderName>`-Erweiterungsmethode des Anbieters in *Program.cs* auf:</span><span class="sxs-lookup"><span data-stu-id="9f3b0-127">To use a provider, call the provider's `Add<ProviderName>` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

<span data-ttu-id="9f3b0-128">Die Standardprojektvorlage ermöglicht die Protokollierung mit der Methode [CreateDefaultBuilder](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___):</span><span class="sxs-lookup"><span data-stu-id="9f3b0-128">The default project template enables logging with the [CreateDefaultBuilder](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___) method:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9f3b0-129">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9f3b0-129">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9f3b0-130">Ein Protokollierungsanbieter ruft die mit einem `ILogger`-Objekt erstellte Meldung ab und zeigt sie an oder speichert sie.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-130">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="9f3b0-131">Beispielsweise zeigt der Konsolenanbieter Meldungen auf der Konsole an, und der Azure App Service-Anbieter kann Meldungen in Azure Blob Storage speichern.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-131">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="9f3b0-132">Um einen Anbieter zu verwenden, installieren Sie das zugehörige NuGet-Paket und rufen die Erweiterungsmethode des Anbieters für eine Instanz von `ILoggerFactory` auf, wie im folgenden Beispiel gezeigt.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-132">To use a provider, install its NuGet package and call the provider's extension method on an instance of `ILoggerFactory`, as shown in the following example.</span></span>

[!code-csharp[](index/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="9f3b0-133">Die ASP.NET Core-[Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) stellt die `ILoggerFactory`-Instanz bereit.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-133">ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection) (DI) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="9f3b0-134">Die Erweiterungsmethoden `AddConsole` und `AddDebug` sind in den Paketen [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) und [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) definiert.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-134">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="9f3b0-135">Jede Erweiterungsmethode ruft die Methode `ILoggerFactory.AddProvider` auf und übergibt eine Instanz des Anbieters.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-135">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span> 

> [!NOTE]
> <span data-ttu-id="9f3b0-136">In der Beispielanwendung für diesen Artikel werden in der `Configure`-Methode der Klasse `Startup` Protokollierungsanbieter hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-136">The sample application for this article adds logging providers in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="9f3b0-137">Wenn Sie für zuvor ausgeführten Code eine Protokollausgabe erhalten möchten, fügen Sie stattdessen Protokollierungsanbieter im `Startup`-Klassenkonstruktor hinzu.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-137">If you want to get log output from code that executes earlier, add logging providers in the `Startup` class constructor instead.</span></span> 

---

<span data-ttu-id="9f3b0-138">Informationen zu jedem [integrierten Protokollierungsanbieter](#built-in-logging-providers) sowie Links zu [Protokollierungsanbietern von Drittanbietern](#third-party-logging-providers) finden Sie weiter unten in diesem Artikel.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-138">You'll find information about each [built-in logging provider](#built-in-logging-providers) and links to [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="9f3b0-139">Beispiel einer Protokollierungsausgabe</span><span class="sxs-lookup"><span data-stu-id="9f3b0-139">Sample logging output</span></span>

<span data-ttu-id="9f3b0-140">Wenn Sie den im vorherigen Abschnitt gezeigten Beispielcode von der Befehlszeile ausführen, werden die Protokolle in der Konsole angezeigt.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-140">With the sample code shown in the preceding section, you'll see logs in the console when you run from the command line.</span></span> <span data-ttu-id="9f3b0-141">Hier ein Beispiel für eine Konsolenausgabe:</span><span class="sxs-lookup"><span data-stu-id="9f3b0-141">Here's an example of console output:</span></span>

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/api/todo/0
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 42.9286ms
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 148.889ms 404
```
 
<span data-ttu-id="9f3b0-142">Diese Protokolle wurden durch einen Wechsel zu `http://localhost:5000/api/todo/0` erstellt, um die Ausführung beider `ILogger`-Aufrufe im vorherigen Abschnitt auszulösen.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-142">These logs were created by going to `http://localhost:5000/api/todo/0`, which triggers execution of both `ILogger` calls shown in the preceding section.</span></span>

<span data-ttu-id="9f3b0-143">Nachfolgend sehen Sie, wie die gleichen Protokolle im Debugfenster angezeigt werden, wenn Sie die Beispielanwendung in Visual Studio ausführen:</span><span class="sxs-lookup"><span data-stu-id="9f3b0-143">Here's an example of the same logs as they appear in the Debug window when you run the sample application in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

<span data-ttu-id="9f3b0-144">Die über die `ILogger`-Aufrufe aus dem vorherigen Abschnitt erstellten Protokolle beginnen mit „TodoApi.Controllers.TodoController“.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-144">The logs that were created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="9f3b0-145">Protokolle, die mit Microsoft-Kategorien beginnen, stammen aus ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-145">The logs that begin with "Microsoft" categories are from ASP.NET Core.</span></span> <span data-ttu-id="9f3b0-146">ASP.NET Core selbst und Ihr Anwendungscode verwenden dieselbe Protokollierungs-API und dieselben Protokollierungsanbieter.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-146">ASP.NET Core itself and your application code are using the same logging API and the same logging providers.</span></span>

<span data-ttu-id="9f3b0-147">In den übrigen Abschnitten dieses Artikels werden einige Details und Optionen für die Protokollierung erläutert.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-147">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="9f3b0-148">NuGet-Pakete</span><span class="sxs-lookup"><span data-stu-id="9f3b0-148">NuGet packages</span></span>

<span data-ttu-id="9f3b0-149">Die Schnittstellen `ILogger` und `ILoggerFactory` befinden sich in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), ihre Standardimplementierungen sind in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/) enthalten.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-149">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="9f3b0-150">Protokollkategorie</span><span class="sxs-lookup"><span data-stu-id="9f3b0-150">Log category</span></span>

<span data-ttu-id="9f3b0-151">Jedes von Ihnen erstellte Protokoll umfasst eine *Kategorie*.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-151">A *category* is included with each log that you create.</span></span> <span data-ttu-id="9f3b0-152">Sie geben die Kategorie an, wenn Sie ein `ILogger`-Objekt erstellen.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-152">You specify the category when you create an `ILogger` object.</span></span> <span data-ttu-id="9f3b0-153">Die Kategorie kann eine beliebige Zeichenfolge sein, aber gemäß Konvention wird der vollqualifizierte Name der Klasse verwendet, mit der die Protokolle geschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-153">The category may be any string, but a convention is to use the fully qualified name of the class from which the logs are written.</span></span> <span data-ttu-id="9f3b0-154">Beispiel: TodoApi.Controllers.TodoController</span><span class="sxs-lookup"><span data-stu-id="9f3b0-154">For example: "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="9f3b0-155">Sie können die Kategorie als Zeichenfolge angeben oder eine Erweiterungsmethode verwenden, die die Kategorie aus dem Typ ableitet.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-155">You can specify the category as a string or use an extension method that derives the category from the type.</span></span> <span data-ttu-id="9f3b0-156">Um die Kategorie als Zeichenfolge anzugeben, rufen Sie `CreateLogger` für eine `ILoggerFactory`-Instanz auf, wie unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-156">To specify the category as a string, call `CreateLogger` on an `ILoggerFactory` instance, as shown below.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

<span data-ttu-id="9f3b0-157">Meistens ist es einfacher, `ILogger<T>` zu verwenden, wie im folgenden Beispiel gezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-157">Most of the time, it will be easier to use `ILogger<T>`, as in the following example.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="9f3b0-158">Dies entspricht dem Aufruf von `CreateLogger` mit dem vollqualifizierten Typnamen `T`.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-158">This is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="9f3b0-159">Protokolliergrad</span><span class="sxs-lookup"><span data-stu-id="9f3b0-159">Log level</span></span>

<span data-ttu-id="9f3b0-160">Jedes Mal, wenn Sie ein Protokoll schreiben, geben Sie den zugehörigen [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel) an.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-160">Each time you write a log, you specify its [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="9f3b0-161">Der Protokolliergrad gibt den Schweregrad oder die Wichtigkeit an.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-161">The log level indicates the degree of severity or importance.</span></span> <span data-ttu-id="9f3b0-162">Beispielsweise könnten Sie ein `Information`-Protokoll schreiben, wenn eine Methode normal beendet wird, ein `Warning`-Protokoll, wenn eine Methode einen 404-Rückgabecode zurückgibt, und ein `Error`-Protokoll, wenn Sie eine unerwartete Ausnahme abfangen.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-162">For example, you might write an `Information` log when a method ends normally, a `Warning` log when a method returns a 404 return code, and an `Error` log when you catch an unexpected exception.</span></span>

<span data-ttu-id="9f3b0-163">Im folgenden Codebeispiel geben die Namen der Methoden (z.B. `LogWarning`) den Protokolliergrad an.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-163">In the following code example, the names of the methods (for example, `LogWarning`) specify the log level.</span></span> <span data-ttu-id="9f3b0-164">Der erste Parameter ist die [Protokollereignis-ID](#log-event-id).</span><span class="sxs-lookup"><span data-stu-id="9f3b0-164">The first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="9f3b0-165">Der zweite Parameter ist eine [Meldungsvorlage](#log-message-template) mit Platzhaltern für Argumentwerte, die von den verbleibenden Methodenparametern bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-165">The second parameter is a [message template](#log-message-template) with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="9f3b0-166">Die Methodenparameter werden weiter unten in diesem Artikel ausführlicher erläutert.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-166">The method parameters are explained in more detail later in this article.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="9f3b0-167">Protokollmethoden, die den Protokolliergrad im Methodennamen enthalten, sind [Erweiterungsmethoden für ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions).</span><span class="sxs-lookup"><span data-stu-id="9f3b0-167">Log methods that include the level in the method name are [extension methods for ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions).</span></span> <span data-ttu-id="9f3b0-168">Diese Methoden rufen im Hintergrund eine `Log`-Methode auf, die einen `LogLevel`-Parameter verwendet.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-168">Behind the scenes, these methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="9f3b0-169">Sie können anstelle eines Aufrufs dieser Erweiterungsmethoden die `Log`-Methode direkt aufrufen, aber die Syntax ist relativ kompliziert.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-169">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="9f3b0-170">Weitere Informationen finden Sie unter [ILogger-Schnittstelle](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) und im [Quellcode für Protokollierungserweiterungen](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="9f3b0-170">For more information, see the [ILogger interface](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) and the [logger extensions source code](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span></span>

<span data-ttu-id="9f3b0-171">ASP.NET Core definiert die folgenden [Protokolliergrade](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel), angeordnet vom geringsten bis zum höchsten Schweregrad.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-171">ASP.NET Core defines the following [log levels](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel), ordered here from least to highest severity.</span></span>

* <span data-ttu-id="9f3b0-172">Trace = 0</span><span class="sxs-lookup"><span data-stu-id="9f3b0-172">Trace = 0</span></span>

  <span data-ttu-id="9f3b0-173">Für Informationen, die nur für Entwickler beim Debuggen von Problemen nützlich sind.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-173">For information that is valuable only to a developer debugging an issue.</span></span> <span data-ttu-id="9f3b0-174">Diese Meldungen können sensible Anwendungsdaten enthalten und sollten daher nicht in einer Produktionsumgebung aktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-174">These messages may contain sensitive application data and so should not be enabled in a production environment.</span></span> <span data-ttu-id="9f3b0-175">*Standardmäßig deaktiviert.*</span><span class="sxs-lookup"><span data-stu-id="9f3b0-175">*Disabled by default.*</span></span> <span data-ttu-id="9f3b0-176">Ein Beispiel: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span><span class="sxs-lookup"><span data-stu-id="9f3b0-176">Example: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span></span>

* <span data-ttu-id="9f3b0-177">Debug = 1</span><span class="sxs-lookup"><span data-stu-id="9f3b0-177">Debug = 1</span></span>

  <span data-ttu-id="9f3b0-178">Für Informationen, die während der Entwicklung und beim Debuggen kurzfristig von Nutzen sind.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-178">For information that has short-term usefulness during development and debugging.</span></span> <span data-ttu-id="9f3b0-179">Beispiel: `Entering method Configure with flag set to true.` Sie würden Protokolle mit dem Protokolliergrad `Debug` aufgrund des hohen Protokollaufkommens nur zur Problembehandlung in der Produktion aktivieren.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-179">Example: `Entering method Configure with flag set to true.` You typically would not enable `Debug` level logs in production unless you are troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="9f3b0-180">Information = 2</span><span class="sxs-lookup"><span data-stu-id="9f3b0-180">Information = 2</span></span>

  <span data-ttu-id="9f3b0-181">Zur Nachverfolgung des allgemeinen Ablaufs der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-181">For tracking the general flow of the application.</span></span> <span data-ttu-id="9f3b0-182">Diese Protokolle haben typischerweise einen langfristigen Nutzen.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-182">These logs typically have some long-term value.</span></span> <span data-ttu-id="9f3b0-183">Ein Beispiel: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="9f3b0-183">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="9f3b0-184">Warning = 3</span><span class="sxs-lookup"><span data-stu-id="9f3b0-184">Warning = 3</span></span>

  <span data-ttu-id="9f3b0-185">Für ungewöhnliche oder unerwartete Ereignisse im Anwendungsverlauf.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-185">For abnormal or unexpected events in the application flow.</span></span> <span data-ttu-id="9f3b0-186">Dazu können Fehler oder andere Bedingungen gehören, die zwar nicht zum Beenden der Anwendung führen, aber untersucht werden müssen.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-186">These may include errors or other conditions that do not cause the application to stop, but which may need to be investigated.</span></span> <span data-ttu-id="9f3b0-187">Behandelte Ausnahmen sind eine typische Verwendung für den Protokolliergrad `Warning`.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-187">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="9f3b0-188">Ein Beispiel: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="9f3b0-188">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="9f3b0-189">Error = 4</span><span class="sxs-lookup"><span data-stu-id="9f3b0-189">Error = 4</span></span>

  <span data-ttu-id="9f3b0-190">Für Fehler und Ausnahmen, die nicht behandelt werden können.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-190">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="9f3b0-191">Diese Meldungen weisen auf einen Fehler in der aktuellen Aktivität oder im aktuellen Vorgang (z.B. die aktuelle HTTP-Anforderung) und nicht auf einen anwendungsweiten Fehler hin.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-191">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an application-wide failure.</span></span> <span data-ttu-id="9f3b0-192">Beispielprotokollmeldung: `Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="9f3b0-192">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="9f3b0-193">Critical = 5</span><span class="sxs-lookup"><span data-stu-id="9f3b0-193">Critical = 5</span></span>

  <span data-ttu-id="9f3b0-194">Für Fehler, die sofortige Aufmerksamkeit erfordern.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-194">For failures that require immediate attention.</span></span> <span data-ttu-id="9f3b0-195">Beispiel: Szenarios mit Datenverlust, Speichermangel.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-195">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="9f3b0-196">Mithilfe des Protokolliergrads können Sie die Menge an Protokollausgabedaten steuern, die in ein bestimmtes Speichermedium geschrieben oder an ein Anzeigefenster ausgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-196">You can use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="9f3b0-197">Beispielsweise können Sie in der Produktion alle Protokolle der Ebene `Information` und niedriger in einen Volumendatenspeicher schreiben und alle Protokolle der Ebene `Warning` und höher in einen Wertdatenspeicher ausgeben.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-197">For example, in production you might want all logs of `Information` level and lower to go to a volume data store, and all logs of `Warning` level and higher to go to a value data store.</span></span> <span data-ttu-id="9f3b0-198">Während der Entwicklung senden Sie normalerweise Protokolle mit dem Schweregrad `Warning` und höher an die Konsole.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-198">During development, you might normally send logs of `Warning` or higher severity to the console.</span></span> <span data-ttu-id="9f3b0-199">Wenn Sie eine Problembehandlung durchführen müssen, können Sie die Ebene `Debug` hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-199">Then when you need to troubleshoot, you can add `Debug` level.</span></span> <span data-ttu-id="9f3b0-200">Im Abschnitt [Protokollfilterung](#log-filtering) weiter unten in diesem Artikel wird erläutert, wie Sie steuern, welche Protokolliergrade ein Anbieter verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-200">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="9f3b0-201">Das ASP.NET Core-Framework schreibt Protokolle der Ebene `Debug` für Frameworkereignisse.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-201">The ASP.NET Core framework writes `Debug` level logs for framework events.</span></span> <span data-ttu-id="9f3b0-202">Das zuvor in diesem Artikel gezeigte Protokollbeispiel enthielt Protokolle unterhalb der Ebene `Information`, deshalb wurden keine Protokolle der Ebene `Debug` gezeigt.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-202">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` level logs were shown.</span></span> <span data-ttu-id="9f3b0-203">Hier sehen Sie ein Beispiel für Konsolenprotokolle, die bei Ausführung der Beispielanwendung angezeigt werden, wenn diese zur Anzeige von Protokollen der Ebene `Debug` und höher für den Konsolenanbieter konfiguriert ist.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-203">Here's an example of console logs if you run the sample application configured to show `Debug` and higher logs for the console provider.</span></span>

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:62555/api/todo/0
dbug: Microsoft.AspNetCore.Routing.Tree.TreeRouter[1]
      Request successfully matched the route with name 'GetTodo' and template 'api/Todo/{id}'.
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Update (TodoApi)' with id '089d59b6-92ec-472d-b552-cc613dfd625d' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Delete (TodoApi)' with id 'f3476abe-4bd9-4ad3-9261-3ead09607366' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action TodoApi.Controllers.TodoController.GetById (TodoApi)
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action method TodoApi.Controllers.TodoController.GetById (TodoApi), returned result Microsoft.AspNetCore.Mvc.NotFoundResult.
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 0.8788ms
dbug: Microsoft.AspNetCore.Server.Kestrel[9]
      Connection id "0HL6L7NEFF2QD" completed keep alive response.
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 2.7286ms 404
```

## <a name="log-event-id"></a><span data-ttu-id="9f3b0-204">Protokollereignis-ID</span><span class="sxs-lookup"><span data-stu-id="9f3b0-204">Log event ID</span></span>

<span data-ttu-id="9f3b0-205">Jedes Mal, wenn Sie ein Protokoll schreiben, geben Sie eine *Ereignis-ID* an.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-205">Each time you write a log, you can specify an *event ID*.</span></span> <span data-ttu-id="9f3b0-206">Die Beispiel-App verwendet hierzu eine lokal definierte `LoggingEvents`-Klasse:</span><span class="sxs-lookup"><span data-stu-id="9f3b0-206">The sample app does this by using a locally-defined `LoggingEvents` class:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

<span data-ttu-id="9f3b0-207">Eine Ereignis-ID ist ein ganzzahliger Wert, mit dessen Hilfe Sie eine Reihe von protokollierten Ereignissen einander zuordnen können.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-207">An event ID is an integer value that you can use to associate a set of logged events with one another.</span></span> <span data-ttu-id="9f3b0-208">Beispielsweise könnte ein Protokoll zum Hinzufügen eines Artikels zu einem Warenkorb die Ereignis-ID 1000 und ein Protokoll zum Abschließen eines Kaufvorgangs die Ereignis-ID 1001 aufweisen.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-208">For instance, a log for adding an item to a shopping cart could be event ID 1000 and a log for completing a purchase could be event ID 1001.</span></span>

<span data-ttu-id="9f3b0-209">Bei der Protokollausgabe kann die Ereignis-ID je nach Anbieter in einem Feld gespeichert oder in die Textmeldung aufgenommen werden.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-209">In logging output, the event ID may be stored in a field or included in the text message, depending on the provider.</span></span> <span data-ttu-id="9f3b0-210">Der Debuganbieter zeigt keine Ereignis-IDs an, aber beim Konsolenanbieter werden die IDs in Klammern hinter der Kategorie angezeigt:</span><span class="sxs-lookup"><span data-stu-id="9f3b0-210">The Debug provider doesn't show event IDs, but the console provider shows them in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="9f3b0-211">Protokollmeldungsvorlage</span><span class="sxs-lookup"><span data-stu-id="9f3b0-211">Log message template</span></span>

<span data-ttu-id="9f3b0-212">Jedes Mal, wenn Sie eine Protokollmeldung schreiben, stellen Sie eine Meldungsvorlage bereit.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-212">Each time you write a log message, you provide a message template.</span></span> <span data-ttu-id="9f3b0-213">Die Meldungsvorlage kann eine Zeichenfolge sein oder benannte Platzhalter enthalten, in die Argumentwerte eingefügt werden.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-213">The message template can be a string or it can contain named placeholders into which argument values are placed.</span></span> <span data-ttu-id="9f3b0-214">Die Vorlage ist keine Formatzeichenfolge, und Platzhalter sollten benannt und nicht nummeriert werden.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-214">The template isn't a format string, and placeholders should be named, not numbered.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="9f3b0-215">Die Reihenfolge der Platzhalter – nicht ihre Namen – bestimmt, welche Parameter verwendet werden, um ihre Werte zur Verfügung zu stellen.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-215">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="9f3b0-216">Wenn Ihr Code folgendermaßen lautet:</span><span class="sxs-lookup"><span data-stu-id="9f3b0-216">If you have the following code:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="9f3b0-217">Die angezeigte Protokollmeldung sieht so aus:</span><span class="sxs-lookup"><span data-stu-id="9f3b0-217">The resulting log message looks like this:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="9f3b0-218">Das Protokollierungsframework formatiert Meldungen in dieser Weise, um den Protollierungsanbietern das Implementieren einer [semantischen Protokollierung (auch bezeichnet als strukturierte Protokollierung)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging) zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-218">The logging framework does message formatting in this way to make it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="9f3b0-219">Da nicht nur die formatierte Meldungsvorlage, sondern die Argumente selbst an das Protokollierungssystem übergeben werden, können Protokollierungsanbieter die Parameterwerte zusätzlich zur Nachrichtenvorlage als Felder speichern.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-219">Because the arguments themselves are passed to the logging system, not just the formatted message template, logging providers can store the parameter values as fields in addition to the message template.</span></span> <span data-ttu-id="9f3b0-220">Angenommen, Sie verwenden Azure Table Storage als Ziel für die Protokollausgabe, und Ihr Aufruf der Protokollierungsmethode sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="9f3b0-220">If you're directing your log output to Azure Table Storage and your logger method call looks like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="9f3b0-221">Jede Azure Table Storage-Entität kann `ID`- und `RequestTime`-Eigenschaften aufweisen, wodurch das Abfragen von Protokolldaten vereinfacht wird.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-221">Each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="9f3b0-222">Sie können alle Protokolle innerhalb eines bestimmten `RequestTime`-Bereichs ermitteln, ohne die Uhrzeit aus den Textmeldungen auslesen zu müssen.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-222">You can find all logs within a particular `RequestTime` range without the need to parse the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="9f3b0-223">Protokollieren von Ausnahmen</span><span class="sxs-lookup"><span data-stu-id="9f3b0-223">Logging exceptions</span></span>

<span data-ttu-id="9f3b0-224">Die Protokollierungsmethoden umfassen Überladungen, die das Übergeben von Ausnahmen ermöglichen, wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="9f3b0-224">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

<span data-ttu-id="9f3b0-225">Die verschiedenen Anbieter verarbeiten die Ausnahmeinformationen auf unterschiedliche Weise.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-225">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="9f3b0-226">Hier sehen Sie ein Beispiel einer Debuganbieterausgabe aus dem obigen Codebeispiel.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-226">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="9f3b0-227">Protokollfilterung</span><span class="sxs-lookup"><span data-stu-id="9f3b0-227">Log filtering</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9f3b0-228">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9f3b0-228">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9f3b0-229">Sie können einen Mindestprotokolliergrad für einen bestimmten Anbieter und eine bestimmte Kategorie oder für alle Anbieter oder alle Kategorien festlegen.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-229">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="9f3b0-230">Alle Protokolle unter dem Mindestgrad werden nicht an diesen Anbieter weitergeleitet, sodass sie nicht angezeigt oder gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-230">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span> 

<span data-ttu-id="9f3b0-231">Wenn Sie alle Protokolle unterdrücken möchten, können Sie `LogLevel.None` als Mindestprotokolliergrad angeben.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-231">If you want to suppress all logs, you can specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="9f3b0-232">Der ganzzahlige Wert von `LogLevel.None` lautet 6 und liegt über `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="9f3b0-232">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="9f3b0-233">**Erstellen von Filterregeln in der Konfiguration**</span><span class="sxs-lookup"><span data-stu-id="9f3b0-233">**Create filter rules in configuration**</span></span>

<span data-ttu-id="9f3b0-234">Die Projektvorlagen erstellen Code, der `CreateDefaultBuilder` aufruft, um die Protokollierung für die Konsolen- und Debuganbieter einzurichten.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-234">The project templates create code that calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="9f3b0-235">Die `CreateDefaultBuilder`-Methode richtet die Protokollierung auch ein, um nach der Konfiguration in einem `Logging`-Abschnitt zu suchen. Hierbei wird Code wie der folgende verwendet:</span><span class="sxs-lookup"><span data-stu-id="9f3b0-235">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

<span data-ttu-id="9f3b0-236">Die Konfigurationsdaten geben die Mindestprotokolliergrade nach Anbieter und Kategorie an, wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="9f3b0-236">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/sample2/appsettings.json)]

<span data-ttu-id="9f3b0-237">Diese JSON-Konfiguration erstellt sechs Filterregeln: eine für den Debuganbieter, vier für den Konsolenanbieter und eine für alle Anbieter.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-237">This JSON creates six filter rules, one for the Debug provider, four for the Console provider, and one that applies to all providers.</span></span> <span data-ttu-id="9f3b0-238">Sie werden später sehen, wie beim Erstellen eines `ILogger`-Objekts immer nur eine dieser Regeln für jeden Anbieter ausgewählt wird.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-238">You'll see later how just one of these rules is chosen for each provider when an `ILogger` object is created.</span></span>

<span data-ttu-id="9f3b0-239">**Filterregeln im Code**</span><span class="sxs-lookup"><span data-stu-id="9f3b0-239">**Filter rules in code**</span></span>

<span data-ttu-id="9f3b0-240">Sie können Filterregeln im Code registrieren, wie das folgende Beispiel zeigt:</span><span class="sxs-lookup"><span data-stu-id="9f3b0-240">You can register filter rules in code, as shown in the following example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="9f3b0-241">Das zweite `AddFilter` gibt den Debuganbieter durch Verwendung des zugehörigen Typnamens an.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-241">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="9f3b0-242">Das erste `AddFilter` gilt für alle Anbieter, weil kein Anbietertyp angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-242">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

<span data-ttu-id="9f3b0-243">**Anwendung von Filterregeln**</span><span class="sxs-lookup"><span data-stu-id="9f3b0-243">**How filtering rules are applied**</span></span>

<span data-ttu-id="9f3b0-244">Die Konfigurationsdaten und der in den vorangegangenen Beispielen gezeigte `AddFilter`-Code erzeugen die in der folgenden Tabelle gezeigten Regeln.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-244">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="9f3b0-245">Die ersten sechs Regeln stammen aus dem Konfigurationsbeispiel, die letzten zwei Filter stammen aus dem Codebeispiel.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-245">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="9f3b0-246">Anzahl</span><span class="sxs-lookup"><span data-stu-id="9f3b0-246">Number</span></span> | <span data-ttu-id="9f3b0-247">Anbieter</span><span class="sxs-lookup"><span data-stu-id="9f3b0-247">Provider</span></span>      | <span data-ttu-id="9f3b0-248">Kategorien beginnend mit...</span><span class="sxs-lookup"><span data-stu-id="9f3b0-248">Categories that begin with ...</span></span>          | <span data-ttu-id="9f3b0-249">Mindestprotokolliergrad</span><span class="sxs-lookup"><span data-stu-id="9f3b0-249">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="9f3b0-250">1</span><span class="sxs-lookup"><span data-stu-id="9f3b0-250">1</span></span>      | <span data-ttu-id="9f3b0-251">Debug</span><span class="sxs-lookup"><span data-stu-id="9f3b0-251">Debug</span></span>         | <span data-ttu-id="9f3b0-252">Alle Kategorien</span><span class="sxs-lookup"><span data-stu-id="9f3b0-252">All categories</span></span>                          | <span data-ttu-id="9f3b0-253">Information</span><span class="sxs-lookup"><span data-stu-id="9f3b0-253">Information</span></span>       |
| <span data-ttu-id="9f3b0-254">2</span><span class="sxs-lookup"><span data-stu-id="9f3b0-254">2</span></span>      | <span data-ttu-id="9f3b0-255">Konsole</span><span class="sxs-lookup"><span data-stu-id="9f3b0-255">Console</span></span>       | <span data-ttu-id="9f3b0-256">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="9f3b0-256">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="9f3b0-257">Warnung</span><span class="sxs-lookup"><span data-stu-id="9f3b0-257">Warning</span></span>           |
| <span data-ttu-id="9f3b0-258">3</span><span class="sxs-lookup"><span data-stu-id="9f3b0-258">3</span></span>      | <span data-ttu-id="9f3b0-259">Konsole</span><span class="sxs-lookup"><span data-stu-id="9f3b0-259">Console</span></span>       | <span data-ttu-id="9f3b0-260">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="9f3b0-260">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="9f3b0-261">Debug</span><span class="sxs-lookup"><span data-stu-id="9f3b0-261">Debug</span></span>             |
| <span data-ttu-id="9f3b0-262">4</span><span class="sxs-lookup"><span data-stu-id="9f3b0-262">4</span></span>      | <span data-ttu-id="9f3b0-263">Konsole</span><span class="sxs-lookup"><span data-stu-id="9f3b0-263">Console</span></span>       | <span data-ttu-id="9f3b0-264">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="9f3b0-264">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="9f3b0-265">Fehler</span><span class="sxs-lookup"><span data-stu-id="9f3b0-265">Error</span></span>             |
| <span data-ttu-id="9f3b0-266">5</span><span class="sxs-lookup"><span data-stu-id="9f3b0-266">5</span></span>      | <span data-ttu-id="9f3b0-267">Konsole</span><span class="sxs-lookup"><span data-stu-id="9f3b0-267">Console</span></span>       | <span data-ttu-id="9f3b0-268">Alle Kategorien</span><span class="sxs-lookup"><span data-stu-id="9f3b0-268">All categories</span></span>                          | <span data-ttu-id="9f3b0-269">Information</span><span class="sxs-lookup"><span data-stu-id="9f3b0-269">Information</span></span>       |
| <span data-ttu-id="9f3b0-270">6</span><span class="sxs-lookup"><span data-stu-id="9f3b0-270">6</span></span>      | <span data-ttu-id="9f3b0-271">Alle Anbieter</span><span class="sxs-lookup"><span data-stu-id="9f3b0-271">All providers</span></span> | <span data-ttu-id="9f3b0-272">Alle Kategorien</span><span class="sxs-lookup"><span data-stu-id="9f3b0-272">All categories</span></span>                          | <span data-ttu-id="9f3b0-273">Debug</span><span class="sxs-lookup"><span data-stu-id="9f3b0-273">Debug</span></span>             |
| <span data-ttu-id="9f3b0-274">7</span><span class="sxs-lookup"><span data-stu-id="9f3b0-274">7</span></span>      | <span data-ttu-id="9f3b0-275">Alle Anbieter</span><span class="sxs-lookup"><span data-stu-id="9f3b0-275">All providers</span></span> | <span data-ttu-id="9f3b0-276">System</span><span class="sxs-lookup"><span data-stu-id="9f3b0-276">System</span></span>                                  | <span data-ttu-id="9f3b0-277">Debug</span><span class="sxs-lookup"><span data-stu-id="9f3b0-277">Debug</span></span>             |
| <span data-ttu-id="9f3b0-278">8</span><span class="sxs-lookup"><span data-stu-id="9f3b0-278">8</span></span>      | <span data-ttu-id="9f3b0-279">Debug</span><span class="sxs-lookup"><span data-stu-id="9f3b0-279">Debug</span></span>         | <span data-ttu-id="9f3b0-280">Microsoft</span><span class="sxs-lookup"><span data-stu-id="9f3b0-280">Microsoft</span></span>                               | <span data-ttu-id="9f3b0-281">Ablaufverfolgung</span><span class="sxs-lookup"><span data-stu-id="9f3b0-281">Trace</span></span>             |

<span data-ttu-id="9f3b0-282">Wenn Sie ein `ILogger`-Objekt zum Schreiben von Protokollen erstellen, wählt das `ILoggerFactory`-Objekt eine einzige Regel pro Anbieter aus, die auf diese Protokollierung angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-282">When you create an `ILogger` object to write logs with, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="9f3b0-283">Alle über dieses `ILogger`-Objekt geschriebenen Meldungen werden auf Grundlage der ausgewählten Regeln gefiltert.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-283">All messages written by that `ILogger` object are filtered based on the selected rules.</span></span> <span data-ttu-id="9f3b0-284">Aus den verfügbaren Regeln wird die für jeden Anbieter und jedes Kategoriepaar spezifischste Regel ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-284">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="9f3b0-285">Beim Erstellen von `ILogger` für eine vorgegebene Kategorie wird für jeden Anbieter der folgende Algorithmus verwendet:</span><span class="sxs-lookup"><span data-stu-id="9f3b0-285">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="9f3b0-286">Es werden alle Regeln ausgewählt, die dem Anbieter oder dem zugehörigen Alias entsprechen.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-286">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="9f3b0-287">Falls keine solchen Regeln vorhanden sind, werden alle Regeln mit einem leeren Anbieter ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-287">If none are found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="9f3b0-288">Aus dem Ergebnis des vorherigen Schritts werden die Regeln mit dem längsten übereinstimmenden Kategoriepräfix ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-288">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="9f3b0-289">Falls keine solchen Regeln vorhanden sind, werden alle Regeln ohne Angabe einer Kategorie ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-289">If none are found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="9f3b0-290">Wenn mehrere Regeln ausgewählt sind, wird die **letzte** Regel verwendet.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-290">If multiple rules are selected take the **last** one.</span></span>
* <span data-ttu-id="9f3b0-291">Wenn keine Regel ausgewählt ist, wird `MinimumLevel` verwendet.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-291">If no rules are selected, use `MinimumLevel`.</span></span>
 
<span data-ttu-id="9f3b0-292">Angenommen, Sie verfügen über die vorherige Liste mit Regeln und erstellen ein `ILogger`-Objekt für die Kategorie „Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine“:</span><span class="sxs-lookup"><span data-stu-id="9f3b0-292">For example, suppose you have the preceding list of rules and you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="9f3b0-293">Für den Debuganbieter gelten die Regeln 1, 6 und 8.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-293">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="9f3b0-294">Regel 8 ist die spezifischste Regel, deshalb wird diese Regel ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-294">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="9f3b0-295">Für den Konsolenanbieter gelten die Regeln 3, 4, 5 und 6.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-295">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="9f3b0-296">Regel 3 ist die spezifischste Regel.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-296">Rule 3 is most specific.</span></span>

<span data-ttu-id="9f3b0-297">Wenn Sie mit `ILogger` Protokolle für die Kategorie „Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine“ erstellen, werden Protokolle der Ebene `Trace` und höher an den Debuganbieter ausgegeben, und Protokolle der Ebene `Debug` und höher gehen an den Konsolenanbieter.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-297">When you create logs with an `ILogger` for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", logs of `Trace` level and above will go to the Debug provider, and logs of `Debug` level and above will go to the Console provider.</span></span>

<span data-ttu-id="9f3b0-298">**Anbieteraliase**</span><span class="sxs-lookup"><span data-stu-id="9f3b0-298">**Provider aliases**</span></span>

<span data-ttu-id="9f3b0-299">Sie können den Typnamen verwenden, um einen Anbieter in der Konfiguration anzugeben, aber jeder Anbieter definiert einen kürzeren *Alias*, der einfacher zu verwenden ist.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-299">You can use the type name to specify a provider in configuration, but each provider defines a shorter *alias* that is easier to use.</span></span> <span data-ttu-id="9f3b0-300">Verwenden Sie für die integrierten Anbieter die folgenden Aliase:</span><span class="sxs-lookup"><span data-stu-id="9f3b0-300">For the built-in providers, use the following aliases:</span></span>

- <span data-ttu-id="9f3b0-301">Konsole</span><span class="sxs-lookup"><span data-stu-id="9f3b0-301">Console</span></span>
- <span data-ttu-id="9f3b0-302">Debug</span><span class="sxs-lookup"><span data-stu-id="9f3b0-302">Debug</span></span>
- <span data-ttu-id="9f3b0-303">EventLog</span><span class="sxs-lookup"><span data-stu-id="9f3b0-303">EventLog</span></span>
- <span data-ttu-id="9f3b0-304">AzureAppServices</span><span class="sxs-lookup"><span data-stu-id="9f3b0-304">AzureAppServices</span></span>
- <span data-ttu-id="9f3b0-305">TraceSource</span><span class="sxs-lookup"><span data-stu-id="9f3b0-305">TraceSource</span></span>
- <span data-ttu-id="9f3b0-306">EventSource</span><span class="sxs-lookup"><span data-stu-id="9f3b0-306">EventSource</span></span>

<span data-ttu-id="9f3b0-307">**Standardmäßiger Mindestprotokolliergrad**</span><span class="sxs-lookup"><span data-stu-id="9f3b0-307">**Default minimum level**</span></span>

<span data-ttu-id="9f3b0-308">Es gibt eine Einstellung für den Mindestprotokolliergrad, die nur dann wirksam wird, wenn für einen bestimmten Anbieter und eine bestimmte Kategorie keine Regeln aus Konfiguration oder Code gelten.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-308">There is a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="9f3b0-309">Im folgenden Beispiel wird das Festlegen des Mindestprotokolliergrads veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="9f3b0-309">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="9f3b0-310">Wenn Sie den Mindestprotokolliergrad nicht explizit festlegen, lautet der Standardwert `Information`, d.h. Protokolle der Ebene `Trace` und `Debug` werden ignoriert.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-310">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

<span data-ttu-id="9f3b0-311">**Filterfunktionen**</span><span class="sxs-lookup"><span data-stu-id="9f3b0-311">**Filter functions**</span></span>

<span data-ttu-id="9f3b0-312">Sie können Code in einer Filterfunktion schreiben, um Filterregeln anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-312">You can write code in a filter function to apply filtering rules.</span></span> <span data-ttu-id="9f3b0-313">Eine Filterfunktion wird für alle Anbieter und Kategorien aufgerufen, denen keine Regeln durch Konfiguration oder Code zugewiesen sind.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-313">A filter function is invoked for all providers and categories that do not have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="9f3b0-314">Über den Code in der Funktion kann auf Anbietertyp, Kategorie und Protokollebene zugegriffen werden. So kann entschieden werden, ob eine Meldung protokolliert werden soll oder nicht.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-314">Code in the function has access to the provider type, category, and log level to decide whether or not a message should be logged.</span></span> <span data-ttu-id="9f3b0-315">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="9f3b0-315">For example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9f3b0-316">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9f3b0-316">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9f3b0-317">Bei einigen Protokollierungsanbietern können Sie angeben, wann Protokolle in ein Speichermedium geschrieben oder ignoriert werden sollen – je nach Protokolliergrad und Kategorie.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-317">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="9f3b0-318">Die Erweiterungsmethoden `AddConsole` und `AddDebug` bieten Überladungen, mit denen Filterkriterien übergeben werden können.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-318">The `AddConsole` and `AddDebug` extension methods provide overloads that let you pass in filtering criteria.</span></span> <span data-ttu-id="9f3b0-319">Der folgende Beispielcode veranlasst den Konsolenanbieter, Protokolle unterhalb der Ebene `Warning` zu ignorieren, während der Debuganbieter vom Framework erstellte Protokolle ignoriert.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-319">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="9f3b0-320">Die Methode `AddEventLog` umfasst eine Überladung mit einer `EventLogSettings`-Instanz, die eine Filterfunktion in ihrer `Filter`-Eigenschaft enthalten kann.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-320">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="9f3b0-321">Der TraceSource-Anbieter stellt keine dieser Überladungen zur Verfügung, da der zugehörige Protokolliergrad und weitere Parameter auf der Verwendung von `SourceSwitch` und `TraceListener` basieren.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-321">The TraceSource provider does not provide any of those overloads, since its logging level and other parameters are based on the `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="9f3b0-322">Sie können Filterregeln für alle bei einer `ILoggerFactory`-Instanz registrierten Anbieter festlegen, indem Sie die Erweiterungsmethode `WithFilter` verwenden.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-322">You can set filtering rules for all providers that are registered with an `ILoggerFactory` instance by using the `WithFilter` extension method.</span></span> <span data-ttu-id="9f3b0-323">Das nachstehende Beispiel begrenzt Frameworkprotokolle (Kategorie beginnt mit „Microsoft“ oder „System“) auf Warnungen, während für App-Protokolle die Debugebene verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-323">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while letting the app log at debug level.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="9f3b0-324">Wenn Sie die Filterung verwenden, um zu verhindern, dass alle Protokolle für eine bestimmte Kategorie geschrieben werden, können Sie `LogLevel.None` als Mindestprotokolliergrad für diese Kategorie angeben.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-324">If you want to use filtering to prevent all logs from being written for a particular category, you can specify `LogLevel.None` as the minimum log level for that category.</span></span> <span data-ttu-id="9f3b0-325">Der ganzzahlige Wert von `LogLevel.None` lautet 6 und liegt über `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="9f3b0-325">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="9f3b0-326">Die Erweiterungsmethode `WithFilter` wird vom NuGet-Paket [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-326">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="9f3b0-327">Die Methode gibt eine neue `ILoggerFactory`-Instanz zurück, die Protokollmeldungen für alle bei ihr registrierten Protokollierungsanbieter filtert.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-327">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="9f3b0-328">Weitere `ILoggerFactory`-Instanzen, die ursprüngliche `ILoggerFactory`-Instanz eingeschlossen, sind nicht betroffen.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-328">It does not affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

---

## <a name="log-scopes"></a><span data-ttu-id="9f3b0-329">Protokollbereiche</span><span class="sxs-lookup"><span data-stu-id="9f3b0-329">Log scopes</span></span>

<span data-ttu-id="9f3b0-330">Sie können einen Satz an logischen Operationen in einem *Bereich* gruppieren, um an jedes der für diesen Satz erstellten Protokolle dieselben Daten anzuhängen.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-330">You can group a set of logical operations within a *scope* in order to attach the same data to each log that is created as part of that set.</span></span> <span data-ttu-id="9f3b0-331">Beispielsweise kann es sinnvoll sein, dass jedes im Rahmen der Verarbeitung einer Transaktion erstellte Protokoll die Transaktions-ID enthält.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-331">For example, you might want every log created as part of processing a transaction to include the transaction ID.</span></span>

<span data-ttu-id="9f3b0-332">Ein Bereich ist ein `IDisposable`-Typ, der von der `ILogger.BeginScope<TState>`-Methode zurückgegeben und so lange beibehalten wird, bis er verworfen wird.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-332">A scope is an `IDisposable` type that is returned by the `ILogger.BeginScope<TState>` method and lasts until it is disposed.</span></span> <span data-ttu-id="9f3b0-333">Sie verwenden einen Bereich, indem Sie Ihre Protokollierungsaufrufe mit einem `using`-Block umschließen, wie hier gezeigt:</span><span class="sxs-lookup"><span data-stu-id="9f3b0-333">You use a scope by wrapping your logger calls in a `using` block, as shown here:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="9f3b0-334">Der folgende Code aktiviert Bereiche für den Konsolenanbieter:</span><span class="sxs-lookup"><span data-stu-id="9f3b0-334">The following code enables scopes for the console provider:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9f3b0-335">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9f3b0-335">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9f3b0-336">In *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="9f3b0-336">In *Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="9f3b0-337">Um die bereichsbasierte Protokollierung zu aktivieren, muss die Konsolenprotokollierungsoption `IncludeScopes` konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-337">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span> <span data-ttu-id="9f3b0-338">Die Konfiguration von `IncludeScopes` mithilfe von *appsettings*-Konfigurationsdateien wird mit der Veröffentlichung von ASP.NET Core 2.1 zur Verfügung stehen.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-338">Configuration of `IncludeScopes` using *appsettings* configuration files will be available with the release of ASP.NET Core 2.1.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9f3b0-339">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9f3b0-339">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9f3b0-340">In *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="9f3b0-340">In *Startup.cs*:</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

---

<span data-ttu-id="9f3b0-341">Jede Protokollmeldung enthält die bereichsbezogenen Informationen:</span><span class="sxs-lookup"><span data-stu-id="9f3b0-341">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="9f3b0-342">Integrierte Protokollierungsanbieter</span><span class="sxs-lookup"><span data-stu-id="9f3b0-342">Built-in logging providers</span></span>

<span data-ttu-id="9f3b0-343">ASP.NET Core wird mit den folgenden Anbietern bereitgestellt:</span><span class="sxs-lookup"><span data-stu-id="9f3b0-343">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="9f3b0-344">Konsole</span><span class="sxs-lookup"><span data-stu-id="9f3b0-344">Console</span></span>](#console)
* [<span data-ttu-id="9f3b0-345">Debuggen</span><span class="sxs-lookup"><span data-stu-id="9f3b0-345">Debug</span></span>](#debug)
* [<span data-ttu-id="9f3b0-346">EventSource</span><span class="sxs-lookup"><span data-stu-id="9f3b0-346">EventSource</span></span>](#eventsource)
* [<span data-ttu-id="9f3b0-347">EventLog</span><span class="sxs-lookup"><span data-stu-id="9f3b0-347">EventLog</span></span>](#eventlog)
* [<span data-ttu-id="9f3b0-348">TraceSource</span><span class="sxs-lookup"><span data-stu-id="9f3b0-348">TraceSource</span></span>](#tracesource)
* [<span data-ttu-id="9f3b0-349">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="9f3b0-349">Azure App Service</span></span>](#appservice)

<a id="console"></a>
### <a name="the-console-provider"></a><span data-ttu-id="9f3b0-350">Der Konsolenanbieter</span><span class="sxs-lookup"><span data-stu-id="9f3b0-350">The console provider</span></span>

<span data-ttu-id="9f3b0-351">Das Anbieterpaket [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) sendet eine Protokollausgabe an die Konsole.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-351">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9f3b0-352">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9f3b0-352">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddConsole()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9f3b0-353">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9f3b0-353">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddConsole()
```

<span data-ttu-id="9f3b0-354">Mithilfe von [AddConsole-Überladungen](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.consoleloggerextensions) können Sie einen Mindestprotokolliergrade, eine Filterfunktion und einen booleschen Wert übergeben, der angibt, welche Bereiche unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-354">[AddConsole overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.consoleloggerextensions) let you pass in an a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span> <span data-ttu-id="9f3b0-355">Darüber hinaus kann ein `IConfiguration`-Objekt übergeben werden, mit dem Bereichsunterstützung und Protokolliergrade angegeben werden können.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-355">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span> 

<span data-ttu-id="9f3b0-356">Wenn Sie erwägen, den Konsolenanbieter in der Produktion einzusetzen, sollten Sie sich darüber im Klaren sein, dass er einen erheblichen Einfluss auf die Leistung hat.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-356">If you are considering the console provider for use in production, be aware that it has a significant impact on performance.</span></span>

<span data-ttu-id="9f3b0-357">Wenn Sie ein neues Projekt in Visual Studio erstellen, sieht die `AddConsole`-Methode folgendermaßen aus:</span><span class="sxs-lookup"><span data-stu-id="9f3b0-357">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="9f3b0-358">Dieser Code verweist auf den `Logging`-Abschnitt der Datei *appSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="9f3b0-358">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](index/sample//appsettings.json)]

<span data-ttu-id="9f3b0-359">Die gezeigten Einstellungen schränken die Frameworkprotokolle auf Warnungen ein, während die App eine Protokollierung auf Debugebene durchführt, wie im Abschnitt [Protokollfilterung](#log-filtering) erläutert.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-359">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="9f3b0-360">Weitere Informationen finden Sie unter [Konfiguration](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="9f3b0-360">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

---

<a id="debug"></a>
### <a name="the-debug-provider"></a><span data-ttu-id="9f3b0-361">Der Debuganbieter</span><span class="sxs-lookup"><span data-stu-id="9f3b0-361">The Debug provider</span></span>

<span data-ttu-id="9f3b0-362">Beim Anbieterpaket [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) erfolgt die Protokollausgabe unter Verwendung der Klasse [System.Diagnostics.Debug](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.debug#System_Diagnostics_Debug) (`Debug.WriteLine`-Methodenaufrufe).</span><span class="sxs-lookup"><span data-stu-id="9f3b0-362">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.debug#System_Diagnostics_Debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="9f3b0-363">Unter Linux werden Protokolle dieses Anbieters in */var/log/message* geschrieben.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-363">On Linux, this provider writes logs to */var/log/message*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9f3b0-364">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9f3b0-364">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddDebug()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9f3b0-365">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9f3b0-365">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddDebug()
```

<span data-ttu-id="9f3b0-366">Mithilfe von [AddDebug-Überladungen](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.debugloggerfactoryextensions) können Sie einen Mindestprotokolliergrad oder eine Filterfunktion übergeben.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-366">[AddDebug overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.debugloggerfactoryextensions) let you pass in a minimum log level or a filter function.</span></span>

---

<a id="eventsource"></a>
### <a name="the-eventsource-provider"></a><span data-ttu-id="9f3b0-367">Der EventSource-Anbieter</span><span class="sxs-lookup"><span data-stu-id="9f3b0-367">The EventSource provider</span></span>

<span data-ttu-id="9f3b0-368">Für Apps, die für ASP.NET Core 1.1.0 oder höher konzipiert sind, kann mit dem Anbieterpaket [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) eine Ereignisablaufverfolgung implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-368">For apps that target ASP.NET Core 1.1.0 or higher, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="9f3b0-369">Verwenden Sie unter Windows [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="9f3b0-369">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="9f3b0-370">Der Anbieter ist plattformunabhängig, aber für Linux oder macOS sind Ereignissammlung und Anzeigetools noch nicht verfügbar.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-370">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9f3b0-371">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9f3b0-371">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddEventSourceLogger()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9f3b0-372">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9f3b0-372">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventSourceLogger()
```

---

<span data-ttu-id="9f3b0-373">Eine gute Möglichkeit zum Erfassen und Anzeigen von Protokollen ist die Verwendung des Hilfsprogramms [PerfView](https://www.microsoft.com/download/details.aspx?id=28567).</span><span class="sxs-lookup"><span data-stu-id="9f3b0-373">A good way to collect and view logs is to use the [PerfView utility](https://www.microsoft.com/download/details.aspx?id=28567).</span></span> <span data-ttu-id="9f3b0-374">Es gibt andere Tools zur Anzeige von ETW-Protokollen, aber PerfView bietet die besten Ergebnisse bei der Arbeit mit ETW-Ereignissen, die von ASP.NET ausgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-374">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span> 

<span data-ttu-id="9f3b0-375">Um PerfView für das Erfassen von Ereignissen zu konfigurieren, die von diesem Anbieter protokolliert wurden, fügen Sie die Zeichenfolge `*Microsoft-Extensions-Logging` zur Liste **Zusätzliche Anbieter** hinzu.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-375">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="9f3b0-376">(Vergessen Sie nicht das Sternchen am Anfang der Zeichenfolge.)</span><span class="sxs-lookup"><span data-stu-id="9f3b0-376">(Don't miss the asterisk at the start of the string.)</span></span>

![Zusätzliche PerfView-Anbieter](index/_static/perfview-additional-providers.png)

<span data-ttu-id="9f3b0-378">Das Erfassen von Ereignissen unter Nano Server erfordert eine zusätzliche Einrichtung:</span><span class="sxs-lookup"><span data-stu-id="9f3b0-378">Capturing events on Nano Server requires some additional setup:</span></span>

* <span data-ttu-id="9f3b0-379">Stellen Sie eine PowerShell-Remotingverbindung mit der Nano Server-Instanz her:</span><span class="sxs-lookup"><span data-stu-id="9f3b0-379">Connect PowerShell remoting to the Nano Server:</span></span>

  ```powershell
  Enter-PSSession [name]
  ```

* <span data-ttu-id="9f3b0-380">Erstellen Sie eine ETW-Sitzung:</span><span class="sxs-lookup"><span data-stu-id="9f3b0-380">Create an ETW session:</span></span>

  ```powershell
  New-EtwTraceSession -Name "MyAppTrace" -LocalFilePath C:\trace.etl
  ```

* <span data-ttu-id="9f3b0-381">Fügen Sie ETW-Anbieter für [CLR](https://docs.microsoft.com/dotnet/framework/performance/clr-etw-providers), ASP.NET Core und (nach Bedarf) weitere hinzu.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-381">Add ETW providers for [CLR](https://docs.microsoft.com/dotnet/framework/performance/clr-etw-providers), ASP.NET Core, and others as needed.</span></span> <span data-ttu-id="9f3b0-382">Die ASP.NET Core-Anbieter-GUID lautet `3ac73b97-af73-50e9-0822-5da4367920d0`.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-382">The ASP.NET Core provider GUID is `3ac73b97-af73-50e9-0822-5da4367920d0`.</span></span> 

  ```powershell
  Add-EtwTraceProvider -Guid "{e13c0d23-ccbc-4e12-931b-d9cc2eee27e4}" -SessionName MyAppTrace
  Add-EtwTraceProvider -Guid "{3ac73b97-af73-50e9-0822-5da4367920d0}" -SessionName MyAppTrace
  ```

* <span data-ttu-id="9f3b0-383">Führen Sie die Website und alle Aktionen aus, für Sie die Ablaufverfolgungsinformationen benötigen.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-383">Run the site and do whatever actions you want tracing information for.</span></span>

* <span data-ttu-id="9f3b0-384">Beenden Sie die Ablaufverfolgungssitzung, wenn Sie fertig sind:</span><span class="sxs-lookup"><span data-stu-id="9f3b0-384">Stop the tracing session when you're finished:</span></span>

  ```powershell
  Stop-EtwTraceSession -Name "MyAppTrace"
  ```

<span data-ttu-id="9f3b0-385">Die Ergebnisdatei *C:\trace.etl* kann mit PerfView wie auf anderen Editionen von Windows analysiert werden.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-385">The resulting *C:\trace.etl* file can be analyzed with PerfView as on other editions of Windows.</span></span>

<a id="eventlog"></a>
### <a name="the-windows-eventlog-provider"></a><span data-ttu-id="9f3b0-386">Der EventLog-Anbieter von Windows</span><span class="sxs-lookup"><span data-stu-id="9f3b0-386">The Windows EventLog provider</span></span>

<span data-ttu-id="9f3b0-387">Das Anbieterpaket [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) sendet eine Protokollausgabe in das Windows-Ereignisprotokoll.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-387">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9f3b0-388">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9f3b0-388">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddEventLog()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9f3b0-389">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9f3b0-389">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventLog()
```

<span data-ttu-id="9f3b0-390">Mithilfe von [AddEventLog-Überladungen](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.eventloggerfactoryextensions) können Sie `EventLogSettings` oder einen Mindestprotokolliergrad übergeben.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-390">[AddEventLog overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.eventloggerfactoryextensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

---

<a id="tracesource"></a>
### <a name="the-tracesource-provider"></a><span data-ttu-id="9f3b0-391">Der TraceSource-Anbieter</span><span class="sxs-lookup"><span data-stu-id="9f3b0-391">The TraceSource provider</span></span>

<span data-ttu-id="9f3b0-392">Das Anbieterpaket [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) verwendet die [System.Diagnostics.TraceSource](https://docs.microsoft.com/dotnet/api/system.diagnostics.tracesource)-Bibliotheken und -Anbieter.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-392">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the [System.Diagnostics.TraceSource](https://docs.microsoft.com/dotnet/api/system.diagnostics.tracesource) libraries and providers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9f3b0-393">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9f3b0-393">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddTraceSource(sourceSwitchName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9f3b0-394">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9f3b0-394">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

---

<span data-ttu-id="9f3b0-395">Mithilfe von [AddTraceSource-Überladungen](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.tracesourcefactoryextensions) können Sie eine Quelloption und einen Listener für die Ablaufverfolgung übergeben.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-395">[AddTraceSource overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.tracesourcefactoryextensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="9f3b0-396">Um diesen Anbieter zu verwenden, muss eine Anwendung (anstelle von .NET Core) unter .NET Framework ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-396">To use this provider, an application has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="9f3b0-397">Mit dem Anbieter können Sie Meldungen an verschiedenste [Listener](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/trace-listeners) weiterleiten, z.B. an den [TextWriterTraceListener](https://docs.microsoft.com/dotnet/api/system.diagnostics.textwritertracelistenerr), der in der Beispielanwendung verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-397">The provider lets you route messages to a variety of [listeners](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/trace-listeners), such as the [TextWriterTraceListener](https://docs.microsoft.com/dotnet/api/system.diagnostics.textwritertracelistenerr) used in the sample application.</span></span>

<span data-ttu-id="9f3b0-398">Im folgenden Beispiel wird ein `TraceSource`-Anbieter konfiguriert, der Protokollmeldungen der Ebene `Warning` und höher an das Konsolenfenster ausgibt.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-398">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

<a id="appservice"></a>
### <a name="the-azure-app-service-provider"></a><span data-ttu-id="9f3b0-399">Der Azure App Service-Anbieter</span><span class="sxs-lookup"><span data-stu-id="9f3b0-399">The Azure App Service provider</span></span>

<span data-ttu-id="9f3b0-400">Das Anbieterpaket [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) schreibt Protokolle in Textdateien in das Dateisystem einer Azure App Service-App und in [Blob Storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in einem Azure Storage-Konto.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-400">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="9f3b0-401">Der Anbieter ist nur für Apps verfügbar, die für ASP.NET Core 1.1.0 oder höher konzipiert sind.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-401">The provider is available only for apps that target ASP.NET Core 1.1.0 or higher.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9f3b0-402">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9f3b0-402">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9f3b0-403">Wenn Sie Anwendungen für .NET Core entwickeln, müssen Sie weder das Anbieterpaket installieren noch `AddAzureWebAppDiagnostics` explizit aufrufen.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-403">If targeting .NET Core, you don't have to install the provider package or explicitly call `AddAzureWebAppDiagnostics`.</span></span> <span data-ttu-id="9f3b0-404">Der Anbieter steht automatisch für Ihre App zur Verfügung, wenn sie die App in Azure App Service bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-404">The provider is automatically available to your app when you deploy the app to Azure App Service.</span></span>

<span data-ttu-id="9f3b0-405">Wenn Sie Anwendungen für .NET Framework entwickeln, fügen Sie das Anbieterpaket Ihrem Projekt hinzu, und rufen Sie `AddAzureWebAppDiagnostics` auf:</span><span class="sxs-lookup"><span data-stu-id="9f3b0-405">If targeting .NET Framework, add the provider package to your project and invoke `AddAzureWebAppDiagnostics`:</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9f3b0-406">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9f3b0-406">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

<span data-ttu-id="9f3b0-407">Mithilfe einer `AddAzureWebAppDiagnostics`-Überladung können Sie [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs) übergeben, mit denen Sie Standardeinstellungen wie z.B. die Vorlage für die Protokollierungsausgabe, den Blobnamen und die Dateigrößenbeschränkung überschreiben können.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-407">An `AddAzureWebAppDiagnostics` overload lets you pass in [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs) with which you can override default settings such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="9f3b0-408">(Eine *Ausgabevorlage* ist eine Meldungsvorlage, die zusätzlich zu dem Protokoll, das Sie beim Aufruf einer `ILogger`-Methode angeben, auf alle Protokolle angewendet wird.)</span><span class="sxs-lookup"><span data-stu-id="9f3b0-408">(*Output template* is a message template that's applied to all logs on top of the one that you provide when you call an `ILogger` method.)</span></span>

---

<span data-ttu-id="9f3b0-409">Wenn Sie eine Bereitstellung in einer App Service-App durchführen, berücksichtigt Ihre Anwendung die Einstellungen im Abschnitt [Diagnoseprotokolle](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) der Seite **App Service** im Azure-Portal.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-409">When you deploy to an App Service app, your application honors the settings in the [Diagnostic Logs](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="9f3b0-410">Wenn Sie diese Einstellungen ändern, treten die Änderungen sofort in Kraft, ein Neustart der App oder eine erneute Bereitstellung des Codes ist nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-410">When you change those settings, the changes take effect immediately without requiring that you restart the app or redeploy code to it.</span></span> 

![Einstellungen für die Azure-Protokollierung](index/_static/azure-logging-settings.png)

<span data-ttu-id="9f3b0-412">Der Standardspeicherort für Protokolldateien ist der Ordner *D:\\home\\LogFiles\\Application*, und der standardmäßige Dateiname lautet *diagnostics-yyyymmdd.txt*.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-412">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="9f3b0-413">Die Dateigröße ist standardmäßig auf 10 MB beschränkt, und die maximal zulässige Anzahl beibehaltener Dateien lautet 2.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-413">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="9f3b0-414">Der Standardblobname lautet *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-414">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span> <span data-ttu-id="9f3b0-415">Weitere Informationen zum Standardverhalten finden Sie unter [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs).</span><span class="sxs-lookup"><span data-stu-id="9f3b0-415">For more information about default behavior, see [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs).</span></span>

<span data-ttu-id="9f3b0-416">Der Anbieter funktioniert nur, wenn Ihr Projekt in der Azure-Umgebung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-416">The provider only works when your project runs in the Azure environment.</span></span> <span data-ttu-id="9f3b0-417">Bei einer lokalen Ausführung zeigt er keine Wirkung – es werden keine Protokolle in lokale Dateien oder lokalen Entwicklungsspeicher für Blobs geschrieben.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-417">It has no effect when you run locally &mdash; it does not write to local files or local development storage for blobs.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="9f3b0-418">Protokollierungsanbieter von Drittanbietern</span><span class="sxs-lookup"><span data-stu-id="9f3b0-418">Third-party logging providers</span></span>

<span data-ttu-id="9f3b0-419">Nachfolgend werden einige Protokollierungsframeworks von Drittanbietern aufgeführt, die mit ASP.NET Core funktionieren:</span><span class="sxs-lookup"><span data-stu-id="9f3b0-419">Here are some third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="9f3b0-420">[elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging): Anbieter für den Elmah.lo-Dienst</span><span class="sxs-lookup"><span data-stu-id="9f3b0-420">[elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging) - provider for the Elmah.Io service</span></span>

* <span data-ttu-id="9f3b0-421">[JSNLog](http://jsnlog.com): Protokolliert JavaScript-Ausnahmen und andere clientseitige Ereignisse in Ihrem serverseitigen Protokoll.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-421">[JSNLog](http://jsnlog.com) - logs JavaScript exceptions and other client-side events in your server-side log.</span></span>

* <span data-ttu-id="9f3b0-422">[Loggr](https://github.com/imobile3/Loggr.Extensions.Logging): Anbieter für den Loggr-Dienst</span><span class="sxs-lookup"><span data-stu-id="9f3b0-422">[Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) - provider for the Loggr service</span></span>

* <span data-ttu-id="9f3b0-423">[NLog](https://github.com/NLog/NLog.Extensions.Logging): Anbieter für den NLog-Dienst</span><span class="sxs-lookup"><span data-stu-id="9f3b0-423">[NLog](https://github.com/NLog/NLog.Extensions.Logging) - provider for the NLog library</span></span>

* <span data-ttu-id="9f3b0-424">[Serilog](https://github.com/serilog/serilog-extensions-logging): Anbieter für den Serilog-Dienst</span><span class="sxs-lookup"><span data-stu-id="9f3b0-424">[Serilog](https://github.com/serilog/serilog-extensions-logging) - provider for the Serilog library</span></span>

<span data-ttu-id="9f3b0-425">Einige Drittanbieterframeworks unterstützen eine [semantische Protokollierung (auch als strukturierte Protokollierung bezeichnet)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="9f3b0-425">Some third-party frameworks can do [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="9f3b0-426">Die Verwendung eines Drittanbieterframeworks ist vergleichbar mit der Verwendung eines integrierten Anbieters: Fügen Sie Ihrem Projekt ein NuGet-Paket hinzu, und rufen Sie eine Erweiterungsmethode für `ILoggerFactory` auf.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-426">Using a third-party framework is similar to using one of the built-in providers: add a NuGet package to your project and call an extension method on `ILoggerFactory`.</span></span> <span data-ttu-id="9f3b0-427">Weitere Informationen finden Sie in der Dokumentation zum jeweiligen Framework.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-427">For more information, see each framework's documentation.</span></span>

<span data-ttu-id="9f3b0-428">Sie können auch eigene, benutzerdefinierte Anbieter erstellen, um weitere Protokollierungsframeworks oder Ihre eigenen Protokollierungsanforderungen zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-428">You can create your own custom providers as well, to support other logging frameworks or your own logging requirements.</span></span>

## <a name="azure-log-streaming"></a><span data-ttu-id="9f3b0-429">Azure-Protokollstreaming</span><span class="sxs-lookup"><span data-stu-id="9f3b0-429">Azure log streaming</span></span>

<span data-ttu-id="9f3b0-430">Das Azure-Protokollstreaming ermöglicht Ihnen eine Echtzeitanzeige der Protokollaktivität für:</span><span class="sxs-lookup"><span data-stu-id="9f3b0-430">Azure log streaming enables you to view log activity in real time from:</span></span> 

* <span data-ttu-id="9f3b0-431">Anwendungsserver</span><span class="sxs-lookup"><span data-stu-id="9f3b0-431">The application server</span></span> 
* <span data-ttu-id="9f3b0-432">Webserver</span><span class="sxs-lookup"><span data-stu-id="9f3b0-432">The web server</span></span>
* <span data-ttu-id="9f3b0-433">Ablaufverfolgung für Anforderungsfehler</span><span class="sxs-lookup"><span data-stu-id="9f3b0-433">Failed request tracing</span></span> 

<span data-ttu-id="9f3b0-434">So konfigurieren Sie das Azure-Protokollstreaming</span><span class="sxs-lookup"><span data-stu-id="9f3b0-434">To configure Azure log streaming:</span></span> 

* <span data-ttu-id="9f3b0-435">Navigieren Sie von der Portalseite Ihrer Anwendung zur Seite **Diagnoseprotokolle**.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-435">Navigate to the **Diagnostics Logs** page from your application's portal page</span></span>
* <span data-ttu-id="9f3b0-436">Aktivieren Sie **Anwendungsprotokollierung (Dateisystem)**.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-436">Set **Application Logging (Filesystem)** to on.</span></span> 

![Seite „Diagnoseprotokolle“ im Azure-Portal](index/_static/azure-diagnostic-logs.png)

<span data-ttu-id="9f3b0-438">Navigieren Sie zur Seite **Protokollstreaming**, um Anwendungsmeldungen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-438">Navigate to the **Log Streaming** page to view application messages.</span></span> <span data-ttu-id="9f3b0-439">Diese werden von der Anwendung über die `ILogger`-Schnittstelle protokolliert.</span><span class="sxs-lookup"><span data-stu-id="9f3b0-439">They are logged by application through the `ILogger` interface.</span></span> 

![Anwendungsprotokollstreaming im Azure-Portal](index/_static/azure-log-streaming.png)


## <a name="see-also"></a><span data-ttu-id="9f3b0-441">Siehe auch</span><span class="sxs-lookup"><span data-stu-id="9f3b0-441">See also</span></span>

[<span data-ttu-id="9f3b0-442">Hochleistungsprotokollierung mit LoggerMessage</span><span class="sxs-lookup"><span data-stu-id="9f3b0-442">High-performance logging with LoggerMessage</span></span>](xref:fundamentals/logging/loggermessage)
