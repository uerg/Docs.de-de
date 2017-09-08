---
title: "Sichten (Übersicht)"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 668c320d-c050-45e3-8161-2f460dc93b2f
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/overview
ms.openlocfilehash: a93ee8165be52e33c2e7da4d3fee2c8225864db9
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
# <a name="rendering-html-with-views-in-aspnet-core-mvc"></a><span data-ttu-id="bbba2-103">Rendern von HTML mit ASP.NET Core MVC-Ansichten</span><span class="sxs-lookup"><span data-stu-id="bbba2-103">Rendering HTML with views in ASP.NET Core MVC</span></span>

<span data-ttu-id="bbba2-104">Durch [Steve Smith](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="bbba2-104">By [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="bbba2-105">ASP.NET Core MVC-Controller können Ergebnisse formatierte mit *Ansichten*.</span><span class="sxs-lookup"><span data-stu-id="bbba2-105">ASP.NET Core MVC controllers can return formatted results using *views*.</span></span>

## <a name="what-are-views"></a><span data-ttu-id="bbba2-106">Was sind Ansichten?</span><span class="sxs-lookup"><span data-stu-id="bbba2-106">What are Views?</span></span>

<span data-ttu-id="bbba2-107">In der Model-View-Controller (MVC)-Muster die *Ansicht* kapselt die Präsentation Details der Interaktion mit der app des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="bbba2-107">In the Model-View-Controller (MVC) pattern, the *view* encapsulates the presentation details of the user's interaction with the app.</span></span> <span data-ttu-id="bbba2-108">Ansichten sind HTML-Vorlagen mit eingebetteten Code, die Inhalt an den Client senden zu generieren.</span><span class="sxs-lookup"><span data-stu-id="bbba2-108">Views are HTML templates with embedded code that generate content to send to the client.</span></span> <span data-ttu-id="bbba2-109">Verwendet [Razor-Syntax](razor.md), sodass Code für die Interaktion mit HTML mit minimalem Codeeinsatz oder Zeremonie.</span><span class="sxs-lookup"><span data-stu-id="bbba2-109">Views use [Razor syntax](razor.md), which allows code to interact with HTML with minimal code or ceremony.</span></span>

<span data-ttu-id="bbba2-110">ASP.NET Core MVC-Ansichten werden *cshtml* Dateien standardmäßig in einem *Ansichten* Ordner innerhalb der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="bbba2-110">ASP.NET Core MVC views are *.cshtml* files stored by default in a *Views* folder within the application.</span></span> <span data-ttu-id="bbba2-111">In der Regel wird jedem Controller einen eigenen Ordner haben, in der Ansichten für bestimmte Controlleraktionen sind.</span><span class="sxs-lookup"><span data-stu-id="bbba2-111">Typically, each controller will have its own folder, in which are views for specific controller actions.</span></span>

![Ansichtenordner im Projektmappen-Explorer](overview/_static/views_solution_explorer.png)

<span data-ttu-id="bbba2-113">Zusätzlich zu den Ansichten aktionsspezifische [Teilansichten](partial.md), [Layouts und andere Dateien besonderen Ansicht](layout.md) können zur Wiederholung reduzieren und ermöglichen die Wiederverwendung in der app-Ansichten verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="bbba2-113">In addition to action-specific views, [partial views](partial.md), [layouts, and other special view files](layout.md) can be used to help reduce repetition and allow for reuse within the app's views.</span></span>

## <a name="benefits-of-using-views"></a><span data-ttu-id="bbba2-114">Vorteile der Verwendung von Sichten</span><span class="sxs-lookup"><span data-stu-id="bbba2-114">Benefits of Using Views</span></span>

<span data-ttu-id="bbba2-115">Bieten die Ansichten [Trennung von Anliegen](http://deviq.com/separation-of-concerns/) innerhalb einer MVC-app, die Benutzer Schnittstelle Ebene Markup getrennt von der Geschäftslogik zu kapseln.</span><span class="sxs-lookup"><span data-stu-id="bbba2-115">Views provide [separation of concerns](http://deviq.com/separation-of-concerns/) within an MVC app, encapsulating user interface level markup separately from business logic.</span></span> <span data-ttu-id="bbba2-116">ASP.NET MVC-Ansichten verwenden [Razor-Syntax](razor.md) Wechsel zwischen den HTML-Markup und Server Side Logik Kinderspiel vornehmen.</span><span class="sxs-lookup"><span data-stu-id="bbba2-116">ASP.NET MVC views use [Razor syntax](razor.md) to make switching between HTML markup and server side logic painless.</span></span> <span data-ttu-id="bbba2-117">Allgemeinen, wiederkehrende Aspekte der Benutzeroberfläche der app einfach wiederverwendet werden zwischen Ansichten unter Verwendung von [Layout und die freigegebenen Direktiven](layout.md) oder [Teilansichten](partial.md).</span><span class="sxs-lookup"><span data-stu-id="bbba2-117">Common, repetitive aspects of the app's user interface can easily be reused between views using [layout and shared directives](layout.md) or [partial views](partial.md).</span></span>

## <a name="creating-a-view"></a><span data-ttu-id="bbba2-118">Erstellen einer Ansicht</span><span class="sxs-lookup"><span data-stu-id="bbba2-118">Creating a View</span></span>

<span data-ttu-id="bbba2-119">Sichten, die für einen Controller spezifisch sind werden erstellt, der *Ansichten / [ControllerName]* Ordner.</span><span class="sxs-lookup"><span data-stu-id="bbba2-119">Views that are specific to a controller are created in the *Views/[ControllerName]* folder.</span></span> <span data-ttu-id="bbba2-120">Sichten, die für Domänencontroller, freigegeben werden befinden sich der */Ansichten/freigegeben* Ordner.</span><span class="sxs-lookup"><span data-stu-id="bbba2-120">Views that are shared among controllers are placed in the */Views/Shared* folder.</span></span> <span data-ttu-id="bbba2-121">Benennen Sie die Ansichtsdatei identisch mit der Aktion zugeordneten Controller und Hinzufügen der *cshtml* Dateierweiterung.</span><span class="sxs-lookup"><span data-stu-id="bbba2-121">Name the view file the same as its associated controller action, and add the *.cshtml* file extension.</span></span> <span data-ttu-id="bbba2-122">Z. B. zum Erstellen einer Ansicht für die *zu* Aktion auf die *Home* Controller, erstellen Sie die *About.cshtml* in der Datei die   */Ansichten/Start*Ordner.</span><span class="sxs-lookup"><span data-stu-id="bbba2-122">For example, to create a view for the *About* action on the *Home* controller, you would create the *About.cshtml* file in the */Views/Home* folder.</span></span>

<span data-ttu-id="bbba2-123">Eine Beispieldatei für die Sicht (*About.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="bbba2-123">A sample view file (*About.cshtml*):</span></span>

<span data-ttu-id="bbba2-124">[!code-html[Main](../../common/samples/WebApplication1/Views/Home/About.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="bbba2-124">[!code-html[Main](../../common/samples/WebApplication1/Views/Home/About.cshtml)]</span></span>

<span data-ttu-id="bbba2-125">*Razor* Code ist gekennzeichnet durch die `@` Symbol.</span><span class="sxs-lookup"><span data-stu-id="bbba2-125">*Razor* code is denoted by the `@` symbol.</span></span> <span data-ttu-id="bbba2-126">C#-Anweisungen innerhalb von geschweiften Klammern Codeblöcke abgegrenzt Razor ausgeführt werden (`{` `}`), z. B. die Zuweisung von "About" auf die `ViewData["Title"]` oben angezeigten Element.</span><span class="sxs-lookup"><span data-stu-id="bbba2-126">C# statements are run within Razor code blocks set off by curly braces (`{` `}`), such as the assignment of "About" to the `ViewData["Title"]` element shown above.</span></span> <span data-ttu-id="bbba2-127">Razor kann verwendet werden, um Werte in HTML anzeigen, indem Sie einfach verweisen auf den Wert mit der `@` Sonderzeichen, wie gezeigt in der `<h2>` und `<h3>` oben aufgeführten Elemente.</span><span class="sxs-lookup"><span data-stu-id="bbba2-127">Razor can be used to display values within HTML by simply referencing the value with the `@` symbol, as shown within the `<h2>` and `<h3>` elements above.</span></span>

<span data-ttu-id="bbba2-128">In dieser Ansicht konzentriert sich auf den Teil der Ausgabe für die er zuständig ist.</span><span class="sxs-lookup"><span data-stu-id="bbba2-128">This view focuses on just the portion of the output for which it is responsible.</span></span> <span data-ttu-id="bbba2-129">Der Rest der Seite Layout und andere allgemeine Aspekte der Ansicht, werden an anderer Stelle angegeben.</span><span class="sxs-lookup"><span data-stu-id="bbba2-129">The rest of the page's layout, and other common aspects of the view, are specified elsewhere.</span></span> <span data-ttu-id="bbba2-130">Erfahren Sie mehr über [Layout und die freigegebenen ansichtslogik](layout.md).</span><span class="sxs-lookup"><span data-stu-id="bbba2-130">Learn more about [layout and shared view logic](layout.md).</span></span>

## <a name="how-do-controllers-specify-views"></a><span data-ttu-id="bbba2-131">Wie Controller Ansichten angeben?</span><span class="sxs-lookup"><span data-stu-id="bbba2-131">How do Controllers Specify Views?</span></span>

<span data-ttu-id="bbba2-132">Ansichten werden in der Regel zurückgegeben, von Aktionen als ein `ViewResult`.</span><span class="sxs-lookup"><span data-stu-id="bbba2-132">Views are typically returned from actions as a `ViewResult`.</span></span> <span data-ttu-id="bbba2-133">Ihrer Aktionsmethode erstellen und zurückgeben kann eine `ViewResult` , häufiger aber direkt, wenn Ihr Controller erbt `Controller`, verwenden Sie einfach die `View` Hilfsmethode, wie dieses Beispiel veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="bbba2-133">Your action method can create and return a `ViewResult` directly, but more commonly if your controller inherits from `Controller`, you'll simply use the `View` helper method, as this example demonstrates:</span></span>

<span data-ttu-id="bbba2-134">*HomeController.cs*</span><span class="sxs-lookup"><span data-stu-id="bbba2-134">*HomeController.cs*</span></span>

<span data-ttu-id="bbba2-135">[!code-csharp[Main](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]</span><span class="sxs-lookup"><span data-stu-id="bbba2-135">[!code-csharp[Main](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]</span></span>

<span data-ttu-id="bbba2-136">Die `View` -Hilfsmethode verfügt über mehrere Überladungen auf, um die Rückgabe Ansichten für app-Entwickler zu vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="bbba2-136">The `View` helper method has several overloads to make returning views easier for app developers.</span></span> <span data-ttu-id="bbba2-137">Sie können optional eine Sicht zurückgegeben sowie ein Modellobjekt Übergabe an die Ansicht angeben.</span><span class="sxs-lookup"><span data-stu-id="bbba2-137">You can optionally specify a view to return, as well as a model object to pass to the view.</span></span>

<span data-ttu-id="bbba2-138">Wenn diese Aktion zurückgegeben wird, die *About.cshtml* oben gezeigten Ansicht gerendert wird:</span><span class="sxs-lookup"><span data-stu-id="bbba2-138">When this action returns, the *About.cshtml* view shown above is rendered:</span></span>

![Zu den Seiten](overview/_static/about-page.png)

### <a name="view-discovery"></a><span data-ttu-id="bbba2-140">View-Ermittlung</span><span class="sxs-lookup"><span data-stu-id="bbba2-140">View Discovery</span></span>

<span data-ttu-id="bbba2-141">Ein Prozess wird aufgerufen, wenn eine Aktion eine Sicht zurückgegeben wird, *Ansicht Ermittlung* erfolgt.</span><span class="sxs-lookup"><span data-stu-id="bbba2-141">When an action returns a view, a process called *view discovery* takes place.</span></span> <span data-ttu-id="bbba2-142">Dieser Prozess wird bestimmt, welche Datei anzeigen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="bbba2-142">This process determines which view file will be used.</span></span> <span data-ttu-id="bbba2-143">Wenn eine bestimmte Datei angegeben wird, sucht die Laufzeit für eine Controller-spezifische Ansicht zuerst, dann entsprechende Ansichtsname in sucht nach der *Shared* Ordner.</span><span class="sxs-lookup"><span data-stu-id="bbba2-143">Unless a specific view file is specified, the runtime looks for a controller-specific view first, then looks for matching view name in the *Shared* folder.</span></span>

<span data-ttu-id="bbba2-144">Wenn eine Aktion gibt die `View` -Methode, wie folgt `return View();`, der Aktionsname wird verwendet, wie der Ansichtsname.</span><span class="sxs-lookup"><span data-stu-id="bbba2-144">When an action returns the `View` method, like so `return View();`, the action name is used as the view name.</span></span> <span data-ttu-id="bbba2-145">Angenommen, wenn dies von einer Aktionsmethode namens "Index" aufgerufen wurden, wäre entspricht dem Übergeben einer Ansichtsname "Index" es.</span><span class="sxs-lookup"><span data-stu-id="bbba2-145">For example, if this were called from an action method named "Index", it would be equivalent to passing in a view name of "Index".</span></span> <span data-ttu-id="bbba2-146">Ein Sichtname explizit an die Methode übergeben werden kann (`return View("SomeView");`).</span><span class="sxs-lookup"><span data-stu-id="bbba2-146">A view name can be explicitly passed to the method (`return View("SomeView");`).</span></span> <span data-ttu-id="bbba2-147">Zeigen Sie in beiden Fällen Ermittlung sucht eine übereinstimmende-Datei in ein:</span><span class="sxs-lookup"><span data-stu-id="bbba2-147">In both of these cases, view discovery searches for a matching view file in:</span></span>

   1. <span data-ttu-id="bbba2-148">Ansichten /<ControllerName>/<ViewName>cshtml</span><span class="sxs-lookup"><span data-stu-id="bbba2-148">Views/<ControllerName>/<ViewName>.cshtml</span></span>

   2. <span data-ttu-id="bbba2-149">Ansichten/freigegeben/<ViewName>cshtml</span><span class="sxs-lookup"><span data-stu-id="bbba2-149">Views/Shared/<ViewName>.cshtml</span></span>

>[!TIP]
> <span data-ttu-id="bbba2-150">Es wird empfohlen, gemäß der Konvention von zurückzugeben `View()` aus Aktionen, wenn möglich, da dabei mehr Flexibilität bietet, einfacher Umgestalten von Code.</span><span class="sxs-lookup"><span data-stu-id="bbba2-150">We recommend following the convention of simply returning `View()` from actions when possible, as it results in more flexible, easier to refactor code.</span></span>

<span data-ttu-id="bbba2-151">Anstatt ein Ansichtsname kann ein Dateipfad für die Sicht angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="bbba2-151">A view file path can be provided, instead of a view name.</span></span> <span data-ttu-id="bbba2-152">In diesem Fall die *cshtml* Erweiterung muss als Teil der Dateipfad angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="bbba2-152">In this case, the *.cshtml* extension must be specified as part of the file path.</span></span> <span data-ttu-id="bbba2-153">Der Pfad muss relativ zum Stammverzeichnis Anwendung (und kann optional beginnen mit "/" oder "~ /").</span><span class="sxs-lookup"><span data-stu-id="bbba2-153">The path should be relative to the application root (and can optionally start with "/" or "~/").</span></span> <span data-ttu-id="bbba2-154">Beispiel: `return View("Views/Home/About.cshtml");`</span><span class="sxs-lookup"><span data-stu-id="bbba2-154">For example: `return View("Views/Home/About.cshtml");`</span></span>

> [!NOTE]
> <span data-ttu-id="bbba2-155">[Teilansichten](partial.md) und [anzeigen Komponenten](view-components.md) ähnliche (aber nicht identisch) Ermittlungsmechanismus verwenden.</span><span class="sxs-lookup"><span data-stu-id="bbba2-155">[Partial views](partial.md) and [view components](view-components.md) use similar (but not identical) discovery mechanisms.</span></span>

> [!NOTE]
> <span data-ttu-id="bbba2-156">Sie können anpassen, dass die Standardkonvention bezüglich unter dem Ansichten innerhalb der app gespeichert werden, mithilfe ein benutzerdefinierten `IViewLocationExpander`.</span><span class="sxs-lookup"><span data-stu-id="bbba2-156">You can customize the default convention regarding where views are located within the app by using a custom `IViewLocationExpander`.</span></span>

>[!TIP]
> <span data-ttu-id="bbba2-157">Sichtnamen möglicherweise Groß-/Kleinschreibung beachtet, abhängig von dem zugrunde liegenden Dateisystem.</span><span class="sxs-lookup"><span data-stu-id="bbba2-157">View names may be case sensitive depending on the underlying file system.</span></span> <span data-ttu-id="bbba2-158">Aus Kompatibilitätsgründen Betriebssystemen müssen Sie immer übereinstimmen Sie Fall zwischen Controller und Aktionsnamen und zugeordnete Ansicht-Ordner und Dateinamen.</span><span class="sxs-lookup"><span data-stu-id="bbba2-158">For compatibility across operating systems, always match case between controller and action names and associated view folders and filenames.</span></span>

## <a name="passing-data-to-views"></a><span data-ttu-id="bbba2-159">Übergeben von Daten mit Ansichten</span><span class="sxs-lookup"><span data-stu-id="bbba2-159">Passing Data to Views</span></span>

<span data-ttu-id="bbba2-160">Sie können Daten mit Ansichten mithilfe mehrerer Mechanismen übergeben.</span><span class="sxs-lookup"><span data-stu-id="bbba2-160">You can pass data to views using several mechanisms.</span></span> <span data-ttu-id="bbba2-161">Die zuverlässigste Methode ist die Angabe ein *Modell* Typ in der Ansicht (gemeinhin als eine *Viewmodel*, um die Unterscheidung von Business-Domäne Modelltypen), und übergeben Sie eine Instanz dieses Typs der Ansicht von der Aktion.</span><span class="sxs-lookup"><span data-stu-id="bbba2-161">The most robust approach is to specify a *model* type in the view (commonly referred to as a *viewmodel*, to distinguish it from business domain model types), and then pass an instance of this type to the view from the action.</span></span> <span data-ttu-id="bbba2-162">Es wird empfohlen, dass Sie ein Modell oder Sicht verwenden, um Daten an eine Ansicht zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="bbba2-162">We recommend you use a model or view model to pass data to a view.</span></span> <span data-ttu-id="bbba2-163">Dadurch wird die Ansicht zu starken typüberprüfung nutzen.</span><span class="sxs-lookup"><span data-stu-id="bbba2-163">This allows the view to take advantage of strong type checking.</span></span> <span data-ttu-id="bbba2-164">Sie können angeben, dass ein Modell für eine Sicht mit der `@model` Richtlinie:</span><span class="sxs-lookup"><span data-stu-id="bbba2-164">You can specify a model for a view using the `@model` directive:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [1]}} -->

```html
@model WebApplication1.ViewModels.Address
   <h2>Contact</h2>
   <address>
       @Model.Street<br />
       @Model.City, @Model.State @Model.PostalCode<br />
       <abbr title="Phone">P:</abbr>
       425.555.0100
   </address>
   ```

<span data-ttu-id="bbba2-165">Nachdem ein Modell für eine Sicht angegeben wurde, kann die Instanz gesendet, um die Sicht zugegriffen werden, in einer stark typisierten mit `@Model` wie oben gezeigt.</span><span class="sxs-lookup"><span data-stu-id="bbba2-165">Once a model has been specified for a view, the instance sent to the view can be accessed in a strongly-typed manner using `@Model` as shown above.</span></span> <span data-ttu-id="bbba2-166">Um eine Instanz des Modelltyps auf die Ansicht zu gewährleisten, werden der Controller es als Parameter übergeben:</span><span class="sxs-lookup"><span data-stu-id="bbba2-166">To provide an instance of the model type to the view, the controller passes it as a parameter:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [13]}} -->

```csharp
public IActionResult Contact()
   {
       ViewData["Message"] = "Your contact page.";

       var viewModel = new Address()
       {
           Name = "Microsoft",
           Street = "One Microsoft Way",
           City = "Redmond",
           State = "WA",
           PostalCode = "98052-6399"
       };
       return View(viewModel);
   }
   ```

<span data-ttu-id="bbba2-167">Es gibt keinerlei Beschränkungen, die Typen, die für eine Sicht als ein Modell bereitgestellt werden können.</span><span class="sxs-lookup"><span data-stu-id="bbba2-167">There are no restrictions on the types that can be provided to a view as a model.</span></span> <span data-ttu-id="bbba2-168">Es wird empfohlen Ansichtsmodelle mit wenigen oder gar keinen Verhalten einfaches altes CLR Object (POCO) übergeben, damit die Geschäftslogik an anderer Stelle in der app gekapselt werden.</span><span class="sxs-lookup"><span data-stu-id="bbba2-168">We recommend passing Plain Old CLR Object (POCO) view models with little or no behavior, so that business logic can be encapsulated elsewhere in the app.</span></span> <span data-ttu-id="bbba2-169">Ein Beispiel dieses Ansatzes ist die *Adresse* Viewmodel im obigen Beispiel verwendet:</span><span class="sxs-lookup"><span data-stu-id="bbba2-169">An example of this approach is the *Address* viewmodel used in the example above:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [13]}} -->

```csharp
namespace WebApplication1.ViewModels
   {
       public class Address
       {
           public string Name { get; set; }
           public string Street { get; set; }
           public string City { get; set; }
           public string State { get; set; }
           public string PostalCode { get; set; }
       }
   }
   ```

> [!NOTE]
> <span data-ttu-id="bbba2-170">Nichts daran hindert Sie die gleichen Klassen als Ihr Modell Geschäftstypen und Ihre Anzeige Modelltypen verwenden.</span><span class="sxs-lookup"><span data-stu-id="bbba2-170">Nothing prevents you from using the same classes as your business model types and your display model types.</span></span> <span data-ttu-id="bbba2-171">Allerdings werden und voneinander getrennt können Ihre Ansichten unabhängig von Ihrer Domäne oder der Persistenz-Modell abweichen, bietet einige Sicherheitsvorteile bei als auch (für Modelle, die Benutzer an der Anwendung mit sendet [modellbindung](../models/model-binding.md)).</span><span class="sxs-lookup"><span data-stu-id="bbba2-171">However, keeping them separate allows your views to vary independently from your domain or persistence model, and can offer some security benefits as well (for models that users will send to the app using [model binding](../models/model-binding.md)).</span></span>

### <a name="loosely-typed-data"></a><span data-ttu-id="bbba2-172">Lose typisierte Daten</span><span class="sxs-lookup"><span data-stu-id="bbba2-172">Loosely Typed Data</span></span>

<span data-ttu-id="bbba2-173">Zusätzlich zu den stark typisierten Ansichten haben alle Ansichten Zugriff auf eine schwach typisierte Auflistung von Daten.</span><span class="sxs-lookup"><span data-stu-id="bbba2-173">In addition to strongly typed views, all views have access to a loosely typed collection of data.</span></span> <span data-ttu-id="bbba2-174">Diese derselben Sammlung verwiesen werden kann, entweder über die `ViewData` oder `ViewBag` Eigenschaften für Controller und Ansichten.</span><span class="sxs-lookup"><span data-stu-id="bbba2-174">This same collection can be referenced through either the `ViewData` or `ViewBag` properties on controllers and views.</span></span> <span data-ttu-id="bbba2-175">Die `ViewBag` Eigenschaft ist ein Wrapper um `ViewData` enthält, die eine dynamische Ansicht über diese Sammlung.</span><span class="sxs-lookup"><span data-stu-id="bbba2-175">The `ViewBag` property is a wrapper around `ViewData` that provides a dynamic view over that collection.</span></span> <span data-ttu-id="bbba2-176">Es ist keine separate Auflistung.</span><span class="sxs-lookup"><span data-stu-id="bbba2-176">It is not a separate collection.</span></span>

<span data-ttu-id="bbba2-177">`ViewData`erfolgt ein Dictionary-Objekt über `string` Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="bbba2-177">`ViewData` is a dictionary object accessed through `string` keys.</span></span> <span data-ttu-id="bbba2-178">Können Sie speichern und Abrufen von Objekten, und Sie müssen diese für einen bestimmten Typ umwandeln, wenn Sie zu extrahieren.</span><span class="sxs-lookup"><span data-stu-id="bbba2-178">You can store and retrieve objects in it, and you'll need to cast them to a specific type when you extract them.</span></span> <span data-ttu-id="bbba2-179">Sie können `ViewData` Daten von einem Controller mit Ansichten, sowie Ansichten (, Teilansichten und Layouts) übergeben.</span><span class="sxs-lookup"><span data-stu-id="bbba2-179">You can use `ViewData` to pass data from a controller to views, as well as within views (and partial views and layouts).</span></span> <span data-ttu-id="bbba2-180">Zeichenfolgendaten können gespeichert und direkt, ohne die Notwendigkeit einer Typumwandlung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="bbba2-180">String data can be stored and used directly, without the need for a cast.</span></span>

<span data-ttu-id="bbba2-181">Legen Sie einige Werte für `ViewData` in einer Aktion:</span><span class="sxs-lookup"><span data-stu-id="bbba2-181">Set some values for `ViewData` in an action:</span></span>

```csharp
public IActionResult SomeAction()
   {
       ViewData["Greeting"] = "Hello";
       ViewData["Address"]  = new Address()
       {
           Name = "Steve",
           Street = "123 Main St",
           City = "Hudson",
           State = "OH",
           PostalCode = "44236"
       };

       return View();
   }
   ```

<span data-ttu-id="bbba2-182">Arbeiten Sie mit den Daten in einer Ansicht:</span><span class="sxs-lookup"><span data-stu-id="bbba2-182">Work with the data in a view:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [3, 6]}} -->

```html
@{
       // Requires cast
       var address = ViewData["Address"] as Address;
   }

   @ViewData["Greeting"] World!

   <address>
       @address.Name<br />
       @address.Street<br />
       @address.City, @address.State @address.PostalCode
   </address>
   ```

<span data-ttu-id="bbba2-183">Die `ViewBag` Objekte bietet dynamische Zugriff auf die Objekte, die in gespeicherten `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="bbba2-183">The `ViewBag` objects provides dynamic access to the objects stored in `ViewData`.</span></span> <span data-ttu-id="bbba2-184">Dies kann einfacher zu handhaben, da es keine Typumwandlung erforderlich sein.</span><span class="sxs-lookup"><span data-stu-id="bbba2-184">This can be more convenient to work with, since it doesn't require casting.</span></span> <span data-ttu-id="bbba2-185">Das gleiche Beispiel wie oben mit `ViewBag` anstelle eines stark typisierten `address` Instanz in der Sicht:</span><span class="sxs-lookup"><span data-stu-id="bbba2-185">The same example as above, using `ViewBag` instead of a strongly typed `address` instance in the view:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [1, 4, 5, 6]}} -->

```html
@ViewBag.Greeting World!

   <address>
       @ViewBag.Address.Name<br />
       @ViewBag.Address.Street<br />
       @ViewBag.Address.City, @ViewBag.Address.State @ViewBag.Address.PostalCode
   </address>
   ```

> [!NOTE]
> <span data-ttu-id="bbba2-186">Da beide auf das gleiche zugrunde liegende verweisen `ViewData` Auflistung Sie mischen und Zuordnen zwischen `ViewData` und `ViewBag` beim Lesen und Schreiben der Werte, wenn einfache.</span><span class="sxs-lookup"><span data-stu-id="bbba2-186">Since both refer to the same underlying `ViewData` collection, you can mix and match between `ViewData` and `ViewBag` when reading and writing values, if convenient.</span></span>

### <a name="dynamic-views"></a><span data-ttu-id="bbba2-187">Dynamische Ansichten</span><span class="sxs-lookup"><span data-stu-id="bbba2-187">Dynamic Views</span></span>

<span data-ttu-id="bbba2-188">Sichten, die einen Modelltyp nicht deklarieren, aber eine Modellinstanz, die an sie übergeben haben, können dieser Instanz dynamisch verweisen.</span><span class="sxs-lookup"><span data-stu-id="bbba2-188">Views that do not declare a model type but have a model instance passed to them can reference this instance dynamically.</span></span> <span data-ttu-id="bbba2-189">Angenommen, wenn eine Instanz von `Address` übergeben wird, um eine Sicht, die nicht deklariert eine `@model`, die Sicht wären immer noch in der Lage, dynamisch wie gezeigt auf die Eigenschaften der Instanz zu verweisen:</span><span class="sxs-lookup"><span data-stu-id="bbba2-189">For example, if an instance of `Address` is passed to a view that doesn't declare an `@model`, the view would still be able to refer to the instance's properties dynamically as shown:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [13, 16, 17, 18]}} -->

```html
<address>
       @Model.Street<br />
       @Model.City, @Model.State @Model.PostalCode<br />
       <abbr title="Phone">P:</abbr>
       425.555.0100
   </address>
   ```

<span data-ttu-id="bbba2-190">Diese Funktion kann einige Flexibilität bieten, aber keinen Schutz Kompilierung kein(e) IntelliSense bietet.</span><span class="sxs-lookup"><span data-stu-id="bbba2-190">This feature can offer some flexibility, but does not offer any compilation protection or IntelliSense.</span></span> <span data-ttu-id="bbba2-191">Wenn die Eigenschaft nicht vorhanden ist, wird die Seite zur Laufzeit fehlschlagen.</span><span class="sxs-lookup"><span data-stu-id="bbba2-191">If the property doesn't exist, the page will fail at runtime.</span></span>

## <a name="more-view-features"></a><span data-ttu-id="bbba2-192">Weitere Funktionen</span><span class="sxs-lookup"><span data-stu-id="bbba2-192">More View Features</span></span>

<span data-ttu-id="bbba2-193">[Kennzeichnen von Hilfsprogrammen](tag-helpers/intro.md) erleichtern das Hinzufügen von serverseitigen Verhalten zu vorhandenen HTML-Tags, vermeiden von benutzerdefiniertem Code oder Hilfsprogramme in Ansichten verwendet werden müssen.</span><span class="sxs-lookup"><span data-stu-id="bbba2-193">[Tag helpers](tag-helpers/intro.md) make it easy to add server-side behavior to existing HTML tags, avoiding the need to use custom code or helpers within views.</span></span> <span data-ttu-id="bbba2-194">Tag-Hilfsprogrammen gelten als Attribute HTML-Elementen, die ignoriert werden, von Editoren, die nicht vertraut, sodass ansichtsmarkup bearbeitet und in einer Vielzahl von Tools gerendert werden.</span><span class="sxs-lookup"><span data-stu-id="bbba2-194">Tag helpers are applied as attributes to HTML elements, which are ignored by editors that aren't familiar with them, allowing view markup to be edited and rendered in a variety of tools.</span></span> <span data-ttu-id="bbba2-195">Tag-Hilfsmethoden sind vielseitig verwendbar, und insbesondere kann [arbeiten mit Formularen](working-with-forms.md) sehr viel einfacher.</span><span class="sxs-lookup"><span data-stu-id="bbba2-195">Tag helpers have many uses, and in particular can make [working with forms](working-with-forms.md) much easier.</span></span>

<span data-ttu-id="bbba2-196">Generieren von benutzerdefinierten HTML-Markup kann mit vielen integrierten HTML-Hilfsmethoden erreicht werden, und eine komplexer Benutzeroberflächen-Logik (u. u. mit einem eigenen datenanforderungen) kann gekapselt werden [Ansichtskomponenten](view-components.md).</span><span class="sxs-lookup"><span data-stu-id="bbba2-196">Generating custom HTML markup can be achieved with many built-in HTML Helpers, and more complex UI logic (potentially with its own data requirements) can be encapsulated in [View Components](view-components.md).</span></span> <span data-ttu-id="bbba2-197">Anzeigen von Komponenten bieten die gleiche Trennung von Anliegen, die den Controller und Ansichten bieten und können entfällt der Bedarf für Aktionen und Ansichten für den Umgang mit Daten, die durch allgemeine Elemente der Benutzeroberfläche verwendet.</span><span class="sxs-lookup"><span data-stu-id="bbba2-197">View components provide the same separation of concerns that controllers and views offer, and can eliminate the need for actions and views to deal with data used by common UI elements.</span></span>

<span data-ttu-id="bbba2-198">Wie viele andere Aspekte von ASP.NET Core Ansichten unterstützen [Abhängigkeitsinjektion](../../fundamentals/dependency-injection.md), sodass Dienste sind [in Sichten eingefügt](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="bbba2-198">Like many other aspects of ASP.NET Core, views support [dependency injection](../../fundamentals/dependency-injection.md), allowing services to be [injected into views](dependency-injection.md).</span></span>
