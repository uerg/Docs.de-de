---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: 'Teil 8: Einkaufswagen mit Ajax-Updates | Microsoft-Dokumentation'
author: jongalloway
description: Dieser tutorialreihe werden alle Schritte ausgeführt, um die ASP.NET MVC Music Store-beispielanwendung zu erstellen. Teil 8 behandelt Einkaufswagen mit Ajax-Updates.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: cab338e56505c453532a26d794eb7bf4e94555a9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831292"
---
<a name="part-8-shopping-cart-with-ajax-updates"></a><span data-ttu-id="c3277-104">Teil 8: Einkaufswagen mit Ajax-Updates</span><span class="sxs-lookup"><span data-stu-id="c3277-104">Part 8: Shopping Cart with Ajax Updates</span></span>
====================
<span data-ttu-id="c3277-105">durch [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="c3277-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="c3277-106">Die MVC Music Store ist ein lernprogrammanwendung, die eingeführt und erläutert Schritt für Schritt, wie ASP.NET MVC und Visual Studio für die Webentwicklung verwenden.</span><span class="sxs-lookup"><span data-stu-id="c3277-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="c3277-107">Die MVC Music Store ist eine Implementierung eines einfachen Beispiels die Alben online verkauft und implementiert grundlegende Verwaltung, Benutzeranmeldung und shopping Cart-Funktionalität.</span><span class="sxs-lookup"><span data-stu-id="c3277-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="c3277-108">Dieser tutorialreihe werden alle Schritte ausgeführt, um die ASP.NET MVC Music Store-beispielanwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="c3277-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="c3277-109">Teil 8 behandelt Einkaufswagen mit Ajax-Updates.</span><span class="sxs-lookup"><span data-stu-id="c3277-109">Part 8 covers Shopping Cart with Ajax Updates.</span></span>


<span data-ttu-id="c3277-110">Lassen wir Benutzern Alben in den Einkaufswagen zu platzieren, ohne zu registrieren, aber sie müssen als Gäste vollständige Auschecken registrieren.</span><span class="sxs-lookup"><span data-stu-id="c3277-110">We'll allow users to place albums in their cart without registering, but they'll need to register as guests to complete checkout.</span></span> <span data-ttu-id="c3277-111">Die Einkaufs- und Auschecken wird in zwei Controller getrennt werden: eine ShoppingCart-Controller, der können anonym eine Warenkorb Elemente hinzufügt und ein Auschecken-Controller, der den Kassenvorgang behandelt.</span><span class="sxs-lookup"><span data-stu-id="c3277-111">The shopping and checkout process will be separated into two controllers: a ShoppingCart Controller which allows anonymously adding items to a cart, and a Checkout Controller which handles the checkout process.</span></span> <span data-ttu-id="c3277-112">Wir beginnen mit den Warenkorb legen in diesem Abschnitt, und anschließend den Kassenvorgang im folgenden Abschnitt zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="c3277-112">We'll start with the Shopping Cart in this section, then build the Checkout process in the following section.</span></span>

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a><span data-ttu-id="c3277-113">Hinzufügen der Warenkorb, Reihenfolge und OrderDetail-Modell-Klassen</span><span class="sxs-lookup"><span data-stu-id="c3277-113">Adding the Cart, Order, and OrderDetail model classes</span></span>

<span data-ttu-id="c3277-114">Unsere Prozesse Warenkorb und Auschecken veranlasst einiger neuer Klassen verwenden.</span><span class="sxs-lookup"><span data-stu-id="c3277-114">Our Shopping Cart and Checkout processes will make use of some new classes.</span></span> <span data-ttu-id="c3277-115">Mit der rechten Maustaste in den Ordner "Models", und fügen Sie eine Warenkorb-Klasse (Cart.cs) durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="c3277-115">Right-click the Models folder and add a Cart class (Cart.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

<span data-ttu-id="c3277-116">Diese Klasse ist recht ähnlich für andere Personen, die wir bisher, mit Ausnahme des Attributs [Key] für die Datensatz-ID-Eigenschaft verwendet haben.</span><span class="sxs-lookup"><span data-stu-id="c3277-116">This class is pretty similar to others we've used so far, with the exception of the [Key] attribute for the RecordId property.</span></span> <span data-ttu-id="c3277-117">Unsere im Warenkorb hat einen Zeichenfolgenbezeichner, der mit dem Namen CartID können anonymes einkaufen, aber die Tabelle enthält einen ganze Zahl Primärschlüssel, der mit dem Namen Datensatz-ID ein.</span><span class="sxs-lookup"><span data-stu-id="c3277-117">Our Cart items will have a string identifier named CartID to allow anonymous shopping, but the table includes an integer primary key named RecordId.</span></span> <span data-ttu-id="c3277-118">Gemäß der Konvention erwartet, dass Entity Framework Code First, dass der primäre Schlüssel für eine Tabelle mit dem Namen Einkaufswagen wird entweder CartId oder die ID, aber wir einfach, die über Anmerkungen oder Code überschreiben können, wenn wir möchten.</span><span class="sxs-lookup"><span data-stu-id="c3277-118">By convention, Entity Framework Code-First expects that the primary key for a table named Cart will be either CartId or ID, but we can easily override that via annotations or code if we want.</span></span> <span data-ttu-id="c3277-119">Dies ist ein Beispiel, wie wir verwendet die einfachen Konventionen in Entity Framework Code First-Wenn diese uns erfüllen, aber wir sind nicht von ihnen eingeschränkt, wenn dies nicht der Fall.</span><span class="sxs-lookup"><span data-stu-id="c3277-119">This is an example of how we can use the simple conventions in Entity Framework Code-First when they suit us, but we're not constrained by them when they don't.</span></span>

<span data-ttu-id="c3277-120">Als Nächstes fügen Sie eine Order-Klasse (Order.cs) durch den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="c3277-120">Next, add an Order class (Order.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

<span data-ttu-id="c3277-121">Diese Klasse dient die Zusammenfassung und Übermittlung von Informationen zu einer Bestellung nachverfolgen.</span><span class="sxs-lookup"><span data-stu-id="c3277-121">This class tracks summary and delivery information for an order.</span></span> <span data-ttu-id="c3277-122">**Nicht noch kompiliert**, da es sich um eine Navigationseigenschaft OrderDetails besitzt, der von einer Klasse abhängig ist dies nicht getan haben wir noch erstellt.</span><span class="sxs-lookup"><span data-stu-id="c3277-122">**It won't compile yet**, because it has an OrderDetails navigation property which depends on a class we haven't created yet.</span></span> <span data-ttu-id="c3277-123">Korrigieren Sie wir jetzt durch das Hinzufügen eine Klasse OrderDetail.cs, Hinzufügen des folgenden Codes benannt.</span><span class="sxs-lookup"><span data-stu-id="c3277-123">Let's fix that now by adding a class named OrderDetail.cs, adding the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

<span data-ttu-id="c3277-124">Wir erstellen eine letzte Aktualisierung um unsere MusicStoreEntities Klasse "dbsets" einbeziehen, die diese neuen Modellklassen, darunter auch einen "DbSet" verfügbar machen&lt;Interpreten&gt;.</span><span class="sxs-lookup"><span data-stu-id="c3277-124">We'll make one last update to our MusicStoreEntities class to include DbSets which expose those new Model classes, also including a DbSet&lt;Artist&gt;.</span></span> <span data-ttu-id="c3277-125">Die aktualisierte MusicStoreEntities-Klasse angezeigt wird, wie unten.</span><span class="sxs-lookup"><span data-stu-id="c3277-125">The updated MusicStoreEntities class appears as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a><span data-ttu-id="c3277-126">Verwalten von der Geschäftslogik Warenkorb</span><span class="sxs-lookup"><span data-stu-id="c3277-126">Managing the Shopping Cart business logic</span></span>

<span data-ttu-id="c3277-127">Als Nächstes erstellen wir die ShoppingCart-Klasse im Ordner "Models".</span><span class="sxs-lookup"><span data-stu-id="c3277-127">Next, we'll create the ShoppingCart class in the Models folder.</span></span> <span data-ttu-id="c3277-128">Die ShoppingCart-Modell verarbeitet die Datenzugriff auf die Warenkorb-Tabelle.</span><span class="sxs-lookup"><span data-stu-id="c3277-128">The ShoppingCart model handles data access to the Cart table.</span></span> <span data-ttu-id="c3277-129">Darüber hinaus wird es der Geschäftslogik zum Hinzufügen und Entfernen von Elementen aus dem Warenkorb verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="c3277-129">Additionally, it will handle the business logic to for adding and removing items from the shopping cart.</span></span>

<span data-ttu-id="c3277-130">Da wir keine Benutzer zum Registrieren für ein Konto aus, um Elemente zu ihrem Einkaufswagen hinzufügen möchten, zugewiesen Benutzer einen temporären eindeutigen Bezeichner (mithilfe einer GUID oder den globally unique Identifier) beim Zugreifen auf den Einkaufswagen.</span><span class="sxs-lookup"><span data-stu-id="c3277-130">Since we don't want to require users to sign up for an account just to add items to their shopping cart, we will assign users a temporary unique identifier (using a GUID, or globally unique identifier) when they access the shopping cart.</span></span> <span data-ttu-id="c3277-131">Wir speichern diese ID mithilfe der ASP.NET-Sitzung-Klasse.</span><span class="sxs-lookup"><span data-stu-id="c3277-131">We'll store this ID using the ASP.NET Session class.</span></span>

<span data-ttu-id="c3277-132">*Hinweis: Die ASP.NET-Sitzung ist eine bequeme Möglichkeit, benutzerspezifische Informationen speichern, die ablaufen wird, nachdem sie die Site zu verlassen. Während unsachgemäße Verwendung des Sitzungsstatus für größere Sites die Auswirkungen auf die Leistung haben kann, funktioniert die einfache Verwendung auch für Demonstrationszwecke.*</span><span class="sxs-lookup"><span data-stu-id="c3277-132">*Note: The ASP.NET Session is a convenient place to store user-specific information which will expire after they leave the site. While misuse of session state can have performance implications on larger sites, our light use will work well for demonstration purposes.*</span></span>

<span data-ttu-id="c3277-133">Die ShoppingCart-Klasse macht die folgenden Methoden verfügbar:</span><span class="sxs-lookup"><span data-stu-id="c3277-133">The ShoppingCart class exposes the following methods:</span></span>

<span data-ttu-id="c3277-134">**AddToCart** ein Album als Parameter akzeptiert und fügt es der Einkaufswagen des Benutzers hinzu.</span><span class="sxs-lookup"><span data-stu-id="c3277-134">**AddToCart** takes an Album as a parameter and adds it to the user's cart.</span></span> <span data-ttu-id="c3277-135">Da die Warenkorb-Tabelle Menge für jedes Album nachverfolgt, enthält sie Logik, um bei Bedarf eine neue Zeile erstellen oder nur die Menge erhöht, wenn der Benutzer bereits eine Kopie des Albums aufgegeben hat.</span><span class="sxs-lookup"><span data-stu-id="c3277-135">Since the Cart table tracks quantity for each album, it includes logic to create a new row if needed or just increment the quantity if the user has already ordered one copy of the album.</span></span>

<span data-ttu-id="c3277-136">**RemoveFromCart** übernimmt ein Album-ID und entfernt sie aus dem Einkaufswagen des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="c3277-136">**RemoveFromCart** takes an Album ID and removes it from the user's cart.</span></span> <span data-ttu-id="c3277-137">Wenn der Benutzer nur eine Kopie des Albums im Einkaufswagen haben, wird die Zeile entfernt.</span><span class="sxs-lookup"><span data-stu-id="c3277-137">If the user only had one copy of the album in their cart, the row is removed.</span></span>

<span data-ttu-id="c3277-138">**EmptyCart** entfernt alle Elemente aus dem Einkaufswagen eines Benutzers.</span><span class="sxs-lookup"><span data-stu-id="c3277-138">**EmptyCart** removes all items from a user's shopping cart.</span></span>

<span data-ttu-id="c3277-139">**GetCartItems** Ruft eine Liste der CartItems für Sie Anzeige- oder ab.</span><span class="sxs-lookup"><span data-stu-id="c3277-139">**GetCartItems** retrieves a list of CartItems for display or processing.</span></span>

<span data-ttu-id="c3277-140">**GetCount** Ruft ein die Gesamtanzahl von Alben, die ein Benutzer im Einkaufswagen hat.</span><span class="sxs-lookup"><span data-stu-id="c3277-140">**GetCount** retrieves a the total number of albums a user has in their shopping cart.</span></span>

<span data-ttu-id="c3277-141">**GetTotal** berechnet die Gesamtkosten für alle Elemente im Warenkorb.</span><span class="sxs-lookup"><span data-stu-id="c3277-141">**GetTotal** calculates the total cost of all items in the cart.</span></span>

<span data-ttu-id="c3277-142">**CreateOrder** wandelt den Einkaufswagen in einen Auftrag während der Phase Auschecken.</span><span class="sxs-lookup"><span data-stu-id="c3277-142">**CreateOrder** converts the shopping cart to an order during the checkout phase.</span></span>

<span data-ttu-id="c3277-143">**GetCart** ist eine statische Methode, wodurch unsere-Controller, ein Warenkorb-Objekt abzurufen.</span><span class="sxs-lookup"><span data-stu-id="c3277-143">**GetCart** is a static method which allows our controllers to obtain a cart object.</span></span> <span data-ttu-id="c3277-144">Er verwendet den **GetCartId** Methode zum Behandeln von lesen die CartId aus der Sitzung des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="c3277-144">It uses the **GetCartId** method to handle reading the CartId from the user's session.</span></span> <span data-ttu-id="c3277-145">Die Methode GetCartId erfordert HttpContextBase, sodass des Benutzers CartId mit der Sitzung des Benutzers zu lesen.</span><span class="sxs-lookup"><span data-stu-id="c3277-145">The GetCartId method requires the HttpContextBase so that it can read the user's CartId from user's session.</span></span>

<span data-ttu-id="c3277-146">Hier ist die vollständige **ShoppingCart-Klasse**:</span><span class="sxs-lookup"><span data-stu-id="c3277-146">Here's the complete **ShoppingCart class**:</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a><span data-ttu-id="c3277-147">ViewModels</span><span class="sxs-lookup"><span data-stu-id="c3277-147">ViewModels</span></span>

<span data-ttu-id="c3277-148">Unser Shopping Cart-Controller, müssen einige komplexen Informationen zu seiner Ansichten kommunizieren, die nicht ordnungsgemäß für unsere Modellobjekte zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="c3277-148">Our Shopping Cart Controller will need to communicate some complex information to its views which doesn't map cleanly to our Model objects.</span></span> <span data-ttu-id="c3277-149">Wir wollen nicht ändern, die Modelle, die Ansichten anpassen; Modellklassen sollte die Domäne, nicht in der Benutzeroberfläche darstellen.</span><span class="sxs-lookup"><span data-stu-id="c3277-149">We don't want to modify our Models to suit our views; Model classes should represent our domain, not the user interface.</span></span> <span data-ttu-id="c3277-150">Eine Lösung wäre, übergeben die Informationen an den Ansichten die ViewBag-Klasse verwenden, haben wir mit den Informationen der Store Manager-Dropdownliste aus, aber eine Vielzahl von Informationen über "ViewBag" übergeben, ruft schwierig zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="c3277-150">One solution would be to pass the information to our Views using the ViewBag class, as we did with the Store Manager dropdown information, but passing a lot of information via ViewBag gets hard to manage.</span></span>

<span data-ttu-id="c3277-151">Dies ist die Verwendung der *"ViewModel"* Muster.</span><span class="sxs-lookup"><span data-stu-id="c3277-151">A solution to this is to use the *ViewModel* pattern.</span></span> <span data-ttu-id="c3277-152">Verwendung dieses Musters erstellen wir die stark typisierte Klassen, die für unseren bestimmten Ansicht-Szenarien optimiert sind, und das Verfügbarmachen der Eigenschaften für das dynamische Werte bzw. den Inhalt von unserem Ansichtsvorlagen benötigt werden.</span><span class="sxs-lookup"><span data-stu-id="c3277-152">When using this pattern we create strongly-typed classes that are optimized for our specific view scenarios, and which expose properties for the dynamic values/content needed by our view templates.</span></span> <span data-ttu-id="c3277-153">Die Controllerklassen können klicken Sie dann Auffüllen und übergeben diese Klassen Ansicht optimiert, unsere ansichtsvorlage verwenden.</span><span class="sxs-lookup"><span data-stu-id="c3277-153">Our controller classes can then populate and pass these view-optimized classes to our view template to use.</span></span> <span data-ttu-id="c3277-154">Dadurch wird typsicherheit, Überprüfung und IntelliSense-Editor in Vorlagen anzeigen.</span><span class="sxs-lookup"><span data-stu-id="c3277-154">This enables type-safety, compile-time checking, and editor IntelliSense within view templates.</span></span>

<span data-ttu-id="c3277-155">Wir erstellen zwei Ansichtsmodelle für die Verwendung in den Einkaufswagen-Controller: die ShoppingCartViewModel übernimmt den Inhalt der Einkaufswagen eines Benutzers, und die ShoppingCartRemoveViewModel wird verwendet, um Informationen zur Bestätigung angezeigt, wenn ein Benutzer etwas entfernt in den Einkaufswagen.</span><span class="sxs-lookup"><span data-stu-id="c3277-155">We'll create two View Models for use in our Shopping Cart controller: the ShoppingCartViewModel will hold the contents of the user's shopping cart, and the ShoppingCartRemoveViewModel will be used to display confirmation information when a user removes something from their cart.</span></span>

<span data-ttu-id="c3277-156">Erstellen wir einen neuen Ordner "ViewModels" im Stammverzeichnis des unseres Projekts halber organisiert an.</span><span class="sxs-lookup"><span data-stu-id="c3277-156">Let's create a new ViewModels folder in the root of our project to keep things organized.</span></span> <span data-ttu-id="c3277-157">Mit der rechten Maustaste in des Projekts, wählen Sie Add / neuen Ordner.</span><span class="sxs-lookup"><span data-stu-id="c3277-157">Right-click the project, select Add / New Folder.</span></span>

![](mvc-music-store-part-8/_static/image1.jpg)

<span data-ttu-id="c3277-158">Benennen Sie den Ordner "ViewModels".</span><span class="sxs-lookup"><span data-stu-id="c3277-158">Name the folder ViewModels.</span></span>

![](mvc-music-store-part-8/_static/image1.png)

<span data-ttu-id="c3277-159">Fügen Sie anschließend die ShoppingCartViewModel-Klasse im Ordner "ViewModels" ein.</span><span class="sxs-lookup"><span data-stu-id="c3277-159">Next, add the ShoppingCartViewModel class in the ViewModels folder.</span></span> <span data-ttu-id="c3277-160">Er verfügt über zwei Eigenschaften: eine Liste der im Warenkorb, und ein decimal-Wert, den Gesamtpreis für alle Elemente im Warenkorb enthalten soll.</span><span class="sxs-lookup"><span data-stu-id="c3277-160">It has two properties: a list of Cart items, and a decimal value to hold the total price for all items in the cart.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

<span data-ttu-id="c3277-161">Fügen Sie nun die ShoppingCartRemoveViewModel zum Ordner "ViewModels" mit den folgenden vier Eigenschaften hinzu.</span><span class="sxs-lookup"><span data-stu-id="c3277-161">Now add the ShoppingCartRemoveViewModel to the ViewModels folder, with the following four properties.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a><span data-ttu-id="c3277-162">Die Einkaufswagencontroller</span><span class="sxs-lookup"><span data-stu-id="c3277-162">The Shopping Cart Controller</span></span>

<span data-ttu-id="c3277-163">Der Controller Warenkorb hat drei Hauptfunktionen: Hinzufügen von Elementen zu einem Einkaufswagen und Entfernen von Elementen aus dem Warenkorb Elemente im Warenkorb anzeigen.</span><span class="sxs-lookup"><span data-stu-id="c3277-163">The Shopping Cart controller has three main purposes: adding items to a cart, removing items from the cart, and viewing items in the cart.</span></span> <span data-ttu-id="c3277-164">Verwenden der drei Klassen wir machen wird gerade erstellt haben: ShoppingCartViewModel ShoppingCartRemoveViewModel "und" ShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="c3277-164">It will make use of the three classes we just created: ShoppingCartViewModel, ShoppingCartRemoveViewModel, and ShoppingCart.</span></span> <span data-ttu-id="c3277-165">Wie bei den StoreController und StoreManagerController fügen wir ein Feld, um eine Instanz der MusicStoreEntities befindet.</span><span class="sxs-lookup"><span data-stu-id="c3277-165">As in the StoreController and StoreManagerController, we'll add a field to hold an instance of MusicStoreEntities.</span></span>

<span data-ttu-id="c3277-166">Fügen Sie einen neuen Warenkorb-Controller, auf das Projekt mithilfe der Vorlage der leeren Controller.</span><span class="sxs-lookup"><span data-stu-id="c3277-166">Add a new Shopping Cart controller to the project using the Empty controller template.</span></span>

![](mvc-music-store-part-8/_static/image2.png)

<span data-ttu-id="c3277-167">Hier ist der vollständige ShoppingCart-Controller.</span><span class="sxs-lookup"><span data-stu-id="c3277-167">Here's the complete ShoppingCart Controller.</span></span> <span data-ttu-id="c3277-168">Die Aktionen für Index "und" Controller hinzufügen, sollte sehr vertraut aussehen.</span><span class="sxs-lookup"><span data-stu-id="c3277-168">The Index and Add Controller actions should look very familiar.</span></span> <span data-ttu-id="c3277-169">Die Controller-Aktionen entfernen und CartSummary behandeln zwei spezielle Fälle, die wir im folgenden Abschnitt eingehen werde.</span><span class="sxs-lookup"><span data-stu-id="c3277-169">The Remove and CartSummary controller actions handle two special cases, which we'll discuss in the following section.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a><span data-ttu-id="c3277-170">AJAX-Updates mit jQuery</span><span class="sxs-lookup"><span data-stu-id="c3277-170">Ajax Updates with jQuery</span></span>

<span data-ttu-id="c3277-171">Wir erstellen neben eine Shopping Cart-Indexseite, die stark typisiert, die ShoppingCartViewModel und verwendet die Listenansicht-Vorlage, die mit derselben Methode wie vor.</span><span class="sxs-lookup"><span data-stu-id="c3277-171">We'll next create a Shopping Cart Index page that is strongly typed to the ShoppingCartViewModel and uses the List View template using the same method as before.</span></span>

![](mvc-music-store-part-8/_static/image3.png)

<span data-ttu-id="c3277-172">Jedoch wird statt einer Html.ActionLink, um Elemente aus dem Einkaufswagen entfernen, jQuery verwendet, um das Click-Ereignis für alle Links in dieser Ansicht die HTML-RemoveLink Klasse "verknüpfen".</span><span class="sxs-lookup"><span data-stu-id="c3277-172">However, instead of using an Html.ActionLink to remove items from the cart, we'll use jQuery to "wire up" the click event for all links in this view which have the HTML class RemoveLink.</span></span> <span data-ttu-id="c3277-173">Anstatt veröffentlichen das Formular, wird diese Click-Ereignishandler nur einen AJAX-Rückruf an unsere RemoveFromCart Controlleraktion vornehmen.</span><span class="sxs-lookup"><span data-stu-id="c3277-173">Rather than posting the form, this click event handler will just make an AJAX callback to our RemoveFromCart controller action.</span></span> <span data-ttu-id="c3277-174">Die RemoveFromCart ein JSON serialisierte Ergebnis zurückgegeben, die unsere jQuery-Rückruf dann analysiert und führt vier schnellen Updates auf der Seite mit jQuery:</span><span class="sxs-lookup"><span data-stu-id="c3277-174">The RemoveFromCart returns a JSON serialized result, which our jQuery callback then parses and performs four quick updates to the page using jQuery:</span></span>

- 1. <span data-ttu-id="c3277-175">Entfernt das Album gelöschte, aus der Liste</span><span class="sxs-lookup"><span data-stu-id="c3277-175">Removes the deleted album from the list</span></span>
- 2. <span data-ttu-id="c3277-176">Aktualisiert die Warenkorb-Anzahl in der Kopfzeile</span><span class="sxs-lookup"><span data-stu-id="c3277-176">Updates the cart count in the header</span></span>
- 3. <span data-ttu-id="c3277-177">Zeigt eine Update-Nachricht an den Benutzer</span><span class="sxs-lookup"><span data-stu-id="c3277-177">Displays an update message to the user</span></span>
- 4. <span data-ttu-id="c3277-178">Aktualisiert den Gesamtpreis Warenkorb</span><span class="sxs-lookup"><span data-stu-id="c3277-178">Updates the cart total price</span></span>

<span data-ttu-id="c3277-179">Da das Remove-Szenario durch ein Ajax-Rückruf in die Ansicht "Index" verarbeitet wird, benötigen wir nicht für RemoveFromCart Aktion eine zusätzliche Ansicht.</span><span class="sxs-lookup"><span data-stu-id="c3277-179">Since the remove scenario is being handled by an Ajax callback within the Index view, we don't need an additional view for RemoveFromCart action.</span></span> <span data-ttu-id="c3277-180">Hier ist der vollständige Code für die /ShoppingCart/Index-Ansicht:</span><span class="sxs-lookup"><span data-stu-id="c3277-180">Here is the complete code for the /ShoppingCart/Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

<span data-ttu-id="c3277-181">Um dies zu testen, müssen wir unsere warenkorbsoftware Elemente hinzugefügt werden können.</span><span class="sxs-lookup"><span data-stu-id="c3277-181">In order to test this out, we need to be able to add items to our shopping cart.</span></span> <span data-ttu-id="c3277-182">Wir aktualisieren unsere **Store Details** Ansicht, um eine Schaltfläche "Element zum Einkaufswagen hinzufügen" enthalten.</span><span class="sxs-lookup"><span data-stu-id="c3277-182">We'll update our **Store Details** view to include an "Add to cart" button.</span></span> <span data-ttu-id="c3277-183">Während wir schon dabei sind, zählen wir einige zusätzliche Informationen zum Album die wir hinzugefügt haben, da wir in dieser Ansicht zum zuletzt aktualisiert: Genre, Künstler, Preis und Albumcover.</span><span class="sxs-lookup"><span data-stu-id="c3277-183">While we're at it, we can include some of the Album additional information which we've added since we last updated this view: Genre, Artist, Price, and Album Art.</span></span> <span data-ttu-id="c3277-184">Der aktualisierte Code der Store-Details anzeigen angezeigt wird, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="c3277-184">The updated Store Details view code appears as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

<span data-ttu-id="c3277-185">Jetzt können wir klicken Sie auf, über den Store und hinzufügen und Entfernen von Alben in und aus unsere warenkorbsoftware zu testen.</span><span class="sxs-lookup"><span data-stu-id="c3277-185">Now we can click through the store and test adding and removing Albums to and from our shopping cart.</span></span> <span data-ttu-id="c3277-186">Führen Sie die Anwendung, und navigieren Sie zu der Store-Index.</span><span class="sxs-lookup"><span data-stu-id="c3277-186">Run the application and browse to the Store Index.</span></span>

![](mvc-music-store-part-8/_static/image4.png)

<span data-ttu-id="c3277-187">Klicken Sie anschließend auf ein Genre, um eine Liste der Alben anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="c3277-187">Next, click on a Genre to view a list of albums.</span></span>

![](mvc-music-store-part-8/_static/image5.png)

<span data-ttu-id="c3277-188">Durch Klicken auf eine Albumtitel jetzt zeigt unserer aktualisierten Album Detailansicht, darunter die Schaltfläche "Element zum Einkaufswagen hinzufügen".</span><span class="sxs-lookup"><span data-stu-id="c3277-188">Clicking on an Album title now shows our updated Album Details view, including the "Add to cart" button.</span></span>

![](mvc-music-store-part-8/_static/image6.png)

<span data-ttu-id="c3277-189">Klicken Sie auf die Schaltfläche "Element zum Einkaufswagen hinzufügen" zeigt unsere Shopping Cart-Index-Ansicht mit der shopping Cart Zusammenfassungsliste.</span><span class="sxs-lookup"><span data-stu-id="c3277-189">Clicking the "Add to cart" button shows our Shopping Cart Index view with the shopping cart summary list.</span></span>

![](mvc-music-store-part-8/_static/image7.png)

<span data-ttu-id="c3277-190">Nach dem Laden Sie Ihr Warenkorb ist leer, können Sie auf der Warenkorb Link entfernen, das Ajax-Update zu Ihrem Warenkorb anzeigen klicken.</span><span class="sxs-lookup"><span data-stu-id="c3277-190">After loading up your shopping cart, you can click on the Remove from cart link to see the Ajax update to your shopping cart.</span></span>

![](mvc-music-store-part-8/_static/image8.png)

<span data-ttu-id="c3277-191">Wir haben das fertige eines funktionsfähiges Einkaufswagen, nicht registrierte Benutzer Elemente Einkaufswagen hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="c3277-191">We've built out a working shopping cart which allows unregistered users to add items to their cart.</span></span> <span data-ttu-id="c3277-192">Im folgenden Abschnitt werden wir zu registrieren und Ausführen des Auscheckvorgangs ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="c3277-192">In the following section, we'll allow them to register and complete the checkout process.</span></span>


> [!div class="step-by-step"]
> <span data-ttu-id="c3277-193">[Zurück](mvc-music-store-part-7.md)
> [Weiter](mvc-music-store-part-9.md)</span><span class="sxs-lookup"><span data-stu-id="c3277-193">[Previous](mvc-music-store-part-7.md)
[Next](mvc-music-store-part-9.md)</span></span>
