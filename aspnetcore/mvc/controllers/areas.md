---
title: Bereiche
author: rick-anderson
description: Zeigt, wie mit Bereichen arbeiten.
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/areas
ms.openlocfilehash: 87bf2eaad1c13d21412051be769992411f685e2e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
# <a name="areas"></a><span data-ttu-id="6d6df-103">Bereiche</span><span class="sxs-lookup"><span data-stu-id="6d6df-103">Areas</span></span>

<span data-ttu-id="6d6df-104">Durch [Dhananjay Kumar](https://twitter.com/debug_mode) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6d6df-104">By [Dhananjay Kumar](https://twitter.com/debug_mode)  and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="6d6df-105">Bereiche sind eine ASP.NET MVC-Funktion verwendet, um verwandte Funktionen als separaten Namespace (für das routing) und die Struktur des Veröffentlichungsordners (für Ansichten) in einer Gruppe zu organisieren.</span><span class="sxs-lookup"><span data-stu-id="6d6df-105">Areas are an ASP.NET MVC feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="6d6df-106">Mithilfe von Bereichen erstellt eine Hierarchie für die Weiterleitung und Hinzufügen von einem anderen Routenparameter, `area`in `controller` und `action`.</span><span class="sxs-lookup"><span data-stu-id="6d6df-106">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action`.</span></span>

<span data-ttu-id="6d6df-107">Bereiche bieten eine Möglichkeit, eine große ASP.NET Core MVC-Web-app in kleinere funktionale Einheiten zu partitionieren.</span><span class="sxs-lookup"><span data-stu-id="6d6df-107">Areas provide a way to partition a large ASP.NET Core MVC Web app into smaller functional groupings.</span></span> <span data-ttu-id="6d6df-108">Ein Bereich ist letztendlich eine MVC-Struktur in einer Anwendung.</span><span class="sxs-lookup"><span data-stu-id="6d6df-108">An area is effectively an MVC structure inside an application.</span></span> <span data-ttu-id="6d6df-109">Klicken Sie in einer MVC-Projekt logische Komponenten wie das Modell, Controller und Ansicht in unterschiedlichen Ordnern beibehalten werden, und MVC verwendet Konventionen zur Namensgebung, um die Beziehung zwischen diesen Komponenten zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="6d6df-109">In an MVC project, logical components like Model, Controller, and View are kept in different folders, and MVC uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="6d6df-110">Für eine große app kann es vorteilhaft sein, die app in separate hohe Ebene Bereiche der Funktionalität partitioniert sein.</span><span class="sxs-lookup"><span data-stu-id="6d6df-110">For a large app, it may be advantageous to partition the  app into separate high level areas of functionality.</span></span> <span data-ttu-id="6d6df-111">Z. B. eine e-Commerce-app mit mehreren Geschäftsbereichen, z. B. Auschecken, Abrechnung und Suche usw. Jede dieser Einheiten haben ihre eigenen logischen Komponentenansichten, Controller und Modelle.</span><span class="sxs-lookup"><span data-stu-id="6d6df-111">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search etc. Each of these units have their own logical component views, controllers, and models.</span></span> <span data-ttu-id="6d6df-112">In diesem Szenario können Sie Bereiche verwenden, um die Geschäftskomponenten im selben Projekt physisch zu partitionieren.</span><span class="sxs-lookup"><span data-stu-id="6d6df-112">In this scenario, you can use Areas to physically partition the business components in the same project.</span></span>

<span data-ttu-id="6d6df-113">Ein Bereich kann als kleinere funktionale Einheiten in einem ASP.NET Core MVC-Projekt mit einer eigenen Gruppe von Controller, Ansichten und Modellen definiert werden.</span><span class="sxs-lookup"><span data-stu-id="6d6df-113">An area can be defined as smaller functional units in an ASP.NET Core MVC project with its own set of controllers, views, and models.</span></span>

<span data-ttu-id="6d6df-114">Erwägen Sie Bereiche in einer MVC Projekts, wenn:</span><span class="sxs-lookup"><span data-stu-id="6d6df-114">Consider using Areas in an MVC project when:</span></span>

* <span data-ttu-id="6d6df-115">Ihre Anwendung mehrere allgemeine funktionalen Komponenten besteht, die logisch getrennt werden soll</span><span class="sxs-lookup"><span data-stu-id="6d6df-115">Your application is made of multiple high-level functional components that should be logically separated</span></span>

* <span data-ttu-id="6d6df-116">Sie möchten das MVC-Projekt zu partitionieren, sodass die einzelnen Funktionsbereiche unabhängig voneinander bearbeitet werden können</span><span class="sxs-lookup"><span data-stu-id="6d6df-116">You want to partition your MVC project so that each functional area can be worked on independently</span></span>

<span data-ttu-id="6d6df-117">Bereich-Funktionen:</span><span class="sxs-lookup"><span data-stu-id="6d6df-117">Area features:</span></span>

* <span data-ttu-id="6d6df-118">Eine ASP.NET-MVC-Anwendung Core kann beliebig viele Bereiche enthalten.</span><span class="sxs-lookup"><span data-stu-id="6d6df-118">An ASP.NET Core MVC app can have any number of areas</span></span>

* <span data-ttu-id="6d6df-119">Jeder Bereich verfügt über einen eigenen Domänencontroller, Modelle und Ansichten</span><span class="sxs-lookup"><span data-stu-id="6d6df-119">Each area has its own controllers, models, and views</span></span>

* <span data-ttu-id="6d6df-120">Ermöglicht es Ihnen, große MVC-Projekte in mehrere allgemeine Komponenten zu organisieren, die unabhängig voneinander bearbeitet werden können</span><span class="sxs-lookup"><span data-stu-id="6d6df-120">Allows you to organize large MVC projects into multiple high-level components that can be worked on independently</span></span>

* <span data-ttu-id="6d6df-121">Mehrere Domänencontroller mit dem gleichen Namen - unterstützt, solange sie verschiedene sind *Bereiche*</span><span class="sxs-lookup"><span data-stu-id="6d6df-121">Supports multiple controllers with the same name - as long as they have different *areas*</span></span>

<span data-ttu-id="6d6df-122">Werfen wir einen Blick auf ein Beispiel zur Veranschaulichung, wie Bereiche erstellt und verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="6d6df-122">Let's take a look at an example to illustrate how Areas are created and used.</span></span> <span data-ttu-id="6d6df-123">Angenommen, Sie haben eine Store-app, zwei unterschiedliche Gruppierungen der Controller und Ansichten verfügt: Produkte und Dienste.</span><span class="sxs-lookup"><span data-stu-id="6d6df-123">Let's say you have a store app that has two distinct groupings of controllers and views: Products and Services.</span></span> <span data-ttu-id="6d6df-124">Ein typische Ordner-Struktur für, dass mithilfe von MVC-Bereichen unten aussieht:</span><span class="sxs-lookup"><span data-stu-id="6d6df-124">A typical folder structure for that using MVC areas looks like below:</span></span>

* <span data-ttu-id="6d6df-125">Projektname</span><span class="sxs-lookup"><span data-stu-id="6d6df-125">Project name</span></span>

  * <span data-ttu-id="6d6df-126">Bereiche</span><span class="sxs-lookup"><span data-stu-id="6d6df-126">Areas</span></span>

    * <span data-ttu-id="6d6df-127">Produkte</span><span class="sxs-lookup"><span data-stu-id="6d6df-127">Products</span></span>

      * <span data-ttu-id="6d6df-128">Domänencontroller</span><span class="sxs-lookup"><span data-stu-id="6d6df-128">Controllers</span></span>

        * <span data-ttu-id="6d6df-129">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="6d6df-129">HomeController.cs</span></span>

        * <span data-ttu-id="6d6df-130">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="6d6df-130">ManageController.cs</span></span>

      * <span data-ttu-id="6d6df-131">Ansichten</span><span class="sxs-lookup"><span data-stu-id="6d6df-131">Views</span></span>

        * <span data-ttu-id="6d6df-132">Startseite</span><span class="sxs-lookup"><span data-stu-id="6d6df-132">Home</span></span>

          * <span data-ttu-id="6d6df-133">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="6d6df-133">Index.cshtml</span></span>

        * <span data-ttu-id="6d6df-134">Verwalten</span><span class="sxs-lookup"><span data-stu-id="6d6df-134">Manage</span></span>

          * <span data-ttu-id="6d6df-135">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="6d6df-135">Index.cshtml</span></span>

    * <span data-ttu-id="6d6df-136">Dienste</span><span class="sxs-lookup"><span data-stu-id="6d6df-136">Services</span></span>

      * <span data-ttu-id="6d6df-137">Domänencontroller</span><span class="sxs-lookup"><span data-stu-id="6d6df-137">Controllers</span></span>

        * <span data-ttu-id="6d6df-138">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="6d6df-138">HomeController.cs</span></span>

      * <span data-ttu-id="6d6df-139">Ansichten</span><span class="sxs-lookup"><span data-stu-id="6d6df-139">Views</span></span>

        * <span data-ttu-id="6d6df-140">Startseite</span><span class="sxs-lookup"><span data-stu-id="6d6df-140">Home</span></span>

          * <span data-ttu-id="6d6df-141">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="6d6df-141">Index.cshtml</span></span>

<span data-ttu-id="6d6df-142">Wenn MVC versucht, eine Ansicht in einem Bereich in der Standardeinstellung zu rendern, wird versucht, suchen Sie in den folgenden Speicherorten:</span><span class="sxs-lookup"><span data-stu-id="6d6df-142">When MVC tries to render a view in an Area, by default, it tries to look in the following locations:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
   /Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
   /Views/Shared/<Action-Name>.cshtml
   ```

<span data-ttu-id="6d6df-143">Hierbei handelt es sich um die Standardspeicherorte, die über geändert werden, können die `AreaViewLocationFormats` auf die `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.</span><span class="sxs-lookup"><span data-stu-id="6d6df-143">These are the default locations which can be changed via the `AreaViewLocationFormats` on the `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.</span></span>

<span data-ttu-id="6d6df-144">Beispielsweise ist in den folgenden Code anstatt den Namen des Ordners als "Bereiche", es wurde geändert in "Kategorien".</span><span class="sxs-lookup"><span data-stu-id="6d6df-144">For example, in the below code instead of having the folder name as 'Areas', it has been changed to 'Categories'.</span></span>

```csharp
services.Configure<RazorViewEngineOptions>(options =>
   {
       options.AreaViewLocationFormats.Clear();
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/{1}/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/Shared/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Views/Shared/{0}.cshtml");
   });
   ```

<span data-ttu-id="6d6df-145">Beachten Sie, die die Struktur wird der *Ansichten* Ordner ist die einzige hier wichtig angesehen wird, und wie des Inhalts der Rest der Ordner *Controller* und *Modelle* ist **nicht** von Bedeutung.</span><span class="sxs-lookup"><span data-stu-id="6d6df-145">One thing to note is that the structure of the *Views* folder is the only one which is considered important here and the content of the rest of the folders like *Controllers* and *Models* does **not** matter.</span></span> <span data-ttu-id="6d6df-146">Angenommen, Sie benötigen eine *Controller* und *Modelle* Ordner überhaupt.</span><span class="sxs-lookup"><span data-stu-id="6d6df-146">For example, you need not have a *Controllers* and *Models* folder at all.</span></span> <span data-ttu-id="6d6df-147">Dies funktioniert, da der Inhalt des *Controller* und *Modelle* ist nur Code, der in eine .dll kompiliert wird, wobei als Inhalt der *Ansichten* wird erst eine Anforderung an, die nicht Sicht wurde hergestellt.</span><span class="sxs-lookup"><span data-stu-id="6d6df-147">This works because the content of *Controllers* and *Models* is just code which gets compiled into a .dll where as the content of the *Views* isn't until a request to that view has been made.</span></span>

<span data-ttu-id="6d6df-148">Nachdem Sie die Ordnerhierarchie definiert haben, müssen Sie MVC mitteilen, dass jedem Controller einen Bereich zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="6d6df-148">Once you've defined the folder hierarchy, you need to tell MVC that each controller is associated with an area.</span></span> <span data-ttu-id="6d6df-149">Nachholen werden, indem den Namen des Controllers, mit der `[Area]` Attribut.</span><span class="sxs-lookup"><span data-stu-id="6d6df-149">You do that by decorating the controller name with the `[Area]` attribute.</span></span>

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

<span data-ttu-id="6d6df-150">Richten Sie eine Routendefinition, die mit der neu erstellten Bereiche arbeitet.</span><span class="sxs-lookup"><span data-stu-id="6d6df-150">Set up a route definition that works with your newly created areas.</span></span> <span data-ttu-id="6d6df-151">Die [Routing an Controlleraktionen](routing.md) Artikel ins Detail, über das Erstellen von Route-Definitionen, einschließlich der Verwendung von herkömmlicher Routen im Vergleich zu attributenrouten wechselt.</span><span class="sxs-lookup"><span data-stu-id="6d6df-151">The [Routing to Controller Actions](routing.md) article goes into detail about how to create route definitions, including using conventional routes versus attribute routes.</span></span> <span data-ttu-id="6d6df-152">In diesem Beispiel verwenden wir eine herkömmliche Route.</span><span class="sxs-lookup"><span data-stu-id="6d6df-152">In this example, we'll use a conventional route.</span></span> <span data-ttu-id="6d6df-153">Öffnen Sie hierzu die *Startup.cs* Datei, und ändern Sie ihn durch Hinzufügen der `areaRoute` mit dem Namen Route-Definition, die unten.</span><span class="sxs-lookup"><span data-stu-id="6d6df-153">To do so, open the *Startup.cs* file and modify it by adding the `areaRoute` named route definition below.</span></span>

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

<span data-ttu-id="6d6df-154">Navigieren zum `http://<yourApp>/products`, die `Index` Aktionsmethode von der `HomeController` in der `Products` Bereich aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="6d6df-154">Browsing to `http://<yourApp>/products`, the `Index` action method of the `HomeController` in the `Products` area will be invoked.</span></span>

## <a name="link-generation"></a><span data-ttu-id="6d6df-155">Linkgenerierung</span><span class="sxs-lookup"><span data-stu-id="6d6df-155">Link Generation</span></span>

* <span data-ttu-id="6d6df-156">Generieren von Links von einer Aktion innerhalb eines Bereichs basiert Controller an eine andere Aktion in den gleichen Controller.</span><span class="sxs-lookup"><span data-stu-id="6d6df-156">Generating links from an action within an area based controller to another action within the same controller.</span></span>

  <span data-ttu-id="6d6df-157">Nehmen wir an, dass die aktuelle Anforderung Pfad ähnelt`/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="6d6df-157">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="6d6df-158">HtmlHelper-Syntax:`@Html.ActionLink("Go to Product's Home Page", "Index")`</span><span class="sxs-lookup"><span data-stu-id="6d6df-158">HtmlHelper syntax: `@Html.ActionLink("Go to Product's Home Page", "Index")`</span></span>

  <span data-ttu-id="6d6df-159">Taghelpers Syntax:`<a asp-action="Index">Go to Product's Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="6d6df-159">TagHelper syntax: `<a asp-action="Index">Go to Product's Home Page</a>`</span></span>

  <span data-ttu-id="6d6df-160">Beachten Sie, dass wir nicht die Werte für "Area" und "Controller" bereitstellen müssen hier, wie sie bereits im Kontext der aktuellen Anforderung verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="6d6df-160">Note that we need not supply the 'area' and 'controller' values here as they're already available in the context of the current request.</span></span> <span data-ttu-id="6d6df-161">Diese Art von Werten heißen `ambient` Werte.</span><span class="sxs-lookup"><span data-stu-id="6d6df-161">These kind of values are called `ambient` values.</span></span>

* <span data-ttu-id="6d6df-162">Generieren von Links von einer Aktion innerhalb eines Bereichs an eine andere Aktion auf einen anderen Controller Controller basierend</span><span class="sxs-lookup"><span data-stu-id="6d6df-162">Generating links from an action within an area based controller to another action on a different controller</span></span>

  <span data-ttu-id="6d6df-163">Nehmen wir an, dass die aktuelle Anforderung Pfad ähnelt`/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="6d6df-163">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="6d6df-164">HtmlHelper-Syntax:`@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Manage")`</span><span class="sxs-lookup"><span data-stu-id="6d6df-164">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Manage")`</span></span>

  <span data-ttu-id="6d6df-165">Taghelpers Syntax:`<a asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="6d6df-165">TagHelper syntax: `<a asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`</span></span>

  <span data-ttu-id="6d6df-166">Beachten Sie, dass hier der Ambiente-Wert von einem "Bereich" verwendet jedoch der Wert "Controller" oben explizit angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="6d6df-166">Note that here the ambient value of an 'area' is used but the 'controller' value is specified explicitly above.</span></span>

* <span data-ttu-id="6d6df-167">Generieren von Links von einer Aktion innerhalb eines Bereichs basiert Controller an eine andere Aktion auf einen anderen Controller und einem anderen Bereich.</span><span class="sxs-lookup"><span data-stu-id="6d6df-167">Generating links from an action within an area based controller to another action on a different controller and a different area.</span></span>

  <span data-ttu-id="6d6df-168">Nehmen wir an, dass die aktuelle Anforderung Pfad ähnelt`/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="6d6df-168">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="6d6df-169">HtmlHelper-Syntax:`@Html.ActionLink("Go to Services’ Home Page", "Index", "Home", new { area = "Services" })`</span><span class="sxs-lookup"><span data-stu-id="6d6df-169">HtmlHelper syntax: `@Html.ActionLink("Go to Services’ Home Page", "Index", "Home", new { area = "Services" })`</span></span>

  <span data-ttu-id="6d6df-170">Taghelpers Syntax:`<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services’ Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="6d6df-170">TagHelper syntax: `<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services’ Home Page</a>`</span></span>

  <span data-ttu-id="6d6df-171">Beachten Sie, dass hier keine ambient-Werte verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="6d6df-171">Note that here no ambient values are used.</span></span>

* <span data-ttu-id="6d6df-172">Generieren von Links aus einer Aktion innerhalb eines Controllers Basis an eine andere Aktion auf einen anderen Controller und **nicht** in einem Bereich.</span><span class="sxs-lookup"><span data-stu-id="6d6df-172">Generating links from an action within an area based controller to another action on a different controller and **not** in an area.</span></span>

  <span data-ttu-id="6d6df-173">HtmlHelper-Syntax:`@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Home", new { area = "" })`</span><span class="sxs-lookup"><span data-stu-id="6d6df-173">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Home", new { area = "" })`</span></span>

  <span data-ttu-id="6d6df-174">Taghelpers Syntax:`<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="6d6df-174">TagHelper syntax: `<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`</span></span>

  <span data-ttu-id="6d6df-175">Da wir generieren möchten basierend Links zu einem Bereich bei den ambient-Wert für "Area" hier Controlleraktion, es leer.</span><span class="sxs-lookup"><span data-stu-id="6d6df-175">Since we want to generate links to a non-area based controller action, we empty the ambient value for 'area' here.</span></span>

## <a name="publishing-areas"></a><span data-ttu-id="6d6df-176">Veröffentlichen von Bereichen</span><span class="sxs-lookup"><span data-stu-id="6d6df-176">Publishing Areas</span></span>

<span data-ttu-id="6d6df-177">Alle `*.cshtml` und `wwwroot/**` Dateien werden veröffentlicht, wenn die Ausgabe `<Project Sdk="Microsoft.NET.Sdk.Web">` dient in der *csproj* Datei.</span><span class="sxs-lookup"><span data-stu-id="6d6df-177">All `*.cshtml` and `wwwroot/**` files are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the *.csproj* file.</span></span>
