---
title: Taghilfsprogramm für verteilten Cache in ASP.NET Core
author: pkellner
description: Veranschaulicht die Arbeit mit dem Cache-Taghilfsprogramm
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: a35a5795c086273e773c613c483fc6343c694bf2
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342171"
---
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a><span data-ttu-id="89451-103">Taghilfsprogramm für verteilten Cache in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="89451-103">Distributed Cache Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="89451-104">Von [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="89451-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="89451-105">Durch das Taghilfsprogramm für verteilten Cache kann die Leistung Ihrer ASP.NET Core-App erheblich verbessert werden, indem deren Inhalte in einer verteilten Cachequelle zwischengespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="89451-105">The Distributed Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to a distributed cache source.</span></span>

<span data-ttu-id="89451-106">Das Taghilfsprogramm für verteilten Cache erbt von derselben Basisklasse wie das Cache-Taghilfsprogramm.</span><span class="sxs-lookup"><span data-stu-id="89451-106">The Distributed Cache Tag Helper inherits from the same base class as the Cache Tag Helper.</span></span> <span data-ttu-id="89451-107">Alle Attribute, die dem Cache-Taghilfsprogramm zugeordnet sind, funktionieren auch für das Taghilfsprogramm für verteilten Cache.</span><span class="sxs-lookup"><span data-stu-id="89451-107">All attributes associated with the Cache Tag Helper will also work on the Distributed Tag Helper.</span></span>

<span data-ttu-id="89451-108">Das Taghilfsprogramm für verteilten Cache folgt dem **Prinzip der expliziten Abhängigkeiten**, das auch als **Constructor Injection** bezeichnet wird.</span><span class="sxs-lookup"><span data-stu-id="89451-108">The Distributed Cache Tag Helper follows the **Explicit Dependencies Principle** known as **Constructor Injection**.</span></span> <span data-ttu-id="89451-109">Insbesondere der `IDistributedCache`-Schnittstellencontainer wird an den Konstruktor des Taghilfsprogramms für verteilten Cache übergeben.</span><span class="sxs-lookup"><span data-stu-id="89451-109">Specifically, the `IDistributedCache` interface container is passed into the Distributed Cache Tag Helper's constructor.</span></span> <span data-ttu-id="89451-110">Wenn keine bestimmte konkrete Implementierung von `IDistributedCache` in `ConfigureServices` erstellt wurde (diese befindet sich üblicherweise in „startup.cs“), verwendet das Taghilfsprogramm für verteilten Cache zum Speichern von zwischengespeicherten Daten denselben In-Memory-Anbieter wie das grundlegende Cache-Taghilfsprogramm.</span><span class="sxs-lookup"><span data-stu-id="89451-110">If no specific concrete implementation of `IDistributedCache` has been created in `ConfigureServices`, usually found in startup.cs, then the Distributed Cache Tag Helper will use the same in-memory provider for storing cached data as the basic Cache Tag Helper.</span></span>

## <a name="distributed-cache-tag-helper-attributes"></a><span data-ttu-id="89451-111">Attribute des Taghilfsprogramms für verteilten Cache</span><span class="sxs-lookup"><span data-stu-id="89451-111">Distributed Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a><span data-ttu-id="89451-112">enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority</span><span class="sxs-lookup"><span data-stu-id="89451-112">enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority</span></span>

<span data-ttu-id="89451-113">Definitionen finden Sie im Cache-Taghilfsprogramm.</span><span class="sxs-lookup"><span data-stu-id="89451-113">See Cache Tag Helper for definitions.</span></span> <span data-ttu-id="89451-114">Das Taghilfsprogramm für verteilten Cache erbt von derselben Klasse wie das Cache-Taghilfsprogramm, sodass diese Attribute mit denen des Cache-Taghilfsprogramms übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="89451-114">Distributed Cache Tag Helper inherits from the same class as Cache Tag Helper so all these attributes are common from Cache Tag Helper.</span></span>

- - -

### <a name="name-required"></a><span data-ttu-id="89451-115">Name (erforderlich)</span><span class="sxs-lookup"><span data-stu-id="89451-115">name (required)</span></span>

| <span data-ttu-id="89451-116">Attributtyp</span><span class="sxs-lookup"><span data-stu-id="89451-116">Attribute Type</span></span>    | <span data-ttu-id="89451-117">Beispielwert</span><span class="sxs-lookup"><span data-stu-id="89451-117">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="89451-118">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="89451-118">string</span></span>    | <span data-ttu-id="89451-119">„my-distributed-cache-unique-key-101“</span><span class="sxs-lookup"><span data-stu-id="89451-119">"my-distributed-cache-unique-key-101"</span></span>     |

<span data-ttu-id="89451-120">Das erforderliche `name`-Attribut wird als Schlüssel für den Cache verwendet, der für jede Instanz eines Taghilfsprogramms für verteilten Cache gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="89451-120">The required `name` attribute is used as a key to that cache stored for each instance of a Distributed Cache Tag Helper.</span></span> <span data-ttu-id="89451-121">Im Gegensatz zum Cache-Taghilfsprogramm, das jeder Instanz des Cache-Taghilfsprogramms basierend auf dem Namen der Razor-Seite und dem Speicherort des Taghilfsprogramms auf der Razor-Seite einen Schlüssel zuweist, basieren die Schlüssel des Taghilfsprogramms für verteilten Cache nur auf dem Attribut `name`.</span><span class="sxs-lookup"><span data-stu-id="89451-121">Unlike the basic Cache Tag Helper that assigns a key to each Cache Tag Helper instance based on the Razor page name and location of the Tag Helper in the razor page, the Distributed Cache Tag Helper only bases its key on the attribute `name`</span></span>

<span data-ttu-id="89451-122">Beispiel für die Verwendung:</span><span class="sxs-lookup"><span data-stu-id="89451-122">Usage Example:</span></span>

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a><span data-ttu-id="89451-123">Implementierungen von IDistributedCache im Taghilfsprogramm für verteilten Cache</span><span class="sxs-lookup"><span data-stu-id="89451-123">Distributed Cache Tag Helper IDistributedCache implementations</span></span>

<span data-ttu-id="89451-124">In ASP.NET Core gibt es zwei Implementierungen von `IDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="89451-124">There are two implementations of `IDistributedCache` built in to ASP.NET Core.</span></span> <span data-ttu-id="89451-125">Eine basiert auf SQL Server, die andere auf Redis.</span><span class="sxs-lookup"><span data-stu-id="89451-125">One is based on SQL Server and the other is based on Redis.</span></span> <span data-ttu-id="89451-126">Ausführliche Informationen zu diesen Implementierungen finden Sie unter <xref:performance/caching/distributed>.</span><span class="sxs-lookup"><span data-stu-id="89451-126">Details of these implementations can be found at <xref:performance/caching/distributed>.</span></span> <span data-ttu-id="89451-127">Für beide Implementierungen wird eine Instanz von `IDistributedCache` in der Datei *startup.cs* in ASP.NET Core festgelegt.</span><span class="sxs-lookup"><span data-stu-id="89451-127">Both implementations involve setting an instance of `IDistributedCache` in ASP.NET Core's *Startup.cs*.</span></span>

<span data-ttu-id="89451-128">Es gibt keine Tagattribute, die einer bestimmten Implementierung von `IDistributedCache` explizit zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="89451-128">There are no tag attributes specifically associated with using any specific implementation of `IDistributedCache`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="89451-129">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="89451-129">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
