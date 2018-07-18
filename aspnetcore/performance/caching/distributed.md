---
title: Arbeiten Sie mit einem verteilten Cache in ASP.NET Core
author: ardalis
description: Erfahren Sie, wie ASP.NET Core verteilte Zwischenspeicherung zum Verbessern der app-Leistung und Skalierbarkeit, insbesondere in einer Cloud oder einer serverfarmumgebung verwenden.
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2017
uid: performance/caching/distributed
ms.openlocfilehash: 861664fcad576c11abe052837b72367eb2b9479a
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095680"
---
# <a name="work-with-a-distributed-cache-in-aspnet-core"></a><span data-ttu-id="21e48-103">Arbeiten Sie mit einem verteilten Cache in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="21e48-103">Work with a distributed cache in ASP.NET Core</span></span>

<span data-ttu-id="21e48-104">Von [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="21e48-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="21e48-105">Verteilte Caches können die Leistung und Skalierbarkeit von ASP.NET Core-apps, verbessern, insbesondere, wenn in der Cloud oder einer Serverfarm gehostet.</span><span class="sxs-lookup"><span data-stu-id="21e48-105">Distributed caches can improve the performance and scalability of ASP.NET Core apps, especially when hosted in the cloud or a server farm.</span></span>

<span data-ttu-id="21e48-106">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="21e48-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-a-distributed-cache"></a><span data-ttu-id="21e48-107">Was ist ein verteilter Cache</span><span class="sxs-lookup"><span data-stu-id="21e48-107">What is a distributed cache</span></span>

<span data-ttu-id="21e48-108">Ein verteilter Cache wird von mehreren app-Servern gemeinsam genutzt (finden Sie unter [Cache – Grundlagen](memory.md#caching-basics)).</span><span class="sxs-lookup"><span data-stu-id="21e48-108">A distributed cache is shared by multiple app servers (see [Cache Basics](memory.md#caching-basics)).</span></span> <span data-ttu-id="21e48-109">Die Informationen im Cache nicht im Speicher der einzelnen Webserver gespeichert, und die zwischengespeicherten Daten sind für alle von der app-Servern zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="21e48-109">The information in the cache isn't stored in the memory of individual web servers, and the cached data is available to all of the app's servers.</span></span> <span data-ttu-id="21e48-110">Dies bietet mehrere Vorteile:</span><span class="sxs-lookup"><span data-stu-id="21e48-110">This provides several advantages:</span></span>

1. <span data-ttu-id="21e48-111">Zwischengespeicherte Daten ist auf allen Webservern kohärent.</span><span class="sxs-lookup"><span data-stu-id="21e48-111">Cached data is coherent on all web servers.</span></span> <span data-ttu-id="21e48-112">Benutzer nicht unterschiedliche Ergebnisse angezeigt, je nachdem welche, die Web Server die Anforderung verarbeitet</span><span class="sxs-lookup"><span data-stu-id="21e48-112">Users don't see different results depending on which web server handles their request</span></span>

2. <span data-ttu-id="21e48-113">Zwischengespeicherte Daten überdauert Web Server neu gestartet wurde und Bereitstellungen.</span><span class="sxs-lookup"><span data-stu-id="21e48-113">Cached data survives web server restarts and deployments.</span></span> <span data-ttu-id="21e48-114">Einzelne Webserver können entfernt oder hinzugefügt werden, ohne Auswirkungen auf den Cache werden.</span><span class="sxs-lookup"><span data-stu-id="21e48-114">Individual web servers can be removed or added without impacting the cache.</span></span>

3. <span data-ttu-id="21e48-115">Der quelldatenspeicher hat weniger Anforderungen, die für sie (als überhaupt mit mehrere in-Memory-Caches "oder" No cache).</span><span class="sxs-lookup"><span data-stu-id="21e48-115">The source data store has fewer requests made to it (than with multiple in-memory caches or no cache at all).</span></span>

> [!NOTE]
> <span data-ttu-id="21e48-116">Wenn Sie einen SQL Server Distributed Cache verwenden zu können, sind einige der Vorteile nur true, wenn Sie eine separate Datenbank-Instanz für den Cache als für die app Daten verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="21e48-116">If using a SQL Server Distributed Cache, some of these advantages are only true if a separate database instance is used for the cache than for the app's source data.</span></span>

<span data-ttu-id="21e48-117">Z. B. einen Cache kann ein verteilter Cache einer app-Reaktionsfähigkeit, erheblich verbessern, da in der Regel Daten aus dem Cache viel schneller als aus einer relationalen Datenbank (oder Webdienst) abgerufen werden können.</span><span class="sxs-lookup"><span data-stu-id="21e48-117">Like any cache, a distributed cache can dramatically improve an app's responsiveness, since typically data can be retrieved from the cache much faster than from a relational database (or web service).</span></span>

<span data-ttu-id="21e48-118">Cache-Konfiguration ist implementierungsspezifisch.</span><span class="sxs-lookup"><span data-stu-id="21e48-118">Cache configuration is implementation specific.</span></span> <span data-ttu-id="21e48-119">In diesem Artikel wird beschrieben, wie so konfigurieren Sie beide Redis und SQL Server verteilte caches.</span><span class="sxs-lookup"><span data-stu-id="21e48-119">This article describes how to configure both Redis and SQL Server distributed caches.</span></span> <span data-ttu-id="21e48-120">Unabhängig davon, welche Implementierung aktiviert ist, der die app interagiert mit dem Cache ein gemeinsames `IDistributedCache` Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="21e48-120">Regardless of which implementation is selected, the app interacts with the cache using a common `IDistributedCache` interface.</span></span>

## <a name="the-idistributedcache-interface"></a><span data-ttu-id="21e48-121">Die IDistributedCache-Schnittstelle</span><span class="sxs-lookup"><span data-stu-id="21e48-121">The IDistributedCache Interface</span></span>

<span data-ttu-id="21e48-122">Die `IDistributedCache` -Schnittstelle enthält synchrone und asynchrone Methoden.</span><span class="sxs-lookup"><span data-stu-id="21e48-122">The `IDistributedCache` interface includes synchronous and asynchronous methods.</span></span> <span data-ttu-id="21e48-123">Die Schnittstelle kann Elemente hinzugefügt, abgerufen und von der Implementierung des verteilten Cache entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="21e48-123">The interface allows items to be added, retrieved, and removed from the distributed cache implementation.</span></span> <span data-ttu-id="21e48-124">Die `IDistributedCache` Schnittstelle enthält die folgenden Methoden:</span><span class="sxs-lookup"><span data-stu-id="21e48-124">The `IDistributedCache` interface includes the following methods:</span></span>

<span data-ttu-id="21e48-125">**Get, "getasync"**</span><span class="sxs-lookup"><span data-stu-id="21e48-125">**Get, GetAsync**</span></span>

<span data-ttu-id="21e48-126">Akzeptiert einen Zeichenfolgenschlüssel und ruft ein zwischengespeichertes Element als ein `byte[]` Wenn im Cache gefunden.</span><span class="sxs-lookup"><span data-stu-id="21e48-126">Takes a string key and retrieves a cached item as a `byte[]` if found in the cache.</span></span>

<span data-ttu-id="21e48-127">**Menge SetAsync**</span><span class="sxs-lookup"><span data-stu-id="21e48-127">**Set, SetAsync**</span></span>

<span data-ttu-id="21e48-128">Fügt ein Element hinzu (als `byte[]`) in den Cache mit einem Zeichenfolgenschlüssel.</span><span class="sxs-lookup"><span data-stu-id="21e48-128">Adds an item (as `byte[]`) to the cache using a string key.</span></span>

<span data-ttu-id="21e48-129">**Aktualisierung der RefreshAsync**</span><span class="sxs-lookup"><span data-stu-id="21e48-129">**Refresh, RefreshAsync**</span></span>

<span data-ttu-id="21e48-130">Aktualisiert ein Element im Cache basierend auf seinen Schlüssel an, das Zurücksetzen von gleitenden Ablauf des Timeouts (sofern vorhanden).</span><span class="sxs-lookup"><span data-stu-id="21e48-130">Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>

<span data-ttu-id="21e48-131">**Remove, RemoveAsync**</span><span class="sxs-lookup"><span data-stu-id="21e48-131">**Remove, RemoveAsync**</span></span>

<span data-ttu-id="21e48-132">Entfernt einen Eintrag im Cache basierend auf seinen Schlüssel an.</span><span class="sxs-lookup"><span data-stu-id="21e48-132">Removes a cache entry based on its key.</span></span>

<span data-ttu-id="21e48-133">Verwenden der `IDistributedCache` Schnittstelle:</span><span class="sxs-lookup"><span data-stu-id="21e48-133">To use the `IDistributedCache` interface:</span></span>

   1. <span data-ttu-id="21e48-134">Fügen Sie die erforderlichen NuGet-Pakete zu Ihrer Projektdatei hinzu.</span><span class="sxs-lookup"><span data-stu-id="21e48-134">Add the required NuGet packages to your project file.</span></span>

   2. <span data-ttu-id="21e48-135">Konfigurieren Sie die konkrete Implementierung der `IDistributedCache` in Ihre `Startup` Klasse `ConfigureServices` -Methode, und fügen sie es dem Container hinzu.</span><span class="sxs-lookup"><span data-stu-id="21e48-135">Configure the specific implementation of `IDistributedCache` in your `Startup` class's `ConfigureServices` method, and add it to the container there.</span></span>

   3. <span data-ttu-id="21e48-136">Von der app [Middleware](xref:fundamentals/middleware/index) oder MVC-Controller-Klassen, fordern Sie eine Instanz von `IDistributedCache` aus dem Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="21e48-136">From the app's [Middleware](xref:fundamentals/middleware/index) or MVC controller classes, request an instance of `IDistributedCache` from the constructor.</span></span> <span data-ttu-id="21e48-137">Die Instanz erfolgt durch [Dependency Injection](../../fundamentals/dependency-injection.md) (DI).</span><span class="sxs-lookup"><span data-stu-id="21e48-137">The instance will be provided by [Dependency Injection](../../fundamentals/dependency-injection.md) (DI).</span></span>

> [!NOTE]
> <span data-ttu-id="21e48-138">Besteht keine Notwendigkeit für eine Singleton- oder Bereichsbezogene Lebensdauer verwenden `IDistributedCache` Instanzen (mindestens für die integrierten Implementierungen).</span><span class="sxs-lookup"><span data-stu-id="21e48-138">There's no need to use a Singleton or Scoped lifetime for `IDistributedCache` instances (at least for the built-in implementations).</span></span> <span data-ttu-id="21e48-139">Sie können auch eine Instanz erstellen, wenn Sie eine möglicherweise (anstelle von [Dependency Injection](../../fundamentals/dependency-injection.md)), aber dies kann Ihr Code schwieriger zu testen, und verstößt gegen die [Prinzip der expliziten Abhängigkeiten](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="21e48-139">You can also create an instance wherever you might need one (instead of using [Dependency Injection](../../fundamentals/dependency-injection.md)), but this can make your code harder to test, and violates the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="21e48-140">Das folgende Beispiel zeigt, wie Sie mit einer Instanz von `IDistributedCache` in einer einfachen middlewarekomponente:</span><span class="sxs-lookup"><span data-stu-id="21e48-140">The following example shows how to use an instance of `IDistributedCache` in a simple middleware component:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/StartTimeHeader.cs)]

<span data-ttu-id="21e48-141">Im obigen Code wird der zwischengespeicherte Wert gelesen, jedoch nie geschrieben.</span><span class="sxs-lookup"><span data-stu-id="21e48-141">In the code above, the cached value is read, but never written.</span></span> <span data-ttu-id="21e48-142">In diesem Beispiel wird nur der Wert festgelegt, wenn ein Server startet, und nicht geändert.</span><span class="sxs-lookup"><span data-stu-id="21e48-142">In this sample, the value is only set when a server starts up, and doesn't change.</span></span> <span data-ttu-id="21e48-143">In einem Szenario mit mehreren Servern wird der letzte Server, starten Sie vorherige Werte überschrieben, die von anderen Servern festgelegt wurden.</span><span class="sxs-lookup"><span data-stu-id="21e48-143">In a multi-server scenario, the most recent server to start will overwrite any previous values that were set by other servers.</span></span> <span data-ttu-id="21e48-144">Die `Get` und `Set` Methoden verwenden die `byte[]` Typ.</span><span class="sxs-lookup"><span data-stu-id="21e48-144">The `Get` and `Set` methods use the `byte[]` type.</span></span> <span data-ttu-id="21e48-145">Aus diesem Grund der Zeichenfolgenwert muss konvertiert werden mithilfe von `Encoding.UTF8.GetString` (für `Get`) und `Encoding.UTF8.GetBytes` (für `Set`).</span><span class="sxs-lookup"><span data-stu-id="21e48-145">Therefore, the string value must be converted using `Encoding.UTF8.GetString` (for `Get`) and `Encoding.UTF8.GetBytes` (for `Set`).</span></span>

<span data-ttu-id="21e48-146">Der folgende code aus *"Startup.cs"* zeigt den Wert, der festgelegt wird:</span><span class="sxs-lookup"><span data-stu-id="21e48-146">The following code from *Startup.cs* shows the value being set:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet1)]

> [!NOTE]
> <span data-ttu-id="21e48-147">Da `IDistributedCache` konfiguriert ist, der `ConfigureServices` -Methode, er kann die `Configure` Methode als Parameter.</span><span class="sxs-lookup"><span data-stu-id="21e48-147">Since `IDistributedCache` is configured in the `ConfigureServices` method, it's available to the `Configure` method as a parameter.</span></span> <span data-ttu-id="21e48-148">Als Parameter hinzufügen, können die konfigurierte Instanz über Dependency Injection bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="21e48-148">Adding it as a parameter will allow the configured instance to be provided through DI.</span></span>

## <a name="using-a-redis-distributed-cache"></a><span data-ttu-id="21e48-149">Mithilfe eines verteilten Redis-Caches</span><span class="sxs-lookup"><span data-stu-id="21e48-149">Using a Redis distributed cache</span></span>

<span data-ttu-id="21e48-150">[Redis](https://redis.io/) ist ein open-Source-Speicher in-Memory-Daten, die häufig als ein verteilter Cache verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="21e48-150">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="21e48-151">Können Sie sie lokal, und Sie können konfigurieren, eine [Azure Redis Cache](https://azure.microsoft.com/services/cache/) für Ihre Azure gehostete ASP.NET Core-apps.</span><span class="sxs-lookup"><span data-stu-id="21e48-151">You can use it locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for your Azure-hosted ASP.NET Core apps.</span></span> <span data-ttu-id="21e48-152">Ihrer ASP.NET Core-app konfiguriert, den Cache-Implementierung mit einer `RedisDistributedCache` Instanz.</span><span class="sxs-lookup"><span data-stu-id="21e48-152">Your ASP.NET Core app configures the cache implementation using a `RedisDistributedCache` instance.</span></span>

<span data-ttu-id="21e48-153">Sie konfigurieren, dass die Redis-Implementierung in `ConfigureServices` und im app-Code darauf zugreifen, durch die Anforderung einer Instanz von `IDistributedCache` (Siehe den Code oben).</span><span class="sxs-lookup"><span data-stu-id="21e48-153">You configure the Redis implementation in `ConfigureServices` and access it in your app code by requesting an instance of `IDistributedCache` (see the code above).</span></span>

<span data-ttu-id="21e48-154">Im Beispielcode wird ein `RedisCache` Implementierung wird verwendet, wenn der Server, für konfiguriert ist eine `Staging` Umgebung.</span><span class="sxs-lookup"><span data-stu-id="21e48-154">In the sample code, a `RedisCache` implementation is used when the server is configured for a `Staging` environment.</span></span> <span data-ttu-id="21e48-155">Daher die `ConfigureStagingServices` -Methode konfiguriert die `RedisCache`:</span><span class="sxs-lookup"><span data-stu-id="21e48-155">Thus the `ConfigureStagingServices` method configures the `RedisCache`:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet2)]

> [!NOTE]
> <span data-ttu-id="21e48-156">Um Redis auf Ihrem lokalen Computer zu installieren, installieren Sie das chocolatey-Paket [ https://chocolatey.org/packages/redis-64/ ](https://chocolatey.org/packages/redis-64/) , und führen Sie `redis-server` über eine Eingabeaufforderung.</span><span class="sxs-lookup"><span data-stu-id="21e48-156">To install Redis on your local machine, install the chocolatey package [https://chocolatey.org/packages/redis-64/](https://chocolatey.org/packages/redis-64/) and run `redis-server` from a command prompt.</span></span>

## <a name="using-a-sql-server-distributed-cache"></a><span data-ttu-id="21e48-157">Verwenden eines SQL Server verteilte Caches</span><span class="sxs-lookup"><span data-stu-id="21e48-157">Using a SQL Server distributed cache</span></span>

<span data-ttu-id="21e48-158">Die SqlServerCache-Implementierung ermöglicht den verteilten Cache, die SQL Server-Datenbank als Sicherungsspeicher verwendet.</span><span class="sxs-lookup"><span data-stu-id="21e48-158">The SqlServerCache implementation allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="21e48-159">Zum Erstellen von SQL Server erstellt die Tabelle, die Sie Sql-Cache-Tool, das Tool verwenden, können eine Tabelle mit dem Namen und das Schema, die Sie angeben.</span><span class="sxs-lookup"><span data-stu-id="21e48-159">To create SQL Server table you can use sql-cache tool, the tool creates a table with the name and schema you specify.</span></span>

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="21e48-160">Hinzufügen `SqlConfig.Tools` auf die `<ItemGroup>` Element der Projektdatei und Ausführung `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="21e48-160">Add `SqlConfig.Tools` to the `<ItemGroup>` element of the project file and run `dotnet restore`.</span></span>

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.Extensions.Caching.SqlConfig.Tools" 
                          Version="2.0.2" />
</ItemGroup>
```

::: moniker-end

<span data-ttu-id="21e48-161">Testen Sie SqlConfig.Tools, indem Sie den folgenden Befehl ausführen:</span><span class="sxs-lookup"><span data-stu-id="21e48-161">Test SqlConfig.Tools by running the following command:</span></span>

```console
dotnet sql-cache create --help
```

<span data-ttu-id="21e48-162">SqlConfig.Tools zeigt Nutzung, Optionen und zu Befehlen.</span><span class="sxs-lookup"><span data-stu-id="21e48-162">SqlConfig.Tools displays usage, options, and command help.</span></span>

<span data-ttu-id="21e48-163">Erstellen Sie eine Tabelle in SQL Server durch Ausführen der `sql-cache create` Befehl:</span><span class="sxs-lookup"><span data-stu-id="21e48-163">Create a table in SQL Server by running the `sql-cache create` command :</span></span>

```console
dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
Table and index were created successfully.
```

<span data-ttu-id="21e48-164">Die erstellte Tabelle weist das folgende Schema:</span><span class="sxs-lookup"><span data-stu-id="21e48-164">The created table has the following schema:</span></span>

![SQL Server-Cache-Tabelle](distributed/_static/SqlServerCacheTable.png)

<span data-ttu-id="21e48-166">Wie alle cacheimplementierungen, Ihre app abrufen und Festlegen von Cache-Werten, die mit einer Instanz von `IDistributedCache`, sondern eine `SqlServerCache`.</span><span class="sxs-lookup"><span data-stu-id="21e48-166">Like all cache implementations, your app should get and set cache values using an instance of `IDistributedCache`, not a `SqlServerCache`.</span></span> <span data-ttu-id="21e48-167">Das Beispiel implementiert `SqlServerCache` in der produktionsumgebung (also die Konfiguration in `ConfigureProductionServices`).</span><span class="sxs-lookup"><span data-stu-id="21e48-167">The sample implements `SqlServerCache` in the Production environment (so it's configured in `ConfigureProductionServices`).</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet3)]

> [!NOTE]
> <span data-ttu-id="21e48-168">Die `ConnectionString` (und optional `SchemaName` und `TableName`) in der Regel außerhalb der quellcodeverwaltung (z. B. "usersecrets"), gespeichert werden sollen, wie sie die Anmeldeinformationen enthalten können.</span><span class="sxs-lookup"><span data-stu-id="21e48-168">The `ConnectionString` (and optionally, `SchemaName` and `TableName`) should typically be stored outside of source control (such as UserSecrets), as they may contain credentials.</span></span>

## <a name="recommendations"></a><span data-ttu-id="21e48-169">Empfehlungen</span><span class="sxs-lookup"><span data-stu-id="21e48-169">Recommendations</span></span>

<span data-ttu-id="21e48-170">Bei der Entscheidung, welche Implementierung der `IDistributedCache` entscheiden Sie für Ihre app, Redis und SQL Server auf Grundlage der vorhandenen Infrastruktur und Umgebung, Ihren leistungsanforderungen und Ihres Teams Erfahrung.</span><span class="sxs-lookup"><span data-stu-id="21e48-170">When deciding which implementation of `IDistributedCache` is right for your app, choose between Redis and SQL Server based on your existing infrastructure and environment, your performance requirements, and your team's experience.</span></span> <span data-ttu-id="21e48-171">Wenn Ihr Team mehr gern mit Redis ist, ist es eine hervorragende Wahl.</span><span class="sxs-lookup"><span data-stu-id="21e48-171">If your team is more comfortable working with Redis, it's an excellent choice.</span></span> <span data-ttu-id="21e48-172">Wenn Ihr Team über SQL Server bevorzugt, können Sie auch, dass diese Implementierung davon überzeugt sein.</span><span class="sxs-lookup"><span data-stu-id="21e48-172">If your team prefers SQL Server, you can be confident in that implementation as well.</span></span> <span data-ttu-id="21e48-173">Beachten Sie, dass eine herkömmliche Cachinglösung Daten im Arbeitsspeicher speichert, die für den schnellen Abruf der Daten ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="21e48-173">Note that a traditional caching solution stores data in-memory which allows for fast retrieval of data.</span></span> <span data-ttu-id="21e48-174">Sie sollten häufig verwendete Daten in einem Cache speichern, und speichern die gesamten Daten in einem Back-End-permanenten Speicher wie SQL Server oder Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="21e48-174">You should store commonly used data in a cache and store the entire data in a backend persistent store such as SQL Server or Azure Storage.</span></span> <span data-ttu-id="21e48-175">Redis-Cache ist eine Cachinglösung, die Ihnen im Vergleich zu SQL-Cache hohe Durchsatzrate mit geringer Latenz bietet.</span><span class="sxs-lookup"><span data-stu-id="21e48-175">Redis Cache is a caching solution which gives you high throughput and low latency as compared to SQL Cache.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="21e48-176">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="21e48-176">Additional resources</span></span>

* [<span data-ttu-id="21e48-177">Redis-Cache in Azure</span><span class="sxs-lookup"><span data-stu-id="21e48-177">Redis Cache on Azure</span></span>](https://azure.microsoft.com/documentation/services/redis-cache/)
* [<span data-ttu-id="21e48-178">SQL­Datenbank in Azure</span><span class="sxs-lookup"><span data-stu-id="21e48-178">SQL Database on Azure</span></span>](https://azure.microsoft.com/documentation/services/sql-database/)
* <xref:performance/caching/memory>
* <xref:fundamentals/primitives/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
