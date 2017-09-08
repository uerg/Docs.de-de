---
title: Verwenden von Gulp in ASP.NET Core
author: rick-anderson
description: Informationen Sie zum Verwenden von Gulp in ASP.NET Core.
keywords: ASP.NET Core Gulp
ms.author: riande
manager: wpickett
ms.date: 02/28/2017
ms.topic: article
ms.assetid: 4095d273-bf3f-46cf-bdcc-18cf6815cbad
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/using-gulp
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 05ea4d5f0a0be08cbbdd114320d3544aae054dd2
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
# <a name="introduction-to-using-gulp-in-aspnet-core"></a><span data-ttu-id="041df-104">Einführung in das Verwenden von Gulp in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="041df-104">Introduction to using Gulp in ASP.NET Core</span></span> 

<span data-ttu-id="041df-105">Durch [Erik Reitan](https://github.com/Erikre), [Scott Addie](https://scottaddie.com), [Daniel Roth](https://github.com/danroth27), und [Shayne Boyer](https://twitter.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="041df-105">By [Erik Reitan](https://github.com/Erikre), [Scott Addie](https://scottaddie.com), [Daniel Roth](https://github.com/danroth27), and [Shayne Boyer](https://twitter.com/spboyer)</span></span>

<span data-ttu-id="041df-106">In einer typischen modernen Webanwendung kann während des Erstellungsprozesses:</span><span class="sxs-lookup"><span data-stu-id="041df-106">In a typical modern web application, the build process might:</span></span>

* <span data-ttu-id="041df-107">Bündeln und verkleinernde JavaScript und CSS-Dateien.</span><span class="sxs-lookup"><span data-stu-id="041df-107">Bundle and minify JavaScript and CSS files.</span></span>
* <span data-ttu-id="041df-108">Führen Sie Tools zum Aufrufen der Bündelung und Minimierung Aufgaben vor jedem Build aus.</span><span class="sxs-lookup"><span data-stu-id="041df-108">Run tools to call the bundling and minification tasks before each build.</span></span>
* <span data-ttu-id="041df-109">Kompilieren Sie weniger oder SASS Dateien CSS.</span><span class="sxs-lookup"><span data-stu-id="041df-109">Compile LESS or SASS files to CSS.</span></span>
* <span data-ttu-id="041df-110">Kompilieren Sie CoffeeScript oder TypeScript-Dateien für JavaScript.</span><span class="sxs-lookup"><span data-stu-id="041df-110">Compile CoffeeScript or TypeScript files to JavaScript.</span></span>

<span data-ttu-id="041df-111">Ein *Task Runner* ist ein Tool, das diese Routine Entwicklungsaufgaben und vieles mehr automatisiert.</span><span class="sxs-lookup"><span data-stu-id="041df-111">A *task runner* is a tool which automates these routine development tasks and more.</span></span> <span data-ttu-id="041df-112">Visual Studio bietet integrierte Unterstützung für zwei gängige JavaScript-basierten Task Runner: [Gulp](http://gulpjs.com) und [Grunt](using-grunt.md).</span><span class="sxs-lookup"><span data-stu-id="041df-112">Visual Studio provides built-in support for two popular JavaScript-based task runners: [Gulp](http://gulpjs.com) and [Grunt](using-grunt.md).</span></span>

## <a name="gulp"></a><span data-ttu-id="041df-113">Gulp</span><span class="sxs-lookup"><span data-stu-id="041df-113">Gulp</span></span>

<span data-ttu-id="041df-114">Gulp ist ein JavaScript-basierten streaming Build Toolkit für clientseitigen Code.</span><span class="sxs-lookup"><span data-stu-id="041df-114">Gulp is a JavaScript-based streaming build toolkit for client-side code.</span></span> <span data-ttu-id="041df-115">Er wird häufig verwendet, um die clientseitige Dateien über eine Reihe von Prozessen zu streamen, wenn ein bestimmtes Ereignis in einer Buildumgebung ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="041df-115">It is commonly used to stream client-side files through a series of processes when a specific event is triggered in a build environment.</span></span> <span data-ttu-id="041df-116">Z. B. Gulp dienen zum Automatisieren [Bündelung und Minimierung](bundling-and-minification.md) oder die Bereinigung einer Entwicklungsumgebung, bevor Sie einen neuen Build.</span><span class="sxs-lookup"><span data-stu-id="041df-116">For instance, Gulp can be used to automate [bundling and minification](bundling-and-minification.md) or the cleansing of a development environment before a new build.</span></span>

<span data-ttu-id="041df-117">Eine Reihe von Aufgaben Gulp gemäß *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="041df-117">A set of Gulp tasks is defined in *gulpfile.js*.</span></span> <span data-ttu-id="041df-118">Die folgenden JavaScript umfasst Gulp Module und gibt die Dateipfade in die bevorstehenden Aufgaben verwiesen werden:</span><span class="sxs-lookup"><span data-stu-id="041df-118">The following JavaScript includes Gulp modules and specifies file paths to be referenced within the forthcoming tasks:</span></span>

```javascript
/// <binding Clean='clean' />
"use strict";

var gulp = require("gulp"),
  rimraf = require("rimraf"),
  concat = require("gulp-concat"),
  cssmin = require("gulp-cssmin"),
  uglify = require("gulp-uglify");

var paths = {
  webroot: "./wwwroot/"
};

paths.js = paths.webroot + "js/**/*.js";
paths.minJs = paths.webroot + "js/**/*.min.js";
paths.css = paths.webroot + "css/**/*.css";
paths.minCss = paths.webroot + "css/**/*.min.css";
paths.concatJsDest = paths.webroot + "js/site.min.js";
paths.concatCssDest = paths.webroot + "css/site.min.css";
```

<span data-ttu-id="041df-119">Der obige Code gibt an, welche Knoten Module erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="041df-119">The above code specifies which Node modules are required.</span></span> <span data-ttu-id="041df-120">Die `require` Funktion jedes Modul importiert, sodass ihre Features der abhängigen Aufgaben genutzt werden können.</span><span class="sxs-lookup"><span data-stu-id="041df-120">The `require` function imports each module so that the dependent tasks can utilize their features.</span></span> <span data-ttu-id="041df-121">Aller importierten Module wird einer Variablen zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="041df-121">Each of the imported modules is assigned to a variable.</span></span> <span data-ttu-id="041df-122">Die Module können entweder nach Name oder Pfad befinden.</span><span class="sxs-lookup"><span data-stu-id="041df-122">The modules can be located either by name or path.</span></span> <span data-ttu-id="041df-123">In diesem Beispiel wird die Module mit dem Namen `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, und `gulp-uglify` anhand des Namens abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="041df-123">In this example, the modules named `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, and `gulp-uglify` are retrieved by name.</span></span> <span data-ttu-id="041df-124">Darüber hinaus eine Reihe von Pfaden werden erstellt, damit die Speicherorte der CSS- und JavaScript-Dateien verwiesen wird, die Aufgaben innerhalb und wiederverwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="041df-124">Additionally, a series of paths are created so that the locations of CSS and JavaScript files can be reused and referenced within the tasks.</span></span> <span data-ttu-id="041df-125">Die folgende Tabelle enthält Beschreibungen der Module, die in enthaltenen *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="041df-125">The following table provides descriptions of the modules included in *gulpfile.js*.</span></span>

|<span data-ttu-id="041df-126">Modulname</span><span class="sxs-lookup"><span data-stu-id="041df-126">Module Name</span></span>|<span data-ttu-id="041df-127">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="041df-127">Description</span></span>|
|---|---|
|<span data-ttu-id="041df-128">gulp</span><span class="sxs-lookup"><span data-stu-id="041df-128">gulp</span></span>|<span data-ttu-id="041df-129">Gulp streaming Buildsystems.</span><span class="sxs-lookup"><span data-stu-id="041df-129">The Gulp streaming build system.</span></span> <span data-ttu-id="041df-130">Weitere Informationen finden Sie unter [gulp](https://www.npmjs.com/package/gulp).</span><span class="sxs-lookup"><span data-stu-id="041df-130">For more information, see [gulp](https://www.npmjs.com/package/gulp).</span></span>|
|<span data-ttu-id="041df-131">rimraf</span><span class="sxs-lookup"><span data-stu-id="041df-131">rimraf</span></span>|<span data-ttu-id="041df-132">Ein Modul, Knoten löschen.</span><span class="sxs-lookup"><span data-stu-id="041df-132">A Node deletion module.</span></span> <span data-ttu-id="041df-133">Weitere Informationen finden Sie unter [Rimraf](https://www.npmjs.com/package/rimraf).</span><span class="sxs-lookup"><span data-stu-id="041df-133">For more information, see [rimraf](https://www.npmjs.com/package/rimraf).</span></span>|
|<span data-ttu-id="041df-134">Gulp concat</span><span class="sxs-lookup"><span data-stu-id="041df-134">gulp-concat</span></span>|<span data-ttu-id="041df-135">Ein Modul, das verkettet Dateien auf Grundlage des Betriebssystems Zeilenendemarke enthält.</span><span class="sxs-lookup"><span data-stu-id="041df-135">A module that concatenates files based on the operating system’s newline character.</span></span> <span data-ttu-id="041df-136">Weitere Informationen finden Sie unter [Gulp Concat](https://www.npmjs.com/package/gulp-concat).</span><span class="sxs-lookup"><span data-stu-id="041df-136">For more information, see [gulp-concat](https://www.npmjs.com/package/gulp-concat).</span></span>|
|<span data-ttu-id="041df-137">Gulp cssmin</span><span class="sxs-lookup"><span data-stu-id="041df-137">gulp-cssmin</span></span>|<span data-ttu-id="041df-138">Ein Modul, das CSS-Dateien verkleinert.</span><span class="sxs-lookup"><span data-stu-id="041df-138">A module that minifies CSS files.</span></span> <span data-ttu-id="041df-139">Weitere Informationen finden Sie unter [Gulp Cssmin](https://www.npmjs.com/package/gulp-cssmin).</span><span class="sxs-lookup"><span data-stu-id="041df-139">For more information, see [gulp-cssmin](https://www.npmjs.com/package/gulp-cssmin).</span></span>|
|<span data-ttu-id="041df-140">Gulp uglify</span><span class="sxs-lookup"><span data-stu-id="041df-140">gulp-uglify</span></span>|<span data-ttu-id="041df-141">Ein Modul, das verkleinert *js* Dateien.</span><span class="sxs-lookup"><span data-stu-id="041df-141">A module that minifies *.js* files.</span></span> <span data-ttu-id="041df-142">Weitere Informationen finden Sie unter [Gulp uglify](https://www.npmjs.com/package/gulp-uglify).</span><span class="sxs-lookup"><span data-stu-id="041df-142">For more information, see [gulp-uglify](https://www.npmjs.com/package/gulp-uglify).</span></span>|

<span data-ttu-id="041df-143">Nachdem die erforderlichen Module importiert wurden, können die Aufgaben angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="041df-143">Once the requisite modules are imported, the tasks can be specified.</span></span> <span data-ttu-id="041df-144">Hier sind sechs Aufgaben registriert haben, durch den folgenden Code dargestellt:</span><span class="sxs-lookup"><span data-stu-id="041df-144">Here there are six tasks registered, represented by the following code:</span></span>

```javascript
gulp.task("clean:js", function (cb) {
  rimraf(paths.concatJsDest, cb);
});

gulp.task("clean:css", function (cb) {
  rimraf(paths.concatCssDest, cb);
});

gulp.task("clean", ["clean:js", "clean:css"]);

gulp.task("min:js", function () {
  return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
    .pipe(concat(paths.concatJsDest))
    .pipe(uglify())
    .pipe(gulp.dest("."));
});

gulp.task("min:css", function () {
  return gulp.src([paths.css, "!" + paths.minCss])
    .pipe(concat(paths.concatCssDest))
    .pipe(cssmin())
    .pipe(gulp.dest("."));
});

gulp.task("min", ["min:js", "min:css"]);
```

<span data-ttu-id="041df-145">Die folgende Tabelle enthält eine Erklärung der Aufgaben, die im obigen Code angegeben:</span><span class="sxs-lookup"><span data-stu-id="041df-145">The following table provides an explanation of the tasks specified in the code above:</span></span>

|<span data-ttu-id="041df-146">Aufgabenname</span><span class="sxs-lookup"><span data-stu-id="041df-146">Task Name</span></span>|<span data-ttu-id="041df-147">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="041df-147">Description</span></span>|
|--- |--- |
|<span data-ttu-id="041df-148">Bereinigen: Js</span><span class="sxs-lookup"><span data-stu-id="041df-148">clean:js</span></span>|<span data-ttu-id="041df-149">Eine Aufgabe, die das Rimraf Knoten löschen-Modul verwendet, um die verkleinerte Version der Datei site.js zu entfernen.</span><span class="sxs-lookup"><span data-stu-id="041df-149">A task that uses the rimraf Node deletion module to remove the minified version of the site.js file.</span></span>|
|<span data-ttu-id="041df-150">Bereinigen: Css</span><span class="sxs-lookup"><span data-stu-id="041df-150">clean:css</span></span>|<span data-ttu-id="041df-151">Eine Aufgabe, die das Rimraf Knoten löschen-Modul verwendet, um die verkleinerte Version der Datei "Site.CSS" ändern zu entfernen.</span><span class="sxs-lookup"><span data-stu-id="041df-151">A task that uses the rimraf Node deletion module to remove the minified version of the site.css file.</span></span>|
|<span data-ttu-id="041df-152">Bereinigen</span><span class="sxs-lookup"><span data-stu-id="041df-152">clean</span></span>|<span data-ttu-id="041df-153">Eine Aufgabe, die Aufrufe der `clean:js` Aufgabe, gefolgt von der `clean:css` Aufgabe.</span><span class="sxs-lookup"><span data-stu-id="041df-153">A task that calls the `clean:js` task, followed by the `clean:css` task.</span></span>|
|<span data-ttu-id="041df-154">Min:js</span><span class="sxs-lookup"><span data-stu-id="041df-154">min:js</span></span>|<span data-ttu-id="041df-155">Eine Aufgabe, die verkleinert und alle JS-Dateien in den Ordner "Js" verkettet.</span><span class="sxs-lookup"><span data-stu-id="041df-155">A task that minifies and concatenates all .js files within the js folder.</span></span> <span data-ttu-id="041df-156">Die. min.js Dateien ausgeschlossen sind.</span><span class="sxs-lookup"><span data-stu-id="041df-156">The .min.js files are excluded.</span></span>|
|<span data-ttu-id="041df-157">Min:CSS</span><span class="sxs-lookup"><span data-stu-id="041df-157">min:css</span></span>|<span data-ttu-id="041df-158">Eine Aufgabe, die verkleinert und alle CSS-Dateien in der Css-Ordner verkettet.</span><span class="sxs-lookup"><span data-stu-id="041df-158">A task that minifies and concatenates all .css files within the css folder.</span></span> <span data-ttu-id="041df-159">Die. min.css Dateien ausgeschlossen sind.</span><span class="sxs-lookup"><span data-stu-id="041df-159">The .min.css files are excluded.</span></span>|
|<span data-ttu-id="041df-160">Min.</span><span class="sxs-lookup"><span data-stu-id="041df-160">min</span></span>|<span data-ttu-id="041df-161">Eine Aufgabe, die Aufrufe der `min:js` Aufgabe, gefolgt von der `min:css` Aufgabe.</span><span class="sxs-lookup"><span data-stu-id="041df-161">A task that calls the `min:js` task, followed by the `min:css` task.</span></span>|

## <a name="running-default-tasks"></a><span data-ttu-id="041df-162">Ausführen von Standardtasks</span><span class="sxs-lookup"><span data-stu-id="041df-162">Running default tasks</span></span>

<span data-ttu-id="041df-163">Wenn Sie eine neue Web-app bereits erstellt haben, erstellen Sie ein neues ASP.NET Web Application-Projekt in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="041df-163">If you haven’t already created a new Web app, create a new ASP.NET Web Application project in Visual Studio.</span></span>

1.  <span data-ttu-id="041df-164">Das Projekt eine neue JavaScript-Datei hinzu, und nennen Sie sie *gulpfile.js*, kopieren Sie den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="041df-164">Add a new JavaScript file to your project and name it *gulpfile.js*, then copy the following code.</span></span>

    ```javascript
    /// <binding Clean='clean' />
    "use strict";
    
    var gulp = require("gulp"),
      rimraf = require("rimraf"),
      concat = require("gulp-concat"),
      cssmin = require("gulp-cssmin"),
      uglify = require("gulp-uglify");
    
    var paths = {
      webroot: "./wwwroot/"
    };
    
    paths.js = paths.webroot + "js/**/*.js";
    paths.minJs = paths.webroot + "js/**/*.min.js";
    paths.css = paths.webroot + "css/**/*.css";
    paths.minCss = paths.webroot + "css/**/*.min.css";
    paths.concatJsDest = paths.webroot + "js/site.min.js";
    paths.concatCssDest = paths.webroot + "css/site.min.css";
    
    gulp.task("clean:js", function (cb) {
      rimraf(paths.concatJsDest, cb);
    });
    
    gulp.task("clean:css", function (cb) {
      rimraf(paths.concatCssDest, cb);
    });
    
    gulp.task("clean", ["clean:js", "clean:css"]);
    
    gulp.task("min:js", function () {
      return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
        .pipe(concat(paths.concatJsDest))
        .pipe(uglify())
        .pipe(gulp.dest("."));
    });
    
    gulp.task("min:css", function () {
      return gulp.src([paths.css, "!" + paths.minCss])
        .pipe(concat(paths.concatCssDest))
        .pipe(cssmin())
        .pipe(gulp.dest("."));
    });
    
    gulp.task("min", ["min:js", "min:css"]);
    ```

2.  <span data-ttu-id="041df-165">Öffnen der *"Package.JSON"* Datei (Hinzufügen Wenn nicht vorhanden), und fügen Sie Folgendes hinzu.</span><span class="sxs-lookup"><span data-stu-id="041df-165">Open the *package.json* file (add if not there) and add the following.</span></span>

    ```json
    {
      "devDependencies": {
        "gulp": "3.9.1",
        "gulp-concat": "2.6.1",
        "gulp-cssmin": "0.1.7",
        "gulp-uglify": "2.0.1",
        "rimraf": "2.6.1"
      }
    }
    ```

3.  <span data-ttu-id="041df-166">In **Projektmappen-Explorer**, mit der rechten Maustaste *gulpfile.js*, und wählen Sie **Taskausführungs-Explorer**.</span><span class="sxs-lookup"><span data-stu-id="041df-166">In **Solution Explorer**, right-click *gulpfile.js*, and select **Task Runner Explorer**.</span></span>
    
    ![Öffnen Sie im Projektmappen-Explorer Taskausführungs-Explorer](using-gulp/_static/02-SolutionExplorer-TaskRunnerExplorer.png)
    
    <span data-ttu-id="041df-168">**Task Runner-Explorer** enthält die Liste der Gulp Aufgaben.</span><span class="sxs-lookup"><span data-stu-id="041df-168">**Task Runner Explorer** shows the list of Gulp tasks.</span></span> <span data-ttu-id="041df-169">(Möglicherweise müssen Sie auf die **aktualisieren** , auf der linken Seite des Projektnamens erscheint.)</span><span class="sxs-lookup"><span data-stu-id="041df-169">(You might have to click the **Refresh** button that appears to the left of the project name.)</span></span>
    
    ![Taskausführungs-Explorer](using-gulp/_static/03-TaskRunnerExplorer.png)

4.  <span data-ttu-id="041df-171">Underneath **Aufgaben** in **Taskausführungs-Explorer**, mit der rechten Maustaste **Bereinigen**, und wählen Sie **ausführen** aus dem Popupmenü.</span><span class="sxs-lookup"><span data-stu-id="041df-171">Underneath **Tasks** in **Task Runner Explorer**, right-click **clean**, and select **Run** from the pop-up menu.</span></span>

    ![Task Runner-Explorer clean-Aufgabe](using-gulp/_static/04-TaskRunner-clean.png)

    <span data-ttu-id="041df-173">**Task Runner-Explorer** erstellt eine neue Registerkarte mit dem Namen **Bereinigen** , und führen Sie die clean-Aufgabe im definierten *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="041df-173">**Task Runner Explorer** will create a new tab named **clean** and execute the clean task as it is defined in *gulpfile.js*.</span></span>

5.  <span data-ttu-id="041df-174">Mit der rechten Maustaste die **Bereinigen** Aufgabe aus, und wählen Sie dann **Bindungen** > **vor dem Build**.</span><span class="sxs-lookup"><span data-stu-id="041df-174">Right-click the **clean** task, then select **Bindings** > **Before Build**.</span></span>

    ![Binden von BeforeBuild Taskausführungs-Explorer](using-gulp/_static/05-TaskRunner-BeforeBuild.png)

    <span data-ttu-id="041df-176">Die **vor dem Build** Bindung konfiguriert die clean-Aufgabe vor jedem Build des Projekts automatisch ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="041df-176">The **Before Build** binding configures the clean task to run automatically before each build of the project.</span></span>

<span data-ttu-id="041df-177">Die Bindungen Sie einrichten, mit **Taskausführungs-Explorer** befinden sich in Form eines Kommentars am oberen Rand der *gulpfile.js* und treten nur in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="041df-177">The bindings you set up with **Task Runner Explorer** are stored in the form of a comment at the top of your *gulpfile.js* and are effective only in Visual Studio.</span></span> <span data-ttu-id="041df-178">Eine Alternative, die keine Visual Studio erforderlich ist, so konfigurieren Sie die automatische Ausführung von Gulp Aufgaben in Ihrer *csproj* Datei.</span><span class="sxs-lookup"><span data-stu-id="041df-178">An alternative that doesn't require Visual Studio is to configure automatic execution of gulp tasks in your *.csproj* file.</span></span> <span data-ttu-id="041df-179">Speichern Sie diese Angabe z. B. Ihrem *csproj* Datei:</span><span class="sxs-lookup"><span data-stu-id="041df-179">For example, put this in your *.csproj* file:</span></span>

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp clean" />
</Target>
```

<span data-ttu-id="041df-180">Nachdem das clean-Aufgabe ausgeführt wird, wenn Sie das Projekt in Visual Studio oder über eine Eingabeaufforderung Ausführen der `dotnet run` Befehl (führen Sie `npm install` erste).</span><span class="sxs-lookup"><span data-stu-id="041df-180">Now the clean task is executed when you run the project in Visual Studio or from a command prompt using the `dotnet run` command (run `npm install` first).</span></span>

## <a name="defining-and-running-a-new-task"></a><span data-ttu-id="041df-181">Definieren und Ausführen eines neuen Tasks</span><span class="sxs-lookup"><span data-stu-id="041df-181">Defining and running a new task</span></span>

<span data-ttu-id="041df-182">Ändern Sie zum Definieren eines neuen Gulp Tasks *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="041df-182">To define a new Gulp task, modify *gulpfile.js*.</span></span>

1.  <span data-ttu-id="041df-183">Fügen Sie den folgenden JavaScript-Code am Ende *gulpfile.js*:</span><span class="sxs-lookup"><span data-stu-id="041df-183">Add the following JavaScript to the end of *gulpfile.js*:</span></span>

    ```javascript
    gulp.task("first", function () {
      console.log('first task! <-----');
    });
    ```

    <span data-ttu-id="041df-184">Diese Aufgabe ist mit dem Namen `first`, und es zeigt einfach eine Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="041df-184">This task is named `first`, and it simply displays a string.</span></span>

2.  <span data-ttu-id="041df-185">Speichern Sie *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="041df-185">Save *gulpfile.js*.</span></span>

3.  <span data-ttu-id="041df-186">In **Projektmappen-Explorer**, mit der rechten Maustaste *gulpfile.js*, und wählen Sie *Taskausführungs-Explorer*.</span><span class="sxs-lookup"><span data-stu-id="041df-186">In **Solution Explorer**, right-click *gulpfile.js*, and select *Task Runner Explorer*.</span></span>

4.  <span data-ttu-id="041df-187">In **Taskausführungs-Explorer**, mit der rechten Maustaste **erste**, und wählen Sie **ausführen**.</span><span class="sxs-lookup"><span data-stu-id="041df-187">In **Task Runner Explorer**, right-click **first**, and select **Run**.</span></span>

    ![Führen Sie die erste Aufgabe Taskausführungs-Explorer](using-gulp/_static/06-TaskRunner-First.png)

    <span data-ttu-id="041df-189">Sie sehen, dass der Ausgabetext angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="041df-189">You’ll see that the output text is displayed.</span></span> <span data-ttu-id="041df-190">Wenn Sie Beispiele, die basierend auf ein häufiges Szenario interessiert sind, finden Sie unter Gulp Rezepte.</span><span class="sxs-lookup"><span data-stu-id="041df-190">If you are interested in examples based on a common scenario, see Gulp Recipes.</span></span>

## <a name="defining-and-running-tasks-in-a-series"></a><span data-ttu-id="041df-191">Definieren und Ausführen von Aufgaben in einer Reihe</span><span class="sxs-lookup"><span data-stu-id="041df-191">Defining and running tasks in a series</span></span>

<span data-ttu-id="041df-192">Wenn Sie mehrere Aufgaben ausführen, werden die Aufgaben gleichzeitig standardmäßig ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="041df-192">When you run multiple tasks, the tasks run concurrently by default.</span></span> <span data-ttu-id="041df-193">Jedoch wenn Sie Aufgaben in einer bestimmten Reihenfolge ausführen müssen, geben Sie bei jeder Aufgabe abgeschlossen ist, auch ist als die Aufgaben auf den Abschluss einer anderen Aufgabe abhängen.</span><span class="sxs-lookup"><span data-stu-id="041df-193">However, if you need to run tasks in a specific order, you must specify when each task is complete, as well as which tasks depend on the completion of another task.</span></span>

1.  <span data-ttu-id="041df-194">Definieren Sie eine Reihe von Aufgaben, die in der Reihenfolge ausgeführt, ersetzen die `first` Aufgabe, die Sie oben im hinzugefügten *gulpfile.js* durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="041df-194">To define a series of tasks to run in order, replace the `first` task that you added above in *gulpfile.js* with the following:</span></span>

    ```javascript
    gulp.task("series:first", function () {
      console.log('first task! <-----');
    });
 
    gulp.task("series:second", ["series:first"], function () {
      console.log('second task! <-----');
    });
 
    gulp.task("series", ["series:first", "series:second"], function () {});
    ```
 
    <span data-ttu-id="041df-195">Sie verfügen jetzt über drei Aufgaben: `series:first`, `series:second`, und `series`.</span><span class="sxs-lookup"><span data-stu-id="041df-195">You now have three tasks: `series:first`, `series:second`, and `series`.</span></span> <span data-ttu-id="041df-196">Die `series:second` Task "" enthält einen zweiten Parameter ein Array von Aufgaben ausgeführt und abgeschlossen gibt, bevor die `series:second` Task ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="041df-196">The `series:second` task includes a second parameter which specifies an array of tasks to be run and completed before the `series:second` task will run.</span></span>  <span data-ttu-id="041df-197">Nach den Angaben im Code oben, nur die `series:first` Aufgabe muss abgeschlossen sein, bevor die `series:second` Task ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="041df-197">As specified in the code above, only the `series:first` task must be completed before the `series:second` task will run.</span></span>

2.  <span data-ttu-id="041df-198">Speichern Sie *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="041df-198">Save *gulpfile.js*.</span></span>

3.  <span data-ttu-id="041df-199">In **Projektmappen-Explorer**, mit der rechten Maustaste *gulpfile.js* , und wählen Sie **Taskausführungs-Explorer** , wenn er nicht bereits geöffnet ist.</span><span class="sxs-lookup"><span data-stu-id="041df-199">In **Solution Explorer**, right-click *gulpfile.js* and select **Task Runner Explorer** if it isn’t already open.</span></span>

4.  <span data-ttu-id="041df-200">In **Taskausführungs-Explorer**, mit der rechten Maustaste **Reihe** , und wählen Sie **ausführen**.</span><span class="sxs-lookup"><span data-stu-id="041df-200">In **Task Runner Explorer**, right-click **series** and select **Run**.</span></span>

    ![Ausführen der Reihe Taskausführungs-Explorer](using-gulp/_static/07-TaskRunner-Series.png)

## <a name="intellisense"></a><span data-ttu-id="041df-202">IntelliSense</span><span class="sxs-lookup"><span data-stu-id="041df-202">IntelliSense</span></span>

<span data-ttu-id="041df-203">IntelliSense bietet codevervollständigung, parameterbeschreibungen und andere Funktionen, um die Produktivität zu steigern und Fehler reduzieren.</span><span class="sxs-lookup"><span data-stu-id="041df-203">IntelliSense provides code completion, parameter descriptions, and other features to boost productivity and to decrease errors.</span></span> <span data-ttu-id="041df-204">Gulp Aufgaben werden in JavaScript geschrieben. aus diesem Grund kann IntelliSense, um Unterstützung zu erhalten, während der Entwicklung bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="041df-204">Gulp tasks are written in JavaScript; therefore, IntelliSense can provide assistance while developing.</span></span> <span data-ttu-id="041df-205">Beim Arbeiten mit JavaScript, listet IntelliSense die Objekte, Funktionen, Eigenschaften und, die zur Verfügung stehenden Parametern basierend auf den aktuellen Kontext.</span><span class="sxs-lookup"><span data-stu-id="041df-205">As you work with JavaScript, IntelliSense lists the objects, functions, properties, and parameters that are available based on your current context.</span></span> <span data-ttu-id="041df-206">Wählen Sie eine Codierungsoption aus der Popupliste aus, die von IntelliSense zur Fertigstellung des Codes bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="041df-206">Select a coding option from the pop-up list provided by IntelliSense to complete the code.</span></span>

![gulp IntelliSense](using-gulp/_static/08-IntelliSense.png)

<span data-ttu-id="041df-208">Weitere Informationen zu IntelliSense finden Sie unter [JavaScript IntelliSense](https://msdn.microsoft.com/library/bb385682).</span><span class="sxs-lookup"><span data-stu-id="041df-208">For more information about IntelliSense, see [JavaScript IntelliSense](https://msdn.microsoft.com/library/bb385682).</span></span>

## <a name="development-staging-and-production-environments"></a><span data-ttu-id="041df-209">Entwicklungs-, Staging-und produktionsumgebungen</span><span class="sxs-lookup"><span data-stu-id="041df-209">Development, staging, and production environments</span></span>

<span data-ttu-id="041df-210">Wenn Gulp verwendet wird, um die clientseitige-Dateien für das Staging und Produktion zu optimieren, werden die verarbeiteten Dateien in einen lokalen Speicherort für Staging und Produktion gespeichert.</span><span class="sxs-lookup"><span data-stu-id="041df-210">When Gulp is used to optimize client-side files for staging and production, the processed files are saved to a local staging and production location.</span></span> <span data-ttu-id="041df-211">Die *_Layout.cshtml* Datei verwendet das **Umgebung** Hilfsprogramm zum Bereitstellen zweier verschiedener Versionen der CSS-Dateien zu kennzeichnen.</span><span class="sxs-lookup"><span data-stu-id="041df-211">The *_Layout.cshtml* file uses the **environment** tag helper to provide two different versions of CSS files.</span></span> <span data-ttu-id="041df-212">Eine Version von CSS-Dateien handelt, für die Entwicklung und die andere Version für das Staging und Produktion optimiert ist.</span><span class="sxs-lookup"><span data-stu-id="041df-212">One version of CSS files is for development and the other version is optimized for both staging and production.</span></span> <span data-ttu-id="041df-213">In Visual Studio-2017, wenn Sie ändern die **ASPNETCORE_ENVIRONMENT** Umgebungsvariablen `Production`, Visual Studio wird die Web-app und einen Link zu den minimierten CSS-Dateien erstellen.</span><span class="sxs-lookup"><span data-stu-id="041df-213">In Visual Studio 2017, when you change the **ASPNETCORE_ENVIRONMENT** environment variable to `Production`, Visual Studio will build the Web app and link to the minimized CSS files.</span></span> <span data-ttu-id="041df-214">Das folgende Markup zeigt die **Umgebung** Hilfsprogramme mit Linktags zum Kennzeichnen der `Development` CSS-Dateien und die verkleinerte `Staging, Production` CSS-Dateien.</span><span class="sxs-lookup"><span data-stu-id="041df-214">The following markup shows the **environment** tag helpers containing link tags to the `Development` CSS files and the minified `Staging, Production` CSS files.</span></span>

```html
<environment names="Development">
    <script src="~/lib/jquery/dist/jquery.js"></script>
    <script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
    <script src="~/js/site.js" asp-append-version="true"></script>
</environment>
<environment names="Staging,Production">
    <script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.2.0.min.js"
            asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
            asp-fallback-test="window.jQuery"
            crossorigin="anonymous"
            integrity="sha384-K+ctZQ+LL8q6tP7I94W+qzQsfRV2a+AfHIi9k8z8l9ggpc8X+Ytst4yBo/hH+8Fk">
    </script>
    <script src="https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js"
            asp-fallback-src="~/lib/bootstrap/dist/js/bootstrap.min.js"
            asp-fallback-test="window.jQuery && window.jQuery.fn && window.jQuery.fn.modal"
            crossorigin="anonymous"
            integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa">
    </script>
    <script src="~/js/site.min.js" asp-append-version="true"></script>
</environment>
```

## <a name="switching-between-environments"></a><span data-ttu-id="041df-215">Wechseln zwischen Umgebungen</span><span class="sxs-lookup"><span data-stu-id="041df-215">Switching between environments</span></span>

<span data-ttu-id="041df-216">Um zwischen dem Kompilieren von für unterschiedliche Umgebungen zu wechseln, Ändern der **ASPNETCORE_ENVIRONMENT** Umgebung des Variablenwerts.</span><span class="sxs-lookup"><span data-stu-id="041df-216">To switch between compiling for different environments, modify the **ASPNETCORE_ENVIRONMENT** environment variable's value.</span></span>

1.  <span data-ttu-id="041df-217">In **Taskausführungs-Explorer**, überprüfen Sie, ob die **min** Task auszuführende vorsieht **vor dem Build**.</span><span class="sxs-lookup"><span data-stu-id="041df-217">In **Task Runner Explorer**, verify that the **min** task has been set to run **Before Build**.</span></span>

2.  <span data-ttu-id="041df-218">In **Projektmappen-Explorer**mit der rechten Maustaste auf den Projektnamen, und wählen Sie **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="041df-218">In **Solution Explorer**, right-click the project name and select **Properties**.</span></span>

    <span data-ttu-id="041df-219">Die Eigenschaftenseite für die Web-app wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="041df-219">The property sheet for the Web app is displayed.</span></span>

3.  <span data-ttu-id="041df-220">Klicken Sie auf die **Debuggen** Registerkarte.</span><span class="sxs-lookup"><span data-stu-id="041df-220">Click the **Debug** tab.</span></span>

4.  <span data-ttu-id="041df-221">Legen Sie den Wert, der die **Hosting-Umgebung:** Umgebungsvariablen `Production`.</span><span class="sxs-lookup"><span data-stu-id="041df-221">Set the value of the **Hosting:Environment** environment variable to `Production`.</span></span>

5.  <span data-ttu-id="041df-222">Drücken Sie **F5** zum Ausführen der Anwendung in einem Browser.</span><span class="sxs-lookup"><span data-stu-id="041df-222">Press **F5** to run the application in a browser.</span></span>

6.  <span data-ttu-id="041df-223">Klicken Sie im Browserfenster mit der rechten Maustaste in der Seite, und wählen **Quelltext anzeigen** um die HTML für die Seite anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="041df-223">In the browser window, right-click the page and select **View Source** to view the HTML for the page.</span></span>

    <span data-ttu-id="041df-224">Beachten Sie, dass das Stylesheet Links auf die verkleinerte CSS-Dateien verweisen.</span><span class="sxs-lookup"><span data-stu-id="041df-224">Notice that the stylesheet links point to the minified CSS files.</span></span>

7.  <span data-ttu-id="041df-225">Schließen Sie den Browser, um der Web-app zu beenden.</span><span class="sxs-lookup"><span data-stu-id="041df-225">Close the browser to stop the Web app.</span></span>

8.  <span data-ttu-id="041df-226">Klicken Sie in Visual Studio zurück, um das Eigenschaftenfenster für die Web-app, und Ändern der **Hosting-Umgebung:** Umgebungsvariable zurück zum `Development`.</span><span class="sxs-lookup"><span data-stu-id="041df-226">In Visual Studio, return to the property sheet for the Web app and change the **Hosting:Environment** environment variable back to `Development`.</span></span>

9.  <span data-ttu-id="041df-227">Drücken Sie **F5** erneut ausführen, die Anwendung in einem Browser.</span><span class="sxs-lookup"><span data-stu-id="041df-227">Press **F5** to run the application in a browser again.</span></span>

10. <span data-ttu-id="041df-228">Klicken Sie im Browserfenster mit der rechten Maustaste in der Seite, und wählen **Quelltext anzeigen** um den HTML-Code für die Seite anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="041df-228">In the browser window, right-click the page and select **View Source** to see the HTML for the page.</span></span>

    <span data-ttu-id="041df-229">Beachten Sie, dass das Stylesheet Links auf die unminified Versionen der CSS-Dateien verweisen.</span><span class="sxs-lookup"><span data-stu-id="041df-229">Notice that the stylesheet links point to the unminified versions of the CSS files.</span></span>

<span data-ttu-id="041df-230">Weitere Informationen zu Umgebungen in ASP.NET Core, finden Sie unter [arbeiten mit mehreren Umgebungen](../fundamentals/environments.md).</span><span class="sxs-lookup"><span data-stu-id="041df-230">For more information related to environments in ASP.NET Core, see [Working with Multiple Environments](../fundamentals/environments.md).</span></span>

## <a name="task-and-module-details"></a><span data-ttu-id="041df-231">Aufgabe und die Modul-details</span><span class="sxs-lookup"><span data-stu-id="041df-231">Task and module details</span></span>

<span data-ttu-id="041df-232">Name einer Funktion ist eine Gulp Aufgabe registriert.</span><span class="sxs-lookup"><span data-stu-id="041df-232">A Gulp task is registered with a function name.</span></span>  <span data-ttu-id="041df-233">Sie können die Abhängigkeiten angeben, wenn andere Aufgaben vor der aktuellen Aufgabe ausgeführt werden müssen.</span><span class="sxs-lookup"><span data-stu-id="041df-233">You can specify dependencies if other tasks must run before the current task.</span></span> <span data-ttu-id="041df-234">Zusätzliche Funktionen ermöglichen es Ihnen, ausführen und überwachen die Gulp Aufgaben, sowie legen Sie die Quelle (*Src*) und das Ziel (*Dest*) der Dateien, die geändert wird.</span><span class="sxs-lookup"><span data-stu-id="041df-234">Additional functions allow you to run and watch the Gulp tasks, as well as set the source (*src*) and destination (*dest*) of the files being modified.</span></span> <span data-ttu-id="041df-235">Im folgenden sind die primären Gulp-API-Funktionen:</span><span class="sxs-lookup"><span data-stu-id="041df-235">The following are the primary Gulp API functions:</span></span>

|<span data-ttu-id="041df-236">Gulp-Funktion</span><span class="sxs-lookup"><span data-stu-id="041df-236">Gulp Function</span></span>|<span data-ttu-id="041df-237">Syntax</span><span class="sxs-lookup"><span data-stu-id="041df-237">Syntax</span></span>|<span data-ttu-id="041df-238">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="041df-238">Description</span></span>|
|---   |--- |--- |
|<span data-ttu-id="041df-239">Aufgabe</span><span class="sxs-lookup"><span data-stu-id="041df-239">task</span></span>  |`gulp.task(name[, deps], fn) { }`|<span data-ttu-id="041df-240">Die `task` Funktion erstellt eine Aufgabe.</span><span class="sxs-lookup"><span data-stu-id="041df-240">The `task` function creates a task.</span></span> <span data-ttu-id="041df-241">Die `name` Parameter definiert den Namen des Tasks.</span><span class="sxs-lookup"><span data-stu-id="041df-241">The `name` parameter defines the name of the task.</span></span> <span data-ttu-id="041df-242">Die `deps` Parameter enthält ein Array von Aufgaben abgeschlossen werden, bevor diese Aufgabe ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="041df-242">The `deps` parameter contains an array of tasks to be completed before this task runs.</span></span> <span data-ttu-id="041df-243">Die `fn` Parameter darstellt, eine Rückruffunktion, die die Vorgänge des Tasks führt.</span><span class="sxs-lookup"><span data-stu-id="041df-243">The `fn` parameter represents a callback function which performs the operations of the task.</span></span>|
|<span data-ttu-id="041df-244">Überwachungsfenster</span><span class="sxs-lookup"><span data-stu-id="041df-244">watch</span></span> |`gulp.watch(glob [, opts], tasks) { }`|<span data-ttu-id="041df-245">Die `watch` Funktion überwacht Dateien und führt Aufgaben aus, wenn eine Datei geändert wird.</span><span class="sxs-lookup"><span data-stu-id="041df-245">The `watch` function monitors files and runs tasks when a file change occurs.</span></span> <span data-ttu-id="041df-246">Die `glob` Parameter ist ein `string` oder `array` , der bestimmt, welche Dateien überwachen.</span><span class="sxs-lookup"><span data-stu-id="041df-246">The `glob` parameter is a `string` or `array` that determines which files to watch.</span></span> <span data-ttu-id="041df-247">Die `opts` Parameter stellt zusätzliche Datei beobachten von Optionen zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="041df-247">The `opts` parameter provides additional file watching options.</span></span>|
|<span data-ttu-id="041df-248">src</span><span class="sxs-lookup"><span data-stu-id="041df-248">src</span></span>   |`gulp.src(globs[, options]) { }`|<span data-ttu-id="041df-249">Die `src` Funktion enthält Dateien, die die Glob Werten entsprechen.</span><span class="sxs-lookup"><span data-stu-id="041df-249">The `src` function provides files that match the glob value(s).</span></span> <span data-ttu-id="041df-250">Die `glob` Parameter ist ein `string` oder `array` , der bestimmt, welche Dateien um zu lesen.</span><span class="sxs-lookup"><span data-stu-id="041df-250">The `glob` parameter is a `string` or `array` that determines which files to read.</span></span> <span data-ttu-id="041df-251">Die `options` Parameter bietet zusätzliche Dateioptionen.</span><span class="sxs-lookup"><span data-stu-id="041df-251">The `options` parameter provides additional file options.</span></span>|
|<span data-ttu-id="041df-252">Dest</span><span class="sxs-lookup"><span data-stu-id="041df-252">dest</span></span>  |`gulp.dest(path[, options]) { }`|<span data-ttu-id="041df-253">Die `dest` Funktion definiert einen Speicherort, auf die Dateien geschrieben werden können.</span><span class="sxs-lookup"><span data-stu-id="041df-253">The `dest` function defines a location to which files can be written.</span></span> <span data-ttu-id="041df-254">Die `path` Parameter ist eine Zeichenfolge oder eine Funktion, die den Zielordner bestimmt.</span><span class="sxs-lookup"><span data-stu-id="041df-254">The `path` parameter is a string or function that determines the destination folder.</span></span> <span data-ttu-id="041df-255">Die `options` Parameter ist ein Objekt, das Ausgabe-Ordneroptionen angibt.</span><span class="sxs-lookup"><span data-stu-id="041df-255">The `options` parameter is an object that specifies output folder options.</span></span>|

<span data-ttu-id="041df-256">Zusätzliche Gulp-API-Referenzinformationen finden Sie unter [Gulp Docs API](https://github.com/gulpjs/gulp/blob/master/docs/API.md).</span><span class="sxs-lookup"><span data-stu-id="041df-256">For additional Gulp API reference information, see [Gulp Docs API](https://github.com/gulpjs/gulp/blob/master/docs/API.md).</span></span>

## <a name="gulp-recipes"></a><span data-ttu-id="041df-257">Gulp Rezepte</span><span class="sxs-lookup"><span data-stu-id="041df-257">Gulp recipes</span></span>

<span data-ttu-id="041df-258">Die Gulp-Community bietet Gulp [Rezepte](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md).</span><span class="sxs-lookup"><span data-stu-id="041df-258">The Gulp community provides Gulp [recipes](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md).</span></span> <span data-ttu-id="041df-259">Diese Rezepte beinhalten Gulp Aufgaben zu allgemeinen Szenarien zu adressieren.</span><span class="sxs-lookup"><span data-stu-id="041df-259">These recipes consist of Gulp tasks to address common scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="041df-260">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="041df-260">Additional resources</span></span>

* [<span data-ttu-id="041df-261">Gulp-Dokumentation</span><span class="sxs-lookup"><span data-stu-id="041df-261">Gulp documentation</span></span>](https://github.com/gulpjs/gulp/blob/master/docs/README.md)
* [<span data-ttu-id="041df-262">Bundling und Minimierung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="041df-262">Bundling and minification in ASP.NET Core</span></span>](bundling-and-minification.md)
* [<span data-ttu-id="041df-263">Verwenden von Grunt in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="041df-263">Using Grunt in ASP.NET Core</span></span>](using-grunt.md)
