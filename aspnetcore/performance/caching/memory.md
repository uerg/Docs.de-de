---
title: In-Memory-caching in ASP.NET Core
author: rick-anderson
description: Zeigt, wie Daten im Arbeitsspeicher in ASP.NET Core zwischenzuspeichern.
keywords: ASP.NET Core, Cache, in-Memory-Leistung
ms.author: riande
manager: wpickett
ms.date: 12/14/2016
ms.topic: article
ms.assetid: 819511cf-d33e-410a-b5a9-bef7fa64d2f3
ms.technology: aspnet
ms.prod: asp.net-core
uid: performance/caching/memory
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f872cd0c355f7961ae8628c28c62d3b51c8db2c5
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
# <a name="introduction-to-in-memory-caching-in-aspnet-core"></a>Einführung in die im Arbeitsspeicher Zwischenspeichern in ASP.NET Core

Durch [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), und [Steve Smith](http://ardalis.com)

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample)

## <a name="caching-basics"></a>Caching-Grundlagen

Caching kann deutlich die Leistung und Skalierbarkeit einer App verbessert werden, verringern den Arbeitsaufwand beim Generieren der Inhalte. Zwischenspeichern funktioniert am besten mit Daten, die sich selten ändern. Caching stellt eine Kopie der Daten, die viel zurückgegeben werden, können aus der Originalquelle schneller. Schreiben und Testen Sie Ihre app niemals hängen mit zwischengespeicherten Daten.

ASP.NET Core unterstützt mehrere unterschiedliche Caches gelten. Die einfachste Cache basiert auf der [IMemoryCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache), der einen Cache im Arbeitsspeicher des Webservers gespeichert darstellt. Apps, die auf eine Serverfarm mit mehreren Servern ausgeführt werden sollten sicherstellen, dass Sitzungen Kurznotiz bei Verwendung von in-Memory-Caches. Persistente Sitzungen stellen Sie sicher, dass nachfolgende Anforderungen von einem Client, die alle auf denselben Server wechseln. Z. B. Azure-Web-apps verwenden [Application Request Routing](http://www.iis.net/learn/extensions/planning-for-arr) (ARR) auf alle nachfolgenden Anforderungen mit dem gleichen Server weiterzuleiten.

Nicht persistente Sitzungen in einer Webfarm erfordert eine [verteilte Caches](distributed.md) Cache Konsistenzprobleme zu vermeiden. Bei einigen apps kann ein verteilter Cache höher Dezentrales Skalieren als ein in-Memory-Cache unterstützen. Mit einem verteilten Cache entlastet den Cachespeicher zu einem externen Prozess. 

Die `IMemoryCache` Cache entfernen Einträge im Cache nicht genügend Arbeitsspeicher vorhanden, es sei denn, die [Zwischenspeichern Priorität](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheitempriority) festgelegt ist, um `CacheItemPriority.NeverRemove`. Sie können festlegen, die `CacheItemPriority` , passen Sie die Priorität des Caches entfernt Elemente nicht genügend Arbeitsspeicher vorhanden.

Die in-Memory-Cache kann jedes Objekt speichern. die Schnittstelle für verteilte Caches ist auf `byte[]`.

## <a name="using-imemorycache"></a>Verwenden von IMemoryCache

In-Memory-caching ist ein *Service* verwiesen, wird die app mithilfe [Abhängigkeitsinjektion](../../fundamentals/dependency-injection.md). Rufen Sie `AddMemoryCache` in `ConfigureServices`:

[!code-csharp[Main](memory/sample/WebCache/Startup.cs?highlight=8)] 

Anfordern der `IMemoryCache` Instanz im Konstruktor:

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor&highlight=3,5-)] 

`IMemoryCache`erfordert die NuGet-Paket "Microsoft.Extensions.Caching.Memory".

Der folgende code verwendet [TryGetValue](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) zum Überprüfen, ob die aktuelle Uhrzeit im Cache befindet. Wenn das Element nicht zwischengespeichert wird, wird ein neuer Eintrag erstellt und hinzugefügt werden, in den Cache mit [festgelegt](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_).

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

Die aktuelle Uhrzeit und die zwischengespeicherten Zeit angezeigt:

[!code-html[Main](memory/sample/WebCache/Views/Home/Cache.cshtml)]

Die zwischengespeicherten `DateTime` Wert bleibt im Cache, während auf Anforderungen innerhalb des Zeitlimits (und keine Entfernung aufgrund von ungenügendem Arbeitsspeicher) vorhanden sind. Die folgende Abbildung zeigt die aktuelle Uhrzeit und eine frühere Zeit, die aus dem Cache abgerufen werden:

![Index-Ansicht mit zwei unterschiedlichen Zeitpunkten angezeigt](memory/_static/time.png)

Der folgende code verwendet [GetOrCreate](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) und [GetOrCreateAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) zum Zwischenspeichern von Daten. 

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

Der folgende code ruft [abrufen](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) die Cachezeit abgerufen:

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

Finden Sie unter [IMemoryCache Methoden](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache) und [CacheExtensions Methoden](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions) eine Beschreibung der Cache-Methoden.

## <a name="using-memorycacheentryoptions"></a>Verwenden von MemoryCacheEntryOptions

Im folgenden Beispiel:

- Legt die absolute Ablaufzeit. Dies ist die maximale Zeit, die der Eintrag zwischengespeichert werden kann und verhindert, dass das Element immer veraltet, wenn gleitende Ablauf fortlaufend verlängert wird.
- Legt eine gleitende Ablaufzeit fest. Anforderungen, die Zugriff auf dieses zwischengespeicherte Element werden die gleitende Ablaufdauer zurückgesetzt.
- Legt die Cachepriorität auf `CacheItemPriority.NeverRemove`. 
- Legt eine [PostEvictionDelegate](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.postevictiondelegate) , werden aufgerufen, nachdem der Eintrag aus dem Cache entfernt wird. Der Rückruf wird auf einem anderen Thread aus dem Code ausgeführt, die das Element aus dem Cache entfernt werden.

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-20)]

## <a name="cache-dependencies"></a>Cache-Abhängigkeiten

Das folgende Beispiel zeigt, wie Sie einen Eintrag im Cache ablaufen, wenn ein abhängiger Eintrag abläuft. Ein `CancellationChangeToken` das zwischengespeicherte Element hinzugefügt wird. Wenn `Cancel` aufgerufen wird, auf die `CancellationTokenSource`, beide Einträge im Cache entfernt werden. 

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

Mit einem `CancellationTokenSource` ermöglicht mehrere Cacheeinträge als Gruppe entfernt werden. Mit der `using` Muster im obigen Code, der Einträge im Cache erstellt innerhalb der `using` Block Trigger und ablaufeinstellungen erben.

### <a name="additional-notes"></a>Zusätzliche Hinweise

- Wenn einen Rückruf verwenden Sie ein Cacheelement neu auffüllen:

  - Mehrere Anforderungen erhalten die zwischengespeicherte Schlüsselwert leer, da der Rückruf abgeschlossen noch nicht. 
  - Dies kann in mehreren Threads, die für das streckungsschema an das zwischengespeicherte Element führen.

- Wenn ein Cacheeintrag verwendet wird, um eine andere zu erstellen, kopiert das untergeordnete Element, des übergeordnete Eintrags Ablauf-Token und zeitbasierte ablaufeinstellungen. Das untergeordnete Element ist nicht abgelaufen durch manuelles Entfernen oder aktualisieren, der des übergeordnete Eintrags.

### <a name="other-resources"></a>Weitere Ressourcen

* [Arbeiten mit einem verteilten Cache](distributed.md)
* [Antwort zwischenspeichern middleware](middleware.md)
