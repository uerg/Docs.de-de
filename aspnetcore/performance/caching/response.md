---
title: Zwischenspeichern von Antworten in ASP.NET Core
author: rick-anderson
description: Erfahren Sie, wie Sie die Zwischenspeicherung von Antworten verwenden können, um die Bandbreitenanforderungen zu senken und die Leistung von ASP.NET Core-Apps zu steigern.
ms.author: riande
ms.date: 09/20/2017
uid: performance/caching/response
ms.openlocfilehash: 99093cd281ffa8dddc574dc27254c0175e2651b3
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207367"
---
# <a name="response-caching-in-aspnet-core"></a><span data-ttu-id="037ab-103">Zwischenspeichern von Antworten in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="037ab-103">Response caching in ASP.NET Core</span></span>

<span data-ttu-id="037ab-104">Durch [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), und [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="037ab-104">By [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Luke Latham](https://github.com/guardrex)</span></span>

> [!NOTE]
> <span data-ttu-id="037ab-105">Zwischenspeichern von Antworten in Razor Pages ist in ASP.NET Core 2.1 oder höher verfügbar.</span><span class="sxs-lookup"><span data-stu-id="037ab-105">Response caching in Razor Pages is available in ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="037ab-106">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([Vorgehensweise zum Herunterladen](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="037ab-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="037ab-107">Zwischenspeichern von Antworten reduziert die Anzahl der Anforderungen, die ein Client oder Proxy an einen Webserver sendet.</span><span class="sxs-lookup"><span data-stu-id="037ab-107">Response caching reduces the number of requests a client or proxy makes to a web server.</span></span> <span data-ttu-id="037ab-108">Zwischenspeichern von Antworten auch verringert die Menge der Arbeit der Webserver ausgeführt werden, um eine Antwort zu generieren.</span><span class="sxs-lookup"><span data-stu-id="037ab-108">Response caching also reduces the amount of work the web server performs to generate a response.</span></span> <span data-ttu-id="037ab-109">Zwischenspeichern von Antworten wird durch Header gesteuert werden, die angeben, wie Sie Client und Proxy-Middleware zum Zwischenspeichern von Antworten soll.</span><span class="sxs-lookup"><span data-stu-id="037ab-109">Response caching is controlled by headers that specify how you want client, proxy, and middleware to cache responses.</span></span>

<span data-ttu-id="037ab-110">Der Webserver kann Antworten zwischenspeichern, wenn Sie hinzufügen [Antworten Zwischenspeichern Middleware](xref:performance/caching/middleware).</span><span class="sxs-lookup"><span data-stu-id="037ab-110">The web server can cache responses when you add [Response Caching Middleware](xref:performance/caching/middleware).</span></span>

## <a name="http-based-response-caching"></a><span data-ttu-id="037ab-111">Zwischenspeichern von Antworten HTTP-basierte</span><span class="sxs-lookup"><span data-stu-id="037ab-111">HTTP-based response caching</span></span>

<span data-ttu-id="037ab-112">Die [Zwischenspeichern von HTTP 1.1-Spezifikation](https://tools.ietf.org/html/rfc7234) beschrieben, wie internetcaches Verhalten soll.</span><span class="sxs-lookup"><span data-stu-id="037ab-112">The [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234) describes how Internet caches should behave.</span></span> <span data-ttu-id="037ab-113">Der primäre HTTP-Header für das caching verwendet [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), dient zum Angeben der Cache *Direktiven*.</span><span class="sxs-lookup"><span data-stu-id="037ab-113">The primary HTTP header used for caching is [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), which is used to specify cache *directives*.</span></span> <span data-ttu-id="037ab-114">Die Anweisungen Steuern des zwischenspeicherverhaltens Anforderungen ihren Weg von den Clients an Server senden und Antworten an Clients ihren Weg von Servern machen.</span><span class="sxs-lookup"><span data-stu-id="037ab-114">The directives control caching behavior as requests make their way from clients to servers and as reponses make their way from servers back to clients.</span></span> <span data-ttu-id="037ab-115">Verschieben von Anforderungen und Antworten über Proxyserver und webanwendungsproxy-Server müssen auch der Zwischenspeicherung von HTTP 1.1-Spezifikation entsprechen.</span><span class="sxs-lookup"><span data-stu-id="037ab-115">Requests and responses move through proxy servers, and proxy servers must also conform to the HTTP 1.1 Caching specification.</span></span>

<span data-ttu-id="037ab-116">Allgemeine `Cache-Control` Direktiven werden in der folgenden Tabelle angezeigt.</span><span class="sxs-lookup"><span data-stu-id="037ab-116">Common `Cache-Control` directives are shown in the following table.</span></span>

| <span data-ttu-id="037ab-117">Direktive</span><span class="sxs-lookup"><span data-stu-id="037ab-117">Directive</span></span>                                                       | <span data-ttu-id="037ab-118">Aktion</span><span class="sxs-lookup"><span data-stu-id="037ab-118">Action</span></span> |
| --------------------------------------------------------------- | ------ |
| [<span data-ttu-id="037ab-119">public</span><span class="sxs-lookup"><span data-stu-id="037ab-119">public</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | <span data-ttu-id="037ab-120">Ein Cache kann die Antwort speichern.</span><span class="sxs-lookup"><span data-stu-id="037ab-120">A cache may store the response.</span></span> |
| [<span data-ttu-id="037ab-121">private</span><span class="sxs-lookup"><span data-stu-id="037ab-121">private</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | <span data-ttu-id="037ab-122">Die Antwort muss nicht von einem freigegebenen Cache gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="037ab-122">The response must not be stored by a shared cache.</span></span> <span data-ttu-id="037ab-123">Ein privater Cache möglicherweise speichern und Wiederverwenden von der Antwort.</span><span class="sxs-lookup"><span data-stu-id="037ab-123">A private cache may store and reuse the response.</span></span> |
| [<span data-ttu-id="037ab-124">Max-age</span><span class="sxs-lookup"><span data-stu-id="037ab-124">max-age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | <span data-ttu-id="037ab-125">Der Client wird nicht akzeptiert, eine Antwort, deren Alter größer als die angegebene Anzahl von Sekunden ist.</span><span class="sxs-lookup"><span data-stu-id="037ab-125">The client won't accept a response whose age is greater than the specified number of seconds.</span></span> <span data-ttu-id="037ab-126">Beispiele: `max-age=60` (60 Sekunden), `max-age=2592000` (1 Monat)</span><span class="sxs-lookup"><span data-stu-id="037ab-126">Examples: `max-age=60` (60 seconds), `max-age=2592000` (1 month)</span></span> |
| [<span data-ttu-id="037ab-127">ohne-cache</span><span class="sxs-lookup"><span data-stu-id="037ab-127">no-cache</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | <span data-ttu-id="037ab-128">**Für Anforderungen**: ein Cache gespeicherte Antwort zum Erfüllen der Anforderung muss nicht verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="037ab-128">**On requests**: A cache must not use a stored response to satisfy the request.</span></span> <span data-ttu-id="037ab-129">Hinweis: Der Ursprungsserver die Antwort erneut für den Client generiert und die Middleware aktualisiert die gespeicherte Antwort im jeweiligen Cache.</span><span class="sxs-lookup"><span data-stu-id="037ab-129">Note: The origin server re-generates the response for the client, and the middleware updates the stored response in its cache.</span></span><br><br><span data-ttu-id="037ab-130">**Für Antworten**: die Antwort darf nicht für eine nachfolgende Anforderung ohne Überprüfung auf dem Ursprungsserver verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="037ab-130">**On responses**: The response must not be used for a subsequent request without validation on the origin server.</span></span> |
| [<span data-ttu-id="037ab-131">ohne-store</span><span class="sxs-lookup"><span data-stu-id="037ab-131">no-store</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | <span data-ttu-id="037ab-132">**Für Anforderungen**: ein Cache muss die Anforderung nicht speichern.</span><span class="sxs-lookup"><span data-stu-id="037ab-132">**On requests**: A cache must not store the request.</span></span><br><br><span data-ttu-id="037ab-133">**Für Antworten**: ein Cache muss einen beliebigen Teil der Antwort nicht speichern.</span><span class="sxs-lookup"><span data-stu-id="037ab-133">**On responses**: A cache must not store any part of the response.</span></span> |

<span data-ttu-id="037ab-134">Andere Cacheheader, die Zwischenspeichern eine Rolle spielen, werden in der folgenden Tabelle angezeigt.</span><span class="sxs-lookup"><span data-stu-id="037ab-134">Other cache headers that play a role in caching are shown in the following table.</span></span>

| <span data-ttu-id="037ab-135">Header</span><span class="sxs-lookup"><span data-stu-id="037ab-135">Header</span></span>                                                     | <span data-ttu-id="037ab-136">Funktion</span><span class="sxs-lookup"><span data-stu-id="037ab-136">Function</span></span> |
| ---------------------------------------------------------- | -------- |
| [<span data-ttu-id="037ab-137">ALTER</span><span class="sxs-lookup"><span data-stu-id="037ab-137">Age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.1)     | <span data-ttu-id="037ab-138">Eine Schätzung der die Zeitspanne in Sekunden seit der Antwort generiert oder erfolgreich auf dem Ursprungsserver überprüft wurde.</span><span class="sxs-lookup"><span data-stu-id="037ab-138">An estimate of the amount of time in seconds since the response was generated or successfully validated at the origin server.</span></span> |
| [<span data-ttu-id="037ab-139">Läuft ab</span><span class="sxs-lookup"><span data-stu-id="037ab-139">Expires</span></span>](https://tools.ietf.org/html/rfc7234#section-5.3) | <span data-ttu-id="037ab-140">Das Datum und Uhrzeit, nach dem die Antwort als veraltet angesehen wird.</span><span class="sxs-lookup"><span data-stu-id="037ab-140">The date/time after which the response is considered stale.</span></span> |
| [<span data-ttu-id="037ab-141">Pragma</span><span class="sxs-lookup"><span data-stu-id="037ab-141">Pragma</span></span>](https://tools.ietf.org/html/rfc7234#section-5.4)  | <span data-ttu-id="037ab-142">Vorhanden ist, für die Abwärtskompatibilität mit HTTP/1.0 für die Einstellung speichert `no-cache` Verhalten.</span><span class="sxs-lookup"><span data-stu-id="037ab-142">Exists for backwards compatibility with HTTP/1.0 caches for setting `no-cache` behavior.</span></span> <span data-ttu-id="037ab-143">Wenn die `Cache-Control` Header vorhanden ist, ist die `Pragma` Header wird ignoriert.</span><span class="sxs-lookup"><span data-stu-id="037ab-143">If the `Cache-Control` header is present, the `Pragma` header is ignored.</span></span> |
| [<span data-ttu-id="037ab-144">Variieren</span><span class="sxs-lookup"><span data-stu-id="037ab-144">Vary</span></span>](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | <span data-ttu-id="037ab-145">Gibt an, dass eine zwischengespeicherte Antwort nicht, wenn alle gesendet werden muss von der `Vary` Headerfelder in der zwischengespeicherten Antwort zur ursprünglichen Anforderung und die neue Anforderung zu entsprechen.</span><span class="sxs-lookup"><span data-stu-id="037ab-145">Specifies that a cached response must not be sent unless all of the `Vary` header fields match in both the cached response's original request and the new request.</span></span> |

## <a name="http-based-caching-respects-request-cache-control-directives"></a><span data-ttu-id="037ab-146">Zwischenspeichern Hinsicht HTTP-basierte Anforderung cachesteuerungsdirektiven</span><span class="sxs-lookup"><span data-stu-id="037ab-146">HTTP-based caching respects request Cache-Control directives</span></span>

<span data-ttu-id="037ab-147">Die [Zwischenspeichern von HTTP 1.1-Spezifikation für den Cache-Control-Header](https://tools.ietf.org/html/rfc7234#section-5.2) erfordert einen Cache, der einen gültigen berücksichtigt `Cache-Control` Header, die vom Client gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="037ab-147">The [HTTP 1.1 Caching specification for the Cache-Control header](https://tools.ietf.org/html/rfc7234#section-5.2) requires a cache to honor a valid `Cache-Control` header sent by the client.</span></span> <span data-ttu-id="037ab-148">Ein Client kann Anforderungen mit stellen eine `no-cache` Headerwert und erzwingen Sie den Server aus, um eine neue Antwort für jede Anforderung zu generieren.</span><span class="sxs-lookup"><span data-stu-id="037ab-148">A client can make requests with a `no-cache` header value and force the server to generate a new response for every request.</span></span>

<span data-ttu-id="037ab-149">Berücksichtigt immer Client `Cache-Control` Anforderungsheader ist sinnvoll, wenn Sie das Ziel der HTTP-Zwischenspeicherung in Betracht ziehen.</span><span class="sxs-lookup"><span data-stu-id="037ab-149">Always honoring client `Cache-Control` request headers makes sense if you consider the goal of HTTP caching.</span></span> <span data-ttu-id="037ab-150">In der offiziellen Spezifikation Zwischenspeicherung dient zum Reduzieren des Latenz und Aufwand der Erfüllung der Anforderungen in einem Netzwerk von Clients, Proxys und Servern.</span><span class="sxs-lookup"><span data-stu-id="037ab-150">Under the official specification, caching is meant to reduce the latency and network overhead of satisfying requests across a network of clients, proxies, and servers.</span></span> <span data-ttu-id="037ab-151">Es ist nicht unbedingt eine Möglichkeit zum Steuern der Last auf einem Ursprungsserver.</span><span class="sxs-lookup"><span data-stu-id="037ab-151">It isn't necessarily a way to control the load on an origin server.</span></span>

<span data-ttu-id="037ab-152">Gibt es keine aktuelle entwicklersteuerung dieses Verhalten beim Zwischenspeichern ist die Verwendung der [Antworten Zwischenspeichern Middleware](xref:performance/caching/middleware) , da die Middleware der offiziellen zwischenspeicherungsspezifikation entspricht.</span><span class="sxs-lookup"><span data-stu-id="037ab-152">There's no current developer control over this caching behavior when using the [Response Caching Middleware](xref:performance/caching/middleware) because the middleware adheres to the official caching specification.</span></span> <span data-ttu-id="037ab-153">[Zukünftige Verbesserungen an die Middleware](https://github.com/aspnet/ResponseCaching/issues/96) gestattet, konfigurieren die Middleware für die Verwendung einer Anforderung ignorieren `Cache-Control` Header, die bei der Entscheidung, um eine zwischengespeicherte Antwort zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="037ab-153">[Future enhancements to the middleware](https://github.com/aspnet/ResponseCaching/issues/96) will permit configuring the middleware to ignore a request's `Cache-Control` header when deciding to serve a cached response.</span></span> <span data-ttu-id="037ab-154">Dies wird Sie bieten eine Möglichkeit, eine bessere Steuerung der Last auf dem Server bei der Verwendung der Middleware.</span><span class="sxs-lookup"><span data-stu-id="037ab-154">This will offer you an opportunity to better control the load on your server when you use the middleware.</span></span>

## <a name="other-caching-technology-in-aspnet-core"></a><span data-ttu-id="037ab-155">Andere cachetechnologie in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="037ab-155">Other caching technology in ASP.NET Core</span></span>

### <a name="in-memory-caching"></a><span data-ttu-id="037ab-156">Zwischenspeicherung im Arbeitsspeicher</span><span class="sxs-lookup"><span data-stu-id="037ab-156">In-memory caching</span></span>

<span data-ttu-id="037ab-157">Zwischenspeicherung im Arbeitsspeicher verwendet Server-Speicher zum Speichern von zwischengespeicherter Daten.</span><span class="sxs-lookup"><span data-stu-id="037ab-157">In-memory caching uses server memory to store cached data.</span></span> <span data-ttu-id="037ab-158">Diese Form des Cachings eignet sich für einen einzelnen oder mehrerer Server mithilfe von *persistente Sitzungen*.</span><span class="sxs-lookup"><span data-stu-id="037ab-158">This type of caching is suitable for a single server or multiple servers using *sticky sessions*.</span></span> <span data-ttu-id="037ab-159">Persistente Sitzungen bedeutet, dass die Anforderungen von einem Client immer auf dem gleichen Server zur Verarbeitung weitergeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="037ab-159">Sticky sessions means that the requests from a client are always routed to the same server for processing.</span></span>

<span data-ttu-id="037ab-160">Weitere Informationen finden Sie unter [in-Memory-Cache](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="037ab-160">For more information, see [Cache in-memory](xref:performance/caching/memory).</span></span>

### <a name="distributed-cache"></a><span data-ttu-id="037ab-161">Verteilter Cache</span><span class="sxs-lookup"><span data-stu-id="037ab-161">Distributed Cache</span></span>

<span data-ttu-id="037ab-162">Verwenden Sie einen verteilten Cache zum Speichern von Daten im Arbeitsspeicher, wenn die app in einer Cloud oder Server-Farm gehostet wird.</span><span class="sxs-lookup"><span data-stu-id="037ab-162">Use a distributed cache to store data in memory when the app is hosted in a cloud or server farm.</span></span> <span data-ttu-id="037ab-163">Der Cache wird auf allen Servern gemeinsam genutzt, die Anforderungen verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="037ab-163">The cache is shared across the servers that process requests.</span></span> <span data-ttu-id="037ab-164">Ein Client kann eine Anforderung übermitteln, die von einem beliebigen Server in der Gruppe verarbeitet wird, wenn zwischengespeicherte Daten für den Client verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="037ab-164">A client can submit a request that's handled by any server in the group if cached data for the client is available.</span></span> <span data-ttu-id="037ab-165">ASP.NET Core bietet SQL Server und verteilt Redis-Caches.</span><span class="sxs-lookup"><span data-stu-id="037ab-165">ASP.NET Core offers SQL Server and Redis distributed caches.</span></span>

<span data-ttu-id="037ab-166">Weitere Informationen finden Sie unter <xref:performance/caching/distributed>.</span><span class="sxs-lookup"><span data-stu-id="037ab-166">For more information, see <xref:performance/caching/distributed>.</span></span>

### <a name="cache-tag-helper"></a><span data-ttu-id="037ab-167">Cache-Taghilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="037ab-167">Cache Tag Helper</span></span>

<span data-ttu-id="037ab-168">Sie können den Inhalt von einem MVC-Ansicht oder Razor-Seite mit Cache-Taghilfsprogramms Zwischenspeichern.</span><span class="sxs-lookup"><span data-stu-id="037ab-168">You can cache the content from an MVC view or Razor Page with the Cache Tag Helper.</span></span> <span data-ttu-id="037ab-169">Das Cache-Taghilfsprogramm verwendet speicherinternes caching, um Daten zu speichern.</span><span class="sxs-lookup"><span data-stu-id="037ab-169">The Cache Tag Helper uses in-memory caching to store data.</span></span>

<span data-ttu-id="037ab-170">Weitere Informationen finden Sie unter [Cache-Taghilfsprogramm im ASP.NET Core MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="037ab-170">For more information, see [Cache Tag Helper in ASP.NET Core MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span></span>

### <a name="distributed-cache-tag-helper"></a><span data-ttu-id="037ab-171">Taghilfsprogramm für verteilten Cache</span><span class="sxs-lookup"><span data-stu-id="037ab-171">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="037ab-172">Sie können den Inhalt von einem MVC-Ansicht oder Razor-Seite in verteilten Cloud oder Webfarm-Szenarios mit dem Taghilfsprogramm für verteilten Cache zwischenspeichern.</span><span class="sxs-lookup"><span data-stu-id="037ab-172">You can cache the content from an MVC view or Razor Page in distributed cloud or web farm scenarios with the Distributed Cache Tag Helper.</span></span> <span data-ttu-id="037ab-173">Das Taghilfsprogramm für verteilten Cache werden SQL Server oder Redis verwendet, um Daten zu speichern.</span><span class="sxs-lookup"><span data-stu-id="037ab-173">The Distributed Cache Tag Helper uses SQL Server or Redis to store data.</span></span>

<span data-ttu-id="037ab-174">Weitere Informationen finden Sie unter [Taghilfsprogramm für verteilten Cache](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="037ab-174">For more information, see [Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).</span></span>

## <a name="responsecache-attribute"></a><span data-ttu-id="037ab-175">ResponseCache-Attribut</span><span class="sxs-lookup"><span data-stu-id="037ab-175">ResponseCache attribute</span></span>

<span data-ttu-id="037ab-176">Die [ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) gibt die Parameter zum Festlegen der entsprechenden Header in das Zwischenspeichern von Antworten erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="037ab-176">The [ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) specifies the parameters necessary for setting appropriate headers in response caching.</span></span>

> [!WARNING]
> <span data-ttu-id="037ab-177">Deaktivieren Sie die Zwischenspeicherung für Inhalte, die Informationen für authentifizierte Clients enthält.</span><span class="sxs-lookup"><span data-stu-id="037ab-177">Disable caching for content that contains information for authenticated clients.</span></span> <span data-ttu-id="037ab-178">Zwischenspeichern von sollte nur aktiviert werden, für Inhalte, die sich nicht ändert, die anhand eines Benutzers Identität oder, ob ein Benutzer angemeldet ist.</span><span class="sxs-lookup"><span data-stu-id="037ab-178">Caching should only be enabled for content that doesn't change based on a user's identity or whether a user is signed in.</span></span>

<span data-ttu-id="037ab-179">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) hängt von der gespeicherten Antwort nach den Werten der angegebenen Liste von Abfrage-Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="037ab-179">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) varies the stored response by the values of the given list of query keys.</span></span> <span data-ttu-id="037ab-180">Wenn ein einzelner Wert des `*` angegeben wird, hängt von die Middleware Antworten von allen anfordern, Abfragezeichenfolgen-Parameter.</span><span class="sxs-lookup"><span data-stu-id="037ab-180">When a single value of `*` is provided, the middleware varies responses by all request query string parameters.</span></span> <span data-ttu-id="037ab-181">`VaryByQueryKeys` erfordert ASP.NET Core 1.1 oder höher.</span><span class="sxs-lookup"><span data-stu-id="037ab-181">`VaryByQueryKeys` requires ASP.NET Core 1.1 or later.</span></span>

<span data-ttu-id="037ab-182">Die Middleware für die Antwort-Zwischenspeicherung muss aktiviert sein, Festlegen der `VaryByQueryKeys` Eigenschaft; andernfalls wird eine Laufzeitausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="037ab-182">The Response Caching Middleware must be enabled to set the `VaryByQueryKeys` property; otherwise, a runtime exception is thrown.</span></span> <span data-ttu-id="037ab-183">Es gibt keine entsprechenden HTTP-Header für die `VaryByQueryKeys` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="037ab-183">There isn't a corresponding HTTP header for the `VaryByQueryKeys` property.</span></span> <span data-ttu-id="037ab-184">Die Eigenschaft ist eine HTTP-Funktion, die von der Antwort-Caching-Middleware verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="037ab-184">The property is an HTTP feature handled by the Response Caching Middleware.</span></span> <span data-ttu-id="037ab-185">Für die Middleware, um eine zwischengespeicherte Antwort zu verarbeiten müssen der Abfragezeichenfolge und den Wert der Abfragezeichenfolge eine vorherige Anforderung übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="037ab-185">For the middleware to serve a cached response, the query string and query string value must match a previous request.</span></span> <span data-ttu-id="037ab-186">Betrachten Sie beispielsweise die Reihenfolge der Anforderungen und Ergebnissen, die in der folgenden Tabelle gezeigt.</span><span class="sxs-lookup"><span data-stu-id="037ab-186">For example, consider the sequence of requests and results shown in the following table.</span></span>

| <span data-ttu-id="037ab-187">Anforderung</span><span class="sxs-lookup"><span data-stu-id="037ab-187">Request</span></span>                          | <span data-ttu-id="037ab-188">Ergebnis</span><span class="sxs-lookup"><span data-stu-id="037ab-188">Result</span></span>                   |
| -------------------------------- | ------------------------ |
| `http://example.com?key1=value1` | <span data-ttu-id="037ab-189">Vom Server zurückgegebenen</span><span class="sxs-lookup"><span data-stu-id="037ab-189">Returned from server</span></span>     |
| `http://example.com?key1=value1` | <span data-ttu-id="037ab-190">Zurückgegeben von middleware</span><span class="sxs-lookup"><span data-stu-id="037ab-190">Returned from middleware</span></span> |
| `http://example.com?key1=value2` | <span data-ttu-id="037ab-191">Vom Server zurückgegebenen</span><span class="sxs-lookup"><span data-stu-id="037ab-191">Returned from server</span></span>     |

<span data-ttu-id="037ab-192">Die erste Anforderung wird vom Server zurückgegebenen und im Middleware zwischengespeichert.</span><span class="sxs-lookup"><span data-stu-id="037ab-192">The first request is returned by the server and cached in middleware.</span></span> <span data-ttu-id="037ab-193">Die zweite Anforderung wird von Middleware zurückgegeben, da die Abfragezeichenfolge die vorherige Anforderung übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="037ab-193">The second request is returned by middleware because the query string matches the previous request.</span></span> <span data-ttu-id="037ab-194">Die dritte Anforderung ist nicht im Cache Middleware, dass Abfragezeichenfolgen-Werts nicht mit eine vorherige Anforderung übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="037ab-194">The third request isn't in the middleware cache because the query string value doesn't match a previous request.</span></span> 

<span data-ttu-id="037ab-195">Die `ResponseCacheAttribute` dient zum Konfigurieren und erstellen Sie (über `IFilterFactory`) eine [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter).</span><span class="sxs-lookup"><span data-stu-id="037ab-195">The `ResponseCacheAttribute` is used to configure and create (via `IFilterFactory`) a [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter).</span></span> <span data-ttu-id="037ab-196">Die `ResponseCacheFilter` die Arbeit der Aktualisierung des entsprechenden HTTP-Header und Funktionen der Antwort durchführt.</span><span class="sxs-lookup"><span data-stu-id="037ab-196">The `ResponseCacheFilter` performs the work of updating the appropriate HTTP headers and features of the response.</span></span> <span data-ttu-id="037ab-197">Der Filter:</span><span class="sxs-lookup"><span data-stu-id="037ab-197">The filter:</span></span>

* <span data-ttu-id="037ab-198">Entfernt alle vorhandenen Header für `Vary`, `Cache-Control`, und `Pragma`.</span><span class="sxs-lookup"><span data-stu-id="037ab-198">Removes any existing headers for `Vary`, `Cache-Control`, and `Pragma`.</span></span> 
* <span data-ttu-id="037ab-199">Schreibt die entsprechenden Header auf Grundlage der Eigenschaften legen Sie in der `ResponseCacheAttribute`.</span><span class="sxs-lookup"><span data-stu-id="037ab-199">Writes out the appropriate headers based on the properties set in the `ResponseCacheAttribute`.</span></span> 
* <span data-ttu-id="037ab-200">Aktualisiert die Antwort Zwischenspeichern von HTTP-Funktion, wenn `VaryByQueryKeys` festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="037ab-200">Updates the response caching HTTP feature if `VaryByQueryKeys` is set.</span></span>

### <a name="vary"></a><span data-ttu-id="037ab-201">Variieren</span><span class="sxs-lookup"><span data-stu-id="037ab-201">Vary</span></span>

<span data-ttu-id="037ab-202">Dieser Header wird nur geschrieben, wenn die `VaryByHeader` festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="037ab-202">This header is only written when the `VaryByHeader` property is set.</span></span> <span data-ttu-id="037ab-203">Festgelegt auf die `Vary` Eigenschaftswert.</span><span class="sxs-lookup"><span data-stu-id="037ab-203">It's set to the `Vary` property's value.</span></span> <span data-ttu-id="037ab-204">Das folgende Beispiel verwendet die `VaryByHeader` Eigenschaft:</span><span class="sxs-lookup"><span data-stu-id="037ab-204">The following sample uses the `VaryByHeader` property:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

<span data-ttu-id="037ab-205">Sie können die Antwortheadern mit Ihres Browsers Netzwerktools anzeigen.</span><span class="sxs-lookup"><span data-stu-id="037ab-205">You can view the response headers with your browser's network tools.</span></span> <span data-ttu-id="037ab-206">Die folgende Abbildung zeigt die Ausgabe auf Edge-F12 die **Netzwerk** Registerkarte, wenn die `About2` Aktionsmethode wird aktualisiert:</span><span class="sxs-lookup"><span data-stu-id="037ab-206">The following image shows the Edge F12 output on the **Network** tab when the `About2` action method is refreshed:</span></span>

![Edge F12-Ausgabe auf der Registerkarte "Netzwerk", wenn die About2 Aktionsmethode aufgerufen wird](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a><span data-ttu-id="037ab-208">NoStore und Location.None</span><span class="sxs-lookup"><span data-stu-id="037ab-208">NoStore and Location.None</span></span>

<span data-ttu-id="037ab-209">`NoStore` überschreibt die meisten anderen Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="037ab-209">`NoStore` overrides most of the other properties.</span></span> <span data-ttu-id="037ab-210">Wenn diese Eigenschaft auf festgelegt ist `true`, `Cache-Control` Header nastaven NA hodnotu `no-store`.</span><span class="sxs-lookup"><span data-stu-id="037ab-210">When this property is set to `true`, the `Cache-Control` header is set to `no-store`.</span></span> <span data-ttu-id="037ab-211">Wenn `Location` nastaven NA hodnotu `None`:</span><span class="sxs-lookup"><span data-stu-id="037ab-211">If `Location` is set to `None`:</span></span>

* <span data-ttu-id="037ab-212">Für `Cache-Control` ist `no-store,no-cache` festgelegt.</span><span class="sxs-lookup"><span data-stu-id="037ab-212">`Cache-Control` is set to `no-store,no-cache`.</span></span>
* <span data-ttu-id="037ab-213">Für `Pragma` ist `no-cache` festgelegt.</span><span class="sxs-lookup"><span data-stu-id="037ab-213">`Pragma` is set to `no-cache`.</span></span>

<span data-ttu-id="037ab-214">Wenn `NoStore` ist `false` und `Location` ist `None`, `Cache-Control` und `Pragma` festgelegt `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="037ab-214">If `NoStore` is `false` and `Location` is `None`, `Cache-Control` and `Pragma` are set to `no-cache`.</span></span>

<span data-ttu-id="037ab-215">Legen Sie Sie in der Regel `NoStore` zu `true` auf Fehler.</span><span class="sxs-lookup"><span data-stu-id="037ab-215">You typically set `NoStore` to `true` on error pages.</span></span> <span data-ttu-id="037ab-216">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="037ab-216">For example:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

<span data-ttu-id="037ab-217">Dies ergibt die folgenden Header:</span><span class="sxs-lookup"><span data-stu-id="037ab-217">This results in the following headers:</span></span>

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a><span data-ttu-id="037ab-218">Position und Dauer</span><span class="sxs-lookup"><span data-stu-id="037ab-218">Location and Duration</span></span>

<span data-ttu-id="037ab-219">Zum Aktivieren der Zwischenspeicherung, `Duration` muss auf einen positiven Wert festgelegt werden und `Location` muss `Any` (Standard) oder `Client`.</span><span class="sxs-lookup"><span data-stu-id="037ab-219">To enable caching, `Duration` must be set to a positive value and `Location` must be either `Any` (the default) or `Client`.</span></span> <span data-ttu-id="037ab-220">In diesem Fall die `Cache-Control` Header wird festgelegt, um den Speicherort angeben, gefolgt von der `max-age` der Antwort.</span><span class="sxs-lookup"><span data-stu-id="037ab-220">In this case, the `Cache-Control` header is set to the location value followed by the `max-age` of the response.</span></span>

> [!NOTE]
> <span data-ttu-id="037ab-221">`Location`die Optionen der `Any` und `Client` übersetzen in `Cache-Control` Headerwerte `public` und `private`bzw.</span><span class="sxs-lookup"><span data-stu-id="037ab-221">`Location`'s options of `Any` and `Client` translate into `Cache-Control` header values of `public` and `private`, respectively.</span></span> <span data-ttu-id="037ab-222">Wie bereits erwähnt, festlegen `Location` zu `None` legt sowohl `Cache-Control` und `Pragma` Header `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="037ab-222">As noted previously, setting `Location` to `None` sets both `Cache-Control` and `Pragma` headers to `no-cache`.</span></span>

<span data-ttu-id="037ab-223">Im folgenden wird ein Beispiel für die Header erstellt, durch Festlegen von `Duration` und verlassen die Standardeinstellung `Location` Wert:</span><span class="sxs-lookup"><span data-stu-id="037ab-223">Below is an example showing the headers produced by setting `Duration` and leaving the default `Location` value:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

<span data-ttu-id="037ab-224">Dies erzeugt die folgende Kopfzeile:</span><span class="sxs-lookup"><span data-stu-id="037ab-224">This produces the following header:</span></span>

```
Cache-Control: public,max-age=60
```

### <a name="cache-profiles"></a><span data-ttu-id="037ab-225">Cacheprofile</span><span class="sxs-lookup"><span data-stu-id="037ab-225">Cache profiles</span></span>

<span data-ttu-id="037ab-226">Anstelle von duplizieren `ResponseCache` auf viele Aktion Attributen für Controller, Cacheprofilen konfiguriert werden können als Optionen beim Einrichten von MVC in die `ConfigureServices` -Methode in der `Startup`.</span><span class="sxs-lookup"><span data-stu-id="037ab-226">Instead of duplicating `ResponseCache` settings on many controller action attributes, cache profiles can be configured as options when setting up MVC in the `ConfigureServices` method in `Startup`.</span></span> <span data-ttu-id="037ab-227">In einer referenzierten Cacheprofil, gefundenen Werte werden verwendet, die standardmäßig von der `ResponseCache` Attribut, und werden überschrieben, nach beliebigen Eigenschaften des Attributs angegeben.</span><span class="sxs-lookup"><span data-stu-id="037ab-227">Values found in a referenced cache profile are used as the defaults by the `ResponseCache` attribute and are overridden by any properties specified on the attribute.</span></span>

<span data-ttu-id="037ab-228">Das Einrichten eines Cacheprofils:</span><span class="sxs-lookup"><span data-stu-id="037ab-228">Setting up a cache profile:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="037ab-229">Verweisen auf ein Cacheprofil aus:</span><span class="sxs-lookup"><span data-stu-id="037ab-229">Referencing a cache profile:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

<span data-ttu-id="037ab-230">Die `ResponseCache` sowohl Aktionen (Methoden) und Controller (Klassen) können Attribute angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="037ab-230">The `ResponseCache` attribute can be applied both to actions (methods) and controllers (classes).</span></span> <span data-ttu-id="037ab-231">Auf Methodenebene Attribute überschreiben die Einstellungen, die in den auf Klassenebene Attribute angegeben.</span><span class="sxs-lookup"><span data-stu-id="037ab-231">Method-level attributes override the settings specified in class-level attributes.</span></span>

<span data-ttu-id="037ab-232">Im obigen Beispiel gibt ein Attributs auf Klassenebene eine Dauer von 30 Sekunden, während ein Attributs auf Methodenebene. ein Cacheprofil mit einer Dauer auf 60 Sekunden festgelegt verweist.</span><span class="sxs-lookup"><span data-stu-id="037ab-232">In the above example, a class-level attribute specifies a duration of 30 seconds, while a method-level attribute references a cache profile with a duration set to 60 seconds.</span></span>

<span data-ttu-id="037ab-233">Die resultierende Header:</span><span class="sxs-lookup"><span data-stu-id="037ab-233">The resulting header:</span></span>

```
Cache-Control: public,max-age=60
```

## <a name="additional-resources"></a><span data-ttu-id="037ab-234">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="037ab-234">Additional resources</span></span>

* [<span data-ttu-id="037ab-235">Das Speichern von Antworten in Caches</span><span class="sxs-lookup"><span data-stu-id="037ab-235">Storing Responses in Caches</span></span>](https://tools.ietf.org/html/rfc7234#section-3)
* [<span data-ttu-id="037ab-236">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="037ab-236">Cache-Control</span></span>](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
