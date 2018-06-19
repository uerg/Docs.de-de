---
title: Weniger, Schriftart, die in ASP.NET Core Awesome und Sass
author: ardalis
description: Erfahren Sie, wie kleiner Sass und Schriftart Awesome in ASP.NET Core-Anwendungen verwenden.
manager: wpickett
ms.author: tdykstra
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/less-sass-fa
ms.openlocfilehash: 3bb1c9006f8633485a420b52b5fa9b91b1875cc5
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
ms.locfileid: "30073522"
---
# <a name="less-sass-and-font-awesome-in-aspnet-core"></a>Weniger, Schriftart, die in ASP.NET Core Awesome und Sass

Von [Steve Smith](https://ardalis.com/)

Benutzer des Webanwendungen haben zunehmend hohe Anforderungen an, wenn es darum geht, formatieren und allgemeine Erfahrung. Innovative Webanwendungen nutzen häufig umfangreiche Tools und Frameworks zum Definieren und verwalten ihre Aussehen und Verhalten konsistent. Frameworks wie [Bootstrap](http://getbootstrap.com/) verlaufen eine lange gegen einen gemeinsamen Satz von Formatvorlagen und Layoutoptionen für Websites zu definieren. Die meisten nicht-trivialen Standorte jedoch profitieren auch von wird effektiv definieren und Verwalten von Formaten und cascading Stylesheet (CSS)-Dateien können als auch einfachen Zugriff auf nicht-Image-Symbole, mit denen der Standort-Schnittstelle eine intuitivere verfügen. Sind Sprachen und Tools, die Unterstützung von [weniger](http://lesscss.org/) und [Sass](http://sass-lang.com/), und Bibliotheken wie [Schriftart Awesome](http://fontawesome.io/), sind in verschiedenen Größen.

## <a name="css-preprocessor-languages"></a>CSS-Präprozessor Sprachen

Sprachen, die in anderen Sprachen kompiliert werden, um die Erfahrung der Arbeit mit der zugrunde liegenden Sprache zu verbessern, werden als Präprozessoren bezeichnet. Es gibt zwei gängige Präprozessoren für CSS: weniger und Sass.  Diese Präprozessoren Hinzufügen von Funktionen zu CSS, z. B. Unterstützung für Variablen und geschachtelte Regeln, die wodurch die Verwaltbarkeit von großen, komplexen Stylesheets verbessert. CSS-Code als eine Sprache ist ein sehr einfaches fehlen Unterstützung auch für etwas so einfaches wie Variablen, und dies CSS-Dateien von und sehr großer stellen tendenziell. Durch das real Programmiersprachenfeatures über Präprozessoren hinzufügen können, verringern die Duplizierung und bieten eine bessere Organisation von Stilregeln. Visual Studio bietet integrierte Unterstützung für beide Less und Sass sowie Erweiterungen, die die Entwicklungsaufgaben weiter verbessern können, bei der Arbeit mit diesen Sprachen.

Ein kurzes Beispiel wie Präprozessoren Lesbarkeit und verwaltbarkeit von Formatinformationen verbessern können sollten Sie dieses CSS:

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

Verwenden weniger, dies kann werden umgeschrieben, um alle diese Duplikation zu vermeiden mit eine *"mixin"* (so genannt, da es Ihnen ermöglicht, "mix in" Eigenschaften von einer Klasse oder in einen anderen Regelsatz):

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

## <a name="less"></a>weniger

Der Präprozessor weniger CSS ausgeführt, die mit Node.js. Verwenden Sie zum Installieren weniger Node Package Manager (Npm) über eine Eingabeaufforderung (-g bedeutet "global"):

```console
npm install -g less
```

Wenn Sie Visual Studio verwenden, können Sie mit einem niedrigeren, indem eine oder mehrere weniger Dateien zum Projekt hinzufügen und konfigurieren dann Gulp (oder Grunt), dass sie zum Zeitpunkt der Kompilierung verarbeitet beginnen. Hinzufügen einer *Stile* Ordner, um das Projekt, und fügen Sie dann einen neuen weniger Datei mit dem Namen *main.less* in diesen Ordner.

![Less-Datei hinzufügen](less-sass-fa/_static/add-less-file.png)

Sobald hinzugefügt wird, sollte die Ordnerstruktur etwa wie folgt aussehen:

![Struktur des Veröffentlichungsordners](less-sass-fa/_static/folder-structure.png)

Jetzt können Sie einige grundlegende formatieren in der Datei hinzufügen, wird in CSS kompiliert und auf den Ordner "Wwwroot" Gulp bereitgestellt werden.

Ändern Sie *main.less* , den folgenden Inhalt aufzunehmen, die eine einfache Farbpalette aus einer einzelnen Basisfarbe erstellt.

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

`@base` die andere @-prefixed Elemente sind Variablen. Jede von ihnen stellt eine Farbe dar. Mit Ausnahme von `@base`, sie sind festgelegt, mit Farbe Funktionen: heller und dunkler zu starten. Heller und dunkler sind größtenteils Erwartungen; Drehfeld passt den Farbton einer Farbe, um eine Anzahl von Grad (in der Nähe der Farbkreis). Der weniger Prozessor ist intelligent genug, um Variablen, die nicht verwendet werden, zu ignorieren, daher zur Veranschaulichung, wie diese Variablen verwendet, sie an einer beliebigen Stelle verwendet müssen. Die Klassen `.baseColor`, usw. wird gezeigt, das die berechneten Werte aller Variablen in der CSS-Datei, das erzeugt wird.

### <a name="get-started"></a>Erste Schritte

Erstellen einer **Npm-Konfigurationsdatei** (*"Package.JSON"*) in den Projektordner und bearbeiten es verweisen `gulp` und `gulp-less`:

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

Installieren von Abhängigkeiten entweder an der Eingabeaufforderung in den Projektordner oder in Visual Studio **Projektmappen-Explorer** (**Abhängigkeiten > Npm > Wiederherstellen von Paketen**).

```console
npm install
```

![VS stellen Pakete wieder her.](less-sass-fa/_static/restore-packages.png)

Erstellen Sie in den Projektordner ein **Konfigurationsdatei Gulp** (*gulpfile.js*) um den automatisierten Prozess zu definieren.  Fügen Sie am Anfang der Datei, um weniger darzustellen und ein Task kleiner ausgeführt, eine Variable hinzu:

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

Öffnen der **Task Runner-Explorer** (**Ansicht > andere Fenster > Task Runner-Explorer**). Für die Aufgaben sehen Sie eine neue Aufgabe, die mit dem Namen `less`. Sie müssen möglicherweise das Fenster zu aktualisieren.

Führen Sie die `less` Aufgabe und finden Sie ähnliche Informationen wie hier gezeigt wird:

![weniger Task runner](less-sass-fa/_static/less-task-runner.png)

Die *"Wwwroot" / Css* Ordner enthält jetzt eine neue Datei *main.css*:

![Main Css erstellt](less-sass-fa/_static/main-css-created.png)

Open *main.css* und etwa Folgendes angezeigt:

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

Fügen Sie eine einfache HTML-Seite, um die *"Wwwroot"* Ordner und Verweis *main.css* die Farbpalette in Aktion zu sehen.

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

Sehen Sie, dass auf der 180 Grad drehen `@base` verwendet, um erzeugen `@background` Farbkreis gegensätzlichen Farbe des Verzeichnisdiensts `@base`:

![Beispiel für weniger Tests](less-sass-fa/_static/less-test-screenshot.png)

Weniger bietet auch Unterstützung für geschachtelte Regeln als auch geschachtelte Abfragen. Z. B. wie definierende geschachtelte Hierarchien, so wie Menüs ausführliche CSS-Regeln führen können diese:

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

Im Idealfall aller der zugehörigen Stilregeln wird zusammengestellt werden in der CSS-Datei, aber in der Praxis ist ' Nothing ' erzwingen diese Regel außer Konvention und ggf. Kommentare.

Definieren dieselben Regeln mit weniger sieht wie folgt:

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

Beachten Sie, dass in diesem Fall werden alle untergeordneten Elemente des `nav` in ihrem Gültigkeitsbereich enthalten sind. Es ist nicht mehr jeder Wiederholung der übergeordneten Elemente (`nav`, `li`, `a`), und die Anzahl der insgesamt Zeilen wurde ebenfalls gelöscht (obwohl einige der der Parameter entspricht ein Ergebnis verstanden, Werte in der gleichen Zeilen im zweiten Beispiel platzieren). Es kann sehr hilfreich, OUI, um alle Regeln für ein angegebenes Benutzeroberflächenautomatisierungs-Element in einem explizit begrenzten Bereich, in diesem Fall finden Sie unter vom Rest der Datei in geschweiften Klammern festgelegt werden deaktiviert.

Die `&` Syntax ist ein kleiner Auswahlzeiger-Feature, mit & des aktuellen Selektor übergeordneten Elements darstellt. Ja, in dem eine {...} Block `&` stellt eine `a` Tag, und somit `&:link` entspricht `a:link`.

Medienabfragen, äußerst nützlich, beim Erstellen reaktionsfähiger Entwürfe können auch stark zur Wiederholung und Komplexität in CSS beitragen. Kleiner ermöglicht Media Abfragen innerhalb von Klassen, geschachtelt werden, damit die gesamte Klassendefinition nicht in anderen wiederholt werden muss der obersten Ebene `@media` Elemente. Hier ist z. B. CSS für ein Menü reagiert:

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

Dies kann eine bessere Leistung in weniger als definiert werden:

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

Eine weitere Funktion von weniger als die bereits gesehen haben, ist die Unterstützung für mathematische Operationen, sodass Stilattribute mit vordefinierten Variablen erstellt werden. Dadurch wird die verwandte Stile wesentlich einfacher aktualisieren, da die Basis-Variable geändert werden kann, und alle abhängigen Werte automatisch geändert.

CSS-Dateien, insbesondere bei umfangreichen Websites (und insbesondere wenn Medienabfragen verwendet werden), in der Regel im Laufe der Zeit sehr groß abrufen deren Verwendung unhandlich vornehmen. Less-Dateien können separat definiert werden, dann herausgezogen zusammen mit `@import` Direktiven. Weniger kann auch zum Importieren der einzelner CSS-Dateien, die auch bei Bedarf verwendet werden.

*Mixins* Parameter annehmen können und weniger unterstützt bedingten Logik in Form von "mixin" schützt, die eine deklarative Methode definieren, wenn bestimmte Mixins wirksam bereitstellen. Eine übliche Verwendung für "mixin" Wächter ist basierte auf wie Farben anpassen oder dunkel der Quellfarbe. Erhält eine "mixin", die einen Parameter für die Farbe des akzeptiert, kann ein Wächter "mixin" verwendet werden, so ändern Sie die Grundlage der jeweiligen Farbe "mixin":

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

Unsere aktuellen angegebenen `@base` Wert `#663333`, weniger dieses Skript erzeugt die folgende CSS:

```css
.feature {
    background-color: #FFF;
    color: #663333;
}
```

Weniger bietet eine Reihe von zusätzlichen Funktionen, aber dies sollten Sie einen Eindruck davon die Leistungsfähigkeit dieser vorverarbeiten Language.

## <a name="sass"></a>Sass

Sass ähnelt, bietet Unterstützung für viele der gleichen Funktionen, jedoch mit etwas andere Syntax. Er basiert, Ruby, statt JavaScript und besitzt somit unterschiedliche einrichtungsanforderungen auszeichnen. Die Originalsprache Sass geschweiften Klammern oder Semikolons verwendet, aber Bereichen mithilfe von Leerraum und Einzüge und stattdessen definiert wurde. Eine neue Syntax in Version 3 des Sass wurde eingeführt, **SCSS** ("Sassy CSS"). SCSS ähnelt CSS Einzugsebenen und Leerzeichen werden ignoriert, und verwendet stattdessen Semikolons und geschweifte Klammern.

Um Sass zu installieren, in der Regel Sie Erstinstallation Ruby (vorinstalliertem auf MacOS), und führen Sie dann:

```console
gem install sass
```

Jedoch wenn Sie Visual Studio ausführen heraus, können Sie mit Sass im Wesentlichen die gleiche Weise loslegen wie bei kleiner. Open *"Package.JSON"* und fügen Sie das Paket "Gulp Sass" `devDependencies`:

```json
"devDependencies": {
  "gulp": "3.9.1",
  "gulp-less": "3.3.0",
  "gulp-sass": "3.1.0"
}
```

Im nächsten Schritt ändern *gulpfile.js* $a Sass und eine Aufgabe Sass Dateien zu kompilieren, und legen die Ergebnisse in den Ordner "Wwwroot" hinzugefügt:

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

Nun Sie die Datei Sass fügen *main2.scss* auf die *Stile* Ordner im Stammverzeichnis des Projekts:

![Scss-Datei hinzufügen](less-sass-fa/_static/add-scss-file.png)

Open *main2.scss* und fügen Sie Folgendes hinzu:

```sass
$base: #CC0000;
body {
    background-color: $base;
}
```

Speichern Sie alle Dateien aus. Wenn Sie eine Aktualisierung ist momentan **Taskausführungs-Explorer**, sehen Sie eine `sass` Aufgabe. Führen Sie ihn, und suchen Sie der */Wwwroot/Css* Ordner. Es ist jetzt eine *main2.css* Datei, mit dem folgenden Inhalt:

```css
body {
    background-color: #CC0000;
}
```

Sass unterstützt die Schachtelung im Wesentlichen identisch, die kleiner ist, bietet ähnliche Vorteile, war. Dateien können von Funktion aufgeteilt werden und enthalten, mit der `@import` Richtlinie:

```sass
@import 'anotherfile';
```

Sass unterstützt Mixins auch mithilfe der `@mixin` Schlüsselwort zu ihrer Definition und `@include` zum Einschließen wie in diesem Beispiel aus [Sass lang.com](http://sass-lang.com):

```sass
@mixin border-radius($radius) {
    -webkit-border-radius: $radius;
    -moz-border-radius: $radius;
    -ms-border-radius: $radius;
    border-radius: $radius;
}

.box { @include border-radius(10px); }
```

Zusätzlich zu den Mixins unterstützt Sass auch das Konzept der Vererbung ermöglicht eine Klasse für eine andere erweitern. Es ist grundsätzlich ein "mixin", jedoch führt zu weniger CSS-Code ähnelt. Es erfolgt mithilfe der `@extend` Schlüsselwort. Um Mixins testen, fügen Sie Folgendes verwenden, um Ihre *main2.scss* Datei:

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

Überprüfen Sie die Ausgabe in *main2.css* nach dem Ausführen der `sass` Task "" **Taskausführungs-Explorer**:

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

Beachten Sie, dass alle von der allgemeinen Eigenschaften der Warnung "mixin" in jeder Klasse wiederholt werden. Die "mixin" hat ein gutes unterstützen Duplizierung zur Entwicklungszeit zu vermeiden, aber es ist immer noch CSS erstellen, erhalten Sie viele Duplikate, wodurch größer als der erforderlichen CSS-Dateien: ein mögliches Leistungsproblem.

Jetzt ersetzen die Warnung "mixin" mit einer `.alert` Klasse, und ändern Sie `@include` zu `@extend` (Denken Sie daran zu erweitern `.alert`, nicht `alert`):

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

Sass einmal ausgeführt, und überprüfen Sie die resultierende CSS:

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

Jetzt werden die Eigenschaften definiert, nur so oft wie erforderlich, und eine bessere Leistung CSS generiert wird.

Sass enthält auch Funktionen und Bedingungslogik Vorgänge, die weniger ähnlich. In der Tat sind die beiden Sprachen Funktionen sehr ähnlich.

## <a name="less-or-sass"></a>Weniger oder Sass?

Es ist immer noch keine Übereinstimmung darüber, ob es im Allgemeinen besser, weniger oder Sass (oder auch, ob die ursprünglichen Sass oder die neuere SCSS-Syntax im Sass bevorzugt). Die wichtigste Entscheidung ist möglicherweise auf **verwenden Sie eines der folgenden Tools**, im Gegensatz zu nur handcodierung von CSS-Dateien. Nachdem Sie, die vorgenommen haben Entscheidungsstrukturen, beide Less und Sass sind eine gute Wahl.

## <a name="font-awesome"></a>Schriftart Awesome

Zusätzlich zu CSS-Präprozessoren ist eine andere hervorragende Ressource zum Erstellen moderner Webanwendungen Awesome Schriftart. Schriftart Umwerfend ist ein Toolkit, das mehr als 500 skalierbare Vektorgrafiken Symbole bereitstellt, mit dem kostenlos in Ihrer Webanwendungen verwendet werden kann. Es wurde ursprünglich dienen zum Arbeiten mit Bootstrap, aber es ist nicht auf das Zielframework oder JavaScript-Bibliotheken.

Die einfachste Möglichkeit zum Einstieg in Schriftart Awesome ist einen Verweis darauf, über seine öffentliche Content Delivery Network (CDN)-Speicherort hinzufügen:

```html
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css">
```

Sie können auch ihn hinzufügen Ihrer Visual Studio-Projekt durch Hinzufügen zum "Abhängigkeiten" in *"bower.JSON"*:

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

Nachdem Sie einen Verweis auf die Schriftart Awesome auf einer Seite haben, können Sie Symbole für Ihre Anwendung hinzufügen, indem Sie die Schriftart Awesome Klassen, die in der Regel mit dem Präfix "Fa-", um Ihre Inline-HTML-Elemente anwenden (z. B. `<span>` oder `<i>`).  Beispielsweise können Sie Symbole einfacher Listen und Menüs, die mithilfe von Code wie folgt hinzufügen:

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

Dies erzeugt die folgenden im Browser – Beachten Sie das Symbol neben jedem Element:

![Symbole](less-sass-fa/_static/list-icons-screenshot.png)

Sie können eine vollständige Liste der verfügbaren Symbole anzeigen:

http://fontawesome.io/icons/

## <a name="summary"></a>Zusammenfassung

"Demand" moderner Webanwendungen zunehmend reaktionsfähig, flüssigen Entwürfe, bei die bereinigen, intuitive und benutzerfreundliche aus einer Vielzahl von Geräten sind. Verwalten von der Komplexität von der CSS-Stylesheets erforderlich, um diese Ziele zu erreichen ist am besten weniger mithilfe der Präprozessor Like oder Sass geschehen. Darüber hinaus Toolkits wie Schriftart Awesome schnell bereitzustellen Well-known Text Navigationsmenüs Symbole und Schaltflächen, Verbessern des allgemeinen Benutzers Ihrer Anwendung auftreten.
