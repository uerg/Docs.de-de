---
title: Arbeiten mit einem verteilten Cache in ASP.NET Core
author: ardalis
description: Erfahren Sie, wie verteilte Zwischenspeicherung verwenden, um die Leistung und Skalierbarkeit von ASP.NET Core-apps zu verbessern, besonders bei der in eine Cloud oder Server-Umgebung gehostet.
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: performance/caching/distributed
ms.openlocfilehash: 877a3e1f8c3282fdd67a389ddf5b4ff49dea3b42
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
---
# <a name="working-with-a-distributed-cache-in-aspnet-core"></a>Arbeiten mit einem verteilten Cache in ASP.NET Core

Durch [Steve Smith](https://ardalis.com/)

Verteilte Caches können die Leistung und Skalierbarkeit von ASP.NET Core-apps verbessern, besonders bei der in eine Cloud oder Server-Umgebung gehostet. Dieser Artikel beschreibt das Arbeiten mit ASP.NET Core des integrierten verteilter Cache Speicherabstraktionen und die Implementierungen.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-a-distributed-cache"></a>Was ist ein verteilter Cache

Ein verteilter Cache wird von mehreren app-Servern gemeinsam genutzt (finden Sie unter [Zwischenspeichern Grundlagen](memory.md#caching-basics)). Die Informationen im Cache befindet sich nicht im Speicher der einzelnen Webserver gespeichert, und die zwischengespeicherten Daten für alle von der app-Servern verfügbar ist. Dies bietet mehrere Vorteile:

1. Zwischengespeicherte Daten sind auf allen Webservern kohärente. Benutzer nicht unterschiedliche Ergebnisse angezeigt, je nachdem welche, die Web Server seine Anforderung behandelt

2. Zwischengespeicherte Daten überdauert Web-Server neu gestartet wurde und Bereitstellungen. Einzelne Webserver können entfernt oder ohne Auswirkungen auf den Cache hinzugefügt werden.

3. Der Datenspeicher für die Quelle hat weniger Anforderungen an ihn (nicht mehrere in-Memory-Caches "oder" No überhaupt-cache).

> [!NOTE]
> Wenn Sie ein verteilter Cache von SQL Server verwenden, sind einige der Vorteile nur "true", wenn eine separate Datenbank-Instanz für den Cache als Quelldaten für die app verwendet wird.

Wie alle Cache möglicherweise ein verteilter Cache einer app-Reaktionsfähigkeit erheblich verbessert, da in der Regel Daten aus dem Cache viel schneller als aus einer relationalen Datenbank (oder Webdienst) abgerufen werden können.

Cachekonfiguration ist implementierungsspezifisch. Dieser Artikel beschreibt, wie so konfigurieren Sie beide Redis und verteilten SQL Server-caches. Unabhängig davon, welche Implementierung ausgewählt ist, interagiert die app mit dem Cache ein gemeinsames `IDistributedCache` Schnittstelle.

## <a name="the-idistributedcache-interface"></a>Die IDistributedCache-Schnittstelle

Die `IDistributedCache` -Schnittstelle enthält synchrone und asynchrone Methoden. Die Schnittstelle kann Elemente hinzugefügt, abgerufen und von der Implementierung verteilter Cache entfernt werden. Die `IDistributedCache` Schnittstelle enthält die folgenden Methoden:

**Get, GetAsync**

Akzeptiert einen Zeichenfolgenschlüssel und ruft ein zwischengespeichertes Element als ein `byte[]` Wenn im Cache gefunden.

**Set, SetAsync**

Fügt ein Element hinzu (als `byte[]`) in den Cache mithilfe eines Schlüssels Zeichenfolge.

**Refresh, RefreshAsync**

Aktualisiert ein Element im Cache basierend auf seinen Schlüssel zurücksetzen gleitenden Ablauf des Timeouts (sofern vorhanden).

**Remove, RemoveAsync**

Entfernt einen Cacheeintrag basierend auf seinen Schlüssel.

Verwenden der `IDistributedCache` Schnittstelle:

   1. Fügen Sie der Projektdatei die erforderlichen NuGet-Pakete hinzu.

   2. Konfigurieren Sie die spezielle Implementierung der `IDistributedCache` in Ihrer `Startup` Klasse `ConfigureServices` -Methode, und fügen sie es den Container hinzu.

   3. Aus der app [Middleware](xref:fundamentals/middleware/index) oder MVC-Controller-Klassen, bitten Sie eine Instanz von `IDistributedCache` aus dem Konstruktor. Die Instanz gebotenen [Abhängigkeitsinjektion](../../fundamentals/dependency-injection.md) (DI).

> [!NOTE]
> Besteht keine Notwendigkeit für eine Singleton oder ausgelegte Lebensdauer verwendet `IDistributedCache` Instanzen (mindestens für die integrierte Implementierungen). Sie können auch eine Instanz erstellen, wo Sie benötigen ein möglicherweise (anstatt [Abhängigkeitsinjektion](../../fundamentals/dependency-injection.md)), Dadurch könnte jedoch Code schwieriger zu testen, und gegen die [expliziten Abhängigkeiten Prinzip](http://deviq.com/explicit-dependencies-principle/).

Das folgende Beispiel zeigt, wie Sie eine Instanz von `IDistributedCache` in einer einfachen middlewarekomponente:

[!code-csharp[Main](./distributed/sample/src/DistCacheSample/StartTimeHeader.cs?highlight=15,18,21,27,28,29,30,31)]

Im obigen Code wird der zwischengespeicherte Wert gelesen, jedoch nie geschrieben. In diesem Beispiel wird der Wert nur festgelegt, wenn ein Server wird gestartet und nicht geändert. In einem Szenario mit mehreren Servern überschreibt der letzte Server, starten Sie alle vorherigen Werte, die von anderen Servern festgelegt wurden. Die `Get` und `Set` Methoden verwenden, die `byte[]` Typ. Aus diesem Grund der String-Wert muss konvertiert werden mit `Encoding.UTF8.GetString` (für `Get`) und `Encoding.UTF8.GetBytes` (für `Set`).

Der folgende code in *Startup.cs* zeigt den Wert festgelegt wird:

[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=2,4,5,6&range=58-66)]

> [!NOTE]
> Seit `IDistributedCache` konfiguriert ist, der `ConfigureServices` Methode, es ist verfügbar, die `Configure` Methode als Parameter. Als Parameter hinzufügen, können die konfigurierte Instanz DI bereitgestellt werden.

## <a name="using-a-redis-distributed-cache"></a>Verwenden einen verteilte Redis-cache

[Redis](https://redis.io/) ist ein open Source-Daten im Arbeitsspeicher speichert, das häufig als verteilte Cache verwendet wird. Lokal verwenden, und Sie können konfigurieren, ein [Azure Redis Cache](https://azure.microsoft.com/services/cache/) für Ihre Azure gehosteten ASP.NET Core-apps. Ihre app ASP.NET Core konfiguriert die Cache-Implementierung mit einem `RedisDistributedCache` Instanz.

Sie konfigurieren die Redis-Implementierung in `ConfigureServices` und im app-Code darauf zugreifen, durch die Anforderung von einer Instanz von `IDistributedCache` (Siehe obigen Code).

Im Beispielcode wird ein `RedisCache` Implementierung wird verwendet, wenn die Serverkonfiguration für einen `Staging` Umgebung. Daher die `ConfigureStagingServices` Methode konfiguriert die `RedisCache`:

[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=8,9,10,11,12,13&range=27-40)]

> [!NOTE]
> Um Redis auf dem lokalen Computer zu installieren, installieren Sie das Paket chocolatey [https://chocolatey.org/packages/redis-64/](https://chocolatey.org/packages/redis-64/) und führen Sie `redis-server` über eine Eingabeaufforderung.

## <a name="using-a-sql-server-distributed-cache"></a>Mithilfe eines SQL Server verteilte Caches

Die Implementierung SqlServerCache ermöglicht verteilten Cache eine SQL Server-Datenbank als Sicherungsspeicher verwendet. Zum Erstellen von SQL Server erstellt die Tabelle, die Sie Sql-Cache-Tool, das Tool verwenden, können eine Tabelle mit dem Namen und das Schema, die Sie angeben.

Um die Sql-Cache-Tool verwenden zu können, fügen `SqlConfig.Tools` auf die `<ItemGroup>` Element von der *csproj* Datei und ein Dotnet-Wiederherstellung ausführen.

[!code-xml[Main](./distributed/sample/src/DistCacheSample/DistCacheSample.csproj?range=23-25)]

Testen Sie SqlConfig.Tools, indem Sie den folgenden Befehl ausführen

```none
C:\DistCacheSample\src\DistCacheSample>dotnet sql-cache create --help
   ```

SQL-Cache-Tool wird Auslastung, Optionen und Befehl Hilfe angezeigt, nun Sie Tabellen in SQLServer erstellen können "Sql-Cache erstellen"-Befehl ausführen:

```none
C:\DistCacheSample\src\DistCacheSample>dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
   info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
       Table and index were created successfully.
   ```

Die erstellte Tabelle weist das folgende Schema:

![SQL Server-Cache-Tabelle](distributed/_static/SqlServerCacheTable.png)

Wie alle Cache Implementierungen Ihrer app abrufen und Festlegen von cachewerte, die mit einer Instanz von soll `IDistributedCache`, sondern eine `SqlServerCache`. Das Beispiel implementiert `SqlServerCache` in der `Production` Umgebung (damit die Konfiguration in `ConfigureProductionServices`).

[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=7,8,9,10,11,12&range=42-56)]

> [!NOTE]
> Die `ConnectionString` (und optional `SchemaName` und `TableName`) in der Regel außerhalb von Datenquellen-Steuerelements (z. B. usersecrets.) gespeichert werden sollen, da sie möglicherweise die Anmeldeinformationen enthalten.

## <a name="recommendations"></a>Empfehlungen

Wenn Sie entscheiden, welche Implementierung der `IDistributedCache` Recht für der app, und wählen Sie zwischen Redis und SQL Server auf Grundlage der vorhandenen Infrastruktur und Umgebung, Ihren leistungsanforderungen und Erfahrung Ihres Teams ist. Wenn Ihr Team nachrichtenorientierten arbeiten mit Redis ist, ist es eine hervorragende Wahl. Wenn vom Team auf SQL Server, können Sie sich, dass diese Implementierung als auch sicher sein. Beachten Sie, dass eine herkömmliche Cachinglösung Daten im Arbeitsspeicher speichert ermöglicht eine schnelle Abrufen von Daten. Häufig verwendete Daten in einem Cache gespeichert, und speichern Sie die gesamten Daten in einem permanenten Back-End-Speicher, z. B. SQL Server oder Azure-Speicher. Redis-Cache ist eine Cachinglösung, die einen hohen Durchsatz und niedriger Latenz im Vergleich zu SQL-Cache enthält.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Redis-Cache für Azure](https://azure.microsoft.com/documentation/services/redis-cache/)
* [SQL­Datenbank in Azure](https://azure.microsoft.com/documentation/services/sql-database/)
* [Zwischenspeicherung im Speicher](xref:performance/caching/memory)
* [Erkennen von Änderungen mit Änderungstoken](xref:fundamentals/primitives/change-tokens)
* [Zwischenspeichern von Antworten](xref:performance/caching/response)
* [Antworten zwischenspeichernde Middleware](xref:performance/caching/middleware)
* [Cache-Taghilfsprogramm](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Taghilfsprogramm für verteilten Cache](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
