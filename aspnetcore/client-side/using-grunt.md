---
title: Verwenden von Grunt in ASP.NET Core
author: rick-anderson
description: ''
ms.author: riande
ms.date: 10/14/2016
uid: client-side/using-grunt
ms.openlocfilehash: 21fa565c930563bbc819c2a02ea71655193513d0
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272973"
---
# <a name="use-grunt-in-aspnet-core"></a><span data-ttu-id="b2110-102">Verwenden von Grunt in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b2110-102">Use Grunt in ASP.NET Core</span></span>

<span data-ttu-id="b2110-103">Durch [Noel Reis](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)</span><span class="sxs-lookup"><span data-stu-id="b2110-103">By [Noel Rice](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)</span></span>

<span data-ttu-id="b2110-104">Grunt ist ein JavaScript-Task Runner, die automatisiert Skript Minimierung, TypeScript-Kompilierung, Code "fusselfreien" Qualitätstools, CSS-Pre-Prozessoren und nahezu jeder sich wiederholende mühsam, die auf diese Weise zur Unterstützung von Cliententwicklung benötigt.</span><span class="sxs-lookup"><span data-stu-id="b2110-104">Grunt is a JavaScript task runner that automates script minification, TypeScript compilation, code quality "lint" tools, CSS pre-processors, and just about any repetitive chore that needs doing to support client development.</span></span> <span data-ttu-id="b2110-105">Grunt ist vollständig in Visual Studio unterstützt, obwohl die Projektvorlagen für ASP.NET Gulp standardmäßig verwenden (finden Sie unter [Verwenden von Gulp](using-gulp.md)).</span><span class="sxs-lookup"><span data-stu-id="b2110-105">Grunt is fully supported in Visual Studio, though the ASP.NET project templates use Gulp by default (see [Use Gulp](using-gulp.md)).</span></span>

<span data-ttu-id="b2110-106">Dieses Beispiel verwendet ein leeres Projekt für ASP.NET Core als Ausgangspunkt, um zeigen, wie der Client-Build-Prozesses von Grund auf neu zu automatisieren.</span><span class="sxs-lookup"><span data-stu-id="b2110-106">This example uses an empty ASP.NET Core project as its starting point, to show how to automate the client build process from scratch.</span></span>

<span data-ttu-id="b2110-107">Das Beispiel fertig bereinigt das Zielverzeichnis für die Bereitstellung, kombiniert JavaScript-Dateien, überprüft der Codequalität, Inhalt von JavaScript-Datei komprimiert und in das Stammverzeichnis Ihrer Webanwendung bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="b2110-107">The finished example cleans the target deployment directory, combines JavaScript files, checks code quality, condenses JavaScript file content and deploys to the root of your web application.</span></span> <span data-ttu-id="b2110-108">Wir verwenden die folgenden Pakete:</span><span class="sxs-lookup"><span data-stu-id="b2110-108">We will use the following packages:</span></span>

* <span data-ttu-id="b2110-109">**Grunt**: die Grunt Task Runner-Paket.</span><span class="sxs-lookup"><span data-stu-id="b2110-109">**grunt**: The Grunt task runner package.</span></span>

* <span data-ttu-id="b2110-110">**für die Altersvorsorge bereinigen Grunt**: ein Plug-in, die Dateien oder Verzeichnisse entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="b2110-110">**grunt-contrib-clean**: A plugin that removes files or directories.</span></span>

* <span data-ttu-id="b2110-111">**Grunt-für die Altersvorsorge-Jshint**: ein Plug-in, die Qualität der JavaScript-Code überprüft.</span><span class="sxs-lookup"><span data-stu-id="b2110-111">**grunt-contrib-jshint**: A plugin that reviews JavaScript code quality.</span></span>

* <span data-ttu-id="b2110-112">**Grunt-für die Altersvorsorge-Concat**: ein Plug-in, die Dateien in einer einzelnen Datei verknüpft.</span><span class="sxs-lookup"><span data-stu-id="b2110-112">**grunt-contrib-concat**: A plugin that joins files into a single file.</span></span>

* <span data-ttu-id="b2110-113">**für die Altersvorsorge uglify Grunt**: ein Plug-in, die JavaScript zur Reduzierung der Größe verkleinert.</span><span class="sxs-lookup"><span data-stu-id="b2110-113">**grunt-contrib-uglify**: A plugin that minifies JavaScript to reduce size.</span></span>

* <span data-ttu-id="b2110-114">**Grunt-für die Altersvorsorge-Überwachung**: ein Plug-in, die Aktivität "Datei" überwacht.</span><span class="sxs-lookup"><span data-stu-id="b2110-114">**grunt-contrib-watch**: A plugin that watches file activity.</span></span>

## <a name="preparing-the-application"></a><span data-ttu-id="b2110-115">Vorbereiten der Anwendung</span><span class="sxs-lookup"><span data-stu-id="b2110-115">Preparing the application</span></span>

<span data-ttu-id="b2110-116">Zum Starten, richten Sie eine neue leere Web-Anwendung und TypeScript-Beispiel-Dateien hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="b2110-116">To begin, set up a new empty web application and add TypeScript example files.</span></span> <span data-ttu-id="b2110-117">TypeScript-Dateien werden automatisch in JavaScript mithilfe der Standardeinstellungen für Visual Studio kompiliert und unsere Rohstoffe mit Grunt verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="b2110-117">TypeScript files are automatically compiled into JavaScript using default Visual Studio settings and will be our raw material to process using Grunt.</span></span>

1.  <span data-ttu-id="b2110-118">Erstellen Sie in Visual Studio ein neues `ASP.NET Web Application`.</span><span class="sxs-lookup"><span data-stu-id="b2110-118">In Visual Studio, create a new `ASP.NET Web Application`.</span></span>

2.  <span data-ttu-id="b2110-119">In der **neues ASP.NET-Projekt** Dialogfeld Wählen Sie die ASP.NET Core **leere** Vorlage, und klicken Sie auf die Schaltfläche "OK".</span><span class="sxs-lookup"><span data-stu-id="b2110-119">In the **New ASP.NET Project** dialog, select the ASP.NET Core **Empty** template and click the OK button.</span></span>

3.  <span data-ttu-id="b2110-120">Überprüfen Sie im Projektmappen-Explorer die Projektstruktur aus.</span><span class="sxs-lookup"><span data-stu-id="b2110-120">In the Solution Explorer, review the project structure.</span></span> <span data-ttu-id="b2110-121">Die `\src` Ordner enthält leere `wwwroot` und `Dependencies` Knoten.</span><span class="sxs-lookup"><span data-stu-id="b2110-121">The `\src` folder includes empty `wwwroot` and `Dependencies` nodes.</span></span>

    ![leere Web-Lösung](using-grunt/_static/grunt-solution-explorer.png)

4.  <span data-ttu-id="b2110-123">Fügen Sie einen neuen Ordner namens `TypeScript` in Ihrem Projektverzeichnis.</span><span class="sxs-lookup"><span data-stu-id="b2110-123">Add a new folder named `TypeScript` to your project directory.</span></span>

5.  <span data-ttu-id="b2110-124">Vor dem Hinzufügen von Dateien, stellen Sie sicher, dass Visual Studio die Option "kompilieren zu speichern" für TypeScript-Dateien überprüft.</span><span class="sxs-lookup"><span data-stu-id="b2110-124">Before adding any files, make sure that Visual Studio has the option 'compile on save' for TypeScript files checked.</span></span> <span data-ttu-id="b2110-125">Navigieren Sie zu **Tools** > **Optionen** > **Texteditor** > **Typescript**  >  **Projekt**:</span><span class="sxs-lookup"><span data-stu-id="b2110-125">Navigate to **Tools** > **Options** > **Text Editor** > **Typescript** > **Project**:</span></span>

    ![Optionen, die Einstellung Auto Compliation TypeScript-Dateien](using-grunt/_static/typescript-options.png)

6.  <span data-ttu-id="b2110-127">Mit der rechten Maustaste die `TypeScript` Verzeichnis, und wählen Sie **hinzufügen > Neues Element** aus dem Kontextmenü.</span><span class="sxs-lookup"><span data-stu-id="b2110-127">Right-click the `TypeScript` directory and select **Add > New Item** from the context menu.</span></span> <span data-ttu-id="b2110-128">Wählen Sie die **JavaScript-Datei** -Element aus, und nennen Sie die Datei *Tastes.ts* (Beachten Sie die \*TS-Erweiterung).</span><span class="sxs-lookup"><span data-stu-id="b2110-128">Select the **JavaScript file** item and name the file *Tastes.ts* (note the \*.ts extension).</span></span> <span data-ttu-id="b2110-129">Kopieren Sie die Zeile des folgenden TypeScript-Code in die Datei (Wenn Sie zu speichern, ein neues *Tastes.js* Datei wird angezeigt, mit der JavaScript-Quelle).</span><span class="sxs-lookup"><span data-stu-id="b2110-129">Copy the line of TypeScript code below into the file (when you save, a new *Tastes.js* file will appear with the JavaScript source).</span></span>
    
    ```typescript
    enum Tastes { Sweet, Sour, Salty, Bitter }
    ```

7.  <span data-ttu-id="b2110-130">Fügen Sie eine zweite Datei, die **TypeScript** Verzeichnis und nennen Sie sie `Food.ts`.</span><span class="sxs-lookup"><span data-stu-id="b2110-130">Add a second file to the **TypeScript** directory and name it `Food.ts`.</span></span> <span data-ttu-id="b2110-131">Kopieren Sie den folgenden Code in die Datei ein.</span><span class="sxs-lookup"><span data-stu-id="b2110-131">Copy the code below into the file.</span></span>

    ```typescript
    class Food {
      constructor(name: string, calories: number) {
        this._name = name;
        this._calories = calories;
      }
    
      private _name: string;
      get Name() {
        return this._name;
      }
    
      private _calories: number;
      get Calories() {
        return this._calories;
      }
    
      private _taste: Tastes;
      get Taste(): Tastes { return this._taste }
      set Taste(value: Tastes) {
        this._taste = value;
      }
    }
    ```

## <a name="configuring-npm"></a><span data-ttu-id="b2110-132">Konfigurieren von NPM</span><span class="sxs-lookup"><span data-stu-id="b2110-132">Configuring NPM</span></span>

<span data-ttu-id="b2110-133">Als Nächstes konfigurieren Sie NPM, die zum Herunterladen von Grunt und Grunt-Aufgaben.</span><span class="sxs-lookup"><span data-stu-id="b2110-133">Next, configure NPM to download grunt and grunt-tasks.</span></span>

1. <span data-ttu-id="b2110-134">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste des Projekts, und wählen **hinzufügen > Neues Element** aus dem Kontextmenü.</span><span class="sxs-lookup"><span data-stu-id="b2110-134">In the Solution Explorer, right-click the project and select **Add > New Item** from the context menu.</span></span> <span data-ttu-id="b2110-135">Wählen Sie die **NPM-Konfigurationsdatei** Element, lassen Sie den Standardnamen *"Package.JSON"*, und klicken Sie auf die **hinzufügen** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="b2110-135">Select the **NPM configuration file** item, leave the default name, *package.json*, and click the **Add** button.</span></span>

2. <span data-ttu-id="b2110-136">In der *"Package.JSON"* Dateien innerhalb der `devDependencies` Objekt von geschweiften Klammern, geben Sie "grunt".</span><span class="sxs-lookup"><span data-stu-id="b2110-136">In the *package.json* file, inside the `devDependencies` object braces, enter "grunt".</span></span> <span data-ttu-id="b2110-137">Wählen Sie `grunt` in Intellisense Liste aus, und drücken Sie die EINGABETASTE.</span><span class="sxs-lookup"><span data-stu-id="b2110-137">Select `grunt` from the Intellisense list and press the Enter key.</span></span> <span data-ttu-id="b2110-138">Visual Studio den Paketnamen Grunt Angebot, und fügen einen Doppelpunkt.</span><span class="sxs-lookup"><span data-stu-id="b2110-138">Visual Studio will quote the grunt package name, and add a colon.</span></span> <span data-ttu-id="b2110-139">Wählen Sie rechts neben dem Doppelpunkt ausmacht, die neueste stabile Version des Pakets vom oberen Rand der Intellisense-Liste (drücken Sie `Ctrl-Space` Wenn Intellisense nicht angezeigt wird).</span><span class="sxs-lookup"><span data-stu-id="b2110-139">To the right of the colon, select the latest stable version of the package from the top of the Intellisense list (press `Ctrl-Space` if Intellisense doesn't appear).</span></span>

    ![Grun Intellisense](using-grunt/_static/devdependencies-grunt.png)
    
    > [!NOTE]
    > <span data-ttu-id="b2110-141">NPM verwendet [semantischer versionsverwaltung](http://semver.org/) , Abhängigkeiten zu organisieren.</span><span class="sxs-lookup"><span data-stu-id="b2110-141">NPM uses [semantic versioning](http://semver.org/) to organize dependencies.</span></span> <span data-ttu-id="b2110-142">Semantische versionsverwaltung, auch bekannt als SemVer, identifiziert die Pakete mit den Nummerierungsschema <major>.<minor>. <patch>. IntelliSense vereinfacht semantischen versionsverwaltung, indem nur einige allgemeine Optionen angezeigt.</span><span class="sxs-lookup"><span data-stu-id="b2110-142">Semantic versioning, also known as SemVer, identifies packages with the numbering scheme <major>.<minor>.<patch>. Intellisense simplifies semantic versioning by showing only a few common choices.</span></span> <span data-ttu-id="b2110-143">Das oberste Element in der Intellisense-Liste (0.4.5 im obigen Beispiel) wird die neueste stabile Version des Pakets betrachtet.</span><span class="sxs-lookup"><span data-stu-id="b2110-143">The top item in the Intellisense list (0.4.5 in the example above) is considered the latest stable version of the package.</span></span> <span data-ttu-id="b2110-144">Das Caretzeichen (^) Symbol entspricht der aktuellsten Hauptversion und die Tilde (~) entspricht der aktuellsten Nebenversion.</span><span class="sxs-lookup"><span data-stu-id="b2110-144">The caret (^) symbol matches the most recent major version and the tilde (~) matches the most recent minor version.</span></span> <span data-ttu-id="b2110-145">Finden Sie unter der [NPM Semver Version Parser Verweis](https://www.npmjs.com/package/semver) als Leitfaden für die vollständige expressivität, die SemVer bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="b2110-145">See the [NPM semver version parser reference](https://www.npmjs.com/package/semver) as a guide to the full expressivity that SemVer provides.</span></span>

3. <span data-ttu-id="b2110-146">Hinzufügen Weitere Abhängigkeiten zu laden grunt-für die Altersvorsorge -\* für Pakete *Bereinigen*, *Jshint*, *Concat*, *uglify*, und *Überwachen* wie im folgenden Beispiel gezeigt.</span><span class="sxs-lookup"><span data-stu-id="b2110-146">Add more dependencies to load grunt-contrib-\* packages for *clean*, *jshint*, *concat*, *uglify*, and *watch* as shown in the example below.</span></span> <span data-ttu-id="b2110-147">Die Versionen müssen nicht im Beispiel übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="b2110-147">The versions don't need to match the example.</span></span>

    ```json
    "devDependencies": {
      "grunt": "0.4.5",
      "grunt-contrib-clean": "0.6.0",
      "grunt-contrib-jshint": "0.11.0",
      "grunt-contrib-concat": "0.5.1",
      "grunt-contrib-uglify": "0.8.0",
      "grunt-contrib-watch": "0.6.1"
    }
    ```

4. <span data-ttu-id="b2110-148">Speichern Sie die *"Package.JSON"* Datei.</span><span class="sxs-lookup"><span data-stu-id="b2110-148">Save the *package.json* file.</span></span>

<span data-ttu-id="b2110-149">Die Pakete für jedes Element DevDependencies werden zusammen mit den Dateien heruntergeladen, die jedes Paket erfordert.</span><span class="sxs-lookup"><span data-stu-id="b2110-149">The packages for each devDependencies item will download, along with any files that each package requires.</span></span> <span data-ttu-id="b2110-150">Sie finden die Paketdateien in der `node_modules` Verzeichnis durch Aktivieren der **alle Dateien anzeigen** Schaltfläche im Projektmappen-Explorer.</span><span class="sxs-lookup"><span data-stu-id="b2110-150">You can find the package files in the `node_modules` directory by enabling the **Show All Files** button in the Solution Explorer.</span></span>

![Grunt node_modules](using-grunt/_static/node-modules.png)

> [!NOTE]
> <span data-ttu-id="b2110-152">Wenn Sie möchten, können Sie Abhängigkeiten im Projektmappen-Explorer manuell wiederherstellen, indem Sie mit der rechten Maustaste auf `Dependencies\NPM` und Auswählen der **wiederherstellen Pakete** Option des Menüs.</span><span class="sxs-lookup"><span data-stu-id="b2110-152">If you need to, you can manually restore dependencies in Solution Explorer by right-clicking on `Dependencies\NPM` and selecting the **Restore Packages** menu option.</span></span>

![Stellen Sie die Pakete wieder her.](using-grunt/_static/restore-packages.png)

## <a name="configuring-grunt"></a><span data-ttu-id="b2110-154">Konfigurieren von Grunt</span><span class="sxs-lookup"><span data-stu-id="b2110-154">Configuring Grunt</span></span>

<span data-ttu-id="b2110-155">Grunt konfiguriert ist, über ein Manifest mit dem Namen *Gruntfile.js* , die definiert, lädt und registriert Sie Aufgaben, die manuell ausführen oder so konfiguriert, dass automatisch basierend auf Ereignisse in Visual Studio ausgeführt werden können.</span><span class="sxs-lookup"><span data-stu-id="b2110-155">Grunt is configured using a manifest named *Gruntfile.js* that defines, loads and registers tasks that can be run manually or configured to run automatically based on events in Visual Studio.</span></span>

1. <span data-ttu-id="b2110-156">Mit der rechten Maustaste des Projekts, und wählen Sie **hinzufügen > Neues Element**.</span><span class="sxs-lookup"><span data-stu-id="b2110-156">Right-click the project and select **Add > New Item**.</span></span> <span data-ttu-id="b2110-157">Wählen Sie die **Grunt-Konfigurationsdatei** aus, lassen Sie den Standardnamen *Gruntfile.js*, und klicken Sie auf die **hinzufügen** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="b2110-157">Select the **Grunt Configuration file** option, leave the default name, *Gruntfile.js*, and click the **Add** button.</span></span>

   <span data-ttu-id="b2110-158">Der anfängliche Code enthält eine Moduldefinition und die `grunt.initConfig()` Methode.</span><span class="sxs-lookup"><span data-stu-id="b2110-158">The initial code includes a module definition and the `grunt.initConfig()` method.</span></span> <span data-ttu-id="b2110-159">Die `initConfig()` dient zum Festlegen der Optionen für jedes Paket und der Rest des Moduls wird laden und Registrieren von Aufgaben.</span><span class="sxs-lookup"><span data-stu-id="b2110-159">The `initConfig()` is used to set options for each package, and the remainder of the module will load and register tasks.</span></span>
    
   ```javascript
   module.exports = function (grunt) {
     grunt.initConfig({
     });
   };
   ```

2. <span data-ttu-id="b2110-160">Innerhalb der `initConfig()` -Methode, fügen Sie die Optionen für die `clean` Aufgabe wie im Beispiel gezeigt *Gruntfile.js* unten.</span><span class="sxs-lookup"><span data-stu-id="b2110-160">Inside the `initConfig()` method, add options for the `clean` task as shown in the example *Gruntfile.js* below.</span></span> <span data-ttu-id="b2110-161">Die clean-Aufgabe nimmt ein Array von Verzeichniszeichenfolgen.</span><span class="sxs-lookup"><span data-stu-id="b2110-161">The clean task accepts an array of directory strings.</span></span> <span data-ttu-id="b2110-162">Dieser Task entfernt Dateien aus "Wwwroot" / Lib und das gesamte/temp-Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="b2110-162">This task removes files from wwwroot/lib and removes the entire /temp directory.</span></span>

    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
        clean: ["wwwroot/lib/*", "temp/"],
      });
    };
    ```

3. <span data-ttu-id="b2110-163">Fügen Sie einen Aufruf hinzu, unter der Methode initConfig() `grunt.loadNpmTasks()`.</span><span class="sxs-lookup"><span data-stu-id="b2110-163">Below the initConfig() method, add a call to `grunt.loadNpmTasks()`.</span></span> <span data-ttu-id="b2110-164">Dadurch wird die Aufgabe aus Visual Studio ausführbar.</span><span class="sxs-lookup"><span data-stu-id="b2110-164">This will make the task runnable from Visual Studio.</span></span>

    ```javascript
    grunt.loadNpmTasks("grunt-contrib-clean");
    ```

4. <span data-ttu-id="b2110-165">Speichern Sie *Gruntfile.js*.</span><span class="sxs-lookup"><span data-stu-id="b2110-165">Save *Gruntfile.js*.</span></span> <span data-ttu-id="b2110-166">Die Datei sollte etwa wie im folgenden Screenshot aussehen.</span><span class="sxs-lookup"><span data-stu-id="b2110-166">The file should look something like the screenshot below.</span></span>

    ![anfängliche gruntfile](using-grunt/_static/gruntfile-js-initial.png)

5. <span data-ttu-id="b2110-168">Mit der rechten Maustaste *Gruntfile.js* , und wählen Sie **Taskausführungs-Explorer** aus dem Kontextmenü.</span><span class="sxs-lookup"><span data-stu-id="b2110-168">Right-click *Gruntfile.js* and select **Task Runner Explorer** from the context menu.</span></span> <span data-ttu-id="b2110-169">Der Taskausführungs-Explorer-Fenster wird geöffnet.</span><span class="sxs-lookup"><span data-stu-id="b2110-169">The Task Runner Explorer window will open.</span></span>

    ![Task Runner-Explorer-Menü](using-grunt/_static/task-runner-explorer-menu.png)

6. <span data-ttu-id="b2110-171">Überprüfen Sie, ob `clean` zeigt unter **Aufgaben** in der Taskausführungs-Explorer.</span><span class="sxs-lookup"><span data-stu-id="b2110-171">Verify that `clean` shows under **Tasks** in the Task Runner Explorer.</span></span>

    ![Task Runner-Explorer-Aufgabenliste](using-grunt/_static/task-runner-explorer-tasks.png)

7. <span data-ttu-id="b2110-173">Mit der rechten Maustaste der clean-Aufgabe aus, und wählen Sie **ausführen** aus dem Kontextmenü.</span><span class="sxs-lookup"><span data-stu-id="b2110-173">Right-click the clean task and select **Run** from the context menu.</span></span> <span data-ttu-id="b2110-174">Ein Befehlsfenster zeigt Fortschritt der Aufgabe.</span><span class="sxs-lookup"><span data-stu-id="b2110-174">A command window displays progress of the task.</span></span>

    ![Task Runner-Explorer ausführen clean-Aufgabe](using-grunt/_static/task-runner-explorer-run-clean.png)
    
    > [!NOTE]
    > <span data-ttu-id="b2110-176">Es sind keine Dateien oder Verzeichnisse noch bereinigen.</span><span class="sxs-lookup"><span data-stu-id="b2110-176">There are no files or directories to clean yet.</span></span> <span data-ttu-id="b2110-177">Auf Wunsch können Sie sie im Projektmappen-Explorer manuell erstellen, und führen die clean-Aufgabe mit einem Test.</span><span class="sxs-lookup"><span data-stu-id="b2110-177">If you like, you can manually create them in the Solution Explorer and then run the clean task as a test.</span></span>
    
8. <span data-ttu-id="b2110-178">Fügen Sie in der Methode initConfig() einen Eintrag für `concat` mit den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="b2110-178">In the initConfig() method, add an entry for `concat` using the code below.</span></span>

    <span data-ttu-id="b2110-179">Die `src` Eigenschaftsarray Listet Dateien in der Reihenfolge kombinieren, dass sie kombiniert werden sollen.</span><span class="sxs-lookup"><span data-stu-id="b2110-179">The `src` property array lists files to combine, in the order that they should be combined.</span></span> <span data-ttu-id="b2110-180">Die `dest` Eigenschaft weist den Pfad zu der Datei, das erzeugt wird.</span><span class="sxs-lookup"><span data-stu-id="b2110-180">The `dest` property assigns the path to the combined file that's produced.</span></span>

    ```javascript
    concat: {
      all: {
        src: ['TypeScript/Tastes.js', 'TypeScript/Food.js'],
        dest: 'temp/combined.js'
      }
    },
    ```
    
    > [!NOTE]
    > <span data-ttu-id="b2110-181">Die `all` Eigenschaft im obigen Code ist der Name eines Ziels.</span><span class="sxs-lookup"><span data-stu-id="b2110-181">The `all` property in the code above is the name of a target.</span></span> <span data-ttu-id="b2110-182">Ziele werden in einige Grunt-Aufgaben verwendet, um mehrere Buildumgebungen zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="b2110-182">Targets are used in some Grunt tasks to allow multiple build environments.</span></span> <span data-ttu-id="b2110-183">Sie können die integrierte Ziele, die mithilfe von Intellisense anzeigen oder eine eigene zuweisen.</span><span class="sxs-lookup"><span data-stu-id="b2110-183">You can view the built-in targets using Intellisense or assign your own.</span></span>
    
9. <span data-ttu-id="b2110-184">Hinzufügen der `jshint` Aufgabe mit den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="b2110-184">Add the `jshint` task using the code below.</span></span>

    <span data-ttu-id="b2110-185">Das Jshint Codequalität Hilfsprogramm ausgeführt wird vor jeder JavaScript-Datei im temporären Verzeichnis gefunden.</span><span class="sxs-lookup"><span data-stu-id="b2110-185">The jshint code-quality utility is run against every JavaScript file found in the temp directory.</span></span>
    
    ```javascript
    jshint: {
      files: ['temp/*.js'],
      options: {
        '-W069': false,
      }
    },
    ```

    > [!NOTE]
    > <span data-ttu-id="b2110-186">Die Option "-W069" wird ein Fehler erzeugt, indem Jshint Wenn JavaScript verwendet die Syntax, um eine Eigenschaft anstatt Punktnotation, d. h. zuweisen Klammer `Tastes["Sweet"]` anstelle von `Tastes.Sweet`.</span><span class="sxs-lookup"><span data-stu-id="b2110-186">The option "-W069" is an error produced by jshint when JavaScript uses bracket syntax to assign a property instead of dot notation, i.e. `Tastes["Sweet"]` instead of `Tastes.Sweet`.</span></span> <span data-ttu-id="b2110-187">Die Option deaktiviert die Warnung, um den Rest des Prozesses zu fortfahren zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="b2110-187">The option turns off the warning to allow the rest of the process to continue.</span></span>

10. <span data-ttu-id="b2110-188">Hinzufügen der `uglify` Aufgabe mit den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="b2110-188">Add the `uglify` task using the code below.</span></span>

    <span data-ttu-id="b2110-189">Der Task verkleinert die *combined.js* -Datei im temporären Verzeichnis gefunden und erstellt die Ergebnisdatei in die standardmäßige Benennungskonvention befolgen "Wwwroot" / Lib  *\<Dateiname\>. min.js*.</span><span class="sxs-lookup"><span data-stu-id="b2110-189">The task minifies the *combined.js* file found in the temp directory and creates the result file in wwwroot/lib following the standard naming convention *\<file name\>.min.js*.</span></span>
    
    ```javascript
    uglify: {
     all: {
       src: ['temp/combined.js'],
       dest: 'wwwroot/lib/combined.min.js'
     }
    },
    ```

11. <span data-ttu-id="b2110-190">Fügen Sie unter dem Aufruf grunt.loadNpmTasks(), die für die Altersvorsorge bereinigen Grunt lädt der gleichen Aufrufs für Jshint Concat und uglify mit den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="b2110-190">Under the call grunt.loadNpmTasks() that loads grunt-contrib-clean, include the same call for jshint, concat and uglify using the code below.</span></span>
    
    ```javascript
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    ```

12. <span data-ttu-id="b2110-191">Speichern Sie *Gruntfile.js*.</span><span class="sxs-lookup"><span data-stu-id="b2110-191">Save *Gruntfile.js*.</span></span> <span data-ttu-id="b2110-192">Die Datei sollte etwa wie im folgenden Beispiel aussehen.</span><span class="sxs-lookup"><span data-stu-id="b2110-192">The file should look something like the example below.</span></span>

    ![Beispiel für eine vollständige grunt](using-grunt/_static/gruntfile-js-complete.png)

13. <span data-ttu-id="b2110-194">Beachten Sie, die die Task Runner-Explorer-Aufgabenliste enthält `clean`, `concat`, `jshint` und `uglify` Aufgaben.</span><span class="sxs-lookup"><span data-stu-id="b2110-194">Notice that the Task Runner Explorer Tasks list includes `clean`, `concat`, `jshint` and `uglify` tasks.</span></span> <span data-ttu-id="b2110-195">Jede Aufgabe in der Reihenfolge ausgeführt, und beobachten Sie die Ergebnisse im Projektmappen-Explorer.</span><span class="sxs-lookup"><span data-stu-id="b2110-195">Run each task in order and observe the results in Solution Explorer.</span></span> <span data-ttu-id="b2110-196">Jede Aufgabe sollte nur dann fehlerfrei ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="b2110-196">Each task should run without errors.</span></span>
    
    ![Führen Sie jede Aufgabe Taskausführungs-explorer](using-grunt/_static/task-runner-explorer-run-each-task.png)
    
    <span data-ttu-id="b2110-198">Die Concat-Aufgabe erstellt ein neues *combined.js* Datei und fügt es in das temp-Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="b2110-198">The concat task creates a new *combined.js* file and places it into the temp directory.</span></span> <span data-ttu-id="b2110-199">Der Task Jshint einfach ausgeführt wird und keine Ausgabe erzeugt.</span><span class="sxs-lookup"><span data-stu-id="b2110-199">The jshint task simply runs and doesn't produce output.</span></span> <span data-ttu-id="b2110-200">Der Task Uglify erstellt ein neues *combined.min.js* Datei und fügt sie in "Wwwroot" / Lib.</span><span class="sxs-lookup"><span data-stu-id="b2110-200">The uglify task creates a new *combined.min.js* file and places it into wwwroot/lib.</span></span> <span data-ttu-id="b2110-201">Nach Abschluss sollte die Lösung der folgende Screenshot aussehen:</span><span class="sxs-lookup"><span data-stu-id="b2110-201">On completion, the solution should look something like the screenshot below:</span></span>
    
    ![Nachdem alle Aufgaben auf Projektmappen-explorer](using-grunt/_static/solution-explorer-after-all-tasks.png)
    
    > [!NOTE]
    > <span data-ttu-id="b2110-203">Weitere Informationen zu den Optionen für jedes Paket finden Sie auf [ https://www.npmjs.com/ ](https://www.npmjs.com/) und Suche den Paketnamen in das Suchfeld auf der Hauptseite.</span><span class="sxs-lookup"><span data-stu-id="b2110-203">For more information on the options for each package, visit [https://www.npmjs.com/](https://www.npmjs.com/) and lookup the package name in the search box on the main page.</span></span> <span data-ttu-id="b2110-204">Beispielsweise können Sie das Paket Grunt für die Altersvorsorge bereinigen, um einen Link zur Dokumentation abzurufen, der allen Parametern erläutert nachschlagen.</span><span class="sxs-lookup"><span data-stu-id="b2110-204">For example, you can look up the grunt-contrib-clean package to get a documentation link that explains all of its parameters.</span></span>

### <a name="all-together-now"></a><span data-ttu-id="b2110-205">Jetzt alle zusammen</span><span class="sxs-lookup"><span data-stu-id="b2110-205">All together now</span></span>

<span data-ttu-id="b2110-206">Verwenden Sie die Grunt `registerTask()` Methode, um eine Reihe von Aufgaben in einer bestimmten Reihenfolge auszuführen.</span><span class="sxs-lookup"><span data-stu-id="b2110-206">Use the Grunt `registerTask()` method to run a series of tasks in a particular sequence.</span></span> <span data-ttu-id="b2110-207">Z. B. zum Ausführen des Beispiels Schritte oben in der Reihenfolge bereinigen -> Concat -> Jshint -> uglify, fügen Sie den folgenden Code hinzu, an das Modul.</span><span class="sxs-lookup"><span data-stu-id="b2110-207">For example, to run the example steps above in the order clean -> concat -> jshint -> uglify, add the code below to the module.</span></span> <span data-ttu-id="b2110-208">Der Code sollte auf der gleichen Ebene wie die Aufrufe loadNpmTasks() außerhalb InitConfig hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="b2110-208">The code should be added to the same level as the loadNpmTasks() calls, outside initConfig.</span></span>

```javascript
grunt.registerTask("all", ['clean', 'concat', 'jshint', 'uglify']);
```

<span data-ttu-id="b2110-209">Die neue Aufgabe angezeigt wird, in der Taskausführungs-Explorer unter Alias Aufgaben.</span><span class="sxs-lookup"><span data-stu-id="b2110-209">The new task shows up in Task Runner Explorer under Alias Tasks.</span></span> <span data-ttu-id="b2110-210">Sie können mit der rechten Maustaste, und führen Sie ihn genau wie andere Aufgaben.</span><span class="sxs-lookup"><span data-stu-id="b2110-210">You can right-click and run it just as you would other tasks.</span></span> <span data-ttu-id="b2110-211">Die `all` Task ausgeführt `clean`, `concat`, `jshint` und `uglify`, in der Reihenfolge.</span><span class="sxs-lookup"><span data-stu-id="b2110-211">The `all` task will run `clean`, `concat`, `jshint` and `uglify`, in order.</span></span>

![Alias-Grunt-Aufgaben](using-grunt/_static/alias-tasks.png)

## <a name="watching-for-changes"></a><span data-ttu-id="b2110-213">Erkennen von Änderungen</span><span class="sxs-lookup"><span data-stu-id="b2110-213">Watching for changes</span></span>

<span data-ttu-id="b2110-214">Ein `watch` -Task sollten Sie die Dateien und Verzeichnisse.</span><span class="sxs-lookup"><span data-stu-id="b2110-214">A `watch` task keeps an eye on files and directories.</span></span> <span data-ttu-id="b2110-215">Die Überwachung wird Aufgaben automatisch ausgelöst, wenn Änderungen erkannt wird.</span><span class="sxs-lookup"><span data-stu-id="b2110-215">The watch triggers tasks automatically if it detects changes.</span></span> <span data-ttu-id="b2110-216">InitConfig zu überwachenden Änderungen an den folgenden Code hinzugefügt \*JS-Dateien im Verzeichnis TypeScript.</span><span class="sxs-lookup"><span data-stu-id="b2110-216">Add the code below to initConfig to watch for changes to \*.js files in the TypeScript directory.</span></span> <span data-ttu-id="b2110-217">Wenn eine JavaScript-Datei geändert wird, `watch` führt die `all` Aufgabe.</span><span class="sxs-lookup"><span data-stu-id="b2110-217">If a JavaScript file is changed, `watch` will run the `all` task.</span></span>

```javascript
watch: {
  files: ["TypeScript/*.js"],
  tasks: ["all"]
}
```

<span data-ttu-id="b2110-218">Fügen Sie einen Aufruf von `loadNpmTasks()` zum Anzeigen der `watch` Aufgabe im Task Runner-Explorer.</span><span class="sxs-lookup"><span data-stu-id="b2110-218">Add a call to `loadNpmTasks()` to show the `watch` task in Task Runner Explorer.</span></span>

```javascript
grunt.loadNpmTasks('grunt-contrib-watch');
```

<span data-ttu-id="b2110-219">Mit der rechten Maustaste der Aufgabe überwachen Taskausführungs-Explorer, und wählen Sie ausführen aus dem Kontextmenü.</span><span class="sxs-lookup"><span data-stu-id="b2110-219">Right-click the watch task in Task Runner Explorer and select Run from the context menu.</span></span> <span data-ttu-id="b2110-220">Das Befehlsfenster, das Ausführen der Überwachung des Tasks anzeigt wird ein "gewartet..." angezeigt.</span><span class="sxs-lookup"><span data-stu-id="b2110-220">The command window that shows the watch task running will display a "Waiting…"</span></span> <span data-ttu-id="b2110-221">Vorgang nicht gefunden werden konnte.</span><span class="sxs-lookup"><span data-stu-id="b2110-221">message.</span></span> <span data-ttu-id="b2110-222">Öffnen Sie eine TypeScript-Dateien, fügen Sie ein Leerzeichen hinzu, und speichern Sie die Datei.</span><span class="sxs-lookup"><span data-stu-id="b2110-222">Open one of the TypeScript files, add a space, and then save the file.</span></span> <span data-ttu-id="b2110-223">Dies auslösen die Aufgabe überwachen und Auslösen anderer Aufgaben in der Reihenfolge ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="b2110-223">This will trigger the watch task and trigger the other tasks to run in order.</span></span> <span data-ttu-id="b2110-224">Der folgende Screenshot zeigt ein Beispiel ausführen.</span><span class="sxs-lookup"><span data-stu-id="b2110-224">The screenshot below shows a sample run.</span></span>

![Ausführen von Aufgaben-Ausgabe](using-grunt/_static/watch-running.png)

## <a name="binding-to-visual-studio-events"></a><span data-ttu-id="b2110-226">Binden an Visual Studio-Ereignisse</span><span class="sxs-lookup"><span data-stu-id="b2110-226">Binding to Visual Studio events</span></span>

<span data-ttu-id="b2110-227">Es sei denn, Sie möchten Ihre Aufgaben manuell zu starten, jedes Mal, wenn Sie in Visual Studio arbeiten, können Sie Aufgaben zum Binden **vor dem Build**, **nach dem Erstellen**, **Bereinigen**, und  **Projekt öffnen** Ereignisse.</span><span class="sxs-lookup"><span data-stu-id="b2110-227">Unless you want to manually start your tasks every time you work in Visual Studio, you can bind tasks to **Before Build**, **After Build**, **Clean**, and **Project Open** events.</span></span>

<span data-ttu-id="b2110-228">Binden wir `watch` , damit es ausgeführt wird, jedes Mal, wenn Visual Studio wird geöffnet.</span><span class="sxs-lookup"><span data-stu-id="b2110-228">Let’s bind `watch` so that it runs every time Visual Studio opens.</span></span> <span data-ttu-id="b2110-229">Im Task Runner-Explorer mit der rechten Maustaste in der Aufgabe überwachen, und wählen **Bindungen > Projekt öffnen** aus dem Kontextmenü.</span><span class="sxs-lookup"><span data-stu-id="b2110-229">In Task Runner Explorer, right-click the watch task and select **Bindings > Project Open** from the context menu.</span></span>

![Binden Sie eine Aufgabe auf das Projekt öffnen](using-grunt/_static/bindings-project-open.png)

<span data-ttu-id="b2110-231">Entladen, und das Projekt erneut laden.</span><span class="sxs-lookup"><span data-stu-id="b2110-231">Unload and reload the project.</span></span> <span data-ttu-id="b2110-232">Wenn das Projekt erneut geladen wird, wird die Überwachung Aufgabe automatisch gestartet.</span><span class="sxs-lookup"><span data-stu-id="b2110-232">When the project loads again, the watch task will start running automatically.</span></span>

## <a name="summary"></a><span data-ttu-id="b2110-233">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="b2110-233">Summary</span></span>

<span data-ttu-id="b2110-234">Grunt ist eine leistungsstarke Task Runner, mit denen meisten Client-Buildaufgaben automatisiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="b2110-234">Grunt is a powerful task runner that can be used to automate most client-build tasks.</span></span> <span data-ttu-id="b2110-235">Grunt nutzt NPM, um die Pakete und Funktionen, die Integration in Visual Studio-Tools zu übermitteln.</span><span class="sxs-lookup"><span data-stu-id="b2110-235">Grunt leverages NPM to deliver its packages, and features tooling integration with Visual Studio.</span></span> <span data-ttu-id="b2110-236">Task Runner-Explorer von Visual Studio erkennt Änderungen an Konfigurationsdateien und stellt eine geeignete Benutzeroberfläche zum Ausführen von Aufgaben, Ausführen von Tasks anzeigen und Aufgaben an Visual Studio-Ereignisse binden.</span><span class="sxs-lookup"><span data-stu-id="b2110-236">Visual Studio's Task Runner Explorer detects changes to configuration files and provides a convenient interface to run tasks, view running tasks, and bind tasks to Visual Studio events.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b2110-237">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="b2110-237">Additional resources</span></span>

   * [<span data-ttu-id="b2110-238">Verwenden von Gulp</span><span class="sxs-lookup"><span data-stu-id="b2110-238">Use Gulp</span></span>](using-gulp.md)
