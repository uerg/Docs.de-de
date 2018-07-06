---
uid: single-page-application/overview/templates/emberjs-template
title: Ember.js-Vorlage | Microsoft-Dokumentation
author: xqiu
description: Ember.js-Vorlage
ms.author: aspnetcontent
ms.date: 01/30/2013
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 2488e9c10550bd9b11c675572c70618f6ca4ac05
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823776"
---
<a name="emberjs-template"></a>Ember.js-Vorlage
====================
durch [Xinyang Qiu](https://github.com/xqiu)

> Die ember.js-MVC-Vorlage wird von Nathan Totten, Thiago Santos und Xinyang Qiu geschrieben.
> 
> [Die ember.js-MVC-Vorlage herunterladen](https://go.microsoft.com/fwlink/?LinkId=282647)


Die ember.js-SPA-Vorlage dient Sie beim schnellen Erstellen von interaktiven clientseitigen Web-apps, die mithilfe der ember.js Einstieg.

"Single-Page Application" (SPA) ist der allgemeine Begriff für eine Webanwendung, die eine einzelne HTML-Seite lädt, und klicken Sie dann die Seite wird dynamisch aktualisiert, anstatt neue Seiten laden. Nach dem ersten Laden der Seite werden die SPA mit dem Server über AJAX-Anforderungen.

![](emberjs-template/_static/image1.png)

AJAX ist nichts neues, aber heute bestehen die JavaScript-Frameworks, die zum Erstellen und Verwalten einer großen anspruchsvollen SPA-Anwendung zu erleichtern. Darüber hinaus sind HTML5 und CSS3 jetzt zum Erstellen umfassender Benutzeroberflächen einfacher.

Die ember.js-SPA-Vorlage verwendet die [Ember](http://emberjs.com/) JavaScript-Bibliothek Seitenupdates von AJAX-Anforderungen zu verarbeiten. Ember.js verwendet Datenbindung, um die Seite mit den neuesten Daten zu synchronisieren. Auf diese Weise müssen Sie keinen Code schreiben, die erläutert, der JSON-Daten, und aktualisiert das DOM. Setzen Sie stattdessen deklarativen Attribute im HTML-Code, die Ember.js mitteilen, wie Sie die Daten zu präsentieren.

Klicken Sie auf der Serverseite der ember.js-Vorlage ist fast identisch mit der [KnockoutJS SPA-Vorlage](../introduction/knockoutjs-template.md). ASP.NET MVC verwendet, um HTML-Dokumente und ASP.NET Web-API zur Verarbeitung von AJAX-Anforderungen vom Client fungieren. Weitere Informationen zu diesen Aspekten der Vorlage finden Sie in der [Knockout.js-Vorlage](../introduction/knockoutjs-template.md) Dokumentation. Dieses Thema konzentriert sich auf die Unterschiede zwischen die Knockout-Vorlage und die ember.js-Vorlage.

## <a name="create-an-emberjs-spa-template-project"></a>Erstellen Sie ein Vorlagenprojekt für ember.js-SPA

Herunterladen Sie und installieren Sie die Vorlage, indem Sie auf die Schaltfläche "herunterladen" oben. Sie müssen möglicherweise Visual Studio neu starten.

In der **Vorlagen** wählen Sie im Bereich **installierte Vorlagen** und erweitern Sie die **Visual C#-** Knoten. Klicken Sie unter **Visual C#-** Option **Web**. Wählen Sie in der Liste der Projektvorlagen das Projekt **ASP.NET MVC 4-Webanwendung**. Nennen Sie das Projekt, und klicken Sie auf **OK**.

![](emberjs-template/_static/image2.png)

In der **neues Projekt** Assistenten **Ember.js-SPA-Projekt**.

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a>Übersicht über die ember.js SPA-Vorlage

Die ember.js-Vorlage verwendet eine Kombination von jQuery, Ember.js, Handlebars.js, um eine glatte, interaktive Benutzeroberfläche zu erstellen.

Ember.js ist eine JavaScript-Bibliothek, die ein Client-Side-MVC-Muster verwendet.

- Ein *Vorlage*, geschrieben in die Handlebars-Vorlagensprache beschreibt die Benutzeroberfläche der Anwendung. Im Releasemodus der [Handlebars-Compiler](https://github.com/Myslik/csharp-ember-handlebars) bündeln und Kompilieren die Handlebars-Vorlage verwendet wird.
- Ein *Modell* speichert die Anwendungsdaten, die sie auf dem Server (ToDo-Listen und ToDo-Elemente) abruft.
- Ein *Controller* speichert den Anwendungszustand. Domänencontroller stellen häufig Modellieren von Daten an den entsprechenden Vorlagen.
- Ein *Ansicht* primitiven Ereignisse aus der Anwendung übersetzt und übergibt diese an den Controller.
- Ein *Router* verwaltet Anwendungsstatus, Synchronisieren von URLs und Vorlagen.

Darüber hinaus kann die Ember-Data-Bibliothek verwendet werden, zum Synchronisieren von JSON-Objekte (abgerufen vom Server über eine RESTful-API) und die Client-Modelle.

Die ember.js-SPA-Vorlage werden die Skripts in acht Ebenen organisiert:

- Web-API\_adapter.js, Webapi\_serializer.js: erweitert die Ember datenbibliothek mit ASP.NET Web-API arbeiten.
- Scripts/helpers.js: Definiert die neue Ember Handlebars-Hilfsprogramme.
- Scripts/App.js: Die app erstellt und konfiguriert die Adapter und Serialisierungsprogramm.
- Skripts/Anwendungsmodelle/\*js: definiert die Modelle.
- Skripts/app/Views/\*js: werden die Ansichten definiert.
- Skripts/app/Controllern/\*js: die Controller definiert.
- Skripts/app/Routes "," Scripts/app/router.js ": Definiert die Routen.
- Vorlagen /\*.hbs: definiert die Handlebars-Vorlagen.

Sehen wir uns einige dieser Skripts genauer an.

## <a name="models"></a>Modelle

Die Modelle werden im Ordner "Skripts/app/Models" definiert. Es gibt zwei Dateien: "todoitem.js" und todoList.js.

**TODO.Model.js** definiert, die die clientseitige (Browser) Modelle für die to-do-Listen. Es gibt zwei Modellklassen: TodoItem und der "Todolist". In Ember ist Modelle eine Unterklasse der DS. Modell. Ein Modell kann es sich um Eigenschaften mit Attributen haben:

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

Modelle können Beziehungen zu anderen Modellen definieren:

[!code-css[Main](emberjs-template/samples/sample2.css)]

Modelle können Eigenschaften berechnet, die an anderen Eigenschaften binden:

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

Modelle können Observer-Funktionen verfügen, die aufgerufen werden, wenn eine beobachtete Eigenschaft ändert:

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a>Ansichten

Die Ansichten werden im Ordner "Skripts/app/Views" definiert. Eine Sicht verschiebt Ereignisse aus der Benutzeroberfläche der Anwendung. Ein Ereignishandler kann einen Rückruf an, die Controllerfunktionen, oder rufen Sie einfach den Datenkontext direkt.

Views/TodoItemEditView.js z. B. stammt der folgende Code. Ereignisbehandlung für ein Eingabetextfeld definiert.

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a>Controller

Die Controller werden im Ordner "Scripts/app/Controller" definiert. Um ein einzelnes Modell darstellen möchten, erweitern `Ember.ObjectController`:

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

Ein Controller kann auch eine Auflistung von Modellen durch die Erweiterung repräsentieren `Ember.ArrayController`. Die TodoListController stellt z. B. ein Array von `todoList` Objekte. Der Controller wird nach "Todolist"-ID, in absteigender Reihenfolge sortiert:

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

Der Controller definiert eine Funktion namens `addTodoList`, die eine neue "Todolist" erstellt und dem Array hinzugefügt. Um anzuzeigen, wie diese Funktion aufgerufen wird, öffnen Sie die Vorlagendatei, die mit dem Namen todoListTemplate.html, im Ordner "Templates". Die folgenden Vorlagencode bindet eine Schaltfläche, um die `addTodoList` Funktion:

[!code-html[Main](emberjs-template/samples/sample8.html)]

Der Controller enthält auch eine `error` -Eigenschaft, die Fehlermeldung enthält. Hier ist der Vorlagencode auf die Fehlermeldung (ebenfalls in todoListTemplate.html) anzuzeigen:

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a>Routen

Router.js definiert, die Routen und die Standardvorlage aus, um anzuzeigen, wird der Zustand der Anwendung, und entspricht URLs auf Routen:

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

TodoListRoute.js lädt Daten für die TodoListRoute durch Überschreiben der SetupController-Funktion:

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

Ember verwendet Benennungskonventionen, um URLs, Routennamen, Controller und Vorlagen zu entsprechen. Weitere Informationen finden Sie unter [ http://emberjs.com/guides/routing/defining-your-routes/ ](http://emberjs.com/guides/routing/defining-your-routes/) der ember.js-Dokumentation.

## <a name="templates"></a>Vorlagen

Ordner "Templates" enthält vier Vorlagen:

- Application.hbs: die Standardvorlage, die gerendert wird, wenn die Anwendung gestartet wird.
- About.hbs: die Vorlage für die Route "/ Info".
- Index.hbs: die Vorlage für den Stamm "/" weiterleiten.
- todoList.hbs: die Vorlage für die "/ Todo" weiterleiten.
- \_navbar.hbs: die Vorlage definiert, im Navigationsmenü.

Die Anwendungsvorlage verhält sich wie eine Masterseite. Sie enthält eine Kopfzeile, Fußzeile und ein "{{Outlet}}" zum Einfügen von anderen Vorlagen im abhängig von der Route. Weitere Informationen zu den Anwendungsvorlagen in Ember, finden Sie unter [ http://guides.emberjs.com/v1.10.0/templates/the-application-template// ](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).

Die "/ TodoList"-Vorlage enthält zwei schleifenausdrücke. Wird von die äußere Schleife `{{#each controller}}`, und der inneren Schleife `{{#each todos}}`. Der folgende Code zeigt eine integrierte `Ember.Checkbox` anzuzeigen, eine benutzerdefinierte `App.TodoItemEditView`, und eine Verbindung mit einem `deleteTodo` Aktion.

[!code-html[Main](emberjs-template/samples/sample12.html)]

Die `HtmlHelperExtensions` in Controllers/HtmlHelperExensions.cs, definierte Klasse definiert ein Hilfsprogramm-Funktion zum Zwischenspeichern und die Vorlage einfügen Dateien, wenn **Debuggen** nastaven NA hodnotu **"true"** in der Datei "Web.config". Diese Funktion wird von der ASP.NET MVC-Ansichtsdatei in Views/Home/App.cshtml definierten aufgerufen:

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

Die Funktion, die ohne Argumente aufgerufen, rendert alle die Vorlagendateien im Ordner "Templates". Sie können auch einen Unterordner oder eine spezifische Vorlagendatei angeben.

Wenn **Debuggen** ist **"false"** in "Web.config", die Anwendung das Bundle-Element "~/bundles/templates" enthält. Dieses Bundle-Element wird in BundleConfig.cs, verwenden die Handlebars-Compiler-Bibliothek hinzugefügt:

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
