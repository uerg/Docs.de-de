---
title: Integritätsprüfungen in ASP.NET Core
author: guardrex
description: Erfahren Sie, wie Sie Integritätsprüfungen für ASP.NET Core-Infrastrukturelemente wie Apps und Datenbanken einrichten.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 12/03/2018
uid: host-and-deploy/health-checks
ms.openlocfilehash: d8fd43d9d689396cf30ca371763cdf7ac9423c77
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52862635"
---
# <a name="health-checks-in-aspnet-core"></a><span data-ttu-id="9b258-103">Integritätsprüfungen in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9b258-103">Health checks in ASP.NET Core</span></span>

<span data-ttu-id="9b258-104">Von [Luke Latham](https://github.com/guardrex) und [Glenn Condron](https://github.com/glennc)</span><span class="sxs-lookup"><span data-stu-id="9b258-104">By [Luke Latham](https://github.com/guardrex) and [Glenn Condron](https://github.com/glennc)</span></span>

<span data-ttu-id="9b258-105">ASP.NET Core bietet Middleware für Integritätsprüfungen und Bibliotheken für die Berichterstellung für Komponenten der App-Infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="9b258-105">ASP.NET Core offers Health Check Middleware and libraries for reporting the health of app infrastructure components.</span></span>

<span data-ttu-id="9b258-106">Integritätsprüfungen werden von einer App als HTTP-Endpunkte verfügbar gemacht.</span><span class="sxs-lookup"><span data-stu-id="9b258-106">Health checks are exposed by an app as HTTP endpoints.</span></span> <span data-ttu-id="9b258-107">Integritätsprüfungs-Endpunkte können für eine Vielzahl von Echtzeit-Überwachungsszenarien konfiguriert werden:</span><span class="sxs-lookup"><span data-stu-id="9b258-107">Health check endpoints can be configured for a variety of real-time monitoring scenarios:</span></span>

* <span data-ttu-id="9b258-108">Integritätstests können von Containerorchestratoren und Lastenausgleichsmodulen verwendet werden, um den Status einer App zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="9b258-108">Health probes can be used by container orchestrators and load balancers to check an app's status.</span></span> <span data-ttu-id="9b258-109">Ein Containerorchestrator kann z.B. auf eine fehlerhafte Integritätsprüfung reagieren, indem die Ausführung einer Bereitstellung angehalten oder ein Container neu gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="9b258-109">For example, a container orchestrator may respond to a failing health check by halting a rolling deployment or restarting a container.</span></span> <span data-ttu-id="9b258-110">Ein Lastenausgleichsmodul kann auf eine fehlerhafte App reagieren, indem Datenverkehr von der fehlerhaften zu einer fehlerfreien Instanz umgeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="9b258-110">A load balancer might react to an unhealthy app by routing traffic away from the failing instance to a healthy instance.</span></span>
* <span data-ttu-id="9b258-111">Die Nutzung von Arbeitsspeicher, Datenträgern und anderen physischen Serverressourcen kann auf einen fehlerfreien Status überwacht werden.</span><span class="sxs-lookup"><span data-stu-id="9b258-111">Use of memory, disk, and other physical server resources can be monitored for healthy status.</span></span>
* <span data-ttu-id="9b258-112">Integritätsprüfungen können die Abhängigkeiten einer App testen, wie z.B. Datenbanken und externe Dienstendpunkte, um Verfügbarkeit und normale Funktionsweise zu bestätigen.</span><span class="sxs-lookup"><span data-stu-id="9b258-112">Health checks can test an app's dependencies, such as databases and external service endpoints, to confirm availability and normal functioning.</span></span>

<span data-ttu-id="9b258-113">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([Vorgehensweise zum Herunterladen](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9b258-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="9b258-114">Die Beispiel-App enthält Beispiele der Szenarien, die in diesem Thema beschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="9b258-114">The sample app includes examples of the scenarios described in this topic.</span></span> <span data-ttu-id="9b258-115">Um die Beispiel-App für ein bestimmtes Szenario auszuführen, verwenden Sie den Befehl [dotnet run](/dotnet/core/tools/dotnet-run) aus dem Ordner des Projekts in einer Befehlsshell.</span><span class="sxs-lookup"><span data-stu-id="9b258-115">To run the sample app for a given scenario, use the [dotnet run](/dotnet/core/tools/dotnet-run) command from the project's folder in a command shell.</span></span> <span data-ttu-id="9b258-116">Informationen zur Verwendung der Beispiel-App finden Sie in der Datei *README.md* der Beispiel-App und den Szenariobeschreibungen in diesem Thema.</span><span class="sxs-lookup"><span data-stu-id="9b258-116">See the sample app's *README.md* file and the scenario descriptions in this topic for details on how to use the sample app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9b258-117">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="9b258-117">Prerequisites</span></span>

<span data-ttu-id="9b258-118">Integritätsprüfungen werden in der Regel mit einem externen Überwachungsdienst oder Containerorchestrator verwendet, um den Status einer App zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="9b258-118">Health checks are usually used with an external monitoring service or container orchestrator to check the status of an app.</span></span> <span data-ttu-id="9b258-119">Bevor Sie Integritätsprüfungen zu einer App hinzufügen, entscheiden Sie, welches Überwachungssystem verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="9b258-119">Before adding health checks to an app, decide on which monitoring system to use.</span></span> <span data-ttu-id="9b258-120">Das Überwachungssystem bestimmt, welche Arten von Integritätsprüfungen erstellt und wie die jeweiligen Endpunkte konfiguriert werden müssen.</span><span class="sxs-lookup"><span data-stu-id="9b258-120">The monitoring system dictates what types of health checks to create and how to configure their endpoints.</span></span>

<span data-ttu-id="9b258-121">Verweisen Sie auf das [Microsoft.AspNetCore.App-Metapaket](xref:fundamentals/metapackage-app), oder fügen Sie einen Paketverweis auf das Paket [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) hinzu.</span><span class="sxs-lookup"><span data-stu-id="9b258-121">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) package.</span></span>

<span data-ttu-id="9b258-122">Die Beispiel-App stellt Startcode bereit, um die Integritätsprüfungen für verschiedene Szenarien zu veranschaulichen.</span><span class="sxs-lookup"><span data-stu-id="9b258-122">The sample app provides start-up code to demonstrate health checks for several scenarios.</span></span> <span data-ttu-id="9b258-123">Das Szenario [Datenbanktest](#database-probe) testet die Integrität einer Datenbankverbindung mithilfe von [BeatPulse](https://github.com/Xabaril/BeatPulse).</span><span class="sxs-lookup"><span data-stu-id="9b258-123">The [database probe](#database-probe) scenario probes the health of a database connection using [BeatPulse](https://github.com/Xabaril/BeatPulse).</span></span> <span data-ttu-id="9b258-124">Das Szenario [DbContext-Test](#entity-framework-core-dbcontext-probe) testet eine Datenbank mithilfe eines EF Core-`DbContext`-Elements.</span><span class="sxs-lookup"><span data-stu-id="9b258-124">The [DbContext probe](#entity-framework-core-dbcontext-probe) scenario probes a database using an EF Core `DbContext`.</span></span> <span data-ttu-id="9b258-125">So erkunden Sie die Datenbankszenarien mithilfe der Beispiel-App:</span><span class="sxs-lookup"><span data-stu-id="9b258-125">To explore the database scenarios using the sample app:</span></span>

* <span data-ttu-id="9b258-126">Erstellen Sie eine Datenbank, und geben Sie die zugehörige Verbindungszeichenfolge in der Datei *appsettings.json* der App an.</span><span class="sxs-lookup"><span data-stu-id="9b258-126">Create a database and provide its connection string in the *appsettings.json* file of the app.</span></span>
* <span data-ttu-id="9b258-127">Fügen Sie einen Paketverweis auf [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/) hinzu.</span><span class="sxs-lookup"><span data-stu-id="9b258-127">Add a package reference to [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span></span>

> [!NOTE]
> <span data-ttu-id="9b258-128">[BeatPulse](https://github.com/Xabaril/BeatPulse) wird von Microsoft nicht verwaltet oder unterstützt.</span><span class="sxs-lookup"><span data-stu-id="9b258-128">[BeatPulse](https://github.com/Xabaril/BeatPulse) isn't maintained or supported by Microsoft.</span></span>

<span data-ttu-id="9b258-129">Ein anderes Szenario für Integritätsprüfungen zeigt, wie Sie Integritätsprüfungen auf einen Verwaltungsport filtern können.</span><span class="sxs-lookup"><span data-stu-id="9b258-129">Another health check scenario demonstrates how to filter health checks to a management port.</span></span> <span data-ttu-id="9b258-130">Für die Beispiel-App müssen Sie eine *Properties/launchSettings.json*-Datei erstellen, die die Verwaltungs-URL und den Verwaltungsport enthält.</span><span class="sxs-lookup"><span data-stu-id="9b258-130">The sample app requires you to create a *Properties/launchSettings.json* file that includes the management URL and management port.</span></span> <span data-ttu-id="9b258-131">Weitere Informationen finden Sie im Abschnitt [Filtern nach Port](#filter-by-port).</span><span class="sxs-lookup"><span data-stu-id="9b258-131">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

## <a name="basic-health-probe"></a><span data-ttu-id="9b258-132">Grundlegender Integritätstest</span><span class="sxs-lookup"><span data-stu-id="9b258-132">Basic health probe</span></span>

<span data-ttu-id="9b258-133">In vielen Apps genügt zur Ermittlung des App-Status eine grundlegende Integritätstestkonfiguration, die die Verfügbarkeit der App für die Verarbeitung von Anforderungen (*Lebendigkeit*) meldet.</span><span class="sxs-lookup"><span data-stu-id="9b258-133">For many apps, a basic health probe configuration that reports the app's availability to process requests (*liveness*) is sufficient to discover the status of the app.</span></span>

<span data-ttu-id="9b258-134">Die grundlegende Konfiguration registriert Integritätsprüfungsdienste und ruft die Middleware für Integritätsprüfungen auf, damit diese an einem URL-Endpunkt mit dem Integritätsstatus antwortet.</span><span class="sxs-lookup"><span data-stu-id="9b258-134">The basic configuration registers health check services and calls the Health Check Middleware to respond at a URL endpoint with a health response.</span></span> <span data-ttu-id="9b258-135">Standardmäßig werden keine spezifischen Integritätsprüfungen registriert, um eine bestimmte Abhängigkeit oder ein bestimmtes Subsystem zu testen.</span><span class="sxs-lookup"><span data-stu-id="9b258-135">By default, no specific health checks are registered to test any particular dependency or subsystem.</span></span> <span data-ttu-id="9b258-136">Die App wird als fehlerfrei angesehen, wenn sie an der URL des Integritätsendpunkts antworten kann.</span><span class="sxs-lookup"><span data-stu-id="9b258-136">The app is considered healthy if it's capable of responding at the health endpoint URL.</span></span> <span data-ttu-id="9b258-137">Der standardmäßige Antwortwriter schreibt den Status (`HealthCheckStatus`) als Klartextantwort zurück an den Client und gibt den Status entweder als `HealthCheckResult.Healthy` oder als `HealthCheckResult.Unhealthy` an.</span><span class="sxs-lookup"><span data-stu-id="9b258-137">The default response writer writes the status (`HealthCheckStatus`) as a plaintext response back to the client, indicating either a `HealthCheckResult.Healthy` or `HealthCheckResult.Unhealthy` status.</span></span>

<span data-ttu-id="9b258-138">Registrieren Sie Integritätsprüfungsdienste mit `AddHealthChecks` in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9b258-138">Register health check services with `AddHealthChecks` in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="9b258-139">Fügen Sie Middleware für Integritätsprüfungen mit `UseHealthChecks` in der Anforderungsverarbeitungspipeline von `Startup.Configure` hinzu.</span><span class="sxs-lookup"><span data-stu-id="9b258-139">Add Health Check Middleware with `UseHealthChecks` in the request processing pipeline of `Startup.Configure`.</span></span>

<span data-ttu-id="9b258-140">In der Beispiel-App wird der Integritätsprüfungs-Endpunkt in `/health` (*BasicStartup.cs*) erstellt:</span><span class="sxs-lookup"><span data-stu-id="9b258-140">In the sample app, the health check endpoint is created at `/health` (*BasicStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/BasicStartup.cs?name=snippet1&highlight=5,10)]

<span data-ttu-id="9b258-141">Um das Szenario für die grundlegende Konfiguration mithilfe der Beispiel-App auszuführen, verwenden Sie den folgenden Befehl aus dem Ordner des Projekts in einer Befehlsshell:</span><span class="sxs-lookup"><span data-stu-id="9b258-141">To run the basic configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario basic
```

### <a name="docker-example"></a><span data-ttu-id="9b258-142">Docker-Beispiel</span><span class="sxs-lookup"><span data-stu-id="9b258-142">Docker example</span></span>

<span data-ttu-id="9b258-143">[Docker](xref:host-and-deploy/docker/index) bietet eine integrierte `HEALTHCHECK`-Direktive, mit der Sie den Status einer App überprüfen können, die die grundlegende Konfiguration für Integritätsprüfungen verwendet:</span><span class="sxs-lookup"><span data-stu-id="9b258-143">[Docker](xref:host-and-deploy/docker/index) offers a built-in `HEALTHCHECK` directive that can be used to check the status of an app that uses the basic health check configuration:</span></span>

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a><span data-ttu-id="9b258-144">Erstellen von Integritätsprüfungen</span><span class="sxs-lookup"><span data-stu-id="9b258-144">Create health checks</span></span>

<span data-ttu-id="9b258-145">Integritätsprüfungen werden durch Implementieren der `IHealthCheck`-Schnittstelle erstellt.</span><span class="sxs-lookup"><span data-stu-id="9b258-145">Health checks are created by implementing the `IHealthCheck` interface.</span></span> <span data-ttu-id="9b258-146">Die `IHealthCheck.CheckHealthAsync`-Methode gibt ein `Task<HealthCheckResult>` zurück, das die Integrität als `Healthy`, `Degraded` oder `Unhealthy` angibt.</span><span class="sxs-lookup"><span data-stu-id="9b258-146">The `IHealthCheck.CheckHealthAsync` method returns a `Task<HealthCheckResult>` that indicates the health as `Healthy`, `Degraded`, or `Unhealthy`.</span></span> <span data-ttu-id="9b258-147">Das Ergebnis wird als Klartextantwort mit einem konfigurierbaren Statuscode geschrieben (diese Konfiguration wird im Abschnitt [Optionen für die Integritätsprüfung](#health-check-options) beschrieben).</span><span class="sxs-lookup"><span data-stu-id="9b258-147">The result is written as a plaintext response with a configurable status code (configuration is described in the [Health check options](#health-check-options) section).</span></span> <span data-ttu-id="9b258-148">`HealthCheckResult` kann auch optionale Schlüssel-Wert-Paare zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="9b258-148">`HealthCheckResult` can also return optional key-value pairs.</span></span>

### <a name="example-health-check"></a><span data-ttu-id="9b258-149">Beispiel für eine Integritätsprüfung</span><span class="sxs-lookup"><span data-stu-id="9b258-149">Example health check</span></span>

<span data-ttu-id="9b258-150">Die folgende `ExampleHealthCheck`-Klasse veranschaulicht das Layout einer Integritätsprüfung:</span><span class="sxs-lookup"><span data-stu-id="9b258-150">The following `ExampleHealthCheck` class demonstrates the layout of a health check:</span></span>

```csharp
public class ExampleHealthCheck : IHealthCheck
{
    public ExampleHealthCheck()
    {
        // Use dependency injection (DI) to supply any required services to the
        // health check.
    }

    public Task<HealthCheckResult> CheckHealthAsync(
        HealthCheckContext context, 
        CancellationToken cancellationToken = default(CancellationToken))
    {
        // Execute health check logic here. This example sets a dummy
        // variable to true.
        var healthCheckResultHealthy = true;

        if (healthCheckResultHealthy)
        {
            return Task.FromResult(
                HealthCheckResult.Healthy("The check indicates a healthy result."));
        }

        return Task.FromResult(
            HealthCheckResult.Unhealthy("The check indicates an unhealthy result."));
    }
}
```

### <a name="register-health-check-services"></a><span data-ttu-id="9b258-151">Registrieren von Integritätsprüfungsdiensten</span><span class="sxs-lookup"><span data-stu-id="9b258-151">Register health check services</span></span>

<span data-ttu-id="9b258-152">Der `ExampleHealthCheck`-Typ wird mit `AddCheck` zu den Integritätsprüfungsdiensten hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="9b258-152">The `ExampleHealthCheck` type is added to health check services with `AddCheck`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHealthChecks()
        .AddCheck<ExampleHealthCheck>("example_health_check");
}
```

<span data-ttu-id="9b258-153">Die in der folgenden Abbildung gezeigte `AddCheck`-Überladung legt den Fehlerstatus (`HealthStatus`) fest, der angegeben werden soll, wenn die Integritätsprüfung einen Fehler meldet.</span><span class="sxs-lookup"><span data-stu-id="9b258-153">The `AddCheck` overload shown in the following example sets the failure status (`HealthStatus`) to report when the health check reports a failure.</span></span> <span data-ttu-id="9b258-154">Wenn der Fehlerstatus auf `null` (Standardwert) festgelegt ist, wird `HealthStatus.Unhealthy` gemeldet.</span><span class="sxs-lookup"><span data-stu-id="9b258-154">If the failure status is set to `null` (default), `HealthStatus.Unhealthy` is reported.</span></span> <span data-ttu-id="9b258-155">Diese Überladung ist ein nützliches Szenario für Bibliotheksersteller: Bei einem Integritätsprüfungsfehler wird der durch die Bibliothek angegebene Fehlerstatus von der App erzwungen, wenn die Implementierung der Integritätsprüfung die Einstellung berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="9b258-155">This overload is a useful scenario for library authors, where the failure status indicated by the library is enforced by the app when a health check failure occurs if the health check implementation honors the setting.</span></span>

<span data-ttu-id="9b258-156">*Tags* können zum Filtern von Integritätsprüfungen verwendet werden (dies wird im Abschnitt [Filtern von Integritätsprüfungen](#filter-health-checks) genauer beschrieben).</span><span class="sxs-lookup"><span data-stu-id="9b258-156">*Tags* can be used to filter health checks (described further in the [Filter health checks](#filter-health-checks) section).</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check", 
        failureStatus: HealthStatus.Degraded, 
        tags: new[] { "example" });
```

<span data-ttu-id="9b258-157">`AddCheck` kann auch eine Lambdafunktion ausführen.</span><span class="sxs-lookup"><span data-stu-id="9b258-157">`AddCheck` can also execute a lambda function.</span></span> <span data-ttu-id="9b258-158">Im folgenden Beispiel wird der Name der Integritätsprüfung als `Example` angegeben, und die Prüfung gibt immer einen fehlerfreien Status zurück:</span><span class="sxs-lookup"><span data-stu-id="9b258-158">In the following example, the health check name is specified as `Example` and the check always returns a healthy state:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHealthChecks()
        .AddCheck("Example", () => 
            HealthCheckResult.Healthy("Example is OK!"), tags: new[] { "example" })
}
```

### <a name="use-health-checks-middleware"></a><span data-ttu-id="9b258-159">Verwenden von Middleware für Integritätsprüfungen</span><span class="sxs-lookup"><span data-stu-id="9b258-159">Use Health Checks Middleware</span></span>

<span data-ttu-id="9b258-160">Rufen Sie `UseHealthChecks` in der Verarbeitungspipeline von `Startup.Configure` mit der Endpunkt-URL oder dem relativen Pfad auf:</span><span class="sxs-lookup"><span data-stu-id="9b258-160">In `Startup.Configure`, call `UseHealthChecks` in the processing pipeline with the endpoint URL or relative path:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health");
}
```

<span data-ttu-id="9b258-161">Wenn die Integritätsprüfungen an einem bestimmten Port lauschen sollen, verwenden Sie eine Überladung von `UseHealthChecks`, um den Port festzulegen (dies wird im Abschnitt [Filtern nach Port](#filter-by-port) genauer beschrieben):</span><span class="sxs-lookup"><span data-stu-id="9b258-161">If the health checks should listen on a specific port, use an overload of `UseHealthChecks` to set the port (described further in the [Filter by port](#filter-by-port) section):</span></span>

```csharp
app.UseHealthChecks("/health", port: 8000);
```

<span data-ttu-id="9b258-162">Middleware für Integritätsprüfungen ist eine *Terminalmiddleware* in der Anforderungsverarbeitungspipeline der App.</span><span class="sxs-lookup"><span data-stu-id="9b258-162">Health Checks Middleware is a *terminal middleware* in the app's request processing pipeline.</span></span> <span data-ttu-id="9b258-163">Der erste gefundene Integritätsprüfungs-Endpunkt, der eine exakte Übereinstimmung mit der Anforderungs-URL darstellt, wird ausgeführt und schließt den Rest der Middlewarepipeline kurz.</span><span class="sxs-lookup"><span data-stu-id="9b258-163">The first health check endpoint encountered that's an exact match to the request URL executes and short-circuits the rest of the middleware pipeline.</span></span> <span data-ttu-id="9b258-164">Wenn kein Kurzschluss erfolgt, wird nach der durch Übereinstimmung ermittelten Integritätsprüfung keine weitere Middleware ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="9b258-164">When short-circuiting occurs, no middleware following the matched health check executes.</span></span>

## <a name="health-check-options"></a><span data-ttu-id="9b258-165">Optionen für die Integritätsprüfung</span><span class="sxs-lookup"><span data-stu-id="9b258-165">Health check options</span></span>

<span data-ttu-id="9b258-166">`HealthCheckOptions` ermöglichen die Anpassung des Verhaltens von Integritätsprüfungen:</span><span class="sxs-lookup"><span data-stu-id="9b258-166">`HealthCheckOptions` provide an opportunity to customize health check behavior:</span></span>

* [<span data-ttu-id="9b258-167">Filtern von Integritätsprüfungen</span><span class="sxs-lookup"><span data-stu-id="9b258-167">Filter health checks</span></span>](#filter-health-checks)
* [<span data-ttu-id="9b258-168">Anpassen des HTTP-Statuscodes</span><span class="sxs-lookup"><span data-stu-id="9b258-168">Customize the HTTP status code</span></span>](#customize-the-http-status-code)
* [<span data-ttu-id="9b258-169">Unterdrücken von Cacheheadern</span><span class="sxs-lookup"><span data-stu-id="9b258-169">Suppress cache headers</span></span>](#suppress-cache-headers)
* [<span data-ttu-id="9b258-170">Anpassen der Ausgabe</span><span class="sxs-lookup"><span data-stu-id="9b258-170">Customize output</span></span>](#customize-output)

### <a name="filter-health-checks"></a><span data-ttu-id="9b258-171">Filtern von Integritätsprüfungen</span><span class="sxs-lookup"><span data-stu-id="9b258-171">Filter health checks</span></span>

<span data-ttu-id="9b258-172">Standardmäßig führt die Middleware für Integritätsprüfungen alle registrierten Integritätsprüfungen aus.</span><span class="sxs-lookup"><span data-stu-id="9b258-172">By default, Health Check Middleware runs all registered health checks.</span></span> <span data-ttu-id="9b258-173">Um eine Teilmenge von Integritätsprüfungen auszuführen, stellen Sie eine Funktion bereit, die einen booleschen Wert an die Option `Predicate` zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="9b258-173">To run a subset of health checks, provide a function that returns a boolean to the `Predicate` option.</span></span> <span data-ttu-id="9b258-174">Im folgenden Beispiel wird die `Bar`-Integritätsprüfung anhand ihres Tags (`bar_tag`) in der Bedingungsanweisung der Funktion herausgefiltert. Dabei wird `true` nur zurückgegeben, wenn die `Tag`-Eigenschaft der Integritätsprüfung mit `foo_tag` oder `baz_tag` übereinstimmt:</span><span class="sxs-lookup"><span data-stu-id="9b258-174">In the following example, the `Bar` health check is filtered out by its tag (`bar_tag`) in the function's conditional statement, where `true` is only returned if the health check's `Tag` property matches `foo_tag` or `baz_tag`:</span></span>

```csharp
using System.Threading.Tasks;
using Microsoft.AspNetCore.Diagnostics.HealthChecks;
using Microsoft.Extensions.Diagnostics.HealthChecks;

public void ConfigureServices(IServiceCollection services)
{
    services.AddHealthChecks()
        .AddCheck("Foo", () => 
            HealthCheckResult.Healthy("Foo is OK!"), tags: new[] { "foo_tag" })
        .AddCheck("Bar", () => 
            HealthCheckResult.Unhealthy("Bar is unhealthy!"), tags: new[] { "bar_tag" })
        .AddCheck("Baz", () => 
            HealthCheckResult.Healthy("Baz is OK!"), tags: new[] { "baz_tag" });
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        // Filter out the 'Bar' health check. Only Foo and Baz execute.
        Predicate = (check) => check.Tags.Contains("foo_tag") || 
            check.Tags.Contains("baz_tag")
    });
}
```

### <a name="customize-the-http-status-code"></a><span data-ttu-id="9b258-175">Anpassen des HTTP-Statuscodes</span><span class="sxs-lookup"><span data-stu-id="9b258-175">Customize the HTTP status code</span></span>

<span data-ttu-id="9b258-176">Verwenden Sie `ResultStatusCodes`, um die Zuordnung des Integritätsstatus zu HTTP-Statuscodes anzupassen.</span><span class="sxs-lookup"><span data-stu-id="9b258-176">Use `ResultStatusCodes` to customize the mapping of health status to HTTP status codes.</span></span> <span data-ttu-id="9b258-177">Die folgenden `StatusCode`-Zuweisungen stellen die von der Middleware verwendeten Standardwerte dar.</span><span class="sxs-lookup"><span data-stu-id="9b258-177">The following `StatusCode` assignments are the default values used by the middleware.</span></span> <span data-ttu-id="9b258-178">Ändern Sie die Statuscodewerte so, dass sie Ihren Anforderungen entsprechen.</span><span class="sxs-lookup"><span data-stu-id="9b258-178">Change the status code values to meet your requirements.</span></span>

```csharp
using Microsoft.AspNetCore.Diagnostics.HealthChecks;
using Microsoft.Extensions.Diagnostics.HealthChecks;

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        // The following StatusCodes are the default assignments for
        // the HealthCheckStatus properties.
        ResultStatusCodes =
        {
            [HealthCheckStatus.Healthy] = StatusCodes.Status200OK,
            [HealthCheckStatus.Degraded] = StatusCodes.Status200OK,
            [HealthCheckStatus.Unhealthy] = StatusCodes.Status503ServiceUnavailable
        }
    });
}
```

### <a name="suppress-cache-headers"></a><span data-ttu-id="9b258-179">Unterdrücken von Cacheheadern</span><span class="sxs-lookup"><span data-stu-id="9b258-179">Suppress cache headers</span></span>

<span data-ttu-id="9b258-180">`AllowCachingResponses` steuert, ob die Middleware für Integritätsprüfungen einer Testantwort HTTP-Header hinzufügt, um die Zwischenspeicherung von Antworten zu verhindern.</span><span class="sxs-lookup"><span data-stu-id="9b258-180">`AllowCachingResponses` controls whether the Health Check Middleware adds HTTP headers to a probe response to prevent response caching.</span></span> <span data-ttu-id="9b258-181">Wenn der Wert `false` (Standard) lautet, legt die Middleware die Header `Cache-Control`, `Expires` und `Pragma` fest oder überschreibt sie, um eine Zwischenspeicherung der Antworten zu verhindern.</span><span class="sxs-lookup"><span data-stu-id="9b258-181">If the value is `false` (default), the middleware sets or overrides the `Cache-Control`, `Expires`, and `Pragma` headers to prevent response caching.</span></span> <span data-ttu-id="9b258-182">Wenn der Wert `true` lautet, ändert die Middleware die Cacheheader der Antwort nicht.</span><span class="sxs-lookup"><span data-stu-id="9b258-182">If the value is `true`, the middleware doesn't modify the cache headers of the response.</span></span>

```csharp
using Microsoft.AspNetCore.Diagnostics.HealthChecks;
using Microsoft.Extensions.Diagnostics.HealthChecks;

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        // The default value is false.
        AllowCachingResponses = false
    });
}
```

### <a name="customize-output"></a><span data-ttu-id="9b258-183">Anpassen der Ausgabe</span><span class="sxs-lookup"><span data-stu-id="9b258-183">Customize output</span></span>

<span data-ttu-id="9b258-184">Mit der Option `ResponseWriter` wird ein Delegat abgerufen oder festgelegt, der zum Schreiben der Antwort verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="9b258-184">The `ResponseWriter` option gets or sets a delegate used to write the response.</span></span> <span data-ttu-id="9b258-185">Der Standarddelegat schreibt eine minimale Klartextantwort mit dem Zeichenfolgenwert `HealthReport.Status`.</span><span class="sxs-lookup"><span data-stu-id="9b258-185">The default delegate writes a minimal plaintext response with the string value of `HealthReport.Status`.</span></span>

```csharp
using Microsoft.AspNetCore.Diagnostics.HealthChecks;
using Microsoft.Extensions.Diagnostics.HealthChecks;

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        // WriteResponse is a delegate used to write the response.
        ResponseWriter = WriteResponse
    });
}

private static Task WriteResponse(HttpContext httpContext, 
    HealthReport result)
{
    httpContext.Response.ContentType = "application/json";

    var json = new JObject(
        new JProperty("status", result.Status.ToString()),
        new JProperty("results", new JObject(result.Entries.Select(pair =>
            new JProperty(pair.Key, new JObject(
                new JProperty("status", pair.Value.Status.ToString()),
                new JProperty("description", pair.Value.Description),
                new JProperty("data", new JObject(pair.Value.Data.Select(
                    p => new JProperty(p.Key, p.Value))))))))));
    return httpContext.Response.WriteAsync(
        json.ToString(Formatting.Indented));
}
```

## <a name="database-probe"></a><span data-ttu-id="9b258-186">Datenbanktest</span><span class="sxs-lookup"><span data-stu-id="9b258-186">Database probe</span></span>

<span data-ttu-id="9b258-187">Eine Integritätsprüfung kann eine Datenbankabfrage angeben, die als boolescher Test ausgeführt wird, um zu ermitteln, ob die Datenbank normal reagiert.</span><span class="sxs-lookup"><span data-stu-id="9b258-187">A health check can specify a database query to run as a boolean test to indicate if the database is responding normally.</span></span>

<span data-ttu-id="9b258-188">Die Beispiel-App verwendet [BeatPulse](https://github.com/Xabaril/BeatPulse), eine Bibliothek für Integritätsprüfungen für ASP.NET Core-Apps, um eine Integritätsprüfung für eine SQL Server-Datenbank auszuführen.</span><span class="sxs-lookup"><span data-stu-id="9b258-188">The sample app uses [BeatPulse](https://github.com/Xabaril/BeatPulse), a health check library for ASP.NET Core apps, to perform a health check on a SQL Server database.</span></span> <span data-ttu-id="9b258-189">BeatPulse führt eine `SELECT 1`-Abfrage in der Datenbank aus, um zu bestätigen, dass die Verbindung mit der Datenbank fehlerfrei ist.</span><span class="sxs-lookup"><span data-stu-id="9b258-189">BeatPulse executes a `SELECT 1` query against the database to confirm the connection to the database is healthy.</span></span>

> [!WARNING]
> <span data-ttu-id="9b258-190">Wenn Sie die Datenbankverbindung mithilfe einer Abfrage überprüfen, wählen Sie eine Abfrage aus, die eine schnelle Antwort zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="9b258-190">When checking a database connection with a query, choose a query that returns quickly.</span></span> <span data-ttu-id="9b258-191">Bei einer Abfrage besteht immer das Risiko, dass die Datenbank überladen und ihre Leistung beeinträchtigt wird.</span><span class="sxs-lookup"><span data-stu-id="9b258-191">The query approach runs the risk of overloading the database and degrading its performance.</span></span> <span data-ttu-id="9b258-192">In den meisten Fällen ist es nicht notwendig, eine Testabfrage auszuführen.</span><span class="sxs-lookup"><span data-stu-id="9b258-192">In most cases, running a test query isn't necessary.</span></span> <span data-ttu-id="9b258-193">Es genügt zumeist, einfach erfolgreich eine Verbindung mit der Datenbank herzustellen.</span><span class="sxs-lookup"><span data-stu-id="9b258-193">Merely making a successful connection to the database is sufficient.</span></span> <span data-ttu-id="9b258-194">Wenn Sie eine Abfrage ausführen müssen, wählen Sie eine einfache SELECT-Abfrage aus, wie z.B. `SELECT 1`.</span><span class="sxs-lookup"><span data-stu-id="9b258-194">If you find it necessary to run a query, choose a simple SELECT query, such as `SELECT 1`.</span></span>

<span data-ttu-id="9b258-195">Um die BeatPulse-Bibliothek zu verwenden, schließen Sie einen Paketverweis auf [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/) ein.</span><span class="sxs-lookup"><span data-stu-id="9b258-195">In order to use the BeatPulse library, include a package reference to [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span></span>

<span data-ttu-id="9b258-196">Geben Sie in der Datei *appsettings.json* der App eine gültige Zeichenfolge für die Datenbankverbindung an.</span><span class="sxs-lookup"><span data-stu-id="9b258-196">Supply a valid database connection string in the *appsettings.json* file of the sample app.</span></span> <span data-ttu-id="9b258-197">Die App verwendet eine SQL Server-Datenbank namens `HealthCheckSample`:</span><span class="sxs-lookup"><span data-stu-id="9b258-197">The app uses a SQL Server database named `HealthCheckSample`:</span></span>

[!code-json[](health-checks/samples/2.x/HealthChecksSample/appsettings.json?highlight=3)]

<span data-ttu-id="9b258-198">Registrieren Sie Integritätsprüfungsdienste mit `AddHealthChecks` in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9b258-198">Register health check services with `AddHealthChecks` in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="9b258-199">Die Beispiel-App ruft die `AddSqlServer`-Methode von BeatPulse mit der Verbindungszeichenfolge der Datenbank (*DbHealthStartup.cs*) auf:</span><span class="sxs-lookup"><span data-stu-id="9b258-199">The sample app calls BeatPulse's `AddSqlServer` method with the database's connection string (*DbHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="9b258-200">Rufen Sie die Middleware für Integritätsprüfungen in der App-Verarbeitungspipeline in `Startup.Configure` auf:</span><span class="sxs-lookup"><span data-stu-id="9b258-200">Call Health Check Middleware in the app processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_Configure)]

<span data-ttu-id="9b258-201">Um das Szenario für den Datenbanktest mithilfe der Beispiel-App auszuführen, verwenden Sie den folgenden Befehl aus dem Ordner des Projekts in einer Befehlsshell:</span><span class="sxs-lookup"><span data-stu-id="9b258-201">To run the database probe scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario db
```

> [!NOTE]
> <span data-ttu-id="9b258-202">[BeatPulse](https://github.com/Xabaril/BeatPulse) wird von Microsoft nicht verwaltet oder unterstützt.</span><span class="sxs-lookup"><span data-stu-id="9b258-202">[BeatPulse](https://github.com/Xabaril/BeatPulse) isn't maintained or supported by Microsoft.</span></span>

## <a name="entity-framework-core-dbcontext-probe"></a><span data-ttu-id="9b258-203">Entity Framework Core-DbContext-Test</span><span class="sxs-lookup"><span data-stu-id="9b258-203">Entity Framework Core DbContext probe</span></span>

<span data-ttu-id="9b258-204">Die `DbContext`-Überprüfung wird in Apps unterstützt, die [Entity Framework Core](/ef/core/) (EF Core) verwenden.</span><span class="sxs-lookup"><span data-stu-id="9b258-204">The `DbContext` check is supported in apps that use [Entity Framework (EF) Core](/ef/core/).</span></span> <span data-ttu-id="9b258-205">Diese Überprüfung bestätigt, dass die App mit der Datenbank kommunizieren kann, die für einen EF Core-`DbContext` konfiguriert wurde.</span><span class="sxs-lookup"><span data-stu-id="9b258-205">This check confirms that the app can communicate with the database configured for an EF Core `DbContext`.</span></span> <span data-ttu-id="9b258-206">Standardmäßig ruft `DbContextHealthCheck` die `CanConnectAsync`-Methode von EF Core auf.</span><span class="sxs-lookup"><span data-stu-id="9b258-206">By default, the `DbContextHealthCheck` calls EF Core's `CanConnectAsync` method.</span></span> <span data-ttu-id="9b258-207">Sie können festlegen, welcher Vorgang ausgeführt wird, wenn Sie die Integrität mithilfe von Überladungen der `AddDbContextCheck`-Methode überprüfen.</span><span class="sxs-lookup"><span data-stu-id="9b258-207">You can customize what operation is run when checking health using overloads of the `AddDbContextCheck` method.</span></span>

<span data-ttu-id="9b258-208">`AddDbContextCheck<TContext>` registriert eine Integritätsprüfung für einen `DbContext` (`TContext`).</span><span class="sxs-lookup"><span data-stu-id="9b258-208">`AddDbContextCheck<TContext>` registers a health check for a `DbContext` (`TContext`).</span></span> <span data-ttu-id="9b258-209">Standardmäßig ist der Name der Integritätsprüfung der Name des `TContext`-Typs.</span><span class="sxs-lookup"><span data-stu-id="9b258-209">By default, the name of the health check is the name of the `TContext` type.</span></span> <span data-ttu-id="9b258-210">Eine Überladung ist verfügbar, um den Fehlerstatus, Tags sowie eine benutzerdefinierte Testabfrage zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="9b258-210">An overload is available to configure the failure status, tags, and a custom test query.</span></span>

<span data-ttu-id="9b258-211">In der Beispiel-App wird `AppDbContext` für `AddDbContextCheck` bereitgestellt und als Dienst in `Startup.ConfigureServices` registriert.</span><span class="sxs-lookup"><span data-stu-id="9b258-211">In the sample app, `AppDbContext` is provided to `AddDbContextCheck` and registered as a service in `Startup.ConfigureServices`.</span></span>

<span data-ttu-id="9b258-212">*DbContextHealthStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="9b258-212">*DbContextHealthStartup.cs*:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="9b258-213">In der Beispiel-App fügt `UseHealthChecks` die Middleware für Integritätsprüfungen in `Startup.Configure` hinzu.</span><span class="sxs-lookup"><span data-stu-id="9b258-213">In the sample app, `UseHealthChecks` adds the Health Check Middleware in `Startup.Configure`.</span></span>

<span data-ttu-id="9b258-214">*DbContextHealthStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="9b258-214">*DbContextHealthStartup.cs*:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_Configure)]

<span data-ttu-id="9b258-215">Um das `DbContext`-Testszenario unter Verwendung der Beispiel-App auszuführen, stellen Sie sicher, dass die durch die Verbindungszeichenfolge angegebene Datenbank in der SQL Server-Instanz nicht vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="9b258-215">To run the `DbContext` probe scenario using the sample app, confirm that the database specified by the the connection string doesn't exist in the SQL Server instance.</span></span> <span data-ttu-id="9b258-216">Falls die Datenbank vorhanden ist, löschen Sie sie.</span><span class="sxs-lookup"><span data-stu-id="9b258-216">If the database exists, delete it.</span></span>

<span data-ttu-id="9b258-217">Führen Sie den folgenden Befehl aus dem Ordner des Projekts in einer Befehlsshell aus:</span><span class="sxs-lookup"><span data-stu-id="9b258-217">Execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario dbcontext
```

<span data-ttu-id="9b258-218">Wenn die App ausgeführt wird, überprüfen Sie den Integritätsstatus, indem Sie in einem Browser eine Anforderung an den `/health`-Endpunkt senden.</span><span class="sxs-lookup"><span data-stu-id="9b258-218">After the app is running, check the health status by making a request to the `/health` endpoint in a browser.</span></span> <span data-ttu-id="9b258-219">Datenbank und `AppDbContext` sind nicht vorhanden, also sendet die App die folgende Antwort:</span><span class="sxs-lookup"><span data-stu-id="9b258-219">The database and `AppDbContext` don't exist, so app provides the following response:</span></span>

```
Unhealthy
```

<span data-ttu-id="9b258-220">Fordern Sie die Beispiel-App auf, die Datenbank zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="9b258-220">Trigger the sample app to create the database.</span></span> <span data-ttu-id="9b258-221">Senden Sie eine `/createdatabase`-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="9b258-221">Make a request to `/createdatabase`.</span></span> <span data-ttu-id="9b258-222">Die App sendet die folgende Antwort:</span><span class="sxs-lookup"><span data-stu-id="9b258-222">The app responds:</span></span>

```
Creating the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="9b258-223">Senden Sie eine Anforderung an den `/health`-Endpunkt.</span><span class="sxs-lookup"><span data-stu-id="9b258-223">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="9b258-224">Datenbank und Kontext sind vorhanden, also sendet die App die folgende Antwort:</span><span class="sxs-lookup"><span data-stu-id="9b258-224">The database and context exist, so app responds:</span></span>

```
Healthy
```

<span data-ttu-id="9b258-225">Fordern Sie die Beispiel-App auf, die Datenbank zu löschen.</span><span class="sxs-lookup"><span data-stu-id="9b258-225">Trigger the sample app to delete the database.</span></span> <span data-ttu-id="9b258-226">Senden Sie eine `/deletedatabase`-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="9b258-226">Make a request to `/deletedatabase`.</span></span> <span data-ttu-id="9b258-227">Die App sendet die folgende Antwort:</span><span class="sxs-lookup"><span data-stu-id="9b258-227">The app responds:</span></span>

```
Deleting the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="9b258-228">Senden Sie eine Anforderung an den `/health`-Endpunkt.</span><span class="sxs-lookup"><span data-stu-id="9b258-228">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="9b258-229">Die App meldet einen fehlerhaften Status:</span><span class="sxs-lookup"><span data-stu-id="9b258-229">The app provides an unhealthy response:</span></span>

```
Unhealthy
```

## <a name="separate-readiness-and-liveness-probes"></a><span data-ttu-id="9b258-230">Separate Tests für Bereitschaft und Lebendigkeit</span><span class="sxs-lookup"><span data-stu-id="9b258-230">Separate readiness and liveness probes</span></span>

<span data-ttu-id="9b258-231">In einigen Hostingszenarien wird ein Integritätsprüfungspaar verwendet, bei dem zwischen zwei App-Status unterschieden wird:</span><span class="sxs-lookup"><span data-stu-id="9b258-231">In some hosting scenarios, a pair of health checks are used that distinguish two app states:</span></span>

* <span data-ttu-id="9b258-232">Die App funktioniert, ist aber noch nicht für den Empfang von Anforderungen bereit.</span><span class="sxs-lookup"><span data-stu-id="9b258-232">The app is functioning but not yet ready to receive requests.</span></span> <span data-ttu-id="9b258-233">Dieser Status gibt die *Bereitschaft* der App wieder.</span><span class="sxs-lookup"><span data-stu-id="9b258-233">This state is the app's *readiness*.</span></span>
* <span data-ttu-id="9b258-234">Die App funktioniert und antwortet auf Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="9b258-234">The app is functioning and responding to requests.</span></span> <span data-ttu-id="9b258-235">Dieser Status gibt die *Lebendigkeit* der App wieder.</span><span class="sxs-lookup"><span data-stu-id="9b258-235">This state is the app's *liveness*.</span></span>

<span data-ttu-id="9b258-236">Die Bereitschaftsprüfung führt in der Regel eine Reihe umfassenderer und zeitaufwendigerer Überprüfungen durch, um zu ermitteln, ob alle Subsysteme und Ressourcen der App verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="9b258-236">The readiness check usually performs a more extensive and time-consuming set of checks to determine if all of the app's subsystems and resources are available.</span></span> <span data-ttu-id="9b258-237">Eine Lebendigkeitsprüfung führt nur eine schnelle Überprüfung aus, um zu ermitteln, ob die App für die Verarbeitung von Anforderungen verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="9b258-237">A liveness check merely performs a quick check to determine if the app is available to process requests.</span></span> <span data-ttu-id="9b258-238">Nachdem die App die Bereitschaftsprüfung einmal bestanden hat, muss die App nicht weiter mit diesen Prüfungen belastet werden – weitere Prüfungen müssen dann nur noch für die Lebendigkeit erfolgen.</span><span class="sxs-lookup"><span data-stu-id="9b258-238">After the app passes its readiness check, there's no need to burden the app further with the expensive set of readiness checks&mdash;further checks only require checking for liveness.</span></span>

<span data-ttu-id="9b258-239">Die Beispiel-App enthält eine Integritätsprüfung, um den Abschluss eines Starttasks mit langer Ausführungsdauer in einem [gehosteten Dienst](xref:fundamentals/host/hosted-services) zu melden.</span><span class="sxs-lookup"><span data-stu-id="9b258-239">The sample app contains a health check to report the completion of long-running startup task in a [Hosted Service](xref:fundamentals/host/hosted-services).</span></span> <span data-ttu-id="9b258-240">`StartupHostedServiceHealthCheck` macht die Eigenschaft `StartupTaskCompleted` verfügbar, die der gehostete Dienst auf `true` festlegen kann, wenn der Task mit langer Ausführungsdauer abgeschlossen ist (*StartupHostedServiceHealthCheck.cs*):</span><span class="sxs-lookup"><span data-stu-id="9b258-240">The `StartupHostedServiceHealthCheck` exposes a property, `StartupTaskCompleted`, that the hosted service can set to `true` when its long-running task is finished (*StartupHostedServiceHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=5)]

<span data-ttu-id="9b258-241">Der Hintergrundtask mit langer Ausführungsdauer wird von einem [gehosteten Dienst](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*) gestartet.</span><span class="sxs-lookup"><span data-stu-id="9b258-241">The long-running background task is started by a [Hosted Service](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span></span> <span data-ttu-id="9b258-242">Nach Abschluss des Tasks wird `StartupHostedServiceHealthCheck.StartupTaskCompleted` auf `true` festgelegt:</span><span class="sxs-lookup"><span data-stu-id="9b258-242">At the conclusion of the task, `StartupHostedServiceHealthCheck.StartupTaskCompleted` is set to `true`:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

<span data-ttu-id="9b258-243">Die Integritätsprüfung wird mit `AddCheck` zusammen mit dem gehosteten Dienst in `Startup.ConfigureServices` registriert.</span><span class="sxs-lookup"><span data-stu-id="9b258-243">The health check is registered with `AddCheck` in `Startup.ConfigureServices` along with the hosted service.</span></span> <span data-ttu-id="9b258-244">Da der gehostete Dienst die Eigenschaft in der Integritätsprüfung festlegen muss, wird die Integritätsprüfung ebenfalls im Dienstcontainer (*LivenessProbeStartup.cs*) registriert:</span><span class="sxs-lookup"><span data-stu-id="9b258-244">Because the hosted service must set the property on the health check, the health check is also registered in the service container (*LivenessProbeStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="9b258-245">Rufen Sie die Middleware für Integritätsprüfungen in der App-Verarbeitungspipeline in `Startup.Configure` auf.</span><span class="sxs-lookup"><span data-stu-id="9b258-245">Call Health Check Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="9b258-246">In der Beispiel-App werden die Integritätsprüfungs-Endpunkte in `/health/ready` für die Bereitschaftsprüfung und in `/health/live` für die Lebendigkeitsprüfung erstellt.</span><span class="sxs-lookup"><span data-stu-id="9b258-246">In the sample app, the health check endpoints are created at `/health/ready` for the readiness check and `/health/live` for the liveness check.</span></span> <span data-ttu-id="9b258-247">Die Bereitschaftsprüfung filtert auf Integritätsprüfungen mit dem `ready`-Tag.</span><span class="sxs-lookup"><span data-stu-id="9b258-247">The readiness check filters health checks to the health check with the `ready` tag.</span></span> <span data-ttu-id="9b258-248">Die Lebendigkeitsprüfung filtert `StartupHostedServiceHealthCheck` durch Rückgabe von `false` in `HealthCheckOptions.Predicate` heraus (weitere Informationen finden Sie unter [Filtern von Integritätsprüfungen](#filter-health-checks)):</span><span class="sxs-lookup"><span data-stu-id="9b258-248">The liveness check filters out the `StartupHostedServiceHealthCheck` by returning `false` in the `HealthCheckOptions.Predicate` (for more information, see [Filter health checks](#filter-health-checks)):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_Configure)]

<span data-ttu-id="9b258-249">Um das Szenario für die Konfiguration von Bereitschafts-/Lebendigkeitsprüfungen mithilfe der Beispiel-App auszuführen, verwenden Sie den folgenden Befehl aus dem Ordner des Projekts in einer Befehlsshell:</span><span class="sxs-lookup"><span data-stu-id="9b258-249">To run the readiness/liveness configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario liveness
```

<span data-ttu-id="9b258-250">Besuchen Sie `/health/ready` mehrmals in einem Browser, bis 15 Sekunden verstrichen sind.</span><span class="sxs-lookup"><span data-stu-id="9b258-250">In a browser, visit `/health/ready` several times until 15 seconds have passed.</span></span> <span data-ttu-id="9b258-251">Die Integritätsprüfung meldet während der ersten 15 Sekunden `Unhealthy`.</span><span class="sxs-lookup"><span data-stu-id="9b258-251">The health check reports `Unhealthy` for the first 15 seconds.</span></span> <span data-ttu-id="9b258-252">Nach 15 Sekunden meldet der Endpunkt `Healthy`, was darauf hindeutet, dass die Ausführung des Tasks mit langer Ausführungsdauer durch den gehosteten Dienst abgeschlossen wurde.</span><span class="sxs-lookup"><span data-stu-id="9b258-252">After 15 seconds, the endpoint reports `Healthy`, which reflects the completion of the long-running task by the hosted service.</span></span>

### <a name="kubernetes-example"></a><span data-ttu-id="9b258-253">Kubernetes-Beispiel</span><span class="sxs-lookup"><span data-stu-id="9b258-253">Kubernetes example</span></span>

<span data-ttu-id="9b258-254">Die Verwendung von Bereitschafts- und Lebendigkeitstests ist in Umgebungen wie [Kubernetes](https://kubernetes.io/) sehr nützlich.</span><span class="sxs-lookup"><span data-stu-id="9b258-254">Using separate readiness and liveness checks is useful in an environment such as [Kubernetes](https://kubernetes.io/).</span></span> <span data-ttu-id="9b258-255">In Kubernetes muss eine App möglicherweise zeitaufwendige Startaufgaben ausführen, bevor sie Anforderungen wie z.B. das Testen der Verfügbarkeit der zugrunde liegenden Datenbank annehmen kann.</span><span class="sxs-lookup"><span data-stu-id="9b258-255">In Kubernetes, an app might be required to perform time-consuming startup work before accepting requests, such as a test of the underlying database availability.</span></span> <span data-ttu-id="9b258-256">Durch Verwendung separater Überprüfungen kann der Orchestrator unterscheiden, ob eine App funktioniert, aber noch nicht bereit ist, oder ob die App nicht gestartet wurde.</span><span class="sxs-lookup"><span data-stu-id="9b258-256">Using separate checks allows the orchestrator to distinguish whether the app is functioning but not yet ready or if the app has failed to start.</span></span> <span data-ttu-id="9b258-257">Weitere Informationen zu Bereitschafts- und Lebendigkeitstests in Kubernetes finden Sie in der Kubernetes-Dokumentation unter [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) (Konfigurieren von Lebendigkeits- und Bereitschaftstests).</span><span class="sxs-lookup"><span data-stu-id="9b258-257">For more information on readiness and liveness probes in Kubernetes, see [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) in the Kubernetes documentation.</span></span>

<span data-ttu-id="9b258-258">Das folgende Beispiel veranschaulicht die Konfiguration eines Bereitschaftstests in Kubernetes:</span><span class="sxs-lookup"><span data-stu-id="9b258-258">The following example demonstrates a Kubernetes readiness probe configuration:</span></span>

```
spec:
  template:
  spec:
    readinessProbe:
      # an http probe
      httpGet:
        path: /health/ready
        port: 80
      # length of time to wait for a pod to initialize
      # after pod startup, before applying health checking
      initialDelaySeconds: 30
      timeoutSeconds: 1
    ports:
      - containerPort: 80
```

## <a name="metric-based-probe-with-a-custom-response-writer"></a><span data-ttu-id="9b258-259">Metrikbasierter Test mit einem benutzerdefinierten Antwortwriter</span><span class="sxs-lookup"><span data-stu-id="9b258-259">Metric-based probe with a custom response writer</span></span>

<span data-ttu-id="9b258-260">Die Beispiel-App veranschaulicht eine Arbeitsspeicher-Integritätsprüfung mit einem benutzerdefinierten Antwortwriter.</span><span class="sxs-lookup"><span data-stu-id="9b258-260">The sample app demonstrates a memory health check with a custom response writer.</span></span>

<span data-ttu-id="9b258-261">`MemoryHealthCheck` meldet einen beeinträchtigten Status, wenn die App mehr Arbeitsspeicher verwendet, als für den Schwellenwert festgelegt wurde (1 GB in der Beispiel-App).</span><span class="sxs-lookup"><span data-stu-id="9b258-261">`MemoryHealthCheck` reports a degraded status if the app uses more than a given threshold of memory (1 GB in the sample app).</span></span> <span data-ttu-id="9b258-262">Das `HealthCheckResult` enthält Garbage Collector-Informationen (GC) für die App (*MemoryHealthCheck.cs*):</span><span class="sxs-lookup"><span data-stu-id="9b258-262">The `HealthCheckResult` includes Garbage Collector (GC) information for the app (*MemoryHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

<span data-ttu-id="9b258-263">Registrieren Sie Integritätsprüfungsdienste mit `AddHealthChecks` in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9b258-263">Register health check services with `AddHealthChecks` in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="9b258-264">Die Integritätsprüfung wird nicht durch Übergabe an `AddCheck` aktiviert, stattdessen wird `MemoryHealthCheck` als Dienst registriert.</span><span class="sxs-lookup"><span data-stu-id="9b258-264">Instead of enabling the health check by passing it to `AddCheck`, the `MemoryHealthCheck` is registered as a service.</span></span> <span data-ttu-id="9b258-265">Alle bei `IHealthCheck` registrierten Dienste stehen für die Dienste und Middleware für Integritätsprüfungen zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="9b258-265">All `IHealthCheck` registered services are available to the health check services and middleware.</span></span> <span data-ttu-id="9b258-266">Es wird empfohlen, Integritätsprüfungsdienste als Singleton-Dienste zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="9b258-266">We recommend registering health check services as Singleton services.</span></span>

<span data-ttu-id="9b258-267">*CustomWriterStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="9b258-267">*CustomWriterStartup.cs*:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="9b258-268">Rufen Sie die Middleware für Integritätsprüfungen in der App-Verarbeitungspipeline in `Startup.Configure` auf.</span><span class="sxs-lookup"><span data-stu-id="9b258-268">Call Health Check Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="9b258-269">Ein `WriteResponse`-Delegat wird in der `ResponseWriter`-Eigenschaft angegeben, um eine benutzerdefinierte JSON-Antwort auszugeben, wenn die Integritätsprüfung ausgeführt wird:</span><span class="sxs-lookup"><span data-stu-id="9b258-269">A `WriteResponse` delegate is provided to the `ResponseWriter` property to output a custom JSON response when the health check executes:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_Configure&highlight=6)]

<span data-ttu-id="9b258-270">Die `WriteResponse`-Methode formatiert das `CompositeHealthCheckResult` als JSON-Objekt und führt zu folgender JSON-Ausgabe für die Integritätsprüfungsantwort:</span><span class="sxs-lookup"><span data-stu-id="9b258-270">The `WriteResponse` method formats the `CompositeHealthCheckResult` into a JSON object and yields JSON output for the health check response:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse)]

<span data-ttu-id="9b258-271">Um den metrikbasierten Test mit benutzerdefinierter Ausgabe des Antwortwriters mithilfe der Beispiel-App auszuführen, verwenden Sie den folgenden Befehl aus dem Ordner des Projekts in einer Befehlsshell:</span><span class="sxs-lookup"><span data-stu-id="9b258-271">To run the metric-based probe with custom response writer output using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario writer
```

> [!NOTE]
> <span data-ttu-id="9b258-272">[BeatPulse](https://github.com/Xabaril/BeatPulse) enthält Szenarien für metrikbasierte Integritätsprüfungen, einschließlich Prüfungen des Datenträgerspeichers und Lebendigkeitsprüfungen mit Maximalwert.</span><span class="sxs-lookup"><span data-stu-id="9b258-272">[BeatPulse](https://github.com/Xabaril/BeatPulse) includes metric-based health check scenarios, including disk storage and maximum value liveness checks.</span></span>
>
> <span data-ttu-id="9b258-273">[BeatPulse](https://github.com/Xabaril/BeatPulse) wird von Microsoft nicht verwaltet oder unterstützt.</span><span class="sxs-lookup"><span data-stu-id="9b258-273">[BeatPulse](https://github.com/Xabaril/BeatPulse) isn't maintained or supported by Microsoft.</span></span>

## <a name="filter-by-port"></a><span data-ttu-id="9b258-274">Filtern nach Port</span><span class="sxs-lookup"><span data-stu-id="9b258-274">Filter by port</span></span>

<span data-ttu-id="9b258-275">Wenn `UseHealthChecks` mit einem Port aufgerufen wird, werden Anforderungen durch Integritätsprüfungen auf den angegebenen Port beschränkt.</span><span class="sxs-lookup"><span data-stu-id="9b258-275">Calling `UseHealthChecks` with a port restricts health check requests to the port specified.</span></span> <span data-ttu-id="9b258-276">Dieses Vorgehen wird normalerweise in einer Containerumgebung angewendet, um einen Port für Überwachungsdienste verfügbar zu machen.</span><span class="sxs-lookup"><span data-stu-id="9b258-276">This is typically used in a container environment to expose a port for monitoring services.</span></span>

<span data-ttu-id="9b258-277">Die Beispiel-App konfiguriert den Port mithilfe des [Umgebungsvariablen-Konfigurationsanbieters](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="9b258-277">The sample app configures the port using the [Environment Variable Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span> <span data-ttu-id="9b258-278">Der Port wird in der Datei *launchSettings.json* festgelegt und über eine Umgebungsvariable an den Konfigurationsanbieter übergeben.</span><span class="sxs-lookup"><span data-stu-id="9b258-278">The port is set in the *launchSettings.json* file and passed to the configuration provider via an environment variable.</span></span> <span data-ttu-id="9b258-279">Sie müssen den Server auch so konfigurieren, dass er am Verwaltungsport auf Anforderungen lauscht.</span><span class="sxs-lookup"><span data-stu-id="9b258-279">You must also configure the server to listen to requests on the management port.</span></span>

<span data-ttu-id="9b258-280">Um die Beispiel-App zum Veranschaulichen der Konfiguration des Verwaltungsports zu verwenden, erstellen Sie die Datei *launchSettings.json* in einem *Eigenschaften*-Ordner.</span><span class="sxs-lookup"><span data-stu-id="9b258-280">To use the sample app to demonstrate management port configuration, create the *launchSettings.json* file in a *Properties* folder.</span></span>

<span data-ttu-id="9b258-281">Die folgende *launchSettings.json* ist in den Projektdateien der Beispiel-App nicht vorhanden und muss manuell erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="9b258-281">The following *launchSettings.json* file isn't included in the sample app's project files and must be created manually.</span></span>

<span data-ttu-id="9b258-282">*Properties/launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="9b258-282">*Properties/launchSettings.json*:</span></span>

```json
{
  "profiles": {
    "SampleApp": {
      "commandName": "Project",
      "commandLineArgs": "",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development",
        "ASPNETCORE_URLS": "http://localhost:5000/;http://localhost:5001/",
        "ASPNETCORE_MANAGEMENTPORT": "5001"
      },
      "applicationUrl": "http://localhost:5000/"
    }
  }
}
```

<span data-ttu-id="9b258-283">Registrieren Sie Integritätsprüfungsdienste mit `AddHealthChecks` in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9b258-283">Register health check services with `AddHealthChecks` in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="9b258-284">Der Aufruf von `UseHealthChecks` gibt den Verwaltungsport an (*ManagementPortStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="9b258-284">The call to `UseHealthChecks` specifies the management port (*ManagementPortStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ManagementPortStartup.cs?name=snippet1&highlight=12,18)]

> [!NOTE]
> <span data-ttu-id="9b258-285">Sie können es vermeiden, die Datei *launchSettings.json* in der Beispiel-App erstellen zu müssen, indem Sie die URLs und den Verwaltungsport explizit im Code festlegen.</span><span class="sxs-lookup"><span data-stu-id="9b258-285">You can avoid creating the *launchSettings.json* file in the sample app by setting the URLs and management port explicitly in code.</span></span> <span data-ttu-id="9b258-286">Fügen Sie in *Program.cs* an der Stelle, an der `WebHostBuilder` erstellt wird, einen Aufruf von `UseUrls` hinzu, und geben Sie den normalen Antwortendpunkt der App und den Endpunkt des Verwaltungsports an.</span><span class="sxs-lookup"><span data-stu-id="9b258-286">In *Program.cs* where the `WebHostBuilder` is created, add a call to `UseUrls` and provide the app's normal response endpoint and the management port endpoint.</span></span> <span data-ttu-id="9b258-287">Geben Sie in *ManagementPortStartup.cs* an der Stelle, an der `UseHealthChecks` aufgerufen wird, explizit den Verwaltungsport an.</span><span class="sxs-lookup"><span data-stu-id="9b258-287">In *ManagementPortStartup.cs* where `UseHealthChecks` is called, specify the management port explicitly.</span></span>
>
> <span data-ttu-id="9b258-288">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="9b258-288">*Program.cs*:</span></span>
>
> ```csharp
> return new WebHostBuilder()
>     .UseConfiguration(config)
>     .UseUrls("http://localhost:5000/;http://localhost:5001/")
>     .ConfigureLogging(builder =>
>     {
>         builder.SetMinimumLevel(LogLevel.Trace);
>         builder.AddConfiguration(config);
>         builder.AddConsole();
>     })
>     .UseKestrel()
>     .UseStartup(startupType)
>     .Build();
> ```
>
> <span data-ttu-id="9b258-289">*ManagementPortStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="9b258-289">*ManagementPortStartup.cs*:</span></span>
>
> ```csharp
> app.UseHealthChecks("/health", port: 5001);
> ```

<span data-ttu-id="9b258-290">Um das Szenario für die Konfiguration des Verwaltungsports mithilfe der Beispiel-App auszuführen, verwenden Sie den folgenden Befehl aus dem Ordner des Projekts in einer Befehlsshell:</span><span class="sxs-lookup"><span data-stu-id="9b258-290">To run the management port configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a><span data-ttu-id="9b258-291">Verteilen einer Integritätsprüfungsbibliothek</span><span class="sxs-lookup"><span data-stu-id="9b258-291">Distribute a health check library</span></span>

<span data-ttu-id="9b258-292">So verteilen Sie eine Integritätsprüfung als Bibliothek:</span><span class="sxs-lookup"><span data-stu-id="9b258-292">To distribute a health check as a library:</span></span>

1. <span data-ttu-id="9b258-293">Schreiben Sie eine Integritätsprüfung, die die `IHealthCheck`-Schnittstelle als eigenständige Klasse implementiert.</span><span class="sxs-lookup"><span data-stu-id="9b258-293">Write a health check that implements the `IHealthCheck` interface as a standalone class.</span></span> <span data-ttu-id="9b258-294">Die Klasse kann [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) (Dependency Injection, DI), Typaktivierung und [benannte Optionen](xref:fundamentals/configuration/options) verwenden, um auf Konfigurationsdaten zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="9b258-294">The class can rely on [dependency injection (DI)](xref:fundamentals/dependency-injection), type activation, and [named options](xref:fundamentals/configuration/options) to access configuration data.</span></span>

   ```csharp
   using System;
   using System.Threading;
   using System.Threading.Tasks;
   using Microsoft.Extensions.Diagnostics.HealthChecks;

   namespace SampleApp
   {
       public class ExampleHealthCheck : IHealthCheck
       {
           private readonly string _data1;
           private readonly int? _data2;

           public ExampleHealthCheck(string data1, int? data2)
           {
               _data1 = data1 ?? throw new ArgumentNullException(nameof(data1));
               _data2 = data2 ?? throw new ArgumentNullException(nameof(data2));
           }

           public async Task<HealthCheckResult> CheckHealthAsync(
               HealthCheckContext context, CancellationToken cancellationToken)
           {
               try
               {
                   // Health check logic
                   //
                   // data1 and data2 are used in the method to
                   // run the probe's health check logic.

                   // Assume that it's possible for this health check
                   // to throw an AccessViolationException.

                   return HealthCheckResult.Healthy();
               }
               catch (AccessViolationException ex)
               {
                   return new HealthCheckResult(
                       context.Registration.FailureStatus,
                       description: "An access violation occurred during the check.",
                       exception: ex,
                       data: null);
               }
           }
       }
   }
   ```

1. <span data-ttu-id="9b258-295">Schreiben Sie eine Erweiterungsmethode mit Parametern, die von der nutzenden App in ihrer `Startup.Configure`-Methode aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="9b258-295">Write an extension method with parameters that the consuming app calls in its `Startup.Configure` method.</span></span> <span data-ttu-id="9b258-296">Nehmen Sie im folgenden Beispiel die folgende Signatur für die Integritätsprüfungsmethode an:</span><span class="sxs-lookup"><span data-stu-id="9b258-296">In the following example, assume the following health check method signature:</span></span>

   ```csharp
   ExampleHealthCheck(string, string, int )
   ```

   <span data-ttu-id="9b258-297">Die oben stehende Signatur gibt an, dass `ExampleHealthCheck` weitere Daten benötigt, um die Testlogik für die Integritätsprüfung zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="9b258-297">The preceding signature indicates that the `ExampleHealthCheck` requires additional data to process the health check probe logic.</span></span> <span data-ttu-id="9b258-298">Die Daten werden für den Delegaten bereitgestellt, der zum Erstellen der Integritätsprüfungsinstanz verwendet wird, wenn die Integritätsprüfung bei einer Erweiterungsmethode registriert wird.</span><span class="sxs-lookup"><span data-stu-id="9b258-298">The data is provided to the delegate used to create the health check instance when the health check is registered with an extension method.</span></span> <span data-ttu-id="9b258-299">Im folgenden Beispiel gibt der Aufrufer optional Folgendes an:</span><span class="sxs-lookup"><span data-stu-id="9b258-299">In the following example, the caller specifies optional:</span></span>

   * <span data-ttu-id="9b258-300">Name der Integritätsprüfung (`name`).</span><span class="sxs-lookup"><span data-stu-id="9b258-300">health check name (`name`).</span></span> <span data-ttu-id="9b258-301">Wenn der Wert `null` ist, wird `example_health_check` verwendet.</span><span class="sxs-lookup"><span data-stu-id="9b258-301">If `null`, `example_health_check` is used.</span></span>
   * <span data-ttu-id="9b258-302">string-Datenpunkt für die Integritätsprüfung (`data1`).</span><span class="sxs-lookup"><span data-stu-id="9b258-302">string data point for the health check (`data1`).</span></span>
   * <span data-ttu-id="9b258-303">integer-Datenpunkt für die Integritätsprüfung (`data2`).</span><span class="sxs-lookup"><span data-stu-id="9b258-303">integer data point for the health check (`data2`).</span></span> <span data-ttu-id="9b258-304">Wenn der Wert `null` ist, wird `1` verwendet.</span><span class="sxs-lookup"><span data-stu-id="9b258-304">If `null`, `1` is used.</span></span>
   * <span data-ttu-id="9b258-305">Fehlerstatus (`HealthStatus`).</span><span class="sxs-lookup"><span data-stu-id="9b258-305">failure status (`HealthStatus`).</span></span> <span data-ttu-id="9b258-306">Die Standardeinstellung ist `null`.</span><span class="sxs-lookup"><span data-stu-id="9b258-306">The default is `null`.</span></span> <span data-ttu-id="9b258-307">Wenn der Wert `null` ist, wird `HealthStatus.Unhealthy` für einen Fehlerstatus gemeldet.</span><span class="sxs-lookup"><span data-stu-id="9b258-307">If `null`, `HealthStatus.Unhealthy` is reported for a failure status.</span></span>
   * <span data-ttu-id="9b258-308">Tags (`IEnumerable<string>`).</span><span class="sxs-lookup"><span data-stu-id="9b258-308">tags (`IEnumerable<string>`).</span></span>

   ```csharp
   using System.Collections.Generic;
   using Microsoft.Extensions.Diagnostics.HealthChecks;

   public static class ExampleHealthCheckBuilderExtensions
   {
       const string NAME = "example_health_check";

       public static IHealthChecksBuilder AddExampleHealthCheck(
           this IHealthChecksBuilder builder, 
           string name = default, 
           string data1, 
           int data2 = 1, 
           HealthStatus? failureStatus = default, 
           IEnumerable<string> tags = default)
       {
           return builder.Add(new HealthCheckRegistration(
               name ?? NAME,
               sp => new ExampleHealthCheck(data1, data2),
               failureStatus,
               tags));
       }
   }
   ```

## <a name="health-check-publisher"></a><span data-ttu-id="9b258-309">Herausgeber der Integritätsprüfung</span><span class="sxs-lookup"><span data-stu-id="9b258-309">Health Check Publisher</span></span>

<span data-ttu-id="9b258-310">Wenn dem Dienstcontainer ein `IHealthCheckPublisher` hinzugefügt wird, führt das Integritätsprüfungssystem Ihre Integritätsprüfungen regelmäßig aus und ruft `PublishAsync` mit dem Ergebnis auf.</span><span class="sxs-lookup"><span data-stu-id="9b258-310">When an `IHealthCheckPublisher` is added to the service container, the health check system periodically executes your health checks and calls `PublishAsync` with the result.</span></span> <span data-ttu-id="9b258-311">Dies ist nützlich in einem Szenario mit pushbasiertem Integritätsüberwachungssystem, in dem jeder Prozess das Überwachungssystem regelmäßig aufrufen muss, um die Integrität zu bestimmen.</span><span class="sxs-lookup"><span data-stu-id="9b258-311">This is useful in a push-based health monitoring system scenario that expects each process to call the monitoring system periodically in order to determine health.</span></span>

<span data-ttu-id="9b258-312">Die `IHealthCheckPublisher`-Schnittstelle weist eine einzige Methode auf:</span><span class="sxs-lookup"><span data-stu-id="9b258-312">The `IHealthCheckPublisher` interface has a single method:</span></span>

```csharp
Task PublishAsync(HealthReport report, CancellationToken cancellationToken);
```

> [!NOTE]
> <span data-ttu-id="9b258-313">[BeatPulse](https://github.com/Xabaril/BeatPulse) enthält Herausgeber für verschiedene Systeme, einschließlich [Application Insights](/azure/application-insights/app-insights-overview).</span><span class="sxs-lookup"><span data-stu-id="9b258-313">[BeatPulse](https://github.com/Xabaril/BeatPulse) includes publishers for several systems, including [Application Insights](/azure/application-insights/app-insights-overview).</span></span>
>
> <span data-ttu-id="9b258-314">[BeatPulse](https://github.com/Xabaril/BeatPulse) wird von Microsoft nicht verwaltet oder unterstützt.</span><span class="sxs-lookup"><span data-stu-id="9b258-314">[BeatPulse](https://github.com/Xabaril/BeatPulse) isn't maintained or supported by Microsoft.</span></span>
