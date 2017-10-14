---
title: Layout
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 29f12d1f-9734-48bd-bf1a-cee53a8ab700
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/layout
ms.openlocfilehash: 064621d8756b007c5b8859111bf3a03a0d7dda81
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2017
---
# <a name="layout"></a><span data-ttu-id="1259f-103">Layout</span><span class="sxs-lookup"><span data-stu-id="1259f-103">Layout</span></span>

<span data-ttu-id="1259f-104">Durch [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="1259f-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="1259f-105">Ansichten verwenden häufig visuelle und programmgesteuerte Elemente.</span><span class="sxs-lookup"><span data-stu-id="1259f-105">Views frequently share visual and programmatic elements.</span></span> <span data-ttu-id="1259f-106">In diesem Artikel erfahren Sie, wie häufige Layouts verwenden, Teilen Direktiven und allgemeine Codes vor dem Rendern von Ansichten in der ASP.NET app ausführen.</span><span class="sxs-lookup"><span data-stu-id="1259f-106">In this article, you'll learn how to use common layouts, share directives, and run common code before rendering views in your ASP.NET app.</span></span>

## <a name="what-is-a-layout"></a><span data-ttu-id="1259f-107">Was ist ein Layout</span><span class="sxs-lookup"><span data-stu-id="1259f-107">What is a Layout</span></span>

<span data-ttu-id="1259f-108">Die meisten Web-apps haben ein gebräuchliches Layout, das den Benutzer ein konsistentes Verhalten bietet, wie sie von einer Seite zu navigieren.</span><span class="sxs-lookup"><span data-stu-id="1259f-108">Most web apps have a common layout that provides the user with a consistent experience as they navigate from page to page.</span></span> <span data-ttu-id="1259f-109">Das Layout enthält i. d. r. allgemeine Benutzeroberflächenelemente wie das app-Header, Navigation oder im Menüelemente und Fußzeile.</span><span class="sxs-lookup"><span data-stu-id="1259f-109">The layout typically includes common user interface elements such as the app header, navigation or menu elements, and footer.</span></span>

![Seitenlayout-Beispiel](layout/_static/page-layout.png)

<span data-ttu-id="1259f-111">Allgemeine HTML-Strukturen, z. B. Skripts und Stylesheets werden viele Seiten in einer app auch häufig verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="1259f-111">Common HTML structures such as scripts and stylesheets are also frequently used by many pages within an app.</span></span> <span data-ttu-id="1259f-112">Alle diese freigegebene Elemente können definiert werden, einem *Layout* -Datei, die dann von einer beliebigen Ansicht innerhalb der app verwendet verwiesen werden kann.</span><span class="sxs-lookup"><span data-stu-id="1259f-112">All of these shared elements may be defined in a *layout* file, which can then be referenced by any view used within the app.</span></span> <span data-ttu-id="1259f-113">Layouts reduzieren doppelte Code in Sichten, sodass führen Sie die [Don't wiederholen selbst (trocken)-Prinzip](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="1259f-113">Layouts reduce duplicate code in views, helping them follow the [Don't Repeat Yourself (DRY) principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="1259f-114">Gemäß der Konvention lautet das Standardlayout für eine ASP.NET app `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="1259f-114">By convention, the default layout for an ASP.NET app is named `_Layout.cshtml`.</span></span> <span data-ttu-id="1259f-115">Die Visual Studio ASP.NET Core MVC-Projektvorlage enthält diese Layoutdatei in die `Views/Shared` Ordner:</span><span class="sxs-lookup"><span data-stu-id="1259f-115">The Visual Studio ASP.NET Core MVC project template includes this layout file in the `Views/Shared` folder:</span></span>

![Ansichtenordner im Projektmappen-explorer](layout/_static/web-project-views.png)

<span data-ttu-id="1259f-117">Dieses Layout definiert eine Vorlage der obersten Ebene für Sichten in der app.</span><span class="sxs-lookup"><span data-stu-id="1259f-117">This layout defines a top level template for views in the app.</span></span> <span data-ttu-id="1259f-118">Apps erfordern kein Layout und apps können mehr als ein Layout definieren, mit anderen Ansichten, die unterschiedliche Layouts angeben.</span><span class="sxs-lookup"><span data-stu-id="1259f-118">Apps do not require a layout, and apps can define more than one layout, with different views specifying different layouts.</span></span>

<span data-ttu-id="1259f-119">Ein Beispiel für `_Layout.cshtml`:</span><span class="sxs-lookup"><span data-stu-id="1259f-119">An example `_Layout.cshtml`:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a><span data-ttu-id="1259f-120">Festlegen eines Layouts</span><span class="sxs-lookup"><span data-stu-id="1259f-120">Specifying a Layout</span></span>

<span data-ttu-id="1259f-121">Razor-Ansichten verfügen über eine `Layout` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="1259f-121">Razor views have a `Layout` property.</span></span> <span data-ttu-id="1259f-122">Einzelne Ansichten Geben Sie ein Layout durch Festlegen dieser Eigenschaft:</span><span class="sxs-lookup"><span data-stu-id="1259f-122">Individual views specify a layout by setting this property:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

<span data-ttu-id="1259f-123">Das angegebene Layout können einen vollständigen Pfad (Beispiel: `/Views/Shared/_Layout.cshtml`) oder einen Teilnamen (Beispiel: `_Layout`).</span><span class="sxs-lookup"><span data-stu-id="1259f-123">The layout specified can use a full path (example: `/Views/Shared/_Layout.cshtml`) or a partial name (example: `_Layout`).</span></span> <span data-ttu-id="1259f-124">Wenn ein partielle Name angegeben ist, werden das Razor-Ansichtsmodul für die Layoutdatei mithilfe der standardmäßigen Ermittlungsprozess durchsucht.</span><span class="sxs-lookup"><span data-stu-id="1259f-124">When a partial name is provided, the Razor view engine will search for the layout file using its standard discovery process.</span></span> <span data-ttu-id="1259f-125">Der Controller zugeordnete Ordner durchsucht zuerst, gefolgt von den `Shared` Ordner.</span><span class="sxs-lookup"><span data-stu-id="1259f-125">The controller-associated folder is searched first, followed by the `Shared` folder.</span></span> <span data-ttu-id="1259f-126">Dieser Discovery-Prozess ist identisch mit dem Kennwort verwendet, um ermitteln [Teilansichten](partial.md).</span><span class="sxs-lookup"><span data-stu-id="1259f-126">This discovery process is identical to the one used to discover [partial views](partial.md).</span></span>

<span data-ttu-id="1259f-127">Standardmäßig muss jedes Layout Aufrufen `RenderBody`.</span><span class="sxs-lookup"><span data-stu-id="1259f-127">By default, every layout must call `RenderBody`.</span></span> <span data-ttu-id="1259f-128">Immer, wenn der Aufruf von `RenderBody` ist platziert werden, wird der Inhalt der Ansicht gerendert.</span><span class="sxs-lookup"><span data-stu-id="1259f-128">Wherever the call to `RenderBody` is placed, the contents of the view will be rendered.</span></span>

<a name="layout-sections-label"></a>

### <a name="sections"></a><span data-ttu-id="1259f-129">Abschnitte</span><span class="sxs-lookup"><span data-stu-id="1259f-129">Sections</span></span>

<span data-ttu-id="1259f-130">Ein Layout kann optional eine oder mehrere verweisen *Abschnitte*, durch den Aufruf `RenderSection`.</span><span class="sxs-lookup"><span data-stu-id="1259f-130">A layout can optionally reference one or more *sections*, by calling `RenderSection`.</span></span> <span data-ttu-id="1259f-131">Abschnitte bieten eine Möglichkeit zum Organisieren, wo bestimmte Seitenelemente platziert werden soll.</span><span class="sxs-lookup"><span data-stu-id="1259f-131">Sections provide a way to organize where certain page elements should be placed.</span></span> <span data-ttu-id="1259f-132">Jeder Aufruf von `RenderSection` können angeben, ob dieses Abschnitts erforderlich oder optional ist.</span><span class="sxs-lookup"><span data-stu-id="1259f-132">Each call to `RenderSection` can specify whether that section is required or optional.</span></span> <span data-ttu-id="1259f-133">Wenn ein erforderliche Abschnitt nicht gefunden wird, wird eine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="1259f-133">If a required section is not found, an exception will be thrown.</span></span> <span data-ttu-id="1259f-134">Einzelne Ansichten Geben Sie den Inhalt in einen Abschnitt mit gerendert werden die `@section` Razor-Syntax.</span><span class="sxs-lookup"><span data-stu-id="1259f-134">Individual views specify the content to be rendered within a section using the `@section` Razor syntax.</span></span> <span data-ttu-id="1259f-135">Wenn eine Sicht ein Abschnitts definiert, gerendert werden müssen (oder tritt ein Fehler auf).</span><span class="sxs-lookup"><span data-stu-id="1259f-135">If a view defines a section, it must be rendered (or an error will occur).</span></span>

<span data-ttu-id="1259f-136">Ein Beispiel für `@section` Definition in einer Ansicht:</span><span class="sxs-lookup"><span data-stu-id="1259f-136">An example `@section` definition in a view:</span></span>

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

<span data-ttu-id="1259f-137">Im obigen Code validierungsskripts hinzugefügt werden die `scripts` Abschnitt für eine Sicht, die ein Formular enthält.</span><span class="sxs-lookup"><span data-stu-id="1259f-137">In the code above, validation scripts are added to the `scripts` section on a view that includes a form.</span></span> <span data-ttu-id="1259f-138">Anderen Sichten in der gleichen Anwendung zusätzliche Skripts möglicherweise nicht erforderlich, und daher einen Abschnitt zu Skripts definieren müssen.</span><span class="sxs-lookup"><span data-stu-id="1259f-138">Other views in the same application might not require any additional scripts, and so wouldn't need to define a scripts section.</span></span>

<span data-ttu-id="1259f-139">In einer Sicht definierten Abschnitte sind nur in der Seite "sofortiges Layout für" verfügbar.</span><span class="sxs-lookup"><span data-stu-id="1259f-139">Sections defined in a view are available only in its immediate layout page.</span></span> <span data-ttu-id="1259f-140">Sie können nicht von Replikatsgruppenelemente ansichtskomponenten oder anderen Teilen des Systems Sicht verwiesen werden.</span><span class="sxs-lookup"><span data-stu-id="1259f-140">They cannot be referenced from partials, view components, or other parts of the view system.</span></span>

### <a name="ignoring-sections"></a><span data-ttu-id="1259f-141">Ignorieren von Abschnitten</span><span class="sxs-lookup"><span data-stu-id="1259f-141">Ignoring sections</span></span>

<span data-ttu-id="1259f-142">Standardmäßig müssen der Text und alle Abschnitte in einer Inhaltsseite alle von der Seite "Layout" gerendert werden.</span><span class="sxs-lookup"><span data-stu-id="1259f-142">By default, the body and all sections in a content page must all be rendered by the layout page.</span></span> <span data-ttu-id="1259f-143">Das Razor-Ansichtsmodul erzwingt dies durch nachverfolgen, ob der Text und jeder Abschnitt gerendert wurde.</span><span class="sxs-lookup"><span data-stu-id="1259f-143">The Razor view engine enforces this by tracking whether the body and each section have been rendered.</span></span>

<span data-ttu-id="1259f-144">Um anzuweisen, das Anzeigemodul, die Text oder Abschnitte ignoriert werden sollen, rufen Sie die `IgnoreBody` und `IgnoreSection` Methoden.</span><span class="sxs-lookup"><span data-stu-id="1259f-144">To instruct the view engine to ignore the body or sections, call the `IgnoreBody` and `IgnoreSection` methods.</span></span>

<span data-ttu-id="1259f-145">Der Text und jeder Abschnitt in einer Razor-Seite müssen entweder gerendert oder ignoriert werden.</span><span class="sxs-lookup"><span data-stu-id="1259f-145">The body and every section in a Razor page must be either rendered or ignored.</span></span>

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a><span data-ttu-id="1259f-146">Importieren von freigegebenen Direktiven</span><span class="sxs-lookup"><span data-stu-id="1259f-146">Importing Shared Directives</span></span>

<span data-ttu-id="1259f-147">Sichten können Razor-Direktiven für viele Dinge sein, z. B. Namespaces importieren oder Ausführen von Aufgaben verwenden [Abhängigkeitsinjektion](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="1259f-147">Views can use Razor directives to do many things, such as importing namespaces or performing [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="1259f-148">Anweisungen, die von vielen Ansichten gemeinsam verwendet werden können angegeben werden, in eine gemeinsame `_ViewImports.cshtml` Datei.</span><span class="sxs-lookup"><span data-stu-id="1259f-148">Directives shared by many views may be specified in a common `_ViewImports.cshtml` file.</span></span> <span data-ttu-id="1259f-149">Die `_ViewImports` Datei unterstützt die folgenden Direktiven:</span><span class="sxs-lookup"><span data-stu-id="1259f-149">The `_ViewImports` file supports the following directives:</span></span>

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

<span data-ttu-id="1259f-150">Die Datei unterstützt keine andere Razor-Funktionen, z. B. Funktionen und Abschnittsdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="1259f-150">The file does not support other Razor features, such as functions and section definitions.</span></span>

<span data-ttu-id="1259f-151">Ein Beispiel für `_ViewImports.cshtml` Datei:</span><span class="sxs-lookup"><span data-stu-id="1259f-151">A sample `_ViewImports.cshtml` file:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

<span data-ttu-id="1259f-152">Die `_ViewImports.cshtml` -Konfigurationsdatei für eine ASP.NET-MVC-Anwendung Core sich in der Regel in befindet der `Views` Ordner.</span><span class="sxs-lookup"><span data-stu-id="1259f-152">The `_ViewImports.cshtml` file for an ASP.NET Core MVC app is typically placed in the `Views` folder.</span></span> <span data-ttu-id="1259f-153">Ein `_ViewImports.cshtml` Datei kann in einem beliebigen Ordner, in dem Fall sie gelten nur für, in diesem Ordner und seine Unterordner Ansichten platziert werden.</span><span class="sxs-lookup"><span data-stu-id="1259f-153">A `_ViewImports.cshtml` file can be placed within any folder, in which case it will only be applied to views within that folder and its subfolders.</span></span> <span data-ttu-id="1259f-154">`_ViewImports`Dateien werden verarbeitet, beginnend auf der Stammebene, und klicken Sie dann für jeden Ordner führende bis zu der Speicherort der die Sicht selbst, weshalb die Angabe von Einstellungen auf der Stammebene können überschrieben werden auf Ordnerebene.</span><span class="sxs-lookup"><span data-stu-id="1259f-154">`_ViewImports` files are processed starting at the root level, and then for each folder leading up to the location of the view itself, so settings specified at the root level may be overridden at the folder level.</span></span>

<span data-ttu-id="1259f-155">Z. B. wenn ein Stammebene `_ViewImports.cshtml` Datei gibt `@model` und `@addTagHelper`, und eine andere `_ViewImports.cshtml` -Datei in den Ordner Controller zugeordnet sind, der die Sicht gibt ein anderes `@model` und fügt eine andere `@addTagHelper`, die Ansicht Zugriff auf beide Tag Hilfsprogramme und wird mit der zweiten `@model`.</span><span class="sxs-lookup"><span data-stu-id="1259f-155">For example, if a root level `_ViewImports.cshtml` file specifies `@model` and `@addTagHelper`, and another `_ViewImports.cshtml` file in the controller-associated folder of the view specifies a different `@model` and adds another `@addTagHelper`, the view will have access to both tag helpers and will use the latter `@model`.</span></span>

<span data-ttu-id="1259f-156">Wenn mehrere `_ViewImports.cshtml` Dateien für eine Sicht ausgeführt werden, Verhalten der Direktiven für die in enthaltenen kombiniert die `ViewImports.cshtml` Dateien werden wie folgt:</span><span class="sxs-lookup"><span data-stu-id="1259f-156">If multiple `_ViewImports.cshtml` files are run for a view, combined behavior of the directives included in the `ViewImports.cshtml` files will be as follows:</span></span>

* <span data-ttu-id="1259f-157">`@addTagHelper`, `@removeTagHelper`: alle ausgeführt werden, in Reihenfolge</span><span class="sxs-lookup"><span data-stu-id="1259f-157">`@addTagHelper`, `@removeTagHelper`: all run, in order</span></span>

* <span data-ttu-id="1259f-158">`@tagHelperPrefix`: die nächste aus der Ansicht überschreibt alle anderen</span><span class="sxs-lookup"><span data-stu-id="1259f-158">`@tagHelperPrefix`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="1259f-159">`@model`: die nächste aus der Ansicht überschreibt alle anderen</span><span class="sxs-lookup"><span data-stu-id="1259f-159">`@model`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="1259f-160">`@inherits`: die nächste aus der Ansicht überschreibt alle anderen</span><span class="sxs-lookup"><span data-stu-id="1259f-160">`@inherits`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="1259f-161">`@using`: sind enthalten; Duplikate werden ignoriert.</span><span class="sxs-lookup"><span data-stu-id="1259f-161">`@using`: all are included; duplicates are ignored</span></span>

* <span data-ttu-id="1259f-162">`@inject`: die nächste aus der Ansicht für jede Eigenschaft überschreibt alle anderen mit dem gleichen Eigenschaftsnamen</span><span class="sxs-lookup"><span data-stu-id="1259f-162">`@inject`: for each property, the closest one to the view overrides any others with the same property name</span></span>

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a><span data-ttu-id="1259f-163">Ausführen von Code vor jeder Ansicht</span><span class="sxs-lookup"><span data-stu-id="1259f-163">Running Code Before Each View</span></span>

<span data-ttu-id="1259f-164">Wenn Sie über code verfügen, müssen Sie vor jeder Ansicht ausführen, dies sollte der `_ViewStart.cshtml` Datei.</span><span class="sxs-lookup"><span data-stu-id="1259f-164">If you have code you need to run before every view, this should be placed in the `_ViewStart.cshtml` file.</span></span> <span data-ttu-id="1259f-165">Gemäß der Konvention werden die `_ViewStart.cshtml` befindet sich in der `Views` Ordner.</span><span class="sxs-lookup"><span data-stu-id="1259f-165">By convention, the `_ViewStart.cshtml` file is located in the `Views` folder.</span></span> <span data-ttu-id="1259f-166">Die Anweisungen in aufgeführten `_ViewStart.cshtml` vor jedem vollständige Ansicht (nicht-Layouts und nicht Teilansichten) ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="1259f-166">The statements listed in `_ViewStart.cshtml` are run before every full view (not layouts, and not partial views).</span></span> <span data-ttu-id="1259f-167">Wie [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` ist hierarchisch aufgebaut.</span><span class="sxs-lookup"><span data-stu-id="1259f-167">Like [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` is hierarchical.</span></span> <span data-ttu-id="1259f-168">Wenn eine `_ViewStart.cshtml` Datei im Ordner "Controller zugeordnete Ansicht" definiert ist, er wird ausgeführt werden, nach der Datei im Stammverzeichnis des definiert die `Views` Ordner (sofern vorhanden).</span><span class="sxs-lookup"><span data-stu-id="1259f-168">If a `_ViewStart.cshtml` file is defined in the controller-associated view folder, it will be run after the one defined in the root of the `Views` folder (if any).</span></span>

<span data-ttu-id="1259f-169">Ein Beispiel für `_ViewStart.cshtml` Datei:</span><span class="sxs-lookup"><span data-stu-id="1259f-169">A sample `_ViewStart.cshtml` file:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

<span data-ttu-id="1259f-170">Die oben genannte Datei gibt an, dass alle Ansichten verwendet die `_Layout.cshtml` Layout.</span><span class="sxs-lookup"><span data-stu-id="1259f-170">The file above specifies that all views will use the `_Layout.cshtml` layout.</span></span>

> [!NOTE]
> <span data-ttu-id="1259f-171">Weder `_ViewStart.cshtml` noch `_ViewImports.cshtml` befinden sich in der Regel der `/Views/Shared` Ordner.</span><span class="sxs-lookup"><span data-stu-id="1259f-171">Neither `_ViewStart.cshtml` nor `_ViewImports.cshtml` are typically placed in the `/Views/Shared` folder.</span></span> <span data-ttu-id="1259f-172">Die app-Ebene Versionen dieser Dateien sollte direkt in die `/Views` Ordner.</span><span class="sxs-lookup"><span data-stu-id="1259f-172">The app-level versions of these files should be placed directly in the `/Views` folder.</span></span>
