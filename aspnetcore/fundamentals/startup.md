---
title: Start der Anwendung in ASP.NET Core
author: ardalis
description: Ermitteln Sie, wie die Startklasse in ASP.NET Core Services und die app-Anforderungspipeline konfiguriert.
ms.author: tdykstra
manager: wpickett
ms.custom: mvc
ms.date: 12/08/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/startup
ms.openlocfilehash: dd2eb3d3996bc0bf277c8d5e772c8568ef9f147e
ms.sourcegitcommit: f5a7f0198628f0d152257d90dba6c3a0747a355a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/19/2017
---
# <a name="application-startup-in-aspnet-core"></a>Start der Anwendung in ASP.NET Core

Durch [Steve Smith](https://ardalis.com), [Tom Dykstra](https://github.com/tdykstra), und [Luke Latham](https://github.com/guardrex)

Die `Startup` Klasse konfiguriert werden, Dienste und die app-Anforderungspipeline.

## <a name="the-startup-class"></a>Die Startklasse.

ASP.NET Core-apps verwenden ein `Startup` -Klasse, mit dem Namen `Startup` gemäß der Konvention. Die `Startup` Klasse:

* Kann optional enthalten eine [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) Methode, um die app-Dienste konfigurieren.
* Umfasst eine [konfigurieren](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) Methode, um die app-Anforderungsverarbeitungspipeline zu erstellen.

`ConfigureServices`und `Configure` werden von der Laufzeit aufgerufen, wenn die app gestartet wird:

[!code-csharp[Main](startup/snapshot_sample/Startup1.cs)]

Geben Sie die `Startup` -Klasse mit der [WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt; ](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) Methode:

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=10)]

Die `Startup` Klassenkonstruktor akzeptiert Abhängigkeiten, die vom Host definiert. Eine übliche Verwendung dieser [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) in der `Startup` Klasse wird zum Einfügen von [IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment) Services-Umgebung zu konfigurieren:

[!code-csharp[Main](startup/snapshot_sample/Startup2.cs)]

Eine Alternative zum Räumen `IHostingStartup` ist die Verwendung einen Konventionen basierenden Ansatz. Die app kann separate definieren `Startup` Klassen für unterschiedliche Umgebungen (z. B. `StartupDevelopment`), und die entsprechenden Startklasse zur Laufzeit aktiviert ist. Die Klasse, deren Namensuffix die aktuelle Umgebung entspricht, wird als Priorität angesehen. Wenn die app, die in der Entwicklungsumgebung ausgeführt wird, und sowohl enthält eine `Startup` Klasse und ein `StartupDevelopment` -Klasse, die `StartupDevelopment` Klasse dient. Weitere Informationen finden Sie unter [arbeiten mit mehreren Umgebungen](xref:fundamentals/environments#startup-conventions).

Weitere Informationen zu `WebHostBuilder`, finden Sie unter der [Hosting](xref:fundamentals/hosting) Thema. Informationen zum Behandeln von Fehlern während des Starts finden Sie unter [Start Ausnahmebehandlung](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>Die ConfigureServices-Methode

Die [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) Methode ist:

* Dies ist optional.
* Wird vom Webhost vor der `Configure` Methode, um die app-Dienste konfigurieren.
* Wobei [Konfigurationsoptionen](xref:fundamentals/configuration/index) werden gemäß der Konvention festgelegt.

Hinzufügen von Diensten auf dem Dienstcontainer Benutzerprozessen sowohl innerhalb der app als auch in der `Configure` Methode. Die Dienste sind durch aufgelöst [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) oder [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices).

Die Webhost möglicherweise einige Dienste vor dem Konfigurieren `Startup` Methoden aufgerufen werden. Details finden Sie in der [Hosting](xref:fundamentals/hosting) Thema. 

Für Features, erhebliche Setup erforderlich sind, stehen `Add[Service]` Erweiterungsmethoden in [IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection). Eine typische Web-app registriert Dienste für Entity Framework, Identitäts- und MVC:

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

## <a name="services-available-in-startup"></a>In den Starttasks verfügbaren Dienste

Die Webhost bietet einige Dienste, die zur Verfügung stehen die `Startup` Klassenkonstruktor. Die app fügt zusätzliche Dienste über `ConfigureServices`. Der Host und die app-Dienste stehen dann im `Configure` und in der gesamten Anwendung.

## <a name="the-configure-method"></a>Die Configure-Methode

Die [konfigurieren](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) Methode wird verwendet, um anzugeben, wie die app auf HTTP-Anforderungen antwortet. Durch Hinzufügen die Anforderungspipeline konfiguriert ist [Middleware](xref:fundamentals/middleware) Komponenten einer [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) Instanz. `IApplicationBuilder`steht für die `Configure` -Methode, aber es ist nicht im Dienstcontainer registriert. Hosting erstellt ein `IApplicationBuilder` und übergibt sie direkt zu `Configure` ([Verweisquelle](https://github.com/aspnet/Hosting/blob/release/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/WebHost.cs#L179-L192)).

Die [ASP.NET Core Vorlagen](/dotnet/core/tools/dotnet-new) konfigurieren Sie die Pipeline mit Unterstützung für eine Developer-Ausnahme-Seite [BrowserLink](http://vswebessentials.com/features/browserlink), Fehlerseiten, statische Dateien und ASP.NET MVC:

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Startup.cs?range=28-48&highlight=5,6,10,13,15)]

Jede `Use` Erweiterungsmethode die Pipeline eine middlewarekomponente hinzugefügt. Für die Instanz, die `UseMvc` Erweiterungsmethode fügt die [routing Middleware](xref:fundamentals/routing) auf der Anforderungspipeline und konfiguriert [MVC](xref:mvc/overview) als Standardhandler.

Zusätzliche Dienste, z. B. `IHostingEnvironment` und `ILoggerFactory`, kann auch in der Methodensignatur angegeben werden. Wenn angegeben, werden zusätzliche Dienste eingefügt, wenn sie verfügbar sind.

Weitere Informationen zur Verwendung von `IApplicationBuilder`, finden Sie unter [Middleware](xref:fundamentals/middleware).

## <a name="convenience-methods"></a>Hilfsmethoden

[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices) und [konfigurieren](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) Hilfsmethoden können verwendet werden, angeben, statt eine `Startup` Klasse. Mehrere Aufrufe `ConfigureServices` untereinander anfügen. Mehrere Aufrufe `Configure` den letzten Methodenaufruf verwenden.

[!code-csharp[Main](startup/snapshot_sample/Program.cs?highlight=16,20)]

## <a name="startup-filters"></a>Startfilter

Verwendung [IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) zum Konfigurieren von Middleware am Anfang oder Ende einer app [konfigurieren](#the-configure-method) Middleware-Pipeline. `IStartupFilter`ist nützlich, um sicherzustellen, dass eine Middleware vor oder nach der Middleware hinzugefügt, indem Bibliotheken am Anfang oder Ende der app-Anforderungsverarbeitungspipeline ausgeführt wird.

`IStartupFilter`implementiert eine einzelne Methode [konfigurieren](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure), dem empfängt und gibt ein `Action<IApplicationBuilder>`. Ein [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) definiert eine Klasse, um eine app-Anforderungspipeline konfigurieren. Weitere Informationen finden Sie unter [erstellen eine middlewarepipeline mit IApplicationBuilder](xref:fundamentals/middleware#creating-a-middleware-pipeline-with-iapplicationbuilder).

Jede `IStartupFilter` eine oder mehrere Middlewares in der Anforderungspipeline implementiert. Die Filter werden in der Reihenfolge aufgerufen, wenn, die Sie dem Dienstcontainer hinzugefügt wurden. Filter können Middleware vor dem Hinzufügen oder nach der Steuerung an den nächsten Filter übergeben wird, daher sie anfügen an den Anfang oder Ende der app-Pipeline.

Die [Beispielapp](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/) ([zum Herunterladen von](xref:tutorials/index#how-to-download-a-sample)) wird veranschaulicht, wie eine Middleware mit registrieren `IStartupFilter`. Die Beispiel-app enthält eine Middleware, die einen Wert für die Optionen über einen Abfragezeichenfolgenparameter festlegt:

[!code-csharp[Main](startup/sample/RequestSetOptionsMiddleware.cs?name=snippet1)]

Die `RequestSetOptionsMiddleware` konfiguriert ist, der `RequestSetOptionsStartupFilter` Klasse:

[!code-csharp[Main](startup/sample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

Die `IStartupFilter` ist im Dienstcontainer in registriert `ConfigureServices`:

[!code-csharp[Main](startup/sample/Startup.cs?name=snippet1&highlight=3)]

Wenn ein Abfragezeichenfolgen-Parameter für `option` angegeben wird, die Middleware die Wert-Zuordnung verarbeitet, bevor die MVC-Middleware die Antwort gerendert wird:

![Anzeigen der gerenderten Indexseite Browserfenster. Als "Von Middleware" basierend auf die Seite mit dem Abfragezeichenfolgen-Parameter und den Wert der Option auf "Von Middleware" anfordert, wird der Wert der Option gerendert.](startup/_static/index.png)

Ausführungsreihenfolge Middleware festgelegt ist, von der Reihenfolge der `IStartupFilter` Registrierungen:

* Mehrere `IStartupFilter` Implementierungen möglicherweise die gleichen Objekte interagieren. Wenn Sortierung wichtig ist, Sortieren ihre `IStartupFilter` service Registrierungen, die mit der Reihenfolge überein, die ihre Middlewares ausgeführt werden soll.
* Bibliotheken können Middleware mit einem oder mehreren hinzufügen `IStartupFilter` Implementierungen, die vor oder nach anderer Middleware app registriert `IStartupFilter`. Zum Aufrufen einer `IStartupFilter` Middleware vor eine Middleware hinzugefügt, indem einer Bibliothek `IStartupFilter`, positionieren Sie die Service-Registrierung vor dem Dienstcontainer die Bibliothek hinzugefügt wird. Rufen Sie sie anschließend, positionieren die Service-Registrierung nach der Bibliothek hinzugefügt.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Hosting](xref:fundamentals/hosting)
* [Arbeiten mit mehreren Umgebungen](xref:fundamentals/environments)
* [Middleware](xref:fundamentals/middleware)
* [Logging (Protokollierung)](xref:fundamentals/logging/index)
* [Konfiguration](xref:fundamentals/configuration/index)
* [StartupLoader-Klasse: FindStartupType-Methode (Referenz-Quelle)](https://github.com/aspnet/Hosting/blob/rel/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs#L66-L116)
