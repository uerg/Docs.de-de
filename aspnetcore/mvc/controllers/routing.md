---
title: Routing zum Controlleraktionen
author: rick-anderson
description: 
ms.author: riande
manager: wpickett
ms.date: 03/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/routing
ms.openlocfilehash: 497ce47fa567f163cb7b1eb891408f0100d15b8a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
# <a name="routing-to-controller-actions"></a><span data-ttu-id="a6616-102">Routing zum Controlleraktionen</span><span class="sxs-lookup"><span data-stu-id="a6616-102">Routing to Controller Actions</span></span>

<span data-ttu-id="a6616-103">Durch [Ryan Nowak](https://github.com/rynowak) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a6616-103">By [Ryan Nowak](https://github.com/rynowak) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a6616-104">ASP.NET Core MVC verwendet Routing [Middleware](../../fundamentals/middleware.md) die URLs der eingehende Anforderungen entsprechen und Aktionen zuordnen.</span><span class="sxs-lookup"><span data-stu-id="a6616-104">ASP.NET Core MVC uses the Routing [middleware](../../fundamentals/middleware.md) to match the URLs of incoming requests and map them to actions.</span></span> <span data-ttu-id="a6616-105">Routen werden in der Startcode oder Attribute definiert.</span><span class="sxs-lookup"><span data-stu-id="a6616-105">Routes are defined in startup code or attributes.</span></span> <span data-ttu-id="a6616-106">Routen wird beschrieben, wie URL-Pfade auf Aktionen abgeglichen werden sollen.</span><span class="sxs-lookup"><span data-stu-id="a6616-106">Routes describe how URL paths should be matched to actions.</span></span> <span data-ttu-id="a6616-107">Routen werden auch verwendet, zum Generieren von URLs (für Links), die in Antworten gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="a6616-107">Routes are also used to generate URLs (for links) sent out in responses.</span></span> 

<span data-ttu-id="a6616-108">Aktionen werden konventionell entweder weitergeleitet oder Attribut weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="a6616-108">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="a6616-109">Platzieren eine Route auf dem Controller oder die Aktion erleichtert Attribut weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="a6616-109">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="a6616-110">Finden Sie unter [gemischten routing](#routing-mixed-ref-label) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="a6616-110">See [Mixed routing](#routing-mixed-ref-label) for more information.</span></span>

<span data-ttu-id="a6616-111">Dieses Dokument wird erläutert, die Interaktionen zwischen MVC und routing und wie normale MVC-apps stellen routing-Funktionen.</span><span class="sxs-lookup"><span data-stu-id="a6616-111">This document will explain the interactions between MVC and routing, and how typical MVC apps make use of routing features.</span></span> <span data-ttu-id="a6616-112">Finden Sie unter [Routing](xref:fundamentals/routing) Informationen zu erweiterten routing.</span><span class="sxs-lookup"><span data-stu-id="a6616-112">See [Routing](xref:fundamentals/routing) for details on advanced routing.</span></span>

## <a name="setting-up-routing-middleware"></a><span data-ttu-id="a6616-113">Einrichten von Middleware Routing</span><span class="sxs-lookup"><span data-stu-id="a6616-113">Setting up Routing Middleware</span></span>

<span data-ttu-id="a6616-114">In Ihrem *konfigurieren* Methode möglicherweise Code ähnelt:</span><span class="sxs-lookup"><span data-stu-id="a6616-114">In your *Configure* method you may see code similar to:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="a6616-115">Beim Aufruf von `UseMvc`, `MapRoute` wird verwendet, um eine einzelne Route erstellen, die wir auf als verweisen müssen die `default` Route.</span><span class="sxs-lookup"><span data-stu-id="a6616-115">Inside the call to `UseMvc`, `MapRoute` is used to create a single route, which we'll refer to as the `default` route.</span></span> <span data-ttu-id="a6616-116">Die meisten MVC-apps verwendet eine Route mit einer Vorlage ähnelt der `default` Route.</span><span class="sxs-lookup"><span data-stu-id="a6616-116">Most MVC apps will use a route with a template similar to the `default` route.</span></span>

<span data-ttu-id="a6616-117">Die routenvorlage `"{controller=Home}/{action=Index}/{id?}"` kann einen URL-Pfad, z. B. entsprechen `/Products/Details/5` und extrahiert die Routenwerte `{ controller = Products, action = Details, id = 5 }` durch den Pfad zu versehen.</span><span class="sxs-lookup"><span data-stu-id="a6616-117">The route template `"{controller=Home}/{action=Index}/{id?}"` can match a URL path like `/Products/Details/5` and will extract the route values `{ controller = Products, action = Details, id = 5 }` by tokenizing the path.</span></span> <span data-ttu-id="a6616-118">MVC versucht, einen Controller mit dem Namen suchen `ProductsController` , und führen Sie die Aktion `Details`:</span><span class="sxs-lookup"><span data-stu-id="a6616-118">MVC will attempt to locate a controller named `ProductsController` and run the action `Details`:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

<span data-ttu-id="a6616-119">Beachten Sie, dass in diesem Beispiel wurden die modellbindung den Wert der verwenden würden `id = 5` festzulegende der `id` Parameter `5` diese Aktion aufrufen.</span><span class="sxs-lookup"><span data-stu-id="a6616-119">Note that in this example, model binding would use the value of `id = 5` to set the `id` parameter to `5` when invoking this action.</span></span> <span data-ttu-id="a6616-120">Finden Sie unter der [Modell Bindung](../models/model-binding.md) Weitere Details.</span><span class="sxs-lookup"><span data-stu-id="a6616-120">See the [Model Binding](../models/model-binding.md) for more details.</span></span>

<span data-ttu-id="a6616-121">Mithilfe der `default` Route:</span><span class="sxs-lookup"><span data-stu-id="a6616-121">Using the `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="a6616-122">Die routenvorlage:</span><span class="sxs-lookup"><span data-stu-id="a6616-122">The route template:</span></span>

* <span data-ttu-id="a6616-123">`{controller=Home}`definiert `Home` als Standard`controller`</span><span class="sxs-lookup"><span data-stu-id="a6616-123">`{controller=Home}` defines `Home` as the default `controller`</span></span>

* <span data-ttu-id="a6616-124">`{action=Index}`definiert `Index` als Standard`action`</span><span class="sxs-lookup"><span data-stu-id="a6616-124">`{action=Index}` defines `Index` as the default `action`</span></span>

* <span data-ttu-id="a6616-125">`{id?}`definiert `id` als optional</span><span class="sxs-lookup"><span data-stu-id="a6616-125">`{id?}` defines `id` as optional</span></span>

<span data-ttu-id="a6616-126">Standard- und optionale Routenparameter müssen nicht in der URL-Pfad für eine Übereinstimmung vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="a6616-126">Default and optional route parameters don't need to be present in the URL path for a match.</span></span> <span data-ttu-id="a6616-127">Finden Sie unter [Route Vorlagenverweis](../../fundamentals/routing.md#route-template-reference) für eine ausführliche Beschreibung der Syntax der Route-Vorlage.</span><span class="sxs-lookup"><span data-stu-id="a6616-127">See [Route Template Reference](../../fundamentals/routing.md#route-template-reference) for a detailed description of route template syntax.</span></span>

<span data-ttu-id="a6616-128">`"{controller=Home}/{action=Index}/{id?}"`kann den URL-Pfad entsprechen `/` und die Routenwerte entsteht `{ controller = Home, action = Index }`.</span><span class="sxs-lookup"><span data-stu-id="a6616-128">`"{controller=Home}/{action=Index}/{id?}"` can match the URL path `/` and will produce the route values `{ controller = Home, action = Index }`.</span></span> <span data-ttu-id="a6616-129">Die Werte für `controller` und `action` stellen die Standardwerte verwenden `id` keine Wert erzeugt, da keine entsprechende Segment in der URL-Pfad vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="a6616-129">The values for `controller` and `action` make use of the default values, `id` doesn't produce a value since there's no corresponding segment in the URL path.</span></span> <span data-ttu-id="a6616-130">MVC würden diese Routenwerte verwenden, um Wählen Sie die `HomeController` und `Index` Aktion:</span><span class="sxs-lookup"><span data-stu-id="a6616-130">MVC would use these route values to select the `HomeController` and `Index` action:</span></span>

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

<span data-ttu-id="a6616-131">Mit diesem Controller Definition und die routenvorlage, die `HomeController.Index` Aktion wird für keines der folgenden URL-Pfade ausgeführt werden:</span><span class="sxs-lookup"><span data-stu-id="a6616-131">Using this controller definition and route template, the `HomeController.Index` action would be executed for any of the following URL paths:</span></span>

* `/Home/Index/17`

* `/Home/Index`

* `/Home`

* `/`

<span data-ttu-id="a6616-132">Die Hilfsmethode `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="a6616-132">The convenience method `UseMvcWithDefaultRoute`:</span></span>

```csharp
app.UseMvcWithDefaultRoute();
```

<span data-ttu-id="a6616-133">Kann verwendet werden, um zu ersetzen:</span><span class="sxs-lookup"><span data-stu-id="a6616-133">Can be used to replace:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="a6616-134">`UseMvc`und `UseMvcWithDefaultRoute` fügen eine Instanz des `RouterMiddleware` die Middleware-Pipeline.</span><span class="sxs-lookup"><span data-stu-id="a6616-134">`UseMvc` and `UseMvcWithDefaultRoute` add an instance of `RouterMiddleware` to the middleware pipeline.</span></span> <span data-ttu-id="a6616-135">MVC interagiert nicht direkt mit Middleware und routing verwendet, um Anforderungen zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="a6616-135">MVC doesn't interact directly with middleware, and uses routing to handle requests.</span></span> <span data-ttu-id="a6616-136">MVC verbunden ist, auf die Routen über eine Instanz des `MvcRouteHandler`.</span><span class="sxs-lookup"><span data-stu-id="a6616-136">MVC is connected to the routes through an instance of `MvcRouteHandler`.</span></span> <span data-ttu-id="a6616-137">Der Code innerhalb eines `UseMvc` ähnelt der folgenden:</span><span class="sxs-lookup"><span data-stu-id="a6616-137">The code inside of `UseMvc` is similar to the following:</span></span>

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

<span data-ttu-id="a6616-138">`UseMvc`Alle Routen nicht direkt definieren die routenauflistung für einen Platzhalter hinzugefügt der `attribute` Route.</span><span class="sxs-lookup"><span data-stu-id="a6616-138">`UseMvc` doesn't directly define any routes, it adds a placeholder to the route collection for the `attribute` route.</span></span> <span data-ttu-id="a6616-139">Die Überladung `UseMvc(Action<IRouteBuilder>)` können Sie Ihre eigenen Routen hinzufügen und unterstützt auch das routing-Attribut.</span><span class="sxs-lookup"><span data-stu-id="a6616-139">The overload `UseMvc(Action<IRouteBuilder>)` lets you add your own routes and also supports attribute routing.</span></span>  <span data-ttu-id="a6616-140">`UseMvc`und alle seine Varianten Fügt einen Platzhalter für die attributenroute - routing-Attribut ist immer verfügbar, unabhängig von der Konfiguration `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="a6616-140">`UseMvc` and all of its variations adds a placeholder for the attribute route - attribute routing is always available regardless of how you configure `UseMvc`.</span></span> <span data-ttu-id="a6616-141">`UseMvcWithDefaultRoute`definiert eine Standardroute und routing-Attribut unterstützt.</span><span class="sxs-lookup"><span data-stu-id="a6616-141">`UseMvcWithDefaultRoute` defines a default route and supports attribute routing.</span></span> <span data-ttu-id="a6616-142">Die [Routing-Attribut](#attribute-routing-ref-label) Abschnitt enthält weitere Informationen zu routing-Attribut.</span><span class="sxs-lookup"><span data-stu-id="a6616-142">The [Attribute Routing](#attribute-routing-ref-label) section includes more details on attribute routing.</span></span>

<a name="routing-conventional-ref-label"></a>

## <a name="conventional-routing"></a><span data-ttu-id="a6616-143">Herkömmliche routing</span><span class="sxs-lookup"><span data-stu-id="a6616-143">Conventional routing</span></span>

<span data-ttu-id="a6616-144">Die `default` Route:</span><span class="sxs-lookup"><span data-stu-id="a6616-144">The `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="a6616-145">ist ein Beispiel für eine *konventionellen routing*.</span><span class="sxs-lookup"><span data-stu-id="a6616-145">is an example of a *conventional routing*.</span></span> <span data-ttu-id="a6616-146">Wir rufen Sie dieses Format *konventionellen routing* da-Objektvariablen eine *Konvention* für URL-Pfade:</span><span class="sxs-lookup"><span data-stu-id="a6616-146">We call this style *conventional routing* because it establishes a *convention* for URL paths:</span></span>

* <span data-ttu-id="a6616-147">das erste Pfadsegment ordnet den Namen des Controllers</span><span class="sxs-lookup"><span data-stu-id="a6616-147">the first path segment maps to the controller name</span></span>

* <span data-ttu-id="a6616-148">die zweite ordnet den Aktionsnamen.</span><span class="sxs-lookup"><span data-stu-id="a6616-148">the second maps to the action name.</span></span>

* <span data-ttu-id="a6616-149">Das dritte Segment wird verwendet, einen optionalen `id` verwendet, um eine modellentität zuordnen</span><span class="sxs-lookup"><span data-stu-id="a6616-149">the third segment is used for an optional `id` used to map to a model entity</span></span>

<span data-ttu-id="a6616-150">Mit diesem `default` Route, die URL-Pfad `/Products/List` ordnet die `ProductsController.List` Aktion und `/Blog/Article/17` ordnet `BlogController.Article`.</span><span class="sxs-lookup"><span data-stu-id="a6616-150">Using this `default` route, the URL path `/Products/List` maps to the `ProductsController.List` action, and `/Blog/Article/17` maps to `BlogController.Article`.</span></span> <span data-ttu-id="a6616-151">Diese Zuordnung basiert auf den Controller- und Namen **nur** und wird nicht anhand des Namespaces, Speicherorte für Quelldateien oder Methodenparameter.</span><span class="sxs-lookup"><span data-stu-id="a6616-151">This mapping is based on the controller and action names **only** and isn't based on namespaces, source file locations, or method parameters.</span></span>

> [!TIP]
> <span data-ttu-id="a6616-152">Mithilfe der herkömmlichen routing mit die Standardroute können Sie die Anwendung schnell erstellen, ohne Sie zu einem neuen URL-Muster für jede Aktion, die Sie definieren.</span><span class="sxs-lookup"><span data-stu-id="a6616-152">Using conventional routing with the default route allows you to build the application quickly without having to come up with a new URL pattern for each action you define.</span></span> <span data-ttu-id="a6616-153">Für eine Anwendung mit CRUD-Stil-Aktionen können Konsistenz für URLs zu, in Ihren Domänencontrollern Ihren Code vereinfachen die Benutzeroberfläche besser vorhersagbar zu machen.</span><span class="sxs-lookup"><span data-stu-id="a6616-153">For an application with CRUD style actions, having consistency for the URLs across your controllers can help simplify your code and make your UI more predictable.</span></span>

> [!WARNING]
> <span data-ttu-id="a6616-154">Die `id` wird als optional definiert, durch die routenvorlage, was bedeutet, dass Ihre Aktionen werden, ohne die ID, die als Teil der URL bereitgestellt ausgeführt können.</span><span class="sxs-lookup"><span data-stu-id="a6616-154">The `id` is defined as optional by the route template, meaning that your actions can execute without the ID provided as part of the URL.</span></span> <span data-ttu-id="a6616-155">In der Regel was passiert, wenn `id` fehlt in der URL ist, dass er festgelegt wird `0` von modellbindung und demzufolge keine Entität befinden sich in der Datenbank entsprechen `id == 0`.</span><span class="sxs-lookup"><span data-stu-id="a6616-155">Usually what will happen if `id` is omitted from the URL is that it will be set to `0` by model binding, and as a result no entity will be found in the database matching `id == 0`.</span></span> <span data-ttu-id="a6616-156">Routing-Attribut können Sie die ID ist für einige Aktionen aus, und nicht für andere stellen eine differenzierte Steuerung gewähren.</span><span class="sxs-lookup"><span data-stu-id="a6616-156">Attribute routing can give you fine-grained control to make the ID required for some actions and not for others.</span></span> <span data-ttu-id="a6616-157">Gemäß der Konvention, die die Dokumentation enthält optionale Parameter wie `id` Wenn sie wahrscheinlich in die richtige Verwendung angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="a6616-157">By convention the documentation will include optional parameters like `id` when they're likely to appear in correct usage.</span></span>

## <a name="multiple-routes"></a><span data-ttu-id="a6616-158">Mehrere Routen</span><span class="sxs-lookup"><span data-stu-id="a6616-158">Multiple routes</span></span>

<span data-ttu-id="a6616-159">Sie können mehrere Routen innerhalb von hinzufügen `UseMvc` durch Hinzufügen von mehr Aufrufe zu `MapRoute`.</span><span class="sxs-lookup"><span data-stu-id="a6616-159">You can add multiple routes inside `UseMvc` by adding more calls to `MapRoute`.</span></span> <span data-ttu-id="a6616-160">Auf diese Weise können Sie mehrere Konventionen definieren, oder konventionellen Routen hinzu, die einer bestimmten Aktion zugeordnet sind, z. B.:</span><span class="sxs-lookup"><span data-stu-id="a6616-160">Doing so allows you to define multiple conventions, or to add conventional routes that are dedicated to a specific action, such as:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="a6616-161">Die `blog` Route hier ist ein *dedizierten konventionellen Route*, was bedeutet, dass das herkömmliche Routingsystem verwendet, aber für eine bestimmte Aktion dediziert ist.</span><span class="sxs-lookup"><span data-stu-id="a6616-161">The `blog` route here is a *dedicated conventional route*, meaning that it uses the conventional routing system, but is dedicated to a specific action.</span></span> <span data-ttu-id="a6616-162">Da `controller` und `action` nicht als Parameter in der routenvorlage angezeigt, können nur die Standardwerte aufweisen und diese Route wird daher immer zugeordnet, auf die Aktion `BlogController.Article`.</span><span class="sxs-lookup"><span data-stu-id="a6616-162">Since `controller` and `action` don't appear in the route template as parameters, they can only have the default values, and thus this route will always map to the action `BlogController.Article`.</span></span>

<span data-ttu-id="a6616-163">Routen in der Auflistung von Routen werden sortiert und verarbeitet werden, in der Reihenfolge, die sie hinzugefügt haben.</span><span class="sxs-lookup"><span data-stu-id="a6616-163">Routes in the route collection are ordered, and will be processed in the order they're added.</span></span> <span data-ttu-id="a6616-164">In diesem Beispiel daher die `blog` Route versucht, bevor die `default` Route.</span><span class="sxs-lookup"><span data-stu-id="a6616-164">So in this example, the `blog` route will be tried before the `default` route.</span></span>

> [!NOTE]
> <span data-ttu-id="a6616-165">*Dedizierte konventionelle Routen* verwenden häufig Catchall-Routenparameter wie `{*article}` zum Erfassen des verbleibenden Teils der URL-Pfad.</span><span class="sxs-lookup"><span data-stu-id="a6616-165">*Dedicated conventional routes* often use catch-all route parameters like `{*article}` to capture the remaining portion of the URL path.</span></span> <span data-ttu-id="a6616-166">Damit kann eine Route "zu gierigen", was bedeutet, dass er URLs findet, die Sie von anderen Routen abgeglichen werden sollen.</span><span class="sxs-lookup"><span data-stu-id="a6616-166">This can make a route 'too greedy' meaning that it matches URLs that you intended to be matched by other routes.</span></span> <span data-ttu-id="a6616-167">Fügen Sie die "gierigen" Routen weiter unten in der Routingtabelle, um dieses Problem zu beheben.</span><span class="sxs-lookup"><span data-stu-id="a6616-167">Put the 'greedy' routes later in the route table to solve this.</span></span>

### <a name="fallback"></a><span data-ttu-id="a6616-168">Fallback</span><span class="sxs-lookup"><span data-stu-id="a6616-168">Fallback</span></span>

<span data-ttu-id="a6616-169">Im Rahmen der anforderungsverarbeitung MVC überprüft, ob die Routenwerte verwendet werden können, auf einem Controller und Aktion in der Anwendung feststellen.</span><span class="sxs-lookup"><span data-stu-id="a6616-169">As part of request processing, MVC will verify that the route values can be used to find a controller and action in your application.</span></span> <span data-ttu-id="a6616-170">Falls die Routenwerte eine Aktion übereinstimmen klicken Sie dann die Route ist kein Übereinstimmung angesehen, und die nächste Route wird versucht.</span><span class="sxs-lookup"><span data-stu-id="a6616-170">If the route values don't match an action then the route isn't considered a match, and the next route will be tried.</span></span> <span data-ttu-id="a6616-171">Hierbei spricht *fallback*, und es wurde vorgesehen, um Fälle zu vereinfachen, konventionelle Routen, die sich überlappen.</span><span class="sxs-lookup"><span data-stu-id="a6616-171">This is called *fallback*, and it's intended to simplify cases where conventional routes overlap.</span></span>

### <a name="disambiguating-actions"></a><span data-ttu-id="a6616-172">Aktionen eindeutig gemacht</span><span class="sxs-lookup"><span data-stu-id="a6616-172">Disambiguating actions</span></span>

<span data-ttu-id="a6616-173">Wenn die beiden Aktionen werden über Weiterleitung durch übereinstimmen, muss MVC eindeutig machen, um wählen "beste" geeignet, da sonst eine Ausnahme auslösen.</span><span class="sxs-lookup"><span data-stu-id="a6616-173">When two actions match through routing, MVC must disambiguate to choose the 'best' candidate or else throw an exception.</span></span> <span data-ttu-id="a6616-174">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a6616-174">For example:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

<span data-ttu-id="a6616-175">Dieser Controller definiert zwei Aktionen, die mit den URL-Pfad übereinstimmen würden `/Products/Edit/17` und Weiterleiten von Daten `{ controller = Products, action = Edit, id = 17 }`.</span><span class="sxs-lookup"><span data-stu-id="a6616-175">This controller defines two actions that would match the URL path `/Products/Edit/17` and route data `{ controller = Products, action = Edit, id = 17 }`.</span></span> <span data-ttu-id="a6616-176">Dies ist ein typisches Muster für MVC-Controller, in denen `Edit(int)` zeigt ein Formular zum Bearbeiten eines Produkts und `Edit(int, Product)` bereitgestellte Formular verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="a6616-176">This is a typical pattern for MVC controllers where `Edit(int)` shows a form to edit a product, and `Edit(int, Product)` processes  the posted form.</span></span> <span data-ttu-id="a6616-177">Um dies zu ermöglichen MVC auswählen müssen `Edit(int, Product)` bei die Anforderung ist ein HTTP `POST` und `Edit(int)` Wenn das HTTP-Verb ist etwas anderes.</span><span class="sxs-lookup"><span data-stu-id="a6616-177">To make this possible MVC would need to choose `Edit(int, Product)` when the request is an HTTP `POST` and `Edit(int)` when the HTTP verb is anything else.</span></span>

<span data-ttu-id="a6616-178">Die `HttpPostAttribute` ( `[HttpPost]` ) ist eine Implementierung von `IActionConstraint` nur die Aktion, die ausgewählt werden, wenn das HTTP-Verb ermöglichen `POST`.</span><span class="sxs-lookup"><span data-stu-id="a6616-178">The `HttpPostAttribute` ( `[HttpPost]` ) is an implementation of `IActionConstraint` that will only allow the action to be selected when the HTTP verb is `POST`.</span></span> <span data-ttu-id="a6616-179">Das Vorhandensein einer `IActionConstraint` macht die `Edit(int, Product)` "bessere" übereinstimmen, als `Edit(int)`, sodass `Edit(int, Product)` wird zuerst versucht werden.</span><span class="sxs-lookup"><span data-stu-id="a6616-179">The presence of an `IActionConstraint` makes the `Edit(int, Product)` a 'better' match than `Edit(int)`, so `Edit(int, Product)` will be tried first.</span></span>

<span data-ttu-id="a6616-180">Sie müssen nur zum Schreiben von benutzerdefinierten `IActionConstraint` Implementierungen in speziellen Szenarien, aber es wichtig zu verstehen, die Rolle des Attribute wie den `HttpPostAttribute` -ähnliche Attributen für andere HTTP-Verben definiert sind.</span><span class="sxs-lookup"><span data-stu-id="a6616-180">You will only need to write custom `IActionConstraint` implementations in specialized scenarios, but it's important to understand the role of attributes like `HttpPostAttribute`  - similar attributes are defined for other HTTP verbs.</span></span> <span data-ttu-id="a6616-181">In herkömmlichen routing wird für Aktionen, die den gleichen Aktionsnamen verwenden, wenn Inhaltspakete Teil von sind häufige eine `show form -> submit form` Workflow.</span><span class="sxs-lookup"><span data-stu-id="a6616-181">In conventional routing it's common for actions to use the same action name when they're part of a `show form -> submit form` workflow.</span></span> <span data-ttu-id="a6616-182">Die Vorteile dieses Muster wird offensichtlich, mehr nach dem Überprüfen der [Verständnis IActionConstraint](#understanding-iactionconstraint) Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="a6616-182">The convenience of this pattern will become more apparent after reviewing the [Understanding IActionConstraint](#understanding-iactionconstraint) section.</span></span>

<span data-ttu-id="a6616-183">Wenn mehrere Routen entsprechen und MVC eine "bewährte" Route kann nicht gefunden werden kann, löst sie eine `AmbiguousActionException`.</span><span class="sxs-lookup"><span data-stu-id="a6616-183">If multiple routes match, and MVC can't find a 'best' route, it will throw an `AmbiguousActionException`.</span></span>

<a name="routing-route-name-ref-label"></a>

### <a name="route-names"></a><span data-ttu-id="a6616-184">Routennamen</span><span class="sxs-lookup"><span data-stu-id="a6616-184">Route names</span></span>

<span data-ttu-id="a6616-185">Die Zeichenfolgen `"blog"` und `"default"` in den folgenden Beispielen werden Routennamen:</span><span class="sxs-lookup"><span data-stu-id="a6616-185">The strings  `"blog"` and `"default"` in the following examples are route names:</span></span>


```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
               defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="a6616-186">Die Routennamen benennen Sie der Route logische, damit für URL-Generierung die benannte Route verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="a6616-186">The route names give the route a logical name so that the named route can be used for URL generation.</span></span> <span data-ttu-id="a6616-187">Dadurch wird URL-Erstellung erheblich vereinfacht, um die Reihenfolge der Routen URL-Generierung kompliziert vornehmen kann.</span><span class="sxs-lookup"><span data-stu-id="a6616-187">This greatly simplifies URL creation when the ordering of routes could make URL generation complicated.</span></span> <span data-ttu-id="a6616-188">Routennamen müssen anwendungsweite eindeutig sein.</span><span class="sxs-lookup"><span data-stu-id="a6616-188">Route names must be unique application-wide.</span></span>

<span data-ttu-id="a6616-189">Routennamen haben keine Auswirkung auf die URL entsprechen oder die Verarbeitung von Anforderungen; Sie sind nur für die Erzeugung der URL verwendet.</span><span class="sxs-lookup"><span data-stu-id="a6616-189">Route names have no impact on URL matching or handling of requests; they're used only for URL generation.</span></span> <span data-ttu-id="a6616-190">[Routing](xref:fundamentals/routing) enthält weitere ausführliche Informationen zum URL-Generierung, einschließlich der URL-Generierung in MVC-spezifische Hilfsprogramme.</span><span class="sxs-lookup"><span data-stu-id="a6616-190">[Routing](xref:fundamentals/routing) has more detailed information on URL generation including URL generation in MVC-specific helpers.</span></span>

<a name="attribute-routing-ref-label"></a>

## <a name="attribute-routing"></a><span data-ttu-id="a6616-191">Routing-Attribut</span><span class="sxs-lookup"><span data-stu-id="a6616-191">Attribute routing</span></span>

<span data-ttu-id="a6616-192">Routing-Attribut verwendet einen Satz von Attributen, um Aktionen routenvorlagen direkt zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="a6616-192">Attribute routing uses a set of attributes to map actions directly to route templates.</span></span> <span data-ttu-id="a6616-193">Im folgenden Beispiel `app.UseMvc();` werden in der `Configure` -Methode und keine Route übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="a6616-193">In the following example, `app.UseMvc();` is used in the `Configure` method and no route is passed.</span></span> <span data-ttu-id="a6616-194">Die `HomeController` entspricht einen Satz von URLs, welche die Standardroute ähnelt `{controller=Home}/{action=Index}/{id?}` entsprechen:</span><span class="sxs-lookup"><span data-stu-id="a6616-194">The `HomeController` will match a set of URLs similar to what the default route `{controller=Home}/{action=Index}/{id?}` would match:</span></span>

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

<span data-ttu-id="a6616-195">Die `HomeController.Index()` Aktion wird ausgeführt werden, für den URL-Pfaden `/`, `/Home`, oder `/Home/Index`.</span><span class="sxs-lookup"><span data-stu-id="a6616-195">The `HomeController.Index()` action will be executed for any of the URL paths `/`, `/Home`, or `/Home/Index`.</span></span>

> [!NOTE]
> <span data-ttu-id="a6616-196">In diesem Beispiel werden eine programming Hauptunterschied zwischen routing-Attribut und herkömmliche routing hervorgehoben.</span><span class="sxs-lookup"><span data-stu-id="a6616-196">This example highlights a key programming difference between attribute routing and conventional routing.</span></span> <span data-ttu-id="a6616-197">Routing-Attribut erfordert weitere Eingabe, um eine Route anzugeben. die herkömmliche Standardroute verarbeitet Routen prägnanter.</span><span class="sxs-lookup"><span data-stu-id="a6616-197">Attribute routing requires more input to specify a route; the conventional default route handles routes more succinctly.</span></span> <span data-ttu-id="a6616-198">Allerdings routing-Attribut zulässt (und erfordert) präzise Kontrolle der routenvorlagen für jede Aktion gelten.</span><span class="sxs-lookup"><span data-stu-id="a6616-198">However, attribute routing allows (and requires) precise control of which route templates apply to each action.</span></span>

<span data-ttu-id="a6616-199">Mit dem Controllernamen und Aktionsnamen routing-Attribut wiedergeben **keine** Rolle in der Aktion aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="a6616-199">With attribute routing the controller name and action names play **no** role in which action is selected.</span></span> <span data-ttu-id="a6616-200">In diesem Beispiel werden die gleichen wie im vorherigen Beispiel-URLs abgeglichen.</span><span class="sxs-lookup"><span data-stu-id="a6616-200">This example will match the same URLs as the previous example.</span></span>

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
> <span data-ttu-id="a6616-201">Die oben genannten routenvorlagen nicht für die Routenparameter definieren `action`, `area`, und `controller`.</span><span class="sxs-lookup"><span data-stu-id="a6616-201">The route templates above don't define route parameters for `action`, `area`, and `controller`.</span></span> <span data-ttu-id="a6616-202">Diese Routenparameter sind in der Tat in attributenrouten nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="a6616-202">In fact, these route parameters are not allowed in attribute routes.</span></span> <span data-ttu-id="a6616-203">Da die routenvorlage bereits mit der Aktion zugeordnet ist, wäre nicht sinnvoll, den Namen der Aktion aus der URL analysieren erleichtern.</span><span class="sxs-lookup"><span data-stu-id="a6616-203">Since the route template is already associated with an action, it wouldn't make sense to parse the action name from the URL.</span></span>

## <a name="attribute-routing-with-httpverb-attributes"></a><span data-ttu-id="a6616-204">Routing mit Attributen Http [Verb]-Attribut</span><span class="sxs-lookup"><span data-stu-id="a6616-204">Attribute routing with Http[Verb] attributes</span></span>

<span data-ttu-id="a6616-205">Routing-Attribut kann auch vornehmen, verwenden die `Http[Verb]` Attribute wie `HttpPostAttribute`.</span><span class="sxs-lookup"><span data-stu-id="a6616-205">Attribute routing can also make use of the `Http[Verb]` attributes such as `HttpPostAttribute`.</span></span> <span data-ttu-id="a6616-206">Alle diese Attribute können keine routenvorlage akzeptieren.</span><span class="sxs-lookup"><span data-stu-id="a6616-206">All of these attributes can accept a route template.</span></span> <span data-ttu-id="a6616-207">Dieses Beispiel zeigt zwei Aktionen, die die gleichen routenvorlage entsprechen:</span><span class="sxs-lookup"><span data-stu-id="a6616-207">This example shows two actions that match the same route template:</span></span>

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

<span data-ttu-id="a6616-208">Für einen URL-Pfad, z. B. `/products` der `ProductsApi.ListProducts` Aktion wird ausgeführt werden, wenn das HTTP-Verb `GET` und `ProductsApi.CreateProduct` wird ausgeführt werden, wenn das HTTP-Verb `POST`.</span><span class="sxs-lookup"><span data-stu-id="a6616-208">For a URL path like `/products` the `ProductsApi.ListProducts` action will be executed when the HTTP verb is `GET` and `ProductsApi.CreateProduct` will be executed when the HTTP verb is `POST`.</span></span> <span data-ttu-id="a6616-209">Zuerst routing-Attribut entspricht den URL für den Satz von routenvorlagen durch routenattributen definiert.</span><span class="sxs-lookup"><span data-stu-id="a6616-209">Attribute routing first matches the URL against the set of route templates defined by route attributes.</span></span> <span data-ttu-id="a6616-210">Nachdem Sie eine routenvorlage für die übereinstimmt, `IActionConstraint` Einschränkungen werden angewendet, um zu bestimmen, welche Aktionen ausgeführt werden können.</span><span class="sxs-lookup"><span data-stu-id="a6616-210">Once a route template matches, `IActionConstraint` constraints are applied to determine which actions can be executed.</span></span>

> [!TIP]
> <span data-ttu-id="a6616-211">Wenn Sie eine REST-API zu erstellen, ist selten, dass Sie verwenden möchten `[Route(...)]` einer Aktionsmethode aufwiesen.</span><span class="sxs-lookup"><span data-stu-id="a6616-211">When building a REST API, it's rare that you will want to use `[Route(...)]` on an action method.</span></span> <span data-ttu-id="a6616-212">Es ist besser, verwenden Sie die spezifischere `Http*Verb*Attributes` präzise dazu, was Ihre API unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="a6616-212">It's better to use the more specific `Http*Verb*Attributes` to be precise about what your API supports.</span></span> <span data-ttu-id="a6616-213">Zu wissen, welche bestimmten logischen Operationen Pfade und HTTP-Verben zugeordnet, werden Clients von REST-APIs erwartet.</span><span class="sxs-lookup"><span data-stu-id="a6616-213">Clients of REST APIs are expected to know what paths and HTTP verbs map to specific logical operations.</span></span>

<span data-ttu-id="a6616-214">Da eine attributenroute für eine bestimmte Aktion gültig ist, ist es ganz einfach als Teil der Vorlage Routendefinition erforderlichen Parameter.</span><span class="sxs-lookup"><span data-stu-id="a6616-214">Since an attribute route applies to a specific action, it's easy to make parameters required as part of the route template definition.</span></span> <span data-ttu-id="a6616-215">In diesem Beispiel `id` als Teil des URL-Pfads erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="a6616-215">In this example, `id` is required as part of the URL path.</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="a6616-216">Die `ProductsApi.GetProduct(int)` Aktion wird ausgeführt für einen URL-Pfad, z. B. `/products/3` jedoch nicht für einen URL-Pfad, z. B. `/products`.</span><span class="sxs-lookup"><span data-stu-id="a6616-216">The `ProductsApi.GetProduct(int)` action will be executed for a URL path like `/products/3` but not for a URL path like `/products`.</span></span> <span data-ttu-id="a6616-217">Finden Sie unter [Routing](../../fundamentals/routing.md) für eine vollständige Beschreibung der routenvorlagen und verwandten Optionen.</span><span class="sxs-lookup"><span data-stu-id="a6616-217">See [Routing](../../fundamentals/routing.md) for a full description of route templates and related options.</span></span>

## <a name="route-name"></a><span data-ttu-id="a6616-218">Routenname</span><span class="sxs-lookup"><span data-stu-id="a6616-218">Route Name</span></span>

<span data-ttu-id="a6616-219">Der folgende Code definiert eine *Routennamen* von `Products_List`:</span><span class="sxs-lookup"><span data-stu-id="a6616-219">The following code  defines a *route name* of `Products_List`:</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="a6616-220">Routennamen können verwendet werden, um eine URL basierend auf einer bestimmten Route zu generieren.</span><span class="sxs-lookup"><span data-stu-id="a6616-220">Route names can be used to generate a URL based on a specific route.</span></span> <span data-ttu-id="a6616-221">Routennamen haben keine Auswirkung auf die URL Vergleichsverhalten von routing und dienen nur zum URL-Generierung.</span><span class="sxs-lookup"><span data-stu-id="a6616-221">Route names have no impact on the URL matching behavior of routing and are only used for URL generation.</span></span> <span data-ttu-id="a6616-222">Routennamen müssen anwendungsweite eindeutig sein.</span><span class="sxs-lookup"><span data-stu-id="a6616-222">Route names must be unique application-wide.</span></span>

> [!NOTE]
> <span data-ttu-id="a6616-223">Vergleichen Sie dies mit der konventionellen *Standardroute*, definiert die `id` Parameter als optional (`{id?}`).</span><span class="sxs-lookup"><span data-stu-id="a6616-223">Contrast this with the conventional *default route*, which defines the `id` parameter as optional (`{id?}`).</span></span> <span data-ttu-id="a6616-224">Diese Fähigkeit, genau APIs geben bietet Vorteile, z. B. schreibbarkeit `/products` und `/products/5` an unterschiedliche Aktionen gesendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="a6616-224">This ability to precisely specify APIs has advantages, such as  allowing `/products` and `/products/5` to be dispatched to different actions.</span></span>

<a name="routing-combining-ref-label"></a>

### <a name="combining-routes"></a><span data-ttu-id="a6616-225">Kombinieren von Routen</span><span class="sxs-lookup"><span data-stu-id="a6616-225">Combining routes</span></span>

<span data-ttu-id="a6616-226">Um machen routing-Attribut weniger wiederkehrende werden routenattributen auf dem Controller mit routenattribute in die einzelnen Aktionen kombiniert.</span><span class="sxs-lookup"><span data-stu-id="a6616-226">To make attribute routing less repetitive, route attributes on the controller are combined with route attributes on the individual actions.</span></span> <span data-ttu-id="a6616-227">Routenvorlagen auf dem Controller definiert sind routenvorlagen zu den Aktionen vorangestellt.</span><span class="sxs-lookup"><span data-stu-id="a6616-227">Any route templates defined on the controller are prepended to route templates on the actions.</span></span> <span data-ttu-id="a6616-228">Platzierung von Route-Attribut auf dem Controller macht **alle** Aktionen im Controller verwenden routing-Attribut.</span><span class="sxs-lookup"><span data-stu-id="a6616-228">Placing a route attribute on the controller makes **all** actions in the controller use attribute routing.</span></span>

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

<span data-ttu-id="a6616-229">In diesem Beispiel wird die URL-Pfad `/products` erfüllen können `ProductsApi.ListProducts`, und die URL-Pfad `/products/5` erfüllen können `ProductsApi.GetProduct(int)`.</span><span class="sxs-lookup"><span data-stu-id="a6616-229">In this example the URL path `/products` can match `ProductsApi.ListProducts`, and the URL path `/products/5` can match `ProductsApi.GetProduct(int)`.</span></span> <span data-ttu-id="a6616-230">Beide der genannten Aktionen nur HTTP abgeglichen `GET` , da sie mit ausgestattet sind die `HttpGetAttribute`.</span><span class="sxs-lookup"><span data-stu-id="a6616-230">Both of these actions only match HTTP `GET` because they're decorated with the `HttpGetAttribute`.</span></span>

<span data-ttu-id="a6616-231">Weiterleiten von Vorlagen, die auf eine Aktion, die mit einem `/` nicht mit routenvorlagen gilt für den Controller kombiniert abrufen.</span><span class="sxs-lookup"><span data-stu-id="a6616-231">Route templates applied to an action that begin with a `/` don't get combined with route templates applied to the controller.</span></span> <span data-ttu-id="a6616-232">In diesem Beispiel vergleicht eine Sammlung von URL-Pfade ähnelt der *Standardroute*.</span><span class="sxs-lookup"><span data-stu-id="a6616-232">This example matches a set of URL paths similar to the *default route*.</span></span>

```csharp
[Route("Home")]
public class HomeController : Controller
{
    [Route("")]      // Combines to define the route template "Home"
    [Route("Index")] // Combines to define the route template "Home/Index"
    [Route("/")]     // Doesn't combine, defines the route template ""
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

<a name="routing-ordering-ref-label"></a>

### <a name="ordering-attribute-routes"></a><span data-ttu-id="a6616-233">Sortierung attributenrouten</span><span class="sxs-lookup"><span data-stu-id="a6616-233">Ordering attribute routes</span></span>

<span data-ttu-id="a6616-234">Im Gegensatz zu herkömmlichen Routen, die in einer definierten Reihenfolge ausgeführt, eine Struktur erstellt, und alle Routen gleichzeitig entspricht routing-Attribut.</span><span class="sxs-lookup"><span data-stu-id="a6616-234">In contrast to conventional routes which execute in a defined order, attribute routing builds a tree and matches all routes simultaneously.</span></span> <span data-ttu-id="a6616-235">Dies verhält sich wie – wenn die routeneinträge liefern platziert wurden, zu einer idealen Sortierung; die spezifischsten Routen haben die Möglichkeit, vor der allgemeineren Routen ausführen.</span><span class="sxs-lookup"><span data-stu-id="a6616-235">This behaves as-if the route entries were placed in an ideal ordering; the most specific routes have a chance to execute before the more general routes.</span></span>

<span data-ttu-id="a6616-236">Z. B. eine Route wie `blog/search/{topic}` ist genauer als eine Route wie `blog/{*article}`.</span><span class="sxs-lookup"><span data-stu-id="a6616-236">For example, a route like `blog/search/{topic}` is more specific than a route like `blog/{*article}`.</span></span> <span data-ttu-id="a6616-237">Logisch gesehen der `blog/search/{topic}` Route "" zuerst, wird standardmäßig ausgeführt, da dies nur sinnvolle Sortierung ist.</span><span class="sxs-lookup"><span data-stu-id="a6616-237">Logically speaking the `blog/search/{topic}` route 'runs' first, by default, because that's the only sensible ordering.</span></span> <span data-ttu-id="a6616-238">Herkömmliche routing verwenden, ist der Entwickler verantwortlich für die Platzierung von Routen in der gewünschten Reihenfolge.</span><span class="sxs-lookup"><span data-stu-id="a6616-238">Using conventional routing, the developer is  responsible for placing routes in the desired order.</span></span>

<span data-ttu-id="a6616-239">Attributenrouten können konfigurieren, dass eine Bestellung mit der `Order` -Eigenschaft aller Attribute Route Framework bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="a6616-239">Attribute routes can configure an order, using the `Order` property of all of the framework provided route attributes.</span></span> <span data-ttu-id="a6616-240">Routen werden verarbeitet, entsprechend eine aufsteigende Sortierung der `Order` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="a6616-240">Routes are processed according to an ascending sort of the `Order` property.</span></span> <span data-ttu-id="a6616-241">Die Standardreihenfolge ist `0`.</span><span class="sxs-lookup"><span data-stu-id="a6616-241">The default order is `0`.</span></span> <span data-ttu-id="a6616-242">Festlegen einer Route mit `Order = -1` ausgeführt wird, bevor die Routen, die eine Bestellung nicht festlegen.</span><span class="sxs-lookup"><span data-stu-id="a6616-242">Setting a route using `Order = -1` will run before routes that don't set an order.</span></span> <span data-ttu-id="a6616-243">Festlegen einer Route mit `Order = 1` nach Route Standardreihenfolge ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="a6616-243">Setting a route using `Order = 1` will run after default route ordering.</span></span>

> [!TIP]
> <span data-ttu-id="a6616-244">Vermeiden Sie je nach `Order`.</span><span class="sxs-lookup"><span data-stu-id="a6616-244">Avoid depending on `Order`.</span></span> <span data-ttu-id="a6616-245">Wenn die URL-Space explizite Werte ordnungsgemäß weiterleiten erforderlich ist, ist es wahrscheinlich für Clients sowie verwirrend.</span><span class="sxs-lookup"><span data-stu-id="a6616-245">If your URL-space requires explicit order values to route correctly, then it's likely confusing to clients as well.</span></span> <span data-ttu-id="a6616-246">Routing-Attribut wird im Allgemeinen die richtige Route mit übereinstimmenden URL ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="a6616-246">In general attribute routing will select the correct route with URL matching.</span></span> <span data-ttu-id="a6616-247">Wenn die Standardreihenfolge zum URL-Generierung nicht funktioniert, verwenden Routennamen als eine Außerkraftsetzung in der Regel einfacher als das Anwenden der `Order` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="a6616-247">If the default order used for URL generation isn't working, using route name as an override is usually simpler than applying the `Order` property.</span></span>

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a><span data-ttu-id="a6616-248">Ersetzung in routenvorlagen Token ([Controller], [Aktion] [Bereich])</span><span class="sxs-lookup"><span data-stu-id="a6616-248">Token replacement in route templates ([controller], [action], [area])</span></span>

<span data-ttu-id="a6616-249">Der Einfachheit halber attributenrouten unterstützen *token Ersatz* indem ein Token in eckige Klammern einschließen (`[`, `]`).</span><span class="sxs-lookup"><span data-stu-id="a6616-249">For convenience, attribute routes support *token replacement* by enclosing a token in square-braces (`[`, `]`).</span></span> <span data-ttu-id="a6616-250">Die Token `[action]`, `[area]`, und `[controller]` ersetzt werden mit den Werten der Aktionsname, der Bereichsname und des Controllernamens von der Aktion, in dem die Route definiert ist.</span><span class="sxs-lookup"><span data-stu-id="a6616-250">The tokens `[action]`, `[area]`, and `[controller]` will be replaced with the values of the action name, area name, and controller name from the action where the route is defined.</span></span> <span data-ttu-id="a6616-251">In diesem Beispiel können die Aktionen URL-Pfade entsprechen, wie in den Kommentaren beschrieben:</span><span class="sxs-lookup"><span data-stu-id="a6616-251">In this example the actions can match URL paths as described in the comments:</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="a6616-252">Tokenersetzung tritt auf, als der letzte Schritt der Erstellung der attributenrouten.</span><span class="sxs-lookup"><span data-stu-id="a6616-252">Token replacement occurs as the last step of building the attribute routes.</span></span> <span data-ttu-id="a6616-253">Im obigen Beispiel verhält sich der folgende Code entspricht:</span><span class="sxs-lookup"><span data-stu-id="a6616-253">The above example will behave the same as the following code:</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="a6616-254">Attributenrouten können auch mit Vererbung kombiniert werden.</span><span class="sxs-lookup"><span data-stu-id="a6616-254">Attribute routes can also be combined with inheritance.</span></span> <span data-ttu-id="a6616-255">Dies ist besonders effektiv mit tokenersetzung kombiniert.</span><span class="sxs-lookup"><span data-stu-id="a6616-255">This is particularly powerful combined with token replacement.</span></span>

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

<span data-ttu-id="a6616-256">Tokenersetzung gilt auch für Routennamen durch attributenrouten definiert.</span><span class="sxs-lookup"><span data-stu-id="a6616-256">Token replacement also applies to route names defined by attribute routes.</span></span> <span data-ttu-id="a6616-257">`[Route("[controller]/[action]", Name="[controller]_[action]")]`generiert einen eindeutigen Routennamen für jede Aktion.</span><span class="sxs-lookup"><span data-stu-id="a6616-257">`[Route("[controller]/[action]", Name="[controller]_[action]")]` will generate a unique route name for each action.</span></span>

<span data-ttu-id="a6616-258">Das literal tokenersetzung Trennzeichen entsprechend `[` oder `]`, es durch die Wiederholung des Zeichens mit Escapezeichen versehen (`[[` oder `]]`).</span><span class="sxs-lookup"><span data-stu-id="a6616-258">To match the literal token replacement delimiter `[` or  `]`, escape it by repeating the character (`[[` or `]]`).</span></span>

<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-routes"></a><span data-ttu-id="a6616-259">Mehrere Routen</span><span class="sxs-lookup"><span data-stu-id="a6616-259">Multiple Routes</span></span>

<span data-ttu-id="a6616-260">Routing unterstützt mehrere Routen, die die gleiche Aktion erreichen definieren des Attributs.</span><span class="sxs-lookup"><span data-stu-id="a6616-260">Attribute routing supports defining multiple routes that reach the same action.</span></span> <span data-ttu-id="a6616-261">Die häufigste Verwendung dieses imitieren, die das Verhalten entspricht der *konventionellen Standardroute* wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="a6616-261">The most common usage of this is to mimic the behavior of the *default conventional route* as shown in the following example:</span></span>

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

<span data-ttu-id="a6616-262">Einfügen von mehreren routenattributen auf dem Controller, bedeutet, dass es sich bei jeweils mit jedem der routenattribute in den Aktionsmethoden kombiniert werden.</span><span class="sxs-lookup"><span data-stu-id="a6616-262">Putting multiple route attributes on the controller means that each one will combine with each of the route attributes on the action methods.</span></span>

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

<span data-ttu-id="a6616-263">Bei mehreren routenattributen (implementiert, `IActionConstraint`) werden auf eine Aktion platziert, und klicken Sie dann jede Aktion-Einschränkung mit der routenvorlage aus dem Attribut kombiniert, die es definiert.</span><span class="sxs-lookup"><span data-stu-id="a6616-263">When multiple route attributes (that implement `IActionConstraint`) are placed on an action, then each action constraint combines with the route template from the attribute that defined it.</span></span>

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
> <span data-ttu-id="a6616-264">Während mehrere Routen zu Aktionen mit leistungsstarke erscheinen kann, ist es besser, URL-Bereich für die Anwendung einfach und klar definierten beibehalten.</span><span class="sxs-lookup"><span data-stu-id="a6616-264">While using multiple routes on actions can seem powerful, it's better to keep your application's URL space simple and well-defined.</span></span> <span data-ttu-id="a6616-265">Verwenden Sie mehrere Routen zu Aktionen nur, z. B. zur Unterstützung von vorhandenen Clients erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="a6616-265">Use multiple routes on actions only where needed, for example to support existing clients.</span></span>

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a><span data-ttu-id="a6616-266">Angeben Optionaler Routenparameter Attribut, Standardwerte und Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="a6616-266">Specifying attribute route optional parameters, default values, and constraints</span></span>

<span data-ttu-id="a6616-267">Attributenrouten unterstützen die gleichen Inline-Syntax als herkömmliche Routen optionale Parameter, Standardwerte und Einschränkungen angeben.</span><span class="sxs-lookup"><span data-stu-id="a6616-267">Attribute routes support the same inline syntax as conventional routes to specify optional parameters, default values, and constraints.</span></span>

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

<span data-ttu-id="a6616-268">Finden Sie unter [Route Vorlagenverweis](../../fundamentals/routing.md#route-template-reference) für eine ausführliche Beschreibung der Syntax der Route-Vorlage.</span><span class="sxs-lookup"><span data-stu-id="a6616-268">See [Route Template Reference](../../fundamentals/routing.md#route-template-reference) for a detailed description of route template syntax.</span></span>

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a><span data-ttu-id="a6616-269">Benutzerdefinierte routenattributen verwenden`IRouteTemplateProvider`</span><span class="sxs-lookup"><span data-stu-id="a6616-269">Custom route attributes using `IRouteTemplateProvider`</span></span>

<span data-ttu-id="a6616-270">Alle routenattribute im Framework bereitgestellt ( `[Route(...)]`, `[HttpGet(...)]` usw.) implementieren die `IRouteTemplateProvider` Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="a6616-270">All of the route attributes provided in the framework ( `[Route(...)]`, `[HttpGet(...)]` , etc.) implement the `IRouteTemplateProvider` interface.</span></span> <span data-ttu-id="a6616-271">MVC sucht nach Attribute auf Controllerklassen und Aktionsmethoden auf, wenn die Anwendung gestartet und verwendet diejenigen, die implementieren `IRouteTemplateProvider` den anfänglichen Satz von Routen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="a6616-271">MVC looks for attributes on controller classes and action methods when the app starts and uses the ones that implement `IRouteTemplateProvider` to build the initial set of routes.</span></span>

<span data-ttu-id="a6616-272">Sie können implementieren `IRouteTemplateProvider` Definieren eigener routenattribute.</span><span class="sxs-lookup"><span data-stu-id="a6616-272">You can implement `IRouteTemplateProvider` to define your own route attributes.</span></span> <span data-ttu-id="a6616-273">Jede `IRouteTemplateProvider` sortieren können Sie eine einzelne Route mit einer benutzerdefinierten routenvorlage definieren, und nennen Sie:</span><span class="sxs-lookup"><span data-stu-id="a6616-273">Each `IRouteTemplateProvider` allows you to define a single route with a custom route template, order, and name:</span></span>

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

<span data-ttu-id="a6616-274">Automatisch das Attribut aus dem obigen Beispiel legt die `Template` auf `"api/[controller]"` Wenn `[MyApiController]` angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="a6616-274">The attribute from the above example automatically sets the `Template` to `"api/[controller]"` when `[MyApiController]` is applied.</span></span>

<a name="routing-app-model-ref-label"></a>

### <a name="using-application-model-to-customize-attribute-routes"></a><span data-ttu-id="a6616-275">Anpassen von attributenrouten mithilfe Anwendungsmodell</span><span class="sxs-lookup"><span data-stu-id="a6616-275">Using Application Model to customize attribute routes</span></span>

<span data-ttu-id="a6616-276">Die *Anwendungsmodell* ist ein Objektmodell erstellt beim Starten aller Metadaten von MVC, weiterleiten und Aktionen ausführen.</span><span class="sxs-lookup"><span data-stu-id="a6616-276">The *application model* is an object model created at startup with all of the metadata used by MVC to route and execute your actions.</span></span> <span data-ttu-id="a6616-277">Die *Anwendungsmodell* enthält alle Daten vom routenattributen erfasst (über `IRouteTemplateProvider`).</span><span class="sxs-lookup"><span data-stu-id="a6616-277">The *application model* includes all of the data gathered from route attributes (through `IRouteTemplateProvider`).</span></span> <span data-ttu-id="a6616-278">Sie können schreiben *Konventionen* so ändern Sie das Anwendungsmodell zum Startzeitpunkt anpassen, wie routing verhält.</span><span class="sxs-lookup"><span data-stu-id="a6616-278">You can write *conventions* to modify the application model at startup time to customize how routing behaves.</span></span> <span data-ttu-id="a6616-279">Dieser Abschnitt zeigt ein einfaches Beispiel für das routing mit Anwendungsmodell anpassen.</span><span class="sxs-lookup"><span data-stu-id="a6616-279">This section shows a simple example of customizing routing using application model.</span></span>

[!code-csharp[Main](routing/sample/main/NamespaceRoutingConvention.cs)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a><span data-ttu-id="a6616-280">Gemischte routing:-Attribut routing Vs konventionellen routing</span><span class="sxs-lookup"><span data-stu-id="a6616-280">Mixed routing: Attribute routing vs conventional routing</span></span>

<span data-ttu-id="a6616-281">MVC-Anwendungen können die Verwendung von herkömmlichen routing und routing-Attribut kombinieren.</span><span class="sxs-lookup"><span data-stu-id="a6616-281">MVC applications can mix the use of conventional routing and attribute routing.</span></span> <span data-ttu-id="a6616-282">Es ist in der Regel verwenden herkömmliche Routen für Domänencontroller, die für den HTML-Seiten für Browser, und das Attribut routing für Domänencontroller, die für den REST-APIs.</span><span class="sxs-lookup"><span data-stu-id="a6616-282">It's typical to use conventional routes for controllers serving HTML pages for browsers, and attribute routing for controllers serving REST APIs.</span></span>

<span data-ttu-id="a6616-283">Aktionen werden konventionell entweder weitergeleitet oder Attribut weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="a6616-283">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="a6616-284">Platzieren eine Route auf dem Controller oder die Aktion erleichtert Attribut weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="a6616-284">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="a6616-285">Aktionen, die attributenrouten definieren, können nicht über die herkömmliche Routen und umgekehrt erreicht werden.</span><span class="sxs-lookup"><span data-stu-id="a6616-285">Actions that define attribute routes cannot be reached through the conventional routes and vice-versa.</span></span> <span data-ttu-id="a6616-286">**Alle** routenattribut auf dem Controller macht alle Aktionen im Controller Attribut weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="a6616-286">**Any** route attribute on the controller makes all actions in the controller attribute routed.</span></span>

> [!NOTE]
> <span data-ttu-id="a6616-287">Welche zwei Arten von routing Systeme unterscheidet, ist der Prozess, der angewendet, nachdem eine routenvorlage für die mit einer URL übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="a6616-287">What distinguishes the two types of routing systems is the process applied after a URL matches a route template.</span></span> <span data-ttu-id="a6616-288">In herkömmlichen routing dienen die Routenwerte aus der Übereinstimmung mit einer Nachschlagetabelle für alle Aktionen für herkömmliche gerouteten die Aktion und den Controller aus.</span><span class="sxs-lookup"><span data-stu-id="a6616-288">In conventional routing, the route values from the match are used to choose the action and controller from a lookup table of all conventional routed actions.</span></span> <span data-ttu-id="a6616-289">Klicken Sie in der routing-Attribut, jede Vorlage ist bereits eine Aktion zugeordnet und keine weiteren Suche erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="a6616-289">In attribute routing, each template is already associated with an action, and no further lookup is needed.</span></span>

<a name="routing-url-gen-ref-label"></a>

## <a name="url-generation"></a><span data-ttu-id="a6616-290">URL-Generierung</span><span class="sxs-lookup"><span data-stu-id="a6616-290">URL Generation</span></span>

<span data-ttu-id="a6616-291">MVC-Anwendungen können routing URL Generation Funktionen verwenden, um URL-Links, um Aktionen zu generieren.</span><span class="sxs-lookup"><span data-stu-id="a6616-291">MVC applications can use routing's URL generation features to generate URL links to actions.</span></span> <span data-ttu-id="a6616-292">Generieren von URLs eliminiert hartcodierten URLs, ausführenden Code robuster und verwaltbar.</span><span class="sxs-lookup"><span data-stu-id="a6616-292">Generating URLs eliminates hardcoding URLs, making your code more robust and maintainable.</span></span> <span data-ttu-id="a6616-293">Dieser Abschnitt konzentriert sich auf die URL Generation Features von MVC und befasst sich nur auf die Grundlagen der Funktionsweise der URL-Generierung.</span><span class="sxs-lookup"><span data-stu-id="a6616-293">This section focuses on the URL generation features provided by MVC and will only cover basics of how URL generation works.</span></span> <span data-ttu-id="a6616-294">Finden Sie unter [Routing](../../fundamentals/routing.md) für eine ausführliche Beschreibung der URL-Generierung.</span><span class="sxs-lookup"><span data-stu-id="a6616-294">See [Routing](../../fundamentals/routing.md) for a detailed description of URL generation.</span></span>

<span data-ttu-id="a6616-295">Die `IUrlHelper` Schnittstelle ist die zugrunde liegenden Teil einer Infrastruktur zwischen MVC und routing für URL-Generierung.</span><span class="sxs-lookup"><span data-stu-id="a6616-295">The `IUrlHelper` interface is the underlying piece of infrastructure between MVC and routing for URL generation.</span></span> <span data-ttu-id="a6616-296">Finden Sie eine Instanz von `IUrlHelper` über die `Url` Eigenschaft im Controller, Ansichten und Anzeigen von Komponenten.</span><span class="sxs-lookup"><span data-stu-id="a6616-296">You'll find an instance of `IUrlHelper` available through the `Url` property in controllers, views, and view components.</span></span>

<span data-ttu-id="a6616-297">In diesem Beispiel wird die `IUrlHelper` Schnittstelle wird verwendet, über die `Controller.Url` Eigenschaft zum Generieren einer URL an eine andere Aktion.</span><span class="sxs-lookup"><span data-stu-id="a6616-297">In this example, the `IUrlHelper` interface is used through the `Controller.Url` property to generate a URL to another action.</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

<span data-ttu-id="a6616-298">Wenn die Anwendung Parallelität mit konventionellen standardmäßig weiterzuleiten, den Wert der `url` Variable wird die Zeichenfolge für den URL-Pfad sein `/UrlGeneration/Destination`.</span><span class="sxs-lookup"><span data-stu-id="a6616-298">If the application is using the default conventional route, the value of the `url` variable will be the URL path string `/UrlGeneration/Destination`.</span></span> <span data-ttu-id="a6616-299">Diese URL-Pfad wird mit den Werten, die zum Übergeben von durch die Kombination aus der aktuellen Anforderung (ambient-Werte), die Routenwerte routing erstellt `Url.Action` und ersetzen diese Werte in der routenvorlage:</span><span class="sxs-lookup"><span data-stu-id="a6616-299">This URL path is created by routing by combining the route values from the current request (ambient values), with the values passed to `Url.Action` and substituting those values into the route template:</span></span>

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

<span data-ttu-id="a6616-300">Jede Routenparameter in der routenvorlage hat sein Wert durch die passenden Namen mit den Werten und die ambient-Werte ersetzt.</span><span class="sxs-lookup"><span data-stu-id="a6616-300">Each route parameter in the route template has its value substituted by matching names with the values and ambient values.</span></span> <span data-ttu-id="a6616-301">Ein Routenparameter, der einen Wert aufweist können einen Standardwert, wenn er verfügt über einen oder übersprungen werden, wenn er optional ist (wie im Fall von `id` in diesem Beispiel).</span><span class="sxs-lookup"><span data-stu-id="a6616-301">A route parameter that doesn't have a value can use a default value if it has one, or be skipped if it's optional (as in the case of `id` in this example).</span></span> <span data-ttu-id="a6616-302">URL-Generierung schlägt fehl, wenn alle erforderlichen Routenparameter einen entsprechenden Wert besitzt.</span><span class="sxs-lookup"><span data-stu-id="a6616-302">URL generation will fail if any required route parameter doesn't have a corresponding value.</span></span> <span data-ttu-id="a6616-303">Sollte die URL-Generierung für eine Route ein Fehler auftritt, wird die nächste Route versucht, bis alle Routen versucht wurden wurden oder eine Übereinstimmung gefunden wird.</span><span class="sxs-lookup"><span data-stu-id="a6616-303">If URL generation fails for a route, the next route is tried until all routes have been tried or a match is found.</span></span>

<span data-ttu-id="a6616-304">Das Beispiel `Url.Action` oben geht davon aus konventionellen routing, aber URL Generation funktioniert auf ähnliche Weise mit routing-Attribut, obwohl die Konzepte unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="a6616-304">The example of `Url.Action` above assumes conventional routing, but URL generation works similarly with attribute routing, though the concepts are different.</span></span> <span data-ttu-id="a6616-305">Mit herkömmlichen routing dienen zum Erweitern einer Vorlage und die Routenwerte für die Routenwerte `controller` und `action` in der Regel angezeigt werden in dieser Vorlage – dies funktioniert, da die URLs von routing gefunden wird, entsprechen einer *Konvention*.</span><span class="sxs-lookup"><span data-stu-id="a6616-305">With conventional routing, the route values are used to expand a template, and the route values for `controller` and `action` usually appear in that template - this works because the URLs matched by routing adhere to a *convention*.</span></span> <span data-ttu-id="a6616-306">In der routing-Attribut, das Routenwerte für `controller` und `action` dürfen nicht angezeigt werden in der Vorlage – sie werden stattdessen verwendet, um nachschlagen, welche Vorlage verwendet.</span><span class="sxs-lookup"><span data-stu-id="a6616-306">In attribute routing, the route values for `controller` and `action` are not allowed to appear in the template - they're instead used to look up which template to use.</span></span>

<span data-ttu-id="a6616-307">In diesem Beispiel wird das routing-Attribut verwendet:</span><span class="sxs-lookup"><span data-stu-id="a6616-307">This example uses attribute routing:</span></span>

[!code-csharp[Main](routing/sample/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

<span data-ttu-id="a6616-308">MVC erstellt eine Nachschlagetabelle Verwaltungsaktionen Attribut weitergeleitet und entspricht der `controller` und `action` Werte auf die routenvorlage für URL-Generierung verwendet.</span><span class="sxs-lookup"><span data-stu-id="a6616-308">MVC builds a lookup table of all attribute routed actions and will match the `controller` and `action` values to select the route template to use for URL generation.</span></span> <span data-ttu-id="a6616-309">Im obigen Beispiel `custom/url/to/destination` generiert wird.</span><span class="sxs-lookup"><span data-stu-id="a6616-309">In the sample above,   `custom/url/to/destination` is generated.</span></span>

### <a name="generating-urls-by-action-name"></a><span data-ttu-id="a6616-310">Generieren von URLs durch den Namen der Aktion</span><span class="sxs-lookup"><span data-stu-id="a6616-310">Generating URLs by action name</span></span>

<span data-ttu-id="a6616-311">`Url.Action` (`IUrlHelper` .</span><span class="sxs-lookup"><span data-stu-id="a6616-311">`Url.Action` (`IUrlHelper` .</span></span> <span data-ttu-id="a6616-312">`Action`) und alle zugehörigen Überladungen alle auf diesem Konzept basieren, dass Sie angeben, was Sie durch Angabe eines Controllernamen und Aktionsnamen die Verknüpfung erstellen möchten.</span><span class="sxs-lookup"><span data-stu-id="a6616-312">`Action`) and all related overloads all are based on that idea that you want to specify what you're linking to by specifying a controller name and action name.</span></span>

> [!NOTE]
> <span data-ttu-id="a6616-313">Bei Verwendung `Url.Action`, aktuelle Route Werte für `controller` und `action` sind für Sie – der Wert der angegebenen `controller` und `action` sind Teil der beiden *ambient Werte* **und** *Werte*.</span><span class="sxs-lookup"><span data-stu-id="a6616-313">When using `Url.Action`, the current route values for `controller` and `action` are specified for you - the value of `controller` and `action` are part of both *ambient values* **and** *values*.</span></span> <span data-ttu-id="a6616-314">Die Methode `Url.Action`, verwendet immer die aktuellen Werte von `action` und `controller` und generiert einen URL-Pfad, die an die aktuelle Aktion weiterleitet.</span><span class="sxs-lookup"><span data-stu-id="a6616-314">The method `Url.Action`, always uses the current values of `action` and `controller` and will generate a URL path that routes to the current action.</span></span>

<span data-ttu-id="a6616-315">Versuche, verwenden Sie die Werte in der ambient-Werte Informationen ausfüllen, die Sie bereitgestellt haben, während der Generierung eine URL-Routing.</span><span class="sxs-lookup"><span data-stu-id="a6616-315">Routing attempts to use the values in ambient values to fill in information that you didn't provide when generating a URL.</span></span> <span data-ttu-id="a6616-316">Verwenden eine Route wie `{a}/{b}/{c}/{d}` und ambient Werte `{ a = Alice, b = Bob, c = Carol, d = David }`, routing verfügt über genügend Informationen zum Generieren einer URL ohne zusätzliche Werte – da alle weiterleiten Parameter einen Wert aufweisen.</span><span class="sxs-lookup"><span data-stu-id="a6616-316">Using a route like `{a}/{b}/{c}/{d}` and ambient values `{ a = Alice, b = Bob, c = Carol, d = David }`, routing has enough information to generate a URL without any additional values - since all route parameters have a value.</span></span> <span data-ttu-id="a6616-317">Wenn Sie den Wert hinzugefügt `{ d = Donovan }`, den Wert `{ d = David }` würde ignoriert werden, und die generierte URL-Pfad wäre `Alice/Bob/Carol/Donovan`.</span><span class="sxs-lookup"><span data-stu-id="a6616-317">If you added the value `{ d = Donovan }`, the value `{ d = David }` would be ignored, and the generated URL path would be `Alice/Bob/Carol/Donovan`.</span></span>

> [!WARNING]
> <span data-ttu-id="a6616-318">URL-Pfade sind hierarchisch.</span><span class="sxs-lookup"><span data-stu-id="a6616-318">URL paths are hierarchical.</span></span> <span data-ttu-id="a6616-319">Im obigen Beispiel wird den Wert hinzufügen `{ c = Cheryl }`, beide Werte `{ c = Carol, d = David }` würde ignoriert werden.</span><span class="sxs-lookup"><span data-stu-id="a6616-319">In the example above, if you added the value `{ c = Cheryl }`, both of the values `{ c = Carol, d = David }` would be ignored.</span></span> <span data-ttu-id="a6616-320">In diesem Fall haben wir mehr einen Wert für `d` und URL-Generierung schlägt fehl.</span><span class="sxs-lookup"><span data-stu-id="a6616-320">In this case we no longer have a value for `d` and URL generation will fail.</span></span> <span data-ttu-id="a6616-321">Sie müssen den gewünschten Wert angeben `c` und `d`.</span><span class="sxs-lookup"><span data-stu-id="a6616-321">You would need to specify the desired value of `c` and `d`.</span></span>  <span data-ttu-id="a6616-322">Sie erwarten wahrscheinlich, dieses Problem mit der Standardroute erreicht werden soll (`{controller}/{action}/{id?}`)- jedoch selten tritt dieses Verhalten in der Praxis als `Url.Action` immer explizit anzugeben, wird eine `controller` und `action` Wert.</span><span class="sxs-lookup"><span data-stu-id="a6616-322">You might expect to hit this problem with the default route (`{controller}/{action}/{id?}`) - but you will rarely encounter this behavior in practice as `Url.Action` will always explicitly specify a `controller` and `action` value.</span></span>

<span data-ttu-id="a6616-323">Mehr Überladungen der `Url.Action` akzeptieren auch ein zusätzliches *Routenwerte* Werte außer für Routenparameter zu verwendendes Objekt `controller` und `action`.</span><span class="sxs-lookup"><span data-stu-id="a6616-323">Longer overloads of `Url.Action` also take an additional *route values* object to provide values for route parameters other than `controller` and `action`.</span></span> <span data-ttu-id="a6616-324">Sie werden am häufigsten finden Sie in diesem mit verwendet `id` wie `Url.Action("Buy", "Products", new { id = 17 })`.</span><span class="sxs-lookup"><span data-stu-id="a6616-324">You will most commonly see this used with `id` like `Url.Action("Buy", "Products", new { id = 17 })`.</span></span> <span data-ttu-id="a6616-325">Gemäß der Konvention der *Routenwerte* Objekt stellt normalerweise ein Objekt des anonymen Typs, aber es kann auch ein `IDictionary<>` oder eine *einfaches altes .NET Objekt*.</span><span class="sxs-lookup"><span data-stu-id="a6616-325">By convention the *route values* object is usually an object of anonymous type, but it can also be an `IDictionary<>` or a *plain old .NET object*.</span></span> <span data-ttu-id="a6616-326">Alle zusätzlichen Routenwerte, die Routenparameter nicht übereinstimmen, werden in der Abfragezeichenfolge platziert.</span><span class="sxs-lookup"><span data-stu-id="a6616-326">Any additional route values that don't match route parameters are put in the query string.</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/TestController.cs)]

> [!TIP]
> <span data-ttu-id="a6616-327">Um eine absolute URL zu erstellen, verwenden Sie eine Überladung, akzeptiert eine `protocol`:`Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span><span class="sxs-lookup"><span data-stu-id="a6616-327">To create an absolute URL, use an overload that accepts a `protocol`: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span></span>

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generating-urls-by-route"></a><span data-ttu-id="a6616-328">Generieren von URLs von route</span><span class="sxs-lookup"><span data-stu-id="a6616-328">Generating URLs by route</span></span>

<span data-ttu-id="a6616-329">Der obige Code veranschaulicht das Generieren einer URL durch die Übergabe der Controller- und Name.</span><span class="sxs-lookup"><span data-stu-id="a6616-329">The code above demonstrated generating a URL by passing in the controller and action name.</span></span> <span data-ttu-id="a6616-330">`IUrlHelper`bietet außerdem die `Url.RouteUrl` -Methodenfamilie.</span><span class="sxs-lookup"><span data-stu-id="a6616-330">`IUrlHelper` also provides the `Url.RouteUrl` family of methods.</span></span> <span data-ttu-id="a6616-331">Diese Methoden ähneln `Url.Action`, aber nicht können, kopieren Sie die aktuellen Werte der `action` und `controller` , die Routenwerte.</span><span class="sxs-lookup"><span data-stu-id="a6616-331">These methods are similar to `Url.Action`, but they don't copy the current values of `action` and `controller` to the route values.</span></span> <span data-ttu-id="a6616-332">Die häufigste Verwendung ist die Angabe von einem Routennamen an eine Route zu verwenden, um die URL in der Regel generieren *ohne* Angabe eines Namens Controller bzw. die Aktionsmethode.</span><span class="sxs-lookup"><span data-stu-id="a6616-332">The most common usage is to specify a route name to use a specific route to generate the URL, generally *without* specifying a controller or action name.</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generating-urls-in-html"></a><span data-ttu-id="a6616-333">Generieren von URLs in HTML</span><span class="sxs-lookup"><span data-stu-id="a6616-333">Generating URLs in HTML</span></span>

<span data-ttu-id="a6616-334">`IHtmlHelper`Stellt die `HtmlHelper` Methoden `Html.BeginForm` und `Html.ActionLink` generieren `<form>` und `<a>` Elemente bzw.</span><span class="sxs-lookup"><span data-stu-id="a6616-334">`IHtmlHelper` provides the `HtmlHelper` methods `Html.BeginForm` and `Html.ActionLink` to generate `<form>` and `<a>` elements respectively.</span></span> <span data-ttu-id="a6616-335">Verwenden Sie diese Methoden die `Url.Action` Methode, um eine URL zu generieren und ähnliche Argumente akzeptieren.</span><span class="sxs-lookup"><span data-stu-id="a6616-335">These methods use the `Url.Action` method to generate a URL and they accept similar arguments.</span></span> <span data-ttu-id="a6616-336">Die `Url.RouteUrl` Begleitgeräte für `HtmlHelper` sind `Html.BeginRouteForm` und `Html.RouteLink` die ähnliche Funktionen aufweisen.</span><span class="sxs-lookup"><span data-stu-id="a6616-336">The `Url.RouteUrl` companions for `HtmlHelper` are `Html.BeginRouteForm` and `Html.RouteLink` which have similar functionality.</span></span>

<span data-ttu-id="a6616-337">TagHelpers Generieren von URLs mit dem `form` taghelpers und die `<a>` taghelpers.</span><span class="sxs-lookup"><span data-stu-id="a6616-337">TagHelpers generate URLs through the `form` TagHelper and the `<a>` TagHelper.</span></span> <span data-ttu-id="a6616-338">Verwenden Sie diese beiden `IUrlHelper` für ihre Implementierung.</span><span class="sxs-lookup"><span data-stu-id="a6616-338">Both of these use `IUrlHelper` for their implementation.</span></span> <span data-ttu-id="a6616-339">Finden Sie unter [arbeiten mit Formularen](../views/working-with-forms.md) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="a6616-339">See [Working with Forms](../views/working-with-forms.md) for more information.</span></span>

<span data-ttu-id="a6616-340">In Ansichten die `IUrlHelper` kann über die `Url` -Eigenschaft für alle Ad-hoc-URL-Generierung, die nicht von den oben genannten abgedeckt.</span><span class="sxs-lookup"><span data-stu-id="a6616-340">Inside views, the `IUrlHelper` is available through the `Url` property for any ad-hoc URL generation not covered by the above.</span></span>

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="generating-urls-in-action-results"></a><span data-ttu-id="a6616-341">Generieren von URLS in Aktionsergebnisse</span><span class="sxs-lookup"><span data-stu-id="a6616-341">Generating URLS in Action Results</span></span>

<span data-ttu-id="a6616-342">Die obigen Beispiele haben gezeigt, mithilfe von `IUrlHelper` in einem Controller, während die häufigste Verwendung in einem Controller zum Generieren einer URL im Rahmen des ein Aktionsergebnis wird.</span><span class="sxs-lookup"><span data-stu-id="a6616-342">The examples above have shown using `IUrlHelper` in a controller, while the most common usage in a controller is to generate a URL as part of an action result.</span></span>

<span data-ttu-id="a6616-343">Die `ControllerBase` und `Controller` Basisklassen bieten Hilfsmethoden für Aktionsergebnisse, die auf eine andere Aktion verweisen.</span><span class="sxs-lookup"><span data-stu-id="a6616-343">The `ControllerBase` and `Controller` base classes provide convenience methods for action results that reference another action.</span></span> <span data-ttu-id="a6616-344">Eine typische Verwendung besteht darin, leiten nach der Benutzereingaben akzeptieren.</span><span class="sxs-lookup"><span data-stu-id="a6616-344">One typical usage is to redirect after accepting user input.</span></span>

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

<span data-ttu-id="a6616-345">Die Aktion Ergebnisse Factorymethoden folgen einem ähnlichen Muster auf die Methoden auf `IUrlHelper`.</span><span class="sxs-lookup"><span data-stu-id="a6616-345">The action results factory methods follow a similar pattern to the methods on `IUrlHelper`.</span></span>

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a><span data-ttu-id="a6616-346">Sonderfall für dedizierte konventionellen Routen</span><span class="sxs-lookup"><span data-stu-id="a6616-346">Special case for dedicated conventional routes</span></span>

<span data-ttu-id="a6616-347">Herkömmliche routing können Sie eine spezielle Art von Route-Definition bezeichnet eine *dedizierten konventionellen Route*.</span><span class="sxs-lookup"><span data-stu-id="a6616-347">Conventional routing can use a special kind of route definition called a *dedicated conventional route*.</span></span> <span data-ttu-id="a6616-348">Im folgenden Beispiel wird die Route mit dem Namen `blog` ist eine dedizierte konventionelle Route.</span><span class="sxs-lookup"><span data-stu-id="a6616-348">In the example below, the route named `blog` is a dedicated conventional route.</span></span>

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="a6616-349">Verwenden diese Route-Definitionen `Url.Action("Index", "Home")` generiert den URL-Pfad `/` mit der `default` Route, aber nicht, warum?</span><span class="sxs-lookup"><span data-stu-id="a6616-349">Using these route definitions, `Url.Action("Index", "Home")` will generate the URL path `/` with the `default` route, but why?</span></span> <span data-ttu-id="a6616-350">Können Sie erraten, die Routenwerte `{ controller = Home, action = Index }` wäre ausreichend, um eine URL generiert die Verwendung `blog`, und das Ergebnis wäre `/blog?action=Index&controller=Home`.</span><span class="sxs-lookup"><span data-stu-id="a6616-350">You might guess the route values `{ controller = Home, action = Index }` would be enough to generate a URL using `blog`, and the result would be `/blog?action=Index&controller=Home`.</span></span>

<span data-ttu-id="a6616-351">Dedizierte konventionelle Routen abhängig ist, ein spezielles Verhalten von Standardwerten, die einen entsprechenden Routenparameter besitzen, die verhindert, dass die Route wird "zu gieriger" mit URL-Generierung.</span><span class="sxs-lookup"><span data-stu-id="a6616-351">Dedicated conventional routes rely on a special behavior of default values that don't have a corresponding route parameter that prevents the route from being "too greedy" with URL generation.</span></span> <span data-ttu-id="a6616-352">In diesem Fall die Standardwerte sind `{ controller = Blog, action = Article }`, und weder `controller` noch `action` wird als ein Routenparameter.</span><span class="sxs-lookup"><span data-stu-id="a6616-352">In this case the default values are `{ controller = Blog, action = Article }`, and neither `controller` nor `action` appears as a route parameter.</span></span> <span data-ttu-id="a6616-353">Wenn routing URL-Generierung ausführt, müssen die angegebenen Werte die Standardwerte übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="a6616-353">When routing performs URL generation, the values provided must match the default values.</span></span> <span data-ttu-id="a6616-354">Mithilfe von URL-Generierung `blog` schlägt fehl, da die Werte `{ controller = Home, action = Index }` stimmen nicht überein. `{ controller = Blog, action = Article }`.</span><span class="sxs-lookup"><span data-stu-id="a6616-354">URL generation using `blog` will fail because the values `{ controller = Home, action = Index }` don't match `{ controller = Blog, action = Article }`.</span></span> <span data-ttu-id="a6616-355">Routing dann ausgewichen, um zu versuchen `default`, die erfolgreich ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="a6616-355">Routing then falls back to try `default`, which succeeds.</span></span>

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a><span data-ttu-id="a6616-356">Bereiche</span><span class="sxs-lookup"><span data-stu-id="a6616-356">Areas</span></span>

<span data-ttu-id="a6616-357">[Bereiche](areas.md) eine MVC-Funktion, die zum Organisieren von verwandten Funktionen in einer Gruppe als eine separate routing-Namespace (für Controlleraktionen) und Ordnerstruktur (Views) verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="a6616-357">[Areas](areas.md) are an MVC feature used to organize related functionality into a group as a separate routing-namespace (for controller actions) and folder structure (for views).</span></span> <span data-ttu-id="a6616-358">Ermöglicht es einer Anwendung mehrere Domänencontroller mit dem gleichen Namen – solange sie verschiedene sind mithilfe von Bereichen *Bereiche*.</span><span class="sxs-lookup"><span data-stu-id="a6616-358">Using areas allows an application to have multiple controllers with the same name - as long as they have different *areas*.</span></span> <span data-ttu-id="a6616-359">Mithilfe von Bereichen erstellt eine Hierarchie für die Weiterleitung und Hinzufügen von einem anderen Routenparameter, `area` auf `controller` und `action`.</span><span class="sxs-lookup"><span data-stu-id="a6616-359">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area` to `controller` and `action`.</span></span> <span data-ttu-id="a6616-360">In diesem Abschnitt wird erläutert, wie routing Bereiche - interagiert, finden Sie unter [Bereiche](areas.md) Weitere Informationen zur Verwendung von Bereichen mit Ansichten.</span><span class="sxs-lookup"><span data-stu-id="a6616-360">This section will discuss how routing interacts with areas - see [Areas](areas.md) for details about how areas are used with views.</span></span>

<span data-ttu-id="a6616-361">Das folgende Beispiel konfiguriert die MVC, um die Standardroute konventionellen verwenden und ein *Bereich Route* für einen Bereich mit dem Namen `Blog`:</span><span class="sxs-lookup"><span data-stu-id="a6616-361">The following example configures MVC to use the default conventional route and an *area route* for an area named `Blog`:</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet1)]

<span data-ttu-id="a6616-362">Beim Abgleich von URL-Pfad wie `/Manage/Users/AddUser`, erzeugt die erste Route die Routenwerte `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="a6616-362">When matching a URL path like `/Manage/Users/AddUser`, the first route will produce the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span> <span data-ttu-id="a6616-363">Die `area` routenwert wird erzeugt, indem ein Standardwert für `area`, in der Tat die Route erstellt, indem `MapAreaRoute` entspricht der folgenden:</span><span class="sxs-lookup"><span data-stu-id="a6616-363">The `area` route value is produced by a default value for `area`, in fact the route created by `MapAreaRoute` is equivalent to the following:</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet2)]

<span data-ttu-id="a6616-364">`MapAreaRoute`erstellt eine Route mit einem Standardwert und die Einschränkung für `area` verwenden den angegebenen Bereichsnamen in diesem Fall `Blog`.</span><span class="sxs-lookup"><span data-stu-id="a6616-364">`MapAreaRoute` creates a route using both a default value and constraint for `area` using the provided area name, in this case `Blog`.</span></span> <span data-ttu-id="a6616-365">Der Standardwert wird sichergestellt, dass die Route immer erzeugt `{ area = Blog, ... }`, die Einschränkung muss den Wert `{ area = Blog, ... }` für URL-Generierung.</span><span class="sxs-lookup"><span data-stu-id="a6616-365">The default value ensures that the route always produces `{ area = Blog, ... }`, the constraint requires the value `{ area = Blog, ... }` for URL generation.</span></span>

> [!TIP]
> <span data-ttu-id="a6616-366">Herkömmliche routing ist die Reihenfolge wichtig.</span><span class="sxs-lookup"><span data-stu-id="a6616-366">Conventional routing is order-dependent.</span></span> <span data-ttu-id="a6616-367">Im Allgemeinen sollten Routen mit Bereichen weiter oben in der Routingtabelle platziert werden, wie sie spezifischere als Routen, ohne einen Bereich sind.</span><span class="sxs-lookup"><span data-stu-id="a6616-367">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="a6616-368">Verwenden das oben genannte Beispiel an, würde die Routenwerte die folgende Aktion übereinstimmen:</span><span class="sxs-lookup"><span data-stu-id="a6616-368">Using the above example, the route values would match the following action:</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

<span data-ttu-id="a6616-369">Die `AreaAttribute` ist, was einen Controller kennzeichnet im Rahmen eines Bereichs, sagen wir, dass in diesem Controller ist der `Blog` Bereich.</span><span class="sxs-lookup"><span data-stu-id="a6616-369">The `AreaAttribute` is what denotes a controller as part of an area, we say that this controller is in the `Blog` area.</span></span> <span data-ttu-id="a6616-370">Domänencontroller ohne eine `[Area]` Attribut sind keine Mitglieder der jeder Bereich und werden **nicht** übereinstimmen, wenn die `area` Route wird von routing bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="a6616-370">Controllers without an `[Area]` attribute are not members of any area, and will **not** match when the `area` route value is provided by routing.</span></span> <span data-ttu-id="a6616-371">Im folgenden Beispiel kann nur der erste Controller aufgeführten entsprechen, die Routenwerte `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="a6616-371">In the following example, only the first controller listed can match the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[Main](routing/sample/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> <span data-ttu-id="a6616-372">Der Namespace der jedem Controller ist aus Gründen der Vollständigkeit im folgenden dargestellt: Andernfalls würde die Controller eine Benennung von in Konflikt stehen und ein Compilerfehler generiert haben.</span><span class="sxs-lookup"><span data-stu-id="a6616-372">The namespace of each controller is shown here for completeness - otherwise the controllers would have a naming conflict and generate a compiler error.</span></span> <span data-ttu-id="a6616-373">Klasse Namespaces haben keine Auswirkungen auf MVCs-routing.</span><span class="sxs-lookup"><span data-stu-id="a6616-373">Class namespaces have no effect on MVC's routing.</span></span>

<span data-ttu-id="a6616-374">Die ersten beiden Controller sind Elemente der Bereiche, und nur überein, wenn von ihren jeweiligen Bereichsname bereitgestellt wird der `area` Wert weiterzuleiten.</span><span class="sxs-lookup"><span data-stu-id="a6616-374">The first two controllers are members of areas, and only match when their respective area name is provided by the `area` route value.</span></span> <span data-ttu-id="a6616-375">Der dritte Controller ist nicht Mitglied eines beliebigen Bereich und die einzige Übereinstimmung kann, wenn kein Wert für `area` von routing bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="a6616-375">The third controller isn't a member of any area, and can only match when no value for `area` is provided by routing.</span></span>

> [!NOTE]
> <span data-ttu-id="a6616-376">Im Hinblick auf Übereinstimmung *kein Wert*, Abwesenheit der `area` Wert ist gleich als wäre der Wert für `area` waren Null oder eine leere Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="a6616-376">In terms of matching *no value*, the absence of the `area` value is the same as if the value for `area` were null or the empty string.</span></span>

<span data-ttu-id="a6616-377">Bei der Ausführung einer Aktion innerhalb eines Bereichs-Wert die Route für `area` als verfügbar ein *ambient Wert* für das routing für die Verwendung für URL-Generierung.</span><span class="sxs-lookup"><span data-stu-id="a6616-377">When executing an action inside an area, the route value for `area` will be available as an *ambient value* for routing to use for URL generation.</span></span> <span data-ttu-id="a6616-378">Dies bedeutet, dass standardmäßig Bereiche agieren *persistente* für URL-Generierung, wie im folgenden Beispiel gezeigt.</span><span class="sxs-lookup"><span data-stu-id="a6616-378">This means that by default areas act *sticky* for URL generation as demonstrated by the following sample.</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet3)]

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<a name="iactionconstraint-ref-label"></a>

## <a name="understanding-iactionconstraint"></a><span data-ttu-id="a6616-379">Grundlegendes zu IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="a6616-379">Understanding IActionConstraint</span></span>

> [!NOTE]
> <span data-ttu-id="a6616-380">Dieser Abschnitt ist eine tief greifende Framework Mechanismen und wie eine Aktion zum Ausführen von MVC auswählt.</span><span class="sxs-lookup"><span data-stu-id="a6616-380">This section is a deep-dive on framework internals and how MVC chooses an action to execute.</span></span> <span data-ttu-id="a6616-381">Eine typische Anwendung erforderlich keinen benutzerdefinierten.`IActionConstraint`</span><span class="sxs-lookup"><span data-stu-id="a6616-381">A typical application won't need a custom `IActionConstraint`</span></span>

<span data-ttu-id="a6616-382">Sie haben wahrscheinlich bereits verwendet `IActionConstraint` , auch wenn Sie nicht mit der Oberfläche vertraut sind.</span><span class="sxs-lookup"><span data-stu-id="a6616-382">You have likely already used `IActionConstraint` even if you're not familiar with the interface.</span></span> <span data-ttu-id="a6616-383">Die `[HttpGet]` Attribut und ähnliche `[Http-VERB]` Attribute implementieren `IActionConstraint` um die Ausführung einer Aktionsmethode einzuschränken.</span><span class="sxs-lookup"><span data-stu-id="a6616-383">The `[HttpGet]` Attribute and similar `[Http-VERB]` attributes implement `IActionConstraint` in order to limit the execution of an action method.</span></span>

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

<span data-ttu-id="a6616-384">Vorausgesetzt, die herkömmliche Standardroute des URL-Pfads `/Products/Edit` wären die Werte `{ controller = Products, action = Edit }`, entsprechen die **beide** der hier gezeigten Aktionen.</span><span class="sxs-lookup"><span data-stu-id="a6616-384">Assuming the default conventional route, the URL path `/Products/Edit` would produce the values `{ controller = Products, action = Edit }`, which would match **both** of the actions shown here.</span></span> <span data-ttu-id="a6616-385">In `IActionConstraint` Terminologie, die wir sagen würden, dass beide der genannten Aktionen Kandidaten - berücksichtigt werden, da beide die Routendaten übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="a6616-385">In `IActionConstraint` terminology we would say that both of these actions are considered candidates - as they both match the route data.</span></span>

<span data-ttu-id="a6616-386">Wenn die `HttpGetAttribute` ausgeführt wird, es wird angegeben, die *Edit()* wird eine Übereinstimmung für *abrufen* und keine Übereinstimmung für alle anderen HTTP-Verb.</span><span class="sxs-lookup"><span data-stu-id="a6616-386">When the `HttpGetAttribute` executes, it will say that *Edit()* is a match for *GET* and isn't a match for any other HTTP verb.</span></span> <span data-ttu-id="a6616-387">Die `Edit(...)` Aktion verfügt nicht über alle definierten Einschränkungen und daher HTTP-Verb entspricht.</span><span class="sxs-lookup"><span data-stu-id="a6616-387">The `Edit(...)` action doesn't have any constraints defined, and so will match any HTTP verb.</span></span> <span data-ttu-id="a6616-388">Vorausgesetzt, sodass eine `POST` – nur `Edit(...)` übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="a6616-388">So assuming a `POST` - only `Edit(...)` matches.</span></span> <span data-ttu-id="a6616-389">Aber für eine `GET` beide Aktionen können dennoch übereinstimmen - jedoch eine Aktion mit einer `IActionConstraint` gilt immer *besser* als eine Aktion ohne.</span><span class="sxs-lookup"><span data-stu-id="a6616-389">But, for a `GET` both actions can still match - however, an action with an `IActionConstraint` is always considered *better* than an action without.</span></span> <span data-ttu-id="a6616-390">Daher da `Edit()` hat `[HttpGet]` sie spezifischere gilt und wird ausgewählt werden, wenn beide Aktionen verglichen werden können.</span><span class="sxs-lookup"><span data-stu-id="a6616-390">So because `Edit()` has `[HttpGet]` it's considered more specific, and will be selected if both actions can match.</span></span>

<span data-ttu-id="a6616-391">Im Prinzip `IActionConstraint` ist eine Form der *überladen*, aber statt Überladen von Methoden mit demselben Namen, ist es überladen zwischen Aktionen, die die gleiche URL entsprechen.</span><span class="sxs-lookup"><span data-stu-id="a6616-391">Conceptually, `IActionConstraint` is a form of *overloading*, but instead of overloading methods with the same name, it's overloading between actions that match the same URL.</span></span> <span data-ttu-id="a6616-392">Routing-Attribut verwendet auch `IActionConstraint` und kann dazu führen, Aktionen, die von verschiedenen Controllern beide vermerkt Kandidaten.</span><span class="sxs-lookup"><span data-stu-id="a6616-392">Attribute routing also uses `IActionConstraint` and can result in actions from different controllers both being considered candidates.</span></span>

<a name="iactionconstraint-impl-ref-label"></a>

### <a name="implementing-iactionconstraint"></a><span data-ttu-id="a6616-393">Implementieren von IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="a6616-393">Implementing IActionConstraint</span></span>

<span data-ttu-id="a6616-394">Die einfachste Methode zum Implementieren einer `IActionConstraint` zum Erstellen einer Klasse abgeleitet ist `System.Attribute` und platzieren Sie es auf Ihre Aktionen und Controllern aus.</span><span class="sxs-lookup"><span data-stu-id="a6616-394">The simplest way to implement an `IActionConstraint` is to create a class derived from `System.Attribute` and place it on your actions and controllers.</span></span> <span data-ttu-id="a6616-395">MVC erkennt automatisch `IActionConstraint` , die als Attribute angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="a6616-395">MVC will automatically discover any `IActionConstraint` that are applied as attributes.</span></span> <span data-ttu-id="a6616-396">Sie verwenden das Anwendungsmodell, um Einschränkungen anzuwenden, und dies ist wahrscheinlich die flexibelste Herangehensweise, da Sie zu Metaprogram hinauszögern wie sie angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="a6616-396">You can use the application model to apply constraints, and this is probably the most flexible approach as it allows you to metaprogram how they're applied.</span></span>

<span data-ttu-id="a6616-397">Im folgenden Beispiel wird eine Einschränkung wählt eine Aktion basierend auf einer *Landeskennzahl* von den Routendaten.</span><span class="sxs-lookup"><span data-stu-id="a6616-397">In the following example a constraint chooses an action based on a *country code* from the route data.</span></span> <span data-ttu-id="a6616-398">Die [vollständige Sample auf GitHub](https://github.com/aspnet/Entropy/blob/dev/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span><span class="sxs-lookup"><span data-stu-id="a6616-398">The [full sample on GitHub](https://github.com/aspnet/Entropy/blob/dev/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span></span>

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

<span data-ttu-id="a6616-399">Sie sind verantwortlich für die Implementierung der `Accept` -Methode und eine "Order" für die Einschränkung auszuführende auswählen.</span><span class="sxs-lookup"><span data-stu-id="a6616-399">You are responsible for implementing the `Accept` method and choosing an 'Order' for the constraint to execute.</span></span> <span data-ttu-id="a6616-400">In diesem Fall die `Accept` -Methode zurückkehrt `true` zu kennzeichnen, die Aktion ist eine Übereinstimmung bei der `country` route entspricht.</span><span class="sxs-lookup"><span data-stu-id="a6616-400">In this case, the `Accept` method returns `true` to denote the action is a match when the `country` route value matches.</span></span> <span data-ttu-id="a6616-401">Dies unterscheidet sich von einem `RouteValueAttribute` dahingehend, dass fallback auf eine Aktion nicht mit Attributen versehen können.</span><span class="sxs-lookup"><span data-stu-id="a6616-401">This is different from a `RouteValueAttribute` in that it allows fallback to a non-attributed action.</span></span> <span data-ttu-id="a6616-402">Das Beispiel zeigt, dass, wenn Sie definieren eine `en-US` Aktion und dann einen Ländercode wie `fr-FR` ausweichen auf einen generischeren Controller, der keine `[CountrySpecific(...)]` angewendet.</span><span class="sxs-lookup"><span data-stu-id="a6616-402">The sample shows that if you define an `en-US` action then a country code like `fr-FR` will fall back to a more generic controller that doesn't have `[CountrySpecific(...)]` applied.</span></span>

<span data-ttu-id="a6616-403">Die `Order` Eigenschaft entscheidet, in welchen *Phase* die Einschränkung gehört.</span><span class="sxs-lookup"><span data-stu-id="a6616-403">The `Order` property decides which *stage* the constraint is part of.</span></span> <span data-ttu-id="a6616-404">Aktionseinschränkungen führen Sie in Gruppen auf Grundlage der `Order`.</span><span class="sxs-lookup"><span data-stu-id="a6616-404">Action constraints run in groups based on the `Order`.</span></span> <span data-ttu-id="a6616-405">Z. B. alle das Framework bereitgestellt, verwenden Sie HTTP-Method-Attribute den gleichen `Order` Wert, sodass sie in der gleichen Stufe ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="a6616-405">For example, all of the framework provided HTTP method attributes use the same `Order` value so that they run in the same stage.</span></span> <span data-ttu-id="a6616-406">Sie können beliebig viele Stufen Sie die gewünschten Richtlinien implementieren müssen.</span><span class="sxs-lookup"><span data-stu-id="a6616-406">You can have as many stages as you need to implement your desired policies.</span></span>

> [!TIP]
> <span data-ttu-id="a6616-407">Um zu entscheiden, auf einen Wert für `Order` überlegen, ob die Einschränkung vor HTTP-Methoden angewendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="a6616-407">To decide on a value for `Order` think about whether or not your constraint should be applied before HTTP methods.</span></span> <span data-ttu-id="a6616-408">Niedrigere Zahlen werden zuerst ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="a6616-408">Lower numbers run first.</span></span>
