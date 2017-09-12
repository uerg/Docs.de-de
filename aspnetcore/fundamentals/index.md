---
title: "ASP.NET Core – Grundlagen"
author: rick-anderson
description: "Dieser Artikel enthält einen allgemeinen Überblick über grundlegende Konzepte, die bei der Erstellung von ASP.NET Core-Anwendungen relevant sind."
keywords: "ASP.NET Core, Grundlagen, Übersicht"
ms.author: riande
manager: wpickett
ms.date: 08/18/2017
ms.topic: get-started-article
ms.assetid: a19b7836-63e4-44e8-8250-50d426dd1070
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/index
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5d8ca35b0e2e4b6e9b1ec745a3a7cf7c3983c461
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2017
---
# <a name="aspnet-core-fundamentals-overview"></a><span data-ttu-id="07f1d-104">Übersicht über Grundlagen von ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="07f1d-104">ASP.NET Core fundamentals overview</span></span>

<span data-ttu-id="07f1d-105">Eine ASP.NET Core-Anwendung ist eine Konsolenanwendung, in deren `Main`-Methode ein Webserver erstellt wird:</span><span class="sxs-lookup"><span data-stu-id="07f1d-105">An ASP.NET Core application is a console app that creates a web server in its `Main` method:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="07f1d-106">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="07f1d-106">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="07f1d-107">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs)]</span><span class="sxs-lookup"><span data-stu-id="07f1d-107">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs)]</span></span>

<span data-ttu-id="07f1d-108">Die `Main`-Methode ruft die `WebHost.CreateDefaultBuilder`-Methode auf, die nach dem Builder-Muster einen Webanwendungshost erstellt.</span><span class="sxs-lookup"><span data-stu-id="07f1d-108">The `Main` method invokes `WebHost.CreateDefaultBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="07f1d-109">Der Builder verfügt über Methoden, die den Webserver (z.B. `UseKestrel`) und die Startklasse (`UseStartup`) definieren.</span><span class="sxs-lookup"><span data-stu-id="07f1d-109">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="07f1d-110">Im obigen Beispiel wird ein [Kestrel](xref:fundamentals/servers/kestrel)-Webserver automatisch zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="07f1d-110">In the preceding example, a [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="07f1d-111">Wenn IIS verfügbar ist, wird versucht, den Webhost von ASP.NET Core auf IIS auszuführen.</span><span class="sxs-lookup"><span data-stu-id="07f1d-111">ASP.NET Core's web host will attempt to run on IIS, if it is available.</span></span> <span data-ttu-id="07f1d-112">Andere Webserver wie [HTTP.sys](xref:fundamentals/servers/httpsys) können durch den Aufruf der entsprechenden Erweiterungsmethode verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="07f1d-112">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="07f1d-113">`UseStartup` wird im nächsten Abschnitt näher erläutert.</span><span class="sxs-lookup"><span data-stu-id="07f1d-113">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="07f1d-114">Der Rückgabetyp `IWebHostBuilder` des `WebHost.CreateDefaultBuilder`-Aufrufs stellt viele optionale Methoden zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="07f1d-114">`IWebHostBuilder`, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="07f1d-115">Zu diesen Methoden gehören `UseHttpSys` für das Hosten der Anwendung in HTTP.sys und `UseContentRoot` für das Festlegen des Stamminhaltsverzeichnisses.</span><span class="sxs-lookup"><span data-stu-id="07f1d-115">Some of these methods include `UseHttpSys` for hosting the application in HTTP.sys, and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="07f1d-116">Mit den Methoden `Build` und `Run` wird das `IWebHost`-Objekt erstellt, das die Anwendung hostet und auf HTTP-Anforderungen lauscht.</span><span class="sxs-lookup"><span data-stu-id="07f1d-116">The `Build` and `Run` methods build the `IWebHost` object that will host the application and begin listening for HTTP requests.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="07f1d-117">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="07f1d-117">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="07f1d-118">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="07f1d-118">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs)]</span></span>

<span data-ttu-id="07f1d-119">Die `Main`-Methode verwendet eine Instanz von `WebHostBuilder`, die nach dem Builder-Muster einen Webanwendungshost erstellt.</span><span class="sxs-lookup"><span data-stu-id="07f1d-119">The `Main` method uses `WebHostBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="07f1d-120">Der Builder verfügt über Methoden, die den Webserver (z.B. `UseKestrel`) und die Startklasse (`UseStartup`) definieren.</span><span class="sxs-lookup"><span data-stu-id="07f1d-120">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="07f1d-121">Im obigen Beispiel wird der [Kestrel](xref:fundamentals/servers/kestrel)-Webserver verwendet.</span><span class="sxs-lookup"><span data-stu-id="07f1d-121">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="07f1d-122">Andere Webserver wie [WebListener](xref:fundamentals/servers/weblistener) können durch den Aufruf der entsprechenden Erweiterungsmethode verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="07f1d-122">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="07f1d-123">`UseStartup` wird im nächsten Abschnitt näher erläutert.</span><span class="sxs-lookup"><span data-stu-id="07f1d-123">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="07f1d-124">`WebHostBuilder` stellt viele optionale Methoden wie z.B. `UseIISIntegration` für das Hosten in IIS und IIS Express und `UseContentRoot` für das Festlegen des Inhaltsstammverzeichnisses zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="07f1d-124">`WebHostBuilder` provides many optional methods, including `UseIISIntegration` for hosting in IIS and IIS Express, and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="07f1d-125">Mit den Methoden `Build` und `Run` wird das `IWebHost`-Objekt erstellt, das die Anwendung hostet und auf HTTP-Anforderungen lauscht.</span><span class="sxs-lookup"><span data-stu-id="07f1d-125">The `Build` and `Run` methods build the `IWebHost` object that will host the application and begin listening for HTTP requests.</span></span>

---

## <a name="startup"></a><span data-ttu-id="07f1d-126">Start</span><span class="sxs-lookup"><span data-stu-id="07f1d-126">Startup</span></span>

<span data-ttu-id="07f1d-127">Die `UseStartup`-Methode in `WebHostBuilder` gibt die `Startup`-Klasse für Ihre Anwendung an:</span><span class="sxs-lookup"><span data-stu-id="07f1d-127">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="07f1d-128">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="07f1d-128">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="07f1d-129">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]</span><span class="sxs-lookup"><span data-stu-id="07f1d-129">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="07f1d-130">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="07f1d-130">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="07f1d-131">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]</span><span class="sxs-lookup"><span data-stu-id="07f1d-131">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]</span></span>

---

<span data-ttu-id="07f1d-132">In der `Startup`-Klasse definieren Sie die Pipeline zur Anforderungsverarbeitung. Außerdem werden in dieser Klasse alle von der Anwendung benötigten Dienste konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="07f1d-132">The `Startup` class is where you define the request handling pipeline and where any services needed by the application are configured.</span></span> <span data-ttu-id="07f1d-133">Die `Startup`-Klasse muss öffentlich sein und die folgenden Methoden enthalten:</span><span class="sxs-lookup"><span data-stu-id="07f1d-133">The `Startup` class must be public and contain the following methods:</span></span>

```csharp
public class Startup
{
    // This method gets called by the runtime. Use this method to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
    }

    // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app)
    {
    }
}
```

* <span data-ttu-id="07f1d-134">`ConfigureServices` definiert die [Dienste](#services) Ihrer Anwendung (z.B. ASP.NET Core MVC, Entity Framework Core, Identity usw.).</span><span class="sxs-lookup"><span data-stu-id="07f1d-134">`ConfigureServices` defines the [Services](#services) used by your application (such as ASP.NET Core MVC, Entity Framework Core, Identity, etc.).</span></span>

* <span data-ttu-id="07f1d-135">`Configure` definiert die [Middleware](xref:fundamentals/middleware) in der Anforderungspipeline.</span><span class="sxs-lookup"><span data-stu-id="07f1d-135">`Configure` defines the [middleware](xref:fundamentals/middleware) in the request pipeline.</span></span>

<span data-ttu-id="07f1d-136">Weitere Informationen finden Sie unter [Application startup (Starten von Anwendungen)](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="07f1d-136">For more information, see [Application startup](xref:fundamentals/startup).</span></span>

## <a name="services"></a><span data-ttu-id="07f1d-137">Dienste</span><span class="sxs-lookup"><span data-stu-id="07f1d-137">Services</span></span>

<span data-ttu-id="07f1d-138">Ein Dienst ist eine Komponente, die für die gemeinsame Nutzung in einer Anwendung vorgesehen ist.</span><span class="sxs-lookup"><span data-stu-id="07f1d-138">A service is a component that is intended for common consumption in an application.</span></span> <span data-ttu-id="07f1d-139">Dienste werden über die [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) zur Verfügung gestellt.</span><span class="sxs-lookup"><span data-stu-id="07f1d-139">Services are made available through [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="07f1d-140">ASP.NET Core enthält einen nativen IoC-Container (Inversion of Control, Steuerungsumkehr), der die [Konstruktorinjektion](xref:mvc/controllers/dependency-injection#constructor-injection) standardmäßig unterstützt.</span><span class="sxs-lookup"><span data-stu-id="07f1d-140">ASP.NET Core includes a native inversion of control (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="07f1d-141">Der native Container kann mit einem Container Ihrer Wahl ersetzt werden.</span><span class="sxs-lookup"><span data-stu-id="07f1d-141">The native container can be replaced with your container of choice.</span></span> <span data-ttu-id="07f1d-142">Neben der losen Kopplung besteht ein weiterer Vorteil darin, dass durch die Abhängigkeitsinjektion Dienste in der gesamten Anwendung zur Verfügung gestellt werden.</span><span class="sxs-lookup"><span data-stu-id="07f1d-142">In addition to its loose coupling benefit, DI makes services available throughout your application.</span></span> <span data-ttu-id="07f1d-143">So kann beispielsweise die [Protokollierung](xref:fundamentals/logging) in der gesamten Anwendung genutzt werden.</span><span class="sxs-lookup"><span data-stu-id="07f1d-143">For example, [logging](xref:fundamentals/logging) is available throughout your application.</span></span>

<span data-ttu-id="07f1d-144">Weitere Informationen finden Sie unter [Dependency injection (Abhängigkeitsinjektion)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="07f1d-144">For more information, see [Dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="middleware"></a><span data-ttu-id="07f1d-145">Middleware</span><span class="sxs-lookup"><span data-stu-id="07f1d-145">Middleware</span></span>

<span data-ttu-id="07f1d-146">In ASP.NET Core erstellen Sie Ihre Anforderungspipeline mithilfe von [Middleware](xref:fundamentals/middleware).</span><span class="sxs-lookup"><span data-stu-id="07f1d-146">In ASP.NET Core, you compose your request pipeline using [Middleware](xref:fundamentals/middleware).</span></span> <span data-ttu-id="07f1d-147">ASP.NET Core-Middleware führt asynchrone Logikoperationen im `HttpContext` aus und ruft anschließend die nächste Middlewareanwendung in der Sequenz auf oder beendet die Anforderung direkt.</span><span class="sxs-lookup"><span data-stu-id="07f1d-147">ASP.NET Core middleware performs asynchronous logic on an `HttpContext` and then either invokes the next middleware in the sequence or terminates the request directly.</span></span> <span data-ttu-id="07f1d-148">Eine Middlewarekomponente mit dem Namen „XYZ“ wird durch den Aufruf einer `UseXYZ`-Erweiterungsmethode in der `Configure`-Methode hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="07f1d-148">A middleware component called "XYZ" is added by invoking a `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="07f1d-149">ASP.NET Core enthält standardmäßig zahlreiche Middlewareanwendungen:</span><span class="sxs-lookup"><span data-stu-id="07f1d-149">ASP.NET Core comes with a rich set of built-in middleware:</span></span>

* [<span data-ttu-id="07f1d-150">Statische Dateien</span><span class="sxs-lookup"><span data-stu-id="07f1d-150">Static files</span></span>](xref:fundamentals/static-files)

* [<span data-ttu-id="07f1d-151">Routing</span><span class="sxs-lookup"><span data-stu-id="07f1d-151">Routing</span></span>](xref:fundamentals/routing)

* [<span data-ttu-id="07f1d-152">Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="07f1d-152">Authentication</span></span>](xref:security/authentication/index)

<span data-ttu-id="07f1d-153">Sie können jede auf [OWIN](http://owin.org) basierende Middleware mit ASP.NET Core verwenden und außerdem benutzerdefinierte Middleware erstellen.</span><span class="sxs-lookup"><span data-stu-id="07f1d-153">You can use any [OWIN](http://owin.org)-based middleware with ASP.NET Core, and you can write your own custom middleware.</span></span>

<span data-ttu-id="07f1d-154">Weitere Informationen finden Sie unter [Middleware](xref:fundamentals/middleware) und [Introduction to Open Web Interface for .NET (OWIN) (Einführung in Open Web Interface for .NET (OWIN))](xref:fundamentals/owin).</span><span class="sxs-lookup"><span data-stu-id="07f1d-154">For more information, see [Middleware](xref:fundamentals/middleware) and [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="servers"></a><span data-ttu-id="07f1d-155">Server</span><span class="sxs-lookup"><span data-stu-id="07f1d-155">Servers</span></span>

<span data-ttu-id="07f1d-156">Das ASP.NET Core-Hostmodell lauscht nicht direkt auf Anforderungen. Vielmehr verwendet es eine HTTP-Serverimplementierung, mit der die Anforderung an die Anwendung weitergeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="07f1d-156">The ASP.NET Core hosting model does not directly listen for requests; rather, it relies on an HTTP server implementation to forward the request to the application.</span></span> <span data-ttu-id="07f1d-157">Die weitergeleitete Anforderung wird von Funktionsobjekten umschlossen, auf die Sie über Schnittstellen zugreifen können.</span><span class="sxs-lookup"><span data-stu-id="07f1d-157">The forwarded request is wrapped as a set of feature objects that you can access through interfaces.</span></span> <span data-ttu-id="07f1d-158">Die Anwendung kombiniert diese Objekte zu einem `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="07f1d-158">The application composes this set into an `HttpContext`.</span></span> <span data-ttu-id="07f1d-159">ASP.NET Core enthält den verwalteten, plattformübergreifenden Webserver [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="07f1d-159">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="07f1d-160">Kestrel wird in der Regel hinter einem Produktionswebserver wie [IIS](https://www.iis.net/) oder [nginx](http://nginx.org) ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="07f1d-160">Kestrel is typically run behind a production web server like [IIS](https://www.iis.net/) or [nginx](http://nginx.org).</span></span>

<span data-ttu-id="07f1d-161">Weitere Informationen finden Sie unter [Server](xref:fundamentals/servers/index) und [Hosting](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="07f1d-161">For more information, see [Servers](xref:fundamentals/servers/index) and [Hosting](xref:fundamentals/hosting).</span></span>

## <a name="content-root"></a><span data-ttu-id="07f1d-162">Inhaltsstammverzeichnis</span><span class="sxs-lookup"><span data-stu-id="07f1d-162">Content root</span></span>

<span data-ttu-id="07f1d-163">Das Inhaltsstammverzeichnis ist der Basispfad zu allen von der Anwendung verwendeten Inhalten wie Ansichten, [Razor-Seiten](xref:mvc/razor-pages/index) und statischen Objekten.</span><span class="sxs-lookup"><span data-stu-id="07f1d-163">The content root is the base path to any content used by the app, such as views, [Razor Pages](xref:mvc/razor-pages/index), and static assets.</span></span> <span data-ttu-id="07f1d-164">Standardmäßig entspricht das Inhaltsstammverzeichnis dem Anwendungsbasispfad der ausführbaren Datei, mit der die Anwendung gehostet wird.</span><span class="sxs-lookup"><span data-stu-id="07f1d-164">By default, the content root is the same as application base path for the executable hosting the application.</span></span> <span data-ttu-id="07f1d-165">Ein alternativer Speicherort für das Inhaltsstammverzeichnis kann mit `WebHostBuilder` angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="07f1d-165">An alternative location for content root is specified with `WebHostBuilder`.</span></span>

## <a name="web-root"></a><span data-ttu-id="07f1d-166">Webstammverzeichnis</span><span class="sxs-lookup"><span data-stu-id="07f1d-166">Web root</span></span>

<span data-ttu-id="07f1d-167">Das Webstammverzeichnis einer Anwendung ist das Projektverzeichnis, in dem sich öffentliche, statische Ressourcen wie CSS-, JavaScript- und Bilddateien befinden.</span><span class="sxs-lookup"><span data-stu-id="07f1d-167">The web root of an application is the directory in the project containing public, static resources like CSS, JavaScript, and image files.</span></span> <span data-ttu-id="07f1d-168">Die Middleware für statische Dateien stellt standardmäßig nur Dateien aus dem Webstammverzeichnis und dessen Unterverzeichnissen bereit.</span><span class="sxs-lookup"><span data-stu-id="07f1d-168">By default, the static files middleware will only serve files from the web root directory and its sub-directories.</span></span> <span data-ttu-id="07f1d-169">Weitere Informationen finden Sie unter [working with static files (Arbeiten mit statischen Dateien)](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="07f1d-169">See [working with static files](xref:fundamentals/static-files) for more info.</span></span> <span data-ttu-id="07f1d-170">Das Webstammverzeichnis ist standardmäßig */wwwroot*. Sie können allerdings mit dem `WebHostBuilder` einen anderen Speicherort angeben.</span><span class="sxs-lookup"><span data-stu-id="07f1d-170">The web root path defaults to */wwwroot*, but you can specify a different location using the `WebHostBuilder`.</span></span>

## <a name="configuration"></a><span data-ttu-id="07f1d-171">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="07f1d-171">Configuration</span></span>

<span data-ttu-id="07f1d-172">ASP.NET Core verwendet ein neues Konfigurationsmodell für die Verarbeitung einfacher Name/Wert-Paare.</span><span class="sxs-lookup"><span data-stu-id="07f1d-172">ASP.NET Core uses a new configuration model for handling simple name-value pairs.</span></span> <span data-ttu-id="07f1d-173">Das neue Konfigurationsmodell basiert nicht auf `System.Configuration` oder *web.config*, sondern ruft Daten aus einer geordneten Menge von Konfigurationsanbietern ab.</span><span class="sxs-lookup"><span data-stu-id="07f1d-173">The new configuration model is not based on `System.Configuration` or *web.config*; rather, it pulls from an ordered set of configuration providers.</span></span> <span data-ttu-id="07f1d-174">Die integrierten Konfigurationsanbieter unterstützen zahlreiche Dateiformate (XML, JSON, INI) und Umgebungsvariablen, durch die eine umgebungsbasierte Konfiguration ermöglicht wird.</span><span class="sxs-lookup"><span data-stu-id="07f1d-174">The built-in configuration providers support a variety of file formats (XML, JSON, INI) and environment variables to enable environment-based configuration.</span></span> <span data-ttu-id="07f1d-175">Sie können auch benutzerdefinierte Konfigurationsanbieter erstellen.</span><span class="sxs-lookup"><span data-stu-id="07f1d-175">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="07f1d-176">Weitere Informationen finden Sie unter [Configuration (Konfiguration)](xref:fundamentals/configuration).</span><span class="sxs-lookup"><span data-stu-id="07f1d-176">For more information, see [Configuration](xref:fundamentals/configuration).</span></span>

## <a name="environments"></a><span data-ttu-id="07f1d-177">Umgebungen</span><span class="sxs-lookup"><span data-stu-id="07f1d-177">Environments</span></span>

<span data-ttu-id="07f1d-178">Umgebungen wie „Entwicklung“ und „Produktion“ sind in ASP.NET Core von besonderer Bedeutung und können über Umgebungsvariablen festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="07f1d-178">Environments, like "Development" and "Production", are a first-class notion in ASP.NET Core and can be set using environment variables.</span></span>

<span data-ttu-id="07f1d-179">Weitere Informationen finden Sie unter [Working with Multiple Environments (Verwenden von mehreren Umgebungen)](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="07f1d-179">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

## <a name="net-core-vs-net-framework-runtime"></a><span data-ttu-id="07f1d-180">.NET Core-Runtime im Vergleich zur .NET Framework-Laufzeit</span><span class="sxs-lookup"><span data-stu-id="07f1d-180">.NET Core vs. .NET Framework runtime</span></span>

<span data-ttu-id="07f1d-181">Eine ASP.NET Core-Anwendung kann für die .NET Core- oder .NET Framework-Laufzeit entwickelt werden.</span><span class="sxs-lookup"><span data-stu-id="07f1d-181">An ASP.NET Core application can target the .NET Core or .NET Framework runtime.</span></span> <span data-ttu-id="07f1d-182">Weitere Informationen finden Sie unter [Wahl zwischen .NET Core und .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="07f1d-182">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="additional-information"></a><span data-ttu-id="07f1d-183">Zusätzliche Informationen</span><span class="sxs-lookup"><span data-stu-id="07f1d-183">Additional information</span></span>

<span data-ttu-id="07f1d-184">Weitere Informationen finden Sie auch in folgenden Themen:</span><span class="sxs-lookup"><span data-stu-id="07f1d-184">See also the following topics:</span></span>

- [<span data-ttu-id="07f1d-185">Fehlerbehandlung</span><span class="sxs-lookup"><span data-stu-id="07f1d-185">Error Handling</span></span>](xref:fundamentals/error-handling)
- [<span data-ttu-id="07f1d-186">File Provider (Dateianbieter)</span><span class="sxs-lookup"><span data-stu-id="07f1d-186">File Providers</span></span>](xref:fundamentals/file-providers)
- [<span data-ttu-id="07f1d-187">Globalization and localization (Globalisierung und Lokalisierung)</span><span class="sxs-lookup"><span data-stu-id="07f1d-187">Globalization and localization</span></span>](xref:fundamentals/localization)
- [<span data-ttu-id="07f1d-188">Logging (Protokollierung)</span><span class="sxs-lookup"><span data-stu-id="07f1d-188">Logging</span></span>](xref:fundamentals/logging)
- [<span data-ttu-id="07f1d-189">Managing Application State (Verwalten eines Anwendungszustands)</span><span class="sxs-lookup"><span data-stu-id="07f1d-189">Managing Application State</span></span>](xref:fundamentals/app-state)
