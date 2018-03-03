---
title: "ASP.NET Core – Grundlagen"
author: rick-anderson
description: Lernen Sie die grundlegenden Konzepte zum Erstellen einer ASP.NET Core-Anwendung kennen.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 09/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: fundamentals/index
ms.openlocfilehash: be37df7789354ac4ce8e373a1560366be157ffa5
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2018
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="14174-103">ASP.NET Core – Grundlagen</span><span class="sxs-lookup"><span data-stu-id="14174-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="14174-104">Eine ASP.NET Core-Anwendung ist eine Konsolenanwendung, in deren `Main`-Methode ein Webserver erstellt wird:</span><span class="sxs-lookup"><span data-stu-id="14174-104">An ASP.NET Core application is a console app that creates a web server in its `Main` method:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="14174-105">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="14174-105">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program2x.cs)]

<span data-ttu-id="14174-106">Die `Main`-Methode ruft die `WebHost.CreateDefaultBuilder`-Methode auf, die nach dem Builder-Muster einen Webanwendungshost erstellt.</span><span class="sxs-lookup"><span data-stu-id="14174-106">The `Main` method invokes `WebHost.CreateDefaultBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="14174-107">Der Builder verfügt über Methoden, die den Webserver (z.B. `UseKestrel`) und die Startklasse (`UseStartup`) definieren.</span><span class="sxs-lookup"><span data-stu-id="14174-107">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="14174-108">Im obigen Beispiel wird der [Kestrel](xref:fundamentals/servers/kestrel)-Webserver automatisch zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="14174-108">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="14174-109">Wenn IIS verfügbar ist, wird versucht, den Webhost von ASP.NET Core auf IIS auszuführen.</span><span class="sxs-lookup"><span data-stu-id="14174-109">ASP.NET Core's web host attempts to run on IIS, if available.</span></span> <span data-ttu-id="14174-110">Andere Webserver wie [HTTP.sys](xref:fundamentals/servers/httpsys) können durch den Aufruf der entsprechenden Erweiterungsmethode verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="14174-110">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="14174-111">`UseStartup` wird im nächsten Abschnitt näher erläutert.</span><span class="sxs-lookup"><span data-stu-id="14174-111">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="14174-112">Der Rückgabetyp `IWebHostBuilder` des `WebHost.CreateDefaultBuilder`-Aufrufs stellt viele optionale Methoden zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="14174-112">`IWebHostBuilder`, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="14174-113">Zu diesen Methoden gehören `UseHttpSys` für das Hosten der Anwendung in HTTP.sys und `UseContentRoot` für das Festlegen des Stamminhaltsverzeichnisses.</span><span class="sxs-lookup"><span data-stu-id="14174-113">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="14174-114">Mit den Methoden `Build` und `Run` wird das `IWebHost`-Objekt erstellt, das die Anwendung hostet und auf HTTP-Anforderungen lauscht.</span><span class="sxs-lookup"><span data-stu-id="14174-114">The `Build` and `Run` methods build the `IWebHost` object that hosts the app and begins listening for HTTP requests.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="14174-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="14174-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program.cs)]

<span data-ttu-id="14174-116">Die `Main`-Methode verwendet eine Instanz von `WebHostBuilder`, die nach dem Builder-Muster einen Webanwendungshost erstellt.</span><span class="sxs-lookup"><span data-stu-id="14174-116">The `Main` method uses `WebHostBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="14174-117">Der Builder verfügt über Methoden, die den Webserver (z.B. `UseKestrel`) und die Startklasse (`UseStartup`) definieren.</span><span class="sxs-lookup"><span data-stu-id="14174-117">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="14174-118">Im obigen Beispiel wird der [Kestrel](xref:fundamentals/servers/kestrel)-Webserver verwendet.</span><span class="sxs-lookup"><span data-stu-id="14174-118">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="14174-119">Andere Webserver wie [WebListener](xref:fundamentals/servers/weblistener) können durch den Aufruf der entsprechenden Erweiterungsmethode verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="14174-119">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="14174-120">`UseStartup` wird im nächsten Abschnitt näher erläutert.</span><span class="sxs-lookup"><span data-stu-id="14174-120">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="14174-121">`WebHostBuilder` stellt viele optionale Methoden wie z.B. `UseIISIntegration` für das Hosten in IIS und IIS Express und `UseContentRoot` für das Festlegen des Inhaltsstammverzeichnisses zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="14174-121">`WebHostBuilder` provides many optional methods, including `UseIISIntegration` for hosting in IIS and IIS Express and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="14174-122">Mit den Methoden `Build` und `Run` wird das `IWebHost`-Objekt erstellt, das die Anwendung hostet und auf HTTP-Anforderungen lauscht.</span><span class="sxs-lookup"><span data-stu-id="14174-122">The `Build` and `Run` methods build the `IWebHost` object that hosts the app and begins listening for HTTP requests.</span></span>

---

## <a name="startup"></a><span data-ttu-id="14174-123">Start</span><span class="sxs-lookup"><span data-stu-id="14174-123">Startup</span></span>

<span data-ttu-id="14174-124">Die `UseStartup`-Methode in `WebHostBuilder` gibt die `Startup`-Klasse für Ihre Anwendung an:</span><span class="sxs-lookup"><span data-stu-id="14174-124">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="14174-125">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="14174-125">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="14174-126">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="14174-126">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]

---

<span data-ttu-id="14174-127">In der `Startup`-Klasse definieren Sie die Pipeline zur Anforderungsverarbeitung. Außerdem werden in dieser Klasse alle von der Anwendung benötigten Dienste konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="14174-127">The `Startup` class is where you define the request handling pipeline and where any services needed by the app are configured.</span></span> <span data-ttu-id="14174-128">Die `Startup`-Klasse muss öffentlich sein und die folgenden Methoden enthalten:</span><span class="sxs-lookup"><span data-stu-id="14174-128">The `Startup` class must be public and contain the following methods:</span></span>

```csharp
public class Startup
{
    // This method gets called by the runtime. Use this method
    // to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
    }

    // This method gets called by the runtime. Use this method
    // to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app)
    {
    }
}
```

<span data-ttu-id="14174-129">`ConfigureServices` definiert die [Dienste](#dependency-injection-services) Ihrer Anwendung (z.B. ASP.NET Core MVC, Entity Framework Core, Identity).</span><span class="sxs-lookup"><span data-stu-id="14174-129">`ConfigureServices` defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="14174-130">`Configure` definiert die [Middleware](xref:fundamentals/middleware/index) für die Anforderungspipeline.</span><span class="sxs-lookup"><span data-stu-id="14174-130">`Configure` defines the [middleware](xref:fundamentals/middleware/index) for the request pipeline.</span></span>

<span data-ttu-id="14174-131">Weitere Informationen finden Sie unter [Application startup (Starten von Anwendungen)](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="14174-131">For more information, see [Application startup](xref:fundamentals/startup).</span></span>

## <a name="content-root"></a><span data-ttu-id="14174-132">Inhaltsstammverzeichnis</span><span class="sxs-lookup"><span data-stu-id="14174-132">Content root</span></span>

<span data-ttu-id="14174-133">Das Inhaltsstammverzeichnis ist der Basispfad zu allen von der Anwendung verwendeten Inhalten wie Ansichten, [Razor-Seiten](xref:mvc/razor-pages/index) und statischen Objekten.</span><span class="sxs-lookup"><span data-stu-id="14174-133">The content root is the base path to any content used by the app, such as views, [Razor Pages](xref:mvc/razor-pages/index), and static assets.</span></span> <span data-ttu-id="14174-134">Standardmäßig entspricht das Inhaltsstammverzeichnis dem Anwendungsbasispfad der ausführbaren Datei, mit der die Anwendung gehostet wird.</span><span class="sxs-lookup"><span data-stu-id="14174-134">By default, the content root is the same as application base path for the executable hosting the app.</span></span>

## <a name="web-root"></a><span data-ttu-id="14174-135">Webstammverzeichnis</span><span class="sxs-lookup"><span data-stu-id="14174-135">Web root</span></span>

<span data-ttu-id="14174-136">Das Webstammverzeichnis einer Anwendung ist das Projektverzeichnis, in dem sich öffentliche statische Ressourcen wie etwa CSS-, JavaScript- und Bilddateien befinden.</span><span class="sxs-lookup"><span data-stu-id="14174-136">The web root of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="14174-137">Abhängigkeitsinjektion (Dienste)</span><span class="sxs-lookup"><span data-stu-id="14174-137">Dependency Injection (Services)</span></span>

<span data-ttu-id="14174-138">Ein Dienst ist eine Komponente, die für die gemeinsame Nutzung in einer Anwendung vorgesehen ist.</span><span class="sxs-lookup"><span data-stu-id="14174-138">A service is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="14174-139">Dienste werden über die [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) zur Verfügung gestellt.</span><span class="sxs-lookup"><span data-stu-id="14174-139">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="14174-140">ASP.NET Core enthält einen nativen IoC-Container (**I**nversion **o**f **C**ontrol, Steuerungsumkehr), der die [Konstruktorinjektion](xref:mvc/controllers/dependency-injection#constructor-injection) standardmäßig unterstützt.</span><span class="sxs-lookup"><span data-stu-id="14174-140">ASP.NET Core includes a native **I**nversion **o**f **C**ontrol (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="14174-141">Bei Bedarf können Sie den nativen Standardcontainer durch einen anderen ersetzen.</span><span class="sxs-lookup"><span data-stu-id="14174-141">You can replace the default native container if you wish.</span></span> <span data-ttu-id="14174-142">Neben der losen Kopplung besteht ein weiterer Vorteil darin, dass durch die Abhängigkeitsinjektion Dienste in der gesamten Anwendung zur Verfügung gestellt werden (z.B. die [Protokollierung](xref:fundamentals/logging/index)).</span><span class="sxs-lookup"><span data-stu-id="14174-142">In addition to its loose coupling benefit, DI makes services available throughout your app (for example, [logging](xref:fundamentals/logging/index)).</span></span>

<span data-ttu-id="14174-143">Weitere Informationen finden Sie unter [Dependency injection (Abhängigkeitsinjektion)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="14174-143">For more information, see [Dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="middleware"></a><span data-ttu-id="14174-144">Middleware</span><span class="sxs-lookup"><span data-stu-id="14174-144">Middleware</span></span>

<span data-ttu-id="14174-145">In ASP.NET Core erstellen Sie Ihre Anforderungspipeline mithilfe von [Middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="14174-145">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="14174-146">ASP.NET Core-Middleware führt asynchrone Logikoperationen im `HttpContext` aus und ruft anschließend die nächste Middlewareanwendung in der Sequenz auf oder beendet die Anforderung direkt.</span><span class="sxs-lookup"><span data-stu-id="14174-146">ASP.NET Core middleware performs asynchronous logic on an `HttpContext` and then either invokes the next middleware in the sequence or terminates the request directly.</span></span> <span data-ttu-id="14174-147">Eine Middlewarekomponente mit dem Namen „XYZ“ wird durch den Aufruf einer `UseXYZ`-Erweiterungsmethode in der `Configure`-Methode hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="14174-147">A middleware component called "XYZ" is added by invoking an `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="14174-148">ASP.NET Core enthält standardmäßig zahlreiche Middlewareanwendungen:</span><span class="sxs-lookup"><span data-stu-id="14174-148">ASP.NET Core includes a rich set of built-in middleware:</span></span>

* [<span data-ttu-id="14174-149">Statische Dateien</span><span class="sxs-lookup"><span data-stu-id="14174-149">Static files</span></span>](xref:fundamentals/static-files)
* [<span data-ttu-id="14174-150">Routing</span><span class="sxs-lookup"><span data-stu-id="14174-150">Routing</span></span>](xref:fundamentals/routing)
* [<span data-ttu-id="14174-151">Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="14174-151">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="14174-152">Antworten komprimierende Middleware</span><span class="sxs-lookup"><span data-stu-id="14174-152">Response Compression Middleware</span></span>](xref:performance/response-compression)
* [<span data-ttu-id="14174-153">URL-umschreibende Middleware</span><span class="sxs-lookup"><span data-stu-id="14174-153">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting)

<span data-ttu-id="14174-154">Jede auf [OWIN](http://owin.org) basierende Middleware steht für ASP.NET Core zur Verfügung. Darüber hinaus können Sie auch Ihre eigene benutzerdefinierte Middleware erstellen.</span><span class="sxs-lookup"><span data-stu-id="14174-154">[OWIN](http://owin.org)-based middleware is available for ASP.NET Core apps, and you can write your own custom middleware.</span></span>

<span data-ttu-id="14174-155">Weitere Informationen finden Sie unter [Middleware](xref:fundamentals/middleware/index) und [Introduction to Open Web Interface for .NET (OWIN) (Einführung in Open Web Interface for .NET (OWIN))](xref:fundamentals/owin).</span><span class="sxs-lookup"><span data-stu-id="14174-155">For more information, see [Middleware](xref:fundamentals/middleware/index) and [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="environments"></a><span data-ttu-id="14174-156">Umgebungen</span><span class="sxs-lookup"><span data-stu-id="14174-156">Environments</span></span>

<span data-ttu-id="14174-157">Umgebungen wie „Entwicklung“ und „Produktion“ sind in ASP.NET Core von besonderer Bedeutung und können über Umgebungsvariablen festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="14174-157">Environments, such as "Development" and "Production", are a first-class notion in ASP.NET Core and can be set using environment variables.</span></span>

<span data-ttu-id="14174-158">Weitere Informationen finden Sie unter [Working with Multiple Environments (Verwenden von mehreren Umgebungen)](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="14174-158">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

## <a name="configuration"></a><span data-ttu-id="14174-159">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="14174-159">Configuration</span></span>

<span data-ttu-id="14174-160">ASP.NET Core verwendet ein Konfigurationsmodell, das auf Name/Wert-Paaren basiert.</span><span class="sxs-lookup"><span data-stu-id="14174-160">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="14174-161">Das Konfigurationsmodell basiert weder auf `System.Configuration` noch auf *web.config*. Die Konfiguration erhält Einstellungen von einer geordneten Menge an Konfigurationsanbietern.</span><span class="sxs-lookup"><span data-stu-id="14174-161">The configuration model isn't based on `System.Configuration` or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="14174-162">Die integrierten Konfigurationsanbieter unterstützen zahlreiche Dateiformate (XML, JSON, INI) und Umgebungsvariablen, durch die eine umgebungsbasierte Konfiguration ermöglicht wird.</span><span class="sxs-lookup"><span data-stu-id="14174-162">The built-in configuration providers support a variety of file formats (XML, JSON, INI) and environment variables to enable environment-based configuration.</span></span> <span data-ttu-id="14174-163">Sie können auch benutzerdefinierte Konfigurationsanbieter erstellen.</span><span class="sxs-lookup"><span data-stu-id="14174-163">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="14174-164">Weitere Informationen finden Sie unter [Konfiguration](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="14174-164">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="logging"></a><span data-ttu-id="14174-165">Protokollierung</span><span class="sxs-lookup"><span data-stu-id="14174-165">Logging</span></span>

<span data-ttu-id="14174-166">ASP.NET Core unterstützt eine Protokollierungs-API, die mit mehreren verschiedenen Protokollanbietern funktioniert.</span><span class="sxs-lookup"><span data-stu-id="14174-166">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="14174-167">Integrierte Anbieter unterstützen das Senden von Protokollen an mindestens einen Zielanbieter.</span><span class="sxs-lookup"><span data-stu-id="14174-167">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="14174-168">Protokollierungsframeworks von Drittanbietern sind ebenso zulässig.</span><span class="sxs-lookup"><span data-stu-id="14174-168">Third-party logging frameworks can be used.</span></span>

[<span data-ttu-id="14174-169">Logging (Protokollierung)</span><span class="sxs-lookup"><span data-stu-id="14174-169">Logging</span></span>](xref:fundamentals/logging/index)

## <a name="error-handling"></a><span data-ttu-id="14174-170">Fehlerbehandlung</span><span class="sxs-lookup"><span data-stu-id="14174-170">Error handling</span></span>

<span data-ttu-id="14174-171">ASP.NET Core verfügt über integrierte Features zur Fehlerbehandlung in Apps. Dazu zählen die Ausnahmenseite für Entwickler, Seiten für benutzerdefinierte Fehler, Seiten für statischen Statuscode sowie die Startausnahmebehandlung.</span><span class="sxs-lookup"><span data-stu-id="14174-171">ASP.NET Core has built-in features for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="14174-172">Weitere Informationen finden Sie unter [Fehlerbehandlung](xref:fundamentals/error-handling).</span><span class="sxs-lookup"><span data-stu-id="14174-172">For more information, see [Error Handling](xref:fundamentals/error-handling).</span></span>

## <a name="routing"></a><span data-ttu-id="14174-173">Routing</span><span class="sxs-lookup"><span data-stu-id="14174-173">Routing</span></span>

<span data-ttu-id="14174-174">ASP.NET Core verfügt über Features zum Routing von App-Anforderung an Routenhandler.</span><span class="sxs-lookup"><span data-stu-id="14174-174">ASP.NET Core offers features for routing of app requests to route handlers.</span></span>

<span data-ttu-id="14174-175">Weitere Informationen finden Sie unter [Routing](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="14174-175">For more information, see [Routing](xref:fundamentals/routing).</span></span>

## <a name="file-providers"></a><span data-ttu-id="14174-176">Dateianbieter</span><span class="sxs-lookup"><span data-stu-id="14174-176">File providers</span></span>

<span data-ttu-id="14174-177">ASP.NET Core abstrahiert den Dateisystemzugriff mithilfe von Dateianbietern, wodurch eine einfache Schnittstelle zum plattformübergreifenden Arbeiten mit Dateien zur Verfügung gestellt wird.</span><span class="sxs-lookup"><span data-stu-id="14174-177">ASP.NET Core abstracts file system access through the use of File Providers, which offers a common interface for working with files across platforms.</span></span>

<span data-ttu-id="14174-178">Weitere Informationen finden Sie unter [Dateianbieter](xref:fundamentals/file-providers).</span><span class="sxs-lookup"><span data-stu-id="14174-178">For more information, see [File Providers](xref:fundamentals/file-providers).</span></span>

## <a name="static-files"></a><span data-ttu-id="14174-179">Statische Dateien</span><span class="sxs-lookup"><span data-stu-id="14174-179">Static files</span></span>

<span data-ttu-id="14174-180">Middleware für statische Dateien kümmert sich um statische Dateien wie etwa HTML, CSS, Image und JavaScript.</span><span class="sxs-lookup"><span data-stu-id="14174-180">Static files middleware serves static files, such as HTML, CSS, image, and JavaScript.</span></span>

<span data-ttu-id="14174-181">Weitere Informationen finden Sie unter [Arbeiten mit statischen Dateien](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="14174-181">For more information, see [Working with static files](xref:fundamentals/static-files).</span></span>

## <a name="hosting"></a><span data-ttu-id="14174-182">Hosting</span><span class="sxs-lookup"><span data-stu-id="14174-182">Hosting</span></span>

<span data-ttu-id="14174-183">ASP.NET Core-Apps konfigurieren und starten einen *Host*, der für das Starten der App und das Verwalten deren Lebensdauer verantwortlich ist.</span><span class="sxs-lookup"><span data-stu-id="14174-183">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="14174-184">Weitere Informationen finden Sie unter [Hosting](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="14174-184">For more information, see [Hosting](xref:fundamentals/hosting).</span></span>

## <a name="session-and-application-state"></a><span data-ttu-id="14174-185">Sitzungs- und Anwendungszustand</span><span class="sxs-lookup"><span data-stu-id="14174-185">Session and application state</span></span>

<span data-ttu-id="14174-186">Sitzungszustand ist ein Feature in ASP.NET Core, das Sie verwenden können, um Benutzerdaten zu speichern, während der Benutzer Ihre Web-App durchsucht.</span><span class="sxs-lookup"><span data-stu-id="14174-186">Session state is a feature in ASP.NET Core that you can use to save and store user data while the user browses your web app.</span></span>

<span data-ttu-id="14174-187">Weitere Informationen finden Sie unter [Session and application state (Sitzungs- und Anwendungszustand)](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="14174-187">For more information, see [Session and application state](xref:fundamentals/app-state).</span></span>

## <a name="servers"></a><span data-ttu-id="14174-188">Server</span><span class="sxs-lookup"><span data-stu-id="14174-188">Servers</span></span>

<span data-ttu-id="14174-189">Das Hostingmodell von ASP.NET Core lauscht nicht direkt auf Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="14174-189">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="14174-190">Das Hostingmodell ist darauf angewiesen, dass eine HTTP-Serverimplementierung die Anforderungen an die App weiterleitet.</span><span class="sxs-lookup"><span data-stu-id="14174-190">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span> <span data-ttu-id="14174-191">Die weitergeleitete Anforderung wird von Funktionsobjekten umschlossen, auf die Sie über Schnittstellen zugreifen können.</span><span class="sxs-lookup"><span data-stu-id="14174-191">The forwarded request is wrapped as a set of feature objects that can be accessed through interfaces.</span></span> <span data-ttu-id="14174-192">ASP.NET Core enthält den verwalteten, plattformübergreifenden Webserver [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="14174-192">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="14174-193">Kestrel wird in der Regel hinter einem Produktionswebserver wie [IIS](https://www.iis.net/) oder [Nginx](http://nginx.org) ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="14174-193">Kestrel is often run behind a production web server, such as [IIS](https://www.iis.net/) or [Nginx](http://nginx.org).</span></span> <span data-ttu-id="14174-194">Kestrel kann als Edgeserver ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="14174-194">Kestrel can be run as an edge server.</span></span>

<span data-ttu-id="14174-195">Weitere Informationen finden Sie unter [Server](xref:fundamentals/servers/index) und in den folgenden Themen:</span><span class="sxs-lookup"><span data-stu-id="14174-195">For more information, see [Servers](xref:fundamentals/servers/index) and the following topics:</span></span>

* [<span data-ttu-id="14174-196">Kestrel</span><span class="sxs-lookup"><span data-stu-id="14174-196">Kestrel</span></span>](xref:fundamentals/servers/kestrel)
* [<span data-ttu-id="14174-197">ASP.NET Core-Modul</span><span class="sxs-lookup"><span data-stu-id="14174-197">ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* <span data-ttu-id="14174-198">[HTTP.sys](xref:fundamentals/servers/httpsys) (früher [WebListener](xref:fundamentals/servers/weblistener))</span><span class="sxs-lookup"><span data-stu-id="14174-198">[HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener))</span></span>

## <a name="globalization-and-localization"></a><span data-ttu-id="14174-199">Globalisierung und Lokalisierung</span><span class="sxs-lookup"><span data-stu-id="14174-199">Globalization and localization</span></span>

<span data-ttu-id="14174-200">Wenn Sie eine mehrsprachige Website mit ASP.NET Core erstellen, können Sie ein breiteres Publikum erreichen.</span><span class="sxs-lookup"><span data-stu-id="14174-200">Creating a multilingual website with ASP.NET Core allows your site to reach a wider audience.</span></span> <span data-ttu-id="14174-201">ASP.NET Core bietet Dienste und Middleware zur Lokalisierung in verschiedene Sprachen und Kulturen.</span><span class="sxs-lookup"><span data-stu-id="14174-201">ASP.NET Core provides services and middleware for localizing into different languages and cultures.</span></span>

<span data-ttu-id="14174-202">Weitere Informationen finden Sie unter [Globalisierung und Lokalisierung](xref:fundamentals/localization).</span><span class="sxs-lookup"><span data-stu-id="14174-202">For more information, see [Globalization and localization](xref:fundamentals/localization).</span></span>

## <a name="request-features"></a><span data-ttu-id="14174-203">Anforderungsfeatures</span><span class="sxs-lookup"><span data-stu-id="14174-203">Request features</span></span>

<span data-ttu-id="14174-204">Ausführliche Informationen zur Webserverimplementierung, die mit HTTP-Anforderungen und -antworten in Verbindung stehen, werden in Schnittstellen definiert.</span><span class="sxs-lookup"><span data-stu-id="14174-204">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="14174-205">Diese Schnittstellen werden von Serverimplementierungen und Middleware verwendet, um die Hostingpipeline der App zu erstellen und anzupassen.</span><span class="sxs-lookup"><span data-stu-id="14174-205">These interfaces are used by server implementations and middleware to create and modify the app's hosting pipeline.</span></span>

<span data-ttu-id="14174-206">Weitere Informationen finden Sie unter [Request Features (Anforderungsfeatures)](xref:fundamentals/request-features).</span><span class="sxs-lookup"><span data-stu-id="14174-206">For more information, see [Request Features](xref:fundamentals/request-features).</span></span>

## <a name="background-tasks"></a><span data-ttu-id="14174-207">Hintergrundaufgaben</span><span class="sxs-lookup"><span data-stu-id="14174-207">Background tasks</span></span>

<span data-ttu-id="14174-208">Hintergrundaufgaben werden als *gehostete Dienste* implementiert.</span><span class="sxs-lookup"><span data-stu-id="14174-208">Background tasks are implemented as *hosted services*.</span></span> <span data-ttu-id="14174-209">Ein gehosteter Dienst ist eine Klasse mit Logik für Hintergrundaufgaben, die die Schnittstelle [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) implementiert.</span><span class="sxs-lookup"><span data-stu-id="14174-209">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span>

<span data-ttu-id="14174-210">Weitere Informationen finden Sie unter [Hintergrundaufgaben mit gehosteten Diensten](xref:fundamentals/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="14174-210">For more information, see [Background tasks with hosted services](xref:fundamentals/hosted-services).</span></span>

## <a name="open-web-interface-for-net-owin"></a><span data-ttu-id="14174-211">Open Web Interface for .NET (OWIN)</span><span class="sxs-lookup"><span data-stu-id="14174-211">Open Web Interface for .NET (OWIN)</span></span>

<span data-ttu-id="14174-212">ASP.NET Core unterstützt Open Web Interface for .NET (OWIN).</span><span class="sxs-lookup"><span data-stu-id="14174-212">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="14174-213">Mit OWIN können Web-Apps von Webservern entkoppelt werden.</span><span class="sxs-lookup"><span data-stu-id="14174-213">OWIN allows web apps to be decoupled from web servers.</span></span>

<span data-ttu-id="14174-214">Weitere Informationen finden Sie unter [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span><span class="sxs-lookup"><span data-stu-id="14174-214">For more information, see [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="websockets"></a><span data-ttu-id="14174-215">WebSockets</span><span class="sxs-lookup"><span data-stu-id="14174-215">WebSockets</span></span>

<span data-ttu-id="14174-216">Bei [WebSockets](https://wikipedia.org/wiki/WebSocket) handelt es sich um ein Protokoll, das bidirektionale persistente Kommunikationskanäle über TCP-Verbindungen ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="14174-216">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="14174-217">Es wird in Chat-, Börsenticker- und Spiele-Apps sowie überall dort verwendet, wo Echtzeitfunktionen in einer Web-App benötigt werden.</span><span class="sxs-lookup"><span data-stu-id="14174-217">It's used for apps such as chat, stock tickers, games, and anywhere you desire real-time functionality in a web app.</span></span> <span data-ttu-id="14174-218">ASP.NET Core unterstützt WebSockets-Features.</span><span class="sxs-lookup"><span data-stu-id="14174-218">ASP.NET Core supports web socket features.</span></span>

<span data-ttu-id="14174-219">Weitere Informationen finden Sie unter [WebSockets](xref:fundamentals/websockets).</span><span class="sxs-lookup"><span data-stu-id="14174-219">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

## <a name="microsoftaspnetcoreall-metapackage"></a><span data-ttu-id="14174-220">Metapaket „Microsoft.AspNetCore.All“</span><span class="sxs-lookup"><span data-stu-id="14174-220">Microsoft.AspNetCore.All metapackage</span></span>

<span data-ttu-id="14174-221">Das Metapaket [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) für ASP.NET Core enthält:</span><span class="sxs-lookup"><span data-stu-id="14174-221">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="14174-222">alle unterstützten Pakete des ASP.NET Core-Teams</span><span class="sxs-lookup"><span data-stu-id="14174-222">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="14174-223">alle unterstützten Pakete von Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="14174-223">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="14174-224">interne und Drittanbieterabhängigkeiten, die von ASP.NET Core und Entity Framework Core verwendet werden</span><span class="sxs-lookup"><span data-stu-id="14174-224">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="14174-225">Weitere Informationen finden Sie unter [Microsoft.AspNetCore.All metapackage (Microsoft.AspNetCore.All-Metapaket)](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="14174-225">For more information, see [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

## <a name="net-core-vs-net-framework-runtime"></a><span data-ttu-id="14174-226">.NET Core-Runtime im Vergleich zur .NET Framework-Laufzeit</span><span class="sxs-lookup"><span data-stu-id="14174-226">.NET Core vs. .NET Framework runtime</span></span>

<span data-ttu-id="14174-227">Eine ASP.NET Core-Anwendung kann für die .NET Core- oder .NET Framework-Laufzeit entwickelt werden.</span><span class="sxs-lookup"><span data-stu-id="14174-227">An ASP.NET Core app can target the .NET Core or .NET Framework runtime.</span></span>

<span data-ttu-id="14174-228">Weitere Informationen finden Sie unter [Wahl zwischen .NET Core und .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="14174-228">For more information, see [Choosing between .NET Core and .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="choose-between-aspnet-core-and-aspnet"></a><span data-ttu-id="14174-229">Wählen zwischen ASP.NET und ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="14174-229">Choose between ASP.NET Core and ASP.NET</span></span>

<span data-ttu-id="14174-230">Weitere Informationen, die Ihnen die Wahl zwischen ASP.NET Core und ASP.NET erleichtern, finden Sie unter [Wählen zwischen ASP.NET Core und ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).</span><span class="sxs-lookup"><span data-stu-id="14174-230">For more information on choosing between ASP.NET Core and ASP.NET, see [Choose between ASP.NET Core and ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).</span></span>
