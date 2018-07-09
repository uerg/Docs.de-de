---
uid: single-page-application/overview/templates/hottowel-template
title: Hot Towel-Vorlage | Microsoft-Dokumentation
author: madskristensen
description: HotTowel-Vorlage
ms.author: aspnetcontent
ms.date: 02/09/2013
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: 6145a34286ef113002b92d722e227005657098d9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825136"
---
<a name="hot-towel-template"></a>Hot Towel-Vorlage
====================
durch [Mads Kristensen](https://github.com/madskristensen)

> Die Hot Towel-MVC-Vorlage wird von John Papa geschrieben.
> 
> Wählen Sie, welche Version Sie herunterladen:
> 
> [Hot Towel-MVC-Vorlage für Visual Studio 2012](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [Hot Towel-MVC-Vorlage für Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> Hot Towel: Da Sie nicht, fahren Sie mit der einseitigen Anwendung ohne eine möchten!


Erstellen einer SPA möchten, jedoch kann nicht entscheiden, wo Sie anfangen? Hot Towel verwenden und in Sekunden müssen Sie eine SPA und alle Tools, die zum Erstellen von darauf erforderlich!

Hot Towel erstellt einen guter Ausgangspunkt für eine Single-Page Application (SPA) mit ASP.NET erstellen. Standardmäßig bietet es eine modulare Struktur für Code, Navigation in der, die Datenbindung, umfangreiche datenverwaltung und einfache, aber elegante formatieren. Hot Towel bietet alles, was Sie benötigen, um einer einseitigen Anwendung zu erstellen, damit Sie in Ihrer app, die nicht die Infrastruktur konzentrieren können.

> Erfahren Sie mehr über das Erstellen einer SPA aus [John Papa des Videos, Lernprogramme und Pluralsight-Kurse](http://johnpapa.net/spa?vsix).


## <a name="application-structure"></a>Anwendungsstruktur

Hot Towel-SPA enthält einen App-Ordner enthält die JavaScript und HTML-Dateien, die Ihre Anwendung zu definieren.

In den App-Ordner:

- durandal
- Dienste
- ViewModels
- Ansichten

Der App-Ordner enthält eine Auflistung von Modulen. Diese Module Kapseln der Funktionalität und Abhängigkeiten von anderen Modulen deklarieren. Ordner "Views" enthält den HTML-Code für Ihre Anwendung aus, und der Ordner "Viewmodels" enthält die Darstellungslogik für die Ansichten (eine allgemeine MVVM-Muster). Ordner "Dienste" ist ideal für gemeinsame Dienste, die Ihre Anwendung z. B. HTTP-Daten abrufen "oder" Lokaler Speicher Interaktion möglicherweise speichern. Es ist üblich, dass mehrere Viewmodels Wiederverwendungsrate für Code aus den Service-Modulen.

## <a name="aspnet-mvc-server-side-application-structure"></a>ASP.NET MVC-Server-Seite-Anwendungsstruktur

Hot Towel baut auf der vertraut und leistungsstarke ASP.NET MVC-Struktur.

- App\_starten
- Inhalt
- Controller
- Modelle
- Skripts
- Ansichten

## <a name="featured-libraries"></a>Ausgewählte Bibliotheken

- ASP.NET MVC
- ASP.NET-Web-API
- ASP.NET Web Optimization - Bündelung und Minimierung
- [Breeze.js](http://Breezejs.com) -Verwaltung von umfangreichen Daten
- [Durandal.js](http://Durandaljs.com) -Navigation und Aufbau der Ansicht
- ["Knockout.js"](http://Knockoutjs.com) -datenbindungen
- ["Require.js"](http://requirejs.org) -Modularität mit AMD und Optimierung
- [Toastr.js](http://jpapa.me/c7toastr) -Popup-Nachrichten
- [Twitter-Bootstrap](http://twitter.github.com/bootstrap/) – stabile CSS-Stile

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a>Installation über die Visual Studio 2012 Hot Towel-SPA-Vorlage

Hot Towel kann als Vorlage Visual Studio 2012 installiert werden. Klicken Sie einfach auf `File`  |  `New Project` , und wählen Sie `ASP.NET MVC 4 Web Application`. Wählen Sie dann den "Hot Towel Single Page Application"-Vorlage und führen Sie!

## <a name="installing-via-the-nuget-package"></a>Über das NuGet-Paket installieren

Hot Towel ist auch ein NuGet-Paket, das Standardfehlerprotokoll, ein vorhandenes leeres ASP.NET MVC-Projekt erweitert. Installieren Sie einfach mithilfe von Nuget aus, und führen Sie!

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a>Wie erstelle ich die Hot Towel?

Beginnen Sie einfach, Code hinzufügen.

1. Fügen Sie eigener serverseitigem Code, vorzugsweise Entity Framework und Web-API (was wirklich mit Breeze.js verwendet hinzu)
2. Hinzufügen von Ansichten, die `App/views` Ordner
3. Hinzufügen von Viewmodels die `App/viewmodels` Ordner
4. Hinzufügen von HTML-"und" Knockout-datenbindungen an die neuen Ansichten
5. Aktualisieren Sie die Navigation Routen in `shell.js`

## <a name="walkthrough-of-the-htmljavascript"></a>Exemplarische Vorgehensweise für die HTML-/JavaScript

### <a name="viewshottowelindexcshtml"></a>Views/HotTowel/index.cshtml

Datei "Index.cshtml" ist das Weiterleiten und die Ansicht für die MVC-Anwendung. Sie enthält alle standard-Meta-Tags, CSS-Links, und JavaScript-Verweise, die Sie erwarten. Der Textkörper enthält ein einzelnes `<div>` der ist, in dem der gesamte Inhalt (Ihre Ansichten) platziert wird, wenn diese angefordert werden. Die `@Scripts.Render` "Require.js" verwendet, um den Einstiegspunkt für den Code der Anwendung, der in enthalten ist, führen die `main.js` Datei. Ein Begrüßungsbildschirm wird bereitgestellt, um zu veranschaulichen, wie Sie die Anzeige eines Begrüßungsbildschirms zu erstellen, während Ihre app geladen wird.

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a>App/Main.js

Die `main.js` Datei enthält den Code, der ausgeführt wird, sobald Ihre app geladen wird. Dies ist, in denen Sie definieren die Routen für die Navigation, starten Sie die Ansichten festlegen und führen Sie alle Setup/bootstrapping z. B. die Daten Ihrer Anwendung vorbereiten möchten.

Die `main.js` -Datei definiert mehrere Durandal Modulen der Anwendung Kick-start. Die Anweisung definieren kann Module Abhängigkeiten aufgelöst werden, und sie für die Funktion verfügbar sind. Zunächst werden die debugging-Meldungen aktiviert, das Senden von Nachrichten über welche Ereignisse, dass es sich bei der Anwendung an das Konsolenfenster ausgeführt werden. Der app.start-Code weist Durandal-Framework, um die Anwendung zu starten. Die Konventionen festgelegt, sodass Durandal, alle Ansichten weiß und Viewmodels bzw. in der gleichen benannten Ordner enthalten sind. Schließlich die `app.setRoot` startet lädt die `shell` mithilfe eines vordefinierten `entrance` Animation.

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a>Ansichten

Ansichten finden Sie in der `App/views` Ordner.

### <a name="shellhtml"></a>Shell.HTML

Die `shell.html` das Masterlayout für den HTML-Code enthält. Alle anderen Ansichten besteht, an einer beliebigen Stelle in der Komponente, die von Ihrem `shell` anzeigen. Hot Towel bietet eine `shell` mit drei Bereichen: ein Header, einen Inhaltsbereich und eine Fußzeile. Jede dieser Regionen wird geladen, mit Inhalt zu anderen Ansichten für das angeforderte bilden.

Die `compose` Bindungen für die Kopf- und Fußzeile sind fest codiert wird, im Hot Towel, zeigen Sie auf die `nav` und `footer` -Sicht bzw. Die Compose-Bindung für den Abschnitt `#content` gebunden ist, um die `router` aktives Element des Moduls. Das heißt, wenn Sie auf ein Navigationslink, die sie die entsprechende Ansicht ist in diesem Bereich geladen wird.

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a>NAV.HTML

Die `nav.html` Navigationslinks für die SPA enthält. Dies ist, in denen die Menüstruktur, z. B. platziert werden kann. Dies ist häufig Daten gebunden (mithilfe von Knockout) auf die `router` Modul, um die Navigation anzuzeigen, die Sie in definiert, die `shell.js`. Knockout sieht aus, für die Datenbindung-Attribute und bindet an der `shell` "ViewModel", um die Navigation Routen anzuzeigen und Progressbar (mithilfe von Twitter Bootstrap) anzuzeigen Wenn der `router` -Modul ist in der Mitte Navigieren von einer Ansicht zu einem anderen (Siehe `router.isNavigating`).

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a>Datei "Home.HTML" und details.html

Diese Ansichten werden HTML-Code für benutzerdefinierte Ansichten enthalten. Wenn die `home` -link in der `nav` Menü "Ansicht" geklickt wird, die `home` im Bereich der Ansicht platziert werden die `shell` anzeigen. Diese Sichten können erweitert oder durch Ihre eigenen benutzerdefinierten Ansichten ersetzt werden.

### <a name="footerhtml"></a>Footer.HTML

Die `footer.html` enthält HTML-Code, der in der Fußzeile am unteren Rand angezeigt wird. die `shell` anzeigen.

## <a name="viewmodels"></a>ViewModels

ViewModels finden Sie in der `App/viewmodels` Ordner.

### <a name="shelljs"></a>Shell.js

Die `shell` Viewmodel enthält Eigenschaften und Funktionen, die an gebunden sind, die `shell` anzeigen. Dies ist häufig, in denen die Menü-Navigation-Bindungen gefunden werden (siehe die `router.mapNav` Logik).

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a>"Home.js" und details.js

Diese Viewmodels enthalten die Eigenschaften und Funktionen, die an gebunden sind, die `home` anzeigen. Außerdem enthält die Darstellungslogik für die Ansicht, und ist der Leim zwischen den Daten und die Ansicht.

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a>Dienste

Dienste befinden sich im Ordner "App-Dienste". Im Idealfall könnte Ihre zukünftige Dienste wie z. B. einem Modul "DataService", die für das Abrufen und Bereitstellen von remote-Daten verantwortlich ist, platziert werden.

### <a name="loggerjs"></a>logger.js

Hot Towel bietet eine `logger` Modul im Ordner "Dienste". Die `logger` Modul ist ideal zum Protokollieren von Meldungen an die Konsole und dem Benutzer im Popup-Popups.
