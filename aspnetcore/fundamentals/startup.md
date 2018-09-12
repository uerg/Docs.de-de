---
title: Anwendungsstart in ASP.NET Core
author: ardalis
description: Informationen zur Vorgehensweise der Startklasse in ASP.NET Core bei der Konfiguration von Diensten und der Anforderungspipeline einer App.
ms.author: tdykstra
ms.custom: mvc
ms.date: 4/13/2018
uid: fundamentals/startup
ms.openlocfilehash: 923d17be9c2bb1a9d338599d1cdc4c34302cddab
ms.sourcegitcommit: 08bf41d4b3e696ab512b044970e8304816f8cc56
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2018
ms.locfileid: "44040094"
---
# <a name="application-startup-in-aspnet-core"></a>Anwendungsstart in ASP.NET Core

Von [Steve Smith](https://ardalis.com), [Tom Dykstra](https://github.com/tdykstra) und [Luke Latham](https://github.com/guardrex)

Die `Startup`-Klasse konfiguriert Dienste und die Anforderungspipeline einer App.

## <a name="the-startup-class"></a>Die Startup-Klasse

ASP.NET Core-Apps verwenden eine `Startup`-Klasse, die standardmäßig `Startup` genannt wird. Die `Startup`-Klasse:

* Kann eine [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices)-Methode enthalten, um App-Dienste zu konfigurieren.
* Muss eine [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure)-Methode enthalten, um die Pipeline einer App zu erstellen, die Anforderungen verarbeitet.

`ConfigureServices` und `Configure` werden beim App-Start durch die Runtime aufgerufen:

[!code-csharp[](startup/snapshot_sample/Startup1.cs)]

Die `Startup`-Klasse wird mit der Methode [WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt;](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) angegeben:

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=10)]

Der Web-Host stellt einige Dienste für den `Startup`-Klassenkonstruktor bereit. Die App fügt über `ConfigureServices` zusätzliche Dienste hinzu. Daher sind sowohl die Host- als auch die App-Dienste in `Configure` und über die App verfügbar.

[Dependency Injection](xref:fundamentals/dependency-injection) wird häufig im Zusammenhang mit der `Startup`-Klasse verwendet, um Folgendes einzufügen:

* [IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment), um Dienste nach Umgebung zu konfigurieren.
* [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) zum Lesen der Konfiguration.
* [Iloggerfactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory) zum Erstellen einer Protokollierung in `Startup.ConfigureServices`.

[!code-csharp[](startup/snapshot_sample/Startup2.cs)]

Sie können auch einen konventionsbasierten Ansatz wählen, anstatt `IHostingEnvironment` einzufügen. Die App kann je nach Umgebung unterschiedliche `Startup`-Klassen definieren (z.B. `StartupDevelopment`). Zur Laufzeit wird dann die passende `Startup`-Klasse ausgewählt. Die Klasse, deren Namenssuffix mit der aktuellen Umgebung übereinstimmt, wird priorisiert. Wenn die App in der Entwicklungsumgebung ausgeführt wird und sowohl eine `Startup`-Klasse als auch eine `StartupDevelopment`-Klasse enthält, wird die `StartupDevelopment`-Klasse verwendet. Weitere Informationen finden Sie unter [Verwenden mehrerer Umgebungen](xref:fundamentals/environments#environment-based-startup-class-and-methods).

Weitere Informationen zu `WebHostBuilder` finden Sie im Artikel [Hosting](xref:fundamentals/host/index). Weitere Informationen zum Umgang mit Fehlern beim Start finden Sie unter [Startup exception handling (Umgang mit Ausnahmen beim Start)](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>Die ConfigureServices-Methode

Für die [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices)-Methode gilt:

* Optional
* Sie wird vor der `Configure`-Methode vom Webhost aufgerufen, um die App-Dienste zu konfigurieren.
* Über diese Methode werden standardmäßig [Konfigurationsoptionen](xref:fundamentals/configuration/index) festgelegt.

Das typische Muster besteht darin, alle `Add{Service}`-Methoden und dann alle `services.Configure{Service}`-Methoden aufzurufen. Ein Beispiel finden Sie unter [Konfigurieren der Identitätsdienste](xref:security/authentication/identity#pw).

Wenn Sie Dienste zum Dienstcontainer hinzufügen, können Sie auch über die App und die `Configure`-Methode auf diese zugreifen. Die Dienste werden über [Dependency Injection](xref:fundamentals/dependency-injection) oder [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices) aufgelöst.

Es kann sein, dass der Webhost einige Dienste konfiguriert, bevor die `Startup`-Methoden aufgerufen werden. Einzelheiten hierzu finden Sie im Thema [Host in ASP.NET Core](xref:fundamentals/host/index).

Für Features, die ein umfangreiches Setup erfordern, sind unter [IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection) `Add[Service]`-Erweiterungsmethoden verfügbar. Web-Apps registrieren Dienste in der Regel für Entity Framework, Identity und MVC:

[!code-csharp[](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

## <a name="the-configure-method"></a>Die Configure-Methode

Die [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure)-Methode wird verwendet, um festzulegen, wie die App auf HTTP-Anforderungen reagiert. Sie konfigurieren die Anforderungspipeline, indem Sie [Middleware](xref:fundamentals/middleware/index)-Komponenten zu einer [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder)-Instanz hinzufügen. Die `Configure`-Methode kann auf `IApplicationBuilder` zugreifen, wird aber nicht im Dienstcontainer registriert. Über das Hosting wird ein `IApplicationBuilder` erstellt, der direkt an `Configure` übergeben wird.

Die Pipeline wird über [ASP.NET Core-Vorlagen](/dotnet/core/tools/dotnet-new) konfiguriert, die folgende Komponenten unterstützen: eine Ausnahmeseite für Entwickler, [BrowserLink](http://vswebessentials.com/features/browserlink), Fehlerseiten, statische Dateien und ASP.NET Core MVC:

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Startup.cs?range=28-48&highlight=5,6,10,13,15)]

Jede `Use`-Erweiterungsmethode fügt eine Middlewarekomponente zu der Anforderungspipeline hinzu. Die `UseMvc`-Erweiterungsmethode fügt z.B. [Routingmiddleware](xref:fundamentals/routing) zur Anforderungspipeline hinzu und konfiguriert [MVC](xref:mvc/overview) als Standardhandler.

Jede Middlewarekomponente in der Anforderungspipeline ist für das Aufrufen der jeweils nächsten Komponente in der Pipeline oder, wenn nötig, für das Kurzschließen der Kette zuständig. Wenn das Kurzschließen nicht entlang der Middlewarekette passiert, hat jede Middleware eine zweite Chance die Anforderung zu verarbeiten, bevor sie an den Client gesendet wird.

In der Methodensignatur können außerdem auch zusätzliche Dienste wie `IHostingEnvironment` und `ILoggerFactory` festgelegt werden. Wenn Sie zusätzliche Dienste angegeben, werden diese eingefügt, sofern sie verfügbar sind.

Weitere Informationen zur Verwendung von `IApplicationBuilder` und der Reihenfolge der Middlewareverarbeitung finden Sie unter [Middleware](xref:fundamentals/middleware/index).

## <a name="convenience-methods"></a>Hilfsmethoden

Die Hilfsmethoden [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices) und [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) können anstelle einer `Startup`-Klasse verwendet werden. Wenn `ConfigureServices` mehrmals aufgerufen wird, werden diese Aufrufe einander angefügt. Wenn `Configure` mehrmals aufgerufen wird, wird der Methodenaufruf verwendet.

[!code-csharp[](startup/snapshot_sample/Program.cs?highlight=18,22)]

## <a name="extend-startup-with-startup-filters"></a>Erweitern Sie den Start mit Startfiltern

Verwenden Sie [IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter), um Middleware am Anfang und am Ende der [Configure](#the-configure-method)-Middlewarepipeline einer App zu konfigurieren. `IStartupFilter` ist nützlich, wenn Sie sicherstellen wollen, dass eine Middleware vor oder nach einer Middleware ausgeführt wird, die von Bibliotheken am Anfang oder am Ende der App-Pipeline hinzufügt wurden, die Anforderungen verarbeitet.

`IStartupFilter` implementiert nur die Methode [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure), die `Action<IApplicationBuilder>` empfängt und zurückgibt. [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) definiert Klassen, um die Anforderungspipeline einer App zu konfigurieren. Weitere Informationen finden Sie unter [Erstellen einer Middlewarepipeline mit IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder).

Jede `IStartupFilter`-Schnittstelle implementiert mindestens eine Middleware in die Anforderungspipeline. Die Filter werden in der Reihenfolge aufgerufen, in der sie zum Dienstcontainer hinzugefügt wurden. Über Filter kann Middleware hinzugefügt werden, bevor oder nachdem dem nächsten Filter die Kontrolle erteilt wird. D.h., sie wird am Anfang oder am Ende einer App-Pipeline hinzugefügt.

In der [Beispiel-App](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/) ([Download](xref:tutorials/index#how-to-download-a-sample)) wird deutlich, wie Sie eine Middleware über `IStartupFilter` registrieren. Die Beispiel-App enthält eine Middleware, die einen Optionswert über einen Parameter der Abfragezeichenfolge festlegt:

[!code-csharp[](startup/sample/RequestSetOptionsMiddleware.cs?name=snippet1)]

Die `RequestSetOptionsMiddleware`-Klasse wird in der `RequestSetOptionsStartupFilter`-Klasse konfiguriert:

[!code-csharp[](startup/sample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

Der `IStartupFilter` wird in [IWebHostBuilder.ConfigureServices](xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.ConfigureServices*) im Dienstcontainer registriert, um zu veranschaulichen, wie der Startfilter `Startup` von außerhalb der `Startup`-Klasse erweitert:

[!code-csharp[](startup/sample/Program.cs?name=snippet1&highlight=4-5)]

Wenn für `option` ein Parameter der Abfragezeichenfolge bereitgestellt wird, verarbeitet die Middleware die Wertzuweisung, bevor die MVC-Middleware die Antwort rendert:

![Browserfenster, in dem die gerenderte Indexseite geöffnet ist Der Wert von „Option“ wird als „From Middleware“ (Von der Middleware) gerendert. Dieser Vorgang ist von der Abfrage der Seite mit dem Parameter der Abfragezeichenfolge und dem Wert der Option abhängig, die auf „From Middleware“ festgelegt ist.](startup/_static/index.png)

Die Reihenfolge der Ausführung der Middleware ist von der Reihenfolge der `IStartupFilter`-Registrierungen abhängig:

* Es kann sein, dass mehrere Implementierungen von `IStartupFilter` mit denselben Objekten interagieren. Wenn die Reihenfolge für Sie wichtig ist, sollten Sie die `IStartupFilter`-Dienstregistrierungen der Implementierungen in der Reihenfolge anordnen, in der die Middleware ausgeführt werden soll.
* Bibliotheken fügen möglicherweise Middleware mit mindestens einer `IStartupFilter`-Implementierung hinzu, die vor oder nach anderer App-Middleware ausgeführt wird, die über `IStartupFilter` registriert wurde. Wenn Sie eine `IStartupFilter`-Middleware vor einer Middleware aufrufen möchten, die über die `IStartupFilter` einer Bibliothek hinzugefügt wurde, positionieren Sie die Dienstregistrierung, bevor die Bibliothek zum Dienstcontainer hinzugefügt wird. Wenn Sie diese danach aufrufen möchten, positionieren Sie die Dienstregistrierung, nachdem die Bibliothek hinzugefügt wurde.

## <a name="add-configuration-at-startup-from-an-external-assembly"></a>Hinzufügen von Konfigurationen aus einer externen Assembly beim Start

Mit einer [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup)-Implementierung können einer App beim Start von einer externen Assemblys aus, die sich außerhalb der `Startup`-Klasse der App befindet, Erweiterungen hinzugefügt werden Weitere Informationen finden Sie im Artikel zum [Erweitern einer App von einer externen Assembly aus](xref:fundamentals/configuration/platform-specific-configuration).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* <xref:fundamentals/host/index>
* <xref:fundamentals/environments>
* <xref:fundamentals/middleware/index>
* <xref:fundamentals/logging/index>
* <xref:fundamentals/configuration/index>
