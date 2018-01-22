---
title: Weniger, Schriftart, die in ASP.NET Core Awesome und Sass
author: ardalis
description: Erfahren Sie, wie kleiner Sass und Schriftart Awesome in ASP.NET Core-Anwendungen verwenden.
ms.author: tdykstra
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/less-sass-fa
ms.openlocfilehash: c3a53d6118a72c00d61d9139b05325fd1cbd53da
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-styling-applications-with-less-sass-and-font-awesome-in-aspnet-core"></a><span data-ttu-id="9207c-103">Einführung in Styling Anwendungen mit einem niedrigeren Sass und Schriftart Awesome in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9207c-103">Introduction to styling applications with Less, Sass, and Font Awesome in ASP.NET Core</span></span>

<span data-ttu-id="9207c-104">Durch [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="9207c-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="9207c-105">Benutzer des Webanwendungen haben zunehmend hohe Anforderungen an, wenn es darum geht, formatieren und allgemeine Erfahrung.</span><span class="sxs-lookup"><span data-stu-id="9207c-105">Users of web applications have increasingly high expectations when it comes to style and overall experience.</span></span> <span data-ttu-id="9207c-106">Innovative Webanwendungen nutzen häufig umfangreiche Tools und Frameworks zum Definieren und verwalten ihre Aussehen und Verhalten konsistent.</span><span class="sxs-lookup"><span data-stu-id="9207c-106">Modern web applications frequently leverage rich tools and frameworks for defining and managing their look and feel in a consistent manner.</span></span> <span data-ttu-id="9207c-107">Frameworks wie [Bootstrap](http://getbootstrap.com/) verlaufen eine lange gegen einen gemeinsamen Satz von Formatvorlagen und Layoutoptionen für Websites zu definieren.</span><span class="sxs-lookup"><span data-stu-id="9207c-107">Frameworks like [Bootstrap](http://getbootstrap.com/) can go a long way toward defining a common set of styles and layout options for web sites.</span></span> <span data-ttu-id="9207c-108">Die meisten nicht-trivialen Standorte jedoch profitieren auch von wird effektiv definieren und Verwalten von Formaten und cascading Stylesheet (CSS)-Dateien können als auch einfachen Zugriff auf nicht-Image-Symbole, mit denen der Standort-Schnittstelle eine intuitivere verfügen.</span><span class="sxs-lookup"><span data-stu-id="9207c-108">However, most non-trivial sites also benefit from being able to effectively define and maintain styles and cascading style sheet (CSS) files, as well as having easy access to non-image icons that help make the site's interface more intuitive.</span></span> <span data-ttu-id="9207c-109">Sind Sprachen und Tools, die Unterstützung von [weniger](http://lesscss.org/) und [Sass](http://sass-lang.com/), und Bibliotheken wie [Schriftart Awesome](http://fontawesome.io/), sind in verschiedenen Größen.</span><span class="sxs-lookup"><span data-stu-id="9207c-109">That's where languages and tools that support [Less](http://lesscss.org/) and [Sass](http://sass-lang.com/), and libraries like [Font Awesome](http://fontawesome.io/), come in.</span></span>

## <a name="css-preprocessor-languages"></a><span data-ttu-id="9207c-110">CSS-Präprozessor Sprachen</span><span class="sxs-lookup"><span data-stu-id="9207c-110">CSS preprocessor languages</span></span>

<span data-ttu-id="9207c-111">Sprachen, die in anderen Sprachen kompiliert werden, um die Erfahrung der Arbeit mit der zugrunde liegenden Sprache zu verbessern, werden als Präprozessoren bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="9207c-111">Languages that are compiled into other languages, in order to improve the experience of working with the underlying language, are referred to as preprocessors.</span></span> <span data-ttu-id="9207c-112">Es gibt zwei gängige Präprozessoren für CSS: weniger und Sass.</span><span class="sxs-lookup"><span data-stu-id="9207c-112">There are two popular preprocessors for CSS: Less and Sass.</span></span>  <span data-ttu-id="9207c-113">Diese Präprozessoren Hinzufügen von Funktionen zu CSS, z. B. Unterstützung für Variablen und geschachtelte Regeln, die wodurch die Verwaltbarkeit von großen, komplexen Stylesheets verbessert.</span><span class="sxs-lookup"><span data-stu-id="9207c-113">These preprocessors add features to CSS, such as support for variables and nested rules, which improve the maintainability of large, complex stylesheets.</span></span> <span data-ttu-id="9207c-114">CSS-Code als eine Sprache ist ein sehr einfaches fehlen Unterstützung auch für etwas so einfaches wie Variablen, und dies CSS-Dateien von und sehr großer stellen tendenziell.</span><span class="sxs-lookup"><span data-stu-id="9207c-114">CSS as a language is very basic, lacking support even for something as simple as variables, and this tends to make CSS files repetitive and bloated.</span></span> <span data-ttu-id="9207c-115">Durch das real Programmiersprachenfeatures über Präprozessoren hinzufügen können, verringern die Duplizierung und bieten eine bessere Organisation von Stilregeln.</span><span class="sxs-lookup"><span data-stu-id="9207c-115">Adding real programming language features via preprocessors can help reduce duplication and provide better organization of styling rules.</span></span> <span data-ttu-id="9207c-116">Visual Studio bietet integrierte Unterstützung für beide Less und Sass sowie Erweiterungen, die die Entwicklungsaufgaben weiter verbessern können, bei der Arbeit mit diesen Sprachen.</span><span class="sxs-lookup"><span data-stu-id="9207c-116">Visual Studio provides built-in support for both Less and Sass, as well as extensions that can further improve the development experience when working with these languages.</span></span>

<span data-ttu-id="9207c-117">Ein kurzes Beispiel wie Präprozessoren Lesbarkeit und verwaltbarkeit von Formatinformationen verbessern können sollten Sie dieses CSS:</span><span class="sxs-lookup"><span data-stu-id="9207c-117">As a quick example of how preprocessors can improve readability and maintainability of style information, consider this CSS:</span></span>

```css
.header {
    color: black;
    font-weight: bold;
    font-size: 18px;
    font-family: Helvetica, Arial, sans-serif;
}

.small-header {
    color: black;
    font-weight: bold;
    font-size: 14px;
    font-family: Helvetica, Arial, sans-serif;
}
```

<span data-ttu-id="9207c-118">Verwenden weniger, dies kann werden umgeschrieben, um alle diese Duplikation zu vermeiden mit eine *"mixin"* (so genannt, da es Ihnen ermöglicht, "mix in" Eigenschaften von einer Klasse oder in einen anderen Regelsatz):</span><span class="sxs-lookup"><span data-stu-id="9207c-118">Using Less, this can be rewritten to eliminate all of the duplication, using a *mixin* (so named because it allows you to "mix in" properties from one class or rule-set into another):</span></span>

```less
.header {
    color: black;
    font-weight: bold;
    font-size: 18px;
    font-family: Helvetica, Arial, sans-serif;
}

.small-header {
    .header;
    font-size: 14px;
}
```

## <a name="less"></a><span data-ttu-id="9207c-119">weniger</span><span class="sxs-lookup"><span data-stu-id="9207c-119">Less</span></span>

<span data-ttu-id="9207c-120">Der Präprozessor weniger CSS ausgeführt, die mit Node.js.</span><span class="sxs-lookup"><span data-stu-id="9207c-120">The Less CSS preprocessor runs using Node.js.</span></span> <span data-ttu-id="9207c-121">Verwenden Sie zum Installieren weniger Node Package Manager (Npm) über eine Eingabeaufforderung (-g bedeutet "global"):</span><span class="sxs-lookup"><span data-stu-id="9207c-121">To install Less, use Node Package Manager (npm) from a command prompt (-g means "global"):</span></span>

```console
npm install -g less
```

<span data-ttu-id="9207c-122">Wenn Sie Visual Studio verwenden, können Sie mit einem niedrigeren, indem eine oder mehrere weniger Dateien zum Projekt hinzufügen und konfigurieren dann Gulp (oder Grunt), dass sie zum Zeitpunkt der Kompilierung verarbeitet beginnen.</span><span class="sxs-lookup"><span data-stu-id="9207c-122">If you're using Visual Studio, you can get started with Less by adding one or more Less files to your project, and then configuring Gulp (or Grunt) to process them at compile-time.</span></span> <span data-ttu-id="9207c-123">Hinzufügen einer *Stile* Ordner, um das Projekt, und fügen Sie dann einen neuen weniger Datei mit dem Namen *main.less* in diesen Ordner.</span><span class="sxs-lookup"><span data-stu-id="9207c-123">Add a *Styles* folder to your project, and then add a new Less file named *main.less* to this folder.</span></span>

![Less-Datei hinzufügen](less-sass-fa/_static/add-less-file.png)

<span data-ttu-id="9207c-125">Sobald hinzugefügt wird, sollte die Ordnerstruktur etwa wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="9207c-125">Once added, your folder structure should look something like this:</span></span>

![Struktur des Veröffentlichungsordners](less-sass-fa/_static/folder-structure.png)

<span data-ttu-id="9207c-127">Jetzt können Sie einige grundlegende formatieren in der Datei hinzufügen, wird in CSS kompiliert und auf den Ordner "Wwwroot" Gulp bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="9207c-127">Now you can add some basic styling to the file, which will be compiled into CSS and deployed to the wwwroot folder by Gulp.</span></span>

<span data-ttu-id="9207c-128">Ändern Sie *main.less* , den folgenden Inhalt aufzunehmen, die eine einfache Farbpalette aus einer einzelnen Basisfarbe erstellt.</span><span class="sxs-lookup"><span data-stu-id="9207c-128">Modify *main.less* to include the following content, which creates a simple color palette from a single base color.</span></span>

```less
@base: #663333;
@background: spin(@base, 180);
@lighter: lighten(spin(@base, 5), 10%);
@lighter2: lighten(spin(@base, 10), 20%);
@darker: darken(spin(@base, -5), 10%);
@darker2: darken(spin(@base, -10), 20%);

body {
    background-color:@background;
}
.baseColor  {color:@base}
.bgLight    {color:@lighter}
.bgLight2   {color:@lighter2}
.bgDark     {color:@darker}
.bgDark2    {color:@darker2}
```

<span data-ttu-id="9207c-129">`@base`die andere @-prefixed Elemente sind Variablen.</span><span class="sxs-lookup"><span data-stu-id="9207c-129">`@base` and the other @-prefixed items are variables.</span></span> <span data-ttu-id="9207c-130">Jede von ihnen stellt eine Farbe dar.</span><span class="sxs-lookup"><span data-stu-id="9207c-130">Each of them represents a color.</span></span> <span data-ttu-id="9207c-131">Mit Ausnahme von `@base`, sie sind festgelegt, mit Farbe Funktionen: heller und dunkler zu starten.</span><span class="sxs-lookup"><span data-stu-id="9207c-131">Except for `@base`, they are set using color functions: lighten, darken, and spin.</span></span> <span data-ttu-id="9207c-132">Heller und dunkler sind größtenteils Erwartungen; Drehfeld passt den Farbton einer Farbe, um eine Anzahl von Grad (in der Nähe der Farbkreis).</span><span class="sxs-lookup"><span data-stu-id="9207c-132">Lighten and darken do pretty much what you would expect; spin adjusts the hue of a color by a number of degrees (around the color wheel).</span></span> <span data-ttu-id="9207c-133">Der weniger Prozessor ist intelligent genug, um Variablen, die nicht verwendet werden, zu ignorieren, daher zur Veranschaulichung, wie diese Variablen verwendet, sie an einer beliebigen Stelle verwendet müssen.</span><span class="sxs-lookup"><span data-stu-id="9207c-133">The Less processor is smart enough to ignore variables that aren't used, so to demonstrate how these variables work, we need to use them somewhere.</span></span> <span data-ttu-id="9207c-134">Die Klassen `.baseColor`, usw. wird gezeigt, das die berechneten Werte aller Variablen in der CSS-Datei, das erzeugt wird.</span><span class="sxs-lookup"><span data-stu-id="9207c-134">The classes `.baseColor`, etc. will demonstrate the calculated values of each of the variables in the CSS file that is produced.</span></span>

### <a name="getting-started"></a><span data-ttu-id="9207c-135">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="9207c-135">Getting started</span></span>

<span data-ttu-id="9207c-136">Erstellen einer **Npm-Konfigurationsdatei** (*"Package.JSON"*) in den Projektordner und bearbeiten es verweisen `gulp` und `gulp-less`:</span><span class="sxs-lookup"><span data-stu-id="9207c-136">Create an **npm Configuration File** (*package.json*) in your project folder and edit it to reference `gulp` and `gulp-less`:</span></span>

```json
{
  "version": "1.0.0",
  "name": "asp.net",
  "private": true,
  "devDependencies": {
    "gulp": "3.9.1",
    "gulp-less": "3.3.0"
  }
}
```

<span data-ttu-id="9207c-137">Installieren von Abhängigkeiten entweder an der Eingabeaufforderung in den Projektordner oder in Visual Studio **Projektmappen-Explorer** (**Abhängigkeiten > Npm > Wiederherstellen von Paketen**).</span><span class="sxs-lookup"><span data-stu-id="9207c-137">Install the dependencies either at a command prompt in your project folder, or in Visual Studio **Solution Explorer** (**Dependencies > npm > Restore packages**).</span></span>

```console
npm install
```

![VS stellen Pakete wieder her.](less-sass-fa/_static/restore-packages.png)

<span data-ttu-id="9207c-139">Erstellen Sie in den Projektordner ein **Konfigurationsdatei Gulp** (*gulpfile.js*) um den automatisierten Prozess zu definieren.</span><span class="sxs-lookup"><span data-stu-id="9207c-139">In the project folder, create a **Gulp Configuration File** (*gulpfile.js*) to define the automated process.</span></span>  <span data-ttu-id="9207c-140">Fügen Sie am Anfang der Datei, um weniger darzustellen und ein Task kleiner ausgeführt, eine Variable hinzu:</span><span class="sxs-lookup"><span data-stu-id="9207c-140">Add a variable at the top of the file to represent Less, and a task to run Less:</span></span>

```javascript
var gulp = require("gulp"),
  fs = require("fs"),
  less = require("gulp-less");

gulp.task("less", function () {
  return gulp.src('Styles/main.less')
    .pipe(less())
    .pipe(gulp.dest('wwwroot/css'));
});
```

<span data-ttu-id="9207c-141">Öffnen der **Task Runner-Explorer** (**Ansicht > andere Fenster > Task Runner-Explorer**).</span><span class="sxs-lookup"><span data-stu-id="9207c-141">Open the **Task Runner Explorer** (**View > Other Windows > Task Runner Explorer**).</span></span> <span data-ttu-id="9207c-142">Für die Aufgaben sehen Sie eine neue Aufgabe, die mit dem Namen `less`.</span><span class="sxs-lookup"><span data-stu-id="9207c-142">Among the tasks, you should see a new task named `less`.</span></span> <span data-ttu-id="9207c-143">Sie müssen möglicherweise das Fenster zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="9207c-143">You might have to refresh the window.</span></span>

<span data-ttu-id="9207c-144">Führen Sie die `less` Aufgabe und finden Sie ähnliche Informationen wie hier gezeigt wird:</span><span class="sxs-lookup"><span data-stu-id="9207c-144">Run the `less` task, and you see output similar to what is shown here:</span></span>

![weniger Task runner](less-sass-fa/_static/less-task-runner.png)

<span data-ttu-id="9207c-146">Die *"Wwwroot" / Css* Ordner enthält jetzt eine neue Datei *main.css*:</span><span class="sxs-lookup"><span data-stu-id="9207c-146">The *wwwroot/css* folder now contains a new file, *main.css*:</span></span>

![Main Css erstellt](less-sass-fa/_static/main-css-created.png)

<span data-ttu-id="9207c-148">Open *main.css* und etwa Folgendes angezeigt:</span><span class="sxs-lookup"><span data-stu-id="9207c-148">Open *main.css* and you see something like the following:</span></span>

```css
body {
    background-color: #336666;
}
.baseColor {
    color: #663333;
}
.bgLight {
    color: #884a44;
}
.bgLight2 {
    color: #aa6355;
}
.bgDark {
    color: #442225;
}
.bgDark2 {
    color: #221114;
}
```

<span data-ttu-id="9207c-149">Fügen Sie eine einfache HTML-Seite, um die *"Wwwroot"* Ordner und Verweis *main.css* die Farbpalette in Aktion zu sehen.</span><span class="sxs-lookup"><span data-stu-id="9207c-149">Add a simple HTML page to the *wwwroot* folder, and reference *main.css* to see the color palette in action.</span></span>

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <link href="css/main.css" rel="stylesheet" />
    <title></title>
</head>
<body>
    <div>
        <div class="baseColor">BaseColor</div>
        <div class="bgLight">Light</div>
        <div class="bgLight2">Light2</div>
        <div class="bgDark">Dark</div>
        <div class="bgDark2">Dark2</div>
    </div>
</body>
</html>
```

<span data-ttu-id="9207c-150">Sehen Sie, dass auf der 180 Grad drehen `@base` verwendet, um erzeugen `@background` Farbkreis gegensätzlichen Farbe des Verzeichnisdiensts `@base`:</span><span class="sxs-lookup"><span data-stu-id="9207c-150">You can see that the 180 degree spin on `@base` used to produce `@background` resulted in the color wheel opposing color of `@base`:</span></span>

![Beispiel für weniger Tests](less-sass-fa/_static/less-test-screenshot.png)

<span data-ttu-id="9207c-152">Weniger bietet auch Unterstützung für geschachtelte Regeln als auch geschachtelte Abfragen.</span><span class="sxs-lookup"><span data-stu-id="9207c-152">Less also provides support for nested rules, as well as nested media queries.</span></span> <span data-ttu-id="9207c-153">Z. B. wie definierende geschachtelte Hierarchien, so wie Menüs ausführliche CSS-Regeln führen können diese:</span><span class="sxs-lookup"><span data-stu-id="9207c-153">For example, defining nested hierarchies like menus can result in verbose CSS rules like these:</span></span>

```css
nav {
    height: 40px;
    width: 100%;
}
nav li {
    height: 38px;
    width: 100px;
}
nav li a:link {
    color: #000;
    text-decoration: none;
}
nav li a:visited {
    text-decoration: none;
    color: #CC3333;
}
nav li a:hover {
    text-decoration: underline;
    font-weight: bold;
}
nav li a:active {
    text-decoration: underline;
}
```

<span data-ttu-id="9207c-154">Im Idealfall aller der zugehörigen Stilregeln wird zusammengestellt werden in der CSS-Datei, aber in der Praxis ist ' Nothing ' erzwingen diese Regel außer Konvention und ggf. Kommentare.</span><span class="sxs-lookup"><span data-stu-id="9207c-154">Ideally all of the related style rules will be placed together within the CSS file, but in practice there is nothing enforcing this rule except convention and perhaps block comments.</span></span>

<span data-ttu-id="9207c-155">Definieren dieselben Regeln mit weniger sieht wie folgt:</span><span class="sxs-lookup"><span data-stu-id="9207c-155">Defining these same rules using Less looks like this:</span></span>

```less
nav {
    height: 40px;
    width: 100%;
    li {
        height: 38px;
        width: 100px;
        a {
            color: #000;
            &:link { text-decoration:none}
            &:visited { color: #CC3333; text-decoration:none}
            &:hover { text-decoration:underline; font-weight:bold}
            &:active {text-decoration:underline}
        }
    }
}
```

<span data-ttu-id="9207c-156">Beachten Sie, dass in diesem Fall werden alle untergeordneten Elemente des `nav` in ihrem Gültigkeitsbereich enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="9207c-156">Note that in this case, all of the subordinate elements of `nav` are contained within its scope.</span></span> <span data-ttu-id="9207c-157">Es ist nicht mehr jeder Wiederholung der übergeordneten Elemente (`nav`, `li`, `a`), und die Anzahl der Zeilen gesamt wurde (obwohl einige der, ist das Ergebnis verstanden, Werte in der gleichen Zeilen im zweiten Beispiel platzieren) ebenfalls gelöscht.</span><span class="sxs-lookup"><span data-stu-id="9207c-157">There is no longer any repetition of parent elements (`nav`, `li`, `a`), and the total line count has dropped as well (though some of that is a result of putting values on the same lines in the second example).</span></span> <span data-ttu-id="9207c-158">Es kann sehr hilfreich, OUI, um alle Regeln für ein angegebenes Benutzeroberflächenautomatisierungs-Element in einem explizit begrenzten Bereich, in diesem Fall finden Sie unter vom Rest der Datei in geschweiften Klammern festgelegt werden deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="9207c-158">It can be very helpful, organizationally, to see all of the rules for a given UI element within an explicitly bounded scope, in this case set off from the rest of the file by curly braces.</span></span>

<span data-ttu-id="9207c-159">Die `&` Syntax ist ein kleiner Auswahlzeiger-Feature, mit & des aktuellen Selektor übergeordneten Elements darstellt.</span><span class="sxs-lookup"><span data-stu-id="9207c-159">The `&` syntax is a Less selector feature, with & representing the current selector parent.</span></span> <span data-ttu-id="9207c-160">Ja, in dem eine {...}</span><span class="sxs-lookup"><span data-stu-id="9207c-160">So, within the a {...}</span></span> <span data-ttu-id="9207c-161">Block `&` stellt eine `a` Tag, und somit `&:link` entspricht `a:link`.</span><span class="sxs-lookup"><span data-stu-id="9207c-161">block, `&` represents an `a` tag, and thus `&:link` is equivalent to `a:link`.</span></span>

<span data-ttu-id="9207c-162">Medienabfragen, äußerst nützlich, beim Erstellen reaktionsfähiger Entwürfe können auch stark zur Wiederholung und Komplexität in CSS beitragen.</span><span class="sxs-lookup"><span data-stu-id="9207c-162">Media queries, extremely useful in creating responsive designs, can also contribute heavily to repetition and complexity in CSS.</span></span> <span data-ttu-id="9207c-163">Kleiner ermöglicht Media Abfragen innerhalb von Klassen, geschachtelt werden, damit die gesamte Klassendefinition nicht in anderen wiederholt werden muss der obersten Ebene `@media` Elemente.</span><span class="sxs-lookup"><span data-stu-id="9207c-163">Less allows media queries to be nested within classes, so that the entire class definition doesn't need to be repeated within different top-level `@media` elements.</span></span> <span data-ttu-id="9207c-164">Hier ist z. B. CSS für ein Menü reagiert:</span><span class="sxs-lookup"><span data-stu-id="9207c-164">For example, here is CSS for a responsive menu:</span></span>

```css
.navigation {
    margin-top: 30%;
    width: 100%;
}
@media screen and (min-width: 40em) {
    .navigation {
        margin: 0;
    }
}
@media screen and (min-width: 62em) {
    .navigation {
        width: 960px;
        margin: 0;
    }
}
```

<span data-ttu-id="9207c-165">Dies kann eine bessere Leistung in weniger als definiert werden:</span><span class="sxs-lookup"><span data-stu-id="9207c-165">This can be better defined in Less as:</span></span>

```less
.navigation {
    margin-top: 30%;
    width: 100%;
    @media screen and (min-width: 40em) {
        margin: 0;
    }
    @media screen and (min-width: 62em) {
        width: 960px;
        margin: 0;
    }
}
```

<span data-ttu-id="9207c-166">Eine weitere Funktion von weniger als die bereits gesehen haben, ist die Unterstützung für mathematische Operationen, sodass Stilattribute mit vordefinierten Variablen erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="9207c-166">Another feature of Less that we have already seen is its support for mathematical operations, allowing style attributes to be constructed from pre-defined variables.</span></span> <span data-ttu-id="9207c-167">Dadurch wird die verwandte Stile wesentlich einfacher aktualisieren, da die Basis-Variable geändert werden kann, und alle abhängigen Werte automatisch geändert.</span><span class="sxs-lookup"><span data-stu-id="9207c-167">This makes updating related styles much easier, since the base variable can be modified and all dependent values change automatically.</span></span>

<span data-ttu-id="9207c-168">CSS-Dateien, insbesondere bei umfangreichen Websites (und insbesondere wenn Medienabfragen verwendet werden), in der Regel im Laufe der Zeit sehr groß abrufen deren Verwendung unhandlich vornehmen.</span><span class="sxs-lookup"><span data-stu-id="9207c-168">CSS files, especially for large sites (and especially if media queries are being used), tend to get quite large over time, making working with them unwieldy.</span></span> <span data-ttu-id="9207c-169">Less-Dateien können separat definiert werden, dann herausgezogen zusammen mit `@import` Direktiven.</span><span class="sxs-lookup"><span data-stu-id="9207c-169">Less files can be defined separately, then pulled together using `@import` directives.</span></span> <span data-ttu-id="9207c-170">Weniger kann auch zum Importieren der einzelner CSS-Dateien, die auch bei Bedarf verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="9207c-170">Less can also be used to import individual CSS files, as well, if desired.</span></span>

<span data-ttu-id="9207c-171">*Mixins* Parameter annehmen können und weniger unterstützt bedingten Logik in Form von "mixin" schützt, die eine deklarative Methode definieren, wenn bestimmte Mixins wirksam bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="9207c-171">*Mixins* can accept parameters, and Less supports conditional logic in the form of mixin guards, which provide a declarative way to define when certain mixins take effect.</span></span> <span data-ttu-id="9207c-172">Eine übliche Verwendung für "mixin" Wächter ist basierte auf wie Farben anpassen oder dunkel der Quellfarbe.</span><span class="sxs-lookup"><span data-stu-id="9207c-172">A common use for mixin guards is to adjust colors based on how light or dark the source color is.</span></span> <span data-ttu-id="9207c-173">Erhält eine "mixin", die einen Parameter für die Farbe des akzeptiert, kann ein Wächter "mixin" verwendet werden, so ändern Sie die Grundlage der jeweiligen Farbe "mixin":</span><span class="sxs-lookup"><span data-stu-id="9207c-173">Given a mixin that accepts a parameter for color, a mixin guard can be used to modify the mixin based on that color:</span></span>

```less
.box (@color) when (lightness(@color) >= 50%) {
    background-color: #000;
}
.box (@color) when (lightness(@color) < 50%) {
    background-color: #FFF;
}
.box (@color) {
    color: @color;
}

.feature {
    .box (@base);
}
```

<span data-ttu-id="9207c-174">Unsere aktuellen angegebenen `@base` Wert `#663333`, weniger dieses Skript erzeugt die folgende CSS:</span><span class="sxs-lookup"><span data-stu-id="9207c-174">Given our current `@base` value of `#663333`, this Less script will produce the following CSS:</span></span>

```css
.feature {
    background-color: #FFF;
    color: #663333;
}
```

<span data-ttu-id="9207c-175">Weniger bietet eine Reihe von zusätzlichen Funktionen, aber dies sollten Sie einen Eindruck davon die Leistungsfähigkeit dieser vorverarbeiten Language.</span><span class="sxs-lookup"><span data-stu-id="9207c-175">Less provides a number of additional features, but this should give you some idea of the power of this preprocessing language.</span></span>

## <a name="sass"></a><span data-ttu-id="9207c-176">Sass</span><span class="sxs-lookup"><span data-stu-id="9207c-176">Sass</span></span>

<span data-ttu-id="9207c-177">Sass ähnelt, bietet Unterstützung für viele der gleichen Funktionen, jedoch mit etwas andere Syntax.</span><span class="sxs-lookup"><span data-stu-id="9207c-177">Sass is similar to Less, providing support for many of the same features, but with slightly different syntax.</span></span> <span data-ttu-id="9207c-178">Er basiert, Ruby, statt JavaScript und besitzt somit unterschiedliche einrichtungsanforderungen auszeichnen.</span><span class="sxs-lookup"><span data-stu-id="9207c-178">It is built using Ruby, rather than JavaScript, and so has different setup requirements.</span></span> <span data-ttu-id="9207c-179">Die Originalsprache Sass wurde nicht in geschweifte oder Semikolons verwendet jedoch stattdessen Bereichen mithilfe von Leerraum und Einzüge definiert.</span><span class="sxs-lookup"><span data-stu-id="9207c-179">The original Sass language did not use curly braces or semicolons, but instead defined scope using white space and indentation.</span></span> <span data-ttu-id="9207c-180">Eine neue Syntax in Version 3 des Sass wurde eingeführt, **SCSS** ("Sassy CSS").</span><span class="sxs-lookup"><span data-stu-id="9207c-180">In version 3 of Sass, a new syntax was introduced, **SCSS** ("Sassy CSS").</span></span> <span data-ttu-id="9207c-181">SCSS ähnelt CSS Einzugsebenen und Leerzeichen werden ignoriert, und verwendet stattdessen Semikolons und geschweifte Klammern.</span><span class="sxs-lookup"><span data-stu-id="9207c-181">SCSS is similar to CSS in that it ignores indentation levels and whitespace, and instead uses semicolons and curly braces.</span></span>

<span data-ttu-id="9207c-182">Um Sass zu installieren, in der Regel Sie Erstinstallation Ruby (vorinstalliertem auf Mac), und führen Sie dann:</span><span class="sxs-lookup"><span data-stu-id="9207c-182">To install Sass, typically you would first install Ruby (pre-installed on Mac), and then run:</span></span>

```console
gem install sass
```

<span data-ttu-id="9207c-183">Jedoch wenn Sie Visual Studio ausführen heraus, können Sie mit Sass im Wesentlichen die gleiche Weise loslegen wie bei kleiner.</span><span class="sxs-lookup"><span data-stu-id="9207c-183">However, if you're running Visual Studio, you can get started with Sass in much the same way as you would with Less.</span></span> <span data-ttu-id="9207c-184">Open *"Package.JSON"* und fügen Sie das Paket "Gulp Sass" `devDependencies`:</span><span class="sxs-lookup"><span data-stu-id="9207c-184">Open *package.json* and add the "gulp-sass" package to `devDependencies`:</span></span>

```json
"devDependencies": {
  "gulp": "3.9.1",
  "gulp-less": "3.3.0",
  "gulp-sass": "3.1.0"
}
```

<span data-ttu-id="9207c-185">Im nächsten Schritt ändern *gulpfile.js* $a Sass und eine Aufgabe Sass Dateien zu kompilieren, und legen die Ergebnisse in den Ordner "Wwwroot" hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="9207c-185">Next, modify *gulpfile.js* to add a sass variable and a task to compile your Sass files and place the results in the wwwroot folder:</span></span>

```javascript
var gulp = require("gulp"),
  fs = require("fs"),
  less = require("gulp-less"),
  sass = require("gulp-sass");

// other content removed

gulp.task("sass", function () {
  return gulp.src('Styles/main2.scss')
    .pipe(sass())
    .pipe(gulp.dest('wwwroot/css'));
});
```

<span data-ttu-id="9207c-186">Nun Sie die Datei Sass fügen *main2.scss* auf die *Stile* Ordner im Stammverzeichnis des Projekts:</span><span class="sxs-lookup"><span data-stu-id="9207c-186">Now you can add the Sass file *main2.scss* to the *Styles* folder in the root of the project:</span></span>

![Scss-Datei hinzufügen](less-sass-fa/_static/add-scss-file.png)

<span data-ttu-id="9207c-188">Open *main2.scss* und fügen Sie Folgendes hinzu:</span><span class="sxs-lookup"><span data-stu-id="9207c-188">Open *main2.scss* and add the following:</span></span>

```sass
$base: #CC0000;
body {
    background-color: $base;
}
```

<span data-ttu-id="9207c-189">Speichern Sie alle Dateien aus.</span><span class="sxs-lookup"><span data-stu-id="9207c-189">Save all of your files.</span></span> <span data-ttu-id="9207c-190">Wenn Sie eine Aktualisierung ist momentan **Taskausführungs-Explorer**, sehen Sie eine `sass` Aufgabe.</span><span class="sxs-lookup"><span data-stu-id="9207c-190">Now when you refresh **Task Runner Explorer**, you see a `sass` task.</span></span> <span data-ttu-id="9207c-191">Führen Sie ihn, und suchen Sie der */Wwwroot/Css* Ordner.</span><span class="sxs-lookup"><span data-stu-id="9207c-191">Run it, and look in the */wwwroot/css* folder.</span></span> <span data-ttu-id="9207c-192">Es ist jetzt eine *main2.css* Datei, mit dem folgenden Inhalt:</span><span class="sxs-lookup"><span data-stu-id="9207c-192">There is now a *main2.css* file, with these contents:</span></span>

```css
body {
    background-color: #CC0000;
}
```

<span data-ttu-id="9207c-193">Sass unterstützt die Schachtelung im Wesentlichen identisch, die kleiner ist, bietet ähnliche Vorteile, war.</span><span class="sxs-lookup"><span data-stu-id="9207c-193">Sass supports nesting in much the same was that Less does, providing similar benefits.</span></span> <span data-ttu-id="9207c-194">Dateien können von Funktion aufgeteilt werden und enthalten, mit der `@import` Richtlinie:</span><span class="sxs-lookup"><span data-stu-id="9207c-194">Files can be split up by function and included using the `@import` directive:</span></span>

```sass
@import 'anotherfile';
```

<span data-ttu-id="9207c-195">Sass unterstützt Mixins auch mithilfe der `@mixin` Schlüsselwort zu ihrer Definition und `@include` zum Einschließen wie in diesem Beispiel aus [Sass lang.com](http://sass-lang.com):</span><span class="sxs-lookup"><span data-stu-id="9207c-195">Sass supports mixins as well, using the `@mixin` keyword to define them and `@include` to include them, as in this example from [sass-lang.com](http://sass-lang.com):</span></span>

```sass
@mixin border-radius($radius) {
    -webkit-border-radius: $radius;
    -moz-border-radius: $radius;
    -ms-border-radius: $radius;
    border-radius: $radius;
}

.box { @include border-radius(10px); }
```

<span data-ttu-id="9207c-196">Zusätzlich zu den Mixins unterstützt Sass auch das Konzept der Vererbung ermöglicht eine Klasse für eine andere erweitern.</span><span class="sxs-lookup"><span data-stu-id="9207c-196">In addition to mixins, Sass also supports the concept of inheritance, allowing one class to extend another.</span></span> <span data-ttu-id="9207c-197">Es ist grundsätzlich ein "mixin", jedoch führt zu weniger CSS-Code ähnelt.</span><span class="sxs-lookup"><span data-stu-id="9207c-197">It's conceptually similar to a mixin, but results in less CSS code.</span></span> <span data-ttu-id="9207c-198">Es erfolgt mithilfe der `@extend` Schlüsselwort.</span><span class="sxs-lookup"><span data-stu-id="9207c-198">It's accomplished using the `@extend` keyword.</span></span> <span data-ttu-id="9207c-199">Um Mixins testen, fügen Sie Folgendes verwenden, um Ihre *main2.scss* Datei:</span><span class="sxs-lookup"><span data-stu-id="9207c-199">To try out mixins, add the following to your *main2.scss* file:</span></span>

```sass
@mixin alert {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    @include alert;
    border-color: green;
}

.error {
    @include alert;
    color: red;
    border-color: red;
    font-weight:bold;
}
```

<span data-ttu-id="9207c-200">Überprüfen Sie die Ausgabe in *main2.css* nach dem Ausführen der `sass` Task "" **Taskausführungs-Explorer**:</span><span class="sxs-lookup"><span data-stu-id="9207c-200">Examine the output in *main2.css* after running the `sass` task in **Task Runner Explorer**:</span></span>

```css
.success {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
    border-color: green;
 }

.error {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
    color: red;
    border-color: red;
    font-weight: bold;
}
```

<span data-ttu-id="9207c-201">Beachten Sie, dass alle von der allgemeinen Eigenschaften der Warnung "mixin" in jeder Klasse wiederholt werden.</span><span class="sxs-lookup"><span data-stu-id="9207c-201">Notice that all of the common properties of the alert mixin are repeated in each class.</span></span> <span data-ttu-id="9207c-202">Die "mixin" hat ein gutes unterstützen Duplizierung zur Entwicklungszeit zu vermeiden, aber es ist immer noch CSS erstellen, erhalten Sie viele Duplikate, wodurch größer als der erforderlichen CSS-Dateien: ein mögliches Leistungsproblem.</span><span class="sxs-lookup"><span data-stu-id="9207c-202">The mixin did a good job of helping eliminate duplication at development time, but it's still creating CSS with a lot of duplication in it, resulting in larger than necessary CSS files - a potential performance issue.</span></span>

<span data-ttu-id="9207c-203">Jetzt ersetzen die Warnung "mixin" mit einer `.alert` Klasse, und ändern Sie `@include` zu `@extend` (Denken Sie daran zu erweitern `.alert`, nicht `alert`):</span><span class="sxs-lookup"><span data-stu-id="9207c-203">Now replace the alert mixin with a `.alert` class, and change `@include` to `@extend` (remembering to extend `.alert`, not `alert`):</span></span>

```sass
.alert {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    @extend .alert;
    border-color: green;
}

.error {
    @extend .alert;
    color: red;
    border-color: red;
    font-weight:bold;
}
```

<span data-ttu-id="9207c-204">Sass einmal ausgeführt, und überprüfen Sie die resultierende CSS:</span><span class="sxs-lookup"><span data-stu-id="9207c-204">Run Sass once more, and examine the resulting CSS:</span></span>

```css
.alert, .success, .error {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    border-color: green;
}

.error {
    color: red;
    border-color: red;
    font-weight: bold;
}
```

<span data-ttu-id="9207c-205">Jetzt werden die Eigenschaften definiert, nur so oft wie erforderlich, und eine bessere Leistung CSS generiert wird.</span><span class="sxs-lookup"><span data-stu-id="9207c-205">Now the properties are defined only as many times as needed, and better CSS is generated.</span></span>

<span data-ttu-id="9207c-206">Sass enthält auch Funktionen und Bedingungslogik Vorgänge, die weniger ähnlich.</span><span class="sxs-lookup"><span data-stu-id="9207c-206">Sass also includes functions and conditional logic operations, similar to Less.</span></span> <span data-ttu-id="9207c-207">In der Tat sind die beiden Sprachen Funktionen sehr ähnlich.</span><span class="sxs-lookup"><span data-stu-id="9207c-207">In fact, the two languages' capabilities are very similar.</span></span>

## <a name="less-or-sass"></a><span data-ttu-id="9207c-208">Weniger oder Sass?</span><span class="sxs-lookup"><span data-stu-id="9207c-208">Less or Sass?</span></span>

<span data-ttu-id="9207c-209">Es ist immer noch keine Übereinstimmung darüber, ob es im Allgemeinen besser, weniger oder Sass (oder auch, ob die ursprünglichen Sass oder die neuere SCSS-Syntax im Sass bevorzugt).</span><span class="sxs-lookup"><span data-stu-id="9207c-209">There is still no consensus as to whether it's generally better to use Less or Sass (or even whether to prefer the original Sass or the newer SCSS syntax within Sass).</span></span> <span data-ttu-id="9207c-210">Die wichtigste Entscheidung ist möglicherweise auf **verwenden Sie eines der folgenden Tools**, im Gegensatz zu nur handcodierung von CSS-Dateien.</span><span class="sxs-lookup"><span data-stu-id="9207c-210">Probably the most important decision is to **use one of these tools**, as opposed to just hand-coding your CSS files.</span></span> <span data-ttu-id="9207c-211">Nachdem Sie, die vorgenommen haben Entscheidungsstrukturen, beide Less und Sass sind eine gute Wahl.</span><span class="sxs-lookup"><span data-stu-id="9207c-211">Once you've made that decision, both Less and Sass are good choices.</span></span>

## <a name="font-awesome"></a><span data-ttu-id="9207c-212">Schriftart Awesome</span><span class="sxs-lookup"><span data-stu-id="9207c-212">Font Awesome</span></span>

<span data-ttu-id="9207c-213">Zusätzlich zu CSS-Präprozessoren ist eine andere hervorragende Ressource zum Erstellen moderner Webanwendungen Awesome Schriftart.</span><span class="sxs-lookup"><span data-stu-id="9207c-213">In addition to CSS preprocessors, another great resource for styling modern web applications is Font Awesome.</span></span> <span data-ttu-id="9207c-214">Schriftart Umwerfend ist ein Toolkit, das mehr als 500 skalierbare Vektorgrafiken Symbole bereitstellt, mit dem kostenlos in Ihrer Webanwendungen verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="9207c-214">Font Awesome is a toolkit that provides over 500 scalable vector icons that can be freely used in your web applications.</span></span> <span data-ttu-id="9207c-215">Es wurde ursprünglich dienen zum Arbeiten mit Bootstrap, aber es ist nicht auf das Zielframework oder JavaScript-Bibliotheken.</span><span class="sxs-lookup"><span data-stu-id="9207c-215">It was originally designed to work with Bootstrap, but it has no dependency on that framework or on any JavaScript libraries.</span></span>

<span data-ttu-id="9207c-216">Die einfachste Möglichkeit zum Einstieg in Schriftart Awesome ist einen Verweis darauf, über seine öffentliche Content Delivery Network (CDN)-Speicherort hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="9207c-216">The easiest way to get started with Font Awesome is to add a reference to it, using its public content delivery network (CDN) location:</span></span>

```html
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css">
```

<span data-ttu-id="9207c-217">Sie können auch ihn hinzufügen Ihrer Visual Studio-Projekt durch Hinzufügen zum "Abhängigkeiten" in *"bower.JSON"*:</span><span class="sxs-lookup"><span data-stu-id="9207c-217">You can also add it to your Visual Studio project by adding it to the "dependencies" in *bower.json*:</span></span>

```json
{
  "name": "ASP.NET",
  "private": true,
  "dependencies": {
    "bootstrap": "3.0.0",
    "jquery": "1.10.2",
    "jquery-validation": "1.11.1",
    "jquery-validation-unobtrusive": "3.2.2",
    "hammer.js": "2.0.4",
    "bootstrap-touch-carousel": "0.8.0",
    "Font-Awesome": "4.3.0"
  }
}
```

<span data-ttu-id="9207c-218">Nachdem Sie einen Verweis auf die Schriftart Awesome auf einer Seite haben, können Sie Symbole für Ihre Anwendung hinzufügen, indem Sie die Schriftart Awesome Klassen, die in der Regel mit dem Präfix "Fa-", um Ihre Inline-HTML-Elemente anwenden (z. B. `<span>` oder `<i>`).</span><span class="sxs-lookup"><span data-stu-id="9207c-218">Once you have a reference to Font Awesome on a page, you can add icons to your application by applying Font Awesome classes, typically prefixed with "fa-", to your inline HTML elements (such as `<span>` or `<i>`).</span></span>  <span data-ttu-id="9207c-219">Beispielsweise können Sie Symbole einfacher Listen und Menüs, die mithilfe von Code wie folgt hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="9207c-219">For example, you can add icons to simple lists and menus using code like this:</span></span>

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title></title>
    <link href="lib/font-awesome/css/font-awesome.css" rel="stylesheet" />
</head>
<body>
    <ul class="fa-ul">
        <li><i class="fa fa-li fa-home"></i> Home</li>
        <li><i class="fa fa-li fa-cog"></i> Settings</li>
    </ul>
</body>
</html>
```

<span data-ttu-id="9207c-220">Dies erzeugt die folgenden im Browser – Beachten Sie das Symbol neben jedem Element:</span><span class="sxs-lookup"><span data-stu-id="9207c-220">This produces the following in the browser - note the icon beside each item:</span></span>

![Symbole](less-sass-fa/_static/list-icons-screenshot.png)

<span data-ttu-id="9207c-222">Sie können eine vollständige Liste der verfügbaren Symbole anzeigen:</span><span class="sxs-lookup"><span data-stu-id="9207c-222">You can view a complete list of the available icons here:</span></span>

<span data-ttu-id="9207c-223">http://fontawesome.io/icons/</span><span class="sxs-lookup"><span data-stu-id="9207c-223">http://fontawesome.io/icons/</span></span>

## <a name="summary"></a><span data-ttu-id="9207c-224">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="9207c-224">Summary</span></span>

<span data-ttu-id="9207c-225">"Demand" moderner Webanwendungen zunehmend reaktionsfähig, flüssigen Entwürfe, bei die bereinigen, intuitive und benutzerfreundliche aus einer Vielzahl von Geräten sind.</span><span class="sxs-lookup"><span data-stu-id="9207c-225">Modern web applications increasingly demand responsive, fluid designs that are clean, intuitive, and easy to use from a variety of devices.</span></span> <span data-ttu-id="9207c-226">Verwalten von der Komplexität von der CSS-Stylesheets erforderlich, um diese Ziele zu erreichen ist am besten weniger mithilfe der Präprozessor Like oder Sass geschehen.</span><span class="sxs-lookup"><span data-stu-id="9207c-226">Managing the complexity of the CSS stylesheets required to achieve these goals is best done using a preprocessor like Less or Sass.</span></span> <span data-ttu-id="9207c-227">Darüber hinaus Toolkits wie Schriftart Awesome schnell bereitzustellen Well-known Text Navigationsmenüs Symbole und Schaltflächen, Verbessern des allgemeinen Benutzers Ihrer Anwendung auftreten.</span><span class="sxs-lookup"><span data-stu-id="9207c-227">In addition, toolkits like Font Awesome quickly provide well-known icons to textual navigation menus and buttons, improving the overall user experience of your application.</span></span>
