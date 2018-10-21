---
title: Arbeiten Sie mit einem verteilten Cache in ASP.NET Core
author: ardalis
description: Erfahren Sie, wie Sie verteiltes Zwischenspeichern in ASP.NET Core verwenden können, um die App-Leistung und Skalierbarkeit zu verbessern, insbesondere in einer Cloud- oder Serverfarmumgebung.
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2017
uid: performance/caching/distributed
ms.openlocfilehash: 85da734f3ae7bcf0936888edfb6ac91d4362eef2
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477475"
---
# <a name="work-with-a-distributed-cache-in-aspnet-core"></a>Arbeiten Sie mit einem verteilten Cache in ASP.NET Core

Von [Steve Smith](https://ardalis.com/)

Verteilte Caches können die Leistung und Skalierbarkeit von ASP.NET Core-apps, verbessern, insbesondere, wenn in der Cloud oder einer Serverfarm gehostet.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-a-distributed-cache"></a>Was ist ein verteilter Cache

Ein verteilter Cache wird von mehreren app-Servern gemeinsam genutzt (finden Sie unter [Cache – Grundlagen](memory.md#caching-basics)). Die Informationen im Cache nicht im Speicher der einzelnen Webserver gespeichert, und die zwischengespeicherten Daten sind für alle von der app-Servern zur Verfügung. Dies bietet mehrere Vorteile:

1. Zwischengespeicherte Daten ist auf allen Webservern kohärent. Benutzer nicht unterschiedliche Ergebnisse angezeigt, je nachdem welche, die Web Server die Anforderung verarbeitet

2. Zwischengespeicherte Daten überdauert Web Server neu gestartet wurde und Bereitstellungen. Einzelne Webserver können entfernt oder hinzugefügt werden, ohne Auswirkungen auf den Cache werden.

3. Der quelldatenspeicher hat weniger Anforderungen, die für sie (als überhaupt mit mehrere in-Memory-Caches "oder" No cache).

> [!NOTE]
> Wenn Sie einen SQL Server Distributed Cache verwenden zu können, sind einige der Vorteile nur true, wenn Sie eine separate Datenbank-Instanz für den Cache als für die app Daten verwendet wird.

Z. B. einen Cache kann ein verteilter Cache einer app-Reaktionsfähigkeit, erheblich verbessern, da in der Regel Daten aus dem Cache viel schneller als aus einer relationalen Datenbank (oder Webdienst) abgerufen werden können.

Cache-Konfiguration ist implementierungsspezifisch. In diesem Artikel wird beschrieben, wie so konfigurieren Sie beide Redis und SQL Server verteilte caches. Unabhängig davon, welche Implementierung aktiviert ist, der die app interagiert mit dem Cache ein gemeinsames `IDistributedCache` Schnittstelle.

## <a name="the-idistributedcache-interface"></a>Die IDistributedCache-Schnittstelle

Die `IDistributedCache` -Schnittstelle enthält synchrone und asynchrone Methoden. Die Schnittstelle kann Elemente hinzugefügt, abgerufen und von der Implementierung des verteilten Cache entfernt werden. Die `IDistributedCache` Schnittstelle enthält die folgenden Methoden:

**Get, "getasync"**

Akzeptiert einen Zeichenfolgenschlüssel und ruft ein zwischengespeichertes Element als ein `byte[]` Wenn im Cache gefunden.

**Menge SetAsync**

Fügt ein Element hinzu (als `byte[]`) in den Cache mit einem Zeichenfolgenschlüssel.

**Aktualisierung der RefreshAsync**

Aktualisiert ein Element im Cache basierend auf seinen Schlüssel an, das Zurücksetzen von gleitenden Ablauf des Timeouts (sofern vorhanden).

**Remove, RemoveAsync**

Entfernt einen Eintrag im Cache basierend auf seinen Schlüssel an.

Verwenden der `IDistributedCache` Schnittstelle:

   1. Fügen Sie die erforderlichen NuGet-Pakete zu Ihrer Projektdatei hinzu.

   2. Konfigurieren Sie die konkrete Implementierung der `IDistributedCache` in Ihre `Startup` Klasse `ConfigureServices` -Methode, und fügen sie es dem Container hinzu.

   3. Von der app [Middleware](xref:fundamentals/middleware/index) oder MVC-Controller-Klassen, fordern Sie eine Instanz von `IDistributedCache` aus dem Konstruktor. Die Instanz erfolgt durch [Dependency Injection](../../fundamentals/dependency-injection.md) (DI).

> [!NOTE]
> Besteht keine Notwendigkeit für eine Singleton- oder Bereichsbezogene Lebensdauer verwenden `IDistributedCache` Instanzen (mindestens für die integrierten Implementierungen). Sie können auch eine Instanz erstellen, wenn Sie eine möglicherweise (anstelle von [Dependency Injection](../../fundamentals/dependency-injection.md)), aber dies kann Ihr Code schwieriger zu testen, und verstößt gegen die [Prinzip der expliziten Abhängigkeiten](http://deviq.com/explicit-dependencies-principle/).

Das folgende Beispiel zeigt, wie Sie mit einer Instanz von `IDistributedCache` in einer einfachen middlewarekomponente:

[!code-csharp[](distributed/sample/src/DistCacheSample/StartTimeHeader.cs)]

Im obigen Code wird der zwischengespeicherte Wert gelesen, jedoch nie geschrieben. In diesem Beispiel wird nur der Wert festgelegt, wenn ein Server startet, und nicht geändert. In einem Szenario mit mehreren Servern wird der letzte Server, starten Sie vorherige Werte überschrieben, die von anderen Servern festgelegt wurden. Die `Get` und `Set` Methoden verwenden die `byte[]` Typ. Aus diesem Grund der Zeichenfolgenwert muss konvertiert werden mithilfe von `Encoding.UTF8.GetString` (für `Get`) und `Encoding.UTF8.GetBytes` (für `Set`).

Der folgende code aus *"Startup.cs"* zeigt den Wert, der festgelegt wird:

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet1)]

Da `IDistributedCache` konfiguriert ist, der `ConfigureServices` -Methode, er kann die `Configure` Methode als Parameter. Als Parameter hinzufügen, können die konfigurierte Instanz über Dependency Injection bereitgestellt werden.

## <a name="using-a-redis-distributed-cache"></a>Mithilfe eines verteilten Redis-Caches

[Redis](https://redis.io/) ist ein open-Source-Speicher in-Memory-Daten, die häufig als ein verteilter Cache verwendet wird. Können Sie sie lokal, und Sie können konfigurieren, eine [Azure Redis Cache](https://azure.microsoft.com/services/cache/) für Ihre Azure gehostete ASP.NET Core-apps. Ihrer ASP.NET Core-app konfiguriert, den Cache-Implementierung mit einer `RedisDistributedCache` Instanz.

Der Redis-Cache erfordert [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis/)

Sie konfigurieren, dass die Redis-Implementierung in `ConfigureServices` und im app-Code darauf zugreifen, durch die Anforderung einer Instanz von `IDistributedCache` (Siehe den Code oben).

Im Beispielcode wird ein `RedisCache` Implementierung wird verwendet, wenn der Server, für konfiguriert ist eine `Staging` Umgebung. Daher die `ConfigureStagingServices` -Methode konfiguriert die `RedisCache`:

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet2)]

Um Redis auf Ihrem lokalen Computer zu installieren, installieren Sie das chocolatey-Paket [ https://chocolatey.org/packages/redis-64/ ](https://chocolatey.org/packages/redis-64/) , und führen Sie `redis-server` über eine Eingabeaufforderung.

## <a name="using-a-sql-server-distributed-cache"></a>Verwenden eines SQL Server verteilte Caches

Die SqlServerCache-Implementierung ermöglicht den verteilten Cache, die SQL Server-Datenbank als Sicherungsspeicher verwendet. Zum Erstellen von SQL Server erstellt die Tabelle, die Sie Sql-Cache-Tool, das Tool verwenden, können eine Tabelle mit dem Namen und das Schema, die Sie angeben.

::: moniker range="< aspnetcore-2.1"

Hinzufügen `SqlConfig.Tools` auf die `<ItemGroup>` Element der Projektdatei und Ausführung `dotnet restore`.

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.Extensions.Caching.SqlConfig.Tools" 
                          Version="2.0.2" />
</ItemGroup>
```

::: moniker-end

Testen Sie SqlConfig.Tools, indem Sie den folgenden Befehl ausführen:

```console
dotnet sql-cache create --help
```

SqlConfig.Tools zeigt Nutzung, Optionen und zu Befehlen.

Erstellen Sie eine Tabelle in SQL Server durch Ausführen der `sql-cache create` Befehl:

```console
dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
Table and index were created successfully.
```

Die erstellte Tabelle weist das folgende Schema:

![SQL Server-Cache-Tabelle](distributed/_static/SqlServerCacheTable.png)

Wie alle cacheimplementierungen, Ihre app abrufen und Festlegen von Cache-Werten, die mit einer Instanz von `IDistributedCache`, sondern eine `SqlServerCache`. Das Beispiel implementiert `SqlServerCache` in der produktionsumgebung (also die Konfiguration in `ConfigureProductionServices`).

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet3)]

> [!NOTE]
> Die `ConnectionString` (und optional `SchemaName` und `TableName`) in der Regel außerhalb der quellcodeverwaltung (z. B. "usersecrets"), gespeichert werden sollen, wie sie die Anmeldeinformationen enthalten können.

## <a name="recommendations"></a>Empfehlungen

Bei der Entscheidung, welche Implementierung der `IDistributedCache` entscheiden Sie für Ihre app, Redis und SQL Server auf Grundlage der vorhandenen Infrastruktur und Umgebung, Ihren leistungsanforderungen und Ihres Teams Erfahrung. Wenn Ihr Team mehr gern mit Redis ist, ist es eine hervorragende Wahl. Wenn Ihr Team über SQL Server bevorzugt, können Sie auch, dass diese Implementierung davon überzeugt sein. Beachten Sie, dass eine herkömmliche Cachinglösung Daten im Arbeitsspeicher speichert, die für den schnellen Abruf der Daten ermöglicht. Sie sollten häufig verwendete Daten in einem Cache speichern, und speichern die gesamten Daten in einem Back-End-permanenten Speicher wie SQL Server oder Azure Storage. Redis-Cache ist eine Cachinglösung, die Ihnen im Vergleich zu SQL-Cache hohe Durchsatzrate mit geringer Latenz bietet.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Redis-Cache in Azure](https://azure.microsoft.com/documentation/services/redis-cache/)
* [SQL­Datenbank in Azure](https://azure.microsoft.com/documentation/services/sql-database/)
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
