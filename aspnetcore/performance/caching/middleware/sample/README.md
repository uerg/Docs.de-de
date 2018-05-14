# <a name="aspnet-core-response-caching-sample"></a>ASP.NET Core Antwort Caching-Beispiel

Dieses Beispiel veranschaulicht die Verwendung von ASP.NET Core [Antwort zwischenspeichern Middleware](https://docs.microsoft.com/aspnet/core/performance/caching/middleware).

Die app antwortet, mit der Indexseite, einschließlich einer `Cache-Control` Header so konfigurieren Sie das Verhalten beim Zwischenspeichern. Setzt die app auch die `Vary` Header so konfigurieren Sie den Cache in der Antwort nur, wenn dienen der `Accept-Encoding` -Header der nachfolgenden Anforderungen, die aus der ursprünglichen Anforderung übereinstimmt.

Beim Ausführen des Beispiels wird die Indexseite bedient, aus dem Cache, wenn gespeichert und für bis zu 10 Sekunden zwischengespeichert.
