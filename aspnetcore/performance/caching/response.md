---
title: Zwischenspeichern von Antworten in ASP.NET Core
author: rick-anderson
description: Informationen Sie zum Verwenden von caching zu niedrigeren bandbreitenanforderungen Antwort und erhöhen Sie der Leistung von ASP.NET Core-apps.
ms.author: riande
ms.date: 09/20/2017
uid: performance/caching/response
ms.openlocfilehash: c53ae3f6ab8d26588533772dd4fdacb36ec12059
ms.sourcegitcommit: 931b6a2d7eb28a0f1295e8a95690b8c4c5f58477
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2018
ms.locfileid: "37077763"
---
# <a name="response-caching-in-aspnet-core"></a><span data-ttu-id="bf2c5-103">Zwischenspeichern von Antworten in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bf2c5-103">Response caching in ASP.NET Core</span></span>

<span data-ttu-id="bf2c5-104">Durch [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), und [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="bf2c5-104">By [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Luke Latham](https://github.com/guardrex)</span></span>

> [!NOTE]
> <span data-ttu-id="bf2c5-105">Zwischenspeichern von Antworten in Razor-Seiten wird in ASP.NET Core 2.1 oder höher verfügbar.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-105">Response caching in Razor Pages is available in ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="bf2c5-106">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bf2c5-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="bf2c5-107">Zwischenspeichern von Antworten verringert die Anzahl der Anforderungen, die ein Client oder Proxy auf einem Webserver vornimmt.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-107">Response caching reduces the number of requests a client or proxy makes to a web server.</span></span> <span data-ttu-id="bf2c5-108">Zwischenspeichern von Antworten auch reduziert die Menge der Arbeit der Webserver durchführt, um eine Antwort zu generieren.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-108">Response caching also reduces the amount of work the web server performs to generate a response.</span></span> <span data-ttu-id="bf2c5-109">Zwischenspeichern von Antworten wird durch Header gesteuert, die angeben, wie der Client und Proxy-Middleware zum Zwischenspeichern von Antworten soll.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-109">Response caching is controlled by headers that specify how you want client, proxy, and middleware to cache responses.</span></span>

<span data-ttu-id="bf2c5-110">Die Webserver kann Antworten zwischenspeichern, beim Hinzufügen [Antwort zwischenspeichern Middleware](xref:performance/caching/middleware).</span><span class="sxs-lookup"><span data-stu-id="bf2c5-110">The web server can cache responses when you add [Response Caching Middleware](xref:performance/caching/middleware).</span></span>

## <a name="http-based-response-caching"></a><span data-ttu-id="bf2c5-111">Zwischenspeichern von Antworten HTTP-basierte</span><span class="sxs-lookup"><span data-stu-id="bf2c5-111">HTTP-based response caching</span></span>

<span data-ttu-id="bf2c5-112">Die [Zwischenspeichern von HTTP 1.1-Spezifikation](https://tools.ietf.org/html/rfc7234) wird beschrieben, wie Internet Caches Verhalten soll.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-112">The [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234) describes how Internet caches should behave.</span></span> <span data-ttu-id="bf2c5-113">Ist der primäre HTTP-Header, die zum Zwischenspeichern verwendeten [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), der verwendet wird, geben Sie den Cache *Direktiven*.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-113">The primary HTTP header used for caching is [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), which is used to specify cache *directives*.</span></span> <span data-ttu-id="bf2c5-114">Die Direktiven steuern Verhalten beim Zwischenspeichern, wie Anforderungen wie von Clients an Server senden und eine Antwort von Servern an Clients die Möglichkeit.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-114">The directives control caching behavior as requests make their way from clients to servers and as reponses make their way from servers back to clients.</span></span> <span data-ttu-id="bf2c5-115">Anforderungen und Antworten verschieben, über Proxy-Server und Proxy-Server müssen auch das Zwischenspeichern von HTTP 1.1-Spezifikation entsprechen.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-115">Requests and responses move through proxy servers, and proxy servers must also conform to the HTTP 1.1 Caching specification.</span></span>

<span data-ttu-id="bf2c5-116">Allgemeine `Cache-Control` Direktiven sind in der folgenden Tabelle gezeigt.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-116">Common `Cache-Control` directives are shown in the following table.</span></span>

| <span data-ttu-id="bf2c5-117">Direktive</span><span class="sxs-lookup"><span data-stu-id="bf2c5-117">Directive</span></span>                                                       | <span data-ttu-id="bf2c5-118">Aktion</span><span class="sxs-lookup"><span data-stu-id="bf2c5-118">Action</span></span> |
| --------------------------------------------------------------- | ------ |
| [<span data-ttu-id="bf2c5-119">public</span><span class="sxs-lookup"><span data-stu-id="bf2c5-119">public</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | <span data-ttu-id="bf2c5-120">Ein Cache möglicherweise die Antwort zu speichern.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-120">A cache may store the response.</span></span> |
| [<span data-ttu-id="bf2c5-121">private</span><span class="sxs-lookup"><span data-stu-id="bf2c5-121">private</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | <span data-ttu-id="bf2c5-122">Die Antwort muss von einem geteilten Datencache nicht gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-122">The response must not be stored by a shared cache.</span></span> <span data-ttu-id="bf2c5-123">Ein privater Cache möglicherweise speichern und wiederverwenden die Antwort.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-123">A private cache may store and reuse the response.</span></span> |
| [<span data-ttu-id="bf2c5-124">Max-age</span><span class="sxs-lookup"><span data-stu-id="bf2c5-124">max-age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | <span data-ttu-id="bf2c5-125">Der Client wird nicht als eine Antwort nicht akzeptiert, deren Alter größer als die angegebene Anzahl von Sekunden ist.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-125">The client won't accept a response whose age is greater than the specified number of seconds.</span></span> <span data-ttu-id="bf2c5-126">Beispiele: `max-age=60` (60 Sekunden), `max-age=2592000` (1 Monat)</span><span class="sxs-lookup"><span data-stu-id="bf2c5-126">Examples: `max-age=60` (60 seconds), `max-age=2592000` (1 month)</span></span> |
| [<span data-ttu-id="bf2c5-127">ohne-cache</span><span class="sxs-lookup"><span data-stu-id="bf2c5-127">no-cache</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | <span data-ttu-id="bf2c5-128">**Für Anforderungen**: ein Caches muss nicht gespeicherte Antwort zum Erfüllen der Anforderung verwenden.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-128">**On requests**: A cache must not use a stored response to satisfy the request.</span></span> <span data-ttu-id="bf2c5-129">Hinweis: Ursprungsservers wird erneut die Antwort für den Client generiert und die Middleware aktualisiert die gespeicherte Antwort in seinem Cache.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-129">Note: The origin server re-generates the response for the client, and the middleware updates the stored response in its cache.</span></span><br><br><span data-ttu-id="bf2c5-130">**Auf Antworten**: die Antwort dürfen nicht für eine nachfolgende Anforderung ohne Überprüfung auf dem Ursprungsserver verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-130">**On responses**: The response must not be used for a subsequent request without validation on the origin server.</span></span> |
| [<span data-ttu-id="bf2c5-131">ohne-Speicher</span><span class="sxs-lookup"><span data-stu-id="bf2c5-131">no-store</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | <span data-ttu-id="bf2c5-132">**Für Anforderungen**: Speichern ein Caches muss nicht die Anforderung.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-132">**On requests**: A cache must not store the request.</span></span><br><br><span data-ttu-id="bf2c5-133">**Auf Antworten**: ein Caches muss einen beliebigen Teil der Antwort nicht speichern.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-133">**On responses**: A cache must not store any part of the response.</span></span> |

<span data-ttu-id="bf2c5-134">Andere Cacheheader, die eine Rolle am caching spielen sind in der folgenden Tabelle gezeigt.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-134">Other cache headers that play a role in caching are shown in the following table.</span></span>

| <span data-ttu-id="bf2c5-135">Header</span><span class="sxs-lookup"><span data-stu-id="bf2c5-135">Header</span></span>                                                     | <span data-ttu-id="bf2c5-136">Funktion</span><span class="sxs-lookup"><span data-stu-id="bf2c5-136">Function</span></span> |
| ---------------------------------------------------------- | -------- |
| [<span data-ttu-id="bf2c5-137">ALTER</span><span class="sxs-lookup"><span data-stu-id="bf2c5-137">Age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.1)     | <span data-ttu-id="bf2c5-138">Eine Schätzung der die Zeitdauer in Sekunden seit die Antwort generiert wurde, oder auf dem Ausgangsserver erfolgreich überprüft.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-138">An estimate of the amount of time in seconds since the response was generated or successfully validated at the origin server.</span></span> |
| [<span data-ttu-id="bf2c5-139">Läuft ab</span><span class="sxs-lookup"><span data-stu-id="bf2c5-139">Expires</span></span>](https://tools.ietf.org/html/rfc7234#section-5.3) | <span data-ttu-id="bf2c5-140">Das Datum/Uhrzeit, nach dem die Antwort als veraltet angesehen wird.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-140">The date/time after which the response is considered stale.</span></span> |
| [<span data-ttu-id="bf2c5-141">Pragma</span><span class="sxs-lookup"><span data-stu-id="bf2c5-141">Pragma</span></span>](https://tools.ietf.org/html/rfc7234#section-5.4)  | <span data-ttu-id="bf2c5-142">Vorhanden ist, für die Kompatibilität mit HTTP/1.0 Abwärtskompatibilität Einstellung zwischenspeichert `no-cache` Verhalten.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-142">Exists for backwards compatibility with HTTP/1.0 caches for setting `no-cache` behavior.</span></span> <span data-ttu-id="bf2c5-143">Wenn die `Cache-Control` Header vorhanden ist, ist die `Pragma` -Header wird ignoriert.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-143">If the `Cache-Control` header is present, the `Pragma` header is ignored.</span></span> |
| [<span data-ttu-id="bf2c5-144">Variieren</span><span class="sxs-lookup"><span data-stu-id="bf2c5-144">Vary</span></span>](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | <span data-ttu-id="bf2c5-145">Gibt an, dass eine zwischengespeicherte Antwort nicht, wenn alle gesendet werden muss von der `Vary` Headerfelder entsprechen, in die zwischengespeicherte Antwort ursprüngliche Anforderung und die neue Anforderung.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-145">Specifies that a cached response must not be sent unless all of the `Vary` header fields match in both the cached response's original request and the new request.</span></span> |

## <a name="http-based-caching-respects-request-cache-control-directives"></a><span data-ttu-id="bf2c5-146">HTTP-basierte Zwischenspeichern Hinsicht anfordern cachesteuerungsdirektiven</span><span class="sxs-lookup"><span data-stu-id="bf2c5-146">HTTP-based caching respects request Cache-Control directives</span></span>

<span data-ttu-id="bf2c5-147">Die [Zwischenspeichern von HTTP 1.1-Spezifikation für den Cache-Control-Header](https://tools.ietf.org/html/rfc7234#section-5.2) erfordert einen Cache für eine gültige berücksichtigt `Cache-Control` vom Client gesendeten Header.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-147">The [HTTP 1.1 Caching specification for the Cache-Control header](https://tools.ietf.org/html/rfc7234#section-5.2) requires a cache to honor a valid `Cache-Control` header sent by the client.</span></span> <span data-ttu-id="bf2c5-148">Ein Client kann Anforderungen mit stellen eine `no-cache` Headerwert "und" Force "dem Server um eine neue Antwort für jede Anforderung zu generieren.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-148">A client can make requests with a `no-cache` header value and force the server to generate a new response for every request.</span></span>

<span data-ttu-id="bf2c5-149">Berücksichtigt immer Client `Cache-Control` Anforderungsheader ist sinnvoll, wenn Sie das Ziel des HTTP-caching in Betracht ziehen.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-149">Always honoring client `Cache-Control` request headers makes sense if you consider the goal of HTTP caching.</span></span> <span data-ttu-id="bf2c5-150">Unter die offizielle Spezifikation caching bedeutet, dass die Latenz und Netzwerkaufwand verringert der Erfüllung von Anforderungen in einem Netzwerk des Clients, Proxys und Servern zu verringern.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-150">Under the official specification, caching is meant to reduce the latency and network overhead of satisfying requests across a network of clients, proxies, and servers.</span></span> <span data-ttu-id="bf2c5-151">Es ist nicht unbedingt eine Möglichkeit, um die Last auf eine Ursprungsserver zu steuern.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-151">It isn't necessarily a way to control the load on an origin server.</span></span>

<span data-ttu-id="bf2c5-152">Keine aktuelle entwicklersteuerung dieses Verhalten beim Zwischenspeichern vorhanden ist, bei Verwendung der [Antwort zwischenspeichern Middleware](xref:performance/caching/middleware) , da die Middleware zu den offiziellen caching-Spezifikation entspricht.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-152">There's no current developer control over this caching behavior when using the [Response Caching Middleware](xref:performance/caching/middleware) because the middleware adheres to the official caching specification.</span></span> <span data-ttu-id="bf2c5-153">[Zukünftiger Verbesserungen der Middleware](https://github.com/aspnet/ResponseCaching/issues/96) gestattet, konfigurieren die Middleware, um einer Anforderung zu ignorieren `Cache-Control` Header, die bei der Entscheidung, die eine zwischengespeicherte Antwort dient.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-153">[Future enhancements to the middleware](https://github.com/aspnet/ResponseCaching/issues/96) will permit configuring the middleware to ignore a request's `Cache-Control` header when deciding to serve a cached response.</span></span> <span data-ttu-id="bf2c5-154">Dies wird Sie bieten eine Möglichkeit, eine bessere Steuerung der Last auf dem Server bei der Verwendung von der Middleware.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-154">This will offer you an opportunity to better control the load on your server when you use the middleware.</span></span>

## <a name="other-caching-technology-in-aspnet-core"></a><span data-ttu-id="bf2c5-155">Andere Zwischenspeichern Technologie in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bf2c5-155">Other caching technology in ASP.NET Core</span></span>

### <a name="in-memory-caching"></a><span data-ttu-id="bf2c5-156">Im Arbeitsspeicher Zwischenspeichern</span><span class="sxs-lookup"><span data-stu-id="bf2c5-156">In-memory caching</span></span>

<span data-ttu-id="bf2c5-157">Im Arbeitsspeicher Zwischenspeichern verwendet Serverarbeitsspeicher zum Speichern von zwischengespeicherter Daten.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-157">In-memory caching uses server memory to store cached data.</span></span> <span data-ttu-id="bf2c5-158">Diese Form des Cachings eignet sich für einen einzelnen oder mehrerer Server mithilfe von *persistente Sitzungen*.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-158">This type of caching is suitable for a single server or multiple servers using *sticky sessions*.</span></span> <span data-ttu-id="bf2c5-159">Persistente Sitzungen bedeutet, dass die Anfragen von einem Client immer mit dem gleichen Server zur Verarbeitung weitergeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-159">Sticky sessions means that the requests from a client are always routed to the same server for processing.</span></span>

<span data-ttu-id="bf2c5-160">Weitere Informationen finden Sie unter [in-Memory-Cache](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="bf2c5-160">For more information, see [Cache in-memory](xref:performance/caching/memory).</span></span>

### <a name="distributed-cache"></a><span data-ttu-id="bf2c5-161">Verteilter Cache</span><span class="sxs-lookup"><span data-stu-id="bf2c5-161">Distributed Cache</span></span>

<span data-ttu-id="bf2c5-162">Verwenden Sie einen verteilten Cache zum Speichern von Daten im Arbeitsspeicher, wenn die app in einer Cloud oder Server-Farm gehostet wird.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-162">Use a distributed cache to store data in memory when the app is hosted in a cloud or server farm.</span></span> <span data-ttu-id="bf2c5-163">Der Cache wird auf den Servern gemeinsam genutzt, die Anforderungen zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-163">The cache is shared across the servers that process requests.</span></span> <span data-ttu-id="bf2c5-164">Ein Client kann eine Anforderung übermitteln, die von einem beliebigen Server in der Gruppe "behandelt wird, wenn zwischengespeicherte Daten für den Client verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-164">A client can submit a request that's handled by any server in the group if cached data for the client is available.</span></span> <span data-ttu-id="bf2c5-165">ASP.NET Core bietet SQL Server und verteilt Redis-Caches.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-165">ASP.NET Core offers SQL Server and Redis distributed caches.</span></span>

<span data-ttu-id="bf2c5-166">Weitere Informationen finden Sie unter [arbeiten mit einem verteilten Cache](xref:performance/caching/distributed).</span><span class="sxs-lookup"><span data-stu-id="bf2c5-166">For more information, see [Work with a distributed cache](xref:performance/caching/distributed).</span></span>

### <a name="cache-tag-helper"></a><span data-ttu-id="bf2c5-167">Cache-Tag-Hilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="bf2c5-167">Cache Tag Helper</span></span>

<span data-ttu-id="bf2c5-168">Den Inhalt aus einer MVC-Ansicht oder Razor-Seite können mit dem Tag-Helfer Cache zwischengespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-168">You can cache the content from an MVC view or Razor Page with the Cache Tag Helper.</span></span> <span data-ttu-id="bf2c5-169">Der Cache-Tag-Hilfsmethode verwendet im Arbeitsspeicher zwischenspeichern, um Daten zu speichern.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-169">The Cache Tag Helper uses in-memory caching to store data.</span></span>

<span data-ttu-id="bf2c5-170">Weitere Informationen finden Sie unter [Cache-Taghilfsprogramm im ASP.NET Core MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="bf2c5-170">For more information, see [Cache Tag Helper in ASP.NET Core MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span></span>

### <a name="distributed-cache-tag-helper"></a><span data-ttu-id="bf2c5-171">Taghilfsprogramm für verteilten Cache</span><span class="sxs-lookup"><span data-stu-id="bf2c5-171">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="bf2c5-172">Den Inhalt aus einer MVC-Ansicht oder Razor-Seite in verteilten Cloud oder Webfarm-Szenarien können mit verteilten Cache-Tag-Hilfsprogramm zwischengespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-172">You can cache the content from an MVC view or Razor Page in distributed cloud or web farm scenarios with the Distributed Cache Tag Helper.</span></span> <span data-ttu-id="bf2c5-173">Das verteilte Cache-Tag-Hilfsobjekt verwendet SQL Server oder Redis zum Speichern von Daten an.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-173">The Distributed Cache Tag Helper uses SQL Server or Redis to store data.</span></span>

<span data-ttu-id="bf2c5-174">Weitere Informationen finden Sie unter [verteilten Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="bf2c5-174">For more information, see [Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).</span></span>

## <a name="responsecache-attribute"></a><span data-ttu-id="bf2c5-175">ResponseCache-Attribut</span><span class="sxs-lookup"><span data-stu-id="bf2c5-175">ResponseCache attribute</span></span>

<span data-ttu-id="bf2c5-176">Die [ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) gibt die Parameter zum Festlegen der entsprechenden Header in Zwischenspeichern von Antworten erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-176">The [ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) specifies the parameters necessary for setting appropriate headers in response caching.</span></span>

> [!WARNING]
> <span data-ttu-id="bf2c5-177">Deaktivieren Sie das Zwischenspeichern für Inhalte, die Informationen für authentifizierte Clients enthält.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-177">Disable caching for content that contains information for authenticated clients.</span></span> <span data-ttu-id="bf2c5-178">Zwischenspeichern sollten nur aktiviert werden, für Inhalte, die nicht ändern, die auf Grundlage eines Benutzers Identität oder gibt an, ob ein Benutzer angemeldet ist.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-178">Caching should only be enabled for content that doesn't change based on a user's identity or whether a user is signed in.</span></span>

<span data-ttu-id="bf2c5-179">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) gespeicherten Antwort von den Werten der angegebenen Liste der Abfrageschlüssel variiert.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-179">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) varies the stored response by the values of the given list of query keys.</span></span> <span data-ttu-id="bf2c5-180">Wenn ein einzelner Wert `*` angegeben wird, hängt von die Middleware Antworten von allen anfordern Abfragezeichenfolgen-Parameter.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-180">When a single value of `*` is provided, the middleware varies responses by all request query string parameters.</span></span> <span data-ttu-id="bf2c5-181">`VaryByQueryKeys` erfordert ASP.NET Core 1.1 oder höher.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-181">`VaryByQueryKeys` requires ASP.NET Core 1.1 or later.</span></span>

<span data-ttu-id="bf2c5-182">Die Antwort zwischenspeichern Middleware muss aktiviert sein, zum Festlegen der `VaryByQueryKeys` Eigenschaft; andernfalls wird eine Laufzeitausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-182">The Response Caching Middleware must be enabled to set the `VaryByQueryKeys` property; otherwise, a runtime exception is thrown.</span></span> <span data-ttu-id="bf2c5-183">Es gibt kein entsprechenden HTTP-Header für die `VaryByQueryKeys` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-183">There isn't a corresponding HTTP header for the `VaryByQueryKeys` property.</span></span> <span data-ttu-id="bf2c5-184">Die Eigenschaft ist eine HTTP-Funktion, die von der Antwort zwischenspeichern Middleware verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-184">The property is an HTTP feature handled by the Response Caching Middleware.</span></span> <span data-ttu-id="bf2c5-185">Für die Middleware, die eine zwischengespeicherte Antwort dient müssen der Abfragezeichenfolge und der Wert der Abfragezeichenfolge eine frühere Anforderung wieder übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-185">For the middleware to serve a cached response, the query string and query string value must match a previous request.</span></span> <span data-ttu-id="bf2c5-186">Betrachten Sie beispielsweise die Sequenz von Anforderungen und Ergebnissen, die in der folgenden Tabelle gezeigt.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-186">For example, consider the sequence of requests and results shown in the following table.</span></span>

| <span data-ttu-id="bf2c5-187">Anforderung</span><span class="sxs-lookup"><span data-stu-id="bf2c5-187">Request</span></span>                          | <span data-ttu-id="bf2c5-188">Ergebnis</span><span class="sxs-lookup"><span data-stu-id="bf2c5-188">Result</span></span>                   |
| -------------------------------- | ------------------------ |
| `http://example.com?key1=value1` | <span data-ttu-id="bf2c5-189">Vom Server zurückgegebene</span><span class="sxs-lookup"><span data-stu-id="bf2c5-189">Returned from server</span></span>     |
| `http://example.com?key1=value1` | <span data-ttu-id="bf2c5-190">Von der Middleware zurückgegeben</span><span class="sxs-lookup"><span data-stu-id="bf2c5-190">Returned from middleware</span></span> |
| `http://example.com?key1=value2` | <span data-ttu-id="bf2c5-191">Vom Server zurückgegebene</span><span class="sxs-lookup"><span data-stu-id="bf2c5-191">Returned from server</span></span>     |

<span data-ttu-id="bf2c5-192">Die erste Anforderung wird vom Server zurückgegebenen und Middleware zwischengespeichert.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-192">The first request is returned by the server and cached in middleware.</span></span> <span data-ttu-id="bf2c5-193">Die zweite Anforderung wird von Middleware zurückgegeben, weil die Abfragezeichenfolge die vorhergehenden Anforderung übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-193">The second request is returned by middleware because the query string matches the previous request.</span></span> <span data-ttu-id="bf2c5-194">Die dritte Anforderung ist nicht im Cache Middleware, da der Wert der Abfragezeichenfolge nicht mit eine frühere Anforderung übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-194">The third request isn't in the middleware cache because the query string value doesn't match a previous request.</span></span> 

<span data-ttu-id="bf2c5-195">Die `ResponseCacheAttribute` dient zum Erstellen und konfigurieren Sie (über `IFilterFactory`) eine [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter).</span><span class="sxs-lookup"><span data-stu-id="bf2c5-195">The `ResponseCacheAttribute` is used to configure and create (via `IFilterFactory`) a [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter).</span></span> <span data-ttu-id="bf2c5-196">Die `ResponseCacheFilter` führt die Arbeit aktualisieren die entsprechenden HTTP-Header und die Funktionen der Antwort.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-196">The `ResponseCacheFilter` performs the work of updating the appropriate HTTP headers and features of the response.</span></span> <span data-ttu-id="bf2c5-197">Der Filter:</span><span class="sxs-lookup"><span data-stu-id="bf2c5-197">The filter:</span></span>

* <span data-ttu-id="bf2c5-198">Entfernt alle vorhandenen Header für `Vary`, `Cache-Control`, und `Pragma`.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-198">Removes any existing headers for `Vary`, `Cache-Control`, and `Pragma`.</span></span> 
* <span data-ttu-id="bf2c5-199">Schreibt die entsprechenden Header auf Grundlage der Eigenschaften legen Sie in der `ResponseCacheAttribute`.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-199">Writes out the appropriate headers based on the properties set in the `ResponseCacheAttribute`.</span></span> 
* <span data-ttu-id="bf2c5-200">Aktualisiert die Antwort zwischenspeichern HTTP-Funktion, wenn `VaryByQueryKeys` festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-200">Updates the response caching HTTP feature if `VaryByQueryKeys` is set.</span></span>

### <a name="vary"></a><span data-ttu-id="bf2c5-201">Variieren</span><span class="sxs-lookup"><span data-stu-id="bf2c5-201">Vary</span></span>

<span data-ttu-id="bf2c5-202">Dieser Header wird nur geschrieben, wenn die `VaryByHeader` festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-202">This header is only written when the `VaryByHeader` property is set.</span></span> <span data-ttu-id="bf2c5-203">Es wird festgelegt, um die `Vary` den Wert der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-203">It's set to the `Vary` property's value.</span></span> <span data-ttu-id="bf2c5-204">Das folgende Beispiel verwendet die `VaryByHeader` Eigenschaft:</span><span class="sxs-lookup"><span data-stu-id="bf2c5-204">The following sample uses the `VaryByHeader` property:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

<span data-ttu-id="bf2c5-205">Sie können die Antwortheadern mit Ihrem Browser Netzwerktools anzeigen.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-205">You can view the response headers with your browser's network tools.</span></span> <span data-ttu-id="bf2c5-206">Die folgende Abbildung zeigt die Ausgabe auf Edge F12 der **Netzwerk** Registerkarte, wenn die `About2` Aktionsmethode aktualisiert wird:</span><span class="sxs-lookup"><span data-stu-id="bf2c5-206">The following image shows the Edge F12 output on the **Network** tab when the `About2` action method is refreshed:</span></span>

![Der Edge F12-Ausgabe auf der Registerkarte "Netzwerk" beim Aufrufen der Aktionsmethode About2](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a><span data-ttu-id="bf2c5-208">NoStore und Location.None</span><span class="sxs-lookup"><span data-stu-id="bf2c5-208">NoStore and Location.None</span></span>

<span data-ttu-id="bf2c5-209">`NoStore` überschreibt die meisten anderen Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-209">`NoStore` overrides most of the other properties.</span></span> <span data-ttu-id="bf2c5-210">Wenn diese Eigenschaft festgelegt wird, um `true`, `Cache-Control` Header wird festgelegt, um `no-store`.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-210">When this property is set to `true`, the `Cache-Control` header is set to `no-store`.</span></span> <span data-ttu-id="bf2c5-211">Wenn `Location` festgelegt ist, um `None`:</span><span class="sxs-lookup"><span data-stu-id="bf2c5-211">If `Location` is set to `None`:</span></span>

* <span data-ttu-id="bf2c5-212">Für `Cache-Control` ist `no-store,no-cache` festgelegt.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-212">`Cache-Control` is set to `no-store,no-cache`.</span></span>
* <span data-ttu-id="bf2c5-213">Für `Pragma` ist `no-cache` festgelegt.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-213">`Pragma` is set to `no-cache`.</span></span>

<span data-ttu-id="bf2c5-214">Wenn `NoStore` ist `false` und `Location` ist `None`, `Cache-Control` und `Pragma` festgelegt `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-214">If `NoStore` is `false` and `Location` is `None`, `Cache-Control` and `Pragma` are set to `no-cache`.</span></span>

<span data-ttu-id="bf2c5-215">Legen Sie Sie in der Regel `NoStore` auf `true` auf Fehlerseiten.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-215">You typically set `NoStore` to `true` on error pages.</span></span> <span data-ttu-id="bf2c5-216">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="bf2c5-216">For example:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

<span data-ttu-id="bf2c5-217">Daraus ergibt sich die folgenden Header:</span><span class="sxs-lookup"><span data-stu-id="bf2c5-217">This results in the following headers:</span></span>

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a><span data-ttu-id="bf2c5-218">Speicherort und die Dauer</span><span class="sxs-lookup"><span data-stu-id="bf2c5-218">Location and Duration</span></span>

<span data-ttu-id="bf2c5-219">So aktivieren Sie das Zwischenspeichern, `Duration` muss auf einen positiven Wert festgelegt werden und `Location` muss entweder `Any` (Standard) oder `Client`.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-219">To enable caching, `Duration` must be set to a positive value and `Location` must be either `Any` (the default) or `Client`.</span></span> <span data-ttu-id="bf2c5-220">In diesem Fall die `Cache-Control` Header festgelegt ist, auf den Speicherortwert, gefolgt von den `max-age` der Antwort.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-220">In this case, the `Cache-Control` header is set to the location value followed by the `max-age` of the response.</span></span>

> [!NOTE]
> <span data-ttu-id="bf2c5-221">`Location`die Optionen der `Any` und `Client` übersetzen in `Cache-Control` Headerwerte `public` und `private`bzw.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-221">`Location`'s options of `Any` and `Client` translate into `Cache-Control` header values of `public` and `private`, respectively.</span></span> <span data-ttu-id="bf2c5-222">Wie bereits erwähnt, festlegen `Location` auf `None` legt `Cache-Control` und `Pragma` Header `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-222">As noted previously, setting `Location` to `None` sets both `Cache-Control` and `Pragma` headers to `no-cache`.</span></span>

<span data-ttu-id="bf2c5-223">Im folgenden ein Beispiel für die Header erstellt wird, durch Festlegen von `Duration` und lassen die Standardeinstellung `Location` Wert:</span><span class="sxs-lookup"><span data-stu-id="bf2c5-223">Below is an example showing the headers produced by setting `Duration` and leaving the default `Location` value:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

<span data-ttu-id="bf2c5-224">Dies erzeugt die folgende Kopfzeile:</span><span class="sxs-lookup"><span data-stu-id="bf2c5-224">This produces the following header:</span></span>

```
Cache-Control: public,max-age=60
```

### <a name="cache-profiles"></a><span data-ttu-id="bf2c5-225">Cacheprofile</span><span class="sxs-lookup"><span data-stu-id="bf2c5-225">Cache profiles</span></span>

<span data-ttu-id="bf2c5-226">Anstelle von duplizieren `ResponseCache` Einstellungen auf viele Aktion Attributen für Controller, CacheProfile können als Optionen konfiguriert werden, beim Einrichten von MVC in der `ConfigureServices` Methode in `Startup`.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-226">Instead of duplicating `ResponseCache` settings on many controller action attributes, cache profiles can be configured as options when setting up MVC in the `ConfigureServices` method in `Startup`.</span></span> <span data-ttu-id="bf2c5-227">In einer referenzierten Cacheprofil, gefundenen Werte dienen als die standardmäßig von der `ResponseCache` Attribut, und werden nach beliebigen Eigenschaften für das Attribut angegebenen außer Kraft gesetzt.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-227">Values found in a referenced cache profile are used as the defaults by the `ResponseCache` attribute and are overridden by any properties specified on the attribute.</span></span>

<span data-ttu-id="bf2c5-228">Einrichten von ein Cacheprofil:</span><span class="sxs-lookup"><span data-stu-id="bf2c5-228">Setting up a cache profile:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="bf2c5-229">Verweisen auf ein Cacheprofil aus:</span><span class="sxs-lookup"><span data-stu-id="bf2c5-229">Referencing a cache profile:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

<span data-ttu-id="bf2c5-230">Die `ResponseCache` Attribut kann auf Aktionen (Methoden) und auf Domänencontrollern (Klassen) angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-230">The `ResponseCache` attribute can be applied both to actions (methods) and controllers (classes).</span></span> <span data-ttu-id="bf2c5-231">Methodenebene Attribute überschreiben die Attribute auf Klassenebene festgelegten Einstellungen.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-231">Method-level attributes override the settings specified in class-level attributes.</span></span>

<span data-ttu-id="bf2c5-232">Im obigen Beispiel gibt ein Attribut auf Klassenebene während einer Methodenebene-Attribut ein Cacheprofil mit einer Dauer von 60 Sekunden festgelegt verweist auf eine Dauer von 30 Sekunden an.</span><span class="sxs-lookup"><span data-stu-id="bf2c5-232">In the above example, a class-level attribute specifies a duration of 30 seconds, while a method-level attribute references a cache profile with a duration set to 60 seconds.</span></span>

<span data-ttu-id="bf2c5-233">Die resultierende Header:</span><span class="sxs-lookup"><span data-stu-id="bf2c5-233">The resulting header:</span></span>

```
Cache-Control: public,max-age=60
```

## <a name="additional-resources"></a><span data-ttu-id="bf2c5-234">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="bf2c5-234">Additional resources</span></span>

* [<span data-ttu-id="bf2c5-235">Das Speichern von Antworten in Caches</span><span class="sxs-lookup"><span data-stu-id="bf2c5-235">Storing Responses in Caches</span></span>](https://tools.ietf.org/html/rfc7234#section-3)
* [<span data-ttu-id="bf2c5-236">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="bf2c5-236">Cache-Control</span></span>](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* [<span data-ttu-id="bf2c5-237">Zwischenspeichern in Speicher</span><span class="sxs-lookup"><span data-stu-id="bf2c5-237">Cache in-memory</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="bf2c5-238">Arbeiten mit einem verteilten Cache</span><span class="sxs-lookup"><span data-stu-id="bf2c5-238">Work with a distributed cache</span></span>](xref:performance/caching/distributed)
* [<span data-ttu-id="bf2c5-239">Erkennen von Änderungen mit Änderungstoken</span><span class="sxs-lookup"><span data-stu-id="bf2c5-239">Detect changes with change tokens</span></span>](xref:fundamentals/primitives/change-tokens)
* [<span data-ttu-id="bf2c5-240">Antworten zwischenspeichernde Middleware</span><span class="sxs-lookup"><span data-stu-id="bf2c5-240">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="bf2c5-241">Cache-Taghilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="bf2c5-241">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="bf2c5-242">Taghilfsprogramm für verteilten Cache</span><span class="sxs-lookup"><span data-stu-id="bf2c5-242">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
