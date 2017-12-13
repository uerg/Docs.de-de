---
title: Verankern Sie die Tag-Hilfsprogramm | Microsoft Docs
author: pkellner
description: Zeigt, wie mit Anker Tag Hilfsprogramm arbeiten
keywords: ASP.NET Core, Taghilfsprogramm
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a011
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: e3754c4313f01bc746ccb8efe11611ae213e3955
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="anchor-tag-helper"></a><span data-ttu-id="0e71b-104">Anchor-Tag-Hilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="0e71b-104">Anchor Tag Helper</span></span>

<span data-ttu-id="0e71b-105">Von [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="0e71b-105">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="0e71b-106">Anchor-Tag-Hilfsprogramm verbessert das HTML-Anchortag (`<a ... ></a>`) Tag, indem Sie neue Attribute hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="0e71b-106">The Anchor Tag Helper enhances the HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="0e71b-107">Der generierte Link (auf der `href` Tag) wird mit neuen Attributen erstellt.</span><span class="sxs-lookup"><span data-stu-id="0e71b-107">The link generated (on the `href` tag) is created using the new attributes.</span></span> <span data-ttu-id="0e71b-108">Diese URL kann ein optionale Protokoll wie Https enthalten.</span><span class="sxs-lookup"><span data-stu-id="0e71b-108">That URL can include an optional protocol such as https.</span></span>

<span data-ttu-id="0e71b-109">Unten Lautsprecher Controller wird in Beispielen in diesem Dokument verwendet.</span><span class="sxs-lookup"><span data-stu-id="0e71b-109">The speaker controller below is used in samples in this document.</span></span>

<br/><span data-ttu-id="0e71b-110">
**SpeakerController.cs**</span><span class="sxs-lookup"><span data-stu-id="0e71b-110">
**SpeakerController.cs**</span></span> 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a><span data-ttu-id="0e71b-111">Anker Tagattribute-Hilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="0e71b-111">Anchor Tag Helper Attributes</span></span>

- - -

### <a name="asp-controller"></a><span data-ttu-id="0e71b-112">ASP-controller</span><span class="sxs-lookup"><span data-stu-id="0e71b-112">asp-controller</span></span>

<span data-ttu-id="0e71b-113">`asp-controller`Dient zum Zuordnen der Controller zum Generieren der URL verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="0e71b-113">`asp-controller` is used to associate which controller will be used to generate the URL.</span></span> <span data-ttu-id="0e71b-114">Die angegebenen Controller müssen im aktuellen Projekt vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="0e71b-114">The controllers specified must exist in the current project.</span></span> <span data-ttu-id="0e71b-115">Der folgende Code Listet alle Lautsprecher:</span><span class="sxs-lookup"><span data-stu-id="0e71b-115">The following code lists all speakers:</span></span> 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

<span data-ttu-id="0e71b-116">Das generierte Markup werden verwendet:</span><span class="sxs-lookup"><span data-stu-id="0e71b-116">The generated markup will be:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="0e71b-117">Wenn die `asp-controller` angegeben ist und `asp-action` ist nicht der Standardeinstellung `asp-action` wird die Standardmethode für die Controller der aktuell ausgeführten angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="0e71b-117">If the `asp-controller` is specified and `asp-action` is not, the default `asp-action` will be the default controller method of the currently executing view.</span></span> <span data-ttu-id="0e71b-118">Ist im obigen Beispiel, wenn `asp-action` wird ausgelassen, und dieses Anchor-Tag-Hilfsprogramm wird generiert *HomeController*des `Index` Ansicht (**/Home**), werden die generierten Markup:</span><span class="sxs-lookup"><span data-stu-id="0e71b-118">That is, in the above example, if `asp-action` is left out, and this Anchor Tag Helper is generated from *HomeController*'s `Index` view (**/Home**), the generated markup will be:</span></span>


```html
<a href="/Home">All Speakers</a>
```

- - -
  
### <a name="asp-action"></a><span data-ttu-id="0e71b-119">ASP-Aktion</span><span class="sxs-lookup"><span data-stu-id="0e71b-119">asp-action</span></span>

<span data-ttu-id="0e71b-120">`asp-action`Der Name der Aktionsmethode im Controller, die eingeschlossen werden in der generierten `href`.</span><span class="sxs-lookup"><span data-stu-id="0e71b-120">`asp-action` is the name of the action method in the controller that will be included in the generated `href`.</span></span> <span data-ttu-id="0e71b-121">Z. B. der folgende Code generierten festlegen `href` um zur Detailseite sprechender Benutzer verweisen:</span><span class="sxs-lookup"><span data-stu-id="0e71b-121">For example, the following code set the generated `href` to point to the speaker detail page:</span></span>

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

<span data-ttu-id="0e71b-122">Das generierte Markup werden verwendet:</span><span class="sxs-lookup"><span data-stu-id="0e71b-122">The generated markup will be:</span></span>

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

<span data-ttu-id="0e71b-123">Wenn kein `asp-controller` -Attribut angegeben ist, wird der Standard-Controller Aufrufen der Sicht, die Ausführung der aktuellen Ansicht verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="0e71b-123">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view will be used.</span></span>  
 
<span data-ttu-id="0e71b-124">Wenn das Attribut `asp-action` ist `Index`, wird keine Aktion an die URL, auf den Standardwert führende angefügt ist `Index` aufgerufenen Methode.</span><span class="sxs-lookup"><span data-stu-id="0e71b-124">If the attribute `asp-action` is `Index`, then no action is appended to the URL, leading to the default `Index` method being called.</span></span> <span data-ttu-id="0e71b-125">Die Aktion angegebenen (oder standardmäßig), muss vorhanden sein, im Controller in referenziert `asp-controller`.</span><span class="sxs-lookup"><span data-stu-id="0e71b-125">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

- - -
  
<a name="route"></a>
### <a name="asp-route-value"></a><span data-ttu-id="0e71b-126">ASP - Route-{Value}</span><span class="sxs-lookup"><span data-stu-id="0e71b-126">asp-route-{value}</span></span>

<span data-ttu-id="0e71b-127">`asp-route-`ist ein Platzhalter-Routenpräfix.</span><span class="sxs-lookup"><span data-stu-id="0e71b-127">`asp-route-` is a wild card route prefix.</span></span> <span data-ttu-id="0e71b-128">Alle Werte, die Sie einfügen, nachdem nachfolgende Bindestrich als einen potenziellen Routenparameter interpretiert wird.</span><span class="sxs-lookup"><span data-stu-id="0e71b-128">Any value you put after the trailing dash will be interpreted as a potential route parameter.</span></span> <span data-ttu-id="0e71b-129">Wenn eine Standardroute nicht gefunden wird, wird diese Routenpräfix zu der generierten Href als Anforderungsparameter und Wert angefügt.</span><span class="sxs-lookup"><span data-stu-id="0e71b-129">If a default route is not found, this route prefix will be appended to the generated href as a request parameter and value.</span></span> <span data-ttu-id="0e71b-130">Andernfalls wird es in der routenvorlage ersetzt.</span><span class="sxs-lookup"><span data-stu-id="0e71b-130">Otherwise it will be substituted in the route template.</span></span>

<span data-ttu-id="0e71b-131">Vorausgesetzt, Sie haben eine Controllermethode wie folgt definiert:</span><span class="sxs-lookup"><span data-stu-id="0e71b-131">Assuming you have a controller method defined as follows:</span></span>

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

<span data-ttu-id="0e71b-132">Und haben die Standardvorlage für die Route definiert, die Ihrem *Startup.cs* wie folgt:</span><span class="sxs-lookup"><span data-stu-id="0e71b-132">And have the default route template defined in your *Startup.cs* as follows:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

<span data-ttu-id="0e71b-133">Die **Cshtml** Datei mit der Anker-Tag-Hilfsmethode zum Verwenden der **Lautsprecher** Modellparameter auf dem Controller an die Ansicht übergeben lautet wie folgt:</span><span class="sxs-lookup"><span data-stu-id="0e71b-133">The **cshtml** file that contains the Anchor Tag Helper necessary to use the **speaker** model parameter passed in from the controller to the view is as follows:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="0e71b-134">Die generierte HTML-Code kann dann wie folgt werden, da **Id** in die Standardroute gefunden wurde.</span><span class="sxs-lookup"><span data-stu-id="0e71b-134">The generated HTML will then be as follows because **id** was found in the default route.</span></span>

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

<span data-ttu-id="0e71b-135">Wenn das Routenpräfix nicht Teil der routing-Vorlage gefunden wird, wird dies ist der Fall mit den folgenden **Cshtml** Datei:</span><span class="sxs-lookup"><span data-stu-id="0e71b-135">If the route prefix is not part of the routing template found, which is the case with the following **cshtml** file:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="0e71b-136">Die generierte HTML-Code kann dann wie folgt werden, da **Speakerid** wurde nicht gefunden, in der Route verglichen:</span><span class="sxs-lookup"><span data-stu-id="0e71b-136">The generated HTML will then be as follows because **speakerid** was not found in the route matched:</span></span>


```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

<span data-ttu-id="0e71b-137">Wenn entweder `asp-controller` oder `asp-action` nicht angegeben werden, und klicken Sie dann die gleichen standardverarbeitung eingehalten wird, wie in der `asp-route` Attribut.</span><span class="sxs-lookup"><span data-stu-id="0e71b-137">If either `asp-controller` or `asp-action` are not specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

- - -

### <a name="asp-route"></a><span data-ttu-id="0e71b-138">ASP-route</span><span class="sxs-lookup"><span data-stu-id="0e71b-138">asp-route</span></span>

<span data-ttu-id="0e71b-139">`asp-route`bietet eine Möglichkeit, eine URL direkt für eine benannte Route erstellen.</span><span class="sxs-lookup"><span data-stu-id="0e71b-139">`asp-route` provides a way to create a URL that links directly to a named route.</span></span> <span data-ttu-id="0e71b-140">Verwenden von routing Attributen, eine Route kann benannt werden entsprechend der `SpeakerController` und verwendet die `Evaluations` Methode.</span><span class="sxs-lookup"><span data-stu-id="0e71b-140">Using routing attributes, a route can be named as shown in the `SpeakerController` and used in its `Evaluations` method.</span></span>

<span data-ttu-id="0e71b-141">`Name = "speakerevals"`teilt dem Anker Tag-Hilfsprogramm zum Generieren einer Route direkt auf diesem Controllermethode, die mit der URL `/Speaker/Evaluations`.</span><span class="sxs-lookup"><span data-stu-id="0e71b-141">`Name = "speakerevals"` tells the Anchor Tag Helper to generate a route directly to that controller method using the URL `/Speaker/Evaluations`.</span></span> <span data-ttu-id="0e71b-142">Wenn `asp-controller` oder `asp-action` angegeben ist, zusätzlich zu `asp-route`, die Route generiert möglicherweise nicht Ihren Erwartungen.</span><span class="sxs-lookup"><span data-stu-id="0e71b-142">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="0e71b-143">`asp-route`sollte nicht verwendet werden, mithilfe eines der Attribute `asp-controller` oder `asp-action` zur Vermeidung eines Konflikts Route.</span><span class="sxs-lookup"><span data-stu-id="0e71b-143">`asp-route` should not be used with either of the attributes `asp-controller` or `asp-action` to avoid a route conflict.</span></span>

- - -

### <a name="asp-all-route-data"></a><span data-ttu-id="0e71b-144">ASP-All-Route-Daten</span><span class="sxs-lookup"><span data-stu-id="0e71b-144">asp-all-route-data</span></span>

<span data-ttu-id="0e71b-145">`asp-all-route-data`ermöglicht das Erstellen eines Wörterbuchs von Schlüssel-Wert-Paaren, in dem der Schlüssel ist der Name des Parameters und der Wert ist der Wert mit dem Schlüssel zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="0e71b-145">`asp-all-route-data` allows creating a dictionary of key value pairs where the key is the parameter name and the value is the value associated with that key.</span></span>

<span data-ttu-id="0e71b-146">Wie im Beispiel unten zeigt wird ein Inline-Wörterbuch erstellt, und die Daten in der Razor-Ansicht übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="0e71b-146">As the example below shows, an inline dictionary is created and the data is passed to the razor view.</span></span> <span data-ttu-id="0e71b-147">Als Alternative können die Daten auch mit Ihrem Modell übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="0e71b-147">As an alternative, the data could also be passed in with your model.</span></span>

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

<span data-ttu-id="0e71b-148">Der obige Code generiert die folgende URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span><span class="sxs-lookup"><span data-stu-id="0e71b-148">The code above generates the following URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span></span>

<span data-ttu-id="0e71b-149">Wenn der Link geklickt wird, wird die Controllermethode `EvaluationsCurrent` aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="0e71b-149">When the link is clicked, the controller method `EvaluationsCurrent` is called.</span></span> <span data-ttu-id="0e71b-150">Wird aufgerufen, da dieser Controller zwei Zeichenfolgenparameter verfügt, die mit übereinstimmen, was erstellt wurde die `asp-all-route-data` Wörterbuch.</span><span class="sxs-lookup"><span data-stu-id="0e71b-150">It is called because that controller has two string parameters that match what has been created from the `asp-all-route-data` dictionary.</span></span>

<span data-ttu-id="0e71b-151">Wenn sämtliche Schlüssel im Wörterbuch übereinstimmen Parameter weiterleiten, diese Werte in der Route nach Bedarf ersetzt, und die andere nicht übereinstimmende Werte als Anforderungsparameter generiert werden.</span><span class="sxs-lookup"><span data-stu-id="0e71b-151">If any keys in the dictionary match route parameters, those values will be substituted in the route as appropriate and the other non-matching values will be generated as request parameters.</span></span>

- - -

### <a name="asp-fragment"></a><span data-ttu-id="0e71b-152">ASP-fragment</span><span class="sxs-lookup"><span data-stu-id="0e71b-152">asp-fragment</span></span>

<span data-ttu-id="0e71b-153">`asp-fragment`definiert ein URL-Fragment, an die URL angefügt werden soll.</span><span class="sxs-lookup"><span data-stu-id="0e71b-153">`asp-fragment` defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="0e71b-154">Anchor-Tag-Hilfsprogramm fügen das Hashzeichen (#).</span><span class="sxs-lookup"><span data-stu-id="0e71b-154">The Anchor Tag Helper will add the hash character (#).</span></span> <span data-ttu-id="0e71b-155">Wenn Sie ein Tag zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="0e71b-155">If you create a tag:</span></span>

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

<span data-ttu-id="0e71b-156">Die generierte URL: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span><span class="sxs-lookup"><span data-stu-id="0e71b-156">The generated URL will be: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span></span>

<span data-ttu-id="0e71b-157">Hash-Tags sind nützlich, wenn die clientseitige Anwendungen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="0e71b-157">Hash tags are useful when building client-side applications.</span></span> <span data-ttu-id="0e71b-158">Sie können zum einfach markieren und suchen beispielsweise in JavaScript verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="0e71b-158">They can be used for easy marking and searching in JavaScript, for example.</span></span>

- - -

### <a name="asp-area"></a><span data-ttu-id="0e71b-159">ASP-Bereich</span><span class="sxs-lookup"><span data-stu-id="0e71b-159">asp-area</span></span>

<span data-ttu-id="0e71b-160">`asp-area`Legt den Bereichsnamen, den ASP.NET Core verwendet wird, um die entsprechenden Route festzulegen.</span><span class="sxs-lookup"><span data-stu-id="0e71b-160">`asp-area` sets the area name that ASP.NET Core uses to set the appropriate route.</span></span> <span data-ttu-id="0e71b-161">Im folgenden sind Beispiele für wie das Area-Attribut bewirkt, dass eine neuzuordnung von Routen.</span><span class="sxs-lookup"><span data-stu-id="0e71b-161">Below are examples of how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="0e71b-162">Festlegen von `asp-area` Blogs Präfixe das Verzeichnis `Areas/Blogs` auf die Routen den zugeordneten Controller und Ansichten für diese Anchortag.</span><span class="sxs-lookup"><span data-stu-id="0e71b-162">Setting `asp-area` to Blogs prefixes the directory `Areas/Blogs` to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="0e71b-163">Projektname</span><span class="sxs-lookup"><span data-stu-id="0e71b-163">Project name</span></span>

  * <span data-ttu-id="0e71b-164">*"Wwwroot"*</span><span class="sxs-lookup"><span data-stu-id="0e71b-164">*wwwroot*</span></span>

  * <span data-ttu-id="0e71b-165">*Bereiche*</span><span class="sxs-lookup"><span data-stu-id="0e71b-165">*Areas*</span></span>

    * <span data-ttu-id="0e71b-166">*Blogs*</span><span class="sxs-lookup"><span data-stu-id="0e71b-166">*Blogs*</span></span>

      * <span data-ttu-id="0e71b-167">*Controller*</span><span class="sxs-lookup"><span data-stu-id="0e71b-167">*Controllers*</span></span>

        * <span data-ttu-id="0e71b-168">*HomeController.cs*</span><span class="sxs-lookup"><span data-stu-id="0e71b-168">*HomeController.cs*</span></span>

      * <span data-ttu-id="0e71b-169">*Ansichten*</span><span class="sxs-lookup"><span data-stu-id="0e71b-169">*Views*</span></span>

        * <span data-ttu-id="0e71b-170">*Startseite*</span><span class="sxs-lookup"><span data-stu-id="0e71b-170">*Home*</span></span>

          * <span data-ttu-id="0e71b-171">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="0e71b-171">*Index.cshtml*</span></span>
          
          * <span data-ttu-id="0e71b-172">*AboutBlog.cshtml*</span><span class="sxs-lookup"><span data-stu-id="0e71b-172">*AboutBlog.cshtml*</span></span>
          
  * <span data-ttu-id="0e71b-173">*Controller*</span><span class="sxs-lookup"><span data-stu-id="0e71b-173">*Controllers*</span></span>
  

        
<span data-ttu-id="0e71b-174">Angeben einer Bereich Tag gültig ist, z. B. ```area="Blogs"``` beim Verweisen auf die ```AboutBlog.cshtml``` Datei sieht wie folgt unter Verwendung des Premium-Tag-Hilfsprogramms.</span><span class="sxs-lookup"><span data-stu-id="0e71b-174">Specifying an area tag that is valid, such as ```area="Blogs"``` when referencing the ```AboutBlog.cshtml``` file will look like the following using the Anchor Tag Helper.</span></span>

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

<span data-ttu-id="0e71b-175">Die generierte HTML-Code wird das Segment Bereiche enthalten und werden wie folgt:</span><span class="sxs-lookup"><span data-stu-id="0e71b-175">The generated HTML will include the areas segment and will be as follows:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> <span data-ttu-id="0e71b-176">Für MVC-Bereiche in einer Webanwendung arbeiten muss die routenvorlage einen Verweis auf den Bereich enthalten, falls vorhanden.</span><span class="sxs-lookup"><span data-stu-id="0e71b-176">For MVC areas to work in a web application, the route template must include a reference to the area if it exists.</span></span> <span data-ttu-id="0e71b-177">Diese Vorlage, die der zweite Parameter ist von der `routes.MapRoute` -Methodenaufruf, als angezeigt:`template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span><span class="sxs-lookup"><span data-stu-id="0e71b-177">That template, which is the second parameter of the `routes.MapRoute` method call, will appear as: `template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span></span>

- - -

### <a name="asp-protocol"></a><span data-ttu-id="0e71b-178">ASP-Protokoll</span><span class="sxs-lookup"><span data-stu-id="0e71b-178">asp-protocol</span></span>

<span data-ttu-id="0e71b-179">Die `asp-protocol` wird ein Protokoll angegeben (z. B. `https`) in der URL.</span><span class="sxs-lookup"><span data-stu-id="0e71b-179">The `asp-protocol` is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="0e71b-180">Ein Beispiel für Anchor-Tag-Hilfsprogramm, das das Protokoll enthält wird wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="0e71b-180">An example Anchor Tag Helper that includes the protocol will look as follows:</span></span>

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

<span data-ttu-id="0e71b-181">und generieren HTML wie folgt:</span><span class="sxs-lookup"><span data-stu-id="0e71b-181">and will generate HTML as follows:</span></span>

```<a href="https://localhost/Home/About">About</a>```

<span data-ttu-id="0e71b-182">Die Domäne im Beispiel ist "localhost", aber die Anker-Tag-Hilfsprogramm verwendet die Website für die öffentliche Domäne beim Generieren der URL.</span><span class="sxs-lookup"><span data-stu-id="0e71b-182">The domain in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

- - -

## <a name="additional-resources"></a><span data-ttu-id="0e71b-183">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="0e71b-183">Additional resources</span></span>

* [<span data-ttu-id="0e71b-184">Bereiche</span><span class="sxs-lookup"><span data-stu-id="0e71b-184">Areas</span></span>](xref:mvc/controllers/areas)
