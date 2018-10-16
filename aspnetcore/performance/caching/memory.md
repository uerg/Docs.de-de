---
title: Zwischenspeichern in Speicher in ASP.NET Core
author: rick-anderson
description: Erfahren Sie, wie Sie Daten im Arbeitsspeicher in ASP.NET Core zwischenspeichern können.
ms.author: riande
ms.custom: mvc
ms.date: 09/15/2018
uid: performance/caching/memory
ms.openlocfilehash: 960aa18f9d14f633118ccd716201e61464085c05
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325925"
---
# <a name="cache-in-memory-in-aspnet-core"></a><span data-ttu-id="55182-103">Zwischenspeichern in Speicher in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="55182-103">Cache in-memory in ASP.NET Core</span></span>

<span data-ttu-id="55182-104">Durch [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), und [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="55182-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="55182-105">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="55182-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="caching-basics"></a><span data-ttu-id="55182-106">Grundlagen der Zwischenspeicherung</span><span class="sxs-lookup"><span data-stu-id="55182-106">Caching basics</span></span>

<span data-ttu-id="55182-107">Caching kann erheblich die Leistung und Skalierbarkeit einer App verbessert werden, verringern den erforderlichen Aufwand zum Generieren von Inhalt.</span><span class="sxs-lookup"><span data-stu-id="55182-107">Caching can significantly improve the performance and scalability of an app by reducing the work required to generate content.</span></span> <span data-ttu-id="55182-108">Funktionsweise der Zwischenspeicherung am stärksten bei den Daten, die nur selten ändern.</span><span class="sxs-lookup"><span data-stu-id="55182-108">Caching works best with data that changes infrequently.</span></span> <span data-ttu-id="55182-109">Caching stellt eine Kopie der Daten, die viel zurückgegeben werden, können aus der Originalquelle schneller.</span><span class="sxs-lookup"><span data-stu-id="55182-109">Caching makes a copy of data that can be returned much faster than from the original source.</span></span> <span data-ttu-id="55182-110">Sie schreiben und Testen Sie Ihre app so, dass keine zwischengespeicherten Daten abhängig.</span><span class="sxs-lookup"><span data-stu-id="55182-110">You should write and test your app to never depend on cached data.</span></span>

<span data-ttu-id="55182-111">ASP.NET Core unterstützt mehrere unterschiedliche Caches gelten.</span><span class="sxs-lookup"><span data-stu-id="55182-111">ASP.NET Core supports several different caches.</span></span> <span data-ttu-id="55182-112">Die einfachste Cache basiert auf der [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), steht für einen Cache im Arbeitsspeicher des Webservers gespeichert.</span><span class="sxs-lookup"><span data-stu-id="55182-112">The simplest cache is based on the [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), which represents a cache stored in the memory of the web server.</span></span> <span data-ttu-id="55182-113">Apps, die auf eine Serverfarm mit mehreren Servern ausgeführt werden sollten sicherstellen, dass Sitzungen Kurznotiz bei Verwendung von in-Memory-Cache.</span><span class="sxs-lookup"><span data-stu-id="55182-113">Apps which run on a server farm of multiple servers should ensure that sessions are sticky when using the in-memory cache.</span></span> <span data-ttu-id="55182-114">Persistente Sitzungen stellen Sie sicher, dass nachfolgende Anforderungen von einem Client, die alle auf dem gleichen Server erfolgen.</span><span class="sxs-lookup"><span data-stu-id="55182-114">Sticky sessions ensure that subsequent requests from a client all go to the same server.</span></span> <span data-ttu-id="55182-115">Z. B. Azure-Web-apps verwenden [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR), alle nachfolgende Anforderungen auf denselben Server weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="55182-115">For example, Azure Web apps use [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) to route all subsequent requests to the same server.</span></span>

<span data-ttu-id="55182-116">Nicht-sticky-Sitzungen in einer Webfarm erfordern eine [verteilter Cache](distributed.md) , Cache Konsistenzprobleme zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="55182-116">Non-sticky sessions in a web farm require a [distributed cache](distributed.md) to avoid cache consistency problems.</span></span> <span data-ttu-id="55182-117">Bei einigen apps kann ein verteilter Cache höheren Skalierung als eine in-Memory-Cache unterstützt.</span><span class="sxs-lookup"><span data-stu-id="55182-117">For some apps, a distributed cache can support higher scale-out than an in-memory cache.</span></span> <span data-ttu-id="55182-118">Mit einem verteilten Cache lädt die Cache-Speicher an einen externen Prozess ab.</span><span class="sxs-lookup"><span data-stu-id="55182-118">Using a distributed cache offloads the cache memory to an external process.</span></span>

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="55182-119">Die `IMemoryCache` Cache-Cacheeinträge Einträge im Cache nicht genügend Arbeitsspeicher vorhanden, es sei denn, die [Zwischenspeichern Priorität](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) nastaven NA hodnotu `CacheItemPriority.NeverRemove`.</span><span class="sxs-lookup"><span data-stu-id="55182-119">The `IMemoryCache` cache will evict cache entries under memory pressure unless the [cache priority](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) is set to `CacheItemPriority.NeverRemove`.</span></span> <span data-ttu-id="55182-120">Sie können festlegen, die `CacheItemPriority` die Priorität anpassen, mit dem Cache Elemente nicht genügend Arbeitsspeicher entfernt.</span><span class="sxs-lookup"><span data-stu-id="55182-120">You can set the `CacheItemPriority` to adjust the priority with which the cache evicts items under memory pressure.</span></span>

::: moniker-end

<span data-ttu-id="55182-121">In-Memory-Cache kann jedes beliebige Objekt speichern. die Schnittstelle für verteilte Caches ist auf `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="55182-121">The in-memory cache can store any object; the distributed cache interface is limited to `byte[]`.</span></span>

## <a name="systemruntimecachingmemorycache"></a><span data-ttu-id="55182-122">System.Runtime.Caching/MemoryCache</span><span class="sxs-lookup"><span data-stu-id="55182-122">System.Runtime.Caching/MemoryCache</span></span>

<span data-ttu-id="55182-123"><xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache> ([NuGet-Paket](https://www.nuget.org/packages/System.Runtime.Caching/)) mit verwendet werden kann:</span><span class="sxs-lookup"><span data-stu-id="55182-123"><xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache> ([NuGet package](https://www.nuget.org/packages/System.Runtime.Caching/)) can be used with:</span></span>

* <span data-ttu-id="55182-124">.NET Standard 2.0 oder höher.</span><span class="sxs-lookup"><span data-stu-id="55182-124">.NET Standard 2.0 or later.</span></span>
* <span data-ttu-id="55182-125">Alle [.NET Implementierung](/dotnet/standard/net-standard#net-implementation-support) , die auf .NET Standard 2.0 oder höher.</span><span class="sxs-lookup"><span data-stu-id="55182-125">Any [.NET implementation](/dotnet/standard/net-standard#net-implementation-support) that targets .NET Standard 2.0 or later.</span></span> <span data-ttu-id="55182-126">Beispiel: ASP.NET Core 2.0 oder höher.</span><span class="sxs-lookup"><span data-stu-id="55182-126">For example, ASP.NET Core 2.0 or later.</span></span>
* <span data-ttu-id="55182-127">.NET Framework 4.5 oder höher.</span><span class="sxs-lookup"><span data-stu-id="55182-127">.NET Framework 4.5 or later.</span></span>

<span data-ttu-id="55182-128">["Microsoft.Extensions.Caching.Memory"](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/) / `IMemoryCache` (in diesem Thema beschrieben) wird empfohlen, `System.Runtime.Caching` / `MemoryCache` da es besser in ASP.NET Core integriert ist.</span><span class="sxs-lookup"><span data-stu-id="55182-128">[Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)/`IMemoryCache` (described in this topic) is recommended over `System.Runtime.Caching`/`MemoryCache` because it's better integrated into ASP.NET Core.</span></span> <span data-ttu-id="55182-129">Z. B. `IMemoryCache` funktioniert nativ in ASP.NET Core [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="55182-129">For example, `IMemoryCache` works natively with ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="55182-130">Verwendung `System.Runtime.Caching` / `MemoryCache` als Brücke Kompatibilität beim Portieren von Code von ASP.NET 4.x zu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="55182-130">Use `System.Runtime.Caching`/`MemoryCache` as a compatibility bridge when porting code from ASP.NET 4.x to ASP.NET Core.</span></span>

## <a name="cache-guidelines"></a><span data-ttu-id="55182-131">Cache-Richtlinien</span><span class="sxs-lookup"><span data-stu-id="55182-131">Cache guidelines</span></span>

* <span data-ttu-id="55182-132">Code sollte immer eine Fallbackoption zum Abrufen von Daten aufweisen und **nicht** richten sich nach der ein zwischengespeicherter Wert, der nicht verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="55182-132">Code should always have a fallback option to fetch data and **not** depend on a cached value being available.</span></span>
* <span data-ttu-id="55182-133">Der Cache verwendet eine wertvolle Ressource, die Arbeitsspeicher.</span><span class="sxs-lookup"><span data-stu-id="55182-133">The cache uses a scarce resource, memory.</span></span> <span data-ttu-id="55182-134">Cache Wachstum zu beschränken:</span><span class="sxs-lookup"><span data-stu-id="55182-134">Limit cache growth:</span></span>
  * <span data-ttu-id="55182-135">Führen Sie **nicht** externe Eingaben wie den Zugriffsschlüssel für den Cache verwenden.</span><span class="sxs-lookup"><span data-stu-id="55182-135">Do **not** use external input as cache keys.</span></span>
  * <span data-ttu-id="55182-136">Verwenden Sie Ablaufzeiten, um den Cache verursachte Wachstum zu begrenzen.</span><span class="sxs-lookup"><span data-stu-id="55182-136">Use expirations to limit cache growth.</span></span>
  * [<span data-ttu-id="55182-137">Verwenden Sie SetSize, Größe und SizeLimit Beschränken der Größe des Caches</span><span class="sxs-lookup"><span data-stu-id="55182-137">Use SetSize, Size, and SizeLimit to limit cache size</span></span>](#use-setsize-size-and-sizelimit-to-limit-cache-size)

## <a name="using-imemorycache"></a><span data-ttu-id="55182-138">Verwenden von IMemoryCache</span><span class="sxs-lookup"><span data-stu-id="55182-138">Using IMemoryCache</span></span>

<span data-ttu-id="55182-139">In-Memory-caching ist ein *Service* auf den verwiesen wird über Ihre app mit [Dependency Injection](../../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="55182-139">In-memory caching is a *service* that's referenced from your app using [Dependency Injection](../../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="55182-140">Rufen Sie `AddMemoryCache` in `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="55182-140">Call `AddMemoryCache` in `ConfigureServices`:</span></span>

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

<span data-ttu-id="55182-141">Anfordern der `IMemoryCache` -Instanz in den Konstruktor:</span><span class="sxs-lookup"><span data-stu-id="55182-141">Request the `IMemoryCache` instance in the constructor:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor)]

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="55182-142">`IMemoryCache` NuGet-Paket erfordert ["Microsoft.Extensions.Caching.Memory"](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).</span><span class="sxs-lookup"><span data-stu-id="55182-142">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="55182-143">`IMemoryCache` NuGet-Paket erfordert ["Microsoft.Extensions.Caching.Memory"](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), steht in der die [metapaket "Microsoft.aspnetcore.All"](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="55182-143">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), which is available in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="55182-144">`IMemoryCache` NuGet-Paket erfordert ["Microsoft.Extensions.Caching.Memory"](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), steht in der die [Microsoft.AspNetCore.App metapaket](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="55182-144">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="55182-145">Der folgende code verwendet [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) zu überprüfen, ob eine Zeit im Cache vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="55182-145">The following code uses [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) to check if a time is in the cache.</span></span> <span data-ttu-id="55182-146">Wenn Sie eine Zeit nicht zwischengespeichert wird, ein neuer Eintrag erstellt und in den Cache mit hinzugefügt [festgelegt](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).</span><span class="sxs-lookup"><span data-stu-id="55182-146">If a time isn't cached, a new entry is created and added to the cache with [Set](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).</span></span>

[!code-csharp [](memory/sample/WebCache/CacheKeys.cs)]

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="55182-147">Die aktuelle Uhrzeit und die Cachezeit werden angezeigt:</span><span class="sxs-lookup"><span data-stu-id="55182-147">The current time and the cached time are displayed:</span></span>

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

<span data-ttu-id="55182-148">Die zwischengespeicherten `DateTime` Wert im Cache bleibt, während Anforderungen innerhalb des Zeitlimits (und keine Entfernung aufgrund von ungenügendem Arbeitsspeicher) vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="55182-148">The cached `DateTime` value remains in the cache while there are requests within the timeout period (and no eviction due to memory pressure).</span></span> <span data-ttu-id="55182-149">Die folgende Abbildung zeigt die aktuelle Uhrzeit und einem älteren Zeitpunkt aus dem Cache abgerufen werden:</span><span class="sxs-lookup"><span data-stu-id="55182-149">The following image shows the current time and an older time retrieved from the cache:</span></span>

![Ansicht "Index" mit zwei verschiedenen Uhrzeiten angezeigt](memory/_static/time.png)

<span data-ttu-id="55182-151">Der folgende code verwendet [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) und [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) zum Zwischenspeichern von Daten.</span><span class="sxs-lookup"><span data-stu-id="55182-151">The following code uses [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) and [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) to cache data.</span></span> 

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

<span data-ttu-id="55182-152">Der folgende code ruft [erhalten](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) der Cachezeit abgerufen:</span><span class="sxs-lookup"><span data-stu-id="55182-152">The following code calls [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) to fetch the cached time:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

<span data-ttu-id="55182-153">Finden Sie unter [IMemoryCache Methoden](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) und [CacheExtensions Methoden](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) eine Beschreibung der Cache-Methoden.</span><span class="sxs-lookup"><span data-stu-id="55182-153">See [IMemoryCache methods](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) and [CacheExtensions methods](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) for a description of the cache methods.</span></span>

## <a name="memorycacheentryoptions"></a><span data-ttu-id="55182-154">MemoryCacheEntryOptions</span><span class="sxs-lookup"><span data-stu-id="55182-154">MemoryCacheEntryOptions</span></span>

<span data-ttu-id="55182-155">Im folgenden Beispiel:</span><span class="sxs-lookup"><span data-stu-id="55182-155">The following sample:</span></span>

- <span data-ttu-id="55182-156">Legt die absolute Ablaufzeit fest.</span><span class="sxs-lookup"><span data-stu-id="55182-156">Sets the absolute expiration time.</span></span> <span data-ttu-id="55182-157">Dies ist die maximale Zeit, die der Eintrag zwischengespeichert werden kann, und verhindert, dass das Element zunehmend veraltet, wenn die gleitende Ablaufzeit ständig erneuert wird.</span><span class="sxs-lookup"><span data-stu-id="55182-157">This is the maximum time the entry can be cached and prevents the item from becoming too stale when the sliding expiration is continuously renewed.</span></span>
- <span data-ttu-id="55182-158">Legt eine gleitende Ablaufzeit fest.</span><span class="sxs-lookup"><span data-stu-id="55182-158">Sets a sliding expiration time.</span></span> <span data-ttu-id="55182-159">Anforderungen, die Zugriff auf diese zwischengespeicherte Element werden der gleitende Ablaufdauer zurückgesetzt.</span><span class="sxs-lookup"><span data-stu-id="55182-159">Requests that access this cached item will reset the sliding expiration clock.</span></span>
- <span data-ttu-id="55182-160">Legt die Cachepriorität auf `CacheItemPriority.NeverRemove`.</span><span class="sxs-lookup"><span data-stu-id="55182-160">Sets the cache priority to `CacheItemPriority.NeverRemove`.</span></span>
- <span data-ttu-id="55182-161">Legt eine [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) wird, aufgerufen werden, nachdem der Eintrag aus dem Cache entfernt wird.</span><span class="sxs-lookup"><span data-stu-id="55182-161">Sets a [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) that will be called after the entry is evicted from the cache.</span></span> <span data-ttu-id="55182-162">Der Rückruf wird auf einem anderen Thread aus dem Code ausgeführt, die das Element aus dem Cache entfernt.</span><span class="sxs-lookup"><span data-stu-id="55182-162">The callback is run on a different thread from the code that removes the item from the cache.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

::: moniker range=">= aspnetcore-2.0"

## <a name="use-setsize-size-and-sizelimit-to-limit-cache-size"></a><span data-ttu-id="55182-163">Verwenden Sie SetSize, Größe und SizeLimit Beschränken der Größe des Caches</span><span class="sxs-lookup"><span data-stu-id="55182-163">Use SetSize, Size, and SizeLimit to limit cache size</span></span>

<span data-ttu-id="55182-164">Ein `MemoryCache` -Instanz kann optional angeben und gilt eine größenbeschränkung.</span><span class="sxs-lookup"><span data-stu-id="55182-164">A `MemoryCache` instance may optionally specify and enforce a size limit.</span></span> <span data-ttu-id="55182-165">Grenzwert für die Arbeitsspeichergröße verfügt keine definierte Maßeinheit, da der Cache keinen Mechanismus, um die Größe der Einträge messen verfügt.</span><span class="sxs-lookup"><span data-stu-id="55182-165">The memory size limit does not have a defined unit of measure because the cache has no mechanism to measure the size of entries.</span></span> <span data-ttu-id="55182-166">Wenn die Cache-Speicher-größenbeschränkung festgelegt ist, müssen alle Einträge Größe angeben.</span><span class="sxs-lookup"><span data-stu-id="55182-166">If the cache memory size limit is set, all entries must specify size.</span></span> <span data-ttu-id="55182-167">Die angegebene Größe ist in Einheiten, die der Entwickler entscheidet.</span><span class="sxs-lookup"><span data-stu-id="55182-167">The size specified is in units the developer chooses.</span></span>

<span data-ttu-id="55182-168">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="55182-168">For example:</span></span>

* <span data-ttu-id="55182-169">Wenn die Zeichenfolgen in erster Linie von die Web-app Zwischenspeichern wurde, kann jeder Eintrag Cachegröße die Länge der Zeichenfolge sein.</span><span class="sxs-lookup"><span data-stu-id="55182-169">If the web app was primarily caching strings, each cache entry size could be the string length.</span></span>
* <span data-ttu-id="55182-170">Die app konnte die Größe aller Einträge geben, als 1, und die maximale Größe beträgt die Anzahl der Einträge.</span><span class="sxs-lookup"><span data-stu-id="55182-170">The app could specify the size of all entries as 1, and the size limit is the count of entries.</span></span>

<span data-ttu-id="55182-171">Der folgende Code erstellt feste Größe ohne Einheiten [MemoryCache](/dotnet/api/microsoft.extensions.caching.memory.memorycache?view=aspnetcore-2.1) zugänglich [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="55182-171">The following code creates a unitless fixed size [MemoryCache](/dotnet/api/microsoft.extensions.caching.memory.memorycache?view=aspnetcore-2.1) accessible by [dependency injection](xref:fundamentals/dependency-injection):</span></span>

[!code-csharp[](memory/sample/RPcache/Services/MyMemoryCache.cs?name=snippet)]

<span data-ttu-id="55182-172">[SizeLimit](/dotnet/api/microsoft.extensions.caching.memory.memorycacheoptions.sizelimit?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheOptions_SizeLimit) keine Einheiten.</span><span class="sxs-lookup"><span data-stu-id="55182-172">[SizeLimit](/dotnet/api/microsoft.extensions.caching.memory.memorycacheoptions.sizelimit?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheOptions_SizeLimit) does not have units.</span></span> <span data-ttu-id="55182-173">Zwischengespeicherter Einträge müssen die Größe angeben, in der alle Einheiten sie halten, am besten geeignet, wenn die Größe des Cachespeichers festgelegt wurde.</span><span class="sxs-lookup"><span data-stu-id="55182-173">Cached entries must specify size in whatever units they deem most appropriate if the cache memory size has been set.</span></span> <span data-ttu-id="55182-174">Alle Benutzer einer Cache-Instanz sollte das gleiche Einheitensystem verwenden.</span><span class="sxs-lookup"><span data-stu-id="55182-174">All users of a cache instance should use the same unit system.</span></span> <span data-ttu-id="55182-175">Ein Eintrag wird nicht zwischengespeichert werden, wenn die Summe der Größen zwischengespeicherten Eintrag angegebenen Wert überschreitet `SizeLimit`.</span><span class="sxs-lookup"><span data-stu-id="55182-175">An entry will not be cached if the sum of the cached entry sizes exceeds the value specified by `SizeLimit`.</span></span> <span data-ttu-id="55182-176">Wenn keine Cache-größenbeschränkung festgelegt ist, wird die Cachegröße, legen Sie auf den Eintrag ignoriert.</span><span class="sxs-lookup"><span data-stu-id="55182-176">If no cache size limit is set, the cache size set on the entry will be ignored.</span></span>

<span data-ttu-id="55182-177">Der folgende code registriert `MyMemoryCache` mit der [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) Container.</span><span class="sxs-lookup"><span data-stu-id="55182-177">The following code registers `MyMemoryCache` with the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span>

[!code-csharp[](memory/sample/RPcache/Startup.cs?name=snippet&highlight=5)]

<span data-ttu-id="55182-178">`MyMemoryCache` wird als ein unabhängiger Memory-Cache für Komponenten erstellt, die dieser Größe beschränkt Cache kennen und wissen, wie Sie die Größe des Eintrags entsprechend festlegen.</span><span class="sxs-lookup"><span data-stu-id="55182-178">`MyMemoryCache` is created as an independent memory cache for components that are aware of this size limited cache and know how to set cache entry size appropriately.</span></span>

<span data-ttu-id="55182-179">Der folgende code verwendet `MyMemoryCache`:</span><span class="sxs-lookup"><span data-stu-id="55182-179">The following code uses `MyMemoryCache`:</span></span>

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet)]

<span data-ttu-id="55182-180">Die Größe des Cacheeintrags festgelegt werden kann [Größe](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size) oder [SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_) Erweiterungsmethode:</span><span class="sxs-lookup"><span data-stu-id="55182-180">The size of the cache entry can be set by [Size](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size) or the [SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_) extension method:</span></span>

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet2&highlight=9,10,14,15)]

::: moniker-end

## <a name="cache-dependencies"></a><span data-ttu-id="55182-181">-Cacheabhängigkeiten</span><span class="sxs-lookup"><span data-stu-id="55182-181">Cache dependencies</span></span>

<span data-ttu-id="55182-182">Das folgende Beispiel zeigt, wie Sie einen Eintrag im Cache ablaufen, wenn ein abhängiger Eintrag abläuft.</span><span class="sxs-lookup"><span data-stu-id="55182-182">The following sample shows how to expire a cache entry if a dependent entry expires.</span></span> <span data-ttu-id="55182-183">Ein `CancellationChangeToken` wird das zwischengespeicherte Element hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="55182-183">A `CancellationChangeToken` is added to the cached item.</span></span> <span data-ttu-id="55182-184">Wenn `Cancel` aufgerufen wird, auf die `CancellationTokenSource`, beide Cacheeinträge entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="55182-184">When `Cancel` is called on the `CancellationTokenSource`, both cache entries are evicted.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

<span data-ttu-id="55182-185">Mit einem `CancellationTokenSource` können mehrere Einträge im Cache als Gruppe entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="55182-185">Using a `CancellationTokenSource` allows multiple cache entries to be evicted as a group.</span></span> <span data-ttu-id="55182-186">Mit der `using` Muster im obigen Code, Einträge im Cache erstellt wird, in der `using` Block übernimmt, Trigger und die ablaufeinstellungen.</span><span class="sxs-lookup"><span data-stu-id="55182-186">With the `using` pattern in the code above, cache entries created inside the `using` block will inherit triggers and expiration settings.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="55182-187">Zusätzliche Hinweise</span><span class="sxs-lookup"><span data-stu-id="55182-187">Additional notes</span></span>

- <span data-ttu-id="55182-188">Wenn Sie einen Rückruf verwenden ein Element im Cache neu auffüllen:</span><span class="sxs-lookup"><span data-stu-id="55182-188">When using a callback to repopulate a cache item:</span></span>

  - <span data-ttu-id="55182-189">Mehrere Anforderungen finde den zwischengespeicherten Schlüssel-Wert leer, da der Rückruf noch nicht abgeschlossen wurde.</span><span class="sxs-lookup"><span data-stu-id="55182-189">Multiple requests can find the cached key value empty because the callback hasn't completed.</span></span>
  - <span data-ttu-id="55182-190">Dies kann in mehrere Threads, die für das streckungsschema an das zwischengespeicherte Element führen.</span><span class="sxs-lookup"><span data-stu-id="55182-190">This can result in several threads repopulating the cached item.</span></span>

- <span data-ttu-id="55182-191">Wenn ein Cacheeintrag verwendet wird, um einen anderen zu erstellen, kopiert das untergeordnete Element der übergeordnete Eintrag Ablauf des Tokens und zeitbasierte ablaufeinstellungen.</span><span class="sxs-lookup"><span data-stu-id="55182-191">When one cache entry is used to create another, the child copies the parent entry's expiration tokens and time-based expiration settings.</span></span> <span data-ttu-id="55182-192">Das untergeordnete Element ist nicht abgelaufen ist, durch manuelles Entfernen oder Aktualisieren von der übergeordnete Eintrag.</span><span class="sxs-lookup"><span data-stu-id="55182-192">The child isn't expired by manual removal or updating of the parent entry.</span></span>

- <span data-ttu-id="55182-193">Verwendung [PostEvictionCallbacks](/dotnet/api/microsoft.extensions.caching.memory.icacheentry.postevictioncallbacks#Microsoft_Extensions_Caching_Memory_ICacheEntry_PostEvictionCallbacks) um die Rückrufe festzulegen, die ausgelöst wird, wenn der Cacheeintrag aus dem Cache entfernt wird.</span><span class="sxs-lookup"><span data-stu-id="55182-193">Use [PostEvictionCallbacks](/dotnet/api/microsoft.extensions.caching.memory.icacheentry.postevictioncallbacks#Microsoft_Extensions_Caching_Memory_ICacheEntry_PostEvictionCallbacks) to set the callbacks that will be fired after the cache entry is evicted from the cache.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="55182-194">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="55182-194">Additional resources</span></span>

* [<span data-ttu-id="55182-195">Arbeiten mit einem verteilten Cache</span><span class="sxs-lookup"><span data-stu-id="55182-195">Work with a distributed cache</span></span>](xref:performance/caching/distributed)
* [<span data-ttu-id="55182-196">Erkennen von Änderungen mit Änderungstoken</span><span class="sxs-lookup"><span data-stu-id="55182-196">Detect changes with change tokens</span></span>](xref:fundamentals/primitives/change-tokens)
* [<span data-ttu-id="55182-197">Zwischenspeichern von Antworten</span><span class="sxs-lookup"><span data-stu-id="55182-197">Response caching</span></span>](xref:performance/caching/response)
* [<span data-ttu-id="55182-198">Antworten zwischenspeichernde Middleware</span><span class="sxs-lookup"><span data-stu-id="55182-198">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="55182-199">Cache-Taghilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="55182-199">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="55182-200">Taghilfsprogramm für verteilten Cache</span><span class="sxs-lookup"><span data-stu-id="55182-200">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
