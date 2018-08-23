---
uid: single-page-application/overview/templates/hottowel-template
title: Hot Towel-Vorlage | Microsoft-Dokumentation
author: madskristensen
description: HotTowel-Vorlage
ms.author: riande
ms.date: 02/09/2013
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: b80b5940b5733a0ae2af3491ccdfa05b84b50343
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832791"
---
<a name="hot-towel-template"></a><span data-ttu-id="d1039-103">Hot Towel-Vorlage</span><span class="sxs-lookup"><span data-stu-id="d1039-103">Hot Towel template</span></span>
====================
<span data-ttu-id="d1039-104">durch [Mads Kristensen](https://github.com/madskristensen)</span><span class="sxs-lookup"><span data-stu-id="d1039-104">by [Mads Kristensen](https://github.com/madskristensen)</span></span>

> <span data-ttu-id="d1039-105">Die Hot Towel-MVC-Vorlage wird von John Papa geschrieben.</span><span class="sxs-lookup"><span data-stu-id="d1039-105">The Hot Towel MVC Template is written by John Papa</span></span>
> 
> <span data-ttu-id="d1039-106">Wählen Sie, welche Version Sie herunterladen:</span><span class="sxs-lookup"><span data-stu-id="d1039-106">Choose which version to download:</span></span>
> 
> [<span data-ttu-id="d1039-107">Hot Towel-MVC-Vorlage für Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="d1039-107">Hot Towel MVC Template for Visual Studio 2012</span></span>](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [<span data-ttu-id="d1039-108">Hot Towel-MVC-Vorlage für Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="d1039-108">Hot Towel MVC Template for Visual Studio 2013</span></span>](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> <span data-ttu-id="d1039-109">Hot Towel: Da Sie nicht, fahren Sie mit der einseitigen Anwendung ohne eine möchten!</span><span class="sxs-lookup"><span data-stu-id="d1039-109">Hot Towel: Because you don't want to go to the SPA without one!</span></span>


<span data-ttu-id="d1039-110">Erstellen einer SPA möchten, jedoch kann nicht entscheiden, wo Sie anfangen?</span><span class="sxs-lookup"><span data-stu-id="d1039-110">Want to build a SPA but can't decide where to start?</span></span> <span data-ttu-id="d1039-111">Hot Towel verwenden und in Sekunden müssen Sie eine SPA und alle Tools, die zum Erstellen von darauf erforderlich!</span><span class="sxs-lookup"><span data-stu-id="d1039-111">Use Hot Towel and in seconds you'll have a SPA and all the tools you need to build on it!</span></span>

<span data-ttu-id="d1039-112">Hot Towel erstellt einen guter Ausgangspunkt für eine Single-Page Application (SPA) mit ASP.NET erstellen.</span><span class="sxs-lookup"><span data-stu-id="d1039-112">Hot Towel creates a great starting point for building a Single Page Application (SPA) with ASP.NET.</span></span> <span data-ttu-id="d1039-113">Standardmäßig bietet es eine modulare Struktur für Code, Navigation in der, die Datenbindung, umfangreiche datenverwaltung und einfache, aber elegante formatieren.</span><span class="sxs-lookup"><span data-stu-id="d1039-113">Out of the box you it provides a modular structure for your code, view navigation, data binding, rich data management and simple but elegant styling.</span></span> <span data-ttu-id="d1039-114">Hot Towel bietet alles, was Sie benötigen, um einer einseitigen Anwendung zu erstellen, damit Sie in Ihrer app, die nicht die Infrastruktur konzentrieren können.</span><span class="sxs-lookup"><span data-stu-id="d1039-114">Hot Towel provides everything you need to build a SPA, so you can focus on your app, not the plumbing.</span></span>

> <span data-ttu-id="d1039-115">Erfahren Sie mehr über das Erstellen einer SPA aus [John Papa des Videos, Lernprogramme und Pluralsight-Kurse](http://johnpapa.net/spa?vsix).</span><span class="sxs-lookup"><span data-stu-id="d1039-115">Learn more about building a SPA from [John Papa's videos, tutorials and Pluralsight courses](http://johnpapa.net/spa?vsix).</span></span>


## <a name="application-structure"></a><span data-ttu-id="d1039-116">Anwendungsstruktur</span><span class="sxs-lookup"><span data-stu-id="d1039-116">Application Structure</span></span>

<span data-ttu-id="d1039-117">Hot Towel-SPA enthält einen App-Ordner enthält die JavaScript und HTML-Dateien, die Ihre Anwendung zu definieren.</span><span class="sxs-lookup"><span data-stu-id="d1039-117">Hot Towel SPA provides an App folder which contains the JavaScript and HTML files that define your application.</span></span>

<span data-ttu-id="d1039-118">In den App-Ordner:</span><span class="sxs-lookup"><span data-stu-id="d1039-118">Inside the App folder:</span></span>

- <span data-ttu-id="d1039-119">durandal</span><span class="sxs-lookup"><span data-stu-id="d1039-119">durandal</span></span>
- <span data-ttu-id="d1039-120">Dienste</span><span class="sxs-lookup"><span data-stu-id="d1039-120">services</span></span>
- <span data-ttu-id="d1039-121">ViewModels</span><span class="sxs-lookup"><span data-stu-id="d1039-121">viewmodels</span></span>
- <span data-ttu-id="d1039-122">Ansichten</span><span class="sxs-lookup"><span data-stu-id="d1039-122">views</span></span>

<span data-ttu-id="d1039-123">Der App-Ordner enthält eine Auflistung von Modulen.</span><span class="sxs-lookup"><span data-stu-id="d1039-123">The App folder contains a collection of modules.</span></span> <span data-ttu-id="d1039-124">Diese Module Kapseln der Funktionalität und Abhängigkeiten von anderen Modulen deklarieren.</span><span class="sxs-lookup"><span data-stu-id="d1039-124">These modules encapsulate functionality and declare dependencies on other modules.</span></span> <span data-ttu-id="d1039-125">Ordner "Views" enthält den HTML-Code für Ihre Anwendung aus, und der Ordner "Viewmodels" enthält die Darstellungslogik für die Ansichten (eine allgemeine MVVM-Muster).</span><span class="sxs-lookup"><span data-stu-id="d1039-125">The views folder contains the HTML for your application and the viewmodels folder contains the presentation logic for the views (a common MVVM pattern).</span></span> <span data-ttu-id="d1039-126">Ordner "Dienste" ist ideal für gemeinsame Dienste, die Ihre Anwendung z. B. HTTP-Daten abrufen "oder" Lokaler Speicher Interaktion möglicherweise speichern.</span><span class="sxs-lookup"><span data-stu-id="d1039-126">The services folder is ideal for housing any common services that your application may need such as HTTP data retrieval or local storage interaction.</span></span> <span data-ttu-id="d1039-127">Es ist üblich, dass mehrere Viewmodels Wiederverwendungsrate für Code aus den Service-Modulen.</span><span class="sxs-lookup"><span data-stu-id="d1039-127">It is common for multiple viewmodels to re-use code from the service modules.</span></span>

## <a name="aspnet-mvc-server-side-application-structure"></a><span data-ttu-id="d1039-128">ASP.NET MVC-Server-Seite-Anwendungsstruktur</span><span class="sxs-lookup"><span data-stu-id="d1039-128">ASP.NET MVC Server Side Application Structure</span></span>

<span data-ttu-id="d1039-129">Hot Towel baut auf der vertraut und leistungsstarke ASP.NET MVC-Struktur.</span><span class="sxs-lookup"><span data-stu-id="d1039-129">Hot Towel builds on the familiar and powerful ASP.NET MVC structure.</span></span>

- <span data-ttu-id="d1039-130">App\_starten</span><span class="sxs-lookup"><span data-stu-id="d1039-130">App\_Start</span></span>
- <span data-ttu-id="d1039-131">Inhalt</span><span class="sxs-lookup"><span data-stu-id="d1039-131">Content</span></span>
- <span data-ttu-id="d1039-132">Controller</span><span class="sxs-lookup"><span data-stu-id="d1039-132">Controllers</span></span>
- <span data-ttu-id="d1039-133">Modelle</span><span class="sxs-lookup"><span data-stu-id="d1039-133">Models</span></span>
- <span data-ttu-id="d1039-134">Skripts</span><span class="sxs-lookup"><span data-stu-id="d1039-134">Scripts</span></span>
- <span data-ttu-id="d1039-135">Ansichten</span><span class="sxs-lookup"><span data-stu-id="d1039-135">Views</span></span>

## <a name="featured-libraries"></a><span data-ttu-id="d1039-136">Ausgewählte Bibliotheken</span><span class="sxs-lookup"><span data-stu-id="d1039-136">Featured Libraries</span></span>

- <span data-ttu-id="d1039-137">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="d1039-137">ASP.NET MVC</span></span>
- <span data-ttu-id="d1039-138">ASP.NET-Web-API</span><span class="sxs-lookup"><span data-stu-id="d1039-138">ASP.NET Web API</span></span>
- <span data-ttu-id="d1039-139">ASP.NET Web Optimization - Bündelung und Minimierung</span><span class="sxs-lookup"><span data-stu-id="d1039-139">ASP.NET Web Optimization - bundling and minification</span></span>
- <span data-ttu-id="d1039-140">[Breeze.js](http://Breezejs.com) -Verwaltung von umfangreichen Daten</span><span class="sxs-lookup"><span data-stu-id="d1039-140">[Breeze.js](http://Breezejs.com) - rich data management</span></span>
- <span data-ttu-id="d1039-141">[Durandal.js](http://Durandaljs.com) -Navigation und Aufbau der Ansicht</span><span class="sxs-lookup"><span data-stu-id="d1039-141">[Durandal.js](http://Durandaljs.com) - navigation and View composition</span></span>
- <span data-ttu-id="d1039-142">["Knockout.js"](http://Knockoutjs.com) -datenbindungen</span><span class="sxs-lookup"><span data-stu-id="d1039-142">[Knockout.js](http://Knockoutjs.com) - data bindings</span></span>
- <span data-ttu-id="d1039-143">["Require.js"](http://requirejs.org) -Modularität mit AMD und Optimierung</span><span class="sxs-lookup"><span data-stu-id="d1039-143">[Require.js](http://requirejs.org) - Modularity with AMD and optimization</span></span>
- <span data-ttu-id="d1039-144">[Toastr.js](http://jpapa.me/c7toastr) -Popup-Nachrichten</span><span class="sxs-lookup"><span data-stu-id="d1039-144">[Toastr.js](http://jpapa.me/c7toastr) - pop-up messages</span></span>
- <span data-ttu-id="d1039-145">[Twitter-Bootstrap](http://twitter.github.com/bootstrap/) – stabile CSS-Stile</span><span class="sxs-lookup"><span data-stu-id="d1039-145">[Twitter Bootstrap](http://twitter.github.com/bootstrap/) - robust CSS styling</span></span>

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a><span data-ttu-id="d1039-146">Installation über die Visual Studio 2012 Hot Towel-SPA-Vorlage</span><span class="sxs-lookup"><span data-stu-id="d1039-146">Installing via the Visual Studio 2012 Hot Towel SPA Template</span></span>

<span data-ttu-id="d1039-147">Hot Towel kann als Vorlage Visual Studio 2012 installiert werden.</span><span class="sxs-lookup"><span data-stu-id="d1039-147">Hot Towel can be installed as a Visual Studio 2012 template.</span></span> <span data-ttu-id="d1039-148">Klicken Sie einfach auf `File`  |  `New Project` , und wählen Sie `ASP.NET MVC 4 Web Application`.</span><span class="sxs-lookup"><span data-stu-id="d1039-148">Just click `File` | `New Project` and choose `ASP.NET MVC 4 Web Application`.</span></span> <span data-ttu-id="d1039-149">Wählen Sie dann den "Hot Towel Single Page Application"-Vorlage und führen Sie!</span><span class="sxs-lookup"><span data-stu-id="d1039-149">Then select the 'Hot Towel Single Page Application" template and run!</span></span>

## <a name="installing-via-the-nuget-package"></a><span data-ttu-id="d1039-150">Über das NuGet-Paket installieren</span><span class="sxs-lookup"><span data-stu-id="d1039-150">Installing via the NuGet Package</span></span>

<span data-ttu-id="d1039-151">Hot Towel ist auch ein NuGet-Paket, das Standardfehlerprotokoll, ein vorhandenes leeres ASP.NET MVC-Projekt erweitert.</span><span class="sxs-lookup"><span data-stu-id="d1039-151">Hot Towel is also a NuGet package that augments an existing empty ASP.NET MVC project.</span></span> <span data-ttu-id="d1039-152">Installieren Sie einfach mithilfe von Nuget aus, und führen Sie!</span><span class="sxs-lookup"><span data-stu-id="d1039-152">Just install using Nuget and then run!</span></span>

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a><span data-ttu-id="d1039-153">Wie erstelle ich die Hot Towel?</span><span class="sxs-lookup"><span data-stu-id="d1039-153">How Do I Build On Hot Towel?</span></span>

<span data-ttu-id="d1039-154">Beginnen Sie einfach, Code hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="d1039-154">Simply start adding code!</span></span>

1. <span data-ttu-id="d1039-155">Fügen Sie eigener serverseitigem Code, vorzugsweise Entity Framework und Web-API (was wirklich mit Breeze.js verwendet hinzu)</span><span class="sxs-lookup"><span data-stu-id="d1039-155">Add your own server-side code, preferably Entity Framework and WebAPI (which really shine with Breeze.js)</span></span>
2. <span data-ttu-id="d1039-156">Hinzufügen von Ansichten, die `App/views` Ordner</span><span class="sxs-lookup"><span data-stu-id="d1039-156">Add views to the `App/views` folder</span></span>
3. <span data-ttu-id="d1039-157">Hinzufügen von Viewmodels die `App/viewmodels` Ordner</span><span class="sxs-lookup"><span data-stu-id="d1039-157">Add viewmodels to the `App/viewmodels` folder</span></span>
4. <span data-ttu-id="d1039-158">Hinzufügen von HTML-"und" Knockout-datenbindungen an die neuen Ansichten</span><span class="sxs-lookup"><span data-stu-id="d1039-158">Add HTML and Knockout data bindings to your new views</span></span>
5. <span data-ttu-id="d1039-159">Aktualisieren Sie die Navigation Routen in `shell.js`</span><span class="sxs-lookup"><span data-stu-id="d1039-159">Update the navigation routes in `shell.js`</span></span>

## <a name="walkthrough-of-the-htmljavascript"></a><span data-ttu-id="d1039-160">Exemplarische Vorgehensweise für die HTML-/JavaScript</span><span class="sxs-lookup"><span data-stu-id="d1039-160">Walkthrough of the HTML/JavaScript</span></span>

### <a name="viewshottowelindexcshtml"></a><span data-ttu-id="d1039-161">Views/HotTowel/index.cshtml</span><span class="sxs-lookup"><span data-stu-id="d1039-161">Views/HotTowel/index.cshtml</span></span>

<span data-ttu-id="d1039-162">Datei "Index.cshtml" ist das Weiterleiten und die Ansicht für die MVC-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="d1039-162">index.cshtml is the starting route and view for the MVC application.</span></span> <span data-ttu-id="d1039-163">Sie enthält alle standard-Meta-Tags, CSS-Links, und JavaScript-Verweise, die Sie erwarten.</span><span class="sxs-lookup"><span data-stu-id="d1039-163">It contains all the standard meta tags, css links, and JavaScript references you would expect.</span></span> <span data-ttu-id="d1039-164">Der Textkörper enthält ein einzelnes `<div>` der ist, in dem der gesamte Inhalt (Ihre Ansichten) platziert wird, wenn diese angefordert werden.</span><span class="sxs-lookup"><span data-stu-id="d1039-164">The body contains a single `<div>` which is where all of the content (your views) will be placed when they are requested.</span></span> <span data-ttu-id="d1039-165">Die `@Scripts.Render` "Require.js" verwendet, um den Einstiegspunkt für den Code der Anwendung, der in enthalten ist, führen die `main.js` Datei.</span><span class="sxs-lookup"><span data-stu-id="d1039-165">The `@Scripts.Render` uses Require.js to run the entrance point for the application's code, which is contained in the `main.js` file.</span></span> <span data-ttu-id="d1039-166">Ein Begrüßungsbildschirm wird bereitgestellt, um zu veranschaulichen, wie Sie die Anzeige eines Begrüßungsbildschirms zu erstellen, während Ihre app geladen wird.</span><span class="sxs-lookup"><span data-stu-id="d1039-166">A splash screen is provided to demonstrate how to create a splash screen while your app loads.</span></span>

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a><span data-ttu-id="d1039-167">App/Main.js</span><span class="sxs-lookup"><span data-stu-id="d1039-167">App/main.js</span></span>

<span data-ttu-id="d1039-168">Die `main.js` Datei enthält den Code, der ausgeführt wird, sobald Ihre app geladen wird.</span><span class="sxs-lookup"><span data-stu-id="d1039-168">The `main.js` file contains the code that will run as soon as your app is loaded.</span></span> <span data-ttu-id="d1039-169">Dies ist, in denen Sie definieren die Routen für die Navigation, starten Sie die Ansichten festlegen und führen Sie alle Setup/bootstrapping z. B. die Daten Ihrer Anwendung vorbereiten möchten.</span><span class="sxs-lookup"><span data-stu-id="d1039-169">This is where you want to define your navigation routes, set your start up views, and perform any setup/bootstrapping such as priming your application's data.</span></span>

<span data-ttu-id="d1039-170">Die `main.js` -Datei definiert mehrere Durandal Modulen der Anwendung Kick-start.</span><span class="sxs-lookup"><span data-stu-id="d1039-170">The `main.js` file defines several of durandal's modules to help the application kick start.</span></span> <span data-ttu-id="d1039-171">Die Anweisung definieren kann Module Abhängigkeiten aufgelöst werden, und sie für die Funktion verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="d1039-171">The define statement helps resolve the modules dependencies so they are available for the function.</span></span> <span data-ttu-id="d1039-172">Zunächst werden die debugging-Meldungen aktiviert, das Senden von Nachrichten über welche Ereignisse, dass es sich bei der Anwendung an das Konsolenfenster ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="d1039-172">First the debugging messages are enabled, which send messages about what events the application is performing to the console window.</span></span> <span data-ttu-id="d1039-173">Der app.start-Code weist Durandal-Framework, um die Anwendung zu starten.</span><span class="sxs-lookup"><span data-stu-id="d1039-173">The app.start code tells durandal framework to start the application.</span></span> <span data-ttu-id="d1039-174">Die Konventionen festgelegt, sodass Durandal, alle Ansichten weiß und Viewmodels bzw. in der gleichen benannten Ordner enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="d1039-174">The conventions are set so that durandal knows all views and viewmodels are contained in the same named folders, respectively.</span></span> <span data-ttu-id="d1039-175">Schließlich die `app.setRoot` startet lädt die `shell` mithilfe eines vordefinierten `entrance` Animation.</span><span class="sxs-lookup"><span data-stu-id="d1039-175">Finally, the `app.setRoot` kicks loads the `shell` using a predefined `entrance` animation.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a><span data-ttu-id="d1039-176">Ansichten</span><span class="sxs-lookup"><span data-stu-id="d1039-176">Views</span></span>

<span data-ttu-id="d1039-177">Ansichten finden Sie in der `App/views` Ordner.</span><span class="sxs-lookup"><span data-stu-id="d1039-177">Views are found in the `App/views` folder.</span></span>

### <a name="shellhtml"></a><span data-ttu-id="d1039-178">Shell.HTML</span><span class="sxs-lookup"><span data-stu-id="d1039-178">shell.html</span></span>

<span data-ttu-id="d1039-179">Die `shell.html` das Masterlayout für den HTML-Code enthält.</span><span class="sxs-lookup"><span data-stu-id="d1039-179">The `shell.html` contains the master layout for your HTML.</span></span> <span data-ttu-id="d1039-180">Alle anderen Ansichten besteht, an einer beliebigen Stelle in der Komponente, die von Ihrem `shell` anzeigen.</span><span class="sxs-lookup"><span data-stu-id="d1039-180">All of your other views will be composed somewhere in side of your `shell` view.</span></span> <span data-ttu-id="d1039-181">Hot Towel bietet eine `shell` mit drei Bereichen: ein Header, einen Inhaltsbereich und eine Fußzeile.</span><span class="sxs-lookup"><span data-stu-id="d1039-181">Hot Towel provides a `shell` with three such regions: a header, a content area, and a footer.</span></span> <span data-ttu-id="d1039-182">Jede dieser Regionen wird geladen, mit Inhalt zu anderen Ansichten für das angeforderte bilden.</span><span class="sxs-lookup"><span data-stu-id="d1039-182">Each of these regions is loaded with contents form other views when requested.</span></span>

<span data-ttu-id="d1039-183">Die `compose` Bindungen für die Kopf- und Fußzeile sind fest codiert wird, im Hot Towel, zeigen Sie auf die `nav` und `footer` -Sicht bzw.</span><span class="sxs-lookup"><span data-stu-id="d1039-183">The `compose` bindings for the header and footer are hard coded in Hot Towel to point to the `nav` and `footer` views, respectively.</span></span> <span data-ttu-id="d1039-184">Die Compose-Bindung für den Abschnitt `#content` gebunden ist, um die `router` aktives Element des Moduls.</span><span class="sxs-lookup"><span data-stu-id="d1039-184">The compose binding for the section `#content` is bound to the `router` module's active item.</span></span> <span data-ttu-id="d1039-185">Das heißt, wenn Sie auf ein Navigationslink, die sie die entsprechende Ansicht ist in diesem Bereich geladen wird.</span><span class="sxs-lookup"><span data-stu-id="d1039-185">In other words, when you click a navigation link it's corresponding view is loaded in this area.</span></span>

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a><span data-ttu-id="d1039-186">NAV.HTML</span><span class="sxs-lookup"><span data-stu-id="d1039-186">nav.html</span></span>

<span data-ttu-id="d1039-187">Die `nav.html` Navigationslinks für die SPA enthält.</span><span class="sxs-lookup"><span data-stu-id="d1039-187">The `nav.html` contains the navigation links for the SPA.</span></span> <span data-ttu-id="d1039-188">Dies ist, in denen die Menüstruktur, z. B. platziert werden kann.</span><span class="sxs-lookup"><span data-stu-id="d1039-188">This is where the menu structure can be placed, for example.</span></span> <span data-ttu-id="d1039-189">Dies ist häufig Daten gebunden (mithilfe von Knockout) auf die `router` Modul, um die Navigation anzuzeigen, die Sie in definiert, die `shell.js`.</span><span class="sxs-lookup"><span data-stu-id="d1039-189">Often this is data bound (using Knockout) to the `router` module to display the navigation you defined in the `shell.js`.</span></span> <span data-ttu-id="d1039-190">Knockout sieht aus, für die Datenbindung-Attribute und bindet an der `shell` "ViewModel", um die Navigation Routen anzuzeigen und Progressbar (mithilfe von Twitter Bootstrap) anzuzeigen Wenn der `router` -Modul ist in der Mitte Navigieren von einer Ansicht zu einem anderen (Siehe `router.isNavigating`).</span><span class="sxs-lookup"><span data-stu-id="d1039-190">Knockout looks for the data-bind attributes and binds those to the `shell` viewmodel to display the navigation routes and to show a progressbar (using Twitter Bootstrap) if the `router` module is in the middle of navigating from one view to another (see `router.isNavigating`).</span></span>

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a><span data-ttu-id="d1039-191">Datei "Home.HTML" und details.html</span><span class="sxs-lookup"><span data-stu-id="d1039-191">home.html and details.html</span></span>

<span data-ttu-id="d1039-192">Diese Ansichten werden HTML-Code für benutzerdefinierte Ansichten enthalten.</span><span class="sxs-lookup"><span data-stu-id="d1039-192">These views contain HTML for custom views.</span></span> <span data-ttu-id="d1039-193">Wenn die `home` -link in der `nav` Menü "Ansicht" geklickt wird, die `home` im Bereich der Ansicht platziert werden die `shell` anzeigen.</span><span class="sxs-lookup"><span data-stu-id="d1039-193">When the `home` link in the `nav` view's menu is clicked, the `home` view will be placed in the content area of the `shell` view.</span></span> <span data-ttu-id="d1039-194">Diese Sichten können erweitert oder durch Ihre eigenen benutzerdefinierten Ansichten ersetzt werden.</span><span class="sxs-lookup"><span data-stu-id="d1039-194">These views can be augmented or replaced with your own custom views.</span></span>

### <a name="footerhtml"></a><span data-ttu-id="d1039-195">Footer.HTML</span><span class="sxs-lookup"><span data-stu-id="d1039-195">footer.html</span></span>

<span data-ttu-id="d1039-196">Die `footer.html` enthält HTML-Code, der in der Fußzeile am unteren Rand angezeigt wird. die `shell` anzeigen.</span><span class="sxs-lookup"><span data-stu-id="d1039-196">The `footer.html` contains HTML that appears in the footer, at the bottom of the `shell` view.</span></span>

## <a name="viewmodels"></a><span data-ttu-id="d1039-197">ViewModels</span><span class="sxs-lookup"><span data-stu-id="d1039-197">ViewModels</span></span>

<span data-ttu-id="d1039-198">ViewModels finden Sie in der `App/viewmodels` Ordner.</span><span class="sxs-lookup"><span data-stu-id="d1039-198">ViewModels are found in the `App/viewmodels` folder.</span></span>

### <a name="shelljs"></a><span data-ttu-id="d1039-199">Shell.js</span><span class="sxs-lookup"><span data-stu-id="d1039-199">shell.js</span></span>

<span data-ttu-id="d1039-200">Die `shell` Viewmodel enthält Eigenschaften und Funktionen, die an gebunden sind, die `shell` anzeigen.</span><span class="sxs-lookup"><span data-stu-id="d1039-200">The `shell` viewmodel contains properties and functions that are bound to the `shell` view.</span></span> <span data-ttu-id="d1039-201">Dies ist häufig, in denen die Menü-Navigation-Bindungen gefunden werden (siehe die `router.mapNav` Logik).</span><span class="sxs-lookup"><span data-stu-id="d1039-201">Often this is where the menu navigation bindings are found (see the `router.mapNav` logic).</span></span>

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a><span data-ttu-id="d1039-202">"Home.js" und details.js</span><span class="sxs-lookup"><span data-stu-id="d1039-202">home.js and details.js</span></span>

<span data-ttu-id="d1039-203">Diese Viewmodels enthalten die Eigenschaften und Funktionen, die an gebunden sind, die `home` anzeigen.</span><span class="sxs-lookup"><span data-stu-id="d1039-203">These viewmodels contain the properties and functions that are bound to the `home` view.</span></span> <span data-ttu-id="d1039-204">Außerdem enthält die Darstellungslogik für die Ansicht, und ist der Leim zwischen den Daten und die Ansicht.</span><span class="sxs-lookup"><span data-stu-id="d1039-204">it also contains the presentation logic for the view, and is the glue between the data and the view.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a><span data-ttu-id="d1039-205">Dienste</span><span class="sxs-lookup"><span data-stu-id="d1039-205">Services</span></span>

<span data-ttu-id="d1039-206">Dienste befinden sich im Ordner "App-Dienste".</span><span class="sxs-lookup"><span data-stu-id="d1039-206">Services are found in the App/services folder.</span></span> <span data-ttu-id="d1039-207">Im Idealfall könnte Ihre zukünftige Dienste wie z. B. einem Modul "DataService", die für das Abrufen und Bereitstellen von remote-Daten verantwortlich ist, platziert werden.</span><span class="sxs-lookup"><span data-stu-id="d1039-207">Ideally your future services such as a dataservice module, that is responsible for getting and posting remote data, could be placed.</span></span>

### <a name="loggerjs"></a><span data-ttu-id="d1039-208">logger.js</span><span class="sxs-lookup"><span data-stu-id="d1039-208">logger.js</span></span>

<span data-ttu-id="d1039-209">Hot Towel bietet eine `logger` Modul im Ordner "Dienste".</span><span class="sxs-lookup"><span data-stu-id="d1039-209">Hot Towel provides a `logger` module in the services folder.</span></span> <span data-ttu-id="d1039-210">Die `logger` Modul ist ideal zum Protokollieren von Meldungen an die Konsole und dem Benutzer im Popup-Popups.</span><span class="sxs-lookup"><span data-stu-id="d1039-210">The `logger` module is ideal for logging messages to the console and to the user in pop up toasts.</span></span>
