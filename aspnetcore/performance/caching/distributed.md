---
title: Arbeiten mit einem verteilten Cache in ASP.NET Core
author: ardalis
description: Erfahren Sie, wie ASP.NET Core verteilte caching zur Verbesserung der app-Leistung und Skalierbarkeit, insbesondere in einer Cloud oder Server-Umgebung verwenden.
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: performance/caching/distributed
ms.openlocfilehash: 635c61cbb72a6a9eb822307bbc80936ee73bedc8
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2018
---
# <a name="working-with-a-distributed-cache-in-aspnet-core"></a><span data-ttu-id="16fca-103">Arbeiten mit einem verteilten Cache in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="16fca-103">Working with a distributed cache in ASP.NET Core</span></span>

<span data-ttu-id="16fca-104">Von [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="16fca-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="16fca-105">Verteilte Caches können die Leistung und Skalierbarkeit von ASP.NET Core-apps verbessern, besonders bei der in eine Cloud oder Server-Umgebung gehostet.</span><span class="sxs-lookup"><span data-stu-id="16fca-105">Distributed caches can improve the performance and scalability of ASP.NET Core apps, especially when hosted in a cloud or server farm environment.</span></span> <span data-ttu-id="16fca-106">Dieser Artikel beschreibt das Arbeiten mit ASP.NET Core des integrierten verteilter Cache Speicherabstraktionen und die Implementierungen.</span><span class="sxs-lookup"><span data-stu-id="16fca-106">This article explains how to work with ASP.NET Core's built-in distributed cache abstractions and implementations.</span></span>

<span data-ttu-id="16fca-107">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="16fca-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-a-distributed-cache"></a><span data-ttu-id="16fca-108">Was ist ein verteilter Cache</span><span class="sxs-lookup"><span data-stu-id="16fca-108">What is a distributed cache</span></span>

<span data-ttu-id="16fca-109">Ein verteilter Cache wird von mehreren app-Servern gemeinsam genutzt (finden Sie unter [Zwischenspeichern Grundlagen](memory.md#caching-basics)).</span><span class="sxs-lookup"><span data-stu-id="16fca-109">A distributed cache is shared by multiple app servers (see [Caching Basics](memory.md#caching-basics)).</span></span> <span data-ttu-id="16fca-110">Die Informationen im Cache befindet sich nicht im Speicher der einzelnen Webserver gespeichert, und die zwischengespeicherten Daten für alle von der app-Servern verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="16fca-110">The information in the cache isn't stored in the memory of individual web servers, and the cached data is available to all of the app's servers.</span></span> <span data-ttu-id="16fca-111">Dies bietet mehrere Vorteile:</span><span class="sxs-lookup"><span data-stu-id="16fca-111">This provides several advantages:</span></span>

1. <span data-ttu-id="16fca-112">Zwischengespeicherte Daten sind auf allen Webservern kohärente.</span><span class="sxs-lookup"><span data-stu-id="16fca-112">Cached data is coherent on all web servers.</span></span> <span data-ttu-id="16fca-113">Benutzer nicht unterschiedliche Ergebnisse angezeigt, je nachdem welche, die Web Server seine Anforderung behandelt</span><span class="sxs-lookup"><span data-stu-id="16fca-113">Users don't see different results depending on which web server handles their request</span></span>

2. <span data-ttu-id="16fca-114">Zwischengespeicherte Daten überdauert Web-Server neu gestartet wurde und Bereitstellungen.</span><span class="sxs-lookup"><span data-stu-id="16fca-114">Cached data survives web server restarts and deployments.</span></span> <span data-ttu-id="16fca-115">Einzelne Webserver können entfernt oder ohne Auswirkungen auf den Cache hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="16fca-115">Individual web servers can be removed or added without impacting the cache.</span></span>

3. <span data-ttu-id="16fca-116">Der Datenspeicher für die Quelle hat weniger Anforderungen an ihn (nicht mehrere in-Memory-Caches "oder" No überhaupt-cache).</span><span class="sxs-lookup"><span data-stu-id="16fca-116">The source data store has fewer requests made to it (than with multiple in-memory caches or no cache at all).</span></span>

> [!NOTE]
> <span data-ttu-id="16fca-117">Wenn Sie ein verteilter Cache von SQL Server verwenden, sind einige der Vorteile nur "true", wenn eine separate Datenbank-Instanz für den Cache als Quelldaten für die app verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="16fca-117">If using a SQL Server Distributed Cache, some of these advantages are only true if a separate database instance is used for the cache than for the app's source data.</span></span>

<span data-ttu-id="16fca-118">Wie alle Cache möglicherweise ein verteilter Cache einer app-Reaktionsfähigkeit erheblich verbessert, da in der Regel Daten aus dem Cache viel schneller als aus einer relationalen Datenbank (oder Webdienst) abgerufen werden können.</span><span class="sxs-lookup"><span data-stu-id="16fca-118">Like any cache, a distributed cache can dramatically improve an app's responsiveness, since typically data can be retrieved from the cache much faster than from a relational database (or web service).</span></span>

<span data-ttu-id="16fca-119">Cachekonfiguration ist implementierungsspezifisch.</span><span class="sxs-lookup"><span data-stu-id="16fca-119">Cache configuration is implementation specific.</span></span> <span data-ttu-id="16fca-120">Dieser Artikel beschreibt, wie so konfigurieren Sie beide Redis und verteilten SQL Server-caches.</span><span class="sxs-lookup"><span data-stu-id="16fca-120">This article describes how to configure both Redis and SQL Server distributed caches.</span></span> <span data-ttu-id="16fca-121">Unabhängig davon, welche Implementierung ausgewählt ist, interagiert die app mit dem Cache ein gemeinsames `IDistributedCache` Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="16fca-121">Regardless of which implementation is selected, the app interacts with the cache using a common `IDistributedCache` interface.</span></span>

## <a name="the-idistributedcache-interface"></a><span data-ttu-id="16fca-122">Die IDistributedCache-Schnittstelle</span><span class="sxs-lookup"><span data-stu-id="16fca-122">The IDistributedCache Interface</span></span>

<span data-ttu-id="16fca-123">Die `IDistributedCache` -Schnittstelle enthält synchrone und asynchrone Methoden.</span><span class="sxs-lookup"><span data-stu-id="16fca-123">The `IDistributedCache` interface includes synchronous and asynchronous methods.</span></span> <span data-ttu-id="16fca-124">Die Schnittstelle kann Elemente hinzugefügt, abgerufen und von der Implementierung verteilter Cache entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="16fca-124">The interface allows items to be added, retrieved, and removed from the distributed cache implementation.</span></span> <span data-ttu-id="16fca-125">Die `IDistributedCache` Schnittstelle enthält die folgenden Methoden:</span><span class="sxs-lookup"><span data-stu-id="16fca-125">The `IDistributedCache` interface includes the following methods:</span></span>

<span data-ttu-id="16fca-126">**Get, GetAsync**</span><span class="sxs-lookup"><span data-stu-id="16fca-126">**Get, GetAsync**</span></span>

<span data-ttu-id="16fca-127">Akzeptiert einen Zeichenfolgenschlüssel und ruft ein zwischengespeichertes Element als ein `byte[]` Wenn im Cache gefunden.</span><span class="sxs-lookup"><span data-stu-id="16fca-127">Takes a string key and retrieves a cached item as a `byte[]` if found in the cache.</span></span>

<span data-ttu-id="16fca-128">**Set, SetAsync**</span><span class="sxs-lookup"><span data-stu-id="16fca-128">**Set, SetAsync**</span></span>

<span data-ttu-id="16fca-129">Fügt ein Element hinzu (als `byte[]`) in den Cache mithilfe eines Schlüssels Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="16fca-129">Adds an item (as `byte[]`) to the cache using a string key.</span></span>

<span data-ttu-id="16fca-130">**Refresh, RefreshAsync**</span><span class="sxs-lookup"><span data-stu-id="16fca-130">**Refresh, RefreshAsync**</span></span>

<span data-ttu-id="16fca-131">Aktualisiert ein Element im Cache basierend auf seinen Schlüssel zurücksetzen gleitenden Ablauf des Timeouts (sofern vorhanden).</span><span class="sxs-lookup"><span data-stu-id="16fca-131">Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>

<span data-ttu-id="16fca-132">**Remove, RemoveAsync**</span><span class="sxs-lookup"><span data-stu-id="16fca-132">**Remove, RemoveAsync**</span></span>

<span data-ttu-id="16fca-133">Entfernt einen Cacheeintrag basierend auf seinen Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="16fca-133">Removes a cache entry based on its key.</span></span>

<span data-ttu-id="16fca-134">Verwenden der `IDistributedCache` Schnittstelle:</span><span class="sxs-lookup"><span data-stu-id="16fca-134">To use the `IDistributedCache` interface:</span></span>

   1. <span data-ttu-id="16fca-135">Fügen Sie der Projektdatei die erforderlichen NuGet-Pakete hinzu.</span><span class="sxs-lookup"><span data-stu-id="16fca-135">Add the required NuGet packages to your project file.</span></span>

   2. <span data-ttu-id="16fca-136">Konfigurieren Sie die spezielle Implementierung der `IDistributedCache` in Ihrer `Startup` Klasse `ConfigureServices` -Methode, und fügen sie es den Container hinzu.</span><span class="sxs-lookup"><span data-stu-id="16fca-136">Configure the specific implementation of `IDistributedCache` in your `Startup` class's `ConfigureServices` method, and add it to the container there.</span></span>

   3. <span data-ttu-id="16fca-137">Aus der app [Middleware](xref:fundamentals/middleware/index) oder MVC-Controller-Klassen, bitten Sie eine Instanz von `IDistributedCache` aus dem Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="16fca-137">From the app's [Middleware](xref:fundamentals/middleware/index) or MVC controller classes, request an instance of `IDistributedCache` from the constructor.</span></span> <span data-ttu-id="16fca-138">Die Instanz gebotenen [Abhängigkeitsinjektion](../../fundamentals/dependency-injection.md) (DI).</span><span class="sxs-lookup"><span data-stu-id="16fca-138">The instance will be provided by [Dependency Injection](../../fundamentals/dependency-injection.md) (DI).</span></span>

> [!NOTE]
> <span data-ttu-id="16fca-139">Besteht keine Notwendigkeit für eine Singleton oder ausgelegte Lebensdauer verwendet `IDistributedCache` Instanzen (mindestens für die integrierte Implementierungen).</span><span class="sxs-lookup"><span data-stu-id="16fca-139">There's no need to use a Singleton or Scoped lifetime for `IDistributedCache` instances (at least for the built-in implementations).</span></span> <span data-ttu-id="16fca-140">Sie können auch eine Instanz erstellen, wo Sie benötigen ein möglicherweise (anstatt [Abhängigkeitsinjektion](../../fundamentals/dependency-injection.md)), Dadurch könnte jedoch Code schwieriger zu testen, und gegen die [expliziten Abhängigkeiten Prinzip](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="16fca-140">You can also create an instance wherever you might need one (instead of using [Dependency Injection](../../fundamentals/dependency-injection.md)), but this can make your code harder to test, and violates the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="16fca-141">Das folgende Beispiel zeigt, wie Sie eine Instanz von `IDistributedCache` in einer einfachen middlewarekomponente:</span><span class="sxs-lookup"><span data-stu-id="16fca-141">The following example shows how to use an instance of `IDistributedCache` in a simple middleware component:</span></span>

[!code-csharp[](./distributed/sample/src/DistCacheSample/StartTimeHeader.cs?highlight=15,18,21,27,28,29,30,31)]

<span data-ttu-id="16fca-142">Im obigen Code wird der zwischengespeicherte Wert gelesen, jedoch nie geschrieben.</span><span class="sxs-lookup"><span data-stu-id="16fca-142">In the code above, the cached value is read, but never written.</span></span> <span data-ttu-id="16fca-143">In diesem Beispiel wird der Wert nur festgelegt, wenn ein Server wird gestartet und nicht geändert.</span><span class="sxs-lookup"><span data-stu-id="16fca-143">In this sample, the value is only set when a server starts up, and doesn't change.</span></span> <span data-ttu-id="16fca-144">In einem Szenario mit mehreren Servern überschreibt der letzte Server, starten Sie alle vorherigen Werte, die von anderen Servern festgelegt wurden.</span><span class="sxs-lookup"><span data-stu-id="16fca-144">In a multi-server scenario, the most recent server to start will overwrite any previous values that were set by other servers.</span></span> <span data-ttu-id="16fca-145">Die `Get` und `Set` Methoden verwenden, die `byte[]` Typ.</span><span class="sxs-lookup"><span data-stu-id="16fca-145">The `Get` and `Set` methods use the `byte[]` type.</span></span> <span data-ttu-id="16fca-146">Aus diesem Grund der String-Wert muss konvertiert werden mit `Encoding.UTF8.GetString` (für `Get`) und `Encoding.UTF8.GetBytes` (für `Set`).</span><span class="sxs-lookup"><span data-stu-id="16fca-146">Therefore, the string value must be converted using `Encoding.UTF8.GetString` (for `Get`) and `Encoding.UTF8.GetBytes` (for `Set`).</span></span>

<span data-ttu-id="16fca-147">Der folgende code in *Startup.cs* zeigt den Wert festgelegt wird:</span><span class="sxs-lookup"><span data-stu-id="16fca-147">The following code from *Startup.cs* shows the value being set:</span></span>

[!code-csharp[](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=2,4,5,6&range=58-66)]

> [!NOTE]
> <span data-ttu-id="16fca-148">Seit `IDistributedCache` konfiguriert ist, der `ConfigureServices` Methode, es ist verfügbar, die `Configure` Methode als Parameter.</span><span class="sxs-lookup"><span data-stu-id="16fca-148">Since `IDistributedCache` is configured in the `ConfigureServices` method, it's available to the `Configure` method as a parameter.</span></span> <span data-ttu-id="16fca-149">Als Parameter hinzufügen, können die konfigurierte Instanz DI bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="16fca-149">Adding it as a parameter will allow the configured instance to be provided through DI.</span></span>

## <a name="using-a-redis-distributed-cache"></a><span data-ttu-id="16fca-150">Verwenden einen verteilte Redis-cache</span><span class="sxs-lookup"><span data-stu-id="16fca-150">Using a Redis distributed cache</span></span>

<span data-ttu-id="16fca-151">[Redis](https://redis.io/) ist ein open Source-Daten im Arbeitsspeicher speichert, das häufig als verteilte Cache verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="16fca-151">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="16fca-152">Lokal verwenden, und Sie können konfigurieren, ein [Azure Redis Cache](https://azure.microsoft.com/services/cache/) für Ihre Azure gehosteten ASP.NET Core-apps.</span><span class="sxs-lookup"><span data-stu-id="16fca-152">You can use it locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for your Azure-hosted ASP.NET Core apps.</span></span> <span data-ttu-id="16fca-153">Ihre app ASP.NET Core konfiguriert die Cache-Implementierung mit einem `RedisDistributedCache` Instanz.</span><span class="sxs-lookup"><span data-stu-id="16fca-153">Your ASP.NET Core app configures the cache implementation using a `RedisDistributedCache` instance.</span></span>

<span data-ttu-id="16fca-154">Sie konfigurieren die Redis-Implementierung in `ConfigureServices` und im app-Code darauf zugreifen, durch die Anforderung von einer Instanz von `IDistributedCache` (Siehe obigen Code).</span><span class="sxs-lookup"><span data-stu-id="16fca-154">You configure the Redis implementation in `ConfigureServices` and access it in your app code by requesting an instance of `IDistributedCache` (see the code above).</span></span>

<span data-ttu-id="16fca-155">Im Beispielcode wird ein `RedisCache` Implementierung wird verwendet, wenn die Serverkonfiguration für einen `Staging` Umgebung.</span><span class="sxs-lookup"><span data-stu-id="16fca-155">In the sample code, a `RedisCache` implementation is used when the server is configured for a `Staging` environment.</span></span> <span data-ttu-id="16fca-156">Daher die `ConfigureStagingServices` Methode konfiguriert die `RedisCache`:</span><span class="sxs-lookup"><span data-stu-id="16fca-156">Thus the `ConfigureStagingServices` method configures the `RedisCache`:</span></span>

[!code-csharp[](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=8,9,10,11,12,13&range=27-40)]

> [!NOTE]
> <span data-ttu-id="16fca-157">Um Redis auf dem lokalen Computer zu installieren, installieren Sie das Paket chocolatey [https://chocolatey.org/packages/redis-64/](https://chocolatey.org/packages/redis-64/) und führen Sie `redis-server` über eine Eingabeaufforderung.</span><span class="sxs-lookup"><span data-stu-id="16fca-157">To install Redis on your local machine, install the chocolatey package [https://chocolatey.org/packages/redis-64/](https://chocolatey.org/packages/redis-64/) and run `redis-server` from a command prompt.</span></span>

## <a name="using-a-sql-server-distributed-cache"></a><span data-ttu-id="16fca-158">Mithilfe eines SQL Server verteilte Caches</span><span class="sxs-lookup"><span data-stu-id="16fca-158">Using a SQL Server distributed cache</span></span>

<span data-ttu-id="16fca-159">Die Implementierung SqlServerCache ermöglicht verteilten Cache eine SQL Server-Datenbank als Sicherungsspeicher verwendet.</span><span class="sxs-lookup"><span data-stu-id="16fca-159">The SqlServerCache implementation allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="16fca-160">Zum Erstellen von SQL Server erstellt die Tabelle, die Sie Sql-Cache-Tool, das Tool verwenden, können eine Tabelle mit dem Namen und das Schema, die Sie angeben.</span><span class="sxs-lookup"><span data-stu-id="16fca-160">To create SQL Server table you can use sql-cache tool, the tool creates a table with the name and schema you specify.</span></span>

<span data-ttu-id="16fca-161">Um die Sql-Cache-Tool verwenden zu können, fügen `SqlConfig.Tools` auf die `<ItemGroup>` Element von der *csproj* Datei und ein Dotnet-Wiederherstellung ausführen.</span><span class="sxs-lookup"><span data-stu-id="16fca-161">To use the sql-cache tool, add `SqlConfig.Tools` to the `<ItemGroup>` element of the *.csproj* file and run dotnet restore.</span></span>

[!code-xml[](./distributed/sample/src/DistCacheSample/DistCacheSample.csproj?range=23-25)]

<span data-ttu-id="16fca-162">Testen Sie SqlConfig.Tools, indem Sie den folgenden Befehl ausführen</span><span class="sxs-lookup"><span data-stu-id="16fca-162">Test SqlConfig.Tools by running the following command</span></span>

```none
C:\DistCacheSample\src\DistCacheSample>dotnet sql-cache create --help
   ```

<span data-ttu-id="16fca-163">SQL-Cache-Tool wird Auslastung, Optionen und Befehl Hilfe angezeigt, nun Sie Tabellen in SQLServer erstellen können "Sql-Cache erstellen"-Befehl ausführen:</span><span class="sxs-lookup"><span data-stu-id="16fca-163">sql-cache tool  will display usage, options and command help, now you can create tables into sql server, running "sql-cache create" command :</span></span>

```none
C:\DistCacheSample\src\DistCacheSample>dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
   info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
       Table and index were created successfully.
   ```

<span data-ttu-id="16fca-164">Die erstellte Tabelle weist das folgende Schema:</span><span class="sxs-lookup"><span data-stu-id="16fca-164">The created table has the following schema:</span></span>

![SQL Server-Cache-Tabelle](distributed/_static/SqlServerCacheTable.png)

<span data-ttu-id="16fca-166">Wie alle Cache Implementierungen Ihrer app abrufen und Festlegen von cachewerte, die mit einer Instanz von soll `IDistributedCache`, sondern eine `SqlServerCache`.</span><span class="sxs-lookup"><span data-stu-id="16fca-166">Like all cache implementations, your app should get and set cache values using an instance of `IDistributedCache`, not a `SqlServerCache`.</span></span> <span data-ttu-id="16fca-167">Das Beispiel implementiert `SqlServerCache` in der `Production` Umgebung (damit die Konfiguration in `ConfigureProductionServices`).</span><span class="sxs-lookup"><span data-stu-id="16fca-167">The sample implements `SqlServerCache` in the `Production` environment (so it's configured in `ConfigureProductionServices`).</span></span>

[!code-csharp[](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=7,8,9,10,11,12&range=42-56)]

> [!NOTE]
> <span data-ttu-id="16fca-168">Die `ConnectionString` (und optional `SchemaName` und `TableName`) in der Regel außerhalb von Datenquellen-Steuerelements (z. B. usersecrets.) gespeichert werden sollen, da sie möglicherweise die Anmeldeinformationen enthalten.</span><span class="sxs-lookup"><span data-stu-id="16fca-168">The `ConnectionString` (and optionally, `SchemaName` and `TableName`) should typically be stored outside of source control (such as UserSecrets), as they may contain credentials.</span></span>

## <a name="recommendations"></a><span data-ttu-id="16fca-169">Empfehlungen</span><span class="sxs-lookup"><span data-stu-id="16fca-169">Recommendations</span></span>

<span data-ttu-id="16fca-170">Wenn Sie entscheiden, welche Implementierung der `IDistributedCache` Recht für der app, und wählen Sie zwischen Redis und SQL Server auf Grundlage der vorhandenen Infrastruktur und Umgebung, Ihren leistungsanforderungen und Erfahrung Ihres Teams ist.</span><span class="sxs-lookup"><span data-stu-id="16fca-170">When deciding which implementation of `IDistributedCache` is right for your app, choose between Redis and SQL Server based on your existing infrastructure and environment, your performance requirements, and your team's experience.</span></span> <span data-ttu-id="16fca-171">Wenn Ihr Team nachrichtenorientierten arbeiten mit Redis ist, ist es eine hervorragende Wahl.</span><span class="sxs-lookup"><span data-stu-id="16fca-171">If your team is more comfortable working with Redis, it's an excellent choice.</span></span> <span data-ttu-id="16fca-172">Wenn vom Team auf SQL Server, können Sie sich, dass diese Implementierung als auch sicher sein.</span><span class="sxs-lookup"><span data-stu-id="16fca-172">If your team prefers SQL Server, you can be confident in that implementation as well.</span></span> <span data-ttu-id="16fca-173">Beachten Sie, dass eine herkömmliche Cachinglösung Daten im Arbeitsspeicher speichert ermöglicht eine schnelle Abrufen von Daten.</span><span class="sxs-lookup"><span data-stu-id="16fca-173">Note that A traditional caching solution stores data in-memory which allows for fast retrieval of data.</span></span> <span data-ttu-id="16fca-174">Häufig verwendete Daten in einem Cache gespeichert, und speichern Sie die gesamten Daten in einem permanenten Back-End-Speicher, z. B. SQL Server oder Azure-Speicher.</span><span class="sxs-lookup"><span data-stu-id="16fca-174">You should store commonly used data in a cache and store the entire data in a backend persistent store such as SQL Server or Azure Storage.</span></span> <span data-ttu-id="16fca-175">Redis-Cache ist eine Cachinglösung, die einen hohen Durchsatz und niedriger Latenz im Vergleich zu SQL-Cache enthält.</span><span class="sxs-lookup"><span data-stu-id="16fca-175">Redis Cache is a caching solution which gives you high throughput and low latency as compared to SQL Cache.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="16fca-176">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="16fca-176">Additional resources</span></span>

* [<span data-ttu-id="16fca-177">Redis-Cache für Azure</span><span class="sxs-lookup"><span data-stu-id="16fca-177">Redis Cache on Azure</span></span>](https://azure.microsoft.com/documentation/services/redis-cache/)
* [<span data-ttu-id="16fca-178">SQL­Datenbank in Azure</span><span class="sxs-lookup"><span data-stu-id="16fca-178">SQL Database on Azure</span></span>](https://azure.microsoft.com/documentation/services/sql-database/)
* [<span data-ttu-id="16fca-179">Zwischenspeicherung im Speicher</span><span class="sxs-lookup"><span data-stu-id="16fca-179">In-memory caching</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="16fca-180">Erkennen von Änderungen mit Änderungstoken</span><span class="sxs-lookup"><span data-stu-id="16fca-180">Detect changes with change tokens</span></span>](xref:fundamentals/primitives/change-tokens)
* [<span data-ttu-id="16fca-181">Zwischenspeichern von Antworten</span><span class="sxs-lookup"><span data-stu-id="16fca-181">Response caching</span></span>](xref:performance/caching/response)
* [<span data-ttu-id="16fca-182">Antworten zwischenspeichernde Middleware</span><span class="sxs-lookup"><span data-stu-id="16fca-182">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="16fca-183">Cache-Taghilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="16fca-183">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="16fca-184">Taghilfsprogramm für verteilten Cache</span><span class="sxs-lookup"><span data-stu-id="16fca-184">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
