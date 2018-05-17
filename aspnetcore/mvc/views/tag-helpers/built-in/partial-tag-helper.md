---
title: Hilfsprogramm für Teiltags in ASP.NET Core
author: scottaddie
description: Lernen Sie das ASP.NET Core-Hilfsprogramm für Teiltags und die Rolle seiner Attribute beim Rendern einer Teilansicht kennen.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 04/13/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/partial-tag-helper
ms.openlocfilehash: 670663b963f4207da793afff44d55b85ba58b7f8
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2018
---
# <a name="partial-tag-helper-in-aspnet-core"></a><span data-ttu-id="69815-103">Hilfsprogramm für Teiltags in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="69815-103">Partial Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="69815-104">Von [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="69815-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

[!INCLUDE [2.1 preview notice](~/includes/2.1.md)]

<span data-ttu-id="69815-105">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="69815-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="overview"></a><span data-ttu-id="69815-106">Übersicht</span><span class="sxs-lookup"><span data-stu-id="69815-106">Overview</span></span>

<span data-ttu-id="69815-107">Das Hilfsprogramm für Teiltags wird für das Rendern einer [Teilansicht](xref:mvc/views/partial) auf Razor-Seiten und in MVC-Apps verwendet.</span><span class="sxs-lookup"><span data-stu-id="69815-107">The Partial Tag Helper is used for rendering a [partial view](xref:mvc/views/partial) in Razor Pages and MVC apps.</span></span> <span data-ttu-id="69815-108">Bedenken Sie dabei Folgendes:</span><span class="sxs-lookup"><span data-stu-id="69815-108">Consider that it:</span></span>

* <span data-ttu-id="69815-109">Das Programm erfordert ASP.NET Core 2.1 oder höher.</span><span class="sxs-lookup"><span data-stu-id="69815-109">Requires ASP.NET Core 2.1 or later.</span></span>
* <span data-ttu-id="69815-110">Es stellt eine Alternative zur [Syntax des HTML-Hilfsprogramms](xref:mvc/views/partial#referencing-a-partial-view) dar.</span><span class="sxs-lookup"><span data-stu-id="69815-110">Is an alternative to [HTML Helper syntax](xref:mvc/views/partial#referencing-a-partial-view).</span></span>
* <span data-ttu-id="69815-111">Es rendert die Teilansicht asynchron.</span><span class="sxs-lookup"><span data-stu-id="69815-111">Renders the partial view asynchronously.</span></span>

<span data-ttu-id="69815-112">Folgende zählen zu den Optionen des HTML-Hilfsprogramms für das Rendern einer Teilansicht:</span><span class="sxs-lookup"><span data-stu-id="69815-112">The HTML Helper options for rendering a partial view include:</span></span>

* [<span data-ttu-id="69815-113">@await Html.PartialAsync</span><span class="sxs-lookup"><span data-stu-id="69815-113">@await Html.PartialAsync</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partialasync)
* [<span data-ttu-id="69815-114">@await Html.RenderPartialAsync</span><span class="sxs-lookup"><span data-stu-id="69815-114">@await Html.RenderPartialAsync</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartialasync)
* [@Html.Partial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partial)
* [@Html.RenderPartial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartial)

<span data-ttu-id="69815-115">Das Modell *Product* wird in den Beispielen in diesem Dokument verwendet:</span><span class="sxs-lookup"><span data-stu-id="69815-115">The *Product* model is used in samples throughout this document:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Models/Product.cs)]

<span data-ttu-id="69815-116">Eine Auflistung der im Hilfsprogramm für Teiltags enthaltenen Attribute folgt.</span><span class="sxs-lookup"><span data-stu-id="69815-116">An inventory of the Partial Tag Helper attributes follows.</span></span>

## <a name="name"></a><span data-ttu-id="69815-117">Name</span><span class="sxs-lookup"><span data-stu-id="69815-117">name</span></span>

<span data-ttu-id="69815-118">Das `name`-Attribut ist erforderlich.</span><span class="sxs-lookup"><span data-stu-id="69815-118">The `name` attribute is required.</span></span> <span data-ttu-id="69815-119">Es gibt den Namen oder den Pfad der Teilansicht an, die gerendert werden soll.</span><span class="sxs-lookup"><span data-stu-id="69815-119">It indicates the name or the path of the partial view to be rendered.</span></span> <span data-ttu-id="69815-120">Wenn der Name einer Teilansicht bereitgestellt wird, wird der Prozess [Ansichtsermittlung](xref:mvc/views/overview#view-discovery) initiiert.</span><span class="sxs-lookup"><span data-stu-id="69815-120">When a partial view name is provided, the [view discovery](xref:mvc/views/overview#view-discovery) process is initiated.</span></span> <span data-ttu-id="69815-121">Dieser Prozess wird umgangen, wenn ein expliziter Pfad bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="69815-121">That process is bypassed when an explicit path is provided.</span></span>

<span data-ttu-id="69815-122">Folgendes Markup verwendet einen expliziten Pfad, der angibt, dass *_ProductPartial.cshtml* aus dem Ordner *Shared* (Freigegeben) geladen werden soll.</span><span class="sxs-lookup"><span data-stu-id="69815-122">The following markup uses an explicit path, indicating that *_ProductPartial.cshtml* is to be loaded from the *Shared* folder.</span></span> <span data-ttu-id="69815-123">Wenn Sie das Attribut [for](#for) verwenden, wird an Modell zur Bindung an die Teilansicht übergeben.</span><span class="sxs-lookup"><span data-stu-id="69815-123">Using the [for](#for) attribute, a model is passed to the partial view for binding.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Name)]

## <a name="for"></a><span data-ttu-id="69815-124">for</span><span class="sxs-lookup"><span data-stu-id="69815-124">for</span></span>

<span data-ttu-id="69815-125">Das `for`-Attribut weist ein [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression)-Element zu, das für das aktuelle Modell ausgewertet werden soll.</span><span class="sxs-lookup"><span data-stu-id="69815-125">The `for` attribute assigns a [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression) to be evaluated against the current model.</span></span> <span data-ttu-id="69815-126">Ein `ModelExpression`-Element leitet die `@Model.`-Syntax ab.</span><span class="sxs-lookup"><span data-stu-id="69815-126">A `ModelExpression` infers the `@Model.` syntax.</span></span> <span data-ttu-id="69815-127">`for="Product"` kann beispielsweise anstelle von `for="@Model.Product"` verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="69815-127">For example, `for="Product"` can be used instead of `for="@Model.Product"`.</span></span> <span data-ttu-id="69815-128">Dieses Standardverhalten für die Ableitung kann überschrieben werden, indem Sie das `@`-Symbol zum Definieren eines Inlineausdrucks verwenden.</span><span class="sxs-lookup"><span data-stu-id="69815-128">This default inference behavior is overridden by using the `@` symbol to define an inline expression.</span></span> <span data-ttu-id="69815-129">Das `for`-Attribut kann nicht mit dem [model](#model)-Attribut verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="69815-129">The `for` attribute can't be used with the [model](#model) attribute.</span></span>

<span data-ttu-id="69815-130">Folgendes Markup lädt *_ProductPartial.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="69815-130">The following markup loads *_ProductPartial.cshtml*:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_For)]

<span data-ttu-id="69815-131">Die Teilansicht ist an die `Product`-Eigenschaft des zugehörigen Seitenmodells gebunden:</span><span class="sxs-lookup"><span data-stu-id="69815-131">The partial view is bound to the associated page model's `Product` property:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Product.cshtml.cs?highlight=8)]

## <a name="model"></a><span data-ttu-id="69815-132">Modell</span><span class="sxs-lookup"><span data-stu-id="69815-132">model</span></span>

<span data-ttu-id="69815-133">Das `model`-Attribut weist eine Modellinstanz zu, die an die Teilansicht übergeben werden soll.</span><span class="sxs-lookup"><span data-stu-id="69815-133">The `model` attribute assigns a model instance to pass to the partial view.</span></span> <span data-ttu-id="69815-134">Das `model`-Attribut kann nicht mit dem [for](#for)-Attribut verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="69815-134">The `model` attribute can't be used with the [for](#for) attribute.</span></span>

<span data-ttu-id="69815-135">Im folgenden Markup wird ein neues `Product`-Objekt instanziiert und an das `model`-Attribut zur Bindung übergeben:</span><span class="sxs-lookup"><span data-stu-id="69815-135">In the following markup, a new `Product` object is instantiated and passed to the `model` attribute for binding:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Model)]

## <a name="view-data"></a><span data-ttu-id="69815-136">view-data</span><span class="sxs-lookup"><span data-stu-id="69815-136">view-data</span></span>

<span data-ttu-id="69815-137">Das `view-data`-Attribut weist ein [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary)-Element zu, das an die Teilansicht übergeben werden soll.</span><span class="sxs-lookup"><span data-stu-id="69815-137">The `view-data` attribute assigns a [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) to pass to the partial view.</span></span> <span data-ttu-id="69815-138">Folgendes Markup stellt die gesamte ViewData-Auflistung für die Teilansicht zur Verfügung:</span><span class="sxs-lookup"><span data-stu-id="69815-138">The following markup makes the entire ViewData collection accessible to the partial view:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_ViewData&highlight=5-)]

<span data-ttu-id="69815-139">Im vorangehenden Code wird der Schlüsselwert `IsNumberReadOnly` auf `true` festgelegt und zur ViewData-Auflistung hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="69815-139">In the preceding code, the `IsNumberReadOnly` key value is set to `true` and added to the ViewData collection.</span></span> <span data-ttu-id="69815-140">Folglich wird `ViewData["IsNumberReadOnly"]` innerhalb der folgenden Teilansicht zur Verfügung gestellt:</span><span class="sxs-lookup"><span data-stu-id="69815-140">Consequently, `ViewData["IsNumberReadOnly"]` is made accessible within the following partial view:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Shared/_ProductViewDataPartial.cshtml?highlight=5)]

<span data-ttu-id="69815-141">In diesem Beispiel bestimmt der Wert von `ViewData["IsNumberReadOnly"]`, ob das Feld *Number* (Anzahl) schreibgeschützt sein soll.</span><span class="sxs-lookup"><span data-stu-id="69815-141">In this example, the value of `ViewData["IsNumberReadOnly"]` determines whether the *Number* field is displayed as read only.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="69815-142">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="69815-142">Additional resources</span></span>

* [<span data-ttu-id="69815-143">Teilansichten</span><span class="sxs-lookup"><span data-stu-id="69815-143">Partial views</span></span>](xref:mvc/views/partial)
* [<span data-ttu-id="69815-144">Schwach typisierte Daten („ViewData“ und „ViewBag“)</span><span class="sxs-lookup"><span data-stu-id="69815-144">Weakly typed data (ViewData and ViewBag)</span></span>](xref:mvc/views/overview#weakly-typed-data-viewdata-and-viewbag)
