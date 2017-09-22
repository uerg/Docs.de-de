---
title: Bundling und Minimierung in ASP.NET Core
author: spboyer
description: 
keywords: "ASP.NET Core Bündelung und Minimierung, CSS, JavaScript, verkleinernde BuildBundlerMinifier"
ms.author: riande
manager: wpickett
ms.date: 02/28/2017
ms.topic: article
ms.assetid: d54230f9-8e5f-4861-a29c-1d3a14e0b0d9
ms.technology: aspnet
ms.prod: aspnet-core
uid: client-side/bundling-and-minification
ms.openlocfilehash: 11528cb2067ced79a09845f9ff78d897da033438
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2017
---
# <a name="bundling-and-minification-in-aspnet-core"></a><span data-ttu-id="54460-103">Bundling und Minimierung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="54460-103">Bundling and minification in ASP.NET Core</span></span>

<span data-ttu-id="54460-104">Bundling und Minimierung sind zwei Techniken können Sie in ASP.NET zur Verbesserung der Leistung beim Laden von Seite für Ihre Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="54460-104">Bundling and minification are two techniques you can use in ASP.NET to improve page load performance for your web application.</span></span> <span data-ttu-id="54460-105">Bundling kombiniert mehrere Dateien in einer einzelnen Datei.</span><span class="sxs-lookup"><span data-stu-id="54460-105">Bundling combines multiple files into a single file.</span></span> <span data-ttu-id="54460-106">Minimierung führt eine Vielzahl von anderen Code, der Optimierungen mit Skripts und CSS, was in kleinere Nutzlasten führt.</span><span class="sxs-lookup"><span data-stu-id="54460-106">Minification performs a variety of different code optimizations to scripts and CSS, which results in smaller payloads.</span></span> <span data-ttu-id="54460-107">Zusammen verwendet werden, verbessert Bündelung und Minimierung die Leistung der Load-Zeit durch die Anzahl der Anforderungen an den Server zu verringern und Verringern der Größe der angeforderten Objekte (z. B. CSS- und JavaScript-Dateien).</span><span class="sxs-lookup"><span data-stu-id="54460-107">Used together, bundling and minification improves load time performance by reducing the number of requests to the server and reducing the size of the requested assets (such as CSS and JavaScript files).</span></span>

<span data-ttu-id="54460-108">Dieser Artikel beschreibt die Vorteile der Verwendung von Bündelung und Minimierung, z. B., wie diese Funktionen mit ASP.NET Core-Anwendungen verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="54460-108">This article explains the benefits of using bundling and minification, including how these features can be used with ASP.NET Core applications.</span></span>

## <a name="overview"></a><span data-ttu-id="54460-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="54460-109">Overview</span></span>

<span data-ttu-id="54460-110">In ASP.NET Core-apps stehen Ihnen mehrere Optionen zum bundling und Minimierung mit clientseitigen Ressourcen.</span><span class="sxs-lookup"><span data-stu-id="54460-110">In ASP.NET Core apps, there are multiple options for bundling and minifying client-side resources.</span></span> <span data-ttu-id="54460-111">Die Standardvorlagen für MVC bieten eine Out-of-Box-Lösung, die mit einer Konfigurationsdatei und BuildBundlerMinifier NuGet-Paket.</span><span class="sxs-lookup"><span data-stu-id="54460-111">The core templates for MVC provide an out-of-the-box solution using a configuration file and BuildBundlerMinifier NuGet package.</span></span> <span data-ttu-id="54460-112">Drittanbieter-tools, z. B. [Gulp](using-gulp.md) und [Grunt](using-grunt.md) sind ebenfalls verfügbar zur Ausführen derselben Aufgaben sollten den Prozessen benötigt werden, zusätzliche Workflow oder Komplexität.</span><span class="sxs-lookup"><span data-stu-id="54460-112">Third party tools, such as [Gulp](using-gulp.md) and [Grunt](using-grunt.md) are also available to accomplish the same tasks should your processes require additional workflow or complexities.</span></span> <span data-ttu-id="54460-113">Verwenden Sie zur Entwurfszeit Bündelung und Minimierung, werden die verkleinerte Dateien vor der Bereitstellung der Anwendung erstellt.</span><span class="sxs-lookup"><span data-stu-id="54460-113">By using design-time bundling and minification, the minified files are created prior to the application’s deployment.</span></span> <span data-ttu-id="54460-114">Bundling und Minimierung mit vor der Bereitstellung bietet den Vorteil des reduzierten Serverlast.</span><span class="sxs-lookup"><span data-stu-id="54460-114">Bundling and minifying before deployment provides the advantage of reduced server load.</span></span> <span data-ttu-id="54460-115">Allerdings es ist wichtig zu wissen, während der Entwurfszeit Bündelung und Minimierung Build Komplexität erhöht und kann nur für statische Dateien.</span><span class="sxs-lookup"><span data-stu-id="54460-115">However, it’s important to recognize that design-time bundling and minification increases build complexity and only works with static files.</span></span>

<span data-ttu-id="54460-116">Bundling und Minimierung wird in erster Linie die erste Anforderung Seitenladezeit verbessern.</span><span class="sxs-lookup"><span data-stu-id="54460-116">Bundling and minification primarily improve the first page request load time.</span></span> <span data-ttu-id="54460-117">Sobald eine Webseite angefordert wurde, speichert der Browser Objekte (JavaScript, CSS und Bilder), um Bündelung und Minimierung wird kein Leistungssteigerung beim Anfordern von derselben Seite bieten oder Seiten auf dem gleichen Standort die gleichen Ressourcen anfordern.</span><span class="sxs-lookup"><span data-stu-id="54460-117">Once a web page has been requested, the browser caches the assets (JavaScript, CSS and images) so bundling and minification won’t provide any performance boost when requesting the same page, or pages on the same site requesting the same assets.</span></span> <span data-ttu-id="54460-118">Wenn Sie nicht Festlegen der Header ordnungsgemäß auf Ihre Medienobjekte abläuft und Bündelung und Minimierung verwenden Sie nicht, Heuristik beim Ermitteln des Browsers der Aktualität kennzeichnet die Medienobjekte veraltete nach ein paar Tagen und der Browser wird für jedes Medienobjekt eine validierungsanforderung erfordern.</span><span class="sxs-lookup"><span data-stu-id="54460-118">If you don’t set the expires header correctly on your assets, and you don’t use bundling and minification, the browser's freshness heuristics will mark the assets stale after a few days and the browser will require a validation request for each asset.</span></span> <span data-ttu-id="54460-119">In diesem Fall geben Bündelung und Minimierung eine Leistungssteigerung auch nach der ersten Seitenanforderung.</span><span class="sxs-lookup"><span data-stu-id="54460-119">In this case, bundling and minification provide a performance increase even after the first page request.</span></span>

### <a name="bundling"></a><span data-ttu-id="54460-120">Bundling</span><span class="sxs-lookup"><span data-stu-id="54460-120">Bundling</span></span>

<span data-ttu-id="54460-121">Bundling ist ein Feature, das ganz einfach kombinieren oder mehrere Dateien in einer einzelnen Datei bündeln.</span><span class="sxs-lookup"><span data-stu-id="54460-121">Bundling is a feature that makes it easy to combine or bundle multiple files into a single file.</span></span> <span data-ttu-id="54460-122">Darin, dass mehrere Dateien in einer einzelnen Datei bundling kombiniert werden, wird die Anzahl der Anforderungen an den Server, die erforderlich sind, abgerufen und eine Web-Ressource, z. B. eine Webseite angezeigt.</span><span class="sxs-lookup"><span data-stu-id="54460-122">Because bundling combines multiple files into a single file, it reduces the number of requests to the server that are required to retrieve and display a web asset, such as a web page.</span></span> <span data-ttu-id="54460-123">Sie können CSS, JavaScript und andere Pakete erstellen.</span><span class="sxs-lookup"><span data-stu-id="54460-123">You can create CSS, JavaScript and other bundles.</span></span> <span data-ttu-id="54460-124">Weniger Dateien bedeutet weniger HTTP-Anforderungen in Ihrem Browser auf dem Server oder aus dem Dienst, der die Anwendung bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="54460-124">Fewer files means fewer HTTP requests from your browser to the server or from the service providing your application.</span></span> <span data-ttu-id="54460-125">Ergibt sich aus diesem verbessert die Leistung beim ersten Seite.</span><span class="sxs-lookup"><span data-stu-id="54460-125">This results in improved first page load performance.</span></span>

### <a name="minification"></a><span data-ttu-id="54460-126">Minimierung</span><span class="sxs-lookup"><span data-stu-id="54460-126">Minification</span></span>

<span data-ttu-id="54460-127">Minimierung führt eine Vielzahl von verschiedenen codeoptimierungen, die zum Verringern der Größe der angeforderten Objekte (z. B. CSS, Bilder, JavaScript-Dateien).</span><span class="sxs-lookup"><span data-stu-id="54460-127">Minification performs a variety of different code optimizations to reduce the size of requested assets (such as CSS, images, JavaScript files).</span></span> <span data-ttu-id="54460-128">Allgemeine Ergebnisse von Minimierung gehören unnötige Leerzeichen und Kommentare entfernen und Verkürzung Variablennamen in einem Zeichen.</span><span class="sxs-lookup"><span data-stu-id="54460-128">Common results of minification include removing unnecessary white space and comments, and shortening variable names to one character.</span></span>

<span data-ttu-id="54460-129">Betrachten Sie die folgenden JavaScript-Funktion:</span><span class="sxs-lookup"><span data-stu-id="54460-129">Consider the following JavaScript function:</span></span>

```javascript
AddAltToImg = function (imageTagAndImageID, imageContext) {
  ///<signature>
  ///<summary> Adds an alt tab to the image
  // </summary>
  //<param name="imgElement" type="String">The image selector.</param>
  //<param name="ContextForImage" type="String">The image context.</param>
  ///</signature>
  var imageElement = $(imageTagAndImageID, imageContext);
  imageElement.attr('alt', imageElement.attr('id').replace(/ID/, ''));
}
```

<span data-ttu-id="54460-130">Nach der Minimierung wird die Funktion auf die folgenden reduziert:</span><span class="sxs-lookup"><span data-stu-id="54460-130">After minification, the function is reduced to the following:</span></span>

```javascript
AddAltToImg=function(t,a){var r=$(t,a);r.attr("alt",r.attr("id").replace(/ID/,""))};
```

<span data-ttu-id="54460-131">Zusätzlich zu entfernen, Kommentare und Leerzeichen nicht erforderlich, wurden die folgenden Parameter und Variablen umbenannt (gekürzt) wie folgt:</span><span class="sxs-lookup"><span data-stu-id="54460-131">In addition to removing the comments and unnecessary whitespace, the following parameters and variable names were renamed (shortened) as follows:</span></span>

<span data-ttu-id="54460-132">Ursprünglich</span><span class="sxs-lookup"><span data-stu-id="54460-132">Original</span></span> | <span data-ttu-id="54460-133">Umbenannt</span><span class="sxs-lookup"><span data-stu-id="54460-133">Renamed</span></span>
--- | :---:
<span data-ttu-id="54460-134">imageTagAndImageID</span><span class="sxs-lookup"><span data-stu-id="54460-134">imageTagAndImageID</span></span> | <span data-ttu-id="54460-135">t</span><span class="sxs-lookup"><span data-stu-id="54460-135">t</span></span>
<span data-ttu-id="54460-136">imageContext</span><span class="sxs-lookup"><span data-stu-id="54460-136">imageContext</span></span> | <span data-ttu-id="54460-137">eine</span><span class="sxs-lookup"><span data-stu-id="54460-137">a</span></span>
<span data-ttu-id="54460-138">imageElement</span><span class="sxs-lookup"><span data-stu-id="54460-138">imageElement</span></span> | <span data-ttu-id="54460-139">b</span><span class="sxs-lookup"><span data-stu-id="54460-139">r</span></span>

## <a name="impact-of-bundling-and-minification"></a><span data-ttu-id="54460-140">Auswirkungen der Bündelung und Minimierung</span><span class="sxs-lookup"><span data-stu-id="54460-140">Impact of bundling and minification</span></span>

<span data-ttu-id="54460-141">Die folgende Tabelle zeigt einige wichtige Unterschiede zwischen alle Objekte einzeln auflisten und Verwenden von Bündelung und Minimierung auf eine einfache Webseite:</span><span class="sxs-lookup"><span data-stu-id="54460-141">The following table shows several important differences between listing all the assets individually and using bundling and minification on a simple web page:</span></span>

<span data-ttu-id="54460-142">Aktion</span><span class="sxs-lookup"><span data-stu-id="54460-142">Action</span></span> | <span data-ttu-id="54460-143">Mit B/M</span><span class="sxs-lookup"><span data-stu-id="54460-143">With B/M</span></span> | <span data-ttu-id="54460-144">Ohne B/M</span><span class="sxs-lookup"><span data-stu-id="54460-144">Without B/M</span></span> | <span data-ttu-id="54460-145">Änderung</span><span class="sxs-lookup"><span data-stu-id="54460-145">Change</span></span>
--- | :---: | :---: | :---:
<span data-ttu-id="54460-146">Dateianforderungen</span><span class="sxs-lookup"><span data-stu-id="54460-146">File Requests</span></span> |<span data-ttu-id="54460-147">7</span><span class="sxs-lookup"><span data-stu-id="54460-147">7</span></span> | <span data-ttu-id="54460-148">18</span><span class="sxs-lookup"><span data-stu-id="54460-148">18</span></span> | <span data-ttu-id="54460-149">157%</span><span class="sxs-lookup"><span data-stu-id="54460-149">157%</span></span>
<span data-ttu-id="54460-150">KB übertragen</span><span class="sxs-lookup"><span data-stu-id="54460-150">KB Transferred</span></span> | <span data-ttu-id="54460-151">156</span><span class="sxs-lookup"><span data-stu-id="54460-151">156</span></span> | <span data-ttu-id="54460-152">264.68</span><span class="sxs-lookup"><span data-stu-id="54460-152">264.68</span></span> | <span data-ttu-id="54460-153">70%</span><span class="sxs-lookup"><span data-stu-id="54460-153">70%</span></span>
<span data-ttu-id="54460-154">Laden Sie die Zeit (MS)</span><span class="sxs-lookup"><span data-stu-id="54460-154">Load Time (MS)</span></span> | <span data-ttu-id="54460-155">885</span><span class="sxs-lookup"><span data-stu-id="54460-155">885</span></span> | <span data-ttu-id="54460-156">2360</span><span class="sxs-lookup"><span data-stu-id="54460-156">2360</span></span> | <span data-ttu-id="54460-157">167%</span><span class="sxs-lookup"><span data-stu-id="54460-157">167%</span></span>

<span data-ttu-id="54460-158">Der gesendeten Bytes hat zu eine erhebliche Reduzierung mit das Bündelung ebenso wie Browsern Recht ausführlich mit den HTTP-Headern, die sie für Anforderungen gelten.</span><span class="sxs-lookup"><span data-stu-id="54460-158">The bytes sent had a significant reduction with bundling as browsers are fairly verbose with the HTTP headers that they apply on requests.</span></span> <span data-ttu-id="54460-159">Die Ladezeit zeigt eine große Verbesserung, jedoch in diesem Beispiel wird lokal ausgeführt wurde.</span><span class="sxs-lookup"><span data-stu-id="54460-159">The load time shows a big improvement, however this example was run locally.</span></span> <span data-ttu-id="54460-160">Wenn Bündelung und Minimierung mit Ressourcen verwenden, die über ein Netzwerk übertragen, erhalten eine größere erzielen bei der Leistung.</span><span class="sxs-lookup"><span data-stu-id="54460-160">You will get greater gains in performance when using bundling and minification with assets transferred over a network.</span></span>

## <a name="using-bundling-and-minification-in-a-project"></a><span data-ttu-id="54460-161">Verwenden in einem Projekt Bündelung und Minimierung</span><span class="sxs-lookup"><span data-stu-id="54460-161">Using bundling and minification in a project</span></span>

<span data-ttu-id="54460-162">Das MVC-Projektvorlage enthält eine `bundleconfig.json` Konfigurationsdatei an, die die Optionen für jedes Paket definiert.</span><span class="sxs-lookup"><span data-stu-id="54460-162">The MVC project template provides a `bundleconfig.json` configuration file which defines the options for each bundle.</span></span> <span data-ttu-id="54460-163">Standardmäßig wird eine Paket-Konfiguration für die benutzerdefinierte JavaScript definiert (`wwwroot/js/site.js`) und Stylesheet (`wwwroot/css/site.css`) Dateien.</span><span class="sxs-lookup"><span data-stu-id="54460-163">By default, a single bundle configuration is defined for the custom JavaScript (`wwwroot/js/site.js`) and Stylesheet (`wwwroot/css/site.css`) files.</span></span>

[!code-json[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/bundleconfig.json)]

<span data-ttu-id="54460-164">Bundle-Optionen:</span><span class="sxs-lookup"><span data-stu-id="54460-164">Bundle options include:</span></span>

* <span data-ttu-id="54460-165">OutputFileName - Namen der Paketdatei ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="54460-165">outputFileName - name of the bundle file to output.</span></span> <span data-ttu-id="54460-166">Einen relativen Pfad darf das `bundleconfig.json` Datei.</span><span class="sxs-lookup"><span data-stu-id="54460-166">Can contain a relative path from the `bundleconfig.json` file.</span></span> <span data-ttu-id="54460-167">**Erforderlich**</span><span class="sxs-lookup"><span data-stu-id="54460-167">**required**</span></span>
* <span data-ttu-id="54460-168">InputFiles - Array von Dateien bündeln.</span><span class="sxs-lookup"><span data-stu-id="54460-168">inputFiles - array of files to bundle together.</span></span> <span data-ttu-id="54460-169">Hierbei handelt es sich um relative Pfade in der Konfigurationsdatei.</span><span class="sxs-lookup"><span data-stu-id="54460-169">These are relative paths to the configuration file.</span></span> <span data-ttu-id="54460-170">**optionale**, * führt ein leerer Wert eine leere Ausgabedatei.</span><span class="sxs-lookup"><span data-stu-id="54460-170">**optional**, *an empty value results in an empty output file.</span></span> <span data-ttu-id="54460-171">[Globmodus](http://www.tldp.org/LDP/abs/html/globbingref.html) Muster werden unterstützt.</span><span class="sxs-lookup"><span data-stu-id="54460-171">[globbing](http://www.tldp.org/LDP/abs/html/globbingref.html) patterns are supported.</span></span>
* <span data-ttu-id="54460-172">verkleinernde - Minimierung von Optionen für die Ausgabe zu geben.</span><span class="sxs-lookup"><span data-stu-id="54460-172">minify - minification options for the output type.</span></span> <span data-ttu-id="54460-173">**optionale**, *Standard:`minify: { enabled: true }`*</span><span class="sxs-lookup"><span data-stu-id="54460-173">**optional**, *default - `minify: { enabled: true }`*</span></span>
  * <span data-ttu-id="54460-174">Konfigurationsoptionen sind pro Datei Ausgabetyp verfügbar.</span><span class="sxs-lookup"><span data-stu-id="54460-174">Configuration options are available per output file type.</span></span>
    * [<span data-ttu-id="54460-175">CSS-Minifier</span><span class="sxs-lookup"><span data-stu-id="54460-175">CSS Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [<span data-ttu-id="54460-176">JavaScript-Minifier</span><span class="sxs-lookup"><span data-stu-id="54460-176">JavaScript Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki)
    * [<span data-ttu-id="54460-177">HTML-Minifier</span><span class="sxs-lookup"><span data-stu-id="54460-177">HTML Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki)
* <span data-ttu-id="54460-178">IncludeInProject - generierte Dateien zur Projektdatei hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="54460-178">includeInProject - add generated files to project file.</span></span> <span data-ttu-id="54460-179">**optionale**, *Standard - "false"*</span><span class="sxs-lookup"><span data-stu-id="54460-179">**optional**, *default - false*</span></span>
* <span data-ttu-id="54460-180">SourceMaps - Quelle Zuordnungen für die gebündelte Datei zu generieren.</span><span class="sxs-lookup"><span data-stu-id="54460-180">sourceMaps - generate source maps for the bundled file.</span></span> <span data-ttu-id="54460-181">**optionale**, *Standard - "false"*</span><span class="sxs-lookup"><span data-stu-id="54460-181">**optional**, *default - false*</span></span>

### <a name="visual-studio-2015--2017"></a><span data-ttu-id="54460-182">Visual Studio 2015 / 2017</span><span class="sxs-lookup"><span data-stu-id="54460-182">Visual Studio 2015 / 2017</span></span>

<span data-ttu-id="54460-183">Open `bundleconfig.json` in Visual Studio, wenn Ihre Umgebung keine Erweiterung installiert; eine Aufforderung erhält angewiesen, den vorhanden ist, die mit diesem Dateityp unterstützen kann.</span><span class="sxs-lookup"><span data-stu-id="54460-183">Open `bundleconfig.json` in Visual Studio, if your environment does not have the extension installed; a prompt is presented suggesting that there is one that could assist with this file type.</span></span>

![Vorschlag BuildBundlerMinifier-Erweiterung](../client-side/bundling-and-minification/_static/bundler-extension-suggestion.png)

<span data-ttu-id="54460-185">Wählen Sie die Ansicht Erweiterungen, und Installieren der **Bundle & Minifier** Erweiterung (Visual Studio erfordert Neustart).</span><span class="sxs-lookup"><span data-stu-id="54460-185">Select View Extensions, and install the **Bundler & Minifier** extension (Requires Visual Studio restart).</span></span>

![Vorschlag BuildBundlerMinifier-Erweiterung](../client-side/bundling-and-minification/_static/view-extension.png)

<span data-ttu-id="54460-187">Wenn der Neustart abgeschlossen ist, müssen Sie den Build zum Ausführen der Prozesse weboptimierungsframework und bündeln die clientseitigen Objekte zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="54460-187">When the restart is complete, you need to configure the build to run the processes of minifying and bundling the client-side assets.</span></span> <span data-ttu-id="54460-188">Mit der rechten Maustaste die `bundleconfig.json` Datei, und wählen Sie *Enable-Paket beim Erstellen... *.</span><span class="sxs-lookup"><span data-stu-id="54460-188">Right-click the `bundleconfig.json` file and select *Enable bundle on build...*.</span></span>

<span data-ttu-id="54460-189">Erstellen Sie das Projekt, und die `bundleconfig.json` in den Buildprozess erzeugen die Ausgabedateien auf Basis der Konfiguration enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="54460-189">Build the project, and the `bundleconfig.json` is included in the build process to produce the output files based on the configuration.</span></span>

```console
1>------ Build started: Project: BuildBundlerMinifierExample, Configuration: Debug Any CPU ------
1>
1>Bundler: Begin processing bundleconfig.json
1>Bundler: Done processing bundleconfig.json
1>BuildBundlerMinifierExample -> C:\BuildBundlerMinifierExample\bin\Debug\netcoreapp1.1\BuildBundlerMinifierExample.dll
========== Build: 1 succeeded or up-to-date, 0 failed, 0 skipped ==========
```

### <a name="visual-studio-code-or-command-line"></a><span data-ttu-id="54460-190">Visual Studio-Code oder über die Befehlszeile</span><span class="sxs-lookup"><span data-stu-id="54460-190">Visual Studio Code or Command Line</span></span>

<span data-ttu-id="54460-191">Visual Studio und die Erweiterung Laufwerk den Bündelung und Minimierung-Prozess, die mit der GUI-Gesten; Allerdings stehen die gleichen Funktionen zur Verfügung, mit der `dotnet` CLI und BuildBundlerMinifier NuGet-Paket.</span><span class="sxs-lookup"><span data-stu-id="54460-191">Visual Studio and the extension drive the bundling and minification process using GUI gestures; however, the same capabilities are available with the `dotnet` CLI and BuildBundlerMinifier NuGet package.</span></span>

<span data-ttu-id="54460-192">Fügen Sie dem Projekt das NuGet-Paket hinzu:</span><span class="sxs-lookup"><span data-stu-id="54460-192">Add the NuGet package to your project:</span></span>

```console
dotnet add package BuildBundlerMinifier
```

<span data-ttu-id="54460-193">Stellen Sie die Abhängigkeiten wieder her:</span><span class="sxs-lookup"><span data-stu-id="54460-193">Restore the dependencies:</span></span>

```console
dotnet restore
```

<span data-ttu-id="54460-194">Erstellen Sie die app ein:</span><span class="sxs-lookup"><span data-stu-id="54460-194">Build the app:</span></span>

```console
dotnet build
```

<span data-ttu-id="54460-195">Die Ausgabe aus dem Buildbefehl zeigt die Ergebnisse der Minimierung und/oder bundling entsprechend was konfiguriert ist.</span><span class="sxs-lookup"><span data-stu-id="54460-195">The output from the build command shows the results of the minification and/or bundling according to what is configured.</span></span>

```console
Microsoft (R) Build Engine version 15.1.545.13942
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Begin processing bundleconfig.json
     Minified wwwroot/css/site.min.css
  Bundler: Done processing bundleconfig.json
  BuildBundlerMinifierExample -> /BuildBundlerMinifierExample/bin/Debug/netcoreapp1.0/BuildBundlerMinifierExample.dll
```

## <a name="adding-files"></a><span data-ttu-id="54460-196">Hinzufügen von Dateien</span><span class="sxs-lookup"><span data-stu-id="54460-196">Adding files</span></span>

<span data-ttu-id="54460-197">In diesem Beispiel wird eine zusätzliche CSS-Datei namens hinzugefügt `custom.css` und konfiguriert Sie für Bündelung und Minimierung mit `site.css`, wodurch ein einzelnes `site.min.css`.</span><span class="sxs-lookup"><span data-stu-id="54460-197">In this example, an additional CSS file is added called `custom.css` and configured for bundling and minification with `site.css`, resulting in a single `site.min.css`.</span></span>

<span data-ttu-id="54460-198">Custom.CSS</span><span class="sxs-lookup"><span data-stu-id="54460-198">custom.css</span></span>

```css
.about, [role=main], [role=complementary]
{
    margin-top: 60px;
}

footer
{
    margin-top: 10px;
}
```

<span data-ttu-id="54460-199">Fügen Sie den relativen Pfad zum `bundleconfig.json`.</span><span class="sxs-lookup"><span data-stu-id="54460-199">Add the relative path to `bundleconfig.json`.</span></span>

[!code-json[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/bundleconfig2.json)]

> [!NOTE]
> <span data-ttu-id="54460-200">Alternativ können Sie das Muster Globmodus missbraucht werden - `"inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]` Dateien und schließt die verkleinerte Dateimuster alle CSS abgerufen.</span><span class="sxs-lookup"><span data-stu-id="54460-200">Alternatively, the globbing pattern could be used - `"inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]` which gets all CSS files and excludes the minified file pattern.</span></span>

<span data-ttu-id="54460-201">Erstellen Sie die Anwendung, und öffnen Sie `site.min.css`, jetzt Beachten Sie, dass Inhalt von `custom.css` wurde an das Ende der Datei angefügt.</span><span class="sxs-lookup"><span data-stu-id="54460-201">Build the application and if you open `site.min.css`, you'll now notice that contents of `custom.css` has been appended to the end of the file.</span></span>

## <a name="controlling-bundling-and-minification"></a><span data-ttu-id="54460-202">Steuern der Bündelung und Minimierung</span><span class="sxs-lookup"><span data-stu-id="54460-202">Controlling bundling and minification</span></span>

<span data-ttu-id="54460-203">Im Allgemeinen möchten Sie die gebündelten und verkleinerte Dateien der app nur in einer produktionsumgebung verwenden.</span><span class="sxs-lookup"><span data-stu-id="54460-203">In general, you want to use the bundled and minified files of your app only in a production environment.</span></span> <span data-ttu-id="54460-204">Während der Entwicklung sollen Sie die ursprünglichen Dateien verwenden, damit Ihre app einfacher zu Debuggen ist.</span><span class="sxs-lookup"><span data-stu-id="54460-204">During development, you want to use your original files so your app is easier to debug.</span></span>

<span data-ttu-id="54460-205">Sie können angeben, welche Skripts und CSS-Dateien in Ihren Seiten verwenden die Umgebung Tag-Hilfsmethode in Ihrer Layoutseiten eingeschlossen werden sollen (finden Sie unter [Tag Hilfsprogramme](../mvc/views/tag-helpers/index.md)).</span><span class="sxs-lookup"><span data-stu-id="54460-205">You can specify which scripts and CSS files to include in your pages using the environment tag helper in your layout pages (see [Tag Helpers](../mvc/views/tag-helpers/index.md)).</span></span> <span data-ttu-id="54460-206">Die Umgebung Tags-Hilfsprogramm wird nur seinen Inhalt gerendert, wenn in bestimmten Umgebungen ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="54460-206">The environment tag helper will only render its contents when running in specific environments.</span></span> <span data-ttu-id="54460-207">Finden Sie unter [arbeiten mit mehreren Umgebungen](../fundamentals/environments.md) ausführliche Informationen zum Angeben der aktuellen Umgebung.</span><span class="sxs-lookup"><span data-stu-id="54460-207">See [Working with Multiple Environments](../fundamentals/environments.md) for details on specifying the current environment.</span></span>

<span data-ttu-id="54460-208">Die folgenden umgebungstag Rendern unverarbeiteten CSS-Dateien aus, bei der Ausführung im die `Development` Umgebung:</span><span class="sxs-lookup"><span data-stu-id="54460-208">The following environment tag will render the unprocessed CSS files when running in the `Development` environment:</span></span>

[!code-html[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/Views/Shared/_Layout.cshtml?highlight=3&range=9-12)]

<span data-ttu-id="54460-209">Diese umgebungs-Tag Rendern die gebündelten und verkleinerte CSS-Dateien aus, bei der Ausführung im `Production` oder `Staging`:</span><span class="sxs-lookup"><span data-stu-id="54460-209">This environment tag will render the bundled and minified CSS files only when running in `Production` or `Staging`:</span></span>

[!code-html[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/Views/Shared/_Layout.cshtml?highlight=5&range=13-18)]

## <a name="consuming-bundleconfigjson-from-gulp"></a><span data-ttu-id="54460-210">Verwenden von Gulp bundleconfig.json</span><span class="sxs-lookup"><span data-stu-id="54460-210">Consuming bundleconfig.json from Gulp</span></span>

<span data-ttu-id="54460-211">Wenn Ihre app Bündelung und Minimierung Workflow zusätzliche Prozesse wie das eine bildverarbeitung Cache Abwehrprogramm, CDN Assest Verarbeitung, usw. erforderlich sind, können Sie das Paket und Minify Prozess in Gulp konvertieren.</span><span class="sxs-lookup"><span data-stu-id="54460-211">If your app bundling and minification workflow requires additional processes such as image processing, cache busting, CDN assest processing, etc., then you can convert the Bundle and Minify process to Gulp.</span></span>

> [!NOTE]
> <span data-ttu-id="54460-212">Konvertierungsoption ist nur in Visual Studio 2015 und 2017 verfügbar.</span><span class="sxs-lookup"><span data-stu-id="54460-212">Conversion option only available in Visual Studio 2015 and 2017.</span></span>

<span data-ttu-id="54460-213">Mit der rechten Maustaste die `bundleconfig.json` , und wählen Sie **in Gulp konvertieren... **. Dadurch wird die `gulpfile.js` und Installieren der erforderlichen Npm-Pakete.</span><span class="sxs-lookup"><span data-stu-id="54460-213">Right-click the `bundleconfig.json` and select **Convert to Gulp...**. This will generate the `gulpfile.js` and install the necessary npm packages.</span></span>

![Konvertieren in Gulp](../client-side/bundling-and-minification/_static/convert-togulp.png)

<span data-ttu-id="54460-215">Die `gulpfile.js` Lesevorgänge erzeugt der `bundleconfig.json` Datei für die Konfiguration, sie können daher weiterhin für die Eingaben/Ausgaben und Einstellungen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="54460-215">The `gulpfile.js` produced reads the `bundleconfig.json` file for the configuration, therefore it can continue to be used for the inputs/outputs and settings.</span></span>

[!code-javascript[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/gulpfile.js)]

<span data-ttu-id="54460-216">Fügen Sie Folgendes in die Datei *.csproj, um Gulp zu aktivieren, wenn das Projekt in Visual Studio 2017 erstellt:</span><span class="sxs-lookup"><span data-stu-id="54460-216">To enable Gulp when the project builds in Visual Studio 2017, add the following to the *.csproj file:</span></span>

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
    <Exec Command="gulp min" />
</Target>
```

<span data-ttu-id="54460-217">Fügen Sie Folgendes, um Gulp zu aktivieren, wenn das Projekt in Visual Studio 2015 erstellt die `project.json` Datei:</span><span class="sxs-lookup"><span data-stu-id="54460-217">To enable Gulp when the project builds in Visual Studio 2015, add the following to the `project.json` file:</span></span>

```json
"scripts": {
    "precompile": "gulp min"
}
```

## <a name="additional-resources"></a><span data-ttu-id="54460-218">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="54460-218">Additional resources</span></span>

* [<span data-ttu-id="54460-219">Verwenden von Gulp</span><span class="sxs-lookup"><span data-stu-id="54460-219">Using Gulp</span></span>](using-gulp.md)
* [<span data-ttu-id="54460-220">Verwenden von Grunt</span><span class="sxs-lookup"><span data-stu-id="54460-220">Using Grunt</span></span>](using-grunt.md)
* [<span data-ttu-id="54460-221">Arbeiten mit mehreren Umgebungen</span><span class="sxs-lookup"><span data-stu-id="54460-221">Working with Multiple Environments</span></span>](../fundamentals/environments.md)
* [<span data-ttu-id="54460-222">Taghilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="54460-222">Tag Helpers</span></span>](../mvc/views/tag-helpers/index.md)
