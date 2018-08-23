---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: 'Teil 5: Erstellen einer dynamischen Benutzeroberfläche mit Knockout.js | Microsoft-Dokumentation'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: d019941700992e173a5812b11b558b6726dfd809
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831290"
---
<a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a><span data-ttu-id="0fc31-102">Teil 5: Erstellen einer dynamischen Benutzeroberfläche mit Knockout.js</span><span class="sxs-lookup"><span data-stu-id="0fc31-102">Part 5: Creating a Dynamic UI with Knockout.js</span></span>
====================
<span data-ttu-id="0fc31-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="0fc31-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="0fc31-104">Abgeschlossenes Projekt herunterladen</span><span class="sxs-lookup"><span data-stu-id="0fc31-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a><span data-ttu-id="0fc31-105">Erstellen einer dynamischen Benutzeroberfläche mit Knockout.js</span><span class="sxs-lookup"><span data-stu-id="0fc31-105">Creating a Dynamic UI with Knockout.js</span></span>

<span data-ttu-id="0fc31-106">In diesem Abschnitt verwenden wir "Knockout.js" der administratoransicht Funktionen hinzu.</span><span class="sxs-lookup"><span data-stu-id="0fc31-106">In this section, we'll use Knockout.js to add functionality to the Admin view.</span></span>

<span data-ttu-id="0fc31-107">["Knockout.js"](http://knockoutjs.com/) ist eine Javascript-Bibliothek, die einfach HTML-Steuerelemente an Daten gebunden werden können.</span><span class="sxs-lookup"><span data-stu-id="0fc31-107">[Knockout.js](http://knockoutjs.com/) is a Javascript library that makes it easy to bind HTML controls to data.</span></span> <span data-ttu-id="0fc31-108">"Knockout.js" wird das Model-View-ViewModel (MVVM)-Muster verwendet.</span><span class="sxs-lookup"><span data-stu-id="0fc31-108">Knockout.js uses the Model-View-ViewModel (MVVM) pattern.</span></span>

- <span data-ttu-id="0fc31-109">Die *Modell* ist die serverseitige Darstellung der Daten in der Business-Domäne (in unserem Fall Produkte und Bestellungen).</span><span class="sxs-lookup"><span data-stu-id="0fc31-109">The *model* is the server-side representation of the data in the business domain (in our case, products and orders).</span></span>
- <span data-ttu-id="0fc31-110">Die *Ansicht* spielt die Darstellungsschicht (HTML).</span><span class="sxs-lookup"><span data-stu-id="0fc31-110">The *view* is the presentation layer (HTML).</span></span>
- <span data-ttu-id="0fc31-111">Die *Anzeigemodell* ist ein Javascript-Objekt, das Modelldaten enthält.</span><span class="sxs-lookup"><span data-stu-id="0fc31-111">The *view-model* is a Javascript object that holds the model data.</span></span> <span data-ttu-id="0fc31-112">Das Ansichtsmodell ist eine Abstraktion der Code der Benutzeroberfläche.</span><span class="sxs-lookup"><span data-stu-id="0fc31-112">The view-model is a code abstraction of the UI.</span></span> <span data-ttu-id="0fc31-113">Es wurde keine Kenntnisse über die HTML-Darstellung.</span><span class="sxs-lookup"><span data-stu-id="0fc31-113">It has no knowledge of the HTML representation.</span></span> <span data-ttu-id="0fc31-114">Stattdessen stellt es sich um abstrakte Funktionen in der Ansicht, z. B. "eine Liste von Elementen".</span><span class="sxs-lookup"><span data-stu-id="0fc31-114">Instead, it represents abstract features of the view, such as "a list of items".</span></span>

<span data-ttu-id="0fc31-115">Die Ansicht an das Ansichtsmodell datengebunden ist.</span><span class="sxs-lookup"><span data-stu-id="0fc31-115">The view is data-bound to the view-model.</span></span> <span data-ttu-id="0fc31-116">Updates an das Ansichtsmodell werden in der Ansicht automatisch wiedergegeben.</span><span class="sxs-lookup"><span data-stu-id="0fc31-116">Updates to the view-model are automatically reflected in the view.</span></span> <span data-ttu-id="0fc31-117">Das Ansichtsmodell ist außerdem ruft Ereignisse aus der Sicht, z. B. Klicks ab und führt Vorgänge für das Modell, z. B. einen Auftrag zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="0fc31-117">The view-model also gets events from the view, such as button clicks, and performs operations on the model, such as creating an order.</span></span>

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

<span data-ttu-id="0fc31-118">Zuerst definieren wir das Ansichtsmodell.</span><span class="sxs-lookup"><span data-stu-id="0fc31-118">First we'll define the view-model.</span></span> <span data-ttu-id="0fc31-119">Danach werden wir das HTML-Markup an das Ansichtsmodell binden.</span><span class="sxs-lookup"><span data-stu-id="0fc31-119">After that, we will bind the HTML markup to the view-model.</span></span>

<span data-ttu-id="0fc31-120">Fügen Sie den folgenden Razor-Abschnitt, um Admin.cshtml:</span><span class="sxs-lookup"><span data-stu-id="0fc31-120">Add the following Razor section to Admin.cshtml:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

<span data-ttu-id="0fc31-121">Sie können eine beliebige Stelle in diesem Abschnitt in der Datei hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="0fc31-121">You can add this section anywhere in the file.</span></span> <span data-ttu-id="0fc31-122">Wenn die Ansicht der Bereich wird am unteren Rand der HTML-Seite gerendert wird, nach rechts vor dem schließenden &lt;/body&gt; Tag.</span><span class="sxs-lookup"><span data-stu-id="0fc31-122">When the view is rendered, the section appears at the bottom of the HTML page, right before the closing &lt;/body&gt; tag.</span></span>

<span data-ttu-id="0fc31-123">All das Skript für diese Seite gelangen in die Skript-Tag, die durch den Kommentar angegeben:</span><span class="sxs-lookup"><span data-stu-id="0fc31-123">All of the script for this page will go inside the script tag indicated by the comment:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

<span data-ttu-id="0fc31-124">Definieren Sie zuerst eine Ansichtsmodell Klasse:</span><span class="sxs-lookup"><span data-stu-id="0fc31-124">First, define a view-model class:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

<span data-ttu-id="0fc31-125">**ko.observableArray** ist eine besondere Art von Objekt in Knockout bezeichnet ein *Observable*.</span><span class="sxs-lookup"><span data-stu-id="0fc31-125">**ko.observableArray** is a special kind of object in Knockout, called an *observable*.</span></span> <span data-ttu-id="0fc31-126">Von der ["Knockout.js" Dokumentation](http://knockoutjs.com/documentation/observables.html): ein beobachtbares Objekt ist ein "JavaScript-Objekt, mit denen Abonnenten über Änderungen benachrichtigt werden kann."</span><span class="sxs-lookup"><span data-stu-id="0fc31-126">From the [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html): An observable is a "JavaScript object that can notify subscribers about changes."</span></span> <span data-ttu-id="0fc31-127">Wenn der Inhalt der Observable-Objekt ändern, wird die Ansicht automatisch entsprechend aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="0fc31-127">When the contents of an observable change, the view is automatically updated to match.</span></span>

<span data-ttu-id="0fc31-128">Zum Auffüllen der `products` Arrays, eine AJAX-Anforderung an die Web-API vornehmen.</span><span class="sxs-lookup"><span data-stu-id="0fc31-128">To populate the `products` array, make an AJAX request to the web API.</span></span> <span data-ttu-id="0fc31-129">Denken Sie daran, dass wir den Basis-URI für die API in der ansichtsbehälter gespeichert (siehe [Teil 4](using-web-api-with-entity-framework-part-4.md) des Lernprogramms).</span><span class="sxs-lookup"><span data-stu-id="0fc31-129">Recall that we stored the base URI for the API in the view bag (see [Part 4](using-web-api-with-entity-framework-part-4.md) of the tutorial).</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

<span data-ttu-id="0fc31-130">Als Nächstes fügen Sie Funktionen hinzu, an das Ansichtsmodell erstellen, aktualisieren und Löschen von Produkten.</span><span class="sxs-lookup"><span data-stu-id="0fc31-130">Next, add functions to the view-model to create, update, and delete products.</span></span> <span data-ttu-id="0fc31-131">Diese Funktionen AJAX-Aufrufe an die Web-API zu übermitteln, und verwenden Sie die Ergebnisse des anzeigemodells zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="0fc31-131">These functions submit AJAX calls to the web API and use the results to update the view-model.</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

<span data-ttu-id="0fc31-132">Jetzt der wichtigste Teil: Wenn das DOM ist fulled geladen und Aufruf der **ko.applyBindings** Funktion, und übergeben Sie eine neue Instanz der der `ProductsViewModel`:</span><span class="sxs-lookup"><span data-stu-id="0fc31-132">Now the most important part: When the DOM is fulled loaded, call the **ko.applyBindings** function and pass in a new instance of the `ProductsViewModel`:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

<span data-ttu-id="0fc31-133">Die **ko.applyBindings** Methode Knockout aktiviert und für die Ansicht das Ansichtsmodell verbindet.</span><span class="sxs-lookup"><span data-stu-id="0fc31-133">The **ko.applyBindings** method activates Knockout and wires up the view-model to the view.</span></span>

<span data-ttu-id="0fc31-134">Nun, wir ein Ansichtsmodell haben, können wir die Bindungen erstellen.</span><span class="sxs-lookup"><span data-stu-id="0fc31-134">Now that we have a view-model, we can create the bindings.</span></span> <span data-ttu-id="0fc31-135">In "Knockout.js", erreichen Sie dies durch Hinzufügen von `data-bind` Attribute auf HTML-Elemente.</span><span class="sxs-lookup"><span data-stu-id="0fc31-135">In Knockout.js, you do this by adding `data-bind` attributes to HTML elements.</span></span> <span data-ttu-id="0fc31-136">Um eine HTML-Liste in ein Array zu binden, verwenden Sie z. B. die `foreach` Bindung:</span><span class="sxs-lookup"><span data-stu-id="0fc31-136">For example, to bind an HTML list to an array, use the `foreach` binding:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

<span data-ttu-id="0fc31-137">Die `foreach` Bindung durchläuft das Array und erstellt Sie untergeordnete Elemente für jedes Objekt im Array.</span><span class="sxs-lookup"><span data-stu-id="0fc31-137">The `foreach` binding iterates through the array and creates child elements for each object in the array.</span></span> <span data-ttu-id="0fc31-138">Bindungen an die untergeordneten Elemente können auf Eigenschaften für die Arrayobjekte verweisen.</span><span class="sxs-lookup"><span data-stu-id="0fc31-138">Bindings on the child elements can refer to properties on the array objects.</span></span>

<span data-ttu-id="0fc31-139">Fügen Sie der Liste "Update-Produkte" die folgenden Bindungen hinzu:</span><span class="sxs-lookup"><span data-stu-id="0fc31-139">Add the following bindings to the "update-products" list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

<span data-ttu-id="0fc31-140">Die `<li>` Element innerhalb des Bereichs befindet, die **Foreach** Bindung.</span><span class="sxs-lookup"><span data-stu-id="0fc31-140">The `<li>` element occurs within the scope of the **foreach** binding.</span></span> <span data-ttu-id="0fc31-141">Bedeutet, dass Knockout wird das Element einmal für jedes Produkt im Rendern der `products` Array.</span><span class="sxs-lookup"><span data-stu-id="0fc31-141">That means Knockout will render the element once for each product in the `products` array.</span></span> <span data-ttu-id="0fc31-142">Alle Bindungen innerhalb der `<li>` -Element auf die produktinstanz verweisen.</span><span class="sxs-lookup"><span data-stu-id="0fc31-142">All of the bindings within the `<li>` element refer to that product instance.</span></span> <span data-ttu-id="0fc31-143">Z. B. `$data.Name` bezieht sich auf die `Name` Eigenschaft für das Produkt.</span><span class="sxs-lookup"><span data-stu-id="0fc31-143">For example, `$data.Name` refers to the `Name` property on the product.</span></span>

<span data-ttu-id="0fc31-144">Verwenden Sie zum Festlegen der Werte, der die Texteingaben der `value` Bindung.</span><span class="sxs-lookup"><span data-stu-id="0fc31-144">To set the values of the text inputs, use the `value` binding.</span></span> <span data-ttu-id="0fc31-145">Die Schaltflächen an Funktionen gebunden sind, in der Modell-Ansicht mit den `click` Bindung.</span><span class="sxs-lookup"><span data-stu-id="0fc31-145">The buttons are bound to functions on the model-view, using the `click` binding.</span></span> <span data-ttu-id="0fc31-146">Der Product-Instanz wird für jede Funktion als Parameter übergeben.</span><span class="sxs-lookup"><span data-stu-id="0fc31-146">The product instance is passed as a parameter to each function.</span></span> <span data-ttu-id="0fc31-147">Weitere Informationen die ["Knockout.js" Dokumentation](http://knockoutjs.com/documentation/observables.html) gute Beschreibungen der verschiedenen Bindungen hat.</span><span class="sxs-lookup"><span data-stu-id="0fc31-147">For more information, the [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html) has good descriptions of the various bindings.</span></span>

<span data-ttu-id="0fc31-148">Fügen Sie eine Bindung für die **übermitteln** Ereignis auf dem Formular Produkt hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="0fc31-148">Next, add a binding for the **submit** event on the Add Product form:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

<span data-ttu-id="0fc31-149">Diese Bindung ruft die `create` Funktion im Ansichtsmodell, beim Erstellen eines neuen Produkts.</span><span class="sxs-lookup"><span data-stu-id="0fc31-149">This binding calls the `create` function on the view-model to create a new product.</span></span>

<span data-ttu-id="0fc31-150">Hier ist der vollständige Code für das anzeigen (Administrator):</span><span class="sxs-lookup"><span data-stu-id="0fc31-150">Here is the complete code for the Admin view:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

<span data-ttu-id="0fc31-151">Führen Sie die Anwendung aus, melden Sie sich mit dem Administratorkonto an, und klicken Sie auf den Link "Admin".</span><span class="sxs-lookup"><span data-stu-id="0fc31-151">Run the application, log in with the Administrator account, and click the "Admin" link.</span></span> <span data-ttu-id="0fc31-152">Sie sollten finden Sie unter der Liste der Produkte und in der Lage zu erstellen, aktualisieren oder Löschen von Produkten.</span><span class="sxs-lookup"><span data-stu-id="0fc31-152">You should see the list of products, and be able to create, update, or delete products.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0fc31-153">[Zurück](using-web-api-with-entity-framework-part-4.md)
> [Weiter](using-web-api-with-entity-framework-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="0fc31-153">[Previous](using-web-api-with-entity-framework-part-4.md)
[Next](using-web-api-with-entity-framework-part-6.md)</span></span>
