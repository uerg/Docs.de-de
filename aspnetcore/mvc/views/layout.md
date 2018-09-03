---
title: Layout in ASP.NET Core
author: ardalis
description: Erfahren Sie, wie man gängige Layouts verwendet, Anweisungen von mehreren Ansichten gemeinsam nutzen lässt und Programmcode vor dem Rendern der Ansichten in einer ASP.NET Core-App ausführt.
ms.author: riande
ms.date: 10/14/2016
uid: mvc/views/layout
ms.openlocfilehash: ad0b339572f387be8a636204015ffc361947acb8
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751570"
---
# <a name="layout-in-aspnet-core"></a><span data-ttu-id="c0407-103">Layout in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c0407-103">Layout in ASP.NET Core</span></span>

<span data-ttu-id="c0407-104">Von [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="c0407-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="c0407-105">Ansichten beinhalten häufig sowohl visuelle als auch programmgesteuerte Elemente.</span><span class="sxs-lookup"><span data-stu-id="c0407-105">Views frequently share visual and programmatic elements.</span></span> <span data-ttu-id="c0407-106">In diesem Artikel erfahren Sie, wie man gängige Layouts verwendet, Anweisungen von mehreren Ansichten gemeinsam nutzen lässt und man Programmcode vor dem Rendern der Ansichten in der ASP.NET Core-App ausführt.</span><span class="sxs-lookup"><span data-stu-id="c0407-106">In this article, you'll learn how to use common layouts, share directives, and run common code before rendering views in your ASP.NET Core app.</span></span>

## <a name="what-is-a-layout"></a><span data-ttu-id="c0407-107">Was ist ein Layout?</span><span class="sxs-lookup"><span data-stu-id="c0407-107">What is a Layout</span></span>

<span data-ttu-id="c0407-108">Die meisten Web-Apps haben ein gebräuchliches Layout, das dem Benutzer beim Navigieren auf den Seiten ein konsistentes Verhalten bietet.</span><span class="sxs-lookup"><span data-stu-id="c0407-108">Most web apps have a common layout that provides the user with a consistent experience as they navigate from page to page.</span></span> <span data-ttu-id="c0407-109">Das Layout enthält normalerweise allgemeine Benutzeroberflächenelemente wie App-Header, Navigations- oder Menüelemente sowie eine Fußzeile.</span><span class="sxs-lookup"><span data-stu-id="c0407-109">The layout typically includes common user interface elements such as the app header, navigation or menu elements, and footer.</span></span>

![Beispiel für ein Seitenlayout](layout/_static/page-layout.png)

<span data-ttu-id="c0407-111">Gängige HTML-Strukturen wie Skripts und Stylesheets werden häufig von vielen Seiten in einer App verwendet.</span><span class="sxs-lookup"><span data-stu-id="c0407-111">Common HTML structures such as scripts and stylesheets are also frequently used by many pages within an app.</span></span> <span data-ttu-id="c0407-112">Alle diese gemeinsamen Elemente werden in einer *Layoutdatei* definiert, auf die dann von jeder Ansicht einer App verwiesen werden kann.</span><span class="sxs-lookup"><span data-stu-id="c0407-112">All of these shared elements may be defined in a *layout* file, which can then be referenced by any view used within the app.</span></span> <span data-ttu-id="c0407-113">Layouts minimieren Codeduplikate in Ansichten, sodass das [DRY-Prinzip (Don‘t Repeat Yourself)](http://deviq.com/don-t-repeat-yourself/) eingehalten wird.</span><span class="sxs-lookup"><span data-stu-id="c0407-113">Layouts reduce duplicate code in views, helping them follow the [Don't Repeat Yourself (DRY) principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="c0407-114">Gemäß Konvention ist `_Layout.cshtml` das Standardlayout für eine ASP.NET Core-App.</span><span class="sxs-lookup"><span data-stu-id="c0407-114">By convention, the default layout for an ASP.NET Core app is named `_Layout.cshtml`.</span></span> <span data-ttu-id="c0407-115">Die Visual Studio ASP.NET Core MVC-Projektvorlage enthält diese Layoutdatei im Ordner `Views/Shared`:</span><span class="sxs-lookup"><span data-stu-id="c0407-115">The Visual Studio ASP.NET Core MVC project template includes this layout file in the `Views/Shared` folder:</span></span>

![Ordner „Views“ (Ansichten) im Projektmappen-Explorer](layout/_static/web-project-views.png)

<span data-ttu-id="c0407-117">Dieses Layout definiert eine übergeordnete Vorlage für die Ansichten einer App.</span><span class="sxs-lookup"><span data-stu-id="c0407-117">This layout defines a top level template for views in the app.</span></span> <span data-ttu-id="c0407-118">Apps erfordern nicht zwingend ein Layout, können aber auch mehrere Layouts definieren. Die Ansichten der App können dann unterschiedliche Layouts nutzen.</span><span class="sxs-lookup"><span data-stu-id="c0407-118">Apps don't require a layout, and apps can define more than one layout, with different views specifying different layouts.</span></span>

<span data-ttu-id="c0407-119">Ein Beispiel-`_Layout.cshtml`:</span><span class="sxs-lookup"><span data-stu-id="c0407-119">An example `_Layout.cshtml`:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a><span data-ttu-id="c0407-120">Festlegen eines Layouts</span><span class="sxs-lookup"><span data-stu-id="c0407-120">Specifying a Layout</span></span>

<span data-ttu-id="c0407-121">Razor-Ansichten verfügen über eine `Layout` -Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="c0407-121">Razor views have a `Layout` property.</span></span> <span data-ttu-id="c0407-122">Durch Festlegen dieser Eigenschaft wird das Layout der jeweiligen Ansicht bestimmt:</span><span class="sxs-lookup"><span data-stu-id="c0407-122">Individual views specify a layout by setting this property:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

<span data-ttu-id="c0407-123">Das Layout kann mit seinem vollständigen Pfad (Beispiel: `/Views/Shared/_Layout.cshtml`) oder über einen Teil seines Namens angegeben werden (Beispiel: `_Layout`).</span><span class="sxs-lookup"><span data-stu-id="c0407-123">The layout specified can use a full path (example: `/Views/Shared/_Layout.cshtml`) or a partial name (example: `_Layout`).</span></span> <span data-ttu-id="c0407-124">Wird ein Teil des Namens angegeben, dann durchsucht die Razor-Ansichts-Engine die Layoutdatei unter Verwendung des standardmäßigen Ermittlungsprozesses.</span><span class="sxs-lookup"><span data-stu-id="c0407-124">When a partial name is provided, the Razor view engine will search for the layout file using its standard discovery process.</span></span> <span data-ttu-id="c0407-125">Zuerst wird der dem Controller zugeordnete Ordner durchsucht, gefolgt vom Ordner `Shared`.</span><span class="sxs-lookup"><span data-stu-id="c0407-125">The controller-associated folder is searched first, followed by the `Shared` folder.</span></span> <span data-ttu-id="c0407-126">Dieser Ermittlungsprozess ist identisch mit dem Prozess zum Auffinden von [Teilansichten](partial.md).</span><span class="sxs-lookup"><span data-stu-id="c0407-126">This discovery process is identical to the one used to discover [partial views](partial.md).</span></span>

<span data-ttu-id="c0407-127">Standardmäßig muss jedes Layout `RenderBody` aufrufen.</span><span class="sxs-lookup"><span data-stu-id="c0407-127">By default, every layout must call `RenderBody`.</span></span> <span data-ttu-id="c0407-128">Wo immer der Aufruf von `RenderBody` platziert ist, wird der Inhalt der Ansicht gerendert.</span><span class="sxs-lookup"><span data-stu-id="c0407-128">Wherever the call to `RenderBody` is placed, the contents of the view will be rendered.</span></span>

<a name="layout-sections-label"></a>

### <a name="sections"></a><span data-ttu-id="c0407-129">Abschnitte</span><span class="sxs-lookup"><span data-stu-id="c0407-129">Sections</span></span>

<span data-ttu-id="c0407-130">Optional kann ein Layout auf mindestens einen *Abschnitt* verweisen, indem es `RenderSection` aufruft.</span><span class="sxs-lookup"><span data-stu-id="c0407-130">A layout can optionally reference one or more *sections*, by calling `RenderSection`.</span></span> <span data-ttu-id="c0407-131">Abschnitte geben an, wo bestimmte Seitenelemente platziert werden sollen.</span><span class="sxs-lookup"><span data-stu-id="c0407-131">Sections provide a way to organize where certain page elements should be placed.</span></span> <span data-ttu-id="c0407-132">Jeder Aufruf von `RenderSection` kann angeben, ob dieser Abschnitt erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="c0407-132">Each call to `RenderSection` can specify whether that section is required or optional.</span></span> <span data-ttu-id="c0407-133">Wenn ein erforderlicher Bereich nicht gefunden werden kann, wird eine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="c0407-133">If a required section isn't found, an exception will be thrown.</span></span> <span data-ttu-id="c0407-134">Einzelne Ansichten verwenden die Razor-Syntax `@section`, um den in einem Abschnitt zu rendernden Inhalt anzugeben.</span><span class="sxs-lookup"><span data-stu-id="c0407-134">Individual views specify the content to be rendered within a section using the `@section` Razor syntax.</span></span> <span data-ttu-id="c0407-135">Wenn eine Ansicht einen Abschnitt definiert, muss dieser auch gerendert werden. Andernfalls wird eine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="c0407-135">If a view defines a section, it must be rendered (or an error will occur).</span></span>

<span data-ttu-id="c0407-136">Eine Beispieldefinition von `@section` in einer Ansicht:</span><span class="sxs-lookup"><span data-stu-id="c0407-136">An example `@section` definition in a view:</span></span>

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

<span data-ttu-id="c0407-137">Im oben stehenden Code werden Validierungsskripts zum Abschnitt `scripts` in einer Ansicht mit einem Formular hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="c0407-137">In the code above, validation scripts are added to the `scripts` section on a view that includes a form.</span></span> <span data-ttu-id="c0407-138">Es ist möglich, dass andere Ansichten in dieser Anwendung keine zusätzlichen Skripts erfordern, sodass auch kein Skriptbereich definiert werden muss.</span><span class="sxs-lookup"><span data-stu-id="c0407-138">Other views in the same application might not require any additional scripts, and so wouldn't need to define a scripts section.</span></span>

<span data-ttu-id="c0407-139">Die Abschnitte, die in einer Ansicht definiert wurden, stehen nur auf deren Layoutseite zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="c0407-139">Sections defined in a view are available only in its immediate layout page.</span></span> <span data-ttu-id="c0407-140">Teilansichten, Ansichtskomponenten und andere Teile eines Ansichtssystems können nicht auf sie verweisen.</span><span class="sxs-lookup"><span data-stu-id="c0407-140">They cannot be referenced from partials, view components, or other parts of the view system.</span></span>

### <a name="ignoring-sections"></a><span data-ttu-id="c0407-141">Ignorieren von Abschnitten</span><span class="sxs-lookup"><span data-stu-id="c0407-141">Ignoring sections</span></span>

<span data-ttu-id="c0407-142">Standardmäßig müssen der Text und die Abschnitte einer Inhaltsseite alle von der Layoutseite gerendert werden.</span><span class="sxs-lookup"><span data-stu-id="c0407-142">By default, the body and all sections in a content page must all be rendered by the layout page.</span></span> <span data-ttu-id="c0407-143">Die Razor-Ansichtsengine erzwingt dies, indem sie erfasst, ob der Text und jeder Abschnitt gerendert wurde.</span><span class="sxs-lookup"><span data-stu-id="c0407-143">The Razor view engine enforces this by tracking whether the body and each section have been rendered.</span></span>

<span data-ttu-id="c0407-144">Rufen Sie die Methoden `IgnoreBody` und `IgnoreSection` auf, um die Ansichtsengine anzuweisen, den Text oder die Abschnitte zu ignorieren.</span><span class="sxs-lookup"><span data-stu-id="c0407-144">To instruct the view engine to ignore the body or sections, call the `IgnoreBody` and `IgnoreSection` methods.</span></span>

<span data-ttu-id="c0407-145">Der Text und jeder Abschnitt einer Razor Page müssen entweder gerendert oder ignoriert werden.</span><span class="sxs-lookup"><span data-stu-id="c0407-145">The body and every section in a Razor page must be either rendered or ignored.</span></span>

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a><span data-ttu-id="c0407-146">Importieren gemeinsam verwendeter Anweisungen</span><span class="sxs-lookup"><span data-stu-id="c0407-146">Importing Shared Directives</span></span>

<span data-ttu-id="c0407-147">Ansichten können Razor-Anweisungen verwenden, um verschiedene Aktionen durchzuführen, wie z.B. das Importieren von Namespaces oder das durchführen von [Dependency Injection](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="c0407-147">Views can use Razor directives to do many things, such as importing namespaces or performing [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="c0407-148">Anweisungen, die von mehreren Ansichten gemeinsam verwendet werden, können in einer gemeinsam verwendeten `_ViewImports.cshtml`-Datei angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="c0407-148">Directives shared by many views may be specified in a common `_ViewImports.cshtml` file.</span></span> <span data-ttu-id="c0407-149">Die `_ViewImports`-Datei unterstützt die folgenden Anweisungen:</span><span class="sxs-lookup"><span data-stu-id="c0407-149">The `_ViewImports` file supports the following directives:</span></span>

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

<span data-ttu-id="c0407-150">Die Datei unterstützt keine anderen Razor-Features wie Funktionen und Abschnittdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="c0407-150">The file doesn't support other Razor features, such as functions and section definitions.</span></span>

<span data-ttu-id="c0407-151">Eine `_ViewImports.cshtml`-Beispieldatei:</span><span class="sxs-lookup"><span data-stu-id="c0407-151">A sample `_ViewImports.cshtml` file:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

<span data-ttu-id="c0407-152">Die `_ViewImports.cshtml`-Datei für eine ASP.NET Core MVC-App befindet sich normalerweise im `Views`-Ordner.</span><span class="sxs-lookup"><span data-stu-id="c0407-152">The `_ViewImports.cshtml` file for an ASP.NET Core MVC app is typically placed in the `Views` folder.</span></span> <span data-ttu-id="c0407-153">Eine `_ViewImports.cshtml`-Datei kann auch in einen anderen Ordner verschoben werden. In diesem Fall wird sie nur auf die Ansichten in diesem Ordner und in dessen Unterordnern angewendet.</span><span class="sxs-lookup"><span data-stu-id="c0407-153">A `_ViewImports.cshtml` file can be placed within any folder, in which case it will only be applied to views within that folder and its subfolders.</span></span> <span data-ttu-id="c0407-154">`_ViewImports`-Dateien werden von der Stammebene aus verarbeitet. Dann wird jeder Ordner verarbeitet, der sich auf dem Weg zur Ansicht befindet. Das bedeutet, dass Einstellungen der Stammebene auf der Ordnerebene außer Kraft gesetzt werden.</span><span class="sxs-lookup"><span data-stu-id="c0407-154">`_ViewImports` files are processed starting at the root level, and then for each folder leading up to the location of the view itself, so settings specified at the root level may be overridden at the folder level.</span></span>

<span data-ttu-id="c0407-155">Wenn z.B. eine `_ViewImports.cshtml`-Datei auf Stammebene `@model` und `@addTagHelper` angibt, und eine andere `_ViewImports.cshtml`-Datei in einer Ansicht des Ordners, der mit einem Controller verknüpft ist, ein anderes `@model` angibt und einen anderen `@addTagHelper` hinzufügt, hat die Ansicht Zugriff auf beide Taghilfsprogramme und verwendet letzteres `@model`.</span><span class="sxs-lookup"><span data-stu-id="c0407-155">For example, if a root level `_ViewImports.cshtml` file specifies `@model` and `@addTagHelper`, and another `_ViewImports.cshtml` file in the controller-associated folder of the view specifies a different `@model` and adds another `@addTagHelper`, the view will have access to both tag helpers and will use the latter `@model`.</span></span>

<span data-ttu-id="c0407-156">Wenn mehrere `_ViewImports.cshtml`-Dateien für eine Ansicht ausgeführt werden, sieht das Verhalten der Anweisungen, die sich in den `ViewImports.cshtml`-Dateien befinden, wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="c0407-156">If multiple `_ViewImports.cshtml` files are run for a view, combined behavior of the directives included in the `ViewImports.cshtml` files will be as follows:</span></span>

* <span data-ttu-id="c0407-157">`@addTagHelper`, `@removeTagHelper`: werden nach der Reihe ausgeführt</span><span class="sxs-lookup"><span data-stu-id="c0407-157">`@addTagHelper`, `@removeTagHelper`: all run, in order</span></span>

* <span data-ttu-id="c0407-158">`@tagHelperPrefix`: dasjenige, das der Ansicht am Nächsten ist, setzt die anderen außer Kraft</span><span class="sxs-lookup"><span data-stu-id="c0407-158">`@tagHelperPrefix`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="c0407-159">`@model`: dasjenige, das der Ansicht am Nächsten ist, setzt die anderen außer Kraft</span><span class="sxs-lookup"><span data-stu-id="c0407-159">`@model`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="c0407-160">`@inherits`: dasjenige, das der Ansicht am Nächsten ist, setzt die anderen außer Kraft</span><span class="sxs-lookup"><span data-stu-id="c0407-160">`@inherits`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="c0407-161">`@using`: alle einbezogen; Duplikate werden ignoriert</span><span class="sxs-lookup"><span data-stu-id="c0407-161">`@using`: all are included; duplicates are ignored</span></span>

* <span data-ttu-id="c0407-162">`@inject`: dasjenige, das der Ansicht am Nächsten ist, setzt für jede Eigenschaft alle anderen mit dem gleichen Namen außer Kraft</span><span class="sxs-lookup"><span data-stu-id="c0407-162">`@inject`: for each property, the closest one to the view overrides any others with the same property name</span></span>

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a><span data-ttu-id="c0407-163">Ausführen von Code vor jeder Ansicht</span><span class="sxs-lookup"><span data-stu-id="c0407-163">Running Code Before Each View</span></span>

<span data-ttu-id="c0407-164">Wenn es Code gibt, der vor jeder Ansicht ausgeführt werden muss, sollte dieser in die `_ViewStart.cshtml`-Datei platziert werden.</span><span class="sxs-lookup"><span data-stu-id="c0407-164">If you have code you need to run before every view, this should be placed in the `_ViewStart.cshtml` file.</span></span> <span data-ttu-id="c0407-165">Per Konvention befindet sich die `_ViewStart.cshtml`-Datei im `Views`-Ordner.</span><span class="sxs-lookup"><span data-stu-id="c0407-165">By convention, the `_ViewStart.cshtml` file is located in the `Views` folder.</span></span> <span data-ttu-id="c0407-166">Die in `_ViewStart.cshtml` aufgelisteten Anweisungen werden vor jeder vollständigen Ansicht (also keine Layouts und keine Teilansichten) ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="c0407-166">The statements listed in `_ViewStart.cshtml` are run before every full view (not layouts, and not partial views).</span></span> <span data-ttu-id="c0407-167">`_ViewStart.cshtml` ist genauso wie [ViewImports.cshtml](xref:mvc/views/layout#viewimports) hierarchisch.</span><span class="sxs-lookup"><span data-stu-id="c0407-167">Like [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` is hierarchical.</span></span> <span data-ttu-id="c0407-168">Wenn eine `_ViewStart.cshtml`-Datei im Ansichtsordner, der mit einem Controller verknüpft ist, definiert wird, wird sie nach derjenigen ausgeführt, die im Stamm des `Views`-Ordners definiert wurde (falls vorhanden).</span><span class="sxs-lookup"><span data-stu-id="c0407-168">If a `_ViewStart.cshtml` file is defined in the controller-associated view folder, it will be run after the one defined in the root of the `Views` folder (if any).</span></span>

<span data-ttu-id="c0407-169">Eine `_ViewStart.cshtml`-Beispieldatei:</span><span class="sxs-lookup"><span data-stu-id="c0407-169">A sample `_ViewStart.cshtml` file:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

<span data-ttu-id="c0407-170">Die oben stehende Datei gibt an, dass alle Ansichten das `_Layout.cshtml`-Layout verwenden.</span><span class="sxs-lookup"><span data-stu-id="c0407-170">The file above specifies that all views will use the `_Layout.cshtml` layout.</span></span>

> [!NOTE]
> <span data-ttu-id="c0407-171">Weder `_ViewStart.cshtml` noch `_ViewImports.cshtml` befinden sich normalerweise im `/Views/Shared`-Ordner.</span><span class="sxs-lookup"><span data-stu-id="c0407-171">Neither `_ViewStart.cshtml` nor `_ViewImports.cshtml` are typically placed in the `/Views/Shared` folder.</span></span> <span data-ttu-id="c0407-172">Die Versionen dieser Dateien auf Anwendungsebene sollten direkt in den `/Views`-Ordner platziert werden.</span><span class="sxs-lookup"><span data-stu-id="c0407-172">The app-level versions of these files should be placed directly in the `/Views` folder.</span></span>
