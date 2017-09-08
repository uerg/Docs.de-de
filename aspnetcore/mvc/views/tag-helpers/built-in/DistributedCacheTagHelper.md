---
title: Verteilte Cache Tag Helper | Microsoft Docs
author: pkellner
description: Zeigt die zum Arbeiten mit Cache-Tag-Hilfsprogramm
keywords: ASP.NET Core, Tag-Hilfsprogramm
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a022
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/DistributedCacheTagHelper
ms.openlocfilehash: b6e0beca0833b1dbe0843e8f8848b976726cc7b0
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
# <a name="distributed-cache-tag-helper"></a><span data-ttu-id="98272-104">Verteilter Cache-Tag-Hilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="98272-104">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="98272-105">Durch [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="98272-105">By [Peter Kellner](http://peterkellner.net)</span></span> 


<span data-ttu-id="98272-106">Das verteilte Cache-Tag-Hilfsprogramm bietet die Möglichkeit, die Leistung Ihrer App ASP.NET Core erheblich zu verbessern, indem Sie dessen Inhalt an eine Quelle verteilter Cache caching.</span><span class="sxs-lookup"><span data-stu-id="98272-106">The Distributed Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to a distributed cache source.</span></span>

<span data-ttu-id="98272-107">Das verteilte Cache-Tag-Hilfsobjekt erbt von der gleichen Basisklasse als die Cache-Tag-Hilfsprogramm.</span><span class="sxs-lookup"><span data-stu-id="98272-107">The Distributed Cache Tag Helper inherits from the same base class as the Cache Tag Helper.</span></span>  <span data-ttu-id="98272-108">Alle Attribute, die dem Cache Tag Hilfsprogramm zugeordnete funktioniert auch auf das Tag-Hilfsobjekt verteilt.</span><span class="sxs-lookup"><span data-stu-id="98272-108">All attributes associated with the Cache Tag Helper will also work on the Distributed Tag Helper.</span></span>


<span data-ttu-id="98272-109">Das verteilte Cache-Tag-Hilfsobjekt folgt die **expliziten Abhängigkeiten Prinzip** genannt **Konstruktoreinfügung**.</span><span class="sxs-lookup"><span data-stu-id="98272-109">The Distributed Cache Tag Helper follows the **Explicit Dependencies Principle** known as **Constructor Injection**.</span></span>  <span data-ttu-id="98272-110">Insbesondere die `IDistributedCache` Schnittstelle Container an den verteilten Cache Tag Helfer-Konstruktor übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="98272-110">Specifically, the `IDistributedCache` interface container is passed into the Distributed Cache Tag Helper's constructor.</span></span>  <span data-ttu-id="98272-111">Wenn keine bestimmte konkrete Implementierung der `IDistributedCache` erstellt wurde, `ConfigureServices`, in der Regel in startup.cs gefunden und dann das verteilte Cache-Tag-Hilfsobjekt wird die gleiche in-Memory-Anbieter zum Speichern von zwischengespeicherten Daten als die grundlegende Cache-Tag-Hilfsprogramm verwenden.</span><span class="sxs-lookup"><span data-stu-id="98272-111">If no specific concrete implementation of `IDistributedCache` has been created in `ConfigureServices`, usually found in startup.cs, then the Distributed Cache Tag Helper will use the same in-memory provider for storing cached data as the basic Cache Tag Helper.</span></span>

## <a name="distributed-cache-tag-helper-attributes"></a><span data-ttu-id="98272-112">Verteilte Cache Tagattribute-Hilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="98272-112">Distributed Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a><span data-ttu-id="98272-113">läuft ab am läuft ab nach Ablauf-gleitende aktiviert variieren-by-Header durch eine Abfrage variieren variieren von Route variieren von Cookie variieren von Benutzer variieren-nach Priorität</span><span class="sxs-lookup"><span data-stu-id="98272-113">enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority</span></span>

<span data-ttu-id="98272-114">Definitionen finden Sie in der Cache-Tag-Hilfsprogramm.</span><span class="sxs-lookup"><span data-stu-id="98272-114">See Cache Tag Helper for definitions.</span></span> <span data-ttu-id="98272-115">Verteilte Cache Tag Helper erbt von derselben Klasse wie Cache-Tag-Hilfsprogramm, damit alle diese Attribute aus dem Cache Tag Helper gemeinsam verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="98272-115">Distributed Cache Tag Helper inherits from the same class as Cache Tag Helper so all these attributes are common from Cache Tag Helper.</span></span>

- - -

### <a name="name-required"></a><span data-ttu-id="98272-116">Name (erforderlich)</span><span class="sxs-lookup"><span data-stu-id="98272-116">name (required)</span></span>

| <span data-ttu-id="98272-117">Attributtyp</span><span class="sxs-lookup"><span data-stu-id="98272-117">Attribute Type</span></span>    | <span data-ttu-id="98272-118">Beispielwert</span><span class="sxs-lookup"><span data-stu-id="98272-118">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="98272-119">string</span><span class="sxs-lookup"><span data-stu-id="98272-119">string</span></span>    | <span data-ttu-id="98272-120">"my-distributed-cache-unique-key-101"</span><span class="sxs-lookup"><span data-stu-id="98272-120">"my-distributed-cache-unique-key-101"</span></span>     |

<span data-ttu-id="98272-121">Die erforderliche `name` Attribut als Schlüssel für diesen Cache gespeichert, die für jede Instanz eines verteilten Cache-Tag-Hilfsprogramms verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="98272-121">The required `name` attribute is used as a key to that cache stored for each instance of a Distributed Cache Tag Helper.</span></span>  <span data-ttu-id="98272-122">Im Gegensatz zu den grundlegenden Cache Tag-Hilfsprogramm, das einen Schlüssel jeder Cache Tag-Hilfsinstanz basierend auf den Namen der Razor-Seite und den Speicherort der Hilfsprogramm-Tag in der Seite "Razor" zuweist, beruht die verteilten Cache Tag Helper nur sie dessen Schlüssel für das Attribut`name`</span><span class="sxs-lookup"><span data-stu-id="98272-122">Unlike the basic Cache Tag Helper that assigns a key to each Cache Tag Helper instance based on the Razor page name and location of the tag helper in the razor page, the Distributed Cache Tag Helper only bases it's key on the attribute `name`</span></span>

<span data-ttu-id="98272-123">Verwendungsbeispiel:</span><span class="sxs-lookup"><span data-stu-id="98272-123">Usage Example:</span></span>

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a><span data-ttu-id="98272-124">Verteilte Cache Tag Helper IDistributedCache Implementierungen</span><span class="sxs-lookup"><span data-stu-id="98272-124">Distributed Cache Tag Helper IDistributedCache Implementations</span></span>

<span data-ttu-id="98272-125">Es gibt zwei Implementierungen von `IDistributedCache` in ASP.NET Core integriert.</span><span class="sxs-lookup"><span data-stu-id="98272-125">There are two implementations of `IDistributedCache` built in to ASP.NET Core.</span></span>  <span data-ttu-id="98272-126">Eine basiert auf **Sql Server** und die andere basiert auf **Redis**.</span><span class="sxs-lookup"><span data-stu-id="98272-126">One is based on **Sql Server** and the other is based on **Redis**.</span></span> <span data-ttu-id="98272-127">Details zu dieser Implementierung finden Sie unter der Ressource, die nachstehenden benannte "Arbeiten mit einem verteilten Cache".</span><span class="sxs-lookup"><span data-stu-id="98272-127">Details of these implementations can be found at the resource referenced below named "Working with a distributed cache".</span></span> <span data-ttu-id="98272-128">Beide Implementierungen umfassen Festlegen einer Instanz des `IDistributedCache` in ASP.NET Core des **startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="98272-128">Both implementations involve setting an instance of `IDistributedCache` in ASP.NET Core's **startup.cs**.</span></span>

<span data-ttu-id="98272-129">Gibt es keine Tagattribute explizit mit einer bestimmten Implementierung des verknüpft sind `IDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="98272-129">There no tag attributes specifically associated with using any specific implementation of `IDistributedCache`.</span></span>



- - -



## <a name="additional-resources"></a><span data-ttu-id="98272-130">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="98272-130">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/builtin-th/CacheTagHelper>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>