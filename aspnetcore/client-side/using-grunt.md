---
title: Verwenden von Grunt in ASP.NET Core
author: rick-anderson
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/using-grunt
ms.openlocfilehash: c23f170b36ac1b9623835337020f2b5ac9514971
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="using-grunt-in-aspnet-core"></a>Verwenden von Grunt in ASP.NET Core 

Durch [Noel Reis](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)

Grunt ist ein JavaScript-Task Runner, die automatisiert Skript Minimierung, TypeScript-Kompilierung, Code "fusselfreien" Qualitätstools, CSS-Pre-Prozessoren und nahezu jeder sich wiederholende mühsam, die auf diese Weise zur Unterstützung von Cliententwicklung benötigt. Grunt ist vollständig in Visual Studio unterstützt, obwohl die Projektvorlagen für ASP.NET Gulp standardmäßig verwenden (finden Sie unter [Verwenden von Gulp](using-gulp.md)).

Dieses Beispiel verwendet ein leeres Projekt für ASP.NET Core als Ausgangspunkt, um zeigen, wie der Client-Build-Prozesses von Grund auf neu zu automatisieren.

Das Beispiel fertig bereinigt das Zielverzeichnis für die Bereitstellung, kombiniert JavaScript-Dateien, überprüft der Codequalität, Inhalt von JavaScript-Datei komprimiert und in das Stammverzeichnis Ihrer Webanwendung bereitgestellt. Wir verwenden die folgenden Pakete:

* **Grunt**: die Grunt Task Runner-Paket.

* **für die Altersvorsorge bereinigen Grunt**: ein Plug-in, die Dateien oder Verzeichnisse entfernt werden.

* **Grunt-für die Altersvorsorge-Jshint**: ein Plug-in, die Qualität der JavaScript-Code überprüft.

* **Grunt-für die Altersvorsorge-Concat**: ein Plug-in, die Dateien in einer einzelnen Datei verknüpft.

* **für die Altersvorsorge uglify Grunt**: ein Plug-in, die JavaScript zur Reduzierung der Größe verkleinert.

* **Grunt-für die Altersvorsorge-Überwachung**: ein Plug-in, die Aktivität "Datei" überwacht.

## <a name="preparing-the-application"></a>Vorbereiten der Anwendung

Zum Starten, richten Sie eine neue leere Web-Anwendung und TypeScript-Beispiel-Dateien hinzufügen. TypeScript-Dateien werden automatisch in JavaScript mithilfe der Standardeinstellungen für Visual Studio kompiliert und unsere Rohstoffe mit Grunt verarbeitet werden.

1.  Erstellen Sie in Visual Studio ein neues `ASP.NET Web Application`.

2.  In der **neues ASP.NET-Projekt** Dialogfeld Wählen Sie die ASP.NET Core **leere** Vorlage, und klicken Sie auf die Schaltfläche "OK".

3.  Überprüfen Sie im Projektmappen-Explorer die Projektstruktur aus. Die `\src` Ordner enthält leere `wwwroot` und `Dependencies` Knoten.

    ![leere Web-Lösung](using-grunt/_static/grunt-solution-explorer.png)

4.  Fügen Sie einen neuen Ordner namens `TypeScript` in Ihrem Projektverzeichnis.

5.  Vor dem Hinzufügen von Dateien, stellen Sie sicher, dass Visual Studio die Option "kompilieren zu speichern" für TypeScript-Dateien überprüft. Navigieren Sie zu **Tools** > **Optionen** > **Texteditor** > **Typescript**  >  **Projekt**:

    ![Optionen, die Einstellung Auto Compliation TypeScript-Dateien](using-grunt/_static/typescript-options.png)

6.  Mit der rechten Maustaste die `TypeScript` Verzeichnis, und wählen Sie **hinzufügen > Neues Element** aus dem Kontextmenü. Wählen Sie die **JavaScript-Datei** -Element aus, und nennen Sie die Datei *Tastes.ts* (Beachten Sie die \*TS-Erweiterung). Kopieren Sie die Zeile des folgenden TypeScript-Code in die Datei (Wenn Sie zu speichern, ein neues *Tastes.js* Datei wird angezeigt, mit der JavaScript-Quelle).
    
    ```typescript
    enum Tastes { Sweet, Sour, Salty, Bitter }
    ```

7.  Fügen Sie eine zweite Datei, die **TypeScript** Verzeichnis und nennen Sie sie `Food.ts`. Kopieren Sie den folgenden Code in die Datei ein.

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

## <a name="configuring-npm"></a>Konfigurieren von NPM

Als Nächstes konfigurieren Sie NPM, die zum Herunterladen von Grunt und Grunt-Aufgaben.

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste des Projekts, und wählen **hinzufügen > Neues Element** aus dem Kontextmenü. Wählen Sie die **NPM-Konfigurationsdatei** Element, lassen Sie den Standardnamen *"Package.JSON"*, und klicken Sie auf die **hinzufügen** Schaltfläche.

2. In der *"Package.JSON"* Dateien innerhalb der `devDependencies` Objekt von geschweiften Klammern, geben Sie "grunt". Wählen Sie `grunt` in Intellisense Liste aus, und drücken Sie die EINGABETASTE. Visual Studio den Paketnamen Grunt Angebot, und fügen einen Doppelpunkt. Wählen Sie rechts neben dem Doppelpunkt ausmacht, die neueste stabile Version des Pakets vom oberen Rand der Intellisense-Liste (drücken Sie `Ctrl-Space` Wenn Intellisense nicht angezeigt wird).

    ![grun Intellisense](using-grunt/_static/devdependencies-grunt.png)
    
    > [!NOTE]
    > NPM verwendet [semantischer versionsverwaltung](http://semver.org/) , Abhängigkeiten zu organisieren. Semantische versionsverwaltung, auch bekannt als SemVer, identifiziert die Pakete mit den Nummerierungsschema <major>.<minor>. <patch>. IntelliSense vereinfacht semantischen versionsverwaltung, indem nur einige allgemeine Optionen angezeigt. Das oberste Element in der Intellisense-Liste (0.4.5 im obigen Beispiel) wird die neueste stabile Version des Pakets betrachtet. Das Caretzeichen (^) Symbol entspricht der aktuellsten Hauptversion und die Tilde (~) entspricht der aktuellsten Nebenversion. Finden Sie unter der [NPM Semver Version Parser Verweis](https://www.npmjs.com/package/semver) als Leitfaden für die vollständige expressivität, die SemVer bereitstellt.

3. Hinzufügen Weitere Abhängigkeiten zu laden grunt-für die Altersvorsorge -\* für Pakete *Bereinigen*, *Jshint*, *Concat*, *uglify*, und *Überwachen* wie im folgenden Beispiel gezeigt. Die Versionen müssen nicht im Beispiel übereinstimmen.

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

4. Speichern Sie die *"Package.JSON"* Datei.

Die Pakete für jedes Element DevDependencies werden zusammen mit den Dateien heruntergeladen, die jedes Paket erfordert. Sie finden die Paketdateien in der `node_modules` Verzeichnis durch Aktivieren der **alle Dateien anzeigen** Schaltfläche im Projektmappen-Explorer.

![Grunt node_modules](using-grunt/_static/node-modules.png)

> [!NOTE]
> Wenn Sie möchten, können Sie Abhängigkeiten im Projektmappen-Explorer manuell wiederherstellen, indem Sie mit der rechten Maustaste auf `Dependencies\NPM` und Auswählen der **wiederherstellen Pakete** Option des Menüs.

![Stellen Sie die Pakete wieder her.](using-grunt/_static/restore-packages.png)

## <a name="configuring-grunt"></a>Konfigurieren von Grunt

Grunt konfiguriert ist, über ein Manifest mit dem Namen *Gruntfile.js* , die definiert, lädt und registriert Sie Aufgaben, die manuell ausführen oder so konfiguriert, dass automatisch basierend auf Ereignisse in Visual Studio ausgeführt werden können.

1.  Mit der rechten Maustaste des Projekts, und wählen Sie **hinzufügen > Neues Element**. Wählen Sie die **Grunt-Konfigurationsdatei** aus, lassen Sie den Standardnamen *Gruntfile.js*, und klicken Sie auf die **hinzufügen** Schaltfläche.

    Der anfängliche Code enthält eine Moduldefinition und die `grunt.initConfig()` Methode. Die `initConfig()` dient zum Festlegen der Optionen für jedes Paket und der Rest des Moduls wird laden und Registrieren von Aufgaben.
    
    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
      });
    };
    ```

2. Innerhalb der `initConfig()` -Methode, fügen Sie die Optionen für die `clean` Aufgabe wie im Beispiel gezeigt *Gruntfile.js* unten. Die clean-Aufgabe nimmt ein Array von Verzeichniszeichenfolgen. Dieser Task entfernt Dateien aus "Wwwroot" / Lib und das gesamte/temp-Verzeichnis.

    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
        clean: ["wwwroot/lib/*", "temp/"],
      });
    };
    ```

3. Fügen Sie einen Aufruf hinzu, unter der Methode initConfig() `grunt.loadNpmTasks()`. Dadurch wird die Aufgabe aus Visual Studio ausführbar.

    ```javascript
    grunt.loadNpmTasks("grunt-contrib-clean");
    ```

4. Speichern Sie *Gruntfile.js*. Die Datei sollte etwa wie im folgenden Screenshot aussehen.

    ![anfängliche gruntfile](using-grunt/_static/gruntfile-js-initial.png)

5. Mit der rechten Maustaste *Gruntfile.js* , und wählen Sie **Taskausführungs-Explorer** aus dem Kontextmenü. Der Taskausführungs-Explorer-Fenster wird geöffnet.

    ![Task Runner-Explorer-Menü](using-grunt/_static/task-runner-explorer-menu.png)

6. Überprüfen Sie, ob `clean` zeigt unter **Aufgaben** in der Taskausführungs-Explorer.

    ![Task Runner-Explorer-Aufgabenliste](using-grunt/_static/task-runner-explorer-tasks.png)

7. Mit der rechten Maustaste der clean-Aufgabe aus, und wählen Sie **ausführen** aus dem Kontextmenü. Ein Befehlsfenster zeigt Fortschritt der Aufgabe.

    ![Task Runner-Explorer ausführen clean-Aufgabe](using-grunt/_static/task-runner-explorer-run-clean.png)
    
    > [!NOTE]
    > Es sind keine Dateien oder Verzeichnisse noch bereinigen. Auf Wunsch können Sie sie im Projektmappen-Explorer manuell erstellen, und führen die clean-Aufgabe mit einem Test.
    
8. Fügen Sie in der Methode initConfig() einen Eintrag für `concat` mit den folgenden Code.

    Die `src` Eigenschaftsarray Listet Dateien in der Reihenfolge kombinieren, dass sie kombiniert werden sollen. Die `dest` Eigenschaft weist den Pfad zu der Datei, das erzeugt wird.

    ```javascript
    concat: {
      all: {
        src: ['TypeScript/Tastes.js', 'TypeScript/Food.js'],
        dest: 'temp/combined.js'
      }
    },
    ```
    
    > [!NOTE]
    > Die `all` Eigenschaft im obigen Code ist der Name eines Ziels. Ziele werden in einige Grunt-Aufgaben verwendet, um mehrere Buildumgebungen zu ermöglichen. Sie können die integrierte Ziele, die mithilfe von Intellisense anzeigen oder eine eigene zuweisen.
    
9. Hinzufügen der `jshint` Aufgabe mit den folgenden Code.

    Das Jshint Codequalität Hilfsprogramm ausgeführt wird vor jeder JavaScript-Datei im temporären Verzeichnis gefunden.
    
    ```javascript
    jshint: {
      files: ['temp/*.js'],
      options: {
        '-W069': false,
      }
    },
    ```

    > [!NOTE]
    > Die Option "-W069" wird ein Fehler erzeugt, indem Jshint Wenn JavaScript verwendet die Syntax, um eine Eigenschaft anstatt Punktnotation, d. h. zuweisen Klammer `Tastes["Sweet"]` anstelle von `Tastes.Sweet`. Die Option deaktiviert die Warnung, um den Rest des Prozesses zu fortfahren zu ermöglichen.

10.  Hinzufügen der `uglify` Aufgabe mit den folgenden Code.

    Der Task verkleinert die *combined.js* -Datei im temporären Verzeichnis gefunden und erstellt die Ergebnisdatei in die standardmäßige Benennungskonvention befolgen "Wwwroot" / Lib  *\<Dateiname\>. min.js*.
    
    ```javascript
    uglify: {
      all: {
        src: ['temp/combined.js'],
        dest: 'wwwroot/lib/combined.min.js'
      }
    },
    ```

11. Fügen Sie unter dem Aufruf grunt.loadNpmTasks(), die für die Altersvorsorge bereinigen Grunt lädt der gleichen Aufrufs für Jshint Concat und uglify mit den folgenden Code.
    
    ```javascript
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    ```

12. Speichern Sie *Gruntfile.js*. Die Datei sollte etwa wie im folgenden Beispiel aussehen.

    ![Beispiel für eine vollständige grunt](using-grunt/_static/gruntfile-js-complete.png)

13. Beachten Sie, die die Task Runner-Explorer-Aufgabenliste enthält `clean`, `concat`, `jshint` und `uglify` Aufgaben. Jede Aufgabe in der Reihenfolge ausgeführt, und beobachten Sie die Ergebnisse im Projektmappen-Explorer. Jede Aufgabe sollte nur dann fehlerfrei ausgeführt.
    
    ![Führen Sie jede Aufgabe Taskausführungs-explorer](using-grunt/_static/task-runner-explorer-run-each-task.png)
    
    Die Concat-Aufgabe erstellt ein neues *combined.js* Datei und fügt es in das temp-Verzeichnis. Der Task Jshint einfach ausgeführt wird und keine Ausgabe erzeugt. Der Task Uglify erstellt ein neues *combined.min.js* Datei und fügt sie in "Wwwroot" / Lib. Nach Abschluss sollte die Lösung der folgende Screenshot aussehen:
    
    ![Nachdem alle Aufgaben auf Projektmappen-explorer](using-grunt/_static/solution-explorer-after-all-tasks.png)
    
    > [!NOTE]
    > Weitere Informationen zu den Optionen für jedes Paket finden Sie auf [https://www.npmjs.com/](https://www.npmjs.com/) und Suche den Paketnamen in das Suchfeld auf der Hauptseite. Beispielsweise können Sie das Paket Grunt für die Altersvorsorge bereinigen, um einen Link zur Dokumentation abzurufen, der allen Parametern erläutert nachschlagen.

### <a name="all-together-now"></a>Jetzt alle zusammen

Verwenden Sie die Grunt `registerTask()` Methode, um eine Reihe von Aufgaben in einer bestimmten Reihenfolge auszuführen. Z. B. zum Ausführen des Beispiels Schritte oben in der Reihenfolge bereinigen -> Concat -> Jshint -> uglify, fügen Sie den folgenden Code hinzu, an das Modul. Der Code sollte auf der gleichen Ebene wie die Aufrufe loadNpmTasks() außerhalb InitConfig hinzugefügt werden.

```javascript
grunt.registerTask("all", ['clean', 'concat', 'jshint', 'uglify']);
```

Die neue Aufgabe angezeigt wird, in der Taskausführungs-Explorer unter Alias Aufgaben. Sie können mit der rechten Maustaste, und führen Sie ihn genau wie andere Aufgaben. Die `all` Task ausgeführt `clean`, `concat`, `jshint` und `uglify`, in der Reihenfolge.

![Alias-Grunt-Aufgaben](using-grunt/_static/alias-tasks.png)

## <a name="watching-for-changes"></a>Überwachen von Änderungen

Ein `watch` -Task sollten Sie die Dateien und Verzeichnisse. Die Überwachung wird Aufgaben automatisch ausgelöst, wenn Änderungen erkannt wird. InitConfig zu überwachenden Änderungen an den folgenden Code hinzugefügt \*JS-Dateien im Verzeichnis TypeScript. Wenn eine JavaScript-Datei geändert wird, `watch` führt die `all` Aufgabe.

```javascript
watch: {
  files: ["TypeScript/*.js"],
  tasks: ["all"]
}
```

Fügen Sie einen Aufruf von `loadNpmTasks()` zum Anzeigen der `watch` Aufgabe im Task Runner-Explorer.

```javascript
grunt.loadNpmTasks('grunt-contrib-watch');
```

Mit der rechten Maustaste der Aufgabe überwachen Taskausführungs-Explorer, und wählen Sie ausführen aus dem Kontextmenü. Das Befehlsfenster, das Ausführen der Überwachung des Tasks anzeigt wird ein "gewartet..." angezeigt. Vorgang nicht gefunden werden konnte. Öffnen Sie eine TypeScript-Dateien, fügen Sie ein Leerzeichen hinzu, und speichern Sie die Datei. Dies auslösen die Aufgabe überwachen und Auslösen anderer Aufgaben in der Reihenfolge ausgeführt werden. Der folgende Screenshot zeigt ein Beispiel ausführen.

![Ausführen von Aufgaben-Ausgabe](using-grunt/_static/watch-running.png)

## <a name="binding-to-visual-studio-events"></a>Binden an Visual Studio-Ereignisse

Es sei denn, Sie möchten Ihre Aufgaben manuell zu starten, jedes Mal, wenn Sie in Visual Studio arbeiten, können Sie Aufgaben zum Binden **vor dem Build**, **nach dem Erstellen**, **Bereinigen**, und  **Projekt öffnen** Ereignisse.

Binden wir `watch` , damit es ausgeführt wird, jedes Mal, wenn Visual Studio wird geöffnet. Im Task Runner-Explorer mit der rechten Maustaste in der Aufgabe überwachen, und wählen **Bindungen > Projekt öffnen** aus dem Kontextmenü.

![Binden Sie eine Aufgabe auf das Projekt öffnen](using-grunt/_static/bindings-project-open.png)

Entladen, und das Projekt erneut laden. Wenn das Projekt erneut geladen wird, wird die Überwachung Aufgabe automatisch gestartet.

## <a name="summary"></a>Zusammenfassung

Grunt ist eine leistungsstarke Task Runner, mit denen meisten Client-Buildaufgaben automatisiert werden kann. Grunt nutzt NPM, um die Pakete und Funktionen, die Integration in Visual Studio-Tools zu übermitteln. Task Runner-Explorer von Visual Studio erkennt Änderungen an Konfigurationsdateien und stellt eine geeignete Benutzeroberfläche zum Ausführen von Aufgaben, Ausführen von Tasks anzeigen und Aufgaben an Visual Studio-Ereignisse binden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

   * [Verwenden von Gulp](using-gulp.md)
