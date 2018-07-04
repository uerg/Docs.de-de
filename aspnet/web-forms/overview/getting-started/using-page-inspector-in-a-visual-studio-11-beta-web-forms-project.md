---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: Verwenden der Seitenprüfung für Visual Studio 2012 in der ASP.NET Web Forms | Microsoft-Dokumentation
author: rick-anderson
description: Seitenprüfung für Visual Studio 2012 ist ein Webentwicklungstool mit einem integrierten Browser. Auswählen eines Elements in der integrierten Browser, und klicken Sie auf der Seitenprüfung...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: 1f1ac1072d33c85ed3e64c493b9cf7970695cea6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384474"
---
<a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a><span data-ttu-id="81148-104">Verwenden der Seitenprüfung für Visual Studio 2012 in ASP.NET Web Forms</span><span class="sxs-lookup"><span data-stu-id="81148-104">Using Page Inspector for Visual Studio 2012 in ASP.NET Web Forms</span></span>
====================
<span data-ttu-id="81148-105">von Tim Ammann</span><span class="sxs-lookup"><span data-stu-id="81148-105">by Tim Ammann</span></span>

> <span data-ttu-id="81148-106">Seitenprüfung für Visual Studio 2012 ist ein Webentwicklungstool mit einem integrierten Browser.</span><span class="sxs-lookup"><span data-stu-id="81148-106">Page Inspector for Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="81148-107">Auswählen eines Elements in der integrierten Browser und der Seitenprüfung sofort hervorgehoben, Quell- und CSS-des-Elements.</span><span class="sxs-lookup"><span data-stu-id="81148-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="81148-108">Sie können einer beliebigen Seite in Ihrer Anwendung navigieren, finden die Quellen der gerenderten Markup, und schnell Browsertools direkt in Visual Studio-Umgebung verwenden.</span><span class="sxs-lookup"><span data-stu-id="81148-108">You can browse any page in your application, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> <span data-ttu-id="81148-109">Dieses Tutorial Shwos wie Überprüfungsmodus zu aktivieren und schnell suchen und Bearbeiten von CSS-Regeln und Text in Ihrer Web-Projekt.</span><span class="sxs-lookup"><span data-stu-id="81148-109">This tutorial shwos how to enable Inspection Mode and then quickly locate and edit CSS rules and text within your web project.</span></span> <span data-ttu-id="81148-110">Das Tutorial verwendet ein Web Forms-Anwendungsprojekt, aber Sie können auch die Seitenprüfung für Websiteprojekte und [MVC](https://go.microsoft.com/?linkid=9802002) Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="81148-110">The tutorial uses a Web Forms Application Project, but you can also use Page Inspector for Web Site projects and [MVC](https://go.microsoft.com/?linkid=9802002) applications.</span></span>
> 
> <span data-ttu-id="81148-111">Das Tutorial enthält die folgenden Abschnitte:</span><span class="sxs-lookup"><span data-stu-id="81148-111">The tutorial has the following sections:</span></span>
> 
> [<span data-ttu-id="81148-112">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="81148-112">Prerequisites</span></span>](#_1_prerequisites)
> 
> [<span data-ttu-id="81148-113">Erstellen einer Webanwendung</span><span class="sxs-lookup"><span data-stu-id="81148-113">Create a Web Application</span></span>](#_2_creating_a)
> 
> [<span data-ttu-id="81148-114">Verwenden Sie die Seitenprüfung, um die Anwendung anzuzeigen</span><span class="sxs-lookup"><span data-stu-id="81148-114">Use Page Inspector to View the Application</span></span>](#_3_using_page)
> 
> [<span data-ttu-id="81148-115">Überprüfungsmodus zu aktivieren</span><span class="sxs-lookup"><span data-stu-id="81148-115">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> 
> [<span data-ttu-id="81148-116">Verwenden der Seitenprüfung Markup ändern</span><span class="sxs-lookup"><span data-stu-id="81148-116">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> 
> [<span data-ttu-id="81148-117">Überprüfungsmodus und HTML-Fenster</span><span class="sxs-lookup"><span data-stu-id="81148-117">Inspection Mode and the HTML Window</span></span>](#_6_inspection_mode)
> 
> [<span data-ttu-id="81148-118">Vorschau der CSS-Änderungen im Fenster Stile</span><span class="sxs-lookup"><span data-stu-id="81148-118">Preview CSS Changes in the Styles Window</span></span>](#_7_previewing_css)
> 
> [<span data-ttu-id="81148-119">Automatische CSS-Synchronisierung</span><span class="sxs-lookup"><span data-stu-id="81148-119">CSS Auto Sync</span></span>](#css_auto_sync)
> 
> [<span data-ttu-id="81148-120">Mithilfe der CSS-Farbauswahl</span><span class="sxs-lookup"><span data-stu-id="81148-120">Using the CSS Color Picker</span></span>](#css_color_picker)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="81148-121">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="81148-121">Prerequisites</span></span>

- <span data-ttu-id="81148-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) oder [Visual Studio Express 2012 für Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="81148-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="81148-123">Rufen Sie die neueste Version der Seitenprüfung mit [Webplattform-Installer](https://go.microsoft.com/fwlink/?LinkId=255386) auf das Azure SDK für .NET 2.0 installiert.</span><span class="sxs-lookup"><span data-stu-id="81148-123">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Azure SDK for .NET 2.0.</span></span>


<span data-ttu-id="81148-124">Die Seitenprüfung ist Microsoft Web Developer Tools enthalten.</span><span class="sxs-lookup"><span data-stu-id="81148-124">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="81148-125">Die neueste Version ist 1.3.</span><span class="sxs-lookup"><span data-stu-id="81148-125">The latest version is 1.3.</span></span> <span data-ttu-id="81148-126">Um die Version haben Sie, Visual Studio ausführen, und wählen Sie **Info zu Microsoft Visual Studio** aus der **Hilfe** Menü.</span><span class="sxs-lookup"><span data-stu-id="81148-126">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="81148-127">Erstellen einer Webanwendung</span><span class="sxs-lookup"><span data-stu-id="81148-127">Create a Web Application</span></span>

<span data-ttu-id="81148-128">Zunächst erstellen Sie eine Webanwendung, der Seitenprüfung mit verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="81148-128">First, you will create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="81148-129">Wählen Sie in Visual Studio **Datei** &gt; **neues Projekt**.</span><span class="sxs-lookup"><span data-stu-id="81148-129">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="81148-130">Erweitern Sie auf der linken Seite **Visual C#-** Option **Web**, und wählen Sie dann **ASP.NET Web Forms-Anwendung**.</span><span class="sxs-lookup"><span data-stu-id="81148-130">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET Web Forms Application**.</span></span>

![Neue Web Forms-Anwendung](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

<span data-ttu-id="81148-132">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="81148-132">Click **OK**.</span></span>

<span data-ttu-id="81148-133">Öffnet die Anwendung in **Quelle** anzeigen.</span><span class="sxs-lookup"><span data-stu-id="81148-133">The application opens in **Source** view.</span></span>

![Neue Web Forms-Anwendung in der Quellansicht](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

<span data-ttu-id="81148-135">Nun, da Sie eine Anwendung gearbeitet haben, können Sie der Seitenprüfung verwenden, überprüfen und ändern Sie sie.</span><span class="sxs-lookup"><span data-stu-id="81148-135">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a><span data-ttu-id="81148-136">Verwenden Sie die Seitenprüfung, um die Anwendung anzuzeigen</span><span class="sxs-lookup"><span data-stu-id="81148-136">Use Page Inspector to View the Application</span></span>

<span data-ttu-id="81148-137">Als Nächstes zeigen Sie die Anwendung mit der Seitenprüfung an.</span><span class="sxs-lookup"><span data-stu-id="81148-137">Next, you will view the application with Page Inspector.</span></span> <span data-ttu-id="81148-138">In **Projektmappen-Explorer**, klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie dann **in Seitenprüfung anzeigen**.</span><span class="sxs-lookup"><span data-stu-id="81148-138">In **Solution Explorer**, right click the project, and then choose **View in Page Inspector**.</span></span>

![In Seitenprüfung anzeigen](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

<span data-ttu-id="81148-140">In der Standardeinstellung ist Page Inspector zum ersten Mal gestartet wird. er als schmalen Fenster auf der linken Seite der Visual Studio-Umgebung angedockt.</span><span class="sxs-lookup"><span data-stu-id="81148-140">By default, when Page Inspector launches for the first time, it is docked as a narrow window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="81148-141">Lassen Sie auf der linken Seite angedockt, und legen Sie ihn auf eine Breite, die Ihnen vertraut ist, oder es in einer der Bereiche Tool für die oben, unten oder rechts Andocken:</span><span class="sxs-lookup"><span data-stu-id="81148-141">Leave it docked on the left side and set it to a width that is comfortable for you, or dock it in one of the tool areas on the top, bottom, or right:</span></span>

![Die Seitenprüfung docking-Positionen](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

<span data-ttu-id="81148-143">Wenn Sie das Fenster der Seitenprüfung abdocken, kann man diese außerhalb von Visual Studio oder sogar über einen zweiten Bildschirm falls vorhanden.</span><span class="sxs-lookup"><span data-stu-id="81148-143">If you undock the Page Inspector window, you can place it outside Visual Studio, or even on a second monitor if you have one.</span></span> <span data-ttu-id="81148-144">Allerdings in der Reihenfolge nach ALT + TAB zwischen der Seitenprüfung und Visual Studio, wenn das Page Inspector-Fenster abgedockt ist, wechseln Sie zu **Tools** &gt; **Optionen** &gt;  **Umgebung** &gt; **Registerkarten und Windows**, und wählen Sie unter **Registerkarte auch**, deaktivieren Sie das Kontrollkästchen namens **Unverankert Toolfenster bleiben immer auf die Klicken Sie im Hauptfenster**:</span><span class="sxs-lookup"><span data-stu-id="81148-144">However, in order to ALT+TAB between Page Inspector and Visual Studio when the Page Inspector window is undocked, go to **Tools** &gt; **Options** &gt; **Environment** &gt; **Tabs and Windows**, and under **Tab Well**, clear the check box called **Floating tool windows always stay on top of the main window**:</span></span>

![Deaktivieren Sie das unverankerte Tool Windows-Kontrollkästchen, ALT + TAB zwischen Visual Studio und das nicht angedockte Fenster für die Seitenprüfung](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

<span data-ttu-id="81148-146">Im obere Bereich des Fensters die Seitenprüfung zeigt die aktuelle Seite in einem Browserfenster angezeigt.</span><span class="sxs-lookup"><span data-stu-id="81148-146">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="81148-147">Im unteren Bereich zeigt die Seite im HTML-Markup auf der linken Seite, und einige Registerkarten auf der rechten Seite, mit denen Sie verschiedene Aspekte der Seite zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="81148-147">The bottom pane shows the page in HTML markup on the left, and some tabs on the right that let you inspect different aspects of the page.</span></span> <span data-ttu-id="81148-148">Der untere Bereich ist ähnlich wie die [F12-Entwicklertools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="81148-148">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span> <span data-ttu-id="81148-149">(Allerdings können im Gegensatz zu den Entwicklertools, Sie der Seitenprüfung direkt in Visual Studio verwenden.)</span><span class="sxs-lookup"><span data-stu-id="81148-149">(However, unlike the developer tools, you can use Page Inspector right within Visual Studio.)</span></span>

![Seitenprüfung](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

<span data-ttu-id="81148-151">In diesem Tutorial verwenden Sie im Browser der Seitenprüfung Bereich und die **HTML** und **Stile** Registerkarten können Sie schnell zu navigieren, und nehmen Sie Änderungen an der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="81148-151">In this tutorial, you will use the Page Inspector browser pane, and the **HTML** and **Styles** tabs to help you rapidly navigate and make changes to the application.</span></span>

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a><span data-ttu-id="81148-152">Überprüfungsmodus zu aktivieren</span><span class="sxs-lookup"><span data-stu-id="81148-152">Enable Inspection Mode</span></span>

<span data-ttu-id="81148-153">Als Nächstes sehen Sie, wie der Seitenprüfung Überprüfungsmodus funktioniert.</span><span class="sxs-lookup"><span data-stu-id="81148-153">Next, you will see how Page Inspector's Inspection Mode works.</span></span> <span data-ttu-id="81148-154">Klicken Sie im Fenster der Seitenprüfung auf die **prüfen** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="81148-154">In the Page Inspector window, click the **Inspect** button.</span></span>

![Element prüfen](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

<span data-ttu-id="81148-156">Um Überprüfungsmodus in Aktion zu sehen, bewegen Sie den Mauszeiger über verschiedene Teile der Seite im Fenster Browser der Seitenprüfung.</span><span class="sxs-lookup"><span data-stu-id="81148-156">To see inspection mode in action, move your mouse over different parts of the page within the Page Inspector browser window.</span></span> <span data-ttu-id="81148-157">Wie Sie dies tun, wird der Mauszeiger ändert sich zu einem großen Pluszeichen, und untergeordnete Element wird hervorgehoben:</span><span class="sxs-lookup"><span data-stu-id="81148-157">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![Mit dem Mauszeiger auf div.content-Wrappers](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

<span data-ttu-id="81148-159">Wenn Sie den Mauszeiger bewegen, beachten Sie, dass</span><span class="sxs-lookup"><span data-stu-id="81148-159">As you move the mouse pointer, note that</span></span>

- <span data-ttu-id="81148-160">Der Inhalt **Quelle** Anzeigen von Änderungen an das Markup für das ausgewählte Element auf der Seite angezeigt.</span><span class="sxs-lookup"><span data-stu-id="81148-160">The content in **Source** view changes to show the markup corresponding to the selected element on the page.</span></span> <span data-ttu-id="81148-161">Das relevante Markup wird hervorgehoben.</span><span class="sxs-lookup"><span data-stu-id="81148-161">The relevant markup is highlighted.</span></span> <span data-ttu-id="81148-162">Wenn die Quelle in einer anderen Datei handelt, wird diese Datei mit den relevanten Markup hervorgehoben in der Quellansicht geöffnet.</span><span class="sxs-lookup"><span data-stu-id="81148-162">If the source is in another file, that file is opened in Source view with the relevant markup highlighted.</span></span>

- <span data-ttu-id="81148-163">Das Markup angezeigt, der **HTML** Registerkarte in der Seitenprüfung ändert sich auch um das ausgewählte Element auf der Seite zu entsprechen.</span><span class="sxs-lookup"><span data-stu-id="81148-163">The markup displayed in the **HTML** tab in Page Inspector also changes to correspond to the selected element on the page.</span></span> <span data-ttu-id="81148-164">In der **HTML** Registerkarte wird das relevante Markup beschrieben.</span><span class="sxs-lookup"><span data-stu-id="81148-164">In the **HTML** tab, the relevant markup is outlined.</span></span>

- <span data-ttu-id="81148-165">Die **Stile** Registerkarte zeigt die CSS-Regeln für die aktuelle Auswahl relevant.</span><span class="sxs-lookup"><span data-stu-id="81148-165">The **Styles** tab shows the CSS rules relevant to the current selection.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="81148-166">Verwenden der Seitenprüfung Markup ändern</span><span class="sxs-lookup"><span data-stu-id="81148-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="81148-167">Jetzt sehen Sie, wie Sie die Seitenprüfung verwenden können, suchen, und nehmen Sie Änderungen an Markup oder Text, dessen Speicherort möglicherweise nicht sofort ersichtlich.</span><span class="sxs-lookup"><span data-stu-id="81148-167">Now you will see how you can use Page Inspector to find and make changes to markup or text whose location might not be immediately obvious.</span></span>

<span data-ttu-id="81148-168">Fügen Sie der Seitenprüfung im Überprüfungsmodus aus, und führen Sie einen Bildlauf zum unteren Rand der Startseite.</span><span class="sxs-lookup"><span data-stu-id="81148-168">Put Page Inspector in Inspection Mode and then scroll to the bottom of the home page.</span></span>

<span data-ttu-id="81148-169">Werden, sobald Sie den Footerbereich eingeben, wird die Seitenprüfung öffnet die *Site.Master* Layoutdatei im **Quelle** Ansicht in eine temporäre Registerkarte rechts neben den anderen Registerkarten, und werden im Abschnitt des Masters Seite, die Sie ausgewählt haben.</span><span class="sxs-lookup"><span data-stu-id="81148-169">As soon as you enter the footer area, Page Inspector opens the *Site.Master* layout file in **Source** view in a temporary tab to the right of the other tabs and highlights the section of the master page that you have selected.</span></span> <span data-ttu-id="81148-170">So erfahren Sie, wie der Seitenprüfung suchen und Anzeigen von Inhalt auf einer Seite, die tatsächlich von einer anderen Datei als die stammen kann, die Sie ursprünglich geöffnet haben können.</span><span class="sxs-lookup"><span data-stu-id="81148-170">This shows you how Page Inspector can find and display content on a page that might actually come from a different file than the one you originally opened.</span></span>

![Highlights der Fußzeile im Überprüfungsmodus](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

<span data-ttu-id="81148-172">Verschieben Sie den Mauszeiger im Fenster Browser der Seitenprüfung auf die Zeile mit dem Copyright <a id="a"> </a>beachten.</span><span class="sxs-lookup"><span data-stu-id="81148-172">In the Page Inspector browser window, move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span>

<span data-ttu-id="81148-173">In der *Site.Master* Seite, die entsprechende Zeile wird hervorgehoben.</span><span class="sxs-lookup"><span data-stu-id="81148-173">In the *Site.Master* page, the corresponding line is highlighted.</span></span>

![Hervorgehobene Zeile der Fußzeile-copyright](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

<span data-ttu-id="81148-175">Hinzufügen von Text an das Ende der Zeile in der *Site.Master* Datei.</span><span class="sxs-lookup"><span data-stu-id="81148-175">Add some text to the end of the line in the *Site.Master* file.</span></span>

<span data-ttu-id="81148-176">&lt;p&gt;&amp;kopieren; &lt;%: DateTime.Now.Year %&gt; -meine ASP.NET Application Rocks!&lt; / p&gt;</span><span class="sxs-lookup"><span data-stu-id="81148-176">&lt;p&gt;&amp;copy; &lt;%: DateTime.Now.Year %&gt; - My ASP.NET Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="81148-177">Jetzt drücken Sie Strg + Alt + Eingabe, oder klicken Sie auf die Update-Leiste zum Anzeigen der Ergebnisse im Fenster Browser der Seitenprüfung.</span><span class="sxs-lookup"><span data-stu-id="81148-177">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![Meine ASP.NET Application Rocks!](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

<span data-ttu-id="81148-179">Sie können daran gedacht, dass die Fußzeile auf war die *"default.aspx"* Seite, aber es stellte sich heraus auf der Seite Masterlayout sein, und der Seitenprüfung fand es für Sie.</span><span class="sxs-lookup"><span data-stu-id="81148-179">You might have thought that the footer was on the *Default.aspx* page, but it turned out to be in the master layout page, and Page Inspector found it for you.</span></span>

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="81148-180">Überprüfungsmodus und HTML-Fenster</span><span class="sxs-lookup"><span data-stu-id="81148-180">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="81148-181">Als Nächstes müssen Sie einen kurzen Blick auf die HTML-Fenster, und wie sie Elemente für Sie zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="81148-181">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="81148-182">Fügen Sie der Seitenprüfung im Überprüfungsmodus aus.</span><span class="sxs-lookup"><span data-stu-id="81148-182">Put Page Inspector in Inspection Mode.</span></span>

![Element prüfen](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

<span data-ttu-id="81148-184">Klicken Sie auf den oberen Teil der Seite die Fehlermeldung "Ihr Logo hier einfügen".</span><span class="sxs-lookup"><span data-stu-id="81148-184">Click the top part of the page that says "your logo here".</span></span> <span data-ttu-id="81148-185">Untersuchen Sie ein bestimmtes Element im Detail, damit die Anzeige im Browserfenster nicht mehr geändert werden, wie Sie den Mauszeiger bewegen.</span><span class="sxs-lookup"><span data-stu-id="81148-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="81148-186">Nun verschieben Sie den Mauszeiger an die **HTML** Fenster.</span><span class="sxs-lookup"><span data-stu-id="81148-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="81148-187">Während Sie den Mauszeiger bewegen, wird der Seitenprüfung beschrieben, das Element innerhalb der **HTML** Fenster und das entsprechende Element im Browserfenster hervorgehoben.</span><span class="sxs-lookup"><span data-stu-id="81148-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![HTML-Fenster](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

<span data-ttu-id="81148-189">Wie zuvor die Seitenprüfung öffnet die *Site.Master* -Datei für Sie in eine temporäre Registerkarte. Klicken Sie auf der Registerkarte "Site.Master", und das entsprechende Markup markiert die &lt;Header&gt; Abschnitt:</span><span class="sxs-lookup"><span data-stu-id="81148-189">As before, Page Inspector opens the *Site.Master* file for you in a temporary tab. Click the Site.Master tab, and the corresponding markup is highlighted in the &lt;header&gt; section:</span></span>

![Hervorgehobene markup](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="81148-191">Vorschau der CSS-Änderungen im Fenster Stile</span><span class="sxs-lookup"><span data-stu-id="81148-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="81148-192">Als Nächstes werden Sie sehen, wie Sie die Seitenprüfung verwenden können **Stile** Fenster Vorschau der Änderungen an CSS.</span><span class="sxs-lookup"><span data-stu-id="81148-192">Next, you will see how you can use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="81148-193">Klicken Sie auf die **prüfen** Schaltfläche, um die Seitenprüfung im Überprüfungsmodus zu platzieren.</span><span class="sxs-lookup"><span data-stu-id="81148-193">Click the **Inspect** button to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="81148-194">Verschieben Sie im Fenster Browser der Seitenprüfung den Mauszeiger über den Abschnitt "Startseite", bis die **div.content-Wrappers** Bezeichnung angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="81148-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![Mit dem Mauszeiger auf Elemente](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

<span data-ttu-id="81148-196">Klicken Sie im Abschnitt div.content-Wrappers einmal, und verschieben Sie den Mauszeiger an die **Stile** Fenster.</span><span class="sxs-lookup"><span data-stu-id="81148-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="81148-197">Deaktivieren Sie unter dem Selektor .featured .content-Wrapper-Klasse einen Wert ein, und aktivieren Sie das Kontrollkästchen für die Background-Color-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="81148-197">Under the .featured .content-wrapper class selector, clear and select the checkbox for the background-color property.</span></span>

![Clear-Hintergrundfarbe](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

<span data-ttu-id="81148-199">Beachten Sie, wie die Änderung sofort im Browser der Seitenprüfung-Fenster zeigt in der Vorschau.</span><span class="sxs-lookup"><span data-stu-id="81148-199">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="81148-200">Wählen Sie das Kontrollkästchen erneut aus, doppelklicken Sie auf den Wert der Eigenschaft, und ändern Sie ihn in `red`.</span><span class="sxs-lookup"><span data-stu-id="81148-200">Select the checkbox again, then double-click the property value and change it to `red`.</span></span> <span data-ttu-id="81148-201">Die Änderung wird sofort aus:</span><span class="sxs-lookup"><span data-stu-id="81148-201">The change shows immediately:</span></span>

![Rote Hintergrundfarbe](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

<span data-ttu-id="81148-203">Die **Stile** Fenster macht es einfach, test und Vorschau CSS Änderungen erforderlich, bevor die Änderungen zu, um den Stil übernehmen Stylesheet selbst.</span><span class="sxs-lookup"><span data-stu-id="81148-203">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="81148-204">Automatische CSS-Synchronisierung</span><span class="sxs-lookup"><span data-stu-id="81148-204">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="81148-205">Dieses Feature erfordert Version 1.3 der Seitenprüfung.</span><span class="sxs-lookup"><span data-stu-id="81148-205">This feature requires version 1.3 of Page Inspector.</span></span>


<span data-ttu-id="81148-206">Das Feature für die automatische CSS-Synchronisierung können Sie eine CSS-Datei direkt bearbeiten und die Änderungen sofort im Browser Seitenprüfung.</span><span class="sxs-lookup"><span data-stu-id="81148-206">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="81148-207">Klicken Sie auf **prüfen** der Seitenprüfung im Überprüfungsmodus zu platzieren.</span><span class="sxs-lookup"><span data-stu-id="81148-207">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="81148-208">Im Browser der Seitenprüfung verschieben Sie den Mauszeiger über den Abschnitt "Startseite", bis die **div.content-Wrappers** Bezeichnung angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="81148-208">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="81148-209">Klicken Sie auf einmal, um dieses Element auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="81148-209">Click once to select this element.</span></span>

<span data-ttu-id="81148-210">Die **Stile** Fenster zeigt alle CSS-Regeln für dieses Element.</span><span class="sxs-lookup"><span data-stu-id="81148-210">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="81148-211">Scrollen Sie zur Klassenselektor Suchen der .featured .content-Wrapper.</span><span class="sxs-lookup"><span data-stu-id="81148-211">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="81148-212">Klicken Sie auf ".featured .content-Wrapper".</span><span class="sxs-lookup"><span data-stu-id="81148-212">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="81148-213">Seitenprüfung öffnet die CSS-Datei, die dieses Format ("Site.CSS") definiert und hebt den entsprechenden CSS-Stil.</span><span class="sxs-lookup"><span data-stu-id="81148-213">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![CSS-Datei](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

<span data-ttu-id="81148-215">Ändern Sie nun den Wert für `background-color` auf "Rot".</span><span class="sxs-lookup"><span data-stu-id="81148-215">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="81148-216">Die Änderung wird sofort im Browser Seitenprüfung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="81148-216">The change appears immediately in the Page Inspector browser.</span></span>

![Browser der Seitenprüfung](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a><span data-ttu-id="81148-218">Mithilfe der CSS-Farbauswahl</span><span class="sxs-lookup"><span data-stu-id="81148-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="81148-219">Als Nächstes erfahren Sie, wie Sie mit der Seitenprüfung schnell finden und das CSS für den markierten Text in der standardanwendung zu ändern.</span><span class="sxs-lookup"><span data-stu-id="81148-219">Next, you'll learn how to use Page Inspector to quickly find and change the CSS for highlighted text in the default application.</span></span> <span data-ttu-id="81148-220">In diesem Beispiel haben Sie sich entschieden, dass Sie nicht wie die blaue Hervorhebung und es zu einer anderen Farbe ändern möchten.</span><span class="sxs-lookup"><span data-stu-id="81148-220">In this example, you've decided that you don't like the blue highlighting and want to change it to another color.</span></span>

<span data-ttu-id="81148-221">Klicken Sie auf die **prüfen** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="81148-221">Click the **Inspect** button.</span></span>

![Element prüfen](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

<span data-ttu-id="81148-223">Im Fenster Browser der Seitenprüfung bewegen Sie den Mauszeiger über die hervorgehobene "Videos, Lernprogramme und Beispiele" Text, damit das CSS Bezeichnung "kennzeichnen" angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="81148-223">In the Page Inspector browser window, move the mouse pointer over the highlighted "videos, tutorials, and samples" text so that the CSS "mark" label appears.</span></span>

![Mit dem Mauszeiger auf das Element markieren](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

<span data-ttu-id="81148-225">Klicken Sie auf den Text, um es auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="81148-225">Click the text to select it.</span></span> <span data-ttu-id="81148-226">Die entsprechenden CSS-Auswahl Mark wird am unteren Rand der **Stile** Fenster.</span><span class="sxs-lookup"><span data-stu-id="81148-226">The corresponding CSS mark selector appears at the bottom of the **Styles** window.</span></span>

![Markieren Sie die Auswahl im Fenster Stile](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

<span data-ttu-id="81148-228">Klicken Sie auf die Auswahl der Markierung.</span><span class="sxs-lookup"><span data-stu-id="81148-228">Click the mark selector.</span></span> <span data-ttu-id="81148-229">Daraufhin wird die *"Site.CSS"* -Datei für die Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="81148-229">This opens the *Site.css* file for the web application.</span></span> <span data-ttu-id="81148-230">Klicken Sie auf der Registerkarte "Site.CSS", und die entsprechenden CSS-Selektor markiert ist:</span><span class="sxs-lookup"><span data-stu-id="81148-230">Click the Site.css tab, and the corresponding CSS for the selector is highlighted:</span></span>

![Markieren Sie die Auswahl im Stylesheet](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

<span data-ttu-id="81148-232">Wählen Sie aus, und entfernen Sie die Zeile mit der Background-Color-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="81148-232">Select and remove the line with the background-color property.</span></span>

<span data-ttu-id="81148-233">Sie verwenden nun die neue Visual Studio 2012-CSS-Farbauswahl, wählen Sie eine neue Farbe für die **markieren** Background-Color-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="81148-233">You will now use the new Visual Studio 2012 CSS color picker to choose a new color for the **mark** background-color property.</span></span>

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a><span data-ttu-id="81148-234">Mithilfe der Visual Studio 2012 CSS-Farbauswahl</span><span class="sxs-lookup"><span data-stu-id="81148-234">Using the Visual Studio 2012 CSS Color Picker</span></span>

<span data-ttu-id="81148-235">CSS-Editor in Visual Studio 2012 verfügt über eine Farbauswahl angezeigt, die ganz einfach auswählen, und fügen Sie Farben.</span><span class="sxs-lookup"><span data-stu-id="81148-235">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="81148-236">Sie verfügt über eine einfache Farbleiste und eine Auswahl von "Pop-Down", die eine präzisere Kontrolle bietet.</span><span class="sxs-lookup"><span data-stu-id="81148-236">It has a simple color bar and a "pop-down" picker that offers finer control.</span></span>

<span data-ttu-id="81148-237">Die Farbauswahl enthält eine Standardpalette mit Farben, unterstützt standardfarbnamen, Hashcodes, RGB-, RGBA-, HSL- und HSLA-Farben und verwaltet eine Liste der Farben, die Sie zuletzt in das Dokument verwendet haben.</span><span class="sxs-lookup"><span data-stu-id="81148-237">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="81148-238">Klicken Sie in der Zeile, in denen die Background-Color-Eigenschaft wurde, geben Sie "bc", und drücken Sie den Pfeil nach unten einmal.</span><span class="sxs-lookup"><span data-stu-id="81148-238">On the line where the background-color property was, type "bc" and press the down arrow once.</span></span>

<span data-ttu-id="81148-239">Wenn Sie das erste Zeichen jedes Worts in einer Eigenschaft Bindestrich getrennte, z. B. "Background-Color" eingeben, filtert IntelliSense die Liste für Sie nur die Eigenschaften anzeigen, die mit übereinstimmen:</span><span class="sxs-lookup"><span data-stu-id="81148-239">When you type the first character of each word in a hyphen-separated property like "background-color", IntelliSense filters the list for you to show only the properties that match:</span></span>

![IntelliSense gefiltert Werte](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

<span data-ttu-id="81148-241">Geben Sie nun einen Doppelpunkt.</span><span class="sxs-lookup"><span data-stu-id="81148-241">Now type a colon.</span></span> <span data-ttu-id="81148-242">Wenn Sie dies tun, wird der vollständige Hintergrundfarbe Eigenschaftenname eingefügt.</span><span class="sxs-lookup"><span data-stu-id="81148-242">When you do, the full background-color property name is inserted.</span></span> <span data-ttu-id="81148-243">Typ **#** oder **Rgb (**, und die Auswahl Farbleiste angezeigt wird:</span><span class="sxs-lookup"><span data-stu-id="81148-243">Type **#** or **rgb(**, and the color picker bar appears:</span></span>

![Die CSS-Auswahl Farbleiste](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

<span data-ttu-id="81148-245">Um die Funktionsweise die Farbleiste für die Auswahl, klicken Sie auf die Farben mit der Maus oder drücken Sie die nach-unten-Taste, und klicken Sie dann verwenden Sie die linke und Rechte Pfeiltasten, um die Farben zu durchlaufen.</span><span class="sxs-lookup"><span data-stu-id="81148-245">To see how the color picker bar works, click its colors with the mouse pointer, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="81148-246">Wenn Sie eine Farbe besuchen, wird der entsprechende Wert für die Background-Color-Eigenschaft in der Vorschau angezeigt:</span><span class="sxs-lookup"><span data-stu-id="81148-246">When you visit a color, the corresponding value for the background-color property is previewed:</span></span>

![Background-Color-Wert der Eigenschaft, die in der Vorschau angezeigt](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

<span data-ttu-id="81148-248">An diesem Punkt können Sie den Wert, und klicken Sie dann ein Semikolon (;) zum Abschließen des CSS-Eintrags auswählen EINGABETASTE.</span><span class="sxs-lookup"><span data-stu-id="81148-248">At this point, you could press Enter to select the value and then a semicolon (;) to complete the CSS entry.</span></span> <span data-ttu-id="81148-249">Jetzt fahren Sie mit dem nächsten Abschnitt, damit Sie sehen können, die Funktionsweise der Farbe Auswahl Pop aus.</span><span class="sxs-lookup"><span data-stu-id="81148-249">For now, go on to the next section so that you can see how the color picker pop-down works.</span></span>

#### <a name="using-the-color-picker-pop-down"></a><span data-ttu-id="81148-250">Mithilfe der Farbe Auswahl Pop aus</span><span class="sxs-lookup"><span data-stu-id="81148-250">Using the Color Picker Pop-Down</span></span>

<span data-ttu-id="81148-251">Wenn bei der Farbleiste die genaue Farbe, die Sie benötigen, können Sie die Pop Dropdown-Farbauswahl.</span><span class="sxs-lookup"><span data-stu-id="81148-251">When the color bar doesn't have the exact color that you're looking for, you can use the color picker pop-down.</span></span>

<span data-ttu-id="81148-252">Um es zu öffnen, klicken Sie auf die Chevron-Doppelpfeil am rechten Ende der Farbleiste, oder ein-oder zweimal drücken Sie den Pfeil nach unten auf der Tastatur.</span><span class="sxs-lookup"><span data-stu-id="81148-252">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![CSS-Farbe Auswahl Pop-ab](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

<span data-ttu-id="81148-254">Klicken Sie auf eine Farbe aus der senkrechte Strich auf der rechten Seite.</span><span class="sxs-lookup"><span data-stu-id="81148-254">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="81148-255">Dies zeigt einen Farbverlauf für diese Farbe im Hauptfenster.</span><span class="sxs-lookup"><span data-stu-id="81148-255">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="81148-256">Wählen Sie eine Farbe direkt aus der senkrechte Strich durch Drücken der EINGABETASTE, oder klicken Sie auf einem beliebigen Zeitpunkt im Hauptfenster mit größerer Genauigkeit zu wählen.</span><span class="sxs-lookup"><span data-stu-id="81148-256">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="81148-257">Wenn eine Farbe vorliegt, auf dem Bildschirm, die Sie verwenden möchten (es muss keine werden innerhalb der Visual Studio-Benutzeroberfläche), Sie können den Wert erfassen, indem Sie mit dem Tool Pipette in der unteren rechten Ecke.</span><span class="sxs-lookup"><span data-stu-id="81148-257">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="81148-258">Sie können auch die Deckkraft einer Farbe ändern, indem Sie den Schieberegler am unteren Rand der Farbauswahl.</span><span class="sxs-lookup"><span data-stu-id="81148-258">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="81148-259">Auf diese Weise Änderungen Werte RGBA-Werte Farbe, da das RGBA-Format Deckkraft darstellen kann.</span><span class="sxs-lookup"><span data-stu-id="81148-259">Doing so changes color values to RGBA values because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="81148-260">Nachdem Sie eine Farbe ausgewählt haben, drücken Sie die EINGABETASTE, und geben Sie ein Semikolon zum Abschließen des Background-Color-Eintrags in der *"Site.CSS"* Datei.</span><span class="sxs-lookup"><span data-stu-id="81148-260">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="81148-261">Die Seite Inspector-Update-Leiste</span><span class="sxs-lookup"><span data-stu-id="81148-261">The Page Inspector Update Bar</span></span>

<span data-ttu-id="81148-262">Die Seitenprüfung sofort erkennt die Änderung an der *"Site.CSS"* Datei (oder einer beliebigen Datei innerhalb der Anwendung) und eine Warnung in eine Update-Leiste angezeigt.</span><span class="sxs-lookup"><span data-stu-id="81148-262">Page Inspector immediately detects the change to the *Site.css* file (or to any file in the application) and displays an alert in an update bar.</span></span>

![Update-Leiste](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

<span data-ttu-id="81148-264">Speichern Sie alle Dateien und den Browser der Seitenprüfung zu aktualisieren, drücken Strg + Alt + Eingabe, oder klicken Sie auf die Update-Leiste.</span><span class="sxs-lookup"><span data-stu-id="81148-264">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="81148-265">Die Änderung in die Hervorhebungsfarbe wird im Browser angezeigt:</span><span class="sxs-lookup"><span data-stu-id="81148-265">The change in the highlight color appears in the browser:</span></span>

![Hervorhebungsfarbe geändert](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a><span data-ttu-id="81148-267">Beachten Sie, dass Sie bequem Browser der Seitenprüfung direkt aus Visual Studio-Umgebung aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="81148-267">Notice that you conveniently refreshed the Page Inspector browser right from within the Visual Studio environment.</span></span> <span data-ttu-id="81148-268">Verwenden anstelle von einem externen Browser der Seitenprüfung können Sie im Editor zu bleiben, wenn Sie Ihre Webanwendungen entwickeln.</span><span class="sxs-lookup"><span data-stu-id="81148-268">Using Page Inspector instead of an external browser lets you stay in the editor when you develop your web applications.</span></span>

## <a name="see-also"></a><span data-ttu-id="81148-269">Siehe auch</span><span class="sxs-lookup"><span data-stu-id="81148-269">See Also</span></span>

<span data-ttu-id="81148-270">[Einführung in der Seitenprüfung](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (Channel 9-Video)</span><span class="sxs-lookup"><span data-stu-id="81148-270">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (Channel 9 video)</span></span>

<span data-ttu-id="81148-271">[Fehlermeldungen der Seitenprüfung](https://go.microsoft.com/?linkid=9813062) (MSDN)</span><span class="sxs-lookup"><span data-stu-id="81148-271">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
