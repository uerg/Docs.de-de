---
title: Verteilte Cache Tag Helper | Microsoft Docs
author: pkellner
description: Zeigt die zum Arbeiten mit Cache-Tag-Hilfsprogramm
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: f5844dade218fdba1169a55fe3ce251a9cc03db2
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="distributed-cache-tag-helper"></a><span data-ttu-id="de3f9-103">Verteilter Cache-Tag-Hilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="de3f9-103">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="de3f9-104">Von [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="de3f9-104">By [Peter Kellner](http://peterkellner.net)</span></span> 


<span data-ttu-id="de3f9-105">Das verteilte Cache-Tag-Hilfsprogramm bietet die Möglichkeit, die Leistung Ihrer App ASP.NET Core erheblich zu verbessern, indem Sie dessen Inhalt an eine Quelle verteilter Cache caching.</span><span class="sxs-lookup"><span data-stu-id="de3f9-105">The Distributed Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to a distributed cache source.</span></span>

<span data-ttu-id="de3f9-106">Das verteilte Cache-Tag-Hilfsobjekt erbt von der gleichen Basisklasse als die Cache-Tag-Hilfsprogramm.</span><span class="sxs-lookup"><span data-stu-id="de3f9-106">The Distributed Cache Tag Helper inherits from the same base class as the Cache Tag Helper.</span></span>  <span data-ttu-id="de3f9-107">Alle Attribute, die dem Cache Tag Hilfsprogramm zugeordnete funktioniert auch auf das Tag-Hilfsobjekt verteilt.</span><span class="sxs-lookup"><span data-stu-id="de3f9-107">All attributes associated with the Cache Tag Helper will also work on the Distributed Tag Helper.</span></span>


<span data-ttu-id="de3f9-108">Das verteilte Cache-Tag-Hilfsobjekt folgt die **expliziten Abhängigkeiten Prinzip** genannt **Konstruktoreinfügung**.</span><span class="sxs-lookup"><span data-stu-id="de3f9-108">The Distributed Cache Tag Helper follows the **Explicit Dependencies Principle** known as **Constructor Injection**.</span></span>  <span data-ttu-id="de3f9-109">Insbesondere die `IDistributedCache` Schnittstelle Container an den verteilten Cache Tag Helfer-Konstruktor übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="de3f9-109">Specifically, the `IDistributedCache` interface container is passed into the Distributed Cache Tag Helper's constructor.</span></span>  <span data-ttu-id="de3f9-110">Wenn keine bestimmte konkrete Implementierung der `IDistributedCache` erstellt wurde, `ConfigureServices`, in der Regel in startup.cs gefunden und dann das verteilte Cache-Tag-Hilfsobjekt wird die gleiche in-Memory-Anbieter zum Speichern von zwischengespeicherten Daten als die grundlegende Cache-Tag-Hilfsprogramm verwenden.</span><span class="sxs-lookup"><span data-stu-id="de3f9-110">If no specific concrete implementation of `IDistributedCache` has been created in `ConfigureServices`, usually found in startup.cs, then the Distributed Cache Tag Helper will use the same in-memory provider for storing cached data as the basic Cache Tag Helper.</span></span>

## <a name="distributed-cache-tag-helper-attributes"></a><span data-ttu-id="de3f9-111">Verteilte Cache Tagattribute-Hilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="de3f9-111">Distributed Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a><span data-ttu-id="de3f9-112">läuft ab am läuft ab nach Ablauf-gleitende aktiviert variieren-by-Header durch eine Abfrage variieren variieren von Route variieren von Cookie variieren von Benutzer variieren-nach Priorität</span><span class="sxs-lookup"><span data-stu-id="de3f9-112">enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority</span></span>

<span data-ttu-id="de3f9-113">Definitionen finden Sie in der Cache-Tag-Hilfsprogramm.</span><span class="sxs-lookup"><span data-stu-id="de3f9-113">See Cache Tag Helper for definitions.</span></span> <span data-ttu-id="de3f9-114">Verteilte Cache Tag Helper erbt von derselben Klasse wie Cache-Tag-Hilfsprogramm, damit alle diese Attribute aus dem Cache Tag Helper gemeinsam verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="de3f9-114">Distributed Cache Tag Helper inherits from the same class as Cache Tag Helper so all these attributes are common from Cache Tag Helper.</span></span>

- - -

### <a name="name-required"></a><span data-ttu-id="de3f9-115">Name (erforderlich)</span><span class="sxs-lookup"><span data-stu-id="de3f9-115">name (required)</span></span>

| <span data-ttu-id="de3f9-116">Attributtyp</span><span class="sxs-lookup"><span data-stu-id="de3f9-116">Attribute Type</span></span>    | <span data-ttu-id="de3f9-117">Beispielwert</span><span class="sxs-lookup"><span data-stu-id="de3f9-117">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="de3f9-118">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="de3f9-118">string</span></span>    | <span data-ttu-id="de3f9-119">"my-distributed-cache-unique-key-101"</span><span class="sxs-lookup"><span data-stu-id="de3f9-119">"my-distributed-cache-unique-key-101"</span></span>     |

<span data-ttu-id="de3f9-120">Die erforderliche `name` Attribut als Schlüssel für diesen Cache gespeichert, die für jede Instanz eines verteilten Cache-Tag-Hilfsprogramms verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="de3f9-120">The required `name` attribute is used as a key to that cache stored for each instance of a Distributed Cache Tag Helper.</span></span>  <span data-ttu-id="de3f9-121">Im Gegensatz zu den grundlegenden Cache Tag-Hilfsprogramm, das einen Schlüssel jeder Cache Tag-Hilfsinstanz basierend auf den Namen der Razor-Seite und den Speicherort der Hilfsprogramm-Tag in der Seite "Razor" zuweist, beruht die verteilten Cache Tag Helper nur sie dessen Schlüssel für das Attribut`name`</span><span class="sxs-lookup"><span data-stu-id="de3f9-121">Unlike the basic Cache Tag Helper that assigns a key to each Cache Tag Helper instance based on the Razor page name and location of the tag helper in the razor page, the Distributed Cache Tag Helper only bases it's key on the attribute `name`</span></span>

<span data-ttu-id="de3f9-122">Verwendungsbeispiel:</span><span class="sxs-lookup"><span data-stu-id="de3f9-122">Usage Example:</span></span>

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a><span data-ttu-id="de3f9-123">Verteilte Cache Tag Helper IDistributedCache Implementierungen</span><span class="sxs-lookup"><span data-stu-id="de3f9-123">Distributed Cache Tag Helper IDistributedCache Implementations</span></span>

<span data-ttu-id="de3f9-124">Es gibt zwei Implementierungen von `IDistributedCache` in ASP.NET Core integriert.</span><span class="sxs-lookup"><span data-stu-id="de3f9-124">There are two implementations of `IDistributedCache` built in to ASP.NET Core.</span></span>  <span data-ttu-id="de3f9-125">Eine basiert auf **Sql Server** und die andere basiert auf **Redis**.</span><span class="sxs-lookup"><span data-stu-id="de3f9-125">One is based on **Sql Server** and the other is based on **Redis**.</span></span> <span data-ttu-id="de3f9-126">Details zu dieser Implementierung finden Sie unter der Ressource, die nachstehenden benannte "Arbeiten mit einem verteilten Cache".</span><span class="sxs-lookup"><span data-stu-id="de3f9-126">Details of these implementations can be found at the resource referenced below named "Working with a distributed cache".</span></span> <span data-ttu-id="de3f9-127">Beide Implementierungen umfassen Festlegen einer Instanz des `IDistributedCache` in ASP.NET Core des **startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="de3f9-127">Both implementations involve setting an instance of `IDistributedCache` in ASP.NET Core's **startup.cs**.</span></span>

<span data-ttu-id="de3f9-128">Stehen keine Tagattribute explizit verknüpft sind mit einer bestimmten Implementierung des `IDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="de3f9-128">There are no tag attributes specifically associated with using any specific implementation of `IDistributedCache`.</span></span>



- - -



## <a name="additional-resources"></a><span data-ttu-id="de3f9-129">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="de3f9-129">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
