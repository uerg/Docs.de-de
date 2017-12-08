---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: 'Teil 8: Einkaufswagen mit Ajax-Updates | Microsoft Docs'
author: jongalloway
description: Diese Reihe von Lernprogrammen details aller die Schritte zum Erstellen von ASP.NET MVC-Music Store-beispielanwendung. Teil 8 deckt Einkaufswagen mit Ajax-Updates.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 75e1dff96f8b56d74c28ff9d522f4766fbad669f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="part-8-shopping-cart-with-ajax-updates"></a><span data-ttu-id="80eb5-104">Teil 8: Einkaufswagen mit Ajax-Updates</span><span class="sxs-lookup"><span data-stu-id="80eb5-104">Part 8: Shopping Cart with Ajax Updates</span></span>
====================
<span data-ttu-id="80eb5-105">durch [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="80eb5-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="80eb5-106">Das MVC-Music Store ist eine lernprogrammanwendung, die führt und erklärt schrittweise, wie mithilfe von ASP.NET MVC und Visual Studio für die Webentwicklung.</span><span class="sxs-lookup"><span data-stu-id="80eb5-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="80eb5-107">Das MVC-Music Store handelt es sich um einfache Beispiel Store Implementierung, die online Musikalben verkauft und implementiert grundlegende standortverwaltung Benutzer anmelden und shopping Cart-Funktionalität.</span><span class="sxs-lookup"><span data-stu-id="80eb5-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="80eb5-108">Diese Reihe von Lernprogrammen details aller die Schritte zum Erstellen von ASP.NET MVC-Music Store-beispielanwendung.</span><span class="sxs-lookup"><span data-stu-id="80eb5-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="80eb5-109">Teil 8 deckt Einkaufswagen mit Ajax-Updates.</span><span class="sxs-lookup"><span data-stu-id="80eb5-109">Part 8 covers Shopping Cart with Ajax Updates.</span></span>


<span data-ttu-id="80eb5-110">Bieten wir Benutzern die Alben in ihrem Einkaufswagen zu platzieren, ohne diesen zu registrieren, aber sie müssen als Gäste vollständige Auschecken registrieren.</span><span class="sxs-lookup"><span data-stu-id="80eb5-110">We'll allow users to place albums in their cart without registering, but they'll need to register as guests to complete checkout.</span></span> <span data-ttu-id="80eb5-111">Die Warenkorb und Auschecken wird in zwei Controller getrennt werden: eine ShoppingCart-Controller dadurch anonym Hinzufügen von Elementen zu einem Einkaufswagen und einen Checkout-Controller die des Auscheckvorgangs behandelt.</span><span class="sxs-lookup"><span data-stu-id="80eb5-111">The shopping and checkout process will be separated into two controllers: a ShoppingCart Controller which allows anonymously adding items to a cart, and a Checkout Controller which handles the checkout process.</span></span> <span data-ttu-id="80eb5-112">Wir müssen mit dem Einkaufswagen in diesem Abschnitt beginnen, und erstellen dann des Auscheckvorgangs im folgenden Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="80eb5-112">We'll start with the Shopping Cart in this section, then build the Checkout process in the following section.</span></span>

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a><span data-ttu-id="80eb5-113">Hinzufügen von Modellklassen Einkaufswagen, Bestell- und OrderDetail</span><span class="sxs-lookup"><span data-stu-id="80eb5-113">Adding the Cart, Order, and OrderDetail model classes</span></span>

<span data-ttu-id="80eb5-114">Unsere Prozesse im Einkaufswagen und Auschecken stellen einige neuer Klassen verwenden.</span><span class="sxs-lookup"><span data-stu-id="80eb5-114">Our Shopping Cart and Checkout processes will make use of some new classes.</span></span> <span data-ttu-id="80eb5-115">Mit der rechten Maustaste in den Ordner Models, und fügen Sie eine Klasse mit dem Einkaufswagen (Cart.cs), durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="80eb5-115">Right-click the Models folder and add a Cart class (Cart.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

<span data-ttu-id="80eb5-116">Diese Klasse ist ziemlich ähnliche an andere Personen, die wir mit Ausnahme des Attributs [Key] für die Eigenschaft RecordId bisher verwendet haben.</span><span class="sxs-lookup"><span data-stu-id="80eb5-116">This class is pretty similar to others we've used so far, with the exception of the [Key] attribute for the RecordId property.</span></span> <span data-ttu-id="80eb5-117">Unsere Warenkorb werden einen Zeichenfolgenbezeichner, der mit dem Namen CartID zum Zulassen anonymer Warenkorb haben, aber die Tabelle enthält einen ganze Zahl Primärschlüssel, der mit dem Namen RecordId.</span><span class="sxs-lookup"><span data-stu-id="80eb5-117">Our Cart items will have a string identifier named CartID to allow anonymous shopping, but the table includes an integer primary key named RecordId.</span></span> <span data-ttu-id="80eb5-118">Gemäß der Konvention wird von Entity Framework Code First erwartet, dass der Primärschlüssel für eine Tabelle mit dem Namen Einkaufswagen, CartId oder ID, aber wir leicht, die über Anmerkungen oder Code überschreiben können gegebenenfalls.</span><span class="sxs-lookup"><span data-stu-id="80eb5-118">By convention, Entity Framework Code-First expects that the primary key for a table named Cart will be either CartId or ID, but we can easily override that via annotations or code if we want.</span></span> <span data-ttu-id="80eb5-119">Dies ist ein Beispiel, wie wir verwenden können einfache Konventionen in Entity Framework Code First, wenn sie uns anpassen, aber es sind nicht von ihnen eingeschränkt, wenn diese nicht.</span><span class="sxs-lookup"><span data-stu-id="80eb5-119">This is an example of how we can use the simple conventions in Entity Framework Code-First when they suit us, but we're not constrained by them when they don't.</span></span>

<span data-ttu-id="80eb5-120">Als Nächstes fügen Sie eine Order-Klasse (Order.cs) durch den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="80eb5-120">Next, add an Order class (Order.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

<span data-ttu-id="80eb5-121">Diese Klasse wird nachverfolgt, Zusammenfassung und die Übermittlung von Informationen für eine Bestellung.</span><span class="sxs-lookup"><span data-stu-id="80eb5-121">This class tracks summary and delivery information for an order.</span></span> <span data-ttu-id="80eb5-122">**Er wird nicht kompiliert werden noch**, da er eine Navigationseigenschaft OrderDetails aufweist, der von einer Klasse abhängt wir noch nicht noch erstellt.</span><span class="sxs-lookup"><span data-stu-id="80eb5-122">**It won't compile yet**, because it has an OrderDetails navigation property which depends on a class we haven't created yet.</span></span> <span data-ttu-id="80eb5-123">Wir beheben, die jetzt durch Hinzufügen von Klasse OrderDetail.cs, den folgenden Code hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="80eb5-123">Let's fix that now by adding a class named OrderDetail.cs, adding the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

<span data-ttu-id="80eb5-124">Stellen wir eine letzte Aktualisierung um unsere MusicStoreEntities-Klasse, um DbSets einzuschließen, die diese neue Modellklassen, darunter auch ein ' DbSet ' verfügbar zu machen&lt;Interpreten&gt;.</span><span class="sxs-lookup"><span data-stu-id="80eb5-124">We'll make one last update to our MusicStoreEntities class to include DbSets which expose those new Model classes, also including a DbSet&lt;Artist&gt;.</span></span> <span data-ttu-id="80eb5-125">Die aktualisierte MusicStoreEntities-Klasse angezeigt wird, als unten.</span><span class="sxs-lookup"><span data-stu-id="80eb5-125">The updated MusicStoreEntities class appears as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a><span data-ttu-id="80eb5-126">Verwalten von der Geschäftslogik Einkaufswagen</span><span class="sxs-lookup"><span data-stu-id="80eb5-126">Managing the Shopping Cart business logic</span></span>

<span data-ttu-id="80eb5-127">Als Nächstes erstellen wir im Ordner Models die ShoppingCart-Klasse.</span><span class="sxs-lookup"><span data-stu-id="80eb5-127">Next, we'll create the ShoppingCart class in the Models folder.</span></span> <span data-ttu-id="80eb5-128">Die ShoppingCart-Modell verarbeitet den Datenzugriff auf die Warenkorb-Tabelle.</span><span class="sxs-lookup"><span data-stu-id="80eb5-128">The ShoppingCart model handles data access to the Cart table.</span></span> <span data-ttu-id="80eb5-129">Darüber hinaus wird es der Geschäftslogik zum Hinzufügen und Entfernen von Elementen aus dem Einkaufswagen behandelt werden.</span><span class="sxs-lookup"><span data-stu-id="80eb5-129">Additionally, it will handle the business logic to for adding and removing items from the shopping cart.</span></span>

<span data-ttu-id="80eb5-130">Da wir nicht benötigen Benutzer zum Registrieren für ein Konto nur für Elemente in seinen Einkaufswagen legen hinzufügen möchten, wir weist Benutzer einen temporären eindeutigen Bezeichner (mithilfe eines GUID oder ein global eindeutiger Bezeichner) beim Zugriff auf des Einkaufswagen Sinn macht.</span><span class="sxs-lookup"><span data-stu-id="80eb5-130">Since we don't want to require users to sign up for an account just to add items to their shopping cart, we will assign users a temporary unique identifier (using a GUID, or globally unique identifier) when they access the shopping cart.</span></span> <span data-ttu-id="80eb5-131">Speichern wir diese ID mithilfe der ASP.NET Session-Klasse.</span><span class="sxs-lookup"><span data-stu-id="80eb5-131">We'll store this ID using the ASP.NET Session class.</span></span>

<span data-ttu-id="80eb5-132">*Hinweis: Die ASP.NET-Sitzung ist eine bequeme Möglichkeit, Sie benutzerspezifische Informationen speichern, die abläuft, wenn sie die Website verlassen haben. Missbrauch des Sitzungsstatus Leistungseinbußen an größeren Standorten kann zwar aufweisen, funktioniert unsere hell verwenden ebenfalls zu Demonstrationszwecken.*</span><span class="sxs-lookup"><span data-stu-id="80eb5-132">*Note: The ASP.NET Session is a convenient place to store user-specific information which will expire after they leave the site. While misuse of session state can have performance implications on larger sites, our light use will work well for demonstration purposes.*</span></span>

<span data-ttu-id="80eb5-133">Die ShoppingCart-Klasse stellt die folgenden Methoden:</span><span class="sxs-lookup"><span data-stu-id="80eb5-133">The ShoppingCart class exposes the following methods:</span></span>

<span data-ttu-id="80eb5-134">**AddToCart** ein Album als Parameter akzeptiert und der Einkaufswagen des Benutzers hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="80eb5-134">**AddToCart** takes an Album as a parameter and adds it to the user's cart.</span></span> <span data-ttu-id="80eb5-135">Da die Warenkorb-Tabelle Menge für jedes Album überwacht wird, enthält sie Logik zum Erstellen Sie ggf. einer neuen Zeile oder die Menge nur erhöht, wenn der Benutzer bereits eine Kopie des Albums aufgegeben hat.</span><span class="sxs-lookup"><span data-stu-id="80eb5-135">Since the Cart table tracks quantity for each album, it includes logic to create a new row if needed or just increment the quantity if the user has already ordered one copy of the album.</span></span>

<span data-ttu-id="80eb5-136">**RemoveFromCart** übernimmt ein Album-ID und entfernt sie aus der Einkaufswagen des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="80eb5-136">**RemoveFromCart** takes an Album ID and removes it from the user's cart.</span></span> <span data-ttu-id="80eb5-137">Wenn der Benutzer nur eine Kopie des Albums in ihrem Einkaufswagen hat, wird die Zeile entfernt.</span><span class="sxs-lookup"><span data-stu-id="80eb5-137">If the user only had one copy of the album in their cart, the row is removed.</span></span>

<span data-ttu-id="80eb5-138">**EmptyCart** entfernt alle Elemente aus dem Einkaufswagen des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="80eb5-138">**EmptyCart** removes all items from a user's shopping cart.</span></span>

<span data-ttu-id="80eb5-139">**GetCartItems** Ruft eine Liste von CartItems für die Anzeige oder Verarbeitung ab.</span><span class="sxs-lookup"><span data-stu-id="80eb5-139">**GetCartItems** retrieves a list of CartItems for display or processing.</span></span>

<span data-ttu-id="80eb5-140">**GetCount** Ruft ein die Gesamtanzahl von Alben ein Benutzers im Einkaufswagen ist.</span><span class="sxs-lookup"><span data-stu-id="80eb5-140">**GetCount** retrieves a the total number of albums a user has in their shopping cart.</span></span>

<span data-ttu-id="80eb5-141">**GetTotal** berechnet die Gesamtkosten für alle Elemente im Warenkorb.</span><span class="sxs-lookup"><span data-stu-id="80eb5-141">**GetTotal** calculates the total cost of all items in the cart.</span></span>

<span data-ttu-id="80eb5-142">**CreateOrder** konvertiert den Einkaufswagen einer Bestellung Phase Auschecken.</span><span class="sxs-lookup"><span data-stu-id="80eb5-142">**CreateOrder** converts the shopping cart to an order during the checkout phase.</span></span>

<span data-ttu-id="80eb5-143">**GetCart** ist eine statische Methode, wodurch unsere Controller, ein Warenkorb-Objekt abzurufen.</span><span class="sxs-lookup"><span data-stu-id="80eb5-143">**GetCart** is a static method which allows our controllers to obtain a cart object.</span></span> <span data-ttu-id="80eb5-144">Er verwendet die **GetCartId** Methode, lesen die CartId aus der Sitzung des Benutzers zu behandeln.</span><span class="sxs-lookup"><span data-stu-id="80eb5-144">It uses the **GetCartId** method to handle reading the CartId from the user's session.</span></span> <span data-ttu-id="80eb5-145">Die GetCartId-Methode erfordert HttpContextBase, sodass der Benutzer CartId mit Sitzung des Benutzers zu lesen.</span><span class="sxs-lookup"><span data-stu-id="80eb5-145">The GetCartId method requires the HttpContextBase so that it can read the user's CartId from user's session.</span></span>

<span data-ttu-id="80eb5-146">Hier wird die vollständige **ShoppingCart Klasse**:</span><span class="sxs-lookup"><span data-stu-id="80eb5-146">Here's the complete **ShoppingCart class**:</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a><span data-ttu-id="80eb5-147">ViewModels</span><span class="sxs-lookup"><span data-stu-id="80eb5-147">ViewModels</span></span>

<span data-ttu-id="80eb5-148">Unser Shopping Cart-Controller müssen einige komplexen Informationen zu den Ansichten zu übergeben, die unserer Modellobjekte ordnungsgemäß zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="80eb5-148">Our Shopping Cart Controller will need to communicate some complex information to its views which doesn't map cleanly to our Model objects.</span></span> <span data-ttu-id="80eb5-149">Wir möchten unsere Modelle entsprechend unserer Ansichten ändern; Modellklassen sollten unsere Domäne, die nicht in der Benutzeroberfläche darstellen.</span><span class="sxs-lookup"><span data-stu-id="80eb5-149">We don't want to modify our Models to suit our views; Model classes should represent our domain, not the user interface.</span></span> <span data-ttu-id="80eb5-150">Eine Lösung wäre, übergeben Sie die Informationen an unsere Sichten, die die ViewBag-Klasse verwenden, haben wir mit den Informationen der Speicher-Manager-Dropdownliste aus, sondern übergeben eine Vielzahl von Informationen über ViewBag ruft schwer zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="80eb5-150">One solution would be to pass the information to our Views using the ViewBag class, as we did with the Store Manager dropdown information, but passing a lot of information via ViewBag gets hard to manage.</span></span>

<span data-ttu-id="80eb5-151">Eine Lösung für diese ist die Verwendung der *ViewModel* Muster.</span><span class="sxs-lookup"><span data-stu-id="80eb5-151">A solution to this is to use the *ViewModel* pattern.</span></span> <span data-ttu-id="80eb5-152">Bei Verwendung dieses Muster erstellen wir stark typisierter Klassen, die für unsere bestimmte Ansicht Szenarien optimiert sind und die Eigenschaften für die dynamischen Werte/Content durch unsere Ansichtsvorlagen erforderlich machen.</span><span class="sxs-lookup"><span data-stu-id="80eb5-152">When using this pattern we create strongly-typed classes that are optimized for our specific view scenarios, and which expose properties for the dynamic values/content needed by our view templates.</span></span> <span data-ttu-id="80eb5-153">Unsere Controllerklassen können Auffüllen und diese Ansicht optimiert Klassen an unsere anzeigen, die zu verwendende Vorlage übergeben.</span><span class="sxs-lookup"><span data-stu-id="80eb5-153">Our controller classes can then populate and pass these view-optimized classes to our view template to use.</span></span> <span data-ttu-id="80eb5-154">Dies ermöglicht die typsicherheit, kompilierzeitüberprüfung und Editor IntelliSense in Vorlagen anzeigen.</span><span class="sxs-lookup"><span data-stu-id="80eb5-154">This enables type-safety, compile-time checking, and editor IntelliSense within view templates.</span></span>

<span data-ttu-id="80eb5-155">Wir erstellen zwei Ansichtsmodelle für die Verwendung in unserer Warenkorb-Controller: die ShoppingCartViewModel, den Inhalt der Einkaufswagen des Benutzers gespeichert werden, und die ShoppingCartRemoveViewModel wird verwendet, um Bestätigungsinformationen zur angezeigt, wenn ein Benutzer ein Element entfernt aus Einkaufswagen.</span><span class="sxs-lookup"><span data-stu-id="80eb5-155">We'll create two View Models for use in our Shopping Cart controller: the ShoppingCartViewModel will hold the contents of the user's shopping cart, and the ShoppingCartRemoveViewModel will be used to display confirmation information when a user removes something from their cart.</span></span>

<span data-ttu-id="80eb5-156">Erstellen Sie einen neuen Ordner für die ViewModels wir im Stammverzeichnis des unsere Projekt organisiert Dinge zu.</span><span class="sxs-lookup"><span data-stu-id="80eb5-156">Let's create a new ViewModels folder in the root of our project to keep things organized.</span></span> <span data-ttu-id="80eb5-157">Mit der rechten Maustaste des Projekts, wählen Sie die Add / neuen Ordner.</span><span class="sxs-lookup"><span data-stu-id="80eb5-157">Right-click the project, select Add / New Folder.</span></span>

![](mvc-music-store-part-8/_static/image1.jpg)

<span data-ttu-id="80eb5-158">Benennen Sie den Ordner ViewModels ein.</span><span class="sxs-lookup"><span data-stu-id="80eb5-158">Name the folder ViewModels.</span></span>

![](mvc-music-store-part-8/_static/image1.png)

<span data-ttu-id="80eb5-159">Als Nächstes fügen Sie die ShoppingCartViewModel-Klasse, im Ordner "ViewModels".</span><span class="sxs-lookup"><span data-stu-id="80eb5-159">Next, add the ShoppingCartViewModel class in the ViewModels folder.</span></span> <span data-ttu-id="80eb5-160">Sie verfügt über zwei Eigenschaften: eine Liste der Warenkorb und einen decimal-Wert, den Gesamtpreis für alle Elemente im Warenkorb enthalten soll.</span><span class="sxs-lookup"><span data-stu-id="80eb5-160">It has two properties: a list of Cart items, and a decimal value to hold the total price for all items in the cart.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

<span data-ttu-id="80eb5-161">Fügen Sie jetzt die ShoppingCartRemoveViewModel zum ViewModels Ordner, mit den folgenden vier Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="80eb5-161">Now add the ShoppingCartRemoveViewModel to the ViewModels folder, with the following four properties.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a><span data-ttu-id="80eb5-162">Die Einkaufswagencontroller</span><span class="sxs-lookup"><span data-stu-id="80eb5-162">The Shopping Cart Controller</span></span>

<span data-ttu-id="80eb5-163">Der Einkaufswagen Controller hat drei Hauptfunktionen: Hinzufügen von Elementen zu einem Einkaufswagen, Entfernen von Elementen aus dem Einkaufswagen und Anzeigen von Elementen in den Warenkorb.</span><span class="sxs-lookup"><span data-stu-id="80eb5-163">The Shopping Cart controller has three main purposes: adding items to a cart, removing items from the cart, and viewing items in the cart.</span></span> <span data-ttu-id="80eb5-164">Verwenden von drei Klassen wir werden vorgenommen erstellte: ShoppingCartViewModel ShoppingCartRemoveViewModel und ShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="80eb5-164">It will make use of the three classes we just created: ShoppingCartViewModel, ShoppingCartRemoveViewModel, and ShoppingCart.</span></span> <span data-ttu-id="80eb5-165">Wie bei den StoreController und StoreManagerController fügen wir ein Feld, um eine Instanz des MusicStoreEntities enthalten.</span><span class="sxs-lookup"><span data-stu-id="80eb5-165">As in the StoreController and StoreManagerController, we'll add a field to hold an instance of MusicStoreEntities.</span></span>

<span data-ttu-id="80eb5-166">Fügen Sie einen neuen Shopping Cart-Controller zum Projekt mit der Vorlage der leeren Controller.</span><span class="sxs-lookup"><span data-stu-id="80eb5-166">Add a new Shopping Cart controller to the project using the Empty controller template.</span></span>

![](mvc-music-store-part-8/_static/image2.png)

<span data-ttu-id="80eb5-167">Hier ist der vollständige ShoppingCart-Controller.</span><span class="sxs-lookup"><span data-stu-id="80eb5-167">Here's the complete ShoppingCart Controller.</span></span> <span data-ttu-id="80eb5-168">Die Aktionen "Index" und "Controller hinzufügen sollten sehr bekannt vorkommen.</span><span class="sxs-lookup"><span data-stu-id="80eb5-168">The Index and Add Controller actions should look very familiar.</span></span> <span data-ttu-id="80eb5-169">Die Controller-Aktionen entfernen und CartSummary behandeln zwei Sonderfälle, die im folgenden Abschnitt erläutert.</span><span class="sxs-lookup"><span data-stu-id="80eb5-169">The Remove and CartSummary controller actions handle two special cases, which we'll discuss in the following section.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a><span data-ttu-id="80eb5-170">AJAX-Updates mit jQuery</span><span class="sxs-lookup"><span data-stu-id="80eb5-170">Ajax Updates with jQuery</span></span>

<span data-ttu-id="80eb5-171">Wir müssen neben eine Shopping Cart Indexseite erstellen, die ist stark typisiert werden, um die ShoppingCartViewModel und die Listenansicht-Vorlage, die mit derselben Methode wie vor verwendet.</span><span class="sxs-lookup"><span data-stu-id="80eb5-171">We'll next create a Shopping Cart Index page that is strongly typed to the ShoppingCartViewModel and uses the List View template using the same method as before.</span></span>

![](mvc-music-store-part-8/_static/image3.png)

<span data-ttu-id="80eb5-172">Anstatt eine Html.ActionLink Elemente aus dem Einkaufswagen zu entfernen, müssen wir jedoch verwenden jQuery "Einrichten von Click-Ereignis für alle Links in dieser Sicht die HTML-RemoveLink Klasse Netzwerkdaten".</span><span class="sxs-lookup"><span data-stu-id="80eb5-172">However, instead of using an Html.ActionLink to remove items from the cart, we'll use jQuery to "wire up" the click event for all links in this view which have the HTML class RemoveLink.</span></span> <span data-ttu-id="80eb5-173">Anstatt die Buchung des Formulars, wird diese Click-Ereignishandler nur einen AJAX-Rückruf an unsere RemoveFromCart Controlleraktion vornehmen.</span><span class="sxs-lookup"><span data-stu-id="80eb5-173">Rather than posting the form, this click event handler will just make an AJAX callback to our RemoveFromCart controller action.</span></span> <span data-ttu-id="80eb5-174">Die RemoveFromCart gibt ein Ergebnis JSON serialisiert unsere jQuery-Rückruf dann analysiert und vier schnelle Updates auf der Seite unter Verwendung von jQuery ausführt:</span><span class="sxs-lookup"><span data-stu-id="80eb5-174">The RemoveFromCart returns a JSON serialized result, which our jQuery callback then parses and performs four quick updates to the page using jQuery:</span></span>

- 1. <span data-ttu-id="80eb5-175">Entfernt das gelöschte Album aus der Liste</span><span class="sxs-lookup"><span data-stu-id="80eb5-175">Removes the deleted album from the list</span></span>
- 2. <span data-ttu-id="80eb5-176">Aktualisiert die Warenkorb-Anzahl in der Kopfzeile</span><span class="sxs-lookup"><span data-stu-id="80eb5-176">Updates the cart count in the header</span></span>
- 3. <span data-ttu-id="80eb5-177">Zeigt dem Benutzer eine Änderungsnachricht</span><span class="sxs-lookup"><span data-stu-id="80eb5-177">Displays an update message to the user</span></span>
- 4. <span data-ttu-id="80eb5-178">Aktualisiert den Gesamtpreis Warenkorb</span><span class="sxs-lookup"><span data-stu-id="80eb5-178">Updates the cart total price</span></span>

<span data-ttu-id="80eb5-179">Da das Remove-Szenario durch ein Ajax-Rückruf in die Indexansicht behandelt wird, erforderlich wir eine zusätzliche Ansicht für nicht RemoveFromCart Aktion.</span><span class="sxs-lookup"><span data-stu-id="80eb5-179">Since the remove scenario is being handled by an Ajax callback within the Index view, we don't need an additional view for RemoveFromCart action.</span></span> <span data-ttu-id="80eb5-180">Hier wird der vollständige Code für die /ShoppingCart/Index-Ansicht:</span><span class="sxs-lookup"><span data-stu-id="80eb5-180">Here is the complete code for the /ShoppingCart/Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

<span data-ttu-id="80eb5-181">Um dies zu testen, müssen wir unsere warenkorbsoftware Elemente hinzugefügt werden können.</span><span class="sxs-lookup"><span data-stu-id="80eb5-181">In order to test this out, we need to be able to add items to our shopping cart.</span></span> <span data-ttu-id="80eb5-182">Aktualisieren wir unsere **Store Details** Sicht auf die Schaltfläche "Zum Warenkorb hinzufügen" enthalten.</span><span class="sxs-lookup"><span data-stu-id="80eb5-182">We'll update our **Store Details** view to include an "Add to cart" button.</span></span> <span data-ttu-id="80eb5-183">Während es bei dieser Gelegenheit können wir enthalten einige zusätzliche Informationen zum Album, der es hinzugefügt haben, da wir in dieser Ansicht zuletzt aktualisiert: "Genre", Interpret, Preis und Album Art.</span><span class="sxs-lookup"><span data-stu-id="80eb5-183">While we're at it, we can include some of the Album additional information which we've added since we last updated this view: Genre, Artist, Price, and Album Art.</span></span> <span data-ttu-id="80eb5-184">Der aktualisierte Code der Store Details anzeigen angezeigt wird, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="80eb5-184">The updated Store Details view code appears as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

<span data-ttu-id="80eb5-185">Jetzt können wir über den Store auf und hinzufügen und Entfernen von Alben zu und von unsere warenkorbsoftware zu testen.</span><span class="sxs-lookup"><span data-stu-id="80eb5-185">Now we can click through the store and test adding and removing Albums to and from our shopping cart.</span></span> <span data-ttu-id="80eb5-186">Führen Sie die Anwendung, und navigieren Sie zu der Columnstore-Index.</span><span class="sxs-lookup"><span data-stu-id="80eb5-186">Run the application and browse to the Store Index.</span></span>

![](mvc-music-store-part-8/_static/image4.png)

<span data-ttu-id="80eb5-187">Klicken Sie dann auf eine "Genre" zum Anzeigen einer Liste von Alben auf.</span><span class="sxs-lookup"><span data-stu-id="80eb5-187">Next, click on a Genre to view a list of albums.</span></span>

![](mvc-music-store-part-8/_static/image5.png)

<span data-ttu-id="80eb5-188">Albumtitel jetzt auf zeigt unseren aktualisierte Album Detailansicht, einschließlich der Schaltfläche "Zum Warenkorb hinzufügen".</span><span class="sxs-lookup"><span data-stu-id="80eb5-188">Clicking on an Album title now shows our updated Album Details view, including the "Add to cart" button.</span></span>

![](mvc-music-store-part-8/_static/image6.png)

<span data-ttu-id="80eb5-189">Klicken auf die Schaltfläche "Zum Warenkorb hinzufügen" zeigt unseren Shopping Cart-Index-Ansicht mit der shopping Cart Zusammenfassungsliste.</span><span class="sxs-lookup"><span data-stu-id="80eb5-189">Clicking the "Add to cart" button shows our Shopping Cart Index view with the shopping cart summary list.</span></span>

![](mvc-music-store-part-8/_static/image7.png)

<span data-ttu-id="80eb5-190">Nach dem Laden von Ihrem Einkaufswagen, können Sie auf Entfernen aus dem Einkaufswagen Link des Ajax-Updates zu Ihrem Warenkorb anzeigen klicken.</span><span class="sxs-lookup"><span data-stu-id="80eb5-190">After loading up your shopping cart, you can click on the Remove from cart link to see the Ajax update to your shopping cart.</span></span>

![](mvc-music-store-part-8/_static/image8.png)

<span data-ttu-id="80eb5-191">Wir haben eine funktionierende Einkaufswagen dadurch nicht registrierte Benutzer das Hinzufügen von Elementen zum Einkaufswagen erstellt.</span><span class="sxs-lookup"><span data-stu-id="80eb5-191">We've built out a working shopping cart which allows unregistered users to add items to their cart.</span></span> <span data-ttu-id="80eb5-192">Im folgenden Abschnitt werden wir ihnen das Registrieren und Ausführen des Auscheckvorgangs gewähren.</span><span class="sxs-lookup"><span data-stu-id="80eb5-192">In the following section, we'll allow them to register and complete the checkout process.</span></span>


>[!div class="step-by-step"]
<span data-ttu-id="80eb5-193">[Zurück](mvc-music-store-part-7.md)
[Weiter](mvc-music-store-part-9.md)</span><span class="sxs-lookup"><span data-stu-id="80eb5-193">[Previous](mvc-music-store-part-7.md)
[Next](mvc-music-store-part-9.md)</span></span>
