---
title: Open Web Interface for .NET (OWIN)
author: ardalis
description: "Ermitteln Sie, wie ASP.NET Core die offene Webschnittstelle für .NET (OWIN) unterstützt die Web-apps von Webservern entkoppelt werden können."
keywords: "ASP.NET Core, offene Webschnittstelle für .NET, OWIN"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 70c4e6bc-a773-4039-96ec-6fe557c9369d
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/owin
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e2ee970a1c9cd05ebee76b92c3e2c7c6c6cc6ef8
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2017
---
# <a name="introduction-to-open-web-interface-for-net-owin"></a><span data-ttu-id="41261-104">Einführung in die Weboberfläche für .NET (OWIN) zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="41261-104">Introduction to Open Web Interface for .NET (OWIN)</span></span>

<span data-ttu-id="41261-105">Durch [Steve Smith](https://ardalis.com/) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="41261-105">By [Steve Smith](https://ardalis.com/) and  [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="41261-106">ASP.NET Core unterstützt Open Web Interface for .NET (OWIN).</span><span class="sxs-lookup"><span data-stu-id="41261-106">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="41261-107">Mit OWIN können Web-Apps von Webservern entkoppelt werden.</span><span class="sxs-lookup"><span data-stu-id="41261-107">OWIN allows web apps to be decoupled from web servers.</span></span> <span data-ttu-id="41261-108">Definiert ein gängiges Verfahren für die Middleware, die in einer Pipeline zur Verarbeitung von Anforderungen und zugeordneten Antworten verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="41261-108">It defines a standard way for middleware to be used in a pipeline to handle requests and associated responses.</span></span> <span data-ttu-id="41261-109">ASP.NET Core-Anwendungen und Middleware können mit OWIN-basierten Anwendungen, Server und Middleware zusammenarbeiten.</span><span class="sxs-lookup"><span data-stu-id="41261-109">ASP.NET Core applications and middleware can interoperate with OWIN-based applications, servers, and middleware.</span></span>

<span data-ttu-id="41261-110">OWIN stellt eine entkoppeln, die zwei Frameworks mit unterschiedlichen Objektmodelle zusammen verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="41261-110">OWIN provides a decoupling layer that allows two frameworks with disparate object models to be used together.</span></span> <span data-ttu-id="41261-111">Die `Microsoft.AspNetCore.Owin` Paket stellt zwei adapterimplementierungen bereit:</span><span class="sxs-lookup"><span data-stu-id="41261-111">The `Microsoft.AspNetCore.Owin` package provides two adapter implementations:</span></span>
- <span data-ttu-id="41261-112">ASP.NET Core zu OWIN</span><span class="sxs-lookup"><span data-stu-id="41261-112">ASP.NET Core to OWIN</span></span> 
- <span data-ttu-id="41261-113">OWIN zu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="41261-113">OWIN to ASP.NET Core</span></span>

<span data-ttu-id="41261-114">Dadurch können ASP.NET Core gehostet werden, zusätzlich zu einem OWIN kompatiblen Server /-Host oder für andere OWIN kompatiblen Komponenten zusätzlich zu ASP.NET Core ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="41261-114">This allows ASP.NET Core to be hosted on top of an OWIN compatible server/host, or for other OWIN compatible components to be run on top of ASP.NET Core.</span></span>

<span data-ttu-id="41261-115">Hinweis: Mit diesen Adaptern mit Leistungseinbußen kommt.</span><span class="sxs-lookup"><span data-stu-id="41261-115">Note: Using these adapters comes with a performance cost.</span></span> <span data-ttu-id="41261-116">Anwendungen mit nur ASP.NET Core-Komponenten sollten nicht die Owin-Paket oder ein Adapter verwenden.</span><span class="sxs-lookup"><span data-stu-id="41261-116">Applications using only ASP.NET Core components should not use the Owin package or adapters.</span></span>

<span data-ttu-id="41261-117">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="41261-117">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="running-owin-middleware-in-the-aspnet-pipeline"></a><span data-ttu-id="41261-118">OWIN-Middleware in der ASP.NET-Pipeline ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="41261-118">Running OWIN middleware in the ASP.NET pipeline</span></span>

<span data-ttu-id="41261-119">ASP.NET Core des owin-Unterstützung bereitgestellt wird, als Teil der `Microsoft.AspNetCore.Owin` Paket.</span><span class="sxs-lookup"><span data-stu-id="41261-119">ASP.NET Core's OWIN support is deployed as part of the `Microsoft.AspNetCore.Owin` package.</span></span> <span data-ttu-id="41261-120">Sie können zur Installation dieses Pakets OWIN-Unterstützung in Ihr Projekt importieren.</span><span class="sxs-lookup"><span data-stu-id="41261-120">You can import OWIN support into your project by installing this package.</span></span>

<span data-ttu-id="41261-121">OWIN-Middleware entspricht der [OWIN-Spezifikation](http://owin.org/spec/spec/owin-1.0.0.html), wofür ein `Func<IDictionary<string, object>, Task>` Schnittstelle und bestimmte Schlüssel festgelegt werden (z. B. `owin.ResponseBody`).</span><span class="sxs-lookup"><span data-stu-id="41261-121">OWIN middleware conforms to the [OWIN specification](http://owin.org/spec/spec/owin-1.0.0.html), which requires a `Func<IDictionary<string, object>, Task>` interface, and specific keys be set (such as `owin.ResponseBody`).</span></span> <span data-ttu-id="41261-122">Die folgenden einfachen owin-Middleware wird "Hello World" angezeigt:</span><span class="sxs-lookup"><span data-stu-id="41261-122">The following simple OWIN middleware displays "Hello World":</span></span>

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

<span data-ttu-id="41261-123">Die Signatur Beispiel gibt eine `Task` und akzeptiert eine `IDictionary<string, object>` OWIN wie erforderlich.</span><span class="sxs-lookup"><span data-stu-id="41261-123">The sample signature returns a `Task` and accepts an `IDictionary<string, object>` as required by OWIN.</span></span>

<span data-ttu-id="41261-124">Der folgende Code veranschaulicht das Hinzufügen der `OwinHello` Middleware (siehe oben), an der ASP.NET-Pipeline mit der `UseOwin` -Erweiterungsmethode.</span><span class="sxs-lookup"><span data-stu-id="41261-124">The following code shows how to add the `OwinHello` middleware (shown above) to the ASP.NET pipeline with the `UseOwin` extension method.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}
```

<span data-ttu-id="41261-125">Sie können andere Aktionen, erfolgen in der OWIN-Pipeline konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="41261-125">You can configure other actions to take place within the OWIN pipeline.</span></span>

> [!NOTE]
> <span data-ttu-id="41261-126">Antwort-Header sollte nur geändert werden, vor dem ersten Schreiben in den Antwortstream.</span><span class="sxs-lookup"><span data-stu-id="41261-126">Response headers should only be modified prior to the first write to the response stream.</span></span>

> [!NOTE]
> <span data-ttu-id="41261-127">Mehrere Aufrufe `UseOwin` wird abgeraten, zur Verbesserung der Leistung.</span><span class="sxs-lookup"><span data-stu-id="41261-127">Multiple calls to `UseOwin` is discouraged for performance reasons.</span></span> <span data-ttu-id="41261-128">Owin-Komponenten funktionieren am besten, wenn einer Gruppe zusammengefasst.</span><span class="sxs-lookup"><span data-stu-id="41261-128">OWIN components will operate best if grouped together.</span></span>

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

## <a name="using-aspnet-hosting-on-an-owin-based-server"></a><span data-ttu-id="41261-129">Verwenden von ASP.NET-Hosting auf einen OWIN-basierten server</span><span class="sxs-lookup"><span data-stu-id="41261-129">Using ASP.NET Hosting on an OWIN-based server</span></span>

<span data-ttu-id="41261-130">OWIN-basierte Server können ASP.NET-Anwendungen hosten.</span><span class="sxs-lookup"><span data-stu-id="41261-130">OWIN-based servers can host ASP.NET applications.</span></span> <span data-ttu-id="41261-131">Ein solcher Server ist [Nowin](https://github.com/Bobris/Nowin), einen Webserver .NET OWIN.</span><span class="sxs-lookup"><span data-stu-id="41261-131">One such server is [Nowin](https://github.com/Bobris/Nowin), a .NET OWIN web server.</span></span> <span data-ttu-id="41261-132">Im Beispiel für diesen Artikel haben ich enthalten ein Projekt, das Nowin verweist und verwendet er zum Erstellen einer `IServer` ASP.NET Core selbst hosten.</span><span class="sxs-lookup"><span data-stu-id="41261-132">In the sample for this article, I've included a project that references Nowin and uses it to create an `IServer` capable of self-hosting ASP.NET Core.</span></span>

[!code-csharp[Main](owin/sample/src/NowinSample/Program.cs?highlight=15)]

<span data-ttu-id="41261-133">`IServer`ist eine Schnittstelle, die erfordert eine `Features` Eigenschaft und eine `Start` Methode.</span><span class="sxs-lookup"><span data-stu-id="41261-133">`IServer` is an interface that requires an `Features` property and a `Start` method.</span></span>

<span data-ttu-id="41261-134">`Start`ist verantwortlich für das Konfigurieren und starten den Server, der in diesem Fall durch eine Reihe von fluent-API-Aufrufe abgeschlossen ist, die Adressen, die analysiert die IServerAddressesFeature festlegen.</span><span class="sxs-lookup"><span data-stu-id="41261-134">`Start` is responsible for configuring and starting the server, which in this case is done through a series of fluent API calls that set addresses parsed from the IServerAddressesFeature.</span></span> <span data-ttu-id="41261-135">Beachten Sie, dass die fluent-Konfiguration von der `_builder` Variable gibt an, dass Anforderungen vom behandelt werden die `appFunc` weiter oben in der Methode definiert.</span><span class="sxs-lookup"><span data-stu-id="41261-135">Note that the fluent configuration of the `_builder` variable specifies that requests will be handled by the `appFunc` defined earlier in the method.</span></span> <span data-ttu-id="41261-136">Dies `Func` für jede Anforderung zur Verarbeitung von eingehenden Anforderungen aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="41261-136">This `Func` is called on each request to process incoming requests.</span></span>

<span data-ttu-id="41261-137">Wir fügen auch eine `IWebHostBuilder` Erweiterung hinzufügen und Konfigurieren des Servers Nowin zu erleichtern.</span><span class="sxs-lookup"><span data-stu-id="41261-137">We'll also add an `IWebHostBuilder` extension to make it easy to add and configure the Nowin server.</span></span>

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

<span data-ttu-id="41261-138">Dabei vorhanden ist, ist erforderlich, führen Sie eine ASP.NET-Anwendung mit diesen benutzerdefinierten Server rufen Sie die Erweiterung in *"Program.cs"*:</span><span class="sxs-lookup"><span data-stu-id="41261-138">With this in place, all that's required to run an ASP.NET application using this custom server to call the extension in *Program.cs*:</span></span>

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

<span data-ttu-id="41261-139">Erfahren Sie mehr über ASP.NET [Server](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="41261-139">Learn more about ASP.NET [Servers](servers/index.md).</span></span>

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a><span data-ttu-id="41261-140">Führen Sie ASP.NET Core auf einen OWIN-basierten Server und verwenden Sie die WebSockets-Unterstützung</span><span class="sxs-lookup"><span data-stu-id="41261-140">Run ASP.NET Core on an OWIN-based server and use its WebSockets support</span></span>

<span data-ttu-id="41261-141">Ein weiteres Beispiel für Funktionen wie OWIN-basierten Servern genutzt werden, indem Sie ASP.NET Core ist der Zugriff auf Funktionen wie WebSockets.</span><span class="sxs-lookup"><span data-stu-id="41261-141">Another example of how OWIN-based servers' features can be leveraged by ASP.NET Core is access to features like WebSockets.</span></span> <span data-ttu-id="41261-142">Der im vorherigen Beispiel verwendete .NET OWIN-Webserver hat die Unterstützung für WebSockets integriert, die von einer ASP.NET Core-Anwendung genutzt werden können.</span><span class="sxs-lookup"><span data-stu-id="41261-142">The .NET OWIN web server used in the previous example has support for Web Sockets built in, which can be leveraged by an ASP.NET Core application.</span></span> <span data-ttu-id="41261-143">Das folgende Beispiel zeigt eine einfache Web-app, die WebSockets unterstützt und wiederholt wieder alles, was über WebSockets an den Server gesendet.</span><span class="sxs-lookup"><span data-stu-id="41261-143">The example below shows a simple web app that supports Web Sockets and echoes back everything sent to the server through WebSockets.</span></span>

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

<span data-ttu-id="41261-144">Dies [Beispiel](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) konfiguriert ist, unter Verwendung des gleichen `NowinServer` wie das vorherige - ist der einzige Unterschied in der Konfiguration der Anwendung in seiner `Configure` Methode.</span><span class="sxs-lookup"><span data-stu-id="41261-144">This [sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) is configured using the same `NowinServer` as the previous one - the only difference is in how the application is configured in its `Configure` method.</span></span> <span data-ttu-id="41261-145">Einen Test mit [einen einfachen Websocket-Client](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) Anwendung veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="41261-145">A test using [a simple websocket client](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) demonstrates  the application:</span></span>

![Web-Socket-Testclient](owin/_static/websocket-test.png)

## <a name="owin-environment"></a><span data-ttu-id="41261-147">OWIN-Umgebung</span><span class="sxs-lookup"><span data-stu-id="41261-147">OWIN environment</span></span>

<span data-ttu-id="41261-148">Erstellen Sie eine OWIN-Umgebung verwenden die `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="41261-148">You can construct a OWIN environment using the `HttpContext`.</span></span>

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a><span data-ttu-id="41261-149">OWIN-Schlüssel</span><span class="sxs-lookup"><span data-stu-id="41261-149">OWIN keys</span></span>

<span data-ttu-id="41261-150">OWIN richtet sich nach einem `IDictionary<string,object>` Objekt zur Übermittlung von Informationen in der gesamten einen HTTP-Anforderung/Antwort-Austausch.</span><span class="sxs-lookup"><span data-stu-id="41261-150">OWIN depends on an `IDictionary<string,object>` object to communicate information throughout an HTTP Request/Response exchange.</span></span> <span data-ttu-id="41261-151">ASP.NET Core implementiert die unten aufgeführten Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="41261-151">ASP.NET Core implements the keys listed below.</span></span> <span data-ttu-id="41261-152">Finden Sie unter der [primären-Spezifikation, Erweiterungen](http://owin.org/#spec), und [owin-Schlüssel Richtlinien und allgemeine Schlüssel](http://owin.org/spec/spec/CommonKeys.html).</span><span class="sxs-lookup"><span data-stu-id="41261-152">See the [primary specification, extensions](http://owin.org/#spec), and [OWIN Key Guidelines and Common Keys](http://owin.org/spec/spec/CommonKeys.html).</span></span>

### <a name="request-data-owin-v100"></a><span data-ttu-id="41261-153">Anfordern von Daten (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="41261-153">Request Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="41261-154">Key</span><span class="sxs-lookup"><span data-stu-id="41261-154">Key</span></span>               | <span data-ttu-id="41261-155">Wert (Datentyp)</span><span class="sxs-lookup"><span data-stu-id="41261-155">Value (type)</span></span> | <span data-ttu-id="41261-156">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="41261-156">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="41261-157">Owin. RequestScheme</span><span class="sxs-lookup"><span data-stu-id="41261-157">owin.RequestScheme</span></span> | `String` |  |
| <span data-ttu-id="41261-158">Owin. RequestMethod</span><span class="sxs-lookup"><span data-stu-id="41261-158">owin.RequestMethod</span></span>  | `String` | |    
| <span data-ttu-id="41261-159">Owin. RequestPathBase</span><span class="sxs-lookup"><span data-stu-id="41261-159">owin.RequestPathBase</span></span>  | `String` | |    
| <span data-ttu-id="41261-160">Owin. RequestPath</span><span class="sxs-lookup"><span data-stu-id="41261-160">owin.RequestPath</span></span> | `String` | |     
| <span data-ttu-id="41261-161">Owin. RequestQueryString</span><span class="sxs-lookup"><span data-stu-id="41261-161">owin.RequestQueryString</span></span>  | `String` | |    
| <span data-ttu-id="41261-162">Owin. RequestProtocol</span><span class="sxs-lookup"><span data-stu-id="41261-162">owin.RequestProtocol</span></span>  | `String` | |    
| <span data-ttu-id="41261-163">Owin. RequestHeaders</span><span class="sxs-lookup"><span data-stu-id="41261-163">owin.RequestHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="41261-164">Owin. RequestBody</span><span class="sxs-lookup"><span data-stu-id="41261-164">owin.RequestBody</span></span> | `Stream`  | |

### <a name="request-data-owin-v110"></a><span data-ttu-id="41261-165">Anfordern von Daten (OWIN v1.1.0)</span><span class="sxs-lookup"><span data-stu-id="41261-165">Request Data (OWIN v1.1.0)</span></span>

| <span data-ttu-id="41261-166">Key</span><span class="sxs-lookup"><span data-stu-id="41261-166">Key</span></span>               | <span data-ttu-id="41261-167">Wert (Datentyp)</span><span class="sxs-lookup"><span data-stu-id="41261-167">Value (type)</span></span> | <span data-ttu-id="41261-168">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="41261-168">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="41261-169">Owin. RequestId</span><span class="sxs-lookup"><span data-stu-id="41261-169">owin.RequestId</span></span> | `String` | <span data-ttu-id="41261-170">Optional</span><span class="sxs-lookup"><span data-stu-id="41261-170">Optional</span></span> |

### <a name="response-data-owin-v100"></a><span data-ttu-id="41261-171">Antwortdaten (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="41261-171">Response Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="41261-172">Key</span><span class="sxs-lookup"><span data-stu-id="41261-172">Key</span></span>               | <span data-ttu-id="41261-173">Wert (Datentyp)</span><span class="sxs-lookup"><span data-stu-id="41261-173">Value (type)</span></span> | <span data-ttu-id="41261-174">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="41261-174">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="41261-175">Owin. ResponseStatusCode</span><span class="sxs-lookup"><span data-stu-id="41261-175">owin.ResponseStatusCode</span></span> | `int` | <span data-ttu-id="41261-176">Optional</span><span class="sxs-lookup"><span data-stu-id="41261-176">Optional</span></span> |
| <span data-ttu-id="41261-177">Owin. ResponseReasonPhrase</span><span class="sxs-lookup"><span data-stu-id="41261-177">owin.ResponseReasonPhrase</span></span> | `String` | <span data-ttu-id="41261-178">Optional</span><span class="sxs-lookup"><span data-stu-id="41261-178">Optional</span></span> |
| <span data-ttu-id="41261-179">Owin. ResponseHeaders</span><span class="sxs-lookup"><span data-stu-id="41261-179">owin.ResponseHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="41261-180">Owin. ResponseBody</span><span class="sxs-lookup"><span data-stu-id="41261-180">owin.ResponseBody</span></span> | `Stream`  | |


### <a name="other-data-owin-v100"></a><span data-ttu-id="41261-181">Andere Daten (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="41261-181">Other Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="41261-182">Key</span><span class="sxs-lookup"><span data-stu-id="41261-182">Key</span></span>               | <span data-ttu-id="41261-183">Wert (Datentyp)</span><span class="sxs-lookup"><span data-stu-id="41261-183">Value (type)</span></span> | <span data-ttu-id="41261-184">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="41261-184">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="41261-185">Owin. CallCancelled</span><span class="sxs-lookup"><span data-stu-id="41261-185">owin.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="41261-186">Owin. Version</span><span class="sxs-lookup"><span data-stu-id="41261-186">owin.Version</span></span>  | `String` | |   


### <a name="common-keys"></a><span data-ttu-id="41261-187">Allgemeine Schlüssel</span><span class="sxs-lookup"><span data-stu-id="41261-187">Common Keys</span></span>

| <span data-ttu-id="41261-188">Key</span><span class="sxs-lookup"><span data-stu-id="41261-188">Key</span></span>               | <span data-ttu-id="41261-189">Wert (Datentyp)</span><span class="sxs-lookup"><span data-stu-id="41261-189">Value (type)</span></span> | <span data-ttu-id="41261-190">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="41261-190">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="41261-191">SSL. ClientCertificate</span><span class="sxs-lookup"><span data-stu-id="41261-191">ssl.ClientCertificate</span></span> | `X509Certificate` |  |
| <span data-ttu-id="41261-192">SSL. LoadClientCertAsync</span><span class="sxs-lookup"><span data-stu-id="41261-192">ssl.LoadClientCertAsync</span></span>  | `Func<Task>` | |    
| <span data-ttu-id="41261-193">Server. RemoteIpAddress</span><span class="sxs-lookup"><span data-stu-id="41261-193">server.RemoteIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="41261-194">Server. Remoteanschluss</span><span class="sxs-lookup"><span data-stu-id="41261-194">server.RemotePort</span></span> | `String` | |     
| <span data-ttu-id="41261-195">Server. LocalIpAddress</span><span class="sxs-lookup"><span data-stu-id="41261-195">server.LocalIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="41261-196">Server. LocalPort</span><span class="sxs-lookup"><span data-stu-id="41261-196">server.LocalPort</span></span>  | `String` | |    
| <span data-ttu-id="41261-197">Server. IsLocal</span><span class="sxs-lookup"><span data-stu-id="41261-197">server.IsLocal</span></span>  | `bool` | |    
| <span data-ttu-id="41261-198">Server. OnSendingHeaders</span><span class="sxs-lookup"><span data-stu-id="41261-198">server.OnSendingHeaders</span></span>  | `Action<Action<object>,object>` | |


### <a name="sendfiles-v030"></a><span data-ttu-id="41261-199">SendFiles v0.3.0</span><span class="sxs-lookup"><span data-stu-id="41261-199">SendFiles v0.3.0</span></span>

| <span data-ttu-id="41261-200">Key</span><span class="sxs-lookup"><span data-stu-id="41261-200">Key</span></span>               | <span data-ttu-id="41261-201">Wert (Datentyp)</span><span class="sxs-lookup"><span data-stu-id="41261-201">Value (type)</span></span> | <span data-ttu-id="41261-202">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="41261-202">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="41261-203">SendFile bereitstellen. "SendAsync"</span><span class="sxs-lookup"><span data-stu-id="41261-203">sendfile.SendAsync</span></span> | <span data-ttu-id="41261-204">Finden Sie unter [Signatur des Delegaten](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="41261-204">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> | <span data-ttu-id="41261-205">Pro Anforderung</span><span class="sxs-lookup"><span data-stu-id="41261-205">Per Request</span></span> |


### <a name="opaque-v030"></a><span data-ttu-id="41261-206">Nicht transparente v0.3.0</span><span class="sxs-lookup"><span data-stu-id="41261-206">Opaque v0.3.0</span></span>

| <span data-ttu-id="41261-207">Key</span><span class="sxs-lookup"><span data-stu-id="41261-207">Key</span></span>               | <span data-ttu-id="41261-208">Wert (Datentyp)</span><span class="sxs-lookup"><span data-stu-id="41261-208">Value (type)</span></span> | <span data-ttu-id="41261-209">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="41261-209">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="41261-210">nicht transparent. Version</span><span class="sxs-lookup"><span data-stu-id="41261-210">opaque.Version</span></span> | `String` |  |
| <span data-ttu-id="41261-211">nicht transparent. Upgrade</span><span class="sxs-lookup"><span data-stu-id="41261-211">opaque.Upgrade</span></span> | `OpaqueUpgrade` | <span data-ttu-id="41261-212">Finden Sie unter [Signatur des Delegaten](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="41261-212">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="41261-213">nicht transparent. Stream</span><span class="sxs-lookup"><span data-stu-id="41261-213">opaque.Stream</span></span> | `Stream` |  |
| <span data-ttu-id="41261-214">nicht transparent. CallCancelled</span><span class="sxs-lookup"><span data-stu-id="41261-214">opaque.CallCancelled</span></span> | `CancellationToken` |  |


### <a name="websocket-v030"></a><span data-ttu-id="41261-215">WebSocket-v0.3.0</span><span class="sxs-lookup"><span data-stu-id="41261-215">WebSocket v0.3.0</span></span>

| <span data-ttu-id="41261-216">Key</span><span class="sxs-lookup"><span data-stu-id="41261-216">Key</span></span>               | <span data-ttu-id="41261-217">Wert (Datentyp)</span><span class="sxs-lookup"><span data-stu-id="41261-217">Value (type)</span></span> | <span data-ttu-id="41261-218">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="41261-218">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="41261-219">WebSocket. Version</span><span class="sxs-lookup"><span data-stu-id="41261-219">websocket.Version</span></span> | `String` |  |
| <span data-ttu-id="41261-220">WebSocket. Akzeptieren</span><span class="sxs-lookup"><span data-stu-id="41261-220">websocket.Accept</span></span> | `WebSocketAccept` | <span data-ttu-id="41261-221">Finden Sie unter [Signatur des Delegaten](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="41261-221">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="41261-222">WebSocket. AcceptAlt</span><span class="sxs-lookup"><span data-stu-id="41261-222">websocket.AcceptAlt</span></span> |  | <span data-ttu-id="41261-223">Nicht-Spezifikation</span><span class="sxs-lookup"><span data-stu-id="41261-223">Non-spec</span></span> |
| <span data-ttu-id="41261-224">WebSocket. Unterprotokoll</span><span class="sxs-lookup"><span data-stu-id="41261-224">websocket.SubProtocol</span></span> | `String` | <span data-ttu-id="41261-225">Finden Sie unter [RFC6455 Abschnitt 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) Schritt 5.5</span><span class="sxs-lookup"><span data-stu-id="41261-225">See [RFC6455 Section 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) Step 5.5</span></span> |
| <span data-ttu-id="41261-226">WebSocket. "SendAsync"</span><span class="sxs-lookup"><span data-stu-id="41261-226">websocket.SendAsync</span></span> | `WebSocketSendAsync` | <span data-ttu-id="41261-227">Finden Sie unter [Signatur des Delegaten](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="41261-227">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="41261-228">WebSocket. ReceiveAsync</span><span class="sxs-lookup"><span data-stu-id="41261-228">websocket.ReceiveAsync</span></span> | `WebSocketReceiveAsync` | <span data-ttu-id="41261-229">Finden Sie unter [Signatur des Delegaten](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="41261-229">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="41261-230">WebSocket. CloseAsync</span><span class="sxs-lookup"><span data-stu-id="41261-230">websocket.CloseAsync</span></span> | `WebSocketCloseAsync` | <span data-ttu-id="41261-231">Finden Sie unter [Signatur des Delegaten](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="41261-231">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="41261-232">WebSocket. CallCancelled</span><span class="sxs-lookup"><span data-stu-id="41261-232">websocket.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="41261-233">WebSocket. ClientCloseStatus</span><span class="sxs-lookup"><span data-stu-id="41261-233">websocket.ClientCloseStatus</span></span> | `int` | <span data-ttu-id="41261-234">Optional</span><span class="sxs-lookup"><span data-stu-id="41261-234">Optional</span></span> |
| <span data-ttu-id="41261-235">WebSocket. ClientCloseDescription</span><span class="sxs-lookup"><span data-stu-id="41261-235">websocket.ClientCloseDescription</span></span> | `String` | <span data-ttu-id="41261-236">Optional</span><span class="sxs-lookup"><span data-stu-id="41261-236">Optional</span></span> |


## <a name="additional-resources"></a><span data-ttu-id="41261-237">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="41261-237">Additional Resources</span></span>

* [<span data-ttu-id="41261-238">Middleware</span><span class="sxs-lookup"><span data-stu-id="41261-238">Middleware</span></span>](middleware.md)

* [<span data-ttu-id="41261-239">Server</span><span class="sxs-lookup"><span data-stu-id="41261-239">Servers</span></span>](servers/index.md)
