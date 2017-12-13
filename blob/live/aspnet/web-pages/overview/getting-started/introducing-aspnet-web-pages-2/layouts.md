---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: "Einführung in ASP.NET Web Pages – erstellen ein konsistentes Layout | Microsoft Docs"
author: tfitzmac
description: "Dieses Lernprogramm veranschaulicht die Layouts verwenden, um ein konsistentes Erscheinungsbild für die Seiten auf einer Website zu erstellen, die ASP.NET Web Pages verwendet. Es wird vorausgesetzt, Sie haben die..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: 692adc5a03892f27c91fe8868c8eab6ce08f49cd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a><span data-ttu-id="d3d05-104">Einführung in ASP.NET Web Pages - erstellen ein konsistentes Layout</span><span class="sxs-lookup"><span data-stu-id="d3d05-104">Introducing ASP.NET Web Pages - Creating a Consistent Layout</span></span>
====================
<span data-ttu-id="d3d05-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d3d05-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d3d05-106">Dieses Lernprogramm veranschaulicht das verwenden *Layouts* ein konsistentes Erscheinungsbild für die Seiten auf einer Website zu erstellen, die ASP.NET Web Pages verwendet.</span><span class="sxs-lookup"><span data-stu-id="d3d05-106">This tutorial shows you how to use *layouts* to create a consistent look for the pages on a site that uses ASP.NET Web Pages.</span></span> <span data-ttu-id="d3d05-107">Es wird vorausgesetzt, Sie haben die Reihe über abgeschlossen [Löschen von Datenbankdaten in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251584).</span><span class="sxs-lookup"><span data-stu-id="d3d05-107">It assumes you have completed the series through [Deleting Database Data in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251584).</span></span>
> 
> <span data-ttu-id="d3d05-108">Lernen Sie:</span><span class="sxs-lookup"><span data-stu-id="d3d05-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="d3d05-109">Welche eine Layoutseite ist.</span><span class="sxs-lookup"><span data-stu-id="d3d05-109">What a layout page is.</span></span>
> - <span data-ttu-id="d3d05-110">Wie Sie Layoutseiten mit dynamischen Inhalten zu kombinieren.</span><span class="sxs-lookup"><span data-stu-id="d3d05-110">How to combine layout pages with dynamic content.</span></span>
> - <span data-ttu-id="d3d05-111">Zum Übergeben von Werten an einer Layoutseite.</span><span class="sxs-lookup"><span data-stu-id="d3d05-111">How to pass values to a layout page.</span></span>


## <a name="about-layouts"></a><span data-ttu-id="d3d05-112">Informationen zu Layouts</span><span class="sxs-lookup"><span data-stu-id="d3d05-112">About Layouts</span></span>

<span data-ttu-id="d3d05-113">Die Seiten, die Sie bisher erstellt haben alle wurden abgeschlossen ist, eigenständige Seiten.</span><span class="sxs-lookup"><span data-stu-id="d3d05-113">The pages you've created so far have all been complete, standalone pages.</span></span> <span data-ttu-id="d3d05-114">Sie gehören alle für denselben Standort, sie jedoch keine gemeinsame Elemente oder ein standard aussehen.</span><span class="sxs-lookup"><span data-stu-id="d3d05-114">They all belong to the same site, but they don't have any common elements or a standard look.</span></span>

<span data-ttu-id="d3d05-115">Die meisten Standorte haben ein konsistentes Erscheinungsbild und das Layout.</span><span class="sxs-lookup"><span data-stu-id="d3d05-115">Most sites do have a consistent look and layout.</span></span> <span data-ttu-id="d3d05-116">Angenommen, Sie zum Wechseln der [Microsoft.com](https://www.microsoft.com/web/) Standort- und sich mit der Registerkarte, sehen Sie, dass alle Seiten eines allgemeinen Layouts und eine visuelle Design entsprechen:</span><span class="sxs-lookup"><span data-stu-id="d3d05-116">For example, if you go to the [Microsoft.com/web](https://www.microsoft.com/web/) site and look around, you see that the pages all adhere to an overall layout and to a visual theme:</span></span>

![Zeigt das Layout der Header, Navigationsbereich, im Inhaltsbereich und die Fußzeile Microsoft.com-Website](layouts/_static/image1.png)

<span data-ttu-id="d3d05-118">Ein *ineffizient* Erstellen dieses Layouts wäre, einen Header, die Navigationsleiste und die Fußzeile auf allen Seiten separat definieren.</span><span class="sxs-lookup"><span data-stu-id="d3d05-118">An *inefficient* way to create this layout would be to define a header, navigation bar, and footer separately on each of your pages.</span></span> <span data-ttu-id="d3d05-119">Werden, würden Sie das gleiche Markup duplizieren.</span><span class="sxs-lookup"><span data-stu-id="d3d05-119">You'd be duplicating the same markup each time.</span></span> <span data-ttu-id="d3d05-120">Wenn Sie etwas ändern möchten (z. B. aktualisieren die Fußzeile), müssten Sie jede Seite separat zu ändern.</span><span class="sxs-lookup"><span data-stu-id="d3d05-120">If you wanted to change something (for example, update the footer), you'd have to change each page separately.</span></span>

<span data-ttu-id="d3d05-121">Wo ist *Layoutseiten* in stammen.</span><span class="sxs-lookup"><span data-stu-id="d3d05-121">That's where *layout pages* come in.</span></span> <span data-ttu-id="d3d05-122">In ASP.NET Web Pages können Sie eine Layoutseite definieren, die einen allgemeinen Container für Seiten auf Ihrer Website bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="d3d05-122">In ASP.NET Web Pages, you can define a layout page that provides an overall container for pages on your site.</span></span> <span data-ttu-id="d3d05-123">Die Seite "Layout" kann z. B. Header, Navigationsbereich und Fußzeile enthalten.</span><span class="sxs-lookup"><span data-stu-id="d3d05-123">For example, the layout page can contain the header, navigation area, and footer.</span></span> <span data-ttu-id="d3d05-124">Die Seite "Layout" enthält einen Platzhalter, wohin der Hauptinhalt gehen.</span><span class="sxs-lookup"><span data-stu-id="d3d05-124">The layout page includes a placeholder where the main content goes.</span></span>

<span data-ttu-id="d3d05-125">Sie können dann einzelne Inhaltsseiten definieren, die das Markup und den Code nur auf dieser Seite enthalten.</span><span class="sxs-lookup"><span data-stu-id="d3d05-125">You can then define individual content pages that contain the markup and the code for only that page.</span></span> <span data-ttu-id="d3d05-126">Inhaltsseiten keine vollständige HTML-Seiten werden; Sie müssen noch nicht einmal haben eine `<body>` Element.</span><span class="sxs-lookup"><span data-stu-id="d3d05-126">Content pages don't have to be complete HTML pages; they don't even have to have a `<body>` element.</span></span> <span data-ttu-id="d3d05-127">Sie haben außerdem eine Codezeile, die ASP.NET mitteilt, welche Layoutseite den Inhalt angezeigt werden soll.</span><span class="sxs-lookup"><span data-stu-id="d3d05-127">They also have a line of code that tells ASP.NET what layout page you want to display the content in.</span></span> <span data-ttu-id="d3d05-128">Hier ist ein Bild, das etwa zeigt die Funktionsweise dieser Beziehung:</span><span class="sxs-lookup"><span data-stu-id="d3d05-128">Here's a picture that shows roughly how this relationship works:</span></span>

![Konzeptionelles Diagramm zeigt zwei Inhaltsseiten und einer Layoutseite, in der sie passen](layouts/_static/image2.png)

<span data-ttu-id="d3d05-130">Diese Aktivität ist einfach, zu verstehen, wenn Sie in Aktion zu sehen.</span><span class="sxs-lookup"><span data-stu-id="d3d05-130">This interaction is easy to understand when you see it in action.</span></span> <span data-ttu-id="d3d05-131">In diesem Lernprogramm ändern Sie Ihre Seiten Filme, um ein Layout zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="d3d05-131">In this tutorial, you'll change your movies pages to use a layout.</span></span>

## <a name="adding-a-layout-page"></a><span data-ttu-id="d3d05-132">Hinzufügen einer Layoutseite</span><span class="sxs-lookup"><span data-stu-id="d3d05-132">Adding a Layout Page</span></span>

<span data-ttu-id="d3d05-133">Beginnen Sie durch das Erstellen einer Layoutseite, die eine typische Seitenlayout mit einem Header, Footer und einen Bereich für den Hauptinhalt definiert.</span><span class="sxs-lookup"><span data-stu-id="d3d05-133">You'll start by creating a layout page that defines a typical page layout with a header, footer, and an area for the main content.</span></span> <span data-ttu-id="d3d05-134">Fügen Sie am Standort WebPagesMovies eine CSHTML-Seite, die mit dem Namen  *\_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d3d05-134">In the WebPagesMovies site, add a CSHTML page named *\_Layout.cshtml*.</span></span>

<span data-ttu-id="d3d05-135">Der führende Unterstrich ( `_` ) Zeichen ist von Bedeutung.</span><span class="sxs-lookup"><span data-stu-id="d3d05-135">The leading underscore ( `_` ) character is significant.</span></span> <span data-ttu-id="d3d05-136">Wenn eine Seite-Name mit einem Unterstrich beginnt, wird nicht ASP.NET diese Seite nicht direkt an den Browser gesendet.</span><span class="sxs-lookup"><span data-stu-id="d3d05-136">If a page's name starts with an underscore, ASP.NET won't directly send that page to the browser.</span></span> <span data-ttu-id="d3d05-137">Diese Konvention können Sie Seiten zu definieren, die für Ihre Website erforderlich sind, aber diese Benutzer direkt anfordern werden sollten.</span><span class="sxs-lookup"><span data-stu-id="d3d05-137">This convention lets you define pages that are required for your site but that users shouldn't be able to request directly.</span></span>

<span data-ttu-id="d3d05-138">Der Inhalt auf der Seite durch Folgendes ersetzt:</span><span class="sxs-lookup"><span data-stu-id="d3d05-138">Replace the content in the page with the following:</span></span>

[!code-html[Main](layouts/samples/sample1.html)]

<span data-ttu-id="d3d05-139">Wie Sie sehen können, ist dieses Markup nur HTML-Code, der verwendet `<div>` Elemente so definieren Sie drei Abschnitte in der Seite plus eins mehr `<div>` Element, das drei Abschnitte enthalten.</span><span class="sxs-lookup"><span data-stu-id="d3d05-139">As you can see, this markup is just HTML that uses `<div>` elements to define three sections in the page plus one more `<div>` element to hold the three sections.</span></span> <span data-ttu-id="d3d05-140">Die Fußzeile der enthält einem gewissen Grad Razor-Code: `@DateTime.Now.Year`, dem wird das aktuelle Jahr an dieser Stelle auf der Seite gerendert.</span><span class="sxs-lookup"><span data-stu-id="d3d05-140">The footer contains a bit of Razor code: `@DateTime.Now.Year`, which will render the current year at that location in the page.</span></span>

<span data-ttu-id="d3d05-141">Beachten Sie, dass es ein Link zu einem Stylesheet, mit dem Namen *Movies.css*.</span><span class="sxs-lookup"><span data-stu-id="d3d05-141">Notice that there's a link to a style sheet named *Movies.css*.</span></span> <span data-ttu-id="d3d05-142">Das Stylesheet ist, in dem die Details des physischen Layouts der Elemente definiert werden.</span><span class="sxs-lookup"><span data-stu-id="d3d05-142">The style sheet is where the details of the physical layout of the elements will be defined.</span></span> <span data-ttu-id="d3d05-143">Sie erstellen, die in wenigen Augenblicken.</span><span class="sxs-lookup"><span data-stu-id="d3d05-143">You'll create that in a moment.</span></span>

<span data-ttu-id="d3d05-144">Die einzigen ungewöhnlichen-Funktion in dieser  *\_Layout.cshtml* Seite ist die `@Render.Body()` Zeile.</span><span class="sxs-lookup"><span data-stu-id="d3d05-144">The only unusual feature in this *\_Layout.cshtml* page is the `@Render.Body()` line.</span></span> <span data-ttu-id="d3d05-145">Dies ist der Platzhalter, wo der Inhalt für den dieses Layout mit einer anderen Seite zusammengeführt wird.</span><span class="sxs-lookup"><span data-stu-id="d3d05-145">That's the placeholder where the content will go when this layout is merged with another page.</span></span>

## <a name="adding-a-css-file"></a><span data-ttu-id="d3d05-146">Hinzufügen einer CSS-Datei</span><span class="sxs-lookup"><span data-stu-id="d3d05-146">Adding a .css File</span></span>

<span data-ttu-id="d3d05-147">Die bevorzugte Methode zum Definieren der tatsächlichen Anordnung (d. h. Darstellung) von Elementen auf der Seite ist die Verwendung von cascading Stylesheet (CSS)-Regeln.</span><span class="sxs-lookup"><span data-stu-id="d3d05-147">The preferred way to define the actual arrangement (that is, appearance) of elements on the page is to use cascading style sheet (CSS) rules.</span></span> <span data-ttu-id="d3d05-148">So Sie erstellen eine *CSS* Datei, die die Regeln für das neue Layout verfügt.</span><span class="sxs-lookup"><span data-stu-id="d3d05-148">So you'll create a *.css* file that has the rules for your new layout.</span></span>

<span data-ttu-id="d3d05-149">Wählen Sie in WebMatrix Stammverzeichnis der Website ein.</span><span class="sxs-lookup"><span data-stu-id="d3d05-149">In WebMatrix, select the root of your site.</span></span> <span data-ttu-id="d3d05-150">Klicken Sie dann in der **Dateien** Registerkarte des Menübands, klicken Sie auf den Pfeil unter der **neu** Schaltfläche, und klicken Sie dann auf **neuer Ordner**.</span><span class="sxs-lookup"><span data-stu-id="d3d05-150">Then in the **Files** tab of the ribbon, click the arrow under the **New** button and then click **New Folder**.</span></span>

![Die Option "Neuer Ordner" unter neu im Menüband.](layouts/_static/image3.png)

<span data-ttu-id="d3d05-152">Nennen Sie diesen Ordner *Stile*.</span><span class="sxs-lookup"><span data-stu-id="d3d05-152">Name the new folder *Styles*.</span></span>

![Benennen den neuen Ordner "Formatvorlagen"](layouts/_static/image4.png)

<span data-ttu-id="d3d05-154">In der neuen *Stile* Ordner, erstellen Sie eine Datei mit dem Namen *Movies.css*.</span><span class="sxs-lookup"><span data-stu-id="d3d05-154">Inside the new *Styles* folder, create a file named *Movies.css*.</span></span>

![Erstellen einer neuen Movies.css-Datei](layouts/_static/image5.png)

<span data-ttu-id="d3d05-156">Ersetzen Sie den Inhalt der neuen *CSS* Datei durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="d3d05-156">Replace the contents of the new *.css* file with the following:</span></span>

[!code-css[Main](layouts/samples/sample2.css)]

<span data-ttu-id="d3d05-157">Es wird nicht viel über diese CSS-Regeln ausgenommen, beachten zwei Dinge angenommen.</span><span class="sxs-lookup"><span data-stu-id="d3d05-157">We won't say much about these CSS rules, except to note two things.</span></span> <span data-ttu-id="d3d05-158">Eine ist, dass zusätzlich zum Festlegen von Schriftarten und Größen, die Regeln absolute Positionierung, verwenden um den Speicherort der Header, Footer und Hauptinhaltsbereich einzurichten.</span><span class="sxs-lookup"><span data-stu-id="d3d05-158">One is that in addition to setting fonts and sizes, the rules use absolute positioning to establish the location of the header, footer, and main content area.</span></span> <span data-ttu-id="d3d05-159">Wenn Sie auf die Positionierung in CSS vertraut sind, erfahren Sie die [CSS-Positionierung](http://www.w3schools.com/css/css_positioning.asp) am Standort W3Schools Tutorial.</span><span class="sxs-lookup"><span data-stu-id="d3d05-159">If you're new to positioning in CSS, you can read the [CSS Positioning](http://www.w3schools.com/css/css_positioning.asp) tutorial at the W3Schools site.</span></span>

<span data-ttu-id="d3d05-160">Die andere Aufgabe zu beachten ist, dass am unteren finden wir die Stilregeln kopiert haben, die ursprünglich definiert einzeln in die *Movies.cshtml* Datei.</span><span class="sxs-lookup"><span data-stu-id="d3d05-160">The other thing to note is that at the bottom, we've copied the style rules that were originally defined individually in the *Movies.cshtml* file.</span></span> <span data-ttu-id="d3d05-161">Diese Regeln verwendet wurden, der [Einführung zum Anzeigen von Daten durch Verwenden von ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251580) Lernprogramm aus, um stellen die `WebGrid` Helper Rendern von Markup, das der Tabelle verteilt hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="d3d05-161">These rules were used in the [Introduction to Displaying Data by Using ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251580) tutorial to make the `WebGrid` helper render markup that added stripes to the table.</span></span> <span data-ttu-id="d3d05-162">(Wenn Sie im Begriff sind, verwenden Sie eine *CSS* Datei für Formaten, können Sie auch Formatierungsregeln für die gesamte Website darin einfügen.)</span><span class="sxs-lookup"><span data-stu-id="d3d05-162">(If you're going to use a *.css* file for style definitions, you might as well put the style rules for the whole site in it.)</span></span>

## <a name="updating-the-movies-file-to-use-the-layout"></a><span data-ttu-id="d3d05-163">Aktualisieren die Filme-Datei, um das Layout verwenden</span><span class="sxs-lookup"><span data-stu-id="d3d05-163">Updating the Movies File to Use the Layout</span></span>

<span data-ttu-id="d3d05-164">Jetzt können Sie die vorhandenen Dateien an Ihrem Standort verwenden Sie das neue Layout aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="d3d05-164">Now you can update the existing files in your site to use the new layout.</span></span> <span data-ttu-id="d3d05-165">Öffnen der *Movies.cshtml* Datei.</span><span class="sxs-lookup"><span data-stu-id="d3d05-165">Open the *Movies.cshtml* file.</span></span> <span data-ttu-id="d3d05-166">Im oberen Bereich als erste Zeile des Codes, fügen Sie Folgendes hinzu:</span><span class="sxs-lookup"><span data-stu-id="d3d05-166">At the top, as the first line of code, add the following:</span></span>

[!code-csharp[Main](layouts/samples/sample3.cs)]

<span data-ttu-id="d3d05-167">Die Seite beginnt jetzt stützen sich auf:</span><span class="sxs-lookup"><span data-stu-id="d3d05-167">The page now starts out this way:</span></span>

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

<span data-ttu-id="d3d05-168">Diese eine Codezeile Weise wird ASP.NET informiert, die bei der *Filme* Seite ausgeführt wird, sollte er mit zusammengeführt werden die  *\_Layout.cshtml* Datei.</span><span class="sxs-lookup"><span data-stu-id="d3d05-168">This one line of code tells ASP.NET that when the *Movies* page runs, it should be merged with the *\_Layout.cshtml* file.</span></span>

<span data-ttu-id="d3d05-169">Da die *Movies.cshtml* Datei jetzt eine Layoutseite verwendet, können Sie das Markup Entfernen der *Movies.cshtml* Seite, die kümmert hat die  *\_Layout.cshtml*Datei.</span><span class="sxs-lookup"><span data-stu-id="d3d05-169">Since the *Movies.cshtml* file now uses a layout page, you can remove the markup from the *Movies.cshtml* page that's taken care of by the *\_Layout.cshtml* file.</span></span> <span data-ttu-id="d3d05-170">Entnehmen Sie der `<!DOCTYPE>`, `<html>`, und `<body>` Start- und Endtags.</span><span class="sxs-lookup"><span data-stu-id="d3d05-170">Take out the `<!DOCTYPE>`, `<html>`, and `<body>` opening and closing tags.</span></span> <span data-ttu-id="d3d05-171">Entnehmen Sie den gesamten `<head>` Element und dessen Inhalt, die Formatierungsregeln für das Raster enthält, da Sie nun diese Regeln haben einer *CSS* Datei.</span><span class="sxs-lookup"><span data-stu-id="d3d05-171">Take out the entire `<head>` element and its contents, which includes the style rules for the grid, since you've now got those rules in a *.css* file.</span></span> <span data-ttu-id="d3d05-172">Während Sie sich befinden, ändern Sie den vorhandenen `<h1>` Element ein `<h2>` Element; Sie müssen ein `<h1>` Element im Layoutseite bereits.</span><span class="sxs-lookup"><span data-stu-id="d3d05-172">While you're at it, change the existing `<h1>` element to an `<h2>` element; you have an `<h1>` element in the layout page already.</span></span> <span data-ttu-id="d3d05-173">Ändern der `<h2>` Text "Liste Filme".</span><span class="sxs-lookup"><span data-stu-id="d3d05-173">Change the `<h2>` text to "List Movies".</span></span>

<span data-ttu-id="d3d05-174">Normalerweise würde nicht müssen Sie diese Arten von Änderungen in einer Inhaltsseite zu machen.</span><span class="sxs-lookup"><span data-stu-id="d3d05-174">Normally you wouldn't have to make these sorts of changes in a content page.</span></span> <span data-ttu-id="d3d05-175">Wenn Sie Ihre Website mit einer Layoutseite fangen, erstellen Sie zunächst Inhaltsseiten ohne all diese Elemente.</span><span class="sxs-lookup"><span data-stu-id="d3d05-175">When you start your site out with a layout page, you create content pages without all these elements to begin with.</span></span> <span data-ttu-id="d3d05-176">In diesem Fall sind jedoch eine Seite "eigenständige" in eine konvertiert, die ein Layout verwendet werden, es gibt also ein Teil der Bereinigung.</span><span class="sxs-lookup"><span data-stu-id="d3d05-176">In this case, though, you're converting a standalone page to one that uses a layout, so there's a bit of cleanup.</span></span>

<span data-ttu-id="d3d05-177">Wenn Sie fertig sind, sind die *Movies.cshtml* Seite wird wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="d3d05-177">When you're finished, the *Movies.cshtml* page will look like the following:</span></span>

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a><span data-ttu-id="d3d05-178">Testen das Layout</span><span class="sxs-lookup"><span data-stu-id="d3d05-178">Testing the Layout</span></span>

<span data-ttu-id="d3d05-179">Jetzt können Sie sehen, wie das Layout aussieht.</span><span class="sxs-lookup"><span data-stu-id="d3d05-179">Now you can see what the layout looks like.</span></span> <span data-ttu-id="d3d05-180">WebMatrix, mit der Maustaste die *Movies.cshtml* Seite und wählen Sie **in Browser starten**.</span><span class="sxs-lookup"><span data-stu-id="d3d05-180">In WebMatrix, right-click the *Movies.cshtml* page and select **Launch in browser**.</span></span> <span data-ttu-id="d3d05-181">Wenn der Browser die Seite angezeigt wird, sieht er auf dieser Seite:</span><span class="sxs-lookup"><span data-stu-id="d3d05-181">When the browser displays the page, it looks like this page:</span></span>

![Filme Seite gerendert wird, verwenden ein layout](layouts/_static/image6.png)

<span data-ttu-id="d3d05-183">ASP.NET zusammengeführt hat den Inhalt der Seite "Movies.cshtml" in der  *\_Layout.cshtml* Seite rechts, wobei die `RenderBody` Methode ist.</span><span class="sxs-lookup"><span data-stu-id="d3d05-183">ASP.NET has merged the content of the Movies.cshtml page into the *\_Layout.cshtml* page right where the `RenderBody` method is.</span></span> <span data-ttu-id="d3d05-184">Und natürlich die  *\_Layout.cshtml* Seite Verweise ein *CSS* Datei, die das Aussehen der Seite definiert.</span><span class="sxs-lookup"><span data-stu-id="d3d05-184">And of course the *\_Layout.cshtml* page references a *.css* file that defines the look of the page.</span></span>

## <a name="updating-the-addmovie-page-to-use-the-layout"></a><span data-ttu-id="d3d05-185">Aktualisieren die AddMovie-Seite, um das Layout verwenden</span><span class="sxs-lookup"><span data-stu-id="d3d05-185">Updating the AddMovie Page to Use the Layout</span></span>

<span data-ttu-id="d3d05-186">Der echte Vorteil des Layouts ist, dass Sie sie für alle Seiten auf Ihrer Website verwenden können.</span><span class="sxs-lookup"><span data-stu-id="d3d05-186">The real benefit of layouts is that you can use them for all the pages in your site.</span></span> <span data-ttu-id="d3d05-187">Öffnen der *AddMovie.cshtml* Seite.</span><span class="sxs-lookup"><span data-stu-id="d3d05-187">Open the *AddMovie.cshtml* page.</span></span>

<span data-ttu-id="d3d05-188">Sie möglicherweise berücksichtigen, dass die *AddMovie.cshtml* Seite hatte ursprünglich einige CSS-Regeln, die das Aussehen von Überprüfungsfehlermeldungen definieren.</span><span class="sxs-lookup"><span data-stu-id="d3d05-188">You might remember that the *AddMovie.cshtml* page originally had some CSS rules in it to define the look of validation error messages.</span></span> <span data-ttu-id="d3d05-189">Da Sie haben eine *CSS* -Datei für Ihre Website jetzt, Sie können diese Regeln zum Verschieben der *CSS* Datei.</span><span class="sxs-lookup"><span data-stu-id="d3d05-189">Since you have a *.css* file for your site now, you can move those rules to the *.css* file.</span></span> <span data-ttu-id="d3d05-190">Entfernen Sie sie aus der *AddMovie.cshtml* Datei, und fügen Sie sie am unteren Rand der *Movies.css* Datei.</span><span class="sxs-lookup"><span data-stu-id="d3d05-190">Remove them from the *AddMovie.cshtml* file and add them to the bottom of the *Movies.css* file.</span></span> <span data-ttu-id="d3d05-191">Verschieben Sie die folgenden Regeln:</span><span class="sxs-lookup"><span data-stu-id="d3d05-191">You are moving the following rules:</span></span>

[!code-css[Main](layouts/samples/sample6.css)]

<span data-ttu-id="d3d05-192">Jetzt stellen die gleichen Arten von Änderungen in *AddMovie.cshtml* , die Sie für *Movies.cshtml* – hinzufügen `Layout="~/_Layout.cshtml;` , und entfernen Sie das HTML-Markup, das nun irrelevante ist.</span><span class="sxs-lookup"><span data-stu-id="d3d05-192">Now make the same sorts of changes in *AddMovie.cshtml* that you did for *Movies.cshtml* — add `Layout="~/_Layout.cshtml;` and remove the HTML markup that's now extraneous.</span></span> <span data-ttu-id="d3d05-193">Ändern Sie das `<h1>`-Element in `<h2>`.</span><span class="sxs-lookup"><span data-stu-id="d3d05-193">Change the `<h1>` element to `<h2>`.</span></span> <span data-ttu-id="d3d05-194">Wenn Sie fertig sind, wird die Seite wie im folgenden Beispiel aussehen:</span><span class="sxs-lookup"><span data-stu-id="d3d05-194">When you're done, the page will look like this example:</span></span>

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

<span data-ttu-id="d3d05-195">Führen Sie die Seite.</span><span class="sxs-lookup"><span data-stu-id="d3d05-195">Run the page.</span></span> <span data-ttu-id="d3d05-196">Jetzt sieht es wie in dieser Abbildung aus:</span><span class="sxs-lookup"><span data-stu-id="d3d05-196">Now it looks like this illustration:</span></span>

!['Add Filme' Seite gerendert wird, verwenden ein layout](layouts/_static/image7.png)

<span data-ttu-id="d3d05-198">Die Seiten der Website ähnliche Änderungen vornehmen möchten – *EditMovie.cshtml* und *DeleteMovie.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d3d05-198">You want to make similar changes to the pages in the site — *EditMovie.cshtml* and *DeleteMovie.cshtml*.</span></span> <span data-ttu-id="d3d05-199">Jedoch, bevor Sie dies tun, können Sie eine andere Änderung des Layouts vornehmen, die etwas flexibler vereinfacht.</span><span class="sxs-lookup"><span data-stu-id="d3d05-199">However, before you do, you can make another change to the layout that makes it a little more flexible.</span></span>

## <a name="passing-title-information-to-the-layout-page"></a><span data-ttu-id="d3d05-200">Übergeben von Informationen zu Softwaretiteln auf der Seite "Layout"</span><span class="sxs-lookup"><span data-stu-id="d3d05-200">Passing Title Information to the Layout Page</span></span>

<span data-ttu-id="d3d05-201">Die  *\_Layout.cshtml* Seite, die Sie erstellt hat eine `<title>` -Element, das auf "Mein Filmwebsite" festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="d3d05-201">The *\_Layout.cshtml* page that you created has a `<title>` element that's set to "My Movie Site".</span></span> <span data-ttu-id="d3d05-202">Die meisten Browser zeigt den Inhalt dieses Elements als Text auf einer Registerkarte:</span><span class="sxs-lookup"><span data-stu-id="d3d05-202">Most browsers display the content of this element as the text on a tab:</span></span>

![Der Seite &lt;Titel&gt; auf einer Browserregisterkarte angezeigten Elemente](layouts/_static/image8.png)

<span data-ttu-id="d3d05-204">Diese Informationen zu Softwaretiteln ist generisch.</span><span class="sxs-lookup"><span data-stu-id="d3d05-204">This title information is generic.</span></span> <span data-ttu-id="d3d05-205">Nehmen Sie an, dass das Titeltext der aktuellen Seite werden soll.</span><span class="sxs-lookup"><span data-stu-id="d3d05-205">Suppose that you want the title text to be more specific to the current page.</span></span> <span data-ttu-id="d3d05-206">(Der Titeltext wird auch von Suchmaschinen verwendet um zu bestimmen, was über die Seite ist.) Sie können Informationen aus einer Inhaltsseite wie übergeben *Movies.cshtml* oder *AddMovie.cshtml* auf der Seite "Layout" und dann diese Informationen, um anpassen, welche Layoutseite rendert.</span><span class="sxs-lookup"><span data-stu-id="d3d05-206">(The title text is also used by search engines to determine what your page is about.) You can pass information from a content page like *Movies.cshtml* or *AddMovie.cshtml* to the layout page, and then use that information to customize what the layout page renders.</span></span>

<span data-ttu-id="d3d05-207">Öffnen der *Movies.cshtml* Seite erneut.</span><span class="sxs-lookup"><span data-stu-id="d3d05-207">Open the *Movies.cshtml* page again.</span></span> <span data-ttu-id="d3d05-208">Fügen Sie im Code oben die folgende Zeile hinzu:</span><span class="sxs-lookup"><span data-stu-id="d3d05-208">In the code at the top, add the following line:</span></span>

[!code-csharp[Main](layouts/samples/sample8.cs)]

<span data-ttu-id="d3d05-209">Die `Page` Objekt steht auf allen *cshtml* Seiten und ist für diesen Zweck, d. h. Informationen zwischen einer Seite und das zugehörige Layout gemeinsam verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="d3d05-209">The `Page` object is available on all *.cshtml* pages and is for this purpose, namely to share information between a page and its layout.</span></span>

<span data-ttu-id="d3d05-210">Öffnen der*\_Layout.cshtml* Seite.</span><span class="sxs-lookup"><span data-stu-id="d3d05-210">Open the*\_Layout.cshtml* page.</span></span> <span data-ttu-id="d3d05-211">Ändern der `<title>` Element so, dass die It wie dieses Markup hinzu:</span><span class="sxs-lookup"><span data-stu-id="d3d05-211">Change the `<title>` element so that it looks like this markup:</span></span>

[!code-html[Main](layouts/samples/sample9.html)]

<span data-ttu-id="d3d05-212">Dieser Code rendert den Inhalt der `Page.Title` Eigenschaft direkt an dieser Position auf der Seite.</span><span class="sxs-lookup"><span data-stu-id="d3d05-212">This code renders whatever is in the `Page.Title` property right at that location in the page.</span></span>

<span data-ttu-id="d3d05-213">Führen Sie die *Movies.cshtml* Seite.</span><span class="sxs-lookup"><span data-stu-id="d3d05-213">Run the *Movies.cshtml* page.</span></span> <span data-ttu-id="d3d05-214">Dieses Mal die Browserregisterkarte zeigt, was Sie als Wert übergeben `Page.Title`:</span><span class="sxs-lookup"><span data-stu-id="d3d05-214">This time the browser tab shows what you passed as the value of `Page.Title`:</span></span>

![Eine Registerkarte des Webbrowsers, mit dem Titel dynamisch erstellt](layouts/_static/image9.png)

<span data-ttu-id="d3d05-216">Wenn Sie möchten, zeigen Sie die Seitenquelle im Browser.</span><span class="sxs-lookup"><span data-stu-id="d3d05-216">If you want, view the page source in the browser.</span></span> <span data-ttu-id="d3d05-217">Sie sehen, dass die `<title>` Element wird als gerendert `<title>List Movies</title>`.</span><span class="sxs-lookup"><span data-stu-id="d3d05-217">You can see that the `<title>` element is rendered as `<title>List Movies</title>`.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="d3d05-218">**Das Page-Objekt**</span><span class="sxs-lookup"><span data-stu-id="d3d05-218">**The Page Object**</span></span>
> 
> <span data-ttu-id="d3d05-219">Eine hilfreiche Funktion von `Page` ist, dass es ein dynamisches Objekt – die `Title` Eigenschaft ist nicht mit einem festen oder reservierten Namen.</span><span class="sxs-lookup"><span data-stu-id="d3d05-219">A useful feature of `Page` is that it's a dynamic object — the `Title` property is not a fixed or reserved name.</span></span> <span data-ttu-id="d3d05-220">Können Sie *alle* Name für den Wert der `Page` Objekt.</span><span class="sxs-lookup"><span data-stu-id="d3d05-220">You can use *any* name for a value of the `Page` object.</span></span> <span data-ttu-id="d3d05-221">Angenommen, Sie können genauso einfach haben übergeben den Titel mithilfe einer Eigenschaft mit dem Namen `Page.CurrentName` oder `Page.MyPage`.</span><span class="sxs-lookup"><span data-stu-id="d3d05-221">For example, you could as easily have passed the title by using a property named `Page.CurrentName` or `Page.MyPage`.</span></span> <span data-ttu-id="d3d05-222">Die einzige Einschränkung besteht darin, dass der Name muss den normalen Regeln für welche Eigenschaften genannt werden können.</span><span class="sxs-lookup"><span data-stu-id="d3d05-222">The only restriction is that the name has to follow the normal rules for what properties can be named.</span></span> <span data-ttu-id="d3d05-223">(Z. B. kann nicht der Name enthalten ein Leerzeichen.)</span><span class="sxs-lookup"><span data-stu-id="d3d05-223">(For example, the name can't contain a space.)</span></span>
> 
> <span data-ttu-id="d3d05-224">Sie können eine beliebige Anzahl von Werten übergeben, indem die `Page` Objekt.</span><span class="sxs-lookup"><span data-stu-id="d3d05-224">You can pass any number of values by using the `Page` object.</span></span> <span data-ttu-id="d3d05-225">Wenn Sie Informationen auf der Seite "Layout" übergeben, konnte Sie Werte übergeben, indem Sie etwa `Page.MovieTitle` und `Page.Genre` und `Page.MovieYear`.</span><span class="sxs-lookup"><span data-stu-id="d3d05-225">If you wanted to pass movie information to the layout page, you could pass values by using something like `Page.MovieTitle` and `Page.Genre` and `Page.MovieYear`.</span></span> <span data-ttu-id="d3d05-226">(Oder eine beliebige andere Namen, die Sie entwickelt, um die Informationen zu speichern.) Die einzige Anforderung – Dies ist wahrscheinlich offensichtliche – besteht darin, dass Sie die gleichen Namen in der Seite Inhalt und die Seite "Layout" verwenden.</span><span class="sxs-lookup"><span data-stu-id="d3d05-226">(Or any other names that you invented to store the information.) The only requirement — which is probably obvious — is that you have to use the same names in the content page and the layout page.</span></span>
> 
> <span data-ttu-id="d3d05-227">Die Informationen, die Sie, indem übergeben die `Page` Objekt ist nicht beschränkt auf nur-Text auf der Seite "Layout" angezeigt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="d3d05-227">The information you pass by using the `Page` object isn't limited to just text to display on the layout page.</span></span> <span data-ttu-id="d3d05-228">Übergeben Sie den Wert auf der Seite "Layout", und klicken Sie dann Code in die Seite "Layout" kann den Wert entscheiden, ob Sie einen Abschnitt der Seite angezeigt wie *CSS* Datei verwenden und so weiter.</span><span class="sxs-lookup"><span data-stu-id="d3d05-228">You can pass a value to the layout page, and then code in the layout page can use the value to decide whether to display a section of the page, what *.css* file to use, and so on.</span></span> <span data-ttu-id="d3d05-229">Die übergebenen Werte in der `Page` Objekt werden wie alle anderen Werte Verwendung im Code.</span><span class="sxs-lookup"><span data-stu-id="d3d05-229">The values you pass in the `Page` object are like any other values that you use in code.</span></span> <span data-ttu-id="d3d05-230">Es ist nur die Werte in der Inhaltsseite stammen und an die Seite "Layout" übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="d3d05-230">It's just that the values originate in the content page and are passed to the layout page.</span></span>


<span data-ttu-id="d3d05-231">Öffnen der *AddMovie.cshtml* Seite, und fügen Sie eine Zeile an den Anfang der Code, der einen Titel für die *AddMovie.cshtml* Seite:</span><span class="sxs-lookup"><span data-stu-id="d3d05-231">Open the *AddMovie.cshtml* page and add a line to the top of the code that provides a title for the *AddMovie.cshtml* page:</span></span>

[!code-csharp[Main](layouts/samples/sample10.cs)]

<span data-ttu-id="d3d05-232">Führen Sie die *AddMovie.cshtml* Seite.</span><span class="sxs-lookup"><span data-stu-id="d3d05-232">Run the *AddMovie.cshtml* page.</span></span> <span data-ttu-id="d3d05-233">Sie finden Sie in den neuen Titel vorhanden:</span><span class="sxs-lookup"><span data-stu-id="d3d05-233">You see the new title there:</span></span>

![Eine Registerkarte des Webbrowsers, dynamisch erstellten Titel "Filme hinzufügen" angezeigt](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a><span data-ttu-id="d3d05-235">Aktualisieren die verbleibenden Seiten, um das Layout zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="d3d05-235">Updating the Remaining Pages to Use the Layout</span></span>

<span data-ttu-id="d3d05-236">Jetzt können Sie die verbleibenden Seiten an Ihrem Standort abschließen, sodass sie das neue Layout verwenden.</span><span class="sxs-lookup"><span data-stu-id="d3d05-236">Now you can finish the remaining pages in your site so that they use the new layout.</span></span> <span data-ttu-id="d3d05-237">Open *EditMovie.cshtml* und *DeleteMovie.cshtml* wiederum, und nehmen Sie die gleichen Änderungen in den einzelnen.</span><span class="sxs-lookup"><span data-stu-id="d3d05-237">Open *EditMovie.cshtml* and *DeleteMovie.cshtml* in turn and make the same changes in each.</span></span>

<span data-ttu-id="d3d05-238">Fügen Sie die Codezeile, die links auf der Seite "Layout" hinzu:</span><span class="sxs-lookup"><span data-stu-id="d3d05-238">Add the line of code that links to the layout page:</span></span>

[!code-csharp[Main](layouts/samples/sample11.cs)]

<span data-ttu-id="d3d05-239">Hinzufügen einer Zeile, um den Titel der Seite festlegen:</span><span class="sxs-lookup"><span data-stu-id="d3d05-239">Add a line to set the title of the page:</span></span>

[!code-csharp[Main](layouts/samples/sample12.cs)]

<span data-ttu-id="d3d05-240">oder:</span><span class="sxs-lookup"><span data-stu-id="d3d05-240">or:</span></span>

[!code-csharp[Main](layouts/samples/sample13.cs)]

<span data-ttu-id="d3d05-241">Entfernen Sie das überflüssige HTML-Markup – im Grunde, lassen Sie nur die Bits, die innerhalb der `<body>` -Element (plus der Codeblock oben).</span><span class="sxs-lookup"><span data-stu-id="d3d05-241">Remove all the extraneous HTML markup — basically, leave only the bits that are inside the `<body>` element (plus the code block at the top).</span></span>

<span data-ttu-id="d3d05-242">Ändern der `<h1>` Element ist ein `<h2>` Element.</span><span class="sxs-lookup"><span data-stu-id="d3d05-242">Change the `<h1>` element to be an `<h2>` element.</span></span>

<span data-ttu-id="d3d05-243">Wenn Sie diese Änderungen vorgenommen haben, testen Sie, und stellen Sie sicher, dass er ordnungsgemäß angezeigt wird und dass der Titel richtig ist.</span><span class="sxs-lookup"><span data-stu-id="d3d05-243">When you've made these changes, test each and make sure that it's displaying properly and that the title is correct.</span></span>

## <a name="parting-thoughts-about-layout-pages"></a><span data-ttu-id="d3d05-244">Trennflächen Gedanken zu Layoutseiten</span><span class="sxs-lookup"><span data-stu-id="d3d05-244">Parting Thoughts About Layout Pages</span></span>

<span data-ttu-id="d3d05-245">In diesem Lernprogramm erstellt Sie eine  *\_Layout.cshtml* Seite und verwendet die `RenderBody` Methode, um Inhalt aus einer anderen Seite zusammenzuführen.</span><span class="sxs-lookup"><span data-stu-id="d3d05-245">In this tutorial you created a *\_Layout.cshtml* page and used the `RenderBody` method to merge content from another page.</span></span> <span data-ttu-id="d3d05-246">Dies ist das grundlegende Muster für die Verwendung von Layouts in Webseiten.</span><span class="sxs-lookup"><span data-stu-id="d3d05-246">That's the basic pattern for using layouts in Web Pages.</span></span>

<span data-ttu-id="d3d05-247">Layoutseiten haben zusätzliche Funktionen, die hier nicht beschrieben.</span><span class="sxs-lookup"><span data-stu-id="d3d05-247">Layout pages have additional features that we didn't cover here.</span></span> <span data-ttu-id="d3d05-248">Sie können z. B. Layoutseiten schachteln – eine andere wiederum eine Layoutseite verweisen kann.</span><span class="sxs-lookup"><span data-stu-id="d3d05-248">For example, you can nest layout pages — one layout page can in turn reference another.</span></span> <span data-ttu-id="d3d05-249">Geschachtelte Layouts ist hilfreich, wenn Sie Unterabschnitte eines Standorts verwenden, die unterschiedliche Layouts erfordern.</span><span class="sxs-lookup"><span data-stu-id="d3d05-249">Nested layouts can be useful if you're working with subsections of a site that require different layouts.</span></span> <span data-ttu-id="d3d05-250">Sie können auch zusätzliche Methoden (z. B. `RenderSection`) einrichten, mit dem Namen Abschnitte in der Seite "Layout".</span><span class="sxs-lookup"><span data-stu-id="d3d05-250">You can also use additional methods (for example, `RenderSection`) to set up named sections in the layout page.</span></span>

<span data-ttu-id="d3d05-251">Die Kombination von Layoutseiten und *CSS* Dateien ist leistungsstark.</span><span class="sxs-lookup"><span data-stu-id="d3d05-251">The combination of layout pages and *.css* files is powerful.</span></span> <span data-ttu-id="d3d05-252">Wie Sie in den nächsten Lernprogramm sehen werden, in WebMatrix können erstellen Sie eine Website auf Grundlage einer *Vorlage*, erhalten Sie eine Site mit vorgefertigten Funktionen darin.</span><span class="sxs-lookup"><span data-stu-id="d3d05-252">As you'll see in the next tutorial series, in WebMatrix you can create a site based on a *template*, which gives you a site that has prebuilt functionality in it.</span></span> <span data-ttu-id="d3d05-253">Die Vorlagen, Layoutseiten und CSS zum Erstellen von Websites, die großartige und deren Funktionen wie Menüs, verwendet.</span><span class="sxs-lookup"><span data-stu-id="d3d05-253">The templates make good use of layout pages and CSS to create sites that look great and that have features like menus.</span></span> <span data-ttu-id="d3d05-254">Hier ist ein Screenshot der Homepage von einer Website auf Basis einer Vorlage, die mit Funktionen, die Layoutseiten und CSS verwenden:</span><span class="sxs-lookup"><span data-stu-id="d3d05-254">Here's a screenshot of the home page from a site based on a template, showing features that use layout pages and CSS:</span></span>

![Layout von WebMatrix-Websitevorlage, die mit den Header, Navigationsbereich, Inhaltsbereichs, optionalen Abschnitt und Anmeldung Links erstellt](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a><span data-ttu-id="d3d05-256">Vollständige Auflistung für Filmdetailseite (die Verwendung eine Layoutseite aktualisiert werden)</span><span class="sxs-lookup"><span data-stu-id="d3d05-256">Complete Listing for Movie Page (Updated to Use a Layout Page)</span></span>

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a><span data-ttu-id="d3d05-257">Auflisten der für die Seite abgeschlossen hinzufügen Filmdetailseite (aktualisiert für Layout)</span><span class="sxs-lookup"><span data-stu-id="d3d05-257">Complete Page Listing for Add Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a><span data-ttu-id="d3d05-258">Auf der Seite abgeschlossen Listeneintrag löschen Filmdetailseite (aktualisiert für Layout)</span><span class="sxs-lookup"><span data-stu-id="d3d05-258">Complete Page Listing for Delete Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a><span data-ttu-id="d3d05-259">Auf der Seite abgeschlossen Listeneintrag bearbeiten (Seite) Film (aktualisiert für Layout)</span><span class="sxs-lookup"><span data-stu-id="d3d05-259">Complete Page Listing for Edit Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a><span data-ttu-id="d3d05-260">Als Nächstes kommen</span><span class="sxs-lookup"><span data-stu-id="d3d05-260">Coming Up Next</span></span>

<span data-ttu-id="d3d05-261">In den nächsten Lernprogrammen erfahren Sie, wie Sie Ihre Website mit dem Internet zu veröffentlichen, sodass "Jeder", die sie anzeigen kann.</span><span class="sxs-lookup"><span data-stu-id="d3d05-261">In the next tutorial, you'll learn how to publish your site to the Internet so everyone can see it.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d3d05-262">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="d3d05-262">Additional Resources</span></span>

- <span data-ttu-id="d3d05-263">[Erstellen ein konsistentes Aussehen](https://go.microsoft.com/fwlink/?LinkID=202891) – ein Artikel, die einige weitere Details zum Arbeiten mit Layouts bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="d3d05-263">[Creating a Consistent Look](https://go.microsoft.com/fwlink/?LinkID=202891) — An article that provides some more detail on working with layouts.</span></span> <span data-ttu-id="d3d05-264">Außerdem werden wie einen Wert an einer Layoutseite übergeben, das Anzeigen oder ausblenden Inhalte beschrieben.</span><span class="sxs-lookup"><span data-stu-id="d3d05-264">It also describes how to pass a value to a layout page that shows or hides some of the content.</span></span>
- <span data-ttu-id="d3d05-265">[Geschachtelte Layoutseiten mit Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) – Mike Brind Blogs ein Beispiel zum Layoutseiten schachteln.</span><span class="sxs-lookup"><span data-stu-id="d3d05-265">[Nested Layout Pages with Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) — Mike Brind blogs an example of how to nest layout pages.</span></span> <span data-ttu-id="d3d05-266">(Schließt einen Download der Seiten).</span><span class="sxs-lookup"><span data-stu-id="d3d05-266">(Includes a download of the pages.)</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="d3d05-267">[Zurück](deleting-data.md)
[Weiter](publishing.md)</span><span class="sxs-lookup"><span data-stu-id="d3d05-267">[Previous](deleting-data.md)
[Next](publishing.md)</span></span>
