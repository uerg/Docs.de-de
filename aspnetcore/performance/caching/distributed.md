---
title: Arbeiten mit einem verteilten Cache
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: 870f082d-6d43-453d-b311-45f3aeb4d2c5
ms.technology: aspnet
ms.prod: asp.net-core
uid: performance/caching/distributed
ms.openlocfilehash: abf680fef9de175082c1e4f4cebc2b9648f18a28
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2017
---
# <a name="working-with-a-distributed-cache"></a><span data-ttu-id="507e3-103">Arbeiten mit einem verteilten cache</span><span class="sxs-lookup"><span data-stu-id="507e3-103">Working with a distributed cache</span></span>

<span data-ttu-id="507e3-104">Durch [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="507e3-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="507e3-105">Verteilte Caches können die Leistung und Skalierbarkeit von ASP.NET Core-apps verbessern, besonders bei der in eine Cloud oder Server-Umgebung gehostet.</span><span class="sxs-lookup"><span data-stu-id="507e3-105">Distributed caches can improve the performance and scalability of ASP.NET Core apps, especially when hosted in a cloud or server farm environment.</span></span> <span data-ttu-id="507e3-106">Dieser Artikel beschreibt das Arbeiten mit ASP.NET Core des integrierten verteilter Cache Speicherabstraktionen und die Implementierungen.</span><span class="sxs-lookup"><span data-stu-id="507e3-106">This article explains how to work with ASP.NET Core's built-in distributed cache abstractions and implementations.</span></span>

[<span data-ttu-id="507e3-107">Anzeigen oder Herunterladen von Beispielcode</span><span class="sxs-lookup"><span data-stu-id="507e3-107">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample)

## <a name="what-is-a-distributed-cache"></a><span data-ttu-id="507e3-108">Was ist ein verteilter Cache</span><span class="sxs-lookup"><span data-stu-id="507e3-108">What is a Distributed Cache</span></span>

<span data-ttu-id="507e3-109">Ein verteilter Cache wird von mehreren app-Servern gemeinsam genutzt (finden Sie unter [Zwischenspeichern Grundlagen](memory.md#caching-basics)).</span><span class="sxs-lookup"><span data-stu-id="507e3-109">A distributed cache is shared by multiple app servers (see [Caching Basics](memory.md#caching-basics)).</span></span> <span data-ttu-id="507e3-110">Die Informationen im Cache nicht in den Speicher der einzelnen Webserver gespeichert, und die zwischengespeicherten Daten für alle von der app-Servern verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="507e3-110">The information in the cache is not stored in the memory of individual web servers, and the cached data is available to all of the app's servers.</span></span> <span data-ttu-id="507e3-111">Dies bietet mehrere Vorteile:</span><span class="sxs-lookup"><span data-stu-id="507e3-111">This provides several advantages:</span></span>

1. <span data-ttu-id="507e3-112">Zwischengespeicherte Daten sind auf allen Webservern kohärente.</span><span class="sxs-lookup"><span data-stu-id="507e3-112">Cached data is coherent on all web servers.</span></span> <span data-ttu-id="507e3-113">Benutzer nicht unterschiedliche Ergebnisse angezeigt, je nachdem welche, die Web Server seine Anforderung behandelt</span><span class="sxs-lookup"><span data-stu-id="507e3-113">Users don't see different results depending on which web server handles their request</span></span>

2. <span data-ttu-id="507e3-114">Zwischengespeicherte Daten überdauert Web-Server neu gestartet wurde und Bereitstellungen.</span><span class="sxs-lookup"><span data-stu-id="507e3-114">Cached data survives web server restarts and deployments.</span></span> <span data-ttu-id="507e3-115">Einzelne Webserver können entfernt oder ohne Auswirkungen auf den Cache hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="507e3-115">Individual web servers can be removed or added without impacting the cache.</span></span>

3. <span data-ttu-id="507e3-116">Der Datenspeicher für die Quelle hat weniger Anforderungen an ihn (nicht mehrere in-Memory-Caches "oder" No überhaupt-cache).</span><span class="sxs-lookup"><span data-stu-id="507e3-116">The source data store has fewer requests made to it (than with multiple in-memory caches or no cache at all).</span></span>

> [!NOTE]
> <span data-ttu-id="507e3-117">Wenn Sie ein verteilter Cache von SQL Server verwenden, sind einige der Vorteile nur "true", wenn eine separate Datenbank-Instanz für den Cache als Quelldaten für die app verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="507e3-117">If using a SQL Server Distributed Cache, some of these advantages are only true if a separate database instance is used for the cache than for the app's source data.</span></span>

<span data-ttu-id="507e3-118">Wie alle Cache möglicherweise ein verteilter Cache einer app-Reaktionsfähigkeit erheblich verbessert, da in der Regel Daten aus dem Cache viel schneller als aus einer relationalen Datenbank (oder Webdienst) abgerufen werden können.</span><span class="sxs-lookup"><span data-stu-id="507e3-118">Like any cache, a distributed cache can dramatically improve an app's responsiveness, since typically data can be retrieved from the cache much faster than from a relational database (or web service).</span></span>

<span data-ttu-id="507e3-119">Cachekonfiguration ist implementierungsspezifisch.</span><span class="sxs-lookup"><span data-stu-id="507e3-119">Cache configuration is implementation specific.</span></span> <span data-ttu-id="507e3-120">Dieser Artikel beschreibt, wie so konfigurieren Sie beide Redis und verteilten SQL Server-caches.</span><span class="sxs-lookup"><span data-stu-id="507e3-120">This article describes how to configure both Redis and SQL Server distributed caches.</span></span> <span data-ttu-id="507e3-121">Unabhängig davon, welche Implementierung ausgewählt ist, interagiert die app mit dem Cache ein gemeinsames `IDistributedCache` Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="507e3-121">Regardless of which implementation is selected, the app interacts with the cache using a common `IDistributedCache` interface.</span></span>

## <a name="the-idistributedcache-interface"></a><span data-ttu-id="507e3-122">Die IDistributedCache-Schnittstelle</span><span class="sxs-lookup"><span data-stu-id="507e3-122">The IDistributedCache Interface</span></span>

<span data-ttu-id="507e3-123">Die `IDistributedCache` -Schnittstelle enthält synchrone und asynchrone Methoden.</span><span class="sxs-lookup"><span data-stu-id="507e3-123">The `IDistributedCache` interface includes synchronous and asynchronous methods.</span></span> <span data-ttu-id="507e3-124">Die Schnittstelle kann Elemente hinzugefügt, abgerufen und von der Implementierung verteilter Cache entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="507e3-124">The interface allows items to be added, retrieved, and removed from the distributed cache implementation.</span></span> <span data-ttu-id="507e3-125">Die `IDistributedCache` Schnittstelle enthält die folgenden Methoden:</span><span class="sxs-lookup"><span data-stu-id="507e3-125">The `IDistributedCache` interface includes the following methods:</span></span>

<span data-ttu-id="507e3-126">**"Get", "GetAsync**</span><span class="sxs-lookup"><span data-stu-id="507e3-126">**Get, GetAsync**</span></span>

<span data-ttu-id="507e3-127">Akzeptiert einen Zeichenfolgenschlüssel und ruft ein zwischengespeichertes Element als ein `byte[]` Wenn im Cache gefunden.</span><span class="sxs-lookup"><span data-stu-id="507e3-127">Takes a string key and retrieves a cached item as a `byte[]` if found in the cache.</span></span>

<span data-ttu-id="507e3-128">**Menge SetAsync**</span><span class="sxs-lookup"><span data-stu-id="507e3-128">**Set, SetAsync**</span></span>

<span data-ttu-id="507e3-129">Fügt ein Element hinzu (als `byte[]`) in den Cache mithilfe eines Schlüssels Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="507e3-129">Adds an item (as `byte[]`) to the cache using a string key.</span></span>

<span data-ttu-id="507e3-130">**Aktualisierung RefreshAsync**</span><span class="sxs-lookup"><span data-stu-id="507e3-130">**Refresh, RefreshAsync**</span></span>

<span data-ttu-id="507e3-131">Aktualisiert ein Element im Cache basierend auf seinen Schlüssel zurücksetzen gleitenden Ablauf des Timeouts (sofern vorhanden).</span><span class="sxs-lookup"><span data-stu-id="507e3-131">Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>

<span data-ttu-id="507e3-132">**Entfernen von removeasync-Vorgang für**</span><span class="sxs-lookup"><span data-stu-id="507e3-132">**Remove, RemoveAsync**</span></span>

<span data-ttu-id="507e3-133">Entfernt einen Cacheeintrag basierend auf seinen Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="507e3-133">Removes a cache entry based on its key.</span></span>

<span data-ttu-id="507e3-134">Verwenden der `IDistributedCache` Schnittstelle:</span><span class="sxs-lookup"><span data-stu-id="507e3-134">To use the `IDistributedCache` interface:</span></span>

   1. <span data-ttu-id="507e3-135">Fügen Sie der Projektdatei die erforderlichen NuGet-Pakete hinzu.</span><span class="sxs-lookup"><span data-stu-id="507e3-135">Add the required NuGet packages to your project file.</span></span>

   2. <span data-ttu-id="507e3-136">Konfigurieren Sie die spezielle Implementierung der `IDistributedCache` in Ihrer `Startup` Klasse `ConfigureServices` -Methode, und fügen sie es den Container hinzu.</span><span class="sxs-lookup"><span data-stu-id="507e3-136">Configure the specific implementation of `IDistributedCache` in your `Startup` class's `ConfigureServices` method, and add it to the container there.</span></span>

   3. <span data-ttu-id="507e3-137">Aus der app [`Middleware](../../fundamentals/middleware.md) or MVC controller classes, request an instance of `IDistributedCache "aus dem Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="507e3-137">From the app's [`Middleware](../../fundamentals/middleware.md) or MVC controller classes, request an instance of `IDistributedCache\` from the constructor.</span></span> <span data-ttu-id="507e3-138">Die Instanz gebotenen [Abhängigkeitsinjektion](../../fundamentals/dependency-injection.md) (DI).</span><span class="sxs-lookup"><span data-stu-id="507e3-138">The instance will be provided by [Dependency Injection](../../fundamentals/dependency-injection.md) (DI).</span></span>

> [!NOTE]
> <span data-ttu-id="507e3-139">Besteht keine Notwendigkeit für eine Singleton oder ausgelegte Lebensdauer verwendet `IDistributedCache` Instanzen (mindestens für die integrierte Implementierungen).</span><span class="sxs-lookup"><span data-stu-id="507e3-139">There is no need to use a Singleton or Scoped lifetime for `IDistributedCache` instances (at least for the built-in implementations).</span></span> <span data-ttu-id="507e3-140">Sie können auch eine Instanz erstellen, wo Sie benötigen ein möglicherweise (anstatt [Abhängigkeitsinjektion](../../fundamentals/dependency-injection.md)), Dadurch könnte jedoch Code schwieriger zu testen, und gegen die [expliziten Abhängigkeiten Prinzip](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="507e3-140">You can also create an instance wherever you might need one (instead of using [Dependency Injection](../../fundamentals/dependency-injection.md)), but this can make your code harder to test, and violates the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="507e3-141">Das folgende Beispiel zeigt, wie Sie eine Instanz von `IDistributedCache` in einer einfachen middlewarekomponente:</span><span class="sxs-lookup"><span data-stu-id="507e3-141">The following example shows how to use an instance of `IDistributedCache` in a simple middleware component:</span></span>

<span data-ttu-id="507e3-142">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/StartTimeHeader.cs?highlight=15,18,21,27,28,29,30,31)]</span><span class="sxs-lookup"><span data-stu-id="507e3-142">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/StartTimeHeader.cs?highlight=15,18,21,27,28,29,30,31)]</span></span>

<span data-ttu-id="507e3-143">Im obigen Code wird der zwischengespeicherte Wert gelesen, jedoch nie geschrieben.</span><span class="sxs-lookup"><span data-stu-id="507e3-143">In the code above, the cached value is read, but never written.</span></span> <span data-ttu-id="507e3-144">In diesem Beispiel wird der Wert nur festgelegt, wenn ein Server wird gestartet und nicht geändert.</span><span class="sxs-lookup"><span data-stu-id="507e3-144">In this sample, the value is only set when a server starts up, and doesn't change.</span></span> <span data-ttu-id="507e3-145">In einem Szenario mit mehreren Servern überschreibt der letzte Server, starten Sie alle vorherigen Werte, die von anderen Servern festgelegt wurden.</span><span class="sxs-lookup"><span data-stu-id="507e3-145">In a multi-server scenario, the most recent server to start will overwrite any previous values that were set by other servers.</span></span> <span data-ttu-id="507e3-146">Die `Get` und `Set` Methoden verwenden, die `byte[]` Typ.</span><span class="sxs-lookup"><span data-stu-id="507e3-146">The `Get` and `Set` methods use the `byte[]` type.</span></span> <span data-ttu-id="507e3-147">Aus diesem Grund der String-Wert muss konvertiert werden mit `Encoding.UTF8.GetString` (für `Get`) und `Encoding.UTF8.GetBytes` (für `Set`).</span><span class="sxs-lookup"><span data-stu-id="507e3-147">Therefore, the string value must be converted using `Encoding.UTF8.GetString` (for `Get`) and `Encoding.UTF8.GetBytes` (for `Set`).</span></span>

<span data-ttu-id="507e3-148">Der folgende code in *Startup.cs* zeigt den Wert festgelegt wird:</span><span class="sxs-lookup"><span data-stu-id="507e3-148">The following code from *Startup.cs* shows the value being set:</span></span>

<span data-ttu-id="507e3-149">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=2,4,5,6&range=58-66)]</span><span class="sxs-lookup"><span data-stu-id="507e3-149">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=2,4,5,6&range=58-66)]</span></span>

> [!NOTE]
> <span data-ttu-id="507e3-150">Seit `IDistributedCache` konfiguriert ist, der `ConfigureServices` Methode, es ist verfügbar, die `Configure` Methode als Parameter.</span><span class="sxs-lookup"><span data-stu-id="507e3-150">Since `IDistributedCache` is configured in the `ConfigureServices` method, it is available to the `Configure` method as a parameter.</span></span> <span data-ttu-id="507e3-151">Als Parameter hinzufügen, können die konfigurierte Instanz DI bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="507e3-151">Adding it as a parameter will allow the configured instance to be provided through DI.</span></span>

## <a name="using-a-redis-distributed-cache"></a><span data-ttu-id="507e3-152">Verwenden einen Redis Cache verteilten</span><span class="sxs-lookup"><span data-stu-id="507e3-152">Using a Redis Distributed Cache</span></span>

<span data-ttu-id="507e3-153">[Redis](https://redis.io/) ist ein open Source-Daten im Arbeitsspeicher speichert, das häufig als verteilte Cache verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="507e3-153">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="507e3-154">Lokal verwenden, und Sie können konfigurieren, ein [Azure Redis Cache](https://azure.microsoft.com/services/cache/) für Ihre Azure gehosteten ASP.NET Core-apps.</span><span class="sxs-lookup"><span data-stu-id="507e3-154">You can use it locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for your Azure-hosted ASP.NET Core apps.</span></span> <span data-ttu-id="507e3-155">Ihre app ASP.NET Core konfiguriert die Cache-Implementierung mit einem `RedisDistributedCache` Instanz.</span><span class="sxs-lookup"><span data-stu-id="507e3-155">Your ASP.NET Core app configures the cache implementation using a `RedisDistributedCache` instance.</span></span>

<span data-ttu-id="507e3-156">Sie konfigurieren die Redis-Implementierung in `ConfigureServices` und im app-Code darauf zugreifen, durch die Anforderung von einer Instanz von `IDistributedCache` (Siehe obigen Code).</span><span class="sxs-lookup"><span data-stu-id="507e3-156">You configure the Redis implementation in `ConfigureServices` and access it in your app code by requesting an instance of `IDistributedCache` (see the code above).</span></span>

<span data-ttu-id="507e3-157">Im Beispielcode wird ein `RedisCache` Implementierung wird verwendet, wenn die Serverkonfiguration für einen `Staging` Umgebung.</span><span class="sxs-lookup"><span data-stu-id="507e3-157">In the sample code, a `RedisCache` implementation is used when the server is configured for a `Staging` environment.</span></span> <span data-ttu-id="507e3-158">Daher die `ConfigureStagingServices` Methode konfiguriert die `RedisCache`:</span><span class="sxs-lookup"><span data-stu-id="507e3-158">Thus the `ConfigureStagingServices` method configures the `RedisCache`:</span></span>

<span data-ttu-id="507e3-159">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=8,9,10,11,12,13&range=27-40)]</span><span class="sxs-lookup"><span data-stu-id="507e3-159">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=8,9,10,11,12,13&range=27-40)]</span></span>

> [!NOTE]
> <span data-ttu-id="507e3-160">Um Redis auf dem lokalen Computer zu installieren, installieren Sie das Paket chocolatey [https://chocolatey.org/packages/redis-64/](https://chocolatey.org/packages/redis-64/) und führen Sie `redis-server` über eine Eingabeaufforderung.</span><span class="sxs-lookup"><span data-stu-id="507e3-160">To install Redis on your local machine, install the chocolatey package [https://chocolatey.org/packages/redis-64/](https://chocolatey.org/packages/redis-64/) and run `redis-server` from a command prompt.</span></span>

## <a name="using-a-sql-server-distributed-cache"></a><span data-ttu-id="507e3-161">Mithilfe eines SQLServer verteilte Caches</span><span class="sxs-lookup"><span data-stu-id="507e3-161">Using a SQL Server Distributed Cache</span></span>

<span data-ttu-id="507e3-162">Die Implementierung SqlServerCache ermöglicht verteilten Cache eine SQL Server-Datenbank als Sicherungsspeicher verwendet.</span><span class="sxs-lookup"><span data-stu-id="507e3-162">The SqlServerCache implementation allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="507e3-163">Zum Erstellen von SQL Server erstellt die Tabelle, die Sie Sql-Cache-Tool, das Tool verwenden, können eine Tabelle mit dem Namen und das Schema, die Sie angeben.</span><span class="sxs-lookup"><span data-stu-id="507e3-163">To create SQL Server table you can use sql-cache tool, the tool creates a table with the name and schema you specify.</span></span>

<span data-ttu-id="507e3-164">Um die Sql-Cache-Tool verwenden zu können, fügen `SqlConfig.Tools` auf die `<ItemGroup>` Element von der *csproj* Datei und ein Dotnet-Wiederherstellung ausführen.</span><span class="sxs-lookup"><span data-stu-id="507e3-164">To use the sql-cache tool, add `SqlConfig.Tools` to the `<ItemGroup>` element of the *.csproj* file and run dotnet restore.</span></span>

<span data-ttu-id="507e3-165">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/DistCacheSample.csproj?range=23-25)]</span><span class="sxs-lookup"><span data-stu-id="507e3-165">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/DistCacheSample.csproj?range=23-25)]</span></span>

<span data-ttu-id="507e3-166">Testen Sie SqlConfig.Tools, indem Sie den folgenden Befehl ausführen</span><span class="sxs-lookup"><span data-stu-id="507e3-166">Test SqlConfig.Tools by running the following command</span></span>

```none
C:\DistCacheSample\src\DistCacheSample>dotnet sql-cache create --help
   ```

<span data-ttu-id="507e3-167">SQL-Cache-Tool wird Auslastung, Optionen und Befehl Hilfe angezeigt, nun Sie Tabellen in SQLServer erstellen können "Sql-Cache erstellen"-Befehl ausführen:</span><span class="sxs-lookup"><span data-stu-id="507e3-167">sql-cache tool  will display usage, options and command help, now you can create tables into sql server, running "sql-cache create" command :</span></span>

```none
C:\DistCacheSample\src\DistCacheSample>dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
   info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
       Table and index were created successfully.
   ```

<span data-ttu-id="507e3-168">Die erstellte Tabelle weist das folgende Schema:</span><span class="sxs-lookup"><span data-stu-id="507e3-168">The created table has the following schema:</span></span>

![SQL Server-Cache-Tabelle](distributed/_static/SqlServerCacheTable.png)

<span data-ttu-id="507e3-170">Wie alle Cache Implementierungen Ihrer app abrufen und Festlegen von cachewerte, die mit einer Instanz von soll `IDistributedCache`, sondern eine `SqlServerCache`.</span><span class="sxs-lookup"><span data-stu-id="507e3-170">Like all cache implementations, your app should get and set cache values using an instance of `IDistributedCache`, not a `SqlServerCache`.</span></span> <span data-ttu-id="507e3-171">Das Beispiel implementiert `SqlServerCache` in der `Production` Umgebung (damit die Konfiguration in `ConfigureProductionServices`).</span><span class="sxs-lookup"><span data-stu-id="507e3-171">The sample implements `SqlServerCache` in the `Production` environment (so it is configured in `ConfigureProductionServices`).</span></span>

<span data-ttu-id="507e3-172">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=7,8,9,10,11,12&range=42-56)]</span><span class="sxs-lookup"><span data-stu-id="507e3-172">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=7,8,9,10,11,12&range=42-56)]</span></span>

> [!NOTE]
> <span data-ttu-id="507e3-173">Die `ConnectionString` (und optional `SchemaName` und `TableName`) in der Regel außerhalb von Datenquellen-Steuerelements (z. B. usersecrets.) gespeichert werden sollen, da sie möglicherweise die Anmeldeinformationen enthalten.</span><span class="sxs-lookup"><span data-stu-id="507e3-173">The `ConnectionString` (and optionally, `SchemaName` and `TableName`) should typically be stored outside of source control (such as UserSecrets), as they may contain credentials.</span></span>

## <a name="recommendations"></a><span data-ttu-id="507e3-174">Empfehlungen</span><span class="sxs-lookup"><span data-stu-id="507e3-174">Recommendations</span></span>

<span data-ttu-id="507e3-175">Wenn Sie entscheiden, welche Implementierung der `IDistributedCache` Recht für der app, und wählen Sie zwischen Redis und SQL Server auf Grundlage der vorhandenen Infrastruktur und Umgebung, Ihren leistungsanforderungen und Erfahrung Ihres Teams ist.</span><span class="sxs-lookup"><span data-stu-id="507e3-175">When deciding which implementation of `IDistributedCache` is right for your app, choose between Redis and SQL Server based on your existing infrastructure and environment, your performance requirements, and your team's experience.</span></span> <span data-ttu-id="507e3-176">Wenn Ihr Team nachrichtenorientierten arbeiten mit Redis ist, ist es eine hervorragende Wahl.</span><span class="sxs-lookup"><span data-stu-id="507e3-176">If your team is more comfortable working with Redis, it's an excellent choice.</span></span> <span data-ttu-id="507e3-177">Wenn vom Team auf SQL Server, können Sie sich, dass diese Implementierung als auch sicher sein.</span><span class="sxs-lookup"><span data-stu-id="507e3-177">If your team prefers SQL Server, you can be confident in that implementation as well.</span></span> <span data-ttu-id="507e3-178">Beachten Sie, dass eine herkömmliche Cachinglösung Daten im Arbeitsspeicher speichert ermöglicht eine schnelle Abrufen von Daten.</span><span class="sxs-lookup"><span data-stu-id="507e3-178">Note that A traditional caching solution stores data in-memory which allows for fast retrieval of data.</span></span> <span data-ttu-id="507e3-179">Häufig verwendete Daten in einem Cache gespeichert, und speichern Sie die gesamten Daten in einem permanenten Back-End-Speicher, z. B. SQL Server oder Azure-Speicher.</span><span class="sxs-lookup"><span data-stu-id="507e3-179">You should store commonly used data in a cache and store the entire data in a backend persistent store such as SQL Server or Azure Storage.</span></span> <span data-ttu-id="507e3-180">Redis-Cache ist eine Cachinglösung, die einen hohen Durchsatz und niedriger Latenz im Vergleich zu SQL-Cache enthält.</span><span class="sxs-lookup"><span data-stu-id="507e3-180">Redis Cache is a caching solution which gives you high throughput and low latency as compared to SQL Cache.</span></span>

<span data-ttu-id="507e3-181">Zusätzliche Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="507e3-181">Additional resources:</span></span>

* [<span data-ttu-id="507e3-182">Im Arbeitsspeicher Zwischenspeichern</span><span class="sxs-lookup"><span data-stu-id="507e3-182">In memory caching</span></span>](memory.md)
* [<span data-ttu-id="507e3-183">Redis-Cache für Azure</span><span class="sxs-lookup"><span data-stu-id="507e3-183">Redis Cache on Azure</span></span>](https://azure.microsoft.com/documentation/services/redis-cache/)
* [<span data-ttu-id="507e3-184">SQL­Datenbank in Azure</span><span class="sxs-lookup"><span data-stu-id="507e3-184">SQL Database on Azure</span></span>](https://azure.microsoft.com/documentation/services/sql-database/)
