---
title: Hintergrundtasks mit gehosteten Diensten in ASP.NET Core
author: guardrex
description: Erfahren Sie, wie Sie Hintergrundtasks mit gehosteten Diensten in ASP.NET Core implementieren.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
uid: fundamentals/host/hosted-services
ms.openlocfilehash: e5455e553cba817dce811391d4a909e501a20d9a
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273780"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="e7827-103">Hintergrundtasks mit gehosteten Diensten in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e7827-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="e7827-104">Von [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e7827-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e7827-105">In ASP.NET Core können Hintergrundtasks als *gehostete Dienste* implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="e7827-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="e7827-106">Ein gehosteter Dienst ist eine Klasse mit Logik für Hintergrundaufgaben, die die Schnittstelle [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) implementiert.</span><span class="sxs-lookup"><span data-stu-id="e7827-106">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="e7827-107">In diesem Artikel sind drei Beispiel für gehostete Dienste enthalten:</span><span class="sxs-lookup"><span data-stu-id="e7827-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="e7827-108">Hintergrundtasks, die auf einem Timer ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="e7827-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="e7827-109">Gehostete Dienste, die einen bereichsbezogenen Dienst aktivieren.</span><span class="sxs-lookup"><span data-stu-id="e7827-109">Hosted service that activates a scoped service.</span></span> <span data-ttu-id="e7827-110">Der bereichsbezogene Dienst kann Dependency Injection verwenden.</span><span class="sxs-lookup"><span data-stu-id="e7827-110">The scoped service can use dependency injection.</span></span>
* <span data-ttu-id="e7827-111">Hintergrundtasks in der Warteschlange, die sequenziell ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="e7827-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="e7827-112">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e7827-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="e7827-113">Die Beispiel-App wird in zwei Versionen bereitgestellt:</span><span class="sxs-lookup"><span data-stu-id="e7827-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="e7827-114">Web Host: Der Webhost eignet sich für das Hosten von Web-Apps.</span><span class="sxs-lookup"><span data-stu-id="e7827-114">Web Host &ndash; The Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="e7827-115">Der in diesem Thema gezeigte Beispielcode stammt aus der Webhostversion des Beispiels.</span><span class="sxs-lookup"><span data-stu-id="e7827-115">The example code shown in this topic is from the Web Host version of the sample.</span></span> <span data-ttu-id="e7827-116">Weitere Informationen finden Sie unter dem Thema [Webhost](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="e7827-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="e7827-117">Generischer Host: Der generische Host wurde in ASP.NET Core 2.1 neu eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e7827-117">Generic Host &ndash; The Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="e7827-118">Weitere Informationen finden Sie unter dem Thema [Generischer Host](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="e7827-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

::: moniker-end

## <a name="ihostedservice-interface"></a><span data-ttu-id="e7827-119">Die IHostedService-Schnittstelle</span><span class="sxs-lookup"><span data-stu-id="e7827-119">IHostedService interface</span></span>

<span data-ttu-id="e7827-120">Gehostete Dienste implementieren die [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice)-Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="e7827-120">Hosted services implement the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="e7827-121">Die Schnittstelle definiert zwei Methoden für Objekte, die vom Host verwaltet werden:</span><span class="sxs-lookup"><span data-stu-id="e7827-121">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="e7827-122">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync): Wird aufgerufen, nachdem der Server gestartet und [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) ausgelöst wurde.</span><span class="sxs-lookup"><span data-stu-id="e7827-122">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) - Called after the server has started and [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) is triggered.</span></span> <span data-ttu-id="e7827-123">`StartAsync` enthält die Logik zum Starten des Hintergrundtasks.</span><span class="sxs-lookup"><span data-stu-id="e7827-123">`StartAsync` contains the logic to start the background task.</span></span>

* <span data-ttu-id="e7827-124">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync): Wird ausgelöst, wenn der Host ein ordnungsgemäßes Herunterfahren ausführt.</span><span class="sxs-lookup"><span data-stu-id="e7827-124">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) - Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="e7827-125">`StopAsync` enthält die Logik zum Beenden des Hintergrundtasks und gibt alle nicht verwalteten Ressourcen frei.</span><span class="sxs-lookup"><span data-stu-id="e7827-125">`StopAsync` contains the logic to end the background task and dispose of any unmanaged resources.</span></span> <span data-ttu-id="e7827-126">Wenn die App unerwartet beendet wird (weil der Prozess der App beispielsweise fehlschlägt), wird `StopAsync` möglicherweise nicht aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="e7827-126">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span>

<span data-ttu-id="e7827-127">Der gehostete Dienst wird beim Start der App einmal aktiviert und beim Beenden der App ordnungsgemäß heruntergefahren.</span><span class="sxs-lookup"><span data-stu-id="e7827-127">The hosted service is activated once at app startup and gracefully shutdown at app shutdown.</span></span> <span data-ttu-id="e7827-128">Wenn [IDisposable](/dotnet/api/system.idisposable) implementiert ist, können Ressourcen freigegeben werden, wenn der Dienstcontainer freigegeben ist.</span><span class="sxs-lookup"><span data-stu-id="e7827-128">When [IDisposable](/dotnet/api/system.idisposable) is implemented, resources can be disposed when the service container is disposed.</span></span> <span data-ttu-id="e7827-129">Wenn während der Ausführung von Hintergrundtasks ein Fehler ausgelöst wird, sollte `Dispose` aufgerufen werden, auch wenn `StopAsync` nicht aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="e7827-129">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="e7827-130">Zeitlich festgelegte Hintergrundtasks</span><span class="sxs-lookup"><span data-stu-id="e7827-130">Timed background tasks</span></span>

<span data-ttu-id="e7827-131">Zeitlich festgelegte Hintergrundtasks verwenden die Klasse [System.Threading.Timer](/dotnet/api/system.threading.timer).</span><span class="sxs-lookup"><span data-stu-id="e7827-131">A timed background task makes use of the [System.Threading.Timer](/dotnet/api/system.threading.timer) class.</span></span> <span data-ttu-id="e7827-132">Der Timer löst die `DoWork`-Methode des Tasks aus.</span><span class="sxs-lookup"><span data-stu-id="e7827-132">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="e7827-133">Der Timer wird durch `StopAsync` deaktiviert und freigegeben, wenn der Dienstcontainer durch `Dispose` freigegeben ist:</span><span class="sxs-lookup"><span data-stu-id="e7827-133">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="e7827-134">Der Dienst wird in `Startup.ConfigureServices` mit der Erweiterungsmethode `AddHostedService` registriert:</span><span class="sxs-lookup"><span data-stu-id="e7827-134">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

<span data-ttu-id="e7827-135">[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="e7827-135">[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="e7827-136">Der Dienst ist in `Startup.ConfigureServices` registriert:</span><span class="sxs-lookup"><span data-stu-id="e7827-136">The service is registered in `Startup.ConfigureServices`:</span></span>

<span data-ttu-id="e7827-137">[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="e7827-137">[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]</span></span>

::: moniker-end

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="e7827-138">Verwenden eines bereichsbezogenen Diensts in einem Hintergrundtask</span><span class="sxs-lookup"><span data-stu-id="e7827-138">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="e7827-139">Erstellen Sie einen Bereich, um bereichsbezogene Dienste in `IHostedService` zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="e7827-139">To use scoped services within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="e7827-140">Bereiche werden für einen gehosteten Dienst nicht standardmäßig erstellt.</span><span class="sxs-lookup"><span data-stu-id="e7827-140">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="e7827-141">Der bereichsbezogene Dienst für Hintergrundtasks enthält die Logik des Hintergrundtasks.</span><span class="sxs-lookup"><span data-stu-id="e7827-141">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="e7827-142">Im folgenden Beispiel wird [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) in den Dienst eingefügt:</span><span class="sxs-lookup"><span data-stu-id="e7827-142">In the following example, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="e7827-143">Der gehostete Dienst erstellt einen Bereich, um den bereichsbezogenen Dienst für Hintergrundtasks aufzulösen, um die `DoWork`-Methode aufzurufen:</span><span class="sxs-lookup"><span data-stu-id="e7827-143">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="e7827-144">Die Dienste werden in `Startup.ConfigureServices` registriert.</span><span class="sxs-lookup"><span data-stu-id="e7827-144">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="e7827-145">Der Implementierung `IHostedService` wird mit der Erweiterungsmethode `AddHostedService` registriert:</span><span class="sxs-lookup"><span data-stu-id="e7827-145">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

<span data-ttu-id="e7827-146">[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="e7827-146">[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="e7827-147">Die Dienste sind in `Startup.ConfigureServices` registriert:</span><span class="sxs-lookup"><span data-stu-id="e7827-147">The services are registered in `Startup.ConfigureServices`:</span></span>

<span data-ttu-id="e7827-148">[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="e7827-148">[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]</span></span>

::: moniker-end

## <a name="queued-background-tasks"></a><span data-ttu-id="e7827-149">Hintergrundtasks in der Warteschlange</span><span class="sxs-lookup"><span data-stu-id="e7827-149">Queued background tasks</span></span>

<span data-ttu-id="e7827-150">Eine Warteschlange für Hintergrundtasks basiert auf dem Element [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) von .NET 4.x ([es ist vorläufig geplant, dieses in ASP.NET Core 3.0 zu integrieren](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="e7827-150">A background task queue is based on the .NET 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([tentatively scheduled to be built-in for ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="e7827-151">In `QueueHostedService` werden Hintergrundtasks (`workItem`) in der Warteschlange aus dieser entfernt und ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="e7827-151">In `QueueHostedService`, background tasks (`workItem`) in the queue are dequeued and executed:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=30-31,35)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="e7827-152">Die Dienste werden in `Startup.ConfigureServices` registriert.</span><span class="sxs-lookup"><span data-stu-id="e7827-152">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="e7827-153">Der Implementierung `IHostedService` wird mit der Erweiterungsmethode `AddHostedService` registriert:</span><span class="sxs-lookup"><span data-stu-id="e7827-153">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

<span data-ttu-id="e7827-154">[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]</span><span class="sxs-lookup"><span data-stu-id="e7827-154">[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="e7827-155">Die Dienste sind in `Startup.ConfigureServices` registriert:</span><span class="sxs-lookup"><span data-stu-id="e7827-155">The services are registered in `Startup.ConfigureServices`:</span></span>

<span data-ttu-id="e7827-156">[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]</span><span class="sxs-lookup"><span data-stu-id="e7827-156">[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]</span></span>

::: moniker-end

<span data-ttu-id="e7827-157">In der Modellklasse für die Indexseite wird `IBackgroundTaskQueue` in den Konstruktor eingefügt und `Queue` zugewiesen:</span><span class="sxs-lookup"><span data-stu-id="e7827-157">In the Index page model class, the `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="e7827-158">Wenn auf der Indexseite auf die Schaltfläche **Task hinzufügen** geklickt wird, wird die `OnPostAddTask`-Methode ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="e7827-158">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="e7827-159">`QueueBackgroundWorkItem` wird aufgerufen, um das Arbeitselement in die Warteschlange einzureihen:</span><span class="sxs-lookup"><span data-stu-id="e7827-159">`QueueBackgroundWorkItem` is called to enqueue the work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a><span data-ttu-id="e7827-160">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="e7827-160">Additional resources</span></span>

* [<span data-ttu-id="e7827-161">Implement background tasks in microservices with IHostedService and the BackgroundService class (Implementieren von Hintergrundtasks in Microservices mit IHostedService und der BackgroundService-Klasse)</span><span class="sxs-lookup"><span data-stu-id="e7827-161">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [<span data-ttu-id="e7827-162">System.Threading.Timer</span><span class="sxs-lookup"><span data-stu-id="e7827-162">System.Threading.Timer</span></span>](/dotnet/api/system.threading.timer)
