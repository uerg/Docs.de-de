---
title: Hintergrundtasks mit gehosteten Diensten in ASP.NET Core
author: guardrex
description: Erfahren Sie, wie Sie Hintergrundtasks mit gehosteten Diensten in ASP.NET Core implementieren.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 8c6a4a039fdc2cbe097d3439b3d79b9228d458b1
ms.sourcegitcommit: 599ebae5c2d6fcb22dfa6ae7d1f4bdfcacb79af4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47210976"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="37d7a-103">Hintergrundtasks mit gehosteten Diensten in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="37d7a-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="37d7a-104">Von [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="37d7a-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="37d7a-105">In ASP.NET Core können Hintergrundtasks als *gehostete Dienste* implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="37d7a-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="37d7a-106">Ein gehosteter Dienst ist eine Klasse mit Logik für Hintergrundaufgaben, die die Schnittstelle <xref:Microsoft.Extensions.Hosting.IHostedService> implementiert.</span><span class="sxs-lookup"><span data-stu-id="37d7a-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="37d7a-107">In diesem Artikel sind drei Beispiel für gehostete Dienste enthalten:</span><span class="sxs-lookup"><span data-stu-id="37d7a-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="37d7a-108">Hintergrundtasks, die auf einem Timer ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="37d7a-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="37d7a-109">Gehostete Dienste, die einen bereichsbezogenen Dienst aktivieren.</span><span class="sxs-lookup"><span data-stu-id="37d7a-109">Hosted service that activates a scoped service.</span></span> <span data-ttu-id="37d7a-110">Der bereichsbezogene Dienst kann Dependency Injection verwenden.</span><span class="sxs-lookup"><span data-stu-id="37d7a-110">The scoped service can use dependency injection.</span></span>
* <span data-ttu-id="37d7a-111">Hintergrundtasks in der Warteschlange, die sequenziell ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="37d7a-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="37d7a-112">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="37d7a-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="37d7a-113">Die Beispiel-App wird in zwei Versionen bereitgestellt:</span><span class="sxs-lookup"><span data-stu-id="37d7a-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="37d7a-114">Web Host: Der Webhost eignet sich für das Hosten von Web-Apps.</span><span class="sxs-lookup"><span data-stu-id="37d7a-114">Web Host &ndash; The Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="37d7a-115">Der in diesem Thema gezeigte Beispielcode stammt aus der Webhostversion des Beispiels.</span><span class="sxs-lookup"><span data-stu-id="37d7a-115">The example code shown in this topic is from the Web Host version of the sample.</span></span> <span data-ttu-id="37d7a-116">Weitere Informationen finden Sie unter dem Thema [Webhost](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="37d7a-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="37d7a-117">Generischer Host: Der generische Host wurde in ASP.NET Core 2.1 neu eingeführt.</span><span class="sxs-lookup"><span data-stu-id="37d7a-117">Generic Host &ndash; The Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="37d7a-118">Weitere Informationen finden Sie unter dem Thema [Generischer Host](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="37d7a-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="package"></a><span data-ttu-id="37d7a-119">Package</span><span class="sxs-lookup"><span data-stu-id="37d7a-119">Package</span></span>

<span data-ttu-id="37d7a-120">Verweisen Sie auf das [Microsoft.AspNetCore.App-Metapaket](xref:fundamentals/metapackage-app), oder fügen Sie einen Paketverweis auf das Paket [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) hinzu.</span><span class="sxs-lookup"><span data-stu-id="37d7a-120">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="37d7a-121">Die IHostedService-Schnittstelle</span><span class="sxs-lookup"><span data-stu-id="37d7a-121">IHostedService interface</span></span>

<span data-ttu-id="37d7a-122">Gehostete Dienste implementieren die <xref:Microsoft.Extensions.Hosting.IHostedService>-Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="37d7a-122">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="37d7a-123">Die Schnittstelle definiert zwei Methoden für Objekte, die vom Host verwaltet werden:</span><span class="sxs-lookup"><span data-stu-id="37d7a-123">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="37d7a-124">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) - `StartAsync` enthält die Logik zum Starten der Hintergrundtask.</span><span class="sxs-lookup"><span data-stu-id="37d7a-124">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) - `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="37d7a-125">Bei Verwendung des [Webhosts](xref:fundamentals/host/web-host) wird `StartAsync` aufgerufen, nachdem der Server gestartet und [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) ausgelöst wurde.</span><span class="sxs-lookup"><span data-stu-id="37d7a-125">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="37d7a-126">Bei Verwendung des [generischen Hosts](xref:fundamentals/host/generic-host) wird `StartAsync` aufgerufen, bevor `ApplicationStarted` ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="37d7a-126">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="37d7a-127">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*): Wird ausgelöst, wenn der Host ein ordnungsgemäßes Herunterfahren ausführt.</span><span class="sxs-lookup"><span data-stu-id="37d7a-127">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) - Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="37d7a-128">`StopAsync` enthält die Logik zum Beenden des Hintergrundtasks und gibt alle nicht verwalteten Ressourcen frei.</span><span class="sxs-lookup"><span data-stu-id="37d7a-128">`StopAsync` contains the logic to end the background task and dispose of any unmanaged resources.</span></span> <span data-ttu-id="37d7a-129">Wenn die App unerwartet beendet wird (weil der Prozess der App beispielsweise fehlschlägt), wird `StopAsync` möglicherweise nicht aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="37d7a-129">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span>

<span data-ttu-id="37d7a-130">Der gehostete Dienst wird beim Start der App einmal aktiviert und beim Beenden der App ordnungsgemäß heruntergefahren.</span><span class="sxs-lookup"><span data-stu-id="37d7a-130">The hosted service is activated once at app startup and gracefully shutdown at app shutdown.</span></span> <span data-ttu-id="37d7a-131">Wenn <xref:System.IDisposable> implementiert ist, können Ressourcen freigegeben werden, wenn der Dienstcontainer freigegeben ist.</span><span class="sxs-lookup"><span data-stu-id="37d7a-131">When <xref:System.IDisposable> is implemented, resources can be disposed when the service container is disposed.</span></span> <span data-ttu-id="37d7a-132">Wenn während der Ausführung von Hintergrundtasks ein Fehler ausgelöst wird, sollte `Dispose` aufgerufen werden, auch wenn `StopAsync` nicht aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="37d7a-132">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="37d7a-133">Zeitlich festgelegte Hintergrundtasks</span><span class="sxs-lookup"><span data-stu-id="37d7a-133">Timed background tasks</span></span>

<span data-ttu-id="37d7a-134">Zeitlich festgelegte Hintergrundtasks verwenden die Klasse [System.Threading.Timer](xref:System.Threading.Timer).</span><span class="sxs-lookup"><span data-stu-id="37d7a-134">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="37d7a-135">Der Timer löst die `DoWork`-Methode des Tasks aus.</span><span class="sxs-lookup"><span data-stu-id="37d7a-135">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="37d7a-136">Der Timer wird durch `StopAsync` deaktiviert und freigegeben, wenn der Dienstcontainer durch `Dispose` freigegeben ist:</span><span class="sxs-lookup"><span data-stu-id="37d7a-136">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="37d7a-137">Der Dienst wird in `Startup.ConfigureServices` mit der Erweiterungsmethode `AddHostedService` registriert:</span><span class="sxs-lookup"><span data-stu-id="37d7a-137">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="37d7a-138">Verwenden eines bereichsbezogenen Diensts in einem Hintergrundtask</span><span class="sxs-lookup"><span data-stu-id="37d7a-138">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="37d7a-139">Erstellen Sie einen Bereich, um bereichsbezogene Dienste in `IHostedService` zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="37d7a-139">To use scoped services within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="37d7a-140">Bereiche werden für einen gehosteten Dienst nicht standardmäßig erstellt.</span><span class="sxs-lookup"><span data-stu-id="37d7a-140">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="37d7a-141">Der bereichsbezogene Dienst für Hintergrundtasks enthält die Logik des Hintergrundtasks.</span><span class="sxs-lookup"><span data-stu-id="37d7a-141">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="37d7a-142">Im folgenden Beispiel wird <xref:Microsoft.Extensions.Logging.ILogger> in den Dienst eingefügt:</span><span class="sxs-lookup"><span data-stu-id="37d7a-142">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="37d7a-143">Der gehostete Dienst erstellt einen Bereich, um den bereichsbezogenen Dienst für Hintergrundtasks aufzulösen, um die `DoWork`-Methode aufzurufen:</span><span class="sxs-lookup"><span data-stu-id="37d7a-143">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="37d7a-144">Die Dienste werden in `Startup.ConfigureServices` registriert.</span><span class="sxs-lookup"><span data-stu-id="37d7a-144">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="37d7a-145">Der Implementierung `IHostedService` wird mit der Erweiterungsmethode `AddHostedService` registriert:</span><span class="sxs-lookup"><span data-stu-id="37d7a-145">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="37d7a-146">Hintergrundtasks in der Warteschlange</span><span class="sxs-lookup"><span data-stu-id="37d7a-146">Queued background tasks</span></span>

<span data-ttu-id="37d7a-147">Eine Warteschlange für Hintergrundtasks basiert auf dem Element <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*>von .NET 4.x ([es ist vorläufig geplant, dieses in ASP.NET Core 3.0 zu integrieren](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="37d7a-147">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="37d7a-148">In `QueueHostedService` werden Hintergrundaufgaben in der Warteschlange aus der Warteschlange entfernt und als <xref:Microsoft.Extensions.Hosting.BackgroundService>, eine Basisklasse zum Bereitstellen von `IHostedService` mit langer Ausführungszeit, ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="37d7a-148">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a <xref:Microsoft.Extensions.Hosting.BackgroundService>, which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=16,20)]

<span data-ttu-id="37d7a-149">Die Dienste werden in `Startup.ConfigureServices` registriert.</span><span class="sxs-lookup"><span data-stu-id="37d7a-149">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="37d7a-150">Der Implementierung `IHostedService` wird mit der Erweiterungsmethode `AddHostedService` registriert:</span><span class="sxs-lookup"><span data-stu-id="37d7a-150">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

<span data-ttu-id="37d7a-151">In der Modellklasse für die Indexseite wird `IBackgroundTaskQueue` in den Konstruktor eingefügt und `Queue` zugewiesen:</span><span class="sxs-lookup"><span data-stu-id="37d7a-151">In the Index page model class, the `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="37d7a-152">Wenn auf der Indexseite auf die Schaltfläche **Task hinzufügen** geklickt wird, wird die `OnPostAddTask`-Methode ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="37d7a-152">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="37d7a-153">`QueueBackgroundWorkItem` wird aufgerufen, um das Arbeitselement in die Warteschlange einzureihen:</span><span class="sxs-lookup"><span data-stu-id="37d7a-153">`QueueBackgroundWorkItem` is called to enqueue the work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a><span data-ttu-id="37d7a-154">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="37d7a-154">Additional resources</span></span>

* [<span data-ttu-id="37d7a-155">Implement background tasks in microservices with IHostedService and the BackgroundService class (Implementieren von Hintergrundtasks in Microservices mit IHostedService und der BackgroundService-Klasse)</span><span class="sxs-lookup"><span data-stu-id="37d7a-155">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [<span data-ttu-id="37d7a-156">System.Threading.Timer</span><span class="sxs-lookup"><span data-stu-id="37d7a-156">System.Threading.Timer</span></span>](xref:System.Threading.Timer)
