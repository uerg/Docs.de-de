---
title: Anchor-Tag-Hilfsprogramm in ASP.NET Core
author: pkellner
description: Lernen Sie die Attribute für das ASP.NET Core-Anchor-Taghilfsprogramm kennen und erfahren Sie, welche Rolle jedes Attribut bei der Erweiterung des Verhaltens des HTML-Anchor-Tags spielt.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 01/31/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 31ff62b6bedb5e577a51f341c89d241d06a83ad3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30899407"
---
# <a name="anchor-tag-helper-in-aspnet-core"></a><span data-ttu-id="c8319-103">Anchor-Tag-Hilfsprogramm in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c8319-103">Anchor Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="c8319-104">Von [Peter Kellner](http://peterkellner.net) und [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="c8319-104">By [Peter Kellner](http://peterkellner.net) and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="c8319-105">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c8319-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="c8319-106">Das [Anchor-Taghilfsprogramm](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper) verbessert das HTML-Anchor-Tag (`<a ... ></a>`), indem neue Attribute hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="c8319-106">The [Anchor Tag Helper](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper) enhances the standard HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="c8319-107">Per Konvention erhalten Attributnamen das Präfix `asp-`.</span><span class="sxs-lookup"><span data-stu-id="c8319-107">By convention, the attribute names are prefixed with `asp-`.</span></span> <span data-ttu-id="c8319-108">Der Attributwert `href` des gerenderten Anchor-Elements wird von den Werten der `asp-`-Attribute bestimmt.</span><span class="sxs-lookup"><span data-stu-id="c8319-108">The rendered anchor element's `href` attribute value is determined by the values of the `asp-` attributes.</span></span>

<span data-ttu-id="c8319-109">Der folgende *SpeakerController* wird in den Beispielen in diesem Dokument verwendet:</span><span class="sxs-lookup"><span data-stu-id="c8319-109">*SpeakerController* is used in samples throughout this document:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?name=snippet_SpeakerController)]

<span data-ttu-id="c8319-110">Eine Liste der `asp-`-Attribute folgt.</span><span class="sxs-lookup"><span data-stu-id="c8319-110">An inventory of the `asp-` attributes follows.</span></span>

## <a name="asp-controller"></a><span data-ttu-id="c8319-111">asp-controller</span><span class="sxs-lookup"><span data-stu-id="c8319-111">asp-controller</span></span>

<span data-ttu-id="c8319-112">Das Attribut [asp-controller](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.controller) weist den Controller zu, der zum Generieren der URL verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="c8319-112">The [asp-controller](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.controller) attribute assigns the controller used for generating the URL.</span></span> <span data-ttu-id="c8319-113">Im folgenden Markup werden alle Lautsprecher aufgeführt:</span><span class="sxs-lookup"><span data-stu-id="c8319-113">The following markup lists all speakers:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspController)]

<span data-ttu-id="c8319-114">Der generierte HTML-Code:</span><span class="sxs-lookup"><span data-stu-id="c8319-114">The generated HTML:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="c8319-115">Wenn das Attribut `asp-controller` angegeben wird und `asp-action` nicht, entspricht der `asp-action`-Standardwert der Controlleraktion, die der aktuell ausgeführten Ansicht zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="c8319-115">If the `asp-controller` attribute is specified and `asp-action` isn't, the default `asp-action` value is the controller action associated with the currently executing view.</span></span> <span data-ttu-id="c8319-116">Wenn `asp-action` im obigen Markup ausgelassen wird und das Anchor-Taghilfsprogramm in der Ansicht *Index* von *HomeController* verwendet wird (*/Home*), wird folgender HTML-Code generiert:</span><span class="sxs-lookup"><span data-stu-id="c8319-116">If `asp-action` is omitted from the preceding markup, and the Anchor Tag Helper is used in *HomeController*'s *Index* view (*/Home*), the generated HTML is:</span></span>

```html
<a href="/Home">All Speakers</a>
```

## <a name="asp-action"></a><span data-ttu-id="c8319-117">asp-action</span><span class="sxs-lookup"><span data-stu-id="c8319-117">asp-action</span></span>

<span data-ttu-id="c8319-118">Der Attributwert [asp-action](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.action) repräsentiert den Namen der Controlleraktion, die im generierten `href`-Attribut enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="c8319-118">The [asp-action](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.action) attribute value represents the controller action name included in the generated `href` attribute.</span></span> <span data-ttu-id="c8319-119">Im folgenden Markup wird der generierte `href`-Attributwert auf die Auswertungsseite des Lautsprechers festgelegt:</span><span class="sxs-lookup"><span data-stu-id="c8319-119">The following markup sets the generated `href` attribute value to the speaker evaluations page:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAction)]

<span data-ttu-id="c8319-120">Der generierte HTML-Code:</span><span class="sxs-lookup"><span data-stu-id="c8319-120">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

<span data-ttu-id="c8319-121">Wenn kein `asp-controller`-Attribut angegeben ist, wird der Standardcontroller zum Aufruf der Ansicht verwendet, die die aktuelle Ansicht ausführt.</span><span class="sxs-lookup"><span data-stu-id="c8319-121">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view is used.</span></span>

<span data-ttu-id="c8319-122">Wenn der Wert des `asp-action`-Attribut `Index` lautet, wird keine Aktion an die URL angefügt, was zum Aufruf der `Index`-Standardaktion führt.</span><span class="sxs-lookup"><span data-stu-id="c8319-122">If the `asp-action` attribute value is `Index`, then no action is appended to the URL, leading to the invocation of the default `Index` action.</span></span> <span data-ttu-id="c8319-123">Die angegebene (oder standardmäßige) Aktion muss im Controller vorhanden sein, auf den in `asp-controller` verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="c8319-123">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

## <a name="asp-route-value"></a><span data-ttu-id="c8319-124">asp-route-{value}</span><span class="sxs-lookup"><span data-stu-id="c8319-124">asp-route-{value}</span></span>

<span data-ttu-id="c8319-125">Das Attribut [asp-route{value}](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) aktiviert ein Platzhalterroutenpräfix.</span><span class="sxs-lookup"><span data-stu-id="c8319-125">The [asp-route-{value}](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) attribute enables a wildcard route prefix.</span></span> <span data-ttu-id="c8319-126">Ein beliebiger Wert im Platzhalter `{value}` wird als potenzieller Routenparameter interpretiert.</span><span class="sxs-lookup"><span data-stu-id="c8319-126">Any value occupying the `{value}` placeholder is interpreted as a potential route parameter.</span></span> <span data-ttu-id="c8319-127">Wenn keine Standardroute gefunden wird, wird dieses Routenpräfix als Anforderungsparameter und -wert an das generierte `href`-Attribut angefügt.</span><span class="sxs-lookup"><span data-stu-id="c8319-127">If a default route isn't found, this route prefix is appended to the generated `href` attribute as a request parameter and value.</span></span> <span data-ttu-id="c8319-128">Andernfalls wird es in der Routenvorlage ersetzt.</span><span class="sxs-lookup"><span data-stu-id="c8319-128">Otherwise, it's substituted in the route template.</span></span>

<span data-ttu-id="c8319-129">Betrachten Sie die folgende Controlleraktion:</span><span class="sxs-lookup"><span data-stu-id="c8319-129">Consider the following controller action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/BuiltInTagController.cs?name=snippet_AnchorTagHelperAction)]

<span data-ttu-id="c8319-130">Mit einer in *Startup.Configure* definierten Standardroutenvorlage:</span><span class="sxs-lookup"><span data-stu-id="c8319-130">With a default route template defined in *Startup.Configure*:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=8-10)]

<span data-ttu-id="c8319-131">Die MVC-Ansicht verwendet das von der Aktion bereitgestellte Modell wie folgt:</span><span class="sxs-lookup"><span data-stu-id="c8319-131">The MVC view uses the model, provided by the action, as follows:</span></span>

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker"
       asp-action="Detail" 
       asp-route-id="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
</body>
</html>
```

<span data-ttu-id="c8319-132">Der `{id?}`-Platzhalter der Standardroute wurde abgeglichen.</span><span class="sxs-lookup"><span data-stu-id="c8319-132">The default route's `{id?}` placeholder was matched.</span></span> <span data-ttu-id="c8319-133">Der generierte HTML-Code:</span><span class="sxs-lookup"><span data-stu-id="c8319-133">The generated HTML:</span></span>

```html
<a href="/Speaker/Detail/12">SpeakerId: 12</a>
```

<span data-ttu-id="c8319-134">Angenommen, das Routenpräfix ist (wie in der folgenden MVC-Ansicht gezeigt) nicht Teil der übereinstimmenden Routingvorlage:</span><span class="sxs-lookup"><span data-stu-id="c8319-134">Assume the route prefix isn't part of the matching routing template, as with the following MVC view:</span></span>

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker" 
       asp-action="Detail" 
       asp-route-speakerid="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
<body>
</html>
```

<span data-ttu-id="c8319-135">Der folgende HTML-Code wird generiert, weil `speakerid` nicht in der übereinstimmenden Route gefunden wurde:</span><span class="sxs-lookup"><span data-stu-id="c8319-135">The following HTML is generated because `speakerid` wasn't found in the matching route:</span></span>

```html
<a href="/Speaker/Detail?speakerid=12">SpeakerId: 12</a>
```

<span data-ttu-id="c8319-136">Wenn entweder `asp-controller` oder `asp-action` nicht angegeben werden, erfolgt der gleiche Standardverarbeitungsprozess wie beim `asp-route`-Attribut.</span><span class="sxs-lookup"><span data-stu-id="c8319-136">If either `asp-controller` or `asp-action` aren't specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

## <a name="asp-route"></a><span data-ttu-id="c8319-137">asp-route</span><span class="sxs-lookup"><span data-stu-id="c8319-137">asp-route</span></span>

<span data-ttu-id="c8319-138">Das Attribut [asp-route](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.route) wird zum Erstellen einer URL verwendet, die direkt auf eine benannte Route verweist.</span><span class="sxs-lookup"><span data-stu-id="c8319-138">The [asp-route](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.route) attribute is used for creating a URL linking directly to a named route.</span></span> <span data-ttu-id="c8319-139">Mit der Verwendung von [Routingattributen](xref:mvc/controllers/routing#attribute-routing) kann eine Route wie in `SpeakerController` gezeigt benannt und in der zugehörigen `Evaluations`-Aktion verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="c8319-139">Using [routing attributes](xref:mvc/controllers/routing#attribute-routing), a route can be named as shown in the `SpeakerController` and used in its `Evaluations` action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=22-24)]

<span data-ttu-id="c8319-140">Im folgenden Markup verweist das `asp-route`-Attribut auf die benannte Route:</span><span class="sxs-lookup"><span data-stu-id="c8319-140">In the following markup, the `asp-route` attribute references the named route:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspRoute)]

<span data-ttu-id="c8319-141">Das Anchor-Taghilfsprogramm generiert eine Route direkt zu dieser Controlleraktion. Dabei wird die URL */Speaker/Evaluations* verwendet.</span><span class="sxs-lookup"><span data-stu-id="c8319-141">The Anchor Tag Helper generates a route directly to that controller action using the URL */Speaker/Evaluations*.</span></span> <span data-ttu-id="c8319-142">Der generierte HTML-Code:</span><span class="sxs-lookup"><span data-stu-id="c8319-142">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

<span data-ttu-id="c8319-143">Wenn `asp-controller` oder `asp-action` zusätzlich zu `asp-route` angegeben sind, entspricht die generierte Route möglicherweise nicht Ihren Erwartungen.</span><span class="sxs-lookup"><span data-stu-id="c8319-143">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="c8319-144">Um einen Routenkonflikt zu vermeiden, darf `asp-route` nicht mit den Attributen `asp-controller` oder `asp-action` verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="c8319-144">To avoid a route conflict, `asp-route` shouldn't be used with the `asp-controller` and `asp-action` attributes.</span></span>

## <a name="asp-all-route-data"></a><span data-ttu-id="c8319-145">asp-all-route-data</span><span class="sxs-lookup"><span data-stu-id="c8319-145">asp-all-route-data</span></span>

<span data-ttu-id="c8319-146">Das Attribut [asp-all-route-data](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) unterstützt das Erstellen eines Wörterbuchs aus Schlüssel-Wert-Paaren.</span><span class="sxs-lookup"><span data-stu-id="c8319-146">The [asp-all-route-data](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) attribute supports the creation of a dictionary of key-value pairs.</span></span> <span data-ttu-id="c8319-147">Der Schlüssel ist der Parametername, der Wert ist der Parameterwert.</span><span class="sxs-lookup"><span data-stu-id="c8319-147">The key is the parameter name, and the value is the parameter value.</span></span>

<span data-ttu-id="c8319-148">Im folgenden Beispiel wird ein Wörterbuch initialisiert und an eine Razor-Ansicht übergeben.</span><span class="sxs-lookup"><span data-stu-id="c8319-148">In the following example, a dictionary is initialized and passed to a Razor view.</span></span> <span data-ttu-id="c8319-149">Alternativ können die Daten auch mit Ihrem Modell übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="c8319-149">Alternatively, the data could be passed in with your model.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAllRouteData)]

<span data-ttu-id="c8319-150">Der oben gezeigte Code generiert den folgenden HTML-Code:</span><span class="sxs-lookup"><span data-stu-id="c8319-150">The preceding code generates the following HTML:</span></span>

```html
<a href="/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true">Speaker Evaluations</a>
```

<span data-ttu-id="c8319-151">Das `asp-all-route-data`-Wörterbuch wird vereinfacht, um eine Abfragezeichenfolge zu erstellen, die die Anforderungen der überladenen `Evaluations`-Aktion erfüllt:</span><span class="sxs-lookup"><span data-stu-id="c8319-151">The `asp-all-route-data` dictionary is flattened to produce a querystring meeting the requirements of the overloaded `Evaluations` action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=26-30)]

<span data-ttu-id="c8319-152">Wenn ein beliebiger Schlüssel im Wörterbuch mit einem Routenparameter übereinstimmt, werden die Werte entsprechend in der Route eingesetzt.</span><span class="sxs-lookup"><span data-stu-id="c8319-152">If any keys in the dictionary match route parameters, those values are substituted in the route as appropriate.</span></span> <span data-ttu-id="c8319-153">Die nicht übereinstimmenden Werte werden als Anforderungsparameter generiert.</span><span class="sxs-lookup"><span data-stu-id="c8319-153">The other non-matching values are generated as request parameters.</span></span>

## <a name="asp-fragment"></a><span data-ttu-id="c8319-154">asp-fragment</span><span class="sxs-lookup"><span data-stu-id="c8319-154">asp-fragment</span></span>

<span data-ttu-id="c8319-155">Das Attribut [asp-fragment](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.fragment) definiert ein URL-Fragment, das an die URL angefügt werden soll.</span><span class="sxs-lookup"><span data-stu-id="c8319-155">The [asp-fragment](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.fragment) attribute defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="c8319-156">Das Anchor-Taghilfsprogramm fügt das Hashzeichen (#) hinzu.</span><span class="sxs-lookup"><span data-stu-id="c8319-156">The Anchor Tag Helper adds the hash character (#).</span></span> <span data-ttu-id="c8319-157">Sehen Sie sich das folgende Markup an:</span><span class="sxs-lookup"><span data-stu-id="c8319-157">Consider the following markup:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspFragment)]

<span data-ttu-id="c8319-158">Der generierte HTML-Code:</span><span class="sxs-lookup"><span data-stu-id="c8319-158">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations#SpeakerEvaluations">Speaker Evaluations</a>
```

<span data-ttu-id="c8319-159">Hashtags sind beim Erstellen von clientseitigen Apps nützlich.</span><span class="sxs-lookup"><span data-stu-id="c8319-159">Hash tags are useful when building client-side apps.</span></span> <span data-ttu-id="c8319-160">Sie können beispielsweise zum einfachen Markieren und Suchen in JavaScript verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="c8319-160">They can be used for easy marking and searching in JavaScript, for example.</span></span>

## <a name="asp-area"></a><span data-ttu-id="c8319-161">asp-area</span><span class="sxs-lookup"><span data-stu-id="c8319-161">asp-area</span></span>

<span data-ttu-id="c8319-162">Das Attribut [asp-area](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.area) legt den Bereichsnamen zum Festlegen der geeigneten Route fest.</span><span class="sxs-lookup"><span data-stu-id="c8319-162">The [asp-area](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.area) attribute sets the area name used to set the appropriate route.</span></span> <span data-ttu-id="c8319-163">Das folgende Beispiel zeigt, wie das Bereichsattribut eine Neuzuordnung von Routen bewirken kann.</span><span class="sxs-lookup"><span data-stu-id="c8319-163">The following example depicts how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="c8319-164">Das Festlegen von `asp-area` auf „Blogs“ stellt das Verzeichnis *Areas/Blogs* den Routen der zugeordneten Controller und Ansichten für dieses Anchor-Tag voran.</span><span class="sxs-lookup"><span data-stu-id="c8319-164">Setting `asp-area` to "Blogs" prefixes the directory *Areas/Blogs* to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="c8319-165">**<Projektname\>**</span><span class="sxs-lookup"><span data-stu-id="c8319-165">**<Project name\>**</span></span>
  * <span data-ttu-id="c8319-166">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="c8319-166">**wwwroot**</span></span>
  * <span data-ttu-id="c8319-167">**Bereiche**</span><span class="sxs-lookup"><span data-stu-id="c8319-167">**Areas**</span></span>
    * <span data-ttu-id="c8319-168">**Blogs**</span><span class="sxs-lookup"><span data-stu-id="c8319-168">**Blogs**</span></span>
      * <span data-ttu-id="c8319-169">**Controller**</span><span class="sxs-lookup"><span data-stu-id="c8319-169">**Controllers**</span></span>
        * <span data-ttu-id="c8319-170">*HomeController.cs*</span><span class="sxs-lookup"><span data-stu-id="c8319-170">*HomeController.cs*</span></span>
      * <span data-ttu-id="c8319-171">**Ansichten**</span><span class="sxs-lookup"><span data-stu-id="c8319-171">**Views**</span></span>
        * <span data-ttu-id="c8319-172">**Home**</span><span class="sxs-lookup"><span data-stu-id="c8319-172">**Home**</span></span>
          * <span data-ttu-id="c8319-173">*AboutBlog.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c8319-173">*AboutBlog.cshtml*</span></span>
          * <span data-ttu-id="c8319-174">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c8319-174">*Index.cshtml*</span></span>
        * <span data-ttu-id="c8319-175">*_ViewStart.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c8319-175">*_ViewStart.cshtml*</span></span>
  * <span data-ttu-id="c8319-176">**Controller**</span><span class="sxs-lookup"><span data-stu-id="c8319-176">**Controllers**</span></span>

<span data-ttu-id="c8319-177">Gemäß der obigen Verzeichnishierarchie lautet das Markup zum Verweis auf die Datei *AboutBlog.cshtml* wie folgt:</span><span class="sxs-lookup"><span data-stu-id="c8319-177">Given the preceding directory hierarchy, the markup to reference the *AboutBlog.cshtml* file is:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspArea)]

<span data-ttu-id="c8319-178">Der generierte HTML-Code:</span><span class="sxs-lookup"><span data-stu-id="c8319-178">The generated HTML:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">About Blog</a>
```

> [!TIP]
> <span data-ttu-id="c8319-179">Die Routenvorlage muss einen Verweis auf den Bereich enthalten, damit Bereiche in einer MVC-App funktionieren.</span><span class="sxs-lookup"><span data-stu-id="c8319-179">For areas to work in an MVC app, the route template must include a reference to the area, if it exists.</span></span> <span data-ttu-id="c8319-180">Diese Vorlage wird durch den zweiten Parameter des `routes.MapRoute`-Methodenaufrufs in *Startup.Configure* repräsentiert: [!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=5)]</span><span class="sxs-lookup"><span data-stu-id="c8319-180">That template is represented by the second parameter of the `routes.MapRoute` method call in *Startup.Configure*: [!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=5)]</span></span>

## <a name="asp-protocol"></a><span data-ttu-id="c8319-181">asp-protocol</span><span class="sxs-lookup"><span data-stu-id="c8319-181">asp-protocol</span></span>

<span data-ttu-id="c8319-182">Das Attribut [asp-protocol](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.protocol) gibt ein Protokoll in Ihrer URL an (z.B. `https`).</span><span class="sxs-lookup"><span data-stu-id="c8319-182">The [asp-protocol](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.protocol) attribute is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="c8319-183">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="c8319-183">For example:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspProtocol)]

<span data-ttu-id="c8319-184">Der generierte HTML-Code:</span><span class="sxs-lookup"><span data-stu-id="c8319-184">The generated HTML:</span></span>

```html
<a href="https://localhost/Home/About">About</a>
```

<span data-ttu-id="c8319-185">Der Hostname im Beispiel lautet „localhost“, aber das Anchor-Taghilfsprogramm verwendet die öffentliche Domäne der Website beim Generieren der URL.</span><span class="sxs-lookup"><span data-stu-id="c8319-185">The host name in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

## <a name="asp-host"></a><span data-ttu-id="c8319-186">asp-host</span><span class="sxs-lookup"><span data-stu-id="c8319-186">asp-host</span></span>

<span data-ttu-id="c8319-187">Mit dem Attribut [asp-host](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.host) wird ein Hostname in Ihrer URL angegeben.</span><span class="sxs-lookup"><span data-stu-id="c8319-187">The [asp-host](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.host) attribute is for specifying a host name in your URL.</span></span> <span data-ttu-id="c8319-188">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="c8319-188">For example:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspHost)]

<span data-ttu-id="c8319-189">Der generierte HTML-Code:</span><span class="sxs-lookup"><span data-stu-id="c8319-189">The generated HTML:</span></span>

```html
<a href="https://microsoft.com/Home/About">About</a>
```

## <a name="asp-page"></a><span data-ttu-id="c8319-190">asp-page</span><span class="sxs-lookup"><span data-stu-id="c8319-190">asp-page</span></span>

<span data-ttu-id="c8319-191">Das Attribut [asp-page](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.page) wird mit Razor Pages verwendet.</span><span class="sxs-lookup"><span data-stu-id="c8319-191">The [asp-page](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.page) attribute is used with Razor Pages.</span></span> <span data-ttu-id="c8319-192">Mit ihm wird der `href`-Attributwerts des Anchor-Tags auf eine bestimmte Seite festgelegt.</span><span class="sxs-lookup"><span data-stu-id="c8319-192">Use it to set an anchor tag's `href` attribute value to a specific page.</span></span> <span data-ttu-id="c8319-193">Wenn Sie dem Seitennamen einen Schrägstrich „/“ voranstellen, wird die URL erstellt.</span><span class="sxs-lookup"><span data-stu-id="c8319-193">Prefixing the page name with a forward slash ("/") creates the URL.</span></span>

<span data-ttu-id="c8319-194">Das folgende Beispiel zeigt auf die Razor Page des Teilnehmers:</span><span class="sxs-lookup"><span data-stu-id="c8319-194">The following sample points to the attendee Razor Page:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPage)]

<span data-ttu-id="c8319-195">Der generierte HTML-Code:</span><span class="sxs-lookup"><span data-stu-id="c8319-195">The generated HTML:</span></span>

```html
<a href="/Attendee">All Attendees</a>
```

<span data-ttu-id="c8319-196">Das `asp-page`-Attribut und die `asp-route`-, `asp-controller`- und `asp-action`-Attribute schließen sich gegenseitig aus.</span><span class="sxs-lookup"><span data-stu-id="c8319-196">The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes.</span></span> <span data-ttu-id="c8319-197">Allerdings kann `asp-page` mit `asp-route-{value}` genutzt werden, um das Routing zu steuern, wie im folgenden Markup veranschaulicht wird:</span><span class="sxs-lookup"><span data-stu-id="c8319-197">However, `asp-page` can be used with `asp-route-{value}` to control routing, as the following markup demonstrates:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageAspRouteId)]

<span data-ttu-id="c8319-198">Der generierte HTML-Code:</span><span class="sxs-lookup"><span data-stu-id="c8319-198">The generated HTML:</span></span>

```html
<a href="/Attendee?attendeeid=10">View Attendee</a>
```

## <a name="asp-page-handler"></a><span data-ttu-id="c8319-199">asp-page-handler</span><span class="sxs-lookup"><span data-stu-id="c8319-199">asp-page-handler</span></span>

<span data-ttu-id="c8319-200">Das Attribut [asp-page-handler](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.pagehandler) wird mit Razor Pages verwendet.</span><span class="sxs-lookup"><span data-stu-id="c8319-200">The [asp-page-handler](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.pagehandler) attribute is used with Razor Pages.</span></span> <span data-ttu-id="c8319-201">Es dient zur Verknüpfung mit bestimmten Seitenhandlern.</span><span class="sxs-lookup"><span data-stu-id="c8319-201">It's intended for linking to specific page handlers.</span></span>

<span data-ttu-id="c8319-202">Betrachten Sie den folgenden Seitenhandler:</span><span class="sxs-lookup"><span data-stu-id="c8319-202">Consider the following page handler:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Attendee.cshtml.cs?name=snippet_OnGetProfileHandler)]

<span data-ttu-id="c8319-203">Das Markup des Seitenmodells verweist auf den Seitenhandler `OnGetProfile`.</span><span class="sxs-lookup"><span data-stu-id="c8319-203">The page model's associated markup links to the `OnGetProfile` page handler.</span></span> <span data-ttu-id="c8319-204">Beachten Sie, dass das Präfix `On<Verb>` des Methodennamens für den Seitenhandler im Attributwert `asp-page-handler` ausgelassen wurde.</span><span class="sxs-lookup"><span data-stu-id="c8319-204">Note that the `On<Verb>` prefix of the page handler method name is omitted in the `asp-page-handler` attribute value.</span></span> <span data-ttu-id="c8319-205">Würde es sich um eine asynchrone Methode handeln, würde auch das Suffix `Async` ausgelassen.</span><span class="sxs-lookup"><span data-stu-id="c8319-205">If this were an asynchronous method, the `Async` suffix would be omitted too.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageHandler)]

<span data-ttu-id="c8319-206">Der generierte HTML-Code:</span><span class="sxs-lookup"><span data-stu-id="c8319-206">The generated HTML:</span></span>

```html
<a href="/Attendee?attendeeid=12&handler=Profile">Attendee Profile</a>
```

## <a name="additional-resources"></a><span data-ttu-id="c8319-207">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="c8319-207">Additional resources</span></span>

* [<span data-ttu-id="c8319-208">Bereiche</span><span class="sxs-lookup"><span data-stu-id="c8319-208">Areas</span></span>](xref:mvc/controllers/areas)
* [<span data-ttu-id="c8319-209">Einführung in Razor Pages</span><span class="sxs-lookup"><span data-stu-id="c8319-209">Intro to Razor Pages</span></span>](xref:mvc/razor-pages/index)
