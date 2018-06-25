---
title: Taghilfsprogramm für verteilten Cache in ASP.NET Core
author: pkellner
description: Veranschaulicht die Arbeit mit dem Cache-Taghilfsprogramm
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: d33c22802030eb9bc77baa64b83c9bbd7e902195
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275174"
---
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a>Taghilfsprogramm für verteilten Cache in ASP.NET Core

Von [Peter Kellner](http://peterkellner.net) 

Durch das Taghilfsprogramm für verteilten Cache kann die Leistung Ihrer ASP.NET Core-App erheblich verbessert werden, indem deren Inhalte in einer verteilten Cachequelle zwischengespeichert werden.

Das Taghilfsprogramm für verteilten Cache erbt von derselben Basisklasse wie das Cache-Taghilfsprogramm. Alle Attribute, die dem Cache-Taghilfsprogramm zugeordnet sind, funktionieren auch für das Taghilfsprogramm für verteilten Cache.

Das Taghilfsprogramm für verteilten Cache folgt dem **Prinzip der expliziten Abhängigkeiten**, das auch als **Constructor Injection** bezeichnet wird. Insbesondere der `IDistributedCache`-Schnittstellencontainer wird an den Konstruktor des Taghilfsprogramms für verteilten Cache übergeben. Wenn keine bestimmte konkrete Implementierung von `IDistributedCache` in `ConfigureServices` erstellt wurde (diese befindet sich üblicherweise in „startup.cs“), verwendet das Taghilfsprogramm für verteilten Cache zum Speichern von zwischengespeicherten Daten denselben In-Memory-Anbieter wie das grundlegende Cache-Taghilfsprogramm.

## <a name="distributed-cache-tag-helper-attributes"></a>Attribute des Taghilfsprogramms für verteilten Cache

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a>enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority

Definitionen finden Sie im Cache-Taghilfsprogramm. Das Taghilfsprogramm für verteilten Cache erbt von derselben Klasse wie das Cache-Taghilfsprogramm, sodass diese Attribute mit denen des Cache-Taghilfsprogramms übereinstimmen.

- - -

### <a name="name-required"></a>Name (erforderlich)

| Attributtyp    | Beispielwert     |
|----------------   |----------------   |
| Zeichenfolge    | „my-distributed-cache-unique-key-101“     |

Das erforderliche `name`-Attribut wird als Schlüssel für den Cache verwendet, der für jede Instanz eines Taghilfsprogramms für verteilten Cache gespeichert wird. Im Gegensatz zum Cache-Taghilfsprogramm, das jeder Instanz des Cache-Taghilfsprogramms basierend auf dem Namen der Razor-Seite und dem Speicherort des Taghilfsprogramms auf der Razor-Seite einen Schlüssel zuweist, basieren die Schlüssel des Taghilfsprogramms für verteilten Cache nur auf dem Attribut `name`.

Beispiel für die Verwendung:

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a>Implementierungen von IDistributedCache im Taghilfsprogramm für verteilten Cache

In ASP.NET Core gibt es zwei Implementierungen von `IDistributedCache`. Eine basiert auf SQL Server, die andere auf Redis. Ausführliche Informationen zu diesen Implementierungen finden Sie unter <xref:performance/caching/distributed>. Für beide Implementierungen wird eine Instanz von `IDistributedCache` in der Datei *startup.cs* in ASP.NET Core festgelegt.

Es gibt keine Tagattribute, die einer bestimmten Implementierung von `IDistributedCache` explizit zugeordnet sind.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
