---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: Einführung in ASP.NET Web Pages - Erstellen eines konsistenten Layouts | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Tutorial erfahren Sie, wie Sie die Layouts zu verwenden, um ein einheitliches Aussehen für die Seiten auf einer Website zu erstellen, die ASP.NET Web Pages verwendet. Es wird vorausgesetzt, Sie haben die...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: a6a007678d58547e9987ebda46bd08ae8aea66f7
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021178"
---
<a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a><span data-ttu-id="30cec-104">Einführung in ASP.NET Web Pages - Erstellen eines konsistenten Layouts</span><span class="sxs-lookup"><span data-stu-id="30cec-104">Introducing ASP.NET Web Pages - Creating a Consistent Layout</span></span>
====================
<span data-ttu-id="30cec-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="30cec-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="30cec-106">In diesem Tutorial erfahren Sie, wie Sie mit *Layouts* ein einheitliches Aussehen für die Seiten auf einer Website zu erstellen, die ASP.NET Web Pages verwendet.</span><span class="sxs-lookup"><span data-stu-id="30cec-106">This tutorial shows you how to use *layouts* to create a consistent look for the pages on a site that uses ASP.NET Web Pages.</span></span> <span data-ttu-id="30cec-107">Es wird vorausgesetzt, Sie haben die Reihe über [Löschen von Daten in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251584).</span><span class="sxs-lookup"><span data-stu-id="30cec-107">It assumes you have completed the series through [Deleting Database Data in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251584).</span></span>
> 
> <span data-ttu-id="30cec-108">Sie lernen Folgendes:</span><span class="sxs-lookup"><span data-stu-id="30cec-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="30cec-109">Welche eine Layoutseite ist.</span><span class="sxs-lookup"><span data-stu-id="30cec-109">What a layout page is.</span></span>
> - <span data-ttu-id="30cec-110">Wie Sie Layoutseiten mit dynamischen Inhalten zu kombinieren.</span><span class="sxs-lookup"><span data-stu-id="30cec-110">How to combine layout pages with dynamic content.</span></span>
> - <span data-ttu-id="30cec-111">Informationen zum Übergeben von Werten an eine Layoutseite.</span><span class="sxs-lookup"><span data-stu-id="30cec-111">How to pass values to a layout page.</span></span>


## <a name="about-layouts"></a><span data-ttu-id="30cec-112">Informationen zu Layouts</span><span class="sxs-lookup"><span data-stu-id="30cec-112">About Layouts</span></span>

<span data-ttu-id="30cec-113">Die Seiten, die Sie bisher erstellt haben alle wurden abgeschlossen ist, eigenständige Seiten.</span><span class="sxs-lookup"><span data-stu-id="30cec-113">The pages you've created so far have all been complete, standalone pages.</span></span> <span data-ttu-id="30cec-114">Alle am selben Standort gehören, jedoch sind keine allgemeine Elemente oder ein standard überprüfen.</span><span class="sxs-lookup"><span data-stu-id="30cec-114">They all belong to the same site, but they don't have any common elements or a standard look.</span></span>

<span data-ttu-id="30cec-115">Die meisten Sites haben ein einheitliches Aussehen und Layout.</span><span class="sxs-lookup"><span data-stu-id="30cec-115">Most sites do have a consistent look and layout.</span></span> <span data-ttu-id="30cec-116">Angenommen, Sie zum Wechseln der [Microsoft.com/Web](https://www.microsoft.com/web/) Standort- und zu suchen, sehen Sie, dass alle Seiten, um ein Layout für die allgemeine und eine visual Studio-Designs entsprechen:</span><span class="sxs-lookup"><span data-stu-id="30cec-116">For example, if you go to the [Microsoft.com/web](https://www.microsoft.com/web/) site and look around, you see that the pages all adhere to an overall layout and to a visual theme:</span></span>

![Microsoft.com/Web-Webseite, die mit dem Layout des der Header, Navigation, Inhaltsbereich und Fußzeile](layouts/_static/image1.png)

<span data-ttu-id="30cec-118">Ein *ineffizient* Möglichkeit zum Erstellen dieses Layout wäre ein Header, Navigationsleiste und Fußzeile separat für jede definieren.</span><span class="sxs-lookup"><span data-stu-id="30cec-118">An *inefficient* way to create this layout would be to define a header, navigation bar, and footer separately on each of your pages.</span></span> <span data-ttu-id="30cec-119">Werden, würden Sie das gleiche Markup duplizieren.</span><span class="sxs-lookup"><span data-stu-id="30cec-119">You'd be duplicating the same markup each time.</span></span> <span data-ttu-id="30cec-120">Wenn Sie Änderungen vornehmen möchten (z. B. aktualisieren die Fußzeile), müssen Sie jede Seite separat zu ändern.</span><span class="sxs-lookup"><span data-stu-id="30cec-120">If you wanted to change something (for example, update the footer), you'd have to change each page separately.</span></span>

<span data-ttu-id="30cec-121">D. h. wo *Layoutseiten* eingehen.</span><span class="sxs-lookup"><span data-stu-id="30cec-121">That's where *layout pages* come in.</span></span> <span data-ttu-id="30cec-122">In ASP.NET Web Pages, können Sie eine Layoutseite definieren, die einen allgemeinen Container für Seiten auf Ihrer Website bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="30cec-122">In ASP.NET Web Pages, you can define a layout page that provides an overall container for pages on your site.</span></span> <span data-ttu-id="30cec-123">Auf der Layoutseite kann z. B. die Header, Navigation und Fußzeile enthalten.</span><span class="sxs-lookup"><span data-stu-id="30cec-123">For example, the layout page can contain the header, navigation area, and footer.</span></span> <span data-ttu-id="30cec-124">Auf der Layoutseite enthält einen Platzhalter Speicherorts für der Hauptinhalt.</span><span class="sxs-lookup"><span data-stu-id="30cec-124">The layout page includes a placeholder where the main content goes.</span></span>

<span data-ttu-id="30cec-125">Sie können dann die einzelnen Inhaltsseiten definieren, die das Markup und Code nur auf dieser Seite enthalten.</span><span class="sxs-lookup"><span data-stu-id="30cec-125">You can then define individual content pages that contain the markup and the code for only that page.</span></span> <span data-ttu-id="30cec-126">Vollständige HTML-Seiten werden müssen Inhaltsseiten keine; Sie müssen auch keine haben eine `<body>` Element.</span><span class="sxs-lookup"><span data-stu-id="30cec-126">Content pages don't have to be complete HTML pages; they don't even have to have a `<body>` element.</span></span> <span data-ttu-id="30cec-127">Sie haben außerdem eine einzige Zeile Code, die ASP.NET mitteilt, welche Layoutseite den Inhalt angezeigt werden soll.</span><span class="sxs-lookup"><span data-stu-id="30cec-127">They also have a line of code that tells ASP.NET what layout page you want to display the content in.</span></span> <span data-ttu-id="30cec-128">Hier ist ein Bild, das ungefähr veranschaulicht die Funktionsweise von dieser Beziehung:</span><span class="sxs-lookup"><span data-stu-id="30cec-128">Here's a picture that shows roughly how this relationship works:</span></span>

![Konzeptionelle Diagramm zeigt zwei Inhaltsseiten und einer Layoutseite aus, in der sie passen](layouts/_static/image2.png)

<span data-ttu-id="30cec-130">Diese Interaktion ist einfach zu verstehen, wenn Sie es in Aktion zu sehen.</span><span class="sxs-lookup"><span data-stu-id="30cec-130">This interaction is easy to understand when you see it in action.</span></span> <span data-ttu-id="30cec-131">In diesem Tutorial ändern Sie Filme Seiten, um ein Layout verwenden.</span><span class="sxs-lookup"><span data-stu-id="30cec-131">In this tutorial, you'll change your movies pages to use a layout.</span></span>

## <a name="adding-a-layout-page"></a><span data-ttu-id="30cec-132">Hinzufügen einer Layoutseite</span><span class="sxs-lookup"><span data-stu-id="30cec-132">Adding a Layout Page</span></span>

<span data-ttu-id="30cec-133">Sie beginnen, erstellen Sie eine Layoutseite, die ein typisches Seitenlayout mit einem Header, Footer und einen Bereich für den Hauptinhalt definiert.</span><span class="sxs-lookup"><span data-stu-id="30cec-133">You'll start by creating a layout page that defines a typical page layout with a header, footer, and an area for the main content.</span></span> <span data-ttu-id="30cec-134">Fügen Sie am Standort WebPagesMovies eine CSHTML-Seite namens  *\_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="30cec-134">In the WebPagesMovies site, add a CSHTML page named *\_Layout.cshtml*.</span></span>

<span data-ttu-id="30cec-135">Der führende Unterstrich ( `_` ) Zeichen ist von Bedeutung.</span><span class="sxs-lookup"><span data-stu-id="30cec-135">The leading underscore ( `_` ) character is significant.</span></span> <span data-ttu-id="30cec-136">Wenn eine Seite-Name mit einem Unterstrich beginnt, wird nicht ASP.NET diese Seite direkt an den Browser senden.</span><span class="sxs-lookup"><span data-stu-id="30cec-136">If a page's name starts with an underscore, ASP.NET won't directly send that page to the browser.</span></span> <span data-ttu-id="30cec-137">Diese Konvention können Sie die Seiten zu definieren, die für Ihre Website erforderlich sind, aber Benutzer direkt anfordern können sollte nicht.</span><span class="sxs-lookup"><span data-stu-id="30cec-137">This convention lets you define pages that are required for your site but that users shouldn't be able to request directly.</span></span>

<span data-ttu-id="30cec-138">Ersetzen Sie den Inhalt auf der Seite Folgendes ein:</span><span class="sxs-lookup"><span data-stu-id="30cec-138">Replace the content in the page with the following:</span></span>

[!code-html[Main](layouts/samples/sample1.html)]

<span data-ttu-id="30cec-139">Wie Sie sehen können, handelt es sich bei diesem Markup ist ausschließlich aus HTML bestehen, die verwendet `<div>` Elemente zum Definieren von drei Abschnitte in der Seite plus 1 mehr `<div>` Element, das drei Abschnitte enthalten soll.</span><span class="sxs-lookup"><span data-stu-id="30cec-139">As you can see, this markup is just HTML that uses `<div>` elements to define three sections in the page plus one more `<div>` element to hold the three sections.</span></span> <span data-ttu-id="30cec-140">Die Fußzeile enthält Razor-Code ein wenig: `@DateTime.Now.Year`, rendert die an dieser Stelle auf der Seite des aktuellen Jahrs.</span><span class="sxs-lookup"><span data-stu-id="30cec-140">The footer contains a bit of Razor code: `@DateTime.Now.Year`, which will render the current year at that location in the page.</span></span>

<span data-ttu-id="30cec-141">Beachten Sie, dass es ein Link zu einem Stylesheet, mit dem Namen *Movies.css*.</span><span class="sxs-lookup"><span data-stu-id="30cec-141">Notice that there's a link to a style sheet named *Movies.css*.</span></span> <span data-ttu-id="30cec-142">Das Stylesheet ist, in dem die Details des physischen Layouts der Elemente definiert werden.</span><span class="sxs-lookup"><span data-stu-id="30cec-142">The style sheet is where the details of the physical layout of the elements will be defined.</span></span> <span data-ttu-id="30cec-143">Sie erstellen, die in Kürze.</span><span class="sxs-lookup"><span data-stu-id="30cec-143">You'll create that in a moment.</span></span>

<span data-ttu-id="30cec-144">Die einzigen ungewöhnlichen-Funktion in diesem  *\_Layout.cshtml* Seite ist die `@Render.Body()` Zeile.</span><span class="sxs-lookup"><span data-stu-id="30cec-144">The only unusual feature in this *\_Layout.cshtml* page is the `@Render.Body()` line.</span></span> <span data-ttu-id="30cec-145">Dies ist der Platzhalter, wo der Inhalt für den dieses Layout mit einer anderen Seite zusammengeführt wird.</span><span class="sxs-lookup"><span data-stu-id="30cec-145">That's the placeholder where the content will go when this layout is merged with another page.</span></span>

## <a name="adding-a-css-file"></a><span data-ttu-id="30cec-146">Hinzufügen einer CSS-Datei</span><span class="sxs-lookup"><span data-stu-id="30cec-146">Adding a .css File</span></span>

<span data-ttu-id="30cec-147">Die bevorzugte Methode zum Definieren der tatsächlichen Anordnung (d. h. Darstellung) von Elementen auf der Seite ist die Verwendung von cascading Stylesheet (CSS)-Regeln.</span><span class="sxs-lookup"><span data-stu-id="30cec-147">The preferred way to define the actual arrangement (that is, appearance) of elements on the page is to use cascading style sheet (CSS) rules.</span></span> <span data-ttu-id="30cec-148">So Sie erstellen eine *CSS* -Datei mit den Regeln für das neue Layout.</span><span class="sxs-lookup"><span data-stu-id="30cec-148">So you'll create a *.css* file that has the rules for your new layout.</span></span>

<span data-ttu-id="30cec-149">Wählen Sie den Stamm Ihrer Website in WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="30cec-149">In WebMatrix, select the root of your site.</span></span> <span data-ttu-id="30cec-150">Klicken Sie dann in der **Dateien** Registerkarte des Menübands, klicken Sie auf den Pfeil unter der **neu** Schaltfläche, und klicken Sie dann auf **neuer Ordner**.</span><span class="sxs-lookup"><span data-stu-id="30cec-150">Then in the **Files** tab of the ribbon, click the arrow under the **New** button and then click **New Folder**.</span></span>

![Die Option "Neuer Ordner" unter neu auf dem Menüband.](layouts/_static/image3.png)

<span data-ttu-id="30cec-152">Nennen Sie diesen Ordner *Stile*.</span><span class="sxs-lookup"><span data-stu-id="30cec-152">Name the new folder *Styles*.</span></span>

![Benennen den neuen Ordner "Formatvorlagen"](layouts/_static/image4.png)

<span data-ttu-id="30cec-154">In der neuen *Stile* Ordner eine Datei namens *Movies.css*.</span><span class="sxs-lookup"><span data-stu-id="30cec-154">Inside the new *Styles* folder, create a file named *Movies.css*.</span></span>

![Erstellen einer neuen Movies.css-Datei](layouts/_static/image5.png)

<span data-ttu-id="30cec-156">Ersetzen Sie den Inhalt der neuen *CSS* Datei durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="30cec-156">Replace the contents of the new *.css* file with the following:</span></span>

[!code-css[Main](layouts/samples/sample2.css)]

<span data-ttu-id="30cec-157">Es wird nicht viel über diese CSS-Regeln, mit Ausnahme von zwei Dinge beachten angenommen.</span><span class="sxs-lookup"><span data-stu-id="30cec-157">We won't say much about these CSS rules, except to note two things.</span></span> <span data-ttu-id="30cec-158">Ist, dass zusätzlich zum Festlegen von Schriftarten und Größen können die Regeln absolute Positionierung, verwenden um den Speicherort der Kopfzeile, Fußzeile und Hauptinhaltsbereich einzurichten.</span><span class="sxs-lookup"><span data-stu-id="30cec-158">One is that in addition to setting fonts and sizes, the rules use absolute positioning to establish the location of the header, footer, and main content area.</span></span> <span data-ttu-id="30cec-159">Wenn Sie neu positionieren in CSS ist, können Sie lesen die [CSS-Positionierung](http://www.w3schools.com/css/css_positioning.asp) am Standort W3Schools Tutorial.</span><span class="sxs-lookup"><span data-stu-id="30cec-159">If you're new to positioning in CSS, you can read the [CSS Positioning](http://www.w3schools.com/css/css_positioning.asp) tutorial at the W3Schools site.</span></span>

<span data-ttu-id="30cec-160">Das andere, was zu beachten ist, dass Sie unten auf die wir Stilregeln kopiert haben, die ursprünglich definiert einzeln in die *Movies.cshtml* Datei.</span><span class="sxs-lookup"><span data-stu-id="30cec-160">The other thing to note is that at the bottom, we've copied the style rules that were originally defined individually in the *Movies.cshtml* file.</span></span> <span data-ttu-id="30cec-161">Diese Regeln wurden in verwendet die [Einführung in die Anzeige von Daten durch Verwenden von ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251580) beschrieben wird, stellen die `WebGrid` Helper Rendern von Markup, das der Tabelle verteilt hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="30cec-161">These rules were used in the [Introduction to Displaying Data by Using ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251580) tutorial to make the `WebGrid` helper render markup that added stripes to the table.</span></span> <span data-ttu-id="30cec-162">(Wenn Sie vorhaben, verwenden Sie eine *CSS* Datei Style-Definitionen, können Sie auch die Stilregeln für die gesamte Website darin einfügen.)</span><span class="sxs-lookup"><span data-stu-id="30cec-162">(If you're going to use a *.css* file for style definitions, you might as well put the style rules for the whole site in it.)</span></span>

## <a name="updating-the-movies-file-to-use-the-layout"></a><span data-ttu-id="30cec-163">Aktualisieren der Filme-Datei, um das Layout zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="30cec-163">Updating the Movies File to Use the Layout</span></span>

<span data-ttu-id="30cec-164">Jetzt können Sie die vorhandenen Dateien an Ihrem Standort verwenden Sie das neue Layout aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="30cec-164">Now you can update the existing files in your site to use the new layout.</span></span> <span data-ttu-id="30cec-165">Öffnen der *Movies.cshtml* Datei.</span><span class="sxs-lookup"><span data-stu-id="30cec-165">Open the *Movies.cshtml* file.</span></span> <span data-ttu-id="30cec-166">Im oberen Bereich als erste Zeile des Codes, fügen Sie Folgendes hinzu:</span><span class="sxs-lookup"><span data-stu-id="30cec-166">At the top, as the first line of code, add the following:</span></span>

[!code-csharp[Main](layouts/samples/sample3.cs)]

<span data-ttu-id="30cec-167">Die Seite beginnt jetzt auf diese Weise:</span><span class="sxs-lookup"><span data-stu-id="30cec-167">The page now starts out this way:</span></span>

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

<span data-ttu-id="30cec-168">Diese eine Codezeile weist ASP.NET, die bei der *Filme* Seite ausgeführt wird, die sie die zusammengeführt werden sollen, mit der  *\_Layout.cshtml* Datei.</span><span class="sxs-lookup"><span data-stu-id="30cec-168">This one line of code tells ASP.NET that when the *Movies* page runs, it should be merged with the *\_Layout.cshtml* file.</span></span>

<span data-ttu-id="30cec-169">Da die *Movies.cshtml* -Datei verwendet jetzt eine Layoutseite verwenden, können Sie das Markup aus Entfernen der *Movies.cshtml* Seite, die vom übernommen hat die  *\_Layout.cshtml*Datei.</span><span class="sxs-lookup"><span data-stu-id="30cec-169">Since the *Movies.cshtml* file now uses a layout page, you can remove the markup from the *Movies.cshtml* page that's taken care of by the *\_Layout.cshtml* file.</span></span> <span data-ttu-id="30cec-170">Nehmen Sie sich die `<!DOCTYPE>`, `<html>`, und `<body>` Start- und Endtags.</span><span class="sxs-lookup"><span data-stu-id="30cec-170">Take out the `<!DOCTYPE>`, `<html>`, and `<body>` opening and closing tags.</span></span> <span data-ttu-id="30cec-171">Nutzen Sie die gesamte `<head>` -Element und dessen Inhalt, enthält die Stilregeln für das Raster, da Sie jetzt diese Regeln haben einem *CSS* Datei.</span><span class="sxs-lookup"><span data-stu-id="30cec-171">Take out the entire `<head>` element and its contents, which includes the style rules for the grid, since you've now got those rules in a *.css* file.</span></span> <span data-ttu-id="30cec-172">Während Sie schon dabei sind, ändern Sie die vorhandene `<h1>` Element eine `<h2>` Element; Sie haben eine `<h1>` Element bereits der Layoutseite.</span><span class="sxs-lookup"><span data-stu-id="30cec-172">While you're at it, change the existing `<h1>` element to an `<h2>` element; you have an `<h1>` element in the layout page already.</span></span> <span data-ttu-id="30cec-173">Ändern der `<h2>` Text, der "Liste Filme".</span><span class="sxs-lookup"><span data-stu-id="30cec-173">Change the `<h2>` text to "List Movies".</span></span>

<span data-ttu-id="30cec-174">Sie müssen normalerweise wäre nicht diese Arten von Änderungen in einer Inhaltsseite zu machen.</span><span class="sxs-lookup"><span data-stu-id="30cec-174">Normally you wouldn't have to make these sorts of changes in a content page.</span></span> <span data-ttu-id="30cec-175">Wenn Sie Ihre Website mit einer Layoutseite beginnen, erstellen Sie zunächst Inhaltsseiten ohne all diese Elemente.</span><span class="sxs-lookup"><span data-stu-id="30cec-175">When you start your site out with a layout page, you create content pages without all these elements to begin with.</span></span> <span data-ttu-id="30cec-176">In diesem Fall sind jedoch eine eigenständigen Seite in einem konvertiert, die ein Layout verwendet wird, gibt es also praktisch zur Bereinigung.</span><span class="sxs-lookup"><span data-stu-id="30cec-176">In this case, though, you're converting a standalone page to one that uses a layout, so there's a bit of cleanup.</span></span>

<span data-ttu-id="30cec-177">Wenn Sie fertig sind, die *Movies.cshtml* Seite wird wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="30cec-177">When you're finished, the *Movies.cshtml* page will look like the following:</span></span>

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a><span data-ttu-id="30cec-178">Testen das Layout</span><span class="sxs-lookup"><span data-stu-id="30cec-178">Testing the Layout</span></span>

<span data-ttu-id="30cec-179">Jetzt können Sie sehen, wie das Layout aussieht.</span><span class="sxs-lookup"><span data-stu-id="30cec-179">Now you can see what the layout looks like.</span></span> <span data-ttu-id="30cec-180">WebMatrix, mit der Maustaste der *Movies.cshtml* Seite und wählen Sie **in Browser starten**.</span><span class="sxs-lookup"><span data-stu-id="30cec-180">In WebMatrix, right-click the *Movies.cshtml* page and select **Launch in browser**.</span></span> <span data-ttu-id="30cec-181">Wenn der Browser die Seite angezeigt wird, sieht es auf dieser Seite:</span><span class="sxs-lookup"><span data-stu-id="30cec-181">When the browser displays the page, it looks like this page:</span></span>

![Seite "Movies" mit einem Layout gerendert](layouts/_static/image6.png)

<span data-ttu-id="30cec-183">ASP.NET hat den Inhalt der Seite Movies.cshtml in zusammengeführt der  *\_Layout.cshtml* Seite rechts, wobei die `RenderBody` Methode ist.</span><span class="sxs-lookup"><span data-stu-id="30cec-183">ASP.NET has merged the content of the Movies.cshtml page into the *\_Layout.cshtml* page right where the `RenderBody` method is.</span></span> <span data-ttu-id="30cec-184">Und natürlich die  *\_Layout.cshtml* Verweise Seite eine *CSS* Datei, die die Darstellung der Seite definiert.</span><span class="sxs-lookup"><span data-stu-id="30cec-184">And of course the *\_Layout.cshtml* page references a *.css* file that defines the look of the page.</span></span>

## <a name="updating-the-addmovie-page-to-use-the-layout"></a><span data-ttu-id="30cec-185">Zum Aktualisieren der AddMovie-Seite, um das Layout zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="30cec-185">Updating the AddMovie Page to Use the Layout</span></span>

<span data-ttu-id="30cec-186">Die eigentlichen Vorteile der Layouts ist, dass Sie sie für alle Seiten auf Ihrer Website verwenden können.</span><span class="sxs-lookup"><span data-stu-id="30cec-186">The real benefit of layouts is that you can use them for all the pages in your site.</span></span> <span data-ttu-id="30cec-187">Öffnen der *AddMovie.cshtml* Seite.</span><span class="sxs-lookup"><span data-stu-id="30cec-187">Open the *AddMovie.cshtml* page.</span></span>

<span data-ttu-id="30cec-188">Erinnern Sie sich, die die *AddMovie.cshtml* Seite hatten ursprünglich eine CSS-Regeln, um das Aussehen von Überprüfungsfehlermeldungen definieren.</span><span class="sxs-lookup"><span data-stu-id="30cec-188">You might remember that the *AddMovie.cshtml* page originally had some CSS rules in it to define the look of validation error messages.</span></span> <span data-ttu-id="30cec-189">Da Sie haben eine *CSS* -Datei für Ihre Website jetzt, Sie können diese Regeln zum Verschieben der *CSS* Datei.</span><span class="sxs-lookup"><span data-stu-id="30cec-189">Since you have a *.css* file for your site now, you can move those rules to the *.css* file.</span></span> <span data-ttu-id="30cec-190">Entfernen Sie sie aus der *AddMovie.cshtml* Datei, und fügen sie am unteren Rand der *Movies.css* Datei.</span><span class="sxs-lookup"><span data-stu-id="30cec-190">Remove them from the *AddMovie.cshtml* file and add them to the bottom of the *Movies.css* file.</span></span> <span data-ttu-id="30cec-191">Verschieben Sie die folgenden Regeln:</span><span class="sxs-lookup"><span data-stu-id="30cec-191">You are moving the following rules:</span></span>

[!code-css[Main](layouts/samples/sample6.css)]

<span data-ttu-id="30cec-192">Stellen Sie jetzt die gleichen Arten von Änderungen in *AddMovie.cshtml* , die Sie für *Movies.cshtml* – hinzufügen `Layout="~/_Layout.cshtml;` und entfernen Sie das HTML-Markup, das nun überflüssige ist.</span><span class="sxs-lookup"><span data-stu-id="30cec-192">Now make the same sorts of changes in *AddMovie.cshtml* that you did for *Movies.cshtml* — add `Layout="~/_Layout.cshtml;` and remove the HTML markup that's now extraneous.</span></span> <span data-ttu-id="30cec-193">Ändern Sie das `<h1>`-Element in `<h2>`.</span><span class="sxs-lookup"><span data-stu-id="30cec-193">Change the `<h1>` element to `<h2>`.</span></span> <span data-ttu-id="30cec-194">Wenn Sie fertig sind, wird die Seite wie im folgenden Beispiel aussehen:</span><span class="sxs-lookup"><span data-stu-id="30cec-194">When you're done, the page will look like this example:</span></span>

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

<span data-ttu-id="30cec-195">Führen Sie die Seite.</span><span class="sxs-lookup"><span data-stu-id="30cec-195">Run the page.</span></span> <span data-ttu-id="30cec-196">Sie sieht jetzt wie in dieser Abbildung aus:</span><span class="sxs-lookup"><span data-stu-id="30cec-196">Now it looks like this illustration:</span></span>

!["Add Filme" Seiten mit einem layout](layouts/_static/image7.png)

<span data-ttu-id="30cec-198">Sie ähnliche Änderungen an den Seiten der Website vornehmen möchten – *EditMovie.cshtml* und *DeleteMovie.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="30cec-198">You want to make similar changes to the pages in the site — *EditMovie.cshtml* and *DeleteMovie.cshtml*.</span></span> <span data-ttu-id="30cec-199">Allerdings können Sie vor dem Ausführen des Layouts einer anderen Änderung vornehmen, die eine größere Flexibilität ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="30cec-199">However, before you do, you can make another change to the layout that makes it a little more flexible.</span></span>

## <a name="passing-title-information-to-the-layout-page"></a><span data-ttu-id="30cec-200">Informationen zu Softwaretiteln übergeben die Seite ' Layout '</span><span class="sxs-lookup"><span data-stu-id="30cec-200">Passing Title Information to the Layout Page</span></span>

<span data-ttu-id="30cec-201">Die  *\_Layout.cshtml* Seite, die Sie erstellt hat eine `<title>` -Element, das auf "Meine Movie-Website" festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="30cec-201">The *\_Layout.cshtml* page that you created has a `<title>` element that's set to "My Movie Site".</span></span> <span data-ttu-id="30cec-202">Die meisten Browser anzeigen den Inhalt dieses Elements als Text auf einer Registerkarte:</span><span class="sxs-lookup"><span data-stu-id="30cec-202">Most browsers display the content of this element as the text on a tab:</span></span>

![Die Seite &lt;Titel&gt; in einer Browserregisterkarte angezeigten Elemente](layouts/_static/image8.png)

<span data-ttu-id="30cec-204">Diese Informationen zu Softwaretiteln ist generisch.</span><span class="sxs-lookup"><span data-stu-id="30cec-204">This title information is generic.</span></span> <span data-ttu-id="30cec-205">Nehmen wir an, dass das Titeltext sollen genauer auf die aktuelle Seite.</span><span class="sxs-lookup"><span data-stu-id="30cec-205">Suppose that you want the title text to be more specific to the current page.</span></span> <span data-ttu-id="30cec-206">(Der Titeltext wird auch von Suchmaschinen verwendet um zu bestimmen, was zu Ihrer Seite ist.) Sie können Informationen übergeben, von einer Inhaltsseite wie *Movies.cshtml* oder *AddMovie.cshtml* auf der Seite "Layout" und dann diese Informationen an die Layoutseite anpassen darstellt.</span><span class="sxs-lookup"><span data-stu-id="30cec-206">(The title text is also used by search engines to determine what your page is about.) You can pass information from a content page like *Movies.cshtml* or *AddMovie.cshtml* to the layout page, and then use that information to customize what the layout page renders.</span></span>

<span data-ttu-id="30cec-207">Öffnen der *Movies.cshtml* Seite erneut.</span><span class="sxs-lookup"><span data-stu-id="30cec-207">Open the *Movies.cshtml* page again.</span></span> <span data-ttu-id="30cec-208">Fügen Sie im Code oben wird die folgende Zeile hinzu:</span><span class="sxs-lookup"><span data-stu-id="30cec-208">In the code at the top, add the following line:</span></span>

[!code-csharp[Main](layouts/samples/sample8.cs)]

<span data-ttu-id="30cec-209">Die `Page` Objekt steht für alle *.cshtml* Seiten und ist für diesen Zweck, d. h. zum Freigeben von Informationen zwischen einer Seite und des Layouts.</span><span class="sxs-lookup"><span data-stu-id="30cec-209">The `Page` object is available on all *.cshtml* pages and is for this purpose, namely to share information between a page and its layout.</span></span>

<span data-ttu-id="30cec-210">Öffnen der<em>\_Layout.cshtml</em> Seite.</span><span class="sxs-lookup"><span data-stu-id="30cec-210">Open the<em>\_Layout.cshtml</em> page.</span></span> <span data-ttu-id="30cec-211">Ändern der `<title>` Element so, dass die It wie dieses Markup hinzu:</span><span class="sxs-lookup"><span data-stu-id="30cec-211">Change the `<title>` element so that it looks like this markup:</span></span>

[!code-html[Main](layouts/samples/sample9.html)]

<span data-ttu-id="30cec-212">Dieser Code rendert den Inhalt der `Page.Title` Eigenschaft direkt an dieser Stelle auf der Seite.</span><span class="sxs-lookup"><span data-stu-id="30cec-212">This code renders whatever is in the `Page.Title` property right at that location in the page.</span></span>

<span data-ttu-id="30cec-213">Führen Sie die *Movies.cshtml* Seite.</span><span class="sxs-lookup"><span data-stu-id="30cec-213">Run the *Movies.cshtml* page.</span></span> <span data-ttu-id="30cec-214">Dieses Mal die Registerkarte "Browser" zeigt, was Sie als Wert des übergebenen `Page.Title`:</span><span class="sxs-lookup"><span data-stu-id="30cec-214">This time the browser tab shows what you passed as the value of `Page.Title`:</span></span>

![Eine Browserregisterkarte mit dem Titel, dynamisch erstellt](layouts/_static/image9.png)

<span data-ttu-id="30cec-216">Sie können den Quellcode der Seite im Browser anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="30cec-216">If you want, view the page source in the browser.</span></span> <span data-ttu-id="30cec-217">Sie sehen, dass die `<title>` als Element gerendert wird `<title>List Movies</title>`.</span><span class="sxs-lookup"><span data-stu-id="30cec-217">You can see that the `<title>` element is rendered as `<title>List Movies</title>`.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="30cec-218">**Das Page-Objekt**</span><span class="sxs-lookup"><span data-stu-id="30cec-218">**The Page Object**</span></span>
> 
> <span data-ttu-id="30cec-219">Ein nützliches Feature von `Page` besteht darin, dass es sich um ein dynamisches Objekt ist – die `Title` Eigenschaft ist keine feste oder reservierte Namen.</span><span class="sxs-lookup"><span data-stu-id="30cec-219">A useful feature of `Page` is that it's a dynamic object — the `Title` property is not a fixed or reserved name.</span></span> <span data-ttu-id="30cec-220">Können Sie *alle* für den Wert der `Page` Objekt.</span><span class="sxs-lookup"><span data-stu-id="30cec-220">You can use *any* name for a value of the `Page` object.</span></span> <span data-ttu-id="30cec-221">Beispielsweise Sie können so einfach haben übergeben den Titel mithilfe einer Eigenschaft mit dem Namen `Page.CurrentName` oder `Page.MyPage`.</span><span class="sxs-lookup"><span data-stu-id="30cec-221">For example, you could as easily have passed the title by using a property named `Page.CurrentName` or `Page.MyPage`.</span></span> <span data-ttu-id="30cec-222">Die einzige Einschränkung besteht darin, dass der Name muss den normalen Regeln für welche Eigenschaften genannt werden können.</span><span class="sxs-lookup"><span data-stu-id="30cec-222">The only restriction is that the name has to follow the normal rules for what properties can be named.</span></span> <span data-ttu-id="30cec-223">(Z. B. kann nicht der Name enthalten ein Leerzeichen.)</span><span class="sxs-lookup"><span data-stu-id="30cec-223">(For example, the name can't contain a space.)</span></span>
> 
> <span data-ttu-id="30cec-224">Sie können eine beliebige Anzahl von Werten übergeben, mit der `Page` Objekt.</span><span class="sxs-lookup"><span data-stu-id="30cec-224">You can pass any number of values by using the `Page` object.</span></span> <span data-ttu-id="30cec-225">Falls gewünscht, können Sie die Informationen auf der Seite "Layout" übergeben, können Sie Werte übergeben, indem Sie etwa `Page.MovieTitle` und `Page.Genre` und `Page.MovieYear`.</span><span class="sxs-lookup"><span data-stu-id="30cec-225">If you wanted to pass movie information to the layout page, you could pass values by using something like `Page.MovieTitle` and `Page.Genre` and `Page.MovieYear`.</span></span> <span data-ttu-id="30cec-226">(Oder jeder anderen Namen, die Sie entwickelt, um die Informationen zu speichern.) Die einzige Anforderung besteht, dies ist wahrscheinlich offensichtlich – besteht darin, dass Sie die gleichen Namen in der Seite Inhalt und die Seite "Layout" verwenden.</span><span class="sxs-lookup"><span data-stu-id="30cec-226">(Or any other names that you invented to store the information.) The only requirement — which is probably obvious — is that you have to use the same names in the content page and the layout page.</span></span>
> 
> <span data-ttu-id="30cec-227">Die Informationen, die Sie, indem übergeben die `Page` Objekt ist nicht beschränkt auf nur Text, der auf der Layoutseite angezeigt.</span><span class="sxs-lookup"><span data-stu-id="30cec-227">The information you pass by using the `Page` object isn't limited to just text to display on the layout page.</span></span> <span data-ttu-id="30cec-228">Sie können einen Wert übergeben, um die Seite "Layout", und klicken Sie dann Code auf der Layoutseite kann den Wert entscheiden, ob Sie einen Abschnitt der Seite angezeigt wie *CSS* -Datei, um verwenden und so weiter.</span><span class="sxs-lookup"><span data-stu-id="30cec-228">You can pass a value to the layout page, and then code in the layout page can use the value to decide whether to display a section of the page, what *.css* file to use, and so on.</span></span> <span data-ttu-id="30cec-229">Die Werte, die Sie übergeben die `Page` Objekt werden wie alle anderen Standardwerten Verwendung im Code.</span><span class="sxs-lookup"><span data-stu-id="30cec-229">The values you pass in the `Page` object are like any other values that you use in code.</span></span> <span data-ttu-id="30cec-230">Es ist einfach, dass die Werte aus der Seite Inhalt stammen und die Seite ' Layout ' übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="30cec-230">It's just that the values originate in the content page and are passed to the layout page.</span></span>


<span data-ttu-id="30cec-231">Öffnen der *AddMovie.cshtml* Seite, und fügen Sie eine Zeile am Anfang der Code, der einen Titel für die *AddMovie.cshtml* Seite:</span><span class="sxs-lookup"><span data-stu-id="30cec-231">Open the *AddMovie.cshtml* page and add a line to the top of the code that provides a title for the *AddMovie.cshtml* page:</span></span>

[!code-csharp[Main](layouts/samples/sample10.cs)]

<span data-ttu-id="30cec-232">Führen Sie die *AddMovie.cshtml* Seite.</span><span class="sxs-lookup"><span data-stu-id="30cec-232">Run the *AddMovie.cshtml* page.</span></span> <span data-ttu-id="30cec-233">Sie sehen den neuen Titel vorhanden:</span><span class="sxs-lookup"><span data-stu-id="30cec-233">You see the new title there:</span></span>

![Eine Browserregisterkarte mit der 'Filme hinzufügen' Titel dynamisch erstellt](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a><span data-ttu-id="30cec-235">Aktualisieren die verbleibenden Seiten, um das Layout zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="30cec-235">Updating the Remaining Pages to Use the Layout</span></span>

<span data-ttu-id="30cec-236">Jetzt können Sie die verbleibenden Seiten auf Ihrer Website abschließen, sodass sie das neue Layout verwenden.</span><span class="sxs-lookup"><span data-stu-id="30cec-236">Now you can finish the remaining pages in your site so that they use the new layout.</span></span> <span data-ttu-id="30cec-237">Open *EditMovie.cshtml* und *DeleteMovie.cshtml* wiederum, und stellen Sie die gleichen Änderungen in den einzelnen.</span><span class="sxs-lookup"><span data-stu-id="30cec-237">Open *EditMovie.cshtml* and *DeleteMovie.cshtml* in turn and make the same changes in each.</span></span>

<span data-ttu-id="30cec-238">Fügen Sie die Zeile des Codes, der mit der Layoutseite verknüpft:</span><span class="sxs-lookup"><span data-stu-id="30cec-238">Add the line of code that links to the layout page:</span></span>

[!code-csharp[Main](layouts/samples/sample11.cs)]

<span data-ttu-id="30cec-239">Hinzufügen einer Zeile, um den Titel der Seite festzulegen:</span><span class="sxs-lookup"><span data-stu-id="30cec-239">Add a line to set the title of the page:</span></span>

[!code-csharp[Main](layouts/samples/sample12.cs)]

<span data-ttu-id="30cec-240">oder:</span><span class="sxs-lookup"><span data-stu-id="30cec-240">or:</span></span>

[!code-csharp[Main](layouts/samples/sample13.cs)]

<span data-ttu-id="30cec-241">Entfernen Sie das überflüssige HTML-Markup, lassen Sie nur die Bits, die in sind im Grunde die `<body>` -Element (plus den Codeblock am oberen Rand).</span><span class="sxs-lookup"><span data-stu-id="30cec-241">Remove all the extraneous HTML markup — basically, leave only the bits that are inside the `<body>` element (plus the code block at the top).</span></span>

<span data-ttu-id="30cec-242">Ändern der `<h1>` Elemente werden eine `<h2>` Element.</span><span class="sxs-lookup"><span data-stu-id="30cec-242">Change the `<h1>` element to be an `<h2>` element.</span></span>

<span data-ttu-id="30cec-243">Wenn Sie diese Änderungen vorgenommen haben, testen Sie jede, und stellen Sie sicher, dass es ordnungsgemäß angezeigt wird und dass der Titel richtig ist.</span><span class="sxs-lookup"><span data-stu-id="30cec-243">When you've made these changes, test each and make sure that it's displaying properly and that the title is correct.</span></span>

## <a name="parting-thoughts-about-layout-pages"></a><span data-ttu-id="30cec-244">Trennflächen Ideen Layoutseiten</span><span class="sxs-lookup"><span data-stu-id="30cec-244">Parting Thoughts About Layout Pages</span></span>

<span data-ttu-id="30cec-245">In diesem Tutorial, in dem Sie erstellt haben eine  *\_Layout.cshtml* Seite und verwendet die `RenderBody` Methode, um Inhalte von einer anderen Seite zusammengeführt.</span><span class="sxs-lookup"><span data-stu-id="30cec-245">In this tutorial you created a *\_Layout.cshtml* page and used the `RenderBody` method to merge content from another page.</span></span> <span data-ttu-id="30cec-246">Dies ist das grundlegende Muster für die Verwendung von Layouts in Webseiten.</span><span class="sxs-lookup"><span data-stu-id="30cec-246">That's the basic pattern for using layouts in Web Pages.</span></span>

<span data-ttu-id="30cec-247">Layoutseiten haben zusätzliche Funktionen, die hier nicht behandelt.</span><span class="sxs-lookup"><span data-stu-id="30cec-247">Layout pages have additional features that we didn't cover here.</span></span> <span data-ttu-id="30cec-248">Sie können z. B. Layoutseiten schachteln – eine Layoutseite kann wiederum eine andere verweisen.</span><span class="sxs-lookup"><span data-stu-id="30cec-248">For example, you can nest layout pages — one layout page can in turn reference another.</span></span> <span data-ttu-id="30cec-249">Geschachtelte Layouts ist hilfreich, wenn Sie mit einer Website in den Unterabschnitten arbeiten, die unterschiedliche Layouts erfordern.</span><span class="sxs-lookup"><span data-stu-id="30cec-249">Nested layouts can be useful if you're working with subsections of a site that require different layouts.</span></span> <span data-ttu-id="30cec-250">Sie können auch zusätzliche Methoden verwenden (z. B. `RenderSection`) einrichten, mit dem Namen Abschnitte der Layoutseite.</span><span class="sxs-lookup"><span data-stu-id="30cec-250">You can also use additional methods (for example, `RenderSection`) to set up named sections in the layout page.</span></span>

<span data-ttu-id="30cec-251">Die Kombination der Layoutseiten, und *CSS* Dateien ist leistungsstark.</span><span class="sxs-lookup"><span data-stu-id="30cec-251">The combination of layout pages and *.css* files is powerful.</span></span> <span data-ttu-id="30cec-252">Wie Sie in den nächsten Lernprogramm sehen werden, in WebMatrix Sie erstellen eine Website auf Grundlage einer *Vorlage*, bietet Ihnen einen Standort vorgefertigter Funktionalität darin.</span><span class="sxs-lookup"><span data-stu-id="30cec-252">As you'll see in the next tutorial series, in WebMatrix you can create a site based on a *template*, which gives you a site that has prebuilt functionality in it.</span></span> <span data-ttu-id="30cec-253">Die Vorlagen, Layoutseiten und CSS zum Erstellen von Websites, die toll Aussehen und deren Features wie Menüs, nutzen.</span><span class="sxs-lookup"><span data-stu-id="30cec-253">The templates make good use of layout pages and CSS to create sites that look great and that have features like menus.</span></span> <span data-ttu-id="30cec-254">Hier ist ein Screenshot der Startseite von einer Website auf Grundlage einer Vorlage, die mit Funktionen, die Layoutseiten und CSS verwenden:</span><span class="sxs-lookup"><span data-stu-id="30cec-254">Here's a screenshot of the home page from a site based on a template, showing features that use layout pages and CSS:</span></span>

![Layout von WebMatrix-Websitevorlage, zeigt der Header, im Navigationsbereich, Inhaltsbereich, Optionaler Abschnitt und Anmeldung Links erstellt](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a><span data-ttu-id="30cec-256">Vollständige Liste für Movie-Seite (wird aktualisiert, um eine Layoutseite verwenden)</span><span class="sxs-lookup"><span data-stu-id="30cec-256">Complete Listing for Movie Page (Updated to Use a Layout Page)</span></span>

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a><span data-ttu-id="30cec-257">Auf der Seite abgeschlossen für hinzufügen Movie-Seite (für das Layout aktualisiert)</span><span class="sxs-lookup"><span data-stu-id="30cec-257">Complete Page Listing for Add Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a><span data-ttu-id="30cec-258">Auf der Seite abgeschlossen-Eintrag für den Film "löschen" (für das Layout aktualisiert)</span><span class="sxs-lookup"><span data-stu-id="30cec-258">Complete Page Listing for Delete Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a><span data-ttu-id="30cec-259">Führen Sie die Liste der Seite für Seite-Film "Edit" (für das Layout aktualisiert)</span><span class="sxs-lookup"><span data-stu-id="30cec-259">Complete Page Listing for Edit Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a><span data-ttu-id="30cec-260">Als Nächstes kommen</span><span class="sxs-lookup"><span data-stu-id="30cec-260">Coming Up Next</span></span>

<span data-ttu-id="30cec-261">Im nächsten Tutorial erfahren Sie, wie Sie Ihre Website mit dem Internet zu veröffentlichen, sodass jeder sie sehen kann.</span><span class="sxs-lookup"><span data-stu-id="30cec-261">In the next tutorial, you'll learn how to publish your site to the Internet so everyone can see it.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="30cec-262">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="30cec-262">Additional Resources</span></span>

- <span data-ttu-id="30cec-263">[Erstellen einer konsistenten aussehen](https://go.microsoft.com/fwlink/?LinkID=202891) – ein Artikel, die weitere Details zum Arbeiten mit Layouts bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="30cec-263">[Creating a Consistent Look](https://go.microsoft.com/fwlink/?LinkID=202891) — An article that provides some more detail on working with layouts.</span></span> <span data-ttu-id="30cec-264">Es wird beschrieben, wie einen Wert an einer Layoutseite zu übergeben, die zeigt, oder Blendet einen Teil des Inhalts.</span><span class="sxs-lookup"><span data-stu-id="30cec-264">It also describes how to pass a value to a layout page that shows or hides some of the content.</span></span>
- <span data-ttu-id="30cec-265">[Geschachtelte Layoutseiten mit Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) – Mike Brind Blogs verdeutlicht, wie Sie Layoutseiten schachteln.</span><span class="sxs-lookup"><span data-stu-id="30cec-265">[Nested Layout Pages with Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) — Mike Brind blogs an example of how to nest layout pages.</span></span> <span data-ttu-id="30cec-266">(Umfasst ein Download der Seiten.)</span><span class="sxs-lookup"><span data-stu-id="30cec-266">(Includes a download of the pages.)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="30cec-267">[Zurück](deleting-data.md)
> [Weiter](publishing.md)</span><span class="sxs-lookup"><span data-stu-id="30cec-267">[Previous](deleting-data.md)
[Next](publishing.md)</span></span>
