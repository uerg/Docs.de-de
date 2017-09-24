---
title: Antwort zwischenspeichern Middleware in ASP.NET Core
author: guardrex
description: Konfiguration und Verwendung der Antwort zwischenspeichern Middleware in ASP.NET Core-Anwendungen.
keywords: ASP.NET Core, Zwischenspeichern von Antworten, caching, ResponseCache, ResponseCaching, Cache-Control, VaryByQueryKeys, Middleware
ms.author: riande
manager: wpickett
ms.date: 08/22/2017
ms.topic: article
ms.assetid: f9267eab-2762-42ac-1638-4a25d2c9d67c
ms.prod: asp.net-core
uid: performance/caching/middleware
ms.openlocfilehash: 07626ae7f40dc6f704d69d71cb7f95d318e6f503
ms.sourcegitcommit: 8005eb4051e568d88ee58d48424f39916052e6e2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2017
---
# <a name="response-caching-middleware-in-aspnet-core"></a>Antwort zwischenspeichern Middleware in ASP.NET Core

Durch [Luke Latham](https://github.com/guardrex) und [John Luo](https://github.com/JunTaoLuo)

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/middleware/samples)

Dieses Dokument enthält ausführliche Informationen zum Konfigurieren der Antwort zwischenspeichern Middleware in ASP.NET Core-apps. Die Middleware wird bestimmt, wenn Antworten zwischengespeichert sind, speichert Antworten und fungiert Antworten aus dem Cache. Eine Einführung in die HTTP-caching und die `ResponseCache` -Attribut angegeben wird, finden Sie unter [Zwischenspeichern von Antworten](response.md).

## <a name="package"></a>Package
Um die Middleware in einem Projekt einzuschließen, fügen Sie einen Verweis auf die [ `Microsoft.AspNetCore.ResponseCaching` ](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) Verpacken, oder verwenden Sie die [ `Microsoft.AspNetCore.All` ](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) Paket.

## <a name="configuration"></a>Konfiguration
In `ConfigureServices`, die Auflistung die Middleware hinzugefügt.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](middleware/samples/2.x/Program.cs?name=snippet1&highlight=4)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](middleware/samples/1.x/Startup.cs?name=snippet1&highlight=3)]

---

Die app so konfigurieren, verwenden Sie die Middleware mit der `UseResponseCaching` Erweiterungsmethode, die die Middleware die Anforderungsverarbeitungspipeline hinzufügt. Die Beispiel-app Fügt eine [ `Cache-Control` ](https://tools.ietf.org/html/rfc7234#section-5.2) Header in die Antwort, die zwischengespeichert werden Antworten für bis zu 10 Sekunden zwischengespeichert. Das Beispiel sendet ein [ `Vary` ](https://tools.ietf.org/html/rfc7231#section-7.1.4) Header so konfigurieren Sie die Middleware zum Verarbeiten einer zwischengespeicherten Antwort nur, wenn die [ `Accept-Encoding` ](https://tools.ietf.org/html/rfc7231#section-5.3.4) -Header der nachfolgenden Anforderungen entspricht, die der ursprünglichen Anforderung.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](middleware/samples/2.x/Program.cs?name=snippet1&highlight=8)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](middleware/samples/1.x/Startup.cs?name=snippet2&highlight=3)]

---

Die Antwort zwischenspeichern Middleware werden lediglich die Reaktionen 200 (OK). Andere Antworten, einschließlich [Fehlerseiten](xref:fundamentals/error-handling), werden von der Middleware ignoriert.

> [!WARNING]
> Mit Inhalt für authentifizierte Clients Antworten müssen nicht zwischengespeichert werden kann, um zu verhindern, dass die Middleware aus speichern und diese Antworten bedient markiert werden. Finden Sie unter [Bedingungen für das caching](#conditions-for-caching) ausführliche Informationen wie die Middleware bestimmt, ob eine Antwort zwischengespeichert werden.

## <a name="options"></a>Optionen
Die Middleware bietet drei Optionen zum Steuern des Zwischenspeichern von Antworten.

| Option                | Standardwert |
| --------------------- | ------------- |
| UseCaseSensitivePaths | Bestimmt, ob die Groß-/Kleinschreibung Pfade Antworten zwischengespeichert werden.</p><p>Der Standardwert ist `false`. |
| MaximumBodySize       | Die größte zwischenspeicherbaren Größe des Antworttexts in Bytes.</p>Der Standardwert ist `64 * 1024 * 1024` (64 MB). |
| SizeLimit             | Die maximale Größe für die Antwort-Cache-Middleware in Bytes. Der Standardwert ist `100 * 1024 * 1024` (100 MB). |

Das folgende Beispiel konfiguriert die Middleware zum Zwischenspeichern von Antworten kleiner als oder gleich 1024 Bytes, die über die Groß-/Kleinschreibung Pfade, speichern die Antworten auf `/page1` und `/Page1` getrennt.

```csharp
services.AddResponseCaching(options =>
{
    options.UseCaseSensitivePaths = true;
    options.MaximumBodySize = 1024;
});
```

## <a name="varybyquerykeys"></a>VaryByQueryKeys
Bei Verwendung von MVC die `ResponseCache` Attribut gibt an, die Parameter zum Festlegen der entsprechenden Header für das Zwischenspeichern von Antworten erforderlich sind. Der einzige Parameter der der `ResponseCache` -Attribut, das die Middleware zwingend erforderlich ist `VaryByQueryKeys`, nicht dem tatsächlichen HTTP-Header entsprechen. Weitere Informationen finden Sie unter [ResponseCache Attribut](response.md#responsecache-attribute).

Wenn kein MVC, können Sie verändern, Zwischenspeichern von Antworten mit der `VaryByQueryKeys` Funktion. Verwenden der `ResponseCachingFeature` direkt aus der `IFeatureCollection` von der `HttpContext`:

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();
if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

## <a name="http-headers-used-by-response-caching-middleware"></a>HTTP-Header von Antwort zwischenspeichern Middleware verwendet
Antwort von der Middleware Zwischenspeichern wird über HTTP-Header konfiguriert. Die entsprechenden relevanten Header sind unten aufgeführt, mit Notizen auf deren caching Einfluss auf.

| Header | Details |
| ------ | ------- |
| Autorisierung | Die Antwort wird nicht zwischengespeichert, wenn der Header vorhanden ist. |
| Cache-Control | Die Middleware berücksichtigt nur Zwischenspeichern von Antworten mit markiert die `public` Cache-Anweisung. Sie können steuern, caching mit den folgenden Parametern:<ul><li>Max-age</li><li>Max-veraltet &#8224;</li><li>Min-frisch</li><li>Must-revalidate-Anweisung</li><li>ohne-cache</li><li>ohne-Speicher</li><li>nur-If-Cache</li><li>private</li><li>public</li><li>s-maxage</li><li>Proxy-Revalidate &#8225;</li></ul>&#8224; Wenn keine Beschränkung auf angegeben wird `max-stale`, die Middleware führt keine Aktion.<br>&#8225; `proxy-revalidate` hat dieselbe Wirkung wie das `must-revalidate`.<br><br>Weitere Informationen finden Sie unter [RFC 7231: Cache-Control-Request-Direktiven](https://tools.ietf.org/html/rfc7234#section-5.2.1). |
| Pragma | Ein `Pragma: no-cache` Header in der Anforderung erzeugt dieselbe Wirkung wie das `Cache-Control: no-cache`. Dieser Header wird überschrieben, indem die relevanten Direktiven in der `Cache-Control` -Header, falls vorhanden. Für die Abwärtskompatibilität mit HTTP/1.0 berücksichtigt. |
| Set-Cookie | Die Antwort wird nicht zwischengespeichert, wenn der Header vorhanden ist. |
| variieren | Die `Vary` Header variiert die zwischengespeicherte Antwort von einer anderen Spaltenüberschrift verwendet wird. Sie können z. B. Antworten zwischenspeichern, codieren Sie dazu die `Vary: Accept-Encoding` -Header, der Antworten für Anforderungen mit Headern zwischenspeichert `Accept-Encoding: gzip` und `Accept-Encoding: text/plain` getrennt. Eine Antwort mit dem Headerwert `*` wird nie gespeichert. |
| Läuft ab | Eine veraltete von dieser Header als Antwort wird nicht gespeichert oder abgerufen werden, es sei denn, die von anderen überschreiben `Cache-Control` Header. |
| If-None-Match | Die vollständige Antwort wird aus dem Cache bedient, wenn der Wert ist keine `*` und die `ETag` der Antwort entspricht nicht der Werte bereitgestellt. Andernfalls wird eine 304 (nicht geändert) Antwort verarbeitet. |
| If-Modified-Since | Wenn die `If-None-Match` Header nicht vorhanden ist, wird eine vollständige Antwort aus dem Cache bedient, wenn die zwischengespeicherte Antwortdatum neuer als der angegebene Wert ist. Andernfalls wird eine 304 (nicht geändert) Antwort verarbeitet. |
| Datum | Wenn aus dem Cache bedient die `Date` Header von der Middleware festgelegt ist, wenn diese Option wurde nicht in der ursprünglichen Antwort angegeben. |
| Content-Length | Wenn aus dem Cache bedient die `Content-Length` Header von der Middleware festgelegt ist, wenn diese Option wurde nicht in der ursprünglichen Antwort angegeben. |
| ALTER | Die `Age` Header in der ursprünglichen Antwort gesendet wird ignoriert. Die Middleware berechnet einen neuen Wert an, wenn eine zwischengespeicherte Antwort bedient. |

## <a name="troubleshooting"></a>Problembehandlung
Das Verhalten beim Zwischenspeichern ist nicht wie erwartet, stellen Sie sicher, dass Antworten zwischengespeichert werden und kann durch Prüfen der eingehenden Anforderungsheader und ausgehenden Header der Antwort aus dem Cache bedient werden. Aktivieren der [Protokollierung](xref:fundamentals/logging) hilft beim Debuggen. Die Middleware-Protokolle, die caching-Verhalten und wenn eine Antwort aus dem Cache abgerufen wird.

Beim Testen und Problembehandlung für das Verhalten beim Zwischenspeichern, möglicherweise einen Browser Anforderungsheader festzulegen, die caching auf unerwünschten Weise beeinflussen. Beispielsweise kann ein Browser festgelegt die `Cache-Control` Header `no-cache` Wenn Sie die Seite aktualisieren. Die folgenden Tools können explizit Anforderungsheader festlegen und Testen des Zwischenspeicherns, als bevorzugt eingestuft werden:

* [Fiddler](http://www.telerik.com/fiddler)
* [Firebug](http://getfirebug.com/)
* [Postman](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a>Bedingungen für das caching
* Die Anforderung muss eine 200 (OK)-Antwort vom Server führen.
* Die Anforderungsmethode muss GET oder HEAD.
* Terminaldienste-Middleware, z. B. Middleware für statische Dateien, muss die Antwort vor der Antwort zwischenspeichern Middleware nicht verarbeitet werden.
* Die `Authorization` Header darf nicht vorhanden sein.
* `Cache-Control`Headerparameter müssen gültig sein, und die Antwort muss markiert sein `public` und nicht gekennzeichnet `private`.
* Die `Pragma: no-cache` Header-Wert darf nicht vorhanden sein wenn die `Cache-Control` Header nicht vorhanden ist, als die `Cache-Control` Header überschreibt die `Pragma` Header, wenn vorhanden.
* Die `Set-Cookie` Header darf nicht vorhanden sein.
* `Vary`Headerparameter müssen gültig und nicht gleich sein `*`.
* Die `Content-Length` Headerwert (falls festgelegt) müssen die Größe des Antworttexts übereinstimmen.
* Die [IHttpSendFileFeature](/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpsendfilefeature) wird nicht verwendet.
* Die Antwort muss entsprechend den Angaben von veralteten der `Expires` Header und die `max-age` und `s-maxage` cache-Anweisungen.
* Antwortpufferung ist erfolgreich, und die Größe der Antwort ist kleiner als die konfigurierte oder default `SizeLimit`.
* Muss die Antwort zwischengespeichert werden gemäß der [RFC 7234](https://tools.ietf.org/html/rfc7234) Spezifikationen. Z. B. die `no-store` Richtlinie darf nicht in der Anforderung oder Antwort Headerfelder vorhanden sein. Finden Sie unter *Abschnitt 3: Speichern von Antworten in Caches* von [RFC 7234](https://tools.ietf.org/html/rfc7234) Details.

> [!NOTE]
> Das Antiforgery System zum Generieren von sicheren Token, um zu verhindern, dass Cross-Site Request Fälschung Websiteübergreifender Angriffen legt die `Cache-Control` und `Pragma` Headern zum `no-cache` so, dass Antworten zwischengespeichert werden nicht.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Application Startup (Starten von Anwendungen)](xref:fundamentals/startup)
* [Middleware](xref:fundamentals/middleware)
