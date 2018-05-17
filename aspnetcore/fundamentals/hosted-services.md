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
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a>Hintergrundtasks mit gehosteten Diensten in ASP.NET Core

Von [Luke Latham](https://github.com/guardrex)

In ASP.NET Core können Hintergrundtasks als *gehostete Dienste* implementiert werden. Ein gehosteter Dienst ist eine Klasse mit Logik für Hintergrundaufgaben, die die Schnittstelle [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) implementiert. In diesem Artikel sind drei Beispiel für gehostete Dienste enthalten:

* Hintergrundtasks, die auf einem Timer ausgeführt werden.
* Gehostete Dienste, die einen bereichsbezogenen Dienst aktivieren. Der bereichsbezogene Dienst kann Dependency Injection verwenden.
* Hintergrundtasks in der Warteschlange, die sequenziell ausgeführt werden.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/hosted-services/samples/2.x) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="ihostedservice-interface"></a>Die IHostedService-Schnittstelle

Gehostete Dienste implementieren die [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice)-Schnittstelle. Die Schnittstelle definiert zwei Methoden für Objekte, die vom Host verwaltet werden:

* [StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync): Wird aufgerufen, nachdem der Server gestartet und [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) ausgelöst wurde. `StartAsync` enthält die Logik zum Starten des Hintergrundtasks.

* [StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync): Wird ausgelöst, wenn der Host ein ordnungsgemäßes Herunterfahren ausführt. `StopAsync` enthält die Logik zum Beenden des Hintergrundtasks und gibt alle nicht verwalteten Ressourcen frei. Wenn die App unerwartet beendet wird (weil der Prozess der App beispielsweise fehlschlägt), wird `StopAsync` möglicherweise nicht aufgerufen.

Der gehostete Dienst ist ein Singleton, das beim Start der App einmal aktiviert und beim Beenden der App ordnungsgemäß heruntergefahren wird. Wenn [IDisposable](/dotnet/api/system.idisposable) implementiert ist, können Ressourcen freigegeben werden, wenn der Dienstcontainer freigegeben ist. Wenn während der Ausführung von Hintergrundtasks ein Fehler ausgelöst wird, sollte `Dispose` aufgerufen werden, auch wenn `StopAsync` nicht aufgerufen wird.

## <a name="timed-background-tasks"></a>Zeitlich festgelegte Hintergrundtasks

Zeitlich festgelegte Hintergrundtasks verwenden die Klasse [System.Threading.Timer](/dotnet/api/system.threading.timer). Der Timer löst die `DoWork`-Methode des Tasks aus. Der Timer wird durch `StopAsync` deaktiviert und freigegeben, wenn der Dienstcontainer durch `Dispose` freigegeben ist:

[!code-csharp[](hosted-services/samples/2.x/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

Der Dienst ist in `Startup.ConfigureServices` registriert:

[!code-csharp[](hosted-services/samples/2.x/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a>Verwenden eines bereichsbezogenen Diensts in einem Hintergrundtask

Erstellen Sie einen Bereich, um bereichsbezogene Dienste in `IHostedService` zu verwenden. Bereiche werden für einen gehosteten Dienst nicht standardmäßig erstellt.

Der bereichsbezogene Dienst für Hintergrundtasks enthält die Logik des Hintergrundtasks. Im folgenden Beispiel wird [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) in den Dienst eingefügt:

[!code-csharp[](hosted-services/samples/2.x/Services/ScopedProcessingService.cs?name=snippet1)]

Der gehostete Dienst erstellt einen Bereich, um den bereichsbezogenen Dienst für Hintergrundtasks aufzulösen, um die `DoWork`-Methode aufzurufen:

[!code-csharp[](hosted-services/samples/2.x/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

Die Dienste sind in `Startup.ConfigureServices` registriert:

[!code-csharp[](hosted-services/samples/2.x/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a>Hintergrundtasks in der Warteschlange

Eine Warteschlange für Hintergrundtasks basiert auf dem Element [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) von .NET 4.x ([es ist vorläufig geplant, dieses in ASP.NET Core 2.2 zu integrieren](https://github.com/aspnet/Hosting/issues/1280)):

[!code-csharp[](hosted-services/samples/2.x/Services/BackgroundTaskQueue.cs?name=snippet1)]

In `QueueHostedService` werden Hintergrundtasks (`workItem`) in der Warteschlange aus dieser entfernt und ausgeführt:

[!code-csharp[](hosted-services/samples/2.x/Services/QueuedHostedService.cs?name=snippet1&highlight=30-31,35)]

Die Dienste sind in `Startup.ConfigureServices` registriert:

[!code-csharp[](hosted-services/samples/2.x/Startup.cs?name=snippet3)]

In der Modellklasse für die Indexseite wird `IBackgroundTaskQueue` in den Konstruktor eingefügt und `Queue` zugewiesen:

[!code-csharp[](hosted-services/samples/2.x/Pages/Index.cshtml.cs?name=snippet1)]

Wenn auf der Indexseite auf die Schaltfläche **Task hinzufügen** geklickt wird, wird die `OnPostAddTask`-Methode ausgeführt. `QueueBackgroundWorkItem` wird aufgerufen, um das Arbeitselement in die Warteschlange einzureihen:

[!code-csharp[](hosted-services/samples/2.x/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Implement background tasks in microservices with IHostedService and the BackgroundService class (Implementieren von Hintergrundtasks in Microservices mit IHostedService und der BackgroundService-Klasse)](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [System.Threading.Timer](/dotnet/api/system.threading.timer)
