---
uid: single-page-application/overview/templates/hottowel-template
title: Im laufenden Systembetrieb Handtuch Vorlage | Microsoft Docs
author: madskristensen
description: HotTowel-Vorlage
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/09/2013
ms.topic: article
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: dbd037c2469d326a3d3248ca07492ed9eb93e225
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="hot-towel-template"></a><span data-ttu-id="16cea-103">Während des Betriebs Handtuch-Vorlage</span><span class="sxs-lookup"><span data-stu-id="16cea-103">Hot Towel template</span></span>
====================
<span data-ttu-id="16cea-104">durch [Mads Kristensen](https://github.com/madskristensen)</span><span class="sxs-lookup"><span data-stu-id="16cea-104">by [Mads Kristensen](https://github.com/madskristensen)</span></span>

> <span data-ttu-id="16cea-105">Die im laufenden Systembetrieb Handtuch MVC-Projektvorlage wird von John Papa geschrieben.</span><span class="sxs-lookup"><span data-stu-id="16cea-105">The Hot Towel MVC Template is written by John Papa</span></span>
> 
> <span data-ttu-id="16cea-106">Wählen Sie, welche Version zum Herunterladen:</span><span class="sxs-lookup"><span data-stu-id="16cea-106">Choose which version to download:</span></span>
> 
> [<span data-ttu-id="16cea-107">Im laufenden Systembetrieb Handtuch MVC-Vorlage für Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="16cea-107">Hot Towel MVC Template for Visual Studio 2012</span></span>](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [<span data-ttu-id="16cea-108">Im laufenden Systembetrieb Handtuch MVC-Vorlage für Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="16cea-108">Hot Towel MVC Template for Visual Studio 2013</span></span>](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> <span data-ttu-id="16cea-109">Im laufenden Systembetrieb Handtuch: Da Sie nicht, fahren Sie mit der SPA ohne eine möchten!</span><span class="sxs-lookup"><span data-stu-id="16cea-109">Hot Towel: Because you don't want to go to the SPA without one!</span></span>


<span data-ttu-id="16cea-110">Eine SPA erstellen möchten, aber keine Entscheidung getroffen werden, wo Sie beginnen?</span><span class="sxs-lookup"><span data-stu-id="16cea-110">Want to build a SPA but can't decide where to start?</span></span> <span data-ttu-id="16cea-111">Im laufenden Systembetrieb Handtuch verwenden und in Sekunden haben eine SPA und alle Tools, die zum Erstellen von darauf erforderlich!</span><span class="sxs-lookup"><span data-stu-id="16cea-111">Use Hot Towel and in seconds you'll have a SPA and all the tools you need to build on it!</span></span>

<span data-ttu-id="16cea-112">Im laufenden Systembetrieb Handtuch erstellt einen guter Ausgangspunkt für die Erstellung einer einzelnen Seite Anwendung (SPA) mit ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="16cea-112">Hot Towel creates a great starting point for building a Single Page Application (SPA) with ASP.NET.</span></span> <span data-ttu-id="16cea-113">Direktes finden Sie es für Ihre Code, Ansicht Navigation, Datenbindung, umfangreiche datenverwaltung und einfache, aber elegante formatieren eine modulare Struktur.</span><span class="sxs-lookup"><span data-stu-id="16cea-113">Out of the box you it provides a modular structure for your code, view navigation, data binding, rich data management and simple but elegant styling.</span></span> <span data-ttu-id="16cea-114">Im laufenden Systembetrieb Handtuch bietet alles, was Sie benötigen eine SPA erstellen, damit Sie auf der app, und nicht die zugrunde liegenden Funktionen konzentrieren können.</span><span class="sxs-lookup"><span data-stu-id="16cea-114">Hot Towel provides everything you need to build a SPA, so you can focus on your app, not the plumbing.</span></span>

> <span data-ttu-id="16cea-115">Weitere Informationen zum Erstellen einer SPA aus [John Papa des Videos, Lernprogramme und Pluralsight Kurse](http://johnpapa.net/spa?vsix).</span><span class="sxs-lookup"><span data-stu-id="16cea-115">Learn more about building a SPA from [John Papa's videos, tutorials and Pluralsight courses](http://johnpapa.net/spa?vsix).</span></span>


## <a name="application-structure"></a><span data-ttu-id="16cea-116">Anwendungsstruktur</span><span class="sxs-lookup"><span data-stu-id="16cea-116">Application Structure</span></span>

<span data-ttu-id="16cea-117">Im laufenden Systembetrieb Handtuch SPA bietet einen App-Ordner, der JavaScript und HTML-Dateien enthält, die in der Anwendung definiert.</span><span class="sxs-lookup"><span data-stu-id="16cea-117">Hot Towel SPA provides an App folder which contains the JavaScript and HTML files that define your application.</span></span>

<span data-ttu-id="16cea-118">Der Ordner "App":</span><span class="sxs-lookup"><span data-stu-id="16cea-118">Inside the App folder:</span></span>

- <span data-ttu-id="16cea-119">durandal</span><span class="sxs-lookup"><span data-stu-id="16cea-119">durandal</span></span>
- <span data-ttu-id="16cea-120">Dienste</span><span class="sxs-lookup"><span data-stu-id="16cea-120">services</span></span>
- <span data-ttu-id="16cea-121">viewmodels</span><span class="sxs-lookup"><span data-stu-id="16cea-121">viewmodels</span></span>
- <span data-ttu-id="16cea-122">Ansichten</span><span class="sxs-lookup"><span data-stu-id="16cea-122">views</span></span>

<span data-ttu-id="16cea-123">Der App-Ordner enthält eine Auflistung von Modulen.</span><span class="sxs-lookup"><span data-stu-id="16cea-123">The App folder contains a collection of modules.</span></span> <span data-ttu-id="16cea-124">Diese Module Funktionalität zu kapseln und Abhängigkeiten auf andere Module deklarieren.</span><span class="sxs-lookup"><span data-stu-id="16cea-124">These modules encapsulate functionality and declare dependencies on other modules.</span></span> <span data-ttu-id="16cea-125">Der Ordner Views enthält den HTML-Code für Ihre Anwendung und der Viewmodels-Ordner enthält die Präsentationslogik für Ansichten (ein allgemeines Muster MVVM).</span><span class="sxs-lookup"><span data-stu-id="16cea-125">The views folder contains the HTML for your application and the viewmodels folder contains the presentation logic for the views (a common MVVM pattern).</span></span> <span data-ttu-id="16cea-126">Der Ordner "Dienste" ist ideal für gemeinsame Dienste, die Ihre Anwendung z. B. HTTP-Datenabruf oder lokalen Speicher Interaktion möglicherweise Gehäuse.</span><span class="sxs-lookup"><span data-stu-id="16cea-126">The services folder is ideal for housing any common services that your application may need such as HTTP data retrieval or local storage interaction.</span></span> <span data-ttu-id="16cea-127">Es ist häufig mehrere Viewmodels Code aus den Modulen Dienst erneut zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="16cea-127">It is common for multiple viewmodels to re-use code from the service modules.</span></span>

## <a name="aspnet-mvc-server-side-application-structure"></a><span data-ttu-id="16cea-128">ASP.NET MVC Server Side Anwendungsstruktur</span><span class="sxs-lookup"><span data-stu-id="16cea-128">ASP.NET MVC Server Side Application Structure</span></span>

<span data-ttu-id="16cea-129">Im laufenden Systembetrieb Handtuch baut auf der vertraut und leistungsfähige ASP.NET MVC-Struktur.</span><span class="sxs-lookup"><span data-stu-id="16cea-129">Hot Towel builds on the familiar and powerful ASP.NET MVC structure.</span></span>

- <span data-ttu-id="16cea-130">App\_starten</span><span class="sxs-lookup"><span data-stu-id="16cea-130">App\_Start</span></span>
- <span data-ttu-id="16cea-131">Inhalt</span><span class="sxs-lookup"><span data-stu-id="16cea-131">Content</span></span>
- <span data-ttu-id="16cea-132">Controller</span><span class="sxs-lookup"><span data-stu-id="16cea-132">Controllers</span></span>
- <span data-ttu-id="16cea-133">Modelle</span><span class="sxs-lookup"><span data-stu-id="16cea-133">Models</span></span>
- <span data-ttu-id="16cea-134">Skripts</span><span class="sxs-lookup"><span data-stu-id="16cea-134">Scripts</span></span>
- <span data-ttu-id="16cea-135">Ansichten</span><span class="sxs-lookup"><span data-stu-id="16cea-135">Views</span></span>

## <a name="featured-libraries"></a><span data-ttu-id="16cea-136">Top-Bibliotheken</span><span class="sxs-lookup"><span data-stu-id="16cea-136">Featured Libraries</span></span>

- <span data-ttu-id="16cea-137">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="16cea-137">ASP.NET MVC</span></span>
- <span data-ttu-id="16cea-138">ASP.NET-Web-API</span><span class="sxs-lookup"><span data-stu-id="16cea-138">ASP.NET Web API</span></span>
- <span data-ttu-id="16cea-139">Optimierung der ASP.NET Web - Bündelung und Minimierung</span><span class="sxs-lookup"><span data-stu-id="16cea-139">ASP.NET Web Optimization - bundling and minification</span></span>
- <span data-ttu-id="16cea-140">[Breeze.js](http://Breezejs.com) -rich datenverwaltung</span><span class="sxs-lookup"><span data-stu-id="16cea-140">[Breeze.js](http://Breezejs.com) - rich data management</span></span>
- <span data-ttu-id="16cea-141">[Durandal.js](http://Durandaljs.com) -Navigation und Zusammenstellung</span><span class="sxs-lookup"><span data-stu-id="16cea-141">[Durandal.js](http://Durandaljs.com) - navigation and View composition</span></span>
- <span data-ttu-id="16cea-142">[Knockout.js](http://Knockoutjs.com) -datenbindungen</span><span class="sxs-lookup"><span data-stu-id="16cea-142">[Knockout.js](http://Knockoutjs.com) - data bindings</span></span>
- <span data-ttu-id="16cea-143">["Require.js"](http://requirejs.org) -Modularität mit AMD und Optimierung</span><span class="sxs-lookup"><span data-stu-id="16cea-143">[Require.js](http://requirejs.org) - Modularity with AMD and optimization</span></span>
- <span data-ttu-id="16cea-144">[Toastr.js](http://jpapa.me/c7toastr) -Popup-Nachrichten</span><span class="sxs-lookup"><span data-stu-id="16cea-144">[Toastr.js](http://jpapa.me/c7toastr) - pop-up messages</span></span>
- <span data-ttu-id="16cea-145">[Twitter-Bootstrap](http://twitter.github.com/bootstrap/) - stabile CSS-Stilen</span><span class="sxs-lookup"><span data-stu-id="16cea-145">[Twitter Bootstrap](http://twitter.github.com/bootstrap/) - robust CSS styling</span></span>

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a><span data-ttu-id="16cea-146">Installation über die Visual Studio 2012 im laufenden Systembetrieb Handtuch SPA-Vorlage</span><span class="sxs-lookup"><span data-stu-id="16cea-146">Installing via the Visual Studio 2012 Hot Towel SPA Template</span></span>

<span data-ttu-id="16cea-147">Im laufenden Systembetrieb Handtuch kann als eine Vorlage für Visual Studio 2012 installiert werden.</span><span class="sxs-lookup"><span data-stu-id="16cea-147">Hot Towel can be installed as a Visual Studio 2012 template.</span></span> <span data-ttu-id="16cea-148">Klicken Sie einfach auf `File`  |  `New Project` , und wählen Sie `ASP.NET MVC 4 Web Application`.</span><span class="sxs-lookup"><span data-stu-id="16cea-148">Just click `File` | `New Project` and choose `ASP.NET MVC 4 Web Application`.</span></span> <span data-ttu-id="16cea-149">Wählen Sie dann die "Hot Handtuch einseitigen Anwendung" Vorlage und ausgeführt!</span><span class="sxs-lookup"><span data-stu-id="16cea-149">Then select the 'Hot Towel Single Page Application" template and run!</span></span>

## <a name="installing-via-the-nuget-package"></a><span data-ttu-id="16cea-150">Installation über das NuGet-Paket</span><span class="sxs-lookup"><span data-stu-id="16cea-150">Installing via the NuGet Package</span></span>

<span data-ttu-id="16cea-151">Im laufenden Systembetrieb Handtuch ist auch ein NuGet-Paket, das ein vorhandenes leeres ASP.NET MVC-Projekt erhöht.</span><span class="sxs-lookup"><span data-stu-id="16cea-151">Hot Towel is also a NuGet package that augments an existing empty ASP.NET MVC project.</span></span> <span data-ttu-id="16cea-152">Installieren Sie einfach mithilfe von Nuget, und führen Sie aus!</span><span class="sxs-lookup"><span data-stu-id="16cea-152">Just install using Nuget and then run!</span></span>

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a><span data-ttu-id="16cea-153">Wie erstellen ich Hot Handtuch?</span><span class="sxs-lookup"><span data-stu-id="16cea-153">How Do I Build On Hot Towel?</span></span>

<span data-ttu-id="16cea-154">Hinzufügen von Code einfach zu starten!</span><span class="sxs-lookup"><span data-stu-id="16cea-154">Simply start adding code!</span></span>

1. <span data-ttu-id="16cea-155">Eigene serverseitige fügen Code hinzu, bevorzugt Entity Framework und WebAPI (die tatsächlich mit Breeze.js verwendet)</span><span class="sxs-lookup"><span data-stu-id="16cea-155">Add your own server-side code, preferably Entity Framework and WebAPI (which really shine with Breeze.js)</span></span>
2. <span data-ttu-id="16cea-156">Hinzufügen von Ansichten der `App/views` Ordner</span><span class="sxs-lookup"><span data-stu-id="16cea-156">Add views to the `App/views` folder</span></span>
3. <span data-ttu-id="16cea-157">Viewmodels zum Hinzufügen der `App/viewmodels` Ordner</span><span class="sxs-lookup"><span data-stu-id="16cea-157">Add viewmodels to the `App/viewmodels` folder</span></span>
4. <span data-ttu-id="16cea-158">Hinzufügen von HTML und Knockout datenbindungen zu Ihren neuen Ansichten</span><span class="sxs-lookup"><span data-stu-id="16cea-158">Add HTML and Knockout data bindings to your new views</span></span>
5. <span data-ttu-id="16cea-159">Aktualisieren Sie die Navigation Routen `shell.js`</span><span class="sxs-lookup"><span data-stu-id="16cea-159">Update the navigation routes in `shell.js`</span></span>

## <a name="walkthrough-of-the-htmljavascript"></a><span data-ttu-id="16cea-160">Exemplarische Vorgehensweise für die HTML/JavaScript</span><span class="sxs-lookup"><span data-stu-id="16cea-160">Walkthrough of the HTML/JavaScript</span></span>

### <a name="viewshottowelindexcshtml"></a><span data-ttu-id="16cea-161">Views/HotTowel/index.cshtml</span><span class="sxs-lookup"><span data-stu-id="16cea-161">Views/HotTowel/index.cshtml</span></span>

<span data-ttu-id="16cea-162">Index.cshtml ist das Route und die Ansicht für die MVC-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="16cea-162">index.cshtml is the starting route and view for the MVC application.</span></span> <span data-ttu-id="16cea-163">Sie enthält alle standard Meta-Tags, Links Css und JavaScript-Verweise, die Sie erwarten würden.</span><span class="sxs-lookup"><span data-stu-id="16cea-163">It contains all the standard meta tags, css links, and JavaScript references you would expect.</span></span> <span data-ttu-id="16cea-164">Der Nachrichtentext enthält ein einzelnes `<div>` ist, auf dem gesamten Inhalt (Ansichten) eingefügt wird, wenn sie angefordert werden.</span><span class="sxs-lookup"><span data-stu-id="16cea-164">The body contains a single `<div>` which is where all of the content (your views) will be placed when they are requested.</span></span> <span data-ttu-id="16cea-165">Die `@Scripts.Render` verwendet "Require.js" den Einstiegspunkt für den Anwendungscode ausführen im enthalten die `main.js` Datei.</span><span class="sxs-lookup"><span data-stu-id="16cea-165">The `@Scripts.Render` uses Require.js to run the entrance point for the application's code, which is contained in the `main.js` file.</span></span> <span data-ttu-id="16cea-166">Ein Begrüßungsbildschirm wird bereitgestellt, um die veranschaulichen, wie Sie einen Begrüßungsbildschirm erstellen, während Ihre app geladen wird.</span><span class="sxs-lookup"><span data-stu-id="16cea-166">A splash screen is provided to demonstrate how to create a splash screen while your app loads.</span></span>

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a><span data-ttu-id="16cea-167">App/Main.js</span><span class="sxs-lookup"><span data-stu-id="16cea-167">App/main.js</span></span>

<span data-ttu-id="16cea-168">Die `main.js` Datei enthält den Code, der ausgeführt wird, sobald die app geladen wird.</span><span class="sxs-lookup"><span data-stu-id="16cea-168">The `main.js` file contains the code that will run as soon as your app is loaded.</span></span> <span data-ttu-id="16cea-169">Dies ist, in denen Ihre Navigation Routen definieren, Festlegen Ihrer Startvorgangs Ansichten und führen Sie alle Setup/bootstrapping wie Ihre Anwendung Daten Einspielen werden sollen.</span><span class="sxs-lookup"><span data-stu-id="16cea-169">This is where you want to define your navigation routes, set your start up views, and perform any setup/bootstrapping such as priming your application's data.</span></span>

<span data-ttu-id="16cea-170">Die `main.js` -Datei definiert mehrere Durandal den Modulen der Anwendung Kick starten.</span><span class="sxs-lookup"><span data-stu-id="16cea-170">The `main.js` file defines several of durandal's modules to help the application kick start.</span></span> <span data-ttu-id="16cea-171">Define-Anweisung kann die Module Abhängigkeiten zu beheben, damit sie für die Funktion verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="16cea-171">The define statement helps resolve the modules dependencies so they are available for the function.</span></span> <span data-ttu-id="16cea-172">Zuerst werden die debugging-Meldungen aktiviert, das Senden von Nachrichten über welche Ereignisse, dass die Anwendung an das Konsolenfenster ausführt.</span><span class="sxs-lookup"><span data-stu-id="16cea-172">First the debugging messages are enabled, which send messages about what events the application is performing to the console window.</span></span> <span data-ttu-id="16cea-173">Der app.start-Code ermittelt Durandal-Framework, um die Anwendung zu starten.</span><span class="sxs-lookup"><span data-stu-id="16cea-173">The app.start code tells durandal framework to start the application.</span></span> <span data-ttu-id="16cea-174">Die Konventionen sind festgelegt, sodass Durandal alle Ansichten kennt und Viewmodels bzw. in die gleichen benannten Ordner enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="16cea-174">The conventions are set so that durandal knows all views and viewmodels are contained in the same named folders, respectively.</span></span> <span data-ttu-id="16cea-175">Schließlich die `app.setRoot` zum Einsatz kommt lädt die `shell` mithilfe eines vordefinierten `entrance` Animation.</span><span class="sxs-lookup"><span data-stu-id="16cea-175">Finally, the `app.setRoot` kicks loads the `shell` using a predefined `entrance` animation.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a><span data-ttu-id="16cea-176">Ansichten</span><span class="sxs-lookup"><span data-stu-id="16cea-176">Views</span></span>

<span data-ttu-id="16cea-177">Sichten befinden sich in der `App/views` Ordner.</span><span class="sxs-lookup"><span data-stu-id="16cea-177">Views are found in the `App/views` folder.</span></span>

### <a name="shellhtml"></a><span data-ttu-id="16cea-178">Shell.HTML</span><span class="sxs-lookup"><span data-stu-id="16cea-178">shell.html</span></span>

<span data-ttu-id="16cea-179">Die `shell.html` master Layout für den HTML-Code enthält.</span><span class="sxs-lookup"><span data-stu-id="16cea-179">The `shell.html` contains the master layout for your HTML.</span></span> <span data-ttu-id="16cea-180">Alle anderen Ansichten wird an einer beliebigen Stelle auf der Seite besteht aus Ihrer `shell` anzeigen.</span><span class="sxs-lookup"><span data-stu-id="16cea-180">All of your other views will be composed somewhere in side of your `shell` view.</span></span> <span data-ttu-id="16cea-181">Im laufenden Systembetrieb Handtuch bietet eine `shell` mit drei Bereichen: einen Header, einen Inhaltsbereich und eine Fußzeile.</span><span class="sxs-lookup"><span data-stu-id="16cea-181">Hot Towel provides a `shell` with three such regions: a header, a content area, and a footer.</span></span> <span data-ttu-id="16cea-182">Jede dieser Regionen mit geladen Inhalt zu anderen Ansichten für das angeforderte bilden.</span><span class="sxs-lookup"><span data-stu-id="16cea-182">Each of these regions is loaded with contents form other views when requested.</span></span>

<span data-ttu-id="16cea-183">Die `compose` Bindungen für die Kopf- und Fußzeile schwer hartcodiert sind in Hot Handtuch, zeigen Sie auf die `nav` und `footer` -Sicht bzw.</span><span class="sxs-lookup"><span data-stu-id="16cea-183">The `compose` bindings for the header and footer are hard coded in Hot Towel to point to the `nav` and `footer` views, respectively.</span></span> <span data-ttu-id="16cea-184">Die Compose-Bindung für den Abschnitt `#content` gebunden ist, um die `router` aktive Element des Moduls.</span><span class="sxs-lookup"><span data-stu-id="16cea-184">The compose binding for the section `#content` is bound to the `router` module's active item.</span></span> <span data-ttu-id="16cea-185">Das heißt, wenn Sie auf ein Navigationslink entsprechende Ansicht ist in diesem Bereich geladen wird.</span><span class="sxs-lookup"><span data-stu-id="16cea-185">In other words, when you click a navigation link it's corresponding view is loaded in this area.</span></span>

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a><span data-ttu-id="16cea-186">NAV.HTML</span><span class="sxs-lookup"><span data-stu-id="16cea-186">nav.html</span></span>

<span data-ttu-id="16cea-187">Die `nav.html` Navigationslinks für die SPA enthält.</span><span class="sxs-lookup"><span data-stu-id="16cea-187">The `nav.html` contains the navigation links for the SPA.</span></span> <span data-ttu-id="16cea-188">Dies ist, wo die Menüstruktur, z. B. platziert werden kann.</span><span class="sxs-lookup"><span data-stu-id="16cea-188">This is where the menu structure can be placed, for example.</span></span> <span data-ttu-id="16cea-189">Dies ist häufig Daten (mit Knockout) gebunden die `router` Modul, um die Navigation anzuzeigen Sie, in definiert der `shell.js`.</span><span class="sxs-lookup"><span data-stu-id="16cea-189">Often this is data bound (using Knockout) to the `router` module to display the navigation you defined in the `shell.js`.</span></span> <span data-ttu-id="16cea-190">Knockout sucht, die Data-Bind-Attribute und bindet diese an die `shell` Viewmodel die Navigation Routen anzeigen und eine "ProgressBar" (mit Twitter Bootstrap) anzuzeigen Wenn der `router` -Modul ist in der Mitte Navigieren von einer Sicht zu einem anderen (Siehe `router.isNavigating`).</span><span class="sxs-lookup"><span data-stu-id="16cea-190">Knockout looks for the data-bind attributes and binds those to the `shell` viewmodel to display the navigation routes and to show a progressbar (using Twitter Bootstrap) if the `router` module is in the middle of navigating from one view to another (see `router.isNavigating`).</span></span>

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a><span data-ttu-id="16cea-191">Datei "Home.HTML" und details.html</span><span class="sxs-lookup"><span data-stu-id="16cea-191">home.html and details.html</span></span>

<span data-ttu-id="16cea-192">Diese Sichten enthalten HTML für benutzerdefinierte Ansichten.</span><span class="sxs-lookup"><span data-stu-id="16cea-192">These views contain HTML for custom views.</span></span> <span data-ttu-id="16cea-193">Wenn der `home` link der `nav` Menü "Ansicht des" geklickt wird, der `home` Sicht befinden sich im Bereich Inhalt des der `shell` anzeigen.</span><span class="sxs-lookup"><span data-stu-id="16cea-193">When the `home` link in the `nav` view's menu is clicked, the `home` view will be placed in the content area of the `shell` view.</span></span> <span data-ttu-id="16cea-194">Diese Sichten können erweitert oder durch Ihre eigenen benutzerdefinierten Ansichten ersetzt werden.</span><span class="sxs-lookup"><span data-stu-id="16cea-194">These views can be augmented or replaced with your own custom views.</span></span>

### <a name="footerhtml"></a><span data-ttu-id="16cea-195">Footer.HTML</span><span class="sxs-lookup"><span data-stu-id="16cea-195">footer.html</span></span>

<span data-ttu-id="16cea-196">Die `footer.html` enthält HTML-Code, der in der Fußzeile am unteren Rand der `shell` anzeigen.</span><span class="sxs-lookup"><span data-stu-id="16cea-196">The `footer.html` contains HTML that appears in the footer, at the bottom of the `shell` view.</span></span>

## <a name="viewmodels"></a><span data-ttu-id="16cea-197">ViewModels</span><span class="sxs-lookup"><span data-stu-id="16cea-197">ViewModels</span></span>

<span data-ttu-id="16cea-198">ViewModels befinden sich in der `App/viewmodels` Ordner.</span><span class="sxs-lookup"><span data-stu-id="16cea-198">ViewModels are found in the `App/viewmodels` folder.</span></span>

### <a name="shelljs"></a><span data-ttu-id="16cea-199">Shell.js</span><span class="sxs-lookup"><span data-stu-id="16cea-199">shell.js</span></span>

<span data-ttu-id="16cea-200">Die `shell` Viewmodel enthält Eigenschaften und Funktionen, die gebunden sind die `shell` anzeigen.</span><span class="sxs-lookup"><span data-stu-id="16cea-200">The `shell` viewmodel contains properties and functions that are bound to the `shell` view.</span></span> <span data-ttu-id="16cea-201">Dies ist häufig, in denen die im Menü Navigation Bindungen gefunden werden (siehe die `router.mapNav` Logik).</span><span class="sxs-lookup"><span data-stu-id="16cea-201">Often this is where the menu navigation bindings are found (see the `router.mapNav` logic).</span></span>

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a><span data-ttu-id="16cea-202">"Home.js" und details.js</span><span class="sxs-lookup"><span data-stu-id="16cea-202">home.js and details.js</span></span>

<span data-ttu-id="16cea-203">Diese Viewmodels enthalten, die Eigenschaften und Funktionen, die gebunden sind die `home` anzeigen.</span><span class="sxs-lookup"><span data-stu-id="16cea-203">These viewmodels contain the properties and functions that are bound to the `home` view.</span></span> <span data-ttu-id="16cea-204">Außerdem enthält die Präsentationslogik für die Ansicht und ist die Verbindung zwischen den Daten und die Ansicht.</span><span class="sxs-lookup"><span data-stu-id="16cea-204">it also contains the presentation logic for the view, and is the glue between the data and the view.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a><span data-ttu-id="16cea-205">Dienste</span><span class="sxs-lookup"><span data-stu-id="16cea-205">Services</span></span>

<span data-ttu-id="16cea-206">Dienste befinden sich im Ordner "App-Dienste".</span><span class="sxs-lookup"><span data-stu-id="16cea-206">Services are found in the App/services folder.</span></span> <span data-ttu-id="16cea-207">Im Idealfall konnte Ihre zukünftige Dienste wie z. B. ein Modul "DataService", die für das Abrufen und Bereitstellen von Remotedaten zuständig ist, platziert werden.</span><span class="sxs-lookup"><span data-stu-id="16cea-207">Ideally your future services such as a dataservice module, that is responsible for getting and posting remote data, could be placed.</span></span>

### <a name="loggerjs"></a><span data-ttu-id="16cea-208">logger.js</span><span class="sxs-lookup"><span data-stu-id="16cea-208">logger.js</span></span>

<span data-ttu-id="16cea-209">Im laufenden Systembetrieb Handtuch bietet eine `logger` Modul im Ordner "Dienste".</span><span class="sxs-lookup"><span data-stu-id="16cea-209">Hot Towel provides a `logger` module in the services folder.</span></span> <span data-ttu-id="16cea-210">Die `logger` Modul eignet sich ideal zum Protokollieren von Nachrichten an die Konsole und in der Popup-Popups an den Benutzer.</span><span class="sxs-lookup"><span data-stu-id="16cea-210">The `logger` module is ideal for logging messages to the console and to the user in pop up toasts.</span></span>
