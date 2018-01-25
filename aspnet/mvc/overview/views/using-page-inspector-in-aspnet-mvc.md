---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: "Verwenden der Seitenprüfung in ASP.NET MVC | Microsoft Docs"
author: rick-anderson
description: "Page Inspector in Visual Studio 2012 ist ein Webentwicklungstool mit einem integrierten Browser. Wählen Sie jedes Element im integrierten Browser und Page Inspector i..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 5b443963a089f96a9dab11b7db4a25451075d6be
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="using-page-inspector-in-aspnet-mvc"></a><span data-ttu-id="0b65b-104">Verwenden der Seitenprüfung in ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="0b65b-104">Using Page Inspector in ASP.NET MVC</span></span>
====================
<span data-ttu-id="0b65b-105">von Tim Ammann</span><span class="sxs-lookup"><span data-stu-id="0b65b-105">by Tim Ammann</span></span>

> <span data-ttu-id="0b65b-106">Page Inspector in Visual Studio 2012 ist ein Webentwicklungstool mit einem integrierten Browser.</span><span class="sxs-lookup"><span data-stu-id="0b65b-106">Page Inspector in Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="0b65b-107">Wählen Sie jedes Element im integrierten Browser und Page Inspector sofort hervorgehoben, des Elements Quell- und CSS.</span><span class="sxs-lookup"><span data-stu-id="0b65b-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="0b65b-108">Sie können alle MVC-Ansicht wechseln, schnelles Auffinden Quellen gerenderten Markups, und Browsertools direkt in Visual Studio-Umgebung verwenden.</span><span class="sxs-lookup"><span data-stu-id="0b65b-108">You can browse any MVC view, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> [<span data-ttu-id="0b65b-109">Zeigt das Video</span><span class="sxs-lookup"><span data-stu-id="0b65b-109">Watch the Video</span></span>](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> <span data-ttu-id="0b65b-110">Dieses Lernprogramm veranschaulicht die Überprüfungsmodus, aktivieren Sie dann schnell suchen und Bearbeiten von Markup und CSS in das Webprojekt.</span><span class="sxs-lookup"><span data-stu-id="0b65b-110">This tutorial shows how to enable Inspection Mode, and then quickly locate and edit markup and CSS within your web project.</span></span> <span data-ttu-id="0b65b-111">Das Lernprogramm verwendet ein MVC-Projekt, aber Sie können auch die Seitenprüfung für [Web Forms](https://go.microsoft.com/?linkid=9802001) und andere ASP.NET-Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="0b65b-111">The tutorial uses an MVC Project, but you can also use Page Inspector for [Web Forms](https://go.microsoft.com/?linkid=9802001) and other ASP.NET applications.</span></span>
> 
> <span data-ttu-id="0b65b-112">Das Lernprogramm enthält die folgenden Abschnitte:</span><span class="sxs-lookup"><span data-stu-id="0b65b-112">The tutorial has the following sections:</span></span>
> 
> - [<span data-ttu-id="0b65b-113">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="0b65b-113">Prerequisites</span></span>](#_1_prerequisites)
> - [<span data-ttu-id="0b65b-114">Erstellen Sie eine Webanwendung</span><span class="sxs-lookup"><span data-stu-id="0b65b-114">Create a Web Application</span></span>](#_2_creating_a)
> - [<span data-ttu-id="0b65b-115">Verwenden Sie die Seitenprüfung auf Durchsuchen, um eine Sicht</span><span class="sxs-lookup"><span data-stu-id="0b65b-115">Use Page Inspector to Browse to a View</span></span>](#_3_using_page)
> - [<span data-ttu-id="0b65b-116">Überprüfungsmodus aktivieren</span><span class="sxs-lookup"><span data-stu-id="0b65b-116">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> - [<span data-ttu-id="0b65b-117">Mit der Seitenprüfung Markup ändern</span><span class="sxs-lookup"><span data-stu-id="0b65b-117">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> - [<span data-ttu-id="0b65b-118">Überprüfungsmodus und das Fenster "HTML"</span><span class="sxs-lookup"><span data-stu-id="0b65b-118">Inspection Mode and the HTML Window</span></span>](#_6_inspection_mode)
> - [<span data-ttu-id="0b65b-119">CSS-Vorschau der Änderungen im Fenster Stile</span><span class="sxs-lookup"><span data-stu-id="0b65b-119">Preview CSS Changes in the Styles window</span></span>](#_7_previewing_css)
> - [<span data-ttu-id="0b65b-120">Automatische CSS-Synchronisierung</span><span class="sxs-lookup"><span data-stu-id="0b65b-120">CSS Auto Sync</span></span>](#css_auto_sync)
> - [<span data-ttu-id="0b65b-121">Mithilfe der CSS-Farbauswahl</span><span class="sxs-lookup"><span data-stu-id="0b65b-121">Using the CSS Color Picker</span></span>](#css_color_picker)
> - [<span data-ttu-id="0b65b-122">Zuordnen von dynamische Seitenelemente für JavaScript</span><span class="sxs-lookup"><span data-stu-id="0b65b-122">Mapping Dynamic Page Elements to JavaScript</span></span>](#map_dynamic_elements)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="0b65b-123">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="0b65b-123">Prerequisites</span></span>

- <span data-ttu-id="0b65b-124">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) oder [Visual Studio Express 2012 für Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="0b65b-124">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="0b65b-125">Verwenden Sie zum Abrufen der neuesten Version des Page Inspector [Webplattform-Installer](https://go.microsoft.com/fwlink/?LinkId=255386) Windows Azure SDK für .NET 2.0 installieren.</span><span class="sxs-lookup"><span data-stu-id="0b65b-125">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Windows Azure SDK for .NET 2.0.</span></span>


<span data-ttu-id="0b65b-126">Seitenprüfung ist mit Microsoft Web Developer Tools enthalten.</span><span class="sxs-lookup"><span data-stu-id="0b65b-126">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="0b65b-127">Die neueste Version ist 1.3.</span><span class="sxs-lookup"><span data-stu-id="0b65b-127">The latest version is 1.3.</span></span> <span data-ttu-id="0b65b-128">Überprüfen Sie die Version haben Sie, Visual Studio ausführen, und wählen Sie **zu Microsoft Visual Studio** aus der **Hilfe** Menü.</span><span class="sxs-lookup"><span data-stu-id="0b65b-128">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="0b65b-129">Erstellen Sie eine Webanwendung</span><span class="sxs-lookup"><span data-stu-id="0b65b-129">Create a Web Application</span></span>

<span data-ttu-id="0b65b-130">Erstellen Sie zunächst eine Webanwendung, der Seitenprüfung mit verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="0b65b-130">First, create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="0b65b-131">Wählen Sie in Visual Studio **Datei** &gt; **neues Projekt**.</span><span class="sxs-lookup"><span data-stu-id="0b65b-131">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="0b65b-132">Erweitern Sie auf der linken Seite **Visual C#-**Option **Web**, und wählen Sie dann **ASP.NET MVC 4-Webanwendung**.</span><span class="sxs-lookup"><span data-stu-id="0b65b-132">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span>

![Neues ASP.NET MVC-Anwendung](using-page-inspector-in-aspnet-mvc/_static/image2.png)

<span data-ttu-id="0b65b-134">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="0b65b-134">Click **OK**.</span></span>

<span data-ttu-id="0b65b-135">In der **neues ASP.NET MVC 4-Projekt** wählen Sie im Dialogfeld **Internetanwendung**.</span><span class="sxs-lookup"><span data-stu-id="0b65b-135">In the **New ASP.NET MVC 4 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="0b65b-136">Lassen Sie **Razor** als Standardansichtsmodul.</span><span class="sxs-lookup"><span data-stu-id="0b65b-136">Leave **Razor** as the default view engine.</span></span>

![Neues ASP.NET MVC-Projekt - Internetanwendung](using-page-inspector-in-aspnet-mvc/_static/image4.png)

<span data-ttu-id="0b65b-138">Öffnet die Anwendung in **Quelle** anzeigen.</span><span class="sxs-lookup"><span data-stu-id="0b65b-138">The application opens in **Source** view.</span></span>

![Neues ASP.NET MVC-Anwendung in der Quellansicht](using-page-inspector-in-aspnet-mvc/_static/image6.png)

<span data-ttu-id="0b65b-140">Nun, dass Sie eine Anwendung mit verwendet haben, können Sie Page Inspector überprüfen und ändern.</span><span class="sxs-lookup"><span data-stu-id="0b65b-140">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a><span data-ttu-id="0b65b-141">Verwenden Sie die Seitenprüfung auf Durchsuchen, um eine Sicht</span><span class="sxs-lookup"><span data-stu-id="0b65b-141">Use Page Inspector to Browse to a View</span></span>

<span data-ttu-id="0b65b-142">In Visual Studio 2012, Sie können mit der rechten Maustaste einer beliebigen Ansicht in Ihrem Projekt, wählen **in Seitenprüfung anzeigen**, und Page Inspector herausfinden, die Route und die Seite angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="0b65b-142">In Visual Studio 2012, you can right-click any view in your project, select **View in Page Inspector**, and Page Inspector will figure out the route and display the page.</span></span>

<span data-ttu-id="0b65b-143">In **Projektmappen-Explorer**, erweitern Sie die **Ansichten** Ordner und dann die **Home** Ordner.</span><span class="sxs-lookup"><span data-stu-id="0b65b-143">In **Solution Explorer**, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="0b65b-144">Klicken Sie mit der mit der rechten Maustaste auf die Index.cshtml-Datei, und wählen Sie **in Seitenprüfung anzeigen**.</span><span class="sxs-lookup"><span data-stu-id="0b65b-144">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

![Index.cshtml in Seitenprüfung anzeigen](using-page-inspector-in-aspnet-mvc/_static/image8.png)

<span data-ttu-id="0b65b-146">Standardmäßig ist die Seitenprüfung als Fenster auf der linken Seite der Visual Studio-Umgebung angedockt.</span><span class="sxs-lookup"><span data-stu-id="0b65b-146">By default, Page Inspector is docked as a window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="0b65b-147">Falls gewünscht, können Sie an anderer Stelle andocken, oder die Verankerung aufheben, des Fensters.</span><span class="sxs-lookup"><span data-stu-id="0b65b-147">If you prefer, you can dock it elsewhere, or undock the window.</span></span> <span data-ttu-id="0b65b-148">Finden Sie unter [Vorgehensweise: anordnen und Andocken von Fenstern](https://msdn.microsoft.com/library/z4y0hsax.aspx).</span><span class="sxs-lookup"><span data-stu-id="0b65b-148">See [How to: Arrange and Dock Windows](https://msdn.microsoft.com/library/z4y0hsax.aspx).</span></span>

<span data-ttu-id="0b65b-149">Im obere Bereich des Fensters Page Inspector zeigt die aktuelle Seite in einem Browserfenster angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0b65b-149">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="0b65b-150">Unteren Bereich zeigt die Seite im HTML-Markup, zusammen mit einigen Registerkarten, mit denen Sie verschiedene Aspekte der Seite zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="0b65b-150">The bottom pane shows the page in HTML markup, along with some tabs that let you inspect different aspects of the page.</span></span> <span data-ttu-id="0b65b-151">Der untere Bereich ist ähnlich wie die [F12 Entwicklertools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="0b65b-151">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span>

![ASP.NET MVC-Anwendung in der Seitenprüfung](using-page-inspector-in-aspnet-mvc/_static/image10.png)

<span data-ttu-id="0b65b-153">In diesem Lernprogramm verwenden Sie die **HTML** und **Stile** Registerkarten, um schnell navigieren und Änderungen an der Anwendung vornehmen.</span><span class="sxs-lookup"><span data-stu-id="0b65b-153">In this tutorial, you will use the **HTML** and **Styles** tabs to navigate quickly and make changes to the application.</span></span>

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a><span data-ttu-id="0b65b-154">EnableInspection-Modus</span><span class="sxs-lookup"><span data-stu-id="0b65b-154">EnableInspection Mode</span></span>

<span data-ttu-id="0b65b-155">Um die Seitenprüfung in den Überprüfungsmodus zu speichern, klicken Sie auf die **Inspect** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="0b65b-155">To put Page Inspector into Inspection Mode, click the **Inspect** button.</span></span> <span data-ttu-id="0b65b-156">Im Überprüfungsmodus Wenn Sie den Mauszeiger über einen beliebigen Teil der gerenderten Seite halten wird das entsprechende Quellmarkup oder der Code hervorgehoben.</span><span class="sxs-lookup"><span data-stu-id="0b65b-156">In Inspection Mode, when you hold the mouse pointer over any part of the rendered page, the corresponding source markup or code is highlighted.</span></span>

![Überprüfungsmodus zum ein-/ausschalten](using-page-inspector-in-aspnet-mvc/_static/image12.png)

<span data-ttu-id="0b65b-158">Jetzt können bewegen Sie die Maus über verschiedene Teile der Seite innerhalb Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="0b65b-158">Now move your mouse over different parts of the page within Page Inspector.</span></span> <span data-ttu-id="0b65b-159">Wie in diesem Fall der Mauszeiger verwandelt sich in einem großen Pluszeichen (+), und das Element unterhalb hervorgehoben wird:</span><span class="sxs-lookup"><span data-stu-id="0b65b-159">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![Mit dem Mauszeiger auf div.content-wrapper](using-page-inspector-in-aspnet-mvc/_static/image14.png)

<span data-ttu-id="0b65b-161">Während Sie den Mauszeiger bewegen, werden in Visual Studio die entsprechenden Razor-Syntax in der Quelldatei hervorgehoben.</span><span class="sxs-lookup"><span data-stu-id="0b65b-161">As you move the mouse pointer, Visual Studio highlights the corresponding Razor syntax in the source file.</span></span> <span data-ttu-id="0b65b-162">Wenn das HTML-Element aus einer anderen Quelldatei stammt, wird die Datei in Visual Studio automatisch geöffnet.</span><span class="sxs-lookup"><span data-stu-id="0b65b-162">If the HTML element comes from another source file, Visual Studio automatically opens the file.</span></span>

<span data-ttu-id="0b65b-163">In der Seitenprüfung die **HTML** Registerkarte zeigt den HTML-Code, der vom Razor-Syntax generiert wurde.</span><span class="sxs-lookup"><span data-stu-id="0b65b-163">In Page Inspector, the **HTML** tab shows the HTML that was generated from the Razor syntax.</span></span> <span data-ttu-id="0b65b-164">Während Sie den Mauszeiger bewegen, werden die HTML-Elemente hervorgehoben.</span><span class="sxs-lookup"><span data-stu-id="0b65b-164">As you move the mouse pointer, the HTML elements are highlighted.</span></span> <span data-ttu-id="0b65b-165">Die **Stile** Registerkarte zeigt die CSS-Regeln für das Element.</span><span class="sxs-lookup"><span data-stu-id="0b65b-165">The **Styles** tab shows the CSS rules for the element.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="0b65b-166">Mit der Seitenprüfung Markup ändern</span><span class="sxs-lookup"><span data-stu-id="0b65b-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="0b65b-167">Page Inspector können Sie Markup suchen, deren Speicherort möglicherweise nicht offensichtlich.</span><span class="sxs-lookup"><span data-stu-id="0b65b-167">Page Inspector lets you find markup whose location might not be obvious.</span></span> <span data-ttu-id="0b65b-168">Anschließend können Sie das Markup ändern und die resultierenden Änderungen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="0b65b-168">Then you can modify the markup and see the resulting changes.</span></span>

<span data-ttu-id="0b65b-169">Um dies zu sehen, klicken Sie auf **Inspect** und führen Sie einen Bildlauf zum unteren Rand der Seite in der Seite Inspektor-Fenster.</span><span class="sxs-lookup"><span data-stu-id="0b65b-169">To see this, click **Inspect** and then scroll to the bottom of the page in the Page Inspector window.</span></span>

<span data-ttu-id="0b65b-170">Wenn Sie den Mauszeiger in der Fußzeile bewegen, wird die Seitenprüfung öffnet die \_Layout.cshtml-Datei und markiert den Abschnitt der Layoutseite, die Sie ausgewählt haben.</span><span class="sxs-lookup"><span data-stu-id="0b65b-170">When you move the mouse pointer into the footer area, Page Inspector opens the \_Layout.cshtml file and highlights the section of the layout page that you have selected.</span></span> <span data-ttu-id="0b65b-171">Wie Sie sehen können, die Fußzeile werden in der Layoutdatei und nicht die Sicht selbst definiert wird.</span><span class="sxs-lookup"><span data-stu-id="0b65b-171">As you can see, the footer are is defined in the layout file, and not the view itself.</span></span>

![Fußzeile](using-page-inspector-in-aspnet-mvc/_static/image16.png)

<span data-ttu-id="0b65b-173">Bewegen Sie den Mauszeiger jetzt über die Zeile mit der Copyright <a id="a"> </a>beachten.</span><span class="sxs-lookup"><span data-stu-id="0b65b-173">Now move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span> <span data-ttu-id="0b65b-174">In der \_Layout.cshtml-Seite, die entsprechende Zeile wird hervorgehoben.</span><span class="sxs-lookup"><span data-stu-id="0b65b-174">In the \_Layout.cshtml page, the corresponding line is highlighted.</span></span>

![Fußzeile copyright Zeile hervorgehoben](using-page-inspector-in-aspnet-mvc/_static/image18.png)

<span data-ttu-id="0b65b-176">Hinzufügen von Text an das Ende der Zeile in der \_Layout.cshtml-Datei.</span><span class="sxs-lookup"><span data-stu-id="0b65b-176">Add some text to the end of the line in the \_Layout.cshtml file.</span></span>

<span data-ttu-id="0b65b-177">&lt;p&gt;&amp;kopieren. @DateTime.Now.Year -Meine ASP.NET MVC-Anwendung einzigartig!  &lt; /p&gt;</span><span class="sxs-lookup"><span data-stu-id="0b65b-177">&lt;p&gt;&amp;copy; @DateTime.Now.Year - My ASP.NET MVC Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="0b65b-178">Jetzt, drücken Sie Strg + Alt + Eingabe, oder klicken Sie auf die Update-Leiste, um die Ergebnisse im Browserfenster Page Inspector anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="0b65b-178">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![Meine ASP.NET Anwendung Rocks!](using-page-inspector-in-aspnet-mvc/_static/image20.png)

<span data-ttu-id="0b65b-180">Möglicherweise haben man, dass die Fußzeile, die in Index.cshtml definiert, aber es war in der \_Layout.cshtml und Page Inspector gefundenen für Sie.</span><span class="sxs-lookup"><span data-stu-id="0b65b-180">You might have thought that the footer defined in Index.cshtml, but it turned out to be in the \_Layout.cshtml, and Page Inspector found it for you.</span></span>

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="0b65b-181">Überprüfungsmodus und das Fenster "HTML"</span><span class="sxs-lookup"><span data-stu-id="0b65b-181">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="0b65b-182">Als Nächstes müssen Sie einen kurzen Blick auf das HTML-Fenster, und wie sie Elemente für Sie zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="0b65b-182">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="0b65b-183">Klicken Sie auf **Inspect** Page Inspector im Überprüfungsmodus ablegen.</span><span class="sxs-lookup"><span data-stu-id="0b65b-183">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="0b65b-184">Klicken Sie auf den oberen Teil der Seite mit dem Hinweis "Die Logohere" angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0b65b-184">Click the top part of the page that says "Your logohere".</span></span> <span data-ttu-id="0b65b-185">Sie sind ein bestimmtes Element im Detail, damit die Anzeige im Browserfenster nicht mehr geändert werden, wie Sie den Mauszeiger untersucht.</span><span class="sxs-lookup"><span data-stu-id="0b65b-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="0b65b-186">Verschieben Sie nun den Mauszeiger an die **HTML** Fenster.</span><span class="sxs-lookup"><span data-stu-id="0b65b-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="0b65b-187">Während Sie den Mauszeiger bewegen, werden Page Inspector das Element innerhalb der **HTML** Fenster und das entsprechende Element im Browserfenster hervorgehoben.</span><span class="sxs-lookup"><span data-stu-id="0b65b-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![HTML-Fenster](using-page-inspector-in-aspnet-mvc/_static/image22.png)

<span data-ttu-id="0b65b-189">Wie bereits zuvor können die Seitenprüfung öffnet die \_Layout.cshtml-Datei für Sie in einer temporären Registerkarte. Klicken Sie auf die \_Layout.cshtml temporäre Registerkarte und das entsprechende Markup wird hervorgehoben werden die &lt;Header&gt; Abschnitt für Sie:</span><span class="sxs-lookup"><span data-stu-id="0b65b-189">As before, Page Inspector opens the \_Layout.cshtml file for you in a temporary tab. Click the \_Layout.cshtml temporary tab, and the corresponding markup will be highlighted in the &lt;header&gt; section for you:</span></span>

![Hervorgehobene markup](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="0b65b-191">CSS-Vorschau der Änderungen im Fenster Stile</span><span class="sxs-lookup"><span data-stu-id="0b65b-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="0b65b-192">Als Nächstes verwenden Sie die Seitenprüfung **Stile** Fenster aus, um Änderungen an CSS in der Vorschau anzeigen.</span><span class="sxs-lookup"><span data-stu-id="0b65b-192">Next, you will use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="0b65b-193">Klicken Sie auf **Inspect** Page Inspector im Überprüfungsmodus ablegen.</span><span class="sxs-lookup"><span data-stu-id="0b65b-193">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="0b65b-194">Im Browserfenster Page Inspector verschieben Sie den Mauszeiger über den Abschnitt "Startseite", bis die **div.content Wrapper** Bezeichnung angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="0b65b-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![Mit dem Mauszeiger auf div.content-wrapper](using-page-inspector-in-aspnet-mvc/_static/image26.png)

<span data-ttu-id="0b65b-196">Klicken Sie in dem Abschnitt div.content Wrapper einmal, und verschieben Sie den Mauszeiger an die **Stile** Fenster.</span><span class="sxs-lookup"><span data-stu-id="0b65b-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="0b65b-197">Die **Stile** Fenster enthält alle CSS-Regeln für dieses Element.</span><span class="sxs-lookup"><span data-stu-id="0b65b-197">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="0b65b-198">Blättern Sie zum Suchen der .featured .content-Wrapper Klassenauswahl.</span><span class="sxs-lookup"><span data-stu-id="0b65b-198">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="0b65b-199">Deaktivieren Sie nun das Kontrollkästchen für die Hintergrundfarbe Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="0b65b-199">Now clear the checkbox for the background-color property.</span></span>

![Clear-Hintergrundfarbe](using-page-inspector-in-aspnet-mvc/_static/image28.png)

<span data-ttu-id="0b65b-201">Beachten Sie, wie die Änderung sofort im Browserfenster Page Inspector in der Vorschau anzeigt.</span><span class="sxs-lookup"><span data-stu-id="0b65b-201">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="0b65b-202">Aktivieren Sie das Kontrollkästchen erneut, doppelklicken Sie auf den Wert der Eigenschaft, und ändern Sie ihn in Rot.</span><span class="sxs-lookup"><span data-stu-id="0b65b-202">Select the checkbox again, then double-click the property value and change it to red.</span></span> <span data-ttu-id="0b65b-203">Die Änderung wird sofort gezeigt:</span><span class="sxs-lookup"><span data-stu-id="0b65b-203">The change shows immediately:</span></span>

![Rote Hintergrundfarbe](using-page-inspector-in-aspnet-mvc/_static/image30.png)

<span data-ttu-id="0b65b-205">Die **Stile** Fenster macht es einfach, testen und eine Vorschau CSS ändert, bevor die Änderungen zu, den Stil übernehmen Stylesheet selbst.</span><span class="sxs-lookup"><span data-stu-id="0b65b-205">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="0b65b-206">Automatische CSS-Synchronisierung</span><span class="sxs-lookup"><span data-stu-id="0b65b-206">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="0b65b-207">Dieses Feature erfordert Version 1.3 der Seitenprüfung.</span><span class="sxs-lookup"><span data-stu-id="0b65b-207">This feature requires version 1.3 of Page Inspector.</span></span>


<span data-ttu-id="0b65b-208">Das Feature für die automatische CSS-Synchronisierung können Sie eine CSS-Datei direkt bearbeiten und die Änderungen werden sofort im Browser Seitenprüfung.</span><span class="sxs-lookup"><span data-stu-id="0b65b-208">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="0b65b-209">Klicken Sie auf **Inspect** Page Inspector im Überprüfungsmodus ablegen.</span><span class="sxs-lookup"><span data-stu-id="0b65b-209">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="0b65b-210">Im Browser Seitenprüfung verschieben Sie den Mauszeiger über den Abschnitt "Startseite", bis die **div.content Wrapper** Bezeichnung angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="0b65b-210">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="0b65b-211">Klicken Sie auf einmal, um dieses Element auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="0b65b-211">Click once to select this element.</span></span>

<span data-ttu-id="0b65b-212">Die **Stile** Fenster enthält alle CSS-Regeln für dieses Element.</span><span class="sxs-lookup"><span data-stu-id="0b65b-212">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="0b65b-213">Blättern Sie zum Suchen der .featured .content-Wrapper Klassenauswahl.</span><span class="sxs-lookup"><span data-stu-id="0b65b-213">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="0b65b-214">Klicken Sie auf ".featured .content-Wrapper".</span><span class="sxs-lookup"><span data-stu-id="0b65b-214">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="0b65b-215">Die Seitenprüfung öffnet die CSS-Datei, die definiert dieses Format ("Site.CSS" ändern) und hebt die entsprechenden CSS-Stil.</span><span class="sxs-lookup"><span data-stu-id="0b65b-215">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

<span data-ttu-id="0b65b-216">Jetzt ändern Sie den Wert für `background-color` auf "Rot".</span><span class="sxs-lookup"><span data-stu-id="0b65b-216">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="0b65b-217">Die Änderung wird sofort im Browser Seitenprüfung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0b65b-217">The change appears immediately in the Page Inspector browser.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a><span data-ttu-id="0b65b-218">Mithilfe der CSS-Farbauswahl</span><span class="sxs-lookup"><span data-stu-id="0b65b-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="0b65b-219">CSS-Editor in Visual Studio 2012 verfügt über ein Farbwähler, der es einfach, wählen aus, und legen Sie Farben.</span><span class="sxs-lookup"><span data-stu-id="0b65b-219">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="0b65b-220">Die Farbauswahl enthält eine Standardpalette mit Farben, unterstützt standardfarbnamen, Hashcodes, RGB-, RGBA-HSL- und HSLA Farben und verwaltet eine Liste der Farben, die Sie im Dokument zuletzt verwendet haben.</span><span class="sxs-lookup"><span data-stu-id="0b65b-220">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="0b65b-221">Im vorherigen Abschnitt den Wert geändert die `background-color` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="0b65b-221">In the previous section, you changed the value of the `background-color` property.</span></span> <span data-ttu-id="0b65b-222">Um die Farbauswahl aufzurufen, platzieren Sie die Einfügemarke nach dem Eigenschaftsnamen und geben  **#**  oder **Rgb (**.</span><span class="sxs-lookup"><span data-stu-id="0b65b-222">To invoke the color picker, place the insertion point after the property name and type **#** or **rgb(**.</span></span>

![Die CSS-Auswahl Farbleiste](using-page-inspector-in-aspnet-mvc/_static/image36.png)

<span data-ttu-id="0b65b-224">Klicken Sie auf eine Farbe aus, wählen Sie ihn aus, oder drücken die nach-unten-Taste, und klicken Sie dann verwenden Sie die linke und Rechte Pfeiltasten, um die Farben zu durchlaufen.</span><span class="sxs-lookup"><span data-stu-id="0b65b-224">Click on a color to select it, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="0b65b-225">Wenn Sie eine Farbe besuchen, wird der entsprechende Hexadezimalwert in der Vorschau angezeigt:</span><span class="sxs-lookup"><span data-stu-id="0b65b-225">When you visit a color, the corresponding hex value is previewed:</span></span>

![Background-Color-Wert der Eigenschaft, die in der Vorschau angezeigt](using-page-inspector-in-aspnet-mvc/_static/image38.png)

<span data-ttu-id="0b65b-227">Wenn der Farbleiste nicht den gewünschten exakte der Farbe haben, können Sie der Farbe Datumsauswahl Pop aus.</span><span class="sxs-lookup"><span data-stu-id="0b65b-227">If the color bar doesn't have the exact color you want, you can use the color picker pop-down.</span></span> <span data-ttu-id="0b65b-228">Um sie zu öffnen, klicken Sie auf das doppelte Chevron am rechten Ende der Farbleiste, oder drücken Sie den Pfeil nach unten ein-oder zweimal auf der Tastatur.</span><span class="sxs-lookup"><span data-stu-id="0b65b-228">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![CSS-Farbe Datumsauswahl Pop aus](using-page-inspector-in-aspnet-mvc/_static/image40.png)

<span data-ttu-id="0b65b-230">Klicken Sie auf eine Farbe aus der senkrechte Strich auf der rechten Seite.</span><span class="sxs-lookup"><span data-stu-id="0b65b-230">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="0b65b-231">Dies zeigt einen Verlauf für diese Farbe im Hauptfenster.</span><span class="sxs-lookup"><span data-stu-id="0b65b-231">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="0b65b-232">Wählen Sie eine Farbe direkt aus der senkrechte Strich durch Drücken der EINGABETASTE, oder klicken Sie auf einem beliebigen Punkt im Hauptfenster mit größerer Genauigkeit auswählen.</span><span class="sxs-lookup"><span data-stu-id="0b65b-232">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="0b65b-233">Wenn eine Farbe vorliegt, auf dem Bildschirm, die Sie verwenden möchten (keinen innerhalb der Visual Studio-Benutzeroberfläche), können Sie den Wert mithilfe der Pipette in der unteren rechten Ecke erfassen.</span><span class="sxs-lookup"><span data-stu-id="0b65b-233">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="0b65b-234">Sie können auch die Deckkraft einer Farbe ändern, mithilfe des Schiebereglers am unteren Rand der Farbauswahl.</span><span class="sxs-lookup"><span data-stu-id="0b65b-234">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="0b65b-235">Auf diese Weise so Änderungen RGBA-Werten, Farbe, da das RGBA-Format Deckkraft darstellen kann.</span><span class="sxs-lookup"><span data-stu-id="0b65b-235">Doing so changes color values to RGBA values, because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="0b65b-236">Nachdem Sie eine Farbe ausgewählt haben, drücken Sie die EINGABETASTE, und geben Sie ein Semikolon zum Abschließen des Hintergrundfarbe Eintrags in der *"Site.CSS" ändern* Datei.</span><span class="sxs-lookup"><span data-stu-id="0b65b-236">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="0b65b-237">Die Seite Inspektor Update-Leiste</span><span class="sxs-lookup"><span data-stu-id="0b65b-237">The Page Inspector Update Bar</span></span>

<span data-ttu-id="0b65b-238">Page Inspector sofort erkennt die Änderung an der *"Site.CSS" ändern* Datei und eine Warnung in eine Update-Leiste angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0b65b-238">Page Inspector immediately detects the change to the *Site.css* file and displays an alert in an update bar.</span></span>

![Update-Leiste](using-page-inspector-in-aspnet-mvc/_static/image42.png)

<span data-ttu-id="0b65b-240">Speichern Sie alle Dateien und Browser der Seitenprüfung zu aktualisieren, drücken Strg + Alt + Eingabe, oder klicken Sie auf die Update-Leiste.</span><span class="sxs-lookup"><span data-stu-id="0b65b-240">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="0b65b-241">Die Änderung in der Hervorhebungsfarbe wird im Browser angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0b65b-241">The change in the highlight color appears in the browser.</span></span>

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a><span data-ttu-id="0b65b-242">Zuordnen von dynamische Seitenelemente für JavaScript</span><span class="sxs-lookup"><span data-stu-id="0b65b-242">Mapping Dynamic Page Elements to JavaScript</span></span>

<span data-ttu-id="0b65b-243">In modernen Webanwendungen werden die Elemente auf der Seite häufig dynamisch mit JavaScript generiert.</span><span class="sxs-lookup"><span data-stu-id="0b65b-243">In modern web applications, elements in the page are often generated dynamically with JavaScript.</span></span> <span data-ttu-id="0b65b-244">Das bedeutet, dass es keine statische Markup (HTML- oder Razor), das diese Seitenelemente entspricht.</span><span class="sxs-lookup"><span data-stu-id="0b65b-244">That means there is no static markup (either HTML or Razor) that corresponds to these page elements.</span></span>

<span data-ttu-id="0b65b-245">Mit Version 1.3 können Page Inspector Elemente zuordnen, die auf der Seite wieder auf den entsprechenden JavaScript-Code dynamisch hinzugefügt wurden.</span><span class="sxs-lookup"><span data-stu-id="0b65b-245">With version 1.3, Page Inspector can now map items that were dynamically added to the page back to the corresponding JavaScript code.</span></span> <span data-ttu-id="0b65b-246">Um diese Funktion zu veranschaulichen, verwenden wir die [einzelnen Seite Anwendung (SPA) Vorlage](../../../single-page-application/overview/introduction/knockoutjs-template.md).</span><span class="sxs-lookup"><span data-stu-id="0b65b-246">To demonstrate this feature, we'll use the [Single Page Application (SPA) template](../../../single-page-application/overview/introduction/knockoutjs-template.md).</span></span>

> [!NOTE]
> <span data-ttu-id="0b65b-247">Die Vorlage SPA erfordert die [ASP.NET und Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="0b65b-247">The SPA template requires the [ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) update.</span></span>


<span data-ttu-id="0b65b-248">Wählen Sie in Visual Studio **Datei** &gt; **neues Projekt**.</span><span class="sxs-lookup"><span data-stu-id="0b65b-248">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="0b65b-249">Erweitern Sie auf der linken Seite **Visual C#-**Option **Web**, und wählen Sie dann **ASP.NET MVC 4-Webanwendung**.</span><span class="sxs-lookup"><span data-stu-id="0b65b-249">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span> <span data-ttu-id="0b65b-250">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="0b65b-250">Click **OK**.</span></span>

<span data-ttu-id="0b65b-251">In der **neues ASP.NET MVC 4-Projekt** wählen Sie im Dialogfeld **einseitigen Anwendung**.</span><span class="sxs-lookup"><span data-stu-id="0b65b-251">In the **New ASP.NET MVC 4 Project** dialog, select **Single Page Application**.</span></span>

<span data-ttu-id="0b65b-252">Erweitern Sie im Projektmappen-Explorer die **Ansichten** Ordner und dann die **Home** Ordner.</span><span class="sxs-lookup"><span data-stu-id="0b65b-252">In Solution Explorer, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="0b65b-253">Klicken Sie mit der mit der rechten Maustaste auf die Index.cshtml-Datei, und wählen Sie **in Seitenprüfung anzeigen**.</span><span class="sxs-lookup"><span data-stu-id="0b65b-253">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

<span data-ttu-id="0b65b-254">Erstes, d. h. angezeigten im Browser Seitenprüfung ist eine Anmeldeseite.</span><span class="sxs-lookup"><span data-stu-id="0b65b-254">The first thing that is displayed in the Page Inspector browser is a login page.</span></span> <span data-ttu-id="0b65b-255">Klicken Sie auf "Registrieren", und erstellen Sie einen Benutzernamen und Kennwort.</span><span class="sxs-lookup"><span data-stu-id="0b65b-255">Click "Sign Up" and create a user name and password.</span></span> <span data-ttu-id="0b65b-256">Sobald Sie sich anmelden, wird die Anwendung Sie anmeldet und erstellt eine to-Do-Liste mit einige Beispiel-Elemente.</span><span class="sxs-lookup"><span data-stu-id="0b65b-256">Once you sign up, the application logs you in and creates a to-do list with some sample items.</span></span>

<span data-ttu-id="0b65b-257">Klicken Sie auf **Inspect** Page Inspector im Überprüfungsmodus ablegen.</span><span class="sxs-lookup"><span data-stu-id="0b65b-257">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span> <span data-ttu-id="0b65b-258">Klicken Sie im Browser Seitenprüfung auf eines der to-do-Elemente.</span><span class="sxs-lookup"><span data-stu-id="0b65b-258">In the Page Inspector browser, click on one of the to-do items.</span></span> <span data-ttu-id="0b65b-259">Beachten Sie, dass anstelle von blau hervorgehoben wird, das Element in Orange ändert, die mit "JS" neben dem Elementnamen hervorgehoben ist.</span><span class="sxs-lookup"><span data-stu-id="0b65b-259">Notice that instead of being highlighted in blue, the element is highlighted in orange, with "JS" next to the element name.</span></span> <span data-ttu-id="0b65b-260">Dies gibt an, dass das Element dynamisch über ein Skript erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="0b65b-260">This indicates that the element was created dynamically through script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

<span data-ttu-id="0b65b-261">Darüber hinaus eine orangefarbene Unterstreichung wird angezeigt, auf die **Aufrufliste** Registerkarte. Dies bedeutet, dass die **Aufrufliste** Bereich verfügt über weitere Informationen über das Element.</span><span class="sxs-lookup"><span data-stu-id="0b65b-261">In addition, an orange underline appears on the **Call Stack** tab. This indicates that the **Call Stack** pane has more information about the element.</span></span>

<span data-ttu-id="0b65b-262">Klicken Sie auf die **Aufrufliste** Registerkarte. Die **Aufrufliste** Bereich zeigt die Aufrufliste für den JavaScript-Aufruf, der das Element erstellt.</span><span class="sxs-lookup"><span data-stu-id="0b65b-262">Click on the **Call Stack** tab. The **Call Stack** pane shows the call stack for the JavaScript call that created the element.</span></span> <span data-ttu-id="0b65b-263">Aufrufe von externen Bibliotheken wie z. B. jQuery werden reduziert, so, dass Sie die Aufrufe an das Anwendungsskript leicht erkennen können.</span><span class="sxs-lookup"><span data-stu-id="0b65b-263">Calls to external libraries such as jQuery are collapsed, so that you can easily see the calls to your application script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

<span data-ttu-id="0b65b-264">Die Knoten mit der Bezeichnung "Bibliotheken" können erweitern, um die vollständige Aufrufliste, darunter auch Aufrufe an externe Bibliotheken finden Sie unter:</span><span class="sxs-lookup"><span data-stu-id="0b65b-264">To see the full stack, including calls to external libraries, you can expand the nodes labeled "External Libraries":</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

<span data-ttu-id="0b65b-265">Wenn Sie ein Element in der Aufrufliste klicken, wird von Visual Studio öffnet die Codedatei und das entsprechende Skript hervorgehoben.</span><span class="sxs-lookup"><span data-stu-id="0b65b-265">If you click an item in the call stack, Visual Studio opens the code file and highlights the corresponding script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a><span data-ttu-id="0b65b-266">Siehe auch</span><span class="sxs-lookup"><span data-stu-id="0b65b-266">See Also</span></span>

<span data-ttu-id="0b65b-267">[Einführung in ASP.NET MVC 4 mit Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (ASP.NET-Website)</span><span class="sxs-lookup"><span data-stu-id="0b65b-267">[Intro to ASP.NET MVC 4 with Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (ASP.net website)</span></span>

<span data-ttu-id="0b65b-268">[Einführung in Seitenprüfung](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (Channel 9-Video)</span><span class="sxs-lookup"><span data-stu-id="0b65b-268">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (Channel 9 video)</span></span>

<span data-ttu-id="0b65b-269">[Fehlermeldungen der Seitenprüfung](https://go.microsoft.com/?linkid=9813062) (MSDN)</span><span class="sxs-lookup"><span data-stu-id="0b65b-269">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
