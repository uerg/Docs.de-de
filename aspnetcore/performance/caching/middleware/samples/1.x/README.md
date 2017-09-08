# <a name="aspnet-core-response-caching-sample-aspnet-core-1x"></a>ASP.NET Core-Antwort, die Caching-Beispiel (ASP.NET Core 1.x)

Dieses Beispiel veranschaulicht die Verwendung von ASP.NET Core [Antwort zwischenspeichern Middleware](xref:performance/caching/middleware) mit ASP.NET Core 1.x. Das ASP.NET Core 2.x-Beispiel finden Sie unter [ASP.NET Core Antwort Caching-Beispiel (ASP.NET Core 2.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/middleware/samples/2.x).

Die Anwendung sendet eine `Hello World!` Nachricht und die aktuelle Zeit zusammen mit einem `Cache-Control` Header so konfigurieren Sie das Verhalten beim Zwischenspeichern. Sendet die Anwendung außerdem eine `Vary` Header so konfigurieren Sie den Cache in der Antwort nur, wenn dienen der `Accept-Encoding` -Header der nachfolgenden Anforderungen, die aus der ursprünglichen Anforderung übereinstimmt.

Beim Ausführen des Beispiels wird eine Antwort aus dem Cache, wenn möglich und gespeicherte für bis zu 10 Sekunden verarbeitet.
