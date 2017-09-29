---
title: Start der Anwendung in ASP.NET Core
author: ardalis
description: "Erläutert die Startklasse in ASP.NET Core."
keywords: ASP.NET Core, Start konfigurieren-Methode, ConfigureServices-Methode
ms.author: tdykstra
manager: wpickett
ms.date: 02/29/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/startup
ms.openlocfilehash: 94db2ff530b5de7fe357cfb591d09b984cb248f9
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/28/2017
---
# <a name="application-startup-in-aspnet-core"></a>Start der Anwendung in ASP.NET Core

Durch [Steve Smith](https://ardalis.com/) und [Tom Dykstra](https://github.com/tdykstra/)

Die `Startup` Klasse konfiguriert werden, Dienste und die Anwendung Anforderungspipeline. 

## <a name="the-startup-class"></a>Die Startklasse.

ASP.NET Core apps erfordern eine `Startup` -Klasse, mit dem Namen `Startup` gemäß der Konvention. Geben Sie den Namen des Start-Klasse in der `Main` des Programms [WebHostBuilderExtensions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderextensions) [ `UseStartup<TStartup>` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) Methode. Finden Sie unter [Hosting](xref:fundamentals/hosting) erfahren Sie mehr über `WebHostBuilder`, dem ausgeführt wird, bevor Sie `Startup`.

Sie können separate definieren `Startup` Klassen für unterschiedliche Umgebungen und den entsprechenden eine zur Laufzeit ausgewählt werden. Bei Angabe von `startupAssembly` in der [WebHost Konfiguration](https://docs.microsoft.com/aspnet/core/fundamentals/hosting?tabs=aspnetcore2x#configuring-a-host) oder hosting-Optionen, Startassembly geladen werden, und suchen Sie nach einer `Startup` oder `Startup[Environment]` Typ. Die Klasse, deren Name Suffix übereinstimmt, die aktuelle Umgebung priorisiert werden wird, wenn also die Anwendung, in ausgeführt wird der *Entwicklung* Umgebung, und enthält sowohl eine `Startup` und ein `StartupDevelopment` -Klasse, die `StartupDevelopment` Klasse verwendet. Finden Sie unter [FindStartupType](https://github.com/aspnet/Hosting/blob/rel/1.1.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs) in `StartupLoader` und [arbeiten mit mehreren Umgebungen](environments.md#startup-conventions).

Alternativ können Sie eine feste definieren `Startup` Klasse, die unabhängig von der Umgebung durch den Aufruf verwendet werden, `UseStartup<TStartup>`. Dies ist die empfohlene Vorgehensweise.

Die `Startup` Klassenkonstruktor kann Abhängigkeiten, die bereitgestellt sind akzeptieren [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection). Eine gängige Methode ist die Verwendung `IHostingEnvironment` einrichten [Konfiguration](xref:fundamentals/configuration) Quellen.

Die `Startup` Klasse umfasst einen `Configure` Methode und Sie können optional eine `ConfigureServices` Methode, die beide aufgerufen werden, wenn die Anwendung gestartet wird. Die Klasse zählen auch [umgebungsspezifische Versionen dieser Methoden](xref:fundamentals/environments#startup-conventions). `ConfigureServices`, falls vorhanden, wird aufgerufen, bevor `Configure`.

Erfahren Sie mehr über [Behandeln von Ausnahmen während des Anwendungsstarts](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>Die ConfigureServices-Methode

Die [ConfigureServices](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.startupbase#Microsoft_AspNetCore_Hosting_StartupBase_ConfigureServices_Microsoft_Extensions_DependencyInjection_IServiceCollection_) Methode ist optional, aber wenn verwendet, wird er aufgerufen, bevor die `Configure` Methode durch den Webhost. Die Webhost möglicherweise einige Dienste vor dem Konfigurieren ``Startup`` Methoden aufgerufen werden (finden Sie unter [hosting](xref:fundamentals/hosting)). Gemäß der Konvention [Konfigurationsoptionen](xref:fundamentals/configuration) werden in dieser Methode festgelegt.

Für Features, die erfordern beträchtliche Setup stehen `Add[Service]` Erweiterungsmethoden in [IServiceCollection](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.iservicecollection). Dieses Beispiels über die Standardvorlage für die Website konfiguriert die app, um Dienste für Entity Framework, Identitäts- und MVC verwenden:

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

Hinzufügen von Diensten auf den Container, werden sie in Ihrer Anwendung über verfügbar gemacht [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection).

## <a name="services-available-in-startup"></a>In den Starttasks verfügbaren Dienste

ASP.NET Core Abhängigkeitsinjektion stellt Dienste bereit, während des Starts der Anwendung. Sie können diese Dienste anfordern, indem Sie z. B. die entsprechende Schnittstelle als Parameter auf Ihre `Startup` Klassenkonstruktor oder den zugehörigen `Configure` Methode. Die `ConfigureServices` nur Methode nimmt ein `IServiceCollection` Parameter (aber registrierten Dienst kann aus der Auflistung abgerufen werden, sodass zusätzliche Parameter nicht erforderlich sind).

Im folgenden sind einige der in der Regel vom angeforderten Dienste `Startup` Methoden:

* Im Konstruktor: `IHostingEnvironment`,`ILogger<Startup>`
* In `ConfigureServices`:`IServiceCollection`
* In `Configure`: `IApplicationBuilder`, `IHostingEnvironment`,`ILoggerFactory`

Keine Dienste hinzugefügt, indem die ``WebHostBuilder`` ``ConfigureServices`` Methode angefordert werden kann, indem Sie die ``Startup`` -Klassenkonstruktor oder den zugehörigen ``Configure`` Methode. Verwendung `WebHostBuilder` alle Dienste Sie bereitstellen müssen, während der `Startup` Methoden.

## <a name="the-configure-method"></a>Die Configure-Methode

Die `Configure` Methode wird verwendet, um anzugeben, wie die ASP.NET-Anwendung auf HTTP-Anforderungen reagiert. Durch Hinzufügen die Anforderungspipeline konfiguriert ist [Middleware](middleware.md) Komponenten einer `IApplicationBuilder` -Instanz, die vom Abhängigkeitsinjektion bereitgestellt wird.

Im folgenden Beispiel aus der Website-Standardvorlage dienen zum Konfigurieren der Pipeline mit Unterstützung für mehrere Erweiterungsmethoden [c#] [BrowserLink](http://vswebessentials.com/features/browserlink), Fehlerseiten, statische Dateien, ASP.NET MVC und Identität.

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]

Jede `Use` Erweiterungsmethode Fügt eine [Middleware](xref:fundamentals/middleware) -Komponente in der Anforderungspipeline. Für die Instanz, die `UseMvc` Erweiterungsmethode fügt die [routing](routing.md) Middleware auf der Anforderungspipeline und konfiguriert [MVC](xref:mvc/overview) als Standardhandler.

Weitere Informationen zur Verwendung von `IApplicationBuilder`, finden Sie unter [Middleware](xref:fundamentals/middleware).

Zusätzliche Dienste wie `IHostingEnvironment` und `ILoggerFactory` kann auch in der Methodensignatur angegeben werden in diesem Fall werden diese Dienste [eingefügten](dependency-injection.md) sofern diese verfügbar sind. 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Arbeiten mit mehreren Umgebungen](xref:fundamentals/environments)
* [Middleware](xref:fundamentals/middleware)
* [Logging (Protokollierung)](xref:fundamentals/logging)
* [Konfiguration](xref:fundamentals/configuration)
