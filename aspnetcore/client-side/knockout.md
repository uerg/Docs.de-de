---
title: Knockout.js MVVM-Framework in ASP.NET Core
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: b20e3b23-1c51-47bf-adac-91b5048567e0
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/knockout
ms.openlocfilehash: d1c5cbd430587b757bb550f8f04355e67f04eb54
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2017
---
# <a name="knockoutjs-mvvm-framework-in-aspnet-core"></a><span data-ttu-id="89ebf-103">Knockout.js MVVM-Framework in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="89ebf-103">Knockout.js MVVM Framework in ASP.NET Core</span></span>

<span data-ttu-id="89ebf-104">Durch [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="89ebf-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="89ebf-105">Knockout ist eine beliebte JavaScript-Bibliothek, die die Erstellung von komplexen datenbasierte Benutzeroberflächen vereinfacht.</span><span class="sxs-lookup"><span data-stu-id="89ebf-105">Knockout is a popular JavaScript library that simplifies the creation of complex data-based user interfaces.</span></span> <span data-ttu-id="89ebf-106">Sie können alleine oder zusammen mit anderen Bibliotheken, z. B. jQuery verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="89ebf-106">It can be used alone or with other libraries, such as jQuery.</span></span> <span data-ttu-id="89ebf-107">Der Hauptzweck ist Benutzeroberflächenelemente an eine zugrunde liegende Datenmodell definiert, die als JavaScript-Objekt, zu binden, wenn Änderungen an der Benutzeroberfläche vorgenommen werden, die das Modell aktualisiert wird, und umgekehrt.</span><span class="sxs-lookup"><span data-stu-id="89ebf-107">Its primary purpose is to bind UI elements to an underlying data model defined as a JavaScript object, such that when changes are made to the UI, the model is updated, and vice versa.</span></span> <span data-ttu-id="89ebf-108">Knockout erleichtert die Verwendung eines Musters Model-View-ViewModel (MVVM) eine Webanwendung clientseitige Verhalten.</span><span class="sxs-lookup"><span data-stu-id="89ebf-108">Knockout facilitates the use of a Model-View-ViewModel (MVVM) pattern in a web application's client-side behavior.</span></span> <span data-ttu-id="89ebf-109">Die beiden wichtigsten Konzepte, die eine Kennenlernen muss bei der Arbeit mit Knockout des MVVM Implementierung werden Wahrnehmbare Elemente und Bindungen.</span><span class="sxs-lookup"><span data-stu-id="89ebf-109">The two main concepts one must learn when working with Knockout's MVVM implementation are Observables and Bindings.</span></span>

## <a name="getting-started"></a><span data-ttu-id="89ebf-110">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="89ebf-110">Getting started</span></span>

<span data-ttu-id="89ebf-111">Knockout wird bereitgestellt, als eine einzelne JavaScript-Datei, also installieren und verwenden, ist sehr einfach mit [Bower](bower.md).</span><span class="sxs-lookup"><span data-stu-id="89ebf-111">Knockout is deployed as a single JavaScript file, so installing and using it is very straightforward using [bower](bower.md).</span></span> <span data-ttu-id="89ebf-112">Angenommen, Sie bereits haben [Bower](bower.md) und [gulp](using-gulp.md) konfiguriert ist, öffnen Sie *"bower.JSON"* in Ihre ASP.NET Core Projekt, und fügen Sie die Abhängigkeit Knockout, wie hier gezeigt:</span><span class="sxs-lookup"><span data-stu-id="89ebf-112">Assuming you already have [bower](bower.md) and [gulp](using-gulp.md) configured, open *bower.json* in your ASP.NET Core project and add the knockout dependency as shown here:</span></span>

```json
{
  "name": "KnockoutDemo",
  "private": true,
  "dependencies": {
    "knockout" : "^3.3.0"
  },
  "exportsOverride": {
  }
}
```

<span data-ttu-id="89ebf-113">Mit diesem erfüllt können Sie dann manuell Bower ausführen durch Öffnen der Taskausführungs-Explorer (unter Ansicht ‣ Weitere Fenster ‣ Taskausführungs-Explorer), und klicken Sie dann unter Aufgaben mit der Maustaste, auf Bower und Ausführung auswählen.</span><span class="sxs-lookup"><span data-stu-id="89ebf-113">With this in place, you can then manually run bower by opening the Task Runner Explorer (under View ‣ Other Windows ‣ Task Runner Explorer) and then under Tasks, right-click on bower and select Run.</span></span> <span data-ttu-id="89ebf-114">Das Ergebnis sollte etwa wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="89ebf-114">The result should appear similar to this:</span></span>

![bower ausgeführten Knockout in Taskausführungs-Explorer](knockout/_static/bower-knockout.png)

<span data-ttu-id="89ebf-116">Wenn Sie in Ihrem Projekts suchen nun `wwwroot` Ordner sollte Knockout unter dem Ordner "Lib" installiert.</span><span class="sxs-lookup"><span data-stu-id="89ebf-116">Now if you look in your project's `wwwroot` folder, you should see knockout installed under the lib folder.</span></span>

![im Ordner "Lib" installiert Knockout](knockout/_static/wwwroot-knockout.png)

<span data-ttu-id="89ebf-118">Es wird empfohlen, dass in Ihrer produktionsumgebung Sie Knockout über ein Netzwerk für die Inhaltsübermittlung oder CDN, verweisen, wie dies die Wahrscheinlichkeit erhöht, dass Ihre Benutzer bereits eine zwischengespeicherte Kopie der Datei müssen und daher nicht auf allen herunterladen müssen.</span><span class="sxs-lookup"><span data-stu-id="89ebf-118">It's recommended that in your production environment you reference knockout via a Content Delivery Network, or CDN, as this increases the likelihood that your users will already have a cached copy of the file and thus will not need to download it at all.</span></span> <span data-ttu-id="89ebf-119">Knockout ist auf mehrere CDNs, einschließlich Microsoft Ajax-CDNS, hier verfügbar:</span><span class="sxs-lookup"><span data-stu-id="89ebf-119">Knockout is available on several CDNs, including the Microsoft Ajax CDN, here:</span></span>

[<span data-ttu-id="89ebf-120">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.3.0.js</span><span class="sxs-lookup"><span data-stu-id="89ebf-120">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js</span></span>](http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js)

<span data-ttu-id="89ebf-121">Um Knockout auf einer Seite einzuschließen, die sie verwenden, fügen Sie einfach eine `<script>` Element auf die Datei verweist, von wo Sie sie (mit der Anwendung oder über ein CDN) gehostet werden:</span><span class="sxs-lookup"><span data-stu-id="89ebf-121">To include Knockout on a page that will use it, simply add a `<script>` element referencing the file from wherever you will be hosting it (with your application, or via a CDN):</span></span>

```html
<script type="text/javascript" src="knockout-3.3.0.js"></script>
```

## <a name="observables-viewmodels-and-simple-binding"></a><span data-ttu-id="89ebf-122">Wahrnehmbare Elemente ViewModels und einfache Bindung</span><span class="sxs-lookup"><span data-stu-id="89ebf-122">Observables, ViewModels, and simple binding</span></span>

<span data-ttu-id="89ebf-123">Sie können bereits mit JavaScript zum Bearbeiten von Elementen auf einer Webseite, entweder über direkten Zugriff auf das DOM oder eine Bibliothek, z. B. jQuery vertraut sein.</span><span class="sxs-lookup"><span data-stu-id="89ebf-123">You may already be familiar with using JavaScript to manipulate elements on a web page, either via direct access to the DOM or using a library like jQuery.</span></span> <span data-ttu-id="89ebf-124">Diese Art von Verhalten in der Regel erfolgt durch Schreiben von Code, um Elementwerte in Reaktion auf bestimmte Benutzeraktionen direkt festzulegen.</span><span class="sxs-lookup"><span data-stu-id="89ebf-124">Typically this kind of behavior is achieved by writing code to directly set element values in response to certain user actions.</span></span> <span data-ttu-id="89ebf-125">Mit Knockout erfolgt ein deklarativer Ansatz stattdessen über die Elemente auf der Seite Eigenschaften für ein Objekt gebunden sind.</span><span class="sxs-lookup"><span data-stu-id="89ebf-125">With Knockout, a declarative approach is taken instead, through which elements on the page are bound to properties on an object.</span></span> <span data-ttu-id="89ebf-126">Anstatt das Schreiben von Code zum Bearbeiten von DOM-Elementen, Benutzeraktionen interagieren einfach das ViewModel-Objekt und Knockout übernimmt das sicherstellen, dass die Seitenelemente synchronisiert werden.</span><span class="sxs-lookup"><span data-stu-id="89ebf-126">Instead of writing code to manipulate DOM elements, user actions simply interact with the ViewModel object, and Knockout takes care of ensuring the page elements are synchronized.</span></span>

<span data-ttu-id="89ebf-127">Betrachten Sie als einfaches Beispiel der Liste unten aus.</span><span class="sxs-lookup"><span data-stu-id="89ebf-127">As a simple example, consider the page list below.</span></span> <span data-ttu-id="89ebf-128">Er enthält eine `<span>` Element mit einem `data-bind` Attribut gibt an, dass der Text-Inhalt AuthorName gebunden werden soll.</span><span class="sxs-lookup"><span data-stu-id="89ebf-128">It includes a `<span>` element with a `data-bind` attribute indicating that the text content should be bound to authorName.</span></span> <span data-ttu-id="89ebf-129">Im nächsten Schritt in eine JavaScript-Blocks eine Variable ViewModel, mit einer einzelnen Eigenschaft definiert wird `authorName`, auf einen beliebigen Wert festgelegt.</span><span class="sxs-lookup"><span data-stu-id="89ebf-129">Next, in a JavaScript block a variable viewModel is defined with a single property, `authorName`, set to some value.</span></span> <span data-ttu-id="89ebf-130">Schließlich ein Aufruf von `ko.applyBindings` vorgenommen wird, wird in dieser ViewModel-Variable übergeben.</span><span class="sxs-lookup"><span data-stu-id="89ebf-130">Finally, a call to `ko.applyBindings` is made, passing in this viewModel variable.</span></span>

```html
<html>
<head>
    <script type="text/javascript" src="lib/knockout/knockout.js"></script>
</head>
<body>
    <h1>Some Article</h1>
    <p>
        By <span data-bind="text: authorName"></span>
    </p>
    <script type="text/javascript">
      var viewModel = {
        authorName: 'Steve Smith'
      };
      ko.applyBindings(viewModel);
    </script>
</body>
</html>
```

<span data-ttu-id="89ebf-131">Bei der Anzeige in den Browser, den Inhalt der <span> Element mit dem Wert in der Variablen ViewModel ersetzt wird:</span><span class="sxs-lookup"><span data-stu-id="89ebf-131">When viewed in the browser, the content of the <span> element is replaced with the value in the viewModel variable:</span></span>

![einfache Bindung Knockout](knockout/_static/simple-binding-screenshot.png)

<span data-ttu-id="89ebf-133">Wir haben jetzt einfache unidirektionale Bindung arbeiten.</span><span class="sxs-lookup"><span data-stu-id="89ebf-133">We now have simple one-way binding working.</span></span> <span data-ttu-id="89ebf-134">Beachten Sie, dass nie im Code wir JavaScript, um den Inhalt der Spanne einen Wert zuzuweisen geschrieben haben.</span><span class="sxs-lookup"><span data-stu-id="89ebf-134">Notice that nowhere in the code did we write JavaScript to assign a value to the span's contents.</span></span> <span data-ttu-id="89ebf-135">Wenn wir das ViewModel bearbeiten möchten, können wir übernehmen dies einen Schritt weiter und eine HTML-Eingabe Textbox hinzufügen und binden Sie mit dem Wert, z. B. so:</span><span class="sxs-lookup"><span data-stu-id="89ebf-135">If we want to manipulate the ViewModel, we can take this a step further and add an HTML input textbox, and bind to its value, like so:</span></span>

```html
<p>
    Author Name: <input type="text" data-bind="value: authorName" />
</p>
```

<span data-ttu-id="89ebf-136">Die Seite wird erneut geladen, sehen wir, dass dieser Wert auf das Eingabefeld tatsächlich gebunden ist:</span><span class="sxs-lookup"><span data-stu-id="89ebf-136">Reloading the page, we see that this value is indeed bound to the input box:</span></span>

![eingabebindung Knockout](knockout/_static/input-binding-screenshot.png)

<span data-ttu-id="89ebf-138">Jedoch wenn wir den Wert in das Textfeld zu ändern, den entsprechenden Wert in der `<span>` Elemente nicht ändern.</span><span class="sxs-lookup"><span data-stu-id="89ebf-138">However, if we change the value in the textbox, the corresponding value in the `<span>` element doesn't change.</span></span> <span data-ttu-id="89ebf-139">Warum?</span><span class="sxs-lookup"><span data-stu-id="89ebf-139">Why not?</span></span>

<span data-ttu-id="89ebf-140">Das Problem besteht darin, dass nichts benachrichtigt den `<span>` , die es benötigt, um aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="89ebf-140">The issue is that nothing notified the `<span>` that it needed to be updated.</span></span> <span data-ttu-id="89ebf-141">Einfach aktualisieren ViewModel ist nicht allein ausreichend, es sei denn, das ViewModel-Eigenschaften in eine Sonderform umschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="89ebf-141">Simply updating the ViewModel isn't by itself sufficient, unless the ViewModel's properties are wrapped in a special type.</span></span> <span data-ttu-id="89ebf-142">Wir verwenden müssen **Wahrnehmbare Elemente** im ViewModel für alle Eigenschaften, die nur geändert, die automatisch aktualisiert, sobald sie auftreten müssen.</span><span class="sxs-lookup"><span data-stu-id="89ebf-142">We need to use **observables** in the ViewModel for any properties that need to have changes automatically updated as they occur.</span></span> <span data-ttu-id="89ebf-143">Durch Ändern der ViewModel verwenden `ko.observable("value")` statt nur "Value" aktualisiert das ViewModel HTML-Elemente, die auf den Wert gebunden sind, wenn eine Änderung erfolgt.</span><span class="sxs-lookup"><span data-stu-id="89ebf-143">By changing the ViewModel to use `ko.observable("value")` instead of just "value", the ViewModel will update any HTML elements that are bound to its value whenever a change occurs.</span></span> <span data-ttu-id="89ebf-144">Beachten Sie, dass Eingabefelder ihr Wert nicht aktualisieren, bis sie den Fokus, verlieren, sodass Sie Änderungen an Elementen gebunden, während der Eingabe angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="89ebf-144">Note that input boxes don't update their value until they lose focus, so you won't see changes to bound elements as you type.</span></span>

> [!NOTE]
> <span data-ttu-id="89ebf-145">Hinzufügen der Unterstützung für live aktualisieren, nachdem jedes Keypress lediglich hinzugefügt wird `valueUpdate: "afterkeydown"` auf die `data-bind` des Attributs.</span><span class="sxs-lookup"><span data-stu-id="89ebf-145">Adding support for live updating after each keypress is simply a matter of adding `valueUpdate: "afterkeydown"` to the `data-bind` attribute's contents.</span></span> <span data-ttu-id="89ebf-146">Sie können dieses Verhalten auch abrufen, mithilfe von `data-bind="textInput: authorName"` sofortige Updates Werte abrufen.</span><span class="sxs-lookup"><span data-stu-id="89ebf-146">You can also get this behavior by using `data-bind="textInput: authorName"` to get instant updates of values.</span></span> 

<span data-ttu-id="89ebf-147">Unsere ViewModel nach der Aktualisierung, um ko.observable zu verwenden:</span><span class="sxs-lookup"><span data-stu-id="89ebf-147">Our viewModel, after updating it to use ko.observable:</span></span>

```javascript
var viewModel = {
  authorName: ko.observable('Steve Smith')
};
ko.applyBindings(viewModel);
```

<span data-ttu-id="89ebf-148">Knockout unterstützt eine Reihe von verschiedenen Arten von Bindungen.</span><span class="sxs-lookup"><span data-stu-id="89ebf-148">Knockout supports a number of different kinds of bindings.</span></span> <span data-ttu-id="89ebf-149">Bisher haben wir gesehen, wie zum Binden an `text` und `value`.</span><span class="sxs-lookup"><span data-stu-id="89ebf-149">So far we've seen how to bind to `text` and to `value`.</span></span> <span data-ttu-id="89ebf-150">Sie können auch auf alle angegebenen Attribute binden.</span><span class="sxs-lookup"><span data-stu-id="89ebf-150">You can also bind to any given attribute.</span></span> <span data-ttu-id="89ebf-151">Für die Instanz, um einen Hyperlink erstellen, mit der ein Ankertag der `src` Attribut auf das ViewModel gebunden werden kann.</span><span class="sxs-lookup"><span data-stu-id="89ebf-151">For instance, to create a hyperlink with an anchor tag, the `src` attribute can be bound to the viewModel.</span></span> <span data-ttu-id="89ebf-152">Knockout unterstützt auch die Bindung an Funktionen.</span><span class="sxs-lookup"><span data-stu-id="89ebf-152">Knockout also supports binding to functions.</span></span> <span data-ttu-id="89ebf-153">Zur Veranschaulichung nehmen wir das ViewModel Einbeziehung des Autors Twitter Handle anzuzeigen und zu aktualisieren das Twitter-Handle als Link zu der Autor Twitter-Seite.</span><span class="sxs-lookup"><span data-stu-id="89ebf-153">To demonstrate this, let's update the viewModel to include the author's twitter handle, and display the twitter handle as a link to the author's twitter page.</span></span> <span data-ttu-id="89ebf-154">Dazu müssen Sie drei Phasen verwenden.</span><span class="sxs-lookup"><span data-stu-id="89ebf-154">We'll do this in three stages.</span></span>

<span data-ttu-id="89ebf-155">Fügen Sie zunächst den HTML-Code, um den Link wird in Klammern nach dem Namen des Autors zeige anzuzeigen:</span><span class="sxs-lookup"><span data-stu-id="89ebf-155">First, add the HTML to display the hyperlink, which we'll show in parentheses after the author's name:</span></span>

```html
<h1>Some Article</h1>
<p>
    By <span data-bind="text: authorName"></span>
    (<a data-bind="attr: { href: twitterUrl}, text: twitterAlias" ></a>)
</p>
```

<span data-ttu-id="89ebf-156">Aktualisieren Sie als Nächstes das ViewModel, um die TwitterUrl und TwitterAlias umfassen:</span><span class="sxs-lookup"><span data-stu-id="89ebf-156">Next, update the viewModel to include the twitterUrl and twitterAlias properties:</span></span>

```javascript
var viewModel = {
  authorName: ko.observable('Steve Smith'),
  twitterAlias: ko.observable('@ardalis'),
  twitterUrl: ko.computed(function() {
    return "https://twitter.com/";
  }, this)
};
ko.applyBindings(viewModel);
```

<span data-ttu-id="89ebf-157">Beachten Sie, dass zu diesem Zeitpunkt noch nicht wir noch die TwitterUrl fahren Sie mit der richtigen URL für diesen Alias Twitter aktualisiert – nur am twitter.com verweist. Beachten Sie außerdem, dass wir eine neue Knockout-Funktion verwenden, `computed`, für TwitterUrl.</span><span class="sxs-lookup"><span data-stu-id="89ebf-157">Notice that at this point we haven't yet updated the twitterUrl to go to the correct URL for this twitter alias – it's just pointing at twitter.com. Also notice that we're using a new Knockout function, `computed`, for twitterUrl.</span></span> <span data-ttu-id="89ebf-158">Dies ist eine Observable-Funktion, mit die alle Benutzeroberflächenelemente benachrichtigt wird, wenn er geändert wird.</span><span class="sxs-lookup"><span data-stu-id="89ebf-158">This is an observable function that will notify any UI elements if it changes.</span></span> <span data-ttu-id="89ebf-159">Hierfür auf anderen Eigenschaften im ViewModel zugreifen, müssen wir jedoch ändern wie wir das ViewModel erstellen, sodass jede Eigenschaft einen eigenen-Anweisung ist.</span><span class="sxs-lookup"><span data-stu-id="89ebf-159">However, for it to have access to other properties in the viewModel, we need to change how we are creating the viewModel, so that each property is its own statement.</span></span>

<span data-ttu-id="89ebf-160">Die überarbeitete ViewModel-Deklaration ist unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="89ebf-160">The revised viewModel declaration is shown below.</span></span> <span data-ttu-id="89ebf-161">Es ist jetzt als Funktion deklariert.</span><span class="sxs-lookup"><span data-stu-id="89ebf-161">It is now declared as a function.</span></span> <span data-ttu-id="89ebf-162">Beachten Sie, dass jede Eigenschaft einen eigenen-Anweisung nun mit einem Semikolon beendet ist.</span><span class="sxs-lookup"><span data-stu-id="89ebf-162">Notice that each property is its own statement now, ending with a semicolon.</span></span> <span data-ttu-id="89ebf-163">Beachten Sie außerdem den Zugriff auf den Eigenschaftswert TwitterAlias müssen, sodass dessen Verweis enthält () ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="89ebf-163">Also notice that to access the twitterAlias property value, we need to execute it, so its reference includes ().</span></span>

```javascript
function viewModel() {
  this.authorName = ko.observable('Steve Smith');
  this.twitterAlias = ko.observable('@ardalis');

  this.twitterUrl = ko.computed(function() {
    return "https://twitter.com/" + this.twitterAlias().replace('@','');
  }, this)
};
ko.applyBindings(viewModel);
```

<span data-ttu-id="89ebf-164">Das Ergebnis funktioniert wie erwartet im Browser:</span><span class="sxs-lookup"><span data-stu-id="89ebf-164">The result works as expected in the browser:</span></span>

![Knockout hyperlink](knockout/_static/hyperlink-screenshot.png)

<span data-ttu-id="89ebf-166">Knockout unterstützt auch die Bindung auf bestimmte Ereignisse des UI-Element, z. B. das Click-Ereignis.</span><span class="sxs-lookup"><span data-stu-id="89ebf-166">Knockout also supports binding to certain UI element events, such as the click event.</span></span> <span data-ttu-id="89ebf-167">Dadurch können Sie problemlos und deklarativ Benutzeroberflächenelemente Funktionen innerhalb der Anwendung ViewModel gebunden.</span><span class="sxs-lookup"><span data-stu-id="89ebf-167">This allows you to easily and declaratively bind UI elements to functions within the application's viewModel.</span></span> <span data-ttu-id="89ebf-168">Als einfaches Beispiel können Sie eine Schaltfläche hinzufügen, wenn angeklickt, ändert der Autor TwitterAlias GROSSBUCHSTABEN werden.</span><span class="sxs-lookup"><span data-stu-id="89ebf-168">As a simple example, we can add a button that, when clicked, modifies the author's twitterAlias to be all caps.</span></span>

<span data-ttu-id="89ebf-169">Zunächst wird die Schaltfläche "hinzufügen", Bindung auf der Schaltfläche klicken Sie auf Ereignis, und verweisen auf den Namen der Funktion, die wir möchten das ViewModel hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="89ebf-169">First, we add the button, binding to the button's click event, and referencing the function name we're going to add to the viewModel:</span></span>

```html
<p>
    <button data-bind="click: capitalizeTwitterAlias">Capitalize</button>
</p>
```

<span data-ttu-id="89ebf-170">Klicken Sie dann fügen Sie die Funktion das ViewModel hinzu, und verknüpfen sie das ViewModel-Status ändern.</span><span class="sxs-lookup"><span data-stu-id="89ebf-170">Then, add the function to the viewModel, and wire it up to modify the viewModel's state.</span></span> <span data-ttu-id="89ebf-171">Beachten Sie, dass wir um einen neuen Wert der Eigenschaft TwitterAlias festzulegen, rufen Sie es als eine Methode und übergeben Sie den neuen Wert ein.</span><span class="sxs-lookup"><span data-stu-id="89ebf-171">Notice that to set a new value to the twitterAlias property, we call it as a method and pass in the new value.</span></span>

```javascript
function viewModel() {
  this.authorName = ko.observable('Steve Smith');
  this.twitterAlias = ko.observable('@ardalis');

  this.twitterUrl = ko.computed(function() {
    return "https://twitter.com/" + this.twitterAlias().replace('@','');
  }, this);

  this.capitalizeTwitterAlias = function() {
    var currentValue = this.twitterAlias();
    this.twitterAlias(currentValue.toUpperCase());
  }
};
ko.applyBindings(viewModel);
```

<span data-ttu-id="89ebf-172">Der Code ausgeführt wird, und klicken auf die Schaltfläche ändert den angezeigten Link wie erwartet:</span><span class="sxs-lookup"><span data-stu-id="89ebf-172">Running the code and clicking the button modifies the displayed link as expected:</span></span>

![Hyperlink Großbuchstaben](knockout/_static/hyperlink-caps-screenshot.png)

## <a name="control-flow"></a><span data-ttu-id="89ebf-174">Ablaufsteuerung</span><span class="sxs-lookup"><span data-stu-id="89ebf-174">Control flow</span></span>

<span data-ttu-id="89ebf-175">Knockout umfasst Bindungen, die bedingte und Schleifen-Vorgänge ausführen können.</span><span class="sxs-lookup"><span data-stu-id="89ebf-175">Knockout includes bindings that can perform conditional and looping operations.</span></span> <span data-ttu-id="89ebf-176">Schleifenvorgänge sind besonders nützlich zum Binden von Listen der Daten an UI-Listen, Menüs und Rastern oder Tabellen.</span><span class="sxs-lookup"><span data-stu-id="89ebf-176">Looping operations are especially useful for binding lists of data to UI lists, menus, and grids or tables.</span></span> <span data-ttu-id="89ebf-177">Die foreach-Bindung wird ein Array durchlaufen.</span><span class="sxs-lookup"><span data-stu-id="89ebf-177">The foreach binding will iterate over an array.</span></span> <span data-ttu-id="89ebf-178">Bei Verwendung mit einem Observable-Array wird wird er automatisch die Elemente der Benutzeroberfläche aktualisiert, wenn Elemente hinzugefügt oder aus dem Array entfernt werden, ohne dass jedes Element in der Benutzeroberflächenautomatisierungs-Struktur neu zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="89ebf-178">When used with an observable array, it will automatically update the UI elements when items are added or removed from the array, without re-creating every element in the UI tree.</span></span> <span data-ttu-id="89ebf-179">Im folgenden Beispiel wird eine neue ViewModel ein Observable Array von Spiel Ergebnisse enthält.</span><span class="sxs-lookup"><span data-stu-id="89ebf-179">The following example uses a new viewModel which includes an observable array of game results.</span></span> <span data-ttu-id="89ebf-180">Sie einer einfachen Tabelle gebunden ist, mit zwei Spalten mit einer `foreach` -Bindung auf die `<tbody>` Element.</span><span class="sxs-lookup"><span data-stu-id="89ebf-180">It is bound to a simple table with two columns using a `foreach` binding on the `<tbody>` element.</span></span> <span data-ttu-id="89ebf-181">Jede `<tr>` Element innerhalb `<tbody>` wird auf ein Element der Auflistung GameResults gebunden werden.</span><span class="sxs-lookup"><span data-stu-id="89ebf-181">Each `<tr>` element within `<tbody>` will be bound to an element of the gameResults collection.</span></span>

```html
<h1>Record</h1>
<table>
    <thead>
        <tr>
            <th>Opponent</th>
            <th>Result</th>
        </tr>
    </thead>
    <tbody data-bind="foreach: gameResults">
        <tr>
            <td data-bind="text:opponent"></td>
            <td data-bind="text:result"></td>
        </tr>
    </tbody>
</table>
<script type="text/javascript">
  function GameResult(opponent, result) {
    var self = this;
    self.opponent = opponent;
    self.result = ko.observable(result);
  }

  function ViewModel() {
    var self = this;

    self.resultChoices = ["Win", "Loss", "Tie"];

    self.gameResults = ko.observableArray([
      new GameResult("Brendan", self.resultChoices[0]),
      new GameResult("Brendan", self.resultChoices[0]),
      new GameResult("Michelle", self.resultChoices[1])
    ]);
  };
  ko.applyBindings(new ViewModel);
</script>
```

<span data-ttu-id="89ebf-182">Beachten Sie, dass dieses Mal wir ViewModel mit dem Großbuchstaben "V" verwenden, da wir erwarten, dass sie mit "new" (im Aufruf ApplyBindings) erstellen.</span><span class="sxs-lookup"><span data-stu-id="89ebf-182">Notice that this time we're using ViewModel with a capital “V" because we expect to construct it using “new" (in the applyBindings call).</span></span> <span data-ttu-id="89ebf-183">Bei der Ausführung Ergebnisseite der in der folgenden Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="89ebf-183">When executed, the page results in the following output:</span></span>

![Knockout Datensatzansichts-Modell](knockout/_static/record-screenshot.png)

<span data-ttu-id="89ebf-185">Um zu zeigen, wird die Funktionsfähigkeit der Observable-Auflistung, fügen Sie ein wenig mehr Funktionalität.</span><span class="sxs-lookup"><span data-stu-id="89ebf-185">To demonstrate that the observable collection is working, let's add a bit more functionality.</span></span> <span data-ttu-id="89ebf-186">Wir können gehören die Fähigkeit zum Aufzeichnen der Ergebnisse von einem anderen Spiel ViewModel, und fügen Sie eine Schaltfläche und einige Elemente der Benutzeroberfläche arbeiten Sie mit dieser neuen Funktion.</span><span class="sxs-lookup"><span data-stu-id="89ebf-186">We can include the ability to record the results of another game to the ViewModel, and then add a button and some UI to work with this new function.</span></span>  <span data-ttu-id="89ebf-187">Zunächst erstellen wir die AddResult-Methode:</span><span class="sxs-lookup"><span data-stu-id="89ebf-187">First, let's create the addResult method:</span></span>

```javascript
// add this to ViewModel()
self.addResult = function() {
  self.gameResults.push(new GameResult("", self.resultChoices[0]));
}
```

<span data-ttu-id="89ebf-188">Binden Sie diese Methode mit einer Schaltfläche mithilfe der `click` Bindung:</span><span class="sxs-lookup"><span data-stu-id="89ebf-188">Bind this method to a button using the `click` binding:</span></span>

```html
<button data-bind="click: addResult">Add New Result</button>
```

<span data-ttu-id="89ebf-189">Öffnen Sie die Seite im Browser, und klicken Sie auf die Schaltfläche ein paar Mal, was zu einer neuen Tabellenzeile mit jedem klicken:</span><span class="sxs-lookup"><span data-stu-id="89ebf-189">Open the page in the browser and click the button a couple of times, resulting in a new table row with each click:</span></span>

![Hinzufügen von Ergebnissen](knockout/_static/record-addresult-screenshot.png)

<span data-ttu-id="89ebf-191">Es gibt einige Möglichkeiten zum Hinzufügen neuer Datensätze in der Benutzeroberfläche, in der Regel entweder Inline oder in einer separaten Form unterstützt.</span><span class="sxs-lookup"><span data-stu-id="89ebf-191">There are a few ways to support adding new records in the UI, typically either inline or in a separate form.</span></span> <span data-ttu-id="89ebf-192">Wir können problemlos ändern Sie die Tabelle, Textfelder und Dropdownlists verwenden, damit alles bearbeitbar ist.</span><span class="sxs-lookup"><span data-stu-id="89ebf-192">We can easily modify the table to use textboxes and dropdownlists so that the whole thing is editable.</span></span> <span data-ttu-id="89ebf-193">Ändern Sie einfach die `<tr>` Element wie gezeigt:</span><span class="sxs-lookup"><span data-stu-id="89ebf-193">Just change the `<tr>` element as shown:</span></span>

```html
<tbody data-bind="foreach: gameResults">
    <tr>
      <td><input data-bind="value:opponent" /></td>
      <td><select data-bind="options: $root.resultChoices, value:result, optionsText: $data"></select></td>
    </tr>
</tbody>
```

<span data-ttu-id="89ebf-194">Beachten Sie, dass `$root` bezieht sich auf den Stamm ViewModel, also, in dem die möglichen Optionen verfügbar gemacht werden.</span><span class="sxs-lookup"><span data-stu-id="89ebf-194">Note that `$root` refers to the root ViewModel, which is where the possible choices are exposed.</span></span> <span data-ttu-id="89ebf-195">`$data`bezieht sich auf das das aktuelle Modell in einem bestimmten Kontext – in diesem Fall ist es auf ein einzelnes Element des Arrays ResultChoices bezieht sich von denen jeder eine einfache Zeichenfolge ist.</span><span class="sxs-lookup"><span data-stu-id="89ebf-195">`$data` refers to whatever the current model is within a given context - in this case it refers to an individual element of the resultChoices array, each of which is a simple string.</span></span>

<span data-ttu-id="89ebf-196">Durch diese Änderung wird das gesamte Raster bearbeitet werden:</span><span class="sxs-lookup"><span data-stu-id="89ebf-196">With this change, the entire grid becomes editable:</span></span>

![Bearbeitbares Raster](knockout/_static/editable-grid-screenshot.png)

<span data-ttu-id="89ebf-198">Wenn wir nicht waren Knockout verwenden, wir konnte erreichen all dies unter Verwendung von jQuery, aber es wahrscheinlich nicht würde annähernd so effizient sein.</span><span class="sxs-lookup"><span data-stu-id="89ebf-198">If we weren't using Knockout, we could achieve all of this using jQuery, but most likely it would not be nearly as efficient.</span></span> <span data-ttu-id="89ebf-199">Knockout verfolgt die gebundenen Daten, die Elemente in das ViewModel entsprechen, welche Elemente der Benutzeroberfläche und aktualisiert nur die Elemente, die hinzugefügt, entfernt oder aktualisiert werden müssen.</span><span class="sxs-lookup"><span data-stu-id="89ebf-199">Knockout tracks which bound data items in the ViewModel correspond to which UI elements, and only updates those elements that need to be added, removed, or updated.</span></span> <span data-ttu-id="89ebf-200">Es würde erheblichen Aufwand Softwareverteilungsfeatures uns mit jQuery oder direkte DOM-Manipulation und selbst dann, wenn wir aggregieren Sie Ergebnisse (z. B. einen Datensatz Win-Verlust) basierend auf Daten der Tabelle anzeigen möchten, wir müssten einmal durchlaufen sie und Analysieren der HTML-Elemente.</span><span class="sxs-lookup"><span data-stu-id="89ebf-200">It would take significant effort to achieve this ourselves using jQuery or direct DOM manipulation, and even then if we then wanted to display aggregate results (such as a win-loss record) based on the table's data, we would need to once more loop through it and parse the HTML elements.</span></span>  <span data-ttu-id="89ebf-201">Mit Knockout ist das Anzeigen des Datensatzes Win-Loss unbedeutend.</span><span class="sxs-lookup"><span data-stu-id="89ebf-201">With Knockout, displaying the win-loss record is trivial.</span></span> <span data-ttu-id="89ebf-202">Sie können Berechnungen innerhalb der ViewModel selbst und zeigen Sie es mit einem einfachen Text-Bindung und einem `<span>`.</span><span class="sxs-lookup"><span data-stu-id="89ebf-202">We can perform the calculations within the ViewModel itself, and then display it with a simple text binding and a `<span>`.</span></span>

<span data-ttu-id="89ebf-203">Um die Zeichenfolge der Gewinn / Verlust Datensatz zu erstellen, können wir eine berechnete Observable-Objekt.</span><span class="sxs-lookup"><span data-stu-id="89ebf-203">To build the win-loss record string, we can use a computed observable.</span></span> <span data-ttu-id="89ebf-204">Beachten Sie, die auf Observable-Eigenschaften in das ViewModel verweist Funktionsaufrufe muss, andernfalls werden sie nicht den Wert der die Observable-Objekt abrufen (d. h. `gameResults()` nicht `gameResults` im Code gezeigt):</span><span class="sxs-lookup"><span data-stu-id="89ebf-204">Note that references to observable properties within the ViewModel must be function calls, otherwise they will not retrieve the value of the observable (i.e. `gameResults()` not `gameResults` in the code shown):</span></span>

```javascript
self.displayRecord = ko.computed(function () {
  var wins = self.gameResults().filter(function (value) { return value.result() == "Win"; }).length;
  var losses = self.gameResults().filter(function (value) { return value.result() == "Loss"; }).length;
  var ties = self.gameResults().filter(function (value) { return value.result() == "Tie"; }).length;
  return wins + " - " + losses + " - " + ties;
}, this);
```

<span data-ttu-id="89ebf-205">Binden Sie diese Funktion auf einen innerhalb der `<h1>` Element am oberen Rand der Seite:</span><span class="sxs-lookup"><span data-stu-id="89ebf-205">Bind this function to a span within the `<h1>` element at the top of the page:</span></span>

```html
<h1>Record <span data-bind="text: displayRecord"></span></h1>
```

<span data-ttu-id="89ebf-206">Das Ergebnis:</span><span class="sxs-lookup"><span data-stu-id="89ebf-206">The result:</span></span>

![Win-Verlust](knockout/_static/record-winloss-screenshot.png)

<span data-ttu-id="89ebf-208">Zeilen hinzufügen oder ändern das ausgewählte Element in der Ergebnisspalte für eine beliebige Zeile aktualisiert den Datensatz, der am oberen Rand des Fensters angezeigt.</span><span class="sxs-lookup"><span data-stu-id="89ebf-208">Adding rows or modifying the selected element in any row's Result column will update the record shown at the top of the window.</span></span>

<span data-ttu-id="89ebf-209">Zusätzlich zur Bindung mit Werten können Sie auch nahezu jede rechtlichen JavaScript-Ausdruck in einer Bindung.</span><span class="sxs-lookup"><span data-stu-id="89ebf-209">In addition to binding to values, you can also use almost any legal JavaScript expression within a binding.</span></span> <span data-ttu-id="89ebf-210">Z. B. wenn ein Element der Benutzeroberfläche nur angezeigt werden sollen, unter bestimmten Umständen kann z. B. wenn ein Wert für einen bestimmten Schwellenwert überschreitet können Sie dies logisch innerhalb der Bindungsausdruck angeben:</span><span class="sxs-lookup"><span data-stu-id="89ebf-210">For example, if a UI element should only appear under certain conditions, such as when a value exceeds a certain threshold, you can specify this logically within the binding expression:</span></span>

```html
<div data-bind="visible: customerValue > 100"></div>
```

<span data-ttu-id="89ebf-211">Dies `<div>` ist nur sichtbar, wenn die CustomerValue mehr als 100 ist.</span><span class="sxs-lookup"><span data-stu-id="89ebf-211">This `<div>` will only be visible when the customerValue is over 100.</span></span>

## <a name="templates"></a><span data-ttu-id="89ebf-212">Vorlagen</span><span class="sxs-lookup"><span data-stu-id="89ebf-212">Templates</span></span>

<span data-ttu-id="89ebf-213">Knockout wurde Unterstützung für Vorlagen, damit Sie problemlos von Ihrem Verhalten, die Benutzeroberfläche trennen oder inkrementellen Laden von UI-Elemente in einer großen Anwendung bei Bedarf können.</span><span class="sxs-lookup"><span data-stu-id="89ebf-213">Knockout has support for templates, so that you can easily separate your UI from your behavior, or incrementally load UI elements into a large application on demand.</span></span> <span data-ttu-id="89ebf-214">Wir können unserer vorherigen Beispiel um jede Zeile eine eigenen Vorlage machen, indem Sie einfach abzufangen HTML out in eine Vorlage, und Angeben der Vorlage nach dem Namen in der Data-Bind-Aufruf auf Aktualisieren `<tbody>`.</span><span class="sxs-lookup"><span data-stu-id="89ebf-214">We can update our previous example to make each row its own template by simply pulling the HTML out into a template and specifying the template by name in the data-bind call on `<tbody>`.</span></span>

```html
<tbody data-bind="template: { name: 'rowTemplate', foreach: gameResults }">
</tbody>
<script type="text/html" id="rowTemplate">
  <tr>
      <td><input data-bind="value:opponent" /></td>
      <td><select data-bind="options: $root.resultChoices, value:result, optionsText: $data"></select></td>
  </tr>
</script>
```

<span data-ttu-id="89ebf-215">Knockout unterstützt auch andere Textvorlagen-Module, wie z. B. der jQuery.tmpl-Bibliothek und des Underscore.js-Vorlagenmodul.</span><span class="sxs-lookup"><span data-stu-id="89ebf-215">Knockout also supports other templating engines, such as the jQuery.tmpl library and Underscore.js's templating engine.</span></span>

## <a name="components"></a><span data-ttu-id="89ebf-216">Komponenten</span><span class="sxs-lookup"><span data-stu-id="89ebf-216">Components</span></span>

<span data-ttu-id="89ebf-217">Komponenten können Sie zum Organisieren und Wiederverwenden von UI-Code in der Regel zusammen mit den ViewModel-Daten, die dem UI-Code abhängt.</span><span class="sxs-lookup"><span data-stu-id="89ebf-217">Components allow you to organize and reuse UI code, usually along with the ViewModel data on which the UI code depends.</span></span> <span data-ttu-id="89ebf-218">Um eine Komponente zu erstellen, müssen Sie einfach zum Angeben der Vorlage und seine ViewModel und geben sie einen Namen.</span><span class="sxs-lookup"><span data-stu-id="89ebf-218">To create a component, you simply need to specify its template and its viewModel, and give it a name.</span></span> <span data-ttu-id="89ebf-219">Hierzu wird `ko.components.register()` aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="89ebf-219">This is done by calling `ko.components.register()`.</span></span> <span data-ttu-id="89ebf-220">Zusätzlich zum Definieren von Vorlagen und Viewmodel Inline, sie können geladen werden aus externen Dateien mit einer Bibliothek wie *"Require.js"*, wodurch sehr sauber und effizienten Codes.</span><span class="sxs-lookup"><span data-stu-id="89ebf-220">In addition to defining the templates and viewmodel inline, they can be loaded from external files using a library like *require.js*, resulting in very clean and efficient code.</span></span>

## <a name="communicating-with-apis"></a><span data-ttu-id="89ebf-221">Bei der Kommunikation mit APIs</span><span class="sxs-lookup"><span data-stu-id="89ebf-221">Communicating with APIs</span></span>

<span data-ttu-id="89ebf-222">Knockout funktionieren mit Daten im JSON-Format.</span><span class="sxs-lookup"><span data-stu-id="89ebf-222">Knockout can work with any data in JSON format.</span></span> <span data-ttu-id="89ebf-223">Ein gängiges Verfahren zum Abrufen und Speichern von Daten mit Knockout mit jQuery, die unterstützt wird die `$.getJSON()` Funktion zum Abrufen von Daten, und die `$.post()` Methode, um Daten über den Browser an einen API-Endpunkt zu senden.</span><span class="sxs-lookup"><span data-stu-id="89ebf-223">A common way to retrieve and save data using Knockout is with jQuery, which supports the `$.getJSON()` function to retrieve data, and the `$.post()` method to send data from the browser to an API endpoint.</span></span> <span data-ttu-id="89ebf-224">Natürlich, wenn Sie eine andere Möglichkeit zum Senden und Empfangen von JSON-Daten lieber, funktioniert Knockout mit ihm auch.</span><span class="sxs-lookup"><span data-stu-id="89ebf-224">Of course, if you prefer a different way to send and receive JSON data, Knockout will work with it as well.</span></span>

## <a name="summary"></a><span data-ttu-id="89ebf-225">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="89ebf-225">Summary</span></span>

<span data-ttu-id="89ebf-226">Knockout bietet eine einfache und elegante Möglichkeit, Elemente der Benutzeroberfläche mit dem aktuellen Status der Clientanwendung, definiert in einem ViewModel gebunden.</span><span class="sxs-lookup"><span data-stu-id="89ebf-226">Knockout provides a simple, elegant way to bind UI elements to the current state of the client application, defined in a ViewModel.</span></span> <span data-ttu-id="89ebf-227">Knockout des Bindungssyntax wird verwendet, das Data-Bind-Attribut, HTML-Elementen, die zu verarbeitende angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="89ebf-227">Knockout's binding syntax uses the data-bind attribute, applied to HTML elements that are to be processed.</span></span> <span data-ttu-id="89ebf-228">Knockout kann effizient Rendern und großen Datasets aktualisieren, indem Sie Benutzeroberflächenelemente nachverfolgen und Verarbeitung nur Änderungen an den betroffenen Elemente.</span><span class="sxs-lookup"><span data-stu-id="89ebf-228">Knockout is able to efficiently render and update large data sets by tracking UI elements and only processing changes to affected elements.</span></span> <span data-ttu-id="89ebf-229">Große Anwendungen können zusammensetzen Benutzeroberflächen-Logik, die mithilfe von Vorlagen und Komponenten, die bei Bedarf aus externen Dateien geladen werden können.</span><span class="sxs-lookup"><span data-stu-id="89ebf-229">Large applications can break up UI logic using templates and components, which can be loaded on demand from external files.</span></span> <span data-ttu-id="89ebf-230">Derzeit ist Version 3, Knockout eine stabile JavaScript-Bibliothek, die Webanwendungen verbessern können, die rich-Client Interaktivität erfordern.</span><span class="sxs-lookup"><span data-stu-id="89ebf-230">Currently version 3, Knockout is a stable JavaScript library that can improve web applications that require rich client interactivity.</span></span>
