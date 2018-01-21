---
title: Ansichten im Kern der ASP.NET MVC
author: ardalis
description: Erfahren Sie, wie Ansichten der app-datendarstellung und die Benutzerinteraktion in ASP.NET Core MVC behandeln.
ms.author: riande
manager: wpickett
ms.date: 12/12/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/overview
ms.openlocfilehash: c0a1f475941f3389e9aa1f5bb7819bef491b2cae
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="views-in-aspnet-core-mvc"></a><span data-ttu-id="2a07a-103">Ansichten im Kern der ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="2a07a-103">Views in ASP.NET Core MVC</span></span>

<span data-ttu-id="2a07a-104">Durch [Steve Smith](https://ardalis.com/) und [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2a07a-104">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="2a07a-105">Dieses Dokument erläutert, Ansichten, die in ASP.NET Core MVC-Anwendungen verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="2a07a-105">This document explains views used in ASP.NET Core MVC applications.</span></span> <span data-ttu-id="2a07a-106">Informationen für Razor-Seiten finden Sie unter [Einführung in Razor-Seiten](xref:mvc/razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="2a07a-106">For information on Razor Pages, see [Introduction to Razor Pages](xref:mvc/razor-pages/index).</span></span>

<span data-ttu-id="2a07a-107">In der **M**Odel -**V**vorhandenes -**C**Ontroller (MVC)-Muster, die *Ansicht* verarbeitet die app Daten Präsentation und Benutzerinteraktion.</span><span class="sxs-lookup"><span data-stu-id="2a07a-107">In the **M**odel-**V**iew-**C**ontroller (MVC) pattern, the *view* handles the app's data presentation and user interaction.</span></span> <span data-ttu-id="2a07a-108">Eine Sicht ist eine HTML-Vorlage mit eingebetteten [Razor Markup](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="2a07a-108">A view is an HTML template with embedded [Razor markup](xref:mvc/views/razor).</span></span> <span data-ttu-id="2a07a-109">Razor-Markup ist Code, der Interaktion mit HTML-Markup, um eine Webseite zu erzeugen, die an den Client gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="2a07a-109">Razor markup is code that interacts with HTML markup to produce a webpage that's sent to the client.</span></span>

<span data-ttu-id="2a07a-110">In ASP.NET-MVC Core Ansichten sind *cshtml* Dateien, die [C#-Programmiersprache](/dotnet/csharp/) in Razor-Markup.</span><span class="sxs-lookup"><span data-stu-id="2a07a-110">In ASP.NET Core MVC, views are *.cshtml* files that use the [C# programming language](/dotnet/csharp/) in Razor markup.</span></span> <span data-ttu-id="2a07a-111">In der Regel anzeigen von Dateien in Ordnern für jede von der app gruppiert [Controller](xref:mvc/controllers/actions).</span><span class="sxs-lookup"><span data-stu-id="2a07a-111">Usually, view files are grouped into folders named for each of the app's [controllers](xref:mvc/controllers/actions).</span></span> <span data-ttu-id="2a07a-112">Die Ordner befinden sich einem *Ansichten* Ordner im Stammverzeichnis der app:</span><span class="sxs-lookup"><span data-stu-id="2a07a-112">The folders are stored in a *Views* folder at the root of the app:</span></span>

![Ordner Views in Projektmappen-Explorer von Visual Studio ist mit dem Basisordner anzuzeigende About.cshtml Contact.cshtml und Index.cshtml Dateien öffnen öffnen](overview/_static/views_solution_explorer.png)

<span data-ttu-id="2a07a-114">Die *Home* Controller wird dargestellt, indem Sie eine *Home* Ordner innerhalb der *Ansichten* Ordner.</span><span class="sxs-lookup"><span data-stu-id="2a07a-114">The *Home* controller is represented by a *Home* folder inside the *Views* folder.</span></span> <span data-ttu-id="2a07a-115">Die *Home* Ordner enthält Ansichten für die *zu*, *Kontakt*, und *Index* (Startseite) Webseiten.</span><span class="sxs-lookup"><span data-stu-id="2a07a-115">The *Home* folder contains the views for the *About*, *Contact*, and *Index* (homepage) webpages.</span></span> <span data-ttu-id="2a07a-116">Wenn ein Benutzer fordert eine der drei Webseiten, Controlleraktionen in der *Home* Controller zu ermitteln, welche der drei Ansichten zum Erstellen und Zurückgeben einer Webseite für den Benutzer verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="2a07a-116">When a user requests one of these three webpages, controller actions in the *Home* controller determine which of the three views is used to build and return a webpage to the user.</span></span>

<span data-ttu-id="2a07a-117">Verwendung [Layouts](xref:mvc/views/layout) konsistent Webseite Abschnitte und codewiederholungen reduzieren.</span><span class="sxs-lookup"><span data-stu-id="2a07a-117">Use [layouts](xref:mvc/views/layout) to provide consistent webpage sections and reduce code repetition.</span></span> <span data-ttu-id="2a07a-118">Layouts enthalten oft den Header, Navigation und im Menü-Elemente und die Fußzeile.</span><span class="sxs-lookup"><span data-stu-id="2a07a-118">Layouts often contain the header, navigation and menu elements, and the footer.</span></span> <span data-ttu-id="2a07a-119">Kopf- und Fußzeilen enthalten normalerweise Textbaustein Markup für viele Metadatenelementen sowie Links zu Skripts und des Stils Bestand.</span><span class="sxs-lookup"><span data-stu-id="2a07a-119">The header and footer usually contain boilerplate markup for many metadata elements and links to script and style assets.</span></span> <span data-ttu-id="2a07a-120">Layouts können Sie die diesem Textbaustein Markup in Ihren Ansichten zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="2a07a-120">Layouts help you avoid this boilerplate markup in your views.</span></span>

<span data-ttu-id="2a07a-121">[Teilansichten](xref:mvc/views/partial) Codeduplikaten durch die Verwaltung von wieder verwendbare Teile der Ansichten reduzieren.</span><span class="sxs-lookup"><span data-stu-id="2a07a-121">[Partial views](xref:mvc/views/partial) reduce code duplication by managing reusable parts of views.</span></span> <span data-ttu-id="2a07a-122">Eine Teilansicht eignet sich z. B. für ein Autor Profildaten für eine Blogwebsite, die in mehreren Ansichten angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="2a07a-122">For example, a partial view is useful for an author biography on a blog website that appears in several views.</span></span> <span data-ttu-id="2a07a-123">Ein Autor Profildaten ist Normalansicht Inhalt und erfordert Code zum Ausführen, um den Inhalt für die Webseite zu erzeugen.</span><span class="sxs-lookup"><span data-stu-id="2a07a-123">An author biography is ordinary view content and doesn't require code to execute in order to produce the content for the webpage.</span></span> <span data-ttu-id="2a07a-124">Autor Profildaten Inhalt ist an die Ansicht von allein, wurden die modellbindung verfügbar, deshalb ideal eine Teilansicht für diesen Typ von Inhalt zu verwenden ist.</span><span class="sxs-lookup"><span data-stu-id="2a07a-124">Author biography content is available to the view by model binding alone, so using a partial view for this type of content is ideal.</span></span>

<span data-ttu-id="2a07a-125">[Anzeigen von Komponenten](xref:mvc/views/view-components) sind partielle ähnlich wie Ansichten, sie codewiederholungen reduzieren können, aber das erscheint für Inhalt anzeigen, die auf dem Server ausgeführt wird, um die Webseite rendern Code erfordert geeignet.</span><span class="sxs-lookup"><span data-stu-id="2a07a-125">[View components](xref:mvc/views/view-components) are similar to partial views in that they allow you to reduce repetitive code, but they're appropriate for view content that requires code to run on the server in order to render the webpage.</span></span> <span data-ttu-id="2a07a-126">Anzeigen von Komponenten sind nützlich, wenn es sich bei der gerenderte Inhalt Datenbankinteraktion haben, z. B. für eine Website mit dem Einkaufswagen ist erforderlich.</span><span class="sxs-lookup"><span data-stu-id="2a07a-126">View components are useful when the rendered content requires database interaction, such as for a website shopping cart.</span></span> <span data-ttu-id="2a07a-127">Ansichtskomponenten sind nicht begrenzt auf die um Bindung zu modellieren, um die Ausgabe der Webseite.</span><span class="sxs-lookup"><span data-stu-id="2a07a-127">View components aren't limited to model binding in order to produce webpage output.</span></span>

## <a name="benefits-of-using-views"></a><span data-ttu-id="2a07a-128">Vorteile der Verwendung von Sichten</span><span class="sxs-lookup"><span data-stu-id="2a07a-128">Benefits of using views</span></span>

<span data-ttu-id="2a07a-129">Ansichten zur Verfügung, zum Herstellen einer [ **S**Eparation **o**f **C**Oncerns (SoC) Entwurf](http://deviq.com/separation-of-concerns/) innerhalb einer MVC-app durch die Trennung von Benutzer-Schnittstelle Markup aus andere Teile der app.</span><span class="sxs-lookup"><span data-stu-id="2a07a-129">Views help to establish a [**S**eparation **o**f **C**oncerns (SoC) design](http://deviq.com/separation-of-concerns/) within an MVC app by separating the user interface markup from other parts of the app.</span></span> <span data-ttu-id="2a07a-130">SoC Entwurf nach macht Ihre app modular aufgebaut, die bietet mehrere Vorteile:</span><span class="sxs-lookup"><span data-stu-id="2a07a-130">Following SoC design makes your app modular, which provides several benefits:</span></span>

* <span data-ttu-id="2a07a-131">Die app ist einfacher zu verwalten, da es besser organisiert ist.</span><span class="sxs-lookup"><span data-stu-id="2a07a-131">The app is easier to maintain because it's better organized.</span></span> <span data-ttu-id="2a07a-132">Ansichten werden im Allgemeinen von app-Funktion gruppiert.</span><span class="sxs-lookup"><span data-stu-id="2a07a-132">Views are generally grouped by app feature.</span></span> <span data-ttu-id="2a07a-133">Dies erleichtert die zugehörige Sichten zu suchen, bei der Arbeit auf eine Funktion.</span><span class="sxs-lookup"><span data-stu-id="2a07a-133">This makes it easier to find related views when working on a feature.</span></span>
* <span data-ttu-id="2a07a-134">Die Teile der app sind lose verbunden.</span><span class="sxs-lookup"><span data-stu-id="2a07a-134">The parts of the app are loosely coupled.</span></span> <span data-ttu-id="2a07a-135">Erstellen und aktualisieren die app-Ansichten getrennt von der Logik und die Daten Zugriff Geschäftskomponenten.</span><span class="sxs-lookup"><span data-stu-id="2a07a-135">You can build and update the app's views separately from the business logic and data access components.</span></span> <span data-ttu-id="2a07a-136">Sie können die Ansichten der app ändern, ohne unbedingt andere Teile der app zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="2a07a-136">You can modify the views of the app without necessarily having to update other parts of the app.</span></span>
* <span data-ttu-id="2a07a-137">Es ist einfacher, das Benutzeroberflächenkomponenten der app zu testen, da die Ansichten separaten Einheiten sind.</span><span class="sxs-lookup"><span data-stu-id="2a07a-137">It's easier to test the user interface parts of the app because the views are separate units.</span></span>
* <span data-ttu-id="2a07a-138">Aufgrund der besseren Organisation ist es weniger wahrscheinlich, dass Sie versehentlich wiederholen Abschnitte der Benutzeroberfläche werden.</span><span class="sxs-lookup"><span data-stu-id="2a07a-138">Due to better organization, it's less likely that you'll accidently repeat sections of the user interface.</span></span>

## <a name="creating-a-view"></a><span data-ttu-id="2a07a-139">Erstellen einer Ansicht</span><span class="sxs-lookup"><span data-stu-id="2a07a-139">Creating a view</span></span>

<span data-ttu-id="2a07a-140">Sichten, die für einen Controller spezifisch sind werden erstellt, der *Ansichten / [ControllerName]* Ordner.</span><span class="sxs-lookup"><span data-stu-id="2a07a-140">Views that are specific to a controller are created in the *Views/[ControllerName]* folder.</span></span> <span data-ttu-id="2a07a-141">Sichten, die für Domänencontroller, freigegeben werden befinden sich der *Ansichten/freigegeben* Ordner.</span><span class="sxs-lookup"><span data-stu-id="2a07a-141">Views that are shared among controllers are placed in the *Views/Shared* folder.</span></span> <span data-ttu-id="2a07a-142">Um eine Ansicht zu erstellen, fügen Sie eine neue Datei hinzu, und geben Sie ihm den gleichen Namen wie der Aktion zugeordneten Controller mit dem *cshtml* Dateierweiterung.</span><span class="sxs-lookup"><span data-stu-id="2a07a-142">To create a view, add a new file and give it the same name as its associated controller action with the *.cshtml* file extension.</span></span> <span data-ttu-id="2a07a-143">Eine Sicht erstellt, der mit übereinstimmt der *zu* Aktion in der *Home* Controller, erstellen eine *About.cshtml* in der Datei die *Ansichten/Start*Ordner:</span><span class="sxs-lookup"><span data-stu-id="2a07a-143">To create a view that corresponds with the *About* action in the *Home* controller, create an *About.cshtml* file in the *Views/Home* folder:</span></span>

[!code-cshtml[Main](../../common/samples/WebApplication1/Views/Home/About.cshtml)]

<span data-ttu-id="2a07a-144">*Razor* Markup beginnt mit der `@` Symbol.</span><span class="sxs-lookup"><span data-stu-id="2a07a-144">*Razor* markup starts with the `@` symbol.</span></span> <span data-ttu-id="2a07a-145">Zur C#-Anweisungen durch Platzieren von C#-code in [Razor Codeblöcke](xref:mvc/views/razor#razor-code-blocks) abgegrenzt von geschweiften Klammern (`{ ... }`).</span><span class="sxs-lookup"><span data-stu-id="2a07a-145">Run C# statements by placing C# code within [Razor code blocks](xref:mvc/views/razor#razor-code-blocks) set off by curly braces (`{ ... }`).</span></span> <span data-ttu-id="2a07a-146">Finden Sie beispielsweise die Zuweisung von "About" um `ViewData["Title"]` oben.</span><span class="sxs-lookup"><span data-stu-id="2a07a-146">For example, see the assignment of "About" to `ViewData["Title"]` shown above.</span></span> <span data-ttu-id="2a07a-147">Sie können Werte in HTML anzeigen, indem Sie einfach verweisen auf den Wert mit dem `@` Symbol.</span><span class="sxs-lookup"><span data-stu-id="2a07a-147">You can display values within HTML by simply referencing the value with the `@` symbol.</span></span> <span data-ttu-id="2a07a-148">Den Inhalt der `<h2>` und `<h3>` oben aufgeführten Elemente.</span><span class="sxs-lookup"><span data-stu-id="2a07a-148">See the contents of the `<h2>` and `<h3>` elements above.</span></span>

<span data-ttu-id="2a07a-149">Den Inhalt der oben gezeigten ist nur ein Teil der gesamten Webseite, die für dem Benutzer gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="2a07a-149">The view content shown above is only part of the entire webpage that's rendered to the user.</span></span> <span data-ttu-id="2a07a-150">Der Rest der Seite Layout und andere allgemeine Aspekte der Sicht werden angegeben, in anderen Dateien anzeigen.</span><span class="sxs-lookup"><span data-stu-id="2a07a-150">The rest of the page's layout and other common aspects of the view are specified in other view files.</span></span> <span data-ttu-id="2a07a-151">Weitere Informationen finden Sie unter der [Layout Thema](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="2a07a-151">To learn more, see the [Layout topic](xref:mvc/views/layout).</span></span>

## <a name="how-controllers-specify-views"></a><span data-ttu-id="2a07a-152">Wie Controller Ansichten angeben</span><span class="sxs-lookup"><span data-stu-id="2a07a-152">How controllers specify views</span></span>

<span data-ttu-id="2a07a-153">Ansichten werden in der Regel zurückgegeben, von Aktionen als ein [ViewResult](/aspnet/core/api/microsoft.aspnetcore.mvc.viewresult), d. h. einen Typ von [ActionResult](/aspnet/core/api/microsoft.aspnetcore.mvc.actionresult).</span><span class="sxs-lookup"><span data-stu-id="2a07a-153">Views are typically returned from actions as a [ViewResult](/aspnet/core/api/microsoft.aspnetcore.mvc.viewresult), which is a type of [ActionResult](/aspnet/core/api/microsoft.aspnetcore.mvc.actionresult).</span></span> <span data-ttu-id="2a07a-154">Die Aktionsmethode erstellen und zurückgeben kann eine `ViewResult` direkt, jedoch, ist nicht im Allgemeinen vorgenommen.</span><span class="sxs-lookup"><span data-stu-id="2a07a-154">Your action method can create and return a `ViewResult` directly, but that isn't commonly done.</span></span> <span data-ttu-id="2a07a-155">Da die meisten Domänencontroller erben [Controller](/aspnet/core/api/microsoft.aspnetcore.mvc.controller), verwenden Sie einfach die `View` Hilfsmethode zurückzugebenden der `ViewResult`:</span><span class="sxs-lookup"><span data-stu-id="2a07a-155">Since most controllers inherit from [Controller](/aspnet/core/api/microsoft.aspnetcore.mvc.controller), you simply use the `View` helper method to return the `ViewResult`:</span></span>

<span data-ttu-id="2a07a-156">*HomeController.cs*</span><span class="sxs-lookup"><span data-stu-id="2a07a-156">*HomeController.cs*</span></span>

[!code-csharp[Main](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]

<span data-ttu-id="2a07a-157">Wenn diese Aktion zurückgegeben wird, die *About.cshtml* anzeigen, die im vorherigen Abschnitt gezeigt als der folgenden Webseite gerendert wird:</span><span class="sxs-lookup"><span data-stu-id="2a07a-157">When this action returns, the *About.cshtml* view shown in the last section is rendered as the following webpage:</span></span>

![Zu den Seiten im Edge-Browser gerendert](overview/_static/about-page.png)

<span data-ttu-id="2a07a-159">Die `View` -Hilfsmethode verfügt über mehrere Überladungen.</span><span class="sxs-lookup"><span data-stu-id="2a07a-159">The `View` helper method has several overloads.</span></span> <span data-ttu-id="2a07a-160">Optional können Sie Folgendes angeben:</span><span class="sxs-lookup"><span data-stu-id="2a07a-160">You can optionally specify:</span></span>

* <span data-ttu-id="2a07a-161">Eine explizite Ansicht zurückgeben:</span><span class="sxs-lookup"><span data-stu-id="2a07a-161">An explicit view to return:</span></span>

  ```csharp
  return View("Orders");
  ```
* <span data-ttu-id="2a07a-162">Ein [Modell](xref:mvc/models/model-binding) Übergabe an die die Sicht:</span><span class="sxs-lookup"><span data-stu-id="2a07a-162">A [model](xref:mvc/models/model-binding) to pass to the the view:</span></span>

  ```csharp
  return View(Orders);
  ```
* <span data-ttu-id="2a07a-163">Eine Sicht und ein Modell:</span><span class="sxs-lookup"><span data-stu-id="2a07a-163">Both a view and a model:</span></span>

  ```csharp
  return View("Orders", Orders);
  ```

### <a name="view-discovery"></a><span data-ttu-id="2a07a-164">View-Ermittlung</span><span class="sxs-lookup"><span data-stu-id="2a07a-164">View discovery</span></span>

<span data-ttu-id="2a07a-165">Ein Prozess wird aufgerufen, wenn eine Aktion eine Sicht zurückgegeben wird, *Ansicht Ermittlung* erfolgt.</span><span class="sxs-lookup"><span data-stu-id="2a07a-165">When an action returns a view, a process called *view discovery* takes place.</span></span> <span data-ttu-id="2a07a-166">Dieser Prozess wird bestimmt, welche Datei verwendet wird anhand des Ansichtsnamens.</span><span class="sxs-lookup"><span data-stu-id="2a07a-166">This process determines which view file is used based on the view name.</span></span> 

<span data-ttu-id="2a07a-167">Das Standardverhalten der `View` Methode (`return View();`) ist eine Sicht mit dem gleichen Namen wie die Aktionsmethode zurückgeben, von dem sie aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="2a07a-167">The default behavior of the `View` method (`return View();`) is to return a view with the same name as the action method from which it's called.</span></span> <span data-ttu-id="2a07a-168">Z. B. die *zu* `ActionResult` Methodennamen des Controllers wird zur Suche nach einer Datei anzeigen, die mit dem Namen *About.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="2a07a-168">For example, the *About* `ActionResult` method name of the controller is used to search for a view file named *About.cshtml*.</span></span> <span data-ttu-id="2a07a-169">Die Common Language Runtime sucht zuerst die *Ansichten / [ControllerName]* Ordner für die Ansicht.</span><span class="sxs-lookup"><span data-stu-id="2a07a-169">First, the runtime looks in the *Views/[ControllerName]* folder for the view.</span></span> <span data-ttu-id="2a07a-170">Wenn es keine entsprechende Ansicht gefunden werden, sucht der *Shared* Ordner für die Ansicht.</span><span class="sxs-lookup"><span data-stu-id="2a07a-170">If it doesn't find a matching view there, it searches the *Shared* folder for the view.</span></span>

<span data-ttu-id="2a07a-171">Es spielt keine Rolle, wenn Sie, implizit zurückkehren die `ViewResult` mit `return View();` oder übergeben Sie den Namen der Ansicht, um explizit die `View` Methode mit `return View("<ViewName>");`.</span><span class="sxs-lookup"><span data-stu-id="2a07a-171">It doesn't matter if you implicitly return the `ViewResult` with `return View();` or explicitly pass the view name to the `View` method with `return View("<ViewName>");`.</span></span> <span data-ttu-id="2a07a-172">Zeigen Sie in beiden Fällen Ermittlung sucht nach einer übereinstimmenden Ansichtsdatei, in der angegebenen Reihenfolge aus:</span><span class="sxs-lookup"><span data-stu-id="2a07a-172">In both cases, view discovery searches for a matching view file in this order:</span></span>

   1. <span data-ttu-id="2a07a-173">*Views/\[ControllerName]\[ViewName].cshtml*</span><span class="sxs-lookup"><span data-stu-id="2a07a-173">*Views/\[ControllerName]\[ViewName].cshtml*</span></span>
   1. <span data-ttu-id="2a07a-174">*Views/Shared/\[ViewName].cshtml*</span><span class="sxs-lookup"><span data-stu-id="2a07a-174">*Views/Shared/\[ViewName].cshtml*</span></span>

<span data-ttu-id="2a07a-175">Anstatt ein Ansichtsname kann ein Dateipfad für die Sicht angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="2a07a-175">A view file path can be provided instead of a view name.</span></span> <span data-ttu-id="2a07a-176">Wenn einen absoluten Pfad, angefangen beim Stamm app verwenden (optional beginnend mit "/" oder "~ /"), wird die *cshtml* Erweiterung muss angegeben werden:</span><span class="sxs-lookup"><span data-stu-id="2a07a-176">If using an absolute path starting at the app root (optionally starting with "/" or "~/"), the *.cshtml* extension must be specified:</span></span>

```csharp
return View("Views/Home/About.cshtml");
```

<span data-ttu-id="2a07a-177">Sie können auch einen relativen Pfad zum Angeben von Ansichten in verschiedenen Verzeichnissen ohne die *cshtml* Erweiterung.</span><span class="sxs-lookup"><span data-stu-id="2a07a-177">You can also use a relative path to specify views in different directories without the *.cshtml* extension.</span></span> <span data-ttu-id="2a07a-178">Innerhalb der `HomeController`, können Sie zurückkehren, die *Index* -Ansicht Ihrer *verwalten* Ansichten mit einem relativen Pfad:</span><span class="sxs-lookup"><span data-stu-id="2a07a-178">Inside the `HomeController`, you can return the *Index* view of your *Manage* views with a relative path:</span></span>

```csharp
return View("../Manage/Index");
```

<span data-ttu-id="2a07a-179">Auf ähnliche Weise können Sie angeben, das aktuelle Controller-spezifische Verzeichnis mit dem ". /" Präfix:</span><span class="sxs-lookup"><span data-stu-id="2a07a-179">Similarly, you can indicate the current controller-specific directory with the "./" prefix:</span></span>

```csharp
return View("./About");
```

<span data-ttu-id="2a07a-180">[Teilansichten](xref:mvc/views/partial) und [anzeigen Komponenten](xref:mvc/views/view-components) ähnliche (aber nicht identisch) Ermittlungsmechanismus verwenden.</span><span class="sxs-lookup"><span data-stu-id="2a07a-180">[Partial views](xref:mvc/views/partial) and [view components](xref:mvc/views/view-components) use similar (but not identical) discovery mechanisms.</span></span>

<span data-ttu-id="2a07a-181">Sie können anpassen, dass die Standardkonvention dafür, wie Ansichten innerhalb der app gespeichert werden, mithilfe ein benutzerdefinierten [IViewLocationExpander](/aspnet/core/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander).</span><span class="sxs-lookup"><span data-stu-id="2a07a-181">You can customize the default convention for how views are located within the app by using a custom [IViewLocationExpander](/aspnet/core/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander).</span></span>

<span data-ttu-id="2a07a-182">Ermittlung der Ansicht basiert auf Dateinamen Suchen von Dateien anzeigen.</span><span class="sxs-lookup"><span data-stu-id="2a07a-182">View discovery relies on finding view files by file name.</span></span> <span data-ttu-id="2a07a-183">Wenn das zugrunde liegende Dateisystem Groß-/Kleinschreibung beachtet wird, werden Sichtnamen wahrscheinlich Groß-/Kleinschreibung beachtet.</span><span class="sxs-lookup"><span data-stu-id="2a07a-183">If the underlying file system is case sensitive, view names are probably case sensitive.</span></span> <span data-ttu-id="2a07a-184">Aus Kompatibilitätsgründen betriebssystemübergreifende übereinstimmen Sie Schreibweise und die zwischen Controller und Aktion und zugeordnete Ansicht-Ordner und Dateinamen.</span><span class="sxs-lookup"><span data-stu-id="2a07a-184">For compatibility across operating systems, match case between controller and action names and associated view folders and file names.</span></span> <span data-ttu-id="2a07a-185">Wenn ein Fehler aufgetreten ist, den eine Datei nicht gefunden werden kann, bei der Arbeit mit einem Dateisystem Groß-/Kleinschreibung beachtet wird, vergewissern Sie sich, dass die Groß-/Kleinschreibung zwischen die angeforderte Ansicht-Datei und den Dateinamen der tatsächlichen Ansicht übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="2a07a-185">If you encounter an error that a view file can't be found while working with a case-sensitive file system, confirm that the casing matches between the requested view file and the actual view file name.</span></span>

<span data-ttu-id="2a07a-186">Führen Sie die bewährte Methode zum Organisieren von der Dateistruktur für Ihre Ansichten, um die Beziehungen zwischen Domänencontrollern, Aktionen und Ansichten für die Verwaltbarkeit und Klarheit widerzuspiegeln.</span><span class="sxs-lookup"><span data-stu-id="2a07a-186">Follow the best practice of organizing the file structure for your views to reflect the relationships among controllers, actions, and views for maintainability and clarity.</span></span>

## <a name="passing-data-to-views"></a><span data-ttu-id="2a07a-187">Übergeben von Daten mit Ansichten</span><span class="sxs-lookup"><span data-stu-id="2a07a-187">Passing data to views</span></span>

<span data-ttu-id="2a07a-188">Sie können Daten mit Ansichten mithilfe von mehreren Ansätzen übergeben.</span><span class="sxs-lookup"><span data-stu-id="2a07a-188">You can pass data to views using several approaches.</span></span> <span data-ttu-id="2a07a-189">Die zuverlässigste Methode ist die Angabe einer [Modell](xref:mvc/models/model-binding) Typ in der Ansicht.</span><span class="sxs-lookup"><span data-stu-id="2a07a-189">The most robust approach is to specify a [model](xref:mvc/models/model-binding) type in the view.</span></span> <span data-ttu-id="2a07a-190">Dieses Modell wird häufig als bezeichnet eine *Viewmodel*.</span><span class="sxs-lookup"><span data-stu-id="2a07a-190">This model is commonly referred to as a *viewmodel*.</span></span> <span data-ttu-id="2a07a-191">Sie können eine Instanz des Typs Viewmodel zur Ansicht übergeben, von der Aktion.</span><span class="sxs-lookup"><span data-stu-id="2a07a-191">You pass an instance of the viewmodel type to the view from the action.</span></span>

<span data-ttu-id="2a07a-192">Ein Viewmodel Daten an eine Ansicht zu übergeben, können die Ansicht zu nutzen *starken* Überprüfung des Typs.</span><span class="sxs-lookup"><span data-stu-id="2a07a-192">Using a viewmodel to pass data to a view allows the view to take advantage of *strong* type checking.</span></span> <span data-ttu-id="2a07a-193">*Starke Typisierung* (oder *stark typisierte*) bedeutet, dass jede Variable und jede Konstante einen explizit definierten Typ aufweist (z. B. `string`, `int`, oder `DateTime`).</span><span class="sxs-lookup"><span data-stu-id="2a07a-193">*Strong typing* (or *strongly-typed*) means that every variable and constant has an explicitly defined type (for example, `string`, `int`, or `DateTime`).</span></span> <span data-ttu-id="2a07a-194">Die Gültigkeit der Typen, die in einer Ansicht verwendet, die zum Zeitpunkt der Kompilierung aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="2a07a-194">The validity of types used in a view is checked at compile time.</span></span>

<span data-ttu-id="2a07a-195">[Visual Studio](https://www.visualstudio.com/vs/) und [Visual Studio Code](https://code.visualstudio.com/) Liste stark typisierte Klasse, Elemente mit einem Feature [IntelliSense](/visualstudio/ide/using-intellisense).</span><span class="sxs-lookup"><span data-stu-id="2a07a-195">[Visual Studio](https://www.visualstudio.com/vs/) and [Visual Studio Code](https://code.visualstudio.com/) list strongly-typed class members using a feature called [IntelliSense](/visualstudio/ide/using-intellisense).</span></span> <span data-ttu-id="2a07a-196">Wenn Sie die Eigenschaften von einem Viewmodel anzeigen möchten, geben Sie den Variablennamen ein für das Viewmodel, gefolgt von einem Punkt (`.`).</span><span class="sxs-lookup"><span data-stu-id="2a07a-196">When you want to see the properties of a viewmodel, type the variable name for the viewmodel followed by a period (`.`).</span></span> <span data-ttu-id="2a07a-197">Dadurch können Sie Code schneller mit weniger Fehlern zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="2a07a-197">This helps you write code faster with fewer errors.</span></span>

<span data-ttu-id="2a07a-198">Geben Sie ein Modell mithilfe der `@model` Richtlinie.</span><span class="sxs-lookup"><span data-stu-id="2a07a-198">Specify a model using the `@model` directive.</span></span> <span data-ttu-id="2a07a-199">Verwenden Sie das Modell mit `@Model`:</span><span class="sxs-lookup"><span data-stu-id="2a07a-199">Use the model with `@Model`:</span></span>

```cshtml
@model WebApplication1.ViewModels.Address

<h2>Contact</h2>
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

<span data-ttu-id="2a07a-200">Um das Modell zur Ansicht bereitzustellen, übergibt er der Controller als Parameter:</span><span class="sxs-lookup"><span data-stu-id="2a07a-200">To provide the model to the view, the controller passes it as a parameter:</span></span>

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

<span data-ttu-id="2a07a-201">Es gelten keine Einschränkungen für die Typen, die Sie eine Ansicht bereitstellen können.</span><span class="sxs-lookup"><span data-stu-id="2a07a-201">There are no restrictions on the model types that you can provide to a view.</span></span> <span data-ttu-id="2a07a-202">Es wird empfohlen, **P**unformatierten **O**%ld **C**LR **O**Viewmodels-Objekt (POCO), mit wenigen oder gar keinen Verhalten (Methoden) definiert.</span><span class="sxs-lookup"><span data-stu-id="2a07a-202">We recommend using **P**lain **O**ld **C**LR **O**bject (POCO) viewmodels with little or no behavior (methods) defined.</span></span> <span data-ttu-id="2a07a-203">Viewmodel-Klassen werden in der Regel entweder gespeichert, der *Modelle* Ordner oder ein separates *ViewModels* Ordner im Stammverzeichnis der app.</span><span class="sxs-lookup"><span data-stu-id="2a07a-203">Usually, viewmodel classes are either stored in the *Models* folder or a separate *ViewModels* folder at the root of the app.</span></span> <span data-ttu-id="2a07a-204">Die *Adresse* Viewmodel verwendet, die im obigen Beispiel ist eine POCO-Viewmodel gespeichert in einer Datei namens *Address.cs*:</span><span class="sxs-lookup"><span data-stu-id="2a07a-204">The *Address* viewmodel used in the example above is a POCO viewmodel stored in a file named *Address.cs*:</span></span>

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
> <span data-ttu-id="2a07a-205">Nichts daran hindert Sie die gleichen Klassen für die Viewmodel-Typen und Ihre Business-Modelltypen verwenden.</span><span class="sxs-lookup"><span data-stu-id="2a07a-205">Nothing prevents you from using the same classes for both your viewmodel types and your business model types.</span></span> <span data-ttu-id="2a07a-206">Verwenden von separaten Modellen kann jedoch Ihre Ansichten unabhängig von der Geschäftslogik und die Daten Zugriff Teile Ihrer app variieren.</span><span class="sxs-lookup"><span data-stu-id="2a07a-206">However, using separate models allows your views to vary independently from the business logic and data access parts of your app.</span></span> <span data-ttu-id="2a07a-207">Trennung von Modellen und Viewmodels bietet zudem Sicherheitsvorteile, wenn Modelle verwenden [modellbindung](xref:mvc/models/model-binding) und [Überprüfung](xref:mvc/models/validation) für Daten, die an die app vom Benutzer gesendet.</span><span class="sxs-lookup"><span data-stu-id="2a07a-207">Separation of models and viewmodels also offers security benefits when models use [model binding](xref:mvc/models/model-binding) and [validation](xref:mvc/models/validation) for data sent to the app by the user.</span></span>


<a name="VD_VB"></a>

### <a name="weakly-typed-data-viewdata-and-viewbag"></a><span data-ttu-id="2a07a-208">Schwach typisierte Daten (ViewData und ViewBag)</span><span class="sxs-lookup"><span data-stu-id="2a07a-208">Weakly-typed data (ViewData and ViewBag)</span></span>

<span data-ttu-id="2a07a-209">Hinweis: `ViewBag` ist in der Razor-Seiten nicht verfügbar.</span><span class="sxs-lookup"><span data-stu-id="2a07a-209">Note: `ViewBag` is not available in the Razor Pages.</span></span>

<span data-ttu-id="2a07a-210">Zusätzlich zu den stark typisierten Ansichten Ansichten haben Zugriff auf eine *schwach typisierte* (so genannte *lose typisierten*) Sammeln von Daten.</span><span class="sxs-lookup"><span data-stu-id="2a07a-210">In addition to strongly-typed views, views have access to a *weakly-typed* (also called *loosely-typed*) collection of data.</span></span> <span data-ttu-id="2a07a-211">Im Gegensatz zu starke Typen *unsichere Typen* (oder *lose Typen*) bedeutet, dass Sie explizit den Typ der Daten deklarieren nicht, Sie verwenden.</span><span class="sxs-lookup"><span data-stu-id="2a07a-211">Unlike strong types, *weak types* (or *loose types*) means that you don't explicitly declare the type of data you're using.</span></span> <span data-ttu-id="2a07a-212">Die Auflistung der schwach typisierten Daten können für kleine Mengen von Daten aus dem Controller und Ansichten übergeben.</span><span class="sxs-lookup"><span data-stu-id="2a07a-212">You can use the collection of weakly-typed data for passing small amounts of data in and out of controllers and views.</span></span>

| <span data-ttu-id="2a07a-213">Übergeben von Daten zwischen einem...</span><span class="sxs-lookup"><span data-stu-id="2a07a-213">Passing data between a ...</span></span>                        | <span data-ttu-id="2a07a-214">Beispiel</span><span class="sxs-lookup"><span data-stu-id="2a07a-214">Example</span></span>                                                                        |
| ------------------------------------------------- | ------------------------------------------------------------------------------ |
| <span data-ttu-id="2a07a-215">Controller und einer Ansicht</span><span class="sxs-lookup"><span data-stu-id="2a07a-215">Controller and a view</span></span>                             | <span data-ttu-id="2a07a-216">Auffüllen einer Dropdownliste mit Daten.</span><span class="sxs-lookup"><span data-stu-id="2a07a-216">Populating a dropdown list with data.</span></span>                                          |
| <span data-ttu-id="2a07a-217">Anzeigen und eine [Layoutansicht](xref:mvc/views/layout)</span><span class="sxs-lookup"><span data-stu-id="2a07a-217">View and a [layout view](xref:mvc/views/layout)</span></span>   | <span data-ttu-id="2a07a-218">Festlegen der  **\<Title >** Elementinhalt in der Layoutansicht aus einer Datei anzeigen.</span><span class="sxs-lookup"><span data-stu-id="2a07a-218">Setting the **\<title>** element content in the layout view from a view file.</span></span>  |
| <span data-ttu-id="2a07a-219">[Teilansicht](xref:mvc/views/partial) und einer Ansicht</span><span class="sxs-lookup"><span data-stu-id="2a07a-219">[Partial view](xref:mvc/views/partial) and a view</span></span> | <span data-ttu-id="2a07a-220">Ein Widget, das Daten basierend auf der Webseite, die vom Benutzer angeforderte anzeigt.</span><span class="sxs-lookup"><span data-stu-id="2a07a-220">A widget that displays data based on the webpage that the user requested.</span></span>      |

<span data-ttu-id="2a07a-221">Diese Auflistung kann verwiesen werden, entweder über die `ViewData` oder `ViewBag` Eigenschaften für Controller und Ansichten.</span><span class="sxs-lookup"><span data-stu-id="2a07a-221">This collection can be referenced through either the `ViewData` or `ViewBag` properties on controllers and views.</span></span> <span data-ttu-id="2a07a-222">Die `ViewData` Eigenschaft ist ein Wörterbuch von schwach typisierte Objekte.</span><span class="sxs-lookup"><span data-stu-id="2a07a-222">The `ViewData` property is a dictionary of weakly-typed objects.</span></span> <span data-ttu-id="2a07a-223">Die `ViewBag` Eigenschaft ist ein Wrapper um `ViewData` , dynamische Eigenschaften bereitstellt, für das zugrunde liegende `ViewData` Auflistung.</span><span class="sxs-lookup"><span data-stu-id="2a07a-223">The `ViewBag` property is a wrapper around `ViewData` that provides dynamic properties for the underlying `ViewData` collection.</span></span>

<span data-ttu-id="2a07a-224">`ViewData`und `ViewBag` dynamisch zur Laufzeit aufgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="2a07a-224">`ViewData` and `ViewBag` are dynamically resolved at runtime.</span></span> <span data-ttu-id="2a07a-225">Seit der Kompilierung typüberprüfung bieten nicht, sind beide im Allgemeinen mehr als fehleranfällig als die Verwendung einer Viewmodel.</span><span class="sxs-lookup"><span data-stu-id="2a07a-225">Since they don't offer compile-time type checking, both are generally more error-prone than using a viewmodel.</span></span> <span data-ttu-id="2a07a-226">Aus diesem Grund einige Entwickler bevorzugen Minimal oder nie `ViewData` und `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="2a07a-226">For that reason, some developers prefer to minimally or never use `ViewData` and `ViewBag`.</span></span>


<a name="VD"></a>

<span data-ttu-id="2a07a-227">**ViewData**</span><span class="sxs-lookup"><span data-stu-id="2a07a-227">**ViewData**</span></span>

<span data-ttu-id="2a07a-228">`ViewData`ist eine [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) Objekt erfolgt über `string` Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="2a07a-228">`ViewData` is a [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) object accessed through `string` keys.</span></span> <span data-ttu-id="2a07a-229">Zeichenfolgendaten können gespeichert und ohne die Notwendigkeit einer Typumwandlung direkt verwendet werden, jedoch müssen Sie eine Umwandlung andere `ViewData` Objektwerten auf bestimmte Typen, wenn Sie zu extrahieren.</span><span class="sxs-lookup"><span data-stu-id="2a07a-229">String data can be stored and used directly without the need for a cast, but you must cast other `ViewData` object values to specific types when you extract them.</span></span> <span data-ttu-id="2a07a-230">Sie können `ViewData` übergeben von Daten von Controller mit Ansichten und in Ansichten, einschließlich [Teilansichten](xref:mvc/views/partial) und [Layouts](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="2a07a-230">You can use `ViewData` to pass data from controllers to views and within views, including [partial views](xref:mvc/views/partial) and [layouts](xref:mvc/views/layout).</span></span>

<span data-ttu-id="2a07a-231">Im folgenden ist ein Beispiel, das Werte für einen Gruß und eine Adresse mit `ViewData` in einer Aktion:</span><span class="sxs-lookup"><span data-stu-id="2a07a-231">The following is an example that sets values for a greeting and an address using `ViewData` in an action:</span></span>

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

<span data-ttu-id="2a07a-232">Arbeiten Sie mit den Daten in einer Ansicht:</span><span class="sxs-lookup"><span data-stu-id="2a07a-232">Work with the data in a view:</span></span>

```cshtml
@{
    // Since Address isn't a string, it requires a cast.
    var address = ViewData["Address"] as Address;
}

@ViewData["Greeting"] World!

<address>
    @address.Name<br>
    @address.Street<br>
    @address.City, @address.State @address.PostalCode
</address>
```

<span data-ttu-id="2a07a-233">**ViewBag**</span><span class="sxs-lookup"><span data-stu-id="2a07a-233">**ViewBag**</span></span>

<span data-ttu-id="2a07a-234">Hinweis: `ViewBag` ist in der Razor-Seiten nicht verfügbar.</span><span class="sxs-lookup"><span data-stu-id="2a07a-234">Note: `ViewBag` is not available in the Razor Pages.</span></span>

<span data-ttu-id="2a07a-235">`ViewBag`ist eine [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) -Objekt, das dynamischen Zugriff auf die Objekte, die in gespeicherten `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="2a07a-235">`ViewBag` is a [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) object that provides dynamic access to the objects stored in `ViewData`.</span></span> <span data-ttu-id="2a07a-236">`ViewBag`ist besonders angenehm beim besser zu handhaben, da es keine Typumwandlung erforderlich.</span><span class="sxs-lookup"><span data-stu-id="2a07a-236">`ViewBag` can be more convenient to work with, since it doesn't require casting.</span></span> <span data-ttu-id="2a07a-237">Das folgende Beispiel zeigt, wie Sie `ViewBag` dasselbe Ergebnis erzielt als mit `ViewData` oben:</span><span class="sxs-lookup"><span data-stu-id="2a07a-237">The following example shows how to use `ViewBag` with the same result as using `ViewData` above:</span></span>

```csharp
public IActionResult SomeAction()
{
    ViewBag.Greeting = "Hello";
    ViewBag.Address  = new Address()
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

```cshtml
@ViewBag.Greeting World!

<address>
    @ViewBag.Address.Name<br>
    @ViewBag.Address.Street<br>
    @ViewBag.Address.City, @ViewBag.Address.State @ViewBag.Address.PostalCode
</address>
```

<span data-ttu-id="2a07a-238">**ViewData und ViewBag gleichzeitig zu verwenden**</span><span class="sxs-lookup"><span data-stu-id="2a07a-238">**Using ViewData and ViewBag simultaneously**</span></span>

<span data-ttu-id="2a07a-239">Hinweis: `ViewBag` ist in der Razor-Seiten nicht verfügbar.</span><span class="sxs-lookup"><span data-stu-id="2a07a-239">Note: `ViewBag` is not available in the Razor Pages.</span></span>

<span data-ttu-id="2a07a-240">Da `ViewData` und `ViewBag` finden Sie in den gleichen zugrunde liegende `ViewData` -Auflistung, können Sie beide `ViewData` und `ViewBag` mischen und zwischen ihnen beim Lesen und Schreiben von Werten entsprechen.</span><span class="sxs-lookup"><span data-stu-id="2a07a-240">Since `ViewData` and `ViewBag` refer to the same underlying `ViewData` collection, you can use both `ViewData` and `ViewBag` and mix and match between them when reading and writing values.</span></span>

<span data-ttu-id="2a07a-241">Legen Sie den Titel mit `ViewBag` und die Beschreibung mit `ViewData` am oberen Rand einer *About.cshtml* anzeigen:</span><span class="sxs-lookup"><span data-stu-id="2a07a-241">Set the title using `ViewBag` and the description using `ViewData` at the top of an *About.cshtml* view:</span></span>

```cshtml
@{
    Layout = "/Views/Shared/_Layout.cshtml";
    ViewBag.Title = "About Contoso";
    ViewData["Description"] = "Let us tell you about Contoso's philosophy and mission.";
}
```

<span data-ttu-id="2a07a-242">Lesen der Eigenschaften jedoch die Verwendung von reverse `ViewData` und `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="2a07a-242">Read the properties but reverse the use of `ViewData` and `ViewBag`.</span></span> <span data-ttu-id="2a07a-243">In der *_Layout.cshtml* Datei, rufen Sie den Titel mit `ViewData` und rufen Sie die Beschreibung mit `ViewBag`:</span><span class="sxs-lookup"><span data-stu-id="2a07a-243">In the *_Layout.cshtml* file, obtain the title using `ViewData` and obtain the description using `ViewBag`:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"]</title>
    <meta name="description" content="@ViewBag.Description">
    ...
```

<span data-ttu-id="2a07a-244">Beachten Sie, dass Zeichenfolgen eine Umwandlung für erfordern `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="2a07a-244">Remember that strings don't require a cast for `ViewData`.</span></span> <span data-ttu-id="2a07a-245">Sie können `@ViewData["Title"]` ohne Umwandlung.</span><span class="sxs-lookup"><span data-stu-id="2a07a-245">You can use `@ViewData["Title"]` without casting.</span></span>

<span data-ttu-id="2a07a-246">Mit beiden `ViewData` und `ViewBag` auf die gleiche Uhrzeit funktioniert wie mischen und anpassen, lesen und schreiben die Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="2a07a-246">Using both `ViewData` and `ViewBag` at the same time works, as does mixing and matching reading and writing the properties.</span></span> <span data-ttu-id="2a07a-247">Das folgende Markup gerendert wird:</span><span class="sxs-lookup"><span data-stu-id="2a07a-247">The following markup is rendered:</span></span>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>About Contoso</title>
    <meta name="description" content="Let us tell you about Contoso's philosophy and mission.">
    ...
```

<span data-ttu-id="2a07a-248">**Zusammenfassung der Unterschiede zwischen ViewData und ViewBag**</span><span class="sxs-lookup"><span data-stu-id="2a07a-248">**Summary of the differences between ViewData and ViewBag**</span></span>

 <span data-ttu-id="2a07a-249">`ViewBag`ist nicht verfügbar in der Razor-Seiten.</span><span class="sxs-lookup"><span data-stu-id="2a07a-249">`ViewBag` is not available in the Razor Pages.</span></span>

* `ViewData`
  * <span data-ttu-id="2a07a-250">Leitet sich von [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary), sodass er Wörterbucheigenschaften verfügt, die hilfreich sein, z. B. `ContainsKey`, `Add`, `Remove`, und `Clear`.</span><span class="sxs-lookup"><span data-stu-id="2a07a-250">Derives from [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary), so it has dictionary properties that can be useful, such as `ContainsKey`, `Add`, `Remove`, and `Clear`.</span></span>
  * <span data-ttu-id="2a07a-251">Schlüssel im Wörterbuch sind Zeichenfolgen, daher Leerzeichen zulässig ist.</span><span class="sxs-lookup"><span data-stu-id="2a07a-251">Keys in the dictionary are strings, so whitespace is allowed.</span></span> <span data-ttu-id="2a07a-252">Ein Beispiel: `ViewData["Some Key With Whitespace"]`</span><span class="sxs-lookup"><span data-stu-id="2a07a-252">Example: `ViewData["Some Key With Whitespace"]`</span></span>
  * <span data-ttu-id="2a07a-253">Beliebiger Typ außer einem `string` umgewandelt werden muss, in der Ansicht mit `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="2a07a-253">Any type other than a `string` must be cast in the view to use `ViewData`.</span></span>
* `ViewBag`
  * <span data-ttu-id="2a07a-254">Leitet sich von [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata), daher lässt die Erstellung von dynamischen Eigenschaften, die Verwendung der Punktnotation (`@ViewBag.SomeKey = <value or object>`), und es ist keine Typumwandlung erforderlich.</span><span class="sxs-lookup"><span data-stu-id="2a07a-254">Derives from [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata), so it allows the creation of dynamic properties using dot notation (`@ViewBag.SomeKey = <value or object>`), and no casting is required.</span></span> <span data-ttu-id="2a07a-255">Die Syntax des `ViewBag` ist es schneller, Controller und Ansichten hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="2a07a-255">The syntax of `ViewBag` makes it quicker to add to controllers and views.</span></span>
  * <span data-ttu-id="2a07a-256">Einfacher, die null-Werte zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="2a07a-256">Simpler to check for null values.</span></span> <span data-ttu-id="2a07a-257">Ein Beispiel: `@ViewBag.Person?.Name`</span><span class="sxs-lookup"><span data-stu-id="2a07a-257">Example: `@ViewBag.Person?.Name`</span></span>

<span data-ttu-id="2a07a-258">**Wann ViewData oder ViewBag verwenden**</span><span class="sxs-lookup"><span data-stu-id="2a07a-258">**When to use ViewData or ViewBag**</span></span>

<span data-ttu-id="2a07a-259">Beide `ViewData` und `ViewBag` sind gleichermaßen gültig Ansätze für kleine Mengen von Daten zwischen Controllern und Ansichten übergeben.</span><span class="sxs-lookup"><span data-stu-id="2a07a-259">Both `ViewData` and `ViewBag` are equally valid approaches for passing small amounts of data among controllers and views.</span></span> <span data-ttu-id="2a07a-260">Die Wahl des welcher Typ verwendet basiert auf bevorzugt.</span><span class="sxs-lookup"><span data-stu-id="2a07a-260">The choice of which one to use is based on preference.</span></span> <span data-ttu-id="2a07a-261">Mischen und zuordnen `ViewData` und `ViewBag` Objekte, die Code ist jedoch einfacher zu lesen und zu verwalten, mit der ein Ansatz besteht darin, die konsistent verwendet.</span><span class="sxs-lookup"><span data-stu-id="2a07a-261">You can mix and match `ViewData` and `ViewBag` objects, however, the code is easier to read and maintain with one approach used consistently.</span></span> <span data-ttu-id="2a07a-262">Beide Vorgehensweisen sind dynamisch zur Laufzeit aufgelöst und anfällig für Laufzeitfehler verursachen.</span><span class="sxs-lookup"><span data-stu-id="2a07a-262">Both approaches are dynamically resolved at runtime and thus prone to causing runtime errors.</span></span> <span data-ttu-id="2a07a-263">Einige Entwicklungsteams vermeiden.</span><span class="sxs-lookup"><span data-stu-id="2a07a-263">Some development teams avoid them.</span></span>

### <a name="dynamic-views"></a><span data-ttu-id="2a07a-264">Dynamische Ansichten</span><span class="sxs-lookup"><span data-stu-id="2a07a-264">Dynamic views</span></span>

<span data-ttu-id="2a07a-265">Geben Sie Sichten, die ein Modell zu deklarieren, nicht mit `@model` jedoch eine Modellinstanz übergebenen aufweisen (z. B. `return View(Address);`) können die Eigenschaften der Instanz dynamisch verweisen:</span><span class="sxs-lookup"><span data-stu-id="2a07a-265">Views that don't declare a model type using `@model` but that have a model instance passed to them (for example, `return View(Address);`) can reference the instance's properties dynamically:</span></span>

```cshtml
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

<span data-ttu-id="2a07a-266">Diese Funktion bietet Flexibilität, aber es bietet keine Kompilierung Schutz noch IntelliSense zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="2a07a-266">This feature offers flexibility but doesn't offer compilation protection or IntelliSense.</span></span> <span data-ttu-id="2a07a-267">Wenn die Eigenschaft nicht vorhanden ist, schlägt die Webseite Generation zur Laufzeit fehl.</span><span class="sxs-lookup"><span data-stu-id="2a07a-267">If the property doesn't exist, webpage generation fails at runtime.</span></span>

## <a name="more-view-features"></a><span data-ttu-id="2a07a-268">Weitere Funktionen</span><span class="sxs-lookup"><span data-stu-id="2a07a-268">More view features</span></span>

<span data-ttu-id="2a07a-269">[Kennzeichnen von Hilfsprogrammen](xref:mvc/views/tag-helpers/intro) erleichtern das serverseitige Verhalten vorhandenen HTML-Tags hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="2a07a-269">[Tag Helpers](xref:mvc/views/tag-helpers/intro) make it easy to add server-side behavior to existing HTML tags.</span></span> <span data-ttu-id="2a07a-270">Verwenden von Tag-Hilfsprogrammen vermeidet die Notwendigkeit zum Schreiben von benutzerdefiniertem Code oder Hilfsprogramme in Ihren Ansichten.</span><span class="sxs-lookup"><span data-stu-id="2a07a-270">Using Tag Helpers avoids the need to write custom code or helpers within your views.</span></span> <span data-ttu-id="2a07a-271">Tag-Hilfsprogrammen, die als Attribute auf HTML-Elemente angewendet werden und von Editoren, die sie nicht verarbeiten können ignoriert werden.</span><span class="sxs-lookup"><span data-stu-id="2a07a-271">Tag helpers are applied as attributes to HTML elements and are ignored by editors that can't process them.</span></span> <span data-ttu-id="2a07a-272">Dadurch können Sie zum Bearbeiten und Rendern von Markup der Sicht in einer Vielzahl von Tools.</span><span class="sxs-lookup"><span data-stu-id="2a07a-272">This allows you to edit and render view markup in a variety of tools.</span></span>

<span data-ttu-id="2a07a-273">Generieren von benutzerdefinierten HTML-Markup kann mit vielen integrierten HTML-Hilfsmethoden erreicht werden.</span><span class="sxs-lookup"><span data-stu-id="2a07a-273">Generating custom HTML markup can be achieved with many built-in HTML Helpers.</span></span> <span data-ttu-id="2a07a-274">Komplexere Benutzeroberflächenlogik kann behandelt werden, indem [Ansichtskomponenten](xref:mvc/views/view-components).</span><span class="sxs-lookup"><span data-stu-id="2a07a-274">More complex user interface logic can be handled by [View Components](xref:mvc/views/view-components).</span></span> <span data-ttu-id="2a07a-275">Ansichtskomponenten finden Sie die gleichen SoC, Controller und Ansichten bieten.</span><span class="sxs-lookup"><span data-stu-id="2a07a-275">View components provide the same SoC that controllers and views offer.</span></span> <span data-ttu-id="2a07a-276">Sie können angeben, entfällt der Bedarf für Aktionen und Ansichten, die mit Daten, die durch allgemeine Elemente der Benutzeroberfläche verwendet.</span><span class="sxs-lookup"><span data-stu-id="2a07a-276">They can eliminate the need for actions and views that deal with data used by common user interface elements.</span></span>

<span data-ttu-id="2a07a-277">Wie viele andere Aspekte von ASP.NET Core Ansichten unterstützen [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection), sodass Dienste sind [in Sichten eingefügt](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="2a07a-277">Like many other aspects of ASP.NET Core, views support [dependency injection](xref:fundamentals/dependency-injection), allowing services to be [injected into views](xref:mvc/views/dependency-injection).</span></span>
