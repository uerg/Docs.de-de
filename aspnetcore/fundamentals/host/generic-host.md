---
title: Generischer .NET-Host
author: guardrex
description: Erfahren Sie mehr über den generischen Host in .NET, der für das Starten der App und das Verwalten der Lebensdauer verantwortlich ist.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/generic-host
ms.openlocfilehash: 15ce81a4226921ce053096751d7678ada36235c0
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34728972"
---
# <a name="net-generic-host"></a>Generischer .NET-Host

Von [Luke Latham](https://github.com/guardrex)

.NET-Apps konfigurieren und starten einen *Host*. Der Host ist verantwortlich für das Starten der App und das Verwalten der Lebensdauer. In diesem Artikel wird der generische ASP.NET Core-Host ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)) erläutert, der für das Hosten von Apps nützlich ist, die keine HTTP-Anforderungen verarbeiten. Weitere Informationen zum Webhost ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)) finden Sie im Artikel [Webhost](xref:fundamentals/host/web-host).

Das Ziel des generischen Hosts besteht darin, die HTTP-Pipeline von der Webhost-API zu entkoppeln, um mehr Szenarios zu ermöglichen. Messaging, Hintergrundtasks und andere Nicht-HTTP-Workloads, die auf dem generischen Host basieren, profitieren von übergreifenden Funktionen wie der Konfiguration, Dependency Injection (DI) und der Protokollerstellung.

Der generische Host ist neu in ASP.NET Core 2.1 und ist nicht für Webhostingszenarios geeignet. Verwenden Sie den [Webhost](xref:fundamentals/host/web-host) für Webhostingszenarios. Der generische Host wird dafür entwickelt, den Webhost in einem zukünftigen Release zu ersetzen und als primäre Host-API in HTTP- und Nicht-HTTP-Szenarios zu fungieren.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

Wenn Sie die Beispiel-App in [Visual Studio Code](https://code.visualstudio.com/) ausführen, verwenden Sie ein *externes oder integriertes Terminal*. Führen Sie das Beispiel nicht in `internalConsole` aus.

So legen Sie die Konsole in Visual Studio Code fest:

1. Öffnen Sie die Datei *.vscode/launch.json*.
1. Suchen Sie in der Konfiguration **.NET Core-Start (Konsole)** den Eintrag **Konsole**. Legen Sie den Wert auf `externalTerminal` oder `integratedTerminal` fest.

## <a name="introduction"></a>Einführung

Die generische Hostbibliothek ist im Namespace [Microsoft.Extensions.Hosting](/dotnet/api/microsoft.extensions.hosting) verfügbar und wird vom NuGet-Paket [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) bereitgestellt. Das Paket `Microsoft.Extensions.Hosting` ist im Metapaket [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) enthalten.

[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) ist der Einstiegspunkt für die Ausführung des Codes. Jede Implementierung von `IHostedService` wird in der Reihenfolge der [Dienstregistrierung in ConfigureServices](#configureservices) ausgeführt. [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) wird in jeder `IHostedService`-Schnittstelle aufgerufen, wenn der Host gestartet wird, und [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) wird in umgekehrter Registrierungsreihenfolge aufgerufen, wenn der Host ordnungsgemäß heruntergefahren wird.

## <a name="set-up-a-host"></a>Einrichten eines Hosts

[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) ist die Hauptkomponente, die Bibliotheken und Apps für die Initialisierung, Erstellung und Ausführung des Hosts verwenden:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="host-configuration"></a>Konfiguration des Hosts

[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) basiert auf folgenden Ansätzen, um die Hostkonfigurationswerte festzulegen:

* Konfigurations-Generator
* Konfiguration der Erweiterungsmethode

### <a name="configuration-builder"></a>Konfigurations-Generator

Die Konfiguration des Host-Generators wird erstellt, indem [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) aus der Implementierung von [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) abgerufen wird. `ConfigureHostConfiguration` verwendet eine [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder)-Schnittstelle, um eine [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration)-Schnittstelle für den Host zu erstellen. Der Konfigurations-Generator initialisiert die [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment)-Schnittstelle, um diese im Buildprozess der App zu verwenden. `ConfigureHostConfiguration` kann mehrfach mit additiven Ergebnissen aufgerufen werden. Der Host verwendet die Option, die zuletzt einen Wert festlegt.

*hostsettings.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

Beispielkonfiguration für `HostBuilder` mithilfe von `ConfigureHostConfiguration`:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

> [!NOTE]
> Die Erweiterungsmethode [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) kann derzeit keine Konfigurationsabschnitte (z.B. `.AddConfiguration(Configuration.GetSection("section"))`) analysieren, die von [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) zurückgegeben werden. Die `GetSection`-Methode filtert die Konfigurationsschlüssel des angeforderten Abschnitts, behält jedoch den Abschnittsnamen in den Schlüsseln bei (z.B. `section:environment`). Die `AddConfiguration`-Methode erwartet, dass die Schlüssel den `HostBuilder`-Schlüsseln entsprechen (z.B. `environment`). Das Vorhandensein des Abschnittsnamens in den Schlüsseln verhindert, dass die Werte des Abschnitts den Host konfigurieren. Dieses Problem wird in einer bevorstehenden Version behoben. Weitere Informationen und Problemumgehungen finden Sie unter [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys (Beim Übergeben des Konfigurationsabschnitts an WebHostBuilder.UseConfiguration werden vollständige Schlüssel verwendet)](https://github.com/aspnet/Hosting/issues/839).

### <a name="extension-method-configuration"></a>Konfiguration der Erweiterungsmethode

Erweiterungsmethoden werden aus der Implementierung von `IHostBuilder` abgerufen, um das Inhaltsstammverzeichnis und die Umgebung zu konfigurieren.

#### <a name="content-root"></a>Inhaltsstammverzeichnis

Diese Einstellung bestimmt, wo der Host mit der Suche nach Inhaltsdateien beginnt.

**Schlüssel:** contentRoot  
**Typ:** *Zeichenfolge*  
**Standard:** Entspricht standardmäßig dem Ordner, in dem die App-Assembly gespeichert ist.  
**Festlegen mit:** `UseContentRoot`  
**Umgebungsvariable:** `ASPNETCORE_CONTENTROOT`

Wenn der Pfad nicht vorhanden ist, kann der Host nicht gestartet werden.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

#### <a name="environment"></a>Umgebung

Legt die [Umgebung](xref:fundamentals/environments) der App fest.

**Schlüssel:** environment  
**Typ:** *Zeichenfolge*  
**Standard:** Produktion  
**Festlegen mit:** `UseEnvironment`  
**Umgebungsvariable:** `ASPNETCORE_ENVIRONMENT`

Die Umgebung kann auf einen beliebigen Wert festgelegt werden. Zu den durch Frameworks definierten Werten zählen `Development`, `Staging` und `Production`. Die Groß-/Kleinschreibung wird bei Werten nicht beachtet. Standardmäßig wird die *Umgebung* aus der `ASPNETCORE_ENVIRONMENT`-Umgebungsvariablen gelesen. Wenn Sie [Visual Studio](https://www.visualstudio.com/) verwenden, werden Umgebungsvariablen möglicherweise in der Datei *launchSettings.json* festgelegt. Weitere Informationen finden Sie unter [Verwenden mehrerer Umgebungen](xref:fundamentals/environments).

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

## <a name="configureappconfiguration"></a>ConfigureAppConfiguration

Die Konfiguration des App-Generators wird erstellt, indem [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) aus der Implementierung von [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) abgerufen wird. `ConfigureAppConfiguration` verwendet eine [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder)-Schnittstelle, um eine [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration)-Schnittstelle für die App zu erstellen. `ConfigureAppConfiguration` kann mehrfach mit additiven Ergebnissen aufgerufen werden. Die App verwendet die Option, die zuletzt einen Wert festlegt. Die von `ConfigureAppConfiguration` erstellte Konfiguration ist unter [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) verfügbar und kann für nachfolgende Vorgänge und in [Diensten](/dotnet/api/microsoft.extensions.hosting.ihost.services) verwendet werden.

Beispielkonfiguration für die App mithilfe von `ConfigureAppConfiguration`:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

*appsettings.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

*appsettings.Development.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

*appsettings.Production.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

> [!NOTE]
> Die Erweiterungsmethode [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) kann derzeit keine Konfigurationsabschnitte (z.B. `.AddConfiguration(Configuration.GetSection("section"))`) analysieren, die von [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) zurückgegeben werden. Die `GetSection`-Methode filtert die Konfigurationsschlüssel des angeforderten Abschnitts, behält jedoch den Abschnittsnamen in den Schlüsseln bei (z.B. `section:Logging:LogLevel:Default`). Die `AddConfiguration`-Methode erwartet eine genaue Übereinstimmung mit den Konfigurationsschlüsseln (z.B. `Logging:LogLevel:Default`). Das Vorhandensein des Abschnittsnamens in den Schlüsseln verhindert, dass die Werte des Abschnitts die App konfigurieren. Dieses Problem wird in einer bevorstehenden Version behoben. Weitere Informationen und Problemumgehungen finden Sie unter [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys (Beim Übergeben des Konfigurationsabschnitts an WebHostBuilder.UseConfiguration werden vollständige Schlüssel verwendet)](https://github.com/aspnet/Hosting/issues/839).

## <a name="configureservices"></a>ConfigureServices

Mithilfe von [ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) werden Dienste zum [Dependency Injection](xref:fundamentals/dependency-injection)-Container der App hinzugefügt. `ConfigureServices` kann mehrfach mit additiven Ergebnissen aufgerufen werden.

Ein gehosteter Dienst ist eine Klasse mit Logik für Hintergrundaufgaben, die die Schnittstelle [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) implementiert. Weitere Informationen finden Sie im Artikel [Hintergrundtasks mit gehosteten Diensten](xref:fundamentals/host/hosted-services).

Die [Beispiel-App](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) verwendet die Erweiterungsmethode `AddHostedService`, um einen Dienst für Lebensdauerereignisse (`LifetimeEventsHostedService`) und einen zeitgesteuerten Hintergrundtask (`TimedHostedService`) zur App hinzuzufügen:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a>ConfigureLogging

Durch [ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) wird ein Delegat für die Konfiguration der bereitgestellten [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder)-Schnittstelle bereitgestellt. `ConfigureLogging` kann mehrfach mit additiven Ergebnissen aufgerufen werden.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a>UseConsoleLifetime

[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) wartet auf `Ctrl+C`/SIGINT oder SIGTERM und ruft [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) auf, um das Herunterfahren zu beginnen. `UseConsoleLifetime` hebt die Blockierung von Erweiterungen wie [RunAsync](#runasync) und [WaitForShutdownAsync](#waitforshutdownasync) auf. [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) ist vorab als Standardimplementierung für die Lebensdauer registriert. Die letzte registrierte Lebensdauer wird verwendet.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a>Containerkonfiguration

Der Host kann eine [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1)-Schnittstelle verwenden, um die Integration in andere Container zu unterstützten. Das Bereitstellen einer Factory ist kein Teil der DI-Containerregistrierung, sondern ein Host, der systemintern verwendet wird, um den entsprechenden DI-Container zu erstellen. [UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) überschreibt die Standardfactory, die zum Erstellen des Dienstanbieters der App verwendet wurde.

Die benutzerdefinierte Containerkonfiguration wird von der Methode [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) verwaltet. `ConfigureContainer` stellt eine stark typisierte Möglichkeit für das Konfigurieren des Containers auf Basis der zugrunde liegenden Host-API bereit. `ConfigureContainer` kann mehrfach mit additiven Ergebnissen aufgerufen werden.

Erstellen eines Dienstcontainers für die App:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

Bereitstellen einer Factory für den Dienstcontainer:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

Verwenden Sie die Factory, und konfigurieren Sie den benutzerdefinierten Dienstcontainer für die App:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a>Erweiterungen

Die Erweiterung des Hosts wird mit der Erweiterungsmethode in `IHostBuilder` durchgeführt. Im folgenden Beispiel wird dargestellt, wie eine Erweiterungsmethode eine `IHostBuilder`-Implementierung mit [RabbitMQ](https://www.rabbitmq.com/) erweitert. Die Erweiterungsmethode (an anderer Stelle in der App) registriert eine `IHostedService`-Schnittstelle von RabbitMQ:

```csharp
// UseRabbitMq is an extension method that sets up RabbitMQ to handle incoming
// messages.
var host = new HostBuilder()
    .UseRabbitMq<MyMessageHandler>()
    .Build();

await host.StartAsync();
```

## <a name="manage-the-host"></a>Verwalten des Hosts

Die Implementierung von [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) ist für das Starten und Beenden der Implementierungen von `IHostedService` verantwortlich, die im Dienstcontainer registriert sind.

### <a name="run"></a>Run

Durch [Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) wird die App gestartet und der aufrufende Thread blockiert, bis der Host heruntergefahren wird:

```csharp
public class Program
{
    public void Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        host.Run();
    }
}
```

### <a name="runasync"></a>RunAsync

Durch [RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) wird die App ausgeführt und eine `Task`-Methode ausgegeben, die abgeschlossen wird, wenn das Abbruchtoken oder das Herunterfahren ausgelöst wird:

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        await host.RunAsync();
    }
}
```

### <a name="runconsoleasync"></a>RunConsoleAsync

[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) aktiviert die Unterstützung der Konsole, erstellt und startet den Host und wartet auf `Ctrl+C`/SIGINT oder SIGTERM, um das Herunterfahren auszulösen.

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var hostBuilder = new HostBuilder();

        await hostBuilder.RunConsoleAsync();
    }
}
```

### <a name="start-and-stopasync"></a>Start und StopAsync

[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) startet den Host synchron.

[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) versucht, den Host innerhalb des angegebenen Timeouts zu beenden.

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            host.Start();

            await host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

### <a name="startasync-and-stopasync"></a>StartAsync und StopAsync

[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) startet die App.

[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) stoppt die App.

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            await host.StartAsync();

            await host.StopAsync();
        }
    }
}
```

### <a name="waitforshutdown"></a>WaitForShutdown

[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) wird über die [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime)-Schnittstelle ausgelöst (z.B. die [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime)-Klasse, die auf `Ctrl+C`/SIGINT oder SIGTERM wartet). `WaitForShutdown` ruft [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) auf.

```csharp
public class Program
{
    public void Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            host.Start();

            host.WaitForShutdown();
        }
    }
}
```

### <a name="waitforshutdownasync"></a>WaitForShutdownAsync

[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) gibt eine `Task`-Methode zurück, die abgeschlossen wird, wenn das Herunterfahren über das angegebene Token ausgelöst wird, und ruft [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) auf.

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            await host.StartAsync();

            await host.WaitForShutdownAsync();
        }

    }
}
```

### <a name="external-control"></a>Externe Steuerung

Die externe Steuerung des Hosts kann mithilfe von Methoden durchgeführt werden, die extern aufgerufen werden können:

```csharp
public class Program
{
    private IHost _host;

    public Program()
    {
        _host = new HostBuilder()
            .Build();
    }

    public async Task StartAsync()
    {
        _host.StartAsync();
    }

    public async Task StopAsync()
    {
        using (_host)
        {
            await _host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) wird am Anfang der [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync)-Methode aufgerufen, die den Vorgang erst fortsetzt, wenn sie abgeschlossen ist. Dies kann verwendet werden, um den Start zu verzögern, bis dieser durch ein externes Ereignis initiiert wird.

## <a name="ihostingenvironment-interface"></a>IHostingEnvironment-Schnittstelle

[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) stellt Informationen über die Hostingumgebung der App bereit. Verwenden Sie [Constructor Injection](xref:fundamentals/dependency-injection) zum Abrufen der `IHostingEnvironment`-Schnittstelle, um deren Eigenschaften und Erweiterungsmethoden zu verwenden:

```csharp
public class MyClass
{
    private readonly IHostingEnvironment _env;

    public MyClass(IHostingEnvironment env)
    {
        _env = env;
    }

    public void DoSomething()
    {
        var environmentName = _env.EnvironmentName;
    }
}
```

Weitere Informationen finden Sie unter [Verwenden mehrerer Umgebungen](xref:fundamentals/environments).

## <a name="iapplicationlifetime-interface"></a>IApplicationLifetime-Schnittstelle

[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) ermöglicht Aktivitäten nach dem Starten und beim Herunterfahren (einschließlich Anforderungen für ordnungsgemäßes Herunterfahren). Bei drei der Eigenschaften der Schnittstelle handelt es sich um Abbruchtokens, die zum Registrieren von `Action`-Methoden verwendet werden, durch die Ereignisse beim Starten und Herunterfahren definiert werden.

| Abbruchtoken | Wird ausgelöst, wenn&#8230; |
| ------------------ | --------------------- |
| [ApplicationStarted](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | Der Host vollständig gestartet wurde. |
| [ApplicationStopped](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | Der Host ein ordnungsgemäßes Herunterfahren abschließt. Alle Anforderungen sollten verarbeitet worden sein. Das Herunterfahren wird blockiert, bis das Ereignis abgeschlossen ist. |
| [ApplicationStopping](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | Der Host ein ordnungsgemäßes Herunterfahren ausführt. Anforderungen möglicherweise noch verarbeitet werden. Das Herunterfahren wird blockiert, bis das Ereignis abgeschlossen ist. |

Fügen Sie den `IApplicationLifetime`-Dienst über einen Konstruktor in eine beliebige Klasse ein. Die [Beispiel-App](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) verwendet die Konstruktorinjektion für eine `LifetimeEventsHostedService`-Klasse (eine `IHostedService`-Implementierung), um die Ereignisse zu registrieren.

*LifetimeEventsHostedService.cs*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) fordert das Beenden der Anwendung an. Die folgende Klasse verwendet `StopApplication`, um eine App ordnungsgemäß herunterzufahren, wenn die `Shutdown`-Methode der Klasse aufgerufen wird:

```csharp
public class MyClass
{
    private readonly IApplicationLifetime _appLifetime;

    public MyClass(IApplicationLifetime appLifetime)
    {
        _appLifetime = appLifetime;
    }

    public void Shutdown()
    {
        _appLifetime.StopApplication();
    }
}
```

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Hintergrundaufgaben mit gehosteten Diensten](xref:fundamentals/host/hosted-services)
* [Hosten von Beispielrepositorys auf GitHub](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
