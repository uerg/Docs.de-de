---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: 'Teil 5: Erstellen einer dynamischen Benutzeroberfläche mit Knockout.js | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: b63446d076fbb1143641dead788042967b996bf8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a><span data-ttu-id="574f1-102">Teil 5: Erstellen einer dynamischen Benutzeroberfläche mit Knockout.js</span><span class="sxs-lookup"><span data-stu-id="574f1-102">Part 5: Creating a Dynamic UI with Knockout.js</span></span>
====================
<span data-ttu-id="574f1-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="574f1-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="574f1-104">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="574f1-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a><span data-ttu-id="574f1-105">Erstellen einer dynamischen Benutzeroberfläche mit Knockout.js</span><span class="sxs-lookup"><span data-stu-id="574f1-105">Creating a Dynamic UI with Knockout.js</span></span>

<span data-ttu-id="574f1-106">In diesem Abschnitt werden wir Knockout.js verwenden, um die Funktionalität der Admin-Ansicht hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="574f1-106">In this section, we'll use Knockout.js to add functionality to the Admin view.</span></span>

<span data-ttu-id="574f1-107">[Knockout.js](http://knockoutjs.com/) ist eine Javascript-Bibliothek, die vereinfacht, HTML-Steuerelementen an Daten gebunden werden.</span><span class="sxs-lookup"><span data-stu-id="574f1-107">[Knockout.js](http://knockoutjs.com/) is a Javascript library that makes it easy to bind HTML controls to data.</span></span> <span data-ttu-id="574f1-108">Knockout.js verwendet das Model-View-ViewModel (MVVM)-Muster.</span><span class="sxs-lookup"><span data-stu-id="574f1-108">Knockout.js uses the Model-View-ViewModel (MVVM) pattern.</span></span>

- <span data-ttu-id="574f1-109">Die *Modell* ist die serverseitige Darstellung der Daten in der Business-Domäne (in unserem Fall Products und Orders).</span><span class="sxs-lookup"><span data-stu-id="574f1-109">The *model* is the server-side representation of the data in the business domain (in our case, products and orders).</span></span>
- <span data-ttu-id="574f1-110">Die *Ansicht* ist die Darstellungsschicht (HTML).</span><span class="sxs-lookup"><span data-stu-id="574f1-110">The *view* is the presentation layer (HTML).</span></span>
- <span data-ttu-id="574f1-111">Die *ViewModel* ist ein Javascript-Objekt, das die Modelldaten enthält.</span><span class="sxs-lookup"><span data-stu-id="574f1-111">The *view-model* is a Javascript object that holds the model data.</span></span> <span data-ttu-id="574f1-112">Das Ansichtsmodell ist eine Abstraktion der Code der Benutzeroberfläche.</span><span class="sxs-lookup"><span data-stu-id="574f1-112">The view-model is a code abstraction of the UI.</span></span> <span data-ttu-id="574f1-113">Sie hat keine Kenntnis der HTML-Darstellung.</span><span class="sxs-lookup"><span data-stu-id="574f1-113">It has no knowledge of the HTML representation.</span></span> <span data-ttu-id="574f1-114">Stattdessen repräsentiert die abstrakte Funktionen von der Sicht, z. B. "eine Liste von Elementen".</span><span class="sxs-lookup"><span data-stu-id="574f1-114">Instead, it represents abstract features of the view, such as "a list of items".</span></span>

<span data-ttu-id="574f1-115">Die Ansicht für das Ansichtsmodell datengebunden ist.</span><span class="sxs-lookup"><span data-stu-id="574f1-115">The view is data-bound to the view-model.</span></span> <span data-ttu-id="574f1-116">Updates für das ViewModel werden automatisch in der Sicht widergespiegelt.</span><span class="sxs-lookup"><span data-stu-id="574f1-116">Updates to the view-model are automatically reflected in the view.</span></span> <span data-ttu-id="574f1-117">Das Ansichtsmodell auch ruft Ereignisse aus der Sicht, z. B. auf eine Schaltfläche, ab und führt Vorgänge für das Modell, z. B. das Erstellen einer Bestellung.</span><span class="sxs-lookup"><span data-stu-id="574f1-117">The view-model also gets events from the view, such as button clicks, and performs operations on the model, such as creating an order.</span></span>

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

<span data-ttu-id="574f1-118">Definieren zunächst das Ansichtsmodell.</span><span class="sxs-lookup"><span data-stu-id="574f1-118">First we'll define the view-model.</span></span> <span data-ttu-id="574f1-119">Danach werden wir das HTML-Markup für das Ansichtsmodell binden.</span><span class="sxs-lookup"><span data-stu-id="574f1-119">After that, we will bind the HTML markup to the view-model.</span></span>

<span data-ttu-id="574f1-120">Fügen Sie den folgenden Razor-Abschnitt, um Admin.cshtml:</span><span class="sxs-lookup"><span data-stu-id="574f1-120">Add the following Razor section to Admin.cshtml:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

<span data-ttu-id="574f1-121">Sie können eine beliebige Stelle in diesem Abschnitt in der Datei hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="574f1-121">You can add this section anywhere in the file.</span></span> <span data-ttu-id="574f1-122">Wenn die Ansicht der Abschnitt wird am unteren Rand der HTML-Seite angezeigt gerendert wird, mit der rechten Maustaste vor dem abschließenden &lt;/body&gt; Tag.</span><span class="sxs-lookup"><span data-stu-id="574f1-122">When the view is rendered, the section appears at the bottom of the HTML page, right before the closing &lt;/body&gt; tag.</span></span>

<span data-ttu-id="574f1-123">Alle des Skripts für diese Seite wird innerhalb des Kommentars erkennbar Skripttags gesendet:</span><span class="sxs-lookup"><span data-stu-id="574f1-123">All of the script for this page will go inside the script tag indicated by the comment:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

<span data-ttu-id="574f1-124">Zuerst definieren Sie eine Ansichtsmodell Klasse:</span><span class="sxs-lookup"><span data-stu-id="574f1-124">First, define a view-model class:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

<span data-ttu-id="574f1-125">**ko.observableArray** ist eine besondere Art von Objekt in Knockout, bezeichnet ein *Observable*.</span><span class="sxs-lookup"><span data-stu-id="574f1-125">**ko.observableArray** is a special kind of object in Knockout, called an *observable*.</span></span> <span data-ttu-id="574f1-126">Aus der [Knockout.js Dokumentation](http://knockoutjs.com/documentation/observables.html): Observable-Objekt ist ein "JavaScript-Objekt, mit denen Abonnenten über Änderungen benachrichtigt werden kann."</span><span class="sxs-lookup"><span data-stu-id="574f1-126">From the [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html): An observable is a "JavaScript object that can notify subscribers about changes."</span></span> <span data-ttu-id="574f1-127">Wenn der Inhalt der Observable-Objekt ändern, wird die Ansicht automatisch entsprechend aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="574f1-127">When the contents of an observable change, the view is automatically updated to match.</span></span>

<span data-ttu-id="574f1-128">Zum Auffüllen der `products` Arrays, nehmen Sie eine AJAX-Anforderung an die Web-API.</span><span class="sxs-lookup"><span data-stu-id="574f1-128">To populate the `products` array, make an AJAX request to the web API.</span></span> <span data-ttu-id="574f1-129">Beachten Sie, dass wir den Basis-URI für die API in den ansichtsbehälter gespeichert (siehe [Teil 4](using-web-api-with-entity-framework-part-4.md) des Lernprogramms).</span><span class="sxs-lookup"><span data-stu-id="574f1-129">Recall that we stored the base URI for the API in the view bag (see [Part 4](using-web-api-with-entity-framework-part-4.md) of the tutorial).</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

<span data-ttu-id="574f1-130">Fügen Sie anschließend die Funktionen zum Erstellen, aktualisieren und Löschen von Produkten Modell anzeigen.</span><span class="sxs-lookup"><span data-stu-id="574f1-130">Next, add functions to the view-model to create, update, and delete products.</span></span> <span data-ttu-id="574f1-131">Diese Funktionen AJAX-Aufrufe an die Web-API senden, und verwenden die Ergebnisse der anzeigen-Modell zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="574f1-131">These functions submit AJAX calls to the web API and use the results to update the view-model.</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

<span data-ttu-id="574f1-132">Jetzt die wichtigste Schritt: Wenn das DOM ist fulled geladen und Aufruf der **ko.applyBindings** Funktion, und übergeben Sie eine neue Instanz der der `ProductsViewModel`:</span><span class="sxs-lookup"><span data-stu-id="574f1-132">Now the most important part: When the DOM is fulled loaded, call the **ko.applyBindings** function and pass in a new instance of the `ProductsViewModel`:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

<span data-ttu-id="574f1-133">Die **ko.applyBindings** Methode Knockout aktiviert und auf die Ansicht das Ansichtsmodell verbindet.</span><span class="sxs-lookup"><span data-stu-id="574f1-133">The **ko.applyBindings** method activates Knockout and wires up the view-model to the view.</span></span>

<span data-ttu-id="574f1-134">Nun mit dem Modell anzeigen, können wir die Bindungen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="574f1-134">Now that we have a view-model, we can create the bindings.</span></span> <span data-ttu-id="574f1-135">In Knockout.js, Sie hierzu fügen `data-bind` -Attribute verwenden, um HTML-Elemente.</span><span class="sxs-lookup"><span data-stu-id="574f1-135">In Knockout.js, you do this by adding `data-bind` attributes to HTML elements.</span></span> <span data-ttu-id="574f1-136">Beispielsweise verwenden, um eine HTML-Liste in ein Array zu binden, die `foreach` Bindung:</span><span class="sxs-lookup"><span data-stu-id="574f1-136">For example, to bind an HTML list to an array, use the `foreach` binding:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

<span data-ttu-id="574f1-137">Die `foreach` Bindung durchläuft das Array und erstellt Sie untergeordnete Elemente für jedes Objekt im Array.</span><span class="sxs-lookup"><span data-stu-id="574f1-137">The `foreach` binding iterates through the array and creates child elements for each object in the array.</span></span> <span data-ttu-id="574f1-138">Bindungen für die untergeordneten Elemente verweisen auf Eigenschaften auf das Array von Objekten.</span><span class="sxs-lookup"><span data-stu-id="574f1-138">Bindings on the child elements can refer to properties on the array objects.</span></span>

<span data-ttu-id="574f1-139">Fügen Sie der Liste "Updateprodukten" die folgenden Bindungen hinzu:</span><span class="sxs-lookup"><span data-stu-id="574f1-139">Add the following bindings to the "update-products" list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

<span data-ttu-id="574f1-140">Die `<li>` Element befindet sich innerhalb des Bereichs der **Foreach** Bindung.</span><span class="sxs-lookup"><span data-stu-id="574f1-140">The `<li>` element occurs within the scope of the **foreach** binding.</span></span> <span data-ttu-id="574f1-141">Dass bedeutet, dass Knockout wird das Element einmal für jedes Produkt in gerendert werden die `products` Array.</span><span class="sxs-lookup"><span data-stu-id="574f1-141">That means Knockout will render the element once for each product in the `products` array.</span></span> <span data-ttu-id="574f1-142">Alle Bindungen innerhalb der `<li>` Element verweisen auf diese produktinstanz.</span><span class="sxs-lookup"><span data-stu-id="574f1-142">All of the bindings within the `<li>` element refer to that product instance.</span></span> <span data-ttu-id="574f1-143">Beispielsweise `$data.Name` bezieht sich auf die `Name` auf der Produkt-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="574f1-143">For example, `$data.Name` refers to the `Name` property on the product.</span></span>

<span data-ttu-id="574f1-144">Verwenden Sie zum Festlegen der Werte von der Texteingaben der `value` Bindung.</span><span class="sxs-lookup"><span data-stu-id="574f1-144">To set the values of the text inputs, use the `value` binding.</span></span> <span data-ttu-id="574f1-145">Die Schaltflächen an Funktionen gebunden sind, auf die Modellansicht mithilfe der `click` Bindung.</span><span class="sxs-lookup"><span data-stu-id="574f1-145">The buttons are bound to functions on the model-view, using the `click` binding.</span></span> <span data-ttu-id="574f1-146">Der Product-Instanz wird für jede Funktion als Parameter übergeben.</span><span class="sxs-lookup"><span data-stu-id="574f1-146">The product instance is passed as a parameter to each function.</span></span> <span data-ttu-id="574f1-147">Weitere Informationen die [Knockout.js Dokumentation](http://knockoutjs.com/documentation/observables.html) gute Beschreibungen der verschiedenen Bindungen hat.</span><span class="sxs-lookup"><span data-stu-id="574f1-147">For more information, the [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html) has good descriptions of the various bindings.</span></span>

<span data-ttu-id="574f1-148">Fügen Sie eine Bindung für die **übermitteln** Ereignis auf dem Formular Produkt hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="574f1-148">Next, add a binding for the **submit** event on the Add Product form:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

<span data-ttu-id="574f1-149">Diese Bindung ruft die `create` Funktion auf das Ansichtsmodell, erstellen Sie ein neues Produkt.</span><span class="sxs-lookup"><span data-stu-id="574f1-149">This binding calls the `create` function on the view-model to create a new product.</span></span>

<span data-ttu-id="574f1-150">Hier wird der vollständige Code für die Admin-Ansicht:</span><span class="sxs-lookup"><span data-stu-id="574f1-150">Here is the complete code for the Admin view:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

<span data-ttu-id="574f1-151">Führen Sie die Anwendung, melden Sie sich mit dem Administratorkonto an, und klicken Sie auf den Link "Admin".</span><span class="sxs-lookup"><span data-stu-id="574f1-151">Run the application, log in with the Administrator account, and click the "Admin" link.</span></span> <span data-ttu-id="574f1-152">Sie sollten finden Sie in der Liste der Produkte und in der Lage, erstellen, aktualisieren oder Löschen von Produkten.</span><span class="sxs-lookup"><span data-stu-id="574f1-152">You should see the list of products, and be able to create, update, or delete products.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="574f1-153">[Zurück](using-web-api-with-entity-framework-part-4.md)
> [Weiter](using-web-api-with-entity-framework-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="574f1-153">[Previous](using-web-api-with-entity-framework-part-4.md)
[Next](using-web-api-with-entity-framework-part-6.md)</span></span>
