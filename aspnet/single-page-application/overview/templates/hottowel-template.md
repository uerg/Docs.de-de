---
uid: single-page-application/overview/templates/hottowel-template
title: Im laufenden Systembetrieb Handtuch Vorlage | Microsoft Docs
author: madskristensen
description: HotTowel-Vorlage
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/09/2013
ms.topic: article
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: bfc6e2c884c422f44e8be5f4f29554ae86f7ecb6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="hot-towel-template"></a>Während des Betriebs Handtuch-Vorlage
====================
durch [Mads Kristensen](https://github.com/madskristensen)

> Die im laufenden Systembetrieb Handtuch MVC-Projektvorlage wird von John Papa geschrieben.
> 
> Wählen Sie, welche Version zum Herunterladen:
> 
> [Im laufenden Systembetrieb Handtuch MVC-Vorlage für Visual Studio 2012](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [Im laufenden Systembetrieb Handtuch MVC-Vorlage für Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)


> Im laufenden Systembetrieb Handtuch: Da Sie nicht, fahren Sie mit der SPA ohne eine möchten!


Eine SPA erstellen möchten, aber keine Entscheidung getroffen werden, wo Sie beginnen? Im laufenden Systembetrieb Handtuch verwenden und in Sekunden haben eine SPA und alle Tools, die zum Erstellen von darauf erforderlich!

Im laufenden Systembetrieb Handtuch erstellt einen guter Ausgangspunkt für die Erstellung einer einzelnen Seite Anwendung (SPA) mit ASP.NET. Direktes finden Sie es für Ihre Code, Ansicht Navigation, Datenbindung, umfangreiche datenverwaltung und einfache, aber elegante formatieren eine modulare Struktur. Im laufenden Systembetrieb Handtuch bietet alles, was Sie benötigen eine SPA erstellen, damit Sie auf der app, und nicht die zugrunde liegenden Funktionen konzentrieren können.

> Weitere Informationen zum Erstellen einer SPA aus [John Papa des Videos, Lernprogramme und Pluralsight Kurse](http://johnpapa.net/spa?vsix).


## <a name="application-structure"></a>Anwendungsstruktur

Im laufenden Systembetrieb Handtuch SPA bietet einen App-Ordner, der JavaScript und HTML-Dateien enthält, die in der Anwendung definiert.

Der Ordner "App":

- Durandal
- Dienste
- ViewModels
- Ansichten

Der App-Ordner enthält eine Auflistung von Modulen. Diese Module Funktionalität zu kapseln und Abhängigkeiten auf andere Module deklarieren. Der Ordner Views enthält den HTML-Code für Ihre Anwendung und der Viewmodels-Ordner enthält die Präsentationslogik für Ansichten (ein allgemeines Muster MVVM). Der Ordner "Dienste" ist ideal für gemeinsame Dienste, die Ihre Anwendung z. B. HTTP-Datenabruf oder lokalen Speicher Interaktion möglicherweise Gehäuse. Es ist häufig mehrere Viewmodels Code aus den Modulen Dienst erneut zu verwenden.

## <a name="aspnet-mvc-server-side-application-structure"></a>ASP.NET MVC Server Side Anwendungsstruktur

Im laufenden Systembetrieb Handtuch baut auf der vertraut und leistungsfähige ASP.NET MVC-Struktur.

- App\_starten
- Inhalt
- Domänencontroller
- Modelle
- Skripts
- Ansichten

## <a name="featured-libraries"></a>Top-Bibliotheken

- ASP.NET MVC
- ASP.NET-Web-API
- Optimierung der ASP.NET Web - Bündelung und Minimierung
- [Breeze.js](http://Breezejs.com) -rich datenverwaltung
- [Durandal.js](http://Durandaljs.com) -Navigation und Zusammenstellung
- [Knockout.js](http://Knockoutjs.com) -datenbindungen
- ["Require.js"](http://requirejs.org) -Modularität mit AMD und Optimierung
- [Toastr.js](http://jpapa.me/c7toastr) -Popup-Nachrichten
- [Twitter-Bootstrap](http://twitter.github.com/bootstrap/) - stabile CSS-Stilen

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a>Installation über die Visual Studio 2012 im laufenden Systembetrieb Handtuch SPA-Vorlage

Im laufenden Systembetrieb Handtuch kann als eine Vorlage für Visual Studio 2012 installiert werden. Klicken Sie einfach auf `File`  |  `New Project` , und wählen Sie `ASP.NET MVC 4 Web Application`. Wählen Sie dann die "Hot Handtuch einseitigen Anwendung" Vorlage und ausgeführt!

## <a name="installing-via-the-nuget-package"></a>Installation über das NuGet-Paket

Im laufenden Systembetrieb Handtuch ist auch ein NuGet-Paket, das ein vorhandenes leeres ASP.NET MVC-Projekt erhöht. Installieren Sie einfach mithilfe von Nuget, und führen Sie aus!

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a>Wie erstellen ich Hot Handtuch?

Hinzufügen von Code einfach zu starten!

1. Eigene serverseitige fügen Code hinzu, bevorzugt Entity Framework und WebAPI (die tatsächlich mit Breeze.js verwendet)
2. Hinzufügen von Ansichten der `App/views` Ordner
3. Viewmodels zum Hinzufügen der `App/viewmodels` Ordner
4. Hinzufügen von HTML und Knockout datenbindungen zu Ihren neuen Ansichten
5. Aktualisieren Sie die Navigation Routen`shell.js`

## <a name="walkthrough-of-the-htmljavascript"></a>Exemplarische Vorgehensweise für die HTML/JavaScript

### <a name="viewshottowelindexcshtml"></a>Views/HotTowel/index.cshtml

Index.cshtml ist das Route und die Ansicht für die MVC-Anwendung. Sie enthält alle standard Meta-Tags, Links Css und JavaScript-Verweise, die Sie erwarten würden. Der Nachrichtentext enthält ein einzelnes `<div>` ist, auf dem gesamten Inhalt (Ansichten) eingefügt wird, wenn sie angefordert werden. Die `@Scripts.Render` verwendet "Require.js" den Einstiegspunkt für den Anwendungscode ausführen im enthalten die `main.js` Datei. Ein Begrüßungsbildschirm wird bereitgestellt, um die veranschaulichen, wie Sie einen Begrüßungsbildschirm erstellen, während Ihre app geladen wird.

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a>App/Main.js

Die `main.js` Datei enthält den Code, der ausgeführt wird, sobald die app geladen wird. Dies ist, in denen Ihre Navigation Routen definieren, Festlegen Ihrer Startvorgangs Ansichten und führen Sie alle Setup/bootstrapping wie Ihre Anwendung Daten Einspielen werden sollen.

Die `main.js` -Datei definiert mehrere Durandal den Modulen der Anwendung Kick starten. Define-Anweisung kann die Module Abhängigkeiten zu beheben, damit sie für die Funktion verfügbar sind. Zuerst werden die debugging-Meldungen aktiviert, das Senden von Nachrichten über welche Ereignisse, dass die Anwendung an das Konsolenfenster ausführt. Der app.start-Code ermittelt Durandal-Framework, um die Anwendung zu starten. Die Konventionen sind festgelegt, sodass Durandal alle Ansichten kennt und Viewmodels bzw. in die gleichen benannten Ordner enthalten sind. Schließlich die `app.setRoot` zum Einsatz kommt lädt die `shell` mithilfe eines vordefinierten `entrance` Animation.

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a>Ansichten

Sichten befinden sich in der `App/views` Ordner.

### <a name="shellhtml"></a>Shell.HTML

Die `shell.html` master Layout für den HTML-Code enthält. Alle anderen Ansichten wird an einer beliebigen Stelle auf der Seite besteht aus Ihrer `shell` anzeigen. Im laufenden Systembetrieb Handtuch bietet eine `shell` mit drei Bereichen: einen Header, einen Inhaltsbereich und eine Fußzeile. Jede dieser Regionen mit geladen Inhalt zu anderen Ansichten für das angeforderte bilden.

Die `compose` Bindungen für die Kopf- und Fußzeile schwer hartcodiert sind in Hot Handtuch, zeigen Sie auf die `nav` und `footer` -Sicht bzw. Die Compose-Bindung für den Abschnitt `#content` gebunden ist, um die `router` aktive Element des Moduls. Das heißt, wenn Sie auf ein Navigationslink entsprechende Ansicht ist in diesem Bereich geladen wird.

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a>NAV.HTML

Die `nav.html` Navigationslinks für die SPA enthält. Dies ist, wo die Menüstruktur, z. B. platziert werden kann. Dies ist häufig Daten (mit Knockout) gebunden die `router` Modul, um die Navigation anzuzeigen Sie, in definiert der `shell.js`. Knockout sucht, die Data-Bind-Attribute und bindet diese an die `shell` Viewmodel die Navigation Routen anzeigen und eine "ProgressBar" (mit Twitter Bootstrap) anzuzeigen Wenn der `router` -Modul ist in der Mitte Navigieren von einer Sicht zu einem anderen (Siehe `router.isNavigating`).

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a>Datei "Home.HTML" und details.html

Diese Sichten enthalten HTML für benutzerdefinierte Ansichten. Wenn der `home` link der `nav` Menü "Ansicht des" geklickt wird, der `home` Sicht befinden sich im Bereich Inhalt des der `shell` anzeigen. Diese Sichten können erweitert oder durch Ihre eigenen benutzerdefinierten Ansichten ersetzt werden.

### <a name="footerhtml"></a>Footer.HTML

Die `footer.html` enthält HTML-Code, der in der Fußzeile am unteren Rand der `shell` anzeigen.

## <a name="viewmodels"></a>ViewModels

ViewModels befinden sich in der `App/viewmodels` Ordner.

### <a name="shelljs"></a>Shell.js

Die `shell` Viewmodel enthält Eigenschaften und Funktionen, die gebunden sind die `shell` anzeigen. Dies ist häufig, in denen die im Menü Navigation Bindungen gefunden werden (siehe die `router.mapNav` Logik).

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a>"Home.js" und details.js

Diese Viewmodels enthalten, die Eigenschaften und Funktionen, die gebunden sind die `home` anzeigen. Außerdem enthält die Präsentationslogik für die Ansicht und ist die Verbindung zwischen den Daten und die Ansicht.

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a>Dienste

Dienste befinden sich im Ordner "App-Dienste". Im Idealfall konnte Ihre zukünftige Dienste wie z. B. ein Modul "DataService", die für das Abrufen und Bereitstellen von Remotedaten zuständig ist, platziert werden.

### <a name="loggerjs"></a>Logger.js

Im laufenden Systembetrieb Handtuch bietet eine `logger` Modul im Ordner "Dienste". Die `logger` Modul eignet sich ideal zum Protokollieren von Nachrichten an die Konsole und in der Popup-Popups an den Benutzer.
