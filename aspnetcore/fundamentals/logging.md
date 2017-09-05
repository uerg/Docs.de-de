---
title: Protokollierung in ASP.NET Core
author: ardalis
description: "Wird das protokollierungsframework in ASP.NET Core eingeführt. Enthält einen Abschnitt für jede integrierte Protokollierungsanbieter und Links zu einigen beliebten Drittanbieter."
keywords: ASP.NET Core, Protokollierung, Protokollanbieter, Microsoft.Extensions.Logging ILogger iloggerfactory-Standardobjekt zur LogLevel WithFilter TraceSource EventLog, EventSource, Bereiche
ms.author: tdykstra
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: ac27ac68-d76a-4f8e-b8ab-ea045803e5f2
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/logging
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9e5c97ce868e281310aa75c16e73298e2aaa0d9d
ms.sourcegitcommit: bd05f7ea8f87ad076ef6e8b704698ebcba5ca80c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2017
---
# <a name="introduction-to-logging-in-aspnet-core"></a><span data-ttu-id="63e47-105">Einführung in ASP.NET Core anmelden</span><span class="sxs-lookup"><span data-stu-id="63e47-105">Introduction to Logging in ASP.NET Core</span></span>

<span data-ttu-id="63e47-106">Durch [Steve Smith](http://ardalis.com) und [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="63e47-106">By [Steve Smith](http://ardalis.com) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="63e47-107">ASP.NET Core unterstützt eine Protokollierung-API, die mit einer Vielzahl von Protokollanbieter funktioniert.</span><span class="sxs-lookup"><span data-stu-id="63e47-107">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="63e47-108">Integrierte Anbieter können Sie die Protokolle auf einem oder mehreren Zielen senden, und Sie eine Drittanbieter-protokollierungsframework einbinden können.</span><span class="sxs-lookup"><span data-stu-id="63e47-108">Built-in providers let you send logs to one or more destinations, and you can plug in a third-party logging framework.</span></span> <span data-ttu-id="63e47-109">In diesem Artikel zeigt, wie die integrierte Protokollierung-API und Anbieter in Ihrem Code verwenden.</span><span class="sxs-lookup"><span data-stu-id="63e47-109">This article shows how to use the built-in logging API and providers in your code.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="63e47-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="63e47-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[<span data-ttu-id="63e47-111">Anzeigen oder Herunterladen von Beispielcode</span><span class="sxs-lookup"><span data-stu-id="63e47-111">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/sample2)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="63e47-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="63e47-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[<span data-ttu-id="63e47-113">Anzeigen oder Herunterladen von Beispielcode</span><span class="sxs-lookup"><span data-stu-id="63e47-113">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/sample)

---

## <a name="how-to-create-logs"></a><span data-ttu-id="63e47-114">Vorgehensweise: Erstellen von Protokollen</span><span class="sxs-lookup"><span data-stu-id="63e47-114">How to create logs</span></span>

<span data-ttu-id="63e47-115">Um Protokolle zu erstellen, erhalten eine `ILogger` -Objekt aus der [Abhängigkeitsinjektion](dependency-injection.md) Container:</span><span class="sxs-lookup"><span data-stu-id="63e47-115">To create logs, get an `ILogger` object from the [dependency injection](dependency-injection.md) container:</span></span>

[!code-csharp[](logging/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="63e47-116">Rufen Sie anschließend Protokollierungsmethoden für dieses Protokollierungsobjekt:</span><span class="sxs-lookup"><span data-stu-id="63e47-116">Then call logging methods on that logger object:</span></span>

[!code-csharp[](logging/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="63e47-117">In diesem Beispiel wird die Protokolle mit der `TodoController` der Klasse die *Kategorie*.</span><span class="sxs-lookup"><span data-stu-id="63e47-117">This example creates logs with the `TodoController` class as the *category*.</span></span>  <span data-ttu-id="63e47-118">Kategorien werden erläutert [weiter unten in diesem Artikel](#log-category).</span><span class="sxs-lookup"><span data-stu-id="63e47-118">Categories are explained [later in this article](#log-category).</span></span>

<span data-ttu-id="63e47-119">ASP.NET Core bietet asynchrone Methoden Protokollierung keine da Protokollierung so schnell sein sollte, dass es sich lohnt die Kosten für das Verwenden von Async.</span><span class="sxs-lookup"><span data-stu-id="63e47-119">ASP.NET Core does not provide async logger methods because logging should be so fast that it isn't worth the cost of using async.</span></span> <span data-ttu-id="63e47-120">Wenn sind in einer Situation, in denen, nicht "true" ist, erwägen Sie, wie, die Sie sich anmelden.</span><span class="sxs-lookup"><span data-stu-id="63e47-120">If you're in a situation where that's not true, consider changing the way you log.</span></span>  <span data-ttu-id="63e47-121">Datenspeicher langsam ist, Schreiben Sie zuerst die protokollmeldungen in einem schnellen gespeichert, und klicken Sie dann später in einem langsamen Speicher verschieben.</span><span class="sxs-lookup"><span data-stu-id="63e47-121">If your data store is slow, write the log messages to a fast store first, then move them to a slow store later.</span></span> <span data-ttu-id="63e47-122">Melden Sie sich z. B. in eine Warteschlange, die gelesen und von einem anderen Prozess langsam Speicher beibehalten.</span><span class="sxs-lookup"><span data-stu-id="63e47-122">For example, log to a message queue that is read and persisted to slow storage by another process.</span></span>

## <a name="how-to-add-providers"></a><span data-ttu-id="63e47-123">Gewusst wie: Hinzufügen von Anbietern</span><span class="sxs-lookup"><span data-stu-id="63e47-123">How to add providers</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="63e47-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="63e47-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="63e47-125">Ein Protokollierungsanbieter nimmt die Nachrichten, die Sie erstellen ein `ILogger` -Objekt und zeigt an, oder gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="63e47-125">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="63e47-126">Z. B. die Konsole Anbieter angezeigt, in der Konsole, und der Azure App Service-Anbieter können sie im Azure-Blob-Speicher speichern.</span><span class="sxs-lookup"><span data-stu-id="63e47-126">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="63e47-127">Um einen Anbieter zu verwenden, rufen Sie des Anbieters `Add<ProviderName>` Erweiterungsmethode in *"Program.cs"*:</span><span class="sxs-lookup"><span data-stu-id="63e47-127">To use a provider, call the provider's `Add<ProviderName>` extension method in *Program.cs*:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

<span data-ttu-id="63e47-128">Eingerichtet, dass der Standardprojektvorlage Protokollierung die Möglichkeit, die Sie im vorangehenden Code angezeigt, aber die `ConfigureLogging` Aufruf erfolgt durch die `CreateDefaultBuilder` Methode.</span><span class="sxs-lookup"><span data-stu-id="63e47-128">The default project template sets up logging the way you see it in the preceding code, but the `ConfigureLogging` call is done by the `CreateDefaultBuilder` method.</span></span> <span data-ttu-id="63e47-129">Hier ist der Code in *"Program.cs"* werden von Projektvorlagen erstellt:</span><span class="sxs-lookup"><span data-stu-id="63e47-129">Here's the code in *Program.cs* that is created by project templates:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="63e47-130">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="63e47-130">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="63e47-131">Ein Protokollierungsanbieter nimmt die Nachrichten, die Sie erstellen ein `ILogger` -Objekt und zeigt an, oder gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="63e47-131">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="63e47-132">Z. B. die Konsole Anbieter angezeigt, in der Konsole, und der Azure App Service-Anbieter können sie im Azure-Blob-Speicher speichern.</span><span class="sxs-lookup"><span data-stu-id="63e47-132">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="63e47-133">Um einen Anbieter zu verwenden, die NuGet-Paket installieren, und rufen Sie die Anbieter-Erweiterungsmethode in einer Instanz von `ILoggerFactory`, wie im folgenden Beispiel gezeigt.</span><span class="sxs-lookup"><span data-stu-id="63e47-133">To use a provider, install its NuGet package and call the provider's extension method on an instance of `ILoggerFactory`, as shown in the following example.</span></span>

[!code-csharp[](logging/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="63e47-134">ASP.NET Core [Abhängigkeitsinjektion](dependency-injection.md) (DI) bietet die `ILoggerFactory` Instanz.</span><span class="sxs-lookup"><span data-stu-id="63e47-134">ASP.NET Core [dependency injection](dependency-injection.md) (DI) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="63e47-135">Die `AddConsole` und `AddDebug` Erweiterungsmethoden werden definiert, der [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) und [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) Pakete.</span><span class="sxs-lookup"><span data-stu-id="63e47-135">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="63e47-136">Jede Erweiterungsmethode Ruft die `ILoggerFactory.AddProvider` -Methode auf und übergibt eine Instanz des Anbieters.</span><span class="sxs-lookup"><span data-stu-id="63e47-136">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span> 

> [!NOTE]
> <span data-ttu-id="63e47-137">Die beispielanwendung für diesen Artikel fügt Protokollanbieter in der `Configure` Methode der `Startup` Klasse.</span><span class="sxs-lookup"><span data-stu-id="63e47-137">The sample application for this article adds logging providers in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="63e47-138">Wenn Sie möchten Code Ausgabeprotokoll entnommen werden, die früher ausgeführt wird, fügen Sie Protokollanbieter in der `Startup` stattdessen-Klassenkonstruktor.</span><span class="sxs-lookup"><span data-stu-id="63e47-138">If you want to get log output from code that executes earlier, add logging providers in the `Startup` class constructor instead.</span></span> 

---

<span data-ttu-id="63e47-139">Finden Sie Informationen zu den einzelnen [integrierte Protokollierungsanbieter](#built-in-logging-providers) sowie links zu [eines Drittanbieters Protokollanbieter](#third-party-logging-providers) weiter unten in den Artikel.</span><span class="sxs-lookup"><span data-stu-id="63e47-139">You'll find information about each [built-in logging provider](#built-in-logging-providers) and links to [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="63e47-140">Beispiele für Protokollausgaben</span><span class="sxs-lookup"><span data-stu-id="63e47-140">Sample logging output</span></span>

<span data-ttu-id="63e47-141">Durch den Beispielcode, der im vorherigen Abschnitt gezeigt wird Protokolle in der Konsole angezeigt werden, wenn Sie über die Befehlszeile ausführen.</span><span class="sxs-lookup"><span data-stu-id="63e47-141">With the sample code shown in the preceding section, you'll see logs in the console when you run from the command line.</span></span> <span data-ttu-id="63e47-142">Hier ist ein Beispiel der Ausgabe in der Konsole:</span><span class="sxs-lookup"><span data-stu-id="63e47-142">Here's an example of console output:</span></span>

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
 
<span data-ttu-id="63e47-143">Diese Protokolle erstellt wurden, navigieren Sie zu `http://localhost:5000/api/todo/0`, die Ausführung beider löst `ILogger` Aufrufe, die im vorherigen Abschnitt gezeigt.</span><span class="sxs-lookup"><span data-stu-id="63e47-143">These logs were created by going to `http://localhost:5000/api/todo/0`, which triggers execution of both `ILogger` calls shown in the preceding section.</span></span>

<span data-ttu-id="63e47-144">Hier ist ein Beispiel für die gleichen Protokolle, wie sie im Debug-Fenster angezeigt werden, wenn Sie die beispielanwendung in Visual Studio ausführen:</span><span class="sxs-lookup"><span data-stu-id="63e47-144">Here's an example of the same logs as they appear in the Debug window when you run the sample application in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

<span data-ttu-id="63e47-145">Die Protokolle, die erstellt wurden die `ILogger` Aufrufe, die im vorherigen Abschnitt gezeigt, die mit "TodoApi.Controllers.TodoController" beginnen.</span><span class="sxs-lookup"><span data-stu-id="63e47-145">The logs that were created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="63e47-146">Die Protokolle, die mit "Microsoft" Kategorien beginnen, werden von ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="63e47-146">The logs that begin with "Microsoft" categories are from ASP.NET Core.</span></span> <span data-ttu-id="63e47-147">ASP.NET Core selbst und Anwendungscode verwenden die gleiche API für die Protokollierung und die gleichen Protokollanbieter.</span><span class="sxs-lookup"><span data-stu-id="63e47-147">ASP.NET Core itself and your application code are using the same logging API and the same logging providers.</span></span>

<span data-ttu-id="63e47-148">Der Rest dieses Artikels erläutert einige Details und Optionen für die Protokollierung.</span><span class="sxs-lookup"><span data-stu-id="63e47-148">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="63e47-149">NuGet-Pakete</span><span class="sxs-lookup"><span data-stu-id="63e47-149">NuGet packages</span></span>

<span data-ttu-id="63e47-150">Die `ILogger` und `ILoggerFactory` Schnittstellen sind [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), und standardimplementierungen für sie sind im [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/).</span><span class="sxs-lookup"><span data-stu-id="63e47-150">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="63e47-151">Log-Kategorie</span><span class="sxs-lookup"><span data-stu-id="63e47-151">Log category</span></span>

<span data-ttu-id="63e47-152">Ein *Kategorie* ist im Lieferumfang jedes Protokoll, die Sie erstellen.</span><span class="sxs-lookup"><span data-stu-id="63e47-152">A *category* is included with each log that you create.</span></span>  <span data-ttu-id="63e47-153">Beim Erstellen, geben Sie die Kategorie ein `ILogger` Objekt.</span><span class="sxs-lookup"><span data-stu-id="63e47-153">You specify the category when you create an `ILogger` object.</span></span> <span data-ttu-id="63e47-154">Die Kategorie kann eine beliebige Zeichenfolge sein, aber eine Konvention ist die Verwendung der vollqualifizierte Name der Klasse, von dem die Protokolle geschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="63e47-154">The category may be any string, but a convention is to use the fully qualified name of the class from which the logs are written.</span></span>  <span data-ttu-id="63e47-155">Zum Beispiel: "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="63e47-155">For example: "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="63e47-156">Sie können die Kategorie angeben, als Zeichenfolge oder eine Erweiterungsmethode, die die Kategorie aus dem Typ abgeleitet ist.</span><span class="sxs-lookup"><span data-stu-id="63e47-156">You can specify the category as a string or use an extension method that derives the category from the type.</span></span> <span data-ttu-id="63e47-157">Rufen Sie zum Angeben der Kategorie als Zeichenfolge `CreateLogger` auf eine `ILoggerFactory` Instanz, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="63e47-157">To specify the category as a string, call `CreateLogger` on an `ILoggerFactory` instance, as shown below.</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

<span data-ttu-id="63e47-158">In den meisten Fällen, es wird einfacher sein, verwenden Sie `ILogger<T>`, wie im folgenden Beispiel.</span><span class="sxs-lookup"><span data-stu-id="63e47-158">Most of the time, it will be easier to use  `ILogger<T>`, as in the following example.</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="63e47-159">Dies entspricht dem Aufruf `CreateLogger` mit dem vollqualifizierten Typnamen des `T`.</span><span class="sxs-lookup"><span data-stu-id="63e47-159">This is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="63e47-160">Protokollebene</span><span class="sxs-lookup"><span data-stu-id="63e47-160">Log level</span></span>

<span data-ttu-id="63e47-161">Jedes Mal Sie ein Protokoll zu schreiben, Sie geben die [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel).</span><span class="sxs-lookup"><span data-stu-id="63e47-161">Each time you write a log, you specify its [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="63e47-162">Die Protokollebene gibt den Grad der Schweregrad "oder" Wichtigkeit an.</span><span class="sxs-lookup"><span data-stu-id="63e47-162">The log level indicates the degree of severity or importance.</span></span>  <span data-ttu-id="63e47-163">Sie können z. B. Schreiben einer `Information` zu protokollieren, wenn eine Methode in der Regel beendet eine `Warning` melden Sie sich nach der Rückkehr von eine Methode 404-Rückgabecode zurück, und eine `Error` zu protokollieren, wenn Sie eine unerwartete Ausnahme abfangen.</span><span class="sxs-lookup"><span data-stu-id="63e47-163">For example, you might write an `Information` log when a method ends normally, a `Warning` log when a method returns a 404 return code, and an `Error` log when you catch an unexpected exception.</span></span>

<span data-ttu-id="63e47-164">Im folgenden Codebeispiel wird die Namen der Methoden (z. B. `LogWarning`) Geben Sie die Protokollebene.</span><span class="sxs-lookup"><span data-stu-id="63e47-164">In the following code example, the names of the methods (for example, `LogWarning`) specify the log level.</span></span>  <span data-ttu-id="63e47-165">Der erste Parameter ist der [Protokollieren der Ereignis-ID](#log-event-id) (weiter unten in diesem Artikel erläutert).</span><span class="sxs-lookup"><span data-stu-id="63e47-165">The first parameter is the [Log event ID](#log-event-id) (explained later in this article).</span></span>  <span data-ttu-id="63e47-166">Die verbleibenden Parameter erstellen eine Meldungszeichenfolge Protokoll.</span><span class="sxs-lookup"><span data-stu-id="63e47-166">The remaining parameters construct a log message string.</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="63e47-167">Log-Methoden, die die Ebene im Methodennamen enthalten sind [Erweiterungsmethoden für ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions).</span><span class="sxs-lookup"><span data-stu-id="63e47-167">Log methods that include the level in the method name are [extension methods for ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions).</span></span> <span data-ttu-id="63e47-168">Hinter den Kulissen diese Methoden Aufrufen einer `Log` Methode, eine `LogLevel` Parameter.</span><span class="sxs-lookup"><span data-stu-id="63e47-168">Behind the scenes, these methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="63e47-169">Sie erreichen die `Log` -Methode direkt statt eines dieser Erweiterungsmethoden, aber die Syntax ist relativ kompliziert.</span><span class="sxs-lookup"><span data-stu-id="63e47-169">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="63e47-170">Weitere Informationen finden Sie unter der [ILogger-Schnittstelle](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) und [protokollierungserweiterungen Quellcode](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="63e47-170">For more information, see the [ILogger interface](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) and the [logger extensions source code](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span></span>

<span data-ttu-id="63e47-171">ASP.NET Core definiert die folgenden [Protokollebenen](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel), hier vom niedrigsten zu höchsten Schweregrad sortiert.</span><span class="sxs-lookup"><span data-stu-id="63e47-171">ASP.NET Core defines the following [log levels](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel), ordered here from least to highest severity.</span></span>

* <span data-ttu-id="63e47-172">Trace = 0</span><span class="sxs-lookup"><span data-stu-id="63e47-172">Trace = 0</span></span>

  <span data-ttu-id="63e47-173">Informationen, die nur für ein Entwickler ein Problem Debuggen nützlich ist.</span><span class="sxs-lookup"><span data-stu-id="63e47-173">For information that is valuable only to a developer debugging an issue.</span></span> <span data-ttu-id="63e47-174">Diese Nachrichten können vertrauliche Daten enthalten und sollte daher nicht in einer produktiven Umgebung aktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="63e47-174">These messages may contain sensitive application data and so should not be enabled in a production environment.</span></span> <span data-ttu-id="63e47-175">*Standardmäßig deaktiviert.*</span><span class="sxs-lookup"><span data-stu-id="63e47-175">*Disabled by default.*</span></span> <span data-ttu-id="63e47-176">Ein Beispiel: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span><span class="sxs-lookup"><span data-stu-id="63e47-176">Example: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span></span>

* <span data-ttu-id="63e47-177">Debug = 1</span><span class="sxs-lookup"><span data-stu-id="63e47-177">Debug = 1</span></span>

  <span data-ttu-id="63e47-178">Informationen hat, der kurzfristige Nützlichkeit während der Entwicklung und Debuggen.</span><span class="sxs-lookup"><span data-stu-id="63e47-178">For information that has short-term usefulness during development and debugging.</span></span> <span data-ttu-id="63e47-179">Beispiel: `Entering method Configure with flag set to true.` Sie in der Regel würde nicht aktivieren `Debug` Ebene in der Produktion protokolliert, es sei denn, Sie wegen hoher Transaktionsvolumen von Protokollen behandeln möchten.</span><span class="sxs-lookup"><span data-stu-id="63e47-179">Example: `Entering method Configure with flag set to true.` You typically would not enable `Debug` level logs in production unless you are troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="63e47-180">Informationen = 2</span><span class="sxs-lookup"><span data-stu-id="63e47-180">Information = 2</span></span>

  <span data-ttu-id="63e47-181">Für das Nachverfolgen des allgemeinen Ablaufs der Anwendung an.</span><span class="sxs-lookup"><span data-stu-id="63e47-181">For tracking the general flow of the application.</span></span> <span data-ttu-id="63e47-182">Diese Protokolle werden in der Regel einige langfristigen Wert aufweisen.</span><span class="sxs-lookup"><span data-stu-id="63e47-182">These logs typically have some long-term value.</span></span> <span data-ttu-id="63e47-183">Ein Beispiel: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="63e47-183">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="63e47-184">Warnung = 3</span><span class="sxs-lookup"><span data-stu-id="63e47-184">Warning = 3</span></span>

  <span data-ttu-id="63e47-185">Ungewöhnliche oder unerwartete Ereignisse in den Anwendungsablauf.</span><span class="sxs-lookup"><span data-stu-id="63e47-185">For abnormal or unexpected events in the application flow.</span></span> <span data-ttu-id="63e47-186">Diesen zählen möglicherweise Fehler oder aus anderen Gründen nicht beenden die Anwendung zu verursachen, aber die untersucht werden müssen.</span><span class="sxs-lookup"><span data-stu-id="63e47-186">These may include errors or other conditions that do not cause the application to stop, but which may need to be investigated.</span></span> <span data-ttu-id="63e47-187">Behandelte Ausnahmen werden von einer gemeinsamen Stelle zum Verwenden der `Warning` Protokollebene.</span><span class="sxs-lookup"><span data-stu-id="63e47-187">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="63e47-188">Ein Beispiel: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="63e47-188">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="63e47-189">Fehler = 4</span><span class="sxs-lookup"><span data-stu-id="63e47-189">Error = 4</span></span>

  <span data-ttu-id="63e47-190">Für Fehler und Ausnahmen, die nicht behandelt werden können.</span><span class="sxs-lookup"><span data-stu-id="63e47-190">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="63e47-191">Diese Meldungen zeigen an, einen Fehler in der aktuellen Aktivität oder einen Vorgang (z. B. die aktuelle HTTP-Anforderung), keine anwendungsweite-Fehler.</span><span class="sxs-lookup"><span data-stu-id="63e47-191">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an application-wide failure.</span></span> <span data-ttu-id="63e47-192">Beispiel-Nachricht:`Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="63e47-192">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="63e47-193">Kritische = 5</span><span class="sxs-lookup"><span data-stu-id="63e47-193">Critical = 5</span></span>

  <span data-ttu-id="63e47-194">Für Fehler, die sofortige Aufmerksamkeit erfordern.</span><span class="sxs-lookup"><span data-stu-id="63e47-194">For failures that require immediate attention.</span></span> <span data-ttu-id="63e47-195">Beispiele: Datenverluste, nicht genügend Speicherplatz vorhanden.</span><span class="sxs-lookup"><span data-stu-id="63e47-195">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="63e47-196">Die Protokollebene können Sie steuern, wie viel Protokollausgabe für einen bestimmten Speichermedium geschrieben werden oder das Fenster auch anzeigen.</span><span class="sxs-lookup"><span data-stu-id="63e47-196">You can use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="63e47-197">Z. B. in einer produktionsumgebung empfiehlt alle Protokolle von `Information` Ebene, und fahren Sie mit einem Datenspeicher Volume und alle Protokolle des senken `Warning` Ebene und höher, fahren Sie mit einem Wert-Datenspeicher.</span><span class="sxs-lookup"><span data-stu-id="63e47-197">For example, in production you might want all logs of `Information` level and lower to go to a volume data store, and all logs of `Warning` level and higher to go to a value data store.</span></span> <span data-ttu-id="63e47-198">Während der Entwicklung können normalerweise Protokolle senden `Warning` oder einem höheren Schweregrad an die Konsole.</span><span class="sxs-lookup"><span data-stu-id="63e47-198">During development, you might normally send logs of `Warning` or higher severity to the console.</span></span> <span data-ttu-id="63e47-199">Wenn Sie beheben müssen, können Sie hinzufügen `Debug` Ebene.</span><span class="sxs-lookup"><span data-stu-id="63e47-199">Then when you need to troubleshoot, you can add `Debug` level.</span></span> <span data-ttu-id="63e47-200">Die [Protokoll filtern](#log-filtering) Abschnitt weiter unten in diesem Artikel wird erläutert, wie die Protokollierungsstufen steuern ein Anbieters behandelt.</span><span class="sxs-lookup"><span data-stu-id="63e47-200">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="63e47-201">Schreibt das Framework ASP.NET Core `Debug` Ebene Protokolle für die Framework-Ereignisse.</span><span class="sxs-lookup"><span data-stu-id="63e47-201">The ASP.NET Core framework writes `Debug` level logs for framework events.</span></span> <span data-ttu-id="63e47-202">Die Protokoll-Beispielen weiter oben in diesem Artikel ausgeschlossen Protokolle unten `Information` Ebene, sodass keine `Debug` Ebene Protokolle angezeigt wurden.</span><span class="sxs-lookup"><span data-stu-id="63e47-202">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` level logs were shown.</span></span> <span data-ttu-id="63e47-203">Hier ist ein Beispiel der konsolenprotokolle, wenn das Ausführen der beispielanwendung, die so konfiguriert, dass `Debug` und höher Protokolle, um die Konsole-Anbieter.</span><span class="sxs-lookup"><span data-stu-id="63e47-203">Here's an example of console logs if you run the sample application configured to show `Debug` and higher logs for the console provider.</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="63e47-204">Melden Sie sich Ereignis-ID</span><span class="sxs-lookup"><span data-stu-id="63e47-204">Log event ID</span></span>

<span data-ttu-id="63e47-205">Sie schreiben ein Protokolls, Sie jedes Mal angeben können eine *Ereignis-ID*.</span><span class="sxs-lookup"><span data-stu-id="63e47-205">Each time you write a log, you can specify an *event ID*.</span></span> <span data-ttu-id="63e47-206">Die Beispiel-app wird mit einer lokal definierten `LoggingEvents` Klasse:</span><span class="sxs-lookup"><span data-stu-id="63e47-206">The sample app does this by using a locally-defined `LoggingEvents` class:</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](logging/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

<span data-ttu-id="63e47-207">Ereignis-ID ist ein Ganzzahlwert, den Sie verwenden können, um einen Satz von protokollierten Ereignisse miteinander zu verknüpfen.</span><span class="sxs-lookup"><span data-stu-id="63e47-207">An event ID is an integer value that you can use to associate a set of logged events with one another.</span></span> <span data-ttu-id="63e47-208">Beispielsweise könnte ein Protokoll für das Hinzufügen eines Elements zu einem Einkaufswagen Ereignis-ID 1000 und ein Protokoll für einen Kauf abschließen konnte Ereignis-ID 1001 sein.</span><span class="sxs-lookup"><span data-stu-id="63e47-208">For instance, a log for adding an item to a shopping cart could be event ID 1000 and a log for completing a purchase could be event ID 1001.</span></span>

<span data-ttu-id="63e47-209">In die Protokollausgabe kann die Ereignis-ID in einem Feld gespeichert oder in der Textnachricht, je nach Anbieter enthalten.</span><span class="sxs-lookup"><span data-stu-id="63e47-209">In logging output, the event ID may be stored in a field or included in the text message, depending on the provider.</span></span>  <span data-ttu-id="63e47-210">Der Debug-Anbieter keine Ereignis-IDs anzeigen, aber der Anbieter für die Konsole zeigt sie in Klammern nach der Kategorie:</span><span class="sxs-lookup"><span data-stu-id="63e47-210">The Debug provider doesn't show event IDs, but the console provider shows them in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-format-string"></a><span data-ttu-id="63e47-211">Log-Format Meldungszeichenfolge</span><span class="sxs-lookup"><span data-stu-id="63e47-211">Log message format string</span></span>

<span data-ttu-id="63e47-212">Jedes Mal, wenn Sie ein Ereignisprotokoll zu schreiben, geben Sie eine Textnachricht.</span><span class="sxs-lookup"><span data-stu-id="63e47-212">Each time you write a log, you provide a text message.</span></span> <span data-ttu-id="63e47-213">Die Meldungszeichenfolge kann benannte Platzhalter enthalten, in welches Argument Werte, wie im folgenden Beispiel gezeigt eingefügt werden:</span><span class="sxs-lookup"><span data-stu-id="63e47-213">The message string can contain named placeholders into which argument values are placed, as in the following example:</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="63e47-214">Die Reihenfolge der Platzhalter, nicht ihren Namen, bestimmt, welche Parameter für diese verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="63e47-214">The order of placeholders, not their names, determines which parameters are used for them.</span></span> <span data-ttu-id="63e47-215">Wenn Ihr Code beispielsweise folgendermaßen lautet:</span><span class="sxs-lookup"><span data-stu-id="63e47-215">For example, if you have the following code:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="63e47-216">Die resultierende protokollmeldung würde wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="63e47-216">The resulting log message would look like this:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="63e47-217">Das protokollierungsframework message Formatierung auf diese Weise an, für die Protokollierung Anbieter implementieren können [semantische Protokollierung, auch bekannt als strukturierte Protokollierung](http://programmers.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="63e47-217">The logging framework does message formatting in this way to make it possible for logging providers to implement [semantic logging, also known as structured logging](http://programmers.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="63e47-218">Da die Argumente an das ereignisprotokollierungssystem, nicht nur die Zeichenfolge formatierte Nachricht übergeben werden können Protokollanbieter Parameterwerte als Felder zusätzlich zu der die Meldungszeichenfolge speichern.</span><span class="sxs-lookup"><span data-stu-id="63e47-218">Because the arguments themselves are passed to the logging system, not just the formatted message string, logging providers can store the parameter values as fields in addition to the message string.</span></span> <span data-ttu-id="63e47-219">Wenn Sie gerichtet sind die Protokollausgabe, um Azure Table Storage ein, und der Methodenaufruf Protokollierung sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="63e47-219">For example, if you are directing your log output to Azure Table Storage, and your logger method call looks like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="63e47-220">Jedes Azure-Tabellenentität möglicherweise `ID` und `RequestTime` Eigenschaften, die Abfragen von Protokolldaten zu vereinfachen, würden.</span><span class="sxs-lookup"><span data-stu-id="63e47-220">Each Azure Table entity could have `ID` and `RequestTime` properties, which would simplify queries on log data.</span></span> <span data-ttu-id="63e47-221">Sie alle Protokolle in einer bestimmten gefunden `RequestTime` Bereich, ohne das Timeout der Textnachricht zu analysieren.</span><span class="sxs-lookup"><span data-stu-id="63e47-221">You could find all logs within a particular `RequestTime` range, without having to parse the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="63e47-222">Protokollieren von Ausnahmen</span><span class="sxs-lookup"><span data-stu-id="63e47-222">Logging exceptions</span></span>

<span data-ttu-id="63e47-223">Die Protokollierung-Methoden verfügen über Überladungen, mit denen Sie eine Ausnahme aus, wie im folgenden Beispiel übergeben:</span><span class="sxs-lookup"><span data-stu-id="63e47-223">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

<span data-ttu-id="63e47-224">Andere Anbieter behandeln die Ausnahmeinformationen auf unterschiedliche Weise.</span><span class="sxs-lookup"><span data-stu-id="63e47-224">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="63e47-225">Hier ist ein Beispiel der Ausgabe der Debug-Anbieter aus der oben gezeigte Code ein.</span><span class="sxs-lookup"><span data-stu-id="63e47-225">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="63e47-226">Protokoll filtern</span><span class="sxs-lookup"><span data-stu-id="63e47-226">Log filtering</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="63e47-227">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="63e47-227">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="63e47-228">Sie können eine minimale Protokollebene für einen bestimmten Anbieter und die Kategorie oder für alle Anbieter oder alle Kategorien angeben.</span><span class="sxs-lookup"><span data-stu-id="63e47-228">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span>  <span data-ttu-id="63e47-229">Alle Protokolle unter dem Mindestwert liegt werden nicht an den Anbieter übergeben, damit sie angezeigt oder gespeichert abrufen nicht.</span><span class="sxs-lookup"><span data-stu-id="63e47-229">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span> 

<span data-ttu-id="63e47-230">Wenn Sie alle Protokolle unterdrücken möchten, können Sie angeben `LogLevel.None` als die minimale Protokollebene.</span><span class="sxs-lookup"><span data-stu-id="63e47-230">If you want to suppress all logs, you can specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="63e47-231">Der ganzzahlige Wert der `LogLevel.None` ist 6. Dies ist höher als `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="63e47-231">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="63e47-232">**Erstellen von Filterregeln in der Konfiguration**</span><span class="sxs-lookup"><span data-stu-id="63e47-232">**Create filter rules in configuration**</span></span>

<span data-ttu-id="63e47-233">Die Projektvorlagen erstellen Code, der Aufrufe `CreateDefaultBuilder` zum Einrichten der Protokollierung für die Konsole und Debuggen.</span><span class="sxs-lookup"><span data-stu-id="63e47-233">The project templates create code that calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="63e47-234">Die `CreateDefaultBuilder` Methode auch so eingerichtet, die Protokollierung für die Konfiguration im suchen eine `Logging` mit Code wie im folgenden Abschnitt:</span><span class="sxs-lookup"><span data-stu-id="63e47-234">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

<span data-ttu-id="63e47-235">Die Konfigurationsdaten gibt die minimale Protokollebenen, Anbieter und die Kategorie, wie im folgenden Beispiel an:</span><span class="sxs-lookup"><span data-stu-id="63e47-235">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](logging/sample2/AppSettings.json)]

<span data-ttu-id="63e47-236">Diese JSON erstellt sechs Filterregeln für den Debug-Anbieter, vier für den Anbieter für die Konsole, und eine, die für alle Anbieter gilt.</span><span class="sxs-lookup"><span data-stu-id="63e47-236">This JSON creates six filter rules, one for the Debug provider, four for the Console provider, and one that applies to all providers.</span></span> <span data-ttu-id="63e47-237">Sehen Sie, wie später nur eine dieser Regeln für jeden Anbieter ausgewählt wird bei einer `ILogger` Objekt erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="63e47-237">You'll see later how just one of these rules is chosen for each provider when an `ILogger` object is created.</span></span>

<span data-ttu-id="63e47-238">**Filterregeln in code**</span><span class="sxs-lookup"><span data-stu-id="63e47-238">**Filter rules in code**</span></span>

<span data-ttu-id="63e47-239">Sie können im Code Filterregeln registrieren, wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="63e47-239">You can register filter rules in code, as shown in the following example:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="63e47-240">Die zweite `AddFilter` gibt den Debug-Anbieter mithilfe der Typname.</span><span class="sxs-lookup"><span data-stu-id="63e47-240">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="63e47-241">Die erste `AddFilter` gilt für alle Anbieter, da es einen Anbietertyp angibt.</span><span class="sxs-lookup"><span data-stu-id="63e47-241">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

<span data-ttu-id="63e47-242">**Wie das Filtern von Regeln angewendet werden**</span><span class="sxs-lookup"><span data-stu-id="63e47-242">**How filtering rules are applied**</span></span>

<span data-ttu-id="63e47-243">Die Konfigurationsdaten werden und die `AddFilter` in den vorherigen Beispielen gezeigten Code erstellen Sie die Regeln, die in der folgenden Tabelle gezeigt.</span><span class="sxs-lookup"><span data-stu-id="63e47-243">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="63e47-244">Die ersten sechs stammen aus dem Konfigurationsbeispiel und die letzten beiden stammen aus dem Codebeispiel.</span><span class="sxs-lookup"><span data-stu-id="63e47-244">The first six come from the configuration example and the last two come from the code example.</span></span>

<span data-ttu-id="63e47-245">Anzahl</span><span class="sxs-lookup"><span data-stu-id="63e47-245">Number</span></span>|<span data-ttu-id="63e47-246">Anbieter</span><span class="sxs-lookup"><span data-stu-id="63e47-246">Provider</span></span>|<span data-ttu-id="63e47-247">Kategorien, die mit beginnen</span><span class="sxs-lookup"><span data-stu-id="63e47-247">Categories that begin with</span></span>|<span data-ttu-id="63e47-248">Minimale Protokollebene</span><span class="sxs-lookup"><span data-stu-id="63e47-248">Minimum log level</span></span>|
------|--------|--------------------------|-----------------|
<span data-ttu-id="63e47-249">1</span><span class="sxs-lookup"><span data-stu-id="63e47-249">1</span></span>|<span data-ttu-id="63e47-250">Debuggen</span><span class="sxs-lookup"><span data-stu-id="63e47-250">Debug</span></span>|<span data-ttu-id="63e47-251">Alle Kategorien</span><span class="sxs-lookup"><span data-stu-id="63e47-251">All categories</span></span>|<span data-ttu-id="63e47-252">Information</span><span class="sxs-lookup"><span data-stu-id="63e47-252">Information</span></span>|
<span data-ttu-id="63e47-253">2</span><span class="sxs-lookup"><span data-stu-id="63e47-253">2</span></span>|<span data-ttu-id="63e47-254">Konsole</span><span class="sxs-lookup"><span data-stu-id="63e47-254">Console</span></span>|<span data-ttu-id="63e47-255">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="63e47-255">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span>|<span data-ttu-id="63e47-256">Warnung</span><span class="sxs-lookup"><span data-stu-id="63e47-256">Warning</span></span>|
<span data-ttu-id="63e47-257">3</span><span class="sxs-lookup"><span data-stu-id="63e47-257">3</span></span>|<span data-ttu-id="63e47-258">Konsole</span><span class="sxs-lookup"><span data-stu-id="63e47-258">Console</span></span>|<span data-ttu-id="63e47-259">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="63e47-259">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>|<span data-ttu-id="63e47-260">Debuggen</span><span class="sxs-lookup"><span data-stu-id="63e47-260">Debug</span></span>|
<span data-ttu-id="63e47-261">4</span><span class="sxs-lookup"><span data-stu-id="63e47-261">4</span></span>|<span data-ttu-id="63e47-262">Konsole</span><span class="sxs-lookup"><span data-stu-id="63e47-262">Console</span></span>|<span data-ttu-id="63e47-263">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="63e47-263">Microsoft.AspNetCore.Mvc.Razor</span></span>|<span data-ttu-id="63e47-264">Fehler</span><span class="sxs-lookup"><span data-stu-id="63e47-264">Error</span></span>|
<span data-ttu-id="63e47-265">5</span><span class="sxs-lookup"><span data-stu-id="63e47-265">5</span></span>|<span data-ttu-id="63e47-266">Konsole</span><span class="sxs-lookup"><span data-stu-id="63e47-266">Console</span></span>|<span data-ttu-id="63e47-267">Alle Kategorien</span><span class="sxs-lookup"><span data-stu-id="63e47-267">All categories</span></span>|<span data-ttu-id="63e47-268">Information</span><span class="sxs-lookup"><span data-stu-id="63e47-268">Information</span></span>|
<span data-ttu-id="63e47-269">6</span><span class="sxs-lookup"><span data-stu-id="63e47-269">6</span></span>|<span data-ttu-id="63e47-270">Alle Anbieter</span><span class="sxs-lookup"><span data-stu-id="63e47-270">All providers</span></span>|<span data-ttu-id="63e47-271">Alle Kategorien</span><span class="sxs-lookup"><span data-stu-id="63e47-271">All categories</span></span>|<span data-ttu-id="63e47-272">Warnung</span><span class="sxs-lookup"><span data-stu-id="63e47-272">Warning</span></span>
<span data-ttu-id="63e47-273">7</span><span class="sxs-lookup"><span data-stu-id="63e47-273">7</span></span>|<span data-ttu-id="63e47-274">Alle Anbieter</span><span class="sxs-lookup"><span data-stu-id="63e47-274">All providers</span></span>|<span data-ttu-id="63e47-275">System</span><span class="sxs-lookup"><span data-stu-id="63e47-275">System</span></span>|<span data-ttu-id="63e47-276">Debuggen</span><span class="sxs-lookup"><span data-stu-id="63e47-276">Debug</span></span>
<span data-ttu-id="63e47-277">8</span><span class="sxs-lookup"><span data-stu-id="63e47-277">8</span></span>|<span data-ttu-id="63e47-278">Debuggen</span><span class="sxs-lookup"><span data-stu-id="63e47-278">Debug</span></span>|<span data-ttu-id="63e47-279">Microsoft</span><span class="sxs-lookup"><span data-stu-id="63e47-279">Microsoft</span></span>|<span data-ttu-id="63e47-280">Ablaufverfolgung</span><span class="sxs-lookup"><span data-stu-id="63e47-280">Trace</span></span>

<span data-ttu-id="63e47-281">Beim Erstellen einer `ILogger` Objekt zum Schreiben von Protokollen, die `ILoggerFactory` Objekt wählt eine einzelne Regel pro Anbieter angewendet, die diese Protokollierung.</span><span class="sxs-lookup"><span data-stu-id="63e47-281">When you create an `ILogger` object to write logs with, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="63e47-282">Alle Nachrichten, die von diesem geschrieben `ILogger` Objekt basierend auf den ausgewählten Regeln gefiltert werden.</span><span class="sxs-lookup"><span data-stu-id="63e47-282">All messages written by that `ILogger` object are filtered based on the selected rules.</span></span> <span data-ttu-id="63e47-283">Die spezifischsten Regel möglich, dass jeder Anbieter und jede Kategorie Paar wird aus den verfügbaren Regeln ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="63e47-283">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="63e47-284">Der folgende Algorithmus wird für jeden Anbieter verwendet, wenn ein `ILogger` wird nach einer bestimmten Kategorie erstellt:</span><span class="sxs-lookup"><span data-stu-id="63e47-284">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="63e47-285">Wählen Sie alle Regeln, die den Anbieter oder dessen Alias entsprechen.</span><span class="sxs-lookup"><span data-stu-id="63e47-285">Select all rules that match the provider or its alias.</span></span>  <span data-ttu-id="63e47-286">Wenn keiner gefunden werden, wählen Sie alle Regeln mit einem leeren Anbieter ein.</span><span class="sxs-lookup"><span data-stu-id="63e47-286">If none are found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="63e47-287">Die aus dem Ergebnis des vorherigen Schritts wählen Sie Regeln mit der längsten Kategorie Präfix.</span><span class="sxs-lookup"><span data-stu-id="63e47-287">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="63e47-288">Wenn keiner gefunden werden, wählen Sie alle Regeln, die eine Kategorie angeben.</span><span class="sxs-lookup"><span data-stu-id="63e47-288">If none are found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="63e47-289">Wenn mehrere Regeln ausgewählt sind, nehmen der **letzten** eine.</span><span class="sxs-lookup"><span data-stu-id="63e47-289">If multiple rules are selected take the **last** one.</span></span>
* <span data-ttu-id="63e47-290">Wenn keine Regeln ausgewählt sind, verwenden Sie `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="63e47-290">If no rules are selected, use `MinimumLevel`.</span></span>
 
<span data-ttu-id="63e47-291">Angenommen, Sie haben der vorangehenden Liste der Regeln und erstellen Sie ein `ILogger` Objekt für die Kategorie "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span><span class="sxs-lookup"><span data-stu-id="63e47-291">For example, suppose you have the preceding list of rules and you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="63e47-292">Für den Debug-Anbieter gelten die Regeln 1, 6 und 8.</span><span class="sxs-lookup"><span data-stu-id="63e47-292">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="63e47-293">Regel 8 ist am spezifischsten, damit das Element ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="63e47-293">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="63e47-294">Wenden Sie Regeln, 3, 4, 5 und 6, für den Anbieter Konsole.</span><span class="sxs-lookup"><span data-stu-id="63e47-294">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="63e47-295">Regel 3 ist am spezifischsten.</span><span class="sxs-lookup"><span data-stu-id="63e47-295">Rule 3 is most specific.</span></span>

<span data-ttu-id="63e47-296">Beim Erstellen von Protokollen mit einer `ILogger` Protokolle auf der Kategorie "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" `Trace` Ebene und höher ist mit der Debug-Anbieter und Protokolle von `Debug` Ebene und wird über die Konsole-Anbieter gesendet.</span><span class="sxs-lookup"><span data-stu-id="63e47-296">When you create logs with an `ILogger` for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", logs of `Trace` level and above will go to the Debug provider, and logs of `Debug` level and above will go to the Console provider.</span></span>

<span data-ttu-id="63e47-297">**Anbieter-Aliasen**</span><span class="sxs-lookup"><span data-stu-id="63e47-297">**Provider aliases**</span></span>

<span data-ttu-id="63e47-298">Können Sie den Namen ein Anbieters in der Konfiguration angeben, aber jeder Anbieter definiert eine kürzere *Alias* ist einfacher zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="63e47-298">You can use the type name to specify a provider in configuration, but each provider defines a shorter *alias* that is easier to use.</span></span> <span data-ttu-id="63e47-299">Verwenden Sie die folgenden Aliase für die integrierte Anbieter:</span><span class="sxs-lookup"><span data-stu-id="63e47-299">For the built-in providers, use the following aliases:</span></span>

- <span data-ttu-id="63e47-300">Konsole</span><span class="sxs-lookup"><span data-stu-id="63e47-300">Console</span></span>
- <span data-ttu-id="63e47-301">Debuggen</span><span class="sxs-lookup"><span data-stu-id="63e47-301">Debug</span></span>
- <span data-ttu-id="63e47-302">Im Ereignisprotokoll</span><span class="sxs-lookup"><span data-stu-id="63e47-302">EventLog</span></span>
- <span data-ttu-id="63e47-303">AzureAppServices</span><span class="sxs-lookup"><span data-stu-id="63e47-303">AzureAppServices</span></span>
- <span data-ttu-id="63e47-304">TraceSource</span><span class="sxs-lookup"><span data-stu-id="63e47-304">TraceSource</span></span>
- <span data-ttu-id="63e47-305">EventSource</span><span class="sxs-lookup"><span data-stu-id="63e47-305">EventSource</span></span>

<span data-ttu-id="63e47-306">**Minimale Standardebene**</span><span class="sxs-lookup"><span data-stu-id="63e47-306">**Default minimum level**</span></span>

<span data-ttu-id="63e47-307">Es gibt eine minimale festlegen, die in Kraft tritt nur dann, wenn keine Regeln von Konfigurations- oder Code für einen bestimmten Anbieter und Kategorie gelten.</span><span class="sxs-lookup"><span data-stu-id="63e47-307">There is a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="63e47-308">Im folgende Beispiel wird gezeigt, wie die Mindestebene festgelegt wird:</span><span class="sxs-lookup"><span data-stu-id="63e47-308">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="63e47-309">Wenn Sie die minimale Stufe nicht explizit festlegen, der Standardwert ist `Information`, was bedeutet, dass `Trace` und `Debug` Protokolle werden ignoriert.</span><span class="sxs-lookup"><span data-stu-id="63e47-309">IF you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

<span data-ttu-id="63e47-310">**Filterfunktionen**</span><span class="sxs-lookup"><span data-stu-id="63e47-310">**Filter functions**</span></span>

<span data-ttu-id="63e47-311">Sie können Code schreiben, in einer Filterfunktion Filterregeln anwenden.</span><span class="sxs-lookup"><span data-stu-id="63e47-311">You can write code in a filter function to apply filtering rules.</span></span> <span data-ttu-id="63e47-312">Filter-Funktion wird aufgerufen, für alle Anbieter und Kategorien, die keine Regeln, die sie per Konfiguration oder Code zugewiesen haben.</span><span class="sxs-lookup"><span data-stu-id="63e47-312">A filter function is invoked for all providers and categories that do not have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="63e47-313">Code in der Funktion hat Zugriff auf die Anbietertyp, Kategorie und Protokollebene, um zu entscheiden, und zwar unabhängig davon, ob eine Nachricht protokolliert werden sollen.</span><span class="sxs-lookup"><span data-stu-id="63e47-313">Code in the function has access to the provider type, category, and log level to decide whether or not a message should be logged.</span></span> <span data-ttu-id="63e47-314">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="63e47-314">For example:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="63e47-315">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="63e47-315">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="63e47-316">Einige Anbieter Protokollierung können Sie angeben, wenn Protokolle auf einem Speichermedium geschrieben oder ignoriert werden sollte, basierend auf Protokollebene und Kategorie zur Verfügung stehen.</span><span class="sxs-lookup"><span data-stu-id="63e47-316">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="63e47-317">Die `AddConsole` und `AddDebug` Erweiterungsmethoden bieten Überladungen, mit denen Sie die Filterkriterien zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="63e47-317">The `AddConsole` and `AddDebug` extension methods provide overloads that let you pass in filtering criteria.</span></span> <span data-ttu-id="63e47-318">Der folgende Code bewirkt, dass den Konsole-Anbieter, um Protokolle unten ignorieren `Warning` auf Ebene, während der Debug-Anbieter Protokolle ignoriert, die das Framework erstellt.</span><span class="sxs-lookup"><span data-stu-id="63e47-318">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](logging/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="63e47-319">Die `AddEventLog` Methode verfügt über eine Überladung mit einer `EventLogSettings` -Instanz, die möglicherweise eine Filterfunktion im enthält seine `Filter` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="63e47-319">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="63e47-320">TraceSource-Anbieter bietet keine keines dieser Überladungen seit seiner Protokolliergrad und andere Parameter abhängig von der `SourceSwitch` und `TraceListener` verwendet.</span><span class="sxs-lookup"><span data-stu-id="63e47-320">The TraceSource provider does not provide any of those overloads, since its logging level and other parameters are based on the  `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="63e47-321">Sie können festlegen, Filtern von Regeln für alle Anbieter, die registriert sind ein `ILoggerFactory` Instanz mithilfe der `WithFilter` Erweiterungsmethode.</span><span class="sxs-lookup"><span data-stu-id="63e47-321">You can set filtering rules for all providers that are registered with an `ILoggerFactory` instance by using the `WithFilter` extension method.</span></span> <span data-ttu-id="63e47-322">Das folgende Beispiel schränkt Framework Protokolle (Kategorie beginnt mit "Microsoft" oder "System")-Warnungen während des app-Protokolls auf Debugebene zugelassen.</span><span class="sxs-lookup"><span data-stu-id="63e47-322">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while letting the app log at debug level.</span></span>

[!code-csharp[](logging/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="63e47-323">Wenn Sie möchten die Filterung verwenden, um zu verhindern, dass alle Protokolle, die für eine bestimmte Kategorie geschrieben wird, können Sie angeben `LogLevel.None` als die minimale Protokollebene für diese Kategorie.</span><span class="sxs-lookup"><span data-stu-id="63e47-323">If you want to use filtering to prevent all logs from being written for a particular category, you can specify `LogLevel.None` as the minimum log level for that category.</span></span> <span data-ttu-id="63e47-324">Der ganzzahlige Wert der `LogLevel.None` ist 6. Dies ist höher als `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="63e47-324">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="63e47-325">Die `WithFilter` Erweiterungsmethode wird bereitgestellt, indem die [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet-Paket.</span><span class="sxs-lookup"><span data-stu-id="63e47-325">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="63e47-326">Die Methode gibt ein neues `ILoggerFactory` -Instanz, die die protokollmeldungen an alle Logger-Anbieter übergeben gefiltert wird, die bei ihm registrierten.</span><span class="sxs-lookup"><span data-stu-id="63e47-326">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="63e47-327">Er hat keinen Einfluss auf andere `ILoggerFactory` Instanzen, einschließlich der ursprünglichen `ILoggerFactory` Instanz.</span><span class="sxs-lookup"><span data-stu-id="63e47-327">It does not affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

---

## <a name="log-scopes"></a><span data-ttu-id="63e47-328">Log-Bereiche</span><span class="sxs-lookup"><span data-stu-id="63e47-328">Log scopes</span></span>

<span data-ttu-id="63e47-329">Sie können einen Satz von logischen Operationen innerhalb Gruppieren eine *Bereich* um dieselben Daten an jedes Protokoll anfügen, die im Rahmen dieser Menge erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="63e47-329">You can group a set of logical operations within a *scope* in order to attach the same data to each log that is created as part of that set.</span></span>  <span data-ttu-id="63e47-330">Beispielsweise sollten jedes Protokoll, das als Teil der Verarbeitung einer Transaktions zum Einschließen der Transaktions-ID erstellt, Sie</span><span class="sxs-lookup"><span data-stu-id="63e47-330">For example, you might want every log created as part of processing a transaction to include the transaction ID.</span></span>

<span data-ttu-id="63e47-331">Ein Bereich ist ein `IDisposable` von zurückgegebene Typ der `ILogger.BeginScope<TState>` -Methode und dauert, bis es entfernt wird.</span><span class="sxs-lookup"><span data-stu-id="63e47-331">A scope is an `IDisposable` type that is returned by the `ILogger.BeginScope<TState>` method and lasts until it is disposed.</span></span> <span data-ttu-id="63e47-332">Sie verwenden einen Bereich, indem Wrapping Ruft die Protokollierung in eine `using` block, wie hier gezeigt:</span><span class="sxs-lookup"><span data-stu-id="63e47-332">You use a scope by wrapping your logger calls in a `using` block, as shown here:</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="63e47-333">Der folgende Code ermöglicht Bereiche für den Anbieter Konsole:</span><span class="sxs-lookup"><span data-stu-id="63e47-333">The following code enables scopes for the console provider:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="63e47-334">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="63e47-334">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="63e47-335">In *"Program.cs"*:</span><span class="sxs-lookup"><span data-stu-id="63e47-335">In *Program.cs*:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="63e47-336">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="63e47-336">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="63e47-337">In *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="63e47-337">In *Startup.cs*:</span></span>

[!code-csharp[](logging/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

---

<span data-ttu-id="63e47-338">Jede Nachricht enthält die Bereichsinformationen:</span><span class="sxs-lookup"><span data-stu-id="63e47-338">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="63e47-339">Integrierte Protokollanbieter</span><span class="sxs-lookup"><span data-stu-id="63e47-339">Built-in logging providers</span></span>

<span data-ttu-id="63e47-340">ASP.NET Core liefert die folgenden Anbieter:</span><span class="sxs-lookup"><span data-stu-id="63e47-340">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="63e47-341">Konsole</span><span class="sxs-lookup"><span data-stu-id="63e47-341">Console</span></span>](#console)
* [<span data-ttu-id="63e47-342">Debuggen</span><span class="sxs-lookup"><span data-stu-id="63e47-342">Debug</span></span>](#debug)
* [<span data-ttu-id="63e47-343">EventSource</span><span class="sxs-lookup"><span data-stu-id="63e47-343">EventSource</span></span>](#eventsource)
* [<span data-ttu-id="63e47-344">EventLog</span><span class="sxs-lookup"><span data-stu-id="63e47-344">EventLog</span></span>](#eventlog)
* [<span data-ttu-id="63e47-345">TraceSource</span><span class="sxs-lookup"><span data-stu-id="63e47-345">TraceSource</span></span>](#tracesource)
* [<span data-ttu-id="63e47-346">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="63e47-346">Azure App Service</span></span>](#appservice)

<a id="console"></a>
### <a name="the-console-provider"></a><span data-ttu-id="63e47-347">Die Konsole-Anbieter</span><span class="sxs-lookup"><span data-stu-id="63e47-347">The console provider</span></span>

<span data-ttu-id="63e47-348">Die [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) anbieterpakets sendet die Ausgabe an die Konsole.</span><span class="sxs-lookup"><span data-stu-id="63e47-348">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="63e47-349">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="63e47-349">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddConsole()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="63e47-350">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="63e47-350">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddConsole()
```

<span data-ttu-id="63e47-351">[AddConsole Überladungen](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.consoleloggerextensions) können Sie übergeben ein einem minimalen Protokollebene, eine Filterfunktion und einen booleschen Wert ab, der angibt, ob die Bereiche unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="63e47-351">[AddConsole overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.consoleloggerextensions) let you pass in an a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span>  <span data-ttu-id="63e47-352">Eine andere Möglichkeit ist die Übergabe in einem `IConfiguration` -Objekt, das Unterstützung von Bereichen und Protokolliergrade angeben kann.</span><span class="sxs-lookup"><span data-stu-id="63e47-352">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span> 

<span data-ttu-id="63e47-353">Wenn Sie die Konsole-Anbieter für die Verwendung in der Produktion in Betracht ziehen, Bedenken Sie, dass es einen entscheidenden Einfluss auf die Leistung auswirkt.</span><span class="sxs-lookup"><span data-stu-id="63e47-353">If you are considering the console provider for use in production, be aware that it has a significant impact on performance.</span></span>

<span data-ttu-id="63e47-354">Beim Erstellen eines neuen Projekts in Visual Studio die `AddConsole` -Methode sieht folgendermaßen aus:</span><span class="sxs-lookup"><span data-stu-id="63e47-354">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="63e47-355">Dieser Code bezieht sich auf die `Logging` Teil der *appSettings.json* Datei:</span><span class="sxs-lookup"><span data-stu-id="63e47-355">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](logging/sample//appsettings.json)]

<span data-ttu-id="63e47-356">Die Einstellungen, die Grenzwert Framework Protokolle, um Warnungen beim ermöglicht es der app angezeigt, die auf Debugebene zu melden, wie in beschrieben die [Protokoll filtern](#log-filtering) Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="63e47-356">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="63e47-357">Weitere Informationen finden Sie unter [Konfiguration](configuration.md).</span><span class="sxs-lookup"><span data-stu-id="63e47-357">For more information, see [Configuration](configuration.md).</span></span>

---

<a id="debug"></a>
### <a name="the-debug-provider"></a><span data-ttu-id="63e47-358">Der Debug-Anbieter</span><span class="sxs-lookup"><span data-stu-id="63e47-358">The Debug provider</span></span>

<span data-ttu-id="63e47-359">Die [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) anbieterpakets schreibt Ausgabeprotokoll mithilfe der [System.Diagnostics.Debug](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.debug#System_Diagnostics_Debug) Klasse (`Debug.WriteLine` Methodenaufrufe).</span><span class="sxs-lookup"><span data-stu-id="63e47-359">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.debug#System_Diagnostics_Debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="63e47-360">Unter Linux, diesen Anbieter schreibt die Protokolle, um */var/log/message*.</span><span class="sxs-lookup"><span data-stu-id="63e47-360">On Linux, this provider writes logs to */var/log/message*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="63e47-361">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="63e47-361">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddDebug()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="63e47-362">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="63e47-362">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddDebug()
```

<span data-ttu-id="63e47-363">[AddDebug Überladungen](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.debugloggerfactoryextensions) können Sie eine minimale Protokollebene oder eine Filterfunktion übergeben.</span><span class="sxs-lookup"><span data-stu-id="63e47-363">[AddDebug overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.debugloggerfactoryextensions) let you pass in a minimum log level or a filter function.</span></span>

---

<a id="eventsource"></a>
### <a name="the-eventsource-provider"></a><span data-ttu-id="63e47-364">EventSource-Anbieter</span><span class="sxs-lookup"><span data-stu-id="63e47-364">The EventSource provider</span></span>

<span data-ttu-id="63e47-365">Für apps, die ASP.NET Core 1.1.0 ausgerichtet oder höher, die [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) anbieterpakets ereignisablaufverfolgung implementieren kann.</span><span class="sxs-lookup"><span data-stu-id="63e47-365">For apps that target ASP.NET Core 1.1.0 or higher, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="63e47-366">Unter Windows verwendet [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="63e47-366">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="63e47-367">Der Anbieter ist plattformübergreifende allerdings gibt es kein Ereignis Auflistung und der Anzeige Tools sind noch für Linux oder MacOS.</span><span class="sxs-lookup"><span data-stu-id="63e47-367">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="63e47-368">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="63e47-368">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddEventSourceLogger()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="63e47-369">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="63e47-369">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventSourceLogger()
```

---

<span data-ttu-id="63e47-370">Eine gute Möglichkeit zum Sammeln und Anzeigen von Protokollen ist die Verwendung der [PerfView-Dienstprogramm](https://www.microsoft.com/download/details.aspx?id=28567).</span><span class="sxs-lookup"><span data-stu-id="63e47-370">A good way to collect and view logs is to use the [PerfView utility](https://www.microsoft.com/download/details.aspx?id=28567).</span></span> <span data-ttu-id="63e47-371">Es gibt andere Tools zum Anzeigen von ETW-Protokolle allerdings PerfView bietet die beste Lösung für das Arbeiten mit ETW-Ereignisse, die von ASP.NET ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="63e47-371">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span> 

<span data-ttu-id="63e47-372">Fügen Sie zum Konfigurieren von PerfView zum Erfassen von Ereignissen, die von diesem Anbieter protokolliert die Zeichenfolge `*Microsoft-Extensions-Logging` auf die **zusätzliche Anbieter** Liste.</span><span class="sxs-lookup"><span data-stu-id="63e47-372">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="63e47-373">(Verpassen Sie nicht das Sternchen am Anfang der Zeichenfolge.)</span><span class="sxs-lookup"><span data-stu-id="63e47-373">(Don't miss the asterisk at the start of the string.)</span></span>

![Zusätzliche Perfview-Anbieter](logging/_static/perfview-additional-providers.png)

<span data-ttu-id="63e47-375">Erfassen von Ereignissen unter Nano Server sind einige zusätzliche Konfigurationsschritte erforderlich:</span><span class="sxs-lookup"><span data-stu-id="63e47-375">Capturing events on Nano Server requires some additional setup:</span></span>

* <span data-ttu-id="63e47-376">Verbinden Sie PowerShell-Remoting mit Nano Server:</span><span class="sxs-lookup"><span data-stu-id="63e47-376">Connect PowerShell remoting to the Nano Server:</span></span>

  ```powershell
  Enter-PSSession [name]
  ```

* <span data-ttu-id="63e47-377">Erstellen Sie eine ETW-Sitzung:</span><span class="sxs-lookup"><span data-stu-id="63e47-377">Create an ETW session:</span></span>

  ```powershell
  New-EtwTraceSession -Name "MyAppTrace" -LocalFilePath C:\trace.etl
  ```

* <span data-ttu-id="63e47-378">Hinzufügen von ETW-Anbietern für [CLR](https://msdn.microsoft.com/library/ff357718), ASP.NET Core und andere je nach Bedarf.</span><span class="sxs-lookup"><span data-stu-id="63e47-378">Add ETW providers for [CLR](https://msdn.microsoft.com/library/ff357718), ASP.NET Core, and others as needed.</span></span> <span data-ttu-id="63e47-379">Der ASP.NET Core-Anbieter-GUID ist `3ac73b97-af73-50e9-0822-5da4367920d0`.</span><span class="sxs-lookup"><span data-stu-id="63e47-379">The ASP.NET Core provider GUID is `3ac73b97-af73-50e9-0822-5da4367920d0`.</span></span> 

  ```powershell
  Add-EtwTraceProvider -Guid "{e13c0d23-ccbc-4e12-931b-d9cc2eee27e4}" -SessionName MyAppTrace
  Add-EtwTraceProvider -Guid "{3ac73b97-af73-50e9-0822-5da4367920d0}" -SessionName MyAppTrace
  ```

* <span data-ttu-id="63e47-380">Führen Sie den Standort aus, und möchten Sie die Aktionen aus, die Ablaufverfolgungsinformationen für den.</span><span class="sxs-lookup"><span data-stu-id="63e47-380">Run the site and do whatever actions you want tracing information for.</span></span>

* <span data-ttu-id="63e47-381">Beenden Sie die Ablaufverfolgung-Sitzung, wenn Sie fertig sind:</span><span class="sxs-lookup"><span data-stu-id="63e47-381">Stop the tracing session when you're finished:</span></span>

  ```powershell
  Stop-EtwTraceSession -Name "MyAppTrace"
  ```

<span data-ttu-id="63e47-382">Das resultierende *C:\trace.etl* Datei mit PerfView analysiert werden kann, wie auf anderen Editionen von Windows.</span><span class="sxs-lookup"><span data-stu-id="63e47-382">The resulting *C:\trace.etl* file can be analyzed with PerfView as on other editions of Windows.</span></span>

<a id="eventlog"></a>
### <a name="the-windows-eventlog-provider"></a><span data-ttu-id="63e47-383">Der Anbieter Windows-Ereignisprotokoll</span><span class="sxs-lookup"><span data-stu-id="63e47-383">The Windows EventLog provider</span></span>

<span data-ttu-id="63e47-384">Die [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) anbieterpakets sendet Ausgabeprotokoll in das Windows-Ereignisprotokoll.</span><span class="sxs-lookup"><span data-stu-id="63e47-384">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="63e47-385">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="63e47-385">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddEventLog()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="63e47-386">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="63e47-386">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventLog()
```

<span data-ttu-id="63e47-387">[AddEventLog Überladungen](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.eventloggerfactoryextensions) Let übergebenen `EventLogSettings` oder eine minimale Protokollebene.</span><span class="sxs-lookup"><span data-stu-id="63e47-387">[AddEventLog overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.eventloggerfactoryextensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

---

<a id="tracesource"></a>
### <a name="the-tracesource-provider"></a><span data-ttu-id="63e47-388">TraceSource-Anbieter</span><span class="sxs-lookup"><span data-stu-id="63e47-388">The TraceSource provider</span></span>

<span data-ttu-id="63e47-389">Die [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) anbieterpakets verwendet die [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx) Bibliotheken und Anbietern.</span><span class="sxs-lookup"><span data-stu-id="63e47-389">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx) libraries and providers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="63e47-390">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="63e47-390">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddTraceSource(sourceSwitchName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="63e47-391">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="63e47-391">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

---

<span data-ttu-id="63e47-392">[AddTraceSource Überladungen](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.tracesourcefactoryextensions) können Sie ein Quellschalter und einen Ablaufverfolgungslistener übergeben.</span><span class="sxs-lookup"><span data-stu-id="63e47-392">[AddTraceSource overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.tracesourcefactoryextensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="63e47-393">Zur Verwendung dieses Anbieters muss eine Anwendung für die .NET Framework (anstelle von .NET Core) ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="63e47-393">To use this provider, an application has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="63e47-394">Mit dem Anbieter können Weiterleiten von Nachrichten mit einer Vielzahl von [Listener](https://msdn.microsoft.com/library/4y5y10s7), wie z. B. die [TextWriterTraceListener](https://msdn.microsoft.com/library/system.diagnostics.textwritertracelistener) in der beispielanwendung verwendet.</span><span class="sxs-lookup"><span data-stu-id="63e47-394">The provider lets you route messages to a variety of [listeners](https://msdn.microsoft.com/library/4y5y10s7), such as the [TextWriterTraceListener](https://msdn.microsoft.com/library/system.diagnostics.textwritertracelistener) used in the sample application.</span></span>

<span data-ttu-id="63e47-395">Das folgende Beispiel konfiguriert eine `TraceSource` Anbieter, der protokolliert `Warning` und höher Nachrichten an das Konsolenfenster.</span><span class="sxs-lookup"><span data-stu-id="63e47-395">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](logging/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

<a id="appservice"></a>
### <a name="the-azure-app-service-provider"></a><span data-ttu-id="63e47-396">Der Azure App Service-Anbieter</span><span class="sxs-lookup"><span data-stu-id="63e47-396">The Azure App Service provider</span></span>

<span data-ttu-id="63e47-397">Die [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) anbieterpakets schreibt Protokolle in Textdateien in eine Azure App Service-app-Dateisystem und [blob-Speicher](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in einem Azure-Speicherkonto.</span><span class="sxs-lookup"><span data-stu-id="63e47-397">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="63e47-398">Der Anbieter ist nur für apps, die ASP.NET Core 1.1.0 ausgerichtet verfügbar oder höher.</span><span class="sxs-lookup"><span data-stu-id="63e47-398">The provider is available only for apps that target ASP.NET Core 1.1.0 or higher.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="63e47-399">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="63e47-399">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

> [!NOTE]
> <span data-ttu-id="63e47-400">ASP.NET Core 2.0 ist in der Vorschau.</span><span class="sxs-lookup"><span data-stu-id="63e47-400">ASP.NET Core 2.0 is in preview.</span></span>  <span data-ttu-id="63e47-401">Apps, mit der neuesten Preview-Version erstellt wurden möglicherweise nicht ausgeführt werden, wenn in Azure App Service bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="63e47-401">Apps created with the latest preview release may not run when deployed to Azure App Service.</span></span> <span data-ttu-id="63e47-402">Wenn ASP.NET Core 2.0 veröffentlicht wird, führt Azure App Service 2.0-apps und das Azure App Service-Anbieter funktioniert als hier angegeben.</span><span class="sxs-lookup"><span data-stu-id="63e47-402">When ASP.NET Core 2.0 is released, Azure App Service will run 2.0 apps, and the Azure App Service provider will work as indicated here.</span></span>

<span data-ttu-id="63e47-403">Sie müssen die Anbieter-Paket oder den Aufruf installieren die `AddAzureWebAppDiagnostics` Erweiterungsmethode.</span><span class="sxs-lookup"><span data-stu-id="63e47-403">You don't have to install the provider package or call the `AddAzureWebAppDiagnostics` extension method.</span></span>  <span data-ttu-id="63e47-404">Der Anbieter ist automatisch für Ihre app verfügbar, wenn Sie die app in Azure App Service bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="63e47-404">The provider is automatically available to your app when you deploy the app to Azure App Service.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="63e47-405">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="63e47-405">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

<span data-ttu-id="63e47-406">Ein `AddAzureWebAppDiagnostics` überladen ermöglicht die Übergabe in [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs), mit denen Sie können Standardeinstellungen, z. B. die Protokollierung Ausgabe Vorlage, die Blob-Name und die maximale Dateigröße überschreiben.</span><span class="sxs-lookup"><span data-stu-id="63e47-406">An `AddAzureWebAppDiagnostics` overload lets you pass in [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs), with which you can override default settings such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="63e47-407">(*Ausgabe Vorlage* ist eine Nachricht-Formatzeichenfolge, die auf alle Protokolle, zusätzlich zu den angewendet wird, die Sie, beim Aufrufen Bereitstellen einer `ILogger` Methode.)</span><span class="sxs-lookup"><span data-stu-id="63e47-407">(*Output template* is a message format string that is applied to all logs, on top of the one that you provide when you call an `ILogger` method.)</span></span>  

---

<span data-ttu-id="63e47-408">Beim Bereitstellen einer App Service-App wird Ihrer Anwendung berücksichtigt die Einstellungen in der [Diagnoseprotokolle](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) Teil der **Anwendungsdienst** Azure-Portal auf der Seite.</span><span class="sxs-lookup"><span data-stu-id="63e47-408">When you deploy to an App Service app, your application honors the settings in the [Diagnostic Logs](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="63e47-409">Wenn Sie diese Einstellungen ändern, werden die Änderungen sofort wirksam ohne, dass Sie die app starten oder erneut Code darauf bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="63e47-409">When you change those settings, the changes take effect immediately without requiring that you restart the app or redeploy code to it.</span></span> 

![Azure protokollierungseinstellungen](logging/_static/azure-logging-settings.png)

<span data-ttu-id="63e47-411">Der Standardspeicherort für Protokolldateien ist der *D:\\home\\LogFiles\\Anwendung* Ordner, und der Standarddateiname ist *Diagnose yyyymmdd.txt*.</span><span class="sxs-lookup"><span data-stu-id="63e47-411">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="63e47-412">Die standardmäßige maximale Dateigröße beträgt 10 MB, und die Standardeinstellung für die maximale Anzahl von Dateien beibehalten, ist 2.</span><span class="sxs-lookup"><span data-stu-id="63e47-412">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="63e47-413">Der Name des Blob ist *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="63e47-413">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span> <span data-ttu-id="63e47-414">Weitere Informationen zum Standardverhalten finden Sie unter [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs).</span><span class="sxs-lookup"><span data-stu-id="63e47-414">For more information about default behavior, see [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs).</span></span>

<span data-ttu-id="63e47-415">Der Anbieter funktioniert nur, wenn das Projekt in der Azure-Umgebung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="63e47-415">The provider only works when your project runs in the Azure environment.</span></span>  <span data-ttu-id="63e47-416">Sie hat keine Auswirkungen, wenn Sie lokal ausführen &mdash; schreibt nicht auf lokale Dateien oder des lokalen Entwicklungs-Speicher für Blobs.</span><span class="sxs-lookup"><span data-stu-id="63e47-416">It has no effect when you run locally &mdash; it does not write to local files or local development storage for blobs.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="63e47-417">Drittanbieter-Protokollanbieter</span><span class="sxs-lookup"><span data-stu-id="63e47-417">Third-party logging providers</span></span>

<span data-ttu-id="63e47-418">Hier sind einige Drittanbieter-Protokollierungsframeworks, die mit ASP.NET Core arbeiten:</span><span class="sxs-lookup"><span data-stu-id="63e47-418">Here are some third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="63e47-419">[ELMAH.IO](https://github.com/elmahio/Elmah.Io.Extensions.Logging) -Anbieter für den Dienst Elmah.Io</span><span class="sxs-lookup"><span data-stu-id="63e47-419">[elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging) - provider for the Elmah.Io service</span></span>

* <span data-ttu-id="63e47-420">[JSNLog](http://jsnlog.com) -JavaScript-Ausnahmen und anderen für clientseitige Ereignisse im Protokoll serverseitige protokolliert.</span><span class="sxs-lookup"><span data-stu-id="63e47-420">[JSNLog](http://jsnlog.com) - logs JavaScript exceptions and other client-side events in your server-side log.</span></span>

* <span data-ttu-id="63e47-421">[Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) -Anbieter für den Dienst Loggr</span><span class="sxs-lookup"><span data-stu-id="63e47-421">[Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) - provider for the Loggr service</span></span>

* <span data-ttu-id="63e47-422">[NLog](https://github.com/NLog/NLog.Extensions.Logging) -Anbieter für die NLog-Bibliothek</span><span class="sxs-lookup"><span data-stu-id="63e47-422">[NLog](https://github.com/NLog/NLog.Extensions.Logging) - provider for the NLog library</span></span>

* <span data-ttu-id="63e47-423">[Serilog](https://github.com/serilog/serilog-framework-logging) -Anbieter für die Serilog-Bibliothek</span><span class="sxs-lookup"><span data-stu-id="63e47-423">[Serilog](https://github.com/serilog/serilog-framework-logging) - provider for the Serilog library</span></span>

<span data-ttu-id="63e47-424">Einige Drittanbieter-Frameworks möglich [semantische Protokollierung, auch bekannt als strukturierte Protokollierung](http://programmers.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="63e47-424">Some third-party frameworks can do [semantic logging, also known as structured logging](http://programmers.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="63e47-425">Verwenden ein Drittanbieter-Framework ähnelt der mit einem der integrierten Anbieter: das Projekt ein NuGet-Paket hinzu, und rufen Sie eine Erweiterungsmethode für `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="63e47-425">Using a third-party framework is similar to using one of the built-in providers: add a NuGet package to your project and call an extension method on `ILoggerFactory`.</span></span> <span data-ttu-id="63e47-426">Weitere Informationen finden Sie unter jeder Framework-Dokumentation.</span><span class="sxs-lookup"><span data-stu-id="63e47-426">For more information, see each framework's documentation.</span></span>

<span data-ttu-id="63e47-427">Sie können auch Ihre eigenen benutzerdefinierten Anbieter erstellen, um andere Protokollierungsframeworks oder Ihren eigenen protokollierungsanforderungen unterstützen.</span><span class="sxs-lookup"><span data-stu-id="63e47-427">You can create your own custom providers as well, to support other logging frameworks or your own logging requirements.</span></span>
