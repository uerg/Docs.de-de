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
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 1fb7633aee288be648d4f9681b43c8911b7dbab9
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "26506799"
---
<a name="emberjs-template"></a><span data-ttu-id="ecb4e-103">EmberJS-Vorlage</span><span class="sxs-lookup"><span data-stu-id="ecb4e-103">EmberJS template</span></span>
====================
<span data-ttu-id="ecb4e-104">durch [Xinyang Qiu](https://github.com/xqiu)</span><span class="sxs-lookup"><span data-stu-id="ecb4e-104">by [Xinyang Qiu](https://github.com/xqiu)</span></span>

> <span data-ttu-id="ecb4e-105">Die EmberJS MVC-Projektvorlage wird von Nathan Totten, Thiago Santos und Xinyang Qiu geschrieben.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-105">The EmberJS MVC Template is written by Nathan Totten, Thiago Santos, and Xinyang Qiu.</span></span>
> 
> [<span data-ttu-id="ecb4e-106">Herunterladen der EmberJS MVC-Vorlage</span><span class="sxs-lookup"><span data-stu-id="ecb4e-106">Download the EmberJS MVC Template</span></span>](https://go.microsoft.com/fwlink/?LinkId=282647)


<span data-ttu-id="ecb4e-107">Die Vorlage EmberJS SPA dient zum schnellen Erstellen von interaktiven clientseitigen webapps, die EmberJS mit Ihnen den Einstieg.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-107">The EmberJS SPA template is designed to get you started quickly building interactive client-side web apps using EmberJS.</span></span>

<span data-ttu-id="ecb4e-108">"Einzelseiten Application" (SPA) ist der allgemeine Begriff für eine Webanwendung, die einzelne HTML-Seite geladen und aktualisiert dann die Seite dynamisch, anstatt neue Seiten zu laden.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-108">"Single-page application" (SPA) is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="ecb4e-109">Nach dem ersten Laden der Seite finden Sie die SPA mit dem Server über AJAX-Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-109">After the initial page load, the SPA talks with the server through AJAX requests.</span></span>

![](emberjs-template/_static/image1.png)

<span data-ttu-id="ecb4e-110">AJAX ist ein bekanntes allerdings heute bestehen die JavaScript-Frameworks, die zum Erstellen und Verwalten einer großen anspruchsvollen SPA-Anwendung zu vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-110">AJAX is nothing new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="ecb4e-111">Darüber hinaus machen HTML 5- und CSS3 sie umfangreiche Benutzeroberflächen erstellen einfacher.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-111">Also, HTML 5 and CSS3 are making it easier to create rich UIs.</span></span>

<span data-ttu-id="ecb4e-112">Die EmberJS SPA-Vorlage verwendet die [Ember](http://emberjs.com/) JavaScript-Bibliothek, um die Seitenupdates von AJAX-Anforderungen verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-112">The EmberJS SPA Template uses the [Ember](http://emberjs.com/) JavaScript library to handle page updates from AJAX requests.</span></span> <span data-ttu-id="ecb4e-113">Ember.js verwendet Datenbindung, um die Seite mit den neuesten Daten zu synchronisieren.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-113">Ember.js uses data binding to synchronize the page with the latest data.</span></span> <span data-ttu-id="ecb4e-114">Auf diese Weise müssen Sie Code schreiben, führt Sie durch die JSON-Daten und aktualisiert das DOM validiert werden.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-114">That way, you don't have to write any of the code that walks through the JSON data and updates the DOM.</span></span> <span data-ttu-id="ecb4e-115">Setzen Sie stattdessen deklarativen Attribute im HTML-Code, der Ember.js anweisen, wie die Daten zu präsentieren.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-115">Instead, you put declarative attributes in the HTML that tell Ember.js how to present the data.</span></span>

<span data-ttu-id="ecb4e-116">Auf dem Server, die Vorlage EmberJS ist fast identisch mit der [KnockoutJS SPA-Vorlage](../introduction/knockoutjs-template.md).</span><span class="sxs-lookup"><span data-stu-id="ecb4e-116">On the server side, the EmberJS template is almost identical to the [KnockoutJS SPA template](../introduction/knockoutjs-template.md).</span></span> <span data-ttu-id="ecb4e-117">ASP.NET-MVC verwendet, um HTML-Dokumente und ASP.NET Web-API, AJAX-Anforderungen vom Client behandeln dienen.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-117">It uses ASP.NET MVC to serve HTML documents, and ASP.NET Web API to handle AJAX requests from the client.</span></span> <span data-ttu-id="ecb4e-118">Weitere Informationen zu diesen Aspekten der Vorlage finden Sie in der [KnockoutJS Vorlage](../introduction/knockoutjs-template.md) Dokumentation.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-118">For more information about those aspects of the template, refer to the [KnockoutJS template](../introduction/knockoutjs-template.md) documentation.</span></span> <span data-ttu-id="ecb4e-119">Dieses Thema konzentriert sich auf die Unterschiede zwischen der Knockout-Vorlage und der EmberJS-Vorlage.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-119">This topic focuses on the differences between the Knockout template and the EmberJS template.</span></span>

## <a name="create-an-emberjs-spa-template-project"></a><span data-ttu-id="ecb4e-120">Erstellen Sie eine EmberJS SPA-Vorlagenprojekt</span><span class="sxs-lookup"><span data-stu-id="ecb4e-120">Create an EmberJS SPA Template Project</span></span>

<span data-ttu-id="ecb4e-121">Herunterladen Sie und installieren Sie die Vorlage, indem Sie auf die Schaltfläche "herunterladen" oben.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-121">Download and install the template by clicking the Download button above.</span></span> <span data-ttu-id="ecb4e-122">Sie müssen möglicherweise Visual Studio neu starten.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-122">You might need to restart Visual Studio.</span></span>

<span data-ttu-id="ecb4e-123">In der **Vorlagen** klicken Sie im Bereich **installierte Vorlagen** und erweitern Sie die **Visual C#-** Knoten.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-123">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="ecb4e-124">Klicken Sie unter **Visual C#-** Option **Web**.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-124">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="ecb4e-125">Wählen Sie in der Liste der Projektvorlagen **ASP.NET MVC 4-Webanwendung**.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-125">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="ecb4e-126">Nennen Sie das Projekt, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-126">Name the project and click **OK**.</span></span>

![](emberjs-template/_static/image2.png)

<span data-ttu-id="ecb4e-127">In der **neues Projekt** Assistenten **Ember.js SPA Projekt**.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-127">In the **New Project** wizard, select **Ember.js SPA Project**.</span></span>

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a><span data-ttu-id="ecb4e-128">EmberJS SPA-Lizenzvorlage – Übersicht</span><span class="sxs-lookup"><span data-stu-id="ecb4e-128">EmberJS SPA Template Overview</span></span>

<span data-ttu-id="ecb4e-129">Die Vorlage EmberJS verwendet eine Kombination von jQuery, Ember.js, Handlebars.js zum Erstellen einer smooth, interaktiven Benutzeroberflächenautomatisierungs.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-129">The EmberJS template uses a combination of jQuery, Ember.js, Handlebars.js to create a smooth, interactive UI.</span></span>

<span data-ttu-id="ecb4e-130">Ember.js ist eine JavaScript-Bibliothek, die eine clientseitige MVC-Muster verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-130">Ember.js is a JavaScript library that uses a client-side MVC pattern.</span></span>

- <span data-ttu-id="ecb4e-131">Ein *Vorlage*, geschrieben in die Lenkern-Vorlagensprache beschreibt die Benutzeroberfläche der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-131">A *template*, written in the Handlebars templating language, describes the application user interface.</span></span> <span data-ttu-id="ecb4e-132">Im Releasemodus die [Lenkern Compiler](https://github.com/Myslik/csharp-ember-handlebars) bündeln und Kompilieren die Vorlage Lenkern verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-132">In release mode, the [Handlebars compiler](https://github.com/Myslik/csharp-ember-handlebars) is used to bundle and compile the handlebars template.</span></span>
- <span data-ttu-id="ecb4e-133">Ein *Modell* speichert die Anwendungsdaten, die sie aus dem Server (TODO-Listen und TODO-Elemente) abruft.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-133">A *model* stores the application data that it gets from the server (ToDo lists and ToDo items).</span></span>
- <span data-ttu-id="ecb4e-134">Ein *Controller* Anwendungsstatus gespeichert.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-134">A *controller* stores application state.</span></span> <span data-ttu-id="ecb4e-135">Controller darstellen häufig Modelldaten an den entsprechenden Vorlagen.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-135">Controllers often present model data to the corresponding templates.</span></span>
- <span data-ttu-id="ecb4e-136">Ein *Ansicht* übersetzt primitiven Ereignisse aus der Anwendung, und übergibt diese an den Controller.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-136">A *view* translates primitive events from the application and passes these to the controller.</span></span>
- <span data-ttu-id="ecb4e-137">Ein *Router* verwaltet Anwendungsstatus, Synchronisieren von URLs und Vorlagen.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-137">A *router* manages application state, keeping URLs and templates in sync.</span></span>

<span data-ttu-id="ecb4e-138">Darüber hinaus kann die Bibliothek Ember Daten zum Synchronisieren von JSON-Objekten, die (vom Server über eine REST-API abgerufen) und die Client-Modelle verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-138">In addition, the Ember Data library can be used to synchronize JSON objects (obtained from the server through a RESTful API) and the client models.</span></span>

<span data-ttu-id="ecb4e-139">Die EmberJS SPA-Vorlage werden die Skripts in acht Ebenen organisiert:</span><span class="sxs-lookup"><span data-stu-id="ecb4e-139">The EmberJS SPA template organizes the scripts into eight layers:</span></span>

- <span data-ttu-id="ecb4e-140">Webapi\_adapter.js, Webapi\_serializer.js: die Bibliothek Ember Daten zum Arbeiten mit ASP.NET Web-API erweitert.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-140">webapi\_adapter.js, webapi\_serializer.js: Extends the Ember Data library to work with ASP.NET Web API.</span></span>
- <span data-ttu-id="ecb4e-141">Scripts/helpers.js: Definiert die neue Ember Lenkern Hilfsprogramme.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-141">Scripts/helpers.js: Defines new Ember Handlebars helpers.</span></span>
- <span data-ttu-id="ecb4e-142">Scripts/App.js: Die app erstellt und konfiguriert die Adapter und Serialisierungsprogramm.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-142">Scripts/app.js: Creates the app and configures the adapter and serializer.</span></span>
- <span data-ttu-id="ecb4e-143">Skripts/app/Models/\*js: definiert die Modelle.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-143">Scripts/app/models/\*.js: Defines the models.</span></span>
- <span data-ttu-id="ecb4e-144">Skripts/app/Ansichten/\*js: die Ansichten definiert.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-144">Scripts/app/views/\*.js: Defines the views.</span></span>
- <span data-ttu-id="ecb4e-145">Skripts/app/Controller/\*js: Controller definiert.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-145">Scripts/app/controllers/\*.js: Defines the controllers.</span></span>
- <span data-ttu-id="ecb4e-146">Skripts/app/Routen, Scripts/app/router.js: Definiert die Routen.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-146">Scripts/app/routes, Scripts/app/router.js: Defines the routes.</span></span>
- <span data-ttu-id="ecb4e-147">Vorlagen /\*.hbs: definiert die Lenkern Vorlagen.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-147">Templates/\*.hbs: Defines the handlebars templates.</span></span>

<span data-ttu-id="ecb4e-148">Sehen wir uns einige dieser Skripts im Detail.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-148">Let's look at some of these scripts in more detail.</span></span>

## <a name="models"></a><span data-ttu-id="ecb4e-149">Modelle</span><span class="sxs-lookup"><span data-stu-id="ecb4e-149">Models</span></span>

<span data-ttu-id="ecb4e-150">Die Modelle werden im Ordner "Skripts/Anwendungsmodelle" definiert.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-150">The models are defined in the Scripts/app/models folder.</span></span> <span data-ttu-id="ecb4e-151">Es gibt zwei Modelldateien: todoItem.js und todoList.js.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-151">There are two model files: todoItem.js and todoList.js.</span></span>

<span data-ttu-id="ecb4e-152">**TODO.Model.js** definiert die clientseitige (Browser) Modelle für die to-do-Listen.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-152">**todo.model.js** defines the client-side (browser) models for the to-do lists.</span></span> <span data-ttu-id="ecb4e-153">Es gibt zwei Modellklassen: TodoItem und "Todolist".</span><span class="sxs-lookup"><span data-stu-id="ecb4e-153">There are two model classes: todoItem and todoList.</span></span> <span data-ttu-id="ecb4e-154">In Ember sind Modelle Unterklassen des DS. Modell.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-154">In Ember, models are subclasses of DS.Model.</span></span> <span data-ttu-id="ecb4e-155">Ein Modell kann Eigenschaften mit Attributen haben:</span><span class="sxs-lookup"><span data-stu-id="ecb4e-155">A model can have properties with attributes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

<span data-ttu-id="ecb4e-156">Modelle, um Beziehungen zu anderen Modellen zu definieren:</span><span class="sxs-lookup"><span data-stu-id="ecb4e-156">Models can define relationships to other models:</span></span>

[!code-css[Main](emberjs-template/samples/sample2.css)]

<span data-ttu-id="ecb4e-157">Modelle können Eigenschaften berechnet, die an andere Eigenschaften gebunden:</span><span class="sxs-lookup"><span data-stu-id="ecb4e-157">Models can have computed properties that bind to other properties:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

<span data-ttu-id="ecb4e-158">Modelle können Observer-Funktionen verfügen, die aufgerufen werden, wenn eine beobachtete Eigenschaft ändert:</span><span class="sxs-lookup"><span data-stu-id="ecb4e-158">Models can have observer functions, which are invoked when an observed property changes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a><span data-ttu-id="ecb4e-159">Ansichten</span><span class="sxs-lookup"><span data-stu-id="ecb4e-159">Views</span></span>

<span data-ttu-id="ecb4e-160">Die Ansichten werden im Ordner "Skripts/app/Sichten" definiert.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-160">The views are defined in the Scripts/app/views folder.</span></span> <span data-ttu-id="ecb4e-161">Eine Sicht übersetzt Ereignisse aus der Benutzeroberfläche der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-161">A view translates events from the application UI.</span></span> <span data-ttu-id="ecb4e-162">Ein Ereignishandler kann ein Rückruf für Controllerfunktionen, oder rufen Sie einfach den Datenkontext direkt.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-162">An event handler can call back to controller functions, or simply call the data context directly.</span></span>

<span data-ttu-id="ecb4e-163">Views/TodoItemEditView.js z. B. stammt der folgende Code.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-163">For example, the following code is from views/TodoItemEditView.js.</span></span> <span data-ttu-id="ecb4e-164">Ereignisbehandlung für ein Eingabetextfeld definiert.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-164">It defines the event handling for an input text field.</span></span>

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a><span data-ttu-id="ecb4e-165">Controller</span><span class="sxs-lookup"><span data-stu-id="ecb4e-165">Controller</span></span>

<span data-ttu-id="ecb4e-166">Die Controller werden im Ordner "Skripts/app/Controller" definiert.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-166">The controllers are defined in the Scripts/app/controllers folder.</span></span> <span data-ttu-id="ecb4e-167">Erweitern, um ein einzelnes Modell darzustellen, `Ember.ObjectController`:</span><span class="sxs-lookup"><span data-stu-id="ecb4e-167">To represent a single model, extend `Ember.ObjectController`:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

<span data-ttu-id="ecb4e-168">Ein Controller kann auch eine Auflistung von Modellen durch die Erweiterung repräsentieren `Ember.ArrayController`.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-168">A controller can also represent a collection of models by extending `Ember.ArrayController`.</span></span> <span data-ttu-id="ecb4e-169">Die TodoListController stellt z. B. ein Array von `todoList` Objekte.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-169">For example, the TodoListController represents an array of `todoList` objects.</span></span> <span data-ttu-id="ecb4e-170">Der Controller wird nach "Todolist"-ID in absteigender Reihenfolge sortiert:</span><span class="sxs-lookup"><span data-stu-id="ecb4e-170">The controller sorts by todoList ID, in descending order:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

<span data-ttu-id="ecb4e-171">Der Controller definiert eine Funktion namens `addTodoList`, das erstellt eine neue "Todolist" und dem Array hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-171">The controller defines a function named `addTodoList`, which creates a new todoList and adds it to the array.</span></span> <span data-ttu-id="ecb4e-172">Um anzuzeigen, wie diese Funktion aufgerufen wird, öffnen Sie die Vorlagendatei, die mit dem Namen todoListTemplate.html, im Ordner "Vorlagen" ein.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-172">To see how this function gets called, open the template file named todoListTemplate.html, in the Templates folder.</span></span> <span data-ttu-id="ecb4e-173">Die folgenden Vorlagencode bindet eine Schaltfläche, um die `addTodoList` Funktion:</span><span class="sxs-lookup"><span data-stu-id="ecb4e-173">The following template code binds a button to the `addTodoList` function:</span></span>

[!code-html[Main](emberjs-template/samples/sample8.html)]

<span data-ttu-id="ecb4e-174">Der Controller enthält auch eine `error` -Eigenschaft, die eine Fehlermeldung enthält.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-174">The controller also contains an `error` property, which holds an error message.</span></span> <span data-ttu-id="ecb4e-175">Hier wird der Vorlagencode (ebenfalls in todoListTemplate.html) die Fehlermeldung angezeigt:</span><span class="sxs-lookup"><span data-stu-id="ecb4e-175">Here is the template code to display the error message (also in todoListTemplate.html):</span></span>

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a><span data-ttu-id="ecb4e-176">Routen</span><span class="sxs-lookup"><span data-stu-id="ecb4e-176">Routes</span></span>

<span data-ttu-id="ecb4e-177">Router.js definiert die Routen und die Standardvorlage richtet Zustand der Anwendung angezeigt werden, und entspricht von URLs zu Routen:</span><span class="sxs-lookup"><span data-stu-id="ecb4e-177">Router.js defines the routes and the default template to display, sets up application state, and matches URLs to routes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

<span data-ttu-id="ecb4e-178">TodoListRoute.js lädt Daten für die TodoListRoute durch Überschreiben der SetupController-Funktion:</span><span class="sxs-lookup"><span data-stu-id="ecb4e-178">TodoListRoute.js loads data for the TodoListRoute by overriding the setupController function:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

<span data-ttu-id="ecb4e-179">Ember verwendet Konventionen zur Namensgebung mit URLs, Routennamen, Controller und Vorlagen übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-179">Ember uses naming conventions to match URLs, route names, controllers, and templates.</span></span> <span data-ttu-id="ecb4e-180">Weitere Informationen finden Sie unter [ http://emberjs.com/guides/routing/defining-your-routes/ ](http://emberjs.com/guides/routing/defining-your-routes/) der EmberJS-Dokumentation.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-180">For more information, see [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) at the EmberJS documentation.</span></span>

## <a name="templates"></a><span data-ttu-id="ecb4e-181">Vorlagen</span><span class="sxs-lookup"><span data-stu-id="ecb4e-181">Templates</span></span>

<span data-ttu-id="ecb4e-182">Der Vorlagen-Ordner enthält vier Vorlagen:</span><span class="sxs-lookup"><span data-stu-id="ecb4e-182">The Templates folder contains four templates:</span></span>

- <span data-ttu-id="ecb4e-183">Application.hbs: die Standardvorlage aus, die gerendert wird, wenn die Anwendung gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-183">application.hbs: The default template that is rendered when the application is started.</span></span>
- <span data-ttu-id="ecb4e-184">About.hbs: die Vorlage für die Route "/ Begriff".</span><span class="sxs-lookup"><span data-stu-id="ecb4e-184">about.hbs: The template for the "/about" route.</span></span>
- <span data-ttu-id="ecb4e-185">Index.hbs: die Vorlage für den Stammknoten "/" Route.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-185">index.hbs: The template for the root "/" route.</span></span>
- <span data-ttu-id="ecb4e-186">todoList.hbs: die Vorlage für die "/ Todo" Route.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-186">todoList.hbs: The template for the "/todo" route.</span></span>
- <span data-ttu-id="ecb4e-187">\_navbar.hbs: die Vorlage definiert, im Navigationsmenü.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-187">\_navbar.hbs: The template defines the navigation menu.</span></span>

<span data-ttu-id="ecb4e-188">Die Anwendungsvorlage verhält sich wie eine Gestaltungsvorlage.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-188">The application template acts like a master page.</span></span> <span data-ttu-id="ecb4e-189">Es enthält einen Header, Footer und eine "{{an}}" zum Einfügen von anderen Vorlagen in abhängig von der Route.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-189">It contains a header, a footer, and an "{{outlet}}" to insert other templates in depending on the route.</span></span> <span data-ttu-id="ecb4e-190">Weitere Informationen zu Vorlagen in der Anwendung in Ember, finden Sie unter [ http://guides.emberjs.com/v1.10.0/templates/the-application-template// ](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).</span><span class="sxs-lookup"><span data-stu-id="ecb4e-190">For more information about application templates in Ember, see [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).</span></span>

<span data-ttu-id="ecb4e-191">Die "/" Todolist "" Vorlage enthält zwei schleifenausdrücke.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-191">The "/todoList" template contains two loop expressions.</span></span> <span data-ttu-id="ecb4e-192">Die äußere Schleife ist `{{#each controller}}`, und der inneren Schleife ist `{{#each todos}}`.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-192">The outside loop is `{{#each controller}}`, and the inside loop is `{{#each todos}}`.</span></span> <span data-ttu-id="ecb4e-193">Der folgende Code zeigt eine integrierte `Ember.Checkbox` anzuzeigen, eine benutzerdefinierte `App.TodoItemEditView`, sowie einen Link mit einem `deleteTodo` Aktion.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-193">The following code shows a built-in `Ember.Checkbox` view, a customized `App.TodoItemEditView`, and a link with a `deleteTodo` action.</span></span>

[!code-html[Main](emberjs-template/samples/sample12.html)]

<span data-ttu-id="ecb4e-194">Die `HtmlHelperExtensions` in Controllers/HtmlHelperExensions.cs, definierte Klasse definiert eine Hilfsprogramm Funktion einfügen Vorlage das Zwischenspeichern von Dateien, wenn **Debuggen** festgelegt ist, um **"true"** in der Datei "Web.config".</span><span class="sxs-lookup"><span data-stu-id="ecb4e-194">The `HtmlHelperExtensions` class, defined in Controllers/HtmlHelperExensions.cs, defines a helper function to cache and insert template files when **debug** is set to **true** in the Web.config file.</span></span> <span data-ttu-id="ecb4e-195">Diese Funktion wird aufgerufen, aus der ASP.NET MVC-Ansicht-Datei in Views/Home/App.cshtml definiert:</span><span class="sxs-lookup"><span data-stu-id="ecb4e-195">This function is called from the ASP.NET MVC view file defined in Views/Home/App.cshtml:</span></span>

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

<span data-ttu-id="ecb4e-196">Die Funktion, die ohne Argumente aufgerufen wird, rendert aller der Dateien im Ordner "Vorlagen".</span><span class="sxs-lookup"><span data-stu-id="ecb4e-196">Called with no arguments, the function renders all of the template files in the Templates folder.</span></span> <span data-ttu-id="ecb4e-197">Sie können auch einen Unterordner oder eine bestimmte Vorlagendatei angeben.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-197">You can also specify a subfolder or a specific template file.</span></span>

<span data-ttu-id="ecb4e-198">Wenn **Debuggen** ist **"false"** in "Web.config", die Anwendung das Bundle-Element "~/bundles/templates" enthält.</span><span class="sxs-lookup"><span data-stu-id="ecb4e-198">When **debug** is **false** in Web.config, the application includes the bundle item "~/bundles/templates".</span></span> <span data-ttu-id="ecb4e-199">Dieses Bundle-Element ist in BundleConfig.cs, mithilfe der Lenkern Compiler-Bibliothek hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="ecb4e-199">This bundle item is added in BundleConfig.cs, using the Handlebars compiler library:</span></span>

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
