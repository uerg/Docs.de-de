---
title: Layout
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/layout
ms.openlocfilehash: e268f045e39188e9cc1e759ff7e6c553662dd669
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
# <a name="layout"></a><span data-ttu-id="f96f3-102">Layout</span><span class="sxs-lookup"><span data-stu-id="f96f3-102">Layout</span></span>

<span data-ttu-id="f96f3-103">Durch [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="f96f3-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="f96f3-104">Ansichten verwenden häufig visuelle und programmgesteuerte Elemente.</span><span class="sxs-lookup"><span data-stu-id="f96f3-104">Views frequently share visual and programmatic elements.</span></span> <span data-ttu-id="f96f3-105">In diesem Artikel erfahren Sie, wie häufige Layouts verwenden, Teilen Direktiven und allgemeine Codes vor dem Rendern von Ansichten in der ASP.NET app ausführen.</span><span class="sxs-lookup"><span data-stu-id="f96f3-105">In this article, you'll learn how to use common layouts, share directives, and run common code before rendering views in your ASP.NET app.</span></span>

## <a name="what-is-a-layout"></a><span data-ttu-id="f96f3-106">Was ist ein Layout</span><span class="sxs-lookup"><span data-stu-id="f96f3-106">What is a Layout</span></span>

<span data-ttu-id="f96f3-107">Die meisten Web-apps haben ein gebräuchliches Layout, das den Benutzer ein konsistentes Verhalten bietet, wie sie von einer Seite zu navigieren.</span><span class="sxs-lookup"><span data-stu-id="f96f3-107">Most web apps have a common layout that provides the user with a consistent experience as they navigate from page to page.</span></span> <span data-ttu-id="f96f3-108">Das Layout enthält i. d. r. allgemeine Benutzeroberflächenelemente wie das app-Header, Navigation oder im Menüelemente und Fußzeile.</span><span class="sxs-lookup"><span data-stu-id="f96f3-108">The layout typically includes common user interface elements such as the app header, navigation or menu elements, and footer.</span></span>

![Seitenlayout-Beispiel](layout/_static/page-layout.png)

<span data-ttu-id="f96f3-110">Allgemeine HTML-Strukturen, z. B. Skripts und Stylesheets werden viele Seiten in einer app auch häufig verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="f96f3-110">Common HTML structures such as scripts and stylesheets are also frequently used by many pages within an app.</span></span> <span data-ttu-id="f96f3-111">Alle diese freigegebene Elemente können definiert werden, einem *Layout* -Datei, die dann von einer beliebigen Ansicht innerhalb der app verwendet verwiesen werden kann.</span><span class="sxs-lookup"><span data-stu-id="f96f3-111">All of these shared elements may be defined in a *layout* file, which can then be referenced by any view used within the app.</span></span> <span data-ttu-id="f96f3-112">Layouts reduzieren doppelte Code in Sichten, sodass führen Sie die [Don't wiederholen selbst (trocken)-Prinzip](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="f96f3-112">Layouts reduce duplicate code in views, helping them follow the [Don't Repeat Yourself (DRY) principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="f96f3-113">Gemäß der Konvention lautet das Standardlayout für eine ASP.NET app `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="f96f3-113">By convention, the default layout for an ASP.NET app is named `_Layout.cshtml`.</span></span> <span data-ttu-id="f96f3-114">Die Visual Studio ASP.NET Core MVC-Projektvorlage enthält diese Layoutdatei in die `Views/Shared` Ordner:</span><span class="sxs-lookup"><span data-stu-id="f96f3-114">The Visual Studio ASP.NET Core MVC project template includes this layout file in the `Views/Shared` folder:</span></span>

![Ansichtenordner im Projektmappen-explorer](layout/_static/web-project-views.png)

<span data-ttu-id="f96f3-116">Dieses Layout definiert eine Vorlage der obersten Ebene für Sichten in der app.</span><span class="sxs-lookup"><span data-stu-id="f96f3-116">This layout defines a top level template for views in the app.</span></span> <span data-ttu-id="f96f3-117">Apps erfordern kein Layout und apps können mehr als ein Layout definieren, mit anderen Ansichten, die unterschiedliche Layouts angeben.</span><span class="sxs-lookup"><span data-stu-id="f96f3-117">Apps don't require a layout, and apps can define more than one layout, with different views specifying different layouts.</span></span>

<span data-ttu-id="f96f3-118">Ein Beispiel für `_Layout.cshtml`:</span><span class="sxs-lookup"><span data-stu-id="f96f3-118">An example `_Layout.cshtml`:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a><span data-ttu-id="f96f3-119">Festlegen eines Layouts</span><span class="sxs-lookup"><span data-stu-id="f96f3-119">Specifying a Layout</span></span>

<span data-ttu-id="f96f3-120">Razor-Ansichten verfügen über eine `Layout` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="f96f3-120">Razor views have a `Layout` property.</span></span> <span data-ttu-id="f96f3-121">Einzelne Ansichten Geben Sie ein Layout durch Festlegen dieser Eigenschaft:</span><span class="sxs-lookup"><span data-stu-id="f96f3-121">Individual views specify a layout by setting this property:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

<span data-ttu-id="f96f3-122">Das angegebene Layout können einen vollständigen Pfad (Beispiel: `/Views/Shared/_Layout.cshtml`) oder einen Teilnamen (Beispiel: `_Layout`).</span><span class="sxs-lookup"><span data-stu-id="f96f3-122">The layout specified can use a full path (example: `/Views/Shared/_Layout.cshtml`) or a partial name (example: `_Layout`).</span></span> <span data-ttu-id="f96f3-123">Wenn ein partielle Name angegeben ist, werden das Razor-Ansichtsmodul für die Layoutdatei mithilfe der standardmäßigen Ermittlungsprozess durchsucht.</span><span class="sxs-lookup"><span data-stu-id="f96f3-123">When a partial name is provided, the Razor view engine will search for the layout file using its standard discovery process.</span></span> <span data-ttu-id="f96f3-124">Der Controller zugeordnete Ordner durchsucht zuerst, gefolgt von den `Shared` Ordner.</span><span class="sxs-lookup"><span data-stu-id="f96f3-124">The controller-associated folder is searched first, followed by the `Shared` folder.</span></span> <span data-ttu-id="f96f3-125">Dieser Discovery-Prozess ist identisch mit dem Kennwort verwendet, um ermitteln [Teilansichten](partial.md).</span><span class="sxs-lookup"><span data-stu-id="f96f3-125">This discovery process is identical to the one used to discover [partial views](partial.md).</span></span>

<span data-ttu-id="f96f3-126">Standardmäßig muss jedes Layout Aufrufen `RenderBody`.</span><span class="sxs-lookup"><span data-stu-id="f96f3-126">By default, every layout must call `RenderBody`.</span></span> <span data-ttu-id="f96f3-127">Immer, wenn der Aufruf von `RenderBody` ist platziert werden, wird der Inhalt der Ansicht gerendert.</span><span class="sxs-lookup"><span data-stu-id="f96f3-127">Wherever the call to `RenderBody` is placed, the contents of the view will be rendered.</span></span>

<a name="layout-sections-label"></a>

### <a name="sections"></a><span data-ttu-id="f96f3-128">Abschnitte</span><span class="sxs-lookup"><span data-stu-id="f96f3-128">Sections</span></span>

<span data-ttu-id="f96f3-129">Ein Layout kann optional eine oder mehrere verweisen *Abschnitte*, durch den Aufruf `RenderSection`.</span><span class="sxs-lookup"><span data-stu-id="f96f3-129">A layout can optionally reference one or more *sections*, by calling `RenderSection`.</span></span> <span data-ttu-id="f96f3-130">Abschnitte bieten eine Möglichkeit zum Organisieren, wo bestimmte Seitenelemente platziert werden soll.</span><span class="sxs-lookup"><span data-stu-id="f96f3-130">Sections provide a way to organize where certain page elements should be placed.</span></span> <span data-ttu-id="f96f3-131">Jeder Aufruf von `RenderSection` können angeben, ob dieses Abschnitts erforderlich oder optional ist.</span><span class="sxs-lookup"><span data-stu-id="f96f3-131">Each call to `RenderSection` can specify whether that section is required or optional.</span></span> <span data-ttu-id="f96f3-132">Wenn ein erforderliche Abschnitt nicht gefunden wird, wird eine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="f96f3-132">If a required section isn't found, an exception will be thrown.</span></span> <span data-ttu-id="f96f3-133">Einzelne Ansichten Geben Sie den Inhalt in einen Abschnitt mit gerendert werden die `@section` Razor-Syntax.</span><span class="sxs-lookup"><span data-stu-id="f96f3-133">Individual views specify the content to be rendered within a section using the `@section` Razor syntax.</span></span> <span data-ttu-id="f96f3-134">Wenn eine Sicht ein Abschnitts definiert, gerendert werden müssen (oder tritt ein Fehler auf).</span><span class="sxs-lookup"><span data-stu-id="f96f3-134">If a view defines a section, it must be rendered (or an error will occur).</span></span>

<span data-ttu-id="f96f3-135">Ein Beispiel für `@section` Definition in einer Ansicht:</span><span class="sxs-lookup"><span data-stu-id="f96f3-135">An example `@section` definition in a view:</span></span>

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

<span data-ttu-id="f96f3-136">Im obigen Code validierungsskripts hinzugefügt werden die `scripts` Abschnitt für eine Sicht, die ein Formular enthält.</span><span class="sxs-lookup"><span data-stu-id="f96f3-136">In the code above, validation scripts are added to the `scripts` section on a view that includes a form.</span></span> <span data-ttu-id="f96f3-137">Anderen Sichten in der gleichen Anwendung zusätzliche Skripts möglicherweise nicht erforderlich, und daher einen Abschnitt zu Skripts definieren müssen.</span><span class="sxs-lookup"><span data-stu-id="f96f3-137">Other views in the same application might not require any additional scripts, and so wouldn't need to define a scripts section.</span></span>

<span data-ttu-id="f96f3-138">In einer Sicht definierten Abschnitte sind nur in der Seite "sofortiges Layout für" verfügbar.</span><span class="sxs-lookup"><span data-stu-id="f96f3-138">Sections defined in a view are available only in its immediate layout page.</span></span> <span data-ttu-id="f96f3-139">Sie können nicht von Replikatsgruppenelemente ansichtskomponenten oder anderen Teilen des Systems Sicht verwiesen werden.</span><span class="sxs-lookup"><span data-stu-id="f96f3-139">They cannot be referenced from partials, view components, or other parts of the view system.</span></span>

### <a name="ignoring-sections"></a><span data-ttu-id="f96f3-140">Ignorieren von Abschnitten</span><span class="sxs-lookup"><span data-stu-id="f96f3-140">Ignoring sections</span></span>

<span data-ttu-id="f96f3-141">Standardmäßig müssen der Text und alle Abschnitte in einer Inhaltsseite alle von der Seite "Layout" gerendert werden.</span><span class="sxs-lookup"><span data-stu-id="f96f3-141">By default, the body and all sections in a content page must all be rendered by the layout page.</span></span> <span data-ttu-id="f96f3-142">Das Razor-Ansichtsmodul erzwingt dies durch nachverfolgen, ob der Text und jeder Abschnitt gerendert wurde.</span><span class="sxs-lookup"><span data-stu-id="f96f3-142">The Razor view engine enforces this by tracking whether the body and each section have been rendered.</span></span>

<span data-ttu-id="f96f3-143">Um anzuweisen, das Anzeigemodul, die Text oder Abschnitte ignoriert werden sollen, rufen Sie die `IgnoreBody` und `IgnoreSection` Methoden.</span><span class="sxs-lookup"><span data-stu-id="f96f3-143">To instruct the view engine to ignore the body or sections, call the `IgnoreBody` and `IgnoreSection` methods.</span></span>

<span data-ttu-id="f96f3-144">Der Text und jeder Abschnitt in einer Razor-Seite müssen entweder gerendert oder ignoriert werden.</span><span class="sxs-lookup"><span data-stu-id="f96f3-144">The body and every section in a Razor page must be either rendered or ignored.</span></span>

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a><span data-ttu-id="f96f3-145">Importieren von freigegebenen Direktiven</span><span class="sxs-lookup"><span data-stu-id="f96f3-145">Importing Shared Directives</span></span>

<span data-ttu-id="f96f3-146">Sichten können Razor-Direktiven für viele Dinge sein, z. B. Namespaces importieren oder Ausführen von Aufgaben verwenden [Abhängigkeitsinjektion](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="f96f3-146">Views can use Razor directives to do many things, such as importing namespaces or performing [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="f96f3-147">Anweisungen, die von vielen Ansichten gemeinsam verwendet werden können angegeben werden, in eine gemeinsame `_ViewImports.cshtml` Datei.</span><span class="sxs-lookup"><span data-stu-id="f96f3-147">Directives shared by many views may be specified in a common `_ViewImports.cshtml` file.</span></span> <span data-ttu-id="f96f3-148">Die `_ViewImports` Datei unterstützt die folgenden Direktiven:</span><span class="sxs-lookup"><span data-stu-id="f96f3-148">The `_ViewImports` file supports the following directives:</span></span>

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

<span data-ttu-id="f96f3-149">Die Datei unterstützt keine andere Razor-Funktionen, z. B. Funktionen und Abschnittsdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="f96f3-149">The file doesn't support other Razor features, such as functions and section definitions.</span></span>

<span data-ttu-id="f96f3-150">Ein Beispiel für `_ViewImports.cshtml` Datei:</span><span class="sxs-lookup"><span data-stu-id="f96f3-150">A sample `_ViewImports.cshtml` file:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

<span data-ttu-id="f96f3-151">Die `_ViewImports.cshtml` -Konfigurationsdatei für eine ASP.NET-MVC-Anwendung Core sich in der Regel in befindet der `Views` Ordner.</span><span class="sxs-lookup"><span data-stu-id="f96f3-151">The `_ViewImports.cshtml` file for an ASP.NET Core MVC app is typically placed in the `Views` folder.</span></span> <span data-ttu-id="f96f3-152">Ein `_ViewImports.cshtml` Datei kann in einem beliebigen Ordner, in dem Fall sie gelten nur für, in diesem Ordner und seine Unterordner Ansichten platziert werden.</span><span class="sxs-lookup"><span data-stu-id="f96f3-152">A `_ViewImports.cshtml` file can be placed within any folder, in which case it will only be applied to views within that folder and its subfolders.</span></span> <span data-ttu-id="f96f3-153">`_ViewImports`Dateien werden verarbeitet, beginnend auf der Stammebene, und klicken Sie dann für jeden Ordner führende bis zu der Speicherort der die Sicht selbst, weshalb die Angabe von Einstellungen auf der Stammebene können überschrieben werden auf Ordnerebene.</span><span class="sxs-lookup"><span data-stu-id="f96f3-153">`_ViewImports` files are processed starting at the root level, and then for each folder leading up to the location of the view itself, so settings specified at the root level may be overridden at the folder level.</span></span>

<span data-ttu-id="f96f3-154">Z. B. wenn ein Stammebene `_ViewImports.cshtml` Datei gibt `@model` und `@addTagHelper`, und eine andere `_ViewImports.cshtml` -Datei in den Ordner Controller zugeordnet sind, der die Sicht gibt ein anderes `@model` und fügt eine andere `@addTagHelper`, die Ansicht Zugriff auf beide Tag Hilfsprogramme und wird mit der zweiten `@model`.</span><span class="sxs-lookup"><span data-stu-id="f96f3-154">For example, if a root level `_ViewImports.cshtml` file specifies `@model` and `@addTagHelper`, and another `_ViewImports.cshtml` file in the controller-associated folder of the view specifies a different `@model` and adds another `@addTagHelper`, the view will have access to both tag helpers and will use the latter `@model`.</span></span>

<span data-ttu-id="f96f3-155">Wenn mehrere `_ViewImports.cshtml` Dateien für eine Sicht ausgeführt werden, Verhalten der Direktiven für die in enthaltenen kombiniert die `ViewImports.cshtml` Dateien werden wie folgt:</span><span class="sxs-lookup"><span data-stu-id="f96f3-155">If multiple `_ViewImports.cshtml` files are run for a view, combined behavior of the directives included in the `ViewImports.cshtml` files will be as follows:</span></span>

* <span data-ttu-id="f96f3-156">`@addTagHelper`, `@removeTagHelper`: alle ausgeführt werden, in Reihenfolge</span><span class="sxs-lookup"><span data-stu-id="f96f3-156">`@addTagHelper`, `@removeTagHelper`: all run, in order</span></span>

* <span data-ttu-id="f96f3-157">`@tagHelperPrefix`: die nächste aus der Ansicht überschreibt alle anderen</span><span class="sxs-lookup"><span data-stu-id="f96f3-157">`@tagHelperPrefix`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="f96f3-158">`@model`: die nächste aus der Ansicht überschreibt alle anderen</span><span class="sxs-lookup"><span data-stu-id="f96f3-158">`@model`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="f96f3-159">`@inherits`: die nächste aus der Ansicht überschreibt alle anderen</span><span class="sxs-lookup"><span data-stu-id="f96f3-159">`@inherits`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="f96f3-160">`@using`: sind enthalten; Duplikate werden ignoriert.</span><span class="sxs-lookup"><span data-stu-id="f96f3-160">`@using`: all are included; duplicates are ignored</span></span>

* <span data-ttu-id="f96f3-161">`@inject`: die nächste aus der Ansicht für jede Eigenschaft überschreibt alle anderen mit dem gleichen Eigenschaftsnamen</span><span class="sxs-lookup"><span data-stu-id="f96f3-161">`@inject`: for each property, the closest one to the view overrides any others with the same property name</span></span>

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a><span data-ttu-id="f96f3-162">Ausführen von Code vor jeder Ansicht</span><span class="sxs-lookup"><span data-stu-id="f96f3-162">Running Code Before Each View</span></span>

<span data-ttu-id="f96f3-163">Wenn Sie über code verfügen, müssen Sie vor jeder Ansicht ausführen, dies sollte der `_ViewStart.cshtml` Datei.</span><span class="sxs-lookup"><span data-stu-id="f96f3-163">If you have code you need to run before every view, this should be placed in the `_ViewStart.cshtml` file.</span></span> <span data-ttu-id="f96f3-164">Gemäß der Konvention werden die `_ViewStart.cshtml` befindet sich in der `Views` Ordner.</span><span class="sxs-lookup"><span data-stu-id="f96f3-164">By convention, the `_ViewStart.cshtml` file is located in the `Views` folder.</span></span> <span data-ttu-id="f96f3-165">Die Anweisungen in aufgeführten `_ViewStart.cshtml` vor jedem vollständige Ansicht (nicht-Layouts und nicht Teilansichten) ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="f96f3-165">The statements listed in `_ViewStart.cshtml` are run before every full view (not layouts, and not partial views).</span></span> <span data-ttu-id="f96f3-166">Wie [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` ist hierarchisch aufgebaut.</span><span class="sxs-lookup"><span data-stu-id="f96f3-166">Like [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` is hierarchical.</span></span> <span data-ttu-id="f96f3-167">Wenn eine `_ViewStart.cshtml` Datei im Ordner "Controller zugeordnete Ansicht" definiert ist, er wird ausgeführt werden, nach der Datei im Stammverzeichnis des definiert die `Views` Ordner (sofern vorhanden).</span><span class="sxs-lookup"><span data-stu-id="f96f3-167">If a `_ViewStart.cshtml` file is defined in the controller-associated view folder, it will be run after the one defined in the root of the `Views` folder (if any).</span></span>

<span data-ttu-id="f96f3-168">Ein Beispiel für `_ViewStart.cshtml` Datei:</span><span class="sxs-lookup"><span data-stu-id="f96f3-168">A sample `_ViewStart.cshtml` file:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

<span data-ttu-id="f96f3-169">Die oben genannte Datei gibt an, dass alle Ansichten verwendet die `_Layout.cshtml` Layout.</span><span class="sxs-lookup"><span data-stu-id="f96f3-169">The file above specifies that all views will use the `_Layout.cshtml` layout.</span></span>

> [!NOTE]
> <span data-ttu-id="f96f3-170">Weder `_ViewStart.cshtml` noch `_ViewImports.cshtml` befinden sich in der Regel der `/Views/Shared` Ordner.</span><span class="sxs-lookup"><span data-stu-id="f96f3-170">Neither `_ViewStart.cshtml` nor `_ViewImports.cshtml` are typically placed in the `/Views/Shared` folder.</span></span> <span data-ttu-id="f96f3-171">Die app-Ebene Versionen dieser Dateien sollte direkt in die `/Views` Ordner.</span><span class="sxs-lookup"><span data-stu-id="f96f3-171">The app-level versions of these files should be placed directly in the `/Views` folder.</span></span>
