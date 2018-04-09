---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: 'Teil 7: Hinzufügen von Funktionen | Microsoft Docs'
author: JoeStagner
description: Diese Reihe von Lernprogrammen sind alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 7 fügt zusätzliche Funktionen, z. B. übe Konto...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: 17f068155f6726047901e2f7d580d375a4e07c87
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="part-7-adding-features"></a><span data-ttu-id="e69c3-104">Teil 7: Hinzufügen von Funktionen</span><span class="sxs-lookup"><span data-stu-id="e69c3-104">Part 7: Adding Features</span></span>
====================
<span data-ttu-id="e69c3-105">durch [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="e69c3-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="e69c3-106">Tailspin Spyworks wird veranschaulicht, wie außergewöhnlich einfache ist die leistungsstarke, skalierbare Anwendungen für .NET-Plattform zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="e69c3-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="e69c3-107">Es wird gezeigt, aus wie die hervorragenden neuen Funktionen in ASP.NET 4 mit um einen Onlineshop, einschließlich Warenkorb, Auschecken und Verwaltung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="e69c3-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="e69c3-108">Diese Reihe von Lernprogrammen sind alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen.</span><span class="sxs-lookup"><span data-stu-id="e69c3-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="e69c3-109">Teil 7 fügt zusätzliche Funktionen wie Konto überprüfen, produktprüfungen, und "gängige Elemente" und "auch erworbenen" Benutzersteuerelemente hinzu.</span><span class="sxs-lookup"><span data-stu-id="e69c3-109">Part 7 adds additional features, such as account review, product reviews, and "popular items" and "also purchased" user controls.</span></span>


## <a id="_Toc260221673"></a>  <span data-ttu-id="e69c3-110">Hinzufügen von Funktionen</span><span class="sxs-lookup"><span data-stu-id="e69c3-110">Adding Features</span></span>

<span data-ttu-id="e69c3-111">Wenn Benutzer unsere Katalog durchsuchen können, platzieren Sie Elemente im Einkaufswagen, und abgeschlossen Sie des Auscheckvorgangs, stehen Sie eine Reihe unterstützen Funktionen, wir aufnimmt, um unsere Website zu verbessern.</span><span class="sxs-lookup"><span data-stu-id="e69c3-111">Though users can browse our catalog, place items in their shopping cart, and complete the checkout process, there are a number of supporting features that we will include to improve our site.</span></span>

1. <span data-ttu-id="e69c3-112">Überprüfen Sie das Konto (Liste Bestellungen platziert und zeigen Sie Details an.)</span><span class="sxs-lookup"><span data-stu-id="e69c3-112">Account Review (List orders placed and view details.)</span></span>
2. <span data-ttu-id="e69c3-113">Einige bestimmten Kontext-Inhalt auf der Startseite hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="e69c3-113">Add some context specific content to the front page.</span></span>
3. <span data-ttu-id="e69c3-114">Hinzufügen einer Funktion können Benutzer überprüfen Sie die Produkte im Katalog.</span><span class="sxs-lookup"><span data-stu-id="e69c3-114">Add a feature to let users Review the products in the catalog.</span></span>
4. <span data-ttu-id="e69c3-115">Erstellen Sie ein benutzerdefiniertes Steuerelement zum Anzeigen von beliebten Elemente und fügen Sie dieses, die steuern, auf der Startseite.</span><span class="sxs-lookup"><span data-stu-id="e69c3-115">Create a User Control to display Popular Items and Place that control on the front page.</span></span>
5. <span data-ttu-id="e69c3-116">Erstellen eines Benutzersteuerelements "Auch" gekauft ", und fügen Sie es auf der Detailseite hinzu.</span><span class="sxs-lookup"><span data-stu-id="e69c3-116">Create an "Also Purchased" user control and add it to the product details page.</span></span>
6. <span data-ttu-id="e69c3-117">Hinzufügen eines Kontakts Seite.</span><span class="sxs-lookup"><span data-stu-id="e69c3-117">Add a Contact Page.</span></span>
7. <span data-ttu-id="e69c3-118">Hinzufügen einer zu Seite.</span><span class="sxs-lookup"><span data-stu-id="e69c3-118">Add an About Page.</span></span>
8. <span data-ttu-id="e69c3-119">Globaler Fehler</span><span class="sxs-lookup"><span data-stu-id="e69c3-119">Global Error</span></span>

## <a id="_Toc260221674"></a>  <span data-ttu-id="e69c3-120">Account-Überprüfung</span><span class="sxs-lookup"><span data-stu-id="e69c3-120">Account Review</span></span>

<span data-ttu-id="e69c3-121">Erstellen Sie zwei ASPX-Seiten, die einen benannten OrderList.aspx und die anderen benannten OrderDetails.aspx im Ordner "Konto"</span><span class="sxs-lookup"><span data-stu-id="e69c3-121">In the "Account" folder create two .aspx pages one named OrderList.aspx and the other named OrderDetails.aspx</span></span>

<span data-ttu-id="e69c3-122">OrderList.aspx wird GridView und EntityDataSoure nutzen, fast so wie wir zuvor haben.</span><span class="sxs-lookup"><span data-stu-id="e69c3-122">OrderList.aspx will leverage the GridView and EntityDataSoure controls much as we have previously.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

<span data-ttu-id="e69c3-123">Die EntityDataSoure wählt die Datensätze aus der Orders-Tabelle gefiltert wird, auf den Benutzernamen (siehe die WhereParameter) die wir in einer Sitzungsvariablen festgelegt, wenn die Benutzeranmeldung's.</span><span class="sxs-lookup"><span data-stu-id="e69c3-123">The EntityDataSoure selects records from the Orders table filtered on the UserName (see the WhereParameter) which we set in a session variable when the user log's in.</span></span>

<span data-ttu-id="e69c3-124">Beachten Sie auch diese Parameter in der HyperlinkField der GridView ein:</span><span class="sxs-lookup"><span data-stu-id="e69c3-124">Note also these parameters in the HyperlinkField of the GridView:</span></span>

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

<span data-ttu-id="e69c3-125">Dazu geben Sie den Link, um die Reihenfolge Detailansicht für jedes Produkt, das Feld "OrderID" als eine QueryString-Parameter auf der Seite "OrderDetails.aspx" angeben.</span><span class="sxs-lookup"><span data-stu-id="e69c3-125">These specify the link to the Order details view for each product specifying the OrderID field as a QueryString parameter to the OrderDetails.aspx page.</span></span>

## <a id="_Toc260221675"></a>  <span data-ttu-id="e69c3-126">OrderDetails.aspx</span><span class="sxs-lookup"><span data-stu-id="e69c3-126">OrderDetails.aspx</span></span>

<span data-ttu-id="e69c3-127">Die Aufträge und eine FormView zum Anzeigen der Auftragsdaten und anderen EntityDataSource mit GridView zum Anzeigen der Zeilenelemente für alle der Reihenfolge zuzugreifen, verwenden Sie ein EntityDataSource-Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="e69c3-127">We will use an EntityDataSource control to access the Orders and a FormView to display the Order data and another EntityDataSource with a GridView to display all the Order's line items.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

<span data-ttu-id="e69c3-128">In der Code-Behind-Datei (OrderDetails.aspx.cs) haben wir zwei wenig Bits des Housekeeping.</span><span class="sxs-lookup"><span data-stu-id="e69c3-128">In the Code Behind file (OrderDetails.aspx.cs) we have two little bits of housekeeping.</span></span>

<span data-ttu-id="e69c3-129">Zunächst müssen wir sicherstellen, dass OrderDetails immer eine OrderId ruft.</span><span class="sxs-lookup"><span data-stu-id="e69c3-129">First we need to make sure that OrderDetails always gets an OrderId.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

<span data-ttu-id="e69c3-130">Wir müssen auch zu berechnen und die Gesamtsumme aus die Einzelposten des Auftrags anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="e69c3-130">We also need to calculate and display the order total from the line items.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>  <span data-ttu-id="e69c3-131">Auf der Startseite</span><span class="sxs-lookup"><span data-stu-id="e69c3-131">The Home Page</span></span>

<span data-ttu-id="e69c3-132">Fügen Sie einige statische Inhalte auf der Seite "default.aspx" ein.</span><span class="sxs-lookup"><span data-stu-id="e69c3-132">Let's add some static content to the Default.aspx page.</span></span>

<span data-ttu-id="e69c3-133">Zunächst erstellen ich einen Ordner "Content" und darin ein Ordner "Abbilder" (und ich werde sind, ein Bild auf der Homepage verwendet werden.)</span><span class="sxs-lookup"><span data-stu-id="e69c3-133">First I'll create a "Content" folder and within it an Images folder (and I'll include an image to be used on the home page.)</span></span>

<span data-ttu-id="e69c3-134">Fügen Sie das folgende Markup hinzu, in den Platzhalter unteren Rand der Seite "default.aspx".</span><span class="sxs-lookup"><span data-stu-id="e69c3-134">Into the bottom placeholder of the Default.aspx page, add the following markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>  <span data-ttu-id="e69c3-135">Produktübersicht</span><span class="sxs-lookup"><span data-stu-id="e69c3-135">Product Reviews</span></span>

<span data-ttu-id="e69c3-136">Zunächst fügen eine Schaltfläche mit einem Link zu einem Formular wir, die es verwenden können, um eine produktprüfung einzugeben.</span><span class="sxs-lookup"><span data-stu-id="e69c3-136">First we'll add a button with a link to a form that we can use to enter a product review.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

<span data-ttu-id="e69c3-137">Beachten Sie, dass wir die "ProductID" in der Abfragezeichenfolge übergeben werden</span><span class="sxs-lookup"><span data-stu-id="e69c3-137">Note that we are passing the ProductID in the query string</span></span>

<span data-ttu-id="e69c3-138">Nächste fügen Sie die Seite mit dem Namen ReviewAdd.aspx</span><span class="sxs-lookup"><span data-stu-id="e69c3-138">Next let's add page named ReviewAdd.aspx</span></span>

<span data-ttu-id="e69c3-139">Diese Seite wird das ASP.NET AJAX-Steuerelement-Toolkit verwenden.</span><span class="sxs-lookup"><span data-stu-id="e69c3-139">This page will use the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="e69c3-140">Wenn Sie noch nicht getan haben, damit Sie es aus herunterladen [DevExpress](http://devexpress.com/act) besteht die Anleitung zum Einrichten des Toolkits für die Verwendung mit Visual Studio hier [ https://www.asp.net/learn/ajax-videos/video-76.aspx ](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span><span class="sxs-lookup"><span data-stu-id="e69c3-140">If you have not already done so you can download it from [DevExpress](http://devexpress.com/act) and there is guidance on setting up the toolkit for use with Visual Studio here [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span></span>

<span data-ttu-id="e69c3-141">Ziehen Sie im Entwurfsmodus Steuerelemente und Validierungssteuerelemente aus der Toolbox, und erstellen Sie ein Format wie die folgende.</span><span class="sxs-lookup"><span data-stu-id="e69c3-141">In design mode, drag controls and validators from the toolbox and build a form like the one below.</span></span>

![](tailspin-spyworks-part-7/_static/image2.jpg)

<span data-ttu-id="e69c3-142">Das Markup sieht in etwa wie folgt.</span><span class="sxs-lookup"><span data-stu-id="e69c3-142">The markup will look something like this.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

<span data-ttu-id="e69c3-143">Nun, dass wir Reviews eingeben können, können diese Berichte auf der Seite "Product" angezeigt.</span><span class="sxs-lookup"><span data-stu-id="e69c3-143">Now that we can enter reviews, lets display those reviews on the product page.</span></span>

<span data-ttu-id="e69c3-144">Fügen Sie diesem Markup auf der Seite "ProductDetails.aspx".</span><span class="sxs-lookup"><span data-stu-id="e69c3-144">Add this markup to the ProductDetails.aspx page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

<span data-ttu-id="e69c3-145">Die Anwendung jetzt ausführen und das Navigieren zu einem Produkt zeigt die Produktinformationen, einschließlich kundenbewertungen.</span><span class="sxs-lookup"><span data-stu-id="e69c3-145">Running our application now and navigating to a product shows the product information including customer reviews.</span></span>

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>  <span data-ttu-id="e69c3-146">Gängige Elementsteuerelement (Benutzersteuerelemente erstellen)</span><span class="sxs-lookup"><span data-stu-id="e69c3-146">Popular Items Control (Creating User Controls)</span></span>

<span data-ttu-id="e69c3-147">Um auf Ihrer Website Umsatz steigern, werden wir eine Reihe von Funktionen zu "vorgeschlagene Sell" gängigen oder verwandte Produkte hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="e69c3-147">In order to increase sales on your web site we will add a couple of features to "suggestive sell" popular or related products.</span></span>

<span data-ttu-id="e69c3-148">Die erste dieser Funktionen wird eine Liste mit den gängigeren Produkt in unserer Produktkatalog sein.</span><span class="sxs-lookup"><span data-stu-id="e69c3-148">The first of these features will be a list of the more popular product in our product catalog.</span></span>

<span data-ttu-id="e69c3-149">Es wird ein "Benutzersteuerelement" zum Anzeigen der meistverkaufte Elemente auf der Startseite der Anwendung erstellt.</span><span class="sxs-lookup"><span data-stu-id="e69c3-149">We will create a "User Control" to display the top selling items on the home page of our application.</span></span> <span data-ttu-id="e69c3-150">Da dies ein Steuerelement sein wird, können wir es auf einer beliebigen Seite durch einfach ziehen und Ablegen von das Steuerelement in Visual Studio-Designer auf eine andere Seite, die wir gefällt.</span><span class="sxs-lookup"><span data-stu-id="e69c3-150">Since this will be a control, we can use it on any page by simply dragging and dropping the control in Visual Studio's designer onto any page that we like.</span></span>

<span data-ttu-id="e69c3-151">In Visual Studio-Projektmappen-Explorer mit der rechten Maustaste auf den Namen der Projektmappe, und erstellen Sie ein neues Verzeichnis mit dem Namen "Steuerelemente".</span><span class="sxs-lookup"><span data-stu-id="e69c3-151">In Visual Studio's solutions explorer, right-click on the solution name and create a new directory named "Controls".</span></span> <span data-ttu-id="e69c3-152">Während es nicht notwendig ist, unterstützen wir unsere Projekt durch das Erstellen von unseren Benutzersteuerelemente im Verzeichnis "Steuerelemente" beibehalten.</span><span class="sxs-lookup"><span data-stu-id="e69c3-152">While it is not necessary to do so, we will help keep our project organized by creating all our user controls in the "Controls" directory.</span></span>

<span data-ttu-id="e69c3-153">Mit der rechten Maustaste auf den Ordner, und wählen Sie "Neues Element":</span><span class="sxs-lookup"><span data-stu-id="e69c3-153">Right-click on the controls folder and choose "New Item" :</span></span>

![](tailspin-spyworks-part-7/_static/image4.jpg)

<span data-ttu-id="e69c3-154">Geben Sie einen Namen für das Steuerelement des "PopularItems".</span><span class="sxs-lookup"><span data-stu-id="e69c3-154">Specify a name for our control of "PopularItems".</span></span> <span data-ttu-id="e69c3-155">Beachten Sie, dass die Dateierweiterung für Benutzersteuerelemente .ascx nicht aspx.</span><span class="sxs-lookup"><span data-stu-id="e69c3-155">Note that the file extension for user controls is .ascx not .aspx.</span></span>

<span data-ttu-id="e69c3-156">Unsere Benutzersteuerelement für gängige Elemente werden wie folgt definiert werden.</span><span class="sxs-lookup"><span data-stu-id="e69c3-156">Our Popular Items User control will be defined as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

<span data-ttu-id="e69c3-157">Hier verwenden wir eine Methode, die wir noch nicht in dieser Anwendung verwendet haben.</span><span class="sxs-lookup"><span data-stu-id="e69c3-157">Here we're using a method we have not used yet in this application.</span></span> <span data-ttu-id="e69c3-158">Verwenden wir die wiederholungsmodul-Steuerelement, und anstatt ein Datenquellen-Steuerelement sind wir Wiederholungsmodul-Steuerelement binden, um die Ergebnisse einer LINQ to Entities-Abfrage.</span><span class="sxs-lookup"><span data-stu-id="e69c3-158">We're using the repeater control and instead of using a data source control we're binding the Repeater Control to the results of a LINQ to Entities query.</span></span>

<span data-ttu-id="e69c3-159">In den Code hinter der unserer Kontrolle erfolgt, die wie folgt.</span><span class="sxs-lookup"><span data-stu-id="e69c3-159">In the code behind of our control we do that as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

<span data-ttu-id="e69c3-160">Beachten Sie auch diese wichtigen Zeile am oberen Rand des Steuerelements Markup.</span><span class="sxs-lookup"><span data-stu-id="e69c3-160">Note also this important line at the top of our control's markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

<span data-ttu-id="e69c3-161">Da die am häufigsten verwendeten Elemente nicht regelmäßig zur ändert können wir eine schmerzenden Richtlinie zum Verbessern der Leistung der Anwendung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="e69c3-161">Since the most popular items won't be changing on a minute to minute basis we can add a aching directive to improve the performance of our application.</span></span> <span data-ttu-id="e69c3-162">Diese Richtlinie bewirkt, dass die Steuerelemente Code nur ausgeführt werden, wenn die zwischengespeicherte Ausgabe des Steuerelements abläuft.</span><span class="sxs-lookup"><span data-stu-id="e69c3-162">This directive will cause the controls code to only be executed when the cached output of the control expires.</span></span> <span data-ttu-id="e69c3-163">Andernfalls wird die zwischengespeicherte Version der Ausgabe des Steuerelements verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="e69c3-163">Otherwise, the cached version of the control's output will be used.</span></span>

<span data-ttu-id="e69c3-164">Jetzt haben wir führen lediglich unsere Default.aspc-Seite unserer neuen Kontrolle einschließt.</span><span class="sxs-lookup"><span data-stu-id="e69c3-164">Now all we have to do is include our new control in our Default.aspc page.</span></span>

<span data-ttu-id="e69c3-165">Mithilfe von ziehen und ablegen, um eine Instanz des Steuerelements in der open-Spalte der unsere Standardformular platzieren.</span><span class="sxs-lookup"><span data-stu-id="e69c3-165">Use drag and drop to place an instance of the control in the open column of our Default form.</span></span>

![](tailspin-spyworks-part-7/_static/image5.jpg)

<span data-ttu-id="e69c3-166">Zeigt die am häufigsten verwendeten Elemente jetzt Wenn wir unsere Anwendung auf der Startseite ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="e69c3-166">Now when we run our application the home page displays the most popular items.</span></span>

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>  <span data-ttu-id="e69c3-167">"Auch gekauft" steuern (Benutzersteuerelemente mit Parametern)</span><span class="sxs-lookup"><span data-stu-id="e69c3-167">"Also Purchased" Control (User Controls with Parameters)</span></span>

<span data-ttu-id="e69c3-168">Dauert vorgeschlagenen Verkauf an die nächste Ebene durch Hinzufügen von Kontext Besonderheit der zweites Benutzersteuerelement, das wir erstellen müssen.</span><span class="sxs-lookup"><span data-stu-id="e69c3-168">The second User Control that we'll create will take suggestive selling to the next level by adding context specificity.</span></span>

<span data-ttu-id="e69c3-169">Die Logik zum Berechnen der obersten Elemente "Auch" gekauft "ist nicht trivial.</span><span class="sxs-lookup"><span data-stu-id="e69c3-169">The logic to calculate the top "Also Purchased" items is non-trivial.</span></span>

<span data-ttu-id="e69c3-170">Unserer Kontrolle "Auch gekauft" wählen die OrderDetails-Datensätze, die (zuvor gekauft) für die aktuell ausgewählte "ProductID", und ziehen Sie die OrderIDs für jede eindeutige Bestellung, die gefunden wird.</span><span class="sxs-lookup"><span data-stu-id="e69c3-170">Our "Also Purchased" control will select the OrderDetails records (previously purchased) for the currently selected ProductID and grab the OrderIDs for each unique order that is found.</span></span>

<span data-ttu-id="e69c3-171">Wählen Sie dann werden wir al Produkte von diesen Aufträge und die Summe der Mengen erworben haben.</span><span class="sxs-lookup"><span data-stu-id="e69c3-171">Then we will select al the products from all those Orders and sum the quantities purchased.</span></span> <span data-ttu-id="e69c3-172">Wir die Produkte nach der Menge Summe sortieren und zeigt die obersten fünf Elemente.</span><span class="sxs-lookup"><span data-stu-id="e69c3-172">We'll sort the products by that quantity sum and display the top five items.</span></span>

<span data-ttu-id="e69c3-173">Dieser Algorithmus wird angesichts die Komplexität von diese Logik, wie eine gespeicherte Prozedur implementiert.</span><span class="sxs-lookup"><span data-stu-id="e69c3-173">Given the complexity of this logic, we will implement this algorithm as a stored procedure.</span></span>

<span data-ttu-id="e69c3-174">Die T-SQL für die gespeicherte Prozedur lautet wie folgt.</span><span class="sxs-lookup"><span data-stu-id="e69c3-174">The T-SQL for the stored procedure is as follows.</span></span>

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

<span data-ttu-id="e69c3-175">Beachten Sie, dass diese gespeicherte Prozedur (SelectPurchasedWithProducts) in der Datenbank vorhanden waren, wenn wir sie enthalten in der vorliegenden Anwendung, und wenn wir, die zusätzlich zu den Tabellen und Sichten, die es benötigt generiert, das Entity Data Model angegebenen Entity Data Model Diese gespeicherte Prozedur sollte enthalten sein.</span><span class="sxs-lookup"><span data-stu-id="e69c3-175">Note that this stored procedure (SelectPurchasedWithProducts) existed in the database when we included it in our application and when we generated the Entity Data Model we specified that, in addition to the Tables and Views that we needed, the Entity Data Model should include this stored procedure.</span></span>

<span data-ttu-id="e69c3-176">Zugriff auf die gespeicherte Prozedur aus dem Entity Data Model müssen wir die Funktion zu importieren.</span><span class="sxs-lookup"><span data-stu-id="e69c3-176">To access the stored procedure from the Entity Data Model we need to import the function.</span></span>

<span data-ttu-id="e69c3-177">Doppelklick auf dem Entity Data Model im Projektmappen-Explorer im Designer zu öffnen, und öffnen die Model-Browser, und klicken Sie dann mit der rechten Maustaste im Designer, und wählen Sie "Funktionsimport hinzufügen".</span><span class="sxs-lookup"><span data-stu-id="e69c3-177">Double Click on the Entity Data Model in the Solutions Explorer to open it in the designer and open the Model Browser, then right-click in the designer and select "Add Function Import".</span></span>

![](tailspin-spyworks-part-7/_static/image1.png)

<span data-ttu-id="e69c3-178">Auf diese Weise wird dieses Dialogfeld geöffnet.</span><span class="sxs-lookup"><span data-stu-id="e69c3-178">Doing so will open this dialog.</span></span>

![](tailspin-spyworks-part-7/_static/image2.png)

<span data-ttu-id="e69c3-179">Füllen Sie die Felder, wie Sie oben sehen die "SelectPurchasedWithProducts" auswählen, und verwenden Sie den Namen der Prozedur für den Namen der importierten Funktion.</span><span class="sxs-lookup"><span data-stu-id="e69c3-179">Fill out the fields as you see above, selecting the "SelectPurchasedWithProducts" and use the procedure name for the name of our imported function.</span></span>

<span data-ttu-id="e69c3-180">Klicken Sie auf "Ok".</span><span class="sxs-lookup"><span data-stu-id="e69c3-180">Click "Ok".</span></span>

<span data-ttu-id="e69c3-181">Dies, die wir einfach für die gespeicherte Prozedur programmieren können, wie wir eines beliebigen anderen Elements im Modell möglicherweise getan haben.</span><span class="sxs-lookup"><span data-stu-id="e69c3-181">Having done this we can simply program against the stored procedure as we might any other item in the model.</span></span>

<span data-ttu-id="e69c3-182">Deshalb in unserem "Steuerelemente" Ordner erstellen ein neuen Benutzersteuerelements, das mit dem Namen AlsoPurchased.ascx.</span><span class="sxs-lookup"><span data-stu-id="e69c3-182">So, in our "Controls" folder create a new user control named AlsoPurchased.ascx.</span></span>

<span data-ttu-id="e69c3-183">Das Markup für dieses Steuerelement wird das Steuerelement PopularItems sehr bekannt vorkommen.</span><span class="sxs-lookup"><span data-stu-id="e69c3-183">The markup for this control will look very familiar to the PopularItems control.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

<span data-ttu-id="e69c3-184">Der wesentliche Unterschied ist, die nicht das Zwischenspeichern der Ausgabe werden, da des Elements, das gerendert werden vom Produkt abweichen.</span><span class="sxs-lookup"><span data-stu-id="e69c3-184">The notable difference is that are not caching the output since the item's to be rendered will differ by product.</span></span>

<span data-ttu-id="e69c3-185">Die "ProductID" wird "Property" an das Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="e69c3-185">The ProductId will be a "property" to the control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

<span data-ttu-id="e69c3-186">Das Steuerelement PreRender-Ereignishandler wir Eed drei Schritte auszuführen.</span><span class="sxs-lookup"><span data-stu-id="e69c3-186">In the control's PreRender event handler we eed to do three things.</span></span>

1. <span data-ttu-id="e69c3-187">Stellen Sie sicher, dass die "ProductID" festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="e69c3-187">Make sure the ProductID is set.</span></span>
2. <span data-ttu-id="e69c3-188">Feststellen Sie, ob alle Produkte aus, die erworben wurden, mit der aktuellen Aktivität.</span><span class="sxs-lookup"><span data-stu-id="e69c3-188">See if there are any products that have been purchased with the current one.</span></span>
3. <span data-ttu-id="e69c3-189">Geben Sie einige Elemente wie #2 festgelegt.</span><span class="sxs-lookup"><span data-stu-id="e69c3-189">Output some items as determined in #2.</span></span>

<span data-ttu-id="e69c3-190">Beachten Sie, wie einfach es ist, rufen Sie die gespeicherte Prozedur über das Modell.</span><span class="sxs-lookup"><span data-stu-id="e69c3-190">Note how easy it is to call the stored procedure through the model.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

<span data-ttu-id="e69c3-191">Nachdem ermittelt wurde, gibt es "auch gekauft werden" können wir einfach Repeater binden, für die von der Abfrage zurückgegebenen Ergebnisse.</span><span class="sxs-lookup"><span data-stu-id="e69c3-191">After determining that there ARE "also purchased" we can simply bind the repeater to the results returned by the query.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

<span data-ttu-id="e69c3-192">Wenn es keine Elemente "auch gekauft wurden" zeigen wir andere gängige Elemente einfach aus unserem Katalog.</span><span class="sxs-lookup"><span data-stu-id="e69c3-192">If there were not any "also purchased" items we'll simply display other popular items from our catalog.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

<span data-ttu-id="e69c3-193">Die "Auch gekauft" Elemente anzeigen, öffnen Sie die Seite ProductDetails.aspx und ziehen die AlsoPurchased im Projektmappen-Explorer, damit es an dieser Position in das Markup angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="e69c3-193">To view the "Also Purchased" items, open the ProductDetails.aspx page and drag the AlsoPurchased control from the Solutions Explorer so that it appears in this position in the markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

<span data-ttu-id="e69c3-194">Auf diese Weise wird einen Verweis auf das Steuerelement am oberen Rand der Seite "Produktdetails" erstellt.</span><span class="sxs-lookup"><span data-stu-id="e69c3-194">Doing so will create a reference to the control at the top of the ProductDetails page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

<span data-ttu-id="e69c3-195">Da das Benutzersteuerelement AlsoPurchased diverse "ProductID" erfordert wird die Eigenschaft "ProductID" unserer Kontrolle mithilfe einer Eval-Anweisung für das aktuelle Modell-Datenelement der Seite festgelegt.</span><span class="sxs-lookup"><span data-stu-id="e69c3-195">Since the AlsoPurchased user control requires a ProductId number we will set the ProductID property of our control by using an Eval statement against the current data model item of the page.</span></span>

![](tailspin-spyworks-part-7/_static/image3.png)

<span data-ttu-id="e69c3-196">Wenn wir erstellen, und jetzt auszuführen, und navigieren Sie zu einem Produkt sehen wir die Elemente "Auch gekauft".</span><span class="sxs-lookup"><span data-stu-id="e69c3-196">When we build and run now and browse to a product we see the "Also Purchased" items.</span></span>

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> <span data-ttu-id="e69c3-197">[Zurück](tailspin-spyworks-part-6.md)
> [Weiter](tailspin-spyworks-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="e69c3-197">[Previous](tailspin-spyworks-part-6.md)
[Next](tailspin-spyworks-part-8.md)</span></span>
