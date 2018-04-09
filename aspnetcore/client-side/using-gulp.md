---
title: Verwenden von Gulp in ASP.NET Core
author: rick-anderson
description: Informationen Sie zum Verwenden von Gulp in ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 02/28/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/using-gulp
ms.openlocfilehash: f776b2025b6ebfeff28d3903aaeac4d7d89665b3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="use-gulp-in-aspnet-core"></a>Verwenden von Gulp in ASP.NET Core

Durch [Erik Reitan](https://github.com/Erikre), [Scott Addie](https://scottaddie.com), [Daniel Roth](https://github.com/danroth27), und [Shayne Boyer](https://twitter.com/spboyer)

In einer typischen moderne Web-app möglicherweise während des Erstellungsprozesses:

* Bündeln und verkleinernde JavaScript und CSS-Dateien.
* Führen Sie Tools zum Aufrufen der Bündelung und Minimierung Aufgaben vor jedem Build aus.
* Kompilieren Sie weniger oder SASS Dateien CSS.
* Kompilieren Sie CoffeeScript oder TypeScript-Dateien für JavaScript.

Ein *Task Runner* ist ein Tool, das diese Routine Entwicklungsaufgaben und vieles mehr automatisiert. Visual Studio bietet integrierte Unterstützung für zwei gängige JavaScript-basierten Task Runner: [Gulp](https://gulpjs.com/) und [Grunt](using-grunt.md).

## <a name="gulp"></a>Gulp

Gulp ist ein JavaScript-basierten streaming Build Toolkit für clientseitigen Code. Er wird häufig verwendet, um die clientseitige Dateien über eine Reihe von Prozessen zu streamen, wenn ein bestimmtes Ereignis in einer Buildumgebung ausgelöst wird. Z. B. Gulp dienen zum Automatisieren [Bündelung und Minimierung](bundling-and-minification.md) oder die Bereinigung einer Entwicklungsumgebung, bevor Sie einen neuen Build.

Eine Reihe von Aufgaben Gulp gemäß *gulpfile.js*. Die folgenden JavaScript umfasst Gulp Module und gibt die Dateipfade in die bevorstehenden Aufgaben verwiesen werden:

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

Der obige Code gibt an, welche Knoten Module erforderlich sind. Die `require` Funktion jedes Modul importiert, sodass ihre Features der abhängigen Aufgaben genutzt werden können. Aller importierten Module wird einer Variablen zugewiesen. Die Module können entweder nach Name oder Pfad befinden. In diesem Beispiel wird die Module mit dem Namen `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, und `gulp-uglify` anhand des Namens abgerufen werden. Darüber hinaus eine Reihe von Pfaden werden erstellt, damit die Speicherorte der CSS- und JavaScript-Dateien verwiesen wird, die Aufgaben innerhalb und wiederverwendet werden können. Die folgende Tabelle enthält Beschreibungen der Module, die in enthaltenen *gulpfile.js*.

| Modulname | Beschreibung |
| ----------- | ----------- |
| gulp        | Gulp streaming Buildsystems. Weitere Informationen finden Sie unter [gulp](https://www.npmjs.com/package/gulp). |
| rimraf      | Ein Modul, Knoten löschen. Weitere Informationen finden Sie unter [Rimraf](https://www.npmjs.com/package/rimraf). |
| Gulp concat | Ein Modul, das verkettet Dateien auf Grundlage des Betriebssystems Zeilenendemarke enthält. Weitere Informationen finden Sie unter [Gulp Concat](https://www.npmjs.com/package/gulp-concat). |
| gulp-cssmin | Ein Modul, das CSS-Dateien verkleinert. Weitere Informationen finden Sie unter [Gulp Cssmin](https://www.npmjs.com/package/gulp-cssmin). |
| Gulp uglify | Ein Modul, das verkleinert *js* Dateien. Weitere Informationen finden Sie unter [Gulp uglify](https://www.npmjs.com/package/gulp-uglify). |

Nachdem die erforderlichen Module importiert wurden, können die Aufgaben angegeben werden. Hier sind sechs Aufgaben registriert haben, durch den folgenden Code dargestellt:

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

Die folgende Tabelle enthält eine Erklärung der Aufgaben, die im obigen Code angegeben:

|Aufgabenname|Beschreibung|
|--- |--- |
|clean:js|Eine Aufgabe, die das Rimraf Knoten löschen-Modul verwendet, um die verkleinerte Version der Datei site.js zu entfernen.|
|Bereinigen: Css|Eine Aufgabe, die das Rimraf Knoten löschen-Modul verwendet, um die verkleinerte Version der Datei "Site.CSS" ändern zu entfernen.|
|Bereinigen|Eine Aufgabe, die Aufrufe der `clean:js` Aufgabe, gefolgt von der `clean:css` Aufgabe.|
|min:js|Eine Aufgabe, die verkleinert und alle JS-Dateien in den Ordner "Js" verkettet. Die. min.js Dateien ausgeschlossen sind.|
|Min:CSS|Eine Aufgabe, die verkleinert und alle CSS-Dateien in der Css-Ordner verkettet. Die. min.css Dateien ausgeschlossen sind.|
|Min.|Eine Aufgabe, die Aufrufe der `min:js` Aufgabe, gefolgt von der `min:css` Aufgabe.|

## <a name="running-default-tasks"></a>Ausführen von Standardtasks

Wenn Sie eine neue Web-app bereits erstellt haben, erstellen Sie ein neues ASP.NET Web Application-Projekt in Visual Studio.

1.  Das Projekt eine neue JavaScript-Datei hinzu, und nennen Sie sie *gulpfile.js*, kopieren Sie den folgenden Code.

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

2.  Öffnen der *"Package.JSON"* Datei (Hinzufügen Wenn nicht vorhanden), und fügen Sie Folgendes hinzu.

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

3.  In **Projektmappen-Explorer**, mit der rechten Maustaste *gulpfile.js*, und wählen Sie **Taskausführungs-Explorer**.
    
    ![Öffnen Sie im Projektmappen-Explorer Taskausführungs-Explorer](using-gulp/_static/02-SolutionExplorer-TaskRunnerExplorer.png)
    
    **Task Runner-Explorer** enthält die Liste der Gulp Aufgaben. (Möglicherweise müssen Sie auf die **aktualisieren** , auf der linken Seite des Projektnamens erscheint.)
    
    ![Taskausführungs-Explorer](using-gulp/_static/03-TaskRunnerExplorer.png)
    
    > [!IMPORTANT]
    > Die **Taskausführungs-Explorer** Kontextmenüelement erscheint nur, wenn *gulpfile.js* befindet sich im Stammverzeichnis des Projekts.

4.  Underneath **Aufgaben** in **Taskausführungs-Explorer**, mit der rechten Maustaste **Bereinigen**, und wählen Sie **ausführen** aus dem Popupmenü.

    ![Task Runner-Explorer clean-Aufgabe](using-gulp/_static/04-TaskRunner-clean.png)

    **Task Runner-Explorer** erstellt eine neue Registerkarte mit dem Namen **Bereinigen** , und führen Sie die clean-Aufgabe im definierten *gulpfile.js*.

5.  Mit der rechten Maustaste die **Bereinigen** Aufgabe aus, und wählen Sie dann **Bindungen** > **vor dem Build**.

    ![Binden von BeforeBuild Taskausführungs-Explorer](using-gulp/_static/05-TaskRunner-BeforeBuild.png)

    Die **vor dem Build** Bindung konfiguriert die clean-Aufgabe vor jedem Build des Projekts automatisch ausgeführt.

Die Bindungen Sie einrichten, mit **Taskausführungs-Explorer** befinden sich in Form eines Kommentars am oberen Rand der *gulpfile.js* und treten nur in Visual Studio. Eine Alternative, die keine Visual Studio erforderlich ist, so konfigurieren Sie die automatische Ausführung von Gulp Aufgaben in Ihrer *csproj* Datei. Speichern Sie diese Angabe z. B. Ihrem *csproj* Datei:

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp clean" />
</Target>
```

Nachdem das clean-Aufgabe ausgeführt wird, wenn Sie das Projekt in Visual Studio oder über eine Eingabeaufforderung Ausführen der [Dotnet ausführen](/dotnet/core/tools/dotnet-run) Befehl (führen Sie `npm install` erste).

## <a name="defining-and-running-a-new-task"></a>Definieren und Ausführen eines neuen Tasks

Ändern Sie zum Definieren eines neuen Gulp Tasks *gulpfile.js*.

1.  Fügen Sie den folgenden JavaScript-Code am Ende *gulpfile.js*:

    ```javascript
    gulp.task("first", function () {
      console.log('first task! <-----');
    });
    ```

    Diese Aufgabe ist mit dem Namen `first`, und es zeigt einfach eine Zeichenfolge.

2.  Speichern Sie *gulpfile.js*.

3.  In **Projektmappen-Explorer**, mit der rechten Maustaste *gulpfile.js*, und wählen Sie *Taskausführungs-Explorer*.

4.  In **Taskausführungs-Explorer**, mit der rechten Maustaste **erste**, und wählen Sie **ausführen**.

    ![Führen Sie die erste Aufgabe Taskausführungs-Explorer](using-gulp/_static/06-TaskRunner-First.png)

    Der Ausgabetext wird angezeigt. Beispiele für häufige Szenarien Grundlage finden Sie unter [Gulp Rezepte](#gulp-recipes).

## <a name="defining-and-running-tasks-in-a-series"></a>Definieren und Ausführen von Aufgaben in einer Reihe

Wenn Sie mehrere Aufgaben ausführen, werden die Aufgaben gleichzeitig standardmäßig ausgeführt. Jedoch wenn Sie Aufgaben in einer bestimmten Reihenfolge ausführen müssen, geben Sie bei jeder Aufgabe abgeschlossen ist, auch ist als die Aufgaben auf den Abschluss einer anderen Aufgabe abhängen.

1.  Definieren Sie eine Reihe von Aufgaben, die in der Reihenfolge ausgeführt, ersetzen die `first` Aufgabe, die Sie oben im hinzugefügten *gulpfile.js* durch Folgendes:

    ```javascript
    gulp.task("series:first", function () {
      console.log('first task! <-----');
    });
 
    gulp.task("series:second", ["series:first"], function () {
      console.log('second task! <-----');
    });
 
    gulp.task("series", ["series:first", "series:second"], function () {});
    ```
 
    Sie verfügen jetzt über drei Aufgaben: `series:first`, `series:second`, und `series`. Die `series:second` Task "" enthält einen zweiten Parameter ein Array von Aufgaben ausgeführt und abgeschlossen gibt, bevor die `series:second` Task ausgeführt wird. Nach den Angaben im Code oben, nur die `series:first` Aufgabe muss abgeschlossen sein, bevor die `series:second` Task ausgeführt wird.

2.  Speichern Sie *gulpfile.js*.

3.  In **Projektmappen-Explorer**, mit der rechten Maustaste *gulpfile.js* , und wählen Sie **Taskausführungs-Explorer** , wenn er nicht bereits geöffnet ist.

4.  In **Taskausführungs-Explorer**, mit der rechten Maustaste **Reihe** , und wählen Sie **ausführen**.

    ![Ausführen der Reihe Taskausführungs-Explorer](using-gulp/_static/07-TaskRunner-Series.png)

## <a name="intellisense"></a>IntelliSense

IntelliSense bietet codevervollständigung, parameterbeschreibungen und andere Funktionen, um die Produktivität zu steigern und Fehler reduzieren. Gulp Aufgaben werden in JavaScript geschrieben. aus diesem Grund kann IntelliSense, um Unterstützung zu erhalten, während der Entwicklung bereitstellen. Beim Arbeiten mit JavaScript, listet IntelliSense die Objekte, Funktionen, Eigenschaften und, die zur Verfügung stehenden Parametern basierend auf den aktuellen Kontext. Wählen Sie eine Codierungsoption aus der Popupliste aus, die von IntelliSense zur Fertigstellung des Codes bereitgestellt.

![gulp IntelliSense](using-gulp/_static/08-IntelliSense.png)

Weitere Informationen zu IntelliSense finden Sie unter [JavaScript IntelliSense](https://docs.microsoft.com/visualstudio/ide/javascript-intellisense).

## <a name="development-staging-and-production-environments"></a>Entwicklungs-, Staging-und produktionsumgebungen

Wenn Gulp verwendet wird, um die clientseitige-Dateien für das Staging und Produktion zu optimieren, werden die verarbeiteten Dateien in einen lokalen Speicherort für Staging und Produktion gespeichert. Die *_Layout.cshtml* Datei verwendet das **Umgebung** Hilfsprogramm zum Bereitstellen zweier verschiedener Versionen der CSS-Dateien zu kennzeichnen. Eine Version von CSS-Dateien handelt, für die Entwicklung und die andere Version für das Staging und Produktion optimiert ist. In Visual Studio-2017, wenn Sie ändern die **ASPNETCORE_ENVIRONMENT** Umgebungsvariablen `Production`, Visual Studio wird die Web-app und einen Link zu den minimierten CSS-Dateien erstellen. Das folgende Markup zeigt die **Umgebung** Hilfsprogramme mit Linktags zum Kennzeichnen der `Development` CSS-Dateien und die verkleinerte `Staging, Production` CSS-Dateien.

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

## <a name="switching-between-environments"></a>Wechseln zwischen Umgebungen

Um zwischen dem Kompilieren von für unterschiedliche Umgebungen zu wechseln, Ändern der **ASPNETCORE_ENVIRONMENT** Umgebung des Variablenwerts.

1.  In **Taskausführungs-Explorer**, überprüfen Sie, ob die **min** Task auszuführende vorsieht **vor dem Build**.

2.  In **Projektmappen-Explorer**mit der rechten Maustaste auf den Projektnamen, und wählen Sie **Eigenschaften**.

    Die Eigenschaftenseite für die Web-app wird angezeigt.

3.  Klicken Sie auf die Registerkarte **Debuggen**.

4.  Legen Sie den Wert, der die **Hosting-Umgebung:** Umgebungsvariablen `Production`.

5.  Drücken Sie **F5** zum Ausführen der Anwendung in einem Browser.

6.  Klicken Sie im Browserfenster mit der rechten Maustaste in der Seite, und wählen **Quelltext anzeigen** um die HTML für die Seite anzuzeigen.

    Beachten Sie, dass das Stylesheet Links auf die verkleinerte CSS-Dateien verweisen.

7.  Schließen Sie den Browser, um der Web-app zu beenden.

8.  Klicken Sie in Visual Studio zurück, um das Eigenschaftenfenster für die Web-app, und Ändern der **Hosting-Umgebung:** Umgebungsvariable zurück zum `Development`.

9.  Drücken Sie **F5** erneut ausführen, die Anwendung in einem Browser.

10. Klicken Sie im Browserfenster mit der rechten Maustaste in der Seite, und wählen **Quelltext anzeigen** um den HTML-Code für die Seite anzuzeigen.

    Beachten Sie, dass das Stylesheet Links auf die unminified Versionen der CSS-Dateien verweisen.

Weitere Informationen zu Umgebungen in ASP.NET Core, finden Sie unter [arbeiten mit mehreren Umgebungen](../fundamentals/environments.md).

## <a name="task-and-module-details"></a>Aufgabe und die Modul-details

Name einer Funktion ist eine Gulp Aufgabe registriert. Sie können die Abhängigkeiten angeben, wenn andere Aufgaben vor der aktuellen Aufgabe ausgeführt werden müssen. Zusätzliche Funktionen ermöglichen es Ihnen, ausführen und überwachen die Gulp Aufgaben, sowie legen Sie die Quelle (*Src*) und das Ziel (*Dest*) der Dateien, die geändert wird. Im folgenden sind die primären Gulp-API-Funktionen:

|Gulp-Funktion|Syntax|Beschreibung|
|---   |--- |--- |
|Aufgabe  |`gulp.task(name[, deps], fn) { }`|Die `task` Funktion erstellt eine Aufgabe. Die `name` Parameter definiert den Namen des Tasks. Die `deps` Parameter enthält ein Array von Aufgaben abgeschlossen werden, bevor diese Aufgabe ausgeführt wird. Die `fn` Parameter darstellt, eine Rückruffunktion, die die Vorgänge des Tasks führt.|
|Überwachungsfenster |`gulp.watch(glob [, opts], tasks) { }`|Die `watch` Funktion überwacht Dateien und führt Aufgaben aus, wenn eine Datei geändert wird. Die `glob` Parameter ist ein `string` oder `array` , der bestimmt, welche Dateien überwachen. Die `opts` Parameter stellt zusätzliche Datei beobachten von Optionen zur Verfügung.|
|src   |`gulp.src(globs[, options]) { }`|Die `src` Funktion enthält Dateien, die die Glob Werten entsprechen. Die `glob` Parameter ist ein `string` oder `array` , der bestimmt, welche Dateien um zu lesen. Die `options` Parameter bietet zusätzliche Dateioptionen.|
|Dest  |`gulp.dest(path[, options]) { }`|Die `dest` Funktion definiert einen Speicherort, auf die Dateien geschrieben werden können. Die `path` Parameter ist eine Zeichenfolge oder eine Funktion, die den Zielordner bestimmt. Die `options` Parameter ist ein Objekt, das Ausgabe-Ordneroptionen angibt.|

Zusätzliche Gulp-API-Referenzinformationen finden Sie unter [Gulp Docs API](https://github.com/gulpjs/gulp/blob/master/docs/API.md).

## <a name="gulp-recipes"></a>Gulp Rezepte

Die Gulp-Community bietet Gulp [Rezepte](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md). Diese Rezepte beinhalten Gulp Aufgaben zu allgemeinen Szenarien zu adressieren.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Gulp-Dokumentation](https://github.com/gulpjs/gulp/blob/master/docs/README.md)
* [Bundling und Minimierung in ASP.NET Core](bundling-and-minification.md)
* [Verwenden von Grunt in ASP.NET Core](using-grunt.md)
