---
uid: single-page-application/overview/templates/emberjs-template
title: EmberJS Vorlage | Microsoft Docs
author: xqiu
description: EmberJS-Vorlage
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 1fb7633aee288be648d4f9681b43c8911b7dbab9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="emberjs-template"></a>EmberJS-Vorlage
====================
durch [Xinyang Qiu](https://github.com/xqiu)

> Die EmberJS MVC-Projektvorlage wird von Nathan Totten, Thiago Santos und Xinyang Qiu geschrieben.
> 
> [Herunterladen der EmberJS MVC-Vorlage](https://go.microsoft.com/fwlink/?LinkId=282647)


Die Vorlage EmberJS SPA dient zum schnellen Erstellen von interaktiven clientseitigen webapps, die EmberJS mit Ihnen den Einstieg.

"Einzelseiten Application" (SPA) ist der allgemeine Begriff für eine Webanwendung, die einzelne HTML-Seite geladen und aktualisiert dann die Seite dynamisch, anstatt neue Seiten zu laden. Nach dem ersten Laden der Seite finden Sie die SPA mit dem Server über AJAX-Anforderungen.

![](emberjs-template/_static/image1.png)

AJAX ist ein bekanntes allerdings heute bestehen die JavaScript-Frameworks, die zum Erstellen und Verwalten einer großen anspruchsvollen SPA-Anwendung zu vereinfachen. Darüber hinaus machen HTML 5- und CSS3 sie umfangreiche Benutzeroberflächen erstellen einfacher.

Die EmberJS SPA-Vorlage verwendet die [Ember](http://emberjs.com/) JavaScript-Bibliothek, um die Seitenupdates von AJAX-Anforderungen verarbeiten. Ember.js verwendet Datenbindung, um die Seite mit den neuesten Daten zu synchronisieren. Auf diese Weise müssen Sie Code schreiben, führt Sie durch die JSON-Daten und aktualisiert das DOM validiert werden. Setzen Sie stattdessen deklarativen Attribute im HTML-Code, der Ember.js anweisen, wie die Daten zu präsentieren.

Auf dem Server, die Vorlage EmberJS ist fast identisch mit der [KnockoutJS SPA-Vorlage](../introduction/knockoutjs-template.md). ASP.NET-MVC verwendet, um HTML-Dokumente und ASP.NET Web-API, AJAX-Anforderungen vom Client behandeln dienen. Weitere Informationen zu diesen Aspekten der Vorlage finden Sie in der [KnockoutJS Vorlage](../introduction/knockoutjs-template.md) Dokumentation. Dieses Thema konzentriert sich auf die Unterschiede zwischen der Knockout-Vorlage und der EmberJS-Vorlage.

## <a name="create-an-emberjs-spa-template-project"></a>Erstellen Sie eine EmberJS SPA-Vorlagenprojekt

Herunterladen Sie und installieren Sie die Vorlage, indem Sie auf die Schaltfläche "herunterladen" oben. Sie müssen möglicherweise Visual Studio neu starten.

In der **Vorlagen** klicken Sie im Bereich **installierte Vorlagen** und erweitern Sie die **Visual C#-** Knoten. Klicken Sie unter **Visual C#-**Option **Web**. Wählen Sie in der Liste der Projektvorlagen **ASP.NET MVC 4-Webanwendung**. Nennen Sie das Projekt, und klicken Sie auf **OK**.

![](emberjs-template/_static/image2.png)

In der **neues Projekt** Assistenten **Ember.js SPA Projekt**.

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a>EmberJS SPA-Lizenzvorlage – Übersicht

Die Vorlage EmberJS verwendet eine Kombination von jQuery, Ember.js, Handlebars.js zum Erstellen einer smooth, interaktiven Benutzeroberflächenautomatisierungs.

Ember.js ist eine JavaScript-Bibliothek, die eine clientseitige MVC-Muster verwendet wird.

- Ein *Vorlage*, geschrieben in die Lenkern-Vorlagensprache beschreibt die Benutzeroberfläche der Anwendung. Im Releasemodus die [Lenkern Compiler](https://github.com/Myslik/csharp-ember-handlebars) bündeln und Kompilieren die Vorlage Lenkern verwendet wird.
- Ein *Modell* speichert die Anwendungsdaten, die sie aus dem Server (TODO-Listen und TODO-Elemente) abruft.
- Ein *Controller* Anwendungsstatus gespeichert. Controller darstellen häufig Modelldaten an den entsprechenden Vorlagen.
- Ein *Ansicht* übersetzt primitiven Ereignisse aus der Anwendung, und übergibt diese an den Controller.
- Ein *Router* verwaltet Anwendungsstatus, Synchronisieren von URLs und Vorlagen.

Darüber hinaus kann die Bibliothek Ember Daten zum Synchronisieren von JSON-Objekten, die (vom Server über eine REST-API abgerufen) und die Client-Modelle verwendet werden.

Die EmberJS SPA-Vorlage werden die Skripts in acht Ebenen organisiert:

- Webapi\_adapter.js, Webapi\_serializer.js: die Bibliothek Ember Daten zum Arbeiten mit ASP.NET Web-API erweitert.
- Scripts/helpers.js: Definiert die neue Ember Lenkern Hilfsprogramme.
- Scripts/App.js: Die app erstellt und konfiguriert die Adapter und Serialisierungsprogramm.
- Skripts/app/Models/\*js: definiert die Modelle.
- Skripts/app/Ansichten/\*js: die Ansichten definiert.
- Skripts/app/Controller/\*js: Controller definiert.
- Skripts/app/Routen, Scripts/app/router.js: Definiert die Routen.
- Vorlagen /\*.hbs: definiert die Lenkern Vorlagen.

Sehen wir uns einige dieser Skripts im Detail.

## <a name="models"></a>Modelle

Die Modelle werden im Ordner "Skripts/Anwendungsmodelle" definiert. Es gibt zwei Modelldateien: todoItem.js und todoList.js.

**TODO.Model.js** definiert die clientseitige (Browser) Modelle für die to-do-Listen. Es gibt zwei Modellklassen: TodoItem und "Todolist". In Ember sind Modelle Unterklassen des DS. Modell. Ein Modell kann Eigenschaften mit Attributen haben:

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

Modelle, um Beziehungen zu anderen Modellen zu definieren:

[!code-css[Main](emberjs-template/samples/sample2.css)]

Modelle können Eigenschaften berechnet, die an andere Eigenschaften gebunden:

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

Modelle können Observer-Funktionen verfügen, die aufgerufen werden, wenn eine beobachtete Eigenschaft ändert:

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a>Ansichten

Die Ansichten werden im Ordner "Skripts/app/Sichten" definiert. Eine Sicht übersetzt Ereignisse aus der Benutzeroberfläche der Anwendung. Ein Ereignishandler kann ein Rückruf für Controllerfunktionen, oder rufen Sie einfach den Datenkontext direkt.

Views/TodoItemEditView.js z. B. stammt der folgende Code. Ereignisbehandlung für ein Eingabetextfeld definiert.

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a>Controller

Die Controller werden im Ordner "Skripts/app/Controller" definiert. Erweitern, um ein einzelnes Modell darzustellen, `Ember.ObjectController`:

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

Ein Controller kann auch eine Auflistung von Modellen durch die Erweiterung repräsentieren `Ember.ArrayController`. Die TodoListController stellt z. B. ein Array von `todoList` Objekte. Der Controller wird nach "Todolist"-ID in absteigender Reihenfolge sortiert:

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

Der Controller definiert eine Funktion namens `addTodoList`, das erstellt eine neue "Todolist" und dem Array hinzugefügt. Um anzuzeigen, wie diese Funktion aufgerufen wird, öffnen Sie die Vorlagendatei, die mit dem Namen todoListTemplate.html, im Ordner "Vorlagen" ein. Die folgenden Vorlagencode bindet eine Schaltfläche, um die `addTodoList` Funktion:

[!code-html[Main](emberjs-template/samples/sample8.html)]

Der Controller enthält auch eine `error` -Eigenschaft, die eine Fehlermeldung enthält. Hier wird der Vorlagencode (ebenfalls in todoListTemplate.html) die Fehlermeldung angezeigt:

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a>Routen

Router.js definiert die Routen und die Standardvorlage richtet Zustand der Anwendung angezeigt werden, und entspricht von URLs zu Routen:

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

TodoListRoute.js lädt Daten für die TodoListRoute durch Überschreiben der SetupController-Funktion:

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

Ember verwendet Konventionen zur Namensgebung mit URLs, Routennamen, Controller und Vorlagen übereinstimmen. Weitere Informationen finden Sie unter [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) der EmberJS-Dokumentation.

## <a name="templates"></a>Vorlagen

Der Vorlagen-Ordner enthält vier Vorlagen:

- Application.hbs: die Standardvorlage aus, die gerendert wird, wenn die Anwendung gestartet wird.
- About.hbs: die Vorlage für die Route "/ Begriff".
- Index.hbs: die Vorlage für den Stammknoten "/" Route.
- todoList.hbs: die Vorlage für die "/ Todo" Route.
- \_navbar.hbs: die Vorlage definiert, im Navigationsmenü.

Die Anwendungsvorlage verhält sich wie eine Gestaltungsvorlage. Es enthält einen Header, Footer und eine "{{an}}" zum Einfügen von anderen Vorlagen in abhängig von der Route. Weitere Informationen zu Vorlagen in der Anwendung in Ember, finden Sie unter [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).

Die "/" Todolist "" Vorlage enthält zwei schleifenausdrücke. Die äußere Schleife ist `{{#each controller}}`, und der inneren Schleife ist `{{#each todos}}`. Der folgende Code zeigt eine integrierte `Ember.Checkbox` anzuzeigen, eine benutzerdefinierte `App.TodoItemEditView`, sowie einen Link mit einem `deleteTodo` Aktion.

[!code-html[Main](emberjs-template/samples/sample12.html)]

Die `HtmlHelperExtensions` in Controllers/HtmlHelperExensions.cs, definierte Klasse definiert eine Hilfsprogramm Funktion einfügen Vorlage das Zwischenspeichern von Dateien, wenn **Debuggen** festgelegt ist, um **"true"** in der Datei "Web.config". Diese Funktion wird aufgerufen, aus der ASP.NET MVC-Ansicht-Datei in Views/Home/App.cshtml definiert:

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

Die Funktion, die ohne Argumente aufgerufen wird, rendert aller der Dateien im Ordner "Vorlagen". Sie können auch einen Unterordner oder eine bestimmte Vorlagendatei angeben.

Wenn **Debuggen** ist **"false"** in "Web.config", die Anwendung das Bundle-Element "~/bundles/templates" enthält. Dieses Bundle-Element ist in BundleConfig.cs, mithilfe der Lenkern Compiler-Bibliothek hinzugefügt:

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
