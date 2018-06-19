---
title: Hintergrundtasks mit gehosteten Diensten in ASP.NET Core
author: guardrex
description: Erfahren Sie, wie Sie Hintergrundtasks mit gehosteten Diensten in ASP.NET Core implementieren.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/hosted-services
ms.openlocfilehash: 89e595fb6ef38745d7377fdaaf1780c64320efe3
ms.sourcegitcommit: d43c84c4c80527c85e49d53691b293669557a79d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2018
ms.locfileid: "29374690"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="0d40a-103">Hintergrundtasks mit gehosteten Diensten in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0d40a-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="0d40a-104">Von [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0d40a-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="0d40a-105">In ASP.NET Core können Hintergrundtasks als *gehostete Dienste* implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="0d40a-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="0d40a-106">Ein gehosteter Dienst ist eine Klasse mit Logik für Hintergrundaufgaben, die die Schnittstelle [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) implementiert.</span><span class="sxs-lookup"><span data-stu-id="0d40a-106">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="0d40a-107">In diesem Artikel sind drei Beispiel für gehostete Dienste enthalten:</span><span class="sxs-lookup"><span data-stu-id="0d40a-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="0d40a-108">Hintergrundtasks, die auf einem Timer ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="0d40a-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="0d40a-109">Gehostete Dienste, die einen bereichsbezogenen Dienst aktivieren.</span><span class="sxs-lookup"><span data-stu-id="0d40a-109">Hosted service that activates a scoped service.</span></span> <span data-ttu-id="0d40a-110">Der bereichsbezogene Dienst kann Dependency Injection verwenden.</span><span class="sxs-lookup"><span data-stu-id="0d40a-110">The scoped service can use dependency injection.</span></span>
* <span data-ttu-id="0d40a-111">Hintergrundtasks in der Warteschlange, die sequenziell ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="0d40a-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="0d40a-112">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/hosted-services/samples/2.x) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0d40a-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/hosted-services/samples/2.x) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="0d40a-113">Die IHostedService-Schnittstelle</span><span class="sxs-lookup"><span data-stu-id="0d40a-113">IHostedService interface</span></span>

<span data-ttu-id="0d40a-114">Gehostete Dienste implementieren die [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice)-Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="0d40a-114">Hosted services implement the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="0d40a-115">Die Schnittstelle definiert zwei Methoden für Objekte, die vom Host verwaltet werden:</span><span class="sxs-lookup"><span data-stu-id="0d40a-115">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="0d40a-116">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync): Wird aufgerufen, nachdem der Server gestartet und [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) ausgelöst wurde.</span><span class="sxs-lookup"><span data-stu-id="0d40a-116">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) - Called after the server has started and [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) is triggered.</span></span> <span data-ttu-id="0d40a-117">`StartAsync` enthält die Logik zum Starten des Hintergrundtasks.</span><span class="sxs-lookup"><span data-stu-id="0d40a-117">`StartAsync` contains the logic to start the background task.</span></span>

* <span data-ttu-id="0d40a-118">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync): Wird ausgelöst, wenn der Host ein ordnungsgemäßes Herunterfahren ausführt.</span><span class="sxs-lookup"><span data-stu-id="0d40a-118">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) - Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="0d40a-119">`StopAsync` enthält die Logik zum Beenden des Hintergrundtasks und gibt alle nicht verwalteten Ressourcen frei.</span><span class="sxs-lookup"><span data-stu-id="0d40a-119">`StopAsync` contains the logic to end the background task and dispose of any unmanaged resources.</span></span> <span data-ttu-id="0d40a-120">Wenn die App unerwartet beendet wird (weil der Prozess der App beispielsweise fehlschlägt), wird `StopAsync` möglicherweise nicht aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="0d40a-120">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span>

<span data-ttu-id="0d40a-121">Der gehostete Dienst ist ein Singleton, das beim Start der App einmal aktiviert und beim Beenden der App ordnungsgemäß heruntergefahren wird.</span><span class="sxs-lookup"><span data-stu-id="0d40a-121">The hosted service is a singleton that's activated once at app startup and gracefully shutdown at app shutdown.</span></span> <span data-ttu-id="0d40a-122">Wenn [IDisposable](/dotnet/api/system.idisposable) implementiert ist, können Ressourcen freigegeben werden, wenn der Dienstcontainer freigegeben ist.</span><span class="sxs-lookup"><span data-stu-id="0d40a-122">When [IDisposable](/dotnet/api/system.idisposable) is implemented, resources can be disposed when the service container is disposed.</span></span> <span data-ttu-id="0d40a-123">Wenn während der Ausführung von Hintergrundtasks ein Fehler ausgelöst wird, sollte `Dispose` aufgerufen werden, auch wenn `StopAsync` nicht aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="0d40a-123">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="0d40a-124">Zeitlich festgelegte Hintergrundtasks</span><span class="sxs-lookup"><span data-stu-id="0d40a-124">Timed background tasks</span></span>

<span data-ttu-id="0d40a-125">Zeitlich festgelegte Hintergrundtasks verwenden die Klasse [System.Threading.Timer](/dotnet/api/system.threading.timer).</span><span class="sxs-lookup"><span data-stu-id="0d40a-125">A timed background task makes use of the [System.Threading.Timer](/dotnet/api/system.threading.timer) class.</span></span> <span data-ttu-id="0d40a-126">Der Timer löst die `DoWork`-Methode des Tasks aus.</span><span class="sxs-lookup"><span data-stu-id="0d40a-126">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="0d40a-127">Der Timer wird durch `StopAsync` deaktiviert und freigegeben, wenn der Dienstcontainer durch `Dispose` freigegeben ist:</span><span class="sxs-lookup"><span data-stu-id="0d40a-127">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="0d40a-128">Der Dienst ist in `Startup.ConfigureServices` registriert:</span><span class="sxs-lookup"><span data-stu-id="0d40a-128">The service is registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="0d40a-129">Verwenden eines bereichsbezogenen Diensts in einem Hintergrundtask</span><span class="sxs-lookup"><span data-stu-id="0d40a-129">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="0d40a-130">Erstellen Sie einen Bereich, um bereichsbezogene Dienste in `IHostedService` zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="0d40a-130">To use scoped services within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="0d40a-131">Bereiche werden für einen gehosteten Dienst nicht standardmäßig erstellt.</span><span class="sxs-lookup"><span data-stu-id="0d40a-131">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="0d40a-132">Der bereichsbezogene Dienst für Hintergrundtasks enthält die Logik des Hintergrundtasks.</span><span class="sxs-lookup"><span data-stu-id="0d40a-132">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="0d40a-133">Im folgenden Beispiel wird [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) in den Dienst eingefügt:</span><span class="sxs-lookup"><span data-stu-id="0d40a-133">In the following example, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="0d40a-134">Der gehostete Dienst erstellt einen Bereich, um den bereichsbezogenen Dienst für Hintergrundtasks aufzulösen, um die `DoWork`-Methode aufzurufen:</span><span class="sxs-lookup"><span data-stu-id="0d40a-134">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="0d40a-135">Die Dienste sind in `Startup.ConfigureServices` registriert:</span><span class="sxs-lookup"><span data-stu-id="0d40a-135">The services are registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="0d40a-136">Hintergrundtasks in der Warteschlange</span><span class="sxs-lookup"><span data-stu-id="0d40a-136">Queued background tasks</span></span>

<span data-ttu-id="0d40a-137">Eine Warteschlange für Hintergrundtasks basiert auf dem Element [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) von .NET 4.x ([es ist vorläufig geplant, dieses in ASP.NET Core 2.2 zu integrieren](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="0d40a-137">A background task queue is based on the .NET 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([tentatively scheduled to be built-in for ASP.NET Core 2.2](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="0d40a-138">In `QueueHostedService` werden Hintergrundtasks (`workItem`) in der Warteschlange aus dieser entfernt und ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="0d40a-138">In `QueueHostedService`, background tasks (`workItem`) in the queue are dequeued and executed:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Services/QueuedHostedService.cs?name=snippet1&highlight=30-31,35)]

<span data-ttu-id="0d40a-139">Die Dienste sind in `Startup.ConfigureServices` registriert:</span><span class="sxs-lookup"><span data-stu-id="0d40a-139">The services are registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Startup.cs?name=snippet3)]

<span data-ttu-id="0d40a-140">In der Modellklasse für die Indexseite wird `IBackgroundTaskQueue` in den Konstruktor eingefügt und `Queue` zugewiesen:</span><span class="sxs-lookup"><span data-stu-id="0d40a-140">In the Index page model class, the `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="0d40a-141">Wenn auf der Indexseite auf die Schaltfläche **Task hinzufügen** geklickt wird, wird die `OnPostAddTask`-Methode ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="0d40a-141">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="0d40a-142">`QueueBackgroundWorkItem` wird aufgerufen, um das Arbeitselement in die Warteschlange einzureihen:</span><span class="sxs-lookup"><span data-stu-id="0d40a-142">`QueueBackgroundWorkItem` is called to enqueue the work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a><span data-ttu-id="0d40a-143">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="0d40a-143">Additional resources</span></span>

* [<span data-ttu-id="0d40a-144">Implement background tasks in microservices with IHostedService and the BackgroundService class (Implementieren von Hintergrundtasks in Microservices mit IHostedService und der BackgroundService-Klasse)</span><span class="sxs-lookup"><span data-stu-id="0d40a-144">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [<span data-ttu-id="0d40a-145">System.Threading.Timer</span><span class="sxs-lookup"><span data-stu-id="0d40a-145">System.Threading.Timer</span></span>](/dotnet/api/system.threading.timer)
