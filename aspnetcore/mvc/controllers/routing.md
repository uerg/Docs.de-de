---
title: Routing zum Controlleraktionen
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 03/14/2017
ms.topic: article
ms.assetid: 26250a4d-bf62-4d45-8549-26801cf956e9
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/routing
ms.openlocfilehash: da67124ffc874c4f83fff077c6429e9f3e571587
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
# <a name="routing-to-controller-actions"></a><span data-ttu-id="8f5d8-103">Routing zum Controlleraktionen</span><span class="sxs-lookup"><span data-stu-id="8f5d8-103">Routing to Controller Actions</span></span>

<span data-ttu-id="8f5d8-104">Durch [Ryan Nowak](https://github.com/rynowak) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8f5d8-104">By [Ryan Nowak](https://github.com/rynowak) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8f5d8-105">ASP.NET Core MVC verwendet Routing [Middleware](../../fundamentals/middleware.md) die URLs der eingehende Anforderungen entsprechen und Aktionen zuordnen.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-105">ASP.NET Core MVC uses the Routing [middleware](../../fundamentals/middleware.md) to match the URLs of incoming requests and map them to actions.</span></span> <span data-ttu-id="8f5d8-106">Routen werden in der Startcode oder Attribute definiert.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-106">Routes are defined in startup code or attributes.</span></span> <span data-ttu-id="8f5d8-107">Routen wird beschrieben, wie URL-Pfade auf Aktionen abgeglichen werden sollen.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-107">Routes describe how URL paths should be matched to actions.</span></span> <span data-ttu-id="8f5d8-108">Routen werden auch verwendet, zum Generieren von URLs (für Links), die in Antworten gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-108">Routes are also used to generate URLs (for links) sent out in responses.</span></span> 

<span data-ttu-id="8f5d8-109">Aktionen werden konventionell entweder weitergeleitet oder Attribut weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-109">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="8f5d8-110">Platzieren eine Route auf dem Controller oder die Aktion erleichtert Attribut weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-110">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="8f5d8-111">Finden Sie unter [gemischten routing](#routing-mixed-ref-label) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-111">See [Mixed routing](#routing-mixed-ref-label) for more information.</span></span>

<span data-ttu-id="8f5d8-112">Dieses Dokument wird erläutert, die Interaktionen zwischen MVC und routing und wie normale MVC-apps stellen routing-Funktionen.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-112">This document will explain the interactions between MVC and routing, and how typical MVC apps make use of routing features.</span></span> <span data-ttu-id="8f5d8-113">Finden Sie unter [Routing](xref:fundamentals/routing) Informationen zu erweiterten routing.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-113">See [Routing](xref:fundamentals/routing) for details on advanced routing.</span></span>

## <a name="setting-up-routing-middleware"></a><span data-ttu-id="8f5d8-114">Einrichten von Middleware Routing</span><span class="sxs-lookup"><span data-stu-id="8f5d8-114">Setting up Routing Middleware</span></span>

<span data-ttu-id="8f5d8-115">In Ihrem *konfigurieren* Methode möglicherweise Code ähnelt:</span><span class="sxs-lookup"><span data-stu-id="8f5d8-115">In your *Configure* method you may see code similar to:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="8f5d8-116">Beim Aufruf von `UseMvc`, `MapRoute` wird verwendet, um eine einzelne Route erstellen, die wir auf als verweisen müssen die `default` Route.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-116">Inside the call to `UseMvc`, `MapRoute` is used to create a single route, which we'll refer to as the `default` route.</span></span> <span data-ttu-id="8f5d8-117">Die meisten MVC-apps verwendet eine Route mit einer Vorlage ähnelt der `default` Route.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-117">Most MVC apps will use a route with a template similar to the `default` route.</span></span>

<span data-ttu-id="8f5d8-118">Die routenvorlage `"{controller=Home}/{action=Index}/{id?}"` kann einen URL-Pfad, z. B. entsprechen `/Products/Details/5` und extrahiert die Routenwerte `{ controller = Products, action = Details, id = 5 }` durch den Pfad zu versehen.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-118">The route template `"{controller=Home}/{action=Index}/{id?}"` can match a URL path like `/Products/Details/5` and will extract the route values `{ controller = Products, action = Details, id = 5 }` by tokenizing the path.</span></span> <span data-ttu-id="8f5d8-119">MVC versucht, einen Controller mit dem Namen suchen `ProductsController` , und führen Sie die Aktion `Details`:</span><span class="sxs-lookup"><span data-stu-id="8f5d8-119">MVC will attempt to locate a controller named `ProductsController` and run the action `Details`:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

<span data-ttu-id="8f5d8-120">Beachten Sie, dass in diesem Beispiel wurden die modellbindung den Wert der verwenden würden `id = 5` festzulegende der `id` Parameter `5` diese Aktion aufrufen.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-120">Note that in this example, model binding would use the value of `id = 5` to set the `id` parameter to `5` when invoking this action.</span></span> <span data-ttu-id="8f5d8-121">Finden Sie unter der [Modell Bindung](../models/model-binding.md) Weitere Details.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-121">See the [Model Binding](../models/model-binding.md) for more details.</span></span>

<span data-ttu-id="8f5d8-122">Mithilfe der `default` Route:</span><span class="sxs-lookup"><span data-stu-id="8f5d8-122">Using the `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="8f5d8-123">Die routenvorlage:</span><span class="sxs-lookup"><span data-stu-id="8f5d8-123">The route template:</span></span>

* <span data-ttu-id="8f5d8-124">`{controller=Home}`definiert `Home` als Standard`controller`</span><span class="sxs-lookup"><span data-stu-id="8f5d8-124">`{controller=Home}` defines `Home` as the default `controller`</span></span>

* <span data-ttu-id="8f5d8-125">`{action=Index}`definiert `Index` als Standard`action`</span><span class="sxs-lookup"><span data-stu-id="8f5d8-125">`{action=Index}` defines `Index` as the default `action`</span></span>

* <span data-ttu-id="8f5d8-126">`{id?}`definiert `id` als optional</span><span class="sxs-lookup"><span data-stu-id="8f5d8-126">`{id?}` defines `id` as optional</span></span>

<span data-ttu-id="8f5d8-127">Standard- und optionale Routenparameter müssen nicht in der URL-Pfad für eine Übereinstimmung vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-127">Default and optional route parameters do not need to be present in the URL path for a match.</span></span> <span data-ttu-id="8f5d8-128">Finden Sie unter [Route Vorlagenverweis](../../fundamentals/routing.md#route-template-reference) für eine ausführliche Beschreibung der Syntax der Route-Vorlage.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-128">See [Route Template Reference](../../fundamentals/routing.md#route-template-reference) for a detailed description of route template syntax.</span></span>

<span data-ttu-id="8f5d8-129">`"{controller=Home}/{action=Index}/{id?}"`kann den URL-Pfad entsprechen `/` und die Routenwerte entsteht `{ controller = Home, action = Index }`.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-129">`"{controller=Home}/{action=Index}/{id?}"` can match the URL path `/` and will produce the route values `{ controller = Home, action = Index }`.</span></span> <span data-ttu-id="8f5d8-130">Die Werte für `controller` und `action` stellen die Standardwerte verwenden `id` wird kein Wert erzeugt, da keine entsprechende Segment in der URL-Pfad vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-130">The values for `controller` and `action` make use of the default values, `id` does not produce a value since there is no corresponding segment in the URL path.</span></span> <span data-ttu-id="8f5d8-131">MVC würden diese Routenwerte verwenden, um Wählen Sie die `HomeController` und `Index` Aktion:</span><span class="sxs-lookup"><span data-stu-id="8f5d8-131">MVC would use these route values to select the `HomeController` and `Index` action:</span></span>

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

<span data-ttu-id="8f5d8-132">Mit diesem Controller Definition und die routenvorlage, die `HomeController.Index` Aktion wird für keines der folgenden URL-Pfade ausgeführt werden:</span><span class="sxs-lookup"><span data-stu-id="8f5d8-132">Using this controller definition and route template, the `HomeController.Index` action would be executed for any of the following URL paths:</span></span>

* `/Home/Index/17`

* `/Home/Index`

* `/Home`

* `/`

<span data-ttu-id="8f5d8-133">Die Hilfsmethode `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="8f5d8-133">The convenience method `UseMvcWithDefaultRoute`:</span></span>

```csharp
app.UseMvcWithDefaultRoute();
```

<span data-ttu-id="8f5d8-134">Kann verwendet werden, um zu ersetzen:</span><span class="sxs-lookup"><span data-stu-id="8f5d8-134">Can be used to replace:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="8f5d8-135">`UseMvc`und `UseMvcWithDefaultRoute` fügen eine Instanz des `RouterMiddleware` die Middleware-Pipeline.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-135">`UseMvc` and `UseMvcWithDefaultRoute` add an instance of `RouterMiddleware` to the middleware pipeline.</span></span> <span data-ttu-id="8f5d8-136">MVC interagiert nicht direkt mit Middleware und routing verwendet, um Anforderungen zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-136">MVC doesn't interact directly with middleware, and uses routing to handle requests.</span></span> <span data-ttu-id="8f5d8-137">MVC verbunden ist, auf die Routen über eine Instanz des `MvcRouteHandler`.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-137">MVC is connected to the routes through an instance of `MvcRouteHandler`.</span></span> <span data-ttu-id="8f5d8-138">Der Code innerhalb eines `UseMvc` ähnelt der folgenden:</span><span class="sxs-lookup"><span data-stu-id="8f5d8-138">The code inside of `UseMvc` is similar to the following:</span></span>

<!-- literal_block {"ids": [], "names": [], "backrefs": [], "dupnames": [], "xml:space": "preserve", "classes": []} -->

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

<span data-ttu-id="8f5d8-139">`UseMvc`definiert alle Routen nicht direkt hinzugefügt einen Platzhalter für die Auflistung der Routen für die `attribute` Route.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-139">`UseMvc` does not directly define any routes, it adds a placeholder to the route collection for the `attribute` route.</span></span> <span data-ttu-id="8f5d8-140">Die Überladung `UseMvc(Action<IRouteBuilder>)` können Sie Ihre eigenen Routen hinzufügen und unterstützt auch das routing-Attribut.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-140">The overload `UseMvc(Action<IRouteBuilder>)` lets you add your own routes and also supports attribute routing.</span></span>  <span data-ttu-id="8f5d8-141">`UseMvc`und alle seine Varianten Fügt einen Platzhalter für die attributenroute - routing-Attribut ist immer verfügbar, unabhängig von der Konfiguration `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-141">`UseMvc` and all of its variations adds a placeholder for the attribute route - attribute routing is always available regardless of how you configure `UseMvc`.</span></span> <span data-ttu-id="8f5d8-142">`UseMvcWithDefaultRoute`definiert eine Standardroute und routing-Attribut unterstützt.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-142">`UseMvcWithDefaultRoute` defines a default route and supports attribute routing.</span></span> <span data-ttu-id="8f5d8-143">Die [Routing-Attribut](#attribute-routing-ref-label) Abschnitt enthält weitere Informationen zu routing-Attribut.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-143">The [Attribute Routing](#attribute-routing-ref-label) section includes more details on attribute routing.</span></span>

<a name=routing-conventional-ref-label></a>

## <a name="conventional-routing"></a><span data-ttu-id="8f5d8-144">Herkömmliche routing</span><span class="sxs-lookup"><span data-stu-id="8f5d8-144">Conventional routing</span></span>

<span data-ttu-id="8f5d8-145">Die `default` Route:</span><span class="sxs-lookup"><span data-stu-id="8f5d8-145">The `default` route:</span></span>

<!-- literal_block {"ids": [], "names": [], "backrefs": [], "dupnames": [], "xml:space": "preserve", "classes": []} -->

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="8f5d8-146">ist ein Beispiel für eine *konventionellen routing*.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-146">is an example of a *conventional routing*.</span></span> <span data-ttu-id="8f5d8-147">Wir rufen Sie dieses Format *konventionellen routing* da-Objektvariablen eine *Konvention* für URL-Pfade:</span><span class="sxs-lookup"><span data-stu-id="8f5d8-147">We call this style *conventional routing* because it establishes a *convention* for URL paths:</span></span>

* <span data-ttu-id="8f5d8-148">das erste Pfadsegment ordnet den Namen des Controllers</span><span class="sxs-lookup"><span data-stu-id="8f5d8-148">the first path segment maps to the controller name</span></span>

* <span data-ttu-id="8f5d8-149">die zweite ordnet den Aktionsnamen.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-149">the second maps to the action name.</span></span>

* <span data-ttu-id="8f5d8-150">Das dritte Segment wird verwendet, einen optionalen `id` verwendet, um eine modellentität zuordnen</span><span class="sxs-lookup"><span data-stu-id="8f5d8-150">the third segment is used for an optional `id` used to map to a model entity</span></span>

<span data-ttu-id="8f5d8-151">Mit diesem `default` Route, die URL-Pfad `/Products/List` ordnet die `ProductsController.List` Aktion und `/Blog/Article/17` ordnet `BlogController.Article`.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-151">Using this `default` route, the URL path `/Products/List` maps to the `ProductsController.List` action, and `/Blog/Article/17` maps to `BlogController.Article`.</span></span> <span data-ttu-id="8f5d8-152">Diese Zuordnung basiert auf den Controller- und Namen **nur** und basiert nicht auf Namespaces, Speicherorte für Quelldateien oder Methodenparameter.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-152">This mapping is based on the controller and action names **only** and is not based on namespaces, source file locations, or method parameters.</span></span>

> [!TIP]
> <span data-ttu-id="8f5d8-153">Mithilfe der herkömmlichen routing mit die Standardroute können Sie die Anwendung schnell erstellen, ohne Sie zu einem neuen URL-Muster für jede Aktion, die Sie definieren.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-153">Using conventional routing with the default route allows you to build the application quickly without having to come up with a new URL pattern for each action you define.</span></span> <span data-ttu-id="8f5d8-154">Für eine Anwendung mit CRUD-Stil-Aktionen können Konsistenz für URLs zu, in Ihren Domänencontrollern Ihren Code vereinfachen die Benutzeroberfläche besser vorhersagbar zu machen.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-154">For an application with CRUD style actions, having consistency for the URLs across your controllers can help simplify your code and make your UI more predictable.</span></span>

> [!WARNING]
> <span data-ttu-id="8f5d8-155">Die `id` wird als optional definiert, durch die routenvorlage, was bedeutet, dass Ihre Aktionen werden, ohne die ID, die als Teil der URL bereitgestellt ausgeführt können.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-155">The `id` is defined as optional by the route template, meaning that your actions can execute without the ID provided as part of the URL.</span></span> <span data-ttu-id="8f5d8-156">In der Regel was passiert, wenn `id` fehlt in der URL ist, dass er festgelegt wird `0` von modellbindung und demzufolge keine Entität befinden sich in der Datenbank entsprechen `id == 0`.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-156">Usually what will happen if `id` is omitted from the URL is that it will be set to `0` by model binding, and as a result no entity will be found in the database matching `id == 0`.</span></span> <span data-ttu-id="8f5d8-157">Routing-Attribut können Sie die ID ist für einige Aktionen aus, und nicht für andere stellen eine differenzierte Steuerung gewähren.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-157">Attribute routing can give you fine-grained control to make the ID required for some actions and not for others.</span></span> <span data-ttu-id="8f5d8-158">Gemäß der Konvention, die die Dokumentation enthält optionale Parameter wie `id` , wenn sie sind wahrscheinlich in die richtige Verwendung angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-158">By convention the documentation will include optional parameters like `id` when they are likely to appear in correct usage.</span></span>

## <a name="multiple-routes"></a><span data-ttu-id="8f5d8-159">Mehrere Routen</span><span class="sxs-lookup"><span data-stu-id="8f5d8-159">Multiple routes</span></span>

<span data-ttu-id="8f5d8-160">Sie können mehrere Routen innerhalb von hinzufügen `UseMvc` durch Hinzufügen von mehr Aufrufe zu `MapRoute`.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-160">You can add multiple routes inside `UseMvc` by adding more calls to `MapRoute`.</span></span> <span data-ttu-id="8f5d8-161">Auf diese Weise können Sie mehrere Konventionen definieren, oder konventionellen Routen hinzu, die einer bestimmten Aktion zugeordnet sind, z. B.:</span><span class="sxs-lookup"><span data-stu-id="8f5d8-161">Doing so allows you to define multiple conventions, or to add conventional routes that are dedicated to a specific action, such as:</span></span>

<!-- literal_block {"ids": [], "names": [], "backrefs": [], "dupnames": [], "xml:space": "preserve", "classes": []} -->

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
}
```

<span data-ttu-id="8f5d8-162">Die `blog` Route hier ist ein *dedizierten konventionellen Route*, was bedeutet, dass das herkömmliche Routingsystem verwendet, aber für eine bestimmte Aktion dediziert ist.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-162">The `blog` route here is a *dedicated conventional route*, meaning that it uses the conventional routing system, but is dedicated to a specific action.</span></span> <span data-ttu-id="8f5d8-163">Da `controller` und `action` nicht als Parameter in der routenvorlage angezeigt, können nur die Standardwerte aufweisen und diese Route wird daher immer zugeordnet, auf die Aktion `BlogController.Article`.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-163">Since `controller` and `action` don't appear in the route template as parameters, they can only have the default values, and thus this route will always map to the action `BlogController.Article`.</span></span>

<span data-ttu-id="8f5d8-164">Routen in der Auflistung von Routen werden sortiert und verarbeitet werden, in der Reihenfolge, die sie hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-164">Routes in the route collection are ordered, and will be processed in the order they are added.</span></span> <span data-ttu-id="8f5d8-165">In diesem Beispiel daher die `blog` Route versucht, bevor die `default` Route.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-165">So in this example, the `blog` route will be tried before the `default` route.</span></span>

> [!NOTE]
> <span data-ttu-id="8f5d8-166">*Dedizierte konventionelle Routen* verwenden häufig Catchall-Routenparameter wie `{*article}` zum Erfassen des verbleibenden Teils der URL-Pfad.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-166">*Dedicated conventional routes* often use catch-all route parameters like `{*article}` to capture the remaining portion of the URL path.</span></span> <span data-ttu-id="8f5d8-167">Damit kann eine Route "zu gierigen", was bedeutet, dass er URLs findet, die Sie von anderen Routen abgeglichen werden sollen.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-167">This can make a route 'too greedy' meaning that it matches URLs that you intended to be matched by other routes.</span></span> <span data-ttu-id="8f5d8-168">Fügen Sie die "gierigen" Routen weiter unten in der Routingtabelle, um dieses Problem zu beheben.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-168">Put the 'greedy' routes later in the route table to solve this.</span></span>

### <a name="fallback"></a><span data-ttu-id="8f5d8-169">Fallback</span><span class="sxs-lookup"><span data-stu-id="8f5d8-169">Fallback</span></span>

<span data-ttu-id="8f5d8-170">Im Rahmen der anforderungsverarbeitung MVC überprüft, ob die Routenwerte verwendet werden können, auf einem Controller und Aktion in der Anwendung feststellen.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-170">As part of request processing, MVC will verify that the route values can be used to find a controller and action in your application.</span></span> <span data-ttu-id="8f5d8-171">Falls die Routenwerte eine Aktion übereinstimmen dann wird die Route kein Übereinstimmung angesehen, und die nächste Route wird versucht.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-171">If the route values don't match an action then the route is not considered a match, and the next route will be tried.</span></span> <span data-ttu-id="8f5d8-172">Hierbei spricht *fallback*, und es wurde vorgesehen, um Fälle zu vereinfachen, konventionelle Routen, die sich überlappen.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-172">This is called *fallback*, and it's intended to simplify cases where conventional routes overlap.</span></span>

### <a name="disambiguating-actions"></a><span data-ttu-id="8f5d8-173">Aktionen eindeutig gemacht</span><span class="sxs-lookup"><span data-stu-id="8f5d8-173">Disambiguating actions</span></span>

<span data-ttu-id="8f5d8-174">Wenn die beiden Aktionen werden über Weiterleitung durch übereinstimmen, muss MVC eindeutig machen, um wählen "beste" geeignet, da sonst eine Ausnahme auslösen.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-174">When two actions match through routing, MVC must disambiguate to choose the 'best' candidate or else throw an exception.</span></span> <span data-ttu-id="8f5d8-175">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8f5d8-175">For example:</span></span>

<!-- literal_block {"ids": [], "names": [], "backrefs": [], "dupnames": [], "xml:space": "preserve", "classes": []} -->

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

<span data-ttu-id="8f5d8-176">Dieser Controller definiert zwei Aktionen, die mit den URL-Pfad übereinstimmen würden `/Products/Edit/17` und Weiterleiten von Daten `{ controller = Products, action = Edit, id = 17 }`.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-176">This controller defines two actions that would match the URL path `/Products/Edit/17` and route data `{ controller = Products, action = Edit, id = 17 }`.</span></span> <span data-ttu-id="8f5d8-177">Dies ist ein typisches Muster für MVC-Controller, in denen `Edit(int)` zeigt ein Formular zum Bearbeiten eines Produkts und `Edit(int, Product)` bereitgestellte Formular verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-177">This is a typical pattern for MVC controllers where `Edit(int)` shows a form to edit a product, and `Edit(int, Product)` processes  the posted form.</span></span> <span data-ttu-id="8f5d8-178">Um dies zu ermöglichen MVC auswählen müssen `Edit(int, Product)` bei die Anforderung ist ein HTTP `POST` und `Edit(int)` Wenn das HTTP-Verb ist etwas anderes.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-178">To make this possible MVC would need to choose `Edit(int, Product)` when the request is an HTTP `POST` and `Edit(int)` when the HTTP verb is anything else.</span></span>

<span data-ttu-id="8f5d8-179">Die `HttpPostAttribute` ( `[HttpPost]` ) ist eine Implementierung von `IActionConstraint` nur die Aktion, die ausgewählt werden, wenn das HTTP-Verb ermöglichen `POST`.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-179">The `HttpPostAttribute` ( `[HttpPost]` ) is an implementation of `IActionConstraint` that will only allow the action to be selected when the HTTP verb is `POST`.</span></span> <span data-ttu-id="8f5d8-180">Das Vorhandensein einer `IActionConstraint` macht die `Edit(int, Product)` "bessere" übereinstimmen, als `Edit(int)`, sodass `Edit(int, Product)` wird zuerst versucht werden.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-180">The presence of an `IActionConstraint` makes the `Edit(int, Product)` a 'better' match than `Edit(int)`, so `Edit(int, Product)` will be tried first.</span></span>

<span data-ttu-id="8f5d8-181">Sie müssen nur zum Schreiben von benutzerdefinierten `IActionConstraint` Implementierungen in speziellen Szenarien, aber es wichtig zu verstehen, die Rolle des Attribute wie den `HttpPostAttribute` -ähnliche Attributen für andere HTTP-Verben definiert sind.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-181">You will only need to write custom `IActionConstraint` implementations in specialized scenarios, but it's important to understand the role of attributes like `HttpPostAttribute`  - similar attributes are defined for other HTTP verbs.</span></span> <span data-ttu-id="8f5d8-182">In herkömmlichen routing es ist üblich für Aktionen, die den gleichen Aktionsnamen verwenden, werden Teil einer `show form -> submit form` Workflow.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-182">In conventional routing it's common for actions to use the same action name when they are part of a `show form -> submit form` workflow.</span></span> <span data-ttu-id="8f5d8-183">Die Vorteile dieses Muster wird offensichtlich, mehr nach dem Überprüfen der [Verständnis IActionConstraint](#understanding-iactionconstraint) Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-183">The convenience of this pattern will become more apparent after reviewing the [Understanding IActionConstraint](#understanding-iactionconstraint) section.</span></span>

<span data-ttu-id="8f5d8-184">Wenn mehrere Routen entsprechen und MVC eine "bewährte" Route kann nicht gefunden werden kann, löst sie eine `AmbiguousActionException`.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-184">If multiple routes match, and MVC can't find a 'best' route, it will throw an `AmbiguousActionException`.</span></span>

<a name=routing-route-name-ref-label></a>

### <a name="route-names"></a><span data-ttu-id="8f5d8-185">Routennamen</span><span class="sxs-lookup"><span data-stu-id="8f5d8-185">Route names</span></span>

<span data-ttu-id="8f5d8-186">Die Zeichenfolgen `"blog"` und `"default"` in den folgenden Beispielen werden Routennamen:</span><span class="sxs-lookup"><span data-stu-id="8f5d8-186">The strings  `"blog"` and `"default"` in the following examples are route names:</span></span>


```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
               defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="8f5d8-187">Die Routennamen benennen Sie der Route logische, damit für URL-Generierung die benannte Route verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-187">The route names give the route a logical name so that the named route can be used for URL generation.</span></span> <span data-ttu-id="8f5d8-188">Dadurch wird URL-Erstellung erheblich vereinfacht, um die Reihenfolge der Routen URL-Generierung kompliziert vornehmen kann.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-188">This greatly simplifies URL creation when the ordering of routes could make URL generation complicated.</span></span> <span data-ttu-id="8f5d8-189">Routen-Namen müssen eindeutig eine anwendungsweite sein.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-189">Routes names must be unique application-wide.</span></span>

<span data-ttu-id="8f5d8-190">Routennamen haben keine Auswirkung auf die URL entsprechen oder die Verarbeitung von Anforderungen; Sie werden nur für die Erzeugung der URL verwendet.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-190">Route names have no impact on URL matching or handling of requests; they are used only for URL generation.</span></span> <span data-ttu-id="8f5d8-191">[Routing](xref:fundamentals/routing) enthält weitere ausführliche Informationen zum URL-Generierung, einschließlich der URL-Generierung in MVC-spezifische Hilfsprogramme.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-191">[Routing](xref:fundamentals/routing) has more detailed information on URL generation including URL generation in MVC-specific helpers.</span></span>

<a name=attribute-routing-ref-label></a>

## <a name="attribute-routing"></a><span data-ttu-id="8f5d8-192">Routing-Attribut</span><span class="sxs-lookup"><span data-stu-id="8f5d8-192">Attribute routing</span></span>

<span data-ttu-id="8f5d8-193">Routing-Attribut verwendet einen Satz von Attributen, um Aktionen routenvorlagen direkt zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-193">Attribute routing uses a set of attributes to map actions directly to route templates.</span></span> <span data-ttu-id="8f5d8-194">Im folgenden Beispiel `app.UseMvc();` werden in der `Configure` -Methode und keine Route übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-194">In the following example, `app.UseMvc();` is used in the `Configure` method and no route is passed.</span></span> <span data-ttu-id="8f5d8-195">Die `HomeController` entspricht einen Satz von URLs, welche die Standardroute ähnelt `{controller=Home}/{action=Index}/{id?}` entsprechen:</span><span class="sxs-lookup"><span data-stu-id="8f5d8-195">The `HomeController` will match a set of URLs similar to what the default route `{controller=Home}/{action=Index}/{id?}` would match:</span></span>

```csharp
public class HomeController : Controller
{
   [Route("")]
   [Route("Home")]
   [Route("Home/Index")]
   public IActionResult Index()
   {
      return View();
   }
   [Route("Home/About")]
   public IActionResult About()
   {
      return View();
   }
   [Route("Home/Contact")]
   public IActionResult Contact()
   {
      return View();
   }
}
```

<span data-ttu-id="8f5d8-196">Die `HomeController.Index()` Aktion wird ausgeführt werden, für den URL-Pfaden `/`, `/Home`, oder `/Home/Index`.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-196">The `HomeController.Index()` action will be executed for any of the URL paths `/`, `/Home`, or `/Home/Index`.</span></span>

> [!NOTE]
> <span data-ttu-id="8f5d8-197">In diesem Beispiel werden eine programming Hauptunterschied zwischen routing-Attribut und herkömmliche routing hervorgehoben.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-197">This example highlights a key programming difference between attribute routing and conventional routing.</span></span> <span data-ttu-id="8f5d8-198">Routing-Attribut erfordert weitere Eingabe, um eine Route anzugeben. die herkömmliche Standardroute verarbeitet Routen prägnanter.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-198">Attribute routing requires more input to specify a route; the conventional default route handles routes more succinctly.</span></span> <span data-ttu-id="8f5d8-199">Allerdings routing-Attribut zulässt (und erfordert) präzise Kontrolle der routenvorlagen für jede Aktion gelten.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-199">However, attribute routing allows (and requires) precise control of which route templates apply to each action.</span></span>

<span data-ttu-id="8f5d8-200">Mit dem Controllernamen und Aktionsnamen routing-Attribut wiedergeben **keine** Rolle in der Aktion aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-200">With attribute routing the controller name and action names play **no** role in which action is selected.</span></span> <span data-ttu-id="8f5d8-201">In diesem Beispiel werden die gleichen wie im vorherigen Beispiel-URLs abgeglichen.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-201">This example will match the same URLs as the previous example.</span></span>

```csharp
public class MyDemoController : Controller
{
   [Route("")]
   [Route("Home")]
   [Route("Home/Index")]
   public IActionResult MyIndex()
   {
      return View("Index");
   }
   [Route("Home/About")]
   public IActionResult MyAbout()
   {
      return View("About");
   }
   [Route("Home/Contact")]
   public IActionResult MyContact()
   {
      return View("Contact");
   }
}
```

> [!NOTE]
> <span data-ttu-id="8f5d8-202">Die oben genannten routenvorlagen nicht für die Routenparameter definieren `action`, `area`, und `controller`.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-202">The route templates above don't define route parameters for `action`, `area`, and `controller`.</span></span> <span data-ttu-id="8f5d8-203">Diese Routenparameter sind in der Tat in attributenrouten nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-203">In fact, these route parameters are not allowed in attribute routes.</span></span> <span data-ttu-id="8f5d8-204">Da die routenvorlage bereits mit der Aktion zugeordnet ist, wäre nicht sinnvoll, den Namen der Aktion aus der URL analysieren erleichtern.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-204">Since the route template is already associated with an action, it wouldn't make sense to parse the action name from the URL.</span></span>

## <a name="attribute-routing-with-httpverb-attributes"></a><span data-ttu-id="8f5d8-205">Routing mit Attributen Http [Verb]-Attribut</span><span class="sxs-lookup"><span data-stu-id="8f5d8-205">Attribute routing with Http[Verb] attributes</span></span>

<span data-ttu-id="8f5d8-206">Routing-Attribut kann auch vornehmen, verwenden die `Http[Verb]` Attribute wie `HttpPostAttribute`.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-206">Attribute routing can also make use of the `Http[Verb]` attributes such as `HttpPostAttribute`.</span></span> <span data-ttu-id="8f5d8-207">Alle diese Attribute können keine routenvorlage akzeptieren.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-207">All of these attributes can accept a route template.</span></span> <span data-ttu-id="8f5d8-208">Dieses Beispiel zeigt zwei Aktionen, die die gleichen routenvorlage entsprechen:</span><span class="sxs-lookup"><span data-stu-id="8f5d8-208">This example shows two actions that match the same route template:</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

```csharp
[HttpGet("/products")]
public IActionResult ListProducts()
{
   // ...
}

[HttpPost("/products")]
public IActionResult CreateProduct(...)
{
   // ...
}
```

<span data-ttu-id="8f5d8-209">Für einen URL-Pfad, z. B. `/products` der `ProductsApi.ListProducts` Aktion wird ausgeführt werden, wenn das HTTP-Verb `GET` und `ProductsApi.CreateProduct` wird ausgeführt werden, wenn das HTTP-Verb `POST`.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-209">For a URL path like `/products` the `ProductsApi.ListProducts` action will be executed when the HTTP verb is `GET` and `ProductsApi.CreateProduct` will be executed when the HTTP verb is `POST`.</span></span> <span data-ttu-id="8f5d8-210">Zuerst routing-Attribut entspricht den URL für den Satz von routenvorlagen durch routenattributen definiert.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-210">Attribute routing first matches the URL against the set of route templates defined by route attributes.</span></span> <span data-ttu-id="8f5d8-211">Nachdem Sie eine routenvorlage für die übereinstimmt, `IActionConstraint` Einschränkungen werden angewendet, um zu bestimmen, welche Aktionen ausgeführt werden können.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-211">Once a route template matches, `IActionConstraint` constraints are applied to determine which actions can be executed.</span></span>

> [!TIP]
> <span data-ttu-id="8f5d8-212">Wenn Sie eine REST-API zu erstellen, ist selten, dass Sie verwenden möchten `[Route(...)]` einer Aktionsmethode aufwiesen.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-212">When building a REST API, it's rare that you will want to use `[Route(...)]` on an action method.</span></span> <span data-ttu-id="8f5d8-213">Es ist besser, verwenden Sie die spezifischere `Http*Verb*Attributes` präzise dazu, was Ihre API unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-213">It's better to use the more specific `Http*Verb*Attributes` to be precise about what your API supports.</span></span> <span data-ttu-id="8f5d8-214">Zu wissen, welche bestimmten logischen Operationen Pfade und HTTP-Verben zugeordnet, werden Clients von REST-APIs erwartet.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-214">Clients of REST APIs are expected to know what paths and HTTP verbs map to specific logical operations.</span></span>

<span data-ttu-id="8f5d8-215">Da eine attributenroute für eine bestimmte Aktion gültig ist, ist es ganz einfach als Teil der Vorlage Routendefinition erforderlichen Parameter.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-215">Since an attribute route applies to a specific action, it's easy to make parameters required as part of the route template definition.</span></span> <span data-ttu-id="8f5d8-216">In diesem Beispiel `id` als Teil des URL-Pfads erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-216">In this example, `id` is required as part of the URL path.</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="8f5d8-217">Die `ProductsApi.GetProduct(int)` Aktion wird ausgeführt für einen URL-Pfad, z. B. `/products/3` jedoch nicht für einen URL-Pfad, z. B. `/products`.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-217">The `ProductsApi.GetProduct(int)` action will be executed for a URL path like `/products/3` but not for a URL path like `/products`.</span></span> <span data-ttu-id="8f5d8-218">Finden Sie unter [Routing](../../fundamentals/routing.md) für eine vollständige Beschreibung der routenvorlagen und verwandten Optionen.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-218">See [Routing](../../fundamentals/routing.md) for a full description of route templates and related options.</span></span>

## <a name="route-name"></a><span data-ttu-id="8f5d8-219">Routenname</span><span class="sxs-lookup"><span data-stu-id="8f5d8-219">Route Name</span></span>

<span data-ttu-id="8f5d8-220">Der folgende Code definiert eine *Routennamen* von `Products_List`:</span><span class="sxs-lookup"><span data-stu-id="8f5d8-220">The following code  defines a *route name* of `Products_List`:</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="8f5d8-221">Routennamen können verwendet werden, um eine URL basierend auf einer bestimmten Route zu generieren.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-221">Route names can be used to generate a URL based on a specific route.</span></span> <span data-ttu-id="8f5d8-222">Routennamen haben keine Auswirkung auf die URL Vergleichsverhalten von routing und dienen nur zum URL-Generierung.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-222">Route names have no impact on the URL matching behavior of routing and are only used for URL generation.</span></span> <span data-ttu-id="8f5d8-223">Routennamen müssen anwendungsweite eindeutig sein.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-223">Route names must be unique application-wide.</span></span>

> [!NOTE]
> <span data-ttu-id="8f5d8-224">Vergleichen Sie dies mit der konventionellen *Standardroute*, definiert die `id` Parameter als optional (`{id?}`).</span><span class="sxs-lookup"><span data-stu-id="8f5d8-224">Contrast this with the conventional *default route*, which defines the `id` parameter as optional (`{id?}`).</span></span> <span data-ttu-id="8f5d8-225">Diese Fähigkeit, genau APIs geben bietet Vorteile, z. B. schreibbarkeit `/products` und `/products/5` an unterschiedliche Aktionen gesendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-225">This ability to precisely specify APIs has advantages, such as  allowing `/products` and `/products/5` to be dispatched to different actions.</span></span>

<a name=routing-combining-ref-label></a>

### <a name="combining-routes"></a><span data-ttu-id="8f5d8-226">Kombinieren von Routen</span><span class="sxs-lookup"><span data-stu-id="8f5d8-226">Combining routes</span></span>

<span data-ttu-id="8f5d8-227">Um machen routing-Attribut weniger wiederkehrende werden routenattributen auf dem Controller mit routenattribute in die einzelnen Aktionen kombiniert.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-227">To make attribute routing less repetitive, route attributes on the controller are combined with route attributes on the individual actions.</span></span> <span data-ttu-id="8f5d8-228">Routenvorlagen auf dem Controller definiert sind routenvorlagen zu den Aktionen vorangestellt.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-228">Any route templates defined on the controller are prepended to route templates on the actions.</span></span> <span data-ttu-id="8f5d8-229">Platzierung von Route-Attribut auf dem Controller macht **alle** Aktionen im Controller verwenden routing-Attribut.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-229">Placing a route attribute on the controller makes **all** actions in the controller use attribute routing.</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

```csharp
[Route("products")]
public class ProductsApiController : Controller
{
   [HttpGet]
   public IActionResult ListProducts() { ... }

   [HttpGet("{id}")]
   public ActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="8f5d8-230">In diesem Beispiel wird die URL-Pfad `/products` erfüllen können `ProductsApi.ListProducts`, und die URL-Pfad `/products/5` erfüllen können `ProductsApi.GetProduct(int)`.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-230">In this example the URL path `/products` can match `ProductsApi.ListProducts`, and the URL path `/products/5` can match `ProductsApi.GetProduct(int)`.</span></span> <span data-ttu-id="8f5d8-231">Beide der genannten Aktionen nur HTTP abgeglichen `GET` , da sie mit ergänzt werden die `HttpGetAttribute`.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-231">Both of these actions only match HTTP `GET` because they are decorated with the `HttpGetAttribute`.</span></span>

<span data-ttu-id="8f5d8-232">Weiterleiten von Vorlagen, die auf eine Aktion, die mit einer `/` sind nicht mit routenvorlagen gilt für den Controller kombiniert abrufen.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-232">Route templates applied to an action that begin with a `/` do not get combined with route templates applied to the controller.</span></span> <span data-ttu-id="8f5d8-233">In diesem Beispiel vergleicht eine Sammlung von URL-Pfade ähnelt der *Standardroute*.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-233">This example matches a set of URL paths similar to the *default route*.</span></span>

```csharp
[Route("Home")]
public class HomeController : Controller
{
    [Route("")]      // Combines to define the route template "Home"
    [Route("Index")] // Combines to define the route template "Home/Index"
    [Route("/")]     // Does not combine, defines the route template ""
    public IActionResult Index()
    {
        ViewData["Message"] = "Home index";
        var url = Url.Action("Index", "Home");
        ViewData["Message"] = "Home index" + "var url = Url.Action; =  " + url;
        return View();
    }

    [Route("About")] // Combines to define the route template "Home/About"
    public IActionResult About()
    {
        return View();
    }   
}
```

<a name=routing-ordering-ref-label></a>

### <a name="ordering-attribute-routes"></a><span data-ttu-id="8f5d8-234">Sortierung attributenrouten</span><span class="sxs-lookup"><span data-stu-id="8f5d8-234">Ordering attribute routes</span></span>

<span data-ttu-id="8f5d8-235">Im Gegensatz zu herkömmlichen Routen, die in einer definierten Reihenfolge ausgeführt, eine Struktur erstellt, und alle Routen gleichzeitig entspricht routing-Attribut.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-235">In contrast to conventional routes which execute in a defined order, attribute routing builds a tree and matches all routes simultaneously.</span></span> <span data-ttu-id="8f5d8-236">Dies verhält sich wie – wenn die routeneinträge liefern platziert wurden, zu einer idealen Sortierung; die spezifischsten Routen haben die Möglichkeit, vor der allgemeineren Routen ausführen.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-236">This behaves as-if the route entries were placed in an ideal ordering; the most specific routes have a chance to execute before the more general routes.</span></span>

<span data-ttu-id="8f5d8-237">Z. B. eine Route wie `blog/search/{topic}` ist genauer als eine Route wie `blog/{*article}`.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-237">For example, a route like `blog/search/{topic}` is more specific than a route like `blog/{*article}`.</span></span> <span data-ttu-id="8f5d8-238">Logisch gesehen der `blog/search/{topic}` Route "" zuerst, wird standardmäßig ausgeführt, da dies nur sinnvolle Sortierung ist.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-238">Logically speaking the `blog/search/{topic}` route 'runs' first, by default, because that's the only sensible ordering.</span></span> <span data-ttu-id="8f5d8-239">Herkömmliche routing verwenden, ist der Entwickler verantwortlich für die Platzierung von Routen in der gewünschten Reihenfolge.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-239">Using conventional routing, the developer is  responsible for placing routes in the desired order.</span></span>

<span data-ttu-id="8f5d8-240">Attributenrouten können konfigurieren, dass eine Bestellung mit der `Order` -Eigenschaft aller Attribute Route Framework bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-240">Attribute routes can configure an order, using the `Order` property of all of the framework provided route attributes.</span></span> <span data-ttu-id="8f5d8-241">Routen werden verarbeitet, entsprechend eine aufsteigende Sortierung der `Order` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-241">Routes are processed according to an ascending sort of the `Order` property.</span></span> <span data-ttu-id="8f5d8-242">Die Standardreihenfolge ist `0`.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-242">The default order is `0`.</span></span> <span data-ttu-id="8f5d8-243">Festlegen einer Route mit `Order = -1` ausgeführt wird, bevor die Routen, die eine Bestellung nicht festlegen.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-243">Setting a route using `Order = -1` will run before routes that don't set an order.</span></span> <span data-ttu-id="8f5d8-244">Festlegen einer Route mit `Order = 1` nach Route Standardreihenfolge ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-244">Setting a route using `Order = 1` will run after default route ordering.</span></span>

> [!TIP]
> <span data-ttu-id="8f5d8-245">Vermeiden Sie je nach `Order`.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-245">Avoid depending on `Order`.</span></span> <span data-ttu-id="8f5d8-246">Wenn die URL-Space explizite Werte ordnungsgemäß weiterleiten erforderlich ist, ist es wahrscheinlich für Clients sowie verwirrend.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-246">If your URL-space requires explicit order values to route correctly, then it's likely confusing to clients as well.</span></span> <span data-ttu-id="8f5d8-247">Routing-Attribut wird im Allgemeinen die richtige Route mit übereinstimmenden URL ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-247">In general attribute routing will select the correct route with URL matching.</span></span> <span data-ttu-id="8f5d8-248">Wenn die Standardreihenfolge zum URL-Generierung nicht funktioniert, verwenden Routennamen als eine Außerkraftsetzung in der Regel einfacher als das Anwenden der `Order` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-248">If the default order used for URL generation isn't working, using route name as an override is usually simpler than applying the `Order` property.</span></span>

<a name=routing-token-replacement-templates-ref-label></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a><span data-ttu-id="8f5d8-249">Ersetzung in routenvorlagen Token ([Controller], [Aktion] [Bereich])</span><span class="sxs-lookup"><span data-stu-id="8f5d8-249">Token replacement in route templates ([controller], [action], [area])</span></span>

<span data-ttu-id="8f5d8-250">Der Einfachheit halber attributenrouten unterstützen *token Ersatz* indem ein Token in eckige Klammern einschließen (`[`, `]`).</span><span class="sxs-lookup"><span data-stu-id="8f5d8-250">For convenience, attribute routes support *token replacement* by enclosing a token in square-braces (`[`, `]`).</span></span> <span data-ttu-id="8f5d8-251">Die Token `[action]`, `[area]`, und `[controller]` ersetzt werden mit den Werten der Aktionsname, der Bereichsname und des Controllernamens von der Aktion, in dem die Route definiert ist.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-251">The tokens `[action]`, `[area]`, and `[controller]` will be replaced with the values of the action name, area name, and controller name from the action where the route is defined.</span></span> <span data-ttu-id="8f5d8-252">In diesem Beispiel können die Aktionen URL-Pfade entsprechen, wie in den Kommentaren beschrieben:</span><span class="sxs-lookup"><span data-stu-id="8f5d8-252">In this example the actions can match URL paths as described in the comments:</span></span>

<span data-ttu-id="8f5d8-253">[!code-csharp[Main](routing/sample/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]</span><span class="sxs-lookup"><span data-stu-id="8f5d8-253">[!code-csharp[Main](routing/sample/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]</span></span>

<span data-ttu-id="8f5d8-254">Tokenersetzung tritt auf, als der letzte Schritt der Erstellung der attributenrouten.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-254">Token replacement occurs as the last step of building the attribute routes.</span></span> <span data-ttu-id="8f5d8-255">Im obigen Beispiel verhält sich der folgende Code entspricht:</span><span class="sxs-lookup"><span data-stu-id="8f5d8-255">The above example will behave the same as the following code:</span></span>

<span data-ttu-id="8f5d8-256">[!code-csharp[Main](routing/sample/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]</span><span class="sxs-lookup"><span data-stu-id="8f5d8-256">[!code-csharp[Main](routing/sample/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]</span></span>

<span data-ttu-id="8f5d8-257">Attributenrouten können auch mit Vererbung kombiniert werden.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-257">Attribute routes can also be combined with inheritance.</span></span> <span data-ttu-id="8f5d8-258">Dies ist besonders effektiv mit tokenersetzung kombiniert.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-258">This is particularly powerful combined with token replacement.</span></span>

```csharp
[Route("api/[controller]")]
public abstract class MyBaseController : Controller { ... }

public class ProductsController : MyBaseController
{
   [HttpGet] // Matches '/api/Products'
   public IActionResult List() { ... }

   [HttpPost("{id}")] // Matches '/api/Products/{id}'
   public IActionResult Edit(int id) { ... }
}
```

<span data-ttu-id="8f5d8-259">Tokenersetzung gilt auch für Routennamen durch attributenrouten definiert.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-259">Token replacement also applies to route names defined by attribute routes.</span></span> <span data-ttu-id="8f5d8-260">`[Route("[controller]/[action]", Name="[controller]_[action]")]`generiert einen eindeutigen Routennamen für jede Aktion.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-260">`[Route("[controller]/[action]", Name="[controller]_[action]")]` will generate a unique route name for each action.</span></span>

<span data-ttu-id="8f5d8-261">Das literal tokenersetzung Trennzeichen entsprechend `[` oder `]`, es durch die Wiederholung des Zeichens mit Escapezeichen versehen (`[[` oder `]]`).</span><span class="sxs-lookup"><span data-stu-id="8f5d8-261">To match the literal token replacement delimiter `[` or  `]`, escape it by repeating the character (`[[` or `]]`).</span></span>

<a name=routing-multiple-routes-ref-label></a>

### <a name="multiple-routes"></a><span data-ttu-id="8f5d8-262">Mehrere Routen</span><span class="sxs-lookup"><span data-stu-id="8f5d8-262">Multiple Routes</span></span>

<span data-ttu-id="8f5d8-263">Routing unterstützt mehrere Routen, die die gleiche Aktion erreichen definieren des Attributs.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-263">Attribute routing supports defining multiple routes that reach the same action.</span></span> <span data-ttu-id="8f5d8-264">Die häufigste Verwendung dieses imitieren, die das Verhalten entspricht der *konventionellen Standardroute* wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="8f5d8-264">The most common usage of this is to mimic the behavior of the *default conventional route* as shown in the following example:</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

<span data-ttu-id="8f5d8-265">Einfügen von mehreren routenattributen auf dem Controller, bedeutet, dass es sich bei jeweils mit jedem der routenattribute in den Aktionsmethoden kombiniert werden.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-265">Putting multiple route attributes on the controller means that each one will combine with each of the route attributes on the action methods.</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

```csharp
[Route("Store")]
[Route("[controller]")]
public class ProductsController : Controller
{
   [HttpPost("Buy")]     // Matches 'Products/Buy' and 'Store/Buy'
   [HttpPost("Checkout")] // Matches 'Products/Checkout' and 'Store/Checkout'
   public IActionResult Buy()
}
```

<span data-ttu-id="8f5d8-266">Bei mehreren routenattributen (implementiert, `IActionConstraint`) werden auf eine Aktion platziert, und klicken Sie dann jede Aktion-Einschränkung mit der routenvorlage aus dem Attribut kombiniert, die es definiert.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-266">When multiple route attributes (that implement `IActionConstraint`) are placed on an action, then each action constraint combines with the route template from the attribute that defined it.</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

```csharp
[Route("api/[controller]")]
public class ProductsController : Controller
{
   [HttpPut("Buy")]      // Matches PUT 'api/Products/Buy'
   [HttpPost("Checkout")] // Matches POST 'api/Products/Checkout'
   public IActionResult Buy()
}
```

> [!TIP]
> <span data-ttu-id="8f5d8-267">Während mehrere Routen zu Aktionen mit leistungsstarke erscheinen kann, ist es besser, URL-Bereich für die Anwendung einfach und klar definierten beibehalten.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-267">While using multiple routes on actions can seem powerful, it's better to keep your application's URL space simple and well-defined.</span></span> <span data-ttu-id="8f5d8-268">Verwenden Sie mehrere Routen zu Aktionen nur, z. B. zur Unterstützung von vorhandenen Clients erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-268">Use multiple routes on actions only where needed, for example to support existing clients.</span></span>

<a name=routing-attr-options></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a><span data-ttu-id="8f5d8-269">Angeben Optionaler Routenparameter Attribut, Standardwerte und Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="8f5d8-269">Specifying attribute route optional parameters, default values, and constraints</span></span>

<span data-ttu-id="8f5d8-270">Attributenrouten unterstützen die gleichen Inline-Syntax als herkömmliche Routen optionale Parameter, Standardwerte und Einschränkungen angeben.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-270">Attribute routes support the same inline syntax as conventional routes to specify optional parameters, default values, and constraints.</span></span>

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

<span data-ttu-id="8f5d8-271">Finden Sie unter [Route Vorlagenverweis](../../fundamentals/routing.md#route-template-reference) für eine ausführliche Beschreibung der Syntax der Route-Vorlage.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-271">See [Route Template Reference](../../fundamentals/routing.md#route-template-reference) for a detailed description of route template syntax.</span></span>

<a name=routing-cust-rt-attr-irt-ref-label></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a><span data-ttu-id="8f5d8-272">Benutzerdefinierte routenattributen verwenden`IRouteTemplateProvider`</span><span class="sxs-lookup"><span data-stu-id="8f5d8-272">Custom route attributes using `IRouteTemplateProvider`</span></span>

<span data-ttu-id="8f5d8-273">Alle routenattribute im Framework bereitgestellt ( `[Route(...)]`, `[HttpGet(...)]` usw.) implementieren die `IRouteTemplateProvider` Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-273">All of the route attributes provided in the framework ( `[Route(...)]`, `[HttpGet(...)]` , etc.) implement the `IRouteTemplateProvider` interface.</span></span> <span data-ttu-id="8f5d8-274">MVC sucht nach Attribute auf Controllerklassen und Aktionsmethoden auf, wenn die Anwendung gestartet und verwendet diejenigen, die implementieren `IRouteTemplateProvider` den anfänglichen Satz von Routen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-274">MVC looks for attributes on controller classes and action methods when the app starts and uses the ones that implement `IRouteTemplateProvider` to build the initial set of routes.</span></span>

<span data-ttu-id="8f5d8-275">Sie können implementieren `IRouteTemplateProvider` Definieren eigener routenattribute.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-275">You can implement `IRouteTemplateProvider` to define your own route attributes.</span></span> <span data-ttu-id="8f5d8-276">Jede `IRouteTemplateProvider` sortieren können Sie eine einzelne Route mit einer benutzerdefinierten routenvorlage definieren, und nennen Sie:</span><span class="sxs-lookup"><span data-stu-id="8f5d8-276">Each `IRouteTemplateProvider` allows you to define a single route with a custom route template, order, and name:</span></span>

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

<span data-ttu-id="8f5d8-277">Automatisch das Attribut aus dem obigen Beispiel legt die `Template` auf `"api/[controller]"` Wenn `[MyApiController]` angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-277">The attribute from the above example automatically sets the `Template` to `"api/[controller]"` when `[MyApiController]` is applied.</span></span>

<a name=routing-app-model-ref-label></a>

### <a name="using-application-model-to-customize-attribute-routes"></a><span data-ttu-id="8f5d8-278">Anpassen von attributenrouten mithilfe Anwendungsmodell</span><span class="sxs-lookup"><span data-stu-id="8f5d8-278">Using Application Model to customize attribute routes</span></span>

<span data-ttu-id="8f5d8-279">Die *Anwendungsmodell* ist ein Objektmodell erstellt beim Starten aller Metadaten von MVC, weiterleiten und Aktionen ausführen.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-279">The *application model* is an object model created at startup with all of the metadata used by MVC to route and execute your actions.</span></span> <span data-ttu-id="8f5d8-280">Die *Anwendungsmodell* enthält alle Daten vom routenattributen erfasst (über `IRouteTemplateProvider`).</span><span class="sxs-lookup"><span data-stu-id="8f5d8-280">The *application model* includes all of the data gathered from route attributes (through `IRouteTemplateProvider`).</span></span> <span data-ttu-id="8f5d8-281">Sie können schreiben *Konventionen* so ändern Sie das Anwendungsmodell zum Startzeitpunkt anpassen, wie routing verhält.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-281">You can write *conventions* to modify the application model at startup time to customize how routing behaves.</span></span> <span data-ttu-id="8f5d8-282">Dieser Abschnitt zeigt ein einfaches Beispiel für das routing mit Anwendungsmodell anpassen.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-282">This section shows a simple example of customizing routing using application model.</span></span>

<span data-ttu-id="8f5d8-283">[!code-csharp[Main](routing/sample/main/NamespaceRoutingConvention.cs)]</span><span class="sxs-lookup"><span data-stu-id="8f5d8-283">[!code-csharp[Main](routing/sample/main/NamespaceRoutingConvention.cs)]</span></span>

<a name=routing-mixed-ref-label></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a><span data-ttu-id="8f5d8-284">Gemischte routing:-Attribut routing Vs konventionellen routing</span><span class="sxs-lookup"><span data-stu-id="8f5d8-284">Mixed routing: Attribute routing vs conventional routing</span></span>

<span data-ttu-id="8f5d8-285">MVC-Anwendungen können die Verwendung von herkömmlichen routing und routing-Attribut kombinieren.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-285">MVC applications can mix the use of conventional routing and attribute routing.</span></span> <span data-ttu-id="8f5d8-286">Es ist in der Regel verwenden herkömmliche Routen für Domänencontroller, die für den HTML-Seiten für Browser, und das Attribut routing für Domänencontroller, die für den REST-APIs.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-286">It's typical to use conventional routes for controllers serving HTML pages for browsers, and attribute routing for controllers serving REST APIs.</span></span>

<span data-ttu-id="8f5d8-287">Aktionen werden konventionell entweder weitergeleitet oder Attribut weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-287">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="8f5d8-288">Platzieren eine Route auf dem Controller oder die Aktion erleichtert Attribut weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-288">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="8f5d8-289">Aktionen, die attributenrouten definieren, können nicht über die herkömmliche Routen und umgekehrt erreicht werden.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-289">Actions that define attribute routes cannot be reached through the conventional routes and vice-versa.</span></span> <span data-ttu-id="8f5d8-290">**Alle** routenattribut auf dem Controller macht alle Aktionen im Controller Attribut weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-290">**Any** route attribute on the controller makes all actions in the controller attribute routed.</span></span>

> [!NOTE]
> <span data-ttu-id="8f5d8-291">Welche zwei Arten von routing Systeme unterscheidet, ist der Prozess, der angewendet, nachdem eine routenvorlage für die mit einer URL übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-291">What distinguishes the two types of routing systems is the process applied after a URL matches a route template.</span></span> <span data-ttu-id="8f5d8-292">In herkömmlichen routing dienen die Routenwerte aus der Übereinstimmung mit einer Nachschlagetabelle für alle Aktionen für herkömmliche gerouteten die Aktion und den Controller aus.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-292">In conventional routing, the route values from the match are used to choose the action and controller from a lookup table of all conventional routed actions.</span></span> <span data-ttu-id="8f5d8-293">Klicken Sie in der routing-Attribut, jede Vorlage ist bereits eine Aktion zugeordnet und keine weiteren Suche erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-293">In attribute routing, each template is already associated with an action, and no further lookup is needed.</span></span>

<a name=routing-url-gen-ref-label></a>

## <a name="url-generation"></a><span data-ttu-id="8f5d8-294">URL-Generierung</span><span class="sxs-lookup"><span data-stu-id="8f5d8-294">URL Generation</span></span>

<span data-ttu-id="8f5d8-295">MVC-Anwendungen können routing URL Generation Funktionen verwenden, um URL-Links, um Aktionen zu generieren.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-295">MVC applications can use routing's URL generation features to generate URL links to actions.</span></span> <span data-ttu-id="8f5d8-296">Generieren von URLs eliminiert hartcodierten URLs, ausführenden Code robuster und verwaltbar.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-296">Generating URLs eliminates hardcoding URLs, making your code more robust and maintainable.</span></span> <span data-ttu-id="8f5d8-297">Dieser Abschnitt konzentriert sich auf die URL Generation Features von MVC und befasst sich nur auf die Grundlagen der Funktionsweise der URL-Generierung.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-297">This section focuses on the URL generation features provided by MVC and will only cover basics of how URL generation works.</span></span> <span data-ttu-id="8f5d8-298">Finden Sie unter [Routing](../../fundamentals/routing.md) für eine ausführliche Beschreibung der URL-Generierung.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-298">See [Routing](../../fundamentals/routing.md) for a detailed description of URL generation.</span></span>

<span data-ttu-id="8f5d8-299">Die `IUrlHelper` Schnittstelle ist die zugrunde liegenden Teil einer Infrastruktur zwischen MVC und routing für URL-Generierung.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-299">The `IUrlHelper` interface is the underlying piece of infrastructure between MVC and routing for URL generation.</span></span> <span data-ttu-id="8f5d8-300">Finden Sie eine Instanz von `IUrlHelper` über die `Url` Eigenschaft im Controller, Ansichten und Anzeigen von Komponenten.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-300">You'll find an instance of `IUrlHelper` available through the `Url` property in controllers, views, and view components.</span></span>

<span data-ttu-id="8f5d8-301">In diesem Beispiel wird die `IUrlHelper` Schnittstelle wird verwendet, über die `Controller.Url` Eigenschaft zum Generieren einer URL an eine andere Aktion.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-301">In this example, the `IUrlHelper` interface is used through the `Controller.Url` property to generate a URL to another action.</span></span>

<span data-ttu-id="8f5d8-302">[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationController.cs?name=snippet_1)]</span><span class="sxs-lookup"><span data-stu-id="8f5d8-302">[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationController.cs?name=snippet_1)]</span></span>

<span data-ttu-id="8f5d8-303">Wenn die Anwendung Parallelität mit konventionellen standardmäßig weiterzuleiten, den Wert der `url` Variable wird die Zeichenfolge für den URL-Pfad sein `/UrlGeneration/Destination`.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-303">If the application is using the default conventional route, the value of the `url` variable will be the URL path string `/UrlGeneration/Destination`.</span></span> <span data-ttu-id="8f5d8-304">Diese URL-Pfad wird mit den Werten, die zum Übergeben von durch die Kombination aus der aktuellen Anforderung (ambient-Werte), die Routenwerte routing erstellt `Url.Action` und ersetzen diese Werte in der routenvorlage:</span><span class="sxs-lookup"><span data-stu-id="8f5d8-304">This URL path is created by routing by combining the route values from the current request (ambient values), with the values passed to `Url.Action` and substituting those values into the route template:</span></span>

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

<span data-ttu-id="8f5d8-305">Jede Routenparameter in der routenvorlage hat sein Wert durch die passenden Namen mit den Werten und die ambient-Werte ersetzt.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-305">Each route parameter in the route template has its value substituted by matching names with the values and ambient values.</span></span> <span data-ttu-id="8f5d8-306">Ein Routenparameter, die keinen Wert kann einen Standardwert verwenden, wenn er verfügt über einen oder übersprungen werden, wenn er optional ist (wie im Fall von `id` in diesem Beispiel).</span><span class="sxs-lookup"><span data-stu-id="8f5d8-306">A route parameter that does not have a value can use a default value if it has one, or be skipped if it is optional (as in the case of `id` in this example).</span></span> <span data-ttu-id="8f5d8-307">URL-Generierung schlägt fehl, wenn alle erforderlichen Routenparameter einen entsprechenden Wert besitzt.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-307">URL generation will fail if any required route parameter doesn't have a corresponding value.</span></span> <span data-ttu-id="8f5d8-308">Sollte die URL-Generierung für eine Route ein Fehler auftritt, wird die nächste Route versucht, bis alle Routen versucht wurden wurden oder eine Übereinstimmung gefunden wird.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-308">If URL generation fails for a route, the next route is tried until all routes have been tried or a match is found.</span></span>

<span data-ttu-id="8f5d8-309">Das Beispiel `Url.Action` oben geht davon aus konventionellen routing, aber URL Generation funktioniert auf ähnliche Weise mit routing-Attribut, obwohl die Konzepte unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-309">The example of `Url.Action` above assumes conventional routing, but URL generation works similarly with attribute routing, though the concepts are different.</span></span> <span data-ttu-id="8f5d8-310">Mit herkömmlichen routing dienen zum Erweitern einer Vorlage und die Routenwerte für die Routenwerte `controller` und `action` in der Regel angezeigt werden in dieser Vorlage – dies funktioniert, da die URLs von routing gefunden wird, entsprechen einer *Konvention*.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-310">With conventional routing, the route values are used to expand a template, and the route values for `controller` and `action` usually appear in that template - this works because the URLs matched by routing adhere to a *convention*.</span></span> <span data-ttu-id="8f5d8-311">In der routing-Attribut, das Routenwerte für `controller` und `action` dürfen nicht angezeigt werden in der Vorlage – sie werden stattdessen verwendet zum Nachschlagen, welche Vorlage verwendet.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-311">In attribute routing, the route values for `controller` and `action` are not allowed to appear in the template - they are instead used to look up which template to use.</span></span>

<span data-ttu-id="8f5d8-312">In diesem Beispiel wird das routing-Attribut verwendet:</span><span class="sxs-lookup"><span data-stu-id="8f5d8-312">This example uses attribute routing:</span></span>

<span data-ttu-id="8f5d8-313">[!code-csharp[Main](routing/sample/main/StartupUseMvc.cs?name=snippet_1)]</span><span class="sxs-lookup"><span data-stu-id="8f5d8-313">[!code-csharp[Main](routing/sample/main/StartupUseMvc.cs?name=snippet_1)]</span></span>

<span data-ttu-id="8f5d8-314">[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]</span><span class="sxs-lookup"><span data-stu-id="8f5d8-314">[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]</span></span>

<span data-ttu-id="8f5d8-315">MVC erstellt eine Nachschlagetabelle Verwaltungsaktionen Attribut weitergeleitet und entspricht der `controller` und `action` Werte auf die routenvorlage für URL-Generierung verwendet.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-315">MVC builds a lookup table of all attribute routed actions and will match the `controller` and `action` values to select the route template to use for URL generation.</span></span> <span data-ttu-id="8f5d8-316">Im obigen Beispiel `custom/url/to/destination` generiert wird.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-316">In the sample above,   `custom/url/to/destination` is generated.</span></span>

### <a name="generating-urls-by-action-name"></a><span data-ttu-id="8f5d8-317">Generieren von URLs durch den Namen der Aktion</span><span class="sxs-lookup"><span data-stu-id="8f5d8-317">Generating URLs by action name</span></span>

<span data-ttu-id="8f5d8-318">`Url.Action` (`IUrlHelper` .</span><span class="sxs-lookup"><span data-stu-id="8f5d8-318">`Url.Action` (`IUrlHelper` .</span></span> <span data-ttu-id="8f5d8-319">`Action`) und alle zugehörigen Überladungen alle auf diesem Konzept basieren, dass Sie angeben, was Sie durch Angabe eines Controllernamen und Aktionsnamen die Verknüpfung erstellen möchten.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-319">`Action`) and all related overloads all are based on that idea that you want to specify what you're linking to by specifying a controller name and action name.</span></span>

> [!NOTE]
> <span data-ttu-id="8f5d8-320">Bei Verwendung `Url.Action`, aktuelle Route Werte für `controller` und `action` sind für Sie – der Wert der angegebenen `controller` und `action` sind Teil der beiden *ambient Werte* **und** *Werte*.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-320">When using `Url.Action`, the current route values for `controller` and `action` are specified for you - the value of `controller` and `action` are part of both *ambient values* **and** *values*.</span></span> <span data-ttu-id="8f5d8-321">Die Methode `Url.Action`, verwendet immer die aktuellen Werte von `action` und `controller` und generiert einen URL-Pfad, die an die aktuelle Aktion weiterleitet.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-321">The method `Url.Action`, always uses the current values of `action` and `controller` and will generate a URL path that routes to the current action.</span></span>

<span data-ttu-id="8f5d8-322">Versuche, verwenden Sie die Werte in der ambient-Werte Informationen ausfüllen, die Sie bereitgestellt haben, während der Generierung eine URL-Routing.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-322">Routing attempts to use the values in ambient values to fill in information that you didn't provide when generating a URL.</span></span> <span data-ttu-id="8f5d8-323">Verwenden eine Route wie `{a}/{b}/{c}/{d}` und ambient Werte `{ a = Alice, b = Bob, c = Carol, d = David }`, routing verfügt über genügend Informationen zum Generieren einer URL ohne zusätzliche Werte – da alle weiterleiten Parameter einen Wert aufweisen.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-323">Using a route like `{a}/{b}/{c}/{d}` and ambient values `{ a = Alice, b = Bob, c = Carol, d = David }`, routing has enough information to generate a URL without any additional values - since all route parameters have a value.</span></span> <span data-ttu-id="8f5d8-324">Wenn Sie den Wert hinzugefügt `{ d = Donovan }`, den Wert `{ d = David }` würde ignoriert werden, und die generierte URL-Pfad wäre `Alice/Bob/Carol/Donovan`.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-324">If you added the value `{ d = Donovan }`, the value `{ d = David }` would be ignored, and the generated URL path would be `Alice/Bob/Carol/Donovan`.</span></span>

> [!WARNING]
> <span data-ttu-id="8f5d8-325">URL-Pfade sind hierarchisch.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-325">URL paths are hierarchical.</span></span> <span data-ttu-id="8f5d8-326">Im obigen Beispiel wird den Wert hinzufügen `{ c = Cheryl }`, beide Werte `{ c = Carol, d = David }` würde ignoriert werden.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-326">In the example above, if you added the value `{ c = Cheryl }`, both of the values `{ c = Carol, d = David }` would be ignored.</span></span> <span data-ttu-id="8f5d8-327">In diesem Fall haben wir mehr einen Wert für `d` und URL-Generierung schlägt fehl.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-327">In this case we no longer have a value for `d` and URL generation will fail.</span></span> <span data-ttu-id="8f5d8-328">Sie müssen den gewünschten Wert angeben `c` und `d`.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-328">You would need to specify the desired value of `c` and `d`.</span></span>  <span data-ttu-id="8f5d8-329">Sie erwarten wahrscheinlich, dieses Problem mit der Standardroute erreicht werden soll (`{controller}/{action}/{id?}`)- jedoch selten tritt dieses Verhalten in der Praxis als `Url.Action` immer explizit anzugeben, wird eine `controller` und `action` Wert.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-329">You might expect to hit this problem with the default route (`{controller}/{action}/{id?}`) - but you will rarely encounter this behavior in practice as `Url.Action` will always explicitly specify a `controller` and `action` value.</span></span>

<span data-ttu-id="8f5d8-330">Mehr Überladungen der `Url.Action` akzeptieren auch ein zusätzliches *Routenwerte* Werte außer für Routenparameter zu verwendendes Objekt `controller` und `action`.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-330">Longer overloads of `Url.Action` also take an additional *route values* object to provide values for route parameters other than `controller` and `action`.</span></span> <span data-ttu-id="8f5d8-331">Sie werden am häufigsten finden Sie in diesem mit verwendet `id` wie `Url.Action("Buy", "Products", new { id = 17 })`.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-331">You will most commonly see this used with `id` like `Url.Action("Buy", "Products", new { id = 17 })`.</span></span> <span data-ttu-id="8f5d8-332">Gemäß der Konvention der *Routenwerte* Objekt stellt normalerweise ein Objekt des anonymen Typs, aber es kann auch ein `IDictionary<>` oder eine *einfaches altes .NET Objekt*.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-332">By convention the *route values* object is usually an object of anonymous type, but it can also be an `IDictionary<>` or a *plain old .NET object*.</span></span> <span data-ttu-id="8f5d8-333">Alle zusätzlichen Routenwerte, die Routenparameter nicht übereinstimmen, werden in der Abfragezeichenfolge platziert.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-333">Any additional route values that don't match route parameters are put in the query string.</span></span>

<span data-ttu-id="8f5d8-334">[!code-csharp[Main](routing/sample/main/Controllers/TestController.cs)]</span><span class="sxs-lookup"><span data-stu-id="8f5d8-334">[!code-csharp[Main](routing/sample/main/Controllers/TestController.cs)]</span></span>

> [!TIP]
> <span data-ttu-id="8f5d8-335">Um eine absolute URL zu erstellen, verwenden Sie eine Überladung, akzeptiert eine `protocol`:`Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span><span class="sxs-lookup"><span data-stu-id="8f5d8-335">To create an absolute URL, use an overload that accepts a `protocol`: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span></span>

<a name=routing-gen-urls-route-ref-label></a>

### <a name="generating-urls-by-route"></a><span data-ttu-id="8f5d8-336">Generieren von URLs von route</span><span class="sxs-lookup"><span data-stu-id="8f5d8-336">Generating URLs by route</span></span>

<span data-ttu-id="8f5d8-337">Der obige Code veranschaulicht das Generieren einer URL durch die Übergabe der Controller- und Name.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-337">The code above demonstrated generating a URL by passing in the controller and action name.</span></span> <span data-ttu-id="8f5d8-338">`IUrlHelper`bietet außerdem die `Url.RouteUrl` -Methodenfamilie.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-338">`IUrlHelper` also provides the `Url.RouteUrl` family of methods.</span></span> <span data-ttu-id="8f5d8-339">Diese Methoden ähneln `Url.Action`, aber nicht kopieren sie die aktuellen Werte von `action` und `controller` , die Routenwerte.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-339">These methods are similar to `Url.Action`, but they do not copy the current values of `action` and `controller` to the route values.</span></span> <span data-ttu-id="8f5d8-340">Die häufigste Verwendung ist die Angabe von einem Routennamen an eine Route zu verwenden, um die URL in der Regel generieren *ohne* Angabe eines Namens Controller bzw. die Aktionsmethode.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-340">The most common usage is to specify a route name to use a specific route to generate the URL, generally *without* specifying a controller or action name.</span></span>

<span data-ttu-id="8f5d8-341">[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]</span><span class="sxs-lookup"><span data-stu-id="8f5d8-341">[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]</span></span>

<a name=routing-gen-urls-html-ref-label></a>

### <a name="generating-urls-in-html"></a><span data-ttu-id="8f5d8-342">Generieren von URLs in HTML</span><span class="sxs-lookup"><span data-stu-id="8f5d8-342">Generating URLs in HTML</span></span>

<span data-ttu-id="8f5d8-343">`IHtmlHelper`Stellt die `HtmlHelper` Methoden `Html.BeginForm` und `Html.ActionLink` generieren `<form>` und `<a>` Elemente bzw..</span><span class="sxs-lookup"><span data-stu-id="8f5d8-343">`IHtmlHelper` provides the `HtmlHelper` methods `Html.BeginForm` and `Html.ActionLink` to generate `<form>` and `<a>` elements respectively.</span></span> <span data-ttu-id="8f5d8-344">Verwenden Sie diese Methoden die `Url.Action` Methode, um eine URL zu generieren und ähnliche Argumente akzeptieren.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-344">These methods use the `Url.Action` method to generate a URL and they accept similar arguments.</span></span> <span data-ttu-id="8f5d8-345">Die `Url.RouteUrl` Begleitgeräte für `HtmlHelper` sind `Html.BeginRouteForm` und `Html.RouteLink` die ähnliche Funktionen aufweisen.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-345">The `Url.RouteUrl` companions for `HtmlHelper` are `Html.BeginRouteForm` and `Html.RouteLink` which have similar functionality.</span></span>

<span data-ttu-id="8f5d8-346">TagHelpers Generieren von URLs mit dem `form` taghelpers und die `<a>` taghelpers..</span><span class="sxs-lookup"><span data-stu-id="8f5d8-346">TagHelpers generate URLs through the `form` TagHelper and the `<a>` TagHelper.</span></span> <span data-ttu-id="8f5d8-347">Verwenden Sie diese beiden `IUrlHelper` für ihre Implementierung.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-347">Both of these use `IUrlHelper` for their implementation.</span></span> <span data-ttu-id="8f5d8-348">Finden Sie unter [arbeiten mit Formularen](../views/working-with-forms.md) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-348">See [Working with Forms](../views/working-with-forms.md) for more information.</span></span>

<span data-ttu-id="8f5d8-349">In Ansichten die `IUrlHelper` kann über die `Url` -Eigenschaft für alle Ad-hoc-URL-Generierung, die nicht von den oben genannten abgedeckt.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-349">Inside views, the `IUrlHelper` is available through the `Url` property for any ad-hoc URL generation not covered by the above.</span></span>

<a name=routing-gen-urls-action-ref-label></a>

### <a name="generating-urls-in-action-results"></a><span data-ttu-id="8f5d8-350">Generieren von URLS in Aktionsergebnisse</span><span class="sxs-lookup"><span data-stu-id="8f5d8-350">Generating URLS in Action Results</span></span>

<span data-ttu-id="8f5d8-351">Die obigen Beispiele haben gezeigt, mithilfe von `IUrlHelper` in einem Controller, während die häufigste Verwendung in einem Controller zum Generieren einer URL im Rahmen des ein Aktionsergebnis wird.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-351">The examples above have shown using `IUrlHelper` in a controller, while the most common usage in a controller is to generate a URL as part of an action result.</span></span>

<span data-ttu-id="8f5d8-352">Die `ControllerBase` und `Controller` Basisklassen bieten Hilfsmethoden für Aktionsergebnisse, die auf eine andere Aktion verweisen.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-352">The `ControllerBase` and `Controller` base classes provide convenience methods for action results that reference another action.</span></span> <span data-ttu-id="8f5d8-353">Eine typische Verwendung besteht darin, leiten nach der Benutzereingaben akzeptieren.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-353">One typical usage is to redirect after accepting user input.</span></span>

```csharp
public Task<IActionResult> Edit(int id, Customer customer)
{
    if (ModelState.IsValid)
    {
        // Update DB with new details.
        return RedirectToAction("Index");
    }
}
```

<span data-ttu-id="8f5d8-354">Die Aktion Ergebnisse Factorymethoden folgen einem ähnlichen Muster auf die Methoden auf `IUrlHelper`.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-354">The action results factory methods follow a similar pattern to the methods on `IUrlHelper`.</span></span>

<a name=routing-dedicated-ref-label></a>

### <a name="special-case-for-dedicated-conventional-routes"></a><span data-ttu-id="8f5d8-355">Sonderfall für dedizierte konventionellen Routen</span><span class="sxs-lookup"><span data-stu-id="8f5d8-355">Special case for dedicated conventional routes</span></span>

<span data-ttu-id="8f5d8-356">Herkömmliche routing können Sie eine spezielle Art von Route-Definition bezeichnet eine *dedizierten konventionellen Route*.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-356">Conventional routing can use a special kind of route definition called a *dedicated conventional route*.</span></span> <span data-ttu-id="8f5d8-357">Im folgenden Beispiel wird die Route mit dem Namen `blog` ist eine dedizierte konventionelle Route.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-357">In the example below, the route named `blog` is a dedicated conventional route.</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="8f5d8-358">Verwenden diese Route-Definitionen `Url.Action("Index", "Home")` generiert den URL-Pfad `/` mit der `default` Route, aber nicht, warum?</span><span class="sxs-lookup"><span data-stu-id="8f5d8-358">Using these route definitions, `Url.Action("Index", "Home")` will generate the URL path `/` with the `default` route, but why?</span></span> <span data-ttu-id="8f5d8-359">Können Sie erraten, die Routenwerte `{ controller = Home, action = Index }` wäre ausreichend, um eine URL generiert die Verwendung `blog`, und das Ergebnis wäre `/blog?action=Index&controller=Home`.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-359">You might guess the route values `{ controller = Home, action = Index }` would be enough to generate a URL using `blog`, and the result would be `/blog?action=Index&controller=Home`.</span></span>

<span data-ttu-id="8f5d8-360">Dedizierte konventionelle Routen abhängig ist, ein spezielles Verhalten von Standardwerten, die einen entsprechenden Routenparameter besitzen, die verhindert, dass die Route wird "zu gieriger" mit URL-Generierung.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-360">Dedicated conventional routes rely on a special behavior of default values that don't have a corresponding route parameter that prevents the route from being "too greedy" with URL generation.</span></span> <span data-ttu-id="8f5d8-361">In diesem Fall die Standardwerte sind `{ controller = Blog, action = Article }`, und weder `controller` noch `action` wird als ein Routenparameter.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-361">In this case the default values are `{ controller = Blog, action = Article }`, and neither `controller` nor `action` appears as a route parameter.</span></span> <span data-ttu-id="8f5d8-362">Wenn routing URL-Generierung ausführt, müssen die angegebenen Werte die Standardwerte übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-362">When routing performs URL generation, the values provided must match the default values.</span></span> <span data-ttu-id="8f5d8-363">Mithilfe von URL-Generierung `blog` schlägt fehl, da die Werte `{ controller = Home, action = Index }` stimmen nicht überein. `{ controller = Blog, action = Article }`.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-363">URL generation using `blog` will fail because the values `{ controller = Home, action = Index }` don't match `{ controller = Blog, action = Article }`.</span></span> <span data-ttu-id="8f5d8-364">Routing dann ausgewichen, um zu versuchen `default`, die erfolgreich ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-364">Routing then falls back to try `default`, which succeeds.</span></span>

<a name=routing-areas-ref-label></a>

## <a name="areas"></a><span data-ttu-id="8f5d8-365">Bereiche</span><span class="sxs-lookup"><span data-stu-id="8f5d8-365">Areas</span></span>

<span data-ttu-id="8f5d8-366">[Bereiche](areas.md) eine MVC-Funktion, die zum Organisieren von verwandten Funktionen in einer Gruppe als eine separate routing-Namespace (für Controlleraktionen) und Ordnerstruktur (Views) verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-366">[Areas](areas.md) are an MVC feature used to organize related functionality into a group as a separate routing-namespace (for controller actions) and folder structure (for views).</span></span> <span data-ttu-id="8f5d8-367">Ermöglicht es einer Anwendung mehrere Domänencontroller mit dem gleichen Namen – solange sie verschiedene sind mithilfe von Bereichen *Bereiche*.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-367">Using areas allows an application to have multiple controllers with the same name - as long as they have different *areas*.</span></span> <span data-ttu-id="8f5d8-368">Mithilfe von Bereichen erstellt eine Hierarchie für die Weiterleitung und Hinzufügen von einem anderen Routenparameter, `area` auf `controller` und `action`.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-368">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area` to `controller` and `action`.</span></span> <span data-ttu-id="8f5d8-369">In diesem Abschnitt wird erläutert, wie routing Bereiche - interagiert, finden Sie unter [Bereiche](areas.md) Weitere Informationen zur Verwendung von Bereichen mit Ansichten.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-369">This section will discuss how routing interacts with areas - see [Areas](areas.md) for details about how areas are used with views.</span></span>

<span data-ttu-id="8f5d8-370">Das folgende Beispiel konfiguriert die MVC, um die Standardroute konventionellen verwenden und ein *Bereich Route* für einen Bereich mit dem Namen `Blog`:</span><span class="sxs-lookup"><span data-stu-id="8f5d8-370">The following example configures MVC to use the default conventional route and an *area route* for an area named `Blog`:</span></span>

<span data-ttu-id="8f5d8-371">[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="8f5d8-371">[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet1)]</span></span>

<span data-ttu-id="8f5d8-372">Beim Abgleich von URL-Pfad wie `/Manage/Users/AddUser`, erzeugt die erste Route die Routenwerte `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-372">When matching a URL path like `/Manage/Users/AddUser`, the first route will produce the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span> <span data-ttu-id="8f5d8-373">Die `area` routenwert wird erzeugt, indem ein Standardwert für `area`, in der Tat die Route erstellt, indem `MapAreaRoute` entspricht der folgenden:</span><span class="sxs-lookup"><span data-stu-id="8f5d8-373">The `area` route value is produced by a default value for `area`, in fact the route created by `MapAreaRoute` is equivalent to the following:</span></span>

<span data-ttu-id="8f5d8-374">[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="8f5d8-374">[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet2)]</span></span>

<span data-ttu-id="8f5d8-375">`MapAreaRoute`erstellt eine Route mit einem Standardwert und die Einschränkung für `area` verwenden den angegebenen Bereichsnamen in diesem Fall `Blog`.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-375">`MapAreaRoute` creates a route using both a default value and constraint for `area` using the provided area name, in this case `Blog`.</span></span> <span data-ttu-id="8f5d8-376">Der Standardwert wird sichergestellt, dass die Route immer erzeugt `{ area = Blog, ... }`, die Einschränkung muss den Wert `{ area = Blog, ... }` für URL-Generierung.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-376">The default value ensures that the route always produces `{ area = Blog, ... }`, the constraint requires the value `{ area = Blog, ... }` for URL generation.</span></span>

> [!TIP]
> <span data-ttu-id="8f5d8-377">Herkömmliche routing ist die Reihenfolge wichtig.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-377">Conventional routing is order-dependent.</span></span> <span data-ttu-id="8f5d8-378">Im Allgemeinen sollten Routen mit Bereichen weiter oben in der Routingtabelle platziert werden, wie sie mehr als Routen, ohne einen Bereich spezifisch sind.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-378">In general, routes with areas should be placed earlier in the route table as they are more specific than routes without an area.</span></span>

<span data-ttu-id="8f5d8-379">Verwenden das oben genannte Beispiel an, würde die Routenwerte die folgende Aktion übereinstimmen:</span><span class="sxs-lookup"><span data-stu-id="8f5d8-379">Using the above example, the route values would match the following action:</span></span>

<span data-ttu-id="8f5d8-380">[!code-csharp[Main](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]</span><span class="sxs-lookup"><span data-stu-id="8f5d8-380">[!code-csharp[Main](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]</span></span>

<span data-ttu-id="8f5d8-381">Die `AreaAttribute` ist, was einen Controller kennzeichnet im Rahmen eines Bereichs, sagen wir, dass in diesem Controller ist der `Blog` Bereich.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-381">The `AreaAttribute` is what denotes a controller as part of an area, we say that this controller is in the `Blog` area.</span></span> <span data-ttu-id="8f5d8-382">Domänencontroller ohne eine `[Area]` Attribut sind keine Mitglieder der jeder Bereich und werden **nicht** übereinstimmen, wenn die `area` Route wird von routing bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-382">Controllers without an `[Area]` attribute are not members of any area, and will **not** match when the `area` route value is provided by routing.</span></span> <span data-ttu-id="8f5d8-383">Im folgenden Beispiel kann nur der erste Controller aufgeführten entsprechen, die Routenwerte `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-383">In the following example, only the first controller listed can match the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span>

<span data-ttu-id="8f5d8-384">[!code-csharp[Main](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]</span><span class="sxs-lookup"><span data-stu-id="8f5d8-384">[!code-csharp[Main](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]</span></span>

<span data-ttu-id="8f5d8-385">[!code-csharp[Main](routing/sample/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]</span><span class="sxs-lookup"><span data-stu-id="8f5d8-385">[!code-csharp[Main](routing/sample/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]</span></span>

<span data-ttu-id="8f5d8-386">[!code-csharp[Main](routing/sample/AreasRouting/Controllers/UsersController.cs)]</span><span class="sxs-lookup"><span data-stu-id="8f5d8-386">[!code-csharp[Main](routing/sample/AreasRouting/Controllers/UsersController.cs)]</span></span>

> [!NOTE]
> <span data-ttu-id="8f5d8-387">Der Namespace der jedem Controller ist aus Gründen der Vollständigkeit im folgenden dargestellt: Andernfalls würde die Controller eine Benennung von in Konflikt stehen und ein Compilerfehler generiert haben.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-387">The namespace of each controller is shown here for completeness - otherwise the controllers would have a naming conflict and generate a compiler error.</span></span> <span data-ttu-id="8f5d8-388">Klasse Namespaces haben keine Auswirkungen auf MVCs-routing.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-388">Class namespaces have no effect on MVC's routing.</span></span>

<span data-ttu-id="8f5d8-389">Die ersten beiden Controller sind Elemente der Bereiche, und nur überein, wenn von ihren jeweiligen Bereichsname bereitgestellt wird der `area` Wert weiterzuleiten.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-389">The first two controllers are members of areas, and only match when their respective area name is provided by the `area` route value.</span></span> <span data-ttu-id="8f5d8-390">Der dritte Controller ist kein Mitglied eines beliebigen Bereich und die einzige Übereinstimmung kann, wenn kein Wert für `area` von routing bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-390">The third controller is not a member of any area, and can only match when no value for `area` is provided by routing.</span></span>

> [!NOTE]
> <span data-ttu-id="8f5d8-391">Im Hinblick auf Übereinstimmung *kein Wert*, Abwesenheit der `area` Wert ist gleich als wäre der Wert für `area` waren Null oder eine leere Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-391">In terms of matching *no value*, the absence of the `area` value is the same as if the value for `area` were null or the empty string.</span></span>

<span data-ttu-id="8f5d8-392">Bei der Ausführung einer Aktion innerhalb eines Bereichs-Wert die Route für `area` als verfügbar ein *ambient Wert* für das routing für die Verwendung für URL-Generierung.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-392">When executing an action inside an area, the route value for `area` will be available as an *ambient value* for routing to use for URL generation.</span></span> <span data-ttu-id="8f5d8-393">Dies bedeutet, dass standardmäßig Bereiche agieren *persistente* für URL-Generierung, wie im folgenden Beispiel gezeigt.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-393">This means that by default areas act *sticky* for URL generation as demonstrated by the following sample.</span></span>

<span data-ttu-id="8f5d8-394">[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet3)]</span><span class="sxs-lookup"><span data-stu-id="8f5d8-394">[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet3)]</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {"linenostart": 1}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#", "source": "/Users/shirhatti/src/Docs/aspnet/mvc/controllers/routing/sample/AreasRouting/Areas/Duck/Controllers/UsersController.cs"} -->

<span data-ttu-id="8f5d8-395">[!code-csharp[Main](routing/sample/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]</span><span class="sxs-lookup"><span data-stu-id="8f5d8-395">[!code-csharp[Main](routing/sample/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]</span></span>

<a name=iactionconstraint-ref-label></a>

## <a name="understanding-iactionconstraint"></a><span data-ttu-id="8f5d8-396">Grundlegendes zu IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="8f5d8-396">Understanding IActionConstraint</span></span>

> [!NOTE]
> <span data-ttu-id="8f5d8-397">Dieser Abschnitt ist eine tief greifende Framework Mechanismen und wie eine Aktion zum Ausführen von MVC auswählt.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-397">This section is a deep-dive on framework internals and how MVC chooses an action to execute.</span></span> <span data-ttu-id="8f5d8-398">Eine typische Anwendung erforderlich keinen benutzerdefinierten.`IActionConstraint`</span><span class="sxs-lookup"><span data-stu-id="8f5d8-398">A typical application won't need a custom `IActionConstraint`</span></span>

<span data-ttu-id="8f5d8-399">Sie haben wahrscheinlich bereits verwendet `IActionConstraint` , auch wenn Sie nicht mit der Oberfläche vertraut sind.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-399">You have likely already used `IActionConstraint` even if you're not familiar with the interface.</span></span> <span data-ttu-id="8f5d8-400">Die `[HttpGet]` Attribut und ähnliche `[Http-VERB]` Attribute implementieren `IActionConstraint` um die Ausführung einer Aktionsmethode einzuschränken.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-400">The `[HttpGet]` Attribute and similar `[Http-VERB]` attributes implement `IActionConstraint` in order to limit the execution of an action method.</span></span>

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

<span data-ttu-id="8f5d8-401">Vorausgesetzt, die herkömmliche Standardroute des URL-Pfads `/Products/Edit` wären die Werte `{ controller = Products, action = Edit }`, entsprechen die **beide** der hier gezeigten Aktionen.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-401">Assuming the default conventional route, the URL path `/Products/Edit` would produce the values `{ controller = Products, action = Edit }`, which would match **both** of the actions shown here.</span></span> <span data-ttu-id="8f5d8-402">In `IActionConstraint` Terminologie, die wir sagen würden, dass beide der genannten Aktionen Kandidaten - berücksichtigt werden, da beide die Routendaten übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-402">In `IActionConstraint` terminology we would say that both of these actions are considered candidates - as they both match the route data.</span></span>

<span data-ttu-id="8f5d8-403">Wenn die `HttpGetAttribute` ausgeführt wird, es wird angegeben, die *Edit()* wird eine Übereinstimmung für *abrufen* und nicht nach Übereinstimmungen für alle anderen HTTP-Verb wird.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-403">When the `HttpGetAttribute` executes, it will say that *Edit()* is a match for *GET* and is not a match for any other HTTP verb.</span></span> <span data-ttu-id="8f5d8-404">Die `Edit(...)` Aktion verfügt nicht über alle definierten Einschränkungen und daher HTTP-Verb entspricht.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-404">The `Edit(...)` action doesn't have any constraints defined, and so will match any HTTP verb.</span></span> <span data-ttu-id="8f5d8-405">Vorausgesetzt, sodass eine `POST` – nur `Edit(...)` übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-405">So assuming a `POST` - only `Edit(...)` matches.</span></span> <span data-ttu-id="8f5d8-406">Aber für eine `GET` beide Aktionen können dennoch übereinstimmen - jedoch eine Aktion mit einer `IActionConstraint` gilt immer *besser* als eine Aktion ohne.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-406">But, for a `GET` both actions can still match - however, an action with an `IActionConstraint` is always considered *better* than an action without.</span></span> <span data-ttu-id="8f5d8-407">Daher da `Edit()` hat `[HttpGet]` sie spezifischere gilt und wird ausgewählt werden, wenn beide Aktionen verglichen werden können.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-407">So because `Edit()` has `[HttpGet]` it is considered more specific, and will be selected if both actions can match.</span></span>

<span data-ttu-id="8f5d8-408">Im Prinzip `IActionConstraint` ist eine Form der *überladen*, aber statt Überladen von Methoden mit demselben Namen, ist es überladen zwischen Aktionen, die die gleiche URL entsprechen.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-408">Conceptually, `IActionConstraint` is a form of *overloading*, but instead of overloading methods with the same name, it is overloading between actions that match the same URL.</span></span> <span data-ttu-id="8f5d8-409">Routing-Attribut verwendet auch `IActionConstraint` und kann dazu führen, Aktionen, die von verschiedenen Controllern beide vermerkt Kandidaten.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-409">Attribute routing also uses `IActionConstraint` and can result in actions from different controllers both being considered candidates.</span></span>

<a name=iactionconstraint-impl-ref-label></a>

### <a name="implementing-iactionconstraint"></a><span data-ttu-id="8f5d8-410">Implementieren von IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="8f5d8-410">Implementing IActionConstraint</span></span>

<span data-ttu-id="8f5d8-411">Die einfachste Methode zum Implementieren einer `IActionConstraint` zum Erstellen einer Klasse abgeleitet ist `System.Attribute` und platzieren Sie es auf Ihre Aktionen und Controllern aus.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-411">The simplest way to implement an `IActionConstraint` is to create a class derived from `System.Attribute` and place it on your actions and controllers.</span></span> <span data-ttu-id="8f5d8-412">MVC erkennt automatisch `IActionConstraint` , die als Attribute angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-412">MVC will automatically discover any `IActionConstraint` that are applied as attributes.</span></span> <span data-ttu-id="8f5d8-413">Sie verwenden das Anwendungsmodell, um Einschränkungen anzuwenden, und dies ist wahrscheinlich die flexibelste Herangehensweise, da Sie zu Metaprogram hinauszögern wie sie angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-413">You can use the application model to apply constraints, and this is probably the most flexible approach as it allows you to metaprogram how they are applied.</span></span>

<span data-ttu-id="8f5d8-414">Im folgenden Beispiel wird eine Einschränkung wählt eine Aktion basierend auf einer *Landeskennzahl* von den Routendaten.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-414">In the following example a constraint chooses an action based on a *country code* from the route data.</span></span> <span data-ttu-id="8f5d8-415">Die [vollständige Sample auf GitHub](https://github.com/aspnet/Entropy/blob/dev/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span><span class="sxs-lookup"><span data-stu-id="8f5d8-415">The [full sample on GitHub](https://github.com/aspnet/Entropy/blob/dev/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span></span>

```csharp
public class CountrySpecificAttribute : Attribute, IActionConstraint
{
    private readonly string _countryCode;

    public CountrySpecificAttribute(string countryCode)
    {
        _countryCode = countryCode;
    }

    public int Order
    {
        get
        {
            return 0;
        }
    }

    public bool Accept(ActionConstraintContext context)
    {
        return string.Equals(
            context.RouteContext.RouteData.Values["country"].ToString(),
            _countryCode,
            StringComparison.OrdinalIgnoreCase);
    }
}
```

<span data-ttu-id="8f5d8-416">Sie sind verantwortlich für die Implementierung der `Accept` -Methode und eine "Order" für die Einschränkung auszuführende auswählen.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-416">You are responsible for implementing the `Accept` method and choosing an 'Order' for the constraint to execute.</span></span> <span data-ttu-id="8f5d8-417">In diesem Fall die `Accept` -Methode zurückkehrt `true` zu kennzeichnen, die Aktion ist eine Übereinstimmung bei der `country` route entspricht.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-417">In this case, the `Accept` method returns `true` to denote the action is a match when the `country` route value matches.</span></span> <span data-ttu-id="8f5d8-418">Dies unterscheidet sich von einem `RouteValueAttribute` dahingehend, dass fallback auf eine Aktion nicht mit Attributen versehen können.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-418">This is different from a `RouteValueAttribute` in that it allows fallback to a non-attributed action.</span></span> <span data-ttu-id="8f5d8-419">Das Beispiel zeigt, dass, wenn Sie definieren eine `en-US` Aktion und dann einen Ländercode wie `fr-FR` ausweichen auf einen generischeren Controller, der keinen `[CountrySpecific(...)]` angewendet.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-419">The sample shows that if you define an `en-US` action then a country code like `fr-FR` will fall back to a more generic controller that does not have `[CountrySpecific(...)]` applied.</span></span>

<span data-ttu-id="8f5d8-420">Die `Order` Eigenschaft entscheidet, in welchen *Phase* die Einschränkung gehört.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-420">The `Order` property decides which *stage* the constraint is part of.</span></span> <span data-ttu-id="8f5d8-421">Aktionseinschränkungen führen Sie in Gruppen auf Grundlage der `Order`.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-421">Action constraints run in groups based on the `Order`.</span></span> <span data-ttu-id="8f5d8-422">Z. B. alle das Framework bereitgestellt, verwenden Sie HTTP-Method-Attribute den gleichen `Order` Wert, sodass sie in der gleichen Stufe ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-422">For example, all of the framework provided HTTP method attributes use the same `Order` value so that they run in the same stage.</span></span> <span data-ttu-id="8f5d8-423">Sie können beliebig viele Stufen Sie die gewünschten Richtlinien implementieren müssen.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-423">You can have as many stages as you need to implement your desired policies.</span></span>

> [!TIP]
> <span data-ttu-id="8f5d8-424">Um zu entscheiden, auf einen Wert für `Order` überlegen, ob die Einschränkung vor HTTP-Methoden angewendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-424">To decide on a value for `Order` think about whether or not your constraint should be applied before HTTP methods.</span></span> <span data-ttu-id="8f5d8-425">Niedrigere Zahlen werden zuerst ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-425">Lower numbers run first.</span></span>
