---
title: Zwischenspeichern von Antworten
author: rick-anderson
description: "Erklärt, wie Antwort zwischenspeichern konfigurieren, um Bandbreite zu senken und die Leistung verbessern."
keywords: ASP.NET Core, Antwort zwischenspeichern, HTTP-Header
ms.author: riande
manager: wpickett
ms.date: 07/10/2017
ms.topic: article
ms.assetid: cb42035a-60b0-472e-a614-cb79f443f654
ms.prod: asp.net-core
uid: performance/caching/response
ms.openlocfilehash: af114401d2f6f183291caba3c015359afb737d93
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2017
---
# <a name="response-caching"></a><span data-ttu-id="4fa94-104">Zwischenspeichern von Antworten</span><span class="sxs-lookup"><span data-stu-id="4fa94-104">Response Caching</span></span>

<span data-ttu-id="4fa94-105">Durch [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), und [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="4fa94-105">By [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Steve Smith](https://ardalis.com/)</span></span>

[<span data-ttu-id="4fa94-106">Anzeigen oder Herunterladen von Beispielcode</span><span class="sxs-lookup"><span data-stu-id="4fa94-106">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/sample)

## <a name="what-is-response-caching"></a><span data-ttu-id="4fa94-107">Was ist das Zwischenspeichern von Antworten</span><span class="sxs-lookup"><span data-stu-id="4fa94-107">What is Response Caching</span></span>

<span data-ttu-id="4fa94-108">*Zwischenspeichern von Antworten* Antworten Cache-bezogene Header hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="4fa94-108">*Response caching* adds cache-related headers to responses.</span></span> <span data-ttu-id="4fa94-109">Diese Header angeben, wie Clients, Proxy und Middleware zum Zwischenspeichern von Antworten soll.</span><span class="sxs-lookup"><span data-stu-id="4fa94-109">These headers specify how you want client, proxy and middleware to cache responses.</span></span> <span data-ttu-id="4fa94-110">Zwischenspeichern von Antworten reduzieren die Anzahl der Anforderungen, die ein Client oder Proxy auf dem Webserver vornimmt.</span><span class="sxs-lookup"><span data-stu-id="4fa94-110">Response caching can reduce the number of requests a client or proxy makes to the web server.</span></span> <span data-ttu-id="4fa94-111">Zwischenspeichern von Antworten kann außerdem verringern Sie die Menge der Arbeit der Webserver durchführt, um die Antwort zu generieren.</span><span class="sxs-lookup"><span data-stu-id="4fa94-111">Response caching can also reduce the amount of work the web server performs to generate the response.</span></span> 

<span data-ttu-id="4fa94-112">Ist der primäre HTTP-Header, die zum Zwischenspeichern verwendeten `Cache-Control`.</span><span class="sxs-lookup"><span data-stu-id="4fa94-112">The primary HTTP header used for caching is `Cache-Control`.</span></span> <span data-ttu-id="4fa94-113">Finden Sie unter der [HTTP 1.1 Caching](https://tools.ietf.org/html/rfc7234#section-5.2) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="4fa94-113">See the [HTTP 1.1 Caching](https://tools.ietf.org/html/rfc7234#section-5.2) for more information.</span></span> <span data-ttu-id="4fa94-114">Allgemeine Cache-Anweisungen:</span><span class="sxs-lookup"><span data-stu-id="4fa94-114">Common cache directives:</span></span>

* [<span data-ttu-id="4fa94-115">public</span><span class="sxs-lookup"><span data-stu-id="4fa94-115">public</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)
* [<span data-ttu-id="4fa94-116">private</span><span class="sxs-lookup"><span data-stu-id="4fa94-116">private</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)
* [<span data-ttu-id="4fa94-117">ohne-cache</span><span class="sxs-lookup"><span data-stu-id="4fa94-117">no-cache</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.4)
* [<span data-ttu-id="4fa94-118">Pragma</span><span class="sxs-lookup"><span data-stu-id="4fa94-118">Pragma</span></span>](https://tools.ietf.org/html/rfc7234#section-5.4)
* [<span data-ttu-id="4fa94-119">Variieren</span><span class="sxs-lookup"><span data-stu-id="4fa94-119">Vary</span></span>](https://tools.ietf.org/html/rfc7231#section-7.1.4)

<span data-ttu-id="4fa94-120">Die Webserver kann Antworten durch Hinzufügen der Antwort zwischenspeichern Middleware Zwischenspeichern.</span><span class="sxs-lookup"><span data-stu-id="4fa94-120">The web server can cache responses by adding the response caching middleware.</span></span> <span data-ttu-id="4fa94-121">Finden Sie unter [Antwort zwischenspeichern Middleware](middleware.md) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="4fa94-121">See [Response caching middleware](middleware.md) for more information.</span></span>

## <a name="distributed-cache-tag-helper"></a><span data-ttu-id="4fa94-122">Verteilter Cache-Tag-Hilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="4fa94-122">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="4fa94-123">Die [verteilten Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/DistributedCacheTagHelper) ermöglicht Verteiltes Zwischenspeichern.</span><span class="sxs-lookup"><span data-stu-id="4fa94-123">The [Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/DistributedCacheTagHelper) enables distributed caching.</span></span>


## <a name="responsecache-attribute"></a><span data-ttu-id="4fa94-124">ResponseCache-Attribut</span><span class="sxs-lookup"><span data-stu-id="4fa94-124">ResponseCache Attribute</span></span>

<span data-ttu-id="4fa94-125">Die `ResponseCacheAttribute` gibt die Parameter zum Festlegen der entsprechenden Header in Zwischenspeichern von Antworten erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="4fa94-125">The `ResponseCacheAttribute` specifies the parameters necessary for setting appropriate headers in response caching.</span></span> <span data-ttu-id="4fa94-126">Finden Sie unter [ResponseCacheAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.responsecacheattribute) eine Beschreibung der Parameter.</span><span class="sxs-lookup"><span data-stu-id="4fa94-126">See [ResponseCacheAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.responsecacheattribute)  for a description of the parameters.</span></span>

>[!WARNING]
> <span data-ttu-id="4fa94-127">Deaktivieren Sie das Zwischenspeichern für Inhalte, die Informationen für authentifizierte Clients enthält.</span><span class="sxs-lookup"><span data-stu-id="4fa94-127">Disable caching for content that contains information for authenticated clients.</span></span> <span data-ttu-id="4fa94-128">Zwischenspeichern sollten nur aktiviert werden, für Inhalte, die nicht geändert wird auf der Identität eines Benutzers basieren, oder ob ein Benutzer angemeldet ist.</span><span class="sxs-lookup"><span data-stu-id="4fa94-128">Caching should only be enabled for content that does not change based on a user's identity, or whether a user is logged in.</span></span>

<span data-ttu-id="4fa94-129">`VaryByQueryKeys string[]`(erfordert ASP.NET Core 1.1.0 und höher): Wenn festgelegt ist, die Antwort zwischenspeichern Middleware gespeicherte Antwort von den Werten der angegebenen Liste der Abfrageschlüssel variiert.</span><span class="sxs-lookup"><span data-stu-id="4fa94-129">`VaryByQueryKeys string[]` (requires ASP.NET Core 1.1.0 and higher): When set, the response caching middleware will vary the stored response by the values of the given list of query keys.</span></span> <span data-ttu-id="4fa94-130">Die Antwort zwischenspeichern Middleware muss aktiviert sein, zum Festlegen der `VaryByQueryKeys` -Eigenschaft, andernfalls eine Laufzeitausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="4fa94-130">The response caching middleware must be enabled to set the `VaryByQueryKeys` property, otherwise a runtime exception will be thrown.</span></span> <span data-ttu-id="4fa94-131">Es ist keine entsprechende HTTP-Header für die `VaryByQueryKeys` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="4fa94-131">There is no corresponding HTTP header for the `VaryByQueryKeys` property.</span></span> <span data-ttu-id="4fa94-132">Diese Eigenschaft ist eine HTTP-Funktion, die von der Antwort zwischenspeichern Middleware verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="4fa94-132">This property is an HTTP feature handled by the response caching middleware.</span></span> <span data-ttu-id="4fa94-133">Für die Middleware, die eine zwischengespeicherte Antwort dient müssen der Abfragezeichenfolge und der Wert der Abfragezeichenfolge eine frühere Anforderung wieder übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="4fa94-133">For the middleware to serve a cached response, the query string and query string value must match a previous request.</span></span> <span data-ttu-id="4fa94-134">Betrachten Sie beispielsweise die folgende Sequenz:</span><span class="sxs-lookup"><span data-stu-id="4fa94-134">For example, consider the following sequence:</span></span>

| <span data-ttu-id="4fa94-135">Anforderung</span><span class="sxs-lookup"><span data-stu-id="4fa94-135">Request</span></span>          | <span data-ttu-id="4fa94-136">Ergebnis</span><span class="sxs-lookup"><span data-stu-id="4fa94-136">Result</span></span> |
| ----------------- | ------------ | 
| `http://example.com?key1=value1` | <span data-ttu-id="4fa94-137">vom Server zurückgegebene</span><span class="sxs-lookup"><span data-stu-id="4fa94-137">returned from server</span></span> |
| `http://example.com?key1=value1` | <span data-ttu-id="4fa94-138">von der Middleware zurückgegeben</span><span class="sxs-lookup"><span data-stu-id="4fa94-138">returned from middleware</span></span> |
| `http://example.com?key1=value2` | <span data-ttu-id="4fa94-139">vom Server zurückgegebene</span><span class="sxs-lookup"><span data-stu-id="4fa94-139">returned from server</span></span> |

<span data-ttu-id="4fa94-140">Die erste Anforderung wird vom Server zurückgegebenen und Middleware zwischengespeichert.</span><span class="sxs-lookup"><span data-stu-id="4fa94-140">The first request is returned by the server and cached in middleware.</span></span> <span data-ttu-id="4fa94-141">Die zweite Anforderung wird von Middleware zurückgegeben, weil die Abfragezeichenfolge die vorhergehenden Anforderung übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="4fa94-141">The second request is returned by middleware because the query string matches the previous request.</span></span> <span data-ttu-id="4fa94-142">Die dritte Anforderung ist nicht im Cache Middleware, da der Wert der Abfragezeichenfolge nicht mit eine frühere Anforderung übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="4fa94-142">The third request is not in the middleware cache because the query string value doesn't match a previous request.</span></span> 

<span data-ttu-id="4fa94-143">Die `ResponseCacheAttribute` dient zum Erstellen und konfigurieren Sie (über `IFilterFactory`) eine `ResponseCacheFilter`.</span><span class="sxs-lookup"><span data-stu-id="4fa94-143">The `ResponseCacheAttribute` is used to configure and create (via `IFilterFactory`) a `ResponseCacheFilter`.</span></span> <span data-ttu-id="4fa94-144">Die `ResponseCacheFilter` führt die Arbeit aktualisieren die entsprechenden HTTP-Header und die Funktionen der Antwort.</span><span class="sxs-lookup"><span data-stu-id="4fa94-144">The `ResponseCacheFilter` performs the work of updating the appropriate HTTP headers and features of the response.</span></span> <span data-ttu-id="4fa94-145">Der Filter:</span><span class="sxs-lookup"><span data-stu-id="4fa94-145">The filter:</span></span>

* <span data-ttu-id="4fa94-146">Entfernt alle vorhandenen Header für `Vary`, `Cache-Control`, und `Pragma`.</span><span class="sxs-lookup"><span data-stu-id="4fa94-146">Removes any existing headers for `Vary`, `Cache-Control`, and `Pragma`.</span></span> 
* <span data-ttu-id="4fa94-147">Schreibt die entsprechenden Header auf Grundlage der Eigenschaften legen Sie in der `ResponseCacheAttribute`.</span><span class="sxs-lookup"><span data-stu-id="4fa94-147">Writes out the appropriate headers based on the properties set in the `ResponseCacheAttribute`.</span></span> 
* <span data-ttu-id="4fa94-148">Aktualisiert die Antwort zwischenspeichern HTTP-Funktion, wenn `VaryByQueryKeys` festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="4fa94-148">Updates the response caching HTTP feature if `VaryByQueryKeys` is set.</span></span>

### <a name="vary"></a><span data-ttu-id="4fa94-149">variieren</span><span class="sxs-lookup"><span data-stu-id="4fa94-149">Vary</span></span>

<span data-ttu-id="4fa94-150">Dieser Header wird nur geschrieben, wenn die `VaryByHeader` festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="4fa94-150">This header is only written when the `VaryByHeader` property is set.</span></span> <span data-ttu-id="4fa94-151">Es wird festgelegt, um die `Vary` den Wert der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="4fa94-151">It is set to the `Vary` property's value.</span></span> <span data-ttu-id="4fa94-152">Das folgende Beispiel verwendet die `VaryByHeader` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="4fa94-152">The following sample uses the `VaryByHeader` property.</span></span>

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

<span data-ttu-id="4fa94-153">Sie können die Antwortheader mit Ihrem Browser Netzwerktools anzeigen.</span><span class="sxs-lookup"><span data-stu-id="4fa94-153">You can view the response headers with your browsers network tools.</span></span> <span data-ttu-id="4fa94-154">Die folgende Abbildung zeigt die Ausgabe auf Edge F12 der **Netzwerk** Registerkarte, wenn die `About2` Aktionsmethode wird aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="4fa94-154">The following image shows the Edge F12 output on the **Network** tab when the `About2` action method is refreshed.</span></span> 

![Edge-F12-Ausgabe auf die ** Netzwerk ** Registerkarte beim Aufrufen der Aktionsmethode "About2"](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a><span data-ttu-id="4fa94-156">NoStore und Location.None</span><span class="sxs-lookup"><span data-stu-id="4fa94-156">NoStore and Location.None</span></span>

<span data-ttu-id="4fa94-157">`NoStore`überschreibt die meisten anderen Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="4fa94-157">`NoStore` overrides most of the other properties.</span></span> <span data-ttu-id="4fa94-158">Wenn diese Eigenschaft festgelegt wird, um `true`die `Cache-Control` Header auf "Nein-Store" festgelegt.</span><span class="sxs-lookup"><span data-stu-id="4fa94-158">When this property is set to `true`, the `Cache-Control` header will be set to "no-store".</span></span> <span data-ttu-id="4fa94-159">Wenn `Location` festgelegt ist, um `None`:</span><span class="sxs-lookup"><span data-stu-id="4fa94-159">If `Location` is set to `None`:</span></span>

* <span data-ttu-id="4fa94-160">Für `Cache-Control` ist `"no-store, no-cache"` festgelegt.</span><span class="sxs-lookup"><span data-stu-id="4fa94-160">`Cache-Control` is set to `"no-store, no-cache"`.</span></span> 
* <span data-ttu-id="4fa94-161">Für `Pragma` ist `no-cache` festgelegt.</span><span class="sxs-lookup"><span data-stu-id="4fa94-161">`Pragma` is set to `no-cache`.</span></span> 

<span data-ttu-id="4fa94-162">Wenn `NoStore` ist `false` und `Location` ist `None`, `Cache-Control` und `Pragma` festgelegt, um `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="4fa94-162">If `NoStore` is `false` and `Location` is `None`,  `Cache-Control` and `Pragma` will be set to `no-cache`.</span></span>

<span data-ttu-id="4fa94-163">Legen Sie Sie in der Regel `NoStore` auf `true` auf Fehlerseiten.</span><span class="sxs-lookup"><span data-stu-id="4fa94-163">You typically set `NoStore` to `true` on error pages.</span></span> <span data-ttu-id="4fa94-164">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="4fa94-164">For example:</span></span>

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

<span data-ttu-id="4fa94-165">Dies führt die folgenden Header:</span><span class="sxs-lookup"><span data-stu-id="4fa94-165">This will result in the following headers:</span></span>

```javascript
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a><span data-ttu-id="4fa94-166">Speicherort und die Dauer</span><span class="sxs-lookup"><span data-stu-id="4fa94-166">Location and Duration</span></span>

<span data-ttu-id="4fa94-167">So aktivieren Sie das Zwischenspeichern, `Duration` muss auf einen positiven Wert festgelegt werden und `Location` muss entweder `Any` (Standard) oder `Client`.</span><span class="sxs-lookup"><span data-stu-id="4fa94-167">To enable caching, `Duration` must be set to a positive value and `Location` must be either `Any` (the default) or `Client`.</span></span> <span data-ttu-id="4fa94-168">In diesem Fall die `Cache-Control` Header wird auf die Location-Wert, gefolgt von der "Max-Age" der Antwort festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="4fa94-168">In this case, the `Cache-Control` header will be set to the location value followed by the "max-age" of the response.</span></span>

> [!NOTE]
> <span data-ttu-id="4fa94-169">`Location`die Optionen der `Any` und `Client` übersetzen in `Cache-Control` Headerwerte `public` und `private`bzw..</span><span class="sxs-lookup"><span data-stu-id="4fa94-169">`Location`'s options of `Any` and `Client` translate into `Cache-Control` header values of `public` and `private`, respectively.</span></span> <span data-ttu-id="4fa94-170">Wie bereits erwähnt, festlegen `Location` auf `None` wird, setzen Sie beide `Cache-Control` und `Pragma` Header `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="4fa94-170">As noted previously, setting `Location` to `None` will set both `Cache-Control` and `Pragma` headers to `no-cache`.</span></span>

<span data-ttu-id="4fa94-171">Im folgenden ein Beispiel für die Header erstellt wird, durch Festlegen von `Duration` und lassen die Standardeinstellung `Location` Wert.</span><span class="sxs-lookup"><span data-stu-id="4fa94-171">Below is an example showing the headers produced by setting `Duration` and leaving the default `Location` value.</span></span>

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

<span data-ttu-id="4fa94-172">Erzeugt die folgenden Header:</span><span class="sxs-lookup"><span data-stu-id="4fa94-172">Produces the following headers:</span></span>

```javascript
Cache-Control: public,max-age=60
   ```

### <a name="cache-profiles"></a><span data-ttu-id="4fa94-173">Cacheprofile</span><span class="sxs-lookup"><span data-stu-id="4fa94-173">Cache Profiles</span></span>

<span data-ttu-id="4fa94-174">Anstelle von duplizieren `ResponseCache` Einstellungen auf viele Aktion Attributen für Controller, CacheProfile können als Optionen konfiguriert werden, beim Einrichten von MVC in der `ConfigureServices` Methode in `Startup`.</span><span class="sxs-lookup"><span data-stu-id="4fa94-174">Instead of duplicating `ResponseCache` settings on many controller action attributes, cache profiles can be configured as options when setting up MVC in the `ConfigureServices` method in `Startup`.</span></span> <span data-ttu-id="4fa94-175">In ein Cacheprofil verwiesen wird, gefundenen Werte verwendet werden als die standardmäßig von der `ResponseCache` Attribut, und wird nach beliebigen Eigenschaften für das Attribut angegebenen überschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="4fa94-175">Values found in a referenced cache profile will be used as the defaults by the `ResponseCache` attribute, and will be overridden by any properties specified on the attribute.</span></span>

<span data-ttu-id="4fa94-176">Einrichten von ein Cacheprofil:</span><span class="sxs-lookup"><span data-stu-id="4fa94-176">Setting up a cache profile:</span></span>

[!code-csharp[Main](response/sample/Startup.cs?name=snippet1)] 

<span data-ttu-id="4fa94-177">Verweisen auf ein Cacheprofil aus:</span><span class="sxs-lookup"><span data-stu-id="4fa94-177">Referencing a cache profile:</span></span>

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

<span data-ttu-id="4fa94-178">Die `ResponseCache` Attribut kann sowohl auf Aktionen (Methoden) sowie auf Domänencontrollern (Klassen) angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="4fa94-178">The `ResponseCache` attribute can be applied both to actions (methods) as well as controllers (classes).</span></span> <span data-ttu-id="4fa94-179">Methodenebene Attribute werden die Attribute auf Klassenebene festgelegten Einstellungen überschrieben.</span><span class="sxs-lookup"><span data-stu-id="4fa94-179">Method-level attributes will override the settings specified in class-level attributes.</span></span>

<span data-ttu-id="4fa94-180">Im obigen Beispiel gibt ein Attribut auf Klassenebene eine Dauer von 30 Sekunden, während eine Methodenebene Attribute verweist auf ein Cacheprofil mit einer Dauer von 60 Sekunden festgelegt.</span><span class="sxs-lookup"><span data-stu-id="4fa94-180">In the above example, a class-level attribute specifies a duration of 30 seconds while a method-level attributes references a cache profile with a duration set to 60 seconds.</span></span>

<span data-ttu-id="4fa94-181">Die resultierende Header:</span><span class="sxs-lookup"><span data-stu-id="4fa94-181">The resulting header:</span></span>

```
Cache-Control: public,max-age=60
   ```

  ### <a name="additional-resources"></a><span data-ttu-id="4fa94-182">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="4fa94-182">Additional Resources</span></span>

* [<span data-ttu-id="4fa94-183">Caching in HTTP aus der Spezifikation</span><span class="sxs-lookup"><span data-stu-id="4fa94-183">Caching in HTTP from the specification</span></span>](https://tools.ietf.org/html/rfc7234#section-3)
* [<span data-ttu-id="4fa94-184">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="4fa94-184">Cache-Control</span></span>](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
