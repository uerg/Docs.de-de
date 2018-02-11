---
title: Bereiche
author: rick-anderson
description: Veranschaulicht das Arbeiten mit Bereichen.
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/areas
ms.openlocfilehash: 1ade49de3f6c58edc4ea7b06bc593b3db797081c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="areas"></a><span data-ttu-id="f88d9-103">Bereiche</span><span class="sxs-lookup"><span data-stu-id="f88d9-103">Areas</span></span>

<span data-ttu-id="f88d9-104">Von [Dhananjay Kumar](https://twitter.com/debug_mode) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f88d9-104">By [Dhananjay Kumar](https://twitter.com/debug_mode)  and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f88d9-105">Bereiche sind ein Feature von ASP.NET MVC, das für die Organisation von verwandten Funktionalitäten in eine Gruppe als separater Namespace (für Routing) und Ordnerstruktur (für Ansichten) verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="f88d9-105">Areas are an ASP.NET MVC feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="f88d9-106">Mithilfe von Bereichen wird eine Hierarchie erstellt, damit das Routing durch Hinzufügen eines anderen Routenparameters ausgeführt werden kann: `area` zu `controller` und `action`.</span><span class="sxs-lookup"><span data-stu-id="f88d9-106">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action`.</span></span>

<span data-ttu-id="f88d9-107">Bereiche sind eine Möglichkeit, eine große ASP.NET Core MVC-Web-App in kleinere funktionale Gruppierungen aufzuteilen.</span><span class="sxs-lookup"><span data-stu-id="f88d9-107">Areas provide a way to partition a large ASP.NET Core MVC Web app into smaller functional groupings.</span></span> <span data-ttu-id="f88d9-108">Ein Bereich ist im Grunde genommen eine MVC-Struktur in einer Anwendung.</span><span class="sxs-lookup"><span data-stu-id="f88d9-108">An area is effectively an MVC structure inside an application.</span></span> <span data-ttu-id="f88d9-109">In einem MVC-Projekt sind logische Komponenten wie Modell, Controller und Ansicht in verschiedenen Ordnern gespeichert. MVC nutzt Namenskonventionen zum Erstellen einer Beziehung zwischen diesen Komponenten.</span><span class="sxs-lookup"><span data-stu-id="f88d9-109">In an MVC project, logical components like Model, Controller, and View are kept in different folders, and MVC uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="f88d9-110">Bei einer großen App kann es von Vorteil sein, die App in mehrere Bereiche auf hoher Funktionalitätsebene zu unterteilen,</span><span class="sxs-lookup"><span data-stu-id="f88d9-110">For a large app, it may be advantageous to partition the  app into separate high level areas of functionality.</span></span> <span data-ttu-id="f88d9-111">Dies gilt z.B. für eine E-Commerce-App mit mehreren Geschäftseinheiten, wie Auftragsabschluss, Abrechnung und Suche usw. Jede dieser Einheiten hat ihre eigenen logischen Komponentenansichten, Controller und Modelle.</span><span class="sxs-lookup"><span data-stu-id="f88d9-111">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search etc. Each of these units have their own logical component views, controllers, and models.</span></span> <span data-ttu-id="f88d9-112">In diesem Szenario können Sie Bereiche verwenden, um die Geschäftskomponenten im selben Projekt physisch zu partitionieren.</span><span class="sxs-lookup"><span data-stu-id="f88d9-112">In this scenario, you can use Areas to physically partition the business components in the same project.</span></span>

<span data-ttu-id="f88d9-113">Ein Bereich kann als kleinere funktionale Einheiten mit eigenen Controllern, Ansichten und Modellen in einem ASP.NET Core MVC-Projekt definiert werden.</span><span class="sxs-lookup"><span data-stu-id="f88d9-113">An area can be defined as smaller functional units in an ASP.NET Core MVC project with its own set of controllers, views, and models.</span></span>

<span data-ttu-id="f88d9-114">Die Verwendung von Bereichen in einem MVC-Projekt ist erwägenswert, wenn:</span><span class="sxs-lookup"><span data-stu-id="f88d9-114">Consider using Areas in an MVC project when:</span></span>

* <span data-ttu-id="f88d9-115">Ihre Anwendung aus mehreren funktionalen Komponenten auf hoher Ebene besteht, die logisch getrennt sein sollten</span><span class="sxs-lookup"><span data-stu-id="f88d9-115">Your application is made of multiple high-level functional components that should be logically separated</span></span>

* <span data-ttu-id="f88d9-116">Sie Ihr MVC-Projekt partitionieren möchten, damit jeder funktionale Bereich unabhängig voneinander bearbeitet werden kann</span><span class="sxs-lookup"><span data-stu-id="f88d9-116">You want to partition your MVC project so that each functional area can be worked on independently</span></span>

<span data-ttu-id="f88d9-117">Features von Bereichen:</span><span class="sxs-lookup"><span data-stu-id="f88d9-117">Area features:</span></span>

* <span data-ttu-id="f88d9-118">Eine ASP.NET Core MVC-App kann über eine beliebige Anzahl von Bereichen verfügen</span><span class="sxs-lookup"><span data-stu-id="f88d9-118">An ASP.NET Core MVC app can have any number of areas</span></span>

* <span data-ttu-id="f88d9-119">Jeder Bereich verfügt über seine eigenen Controller, Modelle und Ansichten</span><span class="sxs-lookup"><span data-stu-id="f88d9-119">Each area has its own controllers, models, and views</span></span>

* <span data-ttu-id="f88d9-120">Ermöglicht Ihnen das Organisieren großer MVC-Projekte in mehrere allgemeine Komponenten, die unabhängig voneinander bearbeitet werden können</span><span class="sxs-lookup"><span data-stu-id="f88d9-120">Allows you to organize large MVC projects into multiple high-level components that can be worked on independently</span></span>

* <span data-ttu-id="f88d9-121">Unterstützt mehrere Controller mit demselben Namen, solange diese über verschiedene *Bereiche* verfügen</span><span class="sxs-lookup"><span data-stu-id="f88d9-121">Supports multiple controllers with the same name - as long as they have different *areas*</span></span>

<span data-ttu-id="f88d9-122">Im Folgenden finden Sie ein Beispiel, das darstellt, wie Bereiche erstellt und verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="f88d9-122">Let's take a look at an example to illustrate how Areas are created and used.</span></span> <span data-ttu-id="f88d9-123">Angenommen, Sie besitzen eine Store-App mit zwei unterschiedlichen Gruppierungen von Controllern und Ansichten: Produkte und Dienste.</span><span class="sxs-lookup"><span data-stu-id="f88d9-123">Let's say you have a store app that has two distinct groupings of controllers and views: Products and Services.</span></span> <span data-ttu-id="f88d9-124">Eine übliche Ordnerstruktur hierfür, die MVC-Bereiche verwendet, sieht folgendermaßen aus:</span><span class="sxs-lookup"><span data-stu-id="f88d9-124">A typical folder structure for that using MVC areas looks like below:</span></span>

* <span data-ttu-id="f88d9-125">Projektname</span><span class="sxs-lookup"><span data-stu-id="f88d9-125">Project name</span></span>

  * <span data-ttu-id="f88d9-126">Bereiche</span><span class="sxs-lookup"><span data-stu-id="f88d9-126">Areas</span></span>

    * <span data-ttu-id="f88d9-127">Produkte</span><span class="sxs-lookup"><span data-stu-id="f88d9-127">Products</span></span>

      * <span data-ttu-id="f88d9-128">Controller</span><span class="sxs-lookup"><span data-stu-id="f88d9-128">Controllers</span></span>

        * <span data-ttu-id="f88d9-129">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="f88d9-129">HomeController.cs</span></span>

        * <span data-ttu-id="f88d9-130">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="f88d9-130">ManageController.cs</span></span>

      * <span data-ttu-id="f88d9-131">Ansichten</span><span class="sxs-lookup"><span data-stu-id="f88d9-131">Views</span></span>

        * <span data-ttu-id="f88d9-132">Startseite</span><span class="sxs-lookup"><span data-stu-id="f88d9-132">Home</span></span>

          * <span data-ttu-id="f88d9-133">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="f88d9-133">Index.cshtml</span></span>

        * <span data-ttu-id="f88d9-134">Verwalten</span><span class="sxs-lookup"><span data-stu-id="f88d9-134">Manage</span></span>

          * <span data-ttu-id="f88d9-135">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="f88d9-135">Index.cshtml</span></span>

    * <span data-ttu-id="f88d9-136">Dienste</span><span class="sxs-lookup"><span data-stu-id="f88d9-136">Services</span></span>

      * <span data-ttu-id="f88d9-137">Controller</span><span class="sxs-lookup"><span data-stu-id="f88d9-137">Controllers</span></span>

        * <span data-ttu-id="f88d9-138">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="f88d9-138">HomeController.cs</span></span>

      * <span data-ttu-id="f88d9-139">Ansichten</span><span class="sxs-lookup"><span data-stu-id="f88d9-139">Views</span></span>

        * <span data-ttu-id="f88d9-140">Startseite</span><span class="sxs-lookup"><span data-stu-id="f88d9-140">Home</span></span>

          * <span data-ttu-id="f88d9-141">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="f88d9-141">Index.cshtml</span></span>

<span data-ttu-id="f88d9-142">Wenn MVC versucht, eine Ansicht in einem Bereich zu rendern, wird standardgemäß in den folgenden Speicherorten gesucht:</span><span class="sxs-lookup"><span data-stu-id="f88d9-142">When MVC tries to render a view in an Area, by default, it tries to look in the following locations:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
   /Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
   /Views/Shared/<Action-Name>.cshtml
   ```

<span data-ttu-id="f88d9-143">Dies sind die Standardspeicherorte, die mithilfe von `AreaViewLocationFormats` unter `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions` geändert werden können.</span><span class="sxs-lookup"><span data-stu-id="f88d9-143">These are the default locations which can be changed via the `AreaViewLocationFormats` on the `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.</span></span>

<span data-ttu-id="f88d9-144">Im unten gezeigten Code wurde der Ordner z.B. von „Areas“ in „Categories“ umbenannt.</span><span class="sxs-lookup"><span data-stu-id="f88d9-144">For example, in the below code instead of having the folder name as 'Areas', it has been changed to 'Categories'.</span></span>

```csharp
services.Configure<RazorViewEngineOptions>(options =>
   {
       options.AreaViewLocationFormats.Clear();
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/{1}/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/Shared/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Views/Shared/{0}.cshtml");
   });
   ```

<span data-ttu-id="f88d9-145">Beachten Sie, dass nur die Struktur des Ordners *Ansichten* in diesem Fall wichtig ist, und dass die Inhalte der restlichen Ordner, wie *Controller* und *Modelle*, **nicht** wichtig sind.</span><span class="sxs-lookup"><span data-stu-id="f88d9-145">One thing to note is that the structure of the *Views* folder is the only one which is considered important here and the content of the rest of the folders like *Controllers* and *Models* does **not** matter.</span></span> <span data-ttu-id="f88d9-146">Sie benötigen z.B. gar keine Ordner für *Controller* und *Modelle*.</span><span class="sxs-lookup"><span data-stu-id="f88d9-146">For example, you need not have a *Controllers* and *Models* folder at all.</span></span> <span data-ttu-id="f88d9-147">Dies funktioniert, weil der Inhalt von *Controller* und *Modelle* nur aus Code besteht, der in eine DLL-Datei kompiliert wird, der Inhalt von *Ansichten* jedoch erst kompiliert wird, wenn die Ansicht angefordert wurde.</span><span class="sxs-lookup"><span data-stu-id="f88d9-147">This works because the content of *Controllers* and *Models* is just code which gets compiled into a .dll where as the content of the *Views* isn't until a request to that view has been made.</span></span>

<span data-ttu-id="f88d9-148">Sobald Sie eine Ordnerhierarchie definiert haben, müssen Sie MVC mitteilen, dass jeder Controller einem Bereich zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="f88d9-148">Once you've defined the folder hierarchy, you need to tell MVC that each controller is associated with an area.</span></span> <span data-ttu-id="f88d9-149">Dazu versehen Sie den Controllernamen mit dem Attribut `[Area]`.</span><span class="sxs-lookup"><span data-stu-id="f88d9-149">You do that by decorating the controller name with the `[Area]` attribute.</span></span>

```csharp
...
   namespace MyStore.Areas.Products.Controllers
   {
       [Area("Products")]
       public class HomeController : Controller
       {
           // GET: /Products/Home/Index
           public IActionResult Index()
           {
               return View();
           }

           // GET: /Products/Home/Create
           public IActionResult Create()
           {
               return View();
           }
       }
   }
   ```

<span data-ttu-id="f88d9-150">Richten Sie eine Routendefinition ein, die mit Ihren neu erstellten Bereichen funktioniert.</span><span class="sxs-lookup"><span data-stu-id="f88d9-150">Set up a route definition that works with your newly created areas.</span></span> <span data-ttu-id="f88d9-151">Der Artikel [Routing to Controller Actions (Routing zu Controlleraktionen)](routing.md) enthält weitere Details dazu, wie Sie Routendefinitionen erstellen, inklusive der Verwendung konventioneller Routen im Vergleich zu Attributrouten.</span><span class="sxs-lookup"><span data-stu-id="f88d9-151">The [Routing to Controller Actions](routing.md) article goes into detail about how to create route definitions, including using conventional routes versus attribute routes.</span></span> <span data-ttu-id="f88d9-152">In diesem Beispiel wird eine konventionelle Route verwendet.</span><span class="sxs-lookup"><span data-stu-id="f88d9-152">In this example, we'll use a conventional route.</span></span> <span data-ttu-id="f88d9-153">Öffnen Sie hierzu die Datei *Startup.cs* und ändern Sie sie, indem Sie die unten genannte Routendefinition `areaRoute` hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="f88d9-153">To do so, open the *Startup.cs* file and modify it by adding the `areaRoute` named route definition below.</span></span>

```csharp
...
   app.UseMvc(routes =>
   {
     routes.MapRoute(
         name: "areaRoute",
         template: "{area:exists}/{controller=Home}/{action=Index}/{id?}");

     routes.MapRoute(
         name: "default",
         template: "{controller=Home}/{action=Index}/{id?}");
   });
   ```

<span data-ttu-id="f88d9-154">Beim Navigieren zu `http://<yourApp>/products` wird die Aktionsmethode `Index` von der `HomeController`-Klasse im Bereich`Products` aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="f88d9-154">Browsing to `http://<yourApp>/products`, the `Index` action method of the `HomeController` in the `Products` area will be invoked.</span></span>

## <a name="link-generation"></a><span data-ttu-id="f88d9-155">Erstellen von Links</span><span class="sxs-lookup"><span data-stu-id="f88d9-155">Link Generation</span></span>

* <span data-ttu-id="f88d9-156">Erstellen von Links von einer Aktion in einem bereichsbasierten Controller zu einer anderen Aktion im selben Controller</span><span class="sxs-lookup"><span data-stu-id="f88d9-156">Generating links from an action within an area based controller to another action within the same controller.</span></span>

  <span data-ttu-id="f88d9-157">Angenommen, der Pfad der aktuellen Anforderung ähnelt `/Products/Home/Create`.</span><span class="sxs-lookup"><span data-stu-id="f88d9-157">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="f88d9-158">HtmlHelper-Syntax: `@Html.ActionLink("Go to Product's Home Page", "Index")`</span><span class="sxs-lookup"><span data-stu-id="f88d9-158">HtmlHelper syntax: `@Html.ActionLink("Go to Product's Home Page", "Index")`</span></span>

  <span data-ttu-id="f88d9-159">TagHelper-Syntax: `<a asp-action="Index">Go to Product's Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="f88d9-159">TagHelper syntax: `<a asp-action="Index">Go to Product's Home Page</a>`</span></span>

  <span data-ttu-id="f88d9-160">Beachten Sie, dass die Werte für „area“ und „controller“ hier nicht angegeben sind, weil sie bereits über den Kontext der aktuellen Anforderung verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="f88d9-160">Note that we need not supply the 'area' and 'controller' values here as they're already available in the context of the current request.</span></span> <span data-ttu-id="f88d9-161">Diese Werte werden als `ambient`-Werte bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="f88d9-161">These kind of values are called `ambient` values.</span></span>

* <span data-ttu-id="f88d9-162">Erstellen von Links von einer Aktion in einem bereichsbasierten Controller zu einer anderen Aktion auf einem anderen Controller</span><span class="sxs-lookup"><span data-stu-id="f88d9-162">Generating links from an action within an area based controller to another action on a different controller</span></span>

  <span data-ttu-id="f88d9-163">Angenommen, der Pfad der aktuellen Anforderung ähnelt `/Products/Home/Create`.</span><span class="sxs-lookup"><span data-stu-id="f88d9-163">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="f88d9-164">HtmlHelper-Syntax: `@Html.ActionLink("Go to Manage Products Home Page", "Index", "Manage")`</span><span class="sxs-lookup"><span data-stu-id="f88d9-164">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products Home Page", "Index", "Manage")`</span></span>

  <span data-ttu-id="f88d9-165">TagHelper-Syntax: `<a asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="f88d9-165">TagHelper syntax: `<a asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span></span>

  <span data-ttu-id="f88d9-166">Beachten Sie, dass der ambient-Wert für „area“ verwendet wird, der Wert für „controller“ wird jedoch explizit oben angegeben.</span><span class="sxs-lookup"><span data-stu-id="f88d9-166">Note that here the ambient value of an 'area' is used but the 'controller' value is specified explicitly above.</span></span>

* <span data-ttu-id="f88d9-167">Erstellen von Links von einer Aktion in einem bereichsbasierten Controller zu einer anderen Aktion auf einem anderen Controller und einem anderen Bereich</span><span class="sxs-lookup"><span data-stu-id="f88d9-167">Generating links from an action within an area based controller to another action on a different controller and a different area.</span></span>

  <span data-ttu-id="f88d9-168">Angenommen, der Pfad der aktuellen Anforderung ähnelt `/Products/Home/Create`.</span><span class="sxs-lookup"><span data-stu-id="f88d9-168">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="f88d9-169">HtmlHelper-Syntax: `@Html.ActionLink("Go to Services Home Page", "Index", "Home", new { area = "Services" })`</span><span class="sxs-lookup"><span data-stu-id="f88d9-169">HtmlHelper syntax: `@Html.ActionLink("Go to Services Home Page", "Index", "Home", new { area = "Services" })`</span></span>

  <span data-ttu-id="f88d9-170">TagHelper-Syntax: `<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="f88d9-170">TagHelper syntax: `<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services Home Page</a>`</span></span>

  <span data-ttu-id="f88d9-171">Beachten Sie, dass hier keine ambient-Werte verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="f88d9-171">Note that here no ambient values are used.</span></span>

* <span data-ttu-id="f88d9-172">Erstellen von Links von einer Aktion in einem bereichsbasierten Controller zu einer anderen Aktion auf einem anderen Controller und **nicht** in einem Bereich.</span><span class="sxs-lookup"><span data-stu-id="f88d9-172">Generating links from an action within an area based controller to another action on a different controller and **not** in an area.</span></span>

  <span data-ttu-id="f88d9-173">HtmlHelper-Syntax: `@Html.ActionLink("Go to Manage Products  Home Page", "Index", "Home", new { area = "" })`</span><span class="sxs-lookup"><span data-stu-id="f88d9-173">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products  Home Page", "Index", "Home", new { area = "" })`</span></span>

  <span data-ttu-id="f88d9-174">TagHelper-Syntax: `<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="f88d9-174">TagHelper syntax: `<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span></span>

  <span data-ttu-id="f88d9-175">Da Links zu einer nicht bereichsbasierten Controlleraktion erstellt werden sollen, wird der ambient-Wert für „area“ hier entfernt.</span><span class="sxs-lookup"><span data-stu-id="f88d9-175">Since we want to generate links to a non-area based controller action, we empty the ambient value for 'area' here.</span></span>

## <a name="publishing-areas"></a><span data-ttu-id="f88d9-176">Veröffentlichen von Bereichen</span><span class="sxs-lookup"><span data-stu-id="f88d9-176">Publishing Areas</span></span>

<span data-ttu-id="f88d9-177">Alle `*.cshtml`- und `wwwroot/**`-Dateien werden für die Ausgabe veröffentlicht, wenn `<Project Sdk="Microsoft.NET.Sdk.Web">` in der *CSPROJ*-Datei enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="f88d9-177">All `*.cshtml` and `wwwroot/**` files are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the *.csproj* file.</span></span>
