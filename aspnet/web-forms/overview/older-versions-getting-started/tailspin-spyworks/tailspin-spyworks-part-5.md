---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: 'Teil 5: Geschäftslogik | Microsoft Docs'
author: JoeStagner
description: Diese Reihe von Lernprogrammen sind alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 5 Fügt einer Geschäftslogik.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: e4342e634ef8c4bcf4e0085650a28f414ab23736
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30885080"
---
<a name="part-5-business-logic"></a><span data-ttu-id="3fb90-104">Teil 5: Geschäftslogik</span><span class="sxs-lookup"><span data-stu-id="3fb90-104">Part 5: Business Logic</span></span>
====================
<span data-ttu-id="3fb90-105">durch [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="3fb90-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="3fb90-106">Tailspin Spyworks wird veranschaulicht, wie außergewöhnlich einfache ist die leistungsstarke, skalierbare Anwendungen für .NET-Plattform zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="3fb90-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="3fb90-107">Es wird gezeigt, aus wie die hervorragenden neuen Funktionen in ASP.NET 4 mit um einen Onlineshop, einschließlich Warenkorb, Auschecken und Verwaltung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="3fb90-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="3fb90-108">Diese Reihe von Lernprogrammen sind alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen.</span><span class="sxs-lookup"><span data-stu-id="3fb90-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="3fb90-109">Teil 5 Fügt einer Geschäftslogik.</span><span class="sxs-lookup"><span data-stu-id="3fb90-109">Part 5 adds some business logic.</span></span>


## <a id="_Toc260221671"></a>  <span data-ttu-id="3fb90-110">Hinzufügen einer Geschäftslogik</span><span class="sxs-lookup"><span data-stu-id="3fb90-110">Adding Some Business Logic</span></span>

<span data-ttu-id="3fb90-111">Wir möchten unsere Warenkorb Erfahrungen verfügbar werden, wenn jemand auf unserer Website besucht.</span><span class="sxs-lookup"><span data-stu-id="3fb90-111">We want our shopping experience to be available whenever someone visits our web site.</span></span> <span data-ttu-id="3fb90-112">Besucher werden in der Lage zu durchsuchen und dem Warenkorb Elemente hinzufügen, selbst wenn sie nicht registriert oder angemeldet sind.</span><span class="sxs-lookup"><span data-stu-id="3fb90-112">Visitors will be able to browse and add items to the shopping cart even if they are not registered or logged in.</span></span> <span data-ttu-id="3fb90-113">Beim Auschecken bereitstehen sie erhält die Option zum Authentifizieren und wenn sie nicht sind noch Elemente werden zum Erstellen eines Kontos können.</span><span class="sxs-lookup"><span data-stu-id="3fb90-113">When they are ready to check out they will be given the option to authenticate and if they are not yet members they will be able to create an account.</span></span>

<span data-ttu-id="3fb90-114">Dies bedeutet, dass wir die Logik zum Konvertieren von des Einkaufswagen Sinn macht aus einem anonymen Status in "registriert" Benutzerzustand implementieren müssen.</span><span class="sxs-lookup"><span data-stu-id="3fb90-114">This means that we will need to implement the logic to convert the shopping cart from an anonymous state to a "Registered User" state.</span></span>

<span data-ttu-id="3fb90-115">Wir erstellen ein Verzeichnis namens "Klassen" und dann mit der rechten Maustaste auf den Ordner und erstellen Sie eine neue "Class"-Datei mit dem Namen MyShoppingCart.cs</span><span class="sxs-lookup"><span data-stu-id="3fb90-115">Let's create a directory named "Classes" then Right-Click on the folder and create a new "Class" file named MyShoppingCart.cs</span></span>

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

<span data-ttu-id="3fb90-116">Wie bereits erwähnt, wir erweitern die Klasse, die Seite "MyShoppingCart.aspx" implementiert, und wir erledigen dies mit. NET leistungsstarke "Teilklasse" erstellen.</span><span class="sxs-lookup"><span data-stu-id="3fb90-116">As previously mentioned we will be extending the class that implements the MyShoppingCart.aspx page and we will do this using .NET's powerful "Partial Class" construct.</span></span>

<span data-ttu-id="3fb90-117">Der generierte Aufruf unsere MyShoppingCart.aspx.cf Datei sieht wie folgt.</span><span class="sxs-lookup"><span data-stu-id="3fb90-117">The generated call for our MyShoppingCart.aspx.cf file looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

<span data-ttu-id="3fb90-118">Beachten Sie die Verwendung des Schlüsselworts "partiell" aus.</span><span class="sxs-lookup"><span data-stu-id="3fb90-118">Note the use of the "partial" keyword.</span></span>

<span data-ttu-id="3fb90-119">Die Klassendatei, die wir gerade generiert wurde, sieht wie folgt.</span><span class="sxs-lookup"><span data-stu-id="3fb90-119">The class file that we just generated looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

<span data-ttu-id="3fb90-120">Wir werden unsere Implementierungen zusammenführen, indem Sie diese Datei als auch die partial-Schlüsselwort hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="3fb90-120">We will merge our implementations by adding the partial keyword to this file as well.</span></span>

<span data-ttu-id="3fb90-121">Unsere neue Klassendatei sieht nun wie folgt.</span><span class="sxs-lookup"><span data-stu-id="3fb90-121">Our new class file now looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

<span data-ttu-id="3fb90-122">Die erste Methode, die wir unsere Klasse hinzufügen, ist die Methode "AddItem".</span><span class="sxs-lookup"><span data-stu-id="3fb90-122">The first method that we will add to our class is the "AddItem" method.</span></span> <span data-ttu-id="3fb90-123">Dies ist die Methode, die letztlich auf die Links "Auf Grafiken hinzufügen" auf den Seiten Produktliste und Produktdetails klickt der Benutzer aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="3fb90-123">This is the method that will ultimately be called when the user clicks on the "Add to Art" links on the Product List and Product Details pages.</span></span>

<span data-ttu-id="3fb90-124">Fügen Sie die folgenden mit Anweisungen am oberen Rand der Seite.</span><span class="sxs-lookup"><span data-stu-id="3fb90-124">Append the following to the using statements at the top of the page.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

<span data-ttu-id="3fb90-125">Und fügen Sie diese Methode der MyShoppingCart-Klasse.</span><span class="sxs-lookup"><span data-stu-id="3fb90-125">And add this method to the MyShoppingCart class.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

<span data-ttu-id="3fb90-126">Wir werden LINQ to Entities verwenden, um festzustellen, ob das Element bereits im Einkaufswagen ist.</span><span class="sxs-lookup"><span data-stu-id="3fb90-126">We are using LINQ to Entities to see if the item is already in the cart.</span></span> <span data-ttu-id="3fb90-127">Wenn wir also die Bestellmenge des Elements aktualisieren, erstellen Sie andernfalls einen neuen Eintrag für das ausgewählte Element</span><span class="sxs-lookup"><span data-stu-id="3fb90-127">If so, we update the order quantity of the item, otherwise we create a new entry for the selected item</span></span>

<span data-ttu-id="3fb90-128">Um diese Methode aufrufen, implementieren wir eine AddToCart.aspx-Seite, die nicht nur Klasse diese Methode, aber dann angezeigt, die aktuelle eine = Einkaufswagen, nachdem das Element hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="3fb90-128">In order to call this method we will implement an AddToCart.aspx page that not only class this method but then displayed the current shopping a=cart after the item has been added.</span></span>

<span data-ttu-id="3fb90-129">Mit der rechten Maustaste auf den Namen der Projektmappe im Projektmappen-Explorer und das Hinzufügen und neue Seite mit dem Namen AddToCart.aspx, wie wir bereits durchgeführt haben.</span><span class="sxs-lookup"><span data-stu-id="3fb90-129">Right-Click on the solution name in the solution explorer and add and new page named AddToCart.aspx as we have done previously.</span></span>

<span data-ttu-id="3fb90-130">Während wir dieser Seite anzuzeigenden Zwischenergebnisse: Niedrig stock Probleme usw., in unserem Implementierung verwenden könnten, wird die Seite nicht tatsächlich gerendert werden, aber stattdessen rufen Sie die Logik "Hinzufügen" und geleitet.</span><span class="sxs-lookup"><span data-stu-id="3fb90-130">While we could use this page to display interim results like low stock issues, etc, in our implementation, the page will not actually render, but rather call the "Add" logic and redirect.</span></span>

<span data-ttu-id="3fb90-131">Hierzu fügen wir den folgenden Code zur Seite\_Load-Ereignis.</span><span class="sxs-lookup"><span data-stu-id="3fb90-131">To accomplish this we'll add the following code to the Page\_Load event.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

<span data-ttu-id="3fb90-132">Beachten Sie, dass wir das Produkt aus einer QueryString-Parameter und Aufrufen der AddItem-Methode der Klasse zum Einkaufswagen hinzufügen abrufen.</span><span class="sxs-lookup"><span data-stu-id="3fb90-132">Note that we are retrieving the product to add to the shopping cart from a QueryString parameter and calling the AddItem method of our class.</span></span>

<span data-ttu-id="3fb90-133">Sofern keine Fehler auftreten erhält die Kontrolle der SHoppingCart.aspx-Seite, wir als Nächstes vollständig implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="3fb90-133">Assuming no errors are encountered control is passed to the SHoppingCart.aspx page which we will fully implement next.</span></span> <span data-ttu-id="3fb90-134">Wenn ein Fehler wird eine Ausnahme ausgelöst werden soll.</span><span class="sxs-lookup"><span data-stu-id="3fb90-134">If there should be an error we throw an exception.</span></span>

<span data-ttu-id="3fb90-135">Derzeit haben wir noch nicht implementiert einen globalen Fehlerhandler damit würde diese Ausnahme nicht durch die Anwendung wechseln, aber wir dies in Kürze behoben werden.</span><span class="sxs-lookup"><span data-stu-id="3fb90-135">Currently we have not yet implemented a global error handler so this exception would go unhandled by our application but we will remedy this shortly.</span></span>

<span data-ttu-id="3fb90-136">Beachten Sie auch die Verwendung der Anweisung Debug.Fail() (verfügbar über `using System.Diagnostics;)`</span><span class="sxs-lookup"><span data-stu-id="3fb90-136">Note also the use of the statement Debug.Fail() (available via `using System.Diagnostics;)`</span></span>

<span data-ttu-id="3fb90-137">Ist die Anwendung innerhalb des Debuggers ausgeführt wird, diese Methode zeigt eine detaillierte Dialogfeld mit Informationen zu den Anwendungsstatus zusammen mit der Fehlermeldung, die wir an.</span><span class="sxs-lookup"><span data-stu-id="3fb90-137">Is the application is running inside the debugger, this method will display a detailed dialog with information about the applications state along with the error message that we specify.</span></span>

<span data-ttu-id="3fb90-138">Wenn die Ausführung in der Produktion Debug.Fail()-Anweisung ignoriert wird.</span><span class="sxs-lookup"><span data-stu-id="3fb90-138">When running in production the Debug.Fail() statement is ignored.</span></span>

<span data-ttu-id="3fb90-139">Beachten Sie im Code über einen Aufruf an eine Methode in unserer shopping Cart-Klassennamen "GetShoppingCartId".</span><span class="sxs-lookup"><span data-stu-id="3fb90-139">You will note in the code above a call to a method in our shopping cart class names "GetShoppingCartId".</span></span>

<span data-ttu-id="3fb90-140">Fügen Sie den Code, um die Methode wie folgt zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="3fb90-140">Add the code to implement the method as follows.</span></span>

<span data-ttu-id="3fb90-141">Beachten Sie, dass wir auch hinzugefügt haben, aktualisieren und Auschecken Schaltflächen und eine Bezeichnung, in dem Einkaufswagen "insgesamt" angezeigt werden können.</span><span class="sxs-lookup"><span data-stu-id="3fb90-141">Note that we've also added update and checkout buttons and a label where we can display the cart "total".</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

<span data-ttu-id="3fb90-142">Wir können unsere warenkorbsoftware nun Elemente hinzufügen, aber wir haben die Logik zum Einkaufswagen angezeigt, nachdem ein Produkt hinzugefügt wurde nicht implementiert.</span><span class="sxs-lookup"><span data-stu-id="3fb90-142">We can now add items to our shopping cart but we have not implemented the logic to display the cart after a product has been added.</span></span>

<span data-ttu-id="3fb90-143">Daher auf der Seite MyShoppingCart.aspx fügen EntityDataSource-Steuerelement und ein Steuerelement GridVire wie folgt wir.</span><span class="sxs-lookup"><span data-stu-id="3fb90-143">So, in the MyShoppingCart.aspx page we'll add an EntityDataSource control and a GridVire control as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

<span data-ttu-id="3fb90-144">Rufen Sie das Formular im Designer, damit Sie können einen auf die Schaltfläche mit den Einkaufskorb aktualisieren Doppelklick und generieren den Click-Ereignishandler, der in der Deklaration im Markup angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="3fb90-144">Call up the form in the designer so that you can double click on the Update Cart button and generate the click event handler that is specified in the declaration in the markup.</span></span>

<span data-ttu-id="3fb90-145">Implementieren wir die Details später jedoch auf diese Weise lassen Sie uns erstellt und die Anwendung ohne Fehler ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="3fb90-145">We'll implement the details later but doing this will let us build and run our application without errors.</span></span>

<span data-ttu-id="3fb90-146">Wenn Sie die Anwendung auszuführen und ein Element zum Einkaufswagen hinzufügen wird dies angezeigt.</span><span class="sxs-lookup"><span data-stu-id="3fb90-146">When you run the application and add an item to the shopping cart you will see this.</span></span>

![](tailspin-spyworks-part-5/_static/image2.jpg)

<span data-ttu-id="3fb90-147">Beachten Sie, dass wir durch die Implementierung von drei benutzerdefinierter Spalten aus der "Default" Rasteranzeige abwichen haben.</span><span class="sxs-lookup"><span data-stu-id="3fb90-147">Note that we have deviated from the "default" grid display by implementing three custom columns.</span></span>

<span data-ttu-id="3fb90-148">Die erste ist ein bearbeitet werden, die für die Menge "Gebunden" Feld:</span><span class="sxs-lookup"><span data-stu-id="3fb90-148">The first is an Editable, "Bound" field for the Quantity:</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

<span data-ttu-id="3fb90-149">Das nächste ist eine "berechnete" Spalte, in dem das gesamte Zeilenelement angezeigt (das Element Kosten und die Menge aus, um sortiert zu werden):</span><span class="sxs-lookup"><span data-stu-id="3fb90-149">The next is a "calculated" column that displays the line item total (the item cost times the quantity to be ordered):</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

<span data-ttu-id="3fb90-150">Schließlich haben wir eine benutzerdefinierte Spalte, die ein CheckBox-Steuerelement enthält, die der Benutzer verwendet, um anzugeben, dass das Element aus dem Einkaufswagen Diagramm entfernt werden soll.</span><span class="sxs-lookup"><span data-stu-id="3fb90-150">Lastly we have a custom column that contains a CheckBox control that the user will use to indicate that the item should be removed from the shopping chart.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

<span data-ttu-id="3fb90-151">Wie Sie sehen können, die Reihenfolge, die gesamte Zeile wir also leer ist Logik zum Berechnen der Order Total hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="3fb90-151">As you can see, the Order Total line is empty so let's add some logic to calculate the Order Total.</span></span>

<span data-ttu-id="3fb90-152">Eine Methode "GetTotal" implementieren wir zuerst unsere MyShoppingCart-Klasse.</span><span class="sxs-lookup"><span data-stu-id="3fb90-152">We'll first implement a "GetTotal" method to our MyShoppingCart Class.</span></span>

<span data-ttu-id="3fb90-153">Fügen Sie in der Datei MyShoppingCart.cs den folgenden Code ein.</span><span class="sxs-lookup"><span data-stu-id="3fb90-153">In the MyShoppingCart.cs file add the following code.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

<span data-ttu-id="3fb90-154">Klicken Sie dann auf der Seite\_Load-ereignishandlers wir werden unsere GetTotal-Methode aufrufen können.</span><span class="sxs-lookup"><span data-stu-id="3fb90-154">Then in the Page\_Load event handler we'll can call our GetTotal method.</span></span> <span data-ttu-id="3fb90-155">Zur gleichen Zeit fügen wir einen Test, um festzustellen, ob der Einkaufswagen leer ist, und die Anzeige entsprechend anpassen, wenn es sich handelt.</span><span class="sxs-lookup"><span data-stu-id="3fb90-155">At the same time we'll add a test to see if the shopping cart is empty and adjust the display accordingly if it is.</span></span>

<span data-ttu-id="3fb90-156">Jetzt ist der Einkaufswagen leer erhalten wir dies:</span><span class="sxs-lookup"><span data-stu-id="3fb90-156">Now if the shopping cart is empty we get this:</span></span>

![](tailspin-spyworks-part-5/_static/image4.jpg)

<span data-ttu-id="3fb90-157">Und falls nicht, siehe wir unsere insgesamt.</span><span class="sxs-lookup"><span data-stu-id="3fb90-157">And if not, we see our total.</span></span>

![](tailspin-spyworks-part-5/_static/image5.jpg)

<span data-ttu-id="3fb90-158">Allerdings ist diese Seite noch nicht abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="3fb90-158">However, this page is not yet complete.</span></span>

<span data-ttu-id="3fb90-159">Wir benötigen zusätzliche Logik, neu zu berechnen den Einkaufswagen Sinn macht, durch das Entfernen von Elementen, die zum Löschen ausgewählt und durch neue Menge Werte bestimmen, wie einige im Raster vom Benutzer geändert wurde.</span><span class="sxs-lookup"><span data-stu-id="3fb90-159">We will need additional logic to recalculate the shopping cart by removing items marked for removal and by determining new quantity values as some may have been changed in the grid by the user.</span></span>

<span data-ttu-id="3fb90-160">Ermöglicht es, fügen Sie eine "RemoveItem"-Methode, um unsere einkaufswagenklasse in MyShoppingCart.cs, um den Fall abzudecken, wenn ein Benutzer ein Element zum Entfernen markiert.</span><span class="sxs-lookup"><span data-stu-id="3fb90-160">Lets add a "RemoveItem" method to our shopping cart class in MyShoppingCart.cs to handle the case when a user marks an item for removal.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

<span data-ttu-id="3fb90-161">Jetzt sehen wir Ad eine aufzurufende Methode die Situation behandelt wird, wenn ein Benutzer einfach ändert sich die Qualität der GridView Reihenfolge aufweisen.</span><span class="sxs-lookup"><span data-stu-id="3fb90-161">Now let's ad a method to handle the circumstance when a user simply changes the quality to be ordered in the GridView.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

<span data-ttu-id="3fb90-162">Mit den grundlegenden entfernen und Update-Features an Stelle können wir die Logik implementieren, die tatsächlich den Einkaufswagen Sinn macht, in der Datenbank aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="3fb90-162">With the basic Remove and Update features in place we can implement the logic that actually updates the shopping cart in the database.</span></span> <span data-ttu-id="3fb90-163">(In MyShoppingCart.cs)</span><span class="sxs-lookup"><span data-stu-id="3fb90-163">(In MyShoppingCart.cs)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

<span data-ttu-id="3fb90-164">Sie werden feststellen, dass diese Methode zwei Parameter erwartet.</span><span class="sxs-lookup"><span data-stu-id="3fb90-164">You'll note that this method expects two parameters.</span></span> <span data-ttu-id="3fb90-165">Eine dem Warenkorb-Id und das andere ist ein Array von Objekten des benutzerdefinierten Typs.</span><span class="sxs-lookup"><span data-stu-id="3fb90-165">One is the shopping cart Id and the other is an array of objects of user defined type.</span></span>

<span data-ttu-id="3fb90-166">Um die Abhängigkeit der unsere Logik Benutzer Schnittstelle Einzelheiten zu minimieren, haben wir eine Datenstruktur definiert, mit denen wir stattgefunden hat die shopping Cart-Elemente zum getesteten Codes unsere Methode des GridView-Steuerelements direkt zugreifen müssen.</span><span class="sxs-lookup"><span data-stu-id="3fb90-166">So as to minimize the dependency of our logic on user interface specifics, we've defined a data structure that we can use to pass the shopping cart items to our code without our method needing to directly access the GridView control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

<span data-ttu-id="3fb90-167">In unsere Datei MyShoppingCart.aspx.cs können wir diese Struktur in unserem Update Schaltfläche klicken-Ereignishandler wie folgt verwenden.</span><span class="sxs-lookup"><span data-stu-id="3fb90-167">In our MyShoppingCart.aspx.cs file we can use this structure in our Update Button Click Event handler as follows.</span></span> <span data-ttu-id="3fb90-168">Beachten Sie, zusätzlich zum Aktualisieren der Einkaufswagen den Einkaufswagen insgesamt sowie aktualisiert wird.</span><span class="sxs-lookup"><span data-stu-id="3fb90-168">Note that in addition to updating the cart we will update the cart total as well.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

<span data-ttu-id="3fb90-169">Beachten Sie bei Interesse diese Codezeile aus:</span><span class="sxs-lookup"><span data-stu-id="3fb90-169">Note with particular interest this line of code:</span></span>

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

<span data-ttu-id="3fb90-170">GetValues() ist eine spezielle Hilfsfunktion, die wie folgt in MyShoppingCart.aspx.cs implementiert wird.</span><span class="sxs-lookup"><span data-stu-id="3fb90-170">GetValues() is a special helper function that we will implement in MyShoppingCart.aspx.cs as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

<span data-ttu-id="3fb90-171">Dies stellt eine saubere Möglichkeit, den Zugriff auf die Werte der gebundenen Elemente in unserer GridView-Steuerelements bereit.</span><span class="sxs-lookup"><span data-stu-id="3fb90-171">This provides a clean way to access the values of the bound elements in our GridView control.</span></span> <span data-ttu-id="3fb90-172">Da unsere "Element entfernen" CheckBox-Steuerelement nicht gebunden ist müssen wir darauf zugreifen, über die FindControl()-Methode.</span><span class="sxs-lookup"><span data-stu-id="3fb90-172">Since our "Remove Item" CheckBox Control is not bound we'll access it via the FindControl() method.</span></span>

<span data-ttu-id="3fb90-173">Zu diesem Zeitpunkt in der Entwicklung des Projekts erhalten wir zum Implementieren des Auscheckvorgangs bereit.</span><span class="sxs-lookup"><span data-stu-id="3fb90-173">At this stage in your project's development we are getting ready to implement the checkout process.</span></span>

<span data-ttu-id="3fb90-174">Bevor wir dabei verwenden Sie Visual Studio zum Generieren der Mitgliedschaftsdatenbank und Hinzufügen eines Benutzers im Repository der Mitgliedschaft.</span><span class="sxs-lookup"><span data-stu-id="3fb90-174">Before doing so let's use Visual Studio to generate the membership database and add a user to the membership repository.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3fb90-175">[Zurück](tailspin-spyworks-part-4.md)
> [Weiter](tailspin-spyworks-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="3fb90-175">[Previous](tailspin-spyworks-part-4.md)
[Next](tailspin-spyworks-part-6.md)</span></span>
