---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: 'Praxisnahe Übung: Visual Studio 2013-Webtools | Microsoft-Dokumentation'
author: rick-anderson
description: Visual Studio ist eine ausgezeichnete Entwicklungsumgebung für. NET-basierten Windows und Webprojekte. Es enthält einen leistungsfähige Text-Editor ein, der auf einfache Weise verwendet werden kann...
ms.author: aspnetcontent
ms.date: 07/16/2014
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: 9553d3129ff4c942eacbba628d1daaf6c4e33635
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807527"
---
<a name="hands-on-lab-visual-studio-2013-web-tools"></a><span data-ttu-id="724ab-104">Praxisnahe Übung: Visual Studio 2013-Webtools</span><span class="sxs-lookup"><span data-stu-id="724ab-104">Hands On Lab: Visual Studio 2013 Web Tools</span></span>
====================
<span data-ttu-id="724ab-105">durch [Web Camps Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="724ab-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="724ab-106">Herunterladen Sie Web Camps Training Kit</span><span class="sxs-lookup"><span data-stu-id="724ab-106">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="724ab-107">Visual Studio ist eine ausgezeichnete Entwicklungsumgebung für. NET-basierten Windows und Webprojekte.</span><span class="sxs-lookup"><span data-stu-id="724ab-107">Visual Studio is an excellent development environment for .NET-based Windows and web projects.</span></span> <span data-ttu-id="724ab-108">Sie enthält einen leistungsfähige Text-Editor ein, der leicht verwendet werden kann, um eigenständige Dateien ohne ein Projekt bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="724ab-108">It includes a powerful text editor that can easily be used to edit standalone files without a project.</span></span>
> 
> <span data-ttu-id="724ab-109">Visual Studio verwaltet eine voll funktionsfähige Analysestruktur an, wie Sie jede Datei bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="724ab-109">Visual Studio maintains a full-featured parse tree as you edit each file.</span></span> <span data-ttu-id="724ab-110">Dadurch können Visual Studio, um eine beispiellose automatische Vervollständigung und dokumentbasierte Aktionen bieten und gleichzeitig die Entwicklung schneller und problemloser.</span><span class="sxs-lookup"><span data-stu-id="724ab-110">This allows Visual Studio to provide unparalleled auto-completion and document-based actions while making the development experience much faster and more pleasant.</span></span> <span data-ttu-id="724ab-111">Diese Funktionen sind besonders interessant, in HTML und CSS-Dokumenten.</span><span class="sxs-lookup"><span data-stu-id="724ab-111">These features are especially powerful in HTML and CSS documents.</span></span>
> 
> <span data-ttu-id="724ab-112">Alle von dieser Leistung steht auch für Erweiterungen, die Editoren mit leistungsfähigen neuen Features entsprechend Ihren Anforderungen erweitern erleichtert.</span><span class="sxs-lookup"><span data-stu-id="724ab-112">All of this power is also available for extensions, making it simple to extend the editors with powerful new features to suit your needs.</span></span> <span data-ttu-id="724ab-113">Web Essentials ist eine Auflistung von (hauptsächlich) webbezogene Verbesserungen in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="724ab-113">Web Essentials is a collection of (mostly) web-related enhancements to Visual Studio.</span></span> <span data-ttu-id="724ab-114">Er enthält viele neue IntelliSense-vervollständigungen (insbesondere bei CSS), neuen Features von Browser Link, automatische JSHint für JavaScript-Dateien, neue Warnungen für HTML, CSS und viele weitere Features, die die moderne Webentwicklung unverzichtbar sind.</span><span class="sxs-lookup"><span data-stu-id="724ab-114">It includes lots of new IntelliSense completions (especially for CSS), new Browser Link features, automatic JSHint for JavaScript files, new warnings for HTML and CSS, and many other features that are essential to modern web development.</span></span>
> 
> <span data-ttu-id="724ab-115">Alle Beispielcode und Ausschnitte sind im Web Camps Training Kit unter enthalten [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="724ab-115">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="724ab-116">Übersicht</span><span class="sxs-lookup"><span data-stu-id="724ab-116">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="724ab-117">Ziele</span><span class="sxs-lookup"><span data-stu-id="724ab-117">Objectives</span></span>

<span data-ttu-id="724ab-118">In dieser praktischen Übungseinheit erfahren Sie, wie Sie:</span><span class="sxs-lookup"><span data-stu-id="724ab-118">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="724ab-119">Verwenden Sie die neuen HTML-Editor-Features in Web Essentials z. B. rich-HTML5-Codeausschnitte und Zen-Codierung</span><span class="sxs-lookup"><span data-stu-id="724ab-119">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="724ab-120">Verwenden Sie die neue CSS-Editor-Funktionen, die im Web Essentials enthalten sind, z. B. die Farbauswahl angezeigt und die QuickInfo für Browser-matrix</span><span class="sxs-lookup"><span data-stu-id="724ab-120">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="724ab-121">Verwenden Sie die neue JavaScript-Editor-Funktionen, die im Web Essentials, z. B. extrahieren in die Datei und IntelliSense für alle HTML-Elemente enthalten</span><span class="sxs-lookup"><span data-stu-id="724ab-121">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="724ab-122">Austauschen von Daten zwischen Ihrem Browser, und Visual Studio verwenden einer Browserverknüpfung</span><span class="sxs-lookup"><span data-stu-id="724ab-122">Exchange data between your browser and Visual Studio using Browser Link</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="724ab-123">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="724ab-123">Prerequisites</span></span>

<span data-ttu-id="724ab-124">Folgendes ist erforderlich, um diese praktische Übungseinheit auszuführen:</span><span class="sxs-lookup"><span data-stu-id="724ab-124">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="724ab-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) oder höher</span><span class="sxs-lookup"><span data-stu-id="724ab-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="724ab-126">Web Essentials 2013</span><span class="sxs-lookup"><span data-stu-id="724ab-126">Web Essentials 2013</span></span>](http://vswebessentials.com/)
- [<span data-ttu-id="724ab-127">Google Chrome</span><span class="sxs-lookup"><span data-stu-id="724ab-127">Google Chrome</span></span>](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="724ab-128">Setup</span><span class="sxs-lookup"><span data-stu-id="724ab-128">Setup</span></span>

<span data-ttu-id="724ab-129">Um die Übungen in dieser praktischen Übungseinheit auszuführen, müssen Sie zunächst die Umgebung einrichten.</span><span class="sxs-lookup"><span data-stu-id="724ab-129">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="724ab-130">Öffnen Sie ein Windows-Explorer-Fenster, und navigieren Sie zu des Labs die **Quelle** Ordner.</span><span class="sxs-lookup"><span data-stu-id="724ab-130">Open a Windows Explorer window and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="724ab-131">Mit der rechten Maustaste **"Setup.cmd"** , und wählen Sie **als Administrator ausführen** zum Starten des Setup-Prozesses, die die Umgebung konfigurieren, und installieren die Visual Studio-Codeausschnitte für diese Übung.</span><span class="sxs-lookup"><span data-stu-id="724ab-131">Right-click **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="724ab-132">Wenn das Dialogfeld "Benutzerkontensteuerung" angezeigt wird, bestätigen Sie die Aktion aus, um den Vorgang fortzusetzen.</span><span class="sxs-lookup"><span data-stu-id="724ab-132">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="724ab-133">Stellen Sie sicher, dass Sie alle Abhängigkeiten für diese laborumgebung aktiviert haben, bevor Sie das Setup ausführen.</span><span class="sxs-lookup"><span data-stu-id="724ab-133">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="724ab-134">Verwenden von Codeausschnitten</span><span class="sxs-lookup"><span data-stu-id="724ab-134">Using the Code Snippets</span></span>

<span data-ttu-id="724ab-135">In diesem Dokument Lab werden Sie aufgefordert, zum Einfügen von Codeblöcken.</span><span class="sxs-lookup"><span data-stu-id="724ab-135">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="724ab-136">Der Einfachheit halber die meisten dieser Code dient als Visual Studio-Codeausschnitten, die Sie in Visual Studio 2013, um zu vermeiden, müssen sie manuell hinzufügen aus zugreifen können.</span><span class="sxs-lookup"><span data-stu-id="724ab-136">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="724ab-137">Jede Übung umfasst eine ab Lösung befindet sich in der **beginnen** Ordner der Übung, mit dem Sie jede Übung unabhängig von den anderen verfolgen kann.</span><span class="sxs-lookup"><span data-stu-id="724ab-137">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="724ab-138">Bedenken Sie bitte, dass die Codeausschnitte, die während der Übung hinzugefügt werden fehlen aus diesen Lösungen ab und funktioniert möglicherweise nicht, bis Sie in dieser Übung abgeschlossen haben.</span><span class="sxs-lookup"><span data-stu-id="724ab-138">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="724ab-139">In den Quellcode für eine Übung, finden Sie auch eine **End** Ordner, der Visual Studio-Projektmappe mit dem Code, die aus der Schritte in der entsprechenden Übung enthält.</span><span class="sxs-lookup"><span data-stu-id="724ab-139">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="724ab-140">Sie können diese Lösungen als Leitfaden verwenden, wenn Sie zusätzliche Hilfe benötigen, wie Sie mithilfe dieser praktischen Übungseinheit arbeiten.</span><span class="sxs-lookup"><span data-stu-id="724ab-140">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="724ab-141">Übungen</span><span class="sxs-lookup"><span data-stu-id="724ab-141">Exercises</span></span>

<span data-ttu-id="724ab-142">Dieser praktischen Übungseinheit enthält die folgenden Übungen:</span><span class="sxs-lookup"><span data-stu-id="724ab-142">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="724ab-143">Verwenden von Browserlink und die Web Essentials</span><span class="sxs-lookup"><span data-stu-id="724ab-143">Working with Browser Link and Web Essentials</span></span>](#Exercise1)
2. [<span data-ttu-id="724ab-144">Nutzung von Codeausschnitten und IntelliSense</span><span class="sxs-lookup"><span data-stu-id="724ab-144">Taking Advantage of Code Snippets and IntelliSense</span></span>](#Exercise2)

> [!NOTE]
> <span data-ttu-id="724ab-145">Wenn Sie Visual Studio zum ersten Mal starten, müssen Sie eine der vordefinierten Einstellungen Sammlungen auswählen.</span><span class="sxs-lookup"><span data-stu-id="724ab-145">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="724ab-146">Jede vordefinierte Sammlung dient einem bestimmten Entwicklungsstil und bestimmt, Fensterlayouts, Editor-Verhalten, IntelliSense-Codeausschnitte und Dialogfeld "Optionen".</span><span class="sxs-lookup"><span data-stu-id="724ab-146">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="724ab-147">In dieser Übung wird beschrieben, die erforderlichen Aktionen zum Ausführen der jeweiligen Aufgabe in Visual Studio bei Verwendung der **allgemeine Entwicklungseinstellungen** Auflistung.</span><span class="sxs-lookup"><span data-stu-id="724ab-147">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="724ab-148">Wenn Sie eine Sammlung mit anderen Einstellungen für Ihre Entwicklungsumgebung auswählen, unter Umständen gibt es bestehen Unterschiede in den Schritten, die Sie berücksichtigen sollten.</span><span class="sxs-lookup"><span data-stu-id="724ab-148">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a><span data-ttu-id="724ab-149">Übung 1: Verwendung von Browserlink und die Web Essentials</span><span class="sxs-lookup"><span data-stu-id="724ab-149">Exercise 1: Working with Browser Link and Web Essentials</span></span>

<span data-ttu-id="724ab-150">**Web Essentials** ist eine Visual Studio-Erweiterung, die eine Vielzahl von nützlichen Features für die moderne Webentwicklung, vor allem darauf konzentriert, die die webentwicklungserfahrung viel schneller und problemloser hinzufügt.</span><span class="sxs-lookup"><span data-stu-id="724ab-150">**Web Essentials** is a Visual Studio extension that adds a variety of useful features for modern web development, mostly focused on making the web development experience much faster and more pleasant.</span></span> <span data-ttu-id="724ab-151">Sie können die Web Essentials aus dem Katalog der Erweiterung in Visual Studio installieren.</span><span class="sxs-lookup"><span data-stu-id="724ab-151">You can install Web Essentials from the Extension Gallery in Visual Studio.</span></span>

<span data-ttu-id="724ab-152">**Browserverknüpfung** ist ein neues Feature in Visual Studio 2013, die einen Kanal zwischen Visual Studio-IDE und jedem geöffneten Browser zum Austauschen von Daten zwischen der Webanwendung und die Visual Studio bietet enthalten.</span><span class="sxs-lookup"><span data-stu-id="724ab-152">**Browser Link** is a new feature included in Visual Studio 2013 that provides a channel between the Visual Studio IDE and any open browser to exchange data between your web application and Visual Studio.</span></span> <span data-ttu-id="724ab-153">Web Essentials erweitert Browserlink mit Tools zum Bearbeiten des DOM-Objekt und die CSS-Formate von Webseiten direkt über den Browser.</span><span class="sxs-lookup"><span data-stu-id="724ab-153">Web Essentials extends Browser Link with tools to manipulate the DOM object model and the CSS styles of your web pages directly from the browser.</span></span>

<span data-ttu-id="724ab-154">In dieser Übung werden Sie lernen Sie einige der vom unterstützten Funktionen **Web Essentials** und **Browserlink** um eine einfache Quiz-Seite zu verbessern.</span><span class="sxs-lookup"><span data-stu-id="724ab-154">In this exercise, you will explore some of the features supported by **Web Essentials** and **Browser Link** to enhance a simple quiz page.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a><span data-ttu-id="724ab-155">Aufgabe 1: Ausführen des Projekts in mehreren Browsern</span><span class="sxs-lookup"><span data-stu-id="724ab-155">Task 1 - Running the Project in Multiple Browsers</span></span>

<span data-ttu-id="724ab-156">In dieser Aufgabe Konfigurieren Sie Ihre Webanwendung in mehreren Browsern gleichzeitig ausführen Dies ist hilfreich für browserübergreifende Tests.</span><span class="sxs-lookup"><span data-stu-id="724ab-156">In this task, you will configure your web application to run in multiple browsers at once, which is useful for cross-browser testing.</span></span>

1. <span data-ttu-id="724ab-157">Open **Microsoft Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="724ab-157">Open **Microsoft Visual Studio**.</span></span>
2. <span data-ttu-id="724ab-158">In der **Datei** , wählen Sie im Menü **Open | Projekt/Projektmappe...**  und navigieren Sie zu **Ex1-WorkingwithBrowserLinkandWebEssentials\Begin** in die **Quelle** Ordner der Umgebung (C:\WebCampsTK\HOL\VSWebTooling\Source).</span><span class="sxs-lookup"><span data-stu-id="724ab-158">In the **File** menu, select **Open | Project/Solution...** and browse to **Ex1-WorkingwithBrowserLinkandWebEssentials\Begin** in the **Source** folder of the lab (C:\WebCampsTK\HOL\VSWebTooling\Source).</span></span> <span data-ttu-id="724ab-159">Wählen Sie **Begin.sln** , und klicken Sie auf **öffnen**.</span><span class="sxs-lookup"><span data-stu-id="724ab-159">Select **Begin.sln** and click **Open**.</span></span>
3. <span data-ttu-id="724ab-160">Erweitern Sie in der Symbolleiste von Visual Studio das Menü "Browser"- **Browserauswahl...** .</span><span class="sxs-lookup"><span data-stu-id="724ab-160">In the Visual Studio toolbar, expand the browser menu and select **Browse With...**.</span></span>

    <span data-ttu-id="724ab-161">![Navigieren Sie mit der Menüoption](visual-studio-2013-web-tools/_static/image1.png "durchsuchen mit... im \"Objektkatalog\"")</span><span class="sxs-lookup"><span data-stu-id="724ab-161">![Browse With menu option](visual-studio-2013-web-tools/_static/image1.png "Browse with... in browser menu")</span></span>

    <span data-ttu-id="724ab-162">*Navigieren Sie mit der Menüoption*</span><span class="sxs-lookup"><span data-stu-id="724ab-162">*Browse With menu option*</span></span>
4. <span data-ttu-id="724ab-163">In der **Browserauswahl** (Dialogfeld), wählen Sie **Google Chrome** und **Internet Explorer** halten die **STRG** gedrückt, und klicken Sie auf  **Als Standard festlegen**.</span><span class="sxs-lookup"><span data-stu-id="724ab-163">In the **Browse With** dialog box, select both **Google Chrome** and **Internet Explorer** by holding down the **CTRL** key and click **Set as Default**.</span></span>

    <span data-ttu-id="724ab-164">![Navigieren Sie im Dialogfeld](visual-studio-2013-web-tools/_static/image2.png "navigieren Sie im Dialogfeld")</span><span class="sxs-lookup"><span data-stu-id="724ab-164">![Browse with dialog box](visual-studio-2013-web-tools/_static/image2.png "Browse with dialog box")</span></span>

    <span data-ttu-id="724ab-165">*Mehrere Standardbrowser auswählen*</span><span class="sxs-lookup"><span data-stu-id="724ab-165">*Selecting multiple default browsers*</span></span>
5. <span data-ttu-id="724ab-166">Sowohl für Google Chrome als auch für Internet Explorer sollte nun wie der Standardbrowser angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="724ab-166">Both Google Chrome and Internet Explorer should now appear as the default browsers.</span></span> <span data-ttu-id="724ab-167">Klicken Sie auf **Abbrechen** um das Dialogfeld zu schließen.</span><span class="sxs-lookup"><span data-stu-id="724ab-167">Click **Cancel** to close the dialog box.</span></span>

    <span data-ttu-id="724ab-168">![Google Chrome und Internet Explorer als Standardbrowser](visual-studio-2013-web-tools/_static/image3.png "Google Chrome und Internet Explorer Standardbrowser")</span><span class="sxs-lookup"><span data-stu-id="724ab-168">![Google Chrome and Internet Explorer as default browsers](visual-studio-2013-web-tools/_static/image3.png "Google Chrome and Internet Explorer default browsers")</span></span>

    <span data-ttu-id="724ab-169">*Google Chrome und Internet Explorer als Standardbrowser*</span><span class="sxs-lookup"><span data-stu-id="724ab-169">*Google Chrome and Internet Explorer as default browsers*</span></span>

    > [!NOTE]
    > <span data-ttu-id="724ab-170">Nach dem Konfigurieren der Standardbrowser der **mehreren Browsern** Option im Menü "Browser" ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="724ab-170">After configuring the default browsers, the **Multiple Browsers** option is selected in the browser menu.</span></span>
    > 
    > <span data-ttu-id="724ab-171">![Mehrere Browser](visual-studio-2013-web-tools/_static/image4.png "mehrere Browser")</span><span class="sxs-lookup"><span data-stu-id="724ab-171">![Multiple browsers](visual-studio-2013-web-tools/_static/image4.png "Multiple browsers")</span></span>
6. <span data-ttu-id="724ab-172">Drücken Sie **STRG** + **F5** um die Anwendung ohne Debuggen auszuführen.</span><span class="sxs-lookup"><span data-stu-id="724ab-172">Press **CTRL** + **F5** to run the application without debugging.</span></span>
7. <span data-ttu-id="724ab-173">Wenn beide Browserfenster öffnen, müssen platzieren Sie eine von ihnen über die anderen, um die Updates gleichzeitig auf beide Browser anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="724ab-173">When both browser windows open, place one of them above the other in order to see the updates on both browsers simultaneously.</span></span> <span data-ttu-id="724ab-174">Der Browser sollte eine Webseite mit einem hellblaue Rechteck angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="724ab-174">The browsers should display a web page with a light-blue rectangle.</span></span>

    <span data-ttu-id="724ab-175">![Platzieren einen Browser über der anderen](visual-studio-2013-web-tools/_static/image5.png "Platzieren von einem Browser über der anderen")</span><span class="sxs-lookup"><span data-stu-id="724ab-175">![Placing one browser above the other](visual-studio-2013-web-tools/_static/image5.png "Placing one browser above the other")</span></span>

    <span data-ttu-id="724ab-176">*Platzieren einen Browser über der anderen*</span><span class="sxs-lookup"><span data-stu-id="724ab-176">*Placing one browser above the other*</span></span>
8. <span data-ttu-id="724ab-177">Schließen Sie den Browser nicht.</span><span class="sxs-lookup"><span data-stu-id="724ab-177">Do not close the browsers.</span></span> <span data-ttu-id="724ab-178">Sie verwenden diese in der nächsten Aufgabe.</span><span class="sxs-lookup"><span data-stu-id="724ab-178">You will use them in the next task.</span></span>

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a><span data-ttu-id="724ab-179">Aufgabe 2: Verwendung Zen-Codierung zum Erstellen von HTML-Elementen</span><span class="sxs-lookup"><span data-stu-id="724ab-179">Task 2 - Using Zen Coding to Create HTML Elements</span></span>

<span data-ttu-id="724ab-180">**Codieren von Zen** ist Sie ein Editor-Plug-In für Hochgeschwindigkeits-HTML, XML, XSL-(oder jedes andere strukturiertem Code-Format) zu programmieren und zu bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="724ab-180">**Zen Coding** is an editor plugin for high-speed HTML, XML, XSL (or any other structured code format) coding and editing.</span></span> <span data-ttu-id="724ab-181">Der Kern der dieses Plug-in ist eine leistungsstarke Abkürzung-Engine – ähnlich wie CSS-Selektoren - Ausdrücke, in HTML-Code erweitern können.</span><span class="sxs-lookup"><span data-stu-id="724ab-181">The core of this plugin is a powerful abbreviation engine which allows you to expand expressions -similar to CSS selectors- into HTML code.</span></span> <span data-ttu-id="724ab-182">Zen-Codierung ist eine schnelle Möglichkeit zum Schreiben, dass HTML mithilfe einer CSS-Selektor-Syntax formatieren.</span><span class="sxs-lookup"><span data-stu-id="724ab-182">Zen Coding is a fast way to write HTML using a CSS style selector syntax.</span></span>

<span data-ttu-id="724ab-183">In dieser Übung verwenden Sie die Codierung Zen-Funktion von Web Essentials bereitgestellt, die HTML-Schaltflächen generieren, die die Optionen der Frage darstellt.</span><span class="sxs-lookup"><span data-stu-id="724ab-183">In this exercise, you will use the Zen Coding feature provided by Web Essentials to generate the HTML buttons that represent the options of the question.</span></span>

1. <span data-ttu-id="724ab-184">Wechseln Sie zurück zu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="724ab-184">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="724ab-185">Öffnen der **"Index.cshtml"** Datei befindet sich in der **Ansichten** | **Startseite** Ordner.</span><span class="sxs-lookup"><span data-stu-id="724ab-185">Open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="724ab-186">Ersetzen der **&lt;!--TODO: Fügen Sie die Optionen zur Verfügung –&gt;** Kommentar mit dem folgenden Code, und drücken Sie **Registerkarte**.</span><span class="sxs-lookup"><span data-stu-id="724ab-186">Replace the **&lt;!-- TODO: add options here--&gt;** comment with the following code, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. <span data-ttu-id="724ab-187">Der Code sollte in HTML erweitert werden.</span><span class="sxs-lookup"><span data-stu-id="724ab-187">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="724ab-188">![Erweitert HTML](visual-studio-2013-web-tools/_static/image6.png "erweitert HTML")</span><span class="sxs-lookup"><span data-stu-id="724ab-188">![Expanded HTML](visual-studio-2013-web-tools/_static/image6.png "Expanded HTML")</span></span>

    <span data-ttu-id="724ab-189">*Erweiterte HTML*</span><span class="sxs-lookup"><span data-stu-id="724ab-189">*Expanded HTML*</span></span>

    > [!NOTE]
    > <span data-ttu-id="724ab-190">Weitere Informationen zur Codierung von Zen-Syntax finden Sie unter den folgenden [Artikel](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).</span><span class="sxs-lookup"><span data-stu-id="724ab-190">To learn more about Zen Coding syntax, see the following [article](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).</span></span>
5. <span data-ttu-id="724ab-191">Klicken Sie auf die **verknüpfte Browser aktualisieren** Schaltfläche, um beide Browser zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="724ab-191">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="724ab-192">![Verknüpfte Browser aktualisieren](visual-studio-2013-web-tools/_static/image7.png "verknüpfte Browser aktualisieren")</span><span class="sxs-lookup"><span data-stu-id="724ab-192">![Refresh linked browsers](visual-studio-2013-web-tools/_static/image7.png "Refresh linked browsers")</span></span>

    <span data-ttu-id="724ab-193">*Verknüpfte Browser aktualisieren*</span><span class="sxs-lookup"><span data-stu-id="724ab-193">*Refresh linked browsers*</span></span>

    <span data-ttu-id="724ab-194">![InternetExplorer - Seite aktualisiert, mit vier Schaltflächen](visual-studio-2013-web-tools/_static/image8.png "InternetExplorer - Seite aktualisiert, mit vier Schaltflächen")</span><span class="sxs-lookup"><span data-stu-id="724ab-194">![Internet Explorer - Page updated with four buttons](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer - Page updated with four buttons")</span></span>

    <span data-ttu-id="724ab-195">*InternetExplorer - Seite mit vier Schaltflächen aktualisiert*</span><span class="sxs-lookup"><span data-stu-id="724ab-195">*Internet Explorer - Page updated with four buttons*</span></span>

    <span data-ttu-id="724ab-196">![Google Chrome – Seite aktualisiert, mit vier Schaltflächen](visual-studio-2013-web-tools/_static/image9.png "Google Chrome – Seite aktualisiert, mit vier Schaltflächen")</span><span class="sxs-lookup"><span data-stu-id="724ab-196">![Google Chrome - Page updated with four buttons](visual-studio-2013-web-tools/_static/image9.png "Google Chrome - Page updated with four buttons")</span></span>

    <span data-ttu-id="724ab-197">*Google Chrome – aktualisiert die Seite mit vier Schaltflächen*</span><span class="sxs-lookup"><span data-stu-id="724ab-197">*Google Chrome - Page updated with four buttons*</span></span>
6. <span data-ttu-id="724ab-198">Wechseln Sie zurück zu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="724ab-198">Switch back to Visual Studio.</span></span>
7. <span data-ttu-id="724ab-199">Sie haben die Schaltflächen auf der Seite hinzugefügt, jedoch müssen Sie immer noch eine Frage der Vorlage hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="724ab-199">You have added the buttons to the page, but you still need to add a template question.</span></span> <span data-ttu-id="724ab-200">Zu diesem Zweck verwenden Sie ein neues Feature in Web Essentials namens **Lorem Ipsum Generator**.</span><span class="sxs-lookup"><span data-stu-id="724ab-200">To do so, you will use a new feature in Web Essentials called **Lorem Ipsum generator**.</span></span> <span data-ttu-id="724ab-201">Suchen Sie die **Div** -Element mit dem **Klasse** Attribut **Front**.</span><span class="sxs-lookup"><span data-stu-id="724ab-201">Locate the **div** element with the **class** attribute **front**.</span></span>
8. <span data-ttu-id="724ab-202">Fügen Sie den folgenden Code als das erste untergeordnete Element von der **Div**, und drücken Sie die **Registerkarte**.</span><span class="sxs-lookup"><span data-stu-id="724ab-202">Add the following code as the first child element of the **div**, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. <span data-ttu-id="724ab-203">Der Code sollte in HTML erweitert werden.</span><span class="sxs-lookup"><span data-stu-id="724ab-203">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="724ab-204">![Lorem Ipsum automatisch generierte](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum automatisch generiert")</span><span class="sxs-lookup"><span data-stu-id="724ab-204">![Lorem Ipsum autogenerated](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum autogenerated")</span></span>

    <span data-ttu-id="724ab-205">*Lorem Ipsum automatisch generiert*</span><span class="sxs-lookup"><span data-stu-id="724ab-205">*Lorem Ipsum autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="724ab-206">Im Rahmen der Zen-Codierung können Sie jetzt Lorem Ipsum-Code direkt im HTML-Editor generieren.</span><span class="sxs-lookup"><span data-stu-id="724ab-206">As part of Zen Coding, you can now generate Lorem Ipsum code directly in the HTML editor.</span></span> <span data-ttu-id="724ab-207">Geben Sie einfach **Lorem** und klicken Sie auf **Registerkarte** und eine 30 Lorem Ipsum Text eingefügt werden soll.</span><span class="sxs-lookup"><span data-stu-id="724ab-207">Simply type **lorem** and hit **TAB** and a 30 word Lorem Ipsum text will be inserted.</span></span> <span data-ttu-id="724ab-208">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="724ab-208">E.g.</span></span> <span data-ttu-id="724ab-209">*lorem10* fügt 10 Lorem Ipsum-Wörter.</span><span class="sxs-lookup"><span data-stu-id="724ab-209">*lorem10* inserts 10 Lorem Ipsum words.</span></span>
10. <span data-ttu-id="724ab-210">Fügen Sie ein Logo am oberen Rand der Frage ein weiteres neues Feature in Web Essentials aufgerufen mit **Lorem Pixel Generator**.</span><span class="sxs-lookup"><span data-stu-id="724ab-210">You will add a logo at the top of the question by using another new feature in Web Essentials called **Lorem Pixel generator**.</span></span> <span data-ttu-id="724ab-211">Fügen Sie den folgenden Code als das erste untergeordnete Element von der **Div** -Element mit **Container** als **Klasse** Wert ein, und drücken Sie **Registerkarte**.</span><span class="sxs-lookup"><span data-stu-id="724ab-211">Add the following code as the first child element of the **div** element with **container** as **class** value, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. <span data-ttu-id="724ab-212">Der Code sollte in HTML erweitern.</span><span class="sxs-lookup"><span data-stu-id="724ab-212">The code should expand to HTML.</span></span>

    <span data-ttu-id="724ab-213">![Lorem Pixel automatisch generierte](visual-studio-2013-web-tools/_static/image11.png "Lorem Pixel automatisch generiert")</span><span class="sxs-lookup"><span data-stu-id="724ab-213">![Lorem Pixel autogenerated](visual-studio-2013-web-tools/_static/image11.png "Lorem Pixel autogenerated")</span></span>

    <span data-ttu-id="724ab-214">*Lorem Pixel automatisch generiert*</span><span class="sxs-lookup"><span data-stu-id="724ab-214">*Lorem Pixel autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="724ab-215">Im Rahmen der Zen-Codierung können Sie auch Lorem-Pixel-Code direkt im HTML-Editor generieren.</span><span class="sxs-lookup"><span data-stu-id="724ab-215">As part of Zen Coding, you can also generate Lorem Pixel code directly in the HTML editor.</span></span> <span data-ttu-id="724ab-216">Geben Sie einfach **Pix-200 x 200-Tiere** und klicken Sie auf **Registerkarte** und **Img** Tag mit einem 200 x 200-Image von einem Tier eingefügt werden soll.</span><span class="sxs-lookup"><span data-stu-id="724ab-216">Simply type **pix-200x200-animals** and hit **TAB** and an **img** tag with a 200x200 image of an animal will be inserted.</span></span> <span data-ttu-id="724ab-217">Weitere Informationen finden Sie unter [Lorem Pixel](http://www.lorempixel.com).</span><span class="sxs-lookup"><span data-stu-id="724ab-217">For more information, refer to [Lorem Pixel](http://www.lorempixel.com).</span></span>
12. <span data-ttu-id="724ab-218">Klicken Sie auf die **verknüpfte Browser aktualisieren** Schaltfläche, um beide Browser zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="724ab-218">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="724ab-219">![InternetExplorer – automatisch generierte Bild und Text](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer – automatisch generierte Bild und Text")</span><span class="sxs-lookup"><span data-stu-id="724ab-219">![Internet Explorer - Autogenerated image and text](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - Autogenerated image and text")</span></span>

    <span data-ttu-id="724ab-220">*InternetExplorer – automatisch generierte Bild und text*</span><span class="sxs-lookup"><span data-stu-id="724ab-220">*Internet Explorer - Autogenerated image and text*</span></span>

    <span data-ttu-id="724ab-221">![Google Chrome – automatisch generierte Bild und Text](visual-studio-2013-web-tools/_static/image13.png "Google Chrome – automatisch generierte Bild und Text")</span><span class="sxs-lookup"><span data-stu-id="724ab-221">![Google Chrome - Autogenerated image and text](visual-studio-2013-web-tools/_static/image13.png "Google Chrome - Autogenerated image and text")</span></span>

    <span data-ttu-id="724ab-222">*Google Chrome – automatisch generierte Bild und text*</span><span class="sxs-lookup"><span data-stu-id="724ab-222">*Google Chrome - Autogenerated image and text*</span></span>

    > [!NOTE]
    > <span data-ttu-id="724ab-223">Da das Bild nach dem Zufallsprinzip ausgewählt ist, wenn Sie den Codeausschnitt hinzufügen, das Bild in den Browsern unterscheiden sich möglicherweise.</span><span class="sxs-lookup"><span data-stu-id="724ab-223">Because the image is selected randomly when adding the code snippet, the image shown in the browsers may differ.</span></span>
13. <span data-ttu-id="724ab-224">Schließen Sie den Browser nicht.</span><span class="sxs-lookup"><span data-stu-id="724ab-224">Do not close the browsers.</span></span> <span data-ttu-id="724ab-225">Sie verwenden diese in der nächsten Aufgabe.</span><span class="sxs-lookup"><span data-stu-id="724ab-225">You will use them in the next task.</span></span>

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a><span data-ttu-id="724ab-226">Aufgabe 3: Aktualisieren einer Style-Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="724ab-226">Task 3 - Updating a Style Property</span></span>

<span data-ttu-id="724ab-227">In dieser Aufgabe verwenden Sie den Browserlink **Modus überprüfen** Feature zum Erkennen des exakten Speicherort, an dem das bestimmte DOM-Element generiert wird, und aktualisieren Sie dann die Color-Eigenschaft dieses Elements, das über eine Farbauswahl angezeigt, die vom Webdienst bereitgestellt Essentials.</span><span class="sxs-lookup"><span data-stu-id="724ab-227">In this task, you will use the Browser Link's **Inspect Mode** feature to detect the exact location where the specific DOM element is generated and then update the color property of that element using a color picker provided by Web Essentials.</span></span>

1. <span data-ttu-id="724ab-228">Drücken Sie in der Internet Explorer-Browser **STRG** + **ALT** + **ich** Aktivieren des Modus zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="724ab-228">In the Internet Explorer browser, press **CTRL** + **ALT** + **I** to enable Inspect Mode.</span></span>
2. <span data-ttu-id="724ab-229">Zeigen Sie auf den hellen blauen Rahmen, und klicken Sie auf.</span><span class="sxs-lookup"><span data-stu-id="724ab-229">Move the pointer over the light blue border and click.</span></span>

    <span data-ttu-id="724ab-230">![Bewegen des Mauszeigers über den hellen blauen Rahmen](visual-studio-2013-web-tools/_static/image14.png "Bewegen des Mauszeigers über den hellen blauen Rahmen")</span><span class="sxs-lookup"><span data-stu-id="724ab-230">![Moving the pointer over the light blue border](visual-studio-2013-web-tools/_static/image14.png "Moving the pointer over the light blue border")</span></span>

    <span data-ttu-id="724ab-231">*Bewegen des Mauszeigers über den hellen blauen Rahmen*</span><span class="sxs-lookup"><span data-stu-id="724ab-231">*Moving the pointer over the light blue border*</span></span>
3. <span data-ttu-id="724ab-232">Wechseln Sie zurück zu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="724ab-232">Switch back to Visual Studio.</span></span> <span data-ttu-id="724ab-233">Beachten Sie, wie das HTML-Element, das Sie im Browser ausgewählt ebenfalls in der Visual Studio-HTML-Editor aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="724ab-233">Notice how the HTML element that you selected in the browser is also selected in the Visual Studio HTML editor.</span></span>

    <span data-ttu-id="724ab-234">![HTML-Element in der Visual Studio-HTML-Editor ausgewählten](visual-studio-2013-web-tools/_static/image15.png "HTML-Element in der Visual Studio-HTML-Editor ausgewählt")</span><span class="sxs-lookup"><span data-stu-id="724ab-234">![HTML element selected in the Visual Studio HTML editor](visual-studio-2013-web-tools/_static/image15.png "HTML element selected in the Visual Studio HTML editor")</span></span>

    <span data-ttu-id="724ab-235">*HTML-Element in der Visual Studio-HTML-Editor ausgewählt*</span><span class="sxs-lookup"><span data-stu-id="724ab-235">*HTML element selected in the Visual Studio HTML editor*</span></span>
4. <span data-ttu-id="724ab-236">Aktualisieren Sie jetzt die **Front** CSS-Klasse, um den Stil des ausgewählten Elements zu ändern.</span><span class="sxs-lookup"><span data-stu-id="724ab-236">You will now update the **front** CSS class in order to change the styling of the selected element.</span></span> <span data-ttu-id="724ab-237">Drücken Sie die zu diesem Zweck **STRG** + **,** zum Öffnen der **Navigieren zu** Suchfeld.</span><span class="sxs-lookup"><span data-stu-id="724ab-237">To do so, press **CTRL** + **,** to open the **Navigate To** search box.</span></span> <span data-ttu-id="724ab-238">Typ **"Site.CSS"** , und drücken Sie **EINGABETASTE** zum Öffnen der Datei.</span><span class="sxs-lookup"><span data-stu-id="724ab-238">Type **site.css** and press **ENTER** to open the file.</span></span>

    <span data-ttu-id="724ab-239">![Öffnen die Datei "Site.CSS"](visual-studio-2013-web-tools/_static/image16.png "Öffnen der Datei \"Site.CSS\"")</span><span class="sxs-lookup"><span data-stu-id="724ab-239">![Opening file Site.css](visual-studio-2013-web-tools/_static/image16.png "Opening file Site.css")</span></span>

    <span data-ttu-id="724ab-240">*Öffnen der Datei "Site.CSS"*</span><span class="sxs-lookup"><span data-stu-id="724ab-240">*Opening file Site.css*</span></span>
5. <span data-ttu-id="724ab-241">Drücken Sie **STRG** + **F** und **.flip-Containter .front** die CSS-Auswahl gefunden.</span><span class="sxs-lookup"><span data-stu-id="724ab-241">Press **CTRL** + **F** and type **.flip-containter .front** to find the CSS selector.</span></span>
6. <span data-ttu-id="724ab-242">Klicken Sie auf das Licht blaue Quadrat aus, in die Border-Eigenschaft der Klasse, um den Farbwähler zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="724ab-242">Click the light blue square in the border property of the class to open the Color Picker.</span></span>

    <span data-ttu-id="724ab-243">![Öffnen die Farbauswahl](visual-studio-2013-web-tools/_static/image17.png "öffnen die Farbauswahl")</span><span class="sxs-lookup"><span data-stu-id="724ab-243">![Opening the Color Picker](visual-studio-2013-web-tools/_static/image17.png "Opening the Color Picker")</span></span>

    <span data-ttu-id="724ab-244">*Öffnen die Farbauswahl*</span><span class="sxs-lookup"><span data-stu-id="724ab-244">*Opening the Color Picker*</span></span>
7. <span data-ttu-id="724ab-245">Erweitern Sie die Farbauswahl, indem Sie auf die Chevron-Schaltfläche, und wählen Sie eine neue Farbe.</span><span class="sxs-lookup"><span data-stu-id="724ab-245">Expand the Color Picker by clicking the chevron button and select a new color.</span></span>

    <span data-ttu-id="724ab-246">![Erweitern die Farbauswahl](visual-studio-2013-web-tools/_static/image18.png "die Farbauswahl erweitern")</span><span class="sxs-lookup"><span data-stu-id="724ab-246">![Expanding the Color Picker](visual-studio-2013-web-tools/_static/image18.png "Expanding the Color Picker")</span></span>

    <span data-ttu-id="724ab-247">*Die Farbauswahl erweitern*</span><span class="sxs-lookup"><span data-stu-id="724ab-247">*Expanding the Color Picker*</span></span>
8. <span data-ttu-id="724ab-248">Drücken Sie **STRG** + **ALT** + **EINGABETASTE** verknüpften Browser zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="724ab-248">Press **CTRL** + **ALT** + **ENTER** to refresh linked browsers.</span></span>
9. <span data-ttu-id="724ab-249">Wechseln Sie zu Internet Explorer, und beachten Sie, wie sich die Farbe des Rahmens geändert hat.</span><span class="sxs-lookup"><span data-stu-id="724ab-249">Switch to Internet Explorer and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="724ab-250">![InternetExplorer – die Farbe des Rahmens aktualisiert](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer – die Farbe des Rahmens aktualisiert")</span><span class="sxs-lookup"><span data-stu-id="724ab-250">![Internet Explorer - Border color updated](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer - Border color updated")</span></span>

    <span data-ttu-id="724ab-251">*InternetExplorer – die Farbe des Rahmens aktualisiert*</span><span class="sxs-lookup"><span data-stu-id="724ab-251">*Internet Explorer - Border color updated*</span></span>
10. <span data-ttu-id="724ab-252">Wechseln Sie zur Google Chrome, und beachten Sie, wie sich die Farbe des Rahmens geändert hat.</span><span class="sxs-lookup"><span data-stu-id="724ab-252">Switch to Google Chrome and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="724ab-253">![Google Chrome – die Farbe des Rahmens aktualisiert](visual-studio-2013-web-tools/_static/image20.png "Google Chrome – die Farbe des Rahmens aktualisiert")</span><span class="sxs-lookup"><span data-stu-id="724ab-253">![Google Chrome - Border color updated](visual-studio-2013-web-tools/_static/image20.png "Google Chrome - Border color updated")</span></span>

    <span data-ttu-id="724ab-254">*Google Chrome – die Farbe des Rahmens aktualisiert*</span><span class="sxs-lookup"><span data-stu-id="724ab-254">*Google Chrome - Border color updated*</span></span>
11. <span data-ttu-id="724ab-255">Wechseln Sie zurück zu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="724ab-255">Switch back to Visual Studio.</span></span>
12. <span data-ttu-id="724ab-256">Wechseln Sie an das Ende der **"Site.CSS"** -Datei, und drücken Sie **STRG** + **F** finden die **.btn** Selektor.</span><span class="sxs-lookup"><span data-stu-id="724ab-256">Go to the end of the **Site.css** file and press **CTRL** + **F** to locate the **.btn** selector.</span></span>
13. <span data-ttu-id="724ab-257">Beachten Sie, dass die **- Webkit-Rahmen-Radius** Eigenschaft Grün unterstrichen ist.</span><span class="sxs-lookup"><span data-stu-id="724ab-257">Notice that the **-webkit-border-radius** property is underlined in green.</span></span>

    <span data-ttu-id="724ab-258">![-Webkit-Rahmen-Radius-Eigenschaft des Selektors Btn](visual-studio-2013-web-tools/_static/image21.png "- Webkit-Rahmen-Radius-Eigenschaft des Selektors Btn")</span><span class="sxs-lookup"><span data-stu-id="724ab-258">![-webkit-border-radius property of the btn selector](visual-studio-2013-web-tools/_static/image21.png "-webkit-border-radius property of the btn selector")</span></span>

    <span data-ttu-id="724ab-259">*-Webkit-Rahmen-Radius-Eigenschaft des Selektors btn*</span><span class="sxs-lookup"><span data-stu-id="724ab-259">*-webkit-border-radius property of the btn selector*</span></span>
14. <span data-ttu-id="724ab-260">Platzieren Sie den Textcursor in den **- Webkit-Rahmen-Radius** Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="724ab-260">Place the caret in the **-webkit-border-radius** property.</span></span> <span data-ttu-id="724ab-261">Eine blaue Linie sollte unter dem ersten Buchstaben des ersten Worts der Eigenschaft angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="724ab-261">A blue line should appear under the first letter of the first word of the property.</span></span> <span data-ttu-id="724ab-262">Dies ist die **Smarttag**.</span><span class="sxs-lookup"><span data-stu-id="724ab-262">This is the **smart tag**.</span></span>
15. <span data-ttu-id="724ab-263">Drücken Sie **STRG** + **.**</span><span class="sxs-lookup"><span data-stu-id="724ab-263">Press **CTRL** + **.**</span></span> <span data-ttu-id="724ab-264">Öffnen Sie das Menü Vorschläge, und klicken Sie auf **hinzufügen fehlender Standardeigenschaft (Border-Radius)**.</span><span class="sxs-lookup"><span data-stu-id="724ab-264">to open the suggestions menu and click **Add missing standard property (border-radius)**.</span></span>

    <span data-ttu-id="724ab-265">![Hinzufügen fehlender Standardeigenschaft Vorschlag](visual-studio-2013-web-tools/_static/image22.png "hinzufügen fehlender Standardeigenschaft Vorschlag")</span><span class="sxs-lookup"><span data-stu-id="724ab-265">![Add missing standard property suggestion](visual-studio-2013-web-tools/_static/image22.png "Add missing standard property suggestion")</span></span>

    <span data-ttu-id="724ab-266">*Fügen Sie fehlender Standardeigenschaft Vorschlag hinzu*</span><span class="sxs-lookup"><span data-stu-id="724ab-266">*Add missing standard property suggestion*</span></span>
16. <span data-ttu-id="724ab-267">Die **Rahmen-Radius** Eigenschaft wird die CSS-Regel automatisch hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="724ab-267">The **border-radius** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="724ab-268">![Fehlende Standardeigenschaft hinzugefügt](visual-studio-2013-web-tools/_static/image23.png "fehlt die standard-Eigenschaft hinzugefügt")</span><span class="sxs-lookup"><span data-stu-id="724ab-268">![Missing standard property added](visual-studio-2013-web-tools/_static/image23.png "Missing standard property added")</span></span>

    <span data-ttu-id="724ab-269">*Fehlende Standardeigenschaft hinzugefügt*</span><span class="sxs-lookup"><span data-stu-id="724ab-269">*Missing standard property added*</span></span>
17. <span data-ttu-id="724ab-270">Zeigen Sie auf die **Rahmen-Radius** Eigenschaft zum Anzeigen der **Browser Matrix QuickInfo**.</span><span class="sxs-lookup"><span data-stu-id="724ab-270">Move the pointer over the **border-radius** property to display the **Browser matrix tooltip**.</span></span> <span data-ttu-id="724ab-271">Die **Browser Matrix QuickInfo** zeigt die Verfügbarkeit der Eigenschaft in jedem Browser.</span><span class="sxs-lookup"><span data-stu-id="724ab-271">The **Browser matrix tooltip** shows the availability of the property in each browser.</span></span>

    <span data-ttu-id="724ab-272">![Browser Matrix QuickInfo](visual-studio-2013-web-tools/_static/image24.png "Browser Matrix QuickInfo")</span><span class="sxs-lookup"><span data-stu-id="724ab-272">![Browser matrix tooltip](visual-studio-2013-web-tools/_static/image24.png "Browser matrix tooltip")</span></span>

    <span data-ttu-id="724ab-273">*Browser-Matrix-QuickInfo*</span><span class="sxs-lookup"><span data-stu-id="724ab-273">*Browser matrix tooltip*</span></span>
18. <span data-ttu-id="724ab-274">Beachten Sie, dass der Wert des der **Rahmen-Radius** -Eigenschaft ist immer noch unterstrichen.</span><span class="sxs-lookup"><span data-stu-id="724ab-274">Notice that the value of the **border-radius** property is still underlined.</span></span> <span data-ttu-id="724ab-275">Zeigen Sie auf den Wert in der Warnmeldung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="724ab-275">Move the pointer over the value to see the warning message.</span></span>

    <span data-ttu-id="724ab-276">![Border-Radius-Eigenschaft Warnung](visual-studio-2013-web-tools/_static/image25.png "Rahmen-Radius-Eigenschaft-Wert-Warnung")</span><span class="sxs-lookup"><span data-stu-id="724ab-276">![Border-radius property value warning](visual-studio-2013-web-tools/_static/image25.png "Border-radius property value warning")</span></span>

    <span data-ttu-id="724ab-277">*Warnung Rahmen-Radius-Eigenschaft*</span><span class="sxs-lookup"><span data-stu-id="724ab-277">*Border-radius property value warning*</span></span>
19. <span data-ttu-id="724ab-278">Entfernen Sie die Einheit für die **Rahmen-Radius** Eigenschaftswert gemäß dem Vorschlag von der QuickInfo.</span><span class="sxs-lookup"><span data-stu-id="724ab-278">Remove the unit of the **border-radius** property value as suggested by the tooltip.</span></span>
20. <span data-ttu-id="724ab-279">Als **Rahmen-Radius** ist die standard-Eigenschaft für die Definition wie abgerundeten Rahmen, die Ecken sind, können Sie entfernen die **- Webkit-Rahmen-Radius** -Eigenschaft und Wert in der CSS-Regel.</span><span class="sxs-lookup"><span data-stu-id="724ab-279">As **border-radius** is the standard property for defining how rounded border corners are, you can remove the **-webkit-border-radius** property and value from the CSS rule.</span></span>
21. <span data-ttu-id="724ab-280">Platzieren Sie den Textcursor in den **Zeilenumbruch** -Eigenschaft und das Smarttag auch unten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="724ab-280">Place the caret in the **word-wrap** property and notice that the smart tag also appears below.</span></span>
22. <span data-ttu-id="724ab-281">Öffnen Sie das Menü, und klicken Sie auf **hinzufügen fehlender Besonderheiten der jeweiligen Hersteller**.</span><span class="sxs-lookup"><span data-stu-id="724ab-281">Open the menu and click **Add missing vendor specifics**.</span></span>

    <span data-ttu-id="724ab-282">![Hinzufügen fehlender Hersteller Besonderheiten Vorschlag](visual-studio-2013-web-tools/_static/image26.png "fehlenden Anbieter Besonderheiten der jeweiligen Empfehlung hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="724ab-282">![Add missing vendor specifics suggestion](visual-studio-2013-web-tools/_static/image26.png "Add missing vendor specifics suggestion")</span></span>

    <span data-ttu-id="724ab-283">*Fügen Sie fehlender Hersteller Besonderheiten Vorschlag hinzu*</span><span class="sxs-lookup"><span data-stu-id="724ab-283">*Add missing vendor specifics suggestion*</span></span>
23. <span data-ttu-id="724ab-284">Die **-ms-Zeilenumbruch** Eigenschaft wird die CSS-Regel automatisch hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="724ab-284">The **-ms-word-wrap** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="724ab-285">![Hersteller (Eigenschaft) bestimmte hinzugefügt](visual-studio-2013-web-tools/_static/image27.png "bestimmten Hersteller (Eigenschaft) hinzugefügt")</span><span class="sxs-lookup"><span data-stu-id="724ab-285">![Vendor specific property added](visual-studio-2013-web-tools/_static/image27.png "Vendor specific property added")</span></span>

    <span data-ttu-id="724ab-286">*Bestimmten Hersteller (Eigenschaft) hinzugefügt*</span><span class="sxs-lookup"><span data-stu-id="724ab-286">*Vendor specific property added*</span></span>

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a><span data-ttu-id="724ab-287">Aufgabe 4: Aktualisieren der HTML-Code aus dem Browser</span><span class="sxs-lookup"><span data-stu-id="724ab-287">Task 4 - Updating the HTML Code from the Browser</span></span>

<span data-ttu-id="724ab-288">In dieser Aufgabe verwenden Sie den Browserlink **Entwurfsmodus** Funktion bearbeiten das DOM-Objekt, aus dem Browser, und übertragen die Änderungen an der HTML-Quelldatei in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="724ab-288">In this task, you will use the Browser Link's **Design Mode** feature to edit the DOM object from the browser and transfer the changes to the HTML source file in Visual Studio.</span></span>

1. <span data-ttu-id="724ab-289">Drücken Sie in Google Chrome, **STRG** + **ALT** + **D** Entwurfsmodus aktivieren.</span><span class="sxs-lookup"><span data-stu-id="724ab-289">In Google Chrome, press **CTRL** + **ALT** + **D** to enable Design Mode.</span></span>
2. <span data-ttu-id="724ab-290">Zeigen Sie auf die **Lorem Ipsum Dolor Sit Amet** bezeichnen, und klicken Sie auf.</span><span class="sxs-lookup"><span data-stu-id="724ab-290">Move the pointer over the **Lorem Ipsum dolor sit amet** label and click.</span></span>

    <span data-ttu-id="724ab-291">![Bearbeiten die Frage](visual-studio-2013-web-tools/_static/image28.png "die Frage bearbeiten")</span><span class="sxs-lookup"><span data-stu-id="724ab-291">![Editing the question](visual-studio-2013-web-tools/_static/image28.png "Editing the question")</span></span>

    <span data-ttu-id="724ab-292">*Die Frage bearbeiten*</span><span class="sxs-lookup"><span data-stu-id="724ab-292">*Editing the question*</span></span>
3. <span data-ttu-id="724ab-293">Ein Cursor sollte angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="724ab-293">A cursor should appear.</span></span> <span data-ttu-id="724ab-294">Ersetzen Sie den ursprünglichen Text mit *Was sieht, wenn eine Frage mehr geschrieben?*, und drücken Sie dann **ESC** zum Entwurfsmodus zu beenden.</span><span class="sxs-lookup"><span data-stu-id="724ab-294">Replace the original text with *What does it look like when I write a longer question?*, and then press **ESC** to exit Design Mode.</span></span>

    <span data-ttu-id="724ab-295">![Frage bearbeitet](visual-studio-2013-web-tools/_static/image29.png "Frage bearbeitet")</span><span class="sxs-lookup"><span data-stu-id="724ab-295">![Question edited](visual-studio-2013-web-tools/_static/image29.png "Question edited")</span></span>

    <span data-ttu-id="724ab-296">*Frage bearbeitet*</span><span class="sxs-lookup"><span data-stu-id="724ab-296">*Question edited*</span></span>
4. <span data-ttu-id="724ab-297">Wechseln Sie zurück zu Visual Studio und öffnen **"Index.cshtml"**, sofern nicht bereits geöffnet.</span><span class="sxs-lookup"><span data-stu-id="724ab-297">Switch back to Visual Studio and open **Index.cshtml**, if not already opened.</span></span> <span data-ttu-id="724ab-298">Beachten Sie, dass der innere Text des der **&lt;p&gt;** Element aktualisiert wurde.</span><span class="sxs-lookup"><span data-stu-id="724ab-298">Notice that the inner text of the **&lt;p&gt;** element has been updated.</span></span>

    <span data-ttu-id="724ab-299">![Aktualisierte Frage im HTML-Seite](visual-studio-2013-web-tools/_static/image30.png "aktualisierte Frage im HTML-Seite")</span><span class="sxs-lookup"><span data-stu-id="724ab-299">![Updated question in the HTML page](visual-studio-2013-web-tools/_static/image30.png "Updated question in the HTML page")</span></span>

    <span data-ttu-id="724ab-300">*Aktualisierte Frage im HTML-Seite*</span><span class="sxs-lookup"><span data-stu-id="724ab-300">*Updated question in the HTML page*</span></span>

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a><span data-ttu-id="724ab-301">Aufgabe 5: Überprüfung SEO-bezogene Warnungen</span><span class="sxs-lookup"><span data-stu-id="724ab-301">Task 5 - Reviewing SEO Related Warnings</span></span>

<span data-ttu-id="724ab-302">**Suchmaschinenoptimierung** (SEO) ist der Prozess der höheren Rang Website auf eine Such-Engine-Liste mit Ergebnissen vornehmen.</span><span class="sxs-lookup"><span data-stu-id="724ab-302">**Search Engine Optimization** (SEO) is the process of making a website rank higher on a search engine's list of results.</span></span> <span data-ttu-id="724ab-303">Je höher die Rangfolge bestimmt der Website und je konsistenter wird aufgeführt, die mehr Besucher die Website erhalten aus dieser Such-Engine.</span><span class="sxs-lookup"><span data-stu-id="724ab-303">The higher the site ranks and the more consistently it is listed, the more visitors the site will get from that search engine.</span></span> <span data-ttu-id="724ab-304">Web Essentials enthält, in der HTML untersucht Instrument, Berichte, die Probleme gefunden, und bietet Unterstützung zu erhalten, um sie zu beheben.</span><span class="sxs-lookup"><span data-stu-id="724ab-304">Web Essentials incorporates an analytical tool that examines HTML, reports the issues found and provides assistance to fix them.</span></span>

1. <span data-ttu-id="724ab-305">Wechseln Sie zu der **Ansicht** Menü, und klicken Sie auf **Fehlerliste** zum Öffnen der **Fehlerliste** Fenster.</span><span class="sxs-lookup"><span data-stu-id="724ab-305">Go to the **View** menu and click **Error List** to open the **Error List** window.</span></span>

    <span data-ttu-id="724ab-306">![In der Ansicht Fehlerliste Menü](visual-studio-2013-web-tools/_static/image31.png "Fehlerliste im Menü \"Ansicht\"")</span><span class="sxs-lookup"><span data-stu-id="724ab-306">![Error List in View menu](visual-studio-2013-web-tools/_static/image31.png "Error List in View menu")</span></span>

    <span data-ttu-id="724ab-307">*Fehler in der Ansicht im Menü aufgeführt.*</span><span class="sxs-lookup"><span data-stu-id="724ab-307">*Error List in View menu*</span></span>
2. <span data-ttu-id="724ab-308">Beachten Sie, dass es eine SEO-Warnung benachrichtigt, die eine **&lt;Meta&gt;** tag für die Beschreibung für die Seite fehlt.</span><span class="sxs-lookup"><span data-stu-id="724ab-308">Notice that there is an SEO warning notifying that a **&lt;meta&gt;** tag for the page description is missing.</span></span> <span data-ttu-id="724ab-309">Doppelklicken Sie auf den SEO-Warnung-Eintrag, um das Problem zu beheben.</span><span class="sxs-lookup"><span data-stu-id="724ab-309">Double-click the SEO warning entry to fix it.</span></span>

    <span data-ttu-id="724ab-310">![Fehlerliste (Fenster)](visual-studio-2013-web-tools/_static/image32.png "Fenster \"Fehlerliste\"")</span><span class="sxs-lookup"><span data-stu-id="724ab-310">![Error List window](visual-studio-2013-web-tools/_static/image32.png "Error List window")</span></span>

    <span data-ttu-id="724ab-311">*Fehlerliste (Fenster)*</span><span class="sxs-lookup"><span data-stu-id="724ab-311">*Error List window*</span></span>
3. <span data-ttu-id="724ab-312">In der **Web Essentials** Dialogfeld klicken Sie auf **Ja** , fügen Sie eine Beschreibung &lt;Meta&gt; Tag.</span><span class="sxs-lookup"><span data-stu-id="724ab-312">In the **Web Essentials** dialog box, click **Yes** to insert a description &lt;meta&gt; tag.</span></span>

    <span data-ttu-id="724ab-313">![Web Essentials-Dialogfeld](visual-studio-2013-web-tools/_static/image33.png "Web Essentials-Dialogfeld")</span><span class="sxs-lookup"><span data-stu-id="724ab-313">![Web Essentials dialog box](visual-studio-2013-web-tools/_static/image33.png "Web Essentials dialog box")</span></span>

    <span data-ttu-id="724ab-314">*Web Essentials-Dialogfeld*</span><span class="sxs-lookup"><span data-stu-id="724ab-314">*Web Essentials dialog box*</span></span>
4. <span data-ttu-id="724ab-315">Der Editor für  **\_Layout.cshtml** wird geöffnet und die **&lt;Meta&gt;** Tag wird automatisch hinzugefügt, die **Head** Teil der HTML-Datei.</span><span class="sxs-lookup"><span data-stu-id="724ab-315">The editor for **\_Layout.cshtml** opens and the **&lt;meta&gt;** tag is automatically added to the **head** section of the HTML file.</span></span>

    <span data-ttu-id="724ab-316">![Meta-Tag, die automatisch auf _Layout hinzugefügt](visual-studio-2013-web-tools/_static/image34.png "Meta-Tag _Layout Seite automatisch hinzugefügt.")</span><span class="sxs-lookup"><span data-stu-id="724ab-316">![Meta tag automatically added in _Layout page](visual-studio-2013-web-tools/_static/image34.png "Meta tag automatically added in _Layout page")</span></span>

    <span data-ttu-id="724ab-317">*Meta-Tag automatisch hinzugefügt, \_Layoutseite*</span><span class="sxs-lookup"><span data-stu-id="724ab-317">*Meta tag automatically added to \_Layout page*</span></span>
5. <span data-ttu-id="724ab-318">Ändern Sie den Wert, der die **Inhalt** Attribut *GeekQuiz* und speichern Sie die Datei.</span><span class="sxs-lookup"><span data-stu-id="724ab-318">Change the value of the **content** attribute to *GeekQuiz* and save the file.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a><span data-ttu-id="724ab-319">Übung 2: Nutzen von Codeausschnitten und IntelliSense</span><span class="sxs-lookup"><span data-stu-id="724ab-319">Exercise 2: Taking Advantage of Code Snippets and IntelliSense</span></span>

<span data-ttu-id="724ab-320">Mit Web Essentials wurde der HTML-Editor mit zusätzlicher Funktionalität erweitert.</span><span class="sxs-lookup"><span data-stu-id="724ab-320">With Web Essentials, the HTML editor has been extended with extra functionality.</span></span> <span data-ttu-id="724ab-321">In dieser Übung sehen Sie einige neue Features, die Sie bei der Entwicklung von Webanwendungen.</span><span class="sxs-lookup"><span data-stu-id="724ab-321">In this exercise, you will see some new features that are helpful when developing web applications.</span></span>

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a><span data-ttu-id="724ab-322">Aufgabe 1: Verwenden von IntelliSense in HTML-Dokumente</span><span class="sxs-lookup"><span data-stu-id="724ab-322">Task 1 - Using IntelliSense in HTML Documents</span></span>

<span data-ttu-id="724ab-323">Das erste neue Feature wird Ihnen in dieser Aufgabe wird aufgerufen, **dynamische IntelliSense**.</span><span class="sxs-lookup"><span data-stu-id="724ab-323">The first new feature you will see in this task is called **Dynamic IntelliSense**.</span></span> <span data-ttu-id="724ab-324">Dynamische IntelliSense liest andere Tags und Attribute, die möglichen Ids abzuleiten, die Sie verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="724ab-324">Dynamic IntelliSense reads other tags and attributes to infer the possible ids you will use.</span></span>

<span data-ttu-id="724ab-325">In dieser Aufgabe erstellen Sie ein neues HTML-Formular-Element enthält eine Bezeichnung und einem Eingabefeld ein.</span><span class="sxs-lookup"><span data-stu-id="724ab-325">In this task, you will create a new HTML form element which contains a label and an input field.</span></span> <span data-ttu-id="724ab-326">Fügen Sie hinzu eine **für** -Attribut auf die Bezeichnung, die die Bindung an die Eingabe, und Sie sehen die IntelliSense-Vorschläge basierend auf die Ids der Eingaben im Bereich.</span><span class="sxs-lookup"><span data-stu-id="724ab-326">Then you will add a **for** attribute to the label to bind it to the input, and you will see IntelliSense suggestions based on the ids of the inputs in scope.</span></span>

1. <span data-ttu-id="724ab-327">Open **Visual Studio Express 2013 für Web** und **Begin.sln** Lösung befindet sich in der **Quelle/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/Anfang** Ordner.</span><span class="sxs-lookup"><span data-stu-id="724ab-327">Open **Visual Studio Express 2013 for Web** and the **Begin.sln** solution located in the **Source/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/Begin** folder.</span></span> <span data-ttu-id="724ab-328">Alternativ können Sie die Lösung weiter nutzen, den Sie in der vorherigen Übung haben.</span><span class="sxs-lookup"><span data-stu-id="724ab-328">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="724ab-329">In **Projektmappen-Explorer**öffnen die **"Index.cshtml"** Datei befindet sich in der **Ansichten** | **Startseite** Ordner.</span><span class="sxs-lookup"><span data-stu-id="724ab-329">In **Solution Explorer**, open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="724ab-330">Hinzufügen der folgenden Form innerhalb der **&lt;Abschnitt&gt;** Element.</span><span class="sxs-lookup"><span data-stu-id="724ab-330">Add the following form inside the **&lt;section&gt;** element.</span></span>

    <span data-ttu-id="724ab-331">(Codeausschnitt - *VisualStudio2013WebTooling* - *Ex2* - *Formular*)</span><span class="sxs-lookup"><span data-stu-id="724ab-331">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *Form*)</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. <span data-ttu-id="724ab-332">Das input-Tag sollte eine Bezeichnung mit dem eine Beschreibung des Felds vorangestellt werden.</span><span class="sxs-lookup"><span data-stu-id="724ab-332">The input tag should be preceded by a label with some description of the field.</span></span> <span data-ttu-id="724ab-333">Fügen Sie die folgende Bezeichnung vor den input-Tag hinzu.</span><span class="sxs-lookup"><span data-stu-id="724ab-333">Add the following label before the input tag.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. <span data-ttu-id="724ab-334">Die **für** Attribut eine **&lt;Bezeichnung&gt;** gibt an, an welches Formularelement eine Beschriftung gebunden ist.</span><span class="sxs-lookup"><span data-stu-id="724ab-334">The **for** attribute of a **&lt;label&gt;** specifies which form element a label is bound to.</span></span> <span data-ttu-id="724ab-335">Der Wert des Attributs muss gleich der Id des verknüpften Elements.</span><span class="sxs-lookup"><span data-stu-id="724ab-335">The attribute's value should be equal to the id of the related element.</span></span> <span data-ttu-id="724ab-336">Hinzufügen der **für** -Attribut auf die **&lt;Bezeichnung&gt;** Element.</span><span class="sxs-lookup"><span data-stu-id="724ab-336">Add the **for** attribute to the **&lt;label&gt;** element.</span></span> <span data-ttu-id="724ab-337">Wie in der folgenden Abbildung dargestellt die &quot;Namen&quot; Wert eingeblendet wird, klicken Sie im IntelliSense basierend auf der Id der Elemente innerhalb des gleichen Bereichs (die einschließende  **&lt;Formular&gt;**).</span><span class="sxs-lookup"><span data-stu-id="724ab-337">As shown in the following figure, the &quot;name&quot; value pops up in the IntelliSense box, based on the id of the elements within the same scope (the enclosing **&lt;form&gt;**).</span></span>

    <span data-ttu-id="724ab-338">![Die Id in IntelliSense angezeigt](visual-studio-2013-web-tools/_static/image35.png "die Id wird in IntelliSense angezeigt.")</span><span class="sxs-lookup"><span data-stu-id="724ab-338">![Showing the id in IntelliSense](visual-studio-2013-web-tools/_static/image35.png "Showing the id in IntelliSense")</span></span>

    <span data-ttu-id="724ab-339">*Die Id wird in IntelliSense angezeigt.*</span><span class="sxs-lookup"><span data-stu-id="724ab-339">*Showing the id in IntelliSense*</span></span>
6. <span data-ttu-id="724ab-340">Löschen Sie die zuletzt hinzugefügte **&lt;Formular&gt;** Element und dessen Inhalt.</span><span class="sxs-lookup"><span data-stu-id="724ab-340">Delete the recently added **&lt;form&gt;** element and its content.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a><span data-ttu-id="724ab-341">Aufgabe 2: mithilfe von HTML-Codeausschnitten</span><span class="sxs-lookup"><span data-stu-id="724ab-341">Task 2 - Using HTML Code Snippets</span></span>

<span data-ttu-id="724ab-342">HTML5 wurden mehr als 25 neue semantische Tags eingeführt.</span><span class="sxs-lookup"><span data-stu-id="724ab-342">HTML5 introduced more than 25 new semantic tags.</span></span> <span data-ttu-id="724ab-343">Visual Studio war bereits ein IntelliSense-Unterstützung für diese Tags, aber Visual Studio 2013 können sie schneller und einfacher, Markup durch das Hinzufügen neuer Codeausschnitte zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="724ab-343">Visual Studio already had IntelliSense support for these tags, but Visual Studio 2013 makes it faster and easier to write markup by adding new code snippets.</span></span> <span data-ttu-id="724ab-344">Obwohl diese Tags nicht komplizierter sind, sind sie auch mit ein paar feinheiten auf, z. B. das Hinzufügen der korrekten Codec-Fallbacks für die *audio* Tag.</span><span class="sxs-lookup"><span data-stu-id="724ab-344">Though these tags are not complicated, they come with a few small subtleties, such as adding the correct codec fallbacks for the *audio* tag.</span></span> <span data-ttu-id="724ab-345">In dieser Aufgabe wird die HTML-Codeausschnitte für das audio Tag angezeigt.</span><span class="sxs-lookup"><span data-stu-id="724ab-345">In this task, you will see the HTML code snippets for the audio tag.</span></span>

1. <span data-ttu-id="724ab-346">In der **"Index.cshtml"** Geben Sie eine Datei  **&lt;Aud** innerhalb der **&lt;Abschnitt&gt;** -Element wie in der folgenden Abbildung gezeigt.</span><span class="sxs-lookup"><span data-stu-id="724ab-346">In the **Index.cshtml** file, type **&lt;aud** inside the **&lt;section&gt;** element as shown in the following figure.</span></span>

    <span data-ttu-id="724ab-347">![Ein audio-Element einfügen](visual-studio-2013-web-tools/_static/image36.png "ein audio-Element einfügen")</span><span class="sxs-lookup"><span data-stu-id="724ab-347">![Inserting an audio element](visual-studio-2013-web-tools/_static/image36.png "Inserting an audio element")</span></span>

    <span data-ttu-id="724ab-348">*Ein audio-Element einfügen*</span><span class="sxs-lookup"><span data-stu-id="724ab-348">*Inserting an audio element*</span></span>
2. <span data-ttu-id="724ab-349">Drücken Sie **Registerkarte** zweimal und beachten Sie, wie im folgende Code auf der Seite hinzugefügt wird, und der Cursor befindet sich auf die **Src** Attribut der ersten Quelle.</span><span class="sxs-lookup"><span data-stu-id="724ab-349">Press **TAB** twice and notice how the following code is added on the page and the cursor is placed on the **src** attribute of the first source.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > <span data-ttu-id="724ab-350">Durch Drücken der **Registerkarte** Schlüssel und der Codeausschnitt eingefügt wird.</span><span class="sxs-lookup"><span data-stu-id="724ab-350">By pressing the **TAB** key twice, the code snippet is inserted.</span></span> <span data-ttu-id="724ab-351">Der audio-Codeausschnitt zeigt die Standardverwendung von der *audio* -Tag mit zwei Quelldateien für die verbesserte Unterstützung.</span><span class="sxs-lookup"><span data-stu-id="724ab-351">The audio snippet shows the standard usage of the *audio* tag, with two source files for improved support.</span></span>
3. <span data-ttu-id="724ab-352">Die zweite Zeile löschen und aktualisieren Sie die Datenquelle der ersten Zeile mit den folgenden Link in der WebCampsTV Katana-Show: [ http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3 ](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3).</span><span class="sxs-lookup"><span data-stu-id="724ab-352">Delete the second line and update the source of the first line with the following link to the WebCampsTV Katana show: [http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3).</span></span> <span data-ttu-id="724ab-353">Der resultierende Code ist unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="724ab-353">The resulting code is shown below.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > <span data-ttu-id="724ab-354">Die Quelldatei *KatanaProject.mp3* als Beispiel verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="724ab-354">The source file *KatanaProject.mp3* is used as an example.</span></span> <span data-ttu-id="724ab-355">Falls gewünscht, können Sie eine andere Quelle.</span><span class="sxs-lookup"><span data-stu-id="724ab-355">You can use another source if you prefer.</span></span>
4. <span data-ttu-id="724ab-356">Drücken Sie **STRG** + **S** zum Speichern der Datei.</span><span class="sxs-lookup"><span data-stu-id="724ab-356">Press **CTRL** + **S** to save the file.</span></span>
5. <span data-ttu-id="724ab-357">Drücken Sie **STRG** + **F5** zum Starten der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="724ab-357">Press **CTRL** + **F5** to start the application.</span></span>
6. <span data-ttu-id="724ab-358">Beachten Sie, dass die Anwendung ein Audioplayer hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="724ab-358">Notice that an audio player was added to the application.</span></span>

    <span data-ttu-id="724ab-359">![Audio-Player in Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Audio-Player in Internet Explorer")</span><span class="sxs-lookup"><span data-stu-id="724ab-359">![Audio player in Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Audio player in Internet Explorer")</span></span>

    <span data-ttu-id="724ab-360">*Audio-Player in Internet Explorer*</span><span class="sxs-lookup"><span data-stu-id="724ab-360">*Audio player in Internet Explorer*</span></span>

    <span data-ttu-id="724ab-361">![Audioplayer in Google Chrome](visual-studio-2013-web-tools/_static/image38.png "Audio-Player im Google Chrome")</span><span class="sxs-lookup"><span data-stu-id="724ab-361">![Audio player in Google Chrome](visual-studio-2013-web-tools/_static/image38.png "Audio player in Google Chrome")</span></span>

    <span data-ttu-id="724ab-362">*Audioplayer in Google Chrome*</span><span class="sxs-lookup"><span data-stu-id="724ab-362">*Audio player in Google Chrome*</span></span>
7. <span data-ttu-id="724ab-363">Schließen Sie den Browser nicht.</span><span class="sxs-lookup"><span data-stu-id="724ab-363">Do not close the browsers.</span></span> <span data-ttu-id="724ab-364">Sie verwenden diese in der nächsten Aufgabe.</span><span class="sxs-lookup"><span data-stu-id="724ab-364">You will use them in the next task.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a><span data-ttu-id="724ab-365">Aufgabe 3: Verwenden von IntelliSense in JavaScript-Dokumenten</span><span class="sxs-lookup"><span data-stu-id="724ab-365">Task 3 - Using IntelliSense in JavaScript Documents</span></span>

<span data-ttu-id="724ab-366">Web Essentials 2013 wird eine Liste von IDs und Klassennamen Stylesheets und HTML-Seiten erstellen.</span><span class="sxs-lookup"><span data-stu-id="724ab-366">With Web Essentials 2013, style sheets and HTML pages produce a list of IDs and class names.</span></span> <span data-ttu-id="724ab-367">In dieser Aufgabe lernen Sie, wie diese Listen mit JavaScript-IntelliSense-Unterstützung in Web Essentials 2013 zu verbessern.</span><span class="sxs-lookup"><span data-stu-id="724ab-367">In this task, you will learn how those lists improve JavaScript IntelliSense support in Web Essentials 2013.</span></span>

1. <span data-ttu-id="724ab-368">In der **"Index.cshtml"** Datei, fügen Sie folgenden Code zum Definieren einer **Skript** Tag für JavaScript-Code.</span><span class="sxs-lookup"><span data-stu-id="724ab-368">In the **Index.cshtml** file, add the following code to define a **script** tag for JavaScript code.</span></span>

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. <span data-ttu-id="724ab-369">Fügen Sie den folgenden Code innerhalb der **Skript** Tag, um den bereit-Abruffunktion definieren.</span><span class="sxs-lookup"><span data-stu-id="724ab-369">Add the following code inside the **script** tag to define the ready callback function.</span></span>

    <span data-ttu-id="724ab-370">(Codeausschnitt - *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)</span><span class="sxs-lookup"><span data-stu-id="724ab-370">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. <span data-ttu-id="724ab-371">Platzieren Sie den Textcursor in den **Skript** Tag, und drücken Sie **STRG** + **.**</span><span class="sxs-lookup"><span data-stu-id="724ab-371">Place the caret in the **script** tag and press **CTRL** + **.**</span></span> <span data-ttu-id="724ab-372">um das Menü "Vorschlag" zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="724ab-372">to open the suggestion menu.</span></span>
4. <span data-ttu-id="724ab-373">Klicken Sie auf **extrahieren Sie in der Datei**.</span><span class="sxs-lookup"><span data-stu-id="724ab-373">Click **Extract To File**.</span></span>

    <span data-ttu-id="724ab-374">![Extrahieren Sie JavaScript in Datei-Vorschlag](visual-studio-2013-web-tools/_static/image39.png "JavaScript zu extrahieren, in der Datei Vorschlag")</span><span class="sxs-lookup"><span data-stu-id="724ab-374">![JavaScript extract to file suggestion](visual-studio-2013-web-tools/_static/image39.png "JavaScript extract to file suggestion")</span></span>

    <span data-ttu-id="724ab-375">*Extrahieren Sie JavaScript in Datei-Vorschlag*</span><span class="sxs-lookup"><span data-stu-id="724ab-375">*JavaScript extract to file suggestion*</span></span>
5. <span data-ttu-id="724ab-376">In der **speichern** wählen Sie im Fenster der **Skripts** Ordner, benennen Sie die Datei **init.js** , und klicken Sie auf **speichern**.</span><span class="sxs-lookup"><span data-stu-id="724ab-376">In the **Save As** window, select the **Scripts** folder, name the file **init.js** and click **Save**.</span></span>

    <span data-ttu-id="724ab-377">![Fenster "Speichern unter"](visual-studio-2013-web-tools/_static/image40.png "Fenster Speichern unter")</span><span class="sxs-lookup"><span data-stu-id="724ab-377">![Save As window](visual-studio-2013-web-tools/_static/image40.png "Save As window")</span></span>

    <span data-ttu-id="724ab-378">*Fenster "Speichern unter"*</span><span class="sxs-lookup"><span data-stu-id="724ab-378">*Save As window*</span></span>

    > [!NOTE]
    > <span data-ttu-id="724ab-379">Die **init.js** Datei wird erstellt und der Inhalt des Skripts wird in der Datei verschoben.</span><span class="sxs-lookup"><span data-stu-id="724ab-379">The **init.js** file is created and the content of the script is moved to the file.</span></span>
    > 
    > <span data-ttu-id="724ab-380">![Init.js-Datei mit den entsprechenden Inhalt erstellt](visual-studio-2013-web-tools/_static/image41.png "Init.js-Datei, die mit den entsprechenden Inhalt erstellt wurde")</span><span class="sxs-lookup"><span data-stu-id="724ab-380">![Init.js file created with the content included](visual-studio-2013-web-tools/_static/image41.png "Init.js file created with the content included")</span></span>
    > 
    > <span data-ttu-id="724ab-381">*Init.js-Datei, die mit den entsprechenden Inhalt erstellt wurde*</span><span class="sxs-lookup"><span data-stu-id="724ab-381">*Init.js file created with the content included*</span></span>
6. <span data-ttu-id="724ab-382">Öffnen der **"Index.cshtml"** Datei, und überprüfen Sie, dass das Skripttag ersetzt wurde, mit einem Verweis auf die **init.js** Datei.</span><span class="sxs-lookup"><span data-stu-id="724ab-382">Open the **Index.cshtml** file and check that the script tag was replaced with a reference to the **init.js** file.</span></span>

    <span data-ttu-id="724ab-383">![Die HTML-Verweis init.js](visual-studio-2013-web-tools/_static/image42.png "Init.js HTML-Referenz")</span><span class="sxs-lookup"><span data-stu-id="724ab-383">![Init.js html reference](visual-studio-2013-web-tools/_static/image42.png "Init.js html reference")</span></span>

    <span data-ttu-id="724ab-384">*Init.js HTML-Referenz*</span><span class="sxs-lookup"><span data-stu-id="724ab-384">*Init.js html reference*</span></span>
7. <span data-ttu-id="724ab-385">Wechseln Sie zu der **Projektmappen-Explorer** und beachten Sie, dass die **init.js** Datei automatisch in der Projektmappe enthalten war.</span><span class="sxs-lookup"><span data-stu-id="724ab-385">Go to the **Solution Explorer** and notice that the **init.js** file was included automatically in the solution.</span></span>

    <span data-ttu-id="724ab-386">![Init.js-Datei, die in der Lösung enthaltenen](visual-studio-2013-web-tools/_static/image43.png "Init.js-Datei, die in Projektmappen enthalten")</span><span class="sxs-lookup"><span data-stu-id="724ab-386">![Init.js file included in solution](visual-studio-2013-web-tools/_static/image43.png "Init.js file included in solution")</span></span>

    <span data-ttu-id="724ab-387">*Init.js-Datei, die in Projektmappen enthalten*</span><span class="sxs-lookup"><span data-stu-id="724ab-387">*Init.js file included in solution*</span></span>
8. <span data-ttu-id="724ab-388">Wechseln Sie zurück zu der **init.js** Datei zum Aktualisieren der **bereit** Callback-Funktion.</span><span class="sxs-lookup"><span data-stu-id="724ab-388">Switch back to the **init.js** file to update the **ready** function callback.</span></span>
9. <span data-ttu-id="724ab-389">In der Definition der Funktion-Rückruf, der an übergebene *bereit*, fügen Sie den folgenden Code rufen Sie alle Elemente durch eine bestimmte Klasse-Attribut.</span><span class="sxs-lookup"><span data-stu-id="724ab-389">Inside the function callback definition that is passed to *ready*, add the following code to get all the elements by a specific class attribute.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. <span data-ttu-id="724ab-390">Drücken Sie **STRG** + **Speicherplatz** zwischen den Anführungszeichen in der **GetElementsByClassName** Funktionsaufruf.</span><span class="sxs-lookup"><span data-stu-id="724ab-390">Press **CTRL** + **Space** between the quotes inside the **getElementsByClassName** function call.</span></span>

    <span data-ttu-id="724ab-391">![Anzeigen der IntelliSense für die Funktion GetElementsByClassName](visual-studio-2013-web-tools/_static/image44.png "zeigt IntelliSense für die GetElementsByClassName-Funktion")</span><span class="sxs-lookup"><span data-stu-id="724ab-391">![Showing IntelliSense for the getElementsByClassName function](visual-studio-2013-web-tools/_static/image44.png "Showing IntelliSense for the getElementsByClassName function")</span></span>

    <span data-ttu-id="724ab-392">*Anzeigen der IntelliSense für die GetElementsByClassName-Funktion*</span><span class="sxs-lookup"><span data-stu-id="724ab-392">*Showing IntelliSense for the getElementsByClassName function*</span></span>

    > [!NOTE]
    > <span data-ttu-id="724ab-393">Beachten Sie, IntelliSense die Klassen, die in der Projekt-Stylesheets definiert zeigt.</span><span class="sxs-lookup"><span data-stu-id="724ab-393">Notice that IntelliSense shows the classes defined in the project style sheets.</span></span>
11. <span data-ttu-id="724ab-394">Ersetzen Sie die Zeile, die Sie mit der folgende Code erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="724ab-394">Replace the line that you have created with the following code.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. <span data-ttu-id="724ab-395">Positionieren Sie den Cursor nach **au** in Anführungszeichen in der **GetElementsByTagName** -Funktion, und drücken Sie **STRG** + **Speicherplatz**.</span><span class="sxs-lookup"><span data-stu-id="724ab-395">Position the cursor after **au** inside the quotes in the **getElementsByTagName** function and press **CTRL** + **Space**.</span></span>

    <span data-ttu-id="724ab-396">![Anzeigen der IntelliSense für die Methode GetElementByTagName](visual-studio-2013-web-tools/_static/image45.png "zeigt IntelliSense für die GetElementByTagName-Methode")</span><span class="sxs-lookup"><span data-stu-id="724ab-396">![Showing IntelliSense for the getElementByTagName method](visual-studio-2013-web-tools/_static/image45.png "Showing IntelliSense for the getElementByTagName method")</span></span>

    <span data-ttu-id="724ab-397">*Anzeigen der IntelliSense für die GetElementsByTagName-Methode*</span><span class="sxs-lookup"><span data-stu-id="724ab-397">*Showing IntelliSense for the getElementsByTagName method*</span></span>
13. <span data-ttu-id="724ab-398">Wählen Sie **&quot;audio&quot;** aus der Liste und drücken Sie **EINGABETASTE**.</span><span class="sxs-lookup"><span data-stu-id="724ab-398">Select **&quot;audio&quot;** from the list and press **ENTER**.</span></span> <span data-ttu-id="724ab-399">Das Ergebnis ist in der folgenden Abbildung dargestellt.</span><span class="sxs-lookup"><span data-stu-id="724ab-399">The result is shown in the following figure.</span></span>

    <span data-ttu-id="724ab-400">![Abrufen von Audio Elementen](visual-studio-2013-web-tools/_static/image46.png "Audio Elemente abrufen")</span><span class="sxs-lookup"><span data-stu-id="724ab-400">![Retrieving Audio Elements](visual-studio-2013-web-tools/_static/image46.png "Retrieving Audio Elements")</span></span>

    <span data-ttu-id="724ab-401">*Abrufen von Audio-Elementen*</span><span class="sxs-lookup"><span data-stu-id="724ab-401">*Retrieving Audio Elements*</span></span>
14. <span data-ttu-id="724ab-402">In **Projektmappen-Explorer**, mit der rechten Maustaste die **init.js** Datei die **Skripts** Ordner, und wählen **minimieren JavaScript-Dateien** aus der **Web Essentials** Menü.</span><span class="sxs-lookup"><span data-stu-id="724ab-402">In **Solution Explorer**, right-click the **init.js** file in the **Scripts** folder and select **Minify JavaScript file(s)** from the **Web Essentials** menu.</span></span>

    <span data-ttu-id="724ab-403">![JavaScript-Dateien zu minimieren](visual-studio-2013-web-tools/_static/image47.png "minimieren JavaScript-Dateien")</span><span class="sxs-lookup"><span data-stu-id="724ab-403">![Minify JavaScript file(s)](visual-studio-2013-web-tools/_static/image47.png "Minify JavaScript files")</span></span>

    <span data-ttu-id="724ab-404">*Minimieren Sie die JavaScript-Dateien*</span><span class="sxs-lookup"><span data-stu-id="724ab-404">*Minify JavaScript file(s)*</span></span>
15. <span data-ttu-id="724ab-405">Wenn Sie aufgefordert werden, um automatische Minimierung zu aktivieren, wenn der Quelldatei geändert, klicken Sie auf **Ja**.</span><span class="sxs-lookup"><span data-stu-id="724ab-405">When prompted to enable automatic minification when the source file changes click **Yes**.</span></span>

    <span data-ttu-id="724ab-406">![Aktivieren automatische Minimierung Warnung](visual-studio-2013-web-tools/_static/image48.png "automatische Minimierung Warnung aktivieren")</span><span class="sxs-lookup"><span data-stu-id="724ab-406">![Enabling automatic minification warning](visual-studio-2013-web-tools/_static/image48.png "Enabling automatic minification warning")</span></span>

    <span data-ttu-id="724ab-407">*Aktivieren automatische Minimierung-Warnung*</span><span class="sxs-lookup"><span data-stu-id="724ab-407">*Enabling automatic minification warning*</span></span>

    > [!NOTE]
    > <span data-ttu-id="724ab-408">Die **init.min.js** wird erstellt und wurde als Abhängigkeit von der **init.js** Datei.</span><span class="sxs-lookup"><span data-stu-id="724ab-408">The **init.min.js** is created and is added as a dependency of the **init.js** file.</span></span>
    > 
    > <span data-ttu-id="724ab-409">![Erstellte init.Min.js Datei](visual-studio-2013-web-tools/_static/image49.png "Init.min.js-Datei erstellt wurde")</span><span class="sxs-lookup"><span data-stu-id="724ab-409">![Init.min.js file created](visual-studio-2013-web-tools/_static/image49.png "Init.min.js file created")</span></span>
    > 
    > <span data-ttu-id="724ab-410">*Init.Min.js-Datei erstellt wurde*</span><span class="sxs-lookup"><span data-stu-id="724ab-410">*Init.min.js file created*</span></span>
16. <span data-ttu-id="724ab-411">Öffnen der **init.min.js** Datei, und beachten Sie, dass die Datei verkleinert wird.</span><span class="sxs-lookup"><span data-stu-id="724ab-411">Open the **init.min.js** file and notice that the file is minified.</span></span>

    <span data-ttu-id="724ab-412">![Dateiinhalt init.Min.js](visual-studio-2013-web-tools/_static/image50.png "Init.min.js Dateiinhalt")</span><span class="sxs-lookup"><span data-stu-id="724ab-412">![Init.min.js file content](visual-studio-2013-web-tools/_static/image50.png "Init.min.js file content")</span></span>

    <span data-ttu-id="724ab-413">*Dateiinhalt init.Min.js*</span><span class="sxs-lookup"><span data-stu-id="724ab-413">*Init.min.js file content*</span></span>
17. <span data-ttu-id="724ab-414">In der **init.js** Datei, fügen Sie den folgenden Code unter der **GetElementsByTagName** Funktionsaufruf alle Elemente der audio wiedergegeben.</span><span class="sxs-lookup"><span data-stu-id="724ab-414">In the **init.js** file, add the following code below the **getElementsByTagName** function call to play all the audio elements.</span></span>

    <span data-ttu-id="724ab-415">(Codeausschnitt - *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)</span><span class="sxs-lookup"><span data-stu-id="724ab-415">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)</span></span>

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. <span data-ttu-id="724ab-416">Klicken Sie auf **STRG** + **S** zum Speichern der Datei.</span><span class="sxs-lookup"><span data-stu-id="724ab-416">Click **CTRL** + **S** to save the file.</span></span> <span data-ttu-id="724ab-417">Da die minimierte Datei bereits geöffnet ist, sehen Sie einem Dialogfeld darauf hingewiesen, dass die Datei außerhalb des Quellcode-Editors geändert wurde.</span><span class="sxs-lookup"><span data-stu-id="724ab-417">Since the minified file is already opened, you will see a dialog box saying that the file was modified outside of the source editor.</span></span> <span data-ttu-id="724ab-418">Klicken Sie auf **Ja**.</span><span class="sxs-lookup"><span data-stu-id="724ab-418">Click **Yes**.</span></span>

    <span data-ttu-id="724ab-419">![Microsoft Visual Studio-Warnung](visual-studio-2013-web-tools/_static/image51.png "Microsoft Visual Studio-Warnung")</span><span class="sxs-lookup"><span data-stu-id="724ab-419">![Microsoft Visual Studio warning](visual-studio-2013-web-tools/_static/image51.png "Microsoft Visual Studio warning")</span></span>

    <span data-ttu-id="724ab-420">*Microsoft Visual Studio-Warnung*</span><span class="sxs-lookup"><span data-stu-id="724ab-420">*Microsoft Visual Studio warning*</span></span>
19. <span data-ttu-id="724ab-421">Wechseln Sie zurück zu den **init.min.js** Datei, um sicherzustellen, dass die Datei mit den neuen Code aktualisiert wurde.</span><span class="sxs-lookup"><span data-stu-id="724ab-421">Switch back to the **init.min.js** file to verify that the file was updated with the new code.</span></span>

    <span data-ttu-id="724ab-422">![Init.Min.js-Datei aktualisiert](visual-studio-2013-web-tools/_static/image52.png "Init.min.js-Datei aktualisiert")</span><span class="sxs-lookup"><span data-stu-id="724ab-422">![Init.min.js file updated](visual-studio-2013-web-tools/_static/image52.png "Init.min.js file updated")</span></span>

    <span data-ttu-id="724ab-423">*Init.Min.js-Datei aktualisiert*</span><span class="sxs-lookup"><span data-stu-id="724ab-423">*Init.min.js file updated*</span></span>
20. <span data-ttu-id="724ab-424">Klicken Sie auf die **Browser Link aktualisieren** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="724ab-424">Click the **Browser Link Refresh** button.</span></span>
21. <span data-ttu-id="724ab-425">Sobald Sie beide Browser aktualisiert werden, werden automatisch Wiedergabe der Audioplayer, die Sie in der vorherigen Aufgabe haben gesehen, gestartet.</span><span class="sxs-lookup"><span data-stu-id="724ab-425">Once both browsers are refreshed the audio players you saw in the previous task will start playing automatically.</span></span>

    <span data-ttu-id="724ab-426">![In der Ansicht enthaltenen Audioplayer](visual-studio-2013-web-tools/_static/image53.png "Audio-Player, die in der Ansicht enthalten")</span><span class="sxs-lookup"><span data-stu-id="724ab-426">![Audio player included in view](visual-studio-2013-web-tools/_static/image53.png "Audio player included in view")</span></span>

    <span data-ttu-id="724ab-427">*In der Ansicht enthaltenen Audioplayer*</span><span class="sxs-lookup"><span data-stu-id="724ab-427">*Audio player included in view*</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="724ab-428">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="724ab-428">Summary</span></span>

<span data-ttu-id="724ab-429">Nach Abschluss dieser praktischen Übungseinheit Sie haben gelernt, wie Sie:</span><span class="sxs-lookup"><span data-stu-id="724ab-429">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="724ab-430">Verwenden Sie die neuen HTML-Editor-Features in Web Essentials z. B. rich-HTML5-Codeausschnitte und Zen-Codierung</span><span class="sxs-lookup"><span data-stu-id="724ab-430">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="724ab-431">Verwenden Sie die neue CSS-Editor-Funktionen, die im Web Essentials enthalten sind, z. B. die Farbauswahl angezeigt und die QuickInfo für Browser-matrix</span><span class="sxs-lookup"><span data-stu-id="724ab-431">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="724ab-432">Verwenden Sie die neue JavaScript-Editor-Funktionen, die im Web Essentials, z. B. extrahieren in die Datei und IntelliSense für alle HTML-Elemente enthalten</span><span class="sxs-lookup"><span data-stu-id="724ab-432">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="724ab-433">Austauschen von Daten zwischen Ihrem Browser, und Visual Studio verwenden einer Browserverknüpfung</span><span class="sxs-lookup"><span data-stu-id="724ab-433">Exchange data between your browser and Visual Studio using Browser Link</span></span>
