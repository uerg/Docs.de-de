---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: 'Teil 5: Geschäftslogik | Microsoft-Dokumentation'
author: JoeStagner
description: Dieser tutorialreihe werden alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 5 fügt etwas Geschäftslogik.
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: fdb73c01d7b6076bc292c640376aa35cc5f52ea6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827047"
---
<a name="part-5-business-logic"></a><span data-ttu-id="127a6-104">Teil 5: Geschäftslogik</span><span class="sxs-lookup"><span data-stu-id="127a6-104">Part 5: Business Logic</span></span>
====================
<span data-ttu-id="127a6-105">durch [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="127a6-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="127a6-106">Tailspin Spyworks wird veranschaulicht, wie außerordentlich einfach es ist, erstellen Sie leistungsstarke, skalierbare Anwendungen für die .NET-Plattform.</span><span class="sxs-lookup"><span data-stu-id="127a6-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="127a6-107">Es wird gezeigt, aus wie die hervorragenden neuen Funktionen in ASP.NET 4 zu verwenden, um eine online-Store, einschließlich der Warenkorb, Auschecken und Verwaltung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="127a6-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="127a6-108">Dieser tutorialreihe werden alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen.</span><span class="sxs-lookup"><span data-stu-id="127a6-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="127a6-109">Teil 5 fügt etwas Geschäftslogik.</span><span class="sxs-lookup"><span data-stu-id="127a6-109">Part 5 adds some business logic.</span></span>


## <a id="_Toc260221671"></a>  <span data-ttu-id="127a6-110">Hinzufügen von Geschäftslogik</span><span class="sxs-lookup"><span data-stu-id="127a6-110">Adding Some Business Logic</span></span>

<span data-ttu-id="127a6-111">Wir möchten unsere Einkaufserlebnis verfügbar sein, wenn jemand auf unserer Website besucht.</span><span class="sxs-lookup"><span data-stu-id="127a6-111">We want our shopping experience to be available whenever someone visits our web site.</span></span> <span data-ttu-id="127a6-112">Besucher werden suchen und Hinzufügen von Elementen zum Einkaufswagen, auch wenn sie nicht registriert oder angemeldet sind.</span><span class="sxs-lookup"><span data-stu-id="127a6-112">Visitors will be able to browse and add items to the shopping cart even if they are not registered or logged in.</span></span> <span data-ttu-id="127a6-113">Wenn sie bereit sind, sehen Sie sich diese erhalten der Möglichkeit, zu authentifizieren, und wenn dies nicht der Fall noch Elemente werden zum Erstellen eines Kontos können.</span><span class="sxs-lookup"><span data-stu-id="127a6-113">When they are ready to check out they will be given the option to authenticate and if they are not yet members they will be able to create an account.</span></span>

<span data-ttu-id="127a6-114">Dies bedeutet, dass die Logik zum Konvertieren des Einkaufswagens ein Status "Anonym" in einen "registriert" Benutzerzustand implementieren müssen.</span><span class="sxs-lookup"><span data-stu-id="127a6-114">This means that we will need to implement the logic to convert the shopping cart from an anonymous state to a "Registered User" state.</span></span>

<span data-ttu-id="127a6-115">Wir erstellen ein Verzeichnis namens "Klassen" und dann mit der rechten Maustaste auf den Ordner und erstellen Sie eine neue "Class"-Datei mit dem Namen MyShoppingCart.cs</span><span class="sxs-lookup"><span data-stu-id="127a6-115">Let's create a directory named "Classes" then Right-Click on the folder and create a new "Class" file named MyShoppingCart.cs</span></span>

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

<span data-ttu-id="127a6-116">Wie bereits erwähnt, wir erweitern die Klasse, die Seite "MyShoppingCart.aspx" implementiert, und wir erledigen dies mit. NET leistungsstarke "Partial Class" erstellen.</span><span class="sxs-lookup"><span data-stu-id="127a6-116">As previously mentioned we will be extending the class that implements the MyShoppingCart.aspx page and we will do this using .NET's powerful "Partial Class" construct.</span></span>

<span data-ttu-id="127a6-117">Der generierte Aufruf für unsere MyShoppingCart.aspx.cf-Datei sieht wie folgt aus.</span><span class="sxs-lookup"><span data-stu-id="127a6-117">The generated call for our MyShoppingCart.aspx.cf file looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

<span data-ttu-id="127a6-118">Beachten Sie die Verwendung des Schlüsselworts "partiell".</span><span class="sxs-lookup"><span data-stu-id="127a6-118">Note the use of the "partial" keyword.</span></span>

<span data-ttu-id="127a6-119">Die Klassendatei, die wir gerade erstellt haben, sieht wie folgt aus.</span><span class="sxs-lookup"><span data-stu-id="127a6-119">The class file that we just generated looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

<span data-ttu-id="127a6-120">Wir werden unsere Implementierungen zusammen, indem diese Datei auch das partial-Schlüsselwort hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="127a6-120">We will merge our implementations by adding the partial keyword to this file as well.</span></span>

<span data-ttu-id="127a6-121">Unsere neue Klassendatei sieht nun folgendermaßen aus.</span><span class="sxs-lookup"><span data-stu-id="127a6-121">Our new class file now looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

<span data-ttu-id="127a6-122">Die erste Methode, mit der wir unsere Klasse hinzugefügt wird, ist die Methode "AddItem".</span><span class="sxs-lookup"><span data-stu-id="127a6-122">The first method that we will add to our class is the "AddItem" method.</span></span> <span data-ttu-id="127a6-123">Dies ist die Methode, die letztlich auf die Links "Hinzufügen, Kunst" auf den Seiten Product List "und" Produktdetails klickt der Benutzer aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="127a6-123">This is the method that will ultimately be called when the user clicks on the "Add to Art" links on the Product List and Product Details pages.</span></span>

<span data-ttu-id="127a6-124">Fügen Sie Folgendes an der mit Anweisungen am oberen Rand der Seite.</span><span class="sxs-lookup"><span data-stu-id="127a6-124">Append the following to the using statements at the top of the page.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

<span data-ttu-id="127a6-125">Ein, und fügen Sie die folgende Methode der MyShoppingCart-Klasse.</span><span class="sxs-lookup"><span data-stu-id="127a6-125">And add this method to the MyShoppingCart class.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

<span data-ttu-id="127a6-126">Wir werden LINQ to Entities verwenden, um festzustellen, ob das Element bereits im Einkaufswagen ist.</span><span class="sxs-lookup"><span data-stu-id="127a6-126">We are using LINQ to Entities to see if the item is already in the cart.</span></span> <span data-ttu-id="127a6-127">Wenn also, wir die Bestellmenge des Elements aktualisieren, erstellen Sie andernfalls einen neuen Eintrag für das ausgewählte Element</span><span class="sxs-lookup"><span data-stu-id="127a6-127">If so, we update the order quantity of the item, otherwise we create a new entry for the selected item</span></span>

<span data-ttu-id="127a6-128">Um diese Methode aufrufen, werden wir implementieren eine AddToCart.aspx-Seite, die nicht nur Klasse diese Methode, aber dann angezeigt, die aktuelle eine = Warenkorb, nachdem das Element hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="127a6-128">In order to call this method we will implement an AddToCart.aspx page that not only class this method but then displayed the current shopping a=cart after the item has been added.</span></span>

<span data-ttu-id="127a6-129">Mit der rechten Maustaste auf den Namen der Projektmappe im Projektmappen-Explorer und das Hinzufügen und neue Seite, die mit dem Namen AddToCart.aspx, wie wir bereits durchgeführt haben.</span><span class="sxs-lookup"><span data-stu-id="127a6-129">Right-Click on the solution name in the solution explorer and add and new page named AddToCart.aspx as we have done previously.</span></span>

<span data-ttu-id="127a6-130">Während wir auf dieser Seite anzuzeigenden Zwischenergebnisse wie Niedrig vordefinierten Probleme usw., in unserer Implementierung verwenden könnten, die Seite wird nicht tatsächlich gerendert werden, aber stattdessen rufen Sie die Logik der "Add" und umleiten.</span><span class="sxs-lookup"><span data-stu-id="127a6-130">While we could use this page to display interim results like low stock issues, etc, in our implementation, the page will not actually render, but rather call the "Add" logic and redirect.</span></span>

<span data-ttu-id="127a6-131">Zu diesem Zweck fügen wir den folgenden Code auf der Seite\_Load-Ereignis.</span><span class="sxs-lookup"><span data-stu-id="127a6-131">To accomplish this we'll add the following code to the Page\_Load event.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

<span data-ttu-id="127a6-132">Beachten Sie, dass wir das Produkt zum Einkaufswagen hinzufügen, aus dem QueryString-Parameter und Aufrufen der Methode "AddItem" unserer Klasse abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="127a6-132">Note that we are retrieving the product to add to the shopping cart from a QueryString parameter and calling the AddItem method of our class.</span></span>

<span data-ttu-id="127a6-133">Sofern keine Fehler auftreten erhält die Kontrolle der SHoppingCart.aspx-Seite, wir als Nächstes vollständig implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="127a6-133">Assuming no errors are encountered control is passed to the SHoppingCart.aspx page which we will fully implement next.</span></span> <span data-ttu-id="127a6-134">Wenn ein Fehler vorhanden sein soll, wird eine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="127a6-134">If there should be an error we throw an exception.</span></span>

<span data-ttu-id="127a6-135">Derzeit haben wir noch nicht implementiert einen globalen Fehlerhandler, diese Ausnahme wird von der Anwendung unbehandelt, aber wir werden dies in Kürze beheben.</span><span class="sxs-lookup"><span data-stu-id="127a6-135">Currently we have not yet implemented a global error handler so this exception would go unhandled by our application but we will remedy this shortly.</span></span>

<span data-ttu-id="127a6-136">Beachten Sie auch die Verwendung der Anweisung Debug.Fail() (verfügbar über `using System.Diagnostics;)`</span><span class="sxs-lookup"><span data-stu-id="127a6-136">Note also the use of the statement Debug.Fail() (available via `using System.Diagnostics;)`</span></span>

<span data-ttu-id="127a6-137">Ist die Anwendung innerhalb des Debuggers ausgeführt wird, diese Methode wird in diesem Dialogfeld ausführliche Informationen über die Anwendungen Zustand zusammen mit der Fehlermeldung, die wir angeben.</span><span class="sxs-lookup"><span data-stu-id="127a6-137">Is the application is running inside the debugger, this method will display a detailed dialog with information about the applications state along with the error message that we specify.</span></span>

<span data-ttu-id="127a6-138">Bei Ausführung in der Produktion die Debug.Fail()-Anweisung ignoriert wird.</span><span class="sxs-lookup"><span data-stu-id="127a6-138">When running in production the Debug.Fail() statement is ignored.</span></span>

<span data-ttu-id="127a6-139">Sie werden im Code über einen Aufruf einer Methode in unserem shopping Cart-Klassennamen "GetShoppingCartId" feststellen.</span><span class="sxs-lookup"><span data-stu-id="127a6-139">You will note in the code above a call to a method in our shopping cart class names "GetShoppingCartId".</span></span>

<span data-ttu-id="127a6-140">Fügen Sie den Code aus, um die Methode wie folgt zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="127a6-140">Add the code to implement the method as follows.</span></span>

<span data-ttu-id="127a6-141">Beachten Sie, dass wir auch hinzugefügt haben, aktualisieren und Auschecken Schaltflächen und eine Bezeichnung, in dem der Warenkorb "Gesamt" angezeigt werden können.</span><span class="sxs-lookup"><span data-stu-id="127a6-141">Note that we've also added update and checkout buttons and a label where we can display the cart "total".</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

<span data-ttu-id="127a6-142">Können wir unsere warenkorbsoftware jetzt Elemente hinzufügen, aber wir haben die Logik zum Warenkorb anzeigen, nachdem ein Produkt hinzugefügt wurde nicht implementiert.</span><span class="sxs-lookup"><span data-stu-id="127a6-142">We can now add items to our shopping cart but we have not implemented the logic to display the cart after a product has been added.</span></span>

<span data-ttu-id="127a6-143">Daher auf der Seite MyShoppingCart.aspx fügen ein EntityDataSource-Steuerelement und ein Steuerelement GridVire wie folgt wir.</span><span class="sxs-lookup"><span data-stu-id="127a6-143">So, in the MyShoppingCart.aspx page we'll add an EntityDataSource control and a GridVire control as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

<span data-ttu-id="127a6-144">Rufen Sie das Formular im Designer, damit Sie einen auf die Schaltfläche mit den Einkaufskorb aktualisieren Doppelklick können, und generieren den Click-Ereignishandler, der in der Deklaration im Markup angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="127a6-144">Call up the form in the designer so that you can double click on the Update Cart button and generate the click event handler that is specified in the declaration in the markup.</span></span>

<span data-ttu-id="127a6-145">Implementieren wir die Details später aber lassen Sie uns erstellen und Ausführen unserer Anwendung ohne Fehler wird dadurch.</span><span class="sxs-lookup"><span data-stu-id="127a6-145">We'll implement the details later but doing this will let us build and run our application without errors.</span></span>

<span data-ttu-id="127a6-146">Wenn Sie die Anwendung auszuführen und ein Element zum Einkaufswagen hinzufügen wird dies angezeigt.</span><span class="sxs-lookup"><span data-stu-id="127a6-146">When you run the application and add an item to the shopping cart you will see this.</span></span>

![](tailspin-spyworks-part-5/_static/image2.jpg)

<span data-ttu-id="127a6-147">Beachten Sie, dass wir aus der "Default" Rasteranzeige Berichtdienste, durch die Implementierung von drei benutzerdefinierter Spalten.</span><span class="sxs-lookup"><span data-stu-id="127a6-147">Note that we have deviated from the "default" grid display by implementing three custom columns.</span></span>

<span data-ttu-id="127a6-148">Die erste ist ein Bearbeitbarer, "Gebunden" Feld für die Menge an:</span><span class="sxs-lookup"><span data-stu-id="127a6-148">The first is an Editable, "Bound" field for the Quantity:</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

<span data-ttu-id="127a6-149">Das nächste ist eine "berechnete"-Spalte, in dem das gesamte Zeilenelement angezeigt (das Element Kosten und die Menge aus, um sortiert zu werden):</span><span class="sxs-lookup"><span data-stu-id="127a6-149">The next is a "calculated" column that displays the line item total (the item cost times the quantity to be ordered):</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

<span data-ttu-id="127a6-150">Schließlich haben wir eine benutzerdefinierte Spalte, die ein CheckBox-Steuerelement enthält, die der Benutzer verwendet, um anzugeben, dass das Element aus dem Einkaufswagen Diagramm entfernt werden soll.</span><span class="sxs-lookup"><span data-stu-id="127a6-150">Lastly we have a custom column that contains a CheckBox control that the user will use to indicate that the item should be removed from the shopping chart.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

<span data-ttu-id="127a6-151">Wie Sie sehen können, die Reihenfolge, die gesamte Zeile nun leer ist, berechnen Sie den Order Total Logik hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="127a6-151">As you can see, the Order Total line is empty so let's add some logic to calculate the Order Total.</span></span>

<span data-ttu-id="127a6-152">Eine Methode "GetTotal" implementieren wir zuerst unsere MyShoppingCart-Klasse.</span><span class="sxs-lookup"><span data-stu-id="127a6-152">We'll first implement a "GetTotal" method to our MyShoppingCart Class.</span></span>

<span data-ttu-id="127a6-153">Fügen Sie den folgenden Code hinzu, in der Datei MyShoppingCart.cs.</span><span class="sxs-lookup"><span data-stu-id="127a6-153">In the MyShoppingCart.cs file add the following code.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

<span data-ttu-id="127a6-154">Klicken Sie dann auf der Seite\_Ereignishandler zum Laden wir werden unsere GetTotal-Methode aufrufen können.</span><span class="sxs-lookup"><span data-stu-id="127a6-154">Then in the Page\_Load event handler we'll can call our GetTotal method.</span></span> <span data-ttu-id="127a6-155">Zur gleichen Zeit fügen wir einen Test, um festzustellen, ob der Einkaufswagen leer ist, und passen Sie die Anzeige entsprechend, wenn es sich handelt.</span><span class="sxs-lookup"><span data-stu-id="127a6-155">At the same time we'll add a test to see if the shopping cart is empty and adjust the display accordingly if it is.</span></span>

<span data-ttu-id="127a6-156">Jetzt ist der Einkaufswagen leer erhalten wir dies:</span><span class="sxs-lookup"><span data-stu-id="127a6-156">Now if the shopping cart is empty we get this:</span></span>

![](tailspin-spyworks-part-5/_static/image4.jpg)

<span data-ttu-id="127a6-157">Und falls nicht, sehen unsere insgesamt.</span><span class="sxs-lookup"><span data-stu-id="127a6-157">And if not, we see our total.</span></span>

![](tailspin-spyworks-part-5/_static/image5.jpg)

<span data-ttu-id="127a6-158">Allerdings ist diese Seite noch nicht abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="127a6-158">However, this page is not yet complete.</span></span>

<span data-ttu-id="127a6-159">Wir benötigen zusätzlichen Logik zum Einkaufswagen ordnungsgemäß neu zu berechnen, durch Entfernen von Elementen, die zum Löschen ausgewählt und durch neue Menge Werte bestimmen, wie einige im Raster vom Benutzer geändert wurde.</span><span class="sxs-lookup"><span data-stu-id="127a6-159">We will need additional logic to recalculate the shopping cart by removing items marked for removal and by determining new quantity values as some may have been changed in the grid by the user.</span></span>

<span data-ttu-id="127a6-160">Können unsere einkaufswagenklasse in MyShoppingCart.cs, um den Fall abzudecken, wenn ein Benutzer ein Element zum Entfernen markiert eine Methode "RemoveItem" hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="127a6-160">Lets add a "RemoveItem" method to our shopping cart class in MyShoppingCart.cs to handle the case when a user marks an item for removal.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

<span data-ttu-id="127a6-161">Jetzt sehen wir Ad einer Methode, die Situation zu behandeln, wenn ein Benutzer einfach die Qualität, die in den GridView-Ansicht sortiert werden ändert.</span><span class="sxs-lookup"><span data-stu-id="127a6-161">Now let's ad a method to handle the circumstance when a user simply changes the quality to be ordered in the GridView.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

<span data-ttu-id="127a6-162">Mit den grundlegenden "Remove" und Update-Features an Stelle können wir die Logik implementieren, die tatsächlich den Einkaufswagen in der Datenbank aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="127a6-162">With the basic Remove and Update features in place we can implement the logic that actually updates the shopping cart in the database.</span></span> <span data-ttu-id="127a6-163">(In MyShoppingCart.cs)</span><span class="sxs-lookup"><span data-stu-id="127a6-163">(In MyShoppingCart.cs)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

<span data-ttu-id="127a6-164">Sie werden feststellen, dass diese Methode zwei Parameter erwartet.</span><span class="sxs-lookup"><span data-stu-id="127a6-164">You'll note that this method expects two parameters.</span></span> <span data-ttu-id="127a6-165">Eine dem Warenkorb-Id und das andere ist ein Array von Objekten des benutzerdefinierten Typs.</span><span class="sxs-lookup"><span data-stu-id="127a6-165">One is the shopping cart Id and the other is an array of objects of user defined type.</span></span>

<span data-ttu-id="127a6-166">Um die Abhängigkeit von unserer Logik Einzelheiten der Schnittstelle von Benutzer zu minimieren, haben wir eine Datenstruktur definiert, die wir verwenden können, die Artikel im Warenkorb zu unserem Code übergeben, ohne die Methode, die direkt auf das GridView-Steuerelement zugreifen müssen.</span><span class="sxs-lookup"><span data-stu-id="127a6-166">So as to minimize the dependency of our logic on user interface specifics, we've defined a data structure that we can use to pass the shopping cart items to our code without our method needing to directly access the GridView control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

<span data-ttu-id="127a6-167">In unserer MyShoppingCart.aspx.cs-Datei können wir diese Struktur in unserem Update Schaltfläche klicken Sie auf-Ereignishandler wie folgt verwenden.</span><span class="sxs-lookup"><span data-stu-id="127a6-167">In our MyShoppingCart.aspx.cs file we can use this structure in our Update Button Click Event handler as follows.</span></span> <span data-ttu-id="127a6-168">Beachten Sie, dass zusätzlich zum Aktualisieren des Warenkorbs die Warenkorb-Summe ebenfalls aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="127a6-168">Note that in addition to updating the cart we will update the cart total as well.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

<span data-ttu-id="127a6-169">Beachten Sie diese Codezeile, mit besonderem Interesse:</span><span class="sxs-lookup"><span data-stu-id="127a6-169">Note with particular interest this line of code:</span></span>

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

<span data-ttu-id="127a6-170">GetValues() ist eine spezielle Hilfsfunktion, die wir in MyShoppingCart.aspx.cs wie folgt implementiert wird.</span><span class="sxs-lookup"><span data-stu-id="127a6-170">GetValues() is a special helper function that we will implement in MyShoppingCart.aspx.cs as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

<span data-ttu-id="127a6-171">Dadurch wird eine saubere Möglichkeit, den Zugriff auf die Werte der gebundenen Elemente in das GridView-Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="127a6-171">This provides a clean way to access the values of the bound elements in our GridView control.</span></span> <span data-ttu-id="127a6-172">Da unsere "Element entfernen" CheckBox-Steuerelement nicht gebunden ist, werden wir darauf über die FindControl()-Methode zugreifen.</span><span class="sxs-lookup"><span data-stu-id="127a6-172">Since our "Remove Item" CheckBox Control is not bound we'll access it via the FindControl() method.</span></span>

<span data-ttu-id="127a6-173">In dieser Phase des Projekts Entwicklung werden wir zum Implementieren des Auscheckvorgangs bereiten.</span><span class="sxs-lookup"><span data-stu-id="127a6-173">At this stage in your project's development we are getting ready to implement the checkout process.</span></span>

<span data-ttu-id="127a6-174">Bevor wir dabei verwenden Sie Visual Studio zum Generieren der Mitgliedschaftsdatenbank und Hinzufügen eines Benutzers in das Repository für die Mitgliedschaft.</span><span class="sxs-lookup"><span data-stu-id="127a6-174">Before doing so let's use Visual Studio to generate the membership database and add a user to the membership repository.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="127a6-175">[Zurück](tailspin-spyworks-part-4.md)
> [Weiter](tailspin-spyworks-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="127a6-175">[Previous](tailspin-spyworks-part-4.md)
[Next](tailspin-spyworks-part-6.md)</span></span>
