---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: 'Teil 7: Erstellen den Hauptknoten Seite | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: 2c378e68a1e6600daf655c19afbfe355e89496d4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="part-7-creating-the-main-page"></a><span data-ttu-id="4f1bf-102">Teil 7: Erstellen den Hauptknoten Seite</span><span class="sxs-lookup"><span data-stu-id="4f1bf-102">Part 7: Creating the Main Page</span></span>
====================
<span data-ttu-id="4f1bf-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="4f1bf-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="4f1bf-104">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="4f1bf-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a><span data-ttu-id="4f1bf-105">Erstellen den Hauptknoten Seite</span><span class="sxs-lookup"><span data-stu-id="4f1bf-105">Creating the Main Page</span></span>

<span data-ttu-id="4f1bf-106">In diesem Abschnitt erstellen Sie die Seite "hauptanwendung".</span><span class="sxs-lookup"><span data-stu-id="4f1bf-106">In this section, you will create the main application page.</span></span> <span data-ttu-id="4f1bf-107">Auf dieser Seite wird komplexer als die Seite "Administrator" sein, damit wir es in mehreren Schritten Ansatz wird.</span><span class="sxs-lookup"><span data-stu-id="4f1bf-107">This page will be more complex than the Admin page, so we'll approach it in several steps.</span></span> <span data-ttu-id="4f1bf-108">Nebenbei sehen Sie, einige erweiterten Knockout.js Techniken.</span><span class="sxs-lookup"><span data-stu-id="4f1bf-108">Along the way, you'll see some more advanced Knockout.js techniques.</span></span> <span data-ttu-id="4f1bf-109">Hier wird das grundlegende Layout der Seite:</span><span class="sxs-lookup"><span data-stu-id="4f1bf-109">Here is the basic layout of the page:</span></span>

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- <span data-ttu-id="4f1bf-110">"Products" enthält ein Array von Produkten.</span><span class="sxs-lookup"><span data-stu-id="4f1bf-110">"Products" holds an array of products.</span></span>
- <span data-ttu-id="4f1bf-111">"Einkaufswagen" enthält ein Array von Produkten mit Mengen.</span><span class="sxs-lookup"><span data-stu-id="4f1bf-111">"Cart" holds an array of products with quantities.</span></span> <span data-ttu-id="4f1bf-112">Klicken auf "Add to Cart" wird der Einkaufswagen aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="4f1bf-112">Clicking "Add to Cart" updates the cart.</span></span>
- <span data-ttu-id="4f1bf-113">"Orders" enthält ein Array von bestellungs-IDs.</span><span class="sxs-lookup"><span data-stu-id="4f1bf-113">"Orders" holds an array of order IDs.</span></span>
- <span data-ttu-id="4f1bf-114">"Details" enthält ein Order Details anzeigen, die ein Array von Elementen (Produkte mit Mengen) ist</span><span class="sxs-lookup"><span data-stu-id="4f1bf-114">"Details" holds an order detail, which is an array of items (products with quantities)</span></span>

<span data-ttu-id="4f1bf-115">Wir beginnen mit einige grundlegende Layout in HTML, ohne die Datenbindung oder das Skript definieren.</span><span class="sxs-lookup"><span data-stu-id="4f1bf-115">We'll start by defining some basic layout in HTML, with no data binding or script.</span></span> <span data-ttu-id="4f1bf-116">Öffnen Sie die Datei Views/Home/Index.cshtml, und Ersetzen Sie den Inhalt durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="4f1bf-116">Open the file Views/Home/Index.cshtml and replace all of the contents with the following:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

<span data-ttu-id="4f1bf-117">Als Nächstes fügen Sie einen Abschnitt zu Skripts hinzu, und erstellen Sie ein leeres Modell anzeigen:</span><span class="sxs-lookup"><span data-stu-id="4f1bf-117">Next, add a Scripts section and create an empty view-model:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

<span data-ttu-id="4f1bf-118">Unsere Ansichtsmodell benötigt basierend auf den Entwurf, die zuvor skizziert, eine Wahrnehmbare Elemente für Produkte, Einkaufswagen, Aufträge und Details.</span><span class="sxs-lookup"><span data-stu-id="4f1bf-118">Based on the design sketched earlier, our view model needs observables for products, cart, orders, and details.</span></span> <span data-ttu-id="4f1bf-119">Fügen Sie die folgenden Variablen, die `AppViewModel` Objekt:</span><span class="sxs-lookup"><span data-stu-id="4f1bf-119">Add the following variables to the `AppViewModel` object:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

<span data-ttu-id="4f1bf-120">Benutzer können Elemente aus der Produktliste in den Einkaufswagen hinzufügen und Entfernen von Elementen aus dem Einkaufswagen.</span><span class="sxs-lookup"><span data-stu-id="4f1bf-120">Users can add items from the products list into the cart, and remove items from the cart.</span></span> <span data-ttu-id="4f1bf-121">Um diese Funktionen zu kapseln, erstellen wir eine andere ViewModel Klasse, die ein Produkt darstellt.</span><span class="sxs-lookup"><span data-stu-id="4f1bf-121">To encapsulate these functions, we'll create another view-model class that represents a product.</span></span> <span data-ttu-id="4f1bf-122">Fügen Sie den folgenden Code zu `AppViewModel` hinzu:</span><span class="sxs-lookup"><span data-stu-id="4f1bf-122">Add the following code to `AppViewModel`:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

<span data-ttu-id="4f1bf-123">Die `ProductViewModel` Klasse enthält zwei Funktionen, die verwendet werden, um das Produkt zu und aus dem Einkaufswagen zu verschieben: `addItemToCart` Fügt eine Einheit des Produkts zum Einkaufswagen und `removeAllFromCart` entfernt alle Mengen des Produkts.</span><span class="sxs-lookup"><span data-stu-id="4f1bf-123">The `ProductViewModel` class contains two functions that are used to move the product to and from the cart: `addItemToCart` adds one unit of the product to the cart, and `removeAllFromCart` removes all quantities of the product.</span></span>

<span data-ttu-id="4f1bf-124">Benutzer können wählen Sie einen vorhandenen Auftrag und erhalten die Auftragsdetails enthält.</span><span class="sxs-lookup"><span data-stu-id="4f1bf-124">Users can select an existing order and get the order details.</span></span> <span data-ttu-id="4f1bf-125">Wir werden diese Funktionen in einer anderen Ansichtsmodell kapseln:</span><span class="sxs-lookup"><span data-stu-id="4f1bf-125">We'll encapsulate this functionality into another view-model:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

<span data-ttu-id="4f1bf-126">Die `OrderDetailsViewModel` wird initialisiert, indem eine Bestellung und den Bestellungsdetails durch Senden einer AJAX-Anforderung an den Server abgerufen.</span><span class="sxs-lookup"><span data-stu-id="4f1bf-126">The `OrderDetailsViewModel` is initialized with an order, and it fetches the order details by sending an AJAX request to the server.</span></span>

<span data-ttu-id="4f1bf-127">Beachten Sie auch, dass die `total` Eigenschaft auf die `OrderDetailsViewModel`.</span><span class="sxs-lookup"><span data-stu-id="4f1bf-127">Also, notice the `total` property on the `OrderDetailsViewModel`.</span></span> <span data-ttu-id="4f1bf-128">Diese Eigenschaft ist eine spezielle Art von Observable-Objekt aufgerufen, eine [Observable-Objekt berechnet](http://knockoutjs.com/documentation/computedObservables.html).</span><span class="sxs-lookup"><span data-stu-id="4f1bf-128">This property is a special kind of observable called a [computed observable](http://knockoutjs.com/documentation/computedObservables.html).</span></span> <span data-ttu-id="4f1bf-129">Wie der Name schon sagt, eine berechnete Observable-Objekt können Sie Daten binden an einen berechneten Wert&#8212;in diesem Fall die Gesamtkosten der Bestellung.</span><span class="sxs-lookup"><span data-stu-id="4f1bf-129">As the name implies, a computed observable lets you data bind to a computed value&#8212;in this case, the total cost of the order.</span></span>

<span data-ttu-id="4f1bf-130">Als Nächstes fügen Sie diese Funktionen `AppViewModel`:</span><span class="sxs-lookup"><span data-stu-id="4f1bf-130">Next, add these functions to `AppViewModel`:</span></span>

- <span data-ttu-id="4f1bf-131">`resetCart` Entfernt alle Elemente aus dem Einkaufswagen.</span><span class="sxs-lookup"><span data-stu-id="4f1bf-131">`resetCart` removes all items from the cart.</span></span>
- <span data-ttu-id="4f1bf-132">`getDetails` Ruft die Details einer Bestellung (durch ein neues Pusing `OrderDetailsViewModel` auf die `details` Liste).</span><span class="sxs-lookup"><span data-stu-id="4f1bf-132">`getDetails` gets the details for an order (by pusing a new `OrderDetailsViewModel` onto the `details` list).</span></span>
- <span data-ttu-id="4f1bf-133">`createOrder` erstellt eine neue Bestellung und leert den Einkaufswagen.</span><span class="sxs-lookup"><span data-stu-id="4f1bf-133">`createOrder` creates a new order and empties the cart.</span></span>


[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

<span data-ttu-id="4f1bf-134">Initialisieren Sie schließlich das Ansichtsmodell durch AJAX-Anforderungen für die Produkte und Aufträge:</span><span class="sxs-lookup"><span data-stu-id="4f1bf-134">Finally, initialize the view model by making AJAX requests for the products and orders:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

<span data-ttu-id="4f1bf-135">OK, also viel Code, aber wir erstellt er Sie schrittweise, wir hoffen Entwurf ist klar.</span><span class="sxs-lookup"><span data-stu-id="4f1bf-135">OK, that's a lot of code, but we built it up step-by-step, so hopefully the design is clear.</span></span> <span data-ttu-id="4f1bf-136">Jetzt können wir einige Knockout.js Bindungen auf den HTML-Code hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="4f1bf-136">Now we can add some Knockout.js bindings to the HTML.</span></span>

<span data-ttu-id="4f1bf-137">**Produkte**</span><span class="sxs-lookup"><span data-stu-id="4f1bf-137">**Products**</span></span>

<span data-ttu-id="4f1bf-138">Hier werden die Bindungen für die Produktliste aus:</span><span class="sxs-lookup"><span data-stu-id="4f1bf-138">Here are the bindings for the product list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

<span data-ttu-id="4f1bf-139">Diese Produkte Array durchläuft und zeigt den Namen und den Preis.</span><span class="sxs-lookup"><span data-stu-id="4f1bf-139">This iterates over the products array and displays the name and price.</span></span> <span data-ttu-id="4f1bf-140">Die Schaltfläche "Hinzufügen, Order" ist sichtbar, nur, wenn der Benutzer angemeldet ist.</span><span class="sxs-lookup"><span data-stu-id="4f1bf-140">The "Add to Order" button is visible only when the user is logged in.</span></span>

<span data-ttu-id="4f1bf-141">Schaltfläche "Hinzufügen, Order" ruft `addItemToCart` auf die `ProductViewModel` Instanz für das Produkt.</span><span class="sxs-lookup"><span data-stu-id="4f1bf-141">The "Add to Order" button calls `addItemToCart` on the `ProductViewModel` instance for the product.</span></span> <span data-ttu-id="4f1bf-142">Dies beweist, dass ein nützliches Feature der Knockout.js: bei einem Ansichtsmodell andere Modelle anzeigen enthält, können Sie die Bindungen auf das interne Modell anwenden.</span><span class="sxs-lookup"><span data-stu-id="4f1bf-142">This demonstrates a nice feature of Knockout.js: When a view-model contains other view-models, you can apply the bindings to the inner model.</span></span> <span data-ttu-id="4f1bf-143">In diesem Beispiel die Bindungen innerhalb der `foreach` gelten für jede der `ProductViewModel` Instanzen.</span><span class="sxs-lookup"><span data-stu-id="4f1bf-143">In this example, the bindings within the `foreach` are applied to each of the `ProductViewModel` instances.</span></span> <span data-ttu-id="4f1bf-144">Dieser Ansatz ist wesentlich übersichtlicher als einfügen alle Funktionen in einem einzelnen Modell anzeigen.</span><span class="sxs-lookup"><span data-stu-id="4f1bf-144">This approach is much cleaner than putting all of the functionality into a single view-model.</span></span>

<span data-ttu-id="4f1bf-145">**Cart**</span><span class="sxs-lookup"><span data-stu-id="4f1bf-145">**Cart**</span></span>

<span data-ttu-id="4f1bf-146">Hier werden die Bindungen für den Einkaufswagen:</span><span class="sxs-lookup"><span data-stu-id="4f1bf-146">Here are the bindings for the cart:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

<span data-ttu-id="4f1bf-147">Dies führt eine Iteration durch die Warenkorb-Array und zeigt den Namen, den Preis und die Menge.</span><span class="sxs-lookup"><span data-stu-id="4f1bf-147">This iterates over the cart array and displays the name, price, and quantity.</span></span> <span data-ttu-id="4f1bf-148">Beachten Sie, dass der Link "Entfernen" und die Schaltfläche "Create Order" für das ViewModel Funktionen gebunden sind.</span><span class="sxs-lookup"><span data-stu-id="4f1bf-148">Note that the "Remove" link and the "Create Order" button are bound to view-model functions.</span></span>

<span data-ttu-id="4f1bf-149">**Aufträge**</span><span class="sxs-lookup"><span data-stu-id="4f1bf-149">**Orders**</span></span>

<span data-ttu-id="4f1bf-150">Hier werden die Bindungen für die Orders-Liste aus:</span><span class="sxs-lookup"><span data-stu-id="4f1bf-150">Here are the bindings for the orders list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

<span data-ttu-id="4f1bf-151">Dies führt eine Iteration durch die Bestellungen und zeigt die Bestell-ID</span><span class="sxs-lookup"><span data-stu-id="4f1bf-151">This iterates over the orders and shows the order ID.</span></span> <span data-ttu-id="4f1bf-152">Das Click-Ereignis auf den Link gebunden ist, um die `getDetails` Funktion.</span><span class="sxs-lookup"><span data-stu-id="4f1bf-152">The click event on the link is bound to the `getDetails` function.</span></span>

<span data-ttu-id="4f1bf-153">**Auftragsdetails**</span><span class="sxs-lookup"><span data-stu-id="4f1bf-153">**Order Details**</span></span>

<span data-ttu-id="4f1bf-154">Hier werden die Bindungen für die Auftragsdetails enthält:</span><span class="sxs-lookup"><span data-stu-id="4f1bf-154">Here are the bindings for the order details:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

<span data-ttu-id="4f1bf-155">Dies führt eine Iteration durch die Elemente in der Reihenfolge und zeigt an, das Produkt, Preis und eines Orts.</span><span class="sxs-lookup"><span data-stu-id="4f1bf-155">This iterates over the items in the order and displays the product, price, and quanity.</span></span> <span data-ttu-id="4f1bf-156">Umgebenden DIV-Elements ist nur sichtbar, wenn das Details-Array ein oder mehrere Elemente enthält.</span><span class="sxs-lookup"><span data-stu-id="4f1bf-156">The surrounding div is visible only if the details array contains one or more items.</span></span>

## <a name="conclusion"></a><span data-ttu-id="4f1bf-157">Schlussbemerkung</span><span class="sxs-lookup"><span data-stu-id="4f1bf-157">Conclusion</span></span>

<span data-ttu-id="4f1bf-158">In diesem Lernprogramm erstellt Sie eine Anwendung, die Entity Framework verwendet, um die Kommunikation mit dem Datenbank- und ASP.NET Web-API, um eine öffentliche Schnittstelle auf die Datenschicht bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="4f1bf-158">In this tutorial, you created an application that uses Entity Framework to communicate with the database, and ASP.NET Web API to provide a public-facing interface on top of the data layer.</span></span> <span data-ttu-id="4f1bf-159">Wir verwenden die ASP.NET MVC 4 zum Rendern der HTML-Seiten und den Knockout.js sowie die jQuery um dynamische Aktivitäten ohne Seite Neuladen bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="4f1bf-159">We use ASP.NET MVC 4 to render the HTML pages, and Knockout.js plus jQuery to provide dynamic interactions without page reloads.</span></span>

<span data-ttu-id="4f1bf-160">Zusätzliche Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="4f1bf-160">Additional resources:</span></span>

- [<span data-ttu-id="4f1bf-161">ASP.NET Data Access-Inhaltszuordnung</span><span class="sxs-lookup"><span data-stu-id="4f1bf-161">ASP.NET Data Access Content Map</span></span>](https://msdn.microsoft.com/library/6759sth4.aspx)
- [<span data-ttu-id="4f1bf-162">Entity Framework Developer Center</span><span class="sxs-lookup"><span data-stu-id="4f1bf-162">Entity Framework Developer Center</span></span>](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [<span data-ttu-id="4f1bf-163">Vorherige</span><span class="sxs-lookup"><span data-stu-id="4f1bf-163">Previous</span></span>](using-web-api-with-entity-framework-part-6.md)
