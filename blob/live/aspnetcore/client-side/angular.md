---
title: "Mithilfe von AngularJS für einseitige Anwendungen (SPAs)"
author: rick-anderson
description: Erfahren Sie, wie eine SPA-Stil ASP.NET-Anwendung mithilfe von AngularJS erstellen
keywords: ASP.NET Core AngularJS, SPA
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 4b30576b-2718-4c39-9253-a59966747893
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/angular
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ccdf1625cdaf2400780500ac5ab86f41537964a9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="using-angularjs-for-single-page-applications-spas-with-aspnet-core"></a><span data-ttu-id="669b3-104">Mithilfe von AngularJS für einseitige Anwendungen (SPAs) mit ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="669b3-104">Using AngularJS for Single Page Applications (SPAs) with ASP.NET Core</span></span>


<span data-ttu-id="669b3-105">Durch [Venkata Koppaka](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/) und [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="669b3-105">By [Venkata Koppaka](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="669b3-106">In diesem Artikel erfahren Sie, wie eine SPA-Stil ASP.NET-Anwendung mithilfe von AngularJS erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="669b3-106">In this article, you will learn how to build a SPA-style ASP.NET application using AngularJS.</span></span>

<span data-ttu-id="669b3-107">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="669b3-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-angularjs"></a><span data-ttu-id="669b3-108">Was ist AngularJS?</span><span class="sxs-lookup"><span data-stu-id="669b3-108">What is AngularJS?</span></span>

<span data-ttu-id="669b3-109">[AngularJS](https://angularjs.org/) einer modernen JavaScript-Frameworks von Google, die häufig zum Arbeiten mit Single Page Applications (SPAs) verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="669b3-109">[AngularJS](https://angularjs.org/) is a modern JavaScript framework from Google commonly used to work with Single Page Applications (SPAs).</span></span> <span data-ttu-id="669b3-110">AngularJS ist open Source unter MIT-Lizenz und den Fortschritt bei der Entwicklung von AngularJS auf gefolgt werden [des GitHub-Repositorys](https://github.com/angular/angular.js).</span><span class="sxs-lookup"><span data-stu-id="669b3-110">AngularJS is open sourced under MIT license, and the development progress of AngularJS can be followed on [its GitHub repository](https://github.com/angular/angular.js).</span></span> <span data-ttu-id="669b3-111">Die Bibliothek wird Angular aufgerufen, da HTML angular geformten Klammern verwendet.</span><span class="sxs-lookup"><span data-stu-id="669b3-111">The library is called Angular because HTML uses angular-shaped brackets.</span></span>

<span data-ttu-id="669b3-112">AngularJS ist eine DOM-Manipulation-Klassenbibliothek wie jQuery, aber es wird eine Teilmenge von jQuery aufgerufen jQLite verwendet.</span><span class="sxs-lookup"><span data-stu-id="669b3-112">AngularJS is not a DOM manipulation library like jQuery, but it uses a subset of jQuery called jQLite.</span></span> <span data-ttu-id="669b3-113">AngularJS basiert hauptsächlich auf deklarative HTML-Attribute, die Sie der HTML-Tags hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="669b3-113">AngularJS is primarily based on declarative HTML attributes that you can add to your HTML tags.</span></span> <span data-ttu-id="669b3-114">Versuchen Sie AngularJS in Ihrem Browser die [Code School Website](https://www.codeschool.com/courses/shaping-up-with-angularjs) oder [W3Schools Website](https://www.w3schools.com/angular/).</span><span class="sxs-lookup"><span data-stu-id="669b3-114">You can try AngularJS in your browser using the [Code School website](https://www.codeschool.com/courses/shaping-up-with-angularjs) or  [W3Schools website](https://www.w3schools.com/angular/).</span></span>

<span data-ttu-id="669b3-115">Dieser Artikel konzentriert sich auf AngularJS mit einige Hinweise auf, in dem Drehmoment Überschrift ist.</span><span class="sxs-lookup"><span data-stu-id="669b3-115">This article focuses on AngularJS with some notes on where Angular is heading.</span></span>

## <a name="getting-started"></a><span data-ttu-id="669b3-116">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="669b3-116">Getting started</span></span>

<span data-ttu-id="669b3-117">Um mithilfe von AngularJS in Ihrer ASP.NET-Anwendung zu starten, müssen Sie die Installation als Teil Ihres Projekts, oder verweisen sie über ein Netzwerk für Inhaltsübermittlung (CDN) aus.</span><span class="sxs-lookup"><span data-stu-id="669b3-117">To start using AngularJS in your ASP.NET application, you must either install it as part of your project, or reference it from a content delivery network (CDN).</span></span>

### <a name="installation"></a><span data-ttu-id="669b3-118">Installation</span><span class="sxs-lookup"><span data-stu-id="669b3-118">Installation</span></span>

<span data-ttu-id="669b3-119">Es gibt verschiedene Möglichkeiten zum Hinzufügen von AngularJS an Ihre Anwendung ein.</span><span class="sxs-lookup"><span data-stu-id="669b3-119">There are several ways to add AngularJS to your application.</span></span> <span data-ttu-id="669b3-120">Wenn Sie eine neue ASP.NET Core-Webanwendung in Visual Studio starten, können Sie unter Verwendung der integrierten AngularJS hinzufügen [Bower](bower.md) unterstützen.</span><span class="sxs-lookup"><span data-stu-id="669b3-120">If you’re starting a new ASP.NET Core web application in Visual Studio, you can add AngularJS using the built-in [Bower](bower.md) support.</span></span> <span data-ttu-id="669b3-121">Open *"bower.JSON"*, und fügen Sie einen Eintrag, um die `dependencies` Eigenschaft:</span><span class="sxs-lookup"><span data-stu-id="669b3-121">Open *bower.json*, and add an entry to the `dependencies` property:</span></span>

<a name="angular-bower-json"></a>

[!code-json[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/bower.json?highlight=9)]

<span data-ttu-id="669b3-122">Beim Speichern der *"bower.JSON"* Angular-Datei in Ihrem Projekts installiert *"Wwwroot" / Lib* Ordner.</span><span class="sxs-lookup"><span data-stu-id="669b3-122">Upon saving the *bower.json* file, Angular will be installed in your project's *wwwroot/lib* folder.</span></span> <span data-ttu-id="669b3-123">Darüber hinaus wird es aufgeführt innerhalb der `Dependencies/Bower` Ordner.</span><span class="sxs-lookup"><span data-stu-id="669b3-123">Additionally, it will be listed within the `Dependencies/Bower` folder.</span></span> <span data-ttu-id="669b3-124">Siehe folgender Screenshot.</span><span class="sxs-lookup"><span data-stu-id="669b3-124">See the screenshot below.</span></span>

![Projektmappen-Explorer mit der AngularJS-Projekt](angular/_static/angular-solution-explorer.png)

<span data-ttu-id="669b3-126">Als Nächstes fügen Sie eine `<script>` Verweis auf das Ende der `<body>` Teil der HTML-Seite oder *_Layout.cshtml* Datei, wie hier gezeigt:</span><span class="sxs-lookup"><span data-stu-id="669b3-126">Next, add a `<script>` reference to the bottom of the `<body>` section of your HTML page or *_Layout.cshtml* file, as shown here:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Shared/_Layout.cshtml?highlight=4&range=48-52)]

<span data-ttu-id="669b3-127">Es wird empfohlen, dass Produktionsanwendungen CDNs für allgemeine Bibliotheken wie AngularJS verwenden.</span><span class="sxs-lookup"><span data-stu-id="669b3-127">It's recommended that production applications utilize CDNs for common libraries like AngularJS.</span></span> <span data-ttu-id="669b3-128">Sie können aus einer von mehreren CDNs, wie diese AngularJS verweisen:</span><span class="sxs-lookup"><span data-stu-id="669b3-128">You can reference AngularJS from one of several CDNs, such as this one:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Shared/_Layout.cshtml?highlight=10&range=53-67)]

<span data-ttu-id="669b3-129">Nachdem Sie einen Verweis auf haben die *angular.js* Skriptdatei, jetzt können Sie mithilfe von AngularJS in Webseiten zu beginnen.</span><span class="sxs-lookup"><span data-stu-id="669b3-129">Once you have a reference to the *angular.js* script file, you're ready to begin using AngularJS in your web pages.</span></span>

## <a name="key-components"></a><span data-ttu-id="669b3-130">Wichtige Komponenten</span><span class="sxs-lookup"><span data-stu-id="669b3-130">Key components</span></span>

<span data-ttu-id="669b3-131">AngularJS umfasst eine Reihe von wesentlichen Komponenten, wie z. B. *Direktiven*, *Vorlagen*, *Repeater*, *Module*,  *Controller*, *Komponenten*, *Komponente Router* und vieles mehr.</span><span class="sxs-lookup"><span data-stu-id="669b3-131">AngularJS includes a number of major components, such as *directives*, *templates*, *repeaters*, *modules*, *controllers*, *components*, *component router* and more.</span></span> <span data-ttu-id="669b3-132">Betrachten wir nun, wie diese Komponenten zusammenarbeiten, um Webseiten Verhalten hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="669b3-132">Let's examine how these components work together to add behavior to your web pages.</span></span>

### <a name="directives"></a><span data-ttu-id="669b3-133">Anweisungen</span><span class="sxs-lookup"><span data-stu-id="669b3-133">Directives</span></span>

<span data-ttu-id="669b3-134">Mithilfe von AngularJS [Direktiven](https://docs.angularjs.org/guide/directive) HTML mit benutzerdefinierten Attributen und Elementen erweitern.</span><span class="sxs-lookup"><span data-stu-id="669b3-134">AngularJS uses [directives](https://docs.angularjs.org/guide/directive) to extend HTML with custom attributes and elements.</span></span> <span data-ttu-id="669b3-135">AngularJS-Direktiven werden definiert über `data-ng-*` oder `ng-*` Präfixe (`ng` ist die Kurzform für winkelförmig).</span><span class="sxs-lookup"><span data-stu-id="669b3-135">AngularJS directives are defined via `data-ng-*` or `ng-*` prefixes (`ng` is short for angular).</span></span> <span data-ttu-id="669b3-136">Es gibt zwei Arten von AngularJS-Direktiven:</span><span class="sxs-lookup"><span data-stu-id="669b3-136">There are two types of AngularJS directives:</span></span>

   1. <span data-ttu-id="669b3-137">**Primitive Direktiven**: Diese werden vom Team Angular vordefiniert und sind Teil des AngularJS-Framework.</span><span class="sxs-lookup"><span data-stu-id="669b3-137">**Primitive Directives**: These are predefined by the Angular team and are part of the AngularJS framework.</span></span>

   2. <span data-ttu-id="669b3-138">**Benutzerdefinierte Direktiven**: Hierbei handelt es sich um benutzerdefinierte Direktiven, die Sie definieren können.</span><span class="sxs-lookup"><span data-stu-id="669b3-138">**Custom Directives**: These are custom directives that you can define.</span></span>

<span data-ttu-id="669b3-139">Der primitiven Direktiven in allen AngularJS-Anwendungen verwendet wird die `ng-app` -Direktive, die die AngularJS-Anwendung gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="669b3-139">One of the primitive directives used in all AngularJS applications is the `ng-app` directive, which bootstraps the AngularJS application.</span></span> <span data-ttu-id="669b3-140">Diese Direktive kann angewendet werden, um die `<body>` Tag oder ein untergeordnetes Element des Texts.</span><span class="sxs-lookup"><span data-stu-id="669b3-140">This directive can be applied to the `<body>` tag or to a child element of the body.</span></span> <span data-ttu-id="669b3-141">Sehen Sie ein Beispiel, in Aktion an.</span><span class="sxs-lookup"><span data-stu-id="669b3-141">Let's see an example in action.</span></span> <span data-ttu-id="669b3-142">Angenommen, Sie sind in einem ASP.NET-Projekt, können Sie entweder hinzufügen eine HTML-Datei, um die `wwwroot` Ordner, oder fügen Sie eine neue Controlleraktion und eine zugeordnete Ansicht.</span><span class="sxs-lookup"><span data-stu-id="669b3-142">Assuming you're in an ASP.NET project, you can either add an HTML file to the `wwwroot` folder, or add a new controller action and an associated view.</span></span> <span data-ttu-id="669b3-143">In diesem Fall habe ich ein neues hinzugefügt `Directives` Aktionsmethode `HomeController.cs`.</span><span class="sxs-lookup"><span data-stu-id="669b3-143">In this case, I've added a new `Directives` action method to `HomeController.cs`.</span></span> <span data-ttu-id="669b3-144">Die zugeordnete Ansicht wird hier gezeigt:</span><span class="sxs-lookup"><span data-stu-id="669b3-144">The associated view is shown here:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Directives.cshtml?highlight=5,7)]

<span data-ttu-id="669b3-145">Um diese Beispiele sind voneinander unabhängig zu halten, verwende ich nicht freigegebenen Layoutdatei.</span><span class="sxs-lookup"><span data-stu-id="669b3-145">To keep these samples independent of one another, I'm not using the shared layout file.</span></span> <span data-ttu-id="669b3-146">Beachten Sie, dass wir das Body-Tag mit ergänzt die `ng-app` Richtlinie an, dass diese Seite ist eine AngularJS-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="669b3-146">You can see that we decorated the body tag with the `ng-app` directive to indicate this page is an AngularJS application.</span></span> <span data-ttu-id="669b3-147">Die `{{2+2}}` ist ein Angular Datenbindungsausdruck, die Sie in wenigen Augenblicken mehr zu lernen soll.</span><span class="sxs-lookup"><span data-stu-id="669b3-147">The `{{2+2}}` is an Angular data binding expression that you will learn more about in a moment.</span></span> <span data-ttu-id="669b3-148">Hier ist das Ergebnis auf, wenn Sie diese Anwendung auszuführen:</span><span class="sxs-lookup"><span data-stu-id="669b3-148">Here is the result if you run this application:</span></span>

![Einfache Angular-Direktive](angular/_static/simple-directive.png)

<span data-ttu-id="669b3-150">Andere primitiven Direktiven in AngularJS gehören:</span><span class="sxs-lookup"><span data-stu-id="669b3-150">Other primitive directives in AngularJS include:</span></span>

<span data-ttu-id="669b3-151">`ng-controller`Bestimmt, welcher Controller JavaScript an die Sicht gebunden ist.</span><span class="sxs-lookup"><span data-stu-id="669b3-151">`ng-controller` Determines which JavaScript controller is bound to which view.</span></span>

<span data-ttu-id="669b3-152">`ng-model`Bestimmt das Modell, zu dem die Werte der Eigenschaften eines HTML-Elements gebunden sind.</span><span class="sxs-lookup"><span data-stu-id="669b3-152">`ng-model` Determines the model to which the values of an HTML element's properties are bound.</span></span>

<span data-ttu-id="669b3-153">`ng-init`Wird verwendet, um die Anwendungsdaten in Form eines Ausdrucks für den aktuellen Bereich zu initialisieren.</span><span class="sxs-lookup"><span data-stu-id="669b3-153">`ng-init` Used to initialize the application data in the form of an expression for the current scope.</span></span>

<span data-ttu-id="669b3-154">`ng-if`Entfernt oder neu erstellt das angegebene HTML-Element im DOM basierend auf der Truthiness des Ausdrucks angegeben.</span><span class="sxs-lookup"><span data-stu-id="669b3-154">`ng-if` Removes or recreates the given HTML element in the DOM based on the truthiness of the expression provided.</span></span>

<span data-ttu-id="669b3-155">`ng-repeat`Wiederholt einen bestimmten Codeblock HTML für eine Menge von Daten an.</span><span class="sxs-lookup"><span data-stu-id="669b3-155">`ng-repeat` Repeats a given block of HTML over a set of data.</span></span>

<span data-ttu-id="669b3-156">`ng-show`Zeigt an, oder blendet Sie aus dem angegebenen HTML-Element, das basierend auf den Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="669b3-156">`ng-show` Shows or hides the given HTML element based on the expression provided.</span></span>

<span data-ttu-id="669b3-157">Eine vollständige Liste der alle primitiven Direktiven in AngularJS unterstützt, finden Sie auf der [Abschnitt "Richtlinie-Dokumentation" auf die AngularJS-Dokumentationswebsite](https://docs.angularjs.org/api/ng/directive).</span><span class="sxs-lookup"><span data-stu-id="669b3-157">For a full list of all primitive directives supported in AngularJS, please refer to the [directive documentation section on the AngularJS documentation website](https://docs.angularjs.org/api/ng/directive).</span></span>

### <a name="data-binding"></a><span data-ttu-id="669b3-158">Datenbindung</span><span class="sxs-lookup"><span data-stu-id="669b3-158">Data binding</span></span>

<span data-ttu-id="669b3-159">AngularJS bietet [Datenbindung](https://docs.angularjs.org/guide/databinding) Out-of-the-Box entweder unterstützen die `ng-bind` -Direktive oder ein Bindung Ausdruckssyntax, z. B. `{{expression}}`.</span><span class="sxs-lookup"><span data-stu-id="669b3-159">AngularJS provides [data binding](https://docs.angularjs.org/guide/databinding) support out-of-the-box using either the `ng-bind` directive or a data binding expression syntax such as `{{expression}}`.</span></span> <span data-ttu-id="669b3-160">AngularJS unterstützt bidirektionale Datenbindung, die bei der Daten aus einem Modell in die Synchronisierung mit einer Ansichtenvorlage jederzeit aufbewahrt wird.</span><span class="sxs-lookup"><span data-stu-id="669b3-160">AngularJS supports two-way data binding where data from a model is kept in synchronization with a view template at all times.</span></span> <span data-ttu-id="669b3-161">Alle Änderungen an der Ansicht sind automatisch im Modell widerspiegeln.</span><span class="sxs-lookup"><span data-stu-id="669b3-161">Any changes to the view are automatically reflected in the model.</span></span> <span data-ttu-id="669b3-162">Ebenso sind alle Änderungen im Modell in der Sicht widergespiegelt.</span><span class="sxs-lookup"><span data-stu-id="669b3-162">Likewise, any changes in the model are reflected in the view.</span></span>

<span data-ttu-id="669b3-163">Erstellen Sie eine HTML-Datei oder eine Controlleraktion mit einer zugehörigen Ansicht mit dem Namen `Databinding`.</span><span class="sxs-lookup"><span data-stu-id="669b3-163">Create either an HTML file or a controller action with an accompanying view named `Databinding`.</span></span> <span data-ttu-id="669b3-164">In der Ansicht gehören:</span><span class="sxs-lookup"><span data-stu-id="669b3-164">Include the following in the view:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Databinding.cshtml?highlight=8,9,10)]

<span data-ttu-id="669b3-165">Beachten Sie, dass Sie mithilfe von Direktiven oder Daten Bindung Modellwerte anzeigen können (`ng-bind`).</span><span class="sxs-lookup"><span data-stu-id="669b3-165">Notice that you can display model values using either directives or data binding (`ng-bind`).</span></span> <span data-ttu-id="669b3-166">Ergebnisseite sollte wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="669b3-166">The resulting page should look like this:</span></span>

![Einfache Datenbindung](angular/_static/simple-databinding.png)

### <a name="templates"></a><span data-ttu-id="669b3-168">Vorlagen</span><span class="sxs-lookup"><span data-stu-id="669b3-168">Templates</span></span>

<span data-ttu-id="669b3-169">[Vorlagen](https://docs.angularjs.org/guide/templates) in AngularJS werden nur einfache HTML-Seiten ergänzt, mit der AngularJS-Direktiven und Elemente.</span><span class="sxs-lookup"><span data-stu-id="669b3-169">[Templates](https://docs.angularjs.org/guide/templates) in AngularJS are just plain HTML pages decorated with AngularJS directives and artifacts.</span></span> <span data-ttu-id="669b3-170">Eine AngularJS-Vorlage ist eine Mischung von Anweisungen, Ausdrücke, Filter und Steuerelemente, die mit HTML, um die Sicht bilden kombiniert werden soll.</span><span class="sxs-lookup"><span data-stu-id="669b3-170">A template in AngularJS is a mixture of directives, expressions, filters, and controls that combine with HTML to form the view.</span></span>

<span data-ttu-id="669b3-171">Fügen Sie einer anderen Ansicht, um Vorlagen zu veranschaulichen, und fügen Sie Folgendes hinzu:</span><span class="sxs-lookup"><span data-stu-id="669b3-171">Add another view to demonstrate templates, and add the following to it:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Templates.cshtml?highlight=8,9,10)]

<span data-ttu-id="669b3-172">Die Vorlage hat AngularJS-Direktiven wie `ng-app`, `ng-init`, `ng-model` und Syntax von Datenbindungsausdrücken zum Binden der `personName` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="669b3-172">The template has AngularJS directives like `ng-app`, `ng-init`, `ng-model` and data binding expression syntax to bind the `personName` property.</span></span> <span data-ttu-id="669b3-173">In den Browser ausgeführt wird, sieht die Ansicht, wie der folgende Screenshot:</span><span class="sxs-lookup"><span data-stu-id="669b3-173">Running in the browser, the view looks like the screenshot below:</span></span>

![Einfaches Beispiel für Vorlagen 1](angular/_static/simple-templates-1.png)

<span data-ttu-id="669b3-175">Wenn Sie den Namen ändern, indem Sie in das Eingabefeld eingeben, sehen Sie den Text neben dem Eingabefeld dynamisch aktualisieren, Anzeigen von Angular bidirektionale Datenbindung in Aktion.</span><span class="sxs-lookup"><span data-stu-id="669b3-175">If you change the name by typing in the input field, you will see the text next to the input field dynamically update, showing Angular two-way data binding in action.</span></span>

![Einfaches Beispiel für Vorlagen 2](angular/_static/simple-templates-2.png)

### <a name="expressions"></a><span data-ttu-id="669b3-177">Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="669b3-177">Expressions</span></span>

<span data-ttu-id="669b3-178">[Ausdrücke](https://docs.angularjs.org/guide/expression) AngularJS sind JavaScript-ähnliches Codeausschnitte, die geschrieben werden, in der `{{ expression }}` Syntax.</span><span class="sxs-lookup"><span data-stu-id="669b3-178">[Expressions](https://docs.angularjs.org/guide/expression) in AngularJS are JavaScript-like code snippets that are written inside the `{{ expression }}` syntax.</span></span> <span data-ttu-id="669b3-179">Die gleiche Weise wie die Daten aus diesen Ausdrücken in HTML gebunden `ng-bind` Direktiven.</span><span class="sxs-lookup"><span data-stu-id="669b3-179">The data from these expressions is bound to HTML the same way as `ng-bind` directives.</span></span> <span data-ttu-id="669b3-180">Der Hauptunterschied zwischen AngularJS-Ausdrücke und reguläre Ausdrücke von JavaScript wird diese AngularJS-Ausdrücke, für ausgewertet werden die `$scope` Objekt im AngularJS.</span><span class="sxs-lookup"><span data-stu-id="669b3-180">The main difference between AngularJS expressions and regular JavaScript expressions is that AngularJS expressions are evaluated against the `$scope` object in AngularJS.</span></span>

<span data-ttu-id="669b3-181">Die AngularJS-Ausdrücke im Beispiel unten Bind `personName` und eine einfache JavaScript berechneten Ausdruck:</span><span class="sxs-lookup"><span data-stu-id="669b3-181">The AngularJS expressions in the sample below bind `personName` and a simple JavaScript calculated expression:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Expressions.cshtml?highlight=8,9,10)]

<span data-ttu-id="669b3-182">Im Beispiel wird der Browser zeigt auf die `personName` Daten und die Ergebnisse der Berechnung:</span><span class="sxs-lookup"><span data-stu-id="669b3-182">The example running in the browser displays the `personName` data and the results of the calculation:</span></span>

![Einfache Ausdrücke](angular/_static/simple-expressions.png)

### <a name="repeaters"></a><span data-ttu-id="669b3-184">Repeater</span><span class="sxs-lookup"><span data-stu-id="669b3-184">Repeaters</span></span>

<span data-ttu-id="669b3-185">Wiederholte in AngularJS erfolgt über einen primitiven Richtlinie aufgerufen `ng-repeat`.</span><span class="sxs-lookup"><span data-stu-id="669b3-185">Repeating in AngularJS is done via a primitive directive called `ng-repeat`.</span></span> <span data-ttu-id="669b3-186">Die `ng-repeat` Richtlinie wiederholt ein angegebenes HTML-Element in einer Ansicht für die Länge eines Arrays aus sich wiederholenden Daten.</span><span class="sxs-lookup"><span data-stu-id="669b3-186">The `ng-repeat` directive repeats a given HTML element in a view over the length of a repeating data array.</span></span> <span data-ttu-id="669b3-187">Repeater in AngularJS können über ein Array von Zeichenfolgen oder Objekte wiederholen.</span><span class="sxs-lookup"><span data-stu-id="669b3-187">Repeaters in AngularJS can repeat over an array of strings or objects.</span></span> <span data-ttu-id="669b3-188">Hier ist eine Beispiel für die Verwendung von wiederholten über ein Array von Zeichenfolgen:</span><span class="sxs-lookup"><span data-stu-id="669b3-188">Here is a sample usage of repeating over an array of strings:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters.cshtml?highlight=8,10,11)]

<span data-ttu-id="669b3-189">Die [repeat-Direktive](https://docs.angularjs.org/api/ng/directive/ngRepeat) gibt eine Reihe von Elementen in eine ungeordnete Liste aus, wie Sie in den Entwicklertools, die in diesem Screenshot gezeigten sehen können:</span><span class="sxs-lookup"><span data-stu-id="669b3-189">The [repeat directive](https://docs.angularjs.org/api/ng/directive/ngRepeat) outputs a series of list items in an unordered list, as you can see in the developer tools shown in this screenshot:</span></span>

![Repeater-Beispiel](angular/_static/repeater.png)

<span data-ttu-id="669b3-191">Hier ist ein Beispiel, das über ein Array von Objekten wird wiederholt.</span><span class="sxs-lookup"><span data-stu-id="669b3-191">Here is an example that repeats over an array of objects.</span></span> <span data-ttu-id="669b3-192">Die `ng-init` Richtlinie richtet eine `names` Array, wobei jedes Element ist ein Objekt, das zuerst enthält und Nachnamen.</span><span class="sxs-lookup"><span data-stu-id="669b3-192">The `ng-init` directive establishes a `names` array, where each element is an object containing first and last names.</span></span> <span data-ttu-id="669b3-193">Die `ng-repeat` Zuweisung `name in names`, gibt Sie ein Listenelement für jedes Arrayelement.</span><span class="sxs-lookup"><span data-stu-id="669b3-193">The `ng-repeat` assignment, `name in names`, outputs a list item for every array element.</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters2.cshtml?highlight=8,9,10,11,13,14)]

<span data-ttu-id="669b3-194">Die Ausgabe wird in diesem Fall wie im vorherigen Beispiel identisch.</span><span class="sxs-lookup"><span data-stu-id="669b3-194">The output in this case is the same as in the previous example.</span></span>

<span data-ttu-id="669b3-195">Angular bietet einige zusätzlichen Richtlinien, die helfen können Verhalten, je nachdem, in dem die Schleife in der Ausführung ist.</span><span class="sxs-lookup"><span data-stu-id="669b3-195">Angular provides some additional directives that can help provide behavior based on where the loop is in its execution.</span></span>

`$index`

<span data-ttu-id="669b3-196">Verwendung `$index` in die `ng-repeat` Schleife, um zu bestimmen, welche Indexposition die Schleife aktuell ist.</span><span class="sxs-lookup"><span data-stu-id="669b3-196">Use `$index` in the `ng-repeat` loop to determine which index position your loop currently is on.</span></span>

<span data-ttu-id="669b3-197">`$even` und `$odd`</span><span class="sxs-lookup"><span data-stu-id="669b3-197">`$even` and `$odd`</span></span>

<span data-ttu-id="669b3-198">Verwendung `$even` in die `ng-repeat` Schleife, um zu bestimmen, ob der aktuelle Index in der Schleife eine sogar indizierten Zeile ist.</span><span class="sxs-lookup"><span data-stu-id="669b3-198">Use `$even` in the `ng-repeat` loop to determine whether the current index in your loop is an even indexed row.</span></span> <span data-ttu-id="669b3-199">Auf ähnliche Weise verwenden `$odd` zu bestimmen, ob der aktuelle Index eine ungerade indizierten Zeile ist.</span><span class="sxs-lookup"><span data-stu-id="669b3-199">Similarly, use `$odd` to determine if the current index is an odd indexed row.</span></span>

<span data-ttu-id="669b3-200">`$first` und `$last`</span><span class="sxs-lookup"><span data-stu-id="669b3-200">`$first` and `$last`</span></span>

<span data-ttu-id="669b3-201">Verwendung `$first` in die `ng-repeat` Schleife, um zu bestimmen, ob der aktuelle Index in der Schleife die erste Zeile ist.</span><span class="sxs-lookup"><span data-stu-id="669b3-201">Use `$first` in the `ng-repeat` loop to determine whether the current index in your loop is the first row.</span></span> <span data-ttu-id="669b3-202">Auf ähnliche Weise verwenden `$last` zu bestimmen, ob der aktuelle Index der letzten Zeile ist.</span><span class="sxs-lookup"><span data-stu-id="669b3-202">Similarly, use `$last` to determine if the current index is the last row.</span></span>

<span data-ttu-id="669b3-203">Im folgenden finden Sie ein Beispiel mit `$index`, `$even`, `$odd`, `$first`, und `$last` in Aktion:</span><span class="sxs-lookup"><span data-stu-id="669b3-203">Below is a sample that shows `$index`, `$even`, `$odd`, `$first`, and `$last` in action:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters3.cshtml?highlight=14,15,16,17,18)]

<span data-ttu-id="669b3-204">So sieht die resultierende Ausgabe aus:</span><span class="sxs-lookup"><span data-stu-id="669b3-204">Here is the resulting output:</span></span>

![Repeater Beispiel 2](angular/_static/repeaters2.png)

### <a name="scope"></a><span data-ttu-id="669b3-206">$scope</span><span class="sxs-lookup"><span data-stu-id="669b3-206">$scope</span></span>

<span data-ttu-id="669b3-207">`$scope`ist ein JavaScript-Objekt, das als Verbindung zwischen der Ansicht (Vorlage) und den Controller (siehe unten) fungiert.</span><span class="sxs-lookup"><span data-stu-id="669b3-207">`$scope` is a JavaScript object that acts as glue between the view (template) and the controller (explained below).</span></span> <span data-ttu-id="669b3-208">Eine Ansichtenvorlage für die in AngularJS kennt nur die Werte, die angefügt werden, um die `$scope` Objekt im Controller.</span><span class="sxs-lookup"><span data-stu-id="669b3-208">A view template in AngularJS only knows about the values attached to the `$scope` object in the controller.</span></span>

> [!NOTE]
> <span data-ttu-id="669b3-209">In der Welt MVVM der `$scope` Objekt in AngularJS wird häufig als "ViewModel" definiert.</span><span class="sxs-lookup"><span data-stu-id="669b3-209">In the MVVM world, the `$scope` object in AngularJS is often defined as the ViewModel.</span></span> <span data-ttu-id="669b3-210">Die AngularJS-Team bezieht sich auf die `$scope` Objekt als das Datenmodell.</span><span class="sxs-lookup"><span data-stu-id="669b3-210">The AngularJS team refers to the `$scope` object as the Data-Model.</span></span> <span data-ttu-id="669b3-211">[Erfahren Sie mehr über Bereiche in AngularJS](https://docs.angularjs.org/guide/scope).</span><span class="sxs-lookup"><span data-stu-id="669b3-211">[Learn more about Scopes in AngularJS](https://docs.angularjs.org/guide/scope).</span></span>

<span data-ttu-id="669b3-212">Im folgenden ist ein einfaches Beispiel, das zeigt, wie zum Festlegen von Eigenschaften auf `$scope` innerhalb einer separaten JavaScript-Datei *scope.js*:</span><span class="sxs-lookup"><span data-stu-id="669b3-212">Below is a simple example showing how to set properties on `$scope` within a separate JavaScript file, *scope.js*:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/scope.js?highlight=2,3)]

<span data-ttu-id="669b3-213">Beachten Sie die `$scope` Parameter an den Controller übergeben, Zeile 2.</span><span class="sxs-lookup"><span data-stu-id="669b3-213">Observe the `$scope` parameter passed to the controller on line 2.</span></span> <span data-ttu-id="669b3-214">Dieses Objekt wurde die Sicht bekannt.</span><span class="sxs-lookup"><span data-stu-id="669b3-214">This object is what the view knows about.</span></span> <span data-ttu-id="669b3-215">In Zeile 3 wird eine Eigenschaft namens "Name", "Mary Jane" festgelegt.</span><span class="sxs-lookup"><span data-stu-id="669b3-215">On line 3, we are setting a property called "name" to "Mary Jane".</span></span>

<span data-ttu-id="669b3-216">Was geschieht, wenn eine bestimmte Eigenschaft von der Sicht nicht gefunden wird?</span><span class="sxs-lookup"><span data-stu-id="669b3-216">What happens when a particular property is not found by the view?</span></span> <span data-ttu-id="669b3-217">Die Sicht definierten unten bezieht sich auf die Eigenschaften "Name" und "Age":</span><span class="sxs-lookup"><span data-stu-id="669b3-217">The view defined below refers to "name" and "age" properties:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Scope.cshtml?highlight=9,10,14)]

<span data-ttu-id="669b3-218">Beachten Sie in Zeile 9, wir Angular bitten, um die Eigenschaft "Name" unter Verwendung der Abfrageausdruckssyntax anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="669b3-218">Notice on line 9 that we are asking Angular to show the "name" property using expression syntax.</span></span> <span data-ttu-id="669b3-219">Zeile 10 bezieht sich auf "age", eine Eigenschaft, die nicht vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="669b3-219">Line 10 then refers to "age", a property that does not exist.</span></span> <span data-ttu-id="669b3-220">Im aktuellen Beispiel zeigt den Namen "Mary Jane", und nichts für Alter der Daten festgelegt.</span><span class="sxs-lookup"><span data-stu-id="669b3-220">The running example shows the name set to "Mary Jane" and nothing for age.</span></span> <span data-ttu-id="669b3-221">Fehlende Eigenschaften werden ignoriert.</span><span class="sxs-lookup"><span data-stu-id="669b3-221">Missing properties are ignored.</span></span>

![Bereichsbeispiel](angular/_static/scope.png)

### <a name="modules"></a><span data-ttu-id="669b3-223">Module</span><span class="sxs-lookup"><span data-stu-id="669b3-223">Modules</span></span>

<span data-ttu-id="669b3-224">Ein [Modul](https://docs.angularjs.org/guide/module) in AngularJS ist eine Auflistung von Domänencontrollern, Dienste, Richtlinien. Die `angular.module()` wird zum Erstellen, registrieren und Abrufen von Modulen in AngularJS-Funktionsaufruf verwendet.</span><span class="sxs-lookup"><span data-stu-id="669b3-224">A [module](https://docs.angularjs.org/guide/module) in AngularJS is a collection of controllers, services, directives, etc. The `angular.module()` function call is used to create, register, and retrieve modules in AngularJS.</span></span> <span data-ttu-id="669b3-225">Alle Module, einschließlich der durch die AngularJS-Team und Drittanbieter-Bibliotheken ausgeliefert müssen registriert werden, mithilfe der `angular.module()` Funktion.</span><span class="sxs-lookup"><span data-stu-id="669b3-225">All modules, including those shipped by the AngularJS team and third party libraries, should be registered using the `angular.module()` function.</span></span>

<span data-ttu-id="669b3-226">Es folgt ein Codeausschnitt, das zum Erstellen eines neuen Moduls in AngularJS veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="669b3-226">Below is a snippet of code that shows how to create a new module in AngularJS.</span></span> <span data-ttu-id="669b3-227">Der erste Parameter ist der Name des Moduls.</span><span class="sxs-lookup"><span data-stu-id="669b3-227">The first parameter is the name of the module.</span></span> <span data-ttu-id="669b3-228">Der zweite Parameter definiert die Abhängigkeiten auf anderen Modulen ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="669b3-228">The second parameter defines dependencies on other modules.</span></span> <span data-ttu-id="669b3-229">Weiter unten in diesem Artikel wir detaillierter wie diese Abhängigkeiten übergeben ein `angular.module()` -Methodenaufruf.</span><span class="sxs-lookup"><span data-stu-id="669b3-229">Later in this article, we will be showing how to pass these dependencies to an `angular.module()` method call.</span></span>

```javascript
var personApp = angular.module('personApp', []);
```

<span data-ttu-id="669b3-230">Verwenden der `ng-app` Richtlinie, um eine AngularJS-Module auf der Seite darzustellen.</span><span class="sxs-lookup"><span data-stu-id="669b3-230">Use the `ng-app` directive to represent an AngularJS module on the page.</span></span> <span data-ttu-id="669b3-231">Module verwenden, weisen Sie den Namen des Moduls, `personApp` in diesem Beispiel zu den `ng-app` -Direktive in unserer Vorlage.</span><span class="sxs-lookup"><span data-stu-id="669b3-231">To use a module, assign the name of the module, `personApp` in this example, to the `ng-app` directive in our template.</span></span>

```html
<body ng-app="personApp">
```

### <a name="controllers"></a><span data-ttu-id="669b3-232">Domänencontroller</span><span class="sxs-lookup"><span data-stu-id="669b3-232">Controllers</span></span>

<span data-ttu-id="669b3-233">[Controller](https://docs.angularjs.org/guide/controller) in AngularJS werden der erste Punkt des Eintrags für Ihren Code.</span><span class="sxs-lookup"><span data-stu-id="669b3-233">[Controllers](https://docs.angularjs.org/guide/controller) in AngularJS are the first point of entry for your code.</span></span> <span data-ttu-id="669b3-234">Die `<module name>.controller()` wird zum Erstellen und Registrieren von Domänencontrollern in AngularJS-Funktionsaufruf verwendet.</span><span class="sxs-lookup"><span data-stu-id="669b3-234">The `<module name>.controller()` function call is used to create and register controllers in AngularJS.</span></span> <span data-ttu-id="669b3-235">Die `ng-controller` Richtlinie wird verwendet, um eine AngularJS-Controller, der HTML-Seite darstellen.</span><span class="sxs-lookup"><span data-stu-id="669b3-235">The `ng-controller` directive is used to represent an AngularJS controller on the HTML page.</span></span> <span data-ttu-id="669b3-236">Die Rolle des Controllers in Angular Zustand und das Verhalten des Datenmodells festgelegt ist (`$scope`).</span><span class="sxs-lookup"><span data-stu-id="669b3-236">The role of the controller in Angular is to set state and behavior of the data model (`$scope`).</span></span> <span data-ttu-id="669b3-237">Domänencontroller sollten nicht verwendet werden, auf das DOM direkt bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="669b3-237">Controllers should not be used to manipulate the DOM directly.</span></span>

<span data-ttu-id="669b3-238">Im folgenden ist ein Codeausschnitt, der einen neuen Domänencontroller registriert.</span><span class="sxs-lookup"><span data-stu-id="669b3-238">Below is a snippet of code that registers a new controller.</span></span> <span data-ttu-id="669b3-239">Die `personApp` Variable im Codeausschnitt verweist auf ein Angular-Modul, das für Zeile 2 definiert wird.</span><span class="sxs-lookup"><span data-stu-id="669b3-239">The `personApp` variable in the snippet references an Angular module, which is defined on line 2.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/controllers.js?highlight=2,5)]

<span data-ttu-id="669b3-240">Die Sicht mithilfe der `ng-controller` Richtlinie weist den Namen des Controllers:</span><span class="sxs-lookup"><span data-stu-id="669b3-240">The view using the `ng-controller` directive assigns the controller name:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Controllers.cshtml?highlight=8,14)]

<span data-ttu-id="669b3-241">Die Seite zeigt "Mary" und "Jane", die entsprechen den `firstName` und `lastName` angefügte Eigenschaften, um die `$scope` Objekt:</span><span class="sxs-lookup"><span data-stu-id="669b3-241">The page shows "Mary" and "Jane" that correspond to the `firstName` and `lastName` properties attached to the `$scope` object:</span></span>

![Controller-Beispiel](angular/_static/controllers.png)

### <a name="components"></a><span data-ttu-id="669b3-243">Komponenten</span><span class="sxs-lookup"><span data-stu-id="669b3-243">Components</span></span>

<span data-ttu-id="669b3-244">[Komponenten](https://docs.angularjs.org/guide/component) in Angular 1.5.x ermöglichen, für die Kapselung und die Möglichkeit, einzelne HTML-Elemente erstellen.</span><span class="sxs-lookup"><span data-stu-id="669b3-244">[Components](https://docs.angularjs.org/guide/component) in Angular 1.5.x allow for the encapsulation and capability of creating individual HTML elements.</span></span> <span data-ttu-id="669b3-245">In Angular 1.4.x installiert sein können Sie die gleiche Funktion mithilfe der .directive()-Methode erreichen.</span><span class="sxs-lookup"><span data-stu-id="669b3-245">In Angular 1.4.x you could achieve the same feature using the .directive() method.</span></span>

<span data-ttu-id="669b3-246">Mithilfe der .component()-Methode, um die Funktionalität der Direktive und den Controller Entwicklung vereinfacht.</span><span class="sxs-lookup"><span data-stu-id="669b3-246">By using the .component() method, development is simplified gaining the functionality of the directive and the controller.</span></span> <span data-ttu-id="669b3-247">Weitere Vorteile umfassen eine; bereichsisolation, bewährte Methoden sind inhärenten und Migration Angular 2 wird ein einfacher Vorgang.</span><span class="sxs-lookup"><span data-stu-id="669b3-247">Other benefits include; scope isolation, best practices are inherent, and migration to Angular 2 becomes an easier task.</span></span> <span data-ttu-id="669b3-248">Die `<module name>.component()` wird zum Erstellen und registrieren Komponenten in AngularJS-Funktionsaufruf verwendet.</span><span class="sxs-lookup"><span data-stu-id="669b3-248">The `<module name>.component()` function call is used to create and register components in AngularJS.</span></span>

<span data-ttu-id="669b3-249">Im folgenden ist ein Codeausschnitt, der eine neue Komponente registriert.</span><span class="sxs-lookup"><span data-stu-id="669b3-249">Below is a snippet of code that registers a new component.</span></span> <span data-ttu-id="669b3-250">Die `personApp` Variable im Codeausschnitt verweist auf ein Angular-Modul, das für Zeile 2 definiert wird.</span><span class="sxs-lookup"><span data-stu-id="669b3-250">The `personApp` variable in the snippet references an Angular module, which is defined on line 2.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/components.js?highlight=2,5,13)]

<span data-ttu-id="669b3-251">Die Ansicht, in dem wir das benutzerdefinierte HTML-Element angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="669b3-251">The view where we are displaying the custom HTML element.</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Components.cshtml?highlight=8)]

<span data-ttu-id="669b3-252">Die zugehörige Vorlage, die von der Komponente verwendet:</span><span class="sxs-lookup"><span data-stu-id="669b3-252">The associated template used by component:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/partials/personcomponent.html?highlight=2,3)]

<span data-ttu-id="669b3-253">Die Seite zeigt "Aftab" und "Ansari", die entsprechen den `firstName` und `lastName` angefügte Eigenschaften, um die `vm` Objekt:</span><span class="sxs-lookup"><span data-stu-id="669b3-253">The page shows "Aftab" and "Ansari" that correspond to the `firstName` and `lastName` properties attached to the `vm` object:</span></span>

![Komponenten-Beispiel](angular/_static/components.png)

### <a name="services"></a><span data-ttu-id="669b3-255">Dienste</span><span class="sxs-lookup"><span data-stu-id="669b3-255">Services</span></span>

<span data-ttu-id="669b3-256">[Services](https://docs.angularjs.org/guide/services) in AngularJS werden im Allgemeinen zum freigegebenen Code, der sofort in eine Datei abstrahiert die während der gesamten Lebensdauer einer Angular-Anwendung verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="669b3-256">[Services](https://docs.angularjs.org/guide/services) in AngularJS are commonly used for shared code that is abstracted away into a file which can be used throughout the lifetime of an Angular application.</span></span> <span data-ttu-id="669b3-257">Dienste werden verzögert instanziiert, was bedeutet, dass sich eine Instanz eines Diensts nicht, wenn eine Komponente, die von dem Dienst abhängt, verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="669b3-257">Services are lazily instantiated, meaning that there will not be an instance of a service unless a component that depends on the service gets used.</span></span> <span data-ttu-id="669b3-258">Factorys sind ein Beispiel für einen Dienst in AngularJS-Anwendungen verwendet.</span><span class="sxs-lookup"><span data-stu-id="669b3-258">Factories are an example of a service used in AngularJS applications.</span></span> <span data-ttu-id="669b3-259">Factorys werden erstellt, mit der `myApp.factory()` Funktionsaufruf, wobei `myApp` ist das Modul.</span><span class="sxs-lookup"><span data-stu-id="669b3-259">Factories are created using the `myApp.factory()` function call, where `myApp` is the module.</span></span>

<span data-ttu-id="669b3-260">Es folgt ein Beispiel für die Verwendung von Factorys in AngularJS verwenden:</span><span class="sxs-lookup"><span data-stu-id="669b3-260">Below is an example that shows how to use factories in AngularJS:</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/simpleFactory.js?highlight=1)]

<span data-ttu-id="669b3-261">Um diese Factory auf dem Controller aufzurufen, übergeben Sie `personFactory` als Parameter an die `controller` Funktion:</span><span class="sxs-lookup"><span data-stu-id="669b3-261">To call this factory from the controller, pass `personFactory` as a parameter to the `controller` function:</span></span>

```javascript
personApp.controller('personController', function($scope,personFactory) {
  $scope.name = personFactory.getName();
});
```

### <a name="using-services-to-talk-to-a-rest-endpoint"></a><span data-ttu-id="669b3-262">Mithilfe der Dienste mit einem REST-Endpunkt zu kommunizieren</span><span class="sxs-lookup"><span data-stu-id="669b3-262">Using services to talk to a REST endpoint</span></span>

<span data-ttu-id="669b3-263">Im folgenden ist ein Beispiel für die End-to-End-Dienste in AngularJS verwenden, für die Interaktion mit einem ASP.NET Core Web-API-Endpunkt.</span><span class="sxs-lookup"><span data-stu-id="669b3-263">Below is an end-to-end example using services in AngularJS to interact with an ASP.NET Core Web API endpoint.</span></span> <span data-ttu-id="669b3-264">Im Beispiel ruft Daten aus der Web-API ab und zeigt die Daten in einer Vorlage anzeigen.</span><span class="sxs-lookup"><span data-stu-id="669b3-264">The example gets data from the Web API and displays the data in a view template.</span></span> <span data-ttu-id="669b3-265">Beginnen wir mit der Ansicht zuerst:</span><span class="sxs-lookup"><span data-stu-id="669b3-265">Let's start with the view first:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Index.cshtml?highlight=5,8,10,17,18,19)]

<span data-ttu-id="669b3-266">In dieser Sicht haben wir eine Angular Modul mit dem Namen `PersonsApp` und ein Controller namens `personController`.</span><span class="sxs-lookup"><span data-stu-id="669b3-266">In this view, we have an Angular module called `PersonsApp` and a controller called `personController`.</span></span> <span data-ttu-id="669b3-267">Verwenden wir `ng-repeat` zum Durchlaufen der Liste der Personen ein.</span><span class="sxs-lookup"><span data-stu-id="669b3-267">We are using `ng-repeat` to iterate over the list of persons.</span></span> <span data-ttu-id="669b3-268">Es werden drei benutzerdefinierte JavaScript-Dateien in den Zeilen 17-19 verweisen.</span><span class="sxs-lookup"><span data-stu-id="669b3-268">We are referencing three custom JavaScript files on lines 17-19.</span></span>

<span data-ttu-id="669b3-269">Die *personApp.js* Datei wird zum Registrieren der `PersonsApp` -Modul und die Syntax ist ähnlich wie in vorherigen Beispielen.</span><span class="sxs-lookup"><span data-stu-id="669b3-269">The *personApp.js* file is used to register the `PersonsApp` module; and, the syntax is similar to previous examples.</span></span> <span data-ttu-id="669b3-270">Wir verwenden die `angular.module` Funktion zum Erstellen einer neuen Instanz des Moduls, mit denen wir arbeiten werden wird.</span><span class="sxs-lookup"><span data-stu-id="669b3-270">We are using the `angular.module` function to create a new instance of the module that we will be working with.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personApp.js?highlight=3)]

<span data-ttu-id="669b3-271">Werfen wir einen Blick auf *personFactory.js*weiter unten.</span><span class="sxs-lookup"><span data-stu-id="669b3-271">Let's take a look at *personFactory.js*, below.</span></span> <span data-ttu-id="669b3-272">Wir werden des Moduls aufrufen `factory` Methode zum Erstellen einer Factory.</span><span class="sxs-lookup"><span data-stu-id="669b3-272">We are calling the module’s `factory` method to create a factory.</span></span> <span data-ttu-id="669b3-273">Zeile 12 zeigt die integrierten Angular `$http` Dienst Personen Informationen von einem Webdienst abrufen.</span><span class="sxs-lookup"><span data-stu-id="669b3-273">Line 12 shows the built-in Angular `$http` service retrieving people information from a web service.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personFactory.js?highlight=6,7,12)]

<span data-ttu-id="669b3-274">In *personController.js*, wir werden des Moduls aufrufen `controller` Methode, um den Controller zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="669b3-274">In *personController.js*, we are calling the module’s `controller` method to create the controller.</span></span> <span data-ttu-id="669b3-275">Die `$scope` des Objekts `people` -Eigenschaft von der PersonFactory (Line 13) zurückgegebenen Daten zugewiesen ist.</span><span class="sxs-lookup"><span data-stu-id="669b3-275">The `$scope` object's `people` property is assigned the data returned from the personFactory (line 13).</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personController.js?highlight=6,7,13)]

<span data-ttu-id="669b3-276">Werfen wir einen Blick auf die Web-API und das Modell vortransformierungsphase.</span><span class="sxs-lookup"><span data-stu-id="669b3-276">Let's take a quick look at the Web API and the model behind it.</span></span> <span data-ttu-id="669b3-277">Die `Person` Modell ist eine POCO (Plain Old CLR-Objekt) mit `Id`, `FirstName`, und `LastName` Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="669b3-277">The `Person` model is a POCO (Plain Old CLR Object) with `Id`, `FirstName`, and `LastName` properties:</span></span>

[!code-csharp[Main](angular/sample/AngularJSSample/src/AngularJSSample/Models/Person.cs)]

<span data-ttu-id="669b3-278">Die `Person` Controller gibt eine JSON-formatierte Liste von `Person` Objekte:</span><span class="sxs-lookup"><span data-stu-id="669b3-278">The `Person` controller returns a JSON-formatted list of `Person` objects:</span></span>

[!code-csharp[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Controllers/Api/PersonController.cs?highlight=9,10,19)]

<span data-ttu-id="669b3-279">Sehen wir die Anwendung in Aktion:</span><span class="sxs-lookup"><span data-stu-id="669b3-279">Let's see the application in action:</span></span>

![Controller mit REST-Ergebnisse](angular/_static/rest-bound.png)

<span data-ttu-id="669b3-281">Sie können [zeigen Sie die Struktur der Anwendung auf GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).</span><span class="sxs-lookup"><span data-stu-id="669b3-281">You can [view the application's structure on GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).</span></span>

> [!NOTE]
> <span data-ttu-id="669b3-282">Weitere Informationen über die Strukturierung der AngularJS-Anwendungen, finden Sie unter [Angular Stilvorgaben des John Papa](https://github.com/johnpapa/angular-styleguide)</span><span class="sxs-lookup"><span data-stu-id="669b3-282">For more on structuring AngularJS applications, see [John Papa's Angular Style Guide](https://github.com/johnpapa/angular-styleguide)</span></span>

&nbsp;

> [!NOTE]
> <span data-ttu-id="669b3-283">Um AngularJS-Module "," Domänencontroller "," Factory zu erstellen, Richtlinie und Ansicht-Dateien einfach Achten Sie beim Auschecken der Sayed Hashimi [SideWaffle Template-Pack für Visual Studio](http://sidewaffle.com/).</span><span class="sxs-lookup"><span data-stu-id="669b3-283">To create AngularJS module, controller, factory, directive and view files easily, be sure to check out Sayed Hashimi's [SideWaffle template pack for Visual Studio](http://sidewaffle.com/).</span></span> <span data-ttu-id="669b3-284">Sayed Hashimi ist Senior Program Manager auf dem Visual Studio Web-Team bei Microsoft und SideWaffle Vorlagen gelten als der Premium-Standard.</span><span class="sxs-lookup"><span data-stu-id="669b3-284">Sayed Hashimi is a Senior Program Manager on the Visual Studio Web Team at Microsoft and SideWaffle templates are considered the gold standard.</span></span> <span data-ttu-id="669b3-285">Zum Zeitpunkt der Erstellung dieses Dokuments ist SideWaffle für Visual Studio 2012, 2013 und 2015 verfügbar.</span><span class="sxs-lookup"><span data-stu-id="669b3-285">At the time of this writing, SideWaffle is available for Visual Studio 2012, 2013, and 2015.</span></span>

### <a name="routing-and-multiple-views"></a><span data-ttu-id="669b3-286">Routing und mehrere Ansichten</span><span class="sxs-lookup"><span data-stu-id="669b3-286">Routing and multiple views</span></span>

<span data-ttu-id="669b3-287">AngularJS verfügt über einen integrierten Route Anbieter SPA (einseitigen-Anwendung) basierten Navigation zu behandeln.</span><span class="sxs-lookup"><span data-stu-id="669b3-287">AngularJS has a built-in route provider to handle SPA (Single Page Application) based navigation.</span></span> <span data-ttu-id="669b3-288">Sie müssen zum Arbeiten mit in AngularJS-routing, Hinzufügen der `angular-route` Bibliothek mithilfe von Bower.</span><span class="sxs-lookup"><span data-stu-id="669b3-288">To work with routing in AngularJS, you must add the `angular-route` library using Bower.</span></span> <span data-ttu-id="669b3-289">Sehen Sie der ["bower.JSON"](#angular-bower-json) Datei am Anfang dieses Artikels, wir es bereits im Projekt verweisen verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="669b3-289">You can see in the [bower.json](#angular-bower-json) file referenced at the start of this article that we are already referencing it in our project.</span></span>

<span data-ttu-id="669b3-290">Nach der Installation des Pakets den Skriptverweis hinzufügen (*angular route.js*) in Ihrer Ansicht anzeigen.</span><span class="sxs-lookup"><span data-stu-id="669b3-290">After you install the package, add the script reference (*angular-route.js*) to your view.</span></span>

<span data-ttu-id="669b3-291">Jetzt sehen wir uns die Person-App, wir haben erstellt wurde, und fügen Sie die Navigation hinzu.</span><span class="sxs-lookup"><span data-stu-id="669b3-291">Now let's take the Person App we have been building and add navigation to it.</span></span> <span data-ttu-id="669b3-292">Wir werden Stellen Sie zunächst eine Kopie der app durch Erstellen eines neuen `PeopleController` bewertungsaktion `Spa` und einer entsprechenden `Spa.cshtml` Ansicht durch Kopieren der Index.cshtml Ansicht in der `People` Ordner.</span><span class="sxs-lookup"><span data-stu-id="669b3-292">First, we will make a copy of the app by creating a new `PeopleController` action called `Spa` and a corresponding `Spa.cshtml` view by copying the Index.cshtml view in the `People` folder.</span></span> <span data-ttu-id="669b3-293">Fügen Sie einen Skriptverweis auf `angular-route` (siehe Zeile 11).</span><span class="sxs-lookup"><span data-stu-id="669b3-293">Add a script reference to `angular-route` (see line 11).</span></span> <span data-ttu-id="669b3-294">Auch hinzufügen, eine `div` mit markiert die `ng-view` Richtlinie (siehe Zeile 6) als Platzhalter, platzieren Sie die Sichten in.</span><span class="sxs-lookup"><span data-stu-id="669b3-294">Also add a `div` marked with the `ng-view` directive (see line 6) as a placeholder to place views in.</span></span> <span data-ttu-id="669b3-295">Wir werden einige zusätzliche verwenden *js* Dateien, die in den Zeilen 13 bis 16 verwiesen werden.</span><span class="sxs-lookup"><span data-stu-id="669b3-295">We are going to be using several additional *.js* files which are referenced on lines 13-16.</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Spa.cshtml?highlight=6,11,12,13,14,15,16)]

<span data-ttu-id="669b3-296">Werfen wir einen Blick auf *personModule.js* Datei, um festzustellen, wie wir das Modul mit dem routing instanziiert werden.</span><span class="sxs-lookup"><span data-stu-id="669b3-296">Let's take a look at *personModule.js* file to see how we are instantiating the module with routing.</span></span> <span data-ttu-id="669b3-297">Wir werden bestanden `ngRoute` als Bibliothek in das Modul.</span><span class="sxs-lookup"><span data-stu-id="669b3-297">We are passing `ngRoute` as a library into the module.</span></span> <span data-ttu-id="669b3-298">Dieses Modul wird verarbeitet, in der vorliegenden Anwendung routing.</span><span class="sxs-lookup"><span data-stu-id="669b3-298">This module handles routing in our application.</span></span>

[!code-javascript[Main](angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personModule.js)]

<span data-ttu-id="669b3-299">Die *personRoutes.js* Datei unten Routen, die basierend auf der Route-Anbieter definiert.</span><span class="sxs-lookup"><span data-stu-id="669b3-299">The *personRoutes.js* file, below, defines routes based on the route provider.</span></span> <span data-ttu-id="669b3-300">Zeilen 4 bis 7 definieren die Navigation, indem Sie effektiv darauf hingewiesen werden, wenn eine URL mit `/persons` wird angefordert, verwenden Sie eine Vorlage mit der Bezeichnung `partials/personlist` entwickeln, indem über `personListController`.</span><span class="sxs-lookup"><span data-stu-id="669b3-300">Lines 4-7 define navigation by effectively saying, when a URL with `/persons` is requested, use a template called `partials/personlist` by working through `personListController`.</span></span> <span data-ttu-id="669b3-301">Linien 8 bis 11, geben Sie eine Detailseite mit einem Routenparameter `personId`.</span><span class="sxs-lookup"><span data-stu-id="669b3-301">Lines 8-11 indicate a detail page with a route parameter of `personId`.</span></span> <span data-ttu-id="669b3-302">Wenn die URL nicht mit einem der Muster übereinstimmt, Angular standardmäßig die `/persons` anzeigen.</span><span class="sxs-lookup"><span data-stu-id="669b3-302">If the URL doesn't match one of the patterns, Angular defaults to the `/persons` view.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personRoutes.js?highlight=4,5,6,7,8,9,10,11,13)]

<span data-ttu-id="669b3-303">Die `personlist.html` Datei ist eine Teilansicht, enthält nur die HTML-, die für die Anzeige Personenliste benötigt.</span><span class="sxs-lookup"><span data-stu-id="669b3-303">The `personlist.html` file is a partial view containing only the HTML needed to display person list.</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/partials/personlist.html?highlight=3)]

<span data-ttu-id="669b3-304">Der Controller wird mithilfe des Moduls definiert `controller` -Funktion in *personListController.js*.</span><span class="sxs-lookup"><span data-stu-id="669b3-304">The controller is defined by using the module's `controller` function in *personListController.js*.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personListController.js?highlight=1)]

<span data-ttu-id="669b3-305">Wenn wir diese Anwendung führen, und navigieren Sie zu der `people/spa#/persons` URL, siehe wir:</span><span class="sxs-lookup"><span data-stu-id="669b3-305">If we run this application and navigate to the `people/spa#/persons` URL, we will see:</span></span>

![Personen Listenansicht](angular/_static/spa-persons.png)

<span data-ttu-id="669b3-307">Wenn wir z. B. eine Detailseite navigieren `people/spa#/persons/2`, sehen wir die Teilansicht Detail:</span><span class="sxs-lookup"><span data-stu-id="669b3-307">If we navigate to a detail page, for example `people/spa#/persons/2`, we will see the detail partial view:</span></span>

![Person detaillierte Statusansicht](angular/_static/spa-persons-2.png)

<span data-ttu-id="669b3-309">Sehen Sie den vollständigen Quellcode und alle Dateien, die auf nicht angezeigt, in diesem Artikel [GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).</span><span class="sxs-lookup"><span data-stu-id="669b3-309">You can view the full source and any files not shown in this article on [GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).</span></span>

### <a name="event-handlers"></a><span data-ttu-id="669b3-310">Ereignishandler</span><span class="sxs-lookup"><span data-stu-id="669b3-310">Event Handlers</span></span>

<span data-ttu-id="669b3-311">Es gibt eine Reihe von Anweisungen in AngularJS, mit denen der Eingabeelemente in Ihre HTML-DOM. Ereignisbehandlung Funktionen hinzugefügt</span><span class="sxs-lookup"><span data-stu-id="669b3-311">There are a number of directives in AngularJS that add event-handling capabilities to the input elements in your HTML DOM.</span></span> <span data-ttu-id="669b3-312">Es folgt eine Liste der Ereignisse, die in AngularJS integriert sind.</span><span class="sxs-lookup"><span data-stu-id="669b3-312">Below is a list of the events that are built into AngularJS.</span></span>

   * `ng-click`

   * `ng-dbl-click`

   * `ng-mousedown`

   * `ng-mouseup`

   * `ng-mouseenter`

   * `ng-mouseleave`

   * `ng-mousemove`

   * `ng-keydown`

   * `ng-keyup`

   * `ng-keypress`

   * `ng-change`

> [!NOTE]
> <span data-ttu-id="669b3-313">Sie können Ihren eigenen Ereignishandler, die mit Hinzufügen der [benutzerdefinierte Direktiven feature AngularJS](https://docs.angularjs.org/guide/directive).</span><span class="sxs-lookup"><span data-stu-id="669b3-313">You can add your own event handlers using the [custom directives feature in AngularJS](https://docs.angularjs.org/guide/directive).</span></span>

<span data-ttu-id="669b3-314">Sehen wir uns an, wie die `ng-click` Ereignis läuft.</span><span class="sxs-lookup"><span data-stu-id="669b3-314">Let's look at how the `ng-click` event is wired up.</span></span> <span data-ttu-id="669b3-315">Erstellen Sie eine neue JavaScript-Datei mit dem Namen *eventHandlerController.js*, und fügen Sie Folgendes hinzu:</span><span class="sxs-lookup"><span data-stu-id="669b3-315">Create a new JavaScript file named *eventHandlerController.js*, and add the following to it:</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/eventHandlerController.js?highlight=5,6,7)]

<span data-ttu-id="669b3-316">Beachten Sie das neue `sayName` -Funktion in `eventHandlerController` in Zeile 5 oben.</span><span class="sxs-lookup"><span data-stu-id="669b3-316">Notice the new `sayName` function in `eventHandlerController` on line 5 above.</span></span> <span data-ttu-id="669b3-317">All-Methode ist jedoch für jetzt eine JavaScript-Warnung für dem Benutzer eine Begrüßungsnachricht angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="669b3-317">All the method is doing for now is showing a JavaScript alert to the user with a welcome message.</span></span>

<span data-ttu-id="669b3-318">Die folgende Sicht bindet eine Domänencontroller-Funktion auf eine AngularJS-Ereignis.</span><span class="sxs-lookup"><span data-stu-id="669b3-318">The view below binds a controller function to an AngularJS event.</span></span> <span data-ttu-id="669b3-319">Zeile 9 verfügt über eine Schaltfläche auf dem die `ng-click` Angular-Richtlinie angewendet wurde.</span><span class="sxs-lookup"><span data-stu-id="669b3-319">Line 9 has a button on which the `ng-click` Angular directive has been applied.</span></span> <span data-ttu-id="669b3-320">Ruft unsere `sayName` -Funktion, die angefügt ist die `$scope` , in dieser Ansicht übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="669b3-320">It calls our `sayName` function, which is attached to the `$scope` object passed to this view.</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Events.cshtml?highlight=9)]

<span data-ttu-id="669b3-321">Im aktuellen Beispiel zeigt, dass des Controllers `sayName` Funktion wird automatisch aufgerufen, wenn die Schaltfläche geklickt wird.</span><span class="sxs-lookup"><span data-stu-id="669b3-321">The running example demonstrates that the controller's `sayName` function is called automatically when the button is clicked.</span></span>

![Click-Ereignis](angular/_static/events.png)

<span data-ttu-id="669b3-323">Detailliertere auf AngularJS integrierte Ereignis-Handler-Direktiven, achten Sie auf die [Dokumentationswebsite](https://docs.angularjs.org/api/ng/directive/ngClick) von AngularJS.</span><span class="sxs-lookup"><span data-stu-id="669b3-323">For more detail on AngularJS built-in event handler directives, be sure to head to the [documentation website](https://docs.angularjs.org/api/ng/directive/ngClick) of AngularJS.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="669b3-324">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="669b3-324">Additional resources</span></span>

* [<span data-ttu-id="669b3-325">Angular Docs</span><span class="sxs-lookup"><span data-stu-id="669b3-325">Angular Docs</span></span>](https://docs.angularjs.org)

* [<span data-ttu-id="669b3-326">Angular 2 Info</span><span class="sxs-lookup"><span data-stu-id="669b3-326">Angular 2 Info</span></span>](https://angular.io/)
