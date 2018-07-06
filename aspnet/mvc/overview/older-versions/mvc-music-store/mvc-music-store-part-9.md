---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: 'Teil 9: Registrierung und Bezahlvorgang | Microsoft-Dokumentation'
author: jongalloway
description: Dieser tutorialreihe werden alle Schritte ausgeführt, um die ASP.NET MVC Music Store-beispielanwendung zu erstellen. Teil 9 sind die Registrierung und Bezahlvorgang.
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: e521e30cb820d834d57c87538b869861a536657b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812872"
---
<a name="part-9-registration-and-checkout"></a><span data-ttu-id="42ead-104">Teil 9: Registrierung und Bezahlvorgang</span><span class="sxs-lookup"><span data-stu-id="42ead-104">Part 9: Registration and Checkout</span></span>
====================
<span data-ttu-id="42ead-105">durch [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="42ead-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="42ead-106">Die MVC Music Store ist ein lernprogrammanwendung, die eingeführt und erläutert Schritt für Schritt, wie ASP.NET MVC und Visual Studio für die Webentwicklung verwenden.</span><span class="sxs-lookup"><span data-stu-id="42ead-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="42ead-107">Die MVC Music Store ist eine Implementierung eines einfachen Beispiels die Alben online verkauft und implementiert grundlegende Verwaltung, Benutzeranmeldung und shopping Cart-Funktionalität.</span><span class="sxs-lookup"><span data-stu-id="42ead-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="42ead-108">Dieser tutorialreihe werden alle Schritte ausgeführt, um die ASP.NET MVC Music Store-beispielanwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="42ead-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="42ead-109">Teil 9 sind die Registrierung und Bezahlvorgang.</span><span class="sxs-lookup"><span data-stu-id="42ead-109">Part 9 covers Registration and Checkout.</span></span>


<span data-ttu-id="42ead-110">In diesem Abschnitt werden wir eine CheckoutController erstellen die des Kunden-Adresse und die Zahlungsinformationen gesammelt werden.</span><span class="sxs-lookup"><span data-stu-id="42ead-110">In this section, we will be creating a CheckoutController which will collect the shopper's address and payment information.</span></span> <span data-ttu-id="42ead-111">Wir müssen die Benutzer mit unserer Website vor dem Auschecken, zu registrieren, damit es sich bei diesem Controller Autorisierung erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="42ead-111">We will require users to register with our site prior to checking out, so this controller will require authorization.</span></span>

<span data-ttu-id="42ead-112">Benutzer werden zur des Auscheckvorgangs aus ihrem Einkaufswagen navigieren, auf die Schaltfläche "Kasse" klicken.</span><span class="sxs-lookup"><span data-stu-id="42ead-112">Users will navigate to the checkout process from their shopping cart by clicking the "Checkout" button.</span></span>

![](mvc-music-store-part-9/_static/image1.jpg)

<span data-ttu-id="42ead-113">Wenn der Benutzer nicht angemeldet ist, wird er zum aufgefordert werden.</span><span class="sxs-lookup"><span data-stu-id="42ead-113">If the user is not logged in, they will be prompted to.</span></span>

![](mvc-music-store-part-9/_static/image1.png)

<span data-ttu-id="42ead-114">Nach der erfolgreichen Anmeldung wird dem Benutzer dann die Ansicht und die Zahlungsmethode angezeigt.</span><span class="sxs-lookup"><span data-stu-id="42ead-114">Upon successful login, the user is then shown the Address and Payment view.</span></span>

![](mvc-music-store-part-9/_static/image2.png)

<span data-ttu-id="42ead-115">Nachdem sie das Formular ausgefüllt und übermittelt die Reihenfolge, werden sie dem Order-Bestätigungsbildschirm angezeigt.</span><span class="sxs-lookup"><span data-stu-id="42ead-115">Once they have filled the form and submitted the order, they will be shown the order confirmation screen.</span></span>

![](mvc-music-store-part-9/_static/image3.png)

<span data-ttu-id="42ead-116">Es wird versucht, entweder eine nicht existierende Bestellung oder eine Bestellung, die Ihnen gehören nicht, anzeigen, wird die Fehleransicht angezeigt.</span><span class="sxs-lookup"><span data-stu-id="42ead-116">Attempting to view either a non-existent order or an order that doesn't belong to you will show the Error view.</span></span>

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a><span data-ttu-id="42ead-117">Migrieren des Einkaufswagens</span><span class="sxs-lookup"><span data-stu-id="42ead-117">Migrating the Shopping Cart</span></span>

<span data-ttu-id="42ead-118">Während des Einkaufsvorgangs anonym ist, wird, wenn der Benutzer auf die Schaltfläche zum Auschecken klickt, sie müssen zum Registrieren und melden Sie sich.</span><span class="sxs-lookup"><span data-stu-id="42ead-118">While the shopping process is anonymous, when the user clicks on the Checkout button, they will be required to register and login.</span></span> <span data-ttu-id="42ead-119">Benutzer erwarten, dass, dass wir die Einkaufswagendaten Abonnenten‟, daher müssen wir die Einkaufswagendaten für einen Benutzer zuordnen, wenn sie die Registrierung oder Anmeldung abgeschlossen beibehält.</span><span class="sxs-lookup"><span data-stu-id="42ead-119">Users will expect that we will maintain their shopping cart information between visits, so we will need to associate the shopping cart information with a user when they complete registration or login.</span></span>

<span data-ttu-id="42ead-120">Dies ist tatsächlich sehr einfach ist, an, wie unsere ShoppingCart-Klasse bereits eine Methode verfügt über die alle Elemente in der aktuellen Einkaufswagen mit einem Benutzernamen zuordnen.</span><span class="sxs-lookup"><span data-stu-id="42ead-120">This is actually very simple to do, as our ShoppingCart class already has a method which will associate all the items in the current cart with a username.</span></span> <span data-ttu-id="42ead-121">Wir benötigen nur diese Methode aufgerufen, wenn ein Benutzer auf Registrierung oder Anmeldung abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="42ead-121">We will just need to call this method when a user completes registration or login.</span></span>

<span data-ttu-id="42ead-122">Öffnen der **AccountController** -Klasse, die wir hinzugefügt, wenn wir Sie Mitgliedschaft und Autorisierung einrichten wurden.</span><span class="sxs-lookup"><span data-stu-id="42ead-122">Open the **AccountController** class that we added when we were setting up Membership and Authorization.</span></span> <span data-ttu-id="42ead-123">Hinzufügen einer using-Anweisung verweisen auf MvcMusicStore.Models, dann fügen Sie die folgende MigrateShoppingCart-Methode:</span><span class="sxs-lookup"><span data-stu-id="42ead-123">Add a using statement referencing MvcMusicStore.Models, then add the following MigrateShoppingCart method:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

<span data-ttu-id="42ead-124">Als Nächstes ändern Sie die Aktion nach der Anmeldung zum MigrateShoppingCart aufrufen, nachdem der Benutzer überprüft wurde, wie unten dargestellt:</span><span class="sxs-lookup"><span data-stu-id="42ead-124">Next, modify the LogOn post action to call MigrateShoppingCart after the user has been validated, as shown below:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

<span data-ttu-id="42ead-125">Stellen Sie die gleiche Änderung das Register-Aktion zu buchen, sofort, nachdem das Benutzerkonto erfolgreich erstellt wurde:</span><span class="sxs-lookup"><span data-stu-id="42ead-125">Make the same change to the Register post action, immediately after the user account is successfully created:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

<span data-ttu-id="42ead-126">Das ist alles – nun eine anonyme Einkaufswagen eines Benutzerkontos nach erfolgreicher Registrierung oder Anmeldung automatisch übertragen werden sollen.</span><span class="sxs-lookup"><span data-stu-id="42ead-126">That's it - now an anonymous shopping cart will be automatically transferred to a user account upon successful registration or login.</span></span>

## <a name="creating-the-checkoutcontroller"></a><span data-ttu-id="42ead-127">Erstellen die CheckoutController</span><span class="sxs-lookup"><span data-stu-id="42ead-127">Creating the CheckoutController</span></span>

<span data-ttu-id="42ead-128">Mit der rechten Maustaste auf den Ordner "Controllers", und fügen Sie einen neuen Controller zum-Projekt namens CheckoutController mithilfe der Vorlage der leeren Controller hinzu.</span><span class="sxs-lookup"><span data-stu-id="42ead-128">Right-click on the Controllers folder and add a new Controller to the project named CheckoutController using the Empty controller template.</span></span>

![](mvc-music-store-part-9/_static/image5.png)

<span data-ttu-id="42ead-129">Fügen Sie zunächst das Authorize-Attribut über der Controller-Klassendeklaration Benutzer registrieren, vor dem Auschecken erforderlich ist:</span><span class="sxs-lookup"><span data-stu-id="42ead-129">First, add the Authorize attribute above the Controller class declaration to require users to register before checkout:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

<span data-ttu-id="42ead-130">*Hinweis: Dies ist vergleichbar mit der Änderung, die wir zuvor an die StoreManagerController vorgenommen, aber in diesem Fall wird in das Authorize-Attribut erforderlich sind, dass der Benutzer Mitglied in einer Administratorrolle sein. Im Controller Auschecken sind wir erfordern, der Benutzer angemeldet sein, jedoch sind nicht erfordern, dass sie sich Administratoren.*</span><span class="sxs-lookup"><span data-stu-id="42ead-130">*Note: This is similar to the change we previously made to the StoreManagerController, but in that case the Authorize attribute required that the user be in an Administrator role. In the Checkout Controller, we're requiring the user be logged in but aren't requiring that they be administrators.*</span></span>

<span data-ttu-id="42ead-131">Der Einfachheit halber wird nicht wir mit Zahlungsinformationen in diesem Tutorial zu tun.</span><span class="sxs-lookup"><span data-stu-id="42ead-131">For the sake of simplicity, we won't be dealing with payment information in this tutorial.</span></span> <span data-ttu-id="42ead-132">Stattdessen lassen wir die Benutzer sehen Sie sich mit einem Angebotscode.</span><span class="sxs-lookup"><span data-stu-id="42ead-132">Instead, we are allowing users to check out using a promotional code.</span></span> <span data-ttu-id="42ead-133">Wir werden diese über eine Konstante, die mit dem Namen PromoCode Angebotscode gespeichert.</span><span class="sxs-lookup"><span data-stu-id="42ead-133">We will store this promotional code using a constant named PromoCode.</span></span>

<span data-ttu-id="42ead-134">Wie in der StoreController werden wir ein Feld für eine Instanz der MusicStoreEntities-Klasse, mit dem Namen StoreDB deklarieren.</span><span class="sxs-lookup"><span data-stu-id="42ead-134">As in the StoreController, we'll declare a field to hold an instance of the MusicStoreEntities class, named storeDB.</span></span> <span data-ttu-id="42ead-135">Um Stellen der MusicStoreEntities-Klasse verwenden, müssen wir zum Hinzufügen einer using-Anweisung für den MvcMusicStore.Models-Namespace.</span><span class="sxs-lookup"><span data-stu-id="42ead-135">In order to make use of the MusicStoreEntities class, we will need to add a using statement for the MvcMusicStore.Models namespace.</span></span> <span data-ttu-id="42ead-136">Der oberste des Controllers Auschecken wird unten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="42ead-136">The top of our Checkout controller appears below.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

<span data-ttu-id="42ead-137">Die CheckoutController verfügen die folgenden Controlleraktionen:</span><span class="sxs-lookup"><span data-stu-id="42ead-137">The CheckoutController will have the following controller actions:</span></span>

<span data-ttu-id="42ead-138">**AddressAndPayment (GET-Methode)** zeigt ein Formular, um dem Benutzer ermöglichen, ihre Informationen eingeben.</span><span class="sxs-lookup"><span data-stu-id="42ead-138">**AddressAndPayment (GET method)** will display a form to allow the user to enter their information.</span></span>

<span data-ttu-id="42ead-139">**AddressAndPayment (POST-Methode)** wird die Überprüfung der Eingabe- und die Reihenfolge zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="42ead-139">**AddressAndPayment (POST method)** will validate the input and process the order.</span></span>

<span data-ttu-id="42ead-140">**Vollständige** wird angezeigt, nachdem ein Benutzer den Kassenvorgang erfolgreich abgeschlossen wurde.</span><span class="sxs-lookup"><span data-stu-id="42ead-140">**Complete** will be shown after a user has successfully finished the checkout process.</span></span> <span data-ttu-id="42ead-141">In dieser Ansicht werden fortlaufende Nummer des Benutzers, als Bestätigung enthalten.</span><span class="sxs-lookup"><span data-stu-id="42ead-141">This view will include the user's order number, as confirmation.</span></span>

<span data-ttu-id="42ead-142">Zuerst benennen Sie die Index-Controlleraktion (die generiert wurde, beim Erstellen des Controllers) lassen Sie uns in AddressAndPayment.</span><span class="sxs-lookup"><span data-stu-id="42ead-142">First, let's rename the Index controller action (which was generated when we created the controller) to AddressAndPayment.</span></span> <span data-ttu-id="42ead-143">Diese Controlleraktion zeigt nur das Formular Auschecken, damit alle Informationen zum Modell erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="42ead-143">This controller action just displays the checkout form, so it doesn't require any model information.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

<span data-ttu-id="42ead-144">Unsere AddressAndPayment POST-Methode wird folgen dem gleichen Muster, die wir in den StoreManagerController verwendet: Es wird versucht, akzeptieren die Übermittlung des Formulars, und führen die Reihenfolge und das Formular wird erneut angezeigt werden, wenn ein Fehler auftritt.</span><span class="sxs-lookup"><span data-stu-id="42ead-144">Our AddressAndPayment POST method will follow the same pattern we used in the StoreManagerController: it will try to accept the form submission and complete the order, and will re-display the form if it fails.</span></span>

<span data-ttu-id="42ead-145">Nach unserem überprüfungsanforderungen für einen Auftrag überprüfen die Formulareingabe erfüllt werden, werden wir den PromoCode-Formular-Wert direkt überprüfen.</span><span class="sxs-lookup"><span data-stu-id="42ead-145">After validating the form input meets our validation requirements for an Order, we will check the PromoCode form value directly.</span></span> <span data-ttu-id="42ead-146">Vorausgesetzt, dass alles korrekt ist, speichern wir die aktualisierte Informationen mit der Reihenfolge, teilen Sie das ShoppingCart-Objekt, das den Auftrag abzuschließen, und leiten Sie an die Aktion abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="42ead-146">Assuming everything is correct, we will save the updated information with the order, tell the ShoppingCart object to complete the order process, and redirect to the Complete action.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

<span data-ttu-id="42ead-147">Nach dem erfolgreichen Abschluss des Prozesses Auschecken werden Benutzer auf die vollständige Controlleraktion umgeleitet.</span><span class="sxs-lookup"><span data-stu-id="42ead-147">Upon successful completion of the checkout process, users will be redirected to the Complete controller action.</span></span> <span data-ttu-id="42ead-148">Diese Aktion wird eine einfache Überprüfung, um sicherzustellen, dass die Reihenfolge tatsächlich für den angemeldeten Benutzer gehört, bevor Sie mit der Bestellnummer als Bestätigung durchführen.</span><span class="sxs-lookup"><span data-stu-id="42ead-148">This action will perform a simple check to validate that the order does indeed belong to the logged-in user before showing the order number as a confirmation.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

<span data-ttu-id="42ead-149">*Hinweis: Die Fehleransicht wurde automatisch erstellt, für uns im Ordner "/Views/Shared" als wir zu des Projekts Beginn.*</span><span class="sxs-lookup"><span data-stu-id="42ead-149">*Note: The Error view was automatically created for us in the /Views/Shared folder when we began the project.*</span></span>

<span data-ttu-id="42ead-150">Der vollständige CheckoutController Code lautet wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="42ead-150">The complete CheckoutController code is as follows:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a><span data-ttu-id="42ead-151">Hinzufügen der AddressAndPayment-Ansicht</span><span class="sxs-lookup"><span data-stu-id="42ead-151">Adding the AddressAndPayment view</span></span>

<span data-ttu-id="42ead-152">Nun erstellen wir die AddressAndPayment-Ansicht.</span><span class="sxs-lookup"><span data-stu-id="42ead-152">Now, let's create the AddressAndPayment view.</span></span> <span data-ttu-id="42ead-153">Mit der rechten Maustaste auf eine der Aktionen AddressAndPayment Controller aus, und fügen Sie eine Ansicht mit dem Namen AddressAndPayment ist stark typisiert, wie eine Bestellung und verwendet die Vorlage bearbeiten, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="42ead-153">Right-click on one of the AddressAndPayment controller actions and add a view named AddressAndPayment which is strongly typed as an Order and uses the Edit template, as shown below.</span></span>

![](mvc-music-store-part-9/_static/image6.png)

<span data-ttu-id="42ead-154">In dieser Ansicht wird stellen zwei Verfahren erläutert, während der Erstellung der Sicht StoreManagerEdit verwenden:</span><span class="sxs-lookup"><span data-stu-id="42ead-154">This view will make use of two of the techniques we looked at while building the StoreManagerEdit view:</span></span>

- <span data-ttu-id="42ead-155">Wir verwenden Html.EditorForModel(), um Felder für das Modell Reihenfolge anzeigen</span><span class="sxs-lookup"><span data-stu-id="42ead-155">We will use Html.EditorForModel() to display form fields for the Order model</span></span>
- <span data-ttu-id="42ead-156">Wir werden Validierungsregeln, die mit einer Order-Klasse mit Validierungsattributen nutzen.</span><span class="sxs-lookup"><span data-stu-id="42ead-156">We will leverage validation rules using an Order class with validation attributes</span></span>

<span data-ttu-id="42ead-157">Wir beginnen, durch die Aktualisierung der Formularcode, um Html.EditorForModel(), gefolgt von einer zusätzlichen Textfeld für die Promo-Code verwenden.</span><span class="sxs-lookup"><span data-stu-id="42ead-157">We'll start by updating the form code to use Html.EditorForModel(), followed by an additional textbox for the Promo Code.</span></span> <span data-ttu-id="42ead-158">Der vollständige Code für die Ansicht AddressAndPayment ist unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="42ead-158">The complete code for the AddressAndPayment view is shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a><span data-ttu-id="42ead-159">Definieren von Validierungsregeln für den Auftrag</span><span class="sxs-lookup"><span data-stu-id="42ead-159">Defining validation rules for the Order</span></span>

<span data-ttu-id="42ead-160">Nun, da unsere Ansicht eingerichtet ist, werden wir die Validierungsregeln für unser Modell der Reihenfolge eingerichtet wie schon zuvor für das Album-Modell.</span><span class="sxs-lookup"><span data-stu-id="42ead-160">Now that our view is set up, we will set up the validation rules for our Order model as we did previously for the Album model.</span></span> <span data-ttu-id="42ead-161">Mit der rechten Maustaste auf den Ordner "Models", und fügen Sie eine Klasse, die mit dem Namen Bestellung.</span><span class="sxs-lookup"><span data-stu-id="42ead-161">Right-click on the Models folder and add a class named Order.</span></span> <span data-ttu-id="42ead-162">Zusätzlich zu den Validierungsattributen, die wir zuvor für das Album verwendet wird, werden wir auch einen regulären Ausdruck verwenden, um die e-Mail-Adresse des Benutzers zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="42ead-162">In addition to the validation attributes we used previously for the Album, we will also be using a Regular Expression to validate the user's email address.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

<span data-ttu-id="42ead-163">Es wird versucht, senden Sie das Formular mit fehlenden oder ungültige Informationen werden nun die clientseitige Validierung mit Fehlermeldung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="42ead-163">Attempting to submit the form with missing or invalid information will now show error message using client-side validation.</span></span>

![](mvc-music-store-part-9/_static/image7.png)

<span data-ttu-id="42ead-164">Okay, haben wir die meisten die harte Arbeit für den Kassenvorgang ausgeführt; Wir müssen lediglich ein paar Wahrscheinlichkeiten und endet um den Vorgang abzuschließen.</span><span class="sxs-lookup"><span data-stu-id="42ead-164">Okay, we've done most of the hard work for the checkout process; we just have a few odds and ends to finish.</span></span> <span data-ttu-id="42ead-165">Wir müssen zwei einfache Ansichten hinzufügen, und wir die Übergabe der Warenkorb Informationen während des Anmeldevorgangs kümmern müssen.</span><span class="sxs-lookup"><span data-stu-id="42ead-165">We need to add two simple views, and we need to take care of the handoff of the cart information during the login process.</span></span>

## <a name="adding-the-checkout-complete-view"></a><span data-ttu-id="42ead-166">Die vollständige Auschecken Ansicht hinzufügen</span><span class="sxs-lookup"><span data-stu-id="42ead-166">Adding the Checkout Complete view</span></span>

<span data-ttu-id="42ead-167">Die vollständige Auschecken Ansicht ist ziemlich einfach, wie er lediglich die Auftrags-ID anzeigen muss</span><span class="sxs-lookup"><span data-stu-id="42ead-167">The Checkout Complete view is pretty simple, as it just needs to display the Order ID.</span></span> <span data-ttu-id="42ead-168">Mit der rechten Maustaste auf die vollständige Controlleraktion aus, und fügen Sie eine Ansicht mit dem Namen abschließen, die als ganze Zahl stark typisiert ist</span><span class="sxs-lookup"><span data-stu-id="42ead-168">Right-click on the Complete controller action and add a view named Complete which is strongly typed as an int.</span></span>

![](mvc-music-store-part-9/_static/image8.png)

<span data-ttu-id="42ead-169">Jetzt aktualisieren wir den Code anzeigen, zum Anzeigen der Auftrags-ID, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="42ead-169">Now we will update the view code to display the Order ID, as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a><span data-ttu-id="42ead-170">Aktualisieren die Fehler anzeigen</span><span class="sxs-lookup"><span data-stu-id="42ead-170">Updating The Error view</span></span>

<span data-ttu-id="42ead-171">Die Standardvorlage enthält eine Fehleransicht im freigegebenen Ordner "Views", so, dass es nicht erneut verwendet an anderer Stelle auf der Website werden kann.</span><span class="sxs-lookup"><span data-stu-id="42ead-171">The default template includes an Error view in the Shared views folder so that it can be re-used elsewhere in the site.</span></span> <span data-ttu-id="42ead-172">Dieser Fehler-Ansicht enthält einen sehr einfachen Fehler und der Layout-Website nicht verwendet werden, damit wir ihn aktualisieren müssen.</span><span class="sxs-lookup"><span data-stu-id="42ead-172">This Error view contains a very simple error and doesn't use our site Layout, so we'll update it.</span></span>

<span data-ttu-id="42ead-173">Da dies eine generische Fehlerseite ist, wird der Inhalt sehr einfach.</span><span class="sxs-lookup"><span data-stu-id="42ead-173">Since this is a generic error page, the content is very simple.</span></span> <span data-ttu-id="42ead-174">Wir nehmen eine Nachricht und einen Link, um zur vorherigen Seite im Verlauf zu navigieren, wenn möchte, dass der Benutzer die Aktion erneut versucht.</span><span class="sxs-lookup"><span data-stu-id="42ead-174">We'll include a message and a link to navigate to the previous page in history if the user wants to re-try their action.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


> [!div class="step-by-step"]
> <span data-ttu-id="42ead-175">[Zurück](mvc-music-store-part-8.md)
> [Weiter](mvc-music-store-part-10.md)</span><span class="sxs-lookup"><span data-stu-id="42ead-175">[Previous](mvc-music-store-part-8.md)
[Next](mvc-music-store-part-10.md)</span></span>
