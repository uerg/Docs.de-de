---
title: Verankern Sie die Tag-Hilfsprogramm | Microsoft Docs
author: pkellner
description: Zeigt, wie mit Anker Tag Hilfsprogramm arbeiten
keywords: ASP.NET Core, Taghilfsprogramm
ms.author: riande
manager: wpickett
ms.date: 12/20/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a011
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 86756a1d09e6e55ca79aed6e5b718088b82b782c
ms.sourcegitcommit: 2b263e87217658caa42eedc4f9d2d21ef0ab5d59
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2018
---
# <a name="anchor-tag-helper"></a><span data-ttu-id="fa238-104">Anchor-Tag-Hilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="fa238-104">Anchor Tag Helper</span></span>

<span data-ttu-id="fa238-105">Von [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="fa238-105">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="fa238-106">Anchor-Tag-Hilfsprogramm verbessert das HTML-Anchortag (`<a ... ></a>`) Tag, indem Sie neue Attribute hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="fa238-106">The Anchor Tag Helper enhances the HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="fa238-107">Der generierte Link (auf der `href` Tag) wird mit neuen Attributen erstellt.</span><span class="sxs-lookup"><span data-stu-id="fa238-107">The link generated (on the `href` tag) is created using the new attributes.</span></span> <span data-ttu-id="fa238-108">Diese URL kann ein optionale Protokoll wie Https enthalten.</span><span class="sxs-lookup"><span data-stu-id="fa238-108">That URL can include an optional protocol such as https.</span></span>

<span data-ttu-id="fa238-109">Unten Lautsprecher Controller wird in Beispielen in diesem Dokument verwendet.</span><span class="sxs-lookup"><span data-stu-id="fa238-109">The speaker controller below is used in samples in this document.</span></span>

<span data-ttu-id="fa238-110">**SpeakerController.cs**</span><span class="sxs-lookup"><span data-stu-id="fa238-110">**SpeakerController.cs**</span></span> 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a><span data-ttu-id="fa238-111">Anker Tagattribute-Hilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="fa238-111">Anchor Tag Helper Attributes</span></span>

### <a name="asp-controller"></a><span data-ttu-id="fa238-112">ASP-controller</span><span class="sxs-lookup"><span data-stu-id="fa238-112">asp-controller</span></span>

<span data-ttu-id="fa238-113">`asp-controller`Dient zum Zuordnen der Controller zum Generieren der URL verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="fa238-113">`asp-controller` is used to associate which controller will be used to generate the URL.</span></span> <span data-ttu-id="fa238-114">Die angegebenen Controller müssen im aktuellen Projekt vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="fa238-114">The controllers specified must exist in the current project.</span></span> <span data-ttu-id="fa238-115">Der folgende Code Listet alle Lautsprecher:</span><span class="sxs-lookup"><span data-stu-id="fa238-115">The following code lists all speakers:</span></span> 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

<span data-ttu-id="fa238-116">Das generierte Markup werden verwendet:</span><span class="sxs-lookup"><span data-stu-id="fa238-116">The generated markup will be:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="fa238-117">Wenn die `asp-controller` angegeben ist und `asp-action` ist nicht der Standardeinstellung `asp-action` wird die Standardmethode für die Controller der aktuell ausgeführten angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="fa238-117">If the `asp-controller` is specified and `asp-action` is not, the default `asp-action` will be the default controller method of the currently executing view.</span></span> <span data-ttu-id="fa238-118">Ist im obigen Beispiel, wenn `asp-action` wird ausgelassen, und dieses Anchor-Tag-Hilfsprogramm wird generiert *HomeController*des `Index` Ansicht (**/Home**), werden die generierten Markup:</span><span class="sxs-lookup"><span data-stu-id="fa238-118">That is, in the above example, if `asp-action` is left out, and this Anchor Tag Helper is generated from *HomeController*'s `Index` view (**/Home**), the generated markup will be:</span></span>

```html
<a href="/Home">All Speakers</a>
```

### <a name="asp-action"></a><span data-ttu-id="fa238-119">ASP-Aktion</span><span class="sxs-lookup"><span data-stu-id="fa238-119">asp-action</span></span>

<span data-ttu-id="fa238-120">`asp-action`Der Name der Aktionsmethode im Controller, die eingeschlossen werden in der generierten `href`.</span><span class="sxs-lookup"><span data-stu-id="fa238-120">`asp-action` is the name of the action method in the controller that will be included in the generated `href`.</span></span> <span data-ttu-id="fa238-121">Z. B. der folgende Code generierten festlegen `href` um zur Detailseite sprechender Benutzer verweisen:</span><span class="sxs-lookup"><span data-stu-id="fa238-121">For example, the following code set the generated `href` to point to the speaker detail page:</span></span>

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

<span data-ttu-id="fa238-122">Das generierte Markup werden verwendet:</span><span class="sxs-lookup"><span data-stu-id="fa238-122">The generated markup will be:</span></span>

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

<span data-ttu-id="fa238-123">Wenn kein `asp-controller` -Attribut angegeben ist, wird der Standard-Controller Aufrufen der Sicht, die Ausführung der aktuellen Ansicht verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="fa238-123">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view will be used.</span></span>  
 
<span data-ttu-id="fa238-124">Wenn das Attribut `asp-action` ist `Index`, wird keine Aktion an die URL, auf den Standardwert führende angefügt ist `Index` aufgerufenen Methode.</span><span class="sxs-lookup"><span data-stu-id="fa238-124">If the attribute `asp-action` is `Index`, then no action is appended to the URL, leading to the default `Index` method being called.</span></span> <span data-ttu-id="fa238-125">Die Aktion angegebenen (oder standardmäßig), muss vorhanden sein, im Controller in referenziert `asp-controller`.</span><span class="sxs-lookup"><span data-stu-id="fa238-125">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

### <a name="asp-page"></a><span data-ttu-id="fa238-126">ASP-Seite</span><span class="sxs-lookup"><span data-stu-id="fa238-126">asp-page</span></span>

<span data-ttu-id="fa238-127">Verwenden der `asp-page` -Attribut in ein Ankertag, legen Sie die URL zu einer bestimmten Seite verweisen.</span><span class="sxs-lookup"><span data-stu-id="fa238-127">Use the `asp-page` attribute in an anchor tag to set its URL to point to a specific page.</span></span> <span data-ttu-id="fa238-128">Der Name der Seite mit einem Schrägstrich voranstellen "/" erstellt die URL.</span><span class="sxs-lookup"><span data-stu-id="fa238-128">Prefixing the page name with a forward slash "/" creates the URL.</span></span> <span data-ttu-id="fa238-129">Die URL im folgenden Beispiel verweist auf die Seite "Lautsprecher" im aktuellen Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="fa238-129">The URL in the sample below points to the "Speaker" page in the current directory.</span></span>

```cshtml
<a asp-page="/Speakers">All Speakers</a>
```

<span data-ttu-id="fa238-130">Die `asp-page` Attribut im vorherigen Codebeispiel rendert die HTML-Ausgabe in der Sicht, ähnlich wie der folgende Codeausschnitt ist:</span><span class="sxs-lookup"><span data-stu-id="fa238-130">The `asp-page` attribute in the previous code sample renders HTML output in the view that is similar to the following snippet:</span></span>

```html
<a href="/items?page=%2FSpeakers">Speakers</a>
```

<span data-ttu-id="fa238-131">Die `asp-page` Attribut sich gegenseitig ausschließende mit ist der `asp-route`, `asp-controller`, und `asp-action` Attribute.</span><span class="sxs-lookup"><span data-stu-id="fa238-131">The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes.</span></span> <span data-ttu-id="fa238-132">Allerdings `asp-page` genutzt werden `asp-route-id` routing, steuern, wie im folgenden Codebeispiel veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="fa238-132">However, `asp-page` can be used with `asp-route-id` to control routing, as the following code sample demonstrates:</span></span>

```cshtml
<a asp-page="/Speaker" asp-route-id="@speaker.Id">View Speaker</a>
```

<span data-ttu-id="fa238-133">Die `asp-route-id` erzeugt die folgende Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="fa238-133">The `asp-route-id` produces the following output:</span></span>

```html
https://localhost:44399/Speakers/Index/2?page=%2FSpeaker
```

> [!NOTE]
> <span data-ttu-id="fa238-134">Verwenden der `asp-page` Attribut in Razor-Seiten, die URLs muss ein relativer Pfad sein, z. B. `"./Speaker"`.</span><span class="sxs-lookup"><span data-stu-id="fa238-134">To use the `asp-page` attribute in Razor Pages, the URLs must be a relative path, for example `"./Speaker"`.</span></span> <span data-ttu-id="fa238-135">Relative Pfade in der `asp-page` Attribut sind nicht verfügbar in MVC-Ansichten.</span><span class="sxs-lookup"><span data-stu-id="fa238-135">Relative paths in the `asp-page` attribute are not available in MVC views.</span></span> <span data-ttu-id="fa238-136">Verwenden Sie stattdessen die Syntax "/" für MVC-Ansichten.</span><span class="sxs-lookup"><span data-stu-id="fa238-136">Use the "/" syntax for MVC views instead.</span></span>

### <a name="asp-route-value"></a><span data-ttu-id="fa238-137">ASP - Route-{Value}</span><span class="sxs-lookup"><span data-stu-id="fa238-137">asp-route-{value}</span></span>

<span data-ttu-id="fa238-138">`asp-route-`ist ein Platzhalter-Routenpräfix.</span><span class="sxs-lookup"><span data-stu-id="fa238-138">`asp-route-` is a wild card route prefix.</span></span> <span data-ttu-id="fa238-139">Alle Werte, die Sie einfügen, nachdem nachfolgende Bindestrich als einen potenziellen Routenparameter interpretiert wird.</span><span class="sxs-lookup"><span data-stu-id="fa238-139">Any value you put after the trailing dash will be interpreted as a potential route parameter.</span></span> <span data-ttu-id="fa238-140">Wenn eine Standardroute nicht gefunden wird, wird diese Routenpräfix zu der generierten Href als Anforderungsparameter und Wert angefügt.</span><span class="sxs-lookup"><span data-stu-id="fa238-140">If a default route is not found, this route prefix will be appended to the generated href as a request parameter and value.</span></span> <span data-ttu-id="fa238-141">Andernfalls wird es in der routenvorlage ersetzt.</span><span class="sxs-lookup"><span data-stu-id="fa238-141">Otherwise it will be substituted in the route template.</span></span>

<span data-ttu-id="fa238-142">Vorausgesetzt, Sie haben eine Controllermethode wie folgt definiert:</span><span class="sxs-lookup"><span data-stu-id="fa238-142">Assuming you have a controller method defined as follows:</span></span>

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

<span data-ttu-id="fa238-143">Und haben die Standardvorlage für die Route definiert, die Ihrem *Startup.cs* wie folgt:</span><span class="sxs-lookup"><span data-stu-id="fa238-143">And have the default route template defined in your *Startup.cs* as follows:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

<span data-ttu-id="fa238-144">Die **Cshtml** Datei mit der Anker-Tag-Hilfsmethode zum Verwenden der **Lautsprecher** Modellparameter auf dem Controller an die Ansicht übergeben lautet wie folgt:</span><span class="sxs-lookup"><span data-stu-id="fa238-144">The **cshtml** file that contains the Anchor Tag Helper necessary to use the **speaker** model parameter passed in from the controller to the view is as follows:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="fa238-145">Die generierte HTML-Code kann dann wie folgt werden, da **Id** in die Standardroute gefunden wurde.</span><span class="sxs-lookup"><span data-stu-id="fa238-145">The generated HTML will then be as follows because **id** was found in the default route.</span></span>

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

<span data-ttu-id="fa238-146">Wenn das Routenpräfix nicht Teil der routing-Vorlage gefunden wird, wird dies ist der Fall mit den folgenden **Cshtml** Datei:</span><span class="sxs-lookup"><span data-stu-id="fa238-146">If the route prefix is not part of the routing template found, which is the case with the following **cshtml** file:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="fa238-147">Die generierte HTML-Code kann dann wie folgt werden, da **Speakerid** wurde nicht gefunden, in der Route verglichen:</span><span class="sxs-lookup"><span data-stu-id="fa238-147">The generated HTML will then be as follows because **speakerid** was not found in the route matched:</span></span>

```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

<span data-ttu-id="fa238-148">Wenn entweder `asp-controller` oder `asp-action` nicht angegeben werden, und klicken Sie dann die gleichen standardverarbeitung eingehalten wird, wie in der `asp-route` Attribut.</span><span class="sxs-lookup"><span data-stu-id="fa238-148">If either `asp-controller` or `asp-action` are not specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

### <a name="asp-route"></a><span data-ttu-id="fa238-149">ASP-route</span><span class="sxs-lookup"><span data-stu-id="fa238-149">asp-route</span></span>

<span data-ttu-id="fa238-150">`asp-route`bietet eine Möglichkeit, eine URL direkt für eine benannte Route erstellen.</span><span class="sxs-lookup"><span data-stu-id="fa238-150">`asp-route` provides a way to create a URL that links directly to a named route.</span></span> <span data-ttu-id="fa238-151">Verwenden von routing Attributen, eine Route kann benannt werden entsprechend der `SpeakerController` und verwendet die `Evaluations` Methode.</span><span class="sxs-lookup"><span data-stu-id="fa238-151">Using routing attributes, a route can be named as shown in the `SpeakerController` and used in its `Evaluations` method.</span></span>

<span data-ttu-id="fa238-152">`Name = "speakerevals"`teilt dem Anker Tag-Hilfsprogramm zum Generieren einer Route direkt auf diesem Controllermethode, die mit der URL `/Speaker/Evaluations`.</span><span class="sxs-lookup"><span data-stu-id="fa238-152">`Name = "speakerevals"` tells the Anchor Tag Helper to generate a route directly to that controller method using the URL `/Speaker/Evaluations`.</span></span> <span data-ttu-id="fa238-153">Wenn `asp-controller` oder `asp-action` angegeben ist, zusätzlich zu `asp-route`, die Route generiert möglicherweise nicht Ihren Erwartungen.</span><span class="sxs-lookup"><span data-stu-id="fa238-153">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="fa238-154">`asp-route`sollte nicht verwendet werden, mithilfe eines der Attribute `asp-controller` oder `asp-action` zur Vermeidung eines Konflikts Route.</span><span class="sxs-lookup"><span data-stu-id="fa238-154">`asp-route` should not be used with either of the attributes `asp-controller` or `asp-action` to avoid a route conflict.</span></span>

### <a name="asp-all-route-data"></a><span data-ttu-id="fa238-155">ASP-All-Route-Daten</span><span class="sxs-lookup"><span data-stu-id="fa238-155">asp-all-route-data</span></span>

<span data-ttu-id="fa238-156">`asp-all-route-data`ermöglicht das Erstellen eines Wörterbuchs von Schlüssel-Wert-Paaren, in dem der Schlüssel ist der Name des Parameters und der Wert ist der Wert mit dem Schlüssel zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="fa238-156">`asp-all-route-data` allows creating a dictionary of key value pairs where the key is the parameter name and the value is the value associated with that key.</span></span>

<span data-ttu-id="fa238-157">Wie im Beispiel unten zeigt wird ein Inline-Wörterbuch erstellt, und die Daten in der Razor-Ansicht übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="fa238-157">As the example below shows, an inline dictionary is created and the data is passed to the razor view.</span></span> <span data-ttu-id="fa238-158">Als Alternative können die Daten auch mit Ihrem Modell übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="fa238-158">As an alternative, the data could also be passed in with your model.</span></span>

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

<span data-ttu-id="fa238-159">Der obige Code generiert die folgende URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span><span class="sxs-lookup"><span data-stu-id="fa238-159">The code above generates the following URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span></span>

<span data-ttu-id="fa238-160">Wenn der Link geklickt wird, wird die Controllermethode `EvaluationsCurrent` aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="fa238-160">When the link is clicked, the controller method `EvaluationsCurrent` is called.</span></span> <span data-ttu-id="fa238-161">Wird aufgerufen, da dieser Controller zwei Zeichenfolgenparameter verfügt, die mit übereinstimmen, was erstellt wurde die `asp-all-route-data` Wörterbuch.</span><span class="sxs-lookup"><span data-stu-id="fa238-161">It is called because that controller has two string parameters that match what has been created from the `asp-all-route-data` dictionary.</span></span>

<span data-ttu-id="fa238-162">Wenn sämtliche Schlüssel im Wörterbuch übereinstimmen Parameter weiterleiten, diese Werte in der Route nach Bedarf ersetzt, und die andere nicht übereinstimmende Werte als Anforderungsparameter generiert werden.</span><span class="sxs-lookup"><span data-stu-id="fa238-162">If any keys in the dictionary match route parameters, those values will be substituted in the route as appropriate and the other non-matching values will be generated as request parameters.</span></span>

### <a name="asp-fragment"></a><span data-ttu-id="fa238-163">ASP-fragment</span><span class="sxs-lookup"><span data-stu-id="fa238-163">asp-fragment</span></span>

<span data-ttu-id="fa238-164">`asp-fragment`definiert ein URL-Fragment, an die URL angefügt werden soll.</span><span class="sxs-lookup"><span data-stu-id="fa238-164">`asp-fragment` defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="fa238-165">Anchor-Tag-Hilfsprogramm fügen das Hashzeichen (#).</span><span class="sxs-lookup"><span data-stu-id="fa238-165">The Anchor Tag Helper will add the hash character (#).</span></span> <span data-ttu-id="fa238-166">Wenn Sie ein Tag zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="fa238-166">If you create a tag:</span></span>

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

<span data-ttu-id="fa238-167">Die generierte URL: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span><span class="sxs-lookup"><span data-stu-id="fa238-167">The generated URL will be: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span></span>

<span data-ttu-id="fa238-168">Hash-Tags sind nützlich, wenn die clientseitige Anwendungen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="fa238-168">Hash tags are useful when building client-side applications.</span></span> <span data-ttu-id="fa238-169">Sie können zum einfach markieren und suchen beispielsweise in JavaScript verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="fa238-169">They can be used for easy marking and searching in JavaScript, for example.</span></span>

### <a name="asp-area"></a><span data-ttu-id="fa238-170">ASP-Bereich</span><span class="sxs-lookup"><span data-stu-id="fa238-170">asp-area</span></span>

<span data-ttu-id="fa238-171">`asp-area`Legt den Bereichsnamen, den ASP.NET Core verwendet wird, um die entsprechenden Route festzulegen.</span><span class="sxs-lookup"><span data-stu-id="fa238-171">`asp-area` sets the area name that ASP.NET Core uses to set the appropriate route.</span></span> <span data-ttu-id="fa238-172">Im folgenden sind Beispiele für wie das Area-Attribut bewirkt, dass eine neuzuordnung von Routen.</span><span class="sxs-lookup"><span data-stu-id="fa238-172">Below are examples of how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="fa238-173">Festlegen von `asp-area` Blogs Präfixe das Verzeichnis `Areas/Blogs` auf die Routen den zugeordneten Controller und Ansichten für diese Anchortag.</span><span class="sxs-lookup"><span data-stu-id="fa238-173">Setting `asp-area` to Blogs prefixes the directory `Areas/Blogs` to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="fa238-174">Projektname</span><span class="sxs-lookup"><span data-stu-id="fa238-174">Project name</span></span>
  * <span data-ttu-id="fa238-175">wwwroot</span><span class="sxs-lookup"><span data-stu-id="fa238-175">wwwroot</span></span>
  * <span data-ttu-id="fa238-176">Bereiche</span><span class="sxs-lookup"><span data-stu-id="fa238-176">Areas</span></span>
    * <span data-ttu-id="fa238-177">Blogs</span><span class="sxs-lookup"><span data-stu-id="fa238-177">Blogs</span></span>
      * <span data-ttu-id="fa238-178">Domänencontroller</span><span class="sxs-lookup"><span data-stu-id="fa238-178">Controllers</span></span>
        * <span data-ttu-id="fa238-179">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="fa238-179">HomeController.cs</span></span>
      * <span data-ttu-id="fa238-180">Ansichten</span><span class="sxs-lookup"><span data-stu-id="fa238-180">Views</span></span>
        * <span data-ttu-id="fa238-181">Startseite</span><span class="sxs-lookup"><span data-stu-id="fa238-181">Home</span></span>
          * <span data-ttu-id="fa238-182">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="fa238-182">Index.cshtml</span></span>
          * <span data-ttu-id="fa238-183">AboutBlog.cshtml</span><span class="sxs-lookup"><span data-stu-id="fa238-183">AboutBlog.cshtml</span></span>
  * <span data-ttu-id="fa238-184">Domänencontroller</span><span class="sxs-lookup"><span data-stu-id="fa238-184">Controllers</span></span>

<span data-ttu-id="fa238-185">Angeben einer Bereich Tag gültig ist, z. B. ```area="Blogs"``` beim Verweisen auf die ```AboutBlog.cshtml``` Datei sieht wie folgt unter Verwendung des Premium-Tag-Hilfsprogramms.</span><span class="sxs-lookup"><span data-stu-id="fa238-185">Specifying an area tag that is valid, such as ```area="Blogs"``` when referencing the ```AboutBlog.cshtml``` file will look like the following using the Anchor Tag Helper.</span></span>

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

<span data-ttu-id="fa238-186">Die generierte HTML-Code wird das Segment Bereiche enthalten und werden wie folgt:</span><span class="sxs-lookup"><span data-stu-id="fa238-186">The generated HTML will include the areas segment and will be as follows:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> <span data-ttu-id="fa238-187">Für MVC-Bereiche in einer Webanwendung arbeiten muss die routenvorlage einen Verweis auf den Bereich enthalten, falls vorhanden.</span><span class="sxs-lookup"><span data-stu-id="fa238-187">For MVC areas to work in a web application, the route template must include a reference to the area if it exists.</span></span> <span data-ttu-id="fa238-188">Diese Vorlage, die der zweite Parameter ist von der `routes.MapRoute` -Methodenaufruf, als angezeigt:`template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span><span class="sxs-lookup"><span data-stu-id="fa238-188">That template, which is the second parameter of the `routes.MapRoute` method call, will appear as: `template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span></span>

### <a name="asp-protocol"></a><span data-ttu-id="fa238-189">ASP-Protokoll</span><span class="sxs-lookup"><span data-stu-id="fa238-189">asp-protocol</span></span>

<span data-ttu-id="fa238-190">Die `asp-protocol` wird ein Protokoll angegeben (z. B. `https`) in der URL.</span><span class="sxs-lookup"><span data-stu-id="fa238-190">The `asp-protocol` is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="fa238-191">Ein Beispiel für Anchor-Tag-Hilfsprogramm, das das Protokoll enthält wird wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="fa238-191">An example Anchor Tag Helper that includes the protocol will look as follows:</span></span>

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

<span data-ttu-id="fa238-192">und generieren HTML wie folgt:</span><span class="sxs-lookup"><span data-stu-id="fa238-192">and will generate HTML as follows:</span></span>

```<a href="https://localhost/Home/About">About</a>```

<span data-ttu-id="fa238-193">Die Domäne im Beispiel ist "localhost", aber die Anker-Tag-Hilfsprogramm verwendet die Website für die öffentliche Domäne beim Generieren der URL.</span><span class="sxs-lookup"><span data-stu-id="fa238-193">The domain in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fa238-194">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="fa238-194">Additional resources</span></span>

* [<span data-ttu-id="fa238-195">Bereiche</span><span class="sxs-lookup"><span data-stu-id="fa238-195">Areas</span></span>](xref:mvc/controllers/areas)
