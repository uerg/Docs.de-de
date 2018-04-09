---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
title: Neues in ASP.NET und Webentwicklung in Visual Studio 2012 | Microsoft Docs
author: rick-anderson
description: Die neue Version von Visual Studio stellt eine Reihe von Verbesserungen konzentriert sich auf die Leistung und die Leistung verbessern, bei der Arbeit mit Web-Technologien...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 6d40d276-1642-4a77-b6c9-02ac914f6805
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 00b43cc548df44edded925521991a095ed856494
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="whats-new-in-aspnet-and-web-development-in-visual-studio-2012"></a><span data-ttu-id="dade5-103">Was ist neu in ASP.NET und Webentwicklung in Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="dade5-103">What's New in ASP.NET and Web Development in Visual Studio 2012</span></span>
====================
<span data-ttu-id="dade5-104">Durch [Web Lager Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="dade5-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="dade5-105">Die neue Version von Visual Studio enthält eine Reihe von Verbesserungen konzentriert sich auf die Leistung und die Leistung verbessern, bei der Arbeit mit Web-Technologien.</span><span class="sxs-lookup"><span data-stu-id="dade5-105">The new version of Visual Studio introduces a number of enhancements focused on improving the experience and performance when working with Web technologies.</span></span> <span data-ttu-id="dade5-106">Visual Studio-Editoren für CSS, JavaScript und HTML haben vollständig überarbeitet, um viele der am häufigsten Bedarf Code Aids, z. B. IntelliSense und automatische Einzug einzuschließen.</span><span class="sxs-lookup"><span data-stu-id="dade5-106">Visual Studio Editors for CSS, JavaScript and HTML have been completely revamped to include many of the most in-demand code aids, such as IntelliSense and automatic indentation.</span></span> <span data-ttu-id="dade5-107">Bezüglich der Leistung sind Bündelung und Minimierung jetzt integriert, wie integrierte Funktionen zum Seite problemlos reduzieren Mal geladen.</span><span class="sxs-lookup"><span data-stu-id="dade5-107">Regarding performance, bundling and minification are now integrated as built-in features to easily reduce page load time.</span></span>
> 
> <span data-ttu-id="dade5-108">Visual Studio ermöglicht Ihnen, mit der neuesten Technologien für die Website zu arbeiten.</span><span class="sxs-lookup"><span data-stu-id="dade5-108">Visual Studio enables you to work with the latest website technologies.</span></span> <span data-ttu-id="dade5-109">Browserübergreifende CSS3 Ausschnitte können Sie sicherstellen, dass Ihre Website unabhängig von der Clientplattform funktioniert, bei der auch die Vorteile der neuen HTML5-Elemente und Funktionen.</span><span class="sxs-lookup"><span data-stu-id="dade5-109">You can use cross-browser CSS3 Snippets to make sure your site works regardless of the client platform while also taking advantage of the new HTML5 elements and features.</span></span>
> 
> <span data-ttu-id="dade5-110">Schreiben und die profilerstellung für JavaScript-Code sollten nicht mit dieser Version von Visual Studio einfacher sein.</span><span class="sxs-lookup"><span data-stu-id="dade5-110">Writing and profiling JavaScript code should be easier with this Visual Studio version.</span></span> <span data-ttu-id="dade5-111">IntelliSense-Listen, die integrierten Funktionen des XML-Dokumentation und Navigation sind jetzt für JavaScript-Code verfügbar.</span><span class="sxs-lookup"><span data-stu-id="dade5-111">IntelliSense lists, integrated XML documentation and navigation features are now available for JavaScript code.</span></span> <span data-ttu-id="dade5-112">Sie haben jetzt den JavaScript-Katalog jederzeit griffbereit.</span><span class="sxs-lookup"><span data-stu-id="dade5-112">You now have the JavaScript catalog at your fingertips.</span></span> <span data-ttu-id="dade5-113">Darüber hinaus können Sie überprüfen ECMAScript5-Kompatibilität mit Skripts und Syntaxfehler in einem frühen Stadium zu erkennen.</span><span class="sxs-lookup"><span data-stu-id="dade5-113">Additionally, you can check ECMAScript5 compliance with your scripts and detect syntax errors at an early stage.</span></span>
> 
> <span data-ttu-id="dade5-114">Zuletzt jedoch nicht mindestens implementiert diese Version von Visual Studio, integrierte Bündelung und Minimierung.</span><span class="sxs-lookup"><span data-stu-id="dade5-114">Last but not least, this Visual Studio version implements built-in bundling and minification.</span></span> <span data-ttu-id="dade5-115">Ihre Skriptdateien und Stylesheets werden verpackt und komprimiert, sodass die Website schneller ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="dade5-115">Your script files and style sheets will be packed and compressed so that the site performs faster.</span></span>
> 
> <span data-ttu-id="dade5-116">Diese Übung führt Sie durch die Optimierungen und neuen Funktionen, die zuvor beschriebenen durch Anwenden von kleinere Änderungen auf eine Beispielwebanwendung im Quellordner bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="dade5-116">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>
> 
> <span data-ttu-id="dade5-117">Alle Beispielcode und Codeausschnitte sind im Web Lager Training Kit unter enthalten [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="dade5-117">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="dade5-118">Ziele</span><span class="sxs-lookup"><span data-stu-id="dade5-118">Objectives</span></span>

<span data-ttu-id="dade5-119">In dieser praktische Übungseinheiten, erfahren Sie, wie Sie:</span><span class="sxs-lookup"><span data-stu-id="dade5-119">In this hands on lab, you will learn how to:</span></span>

- <span data-ttu-id="dade5-120">Verwenden Sie die neuen Features und Verbesserungen in der CSS-editor</span><span class="sxs-lookup"><span data-stu-id="dade5-120">Use the new features and improvements in the CSS editor</span></span>
- <span data-ttu-id="dade5-121">Verwenden Sie die neuen Features und Verbesserungen in der HTML-editor</span><span class="sxs-lookup"><span data-stu-id="dade5-121">Use the new features and improvements in the HTML editor</span></span>
- <span data-ttu-id="dade5-122">Verwenden Sie die neuen Features und Verbesserungen in der JavaScript-editor</span><span class="sxs-lookup"><span data-stu-id="dade5-122">Use the new features and improvements in the JavaScript editor</span></span>
- <span data-ttu-id="dade5-123">Konfigurieren und Verwenden von Bündelung und Minimierung</span><span class="sxs-lookup"><span data-stu-id="dade5-123">Configure and use bundling and minification</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="dade5-124">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="dade5-124">Prerequisites</span></span>

- <span data-ttu-id="dade5-125">[Microsoft Visual Studio Express 2012 für das Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder sogar eine höhere (Lesen [Anhang A](#AppendixA) Anleitungen zur Installation).</span><span class="sxs-lookup"><span data-stu-id="dade5-125">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>
- <span data-ttu-id="dade5-126">[Windows PowerShell](https://support.microsoft.com/kb/968930/) (für Setupskripts - bereits installiert unter Windows 8 und Windows Server 2008 R2)</span><span class="sxs-lookup"><span data-stu-id="dade5-126">[Windows PowerShell](https://support.microsoft.com/kb/968930/) (for setup scripts - already installed on Windows 8 and Windows Server 2008 R2)</span></span>
- <span data-ttu-id="dade5-127">[Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home) - oder einem kompatiblen HTML5-Browser</span><span class="sxs-lookup"><span data-stu-id="dade5-127">[Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home) - or an HTML5 compliant browser</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="dade5-128">Übungen</span><span class="sxs-lookup"><span data-stu-id="dade5-128">Exercises</span></span>

<span data-ttu-id="dade5-129">Diese praktische Übungseinheiten umfasst die folgenden Übungen durcharbeiten:</span><span class="sxs-lookup"><span data-stu-id="dade5-129">This hands on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="dade5-130">Übung 1: Was ist neu in der CSS-Editor</span><span class="sxs-lookup"><span data-stu-id="dade5-130">Exercise 1: What's New in the CSS Editor</span></span>](#Exercise1)
2. [<span data-ttu-id="dade5-131">Übung 2: Was ist neu im HTML-Editor</span><span class="sxs-lookup"><span data-stu-id="dade5-131">Exercise 2: What's New in the HTML Editor</span></span>](#Exercise2)
3. [<span data-ttu-id="dade5-132">Übung 3: Was ist neu in der JavaScript-Editor</span><span class="sxs-lookup"><span data-stu-id="dade5-132">Exercise 3: What's New in the JavaScript Editor</span></span>](#Exercise3)
4. [<span data-ttu-id="dade5-133">Übung 4: Bündelung und Minimierung</span><span class="sxs-lookup"><span data-stu-id="dade5-133">Exercise 4: Bundling and Minification</span></span>](#Exercise4)

<span data-ttu-id="dade5-134">Geschätzte Zeit zum Abschließen dieser testumgebung: **60 Minuten**.</span><span class="sxs-lookup"><span data-stu-id="dade5-134">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Whats_New_in_the_CSS_Editor"></a>
### <a name="exercise-1-whats-new-in-the-css-editor"></a><span data-ttu-id="dade5-135">Übung 1: Was ist neu in der CSS-Editor</span><span class="sxs-lookup"><span data-stu-id="dade5-135">Exercise 1: What's New in the CSS Editor</span></span>

<span data-ttu-id="dade5-136">Web-Entwickler sollten mit viele der Probleme im Zusammenhang mit der Bearbeitung von CSS-vertraut sein.</span><span class="sxs-lookup"><span data-stu-id="dade5-136">Web developers should be familiar with many of the difficulties related to CSS editing.</span></span> <span data-ttu-id="dade5-137">Eines der größten Probleme von CSS-Stilen ist browserübergreifende Kompatibilität.</span><span class="sxs-lookup"><span data-stu-id="dade5-137">One of the biggest issues of CSS styling is cross-browser compatibility.</span></span> <span data-ttu-id="dade5-138">Es kommt häufig vor, dass nach dem Anwenden von Stilen auf Ihrer Website, Sie bemerken, dass er unterscheidet, wenn sie in einem anderen Browser oder das Gerät zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="dade5-138">It often happens that, after applying styles to your site, you notice that it looks different if you open it in another browser or device.</span></span> <span data-ttu-id="dade5-139">Aus diesem Grund können Sie eine sehr viel Zeit verbringen, beachten Sie, dass Sie schließlich in einem Browser funktioniert erleichtern, es, in der anderen aufgegliedert wird diese visual Probleme behoben.</span><span class="sxs-lookup"><span data-stu-id="dade5-139">Therefore, you may spend a considerable time on fixing those visual issues to realize that, when you finally make it work in one browser, it is broken in the others.</span></span>

<span data-ttu-id="dade5-140">Visual Studio enthält nun Funktionen, mit deren Hilfe Entwickler zugreifen, arbeiten und CSS-Stylesheets effektiv zu organisieren.</span><span class="sxs-lookup"><span data-stu-id="dade5-140">Visual Studio now includes features that help developers access, work and organize CSS style sheets effectively.</span></span> <span data-ttu-id="dade5-141">Während dieser Übung werden Sie die neuen Funktionen für eine effektive Organisation und die Edition sowie die CSS3-Codeausschnitte für browserübergreifende Kompatibilität erfüllen.</span><span class="sxs-lookup"><span data-stu-id="dade5-141">Throughout this exercise, you will meet the new features for an effective organization and edition, as well as the CSS3 Code Snippets for cross-browser compatibility.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_New_Editor_Features"></a>
#### <a name="task-1---new-editor-features"></a><span data-ttu-id="dade5-142">Aufgabe 1 – neue Features im Editor</span><span class="sxs-lookup"><span data-stu-id="dade5-142">Task 1 - New Editor Features</span></span>

<span data-ttu-id="dade5-143">In dieser Aufgabe werden die neuen Funktionen der CSS-Editor ermittelt werden.</span><span class="sxs-lookup"><span data-stu-id="dade5-143">In this task, you will discover the new features of the CSS Editor.</span></span> <span data-ttu-id="dade5-144">Diese neuen Editor helfen Ihnen die Ihre Produktivität zu steigern, durch den neuen intelligenten Einzug, der verbesserte Codekommentare und verbesserte IntelliSense-Liste nutzen.</span><span class="sxs-lookup"><span data-stu-id="dade5-144">This new editor will help you increase your productivity by taking advantage of the new smart indentation, the improved code comments and the enhanced IntelliSense list.</span></span>

1. <span data-ttu-id="dade5-145">Starten Sie **Visual Studio** , und öffnen Sie die **WhatsNewASPNET.sln** Projektmappe befindet sich in der **Source\WhatsNewASPNET** Ordner dieser Anleitung.</span><span class="sxs-lookup"><span data-stu-id="dade5-145">Start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="dade5-146">Öffnen Sie im Projektmappen-Explorer die **"Site.CSS" ändern** -Datei unter der **Stile** Ordner.</span><span class="sxs-lookup"><span data-stu-id="dade5-146">In Solution Explorer, open the **Site.css** file located under the **Styles** folder.</span></span> <span data-ttu-id="dade5-147">Stellen Sie sicher, dass die **Texteditor** Tools sind auf der Symbolleiste angezeigt.</span><span class="sxs-lookup"><span data-stu-id="dade5-147">Make sure the **Text Editor** tools are visible on the toolbar.</span></span> <span data-ttu-id="dade5-148">Wählen Sie hierzu die **Ansicht** | **Symbolleisten** Menüoption, und aktivieren Sie die **Text-Editor** Optionen.</span><span class="sxs-lookup"><span data-stu-id="dade5-148">To do that, select the **View** | **Toolbars** menu option, and check the **Text Editor** options.</span></span> <span data-ttu-id="dade5-149">Sie bemerken, dass, seit diese neue Version der **Kommentar** Schaltfläche (![Kommentar Schaltfläche](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png) ) und die **entfernen Sie den Kommentar** Schaltfläche (![kommentieren-Schaltfläche](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png)) für den CSS-Editor auch aktiviert sind.</span><span class="sxs-lookup"><span data-stu-id="dade5-149">You will notice that, since this new version, the **Comment** button (![comment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png) ) and the **Uncomment** button (![uncomment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png) ) are also enabled for the CSS editor.</span></span>

    <span data-ttu-id="dade5-150">![Aktivieren Editor und CSS-Tools](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "-Editor und CSS-Tools aktivieren")</span><span class="sxs-lookup"><span data-stu-id="dade5-150">![Enabling Editor and CSS Tools](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "Enabling Editor and CSS Tools")</span></span>

    <span data-ttu-id="dade5-151">*Aktivieren Editor und CSS-Tools*</span><span class="sxs-lookup"><span data-stu-id="dade5-151">*Enabling Editor and CSS Tools*</span></span>
3. <span data-ttu-id="dade5-152">Führen Sie einen Bildlauf im Code, und wählen Sie eine beliebige CSS-Klassendefinition.</span><span class="sxs-lookup"><span data-stu-id="dade5-152">Scroll the code and select any CSS class definition.</span></span> <span data-ttu-id="dade5-153">Klicken Sie auf die **Kommentar** (![Kommentar Schaltfläche](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png) ) Schaltfläche, um die ausgewählten Zeilen zu kommentieren.</span><span class="sxs-lookup"><span data-stu-id="dade5-153">Click the **Comment** (![comment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png) ) button to comment the selected lines.</span></span> <span data-ttu-id="dade5-154">Klicken Sie auf die **entfernen Sie den Kommentar** (![kommentieren Schaltfläche](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png) ) Schaltfläche, um die Änderungen rückgängig zu machen.</span><span class="sxs-lookup"><span data-stu-id="dade5-154">Then, click the **Uncomment** (![uncomment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png) ) button to undo the changes.</span></span>
4. <span data-ttu-id="dade5-155">Klicken Sie auf die **reduzieren** (![reduzieren](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png) ) und **erweitern** (![erweitern](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png) ) Schaltflächen befinden sich am linken Rand des Texts.</span><span class="sxs-lookup"><span data-stu-id="dade5-155">Click the **Collapse** (![collapse](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png) ) and **Expand** (![expand](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png) ) buttons located on the left margin of the text.</span></span> <span data-ttu-id="dade5-156">Beachten Sie, dass Sie jetzt die Stile, die Sie verwenden ausblenden können, um eine übersichtlichere Ansicht haben.</span><span class="sxs-lookup"><span data-stu-id="dade5-156">Notice that you can now hide the styles you don't use to have a cleaner view.</span></span>

    <span data-ttu-id="dade5-157">![Reduzieren die CSS-Klassen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "Reduzieren von CSS-Klassen")</span><span class="sxs-lookup"><span data-stu-id="dade5-157">![Collapsing CSS classes](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "Collapsing CSS classes")</span></span>

    <span data-ttu-id="dade5-158">*Reduzieren die CSS-Klassen*</span><span class="sxs-lookup"><span data-stu-id="dade5-158">*Collapsing CSS classes*</span></span>
5. <span data-ttu-id="dade5-159">Stellen Sie sicher, dass die intelligenten Einzug-Funktion aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="dade5-159">Make sure that the smart indentation feature is enabled.</span></span> <span data-ttu-id="dade5-160">Wählen Sie die **Tools** | **Optionen** Menüoption, und wählen Sie dann die **Texteditor** | **CSS**  |  **Formatierung** Seite im linken Bereich des Bildschirms.</span><span class="sxs-lookup"><span data-stu-id="dade5-160">Select the **Tools** | **Options** menu option, and then select the **Text Editor** | **CSS** | **Formatting** page in the left pane of the screen.</span></span> <span data-ttu-id="dade5-161">Überprüfen Sie die **Hierarchischer Einzug** Option.</span><span class="sxs-lookup"><span data-stu-id="dade5-161">Check the **Hierarchical indentation** option.</span></span>

    <span data-ttu-id="dade5-162">![Aktivieren der hierarchischen Einzug](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "aktivieren hierarchischen Einzug")</span><span class="sxs-lookup"><span data-stu-id="dade5-162">![Enabling hierarchical indentation](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "Enabling hierarchical indentation")</span></span>

    <span data-ttu-id="dade5-163">*Aktivieren der hierarchischen Einzug*</span><span class="sxs-lookup"><span data-stu-id="dade5-163">*Enabling hierarchical indentation*</span></span>
6. <span data-ttu-id="dade5-164">Suchen der wichtigsten Klassendefinition (.main), und fügen Sie eine Formatvorlage auf das Div-Elemente.</span><span class="sxs-lookup"><span data-stu-id="dade5-164">Locate the main class definition (.main) and append a style to the div elements.</span></span> <span data-ttu-id="dade5-165">Beachten Sie, dass der Code automatisch, Unterstützung der Benutzer der übergeordneten Klassen auf einen Blick finden ausgerichtet.</span><span class="sxs-lookup"><span data-stu-id="dade5-165">You will notice that the code aligns automatically, helping users to find the parent classes at a glance.</span></span>

    <span data-ttu-id="dade5-166">CSS</span><span class="sxs-lookup"><span data-stu-id="dade5-166">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample1.css)]

    <span data-ttu-id="dade5-167">![Hierarchische Ausrichtung in CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "hierarchische Ausrichtung in CSS")</span><span class="sxs-lookup"><span data-stu-id="dade5-167">![Hierarchical alignment in CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "Hierarchical alignment in CSS")</span></span>

    <span data-ttu-id="dade5-168">*Hierarchische Ausrichtung in CSS*</span><span class="sxs-lookup"><span data-stu-id="dade5-168">*Hierarchical alignment in CSS*</span></span>
7. <span data-ttu-id="dade5-169">In **.main Div** Klasse, suchen Sie den Cursor am Ende der **Rahmen: 0px;** , und drücken Sie **EINGABETASTE** die IntelliSense-Liste angezeigt.</span><span class="sxs-lookup"><span data-stu-id="dade5-169">Inside **.main div** class, locate the cursor at the end of **border: 0px;** and press **Enter** to display the IntelliSense list.</span></span> <span data-ttu-id="dade5-170">Mit der Eingabe beginnen **oben** , und beachten Sie, wie die Liste gefiltert wird, während der Eingabe.</span><span class="sxs-lookup"><span data-stu-id="dade5-170">Start typing **top** and notice how the list is filtered as you type.</span></span> <span data-ttu-id="dade5-171">Die Liste zeigt die Elemente, die enthalten **oben** an einen beliebigen Teil des Worts (In früheren Versionen von Visual Studio wird die Liste gefiltert, die von Elementen, die *beginnen* zusammen mit dem Begriff).</span><span class="sxs-lookup"><span data-stu-id="dade5-171">The list will display the elements that contain **top** at any part of the word (In prior versions of Visual Studio, the list is filtered by the items that *begin* with the term).</span></span>

    <span data-ttu-id="dade5-172">![IntelliSense-Erweiterungen in CSS-](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "IntelliSense-Erweiterungen in CSS")</span><span class="sxs-lookup"><span data-stu-id="dade5-172">![IntelliSense enhancements in CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "IntelliSense enhancements in CSS")</span></span>

    <span data-ttu-id="dade5-173">*IntelliSense-Erweiterungen in CSS*</span><span class="sxs-lookup"><span data-stu-id="dade5-173">*IntelliSense enhancements in CSS*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_The_Color_Picker"></a>
#### <a name="task-2---the-color-picker"></a><span data-ttu-id="dade5-174">Aufgabe 2: die Farbauswahl</span><span class="sxs-lookup"><span data-stu-id="dade5-174">Task 2 - The Color Picker</span></span>

<span data-ttu-id="dade5-175">In dieser Aufgabe werden Sie feststellen, dass der neuen CSS-Farbauswahl in Visual Studio IntelliSense integriert.</span><span class="sxs-lookup"><span data-stu-id="dade5-175">In this task, you will discover the new CSS Color Picker integrated into Visual Studio IntelliSense.</span></span>

1. <span data-ttu-id="dade5-176">In **"Site.CSS" ändern,** suchen Sie die Header-Klassendefinition (.header), und platzieren Sie den Cursor neben **Hintergrundfarbe** Attribut, zwischen den &quot;:&quot; und &quot; # &quot; Zeichen in dieser Zeile des Codes **.**</span><span class="sxs-lookup"><span data-stu-id="dade5-176">In **Site.css,** locate the header class definition (.header) and place the cursor next to **background-color** attribute, between the &quot;:&quot; and &quot;#&quot; characters on that line of code **.**</span></span>

    <span data-ttu-id="dade5-177">![Suchen den Cursor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "suchen den Cursor")</span><span class="sxs-lookup"><span data-stu-id="dade5-177">![Locating the cursor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "Locating the cursor")</span></span>

    <span data-ttu-id="dade5-178">*Suchen den cursor*</span><span class="sxs-lookup"><span data-stu-id="dade5-178">*Locating the cursor*</span></span>
2. <span data-ttu-id="dade5-179">Löschen der **Doppelpunkt** (:) und Schreiben Sie ihn erneut aus, um die Farbauswahl angezeigt.</span><span class="sxs-lookup"><span data-stu-id="dade5-179">Delete the **colon** (:) and write it again to display the color picker.</span></span> <span data-ttu-id="dade5-180">Beachten Sie, dass die erste Farben, die Sie sehen die am häufigsten auftretenden Farben Ihres Standorts sind.</span><span class="sxs-lookup"><span data-stu-id="dade5-180">Notice that the first colors you will see are the most frequent colors of your site.</span></span> <span data-ttu-id="dade5-181">Wenn Sie die Farbe weiße klicken, wird die HTML-Farbcode (#fff) den aktuellen-Farbcode im Stylesheet ersetzen.</span><span class="sxs-lookup"><span data-stu-id="dade5-181">If you click the white color, its HTML color code (#fff) will replace the current color code in the stylesheet.</span></span>

    <span data-ttu-id="dade5-182">![Farbauswahl](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "Farbauswahl")</span><span class="sxs-lookup"><span data-stu-id="dade5-182">![Color picker](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "Color picker")</span></span>

    <span data-ttu-id="dade5-183">*Farbauswahl*</span><span class="sxs-lookup"><span data-stu-id="dade5-183">*Color picker*</span></span>
3. <span data-ttu-id="dade5-184">Drücken Sie die **erweitern** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) auf die Farbauswahl, um den Farbverlauf anzuzeigen, und ziehen dann den Farbverlauf Cursor, um eine andere Farbe auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="dade5-184">Press the **Expand** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) button on the color picker to display the color gradient, and then drag the gradient cursor to select a different color.</span></span> <span data-ttu-id="dade5-185">Klicken Sie danach auf die **Formatpipette** Schaltfläche und wählen Sie eine Farbe aus dem Bildschirm.</span><span class="sxs-lookup"><span data-stu-id="dade5-185">After that, click the **Eyedropper** button and select any color from the screen.</span></span> <span data-ttu-id="dade5-186">Beachten Sie, dass der Wert für Hintergrundfarbe dynamisch ändert, während Sie den Cursor bewegen.</span><span class="sxs-lookup"><span data-stu-id="dade5-186">Notice that background color value changes dynamically while you move the cursor.</span></span>

    <span data-ttu-id="dade5-187">![Auswahl einer Farbverlauf](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "Datumsauswahl Farbverlauf")</span><span class="sxs-lookup"><span data-stu-id="dade5-187">![Color picker gradient](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "Color picker gradient")</span></span>

    <span data-ttu-id="dade5-188">*Auswahl einer Farbverlauf*</span><span class="sxs-lookup"><span data-stu-id="dade5-188">*Color picker gradient*</span></span>
4. <span data-ttu-id="dade5-189">In der **Deckkraft** Schieberegler, verschieben Sie die Auswahl in die Mitte des Balkens die Deckkraft zu reduzieren.</span><span class="sxs-lookup"><span data-stu-id="dade5-189">In the **Opacity** slider, move the selector to the center of the bar to reduce the opacity.</span></span> <span data-ttu-id="dade5-190">Beachten Sie, dass die Hintergrundfarbe der Wert jetzt ihrer Skala RGBA ändert.</span><span class="sxs-lookup"><span data-stu-id="dade5-190">Notice that background-color value now changes its scale to RGBA.</span></span>

    <span data-ttu-id="dade5-191">![Farbauswahl Deckkraft](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "Farbauswahl Deckkraft")</span><span class="sxs-lookup"><span data-stu-id="dade5-191">![Color picker Opacity](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "Color picker Opacity")</span></span>

    <span data-ttu-id="dade5-192">*Farbauswahl Deckkraft*</span><span class="sxs-lookup"><span data-stu-id="dade5-192">*Color picker Opacity*</span></span>

    > [!NOTE]
    > <span data-ttu-id="dade5-193">Die Definition des RGBA (Rot, Grün, Blau, Alpha)-Farbe in CSS3 können Sie die Deckkraft Farbwert für ein einzelnes Element definieren.</span><span class="sxs-lookup"><span data-stu-id="dade5-193">The RGBA (Red, Green, Blue, Alpha) color definition in CSS3 enables you to define the color opacity value for a single item.</span></span> <span data-ttu-id="dade5-194">Im Gegensatz zu **Deckkraft -** ein ähnliches Attribut der CSS- **-** RGBA Farben werden auch mit neueren Browsern kompatibel.</span><span class="sxs-lookup"><span data-stu-id="dade5-194">Unlike **opacity -** a similar CSS attribute **-** RGBA colors are also compatible with the latest browsers.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_CSS_Compatible_Code_Snippets"></a>
#### <a name="task-3---css-compatible-code-snippets"></a><span data-ttu-id="dade5-195">Aufgabe 3 - kompatible CSS-Codeausschnitte</span><span class="sxs-lookup"><span data-stu-id="dade5-195">Task 3 - CSS Compatible Code Snippets</span></span>

<span data-ttu-id="dade5-196">In dieser Aufgabe lernen Sie das browserübergreifende kompatibel CSS3 Ausschnitte zu verwenden, um einige Features in Ihrer Website zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="dade5-196">In this task, you will learn how to use cross-browser compatible CSS3 snippets in order to implement some features in your website.</span></span>

1. <span data-ttu-id="dade5-197">In der **"Site.CSS" ändern** Datei, suchen Sie nach der **Header** CSS Klassendefinition (.header), und platzieren Sie den Cursor unterhalb der **/ \*Border-Radius\* /** Platzhalter sind, einen neuen Ausschnitt hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="dade5-197">In the **Site.css** file, locate the **header** CSS class definition (.header) and place the cursor below the **/\*border radius\*/** placeholder to add a new snippet.</span></span> <span data-ttu-id="dade5-198">Drücken Sie **EINGABETASTE** zum Anzeigen der IntelliSense-Liste und Typ **Radius** zum Filtern der Liste.</span><span class="sxs-lookup"><span data-stu-id="dade5-198">Press **Enter** to display the IntelliSense list and type **radius** to filter the list.</span></span> <span data-ttu-id="dade5-199">Wählen Sie die **Border-Radius** aus der Liste mit einem doppelten Mausklick aus, und drücken Sie dann die **Registerkarte** Schlüssel zum Einfügen des Codeausschnitts.</span><span class="sxs-lookup"><span data-stu-id="dade5-199">Select the **border-radius** option from the list with a double click, and then press the **TAB** key to insert the snippet.</span></span> <span data-ttu-id="dade5-200">Geben Sie eine Radiusgröße in Pixel, und drücken Sie **EINGABETASTE**.</span><span class="sxs-lookup"><span data-stu-id="dade5-200">Then, type a radius size in pixels and press **Enter**.</span></span> <span data-ttu-id="dade5-201">Geben Sie z. B. **15px**.</span><span class="sxs-lookup"><span data-stu-id="dade5-201">For instance, type **15px**.</span></span>

    <span data-ttu-id="dade5-202">Die CSS3-Attribute hinzugefügt werden, indem Sie den Ausschnitt rendert abgerundeten Ränder in den meisten HTML5 Kompatibilität Browsern, einschließlich Mozilla und Browser WebKit-basiert.</span><span class="sxs-lookup"><span data-stu-id="dade5-202">The CSS3 attributes added by the snippet will render rounded borders in most HTML5 compliance browsers, including Mozilla and WebKit-based browsers.</span></span>

    <span data-ttu-id="dade5-203">![Mit einem Border-Radius-Ausschnitt](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "mit einem Border-Radius-Ausschnitt")</span><span class="sxs-lookup"><span data-stu-id="dade5-203">![Using a border-radius snippet](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "Using a border-radius snippet")</span></span>

    <span data-ttu-id="dade5-204">*Verwenden einen Border-Radius-Ausschnitt*</span><span class="sxs-lookup"><span data-stu-id="dade5-204">*Using a border-radius snippet*</span></span>
2. <span data-ttu-id="dade5-205">Übernehmen Sie die gleiche **Rahmen** Codeausschnitte in der Seite "-Stil (.page).</span><span class="sxs-lookup"><span data-stu-id="dade5-205">Apply the same **border** snippets in the page style (.page).</span></span>

    <span data-ttu-id="dade5-206">CSS</span><span class="sxs-lookup"><span data-stu-id="dade5-206">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample2.css)]
3. <span data-ttu-id="dade5-207">Drücken Sie **F5** um die Projektmappe auszuführen.</span><span class="sxs-lookup"><span data-stu-id="dade5-207">Press **F5** to run the solution.</span></span> <span data-ttu-id="dade5-208">Beachten Sie, dass jeder Seite jetzt Rahmen gerundet wurde.</span><span class="sxs-lookup"><span data-stu-id="dade5-208">Notice that each page now has rounded borders.</span></span>

    <span data-ttu-id="dade5-209">![Abgerundete Ecken](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "abgerundete Ecken")</span><span class="sxs-lookup"><span data-stu-id="dade5-209">![Rounded corners](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "Rounded corners")</span></span>

    <span data-ttu-id="dade5-210">*Abgerundeten Ecken*</span><span class="sxs-lookup"><span data-stu-id="dade5-210">*Rounded corners*</span></span>
4. <span data-ttu-id="dade5-211">Schließen Sie den Browser, und kehren Sie zu Visual Studio zurück.</span><span class="sxs-lookup"><span data-stu-id="dade5-211">Close the browser and return to Visual Studio.</span></span>
5. <span data-ttu-id="dade5-212">Öffnen der **Custom.css** -Datei unter der **Stile** Ordner, und platzieren Sie den Cursor innerhalb **div.images Ul li Img** Klassendefinition.</span><span class="sxs-lookup"><span data-stu-id="dade5-212">Open the **Custom.css** file located under the **Styles** folder and place the cursor inside **div.images ul li img** class definition.</span></span>
6. <span data-ttu-id="dade5-213">EINGABETASTE drücken, um die IntelliSense-Liste anzuzeigen Typ **' Box-Shadow** , und drücken Sie die **Registerkarte** Taste zweimal, um den Standard-Volumeschattenkopie-Codeausschnitt innerhalb der Klassendefinition einfügen.</span><span class="sxs-lookup"><span data-stu-id="dade5-213">Press enter to display the IntelliSense list, type **box-shadow** and press the **TAB** key twice to insert the default shadow code snippet inside the class definition.</span></span> <span data-ttu-id="dade5-214">Legen Sie die Schattenwerte auf **10px 10px 5px #888**.</span><span class="sxs-lookup"><span data-stu-id="dade5-214">Set the shadow values to **10px 10px 5px #888**.</span></span> <span data-ttu-id="dade5-215">Geben Sie dann **Border-Radius** , und fügen Sie den Codeausschnitt.</span><span class="sxs-lookup"><span data-stu-id="dade5-215">Then, type **border-radius** and insert the code snippet.</span></span> <span data-ttu-id="dade5-216">Typ **15px** festzulegende Radiusgröße, und drücken Sie **EINGABETASTE**.</span><span class="sxs-lookup"><span data-stu-id="dade5-216">Type **15px** to set radius size and press **ENTER**.</span></span>

    <span data-ttu-id="dade5-217">![Abgerundete Ecken mit Schatten](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "abgerundete Ecken mit Schatten")</span><span class="sxs-lookup"><span data-stu-id="dade5-217">![Rounded corners with shadow](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "Rounded corners with shadow")</span></span>

    <span data-ttu-id="dade5-218">*Abgerundeten Ecken mit Schatten*</span><span class="sxs-lookup"><span data-stu-id="dade5-218">*Rounded corners with shadow*</span></span>

    > [!NOTE]
    > <span data-ttu-id="dade5-219">Zu diesem Zeitpunkt wird der Volumeschattenkopie-Attribut mit dem entsprechenden Präfix (Moz, Webkit, o) zur Unterstützung von Mozilla und Browser Webkit (Chrom, Safari, Konkeror) eingefügt.</span><span class="sxs-lookup"><span data-stu-id="dade5-219">At this moment, the shadow attribute is inserted with the corresponding prefix (moz, webkit, o) to support Mozilla and Webkit (Chrome, Safari, Konkeror) browsers.</span></span>
7. <span data-ttu-id="dade5-220">Erstellen Sie eine neue Klasse **div.images Ul li Img:hover** unterhalb der **div.images Ul li Img** Klassendefinition, und platzieren Sie den Cursor innerhalb der Klammern **.**</span><span class="sxs-lookup"><span data-stu-id="dade5-220">Create a new class **div.images ul li img:hover** below the **div.images ul li img** class definition and place the cursor inside the brackets **.**</span></span>

    <span data-ttu-id="dade5-221">CSS</span><span class="sxs-lookup"><span data-stu-id="dade5-221">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample3.css)]
8. <span data-ttu-id="dade5-222">Typ **transformieren** , und drücken Sie die **Registerkarte** Taste zweimal, um die Transformation-Ausschnitt eingefügt wird.</span><span class="sxs-lookup"><span data-stu-id="dade5-222">Type **transform** and press the **TAB** key twice in order to insert the transform snippet.</span></span> <span data-ttu-id="dade5-223">Geben Sie dann die **rotate(-15deg)** den Drehwinkel ändern, wenn Bilder gezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="dade5-223">Then, enter **rotate(-15deg)** to change the rotation angle value when images are hovered.</span></span>

    <span data-ttu-id="dade5-224">CSS</span><span class="sxs-lookup"><span data-stu-id="dade5-224">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample4.css)]
9. <span data-ttu-id="dade5-225">Drücken Sie **F5** führen Sie die Projektmappe, und navigieren Sie zu der Seite "CSS3".</span><span class="sxs-lookup"><span data-stu-id="dade5-225">Press **F5** to run the solution and browse to the CSS3 page.</span></span> <span data-ttu-id="dade5-226">Beachten Sie, dass die Bilder abgerundete Ecken haben und Schatten Feld.</span><span class="sxs-lookup"><span data-stu-id="dade5-226">Notice that the images have rounded corners and box shadows.</span></span> <span data-ttu-id="dade5-227">Zeigen Sie mit der Maus über die Bilder, und beobachten sie drehen.</span><span class="sxs-lookup"><span data-stu-id="dade5-227">Hover the mouse over the images and watch them rotate.</span></span>

    <span data-ttu-id="dade5-228">![Drehen eines Bilds Ausschnitt transformieren](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "Transformation Ausschnitt Drehen eines Bildes")</span><span class="sxs-lookup"><span data-stu-id="dade5-228">![Transform snippet rotating an image](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "Transform snippet rotating an image")</span></span>

    <span data-ttu-id="dade5-229">*Transformieren der Ausschnitt Drehen eines Bildes*</span><span class="sxs-lookup"><span data-stu-id="dade5-229">*Transform snippet rotating an image*</span></span>

    > [!NOTE]
    > <span data-ttu-id="dade5-230">Wenn Sie Internet Explorer 10 verwenden und nicht die Schatten sehen, stellen Sie sicher, dass der Dokumentmodus IE10-Standards festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="dade5-230">If you are using Internet Explorer 10 and cannot see the shadows, make sure the document mode is set to IE10 standards.</span></span> <span data-ttu-id="dade5-231">Drücken Sie **F12** , öffnen Sie Internet Explorer-Entwicklertools, und klicken Sie auf **Dokumentmodus** IE10-Standards zu ändern.</span><span class="sxs-lookup"><span data-stu-id="dade5-231">Press **F12** to open Internet Explorer developer tools and click **Document Mode** to change to IE10 standards.</span></span>

    ![Informationen zu – de](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image21.png)

<a id="Exercise2"></a>

<a id="Exercise_2_Whats_New_in_the_HTML_Editor"></a>
### <a name="exercise-2-whats-new-in-the-html-editor"></a><span data-ttu-id="dade5-233">Übung 2: Was ist neu im HTML-Editor</span><span class="sxs-lookup"><span data-stu-id="dade5-233">Exercise 2: What's New in the HTML Editor</span></span>

<span data-ttu-id="dade5-234">Visual Studio verfügt über eine verbesserte HTML-Editor.</span><span class="sxs-lookup"><span data-stu-id="dade5-234">Visual Studio has an improved HTML editor.</span></span> <span data-ttu-id="dade5-235">Zu den Verbesserungen in dieser Version enthalten sind intelligenten Einzug in HTML-Dokumente, HTML5-Ausschnitte, HTML-Start und Ende Tag übereinstimmen und HTML-Validierung.</span><span class="sxs-lookup"><span data-stu-id="dade5-235">Some of the enhancements included in this version are smart indentation in HTML documents, HTML5 snippets, HTML start and end tag matching, and HTML validation.</span></span> <span data-ttu-id="dade5-236">Während dieser Übung sehen Sie, wie diese Änderungen bei der Arbeit in das Markup für die Website, die flüssige verbessern.</span><span class="sxs-lookup"><span data-stu-id="dade5-236">Throughout this exercise, you will see how these changes improve your fluency when working in the website markup.</span></span>

<span data-ttu-id="dade5-237">Der HTML-Editor wurde auch verbessert, wie CSS-Editor.</span><span class="sxs-lookup"><span data-stu-id="dade5-237">Like the CSS editor, the HTML editor was also improved.</span></span> <span data-ttu-id="dade5-238">Die meisten dieser Verbesserungen sind kleine Server, auf denen die Web Developer Leben erleichtern.</span><span class="sxs-lookup"><span data-stu-id="dade5-238">Most of these improvements are small ones that make the Web developer's life easier.</span></span> <span data-ttu-id="dade5-239">Elemente wie weitere Codeausschnitte für HTML5, intelligenten Einzug übereinstimmende Start- und Endtags, beim Bearbeiten und Validierung, HTML-Dokument DOCTYPE Zielgruppenadressierung einige diese Verbesserungen sind.</span><span class="sxs-lookup"><span data-stu-id="dade5-239">Things like more code snippets for HTML5, smart indentation, matching start and end tags when editing and validation targeting the HTML document DOCTYPE are some of these improvements.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Improved_DOCTYPE_Validation"></a>
#### <a name="task-1---improved-doctype-validation"></a><span data-ttu-id="dade5-240">Aufgabe 1: verbesserte DOCTYPE-Überprüfung</span><span class="sxs-lookup"><span data-stu-id="dade5-240">Task 1 - Improved DOCTYPE Validation</span></span>

<span data-ttu-id="dade5-241">Der HTML-Editor hat jetzt die Möglichkeit zum Überprüfen der Rand der Seite "DOCTYPE", obwohl die Definition der Masterseite sein kann.</span><span class="sxs-lookup"><span data-stu-id="dade5-241">The HTML editor now has the ability to check the DOCTYPE of your page, even though the definition might be in the master page.</span></span> <span data-ttu-id="dade5-242">Abhängig von der "DOCTYPE" Rand der Seite HTML-Editor mit der richtige Satz von Regeln überprüft und filtert die IntelliSense-Liste, die Berücksichtigung der DOCTYPE-Elemente.</span><span class="sxs-lookup"><span data-stu-id="dade5-242">Depending on the DOCTYPE of your page, the HTML editor will validate with the correct set of rules and will filter the IntelliSense list considering the DOCTYPE elements.</span></span>

<span data-ttu-id="dade5-243">In dieser Aufgabe ändern Sie die DOCTYPE einer Seite, um festzustellen, wie sich das Verhalten des HTML-Editor entsprechend ändert.</span><span class="sxs-lookup"><span data-stu-id="dade5-243">In this task, you will change the DOCTYPE of a page to see how the HTML editor behavior changes accordingly.</span></span>

1. <span data-ttu-id="dade5-244">Wenn nicht bereits geöffnet ist, starten Sie **Visual Studio** , und öffnen Sie die **WhatsNewASPNET.sln** Projektmappe befindet sich in der **Source\WhatsNewASPNET** Ordner dieser Anleitung.</span><span class="sxs-lookup"><span data-stu-id="dade5-244">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="dade5-245">Öffnen der **Site.Master** Seite.</span><span class="sxs-lookup"><span data-stu-id="dade5-245">Open the **Site.Master** page.</span></span>
3. <span data-ttu-id="dade5-246">Beachten Sie das Zielschema für Validierung Symbolleiste aus.</span><span class="sxs-lookup"><span data-stu-id="dade5-246">Notice the Target Schema for Validation Toolbar.</span></span> <span data-ttu-id="dade5-247">Die Möglichkeit, die HTML-Editor verhält sich (Validierung, IntelliSense, usw.) ändert sich ordnungsgemäß entsprechend die "Doctype" ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="dade5-247">The way the HTML editor behaves (Validation, IntelliSense, etc.) will properly change to fit the Doctype selected.</span></span>

    <span data-ttu-id="dade5-248">![Doctype HTML-Quellcodebearbeitung Symbolleiste verwenden](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "verwenden "Doctype" in HTML-Quellcodebearbeitung-Symbolleiste")</span><span class="sxs-lookup"><span data-stu-id="dade5-248">![Use Doctype in HTML Source Editing toolbar](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "Use Doctype in HTML Source Editing toolbar")</span></span>

    <span data-ttu-id="dade5-249">*Doctype HTML-Quellcodebearbeitung Symbolleiste verwenden*</span><span class="sxs-lookup"><span data-stu-id="dade5-249">*Use Doctype in HTML Source Editing toolbar*</span></span>
4. <span data-ttu-id="dade5-250">Ändern Sie das Zielschema in HTML 4.01.</span><span class="sxs-lookup"><span data-stu-id="dade5-250">Change the Target Schema to HTML 4.01.</span></span>

    <span data-ttu-id="dade5-251">![Ändern der Symbolleiste des HTML-Quellcodebearbeitung Doctype](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "ändern Doctype HTML-Quellcodebearbeitung-Symbolleiste")</span><span class="sxs-lookup"><span data-stu-id="dade5-251">![Changing Doctype in HTML Source Editing toolbar](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "Changing Doctype in HTML Source Editing toolbar")</span></span>

    <span data-ttu-id="dade5-252">*Doctype HTML-Quellcodebearbeitung Symbolleiste ändern*</span><span class="sxs-lookup"><span data-stu-id="dade5-252">*Changing Doctype in HTML Source Editing toolbar*</span></span>
5. <span data-ttu-id="dade5-253">Platzieren Sie den Cursor unter den **Text** -Element, und beginnen Sie mit den Namen eines HTML5-Elements (z. B. **video**).</span><span class="sxs-lookup"><span data-stu-id="dade5-253">Place the cursor under the **body** element, and start typing the name of an HTML5 element (for example, **video**).</span></span> <span data-ttu-id="dade5-254">Beachten Sie, dass das Element nicht in der IntelliSense-Liste verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="dade5-254">Notice that the element is not available in the IntelliSense list.</span></span>

    <span data-ttu-id="dade5-255">![HTML5-Elemente, die nicht aufgelisteten](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "HTML5-Elemente, die nicht aufgeführt.")</span><span class="sxs-lookup"><span data-stu-id="dade5-255">![HTML5 elements not listed](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "HTML5 elements not listed")</span></span>

    <span data-ttu-id="dade5-256">*HTML5-Elemente, die nicht aufgeführt.*</span><span class="sxs-lookup"><span data-stu-id="dade5-256">*HTML5 elements not listed*</span></span>
6. <span data-ttu-id="dade5-257">Die Änderungen in das Zielschema für Validierung Symbolleiste DOCTYPE Entnahme rückgängig machen: XHTML5 aus der Dropdownliste aus.</span><span class="sxs-lookup"><span data-stu-id="dade5-257">Undo the changes to the Target Schema for Validation Toolbar, picking DOCTYPE: XHTML5 from the dropdown list.</span></span>

    <span data-ttu-id="dade5-258">![Doctype HTML-Quellcodebearbeitung Symbolleiste verwenden](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "verwenden "Doctype" in HTML-Quellcodebearbeitung-Symbolleiste")</span><span class="sxs-lookup"><span data-stu-id="dade5-258">![Use Doctype in HTML Source Editing toolbar](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "Use Doctype in HTML Source Editing toolbar")</span></span>

    <span data-ttu-id="dade5-259">*Zurücksetzen der "Doctype" in HTML-Quellcodebearbeitung-Symbolleiste*</span><span class="sxs-lookup"><span data-stu-id="dade5-259">*Reset Doctype in HTML Source Editing toolbar*</span></span>
7. <span data-ttu-id="dade5-260">Platzieren Sie den Cursor unter den **Text** Element- und anfangen zu tippen erneut eine HTML5-Element (z. B. wie **video**).</span><span class="sxs-lookup"><span data-stu-id="dade5-260">Place the cursor under the **body** element and start typing an HTML5 element again (For example, like **video**).</span></span> <span data-ttu-id="dade5-261">Beachten Sie, dass jetzt die HTML5-Elemente in der IntelliSense-Liste verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="dade5-261">Notice that the HTML5 elements are now available in the IntelliSense list.</span></span>

    <span data-ttu-id="dade5-262">![HTML5-Elemente aufgelistet wird](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "HTML5-Elemente, die aufgelistet werden")</span><span class="sxs-lookup"><span data-stu-id="dade5-262">![HTML5 elements being listed](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "HTML5 elements being listed")</span></span>

    <span data-ttu-id="dade5-263">*HTML5-Elemente, die aufgelistet werden*</span><span class="sxs-lookup"><span data-stu-id="dade5-263">*HTML5 elements being listed*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_StartEnd_Tags_Automatic_Update"></a>
#### <a name="task-2---startend-tags-automatic-update"></a><span data-ttu-id="dade5-264">Aufgabe 2: Start/End-Tags, automatische Updates</span><span class="sxs-lookup"><span data-stu-id="dade5-264">Task 2 - Start/End Tags Automatic Update</span></span>

<span data-ttu-id="dade5-265">Visual Studio aktualisiert den HTML-Code beim Öffnen oder Endtags des Elements, das Sie bearbeiten, um als übereinstimmend.</span><span class="sxs-lookup"><span data-stu-id="dade5-265">Visual Studio now updates the HTML opening or closing tags of the element that you are editing to match each other.</span></span> <span data-ttu-id="dade5-266">Dieses neue Feature verbessert die Produktivität, beim Bearbeiten von HTML-Tags.</span><span class="sxs-lookup"><span data-stu-id="dade5-266">This new feature will improve your productivity when editing HTML tags.</span></span>

1. <span data-ttu-id="dade5-267">Auf der **"default.aspx"** Seite, fügen Sie ein **H3** Element mit einem Titel (z. B. Visual Studio 2012 Rocks!).</span><span class="sxs-lookup"><span data-stu-id="dade5-267">On the **Default.aspx** page, add an **H3** element with a title (for example, Visual Studio 2012 Rocks!).</span></span>


~~~
[!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample5.aspx)]
~~~
2. <span data-ttu-id="dade5-268">Ändern der **H3** Tag, und geben **H2** oder **H1.**</span><span class="sxs-lookup"><span data-stu-id="dade5-268">Change the **H3** tag and type **H2** or **H1.**</span></span>

    <span data-ttu-id="dade5-269">Beachten Sie, dass das Endtag wird automatisch aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="dade5-269">Notice that the end tag automatically updates.</span></span> <span data-ttu-id="dade5-270">Sie können auch ändern, dass das Endtag, um festzustellen, ob das Starttag zu entsprechend aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="dade5-270">You can also modify the end tag to see that the start tag updates accordingly too.</span></span>

    <span data-ttu-id="dade5-271">![Automatische Aktualisierung des Endtags](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "automatische Aktualisierung des Endtags")</span><span class="sxs-lookup"><span data-stu-id="dade5-271">![Automatic update of the end tag](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "Automatic update of the end tag")</span></span>

    <span data-ttu-id="dade5-272">*Automatische Aktualisierung des Endtags*</span><span class="sxs-lookup"><span data-stu-id="dade5-272">*Automatic update of the end tag*</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_New_HTML5_Code_Snippets"></a>
#### <a name="task-3---new-html5-code-snippets"></a><span data-ttu-id="dade5-273">Aufgabe 3 – neue HTML5-Codeausschnitte</span><span class="sxs-lookup"><span data-stu-id="dade5-273">Task 3 - New HTML5 Code Snippets</span></span>

<span data-ttu-id="dade5-274">Visual Studio enthält nun einige HTML5-Codeausschnitte.</span><span class="sxs-lookup"><span data-stu-id="dade5-274">Visual Studio now includes several HTML5 code snippets.</span></span> <span data-ttu-id="dade5-275">In dieser Aufgabe verwenden Sie einige der diese Codeausschnitte.</span><span class="sxs-lookup"><span data-stu-id="dade5-275">In this task, you will use some of these snippets.</span></span>

1. <span data-ttu-id="dade5-276">Fügen Sie einen neuen Ordner namens **audio** auf das Stammverzeichnis der Website-Ordner.</span><span class="sxs-lookup"><span data-stu-id="dade5-276">Add a new folder named **audio** to the root of the web site folder.</span></span> <span data-ttu-id="dade5-277">Öffnen Sie Windows Explorer, und kopieren Sie alle Audiodatei in der **audio** Ordner, der die **WhatsNewASPNET.sln** Lösung.</span><span class="sxs-lookup"><span data-stu-id="dade5-277">Open Windows Explorer and copy any audio file into the **audio** folder of the **WhatsNewASPNET.sln** solution.</span></span>
2. <span data-ttu-id="dade5-278">In der **"default.aspx"** Seite, und suchen Sie den Cursor unter der Web 11 Rocks!!</span><span class="sxs-lookup"><span data-stu-id="dade5-278">In the **Default.aspx** page, locate the cursor under the Web11 Rocks!!</span></span> <span data-ttu-id="dade5-279">Header.</span><span class="sxs-lookup"><span data-stu-id="dade5-279">Header.</span></span> <span data-ttu-id="dade5-280">Typ **audio** , und drücken Sie die TAB-Taste.</span><span class="sxs-lookup"><span data-stu-id="dade5-280">Type **audio** and press the TAB key.</span></span>

    <span data-ttu-id="dade5-281">Neue HTML-Editor enthält Codeausschnitte für HTML5-Inhalt.</span><span class="sxs-lookup"><span data-stu-id="dade5-281">The new HTML editor includes code snippets for HTML5 content.</span></span> <span data-ttu-id="dade5-282">Denken Sie daran, um die richtige DOCTYPE-Definition verwenden, um die HTML5-Ausschnitte zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="dade5-282">Remember to use the proper DOCTYPE definition to enable the HTML5 snippets.</span></span>

    <span data-ttu-id="dade5-283">![Einfügen von HTML5-Codeausschnitte](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "Einfügen von HTML5-Codeausschnitte")</span><span class="sxs-lookup"><span data-stu-id="dade5-283">![Inserting HTML5 Code Snippets](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "Inserting HTML5 Code Snippets")</span></span>

    <span data-ttu-id="dade5-284">*Einfügen von HTML5-Codeausschnitte*</span><span class="sxs-lookup"><span data-stu-id="dade5-284">*Inserting HTML5 Code Snippets*</span></span>
3. <span data-ttu-id="dade5-285">Aktualisieren Sie die audio Quelle auf eine vorhandene Audiodatei verweisen.</span><span class="sxs-lookup"><span data-stu-id="dade5-285">Update the audio source to point to an existing audio file.</span></span>


~~~
[!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample6.aspx)]

> [!NOTE]
> You will need to add the audio file to the solution.
~~~
4. <span data-ttu-id="dade5-286">Drücken Sie **F5** zum Ausführen von der Website und das Audio wiedergegeben.</span><span class="sxs-lookup"><span data-stu-id="dade5-286">Press **F5** to run the site and play the audio.</span></span>

    <span data-ttu-id="dade5-287">![Ausführen der Audiosteuerelement](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "mit audio-Steuerelement")</span><span class="sxs-lookup"><span data-stu-id="dade5-287">![Running the audio control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "Running the audio control")</span></span>

    <span data-ttu-id="dade5-288">*Ausführen von audio-Steuerelement*</span><span class="sxs-lookup"><span data-stu-id="dade5-288">*Running the audio control*</span></span>

    > [!NOTE]
    > <span data-ttu-id="dade5-289">Sie können auch versuchen, weitere Ausschnitte, die in Visual Studio, z. B. Video, Abbildung usw. enthalten.</span><span class="sxs-lookup"><span data-stu-id="dade5-289">You can also try more snippets included in Visual Studio, such as video, figure, etc.</span></span>
5. <span data-ttu-id="dade5-290">Nun versuchen Sie, ein Steuerelement in einem Teil der Seite eingefügt.</span><span class="sxs-lookup"><span data-stu-id="dade5-290">Now, try to insert a control in some part of the page.</span></span> <span data-ttu-id="dade5-291">Z. B. versuchen, fügen Sie eine **GridView** -Steuerelement, aber statt einzugeben  **&lt;Gri,** mit der Eingabe beginnen  **&lt;/GV**.</span><span class="sxs-lookup"><span data-stu-id="dade5-291">For example, try to insert a **GridView** control, but instead of typing **&lt;Gri,** start typing **&lt;GV**.</span></span> <span data-ttu-id="dade5-292">Beachten Sie, die die IntelliSense-Liste zeigt die **Asp: GridView** Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="dade5-292">Notice that the IntelliSense list shows the **asp:GridView** control.</span></span>

    <span data-ttu-id="dade5-293">IntelliSense im HTML-Editor bietet jetzt titelschreibweise Suche als auch teilweise übereinstimmenden (Abrufen aller Elemente, die den Begriff enthält).</span><span class="sxs-lookup"><span data-stu-id="dade5-293">IntelliSense in the HTML Editor now provides title-casing search, as well as partial matching (retrieving all elements that contains the term).</span></span>

    <span data-ttu-id="dade5-294">![Einfügen einer GridView mit IntelliSense-Listen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "Einfügen einer GridView mit IntelliSense-Listen")</span><span class="sxs-lookup"><span data-stu-id="dade5-294">![Inserting a GridView with IntelliSense lists](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "Inserting a GridView with IntelliSense lists")</span></span>

    <span data-ttu-id="dade5-295">*Einfügen einer GridView mit IntelliSense-Listen*</span><span class="sxs-lookup"><span data-stu-id="dade5-295">*Inserting a GridView with IntelliSense lists*</span></span>

    <span data-ttu-id="dade5-296">Wenn Sie eingeben  **&lt;Raster** erhalten Sie alle Elemente, die den Begriff überein, aber Visual Studio schlägt die **Gridview** Steuerelement:</span><span class="sxs-lookup"><span data-stu-id="dade5-296">If you type **&lt;grid** you will get all the items that match the term, but Visual Studio will suggest the **gridview** control:</span></span>

    <span data-ttu-id="dade5-297">![Einfügen einer GridView mit IntelliSense-Listen und teilweise übereinstimmenden](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "eine GridView mit IntelliSense-Listen und teilweise übereinstimmenden einfügen")</span><span class="sxs-lookup"><span data-stu-id="dade5-297">![Inserting a GridView with IntelliSense lists and partial matching](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "Inserting a GridView with IntelliSense lists and partial matching")</span></span>

    <span data-ttu-id="dade5-298">*Einfügen einer GridView mit partiellen Übereinstimmung und IntelliSense-Listen*</span><span class="sxs-lookup"><span data-stu-id="dade5-298">*Inserting a GridView with IntelliSense lists and partial matching*</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_HTML_Editor_Smart_Tags"></a>
#### <a name="task-4---html-editor-smart-tags"></a><span data-ttu-id="dade5-299">Aufgabe 4: HTML-Editor Smart Tags</span><span class="sxs-lookup"><span data-stu-id="dade5-299">Task 4 - HTML Editor Smart Tags</span></span>

<span data-ttu-id="dade5-300">Eine weitere Verbesserung im HTML-Editor ist die Smarttags-Funktion.</span><span class="sxs-lookup"><span data-stu-id="dade5-300">Another improvement in the HTML Editor is the Smart Tags feature.</span></span> <span data-ttu-id="dade5-301">Smarttags erleichtern die allgemeine oder sich wiederholende Entwicklungsaufgaben regelmäßig pro Steuerelement ausführen.</span><span class="sxs-lookup"><span data-stu-id="dade5-301">Smart tags make it easy to perform common or repetitive development tasks on a per-control basis.</span></span> <span data-ttu-id="dade5-302">Diese Funktion wurde bereits verfügbar im HTML-Designer, jedoch nicht in der HTML-Editor.</span><span class="sxs-lookup"><span data-stu-id="dade5-302">This feature was already available in the HTML Designer, but not in the HTML Editor.</span></span>

1. <span data-ttu-id="dade5-303">Open **Site.Master** und suchen Sie die **Asp-Menü:** Element.</span><span class="sxs-lookup"><span data-stu-id="dade5-303">Open **Site.Master** and locate the **asp:Menu** element.</span></span> <span data-ttu-id="dade5-304">Platzieren Sie den Cursor auf dem Starttag und beachten Sie, dass das kleine Symbol angezeigt, die am unteren Rand der Element - intelligenten Aufgabenmenü öffnen sie das klicken.</span><span class="sxs-lookup"><span data-stu-id="dade5-304">Place the cursor on the start tag and notice that the small glyph displayed at the bottom of the element - click it to open the smart tasks menu.</span></span> <span data-ttu-id="dade5-305">Beachten Sie, dass Sie schnellen Zugriff auf bestimmte Aufgaben im Zusammenhang mit der das Menüsteuerelement verfügen.</span><span class="sxs-lookup"><span data-stu-id="dade5-305">Notice that you have quick access to some tasks related to the Menu control.</span></span>

    <span data-ttu-id="dade5-306">![Aufgaben für den Menu-Steuerelement für intelligente](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "Aufgaben für den Menu-Steuerelement für intelligente")</span><span class="sxs-lookup"><span data-stu-id="dade5-306">![Smart tasks for the Menu control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "Smart tasks for the Menu control")</span></span>

    <span data-ttu-id="dade5-307">*Intelligente Aufgaben für den Menu-Steuerelement*</span><span class="sxs-lookup"><span data-stu-id="dade5-307">*Smart tasks for the Menu control*</span></span>

<a id="Ex2Task5"></a>

<a id="Task_5_-_Smart_Indentation"></a>
#### <a name="task-5---smart-indentation"></a><span data-ttu-id="dade5-308">Aufgabe 5: intelligenten Einzug</span><span class="sxs-lookup"><span data-stu-id="dade5-308">Task 5 - Smart Indentation</span></span>

<span data-ttu-id="dade5-309">Eine der empfohlenen Vorgehensweisen im HTML ist die geschachtelten Elemente, um die Lesbarkeit des Codes den Einzug.</span><span class="sxs-lookup"><span data-stu-id="dade5-309">One of the best practices in HTML is indenting the nested elements to keep the code readable.</span></span> <span data-ttu-id="dade5-310">In Visual Studio 2012 werden Sie feststellen, dass der Editor automatisch die Elemente zieht, während Sie den Code schreiben.</span><span class="sxs-lookup"><span data-stu-id="dade5-310">In Visual Studio 2012, you will notice that the editor automatically indents the elements while you are writing the code.</span></span>

> [!NOTE]
> <span data-ttu-id="dade5-311">In der vorherigen Version von Visual Studio intelligenten Einzug verfügbar wurde in der XML-Editor jedoch nicht in der HTML-Editor.</span><span class="sxs-lookup"><span data-stu-id="dade5-311">In previous version of Visual Studio, smart indentation was available in the XML editor but not in the HTML editor.</span></span>


1. <span data-ttu-id="dade5-312">Stellen Sie sicher, dass die Konfiguration der Einzug der HTML-Editor intelligenten Einzug festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="dade5-312">Make sure that the Indenting configuration on the HTML Editor is set to Smart Indentation.</span></span> <span data-ttu-id="dade5-313">Wählen Sie hierzu die **Tools | Optionen** Menüoption, und wählen Sie dann die **Text-Editor | HTML | Registerkarten** Seite im linken Bereich des Bildschirms.</span><span class="sxs-lookup"><span data-stu-id="dade5-313">To do that, select the **Tools | Options** menu option and then select the **Text Editor | HTML | Tabs** page in the left pane of the screen.</span></span> <span data-ttu-id="dade5-314">Wählen Sie die Option intelligenten Einzug.</span><span class="sxs-lookup"><span data-stu-id="dade5-314">Select the Smart indentation option.</span></span>

    <span data-ttu-id="dade5-315">![HTML-Editor-Einstellungen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "HTML-Editor-Einstellungen")</span><span class="sxs-lookup"><span data-stu-id="dade5-315">![HTML Editor settings](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "HTML Editor settings")</span></span>

    <span data-ttu-id="dade5-316">*HTML-Editor-Einstellungen*</span><span class="sxs-lookup"><span data-stu-id="dade5-316">*HTML Editor settings*</span></span>
2. <span data-ttu-id="dade5-317">Auf der **"default.aspx"** Seite, entfernen Sie alle Inhalte unter den audio-Element.</span><span class="sxs-lookup"><span data-stu-id="dade5-317">On the **Default.aspx** page, remove all the content under the audio element.</span></span>
3. <span data-ttu-id="dade5-318">Platzieren Sie den Cursor am Ende das öffnende **audio** -Element, und drücken **EINGABETASTE**.</span><span class="sxs-lookup"><span data-stu-id="dade5-318">Place the cursor at the end of the opening **audio** element and hit **ENTER**.</span></span>

    <span data-ttu-id="dade5-319">Beachten Sie, dass die neue Position des Cursors eine zusätzliche Einzugsebene verfügt.</span><span class="sxs-lookup"><span data-stu-id="dade5-319">Notice that the new position of cursor has an additional indentation level.</span></span>

    <span data-ttu-id="dade5-320">![Einzug in der HTML-Editor für intelligente](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "intelligente Einzug in der HTML-Editor")</span><span class="sxs-lookup"><span data-stu-id="dade5-320">![Smart indentation in the HTML Editor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "Smart indentation in the HTML Editor")</span></span>

    <span data-ttu-id="dade5-321">*Intelligenten Einzug in der HTML-Editor*</span><span class="sxs-lookup"><span data-stu-id="dade5-321">*Smart indentation in the HTML Editor*</span></span>
4. <span data-ttu-id="dade5-322">Wiederherstellen mit dem Inhalt, die Sie entfernt haben, oder schließen Sie das audio Tag **"default.aspx"** ohne die Änderungen zu speichern.</span><span class="sxs-lookup"><span data-stu-id="dade5-322">Restore the audio tag with the content you have removed, or close **Default.aspx** without saving the changes.</span></span>

<a id="Ex2Task6"></a>

<a id="Task_6_-_Extract_to_User_Control"></a>
#### <a name="task-6---extract-to-user-control"></a><span data-ttu-id="dade5-323">Aufgabe 6: "in Benutzersteuerelement extrahieren"</span><span class="sxs-lookup"><span data-stu-id="dade5-323">Task 6 - Extract to User Control</span></span>

<span data-ttu-id="dade5-324">Die Umgestaltung-Tools in Visual Studio, z. B. Extrahieren eines Teils des Codes einer Funktion enthalten sind zahlreichen Funktionen, die die Verbesserung und Umgestaltung den vorhandenen Code zu vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="dade5-324">The Refactoring tools included in Visual Studio, such as extracting a portion of code to a function, are great features that facilitate the improvement and the refactoring the existing code.</span></span> <span data-ttu-id="dade5-325">Die Entsprechung für die ASP.NET-Seiten wäre das Extrahieren von HTML-Code in einem Benutzersteuerelement.</span><span class="sxs-lookup"><span data-stu-id="dade5-325">The counterpart for ASP.NET pages would be the extraction of HTML code to a User Control.</span></span> <span data-ttu-id="dade5-326">Dies manuell würden, z. B. Erstellen eines neuen Benutzersteuerelements Codeabschnitt verschieben, auf das Benutzersteuerelement, registrieren ein Tagpräfix für das Benutzersteuerelement und, instanziieren schließlich das Benutzersteuerelement auf den Seiten, mehrere Schritte umfassen.</span><span class="sxs-lookup"><span data-stu-id="dade5-326">Doing it manually would involve several steps, like creating a new User Control, moving the code section to the User Control, registering a tag prefix for the User Control, and, finally, instantiating the User Control on the pages.</span></span> <span data-ttu-id="dade5-327">Jetzt das neue *in Benutzersteuerelement extrahieren* Tool alle diese Schritte automatisch für Sie ausführt.</span><span class="sxs-lookup"><span data-stu-id="dade5-327">Now, the new *Extract to User Control* tool automatically performs all those steps for you.</span></span>

<span data-ttu-id="dade5-328">In dieser Aufgabe verwenden Sie neue in kontextabhängige Vorgang Benutzersteuerelement extrahieren, um ein neues benutzerdefiniertes Steuerelement aus den ausgewählten Code zu generieren.</span><span class="sxs-lookup"><span data-stu-id="dade5-328">In this task, you will use the new Extract to User Control contextual operation to generate a new user control from the selected code.</span></span>

1. <span data-ttu-id="dade5-329">Auf der **"default.aspx"** Seite der **H2** und **audio** Elemente.</span><span class="sxs-lookup"><span data-stu-id="dade5-329">On the **Default.aspx** page, select the **H2** and **audio** elements.</span></span>
2. <span data-ttu-id="dade5-330">Klicken Sie mit der rechten Maustaste und wählen **in Benutzersteuerelement extrahieren**.</span><span class="sxs-lookup"><span data-stu-id="dade5-330">Right click and select **Extract to User Control**.</span></span>

    <span data-ttu-id="dade5-331">![Menüoption Benutzersteuerelement extrahieren](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "Menüoption Benutzersteuerelement extrahieren")</span><span class="sxs-lookup"><span data-stu-id="dade5-331">![Extract to User Control menu option](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "Extract to User Control menu option")</span></span>

    <span data-ttu-id="dade5-332">*Menüoption Benutzersteuerelement extrahieren*</span><span class="sxs-lookup"><span data-stu-id="dade5-332">*Extract to User Control menu option*</span></span>
3. <span data-ttu-id="dade5-333">Geben Sie einen Namen für das neue Benutzersteuerelement.</span><span class="sxs-lookup"><span data-stu-id="dade5-333">Type a name for the new user control.</span></span> <span data-ttu-id="dade5-334">Z. B. **Jukebox.ascx**, und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="dade5-334">For instance, **Jukebox.ascx**, and then click **OK**.</span></span>

    <span data-ttu-id="dade5-335">![Speichern die extrahierten Benutzersteuerelement](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "das extrahierte Benutzersteuerelement speichern")</span><span class="sxs-lookup"><span data-stu-id="dade5-335">![Saving the extracted user control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "Saving the extracted user control")</span></span>

    <span data-ttu-id="dade5-336">*Speichern die extrahierten Benutzersteuerelement*</span><span class="sxs-lookup"><span data-stu-id="dade5-336">*Saving the extracted user control*</span></span>
4. <span data-ttu-id="dade5-337">Beachten Sie, dass der ausgewählte Code in einem Benutzersteuerelement extrahiert wurden und am ursprüngliche Speicherort des ausgewählten Codes mit einer Instanz des neuen Benutzersteuerelements ersetzt wurde.</span><span class="sxs-lookup"><span data-stu-id="dade5-337">Notice that the selected code was extracted to a user control and the original location of the selected code was replaced with an instance of the new user control.</span></span>

    <span data-ttu-id="dade5-338">![Seite automatisch aktualisiert, um das neue Benutzersteuerelement verwenden](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "Seite automatisch aktualisiert, um das neue Benutzersteuerelement verwenden")</span><span class="sxs-lookup"><span data-stu-id="dade5-338">![Page automatically updated to use the new user control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "Page automatically updated to use the new user control")</span></span>

    <span data-ttu-id="dade5-339">*Seite aktualisiert automatisch, um das neue Benutzersteuerelement verwenden*</span><span class="sxs-lookup"><span data-stu-id="dade5-339">*Page automatically updated to use the new user control*</span></span>
5. <span data-ttu-id="dade5-340">Drücken Sie **F5** führen Sie die Seite, und stellen Sie sicher, dass das Steuerelement funktioniert.</span><span class="sxs-lookup"><span data-stu-id="dade5-340">Press **F5** to run the page and verify that the control works.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Whats_New_in_the_JavaScript_Editor"></a>
### <a name="exercise-3-whats-new-in-the-javascript-editor"></a><span data-ttu-id="dade5-341">Übung 3: Was ist neu in der JavaScript-Editor</span><span class="sxs-lookup"><span data-stu-id="dade5-341">Exercise 3: What's New in the JavaScript Editor</span></span>

<span data-ttu-id="dade5-342">Schreiben oder Bearbeiten von JavaScript-Code ist eine einfache Aufgabe nicht, insbesondere, wenn die Anwendung wird gestartet, an Größe zunehmen, und Sie Umgang mit langer Dateien und Hunderte von Funktionen.</span><span class="sxs-lookup"><span data-stu-id="dade5-342">Writing or editing JavaScript code is not an easy task, especially when your application starts to grow in size and you find yourself dealing with long files and hundreds of functions.</span></span> <span data-ttu-id="dade5-343">Skriptentwickler müssen in der Regel keine werden zusätzliche Aufgaben ausführen, um die Lesbarkeit von Code und in Dateien zu navigieren.</span><span class="sxs-lookup"><span data-stu-id="dade5-343">Script developers usually have to do some extra work to maintain code legibility and navigate across files.</span></span> <span data-ttu-id="dade5-344">Unter Einbeziehung von JavaScript-Bibliotheken wie jQuery geworden Skript Navigation ist eine Herausforderung aufgrund der Codelänge.</span><span class="sxs-lookup"><span data-stu-id="dade5-344">With the inclusion of JavaScript libraries like jQuery, script navigation has become a challenge itself because of the code length.</span></span>

<span data-ttu-id="dade5-345">Visual Studio hat den JavaScript-Editor mit die Zusage, die den Modus Code organisiert und zugänglichen Datenbanken stellen erneuert.</span><span class="sxs-lookup"><span data-stu-id="dade5-345">Visual Studio has renewed the JavaScript editor with the promise to make the code mode accessible and organized.</span></span> <span data-ttu-id="dade5-346">Viele Visual Studio-Funktionen, die bereits in c# oder VB-Editoren vorhanden waren, werden jetzt in der JavaScript-Editor implementiert: Gehe zu Definition, automatischer Einzug, Dokumentation und Validierung, wenn Sie schreiben.</span><span class="sxs-lookup"><span data-stu-id="dade5-346">Many Visual Studio features that already existed in C# or VB editors are now implemented in the JavaScript editor: Go To Definition, automatic indentation, documentation and validation when you are writing.</span></span> <span data-ttu-id="dade5-347">Mit der erneuerte IntelliSense-Liste müssen Sie den JavaScript-Funktion Katalog jederzeit griffbereit.</span><span class="sxs-lookup"><span data-stu-id="dade5-347">With the renewed IntelliSense list you will have the JavaScript function catalog at your fingertips.</span></span>

<span data-ttu-id="dade5-348">In dieser Übung erfahren Sie einige der neuen Features und Verbesserungen der JavaScript-Editor.</span><span class="sxs-lookup"><span data-stu-id="dade5-348">In this exercise, you will learn some of the new features and improvements of JavaScript editor.</span></span> <span data-ttu-id="dade5-349">Sie Beispieldateien durchsuchen und ermitteln jeweils die neuen Eigenschaften, die JavaScript-Programmierung in Visual Studio 2012 effizienter machen.</span><span class="sxs-lookup"><span data-stu-id="dade5-349">You will browse sample files and discover each of the new characteristics that will make your JavaScript programming more efficient within Visual Studio 2012.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_JavaScript_Editor_New_Features"></a>
#### <a name="task-1---javascript-editor-new-features"></a><span data-ttu-id="dade5-350">Aufgabe 1: neue Funktionen für JavaScript-Editor</span><span class="sxs-lookup"><span data-stu-id="dade5-350">Task 1 - JavaScript Editor New Features</span></span>

<span data-ttu-id="dade5-351">Diese Aufgabe werden einige der neuen JavaScript-Editor-Funktionen vorgestellt dem darauf konzentrieren, Organisieren den Code und eine bessere benutzererfahrung zu schalten.</span><span class="sxs-lookup"><span data-stu-id="dade5-351">This task will introduce you to some of the new JavaScript editor features, which focus on organizing your code and bringing a better user experience.</span></span>

1. <span data-ttu-id="dade5-352">Wenn nicht bereits geöffnet ist, starten Sie **Visual Studio** , und öffnen Sie die **WhatsNewASPNET.sln** Projektmappe befindet sich in der **Source\WhatsNewASPNET** Ordner dieser Anleitung.</span><span class="sxs-lookup"><span data-stu-id="dade5-352">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="dade5-353">Drücken Sie **F5** um die Anwendung auszuführen, klicken Sie dann auf den Link "JavaScript" in der Navigationsleiste.</span><span class="sxs-lookup"><span data-stu-id="dade5-353">Press **F5** to run the application, then click the JavaScript link in the navigation bar.</span></span> <span data-ttu-id="dade5-354">Aktualisieren Sie die Seite mehrere Male und Überprüfung wie der Indikator erhöht.</span><span class="sxs-lookup"><span data-stu-id="dade5-354">Refresh the page several times and check how the counter increments.</span></span>

    <span data-ttu-id="dade5-355">![Seitenzähler](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "Seitenzähler")</span><span class="sxs-lookup"><span data-stu-id="dade5-355">![Page counter](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "Page counter")</span></span>

    <span data-ttu-id="dade5-356">*Seitenzähler*</span><span class="sxs-lookup"><span data-stu-id="dade5-356">*Page counter*</span></span>
3. <span data-ttu-id="dade5-357">Schließen Sie den Browser, und wechseln Sie zurück zu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dade5-357">Close the browser and go back to Visual Studio.</span></span>
4. <span data-ttu-id="dade5-358">Öffnen der **JavaScript.aspx** Seite, und suchen Sie die **&lt;Skript&gt;** Block (siehe unten).</span><span class="sxs-lookup"><span data-stu-id="dade5-358">Open the **JavaScript.aspx** page and locate the **&lt;script&gt;** block (shown below).</span></span>

    <span data-ttu-id="dade5-359">Der folgende Code verwendet HTML5 lokalen Speicher zum Speichern einer *PageLoadCount* Variable an, wie oft speichert die Seite vom aktuellen Benutzer besucht wurde wurde.</span><span class="sxs-lookup"><span data-stu-id="dade5-359">The following code uses HTML5 local storage to store a *pageLoadCount* variable that stores the number of times the page has been visited by the current user.</span></span> <span data-ttu-id="dade5-360">Lokaler Speicher ist eine clientseitige Schlüssel-Wert-Datenbank, die mit dem HTML5-Standard eingeführt.</span><span class="sxs-lookup"><span data-stu-id="dade5-360">Local Storage is a client-side key-value database introduced with the HTML5 standard.</span></span> <span data-ttu-id="dade5-361">Die Daten werden auf dem lokalen Computer, in den Browser des Benutzers gespeichert.</span><span class="sxs-lookup"><span data-stu-id="dade5-361">The data is saved on the local machine, inside the user's browser.</span></span>

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample7.html)]

    > [!NOTE]
    > <span data-ttu-id="dade5-362">Stellen Sie sicher, dass die "DOCTYPE" auf XHTML5 festgelegt ist, bevor Sie mit den nächsten Schritten fortfahren.</span><span class="sxs-lookup"><span data-stu-id="dade5-362">Ensure the DOCTYPE is set to XHTML5 before proceeding with the next steps.</span></span>
5. <span data-ttu-id="dade5-363">Bearbeiten Sie den Code, und beachten Sie, dass IntelliSense für JavaScript HTML5-Funktionen wie die lokalen Speicher und ihre internen Methoden enthält.</span><span class="sxs-lookup"><span data-stu-id="dade5-363">Edit the code and notice that IntelliSense for JavaScript includes HTML5 features, like local storage, and their inner methods.</span></span>

    <span data-ttu-id="dade5-364">![HTML5-JavaScript-Funktionen in JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "HTML5-JavaScript-Funktionen in JavaScript")</span><span class="sxs-lookup"><span data-stu-id="dade5-364">![HTML5 JavaScript features in JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "HTML5 JavaScript features in JavaScript")</span></span>

    <span data-ttu-id="dade5-365">*HTML5-JavaScript-Funktionen in JavaScript*</span><span class="sxs-lookup"><span data-stu-id="dade5-365">*HTML5 JavaScript features in JavaScript*</span></span>
6. <span data-ttu-id="dade5-366">Klicken Sie auf eine öffnende Klammer (**{**) aus, das die Skriptoptionen code, und beachten Sie, dass die Klammern hervorgehoben werden.</span><span class="sxs-lookup"><span data-stu-id="dade5-366">Click any opening bracket (**{**) from the scripting code and notice that the brackets are highlighted.</span></span>

    <span data-ttu-id="dade5-367">![Klammern hervorgehoben werden](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "Klammern hervorgehoben werden")</span><span class="sxs-lookup"><span data-stu-id="dade5-367">![Brackets are highlighted](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "Brackets are highlighted")</span></span>

    <span data-ttu-id="dade5-368">*Es werden Klammern hervorgehoben.*</span><span class="sxs-lookup"><span data-stu-id="dade5-368">*Brackets are highlighted*</span></span>
7. <span data-ttu-id="dade5-369">Kommentieren Sie die Funktion **testAutoAlign()** (Wählen Sie die drei Zeilen, und Sie können **STRG** + **K**; **STRG** + **U**), und suchen Sie den Cursor innerhalb der Funktionscode.</span><span class="sxs-lookup"><span data-stu-id="dade5-369">Uncomment the function **testAutoAlign()** (select the three lines and you can use **CTRL** + **K**; **CTRL** + **U**) and locate the cursor inside the function code.</span></span> <span data-ttu-id="dade5-370">Drücken Sie EINGABETASTE, um eine zweite Zeile angefügt werden soll.</span><span class="sxs-lookup"><span data-stu-id="dade5-370">Press enter to append a second line.</span></span> <span data-ttu-id="dade5-371">Beachten Sie, dass der Code jetzt **ausgerichtet** und **automatisch eingezogen**.</span><span class="sxs-lookup"><span data-stu-id="dade5-371">Notice that the code is now **aligned** and **auto-indented**.</span></span>

    <span data-ttu-id="dade5-372">![JavaScript-Code wird automatisch ausgerichtet](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "ist "Auto ausgerichtet" JavaScript-Code")</span><span class="sxs-lookup"><span data-stu-id="dade5-372">![JavaScript code is auto aligned](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "JavaScript code is auto aligned")</span></span>

    <span data-ttu-id="dade5-373">*JavaScript-Code wird automatisch ausgerichtet*</span><span class="sxs-lookup"><span data-stu-id="dade5-373">*JavaScript code is auto aligned*</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Validating_JavaScript"></a>
#### <a name="task-2---validating-javascript"></a><span data-ttu-id="dade5-374">Aufgabe 2: Überprüfen von JavaScript</span><span class="sxs-lookup"><span data-stu-id="dade5-374">Task 2 - Validating JavaScript</span></span>

<span data-ttu-id="dade5-375">In dieser Aufgabe werden neue JavaScript-Validierung für den Standard ECMAScript5 ermittelt werden.</span><span class="sxs-lookup"><span data-stu-id="dade5-375">In this task, you will discover the new JavaScript validation for the ECMAScript5 standard.</span></span> <span data-ttu-id="dade5-376">Diese Funktion hilft Ihnen, kompatibel JavaScript-Code, bei Problemen mit verhindern, dass Skripts vor der Bereitstellung der Website zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="dade5-376">This feature will help you to write compliant JavaScript code, while preventing scripting issues before site deployment.</span></span>

> [!NOTE]
> <span data-ttu-id="dade5-377">Visual Studio 2010 implementiert ECMAStript3-Kompatibilität, während Visual Studio 2012 ECMAScript5-Kompatibilität bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="dade5-377">Visual Studio 2010 implemented ECMAStript3 compliance, while Visual Studio 2012 provides ECMAScript5 compliance.</span></span>


1. <span data-ttu-id="dade5-378">Open **ECMA5script5.js** unterhalb der **Scripts\custom** Projektordner.</span><span class="sxs-lookup"><span data-stu-id="dade5-378">Open **ECMA5script5.js** located under the **Scripts\custom** project folder.</span></span> <span data-ttu-id="dade5-379">Testen Sie jetzt Validierung für ECMAScript5-standard.</span><span class="sxs-lookup"><span data-stu-id="dade5-379">You will now test validation for ECMAScript5 standard.</span></span>

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample8.html)]

    <span data-ttu-id="dade5-380">Sehen Sie sich die &quot; **verwenden strict** &quot; Richtung in der ersten Zeile der Datei, wodurch ECMAScript5 **strict-Modus**.</span><span class="sxs-lookup"><span data-stu-id="dade5-380">You can check out the &quot; **use strict** &quot; direction in the first line of the file, which enables ECMAScript5 **strict mode**.</span></span> <span data-ttu-id="dade5-381">Dieser Modus besteht aus einer Teilmenge der Programmiersprache, die Mehrdeutigkeiten aus der früheren Edition verdeutlicht und fügt einige neuen Funktionen, z. B. Getter und Setter, Library-Unterstützung für JSON und vollständigere Reflektion auf die Objekteigenschaften aus.</span><span class="sxs-lookup"><span data-stu-id="dade5-381">This mode consists in a subset of the language that clarifies ambiguities from the past edition, and adds some new features, such as getters and setters, library support for JSON, and more complete reflection on object properties.</span></span>
2. <span data-ttu-id="dade5-382">Öffnen der **Fehlerliste** Wenn nicht bereits geöffnet (**Ansicht** Menü | **Fehlerliste**).</span><span class="sxs-lookup"><span data-stu-id="dade5-382">Open the **Error List** if not already opened (**View** menu | **Error List**).</span></span> <span data-ttu-id="dade5-383">Beachten Sie, dass die **Funktion** Deklaration unterstrichen ist.</span><span class="sxs-lookup"><span data-stu-id="dade5-383">Notice the **function** declaration is underlined.</span></span> <span data-ttu-id="dade5-384">Dies liegt daran in ECMA5 Standardfunktionen in Language-Strukturen geschachtelt werden nicht möglich.</span><span class="sxs-lookup"><span data-stu-id="dade5-384">This is because in ECMA5 standard functions cannot be nested inside language structures.</span></span> <span data-ttu-id="dade5-385">Der Fehler wird die Liste unterhalb der Warndetails angezeigt.</span><span class="sxs-lookup"><span data-stu-id="dade5-385">In the error list below you will see the warning details.</span></span>

    <span data-ttu-id="dade5-386">![JavaScript-Überprüfungsfehlermeldung](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "Überprüfungsfehlermeldung JavaScript")</span><span class="sxs-lookup"><span data-stu-id="dade5-386">![JavaScript validation error message](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "JavaScript validation error message")</span></span>

    <span data-ttu-id="dade5-387">*JavaScript-Überprüfungsfehlermeldung*</span><span class="sxs-lookup"><span data-stu-id="dade5-387">*JavaScript validation error message*</span></span>
3. <span data-ttu-id="dade5-388">Kommentieren Sie Sie aus der **&quot;verwenden strenge&quot;** Richtung und beachten Sie, die Fehler nicht mehr angezeigt, aber die Warnungen bleiben.</span><span class="sxs-lookup"><span data-stu-id="dade5-388">Comment out the **&quot;use strict&quot;** direction and notice that errors disappear, but the warnings remain.</span></span>
4. <span data-ttu-id="dade5-389">Schreiben Sie eine beliebige Zeichenfolge wie in der letzten Zeile der Datei **&quot;testen&quot;** (einschließlich der Anführungszeichen kennzeichnen, ist als Zeichenfolge).</span><span class="sxs-lookup"><span data-stu-id="dade5-389">In the last line of the file, write any string like **&quot;test&quot;** (include the quotation marks to indicate it is as string).</span></span> <span data-ttu-id="dade5-390">Schreiben Sie einen Punkt neben die Zeichenfolge, die IntelliSense-Liste angezeigt, und wählen Sie die **trim** Option.</span><span class="sxs-lookup"><span data-stu-id="dade5-390">Write a period next to the string to display the IntelliSense list, and select the **trim** option.</span></span>

    <span data-ttu-id="dade5-391">ECMAScript5-Standard verfügen Zeichenfolgenwerte und Variablen, String-Methoden, die definiert, wie das trim, Großbuchstaben, suchen und ersetzen.</span><span class="sxs-lookup"><span data-stu-id="dade5-391">In ECMAScript5 standard, string values and variables also have string methods defined, like trim, uppercase, search and replace.</span></span>

    <span data-ttu-id="dade5-392">![IntelliSense-Liste in JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "in JavaScript IntelliSense-Liste")</span><span class="sxs-lookup"><span data-stu-id="dade5-392">![IntelliSense list in JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "IntelliSense list in JavaScript")</span></span>

    <span data-ttu-id="dade5-393">*IntelliSense-Liste in JavaScript*</span><span class="sxs-lookup"><span data-stu-id="dade5-393">*IntelliSense list in JavaScript*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_XML_Documentation_for_JavaScript"></a>
#### <a name="task-3---xml-documentation-for-javascript"></a><span data-ttu-id="dade5-394">Aufgabe 3 - XML-Dokumentation für JavaScript</span><span class="sxs-lookup"><span data-stu-id="dade5-394">Task 3 - XML Documentation for JavaScript</span></span>

<span data-ttu-id="dade5-395">In dieser Aufgabe untersuchen Sie die Visual Studio-Funktionen für die XML-Dokumentation in JavaScript.</span><span class="sxs-lookup"><span data-stu-id="dade5-395">In this task, you will explore Visual Studio features for XML documentation in JavaScript.</span></span> <span data-ttu-id="dade5-396">Sie sehen, dass die JavaScript-IntelliSense-Liste zeigt nun für jede Funktion die XML-Dokumentation.</span><span class="sxs-lookup"><span data-stu-id="dade5-396">You will see the JavaScript IntelliSense list now shows the XML documentation of each function.</span></span> <span data-ttu-id="dade5-397">Darüber hinaus werden Sie das Navigationsfeature "in JavaScript feststellen.</span><span class="sxs-lookup"><span data-stu-id="dade5-397">Additionally, you will discover the navigation feature in JavaScript.</span></span>

1. <span data-ttu-id="dade5-398">Open **XMLDoc.js** -Datei im **Skripts/Custom** Projektordner.</span><span class="sxs-lookup"><span data-stu-id="dade5-398">Open **XMLDoc.js** file located in **Scripts/custom** project folder.</span></span> <span data-ttu-id="dade5-399">Diese Datei enthält XML-Dokumentation auf jedem der JavaScript-Funktionen.</span><span class="sxs-lookup"><span data-stu-id="dade5-399">This file contains XML documentation on each of the JavaScript functions.</span></span>

    <span data-ttu-id="dade5-400">![JavaScript XML-Dokumentation integriert, um IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "JavaScript XML-Dokumentation integriert, um IntelliSense")</span><span class="sxs-lookup"><span data-stu-id="dade5-400">![JavaScript XML documentation integrated to IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "JavaScript XML documentation integrated to IntelliSense")</span></span>

    <span data-ttu-id="dade5-401">*JavaScript XML-Dokumentation, die für IntelliSense integriert*</span><span class="sxs-lookup"><span data-stu-id="dade5-401">*JavaScript XML documentation integrated to IntelliSense*</span></span>
2. <span data-ttu-id="dade5-402">Im folgenden **hinzufügen** -Funktion in **XMLDoc.js** Datei, erstellen Sie eine neue Funktion mit dem Namen **testen**.</span><span class="sxs-lookup"><span data-stu-id="dade5-402">Below **add** function in **XMLDoc.js** file, create a new function named **test**.</span></span>
3. <span data-ttu-id="dade5-403">In der **testen** funktionieren, rufen Sie die **Multiplizieren** -Funktion, die zwei Parameter empfängt.</span><span class="sxs-lookup"><span data-stu-id="dade5-403">In the **test** function, call the **multiply** function that receives two parameters.</span></span> <span data-ttu-id="dade5-404">Beachten Sie das Kontrollkästchen für die QuickInfo wird angezeigt. die **Multiplizieren** Dokumentation-Funktion.</span><span class="sxs-lookup"><span data-stu-id="dade5-404">Notice the tooltip box is showing the **multiply** function documentation.</span></span>

    [!code-javascript[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample9.js)]

    <span data-ttu-id="dade5-405">![XML-Dokumentation für JavaScript-Funktionen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "XML-Dokumentation für JavaScript-Funktionen")</span><span class="sxs-lookup"><span data-stu-id="dade5-405">![XML documentation for JavaScript functions](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "XML documentation for JavaScript functions")</span></span>

    <span data-ttu-id="dade5-406">*XML-Dokumentation für JavaScript-Funktionen*</span><span class="sxs-lookup"><span data-stu-id="dade5-406">*XML documentation for JavaScript functions*</span></span>
4. <span data-ttu-id="dade5-407">Führen Sie die Funktion Call-Anweisung und den Typ einer *Punkt* zum Öffnen der IntelliSense-Liste auf den zurückgegebenen Wert.</span><span class="sxs-lookup"><span data-stu-id="dade5-407">Complete the function call statement and type a *dot* to open the IntelliSense list on the returned value.</span></span> <span data-ttu-id="dade5-408">Beachten Sie die Visual Studio erkennt die **zurückgeben** Wert in der Dokumentation, die den Wert als Zahl behandelt.</span><span class="sxs-lookup"><span data-stu-id="dade5-408">Notice that Visual Studio is detecting the **return** value in the documentation, treating the value as a number.</span></span>

    <span data-ttu-id="dade5-409">![XML-Dokumentation für die Rückgabetypen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "XML-Dokumentation für die Rückgabetypen")</span><span class="sxs-lookup"><span data-stu-id="dade5-409">![XML documentation for return types](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "XML documentation for return types")</span></span>

    <span data-ttu-id="dade5-410">*XML-Dokumentation für die Rückgabetypen*</span><span class="sxs-lookup"><span data-stu-id="dade5-410">*XML documentation for return types*</span></span>
5. <span data-ttu-id="dade5-411">Fügen Sie nun, einen Aufruf von Funktion hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="dade5-411">Now, insert a call to add function.</span></span> <span data-ttu-id="dade5-412">Beachten Sie, dass der JavaScript-Editor jetzt funktionsüberladungen unterstützt.</span><span class="sxs-lookup"><span data-stu-id="dade5-412">Notice that the JavaScript editor now supports function overloads.</span></span> <span data-ttu-id="dade5-413">Wenn Sie einen Funktionsnamen schreiben, werden Sie keines der verfügbaren Überladungen, die in der Dokumentation angegebenen auswählen können.</span><span class="sxs-lookup"><span data-stu-id="dade5-413">When you write a function name, you will be able to select any of the available overloads specified in the documentation.</span></span>

    <span data-ttu-id="dade5-414">![XML-Dokumentation für Überladungen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "XML-Dokumentation für Überladungen")</span><span class="sxs-lookup"><span data-stu-id="dade5-414">![XML documentation for overloads](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "XML documentation for overloads")</span></span>

    <span data-ttu-id="dade5-415">*XML-Dokumentation für Überladungen*</span><span class="sxs-lookup"><span data-stu-id="dade5-415">*XML documentation for overloads*</span></span>
6. <span data-ttu-id="dade5-416">Open **GotoDefinition.js** Datei, und suchen Sie die **$().html()** Funktionsaufruf.</span><span class="sxs-lookup"><span data-stu-id="dade5-416">Open **GotoDefinition.js** file and locate the **$().html()** function call.</span></span> <span data-ttu-id="dade5-417">Suchen Sie den Cursor auf **html**.</span><span class="sxs-lookup"><span data-stu-id="dade5-417">Locate the cursor on **html**.</span></span>
7. <span data-ttu-id="dade5-418">Drücken Sie **F12** , und navigieren Sie zur Definition.</span><span class="sxs-lookup"><span data-stu-id="dade5-418">Press **F12** and navigate to the definition.</span></span> <span data-ttu-id="dade5-419">Beachten Sie, Sie können jetzt zugreifen und Durchsuchen von JavaScript-Code ohne Verwendung der **suchen** Tool.</span><span class="sxs-lookup"><span data-stu-id="dade5-419">Notice you can now access and browse your JavaScript code without using the **Find** tool.</span></span>
8. <span data-ttu-id="dade5-420">Suchen Sie den Cursor auf die jQuery-Anweisung vor den Signaturblock am unteren Rand der Codedatei ein.</span><span class="sxs-lookup"><span data-stu-id="dade5-420">Locate the cursor on the jQuery instruction prior to the signature block at the bottom of the code file.</span></span> <span data-ttu-id="dade5-421">Drücken Sie **F12**.</span><span class="sxs-lookup"><span data-stu-id="dade5-421">Press **F12**.</span></span> <span data-ttu-id="dade5-422">Sie gelangen auf die jQuery-Bibliotheksdatei.</span><span class="sxs-lookup"><span data-stu-id="dade5-422">You will navigate to the jQuery library file.</span></span> <span data-ttu-id="dade5-423">Beachten Sie, Sie können auch über die jQuery-Dateien, die über navigieren **F12**.</span><span class="sxs-lookup"><span data-stu-id="dade5-423">Notice you can also navigate across the jQuery files using **F12**.</span></span>

    <span data-ttu-id="dade5-424">![Navigieren zu jQuery-Definitionen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "navigieren jQuery-Definitionen")</span><span class="sxs-lookup"><span data-stu-id="dade5-424">![Navigating to jQuery definitions](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "Navigating to jQuery definitions")</span></span>

    <span data-ttu-id="dade5-425">*Navigieren zu jQuery-Definitionen*</span><span class="sxs-lookup"><span data-stu-id="dade5-425">*Navigating to jQuery definitions*</span></span>

> [!NOTE]
> <span data-ttu-id="dade5-426">Stellen Sie sicher, dass GotoDefinition.js keine Syntaxfehler aufweist, bevor Sie die Datei speichern.</span><span class="sxs-lookup"><span data-stu-id="dade5-426">Make sure that GotoDefinition.js has no syntax errors before saving the file.</span></span>


<a id="Exercise4"></a>

<a id="Exercise_4_Bundling_and_Minification"></a>
### <a name="exercise-4-bundling-and-minification"></a><span data-ttu-id="dade5-427">Übung 4: Bündelung und Minimierung</span><span class="sxs-lookup"><span data-stu-id="dade5-427">Exercise 4: Bundling and Minification</span></span>

<span data-ttu-id="dade5-428">Wie oft schließen Ihre Websites mehr als ein JavaScript- oder CSS-Datei?</span><span class="sxs-lookup"><span data-stu-id="dade5-428">How many times do your websites include more than one JavaScript or CSS file?</span></span> <span data-ttu-id="dade5-429">Dies ist ein sehr häufiges Szenario, bei denen Bündelung und Minimierung beitragen können, reduzieren Sie die Dateigröße, und stellen die Website, die schneller ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="dade5-429">This is a very common scenario where bundling and minification can help to reduce the file size and make the site perform faster.</span></span> <span data-ttu-id="dade5-430">Der neuen bundling Funktion in ASP.NET 4.5 packs einen Satz von JS oder CSS-Dateien in einem einzelnen Element, und reduziert die Größe von weboptimierungsframework den Inhalt (d. h. Entfernen von Leerzeichen nicht erforderlich, Entfernen von Kommentaren, Bezeichnern verringert).</span><span class="sxs-lookup"><span data-stu-id="dade5-430">The new bundling feature in ASP.NET 4.5 packs a set of JS or CSS files into a single element, and reduces its size by minifying the content ( i.e. removing not required blank spaces, removing comments, reducing identifiers ).</span></span>

<span data-ttu-id="dade5-431">Bundling und Minimierung in ASP.NET 4.5 erfolgt zur Laufzeit, damit der Prozess identifizieren den Benutzer-Agent (z. B. Internet Explorer, Mozilla usw.), und kann daher die Komprimierung verbessern, indem Sie den Benutzer Browser (z. B. entfernt haben Zugriff auf Inhalte, die bestimmte Mozilla ist Wenn die Anforderung IE stammen).</span><span class="sxs-lookup"><span data-stu-id="dade5-431">Bundling and minification in ASP.NET 4.5 is performed at runtime, so that the process can identify the user agent (for example IE, Mozilla, etc), and thus, improve the compression by targeting the user browser (for instance, removing stuff that is Mozilla specific when the request comes from IE).</span></span>

<span data-ttu-id="dade5-432">In dieser Übung erfahren Sie, wie zum Aktivieren und verwenden die verschiedenen Typen von Bündelung und Minimierung in ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="dade5-432">In this exercise, you will learn how to enable and use the different types of bundling and minification in ASP.NET 4.5.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Installing_the_Bundling_and_Minification_Package_from_NuGet"></a>
#### <a name="task-1---installing-the-bundling-and-minification-package-from-nuget"></a><span data-ttu-id="dade5-433">Aufgabe 1: Installieren der Bündelung und Minimierung-Pakets von NuGet</span><span class="sxs-lookup"><span data-stu-id="dade5-433">Task 1 - Installing the Bundling and Minification Package from NuGet</span></span>

1. <span data-ttu-id="dade5-434">Wenn nicht bereits geöffnet ist, starten Sie **Visual Studio** , und öffnen Sie die **WhatsNewASPNET.sln** Projektmappe befindet sich in der **Source\WhatsNewASPNET** Ordner dieser Anleitung.</span><span class="sxs-lookup"><span data-stu-id="dade5-434">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="dade5-435">Öffnen Sie das NuGet-Paket-Manager-Konsole.</span><span class="sxs-lookup"><span data-stu-id="dade5-435">Open the NuGet Package Manager Console.</span></span> <span data-ttu-id="dade5-436">Zu diesem Zweck verwenden Sie das Menü **Ansicht** | **Weitere Fenster** | **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="dade5-436">To do this, use the menu **View** | **Other Windows** | **Package Manager Console**.</span></span>

    <span data-ttu-id="dade5-437">![Öffnen die Paket-Manager-file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "die Paket-Manager-Konsole öffnen")</span><span class="sxs-lookup"><span data-stu-id="dade5-437">![Opening the package manager file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "Opening the package manager console")</span></span>

    <span data-ttu-id="dade5-438">*Öffnen die Paket-Manager-Konsole*</span><span class="sxs-lookup"><span data-stu-id="dade5-438">*Opening the package manager console*</span></span>
3. <span data-ttu-id="dade5-439">In der **Paket-Manager-Konsole** Typ **Install-Package Microsoft.Web.Optimization** , und drücken Sie **EINGABETASTE**.</span><span class="sxs-lookup"><span data-stu-id="dade5-439">In the **Package Manager Console,** type **Install-Package Microsoft.Web.Optimization** and press **ENTER**.</span></span>

<a id="Ex4Task2"></a>

<a id="Task_2_-_Default_Bundles"></a>
#### <a name="task-2---default-bundles"></a><span data-ttu-id="dade5-440">Aufgabe 2: Standard-Pakete</span><span class="sxs-lookup"><span data-stu-id="dade5-440">Task 2 - Default Bundles</span></span>

<span data-ttu-id="dade5-441">Die einfachste Methode zum Verwenden von Bündelung und Minimierung ist die Standard-Pakete zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="dade5-441">The simplest way to use bundling and minification is to enable the default bundles.</span></span> <span data-ttu-id="dade5-442">Diese Methode verwendet Konventionen können Sie die gebündelte und verkleinerte Version für die JS und CSS-Dateien in einem Ordner zu verweisen.</span><span class="sxs-lookup"><span data-stu-id="dade5-442">This method uses conventions to let you reference the bundled and minified version for the JS and CSS files in a folder.</span></span>

<span data-ttu-id="dade5-443">In dieser Aufgabe lernen Sie, das Aktivieren und die gebündelten und verkleinerte JS und CSS-Dateien zu verweisen und die resultierende Ausgabe anzeigen.</span><span class="sxs-lookup"><span data-stu-id="dade5-443">In this task, you will learn how to enable and reference the bundled and minified JS and CSS files and view the resulting output.</span></span>

1. <span data-ttu-id="dade5-444">Wenn nicht bereits geöffnet ist, starten Sie **Visual Studio** , und öffnen Sie die **WhatsNewASPNET.sln** Projektmappe befindet sich in der **Source\WhatsNewASPNET** Ordner dieser Anleitung.</span><span class="sxs-lookup"><span data-stu-id="dade5-444">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="dade5-445">In der **Projektmappen-Explorer**, erweitern Sie die **Stile**, **Scripts\custom** und **Scripts\bundle** Ordner.</span><span class="sxs-lookup"><span data-stu-id="dade5-445">In the **Solution Explorer**, expand the **Styles**, **Scripts\custom** and **Scripts\bundle** folders.</span></span>

    <span data-ttu-id="dade5-446">Beachten Sie, dass die Anwendung mehrere CSS- und JS-Datei verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="dade5-446">Notice that the application is using more than one CSS and JS file.</span></span>

    <span data-ttu-id="dade5-447">![Mehrere Stylesheets und JavaScript-Dateien in der Anwendung](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "mehrere Stylesheets und JavaScript-Dateien in der Anwendung")</span><span class="sxs-lookup"><span data-stu-id="dade5-447">![Multiple Stylesheets and JavaScript files in the application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "Multiple Stylesheets and JavaScript files in the application")</span></span>

    <span data-ttu-id="dade5-448">*Mehrere Stylesheets und JavaScript-Dateien in der Anwendung*</span><span class="sxs-lookup"><span data-stu-id="dade5-448">*Multiple Stylesheets and JavaScript files in the application*</span></span>
3. <span data-ttu-id="dade5-449">Öffnen der **Global.asax.cs** Datei.</span><span class="sxs-lookup"><span data-stu-id="dade5-449">Open the **Global.asax.cs** file.</span></span>

    <span data-ttu-id="dade5-450">Beachten Sie, dass die neue **Microsoft.Web.Optimization** Namespace am Anfang der Datei auskommentiert ist.</span><span class="sxs-lookup"><span data-stu-id="dade5-450">Notice that the new **Microsoft.Web.Optimization** namespace is commented out at the beginning of the file.</span></span> <span data-ttu-id="dade5-451">Kommentieren Sie die mit Direktive, um die Bündelung und Minimierung Funktionen umfassen.</span><span class="sxs-lookup"><span data-stu-id="dade5-451">Uncomment the using directive to include the bundling and minification features.</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample10.cs)]
~~~
4. <span data-ttu-id="dade5-452">Suchen Sie die **Anwendung\_starten** Methode.</span><span class="sxs-lookup"><span data-stu-id="dade5-452">Locate the **Application\_Start** method.</span></span>

    <span data-ttu-id="dade5-453">Kommentieren Sie in dieser Methode den EnableDefaultBundles-Aufruf wie im folgenden Codeausschnitt gezeigt.</span><span class="sxs-lookup"><span data-stu-id="dade5-453">In this method, uncomment the EnableDefaultBundles call as shown in the snippet below.</span></span> <span data-ttu-id="dade5-454">Dies ermöglicht es, zu der eine gebündelte Auflistung von CSS-Dateien in einem Ordner zu verweisen, indem Sie den Pfad für diesen Ordner sowie die &quot;CSS&quot; oder &quot;JS&quot; Suffix.</span><span class="sxs-lookup"><span data-stu-id="dade5-454">This enables us to reference a bundled collection of CSS files in a folder by using the path to that folder, plus the &quot;CSS&quot; or the &quot;JS&quot; suffix.</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample11.cs)]
~~~
5. <span data-ttu-id="dade5-455">Öffnen der **Optimization.aspx** Datei, und suchen Sie das Inhaltssteuerelement für **HeadContent**.</span><span class="sxs-lookup"><span data-stu-id="dade5-455">Open the **Optimization.aspx** file and locate the content control for **HeadContent**.</span></span>

    <span data-ttu-id="dade5-456">Beachten Sie die CSS-Dateien und die JS-Dateien, die ein einzelnes referenziertes Tag aufweisen.</span><span class="sxs-lookup"><span data-stu-id="dade5-456">Notice the CSS files and the JS files have a single referenced tag.</span></span>


~~~
[!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample12.aspx)]

> [!NOTE]
> This code is for demo purposes. Ideally, you will reference the bundles in the Site.Master file. In this sample code, you will find that some of the bundled files are also being referenced by the Site.Master file, making this last reference redundant.
~~~
6. <span data-ttu-id="dade5-457">Beachten Sie, dass die Links in die bundling Konventionen verwenden die **Href** Attribut, um alle CSS oder JS-Dateien entnommen werden die Formate und Scripts\custom Ordner bzw.</span><span class="sxs-lookup"><span data-stu-id="dade5-457">Notice that the links are using the bundling conventions in the **href** attribute to get all the CSS or JS files from the Styles and Scripts\custom folder respectively.</span></span>

    <span data-ttu-id="dade5-458">Können Sie den Pfad **Skripts/Custom/JS** wie unten dargestellt, bündeln und verkleinernde die JS-Dateien in einem **Skripts/benutzerdefinierte** Ordner.</span><span class="sxs-lookup"><span data-stu-id="dade5-458">You can use the path **Scripts/custom/JS** as shown below to bundle and minify all the JS files inside a **Scripts/custom** folder.</span></span> <span data-ttu-id="dade5-459">Dies ist das Standardverhalten, mit der Standard-Pakete.</span><span class="sxs-lookup"><span data-stu-id="dade5-459">This is the default behavior with the default bundles.</span></span>


~~~
[!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample13.aspx)]
~~~
7. <span data-ttu-id="dade5-460">Öffnen der **Styles\Site.css** Datei.</span><span class="sxs-lookup"><span data-stu-id="dade5-460">Open the **Styles\Site.css** file.</span></span>

    <span data-ttu-id="dade5-461">Beachten Sie, dass die ursprüngliche CSS-Datei enthält Code eingezogen, Leerzeichen und Kommentare, die die Datei zu vergrößern.</span><span class="sxs-lookup"><span data-stu-id="dade5-461">Notice that the original CSS file contains indented code, blank spaces and comments that enlarge the file.</span></span> <span data-ttu-id="dade5-462">(Enthält auch die JavaScript-Datei Leerzeichen und Kommentare).</span><span class="sxs-lookup"><span data-stu-id="dade5-462">(Also the JavaScript file contains blank spaces and comments).</span></span>

    <span data-ttu-id="dade5-463">![Einer der ursprünglichen CSS-Dateien im Ordner "Skripts"](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "eine der ursprünglichen CSS-Dateien im Ordner "Skripts"")</span><span class="sxs-lookup"><span data-stu-id="dade5-463">![One of the original CSS files in the Scripts folder](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "One of the original CSS files in the Scripts folder")</span></span>

    <span data-ttu-id="dade5-464">*Einer der ursprünglichen CSS-Dateien im Ordner "Skripts"*</span><span class="sxs-lookup"><span data-stu-id="dade5-464">*One of the original CSS files in the Scripts folder*</span></span>
8. <span data-ttu-id="dade5-465">Drücken Sie **F5** , führen Sie die Anwendung, und navigieren zu der **Optimierung** Seite.</span><span class="sxs-lookup"><span data-stu-id="dade5-465">Press **F5** to run the application and navigate to the **Optimization** page.</span></span>
9. <span data-ttu-id="dade5-466">Klicken Sie auf die **CSS-Bundle** Link zum Herunterladen, und öffnen Sie die Datei.</span><span class="sxs-lookup"><span data-stu-id="dade5-466">Click on the **CSS Bundle** link to download and open the file.</span></span>

    <span data-ttu-id="dade5-467">Die verkleinerte gebündelte Datei auschecken.</span><span class="sxs-lookup"><span data-stu-id="dade5-467">Check out the minified bundled file.</span></span> <span data-ttu-id="dade5-468">Beachten Sie, dass alle Leerräume, Kommentare und Einzug Zeichen entfernt wurden, generieren eine kleinere Datei.</span><span class="sxs-lookup"><span data-stu-id="dade5-468">You will notice that all the blank spaces, comments and indentation characters have been removed, generating a smaller file.</span></span>

    <span data-ttu-id="dade5-469">![CSS-Dateien gebündelt](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "gebündelt CSS-Dateien")</span><span class="sxs-lookup"><span data-stu-id="dade5-469">![Bundled CSS files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "Bundled CSS files")</span></span>

    <span data-ttu-id="dade5-470">*Gebündelte CSS-Dateien*</span><span class="sxs-lookup"><span data-stu-id="dade5-470">*Bundled CSS files*</span></span>
10. <span data-ttu-id="dade5-471">Klicken Sie nun auf die **JS-Bundle** Link zum Öffnen der Datei JavaScript gebündelt.</span><span class="sxs-lookup"><span data-stu-id="dade5-471">Now click the **JS Bundle** link to open the JavaScript bundled file.</span></span> <span data-ttu-id="dade5-472">Sie können den Explorer Warnung ignorieren.</span><span class="sxs-lookup"><span data-stu-id="dade5-472">You can safely disregard the explorer warning.</span></span> <span data-ttu-id="dade5-473">Beachten Sie die JavaScript-Dateien unter den **benutzerdefinierte** Ordner auch gebündelt und verkleinert.</span><span class="sxs-lookup"><span data-stu-id="dade5-473">Notice the JavaScript files under the **custom** folder are also bundled and minified.</span></span>

    <span data-ttu-id="dade5-474">![JavaScript-Dateien gebündelt](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "gebündelt JavaScript-Dateien")</span><span class="sxs-lookup"><span data-stu-id="dade5-474">![Bundled JavaScript files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "Bundled JavaScript files")</span></span>

    <span data-ttu-id="dade5-475">*Gebündelte JavaScript-Dateien*</span><span class="sxs-lookup"><span data-stu-id="dade5-475">*Bundled JavaScript files*</span></span>

    <span data-ttu-id="dade5-476">Die Aktivierung der Komprimierung für CSS oder JS-Dateien wurde in früheren ASP.NET-Version wesentlich komplexer.</span><span class="sxs-lookup"><span data-stu-id="dade5-476">Enabling compression for CSS or JS files was much more complicated in previous ASP.NET version.</span></span> <span data-ttu-id="dade5-477">Nun, wie Sie gesehen haben, nur müssen Sie eine Zeile im Hinzufügen der *"Global.asax"* Datei ein, aktivieren die Bündelung und verweisen dann gebündelten Dateien von Ihrem Standort.</span><span class="sxs-lookup"><span data-stu-id="dade5-477">Now, as you have seen, you just need to add one line in the *Global.asax* file to enable bundling, and then reference the bundled files from your site.</span></span>

<a id="Ex4Task3"></a>

<a id="Task_3_-_Static_Bundles"></a>
#### <a name="task-3---static-bundles"></a><span data-ttu-id="dade5-478">Aufgabe 3: statische Pakete</span><span class="sxs-lookup"><span data-stu-id="dade5-478">Task 3 - Static Bundles</span></span>

<span data-ttu-id="dade5-479">Der statische Bundle Ansatz ermöglicht Ihnen die Anpassung des Satz von Dateien zum Paket, den Verweis und die Minimierung-Methode, die verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="dade5-479">The static bundle approach allows you to customize the set of files to bundle, the reference and the minification method that will be used.</span></span>

<span data-ttu-id="dade5-480">In dieser Aufgabe Konfigurieren Sie eine statische Paket um einen bestimmten Satz von Dateien zu bündeln und verkleinernde zu definieren.</span><span class="sxs-lookup"><span data-stu-id="dade5-480">In this task, you will configure a static bundle to define a specific set of files to bundle and minify.</span></span>

1. <span data-ttu-id="dade5-481">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="dade5-481">Close the browser.</span></span>
2. <span data-ttu-id="dade5-482">Öffnen der **Global.asax.cs** Datei, und suchen Sie die **Anwendung\_starten** Methode.</span><span class="sxs-lookup"><span data-stu-id="dade5-482">Open the **Global.asax.cs** file and locate the **Application\_Start** method.</span></span>
3. <span data-ttu-id="dade5-483">Kommentieren Sie den statischen Bundle-Code wie im folgenden Code gezeigt.</span><span class="sxs-lookup"><span data-stu-id="dade5-483">Uncomment the static bundle code as shown in the code below.</span></span>

    <span data-ttu-id="dade5-484">Definieren Sie eine statische Paket, das mit verwiesen wird die &quot; **~/StaticBundle** &quot; virtuellen Pfad und Verwendung **JsMinify** für Minimierung von alle angegebenen Dateien mit der **AddFile** Methode.</span><span class="sxs-lookup"><span data-stu-id="dade5-484">You are defining a static bundle that will be referenced with the &quot;**~/StaticBundle**&quot; virtual path and use **JsMinify** for minification of all the specified files with the **AddFile** method.</span></span> <span data-ttu-id="dade5-485">Schließlich Sie statische Paket zum Hinzufügen der **BundleTable** und aktivieren es.</span><span class="sxs-lookup"><span data-stu-id="dade5-485">Finally, you are adding the static bundle to the **BundleTable** and enabling it.</span></span>

    <span data-ttu-id="dade5-486">Beachten Sie, dass die Dateien nicht am gleichen Ort befinden. Dies ist ein weiterer Vorteil über die standardmäßige bündeln.</span><span class="sxs-lookup"><span data-stu-id="dade5-486">Notice that the files are not located in the same place; this is another advantage over the default bundling.</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample14.cs)]
~~~
4. <span data-ttu-id="dade5-487">Öffnen der **Optimization.aspx** Datei.</span><span class="sxs-lookup"><span data-stu-id="dade5-487">Open the **Optimization.aspx** file.</span></span>

    <span data-ttu-id="dade5-488">Beachten Sie, dass der Link zum **statische JS-Bundle** wird unter Verwendung des Pfads, die bei der Konfiguration der statischen Pakets in der Datei Global.asax.cs deklariert haben: **/StaticBundle**.</span><span class="sxs-lookup"><span data-stu-id="dade5-488">Notice that the link to **Static JS Bundle** is using the path you have declared when you configured the static bundle in the Global.asax.cs file: **/StaticBundle**.</span></span>


~~~
[!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample15.aspx)]
~~~
5. <span data-ttu-id="dade5-489">Drücken Sie **F5** führen Sie die Anwendung, und navigieren Sie zu der **Optimierung** Seite.</span><span class="sxs-lookup"><span data-stu-id="dade5-489">Press **F5** to run the application, and then navigate to the **Optimization** page.</span></span>
6. <span data-ttu-id="dade5-490">Klicken Sie auf die **statische JS-Bundle** Link zum Öffnen der Datei.</span><span class="sxs-lookup"><span data-stu-id="dade5-490">Click on the **Static JS Bundle** link to open the file.</span></span>

    <span data-ttu-id="dade5-491">Beachten Sie, dass der verkleinerte JavaScript-Datei gebündelt ist die Ausgabe für alle JavaScript-Dateien, die in der statischen Bundle-Datei unter dem Pfad konfiguriert &quot;/StaticBundle&quot;.</span><span class="sxs-lookup"><span data-stu-id="dade5-491">Notice that the minified bundled JavaScript file is the output for all the JavaScript files configured in the static bundle file under the path &quot;/StaticBundle&quot;.</span></span>

    <span data-ttu-id="dade5-492">![Statische JavaScript-Dateien Bundle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "statische JavaScript-Dateien-Paket")</span><span class="sxs-lookup"><span data-stu-id="dade5-492">![Static JavaScript files bundle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "Static JavaScript files bundle")</span></span>

    <span data-ttu-id="dade5-493">*Bündeln von statische JavaScript-Dateien*</span><span class="sxs-lookup"><span data-stu-id="dade5-493">*Static JavaScript files bundle*</span></span>
7. <span data-ttu-id="dade5-494">Schließen Sie den Browser, und kehren Sie zu Visual Studio zurück.</span><span class="sxs-lookup"><span data-stu-id="dade5-494">Close the browser and return to Visual Studio.</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_-_Dynamic_Folder_Bundles"></a>
#### <a name="task-4---dynamic-folder-bundles"></a><span data-ttu-id="dade5-495">Aufgabe 4: dynamische Ordner Pakete</span><span class="sxs-lookup"><span data-stu-id="dade5-495">Task 4 - Dynamic Folder Bundles</span></span>

<span data-ttu-id="dade5-496">In dieser Aufgabe lernen Sie das dynamische Ordner Pakete zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="dade5-496">In this task, you will learn how to configure dynamic folder bundles.</span></span> <span data-ttu-id="dade5-497">Die Leistungsfähigkeit von dynamischen bundling besteht, können Sie statische JavaScript als auch andere Dateien enthalten, in Sprachen, die in JavaScript kompiliert wird und daher, erfordern einige verarbeiten, bevor die Bündelung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="dade5-497">The power of dynamic bundling is that you can include static JavaScript, as well as other files in languages that compiles into JavaScript, and thus, require some processing before the bundling is executed.</span></span>

<span data-ttu-id="dade5-498">In diesem Beispiel erfahren Sie, wie Sie die **DynamicFolderBundle** Klasse, um ein dynamisches Paket für CofeeScript-Dateien zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="dade5-498">In this example, you will learn how to use the **DynamicFolderBundle** class to create a dynamic bundle for files written in CofeeScript.</span></span> <span data-ttu-id="dade5-499">CofeeScript ist eine Programmiersprache, die in JavaScript kompiliert und eine einfachere Syntax für das Schreiben von JavaScript-Code, JavaScript Übersichtlichkeit und Lesbarkeit verbessern.</span><span class="sxs-lookup"><span data-stu-id="dade5-499">CofeeScript is a programming language that compiles into JavaScript and provides a simpler syntax for writing JavaScript code, enhancing JavaScript's brevity and readability.</span></span>

1. <span data-ttu-id="dade5-500">Öffnen der **Global.asax.cs** Datei, und suchen Sie die **Anwendung\_starten** Methode.</span><span class="sxs-lookup"><span data-stu-id="dade5-500">Open the **Global.asax.cs** file and locate the **Application\_Start** method.</span></span>
2. <span data-ttu-id="dade5-501">Kommentieren Sie den dynamischen Bundle-Code wie im folgenden Code gezeigt.</span><span class="sxs-lookup"><span data-stu-id="dade5-501">Uncomment the dynamic bundle code as shown in the code below.</span></span>

    <span data-ttu-id="dade5-502">Definieren Sie eine dynamische ordnerpaket, die verwendet werden die **CoffeeMinify** benutzerdefinierte Minimierung-Prozessor, die nur für Dateien mit den gelten die &quot; **.coffee** &quot; (Erweiterung CoffeeScript-Dateien).</span><span class="sxs-lookup"><span data-stu-id="dade5-502">You are defining a dynamic folder bundle that will use the **CoffeeMinify** custom minification processor that will only apply to the files with the &quot;**.coffee**&quot; extension (CoffeeScript files).</span></span> <span data-ttu-id="dade5-503">Beachten Sie, dass Sie die einem Suchmuster, zum Auswählen der Dateien verwenden können, wie z. B. innerhalb eines Ordners zu bündeln "\*.coffee".</span><span class="sxs-lookup"><span data-stu-id="dade5-503">Notice that you can use a search pattern to select the files to bundle within a folder, like '\*.coffee'.</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample16.cs)]
~~~
3. <span data-ttu-id="dade5-504">Öffnen Sie das NuGet-Paket-Manager-Konsole.</span><span class="sxs-lookup"><span data-stu-id="dade5-504">Open the NuGet Package Manager Console.</span></span> <span data-ttu-id="dade5-505">Zu diesem Zweck verwenden Sie das Menü **Ansicht** | **Weitere Fenster** | **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="dade5-505">To do this, use the menu **View** | **Other Windows** | **Package Manager Console**.</span></span>
4. <span data-ttu-id="dade5-506">In der **Paket-Manager-Konsole** Typ **Install-Package CoffeeSharp** , und drücken Sie **EINGABETASTE**.</span><span class="sxs-lookup"><span data-stu-id="dade5-506">In the **Package Manager Console,** type **Install-Package CoffeeSharp** and press **ENTER**.</span></span>
5. <span data-ttu-id="dade5-507">Klicken Sie auf die **alle Dateien anzeigen** Schaltfläche der **Projektmappen-Explorer** Fenster</span><span class="sxs-lookup"><span data-stu-id="dade5-507">Click the **Show All Files** button in the **Solution Explorer** window</span></span>

    <span data-ttu-id="dade5-508">![Anzeigen aller Dateien](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "Anzeigen aller Dateien")</span><span class="sxs-lookup"><span data-stu-id="dade5-508">![Showing all files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "Showing all files")</span></span>

    <span data-ttu-id="dade5-509">*Anzeigen aller Dateien*</span><span class="sxs-lookup"><span data-stu-id="dade5-509">*Showing all files*</span></span>
6. <span data-ttu-id="dade5-510">Klicken Sie mit der rechten Maustaste auf die **CoffeeMinify.cs** in der Datei die **Projektmappen-Explorer** , und wählen Sie **zu Projekt hinzufügen**</span><span class="sxs-lookup"><span data-stu-id="dade5-510">Right click the **CoffeeMinify.cs** file in the **Solution Explorer** and select **Include in Project**</span></span>

    <span data-ttu-id="dade5-511">![Schließen Sie die CoffeeMinify.cs-Datei im Projekt](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "CoffeeMinify.cs-Datei in das Projekt einfügen.")</span><span class="sxs-lookup"><span data-stu-id="dade5-511">![Include the CoffeeMinify.cs file in the project](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "Include the CoffeeMinify.cs file in the project")</span></span>

    <span data-ttu-id="dade5-512">*Die CoffeeMinify.cs-Datei in das Projekt einfügen.*</span><span class="sxs-lookup"><span data-stu-id="dade5-512">*Include the CoffeeMinify.cs file in the project*</span></span>
7. <span data-ttu-id="dade5-513">Öffnen der **CoffeeMinify.cs** Datei.</span><span class="sxs-lookup"><span data-stu-id="dade5-513">Open the **CoffeeMinify.cs** file.</span></span>

    <span data-ttu-id="dade5-514">Diese Klasse erbt von JsMinify zu verkleinernde der JavaScript-Ausgabe von CoffeeScript-Code-Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="dade5-514">This class inherits from JsMinify to minify the JavaScript output resulting from the CoffeeScript code compilation.</span></span> <span data-ttu-id="dade5-515">Ruft den CoffeeScript-Compiler um zunächst den JavaScript-Code zu generieren, und er sendet diese dann an die JsMinify.Process-Methode, die den resultierenden Code verkleinernde.</span><span class="sxs-lookup"><span data-stu-id="dade5-515">It calls the CoffeeScript compiler to generate the JavaScript code first, and then it sends it to the JsMinify.Process method to minify the resulting code.</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample17.cs)]
~~~
8. <span data-ttu-id="dade5-516">Öffnen der **Script1.coffee** und **Script2.coffee** Dateien aus dem **Skripts-Bundle** Ordner.</span><span class="sxs-lookup"><span data-stu-id="dade5-516">Open the **Script1.coffee** and **Script2.coffee** files from the **Scripts/bundle** folder.</span></span>

    <span data-ttu-id="dade5-517">Diese Dateien werden CoffeScript Code kompiliert werden, beim Ausführen der im Lieferumfang der CoffeeMinify-Klasse enthalten.</span><span class="sxs-lookup"><span data-stu-id="dade5-517">These files will include the CoffeScript code to be compiled while performing the bundling with the CoffeeMinify class.</span></span>

    <span data-ttu-id="dade5-518">Aus Gründen der Einfachheit halber werden die bereitgestellten CoffeeScript-Dateien nur CoffeeScript-Beispielcode einschließlich.</span><span class="sxs-lookup"><span data-stu-id="dade5-518">For simplicity purposes, the CoffeeScript files provided are only including CoffeeScript sample code.</span></span> <span data-ttu-id="dade5-519">Die Kommentare werden durch den Prozess JsMinify ausgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="dade5-519">The comments are excluded by the JsMinify process.</span></span>

    <span data-ttu-id="dade5-520">![CoffeeScript-Dateien](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "CoffeeScript-Dateien")</span><span class="sxs-lookup"><span data-stu-id="dade5-520">![CoffeeScript files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "CoffeeScript files")</span></span>

    <span data-ttu-id="dade5-521">*CoffeeScript-Dateien*</span><span class="sxs-lookup"><span data-stu-id="dade5-521">*CoffeeScript files*</span></span>

    > [!NOTE]
    > <span data-ttu-id="dade5-522">[CofeeScript](https://github.com/jashkenas/coffeescript/) bietet eine einfachere Syntax zum Schreiben von JavaScript-Code, JavaScript Übersichtlichkeit und Lesbarkeit verbessern, sowie andere Funktionen wie Mustervergleiche und Array Kennzeichnung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="dade5-522">[CofeeScript](https://github.com/jashkenas/coffeescript/) provides a simpler syntax for writing JavaScript code, enhancing JavaScript's brevity and readability, as well as adding other features like array comprehension and pattern matching.</span></span>
9. <span data-ttu-id="dade5-523">Öffnen der **Optimization.aspx** Datei, und suchen Sie die Paket-Links.</span><span class="sxs-lookup"><span data-stu-id="dade5-523">Open the **Optimization.aspx** file and locate the bundle links.</span></span>

    <span data-ttu-id="dade5-524">Beachten Sie, dass der Link zum **dynamische JS-Bundle** verweist auf die **Skripts-Bundle** Ordner mithilfe der **/Kaffee** Suffix, die Sie für das dynamische ordnerpaket konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="dade5-524">Notice that the link to **Dynamic JS Bundle** is referencing the **Scripts/bundle** folder by using the **/Coffee** suffix you configured for the dynamic folder bundle.</span></span>


~~~
[!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample18.aspx)]
~~~
10. <span data-ttu-id="dade5-525">Drücken Sie **F5** führen Sie die Anwendung, und navigieren Sie zu der **Optimierung** Seite.</span><span class="sxs-lookup"><span data-stu-id="dade5-525">Press **F5** to run the application, and then navigate to the **Optimization** page.</span></span>
11. <span data-ttu-id="dade5-526">Klicken Sie auf die **dynamische JS-Bundle** klicken, um die generierte Datei zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="dade5-526">Click on the **Dynamic JS Bundle** link to open the generated file.</span></span>

    <span data-ttu-id="dade5-527">Beachten Sie, die der Inhalt, der in diesem Paket enthalten war, nur enthält **.coffee** Dateien.</span><span class="sxs-lookup"><span data-stu-id="dade5-527">Notice that the content that was included in this bundle only contains **.coffee** files.</span></span> <span data-ttu-id="dade5-528">Sie können auch sehen, dass der CoffeeScript-Code in JavaScript kompiliert wurde und die auskommentierten Zeilen entfernt wurde.</span><span class="sxs-lookup"><span data-stu-id="dade5-528">You can also see that the CoffeeScript code was compiled to JavaScript and the commented-out lines has been removed.</span></span>

    <span data-ttu-id="dade5-529">![Dynamische JS-Dateien zu bündeln](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "dynamische JS-Dateien-Paket")</span><span class="sxs-lookup"><span data-stu-id="dade5-529">![Dynamic JS files bundle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "Dynamic JS files bundle")</span></span>

    <span data-ttu-id="dade5-530">*Bündeln von dynamische JS-Dateien*</span><span class="sxs-lookup"><span data-stu-id="dade5-530">*Dynamic JS files bundle*</span></span>

> [!NOTE]
> <span data-ttu-id="dade5-531">Darüber hinaus können Sie die Bereitstellung dieser Anwendung, die Windows Azure-Websites folgenden [Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mithilfe von Web Deploy](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="dade5-531">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="dade5-532">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="dade5-532">Summary</span></span>

<span data-ttu-id="dade5-533">Diese Übungseinheit hilft Ihnen zu verstehen, was neu in ASP.NET und Webentwicklung in Visual Studio 2012 ist und wie die Vielzahl von Erweiterungen in Visual Studio 2012 nutzen.</span><span class="sxs-lookup"><span data-stu-id="dade5-533">This lab helps you to understand what New in ASP.NET and Web Development in Visual Studio 2012 is and how to take advantage of the variety of enhancements in Visual Studio 2012.</span></span>

<span data-ttu-id="dade5-534">Durch diese praktische Übungseinheit haben wie die neuen Features und Verbesserungen in Visual Studio 2012-Editoren für CSS, JavaScript und HTML verwenden gelernt werden.</span><span class="sxs-lookup"><span data-stu-id="dade5-534">By completing this Hands-On Lab, you have learnt how to use the new features and improvements in Visual Studio 2012 Editors for CSS, JavaScript and HTML.</span></span> <span data-ttu-id="dade5-535">Darüber hinaus haben Sie gelernt, wie Visual Studio 2012 integrierte Bündelung und Minimierung implementiert.</span><span class="sxs-lookup"><span data-stu-id="dade5-535">In addition, you have learnt how Visual Studio 2012 implements built-in bundling and minification.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="dade5-536">Anhang A: Installieren von Visual Studio Express 2012 für das Web</span><span class="sxs-lookup"><span data-stu-id="dade5-536">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="dade5-537">Sie installieren können **Microsoft Visual Studio Express 2012 für das Web** oder ein anderes &quot;Express&quot; Version mithilfe der **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="dade5-537">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="dade5-538">Die folgenden Anweisungen führen Sie durch die erforderlichen Schritte zum Installieren *Visual Studio Express 2012 für das Web* mit *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="dade5-538">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="dade5-539">Wechseln Sie zu [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="dade5-539">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="dade5-540">Auch wenn Sie bereits Webplattform-Installer installiert haben, können Sie öffnen es, und suchen Sie nach dem Produkt &quot; <em>Visual Studio Express 2012 für das Web mit Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="dade5-540">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="dade5-541">Klicken Sie auf **jetzt installieren**.</span><span class="sxs-lookup"><span data-stu-id="dade5-541">Click on **Install Now**.</span></span> <span data-ttu-id="dade5-542">Wenn Sie keine **Webplattform-Installer** Sie Informationen zum Herunterladen und installieren Sie diese zuerst umgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="dade5-542">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="dade5-543">Einmal **Webplattform-Installer** geöffnet ist, klicken Sie auf **installieren** um das Setup zu starten.</span><span class="sxs-lookup"><span data-stu-id="dade5-543">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="dade5-544">![Visual Studio Express installieren](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "Visual Studio Express installieren")</span><span class="sxs-lookup"><span data-stu-id="dade5-544">![Install Visual Studio Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="dade5-545">*Installieren Sie Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="dade5-545">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="dade5-546">Lesen Sie die Produkte, Lizenzen und Begriffe, und klicken Sie auf **ich stimme** um den Vorgang fortzusetzen.</span><span class="sxs-lookup"><span data-stu-id="dade5-546">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akzeptieren der Lizenzbedingungen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image60.png)

    <span data-ttu-id="dade5-548">*Akzeptieren der Lizenzbedingungen*</span><span class="sxs-lookup"><span data-stu-id="dade5-548">*Accepting the license terms*</span></span>
5. <span data-ttu-id="dade5-549">Warten Sie, bis der Prozess herunterladen und die Installation abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="dade5-549">Wait until the downloading and installation process completes.</span></span>

    ![Installationsstatus](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image61.png)

    <span data-ttu-id="dade5-551">*Installationsstatus*</span><span class="sxs-lookup"><span data-stu-id="dade5-551">*Installation progress*</span></span>
6. <span data-ttu-id="dade5-552">Nach Abschluss der Installation klicken Sie auf **Fertig stellen**.</span><span class="sxs-lookup"><span data-stu-id="dade5-552">When the installation completes, click **Finish**.</span></span>

    ![Installation wurde abgeschlossen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image62.png)

    <span data-ttu-id="dade5-554">*Installation wurde abgeschlossen*</span><span class="sxs-lookup"><span data-stu-id="dade5-554">*Installation completed*</span></span>
7. <span data-ttu-id="dade5-555">Klicken Sie auf **beenden** Webplattform-Installer zu schließen.</span><span class="sxs-lookup"><span data-stu-id="dade5-555">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="dade5-556">Um Visual Studio Express für Web zu öffnen, wechseln Sie zu der **starten** Startseite ein, und starten Sie das Schreiben von &quot; **Visual Studio Express**&quot;, klicken Sie dann auf die **Visual Studio Express für Web** Kachel.</span><span class="sxs-lookup"><span data-stu-id="dade5-556">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Visual Studio Express für Web-Kachel](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image63.png)

    <span data-ttu-id="dade5-558">*Visual Studio Express für Web-Kachel*</span><span class="sxs-lookup"><span data-stu-id="dade5-558">*VS Express for Web tile*</span></span>

* * *

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="dade5-559">Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mithilfe von Web Deploy</span><span class="sxs-lookup"><span data-stu-id="dade5-559">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="dade5-560">In diesem Anhang wird gezeigt, wie eine neue Website aus dem Windows Azure-Verwaltungsportal erstellen und veröffentlichen Sie die Anwendung, die Sie erhalten haben anhand der testumgebung profitieren von der Web Deploy Veröffentlichungsfunktion von Windows Azure bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="dade5-560">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="dade5-561">Aufgabe 1: Erstellen einer neuen Website aus dem Windows Azure-Portal</span><span class="sxs-lookup"><span data-stu-id="dade5-561">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="dade5-562">Wechseln Sie zu der [Windows Azure-Verwaltungsportal](https://manage.windowsazure.com/) und melden Sie sich mit den Microsoft-Anmeldeinformationen, die Ihrem Abonnement zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="dade5-562">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="dade5-563">Sie können mit Windows Azure hosten 10 ASP.NET-Websites kostenlos und skalieren Sie, wenn Ihr Datenverkehr zunimmt.</span><span class="sxs-lookup"><span data-stu-id="dade5-563">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="dade5-564">Sie können registrieren [hier](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="dade5-564">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="dade5-565">![Melden Sie sich bei Windows Azure-Portal](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "melden Sie sich bei Windows Azure-Portal")</span><span class="sxs-lookup"><span data-stu-id="dade5-565">![Log on to Windows Azure portal](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="dade5-566">*Melden Sie sich bei Windows Azure-Verwaltungsportal*</span><span class="sxs-lookup"><span data-stu-id="dade5-566">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="dade5-567">Klicken Sie auf **neu** in der Befehlszeile.</span><span class="sxs-lookup"><span data-stu-id="dade5-567">Click **New** on the command bar.</span></span>

    <span data-ttu-id="dade5-568">![Erstellen einer neuen Website](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "Erstellen einer neuen Website")</span><span class="sxs-lookup"><span data-stu-id="dade5-568">![Creating a new Web Site](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="dade5-569">*Erstellen einer neuen Website*</span><span class="sxs-lookup"><span data-stu-id="dade5-569">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="dade5-570">Klicken Sie auf **berechnen** | **Website**.</span><span class="sxs-lookup"><span data-stu-id="dade5-570">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="dade5-571">Wählen Sie dann **Schnellerfassung** Option.</span><span class="sxs-lookup"><span data-stu-id="dade5-571">Then select **Quick Create** option.</span></span> <span data-ttu-id="dade5-572">Verfügbare URL für die neue Website bereitstellen, und klicken Sie auf **Website erstellen**.</span><span class="sxs-lookup"><span data-stu-id="dade5-572">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="dade5-573">Eine Windows Azure-Website ist der Host für eine Webanwendung, die ausgeführt wird, in der Cloud, die Sie steuern und verwalten können.</span><span class="sxs-lookup"><span data-stu-id="dade5-573">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="dade5-574">Die Option Schnellerfassung können Sie eine vollständige Webanwendung, die Windows Azure-Website von außerhalb des Portals bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="dade5-574">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="dade5-575">Es umfasst nicht die Schritte zum Einrichten einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="dade5-575">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="dade5-576">![Erstellen eine neue Website, die mit der Schnellerfassung](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "erstellen eine neue Website, die mit der Schnellerfassung")</span><span class="sxs-lookup"><span data-stu-id="dade5-576">![Creating a new Web Site using Quick Create](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="dade5-577">*Erstellen eine neue Website, die mit der Schnellerfassung*</span><span class="sxs-lookup"><span data-stu-id="dade5-577">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="dade5-578">Warten Sie, bis die neue **Website** wird erstellt.</span><span class="sxs-lookup"><span data-stu-id="dade5-578">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="dade5-579">Nachdem die Website erstellt wurde, klicken Sie auf den Link unter der **URL** Spalte.</span><span class="sxs-lookup"><span data-stu-id="dade5-579">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="dade5-580">Überprüfen Sie, dass die neue Website funktioniert.</span><span class="sxs-lookup"><span data-stu-id="dade5-580">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="dade5-581">![Um die neue Website durchsuchen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "durchsuchen, um die neue Website")</span><span class="sxs-lookup"><span data-stu-id="dade5-581">![Browsing to the new web site](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="dade5-582">*Um die neue Website durchsuchen*</span><span class="sxs-lookup"><span data-stu-id="dade5-582">*Browsing to the new web site*</span></span>

    <span data-ttu-id="dade5-583">![Website](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "Website ausgeführt wird")</span><span class="sxs-lookup"><span data-stu-id="dade5-583">![Web site running](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "Web site running")</span></span>

    <span data-ttu-id="dade5-584">*Die Website ausgeführt wird*</span><span class="sxs-lookup"><span data-stu-id="dade5-584">*Web site running*</span></span>
6. <span data-ttu-id="dade5-585">Wechseln Sie zurück zum Portal, und klicken Sie auf den Namen der Website unter der **Namen** Spalte die Verwaltungsseiten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="dade5-585">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="dade5-586">![Öffnen die Website-Verwaltungsseiten](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "öffnen die Website-Verwaltungsseiten")</span><span class="sxs-lookup"><span data-stu-id="dade5-586">![Opening the web site management pages](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="dade5-587">*Öffnen die Website-Verwaltungsseiten*</span><span class="sxs-lookup"><span data-stu-id="dade5-587">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="dade5-588">In der **Dashboard** Seite der **kurzer Blick** auf die **Herunterladen eines Veröffentlichungsprofils** Link.</span><span class="sxs-lookup"><span data-stu-id="dade5-588">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="dade5-589">Die *Veröffentlichungsprofil* enthält alle Informationen zum Veröffentlichen einer Webanwendung in einer Windows Azure-Website für die einzelnen aktivierten Veröffentlichungsmethoden erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="dade5-589">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="dade5-590">Das Veröffentlichungsprofil enthält die URLs, Benutzeranmeldeinformationen und datenbankzeichenfolgen, die erforderlich sind, eine Verbindung herstellen und die Authentifizierung für alle Endpunkte für die eine Veröffentlichungsmethode aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="dade5-590">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="dade5-591">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span><span class="sxs-lookup"><span data-stu-id="dade5-591">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="dade5-592">![Herunterladen der Website-Veröffentlichungsprofil](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "der Website herunterladen eines Veröffentlichungsprofils")</span><span class="sxs-lookup"><span data-stu-id="dade5-592">![Downloading the web site publish profile](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="dade5-593">*Die Website herunterladen eines Veröffentlichungsprofils*</span><span class="sxs-lookup"><span data-stu-id="dade5-593">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="dade5-594">Laden Sie das Veröffentlichungsprofil an einem bekannten Speicherort ein.</span><span class="sxs-lookup"><span data-stu-id="dade5-594">Download the publish profile file to a known location.</span></span> <span data-ttu-id="dade5-595">In dieser Übung wird weiter Gewusst wie: Verwenden Sie diese Datei zum Veröffentlichen einer Webanwendung auf einem Windows Azure-Websites in Visual Studio angezeigt.</span><span class="sxs-lookup"><span data-stu-id="dade5-595">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="dade5-596">![Speichern das Veröffentlichungsprofil](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "speichern das Veröffentlichungsprofil")</span><span class="sxs-lookup"><span data-stu-id="dade5-596">![Saving the publish profile file](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "Saving the publish profile")</span></span>

    <span data-ttu-id="dade5-597">*Speichern das Veröffentlichungsprofil*</span><span class="sxs-lookup"><span data-stu-id="dade5-597">*Saving the publish profile file*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="dade5-598">Aufgabe 2: konfigurieren den Datenbankserver</span><span class="sxs-lookup"><span data-stu-id="dade5-598">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="dade5-599">Wenn die Anwendung durchführt, verwenden Sie SQL Server Datenbanken Sie einen SQL-Datenbankserver erstellen müssen.</span><span class="sxs-lookup"><span data-stu-id="dade5-599">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="dade5-600">Wenn eine einfache Anwendung bereitstellen, die keine SQL Server verwendet werden sollen, können Sie diese Aufgabe überspringen.</span><span class="sxs-lookup"><span data-stu-id="dade5-600">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="dade5-601">Sie benötigen einen SQL-Datenbankserver zum Speichern der Anwendungsdatenbank.</span><span class="sxs-lookup"><span data-stu-id="dade5-601">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="dade5-602">Sehen Sie die SQL-Datenbankservern über Ihr Abonnement in Windows Azure-Verwaltungsportal am **Sql-Datenbanken** | **Server** | **des Servers Dashboard**.</span><span class="sxs-lookup"><span data-stu-id="dade5-602">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="dade5-603">Wenn Sie keinen Server erstellt haben, können Sie erstellen, mit der **hinzufügen** auf der Befehlsleiste auf die Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="dade5-603">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="dade5-604">Notieren Sie die **Servername und -URL, Administrator-Benutzername und Kennwort**, wie Sie sie in den nächsten Aufgaben verwenden.</span><span class="sxs-lookup"><span data-stu-id="dade5-604">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="dade5-605">Erstellen Sie die Datenbank nicht noch, wie er in einer späteren Phase erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="dade5-605">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="dade5-606">![SQL-Datenbank-Dashboard](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "SQL-Datenbank-Dashboard")</span><span class="sxs-lookup"><span data-stu-id="dade5-606">![SQL Database Server Dashboard](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="dade5-607">*SQL-Datenbank-Dashboard*</span><span class="sxs-lookup"><span data-stu-id="dade5-607">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="dade5-608">In der nächsten Aufgabe testen Sie die Verbindung mit der Datenbank, aus Visual Studio aus diesem Grund Sie die lokale IP-Adresse in der Serverliste des müssen **zulässigen IP-Adressen**.</span><span class="sxs-lookup"><span data-stu-id="dade5-608">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="dade5-609">Klicken Sie hierzu auf **konfigurieren**, wählen Sie die IP-Adresse von **aktuelle Client-IP-Adresse** und fügen Sie ihn auf die **IP-Startadresse** und **IP-Endadresse** Textfelder.</span><span class="sxs-lookup"><span data-stu-id="dade5-609">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes.</span></span> <span data-ttu-id="dade5-610">Geben Sie einen Namen für die Regel, und klicken Sie auf die ![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png) Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="dade5-610">Enter a name for the rule and click the ![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png) button.</span></span>

    ![Client-IP-Adresse hinzufügen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image74.png)

    <span data-ttu-id="dade5-612">*Client-IP-Adresse hinzufügen*</span><span class="sxs-lookup"><span data-stu-id="dade5-612">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="dade5-613">Einmal die **IP-Clientadresse** wird zu den zulässigen IP-Adressen hinzugefügt Liste, klicken Sie auf **speichern** um die Änderungen zu bestätigen.</span><span class="sxs-lookup"><span data-stu-id="dade5-613">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Bestätigen von Änderungen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image75.png)

    <span data-ttu-id="dade5-615">*Bestätigen von Änderungen*</span><span class="sxs-lookup"><span data-stu-id="dade5-615">*Confirm Changes*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="dade5-616">Aufgabe 3: Veröffentlichen einer ASP.NET MVC 4-Anwendung mithilfe von Web Deploy</span><span class="sxs-lookup"><span data-stu-id="dade5-616">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="dade5-617">Wechseln Sie zurück, die ASP.NET MVC 4-Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="dade5-617">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="dade5-618">In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Websiteprojekt, und wählen Sie **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="dade5-618">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="dade5-619">![Veröffentlichen der Anwendung](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "Veröffentlichen der Anwendung")</span><span class="sxs-lookup"><span data-stu-id="dade5-619">![Publishing the Application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "Publishing the Application")</span></span>

    <span data-ttu-id="dade5-620">*Veröffentlichen der Website*</span><span class="sxs-lookup"><span data-stu-id="dade5-620">*Publishing the web site*</span></span>
2. <span data-ttu-id="dade5-621">Importieren Sie das Veröffentlichungsprofil, das Sie in der ersten Aufgabe gespeichert.</span><span class="sxs-lookup"><span data-stu-id="dade5-621">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="dade5-622">![Importieren des Veröffentlichungsprofils](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "Importieren des Veröffentlichungsprofils")</span><span class="sxs-lookup"><span data-stu-id="dade5-622">![Importing the publish profile](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "Importing the publish profile")</span></span>

    <span data-ttu-id="dade5-623">*Importieren Sie das Veröffentlichungsprofil*</span><span class="sxs-lookup"><span data-stu-id="dade5-623">*Importing publish profile*</span></span>
3. <span data-ttu-id="dade5-624">Klicken Sie auf **überprüft, ob Verbindung**.</span><span class="sxs-lookup"><span data-stu-id="dade5-624">Click **Validate Connection**.</span></span> <span data-ttu-id="dade5-625">Klicken Sie nach Abschluss der Überprüfung auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="dade5-625">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="dade5-626">Überprüfung ist beendet, sobald Sie sehen, dass ein grünes Häkchen neben der Schaltfläche "Validate Connection" angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="dade5-626">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="dade5-627">![Überprüfen der Verbindung](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "Überprüfen der Verbindung")</span><span class="sxs-lookup"><span data-stu-id="dade5-627">![Validating connection](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "Validating connection")</span></span>

    <span data-ttu-id="dade5-628">*Überprüfen der Verbindung*</span><span class="sxs-lookup"><span data-stu-id="dade5-628">*Validating connection*</span></span>
4. <span data-ttu-id="dade5-629">In der **Einstellungen** Seite der **Datenbanken** auf die Schaltfläche neben dem Textfeld für die datenbankverbindung (d. h. **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="dade5-629">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="dade5-630">![Web deploy-Konfiguration](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "Web deploy-Konfiguration")</span><span class="sxs-lookup"><span data-stu-id="dade5-630">![Web deploy configuration](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "Web deploy configuration")</span></span>

    <span data-ttu-id="dade5-631">*Web deploy-Konfiguration*</span><span class="sxs-lookup"><span data-stu-id="dade5-631">*Web deploy configuration*</span></span>
5. <span data-ttu-id="dade5-632">Konfigurieren Sie die Verbindung mit der Datenbank wie folgt:</span><span class="sxs-lookup"><span data-stu-id="dade5-632">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="dade5-633">In der **Servernamen** Geben Sie Ihre SQL-Datenbank Server-URL unter Verwendung der *Tcp:* Präfix.</span><span class="sxs-lookup"><span data-stu-id="dade5-633">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="dade5-634">In **Benutzername** Geben Sie Ihre Administrator Serveranmeldenamen an.</span><span class="sxs-lookup"><span data-stu-id="dade5-634">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="dade5-635">In **Kennwort** Geben Sie Ihre Server-Administratorkennwort.</span><span class="sxs-lookup"><span data-stu-id="dade5-635">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="dade5-636">Geben Sie einen neuen Datenbanknamen ein, z. B.: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="dade5-636">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="dade5-637">![Konfigurieren von Zielverbindungszeichenfolge](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "Zielverbindungszeichenfolge konfigurieren")</span><span class="sxs-lookup"><span data-stu-id="dade5-637">![Configuring destination connection string](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="dade5-638">*Konfigurieren von Ziel-Verbindungszeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="dade5-638">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="dade5-639">Klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="dade5-639">Then click **OK**.</span></span> <span data-ttu-id="dade5-640">Bei der Aufforderung zum Erstellen des Datenbank auf **Ja**.</span><span class="sxs-lookup"><span data-stu-id="dade5-640">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="dade5-641">![Erstellen der Datenbank](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "erstellen die Datenbank-Zeichenfolge")</span><span class="sxs-lookup"><span data-stu-id="dade5-641">![Creating the database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "Creating the database string")</span></span>

    <span data-ttu-id="dade5-642">*Erstellen der Datenbank*</span><span class="sxs-lookup"><span data-stu-id="dade5-642">*Creating the database*</span></span>
7. <span data-ttu-id="dade5-643">Die Verbindungszeichenfolge, die Sie verwenden für die Verbindung mit SQL-Datenbank in Windows Azure ist innerhalb der Verbindung standardmäßig Textfeld angezeigt.</span><span class="sxs-lookup"><span data-stu-id="dade5-643">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="dade5-644">Klicken Sie dann auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="dade5-644">Then click **Next**.</span></span>

    <span data-ttu-id="dade5-645">![Verbindungszeichenfolge zur SQL-Datenbank zeigt](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "Verbindungszeichenfolge zur SQL-Datenbank zeigt")</span><span class="sxs-lookup"><span data-stu-id="dade5-645">![Connection string pointing to SQL Database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="dade5-646">*Verbindungszeichenfolge, die auf der SQL-Datenbank*</span><span class="sxs-lookup"><span data-stu-id="dade5-646">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="dade5-647">In der **Vorschau** auf **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="dade5-647">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="dade5-648">![Veröffentlichen der Webanwendung](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "Veröffentlichen der Webanwendung")</span><span class="sxs-lookup"><span data-stu-id="dade5-648">![Publishing the web application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "Publishing the web application")</span></span>

    <span data-ttu-id="dade5-649">*Veröffentlichen der Webanwendung*</span><span class="sxs-lookup"><span data-stu-id="dade5-649">*Publishing the web application*</span></span>
9. <span data-ttu-id="dade5-650">Sobald der Veröffentlichungsprozess abgeschlossen ist, wird der Standardbrowser veröffentlichten Website geöffnet.</span><span class="sxs-lookup"><span data-stu-id="dade5-650">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="dade5-651">![Anwendung in Windows Azure veröffentlicht](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "Veröffentlichung der Anwendung in Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="dade5-651">![Application published to Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="dade5-652">*Anwendung, die auf Windows Azure veröffentlicht werden soll*</span><span class="sxs-lookup"><span data-stu-id="dade5-652">*Application published to Windows Azure*</span></span>
