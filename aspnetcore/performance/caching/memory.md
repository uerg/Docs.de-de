---
title: Cache im Arbeitsspeicher in ASP.NET Core
author: rick-anderson
description: Informationen Sie zum Zwischenspeichern von Daten im Arbeitsspeicher in ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 12/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: performance/caching/memory
ms.openlocfilehash: eca6610caf4e0a654c9a31f89a42e2ac82e94d23
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734483"
---
# <a name="cache-in-memory-in-aspnet-core"></a><span data-ttu-id="18745-103">Cache im Arbeitsspeicher in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="18745-103">Cache in-memory in ASP.NET Core</span></span>

<span data-ttu-id="18745-104">Durch [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), und [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="18745-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="18745-105">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="18745-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="caching-basics"></a><span data-ttu-id="18745-106">Caching-Grundlagen</span><span class="sxs-lookup"><span data-stu-id="18745-106">Caching basics</span></span>

<span data-ttu-id="18745-107">Caching kann deutlich die Leistung und Skalierbarkeit einer App verbessert werden, verringern den Arbeitsaufwand beim Generieren der Inhalte.</span><span class="sxs-lookup"><span data-stu-id="18745-107">Caching can significantly improve the performance and scalability of an app by reducing the work required to generate content.</span></span> <span data-ttu-id="18745-108">Zwischenspeichern funktioniert am besten mit Daten, die sich selten ändern.</span><span class="sxs-lookup"><span data-stu-id="18745-108">Caching works best with data that changes infrequently.</span></span> <span data-ttu-id="18745-109">Caching stellt eine Kopie der Daten, die viel zurückgegeben werden, können aus der Originalquelle schneller.</span><span class="sxs-lookup"><span data-stu-id="18745-109">Caching makes a copy of data that can be returned much faster than from the original source.</span></span> <span data-ttu-id="18745-110">Schreiben und Testen Sie Ihre app niemals hängen mit zwischengespeicherten Daten.</span><span class="sxs-lookup"><span data-stu-id="18745-110">You should write and test your app to never depend on cached data.</span></span>

<span data-ttu-id="18745-111">ASP.NET Core unterstützt mehrere unterschiedliche Caches gelten.</span><span class="sxs-lookup"><span data-stu-id="18745-111">ASP.NET Core supports several different caches.</span></span> <span data-ttu-id="18745-112">Die einfachste Cache basiert auf der [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), der einen Cache im Arbeitsspeicher des Webservers gespeichert darstellt.</span><span class="sxs-lookup"><span data-stu-id="18745-112">The simplest cache is based on the [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), which represents a cache stored in the memory of the web server.</span></span> <span data-ttu-id="18745-113">Apps, die auf eine Serverfarm mit mehreren Servern ausgeführt werden sollten sicherstellen, dass Sitzungen Kurznotiz bei Verwendung von in-Memory-Caches.</span><span class="sxs-lookup"><span data-stu-id="18745-113">Apps which run on a server farm of multiple servers should ensure that sessions are sticky when using the in-memory cache.</span></span> <span data-ttu-id="18745-114">Persistente Sitzungen stellen Sie sicher, dass nachfolgende Anforderungen von einem Client, die alle auf denselben Server wechseln.</span><span class="sxs-lookup"><span data-stu-id="18745-114">Sticky sessions ensure that subsequent requests from a client all go to the same server.</span></span> <span data-ttu-id="18745-115">Z. B. Azure-Web-apps verwenden [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) auf alle nachfolgenden Anforderungen mit dem gleichen Server weiterzuleiten.</span><span class="sxs-lookup"><span data-stu-id="18745-115">For example, Azure Web apps use [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) to route all subsequent requests to the same server.</span></span>

<span data-ttu-id="18745-116">Nicht persistente Sitzungen in einer Webfarm erfordert eine [verteilte Caches](distributed.md) Cache Konsistenzprobleme zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="18745-116">Non-sticky sessions in a web farm require a [distributed cache](distributed.md) to avoid cache consistency problems.</span></span> <span data-ttu-id="18745-117">Bei einigen apps kann ein verteilter Cache höher Dezentrales Skalieren als ein in-Memory-Cache unterstützen.</span><span class="sxs-lookup"><span data-stu-id="18745-117">For some apps, a distributed cache can support higher scale out than an in-memory cache.</span></span> <span data-ttu-id="18745-118">Mit einem verteilten Cache entlastet den Cachespeicher zu einem externen Prozess.</span><span class="sxs-lookup"><span data-stu-id="18745-118">Using a distributed cache offloads the cache memory to an external process.</span></span>

<span data-ttu-id="18745-119">Die `IMemoryCache` Cache entfernen Einträge im Cache nicht genügend Arbeitsspeicher vorhanden, es sei denn, die [Zwischenspeichern Priorität](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) festgelegt ist, um `CacheItemPriority.NeverRemove`.</span><span class="sxs-lookup"><span data-stu-id="18745-119">The `IMemoryCache` cache will evict cache entries under memory pressure unless the [cache priority](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) is set to `CacheItemPriority.NeverRemove`.</span></span> <span data-ttu-id="18745-120">Sie können festlegen, die `CacheItemPriority` , passen Sie die Priorität mit dem Cache Elemente ungenügendem Arbeitsspeicher entfernt.</span><span class="sxs-lookup"><span data-stu-id="18745-120">You can set the `CacheItemPriority` to adjust the priority with which the cache evicts items under memory pressure.</span></span>

<span data-ttu-id="18745-121">Die in-Memory-Cache kann jedes Objekt speichern. die Schnittstelle für verteilte Caches ist auf `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="18745-121">The in-memory cache can store any object; the distributed cache interface is limited to `byte[]`.</span></span>

## <a name="using-imemorycache"></a><span data-ttu-id="18745-122">Verwenden von IMemoryCache</span><span class="sxs-lookup"><span data-stu-id="18745-122">Using IMemoryCache</span></span>

<span data-ttu-id="18745-123">In-Memory-caching ist ein *Service* verwiesen, wird die app mithilfe [Abhängigkeitsinjektion](../../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="18745-123">In-memory caching is a *service* that's referenced from your app using [Dependency Injection](../../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="18745-124">Rufen Sie `AddMemoryCache` in `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="18745-124">Call `AddMemoryCache` in `ConfigureServices`:</span></span>

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

<span data-ttu-id="18745-125">Anfordern der `IMemoryCache` Instanz im Konstruktor:</span><span class="sxs-lookup"><span data-stu-id="18745-125">Request the `IMemoryCache` instance in the constructor:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor)]

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="18745-126">`IMemoryCache` erfordert die NuGet-Paket [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).</span><span class="sxs-lookup"><span data-stu-id="18745-126">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="18745-127">`IMemoryCache` erfordert die NuGet-Paket [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), ist verfügbar in der [Microsoft.AspNetCore.All Metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="18745-127">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), which is avaiable in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="18745-128">`IMemoryCache` erfordert die NuGet-Paket [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), ist verfügbar in der [Microsoft.AspNetCore.App Metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="18745-128">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), which is avaiable in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="18745-129">Der folgende code verwendet [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) zum Überprüfen, ob eine Uhrzeit im Cache befindet.</span><span class="sxs-lookup"><span data-stu-id="18745-129">The following code uses [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) to check if a time is in the cache.</span></span> <span data-ttu-id="18745-130">Wenn Sie eine Zeit zwischengespeichert ist, wird ein neuer Eintrag erstellt und in den Cache mit hinzugefügt [festgelegt](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).</span><span class="sxs-lookup"><span data-stu-id="18745-130">If a time isn't cached, a new entry is created and added to the cache with [Set](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="18745-131">Die aktuelle Uhrzeit und den Cachezeit werden angezeigt:</span><span class="sxs-lookup"><span data-stu-id="18745-131">The current time and the cached time are displayed:</span></span>

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

<span data-ttu-id="18745-132">Die zwischengespeicherten `DateTime` Wert im Cache bleibt, während auf Anforderungen innerhalb des Zeitlimits (und keine Entfernung aufgrund von ungenügendem Arbeitsspeicher) vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="18745-132">The cached `DateTime` value remains in the cache while there are requests within the timeout period (and no eviction due to memory pressure).</span></span> <span data-ttu-id="18745-133">Die folgende Abbildung zeigt die aktuelle Uhrzeit und eine frühere Zeit, die aus dem Cache abgerufen werden:</span><span class="sxs-lookup"><span data-stu-id="18745-133">The following image shows the current time and an older time retrieved from the cache:</span></span>

![Index-Ansicht mit zwei unterschiedlichen Zeitpunkten angezeigt](memory/_static/time.png)

<span data-ttu-id="18745-135">Der folgende code verwendet [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) und [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) zum Zwischenspeichern von Daten.</span><span class="sxs-lookup"><span data-stu-id="18745-135">The following code uses [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) and [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) to cache data.</span></span> 

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

<span data-ttu-id="18745-136">Der folgende code ruft [abrufen](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) die Cachezeit abgerufen:</span><span class="sxs-lookup"><span data-stu-id="18745-136">The following code calls [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) to fetch the cached time:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

<span data-ttu-id="18745-137">Finden Sie unter [IMemoryCache Methoden](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) und [CacheExtensions Methoden](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) eine Beschreibung der Cache-Methoden.</span><span class="sxs-lookup"><span data-stu-id="18745-137">See [IMemoryCache methods](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) and [CacheExtensions methods](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) for a description of the cache methods.</span></span>

## <a name="using-memorycacheentryoptions"></a><span data-ttu-id="18745-138">Verwenden von MemoryCacheEntryOptions</span><span class="sxs-lookup"><span data-stu-id="18745-138">Using MemoryCacheEntryOptions</span></span>

<span data-ttu-id="18745-139">Im folgenden Beispiel:</span><span class="sxs-lookup"><span data-stu-id="18745-139">The following sample:</span></span>

- <span data-ttu-id="18745-140">Legt die absolute Ablaufzeit.</span><span class="sxs-lookup"><span data-stu-id="18745-140">Sets the absolute expiration time.</span></span> <span data-ttu-id="18745-141">Dies ist die maximale Zeit, die der Eintrag zwischengespeichert werden kann und verhindert, dass das Element immer veraltet, wenn gleitende Ablauf fortlaufend verlängert wird.</span><span class="sxs-lookup"><span data-stu-id="18745-141">This is the maximum time the entry can be cached and prevents the item from becoming too stale when the sliding expiration is continuously renewed.</span></span>
- <span data-ttu-id="18745-142">Legt eine gleitende Ablaufzeit fest.</span><span class="sxs-lookup"><span data-stu-id="18745-142">Sets a sliding expiration time.</span></span> <span data-ttu-id="18745-143">Anforderungen, die Zugriff auf dieses zwischengespeicherte Element werden die gleitende Ablaufdauer zurückgesetzt.</span><span class="sxs-lookup"><span data-stu-id="18745-143">Requests that access this cached item will reset the sliding expiration clock.</span></span>
- <span data-ttu-id="18745-144">Legt die Cachepriorität auf `CacheItemPriority.NeverRemove`.</span><span class="sxs-lookup"><span data-stu-id="18745-144">Sets the cache priority to `CacheItemPriority.NeverRemove`.</span></span>
- <span data-ttu-id="18745-145">Legt eine [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) , werden aufgerufen, nachdem der Eintrag aus dem Cache entfernt wird.</span><span class="sxs-lookup"><span data-stu-id="18745-145">Sets a [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) that will be called after the entry is evicted from the cache.</span></span> <span data-ttu-id="18745-146">Der Rückruf wird auf einem anderen Thread aus dem Code ausgeführt, die das Element aus dem Cache entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="18745-146">The callback is run on a different thread from the code that removes the item from the cache.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

## <a name="cache-dependencies"></a><span data-ttu-id="18745-147">Cache-Abhängigkeiten</span><span class="sxs-lookup"><span data-stu-id="18745-147">Cache dependencies</span></span>

<span data-ttu-id="18745-148">Das folgende Beispiel zeigt, wie Sie einen Eintrag im Cache ablaufen, wenn ein abhängiger Eintrag abläuft.</span><span class="sxs-lookup"><span data-stu-id="18745-148">The following sample shows how to expire a cache entry if a dependent entry expires.</span></span> <span data-ttu-id="18745-149">Ein `CancellationChangeToken` das zwischengespeicherte Element hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="18745-149">A `CancellationChangeToken` is added to the cached item.</span></span> <span data-ttu-id="18745-150">Wenn `Cancel` aufgerufen wird, auf die `CancellationTokenSource`, beide Einträge im Cache entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="18745-150">When `Cancel` is called on the `CancellationTokenSource`, both cache entries are evicted.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

<span data-ttu-id="18745-151">Mit einem `CancellationTokenSource` ermöglicht mehrere Cacheeinträge als Gruppe entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="18745-151">Using a `CancellationTokenSource` allows multiple cache entries to be evicted as a group.</span></span> <span data-ttu-id="18745-152">Mit der `using` Muster im obigen Code, der Einträge im Cache erstellt innerhalb der `using` Block Trigger und ablaufeinstellungen erben.</span><span class="sxs-lookup"><span data-stu-id="18745-152">With the `using` pattern in the code above, cache entries created inside the `using` block will inherit triggers and expiration settings.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="18745-153">Zusätzliche Hinweise</span><span class="sxs-lookup"><span data-stu-id="18745-153">Additional notes</span></span>

- <span data-ttu-id="18745-154">Wenn einen Rückruf verwenden Sie ein Cacheelement neu auffüllen:</span><span class="sxs-lookup"><span data-stu-id="18745-154">When using a callback to repopulate a cache item:</span></span>

  - <span data-ttu-id="18745-155">Mehrere Anforderungen erhalten die zwischengespeicherte Schlüsselwert leer, da der Rückruf abgeschlossen noch nicht.</span><span class="sxs-lookup"><span data-stu-id="18745-155">Multiple requests can find the cached key value empty because the callback hasn't completed.</span></span>
  - <span data-ttu-id="18745-156">Dies kann in mehreren Threads, die für das streckungsschema an das zwischengespeicherte Element führen.</span><span class="sxs-lookup"><span data-stu-id="18745-156">This can result in several threads repopulating the cached item.</span></span>

- <span data-ttu-id="18745-157">Wenn ein Cacheeintrag verwendet wird, um eine andere zu erstellen, kopiert das untergeordnete Element, des übergeordnete Eintrags Ablauf-Token und zeitbasierte ablaufeinstellungen.</span><span class="sxs-lookup"><span data-stu-id="18745-157">When one cache entry is used to create another, the child copies the parent entry's expiration tokens and time-based expiration settings.</span></span> <span data-ttu-id="18745-158">Das untergeordnete Element ist nicht abgelaufene durch manuelles Entfernen oder aktualisieren, der des übergeordnete Eintrags.</span><span class="sxs-lookup"><span data-stu-id="18745-158">The child isn't expired by manual removal or updating of the parent entry.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="18745-159">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="18745-159">Additional resources</span></span>

* [<span data-ttu-id="18745-160">Arbeiten mit einem verteilten Cache</span><span class="sxs-lookup"><span data-stu-id="18745-160">Work with a distributed cache</span></span>](xref:performance/caching/distributed)
* [<span data-ttu-id="18745-161">Erkennen von Änderungen mit Änderungstoken</span><span class="sxs-lookup"><span data-stu-id="18745-161">Detect changes with change tokens</span></span>](xref:fundamentals/primitives/change-tokens)
* [<span data-ttu-id="18745-162">Zwischenspeichern von Antworten</span><span class="sxs-lookup"><span data-stu-id="18745-162">Response caching</span></span>](xref:performance/caching/response)
* [<span data-ttu-id="18745-163">Antworten zwischenspeichernde Middleware</span><span class="sxs-lookup"><span data-stu-id="18745-163">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="18745-164">Cache-Taghilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="18745-164">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="18745-165">Taghilfsprogramm für verteilten Cache</span><span class="sxs-lookup"><span data-stu-id="18745-165">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
