---
title: Anchor-Tag-Hilfsprogramm in ASP.NET Core
author: pkellner
description: Zeigt die Arbeit mit dem Anchor-Taghilfsprogramm
manager: wpickett
ms.author: riande
ms.date: 12/20/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 404fc7bc3b35114066f035e1ac28d10a8279ccbc
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="anchor-tag-helper"></a><span data-ttu-id="6548c-103">Anchor-Taghilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="6548c-103">Anchor Tag Helper</span></span>

<span data-ttu-id="6548c-104">Von [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="6548c-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="6548c-105">Das Anchor-Taghilfsprogramm verbessert das HTML-Anchor-Tag (`<a ... ></a>`), indem neue Attribute hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="6548c-105">The Anchor Tag Helper enhances the HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="6548c-106">Der generierte Link (auf dem `href`-Tag) wird mit den neuen Attributen erstellt.</span><span class="sxs-lookup"><span data-stu-id="6548c-106">The link generated (on the `href` tag) is created using the new attributes.</span></span> <span data-ttu-id="6548c-107">Diese URL kann ein optionales Protokoll wie Https enthalten.</span><span class="sxs-lookup"><span data-stu-id="6548c-107">That URL can include an optional protocol such as https.</span></span>

<span data-ttu-id="6548c-108">Der nachfolgende SpeakerController wird in Beispielen in diesem Dokument verwendet.</span><span class="sxs-lookup"><span data-stu-id="6548c-108">The speaker controller below is used in samples in this document.</span></span>

<span data-ttu-id="6548c-109">**SpeakerController.cs**</span><span class="sxs-lookup"><span data-stu-id="6548c-109">**SpeakerController.cs**</span></span> 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a><span data-ttu-id="6548c-110">Attribute von Anchor-Taghilfsprogrammen</span><span class="sxs-lookup"><span data-stu-id="6548c-110">Anchor Tag Helper Attributes</span></span>

### <a name="asp-controller"></a><span data-ttu-id="6548c-111">asp-controller</span><span class="sxs-lookup"><span data-stu-id="6548c-111">asp-controller</span></span>

<span data-ttu-id="6548c-112">`asp-controller` dient dazu, die Controller zuzuordnen, die zum Generieren der URL verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="6548c-112">`asp-controller` is used to associate which controller will be used to generate the URL.</span></span> <span data-ttu-id="6548c-113">Die angegebenen Controller müssen im aktuellen Projekt vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="6548c-113">The controllers specified must exist in the current project.</span></span> <span data-ttu-id="6548c-114">Der folgende Code listet alle Lautsprecher auf:</span><span class="sxs-lookup"><span data-stu-id="6548c-114">The following code lists all speakers:</span></span> 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

<span data-ttu-id="6548c-115">Das generierte Markup sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="6548c-115">The generated markup will be:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="6548c-116">Wenn `asp-controller` angegeben und `asp-action` nicht angegeben ist, wird die Standardcontrollermethode der aktuell ausgeführten Ansicht als Standardeinstellung `asp-action` verwendet.</span><span class="sxs-lookup"><span data-stu-id="6548c-116">If the `asp-controller` is specified and `asp-action` isn't, the default `asp-action` will be the default controller method of the currently executing view.</span></span> <span data-ttu-id="6548c-117">Wird im obigen Beispiel `asp-action` ausgelassen und das Anchor-Taghilfsprogramm aus der `Index`-Ansicht des *HomeController* (**/Home**) generiert, so sieht das generierte Markup wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="6548c-117">That is, in the above example, if `asp-action` is left out, and this Anchor Tag Helper is generated from *HomeController*'s `Index` view (**/Home**), the generated markup will be:</span></span>

```html
<a href="/Home">All Speakers</a>
```

### <a name="asp-action"></a><span data-ttu-id="6548c-118">asp-action</span><span class="sxs-lookup"><span data-stu-id="6548c-118">asp-action</span></span>

<span data-ttu-id="6548c-119">`asp-action` ist der Name der Aktionsmethode im Controller, die im generierten `href`-Attribut enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="6548c-119">`asp-action` is the name of the action method in the controller that will be included in the generated `href`.</span></span> <span data-ttu-id="6548c-120">Der folgende Code legt das generierte `href`-Attribut beispielsweise so fest, dass es auf die Detailseite des Lautsprechers verweist:</span><span class="sxs-lookup"><span data-stu-id="6548c-120">For example, the following code set the generated `href` to point to the speaker detail page:</span></span>

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

<span data-ttu-id="6548c-121">Das generierte Markup sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="6548c-121">The generated markup will be:</span></span>

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

<span data-ttu-id="6548c-122">Wenn kein `asp-controller`-Attribut angegeben ist, wird der Standardcontroller verwendet, der die Ansicht aufruft, die die aktuelle Ansicht ausführt.</span><span class="sxs-lookup"><span data-stu-id="6548c-122">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view will be used.</span></span>  
 
<span data-ttu-id="6548c-123">Wenn das `asp-action`-Attribut `Index` ist, so wird keine Aktion an die URL angefügt, wodurch die `Index`-Standardmethode aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="6548c-123">If the attribute `asp-action` is `Index`, then no action is appended to the URL, leading to the default `Index` method being called.</span></span> <span data-ttu-id="6548c-124">Die angegebene (oder standardmäßige) Aktion muss im Controller vorhanden sein, auf den in `asp-controller` verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="6548c-124">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

### <a name="asp-page"></a><span data-ttu-id="6548c-125">asp-page</span><span class="sxs-lookup"><span data-stu-id="6548c-125">asp-page</span></span>

<span data-ttu-id="6548c-126">Verwenden Sie das `asp-page`-Attribut in einem Anchor-Tag, damit die URL auf eine bestimmten Seite verweist.</span><span class="sxs-lookup"><span data-stu-id="6548c-126">Use the `asp-page` attribute in an anchor tag to set its URL to point to a specific page.</span></span> <span data-ttu-id="6548c-127">Wenn Sie dem Seitennamen einen Schrägstrich „/“ voranstellen, wird die URL erstellt.</span><span class="sxs-lookup"><span data-stu-id="6548c-127">Prefixing the page name with a forward slash "/" creates the URL.</span></span> <span data-ttu-id="6548c-128">Die URL im folgenden Beispiel verweist auf die Seite „Lautsprecher“ im aktuellen Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="6548c-128">The URL in the sample below points to the "Speaker" page in the current directory.</span></span>

```cshtml
<a asp-page="/Speakers">All Speakers</a>
```

<span data-ttu-id="6548c-129">Das `asp-page`-Attribut im vorherigen Codebeispiel rendert die HTML-Ausgabe in der Ansicht, die dem folgenden Codeausschnitt ähnelt:</span><span class="sxs-lookup"><span data-stu-id="6548c-129">The `asp-page` attribute in the previous code sample renders HTML output in the view that's similar to the following snippet:</span></span>

```html
<a href="/items?page=%2FSpeakers">Speakers</a>
```

<span data-ttu-id="6548c-130">Das `asp-page`-Attribut und die `asp-route`-, `asp-controller`- und `asp-action`-Attribute schließen sich gegenseitig aus.</span><span class="sxs-lookup"><span data-stu-id="6548c-130">The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes.</span></span> <span data-ttu-id="6548c-131">Allerdings kann `asp-page` mit `asp-route-id` genutzt werden, um das Routing zu steuern, wie im folgenden Codebeispiel veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="6548c-131">However, `asp-page` can be used with `asp-route-id` to control routing, as the following code sample demonstrates:</span></span>

```cshtml
<a asp-page="/Speaker" asp-route-id="@speaker.Id">View Speaker</a>
```

<span data-ttu-id="6548c-132">`asp-route-id` erzeugt die folgende Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="6548c-132">The `asp-route-id` produces the following output:</span></span>

```html
https://localhost:44399/Speakers/Index/2?page=%2FSpeaker
```

> [!NOTE]
> <span data-ttu-id="6548c-133">Damit Sie das `asp-page`-Attribut in Razor-Seiten verwenden können, müssen die URLs relative Pfade sein, z.B. `"./Speaker"`.</span><span class="sxs-lookup"><span data-stu-id="6548c-133">To use the `asp-page` attribute in Razor Pages, the URLs must be a relative path, for example `"./Speaker"`.</span></span> <span data-ttu-id="6548c-134">Relative Pfade im `asp-page`-Attribut sind nicht in MVC-Ansichten verfügbar.</span><span class="sxs-lookup"><span data-stu-id="6548c-134">Relative paths in the `asp-page` attribute are not available in MVC views.</span></span> <span data-ttu-id="6548c-135">Verwenden Sie stattdessen die Syntax „/“ für MVC-Ansichten.</span><span class="sxs-lookup"><span data-stu-id="6548c-135">Use the "/" syntax for MVC views instead.</span></span>

### <a name="asp-route-value"></a><span data-ttu-id="6548c-136">asp-route-{value}</span><span class="sxs-lookup"><span data-stu-id="6548c-136">asp-route-{value}</span></span>

<span data-ttu-id="6548c-137">`asp-route-` ist ein Platzhalterroutenpräfix.</span><span class="sxs-lookup"><span data-stu-id="6548c-137">`asp-route-` is a wild card route prefix.</span></span> <span data-ttu-id="6548c-138">Alle Werte, die Sie nach dem nachfolgenden Gedankenstrich einfügen, werden als potenzielle Routenparameter interpretiert.</span><span class="sxs-lookup"><span data-stu-id="6548c-138">Any value you put after the trailing dash will be interpreted as a potential route parameter.</span></span> <span data-ttu-id="6548c-139">Wenn keine Standardroute gefunden wird, wird dieses Routenpräfix an das generierte Href-Attribut als Anforderungsparameter und -wert angefügt.</span><span class="sxs-lookup"><span data-stu-id="6548c-139">If a default route isn't found, this route prefix will be appended to the generated href as a request parameter and value.</span></span> <span data-ttu-id="6548c-140">Andernfalls wird es in der Routenvorlage ersetzt.</span><span class="sxs-lookup"><span data-stu-id="6548c-140">Otherwise it will be substituted in the route template.</span></span>

<span data-ttu-id="6548c-141">Nehmen wir an, Sie verfügen über eine Controllermethode, die wie folgt definiert ist:</span><span class="sxs-lookup"><span data-stu-id="6548c-141">Assuming you have a controller method defined as follows:</span></span>

```csharp
public IActionResult AnchorTagHelper(string id)
{
    var speaker = new SpeakerData()
    {
        SpeakerId = 12
    };
    return View(viewName, speaker);
}
```

<span data-ttu-id="6548c-142">Und Sie haben die Standardvorlage für die Route in *Startup.cs* wie folgt definiert:</span><span class="sxs-lookup"><span data-stu-id="6548c-142">And have the default route template defined in your *Startup.cs* as follows:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

<span data-ttu-id="6548c-143">Die **Cshtml**-Datei enthält das Anchor-Taghilfsprogramm, das für die Verwendung des Modellparameters **Lautsprecher** notwendig ist. Die Datei wird aus dem Controller an die Ansicht übergeben und sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="6548c-143">The **cshtml** file that contains the Anchor Tag Helper necessary to use the **speaker** model parameter passed in from the controller to the view is as follows:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="6548c-144">Die generierte HTML sieht dann wie folgt aus, da **Id** in der Standardroute gefunden wurde.</span><span class="sxs-lookup"><span data-stu-id="6548c-144">The generated HTML will then be as follows because **id** was found in the default route.</span></span>

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

<span data-ttu-id="6548c-145">Wenn das Routenpräfix nicht Teil der gefundenen Routingvorlage ist. Dies ist der Fall für die folgende **Cshtml**-Datei:</span><span class="sxs-lookup"><span data-stu-id="6548c-145">If the route prefix isn't part of the routing template found, which is the case with the following **cshtml** file:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="6548c-146">Die generierte HTML sieht dann wie folgt aus, da **speakerid** nicht in der zugewiesenen Route gefunden wurde:</span><span class="sxs-lookup"><span data-stu-id="6548c-146">The generated HTML will then be as follows because **speakerid** wasn't found in the route matched:</span></span>

```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

<span data-ttu-id="6548c-147">Wenn entweder `asp-controller` oder `asp-action` nicht angegeben werden, erfolgt der gleiche Standardverarbeitungsprozess wie beim `asp-route`-Attribut.</span><span class="sxs-lookup"><span data-stu-id="6548c-147">If either `asp-controller` or `asp-action` are not specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

### <a name="asp-route"></a><span data-ttu-id="6548c-148">asp-route</span><span class="sxs-lookup"><span data-stu-id="6548c-148">asp-route</span></span>

<span data-ttu-id="6548c-149">`asp-route` bietet eine Möglichkeit zur Erstellung einer URL, die direkt auf eine benannte Route verlinkt.</span><span class="sxs-lookup"><span data-stu-id="6548c-149">`asp-route` provides a way to create a URL that links directly to a named route.</span></span> <span data-ttu-id="6548c-150">Mit der Verwendung von Routingattributen kann eine Route wie in `SpeakerController` gezeigt benannt und in der `Evaluations`-Methode verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="6548c-150">Using routing attributes, a route can be named as shown in the `SpeakerController` and used in its `Evaluations` method.</span></span>

<span data-ttu-id="6548c-151">`Name = "speakerevals"` befiehlt dem Anchor-Taghilfsprogramm das Generieren einer Route direkt zu dieser Controllermethode. Dabei wird die `/Speaker/Evaluations`-URL verwendet.</span><span class="sxs-lookup"><span data-stu-id="6548c-151">`Name = "speakerevals"` tells the Anchor Tag Helper to generate a route directly to that controller method using the URL `/Speaker/Evaluations`.</span></span> <span data-ttu-id="6548c-152">Wenn `asp-controller` oder `asp-action` zusätzlich zu `asp-route` angegeben sind, entspricht die generierte Route möglicherweise nicht Ihren Erwartungen.</span><span class="sxs-lookup"><span data-stu-id="6548c-152">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="6548c-153">`asp-route` sollte nicht mit den Attributen `asp-controller` oder `asp-action` verwendet werden, um einen Routenkonflikt zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="6548c-153">`asp-route` shouldn't be used with either of the attributes `asp-controller` or `asp-action` to avoid a route conflict.</span></span>

### <a name="asp-all-route-data"></a><span data-ttu-id="6548c-154">asp-all-route-data</span><span class="sxs-lookup"><span data-stu-id="6548c-154">asp-all-route-data</span></span>

<span data-ttu-id="6548c-155">`asp-all-route-data` ermöglicht das Erstellen eines Wörterbuchs mit Schlüssel-Wert-Paaren, in denen der Schlüssel der Name des Parameters und der Wert der dem Schlüssel zugeordnete Wert ist.</span><span class="sxs-lookup"><span data-stu-id="6548c-155">`asp-all-route-data` allows creating a dictionary of key value pairs where the key is the parameter name and the value is the value associated with that key.</span></span>

<span data-ttu-id="6548c-156">Das Beispiel unten zeigt die Erstellung eines Inline-Wörterbuchs und die Datenübertragung an die Razor-Ansicht.</span><span class="sxs-lookup"><span data-stu-id="6548c-156">As the example below shows, an inline dictionary is created and the data is passed to the razor view.</span></span> <span data-ttu-id="6548c-157">Als Alternative können die Daten auch mit Ihrem Modell übertragen werden.</span><span class="sxs-lookup"><span data-stu-id="6548c-157">As an alternative, the data could also be passed in with your model.</span></span>

```cshtml
@{
    var dict =
        new Dictionary<string, string>
        {
            {"speakerId", "11"},
            {"currentYear", "true"}
        };
}
<a asp-route="speakerevalscurrent"
asp-all-route-data="dict">SpeakerEvals</a>
```

<span data-ttu-id="6548c-158">Der obenstehende Code generiert die folgende URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span><span class="sxs-lookup"><span data-stu-id="6548c-158">The code above generates the following URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span></span>

<span data-ttu-id="6548c-159">Wenn auf den Link geklickt wird, wird die Controllermethode `EvaluationsCurrent` aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="6548c-159">When the link is clicked, the controller method `EvaluationsCurrent` is called.</span></span> <span data-ttu-id="6548c-160">Sie wird aufgerufen, da dieser Controller über zwei Zeichenfolgenparameter verfügt, die dem entsprechen, was aus dem `asp-all-route-data`-Wörterbuch erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="6548c-160">It's called because that controller has two string parameters that match what has been created from the `asp-all-route-data` dictionary.</span></span>

<span data-ttu-id="6548c-161">Wenn sämtliche Schlüssel im Wörterbuch Routenparametern entsprechen, werden diese Werte in der Route nach Bedarf ersetzt, und die anderen, nicht übereinstimmenden Werte werden als Anforderungsparameter generiert.</span><span class="sxs-lookup"><span data-stu-id="6548c-161">If any keys in the dictionary match route parameters, those values will be substituted in the route as appropriate and the other non-matching values will be generated as request parameters.</span></span>

### <a name="asp-fragment"></a><span data-ttu-id="6548c-162">asp-fragment</span><span class="sxs-lookup"><span data-stu-id="6548c-162">asp-fragment</span></span>

<span data-ttu-id="6548c-163">`asp-fragment` definiert ein URL-Fragment, das an die URL angefügt werden soll.</span><span class="sxs-lookup"><span data-stu-id="6548c-163">`asp-fragment` defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="6548c-164">Das Anchor-Taghilfsprogramm fügt das Hashzeichen (#) hinzu.</span><span class="sxs-lookup"><span data-stu-id="6548c-164">The Anchor Tag Helper will add the hash character (#).</span></span> <span data-ttu-id="6548c-165">Wenn Sie einen Tag erstellen:</span><span class="sxs-lookup"><span data-stu-id="6548c-165">If you create a tag:</span></span>

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

<span data-ttu-id="6548c-166">Die generierte URL sieht wie folgt aus: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span><span class="sxs-lookup"><span data-stu-id="6548c-166">The generated URL will be: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span></span>

<span data-ttu-id="6548c-167">Hashtags sind beim Erstellen von clientseitigen Anwendungen nützlich.</span><span class="sxs-lookup"><span data-stu-id="6548c-167">Hash tags are useful when building client-side applications.</span></span> <span data-ttu-id="6548c-168">Sie können beispielsweise zum einfachen Markieren und Suchen in JavaScript verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="6548c-168">They can be used for easy marking and searching in JavaScript, for example.</span></span>

### <a name="asp-area"></a><span data-ttu-id="6548c-169">asp-area</span><span class="sxs-lookup"><span data-stu-id="6548c-169">asp-area</span></span>

<span data-ttu-id="6548c-170">`asp-area` legt den Bereichsnamen fest, den ASP.NET Core verwendet, um die entsprechende Route festzulegen.</span><span class="sxs-lookup"><span data-stu-id="6548c-170">`asp-area` sets the area name that ASP.NET Core uses to set the appropriate route.</span></span> <span data-ttu-id="6548c-171">Die folgenden Beispiele zeigen, wie das Bereichsattribut eine Neuzuordnung von Routen bewirken kann.</span><span class="sxs-lookup"><span data-stu-id="6548c-171">Below are examples of how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="6548c-172">Das Festlegen von `asp-area` auf Blogs stellt das Verzeichnis `Areas/Blogs` den Routen der zugeordneten Controller und Ansichten für diesen Anchor-Tag voran.</span><span class="sxs-lookup"><span data-stu-id="6548c-172">Setting `asp-area` to Blogs prefixes the directory `Areas/Blogs` to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="6548c-173">Projektname</span><span class="sxs-lookup"><span data-stu-id="6548c-173">Project name</span></span>
  * <span data-ttu-id="6548c-174">wwwroot</span><span class="sxs-lookup"><span data-stu-id="6548c-174">wwwroot</span></span>
  * <span data-ttu-id="6548c-175">Bereiche</span><span class="sxs-lookup"><span data-stu-id="6548c-175">Areas</span></span>
    * <span data-ttu-id="6548c-176">Blogs</span><span class="sxs-lookup"><span data-stu-id="6548c-176">Blogs</span></span>
      * <span data-ttu-id="6548c-177">Controller</span><span class="sxs-lookup"><span data-stu-id="6548c-177">Controllers</span></span>
        * <span data-ttu-id="6548c-178">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="6548c-178">HomeController.cs</span></span>
      * <span data-ttu-id="6548c-179">Ansichten</span><span class="sxs-lookup"><span data-stu-id="6548c-179">Views</span></span>
        * <span data-ttu-id="6548c-180">Startseite</span><span class="sxs-lookup"><span data-stu-id="6548c-180">Home</span></span>
          * <span data-ttu-id="6548c-181">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="6548c-181">Index.cshtml</span></span>
          * <span data-ttu-id="6548c-182">AboutBlog.cshtml</span><span class="sxs-lookup"><span data-stu-id="6548c-182">AboutBlog.cshtml</span></span>
  * <span data-ttu-id="6548c-183">Controller</span><span class="sxs-lookup"><span data-stu-id="6548c-183">Controllers</span></span>

<span data-ttu-id="6548c-184">Das Angeben eines gültigen Bereichstags, z.B. ```area="Blogs"```, beim Verweisen auf die ```AboutBlog.cshtml```-Datei unter Verwendung des Anchor-Taghilfsprogramms sieht wie folgt aus.</span><span class="sxs-lookup"><span data-stu-id="6548c-184">Specifying an area tag that's valid, such as ```area="Blogs"``` when referencing the ```AboutBlog.cshtml``` file will look like the following using the Anchor Tag Helper.</span></span>

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

<span data-ttu-id="6548c-185">Die generierte HTML enthält das Bereichssegment und sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="6548c-185">The generated HTML will include the areas segment and will be as follows:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> <span data-ttu-id="6548c-186">Die Routenvorlage muss einen Verweis auf den Bereich enthalten (falls vorhanden), damit MVC-Bereiche in einer Webanwendung funktionieren.</span><span class="sxs-lookup"><span data-stu-id="6548c-186">For MVC areas to work in a web application, the route template must include a reference to the area if it exists.</span></span> <span data-ttu-id="6548c-187">Diese Vorlage, die der zweite Parameter des `routes.MapRoute`-Methodenaufrufs ist, wird wie folgt angezeigt: `template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span><span class="sxs-lookup"><span data-stu-id="6548c-187">That template, which is the second parameter of the `routes.MapRoute` method call, will appear as: `template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span></span>

### <a name="asp-protocol"></a><span data-ttu-id="6548c-188">asp-protocol</span><span class="sxs-lookup"><span data-stu-id="6548c-188">asp-protocol</span></span>

<span data-ttu-id="6548c-189">Das `asp-protocol`-Attribut gibt ein Protokoll in Ihrer URL an (z.B. `https`).</span><span class="sxs-lookup"><span data-stu-id="6548c-189">The `asp-protocol` is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="6548c-190">Ein Beispiel für ein Anchor-Taghilfsprogramm, das das Protokoll enthält, sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="6548c-190">An example Anchor Tag Helper that includes the protocol will look as follows:</span></span>

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

<span data-ttu-id="6548c-191">und generiert die HTML wie folgt:</span><span class="sxs-lookup"><span data-stu-id="6548c-191">and will generate HTML as follows:</span></span>

```<a href="https://localhost/Home/About">About</a>```

<span data-ttu-id="6548c-192">Die Domäne im Beispiel ist „localhost“, aber das Anchor-Taghilfsprogramm verwendet die öffentliche Domäne der Website beim Generieren der URL.</span><span class="sxs-lookup"><span data-stu-id="6548c-192">The domain in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6548c-193">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="6548c-193">Additional resources</span></span>

* [<span data-ttu-id="6548c-194">Bereiche</span><span class="sxs-lookup"><span data-stu-id="6548c-194">Areas</span></span>](xref:mvc/controllers/areas)
