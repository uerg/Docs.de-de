---
title: Verwenden von Web-API-Konventionen
author: pranavkm
description: Informationen zu Web-API-Konventionen in ASP.NET Core
monikerRange: '>= aspnetcore-2.2'
ms.author: pranavkm
ms.custom: mvc
ms.date: 11/13/2018
uid: web-api/advanced/conventions
ms.openlocfilehash: ede9a46c160cf6a49aa93da710af0bf0b8f59acc
ms.sourcegitcommit: c4572be5ebb301013a5698caf9b5572b76cb2e34
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/30/2018
ms.locfileid: "52710074"
---
# <a name="use-web-api-conventions"></a><span data-ttu-id="85a9b-103">Verwenden von Web-API-Konventionen</span><span class="sxs-lookup"><span data-stu-id="85a9b-103">Use web API conventions</span></span>

<span data-ttu-id="85a9b-104">Mit ASP.NET Core 2.2 wird die Möglichkeit eingeführt, allgemeine [API-Dokumentation](xref:tutorials/web-api-help-pages-using-swagger) zu extrahieren und diese auf mehrere Aktionen, Controller sowie alle Controller in einer Assembly anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="85a9b-104">ASP.NET Core 2.2 introduces a way to extract common [API documentation](xref:tutorials/web-api-help-pages-using-swagger) and apply it to multiple actions, controllers, or all controllers within an assembly.</span></span> <span data-ttu-id="85a9b-105">Web-API-Konventionen können verwendet werden, damit keine einzelnen Aktionen mit [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute) versehen werden müssen.</span><span class="sxs-lookup"><span data-stu-id="85a9b-105">Web API conventions are a substitute for decorating individual actions with [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute).</span></span> <span data-ttu-id="85a9b-106">Sie können sie nutzen, um die gängigsten Rückgabetypen und Statuscodes, die durch Ihre Aktionen zurückgegeben werden, mit einer Möglichkeit zu definieren, die Konvention auszuwählen, die zu der jeweiligen Aktion passt.</span><span class="sxs-lookup"><span data-stu-id="85a9b-106">It allows you to define the most common "conventional" return types and status codes that you return from your action with a way to select the convention method that applies to an action.</span></span>

<span data-ttu-id="85a9b-107">In der Regel sind verschiedene Standardkonventionen im Lieferumfang von ASP.NET Core MVC 2.2 enthalten (`Microsoft.AspNetCore.Mvc.DefaultApiConventions`).</span><span class="sxs-lookup"><span data-stu-id="85a9b-107">By default, ASP.NET Core MVC 2.2 ships with a set of default conventions, `Microsoft.AspNetCore.Mvc.DefaultApiConventions`.</span></span> <span data-ttu-id="85a9b-108">Diese Konventionen basieren auf dem Controller, dessen Gerüst ASP.NET Core erstellt.</span><span class="sxs-lookup"><span data-stu-id="85a9b-108">The conventions are based on the controller that ASP.NET Core scaffolds.</span></span> <span data-ttu-id="85a9b-109">Wenn Ihre Aktionen dem Muster folgen, das aus dem Gerüstbau resultiert, sollten die Standardkonventionen ausreichen.</span><span class="sxs-lookup"><span data-stu-id="85a9b-109">If your actions follow the pattern that scaffolding produces, you should be successful using the default conventions.</span></span>

<span data-ttu-id="85a9b-110"><xref:Microsoft.AspNetCore.Mvc.ApiExplorer> erkennt zur Laufzeit Konventionen.</span><span class="sxs-lookup"><span data-stu-id="85a9b-110">At runtime, <xref:Microsoft.AspNetCore.Mvc.ApiExplorer> understands conventions.</span></span> <span data-ttu-id="85a9b-111">`ApiExplorer` ist die Abstraktion von MVC, über die mit Dokumentgeneratoren für Open API kommuniziert wird.</span><span class="sxs-lookup"><span data-stu-id="85a9b-111">`ApiExplorer` is MVC's abstraction to communicate with Open API document generators.</span></span> <span data-ttu-id="85a9b-112">Die Attribute der angewendeten Konvention werden einer Aktion zugeordnet und zu der Swagger-Dokumentation der Aktion hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="85a9b-112">Attributes from the applied convention get associated with an action and are included in the action's Swagger documentation.</span></span> <span data-ttu-id="85a9b-113">Auch API-Analysetools erkennen Konventionen.</span><span class="sxs-lookup"><span data-stu-id="85a9b-113">API analyzers also understand conventions.</span></span> <span data-ttu-id="85a9b-114">Wenn Ihre Aktion keiner Konvention folgt (also z.B. einen Statuscode zurückgibt, der nicht von der angewendeten Konvention dokumentiert wird), wird eine Warnung ausgegeben, in der Sie zur Dokumentation aufgefordert werden.</span><span class="sxs-lookup"><span data-stu-id="85a9b-114">If your action is unconventional (for example, it returns a status code that isn't documented by the applied convention), it produces a warning, encouraging you to document it.</span></span>

<span data-ttu-id="85a9b-115">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) ([Vorgehensweise zum Herunterladen](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="85a9b-115">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="apply-web-api-conventions"></a><span data-ttu-id="85a9b-116">Anwenden von Web-API-Konventionen</span><span class="sxs-lookup"><span data-stu-id="85a9b-116">Apply web API conventions</span></span>

<span data-ttu-id="85a9b-117">Es gibt drei Möglichkeiten, eine Konvention anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="85a9b-117">There are three ways to apply a convention.</span></span> <span data-ttu-id="85a9b-118">Konventionen werden nicht miteinander kombiniert.</span><span class="sxs-lookup"><span data-stu-id="85a9b-118">Conventions don't compose.</span></span> <span data-ttu-id="85a9b-119">Jede Aktion kann genau einer Konvention zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="85a9b-119">Each action may be associated with exactly one convention.</span></span> <span data-ttu-id="85a9b-120">Genauere Konventionen (unten aufgeführt) haben Vorrang gegenüber ungenaueren.</span><span class="sxs-lookup"><span data-stu-id="85a9b-120">More specific conventions (detailed below) take precedence over less specific ones.</span></span> <span data-ttu-id="85a9b-121">Die Auswahl ist nicht deterministisch, wenn mindestens zwei Konventionen mit gleicher Priorität für eine Aktion gelten.</span><span class="sxs-lookup"><span data-stu-id="85a9b-121">The selection is non-deterministic when two or more conventions of the same priority apply to an action.</span></span> <span data-ttu-id="85a9b-122">Sie können eine der folgenden Optionen auswählen, um angefangen bei der genausten Konvention bis hin zur ungenausten eine Konvention auf eine Aktion anzuwenden:</span><span class="sxs-lookup"><span data-stu-id="85a9b-122">The following options exist to apply a convention to an action, from the most specific to the least specific:</span></span>

1. <span data-ttu-id="85a9b-123">`Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; Gilt für einzelne Aktionen und gibt den jeweiligen passenden Konventionstyp und die Konventionsmethode an.</span><span class="sxs-lookup"><span data-stu-id="85a9b-123">`Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; Applies to individual actions and specifies the convention type and the convention method that applies.</span></span> <span data-ttu-id="85a9b-124">Im folgenden Beispiel wird die Konventionsmethode `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` auf die Aktion `Update` angewendet:</span><span class="sxs-lookup"><span data-stu-id="85a9b-124">In the following sample, the convention method `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` is applied to the `Update` action:</span></span>

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=apiconventionmethod&highlight=2-3)]

1. <span data-ttu-id="85a9b-125">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute`, auf einen Controller angewendet &mdash; wendet den Konventionstyp bei allen Controlleraktionen an.</span><span class="sxs-lookup"><span data-stu-id="85a9b-125">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` applied to a controller &mdash; Applies the convention type to all actions on the controller.</span></span> <span data-ttu-id="85a9b-126">Konventionsmethoden werden mit Hinweisen dazu versehen, für welche Aktionen sie jeweils anzuwenden sind (werden als Teil von Konventionen zur Dokumenterstellung zugewiesen).</span><span class="sxs-lookup"><span data-stu-id="85a9b-126">Convention methods are decorated with hints that determine which actions it would apply to (details as part of authoring conventions).</span></span> <span data-ttu-id="85a9b-127">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="85a9b-127">For example:</span></span>

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=apiconventiontypeattribute)]

1. <span data-ttu-id="85a9b-128">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute`, auf eine Assembly angewendet &mdash; wendet den Konventionstyp auf alle Controller in der aktuellen Assembly an.</span><span class="sxs-lookup"><span data-stu-id="85a9b-128">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` applied to an assembly &mdash; Applies the convention type to all controllers in the current assembly.</span></span> <span data-ttu-id="85a9b-129">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="85a9b-129">For example:</span></span>

    [!code-csharp[](conventions/sample/Startup.cs?name=apiconventiontypeattribute)]

## <a name="create-web-api-conventions"></a><span data-ttu-id="85a9b-130">Erstellen von Web-API-Konventionen</span><span class="sxs-lookup"><span data-stu-id="85a9b-130">Create web API conventions</span></span>

<span data-ttu-id="85a9b-131">Bei einer Konvention handelt es sich um einen statischen Typ mit Methoden.</span><span class="sxs-lookup"><span data-stu-id="85a9b-131">A convention is a static type with methods.</span></span> <span data-ttu-id="85a9b-132">Diese Methoden werden mit den Attributen `[ProducesResponseType]` oder `[ProducesDefaultResponseType]` versehen.</span><span class="sxs-lookup"><span data-stu-id="85a9b-132">These methods are annotated with `[ProducesResponseType]` or `[ProducesDefaultResponseType]` attributes.</span></span>

```csharp
public static class MyAppConventions
{
    [ProducesResponseType(200)]
    [ProducesResponseType(404)]
    public static void Find(int id)
    {
    }
}
```

<span data-ttu-id="85a9b-133">Wenn diese Konvention auf eine Assembly angewendet wird, wird die Konventionsmethode auf eine beliebige Aktion mit dem Namen `Find` und einem Parameter mit dem Namen `id` angewendet, wenn keine genaueren Metadatenattribute vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="85a9b-133">Applying this convention to an assembly results in the convention method applying to any action with the name `Find` and having exactly one parameter named `id`, as long as they don't have other more specific metadata attributes.</span></span>

<span data-ttu-id="85a9b-134">Neben `[ProducesResponseType]` und `[ProducesDefaultResponseType]` können auch die Attribute `[ApiConventionNameMatch]` und `[ApiConventionTypeMatch]` auf die Konventionsmethode angewendet werden, die die Methoden bestimmt, auf die sie anzuwenden sind.</span><span class="sxs-lookup"><span data-stu-id="85a9b-134">In addition to `[ProducesResponseType]` and `[ProducesDefaultResponseType]`, `[ApiConventionNameMatch]` and `[ApiConventionTypeMatch]` can be applied to the convention method that determines the methods they apply to.</span></span> <span data-ttu-id="85a9b-135">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="85a9b-135">For example:</span></span>

```csharp
[ProducesResponseType(200)]
[ProducesResponseType(404)]
[ApiConventionNameMatch(ApiConventionNameMatchBehavior.Prefix)]
public static void Find(
    [ApiConventionNameMatch(ApiConventionNameMatchBehavior.Suffix)]
    int id)
{ }
```

* <span data-ttu-id="85a9b-136">Wenn die Option `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` auf die Methode angewendet wird, deutet dies darauf hin, dass die Konvention mit jeder beliebigen Aktion übereinstimmt, solange diese das Präfix „Find“ aufweist.</span><span class="sxs-lookup"><span data-stu-id="85a9b-136">The `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` option applied to the method, indicates that the convention can match any action as long as it's prefixed with "Find".</span></span> <span data-ttu-id="85a9b-137">Dies betrifft z.B. die Methoden `Find`, `FindPet` und `FindById`.</span><span class="sxs-lookup"><span data-stu-id="85a9b-137">This includes methods such as `Find`, `FindPet`, or `FindById`.</span></span>
* <span data-ttu-id="85a9b-138">Wenn die Option `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` auf den Parameter angewendet wird, deutet dies darauf hin, dass die Konvention Methoden zugeordnet werden kann, die genau einen Parameter aufweisen, der auf dem Suffixbezeichner endet.</span><span class="sxs-lookup"><span data-stu-id="85a9b-138">The `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` applied to the parameter indicates that the convention can match methods with exactly one parameter ending in the suffix identifier.</span></span> <span data-ttu-id="85a9b-139">Dies betrifft z.B. die Parameter `id` und `petId`.</span><span class="sxs-lookup"><span data-stu-id="85a9b-139">This includes parameters such as `id` or `petId`.</span></span> <span data-ttu-id="85a9b-140">Genauso kann `ApiConventionTypeMatch` auf Typen angewendet, um den Parametertyp einzuschränken.</span><span class="sxs-lookup"><span data-stu-id="85a9b-140">`ApiConventionTypeMatch` can be similarly applied to types to constrain the type of the parameter.</span></span> <span data-ttu-id="85a9b-141">Ein `params[]`-Argument kann verwendet werden, um auf verbleibende Parameter zu verweisen, die nicht genau zugeordnet werden müssen.</span><span class="sxs-lookup"><span data-stu-id="85a9b-141">A `params[]` argument can be used to indicate remaining parameters that don't need to be explicitly matched.</span></span>
