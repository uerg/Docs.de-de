---
title: Routing zu Controlleraktionen
author: rick-anderson
description: 
manager: wpickett
ms.author: riande
ms.date: 03/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/routing
ms.openlocfilehash: d87cb50871b956c51045558d2e4f076de4211f81
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
---
# <a name="routing-to-controller-actions"></a><span data-ttu-id="3f169-102">Routing zu Controlleraktionen</span><span class="sxs-lookup"><span data-stu-id="3f169-102">Routing to Controller Actions</span></span>

<span data-ttu-id="3f169-103">Von [Ryan Nowak](https://github.com/rynowak) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3f169-103">By [Ryan Nowak](https://github.com/rynowak) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3f169-104">ASP.NET Core MVC verwendet [Routing-Middleware](xref:fundamentals/middleware/index), um die URLs der eingehenden Anforderungen abzugleichen und sie Aktionen zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="3f169-104">ASP.NET Core MVC uses the Routing [middleware](xref:fundamentals/middleware/index) to match the URLs of incoming requests and map them to actions.</span></span> <span data-ttu-id="3f169-105">Routen werden im Startcode oder in Attributen definiert</span><span class="sxs-lookup"><span data-stu-id="3f169-105">Routes are defined in startup code or attributes.</span></span> <span data-ttu-id="3f169-106">und beschreiben, wie URL-Pfade Aktionen zugeordnet werden sollen.</span><span class="sxs-lookup"><span data-stu-id="3f169-106">Routes describe how URL paths should be matched to actions.</span></span> <span data-ttu-id="3f169-107">Sie werden auch dazu verwendet, um URLs (für Links) zu generieren, die in Antworten gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="3f169-107">Routes are also used to generate URLs (for links) sent out in responses.</span></span> 

<span data-ttu-id="3f169-108">Aktionen werden entweder herkömmlich oder über Attribute zugeordnet,</span><span class="sxs-lookup"><span data-stu-id="3f169-108">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="3f169-109">d.h., dass eine Route auf dem Controller oder der Aktion platziert wird.</span><span class="sxs-lookup"><span data-stu-id="3f169-109">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="3f169-110">Weitere Informationen finden Sie im Abschnitt [Gemischtes Routing](#routing-mixed-ref-label).</span><span class="sxs-lookup"><span data-stu-id="3f169-110">See [Mixed routing](#routing-mixed-ref-label) for more information.</span></span>

<span data-ttu-id="3f169-111">In diesem Artikel werden die Schnittstellen zwischen MVC und Routing und die Art und Weise erläutert, wie normale MVC-Apps Routingfeatures verwenden.</span><span class="sxs-lookup"><span data-stu-id="3f169-111">This document will explain the interactions between MVC and routing, and how typical MVC apps make use of routing features.</span></span> <span data-ttu-id="3f169-112">Informationen zum erweiterten Routing finden Sie unter [Routing in ASP.NET Core](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="3f169-112">See [Routing](xref:fundamentals/routing) for details on advanced routing.</span></span>

## <a name="setting-up-routing-middleware"></a><span data-ttu-id="3f169-113">Einrichten der Routing-Middleware</span><span class="sxs-lookup"><span data-stu-id="3f169-113">Setting up Routing Middleware</span></span>

<span data-ttu-id="3f169-114">Ihre *Configure*-Methode kann Code wie diesen enthalten:</span><span class="sxs-lookup"><span data-stu-id="3f169-114">In your *Configure* method you may see code similar to:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="3f169-115">Innerhalb des Aufrufs von `UseMvc` wird mit `MapRoute` eine einzelne Route erstellt, die wir `default`-Route nennen.</span><span class="sxs-lookup"><span data-stu-id="3f169-115">Inside the call to `UseMvc`, `MapRoute` is used to create a single route, which we'll refer to as the `default` route.</span></span> <span data-ttu-id="3f169-116">Die meisten MVC-Apps verwenden eine Route mit einer Vorlage, die der `default`-Route ähnelt.</span><span class="sxs-lookup"><span data-stu-id="3f169-116">Most MVC apps will use a route with a template similar to the `default` route.</span></span>

<span data-ttu-id="3f169-117">TDie Routenvorlage `"{controller=Home}/{action=Index}/{id?}"` kann einem URL-Pfad wie `/Products/Details/5` abgleichen und extrahiert die Routenwerte `{ controller = Products, action = Details, id = 5 }`, indem ein Token für den Pfad erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="3f169-117">The route template `"{controller=Home}/{action=Index}/{id?}"` can match a URL path like `/Products/Details/5` and will extract the route values `{ controller = Products, action = Details, id = 5 }` by tokenizing the path.</span></span> <span data-ttu-id="3f169-118">MVC versucht, einen Controller namens `ProductsController` zu suchen und die Aktion `Details` auszuführen:</span><span class="sxs-lookup"><span data-stu-id="3f169-118">MVC will attempt to locate a controller named `ProductsController` and run the action `Details`:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

<span data-ttu-id="3f169-119">In diesem Beispiel hat die Modellbindung den `id`-Parameter mit dem Wert `id = 5` beim Aufrufen dieser Aktion auf `5` festgelegt.</span><span class="sxs-lookup"><span data-stu-id="3f169-119">Note that in this example, model binding would use the value of `id = 5` to set the `id` parameter to `5` when invoking this action.</span></span> <span data-ttu-id="3f169-120">Weitere Informationen finden Sie unter [Modellbindung](../models/model-binding.md).</span><span class="sxs-lookup"><span data-stu-id="3f169-120">See the [Model Binding](../models/model-binding.md) for more details.</span></span>

<span data-ttu-id="3f169-121">Verwenden der `default`-Route:</span><span class="sxs-lookup"><span data-stu-id="3f169-121">Using the `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="3f169-122">Die Routenvorlage:</span><span class="sxs-lookup"><span data-stu-id="3f169-122">The route template:</span></span>

* <span data-ttu-id="3f169-123">`{controller=Home}` definiert `Home` als Standard-`controller`.</span><span class="sxs-lookup"><span data-stu-id="3f169-123">`{controller=Home}` defines `Home` as the default `controller`</span></span>

* <span data-ttu-id="3f169-124">`{action=Index}` definiert `Index` als Standard-`action`.</span><span class="sxs-lookup"><span data-stu-id="3f169-124">`{action=Index}` defines `Index` as the default `action`</span></span>

* <span data-ttu-id="3f169-125">`{id?}` definiert `id` als optional.</span><span class="sxs-lookup"><span data-stu-id="3f169-125">`{id?}` defines `id` as optional</span></span>

<span data-ttu-id="3f169-126">Standardmäßige und optionale Routenparameter müssen nicht im URL-Pfad vorhanden sein, damit es eine Übereinstimmung gibt.</span><span class="sxs-lookup"><span data-stu-id="3f169-126">Default and optional route parameters don't need to be present in the URL path for a match.</span></span> <span data-ttu-id="3f169-127">Eine ausführliche Beschreibung der Syntax der Routenvorlage finden Sie unter [Routenvorlagenreferenz](../../fundamentals/routing.md#route-template-reference).</span><span class="sxs-lookup"><span data-stu-id="3f169-127">See [Route Template Reference](../../fundamentals/routing.md#route-template-reference) for a detailed description of route template syntax.</span></span>

<span data-ttu-id="3f169-128">`"{controller=Home}/{action=Index}/{id?}"` gleicht den URL-Pfad `/` ab und erzeugt die Routenwerte `{ controller = Home, action = Index }`.</span><span class="sxs-lookup"><span data-stu-id="3f169-128">`"{controller=Home}/{action=Index}/{id?}"` can match the URL path `/` and will produce the route values `{ controller = Home, action = Index }`.</span></span> <span data-ttu-id="3f169-129">Die Werte für `controller` und `action` verwenden die Standardwerte. `id` erzeugt keine Werte, da kein entsprechendes Segment im URL-Pfad vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="3f169-129">The values for `controller` and `action` make use of the default values, `id` doesn't produce a value since there's no corresponding segment in the URL path.</span></span> <span data-ttu-id="3f169-130">MVC nutzt diese Routenwerte, um die `HomeController`- und `Index`-Aktion auszuwählen:</span><span class="sxs-lookup"><span data-stu-id="3f169-130">MVC would use these route values to select the `HomeController` and `Index` action:</span></span>

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

<span data-ttu-id="3f169-131">Mit dieser Controllerdefinition und dieser Routenvorlage wird die `HomeController.Index`-Aktion für jeden der folgenden URL-Pfade ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="3f169-131">Using this controller definition and route template, the `HomeController.Index` action would be executed for any of the following URL paths:</span></span>

* `/Home/Index/17`

* `/Home/Index`

* `/Home`

* `/`

<span data-ttu-id="3f169-132">Mit der Hilfsmethode `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="3f169-132">The convenience method `UseMvcWithDefaultRoute`:</span></span>

```csharp
app.UseMvcWithDefaultRoute();
```

<span data-ttu-id="3f169-133">kann Folgendes ersetzt werden:</span><span class="sxs-lookup"><span data-stu-id="3f169-133">Can be used to replace:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="3f169-134">`UseMvc` und `UseMvcWithDefaultRoute` fügen der Middleware-Pipeline eine Instanz von `RouterMiddleware` hinzu.</span><span class="sxs-lookup"><span data-stu-id="3f169-134">`UseMvc` and `UseMvcWithDefaultRoute` add an instance of `RouterMiddleware` to the middleware pipeline.</span></span> <span data-ttu-id="3f169-135">MVC interagiert nicht direkt mit der Middleware und verarbeitet Anforderungen mithilfe von Routing.</span><span class="sxs-lookup"><span data-stu-id="3f169-135">MVC doesn't interact directly with middleware, and uses routing to handle requests.</span></span> <span data-ttu-id="3f169-136">MVC ist über eine Instanz von `MvcRouteHandler` mit den Routen verbunden.</span><span class="sxs-lookup"><span data-stu-id="3f169-136">MVC is connected to the routes through an instance of `MvcRouteHandler`.</span></span> <span data-ttu-id="3f169-137">Der Code in `UseMvc` ähnelt Folgendem:</span><span class="sxs-lookup"><span data-stu-id="3f169-137">The code inside of `UseMvc` is similar to the following:</span></span>

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

<span data-ttu-id="3f169-138">`UseMvc` definiert keine Routen direkt, sondern fügt der Routenauflistung einen Platzhalter für die `attribute`-Route hinzu.</span><span class="sxs-lookup"><span data-stu-id="3f169-138">`UseMvc` doesn't directly define any routes, it adds a placeholder to the route collection for the `attribute` route.</span></span> <span data-ttu-id="3f169-139">Die Überladung `UseMvc(Action<IRouteBuilder>)` ermöglicht es Ihnen, Ihre eigenen Routen hinzuzufügen und unterstützt darüber hinaus das Routingattribut.</span><span class="sxs-lookup"><span data-stu-id="3f169-139">The overload `UseMvc(Action<IRouteBuilder>)` lets you add your own routes and also supports attribute routing.</span></span>  <span data-ttu-id="3f169-140">`UseMvc` und alle seine Varianten fügen einen Platzhalter für die Attributroute hinzu. Attributrouting ist immer verfügbar, unabhängig von der Konfiguration von `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="3f169-140">`UseMvc` and all of its variations adds a placeholder for the attribute route - attribute routing is always available regardless of how you configure `UseMvc`.</span></span> <span data-ttu-id="3f169-141">`UseMvcWithDefaultRoute` definiert eine Standardroute und unterstützt Attributrouting.</span><span class="sxs-lookup"><span data-stu-id="3f169-141">`UseMvcWithDefaultRoute` defines a default route and supports attribute routing.</span></span> <span data-ttu-id="3f169-142">Der Abschnitt [Attributrouting](#attribute-routing-ref-label) enthält weitere Informationen zu dem Thema.</span><span class="sxs-lookup"><span data-stu-id="3f169-142">The [Attribute Routing](#attribute-routing-ref-label) section includes more details on attribute routing.</span></span>

<a name="routing-conventional-ref-label"></a>

## <a name="conventional-routing"></a><span data-ttu-id="3f169-143">Herkömmliches Routing</span><span class="sxs-lookup"><span data-stu-id="3f169-143">Conventional routing</span></span>

<span data-ttu-id="3f169-144">Die `default`-Route:</span><span class="sxs-lookup"><span data-stu-id="3f169-144">The `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="3f169-145">ist ein Beispiel für *herkömmliches Routing*.</span><span class="sxs-lookup"><span data-stu-id="3f169-145">is an example of a *conventional routing*.</span></span> <span data-ttu-id="3f169-146">Das *herkömmliche Routing* heißt so, weil dabei eine *Konvention* für URL-Pfade erstellt wird:</span><span class="sxs-lookup"><span data-stu-id="3f169-146">We call this style *conventional routing* because it establishes a *convention* for URL paths:</span></span>

* <span data-ttu-id="3f169-147">Das erste Pfadsegment entspricht dem Namen des Controllers.</span><span class="sxs-lookup"><span data-stu-id="3f169-147">the first path segment maps to the controller name</span></span>

* <span data-ttu-id="3f169-148">Das zweite entspricht dem Aktionsnamen.</span><span class="sxs-lookup"><span data-stu-id="3f169-148">the second maps to the action name.</span></span>

* <span data-ttu-id="3f169-149">Das dritte Segment wird für eine optionale `id` verwendet, mit der eine Zuordnung zu einer Modellentität vorgenommen wird.</span><span class="sxs-lookup"><span data-stu-id="3f169-149">the third segment is used for an optional `id` used to map to a model entity</span></span>

<span data-ttu-id="3f169-150">Mit dieser `default`-Route wir der URL-Pfad `/Products/List` der `ProductsController.List`-Aktion und `/Blog/Article/17` `BlogController.Article` zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="3f169-150">Using this `default` route, the URL path `/Products/List` maps to the `ProductsController.List` action, and `/Blog/Article/17` maps to `BlogController.Article`.</span></span> <span data-ttu-id="3f169-151">Diese Zuordnung basiert **ausschließlich** auf den Controller- und Aktionsnamen und nicht auf Namespaces, Speicherorten für Quelldateien oder Methodenparametern.</span><span class="sxs-lookup"><span data-stu-id="3f169-151">This mapping is based on the controller and action names **only** and isn't based on namespaces, source file locations, or method parameters.</span></span>

> [!TIP]
> <span data-ttu-id="3f169-152">Die Kombination aus herkömmlichem Routing und Standardrouting ermöglicht es Ihnen, Anwendungen schnell zu erstellen, ohne für jede definierte Aktion ein neues URL-Muster entwerfen zu müssen.</span><span class="sxs-lookup"><span data-stu-id="3f169-152">Using conventional routing with the default route allows you to build the application quickly without having to come up with a new URL pattern for each action you define.</span></span> <span data-ttu-id="3f169-153">Bei Anwendungen mit Aktionen im CRUD-Stil können einheitliche URLs auf allen Controllern dabei helfen, den Code zu vereinfachen und die UI vorhersehbarer zu machen.</span><span class="sxs-lookup"><span data-stu-id="3f169-153">For an application with CRUD style actions, having consistency for the URLs across your controllers can help simplify your code and make your UI more predictable.</span></span>

> [!WARNING]
> <span data-ttu-id="3f169-154">`id` wird von der Routenvorlage als optional definiert, was bedeutet, dass Ihre Aktionen ausgeführt werden, ohne dass die ID als Teil der URL bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="3f169-154">The `id` is defined as optional by the route template, meaning that your actions can execute without the ID provided as part of the URL.</span></span> <span data-ttu-id="3f169-155">Wenn `id` in der URL ausgelassen wird, wird es üblicherweise von der Modellbindung auf `0` festgelegt, und daher wird in der Datenbank keine Entität gefunden, die `id == 0` entspricht.</span><span class="sxs-lookup"><span data-stu-id="3f169-155">Usually what will happen if `id` is omitted from the URL is that it will be set to `0` by model binding, and as a result no entity will be found in the database matching `id == 0`.</span></span> <span data-ttu-id="3f169-156">Mit dem Attributrouting können Sie präzise steuern, für welche Aktionen die ID erforderlich ist und für welche nicht.</span><span class="sxs-lookup"><span data-stu-id="3f169-156">Attribute routing can give you fine-grained control to make the ID required for some actions and not for others.</span></span> <span data-ttu-id="3f169-157">Gemäß der Konvention enthält die Dokumentation optionale Parameter wie `id`, wenn sie wahrscheinlich in der richtigen Verwendung angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="3f169-157">By convention the documentation will include optional parameters like `id` when they're likely to appear in correct usage.</span></span>

## <a name="multiple-routes"></a><span data-ttu-id="3f169-158">Mehrere Routen</span><span class="sxs-lookup"><span data-stu-id="3f169-158">Multiple routes</span></span>

<span data-ttu-id="3f169-159">Sie können mehrere Routen innerhalb von `UseMvc` hinzufügen, indem Sie weitere Aufrufe von `MapRoute` hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="3f169-159">You can add multiple routes inside `UseMvc` by adding more calls to `MapRoute`.</span></span> <span data-ttu-id="3f169-160">So können Sie mehrere Konventionen definieren oder herkömmlichen Routen hinzufügen, die einer bestimmten Aktion zugeordnet sind, z.B.:</span><span class="sxs-lookup"><span data-stu-id="3f169-160">Doing so allows you to define multiple conventions, or to add conventional routes that are dedicated to a specific action, such as:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="3f169-161">Die `blog`-Route hier ist eine *dedizierte herkömmliche Route*, d.h., dass das herkömmliche Routingsystem verwendet wird, aber einer bestimmten Aktion zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="3f169-161">The `blog` route here is a *dedicated conventional route*, meaning that it uses the conventional routing system, but is dedicated to a specific action.</span></span> <span data-ttu-id="3f169-162">Da `controller` und `action` nicht als Parameter in der Routenvorlage vorkommen, können sie nur die Standardwerte haben. Daher wird diese Route immer der Aktion `BlogController.Article` zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="3f169-162">Since `controller` and `action` don't appear in the route template as parameters, they can only have the default values, and thus this route will always map to the action `BlogController.Article`.</span></span>

<span data-ttu-id="3f169-163">Die Routen in der Routenauflistung sind geordnet und werden in der Reihenfolge verarbeitet, in der sie hinzugefügt wurden.</span><span class="sxs-lookup"><span data-stu-id="3f169-163">Routes in the route collection are ordered, and will be processed in the order they're added.</span></span> <span data-ttu-id="3f169-164">In diesem Beispiel wird daher die Route `blog` vor der Route `default` überprüft.</span><span class="sxs-lookup"><span data-stu-id="3f169-164">So in this example, the `blog` route will be tried before the `default` route.</span></span>

> [!NOTE]
> <span data-ttu-id="3f169-165">*Dedizierte herkömmliche Routen* verwenden oft Catchall-Routenparameter wie `{*article}`, um den verbleibenden Teil des URL-Pfads zu erfassen.</span><span class="sxs-lookup"><span data-stu-id="3f169-165">*Dedicated conventional routes* often use catch-all route parameters like `{*article}` to capture the remaining portion of the URL path.</span></span> <span data-ttu-id="3f169-166">Dies kann eine Route „zu gierig“ machen, d.h., dass sie alle URLs abgleicht, die eigentlich von anderen Routen abgeglichen werden sollten.</span><span class="sxs-lookup"><span data-stu-id="3f169-166">This can make a route 'too greedy' meaning that it matches URLs that you intended to be matched by other routes.</span></span> <span data-ttu-id="3f169-167">Fügen Sie die „gierigen“ Routen weiter unten in die Routingtabelle ein, um dieses Problem zu beheben.</span><span class="sxs-lookup"><span data-stu-id="3f169-167">Put the 'greedy' routes later in the route table to solve this.</span></span>

### <a name="fallback"></a><span data-ttu-id="3f169-168">Fallback</span><span class="sxs-lookup"><span data-stu-id="3f169-168">Fallback</span></span>

<span data-ttu-id="3f169-169">Im Rahmen der Anforderungsverarbeitung überprüft MVC, ob mit den Routenwerten ein Controller und eine Aktion in Ihrer Anwendung gefunden werden können.</span><span class="sxs-lookup"><span data-stu-id="3f169-169">As part of request processing, MVC will verify that the route values can be used to find a controller and action in your application.</span></span> <span data-ttu-id="3f169-170">Falls die Routenwerte mit keiner Aktion übereinstimmen, gilt die Route nicht als Übereinstimmung, und die nächste Route wird überprüft.</span><span class="sxs-lookup"><span data-stu-id="3f169-170">If the route values don't match an action then the route isn't considered a match, and the next route will be tried.</span></span> <span data-ttu-id="3f169-171">Dabei spricht man von einem *Fallback*. Dieser Vorgang soll Szenarios vereinfachen, bei denen sich herkömmliche Routen überschneiden.</span><span class="sxs-lookup"><span data-stu-id="3f169-171">This is called *fallback*, and it's intended to simplify cases where conventional routes overlap.</span></span>

### <a name="disambiguating-actions"></a><span data-ttu-id="3f169-172">Aktionen eindeutig zuordnen</span><span class="sxs-lookup"><span data-stu-id="3f169-172">Disambiguating actions</span></span>

<span data-ttu-id="3f169-173">Wenn zwei Aktionen beim Routing übereinstimmen, muss MVC beide analysieren und die beste auswählen oder eine Ausnahme auslösen.</span><span class="sxs-lookup"><span data-stu-id="3f169-173">When two actions match through routing, MVC must disambiguate to choose the 'best' candidate or else throw an exception.</span></span> <span data-ttu-id="3f169-174">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="3f169-174">For example:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

<span data-ttu-id="3f169-175">Dieser Controller definiert zwei Aktionen, die mit dem URL-Pfad `/Products/Edit/17` übereinstimmen und die Daten `{ controller = Products, action = Edit, id = 17 }` weiterleiten.</span><span class="sxs-lookup"><span data-stu-id="3f169-175">This controller defines two actions that would match the URL path `/Products/Edit/17` and route data `{ controller = Products, action = Edit, id = 17 }`.</span></span> <span data-ttu-id="3f169-176">Dies ist ein typisches Muster für MVC-Controller, in dem `Edit(int)` ein Formular zum Bearbeiten eines Produkts anzeigt und `Edit(int, Product)` das bereitgestellte Formular verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="3f169-176">This is a typical pattern for MVC controllers where `Edit(int)` shows a form to edit a product, and `Edit(int, Product)` processes  the posted form.</span></span> <span data-ttu-id="3f169-177">Um dies zu ermöglichen, muss MVC `Edit(int, Product)` auswählen, wenn die Anforderung HTTP-`POST` ist, und `Edit(int)`, wenn das HTTP-Verb ein anderes ist.</span><span class="sxs-lookup"><span data-stu-id="3f169-177">To make this possible MVC would need to choose `Edit(int, Product)` when the request is an HTTP `POST` and `Edit(int)` when the HTTP verb is anything else.</span></span>

<span data-ttu-id="3f169-178">`HttpPostAttribute` (`[HttpPost]`) ist eine Implementierung von `IActionConstraint`, bei der die Aktion nur ausgewählt werden darf, wenn das HTTP-Verb `POST` ist.</span><span class="sxs-lookup"><span data-stu-id="3f169-178">The `HttpPostAttribute` ( `[HttpPost]` ) is an implementation of `IActionConstraint` that will only allow the action to be selected when the HTTP verb is `POST`.</span></span> <span data-ttu-id="3f169-179">Aufgrund des Vorhandenseins von `IActionConstraint` ist `Edit(int, Product)` die „bessere“ Übereinstimmung als `Edit(int)`, sodass `Edit(int, Product)` zuerst überprüft wird.</span><span class="sxs-lookup"><span data-stu-id="3f169-179">The presence of an `IActionConstraint` makes the `Edit(int, Product)` a 'better' match than `Edit(int)`, so `Edit(int, Product)` will be tried first.</span></span>

<span data-ttu-id="3f169-180">Benutzerdefinierte `IActionConstraint`-Implementierungen müssen Sie nur in speziellen Szenarios schreiben. Es ist jedoch wichtig, die Rolle von Attributen wie `HttpPostAttribute` zu kennen, denn ähnliche Attribute sind auch für andere HTTP-Verben definiert.</span><span class="sxs-lookup"><span data-stu-id="3f169-180">You will only need to write custom `IActionConstraint` implementations in specialized scenarios, but it's important to understand the role of attributes like `HttpPostAttribute`  - similar attributes are defined for other HTTP verbs.</span></span> <span data-ttu-id="3f169-181">Beim herkömmlichen Routing nutzen Aktionen oft denselben Aktionsnamen, wenn sie Teil eines `show form -> submit form`-Workflows sind.</span><span class="sxs-lookup"><span data-stu-id="3f169-181">In conventional routing it's common for actions to use the same action name when they're part of a `show form -> submit form` workflow.</span></span> <span data-ttu-id="3f169-182">Die Vorteile dessen werden deutlicher, wenn Sie den Abschnitt [Verstehen von IActionConstraint](#understanding-iactionconstraint) gelesen haben.</span><span class="sxs-lookup"><span data-stu-id="3f169-182">The convenience of this pattern will become more apparent after reviewing the [Understanding IActionConstraint](#understanding-iactionconstraint) section.</span></span>

<span data-ttu-id="3f169-183">Wenn mehrere Routen übereinstimmen und MVC die „beste“ nicht bestimmen kann, löst es eine `AmbiguousActionException` aus.</span><span class="sxs-lookup"><span data-stu-id="3f169-183">If multiple routes match, and MVC can't find a 'best' route, it will throw an `AmbiguousActionException`.</span></span>

<a name="routing-route-name-ref-label"></a>

### <a name="route-names"></a><span data-ttu-id="3f169-184">Routennamen</span><span class="sxs-lookup"><span data-stu-id="3f169-184">Route names</span></span>

<span data-ttu-id="3f169-185">Die Zeichenfolgen `"blog"` und `"default"` in den folgenden Beispielen sind Routennamen:</span><span class="sxs-lookup"><span data-stu-id="3f169-185">The strings  `"blog"` and `"default"` in the following examples are route names:</span></span>


```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
               defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="3f169-186">Der Routenname gibt der Route einen logischen Namen, damit die benannte Route bei der URL-Generierung verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="3f169-186">The route names give the route a logical name so that the named route can be used for URL generation.</span></span> <span data-ttu-id="3f169-187">Dadurch wird die URL-Erstellung erheblich vereinfacht, obwohl die Reihenfolge der Routen die URL-Generierung verkomplizieren sollte.</span><span class="sxs-lookup"><span data-stu-id="3f169-187">This greatly simplifies URL creation when the ordering of routes could make URL generation complicated.</span></span> <span data-ttu-id="3f169-188">Routennamen müssen anwendungsweit eindeutig sein.</span><span class="sxs-lookup"><span data-stu-id="3f169-188">Route names must be unique application-wide.</span></span>

<span data-ttu-id="3f169-189">Routennamen haben keine Auswirkung auf die URL-Zuordnung oder die Verarbeitung von Anforderungen. Sie dienen nur der URL-Generierung.</span><span class="sxs-lookup"><span data-stu-id="3f169-189">Route names have no impact on URL matching or handling of requests; they're used only for URL generation.</span></span> <span data-ttu-id="3f169-190">Unter [Routing](xref:fundamentals/routing) finden Sie weitere ausführliche Informationen zur URL-Generierung, einschließlich der Generierung in MVC-spezifischen Hilfsprogrammen.</span><span class="sxs-lookup"><span data-stu-id="3f169-190">[Routing](xref:fundamentals/routing) has more detailed information on URL generation including URL generation in MVC-specific helpers.</span></span>

<a name="attribute-routing-ref-label"></a>

## <a name="attribute-routing"></a><span data-ttu-id="3f169-191">Attributrouting</span><span class="sxs-lookup"><span data-stu-id="3f169-191">Attribute routing</span></span>

<span data-ttu-id="3f169-192">Beim Attributrouting werden Aktionen mithilfe von Attributen direkt Routenvorlagen zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="3f169-192">Attribute routing uses a set of attributes to map actions directly to route templates.</span></span> <span data-ttu-id="3f169-193">Im folgenden Beispiel wird `app.UseMvc();` in der `Configure`-Methode verwendet, und es wird keine Route übergeben.</span><span class="sxs-lookup"><span data-stu-id="3f169-193">In the following example, `app.UseMvc();` is used in the `Configure` method and no route is passed.</span></span> <span data-ttu-id="3f169-194">`HomeController` entspricht URLs, die denen ähneln, denen die Standardroute `{controller=Home}/{action=Index}/{id?}` entsprechen würde:</span><span class="sxs-lookup"><span data-stu-id="3f169-194">The `HomeController` will match a set of URLs similar to what the default route `{controller=Home}/{action=Index}/{id?}` would match:</span></span>

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

<span data-ttu-id="3f169-195">Die `HomeController.Index()`-Aktion wird für die URL-Pfade `/`, `/Home` und `/Home/Index` jeweils ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="3f169-195">The `HomeController.Index()` action will be executed for any of the URL paths `/`, `/Home`, or `/Home/Index`.</span></span>

> [!NOTE]
> <span data-ttu-id="3f169-196">In diesem Beispiel wird ein wichtiger Unterschied beim Programmieren zwischen dem Attributrouting und dem herkömmlichen Routing hervorgehoben.</span><span class="sxs-lookup"><span data-stu-id="3f169-196">This example highlights a key programming difference between attribute routing and conventional routing.</span></span> <span data-ttu-id="3f169-197">Attributrouting erfordert mehr Eingabe, um eine Route anzugeben. Die herkömmliche Standardroute verarbeitet auch kürzere Routen.</span><span class="sxs-lookup"><span data-stu-id="3f169-197">Attribute routing requires more input to specify a route; the conventional default route handles routes more succinctly.</span></span> <span data-ttu-id="3f169-198">Attributrouting ermöglicht jedoch (und erfordert auch) die präzise Kontrolle der Routenvorlagen, die für die einzelnen Aktionen gelten.</span><span class="sxs-lookup"><span data-stu-id="3f169-198">However, attribute routing allows (and requires) precise control of which route templates apply to each action.</span></span>

<span data-ttu-id="3f169-199">Beim Attributrouting haben der Controller- und der Aktionsname **keine** Auswirkung darauf, welche Aktion ausgewählt wird.</span><span class="sxs-lookup"><span data-stu-id="3f169-199">With attribute routing the controller name and action names play **no** role in which action is selected.</span></span> <span data-ttu-id="3f169-200">In diesem Beispiel werden dieselben URLs wie im vorherigen Beispiel abgeglichen.</span><span class="sxs-lookup"><span data-stu-id="3f169-200">This example will match the same URLs as the previous example.</span></span>

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
> <span data-ttu-id="3f169-201">Die oben genannten Routenvorlagen definieren keine Routenparameter für `action`, `area` und `controller`.</span><span class="sxs-lookup"><span data-stu-id="3f169-201">The route templates above don't define route parameters for `action`, `area`, and `controller`.</span></span> <span data-ttu-id="3f169-202">Diese Routenparameter sind in Attributrouten nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="3f169-202">In fact, these route parameters are not allowed in attribute routes.</span></span> <span data-ttu-id="3f169-203">Da die Routenvorlage bereits einer Aktion zugeordnet ist, wäre es nicht sinnvoll, den Aktionsnamen aus der URL zu analysieren.</span><span class="sxs-lookup"><span data-stu-id="3f169-203">Since the route template is already associated with an action, it wouldn't make sense to parse the action name from the URL.</span></span>

## <a name="attribute-routing-with-httpverb-attributes"></a><span data-ttu-id="3f169-204">Attributrouting mit Http[Verb]-Attributen</span><span class="sxs-lookup"><span data-stu-id="3f169-204">Attribute routing with Http[Verb] attributes</span></span>

<span data-ttu-id="3f169-205">Beim Attributrouting können auch `Http[Verb]`-Attribute wie `HttpPostAttribute` verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="3f169-205">Attribute routing can also make use of the `Http[Verb]` attributes such as `HttpPostAttribute`.</span></span> <span data-ttu-id="3f169-206">Alle diese Attribute können eine Routenvorlage akzeptieren.</span><span class="sxs-lookup"><span data-stu-id="3f169-206">All of these attributes can accept a route template.</span></span> <span data-ttu-id="3f169-207">In diesem Beispiel werden zwei Aktionen gezeigt, die derselben Routenvorlage zugeordnet sind:</span><span class="sxs-lookup"><span data-stu-id="3f169-207">This example shows two actions that match the same route template:</span></span>

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

<span data-ttu-id="3f169-208">Für einen URL-Pfad wie `/products` wird die Aktion `ProductsApi.ListProducts` ausgeführt, wenn das HTTP-Verb `GET` ist, und `ProductsApi.CreateProduct` wird ausgeführt, wenn das HTTP-Verb `POST` entspricht.</span><span class="sxs-lookup"><span data-stu-id="3f169-208">For a URL path like `/products` the `ProductsApi.ListProducts` action will be executed when the HTTP verb is `GET` and `ProductsApi.CreateProduct` will be executed when the HTTP verb is `POST`.</span></span> <span data-ttu-id="3f169-209">Attributrouting gleicht zuerst die URL gegen die Routenvorlagen ab, die von den Routenattributen definiert werden.</span><span class="sxs-lookup"><span data-stu-id="3f169-209">Attribute routing first matches the URL against the set of route templates defined by route attributes.</span></span> <span data-ttu-id="3f169-210">Sobald eine Routenvorlage übereinstimmt, werden `IActionConstraint`-Einschränkungen angewendet, um zu bestimmen, welche Aktionen ausgeführt werden können.</span><span class="sxs-lookup"><span data-stu-id="3f169-210">Once a route template matches, `IActionConstraint` constraints are applied to determine which actions can be executed.</span></span>

> [!TIP]
> <span data-ttu-id="3f169-211">Beim Erstellen einer REST-API wird `[Route(...)]` selten für eine Aktionsmethode verwendet.</span><span class="sxs-lookup"><span data-stu-id="3f169-211">When building a REST API, it's rare that you will want to use `[Route(...)]` on an action method.</span></span> <span data-ttu-id="3f169-212">Es ist besser, das spezifischere `Http*Verb*Attributes` zu nutzen, um präzise anzugeben, was Ihre API unterstützt.</span><span class="sxs-lookup"><span data-stu-id="3f169-212">It's better to use the more specific `Http*Verb*Attributes` to be precise about what your API supports.</span></span> <span data-ttu-id="3f169-213">REST-API-Clients sollten wissen, welche Pfade und HTTP-Verben bestimmten logischen Operationen entsprechen.</span><span class="sxs-lookup"><span data-stu-id="3f169-213">Clients of REST APIs are expected to know what paths and HTTP verbs map to specific logical operations.</span></span>

<span data-ttu-id="3f169-214">Da eine Attributroute für eine bestimmte Aktion gilt, ist es einfach, Parameter als Teil der Routenvorlagendefinition erforderlich festzulegen.</span><span class="sxs-lookup"><span data-stu-id="3f169-214">Since an attribute route applies to a specific action, it's easy to make parameters required as part of the route template definition.</span></span> <span data-ttu-id="3f169-215">In diesem Beispiel ist `id` als Teil des URL-Pfads erforderlich.</span><span class="sxs-lookup"><span data-stu-id="3f169-215">In this example, `id` is required as part of the URL path.</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="3f169-216">Die `ProductsApi.GetProduct(int)`-Aktion wird für einen URL-Pfad wie `/products/3` ausgeführt, jedoch nicht für einen URL-Pfad wie `/products`.</span><span class="sxs-lookup"><span data-stu-id="3f169-216">The `ProductsApi.GetProduct(int)` action will be executed for a URL path like `/products/3` but not for a URL path like `/products`.</span></span> <span data-ttu-id="3f169-217">Eine vollständige Beschreibung und Routenvorlagen und dazugehörige Optionen finden Sie unter [Routing in ASP.NET Core](../../fundamentals/routing.md).</span><span class="sxs-lookup"><span data-stu-id="3f169-217">See [Routing](../../fundamentals/routing.md) for a full description of route templates and related options.</span></span>

## <a name="route-name"></a><span data-ttu-id="3f169-218">Routenname</span><span class="sxs-lookup"><span data-stu-id="3f169-218">Route Name</span></span>

<span data-ttu-id="3f169-219">Im folgenden Code wird ein *Routenname* von `Products_List` definiert:</span><span class="sxs-lookup"><span data-stu-id="3f169-219">The following code  defines a *route name* of `Products_List`:</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="3f169-220">Routennamen können verwendet werden, um basierend auf einer bestimmten Route eine URL zu generieren.</span><span class="sxs-lookup"><span data-stu-id="3f169-220">Route names can be used to generate a URL based on a specific route.</span></span> <span data-ttu-id="3f169-221">Routennamen haben keine Auswirkung auf das URL-Zuordnungsverhalten des Routings und dienen nur zur URL-Generierung.</span><span class="sxs-lookup"><span data-stu-id="3f169-221">Route names have no impact on the URL matching behavior of routing and are only used for URL generation.</span></span> <span data-ttu-id="3f169-222">Routennamen müssen anwendungsweit eindeutig sein.</span><span class="sxs-lookup"><span data-stu-id="3f169-222">Route names must be unique application-wide.</span></span>

> [!NOTE]
> <span data-ttu-id="3f169-223">Vergleichen Sie dies mit der herkömmlichen *Standardroute*, die den `id`-Parameter als optional definiert (`{id?}`).</span><span class="sxs-lookup"><span data-stu-id="3f169-223">Contrast this with the conventional *default route*, which defines the `id` parameter as optional (`{id?}`).</span></span> <span data-ttu-id="3f169-224">APIs präzise angeben zu können, hat Vorteile, z.B. können `/products` und `/products/5` an unterschiedliche Aktionen gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="3f169-224">This ability to precisely specify APIs has advantages, such as  allowing `/products` and `/products/5` to be dispatched to different actions.</span></span>

<a name="routing-combining-ref-label"></a>

### <a name="combining-routes"></a><span data-ttu-id="3f169-225">Kombinieren von Routen</span><span class="sxs-lookup"><span data-stu-id="3f169-225">Combining routes</span></span>

<span data-ttu-id="3f169-226">Um Attributrouting weniger repetitiv zu gestalten, werden Routenattribute auf dem Controller mit Routenattributen auf den einzelnen Aktionen kombiniert.</span><span class="sxs-lookup"><span data-stu-id="3f169-226">To make attribute routing less repetitive, route attributes on the controller are combined with route attributes on the individual actions.</span></span> <span data-ttu-id="3f169-227">Alle auf dem Controller definierten Routenvorlagen werden den Routenvorlagen auf den Aktionen vorangestellt.</span><span class="sxs-lookup"><span data-stu-id="3f169-227">Any route templates defined on the controller are prepended to route templates on the actions.</span></span> <span data-ttu-id="3f169-228">Wenn Routenattribute auf dem Controller platziert werden, verwenden **alle** Aktionen im Controller Attributrouting.</span><span class="sxs-lookup"><span data-stu-id="3f169-228">Placing a route attribute on the controller makes **all** actions in the controller use attribute routing.</span></span>

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

<span data-ttu-id="3f169-229">In diesem Beispiel kann der URL-Pfad `/products` `ProductsApi.ListProducts` und der URL-Pfad `/products/5` `ProductsApi.GetProduct(int)` zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="3f169-229">In this example the URL path `/products` can match `ProductsApi.ListProducts`, and the URL path `/products/5` can match `ProductsApi.GetProduct(int)`.</span></span> <span data-ttu-id="3f169-230">Beide dieser Aktionen entsprechen nur HTTP `GET`, da sie mit `HttpGetAttribute` ausgestattet sind.</span><span class="sxs-lookup"><span data-stu-id="3f169-230">Both of these actions only match HTTP `GET` because they're decorated with the `HttpGetAttribute`.</span></span>

<span data-ttu-id="3f169-231">Routenvorlagen, die auf eine Aktion angewendet werden, die mit einem `/` beginnen, können nicht mit Routenvorlagen kombiniert werden, die auf den Controller angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="3f169-231">Route templates applied to an action that begin with a `/` don't get combined with route templates applied to the controller.</span></span> <span data-ttu-id="3f169-232">In diesem Beispiel werden mehrere URL-Pfade zugeordnet, die der *Standardroute* ähneln.</span><span class="sxs-lookup"><span data-stu-id="3f169-232">This example matches a set of URL paths similar to the *default route*.</span></span>

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

### <a name="ordering-attribute-routes"></a><span data-ttu-id="3f169-233">Ordnen der Attributrouten</span><span class="sxs-lookup"><span data-stu-id="3f169-233">Ordering attribute routes</span></span>

<span data-ttu-id="3f169-234">Im Gegensatz zu herkömmlichen Routen, die in einer definierten Reihenfolge ausgeführt werden, wird beim Attributrouting eine Struktur erstellt, und alle Routen werden gleichzeitig zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="3f169-234">In contrast to conventional routes which execute in a defined order, attribute routing builds a tree and matches all routes simultaneously.</span></span> <span data-ttu-id="3f169-235">Der Vorgang wird also ausgeführt, als ob die Routeneinträge in der idealen Reihenfolge platziert worden wären: Die spezifischsten Routen können also vor den allgemeineren ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="3f169-235">This behaves as-if the route entries were placed in an ideal ordering; the most specific routes have a chance to execute before the more general routes.</span></span>

<span data-ttu-id="3f169-236">Eine Route wie `blog/search/{topic}` ist z.B. spezifischer als eine Route wie `blog/{*article}`.</span><span class="sxs-lookup"><span data-stu-id="3f169-236">For example, a route like `blog/search/{topic}` is more specific than a route like `blog/{*article}`.</span></span> <span data-ttu-id="3f169-237">Logisch gesehen wird die Route `blog/search/{topic}` standardmäßig zuerst ausgeführt, da dies die einzig sinnvolle Reihenfolge ist.</span><span class="sxs-lookup"><span data-stu-id="3f169-237">Logically speaking the `blog/search/{topic}` route 'runs' first, by default, because that's the only sensible ordering.</span></span> <span data-ttu-id="3f169-238">Beim herkömmlichen Routing ist der Entwickler verantwortlich dafür, die Routen in die gewünschte Reihenfolge zu bringen.</span><span class="sxs-lookup"><span data-stu-id="3f169-238">Using conventional routing, the developer is  responsible for placing routes in the desired order.</span></span>

<span data-ttu-id="3f169-239">Attributrouten können mithilfe der `Order`-Eigenschaft aller vom Framework bereitgestellten Routenattribute eine Reihenfolge konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="3f169-239">Attribute routes can configure an order, using the `Order` property of all of the framework provided route attributes.</span></span> <span data-ttu-id="3f169-240">Routen werden entsprechend einer aufsteigenden Reihenfolge der `Order`-Eigenschaft verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="3f169-240">Routes are processed according to an ascending sort of the `Order` property.</span></span> <span data-ttu-id="3f169-241">Die Standardreihenfolge ist `0`.</span><span class="sxs-lookup"><span data-stu-id="3f169-241">The default order is `0`.</span></span> <span data-ttu-id="3f169-242">Wird für eine Route `Order = -1` festgelegt, wird sie vor denjenigen Routen ausgeführt, für die keine Reihenfolge festgelegt wurde.</span><span class="sxs-lookup"><span data-stu-id="3f169-242">Setting a route using `Order = -1` will run before routes that don't set an order.</span></span> <span data-ttu-id="3f169-243">Wird für eine Route `Order = 1` festgelegt, wird sie ausgeführt, nachdem die Standardroutenreihenfolge ausgeführt wurde.</span><span class="sxs-lookup"><span data-stu-id="3f169-243">Setting a route using `Order = 1` will run after default route ordering.</span></span>

> [!TIP]
> <span data-ttu-id="3f169-244">Vermeiden Sie eine Abhängigkeit von `Order`.</span><span class="sxs-lookup"><span data-stu-id="3f169-244">Avoid depending on `Order`.</span></span> <span data-ttu-id="3f169-245">Wenn der URL-Raum explizite Reihenfolgenwerte erfordert, um korrekt weiterzuleiten, ist es wahrscheinlich auch für Clients verwirrend.</span><span class="sxs-lookup"><span data-stu-id="3f169-245">If your URL-space requires explicit order values to route correctly, then it's likely confusing to clients as well.</span></span> <span data-ttu-id="3f169-246">Beim Attributrouting wird im Allgemeinen mithilfe der URL-Zuordnung die richtige Route ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="3f169-246">In general attribute routing will select the correct route with URL matching.</span></span> <span data-ttu-id="3f169-247">Wenn die für die URL-Generierung verwendete Standardreihenfolge nicht funktioniert, ist es meist einfacher, Routennamen als Außerkraftsetzung zu verwenden, statt die `Order`-Eigenschaft anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="3f169-247">If the default order used for URL generation isn't working, using route name as an override is usually simpler than applying the `Order` property.</span></span>

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a><span data-ttu-id="3f169-248">Ersetzen von Token in Routenvorlagen ([controller], [action] [area])</span><span class="sxs-lookup"><span data-stu-id="3f169-248">Token replacement in route templates ([controller], [action], [area])</span></span>

<span data-ttu-id="3f169-249">Der Einfachheit halber unterstützen Attributrouten die *Tokenersetzung*, indem ein Token in eckige Klammern eingeschlossen wird (`[`, `]`).</span><span class="sxs-lookup"><span data-stu-id="3f169-249">For convenience, attribute routes support *token replacement* by enclosing a token in square-braces (`[`, `]`).</span></span> <span data-ttu-id="3f169-250">Die Token `[action]`, `[area]` und `[controller]` werden durch die Werte der Aktionsnamen, den Namen des Bereichs und des Controllers der Aktion ersetzt, in dem die Route definiert ist.</span><span class="sxs-lookup"><span data-stu-id="3f169-250">The tokens `[action]`, `[area]`, and `[controller]` will be replaced with the values of the action name, area name, and controller name from the action where the route is defined.</span></span> <span data-ttu-id="3f169-251">In diesem Beispiel können die Aktionen wie in den Kommentaren beschrieben URL-Pfaden entsprechen:</span><span class="sxs-lookup"><span data-stu-id="3f169-251">In this example the actions can match URL paths as described in the comments:</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="3f169-252">Die Tokenersetzung tritt im letzten Schritt der Erstellung von Attributrouten auf.</span><span class="sxs-lookup"><span data-stu-id="3f169-252">Token replacement occurs as the last step of building the attribute routes.</span></span> <span data-ttu-id="3f169-253">Der folgende Code verhält sich genauso wie der aus dem obigen Beispiel:</span><span class="sxs-lookup"><span data-stu-id="3f169-253">The above example will behave the same as the following code:</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="3f169-254">Attributrouten können auch mit Vererbung kombiniert werden.</span><span class="sxs-lookup"><span data-stu-id="3f169-254">Attribute routes can also be combined with inheritance.</span></span> <span data-ttu-id="3f169-255">Dies ist besonders effektiv in Kombination mit der Tokenersetzung.</span><span class="sxs-lookup"><span data-stu-id="3f169-255">This is particularly powerful combined with token replacement.</span></span>

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

<span data-ttu-id="3f169-256">Tokenersetzung gilt auch für Routennamen, die durch Attributrouten definiert werden.</span><span class="sxs-lookup"><span data-stu-id="3f169-256">Token replacement also applies to route names defined by attribute routes.</span></span> <span data-ttu-id="3f169-257">`[Route("[controller]/[action]", Name="[controller]_[action]")]` generiert für jede Aktion einen eindeutigen Routennamen.</span><span class="sxs-lookup"><span data-stu-id="3f169-257">`[Route("[controller]/[action]", Name="[controller]_[action]")]` will generate a unique route name for each action.</span></span>

<span data-ttu-id="3f169-258">Damit das Trennzeichen `[` oder `]` der Tokenersetzungs-Literalzeichenfolge bei einem Abgleich gefunden wird, muss es doppelt vorhanden sein (`[[` oder `]]`), was einem Escapezeichen entspricht.</span><span class="sxs-lookup"><span data-stu-id="3f169-258">To match the literal token replacement delimiter `[` or  `]`, escape it by repeating the character (`[[` or `]]`).</span></span>

<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-routes"></a><span data-ttu-id="3f169-259">Mehrere Routen</span><span class="sxs-lookup"><span data-stu-id="3f169-259">Multiple Routes</span></span>

<span data-ttu-id="3f169-260">Attributrouting unterstützt das Definieren mehrerer Routen, die zu derselben Aktion führen.</span><span class="sxs-lookup"><span data-stu-id="3f169-260">Attribute routing supports defining multiple routes that reach the same action.</span></span> <span data-ttu-id="3f169-261">Dies wird am häufigsten beim Imitieren des Verhaltens der *herkömmlichen Standardroute* verwendet. Im folgenden Beispiel wird dies gezeigt:</span><span class="sxs-lookup"><span data-stu-id="3f169-261">The most common usage of this is to mimic the behavior of the *default conventional route* as shown in the following example:</span></span>

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

<span data-ttu-id="3f169-262">Wenn mehrere Routenattribute auf dem Controller platziert werden, wird jedes mit jedem der Routenattribute auf den Aktionsmethoden kombiniert.</span><span class="sxs-lookup"><span data-stu-id="3f169-262">Putting multiple route attributes on the controller means that each one will combine with each of the route attributes on the action methods.</span></span>

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

<span data-ttu-id="3f169-263">Werden mehrere Routenattribute (die `IActionConstraint` implementieren) auf einer Aktion platziert, wird jede Aktionseinschränkung mit der Routenvorlage aus dem Attribut kombiniert, das sie definiert.</span><span class="sxs-lookup"><span data-stu-id="3f169-263">When multiple route attributes (that implement `IActionConstraint`) are placed on an action, then each action constraint combines with the route template from the attribute that defined it.</span></span>

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
> <span data-ttu-id="3f169-264">Obwohl das Verwenden mehrerer Routen für Aktionen sehr hilfreich scheint, ist es empfehlenswerter, den URL-Raum der Anwendung einfach und gut definiert zu halten.</span><span class="sxs-lookup"><span data-stu-id="3f169-264">While using multiple routes on actions can seem powerful, it's better to keep your application's URL space simple and well-defined.</span></span> <span data-ttu-id="3f169-265">Verwenden Sie nur mehrere Routen für Aktionen, wenn dies notwendig ist, z.B. um vorhandene Clients zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="3f169-265">Use multiple routes on actions only where needed, for example to support existing clients.</span></span>

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a><span data-ttu-id="3f169-266">Angeben von optionalen Attributroutenparametern, Standardwerten und Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="3f169-266">Specifying attribute route optional parameters, default values, and constraints</span></span>

<span data-ttu-id="3f169-267">Attributrouten unterstützen dieselbe Inline-Syntax wie herkömmliche Routen, um optionale Parameter, Standardwerte und Einschränkungen anzugeben.</span><span class="sxs-lookup"><span data-stu-id="3f169-267">Attribute routes support the same inline syntax as conventional routes to specify optional parameters, default values, and constraints.</span></span>

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

<span data-ttu-id="3f169-268">Eine ausführliche Beschreibung der Syntax der Routenvorlage finden Sie unter [Routenvorlagenreferenz](../../fundamentals/routing.md#route-template-reference).</span><span class="sxs-lookup"><span data-stu-id="3f169-268">See [Route Template Reference](../../fundamentals/routing.md#route-template-reference) for a detailed description of route template syntax.</span></span>

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a><span data-ttu-id="3f169-269">Benutzerdefinierte Routenattribute mithilfe von `IRouteTemplateProvider`</span><span class="sxs-lookup"><span data-stu-id="3f169-269">Custom route attributes using `IRouteTemplateProvider`</span></span>

<span data-ttu-id="3f169-270">Alle im Framework bereitgestellten Routenattribute (`[Route(...)]`, `[HttpGet(...)]` usw.) implementieren die `IRouteTemplateProvider`-Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="3f169-270">All of the route attributes provided in the framework ( `[Route(...)]`, `[HttpGet(...)]` , etc.) implement the `IRouteTemplateProvider` interface.</span></span> <span data-ttu-id="3f169-271">MVC sucht in Controllerklassen und Aktionsmethoden nach Attributen, wenn die Anwendung gestartet wird, und verwendet diejenigen, die `IRouteTemplateProvider` implementieren, um die anfänglichen Routen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="3f169-271">MVC looks for attributes on controller classes and action methods when the app starts and uses the ones that implement `IRouteTemplateProvider` to build the initial set of routes.</span></span>

<span data-ttu-id="3f169-272">Sie können `IRouteTemplateProvider` implementieren, um eigene Routenattribute zu definieren.</span><span class="sxs-lookup"><span data-stu-id="3f169-272">You can implement `IRouteTemplateProvider` to define your own route attributes.</span></span> <span data-ttu-id="3f169-273">Jeder `IRouteTemplateProvider` lässt Sie eine einzelne Route mit einer benutzerdefinierten Routenvorlage, Reihenfolge und einem benutzerdefinierten Namen definieren:</span><span class="sxs-lookup"><span data-stu-id="3f169-273">Each `IRouteTemplateProvider` allows you to define a single route with a custom route template, order, and name:</span></span>

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

<span data-ttu-id="3f169-274">Das Attribut aus dem obigen Beispiel legt `Template` automatisch auf `"api/[controller]"` fest, wenn `[MyApiController]` angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="3f169-274">The attribute from the above example automatically sets the `Template` to `"api/[controller]"` when `[MyApiController]` is applied.</span></span>

<a name="routing-app-model-ref-label"></a>

### <a name="using-application-model-to-customize-attribute-routes"></a><span data-ttu-id="3f169-275">Anpassen von Attributrouten mithilfe des Anwendungsmodells</span><span class="sxs-lookup"><span data-stu-id="3f169-275">Using Application Model to customize attribute routes</span></span>

<span data-ttu-id="3f169-276">Das *Anwendungsmodell* ist ein Objektmodell, das beim Starten aus allen Metadaten von MVC erstellt wird, um Ihre Aktionen weiterzuleiten und auszuführen.</span><span class="sxs-lookup"><span data-stu-id="3f169-276">The *application model* is an object model created at startup with all of the metadata used by MVC to route and execute your actions.</span></span> <span data-ttu-id="3f169-277">Das *Anwendungsmodell* enthält alle Daten, die aus Routenattributen (über `IRouteTemplateProvider`) erfasst wurden.</span><span class="sxs-lookup"><span data-stu-id="3f169-277">The *application model* includes all of the data gathered from route attributes (through `IRouteTemplateProvider`).</span></span> <span data-ttu-id="3f169-278">Sie können *Konventionen* schreiben, damit das Anwendungsmodell beim Startzeitpunkt das Routingverhalten anpasst.</span><span class="sxs-lookup"><span data-stu-id="3f169-278">You can write *conventions* to modify the application model at startup time to customize how routing behaves.</span></span> <span data-ttu-id="3f169-279">Dieser Abschnitt enthält ein einfaches Beispiel für das Anpassen des Routings mithilfe des Anwendungsmodells.</span><span class="sxs-lookup"><span data-stu-id="3f169-279">This section shows a simple example of customizing routing using application model.</span></span>

[!code-csharp[Main](routing/sample/main/NamespaceRoutingConvention.cs)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a><span data-ttu-id="3f169-280">Gemischtes Routing: Attributrouting vs. herkömmliches Routing</span><span class="sxs-lookup"><span data-stu-id="3f169-280">Mixed routing: Attribute routing vs conventional routing</span></span>

<span data-ttu-id="3f169-281">MVC-Anwendungen können herkömmliches Routing und Attributrouting kombinieren.</span><span class="sxs-lookup"><span data-stu-id="3f169-281">MVC applications can mix the use of conventional routing and attribute routing.</span></span> <span data-ttu-id="3f169-282">In der Regel werden herkömmliche Routen für Controller verwendet, die für Browser-HTML-Seiten gedacht sind, und das Attributrouting für Controller für REST-APIs.</span><span class="sxs-lookup"><span data-stu-id="3f169-282">It's typical to use conventional routes for controllers serving HTML pages for browsers, and attribute routing for controllers serving REST APIs.</span></span>

<span data-ttu-id="3f169-283">Aktionen werden entweder herkömmlich oder über Attribute zugeordnet,</span><span class="sxs-lookup"><span data-stu-id="3f169-283">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="3f169-284">d.h., dass eine Route auf dem Controller oder der Aktion platziert wird.</span><span class="sxs-lookup"><span data-stu-id="3f169-284">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="3f169-285">Aktionen, die Attributrouten definieren, können nicht über die herkömmliche Routen und umgekehrt erreicht werden.</span><span class="sxs-lookup"><span data-stu-id="3f169-285">Actions that define attribute routes cannot be reached through the conventional routes and vice-versa.</span></span> <span data-ttu-id="3f169-286">Wenn nur **ein** Routenattribut auf dem Controller platziert wird, werden alle Aktionen im Controller über Attribute zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="3f169-286">**Any** route attribute on the controller makes all actions in the controller attribute routed.</span></span>

> [!NOTE]
> <span data-ttu-id="3f169-287">Die beiden Routingmethoden unterscheiden sich in dem Prozess, der angewendet wird, nachdem eine URL einer Routenvorlage zugeordnet wurde.</span><span class="sxs-lookup"><span data-stu-id="3f169-287">What distinguishes the two types of routing systems is the process applied after a URL matches a route template.</span></span> <span data-ttu-id="3f169-288">Beim herkömmlichen Routing dienen die Routenwerte aus der Zuordnung dazu, aus einer Nachschlagetabelle aller herkömmlich zugeordneten Aktionen die Aktion und den Controller auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="3f169-288">In conventional routing, the route values from the match are used to choose the action and controller from a lookup table of all conventional routed actions.</span></span> <span data-ttu-id="3f169-289">Beim Attributrouting ist jede Vorlage bereits einer Aktion zugeordnet, und keine weitere Suche ist erforderlich.</span><span class="sxs-lookup"><span data-stu-id="3f169-289">In attribute routing, each template is already associated with an action, and no further lookup is needed.</span></span>

<a name="routing-url-gen-ref-label"></a>

## <a name="url-generation"></a><span data-ttu-id="3f169-290">URL-Generierung</span><span class="sxs-lookup"><span data-stu-id="3f169-290">URL Generation</span></span>

<span data-ttu-id="3f169-291">MVC-Anwendungen können Routingfeatures zur URL-Generierung verwenden, um URL-Links zu Aktionen zu generieren.</span><span class="sxs-lookup"><span data-stu-id="3f169-291">MVC applications can use routing's URL generation features to generate URL links to actions.</span></span> <span data-ttu-id="3f169-292">Durch das Generieren von URLs müssen URLs nicht mehr hartcodiert werden, sodass der Code robuster und leichter verwaltbar wird.</span><span class="sxs-lookup"><span data-stu-id="3f169-292">Generating URLs eliminates hardcoding URLs, making your code more robust and maintainable.</span></span> <span data-ttu-id="3f169-293">Dieser Abschnitt konzentriert sich auf die von MVC bereitgestellte URL-Generierung und befasst sich nur kurz mit der Funktionsweise.</span><span class="sxs-lookup"><span data-stu-id="3f169-293">This section focuses on the URL generation features provided by MVC and will only cover basics of how URL generation works.</span></span> <span data-ttu-id="3f169-294">Eine detaillierte Beschreibung der URL-Generierung finden Sie unter [Routing in ASP.NET Core](../../fundamentals/routing.md).</span><span class="sxs-lookup"><span data-stu-id="3f169-294">See [Routing](../../fundamentals/routing.md) for a detailed description of URL generation.</span></span>

<span data-ttu-id="3f169-295">Die `IUrlHelper`-Schnittstelle ist die zugrunde liegende Infrastruktur zwischen MVC und dem Routing zur URL-Generierung.</span><span class="sxs-lookup"><span data-stu-id="3f169-295">The `IUrlHelper` interface is the underlying piece of infrastructure between MVC and routing for URL generation.</span></span> <span data-ttu-id="3f169-296">Eine Instanz von `IUrlHelper` ist über die `Url`-Eigenschaft in Controllern, Ansichten und Ansichtskomponenten verfügbar.</span><span class="sxs-lookup"><span data-stu-id="3f169-296">You'll find an instance of `IUrlHelper` available through the `Url` property in controllers, views, and view components.</span></span>

<span data-ttu-id="3f169-297">In diesem Beispiel wird die `IUrlHelper`-Schnittstelle von der `Controller.Url`-Eigenschaft dazu verwendet, eine URL in einer anderen Aktion zu generieren.</span><span class="sxs-lookup"><span data-stu-id="3f169-297">In this example, the `IUrlHelper` interface is used through the `Controller.Url` property to generate a URL to another action.</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

<span data-ttu-id="3f169-298">Wenn die Anwendung die herkömmliche Standardroute verwendet, ist der Wert der `url`-Variable die URL-Pfadzeichenfolge `/UrlGeneration/Destination`.</span><span class="sxs-lookup"><span data-stu-id="3f169-298">If the application is using the default conventional route, the value of the `url` variable will be the URL path string `/UrlGeneration/Destination`.</span></span> <span data-ttu-id="3f169-299">Dieser URL-Pfad wird vom Routing erstellt, indem Routenwerte aus der aktuellen Anforderung (Umgebungswerte) mit den an `Url.Action` übergebenen Werten kombiniert und anschließend in die Routenvorlage eingefügt werden:</span><span class="sxs-lookup"><span data-stu-id="3f169-299">This URL path is created by routing by combining the route values from the current request (ambient values), with the values passed to `Url.Action` and substituting those values into the route template:</span></span>

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

<span data-ttu-id="3f169-300">Der Wert eines jeden Routenparameters wird in der Routenvorlage durch die entsprechenden Namen mit den Werten und Umgebungswerten ersetzt.</span><span class="sxs-lookup"><span data-stu-id="3f169-300">Each route parameter in the route template has its value substituted by matching names with the values and ambient values.</span></span> <span data-ttu-id="3f169-301">Ein Routenparameter ohne Wert kann einen Standardwert verwenden, wenn er über einen verfügt, oder übersprungen werden, wenn er optional ist (wie im Fall von `id` in diesem Beispiel).</span><span class="sxs-lookup"><span data-stu-id="3f169-301">A route parameter that doesn't have a value can use a default value if it has one, or be skipped if it's optional (as in the case of `id` in this example).</span></span> <span data-ttu-id="3f169-302">Die URL-Generierung schlägt fehl, wenn ein erforderlicher Routenparameter keinen entsprechenden Wert besitzt.</span><span class="sxs-lookup"><span data-stu-id="3f169-302">URL generation will fail if any required route parameter doesn't have a corresponding value.</span></span> <span data-ttu-id="3f169-303">Wenn die URL-Generierung für eine Route fehlschlägt, wird die nächste Route ausprobiert, bis alle Routen getestet wurden oder eine Übereinstimmung gefunden wurde.</span><span class="sxs-lookup"><span data-stu-id="3f169-303">If URL generation fails for a route, the next route is tried until all routes have been tried or a match is found.</span></span>

<span data-ttu-id="3f169-304">In dem Beispiel mit `Url.Action` oben wird von herkömmlichem Routing ausgegangen. Die URL-Generierung funktioniert ähnlich wie das Attributrouting, obwohl sich die Konzepte unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="3f169-304">The example of `Url.Action` above assumes conventional routing, but URL generation works similarly with attribute routing, though the concepts are different.</span></span> <span data-ttu-id="3f169-305">Bei herkömmlichem Routing wird mit den Routenwerten eine Vorlage erweitert, und die Routenwerte für `controller` und `action` kommen in der Regel in dieser Vorlage vor. Das funktioniert, weil sich die mit dem Routing zugeordneten URLs an eine *Konvention* halten.</span><span class="sxs-lookup"><span data-stu-id="3f169-305">With conventional routing, the route values are used to expand a template, and the route values for `controller` and `action` usually appear in that template - this works because the URLs matched by routing adhere to a *convention*.</span></span> <span data-ttu-id="3f169-306">Beim Attributrouting dürfen die Routenwerte für `controller` und `action` nicht in der Vorlage vorkommen. Sie werden stattdessen verwendet, um nachzuschlagen, welche Vorlage verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="3f169-306">In attribute routing, the route values for `controller` and `action` are not allowed to appear in the template - they're instead used to look up which template to use.</span></span>

<span data-ttu-id="3f169-307">In diesem Beispiel wird das Attributrouting verwendet:</span><span class="sxs-lookup"><span data-stu-id="3f169-307">This example uses attribute routing:</span></span>

[!code-csharp[Main](routing/sample/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

<span data-ttu-id="3f169-308">MVC erstellt eine Nachschlagetabelle aller über Attribute zugeordneten Aktionen und ordnet die `controller`- und `action`-Werte zu, um die Routenvorlage auszuwählen, die für die URL-Generierung verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="3f169-308">MVC builds a lookup table of all attribute routed actions and will match the `controller` and `action` values to select the route template to use for URL generation.</span></span> <span data-ttu-id="3f169-309">Im obigen Beispiel wird `custom/url/to/destination` generiert.</span><span class="sxs-lookup"><span data-stu-id="3f169-309">In the sample above,   `custom/url/to/destination` is generated.</span></span>

### <a name="generating-urls-by-action-name"></a><span data-ttu-id="3f169-310">Generieren von URLs nach Aktionsnamen</span><span class="sxs-lookup"><span data-stu-id="3f169-310">Generating URLs by action name</span></span>

<span data-ttu-id="3f169-311">`Url.Action` (`IUrlHelper` .</span><span class="sxs-lookup"><span data-stu-id="3f169-311">`Url.Action` (`IUrlHelper` .</span></span> <span data-ttu-id="3f169-312">`Action`) und alle zugehörigen Überladungen bauen alle auf der Idee auf, dass Sie angeben, was Sie verknüpfen möchten, indem Sie einen Controllernamen und einen Aktionsnamen angeben.</span><span class="sxs-lookup"><span data-stu-id="3f169-312">`Action`) and all related overloads all are based on that idea that you want to specify what you're linking to by specifying a controller name and action name.</span></span>

> [!NOTE]
> <span data-ttu-id="3f169-313">Bei Verwendung von `Url.Action` sind die aktuellen Routenwerte für `controller` und `action` für Sie angegeben. Die Werte von `controller` und `action` bestehen sowohl aus *Umgebungswerten* **als auch** aus *Werten*.</span><span class="sxs-lookup"><span data-stu-id="3f169-313">When using `Url.Action`, the current route values for `controller` and `action` are specified for you - the value of `controller` and `action` are part of both *ambient values* **and** *values*.</span></span> <span data-ttu-id="3f169-314">Die Methode `Url.Action` verwendet immer die aktuellen Werte von `action` und `controller` und generiert einen URL-Pfad, der zur aktuellen Aktion weiterleitet.</span><span class="sxs-lookup"><span data-stu-id="3f169-314">The method `Url.Action`, always uses the current values of `action` and `controller` and will generate a URL path that routes to the current action.</span></span>

<span data-ttu-id="3f169-315">Beim Routing wird versucht, mit den Werten in den Umgebungswerten Informationen auszufüllen, die Sie beim Generieren einer URL nicht bereitgestellt haben.</span><span class="sxs-lookup"><span data-stu-id="3f169-315">Routing attempts to use the values in ambient values to fill in information that you didn't provide when generating a URL.</span></span> <span data-ttu-id="3f169-316">Mit Routen wie `{a}/{b}/{c}/{d}` und Umgebungswerten wie `{ a = Alice, b = Bob, c = Carol, d = David }` hat das Routing genügend Informationen, um eine URL ohne zusätzliche Werte zu generieren, da alle Routenparameter einen Wert aufweisen.</span><span class="sxs-lookup"><span data-stu-id="3f169-316">Using a route like `{a}/{b}/{c}/{d}` and ambient values `{ a = Alice, b = Bob, c = Carol, d = David }`, routing has enough information to generate a URL without any additional values - since all route parameters have a value.</span></span> <span data-ttu-id="3f169-317">Wenn Sie den Wert `{ d = Donovan }` hinzufügen, wird der Wert `{ d = David }` ignoriert, und der generierte URL-Pfad wäre `Alice/Bob/Carol/Donovan`.</span><span class="sxs-lookup"><span data-stu-id="3f169-317">If you added the value `{ d = Donovan }`, the value `{ d = David }` would be ignored, and the generated URL path would be `Alice/Bob/Carol/Donovan`.</span></span>

> [!WARNING]
> <span data-ttu-id="3f169-318">URL-Pfade sind hierarchisch.</span><span class="sxs-lookup"><span data-stu-id="3f169-318">URL paths are hierarchical.</span></span> <span data-ttu-id="3f169-319">Wenn Sie im obigen Beispiel den Wert `{ c = Cheryl }` hinzufügen, werden die beiden Werte `{ c = Carol, d = David }` ignoriert.</span><span class="sxs-lookup"><span data-stu-id="3f169-319">In the example above, if you added the value `{ c = Cheryl }`, both of the values `{ c = Carol, d = David }` would be ignored.</span></span> <span data-ttu-id="3f169-320">In diesem Fall haben wir keinen Wert für `d` mehr, und die URL-Generierung schlägt fehl.</span><span class="sxs-lookup"><span data-stu-id="3f169-320">In this case we no longer have a value for `d` and URL generation will fail.</span></span> <span data-ttu-id="3f169-321">Sie müssten dann den gewünschten Wert von `c` und `d` angeben.</span><span class="sxs-lookup"><span data-stu-id="3f169-321">You would need to specify the desired value of `c` and `d`.</span></span>  <span data-ttu-id="3f169-322">Man könnte annehmen, dass dieses Problem bei der Standardroute auftritt (`{controller}/{action}/{id?}`). Tatsächlich passiert es in der Praxis jedoch selten, das `Url.Action` immer explizit einen `controller`- und `action`-Wert angibt.</span><span class="sxs-lookup"><span data-stu-id="3f169-322">You might expect to hit this problem with the default route (`{controller}/{action}/{id?}`) - but you will rarely encounter this behavior in practice as `Url.Action` will always explicitly specify a `controller` and `action` value.</span></span>

<span data-ttu-id="3f169-323">Längere Überladungen von `Url.Action` akzeptieren auch ein zusätzliches *route values*-Objekt, um andere Werte für Routenparameter als `controller` und `action` bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="3f169-323">Longer overloads of `Url.Action` also take an additional *route values* object to provide values for route parameters other than `controller` and `action`.</span></span> <span data-ttu-id="3f169-324">Es wird in der Regel mit `id` verwendet, z.B. in `Url.Action("Buy", "Products", new { id = 17 })`.</span><span class="sxs-lookup"><span data-stu-id="3f169-324">You will most commonly see this used with `id` like `Url.Action("Buy", "Products", new { id = 17 })`.</span></span> <span data-ttu-id="3f169-325">Gemäß der Konvention ist das *Routenwerte*-Objekt eines des anonymen Typs, es kann aber auch ein `IDictionary<>`-Objekt oder ein *Plain Old .NET Object* sein.</span><span class="sxs-lookup"><span data-stu-id="3f169-325">By convention the *route values* object is usually an object of anonymous type, but it can also be an `IDictionary<>` or a *plain old .NET object*.</span></span> <span data-ttu-id="3f169-326">Alle zusätzlichen Routenwerte, die keinen Routenparametern zugeordnet sind, werden in der Abfragezeichenfolge platziert.</span><span class="sxs-lookup"><span data-stu-id="3f169-326">Any additional route values that don't match route parameters are put in the query string.</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/TestController.cs)]

> [!TIP]
> <span data-ttu-id="3f169-327">Um eine absolute URL zu erstellen, verwenden Sie eine Überladung, die eine `protocol` akzeptiert: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span><span class="sxs-lookup"><span data-stu-id="3f169-327">To create an absolute URL, use an overload that accepts a `protocol`: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span></span>

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generating-urls-by-route"></a><span data-ttu-id="3f169-328">Generieren von URLs nach Routen</span><span class="sxs-lookup"><span data-stu-id="3f169-328">Generating URLs by route</span></span>

<span data-ttu-id="3f169-329">Im obigen Code wurde das Generieren einer URL durch das Übergeben des Controller- und Aktionsnamens veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="3f169-329">The code above demonstrated generating a URL by passing in the controller and action name.</span></span> <span data-ttu-id="3f169-330">`IUrlHelper` stellt außerdem die `Url.RouteUrl`-Methodenfamilie bereit.</span><span class="sxs-lookup"><span data-stu-id="3f169-330">`IUrlHelper` also provides the `Url.RouteUrl` family of methods.</span></span> <span data-ttu-id="3f169-331">Diese Methoden ähneln `Url.Action`, kopieren jedoch nicht die aktuellen Werte `action` und `controller` in die Routenwerte.</span><span class="sxs-lookup"><span data-stu-id="3f169-331">These methods are similar to `Url.Action`, but they don't copy the current values of `action` and `controller` to the route values.</span></span> <span data-ttu-id="3f169-332">Oftmals wird damit ein Routenname angegeben, um mit einer bestimmten Route die URL zu generieren, in der Regel *ohne* Angabe eines Controller- oder Aktionsnamens.</span><span class="sxs-lookup"><span data-stu-id="3f169-332">The most common usage is to specify a route name to use a specific route to generate the URL, generally *without* specifying a controller or action name.</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generating-urls-in-html"></a><span data-ttu-id="3f169-333">Generieren von URLs in HTML</span><span class="sxs-lookup"><span data-stu-id="3f169-333">Generating URLs in HTML</span></span>

<span data-ttu-id="3f169-334">`IHtmlHelper` stellt die `HtmlHelper`-Methoden `Html.BeginForm` und `Html.ActionLink` bereit, um jeweils `<form>`- und `<a>`-Elemente zu generieren.</span><span class="sxs-lookup"><span data-stu-id="3f169-334">`IHtmlHelper` provides the `HtmlHelper` methods `Html.BeginForm` and `Html.ActionLink` to generate `<form>` and `<a>` elements respectively.</span></span> <span data-ttu-id="3f169-335">Diese Methoden verwenden die `Url.Action`-Methode, um eine URL zu generieren, und akzeptieren ähnliche Argumente.</span><span class="sxs-lookup"><span data-stu-id="3f169-335">These methods use the `Url.Action` method to generate a URL and they accept similar arguments.</span></span> <span data-ttu-id="3f169-336">Die `Url.RouteUrl`-Begleiter für `HtmlHelper` sind `Html.BeginRouteForm` und `Html.RouteLink`, die ähnliche Funktionen aufweisen.</span><span class="sxs-lookup"><span data-stu-id="3f169-336">The `Url.RouteUrl` companions for `HtmlHelper` are `Html.BeginRouteForm` and `Html.RouteLink` which have similar functionality.</span></span>

<span data-ttu-id="3f169-337">Taghilfsprogramme generieren URLs mit den Taghilfsprogrammen `form` und `<a>`.</span><span class="sxs-lookup"><span data-stu-id="3f169-337">TagHelpers generate URLs through the `form` TagHelper and the `<a>` TagHelper.</span></span> <span data-ttu-id="3f169-338">Beide implementieren mit `IUrlHelper`.</span><span class="sxs-lookup"><span data-stu-id="3f169-338">Both of these use `IUrlHelper` for their implementation.</span></span> <span data-ttu-id="3f169-339">Weitere Informationen finden Sie unter [Einführung in die Verwendung von Taghilfsprogrammen in Formularen in ASP.NET Core](../views/working-with-forms.md).</span><span class="sxs-lookup"><span data-stu-id="3f169-339">See [Working with Forms](../views/working-with-forms.md) for more information.</span></span>

<span data-ttu-id="3f169-340">In Ansichten ist `IUrlHelper` über die `Url`-Eigenschaft für jede Ad-hoc-URL-Generierung verfügbar, die keine der oben genannten ist.</span><span class="sxs-lookup"><span data-stu-id="3f169-340">Inside views, the `IUrlHelper` is available through the `Url` property for any ad-hoc URL generation not covered by the above.</span></span>

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="generating-urls-in-action-results"></a><span data-ttu-id="3f169-341">Generieren von URLs in Aktionsergebnissen</span><span class="sxs-lookup"><span data-stu-id="3f169-341">Generating URLS in Action Results</span></span>

<span data-ttu-id="3f169-342">In den obigen Beispielen wurde gezeigt, wie `IUrlHelper` in einem Controller verwendet wird und dass es üblicherweise dazu dient, eine URL im Rahmen eines Aktionsergebnisses zu generieren.</span><span class="sxs-lookup"><span data-stu-id="3f169-342">The examples above have shown using `IUrlHelper` in a controller, while the most common usage in a controller is to generate a URL as part of an action result.</span></span>

<span data-ttu-id="3f169-343">Die Basisklassen `ControllerBase` und `Controller` stellen Hilfsmethoden für Aktionsergebnisse bereit, die auf eine andere Aktionen verweisen.</span><span class="sxs-lookup"><span data-stu-id="3f169-343">The `ControllerBase` and `Controller` base classes provide convenience methods for action results that reference another action.</span></span> <span data-ttu-id="3f169-344">Eine typische Verwendung besteht darin, nach dem Akzeptieren einer Benutzereingabe weiterzuleiten.</span><span class="sxs-lookup"><span data-stu-id="3f169-344">One typical usage is to redirect after accepting user input.</span></span>

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

<span data-ttu-id="3f169-345">Die Factorymethoden der Aktionsergebnisse folgen einem ähnlichen Muster wie die Methoden für `IUrlHelper`.</span><span class="sxs-lookup"><span data-stu-id="3f169-345">The action results factory methods follow a similar pattern to the methods on `IUrlHelper`.</span></span>

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a><span data-ttu-id="3f169-346">Sonderfall für dedizierte herkömmliche Routen</span><span class="sxs-lookup"><span data-stu-id="3f169-346">Special case for dedicated conventional routes</span></span>

<span data-ttu-id="3f169-347">Beim herkömmlichen Routing können Sie eine bestimmte Art von Routendefinition verwenden, die als *dedizierte herkömmliche Route* bezeichnet wird.</span><span class="sxs-lookup"><span data-stu-id="3f169-347">Conventional routing can use a special kind of route definition called a *dedicated conventional route*.</span></span> <span data-ttu-id="3f169-348">Die Route namens `blog` im folgenden Beispiel ist eine dedizierte herkömmliche Route.</span><span class="sxs-lookup"><span data-stu-id="3f169-348">In the example below, the route named `blog` is a dedicated conventional route.</span></span>

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="3f169-349">Wenn Sie diese Routendefinitionen verwenden, generiert `Url.Action("Index", "Home")` den URL-Pfad `/` mit der `default`-Route. Die Frage ist nur warum?</span><span class="sxs-lookup"><span data-stu-id="3f169-349">Using these route definitions, `Url.Action("Index", "Home")` will generate the URL path `/` with the `default` route, but why?</span></span> <span data-ttu-id="3f169-350">Man könnte meinen, dass die Routenwerte `{ controller = Home, action = Index }` ausreichen würden, um eine URL mithilfe von `blog` zu generieren, und das Ergebnis wäre `/blog?action=Index&controller=Home`.</span><span class="sxs-lookup"><span data-stu-id="3f169-350">You might guess the route values `{ controller = Home, action = Index }` would be enough to generate a URL using `blog`, and the result would be `/blog?action=Index&controller=Home`.</span></span>

<span data-ttu-id="3f169-351">Dedizierte herkömmliche Routen nutzen ein spezielles Verhalten von Standardwerten, die keinen entsprechenden Routenparameter besitzen, der verhindert, dass die Route bei der URL-Generierung „zu gierig“ wird.</span><span class="sxs-lookup"><span data-stu-id="3f169-351">Dedicated conventional routes rely on a special behavior of default values that don't have a corresponding route parameter that prevents the route from being "too greedy" with URL generation.</span></span> <span data-ttu-id="3f169-352">In diesem Fall sind die Standardwerte `{ controller = Blog, action = Article }`, und weder `controller` noch `action` werden als Routenparameter verwendet.</span><span class="sxs-lookup"><span data-stu-id="3f169-352">In this case the default values are `{ controller = Blog, action = Article }`, and neither `controller` nor `action` appears as a route parameter.</span></span> <span data-ttu-id="3f169-353">Wenn das Routing die URL-Generierung ausführt, müssen die angegebenen Werte mit den Standardwerten übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="3f169-353">When routing performs URL generation, the values provided must match the default values.</span></span> <span data-ttu-id="3f169-354">Die URL-Generierung mithilfe von `blog` schlägt fehl, da die Werte `{ controller = Home, action = Index }` nicht mit `{ controller = Blog, action = Article }` übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="3f169-354">URL generation using `blog` will fail because the values `{ controller = Home, action = Index }` don't match `{ controller = Blog, action = Article }`.</span></span> <span data-ttu-id="3f169-355">Routing greift dann wieder auf `default` zurück, was erfolgreich ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="3f169-355">Routing then falls back to try `default`, which succeeds.</span></span>

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a><span data-ttu-id="3f169-356">Bereiche</span><span class="sxs-lookup"><span data-stu-id="3f169-356">Areas</span></span>

<span data-ttu-id="3f169-357">[Bereiche](areas.md) sind ein MVC-Feature, mit dem verwandte Funktionen in einer Gruppe als separater Routing-Namespace (für Controlleraktionen) und als Ordnerstruktur (für Ansichten) organisiert werden können.</span><span class="sxs-lookup"><span data-stu-id="3f169-357">[Areas](areas.md) are an MVC feature used to organize related functionality into a group as a separate routing-namespace (for controller actions) and folder structure (for views).</span></span> <span data-ttu-id="3f169-358">Mithilfe von Bereichen kann eine Anwendung mehrere Controller mit demselben Namen haben – solange sie verschiedene *Bereiche* haben.</span><span class="sxs-lookup"><span data-stu-id="3f169-358">Using areas allows an application to have multiple controllers with the same name - as long as they have different *areas*.</span></span> <span data-ttu-id="3f169-359">Mithilfe von Bereichen wird außerdem eine Hierarchie erstellt, damit das Routing durch Hinzufügen eines anderen Routenparameters ausgeführt werden kann: `area` zu `controller` und `action`.</span><span class="sxs-lookup"><span data-stu-id="3f169-359">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area` to `controller` and `action`.</span></span> <span data-ttu-id="3f169-360">In diesem Abschnitt wird erläutert, wie Routing mit Bereichen interagiert. Weitere Informationen zur Verwendung von Bereichen mit Ansichten finden Sie unter [Bereiche](areas.md).</span><span class="sxs-lookup"><span data-stu-id="3f169-360">This section will discuss how routing interacts with areas - see [Areas](areas.md) for details about how areas are used with views.</span></span>

<span data-ttu-id="3f169-361">Im folgenden Beispiel wird MVC konfiguriert, sodass es die standardmäßige herkömmliche Route verwendet und eine *Bereichsroute* für einen Bereich namens `Blog`:</span><span class="sxs-lookup"><span data-stu-id="3f169-361">The following example configures MVC to use the default conventional route and an *area route* for an area named `Blog`:</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet1)]

<span data-ttu-id="3f169-362">Beim Abgleich eines URL-Pfads wie `/Manage/Users/AddUser` erzeugt die erste Route die Routenwerte `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="3f169-362">When matching a URL path like `/Manage/Users/AddUser`, the first route will produce the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span> <span data-ttu-id="3f169-363">Der `area`-Routenwert wird von einem Standardwert für `area` erzeugt. Die von `MapAreaRoute` erstellte Route entspricht in der Tat der folgenden:</span><span class="sxs-lookup"><span data-stu-id="3f169-363">The `area` route value is produced by a default value for `area`, in fact the route created by `MapAreaRoute` is equivalent to the following:</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet2)]

<span data-ttu-id="3f169-364">`MapAreaRoute` erstellt mit einem Standardwert und einer Einschränkung für `area` sowie mit dem bereitgestellten Bereichsnamen (in diesem Fall `Blog`) eine Route.</span><span class="sxs-lookup"><span data-stu-id="3f169-364">`MapAreaRoute` creates a route using both a default value and constraint for `area` using the provided area name, in this case `Blog`.</span></span> <span data-ttu-id="3f169-365">Der Standardwert stellt sicher, dass die Route immer `{ area = Blog, ... }` erzeugt, die Einschränkung erfordert den Wert `{ area = Blog, ... }` für URL-Generierung.</span><span class="sxs-lookup"><span data-stu-id="3f169-365">The default value ensures that the route always produces `{ area = Blog, ... }`, the constraint requires the value `{ area = Blog, ... }` for URL generation.</span></span>

> [!TIP]
> <span data-ttu-id="3f169-366">Beim herkömmlichen Routing ist die Reihenfolge wichtig.</span><span class="sxs-lookup"><span data-stu-id="3f169-366">Conventional routing is order-dependent.</span></span> <span data-ttu-id="3f169-367">Routen mit Bereichen werden im Allgemeinen früher in der Routentabelle aufgeführt als die spezifischeren Routen ohne Bereich.</span><span class="sxs-lookup"><span data-stu-id="3f169-367">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="3f169-368">Die Routenwerte im oben genannten Beispiel werden der folgenden Aktion zugeordnet:</span><span class="sxs-lookup"><span data-stu-id="3f169-368">Using the above example, the route values would match the following action:</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

<span data-ttu-id="3f169-369">`AreaAttribute` kennzeichnet einen Controller als Teil eines Bereichs, z.B. des Bereichs `Blog`.</span><span class="sxs-lookup"><span data-stu-id="3f169-369">The `AreaAttribute` is what denotes a controller as part of an area, we say that this controller is in the `Blog` area.</span></span> <span data-ttu-id="3f169-370">Controller ohne `[Area]`-Attribut gehören demnach zu keinem Bereich und stimmen **nicht** überein, wenn die `area`-Route wird vom Routing bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="3f169-370">Controllers without an `[Area]` attribute are not members of any area, and will **not** match when the `area` route value is provided by routing.</span></span> <span data-ttu-id="3f169-371">Im folgenden Beispiel kann nur der erste aufgelistete Controller die Routenwerte `{ area = Blog, controller = Users, action = AddUser }` abgleichen.</span><span class="sxs-lookup"><span data-stu-id="3f169-371">In the following example, only the first controller listed can match the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[Main](routing/sample/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> <span data-ttu-id="3f169-372">Aus Gründen der Vollständigkeit wird hier der Namespace jedes Controllers dargestellt. Andernfalls gäbe es einen Namenskonflikt zwischen den Controllern, und ein Compilerfehler würde generiert.</span><span class="sxs-lookup"><span data-stu-id="3f169-372">The namespace of each controller is shown here for completeness - otherwise the controllers would have a naming conflict and generate a compiler error.</span></span> <span data-ttu-id="3f169-373">Klassennamespaces haben keine Auswirkungen auf das MVC-Routing.</span><span class="sxs-lookup"><span data-stu-id="3f169-373">Class namespaces have no effect on MVC's routing.</span></span>

<span data-ttu-id="3f169-374">Die ersten beiden Controller gehören zu Bereichen und werden können nur abgleichen, wenn ihr jeweiliger Bereichsname vom `area`-Routenwert bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="3f169-374">The first two controllers are members of areas, and only match when their respective area name is provided by the `area` route value.</span></span> <span data-ttu-id="3f169-375">Der dritte Controller gehört keinem Bereich an und kann nur abgleichen, wenn vom Routing kein Wert für `area` bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="3f169-375">The third controller isn't a member of any area, and can only match when no value for `area` is provided by routing.</span></span>

> [!NOTE]
> <span data-ttu-id="3f169-376">Im Hinblick auf das Erkennen *keines Werts* hat die Abwesenheit des `area`-Werts dieselben Auswirkungen, wie wenn der Wert für `area` 0 (null) oder eine leere Zeichenfolge wäre.</span><span class="sxs-lookup"><span data-stu-id="3f169-376">In terms of matching *no value*, the absence of the `area` value is the same as if the value for `area` were null or the empty string.</span></span>

<span data-ttu-id="3f169-377">Beim Ausführen einer Aktion innerhalb eines Bereichs ist der Routenwert für `area` als *Umgebungswert* für das Routing verfügbar und kann für die URL-Generierung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="3f169-377">When executing an action inside an area, the route value for `area` will be available as an *ambient value* for routing to use for URL generation.</span></span> <span data-ttu-id="3f169-378">Das bedeutet, dass Bereiche bei der URL-Generierung wie im folgenden Beispiel dargestellt standardmäßig *beständig* sind.</span><span class="sxs-lookup"><span data-stu-id="3f169-378">This means that by default areas act *sticky* for URL generation as demonstrated by the following sample.</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet3)]

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<a name="iactionconstraint-ref-label"></a>

## <a name="understanding-iactionconstraint"></a><span data-ttu-id="3f169-379">Verstehen von IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="3f169-379">Understanding IActionConstraint</span></span>

> [!NOTE]
> <span data-ttu-id="3f169-380">Dieser Abschnitt enthält Details zu Framework-Mechanismen und dazu, wie eine auszuführende Aktion in MVC auswählt wird.</span><span class="sxs-lookup"><span data-stu-id="3f169-380">This section is a deep-dive on framework internals and how MVC chooses an action to execute.</span></span> <span data-ttu-id="3f169-381">Eine typische Anwendung benötigt kein benutzerdefiniertes `IActionConstraint`.</span><span class="sxs-lookup"><span data-stu-id="3f169-381">A typical application won't need a custom `IActionConstraint`</span></span>

<span data-ttu-id="3f169-382">Sie haben `IActionConstraint` wahrscheinlich bereits verwendet, auch wenn Sie mit der Oberfläche noch nicht vertraut sind.</span><span class="sxs-lookup"><span data-stu-id="3f169-382">You have likely already used `IActionConstraint` even if you're not familiar with the interface.</span></span> <span data-ttu-id="3f169-383">Das `[HttpGet]`-Attribut und ähnliche `[Http-VERB]`-Attribute implementieren `IActionConstraint`, um die Ausführung einer Aktionsmethode einzuschränken.</span><span class="sxs-lookup"><span data-stu-id="3f169-383">The `[HttpGet]` Attribute and similar `[Http-VERB]` attributes implement `IActionConstraint` in order to limit the execution of an action method.</span></span>

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

<span data-ttu-id="3f169-384">Unter Annahme der herkömmlichen Standardroute würde der URL-Pfad `/Products/Edit` die Werte `{ controller = Products, action = Edit }` erzeugen, die **beiden** hier gezeigten Aktionen entsprechen.</span><span class="sxs-lookup"><span data-stu-id="3f169-384">Assuming the default conventional route, the URL path `/Products/Edit` would produce the values `{ controller = Products, action = Edit }`, which would match **both** of the actions shown here.</span></span> <span data-ttu-id="3f169-385">In `IActionConstraint`-Terminologie würde man sagen, dass es sich bei beiden genannten Aktionen um Kandidaten handelt, da beide den Routendaten entsprechen.</span><span class="sxs-lookup"><span data-stu-id="3f169-385">In `IActionConstraint` terminology we would say that both of these actions are considered candidates - as they both match the route data.</span></span>

<span data-ttu-id="3f169-386">Wenn `HttpGetAttribute` ausgeführt wird, wird angegeben, dass *Edit()* mit *GET* übereinstimmt und mit keinem anderen HTTP-Verb.</span><span class="sxs-lookup"><span data-stu-id="3f169-386">When the `HttpGetAttribute` executes, it will say that *Edit()* is a match for *GET* and isn't a match for any other HTTP verb.</span></span> <span data-ttu-id="3f169-387">Für die `Edit(...)`-Aktion wurde keine Einschränkungen definiert, und daher stimmt sie mit allen HTTP-Verben überein.</span><span class="sxs-lookup"><span data-stu-id="3f169-387">The `Edit(...)` action doesn't have any constraints defined, and so will match any HTTP verb.</span></span> <span data-ttu-id="3f169-388">Wenn wir von `POST` ausgehen, stimmt nur `Edit(...)` überein.</span><span class="sxs-lookup"><span data-stu-id="3f169-388">So assuming a `POST` - only `Edit(...)` matches.</span></span> <span data-ttu-id="3f169-389">Für `GET` können jedoch beide Aktionen übereinstimmen. Eine Aktion mit `IActionConstraint` wird einer Aktion ohne jedoch immer *vorgezogen*.</span><span class="sxs-lookup"><span data-stu-id="3f169-389">But, for a `GET` both actions can still match - however, an action with an `IActionConstraint` is always considered *better* than an action without.</span></span> <span data-ttu-id="3f169-390">Da `Edit()` über `[HttpGet]` verfügt, gilt die Methode als spezifischer und wird ausgewählt, wenn beide Aktionen das Verb erkennen.</span><span class="sxs-lookup"><span data-stu-id="3f169-390">So because `Edit()` has `[HttpGet]` it's considered more specific, and will be selected if both actions can match.</span></span>

<span data-ttu-id="3f169-391">Im Prinzip ist `IActionConstraint` eine Form der *Überladung*. Anstatt jedoch Methoden mit demselben Namen zu überladen, führt es zu einer Überladung zwischen Aktionen, die dieselbe URL erkennen.</span><span class="sxs-lookup"><span data-stu-id="3f169-391">Conceptually, `IActionConstraint` is a form of *overloading*, but instead of overloading methods with the same name, it's overloading between actions that match the same URL.</span></span> <span data-ttu-id="3f169-392">Beim Attributrouting wird auch `IActionConstraint` verwendet. Aus diesem Grund kann es zu Aktionen aus verschiedenen Controllern führen, die beide als Kandidaten gelten.</span><span class="sxs-lookup"><span data-stu-id="3f169-392">Attribute routing also uses `IActionConstraint` and can result in actions from different controllers both being considered candidates.</span></span>

<a name="iactionconstraint-impl-ref-label"></a>

### <a name="implementing-iactionconstraint"></a><span data-ttu-id="3f169-393">Implementieren von IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="3f169-393">Implementing IActionConstraint</span></span>

<span data-ttu-id="3f169-394">`IActionConstraint` lässt sich am einfachsten erstellen, indem Sie eine abgeleitete Klasse von `System.Attribute` erstellen und sie auf Ihren Aktionen und Controllern platzieren.</span><span class="sxs-lookup"><span data-stu-id="3f169-394">The simplest way to implement an `IActionConstraint` is to create a class derived from `System.Attribute` and place it on your actions and controllers.</span></span> <span data-ttu-id="3f169-395">MVC erkennt automatisch jedes `IActionConstraint`, das als Attribut verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="3f169-395">MVC will automatically discover any `IActionConstraint` that are applied as attributes.</span></span> <span data-ttu-id="3f169-396">Sie können mithilfe des Anwendungsmodells Einschränkungen anwenden. Dies ist wahrscheinlich die flexibelste Herangehensweise, da Sie über Metaprogrammierung festlegen können, wie die Einschränkungen angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="3f169-396">You can use the application model to apply constraints, and this is probably the most flexible approach as it allows you to metaprogram how they're applied.</span></span>

<span data-ttu-id="3f169-397">Im folgenden Beispiel wählt eine Einschränkung eine Aktion auf Grundlage einer *Landeskennzahl* aus den Routendaten aus.</span><span class="sxs-lookup"><span data-stu-id="3f169-397">In the following example a constraint chooses an action based on a *country code* from the route data.</span></span> <span data-ttu-id="3f169-398">[Das vollständige Beispiel finden Sie auf GitHub.](https://github.com/aspnet/Entropy/blob/dev/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs)</span><span class="sxs-lookup"><span data-stu-id="3f169-398">The [full sample on GitHub](https://github.com/aspnet/Entropy/blob/dev/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span></span>

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

<span data-ttu-id="3f169-399">Sie sind für die Implementierung der `Accept`-Methode und die Festlegung einer Reihenfolge (Order) verantwortlich, in der die Einschränkung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="3f169-399">You are responsible for implementing the `Accept` method and choosing an 'Order' for the constraint to execute.</span></span> <span data-ttu-id="3f169-400">In diesem Fall gibt die `Accept`-Methode `true` zurück, um zu kennzeichnen, dass die Aktion eine Übereinstimmung ist, wenn die `country`-Routenwerte übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="3f169-400">In this case, the `Accept` method returns `true` to denote the action is a match when the `country` route value matches.</span></span> <span data-ttu-id="3f169-401">Dies unterscheidet sich von `RouteValueAttribute` dahingehend, dass ein Fallback auf eine Aktion ohne Attribute zulässig ist.</span><span class="sxs-lookup"><span data-stu-id="3f169-401">This is different from a `RouteValueAttribute` in that it allows fallback to a non-attributed action.</span></span> <span data-ttu-id="3f169-402">In dem Beispiel wird gezeigt, dass wenn Sie eine `en-US`-Aktion definieren, ein Ländercode wie `fr-FR` auf einen generischeren Controller zurückgreift, für den `[CountrySpecific(...)]` nicht angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="3f169-402">The sample shows that if you define an `en-US` action then a country code like `fr-FR` will fall back to a more generic controller that doesn't have `[CountrySpecific(...)]` applied.</span></span>

<span data-ttu-id="3f169-403">Die `Order`-Eigenschaft entscheidet, zu welcher *Phase* die Einschränkung gehört.</span><span class="sxs-lookup"><span data-stu-id="3f169-403">The `Order` property decides which *stage* the constraint is part of.</span></span> <span data-ttu-id="3f169-404">Aktionseinschränkungen werden auf Grundlage von `Order` in Gruppen ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="3f169-404">Action constraints run in groups based on the `Order`.</span></span> <span data-ttu-id="3f169-405">Beispiel: Alle vom Framework bereitgestellten HTTP-Methodenattribute verwenden denselben `Order`-Wert, sodass sie in derselben Phase ausgeführt werden können.</span><span class="sxs-lookup"><span data-stu-id="3f169-405">For example, all of the framework provided HTTP method attributes use the same `Order` value so that they run in the same stage.</span></span> <span data-ttu-id="3f169-406">Es kann so viele Phasen geben, wie zur Implementierung der gewünschten Richtlinien notwendig.</span><span class="sxs-lookup"><span data-stu-id="3f169-406">You can have as many stages as you need to implement your desired policies.</span></span>

> [!TIP]
> <span data-ttu-id="3f169-407">Bei der Entscheidung über einen Wert für `Order` sollten Sie berücksichtigen, ob die Einschränkung vor HTTP-Methoden angewendet werden soll oder nicht.</span><span class="sxs-lookup"><span data-stu-id="3f169-407">To decide on a value for `Order` think about whether or not your constraint should be applied before HTTP methods.</span></span> <span data-ttu-id="3f169-408">Niedrigere Zahlen werden zuerst ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="3f169-408">Lower numbers run first.</span></span>
