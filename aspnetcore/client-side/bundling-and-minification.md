---
title: Bundling und Minimierung in ASP.NET Core
author: scottaddie
description: "Erfahren Sie, wie statische Ressourcen in einer ASP.NET Core-Web-Anwendung zu optimieren, indem Sie Bündelung und Minimierung Techniken anwenden."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/01/2017
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: client-side/bundling-and-minification
ms.openlocfilehash: c271b7ef386bacedbd45fbe9f62c9c486db55b36
ms.sourcegitcommit: 05e798c9bac7b9e9983599afb227ef393905d023
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2017
---
# <a name="bundling-and-minification"></a><span data-ttu-id="42a37-103">Bundling und Minimierung</span><span class="sxs-lookup"><span data-stu-id="42a37-103">Bundling and minification</span></span>

<span data-ttu-id="42a37-104">Von [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="42a37-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="42a37-105">Dieser Artikel beschreibt die Vorteile der Anwendung Bündelung und Minimierung, z. B., wie diese Funktionen mit ASP.NET Core webapps verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="42a37-105">This article explains the benefits of applying bundling and minification, including how these features can be used with ASP.NET Core web apps.</span></span>

## <a name="what-is-bundling-and-minification"></a><span data-ttu-id="42a37-106">Was ist Bündelung und Minimierung?</span><span class="sxs-lookup"><span data-stu-id="42a37-106">What is bundling and minification?</span></span>

<span data-ttu-id="42a37-107">Bundling und Minimierung sind zwei unterschiedliche leistungsoptimierungen, die Sie in einer Web-app anwenden können.</span><span class="sxs-lookup"><span data-stu-id="42a37-107">Bundling and minification are two distinct performance optimizations you can apply in a web app.</span></span> <span data-ttu-id="42a37-108">Zusammen verwendet werden, zur Leistungssteigerung Bündelung und Minimierung durch Reduzierung der Anzahl von serveranforderungen und Verringern der Größe der angeforderten statischen Objekte.</span><span class="sxs-lookup"><span data-stu-id="42a37-108">Used together, bundling and minification improve performance by reducing the number of server requests and reducing the size of the requested static assets.</span></span>

<span data-ttu-id="42a37-109">Bundling und Minimierung wird in erster Linie die erste Anforderung Seitenladezeit verbessern.</span><span class="sxs-lookup"><span data-stu-id="42a37-109">Bundling and minification primarily improve the first page request load time.</span></span> <span data-ttu-id="42a37-110">Sobald eine Webseite angefordert wurde, speichert der Browser statische Objekte (JavaScript, CSS und Bilder).</span><span class="sxs-lookup"><span data-stu-id="42a37-110">Once a web page has been requested, the browser caches the static assets (JavaScript, CSS, and images).</span></span> <span data-ttu-id="42a37-111">Folglich leistungsverbesserung nicht Bündelung und Minimierung beim Anfordern von der gleichen Seite oder Seiten am selben Standort die gleichen Ressourcen anfordern.</span><span class="sxs-lookup"><span data-stu-id="42a37-111">Consequently, bundling and minification don't improve performance when requesting the same page, or pages, on the same site requesting the same assets.</span></span> <span data-ttu-id="42a37-112">Wenn Sie nicht Festlegen der Header ordnungsgemäß auf Ihre Medienobjekte abläuft, und Sie Bündelung und Minimierung nicht verwenden, der Browser Aktualität Heuristik zu kennzeichnen, die Anlagen veraltete nach ein paar Tagen.</span><span class="sxs-lookup"><span data-stu-id="42a37-112">If you don't set the expires header correctly on your assets, and if you don’t use bundling and minification, the browser's freshness heuristics mark the assets stale after a few days.</span></span> <span data-ttu-id="42a37-113">Darüber hinaus muss der Browser eine validierungsanforderung für jedes Medienobjekt.</span><span class="sxs-lookup"><span data-stu-id="42a37-113">Additionally, the browser requires a validation request for each asset.</span></span> <span data-ttu-id="42a37-114">In diesem Fall geben Bündelung und Minimierung verbessert die Leistung auch nach der ersten Seitenanforderung.</span><span class="sxs-lookup"><span data-stu-id="42a37-114">In this case, bundling and minification provide a performance improvement even after the first page request.</span></span>

### <a name="bundling"></a><span data-ttu-id="42a37-115">Bundling</span><span class="sxs-lookup"><span data-stu-id="42a37-115">Bundling</span></span>

<span data-ttu-id="42a37-116">Bundling kombiniert mehrere Dateien in einer einzelnen Datei.</span><span class="sxs-lookup"><span data-stu-id="42a37-116">Bundling combines multiple files into a single file.</span></span> <span data-ttu-id="42a37-117">Bundling reduziert die Anzahl der serveranforderungen, die erforderlich sind, um eine Web-Ressource, z. B. eine Webseite rendern.</span><span class="sxs-lookup"><span data-stu-id="42a37-117">Bundling reduces the number of server requests which are necessary to render a web asset, such as a web page.</span></span> <span data-ttu-id="42a37-118">Sie können eine beliebige Anzahl von einzelnen Pakete speziell für CSS, JavaScript usw. erstellen. Weniger Dateien bedeutet weniger HTTP-Anforderungen über den Browser an den Server oder aus dem Dienst, der die Anwendung bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="42a37-118">You can create any number of individual bundles specifically for CSS, JavaScript, etc. Fewer files means fewer HTTP requests from the browser to the server or from the service providing your application.</span></span> <span data-ttu-id="42a37-119">Ergibt sich aus diesem verbessert die Leistung beim ersten Seite.</span><span class="sxs-lookup"><span data-stu-id="42a37-119">This results in improved first page load performance.</span></span>

### <a name="minification"></a><span data-ttu-id="42a37-120">Minimierung</span><span class="sxs-lookup"><span data-stu-id="42a37-120">Minification</span></span>

<span data-ttu-id="42a37-121">Minimierung entfernt überflüssige Zeichen aus Code ohne Funktionen zu ändern.</span><span class="sxs-lookup"><span data-stu-id="42a37-121">Minification removes unnecessary characters from code without altering functionality.</span></span> <span data-ttu-id="42a37-122">Das Ergebnis ist eine wesentliche Verringerung angeforderten Objekte (z. B. CSS, Bilder und JavaScript-Dateien).</span><span class="sxs-lookup"><span data-stu-id="42a37-122">The result is a significant size reduction in requested assets (such as CSS, images, and JavaScript files).</span></span> <span data-ttu-id="42a37-123">Allgemeine Nebeneffekte der Minimierung gehören Verkürzung Variablennamen an, um ein Zeichen und Kommentare und unnötige Leerzeichen entfernt.</span><span class="sxs-lookup"><span data-stu-id="42a37-123">Common side effects of minification include shortening variable names to one character and removing comments and unnecessary whitespace.</span></span>

<span data-ttu-id="42a37-124">Betrachten Sie die folgenden JavaScript-Funktion:</span><span class="sxs-lookup"><span data-stu-id="42a37-124">Consider the following JavaScript function:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

<span data-ttu-id="42a37-125">Minimierung reduziert die Funktion wie folgt:</span><span class="sxs-lookup"><span data-stu-id="42a37-125">Minification reduces the function to the following:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

<span data-ttu-id="42a37-126">Zusätzlich zu entfernen, Kommentare und Leerzeichen nicht erforderlich, wurden die folgenden Namen für Parameter und Variablen wie folgt umbenannt:</span><span class="sxs-lookup"><span data-stu-id="42a37-126">In addition to removing the comments and unnecessary whitespace, the following parameter and variable names were renamed as follows:</span></span>

<span data-ttu-id="42a37-127">Ursprünglich</span><span class="sxs-lookup"><span data-stu-id="42a37-127">Original</span></span> | <span data-ttu-id="42a37-128">Umbenannt</span><span class="sxs-lookup"><span data-stu-id="42a37-128">Renamed</span></span>
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a><span data-ttu-id="42a37-129">Auswirkungen der Bündelung und Minimierung</span><span class="sxs-lookup"><span data-stu-id="42a37-129">Impact of bundling and minification</span></span>

<span data-ttu-id="42a37-130">In der folgenden Tabelle sind die Unterschiede zwischen einzeln Bestand laden und Verwenden von Bündelung und Minimierung aufgeführt:</span><span class="sxs-lookup"><span data-stu-id="42a37-130">The following table outlines differences between individually loading assets and using bundling and minification:</span></span>

<span data-ttu-id="42a37-131">Aktion</span><span class="sxs-lookup"><span data-stu-id="42a37-131">Action</span></span> | <span data-ttu-id="42a37-132">Mit B/M</span><span class="sxs-lookup"><span data-stu-id="42a37-132">With B/M</span></span> | <span data-ttu-id="42a37-133">Ohne B/M</span><span class="sxs-lookup"><span data-stu-id="42a37-133">Without B/M</span></span> | <span data-ttu-id="42a37-134">Änderung</span><span class="sxs-lookup"><span data-stu-id="42a37-134">Change</span></span>
--- | :---: | :---: | :---:
<span data-ttu-id="42a37-135">Dateianforderungen</span><span class="sxs-lookup"><span data-stu-id="42a37-135">File Requests</span></span>  | <span data-ttu-id="42a37-136">7</span><span class="sxs-lookup"><span data-stu-id="42a37-136">7</span></span>   | <span data-ttu-id="42a37-137">18</span><span class="sxs-lookup"><span data-stu-id="42a37-137">18</span></span>     | <span data-ttu-id="42a37-138">157%</span><span class="sxs-lookup"><span data-stu-id="42a37-138">157%</span></span>
<span data-ttu-id="42a37-139">KB übertragen</span><span class="sxs-lookup"><span data-stu-id="42a37-139">KB Transferred</span></span> | <span data-ttu-id="42a37-140">156</span><span class="sxs-lookup"><span data-stu-id="42a37-140">156</span></span> | <span data-ttu-id="42a37-141">264.68</span><span class="sxs-lookup"><span data-stu-id="42a37-141">264.68</span></span> | <span data-ttu-id="42a37-142">70%</span><span class="sxs-lookup"><span data-stu-id="42a37-142">70%</span></span>
<span data-ttu-id="42a37-143">Laden Sie die Zeit (ms)</span><span class="sxs-lookup"><span data-stu-id="42a37-143">Load Time (ms)</span></span> | <span data-ttu-id="42a37-144">885</span><span class="sxs-lookup"><span data-stu-id="42a37-144">885</span></span> | <span data-ttu-id="42a37-145">2360</span><span class="sxs-lookup"><span data-stu-id="42a37-145">2360</span></span>   | <span data-ttu-id="42a37-146">167%</span><span class="sxs-lookup"><span data-stu-id="42a37-146">167%</span></span>

<span data-ttu-id="42a37-147">Browser sind ziemlich aufwändig im Hinblick auf HTTP-Anforderungsheader.</span><span class="sxs-lookup"><span data-stu-id="42a37-147">Browsers are fairly verbose with regard to HTTP request headers.</span></span> <span data-ttu-id="42a37-148">Die Gesamtanzahl der Bytes zu eine erhebliche Reduzierung von Metrik gesehen haben, wenn Bündelung gesendet.</span><span class="sxs-lookup"><span data-stu-id="42a37-148">The total bytes sent metric saw a significant reduction when bundling.</span></span> <span data-ttu-id="42a37-149">Die Ladezeit zeigt eine erhebliche Verbesserung, jedoch in diesem Beispiel wird lokal ausgeführt wurde.</span><span class="sxs-lookup"><span data-stu-id="42a37-149">The load time shows a significant improvement, however this example ran locally.</span></span> <span data-ttu-id="42a37-150">Deutlichere leistungsverbesserungen werden genutzt, wenn Bündelung und Minimierung mit Ressourcen verwenden, die über ein Netzwerk übertragen.</span><span class="sxs-lookup"><span data-stu-id="42a37-150">Greater performance gains are realized when using bundling and minification with assets transferred over a network.</span></span>

## <a name="choose-a-bundling-and-minification-strategy"></a><span data-ttu-id="42a37-151">Wählen Sie eine Strategie Bündelung und Minimierung</span><span class="sxs-lookup"><span data-stu-id="42a37-151">Choose a bundling and minification strategy</span></span>

<span data-ttu-id="42a37-152">Die Razor-Seiten und MVC-Projektvorlagen bieten eine Out-of-Box-Lösung für Bündelung und Minimierung bestehend aus einer JSON-Konfigurationsdatei an.</span><span class="sxs-lookup"><span data-stu-id="42a37-152">The MVC and Razor Pages project templates provide an out-of-the-box solution for bundling and minification consisting of a JSON configuration file.</span></span> <span data-ttu-id="42a37-153">Drittanbieter-tools, z. B. die [Gulp](xref:client-side/using-gulp) und [Grunt](xref:client-side/using-grunt) task Runner, Aufgaben mit etwas mehr Komplexität.</span><span class="sxs-lookup"><span data-stu-id="42a37-153">Third-party tools, such as the [Gulp](xref:client-side/using-gulp) and [Grunt](xref:client-side/using-grunt) task runners, accomplish the same tasks with a bit more complexity.</span></span> <span data-ttu-id="42a37-154">Ein Tool eines Drittanbieters ist ideal, wenn der Entwicklungsworkflow Verarbeitung hinter Bündelung und Minimierung erfordert&mdash;z. B. Linting und Image-Optimierung.</span><span class="sxs-lookup"><span data-stu-id="42a37-154">A third-party tool is a great fit when your development workflow requires processing beyond bundling and minification&mdash;such as linting and image optimization.</span></span> <span data-ttu-id="42a37-155">Verwenden Sie zur Entwurfszeit Bündelung und Minimierung, werden die verkleinerte Dateien vor der Bereitstellung der app erstellt.</span><span class="sxs-lookup"><span data-stu-id="42a37-155">By using design-time bundling and minification, the minified files are created prior to the app's deployment.</span></span> <span data-ttu-id="42a37-156">Bundling und Minimierung mit vor der Bereitstellung bietet den Vorteil des reduzierten Serverlast.</span><span class="sxs-lookup"><span data-stu-id="42a37-156">Bundling and minifying before deployment provides the advantage of reduced server load.</span></span> <span data-ttu-id="42a37-157">Allerdings es ist wichtig zu wissen, während der Entwurfszeit Bündelung und Minimierung Build Komplexität erhöht und kann nur für statische Dateien.</span><span class="sxs-lookup"><span data-stu-id="42a37-157">However, it's important to recognize that design-time bundling and minification increases build complexity and only works with static files.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="42a37-158">Konfigurieren von Bündelung und Minimierung</span><span class="sxs-lookup"><span data-stu-id="42a37-158">Configure bundling and minification</span></span>

<span data-ttu-id="42a37-159">Geben Sie die Razor-Seiten und MVC-Projektvorlagen einen *bundleconfig.json* Konfigurationsdatei an, die die Optionen für jedes Paket definiert.</span><span class="sxs-lookup"><span data-stu-id="42a37-159">The MVC and Razor Pages project templates provide a *bundleconfig.json* configuration file which defines the options for each bundle.</span></span> <span data-ttu-id="42a37-160">Standardmäßig wird eine Paket-Konfiguration für die benutzerdefinierte JavaScript definiert (*wwwroot/js/site.js*) und Stylesheet (*wwwroot/css/site.css*) Dateien:</span><span class="sxs-lookup"><span data-stu-id="42a37-160">By default, a single bundle configuration is defined for the custom JavaScript (*wwwroot/js/site.js*) and stylesheet (*wwwroot/css/site.css*) files:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

<span data-ttu-id="42a37-161">Bundle-Optionen:</span><span class="sxs-lookup"><span data-stu-id="42a37-161">Bundle options include:</span></span>

* <span data-ttu-id="42a37-162">`outputFileName`: Der Name der Ausgabe der Paketdatei.</span><span class="sxs-lookup"><span data-stu-id="42a37-162">`outputFileName`: The name of the bundle file to output.</span></span> <span data-ttu-id="42a37-163">Einen relativen Pfad darf das *bundleconfig.json* Datei.</span><span class="sxs-lookup"><span data-stu-id="42a37-163">Can contain a relative path from the *bundleconfig.json* file.</span></span> <span data-ttu-id="42a37-164">**Erforderlich**</span><span class="sxs-lookup"><span data-stu-id="42a37-164">**required**</span></span>
* <span data-ttu-id="42a37-165">`inputFiles`: Ein Array von Dateien bündeln.</span><span class="sxs-lookup"><span data-stu-id="42a37-165">`inputFiles`: An array of files to bundle together.</span></span> <span data-ttu-id="42a37-166">Hierbei handelt es sich um relative Pfade in der Konfigurationsdatei.</span><span class="sxs-lookup"><span data-stu-id="42a37-166">These are relative paths to the configuration file.</span></span> <span data-ttu-id="42a37-167">**optionale**, * führt ein leerer Wert eine leere Ausgabedatei.</span><span class="sxs-lookup"><span data-stu-id="42a37-167">**optional**, *an empty value results in an empty output file.</span></span> <span data-ttu-id="42a37-168">[Globmodus](http://www.tldp.org/LDP/abs/html/globbingref.html) Muster werden unterstützt.</span><span class="sxs-lookup"><span data-stu-id="42a37-168">[globbing](http://www.tldp.org/LDP/abs/html/globbingref.html) patterns are supported.</span></span>
* <span data-ttu-id="42a37-169">`minify`: Die Minimierung-Optionen für die Ausgabe geben.</span><span class="sxs-lookup"><span data-stu-id="42a37-169">`minify`: The minification options for the output type.</span></span> <span data-ttu-id="42a37-170">**optionale**, *Standard:`minify: { enabled: true }`*</span><span class="sxs-lookup"><span data-stu-id="42a37-170">**optional**, *default - `minify: { enabled: true }`*</span></span>
  * <span data-ttu-id="42a37-171">Konfigurationsoptionen sind pro Datei Ausgabetyp verfügbar.</span><span class="sxs-lookup"><span data-stu-id="42a37-171">Configuration options are available per output file type.</span></span>
    * [<span data-ttu-id="42a37-172">CSS-Minifier</span><span class="sxs-lookup"><span data-stu-id="42a37-172">CSS Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [<span data-ttu-id="42a37-173">JavaScript-Minifier</span><span class="sxs-lookup"><span data-stu-id="42a37-173">JavaScript Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [<span data-ttu-id="42a37-174">HTML-Minifier</span><span class="sxs-lookup"><span data-stu-id="42a37-174">HTML Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki)
* <span data-ttu-id="42a37-175">`includeInProject`: Flag gibt an, ob die generierte Dateien zur Projektdatei hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="42a37-175">`includeInProject`: Flag indicating whether to add generated files to project file.</span></span> <span data-ttu-id="42a37-176">**optionale**, *Standard - "false"*</span><span class="sxs-lookup"><span data-stu-id="42a37-176">**optional**, *default - false*</span></span>
* <span data-ttu-id="42a37-177">`sourceMap`: Flag gibt an, ob eine quellzuordnung für die gebündelte Datei zu generieren.</span><span class="sxs-lookup"><span data-stu-id="42a37-177">`sourceMap`: Flag indicating whether to generate a source map for the bundled file.</span></span> <span data-ttu-id="42a37-178">**optionale**, *Standard - "false"*</span><span class="sxs-lookup"><span data-stu-id="42a37-178">**optional**, *default - false*</span></span>
* <span data-ttu-id="42a37-179">`sourceMapRootPath`: Der Pfad des Anwendungsstamms für das Speichern der generierten Quelldatei für die Zuordnung.</span><span class="sxs-lookup"><span data-stu-id="42a37-179">`sourceMapRootPath`: The root path for storing the generated source map file.</span></span>

## <a name="build-time-execution-of-bundling-and-minification"></a><span data-ttu-id="42a37-180">Zur Buildzeit Ausführung Bündelung und Minimierung</span><span class="sxs-lookup"><span data-stu-id="42a37-180">Build-time execution of bundling and minification</span></span>

<span data-ttu-id="42a37-181">Die [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) NuGet-Paket ermöglicht die Ausführung von Bündelung und Minimierung zur Buildzeit.</span><span class="sxs-lookup"><span data-stu-id="42a37-181">The [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) NuGet package enables the execution of bundling and minification at build time.</span></span> <span data-ttu-id="42a37-182">Das Paket fügt [MSBuild-Ziele](/visualstudio/msbuild/msbuild-targets) dem zu erstellen und bereinigen Uhrzeit ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="42a37-182">The package injects [MSBuild Targets](/visualstudio/msbuild/msbuild-targets) which run at build and clean time.</span></span> <span data-ttu-id="42a37-183">Die *bundleconfig.json* Datei durch den Buildprozess die Ausgabedateien auf Basis der Konfiguration definierten erzeugen analysiert.</span><span class="sxs-lookup"><span data-stu-id="42a37-183">The *bundleconfig.json* file is analyzed by the build process to produce the output files based on the defined configuration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="42a37-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="42a37-184">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="42a37-185">Hinzufügen der *BuildBundlerMinifier* Paket Ihrem Projekt.</span><span class="sxs-lookup"><span data-stu-id="42a37-185">Add the *BuildBundlerMinifier* package to your project.</span></span>

<span data-ttu-id="42a37-186">Erstellen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="42a37-186">Build the project.</span></span> <span data-ttu-id="42a37-187">Im folgenden wird im Ausgabefenster angezeigt:</span><span class="sxs-lookup"><span data-stu-id="42a37-187">The following appears in the Output window:</span></span>

```console
1>------ Build started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Begin processing bundleconfig.json
1>  Minified wwwroot/css/site.min.css
1>  Minified wwwroot/js/site.min.js
1>Bundler: Done processing bundleconfig.json
1>BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
```

<span data-ttu-id="42a37-188">Bereinigen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="42a37-188">Clean the project.</span></span> <span data-ttu-id="42a37-189">Im folgenden wird im Ausgabefenster angezeigt:</span><span class="sxs-lookup"><span data-stu-id="42a37-189">The following appears in the Output window:</span></span>

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="42a37-190">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="42a37-190">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="42a37-191">Hinzufügen der *BuildBundlerMinifier* Paket Ihrem Projekt:</span><span class="sxs-lookup"><span data-stu-id="42a37-191">Add the *BuildBundlerMinifier* package to your project:</span></span>

```console
dotnet add package BuildBundlerMinifier
```

<span data-ttu-id="42a37-192">Bei Verwendung von ASP.NET Core 1.x, stellen Sie das Paket neu hinzugefügte wieder her:</span><span class="sxs-lookup"><span data-stu-id="42a37-192">If using ASP.NET Core 1.x, restore the newly added package:</span></span>

```console
dotnet restore
```

<span data-ttu-id="42a37-193">Erstellen Sie das Projekt:</span><span class="sxs-lookup"><span data-stu-id="42a37-193">Build the project:</span></span>

```console
dotnet build
```

<span data-ttu-id="42a37-194">Im folgenden wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="42a37-194">The following appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

<span data-ttu-id="42a37-195">Bereinigen Sie das Projekt:</span><span class="sxs-lookup"><span data-stu-id="42a37-195">Clean the project:</span></span>

```console
dotnet clean
```

<span data-ttu-id="42a37-196">Die folgende Ausgabe wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="42a37-196">The following output appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a><span data-ttu-id="42a37-197">Ad-hoc-Ausführung von Bündelung und Minimierung</span><span class="sxs-lookup"><span data-stu-id="42a37-197">Ad hoc execution of bundling and minification</span></span>

<span data-ttu-id="42a37-198">Es ist möglich, die auf einer ad-hoc-Basis der Bündelung und Minimierung Aufgaben ausgeführt werden, ohne Erstellen des Projekts.</span><span class="sxs-lookup"><span data-stu-id="42a37-198">It's possible to run the bundling and minification tasks on an ad hoc basis, without building the project.</span></span> <span data-ttu-id="42a37-199">Hinzufügen der [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet-Paket Ihrem Projekt:</span><span class="sxs-lookup"><span data-stu-id="42a37-199">Add the [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet package to your project:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

<span data-ttu-id="42a37-200">Dieses Paket erweitert, die .NET Core CLI enthalten die *Dotnet-Bundle* Tool.</span><span class="sxs-lookup"><span data-stu-id="42a37-200">This package extends the .NET Core CLI to include the *dotnet-bundle* tool.</span></span> <span data-ttu-id="42a37-201">Der folgende Befehl kann im Paket-Manager-Konsole (PMC)-Fenster oder in eine Befehlsshell ausgeführt werden:</span><span class="sxs-lookup"><span data-stu-id="42a37-201">The following command can be executed in the Package Manager Console (PMC) window or in a command shell:</span></span>

```console
dotnet bundle
```

> [!IMPORTANT]
> <span data-ttu-id="42a37-202">NuGet-Paket-Manager die Datei *.csproj als Abhängigkeiten hinzugefügt `<PackageReference />` Knoten.</span><span class="sxs-lookup"><span data-stu-id="42a37-202">NuGet Package Manager adds dependencies to the *.csproj file as `<PackageReference />` nodes.</span></span> <span data-ttu-id="42a37-203">Die `dotnet bundle` Befehl wird mit der .NET Core-CLI registriert nur, wenn ein `<DotNetCliToolReference />` Knoten verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="42a37-203">The `dotnet bundle` command is registered with the .NET Core CLI only when a `<DotNetCliToolReference />` node is used.</span></span> <span data-ttu-id="42a37-204">Ändern Sie die Datei *.csproj entsprechend an.</span><span class="sxs-lookup"><span data-stu-id="42a37-204">Modify the *.csproj file accordingly.</span></span>

## <a name="add-files-to-workflow"></a><span data-ttu-id="42a37-205">Hinzufügen von Dateien an den workflow</span><span class="sxs-lookup"><span data-stu-id="42a37-205">Add files to workflow</span></span>

<span data-ttu-id="42a37-206">Betrachten Sie ein Beispiel, in dem ein zusätzlicher *custom.css* Datei hinzugefügt wird, wie die folgenden:</span><span class="sxs-lookup"><span data-stu-id="42a37-206">Consider an example in which an additional *custom.css* file is added resembling the following:</span></span>

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

<span data-ttu-id="42a37-207">Zu verkleinernde *custom.css* und bündeln mit *"Site.CSS" ändern* in einem *site.min.css* Datei, fügen Sie den relativen Pfad zum *bundleconfig.json*:</span><span class="sxs-lookup"><span data-stu-id="42a37-207">To minify *custom.css* and bundle it with *site.css* into a *site.min.css* file, add the relative path to *bundleconfig.json*:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> <span data-ttu-id="42a37-208">Alternativ könnte die folgenden Globmodus-Muster verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="42a37-208">Alternatively, the following globbing pattern could be used:</span></span>
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]
> ```
>
> <span data-ttu-id="42a37-209">Dieses Muster Globmodus entspricht allen CSS-Dateien und schließt die verkleinerte Dateimuster.</span><span class="sxs-lookup"><span data-stu-id="42a37-209">This globbing pattern matches all CSS files and excludes the minified file pattern.</span></span>

<span data-ttu-id="42a37-210">Erstellen Sie die Anwendung.</span><span class="sxs-lookup"><span data-stu-id="42a37-210">Build the application.</span></span> <span data-ttu-id="42a37-211">Open *site.min.css* , und beachten Sie den Inhalt des *custom.css* am Ende der Datei angefügt wird.</span><span class="sxs-lookup"><span data-stu-id="42a37-211">Open *site.min.css* and notice the content of *custom.css* is appended to the end of the file.</span></span>

## <a name="environment-based-bundling-and-minification"></a><span data-ttu-id="42a37-212">Umgebung basierende Bündelung und Minimierung</span><span class="sxs-lookup"><span data-stu-id="42a37-212">Environment-based bundling and minification</span></span>

<span data-ttu-id="42a37-213">Als bewährte Methode sollte die gebündelten und verkleinerte Dateien Ihrer App in einer produktiven Umgebung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="42a37-213">As a best practice, the bundled and minified files of your app should be used in a production environment.</span></span> <span data-ttu-id="42a37-214">Stellen die ursprünglichen Dateien zum einfacheren Debuggen der app, während der Entwicklung.</span><span class="sxs-lookup"><span data-stu-id="42a37-214">During development, the original files make for easier debugging of the app.</span></span>

<span data-ttu-id="42a37-215">Geben Sie die einzuschließenden Dateien aus, auf den Seiten mit den [Umgebung Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) in Ihren Ansichten.</span><span class="sxs-lookup"><span data-stu-id="42a37-215">Specify which files to include in your pages by using the [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) in your views.</span></span> <span data-ttu-id="42a37-216">Die Umgebung Tags Hilfsprogramm seinen Inhalt nur gerendert wird, bei der Ausführung in bestimmten [Umgebungen](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="42a37-216">The Environment Tag Helper only renders its contents when running in specific [environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="42a37-217">Die folgenden `environment` Tag rendert die unverarbeiteten CSS-Dateien, bei der Ausführung im die `Development` Umgebung:</span><span class="sxs-lookup"><span data-stu-id="42a37-217">The following `environment` tag renders the unprocessed CSS files when running in the `Development` environment:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="42a37-218">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="42a37-218">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="42a37-219">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="42a37-219">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

---

<span data-ttu-id="42a37-220">Die folgenden `environment` Tag rendert die gebündelten und verkleinerte CSS-Dateien aus, wenn in einer Umgebung ausgeführt, außer `Development`.</span><span class="sxs-lookup"><span data-stu-id="42a37-220">The following `environment` tag renders the bundled and minified CSS files when running in an environment other than `Development`.</span></span> <span data-ttu-id="42a37-221">Z. B. Ausführung `Production` oder `Staging` löst diese Stylesheets gerendert:</span><span class="sxs-lookup"><span data-stu-id="42a37-221">For example, running in `Production` or `Staging` triggers the rendering of these stylesheets:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="42a37-222">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="42a37-222">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="42a37-223">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="42a37-223">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

---

## <a name="consume-bundleconfigjson-from-gulp"></a><span data-ttu-id="42a37-224">Verwenden von Gulp bundleconfig.json</span><span class="sxs-lookup"><span data-stu-id="42a37-224">Consume bundleconfig.json from Gulp</span></span>

<span data-ttu-id="42a37-225">Es gibt Fälle, in denen eine app Bündelung und Minimierung Workflow zusätzliche Verarbeitung erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="42a37-225">There are cases in which an app's bundling and minification workflow requires additional processing.</span></span> <span data-ttu-id="42a37-226">Beispiele sind Image Optimierung, Cache Abwehrprogramm und CDN Asset-Verarbeitung.</span><span class="sxs-lookup"><span data-stu-id="42a37-226">Examples include image optimization, cache busting, and CDN asset processing.</span></span> <span data-ttu-id="42a37-227">Um diese Anforderungen zu erfüllen, können Sie den Workflow Bündelung und Minimierung zum Verwenden von Gulp konvertieren.</span><span class="sxs-lookup"><span data-stu-id="42a37-227">To satisfy these requirements, you can convert the bundling and minification workflow to use Gulp.</span></span>

### <a name="use-the-bundler--minifier-extension"></a><span data-ttu-id="42a37-228">Verwenden Sie die Erweiterung Bundle & Minifier</span><span class="sxs-lookup"><span data-stu-id="42a37-228">Use the Bundler & Minifier extension</span></span>

<span data-ttu-id="42a37-229">Visual Studio [Bundle & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) Erweiterung übernimmt die Konvertierung in Gulp.</span><span class="sxs-lookup"><span data-stu-id="42a37-229">The Visual Studio [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) extension handles the conversion to Gulp.</span></span>

<span data-ttu-id="42a37-230">Mit der rechten Maustaste die *bundleconfig.json* im Projektmappen-Explorer die Datei, und wählen Sie **Bundle & Minifier** > **konvertieren, Gulp...** :</span><span class="sxs-lookup"><span data-stu-id="42a37-230">Right-click the *bundleconfig.json* file in Solution Explorer and select **Bundler & Minifier** > **Convert To Gulp...**:</span></span>

![Konvertieren zu Gulp Kontextmenüelement](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

<span data-ttu-id="42a37-232">Die *gulpfile.js* und *"Package.JSON"* Dateien werden dem Projekt hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="42a37-232">The *gulpfile.js* and *package.json* files are added to the project.</span></span> <span data-ttu-id="42a37-233">Die zur Unterstützung [Npm](https://www.npmjs.com/) Pakete aufgelistet, die der *"Package.JSON"* Datei `devDependencies` Abschnitt installiert sind.</span><span class="sxs-lookup"><span data-stu-id="42a37-233">The supporting [npm](https://www.npmjs.com/) packages listed in the *package.json* file's `devDependencies` section are installed.</span></span>

<span data-ttu-id="42a37-234">Führen Sie den folgenden Befehl in einem PMC-Fenster auf die CLI Gulp als globale Abhängigkeit installieren:</span><span class="sxs-lookup"><span data-stu-id="42a37-234">Run the following command in the PMC window to install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="42a37-235">Die *gulpfile.js* Datei liest die *bundleconfig.json* -Datei für die Eingaben, Ausgaben und Einstellungen.</span><span class="sxs-lookup"><span data-stu-id="42a37-235">The *gulpfile.js* file reads the *bundleconfig.json* file for the inputs, outputs, and settings.</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a><span data-ttu-id="42a37-236">Konvertieren Sie manuell</span><span class="sxs-lookup"><span data-stu-id="42a37-236">Convert manually</span></span>

<span data-ttu-id="42a37-237">Wenn Visual Studio und/oder die Bundle & Minifier-Erweiterung nicht verfügbar sind, konvertieren Sie manuell ein.</span><span class="sxs-lookup"><span data-stu-id="42a37-237">If Visual Studio and/or the Bundler & Minifier extension aren't available, convert manually.</span></span>

<span data-ttu-id="42a37-238">Hinzufügen einer *"Package.JSON"* -Datei mit den folgenden `devDependencies`, in das Projektstammverzeichnis:</span><span class="sxs-lookup"><span data-stu-id="42a37-238">Add a *package.json* file, with the following `devDependencies`, to the project root:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

<span data-ttu-id="42a37-239">Installieren von Abhängigkeiten von den folgenden Befehl ausführen, auf der gleichen Ebene wie *"Package.JSON"*:</span><span class="sxs-lookup"><span data-stu-id="42a37-239">Install the dependencies by running the following command at the same level as *package.json*:</span></span>

```console
npm i
```

<span data-ttu-id="42a37-240">Installieren Sie die CLI Gulp als globale Abhängigkeit:</span><span class="sxs-lookup"><span data-stu-id="42a37-240">Install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="42a37-241">Kopieren der *gulpfile.js* Datei unter dem Projektstamm:</span><span class="sxs-lookup"><span data-stu-id="42a37-241">Copy the *gulpfile.js* file below to the project root:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a><span data-ttu-id="42a37-242">Gulp Tasks ausführen</span><span class="sxs-lookup"><span data-stu-id="42a37-242">Run Gulp tasks</span></span>

<span data-ttu-id="42a37-243">Fügen Sie zum Auslösen der Gulp Minimierung-Aufgabe bevor Sie das Projekt in Visual Studio erstellt die folgenden [MSBuild-Ziel](/visualstudio/msbuild/msbuild-targets) in die Datei *.csproj:</span><span class="sxs-lookup"><span data-stu-id="42a37-243">To trigger the Gulp minification task before the project builds in Visual Studio, add the following [MSBuild Target](/visualstudio/msbuild/msbuild-targets) to the *.csproj file:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

<span data-ttu-id="42a37-244">In diesem Beispiel alle Aufgaben definiert, in der `MyPreCompileTarget` Ziel ausführen, bevor die vordefinierten `Build` Ziel.</span><span class="sxs-lookup"><span data-stu-id="42a37-244">In this example, any tasks defined within the `MyPreCompileTarget` target run before the predefined `Build` target.</span></span> <span data-ttu-id="42a37-245">Ähnlich der folgenden Ausgabe wird in Visual Studio-Fenster "Ausgabe" angezeigt:</span><span class="sxs-lookup"><span data-stu-id="42a37-245">Output similar to the following appears in Visual Studio's Output window:</span></span>

```console
1>------ Build started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
1>[14:17:49] Using gulpfile C:\BuildBundlerMinifierApp\gulpfile.js
1>[14:17:49] Starting 'min:js'...
1>[14:17:49] Starting 'min:css'...
1>[14:17:49] Starting 'min:html'...
1>[14:17:49] Finished 'min:js' after 83 ms
1>[14:17:49] Finished 'min:css' after 88 ms
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
```

<span data-ttu-id="42a37-246">Alternativ kann der Task Runner-Explorer von Visual Studio verwendet werden, Gulp Aufgaben an bestimmten Visual Studio-Ereignisse binden.</span><span class="sxs-lookup"><span data-stu-id="42a37-246">Alternatively, Visual Studio's Task Runner Explorer may be used to bind Gulp tasks to specific Visual Studio events.</span></span> <span data-ttu-id="42a37-247">Finden Sie unter [Ausführen von Standardaufgaben](xref:client-side/using-gulp#running-default-tasks) Anweisungen auf diese Weise.</span><span class="sxs-lookup"><span data-stu-id="42a37-247">See [Running default tasks](xref:client-side/using-gulp#running-default-tasks) for instructions on doing that.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="42a37-248">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="42a37-248">Additional resources</span></span>

* [<span data-ttu-id="42a37-249">Verwenden von Gulp</span><span class="sxs-lookup"><span data-stu-id="42a37-249">Using Gulp</span></span>](xref:client-side/using-gulp)
* [<span data-ttu-id="42a37-250">Verwenden von Grunt</span><span class="sxs-lookup"><span data-stu-id="42a37-250">Using Grunt</span></span>](xref:client-side/using-grunt)
* [<span data-ttu-id="42a37-251">Arbeiten mit mehreren Umgebungen</span><span class="sxs-lookup"><span data-stu-id="42a37-251">Working with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="42a37-252">Taghilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="42a37-252">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
