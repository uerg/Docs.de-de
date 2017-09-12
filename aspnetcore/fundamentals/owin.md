---
title: "Offene Webschnittstelle für .NET (OWIN)"
author: ardalis
description: "Einführung in die Weboberfläche für .NET (OWIN) zu öffnen."
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
ms.openlocfilehash: 796d075d4d0c6b888be4fc2225fc482acdbad498
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2017
---
# <a name="introduction-to-open-web-interface-for-net-owin"></a>Einführung in die Weboberfläche für .NET (OWIN) zu öffnen.

Durch [Steve Smith](https://ardalis.com/) und [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core unterstützt die offene Webschnittstelle für .NET (OWIN). OWIN kann webapps auf Webservern entkoppelt werden. Definiert ein gängiges Verfahren für die Middleware, die in einer Pipeline zur Verarbeitung von Anforderungen und zugeordneten Antworten verwendet werden. ASP.NET Core-Anwendungen und Middleware können mit OWIN-basierten Anwendungen, Server und Middleware zusammenarbeiten.

OWIN stellt eine entkoppeln, die zwei Frameworks mit unterschiedlichen Objektmodelle zusammen verwendet werden können. Die `Microsoft.AspNetCore.Owin` Paket stellt zwei adapterimplementierungen bereit:
- ASP.NET Core zu OWIN 
- OWIN zu ASP.NET Core

Dadurch können ASP.NET Core gehostet werden, zusätzlich zu einem OWIN kompatiblen Server /-Host oder für andere OWIN kompatiblen Komponenten zusätzlich zu ASP.NET Core ausgeführt werden.

Hinweis: Mit diesen Adaptern mit Leistungseinbußen kommt. Anwendungen mit nur ASP.NET Core-Komponenten sollten nicht die Owin-Paket oder ein Adapter verwenden.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample)

## <a name="running-owin-middleware-in-the-aspnet-pipeline"></a>OWIN-Middleware in der ASP.NET-Pipeline ausgeführt wird.

ASP.NET Core des owin-Unterstützung bereitgestellt wird, als Teil der `Microsoft.AspNetCore.Owin` Paket. Sie können zur Installation dieses Pakets OWIN-Unterstützung in Ihr Projekt importieren.

OWIN-Middleware entspricht der [OWIN-Spezifikation](http://owin.org/spec/spec/owin-1.0.0.html), wofür ein `Func<IDictionary<string, object>, Task>` Schnittstelle und bestimmte Schlüssel festgelegt werden (z. B. `owin.ResponseBody`). Die folgenden einfachen owin-Middleware wird "Hello World" angezeigt:

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

Die Signatur Beispiel gibt eine `Task` und akzeptiert eine `IDictionary<string, object>` OWIN wie erforderlich.

Der folgende Code veranschaulicht das Hinzufügen der `OwinHello` Middleware (siehe oben), an der ASP.NET-Pipeline mit der `UseOwin` -Erweiterungsmethode.

<!-- literal_block {"ids": [], "names": [], "highlight_args": {"linenostart": 1}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#", "source": "/Users/shirhatti/src/Docs/aspnet/fundamentals/owin/sample/src/OwinSample/Startup.cs"} -->

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}


```

Sie können andere Aktionen, erfolgen in der OWIN-Pipeline konfigurieren.

> [!NOTE]
> Antwort-Header sollte nur geändert werden, vor dem ersten Schreiben in den Antwortstream.

> [!NOTE]
> Mehrere Aufrufe `UseOwin` wird abgeraten, zur Verbesserung der Leistung. Owin-Komponenten funktionieren am besten, wenn einer Gruppe zusammengefasst.

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

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

<a name=hosting-on-owin></a>

## <a name="using-aspnet-hosting-on-an-owin-based-server"></a>Verwenden von ASP.NET-Hosting auf einen OWIN-basierten server

OWIN-basierte Server können ASP.NET-Anwendungen hosten. Ein solcher Server ist [Nowin](https://github.com/Bobris/Nowin), einen Webserver .NET OWIN. Im Beispiel für diesen Artikel haben ich enthalten ein Projekt, das Nowin verweist und verwendet er zum Erstellen einer `IServer` ASP.NET Core selbst hosten.

[!code-csharp[Main](owin/sample/src/NowinSample/Program.cs?highlight=15)]

`IServer`ist eine Schnittstelle, die erfordert eine `Features` Eigenschaft und eine `Start` Methode.

`Start`ist verantwortlich für das Konfigurieren und starten den Server, der in diesem Fall durch eine Reihe von fluent-API-Aufrufe abgeschlossen ist, die Adressen, die analysiert die IServerAddressesFeature festlegen. Beachten Sie, dass die fluent-Konfiguration von der `_builder` Variable gibt an, dass Anforderungen vom behandelt werden die `appFunc` weiter oben in der Methode definiert. Dies `Func` für jede Anforderung zur Verarbeitung von eingehenden Anforderungen aufgerufen wird.

Wir fügen auch eine `IWebHostBuilder` Erweiterung hinzufügen und Konfigurieren des Servers Nowin zu erleichtern.

<!-- literal_block {"ids": [], "names": [], "highlight_args": {"hl_lines": [11], "linenostart": 1}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#", "source": "/Users/shirhatti/src/Docs/aspnet/fundamentals/owin/sample/src/NowinSample/NowinWebHostBuilderExtensions.cs"} -->

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

Dabei vorhanden ist, ist erforderlich, führen Sie eine ASP.NET-Anwendung mit diesen benutzerdefinierten Server rufen Sie die Erweiterung in *"Program.cs"*:

<!-- literal_block {"ids": [], "names": [], "highlight_args": {"hl_lines": [15], "linenostart": 1}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#", "source": "/Users/shirhatti/src/Docs/aspnet/fundamentals/owin/sample/src/NowinSample/Program.cs"} -->

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

Erfahren Sie mehr über ASP.NET [Server](servers/index.md).

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a>Führen Sie ASP.NET Core auf einen OWIN-basierten Server und verwenden Sie die WebSockets-Unterstützung

Ein weiteres Beispiel für Funktionen wie OWIN-basierten Servern genutzt werden, indem Sie ASP.NET Core ist der Zugriff auf Funktionen wie WebSockets. Der im vorherigen Beispiel verwendete .NET OWIN-Webserver hat die Unterstützung für WebSockets integriert, die von einer ASP.NET Core-Anwendung genutzt werden können. Das folgende Beispiel zeigt eine einfache Web-app, die WebSockets unterstützt und wiederholt wieder alles, was über WebSockets an den Server gesendet.

<!-- literal_block {"ids": [], "names": [], "highlight_args": {"hl_lines": [7, 9, 10], "linenostart": 1}, "backrefs": [], "dupnames": [], "linenos": true, "classes": [], "xml:space": "preserve", "language": "c#", "source": "/Users/shirhatti/src/Docs/aspnet/fundamentals/owin/sample/src/NowinWebSockets/Startup.cs"} -->

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

Dies [Beispiel](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) konfiguriert ist, unter Verwendung des gleichen `NowinServer` wie das vorherige - ist der einzige Unterschied in der Konfiguration der Anwendung in seiner `Configure` Methode. Einen Test mit [einen einfachen Websocket-Client](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) Anwendung veranschaulicht:

![Web-Socket-Testclient](owin/_static/websocket-test.png)

## <a name="owin-environment"></a>OWIN-Umgebung

Erstellen Sie eine OWIN-Umgebung verwenden die `HttpContext`.

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a>OWIN-Schlüssel

OWIN richtet sich nach einem `IDictionary<string,object>` Objekt zur Übermittlung von Informationen in der gesamten einen HTTP-Anforderung/Antwort-Austausch. ASP.NET Core implementiert die unten aufgeführten Schlüssel. Finden Sie unter der [primären-Spezifikation, Erweiterungen](http://owin.org/#spec), und [owin-Schlüssel Richtlinien und allgemeine Schlüssel](http://owin.org/spec/spec/CommonKeys.html).

### <a name="request-data-owin-v100"></a>Anfordern von Daten (OWIN v1.0.0)

| Key               | Wert (Datentyp) | Beschreibung |
| ----------------- | ------------ | ----------- |
| Owin. RequestScheme | `String` |  |
| Owin. RequestMethod  | `String` | |    
| Owin. RequestPathBase  | `String` | |    
| Owin. RequestPath | `String` | |     
| Owin. RequestQueryString  | `String` | |    
| Owin. RequestProtocol  | `String` | |    
| Owin. RequestHeaders | `IDictionary<string,string[]>`  | |
| Owin. RequestBody | `Stream`  | |

### <a name="request-data-owin-v110"></a>Anfordern von Daten (OWIN v1.1.0)

| Key               | Wert (Datentyp) | Beschreibung |
| ----------------- | ------------ | ----------- |
| Owin. RequestId | `String` | Optional |

### <a name="response-data-owin-v100"></a>Antwortdaten (OWIN v1.0.0)

| Key               | Wert (Datentyp) | Beschreibung |
| ----------------- | ------------ | ----------- |
| Owin. ResponseStatusCode | `int` | Optional |
| Owin. ResponseReasonPhrase | `String` | Optional |
| Owin. ResponseHeaders | `IDictionary<string,string[]>`  | |
| Owin. ResponseBody | `Stream`  | |


### <a name="other-data-owin-v100"></a>Andere Daten (OWIN v1.0.0)

| Key               | Wert (Datentyp) | Beschreibung |
| ----------------- | ------------ | ----------- |
| Owin. CallCancelled | `CancellationToken` |  |
| Owin. Version  | `String` | |   


### <a name="common-keys"></a>Allgemeine Schlüssel

| Key               | Wert (Datentyp) | Beschreibung |
| ----------------- | ------------ | ----------- |
| SSL. ClientCertificate | `X509Certificate` |  |
| SSL. LoadClientCertAsync  | `Func<Task>` | |    
| Server. RemoteIpAddress  | `String` | |    
| Server. Remoteanschluss | `String` | |     
| Server. LocalIpAddress  | `String` | |    
| Server. LocalPort  | `String` | |    
| Server. IsLocal  | `bool` | |    
| Server. OnSendingHeaders  | `Action<Action<object>,object>` | |


### <a name="sendfiles-v030"></a>SendFiles v0.3.0

| Key               | Wert (Datentyp) | Beschreibung |
| ----------------- | ------------ | ----------- |
| SendFile bereitstellen. "SendAsync" | Finden Sie unter [Signatur des Delegaten](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) | Pro Anforderung |


### <a name="opaque-v030"></a>Nicht transparente v0.3.0

| Key               | Wert (Datentyp) | Beschreibung |
| ----------------- | ------------ | ----------- |
| nicht transparent. Version | `String` |  |
| nicht transparent. Upgrade | `OpaqueUpgrade` | Finden Sie unter [Signatur des Delegaten](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) |
| nicht transparent. Stream | `Stream` |  |
| nicht transparent. CallCancelled | `CancellationToken` |  |


### <a name="websocket-v030"></a>WebSocket-v0.3.0

| Key               | Wert (Datentyp) | Beschreibung |
| ----------------- | ------------ | ----------- |
| WebSocket. Version | `String` |  |
| WebSocket. Akzeptieren | `WebSocketAccept` | Finden Sie unter [Signatur des Delegaten](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) |
| WebSocket. AcceptAlt |  | Nicht-Spezifikation |
| WebSocket. Unterprotokoll | `String` | Finden Sie unter [RFC6455 Abschnitt 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) Schritt 5.5 |
| WebSocket. "SendAsync" | `WebSocketSendAsync` | Finden Sie unter [Signatur des Delegaten](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| WebSocket. ReceiveAsync | `WebSocketReceiveAsync` | Finden Sie unter [Signatur des Delegaten](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| WebSocket. CloseAsync | `WebSocketCloseAsync` | Finden Sie unter [Signatur des Delegaten](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| WebSocket. CallCancelled | `CancellationToken` |  |
| WebSocket. ClientCloseStatus | `int` | Optional |
| WebSocket. ClientCloseDescription | `String` | Optional |


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Middleware](middleware.md)

* [Server](servers/index.md)
