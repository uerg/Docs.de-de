---
title: "Übersicht über ASP.NET Core MVC"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 89af38d1-52e0-4db7-b791-dbce909b0714
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/overview
ms.openlocfilehash: 67394b066c18a149a97b957d6478ba8301ea8147
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2017
---
# <a name="overview-of-aspnet-core-mvc"></a><span data-ttu-id="62584-103">Übersicht über ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="62584-103">Overview of ASP.NET Core MVC</span></span>

<span data-ttu-id="62584-104">Durch [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="62584-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="62584-105">ASP.NET Core MVC ist ein funktionsreiches Framework zum Erstellen von Web-apps und APIs mit Model-View-Controller entwerfen Muster.</span><span class="sxs-lookup"><span data-stu-id="62584-105">ASP.NET Core MVC is a rich framework for building web apps and APIs using the Model-View-Controller design pattern.</span></span>

## <a name="what-is-the-mvc-pattern"></a><span data-ttu-id="62584-106">Was ist das MVC-Schema?</span><span class="sxs-lookup"><span data-stu-id="62584-106">What is the MVC pattern?</span></span>

<span data-ttu-id="62584-107">Das Architekturschema Model-View-Controller (MVC) trennt eine Anwendung in drei Hauptgruppen von Komponenten: Modelle, Ansichten und Controllern.</span><span class="sxs-lookup"><span data-stu-id="62584-107">The Model-View-Controller (MVC) architectural pattern separates an application into three main groups of components: Models, Views, and Controllers.</span></span> <span data-ttu-id="62584-108">Das Muster erleichtert erzielen [Trennung von Anliegen](http://deviq.com/separation-of-concerns/).</span><span class="sxs-lookup"><span data-stu-id="62584-108">This pattern helps to achieve [separation of concerns](http://deviq.com/separation-of-concerns/).</span></span> <span data-ttu-id="62584-109">Mit diesem Muster werden benutzeranforderungen an einen Controller weitergeleitet, die für die Arbeit mit dem Modell auf Benutzeraktionen ausführen und/oder das Abrufen von Ergebnissen von Abfragen.</span><span class="sxs-lookup"><span data-stu-id="62584-109">Using this pattern, user requests are routed to a Controller which is responsible for working with the Model to perform user actions and/or retrieve results of queries.</span></span> <span data-ttu-id="62584-110">Der Controller wählt die Sicht, die dem Benutzer angezeigt, und alle dafür erforderlichen Modelldaten erhalten.</span><span class="sxs-lookup"><span data-stu-id="62584-110">The Controller chooses the View to display to the user, and provides it with any Model data it requires.</span></span>

<span data-ttu-id="62584-111">Das folgende Diagramm zeigt die drei wichtigsten Komponenten und welche verweisen auf die anderen:</span><span class="sxs-lookup"><span data-stu-id="62584-111">The following diagram shows the three main components and which ones reference the others:</span></span>

![MVC-Muster](overview/_static/mvc.png)

<span data-ttu-id="62584-113">Diese Abgrenzung Aufgaben können Sie die Anwendung im Hinblick auf Komplexität skaliert, da es einfacher ist, code, Debuggen und Testen etwas (Modell, Ansicht und Controller), das einen einzelnen Auftrag hat (und folgt dem [Prinzip einzigen Verantwortung ](http://deviq.com/single-responsibility-principle/)).</span><span class="sxs-lookup"><span data-stu-id="62584-113">This delineation of responsibilities helps you scale the application in terms of complexity because it’s easier to code, debug, and test something (model, view, or controller) that has a single job (and follows the [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/)).</span></span> <span data-ttu-id="62584-114">Es ist schwieriger zu aktualisieren, Test- und Debugcode, der über mindestens zwei dieser drei Bereiche verteilt Abhängigkeiten aufweist.</span><span class="sxs-lookup"><span data-stu-id="62584-114">It's more difficult to update, test, and debug code that has dependencies spread across two or more of these three areas.</span></span> <span data-ttu-id="62584-115">Beispielsweise ist Benutzeroberflächenlogik häufiger als Geschäftslogik zu ändern.</span><span class="sxs-lookup"><span data-stu-id="62584-115">For example, user interface logic tends to change more frequently than business logic.</span></span> <span data-ttu-id="62584-116">Wenn Präsentation Code und Geschäftslogik in einem einzigen Objekt kombiniert werden, müssen Sie zum Ändern eines Objekts, die Geschäftslogik enthält jedes Mal, wenn Sie die Benutzeroberfläche ändern.</span><span class="sxs-lookup"><span data-stu-id="62584-116">If presentation code and business logic are combined in a single object, you have to modify an object containing business logic every time you change the user interface.</span></span> <span data-ttu-id="62584-117">Dies ist wahrscheinlich, Fehler verursachen und erfordern, die ein erneuter Test alle Geschäftslogik nach jeder minimalen Benutzeroberfläche ändern.</span><span class="sxs-lookup"><span data-stu-id="62584-117">This is likely to introduce errors and require the retesting of all business logic after every minimal user interface change.</span></span>

> [!NOTE]
> <span data-ttu-id="62584-118">Die Ansicht und Controller richten sich nach dem Modell.</span><span class="sxs-lookup"><span data-stu-id="62584-118">Both the view and the controller depend on the model.</span></span> <span data-ttu-id="62584-119">Das Modell hängt jedoch weder für die Sicht als auch für den Controller.</span><span class="sxs-lookup"><span data-stu-id="62584-119">However, the model depends on neither the view nor the controller.</span></span> <span data-ttu-id="62584-120">Dies ist einer der Hauptvorteile der Trennung.</span><span class="sxs-lookup"><span data-stu-id="62584-120">This is one of the key benefits of the separation.</span></span> <span data-ttu-id="62584-121">Aufgrund dieser Trennung kann das Modell erstellt und getestet werden unabhängig von der die visuelle Darstellung.</span><span class="sxs-lookup"><span data-stu-id="62584-121">This separation allows the model to be built and tested independent of the visual presentation.</span></span>

### <a name="model-responsibilities"></a><span data-ttu-id="62584-122">Model Verantwortlichkeiten</span><span class="sxs-lookup"><span data-stu-id="62584-122">Model Responsibilities</span></span>

<span data-ttu-id="62584-123">Das Modell in einer MVC-Anwendung stellt den Zustand der Anwendung und alle Geschäftslogiken oder Vorgänge, die von ihm ausgeführt werden soll.</span><span class="sxs-lookup"><span data-stu-id="62584-123">The Model in an MVC application represents the state of the application and any business logic or operations that should be performed by it.</span></span> <span data-ttu-id="62584-124">Im Modell zusammen mit jeder Implementierungslogik für das Beibehalten des Zustands der Anwendung sollte Geschäftslogik gekapselt werden.</span><span class="sxs-lookup"><span data-stu-id="62584-124">Business logic should be encapsulated in the model, along with any implementation logic for persisting the state of the application.</span></span> <span data-ttu-id="62584-125">Stark typisierte Ansichten mithilfe für diese Sicht zeigt in der Regel von ViewModel-Typen, die speziell für die Daten enthalten; der Controller wird erstellen und füllen diese ViewModel-Instanzen aus dem Modell.</span><span class="sxs-lookup"><span data-stu-id="62584-125">Strongly-typed views will typically use ViewModel types specifically designed to contain the data to display on that view; the controller will create and populate these ViewModel instances from the model.</span></span>

> [!NOTE]
> <span data-ttu-id="62584-126">Es gibt viele Möglichkeiten, das Modell in einer app zu organisieren, die das MVC-Architekturschema verwendet.</span><span class="sxs-lookup"><span data-stu-id="62584-126">There are many ways to organize the model in an app that uses the MVC architectural pattern.</span></span> <span data-ttu-id="62584-127">Erfahren Sie mehr über einige [verschiedene Arten von Modelltypen](http://deviq.com/kinds-of-models/).</span><span class="sxs-lookup"><span data-stu-id="62584-127">Learn more about some [different kinds of model types](http://deviq.com/kinds-of-models/).</span></span>

### <a name="view-responsibilities"></a><span data-ttu-id="62584-128">Zuständigkeitsbereiche anzeigen</span><span class="sxs-lookup"><span data-stu-id="62584-128">View Responsibilities</span></span>

<span data-ttu-id="62584-129">Ansichten sind für die Darstellung von Inhalten über die Benutzeroberfläche zuständig.</span><span class="sxs-lookup"><span data-stu-id="62584-129">Views are responsible for presenting content through the user interface.</span></span> <span data-ttu-id="62584-130">Verwenden sie die [Razor-Ansichtsmodul](#razor-view-engine) .NET Code in HTML-Markup eingebettet werden sollen.</span><span class="sxs-lookup"><span data-stu-id="62584-130">They use the [Razor view engine](#razor-view-engine) to embed .NET code in HTML markup.</span></span> <span data-ttu-id="62584-131">Minimale Logik in Ansichten muss, und eine Logik in ihnen sollte beziehen Inhalt darstellen.</span><span class="sxs-lookup"><span data-stu-id="62584-131">There should be minimal logic within views, and any logic in them should relate to presenting content.</span></span> <span data-ttu-id="62584-132">Wenn Sie feststellen, nötig, viel Logik in der Ansicht auszuführen, Dateien, um Daten aus einem komplexen Modell anzuzeigen, können Sie verwenden eine [Ansichtskomponente](views/view-components.md), ViewModel, oder der Vorlage anzeigen, um die Sicht zu vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="62584-132">If you find the need to perform a great deal of logic in view files in order to display data from a complex model, consider using a [View Component](views/view-components.md), ViewModel, or view template to simplify the view.</span></span>

### <a name="controller-responsibilities"></a><span data-ttu-id="62584-133">Controller Zuständigkeiten</span><span class="sxs-lookup"><span data-stu-id="62584-133">Controller Responsibilities</span></span>

<span data-ttu-id="62584-134">Controller sind die Komponenten behandeln Benutzerinteraktionen, mit dem Modell arbeiten und letztlich wählen Sie eine Ansicht zu rendern.</span><span class="sxs-lookup"><span data-stu-id="62584-134">Controllers are the components that handle user interaction, work with the model, and ultimately select a view to render.</span></span> <span data-ttu-id="62584-135">In einer MVC-Anwendung zeigt die Ansicht nur Informationen; der Controller behandelt und antwortet auf Benutzereingaben und Interaktion.</span><span class="sxs-lookup"><span data-stu-id="62584-135">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span> <span data-ttu-id="62584-136">In der MVC-Muster, der Controller ist der anfängliche Einstiegspunkt und ist verantwortlich für die Auswahl, welches Modell zum Arbeiten mit Typen und die zu rendernde Ansicht (daher Sie seinen Namen - Steuerelemente wie die app auf eine bestimmte Anforderung reagiert).</span><span class="sxs-lookup"><span data-stu-id="62584-136">In the MVC pattern, the controller is the initial entry point, and is responsible for selecting which model types to work with and which view to render (hence its name - it controls how the app responds to a given request).</span></span>

> [!NOTE]
> <span data-ttu-id="62584-137">Domänencontroller sollten durch zu viele Aufgaben nicht übermäßig kompliziert sein.</span><span class="sxs-lookup"><span data-stu-id="62584-137">Controllers should not be overly complicated by too many responsibilities.</span></span> <span data-ttu-id="62584-138">Damit Controllerlogik nicht übermäßig kompliziert, verwenden die [Prinzip einzigen Verantwortung](http://deviq.com/single-responsibility-principle/) Push Geschäftslogik aus dem Controller und in das Domänenmodell.</span><span class="sxs-lookup"><span data-stu-id="62584-138">To keep controller logic from becoming overly complex, use the [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/) to push business logic out of the controller and into the domain model.</span></span>

>[!TIP]
> <span data-ttu-id="62584-139">Wenn Sie feststellen, dass Ihre Controlleraktionen häufig die gleichen Arten von Aktionen ausführen, führen Sie Sie der [nicht selbst Prinzip wiederholen](http://deviq.com/don-t-repeat-yourself/) umgezogen diese häufig verwendete Aktionen in [Filter](#filters).</span><span class="sxs-lookup"><span data-stu-id="62584-139">If you find that your controller actions frequently perform the same kinds of actions, you can follow the [Don't Repeat Yourself principle](http://deviq.com/don-t-repeat-yourself/) by moving these common actions into [filters](#filters).</span></span>

## <a name="what-is-aspnet-core-mvc"></a><span data-ttu-id="62584-140">Was ist der Kern von ASP.NET-MVC</span><span class="sxs-lookup"><span data-stu-id="62584-140">What is ASP.NET Core MVC</span></span>

<span data-ttu-id="62584-141">Das ASP.NET-MVC-Framework Core ist eine einfache open Source mitgliedschaftsbasierte PresentationFramework für die Verwendung mit ASP.NET Core optimiert.</span><span class="sxs-lookup"><span data-stu-id="62584-141">The ASP.NET Core MVC framework is a lightweight, open source, highly testable presentation framework optimized for use with ASP.NET Core.</span></span>

<span data-ttu-id="62584-142">ASP.NET Core MVC Weise Mustern basierende dynamische Websites erstellen, die eine klare Trennung von Anliegen ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="62584-142">ASP.NET Core MVC provides a patterns-based way to build dynamic websites that enables a clean separation of concerns.</span></span> <span data-ttu-id="62584-143">Er bietet Ihnen die vollständige Kontrolle über Markup, TDD-freundliche Entwicklung und Verwendungen unterstützt die aktuellsten Webstandards.</span><span class="sxs-lookup"><span data-stu-id="62584-143">It gives you full control over markup, supports TDD-friendly development and uses the latest web standards.</span></span>

## <a name="features"></a><span data-ttu-id="62584-144">Features</span><span class="sxs-lookup"><span data-stu-id="62584-144">Features</span></span>

<span data-ttu-id="62584-145">ASP.NET Core MVC umfasst Folgendes:</span><span class="sxs-lookup"><span data-stu-id="62584-145">ASP.NET Core MVC includes the following:</span></span>

* [<span data-ttu-id="62584-146">Routing</span><span class="sxs-lookup"><span data-stu-id="62584-146">Routing</span></span>](#routing)
* [<span data-ttu-id="62584-147">Wurden die modellbindung</span><span class="sxs-lookup"><span data-stu-id="62584-147">Model binding</span></span>](#model-binding)
* [<span data-ttu-id="62584-148">Modellvalidierung</span><span class="sxs-lookup"><span data-stu-id="62584-148">Model validation</span></span>](#model-validation)
* [<span data-ttu-id="62584-149">Abhängigkeitsinjektion</span><span class="sxs-lookup"><span data-stu-id="62584-149">Dependency injection</span></span>](../fundamentals/dependency-injection.md)
* [<span data-ttu-id="62584-150">Filter</span><span class="sxs-lookup"><span data-stu-id="62584-150">Filters</span></span>](#filters)
* [<span data-ttu-id="62584-151">Bereiche</span><span class="sxs-lookup"><span data-stu-id="62584-151">Areas</span></span>](#areas)
* [<span data-ttu-id="62584-152">Web-APIs</span><span class="sxs-lookup"><span data-stu-id="62584-152">Web APIs</span></span>](#web-apis)
* [<span data-ttu-id="62584-153">Prüfbarkeit</span><span class="sxs-lookup"><span data-stu-id="62584-153">Testability</span></span>](#testability)
* [<span data-ttu-id="62584-154">Razor-Ansichtsmodul</span><span class="sxs-lookup"><span data-stu-id="62584-154">Razor view engine</span></span>](#razor-view-engine)
* [<span data-ttu-id="62584-155">Stark typisierte Ansichten</span><span class="sxs-lookup"><span data-stu-id="62584-155">Strongly typed views</span></span>](#strongly-typed-views)
* [<span data-ttu-id="62584-156">Tag Helpers (Taghilfsprogramme)</span><span class="sxs-lookup"><span data-stu-id="62584-156">Tag Helpers</span></span>](#tag-helpers)
* [<span data-ttu-id="62584-157">Anzeigen von Komponenten</span><span class="sxs-lookup"><span data-stu-id="62584-157">View Components</span></span>](#view-components)

### <a name="routing"></a><span data-ttu-id="62584-158">Routing</span><span class="sxs-lookup"><span data-stu-id="62584-158">Routing</span></span>

<span data-ttu-id="62584-159">ASP.NET Core MVC basiert auf der Basis von [ASP.NET Core routing](../fundamentals/routing.md), eine leistungsstarke URL-Zuordnung-Komponente, mit dem Sie Anwendungen mit verständlichen und durchsuchbaren URLs zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="62584-159">ASP.NET Core MVC is built on top of [ASP.NET Core's routing](../fundamentals/routing.md), a powerful URL-mapping component that lets you build applications that have comprehensible and searchable URLs.</span></span> <span data-ttu-id="62584-160">Dadurch können Sie die URL-Benennungsschemas für Ihre Anwendung zu definieren, die funktionieren gut für Search Engine Optimization (SEO) und für die linkgenerierung ohne Berücksichtigung wie die Dateien auf dem Webserver organisiert sind.</span><span class="sxs-lookup"><span data-stu-id="62584-160">This enables you to define your application's URL naming patterns that work well for search engine optimization (SEO) and for link generation, without regard for how the files on your web server are organized.</span></span> <span data-ttu-id="62584-161">Sie können definieren, die Routen, die mit einer geeigneten Route Vorlage, die routeneinschränkungen-Wert, Standardwerte und optionale Werte unterstützt.</span><span class="sxs-lookup"><span data-stu-id="62584-161">You can define your routes using a convenient route template syntax that supports route value constraints, defaults and optional values.</span></span>

<span data-ttu-id="62584-162">*Routing konventionsbasierte* akzeptiert, ermöglicht Sie die URL global definieren Formaten, bei denen die Anwendung, und wie diese jeweils über Formaten ordnet eine bestimmte Aktionsmethode auf angegebene Controller.</span><span class="sxs-lookup"><span data-stu-id="62584-162">*Convention-based routing* enables you to globally define the URL formats that your application accepts and how each of those formats maps to a specific action method on given controller.</span></span> <span data-ttu-id="62584-163">Wenn eine eingehende Anforderung empfangen wird, wird das Routingmodul analysiert die URL und liegt eine Übereinstimmung mit einer der definierten URL-Formate und ruft dann die zugehörigen Controlleraktionsmethode.</span><span class="sxs-lookup"><span data-stu-id="62584-163">When an incoming request is received, the routing engine parses the URL and matches it to one of the defined URL formats, and then calls the associated controller's action method.</span></span>

```csharp
routes.MapRoute(name: "Default", template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="62584-164">*-Attribut routing* ermöglicht es Ihnen, die Routinginformationen angeben, durch das ergänzen der Controller und Aktionen mit Attributen, die Routen für die Anwendung zu definieren.</span><span class="sxs-lookup"><span data-stu-id="62584-164">*Attribute routing* enables you to specify routing information by decorating your controllers and actions with attributes that define your application's routes.</span></span> <span data-ttu-id="62584-165">Dies bedeutet, dass die Routendefinitionen, neben dem Controller und Aktion platziert werden, mit denen sie verknüpft sind.</span><span class="sxs-lookup"><span data-stu-id="62584-165">This means that your route definitions are placed next to the controller and action with which they're associated.</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [1, 4]}} -->

```csharp
[Route("api/[controller]")]
public class ProductsController : Controller
{
  [HttpGet("{id}")]
  public IActionResult GetProduct(int id)
  {
    ...
  }
}
```

### <a name="model-binding"></a><span data-ttu-id="62584-166">Wurden die modellbindung</span><span class="sxs-lookup"><span data-stu-id="62584-166">Model binding</span></span>

<span data-ttu-id="62584-167">ASP.NET Core MVC [modellbindung](models/model-binding.md) konvertiert Client-Anforderungsdaten (Formularwerte, Routendaten, Abfragezeichenfolgen-Parameter, HTTP-Header) in Objekte, die der Controller verarbeiten kann.</span><span class="sxs-lookup"><span data-stu-id="62584-167">ASP.NET Core MVC [model binding](models/model-binding.md) converts client request data  (form values, route data, query string parameters, HTTP headers) into objects that the controller can handle.</span></span> <span data-ttu-id="62584-168">Daher muss nicht mehr der Controllerlogik zu ermitteln, die eingehende Anforderungsdaten auszuführen; Sie enthalten die Daten einfach, als Parameter für die Aktionsmethoden.</span><span class="sxs-lookup"><span data-stu-id="62584-168">As a result, your controller logic doesn't have to do the work of figuring out the incoming request data; it simply has the data as parameters to its action methods.</span></span>

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null) { ... }
   ```

### <a name="model-validation"></a><span data-ttu-id="62584-169">modellvalidierung</span><span class="sxs-lookup"><span data-stu-id="62584-169">Model validation</span></span>

<span data-ttu-id="62584-170">ASP.NET Core MVC unterstützt [Überprüfung](models/validation.md) durch das ergänzen der Model-Objekts mit Daten Anmerkung Validierungsattribute.</span><span class="sxs-lookup"><span data-stu-id="62584-170">ASP.NET Core MVC supports [validation](models/validation.md) by decorating your model object with data annotation validation attributes.</span></span> <span data-ttu-id="62584-171">Die überprüfungsattribute werden auf dem Client überprüft, bevor Werte an den Server zurückgesendet werden, als auch auf dem Server vor der Controlleraktion aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="62584-171">The validation attributes are checked on the client side before values are posted to the server, as well as on the server before the controller action is called.</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [4, 5, 8, 9]}} -->

```csharp
using System.ComponentModel.DataAnnotations;
public class LoginViewModel
{
    [Required]
    [EmailAddress]
    public string Email { get; set; }

    [Required]
    [DataType(DataType.Password)]
    public string Password { get; set; }

    [Display(Name = "Remember me?")]
    public bool RememberMe { get; set; }
}
```

<span data-ttu-id="62584-172">Eine Controlleraktion:</span><span class="sxs-lookup"><span data-stu-id="62584-172">A controller action:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [3]}} -->

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null)
{
    if (ModelState.IsValid)
    {
      // work with the model
    }
    // If we got this far, something failed, redisplay form
    return View(model);
}
```

<span data-ttu-id="62584-173">Das Framework verarbeitet Anforderungsdaten sowohl auf dem Client als auch auf dem Server zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="62584-173">The framework will handle validating request data both on the client and on the server.</span></span> <span data-ttu-id="62584-174">Validierungslogik auf Modelltypen angegeben wird, die den gerenderten Ansichten als unaufdringlichen Anmerkungen hinzugefügt und wird im Browser mit erzwungen [jQuery-Validierung](https://jqueryvalidation.org/).</span><span class="sxs-lookup"><span data-stu-id="62584-174">Validation logic specified on model types is added to the rendered views as unobtrusive annotations and is enforced in the browser with [jQuery Validation](https://jqueryvalidation.org/).</span></span>

### <a name="dependency-injection"></a><span data-ttu-id="62584-175">Abhängigkeitsinjektion</span><span class="sxs-lookup"><span data-stu-id="62584-175">Dependency injection</span></span>

<span data-ttu-id="62584-176">ASP.NET Core verfügt über integrierte Unterstützung für [abhängigkeiteneinschleusung (DI)](../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="62584-176">ASP.NET Core has built-in support for [dependency injection (DI)](../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="62584-177">In ASP.NET-MVC Core [Controller](controllers/dependency-injection.md) können Anforderung erforderlichen Dienste über ihre Konstruktoren, die ihnen ermöglicht, befolgen die [expliziten Abhängigkeiten Prinzip](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="62584-177">In ASP.NET Core MVC, [controllers](controllers/dependency-injection.md) can request needed services through their constructors, allowing them to follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="62584-178">Ihrer app können Sie auch [Abhängigkeitsinjektion in der Sicht Dateien](views/dependency-injection.md)unter Verwendung der `@inject` Richtlinie:</span><span class="sxs-lookup"><span data-stu-id="62584-178">Your app can also use [dependency injection in view files](views/dependency-injection.md), using the `@inject` directive:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [1]}} -->

```html
@inject SomeService ServiceName
<!DOCTYPE html>
<html>
<head>
  <title>@ServiceName.GetTitle</title>
</head>
<body>
  <h1>@ServiceName.GetTitle</h1>
</body>
</html>
```

### <a name="filters"></a><span data-ttu-id="62584-179">Filter</span><span class="sxs-lookup"><span data-stu-id="62584-179">Filters</span></span>

<span data-ttu-id="62584-180">[Filter](controllers/filters.md) ermöglicht es Entwicklern, querschnittliche bedenken, wie Ausnahmebehandlung oder Autorisierung zu kapseln.</span><span class="sxs-lookup"><span data-stu-id="62584-180">[Filters](controllers/filters.md) help developers encapsulate cross-cutting concerns, like exception handling or authorization.</span></span> <span data-ttu-id="62584-181">Filter Ausführen von benutzerdefinierten vorab und nachträglich verarbeiteten Logik für Aktionsmethoden zu aktivieren und zu bestimmten Zeitpunkten in der Ausführungspipeline für eine bestimmte Anforderung Ausführung konfiguriert werden können.</span><span class="sxs-lookup"><span data-stu-id="62584-181">Filters enable running custom pre- and post-processing logic for action methods, and can be configured to run at certain points within the execution pipeline for a given request.</span></span> <span data-ttu-id="62584-182">Filter können auf Domänencontrollern oder Aktionen, die als Attribute angewendet werden (oder Global ausgeführt werden).</span><span class="sxs-lookup"><span data-stu-id="62584-182">Filters can be applied to controllers or actions as attributes (or can be run globally).</span></span> <span data-ttu-id="62584-183">Mehrere Filter (z. B. `Authorize`) im Framework enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="62584-183">Several filters (such as `Authorize`) are included in the framework.</span></span>


```csharp
[Authorize]
   public class AccountController : Controller
   {

```

### <a name="areas"></a><span data-ttu-id="62584-184">Bereiche</span><span class="sxs-lookup"><span data-stu-id="62584-184">Areas</span></span>

<span data-ttu-id="62584-185">[Bereiche](controllers/areas.md) bieten eine Möglichkeit, eine große ASP.NET Core MVC-Web-app in kleinere funktionale Einheiten zu partitionieren.</span><span class="sxs-lookup"><span data-stu-id="62584-185">[Areas](controllers/areas.md) provide a way to partition a large ASP.NET Core MVC Web app into smaller functional groupings.</span></span> <span data-ttu-id="62584-186">Ein Bereich ist letztendlich eine MVC-Struktur in einer Anwendung.</span><span class="sxs-lookup"><span data-stu-id="62584-186">An area is effectively an MVC structure inside an application.</span></span> <span data-ttu-id="62584-187">Klicken Sie in einer MVC-Projekt logische Komponenten wie das Modell, Controller und Ansicht in unterschiedlichen Ordnern beibehalten werden, und MVC verwendet Konventionen zur Namensgebung, um die Beziehung zwischen diesen Komponenten zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="62584-187">In an MVC project, logical components like Model, Controller, and View are kept in different folders, and MVC uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="62584-188">Für eine große app kann es vorteilhaft sein, die app in separate hohe Ebene Bereiche der Funktionalität partitioniert sein.</span><span class="sxs-lookup"><span data-stu-id="62584-188">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="62584-189">Z. B. eine e-Commerce-app mit mehreren Geschäftsbereichen, z. B. Auschecken, Abrechnung und Suche usw.. Jede dieser Einheiten haben ihre eigenen logischen Komponentenansichten, Controller und Modelle.</span><span class="sxs-lookup"><span data-stu-id="62584-189">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search etc. Each of these units have their own logical component views, controllers, and models.</span></span>

### <a name="web-apis"></a><span data-ttu-id="62584-190">Web-APIs</span><span class="sxs-lookup"><span data-stu-id="62584-190">Web APIs</span></span>

<span data-ttu-id="62584-191">Abgesehen davon, dass eine hervorragende Plattform zum Erstellen von Websites, weist ASP.NET Core MVC bietet umfassende Unterstützung für das Erstellen von Web-APIs.</span><span class="sxs-lookup"><span data-stu-id="62584-191">In addition to being a great platform for building web sites, ASP.NET Core MVC has great support for building Web APIs.</span></span> <span data-ttu-id="62584-192">Sie können Dienste erstellen, die eine Breite Palette von Clients einschließlich Browsern und mobilen Geräten erreichen können.</span><span class="sxs-lookup"><span data-stu-id="62584-192">You can build services that can reach a broad range of clients including browsers and mobile devices.</span></span>

<span data-ttu-id="62584-193">Das Framework bietet Unterstützung für HTTP-Inhalt-Aushandlung mit integrierter Unterstützung für [Formatieren von Daten](models/formatting.md) als JSON oder XML.</span><span class="sxs-lookup"><span data-stu-id="62584-193">The framework includes support for HTTP content-negotiation with built-in support for [formatting data](models/formatting.md) as JSON or XML.</span></span> <span data-ttu-id="62584-194">Schreiben von [benutzerdefinierten Formatierer](advanced/custom-formatters.md) zum Hinzufügen der Unterstützung für Ihre eigenen Formate.</span><span class="sxs-lookup"><span data-stu-id="62584-194">Write [custom formatters](advanced/custom-formatters.md) to add support for your own formats.</span></span>

<span data-ttu-id="62584-195">Verwenden Sie zum Aktivieren der Unterstützung für Hypermedia linkgenerierung.</span><span class="sxs-lookup"><span data-stu-id="62584-195">Use link generation to enable support for hypermedia.</span></span> <span data-ttu-id="62584-196">Aktivieren der Unterstützung für einfache [Cross-Origin Resource sharing (CORS)](http://www.w3.org/TR/cors/) , damit Ihre Web-APIs für mehrere Webanwendungen freigegeben sein können.</span><span class="sxs-lookup"><span data-stu-id="62584-196">Easily enable support for [cross-origin resource sharing (CORS)](http://www.w3.org/TR/cors/) so that your Web APIs can be shared across multiple Web applications.</span></span>

### <a name="testability"></a><span data-ttu-id="62584-197">Prüfbarkeit</span><span class="sxs-lookup"><span data-stu-id="62584-197">Testability</span></span>

<span data-ttu-id="62584-198">Das Framework Schnittstellen und Abhängigkeitsinjektion nutzen Es eignet sich besonders gut für Komponententests und das Framework enthält Barrierefreiheitsfunktionen (z. B. ein TestHost und InMemory-Anbieter für Entity Framework) [Integrationstests](../testing/integration-testing.md) schnelle und einfache als auch.</span><span class="sxs-lookup"><span data-stu-id="62584-198">The framework's use of interfaces and dependency injection make it well-suited to unit testing, and the framework includes features (like a TestHost and InMemory provider for Entity Framework) that make [integration testing](../testing/integration-testing.md) quick and easy as well.</span></span> <span data-ttu-id="62584-199">Erfahren Sie mehr über [testen Controllerlogik](controllers/testing.md).</span><span class="sxs-lookup"><span data-stu-id="62584-199">Learn more about [testing controller logic](controllers/testing.md).</span></span>

### <a name="razor-view-engine"></a><span data-ttu-id="62584-200">Razor-Ansichtsmodul</span><span class="sxs-lookup"><span data-stu-id="62584-200">Razor view engine</span></span>

<span data-ttu-id="62584-201">[ASP.NET Core MVC-Ansichten](views/overview.md) verwenden die [Razor-Ansichtsmodul](views/razor.md) Ansichten gerendert werden.</span><span class="sxs-lookup"><span data-stu-id="62584-201">[ASP.NET Core MVC views](views/overview.md) use the [Razor view engine](views/razor.md) to render views.</span></span> <span data-ttu-id="62584-202">Razor ist eine kompakte, ausdrucksbasierte und flüssigen Vorlage Markupsprache zum Definieren von Ansichten mit eingebetteten C#-Code.</span><span class="sxs-lookup"><span data-stu-id="62584-202">Razor is a compact, expressive and fluid template markup language for defining views using embedded C# code.</span></span> <span data-ttu-id="62584-203">Razor wird verwendet, um Webinhalte auf dem Server dynamisch zu generieren.</span><span class="sxs-lookup"><span data-stu-id="62584-203">Razor is used to dynamically generate web content on the server.</span></span> <span data-ttu-id="62584-204">Sie können den Servercode ordnungsgemäß mit Client-Side-Inhalte und Code kombinieren.</span><span class="sxs-lookup"><span data-stu-id="62584-204">You can cleanly mix server code with client side content and code.</span></span>

```text
<ul>
  @for (int i = 0; i < 5; i++) {
    <li>List item @i</li>
  }
</ul>
```

<span data-ttu-id="62584-205">Mithilfe der Razor-Ansichtsmodul können [Layouts](views/layout.md), [Teilansichten](views/partial.md) und ersetzbaren Abschnitte.</span><span class="sxs-lookup"><span data-stu-id="62584-205">Using the Razor view engine you can define [layouts](views/layout.md), [partial views](views/partial.md) and replaceable sections.</span></span>

### <a name="strongly-typed-views"></a><span data-ttu-id="62584-206">Stark typisierte Ansichten</span><span class="sxs-lookup"><span data-stu-id="62584-206">Strongly typed views</span></span>

<span data-ttu-id="62584-207">In MVC Razor-Ansichten können stark basierend auf Ihrem Modell eingegeben werden.</span><span class="sxs-lookup"><span data-stu-id="62584-207">Razor views in MVC can be strongly typed based on your model.</span></span> <span data-ttu-id="62584-208">Domänencontroller können mit Ansichten, die Ihre Ansichten, typüberprüfung und IntelliSense-Unterstützung aktivieren ein stark typisiertes Modell übergeben.</span><span class="sxs-lookup"><span data-stu-id="62584-208">Controllers can pass a strongly typed model to views enabling your views to have type checking and IntelliSense support.</span></span>

<span data-ttu-id="62584-209">Die folgende Sicht definiert z. B. ein Modell vom Typ `IEnumerable<Product>`:</span><span class="sxs-lookup"><span data-stu-id="62584-209">For example, the following view defines a model of type `IEnumerable<Product>`:</span></span>

```html
@model IEnumerable<Product>
<ul>
    @foreach (Product p in Model)
    {
        <li>@p.Name</li>
    }
</ul>
```

### <a name="tag-helpers"></a><span data-ttu-id="62584-210">Tag-Hilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="62584-210">Tag Helpers</span></span>

<span data-ttu-id="62584-211">[Kennzeichnen von Hilfsprogrammen](views/tag-helpers/intro.md) Aktivieren der serverseitigen Code zu erstellen und das rendering von HTML-Elementen in Razor-Dateien teilnehmen.</span><span class="sxs-lookup"><span data-stu-id="62584-211">[Tag Helpers](views/tag-helpers/intro.md) enable server side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="62584-212">Können Sie benutzerdefinierte Tags definieren Tag Hilfsprogramme (z. B. `<environment>`) oder um das Verhalten der vorhandene Tags ändern (z. B. `<label>`).</span><span class="sxs-lookup"><span data-stu-id="62584-212">You can use tag helpers to define custom tags (for example, `<environment>`) or to modify the behavior of existing tags (for example, `<label>`).</span></span> <span data-ttu-id="62584-213">Tag-Hilfsprogramme, die auf bestimmte Elemente, die basierend auf der Elementname und dessen Attribute gebunden werden.</span><span class="sxs-lookup"><span data-stu-id="62584-213">Tag Helpers bind to specific elements based on the element name and its attributes.</span></span> <span data-ttu-id="62584-214">Sie bieten die Vorteile des serverseitigen Renderings gelernter Bearbeitungsart HTML.</span><span class="sxs-lookup"><span data-stu-id="62584-214">They provide the benefits of server-side rendering while still preserving an HTML editing experience.</span></span>

<span data-ttu-id="62584-215">Es gibt viele integrierte Tag Hilfen für allgemeine Aufgaben – z. B. das Erstellen von Formularen, Links, Laden von Ressourcen und weitere- und sogar noch stärker verfügbar in öffentlichen GitHub-Repositorys und als NuGet-Pakete.</span><span class="sxs-lookup"><span data-stu-id="62584-215">There are many built-in Tag Helpers for common tasks - such as creating forms, links, loading assets and more - and even more available in public GitHub repositories and as NuGet packages.</span></span> <span data-ttu-id="62584-216">Tag-Hilfsprogramme in c# erstellt wurden, und sie Zielen auf HTML-Elemente, die basierend auf Elementnamen, Attributnamen oder übergeordneten Tags.</span><span class="sxs-lookup"><span data-stu-id="62584-216">Tag Helpers are authored in C#, and they target HTML elements based on element name, attribute name, or parent tag.</span></span> <span data-ttu-id="62584-217">Z. B. die integrierten LinkTagHelper kann verwendet werden, um einen Link zum Erstellen der `Login` Aktion von der `AccountsController`:</span><span class="sxs-lookup"><span data-stu-id="62584-217">For example, the built-in LinkTagHelper can be used to create a link to the `Login` action of the `AccountsController`:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [3]}} -->

```html
<p>
    Thank you for confirming your email.
    Please <a asp-controller="Account" asp-action="Login">Click here to Log in</a>.
</p>
```

<span data-ttu-id="62584-218">Die `EnvironmentTagHelper` können verwendet werden, um Ihre Ansichten (z. B. unformatierte oder verkleinerte) basierend auf der Common Language Runtime-Umgebung, z. B. Entwicklungs-, Staging- oder Produktionsumgebung anderen Skripts einschließt:</span><span class="sxs-lookup"><span data-stu-id="62584-218">The `EnvironmentTagHelper` can be used to include different scripts in your views (for example, raw or minified) based on the runtime environment, such as Development, Staging, or Production:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [1, 3, 4, 9]}} -->

```html
<environment names="Development">
    <script src="~/lib/jquery/dist/jquery.js"></script>
</environment>
<environment names="Staging,Production">
    <script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.1.4.min.js"
            asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
            asp-fallback-test="window.jQuery">
    </script>
</environment>
```

<span data-ttu-id="62584-219">Tag Hilfsvorlagen können eine HTML-freundliche entwicklungserfahrung und eine umfassende IntelliSense-Umgebung zum Erstellen von HTML und Razor-Markup.</span><span class="sxs-lookup"><span data-stu-id="62584-219">Tag Helpers provide an HTML-friendly development experience and a rich IntelliSense environment for creating HTML and Razor markup.</span></span> <span data-ttu-id="62584-220">Die meisten integrierten Tag Hilfsprogramme vorhandenen HTML-Elemente als Ziel und geben Sie serverseitige Attribute für das Element.</span><span class="sxs-lookup"><span data-stu-id="62584-220">Most of the built-in Tag Helpers target existing HTML elements and provide server-side attributes for the element.</span></span>

### <a name="view-components"></a><span data-ttu-id="62584-221">Anzeigen von Komponenten</span><span class="sxs-lookup"><span data-stu-id="62584-221">View Components</span></span>

<span data-ttu-id="62584-222">[Anzeigen von Komponenten](views/view-components.md) ermöglichen es Ihnen, Renderinglogik Verpacken und diesen in der gesamten Anwendung wiederverwenden.</span><span class="sxs-lookup"><span data-stu-id="62584-222">[View Components](views/view-components.md) allow you to package rendering logic and reuse it throughout the application.</span></span> <span data-ttu-id="62584-223">Ähnlich [Teilansichten](views/partial.md), aber mit zugeordneten Logik.</span><span class="sxs-lookup"><span data-stu-id="62584-223">They're similar to [partial views](views/partial.md), but with associated logic.</span></span>
