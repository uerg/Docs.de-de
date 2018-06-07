---
title: Ansichtskomponenten in ASP.NET Core
author: rick-anderson
description: Erfahren Sie, wie Ansichtskomponenten in ASP.NET Core verwendet werden und wie sie Apps hinzugefügt werden.
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-components
ms.openlocfilehash: cdf44fc15ac64497b2589e8b7b289beb94c5b2c4
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
ms.locfileid: "33962683"
---
# <a name="view-components-in-aspnet-core"></a><span data-ttu-id="123d8-103">Ansichtskomponenten in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="123d8-103">View components in ASP.NET Core</span></span>

<span data-ttu-id="123d8-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="123d8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="123d8-105">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="123d8-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="view-components"></a><span data-ttu-id="123d8-106">Ansichtskomponenten</span><span class="sxs-lookup"><span data-stu-id="123d8-106">View components</span></span>

<span data-ttu-id="123d8-107">Ansichtskomponenten ähneln zwar den Teilansichten, sind aber wesentlich leistungsstärker.</span><span class="sxs-lookup"><span data-stu-id="123d8-107">View components are similar to partial views, but they're much more powerful.</span></span> <span data-ttu-id="123d8-108">Ansichtskomponenten verwenden keine Modellbindungen und sind nur von den Daten abhängig, die bei ihrem Aufruf bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="123d8-108">View components don't use model binding, and only depend on the data provided when calling into it.</span></span> <span data-ttu-id="123d8-109">Für diesen Artikel wurde ASP.NET Core-MVC verwendet. Es funktionieren aber auch Ansichtskomponenten zusammen mit Razor-Seiten.</span><span class="sxs-lookup"><span data-stu-id="123d8-109">This article was written using ASP.NET Core MVC, but view components also work with Razor Pages.</span></span>

<span data-ttu-id="123d8-110">Eine Ansichtskomponente:</span><span class="sxs-lookup"><span data-stu-id="123d8-110">A view component:</span></span>

* <span data-ttu-id="123d8-111">Rendert nur einen Block statt einer gesamten Antwort.</span><span class="sxs-lookup"><span data-stu-id="123d8-111">Renders a chunk rather than a whole response.</span></span>
* <span data-ttu-id="123d8-112">Umfasst die gleiche Trennung von Belangen und Vorzüge der Testbarkeit, die auch zwischen einem Controller und einer Ansicht bestehen.</span><span class="sxs-lookup"><span data-stu-id="123d8-112">Includes the same separation-of-concerns and testability benefits found between a controller and view.</span></span>
* <span data-ttu-id="123d8-113">Kann Parameter und Geschäftslogik aufweisen.</span><span class="sxs-lookup"><span data-stu-id="123d8-113">Can have parameters and business logic.</span></span>
* <span data-ttu-id="123d8-114">Wird normalerweise von einer Layoutseite aus aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="123d8-114">Is typically invoked from a layout page.</span></span>

<span data-ttu-id="123d8-115">Ansichtskomponenten wurden für wiederverwendbare Renderinglogik entwickelt, die für eine Teilansicht zu komplex ist. Dazu gehören:</span><span class="sxs-lookup"><span data-stu-id="123d8-115">View components are intended anywhere you have reusable rendering logic that's too complex for a partial view, such as:</span></span>

* <span data-ttu-id="123d8-116">Dynamische Navigationsmenüs</span><span class="sxs-lookup"><span data-stu-id="123d8-116">Dynamic navigation menus</span></span>
* <span data-ttu-id="123d8-117">Tag Cloud (dort, wo die Datenbank abgefragt wird)</span><span class="sxs-lookup"><span data-stu-id="123d8-117">Tag cloud (where it queries the database)</span></span>
* <span data-ttu-id="123d8-118">ein Anmeldebereich</span><span class="sxs-lookup"><span data-stu-id="123d8-118">Login panel</span></span>
* <span data-ttu-id="123d8-119">Einkaufswagen</span><span class="sxs-lookup"><span data-stu-id="123d8-119">Shopping cart</span></span>
* <span data-ttu-id="123d8-120">vor Kurzem veröffentlichte Artikel</span><span class="sxs-lookup"><span data-stu-id="123d8-120">Recently published articles</span></span>
* <span data-ttu-id="123d8-121">Inhalt in einer Seitenleiste auf einem klassischen Blog</span><span class="sxs-lookup"><span data-stu-id="123d8-121">Sidebar content on a typical blog</span></span>
* <span data-ttu-id="123d8-122">Ein Anmeldebereich, der auf jeder Seite gerendert wird und der die Links zum Abmelden bzw. Anmelden anzeigt, je nachdem, ob der Benutzer an- oder abgemeldet ist</span><span class="sxs-lookup"><span data-stu-id="123d8-122">A login panel that would be rendered on every page and show either the links to log out or log in, depending on the log in state of the user</span></span>

<span data-ttu-id="123d8-123">Eine Ansichtskomponenten besteht aus zwei Teilen: der Klasse (normalerweise von [ViewComponent](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponent) abgeleitet) und dem von dieser Klasse zurückgegebenen Ergebnis (normalerweise eine Ansicht).</span><span class="sxs-lookup"><span data-stu-id="123d8-123">A view component consists of two parts: the class (typically derived from [ViewComponent](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponent)) and the result it returns (typically a view).</span></span> <span data-ttu-id="123d8-124">Eine Ansichtskomponente kann, ähnlich wie Controller, ein POCO sein. Die meisten Entwickler sollten jedoch von den Methoden und Eigenschaften, die von `ViewComponent` abgeleitet werden, Gebrauch machen.</span><span class="sxs-lookup"><span data-stu-id="123d8-124">Like controllers, a view component can be a POCO, but most developers will want to take advantage of the methods and properties available by deriving from `ViewComponent`.</span></span>

## <a name="creating-a-view-component"></a><span data-ttu-id="123d8-125">Erstellen einer Ansichtskomponente</span><span class="sxs-lookup"><span data-stu-id="123d8-125">Creating a view component</span></span>

<span data-ttu-id="123d8-126">In diesem Abschnitt werden die allgemeinen Anforderungen zum Erstellen einer Ansichtskomponente beschrieben.</span><span class="sxs-lookup"><span data-stu-id="123d8-126">This section contains the high-level requirements to create a view component.</span></span> <span data-ttu-id="123d8-127">Im Folgenden wird jeder Schritt ausführlich betrachtet, und Sie erstellen im Zuge dessen eine Ansichtskomponente.</span><span class="sxs-lookup"><span data-stu-id="123d8-127">Later in the article, we'll examine each step in detail and create a view component.</span></span>

### <a name="the-view-component-class"></a><span data-ttu-id="123d8-128">Die Ansichtskomponentenklasse</span><span class="sxs-lookup"><span data-stu-id="123d8-128">The view component class</span></span>

<span data-ttu-id="123d8-129">Eine Ansichtskomponentenklasse kann durch folgende Aktionen erstellt werden:</span><span class="sxs-lookup"><span data-stu-id="123d8-129">A view component class can be created by any of the following:</span></span>

* <span data-ttu-id="123d8-130">durch die Ableitung von *ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="123d8-130">Deriving from *ViewComponent*</span></span>
* <span data-ttu-id="123d8-131">durch Ergänzen der Klasse mit dem Attribut `[ViewComponent]` oder durch das Ableiten von einer Klasse mit dem Attribut `[ViewComponent]`</span><span class="sxs-lookup"><span data-stu-id="123d8-131">Decorating a class with the `[ViewComponent]` attribute, or deriving from a class with the `[ViewComponent]` attribute</span></span>
* <span data-ttu-id="123d8-132">durch das Erstellen einer Klasse, deren Name mit dem Suffix *ViewComponent* endet</span><span class="sxs-lookup"><span data-stu-id="123d8-132">Creating a class where the name ends with the suffix *ViewComponent*</span></span>

<span data-ttu-id="123d8-133">Ansichtskomponenten müssen, genauso wie Controller, öffentliche, unverschachtelt und nicht abstrakte Klassen sein.</span><span class="sxs-lookup"><span data-stu-id="123d8-133">Like controllers, view components must be public, non-nested, and non-abstract classes.</span></span> <span data-ttu-id="123d8-134">Der Ansichtskomponentenname ist der Klassenname ohne den Suffix „ViewComponent“.</span><span class="sxs-lookup"><span data-stu-id="123d8-134">The view component name is the class name with the "ViewComponent" suffix removed.</span></span> <span data-ttu-id="123d8-135">Er kann zudem explizit mit der Eigenschaft `ViewComponentAttribute.Name` angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="123d8-135">It can also be explicitly specified using the `ViewComponentAttribute.Name` property.</span></span>

<span data-ttu-id="123d8-136">Eine Ansichtskomponentenklasse:</span><span class="sxs-lookup"><span data-stu-id="123d8-136">A view component class:</span></span>

* <span data-ttu-id="123d8-137">Unterstützt [Constructor Dependency Injection](../../fundamentals/dependency-injection.md) vollständig</span><span class="sxs-lookup"><span data-stu-id="123d8-137">Fully supports constructor [dependency injection](../../fundamentals/dependency-injection.md)</span></span>

* <span data-ttu-id="123d8-138">Ist nicht Bestandteil des Controllerlebenszyklus. Dies bedeutet, dass Sie keine [Filter](../controllers/filters.md) in Ansichtskomponenten verwenden können.</span><span class="sxs-lookup"><span data-stu-id="123d8-138">Doesn't take part in the controller lifecycle, which means you can't use [filters](../controllers/filters.md) in a view component</span></span>

### <a name="view-component-methods"></a><span data-ttu-id="123d8-139">Ansichtskomponentenmethoden</span><span class="sxs-lookup"><span data-stu-id="123d8-139">View component methods</span></span>

<span data-ttu-id="123d8-140">Eine Ansichtskomponente definiert Ihre Logik in einer `InvokeAsync`-Methode, die ein `IViewComponentResult`-Objekt zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="123d8-140">A view component defines its logic in an `InvokeAsync` method that returns an `IViewComponentResult`.</span></span> <span data-ttu-id="123d8-141">Parameter stammen direkt vom Aufruf der Ansichtskomponente und nicht von der Modellbindung.</span><span class="sxs-lookup"><span data-stu-id="123d8-141">Parameters come directly from invocation of the view component, not from model binding.</span></span> <span data-ttu-id="123d8-142">Eine Ansichtskomponente behandelt nie direkt eine Anfrage.</span><span class="sxs-lookup"><span data-stu-id="123d8-142">A view component never directly handles a request.</span></span> <span data-ttu-id="123d8-143">Normalerweise initialisiert eine Ansichtskomponente ein Modell und übergibt dieses an eine Ansicht, indem sie die `View`-Methode aufruft.</span><span class="sxs-lookup"><span data-stu-id="123d8-143">Typically, a view component initializes a model and passes it to a view by calling the `View` method.</span></span> <span data-ttu-id="123d8-144">Zusammengefasst bedeutet dies für Komponentenmethoden Folgendes:</span><span class="sxs-lookup"><span data-stu-id="123d8-144">In summary, view component methods:</span></span>

* <span data-ttu-id="123d8-145">Sie definieren eine `InvokeAsync`-Methode, die ein `IViewComponentResult`-Objekt zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="123d8-145">Define an `InvokeAsync` method that returns an `IViewComponentResult`</span></span>
* <span data-ttu-id="123d8-146">Normalerweise initialisieren sie ein Modell und übergeben dieses an eine Ansicht, indem sie die `ViewComponent` `View`-Methode aufrufen.</span><span class="sxs-lookup"><span data-stu-id="123d8-146">Typically initializes a model and passes it to a view by calling the `ViewComponent` `View` method</span></span>
* <span data-ttu-id="123d8-147">Parameter stammen vom Methodenaufruf und nicht HTTP. Es gibt keine Modellbindung.</span><span class="sxs-lookup"><span data-stu-id="123d8-147">Parameters come from the calling method, not HTTP, there's no model binding</span></span>
* <span data-ttu-id="123d8-148">Sie können nicht direkt als HTTP-Endpunkt erreicht werden. Stattdessen werden sie über Ihren Code aufgerufen (normalerweise in einer Ansicht).</span><span class="sxs-lookup"><span data-stu-id="123d8-148">Are not reachable directly as an HTTP endpoint, they're invoked from your code (usually in a view).</span></span> <span data-ttu-id="123d8-149">Eine Ansichtskomponente behandelt nie eine Anfrage.</span><span class="sxs-lookup"><span data-stu-id="123d8-149">A view component never handles a request</span></span>
* <span data-ttu-id="123d8-150">Sie werden in der Signatur überladen und nicht in Details der aktuellen HTTP-Anforderung</span><span class="sxs-lookup"><span data-stu-id="123d8-150">Are overloaded on the signature rather than any details from the current HTTP request</span></span>

### <a name="view-search-path"></a><span data-ttu-id="123d8-151">Anzeigen des Suchpfads</span><span class="sxs-lookup"><span data-stu-id="123d8-151">View search path</span></span>

<span data-ttu-id="123d8-152">Die Runtime sucht in den folgenden Pfaden nach der Ansicht:</span><span class="sxs-lookup"><span data-stu-id="123d8-152">The runtime searches for the view in the following paths:</span></span>

   * <span data-ttu-id="123d8-153">Views/\<controllername>/Components/\<ansichtskomponentenname>/\<ansichtsname></span><span class="sxs-lookup"><span data-stu-id="123d8-153">Views/\<controller_name>/Components/\<view_component_name>/\<view_name></span></span>
   * <span data-ttu-id="123d8-154">Views/Shared/Components/\<ansichtskomponentenname>/\<ansichtsname></span><span class="sxs-lookup"><span data-stu-id="123d8-154">Views/Shared/Components/\<view_component_name>/\<view_name></span></span>

<span data-ttu-id="123d8-155">Der Standardansichtsname für die Ansichtskomponente ist *Default*. Dies bedeutet, dass Ihre Ansichtsdatei normalerweise *Default.cshtml* heißt.</span><span class="sxs-lookup"><span data-stu-id="123d8-155">The default view name for a view component is *Default*, which means your view file will typically be named *Default.cshtml*.</span></span> <span data-ttu-id="123d8-156">Sie können einen anderen Ansichtsnamen angeben, wenn Sie die Ansichtskomponentenergebnisse erstellen oder die `View`-Methode aufrufen.</span><span class="sxs-lookup"><span data-stu-id="123d8-156">You can specify a different view name when creating the view component result or when calling the `View` method.</span></span>

<span data-ttu-id="123d8-157">Es wird empfohlen, dass Sie die Ansichtsdatei *Default.cshtml* nennen und den Pfad *View/Shared/Components/\<ansichtskomponentenname>/\<ansichtsname>* verwenden.</span><span class="sxs-lookup"><span data-stu-id="123d8-157">We recommend you name the view file *Default.cshtml* and use the *Views/Shared/Components/\<view_component_name>/\<view_name>* path.</span></span> <span data-ttu-id="123d8-158">Die Ansichtskomponente `PriorityList`, die in diesem Beispiel verwendet wird, verwendet *Views/Shared/Components/PriorityList/Default.cshtml* für die Ansichtskomponentenansicht.</span><span class="sxs-lookup"><span data-stu-id="123d8-158">The `PriorityList` view component used in this sample uses *Views/Shared/Components/PriorityList/Default.cshtml* for the view component view.</span></span>

## <a name="invoking-a-view-component"></a><span data-ttu-id="123d8-159">Aufrufen einer Ansichtskomponente</span><span class="sxs-lookup"><span data-stu-id="123d8-159">Invoking a view component</span></span>

<span data-ttu-id="123d8-160">Rufen Sie Folgendes in der Ansicht auf, wenn Sie die Ansichtskomponente verwenden möchten:</span><span class="sxs-lookup"><span data-stu-id="123d8-160">To use the view component, call the following inside a view:</span></span>

```cshtml
@Component.InvokeAsync("Name of view component", <anonymous type containing parameters>)
```

<span data-ttu-id="123d8-161">Die Parameter werden an die `InvokeAsync`-Methode übergeben.</span><span class="sxs-lookup"><span data-stu-id="123d8-161">The parameters will be passed to the `InvokeAsync` method.</span></span> <span data-ttu-id="123d8-162">Die Ansichtskomponente `PriorityList`, die im Artikel entwickelt wurde, wird von der Ansichtsdatei *Views/Todo/Index.cshtml* aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="123d8-162">The `PriorityList` view component developed in the article is invoked from the *Views/Todo/Index.cshtml* view file.</span></span> <span data-ttu-id="123d8-163">Im folgenden Code wird die `InvokeAsync`-Methode mit zwei Parametern aufgerufen:</span><span class="sxs-lookup"><span data-stu-id="123d8-163">In the following, the `InvokeAsync` method is called with two parameters:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

## <a name="invoking-a-view-component-as-a-tag-helper"></a><span data-ttu-id="123d8-164">Aufrufen einer Ansichtskomponente als Taghilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="123d8-164">Invoking a view component as a Tag Helper</span></span>

<span data-ttu-id="123d8-165">In ASP.NET Core 1.1 und höher können Sie eine Ansichtskomponente als [Taghilfsprogramm](xref:mvc/views/tag-helpers/intro) aufrufen.</span><span class="sxs-lookup"><span data-stu-id="123d8-165">For ASP.NET Core 1.1 and higher, you can invoke a view component as a [Tag Helper](xref:mvc/views/tag-helpers/intro):</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="123d8-166">Namen von Klassen und Methodenparameter für Taghilfsprogramme, die in Pascal-Schreibweise angegeben sind, werden in [Lower Kebab-Schreibweise](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101) übersetzt.</span><span class="sxs-lookup"><span data-stu-id="123d8-166">Pascal-cased class and method parameters for Tag Helpers are translated into their [lower kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="123d8-167">Das Taghilfsprogramm zum Aufrufen einer Ansichtskomponente verwendet das `<vc></vc>`-Element.</span><span class="sxs-lookup"><span data-stu-id="123d8-167">The Tag Helper to invoke a view component uses the `<vc></vc>` element.</span></span> <span data-ttu-id="123d8-168">Die Ansichtskomponente wird wie folgt angegeben:</span><span class="sxs-lookup"><span data-stu-id="123d8-168">The view component is specified as follows:</span></span>

```cshtml
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

<span data-ttu-id="123d8-169">Beachten Sie: Damit Sie eine Ansichtskomponente als Taghilfsprogramm verwenden können, müssen Sie die Assembly, die die Ansichtskomponente enthält, mit der `@addTagHelper`-Anweisung registrieren.</span><span class="sxs-lookup"><span data-stu-id="123d8-169">Note: In order to use a View Component as a Tag Helper, you must register the assembly containing the View Component using the `@addTagHelper` directive.</span></span> <span data-ttu-id="123d8-170">Wenn Ihre Ansichtskomponente z.B. eine Assembly mit dem Namen „MeineWebApp“ ist, fügen Sie die folgende Anweisung zu der Datei `_ViewImports.cshtml` hinzu:</span><span class="sxs-lookup"><span data-stu-id="123d8-170">For example, if your View Component is in an assembly called "MyWebApp", add the following directive to the `_ViewImports.cshtml` file:</span></span>

```cshtml
@addTagHelper *, MyWebApp
```

<span data-ttu-id="123d8-171">Sie können eine Ansichtskomponente als Taghilfsprogramm für jede Datei registrieren, die auf die Ansichtskomponente verweist.</span><span class="sxs-lookup"><span data-stu-id="123d8-171">You can register a View Component as a Tag Helper to any file that references the View Component.</span></span> <span data-ttu-id="123d8-172">Weitere Informationen zum Registrieren eines Taghilfsprogramms finden Sie unter [Managing Tag Helper Scope (Verwalten des Bereichs des Taghilfsprogramms)](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope).</span><span class="sxs-lookup"><span data-stu-id="123d8-172">See [Managing Tag Helper Scope](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) for more information on how to register Tag Helpers.</span></span>

<span data-ttu-id="123d8-173">Die `InvokeAsync`-Methode, die in diesem Tutorial verwendet wird:</span><span class="sxs-lookup"><span data-stu-id="123d8-173">The `InvokeAsync` method used in this tutorial:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="123d8-174">In Taghilfsprogramm-Markup:</span><span class="sxs-lookup"><span data-stu-id="123d8-174">In Tag Helper markup:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="123d8-175">Im Beispiel oben stehenden Beispiel wird die Ansichtskomponente `PriorityList` zu `priority-list`.</span><span class="sxs-lookup"><span data-stu-id="123d8-175">In the sample above, the `PriorityList` view component becomes `priority-list`.</span></span> <span data-ttu-id="123d8-176">Die Parameter der Ansichtskomponente werden als Attribute in Lower Kebab-Schreibweise übergeben.</span><span class="sxs-lookup"><span data-stu-id="123d8-176">The parameters to the view component are passed as attributes in lower kebab case.</span></span>

### <a name="invoking-a-view-component-directly-from-a-controller"></a><span data-ttu-id="123d8-177">Direkter Aufrufe einer Ansichtskomponente von einem Controller</span><span class="sxs-lookup"><span data-stu-id="123d8-177">Invoking a view component directly from a controller</span></span>

<span data-ttu-id="123d8-178">Ansichtskomponenten werden normalerweise von einer Ansicht aus aufgerufen, aber Sie können sie auch direkt von einer Controllermethode aus aufrufen.</span><span class="sxs-lookup"><span data-stu-id="123d8-178">View components are typically invoked from a view, but you can invoke them directly from a controller method.</span></span> <span data-ttu-id="123d8-179">Obwohl Ansichtskomponenten keine Endpunkte so wie Controller definieren, können Sie trotzdem eine Controlleraktion implementieren, die den Inhalt von `ViewComponentResult` zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="123d8-179">While view components don't define endpoints like controllers, you can easily implement a controller action that returns the content of a `ViewComponentResult`.</span></span>

<span data-ttu-id="123d8-180">In diesem Beispiel wird die Ansichtskomponente direkt von einem Controller aus aufgerufen:</span><span class="sxs-lookup"><span data-stu-id="123d8-180">In this example, the view component is called directly from the controller:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a><span data-ttu-id="123d8-181">Exemplarische Vorgehensweise: Erstellen einer einfachen Ansichtskomponente</span><span class="sxs-lookup"><span data-stu-id="123d8-181">Walkthrough: Creating a simple view component</span></span>

<span data-ttu-id="123d8-182">[Laden Sie den Startercode herunter](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), und erstellen und testen Sie diesen.</span><span class="sxs-lookup"><span data-stu-id="123d8-182">[Download](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), build and test the starter code.</span></span> <span data-ttu-id="123d8-183">Dabei handelt es sich um ein einfaches Projekt mit einem `Todo`-Controller, in dem eine Liste von *ToDo*-Elementen angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="123d8-183">It's a simple project with a `Todo` controller that displays a list of *Todo* items.</span></span>

![Liste mit ToDo-Elementen](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a><span data-ttu-id="123d8-185">Hinzufügen einer ViewComponent-Klasse</span><span class="sxs-lookup"><span data-stu-id="123d8-185">Add a ViewComponent class</span></span>

<span data-ttu-id="123d8-186">Erstellen Sie einen *ViewComponents*-Ordner, und fügen Sie die folgende `PriorityListViewComponent`-Klasse hinzu:</span><span class="sxs-lookup"><span data-stu-id="123d8-186">Create a *ViewComponents* folder and add the following `PriorityListViewComponent` class:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

<span data-ttu-id="123d8-187">Bemerkungen zum Code:</span><span class="sxs-lookup"><span data-stu-id="123d8-187">Notes on the code:</span></span>

* <span data-ttu-id="123d8-188">Ansichtskomponentenklassen können sich in **jedem** Ordner im Projekt befinden.</span><span class="sxs-lookup"><span data-stu-id="123d8-188">View component classes can be contained in **any** folder in the project.</span></span>
* <span data-ttu-id="123d8-189">Da der Klassenname „PriorityList**ViewComponent** mit dem Suffix **ViewComponent** endet, verwendet die Runtime die Zeichenfolge „PriorityList“, wenn Sie von einer Ansicht aus auf die Klassenkomponente verweist.</span><span class="sxs-lookup"><span data-stu-id="123d8-189">Because the class name PriorityList**ViewComponent** ends with the suffix **ViewComponent**, the runtime will use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="123d8-190">Dies wird im Folgenden noch ausführlicher erklärt.</span><span class="sxs-lookup"><span data-stu-id="123d8-190">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="123d8-191">Das `[ViewComponent]`-Attribut kann den Namen ändern, der zum Verweis auf eine Ansichtskomponente verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="123d8-191">The `[ViewComponent]` attribute can change the name used to reference a view component.</span></span> <span data-ttu-id="123d8-192">Wir hätten die Klasse auch `XYZ` nennen und das `ViewComponent`-Attribut anwenden können:</span><span class="sxs-lookup"><span data-stu-id="123d8-192">For example, we could've named the class `XYZ` and applied the `ViewComponent` attribute:</span></span>

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* <span data-ttu-id="123d8-193">Das oben stehende `[ViewComponent]`-Attribut teilt dem Ansichtskomponentenselektor mit, den Namen `PriorityList` zu verwenden, wenn er nach den Ansichten sucht, die mit der Komponente verknüpft sind. Zudem wird er informiert, die Zeichenfolge „“ zu verwenden, wenn er von einer Ansicht aus auf die Klassenkomponente verweist.</span><span class="sxs-lookup"><span data-stu-id="123d8-193">The `[ViewComponent]` attribute above tells the view component selector to use the name `PriorityList` when looking for the views associated with the component, and to use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="123d8-194">Dies wird im Folgenden noch ausführlicher erklärt.</span><span class="sxs-lookup"><span data-stu-id="123d8-194">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="123d8-195">Die Komponente verwendet [Dependency Injection](../../fundamentals/dependency-injection.md), um den Datenkontext verfügbar zu machen.</span><span class="sxs-lookup"><span data-stu-id="123d8-195">The component uses [dependency injection](../../fundamentals/dependency-injection.md) to make the data context available.</span></span>
* <span data-ttu-id="123d8-196">`InvokeAsync` macht eine Methode verfügbar, die von einer Ansicht aus aufgerufen werden kann, und akzeptiert eine arbiträre Anzahl von Argumenten.</span><span class="sxs-lookup"><span data-stu-id="123d8-196">`InvokeAsync` exposes a method which can be called from a view, and it can take an arbitrary number of arguments.</span></span>
* <span data-ttu-id="123d8-197">Die `InvokeAsync`-Methode gibt mehrere `ToDo`-Elemente zurück, die die Bedingungen der Parameter `isDone` und `maxPriority` erfüllen.</span><span class="sxs-lookup"><span data-stu-id="123d8-197">The `InvokeAsync` method returns the set of `ToDo` items that satisfy the `isDone` and `maxPriority` parameters.</span></span>

### <a name="create-the-view-component-razor-view"></a><span data-ttu-id="123d8-198">Erstellen der Razor-Ansicht der Ansichtskomponente</span><span class="sxs-lookup"><span data-stu-id="123d8-198">Create the view component Razor view</span></span>

* <span data-ttu-id="123d8-199">Erstellen Sie den Ordner *Views/Shared/Components*.</span><span class="sxs-lookup"><span data-stu-id="123d8-199">Create the *Views/Shared/Components* folder.</span></span> <span data-ttu-id="123d8-200">Diese Ordner **muss** den Namen *Components* besitzen.</span><span class="sxs-lookup"><span data-stu-id="123d8-200">This folder **must** be named *Components*.</span></span>

* <span data-ttu-id="123d8-201">Erstellen Sie den Ordner *Views/Shared/Components/PriorityList*.</span><span class="sxs-lookup"><span data-stu-id="123d8-201">Create the *Views/Shared/Components/PriorityList* folder.</span></span> <span data-ttu-id="123d8-202">Der Ordnername muss mit dem Namen der Ansichtskomponentenklasse oder mit dem Namen der Klasse ohne Suffix (wenn wir uns an die Konvention gehalten und *ViewComponent* als Suffix im Klassennamen verwendet haben) übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="123d8-202">This folder name must match the name of the view component class, or the name of the class minus the suffix (if we followed convention and used the *ViewComponent* suffix in the class name).</span></span> <span data-ttu-id="123d8-203">Wenn Sie das Attribut `ViewComponent` verwenden, muss der Klassenname mit der Attributbezeichnung übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="123d8-203">If you used the `ViewComponent` attribute, the class name would need to match the attribute designation.</span></span>

* <span data-ttu-id="123d8-204">Erstellen Sie die Razor-Ansicht *Views/Shared/Components/PriorityList/Default.cshtml*: [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="123d8-204">Create a *Views/Shared/Components/PriorityList/Default.cshtml* Razor view: [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span></span>
    
   <span data-ttu-id="123d8-205">Die Razor-Ansicht nimmt eine Liste von `TodoItem` an und zeigt diese an.</span><span class="sxs-lookup"><span data-stu-id="123d8-205">The Razor view takes a list of `TodoItem` and displays them.</span></span> <span data-ttu-id="123d8-206">Wenn die `InvokeAsync`-Methode der Ansichtskomponente nicht den Namen der Ansicht übergibt (wie in unserem Beispiel), wird *Default* per Konvention für den Ansichtsnamen verwendet.</span><span class="sxs-lookup"><span data-stu-id="123d8-206">If the view component `InvokeAsync` method doesn't pass the name of the view (as in our sample), *Default* is used for the view name by convention.</span></span> <span data-ttu-id="123d8-207">Später in diesem Tutorial erfahren Sie, wie Sie den Namen der Ansicht übergeben.</span><span class="sxs-lookup"><span data-stu-id="123d8-207">Later in the tutorial, I'll show you how to pass the name of the view.</span></span> <span data-ttu-id="123d8-208">Fügen Sie eine Ansicht zu einem controllerspezifischen Ansichtsordner hinzu, um das Standardformat für einen spezifischen Controller zu überschreiben (z.B. *Views/Todo/Components/PriorityList/Default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="123d8-208">To override the default styling for a specific controller, add a view to the controller-specific view folder (for example *Views/Todo/Components/PriorityList/Default.cshtml)*.</span></span>
    
    <span data-ttu-id="123d8-209">Wenn die Ansichtskomponente controllerspezifisch ist, können Sie sie dem controllerspezifischen Ordner hinzufügen (*Views/Todo/Components/PriorityList/Default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="123d8-209">If the view component is controller-specific, you can add it to the controller-specific folder (*Views/Todo/Components/PriorityList/Default.cshtml*).</span></span>

* <span data-ttu-id="123d8-210">Fügen Sie ein `div`-Objekt, das einen Aufruf an eine Prioritätslistenkomponente enthält, am Ende der Datei *Views/Todo/index.cshtml* hinzu:</span><span class="sxs-lookup"><span data-stu-id="123d8-210">Add a `div` containing a call to the priority list component to the bottom of the *Views/Todo/index.cshtml* file:</span></span>

    [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFirst.cshtml?range=34-38)]

<span data-ttu-id="123d8-211">Das Markup `@await Component.InvokeAsync` zeigt die Syntax für die aufrufenden Ansichtskomponenten an.</span><span class="sxs-lookup"><span data-stu-id="123d8-211">The markup `@await Component.InvokeAsync` shows the syntax for calling view components.</span></span> <span data-ttu-id="123d8-212">Das erste Argument ist der Name der aufzurufenden Komponente.</span><span class="sxs-lookup"><span data-stu-id="123d8-212">The first argument is the name of the component we want to invoke or call.</span></span> <span data-ttu-id="123d8-213">Darauffolgende Parameter werden an die Komponente übergeben.</span><span class="sxs-lookup"><span data-stu-id="123d8-213">Subsequent parameters are passed to the component.</span></span> <span data-ttu-id="123d8-214">`InvokeAsync` kann eine arbiträre Anzahl von Argumenten annehmen.</span><span class="sxs-lookup"><span data-stu-id="123d8-214">`InvokeAsync` can take an arbitrary number of arguments.</span></span>

<span data-ttu-id="123d8-215">Testen der App</span><span class="sxs-lookup"><span data-stu-id="123d8-215">Test the app.</span></span> <span data-ttu-id="123d8-216">In der folgenden Abbildung werden die ToDo-Liste und die Elemente mit Priorität angezeigt:</span><span class="sxs-lookup"><span data-stu-id="123d8-216">The following image shows the ToDo list and the priority items:</span></span>

![ToDo-Liste und Prioritätselemente](view-components/_static/pi.png)

<span data-ttu-id="123d8-218">Sie können Sie Ansichtskomponente auch direkt vom Controller aus aufrufen:</span><span class="sxs-lookup"><span data-stu-id="123d8-218">You can also call the view component directly from the controller:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![Prioritätselemente der IndexVC-Aktion](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a><span data-ttu-id="123d8-220">Angeben eines Ansichtsnamens</span><span class="sxs-lookup"><span data-stu-id="123d8-220">Specifying a view name</span></span>

<span data-ttu-id="123d8-221">Eine komplexe Ansichtskomponente erfordert möglicherweise, dass unter bestimmten Umständen eine Ansicht angegeben wird, die nicht dem Standard entspricht.</span><span class="sxs-lookup"><span data-stu-id="123d8-221">A complex view component might need to specify a non-default view under some conditions.</span></span> <span data-ttu-id="123d8-222">Der folgende Code zeigt, wie die Ansicht „PVC“ von der `InvokeAsync`-Methode aus angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="123d8-222">The following code shows how to specify the "PVC" view  from the `InvokeAsync` method.</span></span> <span data-ttu-id="123d8-223">Aktualisieren Sie die `InvokeAsync`-Methode in der `PriorityListViewComponent`-Klasse.</span><span class="sxs-lookup"><span data-stu-id="123d8-223">Update the `InvokeAsync` method in the `PriorityListViewComponent` class.</span></span>

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

<span data-ttu-id="123d8-224">Kopieren Sie die Datei *Views/Shared/Components/PriorityList/Default.cshtml* in eine Ansicht mit dem Namen *Views/Shared/Components/PriorityList/PVC.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="123d8-224">Copy the *Views/Shared/Components/PriorityList/Default.cshtml* file to a view named *Views/Shared/Components/PriorityList/PVC.cshtml*.</span></span> <span data-ttu-id="123d8-225">Fügen Sie eine Überschrift hinzu, um anzugeben, dass die PVC-Ansicht verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="123d8-225">Add a heading to indicate the PVC view is being used.</span></span>

[!code-cshtml[](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

<span data-ttu-id="123d8-226">Aktualisieren Sie *Views/TodoList/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="123d8-226">Update *Views/TodoList/Index.cshtml*:</span></span>

<!-- Views/TodoList/Index.cshtml is never imported, so change to test tutorial -->

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="123d8-227">Führen Sie die App aus und überprüfen Sie die PVC-Ansicht.</span><span class="sxs-lookup"><span data-stu-id="123d8-227">Run the app and verify PVC view.</span></span>

![Ansichtskomponente mit Priorität](view-components/_static/pvc.png)

<span data-ttu-id="123d8-229">Wenn die PVC-Ansicht nicht gerendert wird, stellen Sie sicher, dass Sie die Ansichtskomponente mit einer Priorität von 4 oder höher aufrufen.</span><span class="sxs-lookup"><span data-stu-id="123d8-229">If the PVC view isn't rendered, verify you are calling the view component with a priority of 4 or higher.</span></span>

### <a name="examine-the-view-path"></a><span data-ttu-id="123d8-230">Untersuchen des Ansichtspfads</span><span class="sxs-lookup"><span data-stu-id="123d8-230">Examine the view path</span></span>

* <span data-ttu-id="123d8-231">Ändern Sie den Prioritätsparameter in drei oder weniger, damit die Prioritätsansicht nicht zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="123d8-231">Change the priority parameter to three or less so the priority view isn't returned.</span></span>
* <span data-ttu-id="123d8-232">Benennen Sie *Views/Todo/Components/PriorityList/Default.cshtml* vorrübergehend in *1Default.cshtml* um.</span><span class="sxs-lookup"><span data-stu-id="123d8-232">Temporarily rename the *Views/Todo/Components/PriorityList/Default.cshtml* to *1Default.cshtml*.</span></span>
* <span data-ttu-id="123d8-233">Wenn Sie die App testen, erhalten Sie die folgende Fehlermeldung:</span><span class="sxs-lookup"><span data-stu-id="123d8-233">Test the app, you'll get the following error:</span></span>

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' wasn't found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* <span data-ttu-id="123d8-234">Kopieren Sie *Views/Todo/Components/PriorityList/1Default.cshtml* nach *Views/Shared/Components/PriorityList/Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="123d8-234">Copy *Views/Todo/Components/PriorityList/1Default.cshtml* to *Views/Shared/Components/PriorityList/Default.cshtml*.</span></span>
* <span data-ttu-id="123d8-235">Fügen Sie der ToDo-Ansichtskomponentenansicht *Shared* (Freigegeben) Markup hinzu, um anzugeben, dass die Ansicht aus dem Ordner *Shared* stammt.</span><span class="sxs-lookup"><span data-stu-id="123d8-235">Add some markup to the *Shared* Todo view component view to indicate the view is from the *Shared* folder.</span></span>
* <span data-ttu-id="123d8-236">Testen Sie die Komponentenansicht **Shared** (Freigegeben).</span><span class="sxs-lookup"><span data-stu-id="123d8-236">Test the **Shared** component view.</span></span>

![ToDo-Ausgabe mit Komponentenansicht „Shared“](view-components/_static/shared.png)

### <a name="avoiding-magic-strings"></a><span data-ttu-id="123d8-238">Vermeiden „magischer“ Zeichenfolgen</span><span class="sxs-lookup"><span data-stu-id="123d8-238">Avoiding magic strings</span></span>

<span data-ttu-id="123d8-239">Wenn Sie Sicherheit zu Kompilierzeit haben möchten, können Sie den hart codierten Komponentennamen durch den Klassennamen ersetzen.</span><span class="sxs-lookup"><span data-stu-id="123d8-239">If you want compile time safety, you can replace the hard-coded view component name with the class name.</span></span> <span data-ttu-id="123d8-240">Erstellen Sie die Ansichtskomponente ohne den Suffix „ViewComponent“:</span><span class="sxs-lookup"><span data-stu-id="123d8-240">Create the view component without the "ViewComponent" suffix:</span></span>

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

<span data-ttu-id="123d8-241">Fügen Sie eine `using`-Anweisung zu Ihrer Razor-Ansichtsdatei hinzu, und verwenden Sie den `nameof`-Operator:</span><span class="sxs-lookup"><span data-stu-id="123d8-241">Add a `using` statement to your Razor view file, and use the `nameof` operator:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexNameof.cshtml?range=1-6,35-)]

## <a name="additional-resources"></a><span data-ttu-id="123d8-242">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="123d8-242">Additional resources</span></span>

* [<span data-ttu-id="123d8-243">Abhängigkeitsinjektion in Ansichten</span><span class="sxs-lookup"><span data-stu-id="123d8-243">Dependency injection into views</span></span>](xref:mvc/views/dependency-injection)
