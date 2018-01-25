---
title: Open Web Interface for .NET (OWIN)
author: ardalis
description: "Ermitteln Sie, wie ASP.NET Core die offene Webschnittstelle für .NET (OWIN) unterstützt die Web-apps von Webservern entkoppelt werden können."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/owin
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 42ffa01745b7a492b3b8cb2778805f254863b890
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-open-web-interface-for-net-owin"></a><span data-ttu-id="1d886-103">Einführung in die Weboberfläche für .NET (OWIN) zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="1d886-103">Introduction to Open Web Interface for .NET (OWIN)</span></span>

<span data-ttu-id="1d886-104">Durch [Steve Smith](https://ardalis.com/) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1d886-104">By [Steve Smith](https://ardalis.com/) and  [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1d886-105">ASP.NET Core unterstützt Open Web Interface for .NET (OWIN).</span><span class="sxs-lookup"><span data-stu-id="1d886-105">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="1d886-106">Mit OWIN können Web-Apps von Webservern entkoppelt werden.</span><span class="sxs-lookup"><span data-stu-id="1d886-106">OWIN allows web apps to be decoupled from web servers.</span></span> <span data-ttu-id="1d886-107">Definiert ein gängiges Verfahren für die Middleware, die in einer Pipeline zur Verarbeitung von Anforderungen und zugeordneten Antworten verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="1d886-107">It defines a standard way for middleware to be used in a pipeline to handle requests and associated responses.</span></span> <span data-ttu-id="1d886-108">ASP.NET Core-Anwendungen und Middleware können mit OWIN-basierten Anwendungen, Server und Middleware zusammenarbeiten.</span><span class="sxs-lookup"><span data-stu-id="1d886-108">ASP.NET Core applications and middleware can interoperate with OWIN-based applications, servers, and middleware.</span></span>

<span data-ttu-id="1d886-109">OWIN stellt eine entkoppeln, die zwei Frameworks mit unterschiedlichen Objektmodelle zusammen verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="1d886-109">OWIN provides a decoupling layer that allows two frameworks with disparate object models to be used together.</span></span> <span data-ttu-id="1d886-110">Die `Microsoft.AspNetCore.Owin` Paket stellt zwei adapterimplementierungen bereit:</span><span class="sxs-lookup"><span data-stu-id="1d886-110">The `Microsoft.AspNetCore.Owin` package provides two adapter implementations:</span></span>
- <span data-ttu-id="1d886-111">ASP.NET Core zu OWIN</span><span class="sxs-lookup"><span data-stu-id="1d886-111">ASP.NET Core to OWIN</span></span> 
- <span data-ttu-id="1d886-112">OWIN zu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1d886-112">OWIN to ASP.NET Core</span></span>

<span data-ttu-id="1d886-113">Dadurch können ASP.NET Core gehostet werden, zusätzlich zu einem OWIN kompatiblen Server /-Host oder für andere OWIN kompatiblen Komponenten zusätzlich zu ASP.NET Core ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="1d886-113">This allows ASP.NET Core to be hosted on top of an OWIN compatible server/host, or for other OWIN compatible components to be run on top of ASP.NET Core.</span></span>

<span data-ttu-id="1d886-114">Hinweis: Mit diesen Adaptern mit Leistungseinbußen kommt.</span><span class="sxs-lookup"><span data-stu-id="1d886-114">Note: Using these adapters comes with a performance cost.</span></span> <span data-ttu-id="1d886-115">Anwendungen, die nur ASP.NET Core-Komponenten verwenden darf nicht das Owin-Paket oder ein Adapter verwenden.</span><span class="sxs-lookup"><span data-stu-id="1d886-115">Applications using only ASP.NET Core components shouldn't use the Owin package or adapters.</span></span>

<span data-ttu-id="1d886-116">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1d886-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="running-owin-middleware-in-the-aspnet-pipeline"></a><span data-ttu-id="1d886-117">OWIN-Middleware in der ASP.NET-Pipeline ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="1d886-117">Running OWIN middleware in the ASP.NET pipeline</span></span>

<span data-ttu-id="1d886-118">ASP.NET Core des owin-Unterstützung bereitgestellt wird, als Teil der `Microsoft.AspNetCore.Owin` Paket.</span><span class="sxs-lookup"><span data-stu-id="1d886-118">ASP.NET Core's OWIN support is deployed as part of the `Microsoft.AspNetCore.Owin` package.</span></span> <span data-ttu-id="1d886-119">Sie können zur Installation dieses Pakets OWIN-Unterstützung in Ihr Projekt importieren.</span><span class="sxs-lookup"><span data-stu-id="1d886-119">You can import OWIN support into your project by installing this package.</span></span>

<span data-ttu-id="1d886-120">OWIN-Middleware entspricht der [OWIN-Spezifikation](http://owin.org/spec/spec/owin-1.0.0.html), wofür ein `Func<IDictionary<string, object>, Task>` Schnittstelle und bestimmte Schlüssel festgelegt werden (z. B. `owin.ResponseBody`).</span><span class="sxs-lookup"><span data-stu-id="1d886-120">OWIN middleware conforms to the [OWIN specification](http://owin.org/spec/spec/owin-1.0.0.html), which requires a `Func<IDictionary<string, object>, Task>` interface, and specific keys be set (such as `owin.ResponseBody`).</span></span> <span data-ttu-id="1d886-121">Die folgenden einfachen owin-Middleware wird "Hello World" angezeigt:</span><span class="sxs-lookup"><span data-stu-id="1d886-121">The following simple OWIN middleware displays "Hello World":</span></span>

```csharp
public Task OwinHello(IDictionary<string, object> environment)
{
    string responseText = "Hello World via OWIN";
    byte[] responseBytes = Encoding.UTF8.GetBytes(responseText);

    // OWIN Environment Keys: http://owin.org/spec/spec/owin-1.0.0.html
    var responseStream = (Stream)environment["owin.ResponseBody"];
    var responseHeaders = (IDictionary<string, string[]>)environment["owin.ResponseHeaders"];

    responseHeaders["Content-Length"] = new string[] { responseBytes.Length.ToString(CultureInfo.InvariantCulture) };
    responseHeaders["Content-Type"] = new string[] { "text/plain" };

    return responseStream.WriteAsync(responseBytes, 0, responseBytes.Length);
}
```

<span data-ttu-id="1d886-122">Die Signatur Beispiel gibt eine `Task` und akzeptiert eine `IDictionary<string, object>` OWIN wie erforderlich.</span><span class="sxs-lookup"><span data-stu-id="1d886-122">The sample signature returns a `Task` and accepts an `IDictionary<string, object>` as required by OWIN.</span></span>

<span data-ttu-id="1d886-123">Der folgende Code veranschaulicht das Hinzufügen der `OwinHello` Middleware (siehe oben), an der ASP.NET-Pipeline mit der `UseOwin` -Erweiterungsmethode.</span><span class="sxs-lookup"><span data-stu-id="1d886-123">The following code shows how to add the `OwinHello` middleware (shown above) to the ASP.NET pipeline with the `UseOwin` extension method.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}
```

<span data-ttu-id="1d886-124">Sie können andere Aktionen, erfolgen in der OWIN-Pipeline konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="1d886-124">You can configure other actions to take place within the OWIN pipeline.</span></span>

> [!NOTE]
> <span data-ttu-id="1d886-125">Antwort-Header sollte nur geändert werden, vor dem ersten Schreiben in den Antwortstream.</span><span class="sxs-lookup"><span data-stu-id="1d886-125">Response headers should only be modified prior to the first write to the response stream.</span></span>

> [!NOTE]
> <span data-ttu-id="1d886-126">Mehrere Aufrufe `UseOwin` wird abgeraten, zur Verbesserung der Leistung.</span><span class="sxs-lookup"><span data-stu-id="1d886-126">Multiple calls to `UseOwin` is discouraged for performance reasons.</span></span> <span data-ttu-id="1d886-127">Owin-Komponenten funktionieren am besten, wenn einer Gruppe zusammengefasst.</span><span class="sxs-lookup"><span data-stu-id="1d886-127">OWIN components will operate best if grouped together.</span></span>

```csharp
app.UseOwin(pipeline =>
{
    pipeline(next =>
    {
        // do something before
        return OwinHello;
        // do something after
    });
});
```

<a name="hosting-on-owin"></a>

## <a name="using-aspnet-hosting-on-an-owin-based-server"></a><span data-ttu-id="1d886-128">Verwenden von ASP.NET-Hosting auf einen OWIN-basierten server</span><span class="sxs-lookup"><span data-stu-id="1d886-128">Using ASP.NET Hosting on an OWIN-based server</span></span>

<span data-ttu-id="1d886-129">OWIN-basierte Server können ASP.NET-Anwendungen hosten.</span><span class="sxs-lookup"><span data-stu-id="1d886-129">OWIN-based servers can host ASP.NET applications.</span></span> <span data-ttu-id="1d886-130">Ein solcher Server ist [Nowin](https://github.com/Bobris/Nowin), einen Webserver .NET OWIN.</span><span class="sxs-lookup"><span data-stu-id="1d886-130">One such server is [Nowin](https://github.com/Bobris/Nowin), a .NET OWIN web server.</span></span> <span data-ttu-id="1d886-131">Im Beispiel für diesen Artikel haben ich enthalten ein Projekt, das Nowin verweist und verwendet er zum Erstellen einer `IServer` ASP.NET Core selbst hosten.</span><span class="sxs-lookup"><span data-stu-id="1d886-131">In the sample for this article, I've included a project that references Nowin and uses it to create an `IServer` capable of self-hosting ASP.NET Core.</span></span>

[!code-csharp[Main](owin/sample/src/NowinSample/Program.cs?highlight=15)]

<span data-ttu-id="1d886-132">`IServer`ist eine Schnittstelle, die erfordert eine `Features` Eigenschaft und eine `Start` Methode.</span><span class="sxs-lookup"><span data-stu-id="1d886-132">`IServer` is an interface that requires an `Features` property and a `Start` method.</span></span>

<span data-ttu-id="1d886-133">`Start`ist verantwortlich für das Konfigurieren und starten den Server, der in diesem Fall durch eine Reihe von fluent-API-Aufrufe abgeschlossen ist, die Adressen, die analysiert die IServerAddressesFeature festlegen.</span><span class="sxs-lookup"><span data-stu-id="1d886-133">`Start` is responsible for configuring and starting the server, which in this case is done through a series of fluent API calls that set addresses parsed from the IServerAddressesFeature.</span></span> <span data-ttu-id="1d886-134">Beachten Sie, dass die fluent-Konfiguration von der `_builder` Variable gibt an, dass Anforderungen vom behandelt werden die `appFunc` weiter oben in der Methode definiert.</span><span class="sxs-lookup"><span data-stu-id="1d886-134">Note that the fluent configuration of the `_builder` variable specifies that requests will be handled by the `appFunc` defined earlier in the method.</span></span> <span data-ttu-id="1d886-135">Dies `Func` für jede Anforderung zur Verarbeitung von eingehenden Anforderungen aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="1d886-135">This `Func` is called on each request to process incoming requests.</span></span>

<span data-ttu-id="1d886-136">Wir fügen auch eine `IWebHostBuilder` Erweiterung hinzufügen und Konfigurieren des Servers Nowin zu erleichtern.</span><span class="sxs-lookup"><span data-stu-id="1d886-136">We'll also add an `IWebHostBuilder` extension to make it easy to add and configure the Nowin server.</span></span>

```csharp
using System;
using Microsoft.AspNetCore.Hosting.Server;
using Microsoft.Extensions.DependencyInjection;
using Nowin;
using NowinSample;

namespace Microsoft.AspNetCore.Hosting
{
    public static class NowinWebHostBuilderExtensions
    {
        public static IWebHostBuilder UseNowin(this IWebHostBuilder builder)
        {
            return builder.ConfigureServices(services =>
            {
                services.AddSingleton<IServer, NowinServer>();
            });
        }

        public static IWebHostBuilder UseNowin(this IWebHostBuilder builder, Action<ServerBuilder> configure)
        {
            builder.ConfigureServices(services =>
            {
                services.Configure(configure);
            });
            return builder.UseNowin();
        }
    }
}
```

<span data-ttu-id="1d886-137">Dabei vorhanden ist, ist erforderlich, führen Sie eine ASP.NET-Anwendung mit diesen benutzerdefinierten Server rufen Sie die Erweiterung in *"Program.cs"*:</span><span class="sxs-lookup"><span data-stu-id="1d886-137">With this in place, all that's required to run an ASP.NET application using this custom server to call the extension in *Program.cs*:</span></span>

```csharp

using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Hosting;

namespace NowinSample
{
    public class Program
    {
        public static void Main(string[] args)
        {
            var host = new WebHostBuilder()
                .UseNowin()
                .UseContentRoot(Directory.GetCurrentDirectory())
                .UseIISIntegration()
                .UseStartup<Startup>()
                .Build();

            host.Run();
        }
    }
}

```

<span data-ttu-id="1d886-138">Erfahren Sie mehr über ASP.NET [Server](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="1d886-138">Learn more about ASP.NET [Servers](servers/index.md).</span></span>

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a><span data-ttu-id="1d886-139">Führen Sie ASP.NET Core auf einen OWIN-basierten Server und verwenden Sie die WebSockets-Unterstützung</span><span class="sxs-lookup"><span data-stu-id="1d886-139">Run ASP.NET Core on an OWIN-based server and use its WebSockets support</span></span>

<span data-ttu-id="1d886-140">Ein weiteres Beispiel für Funktionen wie OWIN-basierten Servern genutzt werden, indem Sie ASP.NET Core ist der Zugriff auf Funktionen wie WebSockets.</span><span class="sxs-lookup"><span data-stu-id="1d886-140">Another example of how OWIN-based servers' features can be leveraged by ASP.NET Core is access to features like WebSockets.</span></span> <span data-ttu-id="1d886-141">Der im vorherigen Beispiel verwendete .NET OWIN-Webserver hat die Unterstützung für WebSockets integriert, die von einer ASP.NET Core-Anwendung genutzt werden können.</span><span class="sxs-lookup"><span data-stu-id="1d886-141">The .NET OWIN web server used in the previous example has support for Web Sockets built in, which can be leveraged by an ASP.NET Core application.</span></span> <span data-ttu-id="1d886-142">Das folgende Beispiel zeigt eine einfache Web-app, die WebSockets unterstützt und wiederholt wieder alles, was über WebSockets an den Server gesendet.</span><span class="sxs-lookup"><span data-stu-id="1d886-142">The example below shows a simple web app that supports Web Sockets and echoes back everything sent to the server through WebSockets.</span></span>

```csharp
public class Startup
{
    public void Configure(IApplicationBuilder app)
    {
        app.Use(async (context, next) =>
        {
            if (context.WebSockets.IsWebSocketRequest)
            {
                WebSocket webSocket = await context.WebSockets.AcceptWebSocketAsync();
                await EchoWebSocket(webSocket);
            }
            else
            {
                await next();
            }
        });

        app.Run(context =>
        {
            return context.Response.WriteAsync("Hello World");
        });
    }

    private async Task EchoWebSocket(WebSocket webSocket)
    {
        byte[] buffer = new byte[1024];
        WebSocketReceiveResult received = await webSocket.ReceiveAsync(
            new ArraySegment<byte>(buffer), CancellationToken.None);

        while (!webSocket.CloseStatus.HasValue)
        {
            // Echo anything we receive
            await webSocket.SendAsync(new ArraySegment<byte>(buffer, 0, received.Count), 
                received.MessageType, received.EndOfMessage, CancellationToken.None);

            received = await webSocket.ReceiveAsync(new ArraySegment<byte>(buffer), 
                CancellationToken.None);
        }

        await webSocket.CloseAsync(webSocket.CloseStatus.Value, 
            webSocket.CloseStatusDescription, CancellationToken.None);
    }
}
```

<span data-ttu-id="1d886-143">Dies [Beispiel](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) konfiguriert ist, unter Verwendung des gleichen `NowinServer` wie das vorherige - ist der einzige Unterschied in der Konfiguration der Anwendung in seiner `Configure` Methode.</span><span class="sxs-lookup"><span data-stu-id="1d886-143">This [sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) is configured using the same `NowinServer` as the previous one - the only difference is in how the application is configured in its `Configure` method.</span></span> <span data-ttu-id="1d886-144">Einen Test mit [einen einfachen Websocket-Client](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) Anwendung veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="1d886-144">A test using [a simple websocket client](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) demonstrates  the application:</span></span>

![Web-Socket-Testclient](owin/_static/websocket-test.png)

## <a name="owin-environment"></a><span data-ttu-id="1d886-146">OWIN-Umgebung</span><span class="sxs-lookup"><span data-stu-id="1d886-146">OWIN environment</span></span>

<span data-ttu-id="1d886-147">Erstellen Sie eine OWIN-Umgebung verwenden die `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="1d886-147">You can construct a OWIN environment using the `HttpContext`.</span></span>

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a><span data-ttu-id="1d886-148">OWIN-Schlüssel</span><span class="sxs-lookup"><span data-stu-id="1d886-148">OWIN keys</span></span>

<span data-ttu-id="1d886-149">OWIN richtet sich nach einem `IDictionary<string,object>` Objekt zur Übermittlung von Informationen in der gesamten einen HTTP-Anforderung/Antwort-Austausch.</span><span class="sxs-lookup"><span data-stu-id="1d886-149">OWIN depends on an `IDictionary<string,object>` object to communicate information throughout an HTTP Request/Response exchange.</span></span> <span data-ttu-id="1d886-150">ASP.NET Core implementiert die unten aufgeführten Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="1d886-150">ASP.NET Core implements the keys listed below.</span></span> <span data-ttu-id="1d886-151">Finden Sie unter der [primären-Spezifikation, Erweiterungen](http://owin.org/#spec), und [owin-Schlüssel Richtlinien und allgemeine Schlüssel](http://owin.org/spec/spec/CommonKeys.html).</span><span class="sxs-lookup"><span data-stu-id="1d886-151">See the [primary specification, extensions](http://owin.org/#spec), and [OWIN Key Guidelines and Common Keys](http://owin.org/spec/spec/CommonKeys.html).</span></span>

### <a name="request-data-owin-v100"></a><span data-ttu-id="1d886-152">Anfordern von Daten (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="1d886-152">Request Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="1d886-153">Key</span><span class="sxs-lookup"><span data-stu-id="1d886-153">Key</span></span>               | <span data-ttu-id="1d886-154">Wert (Datentyp)</span><span class="sxs-lookup"><span data-stu-id="1d886-154">Value (type)</span></span> | <span data-ttu-id="1d886-155">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="1d886-155">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="1d886-156">owin.RequestScheme</span><span class="sxs-lookup"><span data-stu-id="1d886-156">owin.RequestScheme</span></span> | `String` |  |
| <span data-ttu-id="1d886-157">owin.RequestMethod</span><span class="sxs-lookup"><span data-stu-id="1d886-157">owin.RequestMethod</span></span>  | `String` | |    
| <span data-ttu-id="1d886-158">owin.RequestPathBase</span><span class="sxs-lookup"><span data-stu-id="1d886-158">owin.RequestPathBase</span></span>  | `String` | |    
| <span data-ttu-id="1d886-159">owin.RequestPath</span><span class="sxs-lookup"><span data-stu-id="1d886-159">owin.RequestPath</span></span> | `String` | |     
| <span data-ttu-id="1d886-160">owin.RequestQueryString</span><span class="sxs-lookup"><span data-stu-id="1d886-160">owin.RequestQueryString</span></span>  | `String` | |    
| <span data-ttu-id="1d886-161">owin.RequestProtocol</span><span class="sxs-lookup"><span data-stu-id="1d886-161">owin.RequestProtocol</span></span>  | `String` | |    
| <span data-ttu-id="1d886-162">owin.RequestHeaders</span><span class="sxs-lookup"><span data-stu-id="1d886-162">owin.RequestHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="1d886-163">owin.RequestBody</span><span class="sxs-lookup"><span data-stu-id="1d886-163">owin.RequestBody</span></span> | `Stream`  | |

### <a name="request-data-owin-v110"></a><span data-ttu-id="1d886-164">Anfordern von Daten (OWIN v1.1.0)</span><span class="sxs-lookup"><span data-stu-id="1d886-164">Request Data (OWIN v1.1.0)</span></span>

| <span data-ttu-id="1d886-165">Key</span><span class="sxs-lookup"><span data-stu-id="1d886-165">Key</span></span>               | <span data-ttu-id="1d886-166">Wert (Datentyp)</span><span class="sxs-lookup"><span data-stu-id="1d886-166">Value (type)</span></span> | <span data-ttu-id="1d886-167">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="1d886-167">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="1d886-168">owin.RequestId</span><span class="sxs-lookup"><span data-stu-id="1d886-168">owin.RequestId</span></span> | `String` | <span data-ttu-id="1d886-169">Optional</span><span class="sxs-lookup"><span data-stu-id="1d886-169">Optional</span></span> |

### <a name="response-data-owin-v100"></a><span data-ttu-id="1d886-170">Antwortdaten (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="1d886-170">Response Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="1d886-171">Key</span><span class="sxs-lookup"><span data-stu-id="1d886-171">Key</span></span>               | <span data-ttu-id="1d886-172">Wert (Datentyp)</span><span class="sxs-lookup"><span data-stu-id="1d886-172">Value (type)</span></span> | <span data-ttu-id="1d886-173">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="1d886-173">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="1d886-174">owin.ResponseStatusCode</span><span class="sxs-lookup"><span data-stu-id="1d886-174">owin.ResponseStatusCode</span></span> | `int` | <span data-ttu-id="1d886-175">Optional</span><span class="sxs-lookup"><span data-stu-id="1d886-175">Optional</span></span> |
| <span data-ttu-id="1d886-176">owin.ResponseReasonPhrase</span><span class="sxs-lookup"><span data-stu-id="1d886-176">owin.ResponseReasonPhrase</span></span> | `String` | <span data-ttu-id="1d886-177">Optional</span><span class="sxs-lookup"><span data-stu-id="1d886-177">Optional</span></span> |
| <span data-ttu-id="1d886-178">owin.ResponseHeaders</span><span class="sxs-lookup"><span data-stu-id="1d886-178">owin.ResponseHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="1d886-179">owin.ResponseBody</span><span class="sxs-lookup"><span data-stu-id="1d886-179">owin.ResponseBody</span></span> | `Stream`  | |


### <a name="other-data-owin-v100"></a><span data-ttu-id="1d886-180">Andere Daten (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="1d886-180">Other Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="1d886-181">Key</span><span class="sxs-lookup"><span data-stu-id="1d886-181">Key</span></span>               | <span data-ttu-id="1d886-182">Wert (Datentyp)</span><span class="sxs-lookup"><span data-stu-id="1d886-182">Value (type)</span></span> | <span data-ttu-id="1d886-183">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="1d886-183">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="1d886-184">owin.CallCancelled</span><span class="sxs-lookup"><span data-stu-id="1d886-184">owin.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="1d886-185">owin.Version</span><span class="sxs-lookup"><span data-stu-id="1d886-185">owin.Version</span></span>  | `String` | |   


### <a name="common-keys"></a><span data-ttu-id="1d886-186">Allgemeine Schlüssel</span><span class="sxs-lookup"><span data-stu-id="1d886-186">Common Keys</span></span>

| <span data-ttu-id="1d886-187">Key</span><span class="sxs-lookup"><span data-stu-id="1d886-187">Key</span></span>               | <span data-ttu-id="1d886-188">Wert (Datentyp)</span><span class="sxs-lookup"><span data-stu-id="1d886-188">Value (type)</span></span> | <span data-ttu-id="1d886-189">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="1d886-189">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="1d886-190">ssl.ClientCertificate</span><span class="sxs-lookup"><span data-stu-id="1d886-190">ssl.ClientCertificate</span></span> | `X509Certificate` |  |
| <span data-ttu-id="1d886-191">ssl.LoadClientCertAsync</span><span class="sxs-lookup"><span data-stu-id="1d886-191">ssl.LoadClientCertAsync</span></span>  | `Func<Task>` | |    
| <span data-ttu-id="1d886-192">server.RemoteIpAddress</span><span class="sxs-lookup"><span data-stu-id="1d886-192">server.RemoteIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="1d886-193">server.RemotePort</span><span class="sxs-lookup"><span data-stu-id="1d886-193">server.RemotePort</span></span> | `String` | |     
| <span data-ttu-id="1d886-194">server.LocalIpAddress</span><span class="sxs-lookup"><span data-stu-id="1d886-194">server.LocalIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="1d886-195">server.LocalPort</span><span class="sxs-lookup"><span data-stu-id="1d886-195">server.LocalPort</span></span>  | `String` | |    
| <span data-ttu-id="1d886-196">server.IsLocal</span><span class="sxs-lookup"><span data-stu-id="1d886-196">server.IsLocal</span></span>  | `bool` | |    
| <span data-ttu-id="1d886-197">server.OnSendingHeaders</span><span class="sxs-lookup"><span data-stu-id="1d886-197">server.OnSendingHeaders</span></span>  | `Action<Action<object>,object>` | |


### <a name="sendfiles-v030"></a><span data-ttu-id="1d886-198">SendFiles v0.3.0</span><span class="sxs-lookup"><span data-stu-id="1d886-198">SendFiles v0.3.0</span></span>

| <span data-ttu-id="1d886-199">Key</span><span class="sxs-lookup"><span data-stu-id="1d886-199">Key</span></span>               | <span data-ttu-id="1d886-200">Wert (Datentyp)</span><span class="sxs-lookup"><span data-stu-id="1d886-200">Value (type)</span></span> | <span data-ttu-id="1d886-201">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="1d886-201">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="1d886-202">sendfile.SendAsync</span><span class="sxs-lookup"><span data-stu-id="1d886-202">sendfile.SendAsync</span></span> | <span data-ttu-id="1d886-203">Finden Sie unter [Signatur des Delegaten](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="1d886-203">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> | <span data-ttu-id="1d886-204">Pro Anforderung</span><span class="sxs-lookup"><span data-stu-id="1d886-204">Per Request</span></span> |


### <a name="opaque-v030"></a><span data-ttu-id="1d886-205">Nicht transparente v0.3.0</span><span class="sxs-lookup"><span data-stu-id="1d886-205">Opaque v0.3.0</span></span>

| <span data-ttu-id="1d886-206">Key</span><span class="sxs-lookup"><span data-stu-id="1d886-206">Key</span></span>               | <span data-ttu-id="1d886-207">Wert (Datentyp)</span><span class="sxs-lookup"><span data-stu-id="1d886-207">Value (type)</span></span> | <span data-ttu-id="1d886-208">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="1d886-208">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="1d886-209">opaque.Version</span><span class="sxs-lookup"><span data-stu-id="1d886-209">opaque.Version</span></span> | `String` |  |
| <span data-ttu-id="1d886-210">opaque.Upgrade</span><span class="sxs-lookup"><span data-stu-id="1d886-210">opaque.Upgrade</span></span> | `OpaqueUpgrade` | <span data-ttu-id="1d886-211">Finden Sie unter [Signatur des Delegaten](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="1d886-211">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="1d886-212">opaque.Stream</span><span class="sxs-lookup"><span data-stu-id="1d886-212">opaque.Stream</span></span> | `Stream` |  |
| <span data-ttu-id="1d886-213">opaque.CallCancelled</span><span class="sxs-lookup"><span data-stu-id="1d886-213">opaque.CallCancelled</span></span> | `CancellationToken` |  |


### <a name="websocket-v030"></a><span data-ttu-id="1d886-214">WebSocket-v0.3.0</span><span class="sxs-lookup"><span data-stu-id="1d886-214">WebSocket v0.3.0</span></span>

| <span data-ttu-id="1d886-215">Key</span><span class="sxs-lookup"><span data-stu-id="1d886-215">Key</span></span>               | <span data-ttu-id="1d886-216">Wert (Datentyp)</span><span class="sxs-lookup"><span data-stu-id="1d886-216">Value (type)</span></span> | <span data-ttu-id="1d886-217">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="1d886-217">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="1d886-218">websocket.Version</span><span class="sxs-lookup"><span data-stu-id="1d886-218">websocket.Version</span></span> | `String` |  |
| <span data-ttu-id="1d886-219">websocket.Accept</span><span class="sxs-lookup"><span data-stu-id="1d886-219">websocket.Accept</span></span> | `WebSocketAccept` | <span data-ttu-id="1d886-220">Finden Sie unter [Signatur des Delegaten](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="1d886-220">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="1d886-221">websocket.AcceptAlt</span><span class="sxs-lookup"><span data-stu-id="1d886-221">websocket.AcceptAlt</span></span> |  | <span data-ttu-id="1d886-222">Nicht-Spezifikation</span><span class="sxs-lookup"><span data-stu-id="1d886-222">Non-spec</span></span> |
| <span data-ttu-id="1d886-223">websocket.SubProtocol</span><span class="sxs-lookup"><span data-stu-id="1d886-223">websocket.SubProtocol</span></span> | `String` | <span data-ttu-id="1d886-224">Finden Sie unter [RFC6455 Abschnitt 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) Schritt 5.5</span><span class="sxs-lookup"><span data-stu-id="1d886-224">See [RFC6455 Section 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) Step 5.5</span></span> |
| <span data-ttu-id="1d886-225">websocket.SendAsync</span><span class="sxs-lookup"><span data-stu-id="1d886-225">websocket.SendAsync</span></span> | `WebSocketSendAsync` | <span data-ttu-id="1d886-226">Finden Sie unter [Signatur des Delegaten](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="1d886-226">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="1d886-227">websocket.ReceiveAsync</span><span class="sxs-lookup"><span data-stu-id="1d886-227">websocket.ReceiveAsync</span></span> | `WebSocketReceiveAsync` | <span data-ttu-id="1d886-228">Finden Sie unter [Signatur des Delegaten](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="1d886-228">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="1d886-229">websocket.CloseAsync</span><span class="sxs-lookup"><span data-stu-id="1d886-229">websocket.CloseAsync</span></span> | `WebSocketCloseAsync` | <span data-ttu-id="1d886-230">Finden Sie unter [Signatur des Delegaten](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="1d886-230">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="1d886-231">websocket.CallCancelled</span><span class="sxs-lookup"><span data-stu-id="1d886-231">websocket.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="1d886-232">websocket.ClientCloseStatus</span><span class="sxs-lookup"><span data-stu-id="1d886-232">websocket.ClientCloseStatus</span></span> | `int` | <span data-ttu-id="1d886-233">Optional</span><span class="sxs-lookup"><span data-stu-id="1d886-233">Optional</span></span> |
| <span data-ttu-id="1d886-234">websocket.ClientCloseDescription</span><span class="sxs-lookup"><span data-stu-id="1d886-234">websocket.ClientCloseDescription</span></span> | `String` | <span data-ttu-id="1d886-235">Optional</span><span class="sxs-lookup"><span data-stu-id="1d886-235">Optional</span></span> |


## <a name="additional-resources"></a><span data-ttu-id="1d886-236">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="1d886-236">Additional Resources</span></span>

* [<span data-ttu-id="1d886-237">Middleware</span><span class="sxs-lookup"><span data-stu-id="1d886-237">Middleware</span></span>](middleware.md)

* [<span data-ttu-id="1d886-238">Server</span><span class="sxs-lookup"><span data-stu-id="1d886-238">Servers</span></span>](servers/index.md)
