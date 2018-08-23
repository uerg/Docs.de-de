---
title: Erstellen von ansprechenden, antwortenden Websites mit Bootstrap und ASP.NET Core
author: ardalis
description: Informationen Sie zum Verwenden von Bootstrap für die Entwicklung von reaktionsfähigen Webanwendungen mit ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: client-side/bootstrap
ms.openlocfilehash: 1ccf33b299739f5aa963a53feb70b44b290443ca
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823971"
---
# <a name="build-beautiful-responsive-sites-with-bootstrap-and-aspnet-core"></a>Erstellen von ansprechenden, antwortenden Websites mit Bootstrap und ASP.NET Core

<a name="bootstrap-index"></a>

Von [Steve Smith](https://ardalis.com/)

Bootstrap ist aktuell das am häufigsten verwendeten Webframework für die Entwicklung von reaktionsfähigen Webanwendungen. Es bietet eine Reihe von Features und Vorteile, die mit Ihrer Website, die benutzererfahrung verbessern können, unabhängig davon, ob Sie am Front-End-Entwurf und Entwicklung oder Experte für Einsteiger. Bootstrap wird als ein Satz von CSS und JavaScript-Dateien bereitgestellt und dient zur Skalierung Ihrer Website oder Anwendung von Smartphones über Tablets bis zu Desktops bei einer effizienten.

## <a name="get-started"></a>Erste Schritte

Es gibt mehrere Möglichkeiten zum Einstieg in Bootstrap. Wenn Sie eine neue Webanwendung in Visual Studio starten, können Sie die Standard-Starter-Vorlage für ASP.NET Core, auswählen, in denen Groß-/Kleinschreibung Bootstrap bereits vorinstalliert wird:

![Starten Sie in der Projektmappenansicht für Starter-Vorlage](bootstrap/_static/bootstrap-in-starter-template.png)

Hinzufügen von Bootstrap zu einer ASP.NET Core-Projekt ist einfach eine Frage der hinzugefügt wird *"bower.JSON"* als Abhängigkeit:

[!code-json[](../common/samples/WebApplication1/bower.json?highlight=5)]

Dies ist die empfohlene Methode zum Hinzufügen von Bootstrap zu einem ASP.NET Core-Projekt.

Sie können auch mit einer von mehreren Paket-Manager, z. B. Bower, Npm und NuGet Bootstrap installieren. In jedem Fall ist der Prozess im Wesentlichen identisch:

### <a name="bower"></a>Bower

```console
bower install bootstrap
```

### <a name="npm"></a>npm

```console
npm install bootstrap
```

### <a name="nuget"></a>NuGet

```console
Install-Package bootstrap
```

> [!NOTE]
> Die empfohlene Vorgehensweise für das clientseitige Abhängigkeiten wie Bootstrap in ASP.NET Core über Bower installieren (mit *"bower.JSON"*, wie oben gezeigt). Die Verwendung von Npm/NuGet werden angezeigt, um zu veranschaulichen, wie einfach das Bootstrap-Stil auf andere Arten von Webanwendungen, einschließlich früherer Versionen von ASP.NET hinzugefügt werden kann.

Wenn Sie Ihre eigenen lokalen Versionen von Bootstrap verweisen, müssen Sie sie in alle Seiten zu verweisen, die dazu verwendet werden. In der Produktion sollten Sie Bootstrap Verwenden eines CDN verweisen. In der standardmäßigen ASP.NET Core-Site-Vorlage die *"_Layout.cshtml"* Datei also wie folgt aus:

[!code-html[](../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=9,13,51,59)]

> [!NOTE]
> Wenn Sie eine der Bootstrap jQuery-Plug-Ins verwenden möchten, müssen Sie auch jQuery zu verweisen.

## <a name="basic-templates-and-features"></a>Basic-Vorlagen und Funktionen

Die meisten einfache bootstrapvorlage entspricht weitgehend der *"_Layout.cshtml"* gezeigte Datei, einfach und höher umfasst ein einfachen Menüs für die Navigation und einem Ort, an den Rest der Seite gerendert.

### <a name="basic-navigation"></a>Grundlagen zur navigation

Die Standardvorlage verwendet eine Reihe von `<div>` Elemente, die eine obere Navigationsleiste und dem Hauptteil der Seite dargestellt. Wenn es sich bei Verwendung von HTML5 können Sie die erste ersetzen `<div>` markieren mit einer `<nav>` Tag, um den gleichen Effekt, erhalten jedoch eine präzisere Semantik. In diesem ersten `<div>` können Sie sehen, es gibt noch mehrere andere. Zunächst wird eine `<div>` mit einer Klasse "Container", und klicken Sie dann in der Sie zwei weitere `<div>` Elemente: "Navbar-Header" und "Navbar-Collapse". Das div-Navbar-Header-Element enthält eine Schaltfläche, die angezeigt wird, wenn der Bildschirm unter eine bestimmte Mindestbreite, 3, horizontale Linien angezeigt wird (ein so genannter "Hamburger-Symbol"). Das Symbol wird mit reinen HTML und CSS dargestellt; kein Bild ist erforderlich. Dies ist der Code, der das Symbol mit den einzelnen zeigt die <span> Tags, der rendering einen weißen Balken:

```html
<button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
</button>
```

Darüber hinaus den Anwendungsnamen, die in der oberen linken Ecke angezeigt wird. Das Navigationsmenü im Hauptbereich wird gerendert, indem die `<ul>` Element im zweiten DIV-Element, und enthält Links zur Startseite, Info, und wenden Sie sich an. Unten im Navigationsbereich den Hauptteil der einzelnen Seiten gerendert wird, in einer anderen `<div>`, mit den Klassen "Container" und "Text-Inhalt" markiert. In der einfachen \_Layout-Datei, die hier gezeigten, den Inhalt der Seite werden durch die spezifische Ansicht zugeordnet, die Seite, und klicken Sie dann eine einfache gerendert `<footer>` hinzugefügt wird, an das Ende der `<div>` Element. Sie können sehen, wie die integrierte zu den Seiten mit angezeigt wird mit dieser Vorlage:

![Zu den Seiten](bootstrap/_static/about-page-wide.png)

Mit "Hamburger"-Schaltfläche in der rechten oberen Ecke, die reduzierte Navigationsleiste angezeigt wird, wenn eine bestimmte Breite des Fensters unterschreitet:

![Info-Seite mit Hamburger-Menü](bootstrap/_static/about-page-hamburger.png)

Klicken auf das Symbol zeigen, die Elemente in einer vertikalen Schublade, die zwischen dem oberen Rand der Seite nach unten verschoben wird:

![Info-Seite mit erweiterten Hamburger-Menü](bootstrap/_static/about-page-hamburger-open.png)

### <a name="typography-and-links"></a>Typografie und links

Bootstrap richtet ein grundlegende Typografie, Farben und Links in der CSS-Formatierung des Standorts. Dieser CSS-Datei enthält die Standardstile für Tabellen, Schaltflächen, Formularelemente, Bilder usw. ([erfahren Sie mehr](http://getbootstrap.com/css/)). Ein besonders nützliches Feature ist das Layoutsystem Raster, das ich gleich beschreibe.

### <a name="grids"></a>Raster

Eines der beliebtesten Features von Bootstrap ist die Layout-Rastersystem. Moderne Webanwendungen vermeiden Sie die Verwendung der `<table>` Tag für das Layout, stattdessen die Verwendung dieses Elements in tatsächlichen tabellarische Daten einschränken. Stattdessen Spalten und Zeilen können angeordnet werden mithilfe einer Reihe von `<div>` Elemente und die entsprechenden CSS-Klassen. Es gibt mehrere Vorteile dieses Ansatzes, einschließlich der Möglichkeit zum Anpassen des Layouts von Rastern vertikal auf schmalen Bildschirmen, z.B. auf Smartphones darstellen.

[Layout des Bootstrap-Rastersystem](http://getbootstrap.com/css/#grid) basiert auf zwölf Spalten. Diese Zahl wurde gewählt, da es in 1, 2, 3 oder 4 Spalten gleichmäßig unterteilt werden kann, und die Spaltenbreite zur variieren können, innerhalb von 1/12. die vertikale Breite des Bildschirms. Zum Einstieg in das Raster Layoutsystem, sollten Sie beginnen mit einem Container `<div>` und fügen Sie dann eine Zeile `<div>`wie hier gezeigt:

```html
<div class="container">
    <div class="row">
        ...
    </div>
</div>
```

Fügen Sie zusätzliche `<div>` Elemente für jede Spalte, und geben Sie die Anzahl der Spalten, die `<div>` sollten (von 12) belegen, als Teil einer CSS-Klasse, die mit "Col - Md-" beginnen. Sollten Sie einfach zwei Spalten mit gleicher Größe haben, würden Sie z. B. eine Klasse von "Col-Md-6" für jede verwenden. In diesem Fall "Md" ist die Kurzform für "Mittel" und bezieht sich auf dem Desktopcomputer Standardformaten Anzeigegrößen. Es gibt vier verschiedene Optionen, die, denen Sie auswählen können, und jedes wird für höhere Breiten verwendet werden, es sei denn, der außer Kraft gesetzt (also wenn Sie das Layout, unabhängig von der Bildschirmbreite behoben werden soll, geben Sie einfach Xs-Klassen können).

Präfix der CSS-Klasse | Speicherstufe des Geräts | Breite
:---: | :---: | :---:
col-xs- | Smartphones | < 768px
col-sm- | Tablets | > = 768px
Col-Md - | Desktops | > = 992px
SP-Lg - | Größere Desktopanzeigen | > = 1200px

Beim Angeben von zwei Spalten mit "Col-Md-6" das resultierende Layout werden zwei Spalten am desktop Lösungen, aber diese beiden Spalten werden vertikal gestapelt, beim Rendern auf kleineren Geräten (oder einem engeren Browserfenster auf einem Desktop), Benutzer einfach anzeigen der Inhalt ohne die Notwendigkeit, einen horizontalen Bildlauf durchführen.

Bootstrap wird immer auf ein einspaltiges Layout, standardmäßig, daher müssen Sie nur Spalten angeben, wenn Sie möchten, dass mehr als eine Spalte. Sie explizit anzugeben, dass möchten nur dann eine `<div>` nehmen Sie alle 12 Spalten wäre das Überschreiben des Verhaltens einer größeren Speicherstufe des Geräts. Wenn Sie mehrere Geräteklassen Ebene angeben möchten, müssen Sie die spaltenrenderings an bestimmten Punkten zurücksetzen. Hinzufügen eines Clearfix DIV-Elements, das nur innerhalb einer bestimmten Ansicht sichtbar ist kann dies zu erreichen, wie hier gezeigt:

![Raster für schmale und breitzeichenfolgen viewport](bootstrap/_static/narrow-and-wide-viewport-grid.png)

Im obigen Beispiel freigeben 1 und 2 eine Zeile in das Layout "Md", während zwei und drei eine Zeile in das Layout von "Xs" verwendet. Ohne die Clearfix `<div>`, 2 und 3 werden nicht ordnungsgemäß in der Ansicht "Xs" (Beachten Sie, die nur eine, vier und fünf angezeigt werden) angezeigt:

![Raster ohne clearfix](bootstrap/_static/grid-without-clearfix.png)

In diesem Beispiel nur eine einzige Zeile `<div>` verwendet wurde, und Bootstrap noch vor allem hat das richtige in Bezug auf das Layout und Stapeln der Spalten. In der Regel sollten Sie eine Zeile angeben `<div>` für jede horizontale Zeile das Layout ist erforderlich, und Sie können natürlich Bootstrap Rastern in anderen schachteln. Wenn Sie dies tun, belegt jede verschachtelte Raster 100 % der Breite des Elements in dem sie abgelegt wird, die mithilfe von Spaltenklassen unterteilt werden kann.

### <a name="jumbotron"></a>Jumbotron

Wenn Sie die standardmäßige ASP.NET Core MVC-Vorlagen in Visual Studio 2012 oder 2013 verwendet haben, haben Sie wahrscheinlich den Jumbotron in Aktion gesehen. Es verweist auf eine große voller Breite Abschnitt einer Seite, die verwendet werden kann, um ein großes Hintergrundbild, einen Aufruf der Aktion, einer Rotator oder ähnliche Elemente anzuzeigen. Um eine Jumbotron zu einer Seite hinzuzufügen, fügen Sie einfach eine `<div>` Geben sie eine Klasse von "Jumbotron", und platzieren Sie einen Container `<div>` innerhalb und eigene Inhalte hinzufügen. Wir können einfach den Standard zu den Seiten für die Verwendung einer Jumbotron die Hauptüberschriften ihm angezeigten anpassen:

![Jumbotron-Beispiel](bootstrap/_static/jumbotron.png)

### <a name="buttons"></a>Schaltflächen

Schaltfläche "-Standardklassen und zugehörigen Farben werden in der folgenden Abbildung angezeigt.

![Design-Schaltflächen](bootstrap/_static/theme-buttons.png)

### <a name="badges"></a>Badges

Badges finden Sie in kleinen, in der Regel numerischen Legenden neben einem Navigationselement. Sie können eine Anzahl von Nachrichten oder Benachrichtigungen, die darauf warten, oder das Vorhandensein von Updates angeben. Die Angabe von solche Badges ist so einfach wie das Hinzufügen einer `<span>` den Text enthält, mit einer Klasse von "-Badge":

![Design badges](bootstrap/_static/theme-badges.png)

### <a name="alerts"></a>Benachrichtigungen

Sie müssen möglicherweise eine Art von Benachrichtigung, Warnung oder Fehlermeldung, die die Benutzer der Anwendung angezeigt. Das ist, in dem die Warnung Standardklassen nützlich sind. Es gibt vier verschiedene Schweregrade mit zugeordneten Farbschemas aus:

![Design-Warnungen](bootstrap/_static/theme-alerts.png)

### <a name="navbars-and-menus"></a>Navigationsleisten und Menüs

Unsere Layout enthält bereits einen standard-Navigationsleiste, aber die Bootstrap-Design unterstützt zusätzliche Gestaltungsoptionen. Wir können auch auf einfache Weise die Navigationsleiste vertikal angezeigt wird, statt horizontal, die wahlweise hat, sowie Hinzufügen von untergeordneten Navigation in den Flyout-Menüs Elemente optional. Einfache Navigation-Menüs, z. B. Registerkarte leisten, basieren auf der Basis von `<ul>` Elemente. Dies können sehr einfach, indem sie mit der CSS-Klassen "Nav" und "Nav-Registerkarten" Bereitstellung erstellt werden:

![Design Seitenübersichten](bootstrap/_static/theme-tabstrips.png)

Navigationsleisten werden auf ähnliche Weise erstellt, aber Sie sind ein wenig komplexer. Sie beginnen mit einem `<nav>` oder `<div>` mit einer Klasse von "Navbar", in dem ein Div-Containerelement die übrigen Elemente enthält. Die Seite enthält eine Navigationsleiste bereits im Kopf: unten abgebildet wird einfach erweitert, Hinzufügen von Unterstützung für eine Dropdown-Menü:

![Design Navigationsleisten](bootstrap/_static/theme-navbars.png)

### <a name="additional-elements"></a>Zusätzliche Elemente

Das Standarddesign kann auch verwendet werden, HTML-Tabellen in eine ordentlich formatierte Stil, einschließlich der Unterstützung für Stripesetvolumes Ansichten zu präsentieren. Es gibt Bezeichnungen mit Stilen, die auf die Schaltflächen ähneln. Sie können benutzerdefinierte Dropdownmenüs, die zusätzlichen Stilen Optionen über die standard-HTML unterstützen erstellen `<select>` Elements zusammen mit Navigationsleisten, wie unsere Starter-Standardwebsite wird bereits verwendet. Wenn Sie eine Statusanzeige benötigen, stehen die Formate auswählen, Gruppen aufzulisten sowie Bereiche, die einen Titel und Inhalt enthalten. Erkunden Sie zusätzliche Optionen aus dem standard Bootstrap-Design hier:

[http://getbootstrap.com/examples/theme/](http://getbootstrap.com/examples/theme/)

## <a name="more-themes"></a>Weitere Themen

Sie können die standard-Bootstrap-Design erweitern, durch das Überschreiben einiger oder aller dazugehörigen CSS, Anpassen der Farben und Stile entsprechend den Anforderungen Ihrer eigenen Anwendung. Wenn Sie über eine vorgefertigte Design starten möchten, stehen Ihnen mehrere Kataloge Design ist online, die auf die Bootstrap-Designs, wie z. B. WrapBootstrap.com (mit einer Vielzahl von professionellen Designs) und Bootswatch.com (die kostenlose Designs bietet) spezialisiert sind verfügbar. Einige der verfügbaren Vorlagen kostenpflichtigen renderingmodule Funktionalitätsebene auf der einfachen Bootstrap-Design, z. B. umfangreiche Unterstützung für die Verwaltung von Menüs und Dashboards mit umfassenden Diagrammen und Messgeräten. Ein Beispiel für eine beliebte kostenpflichtigen Vorlage ist Inspinia, die im folgenden Screenshot angezeigt wird:

![Beispiel-Design inspinia](bootstrap/_static/theme-inspinia.png)

Wenn Sie das Bootstrap-Design ändern möchten, setzen Sie die *Datei "Bootstrap.CSS"* -Datei für das Design, Sie, in möchten, der **Wwwroot/Css** Ordner, und ändern Sie die Verweise in *"_Layout.cshtml"* um es zu verweisen. Ändern Sie die Links für alle Umgebungen:

```html
<environment names="Development">
    <link rel="stylesheet" href="~/css/bootstrap.css" />
```

```html
<environment names="Staging,Production">
    <link rel="stylesheet" href="~/css/bootstrap.min.css" />
```

Wenn Sie Ihr eigenes Dashboard erstellen möchten, beginnen Sie aus dem kostenlosen Beispiel zur Verfügung: [ http://getbootstrap.com/examples/dashboard/ ](http://getbootstrap.com/examples/dashboard/).

## <a name="components"></a>Komponenten

Zusätzlich zu diesen Elementen, die bereits erläutert, enthält das Bootstrap-Unterstützung für eine Vielzahl von [integrierte Benutzeroberflächenkomponenten](http://getbootstrap.com/components/).

### <a name="glyphicons"></a>Glyphicons

Bootstrap enthält die Symbolsätze aus Glyphicons ([http://glyphicons.com](http://glyphicons.com)), mit mehr als 200 Symbolen, die für die Verwendung in der Bootstrap-fähigen Anwendung frei verfügbar. Hier ist nur ein kleinen Teil:

![Glyphicons](bootstrap/_static/theme-glyphicons.png)

### <a name="input-groups"></a>Eingabe-Gruppen

Eingabe mit Gruppen können Bündelung der zusätzliche Text oder Schaltflächen mit der ein input-Element, sodass der Benutzer eine intuitivere Benutzeroberfläche:

![Eingabe-Gruppen](bootstrap/_static/input-groups.png)

### <a name="breadcrumbs"></a>Der Breadcrumb-Leiste

Der Breadcrumb-Leiste sind eine gemeinsame UI-Komponente verwendet, um einem Benutzer die zuletzt verwendeten Verlauf oder des tiefensegments innerhalb eines Standorts Navigationshierarchie anzuzeigen. Ganz einfach hinzufügen, indem Sie die Klasse "Brotkrümelnavigation" an anwenden `<ol>` List-Element. Enthalten eine integrierte Unterstützung für die Paginierung mithilfe der Klasse "Paginierung" auf einem `<ul>` Element innerhalb einer `<nav>`. Reaktionsfähige eingebettete Diashows und Videos hinzufügen, indem `<iframe>`, `<embed>`, `<video>`, oder `<object>` -Elemente, die Bootstrap-Stil automatisch formatieren wird. Geben Sie ein bestimmtes Seitenverhältnis mithilfe von bestimmten Klassen wie "Einbetten-Reaktionsfähigkeit-16by9 an".

## <a name="javascript-support"></a>JavaScript-Unterstützung

Bootstrap JavaScript-Bibliothek enthält API-Unterstützung für die enthaltenen Komponenten, und damit ihr Verhalten in Ihrer Anwendung programmgesteuert zu kontrollieren. Darüber hinaus *"Bootstrap.js"* enthält mehr als einem Dutzend benutzerdefinierter jQuery-Plug-Ins, durch zusätzliche Features wie Übergänge, modale Dialogfelder, scrollen Sie zur Erkennung (Aktualisieren der Stile basierend auf, in denen der Benutzer einen Bildlauf im Dokument durchgeführt wurde), Collapse-Verhalten, Laufbänder und Anbringung Menüs an das Fenster, sodass sie außerhalb des Bildschirms einen Bildlauf nicht. Es ist nicht genügend Platz zum abdecken aller die JavaScript-Add-Ons in Bootstrap – weitere Bitte finden Sie unter integrierten [ http://getbootstrap.com/javascript/ ](http://getbootstrap.com/javascript/).

## <a name="summary"></a>Zusammenfassung

Bootstrap bietet ein Webframework, das so schnell und produktiver gestalten und formatieren es eine Vielzahl von Websites und Anwendungen verwendet werden kann. Die grundlegende Typografie und Formatvorlagen bieten eine angenehme Aussehen und Verhalten, die einfach durch die Unterstützung von benutzerdefinierten Designs können bearbeitet werden können, die manuell erstelltes oder im Handel erworben werden können. Es unterstützt eine Vielzahl von Webkomponenten, die in der Vergangenheit teuer Steuerelemente von Drittanbietern unterstützt moderne und offene Webstandards zu erreichen, würde angefordert haben.
