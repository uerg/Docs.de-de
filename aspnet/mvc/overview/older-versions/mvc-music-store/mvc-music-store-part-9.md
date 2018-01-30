---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: 'Teil 9: Registrierung und Kasse | Microsoft Docs'
author: jongalloway
description: Diese Reihe von Lernprogrammen details aller die Schritte zum Erstellen von ASP.NET MVC-Music Store-beispielanwendung. Registrierung und Auschecken, werden Teil 9 behandelt.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: 71f87043be064d24bdfb203380fb6cf651527e30
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
<a name="part-9-registration-and-checkout"></a><span data-ttu-id="467e8-104">Teil 9: Registrierung und Kasse</span><span class="sxs-lookup"><span data-stu-id="467e8-104">Part 9: Registration and Checkout</span></span>
====================
<span data-ttu-id="467e8-105">durch [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="467e8-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="467e8-106">Das MVC-Music Store ist eine lernprogrammanwendung, die führt und erklärt schrittweise, wie mithilfe von ASP.NET MVC und Visual Studio für die Webentwicklung.</span><span class="sxs-lookup"><span data-stu-id="467e8-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="467e8-107">Das MVC-Music Store handelt es sich um einfache Beispiel Store Implementierung, die online Musikalben verkauft und implementiert grundlegende standortverwaltung Benutzer anmelden und shopping Cart-Funktionalität.</span><span class="sxs-lookup"><span data-stu-id="467e8-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="467e8-108">Diese Reihe von Lernprogrammen details aller die Schritte zum Erstellen von ASP.NET MVC-Music Store-beispielanwendung.</span><span class="sxs-lookup"><span data-stu-id="467e8-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="467e8-109">Registrierung und Auschecken, werden Teil 9 behandelt.</span><span class="sxs-lookup"><span data-stu-id="467e8-109">Part 9 covers Registration and Checkout.</span></span>


<span data-ttu-id="467e8-110">In diesem Abschnitt wird eine CheckoutController erstellen wir werden die Adresse des Käufers und Zahlungsinformationen gesammelt werden.</span><span class="sxs-lookup"><span data-stu-id="467e8-110">In this section, we will be creating a CheckoutController which will collect the shopper's address and payment information.</span></span> <span data-ttu-id="467e8-111">Wir müssen Benutzer mit unserer Website vor dem Auschecken, registrieren Sie sich damit diesen Controller Autorisierung erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="467e8-111">We will require users to register with our site prior to checking out, so this controller will require authorization.</span></span>

<span data-ttu-id="467e8-112">Benutzer werden zu des Auscheckvorgangs aus Einkaufswagen navigieren, indem Sie auf die Schaltfläche "Kasse".</span><span class="sxs-lookup"><span data-stu-id="467e8-112">Users will navigate to the checkout process from their shopping cart by clicking the "Checkout" button.</span></span>

![](mvc-music-store-part-9/_static/image1.jpg)

<span data-ttu-id="467e8-113">Wenn der Benutzer nicht angemeldet ist, wird er auf aufgefordert werden.</span><span class="sxs-lookup"><span data-stu-id="467e8-113">If the user is not logged in, they will be prompted to.</span></span>

![](mvc-music-store-part-9/_static/image1.png)

<span data-ttu-id="467e8-114">Nach der erfolgreichen Anmeldung wird der Benutzer dann die Adresse "und" Payment-Ansicht angezeigt.</span><span class="sxs-lookup"><span data-stu-id="467e8-114">Upon successful login, the user is then shown the Address and Payment view.</span></span>

![](mvc-music-store-part-9/_static/image2.png)

<span data-ttu-id="467e8-115">Nachdem sie das Formular ausgefüllt und übermittelt die Reihenfolge, werden sie dem Bestätigungsbildschirm Reihenfolge angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="467e8-115">Once they have filled the form and submitted the order, they will be shown the order confirmation screen.</span></span>

![](mvc-music-store-part-9/_static/image3.png)

<span data-ttu-id="467e8-116">Versuchen, eine nicht existierende Bestellung oder eine Bestellung, die Ihnen gehört anzuzeigen, wird der Fehler-Sicht angezeigt.</span><span class="sxs-lookup"><span data-stu-id="467e8-116">Attempting to view either a non-existent order or an order that doesn't belong to you will show the Error view.</span></span>

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a><span data-ttu-id="467e8-117">Migrieren den Einkaufswagen</span><span class="sxs-lookup"><span data-stu-id="467e8-117">Migrating the Shopping Cart</span></span>

<span data-ttu-id="467e8-118">Während des Einkaufsvorgangs anonym ist, wird der Benutzer klickt auf die Schaltfläche "Auschecken", werden sie beim Registrieren benötigt und melden Sie sich.</span><span class="sxs-lookup"><span data-stu-id="467e8-118">While the shopping process is anonymous, when the user clicks on the Checkout button, they will be required to register and login.</span></span> <span data-ttu-id="467e8-119">Benutzer erwarten, dass, dass wir ihre Einkaufswagendaten zwischen Besuchen bewahren, damit ein Benutzer die Einkaufswagendaten zuordnen, wenn sie die Registrierung oder Anmeldung abgeschlossen werden muss.</span><span class="sxs-lookup"><span data-stu-id="467e8-119">Users will expect that we will maintain their shopping cart information between visits, so we will need to associate the shopping cart information with a user when they complete registration or login.</span></span>

<span data-ttu-id="467e8-120">Dies ist tatsächlich sehr einfach zum Zweck, wie unsere ShoppingCart-Klasse bereits über eine Methode verfügt, die alle Elemente in der aktuellen Einkaufswagen mit einem Benutzernamen zuordnen.</span><span class="sxs-lookup"><span data-stu-id="467e8-120">This is actually very simple to do, as our ShoppingCart class already has a method which will associate all the items in the current cart with a username.</span></span> <span data-ttu-id="467e8-121">Wir benötigen nur diese Methode aufgerufen, wenn ein Benutzer auf Registrierung oder Anmeldung abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="467e8-121">We will just need to call this method when a user completes registration or login.</span></span>

<span data-ttu-id="467e8-122">Öffnen der **AccountController** -Klasse, die wir hinzugefügt, wenn wir Mitgliedschaft einrichten und der Autorisierung wurden.</span><span class="sxs-lookup"><span data-stu-id="467e8-122">Open the **AccountController** class that we added when we were setting up Membership and Authorization.</span></span> <span data-ttu-id="467e8-123">Hinzufügen einer Anweisung verweisen auf MvcMusicStore.Models, dann fügen Sie die folgenden MigrateShoppingCart-Methode:</span><span class="sxs-lookup"><span data-stu-id="467e8-123">Add a using statement referencing MvcMusicStore.Models, then add the following MigrateShoppingCart method:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

<span data-ttu-id="467e8-124">Im nächsten Schritt ändern Sie die Aktion nach der Anmeldung um MigrateShoppingCart aufgerufen wird, nachdem der Benutzer überprüft wurde, wie unten dargestellt:</span><span class="sxs-lookup"><span data-stu-id="467e8-124">Next, modify the LogOn post action to call MigrateShoppingCart after the user has been validated, as shown below:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

<span data-ttu-id="467e8-125">Stellen Sie die gleiche Änderung registriert wird, die Aktion, post, sofort, nachdem das Benutzerkonto erfolgreich erstellt wurde:</span><span class="sxs-lookup"><span data-stu-id="467e8-125">Make the same change to the Register post action, immediately after the user account is successfully created:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

<span data-ttu-id="467e8-126">Das ist alles -, jetzt eine anonyme Einkaufswagen ein Benutzerkonto nach erfolgreicher Registrierung oder Anmeldung automatisch übertragen werden sollen.</span><span class="sxs-lookup"><span data-stu-id="467e8-126">That's it - now an anonymous shopping cart will be automatically transferred to a user account upon successful registration or login.</span></span>

## <a name="creating-the-checkoutcontroller"></a><span data-ttu-id="467e8-127">Erstellen die CheckoutController</span><span class="sxs-lookup"><span data-stu-id="467e8-127">Creating the CheckoutController</span></span>

<span data-ttu-id="467e8-128">Mit der rechten Maustaste auf den Ordner Controller, und fügen Sie einen neuen Domänencontroller für das Projekt mit dem Namen CheckoutController mithilfe der Vorlage für leere Controller hinzu.</span><span class="sxs-lookup"><span data-stu-id="467e8-128">Right-click on the Controllers folder and add a new Controller to the project named CheckoutController using the Empty controller template.</span></span>

![](mvc-music-store-part-9/_static/image5.png)

<span data-ttu-id="467e8-129">Fügen Sie zunächst die Authorize-Attribut über die Klassendeklaration Controller Benutzer registrieren, bevor Sie Auschecken erforderlich ist:</span><span class="sxs-lookup"><span data-stu-id="467e8-129">First, add the Authorize attribute above the Controller class declaration to require users to register before checkout:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

<span data-ttu-id="467e8-130">*Hinweis: Dies ist vergleichbar mit der Änderung, die wir zuvor die StoreManagerController vorgenommen haben, aber in diesem Fall wird in Authorize-Attribut erforderlich sind, dass der Benutzer Mitglied einer Administratorrolle sein. Im Controller auschecken möchten wir erfordern, der Benutzer angemeldet sein, jedoch sind nicht erfordern, dass diese Administratoren.*</span><span class="sxs-lookup"><span data-stu-id="467e8-130">*Note: This is similar to the change we previously made to the StoreManagerController, but in that case the Authorize attribute required that the user be in an Administrator role. In the Checkout Controller, we're requiring the user be logged in but aren't requiring that they be administrators.*</span></span>

<span data-ttu-id="467e8-131">Der Einfachheit halber wird nicht wir Umgang mit Zahlungsinformationen in diesem Lernprogramm werden.</span><span class="sxs-lookup"><span data-stu-id="467e8-131">For the sake of simplicity, we won't be dealing with payment information in this tutorial.</span></span> <span data-ttu-id="467e8-132">Stattdessen werden wir Benutzer sehen Sie sich mithilfe eines Werbe-Codes ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="467e8-132">Instead, we are allowing users to check out using a promotional code.</span></span> <span data-ttu-id="467e8-133">Wir werden diesen Angebotscode eine Konstante mit dem Namen PromoCode gespeichert.</span><span class="sxs-lookup"><span data-stu-id="467e8-133">We will store this promotional code using a constant named PromoCode.</span></span>

<span data-ttu-id="467e8-134">Ein Feld an eine Instanz der MusicStoreEntities-Klasse, die mit dem Namen StoreDB halten müssen wie in der StoreController deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="467e8-134">As in the StoreController, we'll declare a field to hold an instance of the MusicStoreEntities class, named storeDB.</span></span> <span data-ttu-id="467e8-135">Damit auch der MusicStoreEntities-Klasse verwenden, müssen wir hinzufügen, eine using-Anweisung für den MvcMusicStore.Models-Namespace.</span><span class="sxs-lookup"><span data-stu-id="467e8-135">In order to make use of the MusicStoreEntities class, we will need to add a using statement for the MvcMusicStore.Models namespace.</span></span> <span data-ttu-id="467e8-136">Am Anfang unserer Checkout-Controller wird unten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="467e8-136">The top of our Checkout controller appears below.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

<span data-ttu-id="467e8-137">Die CheckoutController müssen die folgenden Controlleraktionen:</span><span class="sxs-lookup"><span data-stu-id="467e8-137">The CheckoutController will have the following controller actions:</span></span>

<span data-ttu-id="467e8-138">**AddressAndPayment (GET-Methode)** zeigt ein Formular, um den Benutzer zur Eingabe ihrer Informationen zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="467e8-138">**AddressAndPayment (GET method)** will display a form to allow the user to enter their information.</span></span>

<span data-ttu-id="467e8-139">**AddressAndPayment (POST-Methode)** wird die Überprüfung der Eingabe- und die Reihenfolge zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="467e8-139">**AddressAndPayment (POST method)** will validate the input and process the order.</span></span>

<span data-ttu-id="467e8-140">**Vollständige** wird angezeigt, nachdem ein Benutzer des Auscheckvorgangs erfolgreich beendet wurde.</span><span class="sxs-lookup"><span data-stu-id="467e8-140">**Complete** will be shown after a user has successfully finished the checkout process.</span></span> <span data-ttu-id="467e8-141">In dieser Ansicht wird als Bestätigung des Benutzers Reihenfolgennummer enthalten.</span><span class="sxs-lookup"><span data-stu-id="467e8-141">This view will include the user's order number, as confirmation.</span></span>

<span data-ttu-id="467e8-142">Erstens wir benennen die Index-Controlleraktion (die generiert wurde, wenn wir den Controller erstellt), AddressAndPayment.</span><span class="sxs-lookup"><span data-stu-id="467e8-142">First, let's rename the Index controller action (which was generated when we created the controller) to AddressAndPayment.</span></span> <span data-ttu-id="467e8-143">Diese Controlleraktion zeigt nur das Formular Auschecken alle Modellinformationen benötigen Sie nicht.</span><span class="sxs-lookup"><span data-stu-id="467e8-143">This controller action just displays the checkout form, so it doesn't require any model information.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

<span data-ttu-id="467e8-144">Unsere AddressAndPayment POST-Methode wird folgen dem gleichen Muster, die wir in der StoreManagerController verwendet: Es wird versucht, die Übermittlung des Formulars übernehmen und die Reihenfolge zu beenden und das Formular erneut angezeigt, wenn ein Fehler auftritt.</span><span class="sxs-lookup"><span data-stu-id="467e8-144">Our AddressAndPayment POST method will follow the same pattern we used in the StoreManagerController: it will try to accept the form submission and complete the order, and will re-display the form if it fails.</span></span>

<span data-ttu-id="467e8-145">Nach unserer Prüfungsanforderungen für einen Auftrag überprüfen die Formulareingabe erfüllt werden, werden wir die PromoCode Formularwert direkt überprüfen.</span><span class="sxs-lookup"><span data-stu-id="467e8-145">After validating the form input meets our validation requirements for an Order, we will check the PromoCode form value directly.</span></span> <span data-ttu-id="467e8-146">Vorausgesetzt, dass alles richtig ist, wird es speichert die aktualisierte Informationen mit der Reihenfolge, sagen Sie das ShoppingCart-Objekt, das den Auftrag abzuschließen, und leiten Sie die Aktion abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="467e8-146">Assuming everything is correct, we will save the updated information with the order, tell the ShoppingCart object to complete the order process, and redirect to the Complete action.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

<span data-ttu-id="467e8-147">Nach erfolgreichem Abschluss des Prozesses Auschecken werden Benutzer auf die Aktion der controllerrolle abschließen umgeleitet.</span><span class="sxs-lookup"><span data-stu-id="467e8-147">Upon successful completion of the checkout process, users will be redirected to the Complete controller action.</span></span> <span data-ttu-id="467e8-148">Diese Aktion wird einer einfachen Überprüfung, um zu überprüfen, dass der Auftrag tatsächlich für den angemeldeten Benutzer gehört, bevor Sie mit der Bestellnummer als Bestätigung.</span><span class="sxs-lookup"><span data-stu-id="467e8-148">This action will perform a simple check to validate that the order does indeed belong to the logged-in user before showing the order number as a confirmation.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

<span data-ttu-id="467e8-149">*Hinweis: Die Fehleransicht wurde automatisch erstellt für uns im Ordner "/Views/Shared", wenn wir das Projekt wurde gestartet.*</span><span class="sxs-lookup"><span data-stu-id="467e8-149">*Note: The Error view was automatically created for us in the /Views/Shared folder when we began the project.*</span></span>

<span data-ttu-id="467e8-150">Der vollständige CheckoutController Code lautet wie folgt:</span><span class="sxs-lookup"><span data-stu-id="467e8-150">The complete CheckoutController code is as follows:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a><span data-ttu-id="467e8-151">Hinzufügen der AddressAndPayment-Ansicht</span><span class="sxs-lookup"><span data-stu-id="467e8-151">Adding the AddressAndPayment view</span></span>

<span data-ttu-id="467e8-152">Jetzt erstellen wir die AddressAndPayment-Sicht.</span><span class="sxs-lookup"><span data-stu-id="467e8-152">Now, let's create the AddressAndPayment view.</span></span> <span data-ttu-id="467e8-153">Mit der rechten Maustaste auf eines der AddressAndPayment-Controlleraktionen und fügen Sie eine Ansicht mit dem Namen AddressAndPayment stark typisiert ist, als eine Bestellung und verwendet die Vorlage bearbeiten, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="467e8-153">Right-click on one of the the AddressAndPayment controller actions and add a view named AddressAndPayment which is strongly typed as an Order and uses the Edit template, as shown below.</span></span>

![](mvc-music-store-part-9/_static/image6.png)

<span data-ttu-id="467e8-154">In dieser Ansicht wird stellen zwei Techniken erläutert, während der Erstellung der Sicht StoreManagerEdit verwenden:</span><span class="sxs-lookup"><span data-stu-id="467e8-154">This view will make use of two of the techniques we looked at while building the StoreManagerEdit view:</span></span>

- <span data-ttu-id="467e8-155">Wir verwenden Html.EditorForModel(), um Felder für das Modell Reihenfolge anzeigen</span><span class="sxs-lookup"><span data-stu-id="467e8-155">We will use Html.EditorForModel() to display form fields for the Order model</span></span>
- <span data-ttu-id="467e8-156">Wir werden Validierungsregeln Validierungsattribute Order-Klasse mit nutzen.</span><span class="sxs-lookup"><span data-stu-id="467e8-156">We will leverage validation rules using an Order class with validation attributes</span></span>

<span data-ttu-id="467e8-157">Wir beginnen mit den Formularcode, um Html.EditorForModel(), gefolgt von einer zusätzlichen Textfeld für die Promo-Code aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="467e8-157">We'll start by updating the form code to use Html.EditorForModel(), followed by an additional textbox for the Promo Code.</span></span> <span data-ttu-id="467e8-158">Der vollständige Code für die Sicht AddressAndPayment ist unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="467e8-158">The complete code for the AddressAndPayment view is shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a><span data-ttu-id="467e8-159">Definieren von Validierungsregeln für den Auftrag</span><span class="sxs-lookup"><span data-stu-id="467e8-159">Defining validation rules for the Order</span></span>

<span data-ttu-id="467e8-160">Nun, dass unsere Ansicht eingerichtet ist, wird die Validierungsregeln für unsere Reihenfolge Modell richten wir wir zuvor für das Modell Album ausgeführt haben.</span><span class="sxs-lookup"><span data-stu-id="467e8-160">Now that our view is set up, we will set up the validation rules for our Order model as we did previously for the Album model.</span></span> <span data-ttu-id="467e8-161">Mit der rechten Maustaste auf den Ordner Models, und fügen Sie eine Klasse mit dem Namen Reihenfolge.</span><span class="sxs-lookup"><span data-stu-id="467e8-161">Right-click on the Models folder and add a class named Order.</span></span> <span data-ttu-id="467e8-162">Neben den Attributen der Überprüfung, die wir zuvor für das Album verwendet, werden auch einen regulären Ausdruck verwendet. um die e-Mail-Adresse des Benutzers zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="467e8-162">In addition to the validation attributes we used previously for the Album, we will also be using a Regular Expression to validate the user's email address.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

<span data-ttu-id="467e8-163">Versuch, das Formular aus, mit fehlendem oder ungültige Informationen wird jetzt die clientseitige Validierung mit Fehlermeldung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="467e8-163">Attempting to submit the form with missing or invalid information will now show error message using client-side validation.</span></span>

![](mvc-music-store-part-9/_static/image7.png)

<span data-ttu-id="467e8-164">Zugegeben, haben wir die meisten die harte Arbeit für des Auscheckvorgangs; Wir haben nur wenige Odds und endet, um den Vorgang abzuschließen.</span><span class="sxs-lookup"><span data-stu-id="467e8-164">Okay, we've done most of the hard work for the checkout process; we just have a few odds and ends to finish.</span></span> <span data-ttu-id="467e8-165">Es müssen zwei Ansichten für das einfache hinzufügen, und wir die Übergabe der Einkaufswagen Informationen während des Anmeldevorgangs kümmern müssen.</span><span class="sxs-lookup"><span data-stu-id="467e8-165">We need to add two simple views, and we need to take care of the handoff of the cart information during the login process.</span></span>

## <a name="adding-the-checkout-complete-view"></a><span data-ttu-id="467e8-166">Hinzufügen der Ansicht für die vollständige Auschecken</span><span class="sxs-lookup"><span data-stu-id="467e8-166">Adding the Checkout Complete view</span></span>

<span data-ttu-id="467e8-167">Die Auschecken vollständige Ansicht ist ganz einfach, da es lediglich die Auftrags-ID anzeigen muss</span><span class="sxs-lookup"><span data-stu-id="467e8-167">The Checkout Complete view is pretty simple, as it just needs to display the Order ID.</span></span> <span data-ttu-id="467e8-168">Mit der rechten Maustaste auf die Aktion der controllerrolle abschließen, und fügen Sie eine Ansicht mit dem Namen Fertig stellen die stark typisiert als ein "int".</span><span class="sxs-lookup"><span data-stu-id="467e8-168">Right-click on the Complete controller action and add a view named Complete which is strongly typed as an int.</span></span>

![](mvc-music-store-part-9/_static/image8.png)

<span data-ttu-id="467e8-169">Jetzt werden wir den Code anzeigen, um die Bestell-ID anzeigen aktualisieren, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="467e8-169">Now we will update the view code to display the Order ID, as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a><span data-ttu-id="467e8-170">Aktualisieren der Fehleransicht</span><span class="sxs-lookup"><span data-stu-id="467e8-170">Updating The Error view</span></span>

<span data-ttu-id="467e8-171">Die Standardvorlage enthält ein Fehleransicht im Ordner Views freigegeben, sodass wiederverwendeten an anderer Stelle in der Website möglich.</span><span class="sxs-lookup"><span data-stu-id="467e8-171">The default template includes an Error view in the Shared views folder so that it can be re-used elsewhere in the site.</span></span> <span data-ttu-id="467e8-172">Dieser Fehler-Ansicht enthält einen sehr einfachen Fehler und unserer Website Layout, nicht verwendet werden, damit wir aktualisiert werden müssen.</span><span class="sxs-lookup"><span data-stu-id="467e8-172">This Error view contains a very simple error and doesn't use our site Layout, so we'll update it.</span></span>

<span data-ttu-id="467e8-173">Da dies eine generische Fehlerseite ist, ist der Inhalt sehr einfach.</span><span class="sxs-lookup"><span data-stu-id="467e8-173">Since this is a generic error page, the content is very simple.</span></span> <span data-ttu-id="467e8-174">Es wird eine Nachricht und einen Link, um zur vorherigen Seite im Verlauf zu navigieren, wenn der Benutzer möchte ihre Aktion erneut versuchen, das enthalten.</span><span class="sxs-lookup"><span data-stu-id="467e8-174">We'll include a message and a link to navigate to the previous page in history if the user wants to re-try their action.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


>[!div class="step-by-step"]
<span data-ttu-id="467e8-175">[Zurück](mvc-music-store-part-8.md)
[Weiter](mvc-music-store-part-10.md)</span><span class="sxs-lookup"><span data-stu-id="467e8-175">[Previous](mvc-music-store-part-8.md)
[Next](mvc-music-store-part-10.md)</span></span>
