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
ms.openlocfilehash: d8512bdd49b61019f22a49900bdd65086d821a6b
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2017
---
# <a name="bundling-and-minification-in-aspnet-core"></a>Bundling und Minimierung in ASP.NET Core

Bundling und Minimierung sind zwei Techniken können Sie in ASP.NET zur Verbesserung der Leistung beim Laden von Seite für Ihre Webanwendung. Bundling kombiniert mehrere Dateien in einer einzelnen Datei. Minimierung führt eine Vielzahl von anderen Code, der Optimierungen mit Skripts und CSS, was in kleinere Nutzlasten führt. Zusammen verwendet werden, verbessert Bündelung und Minimierung die Leistung der Load-Zeit durch die Anzahl der Anforderungen an den Server zu verringern und Verringern der Größe der angeforderten Objekte (z. B. CSS- und JavaScript-Dateien).

Dieser Artikel beschreibt die Vorteile der Verwendung von Bündelung und Minimierung, z. B., wie diese Funktionen mit ASP.NET Core-Anwendungen verwendet werden können.

## <a name="overview"></a>Übersicht

In ASP.NET Core-apps stehen Ihnen mehrere Optionen zum bundling und Minimierung mit clientseitigen Ressourcen. Die Standardvorlagen für MVC bieten eine Out-of-Box-Lösung, die mit einer Konfigurationsdatei und BuildBundlerMinifier NuGet-Paket. Drittanbieter-tools, z. B. [Gulp](using-gulp.md) und [Grunt](using-grunt.md) sind ebenfalls verfügbar zur Ausführen derselben Aufgaben sollten den Prozessen benötigt werden, zusätzliche Workflow oder Komplexität. Verwenden Sie zur Entwurfszeit Bündelung und Minimierung, werden die verkleinerte Dateien vor der Bereitstellung der Anwendung erstellt. Bundling und Minimierung mit vor der Bereitstellung bietet den Vorteil des reduzierten Serverlast. Allerdings es ist wichtig zu wissen, während der Entwurfszeit Bündelung und Minimierung Build Komplexität erhöht und kann nur für statische Dateien.

Bundling und Minimierung wird in erster Linie die erste Anforderung Seitenladezeit verbessern. Sobald eine Webseite angefordert wurde, speichert der Browser Objekte (JavaScript, CSS und Bilder), um Bündelung und Minimierung wird kein Leistungssteigerung beim Anfordern von derselben Seite bieten oder Seiten auf dem gleichen Standort die gleichen Ressourcen anfordern. Wenn Sie nicht Festlegen der Header ordnungsgemäß auf Ihre Medienobjekte abläuft und Bündelung und Minimierung verwenden Sie nicht, Heuristik beim Ermitteln des Browsers der Aktualität kennzeichnet die Medienobjekte veraltete nach ein paar Tagen und der Browser wird für jedes Medienobjekt eine validierungsanforderung erfordern. In diesem Fall geben Bündelung und Minimierung eine Leistungssteigerung auch nach der ersten Seitenanforderung.

### <a name="bundling"></a>Bundling

Bundling ist ein Feature, das ganz einfach kombinieren oder mehrere Dateien in einer einzelnen Datei bündeln. Darin, dass mehrere Dateien in einer einzelnen Datei bundling kombiniert werden, wird die Anzahl der Anforderungen an den Server, die erforderlich sind, abgerufen und eine Web-Ressource, z. B. eine Webseite angezeigt. Sie können CSS, JavaScript und andere Pakete erstellen. Weniger Dateien bedeutet weniger HTTP-Anforderungen in Ihrem Browser auf dem Server oder aus dem Dienst, der die Anwendung bereitstellen. Ergibt sich aus diesem verbessert die Leistung beim ersten Seite.

### <a name="minification"></a>Minimierung

Minimierung führt eine Vielzahl von verschiedenen codeoptimierungen, die zum Verringern der Größe der angeforderten Objekte (z. B. CSS, Bilder, JavaScript-Dateien). Allgemeine Ergebnisse von Minimierung gehören unnötige Leerzeichen und Kommentare entfernen und Verkürzung Variablennamen in einem Zeichen.

Betrachten Sie die folgenden JavaScript-Funktion:

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

Nach der Minimierung wird die Funktion auf die folgenden reduziert:

```javascript
AddAltToImg=function(t,a){var r=$(t,a);r.attr("alt",r.attr("id").replace(/ID/,""))};
```

Zusätzlich zu entfernen, Kommentare und Leerzeichen nicht erforderlich, wurden die folgenden Parameter und Variablen umbenannt (gekürzt) wie folgt:

Ursprünglich | Umbenannt
--- | :---:
imageTagAndImageID | t
imageContext | eine
imageElement | b

## <a name="impact-of-bundling-and-minification"></a>Auswirkungen der Bündelung und Minimierung

Die folgende Tabelle zeigt einige wichtige Unterschiede zwischen alle Objekte einzeln auflisten und Verwenden von Bündelung und Minimierung auf eine einfache Webseite:

Aktion | Mit B/M | Ohne B/M | Änderung
--- | :---: | :---: | :---:
Dateianforderungen |7 | 18 | 157%
KB übertragen | 156 | 264.68 | 70%
Laden Sie die Zeit (MS) | 885 | 2360 | 167%

Der gesendeten Bytes hat zu eine erhebliche Reduzierung mit das Bündelung ebenso wie Browsern Recht ausführlich mit den HTTP-Headern, die sie für Anforderungen gelten. Die Ladezeit zeigt eine große Verbesserung, jedoch in diesem Beispiel wird lokal ausgeführt wurde. Wenn Bündelung und Minimierung mit Ressourcen verwenden, die über ein Netzwerk übertragen, erhalten eine größere erzielen bei der Leistung.

## <a name="using-bundling-and-minification-in-a-project"></a>Verwenden in einem Projekt Bündelung und Minimierung

Das MVC-Projektvorlage enthält eine `bundleconfig.json` Konfigurationsdatei an, die die Optionen für jedes Paket definiert. Standardmäßig wird eine Paket-Konfiguration für die benutzerdefinierte JavaScript definiert (`wwwroot/js/site.js`) und Stylesheet (`wwwroot/css/site.css`) Dateien.

[!code-json[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/bundleconfig.json)]

Bundle-Optionen:

* OutputFileName - Namen der Paketdatei ausgegeben. Einen relativen Pfad darf das `bundleconfig.json` Datei. **Erforderlich**
* InputFiles - Array von Dateien bündeln. Hierbei handelt es sich um relative Pfade in der Konfigurationsdatei. **optionale**, * führt ein leerer Wert eine leere Ausgabedatei. [Globmodus](http://www.tldp.org/LDP/abs/html/globbingref.html) Muster werden unterstützt.
* verkleinernde - Minimierung von Optionen für die Ausgabe zu geben. **optionale**, *Standard:`minify: { enabled: true }`*
  * Konfigurationsoptionen sind pro Datei Ausgabetyp verfügbar.
    * [CSS-Minifier](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [JavaScript-Minifier](https://github.com/madskristensen/BundlerMinifier/wiki)
    * [HTML-Minifier](https://github.com/madskristensen/BundlerMinifier/wiki)
* IncludeInProject - generierte Dateien zur Projektdatei hinzufügen. **optionale**, *Standard - "false"*
* SourceMaps - Quelle Zuordnungen für die gebündelte Datei zu generieren. **optionale**, *Standard - "false"*

### <a name="visual-studio-2015--2017"></a>Visual Studio 2015 / 2017

Open `bundleconfig.json` in Visual Studio, wenn Ihre Umgebung keine Erweiterung installiert; eine Aufforderung erhält angewiesen, den vorhanden ist, die mit diesem Dateityp unterstützen kann.

![Vorschlag BuildBundlerMinifier-Erweiterung](../client-side/bundling-and-minification/_static/bundler-extension-suggestion.png)

Wählen Sie die Ansicht Erweiterungen, und Installieren der **Bundle & Minifier** Erweiterung (Visual Studio erfordert Neustart).

![Vorschlag BuildBundlerMinifier-Erweiterung](../client-side/bundling-and-minification/_static/view-extension.png)

Wenn der Neustart abgeschlossen ist, müssen Sie den Build zum Ausführen der Prozesse weboptimierungsframework und bündeln die clientseitigen Objekte zu konfigurieren. Mit der rechten Maustaste die `bundleconfig.json` Datei, und wählen Sie *Enable-Paket beim Erstellen... *.

Erstellen Sie das Projekt, und die `bundleconfig.json` in den Buildprozess erzeugen die Ausgabedateien auf Basis der Konfiguration enthalten ist.

```console
1>------ Build started: Project: BuildBundlerMinifierExample, Configuration: Debug Any CPU ------
1>
1>Bundler: Begin processing bundleconfig.json
1>Bundler: Done processing bundleconfig.json
1>BuildBundlerMinifierExample -> C:\BuildBundlerMinifierExample\bin\Debug\netcoreapp1.1\BuildBundlerMinifierExample.dll
========== Build: 1 succeeded or up-to-date, 0 failed, 0 skipped ==========
```

### <a name="visual-studio-code-or-command-line"></a>Visual Studio-Code oder über die Befehlszeile

Visual Studio und die Erweiterung Laufwerk den Bündelung und Minimierung-Prozess, die mit der GUI-Gesten; Allerdings stehen die gleichen Funktionen zur Verfügung, mit der `dotnet` CLI und BuildBundlerMinifier NuGet-Paket.

Fügen Sie dem Projekt das NuGet-Paket hinzu:

```console
dotnet add package BuildBundlerMinifier
```

Stellen Sie die Abhängigkeiten wieder her:

```console
dotnet restore
```

Erstellen Sie die app ein:

```console
dotnet build
```

Die Ausgabe aus dem Buildbefehl zeigt die Ergebnisse der Minimierung und/oder bundling entsprechend was konfiguriert ist.

```console
Microsoft (R) Build Engine version 15.1.545.13942
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Begin processing bundleconfig.json
     Minified wwwroot/css/site.min.css
  Bundler: Done processing bundleconfig.json
  BuildBundlerMinifierExample -> /BuildBundlerMinifierExample/bin/Debug/netcoreapp1.0/BuildBundlerMinifierExample.dll
```

## <a name="adding-files"></a>Hinzufügen von Dateien

In diesem Beispiel wird eine zusätzliche CSS-Datei namens hinzugefügt `custom.css` und konfiguriert Sie für Bündelung und Minimierung mit `site.css`, wodurch ein einzelnes `site.min.css`.

Custom.CSS

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

Fügen Sie den relativen Pfad zum `bundleconfig.json`.

[!code-json[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/bundleconfig2.json)]

> [!NOTE]
> Alternativ können Sie das Muster Globmodus missbraucht werden - `"inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]` Dateien und schließt die verkleinerte Dateimuster alle CSS abgerufen.

Erstellen Sie die Anwendung, und öffnen Sie `site.min.css`, jetzt Beachten Sie, dass Inhalt von `custom.css` wurde an das Ende der Datei angefügt.

## <a name="controlling-bundling-and-minification"></a>Steuern der Bündelung und Minimierung

Im Allgemeinen möchten Sie die gebündelten und verkleinerte Dateien der app nur in einer produktionsumgebung verwenden. Während der Entwicklung sollen Sie die ursprünglichen Dateien verwenden, damit Ihre app einfacher zu Debuggen ist.

Sie können angeben, welche Skripts und CSS-Dateien in Ihren Seiten verwenden die Umgebung Tag-Hilfsmethode in Ihrer Layoutseiten eingeschlossen werden sollen (finden Sie unter [Tag Hilfsprogramme](../mvc/views/tag-helpers/index.md)). Die Umgebung Tags-Hilfsprogramm wird nur seinen Inhalt gerendert, wenn in bestimmten Umgebungen ausgeführt. Finden Sie unter [arbeiten mit mehreren Umgebungen](../fundamentals/environments.md) ausführliche Informationen zum Angeben der aktuellen Umgebung.

Die folgenden umgebungstag Rendern unverarbeiteten CSS-Dateien aus, bei der Ausführung im die `Development` Umgebung:

[!code-html[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/Views/Shared/_Layout.cshtml?highlight=3&range=9-12)]

Diese umgebungs-Tag Rendern die gebündelten und verkleinerte CSS-Dateien aus, bei der Ausführung im `Production` oder `Staging`:

[!code-html[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/Views/Shared/_Layout.cshtml?highlight=5&range=13-18)]

## <a name="consuming-bundleconfigjson-from-gulp"></a>Verwenden von Gulp bundleconfig.json

Wenn Ihre app Bündelung und Minimierung Workflow zusätzliche Prozesse wie das eine bildverarbeitung Cache Abwehrprogramm, CDN Assest Verarbeitung, usw. erforderlich sind, können Sie das Paket und Minify Prozess in Gulp konvertieren.

> [!NOTE]
> Konvertierungsoption ist nur in Visual Studio 2015 und 2017 verfügbar.

Mit der rechten Maustaste die `bundleconfig.json` , und wählen Sie **in Gulp konvertieren... **. Dadurch wird die `gulpfile.js` und Installieren der erforderlichen Npm-Pakete.

![Konvertieren in Gulp](../client-side/bundling-and-minification/_static/convert-togulp.png)

Die `gulpfile.js` Lesevorgänge erzeugt der `bundleconfig.json` Datei für die Konfiguration, sie können daher weiterhin für die Eingaben/Ausgaben und Einstellungen verwendet werden.

[!code-javascript[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/gulpfile.js)]

Fügen Sie Folgendes in die Datei *.csproj, um Gulp zu aktivieren, wenn das Projekt in Visual Studio 2017 erstellt:

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
    <Exec Command="gulp min" />
</Target>
```

Fügen Sie Folgendes, um Gulp zu aktivieren, wenn das Projekt in Visual Studio 2015 erstellt die `project.json` Datei:

```json
"scripts": {
    "precompile": "gulp min"
}
```

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Verwenden von Gulp](using-gulp.md)
* [Verwenden von Grunt](using-grunt.md)
* [Arbeiten mit mehreren Umgebungen](../fundamentals/environments.md)
* [Tag Helpers (Taghilfsprogramme)](../mvc/views/tag-helpers/index.md)
